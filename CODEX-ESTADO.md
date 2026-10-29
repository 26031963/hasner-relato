# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 13:43** · publicado em **29/10/2026 13:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 13:43 · sessao 1ee182181f58

**Instalado: `codex-cli 0.165.0`.** Conferi `codex --version`, `codex --help`, `codex exec --help`, `codex features list` e `codex login status`. Login atual: **ChatGPT**. Não li código do Hasner.

### 1. Subagentes nativos

**Sim.** A instalação informa `multi_agent stable true`. A documentação descreve agentes paralelos com contextos separados, resultados devolvidos ao principal e consolidação na resposta final. Os filhos herdam sandbox e aprovação do principal. [Documentação de subagentes](https://learn.chatgpt.com/docs/agent-configuration/subagents).

Acionamento por instrução ao agente, por exemplo:

```bash
codex --enable multi_agent \
  --sandbox read-only --ask-for-approval never \
  -C "$HOME/codex_ws" \
  'Divida a investigação em tarefas independentes. Use até três subagentes explorer em paralelo, espere os resultados e consolide.'
```

Para configurar explicitamente o teto:

```bash
codex --enable multi_agent \
  -c agents.max_threads=3 \
  --sandbox read-only --ask-for-approval never \
  -C "$HOME/codex_ws"
```

`agents.max_threads` é documentado como alias de `agents.max_concurrent_threads_per_session`, excluindo o principal. **Padrão efetivo e aplicação desse teto na 0.165.0: [nao verificado]**; não consegui inicializar uma sessão de teste. [Referência de configuração](https://learn.chatgpt.com/docs/config-file/config-reference). [nao verificado]

`codex agents` abre o navegador de sessões; não é um comando shell para criar um filho — confirmado pelo help.

### 2. `exec`: pergunta de arquivo, resposta em arquivo

Sintaxe conferida no help instalado:

```bash
codex --ask-for-approval never exec \
  --sandbox read-only \
  --skip-git-repo-check \
  -C "$HOME/codex_ws" \
  --output-last-message /tmp/codex-resposta.txt \
  - < /tmp/codex-pergunta.txt
```

**A posição importa nesta versão:** `codex exec -a never` foi rejeitado. A opção funciona no parser global, **antes de `exec`**.

Flags confirmadas:

| Flag | Efeito indicado pelo help |
|---|---|
| `-` | Lê o prompt de stdin. |
| `-C` | Define diretório de trabalho. |
| `--skip-git-repo-check` | Permite diretório sem `.git`, como cópia por archive. |
| `-o`, `--output-last-message` | Grava a última mensagem. |
| `--json` | Emite **eventos JSONL em stdout**. |
| `--output-schema` | Define JSON Schema da resposta final. |
| `--ephemeral` | Evita persistir arquivos da sessão. |

Eventos JSONL separados da resposta final:

```bash
codex --ask-for-approval never exec \
  --sandbox read-only --skip-git-repo-check \
  -C "$HOME/codex_ws" \
  --json \
  -o /tmp/codex-final.txt \
  - < /tmp/codex-pergunta.txt \
  > /tmp/codex-eventos.jsonl
```

Para **resposta final JSON**, acrescente:

```bash
--output-schema /tmp/schema.json -o /tmp/codex-final.json
```

`--json` sozinho não transforma a resposta final em JSON estruturado. Origem: help instalado e [documentação não interativa](https://learn.chatgpt.com/docs/non-interactive-mode). Execução completa com arquivos: **[nao verificado]**.

### 3. Vários `codex exec` simultâneos

Exemplo shell, com saídas distintas:

```bash
codex --ask-for-approval never exec \
  -s read-only --skip-git-repo-check -C "$HOME/codex_ws" \
  -o /tmp/resposta-a.txt - < /tmp/pergunta-a.txt &
pid_a=$!

codex --ask-for-approval never exec \
  -s read-only --skip-git-repo-check -C "$HOME/codex_ws" \
  -o /tmp/resposta-b.txt - < /tmp/pergunta-b.txt &
pid_b=$!

wait "$pid_a"; rc_a=$?
wait "$pid_b"; rc_b=$?
printf 'a=%s b=%s\n' "$rc_a" "$rc_b"
```

**Este teste paralelo não foi executado.** [nao verificado]

- Conflito de sessões/cache na 0.165.0: **[nao verificado]**.
- Limite de processos concorrentes: **[nao verificado]**; não aparece nos helps consultados. Não confundir com teto de subagentes.
- Limites de uso/taxa desta conta ChatGPT e comportamento sob concorrência: **[nao verificado]**.
- O help oferece `--no-daemon`, antes de `exec`, para não usar o servidor compartilhado. Isso não comprova isolamento de todos os caches.

### 4. Outro processo pode chamar e esperar?

**Sim, esse é o uso documentado de `codex exec`: scripts/CI, sem TUI.** O comando em primeiro plano só devolve o controle ao terminar. [Origem: documentação oficial](https://learn.chatgpt.com/docs/non-interactive-mode).

```bash
codex --ask-for-approval never exec \
  --sandbox read-only --skip-git-repo-check \
  -C "$HOME/codex_ws" \
  -o /tmp/resposta.txt - < /tmp/pergunta.txt
rc=$?

if [ "$rc" -eq 0 ]; then
  cat /tmp/resposta.txt
else
  printf 'codex falhou: %s\n' "$rc" >&2
fi
```

- **TTY:** não exigido pelo modo documentado.
- **Login:** `codex login status` confirmou autenticação nesta sessão. Disponibilidade para outro usuário/ambiente: **[nao verificado]**.
- **Confirmações de execução:** `never` manda devolver falhas ao agente, sem pedir aprovação — help instalado.
- Outros bloqueios de inicialização/configuração: não estão eliminados por `never`.

**Teste realmente rodado**, sem TTY e sem ler arquivos: [nao verificado]

```bash
codex --ask-for-approval never --no-daemon exec \
  --ephemeral --skip-git-repo-check \
  --sandbox read-only -C /tmp \
  -o /dev/null \
  'Nao use ferramentas, nao leia arquivos. Responda apenas CLI_OK.'
```

Resultado:

```text
Error: failed to initialize in-process app-server client:
Read-only file system (os error 30)
```

Portanto, confirmei sintaxe e tentativa de inicialização; **não confirmei uma resposta completa do modelo**. O sandbox `read-only` do agente não dispensa armazenamento gravável para o runtime do CLI e para o arquivo de saída.

## 29/10 12:28 · sessao 1ee182181f58

HEAD lido: `5a7ea7a6afda8d4d0accfc2dae9b2973df6e2dad`, por `git show`. Sem escrita, testes ou banco. Os dados dos IDs são os fornecidos no laudo; seus registros e trilhas: **nao li**.

## T16 — Data da pergunta × data da célula

### (a) Onde nasce `data_evento`

| Caminho | Origem da data |
|---|---|
| Fábrica por célula | Passa **`cel.data`** e coloca esse dia no contexto do motivo (`app/chamados/services/fabrica_da_celula.py:123`, `:108`). |
| Gerador de perguntas | Usa a data do contexto e entrega ao gate como `data_evento` (`app/chamados/services/disputa_emissao.py:343`, `:347`, `:396`). |
| Contexto sem data | `_data_furo_da_grade` toma `entrada_sem_saida`/`ultima_batida`, converte para **dia civil local** e verifica a célula desse dia; não pergunta `Turno.data_turno` (`app/chamados/services/disputa_emissao.py:57`, `:61`, `:66`, `:68`). |
| Último fallback de anomalia | Usa o dia civil local de `ultima_batida` (`app/chamados/services/disputa_emissao.py:350`, `:364`). |
| Marcos nomeados do chamado | Usa o dia declarado no contexto e consulta a ata desse dia (`app/chamados/services/disputa_emissao.py:973`, `:978`). |
| Agrupamento por batida-origem | Começa pelo dia civil da entrada; depois resolve a data do evento pelo motivo (`app/chamados/services/disputa_emissao.py:1002`, `:1003`, `:1006`). |
| Motivo `orfao_14h` | Usa a **data da saída esperada**, que pode ser D+1 num noturno (`app/escala/servico_jornada.py:482`, `:487`, `:490`). |
| Sem batida-origem | Usa a data do contexto do chamado (`app/chamados/services/disputa_emissao.py:1031`). |
| Conversa sem data | O gate atribui **hoje** (`app/chamados/models.py:2054`, `:2057`). |
| Associação da pergunta à célula | Busca a célula pela **própria `data_evento`**, não copia a célula do chamado (`app/chamados/models.py:2060`). |

A costura só preenche data quando ela está vazia; não corrige data já existente (`app/chamados/services/fio.py:788`, `:790`). O juiz admite D/D+1 quando reconhece turno noturno **ou quando não consegue determinar se cruza a meia-noite** (`app/chamados/juizes.py:1712`, `:1721`, `:1730`, `:1732`).

**Limite para o caso:** com entrada de28/10 e DNA realmente diurno06–18, `resolver_data_evento` não produz29/10 sozinho: o fim encadeado permanece em28/10 (`app/escala/utils.py:925`, `:927`, `:930`). Para explicar a pergunta de29/10 é necessário outro contexto/ramo ou outro DNA lido naquele ato. Isso é **hipótese**; não está demonstrado apenas pelo casamento de18:01 com S18.

### (b) Qual vínculo/DNA casa18:01

| Etapa | Regra encontrada |
|---|---|
| Escolha do vínculo | A grade carrega vínculos/células e chama `vinculo_do_dia` (`app/escala/utils.py:1240`, `:1252`, `:1254`). |
| Geradora declarada | Vale apenas dentro da vigência. Se estiver fora, retorna `None`, sem trocar silenciosamente por outro vínculo (`app/escala/alimentacao.py:237`, `:247`, `:250`). Sem geradora, escolhe vínculo que cobre o dia (`:253`, `:254`). |
| Marcos usados | Primeiro DNA da célula do dia; na ausência, template (`app/escala/utils.py:104`, `:1547`). |
| Matcher | Recebe marcos prontos; não escolhe vínculo nem data (`app/escala/utils.py:551`). |
| Uso do papel do turno | Exige exatamente um turno fechado; neste chamador, também intervalo declarado (`app/escala/utils.py:1533`, `:1534`, `:1641`, `:1642`). |
| Caminho restante | DP por distância, sem exigir que tipo gravado coincida com tipo do marco (`app/escala/utils.py:595`, `:599`); conserva `tipo_real` separadamente (`:627`). |

Assim, **uma entrada isolada18:01 pode ocupar S18** diante de marcos06/18: o turno aberto não fornece papel, e a DP escolhe o marco próximo (`app/escala/utils.py:1534`, `:595`, `:599`). Isso demonstra atribuição posicional, não demonstra erro de data civil.

Pelas vigências informadas, #1482 encerrado em13/10 não cobre28/10; se a célula ainda o nomeia como geradora, o juiz retorna `None` (`app/escala/alimentacao.py:250`). Geradora e DNA efetivos da célula28/10: **nao li**.

Há uma alimentação diferente no datador da saída: ele busca vínculo **ativo e sem fim**, em vez da composição histórica do builder, e depois lê os marcos do dia (`app/escala/utils.py:920`, `:922`, `:927`). Que essa diferença tenha causado p#51310: **hipótese**.

### (c) Resolução sem resposta humana

| Via / escritor | Testemunha consultada |
|---|---|
| `sem_turno_aberto` (`app/chamados/reconciliador.py:402`, `:406`) | Exige ausência de resposta, ausência de turno aberto e pergunta já não viva (`:365`, `:372`, `:398`). Não basta faltar turno aberto. |
| `celula_absolveu` (`app/chamados/reconciliador.py:548`, `:550`) | Célula não acusadora, sem pendências de teto, pergunta não viva (`:525`, `:529`, `:538`, `:544`). |
| `fantasma` (`app/chamados/models.py:2170`, `:2171`) | Sem resposta, lâmpada do próprio marco acesa e dia encerrado; consumidor grava pela porta (`app/chamados/reconciliador.py:878`, `:880`, `:884`, `:886`, `:940`). |
| `juiz_aprovou` (`app/chamados/services/materializacao.py:1337`, `:1341`) | Usa `_pergunta_materializada_de_fato`; corpo integral desse juiz neste HEAD: **nao li**. |
| `completude_turno` | `via_pelo_fato` consulta `TurnoMaterializado.saida` pela `data_evento` (`app/chamados/juizes.py:1203`, `:1207`, `:1209`). |
| `ausencia_dia` | O mesmo juiz usa ausência aprovada no dia da pergunta (`app/chamados/juizes.py:1199`, `:1201`). |

Quando consulta a lâmpada, o juiz lê **`p.celula`**, seu DNA e ATA — não necessariamente a célula do chamado (`app/chamados/juizes.py:842`, `:845`, `:849`, `:866`). Via concreta de p#51310: **nao li**.

**Não são uma única classe comprovada de “dia pelo dia civil”.** Há mistura de semânticas de data em(a), escolha de vínculo/DNA e atribuição posicional em(b), e resolução por testemunha em(c). A cadeia causal específica entre as três continua **hipótese**, pelas diferenças demonstradas acima.

## T17 — Chamado em cobrança sem pergunta viva

**`COBRANCA` depende somente de `status_local` ser `aberto` ou `em_analise`; não exige fio vivo** (`app/chamados/catalogo/motor.py:29`). Fio morto não provoca transição automática: alguém precisa chamar uma porta aceita pelo motor e pela premissa (`app/chamados/models.py:440`, `:467`, `:470`).

| Caminho | O que pode impedir fechamento/reparo |
|---|---|
| `reconciliar()` | Para furo, exige premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:495`, `:498`). **Hipótese:** célula ainda furada, sem essas exceções, conserva cobrança mesmo sem fio (`:522`, `:525`). |
| Reconciliador geral | Só retrata quando encontra premissa conhecida e morta (`app/chamados/reconciliador.py:1203`, `:1208`, `:1209`). `_retratar` ainda pode recusar por irmã respondida sem consumo ou pelo gate final (`:435`, `:456`). |
| `consumir_eventos_celula` | Executa retratação de absolvidos e encerramento de órfãs pela lâmpada (`app/chamados/reconciliador.py:1145`, `:1148`). Sem falha/adiamento, marca processado mesmo sem alteração (`:1165`). **Processado não significa fio reparado.** |
| Retratar absolvidos | Exige célula com veredito **não acusador** e sem pendência de teto (`app/chamados/reconciliador.py:819`, `:823`, `:825`). Célula acusadora fica fora. |
| Encerrar órfãs pela lâmpada | Seleciona perguntas não materializadas/não validadas, em disputa aberta (`app/chamados/reconciliador.py:836`, `:844`); exige pergunta **viva**, sem texto e marco aceso (`:878`, `:880`, `:882`). Não ressuscita pergunta morta. |
| `ressuscitar_perguntas` | Só alcança resolvedoras associadas, materializadas, sem texto, cujo marco não esteja comprovadamente aceso (`app/chamados/models.py:1112`, `:1119`). Pergunta ausente, validada sem materialização, com texto ou ligada a outro chamado fica fora. Limpeza em `:1162`. |
| Pedido de ressuscitação pelo cartório | Pula chamado sem perguntas, com fio vivo ou com qualquer resposta textual (`app/ponto/services/cartorio.py:1261`, `:1263`). Duplicatas tentam fechamento; demais seguem para ressuscitação (`:1265`, `:1271`). Impressão igual impede executar esse corpo (`:1092`). |
| `reabrir()` | Só há transição de resolvido/fechado; não é reparo para chamado já aberto (`app/chamados/catalogo/motor.py:113`). Quando consegue reabrir, pede ressuscitação (`app/chamados/models.py:1098`). |
| Fábrica | Exige célula elegível e motivo vencido (`app/chamados/services/fabrica_da_celula.py:73`, `:80`, `:81`). Dedup considera pergunta morta não duplicata como existente (`:93`, `:95`); o gate reutiliza o fato sem limpar carimbos (`app/chamados/models.py:2061`, `:2084`). |
| Gatilho da fábrica | Só alcança células efetivamente julgadas, não a mera ausência de fio (`app/ponto/services/cartorio.py:769`, `:770`). |
| Evento de beco | Exige pai já resolvido; pai aberto não entra (`app/chamados/services/becos.py:31`). |

Nascimento e mudança de veredito/ata geram eventos, mas somente eventos pendentes alimentam o consumidor (`app/chamados/models.py:736`, `:739`; `app/ponto/portas/celula.py:623`, `:642`; `app/chamados/reconciliador.py:1128`). Isso não comprova reparo do estoque anterior nem superação das guardas acima.

**O261 conta #22914? “Aberto sem pergunta viva” não basta:**

- Precisa módulo de furo e célula associada (`app/chamados/services/vigia_fio.py:184`, `:199`).
- A célula precisa ser acusadora (`:227`).
- O vigia reúne perguntas da **mesma célula**, não só resolvedoras daquele chamado. Outra pergunta viva da célula pode excluí-lo (`:242`, `:244`, `:258`).
- Exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`:261`, `:266`, `:273`, `:279`).
- Passando tudo, soma uma célula em `fio_morto` e, com cobrança, em `fio_morto_cobrando` (`:286`, `:288`).

**Por que nenhum caminho alcançou especificamente #22914: não provado.** Módulo, célula, data, veredito, carimbos das perguntas e estado dos eventos desse ID: **nao li**. Escolher uma das condições acima como causa seria **hipótese**.

## T18 — Retrato × `o_que_segura`

| Fonte | O que efetivamente lê |
|---|---|
| Ficha/drawer | Busca `RetratoFolha` da empresa/competência e localiza a linha do colaborador (`app/colaboradores/views.py:980`, `:982`). O localizador lê **`RetratoFolha.linhas`** (`app/folha/retrato_folha.py:110`, `:935`). |
| Pendências exibidas | Template mostra `retrato_fazer` e **`retrato_linha.datas`**, já gravadas (`app/templates/folha/_retrato_o_que_fazer.html:33`, `:44`, `:46`). |
| Produção da foto | `montar_linhas` chama `fora_da_folha(..., com_itens=True)` (`app/folha/retrato_folha.py:1157`), que usa `classificar_export` e `motivos_retencao_celula` (`app/folha/fora_da_folha.py:120`, `:93`, `:139`). Grava status, motivo, dias e datas (`app/folha/retrato_folha.py:1180`, `:1183`). |
| `o_que_segura` | Consulta **agora** `classificar_export` e a mesma autoridade de dias retidos; devolve o status em `entra` (`app/folha/fora_da_folha.py:191`, `:193`, `:212`). |

**Não encontrei duas regras de retenção nesse caminho.** As duas leituras descendem das mesmas autoridades; a ficha lê a resposta gravada e `o_que_segura` pede resposta atual (`app/folha/retrato_folha.py:1157`; `app/folha/fora_da_folha.py:191`, `:193`).

Há duas distinções relevantes:

1. **“Folga” pode ser rótulo de causa**, não classificação atual de cada data. `tipo='folga'` pode vir de causas como `escala_sem_folga_definida` ou `dias_trocados`, lidas de `FechamentoMensal.causa_espelho` (`app/folha/retrato_folha.py:233`, `:238`, `:374`, `:1115`).
2. O juiz atual de retenção usa veredito/protestos da célula; não possui nesse ramo veto independente por `trabalha=False`. Portanto uma folga ainda acusadora pode reter; uma célula absolvida deixa de reter por esse critério (`app/folha/export.py:1188`, `:1215`, `:1227`, `:1235`).

**O frescor pode não perceber mudança da célula:** compara somente `Max(FechamentoMensal.atualizado_em)` com `RetratoFolha.lavrado_em` (`app/folha/retrato_folha.py:1298`, `:1306`). Veredito/ata são gravados pela porta de célula separadamente (`app/ponto/portas/celula.py:691`, `:697`).

**Hipótese de defasagem:** célula deixa de reter depois da lavra, retrato não é relavrado e nenhum fechamento recebe timestamp posterior. A ficha conserva `linhas.datas`; `o_que_segura` responde `entra`; o indicador ainda pode dizer atualizado (`app/folha/retrato_folha.py:1183`, `:1303`; `app/folha/fora_da_folha.py:212`).

**Conclusão:** o código sustenta **projeção velha como hipótese**, não uma derivação paralela de folga na ficha. Para confirmar o ocorrido, a linha gravada, `lavrado_em`, timestamps dos fechamentos e histórico das células: **nao li**.

## 29/10 12:27 · sessao bfa1cd7083b2

T16 — fonte fixada em `5a7ea7a6afda8d4d0accfc2dae9b2973df6e2dad`, lida por `git show`. Os vínculos e IDs do caso são dados do laudo fornecido; registros e trilha de `#35582`/`p#51310`: **nao li**, sem banco.

### (a) De onde nasce a data da pergunta

| Caminho | Origem da data |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:123` → `:108` | **Dia da célula**: passa `cel.data` e grava esse dia em `contexto[motivo]['data']`. |
| `app/chamados/services/disputa_emissao.py:343`, `:347`, `:396` | Gerador usa a data do contexto; entrega-a como `data_evento` ao gate. |
| `app/chamados/services/disputa_emissao.py:322` → `_data_furo_da_grade:42` | Quando falta texto/data, toma timestamp de `entrada_sem_saida`/`ultima_batida` (`:57`), converte para **data civil local** (`:61`) e só verifica se a célula desse dia é trabalho (`:66`, `:68`). Não pergunta `Turno.data_turno`. |
| `app/chamados/services/disputa_emissao.py:350`, `:364` | Último fallback toma a **data civil local de `ultima_batida`**. |
| `app/chamados/services/disputa_emissao.py:973`, `:978` | Caminho de marcos nomeados usa o **dia declarado no contexto do chamado**, depois consulta a ata daquele dia. |
| `app/chamados/services/disputa_emissao.py:1002`, `:1006` | Caminho agrupado por batida-origem começa pela data civil da entrada para classificar; depois `resolver_data_evento` pode convertê-la em **data da saída esperada**. |
| `app/escala/servico_jornada.py:482`, `:487`, `:490` | Para `orfao_14h`, data da pergunta é a **data da saída prevista**, podendo ser D+1 para turno noturno. |
| `app/chamados/services/disputa_emissao.py:1031` | Sem batida-origem, usa data do contexto do chamado. |
| `app/chamados/services/disputa_emissao.py:259`, `:272` | Perguntas de `entradas_pendentes` recebem o dia já existente na lista do contexto. |
| `app/chamados/models.py:2054`, `:2057` | Conversa sem `data_evento` recebe **hoje**, sem identidade de fato. |
| `app/chamados/models.py:2060` | O gate associa `PerguntaDisputa.celula` buscando **pela própria `data_evento`**, não copiando a célula do chamado. |

A costura posterior só preenche data **se a pergunta ainda não tem data**; não corrige uma data já preenchida (`app/chamados/services/fio.py:788`, `:790`). Compara a resolvedora antes de salvar (`:792`, `:799`).

O juiz da costura lê o dia do **contexto do chamado** (`app/chamados/juizes.py:1712`), permite dia igual (`:1721`) e permite diferença de um dia quando o turno cruza a meia-noite **ou quando não consegue determinar** (`:1730`, `:1732`). Portanto, diferença D/D+1 não é automaticamente erro; a unidade declarada é a jornada.

**Limite causal do caso:** com DNA diurno `06–18` em28/10, `resolver_data_evento` não gera29/10 sozinho: `data_saida_esperada` usa a data civil da entrada e encadeia o fim de06–18 no próprio dia (`app/escala/utils.py:925`, `:927`, `:930`). Para explicar29/10, é necessário identificar outro contexto/ramo ou DNA noturno lido naquele ato — **hipótese**, não demonstrada pelos IDs.

### (b) Qual vínculo/DNA casa a batida18:01

- A grade carrega todos os vínculos e células da janela (`app/escala/utils.py:1240`, `:1252`); escolhe o vínculo por `vinculo_do_dia` (`:1254`, `:1255`).
- A geradora da célula vence apenas dentro da vigência; fora dela o juiz devolve `None`, sem escolher outro silenciosamente (`app/escala/alimentacao.py:237`, `:247`, `:250`, `:251`). Sem geradora, seleciona dentre os vínculos que cobrem o dia (`:253`, `:254`).
- Os marcos vêm primeiro do **DNA da célula daquele dia**, e só na ausência dele do template (`app/escala/utils.py:84`, `:104`, `:1547`); a cadeia absoluta nasce em `:1559`.
- `_match_marcos` **não escolhe vínculo nem dia**: recebe `abs_marcos` prontos (`app/escala/utils.py:551`).
- O papel do turno só entra quando há exatamente um turno fechado (`:1533`, `:1534`) e, nesse chamador, intervalo declarado (`:1641`, `:1642`). Fora disso, o builder mantém a DP com `derivar=False`.
- Na DP, candidatos são escolhidos pelo envelope e alinhados por distância, sem restringir casamento pelo tipo gravado (`app/escala/utils.py:595`, `:599`). A célula conserva `tipo_real` da batida separadamente (`:627`).

**Consequência estática:** diante dos marcos diurnos06/18, uma entrada isolada18:01 pode sentar no marco **S18**, por proximidade, sem que seu botão `E` imponha o marco de entrada (`app/escala/utils.py:595`, `:599`, `:627`). Isso é **atribuição posicional de papel**, não demonstração de erro de data civil.

Para28/10, o vínculo noturno encerrado em13/10 não cobre a data informada; se a célula28/10 aponta para ele, a escolha retorna `None` (`app/escala/alimentacao.py:250`). Se aponta para#1483 válido e seu DNA é06–18, esse DNA é usado. Geradora/DNA reais dessas células: **nao li**.

Há ainda uma segunda escolha diferente no datador da saída: `data_saida_esperada` busca vínculo **ativo e sem fim**, não o vínculo histórico pela mesma composição do builder (`app/escala/utils.py:920`, `:922`), embora depois chame `ec.marcos_do_dia(d)` (`:927`). Isso é uma diferença de alimentação demonstrada; que tenha causado `p#51310`: **hipótese**.

### (c) Vias que resolvem sem resposta humana

| Via / escritor | Passa pela célula? |
|---|---|
| `sem_turno_aberto`: `app/chamados/reconciliador.py:402`, `:406` | **Sim indiretamente pelo juiz**: exige ausência de resposta (`:365`), ausência de turno aberto (`:372`) e `pergunta_viva(p)==False` (`:398`, `:399`). Não basta ausência de turno. |
| `celula_absolveu`: `app/chamados/reconciliador.py:548`, `:550` | **Sim diretamente**: célula julgada não acusadora (`:525`, `:529`), sem marcos pendentes (`:538`) e pergunta não viva (`:544`). |
| `fantasma`: `app/chamados/models.py:2170`, `:2171` | **Sim, pelo evento da lâmpada**: `veredito_orfa` exige sem resposta, marco da própria pergunta aceso e dia encerrado (`app/chamados/reconciliador.py:878`, `:880`, `:884`, `:886`); consumidor chama a porta em `:940`. |
| `juiz_aprovou`: `app/chamados/services/materializacao.py:1337`, `:1339`, `:1341` | Pergunta ao juiz `_pergunta_materializada_de_fato`; o corpo completo desse juiz neste HEAD: **nao li integralmente**. Não é prova de resposta humana. |
| `completude_turno` escolhida por `via_pelo_fato` (`app/chamados/juizes.py:1203`, `:1207`, `:1209`) | A testemunha específica é `TurnoMaterializado.saida`, consultado pela **data_evento**; não é consulta direta da lâmpada nessa escolha. |
| `ausencia_dia` escolhida pelo mesmo juiz (`app/chamados/juizes.py:1199`, `:1201`) | A testemunha é ausência aprovada no dia da pergunta. |

A lâmpada consultada é a de **`p.celula`**, usando motivo, DNA e ATA dessa célula (`app/chamados/juizes.py:842`, `:845`, `:849`, `:866`). Não é necessariamente a célula associada ao chamado.

A via/materialização concreta de `p#51310`: **nao li**. Não é possível escolher entre essas vias apenas pela informação “sem resposta humana”.

**Classificação:** (a), (b) e (c) **não são uma única classe comprovada de “dia pelo dia civil”**. Há em(a) mistura explícita entre dia da célula, contexto, data civil e data de saída; em(b), seleção de vínculo/DNA seguida de atribuição posicional; em(c), carimbo por testemunha/juiz, podendo ler a célula da própria pergunta. A ligação causal entre as três etapas do caso permanece **hipótese** nas condições e linhas acima.

## 29/10 12:26 · sessao 0364591182dc

T18 — fonte `5a7ea7a6afda8d4d0accfc2dae9b2973df6e2dad`, por `git show`; sem testes/banco.

**As duas fontes de retenção são a mesma autoridade, em instantes diferentes. Não encontrei segunda regra de folga nesse caminho.**

| Leitor | Fonte e campos |
|---|---|
| Ficha/drawer | Busca `RetratoFolha` por empresa/competência e chama `linha_do_colab` (`app/colaboradores/views.py:980`, `:982`). O localizador lê `RetratoFolha.linhas`, sem rejulgar (`app/folha/retrato_folha.py:110`, `:935`). |
| Pendências mostradas | O template mostra `retrato_fazer` e **`retrato_linha.datas`** (`app/templates/folha/_retrato_o_que_fazer.html:33`, `:44`, `:46`). A linha guarda `status`, `motivo`, `dias`, `datas`, `dono`, `tipo`, `saldo` (`app/folha/retrato_folha.py:1177`, `:1180`–`:1188`). |
| Produção do retrato | `montar_linhas` chama `fora_da_folha(..., com_itens=True)` (`app/folha/retrato_folha.py:1157`), que chama `classificar_export` (`app/folha/fora_da_folha.py:120`) e `motivos_retencao_celula` para obter os dias (`:93`, `:139`, `:168`). O comando grava a foto com `lavrado_em` (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`–`:76`). |
| `o_que_segura` | Chama **agora** `classificar_export` para o status e a mesma `_dias_que_retem` para os dias (`app/folha/fora_da_folha.py:191`, `:193`). Retorna o status em `entra`, não uma regra própria (`:212`). |

**Regra de retenção de dias:** o juiz lê células atuais da janela (`app/folha/export.py:1188`), retém vereditos acusadores (`:1215`) salvo protestos integralmente não cobradores ou exceção de pausa (`:1227`, `:1235`). Não há nesse ramo exclusão independente por `c.trabalha=False`: uma célula de folga ainda acusadora pode reter; uma célula absolvida deixa de entrar por esse critério (`:1215`–`:1238`). Essa é a regra usada tanto na lavra do retrato quanto em `o_que_segura` (`fora_da_folha.py:93`, `:139`, `:193`).

**“Folga” na ficha pode ser classificação da causa, não julgamento de cada data:** `tipo='folga'` vem de `FechamentoMensal.causa_espelho`, por causas `escala_sem_folga_definida` ou `dias_trocados` (`app/folha/retrato_folha.py:233`, `:238`, `:374`, `:382`, `:1115`). As datas mostradas são a lista anteriormente gravada, não datas que o drawer voltou a classificar como folga (`app/templates/folha/_retrato_o_que_fazer.html:43`–`:46`).

**O frescor tem uma lacuna demonstrável:** compara somente `Max(FechamentoMensal.atualizado_em)` com `RetratoFolha.lavrado_em` (`app/folha/retrato_folha.py:1298`–`:1306`). Não compara `CelulaDia.veredito_em`, ATA ou protestos. A porta de célula grava esses campos separadamente (`app/ponto/portas/celula.py:691`–`:697`).

**Hipótese de defasagem:** depois da lavra, a célula deixa de acusar, mas o retrato não é relavrado; `o_que_segura` lê o estado atual e responde `entra`, enquanto a ficha continua mostrando `linhas.datas` antigas. Se nenhum fechamento receber timestamp posterior, o próprio indicador ainda pode dizer “atualizado” (`app/folha/retrato_folha.py:1303`–`:1306`).

**Conclusão:** há suporte no código para **projeção velha**, não para duas derivações independentes de retenção. Para afirmar que foi exatamente o ocorrido no laudo, faltam a linha efetivamente gravada, `lavrado_em`, timestamps dos fechamentos e histórico das células: **nao li**, pois exigiriam os dados. Também não está provado se “folga” mencionado é o `tipo` da causa ou a situação atual das datas exibidas.

## 29/10 12:26 · sessao 1b017e44d7fa

T17 — fonte `5a7ea7a6afda8d4d0accfc2dae9b2973df6e2dad`, somente `git show`; sem testes/banco. Dados de #22914 além do laudo (“aberto, sem pergunta viva”): **nao li**.

**O que o mantém em cobrança:** `COBRANCA` é exclusivamente `status_local in ('aberto','em_analise')`; não exige pergunta viva (`app/chamados/catalogo/motor.py:29`). A transição `RECONCILIAR` existe para esses estados, mas não acontece automaticamente por fio morto (`:111`); precisa de chamada que passe pelo gate (`app/chamados/models.py:440`, `:467`, `:470`).

| Caminho | O que faz / condição que pode impedir alcançar o caso |
|---|---|
| **`ch.reconciliar()`** (`app/chamados/models.py:434`) | Não pergunta “não há pergunta viva, então fecha”. Para furo exige premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`:495`, `:498`); caso contrário retorna False e registra “dia furado para folha” (`:522`, `:525`). A premissa lê justificativa aprovada, decisão admin, lâmpadas acesas/inexistentes e furo da célula (`:937`, `:951`, `:953`, `:959`, `:968`, `:970`). Logo **hipótese**: célula ainda furada + sem irmão/agrupamento conserva cobrança mesmo sem fio. |
| **Reconciliador geral** (`app/chamados/reconciliador.py:1176`) | Percorre chamados vivos (`:1203`); só chama retratação se `_veredito_premissa` existir e não estiver vivo (`:1208`, `:1209`). Classes fora do catálogo ou premissa ainda viva ficam (`:247`, `:257`, `:258`). `_retratar` ainda recusa se há irmã respondida sem consumo (`:435`, `:436`) ou se `reconciliar` recusa (`:456`). |
| **Consumidor de `EventoCelula`** (`app/chamados/reconciliador.py:1102`) | Para cada colaborador chama **duas operações**, não reparo geral de fio: retratar chamados absolvidos (`:1145`) e encerrar órfãs pela lâmpada (`:1148`). Sem falha/adiamento marca evento processado (`:1165`), mesmo que nenhuma dessas operações altere chamado/pergunta. Portanto “evento consumido” não prova “fio reparado”. |
| **Retratar chamado absolvido** (`app/chamados/reconciliador.py:757`) | Só módulos de turno vivos (`:792`, `:794`); exige dia/célula, veredito existente **não acusador**, e nenhuma pendência de teto (`:819`, `:823`, `:825`). Célula acusadora não entra. Depois `_retratar` pode recusar pelas guardas citadas (`:800`; `:435`, `:456`). |
| **Encerrar órfã pela lâmpada** (`app/chamados/reconciliador.py:908`) | Opera perguntas **ainda não materializadas, não validadas, em disputa aberta**, somente `orfao_14h`/`saida_sem_entrada` (`:836`, `:844`–`:847`). Exige sem texto, próprio marco aceso, pergunta **viva** e teto passado (`:878`, `:880`, `:882`, `:884`, `:886`). Se o fio de #22914 já está morto, esse consumidor **não ressuscita** a pergunta; faz o contrário e só dentro desse universo. |
| **`ressuscitar_perguntas()`** (`app/chamados/models.py:1122`) | Só seleciona perguntas associadas por `perguntas_que_resolvem`, com `materializada_em != None`, sem texto, cujo próprio marco não está comprovadamente aceso (`:1112`–`:1119`). Validada sem materialização, pergunta com texto, pergunta ausente ou ligada a outro chamado não entram. A porta limpa os carimbos em `:1162` e reabre disputas em `:1188`. |
| **Ressuscitação pedida pelo cartório** (`app/ponto/services/cartorio.py:1257`) | Só chamado ainda vivo; pula se não tem perguntas, se já há fio vivo ou se qualquer pergunta tem texto (`:1258`, `:1261`, `:1263`). Se todas são duplicatas, tenta fechar chamado (`:1265`, `:1266`); senão pede ressuscitação (`:1271`), sujeita ao filtro acima. Pulo por impressão igual impede executar esse corpo (`:1092`). |
| **`reabrir()` do chamado** (`app/chamados/models.py:1087`) | Reabertura bem-sucedida chama `ressuscitar_perguntas` (`:1098`), mas `REABRIR` só tem transição de `resolvido/fechado`, não de `aberto/em_analise` (`app/chamados/catalogo/motor.py:113`). Portanto não é reparo de chamado que já está aberto. |
| **Fábrica da célula** (`app/chamados/services/fabrica_da_celula.py:68`) | Exige célula de trabalho, colaborador ativo, data não futura, motivo vencido e veredito compatível (`:73`, `:75`, `:80`, `:81`). Dedup exclui apenas duplicatas: **pergunta morta de outra via ainda satisfaz existência** (`:93`, `:95`). O gate de identidade reencontra o fato e retorna a pergunta existente sem limpar validação/materialização (`app/chamados/models.py:2061`, `:2062`, `:2084`). |
| **Gatilho da fábrica** (`app/ponto/services/cartorio.py:759`) | Só pergunta para célula julgada nessa passada (`:769`, `:770`); não é acionada pela mera ausência de fio. Impressão igual pula (`:1092`). |
| **Evento de beco de disputa** (`app/chamados/services/becos.py:25`) | Exige pai resolvido; se pai segue aberto, `classificar` retorna None (`:31`). Portanto não fecha diretamente o chamado aberto do laudo. |

**Há evento para nascimento recente**, mas não é prova de reparo do estoque: `abrir` emite evento para chamado de turno com célula (`app/chamados/models.py:736`, `:738`, `:739`); lavra grava evento quando muda `veredito` ou `ata` (`app/ponto/portas/celula.py:623`, `:638`, `:642`). Sem evento pendente, o consumidor não varre espontaneamente aquele colaborador (`app/chamados/reconciliador.py:1128`, `:1135`). Se o evento existe, as guardas da tabela continuam valendo.

**O261 conta #22914? Não é decidível só com “aberto sem pergunta viva”.**

- Precisa célula associada e módulo de `FURO_MODULES` (`app/chamados/services/vigia_fio.py:184`, `:199`); vocabulário: `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto` (`app/chamados/catalogo/furo_modulos.py:15`).
- Precisa célula **acusadora** (`vigia_fio.py:226`, `:227`).
- O fio é reduzido pelas perguntas da **mesma célula**, não somente pelas resolvedoras daquele chamado (`:242`, `:244`, `:258`). Assim, outra pergunta viva da célula pode excluir #22914 embora nenhuma resolvedora dele esteja viva.
- Exclui hoje/futuro (`:261`), decisão do dia oferecida ao admin (`:266`), ausência aberta (`:273`) e aceite de folga validado (`:279`).
- Se passa tudo, conta uma célula em `fio_morto` (`:286`) e, havendo chamado em `COBRANCA`, também em `fio_morto_cobrando` (`:200`, `:288`, `:289`).

**Conclusão específica:** por que **nenhum** caminho alcançou #22914 e se ele entra no O261: **não provado**. Faltam, no material lido, módulo/célula/data/veredito, campos das perguntas e resultado dos eventos. As guardas acima demonstram caminhos de permanência; escolher uma como causa desse ID seria **hipótese**.

## 29/10 12:25 · sessao 0364591182dc

HEAD: `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. Sem escrita, testes ou banco. **Hipótese** abaixo indica caminho possível por leitura, não ocorrência demonstrada.

## T12 — Entradas do julgamento fora da impressão

A chave contém Batidas `(pk,tipo,timestamp)`, cobertura escolhida `.status`, Chamados `(pk,status_local)`, DNA `(dna_versao,marcos)`, veto de régua, quantidade de marcos vencidos, versão da regra, encerramento, quantidade de chamados com fio e classificação da cobertura parcial (`app/ponto/services/cartorio.py:323`, `:329`, `:334`, `:342`, `:352`, `:367`, `:375`). O pulo normal ocorre em `:1092`; o ramo sem adesão tem chave/pulo próprios em `:941`, `:943`.

| Entrada efetivamente lida | Está na chave? |
|---|---|
| Batidas apuráveis da janela (`app/ponto/services/cartorio.py:871`, `:1042`) | **Sim**, identidade, tipo e instante (`:324`). Retratação muda o conjunto apurável. |
| `Batida.is_intervalo`, consumida pela grade/pareador (`app/escala/utils.py:1267`; `app/ponto/turnos.py:1261`) | **Não** na tupla serializada (`app/ponto/services/cartorio.py:324`). |
| Primeira batida apurável **da vida**, que define adesão (`app/ponto/services/cartorio.py:899`, `:901`) | **Não diretamente**: a chave usa a janela local `bj` (`:1042`). `nunca_bateu` tem exceção ao pulo normal (`:1092`), mas isso não serializa a data de adesão. |
| Chamados e estados usados no julgamento (`app/ponto/services/cartorio.py:1040`, `:1069`) | **Sim**, identidade/estado (`:326`). |
| Fio vivo (`app/ponto/services/cartorio.py:1074`, `:1110`) | **Sim como quantidade**, não identidade/conteúdo das perguntas (`:1087`, `:367`). |
| Bordas/pausas do DNA da célula corrente (`app/escala/utils.py:53`, `:104`, `:1547`) | **Sim**, em `dna.marcos` (`app/ponto/services/cartorio.py:327`). |
| `CelulaDia.trabalha`, geradora e vínculo escolhido (`app/escala/utils.py:1252`, `:1255`, `:1545`; `app/ponto/supra_juiz.py:136`, `:171`) | **Não diretamente**: o hash da célula contém apenas versão/marcos do DNA (`app/ponto/services/cartorio.py:327`). |
| Vigência, atividade, âncora, ciclo, intervalo livre, defeito de folga e template (`app/escala/utils.py:1240`, `:1300`, `:1313`; `app/ponto/supra_juiz.py:175`) | **Parcial**: marcos congelados entram; os demais campos não são serializados (`app/ponto/services/cartorio.py:327`). |
| FolgaDia/foto, que sobrepõe tipo e remove faltantes/órfãs (`app/escala/utils.py:1109`, `:1118`, `:1119`) | **Não diretamente** (`app/ponto/services/cartorio.py:323`). |
| Justificativa aprovada, limpeza de faltantes e `dia.justificado` (`app/escala/utils.py:1157`, `:1200`, `:1203`) | **Não**. A ata lê essa marca para dispensar lâmpadas (`app/ponto/services/cartorio.py:602`, `:610`). |
| Ausência escolhida para cobrança, inclusive não aprovada (`app/ponto/services/cartorio.py:1041`) | **Parcial**: somente o status escolhido (`:325`), não identidade, tipo, datas, minutos ou horas. |
| Cobertura aprovada usada pela grade para produzir tipo do dia, faltantes e órfãs (`app/escala/utils.py:1148`, `:1173`, `:1180`, `:1195`) | **Parcial**: o hash recebe status da cobertura de cobrança e classificação parcial, não a linha completa da cobertura de folha (`app/ponto/services/cartorio.py:1083`, `:1088`). |
| Integralidade e posição das batidas perante ausência parcial (`app/escala/utils.py:1190`, `:1194`) | **Sim como resultado comprimido**: parcial/`sem_periodo`/`dentro`/`fora` (`app/ponto/services/cartorio.py:375`, `:1088`); horas/minutos exatos não entram. |
| Tipo usado em `lampada.dispensada` (`app/ponto/services/cartorio.py:604`, `:610`) | **Não**: duas ausências integrais aprovadas de tipos diferentes podem preservar os agregados da chave (`:325`, `:375`). |
| Catálogo TipoAusencia, incluindo efeito e parcialidade (`app/ponto/catalogo/ausencias.py:249`, `:250`; `app/escala/utils.py:1147`) | **Não diretamente**; somente resultados derivados de cobertura entram (`app/ponto/services/cartorio.py:325`, `:375`). |
| Isenção do colaborador (`app/escala/utils.py:865`; `app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:1113`) | **Não diretamente**. O veto serializado trata apenas régua degenerada/suspeita (`app/ponto/services/furos_vetados.py:19`, `:28`, `:30`). |
| Demissão usada no cadastro do dia (`app/ponto/services/cartorio.py:844`; `app/ponto/supra_juiz.py:169`) | **Não** (`app/ponto/services/cartorio.py:323`). |
| Situação e afastamento atual, controlando emissão (`app/ponto/services/cartorio.py:1033`, `:1340`, `:1381`) | **Não**; são entradas dos efeitos de emissão, não necessariamente do veredito/ata (`:323`). |
| Encerramento e teto por instante (`app/ponto/services/cartorio.py:1049`, `:1056`, `:1117`) | **Sim como agregados**: encerramento e quantidade de vencidos (`:1084`, `:1085`; serialização em `:334`, `:352`). |
| Versão declarada da regra (`app/ponto/services/cartorio.py:890`) | **Sim** (`:342`, `:1086`); isso não equivale automaticamente à versão de qualquer código alterado. |
| Ata/DNA de outras células da janela, usados pelo pré-juiz (`app/escala/utils.py:1252`, `:1519`, `:1524`) | **Não** na chave da célula corrente (`app/ponto/services/cartorio.py:323`, `:1083`). |
| Feriado/localização e precedência | **Não serializados diretamente**. Há, porém, uma diferença importante: `julgar_celula` recalcula a chave `ata.precedencia` **depois** de `julgar_colab`, mesmo que este pule (`app/ponto/services/cartorio.py:1519`, `:1560`, `:1568`). O batch chama `julgar_colab` diretamente (`app/ponto/management/commands/processar_cartorio.py:65`). |
| `TurnoMaterializado` | **Não é entrada direta encontrada**: o carregador de encerramento usa `turnos_do_colab` (`app/ponto/turnos.py:2011`, `:2013`, `:2018`). |

**Escritas e invalidação por `forcar=True`:**

| Escrita da entrada ausente | Força outro julgamento? |
|---|---|
| Justificativa aprovar/rejeitar/materializar (`app/ponto/services/justificativa.py:88`, `:170`, `:265`) | Signal chama cartório **sem force** (`app/ponto/signals.py:161`, `:165`, `:93`). |
| Ausência criar/editar/decidir/apagar (`app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`, `:1174`) | Signals de save/delete chamam **sem force** (`app/ponto/signals.py:143`, `:150`, `:93`). |
| TipoAusencia salvar/criar (`app/ponto/services/ausencia.py:1292`, `:1294`) | Receiver encontrado invalida o catálogo, não força julgamento (`app/ponto/models.py:1233`, `:1236`). |
| Registrar/retratar/apagar Batida (`app/ponto/registro_batida.py:94`, `:267`, `:285`) | Signals sem force (`app/ponto/signals.py:131`, `:138`, `:93`), mas presença/tipo/instante normalmente mudam a chave. Escritor produtivo que altere **somente** `is_intervalo`: **nao li**. |
| Flags/demissão/posto de colaborador (`app/colaboradores/services/colaborador.py:32`, `:53`, `:61`, `:68`, `:75`) | Nenhum force nesses setters. O signal de isenção silencia chamados (`app/chamados/signals.py:207`, `:217`); isso pode alterar indiretamente outro componente da chave. |
| Vigência/âncora e FolgaDia (`app/colaboradores/services/vinculo.py:799`, `:845`; `app/escala/folgas.py:172`, `:259`, `:289`) | **Condicional:** regeneração força somente datas tocadas, com `recalculo=True` e filtros de data/via humana (`app/ponto/portas/celula.py:359`, `:368`, `:371`). |
| Editar TipoEscala (`app/escala/services/cadastro_tipo.py:306`) | Regenera com `recalculo=False` (`:348`, `:350`); não alcança aquele force (`app/ponto/portas/celula.py:359`). |
| Reverter regeneração (`app/ponto/portas/celula.py:537`) | Sem chamada equivalente de force no corpo até `:551`. |
| Gravar ata/veredito (`app/ponto/portas/celula.py:650`, `:671`) | Não chama julgamento forçado nessa operação; decisão administrativa tem preservação própria (`:645`, `:646`). |
| Feriado criar/editar/excluir (`app/core/views_config.py:81`, `:68`, `:198`) | Save agenda reavaliador de chamados, não force do cartório (`app/chamados/signals.py:259`, `:269`); ausência de post_delete declarada em `:255`. |
| Alterar município/localização (`app/colaboradores/services/estrutura.py:50`, `:70`; `app/colaboradores/management/commands/seed_municipios.py:38`) | Nenhum force encontrado nessas portas. Outros chamadores: **nao li**. |

Existem forças adicionais no pós-vínculo, segunda passada enfileirada e ação explícita de cobrança; não demonstram cobertura universal das escritas acima (`app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/services/cartorio.py:746`; `app/chamados/services/cobrar_dia.py:29`).

**Podem deixar célula velha — hipóteses quando os demais componentes permanecem iguais:** justificativa decidida; alteração de tipo/conteúdo de ausência sem mudar status/parcial; catálogo de ausência; isenção/demissão; foto de folga; vínculo/template/geradora fora das condições de force; ata de outra célula da janela; primeira batida global fora de `bj`. As omissões e gatilhos estão nas linhas das duas tabelas. Para Feriado/localização, não confundir veredito/lâmpadas com `ata.precedencia`, que tem atualização separada em `julgar_celula` (`app/ponto/services/cartorio.py:1568`).

Completude recursiva de todos os métodos de modelo e chamadores de cadastro: **nao li integralmente**. Ocorrências reais de célula velha: não verificadas.

## T13 — Outros caches com chave incompleta

| Sítio / proteção | Chave e pulo | Entrada ausente / suspeita |
|---|---|---|
| **Memo de `pergunta_viva`** (`app/chamados/juizes.py:778`) | Campos da pergunta, veredito/via da célula e estado do destino (`:767`); igualdade retorna memo (`:791`, `:794`). | O cálculo lê motivo, DNA, lâmpadas e pendências (`:842`, `:849`, `:1051`, `:1064`), ausentes da chave. **Hipótese:** mesma instância reutilizada após alteração de ATA/DNA/motivo mantém resposta antiga. Cache limitado à instância/request (`:788`). |
| **Lavra S2 / seleção de perguntas stale** (`app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:31`) | Nunca lavrado, prazo expirado ou timestamps de insumos posteriores ao julgamento (`veredito_lavrado.py:137`, `:179`; `raiox.py:43`). | **Versão do código não entra.** O backfill declara que mudança de regra/código não é detectada por `esta_stale` (`app/chamados/management/commands/backfill_veredito_lavrado.py:5`). Há reparo separado por comparação com juiz atual (`:11`). |
| **`houve_fato_novo`**, gate de renascimento (`app/ponto/services/cartorio.py:249`; `app/chamados/models.py:956`) | Timestamps de Batida, Ausência, Justificativa, célula e marco vencido (`cartorio.py:272`, `:275`, `:279`, `:285`, `:287`). | **Exclusão física não deixa linha consultável. Hipótese:** exclusão posterior à decisão admin, sem outro carimbo observado, pode não ser reconhecida. Feriado/localização/vínculo também dependem de efeitos indiretos nesses carimbos. Não é cache de cálculo; pula renascimento. |

**Reaproveitamentos adjacentes, distintos de pular o cálculo do cartório:**

| Sítio | Chave | Entrada ausente / limite |
|---|---|---|
| Frescor do RetratoFolha (`app/folha/retrato_folha.py:1268`) | Compara `FechamentoMensal.atualizado_em` com `RetratoFolha.lavrado_em` (`:1296`, `:1306`). | Retrato usa retenções dependentes de células/vereditos (`:1157`; `app/folha/export.py:1188`, `:1215`). **Hipótese:** célula muda sem atualizar fechamento e o rótulo continua atualizado. O lavrador não pula montagem por isso (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`). |
| ETag do placar (`app/chamados/services/painel.py:79`) | Máximos de Chamado/Pergunta, filtros, usuário/schema e quantum de120s (`:64`, `:91`); igualdade permite304 (`app/chamados/views.py:822`). | Resposta usa PlacarNavbar e particionamento dependente do fio (`views.py:844`; `painel.py:227`). Célula/ATA e `PlacarNavbar.lavrado_em` não entram diretamente. **Hipótese limitada ao quantum:** fonte muda sem mover os máximos e há304. |

**Procurados sem outro caso de chave incompleta demonstrado:**

- **DiaPago:** monta linhas e faz `delete/bulk_create`; não compara hash de entrada para pular (`app/ponto/services/dia_pago.py:333`, `:343`, `:357`). Hash no comando é prova antes/depois (`app/ponto/management/commands/lavrar_dias_pagos.py:91`, `:132`).
- **FechamentoMensal:** invalidação por `derivado_em=None`; leitor chama derivação online (`app/ponto/services/fechamento.py:1611`, `:1655`, `:1718`).
- **Lavra de ata/veredito:** evita gravação quando a **saída** já é igual; não evita produzir o resultado (`app/ponto/portas/celula.py:643`, `:661`).
- **Espelho em lote:** hash identifica pedido e só reutiliza job em curso (`app/relatorios/lote_espelho.py:49`, `:98`).
- **Retrato de HE/teto:** apuração chama espelho; lavra usa `update_or_create` (`app/ponto/services/he_pendente_lavrado.py:92`, `:180`).

Foram buscados hashes, impressões, fingerprints, carimbos e condições de reaproveitamento em `app/`. Caches de sessão, snapshots de inteligência e demais memos fora dos sítios acima: **nao li**. Nenhuma hipótese desta seção foi reproduzida.

## T14 — Caminhos para fio morto O261

O contador exige chamado vivo com célula, módulo de furo, célula acusadora e ausência de pergunta viva; ainda exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`app/chamados/services/vigia_fio.py:184`, `:199`, `:227`, `:258`, `:261`, `:266`, `:273`, `:279`). Só então conta em `:286`.

**L** marca escrita direta na pergunta/vitalidade/veredito equivalente à lâmpada; não significa, por si, violação da regra (`CLAUDE.md:273`, `:276`).

| Caminho | Como pode produzir/conservar fio morto |
|---|---|
| **L — validar pergunta** (`app/chamados/services/validacao.py:131`, `:133`) | `validada_em` mata antes de consultar a célula acusadora (`app/chamados/juizes.py:1006`). **Hipótese:** chamado/célula permanecem e não há outro fio. A porta tenta devolver validação de resposta-hora recusada; não é falha inevitável (`validacao.py:415`, `:418`). |
| **L — carimbar `fato_ocupado`** (`app/chamados/services/materializacao.py:1042`, `:1053`) | Via de identidade materializada mata mesmo com célula acusadora (`app/chamados/juizes.py:1039`, `:1044`), sem fechar chamado/absolver célula nessa operação. **Hipótese**, condicionada aos filtros do contador. |
| **L — `paridade_recusada`** (`app/chamados/services/materializacao.py:858`, `:868`) | Também encerra por identidade (`app/chamados/juizes.py:1042`). Porém exige marco já existente no espelho antes do carimbo (`materializacao.py:840`, `:841`); não prova morte cega. Pergunta previamente validada já estava morta (`:848`). |
| **L — encerrar administrativamente disputa/perguntas** (`app/chamados/models.py:1404`, `:2036`, `:2039`) | A identidade administrativa vence a acusação (`app/chamados/juizes.py:1039`). Signal tenta fechar o pai da disputa, não incondicionalmente todos os chamados-destino (`app/chamados/signals.py:19`, `:42`). **Hipótese:** destino de furo sobrevive. |
| **L — remover motivo apagando perguntas** (`app/chamados/models.py:1519`) | Com motivos restantes, retorna sem fechar disputa/pai nem absolver célula (`:1522`, `:1524`). **Hipótese:** perguntas apagadas eram o único fio daquela célula. O evento de pergunta é `post_save`, não esse delete (`app/chamados/signals.py:185`). |
| **L — marcar duplicata** (`app/chamados/services/fio.py:844`, `:848`) | Mata a excedente por identidade (`app/chamados/juizes.py:1040`). **Não produz sozinho** se a canônica permanece viva na mesma célula; a migração escolhe canônica entre vivas (`fio.py:882`, `:890`). |
| **Abrir `batida_ausente` antes de produzir perguntas** (`app/ponto/management/commands/detectar_ausencias.py:688`) | Etapa de fio é posterior e best-effort (`:777`, `:791`, `:792`). Falha ou ausência de órfãos elegíveis não desfaz nascimento. **Hipótese de persistência**, pois cartório/outros eventos ainda podem reparar. |
| **Abrir/reusar `turno_aberto`** (`app/ponto/management/commands/processar_alertas_turno.py:318`) | Perguntas só são tentadas se `criado`, em `try/except` (`:355`, `:367`, `:368`). Reuso não executa essa etapa. **Hipótese**, não consequência inevitável. |
| **Grade emite sem pergunta** (`app/ponto/management/commands/reconciliar_grade.py:498`) | Abre/reabre e retorna sem gerar perguntas (`:519`, `:520`). Caminho manual executável; o cron não passa `--apply` (`app/config/crons.py:436`). |
| **View cria bloqueio de turno** (`app/ponto/views.py:290`) | Ramo cria `entrada_bloqueada_turno_aberto` sem pergunta (`:298`, `:301`). O módulo pertence ao vocabulário (`app/chamados/catalogo/furo_modulos.py:15`), mas O261 ainda exige célula associada e acusadora; ocorrência dessas condições não provada. |
| **Pergunta recusada por ausência de ata legível** (`app/chamados/services/disputa_emissao.py:833`, `:842`) | Chamado pode existir antes; posterior célula acusadora pode encontrá-lo sem fio. **Hipótese**, sujeita à fábrica do julgamento seguinte. |
| **L — célula volta a acusar após pergunta morta** (`app/ponto/services/cartorio.py:1409`; `app/ponto/portas/celula.py:652`, `:671`) | Validação e identidades terminadoras continuam vencendo a acusação (`app/chamados/juizes.py:1006`, `:1044`). Para outras mortes, acusação posterior pode tornar pergunta viva novamente (`:1055`, `:1057`). |
| **Fábrica preserva identidade morta** (`app/chamados/services/fabrica_da_celula.py:93`, `:95`) | Dedup considera perguntas existentes não duplicatas, não apenas vivas. O gate reencontra o fato sem limpar validação/materialização (`app/chamados/models.py:1966`, `:1988`). Pode conservar a falta de fio após relavra acusadora. |

**Reparos existentes têm recortes:**

- Beco por evento exige pai já resolvido, não o chamado vivo do O261 (`app/chamados/services/becos.py:31`).
- Ressuscitação do cartório ignora chamado sem perguntas e perguntas com texto (`app/ponto/services/cartorio.py:1261`, `:1263`).
- `ressuscitar_perguntas` seleciona materializadas sem texto; validada sem materialização não entra (`app/chamados/models.py:1028`, `:1031`).
- Fábrica por evento só alcança célula julgada; impressão igual pode impedir esse caminho (`app/ponto/services/cartorio.py:1092`, `:769`, `:770`).

Não tratar como produtor automático a absolvição por célula: ela exige não acusação/ausência de pendências e pode ser desfeita pela acusação posterior (`app/chamados/reconciliador.py:529`, `:538`; `app/chamados/juizes.py:1055`). Tranca também tenta encerrar chamados, além de matar perguntas (`app/chamados/services/competencia_trancada.py:174`, `:177`).

Todos os escritores fora das portas e caminhos citados: **nao li integralmente**. Ocorrências/quantidades de produção: **nao li**.

## T15 — Emissor de `batida_ausente` pergunta à precedência?

Os nascimentos diretos encontrados são o funil de ausência, o emissor da grade e o reemissor C2 (`app/ponto/management/commands/detectar_ausencias.py:688`; `app/ponto/management/commands/reconciliar_grade.py:498`; `app/chamados/reconciliador.py:710`).

| Caminho | Consulta `fatos_do_dia` antes de criar? | O que efetivamente barra |
|---|---|---|
| `detectar_ausencias.handle` → `_abrir_chamado` (`app/ponto/management/commands/detectar_ausencias.py:439`, `:688`) | **Sim**, mas o resultado só veta `faixa == 'feriado'` (`:615`, `:616`). | Folga depende de `eh_dia_trabalho`/célula (`:143`, `:305`, `:626`); ausência é consultada no chamador (`:346`, `:805`). Não há veto geral de todas as faixas da precedência no funil. |
| Cartório → mesmo funil (`app/ponto/services/cartorio.py:1358`, `:1394`) | **Sim indiretamente**, pelo mesmo `detectar_ausencias.py:616`. | Antes disso usa protestos e ata do julgamento (`cartorio.py:1339`, `:1381`, `:1387`); não substitui o veto por feriado do funil. |
| `emitir_furo_retroativo` → mesmo funil (`app/ponto/management/commands/emitir_furo_retroativo.py:142`) | **Sim indiretamente**. | Exige `FURO_SEM_COBRANCA`, vínculo e trabalho previsto (`:109`, `:118`, `:127`), depois executa os vetos do funil. |
| `reconciliar_grade._emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:498`) | **Não nesse emissor.** | Régua e veto parcial 

[truncado em 20000 de 22228 caracteres]

## 29/10 12:25 · sessao 0364591182dc

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 12:25 · sessao 0364591182dc

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 12:25 · sessao 0364591182dc

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 12:25 · sessao 0364591182dc

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 12:25 · sessao 1b017e44d7fa

HEAD: `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. Sem escrita, testes ou banco. **Hipótese** abaixo indica caminho possível por leitura, não ocorrência demonstrada.

## T12 — Entradas do julgamento fora da impressão

A chave contém Batidas `(pk,tipo,timestamp)`, cobertura escolhida `.status`, Chamados `(pk,status_local)`, DNA `(dna_versao,marcos)`, veto de régua, quantidade de marcos vencidos, versão da regra, encerramento, quantidade de chamados com fio e classificação da cobertura parcial (`app/ponto/services/cartorio.py:323`, `:329`, `:334`, `:342`, `:352`, `:367`, `:375`). O pulo normal ocorre em `:1092`; o ramo sem adesão tem chave/pulo próprios em `:941`, `:943`.

| Entrada efetivamente lida | Está na chave? |
|---|---|
| Batidas apuráveis da janela (`app/ponto/services/cartorio.py:871`, `:1042`) | **Sim**, identidade, tipo e instante (`:324`). Retratação muda o conjunto apurável. |
| `Batida.is_intervalo`, consumida pela grade/pareador (`app/escala/utils.py:1267`; `app/ponto/turnos.py:1261`) | **Não** na tupla serializada (`app/ponto/services/cartorio.py:324`). |
| Primeira batida apurável **da vida**, que define adesão (`app/ponto/services/cartorio.py:899`, `:901`) | **Não diretamente**: a chave usa a janela local `bj` (`:1042`). `nunca_bateu` tem exceção ao pulo normal (`:1092`), mas isso não serializa a data de adesão. |
| Chamados e estados usados no julgamento (`app/ponto/services/cartorio.py:1040`, `:1069`) | **Sim**, identidade/estado (`:326`). |
| Fio vivo (`app/ponto/services/cartorio.py:1074`, `:1110`) | **Sim como quantidade**, não identidade/conteúdo das perguntas (`:1087`, `:367`). |
| Bordas/pausas do DNA da célula corrente (`app/escala/utils.py:53`, `:104`, `:1547`) | **Sim**, em `dna.marcos` (`app/ponto/services/cartorio.py:327`). |
| `CelulaDia.trabalha`, geradora e vínculo escolhido (`app/escala/utils.py:1252`, `:1255`, `:1545`; `app/ponto/supra_juiz.py:136`, `:171`) | **Não diretamente**: o hash da célula contém apenas versão/marcos do DNA (`app/ponto/services/cartorio.py:327`). |
| Vigência, atividade, âncora, ciclo, intervalo livre, defeito de folga e template (`app/escala/utils.py:1240`, `:1300`, `:1313`; `app/ponto/supra_juiz.py:175`) | **Parcial**: marcos congelados entram; os demais campos não são serializados (`app/ponto/services/cartorio.py:327`). |
| FolgaDia/foto, que sobrepõe tipo e remove faltantes/órfãs (`app/escala/utils.py:1109`, `:1118`, `:1119`) | **Não diretamente** (`app/ponto/services/cartorio.py:323`). |
| Justificativa aprovada, limpeza de faltantes e `dia.justificado` (`app/escala/utils.py:1157`, `:1200`, `:1203`) | **Não**. A ata lê essa marca para dispensar lâmpadas (`app/ponto/services/cartorio.py:602`, `:610`). |
| Ausência escolhida para cobrança, inclusive não aprovada (`app/ponto/services/cartorio.py:1041`) | **Parcial**: somente o status escolhido (`:325`), não identidade, tipo, datas, minutos ou horas. |
| Cobertura aprovada usada pela grade para produzir tipo do dia, faltantes e órfãs (`app/escala/utils.py:1148`, `:1173`, `:1180`, `:1195`) | **Parcial**: o hash recebe status da cobertura de cobrança e classificação parcial, não a linha completa da cobertura de folha (`app/ponto/services/cartorio.py:1083`, `:1088`). |
| Integralidade e posição das batidas perante ausência parcial (`app/escala/utils.py:1190`, `:1194`) | **Sim como resultado comprimido**: parcial/`sem_periodo`/`dentro`/`fora` (`app/ponto/services/cartorio.py:375`, `:1088`); horas/minutos exatos não entram. |
| Tipo usado em `lampada.dispensada` (`app/ponto/services/cartorio.py:604`, `:610`) | **Não**: duas ausências integrais aprovadas de tipos diferentes podem preservar os agregados da chave (`:325`, `:375`). |
| Catálogo TipoAusencia, incluindo efeito e parcialidade (`app/ponto/catalogo/ausencias.py:249`, `:250`; `app/escala/utils.py:1147`) | **Não diretamente**; somente resultados derivados de cobertura entram (`app/ponto/services/cartorio.py:325`, `:375`). |
| Isenção do colaborador (`app/escala/utils.py:865`; `app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:1113`) | **Não diretamente**. O veto serializado trata apenas régua degenerada/suspeita (`app/ponto/services/furos_vetados.py:19`, `:28`, `:30`). |
| Demissão usada no cadastro do dia (`app/ponto/services/cartorio.py:844`; `app/ponto/supra_juiz.py:169`) | **Não** (`app/ponto/services/cartorio.py:323`). |
| Situação e afastamento atual, controlando emissão (`app/ponto/services/cartorio.py:1033`, `:1340`, `:1381`) | **Não**; são entradas dos efeitos de emissão, não necessariamente do veredito/ata (`:323`). |
| Encerramento e teto por instante (`app/ponto/services/cartorio.py:1049`, `:1056`, `:1117`) | **Sim como agregados**: encerramento e quantidade de vencidos (`:1084`, `:1085`; serialização em `:334`, `:352`). |
| Versão declarada da regra (`app/ponto/services/cartorio.py:890`) | **Sim** (`:342`, `:1086`); isso não equivale automaticamente à versão de qualquer código alterado. |
| Ata/DNA de outras células da janela, usados pelo pré-juiz (`app/escala/utils.py:1252`, `:1519`, `:1524`) | **Não** na chave da célula corrente (`app/ponto/services/cartorio.py:323`, `:1083`). |
| Feriado/localização e precedência | **Não serializados diretamente**. Há, porém, uma diferença importante: `julgar_celula` recalcula a chave `ata.precedencia` **depois** de `julgar_colab`, mesmo que este pule (`app/ponto/services/cartorio.py:1519`, `:1560`, `:1568`). O batch chama `julgar_colab` diretamente (`app/ponto/management/commands/processar_cartorio.py:65`). |
| `TurnoMaterializado` | **Não é entrada direta encontrada**: o carregador de encerramento usa `turnos_do_colab` (`app/ponto/turnos.py:2011`, `:2013`, `:2018`). |

**Escritas e invalidação por `forcar=True`:**

| Escrita da entrada ausente | Força outro julgamento? |
|---|---|
| Justificativa aprovar/rejeitar/materializar (`app/ponto/services/justificativa.py:88`, `:170`, `:265`) | Signal chama cartório **sem force** (`app/ponto/signals.py:161`, `:165`, `:93`). |
| Ausência criar/editar/decidir/apagar (`app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`, `:1174`) | Signals de save/delete chamam **sem force** (`app/ponto/signals.py:143`, `:150`, `:93`). |
| TipoAusencia salvar/criar (`app/ponto/services/ausencia.py:1292`, `:1294`) | Receiver encontrado invalida o catálogo, não força julgamento (`app/ponto/models.py:1233`, `:1236`). |
| Registrar/retratar/apagar Batida (`app/ponto/registro_batida.py:94`, `:267`, `:285`) | Signals sem force (`app/ponto/signals.py:131`, `:138`, `:93`), mas presença/tipo/instante normalmente mudam a chave. Escritor produtivo que altere **somente** `is_intervalo`: **nao li**. |
| Flags/demissão/posto de colaborador (`app/colaboradores/services/colaborador.py:32`, `:53`, `:61`, `:68`, `:75`) | Nenhum force nesses setters. O signal de isenção silencia chamados (`app/chamados/signals.py:207`, `:217`); isso pode alterar indiretamente outro componente da chave. |
| Vigência/âncora e FolgaDia (`app/colaboradores/services/vinculo.py:799`, `:845`; `app/escala/folgas.py:172`, `:259`, `:289`) | **Condicional:** regeneração força somente datas tocadas, com `recalculo=True` e filtros de data/via humana (`app/ponto/portas/celula.py:359`, `:368`, `:371`). |
| Editar TipoEscala (`app/escala/services/cadastro_tipo.py:306`) | Regenera com `recalculo=False` (`:348`, `:350`); não alcança aquele force (`app/ponto/portas/celula.py:359`). |
| Reverter regeneração (`app/ponto/portas/celula.py:537`) | Sem chamada equivalente de force no corpo até `:551`. |
| Gravar ata/veredito (`app/ponto/portas/celula.py:650`, `:671`) | Não chama julgamento forçado nessa operação; decisão administrativa tem preservação própria (`:645`, `:646`). |
| Feriado criar/editar/excluir (`app/core/views_config.py:81`, `:68`, `:198`) | Save agenda reavaliador de chamados, não force do cartório (`app/chamados/signals.py:259`, `:269`); ausência de post_delete declarada em `:255`. |
| Alterar município/localização (`app/colaboradores/services/estrutura.py:50`, `:70`; `app/colaboradores/management/commands/seed_municipios.py:38`) | Nenhum force encontrado nessas portas. Outros chamadores: **nao li**. |

Existem forças adicionais no pós-vínculo, segunda passada enfileirada e ação explícita de cobrança; não demonstram cobertura universal das escritas acima (`app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/services/cartorio.py:746`; `app/chamados/services/cobrar_dia.py:29`).

**Podem deixar célula velha — hipóteses quando os demais componentes permanecem iguais:** justificativa decidida; alteração de tipo/conteúdo de ausência sem mudar status/parcial; catálogo de ausência; isenção/demissão; foto de folga; vínculo/template/geradora fora das condições de force; ata de outra célula da janela; primeira batida global fora de `bj`. As omissões e gatilhos estão nas linhas das duas tabelas. Para Feriado/localização, não confundir veredito/lâmpadas com `ata.precedencia`, que tem atualização separada em `julgar_celula` (`app/ponto/services/cartorio.py:1568`).

Completude recursiva de todos os métodos de modelo e chamadores de cadastro: **nao li integralmente**. Ocorrências reais de célula velha: não verificadas.

## T13 — Outros caches com chave incompleta

| Sítio / proteção | Chave e pulo | Entrada ausente / suspeita |
|---|---|---|
| **Memo de `pergunta_viva`** (`app/chamados/juizes.py:778`) | Campos da pergunta, veredito/via da célula e estado do destino (`:767`); igualdade retorna memo (`:791`, `:794`). | O cálculo lê motivo, DNA, lâmpadas e pendências (`:842`, `:849`, `:1051`, `:1064`), ausentes da chave. **Hipótese:** mesma instância reutilizada após alteração de ATA/DNA/motivo mantém resposta antiga. Cache limitado à instância/request (`:788`). |
| **Lavra S2 / seleção de perguntas stale** (`app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:31`) | Nunca lavrado, prazo expirado ou timestamps de insumos posteriores ao julgamento (`veredito_lavrado.py:137`, `:179`; `raiox.py:43`). | **Versão do código não entra.** O backfill declara que mudança de regra/código não é detectada por `esta_stale` (`app/chamados/management/commands/backfill_veredito_lavrado.py:5`). Há reparo separado por comparação com juiz atual (`:11`). |
| **`houve_fato_novo`**, gate de renascimento (`app/ponto/services/cartorio.py:249`; `app/chamados/models.py:956`) | Timestamps de Batida, Ausência, Justificativa, célula e marco vencido (`cartorio.py:272`, `:275`, `:279`, `:285`, `:287`). | **Exclusão física não deixa linha consultável. Hipótese:** exclusão posterior à decisão admin, sem outro carimbo observado, pode não ser reconhecida. Feriado/localização/vínculo também dependem de efeitos indiretos nesses carimbos. Não é cache de cálculo; pula renascimento. |

**Reaproveitamentos adjacentes, distintos de pular o cálculo do cartório:**

| Sítio | Chave | Entrada ausente / limite |
|---|---|---|
| Frescor do RetratoFolha (`app/folha/retrato_folha.py:1268`) | Compara `FechamentoMensal.atualizado_em` com `RetratoFolha.lavrado_em` (`:1296`, `:1306`). | Retrato usa retenções dependentes de células/vereditos (`:1157`; `app/folha/export.py:1188`, `:1215`). **Hipótese:** célula muda sem atualizar fechamento e o rótulo continua atualizado. O lavrador não pula montagem por isso (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`). |
| ETag do placar (`app/chamados/services/painel.py:79`) | Máximos de Chamado/Pergunta, filtros, usuário/schema e quantum de120s (`:64`, `:91`); igualdade permite304 (`app/chamados/views.py:822`). | Resposta usa PlacarNavbar e particionamento dependente do fio (`views.py:844`; `painel.py:227`). Célula/ATA e `PlacarNavbar.lavrado_em` não entram diretamente. **Hipótese limitada ao quantum:** fonte muda sem mover os máximos e há304. |

**Procurados sem outro caso de chave incompleta demonstrado:**

- **DiaPago:** monta linhas e faz `delete/bulk_create`; não compara hash de entrada para pular (`app/ponto/services/dia_pago.py:333`, `:343`, `:357`). Hash no comando é prova antes/depois (`app/ponto/management/commands/lavrar_dias_pagos.py:91`, `:132`).
- **FechamentoMensal:** invalidação por `derivado_em=None`; leitor chama derivação online (`app/ponto/services/fechamento.py:1611`, `:1655`, `:1718`).
- **Lavra de ata/veredito:** evita gravação quando a **saída** já é igual; não evita produzir o resultado (`app/ponto/portas/celula.py:643`, `:661`).
- **Espelho em lote:** hash identifica pedido e só reutiliza job em curso (`app/relatorios/lote_espelho.py:49`, `:98`).
- **Retrato de HE/teto:** apuração chama espelho; lavra usa `update_or_create` (`app/ponto/services/he_pendente_lavrado.py:92`, `:180`).

Foram buscados hashes, impressões, fingerprints, carimbos e condições de reaproveitamento em `app/`. Caches de sessão, snapshots de inteligência e demais memos fora dos sítios acima: **nao li**. Nenhuma hipótese desta seção foi reproduzida.

## T14 — Caminhos para fio morto O261

O contador exige chamado vivo com célula, módulo de furo, célula acusadora e ausência de pergunta viva; ainda exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`app/chamados/services/vigia_fio.py:184`, `:199`, `:227`, `:258`, `:261`, `:266`, `:273`, `:279`). Só então conta em `:286`.

**L** marca escrita direta na pergunta/vitalidade/veredito equivalente à lâmpada; não significa, por si, violação da regra (`CLAUDE.md:273`, `:276`).

| Caminho | Como pode produzir/conservar fio morto |
|---|---|
| **L — validar pergunta** (`app/chamados/services/validacao.py:131`, `:133`) | `validada_em` mata antes de consultar a célula acusadora (`app/chamados/juizes.py:1006`). **Hipótese:** chamado/célula permanecem e não há outro fio. A porta tenta devolver validação de resposta-hora recusada; não é falha inevitável (`validacao.py:415`, `:418`). |
| **L — carimbar `fato_ocupado`** (`app/chamados/services/materializacao.py:1042`, `:1053`) | Via de identidade materializada mata mesmo com célula acusadora (`app/chamados/juizes.py:1039`, `:1044`), sem fechar chamado/absolver célula nessa operação. **Hipótese**, condicionada aos filtros do contador. |
| **L — `paridade_recusada`** (`app/chamados/services/materializacao.py:858`, `:868`) | Também encerra por identidade (`app/chamados/juizes.py:1042`). Porém exige marco já existente no espelho antes do carimbo (`materializacao.py:840`, `:841`); não prova morte cega. Pergunta previamente validada já estava morta (`:848`). |
| **L — encerrar administrativamente disputa/perguntas** (`app/chamados/models.py:1404`, `:2036`, `:2039`) | A identidade administrativa vence a acusação (`app/chamados/juizes.py:1039`). Signal tenta fechar o pai da disputa, não incondicionalmente todos os chamados-destino (`app/chamados/signals.py:19`, `:42`). **Hipótese:** destino de furo sobrevive. |
| **L — remover motivo apagando perguntas** (`app/chamados/models.py:1519`) | Com motivos restantes, retorna sem fechar disputa/pai nem absolver célula (`:1522`, `:1524`). **Hipótese:** perguntas apagadas eram o único fio daquela célula. O evento de pergunta é `post_save`, não esse delete (`app/chamados/signals.py:185`). |
| **L — marcar duplicata** (`app/chamados/services/fio.py:844`, `:848`) | Mata a excedente por identidade (`app/chamados/juizes.py:1040`). **Não produz sozinho** se a canônica permanece viva na mesma célula; a migração escolhe canônica entre vivas (`fio.py:882`, `:890`). |
| **Abrir `batida_ausente` antes de produzir perguntas** (`app/ponto/management/commands/detectar_ausencias.py:688`) | Etapa de fio é posterior e best-effort (`:777`, `:791`, `:792`). Falha ou ausência de órfãos elegíveis não desfaz nascimento. **Hipótese de persistência**, pois cartório/outros eventos ainda podem reparar. |
| **Abrir/reusar `turno_aberto`** (`app/ponto/management/commands/processar_alertas_turno.py:318`) | Perguntas só são tentadas se `criado`, em `try/except` (`:355`, `:367`, `:368`). Reuso não executa essa etapa. **Hipótese**, não consequência inevitável. |
| **Grade emite sem pergunta** (`app/ponto/management/commands/reconciliar_grade.py:498`) | Abre/reabre e retorna sem gerar perguntas (`:519`, `:520`). Caminho manual executável; o cron não passa `--apply` (`app/config/crons.py:436`). |
| **View cria bloqueio de turno** (`app/ponto/views.py:290`) | Ramo cria `entrada_bloqueada_turno_aberto` sem pergunta (`:298`, `:301`). O módulo pertence ao vocabulário (`app/chamados/catalogo/furo_modulos.py:15`), mas O261 ainda exige célula associada e acusadora; ocorrência dessas condições não provada. |
| **Pergunta recusada por ausência de ata legível** (`app/chamados/services/disputa_emissao.py:833`, `:842`) | Chamado pode existir antes; posterior célula acusadora pode encontrá-lo sem fio. **Hipótese**, sujeita à fábrica do julgamento seguinte. |
| **L — célula volta a acusar após pergunta morta** (`app/ponto/services/cartorio.py:1409`; `app/ponto/portas/celula.py:652`, `:671`) | Validação e identidades terminadoras continuam vencendo a acusação (`app/chamados/juizes.py:1006`, `:1044`). Para outras mortes, acusação posterior pode tornar pergunta viva novamente (`:1055`, `:1057`). |
| **Fábrica preserva identidade morta** (`app/chamados/services/fabrica_da_celula.py:93`, `:95`) | Dedup considera perguntas existentes não duplicatas, não apenas vivas. O gate reencontra o fato sem limpar validação/materialização (`app/chamados/models.py:1966`, `:1988`). Pode conservar a falta de fio após relavra acusadora. |

**Reparos existentes têm recortes:**

- Beco por evento exige pai já resolvido, não o chamado vivo do O261 (`app/chamados/services/becos.py:31`).
- Ressuscitação do cartório ignora chamado sem perguntas e perguntas com texto (`app/ponto/services/cartorio.py:1261`, `:1263`).
- `ressuscitar_perguntas` seleciona materializadas sem texto; validada sem materialização não entra (`app/chamados/models.py:1028`, `:1031`).
- Fábrica por evento só alcança célula julgada; impressão igual pode impedir esse caminho (`app/ponto/services/cartorio.py:1092`, `:769`, `:770`).

Não tratar como produtor automático a absolvição por célula: ela exige não acusação/ausência de pendências e pode ser desfeita pela acusação posterior (`app/chamados/reconciliador.py:529`, `:538`; `app/chamados/juizes.py:1055`). Tranca também tenta encerrar chamados, além de matar perguntas (`app/chamados/services/competencia_trancada.py:174`, `:177`).

Todos os escritores fora das portas e caminhos citados: **nao li integralmente**. Ocorrências/quantidades de produção: **nao li**.

## T15 — Emissor de `batida_ausente` pergunta à precedência?

Os nascimentos diretos encontrados são o funil de ausência, o emissor da grade e o reemissor C2 (`app/ponto/management/commands/detectar_ausencias.py:688`; `app/ponto/management/commands/reconciliar_grade.py:498`; `app/chamados/reconciliador.py:710`).

| Caminho | Consulta `fatos_do_dia` antes de criar? | O que efetivamente barra |
|---|---|---|
| `detectar_ausencias.handle` → `_abrir_chamado` (`app/ponto/management/commands/detectar_ausencias.py:439`, `:688`) | **Sim**, mas o resultado só veta `faixa == 'feriado'` (`:615`, `:616`). | Folga depende de `eh_dia_trabalho`/célula (`:143`, `:305`, `:626`); ausência é consultada no chamador (`:346`, `:805`). Não há veto geral de todas as faixas da precedência no funil. |
| Cartório → mesmo funil (`app/ponto/services/cartorio.py:1358`, `:1394`) | **Sim indiretamente**, pelo mesmo `detectar_ausencias.py:616`. | Antes disso usa protestos e ata do julgamento (`cartorio.py:1339`, `:1381`, `:1387`); não substitui o veto por feriado do funil. |
| `emitir_furo_retroativo` → mesmo funil (`app/ponto/management/commands/emitir_furo_retroativo.py:142`) | **Sim indiretamente**. | Exige `FURO_SEM_COBRANCA`, vínculo e trabalho previsto (`:109`, `:118`, `:127`), depois executa os vetos do funil. |
| `reconciliar_grade._emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:498`) | **Não nesse emissor.** | Régua e veto parcial 

[truncado em 20000 de 22228 caracteres]

## 29/10 12:25 · sessao 1b017e44d7fa

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 12:25 · sessao 1b017e44d7fa

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 12:25 · sessao 1b017e44d7fa

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 12:25 · sessao 1b017e44d7fa

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 12:25 · sessao bfa1cd7083b2

HEAD: `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. Sem escrita, testes ou banco. **Hipótese** abaixo indica caminho possível por leitura, não ocorrência demonstrada.

## T12 — Entradas do julgamento fora da impressão

A chave contém Batidas `(pk,tipo,timestamp)`, cobertura escolhida `.status`, Chamados `(pk,status_local)`, DNA `(dna_versao,marcos)`, veto de régua, quantidade de marcos vencidos, versão da regra, encerramento, quantidade de chamados com fio e classificação da cobertura parcial (`app/ponto/services/cartorio.py:323`, `:329`, `:334`, `:342`, `:352`, `:367`, `:375`). O pulo normal ocorre em `:1092`; o ramo sem adesão tem chave/pulo próprios em `:941`, `:943`.

| Entrada efetivamente lida | Está na chave? |
|---|---|
| Batidas apuráveis da janela (`app/ponto/services/cartorio.py:871`, `:1042`) | **Sim**, identidade, tipo e instante (`:324`). Retratação muda o conjunto apurável. |
| `Batida.is_intervalo`, consumida pela grade/pareador (`app/escala/utils.py:1267`; `app/ponto/turnos.py:1261`) | **Não** na tupla serializada (`app/ponto/services/cartorio.py:324`). |
| Primeira batida apurável **da vida**, que define adesão (`app/ponto/services/cartorio.py:899`, `:901`) | **Não diretamente**: a chave usa a janela local `bj` (`:1042`). `nunca_bateu` tem exceção ao pulo normal (`:1092`), mas isso não serializa a data de adesão. |
| Chamados e estados usados no julgamento (`app/ponto/services/cartorio.py:1040`, `:1069`) | **Sim**, identidade/estado (`:326`). |
| Fio vivo (`app/ponto/services/cartorio.py:1074`, `:1110`) | **Sim como quantidade**, não identidade/conteúdo das perguntas (`:1087`, `:367`). |
| Bordas/pausas do DNA da célula corrente (`app/escala/utils.py:53`, `:104`, `:1547`) | **Sim**, em `dna.marcos` (`app/ponto/services/cartorio.py:327`). |
| `CelulaDia.trabalha`, geradora e vínculo escolhido (`app/escala/utils.py:1252`, `:1255`, `:1545`; `app/ponto/supra_juiz.py:136`, `:171`) | **Não diretamente**: o hash da célula contém apenas versão/marcos do DNA (`app/ponto/services/cartorio.py:327`). |
| Vigência, atividade, âncora, ciclo, intervalo livre, defeito de folga e template (`app/escala/utils.py:1240`, `:1300`, `:1313`; `app/ponto/supra_juiz.py:175`) | **Parcial**: marcos congelados entram; os demais campos não são serializados (`app/ponto/services/cartorio.py:327`). |
| FolgaDia/foto, que sobrepõe tipo e remove faltantes/órfãs (`app/escala/utils.py:1109`, `:1118`, `:1119`) | **Não diretamente** (`app/ponto/services/cartorio.py:323`). |
| Justificativa aprovada, limpeza de faltantes e `dia.justificado` (`app/escala/utils.py:1157`, `:1200`, `:1203`) | **Não**. A ata lê essa marca para dispensar lâmpadas (`app/ponto/services/cartorio.py:602`, `:610`). |
| Ausência escolhida para cobrança, inclusive não aprovada (`app/ponto/services/cartorio.py:1041`) | **Parcial**: somente o status escolhido (`:325`), não identidade, tipo, datas, minutos ou horas. |
| Cobertura aprovada usada pela grade para produzir tipo do dia, faltantes e órfãs (`app/escala/utils.py:1148`, `:1173`, `:1180`, `:1195`) | **Parcial**: o hash recebe status da cobertura de cobrança e classificação parcial, não a linha completa da cobertura de folha (`app/ponto/services/cartorio.py:1083`, `:1088`). |
| Integralidade e posição das batidas perante ausência parcial (`app/escala/utils.py:1190`, `:1194`) | **Sim como resultado comprimido**: parcial/`sem_periodo`/`dentro`/`fora` (`app/ponto/services/cartorio.py:375`, `:1088`); horas/minutos exatos não entram. |
| Tipo usado em `lampada.dispensada` (`app/ponto/services/cartorio.py:604`, `:610`) | **Não**: duas ausências integrais aprovadas de tipos diferentes podem preservar os agregados da chave (`:325`, `:375`). |
| Catálogo TipoAusencia, incluindo efeito e parcialidade (`app/ponto/catalogo/ausencias.py:249`, `:250`; `app/escala/utils.py:1147`) | **Não diretamente**; somente resultados derivados de cobertura entram (`app/ponto/services/cartorio.py:325`, `:375`). |
| Isenção do colaborador (`app/escala/utils.py:865`; `app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:1113`) | **Não diretamente**. O veto serializado trata apenas régua degenerada/suspeita (`app/ponto/services/furos_vetados.py:19`, `:28`, `:30`). |
| Demissão usada no cadastro do dia (`app/ponto/services/cartorio.py:844`; `app/ponto/supra_juiz.py:169`) | **Não** (`app/ponto/services/cartorio.py:323`). |
| Situação e afastamento atual, controlando emissão (`app/ponto/services/cartorio.py:1033`, `:1340`, `:1381`) | **Não**; são entradas dos efeitos de emissão, não necessariamente do veredito/ata (`:323`). |
| Encerramento e teto por instante (`app/ponto/services/cartorio.py:1049`, `:1056`, `:1117`) | **Sim como agregados**: encerramento e quantidade de vencidos (`:1084`, `:1085`; serialização em `:334`, `:352`). |
| Versão declarada da regra (`app/ponto/services/cartorio.py:890`) | **Sim** (`:342`, `:1086`); isso não equivale automaticamente à versão de qualquer código alterado. |
| Ata/DNA de outras células da janela, usados pelo pré-juiz (`app/escala/utils.py:1252`, `:1519`, `:1524`) | **Não** na chave da célula corrente (`app/ponto/services/cartorio.py:323`, `:1083`). |
| Feriado/localização e precedência | **Não serializados diretamente**. Há, porém, uma diferença importante: `julgar_celula` recalcula a chave `ata.precedencia` **depois** de `julgar_colab`, mesmo que este pule (`app/ponto/services/cartorio.py:1519`, `:1560`, `:1568`). O batch chama `julgar_colab` diretamente (`app/ponto/management/commands/processar_cartorio.py:65`). |
| `TurnoMaterializado` | **Não é entrada direta encontrada**: o carregador de encerramento usa `turnos_do_colab` (`app/ponto/turnos.py:2011`, `:2013`, `:2018`). |

**Escritas e invalidação por `forcar=True`:**

| Escrita da entrada ausente | Força outro julgamento? |
|---|---|
| Justificativa aprovar/rejeitar/materializar (`app/ponto/services/justificativa.py:88`, `:170`, `:265`) | Signal chama cartório **sem force** (`app/ponto/signals.py:161`, `:165`, `:93`). |
| Ausência criar/editar/decidir/apagar (`app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`, `:1174`) | Signals de save/delete chamam **sem force** (`app/ponto/signals.py:143`, `:150`, `:93`). |
| TipoAusencia salvar/criar (`app/ponto/services/ausencia.py:1292`, `:1294`) | Receiver encontrado invalida o catálogo, não força julgamento (`app/ponto/models.py:1233`, `:1236`). |
| Registrar/retratar/apagar Batida (`app/ponto/registro_batida.py:94`, `:267`, `:285`) | Signals sem force (`app/ponto/signals.py:131`, `:138`, `:93`), mas presença/tipo/instante normalmente mudam a chave. Escritor produtivo que altere **somente** `is_intervalo`: **nao li**. |
| Flags/demissão/posto de colaborador (`app/colaboradores/services/colaborador.py:32`, `:53`, `:61`, `:68`, `:75`) | Nenhum force nesses setters. O signal de isenção silencia chamados (`app/chamados/signals.py:207`, `:217`); isso pode alterar indiretamente outro componente da chave. |
| Vigência/âncora e FolgaDia (`app/colaboradores/services/vinculo.py:799`, `:845`; `app/escala/folgas.py:172`, `:259`, `:289`) | **Condicional:** regeneração força somente datas tocadas, com `recalculo=True` e filtros de data/via humana (`app/ponto/portas/celula.py:359`, `:368`, `:371`). |
| Editar TipoEscala (`app/escala/services/cadastro_tipo.py:306`) | Regenera com `recalculo=False` (`:348`, `:350`); não alcança aquele force (`app/ponto/portas/celula.py:359`). |
| Reverter regeneração (`app/ponto/portas/celula.py:537`) | Sem chamada equivalente de force no corpo até `:551`. |
| Gravar ata/veredito (`app/ponto/portas/celula.py:650`, `:671`) | Não chama julgamento forçado nessa operação; decisão administrativa tem preservação própria (`:645`, `:646`). |
| Feriado criar/editar/excluir (`app/core/views_config.py:81`, `:68`, `:198`) | Save agenda reavaliador de chamados, não force do cartório (`app/chamados/signals.py:259`, `:269`); ausência de post_delete declarada em `:255`. |
| Alterar município/localização (`app/colaboradores/services/estrutura.py:50`, `:70`; `app/colaboradores/management/commands/seed_municipios.py:38`) | Nenhum force encontrado nessas portas. Outros chamadores: **nao li**. |

Existem forças adicionais no pós-vínculo, segunda passada enfileirada e ação explícita de cobrança; não demonstram cobertura universal das escritas acima (`app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/services/cartorio.py:746`; `app/chamados/services/cobrar_dia.py:29`).

**Podem deixar célula velha — hipóteses quando os demais componentes permanecem iguais:** justificativa decidida; alteração de tipo/conteúdo de ausência sem mudar status/parcial; catálogo de ausência; isenção/demissão; foto de folga; vínculo/template/geradora fora das condições de force; ata de outra célula da janela; primeira batida global fora de `bj`. As omissões e gatilhos estão nas linhas das duas tabelas. Para Feriado/localização, não confundir veredito/lâmpadas com `ata.precedencia`, que tem atualização separada em `julgar_celula` (`app/ponto/services/cartorio.py:1568`).

Completude recursiva de todos os métodos de modelo e chamadores de cadastro: **nao li integralmente**. Ocorrências reais de célula velha: não verificadas.

## T13 — Outros caches com chave incompleta

| Sítio / proteção | Chave e pulo | Entrada ausente / suspeita |
|---|---|---|
| **Memo de `pergunta_viva`** (`app/chamados/juizes.py:778`) | Campos da pergunta, veredito/via da célula e estado do destino (`:767`); igualdade retorna memo (`:791`, `:794`). | O cálculo lê motivo, DNA, lâmpadas e pendências (`:842`, `:849`, `:1051`, `:1064`), ausentes da chave. **Hipótese:** mesma instância reutilizada após alteração de ATA/DNA/motivo mantém resposta antiga. Cache limitado à instância/request (`:788`). |
| **Lavra S2 / seleção de perguntas stale** (`app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:31`) | Nunca lavrado, prazo expirado ou timestamps de insumos posteriores ao julgamento (`veredito_lavrado.py:137`, `:179`; `raiox.py:43`). | **Versão do código não entra.** O backfill declara que mudança de regra/código não é detectada por `esta_stale` (`app/chamados/management/commands/backfill_veredito_lavrado.py:5`). Há reparo separado por comparação com juiz atual (`:11`). |
| **`houve_fato_novo`**, gate de renascimento (`app/ponto/services/cartorio.py:249`; `app/chamados/models.py:956`) | Timestamps de Batida, Ausência, Justificativa, célula e marco vencido (`cartorio.py:272`, `:275`, `:279`, `:285`, `:287`). | **Exclusão física não deixa linha consultável. Hipótese:** exclusão posterior à decisão admin, sem outro carimbo observado, pode não ser reconhecida. Feriado/localização/vínculo também dependem de efeitos indiretos nesses carimbos. Não é cache de cálculo; pula renascimento. |

**Reaproveitamentos adjacentes, distintos de pular o cálculo do cartório:**

| Sítio | Chave | Entrada ausente / limite |
|---|---|---|
| Frescor do RetratoFolha (`app/folha/retrato_folha.py:1268`) | Compara `FechamentoMensal.atualizado_em` com `RetratoFolha.lavrado_em` (`:1296`, `:1306`). | Retrato usa retenções dependentes de células/vereditos (`:1157`; `app/folha/export.py:1188`, `:1215`). **Hipótese:** célula muda sem atualizar fechamento e o rótulo continua atualizado. O lavrador não pula montagem por isso (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`). |
| ETag do placar (`app/chamados/services/painel.py:79`) | Máximos de Chamado/Pergunta, filtros, usuário/schema e quantum de120s (`:64`, `:91`); igualdade permite304 (`app/chamados/views.py:822`). | Resposta usa PlacarNavbar e particionamento dependente do fio (`views.py:844`; `painel.py:227`). Célula/ATA e `PlacarNavbar.lavrado_em` não entram diretamente. **Hipótese limitada ao quantum:** fonte muda sem mover os máximos e há304. |

**Procurados sem outro caso de chave incompleta demonstrado:**

- **DiaPago:** monta linhas e faz `delete/bulk_create`; não compara hash de entrada para pular (`app/ponto/services/dia_pago.py:333`, `:343`, `:357`). Hash no comando é prova antes/depois (`app/ponto/management/commands/lavrar_dias_pagos.py:91`, `:132`).
- **FechamentoMensal:** invalidação por `derivado_em=None`; leitor chama derivação online (`app/ponto/services/fechamento.py:1611`, `:1655`, `:1718`).
- **Lavra de ata/veredito:** evita gravação quando a **saída** já é igual; não evita produzir o resultado (`app/ponto/portas/celula.py:643`, `:661`).
- **Espelho em lote:** hash identifica pedido e só reutiliza job em curso (`app/relatorios/lote_espelho.py:49`, `:98`).
- **Retrato de HE/teto:** apuração chama espelho; lavra usa `update_or_create` (`app/ponto/services/he_pendente_lavrado.py:92`, `:180`).

Foram buscados hashes, impressões, fingerprints, carimbos e condições de reaproveitamento em `app/`. Caches de sessão, snapshots de inteligência e demais memos fora dos sítios acima: **nao li**. Nenhuma hipótese desta seção foi reproduzida.

## T14 — Caminhos para fio morto O261

O contador exige chamado vivo com célula, módulo de furo, célula acusadora e ausência de pergunta viva; ainda exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`app/chamados/services/vigia_fio.py:184`, `:199`, `:227`, `:258`, `:261`, `:266`, `:273`, `:279`). Só então conta em `:286`.

**L** marca escrita direta na pergunta/vitalidade/veredito equivalente à lâmpada; não significa, por si, violação da regra (`CLAUDE.md:273`, `:276`).

| Caminho | Como pode produzir/conservar fio morto |
|---|---|
| **L — validar pergunta** (`app/chamados/services/validacao.py:131`, `:133`) | `validada_em` mata antes de consultar a célula acusadora (`app/chamados/juizes.py:1006`). **Hipótese:** chamado/célula permanecem e não há outro fio. A porta tenta devolver validação de resposta-hora recusada; não é falha inevitável (`validacao.py:415`, `:418`). |
| **L — carimbar `fato_ocupado`** (`app/chamados/services/materializacao.py:1042`, `:1053`) | Via de identidade materializada mata mesmo com célula acusadora (`app/chamados/juizes.py:1039`, `:1044`), sem fechar chamado/absolver célula nessa operação. **Hipótese**, condicionada aos filtros do contador. |
| **L — `paridade_recusada`** (`app/chamados/services/materializacao.py:858`, `:868`) | Também encerra por identidade (`app/chamados/juizes.py:1042`). Porém exige marco já existente no espelho antes do carimbo (`materializacao.py:840`, `:841`); não prova morte cega. Pergunta previamente validada já estava morta (`:848`). |
| **L — encerrar administrativamente disputa/perguntas** (`app/chamados/models.py:1404`, `:2036`, `:2039`) | A identidade administrativa vence a acusação (`app/chamados/juizes.py:1039`). Signal tenta fechar o pai da disputa, não incondicionalmente todos os chamados-destino (`app/chamados/signals.py:19`, `:42`). **Hipótese:** destino de furo sobrevive. |
| **L — remover motivo apagando perguntas** (`app/chamados/models.py:1519`) | Com motivos restantes, retorna sem fechar disputa/pai nem absolver célula (`:1522`, `:1524`). **Hipótese:** perguntas apagadas eram o único fio daquela célula. O evento de pergunta é `post_save`, não esse delete (`app/chamados/signals.py:185`). |
| **L — marcar duplicata** (`app/chamados/services/fio.py:844`, `:848`) | Mata a excedente por identidade (`app/chamados/juizes.py:1040`). **Não produz sozinho** se a canônica permanece viva na mesma célula; a migração escolhe canônica entre vivas (`fio.py:882`, `:890`). |
| **Abrir `batida_ausente` antes de produzir perguntas** (`app/ponto/management/commands/detectar_ausencias.py:688`) | Etapa de fio é posterior e best-effort (`:777`, `:791`, `:792`). Falha ou ausência de órfãos elegíveis não desfaz nascimento. **Hipótese de persistência**, pois cartório/outros eventos ainda podem reparar. |
| **Abrir/reusar `turno_aberto`** (`app/ponto/management/commands/processar_alertas_turno.py:318`) | Perguntas só são tentadas se `criado`, em `try/except` (`:355`, `:367`, `:368`). Reuso não executa essa etapa. **Hipótese**, não consequência inevitável. |
| **Grade emite sem pergunta** (`app/ponto/management/commands/reconciliar_grade.py:498`) | Abre/reabre e retorna sem gerar perguntas (`:519`, `:520`). Caminho manual executável; o cron não passa `--apply` (`app/config/crons.py:436`). |
| **View cria bloqueio de turno** (`app/ponto/views.py:290`) | Ramo cria `entrada_bloqueada_turno_aberto` sem pergunta (`:298`, `:301`). O módulo pertence ao vocabulário (`app/chamados/catalogo/furo_modulos.py:15`), mas O261 ainda exige célula associada e acusadora; ocorrência dessas condições não provada. |
| **Pergunta recusada por ausência de ata legível** (`app/chamados/services/disputa_emissao.py:833`, `:842`) | Chamado pode existir antes; posterior célula acusadora pode encontrá-lo sem fio. **Hipótese**, sujeita à fábrica do julgamento seguinte. |
| **L — célula volta a acusar após pergunta morta** (`app/ponto/services/cartorio.py:1409`; `app/ponto/portas/celula.py:652`, `:671`) | Validação e identidades terminadoras continuam vencendo a acusação (`app/chamados/juizes.py:1006`, `:1044`). Para outras mortes, acusação posterior pode tornar pergunta viva novamente (`:1055`, `:1057`). |
| **Fábrica preserva identidade morta** (`app/chamados/services/fabrica_da_celula.py:93`, `:95`) | Dedup considera perguntas existentes não duplicatas, não apenas vivas. O gate reencontra o fato sem limpar validação/materialização (`app/chamados/models.py:1966`, `:1988`). Pode conservar a falta de fio após relavra acusadora. |

**Reparos existentes têm recortes:**

- Beco por evento exige pai já resolvido, não o chamado vivo do O261 (`app/chamados/services/becos.py:31`).
- Ressuscitação do cartório ignora chamado sem perguntas e perguntas com texto (`app/ponto/services/cartorio.py:1261`, `:1263`).
- `ressuscitar_perguntas` seleciona materializadas sem texto; validada sem materialização não entra (`app/chamados/models.py:1028`, `:1031`).
- Fábrica por evento só alcança célula julgada; impressão igual pode impedir esse caminho (`app/ponto/services/cartorio.py:1092`, `:769`, `:770`).

Não tratar como produtor automático a absolvição por célula: ela exige não acusação/ausência de pendências e pode ser desfeita pela acusação posterior (`app/chamados/reconciliador.py:529`, `:538`; `app/chamados/juizes.py:1055`). Tranca também tenta encerrar chamados, além de matar perguntas (`app/chamados/services/competencia_trancada.py:174`, `:177`).

Todos os escritores fora das portas e caminhos citados: **nao li integralmente**. Ocorrências/quantidades de produção: **nao li**.

## T15 — Emissor de `batida_ausente` pergunta à precedência?

Os nascimentos diretos encontrados são o funil de ausência, o emissor da grade e o reemissor C2 (`app/ponto/management/commands/detectar_ausencias.py:688`; `app/ponto/management/commands/reconciliar_grade.py:498`; `app/chamados/reconciliador.py:710`).

| Caminho | Consulta `fatos_do_dia` antes de criar? | O que efetivamente barra |
|---|---|---|
| `detectar_ausencias.handle` → `_abrir_chamado` (`app/ponto/management/commands/detectar_ausencias.py:439`, `:688`) | **Sim**, mas o resultado só veta `faixa == 'feriado'` (`:615`, `:616`). | Folga depende de `eh_dia_trabalho`/célula (`:143`, `:305`, `:626`); ausência é consultada no chamador (`:346`, `:805`). Não há veto geral de todas as faixas da precedência no funil. |
| Cartório → mesmo funil (`app/ponto/services/cartorio.py:1358`, `:1394`) | **Sim indiretamente**, pelo mesmo `detectar_ausencias.py:616`. | Antes disso usa protestos e ata do julgamento (`cartorio.py:1339`, `:1381`, `:1387`); não substitui o veto por feriado do funil. |
| `emitir_furo_retroativo` → mesmo funil (`app/ponto/management/commands/emitir_furo_retroativo.py:142`) | **Sim indiretamente**. | Exige `FURO_SEM_COBRANCA`, vínculo e trabalho previsto (`:109`, `:118`, `:127`), depois executa os vetos do funil. |
| `reconciliar_grade._emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:498`) | **Não nesse emissor.** | Régua e veto parcial 

[truncado em 20000 de 22228 caracteres]

## 29/10 12:25 · sessao bfa1cd7083b2

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 12:25 · sessao bfa1cd7083b2

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 12:25 · sessao bfa1cd7083b2

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 12:25 · sessao bfa1cd7083b2

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

_53 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
