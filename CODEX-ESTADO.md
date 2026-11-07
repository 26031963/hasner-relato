# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 00:21** · publicado em **07/11/2026 00:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 00:21 · sessao 10bc0fe98df9 · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: bloqueia considerar o commit como E1 concluída; o patch do índice também precisa de correções nos caminhos que alteram a identidade.** Há decisões sem linha, perda da resposta legada, absorção sem atomicidade completa e inversão de travas. A ausência do índice no commit é deliberada e foi tratada como unidade separada. (app/chamados/models.py:1508; app/chamados/models.py:2658; app/chamados/services/disputa_emissao.py:1351; app/chamados/services/materializacao.py:550; app/chamados/services/disputa_emissao.py:1129; app/chamados/migrations/0051_decisao_pergunta.py:3)

Resultados executados de testes, conteúdo do acervo, colisões reais, histórico de migrações em produção e igualdade dos hashes FM/TXT 09–10: **[não verificado]**. [nao verificado]

1. **Campos, escritores, leitores e significados**

   O escritor físico da sequência é `registrar → bulk_create`. Os produtores dos valores são os atos abaixo; portanto “uma porta de INSERT” não significa “todos os escritores da projeção registram decisões”. (app/chamados/models.py:3364; app/chamados/models.py:2658)

   | Produtor | Campos de `DecisaoPergunta` fornecidos |
   |---|---|
   | Encerramento da disputa | `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo` — app/chamados/models.py:1862 |
   | Remoção do motivo | Os cinco anteriores — app/chamados/models.py:1982 |
   | Resposta do colaborador | `ato`, `ato_em`, `ato_por`, `ato_texto` — app/chamados/services/disputa_emissao.py:1351 |
   | Reabertura/recusa | Os cinco primeiros e `validada_original_por` — app/chamados/services/disputa_emissao.py:1460 |
   | Resposta automática | `ato`, `ato_em`, `ato_texto`, `ato_motivo`; autor fica nulo — app/chamados/services/disputa_emissao.py:1493 |
   | Recuperação pela trilha | Os mesmos da automática — app/chamados/services/disputa_emissao.py:1514 |
   | Validação | `ato`, `ato_em`, `ato_por`, `ato_texto` — app/chamados/services/validacao.py:138 |
   | Devolução | Os cinco primeiros — app/chamados/services/validacao.py:203 |
   | Absorção | `ato`, `ato_em`, `ato_por`, `ato_texto`, `absorcao_escape`, `absorcao_ausencia` — app/chamados/services/materializacao.py:555 |

   | Campo | Leitores da sequência fora dos testes | [nao verificado]
   |---|---|
   | `ato` | Seleção do histórico: app/chamados/models.py:2961,2965; ausência de decisão no DRY: app/chamados/management/commands/backfill_decisoes_pergunta.py:31; apresentação: app/templates/chamados/partials/modal_fio.html:208,211; checks: app/chamados/models.py:3344,3349 |
   | `ato_em` | Ordenação: app/chamados/models.py:3341; apresentação: app/templates/chamados/partials/modal_fio.html:209,211 |
   | `ato_por` | Carregamento: app/chamados/services/fio.py:232; apresentação: app/templates/chamados/partials/modal_fio.html:211 |
   | `ato_texto` | Apresentação: app/templates/chamados/partials/modal_fio.html:209,211 |
   | `ato_motivo` | Apresentação: app/templates/chamados/partials/modal_fio.html:211 |
   | `validada_original_por` | Carregamento: app/chamados/services/fio.py:232; apresentação: app/templates/chamados/partials/modal_fio.html:211; check: app/chamados/models.py:3351 |
   | `absorcao_escape`, `absorcao_ausencia` | Checks: app/chamados/models.py:3344,3346. O leitor de negócio continua reconstruindo a absorção pelos carimbos: app/chamados/services/materializacao.py:595,615,619 |

   A migração também escreve esses campos: renomeia texto/instante/autor/motivo anteriores, atribui `ato='reabertura'` às linhas antigas e introduz as ligações de absorção nulas. A fotografia anterior do validador já existia na 0050. (app/chamados/migrations/0051_decisao_pergunta.py:41,49,67; app/chamados/migrations/0050_resposta_anterior.py:26)

   | Campo novo de `PerguntaDisputa` | Escritores | Leitores |
   |---|---|---|
   | `colaborador` | Derivação na inserção: app/chamados/models.py:2923,2935; preenchimento do acervo: app/chamados/migrations/0051_decisao_pergunta.py:25; fixture direta: app/chamados/tests/test_e1_entradas.py:313 | Dedup: app/chamados/models.py:2430; conferência de nascimento: app/chamados/models.py:2934,2936,2938; DRY: app/chamados/management/commands/backfill_decisoes_pergunta.py:34,36,54; teste: app/chamados/tests/test_e1_entradas.py:174 |
   | `instancia_marco` | Default e derivação: app/chamados/models.py:2241,2941; migração: app/chamados/migrations/0051_decisao_pergunta.py:27,96; fixture: app/chamados/tests/test_e1_entradas.py:314 | Dedup/derivação: app/chamados/models.py:2431,2939; DRY: app/chamados/management/commands/backfill_decisoes_pergunta.py:54; teste: app/chamados/tests/test_e1_entradas.py:175 |
   | `conversa` | Default: app/chamados/models.py:2242; portas e INSERT: app/chamados/models.py:2450,2461,2546; todo o legado recebe `False`: app/chamados/migrations/0051_decisao_pergunta.py:99 | Trava/dedup/derivação: app/chamados/models.py:2474,2495,2939; DRY: app/chamados/management/commands/backfill_decisoes_pergunta.py:55; testes: app/chamados/tests/test_e1_entradas.py:176,182 |

   A fotografia genérica dos testes lê todos os campos dos dois modelos. O índice anexado acrescenta leitura de `colaborador`, `data_evento`, `motivo`, `instancia_marco`, `conversa` e `via_resolucao`. (app/chamados/tests/test_e1_entradas.py:281,282; app/chamados/migrations/0052_pergunta_unica_por_fato.py:19,20 — anexo)

   **Não há um significado uniforme para todas as projeções:**

   - `respondida_em` pode ser o instante da resposta recebida, um instante histórico recuperado da trilha ou um carimbo automático colocado pelo encerramento; `validada_em` pode representar validação do admin ou preenchimento automático por `carimbar(resposta='validar')`. **Fila**, porque os leitores que tratam presença do carimbo como ato humano não distinguem essas origens. Contraexemplo: encerramento por fato preenche `validada_em` sem passar por `_carimbar_validacao`. (app/chamados/services/disputa_emissao.py:1325,1511,1681; app/chamados/models.py:2658,2661; app/chamados/services/validacao.py:133)
   - `materializada_em` significa tanto efeito aplicado quanto encerramento sem efeito. **Fila:** pergunta de hora respondida e depois retratada ganha carimbo sem exigir batida, mas `estado_aplicacao_pergunta` devolve `aplicada`, excetuando apenas paridade. (app/chamados/models.py:1987; app/chamados/services/materializacao.py:434,438,442; app/chamados/services/fio.py:336)
   - `conversa=False` significa nascimento pela porta de fato nas linhas novas e simplesmente default da migração nas antigas. **Bloqueia usar a coluna como classificação histórica comprovada.** Duas conversas antigas do mesmo motivo/dia passam a parecer duas linhas do fato. (app/chamados/models.py:2450,2461; app/chamados/migrations/0051_decisao_pergunta.py:4,99)
   - `instancia_marco=''` reúne motivos sem marco e conversas; não identifica ocorrência de batida, pausa ou episódio. A própria derivação declara que duas pausas ainda não são separadas. (app/chamados/services/declaracao_texto.py:47,52,57; app/chamados/models.py:2939)
   - Os carimbos são `DateTimeField`, com configuração de fuso `America/Sao_Paulo` e `USE_TZ=True`; `data_evento` é uma data. Não encontrei uma troca explícita de unidade nesses campos novos. Validade de todos os instantes históricos recebidos em execução: **[não verificado]**. (app/chamados/models.py:2230,2249,2250,3325; app/config/settings/base.py:93,95)
   - O contexto conserva a distinção dia civil/dia do turno: conversa nasce pela sua própria porta, enquanto o rechaveamento existe justamente para retirar do acervo o segundo significado “dia da saída”. A migração 0051 não corrige esse acervo. (app/chamados/models.py:2453; app/chamados/services/fio.py:862; app/chamados/migrations/0051_decisao_pergunta.py:25)

2. **Há caminhos de produção sem linha em `DecisaoPergunta`**

   | Escritor persistente | Campos afetados | Registra linha? |
   |---|---|---|
   | Resposta normal | Texto, resposta, eventual limpeza da validação e descarimbo | **Sim**, apenas a resposta nova — app/chamados/services/disputa_emissao.py:1326,1339,1344,1348,1351 |
   | Reabertura/recusa | Texto, resposta e validação; opcionalmente materialização/via | **Condicional:** só se houver texto ou `respondida_em` — app/chamados/services/disputa_emissao.py:1444,1460,1466,1468,1470 |
   | Resposta automática | Texto e resposta | **Sim** — app/chamados/services/disputa_emissao.py:1489,1493 |
   | Carimbo pela trilha | Resposta | **Sim** — app/chamados/services/disputa_emissao.py:1511,1514 |
   | `_carimbar_validacao` | Validação | **Sim** — app/chamados/services/validacao.py:133,138 |
   | `devolver_validacao_recusada` | Limpa validação | **Sim**, quando havia validação — app/chamados/services/validacao.py:194,197,203 |
   | `carimbar` | Materialização/via e, opcionalmente, resposta/validação | **Não na porta** — app/chamados/models.py:2655,2658,2661,2678 |
   | `carimbar_identidade` | Materialização/via | **Não na porta** — app/chamados/models.py:2705,2708,2713 |
   | `descarimbar` | Limpa materialização/via | **Não** — app/chamados/models.py:2717,2721 |
   | `descarimbar_em_lote` | Limpa materialização/via e campos adicionais | **Não** — app/chamados/models.py:2768 |
   | `encerrar_por_fato_no_espelho` | Troca via | **Não** — app/chamados/models.py:2778 |
   | `relavrar` | Troca via | **Não**; registra `evento`, outra trilha — app/chamados/services/relavra_via.py:123,130 |

   Os chamadores de carimbo também precisam ser diferenciados:

   | Caminho | Linha correspondente |
   |---|---|
   | Encerramento da disputa por admin/desligamento | `SILENCIO` — app/chamados/models.py:1859,1862 |
   | Remoção de motivo | `RETRATACAO` — app/chamados/models.py:1982,1987 |
   | Absorção | `ABSORCAO`, após carimbar — app/chamados/services/materializacao.py:550,555 |
   | Resolver o dia | **Nenhuma** para a pergunta encerrada — app/ponto/services/veredito_celula.py:174,177 |
   | Aceite de folga | Há `VALIDACAO`; o fechamento adicional não tem linha própria — app/chamados/services/validacao.py:279,280,306,315 |
   | Duplicata | **Nenhuma** — app/chamados/services/fio.py:952 |
   | Paridade/fato ocupado | **Nenhuma** — app/chamados/services/materializacao.py:909,1084 |
   | Sem turno/célula absolveu | **Nenhuma** — app/chamados/reconciliador.py:405,586 |
   | Materialização normal | **Nenhuma na chamada de carimbo**, inclusive validação automática — app/chamados/services/materializacao.py:1262 |
   | Pendentes aprovadas pelo juiz | **Nenhuma** — app/chamados/services/materializacao.py:1416 |
   | Encerramento pelo fato/sem credor | **Nenhuma**, inclusive carimbo automático de resposta/validação — app/chamados/services/disputa_emissao.py:1681,1755 |
   | Reabertura por competência/fantasma | **Nenhuma** — app/chamados/services/competencia_trancada.py:254; app/chamados/management/commands/reabrir_orfas_enterradas.py:147 |
   | Desfazer carimbo sem lastro | **Nenhuma** — app/chamados/services/relavra_via.py:231 |
   | Encerrar por fato no espelho | **Nenhuma** — app/chamados/management/commands/encerrar_perguntas_com_fato.py:68 |

   **Bloqueia — perda do legado no primeiro uso.** Contraexemplo: pergunta com `08:00`, `respondida_em` antigo e nenhuma decisão recebe `09:00`. O código sobrescreve a projeção e insere somente `09:00`; `respostas_anteriores` devolve lista vazia. O DRY não preserva previamente `08:00`. (app/chamados/services/disputa_emissao.py:1326,1351; app/chamados/models.py:2960; app/chamados/management/commands/backfill_decisoes_pergunta.py:33)

   **Bloqueia — reabertura sem decisão.** Contraexemplo: irmã muda absorvida, com decisão `ABSORCAO`, é devolvida após rejeição da ausência. O filtro que produz decisões ignora a irmã sem texto e sem resposta, mas o `UPDATE` remove seu carimbo. O mesmo predicado deixa passar validação sem texto/resposta sem fotografá-la. (app/chamados/services/materializacao.py:668; app/chamados/services/disputa_emissao.py:1466,1468)

   **Bloqueia — ressuscitação apaga decisões projetadas.** Limpa `respondida_em`, `validada_em`, materialização e via, sem inserir decisão; a trilha existente é `LogAuditoria`. (app/chamados/models.py:1508,1522,2768)

   **Bloqueia — absorção pode ficar sem sua linha.** No caminho web/API, a transação da resposta termina antes da absorção. `q.carimbar` salva em seu próprio atomic e o INSERT da decisão vem depois. Contraexemplo: falha de `bulk_create` após o carimbo deixa a pergunta materializada sem `ABSORCAO`. (app/chamados/views.py:1283; app/api/views.py:2659; app/chamados/services/disputa_emissao.py:1347,1381; app/chamados/services/materializacao.py:550,555; app/chamados/models.py:2925)

   **Fila — autoria disponível é descartada.** A reabertura pelo colaborador conhece `autor`, mas chama a porta sem `por`; a devolução da ausência conhece motivo, mas também não o transmite. (app/chamados/models.py:2146,2158; app/chamados/services/materializacao.py:665,668)

   Não localizei chamada explícita de produção que apague diretamente `PerguntaDisputa` ou `DisputaSupervisao`. A FK disputa→pergunta é `CASCADE`; pergunta com decisão, pergunta usada como escape e ausência usada na absorção são protegidas. Assim, apagar a disputa pode apagar perguntas **sem decisões**, enquanto a existência de decisão impede o apagamento correspondente. (app/chamados/models.py:2223,3323,3333,3335)

   A imutabilidade não é integral: autores usam `SET_NULL`. Exclusão de usuário pode alterar autoria histórica sem passar pelos métodos bloqueados; ocorrência desse fluxo em produção: **[não verificado]**. (app/chamados/models.py:3326,3331,3355)

3. **Corrida e ordem das travas**

   **Entre dois nascimentos datados que passam por `_nascer`, a trava serializa o dedup.** Ela é adquirida antes de `do_fato`; dentro desse recorte, o segundo ato consulta depois do primeiro. A conversa não adquire essa trava. (app/chamados/models.py:2473,2475,2495,2498)

   **A trava sozinha não garante unicidade da árvore.** Rechaveamento, datação posterior e descarimbo não participam da mesma trava; uma criação direta também passa apenas pela derivação de identidade do `save`. O teste do próprio DRY constrói uma segunda linha diretamente. (app/chamados/services/fio.py:836,887,893; app/chamados/models.py:2768,2923; app/chamados/tests/test_e1_entradas.py:313)

   **Com o patch, o banco impede a segunda identidade indexada, mas os caminhos concorrentes passam a poder levantar `IntegrityError` sem recuperação.** Conversas e vias fora do fato são excluídas; datas nulas continuam fora de uma garantia de igualdade entre chaves nulas. (app/chamados/migrations/0052_pergunta_unica_por_fato.py:19,20 — anexo; app/chamados/services/fio.py:851,904; app/chamados/models.py:2230)

   **Bloqueia — ciclo de travas concreto, nos dois pacotes:**

   - A declaração abre transação, trava `DisputaSupervisao` e depois pede `Colaborador`. (app/chamados/services/declaracao_texto.py:312,320,336; app/chamados/services/disputa_emissao.py:501; app/chamados/models.py:2475)
   - O agrupador de órfãos pode ler a disputa sem trava, abrir pergunta mantendo `Colaborador` e depois atualizar a mesma disputa. (app/chamados/services/disputa_emissao.py:934,971,1088,1129)
   - Intercalação: A segura Disputa e espera Colaborador; B segura Colaborador e espera Disputa. O comentário anterior já reconhece a ordem Disputa→Colaborador; o novo comentário “colaborador primeiro” não descreve a ordem global. (app/chamados/models.py:847,2470)

   Mudança posterior de motivo, instância, conversa ou colaborador não é impedida pelo `save`: a conferência só ocorre na inserção, e instância explicitamente não vazia é aceita sem confronto com o motivo. Não localizei escritor de produção que faça essas mudanças posteriores; existência de tal caminho dinâmico: **[não verificado]**. (app/chamados/models.py:2923,2934,2939)

4. **Migrações 0051 e 0052**

   **0051: compatível estruturalmente com o legado descrito; segurança no acervo real [não verificado].** O rename preserva as linhas, o novo ato recebe `reabertura`, a foto antiga continua permitida e as ligações de absorção nascem nulas. O `SET CONSTRAINTS ALL IMMEDIATE` está antes do `ALTER ... NOT NULL`, tratando os eventos diferidos produzidos pelo preenchimento da FK. (app/chamados/migrations/0051_decisao_pergunta.py:41,49,67,85,101,104)

   **Fila — a migração não identifica conversas antigas nem corrige o dia antigo.** Contraexemplo: duas conversas históricas iguais em colaborador/dia/motivo tornam-se colisão do índice, porque ambas recebem `conversa=False`. (app/chamados/migrations/0051_decisao_pergunta.py:4,25,99)

   **O congelado bate com o vivo:** os quatro pares são `saida_sem_entrada→inicio`, `orfao_14h→fim`, `intervalo_saida→int_ida`, `intervalo_volta→int_volta`; há comparação direta em teste. (app/chamados/migrations/0051_decisao_pergunta.py:14; app/chamados/services/declaracao_texto.py:42,52; app/chamados/tests/test_e1_entradas.py:259)

   **0052: bloqueia aplicar como está sem resolver os caminhos do item 8 e classificar o acervo.** Zero colisões no DRY não foi fornecido. Além disso, o DRY agrupa `data_evento=NULL`, enquanto o índice não torna essas chaves nulas iguais. Uma colisão criada entre DRY e DDL faria a instalação do índice falhar; não seria corrupção silenciosa. (app/chamados/management/commands/backfill_decisoes_pergunta.py:54,56; app/chamados/migrations/0052_pergunta_unica_por_fato.py:19,20 — anexo; **[não verificado]** quanto ao acervo)

5. **`backfill_decisoes_pergunta`**

   **Não escreve modelos e lê a autoridade correta.** Faz consultas e saída em `stdout`, sem opção de apply; consulta `folha.export.competencia_entregue`, autoridade `_K6`, que considera o TXT vigente não invalidado. A competência do fato vem do adaptador existente. (app/chamados/management/commands/backfill_decisoes_pergunta.py:25,51,67,71,76; app/core/juizes.py:312; app/folha/export.py:580; app/ponto/services/fechamento.py:1595)

   **Fila — não suporta todo o domínio permitido pelo modelo.** Contraexemplo: pergunta legada respondida sem `data_evento` chega a `competencia_do_fato(None)` e à comparação `None < None`; duplicatas sem data e sem resposta chegam a `dia.isoformat()`. Presença real dessas linhas: **[não verificado]**. (app/chamados/models.py:2230; app/chamados/management/commands/backfill_decisoes_pergunta.py:45,90; app/ponto/services/fechamento.py:1588,1590)

   O DRY mede ausência de qualquer ato daquele tipo, não se a decisão existente corresponde à projeção vigente. Portanto uma primeira resposta E1 já presente faz o DRY deixar de apontar a resposta legada sobrescrita do item 2. (app/chamados/management/commands/backfill_decisoes_pergunta.py:31,33; app/chamados/services/disputa_emissao.py:1351)

6. **Classes de falha solicitadas**

   | Classe | Veredito e contraexemplo |
   |---|---|
   | Segundo juiz registrado | **Não caracterizado nas adições.** A instância inverte o catálogo; competência chama o juiz existente. O espelho SQL é declarado como continente, não substituto exato do C6. (app/chamados/services/declaracao_texto.py:52; app/core/juizes.py:312; app/chamados/models.py:2842) |
   | Segundo escritor fora da porta | **Bloqueia.** `carimbar` escreve validação fora do escritor indicado em `JUIZES.md` e não cria decisão. Contraexemplo: encerramento por fato com `resposta='validar'`. (app/docs/JUIZES.md:38; app/chamados/models.py:2661; app/chamados/services/disputa_emissao.py:1681) |
   | Relógio fora da borda | **Fila.** Há leituras dentro dos atos, sem instante recebido por parâmetro, inclusive duas leituras na mesma retratação para contexto e decisão. Não apareceu novo juiz puro lendo relógio nesta mudança. (app/chamados/models.py:1965,1978; app/chamados/services/validacao.py:133,203) |
   | Derivação paralela da fonte única | **Bloqueia.** O lote de declaração não aplica o novo domínio de `do_fato`; uma retratada validada bloqueia o formulário no lote, enquanto a consulta avulsa permite. (app/chamados/services/declaracao_texto.py:434,436,546; app/chamados/models.py:2432) |
   | Selo/RE

[truncado em 20000 de 49082 caracteres]

## 07/11 00:14 · sessao 70b6448f994a · - · - · origem=Ronald (tmux)

Achados confirmados e referências já enviados ao agente principal:

- **Bloqueia:** resposta nova de pergunta legada sem decisões guarda apenas o texto novo; a antiga desaparece da fonte histórica. `app/chamados/services/disputa_emissao.py:1326`, `app/chamados/services/disputa_emissao.py:1351`, `app/chamados/models.py:2960`.
- **Bloqueia:** `carimbar` escreve resposta/validação sem linhas correspondentes; existem chamadores de produção. `app/chamados/models.py:2658`, `app/chamados/models.py:2661`, `app/chamados/services/disputa_emissao.py:1681`, `app/chamados/services/materializacao.py:1262`.
- **Bloqueia:** ressurreição limpa carimbos e validação sem decisão; reabertura da irmã muda absorvida também não registra o ato. `app/chamados/models.py:1508`, `app/chamados/models.py:2768`, `app/chamados/services/disputa_emissao.py:1466`, `app/chamados/services/materializacao.py:668`.
- **Bloqueia:** absorção pelo núcleo da resposta salva o carimbo antes de registrar a ligação, sem transação envolvendo ambos. Falha no registro deixa carimbo persistido sem `ABSORCAO`. `app/chamados/services/disputa_emissao.py:1381`, `app/chamados/services/materializacao.py:550`, `app/chamados/services/materializacao.py:555`.
- **Bloqueia:** Resolver dia encerra perguntas sem registrar `SILENCIO`. `app/ponto/services/veredito_celula.py:174`, `app/ponto/services/veredito_celula.py:177`, `app/chamados/models.py:2591`.
- **Fila:** `materializada_em` significa tanto efeito aplicado quanto encerramento sem efeito. Uma pergunta de hora retratada pode aparecer como aplicada. `app/chamados/services/materializacao.py:434`, `app/chamados/services/materializacao.py:442`, `app/chamados/services/fio.py:336`.
- **Fila:** reabertura pelo colaborador e devolução por ausência rejeitada conhecem autor/motivo, mas não os passam à decisão. `app/chamados/models.py:2158`, `app/chamados/services/materializacao.py:665`, `app/chamados/services/materializacao.py:668`.
- **Limite da imutabilidade:** autoria usa `SET_NULL`; exclusão do usuário pode modificar uma decisão histórica. Existência de tal exclusão em produção não foi verificada. `app/chamados/models.py:3326`, `app/chamados/models.py:3331`.
- Não encontrei chamada explícita de produção a `PerguntaDisputa.delete()` ou `DisputaSupervisao.delete()`. A relação disputa→pergunta usa `CASCADE`, mas pergunta com decisão recebe proteção por `PROTECT`; colaborador também é protegido pela coluna nova. `app/chamados/models.py:2223`, `app/chamados/models.py:2240`, `app/chamados/models.py:3323`.

Enviei inventário de referências de produção, incluindo comandos e templates, para os cinco campos de projeção e todos os campos de `DecisaoPergunta`. Os campos `absorcao_escape` e `absorcao_ausencia` ainda não têm leitor de negócio; o leitor da absorção continua reconstruindo o grupo pelo carimbo. `app/chamados/services/materializacao.py:555`, `app/chamados/services/materializacao.py:595`, `app/chamados/services/materializacao.py:619`.

## 07/11 00:13 · sessao 01b79c614c78 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos por inspeção estática; ocorrência em produção e cardinalidades do acervo permanecem [não verificado].

1. **Bloqueia — ordem de trava invertida, com ciclo concreto.** `declarar_hora_colab` abre transação, trava a disputa por `_abrir_ou_agregar_disputa` e depois pede a trava do colaborador ao abrir pergunta (`app/chamados/services/declaracao_texto.py:312`, `app/chamados/services/declaracao_texto.py:320`, `app/chamados/services/declaracao_texto.py:336`, `app/chamados/services/disputa_emissao.py:501`, `app/chamados/models.py:2475`). O agrupador de órfãos abre uma transação maior, lê disputa existente sem trava, abre pergunta — mantendo a trava do colaborador — e depois salva os motivos da disputa (`app/chamados/services/disputa_emissao.py:934`, `app/chamados/services/disputa_emissao.py:971`, `app/chamados/services/disputa_emissao.py:1088`, `app/chamados/services/disputa_emissao.py:1129`). Contraexemplo: A segura Disputa e espera Colaborador; B segura Colaborador e espera o UPDATE da mesma Disputa. O comentário anterior já declara Disputa→Colaborador, enquanto `_nascer` agora anuncia Colaborador primeiro (`app/chamados/models.py:847`, `app/chamados/models.py:2470`). O índice não corrige isso.

2. **Bloqueia — segunda derivação da identidade divergiu da fonte.** O leitor avulso da projeção usa `do_fato`, que agora exclui duplicata/retratada e exige instância; o lote do fio/API continua agrupando por colaborador da disputa, motivo e data, incluindo as linhas retiradas do fato (`app/chamados/models.py:2430`, `app/chamados/services/declaracao_texto.py:433`, `app/chamados/services/declaracao_texto.py:546`). A escolha final aceita a última linha não reutilizável e consulta sua validação (`app/chamados/services/declaracao_texto.py:251`, `app/chamados/services/declaracao_texto.py:266`). Contraexemplo: única pergunta antiga, validada e depois retratada, para um marco atualmente faltante. Consulta avulsa recebe lista vazia e permite declarar; o lote inclui a retratada validada e recusa/esconde o formulário (`app/chamados/services/declaracao_texto.py:434`, `app/chamados/services/declaracao_texto.py:436`, `app/chamados/services/declaracao_texto.py:438`). O índice não cura.

3. **Bloqueia para o patch — `datou` contorna a consulta de colisão.** `reapontar_resolvedora` data uma pergunta sem data e salva sem consultar `do_fato` nem travar Colaborador (`app/chamados/services/fio.py:834`, `app/chamados/services/fio.py:850`). Seus gatilhos existentes incluem reemissão, histórico e derivação de destino (`app/chamados/reconciliador.py:944`, `app/chamados/reconciliador.py:1628`, `app/chamados/reconciliador.py:1641`). Contraexemplo: p1 sem data e p2 já no destino `(colab,D,motivo,instância)`; datar p1 em D cria segunda linha sem índice, ou levanta `IntegrityError` sem tratamento com o patch. O acervo atual contendo p1 é [não verificado].

4. **Bloqueia para o patch — reentrada de retratada/duplicata pelo descarimbo.** `_universo_da_ressuscitacao` seleciona materializadas sem texto, sem excluir vias de identidade; limpa a via na cópia ANTES de perguntar ao C6 (`app/chamados/models.py:1438`, `app/chamados/models.py:1459`). A porta depois limpa os campos reais em lote (`app/chamados/models.py:1508`, `app/chamados/models.py:2770`). Contraexemplo: p1 muda retratada; o fato volta e p2 nasce; cartório/reabertura ressuscita p1 porque seu marco está apagado. Sem índice, p1 e p2 voltam ao mesmo fato; com índice, o UPDATE falha. Gatilhos de produção: cartório, reabertura e desarquivamento (`app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1406`, `app/chamados/views_arquivar.py:86`).

5. **Fila/bloqueia conforme promessa — `rechavear` ainda tem corrida fora da trava comum.** Trava apenas a própria pergunta e consulta o destino antes de salvar; não participa da trava Colaborador de `_nascer` (`app/chamados/services/fio.py:886`, `app/chamados/services/fio.py:893`, `app/chamados/services/fio.py:905`). Contraexemplo: rechaveamento verifica destino vazio; nascimento simultâneo cria ali; rechaveamento salva depois. Sem índice duplica; com índice impede duplicação por erro sem recuperação. Dois rechaveamentos de perguntas distintas para o mesmo destino têm o mesmo problema (`app/chamados/services/fio.py:887`, `app/chamados/services/fio.py:893`).

6. **Bloqueia — conversa e fato ainda se misturam em `do_fato`.** O índice e o DRY excluem `conversa=True`, mas `do_fato` não filtra conversa (`app/chamados/models.py:2430`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; patch fornecido `e1_indice_0052.patch:19`). Contraexemplo: conversa administrativa de `posto_desativado`, instância `''`, no dia D; emissão datada posterior do mesmo motivo/D devolve a conversa como pergunta existente (`app/chamados/services/acoes_disputa.py:247`, `app/chamados/services/acoes_disputa.py:260`, `app/chamados/models.py:2498`). O problema independe do índice.

7. **Fila para o modelo; bloqueia aplicar índice sem classificar acervo — instância vazia não significa necessariamente “dia inteiro”.** Somente quatro motivos têm instância: entrada, saída e os dois lados do intervalo (`app/chamados/services/declaracao_texto.py:42`, `app/chamados/services/declaracao_texto.py:57`). Fora deles, catálogo inclui `sem_posto`, `em_ferias`, `posto_desativado`, `colisao_escala`, `ordem_substituicao_invalida`, `batida_fora_escala_grave`, `gps_ausente`, `foto_ausente`, `gps_mock`, `device_comprometido`, `liveness_indisponivel`, `ancora_ausente` (`app/chamados/catalogo/perguntas.py:36`, `app/chamados/catalogo/perguntas.py:118`). Foto, mock, device e liveness são anomalias de BATIDA: duas batidas do mesmo turno podem apresentar duas ocorrências legítimas do mesmo motivo (`app/api/views.py:479`, `app/api/views.py:488`, `app/api/views.py:504`, `app/api/views.py:519`). O contexto carrega o ID da batida e a data do emissor vem dessa batida, mas essa ocorrência não entra na identidade (`app/api/views.py:788`, `app/chamados/services/disputa_emissao.py:379`). O dedup atual já colapsa esses fatos; o índice cristaliza a limitação (`app/chamados/models.py:2498`). Conversas legítimas antigas também recebem `conversa=False` na migração, embora reabertura administrativa permita qualquer motivo e crie conversa nova (`app/chamados/migrations/0051_decisao_pergunta.py:99`, `app/chamados/services/acoes_disputa.py:247`, `app/chamados/services/acoes_disputa.py:260`).

8. **Mudança de continência: correta isoladamente.** C6 e SQL passam a consultar a mesma tupla, com materialização presente e antes da resposta; portanto a exclusão nova não esconde pergunta considerada viva por esse degrau do C6 (`app/chamados/juizes.py:1147`, `app/chamados/models.py:2855`, `app/chamados/models.py:2862`). Isso não constitui prova executada de toda a continência da árvore [não verificado].

9. **Outros leitores alterados por retratada/identidade.**
   - Materialização de fato passa a aceitar retratada sem testemunha de batida, por identidade (`app/chamados/juizes.py:648`, `app/chamados/juizes.py:658`).
   - `carimbar` passa a recusá-la e `carimbar_identidade` passa a aceitá-la (`app/chamados/models.py:2646`, `app/chamados/models.py:2703`).
   - `q_carimbo_desfazivel` deixa de oferecê-la à porta de desfazer; a porta relê esse filtro travado (`app/chamados/services/relavra_via.py:179`, `app/chamados/services/relavra_via.py:221`).
   - Auditor reconhece a via e a classifica como identidade (`app/chamados/management/commands/auditar_invariantes_chamados.py:47`, `app/chamados/management/commands/auditar_invariantes_chamados.py:195`, `app/chamados/management/commands/auditar_invariantes_chamados.py:206`).
   - Filas de correção/validação mudam indiretamente por consultarem C6 (`app/chamados/juizes.py:1394`, `app/chamados/juizes.py:1401`).

Inventário dos três campos novos, com ocorrências qualificadas:

| Campo | Escritores/defaults | Leitores |
|---|---|---|
| `colaborador` | declaração FK `app/chamados/models.py:2240`; inserção deriva em `:2935`, acionada em `:2923`; migração deriva DS `app/chamados/migrations/0051_decisao_pergunta.py:25`, coluna nullable `:92`, ALTER obrigatório `:105`; fixture explícita `app/chamados/tests/test_e1_entradas.py:313` | `do_fato` `app/chamados/models.py:2430`; consistência de inserção `:2934`, `:2936`, `:2938`; DRY `app/chamados/management/commands/backfill_decisoes_pergunta.py:34`, `:36`, `:54`; teste `app/chamados/tests/test_e1_entradas.py:174` |
| `instancia_marco` | default `app/chamados/models.py:2241`; derivação `:2941`; default migração `app/chamados/migrations/0051_decisao_pergunta.py:96`; backfill `:27`; fixture `app/chamados/tests/test_e1_entradas.py:314` | `app/chamados/models.py:2431`, `:2939`; DRY `app/chamados/management/commands/backfill_decisoes_pergunta.py:54`; teste `app/chamados/tests/test_e1_entradas.py:175` |
| `conversa` | default `app/chamados/models.py:2242`; portas passam False/True `:2450`, `:2461`; INSERT `:2546`; default de TODO acervo `app/chamados/migrations/0051_decisao_pergunta.py:99` | `_nascer` `app/chamados/models.py:2474`, `:2495`; derivação `:2939`; DRY `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; testes `app/chamados/tests/test_e1_entradas.py:176`, `:182` |

A fotografia genérica `.values()` dos testes lê também os três campos (`app/chamados/tests/test_e1_entradas.py:282`). O patch acrescenta leitores das três colunas na constraint; os testes anexos novos escrevem colaborador/instância e consultam a constraint (`e1_indice_0052.patch`, referências de linhas exatas do arquivo completo [não verificado]).

Não encontrei `PerguntaDisputa.objects.create` de produção fora do `cls.objects.create` de `_nascer`; o exemplo fora da porta está no teste e é aceito sem índice (`app/chamados/models.py:2545`, `app/chamados/tests/test_e1_entradas.py:313`). A derivação no `save` protege inserções normais, mas não torna a chave imutável: só roda quando `_state.adding`; instância não vazia fornecida nem é validada contra o motivo (`app/chamados/models.py:2923`, `app/chamados/models.py:2939`). Escritor de produção que mude motivo/instância/conversa depois de nascer: [não verificado].

## 07/11 00:11 · sessao 793560f8174a · - · - · origem=Ronald (tmux)

4. **0051: estruturalmente compatível com o legado descrito; segurança do acervo real [não verificado].** Renomeia tabela/campos, atribui `reabertura` às decisões antigas e cria ligações de absorção nulas, compatíveis com os checks; preenche o colaborador pela disputa e a instância pelo motivo. O `SET CONSTRAINTS ALL IMMEDIATE` antecede o `ALTER ... NOT NULL`, evitando os eventos de FK pendentes que o comentário identifica. Referências: `app/chamados/migrations/0051_decisao_pergunta.py:25`, `:41`, `:49`, `:67`, `:76`, `:101`, `:104`, `:105`.

   **Fila — conversa antiga vira fato na coluna.** Todas recebem `conversa=False`, sem classificação histórica. Contraexemplo: duas conversas antigas do mesmo motivo/dia entram no grupo de colisão e impedem a 0052, embora conversas novas sejam excluídas. A própria migração declara essa limitação. Referências: `app/chamados/migrations/0051_decisao_pergunta.py:4`, `:98`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`.

   O congelado bate com o vivo: os quatro pares são exatamente `saida_sem_entrada→inicio`, `orfao_14h→fim`, `intervalo_saida→int_ida`, `intervalo_volta→int_volta`; há teste direto de igualdade. Referências: `app/chamados/migrations/0051_decisao_pergunta.py:14`; `app/chamados/services/declaracao_texto.py:42`, `:52`; `app/chamados/tests/test_e1_entradas.py:259`.

   **0052: aplicação segura no acervo [não verificado].** Não há resultados do DRY no dado fornecido. O patch exige zero colisões, enquanto a 0051 preserva conversas antigas como `False`. Referências: anexo `e1_indice_0052.patch`, migração adicionada, linhas 3–5 e 19–21; `app/chamados/migrations/0051_decisao_pergunta.py:98`.

5. **O command não escreve e consulta a autoridade correta.** Apenas consulta modelos e escreve em `stdout`; não possui flag de apply. Chama `competencia_entregue`, autoridade registrada como `_K6`, cujo código considera a exportação vigente não invalidada. Referências: `app/chamados/management/commands/backfill_decisoes_pergunta.py:25`, `:51`, `:67`, `:71`, `:76`; `app/core/juizes.py:312`; `app/folha/export.py:556`, `:580`.

   **Fila — o DRY não suporta data nula.** Contraexemplo: pergunta legada respondida, sem decisão e com `data_evento=None`; `linhas_a_criar` chama `competencia_do_fato(None)`, chegando a `None < None`. Duas perguntas nulas sem resposta também chegam a `dia.isoformat()` ao listar colisões. O campo permite nulo; ocorrência real [não verificado]. Referências: `app/chamados/models.py:2230`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:33`, `:45`, `:90`; `app/ponto/services/fechamento.py:1588`, `:1590`, `:1595`.

   O agrupamento por competência usa o adaptador existente e o corte da empresa, sem derivação própria. Referências: `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`; `app/ponto/services/fechamento.py:1595`; `app/ponto/services/dia_pago.py:700`, `:714`.

9. A alteração de dependência corrige instalações novas: o `RunPython` pede `LogAuditoria`, criada em `core.0002`. A única mudança da 0057 está na dependência; suas operações permanecem no mesmo `RunPython`. Referências: `app/ponto/migrations/0057_a4_backfill_pendente.py:23`, `:70`, `:78`; `app/core/migrations/0002_log_auditoria.py:16`.

   Dada a premissa de migração já aplicada, a expectativa é que o executor não repita seu `RunPython` apenas pela nova dependência; comportamento do executor não está demonstrado pelo código da cópia [não verificado]. Estado real do histórico de migrações em produção [não verificado]. [nao verificado]

10. **Fila — a justificativa descreve a mudança, mas erra a unidade do custo.** O `+6` corresponde estruturalmente a duas chamadas contendo `atomic` + `SELECT ... FOR UPDATE`; os números medidos apresentados no comentário não foram verificados. O comentário diz custo por “[nome]”, mas a trava é adquirida antes do dedup e também custa quando retorna uma pergunta existente. Contraexemplo: emitir duas vezes o mesmo fato; a segunda chamada bloqueia/consulta e retorna `criada=False`, sem nascimento. Referências: `app/ponto/tests/test_selo_chokepoint_escrita.py:167`, `:172`, `:174`, `:181`; `app/chamados/models.py:2473`, `:2475`, `:2498`, `:2542`.

   “Conversa não trava” descreve corretamente a ausência do `SELECT FOR UPDATE`; o `atomic` envolve também a conversa. Referências: `app/chamados/models.py:2473`, `:2474`.

6. Dentro deste recorte, não apareceu segundo juiz: competência exportada chama `_K6`, competência do fato reaproveita o adaptador existente e instância é inversão do catálogo. Referências: `app/chamados/management/commands/backfill_decisoes_pergunta.py:28`, `:45`, `:71`; `app/core/juizes.py:312`; `app/chamados/services/declaracao_texto.py:52`.

   O teste de corrida comprova somente o caminho comum por `abrir`, com duas threads e a mesma identidade; seu título promete unicidade “NO BANCO”, mas o commit sem patch não possui essa constraint. Contraexemplo externo ao domínio do teste: inserção direta repetida, que o próprio teste do DRY constrói com sucesso esperado. Referências: `app/chamados/tests/test_e1_identidade_corrida.py:1`, `:49`, `:74`, `:81`; `app/chamados/tests/test_e1_entradas.py:309`, `:313`; `app/chamados/models.py:2942`.

## 06/11 23:35 · sessao 0c388599c44e · V2-BUGS-i2 · verificador · origem=Code

**Não aprovaria o encerramento da volta 3.** P1/P2/P3 foram corrigidos nos caminhos mensais correspondentes, mas P4 continua incompleto entre pares da mesma jornada. O helper novo também contém derivação civil, contrariando a justificativa do censo; leitores do export ainda usam outro dia. (`app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`, `app/ponto/services/fechamento.py:942`, `app/ponto/motor_calculo_v2.py:1048`, `app/ponto/motor_calculo_v2.py:964`, `app/folha/porta_export.py:104`)

**1. Destino de P1–P5**

| Achado | Verificação |
|---|---|
| **P1 — dobra 12x36 civil** | **Curado no caminho mensal com chave:** o laço grava o dia antes de calcular o período, e a dobra agora o lê. Não elimina o fallback civil nem o segundo julgamento preexistente de pertença. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:2849`, `app/ponto/motor_calculo_v2.py:964`, `app/ponto/motor_calculo_v2.py:2443`) |
| **P2 — FT comercial sem dobra/split** | **Curado nesse caminho:** o corpo comercial consulta o dia mapeado, escreve a dobra e depois aplica o split. A cauda exclusivamente posterior às 05h ainda perde prorrogação herdada de outro par. (`app/ponto/motor_calculo_v2.py:3319`, `app/ponto/motor_calculo_v2.py:3324`, `app/ponto/motor_calculo_v2.py:1048`) |
| **P3 — filtro de escala certa civil** | **Curado na origem do filtro:** filtro e promoção consultam `_dia_de` com o mesmo mapa. (`app/ponto/services/fechamento.py:940`, `app/ponto/services/fechamento.py:942`, `app/ponto/services/fechamento.py:954`) |
| **P4 — HE noturna prorrogada** | **Parcial:** recupera o contexto do próprio par; não recupera o contexto da jornada composta por outros pares. (`app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:1048`) |
| **P5 — testes forçavam escala certa** | **Curado para a fronteira pedida:** F11i responde conforme o dia recebido e exige sete horas de feriado. Não valida os critérios internos reais de escala certa nem cobra o noturno nessa variante. (`app/ponto/tests/test_o280_bateria_da_lei.py:1819`, `app/ponto/tests/test_o280_bateria_da_lei.py:1918`, `app/ponto/tests/test_o280_bateria_da_lei.py:1919`) |

**2. Escritores, leitores e significado dos campos**

O mapa novo não contém exclusivamente a resposta do juiz registrado: `_dia_do_turno` lê `data_turno`, mas o laço pode substituí-lo pela herança de até seis horas. Partido e comercial depois remontam agrupamentos diretamente pelo mapa do juiz. Existem, portanto, duas procedências para “dia da jornada”. (`app/ponto/motor_calculo_v2.py:909`, `app/ponto/motor_calculo_v2.py:2439`, `app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:2982`, `app/ponto/motor_calculo_v2.py:3382`)

| Dado | Quem escreve e quem lê |
|---|---|
| **`data_jornada` / mapa por entrada** | O laço calcula `data_jornada`, grava `_dia_jornada_do_par[localizar(entrada)]` e chama `calcular_periodo`. O helper lê esse mapa nos corpos 12x36/comercial. (`app/ponto/motor_calculo_v2.py:2440`, `app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2495`, `app/ponto/motor_calculo_v2.py:964`, `app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`) |
| **Mapa por período** | A base grava `id(periodo) → data_jornada`; `_aplicar_teto_pontualidade` publica/mescla em `resultado.dia_da_jornada`. Partido também publica mapa próprio; fechamento mescla mapas das fatias e entrega o resultado ao DiaPago. (`app/ponto/motor_calculo_v2.py:2497`, `app/ponto/motor_calculo_v2.py:1108`, `app/ponto/motor_calculo_v2.py:3012`, `app/ponto/services/fechamento.py:315`, `app/ponto/services/fechamento.py:1225`) |
| **`minutos_extra_100_feriado`** | Escrito nas dobras por período, FT partido e dobras diárias repartidas. ResultadoMes soma regulares; fechamento acrescenta trabalhado dos FT promovidos; DiaPago lê regulares e promove FT na sua linha. (`app/ponto/motor_calculo_v2.py:2852`, `app/ponto/motor_calculo_v2.py:3007`, `app/ponto/motor_calculo_v2.py:3155`, `app/ponto/motor_calculo_v2.py:3327`, `app/ponto/motor_calculo_v2.py:3414`, `app/ponto/motor_calculo_v2.py:337`, `app/ponto/services/fechamento.py:962`, `app/ponto/services/dia_pago.py:183`, `app/ponto/services/dia_pago.py:250`) |
| **`minutos_extra_100_noturna`** | `he_noturna` calcula; `_aplicar_he_noturna` escreve retorno positivo. Resets/bloqueio zeram. ResultadoMes soma regulares; FM e DiaPago acrescentam o campo dos FT promovidos. (`app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:1053`, `app/ponto/motor_calculo_v2.py:1509`, `app/ponto/motor_calculo_v2.py:2978`, `app/ponto/motor_calculo_v2.py:349`, `app/ponto/services/fechamento.py:970`, `app/ponto/services/dia_pago.py:252`) |
| **Noturno relógio/legal** | Não existe campo genérico `minutos_noturnos` no período: há relógio e legais. `_escrever_noturnas` recebe a conta; legais aplicam o fator. ResultadoMes inclui regulares e FT; FM e DiaPago leem esses valores. (`app/ponto/motor_calculo_v2.py:219`, `app/ponto/motor_calculo_v2.py:220`, `app/ponto/motor_calculo_v2.py:1016`, `app/ponto/motor_calculo_v2.py:1033`, `app/ponto/motor_calculo_v2.py:326`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:224`) |
| **Persistência FM/DiaPago** | FM persiste no fechamento; `lavrar` substitui as linhas da versão do DiaPago. A restauração também pode atualizar somente FM. (`app/ponto/services/fechamento.py:1152`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1400`) |

**Os leitores não compartilham integralmente o mesmo dia:**

| Leitor | Contrato encontrado |
|---|---|
| **Fechamento/DiaPago** | Filtro, promoção e lavratura usam o mapa da jornada; `_dia_de` também possui fallback civil se faltar mapa/chave. (`app/ponto/services/fechamento.py:942`, `app/ponto/services/fechamento.py:954`, `app/ponto/services/dia_pago.py:129`, `app/ponto/services/dia_pago.py:132`, `app/ponto/services/dia_pago.py:245`) |
| **Calendário** | Dinheiro vem do DiaPago por data; geometria vem de `data_turno`. Pode divergir da base quando esta sobrescreve a resposta do juiz pela herança de seis horas. (`app/colaboradores/services/calendario.py:221`, `app/colaboradores/services/calendario.py:224`, `app/colaboradores/services/calendario.py:655`, `app/ponto/motor_calculo_v2.py:2443`) |
| **Tela/espelho/PDF** | Cards usam o juiz do dia; rubricas diárias leem DiaPago. O resumo provisório do espelho ainda recorta períodos pela data civil; o PDF chama essa soma sem o recorte. (`app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:813`, `app/ponto/services/espelho.py:84`, `app/ponto/services/espelho.py:439`, `app/relatorios/pdf_espelho.py:502`, `app/relatorios/pdf_espelho.py:837`) |
| **`porta_export`** | Uma testemunha agrega pela data civil local; outra pela data crua de entrada. Não seguem a atribuição por jornada do escritor. (`app/folha/porta_export.py:104`, `app/folha/porta_export.py:538`) |
| **Calculador** | A alimentação exige mapa por período; sem ele declara `sem_dia_da_jornada` e ignora o período. O split chama a mesma `he_noturna`, por par, preservando a limitação de contexto. (`app/ponto/calculador/alimentacao.py:78`, `app/ponto/calculador/alimentacao.py:81`, `app/ponto/calculador/regras.py:827`, `app/ponto/calculador/regras.py:832`) |
| **TXT** | Lê FM: HE100 normal/feriado e adicional noturno total. Não emite evento separado para HE100 noturna; alteração somente nesse subconjunto não muda esses eventos. (`app/folha/export.py:420`, `app/folha/export.py:424`, `app/folha/export.py:443`, `app/folha/export.py:447`) |

**3. O fallback é derivação paralela, apesar do comentário**

Quando a chave falta, `_dia_da_jornada_do_par` devolve a data civil local da entrada. Não consulta o mapa do juiz disponível na instância, não declara ausência de atribuição e não demonstra que aquele par constitui sozinho uma jornada. (`app/ponto/motor_calculo_v2.py:964`, `app/ponto/motor_calculo_v2.py:2433`)

**Contraexemplo reproduzido em memória:** entrada em **07/10 às 06h**, mapa novo vazio e `_dia_turno_juiz_calc` atribuindo essa entrada a **06/10**. O helper retorna **07/10**. Assim, sua resposta pode contradizer uma atribuição já disponível; “não decide dia nenhum” é falso nesse ramo. (`app/ponto/motor_calculo_v2.py:964`, `app/ponto/tests/test_contract_juiz_batida.py:329`)

No caminho mensal normal, a chave é preenchida antes da chamada. Testes que instanciam motor e chamam `calcular_periodo` diretamente alcançam o fallback. Existe ainda chamada produtiva auxiliar no crédito de aberto, fora daquele preenchimento; sua entrada só cai no fallback se não tiver sido previamente mapeada. Fixture produtiva integrada comprovando essa ausência: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2495`, `app/ponto/tests/test_12x36_com_escala_noturna.py:38`, `app/ponto/tests/test_12x36_com_escala_noturna.py:41`, `app/ponto/motor_calculo_v2.py:2590`)

O auxiliar empresta somente trabalhado e fora da janela; sua dobra/split não são copiados. Portanto esse chamador, isoladamente, não prova dinheiro novo incorreto. (`app/ponto/motor_calculo_v2.py:2591`, `app/ponto/motor_calculo_v2.py:2592`, `app/ponto/motor_calculo_v2.py:2596`)

**4. P4 fora de feriado**

A mudança não recebe nem consulta feriado. A reprodução abaixo extraiu funções por AST e restaurou em memória somente o trecho anterior de `he_noturna`; não executou o commit pai completo. (`app/ponto/motor_calculo_v2.py:414`, `app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:453`)

| Par / condição | HE100 noturna: corpo anterior → atual |
|---|---:|
| 22–07, HE100 final de 60, prorrogação ligada | **0 → 60**. Correção no contexto do próprio par. (`app/ponto/motor_calculo_v2.py:453`) |
| 22–07, HE100 final de 60, pausa 06–06h15 | **0 → 60**; o trecho recua por trabalho e desconta a pausa. (`app/ponto/motor_calculo_v2.py:445`, `app/ponto/tests/test_o280_bateria_da_lei.py:2003`) |
| 22–07, HE100 final de 60, prorrogação desligada | **0 → 0**. (`app/ponto/tests/test_o280_bateria_da_lei.py:2005`) |
| Cauda própria 06–07, HE100 de 60 | **0 → 0**, mesmo quando deveria continuar a jornada noturna do par anterior. (`app/ponto/motor_calculo_v2.py:114`, `app/ponto/motor_calculo_v2.py:1048`) |
| 22–01 dois dias depois, 27h, HE100 final de 60 | **60 → 0**, inclusive sem prorrogação. O zero decorre do teto do recorte inteiro. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:201`, `app/ponto/motor_calculo_v2.py:453`) |

**O resto de P4 é concreto:** numa jornada com par noturno até 05h e cauda 06–07, herdar o dia não transporta a noturnidade. Tanto o adicional total quanto o split continuam medidos por pares isolados. Pela regra fornecida de prorrogação da jornada, a cauda de 60 minutos permanece sem os 60 devidos. (`app/ponto/motor_calculo_v2.py:2444`, `app/ponto/motor_calculo_v2.py:1013`, `app/ponto/motor_calculo_v2.py:1048`, `app/ponto/calculador/regras.py:85`)

Para **mais de 26h**, o zero é uma guarda de sanidade, não demonstração do valor da regra: a função zera o todo e limita a parte a esse zero. O pareamento normal declara teto de 26h; ocorrência produtiva além dele: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:201`, `app/ponto/motor_calculo_v2.py:209`)

Para **Londrina cl.38-d**, o afastamento da redução troca o fator para 1; não elimina minutos noturnos de relógio nem decide prorrogação. O split de 60 permanece 60 quando a prorrogação está ligada. A configuração concreta dos flags na produção fornecida: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:1022`, `app/ponto/motor_calculo_v2.py:1051`, `app/core/regua_cct.py:466`, `app/core/regua_cct.py:470`)

**5. Sítios restantes e dinheiro**

- **Testemunha do export por dia civil:** no desenho F11h, compara períodos **3h em d / 4h em d+1** contra DiaPago **7h em d / 0h em d+1**. Acusa diferença de atribuição como divergência de quantidade. Essa reconciliação permanece fora de `falhas`. (`app/folha/porta_export.py:104`, `app/ponto/services/dia_pago.py:245`, `app/ponto/tests/test_o280_bateria_da_lei.py:1889`, `app/folha/porta_export.py:696`)
- **Invariante HE/intra por data crua:** consulta previsto e intervalo no dia de `_p.entrada.date()`, que pode inclusive ser UTC. A redistribuição por par pode mudar a acusação; esse contador integra `falhas` e pode afetar a liberação do TXT. Ocorrência na frota: **[não verificado]**. (`app/folha/porta_export.py:538`, `app/folha/porta_export.py:548`, `app/folha/porta_export.py:560`, `app/folha/porta_export.py:689`)
- **Alerta com dia errado:** decide folga semanal por `data_jornada`, mas imprime o dia da semana da entrada civil. Cauda de segunda pertencente à jornada de domingo pode dizer “folga semanal (Segunda)”. Aqui o efeito direto é textual. (`app/ponto/motor_calculo_v2.py:2511`, `app/ponto/motor_calculo_v2.py:2516`, `app/ponto/motor_calculo_v2.py:2521`)
- **Janela anterior à dobra ainda civil:** empresa e marcos são consultados por `entrada.date()`. Corrigir o dia da dobra não corrige esse recorte anterior do par efetivo, que participa da duração paga. Contraexemplo integrado novo: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:1562`, `app/ponto/motor_calculo_v2.py:1570`, `app/ponto/motor_calculo_v2.py:2797`)
- **Banco/DSR comerciais:** ainda usam data civil para extras/presença/denominador semanal. DSR subtrai HE100 feriado, conservando zero para a dobra repartida; os demais usos continuam temporalmente divergentes. Efeito concreto na frota: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:3502`, `app/ponto/motor_calculo_v2.py:3520`, `app/ponto/motor_calculo_v2.py:3543`, `app/ponto/motor_calculo_v2.py:3549`)

Há também uma divergência preexistente com a formulação **literal e sem exceção** do pedido: o código continua condicionando dobra a `feriado_em_dobra`, e a fábrica desliga esse flag para 12x36 por padrão; o TXT também suprime a parcela nesse regime sem a cláusula. Logo a cópia não implementa universalmente “feriado trabalhado = HE100”. (`app/ponto/motor_calculo_v2.py:949`, `app/ponto/motor_calculo_v2.py:3678`, `app/ponto/motor_calculo_v2.py:3749`, `app/folha/export.py:422`)

**6. Mordida dos REDs e censo**

Nenhum dos seis REDs aceita qualquer resultado: `confere` e `cobra_a_lei` usam igualdade dos campos esperados. Isso também significa que campos omitidos da expectativa não são certificados. (`app/ponto/tests/test_o280_bateria_da_lei.py:1309`, `app/ponto/tests/test_o280_bateria_da_lei.py:1321`)

| RED | Valor exigido / limite |
|---|---|
| **F10d/e** | Cobram zero de feriado; F10e saiu de `expectedFailure`. Invertem efetivamente a proteção dos 90 indevidos. (`app/ponto/tests/test_o280_bateria_da_lei.py:1593`, `app/ponto/tests/test_o280_bateria_da_lei.py:1596`, `app/ponto/tests/test_o280_bateria_da_lei.py:1608`) |
| **F10f** | Cobra 660 de dobra, incluindo a cauda. **Não cobra o noturno da cauda**, portanto deixa passar o resto de P4. (`app/ponto/tests/test_o280_bateria_da_lei.py:1624`) |
| **F10g** | Exige uma cauda distinta em FT e cobra 120 trabalhados/feriado/noturna. (`app/ponto/tests/test_o280_bateria_da_lei.py:1654`, `app/ponto/tests/test_o280_bateria_da_lei.py:1657`) |
| **F11i** | Cobra 7h de feriado com resposta de escala certa dependente do dia recebido. Não cobra noturno nessa variante. (`app/ponto/tests/test_o280_bateria_da_lei.py:1918`, `app/ponto/tests/test_o280_bateria_da_lei.py:1919`) |
| **N-PRORROG** | Cobra **60/60/0**. Na reprodução pura, o corpo anterior reconstruído deu **0/0/0**: há mordida numérica demonstrada. Só cobre contexto dentro do mesmo par. (`app/ponto/tests/test_o280_bateria_da_lei.py:2001`, `app/ponto/tests/test_o280_bateria_da_lei.py:2006`, `app/ponto/motor_calculo_v2.py:453`) |

F10d/e/f/g e F11i exercitam os ramos alterados; execução integrada desses REDs no pai completo e na cópia atual: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`, `app/ponto/services/fechamento.py:942`)

**O censo 10→11 é aritmeticamente legítimo para seu contrato estrutural**, que conta métodos privados e vigia portas públicas do dia. **Não legitima a justificativa semântica:** o contador examina nomes, não se o método deriva datas; passa mesmo com o fallback civil. “Não decide dia nenhum” encobre justamente esse ramo. (`app/ponto/tests/test_contract_juiz_batida.py:140`, `app/ponto/tests/test_contract_juiz_batida.py:319`, `app/ponto/tests/test_contract_juiz_batida.py:329`, `app/ponto/tests/test_contract_juiz_batida.py:330`, `app/ponto/motor_calculo_v2.py:964`)

**7. Classes estruturais**

- **Segundo juiz / derivação paralela:** permanece a sobrescrita por até seis horas; o helper novo acrescenta derivação civil quando falta chave. A pertença já tem autoridade registrada. (`app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:964`, `app/core/juizes.py:196`)
- **Campo com dois significados — F1 permanece:** v2 corrigido deposita a quantidade própria em cada par; `reler` zera os pais HE, preserva splits e recoloca toda dobra no último. O primeiro par do NPART pode terminar com **HE100=0 e HE100 noturna=120**. Efeito produtivo dessa releitura: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:3151`, `app/ponto/calculador/periodo.py:761`, `app/ponto/calculador/periodo.py:770`, `app/ponto/calculador/periodo.py:841`)
- **Porta de escrita:** as alterações de FM/DiaPago permanecem nos serviços existentes. O risco preexistente de persistir FM e capturar falha posterior do DiaPago continua. (`app/core/portas.py:500`, `app/ponto/services/fechamento.py:1152`, `app/ponto/services/fechamento.py:1207`, `app/ponto/services/fechamento.py:1246`, `app/ponto/services/dia_pago.py:359`)
- **Selo sem mordida:** os novos REDs têm mordida nos campos cobrados; o censo não prova ausência de juiz semântico, e F10f deixa descoberta a prorrogação da cauda. (`app/ponto/tests/test_o280_bateria_da_lei.py:1309`, `app/ponto/tests/test_o280_bateria_da_lei.py:1624`, `app/ponto/tests/test_contract_juiz_batida.py:140`)
- **Relógio/travas:** os trechos novos usam instantes recebidos, sem nova leitura de “agora”. Ordem de travas e execução concorrente completa: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:452`, `app/ponto/motor_calculo_v2.py:963`, `app/ponto/services/fechamento.py:942`)

A validação executada foi estática e por funções puras em memória. Suíte Django, pai completo, banco, logs e novo DIFF de frota/TXT sobre **b6b1e3d5**, especialmente fora de feriado: **[não verificado]**.

## 06/11 23:32 · sessao fbda007c5eeb · - · - · origem=Ronald (tmux)

P1/P2 corrigem os dois sítios por período quando o laço mensal forneceu a chave: mapa preenchido antes da chamada em `app/ponto/motor_calculo_v2.py:2494`; 12x36 lê em `:2835` e dobra em `:2849`; comercial lê em `:3319` e dobra em `:3324`. A correção não elimina a derivação paralela preexistente: a base sobrescreve o dia do juiz pela herança de intervalo ≤6h em `:2439–2444`; partido remonta seu dia pelo juiz em `:2982`, inclusive FT em `:2995`; comercial refaz o agrupamento dos regulares em `:3382`. Portanto “sempre o mesmo juiz” não está demonstrado pelo código. (`app/ponto/motor_calculo_v2.py:2439`, `app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:2982`, `app/ponto/motor_calculo_v2.py:3382`)

P4 é parcial: cura a perda da prorrogação dentro do próprio par, mas não transporta contexto noturno entre pares da mesma jornada. A casca continua entregando somente as pontas efetivas daquele período; data herdada não leva informação de prorrogação. (`app/ponto/motor_calculo_v2.py:1048`, `app/ponto/motor_calculo_v2.py:2444`)

Reproduzi as funções por extração AST e execução em memória, sem Django/arquivos. “Pai reconstruído” abaixo restaura somente o corpo anterior de `he_noturna` fornecido no diff; não é execução do commit pai inteiro. (`app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:453`)

| Entrada/saída, prorrogação ligada | HE100 | Pai reconstruído → atual |
|---|---:|---:|
| 22–07 seguinte | 60 | 0 → 60 |
| 22–07 seguinte | 120 | 0 → 120 |
| 00–07 | 120 | 0 → 120 |
| 04–07 | 120 | 0 → 120 |
| Cauda própria 06–07 | 60 | 0 → 0 |
| 22–01, dois dias depois, 27h | 60 | 60 → 0 |
| 22–06, dois dias depois, 32h | 120 | 120 → 0 |

As mudanças independem de feriado; a função não recebe esse atributo. Os quatro primeiros resultados vêm da medição do trecho no contexto do próprio par. A cauda própria permanece zero porque começa depois das 05h e não contém o gatilho 00–05; caso seja continuação da jornada noturna anterior, falta o contexto exigido pela regra do pedido. (`app/ponto/motor_calculo_v2.py:414`, `app/ponto/motor_calculo_v2.py:114`, `app/ponto/motor_calculo_v2.py:453`)

Nos pares >26h o zero novo vem da guarda de sanidade, não de comprovação do valor legal: o noturno do todo é zerado e o medidor da parte o limita a esse zero. O pareamento normal declara teto de 26h; ocorrência produtiva além desse teto: [não verificado]. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:201`, `app/ponto/motor_calculo_v2.py:204`)

Londrina cl.38-d não exige redução do split: ele mede minutos de relógio. O afastamento da hora reduzida altera o fator para 1; a prorrogação é outro parâmetro. Para 22–07 com HE final 60, o split atual é 60 se prorrogação ligada, zero se desligada; o fator da cl.38-d não modifica essa classificação. A configuração produtiva concreta desses flags não foi fornecida. (`app/ponto/motor_calculo_v2.py:1022`, `app/ponto/motor_calculo_v2.py:1051`, `app/core/regua_cct.py:466`, `app/core/regua_cct.py:470`; configuração: [não verificado])

O fallback novo **deriva dia civil** quando falta a chave, inclusive quando existe mapa da autoridade disponível na instância: ele não consulta `_dia_turno_juiz_calc`, apenas `_dia_jornada_do_par`. Contraexemplo puro: entrada `13/10 06h`, mapa novo vazio, mapa do juiz atribuindo essa entrada a `12/10`; o helper devolve `13/10`. Logo a afirmação documental “não decide dia nenhum” omite esse ramo. (`app/ponto/motor_calculo_v2.py:964`, `app/ponto/motor_calculo_v2.py:2433`, `app/ponto/tests/test_contract_juiz_batida.py:329`)

Há chamador produtivo fora do laço preenchedor: os pares auxiliares do crédito de aberto passam por `calcular_periodo` em `:2590`. Ele só cai no fallback se sua entrada não foi entrada de um período previamente registrado em `:2494`; não reproduzi um fixture integrado que demonstre essa ausência. O crédito copia somente trabalhado/fora da janela, descartando a HE/noturno calculados no auxiliar; não atribuir efeito monetário à dobra auxiliar sem prova adicional. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2590`, `app/ponto/motor_calculo_v2.py:2591`, `app/ponto/motor_calculo_v2.py:2596`; fixture integrado: [não verificado])

Chamadas diretas dos testes chegam ao fallback porque constroem o motor e chamam `calcular_periodo` sem `calcular_mes`; exemplos concretos em `app/ponto/tests/test_12x36_com_escala_noturna.py:38` e `:41`, além de `:50` e `:53`. O censo 10→11 é coerente com a pergunta estrutural “quantos ajudantes privados do dia existem”, mas não certifica ausência de derivador paralelo: o contrato cobra contagem/classe e ausência de porta pública; seu comentário adicional de pureza é falso no fallback. (`app/ponto/tests/test_contract_juiz_batida.py:319`, `app/ponto/tests/test_contract_juiz_batida.py:330`, `app/ponto/tests/test_contract_juiz_batida.py:332`, `app/ponto/motor_calculo_v2.py:964`)

Restam leitores civis monetários no próprio comercial: extras semanais do banco usam a data civil da entrada; DSR reconstrói presença, semana das extras e denominador de dias trabalhados pela mesma data civil. Assim não seguem integralmente o dia que o motor escreveu no mapa. Um par de domingo à noite cuja cauda cai na segunda pode atravessar a fronteira semanal nesses leitores. Efeito na frota fornecida: [não verificado]. (`app/ponto/motor_calculo_v2.py:3502`, `app/ponto/motor_calculo_v2.py:3520`, `app/ponto/motor_calculo_v2.py:3545`, `app/ponto/motor_calculo_v2.py:3549`)

## 06/11 23:32 · sessao b9ba20b70125 · - · - · origem=Ronald (tmux)

Achados e mapa de leitores, somente pela cópia:

1. **P3 está curado no sítio:** filtro de escala certa e promoção usam `_dia_de(p, resultado.dia_da_jornada)`, antes de separar FT certa/feriado/sem escala. A mesma partição segue para `lavrar`. (`app/ponto/services/fechamento.py:941`, `app/ponto/services/fechamento.py:954`, `app/ponto/services/fechamento.py:988`, `app/ponto/services/fechamento.py:1211`)

2. **A testemunha da exportação continua comparando dias diferentes.** `dias_fora_do_dia_pago` soma períodos pela data civil local da entrada; DiaPago foi agrupado por jornada. No F11h, períodos de 3h em d e 4h em d+1 produzem espelho civil 3h/4h versus DiaPago 7h/0h, acusando divergência apesar da atribuição correta ao turno. Esse contador continua fora de `falhas`. (`app/folha/porta_export.py:104`, `app/ponto/services/dia_pago.py:245`, `app/ponto/tests/test_o280_bateria_da_lei.py:1889`, `app/folha/porta_export.py:696`)

3. **Outro invariante do export usa data civil crua:** `_p.entrada.date()` agrupa HE/intra/trabalhado, podendo inclusive usar dia UTC; previsto e intervalo declarado são consultados nessa data. A redistribuição da dobra de um portador para vários períodos pode mudar essa acusação. Ele integra `falhas`, portanto pode afetar a liberação do TXT; ocorrência na frota é [não verificado]. (`app/folha/porta_export.py:538`, `app/folha/porta_export.py:548`, `app/folha/porta_export.py:560`, `app/folha/porta_export.py:689`)

4. **Alerta ainda mistura dia julgado e dia civil:** a escolha “folga semanal” consulta `data_jornada`, mas imprime o nome do dia pelo `weekday()` da entrada do par. Cauda de segunda pertencente à jornada de domingo pode dizer “folga semanal (Segunda)”. Isso muda texto, sem escrita direta de rubrica nesse ramo. (`app/ponto/motor_calculo_v2.py:2511`, `app/ponto/motor_calculo_v2.py:2516`, `app/ponto/motor_calculo_v2.py:2521`)

5. **Janela continua civil antes da dobra:** `_janela_do_dia` consulta empresa e marcos por `entrada.date()`. A dobra agora usar o mapa não corrige esse recorte anterior do par efetivo. Trata-se da mesma inconsistência temporal, embora a pergunta ali seja janela/marcos, não identificação de feriado; pode alterar duração e HE. O próprio calculador documenta pares invertidos por essa ancoragem, mas os números comentados não foram reproduzidos nesta análise: [não verificado]. (`app/ponto/motor_calculo_v2.py:1562`, `app/ponto/motor_calculo_v2.py:1570`, `app/ponto/calculador/periodo.py:660`)

**Escritores e leitores dos campos**

| Campo | Escrita e leitura |
|---|---|
| `minutos_extra_100_feriado` | Dobras por período 12x36/comercial; FT partido; dobras diárias repartidas. ResultadoMes soma regulares; fechamento acrescenta FT promovida. DiaPago lê regulares e, para FT feriado, promove trabalhado. (`app/ponto/motor_calculo_v2.py:2852`, `app/ponto/motor_calculo_v2.py:3007`, `app/ponto/motor_calculo_v2.py:3155`, `app/ponto/motor_calculo_v2.py:3327`, `app/ponto/motor_calculo_v2.py:3414`, `app/ponto/motor_calculo_v2.py:337`, `app/ponto/services/fechamento.py:962`, `app/ponto/services/dia_pago.py:183`, `app/ponto/services/dia_pago.py:250`) |
| `minutos_extra_100_noturna` | Casca `_aplicar_he_noturna` escreve retorno positivo de `he_noturna`; resets e bloqueio zeram. ResultadoMes soma regulares; fechamento e DiaPago acrescentam campo dos FT promovidos. (`app/ponto/motor_calculo_v2.py:1048`, `app/ponto/motor_calculo_v2.py:1053`, `app/ponto/motor_calculo_v2.py:1509`, `app/ponto/motor_calculo_v2.py:2978`, `app/ponto/motor_calculo_v2.py:349`, `app/ponto/services/fechamento.py:970`, `app/ponto/services/dia_pago.py:252`) |
| Noturno relógio/legal | `_escrever_noturnas` lê `calcular_noturnas`; legais = relógio × fator. ResultadoMes soma regulares e FT; FM recebe o total; DiaPago soma os campos nos respectivos dias. Não há um campo genérico `minutos_noturnos` no dataclass. (`app/ponto/motor_calculo_v2.py:219`, `app/ponto/motor_calculo_v2.py:220`, `app/ponto/motor_calculo_v2.py:1013`, `app/ponto/motor_calculo_v2.py:1016`, `app/ponto/motor_calculo_v2.py:1033`, `app/ponto/motor_calculo_v2.py:326`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:224`) |
| FM / DiaPago persistidos | FM recebe totais e persiste por `save`/`update`; `lavrar` apaga/recria a versão do DiaPago. Falha posterior da lavratura é capturada após persistência do FM. Restauração atualiza somente FM. (`app/ponto/services/fechamento.py:1152`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1246`, `app/ponto/services/fechamento.py:1400`) |

**Os leitores não compartilham integralmente o mesmo contrato de dia.**

- Calendário lê dinheiro do DiaPago por data; suas batidas/cards vêm de `turnos_do_colab` pela `CargaDaJanela`, agrupados por `t.data_turno`, não por data civil. Assim, quando a base sobrescreve o dia do juiz pela herança de seis horas, dinheiro e geometria podem ter origens diferentes. (`app/colaboradores/services/calendario.py:221`, `app/colaboradores/services/calendario.py:224`, `app/colaboradores/services/calendario.py:610`, `app/colaboradores/services/calendario.py:655`, `app/ponto/motor_calculo_v2.py:2443`)
- Espelho também agrupa cards pelo juiz `dia_das_batidas`, mas lê rubricas do DiaPago. Seu resumo provisório ainda recorta períodos pela data civil local; o PDF chama a mesma soma sem esse recorte. (`app/ponto/services/espelho.py:450`, `app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:813`, `app/ponto/services/espelho.py:84`, `app/ponto/services/espelho.py:439`, `app/relatorios/pdf_espelho.py:502`)
- Tela diária e PDF imprimem rubricas lavradas; não remontam o split. O PDF ainda consulta os períodos para decidir presença de coluna. (`app/templates/ponto/espelho.html:268`, `app/templates/ponto/espelho.html:283`, `app/relatorios/pdf_espelho.py:837`, `app/relatorios/pdf_espelho.py:854`)
- TXT lê FM: HE100 normal/feriado e adicional noturno total. Não emite evento separado para HE100 noturna; portanto mudanças apenas no subconjunto não alteram essas linhas do TXT. Mudança de `horas_noturnas` altera a entrada do adicional. (`app/folha/export.py:420`, `app/folha/export.py:424`, `app/folha/export.py:443`, `app/folha/export.py:447`)

**V3 conserva campo com dois significados, já F1 da fila.** A alimentação lê dia da jornada e pares portadores de HE; sem mapa declara contador e ignora o período. `reler`, porém, zera pais HE em todos os pares, preserva splits e recoloca a dobra inteira no último. No NPART corrigido, o primeiro pode terminar HE100=0 e HE100 noturna=120. É subconjunto maior que pai por período. Chamadas encontradas de `reler` na cópia são testes; efeito monetário produtivo decorrente é [não verificado]. (`app/ponto/calculador/alimentacao.py:78`, `app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/periodo.py:611`, `app/ponto/calculador/periodo.py:761`, `app/ponto/calculador/periodo.py:770`, `app/ponto/calculador/periodo.py:841`, `app/ponto/tests/test_o280_bateria_da_lei.py:4678`)

Banco e DSR ainda agrupam extras por data civil; DSR subtrai HE100 feriado, conservando zero para a dobra repartida. A6 ainda conta ocorrências sem deduplicar dia. Esses restos já constam da fila fornecida; impacto concreto na frota é [não verificado]. (`app/ponto/motor_calculo_v2.py:3502`, `app/ponto/motor_calculo_v2.py:3543`, `app/ponto/motor_calculo_v2.py:3545`, `app/ponto/services/esmeril_espelho.py:177`, `app/ponto/services/esmeril_espelho.py:179`)

## 06/11 23:31 · sessao 9f3eff5d28e6 · - · - · origem=Ronald (tmux)

Os seis REDs cobram valores explícitos; nenhum aceita qualquer resultado ou exceção: `confere` compara o subconjunto esperado por igualdade, e `cobra_a_lei` faz a mesma comparação diretamente. (`app/ponto/tests/test_o280_bateria_da_lei.py:1306`, `app/ponto/tests/test_o280_bateria_da_lei.py:1321`)

| RED | Valor exigido e mordida |
|---|---|
| F10d | Dois pares, 660 trabalhados, HE100/feriado zero; troca a caracterização indevida de 90 pela regra. (`app/ponto/tests/test_o280_bateria_da_lei.py:1593`) |
| F10e | HE100 feriado zero, sem `expectedFailure`; é praticamente a mesma fronteira de F10d, com uma única rubrica. (`app/ponto/tests/test_o280_bateria_da_lei.py:1596`, `app/ponto/tests/test_o280_bateria_da_lei.py:1608`) |
| F10f | Dois pares, 660 trabalhados e 660 de HE100/feriado. **Não cobra noturno da cauda**; portanto pode passar mesmo que a prorrogação herdada continue perdida. (`app/ponto/tests/test_o280_bateria_da_lei.py:1624`) |
| F10g | Exige exatamente uma cauda em FT e cobra 120 trabalhados/feriado/HE100 noturna nela. A fixture exercita efetivamente um par distinto após meia-noite. (`app/ponto/tests/test_o280_bateria_da_lei.py:1654`, `app/ponto/tests/test_o280_bateria_da_lei.py:1657`) |
| F11i | A autoridade simulada responde `dia == fer`; exige FM feriado 7h, folga zero e DiaPago feriado 7h. Exercita a data passada ao filtro, mas **não cobra noturno**, nem valida os critérios reais internos de `escala_certa_no_dia`. (`app/ponto/tests/test_o280_bateria_da_lei.py:1819`, `app/ponto/tests/test_o280_bateria_da_lei.py:1918`, `app/ponto/tests/test_o280_bateria_da_lei.py:1919`) |
| N-PRORROG | Cobra `(60,60,0)` para prorrogação ligada sem pausa, ligada com pausa e desligada. Cobre contexto interno do mesmo par 22–07; não cobre prorrogação herdada de outro par. (`app/ponto/tests/test_o280_bateria_da_lei.py:2001`, `app/ponto/tests/test_o280_bateria_da_lei.py:2006`) |

Reproduzi em memória as funções puras, sem importar Django nem gravar arquivo. O corpo atual deu **`(60,60,0)`**; recolocando somente o trecho anterior de `he_noturna`, reconstruído pelo diff fornecido, deu **`(0,0,0)`**. Isso demonstra mordida numérica de N-PRORROG no corpo anterior, **não execução do pai inteiro**. (`app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:453`, `app/ponto/tests/test_o280_bateria_da_lei.py:2006`; execução do pai inteiro: **[não verificado]**)

Para F10d/e/f/g e F11i, os pontos alterados correspondem à condição exercitada pelos REDs; execução integrada no pai e na cópia atual: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`, `app/ponto/services/fechamento.py:942`)

**Censo 10 → 11:** reproduzi por AST **11 privados, 35 públicos e 19 portas**. O incremento é aritmeticamente legítimo para o contrato efetivo de ausência de nova **porta pública do DIA**; `_dia_da_jornada_do_par` é privado e recebe o par. O teste conta nomes que contêm `dia`/`jornada`, não examina a semântica de suas decisões. (`app/ponto/tests/test_contract_juiz_batida.py:132`, `app/ponto/tests/test_contract_juiz_batida.py:313`, `app/ponto/tests/test_contract_juiz_batida.py:330`)

**A justificativa do incremento é falsa em seu absoluto:** “não decide dia nenhum” só vale quando a chave existe. A expressão `.get(_e, _e.date())` deriva o dia civil quando falta a chave, enquanto a pertinência da batida já tem autoridade registrada em `turnos_do_colab`. O censo não distingue leitor de mapa de derivador paralelo; passa com ambos. (`app/ponto/tests/test_contract_juiz_batida.py:329`, `app/ponto/motor_calculo_v2.py:964`, `app/core/juizes.py:196`)

O laço mensal grava a chave antes da chamada normal, mas existe outra chamada produtiva no crédito de aberto sem gravação correspondente nesse ponto. O novo helper pode decidir por fallback nesse caminho; o período auxiliar depois empresta somente trabalhado/fora da janela, portanto isso isoladamente não prova dinheiro novo incorreto. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2590`, `app/ponto/motor_calculo_v2.py:2596`)

A base também conserva o segundo julgamento preexistente: recebe o dia do juiz e pode substituí-lo pela herança de intervalo até seis horas. O mapa novo passa adiante essa resposta composta, não necessariamente apenas o veredito do juiz registrado. (`app/ponto/motor_calculo_v2.py:2439`, `app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:2494`)

As adições do diff usam instantes recebidos e datas derivadas deles; não introduzem leitura de relógio nas funções alteradas. Isso não certifica concorrência nem ordem de travas da execução completa. (`app/ponto/motor_calculo_v2.py:452`, `app/ponto/motor_calculo_v2.py:963`, `app/ponto/services/fechamento.py:942`; execução concorrente: **[não verificado]**)

## 06/11 22:48 · sessao 7c8459a8b853 · FIO-E0-V4-E1 · censo · origem=Code

O `remover_motivo` citado é **`DisputaSupervisao.remover_motivo`, em `app/chamados/models.py:1900`**. Ele apaga perguntas em `app/chamados/models.py:1976`; a exclusão também alcança `RespostaAnterior` por `CASCADE`, definido em `app/chamados/models.py:3179`.

O relatório abaixo é um censo estático da cópia. Esquema instalado, execução das portas, existência das trilhas e passivo de produção: **[nao verificado]**.

**1. Decisões sobrescritas ou apagadas**

“Há trilha” abaixo significa que o código tenta produzi-la; não garante sua presença em produção. O escritor de eventos admite falha sem derrubar o ato, em `app/core/observ.py:94` e `app/core/observ.py:105`.

| Escritor | Campos gravados na mesma pergunta | Anterior preservado? |
|---|---|---|
| `responder_pergunta_disputa_core` | Substitui `resposta_colab` e `respondida_em`; limpa `validada_em/por`; pode retirar materialização sem lastro. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1300` | **Não cria RA.** Guarda a validação desfeita em evento, mas não fotografa a resposta substituída. O log contém a resposta nova truncada; a nota contém a nova resposta/display e é best-effort. `app/chamados/services/disputa_emissao.py:1309`, `app/chamados/services/disputa_emissao.py:1318`, `app/chamados/services/disputa_emissao.py:1355` |
| Escape no mesmo core | Substitui `recusa_motivo` por “Colaborador respondeu…”. `app/chamados/services/disputa_emissao.py:1340` | Sem fotografia do motivo anterior e sem RA nesse bloco. `app/chamados/services/disputa_emissao.py:1331`, `app/chamados/services/disputa_emissao.py:1342` |
| `responder_automaticamente` | Escreve texto e instante se o texto estava vazio; pode substituir um instante existente nesse meio-estado. `app/chamados/services/disputa_emissao.py:1431` | Sem RA ou evento próprio na função. `app/chamados/services/disputa_emissao.py:1422` |
| `carimbar_resposta_pela_trilha` | Preenche `respondida_em` somente quando nulo, usando o instante fornecido. `app/chamados/services/disputa_emissao.py:1448` | Não perde carimbo preenchido; registra instante/motivo em evento. `app/chamados/services/disputa_emissao.py:1452` |
| `_carimbar_validacao` | Atribui `validada_em=now()` e `validada_por=autor`. `app/chamados/services/validacao.py:131` | O fluxo normal recusa pergunta já validada e registra log, mas não cria RA por aceite. `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:440` |
| `devolver_validacao_recusada` | Limpa `validada_em/por`, conservando a resposta. `app/chamados/services/validacao.py:180`, `app/chamados/services/validacao.py:188` | Evento guarda antes/depois da assinatura, motivo e fechamento da disputa; sem RA. `app/chamados/services/validacao.py:205` |
| `recusar_pergunta` | Substitui `recusa_motivo`; delega a limpeza da resposta/validação à reabertura. `app/chamados/services/validacao.py:541`, `app/chamados/services/validacao.py:547` | Tem fotografia da resposta, instante e recusa anteriores; nota e evento. A reabertura cria RA, mas a chamada **não passa autor/motivo** para ela. `app/chamados/services/validacao.py:551`, `app/chamados/services/validacao.py:555`, `app/chamados/services/validacao.py:547` |
| `reabrir_para_correcao` | Limpa texto, resposta e validação; com `desfazer_absorcao=True`, limpa também materialização, autor e via. `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413` | **Cria RA antes do update, no mesmo atomic**, somente se havia texto ou carimbo de resposta. A fotografia não inclui materialização, via, pendência ou recusa. `app/chamados/services/disputa_emissao.py:1401`, `app/chamados/services/disputa_emissao.py:1406` |
| `_materializar_resposta_hora` | Substitui `pendencia_admin` e `recusa_motivo`. `app/chamados/services/materializacao.py:820`, `app/chamados/services/materializacao.py:825` | Sem RA ou fotografia anterior nesse bloco. `app/chamados/services/materializacao.py:822` |
| `_carimbar_recusa` | Troca `recusa_motivo` quando diferente; colisão também carimba `fato_ocupado`. `app/chamados/services/materializacao.py:1064`, `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1078` | Nota contém a recusa atual traduzida, não a anterior; depende de haver chamado-destino. Sem RA. `app/chamados/services/materializacao.py:1079` |
| `carimbar` | Substitui `materializada_em/por/via_resolucao`; pode preencher resposta/validação nulas. `app/chamados/models.py:2583`, `app/chamados/models.py:2587` | A fotografia serve para rollback **em memória**, não para histórico persistente. Sem RA. `app/chamados/models.py:2595`, `app/chamados/models.py:2609`, `app/chamados/models.py:2616` |
| `carimbar_identidade` | Troca carimbo/via e, opcionalmente, recusa. `manter_carimbo=True` conserva instante/autor, mas troca a via. `app/chamados/models.py:2636`, `app/chamados/models.py:2641` | Sem histórico próprio; preservação depende do chamador. `app/chamados/models.py:2625`, `app/chamados/models.py:2644` |
| `descarimbar` / `descarimbar_em_lote` | Limpam materialização, autor e via; o lote aceita campos adicionais. `app/chamados/models.py:2646`, `app/chamados/models.py:2691` | Sem RA ou trilha própria nessas primitivas. `app/chamados/models.py:2652`, `app/chamados/models.py:2699` |
| `encerrar_por_fato_no_espelho` | Substitui apenas a via por `fato_materializado`. `app/chamados/models.py:2707` | Sem fotografia anterior ou RA na função. `app/chamados/models.py:2702` |
| `relavrar` | Substitui via derivada e cache de veredito/relógio. `app/chamados/services/relavra_via.py:121` | Evento conserva via/veredito anteriores e novos; não conserva todos os carimbos nem cria RA. `app/chamados/services/relavra_via.py:130` |
| `conferir_data_do_fato` | Preenche `data_conferida_em/por`; não altera `data_evento`. `app/chamados/services/validacao.py:615` | Guarda justificativa e os dias comparados no log; carimbo já preenchido é recusado. Sem RA. `app/chamados/services/validacao.py:620` |
| `lavrar` / `gravar_lavras_em_lote` | Substituem `veredito_lavrado`, `julgado_em`, `janela_expira_em`. `app/chamados/veredito_lavrado.py:207`, `app/chamados/veredito_lavrado.py:227` | São cache regravável, sem sequência anterior por pergunta nessas funções. `app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:211` |

Os atos abaixo passam pelas primitivas anteriores; continuam relevantes porque escolhem **qual decisão sobrescrever ou retirar**:

| Ato delegado | Efeito e preservação |
|---|---|
| `fechar_por_admin` | Troca outra via por `admin_decidiu`; a guarda protege somente repetição do próprio carimbo. Sem RA. `app/chamados/models.py:2513`, `app/chamados/models.py:2522` |
| `retratar_duplicata` | Troca via/recusa para apontar a canônica; pode conservar instante/autor antigos. Não fotografa a via/recusa substituídas. `app/chamados/services/fio.py:891` |
| `ressuscitar_perguntas` | Limpa materialização, validação e resposta temporal do universo selecionado sem texto. Log conserva IDs/rótulos, não fotografia integral das assinaturas apagadas. `app/chamados/models.py:1434`, `app/chamados/models.py:1504`, `app/chamados/models.py:1518` |
| `encerrar_fantasma`, `reabrir_fantasma`, `reabrir_por_resposta`, `reabrir_por_competencia` | Carimbam ou retiram carimbos; não criam RA por esses atos. `app/chamados/models.py:2721`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/models.py:2756` |
| `desfazer_carimbo_sem_lastro` | Relê sob lock e descarimba; evento guarda instante/via anteriores, mas não o autor anterior da materialização. `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:229` |

Na **disputa**, os escritores adicionais de decisão/remoção são:

| Escritor | Perda e trilha |
|---|---|
| `remover_motivo` | Apaga perguntas de todo aquele motivo, inclusive respondidas; apaga RA por cascata. `contexto.revalidacoes` guarda evento/instante/motivo removido, não as perguntas apagadas. `app/chamados/models.py:1955`, `app/chamados/models.py:1976`, `app/chamados/models.py:3179` |
| `aparar_motivo_respondido` | Retira motivo e sua chave do contexto, conservando perguntas; sem fotografia própria do contexto anterior. `app/chamados/models.py:1893` |
| `fechar` | Grava `fechada_em/por`, `resolucao`, `motivo_fechamento`. Em desligamento/admin_encerra também encerra perguntas vivas por `admin_decidiu`; eventos não fotografam integralmente seus carimbos anteriores. `app/chamados/models.py:1852`, `app/chamados/models.py:1856`, `app/chamados/models.py:1861` |
| `reabrir_admin` / `reabrir_sistema` | Limpam fechamento, autor, resolução e motivo; admin incrementa contador. Nota anterior de resolução depende do receiver do fechamento chegar a copiá-la. `app/chamados/models.py:2063`, `app/chamados/models.py:2087`, `app/chamados/signals.py:42` |
| `limpar_resolucao_residual` | Apaga resolução de disputa aberta; tenta copiar o texto anterior para nota quando há chamado. `app/chamados/models.py:2170` |

O rol `motivos/contexto` também é regravado na emissão/agregação, sem versionamento integral nesses blocos: helper comum, agrupador de órfãos e âncora. Isso é distinto de registrar uma decisão sobre a resposta. `app/chamados/services/disputa_emissao.py:477`, `app/chamados/services/disputa_emissao.py:492`, `app/chamados/services/disputa_emissao.py:922`, `app/chamados/services/disputa_emissao.py:970`, `app/chamados/services/disputa_emissao.py:998`, `app/chamados/services/disputa_emissao.py:1089`, `app/chamados/services/disputa_emissao.py:1190`.

**Silêncio:** existe encerramento humano com motivo; ele é tratado como decisão sobre perguntas mudas e passa por `fechar`. A antiga expiração automática por conversa parada foi removida, segundo a lápide do comando. `app/chamados/models.py:1872`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`. Expiração da janela do juiz continua existindo por leitura do relógio, sem nascer uma decisão imutável equivalente. `app/chamados/juizes.py:1197`, `app/chamados/veredito_lavrado.py:95`.

**2. RespostaAnterior**

O modelo guarda:

| Campo | Papel |
|---|---|
| `pergunta` | FK com `CASCADE`. `app/chamados/models.py:3179` |
| `resposta_anterior` | Texto retirado da pergunta. `app/chamados/models.py:3180` |
| `respondida_original_em` | Instante da resposta retirada. `app/chamados/models.py:3181` |
| `validada_original_em/por` | Assinatura copiada da pergunta. `app/chamados/models.py:3182` |
| `versao_reaberta_em/por` | Instante e autor da reabertura. `app/chamados/models.py:3185` |
| `motivo_reabertura` | Texto até 200 caracteres. `app/chamados/models.py:3188` |

O único escritor encontrado é `reabrir_para_correcao`: `bulk_create` antes da limpeza, no mesmo `atomic`; só cria versão quando havia texto ou carimbo de resposta. Ordenação é por reabertura e PK descendentes. `app/chamados/services/disputa_emissao.py:1401`, `app/chamados/services/disputa_emissao.py:1406`, `app/chamados/models.py:3190`.

Os seis chamadores encontrados **omitem `por` e `motivo`**, deixando os defaults vazios: colaborador, view de reabertura, ação do fio, recusa, devolução da absorção e command de limbo. `app/chamados/models.py:2146`, `app/chamados/views.py:1436`, `app/chamados/services/acoes_disputa.py:103`, `app/chamados/services/validacao.py:547`, `app/chamados/services/materializacao.py:658`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`, `app/chamados/services/disputa_emissao.py:1362`.

Leitores encontrados: helper de consulta, prefetch do fio e modal. O modal lê texto, instante original, autores, instante e motivo da reabertura; `validada_original_em` é armazenado, mas não aparece nessa apresentação. `app/chamados/services/disputa_emissao.py:1723`, `app/chamados/services/fio.py:227`, `app/templates/chamados/partials/modal_fio.html:208`.

Para virar a sequência única pedida, faltam, por comparação com os escritores:

- Registro em **cada** resposta, aceite, recusa, reabertura, retratação e decisão de silêncio; hoje RA nasce somente na reabertura. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1406`, `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:545`, `app/chamados/models.py:1852`.
- Tipo do fato, decisão/resposta à qual ele se aplica, identidade da instância do marco e relógio próprio de cada decisão; esses campos não constam do esquema atual de RA. `app/chamados/models.py:3179`.
- Preservação de recusa, via e absorção; a fotografia atual copia somente texto, resposta e validação. `app/chamados/services/disputa_emissao.py:1407`.
- Proteção contra exclusão em cascata e contra alteração posterior; a classe/migration não estabelecem proteção append-only. `app/chamados/models.py:3179`, `app/chamados/models.py:3190`, `app/chamados/migrations/0050_resposta_anterior.py:16`.

O histórico atual também não resolve essa lacuna: reconstrói eventos pelos **carimbos e texto atuais** da pergunta, sem consultar RA. `app/chamados/services/historico.py:96`.

**3. Identidade e acha-ou-cria**

Há **um nascimento central efetivo**, `cls.objects.create`, precedido de `.last()` por `(colaborador via disputa, motivo, data_evento)`. Célula e destino não integram essa chave; uma pergunta reaproveitada conserva sua disputa original. `app/chamados/models.py:2392`, `app/chamados/models.py:2431`, `app/chamados/models.py:2435`, `app/chamados/models.py:2450`, `app/chamados/models.py:2476`.

| Sítio de produção | Busca/chave local antes de `abrir` |
|---|---|
| `gerar_perguntas_disputa` | Sem data: disputa+motivo; datadas: disputa+motivo+dia. Depois aplica o dedup global do modelo. `app/chamados/services/disputa_emissao.py:299`, `app/chamados/services/disputa_emissao.py:370`, `app/chamados/services/disputa_emissao.py:386`, `app/chamados/services/disputa_emissao.py:389` |
| `_criar_pergunta_se_couber` | `_ja_perguntou` usa disputa+motivo+dia e vitalidade; depois `abrir`. `app/chamados/services/disputa_emissao.py:765`, `app/chamados/services/disputa_emissao.py:770`, `app/chamados/services/disputa_emissao.py:1237` |
| Emissão de âncora | Procura destino+`ancora_ausente` não materializada; chama `abrir` sem data. `app/chamados/services/disputa_emissao.py:1149`, `app/chamados/services/disputa_emissao.py:1195` |
| Reabertura administrativa | Escolhe última disputa fechada do chamado; cria confirmação sem data, sem dedup próprio de pergunta. `app/chamados/services/acoes_disputa.py:252`, `app/chamados/services/acoes_disputa.py:259` |
| Declaração do colaborador | Primeira reutilizável por disputa+motivo+dia; se nenhuma, `abrir`. `app/chamados/services/declaracao_texto.py:226`, `app/chamados/services/declaracao_texto.py:313`, `app/chamados/services/declaracao_texto.py:325` |
| Aplicação de declaração pelo admin | Mesma candidata reutilizável; fallback para `abrir`. `app/chamados/services/declaracao_texto.py:611`, `app/chamados/services/declaracao_texto.py:614` |

São **seis chamadores diretos do nascimento central**, nos endereços acima. Não foi encontrada chamada concreta de `get_or_create/update_or_create` para essa entidade; completude além da busca estática: **[nao verificado]**.

A conversa é uma exceção relevante: `data_evento=None` na entrada desliga o dedup e vira `timezone.localdate()` antes de gravar. Assim, consultas posteriores não distinguem por esse campo uma conversa de uma pergunta datada do mesmo dia. `app/chamados/models.py:2424`, `app/chamados/models.py:2476`.

**Unicidade:** não há constraint composta da pergunta declarada no `Meta`; a migration criadora também não a estabelece. Há PK/FKs e índices simples do cache, que não protegem a identidade solicitada. Índices/constraints realmente instalados em produção: **[nao verificado]**. `app/chamados/models.py:2857`, `app/chamados/migrations/0014_perguntadisputa.py:19`, `app/chamados/migrations/0014_perguntadisputa.py:30`, `app/chamados/models.py:2346`.

**Locks:** não existe trava da identidade dentro de `abrir`; seu `.last()→create` permanece separado. O `atomic` de `save` protege cada gravação, não torna a busca anterior exclusiva. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`, `app/chamados/models.py:2853`.

| Caminho | Proteção encontrada |
|---|---|
| Helper de disputa | Trava disputa **existente** por colaborador+aberta; a geração normal acontece depois do retorno do helper. `app/chamados/services/disputa_emissao.py:466`, `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:605` |
| Declarações | Têm atomic externo e usam o helper de disputa; isso não substitui unicidade da pergunta nem cobre a ausência de uma disputa existente. `app/chamados/services/declaracao_texto.py:302`, `app/chamados/services/declaracao_texto.py:310`, `app/chamados/services/declaracao_texto.py:605`, `app/chamados/services/disputa_emissao.py:467` |
| Âncora | Atomic e lock de disputa existente; teste de pergunta ocorre antes desse lock. `app/chamados/services/disputa_emissao.py:1148`, `app/chamados/services/disputa_emissao.py:1149`, `app/chamados/services/disputa_emissao.py:1156` |
| Agrupador de órfãos | Atomic, mas seleção da disputa sem `select_for_update`. `app/chamados/services/disputa_emissao.py:898`, `app/chamados/services/disputa_emissao.py:935` |
| Reabertura admin | Sem lock próprio nessa sequência de seleção/reabertura/nascimento. `app/chamados/services/acoes_disputa.py:252`, `app/chamados/services/acoes_disputa.py:259` |

A janela estática da corrida está comprovada; sua reprodução concorrente em banco: **[nao verificado]**. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`.

**Instância de marco candidata:** o dado já oferece as chaves `hi/hii/hfi/hii2/hfi2/hf`, qualificadas pela célula, com ordem canônica e exposição `(tipo, chave, hora)`. A pergunta ainda não persiste essa chave. Usar célula+chave é uma inferência de encaixe; estabilidade através de regeneração do DNA: **[nao verificado]**. `app/escala/models.py:1652`, `app/escala/models.py:1659`, `app/escala/models.py:1672`, `app/chamados/models.py:2209`.

Hoje há perda concreta dessa distinção: declaração reduz marcos internos a ida/volta pelo tipo, e o juiz admite ambiguidade com duas pausas. Cartório e pauta já consomem o vocabulário de marcos da célula. `app/chamados/services/declaracao_texto.py:118`, `app/chamados/services/declaracao_texto.py:300`, `app/chamados/juizes.py:1106`, `app/ponto/services/cartorio.py:586`, `app/pautas/services.py:417`.

**4. Absorção**

A ligação atual é **reconstruída**, não uma FK/fato explícito do ato: o materializador obtém a ausência, captura um `agora` e tenta carimbar as perguntas-hora da mesma disputa/dia com esse instante. `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:549`.

O leitor:

1. Restringe colaborador, dia, tipo de pergunta e vias de absorção. `app/chamados/services/materializacao.py:605`.
2. Procura uma resposta de escape compatível com o tipo da ausência e extrai `(disputa_id, materializada_em)`. `app/chamados/services/materializacao.py:602`, `app/chamados/services/materializacao.py:609`.
3. Recupera as irmãs daquele grupo e exclui perguntas com batida-origem viva. `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:616`.

| Campo | Outros escritores |
|---|---|
| `disputa` | Nascimento em `abrir`; não foi encontrada transferência posterior. Reapontamento muda destino, não disputa. `app/chamados/models.py:2476`, `app/chamados/services/fio.py:845` |
| `data_evento` | Nascimento; reapontamento preenche somente quando estava vazia. `app/chamados/models.py:2476`, `app/chamados/services/fio.py:832`, `app/chamados/services/fio.py:847` |
| `materializada_em` | Atribuído pelo helper de carimbo; persistido por `carimbar`, `carimbar_ide

[truncado em 20000 de 36686 caracteres]

## 06/11 22:46 · sessao ee0df63a3e82 · FIO-E0-V4-E2 · censo · origem=Code

**O censo mostra cinco respostas de escopos diferentes:** existência de chamado, vitalidade de pergunta, necessidade de emitir pergunta, alcance pela fila e atribuição fora do fio. Nenhuma delas, isoladamente, responde por todas as ações exigíveis de cada instância de marco. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1135`, `app/chamados/services/fabrica_da_celula.py:74`, `app/chamados/juizes.py:1341`, `app/chamados/services/vigia_fio.py:274`)

**A bateria C-E não fecha nesta cópia.** As referências do aval a `BACKLOG:1797/1813/1821` apontam aqui para outras obras; localização e descrição individual dos dez casos FIO-E0: **[nao verificado]**. (`app/docs/BACKLOG.md:1797`, `app/docs/BACKLOG.md:1813`, `app/docs/BACKLOG.md:1821`)

**1. Os cinco respondedores**

**`cobranca_viva_do_dia(colaborador, modulo_origem, dia, *, excluir=None)`**

- **Responde:** `ChamadoColaborador | None`; escolhe o chamado mais novo dos módulos pedidos, em `VIVOS`, cujo dia forte coincide com `dia`. Não confere marco faltante, efeitos, ausência ou silêncio. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1894`)
- **Lê:** chamado `colaborador`, `modulo_origem`, `status_local`, `contexto_json.data_turno`, `celula.data`, `criado_em`, `pk`; o desempate é `-criado_em,-pk`. O dia forte prioriza a célula e depois as informações declaradas do chamado. (`app/chamados/juizes.py:1897`, `app/chamados/juizes.py:80`)
- **Relógio:** recebe o dia; esse percurso não consulta o instante atual. `criado_em` ordena candidatos. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1899`)
- **Todos os chamadores diretos de produção encontrados**, incluindo alias:

| Chamador | Local da chamada |
|---|---|
| `ChamadoColaborador._plano_pergunta_viva` | `app/chamados/models.py:373` |
| `ChamadoColaborador._gate_premissa_do_fechamento` | `app/chamados/models.py:711` |
| `_julgar_colab_corpo`, via `_cobr_c8` | `app/ponto/services/cartorio.py:1604` |
| `resolver_cobranca_da_entrada` | `app/ponto/services/entrada_sincronizada.py:28` |
| `_autoresolve_f2_f4` | `app/ponto/services/triagem_batida.py:589` |

**`pergunta_viva(p)`**

- **Responde:** booleano, delegando a `_pergunta_viva_calc`, com memo `_pv_memo` escrito na instância. Portanto, não é pura pelos argumentos explícitos: consulta outras fontes e relógios internos. (`app/chamados/juizes.py:822`, `app/chamados/juizes.py:835`, `app/chamados/juizes.py:1197`)
- **Ordem decisiva:** validação mata; tranca aplicável mata; resposta com texto permanece viva salvo decisão administrativa/concordância com a luz; identidade materializada encerra; materialização com marco aceso encerra; célula acusadora mantém viva; depois entram teto, classificação da falta e conferência de materialização de fato. (`app/chamados/juizes.py:1135`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1194`)
- **Lê diretamente:** pergunta `validada_em`, `respondida_em`, `resposta_colab`, `via_resolucao`, `materializada_em`, `data_evento`, `tipo_resposta`, `motivo`, `celula`, `disputa.colaborador`, `chamado_destino`; célula `veredito`, `veredito_via`, `dna.marcos`, `ata.lampadas` — `hora`, `tipo`, `acesa`, `luz`, pendência temporal. (`app/chamados/juizes.py:1135`, `app/chamados/juizes.py:886`, `app/chamados/juizes.py:1099`)
- **Lê transitivamente:** empresa/corte da competência, tranca e toque humano posterior; batidas ligadas à pergunta, retratação, slot esperado, batida no slot, turnos, ausências, vínculo do dia e estado do destino em um ramo legado. (`app/chamados/juizes.py:935`, `app/chamados/juizes.py:1065`, `app/chamados/juizes.py:718`, `app/chamados/juizes.py:743`, `app/escala/servico_jornada.py:340`)
- **Relógios ocultos:** `_dentro_da_janela_viva` usa `date.today()`; `classificar_falta` usa `_tz.now()` contra o fim do turno. A assinatura não recebe `agora`. (`app/chamados/juizes.py:556`, `app/chamados/juizes.py:562`, `app/escala/servico_jornada.py:375`)

**Todos os chamadores diretos de produção encontrados**, com aliases resolvidos:

| Arquivo | Chamadores e linhas das chamadas |
|---|---|
| `app/chamados/juizes.py` | `pergunta_sem_lastro_ou_viva:1225`; `perguntas_vivas_de:1230`; `fio_vivo:1242`; `alimentar_portas:1302`; `perguntas_respondidas_a_validar:1403`; `_dias_das_perguntas_vivas:2031` |
| `app/chamados/models.py` | `_vivas_entre:351`; `_universo_da_ressuscitacao:1458`; `pendentes_do_colaborador:1739`; `DisputaSupervisao.fechar:1837`; `PerguntaDisputa.estado:2502`; `carimbar:2596`; `viva:2809` |
| `app/chamados/views.py` | `responder_pergunta_disputa:1242,1257`; `reabrir_disputa_supervisao:1417` |
| `app/api/views.py` | `api_disputa_responder:2631` |
| `app/chamados/relatorios_disputa.py` | `classificar:18` |
| `app/chamados/reconciliador.py` | `lavrar_sem_turno_aberto:398`; `perguntas_absolvidas_pela_celula:651`; `_viva_em_competencia_aberta:748`; `veredito_orfa:1075`; `reatar_pergunta_orfa:1579` |
| `app/colaboradores/services/calendario.py` | `contexto_calendario:476` |
| `app/ponto/services/dia_do_colab.py` | `dia_do_colab:71` |
| `app/chamados/services/fio.py` | `montar_chamados_fio:234`; `processar_post_fio:602`; `_costura_nao_mata_o_fio:775`; `migrar_resolvedoras_vivas:932,935` |
| `app/chamados/services/pergunta_no_app.py` | `perguntas_do_app:53`; `_marco_ja_perguntado:111` |
| `app/chamados/services/acoes_chamado.py` | `montar_contexto_detalhe:348` |
| `app/chamados/services/disputa_emissao.py` | `resolver_perguntas_por_chamado_resolvido:1597,1679` |
| `app/chamados/services/competencia_trancada.py` | `medir:122,132,142` |
| `app/chamados/services/relavra_via.py` | `desfazer_carimbo_sem_lastro:227` |
| `app/chamados/services/aviso_pendencia.py` | `perguntas_pendentes_do_colab:13` |
| `app/chamados/services/vigia_fio.py` | `viva_em_disputa_fechada:755`; `carimbos_sem_lastro_por_dono:813` |
| `app/chamados/management/commands/disparar_perguntas_competencia.py` | `handle:148` |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py` | `handle:83` |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | `_contraprova_f1t:202`; `_i6_pares:540`; `handle:858,894,1650,1707,1766,1779` |

A tabela identifica chamadas da função. Há também entrada indireta por `.viva`, que delega integralmente ao juiz, e por `.estado`; eliminar apenas os imports explícitos deixaria essas entradas ativas. (`app/chamados/models.py:2492`, `app/chamados/models.py:2798`)

**`motivos_a_perguntar(cel, hoje, agora=None, *, carimbadas=None, julgada_antes=_JULGADA_DA_CELULA)`**

- **Responde:** lista de motivos que ainda precisam ser emitidos; opcionalmente acrescenta IDs à lista `carimbadas`. Não devolve todas as ações pendentes de perguntas existentes. (`app/chamados/services/fabrica_da_celula.py:74`, `app/chamados/services/fabrica_da_celula.py:123`)
- **Lê:** célula/colaborador — `data`, `trabalha`, `ata`, `veredito`, `julgada_em`, `situacao`; dia sem trabalho; marcos vencidos; parcial destinada à lista administrativa; perguntas anteriores do colab-dia. (`app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/fabrica_da_celula.py:118`)
- **Deduplicação atual:** qualquer pergunta anterior do mesmo motivo bloqueia emissão, exceto `VIA_DUPLICATA`. Não consulta vitalidade nem discrimina a instância do marco. Duas pausas podem ser comprimidas no mesmo motivo. (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:59`)
- **Entradas transitivas:** `_ata_do_dia`/`_marcos_faltantes`; vínculo e marcos encadeados; `parcial_so_lista`; julgamento anterior e fatos novos de batida, ausência, justificativa, edição/regeneração da célula ou vencimento de marco. (`app/chamados/services/disputa_emissao.py:818`, `app/chamados/services/declaracao_texto.py:88`, `app/ponto/services/cartorio.py:329`, `app/ponto/services/cartorio.py:370`)
- **Relógio:** `hoje` e `agora` por parâmetro; sem `agora`, usa `timezone.localtime()`. O ramo sem ata/marcos pode chamar `classificar_falta`, que ainda lê seu próprio `now()`. (`app/chamados/services/fabrica_da_celula.py:93`, `app/chamados/services/fabrica_da_celula.py:55`, `app/escala/servico_jornada.py:377`)
- **Únicos chamadores diretos encontrados:** `perguntar_pela_celula` e `motivos_que_o_evento_deixou`. (`app/chamados/services/fabrica_da_celula.py:164`, `app/chamados/services/fabrica_da_celula.py:199`)

**`porta_alcancavel_do_dia(celula_id, *, alimentacao=None)`**

- **Responde:** `None` ou `{rota, fila, verbos, fio}`. Soma chamado da célula na fila com pergunta viva no app, sob âncora na fila e sem resposta carimbada vazia. O fio prioriza `cobrar→colab`, depois `validar→admin`, senão `chamado`. (`app/chamados/juizes.py:1327`, `app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1375`)
- **Lê:** partição global dos chamados vivos, pertença à fila global, perguntas da célula julgadas pelo C6, célula→colaborador e perguntas do app por colaborador. Chamados fornecem `id`, `modulo_origem`, `silenciado_ate`, `criado_em`, `urgencia`; perguntas fornecem resposta, destino e chamado da disputa, além das entradas transitivas do C6. (`app/chamados/juizes.py:1282`, `app/chamados/juizes.py:1309`, `app/chamados/services/verbos.py:44`, `app/chamados/services/fio.py:116`)
- **Relógio:** não recebe instante; `particionar_verbos` e `qs_na_fila` fazem leituras distintas de `timezone.now()`. (`app/chamados/services/verbos.py:35`, `app/chamados/services/painel.py:188`)
- **Todos os chamadores diretos encontrados:** `censo_alcance`, `cadastro_realidade.beco_em_lote` e `fora_da_folha._porta`. Esta última atende `_fio_do_dia` e a lista de chamados de `o_que_segura`. (`app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:460`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:225`)

**`dono_fora_do_fio(colab, data, perguntas, lista, com_papel=False, *, oferecido)`**

- **Responde, nesta ordem:** `decisao_do_dia_pendente`, `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia`, `fio_do_admin` ou `None`; com `com_papel=True`, devolve também o papel. (`app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:304`)
- **Lê:** `oferecido`; ausência cobrindo a data para cobrança e status em `ABERTAS`; perguntas com `estado==VALIDADA` e resposta `FOLGA_CONTESTA`; presença e dono do dia na lista administrativa. (`app/chamados/services/vigia_fio.py:306`, `app/chamados/services/validacao.py:216`)
- **Relógio:** não lê diretamente. O calendário que produz `oferecido` lê seu instante; a lista recebe `hoje/agora` e os repassa ao carregamento. (`app/colaboradores/services/calendario.py:159`, `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`)
- **Todos os chamadores diretos encontrados:** `censo` e `censo_alcance`. (`app/chamados/services/vigia_fio.py:431`, `app/chamados/services/vigia_fio.py:654`)

**2. União das entradas**

| Família | Fatos e insumos necessários evidenciados pelo código |
|---|---|
| **Pessoa e contexto** | Identidade, situação, empresa e corte da competência; vínculo do dia, inclusive geradora da célula e tipo da escala. (`app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/juizes.py:931`, `app/ponto/services/cartorio.py:840`) |
| **Célula e instâncias de marco** | Data do turno, `trabalha`, DNA e geradora, sequência dos marcos com tipo/hora e encadeamento noturno, edição/regeneração. Hoje a pergunta mapeia motivo→chave e, nas pausas múltiplas, agrupa várias luzes; isso não fornece identidade individual suficiente. (`app/chamados/juizes.py:867`, `app/chamados/juizes.py:893`, `app/ponto/services/cartorio.py:365`, `app/ponto/services/cartorio.py:845`) |
| **Batidas e efeitos** | Universo apurável, timestamp, tipo, associação ao turno/marco, pergunta de origem, criação e retratação; conferência do slot e do efeito real. A porta apurável exclui `retratada_em` preenchida. (`app/ponto/turnos.py:2844`, `app/chamados/juizes.py:718`, `app/ponto/services/cartorio.py:352`) |
| **Ausências e justificativas** | Tipo, status, faixa de datas, período/minutos da parcial e decisões/carimbos; distinguir cobertura do dia inteiro e do resto exigível. (`app/ponto/turnos.py:2536`, `app/ponto/services/cartorio.py:355`, `app/ponto/services/cartorio.py:370`) |
| **Decisões da pergunta/dia** | Resposta, validação, recusa/reabertura/retração, via, instante, alvo e decisão administrativa; conferir os efeitos ligados à decisão. Hoje são lidos campos correntes e `RespostaAnterior`, não está demonstrada uma sequência única imutável de decisões E1. (`app/chamados/juizes.py:1135`, `app/chamados/services/fio.py:216`, `app/chamados/models.py:2513`; sequência E1 completa: **[nao verificado]**) |
| **Chamados e portas** | Módulo, célula/dia, estado, silêncio, âncora/destino e chamado da disputa; universo global relevante ao crédito; verbos, presença no app, “Resolver dia” e lista administrativa por dia/dono. (`app/chamados/juizes.py:1282`, `app/chamados/juizes.py:1378`, `app/chamados/services/vigia_fio.py:258`, `app/chamados/services/vigia_fio.py:313`) |
| **Competência e atos aplicáveis** | Tranca, instante da tranca e toque humano posterior sobre o colab-dia. Hoje esse toque depende de `LogAuditoria`; não foi consultado dado de log nesta análise. (`app/chamados/juizes.py:935`, `app/chamados/juizes.py:954`, `app/chamados/juizes.py:1065`) |
| **Tempo** | Um `agora` para vencimento dos marcos, silêncio e janela; `dia_do_turno` deve conservar o eixo da jornada, inclusive meia-noite. (`app/ponto/services/cartorio.py:831`, `app/chamados/services/verbos.py:48`, `app/ponto/turnos.py:3013`) |

**Dependências atuais de estado/projeção, distintas dos fatos de origem:**

- **`veredito_lavrado`: dependência direta da porta, pelo crédito dos verbos.** O crédito exclui pergunta materializada cujo veredito gravado é `VL_SIM`; o código admite leitura desse campo desatualizado até o cron. (`app/chamados/services/raiox.py:110`, `app/chamados/services/raiox.py:133`)
- **`CelulaDia.veredito`, `veredito_via`, ata e julgamento anterior:** influenciam vitalidade, emissão e encaminhamento da parcial. São resultados/carimbos já gravados usados na decisão atual. (`app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1181`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`)
- **`materializada_em` e `via_resolucao`:** têm função de encerramento, inclusive por identidade; validação encerra o C6 antes da conferência do efeito. Não se pode tratar todo `materializada_em` como prova de batida. (`app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1207`)
- **`.estado` não é coluna persistida:** é propriedade derivada de validação, materialização, resposta e C6. Mesmo assim, `dono_fora_do_fio` depende do rótulo exato `VALIDADA`. (`app/chamados/models.py:2492`, `app/chamados/services/vigia_fio.py:311`)
- **Estado dos chamados:** o C8 usa `status_local`; a porta usa esse estado mais silêncio e crédito. Um ramo da prova de materialização ainda consulta destino encerrado. (`app/chamados/juizes.py:1897`, `app/chamados/services/painel.py:186`, `app/chamados/juizes.py:743`)

**3. `CargaDaJanela` e carregamento por janela**

**Existe:** `CargaDaJanela(colaboradores, ini, fim)`, em `app/ponto/turnos.py:2922`. Carrega células com pad, geradoras, candidatos/vínculos, tipos e atas; carrega batidas apuráveis e última batida sob demanda; memoriza turnos por colaborador. Oferece `turnos`, `dias_encerrados` e `dia_das_batidas`. (`app/ponto/turnos.py:2936`, `app/ponto/turnos.py:2959`, `app/ponto/turnos.py:2974`, `app/ponto/turnos.py:3001`)

**Os nomes literais `precedencia_cobertura_teto` e `portas_com_universo_global` não foram localizados como funções nesta cópia: [nao verificado].** As composições existentes são:

| Composição atual | Granularidade e consultas |
|---|---|
| Precedência | `fatos_do_dia` delega ao período `[dia,dia]`; `fatos_do_periodo` já atende uma janela por colaborador, mas consulta ausências e folgas desse colaborador. (`app/ponto/precedencia.py:242`, `app/ponto/precedencia.py:259`, `app/ponto/precedencia.py:273`) |
| Cobertura | `ausencia_cobre` faz consulta pontual por colab-dia; `cobertura_ausencia_periodo` já carrega a janela por colaborador com o mesmo desempate. (`app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2733`) |
| Teto | `vencidos_no_instante` chama `marcos_vencidos` por célula/dia. Há tratamento próprio para passado sem marcos; não equivale a simplesmente testar “dia encerrado”. (`app/chamados/services/fabrica_da_celula.py:15`, `app/ponto/services/cartorio.py:831`) |
| Portas globais | `alimentar_portas` particiona `qs_vivos()` global e lê `qs_na_fila()` global, depois agrupa perguntas/células pedidas. Sem alimentação, a porta abre esse carregamento para uma célula. (`app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1290`, `app/chamados/juizes.py:1375`) |

**Hoje os leitores principais das portas já reutilizam lote:** vigia, Cadastro×Realidade e folha passam alimentação; não há justificativa estática para chamar novamente a partição global a cada colab-dia. Permanecem consultas internas do C6 e carregamento do app por colaborador. (`app/chamados/services/vigia_fio.py:604`, `app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:456`, `app/folha/fora_da_folha.py:218`, `app/chamados/juizes.py:1260`, `app/chamados/juizes.py:1309`)

**Para carregar por janela — inferência de implementação:** ampliar a alimentação com precedência/cobertura/teto, perguntas e decisões por instância, efeitos e portas; realizar uma partição global compartilhada; fornecer o mesmo `agora`; impedir que o corpo puro volte ao ORM, ao relógio ou ao memo de derivação antigo. Os pontos atuais a remover do corpo estão evidenciados nas consultas da precedência, cobertura, C6 e portas. (`app/ponto/precedencia.py:273`, `app/ponto/turnos.py:2552`, `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:1282`)

**Custo medido nesta cópia, 2,95 queries/colab-dia, meta ≤1 e complexidade efetiva da frota: [nao verificado].** O anexo os fornece como referência/meta; o censo estático não prova esses resultados.

**4. Divergências entre os cinco**

São contraexemplos permitidos pelos predicados; ocorrência e quantidade em produção: **[nao verificado]**.

| Mesmo cenário de colab-dia | Respostas diferentes pelo código |
|---|---|
| Único chamado `registrado` ou com silêncio vigente; nenhuma outra porta | C8 pode devolver o chamado porque usa `VIVOS`; porta pode devolver `None`, pois a fila exige `COBRANCA` e exclui silêncio futuro. (`app/chamados/juizes.py:1897`, `app/chamados/services/painel.py:186`, `app/chamados/juizes.py:1382`) |
| Pergunta respondida com texto, sem validação/tranca/decisão que a encerre, sob âncora terminal | C6 pode dizer viva; porta rejeita a âncora fora da fila. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1378`) |
| Pergunta com `respondida_em`, texto vazio e célula acusadora, sem outra porta | C6 pode dizer viva; porta rejeita `respondida_sem_texto`. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1254`, `app/chamados/juizes.py:1380`) |
| Marco continua faltante, pergunta já validada sem plantio | C6 diz morta imediatamente; fábrica não emite porque o motivo já existe; a falta pode permanecer na ata. (`app/chamados/juizes.py:1136`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/declaracao_texto.py:109`) |
| Próprio marco aceso, outro marco do dia faltante, pergunta sem materialização | C6 pode continuar viva pelo veredito acusador do dia: nesse ramo, a luz do próprio marco só encerra com `materializada_em`. A fábrica não reemite o motivo existente. (`app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1181`, `app/chamados/services/fabrica_da_celula.py:123`) |
| Competência trancada, sem

[truncado em 20000 de 27062 caracteres]

## 06/11 22:44 · sessao 7735e49f02c8 · - · - · origem=Ronald (tmux)

Censo de portas fechado por regra explícita: contar **funções que executam a persistência da pergunta** (`save`, `update`, `create`, `delete` ou `bulk_update`), sem contar novamente o helper que só atribui em memória, o chamador que apenas delega, nem ramos/arestas como portas diferentes. O código separa atribuição em memória e persistência em `app/chamados/models.py:2556-2560`, `app/chamados/models.py:2585-2609`, `app/chamados/veredito_lavrado.py:197-207`.

**Resultado: 15 funções para ciclo/estado derivado; 17 incluindo cache; 21 incluindo estado operacional de recusa, pendência e conferência.** São os sítios enumerados abaixo; **“16 portas” do aval não é o total desse recorte vivo**. A lista P13 atual enumera campos/arquivos, não 16 funções, e seu próprio comentário histórico distingue contagem por arquivo de contagem por mão. `app/core/tests/test_contract_escritores_estado.py:61-82`, `app/core/tests/test_contract_escritores_estado.py:83-103`, `app/core/tests/test_contract_escritores_estado.py:133-173`, `app/chamados/services/validacao.py:127-133`.

Não existe `status` persistido em `PerguntaDisputa`: `estado` deriva quatro rótulos de `validada_em`, `materializada_em`, `respondida_em` e `pergunta_viva`. Adotei P=pendente, A=respondida, V=validada, R=resolvida. A fórmula exata é: com validação, V/R conforme exista materialização; sem validação, se viva, P/A conforme exista resposta; se morta e materializada, R; caso contrário P/A. `app/chamados/models.py:2377-2378`, `app/chamados/models.py:2491-2506`.

| # | Persistência de ciclo/estado | Origem → destino permitido pelo código |
|---|---|---|
| 1 | `PerguntaDisputa.abrir` | inexistente → normalmente P; reaproveitada → estado recalculado pela nova célula/destino; `**campos` pode alterar os valores iniciais. `app/chamados/models.py:2398-2405`, `app/chamados/models.py:2430-2477` |
| 2 | `PerguntaDisputa.carimbar` | carimba materialização/via, podendo preencher resposta e validação; só persiste se o C6 disser morta depois da aplicação, portanto destino R. Recusa desfaz em memória e não persiste. `app/chamados/models.py:2562-2614` |
| 3 | `PerguntaDisputa.carimbar_identidade` | estado anterior → estado resultante da via de identidade; não garante R: resposta não validada ainda pode manter A. Mantém instante/autor antigos quando `manter_carimbo=True`. `app/chamados/models.py:2625-2644` |
| 4 | `PerguntaDisputa.descarimbar` | remove materialização/autor/via → V se validação permanecer; caso contrário P/A segundo `respondida_em`. `app/chamados/models.py:2646-2653`, `app/chamados/models.py:2499-2506` |
| 5 | `PerguntaDisputa.descarimbar_em_lote` | mesma retirada dos três campos, mais `outros` definidos pelo chamador; destino depende desses campos adicionais. `app/chamados/models.py:2691-2700` |
| 6 | `PerguntaDisputa.encerrar_por_fato_no_espelho` | troca apenas a via por `fato_materializado`; normalmente pergunta já materializada, podendo mudar vitalidade/rotulagem sem mudar instante. `app/chamados/models.py:2702-2711` |
| 7 | `_carimbar_validacao` | estado sem validação → V se sem materialização, R se materializada. A guardas de resposta/validação anterior estão em `validar_pergunta`, não nesse helper. `app/chamados/services/validacao.py:127-133`, `app/chamados/services/validacao.py:363-368`, `app/chamados/models.py:2499-2501` |
| 8 | `devolver_validacao_recusada` | V → normalmente A; só desfaz validação de resposta de hora não materializada com motivo de recusa. Também pode reabrir a disputa. `app/chamados/services/validacao.py:155-190`, `app/chamados/services/validacao.py:195-212` |
| 9 | `responder_pergunta_disputa_core` | P/A/V/R → resposta nova, instante novo, validação retirada; remove materialização somente das vias sem lastro. Destino normalmente A, podendo permanecer R se materialização sobreviver e C6 disser morta. `app/chamados/services/disputa_emissao.py:1282-1301`, `app/chamados/models.py:2735-2749`, `app/chamados/models.py:2499-2506` |
| 10 | `reabrir_para_correcao` | limpa texto/resposta/validação; com `desfazer_absorcao=True` limpa também materialização/via/autor. Nesse ramo → P. No padrão, materialização pode sobreviver: o destino P prometido pelo comentário não é incondicional pela fórmula de `estado`. `app/chamados/services/disputa_emissao.py:1386-1418`, `app/chamados/models.py:2499-2506` |
| 11 | `responder_automaticamente` | pergunta sem texto → texto+instante; normalmente P→A. Não limpa validação/materialização anteriores; não atua quando já há texto. `app/chamados/services/disputa_emissao.py:1422-1436` |
| 12 | `carimbar_resposta_pela_trilha` | texto existente sem instante → instante da nota; normalmente P→A. Já carimbada ou sem texto é no-op. `app/chamados/services/disputa_emissao.py:1439-1455` |
| 13 | `reapontar_resolvedora` | destino antigo→novo; data nula→data fornecida; relavra cache no mesmo `save`. Não altera resposta/validação/materialização, mas muda insumos da vitalidade. `app/chamados/services/fio.py:829-849` |
| 14 | `relavrar` | via derivada antiga→via atual; relavra cache junto. Com materialização e sem validação, pode alterar R↔P/A pela vitalidade; carimbos de resposta/validação/materialização permanecem. `app/chamados/services/relavra_via.py:104-126`, `app/chamados/models.py:2499-2506` |
| 15 | `DisputaSupervisao.remover_motivo` | pergunta existente→apagada; inclui suas `RespostaAnterior` por cascade. `app/chamados/models.py:1976`, `app/chamados/models.py:3179` |

As duas portas adicionais de **cache** são `lavrar`, que grava `veredito_lavrado`, `julgado_em`, `janela_expira_em`, e `gravar_lavras_em_lote`, que executa o `bulk_update`; o command `backfill_veredito_lavrado` apenas prepara os valores e delega a gravação. Elas não mudam diretamente a property `estado`, mas mudam o estado materializado usado pelos leitores SQL. `app/chamados/veredito_lavrado.py:194-231`, `app/chamados/management/commands/backfill_veredito_lavrado.py:134-140`.

As quatro portas adicionais de **estado operacional** são:

| # | Porta | Origem → destino |
|---|---|---|
| 18 | `_materializar_resposta_hora` | pendência/motivo anteriores → pendência `tipo_nao_conferido` e motivo produzidos pelo cálculo. A mesma função também usa a porta de identidade para paridade. `app/chamados/services/materializacao.py:820-826`, `app/chamados/services/materializacao.py:899` |
| 19 | `_carimbar_recusa` | motivo anterior → motivo atual; na colisão usa também `carimbar_identidade(VIA_FATO_OCUPADO)`. `app/chamados/services/materializacao.py:1058-1077` |
| 20 | `recusar_pergunta` | motivo anterior→recusa humana, depois delega a retirada da resposta/validação para `reabrir_para_correcao`. `app/chamados/services/validacao.py:537-558` |
| 21 | `conferir_data_do_fato` | divergente não conferida→conferida; grava instante/autor, liberando a materialização. Não muda `data_evento`. `app/chamados/services/validacao.py:611-628` |

**Wrappers semânticos separados da contagem acima:** há 16 funções que chamam diretamente as primitivas `carimbar`, `carimbar_identidade`, `descarimbar` ou `descarimbar_em_lote`, excluídas as próprias primitivas e `reabrir_para_correcao`, que já persiste diretamente no ramo padrão. Esse 16 é uma contagem de **chamadores imediatos**, não 16 escritores independentes. Dois deles, `_materializar_resposta_hora` e `_carimbar_recusa`, já aparecem no recorte operacional por outras escritas próprias. `app/chamados/models.py:1504`, `app/chamados/models.py:2522`, `app/chamados/models.py:2721-2756`, `app/chamados/services/materializacao.py:825-826`, `app/chamados/services/materializacao.py:1074-1078`.

| Wrapper | Ato escolhido e referência |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | Limpa carimbo, validação e resposta temporal das perguntas sem texto selecionadas pelo C6; →P. `app/chamados/models.py:1434-1458`, `app/chamados/models.py:1492-1504` |
| `PerguntaDisputa.fechar_por_admin` | Via `admin_decidiu`; idem mesma via+carimbo é no-op. `app/chamados/models.py:2508-2523` |
| `PerguntaDisputa.encerrar_fantasma` | Via `fantasma`, sem tocar texto; C6 pode recusar. `app/chamados/models.py:2713-2721` |
| `PerguntaDisputa.reabrir_fantasma` | Retira exclusivamente carimbo de fantasma. `app/chamados/models.py:2723-2733` |
| `PerguntaDisputa.reabrir_por_resposta` | Retira somente carimbo das vias sem lastro, sem salvar; save pertence ao escritor da resposta. `app/chamados/models.py:2735-2749`, `app/chamados/services/disputa_emissao.py:1300-1301` |
| `PerguntaDisputa.reabrir_por_competencia` | Retira exclusivamente via histórica `competencia_trancada`. `app/chamados/models.py:2751-2757` |
| `retratar_duplicata` | Via `duplicata`, conserva instante/autor se já carimbada e escreve a canônica no motivo. `app/chamados/services/fio.py:881-895` |
| `resolver_perguntas_por_chamado_resolvido` | Dois sítios na mesma função: carimbo pela via do fato com auto validação; ramo sem credor carimba com resposta apenas quando há texto. `app/chamados/services/disputa_emissao.py:1617-1618`, `app/chamados/services/disputa_emissao.py:1684-1692` |
| `_materializar_ausencia_de_resposta` | Absorção do dia, vias declaração/ausência, `resposta='validar'`. `app/chamados/services/materializacao.py:548-549` |
| `_materializar_resposta_hora` | Via de identidade `paridade_recusada`. `app/chamados/services/materializacao.py:899` |
| `_carimbar_recusa` | Via de identidade `fato_ocupado` quando aplicável. `app/chamados/services/materializacao.py:1074-1075` |
| `_materializar_uma` | Via declaração ou via sem declaração depois de produzir/conferir efeito. `app/chamados/services/materializacao.py:1243-1254` |
| `materializar_pendentes_aprovadas` | Via `juiz_aprovou`. `app/chamados/services/materializacao.py:1393-1406` |
| `desfazer_carimbo_sem_lastro` | Retira carimbo de pergunta que o C6 já considera viva, sob lock. `app/chamados/services/relavra_via.py:221-231` |
| `lavrar_sem_turno_aberto` | Via `sem_turno_aberto`, sujeita ao C6 com carimbo aplicado. `app/chamados/reconciliador.py:405` |
| `retratar_perguntas_absolvidas_por_celula` | Via `celula_absolveu`, sujeita ao C6. `app/chamados/reconciliador.py:586` |

Os commands não viram escritores extras ao chamar essas portas: encerrar por fato chama `encerrar_por_fato_no_espelho`; reabrir órfãs chama `reabrir_fantasma`; curar limbo chama `carimbar_resposta_pela_trilha`/`reabrir_para_correcao`; relavrar I6 chama `relavrar`. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:147`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67-69`, `app/chamados/management/commands/relavrar_via_i6.py:100-102`.

**Estado pode mudar sem escrita na pergunta:** tranca da competência, decisão administrativa/veredito da célula, lâmpada do marco, batida concordante com resposta, classificação da falta, janela temporal e lastro externo são lidos pelo C6. Portanto um censo de `save` na pergunta não fecha todas as mudanças da vitalidade, e relógio/expiração não equivalem a uma decisão append-only já existente. `app/chamados/juizes.py:1135-1218`, `app/chamados/veredito_lavrado.py:95-118`, `app/chamados/veredito_lavrado.py:173-185`.

**Leitores/semântica relevantes ao item 6:**

- `estado` lê os três carimbos e o C6; `.viva` delega diretamente ao C6. `q_vivas` é deliberadamente um prefiltro generoso e não uma implementação equivalente. Logo “estado P/A”, “viva” e “casou q_vivas” não são sinônimos. `app/chamados/models.py:2491-2506`, `app/chamados/models.py:2760-2796`, `app/chamados/models.py:2798-2809`.
- `respondida_em` sozinho decide `.respondida`, enquanto o C6 exige também texto não vazio para o ramo da resposta viva; `responder_automaticamente` testa apenas texto para decidir no-op. Há significados diferentes de “tem resposta”. `app/chamados/models.py:2862-2864`, `app/chamados/juizes.py:1150-1162`, `app/chamados/services/disputa_emissao.py:1431-1435`.
- O materializador seleciona pela nulidade de `materializada_em`; já o C6 pode declarar viva uma pergunta carimbada e morta uma sem carimbo. O help text “batida criada + chamado fechado” não cobre vias fantasma, duplicata, admin e ausência. `app/chamados/services/materializacao.py:1344`, `app/chamados/juizes.py:1135-1218`, `app/chamados/models.py:2827-2829`, `app/chamados/models.py:2625-2632`, `app/chamados/models.py:2713-2721`.
- `validada_em` é instante da decisão no serviço; a porta genérica também pode preenchê-lo automaticamente com o instante do carimbo. Portanto sua existência não prova, sozinha, validação humana. O painel exclui o usuário sistema ao contar validação humana. `app/chamados/services/validacao.py:127-133`, `app/chamados/models.py:2587-2594`, `app/chamados/services/painel.py:255-258`.
- O cache interpreta `julgado_em` como instante da avaliação e `janela_expira_em` como **data** de vencimento. `expira_em` parte de `datetime.date.today()`, enquanto `esta_stale` usa `timezone.now()` e `.date()`. Isso prova referências temporais diferentes no código, não prova divergência concreta de produção. `app/chamados/veredito_lavrado.py:95-118`, `app/chamados/veredito_lavrado.py:173-185`, `app/chamados/veredito_lavrado.py:227-229`.
- `data_evento` é declarado como snapshot do evento, usado como dia do fato para recalcular e como chave de célula; conversa sem data recebe `timezone.localdate()` de nascimento. Assim o mesmo campo já representa dia do fato/turno e dia civil da conversa. `app/chamados/models.py:2218-2220`, `app/chamados/services/validacao.py:404-408`, `app/chamados/models.py:2422-2430`.
- `recusa_motivo` é lido como causa para desfazer validação, mas também guarda referência de duplicata e pendência de tipo; não é sequência histórica nem um único gênero de decisão. `app/chamados/services/validacao.py:180-185`, `app/chamados/services/fio.py:891-895`, `app/chamados/services/materializacao.py:820-826`.
- A conferência da data lê a divergência entre dia do fato e dia apresentado pelo chamado e registra justificativa; não converte `data_evento` nem unifica os relógios. `app/chamados/services/validacao.py:611-625`.

O banco instalado, a execução efetiva dessas portas em produção, o passivo e qualquer número citado em comentários como medição histórica permanecem **[nao verificado]**.

## 06/11 22:44 · sessao 720860fb5d12 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo são relativos à raiz; nenhuma escrita executada. O esquema efetivamente instalado em produção permanece [nao verificado].

**(3) Identidade: um `create` central, seis chamadas de produção.**

| Sítio | Busca/chave anterior à criação | Destino |
|---|---|---|
| `app/chamados/models.py:2398` `abrir` | `do_fato`: colaborador via disputa + motivo + `data_evento`, ordenado por PK (`:2392`); pega **última** (`:2431`). Célula é consultada por colaborador+data (`:2430`), mas não integra a chave. | `cls.objects.create` em `:2476`; reaproveitamento muda célula/destino, sem transferir a disputa (`:2435`, `:2450`, `:2474`). |
| `app/chamados/services/disputa_emissao.py:389` `gerar_perguntas_disputa` | Sem data: disputa+motivo (`:299`); motivos datados: disputa+motivo+data (`:370`, `:386`); depois passa pelo dedup global de `abrir`. | Data derivada do evento ou fallback (`:396`). |
| `app/chamados/services/disputa_emissao.py:770` `_criar_pergunta_se_couber` | `_ja_perguntou` (`:765`) usa disputa+motivo+data (`:1237`) e `fio_vivo`, não simples existência (`:1243`). | `abrir` com data/destino (`:771`, `:776`). |
| `app/chamados/services/disputa_emissao.py:1195` âncora | Existe âncora não materializada por chamado_destino+motivo (`:1149`); disputa aberta por colaborador (`:1156`). | `abrir` **sem data**, motivo `ancora_ausente` (`:1197`). |
| `app/chamados/services/acoes_disputa.py:259` reabertura admin | Última disputa fechada do chamado (`:252`), sem dedup de pergunta próprio. | `abrir` sem data (`:260`): confirmação de conversa. |
| `app/chamados/services/declaracao_texto.py:325` declaração colab | `_candidata_reusavel` (`:313`) pega **primeira reutilizável** de disputa+motivo+dia (`:226`), depois fallback a `abrir`. | Motivo reduzido de posição/tipo (`:299`, `:300`), dia informado (`:326`). |
| `app/chamados/services/declaracao_texto.py:614` aplicar declaração admin | Mesma candidata (`:611`), depois `abrir`. | Motivo e dia da proposta (`:614`). |

Na contagem: **1 algoritmo central de acha/cria, 6 chamadores**; nenhuma chamada concreta a `PerguntaDisputa.objects.get_or_create/update_or_create` encontrada [nao verificado como garantia além da busca estática]; central efetivo é `app/chamados/models.py:2431` / `:2476`.

A conversa nasce sempre: o parâmetro original `data_evento=None` desliga o dedup e é convertido ao **dia civil local da emissão**, `timezone.localdate()` (`app/chamados/models.py:2424`). Logo o mesmo campo guarda dia de fato/turno e dia civil da conversa; não há flag persistida distinguindo isso (`:2427`, `:2476`).

**Constraint e locks.** `PerguntaDisputa.Meta` só declara nomes e ordenação (`app/chamados/models.py:2857`); a migration criadora também só esses options (`app/chamados/migrations/0014_perguntadisputa.py:31`). Nenhuma unicidade composta da pergunta está declarada ali; presença real de índice/constraint em PROD [nao verificado]. Há PK e FKs (`app/chamados/models.py:2209`, `:2211`), e índices simples de cache (`:2348`, `:2352`, `:2358`), não a identidade pedida.

`abrir` não trava a busca `last` nem envolve busca+create em atomic; `save` apenas envolve cada gravação individual em atomic (`app/chamados/models.py:2431`, `:2476`, `:2853`). Há locks **externos e parciais**:

- `_abrir_ou_agregar_disputa`: atomic + `select_for_update` na disputa **existente**, por colaborador+aberta (`app/chamados/services/disputa_emissao.py:466`). `criar_ou_atualizar_disputa` só gera as perguntas **depois** do retorno desse helper (`:598`, `:605`). Portanto o helper não estabelece por si só lock cobrindo `last→create` da pergunta.
- Declarações colab/admin mantêm atomic externo (`app/chamados/services/declaracao_texto.py:302`, `:605`), e usam `disputa_para_declaracao` (`:310`, `:608`), que chama o helper travado (`app/chamados/services/disputa_emissao.py:563`).
- Âncora: atomic + lock na disputa existente (`app/chamados/services/disputa_emissao.py:1148`, `:1156`), mas teste de existência da pergunta ocorre antes do lock (`:1149`).
- Agrupador de órfãos tem atomic (`app/chamados/services/disputa_emissao.py:898`) mas a seleção da disputa atual é **sem** `select_for_update` (`:935`), apesar da docstring prometer lock (`:881`).
- Reabertura admin não declara lock próprio (`app/chamados/services/acoes_disputa.py:252`, `:259`).
- Command `disparar_perguntas_competencia`: dedup de existência colab+motivo+dia (`:211`), depois chama fábrica (`:280`), com atomic em apply (`:288`); a fábrica chama `criar_ou_atualizar_disputa` e `gerar_perguntas_disputa` (`app/chamados/services/fabrica_da_celula.py:139`), sem lock próprio da identidade da pergunta.
- Command `smoke_chamados` também chama helper e depois gerador (`app/chamados/management/commands/smoke_chamados.py:99`, `:102`).

A corrida não foi executada/provada em banco [nao verificado]; a janela estática `last→create` é visível em `app/chamados/models.py:2431` / `:2476`.

**Instância de marco candidata.** A pergunta só tem FK célula, motivo e data, sem campo de marco/ordem (`app/chamados/models.py:2209`, `:2216`, `:2218`). O vocabulário existente apropriado é a chave do DNA da célula, **`hi/hii/hfi/hii2/hfi2/hf`**, na ordem canônica declarada por `CelulaDia.chaves_do_turno` (`app/escala/models.py:1652`, `:1659`), exposta por `marcos_presentes` como `(tipo,chave,hora)` (`:1672`). Candidato de desenho, não campo já existente: `instancia_marco = chave` qualificada pela célula; a decisão sobre identidade através de regeneração do DNA [nao verificado].

Há prova concreta de perda da instância: `_MOTIVO_POR_MARCO` só distingue início/fim/ida/volta (`app/chamados/services/declaracao_texto.py:42`); marcos internos são todos reduzidos a `int_ida` ou `int_volta` pelo tipo (`:118`). O juiz admite que duas pausas deixam “duas luzes para uma resposta” e retorna indeterminado (`app/chamados/juizes.py:1106`). Leitores do vocabulário de marco: cartório lê `chaves_do_turno` (`app/ponto/services/cartorio.py:586`); pauta lê `marcos_presentes` (`app/pautas/services.py:417`).

**(4) Absorção: reconstrução por coincidência, sem fato/FK do ato.**

`_materializar_ausencia_de_resposta` cria/retorna ausência por colab+data+tipo (`app/chamados/services/materializacao.py:520`), captura **um** `agora` (`:529`), varre perguntas-hora não materializadas da mesma disputa/data (`:531`) e tenta carimbar cada uma com esse instante (`:549`). O escape e irmãs mudas usam via `ausencia_dia`; quem declarou hora usa `declaracao` (`:548`). O carimbo pode ser recusado pelo C6, e a irmã então fica fora do grupo, com evento `irma_viva_na_ausencia` (`:550`, `:554`).

O leitor `perguntas_absorvidas_por` limita colab/dia/tipo de pergunta/via (`app/chamados/services/materializacao.py:605`), encontra escapes compatíveis com o tipo da ausência e extrai pares `(disputa_id,materializada_em)` (`:602`, `:609`), recupera o grupo (`:614`) e exclui perguntas com batida-origem viva (`:616`). Portanto a ligação é hoje **inferida** por `(disputa,data_evento,materializada_em)` + tipo/dia da ausência, não registrada como ato explícito (`:588`).

Esmeril adicional: a docstring chama `(colab,tipo,data)` de “chave única da ausência” (`app/chamados/services/materializacao.py:595`), mas o modelo atual declara unicidade **parcial somente não rejeitada** (`app/ponto/models.py:827`). Ao recriar/reabrir, o serviço escolhe a última rejeitada do tipo/dia (`app/ponto/services/ausencia.py:254`). Assim o trio não identifica inequivocamente uma ausência histórica rejeitada; a ligação histórica por FK permanece ausente (`app/chamados/services/materializacao.py:588`).

O ato que precisaria carimbar ligação explícita é a própria absorção bem sucedida: ausência efetiva devolvida por `_criar_ausencia` + pergunta escape + irmãs aceitas pela porta + instante comum (`app/chamados/services/materializacao.py:520`, `:529`, `:549`). Hoje só a irmã **recusada** recebe evento com IDs explícitos de escape/ausência (`:554`).

**Quem mais escreve os três campos:**

- `disputa`: atribuição no nascimento (`app/chamados/models.py:2476`); não encontrei transferência posterior da FK da pergunta. Reapontar altera `chamado_destino`, não disputa (`app/chamados/services/fio.py:845`).
- `data_evento`: nascimento (`app/chamados/models.py:2476`); `reapontar_resolvedora` preenche apenas se vazia (`app/chamados/services/fio.py:832`, `:834`, `:847`). `None` no ramo de recusa é só desfazer em memória (`:843`).
- `materializada_em`: atribuição única em `_escrever_carimbo` (`app/chamados/models.py:2558`), persistida por `carimbar` (`:2609`), `carimbar_identidade` (`:2644`), `descarimbar` (`:2652`) e limpeza em lote (`:2699`). Chamadores com outros atos compartilham o campo:
  - fechar admin (`app/chamados/models.py:2522`);
  - fantasma/reabrir fantasma/por resposta/por competência (`:2721`, `:2732`, `:2749`, `:2756`);
  - descarimbo na ressurreição (`:1504`);
  - duplicata (`app/chamados/services/fio.py:891`);
  - reabertura correção (`app/chamados/services/disputa_emissao.py:1414`);
  - chamado resolvido (`:1617`) e resolução sem resposta (`:1691`);
  - paridade/colisão/materialização/juiz aprovou (`app/chamados/services/materializacao.py:899`, `:1074`, `:1252`, `:1406`);
  - sem turno/célula absolveu (`app/chamados/reconciliador.py:405`, `:586`);
  - desfazer carimbo sem lastro (`app/chamados/services/relavra_via.py:231`).
  - revalidação externa monta `descarimbar(salvar=False)` em memória (`app/chamados/models.py:1455`) e restaura estado para consultar juiz; checar contexto antes de contar como gravação.
  
O help_text diz “batida criada + chamado fechado” (`app/chamados/models.py:2829`), enquanto o campo também representa ausência, fantasma, duplicata e recusa (`app/chamados/services/materializacao.py:549`, `app/chamados/models.py:2721`, `app/chamados/services/fio.py:891`, `app/chamados/services/materializacao.py:899`). O significado não é uniforme; usar esse instante como identidade permanente de absorção depende de um campo reutilizado e apagável (`app/chamados/models.py:2699`).

**Leitores para mapa do item (6):**

| Campo | Leitores comprovados |
|---|---|
| colaborador via disputa | identidade `app/chamados/models.py:2392`; app `app/chamados/services/pergunta_no_app.py:46`; folha agrupa `app/folha/previa.py:67`; absorção `app/chamados/services/materializacao.py:606`. |
| disputa / disputa_id | candidata por disputa `app/chamados/services/declaracao_texto.py:227`; absorção agrupa `app/chamados/services/materializacao.py:609`; explicador recorta disputa `app/chamados/services/explicador.py:122`; juiz usa colaborador da disputa `app/chamados/juizes.py:1200`. |
| motivo | identidade `app/chamados/models.py:2392`; candidata `app/chamados/services/declaracao_texto.py:227`; fábrica agrupa/dedup por motivo `app/chamados/services/fabrica_da_celula.py:122`; declaração reduz marco a motivo `app/chamados/services/declaracao_texto.py:300`. |
| data_evento | identidade `app/chamados/models.py:2393`; materializador escolhe dia da batida `app/chamados/services/materializacao.py:344`; absorção `:606`; folha agrupa dia `app/folha/previa.py:65`; expiração `app/chamados/veredito_lavrado.py:112`; juiz janela `app/chamados/juizes.py:1197`; fábrica dia da célula `app/chamados/services/fabrica_da_celula.py:118`; badge/modelo `app/chamados/models.py:373`; app dia `app/chamados/services/pergunta_no_app.py:92`. |
| celula | gate reaponta `app/chamados/models.py:2435`; juiz usa veredito admin e acusação `app/chamados/juizes.py:1147`, `:1181`; outbox escolhe célula ou fallback colab+dia `app/chamados/models.py:2681`. |
| materializada_em | `estado` (`app/chamados/models.py:2501`); C6 (`app/chamados/juizes.py:1171`, `:1177`, `:1207`); SQL vivas (`app/chamados/models.py:2784`); absorção (`app/chamados/services/materializacao.py:609`); frescor da lavra via `Max` (`app/chamados/veredito_lavrado.py:162`); fila materialização (`app/chamados/services/materializacao.py:1347`). |
| chamado_destino | guarda de costura (`app/chamados/services/fio.py:838`); reuso declaração (`app/chamados/services/declaracao_texto.py:314`); absorção decide reconciliar (`app/chamados/services/materializacao.py:563`); juiz/estado usa FK por outras funções [nao verificado para lista completa]. |
| marco/chave/ordem | `CelulaDia.marcos_presentes` (`app/escala/models.py:1679`), cartório (`app/ponto/services/cartorio.py:586`), pauta (`app/pautas/services.py:417`); declaração perde chave ao reduzir posição+tipo (`app/chamados/services/declaracao_texto.py:118`). |

Mistura temporal adicional: o decodificador de contexto prioriza `data_turno`, mas também aceita `data_batida`, data textual de timestamp de entrada e horário previsto (`app/chamados/data_evento.py:27`); `date.fromisoformat(v[:10])` extrai a data escrita sem conversão de timezone (`:11`). O relógio de expiração usa `datetime.date.today()` (`app/chamados/veredito_lavrado.py:112`), conversa usa `timezone.localdate()` (`app/chamados/models.py:2427`), e batida materializada combina `data_evento` com HH:MM e depois ajusta overnight (`app/chamados/services/materializacao.py:414`, `:420`). Equivalência universal de dia civil/dia de turno não está comprovada e o código mostra domínios distintos nesses pontos.

## 06/11 22:43 · sessao ae89694c2775 · - · - · origem=Ronald (tmux)

Censo somente estático; os pontos abaixo descrevem esta cópia, sem afirmar ocorrência em produção. Fontes nas respectivas linhas.

**1. `cobranca_viva_do_dia(colaborador, modulo_origem, dia, *, excluir=None)`**

Responde `ChamadoColaborador | None`: chamado mais novo do colaborador e módulos pedidos, com `status_local ∈ VIVOS`, dia forte igual ao solicitado, excluindo opcionalmente o chamado indicado. Ordena por `-criado_em,-pk`; não confere exigibilidade de marco, batida, ausência, decisão nem silêncio/arquivo. `app/chamados/juizes.py:1886-1902`.

Lê `ChamadoColaborador.colaborador`, `modulo_origem`, `status_local`, `contexto_json.data_turno`, `celula.data`, `criado_em`, `pk`; no `dia_do_fato(..., fortes=True)`, a célula vence, depois a chave de data declarada pelo catálogo e `data_turno`. `app/chamados/juizes.py:1897-1902`, `app/chamados/juizes.py:80-121`.

Relógio: recebe `dia`; não lê o instante atual nesse percurso. O `criado_em` serve à ordenação. `app/chamados/juizes.py:1886-1902`.

Todos os chamadores diretos encontrados no código de produção, incluindo alias:

- `ChamadoColaborador._plano_pergunta_viva`: `app/chamados/models.py:373`.
- `ChamadoColaborador._gate_premissa_do_fechamento`: `app/chamados/models.py:711`.
- `_julgar_colab_corpo` via `_cobr_c8`: `app/ponto/services/cartorio.py:1604`.
- `resolver_cobranca_da_entrada`: `app/ponto/services/entrada_sincronizada.py:28`.
- `_autoresolve_f2_f4`: `app/ponto/services/triagem_batida.py:589`.

**2. `pergunta_viva(p)`**

Responde bool, com memo na instância (`_pv_memo`); delega ao cálculo, não é função pura de argumentos explícitos: lê ORM, relógio interno, contexto/memos e escreve o memo no objeto. `app/chamados/juizes.py:822-848`, `app/chamados/juizes.py:1197-1218`.

Precedência efetiva:

1. `validada_em` preenchida → morta. `app/chamados/juizes.py:1136`.
2. Competência trancada, salvo toque humano posterior, ou via legada `competencia_trancada` materializada → morta. `app/chamados/juizes.py:1140-1146`, `app/chamados/juizes.py:919-954`.
3. Resposta com texto e carimbo → viva, exceto admin decidiu o dia ou resposta concorda com a luz e o dia não acusa. `app/chamados/juizes.py:1148-1162`, `app/chamados/juizes.py:1087-1132`.
4. Via de identidade com materialização → morta. `app/chamados/juizes.py:1170-1172`.
5. Materializada com luz do marco acesa → morta. `app/chamados/juizes.py:1177-1179`.
6. Célula acusadora → viva; célula não acusadora absolve quando não há marcos pendentes ou a luz da pergunta acendeu. `app/chamados/juizes.py:1181-1196`.
7. Na janela viva, `classificar_falta` acusando → viva. `app/chamados/juizes.py:1197-1203`.
8. Sem materialização → viva; com materialização, exige testemunha externa, ou morre se esse último juiz falhar. `app/chamados/juizes.py:1207-1218`.

Entradas diretas: `PerguntaDisputa.validada_em`, `respondida_em`, `resposta_colab`, `via_resolucao`, `materializada_em`, `data_evento`, `tipo_resposta`, `motivo`, `celula`, `disputa.colaborador`, `chamado_destino`; célula: `veredito`, `veredito_via`, `dna.marcos`, `ata.lampadas` (`hora,tipo,acesa,luz`), marcos pendentes. `app/chamados/juizes.py:1135-1218`, `app/chamados/juizes.py:886-916`, `app/chamados/juizes.py:1099-1132`.

Entradas transitivas relevantes:

- Empresa/corte da janela, `PeriodoFechamento.fechado_em`, toque humano posterior em `LogAuditoria`; atos `cobrar_dia_manual`, `veredito_celula`, `desarquivar_chamado`, com usuário diferente do sistema. `app/chamados/juizes.py:935-954`, `app/chamados/juizes.py:961-963`, `app/chamados/juizes.py:1065-1084`.
- `Batida` ligada à pergunta e não retratada, batida no slot (`timestamp,tipo,retratada_em`), slot derivado pelo materializador, completude via `turnos_do_colab`, trilha `retratar_pergunta_obsoleta`; fallback lê `chamado_destino.status_local ∈ ENCERRADOS`. `app/chamados/juizes.py:326-347`, `app/chamados/juizes.py:350-372`, `app/chamados/juizes.py:271-282`, `app/chamados/juizes.py:718-778`.
- `classificar_falta` lê célula (`trabalha,dna`), ausências de cobrança (`tipo,minutos,data,data_fim`), vínculo/tipo do dia, marcos congelados e turnos com batidas apuráveis. `app/escala/servico_jornada.py:302-304`, `app/escala/servico_jornada.py:340-355`, `app/escala/servico_jornada.py:391-435`.

Relógios ocultos: `_dentro_da_janela_viva` lê `date.today()` e monta janela anterior+corrente sem empresa explícita; `classificar_falta` lê `_tz.now()` contra o fim previsto. Não recebe `agora`. `app/chamados/juizes.py:556-588`, `app/escala/servico_jornada.py:375-378`.

Dependência de estado gravado: `validada_em` mata antes de conferir efeito; `via_resolucao`+`materializada_em` encerram por identidade; `celula.veredito/veredito_via/ata` e `destino.status_local` também entram como projeções/estado. Não lê um campo persistido `estado` da pergunta: a property `estado` é derivada dela. `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1170-1196`, `app/chamados/juizes.py:743-759`, `app/chamados/models.py:2492-2506`.

Todos os chamadores diretos de produção identificados por AST, com aliases resolvidos:

- Juízes auxiliares: `pergunta_sem_lastro_ou_viva:1225`, `perguntas_vivas_de:1230`, `fio_vivo:1242`, `alimentar_portas:1302`, `perguntas_respondidas_a_validar:1403`, `_dias_das_perguntas_vivas:2031`, todos em `app/chamados/juizes.py`.
- Modelos: `_vivas_entre:351`, `_universo_da_ressuscitacao:1458`, `pendentes_do_colaborador:1739`, `DisputaSupervisao.fechar:1837`, `PerguntaDisputa.estado:2502`, `carimbar:2596`, `viva:2809`, todos em `app/chamados/models.py`.
- Views: `responder_pergunta_disputa:1242,1257`, `reabrir_disputa_supervisao:1417`, em `app/chamados/views.py`; `api_disputa_responder` em `app/api/views.py:2631`.
- Relatório `classificar`: `app/chamados/relatorios_disputa.py:18`.
- Reconciliador: `lavrar_sem_turno_aberto:398`, `perguntas_absolvidas_pela_celula:651`, `_viva_em_competencia_aberta:748`, `veredito_orfa:1075`, `reatar_pergunta_orfa:1579`, em `app/chamados/reconciliador.py`.
- Calendário `contexto_calendario`: `app/colaboradores/services/calendario.py:476`; `dia_do_colab`: `app/ponto/services/dia_do_colab.py:71`.
- Fio: `montar_chamados_fio:234`, `processar_post_fio:602`, `_costura_nao_mata_o_fio:775`, `migrar_resolvedoras_vivas:932,935`, em `app/chamados/services/fio.py`.
- App: `perguntas_do_app:53`, `_marco_ja_perguntado:111`, em `app/chamados/services/pergunta_no_app.py`.
- `montar_contexto_detalhe`: `app/chamados/services/acoes_chamado.py:348`.
- `resolver_perguntas_por_chamado_resolvido`: `app/chamados/services/disputa_emissao.py:1597` e `:1679`.
- `medir`: `app/chamados/services/competencia_trancada.py:122`, `:132`, `:142`.
- `desfazer_carimbo_sem_lastro`: `app/chamados/services/relavra_via.py:227`.
- `perguntas_pendentes_do_colab`: `app/chamados/services/aviso_pendencia.py:13`.
- Vigia: `viva_em_disputa_fechada:755`, `carimbos_sem_lastro_por_dono:813`, em `app/chamados/services/vigia_fio.py`.
- Comandos operacionais: `disparar_perguntas_competencia.handle`, `app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `reabrir_orfas_enterradas.handle`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:83`; auditor `_contraprova_f1t:202`, `_i6_pares:540`, `handle:858,894,1650,1707,1766,1779`, em `app/chamados/management/commands/auditar_invariantes_chamados.py`.

A property `.viva` é outra entrada transitiva que delega integralmente ao juiz; o inventário acima é de chamadas diretas da função, não de todos os acessos às properties. `app/chamados/models.py:2800-2809`.

**3. `motivos_a_perguntar(cel, hoje, agora=None, *, carimbadas=None, julgada_antes=_JULGADA_DA_CELULA)`**

Responde lista de motivos novos da célula; opcionalmente acrescenta IDs a `carimbadas`. Isso significa “o que ainda falta emitir”, não “todas as ações atualmente exigíveis”. `app/chamados/services/fabrica_da_celula.py:74-85`, `:118-126`.

Lê célula/colaborador (`data,trabalha,ata,veredito,julgada_em,colaborador.situacao`), `dia_sem_trabalho`, teto por instante e marcos do vínculo; corta futuro, inativo, dia sem trabalho, veredito não acusador e `parcial_so_lista`. `app/chamados/services/fabrica_da_celula.py:88-104`, `app/ponto/services/cartorio.py:831-848`.

Dedup: lê **qualquer** `PerguntaDisputa` do colab-dia, exclui apenas `VIA_DUPLICATA`, junta por `motivo`; não aplica `pergunta_viva` ao conjunto `ja` e não discrimina instância do marco. `app/chamados/services/fabrica_da_celula.py:118-126`.

`carimbadas`: usa `materializada_em`, `via_resolucao` fora das identidades e `disputa.fechada_em is None`; são elegibilidade de desfazer, não vitalidade. `app/chamados/services/relavra_via.py:169-182`.

Faltantes saem de `_ata_do_dia`→`_marcos_faltantes`→`grade_da_celula`, olhando dia e véspera e lampadas apagadas; sem escala e sem ata, pode recorrer a `classificar_falta`. Os motivos são comprimidos, sem duplicar motivo para duas pausas. `app/chamados/services/fabrica_da_celula.py:34-65`, `app/chamados/services/disputa_emissao.py:818-844`, `app/chamados/services/declaracao_texto.py:65-127`.

`parcial_so_lista` depende de `cobertura_parcial`, `periodo_parcial`, julgamento anterior e fato novo; este lê Batida criado/retratada, Ausencia criado/atualizado/aprovada, Justificativa criado/atualizado/aprovada, célula regenerada/editada e marco que venceu. `app/ponto/services/cartorio.py:329-400`.

Relógio: `hoje` e `agora` vêm por parâmetro; se `agora=None`, lê `timezone.localtime()`. O ramo fallback `classificar_falta` continua lendo `_tz.now()` mesmo com `agora` passado. `app/chamados/services/fabrica_da_celula.py:93-98`, `:55-57`, `app/escala/servico_jornada.py:375-378`.

Únicos chamadores diretos em produção: `perguntar_pela_celula` (`app/chamados/services/fabrica_da_celula.py:164`) e `motivos_que_o_evento_deixou` (`:199`).

**Contradições/casos estáticos úteis à consolidação**

- `registrado` é `VIVOS`, portanto `cobranca_viva_do_dia` o devolve; `registrado` não é `COBRANCA` e não pede ação. Diferença de universo já no nome “cobrança viva”. `app/chamados/juizes.py:1894-1902`, `app/chamados/catalogo/motor.py:24-29`.
- Chamado `VIVOS` de dia já coberto/folga/marco aceso continua retornável pelo C8; fábrica pode dar `[]` por dia sem trabalho ou veredito não acusador, pergunta pode estar morta. É construção permitida pelos predicados, ocorrência no dado raspado [nao verificado]. `app/chamados/juizes.py:1897-1902`, `app/chamados/services/fabrica_da_celula.py:91-100`, `app/chamados/juizes.py:1177-1196`.
- Dia acusa, pergunta validada sem plantio: `pergunta_viva=False` imediatamente; fábrica não emite porque já tem o motivo; falta continua na ata. `app/chamados/juizes.py:1136-1137`, `app/chamados/services/fabrica_da_celula.py:118-126`, `app/chamados/services/declaracao_texto.py:109-123`.
- Pergunta sem carimbo de materialização, próprio marco aceso e outro marco faltante: C6 pode retornar viva por `cel.veredito ∈ VEREDITOS_ACUSAM`; o teste da luz exige `materializada_em` naquele ramo. Fábrica não reemite o motivo já existente. `app/chamados/juizes.py:1177-1184`, `app/chamados/services/fabrica_da_celula.py:118-126`.
- Competência trancada sem toque: C6 dá morta; fábrica não consulta tranca na seleção e pode listar um motivo novo. `app/chamados/juizes.py:1140-1141`, `app/chamados/services/fabrica_da_celula.py:88-126`.
- Duas pausas: C6 da pergunta de intervalo exige luz de **todas** as pausas; fábrica converte várias instâncias faltantes no mesmo motivo e colapsa em uma saída. Nenhum dos dois entrega a identidade de cada instância exigida pelo v4. `app/chamados/juizes.py:894-916`, `app/chamados/services/fabrica_da_celula.py:59-65`, `:123-126`.

## 06/11 22:43 · sessao f2d92a16f8d6 · - · - · origem=Ronald (tmux)

Somente leitura; censo estático da cópia.

**`porta_alcancavel_do_dia(celula_id, *, alimentacao=None)`**

- Retorna `None` ou `{rota, fila, verbos, fio}`; soma chamados da célula na fila e perguntas vivas no app, com âncora na fila e resposta não vazia quando respondidas. O fio prioriza `cobrar→colab`, depois `validar→admin`, senão `chamado`. `app/chamados/juizes.py:1327-1338,1341-1387`.
- Lê, via `alimentar_portas`, a partição **global** dos chamados vivos, pertença à fila global, perguntas das células julgadas pelo C6 e célula→colaborador; o app é carregado sob demanda, uma vez por colaborador no mapa do lote. `app/chamados/juizes.py:1282-1320`.
- Campos explícitos: chamado `id/modulo_origem/silenciado_ate/criado_em/urgencia`, perguntas `respondida_em/resposta_colab/chamado_destino_id/disputa.chamado_id`, além das entradas transitivas de C6 e do app. `app/chamados/services/verbos.py:44-57`; `app/chamados/juizes.py:1254-1257`; `app/chamados/services/fio.py:116-117`.
- Não recebe relógio: a partição lê `timezone.now()` e `qs_na_fila` lê outro `timezone.now()`. Portanto uma alimentação pode cruzar fronteira de silêncio entre as duas leituras. `app/chamados/services/verbos.py:35,48-51`; `app/chamados/services/painel.py:186-188`.
- Chamadores diretos de produção: `vigia_fio.censo_alcance`, `cadastro_realidade.beco_em_lote`, `fora_da_folha._porta`; esta última alimenta `_fio_do_dia`, usado por `fora_da_folha` e `o_que_segura`, e é chamada novamente para listar os chamados de `o_que_segura`. `app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:460`; `app/folha/fora_da_folha.py:65-78,178,221-225`.

**`dono_fora_do_fio(colab, data, perguntas, lista, com_papel=False, *, oferecido)`**

- Retorna, nesta ordem: `decisao_do_dia_pendente`, `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia`, `fio_do_admin`, ou `None`; com `com_papel=True`, retorna `(balde,papel)`. `app/chamados/services/vigia_fio.py:274-315`.
- Lê `oferecido` já calculado; ausência cobrindo a data para cobrança e seu status em `ABERTAS`; todas as perguntas da célula, buscando `estado==VALIDADA` e código `FOLGA_CONTESTA`; presença e dono lavrado na lista do admin. `app/chamados/services/vigia_fio.py:306-314`; `app/chamados/services/validacao.py:216-218`.
- Ausência usa `colaborador/data/data_fim/tipo/status`, selecionando maior `(data,pk)`; cobrança aceita toda não rejeitada. `app/ponto/turnos.py:2509-2517,2536-2557`.
- `p.estado` **é property**, não coluna de estado: validação sem materialização→`VALIDADA`; validação com materialização→`RESOLVIDA`; os demais ramos consultam C6 e carimbos. `app/chamados/models.py:2492-2506`.
- Não recebe nem lê relógio diretamente. O `oferecido` vem do calendário, que lê seu próprio `timezone.now()` e deriva `hoje`; a lista recebe `hoje/agora` no construtor e passa ambos a `cadastro_realidade.lista`. `app/chamados/services/vigia_fio.py:199-216,258-271`; `app/colaboradores/services/calendario.py:117-138,159-164`.
- Todos os chamadores de produção encontrados: `censo` e `censo_alcance`. `app/chamados/services/vigia_fio.py:431,654-656`.

**Carga e custo estático**

- Existe `CargaDaJanela(colaboradores, ini, fim)`, carregador dos mesmos corpos de `turnos_do_colab`, `dias_encerrados` e `dia_das_batidas`. Usa janela com pad, células/geradoras, candidatos a vínculo, todos os vínculos com tipo, atas; batidas apuráveis e última batida são carregadas sob demanda em lote, turnos memorizados por colaborador. `app/ponto/turnos.py:2922-2964,2974-3010`.
- Não carrega atualmente perguntas/decisões, chamados/partição global, ausências/cobertura ou precedência: o construtor e os métodos existentes terminam nas entradas e respostas acima. `app/ponto/turnos.py:2936-3010`.
- Os nomes literais `precedencia_cobertura_teto` e `portas_com_universo_global` não foram encontrados por busca em todo o repositório: existência dessas funções nesta cópia **[nao verificado]**.
- Composição existente para portas: `alimentar_portas` chama `particionar_verbos(qs_vivos())` **global**, depois `qs_na_fila()` **global**, filtra perguntas pelas células pedidas e reutiliza o lote. Chamá-la por célula repetiria o universo global; os leitores existentes passam alimentação em lote. `app/chamados/juizes.py:1285-1306,1375`; `app/chamados/services/vigia_fio.py:604,635`; `app/escala/services/cadastro_realidade.py:456,460`; `app/folha/fora_da_folha.py:166,218`.
- Ainda há consultas por pergunta/colaborador no C6 e app; o próprio carregador declara essa limitação. `_no_app` memoiza por colaborador, não por dia. `app/chamados/juizes.py:1260-1262,1309-1320`.
- Precedência de um dia delega a `fatos_do_periodo(colab,data,data)`; período aceita feriados/vínculos/foto/células previamente carregados, mas ainda consulta ausências e folgas por colaborador. `app/ponto/precedencia.py:242-282`.
- Cobertura pontual faz `.first()` por colab-dia; `cobertura_ausencia_periodo` já fornece uma consulta por colaborador para toda a janela com o mesmo desempate. `app/ponto/turnos.py:2536-2557,2733-2770`.
- O teto da pergunta passa por `vencidos_no_instante→cartorio.marcos_vencidos(colab,cel,cel.data,agora)`; não equivale simplesmente a “dia encerrado”, pois mantém ramo de passado sem marcos. `app/chamados/services/fabrica_da_celula.py:15-31`.
- Para janela: ampliar a alimentação com resultados de precedência/cobertura/teto, decisões e perguntas por instância, e partição global única; eliminar consultas internas dos corpos puros e passar o mesmo `agora` às leituras temporais. Isto é necessidade inferida dos pontos de consulta atuais, não implementação existente. `app/ponto/turnos.py:2936-3010`; `app/ponto/precedencia.py:273-282`; `app/chamados/juizes.py:1282-1320`; `app/chamados/services/verbos.py:35`; `app/chamados/services/painel.py:188`.

**Divergências concretas pelo código**

1. Um único chamado `registrado` ou silenciado pode ser retornado pelo C8 porque ele filtra `VIVOS`, mas dar porta `None`, pois a fila exige `COBRANCA` e silêncio vencido, supondo nenhuma outra porta. `app/chamados/juizes.py:1894-1902`; `app/chamados/services/painel.py:186-188`; `app/chamados/juizes.py:1382-1383`.
2. Pergunta respondida com texto, não validada/trancada, sem decisão admin nem concordância com a luz, pode viver mesmo sob âncora terminal; a porta exige âncora na fila e pode retornar `None`. `app/chamados/juizes.py:1135-1162,1378-1383`.
3. Pergunta com `respondida_em` e texto vazio pode viver se a célula acusa; a porta rejeita explicitamente `respondida_sem_texto`. `app/chamados/juizes.py:1150,1181-1184,1254-1257,1380`.
4. Porta exclui explicitamente “Resolver dia”; `dono_fora_do_fio` responde decisão pendente quando `oferecido=True`, mesmo sem porta. Também pode responder ausência aberta. São respostas de escopos distintos. `app/chamados/juizes.py:1367-1369`; `app/chamados/services/vigia_fio.py:306-310`.
5. Existência da porta lê C6, mas verbo lê crédito SQL com `q_vivas` e exclui materialização cujo `veredito_lavrado=sim`; o campo pode ficar velho até o cron. Assim pergunta viva e verbo atribuído à âncora podem divergir. `app/chamados/juizes.py:1361-1366`; `app/chamados/services/raiox.py:72,110-118,133-149`.
6. Folga aceita com `validada_em` **e** `materializada_em` vira `RESOLVIDA`; o ramo de dono exige literalmente `VALIDADA`, logo não reconhece essa linha como `escala_nao_descreve_o_dia` — dependendo dos demais ramos, pode cair na lista ou em `None`. `app/chamados/models.py:2499-2501`; `app/chamados/services/vigia_fio.py:311-315`.
7. O que a fábrica considera “ainda perguntar” exclui todo motivo já presente, salvo duplicatas, independentemente de sua vitalidade; uma pergunta existente viva pode significar C6=True e `motivos_a_perguntar=[]`. `app/chamados/services/fabrica_da_celula.py:118-126`.

Ocorrência dessas classes em produção, quantidade de consultas e custo de frota nesta execução: **[nao verificado]**.

## 06/11 22:43 · sessao e59c7c967e20 · - · - · origem=Ronald (tmux)

Censo dos itens 1–2, somente código; existência/conteúdo das trilhas em produção [nao verificado].

**1. Escritores de decisões na mesma linha**

| Ato/escritor | Campos e perda do anterior | Preservação encontrada |
|---|---|---|
| `responder_pergunta_disputa_core` | Substitui `resposta_colab` e `respondida_em`; se já validada, zera `validada_em/por`; pode descarimbar absorção sem lastro. `app/chamados/services/disputa_emissao.py:1282`, `:1295`, `:1300`. | Não cria `RespostaAnterior`. Evento conserva a validação anterior, mas não a resposta anterior (`:1309`); log da nova resposta é truncado em 24 caracteres (`:1318`–`:1321`); nota conserva a nova resposta/display, com deduplicação e execução best-effort (`:1346`–`:1358`). Não garante sequência completa de correções. |
| Escape `FOLGA_CONTESTA/OUTRO_MOTIVO`, no mesmo core | Substitui `recusa_motivo` por texto “Colaborador respondeu…”, truncado a 240. `app/chamados/services/disputa_emissao.py:1331`, `:1340`. | Sem snapshot do motivo anterior e sem RA nesse ato; nota da resposta vem depois (`:1343`–`:1358`). |
| `reabrir_para_correcao` | Zera `respondida_em`, `validada_em/por`, `resposta_colab`; com `desfazer_absorcao=True`, também `materializada_em/por` e `via_resolucao`. `app/chamados/services/disputa_emissao.py:1393`, `:1413`–`:1416`; `app/chamados/models.py:2699`. | Cria RA antes do update, no mesmo atomic, apenas se havia texto ou `respondida_em`; copia resposta e validação, mas não recusa/materialização/via (`disputa_emissao.py:1401`–`:1412`). |
| `responder_automaticamente` | Escreve texto e `respondida_em` quando texto vazio; pode substituir carimbo existente com texto vazio. `app/chamados/services/disputa_emissao.py:1431`–`:1435`. | Não cria RA, nem evento próprio nessa função (`:1422`–`:1436`). |
| `carimbar_resposta_pela_trilha` | Preenche somente `respondida_em` nulo, com instante passado; não substitui carimbo preenchido. `app/chamados/services/disputa_emissao.py:1448`–`:1451`. | Evento com novo instante/motivo, sem estado anterior necessário porque guarda exige nulo (`:1452`–`:1454`). |
| `_carimbar_validacao`, chamado pelo validar e pelo aceite de folga | Atribui `validada_em=now`, `validada_por=autor`. `app/chamados/services/validacao.py:127`–`:133`, `:263`, `:290`, `:396`. | `validar_pergunta` recusa carimbo já preenchido (`:366`–`:368`); registra log de validação (`:440`–`:443`). Não cria linha RA a cada aceite. |
| `devolver_validacao_recusada` | Zera `validada_em/por` quando materialização de hora foi recusada e há motivo; resposta fica. `app/chamados/services/validacao.py:180`–`:190`. | Evento `validacao_devolvida` contém antes/depois de assinatura, motivo e fechamento da disputa (`:205`–`:212`); sem RA. A foto anterior fornecida pelo fluxo de validar é anterior ao carimbo recém-tentado (`:384`–`:396`, `:425`). |
| `recusar_pergunta` | Substitui `recusa_motivo`; chama reabertura que limpa resposta/carimbos. `app/chamados/services/validacao.py:541`–`:547`. | RA pela reabertura; nota guarda resposta anterior e motivo; evento guarda resposta, horário e recusa anteriores (`:551`–`:560`). **Não passa `por/motivo` à RA** (`:547`). |
| `_materializar_batida_retroativa` — pendência de tipo | Substitui `pendencia_admin/recusa_motivo` pelo primeiro resultado de `_ts_tipo_do_plantio`. `app/chamados/services/materializacao.py:820`–`:826`. | Sem RA e sem snapshot anterior nesse bloco (`:820`–`:828`). |
| `_carimbar_recusa` | Substitui `recusa_motivo` somente se diferente; colisão também carimba identidade `fato_ocupado`. `app/chamados/services/materializacao.py:1064`–`:1078`. | Nota no chamado-destino quando muda; contém recusa traduzida atual, não motivo anterior (`:1079`–`:1082`). Sem destino, não escreve essa nota (`:1080`). |
| `carimbar` | Substitui `materializada_em/por/via`; pode preencher resposta/validação nulas. `app/chamados/models.py:2583`–`:2594`. | Foto anterior serve para desfazer **em memória** em caso de recusa/exceção; não é fato persistido de histórico (`:2595`–`:2613`). RA não é chamada. |
| `carimbar_identidade` | Substitui carimbo/via e, quando fornecida, recusa; `manter_carimbo=True` preserva instante/autor anterior, mas troca via. `app/chamados/models.py:2636`–`:2644`. | Não contém histórico próprio; trilha depende de quem chama (`:2625`–`:2644`). |
| `fechar_por_admin` | Encaminha para identidade `admin_decidiu`; carimbo de outra via pode ser sobrescrito. `app/chamados/models.py:2513`–`:2522`. | A guarda só impede repetir o próprio `admin_decidiu` já materializado (`:2513`–`:2521`). Sem RA. |
| `retratar_duplicata` | Via `duplicata`; substitui recusa pelo apontamento da canônica; preserva carimbo anterior quando existe. `app/chamados/services/fio.py:882`–`:894`. | Conserva ambas perguntas; não conserva antiga via/recusa nesse ato (`:878`–`:895`). |
| `descarimbar` / `descarimbar_em_lote` | Zera `materializada_em/por/via`; lote aceita outros campos para zerar junto. `app/chamados/models.py:2646`–`:2652`, `:2691`–`:2700`. | Sem trilha própria. A preservação depende dos chamadores. |
| `ressuscitar_perguntas` | Descarimba e zera `validada_em/por/respondida_em`; universo sem resposta textual. `app/chamados/models.py:1434`–`:1438`, `:1504`. | Log agrega IDs e rótulos de estado, não horários/assinaturas anteriores por pergunta; best-effort (`:1517`–`:1530`). |
| `encerrar_por_fato_no_espelho` | Substitui só `via_resolucao` por `fato_materializado`. `app/chamados/models.py:2707`–`:2710`. | Sem RA ou snapshot anterior na função (`:2702`–`:2711`). |
| `encerrar_fantasma` / `reabrir_fantasma` / `reabrir_por_resposta` / `reabrir_por_competencia` | Respectivamente carimba ou desfaz materialização/via. `app/chamados/models.py:2718`–`:2721`, `:2730`–`:2732`, `:2747`–`:2749`, `:2754`–`:2756`. | Funções delegam a `carimbar/descarimbar`; nenhuma cria RA. |
| `relavrar` | Substitui via derivada e lavra cache de veredito/relógio. `app/chamados/services/relavra_via.py:121`–`:126`. | Evento conserva antes/depois da via e veredito, sem RA (`:130`–`:136`). |
| `desfazer_carimbo_sem_lastro` | Descarimba sem tocar resposta/validação; relê pergunta/disputa com `select_for_update`. `app/chamados/services/relavra_via.py:217`–`:231`. | Evento conserva materialização/via anteriores, mas não autor anterior do carimbo (`:229`–`:236`). |
| `DisputaSupervisao.remover_motivo` | **É `app/chamados/models.py:1976`**. Apaga todas perguntas daquele motivo, respondidas inclusive; substitui `motivos/contexto`. | `contexto.revalidacoes` guarda evento, instante e motivo removido, não conteúdo das perguntas (`:1955`–`:1959`). **RA também cai em cascata**, pois FK usa CASCADE (`:3179`). Chamadores: posto reativado `app/colaboradores/signals.py:77`; âncora calibrada `app/escala/signals.py:161`. |
| `Disputa.fechar` — silêncio decidido | Fecha disputa e grava `resolucao/motivo_fechamento`; portas desligamento/admin_encerra carimbam perguntas vivas como admin_decidiu. `app/chamados/models.py:1852`–`:1865`. | Eventos por pergunta guardam via anterior e motivo; evento da disputa guarda motivo/resolução e IDs vivos (`:1856`–`:1869`). Sem RA por decisão de silêncio. |
| `Disputa.reabrir_admin/reabrir_sistema` | Zera `fechada_em/por`, `resolucao`, `motivo_fechamento`; admin incrementa contador. `app/chamados/models.py:2063`–`:2075`, `:2087`–`:2091`. | Sistema anexa nota da reabertura (`:2092`–`:2094`). Resolução anterior pode ter sido copiada no fechamento, mas receiver só faz a nota se o chamado-pai reconciliar (`app/chamados/signals.py:42`–`:45`); preservação integral não garantida. |
| `limpar_resolucao_residual` | Limpa resolução de disputa aberta. `app/chamados/models.py:2170`–`:2174`. | Copia texto anterior para nota, se há chamado (`:2175`–`:2181`). |
| `aparar_motivo_respondido` | Remove motivo/contexto quando resolvido por outro caminho, mantendo perguntas. `app/chamados/models.py:1890`–`:1897`. | Sem snapshot próprio do contexto/motivo anteriores; é distinto de `remover_motivo` (`:1881`–`:1888`). |

**Expiração automática por silêncio não é porta ativa encontrada:** o comando declara removidos `SILENCIO_DO_COLAB_DIAS`, `_disputas_expiradas`, `_encerrar_expiradas`, `_parada_desde`; conversa parada vai ao admin. `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`–`:64`.

**Trilha não equivale a garantia de preservação:** `core.observ.evento` grava LogAuditoria em best-effort, falha não derruba o ato. `app/core/observ.py:94`–`:105`; os campos incluem `valor_antes`, `valor_depois`, ator, usuário e relógio `criado_em`. `app/core/models.py:295`–`:307`.

**2. RespostaAnterior**

- Modelo em `app/chamados/models.py:3165`; migração de criação `app/chamados/migrations/0050_resposta_anterior.py:16`.
- Campos: `pergunta` FK CASCADE; `resposta_anterior`; `respondida_original_em`; `validada_original_em/por`; `versao_reaberta_em/por`; `motivo_reabertura` até 200. Ordena por reabertura descendente e PK descendente. `app/chamados/models.py:3179`–`:3191`.
- Único escritor de produção encontrado: `reabrir_para_correcao`, com `bulk_create`, antes do update e no mesmo atomic; só gera versão se há texto ou carimbo de resposta. `app/chamados/services/disputa_emissao.py:1401`–`:1416`.
- Leitores diretos encontrados: helper `respostas_anteriores` em `app/chamados/services/disputa_emissao.py:1723`–`:1727`; prefetch do fio em `app/chamados/services/fio.py:216`–`:228`; apresentação em `app/templates/chamados/partials/modal_fio.html:204`–`:208`.
- **Os seis chamadores não passam autor/motivo da reabertura**, embora a função aceite esses parâmetros: `app/chamados/models.py:2146`; `app/chamados/views.py:1436`; `app/chamados/services/acoes_disputa.py:103`; `app/chamados/services/validacao.py:547`; `app/chamados/services/materializacao.py:658`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`. A versão recebe, portanto, defaults `por=None`, `motivo=''`. `app/chamados/services/disputa_emissao.py:1362`, `:1409`.
- Não há proteção de imutabilidade declarada no modelo/migração; a classe define campos e ordenação, sem `save/delete` próprios ou constraint de append-only. `app/chamados/models.py:3165`–`:3191`; `app/chamados/migrations/0050_resposta_anterior.py:15`–`:32`.
- Para representar a sequência pedida, faltam tipos de decisão/fato e gravação a cada resposta, validação, recusa, silêncio/reabertura/retratação; autor e relógio de cada decisão; vínculo com resposta/decisão aplicável e instância do marco; absorção e seus carimbos; preservação contra cascata. A lacuna é deduzida comparando o esquema restrito de RA (`models.py:3179`–`:3191`) com os escritores acima (`disputa_emissao.py:1282`, `validacao.py:131`, `:545`, `models.py:1852`, `:1976`).
- O “histórico” atual do colaborador reconstitui eventos pelos **carimbos atuais** da pergunta e pelo texto atual truncado, sem ler RA: `app/chamados/services/historico.py:96`–`:114`. Assim, não representa sozinho uma sequência imutável de decisões.

## 06/11 22:42 · sessao d5c086f59f77 · V2-BUGS-I2 · verificador · origem=Code

**Não aprovaria o encerramento da volta 2.** Os reparos principais existem, mas a mesma classe de erro sobrevive na dobra por data civil do corpo 12x36 e no FT comercial; há também leitor que conserva o contrato “toda dobra no último período”. (`app/ponto/motor_calculo_v2.py:2818`, `app/ponto/motor_calculo_v2.py:3290`, `app/ponto/calculador/periodo.py:841`)

**1. Destino dos achados (a)–(g)**

| Achado | Verificação da volta 2 |
|---|---|
| **(a) F11d protegia 240 incorretos** | **Curado nesse teste:** agora exige 360 e HE50 noturna zero. F11e deixou de ser necessário nesse caso. Sobrevive outro selo protegendo erro da mesma classe: F10d exige 90 minutos pela data civil; F10e cobra zero sob `expectedFailure`. (`app/ponto/tests/test_o280_bateria_da_lei.py:1725`, `app/ponto/tests/test_o280_bateria_da_lei.py:1575`, `app/ponto/tests/test_o280_bateria_da_lei.py:1578`) |
| **(b) Só o último par levava o noturno** | **Curado na escrita da dobra dos períodos regulares**, no partido e comercial: cada período recebe seu trabalhado e seu split. O pressuposto antigo permanece na releitura do calculador. (`app/ponto/motor_calculo_v2.py:3122`, `app/ponto/motor_calculo_v2.py:3378`, `app/ponto/calculador/periodo.py:758`) |
| **(c) FT partido não tinha dobra/split** | **Curado na origem desse caminho:** o mensal escreve nos próprios FT, pelo mapa da jornada, antes de o fechamento ler. Isso não cobre o FT comercial ainda decidido pela data civil. (`app/ponto/motor_calculo_v2.py:2970`, `app/ponto/motor_calculo_v2.py:2978`, `app/ponto/motor_calculo_v2.py:3290`) |
| **(d) Split usava pontas cruas** | **Curado na casca:** usa o par efetivo. Os três construtores fechados carimbam esse par; não identifiquei caminho fechado normal que passe a zero por ausência dele. (`app/ponto/motor_calculo_v2.py:1021`, `app/ponto/motor_calculo_v2.py:2774`, `app/ponto/motor_calculo_v2.py:2901`, `app/ponto/motor_calculo_v2.py:3260`) |
| **(e) Prorrogação contava minutos duas vezes** | **Curada a sobreposição na função de origem.** A medição de frota exigida antes dessa alteração não está demonstrada pelo material fornecido: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:115`; exigência de medir antes em `app/docs/PROMPTS.md:1922`) |
| **(f) Promoção usava dia civil** | **Curado o sítio da promoção**, que agora consulta `_dia_de` com o mapa. **Parcial na classe:** o filtro anterior de escala certa e as dobras por período continuam civis. (`app/ponto/services/fechamento.py:953`, `app/ponto/services/fechamento.py:935`, `app/ponto/motor_calculo_v2.py:2818`, `app/ponto/motor_calculo_v2.py:3290`) |
| **(g) Divergência fora de `falhas`** | **Produto permanece igual:** a reconciliação continua publicada fora de `falhas`. Inclusão efetiva na sonda s1: **[não verificado]**. (`app/folha/porta_export.py:687`, `app/folha/porta_export.py:696`) |

**2. Escritores, leitores e efeito da redistribuição**

Não existe nesse resultado um único campo `minutos_noturnos`: existem relógio e legais, com unidades distintas. A redução multiplica relógio pelo fator; o split HE noturna permanece em minutos de relógio. (`app/ponto/motor_calculo_v2.py:219`, `app/ponto/motor_calculo_v2.py:220`, `app/ponto/motor_calculo_v2.py:991`, `app/ponto/motor_calculo_v2.py:446`)

| Campo por período | Quem escreve | Quem lê |
|---|---|---|
| `minutos_extra` | Dobras por período; FT partido; dobras diárias repartidas; excedentes ainda no último; resets e bloqueio. (`app/ponto/motor_calculo_v2.py:2819`, `app/ponto/motor_calculo_v2.py:2974`, `app/ponto/motor_calculo_v2.py:3122`, `app/ponto/motor_calculo_v2.py:3175`, `app/ponto/motor_calculo_v2.py:3378`, `app/ponto/motor_calculo_v2.py:1482`) | Resumo do espelho, assinatura A6 e invariante da exportação. (`app/ponto/services/espelho.py:112`, `app/ponto/services/esmeril_espelho.py:176`, `app/folha/porta_export.py:542`) |
| `minutos_extra_100` | Dobras, faixa acima de 120 minutos, resets/bloqueio; releitura também escreve. (`app/ponto/motor_calculo_v2.py:2820`, `app/ponto/motor_calculo_v2.py:2863`, `app/ponto/motor_calculo_v2.py:3356`, `app/ponto/calculador/periodo.py:843`) | ResultadoMes, DiaPago, alimentação do calculador, banco e DSR. (`app/ponto/motor_calculo_v2.py:334`, `app/ponto/services/dia_pago.py:182`, `app/ponto/calculador/alimentacao.py:103`, `app/ponto/motor_calculo_v2.py:3466`, `app/ponto/motor_calculo_v2.py:3509`) |
| `minutos_extra_100_feriado` | Ramos da dobra, resets/bloqueio e releitura. (`app/ponto/motor_calculo_v2.py:2821`, `app/ponto/motor_calculo_v2.py:2976`, `app/ponto/motor_calculo_v2.py:3124`, `app/ponto/motor_calculo_v2.py:3380`, `app/ponto/calculador/periodo.py:844`) | ResultadoMes, DiaPago e exclusão da dobra na base do DSR. (`app/ponto/motor_calculo_v2.py:337`, `app/ponto/services/dia_pago.py:183`, `app/ponto/motor_calculo_v2.py:3509`) |
| `minutos_extra_100_noturna` | `_aplicar_he_noturna`, resets e bloqueio. A casca só atribui resultado positivo. (`app/ponto/motor_calculo_v2.py:1028`, `app/ponto/motor_calculo_v2.py:2947`, `app/ponto/motor_calculo_v2.py:3359`, `app/ponto/motor_calculo_v2.py:1484`) | ResultadoMes, DiaPago normal/promovido e promoção mensal. (`app/ponto/motor_calculo_v2.py:349`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:252`, `app/ponto/services/fechamento.py:971`) |
| Noturnos relógio/legais | `_escrever_noturnas`; bloqueio pode zerar; releitura também atribui os dois. (`app/ponto/motor_calculo_v2.py:1009`, `app/ponto/motor_calculo_v2.py:1482`, `app/ponto/calculador/periodo.py:755`) | ResultadoMes inclui regulares e FT; DiaPago e resumo leem os campos correspondentes. (`app/ponto/motor_calculo_v2.py:326`, `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:224`, `app/ponto/services/espelho.py:99`) |

O FM recebe os totais regulares e depois acrescenta FT promovida. O DiaPago recebe as mesmas partições, soma por jornada e lê o subconjunto noturno do FT. A persistência continua no serviço de fechamento e em `lavrar`; a restauração pode modificar somente FM. (`app/ponto/services/fechamento.py:918`, `app/ponto/services/fechamento.py:960`, `app/ponto/services/fechamento.py:1210`, `app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:244`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1399`)

**Leitores cujo comportamento muda:**

- **Calculador/oráculo:** antes recebia somente o par portador da HE; agora recebe todos os pares com HE positiva. No NPART, passa de uma ponta com 420 para duas com 180/240, permitindo somar 120+240 noturnos. (`app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/regras.py:827`, `app/ponto/calculador/regras.py:835`)
- **A6 do esmeril:** transforma uma ocorrência de 420 em ocorrências de 180 e 240, agrupadas por quantidade e data civil. **Há erro próprio:** exige três elementos da lista, sem deduplicar dias; três pares iguais podem representar “três dias”. A6 não decide escala certa, que consulta somente A2/A8. (`app/ponto/services/esmeril_espelho.py:175`, `app/ponto/services/esmeril_espelho.py:179`, `app/escala/services/escala_certa.py:16`)
- **Invariante da `porta_export`:** agrupa pela data crua de entrada. No exemplo de dois pares atravessando meia-noite, a distribuição civil muda de 0/420 para 180/240; pode mudar a acusação quando houver intra positiva. **Esse leitor continua usando outro dia.** (`app/folha/porta_export.py:538`, `app/folha/porta_export.py:542`, `app/folha/porta_export.py:546`, `app/folha/porta_export.py:561`)
- **Banco comercial:** extras continuam agrupadas pela data civil da semana; a semana portadora pode mudar. Efeito monetário concreto na frota: **[não verificado]**. O DSR subtrai HE100 feriado por período, conservando zero para a dobra repartida. (`app/ponto/motor_calculo_v2.py:3465`, `app/ponto/motor_calculo_v2.py:3468`, `app/ponto/motor_calculo_v2.py:3509`)
- **Tela/espelho/PDF:** o resumo soma períodos; a tela diária e os valores do PDF leem rubricas. O PDF também consulta períodos para decidir presença de coluna. A redistribuição isolada conserva os totais de HE; as correções do noturno alteram suas rubricas. (`app/ponto/services/espelho.py:112`, `app/templates/ponto/espelho.html:268`, `app/relatorios/pdf_espelho.py:837`, `app/relatorios/pdf_espelho.py:844`)
- **TXT:** não emite evento próprio para o split HE100 noturna; lê HE100 normal/feriado e adicional noturno total. Portanto o reparo global de `minutos_noturnos` pode atingir o adicional exportado, mesmo fora de feriado. Igualdade de arquivos gerados: **[não verificado]**. (`app/folha/export.py:420`, `app/folha/export.py:439`, `app/folha/export.py:447`)

**Sim, existe pressuposto explícito “só o último”:** `calculador/periodo.py::reler` zera HE nos pares e recoloca toda dobra no último, mas preserva os splits noturnos. Relendo o NPART corrigido, o primeiro período pode ficar com **HE100=0 e HE100 noturna=120**. É subconjunto maior que seu pai por período. Chamador produtivo dessa releitura e dinheiro atual decorrente: **[não verificado]**. (`app/ponto/calculador/periodo.py:758`, `app/ponto/calculador/periodo.py:761`, `app/ponto/calculador/periodo.py:770`, `app/ponto/calculador/periodo.py:841`)

**3. Recorte longo: impacto fora de feriado e valor ainda errado**

A alteração não consulta feriado: alcança qualquer chamada com prorrogação habilitada cuja extensão do 00–05 ultrapasse 22h. Com prorrogação desabilitada, essa linha não atua. A exceção de hora reduzida controla o fator, separadamente da prorrogação. (`app/ponto/motor_calculo_v2.py:114`, `app/ponto/motor_calculo_v2.py:115`, `app/ponto/motor_calculo_v2.py:995`)

Na extração em memória das funções, reconstruindo pontualmente a linha anterior pelo diff, obtive estes valores; isso não constitui execução do pai inteiro. (`app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:115`)

| Período, prorrogação ligada | Antes → agora, minutos de relógio |
|---|---:|
| 04–23 | 1.200 → 1.140 |
| 04–01 seguinte | 1.440 → 1.260 |
| 00–00 seguinte, 24h | 1.560 → 1.440 |
| 04–04 seguinte, 24h | 1.800 → 1.440 |
| 08–08 seguinte, 24h | 600 → 600 |
| 22–08 seguinte | 600 → 600 |

Essas diferenças seguem da sobreposição removida; **cruzar meia-noite, sozinho, não implica mudança**, nem um período habitual de 12h exige alcançar o trecho corrigido. Quantos períodos reais fora de feriado mudam: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:98`, `app/ponto/motor_calculo_v2.py:115`, `app/ponto/motor_calculo_v2.py:120`)

Não encontrei contraexemplo ao teto `min(fim, s2_ini)` em si nos recortes examinados. **Existe valor novo ainda errado no split:** jornada contínua 22–07, prorrogação ligada, HE100 final de 60 minutos, das 06–07. O noturno da jornada é 540, mas `he_noturna` devolve zero: recalcula a janela começando às 06h e perde o fato de que esse trecho prorroga a noite anterior. Pela regra de prorrogação declarada, esses 60 deveriam ser HE100 noturna. É defeito remanescente, não regressão criada pelo teto. (`app/ponto/motor_calculo_v2.py:85`, `app/ponto/motor_calculo_v2.py:114`, `app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:446`)

**4. Par efetivo ausente**

Nos três construtores fechados, inclusive quando o período depois é classificado FT, o carimbo precede o split. Não encontrei comercial, partido ou 12x36 fechado normal perdendo HE noturna pela guarda nova. (`app/ponto/motor_calculo_v2.py:2467`, `app/ponto/motor_calculo_v2.py:2774`, `app/ponto/motor_calculo_v2.py:2901`, `app/ponto/motor_calculo_v2.py:3260`)

O par falta nos retornos de aberto. Há aberto creditado que copia somente trabalhado/fora da janela de um período auxiliar, descartando efetivos e noturnas; uma dobra posterior pode carregar HE100 com split zero. A guarda anterior também retornava sem saída, portanto isso não demonstra regressão desta troca. A quantidade noturna devida nesse aberto não está demonstrada: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2761`, `app/ponto/motor_calculo_v2.py:2888`, `app/ponto/motor_calculo_v2.py:3239`, `app/ponto/motor_calculo_v2.py:2562`, `app/ponto/motor_calculo_v2.py:3126`)

**5. Datas civis restantes: dinheiro e escopo**

- **`_ft_certa → escala_certa`:** continua passando data civil antes da promoção corrigida. Pode mudar dinheiro se vínculo/LIMBO ou competência/assinaturas diferirem entre cabeça e cauda: o excluído vai para trabalhadas sem 100% e nem chega à promoção. É outra pergunta — confiabilidade da escala — com a mesma inconsistência temporal. Ocorrência real: **[não verificado]**. (`app/ponto/services/fechamento.py:935`, `app/escala/services/escala_certa.py:31`, `app/escala/services/escala_certa.py:38`, `app/ponto/services/fechamento.py:952`, `app/ponto/services/fechamento.py:991`)
- **Comercial por período:** usa `e.date()`. Regulares são corrigidos depois; **FT fica fora desse reset**. Condicional concreto: FT 03–07 de d+1, pertencente à jornada feriada d, prorrogação ligada e escala certa. O corpo civil não escreve HE para suas quatro horas; o fechamento agora promove quatro horas pela jornada e lê split zero. Pela regra, deveria ler 240 minutos noturnos. Execução integrada desse fixture no pareador comercial: **[não verificado]**. **É a mesma pergunta do item 2: dobra e seu subconjunto.** (`app/ponto/motor_calculo_v2.py:3290`, `app/ponto/motor_calculo_v2.py:3308`, `app/ponto/motor_calculo_v2.py:3351`, `app/ponto/services/fechamento.py:953`, `app/ponto/services/fechamento.py:969`)
- **12x36/plantão:** conserva dobra por `e.date()` sem correção mensal equivalente. F10d já codifica a cauda de 90 minutos dobrada indevidamente; F10e declara que deveria ser zero. **Também é a mesma classe da dobra pelo dia errado.** (`app/ponto/motor_calculo_v2.py:2818`, `app/ponto/motor_calculo_v2.py:3555`, `app/ponto/tests/test_o280_bateria_da_lei.py:1575`, `app/ponto/tests/test_o280_bateria_da_lei.py:1582`)

**6. Mordida dos testes** [nao verificado]

| Testes | O que efetivamente protegem | [nao verificado]
|---|---|
| F11d/f/g/h | Valores explícitos da regra, incluindo FM e DiaPago; `confere` usa igualdade. Não aceitam qualquer resultado ou exceção. Execução no pai inteiro: **[não verificado]**. (`app/ponto/tests/test_o280_bateria_da_lei.py:1286`, `app/ponto/tests/test_o280_bateria_da_lei.py:1725`, `app/ponto/tests/test_o280_bateria_da_lei.py:1780`, `app/ponto/tests/test_o280_bateria_da_lei.py:1808`, `app/ponto/tests/test_o280_bateria_da_lei.py:1825`) |
| B-SPLIT-EFETIVO e s5b | Cobram zero pelo par efetivo; s5b também exige resultado positivo com o cru. Na reprodução em memória, a casca atual passou e a leitura crua reconstruída falhou por asserção. (`app/ponto/tests/test_o280_bateria_da_lei.py:1860`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:191`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:196`) |
| N-LONGO-a/b | Cobram 1.140/1.260. A linha anterior reconstruída produz 1.200/1.440; mordem numericamente a sobreposição. (`app/ponto/tests/test_o280_bateria_da_lei.py:1882`, `app/ponto/tests/test_o280_bateria_da_lei.py:1902`, `app/ponto/motor_calculo_v2.py:115`) |
| P5 VOLTA | Compara contra regra independente minuto a minuto e propriedades. Os três testes passam com o teto; reconstruindo a extensão antiga, falham por asserções. **Protegem a regra local, não apenas o código novo.** Essa regra declara expressamente o gatilho 00–05 e o teto de 26h como escolhas da casa. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:192`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:372`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:381`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:390`) |

**Limite importante:** F11f/g/h forçam `escala_certa_no_dia=True`; assim não exercitam o erro civil do filtro anterior. Os testes novos são estritos, mas não certificam essa fronteira. (`app/ponto/tests/test_o280_bateria_da_lei.py:1749`)

**7. Classes estruturais que permanecem**

- **Segundo juiz de pertença:** a base lê o mapa da autoridade e depois pode sobrescrevê-lo por sua própria regra de intervalo de até seis horas. O partido volta a publicar o mapa direto do juiz; classificação e mapa final podem, portanto, ter procedências diferentes. Caso real divergente: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2412`, `app/ponto/motor_calculo_v2.py:2417`, `app/ponto/motor_calculo_v2.py:2951`, `app/ponto/motor_calculo_v2.py:2981`)
- **Campo com dois significados:** HE como quantidade própria do período no v2 corrigido; HE como total diário depositado no último na releitura. A preservação dos splits torna a inconsistência numérica, não apenas terminológica. (`app/ponto/motor_calculo_v2.py:3123`, `app/ponto/calculador/periodo.py:758`, `app/ponto/calculador/periodo.py:843`)
- **Selo protegendo defeito:** F11d foi corrigido; F10d/F10e conservam o padrão errado/esperadamente falho. (`app/ponto/tests/test_o280_bateria_da_lei.py:1725`, `app/ponto/tests/test_o280_bateria_da_lei.py:1575`, `app/ponto/tests/test_o280_bateria_da_lei.py:1578`)
- **Portas e ordem:** as adições permanecem nas portas existentes. Continua possível persistir FM e capturar falha posterior na lavratura do DiaPago, deixando divergência; ocorrência real: **[não verificado]**. (`app/core/portas.py:500`, `app/ponto/services/fechamento.py:1151`, `app/ponto/services/fechamento.py:1206`, `app/ponto/services/fechamento.py:1245`)
- **Relógio:** as linhas alteradas usam instantes recebidos, sem nova leitura de “agora”. Não há evidência no diff de nova inversão de trava; execução concorrente: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:115`, `app/ponto/motor_calculo_v2.py:1024`, `app/ponto/services/fechamento.py:953`)

A validação executada ficou em leitura e reproduções em memória das funções/testes puros citados. Suíte Django, pai completo, impacto de frota, banco, logs e TXT gerado: **[não verificado]**. [nao verificado]

## 06/11 22:42 · sessao 5499de46e925 · - · - · origem=Ronald (tmux)

A bateria FIO-E0 C-E não fecha nesta cópia: buscas por `FIO-E0`, `C-A`…`C-E` e `V4-1`…`V4-5` nos arquivos `.md/.txt/.py` fora de logs não localizaram os quadros. Localização e conteúdo desses documentos: [nao verificado].

O topo do RELATO contém fatos até aproximadamente 20:47; os avais colados pelo usuário são posteriores, aproximadamente 22:1x/22:4x. Portanto, a ausência pode refletir a defasagem da cópia, sem prova de inexistência na árvore posterior (`app/docs/RELATO.md:3-10`; horário dos avais: [nao verificado] em arquivo, fornecido no chat).

As referências do anexo não correspondem ao BACKLOG desta cópia:

- `app/docs/BACKLOG.md:1797`: BUG-PLANTIO-NOTURNO-MAIS-1.
- `app/docs/BACKLOG.md:1813`: BO-[nome]-0611.
- `app/docs/BACKLOG.md:1821`: O10-TROCA-DE-PLANTAO-ENTRA.

Para o item (5), o máximo sustentado é:

| Casos | O que está provado |
|---|---|
| 1, 2, 6, 7, 10 | Faltam segundo o aval colado; descrição individual [nao verificado]. |
| 3, 4, 5, 8, 9 | Existência na C-E segundo o aval colado; localização, conteúdo e implementação executável [nao verificado]. |

Há outra auditoria Fernando de dez itens, mas não há ponte que permita identificá-la com a bateria FIO-E0: o BACKLOG diz “registrado, nada construído”, item 7 sem tabela de tradução e item 10 sem último clique (`app/docs/BACKLOG.md:1733`). Não usar essa linha para preencher os casos FIO-E0.

Testes executáveis relacionados encontrados, sem correspondência numérica C-E comprovada: [nao verificado]

- Marco vencido hoje, ainda não vencido, dois instantes com resultados diferentes, dia futuro e competência: `app/chamados/tests/test_pergunta_do_dia_em_curso.py:70-105`, `:125-135`.
- Dia misto: quatro marcos faltantes; entrada 08:00 validada sem registro, demais 12:00/13:00/17:00 aceitos; testa projeção, fio, API e supervisor: `app/chamados/tests/test_o318_dia_misto_projecao_por_marco.py:4-22`, `:132`, `:184`, `:214`, `:235`, `:250`.
- Veredito provisório até o teto não mata pergunta; fato encerrado sem acusação mata; equivalência `.viva` e consulta: `app/chamados/tests/test_o261_pergunta_viva_teto.py:52-80`.
- Resposta concordante com batida/luz até cinco minutos e dia coerente morre; seis minutos, dia acusando ou luz ausente permanecem vivos: `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:7-11`, `:56-66`.
- Cobrança diária renasce por novo marco faltante, sem criar outro chamado: `app/chamados/tests/test_reabrir_pergunta_o_marco.py:12-14`, `:57-73`.
- Precedência de validada, resposta, decisão admin, identidade, absolvição/acusação da célula e repetibilidade: `app/chamados/tests/test_pergunta_viva.py:17-77`.

Esses testes foram lidos; resultado de execução [nao verificado]. [nao verificado]

## 06/11 22:40 · sessao 439c496efa85 · - · - · origem=Ronald (tmux)

- **(a) curado:** F11d exige `he100_not=360`, `not_rel=360`, HE100=420 e HE50 noturna=0; F11e com `expectedFailure` deixou de existir nesse trecho. A comparação é `assertEqual` dos campos esperados, não captura de exceção. (`app/ponto/tests/test_o280_bateria_da_lei.py:1716`, `:1726`, `:1286`)

- **F11f/g/h cobram valores, sem aceitar qualquer resultado:** F11f exige FM/DP noturna=2h; F11g exige folga=7h e feriado=0; F11h exige FM/DP feriado=7h e noturna=6h. O helper usa `getattr(dp,...,0)`, mas DP ausente ainda reprova porque cada caso exige ao menos um valor positivo do DP. (`app/ponto/tests/test_o280_bateria_da_lei.py:1757`, `:1780`, `:1808`, `:1825`)

- **Limite desses testes:** todos os três forçam `escala_certa_no_dia=True`; portanto não provam o caminho real de decisão da escala certa, inclusive seu uso da data civil. (`app/ponto/tests/test_o280_bateria_da_lei.py:1748`)

- **B-SPLIT-EFETIVO** exige trabalhado=780, extra=300, fora=60, noturno=0 e ambos splits=0. A casca s5b exige `(0,0)` para par efetivo e positivo para o cru; não é teste vazio. Contudo o falso fornece automaticamente o par efetivo, então não cobre sua ausência em períodos reais. (`app/ponto/tests/test_o280_bateria_da_lei.py:1856`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:48`, `:191`, `:193`, `:196`)

- **N-LONGO-a/b mordem a soma duplicada:** resultados exigidos 1.140 e 1.260; também verificam o split HE50. O caso b não verifica explicitamente `_n100`, mas a exige HE100=0 na chamada. (`app/ponto/tests/test_o280_bateria_da_lei.py:1871`, `:1873`, `:1888`, `:1890`)

- **P5 VOLTA protege a regra local:** `_regra` conta união de minutos sem chamar a função de cálculo; `_celula` compara desconto à regra e às propriedades parte≤todo/peça≤duração. Os VOLTA continuam exigindo lista de falhas vazia; não aceitam resultado arbitrário. Seu oráculo incorpora duas convenções declaradas da casa: qualquer toque em 00–05 aciona prorrogação e duração acima de 26h resulta zero. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151`, `:155`, `:168`, `:177`, `:192`, `:205`, `:372`, `:381`, `:390`)

- **Prova executada somente em memória:** funções extraídas por AST, sem Django/DB; com o código atual, os três VOLTA passaram. Repondo somente `s1_fim_efetivo=fim`, conforme o diff fornecido, surgiram 1/2/1 falhas, todas de asserção; N-LONGO voltou a 1.200/1.440. A casca s5b passou atual e falhou ao restaurar a leitura crua. Isso é mutação pontual reconstruída, não execução do commit pai completo nem da suíte Django. (`app/ponto/motor_calculo_v2.py:115`, `:1023`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:372`, `:381`, `:390`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:185`) Execução integral no pai: **[não verificado]**.

- **(g) não demonstrado pelo material:** a divergência FM×DP continua publicada fora de `falhas`; os arquivos da sonda citada na triagem não foram fornecidos na cópia localizada. Logo, inclusão/correção na sonda e resultado da frota: **[não verificado]**. (`app/folha/porta_export.py:687`, `:696`, `:697`)

- **Fragilidade preexistente, fora dos novos VOLTA:** `_sob_mutante` aceita qualquer `AssertionError` da chamada, sem conferir mensagem ou valor causador; os novos VOLTA não usam esse mecanismo. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:345`, `:348`, `:373`, `:387`, `:393`)

## 06/11 22:40 · sessao f70c1404c640 · - · - · origem=Ronald (tmux)

Checklist 2 — mapa e efeitos verificados por leitura.

| Campo por período | Escritores no v2 | Leitores relevantes |
|---|---|---|
| `minutos_extra` | Dobra por período 12x36/comercial; FT partido; dobra por dia agora repartida; excedente diário continua no último; resets e bloqueio. (`app/ponto/motor_calculo_v2.py:2819`, `:2974`, `:3122`, `:3175`, `:3208`, `:3291`, `:3354`, `:3378`, `:3437`, `:1482`) | `horas_extra`; resumo espelho; A6 do esmeril; invariante da porta_export. (`app/ponto/motor_calculo_v2.py:270`; `app/ponto/services/espelho.py:112`; `app/ponto/services/esmeril_espelho.py:176`; `app/folha/porta_export.py:542`) |
| `minutos_extra_100` | Dobras e faixa acima de 120; FT partido; repartição por período; resets/bloqueio. (`app/ponto/motor_calculo_v2.py:2820`, `:2863`, `:2944`, `:2975`, `:3123`, `:3292`, `:3315`, `:3356`, `:3379`, `:3442`, `:1483`) | ResultadoMes; DiaPago; resumo; calculador alimenta split; banco/DSR. (`app/ponto/motor_calculo_v2.py:334`, `:3466`, `:3509`; `app/ponto/services/dia_pago.py:182`; `app/ponto/services/espelho.py:114`; `app/ponto/calculador/alimentacao.py:103`) |
| `minutos_extra_100_feriado` | Dobras por período, FT partido e por dia; resets/bloqueio. (`app/ponto/motor_calculo_v2.py:2821`, `:2945`, `:2976`, `:3124`, `:3293`, `:3357`, `:3380`, `:1483`) | ResultadoMes/DiaPago; exclusão do feriado na base semanal do DSR. (`app/ponto/motor_calculo_v2.py:337`, `:2646`, `:3509`; `app/ponto/services/dia_pago.py:183`) |
| `minutos_extra_100_noturna` | `_aplicar_he_noturna`; resets/bloqueio. A casca só atribui quando o resultado é positivo. (`app/ponto/motor_calculo_v2.py:1028`, `:2947`, `:3359`, `:1484`) | ResultadoMes, DiaPago normal e FT feriada, promoção FM. (`app/ponto/motor_calculo_v2.py:349`; `app/ponto/services/dia_pago.py:184`, `:252`; `app/ponto/services/fechamento.py:971`) |
| Noturnos | Campos efetivos são `minutos_noturnos_relogio` e `_legais`, escritos por `_escrever_noturnas`; a redução multiplica o relógio pelo fator. (`app/ponto/motor_calculo_v2.py:219`, `:220`, `:991`, `:1009`) | ResultadoMes soma legais de regulares + FT; DiaPago normal/FT; resumo soma legais e relógio separadamente. (`app/ponto/motor_calculo_v2.py:326`; `app/ponto/services/dia_pago.py:179`, `:224`; `app/ponto/services/espelho.py:99`, `:103`) |

FM recebe os totais de ResultadoMes em `fechamento.py:918-924`; FT feriada acrescenta trabalhado às HE100 e feriado, e lê o subconjunto noturno do período em `:960-971`. DiaPago normal soma cada período pelo mapa da jornada; FT promovida usa trabalhado para HE100/feriado e lê o subconjunto. (`app/ponto/services/dia_pago.py:173`, `:182`, `:244`, `:249`, `:252`)

A porta persistente do DiaPago continua `lavrar`, com `bulk_create` após apagar a versão; a versão oráculo pode substituir os valores dentro dessa mesma porta. A restauração de FM aceita os campos pedidos e pode alterar somente FM. (`app/ponto/services/dia_pago.py:329`, `:345`, `:359`; `app/ponto/services/fechamento.py:1381`, `:1399`)

**Efeitos concretos da repartição**

- **Calculador produtivo/oráculo acompanha a nova repartição:** só inclui em `he_pontas` períodos com HE positiva e depois chama `he_noturna` para cada par. NPART passa de uma ponta com 420 para duas com 180/240, permitindo o split 120+240=360. A porta do fechamento alimenta esse caminho e grava versão `oraculo`. (`app/ponto/calculador/alimentacao.py:102`, `:106`; `app/ponto/calculador/regras.py:827`, `:832`, `:835`; `app/ponto/services/fechamento.py:1276`, `:1278`, `:1301`)
- **A6 muda sua contagem:** agrupa `minutos_extra` por período e guarda a data civil, sem deduplicar datas; usa `len(lista)>=3`. NPART muda uma ocorrência de 420 para ocorrências de 180 e 240, podendo acender/apagar a assinatura; três pares iguais no mesmo dia também podem satisfazer “3+ dias”. Essa assinatura não rege dinheiro: escala certa só pergunta A2/A8 e retorna antes de A6. (`app/ponto/services/esmeril_espelho.py:20`, `:175`, `:177`, `:179`, `:101`; `app/escala/services/escala_certa.py:16`, `:42`)
- **A testemunha da porta_export muda a distribuição entre dias:** agrupa pela data civil/UTC de `entrada`, sem mapa da jornada. NPART move HE de 0/420 para 180/240 entre as datas civis; o invariante pode passar a acusar ou silenciar outra linha quando há intra positiva. (`app/folha/porta_export.py:535`, `:538`, `:542`, `:545`, `:561`)
- **Resumo de espelho soma todos os períodos:** preserva total de HE100 com a repartição; FT é excluída da HE desse resumo. Tela diária lê `dia.rubricas`; PDF lê rubricas para valores e consulta períodos somente para decidir presença de coluna. (`app/ponto/services/espelho.py:82`, `:105`, `:112`, `:114`; `app/templates/ponto/espelho.html:268`, `:279`; `app/relatorios/pdf_espelho.py:837`, `:844`, `:857`)
- **TXT não lê o split noturno para emitir evento próprio:** lê HE100 normal/feriado e noturno total; redistribuir HE por período mantendo totais não altera esses eventos diretamente. Igualdade de TXT produzido: [não verificado]. (`app/folha/export.py:420`, `:424`, `:439`, `:447`)
- **DSR exclui a dobra por período:** como HE100 e feriado recebem o mesmo valor em cada período, sua diferença permanece zero. Banco comercial agrupa extras por data civil da semana, podendo mudar a semana portadora; efeito monetário concreto em dados reais: [não verificado]. (`app/ponto/motor_calculo_v2.py:3465`, `:3468`, `:3474`, `:3509`)

**Pressuposto “só o último” ainda existe e conflita com a nova escrita**

`calculador/periodo.py::reler` declara e implementa que HE é rubrica do dia: zera HE em todos e recoloca toda dobra somente no último. Porém não zera nem recalcula `minutos_extra_100_noturna`. Assim, ao reler NPART já repartido pelo v2, o primeiro par pode ficar com **HE100=0 e HE100 noturna=120**; o segundo fica com HE100=420/noturna=240. O total mascara o subconjunto maior que seu pai no primeiro período. (`app/ponto/calculador/periodo.py:758`, `:761`, `:770`, `:835`, `:841`, `:844`)

A lista usada para zerar rubricas de uma cópia duplicada também omite ambos os splits noturnos. Portanto a cópia pode preservar esses subconjuntos depois de zerar HE50/HE100. (`app/ponto/calculador/periodo.py:182`, `:185`, `:739`, `:740`)

Chamador produtivo de `reler` e efeito financeiro atual desses dois defeitos: **[não verificado]**. O caminho produtivo/oráculo observado usa `insumos_do_calculador`/`rubricas_por_dia`, conforme descrito acima. (`app/ponto/services/fechamento.py:1274`, `:1276`, `:1278`)

## 06/11 22:40 · sessao 1ae8f22baf0e · - · - · origem=Ronald (tmux)

**(4) Par efetivo: não encontrei regressão para período fechado; há lacuna preexistente em aberto creditado.**

- 12x36 carimba antes do split (`motor_calculo_v2.py:2774`, `:2864`); partido carimba antes do processamento mensal, inclusive FT (`:2901`, `:2978`, `:3126`); comercial carimba antes do split (`:3260`, `:3316`). A classificação FT ocorre depois de chamar `calcular_periodo`, sem desviar desses carimbos (`:2467`, `:2477`, `:2501`).
- O par falta nos retornos de período aberto dos três construtores (`:2761`, `:2888`, `:3239`). Existe aberto com minutos trabalhados positivos: o crédito copia de `_pc` apenas trabalhado e fora da janela, descartando o par efetivo e noturnas (`:2562`, `:2568`). Eventual dobra mensal desse aberto pode carregar HE100 com HE100 noturna zero (`:3115`, `:3126`; `:3375`, `:3382`). O problema não nasceu da guarda nova: a guarda anterior também retornava sem `saida` — diff_v2 fornecido; execução comparativa [não verificado].

**(5) A cura temporal está parcial.**

1. **`_ft_certa` continua civil e pode alterar dinheiro.** O filtro anterior à promoção passa `timezone.localtime(p.entrada).date()` (`app/ponto/services/fechamento.py:934`). O juiz consulta vínculo/LIMBO e competência/assinaturas pelo dia recebido (`app/escala/services/escala_certa.py:31`, `:38`, `:42`). Se a cauda cruza vínculo ou competência com resposta diferente, perde ou ganha adicional: os excluídos nem chegam ao novo juízo feriado (`fechamento.py:952`) e entram em trabalhadas sem 100% (`:987`, `:991`). É pergunta distinta — “escala certa?” — com a mesma inconsistência temporal. Ocorrência na frota [não verificado].

2. **Comercial por período continua civil; para regulares há posterior correção, para FT não.** O corpo decide por `e.date()` (`motor_calculo_v2.py:3286`, `:3290`); o mensal zera/recalcula somente `resultado.periodos` (`:3348`, `:3351`, `:3354`). Portanto a classificação inicial dos FT sobrevive. Contraexemplo lógico: FT fechado **03–07 de d+1**, pertencendo à jornada feriada d, com prorrogação habilitada e escala certa. O corpo civil não feriado não gera HE para 240 minutos (`:3304`, `:3308`, `:3316`); a promoção pela jornada gera quatro horas HE100 feriado, mas lê HE100 noturna zero (`fechamento.py:953`, `:960`, `:969`), enquanto a regra exige 240 minutos noturnos. Execução desse fixture pelo pareador comercial [não verificado]. É a mesma classe do item 2: dobra e seu subconjunto noturno.

3. **12x36/plantão também conserva decisão civil com efeito monetário.** A dobra por período usa `e.date()` (`motor_calculo_v2.py:2805`, `:2818`); plantão reutiliza esse corpo (`:3555`). O próprio F10d exige os **90 minutos errados** da cauda civil feriada (`app/ponto/tests/test_o280_bateria_da_lei.py:1574`), enquanto F10e exige a regra zero sob `expectedFailure`, incluindo montagem e execução (`:1578`, `:1589`). Esse é selo preexistente que protege defeito, remanescente da mesma classe.

**(7) Autoridade paralela preexistente permanece.**

O registro entrega a pertença da batida ao juiz `turnos_do_colab` (`app/core/juizes.py:196`), mas a base primeiro lê seu mapa e depois sobrepõe `data_jornada = _prev_dj` por distância própria de até seis horas (`motor_calculo_v2.py:2412`, `:2415`, `:2417`). Não há guarda exigindo que o juiz tenha omitido a batida. O partido refaz seu mapa diretamente do juiz (`:2951`, `:2964`) e o sobrepõe no resultado (`:2980`), podendo classificação FT e mapa final terem fontes efetivas diferentes. Contradição concreta no dado de produção [não verificado].

**Portas e ordem de persistência.**

O diff mantém FM no serviço registrado (`app/core/portas.py:500`; `fechamento.py:969`) e DiaPago em `lavrar` (`app/ponto/services/dia_pago.py:244`, `:252`). Permanece a ordem FM persistido → tentativa de DiaPago → exceção capturada (`fechamento.py:1151`, `:1206`, `:1245`); logo falha nessa segunda etapa pode deixar FM e DP divergentes, sem que essa volta cure a atomicidade. Ocorrência real [não verificado].

_2638 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
