# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 08:25** · publicado em **13/11/2026 08:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 08:25 · sessao ce8b6039e8c7 · - · - · origem=Ronald (tmux)

A. Depois do diff, `pode_executar_csf` pergunta:

- Origem: qualquer disputa relacionada devolve lista não vazia de `respostas_sem_veredito()`? Se sim, recusa. Não filtra disputa aberta. (`diff app/ponto/supra_juiz.py:563`)
- Destino: a pergunta que resolve o chamado pertence à lista `q.disputa.respostas_sem_veredito()`? Se sim, recusa; não basta outra pergunta pendente da mesma disputa. (`diff app/ponto/supra_juiz.py:566-568`)
- A função, `_gate_conversa_do_pai` e `_decisao_do_fechamento` não conservam leituras diretas de `respondida_em`/`validada_em`; o gate aplica a composição inteira, embora a docstring diga “só PAI”. (`app/chamados/models.py:664-697`; `app/chamados/models.py:704-716`; `diff app/ponto/supra_juiz.py:563-568`)

O predicado existente é exatamente: `validada_em is None`, `materializada_em is None`, **carimbo de resposta OU texto.strip()**, e dia não trancado, salvo `ignorar_tranca=True`. Portanto carimbo sem texto conta, texto sem carimbo conta, validação/materialização excluem e tranca exclui; “julgada”, sequência de decisões, `recusa_motivo` e `via_resolucao` isolados não são consultados. (`app/chamados/models.py:1856-1876`)

Desligamento e reconciliador usam a mesma chamada, sem alterar `ignorar_tranca`; `fechar()` também. (`app/colaboradores/services/desligamento.py:141`; `app/chamados/reconciliador.py:1164`; `app/chamados/models.py:1916`)

B. Caminho da premissa morta: `retratar_premissa_morta_colab` → `_retratar` → `transicionar_com_pauta(... ato=ch.reconciliar)` → `reconciliar` → `_decisao_do_fechamento` → `_gate_conversa_do_pai` → `pode_executar_csf` → predicado A. (`app/chamados/reconciliador.py:1459-1465`; `app/chamados/reconciliador.py:524`; `app/chamados/services/fecho_runtime.py:165`; `app/chamados/models.py:652`; `app/chamados/models.py:686`; `app/chamados/models.py:716`; `diff app/ponto/supra_juiz.py:563-568`)

**Mas existe uma guarda anterior divergente:** `_retratar` consulta `retencao_da_retratacao` antes de chegar ao gate; esta chama `irmas_respondidas_sem_consumo`. Essa função usa carimbo **E texto**, sem exclusão por tranca, no universo de crédito destino/pai. Assim uma pergunta respondida, não validada/materializada, mas trancada, pode ser retida aqui embora A a exclua; esse ramo não chega a A. (`app/chamados/reconciliador.py:519-524`; `app/chamados/reconciliador.py:447-450`; `app/chamados/juizes.py:2028-2034`; `app/chamados/models.py:1872-1876`)

Outro caminho preservado pergunta pela resposta usando **somente texto** antes de chamar o fechamento sistêmico: `_tem_resposta_do_colab` no cartório. Pergunta validada com texto continua excluída desses ramos, diferentemente de A. (`app/ponto/services/cartorio.py:1637-1651`; `app/ponto/services/cartorio.py:1661-1666`)

D. O gate pode segurar um pai quando outra disputa relacionada ainda tem resposta pendente, inclusive disputa fechada cujos campos posteriores voltaram a representar pendência; também consulta perguntas que resolvem esse chamado, ainda que pertençam a disputa com outro pai. O limite de entrada é apenas `pk` e categoria `disputa_supervisao`. (`app/chamados/models.py:713-716`; `diff app/ponto/supra_juiz.py:563-568`)

Os três criadores de produção reutilizam **qualquer disputa aberta do colaborador** antes de criar. Logo nenhum deles cria diretamente a segunda enquanto a primeira está aberta, como faz o helper do teste. (`app/chamados/services/disputa_emissao.py:531-566`; `app/chamados/services/disputa_emissao.py:1096-1103`; `app/chamados/services/disputa_emissao.py:1332-1338`; `diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:91-100`)

Entretanto, **o estado é alcançável por histórico e reabertura**:

1. D1 fecha, S88 resolve P. (`app/chamados/models.py:1962-1966`; `app/chamados/signals.py:43`)
2. Gestor muda P de resolvido para em análise pela resposta livre; essa matriz permite a transição e não reabre D1. (`app/chamados/services/acoes_chamado.py:319-322`; `app/chamados/services/fecho_runtime.py:175`; `app/chamados/models.py:128-199`; `app/chamados/catalogo/motor.py:102`)
3. Sem disputa aberta, o emissor reutiliza o mesmo P vivo pelo lookup categoria/módulo/status e cria D2. (`app/chamados/services/disputa_emissao.py:568-591`; `app/chamados/models.py:936-954`)
4. A ação administrativa encontra D1 fechada desse P, reabre sem conferir D2 e cria pergunta nova. Depois da resposta, há duas disputas abertas do mesmo pai. (`app/chamados/services/acoes_disputa.py:255-268`; `app/chamados/models.py:2168-2191`; `app/chamados/services/disputa_emissao.py:1467-1468`)

Portanto a fixture **fabrica diretamente um estado alcançável**, mas não testa sua criação por esse caminho; ocorrência em produção: [não verificado]. (`diff app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:91-100`)

Quanto ao destino ser um pai: a estrutura permite conversa do mesmo colaborador com esse destino, mas o derivador habitual exclui `disputa_supervisao`; não demonstrei um chamador de produção que construa tal arranjo. (`app/chamados/models.py:2824-2832`; `app/chamados/juizes.py:2263-2271`; `app/chamados/services/disputa_emissao.py:198-201`; [não verificado])

Bloqueante sustentado para consolidar: **a migração ao juiz A não cobre a guarda anterior de retratação e deixa intacto o predicado paralelo do cartório**, com divergências concretas de critério e alcance. (`app/chamados/reconciliador.py:519-524`; `app/chamados/juizes.py:2028-2034`; `app/ponto/services/cartorio.py:1641-1648`; `app/chamados/models.py:1872-1876`)

## 13/11 08:25 · sessao 7bfb0adffbf9 · - · - · origem=Ronald (tmux)

B. Os quatro nomes `LEGADO_*` entram em `ESTADOS_DE_LEGADO` e são registrados em `ESTADOS_LEGADOS`; esse último conta **leituras**, não células distintas (diff `app/escala/services/ata.py:28`, `:33`, `:115`). O censo corrige essa diferença contando cada ata não NULL pelo mesmo `leitura_orfas`, sem reclassificar seu conteúdo; NULL passado é contado por SQL separado (diff `app/escala/services/ata.py:203`, `:206`, `:209`). Contudo, os leitores não tratam LEGADO pelo nome: `horas or []`, `quantidade or 0` e `bool(horas) or bool(quantidade)` ignoram `.estado`, incluindo conversão de `None` para zero (HEAD `app/escala/services/ata.py:119`, `:130`, `:136`). Só o leitor novo de instantes verifica o estado (HEAD `app/escala/services/ata.py:147`).

C. O universo do censo é toda `CelulaDia`, sem recorte de empresa, situação, origem ou data para atas não NULL; NULL passado entra no débito e NULL hoje/futuro fica fora dele (diff `app/escala/services/ata.py:203`, `:209`, `:212`, `:213`). O corte do censo vem uma vez de `_hoje`, usando `datetime.date.today`; fuso efetivo do processo [não verificado] (diff `app/ponto/management/commands/tripwire_celulas.py:357`, `:359`, `:369`, `:423`). Entretanto, o **comando inteiro** ainda relê o relógio nos helpers chamados sem `ini/fim`: `medir_pausa_faltando` e `medir_sobra_com_tipo_trocado` (HEAD `app/ponto/management/commands/tripwire_celulas.py:87`, `:249`, `:381`, `:392`; diff novas `:88`, `:250`, `:387`, `:398`). O universo também não é uma fotografia única: há consultas separadas para atas gravadas, NULL passado, todas NULL e total (diff `app/escala/services/ata.py:203`, `:209`, `:212`, `:213`).

O contador corresponde à fórmula declarada para abrir CONTRACT — legado + ilegal + não lavrada passada — e a mensagem nomeia CONTRACT como pouso próprio; resultado real `contador=0` [não verificado] (diff `app/escala/services/ata.py:192`; diff `app/ponto/management/commands/tripwire_celulas.py:425`, `:436`). O cron está registrado como vigia (HEAD `app/config/crons.py:2171`). As rotas inspecionadas consultam banco e imprimem alarmes, mas escrevem memória e log quando passam pela porta (diff `app/escala/services/ata.py:48`, `:49`, `:115`, `:206`).

D. `_ilegal` em produção adiciona um sítio ao conjunto global, faz `logging.error` e levanta `EstadoIlegalDaAta`; não há condicionamento a CI (HEAD `app/escala/services/ata.py:43`, `:46`, `:47`, `:48`). LEGADO não chama `_ilegal`; apenas incrementa `ESTADOS_LEGADOS` (HEAD `app/escala/services/ata.py:112`). Ata não mapping chama `_ilegal`; o censo captura a exceção, conta ILEGAL e deixa o registro global para trás (diff `app/escala/services/ata.py:91`, `:206`, `:207`, `:208`). Isso provoca falha no hook/runner quando ocorre no mesmo processo da suíte; processos diferentes não compartilham esses conjuntos (HEAD `app/escala/services/ata_ci.py:62`, `:65`; `app/escala/runner_ata_ci.py:11`, `:16`).

O helper **pode esconder outro ilegal real do mesmo sítio**: o registro é um `set` de `(arquivo, linha, motivo)`, e todas as atas não mapping recusadas por `_valores_orfas` convergem no mesmo sítio; o helper verifica um sítio, não uma ocorrência/identidade de fixture, e limpa todo o conjunto (HEAD `app/escala/services/ata.py:45`, `:46`, `:89`; diff `app/escala/tests/test_tipo_item3_v5_expand.py:259`, `:260`, `:261`, `:262`). Detecta ilegais de outros sítios, mas não duplicatas no mesmo sítio (mesmas referências).

E. Mordida prevista por inspeção; execução dos mutantes [não verificado]:

| Mutante | Teste que falha | [nao verificado]
|---|---|
| LEGADO contado como LAVRADA | `test_RED_censo_nomeia_cada_estado_e_conta_o_debito`, mapa esperado; também saída `ata_fora_da_forma=7` no teste do comando (diff `app/escala/tests/test_tipo_item3_v5_expand.py:264`, `:267`, `:282`, `:298`). |
| Ignora `n_orfas != len(orfas)` | Os mesmos testes: a fixture `orfas=[]/n_orfas=2` precisa ser `LEGADO_DESCASADA` (diff `app/escala/tests/test_tipo_item3_v5_expand.py:243`, `:267`, `:298`). |
| Remove `ATA-FORA-DA-FORMA>0` | `test_o_tripwire_publica_a_linha_e_acusa`, assert literal do alarme (diff `app/escala/tests/test_tipo_item3_v5_expand.py:282`, `:300`). |

`test_MORDE_frota_na_forma_da_zero` não morde esses três: ele elimina todas as células exceto uma lavrada válida antes da medição (diff `app/escala/tests/test_tipo_item3_v5_expand.py:275`, `:277`, `:279`).

A cópia em disco contém HEAD sem o delta; os números acima identificados como **diff** correspondem ao anexo fornecido, não a código encontrado no disco (HEAD `app/escala/services/ata.py:176`; HEAD `app/ponto/management/commands/tripwire_celulas.py:363`).

## 13/11 08:25 · sessao 54e1555b97b2 · - · - · origem=Ronald (tmux)

**G — bloqueante demonstrável:** leitores de LEGADO não tratam o estado pelo nome; continuam em `horas or []`, `quantidade or 0` e coerção booleana. Nomear/contar antes não elimina o fallback proibido. `app/escala/services/ata.py` HEAD:93–114,117–136; diff:95–116,119–138. `_orfas_da_ata` também ignora `.estado` e usa `int(_l.quantidade or 0)`. `app/escala/services/leitor_celula.py`:189–194.

**G — sem prova de (B) agora:** escritor mantém `orfas` como strings HH:MM e `n_orfas=len`; o ISO entra em chave adicional. `app/escala/services/ata.py` HEAD:281–295; diff:321–335. O consumidor da chave nova é o próprio acessor; os usos encontrados são testes. `app/escala/services/ata.py` HEAD:139–164; `app/escala/tests/test_tipo_item3_v5_expand.py`:79,85,92,103,123. Assim, a troca produtiva de formato e leitor no mesmo pouso não está demonstrada [não verificado].

**H — relógio:** o censo novo recebe `hoje` da borda; porém o comando inteiro continua lendo relógio dentro de `medir_pausa_faltando` e `medir_sobra_com_tipo_trocado`, chamados sem argumentos. `app/ponto/management/commands/tripwire_celulas.py` HEAD:85–88,247–250,381,392; diff:85–88,247–250,390,401. São sítios preexistentes no diff, portanto não apresentar como regressão introduzida pelo delta [não verificado quanto à execução].

**H — campo com dois contextos de dia, já preservado:** PDF consulta uma entrada usando somente HH:MM contra conjunto que reúne órfãs de todos os dias da janela. `app/relatorios/pdf_espelho.py`:174–178,472–477. O teste novo afirma expressamente que esse defeito da base permanece; não é correção nem regressão desta volta. `app/escala/tests/test_tipo_item3_v5_expand.py`:151–159.

**H — não demonstrados:** segundo juiz, segundo escritor produtivo, alteração de ordem de trava ou abertura do pouso A das 12 chaves [não verificado]. O caminho visível continua `ata_do_dia → gravar_orfas → lavrar_veredito`; a catraca abrange três chaves. `app/ponto/services/cartorio.py`:772; `app/escala/services/ata.py` HEAD:32,281–295; `app/ponto/portas/celula.py`:998,1003,1036,1043.

**Pré-requisito de EXPAND:** os anexos não fornecem o DIFF por leitor na frota igual a zero antes do opus; seu cumprimento permanece [não verificado].

## 13/11 08:25 · sessao b8a8d7fad656 · - · - · origem=Ronald (tmux)

A/F — conclusões estáticas; DIFF por leitor na frota não consta do dado fornecido [não verificado].

Para atas `Mapping`, os oito leitores migrados conservam suas expressões da base nos quatro estados solicitados; “igual à base” não significa que todos devolvem a mesma coisa entre si:

| Leitor | NULA | HH:MM legado completo | Sem chaves / chave faltante / descasada |
|---|---|---|---|
| `_orfas_da_ata` | `[]` no leitor público | textos não vazios | textos existentes; se nenhum, `['?'] * int(n or 0)` — `app/escala/services/leitor_celula.py:383`, `:189`, `:193`, `:194`, `:424` |
| `_celula_furada` | `None` pelo guard sem `lampadas` | lista OU contagem acusa, após guardas anteriores | mesmo bool lista OU contagem — `app/chamados/juizes.py:494`, `:506`, `:509`, `:511`, `:517` |
| Materialização | nenhuma marca órfã | HH:MM válidos entram em `marcas` | só usa lista; ignora `n_orfas` — `app/chamados/services/materializacao.py:173`, `:178`, `:182` |
| `periodos_do_dia` | `orfas=[]` | `[str(h), ...]` | só usa lista; ignora contagem — `app/ponto/juiz_batida.py:158`, `:163` |
| Tripwire sobra | célula ignorada | horas cruas | horas cruas, inclusive `None`; ignora contagem — `app/ponto/management/commands/tripwire_celulas.py:260`, `:263`, `:266` |
| `insumos_da_ata` | `(False, False)` sem lâmpadas | sem lâmpadas, n positivo dá `(None,None)` | usa somente contagem no ramo sem lâmpadas — `app/ponto/services/cartorio.py:834`, `:836`, `:841` |
| Cluster | `None`, salvo juiz dia sem trabalho | segue medindo assinatura HH:MM | lista ausente/não-lista dá `None`; mismatch de n é ignorado — `app/ponto/services/cluster_espurio.py:25`, `:31`, `:32`, `:34`, `:36` |
| Esmeril | nenhuma órfã | mesmo bucket por HH:MM | somente lista, ignora contagem — `app/ponto/services/esmeril_espelho.py:126`, `:127`, `:129` |
| Cartão pela célula | conjunto vazio | conjunto de strings por data | somente lista, ignora contagem — `app/relatorios/cartao_pela_celula.py:288`, `:289` |

Exceção de API isolada: `_orfas_da_ata(None)` mudou de erro da antiga expressão `.get` para `[]`; o leitor público já normalizava `None` para `{}`, portanto esse caminho público conserva a saída. A expressão antiga está no hunk removido fornecido; nova guarda em `app/escala/services/leitor_celula.py:189`; normalização e chamada em `:383` e `:424`.

Há leitor adicional fora da migração: `vigia_pareador_celula.censo` recebe ata via `values_list` e lê as duas chaves cruas com fallback; lista presente com n ausente/zero vira conjunto vazio. Não consulta estado nomeado nem acessor — `app/ponto/services/vigia_pareador_celula.py:69`, `:71`, `:73`, `:74`. Essa leitura existe na cópia, mas não foi tocada pelos hunks fornecidos; provar se a matriz declarada previa sua cobertura requer a matriz ausente [não verificado].

Escritores e leitores dos campos:

- `orfas`/`n_orfas`: valores escritos por `gravar_orfas`; chamada do cartório em `app/ponto/services/cartorio.py:772`; implementação HEAD em `app/escala/services/ata.py:289`, `:290`, `:291`. Leituras centralizadas em `_valores_orfas`, `app/escala/services/ata.py:86`, `:90`; consumidores acima e vigia cru em `app/ponto/services/vigia_pareador_celula.py:74`.
- `orfas_instantes`: gravada ou removida por `gravar_orfas`, HEAD `app/escala/services/ata.py:292`, `:293`, `:294`, `:295`; única leitura de produção encontrada está em `_instantes_gravados`/`instantes_orfas`, HEAD `:139`, `:141`, `:144`, `:149`, `:158`. Não encontrei consumidor de negócio chamando `instantes_orfas`; os chamados explícitos do dado são testes, `app/escala/tests/test_tipo_item3_v5_expand.py:78`, `:84`, `:91`.
- `HoraComInstante`: criada por `hora_de`; texto continua `strftime('%H:%M')` e `.instante` guarda datetime aware — HEAD `app/escala/services/ata.py:57`, `:61`, `:68`, `:73`, `:76`. Origem local é `timezone.localtime` nos builders — `app/escala/utils.py:686`, `:1326`; as substituições estão em `:329`, `:624`, `:665`, `:802`, `:1835`, `:1837`.
- O atributo `.instante` só é consumido na gravação e cópia/pickle, HEAD `app/escala/services/ata.py:65`, `:293`; os consumidores legados continuam usando texto, identidade de hash, slices, ordenação, quantidade ou verdade — exemplo PDF `app/relatorios/pdf_espelho.py:178`, `:477`, `:609`; propositor `app/ponto/propositor.py:24`, `:30`, `:55`; classe órfã `app/escala/orfa_classe.py:58`, `:74`; horas do dia `app/ponto/turnos.py:2667`.

Não há unidade temporal comum comprovável no código:

- A grade mistura órfãs do turno (`orfas_por_data[d]`) com sobras guardadas pela data civil da batida — `app/escala/utils.py:1441`, `:1761`, `:1814`, `:1817`, `:1823`, `:1837`.
- Vigia pressupõe data civil — `app/ponto/services/vigia_pareador_celula.py:66`, `:67`, `:109`; supra juiz declara dia-TURNO — `app/ponto/supra_juiz.py:509`, `:510`, `:519`.
- `horas_reais_do_dia` interpreta órfã HH:MM como deslocamento simples desde meia-noite da linha, enquanto usa rollover para células — `app/ponto/turnos.py:2646`, `:2649`, `:2663`, `:2665`, `:2667`.
- PDF une HH:MM de todos os dias da janela e compara entrada sem data; `HoraComInstante` conserva esse defeito pelo hash textual — `app/relatorios/pdf_espelho.py:174`, `:178`, `:472`, `:477`; teste novo documenta e fixa o comportamento em `app/escala/tests/test_tipo_item3_v5_expand.py:151`, `:160`.

A cópia local é HEAD sem o delta fornecido: a numeração HEAD de `ata.py` depois de :175 difere da numeração nova de `diff.txt`; o censo só existe no anexo delta [não verificado como arquivo local].

## 13/11 08:24 · sessao 4d8a5ac1b5a3 · - · - · origem=Ronald (tmux)

**H — matriz de escrita/leitura e significado**

| Campo/valor | Quem escreve | Quem lê / significado |
|---|---|---|
| `respondida_em` | Resposta humana: `app/chamados/services/disputa_emissao.py:1467-1469`; resposta automática: `:1652-1656`; recuperação pela trilha: `:1670-1678`; simetria com texto: `app/chamados/models.py:3160-3167`. Reabertura limpa em `disputa_emissao.py:1593`; ressuscitação limpa em `models.py:1596`. | Juiz existente aceita **carimbo OU texto**, sem validação/materialização e fora da tranca: `app/chamados/models.py:1872-1876`. C6 exige **carimbo E texto** no degrau da resposta: `app/chamados/juizes.py:1417`. Estado apresentado usa carimbo para RESPONDIDA: `app/chamados/models.py:3075-3078`. |
| `validada_em` | Decisão humana: `app/chamados/services/validacao.py:127-141`; simetria: `app/chamados/models.py:3164-3167`. Nova resposta limpa em `disputa_emissao.py:1471-1482`; devolução em `validacao.py:195-202`; reabertura em `disputa_emissao.py:1593`; ressuscitação em `models.py:1596`. | Exclui resposta de `respostas_sem_veredito`: `app/chamados/models.py:1873`; mata a pergunta no C6: `app/chamados/juizes.py:1401-1402`; impede nova validação: `app/chamados/services/validacao.py:396-398`. |
| `status_local` | Núcleo: `app/chamados/models.py:228`, persistência `:296`; nascimento: `:1125`. Simulação altera/restaura somente memória: `:356`, `:371`. | Motor usa o estado para RECONCILIAR: `app/chamados/models.py:674`; gate o grava no payload: `:720`; contador exige VIVOS: `app/chamados/juizes.py:2522`. Fonte dos conjuntos: `app/chamados/catalogo/motor.py:24-31`. |
| Trilha `fechamento_recusado` / `conversa_exige_decisao_humana` | Gate → `_registrar_recusa` → `evento`: `app/chamados/models.py:718-722`, `:397-408`; escritor físico: `app/core/observ.py:134-137`. | Deduplicador compara última recusa/payload e transições posteriores: `app/chamados/models.py:403-406`; contador lê qualquer recusa histórica do motivo e cruza com estado atualmente vivo: `app/chamados/juizes.py:2519-2522`. |
| Nota `retido_pela_conversa` | Novo gate chama a porta de nota, somente sob `registrar`: **diff `app/chamados/models.py:718`, `:728-735`**; gravação física em `app/chamados/services/nota.py:103-109`. | Timeline lê todos os Followups por data: `app/chamados/views.py:372`; apresentação percorre todos: `app/templates/chamados/detalhe_local.html:112`. Leitor específico de produção desse `tipo`: **[não verificado]**. |
| `objeto_ref` | Novo gate monta string dos PKs ordenados: **diff `app/chamados/models.py:728`, `:735`**. | Porta transforma em hash com chamado, tipo e autor: `app/chamados/services/nota.py:62-65`, `:95-101`. **Não é campo persistido separadamente**: campos criados em `:88-89`, acrescidos de `evento_chave` em `:101`. Unicidade parcial: `app/chamados/models.py:1728-1730`. |

**Não há significado universal “carimbo de validação = alguém julgou”:** o próprio contrato distingue decisão de validar de carimbos de simetria, explicitamente. Essa duplicidade preexiste ao delta; a simetria atual exige texto e passa pelo C6 antes de gravar. (`app/chamados/services/validacao.py:19-26`; `app/chamados/models.py:3160-3175`.)

**Bloqueante relevante para B/I: predicado paralelo permanece no cartório.** `_tem_resposta_do_colab` pergunta apenas por texto e preserva os chamados correspondentes; o diff não modifica esse ramo. Assim, texto validado/materializado libera o novo CSF, mas continua isentando o chamado no cartório. Carimbo sem texto, sem validação/materialização, produz o inverso. (`app/ponto/services/cartorio.py:1637-1649`, `:1661-1666`; `app/chamados/models.py:1872-1876`; **diff `app/ponto/supra_juiz.py:563-569`**.)

**Nota — dois limites materiais:**

- O juiz pode recusar pela metade DESTINO, mas a coleta de perguntas da nota percorre somente `disputa_origem`. Nesse caso `_pend` vazio deixa a recusa sem nota. (`app/chamados/models.py:713-716`; **diff `app/ponto/supra_juiz.py:566-568`; diff `app/chamados/models.py:728-735`**.)
- O texto “a disputa fechou” não corresponde a uma precondição do gate: uma reconciliação direta com disputa ainda aberta também pode alcançá-lo. Depois do veredito/fecho do pai, a nota continua como registro histórico; a porta apenas cria ou recupera, e a timeline não filtra por estado atual. (`app/chamados/models.py:642-655`, `:713-716`; **diff `app/chamados/models.py:732-734`**; `app/chamados/services/nota.py:103-109`; `app/chamados/views.py:372`.)

**Classes I identificadas:** derivação paralela divergente para a pergunta “há resposta que impede reconciliação”; leitores dos carimbos com critérios diferentes; nota com universo menor que o da recusa. Novo escritor de estado fora da porta, relógio novo e nova aquisição de trava no delta examinado: **[não verificado]**. (`app/ponto/services/cartorio.py:1641`; `app/chamados/models.py:1872-1876`; **diff `app/chamados/models.py:728-735`; diff `app/ponto/supra_juiz.py:563-568`**.)

## 13/11 08:10 · sessao fa61a1941543 · ACELERA-DIFF · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 13/11 08:05 · sessao 20612ba1fceb · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 13/11 08:04 · sessao c7334fc8cc25 · - · - · origem=Ronald (tmux)

**c — BLOQUEIA:** a área comum continua gravável: `container()` monta `$SAIDA:/sombra` sem `:ro`; a sonda pode sobrescrever `lavra_prod.json`, `repor_lavra.py` ou outra sonda. `PGOPTIONS` não protege arquivos. A montagem permaneceu inalterada no diff. (HEAD `bin/sombra.sh:189-191`; diff `bin/sombra.sh:499-514` usa esse mesmo `container()`.)

Escritas identificadas no caminho:

- Inicialização cria `$SAIDA` quando ausente. (HEAD `bin/sombra.sh:81-84`.)
- Trava cria `logs/`, abre/cria `sombra.lock` em append e acrescenta evento a `sombra_espera.jsonl`. (Diff `bin/sombra_trava.sh:58-65`.)
- `tee` cria/trunca `logs/$_rot.out`; duas execuções do mesmo script sem rótulos distintos usam o mesmo destino. **FILA:** corrida introduzida pela simultaneidade, com perda/mistura dos resultados. (Diff `bin/sombra.sh:502-514`, conforme posição final a conferir; trecho novo iniciado no hunk `@@ -459,6 +479,21 @@`.)
- Django cria `/app/logs` e abre `errors.log` e `ponto.log`; são tmpfs particulares de cada container. (HEAD `app/config/settings/saas.py:118`, `:138-139`, `:162-163`; `bin/sombra.sh:189`.)
- Docker cria/remove o container; camada gravável local permanece permitida, pois inexiste `--read-only`; `/app` e `/sombra_bin` são mounts somente leitura, `/app/media` é tmpfs. (HEAD `bin/sombra.sh:183-191`.)
- Portão tenta `CREATE TEMP TABLE`; escrita efetivamente aceita depende do runtime **[nao verificado]**. Não há atualização de lavra no novo ramo. (Diff `bin/sombra_confere_leitura.py:21-26`; novo ramo do diff.)
- A sonda é código arbitrário carregado por `exec`; suas escritas concretas não constam no dado **[nao verificado]**. (Novo ramo do diff, comando `exec(open('/sombra/$_scr').read())`.)
- `sombra_db.sh --conferir` faz inspeções e `SELECT 1`; retorna antes das rotinas de criação/update. (HEAD `bin/sombra_db.sh:57-84`, `:86-100`.)
- `md5_lavra_prod` usa processo dentro de `saas_core`, cujo startup Django herda os handlers de arquivo; settings efetivos e escritas reais desse container **[nao verificado]**. (Diff `bin/sombra.sh:145-151`; HEAD `app/config/settings/saas.py:138-139`, `:162-163`.)

**BLOQUEIA adicional:** a cura indicada pelo portão, `--lavra`, continua despachando `lavra_de_prod` sem adquirir trava. Ela pode escrever arquivos e snapshots enquanto leitores seguram a compartilhada. É defeito preexistente que passa a furar o novo contrato. (HEAD `bin/sombra.sh:462`, `:144`, `:151`, `:157-163`; diff `bin/sombra_confere_leitura.py:32-34`.)

**e — FILA:** o selo verifica palavras, não comportamento da porta. Substituir integralmente `sombra_confere_leitura.py` por `print('ok')` mantém o selo verde: ele só exige existência e procura referências/flags/`&&` no shell. Também `echo RECUSADA; return 0` passa (d), e `echo NEGADA; return 0` passa os casos de recusa, que ignoram o rc. (Diff `bin/tests/test_sombra_leitura_compartilhada.sh:13-14`, `:35`, `:38`, `:41`, `:44`, `:49-50`, `:55-66`.)

**f — sem regressão demonstrada nesses consumidores:**

- `modo` é aceito pelo leitor conhecido: `json.loads` e `.get` ignoram campos extras; placar usa apenas a saída agregada. (HEAD `bin/sombra_trava.sh:49-57`; `bin/placar_code.sh:166`.)
- Os outros `container()` existentes recebem array vazio quando `SOMBRA_SO_LEITURA` não existe; variável prefixada na chamada nova não muda persistentemente as chamadas seguintes. (Diff `bin/sombra.sh:200-203`; chamadas HEAD `bin/sombra.sh:166`, `:204-206`, `:266-275`, `:346`, `:351`.)
- Selos conhecidos recortam funções/ramo por delimitadores, não números de linha; o diff preserva seus alvos. Execução contra o diff **[nao verificado]**. (HEAD `bin/tests/test_ensaio_de_dinheiro_sincroniza_lavra.sh:17-27`; `bin/tests/test_bloco_repoe_a_lavra.sh:29-48`; `bin/tests/test_sombra_guarda_num_sitio.sh:25-33`.)
- Outros usuários de fd 8 existem; colisão em shell que sobrescreva esse descritor é anterior ao diff, pois o escritor do HEAD já usa fd 8. (HEAD `bin/sombra_trava.sh:26`; `bin/trava.sh:64`; `bin/integrador.sh:41`; `bin/codex_pergunta.sh:368`.)

## 13/11 08:04 · sessao eed45d568fae · - · - · origem=Ronald (tmux)

- **a — exclusão normal correta:** escritor HEAD usa `flock` exclusivo no mesmo `logs/sombra.lock`; leitor novo usa `flock -s`. Escritor herdado conserva exclusiva; leitura repetida retorna antes de reabrir fd 8; escritor novo sob leitura é recusado. `simular_folha par` conserva a herança nas duas fotos. Referências: HEAD `bin/sombra_trava.sh:16,22,26,28,38`; diff `bin/sombra_trava.sh:29,30,53,54,58,60,69`; HEAD `bin/simular_folha.sh:96-99`.

- **BLOQUEIA se a alegação inclui filhos executando HEAD:** pai segura leitura nova; filho executa função escritora HEAD, que ignora `SOMBRA_TRAVA_LEITURA`, reabre seu fd 8 e espera exclusiva; pai ainda tem compartilhada e aguarda o filho. Resultado: autoespera até timeout. Isso não ocorre com escritor velho independente já rodando, que é excluído corretamente. Referências: diff `bin/sombra_trava.sh:58,60,69`; HEAD `bin/sombra_trava.sh:22,26,28`; HEAD `bin/sombra.sh:461` fornece execução síncrona do comando embrulhado. Existência de uma árvore antiga concreta disponível para esse filho: **[não verificado]**.

- **BLOQUEIA — escritor fora do protocolo:** `--lavra` continua chamando diretamente `lavra_de_prod`, sem exclusiva. Portanto pode gravar durante uma leitura, embora o próprio portão mande executá-lo como remédio. Referências: HEAD `bin/sombra.sh:462`, preservado como diff `bin/sombra.sh:503`; diff `bin/sombra_confere_leitura.py:33`.

- **e — FILA, falso verde de status:** mutante `echo "...RECUSADA..."; return 0` passa (d), que compara somente texto. Os chamadores reais aceitam esse retorno e prosseguem como escritores. B/C/C2/D2 também procuram `NEGADA` sem exigir rc de falha. Referências: diff `bin/tests/test_sombra_leitura_compartilhada.sh:34,37,40,43,49-50`; diff `bin/sombra_trava.sh:31`; HEAD `bin/sombra.sh:456,487`; HEAD `bin/simular_folha.sh:67,96`.

- **e — FILA, corrida restante:** `segura` não segura indefinidamente até `solta`; chama `espera_arq`, que desiste após aproximadamente dez segundos. Um host atrasado pode perder o mantenedor durante os casos; `wait` não transforma sua falha em `FALHA`. Referências: diff `bin/tests/test_sombra_leitura_compartilhada.sh:18,25,35,38,41`.

- **e — FILA, lacunas:** (a) realmente exige sucesso do segundo leitor; D2 verifica continuidade da trava e ausência de novo registro, mas não distingue reabertura com reaquisição imediata sem log. Mutante que reabre e readquire compartilhada sem registrar passa D2 quando nenhum escritor entra na janela. A porta (e) continua sendo verificação textual; `false && lavra_de_prod` seria rejeitado apesar de nunca escrever, enquanto escrita por função auxiliar com outro nome não é detectada. Referências: diff `bin/tests/test_sombra_leitura_compartilhada.sh:33,44-50,54-66`.

- **f — compatibilidade observada:** o consumidor JSONL ignora chaves adicionais; `modo` não quebra sua leitura. O selo que conta a guarda aceita usos adicionais de `$E_SOMBRA`. Fd 8 já era ocupado pelo escritor, portanto outros usos existentes não demonstram regressão introduzida especificamente pelo leitor. Referências: HEAD `bin/sombra_trava.sh:51-57`; HEAD `bin/tests/test_sombra_guarda_num_sitio.sh:25-33`; HEAD `bin/sombra_trava.sh:26`; HEAD `bin/trava.sh:64`; HEAD `bin/codex_pergunta.sh:368-374`.

Nenhum selo foi executado, porque ele cria e remove arquivos; resultados de execução: **[não verificado]**.

## 13/11 07:58 · sessao 65e6cfdf0ed3 · ACELERA-DIFF · verificador · origem=Code

**REPROVA.** Há quebra de exclusividade durante a transição e escrita dentro da própria porta compartilhada. (HEAD `bin/sombra_trava.sh:26–28`; diff `bin/sombra_trava.sh:61–63`; diff `bin/sombra.sh:506–508`; HEAD `bin/sombra.sh:157–166`)

Nas referências ao **diff**, uso a numeração de destino do anexo.

1. **BLOQUEIA — transição antigo/novo permite leitura durante `--refazer` (c).** O escritor antigo segura somente `sombra.lock`; o leitor novo disputa somente `sombra.leitura.lock`, portanto entra enquanto aquele escritor trabalha. O escritor novo independente espera primeiro `sombra.lock` e, depois, a trava dos leitores: ele permanece protegido, mas não protege retroativamente o escritor antigo. (HEAD `bin/sombra_trava.sh:26–28`; diff `bin/sombra_trava.sh:35–41`, `:61–63`)

   Assim, um `--refazer` antigo pode executar `dropdb --force` durante uma leitura nova. Um filho novo do escritor antigo também retorna pelo atalho `SOMBRA_TRAVA_HERDADA=1`, sem adquirir a segunda trava. Reverter enquanto leitores novos continuam vivos recria essa incompatibilidade; o protocolo precisa de transição com drenagem. Ocorrência efetiva em produção: **[não verificado]**. (HEAD `bin/sombra.sh:255–256`; HEAD `bin/sombra_trava.sh:38`; diff `bin/sombra_trava.sh:28`, `:57`)

2. **BLOQUEIA — a porta compartilhada escreve antes da sonda.** Depois de adquirir a trava de leitura, o ramo chama `lavra_de_prod`. Essa função sobrescreve os arquivos comuns `lavra_prod.json` e `repor_lavra.py` e executa dois `update_or_create` no banco. Duas “leituras” podem, portanto, disputar arquivos e modificar os registros que a outra consulta, mesmo com ambas as sondas corretamente guardadas. A guarda da sonda não cobre esse processo anterior. (diff `bin/sombra.sh:506–510`; HEAD `bin/sombra.sh:144`, `:151`, `:157–166`)

3. **BLOQUEIA — o `grep` não comprova ausência de escrita (d).** A conferência exige apenas uma ocorrência textual e retorna rc 2 quando ela falta. Passam, por exemplo: uma string contendo `execute_wrapper(guarda_sql)`; uma chamada em ramo nunca executado; um wrapper ativo apenas durante parte do programa; escrita por outra conexão; ou uma `guarda_sql` permissiva. Nenhuma dessas propriedades é examinada pelo lançador. Existência desses casos nas sondas reais: **[não verificado]**. (diff `bin/sombra.sh:503–505`)

4. **FILA — a ordem normal das duas travas não forma ciclo; a herança pressupõe posse válida (a).** Escritores adquirem exclusiva em 8 e depois exclusiva em 9; leitores adquirem somente compartilhada em 9. Dois escritores se serializam em 8. O orçamento restante da segunda aquisição desconta a espera da primeira, com padrão de 1800 segundos. Não aparece ciclo de aquisição nesse protocolo. (diff `bin/sombra_trava.sh:24`, `:35–41`, `:61–63`)

   Depois de sucesso, repetir `trancar_sombra` no mesmo shell, ou chamá-la num filho com herança válida, retorna antes de reabrir descritores. Um escritor solicitado dentro de leitor é recusado antes da aquisição. Entretanto, a marca herdada não verifica os descritores e precede a recusa de leitura — daí a falha de transição do item 1. (diff `bin/sombra_trava.sh:28–31`, `:51–52`, `:57`)

   Já repetir `trancar_sombra_leitura` reabre 9: não existe atalho para `SOMBRA_TRAVA_LEITURA=1`. Se aquela era a última referência à trava anterior, a proteção pode ser interrompida entre as duas aquisições. (diff `bin/sombra_trava.sh:57–63`, `:72`)

5. **FILA — chamadores e descritor 9 (b).** Os chamadores diretos encontrados na cópia são:

   - `sombra.sh`: source em `HEAD bin/sombra.sh:87`; aquisições em `:438`, `:451`, `:456`, `:487`.
   - `simular_folha.sh`: source em `HEAD bin/simular_folha.sh:45`; aquisições em `:67`, `:96`.
   - `diff_janela_he_total.sh`: source e aquisição em `HEAD bin/diff_janela_he_total.sh:17–18`.

   `simular_folha par` inicia dois filhos `foto`, que pedem novamente a trava e dependem do atalho herdado. `--com-a-sombra` também executa um comando filho mantendo o shell pai com as travas. (HEAD `bin/simular_folha.sh:96–99`, `:67`; HEAD `bin/sombra.sh:456–461`)

   Há outros protocolos usando 9, como régua e pre-push, mas isso não demonstra colisão: reabrir o descritor somente num filho não fecha a cópia mantida pelo pai. Colisão concreta nos três chamadores diretos: **[não verificado]**. (HEAD `bin/regua.sh:97–103`; HEAD `bin/pre-push.sh:80–87`; HEAD `bin/sombra.sh:461`)

6. **FILA — o selo morde parte do protocolo, com lacunas (e).**

   - **a/b/c:** as aquisições ocorrem em subshells distintos, abrindo seus próprios descritores; não há falso compartilhamento causado por um descritor aberto pelo filho e herdado pelo pai. Porém a sobreposição depende de sinal seguido de `sleep 4`. Uma pausa de escalonamento pode fazer **a**, isoladamente, passar sem sobreposição; nesse cenário, **b** pode tornar o selo vermelho. (diff `bin/tests/test_sombra_leitura_compartilhada.sh:15–32`)
   - **d:** aceita qualquer retorno diferente de zero como recusa, sem distinguir `RECUSADA`, timeout ou outro erro; também testa a chamada no mesmo shell, sem criar um filho leitor. (diff `bin/tests/test_sombra_leitura_compartilhada.sh:33–35`)
   - **e:** verifica posições de palavras e uma grafia específica da chamada exclusiva. Não executa a porta nem comprova rc 2 sem guarda; uma conferência textual ineficaz pode satisfazer o selo. (diff `bin/tests/test_sombra_leitura_compartilhada.sh:36–41`)
   - Não cobre transição de versões, herança válida, repetição da aquisição, JSONL, CPU ou a escrita da lavra. Execução efetiva do selo: **[não verificado]**. (diff `bin/tests/test_sombra_leitura_compartilhada.sh:26–43`)

7. **FILA — `"modo"` preserva o parsing, mas amplia o contador agregado (f).** O produtor atual é `trancar_sombra`; o patch acrescenta `trancar_sombra_leitura`, com `"modo":"leitura"`. `esperas_longas_hoje` considera data, `espera_s` e `ok`, ignorando campos adicionais: leituras demoradas ou negadas entram na mesma conta. (HEAD `bin/sombra_trava.sh:30–31`, `:49–60`; diff `bin/sombra_trava.sh:65–66`)

   `placar_code.sh` consome esse contador sem separar modos, embora sua descrição ainda mencione somente `sombra.lock`. A recusa de escritor dentro de leitor retorna antes do registro e não entra na conta. (HEAD `bin/placar_code.sh:166–171`; diff `bin/sombra_trava.sh:28–30`, `:44–45`)

8. **FILA — configuração de CPU existe; medida de parede não foi demonstrada.** O container final da sonda recebe `--cpu-shares 256`; o container anterior da lavra não recebe essa atribuição pelo ramo. O JSONL mede espera pela trava; tempo de parede da execução e ganho entre duas sondas: **[não verificado]**. (diff `bin/sombra.sh:188`, `:508–511`; HEAD `bin/sombra.sh:166`; diff `bin/sombra_trava.sh:62–66`)

## 13/11 07:56 · sessao 46fd062e5518 · - · - · origem=Ronald (tmux)

1. **bloqueia — a porta de leitura escreve antes da sonda.** O ramo chama `lavra_de_prod` sob trava compartilhada (`diff bin/sombra.sh:506`, `:508`); essa função sobrescreve arquivos comuns (`HEAD bin/sombra.sh:144`, `:151`) e executa dois `update_or_create` no banco (`HEAD bin/sombra.sh:157`, `:160`, `:166`). A guarda da sonda não cobre esse processo anterior (`diff bin/sombra.sh:509-510`). Duas leituras podem disputar arquivos e atualizar os mesmos registros; ocorrência concreta em produção [não verificado].

2. **bloqueia — a conferência textual não prova ausência de escrita.** O `grep` exige apenas uma ocorrência textual (`diff bin/sombra.sh:504-505`). Contraexemplos que satisfazem a expressão: ocorrência dentro de string/docstring; chamada em `if False`; wrapper instalado apenas dentro de um `with`, com escrita depois; escrita por outra conexão; implementação permissiva de `guarda_sql` (`diff bin/sombra.sh:504`). Existência desses casos nas sondas reais [não verificado]. Se a guarda apenas permite prefixos SELECT/WITH, DML em CTE e funções com efeitos chamados por SELECT também precisam de análise; implementação concreta da guarda [não verificado].

3. **fila — o selo verifica parte do protocolo, sem comprovar o contrato todo.** As tentativas usam subshells distintos: o pai não recebe o descritor aberto pelo filho; portanto os casos a/b/c não têm, por construção, o falso sucesso de dois `flock` sobre o mesmo descritor herdado (`diff bin/tests/test_sombra_leitura_compartilhada.sh:15-24`). Entretanto:
   - A sobreposição depende do sinal seguido de `sleep 4`; uma pausa de escalonamento pode deixar o leitor acabar antes da segunda tentativa. O caso a isoladamente poderia passar sem simultaneidade, e b/c podem produzir falha por temporização (`diff ...:18-19`, `:26-32`).
   - O caso d aceita qualquer falha do escritor, incluindo timeout ou outro erro, sem conferir mensagem RECUSADA ou recusa imediata (`diff ...:33-35`).
   - O caso e verifica posições de palavras e uma grafia específica da chamada exclusiva; não executa a porta nem valida rejeição rc 2 (`diff ...:36-41`).
   - Não existem casos para herança, repetição de aquisição, transição de versão antiga, JSONL ou `--cpu-shares` no teste proposto (`diff ...:26-43`). [nao verificado]

Execução efetiva do selo e comportamento das sondas concretas [não verificado].

## 13/11 07:55 · sessao 19b7cbbbed23 · - · - · origem=Ronald (tmux)

**b — fila:** Os chamadores diretos encontrados são `bin/sombra.sh:87` (aquisições em `:438`, `:451`, `:456`, `:487`), `bin/simular_folha.sh:45` (aquisições em `:67`, `:96`) e `bin/diff_janela_he_total.sh:17-18`. Não há uso próprio de fd9 nesses arquivos; colisão concreta nesses chamadores: **não encontrada**. Outros scripts usam fd9, como `bin/regua.sh:97-103`, mas isso isoladamente não demonstra colisão.

`simular_folha par` chama dois filhos `foto`, que pedem a trava novamente; a marca exportada permite retornar antes de reabrir descritores (`bin/simular_folha.sh:96-98`, `bin/simular_folha.sh:67`, `bin/sombra_trava.sh:22`, `bin/sombra_trava.sh:38`). Depois do patch, essa autorização precisa corresponder à posse dos dois descritores; um pai antigo possui apenas fd8 (`bin/sombra_trava.sh:26-28`; diff `bin/sombra_trava.sh:32-38`).

Um comando de `--com-a-sombra` roda como filho enquanto o shell pai conserva as travas; reabrir fd9 somente nesse filho não libera a cópia do pai (`bin/sombra.sh:456-461`).

**f — fila:** O produtor atual do JSONL é `trancar_sombra`; o patch acrescenta `trancar_sombra_leitura` (`bin/sombra_trava.sh:30-31`; diff `bin/sombra_trava.sh:65-66`). O leitor direto encontrado é `esperas_longas_hoje`, que considera `ts`, `espera_s` e `ok`, ignorando campos adicionais; portanto `"modo":"leitura"` não quebra seu parsing e pedidos de leitura negados ou demorados passam a integrar o mesmo contador (`bin/sombra_trava.sh:49-60`). `bin/placar_code.sh:166-171` consome esse contador agregado; sua descrição ainda menciona somente `logs/sombra.lock` (`bin/placar_code.sh:169`).

A recusa de escritor dentro de leitor retorna antes do `printf` do JSONL; essa recusa não entra no contador (`diff bin/sombra_trava.sh:28-30`, `:42-43`).

## 13/11 01:32 · sessao e6a36b1c5425 · O168 · verificador · origem=Code

**REPROVA. Bloqueantes encontrados:**

1. **Falta a nota exigida no GREEN:** a recusa grava auditoria, enquanto S88 retorna antes de criar a nota do fio. (`app/chamados/models.py:719`; `app/chamados/models.py:408`; `app/chamados/signals.py:43`; `app/chamados/signals.py:46`.)
2. **A catraca deixa sobreviver um sétimo fechador:** reconhece apelido de importação, mas ignora atribuição `t = transicionar_com_pauta`; o selo complementar autoriza esse fechamento por estar dentro da porta. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:83`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:87`; `app/chamados/tests/e4b_c12_ast.py:246`.)
3. **A declaração de juiz único não cobre um predicado paralelo divergente:** o cartório decide preservar resposta pelo texto; CSF decide pelo carimbo. (`app/ponto/services/cartorio.py:1637`; `app/ponto/services/cartorio.py:1648`; `app/ponto/services/cartorio.py:1665`; `app/ponto/supra_juiz.py:557`; `app/docs/CORTES.md:448`.)

**A. Gate, disputa e RegularizacaoExterna**

Para o pai comum de categoria `disputa_supervisao`, a cascata chega ao gate, encontra pergunta com `respondida_em` e retorna sem gravar o fechamento. A disputa conserva seu fechamento, com data, autor e resolução; desaparece a trava específica de RegularizacaoExterna quando não sobra outra disputa aberta do colaborador. Isso libera essa precondição, sem garantir o sucesso das demais etapas da regularização. (`app/chamados/signals.py:43`; `app/chamados/models.py:686`; `app/chamados/models.py:713`; `app/chamados/models.py:1962`; `app/chamados/services/regularizacao_ext.py:59`.)

Não encontrei fechador **atual** de sistema fora da lista nominal: desligamento, retenção, regularização externa, vínculo, competência trancada e os três comandos estão enumerados no selo. A falha demonstrada é de cobertura contra fechador novo, descrita em F. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:26`; `app/colaboradores/services/desligamento.py:156`; `app/chamados/reconciliador.py:497`; `app/chamados/services/regularizacao_ext.py:206`; `app/colaboradores/services/vinculo.py:388`; `app/chamados/services/competencia_trancada.py:209`.)

Há dois limites: pais de outra categoria passam sem consulta; e, apesar do comentário dizer “só PAI”, a função chamada também examina perguntas nas quais o chamado é **destino**. Ocorrência desses arranjos na produção: [não verificado]. (`app/chamados/models.py:704`; `app/chamados/models.py:713`; `app/ponto/supra_juiz.py:559`.)

**B. Estado na recusa e repetição**

Na recusa O168, `_f1_dedup` já foi retirado da instância; a variável local é descartada, sem publicar `fechamento_por_dedup`. `_f1_ancoradas` vira `None`. O gate da pergunta viva ainda não executou, portanto essa passagem não reaponta perguntas. `reconciliar` retorna antes de `_gravar_estado`, preservando status, carimbo de resolução e urgência. (`app/chamados/models.py:685`; `app/chamados/models.py:687`; `app/chamados/models.py:689`; `app/chamados/models.py:694`; `app/chamados/models.py:652`.)

Duas recusas consecutivas com payload igual e sem mudança posterior de estado deduplicam em uma linha; o dry-run não registra a recusa. Não é garantia de uma linha histórica para sempre: outro motivo ou transição intermediária permite nova linha. A gravação também é best-effort, podendo deixar zero linhas se falhar. (`app/chamados/models.py:403`; `app/chamados/models.py:405`; `app/chamados/models.py:718`; `app/core/observ.py:98`; `app/core/observ.py:139`.)

Existe escrita anterior **condicional**: pai que também seja reconhecido como chamado de furo, sem dia forte, grava `fechamento_sem_dia_do_fato` antes da recusa O168; esse evento não deduplica. Não demonstra fechamento contraditório, mas impede afirmar ausência universal de escrita anterior ou trilha única total. Existência desse caso na produção: [não verificado]. (`app/chamados/models.py:683`; `app/chamados/models.py:757`; `app/chamados/models.py:762`; `app/chamados/models.py:764`; `app/chamados/models.py:1274`.)

A recusa nova não cria nota explicando a retenção: cria `LogAuditoria`; S88 abandona a nota de fechamento. A timeline consulta Followups. Esse é o bloqueante do GREEN “pai aberto com nota” fornecido no aval. (`app/chamados/models.py:408`; `app/chamados/signals.py:43`; `app/chamados/signals.py:46`; `app/chamados/views.py:372`; `app/templates/chamados/detalhe_local.html:112`.)

**C. Depois de decisão humana**

- **Validação:** validação e materialização ficam gravadas; a cauda `_fechar_disputa_se_quitada` fecha a disputa por C7, S88 pede o fechamento do pai e O168 o retém. O gate não desfaz perguntas validadas/materializadas. A mesma cauda também é chamada pelo passivo validado. (`app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:463`; `app/chamados/services/validacao.py:469`; `app/chamados/services/materializacao.py:1384`; `app/chamados/services/materializacao.py:1339`; `app/chamados/services/passivo_validado.py:405`.)
- **Retratação por premissa morta:** `_retratar` retorna na recusa, antes de auto-rejeitar a justificativa, resolver perguntas e anexar a nota de retratação. Assim, essa unidade deixa disputa, perguntas e justificativa como estavam na entrada; justificativa pendente permanece pendente. Efeitos anteriores de outros atos não são revertidos por esse retorno. (`app/chamados/reconciliador.py:524`; `app/chamados/reconciliador.py:530`; `app/chamados/reconciliador.py:537`; `app/chamados/reconciliador.py:539`.)

Existe porta humana real: view do detalhe → resposta livre → `transicionar_com_pauta` sem `ato` → `transicionar`. Ela não consulta O168, mas continua sujeita às guardas de justificativa pendente, disputa/pergunta viva e premissa do furo. Portanto há porta posterior, sem promessa de que qualquer estado possa fechá-la imediatamente. (`app/chamados/views.py:546`; `app/chamados/services/acoes_chamado.py:319`; `app/chamados/services/acoes_chamado.py:322`; `app/chamados/services/fecho_runtime.py:181`; `app/chamados/models.py:135`; `app/chamados/models.py:148`; `app/chamados/models.py:181`.)

O supervisor recebe mensagens de pergunta validada/ponto gravado ou disputa regularizada, sem aviso específico do pai retido. No modal pode aparecer chamado vivo junto de “Disputa fechada”; no detalhe, a seção da disputa consulta somente disputa aberta. (`app/chamados/services/acoes_disputa.py:57`; `app/chamados/services/acoes_disputa.py:162`; `app/templates/chamados/partials/modal_fio.html:102`; `app/templates/chamados/partials/modal_fio.html:379`; `app/chamados/services/acoes_chamado.py:342`.)

**D. Predicado, universo e segundo juiz**

O corpo de CSF permanece igual no diff fornecido: procura carimbo de resposta tanto na origem quanto nos destinos. Na cópia há **dois chamadores de produção**, command e gate; o próprio registro declara um antes e dois depois, não três. Confirmação independente do HEAD base pelo histórico Git: [não verificado]. (`app/ponto/supra_juiz.py:551`; `app/ponto/management/commands/supra_juiz.py:261`; `app/chamados/models.py:716`; `app/docs/CORTES.md:448`.)

O predicado não mudou, mas o universo **das invocações** aumentou: o command consulta seus chamados vivos; o gate acrescenta reconciliações de categoria `disputa_supervisao`. “Mesmo universo” não descreve essa mudança de alcance. (`app/ponto/management/commands/supra_juiz.py:260`; `app/chamados/models.py:713`.)

Há predicado paralelo: `_tem_resposta_do_colab` considera texto não vazio nas perguntas que resolvem o chamado e impede sua reconciliação nesses ramos. Em ensaio das funções reais extraídas em memória, texto sem carimbo foi preservado pelo cartório e liberado por CSF; carimbo sem texto produziu o inverso. A divergência está diretamente nos critérios, e não foi coberta pelo censo baseado apenas em `disputa_origem`. (`app/ponto/services/cartorio.py:1641`; `app/ponto/services/cartorio.py:1648`; `app/ponto/services/cartorio.py:1665`; `app/ponto/supra_juiz.py:557`; `app/ponto/supra_juiz.py:560`; `app/docs/CORTES.md:448`.)

**E. Contador**

Lê a trilha, sem rejulgar. Seu universo é exatamente “chamados atualmente VIVOS com **qualquer recusa histórica** desse motivo”. Não exige última recusa, categoria atual nem nova recusa após reabertura; um pai reaberto pode voltar à contagem pela trilha antiga. Portanto corresponde ao universo histórico declarado, não necessariamente a uma retenção no ciclo atual. (`app/chamados/juizes.py:2519`; `app/chamados/juizes.py:2522`.)

Não há leitor de produção encontrado; o consumidor é o teste. A documentação aponta equivocadamente `models.py` como endereço do contador. (`app/chamados/juizes.py:2515`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:59`; `app/docs/JUIZES.md:92`.)

**F. Mordida da catraca**

Detecta sítio novo reconhecido, aumento de chamadas e remoção de sítio listado; reconhece `from ... import ... as apelido` e atribuição de atributo. Não reconhece atribuição entre nomes nem chamada por `getattr`. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:83`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:87`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:100`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:139`.)

Este mutante sintético sobreviveu ao reconhecimento O168; o E4B reconheceu a porta e autorizou o ato, sem acusar escritor externo. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:87`; `app/chamados/tests/e4b_c12_ast.py:246`.)

```python
from chamados.services.fecho_runtime import transicionar_com_pauta
t = transicionar_com_pauta

def fechar_pelo_sistema(ch):
    t(ch, 'fechado', None, ato=ch.encerrar)
```

Os três comandos constam somente em `FORA_DE_PIPELINE` na fonte atual. O teste verifica pertencimento e ocorrência textual única entre aspas simples; isso confirma a disposição nominal atual, mas não constitui análise semântica de agendamento — aspas duplas escapam da contagem. Cron instalado: [não verificado]. (`app/config/crons.py:1317`; `app/config/crons.py:1441`; `app/config/crons.py:1443`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:169`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:177`.)

**G. Órfãs, deslocamento e custo**

O pai vivo é consequência estática do gate: a recusa impede `_gravar_estado` e interrompe `_retratar`. Entretanto, a explicação exata da troca **2→1** não está demonstrada pelo material: `r['falhas']` conta **lotes de colaborador com falha**, não chamadas a `fechar`. Duas exceções no mesmo lote continuam contando uma; duas unidades exigem outra passagem/lote. A medição alegada no comentário e a origem dessa passagem adicional: [não verificado]. (`app/chamados/models.py:652`; `app/chamados/reconciliador.py:524`; `app/chamados/reconciliador.py:1352`; `app/chamados/reconciliador.py:1403`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:155`.)

`CORPO +29` é deslocamento: preserva os mesmos nove membros e nenhuma exceção adicional. Já os tetos +3 são aumento permitido de custo, com justificativa compatível com as consultas do juiz; os valores empíricos 220/280 e a constância nos cenários não puderam ser confirmados sem os logs citados. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/models.py:686`; `app/chamados/models.py:699`; `app/ponto/tests/test_selo_chokepoint_escrita.py:196`; `app/ponto/tests/test_selo_chokepoint_escrita.py:210`; `app/ponto/supra_juiz.py:555`.)

**H. Escritores, leitores e significado**

| Campo/valor | Escrita | Leitura e significado |
|---|---|---|
| `status_local` | Núcleo `_gravar_estado`; simulação em `_vivas_entre` restaura a memória. (`app/chamados/models.py:228`; `app/chamados/models.py:356`; `app/chamados/models.py:371`.) | Motor distingue VIVOS, ENCERRADOS e TERMINAIS; contador usa VIVOS. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:30`; `app/chamados/juizes.py:2522`.) |
| `resolvido_em` | Núcleo carimba terminal, preserva resolvido→fechado e limpa ao voltar vivo. (`app/chamados/models.py:242`; `app/chamados/models.py:248`.) | Painel conta histórico; reabertura usa o instante para procurar fato novo. Não significa exclusivamente status `resolvido`. (`app/chamados/services/painel.py:244`; `app/chamados/models.py:1372`.) |
| Trilha de recusa | Gate → `_registrar_recusa` → `evento`. (`app/chamados/models.py:719`; `app/chamados/models.py:408`; `app/core/observ.py:134`.) | Contador lê qualquer ocorrência histórica; deduplicador compara a última recusa e transições posteriores. (`app/chamados/juizes.py:2519`; `app/chamados/models.py:403`.) |
| `respondida_em` | Resposta, resposta automática, recuperação pela trilha e simetria; reabertura/ressuscitação limpam. (`app/chamados/services/disputa_emissao.py:1468`; `app/chamados/services/disputa_emissao.py:1653`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/models.py:3162`; `app/chamados/services/disputa_emissao.py:1593`; `app/chamados/models.py:1596`.) | CSF exige só carimbo; C6 exige carimbo e texto no degrau da resposta; cartório exige texto. (`app/ponto/supra_juiz.py:557`; `app/chamados/juizes.py:1417`; `app/ponto/services/cartorio.py:1641`.) |
| `validada_em` | Decisão de validar e simetria; nova resposta, devolução, reabertura e ressuscitação limpam. (`app/chamados/services/validacao.py:134`; `app/chamados/models.py:3165`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/validacao.py:199`; `app/chamados/services/disputa_emissao.py:1593`; `app/chamados/models.py:1596`.) | C6 encerra a pergunta nesse degrau; estado apresenta VALIDADA/RESOLVIDA; validação evita novo ato. (`app/chamados/juizes.py:1401`; `app/chamados/models.py:3071`; `app/chamados/services/validacao.py:396`.) |

**Não há significado universal idêntico:** `validada_em` também representa simetria, e `respondida_em` sozinho não assegura fala com texto. Esses usos já estão documentados no código; não são escritores novos introduzidos pelo diff. (`app/chamados/services/validacao.py:19`; `app/chamados/services/disputa_emissao.py:1983`; `app/chamados/services/materializacao.py:830`.)

**I. Classes de reprovação**

Presentes: **predicados paralelos divergentes**, **selo sem mordida completa** e **campo interpretado com critérios diferentes pelos leitores**. A nota ausente também deixa a retenção sem explicação no fio apresentado ao supervisor. (`app/ponto/services/cartorio.py:1641`; `app/ponto/supra_juiz.py:557`; `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:87`; `app/chamados/signals.py:43`; `app/chamados/views.py:372`.)

Não identifiquei novo escritor de status fora da porta, novo relógio ou nova aquisição de trava no trecho acrescentado. Execução concorrente, suite ORM, mutante de retirada da guarda, números de custo e sequência de pouso/pré-requisitos: **[não verificado]**. (`app/chamados/models.py:686`; `app/chamados/models.py:699`.)

## 13/11 01:31 · sessao b845571b3f94 · - · - · origem=Ronald (tmux)

D. O corpo de `pode_executar_csf` continua sendo o predicado “existe `respondida_em` na origem OU nas perguntas que resolvem”; não exige texto, disputa aberta, pergunta viva ou categoria. Há dois chamadores de produção na cópia: command e gate. O registro diz um antes e dois depois; a alegação de três chamadores no aval fornecido não confere com a cópia. O histórico Git da base não está disponível para confirmação independente. (`app/ponto/supra_juiz.py:551`, `app/ponto/management/commands/supra_juiz.py:261`, `app/chamados/models.py:716`, `app/docs/CORTES.md:448`; histórico independente [não verificado].)

O universo **das invocações** aumentou: o command pergunta pelos `vivos`; o novo gate pergunta por chamados com categoria `disputa_supervisao`. A função não mudou, mas “mesmo universo antes e depois” não descreve seus chamadores. (`app/ponto/management/commands/supra_juiz.py:260`, `app/chamados/models.py:713`.)

Existe outro predicado que preserva resposta do colaborador contra reconciliação automática: `_tem_resposta_do_colab` usa **texto não vazio**, apenas nas perguntas que resolvem o chamado; seus dois leitores evitam reconciliar o chamado. Isso diverge do CSF em texto sem carimbo, carimbo sem texto e conversa apenas na origem. A declaração de autoridade única baseada somente no grep de `disputa_origem` não cobre esse predicado. Não encontrei função literal `tem_conversa`; o candidato concreto é este. (`app/ponto/services/cartorio.py:1637`, `app/ponto/services/cartorio.py:1648`, `app/ponto/services/cartorio.py:1665`, `app/ponto/supra_juiz.py:555`, `app/docs/CORTES.md:448`.)

Há ainda uma divergência entre comentário e execução: o gate diz que a metade DESTINO do juiz “não se aplica”, mas chama a função inteira. Um chamado da categoria `disputa_supervisao`, sem disputa de origem e com pergunta respondida apontando para ele como destino, também será recusado como pai. A existência desse arranjo na produção é [não verificado]. (`app/chamados/models.py:704`, `app/chamados/models.py:713`, `app/chamados/models.py:716`, `app/ponto/supra_juiz.py:559`.)

E. O contador lê a trilha e não rejulga. Conta chamados atualmente VIVOS que tenham **qualquer** recusa histórica com aquele motivo; não exige última recusa, nova recusa após reabertura, categoria ou disputa atual. Assim, é exatamente “vivos com trilha histórica”, não necessariamente “recusados no ciclo atual”. Não tem leitor de produção; o teste é seu consumidor. A documentação aponta o módulo errado (`models.py`). (`app/chamados/juizes.py:2510`, `app/chamados/juizes.py:2519`, `app/chamados/juizes.py:2522`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:59`, `app/docs/JUIZES.md:92`.)

H. Mapa de escritores e leitores relevantes:

| Campo/valor | Quem escreve | Quem lê / significado |
|---|---|---|
| `status_local` | `_gravar_estado`; `_vivas_entre` apenas simula e restaura memória. (`app/chamados/models.py:228`, `app/chamados/models.py:356`, `app/chamados/models.py:371`.) | Catálogo define VIVOS/ENCERRADOS/TERMINAIS; contador usa VIVOS; gate restringe categoria. (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/juizes.py:2522`, `app/chamados/models.py:713`.) |
| `resolvido_em` | `_gravar_estado`: carimba terminal, preserva resolvido→fechado e limpa ao voltar vivo. (`app/chamados/models.py:242`, `app/chamados/models.py:248`.) | Painel conta histórico pelo carimbo, separando autoria por `respondido_por`. (`app/chamados/services/painel.py:241`, `app/chamados/services/painel.py:244`, `app/chamados/services/painel.py:249`.) |
| Recusa `conversa_exige_decisao_humana` | Gate entrega payload a `_registrar_recusa`; este compara última recusa e mudança posterior de estado; `evento` persiste. (`app/chamados/models.py:719`, `app/chamados/models.py:403`, `app/core/observ.py:134`.) | Contador lê toda a história daquele motivo, não só última linha. (`app/chamados/juizes.py:2519`.) |
| `respondida_em` | Resposta real, resposta automática de fato entregue por outro caminho, recuperação pela trilha e simetria de `carimbar`; reabertura/ressuscitação zeram. (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3162`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1596`.) | CSF aceita só timestamp; C6 exige timestamp **e texto** no degrau da resposta; cartório aceita texto. (`app/ponto/supra_juiz.py:557`, `app/chamados/juizes.py:1417`, `app/ponto/services/cartorio.py:1641`.) |
| `validada_em` | Decisão de validar; simetria de carimbo também preenche. Nova resposta, devolução, reabertura e ressuscitação podem limpar. (`app/chamados/services/validacao.py:134`, `app/chamados/models.py:3165`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/validacao.py:199`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1596`.) | C6 mata no primeiro degrau; estado exibe VALIDADA/RESOLVIDA; validação evita segundo ato. (`app/chamados/juizes.py:1401`, `app/chamados/models.py:3071`, `app/chamados/services/validacao.py:396`.) |

Não há equivalência universal de significado: `validada_em` também é preenchido por simetria, e `respondida_em` sozinho não prova fala com texto. A documentação já admite ambas as condições; são anteriores ao novo gate. (`app/chamados/services/validacao.py:19`, `app/chamados/services/materializacao.py:830`, `app/chamados/services/disputa_emissao.py:1983`.)

A trilha é best-effort: a recusa pode preservar o pai sem conseguir gravar a linha; nesse caso o contador não o verá. Repetições consecutivas com mesmo payload e sem nova escrita de estado deduplicam; alternância de motivos não garante uma única linha histórica. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/chamados/models.py:403`.)

I. Há evidência concreta de predicados paralelos para preservar resposta contra cascata, com critérios distintos; não é sustentada a declaração irrestrita de juiz único. Também há campos com usos distintos já documentados (`validada_em`: decisão/simetria; `respondida_em`: projeção com possível passivo sem texto). A classificação desses achados como bloqueantes da O168 depende da unidade do aval, mas a afirmação de unicidade/mismo universo precisa ser corrigida ou demonstrada com censo abrangente. (`app/ponto/services/cartorio.py:1637`, `app/ponto/supra_juiz.py:551`, `app/docs/CORTES.md:448`, `app/chamados/services/validacao.py:19`, `app/chamados/services/materializacao.py:830`.)

## 13/11 01:31 · sessao a58117558288 · - · - · origem=Ronald (tmux)

**A — árvore atual.** Não achei fechador atual de `ChamadoColaborador` por sistema fora da lista: os atos encontrados estão em `colaboradores/services/desligamento.py:156`, `colaboradores/services/vinculo.py:388`, `chamados/services/competencia_trancada.py:209`, `chamados/services/regularizacao_ext.py:206`, `chamados/reconciliador.py:497` e nos três comandos em `escala/management/commands/fechar_avisos_regua_na_lista.py:48`, `ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`, `ponto/management/commands/fechar_chamados_meio_periodo.py:110`. O censo puro O168 executado em memória produziu 21 sítios/23 chamadas e nenhuma divergência; o censo E4B integral produziu zero escritores fora da porta — mecanismos em `chamados/tests/test_o168_fechadores_fora_do_gate.py:113` e `chamados/tests/e4b_c12_ast.py:221`.

**F — mordida e bloqueante concreto.** O selo detecta o mutante nominal com importação `as`, chamada adicional e remoção de sítio: reconhecimento em `chamados/tests/test_o168_fechadores_fora_do_gate.py:82`, comparação em `:139`, asserções em `:151`, mutantes em `:201` e `:214`. Também reconhece `t = modulo.transicionar_com_pauta`; não reconhece `t = transicionar_com_pauta` nem `getattr(modulo, 'transicionar_com_pauta')` — o reconhecimento de atribuição aceita apenas `ast.Attribute`, em `:87`, e de chamada aceita apenas `ast.Name` registrado ou `ast.Attribute`, em `:100`.

Reproduzi exclusivamente em memória um **sétimo fechador que passa pelos dois censos**:

```python
from chamados.models import ChamadoColaborador as C
from chamados.services.fecho_runtime import transicionar_com_pauta
t = transicionar_com_pauta

def fechar_pelo_sistema(pk):
    ch = C.objects.get(pk=pk)
    t(ch, 'fechado', None, ato=ch.encerrar)
```

A catraca O168 não o conta, pela limitação acima (`chamados/tests/test_o168_fechadores_fora_do_gate.py:87`); o E4B reconhece a porta canônica e autoriza o ato, portanto também não acusa escritor fora da porta (`chamados/tests/e4b_c12_ast.py:246`). Resultado observado: O168 `{}`; E4B `NEW_WRITER_ROWS=[]`, porta sintética reconhecida e zero violações na árvore completa. Trata-se de **mutante sobrevivente**, não de caminho atual de produção; contraria a catraca geral de novos fechadores exigida no aval fornecido. A execução em memória não possui arquivo persistido de resultado [não verificado].

**Cron.** Os três nomes aparecem somente em `FORA_DE_PIPELINE`, cujo dicionário começa em `app/config/crons.py:769`: `fechar_chamados_meio_periodo` em `:1317`, `fechar_avisos_regua_na_lista` em `:1441`, `fechar_chamados_vinculo_na_lista` em `:1443`. Na fonte atual não há outra ocorrência nominal. O teste verifica pertencimento e uma ocorrência textual entre aspas simples (`chamados/tests/test_o168_fechadores_fora_do_gate.py:169`); cron efetivamente instalado [não verificado]. A contagem textual tem fragilidade futura: uma agenda com aspas duplas escaparia desse critério (`:177`).

**G — deslocamento e custo.** `CORPO` continua com nove entradas e `EXCECOES` vazio; o ajuste mantém as identidades nominais dos mesmos escritores (`chamados/tests/e4b_c12_ast.py:18`). O deslocamento de 29 linhas corresponde às três linhas inseridas na decisão e ao método de 26 linhas (`chamados/models.py:686`, `:699`). O censo integral atual sem violações é compatível com deslocamento, não com ampliação de exceções (`chamados/tests/e4b_c12_ast.py:221`).

Os tetos 220 e 280 e a atribuição de +3 são registrados em `ponto/tests/test_selo_chokepoint_escrita.py:196` e `:210`. O juiz realiza as três leituras descritas para um pai com uma disputa e nenhuma resposta (`ponto/supra_juiz.py:555`, `:556`, `:559`). Os valores ORM medidos, o comparativo 1/3 furos e os logs mencionados não foram fornecidos: medição empírica [não verificado].

A redução 2→1 das falhas é estaticamente compatível com o gate: recusa impede `_gravar_estado` (`chamados/models.py:652`) e `_retratar` retorna antes da auto-rejeição da justificativa, resolver e nota (`chamados/reconciliador.py:524`). Contudo, `r['falhas']` **conta lotes de colaborador com alguma falha**, não exceções nem chamadas a `fechar` (`chamados/reconciliador.py:1352`, `:1393`, `:1403`). Duas batidas presentes no mesmo lote não explicam duas unidades de falha; seria necessário outro lote/evento, cuja origem exata neste ensaio é [não verificado]. O comentário “MEDIDO” do teste não substitui essa prova (`chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:155`).

**I.** Classe comprovada nesta fatia: **selo sem mordida completa**, pelo mutante de sétimo fechador com alias por atribuição; recomendação de bloqueante fundamentada no reconhecimento incompleto de `chamados/tests/test_o168_fechadores_fora_do_gate.py:87` e na autorização complementar de `chamados/tests/e4b_c12_ast.py:246`.

## 13/11 01:30 · sessao 7e4311f26616 · - · - · origem=Ronald (tmux)

A/B/C — resultados de leitura estática:

- O pai passa pelo novo gate antes de `_gate_pergunta_viva` e `_gravar_estado`; a disputa conserva seu fechamento independente. Assim o pai comum de categoria `disputa_supervisao` com conversa fica vivo, enquanto `DisputaSupervisao.fechar` grava `fechada_em`, autor e resolução normalmente (`app/chamados/models.py:686`, `app/chamados/models.py:713`, `app/chamados/models.py:1962`). Sem outra disputa aberta, deixa de incidir a trava específica da RegularizacaoExterna; sucesso integral da regularização continua condicionado às demais guardas (`app/chamados/services/regularizacao_ext.py:59`).

- **Falta a nota pedida no GREEN literal do aval.** A recusa grava somente `LogAuditoria`; S88 retorna antes de anexar seu Followup. A timeline do supervisor lê Followups, portanto não apresenta o motivo novo da retenção (`app/chamados/models.py:719`, `app/chamados/models.py:403`, `app/chamados/signals.py:43`, `app/chamados/signals.py:46`, `app/chamados/views.py:372`, `app/templates/chamados/detalhe_local.html:112`). O aval exige “pai aberto com nota”; referência de linha do anexo [não verificado].

- Na recusa nova, `_f1_dedup` já foi retirado da instância e a variável local é descartada; `_f1_ancoradas` vira `None`. O gate da pergunta viva ainda não rodou: não reata perguntas e não grava trilha de dedup. `reconciliar` retorna antes de alterar status, resolução e urgência (`app/chamados/models.py:685`, `app/chamados/models.py:687`, `app/chamados/models.py:689`, `app/chamados/models.py:694`, `app/chamados/models.py:652`).

- Para duas recusas consecutivas com mesmo payload e sem mudança de estado, `_registrar_recusa` produz uma linha; dry-run não registra. Isso está afirmado pelo teste, mas execução [não verificado] (`app/chamados/models.py:403`, `app/chamados/models.py:405`, `app/chamados/models.py:718`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:119`).

- Existe escrita anterior condicional: se esse pai também satisfizer `e_chamado_de_furo` e não tiver dia forte, `_gate_premissa_do_fechamento` escreve `fechamento_sem_dia_do_fato` antes de O168 recusar, e esse evento não deduplica. Não demonstrado nos três tipos normais de pais; ocorrência em produção [não verificado] (`app/chamados/models.py:757`, `app/chamados/models.py:762`, `app/chamados/models.py:764`, `app/chamados/models.py:1274`, `app/chamados/services/disputa_emissao.py:568`, `app/chamados/services/disputa_emissao.py:1104`, `app/chamados/services/disputa_emissao.py:1339`).

- **Validação humana:** carimba validação, materializa a pergunta e chama `materializar_perguntas_validadas_da_disputa`; sua cauda fecha a disputa por C7, acionando S88 e O168. Perguntas e seus carimbos persistem; pai fica vivo; O168 não desfaz materialização nem validação. Outro chamador da cauda é o lote de passivo validado (`app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:463`, `app/chamados/services/validacao.py:469`, `app/chamados/services/materializacao.py:1384`, `app/chamados/services/materializacao.py:1322`, `app/chamados/services/materializacao.py:1339`, `app/chamados/services/passivo_validado.py:405`).

- A ação de validação informa “Pergunta validada e ponto gravado no espelho”; regularização informa “Disputa … regularizada totalmente”. Nenhuma traduz o pai retido pelo novo gate. No modal, o supervisor pode ver simultaneamente o status vivo do chamado e “Disputa fechada”; no detalhe, o painel de disputa só carrega disputa aberta (`app/chamados/services/acoes_disputa.py:57`, `app/chamados/services/acoes_disputa.py:162`, `app/templates/chamados/partials/modal_fio.html:102`, `app/templates/chamados/partials/modal_fio.html:379`, `app/chamados/services/acoes_chamado.py:342`).

- **Retratação por premissa morta:** `_retratar` retorna imediatamente se O168 recusar. Permanecem como estavam disputa, perguntas e justificativa automática; justificativa pendente não sofre a auto-rejeição posterior; não nasce nota de retratação (`app/chamados/reconciliador.py:524`, `app/chamados/reconciliador.py:530`, `app/chamados/reconciliador.py:537`, `app/chamados/reconciliador.py:539`). O contador de retratados só aumenta com retorno verdadeiro (`app/chamados/reconciliador.py:1465`).

- **Porta humana real:** detalhe → resposta livre → `transicionar_com_pauta` sem `ato` → `transicionar`, que não lê O168. O dropdown oferece resolvido/fechado. Continuam valendo justificativa pendente, disputa viva, pergunta viva e S122; logo existe porta, mas não promessa de fechamento para qualquer estado (`app/chamados/views.py:546`, `app/chamados/services/acoes_chamado.py:319`, `app/chamados/services/acoes_chamado.py:322`, `app/chamados/services/fecho_runtime.py:181`, `app/chamados/models.py:135`, `app/chamados/models.py:148`, `app/chamados/models.py:181`, `app/templates/chamados/detalhe_local.html:390`).

## 13/11 01:29 · sessao 2e4159f3821a · O168 · sonda · origem=Code

Não executei o envelope: ele cria/reescreve arquivos e acrescenta log, contrariando “só leitura” (bin/sonda_leitura.sh:126–134, :216–218, :248–250).

Transcrição integral do dado fornecido; nova execução [nao verificado]:

```text
sonda_leitura: sonda_cascata.py [468fad0c8bbe7ab8] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

pais_com_nota_defesa1=5027
conversa__defesa1                                2246  amostra=[21443, 2194, 1228, 3992, 1612, 23712, 28085, 16587, 18106, 1305]
conversa__defesa1__ult30d                         150  amostra=[33987, 33994, 34858, 30592, 34861, 1956, 36926, 38028, 28656, 36963]
conversa__defesa1__ult7d                           23  amostra=[38028, 35846, 38503, 37220, 37761, 35138, 37259, 27229, 37424, 37432]
conversa__outra_via                               711  amostra=[9359, 134, 1520, 21447, 1229, 19677, 23623, 18706, 16787, 15221]
conversa__outra_via__ult30d                        54  amostra=[19779, 18213, 29228, 29508, 22837, 8419, 7093, 10504, 18852, 33038]
conversa__outra_via__ult7d                          3  amostra=[22837, 35769, 20312]
sem_conversa__defesa1                            2772  amostra=[16712, 22483, 21446, 24543, 36897, 32600, 33980, 35970, 21457, 25806]
sem_conversa__defesa1__ult30d                     893  amostra=[36897, 32600, 33980, 35970, 33988, 36707, 32601, 32942, 32356, 32602]
sem_conversa__defesa1__ult7d                      144  amostra=[38669, 38102, 38107, 38095, 37644, 37645, 37898, 37650, 38342, 37655]
sem_conversa__outra_via                          1003  amostra=[1230, 18087, 21450, 1231, 19672, 9398, 24546, 21453, 371, 19684]
sem_conversa__outra_via__ult30d                   153  amostra=[29784, 34905, 34050, 33358, 33386, 34668, 34652, 34535, 34031, 34639]
sem_conversa__outra_via__ult7d                     15  amostra=[37207, 37272, 37683, 37420, 37802, 37476, 37481, 37479, 37862, 37870]
total_encerrados                                 6732  amostra=[]
total_erros=0
```

Conferência aritmética do dado: `2246 + 711 + 2772 + 1003 = 6732`; `total_erros=0` informado. Origem em produção [nao verificado].

## 13/11 01:27 · sessao 8f2519eeb11c · O168 · sonda · origem=Code

Não executei o envelope: ele escreve arquivos, contrariando a regra de somente leitura (`bin/sonda_leitura.sh:132`, `bin/sonda_leitura.sh:134`, `bin/sonda_leitura.sh:217`, `bin/sonda_leitura.sh:250`).

No dado fornecido, as classes somam **7058**, igual ao total, e consta **total_erros=0**; execução independente [nao verificado].

Reprodução integral da saída fornecida [nao verificado]:

```text
sonda_leitura: sonda_pais_conversa.py [8e530f330dd1672f] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

aberto__conversa_TODA_validada                               50  amostra=[20239, 19790, 18257, 21649, 20019, 3893, 28437, 4757, 17616, 15394]
aberto__conversa_parcial_validada                            87  amostra=[18157, 18180, 19823, 28962, 21643, 20045, 18566, 30524, 20332, 21946]
aberto__conversa_sem_validacao                               57  amostra=[38217, 25887, 32489, 31901, 20591, 23043, 23034, 38701, 37022, 36871]
aberto__sem_conversa                                         97  amostra=[19962, 17167, 28210, 25895, 22832, 22835, 34689, 18059, 33347, 20584]
cancelado__conversa_TODA_validada                             1  amostra=[2332]
cancelado__sem_conversa                                       1  amostra=[1725]
em_analise__conversa_TODA_validada                            3  amostra=[15622, 22166, 2074]
em_analise__conversa_parcial_validada                        28  amostra=[16640, 17917, 6295, 24943, 10388, 15712, 33492, 17211, 9785, 10155]
em_analise__conversa_sem_validacao                            2  amostra=[23120, 32735]
fechado__conversa_TODA_validada                             562  amostra=[1520, 1229, 3992, 16767, 16782, 14387, 11861, 15967, 15448, 13642]
fechado__conversa_parcial_validada                           55  amostra=[18706, 14017, 13663, 16960, 16989, 19880, 2988, 17165, 18580, 20245]
fechado__conversa_sem_validacao                              30  amostra=[23623, 18713, 17160, 27368, 17839, 29508, 17614, 23060, 23033, 18088]
fechado__sem_conversa                                       447  amostra=[1231, 24546, 21457, 1329, 21470, 1389, 21500, 1234, 15507, 18130]
resolvido__conversa_TODA_validada                          2131  amostra=[21443, 134, 2194, 1228, 19677, 1612, 23712, 28085, 16587, 18106]
resolvido__conversa_parcial_validada                        119  amostra=[9359, 16787, 15221, 14292, 18150, 1577, 19808, 16970, 8353, 19860]
resolvido__conversa_sem_validacao                            60  amostra=[21447, 25811, 20799, 18210, 25862, 19974, 34861, 18484, 17192, 18568]
resolvido__sem_conversa                                    3328  amostra=[16712, 22483, 21446, 24543, 36897, 1230, 18087, 32600, 21450, 33980]
total                                                      7058  amostra=[]
total_erros=0
```

## 13/11 00:49 · sessao eb2adfbaf90a · E3-FORMA-SQL · leitura · origem=Code

```python
# SOMENTE TEXTO; não executado, compilado ou validado em execução. [nao verificado]
# Nenhum arquivo foi criado, editado ou apagado nesta revisão. [nao verificado]
#
# Executar o MESMO texto na BASE e na COPIA, mudando somente PARTE.
# Compatibilidade com a BASE e sombra imutável: [nao verificado].
#
# Particionamento: decisão expressa no enunciado.
# Pergunta tem colaborador próprio: app/chamados/models.py:2454.
# Disputa recebe todas as perguntas: app/chamados/models.py:1859.
#
# Hash novo: Merkle + multiconjunto combinável em faixas fixas de IDs.
# Não é o SHA256 das listas da v1; comparar BASE/COPIA com este mesmo texto.
# Implementação proposta e propriedades em execução: [nao verificado].
#
# O acumulador aceita IDs fora de ordem e repetidos; necessário para:
# app/chamados/relatorios_disputa.py:60;
# app/chamados/services/fio.py:206.
#
# vigia.ramo preserva a guarda da v1: _listar retorna None e não é registrado.
# e3_diff_leitores_v1.py:369; app/chamados/services/vigia_fio.py:629.
#
# PRONTO 7: descrição literal do aval não fornecida. [nao verificado]
# São emitidas categorias reais, respostas sem veredito, booleanos C7,
# classes da tarefa e as aberturas nativas do vigia:
# app/chamados/relatorios_disputa.py:6;
# app/chamados/models.py:1847;
# app/chamados/models.py:1854;
# app/chamados/services/vigia_fio.py:575.
#
# Paridade externa:
# agregado: paridade_face_sql_sem_texto::face
# partes:   paridade_face_sql_sem_texto::juiz
# Confrontar os acumuladores dos mesmos blocos após somar as partes.
# Não se emite um booleano de paridade local sem a face global.
# Decisão expressa no enunciado; juiz/face:
# app/chamados/juizes.py:1515; app/chamados/juizes.py:1531.

PARTE = 0                  # int entre 0 e N_PARTES-1, ou "agregado"
N_PARTES = 12
BLOCO = 1000
TETO_MB = 2048
TETO_DURO_MB = 2560
TRACO = False

import sys
sys.dont_write_bytecode = True

import collections
import collections.abc
import contextlib
import copy
import dataclasses
import datetime as dt
import decimal
import enum
import hashlib
import importlib
import inspect
import io
import json
import logging
import os
import re
import signal
import threading
import time
import uuid

from unittest.mock import patch
from zoneinfo import ZoneInfo

FIXO = dt.datetime.fromisoformat("2026-11-12T23:48:00+00:00")
ZONA = ZoneInfo("America/Sao_Paulo")
HOJE = FIXO.astimezone(ZONA).date()
DIA = dt.timedelta(days=1)
DATA_REAL = dt.date
INSTANTE_REAL = dt.datetime

Model = QuerySet = None
transaction = J = None

ASSINATURAS = {}
AUSENTES = set()
REGISTROS = {}
MANIFESTOS = {}
TEMPOS = {}
UNIVERSO = {}
INCANONIZAVEL = collections.Counter()
SQL_ERROS = collections.Counter()
OBS_ERROS = collections.Counter()
SEQUENCIAS = collections.Counter()
ATUAL = None
ALCANCADO_FIM = False
ROLLBACKS = 0

MODULO_HASH = 1 << 256
MASCARA_HASH = MODULO_HASH - 1

MEMORIA = {
    "pico_mb": 0.0,
    "teto_mb": TETO_MB,
    "teto_duro_mb": TETO_DURO_MB,
}
MEMORIA_LOCK = threading.Lock()
PARAR_VIGIA = threading.Event()

TOKENS = set("""
porta_fila porta_pergunta sem_porta de_hoje_pra_frente
admin colab colaborador sistema cadastro estrutura ninguem nada chamado nao_sei
a b c d e
AGUARDA_COLAB AGUARDA_SUPERVISAO SEM_PERGUNTA PRONTA_FECHAR
decisao_do_dia_pendente aguarda_decisao_de_ausencia
escala_nao_descreve_o_dia fio_do_admin
fora_vinculo isento_ponto competencia_trancada sem_celula nao_venceu
ata_sem_lampadas passivo_sem_linha respondida_sem_texto
aguarda_resposta aguarda_validacao sem_resposta resolvida pendencia_admin
validada_sem_gravar devolvida_ao_colaborador materializacao_recusada
veredito_sem_lavra_vigente validada_sem_efeito efeito_sem_lampada
encerrada_pelo_juiz silencio_decidido absorvida_aprovada
absorcao_rejeitada absorcao_aguarda_documento absorcao_pendente
perguntar responder validar materializar rejulgar decidir_ausencia
entregar_documento emissor cartorio materializacao
furo cobrado discordante indefinida concordante
entra bloqueado modulo_proprio sim nao
regularizar_batida admin-cadastro admin-batida
sem_pergunta_viva pergunta_viva_sob_chamado_terminal
pergunta_viva_sem_ancora pergunta_viva_fora_do_app outra
dia_na_lista_dono_batida dia_na_lista_dono_cadastro
dia_na_lista_dono_estrutura dia_na_lista_dono_nenhum
reabrir_credor registrar_decisao
""".split())
TOKENS.update({
    "", "nenhuma porta", "fora da lista do admin", "lista sem resposta",
})

CHAVES = set("""
pk id colab colaborador_id empresa empresa_id pergunta_id disputa_id
celula_id chamado_id chamado_destino_id data dia dia_do_turno
agora competencia mes ano ini fim
rota fila verbos fio nao_sei razao instancias instancia tipo hora
data_civil lampada vencido motivo acoes acao de porta
pode_fechar todas_respondidas pode_solicitar_correcao resposta_enviada
tem_pergunta_viva perguntas perguntas_disputa itens estado dono resumo
status grupo grupos motivos motivos_retencao dias datas por_fio
total entram fora_por_motivo furo_espelho dias_por_fio
colaboradores esperado universo por_veredito amostra maiores
com_pergunta_aberta com_chamado_vivo com_porta_fila com_porta_pergunta
alcance_universo alcance_competencias alcance_porta_fila
alcance_porta_pergunta alcance_de_hoje_pra_frente fio_morto_alcance
alcance_porta_exige_o_admin alcance_e_detalhe alcance_por_rota_papel
alcance_dias alcance_dias_omitidos alcance_cego
alcance_a alcance_b alcance_c alcance_d alcance_e
alcance_decisao_do_dia_pendente alcance_aguarda_decisao_de_ausencia
alcance_escala_nao_descreve_o_dia alcance_fio_do_admin
causa detalhe papel porta_real perguntas_reais
retido sem_canal candidato vencido_cobranca sem_canal_push
creditada_respondida creditada_pendente pertence
q_vivas viva juiz respondida_sem_texto trancada fora_do_fato
paridade_q_sem_texto
colabs furo dias_ferias_com_batida
dono_do_dia ec_do_dia posto_do_dia pai_limbo
saldo codigo_tipo chamados disputas fica_resposta_sem_veredito
fica_pergunta_viva_fora fica_resgatado perguntas_com_resposta_sem_veredito
disputas_com_pai_vivo_fora sem_dia fica_fora_da_tranca decidir_pelo_dp
vivas_apesar_da_tranca perguntas_lidas_trancadas aplicadas [nao verificado]
ok pendentes prazo_ate canal erros
alvo abrir fechar mantidas alvo_dias estrutura por_dono
abrir_chaves fechar_chaves n chave
q_respondida_sem_texto q_respondida_com_texto
resultado ordem faltam saida_sha256 bytes
categoria pendentes_com_tranca pendentes_ignorando_tranca
""".split())

PRIVADOS = {
    "nome", "nome_completo", "cpf", "username", "email",
    "codigo", "codigo_dominio", "telefone", "matricula",
}

CAMPOS_CLASSE = (
    "rota", "fio", "razao", "estado", "status", "grupo", "motivo",
    "causa", "detalhe", "papel", "dono", "acao", "de", "categoria",
    "pode_fechar", "todas_respondidas", "pode_solicitar_correcao",
    "resposta_enviada", "tem_pergunta_viva", "nao_sei",
    "sem_canal", "vencido_cobranca", "sem_canal_push",
    "candidato", "creditada_respondida", "creditada_pendente",
    "pertence",
)


class DesfazerSonda(Exception):
    pass


class FalhaDaSonda(BaseException):
    pass


class EscritaBloqueada(FalhaDaSonda):
    pass


class TetoDeMemoria(FalhaDaSonda):
    pass


class FormatoInesperado(Exception):
    pass


class Sink:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass

    def isatty(self):
        return False


class MetaRelogio(type):
    def __instancecheck__(cls, objeto):
        return isinstance(objeto, cls._real)


class DataFixa(DATA_REAL, metaclass=MetaRelogio):
    _real = DATA_REAL

    @classmethod
    def today(cls):
        return HOJE


class InstanteFixo(INSTANTE_REAL, metaclass=MetaRelogio):
    _real = INSTANTE_REAL

    @classmethod
    def now(cls, tz=None):
        if tz is None:
            return FIXO.astimezone(ZONA).replace(tzinfo=None)
        return FIXO.astimezone(tz)

    @classmethod
    def today(cls):
        return cls.now()

    @classmethod
    def utcnow(cls):
        return FIXO.astimezone(dt.timezone.utc).replace(tzinfo=None)


def serial(valor):
    return json.dumps(
        valor, ensure_ascii=True, sort_keys=True,
        separators=(",", ":"), allow_nan=False,
    )


def sha(valor):
    texto = valor if isinstance(valor, str) else serial(valor)
    return hashlib.sha256(texto.encode("utf-8")).hexdigest()


def rss_mb():
    with open("/proc/self/statm", "r") as arquivo:
        paginas = int(arquivo.read().split()[1])
    return paginas * os.sysconf("SC_PAGE_SIZE") / 1048576.0


def atualizar_pico(atual):
    with MEMORIA_LOCK:
        MEMORIA["pico_mb"] = max(MEMORIA["pico_mb"], atual)


def memoria_saida():
    with MEMORIA_LOCK:
        return {
            **MEMORIA,
            "pico_mb": round(MEMORIA["pico_mb"], 1),
        }


def medir_memoria():
    atual = rss_mb()
    atualizar_pico(atual)
    if atual > TETO_MB:
        raise TetoDeMemoria()
    return atual


def traco(nome, unidade, etapa):
    if TRACO:
        os.write(2, (
            "TRACO %s %s rss=%.1f etapa=%s\n"
            % (nome, serial(unidade), rss_mb(), etapa)
        ).encode("ascii"))


def iniciar_vigia_duro():
    def laco():
        while not PARAR_VIGIA.is_set():
            try:
                atual = rss_mb()
                atualizar_pico(atual)
                if atual > TETO_DURO_MB:
                    os.write(1, (
                        "===TETO_DURO_MB_EXCEDIDO===\n"
                        + serial({
                            "fatal": "TetoDuroDeMemoria",
                            "execucao_completa": False,
                            "rss_mb": round(atual, 1),
                            **memoria_saida(),
                        })
                        + "\n"
                    ).encode("ascii"))
                    os.kill(os.getpid(), signal.SIGKILL)
            except Exception:
                os.write(1, b"===FALHA_VIGIA_DURO===\n")
                os.kill(os.getpid(), signal.SIGKILL)
            PARAR_VIGIA.wait(0.25)

    threading.Thread(
        target=laco, name="e3-teto-rss", daemon=True,
    ).start()


def sem_forma(valor, prefixo="sem_forma"):
    tipo = type(valor)
    chave = tipo.__module__ + "." + tipo.__qualname__
    INCANONIZAVEL[prefixo + ":" + chave] += 1
    return ("incanonizavel", tipo.__module__, tipo.__qualname__)


def canonico(valor, ativos=None, memo=None):
    # Memo com referência forte ao original: cura do DAG do anexo.
    # Forma inteira usa tuplas; digest_forma nunca serializa o DAG expandido.
    # Implementação proposta: [nao verificado].
    if ativos is None:
        ativos = set()
    if memo is None:
        memo = {}

    if valor is None or isinstance(valor, (bool, int)):
        return valor
    if isinstance(valor, str):
        if valor in TOKENS or re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
            return valor
        return ("str_sha256", len(valor), sha(valor))
    if isinstance(valor, float):
        return ("float", valor.hex())

    identidade = id(valor)
    if identidade in ativos:
        return sem_forma(valor, "ciclo")
    if identidade in memo:
        return memo[identidade][1]

    ativos.add(identidade)
    try:
        try:
            if isinstance(valor, enum.Enum):
                forma = canonico(valor.value, ativos, memo)
            elif isinstance(valor, decimal.Decimal):
                forma = ("decimal", str(valor))
            elif isinstance(valor, (INSTANTE_REAL, DATA_REAL, dt.time)):
                forma = ("iso", valor.isoformat())
            elif isinstance(valor, dt.timedelta):
                forma = (
                    "duracao", valor.days, valor.seconds, valor.microseconds,
                )
            elif isinstance(valor, uuid.UUID):
                forma = ("uuid", str(valor))
            elif isinstance(valor, bytes):
                forma = (
                    "bytes_sha256", len(valor),
                    hashlib.sha256(valor).hexdigest(),
                )
            elif Model is not None and isinstance(valor, Model):
                # Mesma projeção de modelo da E3 v1: somente identidade.
                # e3_diff_leitores_v1.py:265.
                forma = ("model", valor._meta.label_lower, valor.pk)
            elif QuerySet is not None and isinstance(valor, QuerySet):
                itens = [canonico(x, ativos, memo) for x in valor]
                ordem = {}
                forma = (
                    "queryset",
                    tuple(sorted(
                        itens, key=lambda x: digest_forma(x, ordem),
                    )),
                )
            elif isinstance(valor, collections.abc.Mapping):
                itens = []
                for chave, conteudo in valor.items():
                    if isinstance(chave, str) and chave.lower() in PRIVADOS:
                        continue
                    k = (
                        chave
                        if isinstance(chave, str) and chave in CHAVES
                        else canonico(chave, ativos, memo)
                    )
                    itens.append((k, canonico(conteudo, ativos, memo)))
                ordem = {}
                forma = (
                    "dict",
                    tuple(sorted(
                        itens,
                        key=lambda x: digest_forma(x[0], ordem),
                    )),
                )
            elif isinstance(valor, (set, frozenset)):
                itens = [canonico(x, ativos, memo) for x in valor]
                ordem = {}
                forma = (
                    "set",
                    tuple(sorted(
                        itens, key=lambda x: digest_forma(x, ordem),
                    )),
                )
            elif isinstance(valor, (list, tuple)):
                forma = (
                    "list" if isinstance(valor, list) else "tuple",
                    tuple(canonico(x, ativos, memo) for x in valor),
                )
            elif dataclasses.is_dataclass(valor) and not isinstance(valor, type):
                forma = canonico({
                    f.name: getattr(valor, f.name)
                    for f in dataclasses.fields(valor)
                    if f.name.lower() not in PRIVADOS
                }, ativos, memo)
            else:
                forma = sem_forma(valor)
        except Exception:
            forma = sem_forma(valor, "erro_canonizador")
    finally:
        ativos.remove(identidade)

    memo[identidade] = (valor, forma)
    return forma


def digest_forma(forma, memo=None):
    if memo is None:
        memo = {}
    if isinstance(forma, tuple):
        chave = id(forma)
        if chave in memo:
            return memo[chave][1]
        h = hashlib.sha256(b"T%d(" % len(forma))
        for item in forma:
            h.update(digest_forma(item, memo).encode("ascii"))
            h.update(b",")
        h.update(b")")
        digest = h.hexdigest()
        memo[chave] = (forma, digest)
        return digest
    return hashlib.sha256(
        ("P" + repr(forma)).encode("utf-8")
    ).hexdigest()


def bytes_forma(forma, memo=None):
    # Comprimento JSON expandido calculado sem construir esse JSON.
    # Implementação proposta: [nao verificado].
    if memo is None:
        memo = {}
    if not isinstance(forma, tuple):
        return len(serial(forma).encode("utf-8"))
    chave = id(forma)
    if chave in memo:
        return memo[chave][1]
    tamanho = (
        2 + max(0, len(forma) - 1)
        + sum(bytes_forma(x, memo) for x in forma)
    )
    memo[chave] = (forma, tamanho)
    return tamanho


def primeiro_id(unidade):
    if type(unidade) is int:
        return unidade
    if isinstance(unidade, (list, tuple)):
        for item in unidade:
            ident = primeiro_id(item)
            if ident is not None:
                return ident
    return None


def rotulo_classe(valor):
    if valor is None:
        return "None"
    if type(valor) is bool:
        return "True" if valor else "False"
    if isinstance(valor, str):
        return valor if valor in TOKENS else "FORA_DO_VOCABULARIO"
    if isinstance(valor, enum.Enum):
        return rotulo_classe(valor.value)
    return "TIPO:" + type(valor).__name__


class Registro:
    # Não conserva eventos/linhas: somente acumuladores por faixa de IDs,
    # bitmap de IDs da faixa e contadores de classes.
    # Algoritmo proposto; validação em execução: [nao verificado].
    def __init__(self):
        self.chamadas = 0
        self.projecoes = 0
        self.excecoes = collections.Counter()
        self.hist = collections.Counter()
        self.classes = collections.Counter()
        self.metricas = collections.Counter()
        self.blocos = {}

    def contar_classes(self, valor):
        self.hist[rotulo_classe(valor)] += 1
        if not isinstance(valor, collections.abc.Mapping):
            return

        def campos(d, prefixo):
            for campo in CAMPOS_CLASSE:
                if campo in d:
                    v = d[campo]
                    if v is None or isinstance(v, (bool, str, enum.Enum)):
                        self.classes[
                            prefixo + campo + ":" + rotulo_classe(v)
                        ] += 1

        campos(valor, "")
        for instancia in valor.get("instancias") or ():
            if not isinstance(instancia, collections.abc.Mapping):
                continue
            campos(instancia, "instancia.")
            for acao in instancia.get("acoes") or ():
                if isinstance(acao, collections.abc.Mapping):
                    campos(acao, "acao.")

    def adicionar(self, ident, contexto, valor, exc=None, projecao=False):
        if projecao:
            self.projecoes += 1
        else:
            self.chamadas += 1
        if exc:
            self.excecoes[exc] += 1
        self.contar_classes(valor)

        evento = (
            canonico(contexto),
            ("forma_sha256", digest_forma(canonico(valor))),
        )
        digest = digest_forma(evento)
        numero = int(digest, 16)

        if ident is not None and (type(ident) is not int or ident < 0):
            raise FalhaDaSonda()
        faixa = None if ident is None else ident // BLOCO
        bloco = self.blocos.setdefault(faixa, {
            "n": 0, "soma": 0, "quadrados": 0, "xor": 0, "bitmap": 0,
        })
        bloco["n"] += 1
        bloco["soma"] = (bloco["soma"] + numero) & MASCARA_HASH
        bloco["quadrados"] = (
            bloco["quadrados"] + numero * numero
        ) & MASCARA_HASH
        bloco["xor"] ^= numero
        if ident is not None:
            bloco["bitmap"] |= 1 << (ident % BLOCO)

    def resultado(self):
        blocos = []
        for faixa in sorted(
            self.blocos, key=lambda x: (-1 if x is None else x),
        ):
            b = self.blocos[faixa]
            atomos = {
                "n": b["n"],
                "soma256": "%064x" % b["soma"],
                "quadrados256": "%064x" % b["quadrados"],
                "xor256": "%064x" % b["xor"],
            }
            blocos.append({
                "faixa": faixa,
                "id_de": None if faixa is None else faixa * BLOCO,
                "id_ate": (
                    None if faixa is None else (faixa + 1) * BLOCO - 1
                ),
                "ids_distintos": b["bitmap"].bit_count(),
                "ids_bitmap_hex": format(b["bitmap"], "x"),
                **atomos,
                "sha256": sha(atomos),
            })
        return {
            "universo_registros": self.chamadas + self.projecoes,
            "chamadas_ou_observacoes": self.chamadas,
            "projecoes_de_resultado": self.projecoes,
            "excecoes_por_classe": dict(sorted(self.excecoes.items())),
            "histograma": dict(sorted(self.hist.items())),
            "classes": dict(sorted(self.classes.items())),
            "metricas": dict(sorted(self.metricas.items())),
            "blocos": blocos,
        }


def registro(nome):
    return REGISTROS.setdefault(nome, Registro())


def guardar(
    

[truncado em 20000 de 82527 caracteres]

## 13/11 00:47 · sessao fd03853988f3 · BUG-CADASTRO-C5E · censo · origem=Code

As referências aos anexos contam desde a primeira linha de cada bloco, incluindo linhas em branco.

| Achado | Letra | Trecho literal .q | Trecho literal .r | Trecho triagem e motivo |
|---|---|---|---|---|
| **4/6 — duas leituras no signal; ausência no universo** | **b** | “Ache um ato que leia duas vezes ou deixe um destino ler o relogio por conta propria.” (`pv1.q:10–11`) | “Na edição, `existente.save()` pode alcançar os signals: o de vigência lê **duas vezes**; o de âncora lê uma vez.” (`pv1.r:55`) | “FORA (aval: PROIBIDO ampliar aos ~30 interiores): […] escala/signals.py:67/119” (`triagem_v1.txt:11`). **Descarte por escopo do aval.** A dupla leitura foi vista; a omissão específica de `UNIVERSO_RELOGIO` não foi denunciada nesse trecho. (`pv1.r:55`; `app/ponto/tests/test_cadastro_futuro_c5e.py:326–332`) |
| **1 — relógios adicionais no Salvar e destinos** | **b** | “se o MESMO valor desce a todas as portas e a todo efeito do ato” (`pv1.q:9–10`) | “Ele relê `localdate()` e decide os dias julgáveis com esse novo valor”; “`_copiar_folga_padrao` também lê `date.today()` no meio do Salvar.” (`pv1.r:53`; `pv1.r:56`) | “FORA […] rabo _o_chao_do_dia_mudou:464, pos_vinculo:56, escala/signals.py:67/119, _copiar_folga_padrao:163 […] vinculo.py:748 -> RELOGIO-NA-BORDA.” (`triagem_v1.txt:11–12`). **Descarte por proibição de ampliar a cura.** |
| **6 — cron ainda regenera HOJE** | **b** | “Em cada ato que chega a essas portas (view de Salvar, commands […]), onde o relogio e lido” e “Quem LE e quem ESCREVE cada valor que o diff passa a mover” (`pv1.q:8–9`; `pv1.q:14`) | “Uma leitura `date.today()`; mesmo valor para vigia, regeneração de hoje, geração e lavratura.” (`pv1.r:44`) | “DESCARTADO 3 (c5): continua descartado (Codex item 7 concorda).” (`triagem_c5d.txt:6`). O descarte remete à defesa de conservar o alcance de hoje pela juíza, registrada no item 7 de `preverif_c5d.r`; sua pergunta original não foi fornecida **[nao verificado]**. |
| **7 — DNA órfão após exclusão no snapshot** | **e** | O recorte explícito é “o `hoje` e o que ele decide” (`pv1.q:14`). A volta 3 acrescenta “O que mais esta errado na obra inteira (8dc931da9..4b34ed21a)” (`pv3.q:9`), sem nomear exclusão, `SET_NULL` ou integridade do DNA. | “o snapshot mantém vínculos antes de células, com selo exigindo `E → C`” (`pv3.r:67`). Esse trecho examina travas; o problema de DNA após exclusão não é tratado. | Descarte específico desse problema: **[nao verificado]**. Classifico **não perguntado pelo escopo demonstrável**: snapshot aparece como relógio/travas, sem pergunta específica sobre os resíduos do `delete()`. (`pv1.q:14–16`; `pv3.r:67–68`; `app/ponto/tests/test_cadastro_futuro_c5e.py:344`) |
| **8 — comprovação do D7 e reversão em logs** | **c** | “inclusivo/exclusivo em `d > hoje` x `d >= hoje`” (`pv1.q:16`) | “Já a célula ausente de vínculo inativo só nasce em `d > hoje`.” (`pv1.r:95`); “não houve acesso a banco/log nem execução da suíte com banco” (`pv3.r:73`) | Medição anterior/posterior e prova de reversão nas triagens fornecidas: **[nao verificado]**. A comprovação solicitada depende dos dados/logs ausentes. (`pv3.r:73`) | [nao verificado]

A conferência da cópia confirma os sítios citados:

- **4/6 e 1:** leituras em `app/escala/signals.py:67–68` e `:119`; `app/colaboradores/services/vinculo.py:163`, `:194`, `:740` e `:752`; `app/escala/folgas.py:280`; `app/ponto/portas/celula.py:464`. O universo da catraca omite `escala/signals.py`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:326–332`)
- **6:** `_regenerar_hoje_existentes` chama a porta para hoje e é executado pelo command. (`app/ponto/management/commands/gerar_celulas.py:303`, `:323–325`, `:347`)
- **7:** o snapshot executa `remover.delete()` e depois percorre os vínculos do snapshot; a geradora usa `SET_NULL`, e o DNA é JSON separado. (`app/colaboradores/services/vinculo.py:1029`, `:1034–1035`; `app/escala/models.py:1693`, `:1697`). O vigia incrementa o contador de divergência. (`app/ponto/services/vigia_cadastro_futuro.py:60–61`)
- **8:** a seleção D7 existe, com guarda que exclui o inativo em `d <= hoje`. Isso sustenta a leitura estática; resultado em produção e reversão permanecem **[nao verificado]**. (`app/ponto/portas/celula.py:883`, `:908–910`)

Por volta, **pv1** pediu atos, chamadores e leitores/escritores; **pv2** concentrou-se no delta e proibiu ampliar aos interiores; **pv3** declarou delta apenas de teste, acrescentando revisão da obra inteira com o mesmo veto. (`pv1.q:8–16`; `pv2.q:4–8`; `pv3.q:2`, `:9`). A existência exata nos hashes anteriores e o conteúdo dos avais/diffs apenas mencionados nos anexos permanecem **[nao verificado]**. [nao verificado]

**Contagem: a=0 | b=3 | c=1 | d=0 | e=1.**

_4501 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
