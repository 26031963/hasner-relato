# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 23:52** · publicado em **31/10/2026 23:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 23:52 · sessao c9c21cb2b2af · CONTENCAO-QUE-MORDE · leitura · origem=Code

**O contrato pressupõe mecanismos que a cópia ainda não oferece:** não há fiscalização geral de contadores por duas gerações; B1 exige consumo de tokens; verificador final sem obra reconhecida não é negado por esse motivo. (`bin/hook_stop_fila1.py:1034`, `bin/disjuntor.py:452`, `bin/disjuntor.py:1457`)

**(1) Composição, “esperado”, geração e leitores/escritores**

`gerar_estado.py` reúne resultados de medidores e escreve **Markdown**, sem esquema unificado `{contador, valor, esperado, geracao}`. `sem_agente` e interrompidas recebem “esperado 0” na prosa; fio morto tem mapa `esperado` estruturado no carimbo; leitores do dia têm `esperado_no_fim`. Portanto, não é tudo prosa, mas também não existe campo estruturado uniforme por contador. (`bin/gerar_estado.py:222`, `bin/gerar_estado.py:240`, `bin/fio_estado.py:123`, `app/core/medida_leitor_do_dia.py:409`)

Existe indicação de regeneração: o gerador obtém data/hora até segundos e imprime **“Medido em”**. Não existe número monotônico nem protocolo de gerações consecutivas; usar esse texto como identidade exigiria definir repetição, ausência, leitura inválida e regenerações no mesmo segundo. (`bin/gerar_estado.py:294`, `bin/gerar_estado.py:300`, `bin/gerar_estado.py:376`)

| Artefato/medida | Quem escreve ou produz | Quem lê |
|---|---|---|
| ESTADO | `relato.sh` chama o gerador para um temporário; `gerar` escreve `dst`; publicação vai para outro repositório por padrão | Destino padrão `/home/ronald/hasner-relato`; o Stop atual consulta B4/B2/recados, sem ler esse ESTADO. (`bin/relato.sh:23`, `bin/relato.sh:38`, `bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1034`) |
| Raias | Declarações por `raia_agente.sh`, combinadas com Git pelo medidor | `raias_estado` → gerador. (`bin/raia_agente.sh:49`, `bin/raias_estado.py:91`, `bin/gerar_estado.py:215`) |
| Interrompidas | Registro na célula de estado do BACKLOG | `interrompidas_estado` → gerador. (`bin/interrompidas_estado.py:89`, `bin/interrompidas_estado.py:105`, `bin/gerar_estado.py:236`) |
| Fio morto | `vigiar_fio_morto` grava carimbo com censo e esperado | `fio_estado` compara os dois → gerador. (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `bin/fio_estado.py:125`, `bin/gerar_estado.py:260`) |
| Leitores do dia | Censo AST calculado na árvore | `leitor_dia_estado` → gerador. (`app/core/medida_leitor_do_dia.py:350`, `bin/leitor_dia_estado.py:35`, `bin/gerar_estado.py:290`) |
| Disjuntores | `dispara` escreve trilha; `_com_estado` grava estado JSON sob trava | `ler_estado`, decisões dos disjuntores e painel do gerador. (`bin/disjuntor.py:139`, `bin/disjuntor.py:237`, `bin/disjuntor.py:291`, `bin/gerar_estado.py:280`) |
| Cota | `statusline_cota.py` | `disjuntor.ler_cota`. (`bin/statusline_cota.py:51`, `bin/disjuntor.py:1064`) |

**(2) Contadores com esperado declarado e valores disponíveis**

Não há nesta cópia um ESTADO publicado nem os carimbos de execução necessários para afirmar valores atuais de produção. **Valores atuais desses artefatos: [não verificado].** O `logs/` é excluído do versionamento. (`.gitignore:53`) [nao verificado]

| Contador publicado pelo caminho do ESTADO | Esperado declarado | Valor atual verificável | [nao verificado]
|---|---|---|
| `sem_agente` | 0. (`bin/gerar_estado.py:222`) | [não verificado] |
| `interrompidas_mais_de_24h` | 0. (`bin/gerar_estado.py:240`) | [não verificado] |
| `fio_morto`, `fio_morto_cobrando`, `fio_morto_com_alvo`, `dias_futuros_no_universo` | Todos 0. (`app/chamados/management/commands/vigiar_fio_morto.py:87`) | [não verificado] |
| `respondida_sem_resposta`, `materializada_viva`, `fio_morto_alcance` | Todos 0. (`app/chamados/management/commands/vigiar_fio_morto.py:95`) | [não verificado] |
| Disparos B1, B2, B3, B4, B4-CODEX e ANEXO-ENXUTO em 24 h | 0, declarado no texto do painel. (`bin/disjuntor.py:1678`, `bin/disjuntor.py:1691`) | [não verificado] |
| Negações sem linha no RELATO | 0. (`bin/disjuntor.py:1683`) | [não verificado] |
| Verificador final sem obra reconhecida | 0. (`bin/disjuntor.py:1692`) | [não verificado] |
| Leitores com derivação própria do dia | **0 no fim do programa**, não uma declaração de que deveriam estar zerados agora. (`app/core/medida_leitor_do_dia.py:409`) | Reconto somente de leitura da cópia: **5 = 5 status + 0 valor**, correspondentes às cinco funções declaradas; cálculo pelo censo. (`app/core/medida_leitor_do_dia.py:319`, `app/core/medida_leitor_do_dia.py:390`, `app/core/medida_leitor_do_dia.py:394`) |

Há números **históricos gravados**, que não provam o “hoje” do contrato: raias `sem_agente=7` em 11/10; interrompidas acima de 24 h `=0` em 18/10; fio antigo `=4` e alcance `=388` na construção F5 de 30/10; disparos em 24 h `=0` às 08:3x de 31/10. (`app/docs/BACKLOG.md:1366`, `app/docs/BACKLOG.md:1483`, `app/docs/RELATO.md:249`, `app/docs/RELATO.md:166`)

Nem toda medida do ESTADO tem esperado zero: recusas explicitamente **não** têm; os placares estruturais possuem `numero`/`meta`, inclusive textuais. Logo, “nenhuma allowlist” ainda exige definir como descobrir quais medidas têm expectativa comparável. (`bin/recusa_estado.py:11`, `bin/gerar_estado.py:97`)

**(3) O que B1 chama de agente ocioso**

B1 lê o JSONL do subagente e seu `.meta.json`, derivados do caminho do transcript principal. Isenta tipos de somente leitura, incluindo `verificador` e `Explore`. (`bin/disjuntor.py:335`, `bin/disjuntor.py:353`, `bin/disjuntor.py:418`, `bin/disjuntor.py:91`)

A condição atual exige:

- Vida registrada de pelo menos **15 minutos** e tokens positivos na janela `agora − 15 min`. Soma `input_tokens`, `output_tokens` e `cache_creation_input_tokens`, deduplicando mensagens; não soma cache lido. (`bin/disjuntor.py:428`, `bin/disjuntor.py:443`, `bin/disjuntor.py:449`, `bin/disjuntor.py:450`)
- Diretório existente cujo caminho absoluto terminado em `wt-*` apareceu no transcript. Isso identifica uma cópia citada, sem comprovar propriedade. (`bin/disjuntor.py:93`, `bin/disjuntor.py:425`)
- Nenhum arquivo regular com **mtime recente** nas cópias reconhecidas, excluídos `.git` e caches. “Novo” inclui arquivo antigo editado ou tocado, sem verificar autoria ou conteúdo. (`bin/disjuntor.py:115`, `bin/disjuntor.py:387`, `bin/disjuntor.py:405`, `bin/disjuntor.py:454`)

**B1 não examina jobs/processos próprios em fundo. Sem tokens recentes, não dispara.** Também não possui timer autônomo: a medida ocorre no PreToolUse do agente, no máximo uma vez por minuto; DIFF/apply suspende B1 e `SubagentHandback` é permitido. O Stop apenas anuncia agentes cujo B1 já disparou. (`bin/disjuntor.py:413`, `bin/disjuntor.py:452`, `bin/disjuntor.py:478`, `bin/disjuntor.py:1434`, `bin/hook_stop_fila1.py:1053`)

**(4) “Verificador sem obra lida”**

A medida reconhece **rótulo de obra no prompt**, não comprova leitura efetiva de arquivos da obra. `_veredito_e_obra` examina a primeira mensagem humana do transcript; a varredura coleta finais sem obra reconhecida e acumula IDs únicos em `b3_sem_obra`. O painel publica o comprimento dessa lista com esperado 0. (`bin/disjuntor.py:504`, `bin/disjuntor.py:546`, `bin/disjuntor.py:736`, `bin/disjuntor.py:892`, `bin/disjuntor.py:1624`, `bin/disjuntor.py:1692`)

Esse contador é **acumulado**, sem janela de 24 h nesse cálculo. (`bin/disjuntor.py:892`, `bin/disjuntor.py:1626`)

No PreToolUse atual, final sem obra reconhecida faz `nb3f = None`: pula a decisão B3 e segue para B4. Pode ser negado por cota/escada ou outra guarda aplicável, mas **não pela ausência de obra em si**. Exceção no ramo B3 é caso separado: gera negativa por “não sei”. (`bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1496`)

**(5) `statusline_cota.py` e ausência de `cota.json`**

Grava somente quando chegam **os dois** campos semanais como números finitos, excluindo booleanos: `rate_limits.seven_day.used_percentage` e `rate_limits.seven_day.resets_at`. Ao gravar, não exige reset futuro nem valida faixa percentual. (`bin/statusline_cota.py:26`, `bin/statusline_cota.py:40`, `bin/statusline_cota.py:45`, `bin/statusline_cota.py:47`)

Também lê `context_window.total_input_tokens`, `context_window.used_percentage` e `session_id`. Grava esses dados como `contexto_tokens`, `contexto_pct`, `session_id`, junto de `seven_day_pct`, `seven_day_resets_at` e `hora` gerada, usando temporário e substituição atômica. Sem medida válida preserva o arquivo anterior; falha de escrita é silenciada. (`bin/statusline_cota.py:43`, `bin/statusline_cota.py:49`, `bin/statusline_cota.py:51`, `bin/statusline_cota.py:54`, `bin/statusline_cota.py:55`)

`ler_cota` decide apenas por percentual e reset: arquivo ausente/ilegível, medida inválida ou reset vencido resultam em `None`. Hora e sessão não determinam validade. (`bin/disjuntor.py:1069`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1075`)

Com `None`, B4 não bloqueia por desconhecimento da cota. Sem escada humana, o Stop remove `b4_para` e segue seu fluxo normal; **não conta Stops consecutivos de cota desconhecida**. O painel mostra “não sei”. (`bin/disjuntor.py:1126`, `bin/disjuntor.py:1204`, `bin/disjuntor.py:1208`, `bin/disjuntor.py:1690`)

**(6) Veredito por linha do contrato e limites omitidos**

| Linha | Confronto com a cópia |
|---|---|
| **1 — QUEM CHAMA** | Correta como proposta de integração, incompleta como descrição do existente: Stop não lê contadores gerais do ESTADO; há expectativas heterogêneas e falta implementar a história de gerações. `_com_estado` já é a porta de gravação do estado próprio. (`bin/hook_stop_fila1.py:1034`, `bin/gerar_estado.py:222`, `bin/fio_estado.py:123`, `bin/disjuntor.py:237`) |
| **2 — QUEM ESPERA** | Os novos bloqueios não existem: sem tokens B1 permite; final sem obra pula B3; cota desconhecida permite pelo B4. “Escada N” reconhece somente a palavra inicial, ignora N e não grava medida manual com hora; a condição dura até outra mensagem humana. (`bin/disjuntor.py:452`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:1127`, `bin/disjuntor.py:87`, `bin/disjuntor.py:1079`, `bin/disjuntor.py:1114`) |
| **3 — FALHA/LIMITE** | Desligar hoje não exige linha no RELATO: basta existência da chave, inclusive link. “Nunca verde calado” não é garantia transversal: leitura falha do BACKLOG vira texto vazio no medidor de interrompidas; falhas gerais do Stop liberam. “≥4 fora hoje” **[não verificado]**. (`bin/disjuntor.py:144`, `bin/hook_stop_fila1.py:1039`, `bin/interrompidas_estado.py:91`, `bin/hook_stop_fila1.py:1305`) |
| **4 — MEDIDA** | Os testes existentes não demonstram os quatro novos comportamentos: o caso sem obra verifica contagem posterior; B1 usa consumo recente; o outro arquivo testa portas PAREI. Mordida dos quatro mutantes novos e estado das raias antigas: **[não verificado]**. (`bin/tests/test_disjuntor.sh:227`, `bin/tests/test_disjuntor.sh:427`, `bin/tests/test_hook_parei_so_lei.sh:40`) |
| **5 — QUEM DESLIGA** | Há guarda textual contra ferramentas que nomeiem a chave, inclusive quando desligado; isso não autentica a identidade de Ronald nem exige registro no RELATO. A política humana é mais ampla que a proteção implementada. (`bin/disjuntor.py:152`, `bin/disjuntor.py:160`, `bin/disjuntor.py:1486`) |

O limite central omitido é **atribuição e observabilidade**: mtime não prova trabalho do agente; ausência de ferramenta não provoca medição B1; “obra lida” significa identificação textual; e ausência de `cota.json` não prova por que o payload faltou. O RELATO registra a ausência, mas o escritor silencia erros e não conserva o payload recebido: a **causa real continua [não verificado]**. (`bin/disjuntor.py:405`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:504`, `app/docs/RELATO.md:5597`, `bin/statusline_cota.py:35`, `bin/statusline_cota.py:55`)

## 31/10 23:51 · sessao a24b0a52d011 · - · - · origem=Ronald (tmux)

`gerar_estado.py` compõe **Markdown**, com medidores separados; não há objeto unificado `{contador, medido, esperado, geracao}`. As metas `sem_agente=0` e `interrompidas_mais_de_24h=0` são texto interpolado; fio morto e leitores do dia possuem expectativas estruturadas em suas próprias fontes (`bin/gerar_estado.py:222`, `bin/gerar_estado.py:240`, `bin/fio_estado.py:123`, `app/core/medida_leitor_do_dia.py:409`).

Há **carimbo textual de regeneração**: executa `date '+%d/%m/%Y %H:%M:%S'` e publica `Medido em …`; não há número monotônico ou identidade robusta de geração. Assim, dizer que não existe nenhuma indicação de regeneração seria incorreto; dizer que já existe protocolo de “duas gerações consecutivas” também seria incorreto (`bin/gerar_estado.py:294`, `bin/gerar_estado.py:300`, `bin/gerar_estado.py:376`).

Escritores/leitores:

- `bin/relato.sh` chama o gerador para `$TMP/ESTADO.md`; o gerador grava `dst`; o relato raspa/publica em `$DEST/ESTADO.md`, cujo destino padrão é outro repositório, `/home/ronald/hasner-relato` (`bin/relato.sh:23`, `bin/relato.sh:38`, `bin/gerar_estado.py:376`, `bin/relato.sh:103`).
- A programação declarada do timer é 5 minutos, sem garantia de execução efetiva nesta cópia (`bin/hasner-relato.timer:6`; execução efetiva [não verificado]).
- Raias: `raia_agente.sh` escreve `logs/raias/<nome>.agente`; `raias_estado.ler()` combina declarações e Git; o gerador consome o resultado (`bin/raia_agente.sh:49`, `bin/raia_agente.sh:54`, `bin/raias_estado.py:91`, `bin/raias_estado.py:101`, `bin/gerar_estado.py:215`).
- Interrompidas: o medidor lê a célula de estado do BACKLOG e o juiz do topo; o gerador publica a composição (`bin/interrompidas_estado.py:89`, `bin/interrompidas_estado.py:93`, `bin/interrompidas_estado.py:105`, `bin/gerar_estado.py:236`).
- Fio: `vigiar_fio_morto` grava atomicamente o carimbo com `esperado`; `fio_estado` lê/compara; o gerador chama esse leitor (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `app/chamados/management/commands/vigiar_fio_morto.py:131`, `app/chamados/management/commands/vigiar_fio_morto.py:211`, `bin/fio_estado.py:104`, `bin/fio_estado.py:125`, `bin/gerar_estado.py:260`).
- Leitores do dia: censo AST no ato, retornando `numero`, `itens`, `esperado_no_fim`; o adaptador compõe Markdown, consumido pelo gerador (`app/core/medida_leitor_do_dia.py:390`, `app/core/medida_leitor_do_dia.py:393`, `app/core/medida_leitor_do_dia.py:409`, `bin/leitor_dia_estado.py:35`, `bin/leitor_dia_estado.py:46`, `bin/gerar_estado.py:290`).
- Recusas: envelope de resposta escreve a trilha `ponto.log`; medidor usa o mesmo leitor da LINHA HAIKU; não é esperado zero (`bin/recusa_estado.py:5`, `bin/recusa_estado.py:11`).
- Codex limite: escritor declarado é `codex_pergunta.sh`, leitor/compositor é `codex_limite_estado`, consumido pelo gerador (`bin/gerar_estado.py:267`, `bin/codex_limite_estado.py:247`).
- Orçamento: o gerador documenta Stop como escritor da trilha/estado e `marco_de_clear.py` como escritor do marco; consome `orcamento_contexto` (`bin/gerar_estado.py:271`).
- Disjuntores: o gerador consome `disjuntor.ler/linha`; a própria seção declara `disjuntor.dispara` escritor da trilha (`bin/gerar_estado.py:277`).

Censo do **esperado declarado** dentro das seções operacionais do ESTADO:

| Contador | Esperado | Valor corrente comprovável |
|---|---|---|
| `sem_agente` | 0 (`bin/gerar_estado.py:222`) | [não verificado] |
| `interrompidas_mais_de_24h` | 0 (`bin/gerar_estado.py:240`) | [não verificado] |
| `fio_morto`, `fio_morto_cobrando`, `fio_morto_com_alvo`, `dias_futuros_no_universo` | 0, dados do escritor (`app/chamados/management/commands/vigiar_fio_morto.py:87`) | [não verificado] |
| `respondida_sem_resposta`, `materializada_viva`, `fio_morto_alcance` | 0, dados do escritor (`app/chamados/management/commands/vigiar_fio_morto.py:95`) | [não verificado] |
| Disparos B1/B2/B3/B4/B4-CODEX/ANEXO-ENXUTO em 24h | 0, rótulo do compositor (`bin/disjuntor.py:1678`, `bin/disjuntor.py:1691`) | [não verificado] |
| Negações sem linha no RELATO | 0, rótulo do compositor (`bin/disjuntor.py:1683`) | [não verificado] |
| Verificador final sem obra lida | 0, rótulo do compositor (`bin/disjuntor.py:1692`) | [não verificado] |
| Leitores com derivação própria do dia | 0 **no fim do programa** (`app/core/medida_leitor_do_dia.py:409`) | Declarações atuais: **5 entradas**, o que não substitui o censo medido (`app/core/medida_leitor_do_dia.py:319`) |

`recusa_estado` explicitamente rejeita esperado zero; orçamento e limite Codex compõem medidas informativas sem campo `esperado`; os placares estruturais usam campos `meta` e `numero`, incluindo strings extensas, que não são automaticamente um esquema numérico comparável (`bin/recusa_estado.py:11`, `bin/orcamento_contexto.py:382`, `bin/codex_limite_estado.py:246`, `bin/gerar_estado.py:97`, `app/core/placar_estrutural.py:21`).

Os arquivos de execução não vieram na cópia inspecionada; `logs/` é ignorado. Não há dado suficiente para confirmar “>=4 fora hoje” (`.gitignore:53`; contagem atual [não verificado]).

Números **históricos gravados em documentos**, que não devem ser promovidos a atuais:

- Raias em **11/10 17:3x**: `sem_agente=7`, `velhas=6`, união 30 (`app/docs/BACKLOG.md:1366`).
- Interrompidas em **18/10 00:4x**: universo 2, abertas 1, maiores de 24h 0 (`app/docs/BACKLOG.md:1483`).
- Fio em **30/10 21:27**, ainda durante a construção F5: `fio_morto_alcance=388`, contador antigo `fio_morto=4` (`app/docs/RELATO.md:249`).
- Leitores em **31/10 00:0x**: 7; a fonte atual já declara somente 5 entradas (`app/docs/RELATO.md:227`, `app/core/medida_leitor_do_dia.py:319`).
- Disparos em **31/10 08:3x**: documento registra 0 e cota ainda sem arquivo; isso não comprova o estado posterior (`app/docs/RELATO.md:166`).

Limites adicionais encontrados: `interrompidas_estado.ler()` trata falha de leitura do BACKLOG como texto vazio, resultando em lista vazia/zeros; portanto “ESTADO ilegível nunca verde calado” não descreve uma garantia transversal dos medidores atuais (`bin/interrompidas_estado.py:88`, `bin/interrompidas_estado.py:92`, `bin/interrompidas_estado.py:136`). Fio tem tratamento melhor: ausência é não medido, erro de forma é ilegível, universo vazio é cego e carimbo velho não dá número na manchete (`bin/fio_estado.py:106`, `bin/fio_estado.py:115`, `bin/fio_estado.py:87`, `bin/fio_estado.py:140`).

## 31/10 23:51 · sessao d7d684624d82 · - · - · origem=Ronald (tmux)

1. **Stop atual não fiscaliza contadores gerais nem “duas gerações”.** A integração chama B4, B2 e recados de B3/B1; recados vêm do estado próprio do disjuntor e transcripts, sem parser de `ESTADO`, `esperado`, `FORA` ou geração nessa integração. (`bin/hook_stop_fila1.py:1034`, `bin/hook_stop_fila1.py:1043`, `bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:1053`, `bin/hook_stop_fila1.py:1206`, `bin/hook_stop_fila1.py:1294`, `bin/hook_stop_fila1.py:1299`)

2. **`statusline_cota.py` só grava quando os dois números semanais chegam válidos:** `rate_limits.seven_day.used_percentage` e `.resets_at`, ambos numéricos finitos, excluindo booleanos; não exige que reset esteja no futuro nem valida faixa percentual ao gravar. (`bin/statusline_cota.py:26`, `bin/statusline_cota.py:40`, `bin/statusline_cota.py:45`, `bin/statusline_cota.py:47`)

   Lê também `context_window.total_input_tokens`, `.used_percentage` e `session_id`; grava `seven_day_pct`, `seven_day_resets_at`, `hora` gerada, `contexto_tokens`, `contexto_pct`, `session_id`; usa temporário e `os.replace`. (`bin/statusline_cota.py:43`, `bin/statusline_cota.py:49`, `bin/statusline_cota.py:51`, `bin/statusline_cota.py:54`)

   Sem medida válida, preserva o arquivo anterior; erros na gravação são engolidos e a saída visual ainda pode apresentar a porcentagem recebida. (`bin/statusline_cota.py:47`, `bin/statusline_cota.py:55`, `bin/statusline_cota.py:61`)

3. **Arquivo de cota ausente, ilegível, não numérico, não finito ou vencido → `None`.** `ler_cota` usa apenas `seven_day_pct` e `seven_day_resets_at`; hora/sessão não participam da decisão. (`bin/disjuntor.py:1064`, `bin/disjuntor.py:1069`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1072`, `bin/disjuntor.py:1075`)

   `None` não gera contenção por cota: B4 permite novo Agent se não houver escada humana; no Stop, sem escada, remove `b4_para` e retorna `None`. Não há contagem de Stops com cota desconhecida nesse caminho. (`bin/disjuntor.py:1108`, `bin/disjuntor.py:1114`, `bin/disjuntor.py:1126`, `bin/disjuntor.py:1204`, `bin/disjuntor.py:1208`)

   O teste existente exige liberação com medida NaN e com reset vencido. (`bin/tests/test_disjuntor.sh:453`, `bin/tests/test_disjuntor.sh:454`, `bin/tests/test_disjuntor.sh:473`)

4. **Estado próprio:** `_com_estado` lê/decide/grava JSON sob flock e substituição atômica; ausente começa `{}`, ilegível levanta, falha de gravação registra FALHA e conserva a decisão já tomada; `ler_estado` lê sem essa mutação. (`bin/disjuntor.py:237`, `bin/disjuntor.py:249`, `bin/disjuntor.py:262`, `bin/disjuntor.py:268`, `bin/disjuntor.py:271`, `bin/disjuntor.py:281`, `bin/disjuntor.py:291`)

5. **Desligamento não exige linha no RELATO.** Basta `lexists` da chave, inclusive link; Stop retorna imediatamente e PreToolUse pula `pretool`, mantendo `guarda_chave`. O teste desliga inclusive com topo neutro. (`bin/disjuntor.py:144`, `bin/disjuntor.py:1486`, `bin/hook_stop_fila1.py:1039`, `bin/tests/test_disjuntor.sh:591`)

   A guarda é textual, negando ferramentas que nomeiem a chave e alterações reconhecidas de settings; isso implementa uma proteção de caminho, não uma autenticação de identidade humana. (`bin/disjuntor.py:152`, `bin/disjuntor.py:160`, `bin/disjuntor.py:164`, `bin/tests/test_disjuntor.sh:601`, `bin/tests/test_disjuntor.sh:605`)

6. **“Escada N” não é hoje fonte numérica manual com hora:** o parser reconhece somente a palavra inicial `escada`; ignora o N, não grava cota, e considera a última mensagem humana, encerrando a condição na mensagem humana seguinte. (`bin/disjuntor.py:87`, `bin/disjuntor.py:1080`, `bin/disjuntor.py:1102`, `bin/disjuntor.py:1114`)

7. **Medida/testes do contrato ainda não demonstrados:** `test_hook_parei_so_lei.sh` cobre decisão técnica, pergunta de lei, `!` e item de só registro; os quatro mutantes da nova contenção não aparecem nesses casos. (`bin/tests/test_hook_parei_so_lei.sh:40`, `bin/tests/test_hook_parei_so_lei.sh:48`, `bin/tests/test_hook_parei_so_lei.sh:59`, `bin/tests/test_hook_parei_so_lei.sh:65`)

   O teste existente de verificador sem obra comprova contagem posterior no painel, e seu mutante tira o contador; não é teste de recusa preventiva por ausência de obra. (`bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:1585`)

8. **Payload sem `rate_limits`: causa não demonstrada.** O RELATO registra ausência de `cota.json` e afirma que o payload nunca trouxe a seção; o script não salva payload recebido nem diagnostica a origem da ausência. A causa real do payload ausente permanece **[não verificado]**. (`app/docs/RELATO.md:5597`, `bin/statusline_cota.py:35`, `bin/statusline_cota.py:45`, `bin/statusline_cota.py:51`)

9. **Configuração gravada:** Stop, PreToolUse e statusline estão configurados para caminhos absolutos `/home/ronald/saas-hasner/...`; sua instalação/execução real nesta sessão não é demonstrada só por essa cópia. (`.claude/settings.json:9`, `.claude/settings.json:68`, `.claude/settings.json:76`; execução real **[não verificado]**)

## 31/10 23:50 · sessao 3a37361d563a · - · - · origem=Ronald (tmux)

B1:

- Fonte: `<transcript principal sem .jsonl>/subagents/agent-<id>.jsonl` e o `.meta.json` correspondente; exige `agent_id` e transcript existente. (`bin/disjuntor.py:335`, `bin/disjuntor.py:353`, `bin/disjuntor.py:464`)
- Isenta `verificador`, `Explore`, `Plan`, `claude-code-guide`, `statusline-setup`; meta ilegível significa “não sei” e não dispara. (`bin/disjuntor.py:90`, `bin/disjuntor.py:415`)
- Copia própria significa caminho absoluto terminado em `wt-*` citado em qualquer texto do transcript e que seja diretório existente; não há prova de propriedade desse caminho. (`bin/disjuntor.py:92`, `bin/disjuntor.py:425`)
- Janela: `agora - 15*60`; exige primeiro timestamp válido anterior/igual ao início da janela e tokens positivos em mensagens `assistant` recentes, deduplicadas por `message.id`/`uuid`; soma `input_tokens`, `output_tokens`, `cache_creation_input_tokens`, sem `cache_read_input_tokens`. (`bin/disjuntor.py:428`, `bin/disjuntor.py:441`, `bin/disjuntor.py:449`)
- **Sem tokens nos últimos 15 minutos não dispara**, contrariando contrato 2 “gastando tokens ou não”. (`bin/disjuntor.py:452`)
- “Arquivo novo” significa qualquer arquivo regular sem seguir symlinks com `mtime >= janela`, em qualquer cópia reconhecida; edição/touch de arquivo velho basta, sem autoria nem conteúdo/commit, excluindo `.git` e caches listados. (`bin/disjuntor.py:115`, `bin/disjuntor.py:387`, `bin/disjuntor.py:405`, `bin/disjuntor.py:454`)
- **Não mede job/processo próprio em fundo**: `medir_b1` lê apenas meta/transcript/cópias/mtime; a outra função `agentes_sem_entrega` reconhece lançamento e término do próprio subagente a partir de strings do transcript principal, não jobs internos dele. (`bin/disjuntor.py:366`, `bin/disjuntor.py:413`)
- B1 é reavaliado no PreToolUse no máximo uma vez/minuto; depois de disparado fica latched. Não há timer autônomo: somente entrada de ferramenta daquele agente provoca medida; `SubagentHandback` passa e `logs/sombra.lock` tomada/ilegível suspende B1. (`bin/disjuntor.py:473`, `bin/disjuntor.py:307`, `bin/disjuntor.py:1434`)
- Stop **não mede B1**: apenas cruza B1 já disparados com agentes sem notificação de término e pede `TaskStop`. (`bin/hook_stop_fila1.py:1053`)

Verificador sem obra:

- Reconhece “final” por palavras leitura/verificação/revisão/review com final, com exclusões de desenho; reconhece obra por âncoras `obra(s)` ou `leitura ... da`, token ID e validação de caixa/cauda. Isso mede rótulo no prompt, não evidência de arquivos efetivamente lidos. (`bin/disjuntor.py:94`, `bin/disjuntor.py:110`, `bin/disjuntor.py:500`, `bin/disjuntor.py:504`, `bin/disjuntor.py:518`)
- `_veredito_e_obra` examina primeira mensagem `user` do transcript; final sem obra reconhecida retorna `obra=''`, mesmo sem handback/veredito. (`bin/disjuntor.py:546`, `bin/disjuntor.py:551`, `bin/disjuntor.py:557`)
- `_reprovacoes_finais` varre apenas agentes cujo meta diz `agentType=verificador`; coleta IDs finais sem obra; `decide_b3` acumula IDs únicos em `b3_sem_obra` sob `_com_estado`. (`bin/disjuntor.py:736`, `bin/disjuntor.py:878`, `bin/disjuntor.py:892`, `bin/disjuntor.py:959`)
- `_b3_sem_obra` lê o comprimento da lista; ilegível é `None`; `ler()` expõe e `linha()` imprime “verificador final sem obra lida (esperado 0)”. **Não é janela de 24h**, embora esteja ao lado de medidas de 24h. (`bin/disjuntor.py:1620`, `bin/disjuntor.py:1624`, `bin/disjuntor.py:1691`)
- **Hoje PreToolUse não nega simplesmente por faltar obra**: `_nega_b3(ob,...) if ob else None`; segue B4, que pode negar por cota/escada, senão libera. Exceção no ramo B3 é caso distinto e nega por “não sei”, inclusive classificação `'*'` sem obra. (`bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1410`, `bin/disjuntor.py:1496`)
- Teste atual só comprova que final sem obra já executado entra no contador após Stop; não testa a negativa contratada na criação. (`bin/tests/test_disjuntor.sh:427`)
- Testes B1 usam explicitamente 510 tokens recentes; controles cobrem arquivo recente, somente leitura, vida curta e DIFF, sem controle de agente quieto com job próprio. (`bin/tests/test_disjuntor.sh:227`, `bin/tests/test_disjuntor.sh:243`, `bin/tests/test_disjuntor.sh:252`)
- `test_hook_parei_so_lei.sh` contém casos de decisão técnica, pergunta de lei, espera de `!` e só registro; não é evidência dos quatro mutantes novos do contrato. (`bin/tests/test_hook_parei_so_lei.sh:40`, `bin/tests/test_hook_parei_so_lei.sh:48`, `bin/tests/test_hook_parei_so_lei.sh:59`, `bin/tests/test_hook_parei_so_lei.sh:65`)

Limites adicionais:

- O desligamento existente testa só `lexists(DISJUNTOR_DESLIGADO)`; Stop e main PreToolUse pulam disjuntores sem exigir linha no RELATO. A guarda de chave ainda funciona desligado. (`bin/disjuntor.py:144`, `bin/disjuntor.py:1486`, `bin/hook_stop_fila1.py:1039`)
- Falha aberta geral permanece fora do ramo B3; B1 “não sei” nunca dispara, incompatível com supor proteção universal que nunca fica verde silencioso. (`bin/disjuntor.py:414`, `bin/disjuntor.py:1496`)
- `escada <N>` hoje só aciona escada por prefixo da última mensagem humana e dura até próxima humana; não grava percentual/hora em `cota.json`. (`bin/disjuntor.py:1079`, `bin/disjuntor.py:1108`)

## 31/10 23:48 · sessao 35d3d4711061 · DISJUNTOR-TETO-DA-OBRA · leitura · origem=Code

**A premissa central precisa de correção: o B3 não conta todas as voltas de construtor/verificador por obra. Ele mede reprovações finais de agentes e chamadas Codex triadas PROCEDENTE, com limiares separados.** (`bin/disjuntor.py:736`, `bin/disjuntor.py:857`, `bin/disjuntor.py:910`)

1. **Contagens, fontes, escritores e leitores — contrato 1**

   | Medida atual | Fonte e escritor | Quem conta/lê |
   |---|---|---|
   | Reprovações finais por obra | Transcripts `subagents/agent-*.jsonl`, metadado `agentType=verificador`, prompt reconhecido como final e `SubagentHandback` iniciado por `REPROVADO`. O código atribui os transcripts ao harness; a implementação desse escritor é **[não verificado]**. | `_reprovacoes_finais` coleta por `agent_id`; `decide_b3` acumula em `b3[obra].reprovados`. (`bin/disjuntor.py:335`, `bin/disjuntor.py:546`, `bin/disjuntor.py:736`, `bin/disjuntor.py:896`) |
   | Chamadas Codex verificador PROCEDENTES por obra | O wrapper escreve resposta e `<resposta>.id`; `--triagem` escreve `logs/codex/triagem.log`. | `_triagens` lê o último veredito de cada chamada; `decide_b3` recalcula `b3[obra].codex`. Uma chamada com vários achados vale uma volta. (`bin/codex_pergunta.sh:198`, `bin/disjuntor.py:773`, `bin/disjuntor.py:813`, `bin/disjuntor.py:850`, `bin/disjuntor.py:904`) |
   | Voltas de construtor | Não existe essa medida no B3; a consulta na abertura de `Agent` alcança somente `verificador` com prompt final. | Construtores ficam fora dessa consulta. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`) |

   O escritor do JSON é `_com_estado`, usado pela decisão, triagem e rearme. Os consumidores da decisão são Stop, PreToolUse para **Agent e Bash/Codex**, e também o wrapper por `--b3-parada`; portanto o contrato 1 omite um chamador existente. (`bin/disjuntor.py:237`, `bin/disjuntor.py:818`, `bin/disjuntor.py:959`, `bin/disjuntor.py:1032`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:1443`, `bin/disjuntor.py:1457`, `bin/codex_pergunta.sh:165`)

   **Nem tudo zera no rearme:** `reprovados` e `codex` ficam vazios e `disparado=False`, mas os identificadores são preservados em `rearmado`, incluindo todas as chamadas já triadas da obra, qualquer que seja o veredito. A liberação por linha preserva identificadores em `liberado` e hashes em `consumidas`. Sobrevive histórico parcial, não um contador completo de voltas. (`bin/disjuntor.py:1017`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:936`)

   Os limiares atuais são **uma reprovação final OU três chamadas Codex PROCEDENTES**; o número retornado é `max(n, nc)`, não a soma. (`bin/disjuntor.py:76`, `bin/disjuntor.py:77`, `bin/disjuntor.py:912`, `bin/disjuntor.py:924`)

2. **Segundo ERRO-BOBO — contrato 3**

   `linhas_libera_b3` não consulta histórico: aceita `ERRO-BOBO <obra>: <motivo>` com ao menos uma letra ou dígito no motivo. Não existe regra especial para o segundo erro da obra. (`bin/disjuntor.py:626`, `bin/disjuntor.py:650`)

   `decide_b3` verifica se a linha está fora da fotografia do disparo, ainda não foi consumida e tem `mtime` suficiente. **Outro ERRO-BOBO, com hash diferente e atendendo essas condições, libera novamente**; não vira DESENHO obrigatório. Repetir a mesma linha consumida não libera. (`bin/disjuntor.py:930`, `bin/disjuntor.py:932`, `bin/disjuntor.py:934`, `bin/disjuntor.py:938`)

   O teste atual cobre consumo da mesma linha e liberação por ERRO-BOBO; não estabelece uma proibição para o segundo ERRO-BOBO distinto. (`bin/tests/test_disjuntor.sh:382`, `bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:1468`)

3. **O que a triagem prova — contrato 3**

   **O sidecar é escrito pelo wrapper**, após sucesso e resposta nova: contém `obra`, `papel`, `hash` do commit consultado, `pid`, `fim` e `sha` da resposta. `--triagem` exige papel `verificador` e confere se o SHA corresponde à resposta. (`bin/codex_pergunta.sh:198`, `bin/disjuntor.py:753`, `bin/disjuntor.py:795`)

   **A linha escrita por `--triagem`** tem quatro campos separados por TAB: `data/hora`, `obra`, `volta`, `veredito`. A identidade efetiva da volta é `hash12@pid@fim@sha12`; os vereditos aceitos são `procedente` e `descartado`. Repetir o mesmo veredito é idempotente; mudar o veredito acrescenta linha e prevalece o último. (`bin/disjuntor.py:79`, `bin/disjuntor.py:802`, `bin/disjuntor.py:808`, `bin/disjuntor.py:813`, `bin/disjuntor.py:850`)

   Isso permite identificar, **por obra e chamada**, uma classificação PROCEDENTE. Não individualiza cada achado nem registra vínculo entre achado, diff seguinte e cura; o SHA confere a resposta, não a correção. (`bin/disjuntor.py:773`, `bin/disjuntor.py:800`, `bin/disjuntor.py:813`)

   Consequentemente, **um PROCEDENTE isolado sem cura não impede hoje o próximo final** pelo B3. O bloqueio Codex exige três PROCEDENTES; mesmo depois disso, uma linha DESENHO válida pode liberar sem comprovação de diff. O teste atual exercita essa liberação apenas escrevendo DESENHO. (`bin/disjuntor.py:912`, `bin/disjuntor.py:930`, `bin/disjuntor.py:938`, `bin/tests/test_disjuntor.sh:1153`, `bin/tests/test_disjuntor.sh:1162`)

4. **ESTADO e medida — contratos 2 e 4**

   O gerador importa `disjuntor.ler/linha` e compõe a seção por essa interface. Hoje `ler` agrega disparos, negativas e liberações em 24 horas; do JSON B3 consulta `b3_sem_obra`. Não publica voltas por obra. (`bin/gerar_estado.py:277`, `bin/gerar_estado.py:280`, `bin/disjuntor.py:1548`, `bin/disjuntor.py:1554`, `bin/disjuntor.py:1620`, `bin/disjuntor.py:1624`, `bin/disjuntor.py:1691`)

   **Lugar natural proposto:** acrescentar a leitura e a apresentação em `disjuntor.ler/linha`, mantendo `gerar_estado` como consumidor. É uma inferência da divisão atual; o contador persistente ainda precisa de definição. (`bin/gerar_estado.py:277`, `bin/gerar_estado.py:281`, `bin/disjuntor.py:1615`)

   Para listar **todas as obras abertas**, falta definir a fonte desse universo: registros B3 nascem de reprovações ou PROCEDENTES, portanto não representam obras abertas sem esses eventos. (`bin/disjuntor.py:897`, `bin/disjuntor.py:902`)

   Os três cenários novos e seus mutantes não constam da suíte atual; a lista existente cobre os mecanismos anteriores de B3, triagem, rearme e concorrência. “Cada um morde” e a contagem real **GRADE-FOLGA-JUIZ = 9** são **[não verificado]**; o anexo fornece somente a expectativa. (`bin/tests/test_disjuntor.sh:1468`, `bin/tests/test_disjuntor.sh:1507`, `bin/tests/test_disjuntor.sh:1587`, `bin/tests/test_disjuntor.sh:1784`)

   Não executei a suíte: ela cria temporários, árvores e arquivos mutantes, incompatíveis com a regra de somente leitura. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:1475`)

5. **Correções e limites do contrato, linha por linha**

   | Linha | Avaliação diante da cópia |
   |---|---|
   | **1** | **Premissa incompleta:** as fontes já lidas não cobrem todas as voltas, e há também o chamador do wrapper. Guardar o teto dentro de `decide_b3` não faz construtores passarem automaticamente por essa decisão. (`bin/disjuntor.py:736`, `bin/disjuntor.py:857`, `bin/disjuntor.py:1454`, `bin/codex_pergunta.sh:165`) |
   | **2** | **Comportamento novo:** não há teto de seis voltas, emissão de `PADRAO-DE-CIMA` ou comandos `reabre com N voltas`/`redesenha` nessa interface. A CLI atual oferece triagem, consulta e rearme. (`bin/disjuntor.py:76`, `bin/disjuntor.py:912`, `bin/disjuntor.py:1699`) |
   | **3** | **Exige mudanças:** segundo ERRO-BOBO ainda libera; não há prova de cura; “B3 do dia” é impreciso, pois decisão e rearme não têm janela diária. Estado ilegível de fato levanta em `_com_estado`, e o juiz transforma a exceção em NAO_SEI, recusado na porta B3. (`bin/disjuntor.py:930`, `bin/disjuntor.py:1003`, `bin/disjuntor.py:265`, `bin/disjuntor.py:981`, `bin/disjuntor.py:1046`) |
   | **4** | **Critério futuro, sem comprovação atual:** faltam os três cenários novos e o painel por obra; execução e número nove são **[não verificado]**. (`bin/tests/test_disjuntor.sh:1468`, `bin/disjuntor.py:1691`) |
   | **5** | **Autoridade parcialmente instrumentalizada:** há guarda textual contra o Code citar a chave ou executar `--rearma-b3`; a função de rearme não autentica Ronald, e `USER` serve ao registro. Reabertura por quantidade e redesenho ainda não existem na CLI. (`bin/disjuntor.py:160`, `bin/disjuntor.py:1003`, `bin/disjuntor.py:1026`, `bin/disjuntor.py:1699`) |

   **ID não reconhecido:** um Agent final sem obra reconhecida pula a consulta B3; posteriormente pode aparecer apenas em `b3_sem_obra`. O contrato precisa declarar o tratamento dessa abertura. (`bin/disjuntor.py:1455`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:746`, `bin/disjuntor.py:893`)

   **Concorrência:** a transição B3 é serializada, mas não há reserva de uma volta por obra entre autorização e execução. O wrapper trava por caminho da resposta; por inferência, chamadas da mesma obra com respostas diferentes podem passar antes de chegarem novas triagens. (`bin/disjuntor.py:253`, `bin/disjuntor.py:286`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:174`)

   **Desligamento incompleto:** Stop e PreToolUse consultam `DISJUNTOR_DESLIGADO`; a porta B3 do wrapper não consulta. Assim, pela leitura do fluxo, a chave não libera essa porta para obra PARADA/NAO_SEI. Também não há exigência de linha no RELATO para reconhecer o desligamento: basta a existência da chave. (`bin/hook_stop_fila1.py:1039`, `bin/disjuntor.py:1487`, `bin/codex_pergunta.sh:165`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:144`)

   **Retroatividade:** o histórico disponível é parcial, e a porta do wrapper decide sem transcript, usando reprovações de agentes já persistidas. Portanto “obras abertas entram com as voltas que têm” precisa definir alcance, fontes e tratamento de voltas ausentes ou ainda em voo. (`bin/disjuntor.py:736`, `bin/disjuntor.py:898`, `bin/disjuntor.py:1039`)

## 31/10 23:47 · sessao b0d1525dba7e · - · - · origem=Ronald (tmux)

- **Chamadores reais:** Stop chama `julga_b3` em `bin/hook_stop_fila1.py:1048`, acrescentado ao recado em `bin/hook_stop_fila1.py:1300`; PreToolUse chama `estado_b3` via `_nega_b3` (`bin/disjuntor.py:1402`), tanto para `Bash codex_pergunta` (`bin/disjuntor.py:1440`) quanto para `Agent verificador final` (`bin/disjuntor.py:1454`). Existe ainda a porta do wrapper: `bin/codex_pergunta.sh:165` → `b3_parada` → `estado_b3` (`bin/disjuntor.py:1042`) → `julga_b3` → `decide_b3` (`bin/disjuntor.py:980`). Portanto linha 1 omite um chamador existente e restringe incorretamente PreToolUse a Agent. (`bin/disjuntor_settings.json:7` usa matcher vazio.)

- **Não há contagem total de construtor/verificador em B3:** os transcripts produzem exclusivamente reprovações finais por `agent_id` (`bin/disjuntor.py:736`); são exigidos metadado `agentType=verificador`, prompt reconhecido como final e entrega `SubagentHandback` cuja primeira linha começa `REPROVADO` (`bin/disjuntor.py:742`, `bin/disjuntor.py:555`, `bin/disjuntor.py:561`). O harness é identificado no código como escritor dos transcripts (`bin/disjuntor.py:336`); sua implementação não está verificada. A outra fonte são voltas Codex verificador triadas PROCEDENTE, uma por chamada (`bin/disjuntor.py:773`, `bin/disjuntor.py:858`). O retorno usa **max(n, nc)**, não soma (`bin/disjuntor.py:924`).

- **Outras medidas não equivalem a voltas por obra:** o Stop guarda bloqueios por HEAD, teto 5 (`bin/hook_stop_fila1.py:35`, `bin/hook_stop_fila1.py:37`, `bin/hook_stop_fila1.py:1250`, `bin/hook_stop_fila1.py:1277`); o wrapper tem default `CODEX_TETO=6`, mas para execuções simultâneas globais, configurável por env (`bin/codex_pergunta.sh:47`, `bin/codex_pergunta.sh:137`). O painel conta agentes Claude por modelo/tipo em 24h, sem obra (`bin/disjuntor.py:1631`, `bin/disjuntor.py:1646`), e chamadas Codex por papel, incluindo `ini` de chamadas recusadas (`bin/codex_limite_estado.py:71`, `bin/codex_pergunta.sh:88`, `bin/codex_pergunta.sh:165`).

- **ESTADO:** `bin/gerar_estado.py:280` importa `ler/linha` de `disjuntor`; `bin/gerar_estado.py:281` compõe a linha, inserida na seção Disjuntores em `bin/gerar_estado.py:336` e `bin/gerar_estado.py:365`. O leitor atual deriva disparos, negativas e liberações da trilha em janela 24h (`bin/disjuntor.py:1548`, `bin/disjuntor.py:1554`, `bin/disjuntor.py:1568`); do JSON B3 lê apenas `b3_sem_obra` (`bin/disjuntor.py:1624`). Não publica lista ou contagem por obra (`bin/disjuntor.py:1615`, `bin/disjuntor.py:1691`). Lugar natural proposto: ampliar esse `ler/linha`, preservando `gerar_estado` como consumidor; é inferência arquitetural sustentada por `bin/gerar_estado.py:277`.

- **Universo de obras abertas:** o gerador já usa BACKLOG/bloco OBRAS para obras interrompidas (`bin/gerar_estado.py:236`); leitor correspondente abre `app/docs/BACKLOG.md` e recorta `OBRAS:INICIO/FIM` (`bin/interrompidas_estado.py:89`, `bin/interrompidas_estado.py:94`). Esse leitor só enumera interrompidas (`bin/interrompidas_estado.py:106`); o contrato ainda precisa definir a fonte e o critério de todas as abertas. O B3 cria registros apenas a partir de reprovações ou PROCEDENTES (`bin/disjuntor.py:897`, `bin/disjuntor.py:902`).

- **ID ausente:** Agent final sem ID reconhecido **pula a consulta B3**, pois `_nega_b3` só roda se `ob` (`bin/disjuntor.py:1455` e `bin/disjuntor.py:1457`). Depois pode aparecer apenas no agregado `b3_sem_obra` (`bin/disjuntor.py:746`, `bin/disjuntor.py:893`). Reconhecedor exige formato específico e primeira ocorrência válida (`bin/disjuntor.py:97`, `bin/disjuntor.py:104`, `bin/disjuntor.py:514`, `bin/disjuntor.py:521`).

- **Chave desligada não cobre a porta do wrapper:** Stop verifica `desligado` (`bin/hook_stop_fila1.py:1039`), PreToolUse também (`bin/disjuntor.py:1487`), mas wrapper consulta incondicionalmente para papel verificador (`bin/codex_pergunta.sh:164`), e `b3_parada` consulta o juiz sem checar a chave (`bin/disjuntor.py:1042`). Inferência direta: uma obra parada/NAO_SEI continua recusada por essa porta mesmo com a chave existente. O contrato precisa declarar ou corrigir essa divergência.

- **Concorrência:** transição B3 é serializada por flock e gravação atômica (`bin/disjuntor.py:249`, `bin/disjuntor.py:253`, `bin/disjuntor.py:268`, `bin/disjuntor.py:274`); anúncios também ficam sob a trava (`bin/disjuntor.py:284`). O wrapper trava **por caminho da resposta** (`bin/codex_pergunta.sh:162`), verifica B3 (`bin/codex_pergunta.sh:165`) e depois executa Codex (`bin/codex_pergunta.sh:174`). Não há reserva por obra nessa transação; duas respostas diferentes da mesma obra podem ser admitidas antes dos resultados serem triados — inferência desses trechos e da gravação posterior à resposta em `bin/disjuntor.py:802` e `bin/disjuntor.py:813`.

- **“B3 do dia” e autoridade Ronald:** as decisões/rearme não possuem reset por data (`bin/disjuntor.py:896`, `bin/disjuntor.py:1003`); 24h é só a janela do painel (`bin/disjuntor.py:1548`). “Só Ronald” é guarda textual de ferramentas (`bin/disjuntor.py:160`, `bin/disjuntor.py:162`), sem autenticação em `rearma_b3`; `USER` é usado somente no registro (`bin/disjuntor.py:1026`).

## 31/10 23:46 · sessao 13474eb913ba · - · - · origem=Ronald (tmux)

Leitura integral concluída; o teste tem 1.791 linhas e sua execução cria temporários, árvores e cópias mutantes, portanto não o executei sob a regra de somente leitura. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:50`, `bin/tests/test_disjuntor.sh:1475`)

- **Linha 1 do contrato — trilha insuficiente para contar toda volta:** os testes distinguem a leitura curta que não conta, a reprovação final que para a obra e a chamada Codex que só conta depois de triada; não equivalem a um contador geral de construtor/verificador. (`bin/tests/test_disjuntor.sh:341`, `bin/tests/test_disjuntor.sh:344`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1150`)
- **Linha 2 — teto 6 e sétima negada:** não aparecem entre os casos declarados; há limites diferentes: primeira reprovação final e terceira volta Codex PROCEDENTE. (`bin/tests/test_disjuntor.sh:785`, `bin/tests/test_disjuntor.sh:1153`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1468`)
- **Linha 3 — rearme:** os testes exigem que o rearme exclua também triagens PROCEDENTES ainda não varridas, inclusive numa obra sem decisão anterior. Isso comprova reset do B3, não permanência de um teto acumulado. (`bin/tests/test_disjuntor.sh:1181`, `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1287`)
- **Linha 3 — segundo ERRO-BOBO:** existe teste de liberação por ERRO-BOBO e de consumo único da mesma linha; não há cenário de segundo ERRO-BOBO diferente na mesma obra. A forma aceita até uma letra ou um dígito como motivo. (`bin/tests/test_disjuntor.sh:382`, `bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:706`, `bin/tests/test_disjuntor.sh:740`, `bin/tests/test_disjuntor.sh:1468`)
- **Linha 3 — PROCEDENTE sem cura:** o teste atual libera uma obra com três PROCEDENTES apenas escrevendo DESENHO; não produz nem verifica diff de cura nesse caminho. (`bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1162`, `bin/tests/test_disjuntor.sh:1164`, `bin/tests/test_disjuntor.sh:1165`)
- **Linha 4 — três mutantes novos:** não constam no inventário atual; os mutantes existentes cobrem limiar Codex 3→2, contagem de DESCARTADO, reset e consumo da mesma linha, entre outros. (`bin/tests/test_disjuntor.sh:1558`, `bin/tests/test_disjuntor.sh:1560`, `bin/tests/test_disjuntor.sh:1597`, `bin/tests/test_disjuntor.sh:1632`, `bin/tests/test_disjuntor.sh:1784`) “Cada um morde” e “GRADE-FOLGA-JUIZ = 9” permanecem **[não verificado]**.
- **Linha 5 — chave e autorização:** há testes para a chave desligar PreToolUse e Stop, e para negar ao Code comandos com a chave ou `--rearma-b3`. Não há testes de “reabre com N voltas”, “redesenha” ou reversão de commit neste instrumento. (`bin/tests/test_disjuntor.sh:585`, `bin/tests/test_disjuntor.sh:593`, `bin/tests/test_disjuntor.sh:596`, `bin/tests/test_disjuntor.sh:603`, `bin/tests/test_disjuntor.sh:1468`)

Limites úteis para a consolidação: há teste de reprovação final sem obra reconhecida aparecer em `b3_sem_obra`; há testes de transição sob uma trava, anúncio sob a trava, triagem bloqueada por trava ocupada e consulta após obtenção de slot. Isso não constitui teste de reserva atômica de seis admissões concorrentes. (`bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:820`, `bin/tests/test_disjuntor.sh:1064`, `bin/tests/test_disjuntor.sh:1347`, `bin/tests/test_disjuntor.sh:1419`, `bin/tests/test_disjuntor.sh:1468`)

## 31/10 23:31 · sessao 5f457781992f · CLAUDE-MD-NO-RELATO · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 23:30 · sessao 2875aa66955d · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 23:30 · sessao 0fca0dcded6b · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 23:30 · sessao e067b098e638 · - · - · origem=Ronald (tmux)

- **Lista de pares:** definida por `bin/relato.sh:100-111`; o consumidor é `raspar_publico.main`, que separa `src:dst`, lê a fonte e grava no destino público (`bin/raspar_publico.py:19-40`). O gerador/escritor automático do próprio código dessa lista não foi identificado [não verificado].
- **Lista do `git add`:** explícita em `bin/relato.sh:115`; o comando escreve o índice do repositório apontado por `DEST`, definido em `bin/relato.sh:23`, após o `cd` de `bin/relato.sh:114`.
- **Terceira lista obrigatória, omitida no pedido/contrato:** `git commit -- <paths>` enumera novamente os arquivos em `bin/relato.sh:124-125`. Acrescentar CLAUDE apenas ao raspador e ao `git add` deixa o arquivo no índice, fora desse commit (`bin/relato.sh:115`, `bin/relato.sh:124-125`).
- **Universo do selo:** o próprio selo executa `git ls-files`, escreve a quantidade num temporário e percorre esses caminhos (`bin/tests/test_publicacao_sem_nome.sh:60-67`). Lê o conteúdo **do disco**, não de `HEAD` ou do remoto, e chama `J.achados` (`bin/tests/test_publicacao_sem_nome.sh:67-72`). O contador impresso vem do temporário (`bin/tests/test_publicacao_sem_nome.sh:117-119`). Portanto, um CLAUDE apenas staged pode elevar N para N+1 sem estar no commit público; essa conclusão decorre da combinação acima (`bin/relato.sh:115`, `bin/relato.sh:125`, `bin/tests/test_publicacao_sem_nome.sh:60-72`).
- **Outro publicador com listas próprias:** `codex_estado.sh` gera/raspa/copia apenas `CODEX-ESTADO.md` (`bin/codex_estado.sh:24-36`) e o enumera no `git add`, no diff e no commit (`bin/codex_estado.sh:79-84`). O relato o chama como etapa separada (`bin/relato.sh:96-97`).
- **Selos vizinhos:** `test_codex_estado.sh` lê os scripts e verifica isolamento/raspagem temporária (`bin/tests/test_codex_estado.sh:100-113`); `test_codex_estado_evento.sh` verifica chamadas do produtor único e dos nomes (`bin/tests/test_codex_estado_evento.sh:27-30`). Esses trechos não enumeram o universo geral publicado (`bin/tests/test_codex_estado.sh:100-113`, `bin/tests/test_codex_estado_evento.sh:27-30`).
- **Chamador existente:** o serviço chama `relato.sh` (`bin/hasner-relato.service:14-15`), e o timer declara cinco minutos (`bin/hasner-relato.timer:5-7`); dizer “a cada marco” como descrição exclusiva da frequência omite esse chamador temporal (`bin/hasner-relato.service:15`, `bin/hasner-relato.timer:6`).
- **Enumeração descritiva:** a `Description` do serviço lista alguns documentos, mas não controla o conteúdo publicado (`bin/hasner-relato.service:10`, `bin/hasner-relato.service:15`).
- Outro selo/gerador com enumeração geral do repositório público não foi identificado na busca; inexistência absoluta [não verificado].

## 31/10 23:18 · sessao a99212a31c4e · cobparc · verificador · origem=Code

**Parecer: REPROVAR.** Há contraexemplos em que a noturna não recebe a dispensa devida, a ata conserva uma dispensa antiga, e a emissão contorna o gate “lista sem push”. Também há leitores que continuam julgando o dia por regras diferentes da grade. (`app/escala/utils.py:1335`; `app/ponto/turnos.py:2661`; `app/ponto/services/cartorio.py:1177`; `app/ponto/services/cartorio.py:1509`; `app/escala/servico_jornada.py:453`)

A identidade Git da cópia, o parentesco dos commits, resultados RED/GREEN e incidência em produção ficam **[nao verificado]**. Os contraexemplos abaixo são de código; não atribuo os números históricos encontrados em comentários à produção atual **[nao verificado]**. [nao verificado]

Para a **pergunta 1**, este é o censo dos acessos diretos de produção aos campos nomeados. “Muda” descreve o efeito sobre o leitor quando recebe os novos valores.

| Campo/retorno | Quem escreve | Todos os leitores diretos e efeito |
|---|---|---|
| Célula `dispensada` | A grade cria célula sem hora e não missing; o leitor reidrata o marcador da lâmpada. (`app/escala/utils.py:1207`; `app/escala/services/leitor_celula.py:120`) | Cartório propaga para a lâmpada; muda a ata produzida. Supra usa o marcador em `_parcial_sem_resto`; passa a impedir o bloco de furo sem batida quando não sobra missing. (`app/ponto/services/cartorio.py:430`; `app/ponto/supra_juiz.py:311`) |
| Dia `cobertura_parcial` | **Dois materializadores:** grade e leitor da célula, ambos perguntando ao juiz de integralidade. (`app/escala/utils.py:1191`; `app/escala/services/leitor_celula.py:401`) | Leitor usa para calcular `periodo_parcial`; cartório copia à ata, impede dispensa integral, participa do ramo nunca-bateu, da impressão e do gate sem push; status recebe o flag; supra distingue cobertura integral, dia cobrável e período. (`app/escala/services/leitor_celula.py:402`; `app/ponto/services/cartorio.py:601`; `:626`; `:1006`; `:1160`; `:1567`; `app/ponto/status_dia.py:149`; `:98`; `app/ponto/supra_juiz.py:268`; `:269`; `:304`; `:311`; `:381`; `:437`) |
| Dia `periodo_parcial` | Grade e leitor da célula calculam separadamente. (`app/escala/utils.py:1195`; `app/escala/services/leitor_celula.py:404`) | Grade exige período para dispensar; cartório inclui o **resultado agregado** na impressão; supra escolhe cadastro, fato em ausência ou furo. O ramo `'fora'` agora cobra missing. (`app/escala/utils.py:1204`; `app/ponto/services/cartorio.py:1014`; `:1169`; `:386`; `app/ponto/supra_juiz.py:381`; `:390`; `:437`) |
| Ata `cobertura_parcial` | `ata_do_dia` grava `True` quando a grade declara parcial. (`app/ponto/services/cartorio.py:601`) | Único leitor direto: `dia_sem_trabalho`. Parcial deixa de calar o dia inteiro, salvo `trabalha=False`. **O leitor da célula não usa esse carimbo para montar o dia: consulta cobertura novamente.** (`app/chamados/juizes.py:411`; `app/escala/services/leitor_celula.py:396`; `:401`) |
| Lâmpada `dispensada` | Propagação da célula e dispensa integral por ausência/justificativa. (`app/ponto/services/cartorio.py:431`; `:635`) | Único leitor direto: `_celulas_da_ata`, que agora preserva o motivo. Os consumidores de `acesa` recebem indiretamente a dispensa como lâmpada acesa sem luz. (`app/escala/services/leitor_celula.py:119`; `app/ponto/services/cartorio.py:611`) |
| `out['parcial_na_lista_sem_push']` | O próprio gate incrementa, lendo o acumulador anterior. (`app/ponto/services/cartorio.py:1570`) | A leitura nominal externa é o teste. O resumo do comando continua imprimindo `emitiria_furo_parcial`, sem exibir o contador novo; portanto esse contador, sozinho, não implementa uma lista administrativa. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:246`; `app/ponto/management/commands/processar_cartorio.py:66`) |
| Retorno de `deslocamentos_dos_marcos` | `{índice: minutos}` produzido com `encadear_marcos`. (`app/ponto/turnos.py:2661`) | Grade consulta índice/deslocamento para dispensar missing; `horas_reais_do_dia` usa deslocamento mais delta. São os dois consumidores de produção. A extração preserva o cálculo das batidas, mas passa a aplicá-lo também à dispensa. (`app/escala/utils.py:1205`; `:1209`; `app/ponto/turnos.py:2672`; `:2679`) |

Os leitores indiretos de `dia_sem_trabalho` também mudam: com ata parcial e `trabalha` diferente de `False`, deixam de excluir o dia inteiro.

| Leitor | Mudança |
|---|---|
| `_celula_furada` | Deixa de desviar para a véspera por natureza do dia; examina as lâmpadas atuais. (`app/chamados/juizes.py:451`; `:465`) |
| Premissa do chamado | Deixa de considerar o dia automaticamente regularizado; continua pelas verificações de marcos/furo. (`app/chamados/models.py:1232`; `:1236`; `:1247`) |
| Materialização | Passa a examinar o plantio no dia parcial. (`app/chamados/services/materializacao.py:160`) |
| Fábrica da célula | Passa a produzir motivos pela ata parcial. (`app/chamados/services/fabrica_da_celula.py:80`; `:111`) |
| Cluster espúrio | Passa a examinar órfãs e missing no dia parcial. (`app/ponto/services/cluster_espurio.py:28`; `:31`) |
| Detector de intervalo | Passa a examinar pausas apagadas no dia parcial. (`app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `:69`) |
| Reconciliação da grade | Parcial deixa de entrar em `_indef` por esse predicado; ainda existe o julgamento separado `_lei_cala_no_dia`. (`app/ponto/management/commands/reconciliar_grade.py:281`; `:290`) |
| Supra | Pergunta ao juiz, mas esse ramo exige `prev=0` e cobertura ausente; uma parcial aprovada com status informado não o alcança. (`app/ponto/supra_juiz.py:492`; `:494`) |

Os acessos diretos em testes também entram no censo: [nao verificado]

| Campo | Leitores de teste | [nao verificado]
|---|---|
| Célula `dispensada` | `app/ponto/tests/test_cobparc_marco_no_periodo.py:67`, `:75` |
| Lâmpada `dispensada` | `app/ponto/tests/test_ausencia_ata_dispensa.py:26`, `:61`, `:72`; `app/ponto/tests/test_bateria_lavratura.py:884`; `app/ponto/tests/test_cobparc_marco_no_periodo.py:106`, `:172`, `:193` |
| Dia `cobertura_parcial` | `app/escala/tests/test_cobertura_parcial_periodo.py:122`; `app/ponto/tests/test_grade_da_celula_cobertura_parcial.py:29`, `:33`; `app/ponto/tests/test_e3_status_lavrado.py:145`, `:146`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:63` |
| Ata `cobertura_parcial` | `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:68`, `:72`, `:73`, `:74`; `app/ponto/tests/test_cobparc_marco_no_periodo.py:176` |
| `periodo_parcial` | `app/escala/tests/test_cobertura_parcial_periodo.py:123`, `:128`, `:133`, `:177`, `:180`, `:330`, `:334`; `app/ponto/tests/test_cobparc_marco_no_periodo.py:80` |
| Contador sem push | `app/ponto/tests/test_cobparc_marco_no_periodo.py:246` |
| Deslocamentos | Exercitados indiretamente pelos casos noturnos; esses testes não comparam diretamente o mapa retornado. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:64`; `:71`) |

Para a **pergunta 2, sim**, sobrevivem respostas incompatíveis com a grade:

- **ALTA — dispensa contada como batida.** A nova célula tem `missing=False`, `hora=None`; `n_bat` do cartório conta qualquer dict não missing. Contraexemplo: nenhuma batida real, E08 dispensada e S17 missing. O cartório informa uma batida ao supra e troca `FURO_SEM_COBRANCA` por `FURO_PARCIAL`. (`app/escala/utils.py:1207`; `app/ponto/services/cartorio.py:1186`; `app/ponto/supra_juiz.py:313`; `:435`)
- **ALTA — mesma confusão no status.** `fatos_do_status` exclui isenção, mas não dispensa. Com todos os marcos dispensados e nenhuma batida, informa `tem_batida=True`; um veredito concorde pode virar status `ok`, pulando a classificação de ausência. (`app/ponto/status_dia.py:143`; `:147`; `:98`; `:119`)
- **ALTA — cobertura ainda cala qualquer pergunta.** `_coberto_por_ausencia` considera suficiente existir ausência não-falta para cobrança, sem consultar período ou lâmpada. Parcial 14–17 com E13 apagada é cobrável na grade, mas esse caminho impede a pergunta e o diagnóstico do app aceita a cobertura como explicação. (`app/chamados/services/materializacao.py:65`; `app/chamados/services/disputa_emissao.py:763`; `app/chamados/services/pergunta_no_app.py:74`)
- **ALTA — o juiz registrado de furo continua independente da dispensa.** `classificar_falta` consulta a integralidade da ausência, depois decide por batidas E/S brutas. E08 presente e S17 dispensada por parcial 16–17 ainda podem resultar em `orfao_14h`. Esse juiz continua chamado, inclusive no caminho de disputa e no fallback da fábrica. (`app/core/juizes.py:180`; `app/escala/servico_jornada.py:341`; `:455`; `app/chamados/services/disputa_emissao.py:738`; `app/chamados/services/fabrica_da_celula.py:56`)

Para a **pergunta 3**, os contraexemplos dão estes resultados:

| Caso | Código novo versus regra pedida |
|---|---|
| **Noturna simples 19–07; parcial 23–07** | Com sequência E19/S07, S07 recebe deslocamento 1860 e é dispensada: atende ao período do dia de início. (`app/ponto/turnos.py:2636`; `:2661`; `app/escala/utils.py:1210`) |
| **Noturna com pausa 02–03** | **ALTA.** A régua usa a menor de **todas** as entradas como âncora: E03. A ordem pode virar S02/E03/S07/E19; o helper recebe essa régua e calcula S07 como 420, fora de 23–07. A regra exige a dispensa de S07 em D+1. O teste novo usa apenas E19/S07 e não pega esse caso. (`app/escala/utils.py:1335`; `:1343`; `:1805`; `app/ponto/turnos.py:2661`; `app/ponto/tests/test_cobparc_marco_no_periodo.py:56`; `app/docs/CORTES.md:329`) |
| **Período exatamente na borda do marco** | Inclusivo: missing exatamente em `hora_ini` ou `hora_fim` é dispensado. Batida real nessa borda também é considerada dentro e permanece real; a substituição só ocorre para missing. (`app/ponto/turnos.py:2639`; `app/escala/utils.py:1209`) |
| **Duas parciais no mesmo dia** | Uma única ausência vence por data/pk; não existe união dos períodos. Ex.: 08–12 e 13–17, sendo a segunda mais recente: só 13–17 alimenta a dispensa. Isso segue o desempate registrado; obrigação de unir ambas é **[nao verificado]**. (`app/ponto/turnos.py:2542`; `:2772`; `:2784`) |
| **Parcial não aprovada** | A grade usa `para='folha'`, portanto não dispensa. Para cobrança, o juiz aceita não rejeitadas; porém `_celula_furada` agora deixa a parcial passar para a ata, enquanto `_coberto_por_ausencia` continua suspendendo pelo registro. Há divergência entre leitores. O status literal `pendente` não é opção atual; existem `aguardando_documento` e `aguardando_decisao`. (`app/escala/utils.py:1149`; `app/ponto/turnos.py:2514`; `:2516`; `app/chamados/juizes.py:433`; `app/chamados/services/materializacao.py:65`; `app/ponto/models.py:701`) |
| **Justificativa parcial** | **ALTA para o pedido amplo.** Qualquer justificativa aprovada ainda limpa todos os missing do dia de trabalho e a ata dispensa o restante por `justificado`. Ex.: justificativa de S17 também apaga E08 missing. Há, entretanto, corte explícito separando essa entrega da ausência parcial; não é uma implementação concluída da justificativa por marco. (`app/escala/utils.py:1215`; `app/ponto/services/cartorio.py:627`; `:635`; `app/docs/CORTES.md:329`; `:335`) |
| **Marco do DNA fora da régua** | **ALTA.** Régua E08/S17; DNA também S12/E13; parcial 12–13. A grade só dispensa células existentes com índice em `_off`. O ramo DNA sem posição produz lâmpada apagada/indeterminada e não recebe marcador. A regra exige dispensar S12/E13. (`app/escala/utils.py:1209`; `app/ponto/services/cartorio.py:491`; `:493`; `:510`; `:513`) |
| **Dia sem DNA** | Com marcos e células existentes, o ramo por células preserva a dispensa: atende esse caso. (`app/ponto/services/cartorio.py:551`; `:560`; `app/escala/utils.py:1204`) |
| **Dia sem marcos** | Não dispensa nem inventa deslocamentos. Com células vazias, não há `FURO_PARCIAL`; ainda pode haver `FURO_SEM_COBRANCA` se existir previsto positivo. Se esse previsto é legítimo no caso real, **[nao verificado]**. (`app/ponto/turnos.py:2658`; `app/escala/utils.py:1204`; `app/ponto/supra_juiz.py:313`; `:435`) |

Para a **pergunta 4**, a mudança é uma **migração de leitor**, desde que receba o carimbo correto. O juiz considera parcial somente tipo que aceita horas, registro de um dia e `0 < minutos < 480`; o cartório agora lê esse resultado em vez de usar `minutos>0`. (`app/ponto/turnos.py:2595`; `:2598`; `app/escala/utils.py:1191`; `app/ponto/services/cartorio.py:625`)

Assim, entre ausências aprovadas com minutos positivos, passam de “não dispensar integralmente” para “dispensar integralmente”:

- tipos fora do catálogo parcial;
- registros de vários dias;
- registros com minutos iguais ou superiores a 480. (`app/ponto/turnos.py:2598`; `app/ponto/services/cartorio.py:626`)

**MÉDIA — carimbo ausente é tratado como integral.** `dia=None` ou ausência da chave também autoriza a dispensa integral. Contraexemplo: chamar a função com ausência aprovada de 90 minutos e `dia=None` acende todas as lâmpadas apagadas. Quantos chamadores reais chegam com esse insumo incompleto: **[nao verificado]**. (`app/ponto/services/cartorio.py:625`; `:627`; `:631`)

Para a **pergunta 5**, numa passada isolada, `julgada_em` é o carimbo anterior **da instância recebida**. O gate vem antes da lavra e do `carimbar`, que só depois atribui `julgada_em=agora`. `forcar=True` ignora o salto por impressão igual, mas não ignora esse gate. No primeiro julgamento, `julgada_em=None` libera emissão sem consultar `houve_fato_novo`. (`app/ponto/services/cartorio.py:1177`; `:1567`; `:1587`; `:1607`; `app/ponto/portas/celula.py:800`)

Há três falhas relevantes nessa ordem:

- **ALTA — a edição do período pode nem alcançar o gate.** Batidas 07:58/12:02; ausência muda de 13–17 para 14–17. E13 deveria voltar de dispensada para missing, mas ambos os períodos resultam em `'fora'`. A impressão guarda esse agregado, sem limites declarados ou conjunto de dispensas; com os demais insumos iguais, o julgamento é pulado e a ata anterior permanece. (`app/escala/utils.py:1195`; `:1209`; `app/ponto/services/cartorio.py:334`; `:386`; `:1169`; `:1177`)
- **ALTA — o gate só cobre `FURO_PARCIAL`.** Parcial 09–10, marcos E08/S17, nenhuma batida e nenhum marco dispensado: sai `FURO_SEM_COBRANCA`. Esse código, cobrança morta e limbo usam outro bloco de emissão. O retroativo também chama o emissor diretamente. O funil possui vetos, mas não esse gate, e pode abrir chamado e disparar push. (`app/ponto/supra_juiz.py:313`; `:367`; `app/ponto/services/cartorio.py:1509`; `:1528`; `app/ponto/management/commands/emitir_furo_retroativo.py:109`; `:142`; `app/ponto/management/commands/detectar_ausencias.py:625`; `:688`; `:769`)
- **ALTA, risco concorrente — não há releitura travada cobrindo decisão e emissão.** A entrada busca `.first()` simples; a transação da porta protege save/outbox depois da emissão. Dois julgamentos podem carregar carimbo/lista antigos e ambos atravessar `not vivos`; o emissor pode deduplicar o chamado, mas seu push fica depois da abertura. Ocorrência produtiva dessa interlevação: **[nao verificado]**. (`app/ponto/services/cartorio.py:1730`; `:1150`; `:1571`; `app/ponto/portas/celula.py:639`; `app/ponto/management/commands/detectar_ausencias.py:688`; `:769`)

Além disso, “lista sem push” não significa necessariamente “só lista administrativa”: depois do julgamento, a fábrica ainda pode criar disputa/perguntas pela ata, com push silenciado. (`app/ponto/services/cartorio.py:838`; `:840`; `app/chamados/services/fabrica_da_celula.py:124`; `:158`)

Para a **pergunta 6**, os blocos de fato e BUG-CONCORDE são `if/elif`: não acrescentam ambos o mesmo furo numa única passagem. Entretanto, o ramo final de cobertura é independente e continua considerando qualquer cobertura aprovada. (`app/ponto/supra_juiz.py:379`; `:435`; `:523`)

- **MÉDIA — dupla acusação contraditória.** Parcial 14–17, realizado 240, E13 missing, S17 dispensada e chamado vivo: saem **`FURO_PARCIAL` e `COBRANCA_EM_DIA_COBERTO`**. O mapa prioriza `furo`; essa dupla, por si só, não demonstra dois chamados. (`app/ponto/supra_juiz.py:390`; `:523`; `app/escala/models.py:1579`)
- **ALTA — novo `'fora'` esquece CADASTRO.** No mesmo caso, com `sem_vinculo_no_dia=True`, o ramo novo acusa `FURO_PARCIAL`; os outros ramos equivalentes desviam para `PREVISTO_COM_DIA_INDEFINIDO`. Alcance produtivo de parcial com previsto positivo e vínculo inválido: **[nao verificado]**. (`app/ponto/supra_juiz.py:394`; `:419`; `:443`)
- **MÉDIA — `_parcial_sem_resto` cala mais que furo.** Todos os marcos dispensados impedem a entrada no bloco inteiro, inclusive seus ramos de cadastro. Contraexemplo de entrada: previsto 480, nenhuma batida, todas as células dispensadas e `sem_vinculo_no_dia=True`: retorna sem a acusação de cadastro daquele bloco. A grade atual tende a fornecer previsto zero sem vínculo, portanto incidência real: **[nao verificado]**. (`app/ponto/supra_juiz.py:247`; `:311`; `:337`)
- Nenhum ramo foi demonstrado totalmente morto para todos os insumos; a perda de alcance identificada é condicionada pelas novas guardas. (`app/ponto/supra_juiz.py:311`; `:313`; `:435`)

Para a **pergunta 7**, o checklist fica assim:

| Classe | Resultado, gravidade e contraexemplo |
|---|---|
| Segundo juiz duplicando autoridade registrada | **ALTA.** O juiz registrado de furo continua respondendo pelas batidas brutas, enquanto grade/ata dispensam por marco: E08 real e S17 dispensada ainda podem gerar `orfao_14h`. A comparação com o período reutiliza o juiz registrado; o problema não é a existência do helper novo. (`app/core/juizes.py:180`; `:244`; `app/escala/servico_jornada.py:455`; `app/escala/utils.py:1210`) |
| Segundo escritor fora da porta | Não foi identificado novo escritor persistente fora da porta neste caminho: os dicts são montados e a gravação continua por lavra/carimbo. Há **dois materializadores** de cobertura/período, o que é problema distinto de escrita direta no banco. (`app/ponto/services/cartorio.py:1587`; `:1607`; `app/ponto/portas/celula.py:639`; `app/escala/services/leitor_celula.py:401`) |
| Relógio fora da borda | O gate novo recebe `agora` explicitamente. **MÉDIA, preexistente:** `julgar_celula` ainda consulta data/hora reais mesmo aceitando `agora`; uma reprodução com instante fornecido pode combinar relógios diferentes. (`app/ponto/services/cartorio.py:1568`; `:1719`; `:1740`; `:1743`) |
| Derivação paralela da fonte única | **ALTA.** O leitor recompõe cobertura/período atuais sobre lâmpadas persistidas; `classificar_falta` e `_coberto_por_ausencia` mantêm respostas próprias. Ex.: ata antiga dispensa E13, período atual mudou para 14–17, mas o leitor recalcula apenas os carimbos, sem retirar a dispensa antiga. (`app/escala/services/leitor_celula.py:357`; `:396`; `:404`; `app/escala/servico_jornada.py:453`; `app/chamados/services/materializacao.py:65`) |
| Selo/RED sem mordida | **MÉDIA.** O teste “com fato novo” usa `julgada_em=None`: testa primeiro julgamento. Os testes de lista forçam o julgamento e não pegam a colisão da impressão. A noturna testada não tem pausa. Resultado real RED/GREEN: **[nao verificado]**. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:234`; `:248`; `:56`) |
| Ordem de trava | **ALTA como risco de concorrência.** Gate/emissão antecedem a transação de save/outbox e usam instância sem releitura travada; dois julgamentos simultâneos podem decidir sobre o mesmo estado antigo. Duplicidade efetiva em produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:1730`; `:1571`; `:1607`; `app/ponto/portas/celula.py:639`) |

## 31/10 23:16 · sessao 0ed20c3f593c · - · - · origem=Ronald (tmux)

Achados e respostas, só código:

- **Alta — dispensa perde marcos do DNA fora da régua.** `_marcar_ausencias` só altera células existentes e índices presentes em `_off` (`app/escala/utils.py:1204-1211`). No cartório, marco do DNA sem posição na régua entra em outro ramo, que produz lâmpada apagada ou indeterminada e não consulta a dispensa (`app/ponto/services/cartorio.py:491-514`). Exemplo: régua com E08/S17; DNA inclui S12/E13; parcial 12–13; esses dois marcos deveriam ser dispensados pela regra do pedido, mas a ata pode gravá-los apagados. Não há teste novo desse ramo (`app/ponto/tests/test_cobparc_marco_no_periodo.py:159-188`).

- **Média — acusações simultâneas contraditórias.** Parcial 14–17, realizado 240, E13 missing, S17 dispensada e chamado vivo produz `FURO_PARCIAL` pelo ramo novo e `COBRANCA_EM_DIA_COBERTO` pelo ramo final, que continua usando cobertura aprovada sem distinguir parcial (`app/ponto/supra_juiz.py:390-397`, `app/ponto/supra_juiz.py:523-526`). Executei a função extraída por AST em memória e confirmei ambos os códigos. O veredito final é `furo`, porque sua precedência vem antes de `coberta`; não são dois chamados gerados só por essa dupla classificação (`app/escala/models.py:1579-1599`).

- **Alta — ramo novo esquece o dono CADASTRO.** Mesmo exemplo, com `sem_vinculo_no_dia=True`, sai `FURO_PARCIAL`; o ramo antigo com fato e o ramo BUG-CONCORDE destinam essa condição a `PREVISTO_COM_DIA_INDEFINIDO` (`app/ponto/supra_juiz.py:390-397`, `app/ponto/supra_juiz.py:414-422`, `app/ponto/supra_juiz.py:443-448`). Confirmado com execução AST. Alcance em dados reais com parcial e previsto positivo sem vínculo: [nao verificado].

- **Média — `_parcial_sem_resto` silencia também CADASTRO.** A guarda nova está antes do bloco inteiro; portanto, todos os marcos dispensados também tornam inacessíveis, nesse subconjunto, os ramos `sem_vinculo`, `sem_ancora` e `regua_degenerada` (`app/ponto/supra_juiz.py:311-358`). Exemplo AST: parcial, previsto480, realizado0, duas células dispensadas, batidas0 e `sem_vinculo_no_dia=True` retorna `[]`. O próprio código diz que a grade atual tende a entregar previsto0 quando falta vínculo; alcance concreto em produção é [nao verificado] (`app/ponto/supra_juiz.py:247-251`). Não identifiquei ramo totalmente morto em todos os insumos; trata-se de perda de alcance condicionada.

- **Alta — dia sem batida e nenhuma dispensa usa outra emissão.** Parcial sem marco dentro do período, previsto480, realizado0, nenhuma batida retorna `FURO_SEM_COBRANCA`, não `FURO_PARCIAL`; confirmado em AST (`app/ponto/supra_juiz.py:313-370`). Logo o gate novo, limitado ao emissor `FURO_PARCIAL`, não protege essa classificação (`app/ponto/services/cartorio.py:1551-1575`). Além disso, `n_bat` conta célula dispensada como batida por testar apenas `not missing`, o que pode escolher outro ramo sem haver marcação real (`app/ponto/services/cartorio.py:1186-1188`).

Contraexemplos:

| Caso | Resultado do código e limite |
|---|---|
| Borda exata | Inclusiva: `ini <= m <= fim`; missing exatamente em início/fim é dispensado (`app/ponto/turnos.py:2634-2641`, `app/escala/utils.py:1209-1210`). |
| Noturna simples 19–07, parcial23–07 | A função mede S07 em 1860 se recebe sequência E19/S07; teste novo só cobre essa sequência simples (`app/ponto/turnos.py:2644-2661`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:56-75`). Noturna com pausa/globalização da régua: achado confirmado pelo agente principal [nao verificado por este agente]. |
| Duas parciais no mesmo dia | A cobertura escolhe uma única ausência, a mais recente por data/pk; a segunda sobrescreve a primeira, sem união de períodos (`app/ponto/turnos.py:2542-2557`, `app/ponto/turnos.py:2772-2785`). Exemplo: ausência 08–12 e outra13–17, maior pk na segunda, só13–17 dispensa. A cópia registra esse desempate; obrigação de unir períodos não está estabelecida [nao verificado]. |
| Parcial não aprovada | A grade consulta `para='folha'`, que filtra só aprovada; não aplica dispensa (`app/escala/utils.py:1149-1152`, `app/ponto/turnos.py:2514-2517`). Ausência não possui status vivo literal `pendente`; usa aguardando_documento/aguardando_decisao (`app/ponto/models.py:690-705`). |
| Justificativa parcial | A grade ainda apaga todos os missing do dia de trabalho e marca `justificado=True`; efeito financeiro continua tomando a data inteira (`app/escala/utils.py:1215-1220`, `app/ponto/services/efeito_ausencia.py:36-41`). Exemplo esqueci_saida cobrindo S17 também apaga E08 missing. Isso não atende o pedido amplo atual, mas a separação da entrega de justificativa foi explicitamente registrada em `app/docs/CORTES.md:329`. |
| Dia sem DNA | Cartório usa ramo por células; dispensa é propagada normalmente se a grade tinha marcos e missing para substituir (`app/ponto/services/cartorio.py:551-561`, `app/escala/utils.py:1204-1211`). |
| Dia sem marcos | A grade não dispensa nada; helper devolve `{}` e não inventa marcos (`app/escala/utils.py:1204`, `app/ponto/turnos.py:2658-2659`). Com células missing existentes, a ata pode continuar produzindo lâmpadas a partir das células (`app/ponto/services/cartorio.py:552-561`). Com células vazias, não há lâmpadas nem `FURO_PARCIAL`; eventual acusação sem batida depende de previsto (`app/ponto/supra_juiz.py:305-316`, `app/ponto/supra_juiz.py:435-436`). |

Os blocos de fato e BUG-CONCORDE são `if/elif`, então não acrescentam ambos `FURO_PARCIAL`/`FATO_EM_AUSENCIA` na mesma execução; o furo sem batida é outro `if` independente, protegido normalmente pelo insumo `batidas_no_dia` (`app/ponto/supra_juiz.py:313-316`, `app/ponto/supra_juiz.py:379-448`). A duplicação efetivamente confirmada é com `COBRANCA_EM_DIA_COBERTO` (`app/ponto/supra_juiz.py:523-526`).

O teste chamado “com fato novo” monta `julgada_em=None`; testa primeiro julgamento, não alteração de fatos posterior a julgamento existente (`app/ponto/tests/test_cobparc_marco_no_periodo.py:248-252`). Portanto sua cobertura desse requisito específico não está demonstrada [nao verificado].

## 31/10 23:15 · sessao e69c9b99317c · - · - · origem=Ronald (tmux)

Censo concluído; nenhuma ocorrência executável dos campos nomeados apareceu fora de `app` na busca que incluiu arquivos ocultos e ignorados. [nao verificado: arquivos inacessíveis]

Achados principais:

- **P1 — dispensa vira batida fictícia.** A grade cria `missing=False, hora=None` em `app/escala/utils.py:1207`; o cartório conta isso como batida em `app/ponto/services/cartorio.py:1186`, e o status também em `app/ponto/status_dia.py:143`. Sem batidas reais, um marco dispensado e outro missing desviam do furo sem batida (`app/ponto/supra_juiz.py:313`) para o ramo de “batida sem par” (`app/ponto/supra_juiz.py:435`). Com todos dispensados, `tem_batida=True` pode produzir status `ok`, em vez de ausência (`app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:119`).
- **P1 — leitor ainda cala o restante da parcial.** `_coberto_por_ausencia` responde pela existência de qualquer ausência não-falta, sem período ou lâmpada (`app/chamados/services/materializacao.py:65`). A fábrica antiga suprime perguntas por essa resposta (`app/chamados/services/disputa_emissao.py:763`); o diagnóstico do app também considera a ausência explicação suficiente (`app/chamados/services/pergunta_no_app.py:74`). Exemplo: parcial 14–17, E13 apagada; grade cobra E13, esse caminho impede sua pergunta (`app/escala/utils.py:1209`, `app/chamados/services/disputa_emissao.py:763`).
- **P1/P2 — juiz registrado continua derivando falta sem dispensa por marco.** `classificar_falta` consulta `cobre_parte_do_dia` (`app/escala/servico_jornada.py:341`), depois decide por E/S brutos (`app/escala/servico_jornada.py:453`). E08 presente e S17 dispensada pela parcial 16–17 continuam retornando `orfao_14h` (`app/escala/servico_jornada.py:455`). Há chamadores em produção (`app/escala/servico_jornada.py:557`, `app/chamados/services/disputa_emissao.py:738`) e fallback da fábrica (`app/chamados/services/fabrica_da_celula.py:56`).

Inventários completos dos campos, testes e oito chamadores de `dia_sem_trabalho`, com impacto por leitor, enviados nas mensagens anteriores. [nao verificado]

## 31/10 23:15 · sessao 8967a7c259ea · - · - · origem=Ronald (tmux)

Achados estáticos para Q4/Q5/Q7, sem execução de testes nem acesso a dados produtivos [nao verificado]. [nao verificado]

1. **ALTO — impressão não registra o período declarado nem o conjunto de dispensas.** O hash contém batidas, status da cobertura, chamados, DNA e apenas o resultado agregado `periodo_parcial` (`fora`, `dentro` etc.); não contém `hora_ini/hora_fim` nem `celulas[].dispensada`. A chamada passa o agregado e pula o julgamento se o hash não mudou: `app/ponto/services/cartorio.py:334`, `:386`, `:1164`, `:1169`, `:1173`, `:1177`. Contraexemplo: batidas 07:58/12:02, ausência aprovada 13:00–17:00 alterada para 14:00–17:00; ambas dão `fora`, mas E13 muda de dispensado para missing pela grade (`app/escala/utils.py:1195`, `:1206`, `:1210`). Sem `forcar`, pode ficar a ata anterior, e `houve_fato_novo` nem chega a ser chamado porque está depois do pulo (`app/ponto/services/cartorio.py:1177`, `:1567`). Os testes desta obra chamam sempre com `forcar=True`, portanto não cobrem isso (`app/ponto/tests/test_cobparc_marco_no_periodo.py:233`).

2. **ALTO — `n_bat` passa a contar dispensa sem hora como batida.** A nova célula é `missing=False, hora=None` (`app/escala/utils.py:1207`), e o cartório conta todo dict não missing (`app/ponto/services/cartorio.py:1186`). Contraexemplo: nenhum ponto real, E08 dispensada, S17 missing: `batidas_no_dia=1`; muda a seleção dos ramos do supra, inclusive deixa de satisfazer o ramo sem batidas e pode entrar no ramo BUG-CONCORDE (`app/ponto/supra_juiz.py:314`, `:438`). Isso é alteração indireta fora das linhas do diff que mexem no supra (`app/ponto/services/cartorio.py:1186`).

3. **Migração de `dispensar_por_ausencia`:** agora usa `status='aprovada' && !dia.cobertura_parcial`, depois acende toda lâmpada não acesa (`app/ponto/services/cartorio.py:625`, `:631`). A grade usa `cobre_parte_do_dia` (`app/escala/utils.py:1149`, `:1151`, `:1191`), cujo critério é tipo no catálogo parcial + um único dia + `0 < minutos < 480` (`app/ponto/turnos.py:2595`). Portanto mudam as aprovadas com minutos>0 e **tipo fora do catálogo parcial, período de mais de um dia, ou minutos≥480**; deixam de ser bloqueadas pelo antigo teste de minutos e passam a dispensa integral (`app/ponto/turnos.py:2598`, `app/ponto/services/cartorio.py:625`). É migração para insumo da grade, sem juiz novo nessa função; porém `dia=None` ou carimbo ausente também vira “integral” (`app/ponto/services/cartorio.py:626`). Quantidades/casos reais dessas classes [nao verificado].

4. **Ordem temporal do gate está correta numa passada isolada:** lê `cel.julgada_em` antes da lavra e antes do `carimbar`; a porta só atribui `julgada_em=agora` depois (`app/ponto/services/cartorio.py:1567`, `:1587`, `:1607`; `app/ponto/portas/celula.py:800`). Primeiro julgamento (`None`) emite; `forcar=True` atravessa apenas o pulo do hash, não o gate de lista (`app/ponto/services/cartorio.py:1177`, `:1567`). O teste chamado “com fato novo” testa `julgada_em=None`, portanto primeiro julgamento; não demonstra fato novo posterior a um carimbo anterior (`app/ponto/tests/test_cobparc_marco_no_periodo.py:248`).

5. **O gate lê o carimbo da instância recebida, sem releitura travada.** `julgar_celula` busca `.first()` simples e entrega a instância; o corpo opera nela (`app/ponto/services/cartorio.py:1730`, `:1743`, `:1119`). A transação da porta só envolve o save e outbox, já depois da emissão (`app/ponto/portas/celula.py:639`, `app/ponto/services/cartorio.py:1572`, `:1607`). Contraexemplo concorrente: dois julgamentos carregam a mesma instância anterior e ambos chegam a `not vivos`; não há serialização do trecho leitura→gate→emissão no cartório (`app/ponto/services/cartorio.py:1150`, `:1567`, `:1571`). Efeito concreto em produção [nao verificado]; o funil pode deduplicar chamado, mas faz push mesmo ao encontrar um vivo (`app/ponto/management/commands/detectar_ausencias.py:688`, `:734`, `:769`).

6. **Há caminhos de emissão fora do novo gate.**
   - No próprio cartório, `FURO_SEM_COBRANCA`, `FURO_COM_COBRANCA_MORTA` e limbo entram em outro bloco e chamam `_abrir_chamado` sem `_parcial_so_lista` (`app/ponto/services/cartorio.py:1509`, `:1528`).
   - `emitir_furo_retroativo` seleciona `FURO_SEM_COBRANCA` e chama diretamente o mesmo emissor (`app/ponto/management/commands/emitir_furo_retroativo.py:109`, `:142`).
   - O detector corrente também chama o funil diretamente (`app/ponto/management/commands/detectar_ausencias.py:439`).
   - O funil verifica veto da célula, mas não consulta o novo gate; cria em `:688` e faz push de supervisão para escala não flexível em `:769` (`app/ponto/management/commands/detectar_ausencias.py:625`, `:688`, `:769`). O veto compartilhado lê só trabalha/veredito, não carimbo de parcial nem fato novo (`app/ponto/marcos.py:155`).
   - A fábrica posterior do cartório continua criando perguntas/disputa pela ata, mesmo quando o novo gate impediu chamado; o push dessa fábrica é silenciado (`app/ponto/services/cartorio.py:838`, `app/chamados/services/fabrica_da_celula.py:124`, `:158`). Logo “sem chamado/push” não significa “sem pergunta ao colaborador” (`app/ponto/services/cartorio.py:1565`, `app/chamados/services/fabrica_da_celula.py:160`).

7. **Relógio:** este uso passa `agora` explicitamente a `houve_fato_novo`, então não abre leitura nova de relógio no caminho normal (`app/ponto/services/cartorio.py:1568`). O helper tem fallback `ate or timezone.now()` para outros chamadores (`app/ponto/services/cartorio.py:276`). Os escritores considerados são batidas criadas/retratadas, ausência e justificativa criadas/atualizadas/aprovadas, DNA editado/regenerado e marco vencido apagado (`app/ponto/services/cartorio.py:283`, `:286`, `:290`, `:296`, `:298`). O helper consulta a ata persistida para o marco vencido, e não `_ata_agora` (`app/ponto/services/cartorio.py:320`).

8. **Teste sem mordida para o fato novo real:** faltam no arquivo da obra testes com `julgada_em!=None` e ausência/batida posterior, testes sem `forcar`, e testes de concorrência; os dois testes de lista usam ausência criada na fixture e AGORA fixado antes da criação em tempo real, tornando o limite superior relevante (`app/ponto/tests/test_cobparc_marco_no_periodo.py:140`, `:226`, `:241`, `:248`; filtro `(desde, ate]` em `app/ponto/services/cartorio.py:278`). Resultado RED/GREEN e datas efetivas do ambiente da execução [nao verificado].

## 31/10 23:00 · sessao 77d1b4ef88aa · O261-E-F1C4-LEN-VIVOS · verificador · origem=Code

**A volta 2 corrige a contagem de sucesso, e o mock morde a regressão indicada.** O incremento agora depende do retorno da porta; a recusa tem contador próprio. (`app/ponto/management/commands/supra_juiz.py:286–290`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:162–168`)

1) **Quem escreve e quem lê**

- **Escritor das duas chaves:** o command incrementa `CSF_RETRATADO` quando `_retr(...)` retorna verdadeiro e `CSF_RETRATACAO_RECUSADA` no ramo contrário. A porta real retorna `False` em retenção ou reconciliação negada, e `True` após concluir a unidade. (`app/ponto/management/commands/supra_juiz.py:286–290`; `app/chamados/reconciliador.py:520–548`)
- **Leitor genérico:** o próprio command percorre o `placar` e imprime chave e valor. O cron encaminha stdout/stderr ao log; seu envelope registra duração/exit, sem interpretar essas chaves. (`app/ponto/management/commands/supra_juiz.py:299–300`; `app/config/crons.py:1801–1802`; `bin/cron_run.sh:11–13`)
- **Leitores nominais encontrados nos testes:** golden exige `CSF_RETRATADO`; lavra sem código proíbe essa chave; teste da recusa proíbe sucesso e exige `CSF_RETRATACAO_RECUSADA`. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:122,153,167–168`)
- **Documentos que usam `CSF_RETRATADO`:** condição de medição zero em `app/docs/PROMPTS.md:832,836` e `app/docs/BACKLOG.md:1297`; registro histórico de cinco linhas com valor 1 em `app/docs/BACKLOG.md:1299`; registro histórico de uma linha com valor 1 em `app/docs/RELATO-ARQUIVO.md:17919`. São relatos documentais, não medições atuais verificadas. [não verificado]
- **A6 não lê nenhuma das duas:** consulta somente `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`. (`app/ponto/management/commands/supra_juiz.py:304–310`)

**Consequência para as medições:** agora “`CSF_RETRATADO=0`” pode coexistir com tentativas recusadas. Portanto, zero sucesso sozinho não demonstra ausência de trabalho residual; os números históricos também não comprovam sucessos sob a semântica corrigida. (`app/ponto/management/commands/supra_juiz.py:286–290`; `app/docs/BACKLOG.md:1299`)

2) **O mock alcança o import e morde? Sim.**

O patch permanece ativo durante `_executa()`. Nesse intervalo, o comando executa `from chamados.reconciliador import retratar as _retr`, capturando o atributo já substituído. A importação local está dentro do laço dos dias, antes do laço dos chamados. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:162–163`; `app/ponto/management/commands/supra_juiz.py:205,258,260`)

Voltando a `_retr(...); placar['CSF_RETRATADO'] += 1`, falham a proibição de sucesso e a exigência de recusa. O espaço final em `'CSF_RETRATADO '` funciona porque a impressão acrescenta espaços depois da chave. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:167–168`; `app/ponto/management/commands/supra_juiz.py:300`)

O teste cobre a reação ao retorno `False`; não exercita as causas reais da recusa, pois substitui a porta. Suíte Django completa verde: [não verificado]. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:162`)

3) **Achados que permanecem e limites novos**

- **Lavra ausente:** runtime preserva o chamado, conta e continua; não aborta. Isso coincide com o teste. Não contesto a interpretação de “PAREI” como condição prévia do construtor, mas a cobertura da medição zero informada não está demonstrada pelos anexos. (`app/ponto/management/commands/supra_juiz.py:247–248`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:134–140`; cobertura: [não verificado])
- **A6:** permanece procedente a lacuna observacional; nenhuma chave nova participa do alarme. (`app/ponto/management/commands/supra_juiz.py:305–306`)
- **Ordem dos crons:** permanece o risco. O executor declara dependência do cartório, porém sua linha é renderizada por horário e executada diretamente, sem espera pelo arquivo do cartório. Ocorrência efetiva em produção: [não verificado]. (`app/config/crons.py:580,1785–1814`; `bin/cron_run.sh:11`)
- **Limite adicional de cobertura, preexistente:** colaborador sem batidas na janela expandida é pulado **antes** de carregar chamados e lavra. Assim, um vivo desse colaborador não entra na retratação nem em `CSF_SEM_LAVRA`. O contador zero, isoladamente, não constitui censo de todos os vivos. Existência desses casos em produção: [não verificado]. (`app/ponto/management/commands/supra_juiz.py:102–118,165,190,247`)
- **Derivação paralela remanescente:** o relatório do command continua usando `len(vivos)`; o cartório usa `len(_protestam)`. Essa diferença já não decide a retratação. `emitir_furo_retroativo` também mantém sua classificação com `len(vivos)` para decidir emissão. (`app/ponto/management/commands/supra_juiz.py:223–250`; `app/ponto/services/cartorio.py:1206–1214`; `app/ponto/management/commands/emitir_furo_retroativo.py:99–109,142`)

**No caminho alterado, não identifiquei segundo juiz nem escrita fora da porta:** lê lavra, obtém o código do mapa canônico, aplica as guardas e chama o retratador existente. O selo com mock tem mordida para a regressão solicitada. (`app/ponto/management/commands/supra_juiz.py:49–54,246–290`; `app/chamados/reconciliador.py:546–548`)

A ressalva documental concreta é o `--help`, ainda limitado a `COBRANCA_SEM_FURO`. Já `CLAUDE.md:432` não prova contradição sozinho: mudar o classificador compartilhado ainda pode alterar a lavra pelo cartório e, indiretamente, a retratação. (`app/ponto/management/commands/supra_juiz.py:63`; `app/ponto/services/cartorio.py:1212–1214`; `CLAUDE.md:432`)

## 31/10 22:59 · sessao 220c89f99050 · - · - · origem=Ronald (tmux)

- A correção do retratador está efetiva no código: lê `veredito/protestos` persistidos, exige `cobranca_indevida`, obtém o código do mapa canônico, preserva as guardas de conversa e regeneração e delega a escrita a `chamados.reconciliador.retratar`; só incrementa sucesso se a porta retornar verdadeiro. Não identifiquei escrita direta nem novo julgamento nesse caminho. (`app/ponto/management/commands/supra_juiz.py:165`, `:246`, `:250`, `:258`, `:261`, `:275`, `:286`; `app/escala/models.py:1579`; `app/chamados/reconciliador.py:546`)

- A porta realmente pode recusar: retenção retorna `False`, `ch.reconciliar()` negado retorna `False`, e sucesso retorna `True` depois da unidade atômica. O julgamento de fechamento permanece na autoridade existente do modelo, incluindo premissa e pergunta viva. (`app/chamados/reconciliador.py:520`, `:525`, `:543`; `app/chamados/models.py:615`, `:646`, `:649`)

- **Volta 1, A6: procede.** Só consulta `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`; nenhuma chave nova entra no exit code. Estar fora do aval é interpretação de escopo fornecida pelo usuário, não defeito novo desta alteração. (`app/ponto/management/commands/supra_juiz.py:304`; escopo: [não verificado])

- **Volta 1, cron: risco procede no código.** Existe `depende=('processar_cartorio',)` declarado, mas `linhas()` renderiza horário e execução direta, sem consumir `depende`; o envelope executa imediatamente o comando recebido. A espera por arquivo está no mecanismo distinto dos encadeados. Logo, a declaração não garante que o executor espere a lavra. Ocorrência efetiva em produção: [não verificado]. (`app/config/crons.py:580`, `:1785`, `:1801`, `:1812`; `bin/cron_run.sh:11`; `bin/encadeado.sh:77`)

- **Novo limite identificado, pré-existente:** colaborador sem batidas na janela expandida é pulado antes de carregar chamados e lavra; portanto não entra no executor nem em `CSF_SEM_LAVRA`, mesmo que tenha chamado vivo. Não é estreitamento novo da lista `vivos`, mas significa que o contador não prova ausência de lavra para todos os vivos sem declarar esse denominador. A medição “zero dias” fornecida não revela seu universo; existência de casos omitidos em produção: [não verificado]. (`app/ponto/management/commands/supra_juiz.py:102`, `:110`, `:117`, `:165`, `:190`, `:212`, `:247`)

- **Documentação viva ainda contraditória:** `CLAUDE.md` continua dizendo que mudar `classificar_dia` muda a retratação em produção, e o help do argumento ainda descreve apenas `COBRANCA_SEM_FURO`; o docstring principal corrigido diz que a escrita depende da lavra. (`CLAUDE.md:432`; `app/ponto/management/commands/supra_juiz.py:63`, `:20`, `:246`)

- **Derivação paralela remanescente:** o placar/alarme do command ainda usa `len(vivos)`, enquanto o cartório usa `len(_protestam)`; agora essa divergência não comanda a retratação. `emitir_furo_retroativo` também mantém `len(vivos)` e sua própria chamada ao mesmo classificador, com emissão condicionada a `FURO_SEM_COBRANCA`; é remanescente fora da mudança do retratador, não evidência de um juiz novo introduzido pela volta 2. (`app/ponto/management/commands/supra_juiz.py:223`, `:235`, `:246`; `app/ponto/services/cartorio.py:1206`, `:1212`; `app/ponto/management/commands/emitir_furo_retroativo.py:99`, `:109`, `:142`)

- Ausência de lavra conta e preserva o chamado, sem abortar o command; o teste exige exatamente isso. A leitura do “PAREI” como portão de medição prévia é compatível com esse desenho, mas o código e os anexos não permitem provar que o portão de produção foi satisfeito. (`app/ponto/management/commands/supra_juiz.py:247`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:134`; medição/portão: [não verificado])

## 31/10 22:59 · sessao e6cac34f08b5 · - · - · origem=Ronald (tmux)

Sim, o teste morde a regressão: força `False`, exige uma chamada, preserva o chamado vivo, proíbe `CSF_RETRATADO ` e exige `CSF_RETRATACAO_RECUSADA` (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:162`, `:164`, `:166`, `:167`, `:168`). A saída imprime espaços depois da chave, portanto a asserção com espaço final encontra um incremento indevido (`app/ponto/management/commands/supra_juiz.py:300`).

O patch alcança `_retr`: a importação local acontece durante `_executa()`, dentro do contexto ativo do patch, e captura o atributo substituído de `chamados.reconciliador` (`app/ponto/management/commands/supra_juiz.py:258`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:162`, `:163`).

Executei em memória o ramo AST real e um mutante com incremento incondicional: o atual passou nas três verificações equivalentes; o mutante falhou nas duas verificações do placar. Essa demonstração cobre o ramo e a formatação, não a execução Django completa (`app/ponto/management/commands/supra_juiz.py:286`, `:288`, `:290`, `:300`); suíte Django verde: [não verificado].

O teste valida a reação ao contrato `False`; as causas reais ficam substituídas pelo mock (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:162`). A implementação real devolve `False` em retenção ou `reconciliar()` negado e `True` após a unidade, propagados pelo wrapper público (`app/chamados/reconciliador.py:520`, `:523`, `:525`, `:526`, `:543`, `:548`).

## 31/10 22:58 · sessao 602e0d9b8696 · - · - · origem=Ronald (tmux)

Inventário na cópia, incluindo arquivos ocultos:

- **Escreve ambos:** `app/ponto/management/commands/supra_juiz.py:286` testa o retorno da porta; incrementa `CSF_RETRATADO` em `:288` e `CSF_RETRATACAO_RECUSADA` em `:290`.
- **Lê ambos genericamente:** o próprio command percorre `placar.most_common()` e imprime as chaves/valores (`app/ponto/management/commands/supra_juiz.py:299`, `:300`).
- **Leitores nominais executáveis encontrados:** testes: golden exige retratado (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:122`); lavra sem código proíbe retratado (`:153`); mock proíbe retratado e exige recusada (`:167`, `:168`). Não localizei parser nominal dessas chaves fora disso; abrangência externa à cópia [não verificado].
- **A6 não lê nenhuma delas:** a lista `ZERO` contém apenas `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO` (`app/ponto/management/commands/supra_juiz.py:305`, `:306`).
- **Transporte cron:** executor está em `app/config/crons.py:580`; stdout/stderr seguem para log pelo renderizador (`app/config/crons.py:1801`, `:1802`); o envelope registra duração/exit, sem interpretar os contadores (`bin/cron_run.sh:11`, `:13`).

**Docs e medições que consomem `CSF_RETRATADO`:**

- Exigem medição zero como condição para retirar `--executar`: `app/docs/PROMPTS.md:832`, `:836`; `app/docs/BACKLOG.md:1297`.
- Registram historicamente **cinco linhas de valor 1, não zero**: `app/docs/BACKLOG.md:1299`.
- Registram historicamente **`CSF_RETRATADO 1`**: `app/docs/RELATO-ARQUIVO.md:17919`.
- Nenhuma ocorrência documental de `CSF_RETRATACAO_RECUSADA` localizada além da explicação do teste (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:155`).

**Achado documental:** o docstring novo abandonou a classificação do command como decisão de escrita (`app/ponto/management/commands/supra_juiz.py:21`), mas `CLAUDE.md:432` ainda diz que mudar `classificar_dia` muda o retratado em produção; a tabela de cron ainda descreve a retratação pelos códigos (`app/docs/CRONS.md:64`, `:65`). O ramo efetivo agora lê lavra (`app/ponto/management/commands/supra_juiz.py:246`, `:249`, `:250`).

A condição documental “`CSF_RETRATADO` zero” passa a provar somente ausência de retornos verdadeiros, podendo coexistir com recusas; A6 não cobre as recusas (`app/ponto/management/commands/supra_juiz.py:286`, `:290`, `:305`). Quantidades atuais em produção [não verificado].

## 31/10 22:44 · sessao 36fc57470bf3 · O261-E-F1C4-LEN-VIVOS · verificador · origem=Code

**Parecer: não aprovar ainda.** A decisão de retratar passou a consumir a lavra, mas o requisito **“lavra ausente em qualquer dia do universo = PAREI com o número”** não está atendido: o command conta ausência somente quando há chamados vivos, continua executando e não inclui essa ocorrência no alarme. O número de lavras ausentes em produção é **[nao verificado]**. (`app/ponto/management/commands/supra_juiz.py:245–252,278–305`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:3–5`)

A execução da suite e a confirmação do histórico Git `3e5ea58a..1594b9f2` são **[nao verificado]**; as conclusões abaixo são de leitura estática da cópia e do diff fornecido.

1. **Quem escreve e quem lê**

| Campo/valor | Escritor | Leitor no caminho afetado |
|---|---|---|
| `CelulaDia.veredito` | A porta atribui e persiste; o método do modelo delega. Chamadores produtivos: três pontos do cartório e a decisão administrativa. | O command carrega a lavra e exige `cobranca_indevida` para entrar na retratação. |
| `CelulaDia.protestos` | A mesma porta grava os códigos recebidos do cartório; a decisão administrativa preserva os anteriores. | O command procura um código pertencente à lâmpada e o passa ao texto da retratação. |
| `ChamadoColaborador.status_local` | `retratar → _retratar → reconciliar → _gravar_estado`; a transição `RECONCILIAR` leva os três estados vivos a `resolvido`, quando as guardas permitem. | O command seleciona `VIVOS`; a porta relê o estado e consulta transição, premissa e pergunta viva antes de fechar. |
| `CSF_SEM_LAVRA` | Incrementado uma vez por dia percorrido com vivos e `veredito is None`. | Impressão genérica do `Counter` e asserção do novo teste. | [nao verificado]
| `CSF_LAVRA_SEM_CODIGO` | Incrementado uma vez por dia com vivos, `cobranca_indevida` e nenhum código reconhecido nos protestos. | Impressão genérica do `Counter`; consumidor específico adicional: **[nao verificado]**. |

Fontes da tabela: `app/ponto/portas/celula.py:737–775`; `app/escala/models.py:1787–1796`; `app/ponto/services/cartorio.py:734,1044,1565`; `app/ponto/services/veredito_celula.py:330`; `app/ponto/management/commands/supra_juiz.py:164–165,211,245–252,283–295`; `app/chamados/reconciliador.py:505–548`; `app/chamados/models.py:222,279,605–657`; `app/chamados/catalogo/motor.py:24,111`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:140`.

O vocabulário vem de `_MAPA_VEREDITO`: `cobranca_indevida` corresponde a `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_SEM_FURO`. O helper novo apenas extrai esse conjunto. O administrador só pode lavrar `trabalhou`, `faltou` ou `justificado`, e sua decisão prevalece sobre o cartório. (`app/escala/models.py:1579–1599,1727`; `app/ponto/management/commands/supra_juiz.py:48–53`; `app/ponto/portas/celula.py:655–664,748–751`)

No código produtivo, a atribuição persistente de `veredito/protestos` está na porta; há também atribuição de `veredito` numa cópia exclusivamente em memória. Quando o cartório usa `save=False`, `carimbar` persiste os campos lavrados pela mesma porta. A migration citada apenas adiciona campos nulos. (`app/ponto/portas/celula.py:667–682,769–775,787–807`; `app/escala/migrations/0036_b5_celula_veredito.py:13–21`)

Para não esconder escritores de fixtures, estes são os sítios adicionais de escrita direta identificados, incluindo criação, `update`, atribuição em memória e helpers com `**kwargs`. Os diretórios da primeira coluna prefixam os arquivos da segunda; exaustividade contra escrita dinâmica arbitrária é **[nao verificado]**.

| Diretório | Arquivos:linhas |
|---|---|
| `app/api/tests/` | `test_diagnostico_escala.py:65` |
| `app/chamados/tests/` | `_celula_absolve.py:21`; `test_adesao_agrupa_so_vivo.py:45`; `test_b2d_resposta_visivel.py:97`; `test_bug117_marco_no_espelho.py:60`; `test_bug125_cobranca_cega.py:71,74,153`; `test_c2_le_o_protesto_que_cobra.py:40`; `test_c5_canal.py:28`; `test_c5_regeneracao.py:24`; `test_colab_nao_vaza_fio_mudo.py:32`; `test_e2_pergunta_so_ao_c6.py:51`; `test_emissao_reata_orfa.py:37,91`; `test_emissao_reemite_c2.py:48,61`; `test_f1c4_3_emissores.py:71`; `test_fabrica_pela_ata.py:29`; `test_fabrica_por_evento.py:27,51`; `test_fio_disputa_resolvido.py:48`; `test_fio_le_o_juiz.py:52`; `test_limbo_nao_fecha_com_filho_no_defeito.py:68`; `test_o261_c_viva_sem_chamado_do_dia.py:99`; `test_o261_resposta_vazia.py:46`; `test_o261e_f1_porta_encerramento.py:183`; `test_orfa_sem_candidato.py:41`; `test_orfas_nao_morre_na_guarda_da_porta.py:65,97`; `test_orfas_pelo_evento.py:63,306,310,369`; `test_pergunta_do_dia_em_curso.py:46,49,179,194,208,226,336`; `test_pergunta_respondida_e_marco_aceso.py:44`; `test_reabrir_orfas_enterradas.py:58,80`; `test_reconciliar_pergunta_a_premissa.py:171,223,228,240,250`; `test_reemissao_c2.py:53`; `test_resposta_tardia.py:40`; `test_selo_celula_absolveu.py:53`; `test_selo_disparo_nao_cobra_desligado.py:71`; `test_selo_emissor_fio_mudo.py:70,262`; `test_selo_f1_cego.py:90,136`; `test_selo_memo_pergunta_viva.py:130,135,144`; `test_selo_piso_i6_da_lei.py:72`; `test_tranca_de_verdade.py:60` |
| `app/colaboradores/tests/` | `test_calendario_hoje_nao_e_falta.py:74`; `test_calendario_um_juiz.py:146`; `test_tela_folga_rotulo.py:51` |
| `app/escala/tests/` | `test_cobertura_parcial_periodo.py:280`; `test_propositor_hora_errada.py:83`; `test_semana_do_posto.py:76` |
| `app/folha/tests/` | `test_furo_so_intervalo_nao_retem.py:74`; `test_haiku_fora_da_folha.py:51,54,376`; `test_o259_vigia_pausa.py:54`; `test_o48_passo4_simular_folha_lido.py:108,110`; `test_previa_cobre_pelo_juiz.py:59,63,73,80,87,94,100,105`; `test_txt_le_a_celula_e_o_juiz.py:136`; `test_vigia_retido_calado.py:79,81` |
| `app/pautas/tests/` | `test_pauta_do_dia.py:28` |
| `app/ponto/tests/` | `test_a2_precedencia.py:200`; `test_ausencias_sem_efeito.py:32,41`; `test_ausencias_sem_efeito_pelo_juiz.py:36`; `test_bateria_lavratura.py:128,292,1162`; `test_cartorio_so_chamado_de_batida.py:87`; `test_fechar3_troca_escala_gate.py:142,186,232,269`; `test_furo_cobranca_morta_reabre.py:114`; `test_furo_parcial_sem_cobranca.py:223`; `test_furos_sem_cobranca_portas.py:31`; `test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:52`; `test_o320_pauta_dos_discordantes.py:40,183,206`; `test_o322_afastado_nao_se_cobra.py:119`; `test_ordem_regen_julga_antes_do_dinheiro.py:121`; `test_orfas_vigia.py:55`; `test_selo_b5u_nunca_bateu_ve_ausencia.py:111`; `test_t31_gate_celula.py:22`; `test_varredura_classe_b_ausencia.py:80`; `test_vigia_ausencia_veredito.py:28` |
| `app/relatorios/tests/` | `test_cauda_trilha_do_cartao.py:104`; `test_dia_da_frota.py:33,103` |

Os testes que chamam `lavrar_veredito`, inclusive o novo, compartilham o escritor canônico; o selo de escritores exclui deliberadamente `tests` e `migrations` da varredura produtiva. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:83–87`; `app/ponto/tests/test_selo_veredito_so_pela_porta.py:171–176`)

Os consumidores de placar/stdout identificados são:

- **Dentro do command:** impressão de todas as chaves; TOP, exemplos e dump; alarme A6. As chaves novas aparecem no placar, mas não alimentam TOP/exemplos/dump nem o A6. (`app/ponto/management/commands/supra_juiz.py:234–252,292–314`)
- **Teste que captura stdout:** `_executa` e `_codigos_do_command`; asserções dos códigos diagnósticos, `CSF_RETRATADO` e `CSF_SEM_LAVRA`. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:89–104,110,122,128,140`)
- **Crons:** executor e dois alarmes; stdout/stderr são redirecionados aos respectivos logs. (`app/config/crons.py:580–585,1801–1802`)
- **Scripts:** `sombra.sh` lê saída para reconhecer erro de argumentos e classifica o exit code; `cron_run.sh` registra nome, empresa, duração e saída, sem interpretar o placar. (`bin/sombra.sh:381–399`; `bin/cron_run.sh:11–13,22–35`)
- **Leitores da telemetria:** pipeline, detector de crons quebrados e relatório de duração. O teste de crons quebrados aceita o exit 2 do supra como alarme de negócio. (`app/core/management/commands/pipeline_placar.py:60–109`; `app/core/services/crons_quebrados.py:24–47`; `bin/crons_duracao.py:61–76`; `app/core/tests/test_crons_quebrados.py:45`)
- **Contratos documentais:** referências a logs e à medição de `CSF_RETRATADO`. (`app/docs/CRONS.md:64–73`; `app/docs/BACKLOG.md:1297–1299`; `app/docs/PROMPTS.md:832–836`)

Outro parser específico dos logs do supra ou enumeração externa fechada de suas chaves: **[nao verificado]**.

2. **Segundo juiz e pergunta respondida**

**Não nasce outro juiz de mérito.** O helper lê o mapa existente, e o retratador passa a perguntar: **“a célula deste dia foi lavrada como `cobranca_indevida`, com qual código?”** A autoridade é o julgamento lavrado pelo cartório, pela porta da célula; isso corresponde à L-061. (`app/ponto/management/commands/supra_juiz.py:48–53,245–252`; `app/ponto/services/cartorio.py:1212–1223,1565`; `app/ponto/portas/celula.py:745–775`; `app/docs/LEIS.md:95`)

Vida do chamado, transição, premissa, pergunta viva, cobrança órfã e dia sem trabalho continuam com suas autoridades registradas. A leitura da lâmpada não substitui essas guardas. (`app/core/juizes.py:256–273`; `app/chamados/models.py:635–653`; `app/docs/JUIZES.md:40–60`)

O command **continua recalculando contradições para placar/alarme**, inclusive com `len(vivos)`. Isso permanece uma avaliação diagnóstica paralela à lavra; o diff retira dela a autorização de escrita. (`app/ponto/management/commands/supra_juiz.py:222–248`)

3. **Fonte única, relógio, porta e mordida dos testes** [nao verificado]

A decisão nova consome a fonte lavrada e extrai seu vocabulário do mapa existente. O bloco não acrescenta leitura de relógio nem atribuição direta a estado; a escrita continua delegada ao retratador canônico. Os testes congelam o relógio. (`app/ponto/management/commands/supra_juiz.py:48–53,245–285`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:33`)

`vivos` permanece baseado no conjunto completo `VIVOS`, inclusive `registrado`; `_com_fio` continua separado. Não houve estreitamento desse conjunto no novo caminho. (`app/chamados/catalogo/motor.py:24`; `app/ponto/management/commands/supra_juiz.py:211–214,259`; `app/docs/CORTES.md:255`)

A previsão estática para restaurar a costura anterior é:

| Caso novo | Falharia anteriormente? | O que efetivamente mede |
|---|---|---|
| RED: lavra `concorde`, command deriva CSF | **Sim** | Impede a classificação diagnóstica de autorizar retratação contra a lavra. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:106–114`) |
| Golden: lavra indevida, dia completo | **Não** | É controle positivo; a classificação anterior também produziria CSF. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:116–122`; `app/ponto/supra_juiz.py:443–447`) |
| MORDE: lavra indevida, command sem CSF | **Sim** | Este é o caso positivo que satisfaz o RED literal e distingue a nova autorização. A lavra é forçada pela porta na fixture. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:83–87,124–132`) |
| Sem lavra | **Sim, pela chave ausente** | Não prova que o código anterior retrataria: sem julgamento, a fixture fica sem ata e o leitor não reconstrói suas batidas. A asserção de status já poderia passar; a chave nova é a mordida. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:64–79,134–140`; `app/escala/services/leitor_celula.py:146–148,332–365`) |

Portanto, o teste chamado RED usa `concorde`, mas o caso MORDE cobre **vivo + `cobranca_indevida` lavrada**. Não falta totalmente o caso positivo; ele está sob outro nome. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:4,106–132`)

Faltam casos para ausência de lavra **sem vivos**, parada antes de outras escritas, `CSF_LAVRA_SEM_CODIGO`, recusa do retratador e verificação da nota produzida. Os quatro casos existentes não afirmam esses comportamentos. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:106–140`; `app/ponto/management/commands/supra_juiz.py:246–252,283–285`)

4. **Quando a lavra pode estar desatualizada**

| Etapa declarada | Horário/ordem |
|---|---|
| `processar_cartorio --apply` | Empresas 2/3/4: **06:27/29/31**. |
| Corrente após o cartório | `apurar_furos_diarios → disparar_perguntas_competencia → reconciliar_perguntas_orfas`, por encadeamento. |
| `supra_juiz --executar` | Empresas 2/3/4: **06:54/56/58**. |
| A6 encerrada / corrente | **07:25/27/29** e **07:33/35/37**. |

A expansão e a ordem acima vêm de `app/config/crons.py:17,29–33,580–597,1444–1469`.

O executor permanece agendado por horário. Seu `depende` não vira espera no wrapper; diferentemente da corrente encadeada, ele pode iniciar sem comprovar o término do cartório. O que efetivamente ocorreu em produção é **[nao verificado]**. (`app/config/crons.py:580,1794–1802`; `bin/cron_run.sh:11`; `app/config/crons.py:1444–1452`)

As janelas de desatualização identificáveis são:

- Alteração posterior à passada do cartório sem relavra bem-sucedida: os signals normalmente julgam, mas há saídas por `raw`, anti-recursão e tratamento de exceções; bulk/update não disparam esses signals. (`app/ponto/signals.py:78–93,131–140`; `app/chamados/signals.py:164–180`)
- Escrita sob `calado()`, situação que a fixture cria deliberadamente. (`app/ponto/services/cartorio.py:70–79`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:64–79`)
- Dia corrente: o batch considera dias anteriores a hoje, e o julgamento por evento pode retornar antes do fim do turno. (`app/ponto/management/commands/processar_cartorio.py:49–62`; `app/ponto/services/cartorio.py:1714–1719`)
- Concorrência durante o próprio executor: grade, lavra e chamados são lidos separadamente; o ramo novo não revalida impressão/versão antes da tentativa de escrita. (`app/ponto/management/commands/supra_juiz.py:128,164–165,189–211,245–285`)

**Nos dois sentidos:** lavra indevida antiga pode autorizar tentativa de retratar após surgir lastro novo; lavra antiga diferente de indevida pode impedir uma tentativa que a relavra atual autorizaria. O fechamento efetivo continua condicionado às guardas da porta — não é correto afirmar retratação indevida consumada apenas pelo primeiro cenário. (`app/ponto/management/commands/supra_juiz.py:248–285`; `app/chamados/reconciliador.py:520–526`; `app/chamados/models.py:646–653`)

5. **`emitir_furo_retroativo`: não é o mesmo efeito**

Ele passa `chamados_vivos=len(vivos)`, mas só age quando recebe **`FURO_SEM_COBRANCA`**. Esse ramo usa `cobrancas_vivas` e `chamados_total`, não `chamados_vivos`. (`app/ponto/management/commands/emitir_furo_retroativo.py:99–110`; `app/ponto/supra_juiz.py:305–362`)

O parâmetro influencia `COBRANCA_SEM_FURO`, `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_EM_DIA_COBERTO`, códigos ignorados pelo retroativo. Portanto, passar esse parâmetro ali não reproduz a autorização anterior de retratar. (`app/ponto/supra_juiz.py:443–475,501–504`; `app/ponto/management/commands/emitir_furo_retroativo.py:109–142`)

6. **Contagens, selos e literais**

- **`CSF_RETRATADO` pode ser falso.** O command incrementa sem conferir o retorno de `_retr`, que pode devolver `False` por retenção ou recusa de fechamento. É defeito herdado do bloco deslocado; o novo conjunto de candidatos continua exposto a ele. (`app/ponto/management/commands/supra_juiz.py:283–285`; `app/chamados/reconciliador.py:520–526`)
- **A6 pode dizer OK apesar de lavra ausente ou incompleta.** Sua enumeração fechada contém apenas os dois códigos antigos. (`app/ponto/management/commands/supra_juiz.py:299–305`)
- **T61 conserva os literais exigidos**, mas seu teste da guarda só compara posições textuais, não aninhamento. Ele não prova a nova autorização pela lavra. (`app/ponto/tests/test_t61_retratacao_em_seco.py:29–82`; `app/ponto/management/commands/supra_juiz.py:44,278–290`)
- **F1C4 continua aplicável ao classificador diagnóstico.** Seus testes com nomes “retrata” verificam códigos, não o estado após `supra_juiz --executar`; os três produtores declarados continuam presentes. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:30–74,163–167`; `app/ponto/management/commands/supra_juiz.py:222`)
- **O tripwire de pendência não perde sua ocorrência:** `Command.handle` continua contando/passando vivos ao classificador. A declaração e o teto não devem ser baixados por esta mudança. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:135–148,371–384`; `app/ponto/management/commands/supra_juiz.py:223–224`)
- **O selo de cobrança órfã permanece textual**, verificando presença/ordem da guarda e seu literal de placar. (`app/escala/tests/test_cobranca_orfa_regeneracao.py:181–194`)
- **O docstring ficou desatualizado:** “mudar a classificação muda o que é retratado” deixa de descrever a autorização direta, agora baseada na lavra. (`app/ponto/management/commands/supra_juiz.py:20,238–248`)

Suite verde e ausência de outros consumidores externos quebrados: **[nao verificado]**. [nao verificado]

7. **Contraexemplos e limites da conclusão**

- **Dia A sem lavra e sem chamados vivos; dia B com lavra indevida e chamado elegível.** A não é contado em `CSF_SEM_LAVRA`, e B pode ser processado normalmente. Isso contraria a exigência de parar por ausência em qualquer dia do universo. (`app/ponto/management/commands/supra_juiz.py:245–247,278–285`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:4–5`)
- **Dia A sem lavra e com vivo; dia B elegível.** A é contado, mas não impede B nem provoca parada ao final. Contar não satisfaz, sozinho, o “PAREI”. (`app/ponto/management/commands/supra_juiz.py:246–247,278–305`)
- **Lavra indevida sem código reconhecido.** O command preserva o chamado e conta a inconsistência, mas pode terminar normalmente. A porta aceita veredito explícito e códigos vazios, portanto essa combinação é representável pelo código. (`app/ponto/portas/celula.py:658–661,745–746`; `app/ponto/management/commands/supra_juiz.py:248–252,299–305`)
- **Lavra indevida com fechamento recusado pela porta.** O status continua vivo, enquanto o placar diz `CSF_RETRATADO`. Aqui o erro demonstrável é o contador; a recusa da porta pode estar correta. (`app/chamados/models.py:615–619,646–653`; `app/ponto/management/commands/supra_juiz.py:283–285`)

Já **dia completo/folga com chamado vivo e lavra atual sem `cobranca_indevida` não é, por si, contraexemplo à mudança**: os cortes mandam excluir o próprio chamado da formação dessa lâmpada, preservando `vivos` para outras decisões. A cura autorizada é consumir a lavra; recalcular para recuperar a retratação anterior contrariaria esse desenho. (`app/docs/CORTES.md:255,405,409,412,415`; `app/ponto/services/cartorio.py:1206–1214`; `app/docs/LEIS.md:95`)

Ocorrência e quantidade desses estados na produção: **[nao verificado]**.

_1293 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
