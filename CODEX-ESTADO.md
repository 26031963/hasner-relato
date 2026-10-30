# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 23:05** · publicado em **29/10/2026 23:05** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 23:05 · sessao 9ff2e22e4a8d · - · - · origem=Ronald (tmux)

Achados comprováveis por leitura; testes não executados para preservar a árvore. [nao verificado]

1. **Recusa não impede nascimento — gate sem mordida.** Em `app/chamados/models.py:1766-1773`, `religar_pai_vivo()` recusa chamado terminal devolvido pelo ramo AGRUPADO-EM, mas `PerguntaDisputa.abrir()` ignora o resultado e cria a pergunta em `:2176-2179`. Cenário: disputa aberta sob pai terminal; existe chamado da mesma chave agrupado sob outro pai vivo. `ChamadoColaborador.abrir()` devolve esse filho terminal em `:689-696`; a disputa permanece no pai terminal e a pergunta sem destino nasce viva ali. O agrupamento aceita pai vivo do mesmo colaborador (`:575-596`). Classe: **pouso antes do pré-requisito / gate sem mordida**.

2. **Trava termina antes do nascimento — corrida com fechamento.** O atomic de religamento termina em `app/chamados/models.py:1782`; o INSERT ocorre depois, em `:2178`. Um caminho real gera perguntas após sair do atomic de `_abrir_ou_agregar_disputa` (`app/chamados/services/disputa_emissao.py:598-606,618`). Cenário: disputa aberta com perguntas antigas validadas e pai terminal; T1 religa a pai novo vivo e libera trava; T2 fecha esse pai antes do INSERT; T1 cria pergunta viva sob pai já fechado. T2 passa S127 porque perguntas antigas existem e nenhuma está viva (`app/chamados/juizes.py:1006-1007,1817-1823`; fechamento em `app/chamados/models.py:142-155,183,262-264`). O resolver não altera antigas já materializadas/validadas (`app/chamados/services/disputa_emissao.py:1634-1637`). Classe: **ordem de trava/transação / pré-requisito não preservado**. O teste de idempotência só faz duas chamadas sequenciais (`app/chamados/tests/test_o261_pai_terminal.py:116-120`).

3. **Reabertura parcialmente gravada e retry inútil.** `reabrir_sistema()` salva abertura e nota antes do atomic do religamento (`app/chamados/models.py:1811-1819`). Cenário: pai terminal legado com `urgencia_original='critica'`; a nova porta copia esse valor (`:1761`), e `ChamadoColaborador.abrir()` rejeita antes do dedup (`:659-661`; validação em `app/chamados/juizes.py:1880-1889`). A disputa fica aberta com pai terminal; retry retorna imediatamente porque já está aberta (`app/chamados/models.py:1805-1806`). O estoque histórico desses valores é documentado em `app/chamados/juizes.py:1850-1855`; sua presença atual no banco não foi verificada. Classe: **transação incompleta / idempotência de retry**.

Matriz dos testes, por inspeção: [nao verificado]

| Teste | Sem mudança correspondente | [nao verificado]
|---|---|
| Nascimento sem destino | Falha na âncora viva (`test_o261_pai_terminal.py:74-82`). |
| Pai vivo permanece | **Passa sem cura**; controle negativo (`:85-92`). |
| Destino vivo não religa | **Passa sem cura**; controle negativo (`:95-108`). |
| Idempotência | Falha pela trilha ausente; não testa concorrência (`:111-126`). |
| Reabrir sistema | Falha sem novo religamento (`:129-139`). |
| Vigia conta estado | Falha com função ausente/contador zero (`:142-153`). |
| Carimbo alterado | Falha sem chave nova, mas constante zero no comando passa: fixture não planta pai terminal (`app/chamados/tests/test_o261_vigia_fio.py:689-710`). |

A serialização de dois religamentos sobre a mesma disputa está presente: leitura sob trava da disputa e do colaborador, seguida do gate que trava colaborador (`app/chamados/models.py:1743-1749,666-670`). Isso não protege o intervalo até a criação da pergunta nem coordena o fechador (`:2177-2179,122-183`).

## 29/10 23:05 · sessao f7e20b14288d · - · - · origem=Ronald (tmux)

Fila admin — evidências consolidadas:

| Caminho | Seleção/efeito | Exportada? Trancada/fechada? |
|---|---|---|
| `qs_vivos` → `qs_na_fila` | Universo `status_local ∈ VIVOS`; fila `status_local ∈ COBRANCA` menos `silenciado_ate > now` (`app/chamados/services/painel.py:153-155`, `186-188`). | Nenhum filtro por `data_evento`, exportação ou tranca nesses métodos (`app/chamados/services/painel.py:153-155`, `186-188`). |
| Lista principal / resumo | View chama `montar_contexto_painel`; lista padrão corta por `COBRANCA`, depois exclui verbos arquivado/histórico; resumo agrupa essa mesma lista (`app/chamados/views.py:726-738`; `app/chamados/services/painel.py:314-329`, `337-338`, `462-475`). | Nenhum filtro de exportação/tranca nesses cortes (`app/chamados/services/painel.py:314-329`). |
| Totais `abertos`, `em_analise`, `ativos`, `emergencias` | Contam `qs_na_fila`; `ativos=abertos+em_analise` (`app/chamados/services/painel.py:210-223`). | Sem corte adicional por competência (`app/chamados/services/painel.py:210-223`). |
| Crédito da pergunta ao pai | Busca `Q(chamado_destino__in=vivo) | Q(disputa__chamado__in=vivo)`; sem destino, credita `disputa.chamado`; filtra `q_vivas()` e exclui materializada com `VL_SIM` (`app/chamados/services/raiox.py:54-55`, `72`, `133-149`). | Exportação: não. Tranca dinâmica: **não chama juiz exato**. `q_vivas` exclui legado carimbado com `VIA_COMPETENCIA_TRANCADA`, mas declara não expressar a tranca por leitura (`app/chamados/models.py:2325-2332`, `2342-2353`). |
| Pilhas / verbos / pilulas | `particionar_verbos` usa esses créditos; respondida → validar, não respondida → cobrar, salvo precedência auditoria/módulo; aplica arquivado/histórico conforme silêncio (`app/chamados/services/verbos.py:31-59`; `app/chamados/catalogo/natureza.py:41-63`, `72-75`). | Sem filtro adicional de exportação ou tranca; herda limitações de `creditos_perguntas` (`app/chamados/services/verbos.py:31-59`). |
| Placar JSON / navbar | JSON lê lavrado sem filtros ou recalcula contexto; cron lavra contexto; badge admin soma validar+decidir; fallback navbar recalcula mesma partição (`app/chamados/views.py:840-861`; `app/core/management/commands/lavrar_badge_navbar.py:39-42`, `68-83`; `app/core/context_processors.py:88-93`; `app/chamados/services/verbos.py:19-24`). | Sem corte adicional; herda métodos acima (`app/core/management/commands/lavrar_badge_navbar.py:39-42`, `72-83`). |
| Fila copiloto / idade / gestão | `por_verbo` usa `particionar_verbos(qs_vivos())`; estoque/gestão usam `qs_na_fila` (`app/chamados/services/fila.py:53-55`, `88-100`, `117-141`, `145-170`). | Sem corte adicional por exportação/tranca (`app/chamados/services/fila.py:53-55`, `88-100`, `145-170`). |
| Inteligência `na_fila` | Usa vivos e conta chamados cujo verbo pertence a `VERBOS` (`app/chamados/services/na_fila.py:10-26`). | Sem corte adicional; herda crédito/partição (`app/chamados/services/na_fila.py:15-24`). |
| Fila por causa / toques | Universo `qs_na_fila`; classifica cada chamado; resposta sem veredito ganha gaveta própria (`app/chamados/services/fila_por_causa.py:158-161`, `195-200`, `210-216`). | Não exclui competência exportada ou trancada no universo; `respostas_sem_veredito` é outra leitura cuja implementação cabe à investigação de julgamento (`app/chamados/services/fila_por_causa.py:158-161`, `210-216`). |
| Modal/fio admin | Admin carrega todos chamados; busca perguntas de disputas abertas e vivas de fechadas; agrupa por `chamado_que_lista` (`app/chamados/services/fio.py:173-184`, `218-240`). | Exportação: não. Em disputa **aberta**, anexa todas sem juiz; em disputa **fechada**, exige `pergunta_viva` (`app/chamados/services/fio.py:229-239`). Modal desenha cada pergunta anexada (`app/templates/chamados/partials/modal_fio.html:154-155`). |
| Destaque/ordenação do fio | `tem_pergunta_viva` usa property `.viva` + `pergunta_sem_acao_possivel`; cards com ela sobem (`app/chamados/services/fio.py:279-281`, `409-415`). | `.viva` delega ao juiz, logo tranca dinâmica conta; exportação não é condição desse trecho (`app/chamados/models.py:2355-2360`; `app/chamados/juizes.py:1005-1016`). |
| Detalhe local do pai | Busca disputa aberta por `chamado`, carrega perguntas dela e filtra `_viva(_p)` (`app/chamados/services/acoes_chamado.py:335-348`). | Exportação: não; tranca dinâmica: sim, pelo C6 (`app/chamados/juizes.py:1005-1016`). |
| Contador modal | Ativos via `qs_na_fila`, disputas via `fechada_em IS NULL` (`app/chamados/views.py:900-912`). | Não corta pela competência das perguntas (`app/chamados/views.py:900-912`). |

Distinção importante: **trancada não é equivalente a somente exportada na árvore atual**. A própria regra diz “NUNCA exportada: competência só exportada segue viva”; a antiga `dia_em_competencia_exportada` foi removida e há lápide explícita (`app/chamados/services/competencia_trancada.py:3-11`; `app/chamados/juizes.py:1504-1526`). O juiz exato mata pela tranca **do `data_evento`**, salvo toque humano posterior à tranca (`app/chamados/juizes.py:887-907`, `1005-1016`).

Mecanismos vizinhos que poderiam confundir a conclusão:

- `arquivar_competencia_encerrada` silencia **chamados**, usando dia do chamado/criação; pula chamado com disputa contendo qualquer pergunta viva. Não separa perguntas por exportação (`app/chamados/management/commands/arquivar_competencia_encerrada.py:100-128`, `148-162`).
- Serviço de tranca seleciona perguntas por `data_evento` na janela; hoje não as carimba, pois C6 lê a tranca. Preserva chamado com pergunta viva fora da competência e disputa cujo pai vivo fica fora (`app/chamados/services/competencia_trancada.py:106-149`, `171-198`). Isso atende **tranca**, não “apenas exportada” (`app/chamados/services/competencia_trancada.py:3-11`).
- `religar_pai_vivo` não lê nenhuma pergunta/competência/exportação; cria/obtém pai vivo com prazo e escreve apenas `d.chamado` (`app/chamados/models.py:1741-1782`). Wrapper exige existir **alguma** viva sem destino, sem particionar demais perguntas (`app/chamados/models.py:1788-1795`). Todas sem destino passam a usar pai novo pelo `or` da ancoragem (`app/chamados/services/fio.py:116-117`).

Não encontrei AGENTS na busca `rg --files -g AGENTS.md`; existência fora desse universo [nao verificado].

## 29/10 23:05 · sessao dfa06780a531 · - · - · origem=Ronald (tmux)

Achado principal: `religar_pai_vivo` substitui “chamado vivo do dia” por “container vivo da pessoa”, contrariando o pedido literal de `app/docs/CORTES.md:381`. A busca nova não recebe dia/célula nem consulta a cobrança existente; filtra apenas colaborador, categoria, módulo e `VIVOS`. `app/chamados/models.py:1749`

Cenário concreto: existe pergunta viva sem destino de 15/10, disputa fechada com pai resolvido e cobrança `batida_ausente` aberta daquele colaborador/dia. `desfazer_carimbo_sem_lastro` reabre essa disputa; a nova cadeia cria outro `disputa_supervisao` e mantém a pergunta sem destino, embora a cobrança correta já exista. Caminho: `app/chamados/services/relavra_via.py:195`, `app/chamados/services/relavra_via.py:222`, `app/chamados/models.py:1819`, `app/chamados/models.py:1795`, `app/chamados/models.py:1749`. Classe: derivação/chave paralela e cura fora da origem do vínculo.

A autoridade do dia seleciona cobrança por dia forte e desempate “mais nova”; a porta canônica de costura valida, aponta destino e lavra no mesmo ato. `app/chamados/juizes.py:1610`, `app/chamados/juizes.py:1621`, `app/chamados/services/fio.py:740`, `app/chamados/services/fio.py:792`

O pai novo também nasce sem célula e sem `data_turno`; portanto o juiz forte do dia não o identifica como cobrança de 15/10. `app/chamados/models.py:1762`, `app/chamados/models.py:905`, `app/chamados/juizes.py:118`

Há uma incompletude estrutural adicional: o novo `any(pergunta_viva(q) for q in qs)` copia literalmente a redução de `fio_vivo`, autoridade registrada C15. Hoje os booleanos coincidem; não há diferença comportamental demonstrada, mas resta segundo escritor da redução. `app/chamados/models.py:1793`, `app/chamados/juizes.py:1102`, `app/core/juizes.py:241`

Não identifiquei cópia nova de C7 nem registro novo que dispare JUIZ-NOVO; `pai_terminal` consulta o `VIVOS` oficial. `app/chamados/models.py:1434`, `app/chamados/models.py:1715`, `app/core/juizes.py:233`, `CLAUDE.md:86`

Não elevaria o `_tz.now()` do SLA a achado: encontrei a classe proibida citada, mas nenhuma regra concreta de borda aplicável que sustentasse a reprovação. `app/chamados/models.py:1763`, `CLAUDE.md:592`

## 29/10 23:04 · sessao 842bc2ead565 · - · - · origem=Ronald (tmux)

Censo de caminhos, por leitura:

- Nascimento: seis chamadas a `PerguntaDisputa.abrir`: `app/chamados/services/disputa_emissao.py:389`, `:770`, `:1160`; `app/chamados/services/acoes_disputa.py:265`; `app/chamados/services/declaracao_texto.py:322`, `:611`. A criação efetiva fica em `app/chamados/models.py:2178`.
- Nascimento da disputa: núcleo comum em `app/chamados/services/disputa_emissao.py:523`, agrupador manual em `:927` e âncora manual em `:1147`; usam `ChamadoColaborador.abrir` antes de criar, respectivamente em `:504`, `:908`, `:1128`.
- Reabertura da disputa: duas portas escrevem `fechada_em=None`, `reabrir_admin` em `app/chamados/models.py:1701` e `reabrir_sistema` em `:1811`. A ação administrativa cria pergunta depois, alcançando o gate novo (`app/chamados/services/acoes_disputa.py:256`, `:265`).
- Chamadores de `reabrir_sistema`: ressuscitação em `app/chamados/models.py:1189`; desfazer carimbo em `app/chamados/services/relavra_via.py:222`; devolver validação em `app/chamados/services/validacao.py:200`; rejeição de ausência em `app/chamados/services/materializacao.py:631`; comando de órfãs em `app/chamados/management/commands/reabrir_orfas_enterradas.py:154`.
- Reabertura de pergunta também ocorre em `app/chamados/models.py:2269`, `:2284`, `:2303`, e na porta `app/chamados/services/disputa_emissao.py:1327`; esta é chamada pelo colaborador em `app/chamados/models.py:1871`, pela recusa administrativa em `app/chamados/services/validacao.py:540`, por ausência rejeitada em `app/chamados/services/materializacao.py:628` e pelo comando de limbo em `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.

Achados para consolidar:

1. **Gate novo não cobre a pergunta existente que ganha vida ao ser reapontada.** `PerguntaDisputa.abrir` troca `celula`, relavra, salva e retorna a existente antes da chamada nova a `religar_pai_vivo` (`app/chamados/models.py:2154`, `:2168`, `:2171`, `:2177`). Cenário: fato existente sem destino, disputa aberta e pai resolvido; sua célula histórica absolve, mas a célula correta encontrada para o colaborador/dia acusa. O reapontamento torna a pergunta viva e a devolve ainda sob pai terminal. Classe: **cura incompleta na origem / caminho que escapa do gate**. A vida é decidida com os insumos da pergunta, inclusive célula, conforme a documentação do prefiltro em `app/chamados/models.py:2317`.

2. **Reabrir pergunta em disputa já aberta continua sem garantir pai vivo.** A rejeição da ausência limpa os carimbos das perguntas e chama `reabrir_sistema` (`app/chamados/services/materializacao.py:628`, `:631`). Porém essa porta retorna imediatamente quando a disputa já está aberta, antes do religamento recém-adicionado (`app/chamados/models.py:1805`, `:1819`). Cenário: disputa aberta sob pai resolvido, pergunta sem destino absorvida por ausência; ausência rejeitada e nenhuma outra ausência cobrindo o dia → pergunta volta a pendente, pai permanece resolvido. Classe: **cura incompleta / pré-requisito da reabertura não assegurado**. A condição de rejeição e ausência sem cobertura está em `app/chamados/services/materializacao.py:608`, `:615`.

Observações de autoridade:

- O diff não acrescenta outra implementação de `pode_fechar`; a autoridade registrada continua `DisputaSupervisao.pode_fechar` em `app/core/juizes.py:233`.
- O novo `any(pergunta_viva(q)...)` em `app/chamados/models.py:1793` repete a redução já existente em `fio_vivo`, `app/chamados/juizes.py:1102`–`:1111`. Há duplicação estrutural da redução C15, mas divergência funcional atual entre as duas expressões **[não verificado]**.
- A nova escrita do vínculo fica em `app/chamados/models.py:1774`; os outros três locais identificados são os nascimentos citados acima. Segundo escritor posterior ao nascimento **[não verificado]**.

Não executei testes ou operações de escrita. **[não verificado]** [nao verificado]

## 29/10 23:03 · sessao 96578cc4f702 · o261c2 · sonda · origem=Code

- MISTAS 18 disputas | perguntas EXPORTADAS nelas 201 | ABERTAS nelas 212 | colabs 18. [nao verificado]
- EXPORTADAS nas mistas por competencia: `{'2:09/2026': 192, '3:09/2026': 9}`. [nao verificado]
- SO-EXPORTADA 22 disputas | perguntas 269. [nao verificado]
- SO-ABERTA 33 disputas | perguntas 125. [nao verificado]
- universo repartido 807 (tem de ser == vigia 807) | disputas 73 | colabs 66. Universo == VIGIA: 807 == 807. [nao verificado]
- ERROS=0. [nao verificado]

As duas somas fecham: ABERTA + EXPORTADA + sem data = 337 + 470 + 0 = 807; mistas + so-exportada + so-aberta = 18 + 22 + 33 = 73. [nao verificado]

## 29/10 22:09 · sessao 61d5e68cd5c8 · - · - · origem=Ronald (tmux)

Achado confirmado: novo `any(pergunta_viva(q) for q in qs)` duplica C15, registrado como `fio_vivo` em `app/core/juizes.py:123` e `:241`; autoridade em `app/chamados/juizes.py:1102`. O contrato existente detecta exatamente essa forma: `app/chamados/tests/test_o261_fio_tem_uma_porta.py:99`, `:127`, `:142`. Executei o detector AST real contra o trecho novo em memória: acusou `religar_se_fio_sob_pai_morto`.

Outros pontos encaminhados:

- Admin conserva `ch` antigo: nota e push usam esse objeto mesmo se `PerguntaDisputa.abrir` mudar o pai; base `app/chamados/services/acoes_disputa.py:270`, `:275`.
- Novo pai manual perde `contexto_json.chamados_orfaos_ids`, necessário para dissolução imediata por entrada sincronizada: nascimento em `app/chamados/services/disputa_emissao.py:920`; leitor em `app/ponto/services/entrada_sincronizada.py:47`.
- Reparentar muda a âncora de todas as perguntas sem destino, inclusive históricas, enquanto followups ficam no pai anterior: `app/chamados/services/fio.py:117`; novas respostas escrevem no pai atual em `app/chamados/services/disputa_emissao.py:1309`.
- Não encontrei segundo escritor de `DisputaSupervisao.chamado` pós-nascimento na base; escritores de nascimento em `app/chamados/services/disputa_emissao.py:527`, `:931`, `:1151`.
- Não classificaria a suíte inteira como sem mordida: os testes positivos de pai terminal verificam mudança ausente na base; os controles de pai vivo/destino vivo passam também sem cura. O teste de idempotência usa motivos diferentes e não demonstra duas entradas iguais, conforme anexo `app/chamados/tests/test_o261_pai_terminal.py:111`.

Censo ampliado de consumidores foi enviado nas mensagens anteriores, com linhas.

## 29/10 22:09 · sessao 6d3f808735cb · - · - · origem=Ronald (tmux)

Achados de escopo e selo:

- **P2 — Escopo de testes diverge do corte.** O corte permite “testes de tela”; o wrapper exige testes **novos**, por nome/caminho. Assim, alteração legítima de teste existente é contada fora, enquanto teste novo sem relação com tela passa. O selo exercita somente teste novo. Referências: `app/docs/CORTES.md:394`; `bin/codex_pergunta.sh:116`; `bin/codex_pergunta.sh:117`; `bin/tests/test_codex_pergunta.sh:100`; `bin/tests/test_codex_pergunta.sh:164`.

- **P2 — Selo não garante o destino efetivo da chamada.** `_estrutura` aceita presença de `-C "$WS"` e proíbe somente algumas grafias de destino vivo. Mantendo esse argumento e acrescentando `-C "$VIVO/app"`, sua verificação continua sem diagnósticos; o teste comportamental também verifica somente presença de argumentos isolados. Confirmei a lacuna estrutural executando exclusivamente a função extraída em memória com essa mutação, sem arquivos. Referências: `bin/tests/test_codex_pergunta.sh:35`; `bin/tests/test_codex_pergunta.sh:39`; `bin/tests/test_codex_pergunta.sh:40`; `bin/tests/test_codex_pergunta.sh:133`; `bin/codex_pergunta.sh:251`. Execução comportamental do mutante: **[nao verificado]**.

- **P2 — Ressalva da 7b ausente.** As proibições gerais de escrita continuam sem exceção para o piloto, embora o wrapper já habilite `workspace-write`; falta a ressalva expressamente exigida pelo contrato v2(a) fornecido. Referências: `CLAUDE.md:578`; `CLAUDE.md:588`; `CLAUDE.md:599`; `bin/codex_pergunta.sh:246`.

- **P2 — Procedimento publicado ainda contradiz v2.** O RELATO continua mandando remover o papel “no mesmo marco”, enquanto v2(j) exige pouso próprio posterior; o descarte publicado usa `checkout .`, sem limpar novos arquivos, divergindo do reset à base + `clean -fdx` do v2(h). Referências: `app/docs/RELATO.md:10`; `app/docs/RELATO.md:12`; `bin/codex_pergunta.sh:39`.

Não trataria `rc=0` com `fora_do_escopo>0` como falha isolada do wrapper: descarte e selos semânticos são responsabilidade expressa do Code. Referências: `app/docs/RELATO.md:10`; `app/docs/RELATO.md:11`; `app/docs/CORTES.md:394`.

## 29/10 22:08 · sessao 06d376475023 · - · - · origem=Ronald (tmux)

Achados e lacunas dos consumidores/selos:

- **Alta — orientação de LIMITE incorreta para construtor.** `bin/codex_pergunta.sh:263` manda seguir por sonnet sem esperar, inclusive no novo papel. `bin/codex_limite_estado.py:157` repete essa instrução global. Contraria a espera do piloto e a proibição de reconstrução pelo Code, também registradas em `app/docs/RELATO.md:10`. O selo só simula LIMITE para leitura (`bin/tests/test_codex_limite.sh:24`); o falso construtor só tem modos ok/escreve/dorme (`bin/tests/test_codex_pergunta.sh:96`).

- **Média — classificação de construtor sem mordida no selo de estado.** As fixtures só incluem papéis antigos e inventado (`bin/tests/test_codex_papel_estado.sh:14`); a asserção aceita o prefixo terminando em sonda (`bin/tests/test_codex_papel_estado.sh:31`). Remover construtor de `PAPEIS` faria o papel cair em outro (`bin/codex_limite_estado.py:78`) sem contrariar essas asserções.

- **Média — ESPERANDO não tem integração testada.** O selo aceita a linha em qualquer posição e qualquer PID numérico (`bin/tests/test_codex_pergunta.sh:160`), sem exigir primeira linha útil, PID do wrapper ou aceitação pelo hook. O consumidor real exige identidade do processo/script (`bin/hook_stop_fila1.py:474`, `bin/hook_stop_fila1.py:521`). Não identifiquei incompatibilidade nominal entre a emissão atual (`bin/codex_pergunta.sh:90`) e esse consumidor; execução integrada [nao verificado].

- **Média — cobertura parcial da preservação e do timeout.** O teste comportamental antigo exercita leitura (`bin/tests/test_codex_pergunta.sh:129`), sem censo/verificador. O teste sonda continua separado (`bin/tests/test_codex_sonda.sh:58`). Timeout cobre gate e exec nos cenários (`bin/tests/test_codex_pergunta.sh:179`, `bin/tests/test_codex_pergunta.sh:187`), sem cenário de todos os slots ocupados ou filho resistente a TERM.

- **Média — ressalva de 7b ainda pendente para o pouso.** Permanecem proibições gerais de escrita e sandbox diferente de read-only em `CLAUDE.md:574`, `CLAUDE.md:578`, `CLAUDE.md:588` e `CLAUDE.md:599`. O contrato v2(a) anexado exige ressalva no pouso; ocorrência desse pouso [nao verificado]. [nao verificado]

Compatibilidade confirmada por leitura: os 12 campos não quebram o parser, que aceita pelo menos nove e conserva os índices antigos (`bin/codex_limite_estado.py:66`). Construtor está no vocabulário (`bin/codex_limite_estado.py:23`); o gerador mantém o leitor único e propaga papel/origem sem enumeração paralela (`bin/gerar_codex_estado.py:41`, `bin/gerar_codex_estado.py:155`, `bin/gerar_codex_estado.py:176`).

Nenhum teste executado, pois criam arquivos temporários (`bin/tests/test_codex_pergunta.sh:68`, `bin/tests/test_codex_papel_estado.sh:7`); resultados de execução [nao verificado].

## 29/10 22:08 · sessao 61d74141d87b · - · - · origem=Ronald (tmux)

Achados sustentados pela base e pelo diff:

1. **Chamado extra na fila / pouso antes do pré-requisito.** O novo trecho de `PerguntaDisputa.abrir` religa qualquer nascimento sem destino antes de saber se a pergunta vive. Cenário: célula com veredito não acusatório, sem marcos pendentes; disputa aberta sob pai resolvido; nenhuma pergunta existente. O helper cria pai aberto, mas C6 declara a pergunta morta. A guarda do chamado só examina módulos de turno, e a fila inclui o novo pai aberto. Base: `app/chamados/juizes.py:1053`, `app/chamados/models.py:848`, `app/chamados/services/painel.py:186`; trecho novo imediatamente antes do `create` hoje em `app/chamados/models.py:2085`.

2. **Segundo juiz registrado.** `religar_se_fio_sob_pai_morto` introduz `any(pergunta_viva(q) for q in qs)`, duplicando a redução C15, cuja autoridade é `fio_vivo`. Registro: `app/core/juizes.py:123`; autoridade: `app/chamados/juizes.py:1102`; consumidor já convertido: `app/chamados/services/disputa_emissao.py:1188`. Ler `VIVOS` em `pai_terminal`, isoladamente, respeita C1: `app/core/juizes.py:100`.

3. **Derivação paralela da chave do pai; pode haver dois pais vivos.** Nascimento canônico fixa módulo `disputa_supervisao`, enquanto os manuais usam `disputa_supervisao_manual` e incluem subcategoria no lookup. O novo helper herda módulo do pai velho e tira subcategoria do lookup. Cenário: disputa aberta com pai manual terminal e outro pai canônico vivo do mesmo colaborador; o helper não encontra o canônico e cria outro manual vivo. Também pode reutilizar pai manual de outro subtipo. Fontes: `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:908`, `app/chamados/services/disputa_emissao.py:1128`; ausência de unicidade: `app/chamados/models.py:639`.

4. **Ordem de trava: caminho inverso alcança o helper.** `ChamadoColaborador.abrir` segura Colaborador, renasce chamado existente, ressuscita perguntas e chama `reabrir_sistema`, que passa a travar Disputa. Caminho: `app/chamados/models.py:669` → `:685` → `:770` → `:1098` → `:1188`. O concorrente novo segura Disputa e solicita Colaborador no mesmo gate. Cenário possível com disputa fechada contendo pergunta com destino ressuscitável e outra viva sem destino; um emissor conserva instância aberta obtida antes do fechamento e entra no novo nascimento. Além disso, `select_for_update().select_related('colaborador')` trava ambas as tabelas sem `of=('self',)`, portanto a consulta não expressa a sequência individual prometida pela docstring. Lei: `app/chamados/models.py:646`.

5. **Consumidor do ponteiro não atualizado no admin.** Se `ch.reabrir()` recusa, o gate pode mover `d_fec.chamado` para novo pai. O diff refresca `d_fec`, mas mantém `ch` antigo: justificativa administrativa continua anexada ao pai terminal e push anuncia seu número. Recusas: `app/chamados/models.py:1051`; nota: `app/chamados/services/acoes_disputa.py:269`; push: `:275`. A pergunta administrativa é conversa e nasce sempre: `app/chamados/models.py:2033`.

6. **Idempotência da recusa.** Duas chamadas diretas iguais ao novo `religar_pai_vivo`, com gate devolvendo filho terminal AGRUPADO-EM, produzem duas trilhas `disputa_religar_recusado`; o estado não é curado e o nascimento continua. O comportamento do gate que devolve esse terminal está em `app/chamados/models.py:689`. A duplicação das trilhas decorre do trecho novo do diff; frequência em produção: [nao verificado].

O teste anexado usa sempre célula acusatória e não cobre o achado 1, nem módulos manuais, concorrência ou recusa AGRUPADO. A efetiva execução dos testes/RED: [nao verificado]. [nao verificado]

## 29/10 22:08 · sessao 12b050fdf0bb · - · - · origem=Ronald (tmux)

Achados por inspeção estática; execução real destes cenários [nao verificado].

- **Alta — bypass da exclusão, timeout e protocolo `.rc`.** `CODEX__INTERNO=1` só exige que alguém tenha o lock; não comprova que o processo recebeu o lock do externo. Durante uma chamada ativa, uma segunda chamada interna passa em `flock -n 6`, pula validação de volta, timeout e escrita de `.rc`, e alcança o `codex exec`. Referências: `bin/codex_pergunta.sh:84`, `:85`, `:91`, `:94`, `:98`, `:100`, `:246`. O selo só testa chamada interna com lock livre: `bin/tests/test_codex_pergunta.sh:147`.

- **Alta — sinal prematuro de conclusão por ordem errada de trava.** Volta inválida ou destino inválido gravam `<obra>.rc` antes de tentar o lock. Uma segunda chamada errada com a mesma obra pode produzir `rc=2` enquanto a primeira ainda escreve. Referências: `bin/codex_pergunta.sh:75`, `:85`, `:86`, `:88`. O selo preserva `.rc` apenas no caso lock ocupado com argumentos válidos: `bin/tests/test_codex_pergunta.sh:170`, `:174`, `:176`.

- **Alta — confinamento não valida o diretório efetivamente passado ao Codex.** `_destino_ok` resolve o caminho físico da worktree, mas aceita `app/` por simples `-d`. Uma worktree literal com `app` sendo symlink para outro lugar passa na validação; a chamada usa esse `app` em `-C`. Referências: `bin/codex_pergunta.sh:78`, `:79`, `:81`, `:247`. O selo verifica symlink da raiz, não de `app/`: `bin/tests/test_codex_pergunta.sh:144`. Existência disso no piloto [nao verificado].

- **Alta — lock não é único por worktree.** A identidade da trava depende de `CODEX_LOGD` ou do diretório da cópia do wrapper, enquanto o destino é sempre a mesma worktree literal. Duas chamadas com `LOGD` distintos usam travas e slots distintos sobre o mesmo destino. Referências: `bin/codex_pergunta.sh:42`, `:44`, `:67`, `:74`, `:87`, `:224`, `:230`. Uso real de diretórios distintos [nao verificado].

- **Média — `.rc` não cobre erros de instrumento anteriores ao ramo construtor.** Pergunta ilegível, hash inválido e anexos/sonda ilegíveis terminam antes da inicialização/publicação do `.rc`, podendo deixar arquivo antigo no lugar. Referências: `bin/codex_pergunta.sh:57`, `:58`, `:65`, `:66`, `:74`, `:89`. Também não há tratamento de sinal no externo para garantir publicação em interrupção: `bin/codex_pergunta.sh:84`, `:91`, `:94`; comportamento de interrupção em produção [nao verificado].

- **Média — instrução de LIMITE contradiz o piloto.** O caminho comum de rc75 manda seguir por sonnet mesmo para construtor, embora o contrato fornecido determine esperar e não reconstruir. Referência: `bin/codex_pergunta.sh:263`; contrato anexo, item (g) [nao verificado como arquivo local].

- **Média — base declarada não é vinculada à worktree.** O hash é resolvido em `VIVO`, mas a validação da worktree não compara seu HEAD nem repositório de origem com esse hash. `BASE: $HASH` pode descrever outra árvore; `wt_head` só é registrado no fim. Referências: `bin/codex_pergunta.sh:58`, `:81`, `:134`, `:203`. Divergência real [nao verificado].

- **Média — falta a ressalva da regra 7b exigida pelo contrato v2(a).** A cópia mantém proibições categóricas de escrita pelo Codex. Referências: `CLAUDE.md:565`, `:574`, `:578`, `:588`, `:599`. O corte autoriza exceção circunscrita: `app/docs/CORTES.md:394`.

## 29/10 22:07 · sessao 3140036661a5 · - · - · origem=Ronald (tmux)

Censo dos chamadores de produção, pela cópia base:

| Porta | Chamadores |
|---|---|
| `PerguntaDisputa.abrir` | `app/chamados/services/disputa_emissao.py:389` (`gerar_perguntas_disputa`), `:770` (`_criar_pergunta_se_couber`), `:1160` (`criar_disputa_ancora_de_chamado`); `app/chamados/services/declaracao_texto.py:322` (`declarar_hora_colab`), `:611` (`aplicar_declaracao`); `app/chamados/services/acoes_disputa.py:259` (`acao_reabrir_disputa_admin`). |
| `DisputaSupervisao.reabrir_sistema` | `app/chamados/models.py:1189` (`ressuscitar_perguntas`); `app/chamados/services/relavra_via.py:222`; `app/chamados/services/validacao.py:200`; `app/chamados/services/materializacao.py:631`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:154`. |

Achados:

1. **O nascimento continua após religação recusada.** O gate de chamado pode devolver filho terminal agrupado sob pai vivo, mesmo com `status_local__in=VIVOS` (`app/chamados/models.py:689`, `:695`). O diff detecta isso em `religar_pai_vivo` e retorna `False`, mas o novo bloco de `PerguntaDisputa.abrir` ignora esse retorno e segue ao `create` (inserção antes da linha base `app/chamados/models.py:2085`). Cenário: disputa aberta sob pai terminal agrupado, emissão de novo fato vivo sem destino por `gerar_perguntas_disputa` (`app/chamados/services/disputa_emissao.py:389`). A pergunta nasce viva sob o mesmo terminal; o evento de recusa apenas registra. Duas tentativas idênticas de religação recusada também geram dois `disputa_religar_recusado`, contrariando “1 trilha” (bloco novo no diff).

2. **Chamado a mais antes de determinar o destino final.** `gerar_perguntas_disputa` abre a pergunta (`app/chamados/services/disputa_emissao.py:389`) e somente depois tenta `reatar_pergunta_orfa` (`:404`, `:409`). O reatamento pode criar/reusar chamado do dia (`app/chamados/reconciliador.py:779`) e costurar a pergunta nele (`:819`), ou reconhecer histórico e resolver (`:1433`, `:1439`). O novo gate terá criado pai vivo antes disso; depois da costura, ele sobra vivo sem aquela pergunta ancorada. A possibilidade é demonstrável pelo fluxo; ocorrência concreta em produção: [nao verificado].

3. **Inversão de ordem de trava.** `ChamadoColaborador.abrir` trava Colaborador (`app/chamados/models.py:669`), renasce chamado encerrado (`:685`, `:686`), chama `reabrir` (`:770`), que ressuscita perguntas (`:1098`), que reabre disputa (`:1189`). O novo `reabrir_sistema` pode então chamar a religação, que trava Disputa. Na direção contrária, a emissão trava Disputa primeiro (`app/chamados/services/disputa_emissao.py:467`) e entra no gate que trava Colaborador (`app/chamados/models.py:669`). Cenário: chamado do dia encerrado renasce e ressuscita pergunta destinada nele; sua disputa fechada também tem outra pergunta viva sem destino sob pai terminal. Concorrência com emissão nessa disputa cria C→D versus D→C. Deadlock executado: [nao verificado].

4. **Retorno antecipado não cobre perguntas existentes.** O dedup de identidade devolve `ex` antes do novo guard (`app/chamados/models.py:2061`, `:2084`), podendo inclusive trocar célula e relavrar (`:2071`, `:2081`). Logo uma entrada que reaproveita pergunta viva sem destino sob pai terminal mantém o defeito. Isso é reaproveitamento, não nascimento novo; não deve ser anunciado como falha no caminho de `create`.

## 29/10 22:07 · sessao 3c6958a11cf4 · o261c2 · sonda · origem=Code

```text
VIGIA viva_sob_pai_terminal (a funcao do pouso): {'perguntas': 807, 'disputas': 73} [nao verificado]
universo repartido 807 (tem de ser == vigia 807) | disputas 73 | colabs 66
status do pai: {'resolvido': 752, 'fechado': 55} | todos terminais: True
ancora (chamado_que_lista) == pai em todas: True
competencia ABERTA 337 | EXPORTADA 470 em 2 empresa x competencia | sem data_evento 0
ABERTA por competencia: {'2:10/2026': 205, '3:10/2026': 10, '4:04/2026': 2, '4:05/2026': 52, '2:11/2026': 66, '3:11/2026': 2}
EXPORTADA por competencia: {'2:09/2026': 446, '3:09/2026': 24}
DISPUTAS: so aberta 33 | so exportada 22 | mista 18 | com sem-data 0
por motivo: {'saida_sem_entrada': 236, 'intervalo_saida': 226, 'intervalo_volta': 211, 'orfao_14h': 109, 'batida_fora_escala_grave': 20, 'sem_posto': 3, 'foto_ausente': 2}
por via_resolucao: {'-': 191, 'completude_turno': 4, 'chamado_encerrado': 316, 'sem_turno_aberto': 287, 'admin_decidiu': 2, 'ausencia_dia': 6, 'celula_absolveu': 1}
por mes do dia: {'2026-04': 20, '2026-05': 34, '2026-08': 150, '2026-09': 382, '2026-10': 221}
p#51322: no universo=True viva=True destino=None disputa=4376 aberta=True pai=19611 status_pai=resolvido
p#34412: no universo=True viva=True destino=None disputa=4376 aberta=True pai=19611 status_pai=resolvido
FORA (nao e desta cura): vivas com DESTINO terminal 2155
FORA (de viva_em_disputa_fechada, dono admin): vivas sem destino em disputa FECHADA sob pai terminal 31
ERROS=0
FIM-DA-SONDA
```

Há a linha `ERROS=0`, que informa zero erros. [nao verificado]  
Universo repartido igual ao vigia: 807 = 807. [nao verificado]  
A soma fecha no universo: 337 + 470 + 0 = 807. [nao verificado]

## 29/10 21:57 · sessao e114400beabb · o261c2 · leitura · origem=Code

**Sim: uma pergunta pode nascer ligada a chamado pai terminal.** O caso mais explícito é a reabertura administrativa: cria a pergunta e só depois tenta reabrir o chamado (`app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/acoes_disputa.py:265`).

Antes do inventário, há dois vínculos diferentes: `PerguntaDisputa.disputa` → `DisputaSupervisao.chamado`, e `PerguntaDisputa.chamado_destino`. Não existe campo direto `PerguntaDisputa.chamado` nessas declarações (`app/chamados/models.py:1849`, `app/chamados/models.py:1324`, `app/chamados/models.py:2283`).

Os conjuntos oficiais são `VIVOS=('aberto','em_analise','registrado')`, `ENCERRADOS=('resolvido','fechado')` e `TERMINAIS=ENCERRADOS+('cancelado','superado')`. Portanto, excluir apenas `ENCERRADOS` deixa passar dois estados terminais (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`).

**1–2) Sítios de criação e possibilidade de pai terminal**

O inventário encontrado contém **seis chamadas de produção a `PerguntaDisputa.abrir`**, discriminadas abaixo, convergindo na inserção física `cls.objects.create(...)` (`app/chamados/models.py:2085`). A classe homônima conceitual `Pergunta`, do catálogo, é uma `dataclass`, não um modelo Django (`app/chamados/catalogo/perguntas.py:14`, `app/chamados/catalogo/perguntas.py:17`).

Três sítios compartilham a escolha de pai por `_abrir_ou_agregar_disputa`: geração comum e as duas declarações. A query é:

```python
DisputaSupervisao.objects.select_for_update().filter(
    colaborador=colaborador, fechada_em__isnull=True,
).first()
```

Ela filtra **disputa aberta**, sem filtrar status do chamado pai e sem recorte de data. O ramo existente devolve essa disputa preservando seu pai; a ordenação padrão da disputa é `-aberta_em` (`app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:502`, `app/chamados/models.py:1390`).

Sem disputa aberta, o mesmo núcleo obtém o pai por:

```python
ChamadoColaborador.abrir(
    colaborador=colaborador,
    categoria='disputa_supervisao',
    modulo_origem='disputa_supervisao',
    status_local__in=VIVOS,
    defaults=...,
)
```

Depois cria a disputa com `chamado=chamado`. `subcategoria` fica nos `defaults`, não participa desse lookup (`app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:514`, `app/chamados/services/disputa_emissao.py:523`).

| Sítio / função | Escolha dos vínculos e status terminal no nascimento |
|---|---|
| **`gerar_perguntas_disputa`**, chamada em `app/chamados/services/disputa_emissao.py:389`; definição em `:280` | Recebe a disputa. O fluxo comum vem de `criar_ou_atualizar_disputa`, usando o núcleo acima: **pai terminal é permitido no reuso de disputa aberta**, pois o status dele não é conferido (`app/chamados/services/disputa_emissao.py:598`, `:606`, `:618`, `:467`). O destino é derivado pela query `ChamadoColaborador.objects.filter(colaborador=disputa.colaborador, status_local__in=VIVOS).exclude(categoria='disputa_supervisao')`: **terminais são excluídos na seleção do destino** (`app/chamados/services/disputa_emissao.py:181`). Há filtros adicionais de motivo atendido, ausência de pergunta não materializada do mesmo motivo, dia compatível e candidato único após desempate por turno (`:194`, `:209`, `:214`, `:219`, `:221`). Pode nascer sem destino; o reatamento ocorre depois da criação (`:404`, `:409`). |
| **`_criar_pergunta_se_couber`**, chamada em `app/chamados/services/disputa_emissao.py:770`; definição em `:756` | Recebe disputa e destino. O agrupador tem **dois caminhos de seleção da disputa**. No repasse, busca `PerguntaDisputa.objects.filter(chamado_destino=ch, materializada_em__isnull=True).select_related('disputa').first()` e usa `_pv.disputa`, **sem exigir disputa aberta nem pai vivo**; chama a complementação, que chega a este sítio (`app/chamados/services/disputa_emissao.py:874`, `:877`, `:885`, `:813`). No caminho geral, usa `DisputaSupervisao.objects.filter(colaborador=colaborador, fechada_em__isnull=True).first()`, também sem status do pai (`:900`). Sem disputa, chama `ChamadoColaborador.abrir` com categoria `disputa_supervisao`, módulo `disputa_supervisao_manual`, subcategoria `orfao_14h`, `status_local__in=VIVOS`, e associa o retorno como pai (`:908`, `:931`). **Pai terminal: sim; disputa já fechada: também possível no repasse.** |
| **`criar_disputa_ancora_de_chamado`**, chamada em `app/chamados/services/disputa_emissao.py:1160`; definição em `:1093` | Escolhe `DisputaSupervisao.objects.select_for_update().filter(colaborador=colaborador, fechada_em__isnull=True).first()`, **sem status do pai** (`app/chamados/services/disputa_emissao.py:1121`). Sem disputa, obtém pai por `ChamadoColaborador.abrir` com categoria `disputa_supervisao`, módulo `disputa_supervisao_manual`, subcategoria `ancora_ausente`, `status_local__in=VIVOS`; associa em `chamado=chamado_pai` (`:1128`, `:1151`). Destino é o argumento `chamado_limbo`, exigindo categoria `ancora_limbo` e recusando apenas `ENCERRADOS`: **pai terminal permitido no reuso; destino cancelado/superado não é barrado pelo teste de status**, enquanto resolvido/fechado é barrado (`:1107`, `:1109`, `:1166`). |
| **`declarar_hora_colab`**, chamada em `app/chamados/services/declaracao_texto.py:322`; definição em `:257` | Obtém disputa por `disputa_para_declaracao`, que delega ao núcleo comum acima: **pai terminal permitido no reuso de disputa aberta** (`app/chamados/services/declaracao_texto.py:307`, `app/chamados/services/disputa_emissao.py:563`, `:467`). Destino é diretamente o argumento `ch`, sem query nem teste de status nesta função; exige módulo de furo e hora correspondente a marco faltante (`app/chamados/services/declaracao_texto.py:271`, `:281`, `:292`, `:323`). **Pelo contrato da função, destino terminal é permitido.** No chamador HTTP atual, porém, `resposta_tardia.receber` antecede a declaração: chamado não vivo precisa renascer ou a resposta retorna como tardia antes de chegar à criação (`app/chamados/views.py:1053`, `:1054`, `:1090`, `app/chamados/services/resposta_tardia.py:35`, `:45`, `:67`). |
| **`aplicar_declaracao`**, chamada em `app/chamados/services/declaracao_texto.py:611`; definição em `:589` | Também obtém disputa por `disputa_para_declaracao` e pelo núcleo comum: **pai terminal permitido no reuso** (`app/chamados/services/declaracao_texto.py:605`, `app/chamados/services/disputa_emissao.py:563`, `:467`). Destino é diretamente `ch`; as propostas verificam módulo, dia e marcos faltantes, **sem filtro de status** (`app/chamados/services/declaracao_texto.py:551`, `:554`, `:563`, `:612`). **Pai e destino podem ser terminais.** O detalhe administrativo busca o chamado por PK e `ticket_id_glpi__isnull=True`, sem status, e chama esta função (`app/chamados/views.py:363`, `:480`). |
| **`acao_reabrir_disputa_admin`**, chamada em `app/chamados/services/acoes_disputa.py:259`; definição em `:232` | Escolhe explicitamente a disputa fechada mais recentemente: `DisputaSupervisao.objects.filter(chamado=ch, fechada_em__isnull=False).order_by('-fechada_em').first()` (`app/chamados/services/acoes_disputa.py:252`). Reabre **a disputa** antes da pergunta; esse método limpa os campos de fechamento da disputa (`:256`, `app/chamados/models.py:1701`, `:1713`). Não passa destino, portanto ele é `None` (`app/chamados/services/acoes_disputa.py:259`, `app/chamados/models.py:2028`). **O chamado pai pode estar resolvido/fechado exatamente no nascimento:** a pergunta nasce em `:259`; `ch.reabrir()` só vem em `:265–266`. |

No sítio `_criar_pergunta_se_couber`, o **destino** depende do ramo do agrupador: chamado de intervalo `_chi`; chamado `ch` no caminho por ata; primeiro chamado do grupo por batida, `chamados_grupo[0]`, na ordem herdada do chamador; ou `ch` no caminho sem batida de origem (`app/chamados/services/disputa_emissao.py:956`, `:978`, `:1000`, `:1012`, `:1043`). Esses chamados passam por `chamado_orfao_e_regularizavel`, cuja regra exclui apenas `ENCERRADOS`: **resolvido/fechado são recusados; cancelado/superado podem passar se satisfizerem as demais guardas** (`app/chamados/services/disputa_emissao.py:870`, `:892`, `:729`, `:691`, `:718`).

**O filtro `status_local__in=VIVOS` passado a `ChamadoColaborador.abrir` não garante retorno vivo em todos os ramos.** Após o lookup inicial, o gate remove os filtros `status_local`, procura chamados com `contexto_json.agrupado_em` e pode devolver o próprio filho terminal quando o outro chamado indicado em `agrupado_em` está vivo. O juiz confere o status desse outro pai, não o do filho devolvido (`app/chamados/models.py:670`, `:689`, `:691`, `:693`, `:695`, `:581`, `:588`, `:596`).

Na **inserção física**, `PerguntaDisputa.abrir` recebe ambos os vínculos e não exige disputa aberta nem chamado vivo. A validação de `chamado_destino` trata da compatibilidade de data/turno, sem conferir status (`app/chamados/models.py:2028`, `:2043`, `:2085`, `app/chamados/juizes.py:1702`). Assim, “excluído por `VIVOS`” acima descreve a seleção; não há revalidação de status no INSERT (`app/chamados/models.py:2085`).

Há ainda uma distinção entre **nascimento e reaproveitamento**: o dedup procura a última pergunta por colaborador, motivo e data, em qualquer disputa. Pode reapontar destino/célula, mas preserva a disputa original e retorna sem criar outra pergunta (`app/chamados/models.py:2022`, `:2061`, `:2064`, `:2071`, `:2084`).

**3) Funções existentes para “chamado vivo do dia” e terminalidade**

**Existe `cobranca_viva_do_dia(colaborador, modulo_origem, dia, excluir=None)`**. Além de colaborador/data, ela exige módulo ou conjunto de módulos (`app/chamados/juizes.py:1610`, `:1620`). Sua seleção é:

```python
ChamadoColaborador.objects.filter(
    colaborador=colaborador,
    modulo_origem__in=_mods,
    status_local__in=VIVOS,
).filter(
    Q(contexto_json__data_turno=dia.isoformat()) | Q(celula__data=dia)
).select_related('celula').order_by('-criado_em', '-pk')
```

Exclui opcionalmente o próprio chamado e retorna o primeiro cujo `dia_do_fato(c, fortes=True)` confirma o dia (`app/chamados/juizes.py:1621`, `:1624`, `:1626`).

Seus chamadores diretos de produção são:

- `_gate_premissa_do_fechamento` — `app/chamados/models.py:498`.
- `sobrevivente_por_ultimo` — `app/chamados/juizes.py:1644`.
- `_autoresolve_f2_f4` — `app/ponto/services/triagem_batida.py:589`.
- `resolver_cobranca_da_entrada` — `app/ponto/services/entrada_sincronizada.py:28`.

Há também seletores plurais: `chamados_vivos_do_dia`, consumido por `fechar_o_fio_do_dia`, e `_chamados_vivos_do_dia`, consumido pelo serviço de cobertura (`app/ponto/services/veredito_celula.py:136`, `:182`, `app/ponto/services/cobertura.py:218`, `:310`).

**Existe teste explícito de terminalidade:** `_resolv(ch)` verifica `status_local in RESOLVIDO`, sendo `RESOLVIDO = TERMINAIS`. É usado para o pai e os destinos por `classificar` e `fechar_beco` (`app/chamados/services/becos.py:13`, `:21`, `:30`, `:31`, `:43`, `:46`). Essas funções são chamadas pelo wrapper do evento e pelo comando de becos; o wrapper é acionado após commit pelo `post_save` da pergunta (`app/chamados/services/becos.py:69`, `:73`, `app/chamados/management/commands/fechar_becos_disputa.py:39`, `:57`, `app/chamados/signals.py:191`, `:200`, `:202`).

`core/juizes.py` registra C1, “o chamado está vivo?”, apontando a autoridade para `catalogo/motor.py::VIVOS`; a pergunta sobre fio vivo é registrada separadamente em C15 (`app/core/juizes.py:107`, `:227`, `:123`, `:241`).

**4) Leitores dependentes desses vínculos**

| Leitor | Regra efetiva |
|---|---|
| **Fila administrativa `qs_na_fila`** | Não busca perguntas: parte de chamados em `VIVOS`, restringe a `COBRANCA` e exclui os silenciados no futuro. **Uma pergunta viva não faz seu pai terminal entrar nessa fila** (`app/chamados/services/painel.py:155`, `:186`, `:187`). |
| **Crédito de perguntas / partição por verbos** | `particionar_verbos` chama `creditos_perguntas`. Este busca perguntas cujo destino **ou** pai está no universo de chamados vivos fornecido; credita ao destino se ele pertence a esse universo, senão ao pai. Se ambos são terminais, a pergunta fica fora dessa seleção (`app/chamados/services/verbos.py:31`, `app/chamados/services/raiox.py:54`, `:144`, `:146`). |
| **`fio.chamado_que_lista`** | Retorna `p.chamado_destino_id or d.chamado_id`, **sem consultar status** (`app/chamados/services/fio.py:116`, `:117`). |
| **Montagem do fio administrativo** | Carrega todos os chamados do colaborador, sem status; inclui perguntas de disputas abertas e perguntas vivas de disputas fechadas. Agrupa por `chamado_que_lista` e anexa ao card. Portanto, pai/destino terminal não é excluído apenas pelo status (`app/chamados/services/fio.py:174`, `:213`, `:218`, `:234`, `:236`, `:249`). |
| **PWA do colaborador** | Usa `montar_chamados_fio(..., so_janela=True)`: restringe chamados à janela e não inclui perguntas de disputas fechadas. Os formulários consultam `p.viva` (`app/chamados/views.py:569`, `app/chamados/services/fio.py:169`, `:213`, `app/templates/chamados/partials/_card_atendimento.html:77`). |
| **Janela do colaborador** | Amplia a janela com **destinos** de perguntas `q_vivas`, não com seus pais. Filtra colaborador, janela ou destino, `silenciado_ate IS NULL` e módulos internos; **não filtra status** (`app/chamados/services/janela_colab.py:61`, `:67`, `:70`). |
| **API nativa `api_chamados_v2`** | Usa essa janela e somente disputas abertas. O destino vence **apenas se estiver em `VIVOS`**; caso contrário, ancora no pai da disputa. Só serializa perguntas nos cards presentes; `pode_responder` é falso para `ENCERRADOS` (`app/api/views.py:1015`, `:1028`, `:1047`, `:1155`, `:1177`). |
| **Questionário separado `api_disputa_perguntas`** | Usa `perguntas_do_app`, selecionando por colaborador, `q_vivas` e `pergunta_viva`, **sem exigir disputa aberta ou pai vivo**. Cada item informa o destino; o envelope usa o primeiro destino pendente não nulo ou o pai da primeira disputa, sem teste de status nessa escolha (`app/api/views.py:2603`, `app/chamados/services/pergunta_no_app.py:42`, `app/api/views.py:2626`, `:2642`). |
| **Lote de validação / mensageria** | `chamado_da_pergunta` retorna destino ou pai, sem status. É consumido na montagem do lote e pela API de mensageria (`app/chamados/services/validacao.py:786`, `:788`, `:869`, `app/api/views_mensageria.py:542`, `:546`). |

Portanto, os leitores **não compartilham uma única regra de âncora**: fio/lote preferem qualquer destino não nulo; crédito administrativo prefere destino pertencente ao universo vivo; API nativa prefere destino cujo status está em `VIVOS` (`app/chamados/services/fio.py:117`, `app/chamados/services/validacao.py:788`, `app/chamados/services/raiox.py:144`, `app/api/views.py:1047`).

Ocorrências concretas no banco de produção e eventual criação por código externo ao recorte: **[nao verificado]**.

## 29/10 21:55 · sessao c6476a4dda70 · - · - · origem=Ronald (tmux)

**Censo: 6 chamadas produtivas a `PerguntaDisputa.abrir`, mais 1 inserção física no próprio gate.** A outra classe `Pergunta` encontrada é `dataclass` de catálogo, sem ORM (`app/chamados/catalogo/perguntas.py:14`, `:17`, `:18`); o modelo persistente é `PerguntaDisputa`, com `disputa` FK obrigatória (`app/chamados/models.py:1849`), e a disputa aponta para `chamado` opcional (`app/chamados/models.py:1324`).

| Sítio / função | Escolha da disputa/chamado pai | Destino escolhido | Terminal no nascimento |
|---|---|---|---|
| `app/chamados/models.py:2085` — `PerguntaDisputa.abrir`, assinatura `:2028` | Recebe `disputa` do caller; não escolhe nem testa `fechada_em`/`disputa.chamado.status_local` (`:2036`, `:2085`). Dedup de fato pode devolver pergunta de outra disputa, sem trocar `ex.disputa`: consulta `do_fato(...).last()` em `:2061`, retorna em `:2084`; identidade filtra apenas colaborador, motivo, data (`:2022`). | Recebe `chamado_destino`; `resolvedora_valida` é chamada em `:2043-2051` e verifica datas/turno, sem status (`app/chamados/juizes.py:1702-1738`). | **Sim**, ambos os vínculos podem apontar para pais terminais se fornecidos; gate não impõe vitalidade. |
| `app/chamados/services/disputa_emissao.py:389` — `gerar_perguntas_disputa`, definição `:280` | Recebe disputa; fluxo usual `criar_ou_atualizar_disputa` chama núcleo em `:598` e gera perguntas em `:606`, `:618`. Núcleo escolhe `DisputaSupervisao.objects.select_for_update().filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:467-469`), **sem filtrar status do chamado pai**, retorna existente em `:502`. Sem disputa: `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao', status_local__in=VIVOS, defaults=...)` (`:504-519`), depois cria disputa com esse pai (`:523-528`). | `_derivar_chamado_destino` chamado em `:380`, `:382`; query exata: `ChamadoColaborador.objects.filter(colaborador=disputa.colaborador, status_local__in=VIVOS).exclude(categoria='disputa_supervisao')` (`:181-184`); filtros adicionais motivo atendido (`:194-196`), ausência de pergunta mesmo motivo não materializada (`:209-212`), dia (`:214-217`), desempate por turno (`:219-220`), exige candidato único (`:221-237`). Pode nascer sem destino e chamar reatamento depois (`:404-410`). | **Pai da disputa: sim**, ramo disputa existente só confere `fechada_em`; **destino derivado: terminais excluídos pela query VIVOS** (conclusão sobre elegibilidade, sem afirmar proteção contra corrida). |
| `app/chamados/services/disputa_emissao.py:770` — `_criar_pergunta_se_couber`, definição `:756` | Recebe disputa. No agrupador `criar_disputa_a_partir_de_chamados_orfaos`, há **dois ramos**: repasse escolhe `_pv = PerguntaDisputa.objects.filter(chamado_destino=ch, materializada_em__isnull=True).select_related('disputa').first()` (`:874`) e usa `_dv = _pv.disputa` (`:877`), **sem filtrar fechamento/status**, gerando irmãs em `:885` → `:813`; ramo geral usa `DisputaSupervisao.objects.filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:900-902`). Sem disputa, pai por `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao_manual', subcategoria='orfao_14h', status_local__in=VIVOS, defaults=...)` (`:908-923`), FK em `:931`. | Recebe destinos da lista do caller: intervalo `_chi` (`:956-958`); por ata `ch` (`:978-979`); grupo por batida escolhe `canonico = chamados_grupo[0]`, primeiro da ordem herdada (`:998-1001`), passado em `:1012-1018`; sem origem passa `ch` (`:1043-1049`); repasse passa `ch` (`:885`). Listas submetidas a `chamado_orfao_e_regularizavel` (`:870`, `:892`), que chama `motivo_fora_da_fabrica` (`:729`). Esse juiz exclui apenas `ENCERRADOS` (`:691-692`), não todos TERMINAIS. | **Disputa: pode estar fechada no repasse** (`:874-885`). **Chamado pai: pode ser terminal**, nenhum teste no reuso. **Destino: resolvido/fechado excluídos; cancelado/superado não excluídos pelo teste de status**, podendo passar demais guardas (`:689-720`). |
| `app/chamados/services/disputa_emissao.py:1160` — `criar_disputa_ancora_de_chamado`, definição `:1093` | `DisputaSupervisao.objects.select_for_update().filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:1121-1123`), sem status do chamado pai. Sem disputa: `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao_manual', subcategoria='ancora_ausente', status_local__in=VIVOS, defaults=...)` (`:1128-1143`), FK em `:1151`. | Argumento `chamado_limbo`, exigindo categoria `ancora_limbo` (`:1107-1108`) e recusando apenas `status_local in ENCERRADOS` (`:1109-1110`); passado como destino em `:1166`. | **Pai da disputa: sim** no reuso; **destino: cancelado/superado permitidos pelo teste de status**, resolvido/fechado recusados. |
| `app/chamados/services/declaracao_texto.py:322` — `declarar_hora_colab`, definição `:257` | `disputa_para_declaracao(...)` (`:307-309`) → núcleo (`app/chamados/services/disputa_emissao.py:563-566`), mesma query/novo pai da linha de `gerar_perguntas_disputa` acima (`:467-528`). | Argumento `ch`, passado em `:323`. Função não filtra status; exige módulo de furo (`:271-272`), data e hora casando marco faltante (`:281-295`). | **Contrato da função: permite destino terminal**, pois não testa status. **HTTP atual protege**: `app/chamados/views.py:1046` pega por `pk,colaborador`, sem status, mas chama `resposta_tardia.receber` em `:1053`; retorna antes da declaração quando tardia (`:1054-1055`). `receber` deixa vivos seguir (`app/chamados/services/resposta_tardia.py:35-36`), ou renasce e atualiza objeto (`:45-47`), ou devolve `tardia` (`:67`). **Chamado pai da disputa continua podendo ser terminal**, por núcleo reutilizar disputa aberta sem testar pai. |
| `app/chamados/services/declaracao_texto.py:611` — `aplicar_declaracao`, definição `:589` | `disputa_para_declaracao(...)` (`:605-607`), mesmo núcleo/queries (`app/chamados/services/disputa_emissao.py:467-528`, `:563-566`). | Argumento `ch`, passado em `:612`. Propostas exigem módulo/dia/marco faltante, **sem status** (`app/chamados/services/declaracao_texto.py:551-568`). | **Sim**, pai da disputa e destino podem ser terminais. HTTP busca `get_object_or_404(ChamadoColaborador, pk=chamado_id, ticket_id_glpi__isnull=True)` (`app/chamados/views.py:363`), sem status; gestor chama aplicar em `:478-480`, sem `resposta_tardia` nesse ramo. |
| `app/chamados/services/acoes_disputa.py:259` — `acao_reabrir_disputa_admin`, definição `:232` | Query **explicitamente fechada**: `DisputaSupervisao.objects.filter(chamado=ch, fechada_em__isnull=False).order_by('-fechada_em').first()` (`:252`); reabre disputa antes (`:256`), via `reabrir_admin` que zera fechamento (`app/chamados/models.py:1701-1713`). | Não passa destino (`app/chamados/services/acoes_disputa.py:259-264`), portanto `None` pelo default (`app/chamados/models.py:2028`). | **Chamado pai pode estar resolvido/fechado no instante exato da criação**: `.abrir` ocorre em `:259`, `ch.reabrir()` apenas **depois**, em `:265-266`. Disputa já está reaberta. |

**Atenção ao significado de `ChamadoColaborador.abrir(... status_local__in=VIVOS)`: não garante retorno vivo em todos os ramos.** O gate primeiro faz `cls.objects.filter(colaborador=colaborador, **lookup).first()` (`app/chamados/models.py:670`), mas depois remove todos os filtros `status_local` (`:691`) e procura objetos com `contexto_json__has_key='agrupado_em'` (`:693-694`), retornando `_f` quando `agrupado_sob_pai_vivo()` (`:695-696`). Esse juiz verifica o status do **outro chamado referido em `agrupado_em`**, não do próprio `_f` (`:581`, `:588-596`). Assim, mesmo a obtenção de pai “novo” nos três emissores pode devolver um chamado terminal agrupado sob outro vivo. O comentário declara expressamente “devolve o filho” (`:689-690`), e a docstring identifica filho resolvido (`:575-580`).

Estados exatos: `VIVOS=('aberto','em_analise','registrado')`, `ENCERRADOS=('resolvido','fechado')`, `TERMINAIS=ENCERRADOS+('cancelado','superado')` (`app/chamados/catalogo/motor.py:24`, `:30-31`).

A enumeração negativa (“nenhum outro create/admin/SQL/serializer”) resulta de busca estática em todos os Python de produção e buscas por SQL/aliases/reverse manager; não há referência única de código capaz de provar ausência global **[não verificado]**. Os seis callers e a inserção física acima são os sítios concretos encontrados.

## 29/10 21:54 · sessao 77a2372554f7 · - · - · origem=Ronald (tmux)

**Mapa dos vínculos:** pergunta → `disputa` é FK obrigatória; o chamado pai é `pergunta.disputa.chamado`; existe também `pergunta.chamado_destino` opcional. Referências: `app/chamados/models.py:1849`, `app/chamados/models.py:1324`, `app/chamados/models.py:2283`.

| Leitor | Dependência e comportamento |
|---|---|
| Fila administrativa `qs_na_fila` | **Não lê perguntas diretamente**: toma `qs_vivos` (`status_local__in=VIVOS`) e restringe a `COBRANCA`, excluindo `silenciado_ate > now`. Portanto chamado terminal não entra nessa fila, mesmo tendo pergunta viva. `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:187`. `VIVOS`, `COBRANCA`, `TERMINAIS`: `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:31`. |
| Partição administrativa por verbos | `particionar_verbos` chama `creditos_perguntas`: `app/chamados/services/verbos.py:31`. Este busca perguntas cujo destino **ou** pai pertence ao queryset de chamados vivos: `app/chamados/services/raiox.py:54`. Usa `q_vivas`, exclui materializada com veredito `VL_SIM`: `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`. Credita ao destino se ele pertence ao universo vivo fornecido; senão ao pai: `app/chamados/services/raiox.py:140`. Se ambos terminais, pergunta fica fora do universo dessa consulta: inferência direta de `app/chamados/services/raiox.py:54`. |
| `fio.chamado_que_lista` | Retorna **sempre** `p.chamado_destino_id or p.disputa.chamado_id`; não consulta status de nenhum deles: `app/chamados/services/fio.py:116`, `app/chamados/services/fio.py:117`. |
| Montagem do fio admin | Busca todos os chamados do colaborador, sem filtro de status: `app/chamados/services/fio.py:174`. Busca disputas com chamado não nulo: `app/chamados/services/fio.py:181`. Carrega perguntas das disputas abertas e, no admin, perguntas candidatas a vivas das disputas fechadas; confirma estas com `pergunta_viva`: `app/chamados/services/fio.py:213`, `app/chamados/services/fio.py:218`, `app/chamados/services/fio.py:231`. Agrupa pela âncora `chamado_que_lista` e anexa ao card correspondente: `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:249`. Assim, pai terminal não é excluído só pelo status no fio admin; destino existente ganha a âncora mesmo terminal: `app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:117`. |
| Fio PWA do colaborador | `meu_atendimento` chama `montar_chamados_fio(..., so_janela=True)`: `app/chamados/views.py:566`, `app/chamados/views.py:569`. Nesse modo usa `chamados_visiveis_colab` e **não** inclui perguntas de disputas fechadas: `app/chamados/services/fio.py:169`, `app/chamados/services/fio.py:213`. Os formulários usam as perguntas anexadas ao card e `p.viva`: `app/templates/chamados/partials/_card_atendimento.html:73`, `app/templates/chamados/partials/_card_atendimento.html:77`. |
| Janela dos chamados do colaborador | Estende a janela com IDs de **destinos**, não pais, de perguntas `q_vivas`: `app/chamados/services/janela_colab.py:61`. Consulta chamados por janela OR esses destinos, colaborador, `silenciado_ate IS NULL`, excluindo módulos internos. **Não há filtro de status atual nessa função**: `app/chamados/services/janela_colab.py:67`, `app/chamados/services/janela_colab.py:70`. Portanto não reproduzir literalmente comentários antigos dizendo que todo terminal some da lista. |
| API nativa `/chamados` (`api_chamados_v2`) | Usa a mesma janela: `app/api/views.py:1015`. Considera apenas disputas abertas: `app/api/views.py:1028`. Sua âncora difere do fio: destino vence somente quando `status_local in VIVOS`; caso contrário usa `p.disputa.chamado_id`: `app/api/views.py:1043`, `app/api/views.py:1047`. Serializa perguntas por card presente na lista: `app/api/views.py:1122`, `app/api/views.py:1158`, `app/api/views.py:1177`. Campo `pode_responder` é falso para `ENCERRADOS`: `app/api/views.py:1155`. |
| Questionário separado do app (`api_disputa_perguntas`) | Usa `perguntas_do_app`: `app/api/views.py:2603`. A seleção filtra colaborador por disputa, `q_vivas` e `pergunta_viva`, **sem exigir disputa aberta nem pai vivo**: `app/chamados/services/pergunta_no_app.py:42`. Cada item informa `chamado_id=p.chamado_destino_id`: `app/api/views.py:2626`. O `chamado_id` do envelope é o primeiro destino não nulo de pergunta não RESPONDIDA, ou o pai da primeira disputa; **não valida status desse destino/pai**: `app/api/views.py:2615`, `app/api/views.py:2642`. |
| Detalhe local administrativo | Resolve apenas disputa aberta vinculada diretamente ao chamado exibido; depois filtra suas perguntas por `pergunta_viva`. Não usa âncora destino para selecionar a disputa no GET: `app/chamados/services/acoes_chamado.py:335`, `app/chamados/services/acoes_chamado.py:341`, `app/chamados/services/acoes_chamado.py:348`. |
| Portas administrativas que consomem a listagem | Validar/recusar/corrigir no detalhe verificam igualdade com `chamado_que_lista`, exigindo disputa aberta: `app/chamados/views.py:493`, `app/chamados/views.py:499`, `app/chamados/views.py:510`, `app/chamados/views.py:516`, `app/chamados/views.py:528`, `app/chamados/views.py:534`. No fio, validação permite disputa fechada se pergunta continua viva: `app/chamados/services/fio.py:589`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:594`, `app/chamados/services/fio.py:599`; recusar/corrigir ainda exigem disputa aberta: `app/chamados/services/fio.py:626`. |
| Fila de validação em lote / IA | `chamado_da_pergunta` também retorna destino ou pai, sem status: `app/chamados/services/validacao.py:786`. Consumidores: montagem do lote `app/chamados/services/validacao.py:869` e resposta da API de mensageria `app/api/views_mensageria.py:542`, `app/api/views_mensageria.py:546`. |
| Juiz de irmãs com resposta pendente | Seleciona pergunta por destino `=ch` OU pai `=ch` quando destino não está em `VIVOS`; depende expressamente da mesma regra de crédito: `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:1474`. |

**Conclusão para o relatório:** há três regras de âncora diferentes: fio/lote usam destino não nulo ou pai; crédito administrativo usa destino pertencente ao universo vivo ou pai; API nativa usa destino de status VIVOS ou pai. Referências respectivas: `app/chamados/services/fio.py:117`, `app/chamados/services/validacao.py:788`, `app/chamados/services/raiox.py:144`, `app/api/views.py:1047`.

## 29/10 21:54 · sessao 293a5aca92dc · - · - · origem=Ronald (tmux)

Evidências para o item 3:

- **Há seletor explícito de chamado vivo do dia:** `cobranca_viva_do_dia(colaborador, modulo_origem, dia, excluir=None)`, em `app/chamados/juizes.py:1610`. Filtra `colaborador`, `modulo_origem__in` e `status_local__in=VIVOS`; restringe a `contexto_json__data_turno=dia.isoformat()` **ou** `celula__data=dia`; ordena por `-criado_em, -pk`; exclui opcionalmente o próprio chamado; retorna o primeiro para o qual `dia_do_fato(c, fortes=True) == dia` (`app/chamados/juizes.py:1620–1626`).
- Seus chamadores diretos de produção são `_gate_premissa_do_fechamento`, em `app/chamados/models.py:498`; `sobrevivente_por_ultimo`, em `app/chamados/juizes.py:1644`; `_autoresolve_f2_f4`, em `app/ponto/services/triagem_batida.py:589`; e `resolver_cobranca_da_entrada`, em `app/ponto/services/entrada_sincronizada.py:28`.
- `sobrevivente_por_ultimo` põe o chamado escolhido por aquele juiz por último no lote (`app/chamados/juizes.py:1641–1647`). É chamado por `fechar_o_fio_do_dia` (`app/ponto/services/veredito_celula.py:182`), pelo cartório (`app/ponto/services/cartorio.py:1196`) e pelo serviço de cobertura (`app/ponto/services/cobertura.py:310`).

**Status declarados:**

```python
VIVOS      = ('aberto', 'em_analise', 'registrado')
COBRANCA   = ('aberto', 'em_analise')
ENCERRADOS = ('resolvido', 'fechado')
TERMINAIS  = ENCERRADOS + ('cancelado', 'superado')
```

A declaração está em `app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`. O registro de juízes formula C1 como “o chamado está vivo?” (`app/core/juizes.py:107`) e aponta a autoridade para `chamados/catalogo/motor.py::VIVOS` (`app/core/juizes.py:227`); separa a pergunta sobre o fio em C15 (`app/core/juizes.py:123–125`, `:241`).

**Há função que testa terminalidade**, embora local ao serviço de becos: `_resolv(ch)` retorna `ch is not None and getattr(ch, 'status_local', None) in RESOLVIDO` (`app/chamados/services/becos.py:21–22`), sendo `RESOLVIDO = TERMINAIS` (`app/chamados/services/becos.py:11–13`). Portanto, não cabe afirmar ausência absoluta de função “pai terminal”.

- `classificar(d)` usa `_resolv` para destinos das perguntas (`app/chamados/services/becos.py:30`) e para `d.chamado`, o pai (`:31`).
- `fechar_beco(d, sistema)` usa para o pai (`app/chamados/services/becos.py:43`) e destinos (`:46`).
- `classificar` é chamado pelo wrapper `fechar_beco_da_disputa` (`app/chamados/services/becos.py:69`) e pelo comando `fechar_becos_disputa` (`app/chamados/management/commands/fechar_becos_disputa.py:39`).
- `fechar_beco` é chamado pelo wrapper (`app/chamados/services/becos.py:73`) e pelo comando (`app/chamados/management/commands/fechar_becos_disputa.py:57`).
- O wrapper é chamado após commit pelo signal `post_save` de `PerguntaDisputa` (`app/chamados/signals.py:191–202`).

Outros seletores do dia existentes:

- `chamados_vivos_do_dia(colaborador, data, cel=None)` reúne chamados com `status_local__in=VIVOS` e `contexto_json__data_turno=data.isoformat()`, somando os `cel.chamados` também vivos sem duplicar (`app/ponto/services/veredito_celula.py:136–157`). Chamador direto: `fechar_o_fio_do_dia`, `app/ponto/services/veredito_celula.py:182`.
- `_chamados_vivos_do_dia(titular, dia)` percorre chamados do titular em `VIVOS`, ordenados por `-criado_em,-pk`; aceita data da célula igual ao dia ou, sem data de célula, a data declarada pelo catálogo (`app/ponto/services/cobertura.py:218–231`). Chamador: `app/ponto/services/cobertura.py:310`.
- `agrupado_sob_pai_vivo` é específico do pai registrado em `contexto_json.agrupado_em`: busca por PK, verifica mesmo colaborador e retorna `pai.status_local in VIVOS` (`app/chamados/models.py:581–596`). Seus leitores de produção: fechamento (`app/chamados/models.py:495`), dedup de abertura (`:695`), impedimento de reabertura (`:1012`), badge do espelho (`app/escala/utils.py:1036`) e calendário do colaborador (`app/colaboradores/services/calendario.py:556`).

## 29/10 21:48 · sessao 613a61b3159d · piloto-desligamento · leitura · origem=Code

A leitura mostra dois limites importantes: **o resumo do espelho pode usar a competência inteira, mesmo quando os dias foram limitados por uma data**, e **o saldo cadastrado de férias não calcula proporcionais até essa data**. São comportamentos distintos no código, detalhados abaixo. (`app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:1288`; `app/ferias/models.py:216`.)

A competência contendo a DATA é obtida por `app/ponto/janelas.py::janela_atual(data, empresa=None)`. O corte vem de `Empresa.dia_inicio_competencia`, com padrão 21 e limite efetivo entre 2 e 28; `janela_fechamento(mes, ano, empresa=None)` identifica a competência pelo mês/ano do **fim**. Com corte 21, retorna dia 21 do mês anterior até dia 20 do mês informado. ([janelas.py:10](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ponto/janelas.py:10), `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`.)

**B1 — Espelho apurado: há uma fachada atual, mas ela não garante o resumo parcial até a DATA.** (`app/ponto/services/espelho.py:1198`, `app/ponto/services/espelho.py:1288`.)

- **FONTE:** `app/ponto/services/espelho.py::espelho_do_colab(colaborador, hoje, apur_ini, apur_fim, *, ano=None, modo_gestor=False, agora)`. Retorna, entre outras chaves, `dias`, `resumo` e `resultado`. É exatamente a função chamada pela tela atual, que publica `dias` e `resumo_mes`. ([espelho.py:1198](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ponto/services/espelho.py:1198), `app/ponto/services/espelho.py:1298`, `app/ponto/views.py:154`, `app/ponto/views.py:158`.)
- **UNIDADE:** trabalhadas, extras, atrasos e `horas_falta` são **horas decimais**; `dias_falta` é contagem de dias e `datas_falta` é lista de datas. A tela formata as horas com uma casa decimal seguida de `h`, não em h:min. (`app/relatorios/cartao_pela_celula.py:93`, `app/relatorios/cartao_pela_celula.py:102`, `app/relatorios/cartao_pela_celula.py:104`, `app/relatorios/cartao_pela_celula.py:111`; `app/templates/ponto/espelho.html:38`, `app/templates/ponto/espelho.html:62`.)
- **UNIVERSO:** um colaborador explícito. Os dias visuais vão de `min(piso_visual(colaborador, hoje), apur_ini)` até `hoje`; portanto podem começar **antes da competência**. O resumo inicial restringe as rubricas aos períodos cuja entrada local pertence a `[apur_ini, apur_fim]`. Depois, se essas pontas forem uma competência completa, `folha_manda` aplica os totais dessa competência. (`app/ponto/services/espelho.py:1229`, `app/ponto/services/espelho.py:1266`, `app/ponto/services/espelho.py:83`, `app/ponto/services/espelho.py:450`; `app/relatorios/cartao_pela_celula.py:321`.)
- **EXCLUSÕES:** a apuração exclui batidas retratadas; pendentes do DP continuam entrando. Na fonte final folha/lavratura, `total_trabalhadas` usa horas normais, enquanto folga trabalhada fica separada em `trab_folga`. Furo apurado sem decisão não se torna automaticamente falta decidida. (`app/ponto/turnos.py:2818`, `app/ponto/turnos.py:2838`; `app/relatorios/cartao_pela_celula.py:111`, `app/relatorios/cartao_pela_celula.py:116`, `app/relatorios/cartao_pela_celula.py:174`, `app/relatorios/cartao_pela_celula.py:373`.)

A fonte diária e a fonte do resumo são diferentes:

| Parte | Fonte efetiva e limite |
|---|---|
| Registros por dia | `app/ponto/services/dia_pago.py::linhas_do_periodo(colab, ini, fim, *, versao='motor')`: mapa `{date: DiaPago}`, somente linhas `tipo=dia`, com datas inclusivas. (`app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:852`.) |
| Rubricas que o HTML imprime por dia | `app/ponto/services/dia_pago.py::rubricas_do_dia(linha)`: trabalhadas, extras 50/100, atraso, antecipada, noturnas e intra, em horas. **Não retorna `horas_falta`**. Sem linha, sinaliza `lavrada=False`; o template informa ausência de apuração. (`app/ponto/services/dia_pago.py:899`, `app/ponto/services/dia_pago.py:908`, `app/ponto/services/dia_pago.py:914`; `app/templates/ponto/espelho.html:263`.) |
| Outra medida diária disponível | `app/ponto/services/dia_pago.py::pago_do_dia(linha)`: trabalhadas + folga trabalhada + folga-feriado; pode retornar `None`. Alimenta `dia.pago_h`, enquanto a linha de rubricas do HTML imprime `rubricas.trabalhadas`. (`app/ponto/services/dia_pago.py:865`, `app/ponto/services/dia_pago.py:888`, `app/ponto/services/dia_pago.py:895`; `app/ponto/services/espelho.py:822`; `app/templates/ponto/espelho.html:268`.) |
| Resumo final | `app/relatorios/cartao_pela_celula.py::folha_manda(colaborador, data_ini, data_fim_pedido, resumo)`: tenta folha, depois lavratura; sem ambas, conserva o resumo do motor com `fonte_dos_totais='sem apuracao ainda'`. (`app/relatorios/cartao_pela_celula.py:296`, `app/relatorios/cartao_pela_celula.py:321`, `app/relatorios/cartao_pela_celula.py:340`.) |

**Corte e relógio:** a fachada recebe `hoje` e `agora`, mas não os transmite a toda a cadeia. `folha_manda` recebe as pontas da competência, sem `hoje`; `fechamento_lido` recebe apenas mês/ano e o recalculador captura seu próprio `timezone.now()`. A construção dos dias em aberto também chama `ate_ontem` e `turno_aberto_de` sem injetar o relógio da fachada. Portanto, passar a DATA em `hoje` **não garante** totais e estados inteiramente avaliados nesse corte. (`app/ponto/services/espelho.py:1288`; `app/ponto/services/fechamento.py:1703`, `app/ponto/services/fechamento.py:1727`, `app/ponto/services/fechamento.py:595`; `app/relatorios/cartao_pela_celula.py:37`, `app/relatorios/cartao_pela_celula.py:398`, `app/relatorios/cartao_pela_celula.py:412`.)

Há outras candidatas, com contratos diferentes:

| Candidata e assinatura | O que responde |
|---|---|
| `app/ponto/services/dia_pago.py::soma_do_periodo(colaboradores, ini, fim, *, versao='motor')` | **Totais lavrados até um corte explícito**, por colaborador, incluindo trabalhadas, extras, atraso e `horas_falta`. Retorna também `sem_lavratura` e `sem_apuracao`. Exclui ajustes de competência, DSR e banco; não consulta relógio no corpo. É a soma por intervalo, mas não reproduz a seleção de fontes do resumo atual. (`app/ponto/services/dia_pago.py:34`, `app/ponto/services/dia_pago.py:750`, `app/ponto/services/dia_pago.py:782`, `app/ponto/services/dia_pago.py:828`.) |
| `app/ponto/services/espelho.py::autoridade_do_periodo(colaborador, ini, fim)` | Resultado do motor para intervalo explícito, utilizado pela fachada; sozinho não é o resumo final da tela. (`app/ponto/services/espelho.py:982`, `app/ponto/services/espelho.py:1195`, `app/ponto/services/espelho.py:1231`.) |
| `app/ponto/services/espelho.py::somar_periodos(periodos, periodos_ft, dentro=None)` | Soma rubricas do motor, em horas; aceita filtro de intervalo, não soma faltas e não escolhe folha/lavratura. (`app/ponto/services/espelho.py:51`, `app/ponto/services/espelho.py:77`, `app/ponto/services/espelho.py:117`.) |
| `app/ponto/services/fechamento.py::fechamento_lido(colaborador, mes, ano)` | Totais online da competência inteira; pode devolver `None`, sem parâmetro de corte. (`app/ponto/services/fechamento.py:1703`, `app/ponto/services/fechamento.py:1727`.) |
| `app/relatorios/cartao_pela_celula.py::totais_da_folha(colab, mes, ano)` | Mapeia o fechamento online para o vocabulário do resumo; exige existir fechamento cadastrado. Sem corte livre. (`app/relatorios/cartao_pela_celula.py:44`, `app/relatorios/cartao_pela_celula.py:69`.) |
| `app/relatorios/cartao_pela_celula.py::_totais_da_lavratura(colab, mes, ano)` | Alternativa da competência inteira quando não há totais da folha. Usa `soma_da_competencia`, não a soma parcial. (`app/relatorios/cartao_pela_celula.py:126`, `app/relatorios/cartao_pela_celula.py:141`.) |
| `app/relatorios/pdf_espelho.py::_coletar_dados_espelho(colaborador, data_ini, data_fim, _turnos_juiz=None, *, agora)` | Coletor do PDF: limita dias a ontem, lê a fachada e reaplica `folha_manda`; não é a função escolhida pela tela HTML. (`app/relatorios/pdf_espelho.py:187`, `app/relatorios/pdf_espelho.py:286`, `app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:624`.) |

A própria seleção da janela difere entre consumidores: a web usa `janela_fechamento` para mês/ano do GET, com padrão no mês civil atual; a API usa `janela_atual` quando mês/ano coincidem com o mês civil atual. Ambas chamam `espelho_do_colab`. (`app/ponto/views.py:97`, `app/ponto/views.py:101`, `app/ponto/views.py:154`; `app/api/views.py:1362`, `app/api/views.py:1369`.)

**B2 — Faltas e ausências justificadas: não há uma saída única nas fontes identificadas que reúna os dois conjuntos com tipo.** Há lista de faltas decididas, cobertura por dia e lista de registros. (`app/folha/export.py:158`; `app/ponto/turnos.py:2487`, `app/ponto/turnos.py:2707`.)

- **FONTE das faltas:** `app/folha/export.py::datas_falta_janela(colaborador, ini, fim)`. **UNIDADE:** lista de datas; a função não retorna horas nem o tipo junto de cada data. **UNIVERSO:** um colaborador, intervalo inclusivo informado, sem competência obrigatória. **EXCLUSÕES:** só entram ausências aprovadas de tipos que o catálogo vigente declara `DESCONTA`; ficam fora pendentes, rejeitadas, tipos abonados/suprimidos e furo sem decisão. Recebe as pontas e não consulta relógio no corpo. ([export.py:158](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/folha/export.py:158), `app/folha/export.py:75`, `app/folha/export.py:135`, `app/folha/export.py:168`.)
- **FONTE por dia, com tipo:** `app/ponto/turnos.py::cobertura_ausencia_periodo(colab, ini, fim, *, para)`. **UNIDADE:** mapa `{date: Ausencia}`, com `tipo`, status e minutos no registro. **UNIVERSO:** um colaborador e dias inclusivos do intervalo, expandindo dias únicos, intervalos fechados e abertos permitidos. **EXCLUSÕES:** `para='folha'` aceita só aprovadas; `'cobranca'` exclui apenas rejeitadas. Em sobreposição, conserva somente a ausência de maior `(data, pk)`. Não restringe automaticamente a “justificadas”; também pode devolver falta ou ausência que suprime o dia. Recebe as pontas, sem relógio no corpo. ([turnos.py:2707](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ponto/turnos.py:2707), `app/ponto/turnos.py:2475`, `app/ponto/turnos.py:2727`, `app/ponto/turnos.py:2743`; `app/ponto/models.py:749`.)
- **FONTE de todos os registros com tipo:** `app/ponto/turnos.py::ausencias_do_periodo(colab, ini, fim, *, para)`. Retorna queryset dos registros que intersectam o intervalo, com o mesmo filtro folha/cobrança, **sem eliminar sobreposições**. Não retorna uma contagem pronta de dias nem filtra apenas justificadas; recebe datas, sem relógio no corpo. (`app/ponto/turnos.py:2487`.)

As candidatas próximas e suas diferenças são:

| Candidata e assinatura | Diferença |
|---|---|
| `app/folha/export.py::datas_falta(colaborador, empresa, mes, ano)` | Mesma pergunta das faltas, pela competência inteira da empresa; delega a `datas_falta_janela`. Não recebe corte parcial. (`app/folha/export.py:173`.) |
| `app/folha/export.py::ausencias_na_janela(colaborador, tipos, ini, fim)` | Registros aprovados dos tipos pedidos, preservando tipo; aceita corte explícito. É utilizada pela expansão das faltas. (`app/folha/export.py:60`, `app/folha/export.py:149`.) |
| `app/folha/export.py::datas_abonadas(colaborador, empresa, mes, ano)` | Datas da rubrica 8932, **sem tipo no retorno**. Exclui férias, declaração e licença maternidade; portanto não é lista geral de justificadas. Só recebe competência. (`app/folha/export.py:182`, `app/folha/export.py:190`; `app/ponto/catalogo/ausencias.py:290`.) |
| `app/ponto/services/efeito_ausencia.py::efeitos_da_ausencia(colaborador, data_ini, data_fim)` | Conjuntos de justificadas/suprimidas/dia inteiro e minutos parciais. Inclui `Justificativa` aprovada, além de ausências; não devolve tipo. (`app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/efeito_ausencia.py:36`, `app/ponto/services/efeito_ausencia.py:59`.) |
| `app/ponto/services/dia_decidido.py::decididos_do_periodo(colaborador, data_ini, data_fim, *, ausencias=None)` | Histórico por dia com tipo/status/minutos, **incluindo rejeitadas**. Sobreposições têm desempate; registro sem fim expande só o dia inicial nesse corpo. (`app/ponto/services/dia_decidido.py:504`, `app/ponto/services/dia_decidido.py:521`, `app/ponto/services/dia_decidido.py:542`.) |
| `app/ponto/turnos.py::ausencias_vigentes(de, ate, colaborador=None)` e `::ausencias_do_dp(de, ate, colaborador=None)` | A primeira não filtra status/tipo; a segunda exclui férias e rejeitadas. Omitir colaborador permite universo coletivo. Não equivalem a justificadas aprovadas. (`app/ponto/turnos.py:2921`, `app/ponto/turnos.py:2944`, `app/ponto/turnos.py:2948`.) |

**O espelho atual usa várias delas:** cobertura `'cobranca'` para o ícone, cobertura `'folha'` para o rótulo, `decididos_do_periodo` para decisões e `efeitos_da_ausencia` na alimentação do motor. As faltas decididas do resumo passam por `folha_manda → datas_falta_janela`. (`app/ponto/services/espelho.py:590`, `app/ponto/services/espelho.py:599`, `app/ponto/services/espelho.py:658`, `app/ponto/services/espelho.py:1072`; `app/relatorios/cartao_pela_celula.py:373`.)

**B3 — Férias vencidas e proporcionais: as fontes localizadas não atendem o bloco completo.** Há saldo cadastrado, status por data e avisos; cálculo de proporcionais/avos até o corte não foi localizado. A inexistência absoluta de outra implementação permanece **[não verificado]**.

- **FONTE do saldo por aquisitivo:** `app/ferias/models.py::PeriodoAquisitivo.dias_saldo(self)`, uma **property**. **UNIDADE:** dias. **UNIVERSO:** aquele período aquisitivo cadastrado, sem competência mensal. **EXCLUSÕES:** subtrai concedidos e vendidos. Não recebe data, não calcula proporcionalidade nem reconstrói saldo histórico: retorna `dias_direito - dias_concedidos - dias_vendidos`. ([models.py:216](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ferias/models.py:216), `app/ferias/models.py:119`, `app/ferias/models.py:124`.)
- **FONTE da classificação na DATA:** `app/ferias/models.py::status_de_periodo(pa, hoje=None)`. **UNIDADE:** categoria textual. Saldo não positivo resulta em `concedido`; fim do aquisitivo maior ou igual à referência resulta em `em_curso`; depois, limite de concessão menor que a referência resulta em `vencido`; o restante é `adquirido`. Recebe data; se omitida, usa `date.today()`. Avalia os contadores atuais de saldo antes das datas. ([models.py:65](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ferias/models.py:65), `app/ferias/models.py:94`.)
- **FONTE agregada:** `app/ferias/situacao.py::situacao_de_ferias(colab, hoje=None)`. Retorna `saldo_total` em dias, períodos, agendamentos e avisos. Busca todos os aquisitivos do colaborador, sem recorte de competência ou de início até a DATA; recebe referência ou usa `date.today()`. O aviso de vencido exige limite de concessão `< hoje` e saldo positivo. ([situacao.py:138](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ferias/situacao.py:138), `app/ferias/situacao.py:155`, `app/ferias/situacao.py:162`, `app/ferias/situacao.py:192`.)

Há uma diferença entre a descrição e o corpo de `situacao_de_ferias`: embora a docstring fale em períodos já adquiridos, `saldo_total` soma **todos os períodos com saldo positivo**, sem excluir em curso ou futuros cadastrados. O retorno também não separa um total numérico exclusivo de vencidas; o aviso textual mostra no máximo três períodos. (`app/ferias/situacao.py:146`, `app/ferias/situacao.py:200`, `app/ferias/situacao.py:296`.)

As outras candidatas não calculam proporcionais:

- `app/ferias/models.py::vencendo(pa, dias=DIAS_VENCENDO, hoje=None)` retorna booleano para adquirido próximo do limite; exclui vencidos por exigir `adquirido`. `app/ferias/services.py::periodos_vencendo(hoje=None, dias=JANELA_VENCIMENTO_DIAS, colaborador=None, empresa=None)` retorna queryset por **status armazenado**, com universo coletivo quando colaborador é omitido. (`app/ferias/models.py:252`; `app/ferias/services.py:111`.)
- `app/ferias/models.py::dias_direito_por_faltas(faltas)` aplica faixas de direito; `PeriodoAquisitivo.faltas_periodo(self)` conta registros aprovados do tipo literal `falta`, pela data inicial dentro do aquisitivo. Não expande intervalos nem calcula avos. (`app/ferias/models.py:52`, `app/ferias/models.py:140`.)
- `app/ferias/views.py::saldo_ferias(request, cid)` lê o relógio e sincroniza os períodos, em cadeia que pode salvar alterações; não é um leitor puro. O espelho de ponto usa a cobertura diária de férias, sem agregar saldo por aquisitivo nesse fluxo. (`app/ferias/views.py:98`, `app/ferias/views.py:108`, `app/ferias/views.py:113`; `app/ferias/services.py:238`; `app/ponto/services/espelho.py:590`.)

**B4 — O que segura a folha: existe a função individual; motivos e portas não vêm numa fonte única completa.** (`app/folha/fora_da_folha.py:179`; `app/folha/retrato_folha.py:974`.)

- **FONTE:** `app/folha/fora_da_folha.py::o_que_segura(colab, mes, ano)`. Retorna `entra`, `motivo`, `grupo`, `motivos_retencao` e lista de dias com data/veredito/motivo/fio/ação/IDs de chamados. **Não retorna URLs**, nem as travas de aprovação. ([fora_da_folha.py:179](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/folha/fora_da_folha.py:179), `app/folha/fora_da_folha.py:209`, `app/folha/fora_da_folha.py:211`.)
- **UNIDADE:** categorias, datas ISO e IDs; `dias` é uma lista, não horas ou dinheiro. `entra` contém o status textual da classificação, podendo ser `None` quando não existe item para o colaborador. (`app/folha/fora_da_folha.py:191`, `app/folha/fora_da_folha.py:209`, `app/folha/fora_da_folha.py:212`.)
- **UNIVERSO:** um colaborador, competência inteira da empresa. A classificação geral vem dos fechamentos cadastrados para empresa/mês/ano; os dias são consultados separadamente, mesmo quando o motivo geral não é `furo_espelho`. (`app/folha/fora_da_folha.py:188`, `app/folha/fora_da_folha.py:193`; `app/folha/export.py:764`.)
- **CORTE/RELÓGIO:** não recebe a DATA. A autoridade diária é `app/folha/export.py::motivos_retencao_celula(colab, ini, fim, hoje=None, com_dias=False)`, mas a fachada não injeta `hoje`, levando ao uso de `timezone.localdate()`. Nessa autoridade, `hoje` distingue passado para ausência de célula/veredito; **não limita todos os motivos**, pois vereditos acusadores não passam pelo teste `d < hoje`. (`app/folha/fora_da_folha.py:90`; `app/folha/export.py:1150`, `app/folha/export.py:1179`, `app/folha/export.py:1213`, `app/folha/export.py:1215`.)
- **EXCLUSÕES diárias:** isentos; dias anteriores à admissão/posteriores à demissão; dias sem célula e sem vínculo; ausência de célula/veredito em hoje/futuro ou no dia da demissão; protestos integralmente não cobráveis; furo exclusivamente da pausa nas condições específicas da função. Uma célula acusadora no dia da demissão ainda pode reter. (`app/folha/export.py:1174`, `app/folha/export.py:1180`, `app/folha/export.py:1209`, `app/folha/export.py:1211`, `app/folha/export.py:1235`, `app/folha/export.py:1237`.)

Os **motivos gerais do TXT**, em ordem de precedência, são estes. A classificação para no primeiro aplicável; portanto não enumera todos os problemas gerais simultâneos. (`app/folha/export.py:746`, `app/folha/export.py:840`, `app/folha/export.py:948`.)

| Motivo | Porta existente comprovada |
|---|---|
| `rescisao_modulo_proprio` | O ramo orienta ao módulo de rescisão do Domínio, fora do TXT mensal; não fornece URL Django. (`app/folha/export.py:840`.) |
| `sem_codigo_dominio` | `/colaboradores/<cid>/editar/` → `colaboradores.views::editar_colaborador`; a gravação atribui `codigo_dominio`. (`app/folha/export.py:846`; `app/colaboradores/urls.py:50`; `app/colaboradores/views.py:1334`.) |
| `ferias_com_batida` | Não há escolha única codificada. Existem `/ferias/agendamento/<ag_id>/editar-datas-admin/` → `ferias.views::editar_datas_admin`, cancelamento do agendamento e `/ponto/batida/<batida_id>/retratar/` → `ponto.views::retratar_

[truncado em 20000 de 25389 caracteres]

## 29/10 21:46 · sessao e24e3f8ecbfa · - · - · origem=Ronald (tmux)

B4 tem fonte pronta para “o que segura”, mas a resposta e as portas de resolução estão em funções distintas; o mapa de portas é explicitamente parcial. `o_que_segura` devolve textos de ação e IDs de chamados, sem URLs. (`app/folha/fora_da_folha.py:179-213`; `app/folha/retrato_folha.py:974-1025`)

**FONTE e assinatura**

- `app/folha/fora_da_folha.py::o_que_segura(colab, mes, ano)`: retorna competência, colaborador/nome, `entra` (valor textual do `status`, não booleano), `motivo`, `grupo`, `motivos_retencao` e lista `dias`; cada dia tem `data`, `veredito`, `motivo`, `fio`, `destrava`, `chamado`. (`app/folha/fora_da_folha.py:179-213`)
- A classificação de entrada vem de `app/folha/export.py::classificar_export(empresa, mes, ano, fechamentos=None)`, buscando o item daquele colaborador entre os fechamentos da empresa; quando o colaborador não aparece, `entra`/`motivo` ficam `None`. (`app/folha/fora_da_folha.py:188-193`, `app/folha/fora_da_folha.py:211-213`; `app/folha/export.py:746-769`)
- Os dias vêm da autoridade `app/folha/export.py::motivos_retencao_celula(colab, ini, fim, hoje=None, com_dias=False)`, chamada com `com_dias=True`. (`app/folha/fora_da_folha.py:90-94`; `app/folha/export.py:1150`, `app/folha/export.py:1200-1240`)

**UNIDADE**

É uma resposta categórica com **lista de dias ISO**, motivos e IDs, sem valor monetário ou total de horas; a função não traz contador `dias`, mas uma lista. (`app/folha/fora_da_folha.py:209-213`)

**UNIVERSO, corte e relógio**

- Um colaborador na competência `mes/ano`, com janela completa da empresa obtida por `janela_fechamento`; não recebe data de último dia trabalhado. (`app/folha/fora_da_folha.py:179`, `app/folha/fora_da_folha.py:188-193`)
- A janela usa `Empresa.dia_inicio_competencia` por `corte_da_empresa`, default 21 e clamp 2–28. (`app/ponto/janelas.py:36-57`)
- A retenção diária recorta início pela admissão e fim pela demissão; janela colapsada retorna vazia. (`app/folha/export.py:1180-1188`)
- `o_que_segura` não injeta `hoje`, portanto a retenção lê `timezone.localdate()` internamente. O núcleo diário permite receber `ini`, `fim` e `hoje`, mas `hoje` serve para decidir se um dia sem célula/veredito é passado: **não é teto geral de corte**, porque o ramo de vereditos acusadores não testa `d < hoje`. (`app/folha/fora_da_folha.py:90-94`; `app/folha/export.py:1179`, `app/folha/export.py:1206-1238`)
- Mesmo que o motivo geral do TXT seja cadastro, férias ou rescisão, `o_que_segura` também busca os dias por célula; não condiciona a lista diária a `motivo == furo_espelho`. (`app/folha/fora_da_folha.py:191-194`)

**EXCLUSÕES diárias**

- Isento de ponto: nenhum motivo/dia. (`app/folha/export.py:1173-1178`)
- Dias fora de admissão/demissão; sem célula **e** sem vínculo cobrindo o dia; ausências de célula/veredito no próprio dia da demissão; ausência de célula/veredito em hoje ou futuro. (`app/folha/export.py:1180-1187`, `app/folha/export.py:1209-1214`, `app/folha/export.py:1237-1238`)
- Dia acusador com protestos todos em `NAO_COBRAVEIS_DO_COLABORADOR`; hoje esse conjunto contém apenas `FURO_PRE_ADOCAO`. (`app/folha/export.py:1227-1236`; `app/ponto/supra_juiz.py:34-36`)
- Furo exclusivamente de pausa com protestos exatamente `{'FURO_PARCIAL'}` e confirmação do juiz de marcos; entrada/saída abertas, desalinhamento ou desconhecimento não recebem essa isenção. (`app/folha/export.py:1122-1129`, `app/folha/export.py:1132-1147`, `app/folha/export.py:1235-1236`)

**Todos os motivos gerais do TXT e portas encontradas**

| Motivo | Condição e porta existente comprovada |
|---|---|
| `rescisao_modulo_proprio` | Qualquer `data_demissao` preenchida exclui do TXT mensal; o detalhe manda para módulo de RESCISÃO do Domínio. Não há URL Django fornecida nesse ramo. (`app/folha/export.py:840-845`) |
| `sem_codigo_dominio` | Código vazio; cadastro pode ser editado em `/colaboradores/<cid>/editar/` → `colaboradores.views::editar_colaborador`, cujo formulário contém `codigo_folha` e cuja gravação atribui `codigo_dominio`. (`app/folha/export.py:846-849`; `app/colaboradores/urls.py:50`; `app/templates/colaboradores/editar.html:62`; `app/colaboradores/views.py:1237`, `app/colaboradores/views.py:1334`) |
| `ferias_com_batida` | Datas de batida dentro de férias não canceladas; exclui batidas retratadas. Não há porta única no mapa: tipo `ferias` é `None`. Existem portas específicas de editar datas/cancelar agendamento, exigindo `ag_id`, e retratar batida, exigindo `batida_id`. Escolher qual ato resolve cada caso não está codificado em `o_que_segura`. (`app/folha/export.py:453-494`, `app/folha/export.py:850-858`; `app/folha/retrato_folha.py:993`; `app/ferias/urls.py:18-22`; `app/ponto/urls.py:61`) |
| `cadastro_zero` | Fechamento zero, ≥2 batidas apuráveis e nenhum previsto na janela; o detalhe manda cadastrar vínculo/escala e recalcular. Vínculo: `/colaboradores/vincular/<cid>/painel/` → `painel_vinculo`; gravação `/colaboradores/<cid>/vincular/escala/` → `vincular_escala`; recálculo POST `/ponto/fechamento/recalcular/` → `recalcular_fechamento`. (`app/folha/export.py:876-940`; `app/colaboradores/urls.py:12-13`; `app/ponto/urls.py:31`; `app/ponto/views.py:1227-1254`) |
| `furo_espelho` | Algum motivo diário da autoridade de célula; encaminhamento depende do fio/causa, não apenas deste rótulo. O raio-X atual aponta `/chamados/painel/?colab_id=<cid>`; há também a porta de decidir fato do dia POST `/ponto/celula/veredito/` → `veredito_celula_view`, com `colab`, `datas`/`data`, `veredito` e opcional `tipo_ausencia`. (`app/folha/export.py:942-947`; `app/folha/views.py:602`, `app/folha/views.py:667`; `app/ponto/urls.py:58`; `app/ponto/views.py:3302-3331`) |
| `nao_homologado` | Códigos de eventos fora do catálogo Domínio; porta de rubricas `/folha/rubricas/` → `folha.views::lista_rubricas`, com vincular/desvincular evento/código por empresa. O mapa do retrato classifica como cadastro e abre cadastro do colaborador, não fornece um link específico de rubricas por este motivo. (`app/folha/export.py:948-953`; `app/folha/urls.py:8`; `app/folha/views.py:27-55`; `app/folha/retrato_folha.py:211-213`, `app/folha/retrato_folha.py:992`) |

As URLs acima recebem os prefixos do projeto em `app/config/urls.py:26-34`.

A ordem de precedência do motivo geral é a da tabela: a função sai no primeiro ramo aplicável; portanto não é inventário completo de todos os motivos gerais simultâneos. (`app/folha/export.py:840-858`, `app/folha/export.py:933-953`)

**Todos os motivos diários e portas por fio**

Os motivos possíveis são `sem_celula` e `espelho_` concatenado aos sete `VEREDITOS_ACUSAM`: `espelho_furo`, `espelho_discordante`, `espelho_indefinida`, `espelho_cobranca_indevida`, `espelho_cobrado`, `espelho_nunca_bateu`, `espelho_fato_em_ausencia`. Não existe nesse código um mapa “cada um desses oito motivos → uma URL”; o retorno aplica o fio do dia. (`app/folha/export.py:1212-1238`; `app/escala/models.py:1735-1736`; `app/folha/fora_da_folha.py:203-213`)

| Fio | Texto/porta comprovada |
|---|---|
| `admin` | Validar ou recusar resposta; ações POST no detalhe local `/chamados/local/<chamado_id>/`, `acao=validar_pergunta` ou `recusar_pergunta`; alternativa POST `/chamados/validar-inline/<cid>/` → `views_cobrar::validar_inline`. O mapa de portas do retrato dá `None` ao fio admin e delega ao tipo. (`app/folha/fora_da_folha.py:38-42`; `app/chamados/urls.py:14`, `app/chamados/urls.py:33`; `app/chamados/views.py:483-518`; `app/chamados/views_cobrar.py:145-154`; `app/folha/retrato_folha.py:975`) |
| `colab` | Colaborador responde e depois admin valida; `/chamados/disputa/responder/<pergunta_id>/` → `responder_pergunta_disputa`; mapa do retrato abre `/chamados/modal/<colaborador_id>/` → `modal_fio_colaborador` com rótulo “Cobrar no fio”. `o_que_segura` não fornece `pergunta_id`. (`app/folha/fora_da_folha.py:40`, `app/folha/fora_da_folha.py:209-213`; `app/chamados/urls.py:17`, `app/chamados/urls.py:39`; `app/folha/retrato_folha.py:976`) |
| `chamado` | Resolver chamados vivos cujos IDs saem em `chamado`; detalhe `/chamados/local/<chamado_id>/`; mapa do retrato abre modal do fio com “Validar no fio”. (`app/folha/fora_da_folha.py:198-210`; `app/chamados/urls.py:14`, `app/chamados/urls.py:39`; `app/folha/retrato_folha.py:977`) |
| `nada` | Texto “SEM FIO… este dia precisa de alguém para olhar”; nenhum URL fornecido. No retrato esse estado gravado aparece como `ninguem`, com `None` explícito no mapa. (`app/folha/fora_da_folha.py:42`, `app/folha/fora_da_folha.py:59-73`; `app/folha/retrato_folha.py:978`) |

O mapa por **tipo** abre vínculo para `escala`, `folga`, `sem_escala`, e cadastro para `cadastro`; dá `None` para `nao_bate`, `falta`, `ferias` e sem causa. O próprio comentário registra que a porta de ausência/justificativa exige chamado, não colaborador; `/chamados/local/<chamado_id>/ir-para-ausencia/` redireciona para `/ponto/ausencias/?registro=<aid>`. (`app/folha/retrato_folha.py:980-995`; `app/chamados/urls.py:44`; `app/chamados/views.py:1494-1519`)

**Pergunta distinta: travas de aprovação**

`classificar_export` também devolve `travas_aprovacao`, separada do motivo de entrada no TXT: `conta_vazia` se horas trabalhadas zero, `turno_aberto` se contador >0, `sem_ancora_12x36` para escala ativa 12x36 sem âncora de colaborador ativo. (`app/folha/export.py:751-754`, `app/folha/export.py:786-814`, `app/folha/export.py:826`)

- `conta_vazia`: modo selecionado explicitamente ignora a trava; aprovação individual `/ponto/fechamento/aprovar-colaborador/` e lote `/ponto/fechamento/aprovar/`. Não representa necessariamente um dado a corrigir. (`app/ponto/views.py:25-33`, `app/ponto/views.py:1506-1517`; `app/ponto/urls.py:32-33`)
- `turno_aberto`: porta de decidir dia `/ponto/celula/veredito/`, e fio do chamado quando houver; a lista `/ponto/turnos-abertos/` é apenas leitura e limita-se a ativos/turnos recentes de 26h, portanto não cobre universalmente esta trava mensal. (`app/ponto/urls.py:15`, `app/ponto/urls.py:58`; `app/ponto/views.py:362-409`, `app/ponto/views.py:3302-3331`)
- `sem_ancora_12x36`: painel de vínculo e formulário de fase; campo `data_ancora` no painel, gravação pela porta já existente de vincular escala. (`app/colaboradores/urls.py:12-15`; `app/templates/colaboradores/partials/painel_vinculo.html:156-159`; `app/colaboradores/views_fase.py:1-9`)

**Candidatas e tela atual**

- `motivos_retencao_espelho(turnos_abertos, inconsistencias, grade, fech=None, hoje=None)` é compatibilidade, ignora os três primeiros parâmetros e delega à autoridade de célula na competência do fechamento; não é outro cálculo. (`app/folha/export.py:1243-1249`)
- `fora_da_folha(empresa, mes, ano, com_itens=False)` responde pelo conjunto da empresa e só detalha por dia/fio os classificados como `furo_espelho`; não substitui a resposta individual geral. (`app/folha/fora_da_folha.py:97-105`, `app/folha/fora_da_folha.py:119-139`)
- `portas_da_linha(linha)` e `o_que_fazer(linha, hoje=None)` leem linha de retrato gravado e fornecem links/rótulos; não substituem a investigação viva por dia de `o_que_segura`. (`app/folha/retrato_folha.py:915-938`, `app/folha/retrato_folha.py:998-1035`)
- A view atual de espelho usa `espelho_do_colab` para dias/resumo; não chama `o_que_segura` nesse fluxo. Os marcos faltantes do template apontam ao painel de chamados se `c.chamado_id` existe e ficam sem link caso contrário. (`app/ponto/views.py:153-165`, `app/ponto/views.py:193-213`; `app/templates/ponto/espelho.html:228-230`)

## 29/10 21:46 · sessao d3d6975b1ed8 · - · - · origem=Ronald (tmux)

B2 não tem uma função única que devolva simultaneamente “faltas + ausências justificadas, com tipo”; existem fontes distintas para dias decididos, cobertura e histórico. Isso aparece nas saídas de `datas_falta_janela` (lista de datas), `cobertura_ausencia_periodo` (mapa data→Ausencia) e `decididos_do_periodo` (mapa data→cartão com tipo/status). (`app/folha/export.py:158`, `app/ponto/turnos.py:2707`, `app/ponto/services/dia_decidido.py:504`, `app/ponto/services/dia_decidido.py:521`)

**B2 — fontes e limites**

- **Faltas decididas:** `app/folha/export.py::datas_falta_janela(colaborador, ini, fim)` retorna `[date]`; `datas_falta(colaborador, empresa, mes, ano)` é a candidata equivalente por competência, delegando à mesma função após `janela_competencia`. (`app/folha/export.py:158`, `app/folha/export.py:173`)
- **Universo/exclusões:** um colaborador, intervalo inclusivo informado; somente `Ausencia.status='aprovada'` de tipos cujo catálogo vigente declara `DESCONTA`, expandindo intervalos e recortando pelas pontas. Portanto não é restrito ao código literal `'falta'`; não inclui furo sem decisão, ausência pendente/rejeitada nem tipos abonados/suprimidos. (`app/folha/export.py:60`, `app/folha/export.py:75`, `app/folha/export.py:135`, `app/folha/export.py:168`, `app/ponto/catalogo/ausencias.py:331`)
- **Corte/relógio:** as duas funções recebem datas ou mes/ano; os corpos não consultam relógio. O wrapper utiliza `janela_competencia(empresa, mes, ano)`, que delega a `ponto.janelas.janela_fechamento`. (`app/folha/export.py:47`, `app/folha/export.py:158`, `app/folha/export.py:173`)
- **Detalhe com tipo, por dia:** `app/ponto/turnos.py::cobertura_ausencia_periodo(colab, ini, fim, *, para)` retorna `{date: Ausencia}`; `Ausencia` traz `tipo`, `status`, `minutos`, `data`, `data_fim`. Unidade: dias identificados por data, acompanhados do registro, não um total de horas. (`app/ponto/turnos.py:2707`, `app/ponto/turnos.py:2732`, `app/ponto/models.py:749`, `app/ponto/models.py:755`)
- **Universo/exclusões da cobertura:** um colaborador; datas dentro de `[ini,fim]`; range fechado, dia único e range sem fim somente para `TIPOS_EM_ABERTO`. Com `para='folha'` entram só aprovadas; com `'cobranca'` entram todas exceto rejeitadas. Em sobreposição fica uma ausência por dia: maior `(data, pk)`. Não há filtro automático apenas de “justificadas”; o tipo e o efeito do registro distinguem abonadas, descontadas e suprimidas. (`app/ponto/turnos.py:2475`, `app/ponto/turnos.py:2727`, `app/ponto/turnos.py:2734`, `app/ponto/turnos.py:2743`, `app/ponto/services/dia_decidido.py:453`)
- **Todas as ausências, sem desempate:** `app/ponto/turnos.py::ausencias_do_periodo(colab, ini, fim, *, para)` retorna queryset de todos os registros que tocam o período com o mesmo filtro por efeito folha/cobrança. Pode preservar múltiplos lançamentos sobre o mesmo dia. (`app/ponto/turnos.py:2487`)
- **Histórico com decisões rejeitadas:** `app/ponto/services/dia_decidido.py::decididos_do_periodo(colaborador, data_ini, data_fim, *, ausencias=None)` retorna mapa por dia; inclui rejeitadas, maior `(data,pk)` vence; o cartão contém `tipo`, `status`, `minutos`, `ausencia_id`, autor e instante da decisão. Não filtra só aprovadas e, no corpo, `data_fim=None` expande apenas o dia inicial, diferindo da cobertura aberta. (`app/ponto/services/dia_decidido.py:504`, `app/ponto/services/dia_decidido.py:521`, `app/ponto/services/dia_decidido.py:542`, `app/ponto/services/dia_decidido.py:550`)
- **Espelho atual:** `montar_dias` consulta cobertura `'cobranca'` para ícone, `'folha'` para rótulo e também `decididos_do_periodo`; não escolhe uma única dessas respostas. O resumo consulta faltas decididas por `folha_manda → datas_falta_janela`. (`app/ponto/services/espelho.py:590`, `app/ponto/services/espelho.py:599`, `app/ponto/services/espelho.py:658`, `app/ponto/services/espelho.py:1287`, `app/relatorios/cartao_pela_celula.py:373`)

Outras candidatas que respondem perguntas mais estreitas:

- `app/folha/export.py::datas_abonadas(colaborador, empresa, mes, ano)` devolve datas da rubrica 8932, aprovadas, pelos tipos abonados vigentes **excluindo férias, declaração e licença maternidade**. Perde o tipo no retorno; não é lista geral de justificadas e só aceita competência, não corte livre. (`app/folha/export.py:182`, `app/folha/export.py:190`, `app/ponto/catalogo/ausencias.py:288`)
- `app/ponto/services/efeito_ausencia.py::efeitos_da_ausencia(colaborador, data_ini, data_fim)` retorna sets `justificadas`, `suprimidas`, `dia_inteiro` e `min_parcial_pagos`. Soma `Justificativa` aprovada por `data_turno` às ausências aprovadas que abonam; parcial por minutos não abona o dia inteiro. Não devolve tipo. É usado na alimentação do motor do espelho. (`app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/efeito_ausencia.py:36`, `app/ponto/services/efeito_ausencia.py:49`, `app/ponto/services/efeito_ausencia.py:59`, `app/ponto/services/espelho.py:1072`)
- `app/ponto/turnos.py::ausencias_vigentes(de, ate, colaborador=None)` é a fonte da lista administrativa: não filtra status nem tipo, e permite todos os colaboradores se o argumento for omitido. A view usa mês civil por padrão, com `de/ate` opcionais; não deriva competência. (`app/ponto/turnos.py:2948`, `app/ponto/turnos.py:2964`, `app/ponto/views.py:2177`, `app/ponto/views.py:2179`)
- `app/ponto/turnos.py::ausencias_do_dp(de, ate, colaborador=None)` delega a vigentes, exclui férias e rejeitadas; não é equivalente à lista de justificadas aprovadas. (`app/ponto/turnos.py:2921`, `app/ponto/turnos.py:2944`)

**B3 — existe saldo/status, não uma resposta completa de proporcionais até o corte**

A inexistência absoluta de outra função de férias proporcionais em toda a árvore permanece **[não verificado]**. Nos corpos efetivamente identificados, não há cálculo de avos ou proporcional: existem o saldo aritmético cadastrado e a classificação por datas, abaixo. (`app/ferias/models.py:94`, `app/ferias/models.py:215`, `app/ferias/situacao.py:296`)

- **FONTE por período:** `app/ferias/models.py::PeriodoAquisitivo.dias_saldo(self)` é uma **property**, sem data, retornando `dias_direito - dias_concedidos - dias_vendidos`, em **dias**. Universo: o PA desse objeto, com datas de início/fim e limite de concessão cadastrados. Exclui dias concedidos e vendidos; não rateia um PA em curso, não reconstrói saldo histórico na data de corte e não transforma dias em dinheiro. (`app/ferias/models.py:119`, `app/ferias/models.py:124`, `app/ferias/models.py:215`)
- **Status na data:** `app/ferias/models.py::status_de_periodo(pa, hoje=None)` devolve uma palavra, não dias. Usa a data fornecida; se omitida, `date.today()`. Saldo não positivo → `'concedido'`; `data_fim is None` ou `data_fim >= hoje` → `'em_curso'`; limite de concessão `< hoje` → `'vencido'`; restante → `'adquirido'`. O saldo atual é avaliado antes das datas. (`app/ferias/models.py:65`, `app/ferias/models.py:94`)
- **Leitor agregado:** `app/ferias/situacao.py::situacao_de_ferias(colab, hoje=None)` aceita a referência; se omitida lê `datetime.date.today()`. Busca **todos** os PAs daquele colaborador ordenados por início, sem competência e sem filtro de início/fim até o corte. Retorna `saldo_total`, `periodos`, agendamentos e avisos. (`app/ferias/situacao.py:138`, `app/ferias/situacao.py:155`, `app/ferias/situacao.py:162`)
- **Vencidas nesse leitor:** aviso `pa_vencido` para PAs com limite `< hoje` e saldo positivo; o texto lista no máximo três períodos e a âncora aponta ao primeiro. O retorno `periodos` preserva todos os PAs com saldo positivo, mas não traz lista numérica separada de vencidos. (`app/ferias/situacao.py:192`, `app/ferias/situacao.py:200`, `app/ferias/situacao.py:296`)
- **Cuidado documentado pelo corpo:** embora a docstring diga “saldo somando períodos já adquiridos”, a execução soma **todos os PAs com saldo positivo**, inclusive em curso/futuros se cadastrados; não há filtro por aquisição completa. (`app/ferias/situacao.py:146`, `app/ferias/situacao.py:162`, `app/ferias/situacao.py:296`)
- **Corte não vira histórico:** `dias_concedidos` é incrementado ao agendar, antes do gozo; portanto passar uma data antiga a `situacao_de_ferias` ou `status_de_periodo` muda comparação de datas, mas as funções continuam lendo saldo cadastrado atual, já afetado pelo agendamento. (`app/ferias/services.py:306`, `app/ferias/services.py:321`, `app/ferias/models.py:95`, `app/ferias/situacao.py:68`)

Candidatas próximas, mas não equivalentes ao bloco:

- `app/ferias/models.py::vencendo(pa, dias=DIAS_VENCENDO, hoje=None)` retorna booleano; usa `status_de_periodo == 'adquirido'` e limite até `hoje+dias`. Exclui vencidos porque exige adquirido. (`app/ferias/models.py:252`)
- `app/ferias/services.py::periodos_vencendo(hoje=None, dias=JANELA_VENCIMENTO_DIAS, colaborador=None, empresa=None)` retorna queryset filtrando **status armazenado `'adquirido'`**, sem limite inferior, sem filtro explícito por saldo. Pode consultar todos os colaboradores quando omitido; não é o mesmo predicado de `vencendo`. (`app/ferias/services.py:111`)
- `app/ferias/models.py::PeriodoAquisitivo.faltas_periodo(self)` conta registros com tipo literal `'falta'`, aprovados e **data inicial** entre início/fim do PA; não expande ranges nem recorta por desligamento. `dias_direito_por_faltas(faltas)` aplica faixas 30/24/18/12/0. Isso é redução cadastral por faltas, não férias proporcionais. (`app/ferias/models.py:52`, `app/ferias/models.py:140`)
- `app/ferias/views.py::saldo_ferias(request, cid)` lê `date.today()` e chama `p.sincronizar_status(hoje)`; essa cadeia pode salvar PA e recalcular art.130. Não é função de leitura apropriada para executar neste trabalho. (`app/ferias/views.py:98`, `app/ferias/views.py:108`, `app/ferias/views.py:113`, `app/ferias/services.py:215`, `app/ferias/services.py:238`)
- O espelho de ponto chama `espelho_do_colab`, que não agrega saldos por PA; ele consulta as **ausências de férias que cobrem dias**, via cobertura. Não há chamada a `situacao_de_ferias`/saldo de PA nesse caminho identificado. (`app/ponto/views.py:157`, `app/ponto/services/espelho.py:590`, `app/ponto/services/espelho.py:1198`)

## 29/10 21:46 · sessao 93e33cd7acec · - · - · origem=Ronald (tmux)

B1 tem uma fachada usada pelo espelho atual, mas **não há nela garantia de devolver todos os totais até uma DATA arbitrária**: `hoje` limita o motor e os dias; ao final, os totais podem ser substituídos pelos da competência inteira. Isso está no encadeamento de `espelho_do_colab` e `folha_manda` (`app/ponto/services/espelho.py:1229`, `app/ponto/services/espelho.py:1266`, `app/ponto/services/espelho.py:1287`, `app/relatorios/cartao_pela_celula.py:321`).

- **FONTE da tela:** `app/ponto/services/espelho.py::espelho_do_colab(colaborador, hoje, apur_ini, apur_fim, *, ano=None, modo_gestor=False, agora)`. Devolve `dias`, `resumo`, `resultado`, batidas e escala (`app/ponto/services/espelho.py:1198`, `app/ponto/services/espelho.py:1298`). A view atual chama exatamente essa função e publica `dias`/`resumo_mes` (`app/ponto/views.py:157`).  
- **UNIDADE:** totais trabalhados, extras, atrasos, saída antecipada, faltas e abonos em **horas decimais**; `dias_falta` é contagem de dias e `datas_falta` lista de datas (`app/relatorios/cartao_pela_celula.py:93`, `app/relatorios/cartao_pela_celula.py:102`, `app/relatorios/cartao_pela_celula.py:104`, `app/relatorios/cartao_pela_celula.py:111`). O template imprime uma casa decimal seguida de `h`, inclusive extras e atrasos (`app/templates/ponto/espelho.html:38`, `app/templates/ponto/espelho.html:51`, `app/templates/ponto/espelho.html:62`, `app/templates/ponto/espelho.html:269`).
- **UNIVERSO:** um colaborador passado explicitamente; dias visuais desde `min(piso_visual(colaborador, hoje), apur_ini)` até `hoje`, portanto os dias podem começar antes da competência. O motor usa essa mesma janela visual (`app/ponto/services/espelho.py:1229`, `app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:1266`). O resumo inicial restringe rubricas aos períodos cuja entrada local cai em `[apur_ini, apur_fim]` (`app/ponto/services/espelho.py:83`, `app/ponto/services/espelho.py:450`). Depois `folha_manda` identifica se essas duas pontas correspondem a uma competência inteira, e então tenta fontes da competência inteira (`app/relatorios/cartao_pela_celula.py:27`, `app/relatorios/cartao_pela_celula.py:321`).
- **EXCLUSÕES:** batidas retratadas ficam fora da apuração; batidas pendentes do DP continuam contando (`app/ponto/turnos.py:2818`, `app/ponto/turnos.py:2838`). No resumo inicial, períodos fora das pontas da competência não entram (`app/ponto/services/espelho.py:83`). Folga trabalhada não entra no atraso, saída antecipada nem HE 50/100 dessa soma; entra em trabalhadas, noturno, intra e em `trab_folga`/`total_extras` por tratamento próprio (`app/ponto/services/espelho.py:95`, `app/ponto/services/espelho.py:105`). Na fonte final folha/lavratura, `total_trabalhadas` usa somente `horas_trabalhadas`, e folga vai separadamente para `trab_folga` (`app/relatorios/cartao_pela_celula.py:111`, `app/relatorios/cartao_pela_celula.py:116`, `app/relatorios/cartao_pela_celula.py:174`).

**Fontes reais por dia:** a fachada carrega `DiaPago` com `app/ponto/services/dia_pago.py::linhas_do_periodo(colab, ini, fim, *, versao='motor')`; são somente linhas `tipo=dia`, versão pedida e datas inclusivas, devolvidas como `{date: DiaPago}` (`app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:852`). Cada dia recebe `dia_pago`, `pago_h` e `rubricas` (`app/ponto/services/espelho.py:816`).

- A linha monetária da **tela HTML** lê `rubricas_do_dia(linha)`, que devolve horas trabalhadas, extras 50/100, atraso, antecipada, noturno, intra e `lavrada`; não devolve `horas_falta`. Linha ausente devolve zeros com `lavrada=False`, e o template mostra “sem apuração ainda” (`app/ponto/services/dia_pago.py:899`, `app/ponto/services/dia_pago.py:908`, `app/ponto/services/dia_pago.py:914`, `app/templates/ponto/espelho.html:263`).
- `pago_do_dia(linha)` é outra medida: soma trabalhadas + folga trabalhada + folga-feriado; devolve `None` para linha ausente ou sem rubricas monetárias. Essa medida está em `dia['pago_h']`, mas a linha HTML acima imprime `rubricas.trabalhadas` (`app/ponto/services/dia_pago.py:865`, `app/ponto/services/dia_pago.py:888`, `app/ponto/services/dia_pago.py:895`, `app/ponto/services/espelho.py:822`, `app/templates/ponto/espelho.html:268`).
- Faltas também aparecem como ausência/tipo e palavra do dia, não como campo `horas_falta` em `rubricas_do_dia` (`app/templates/ponto/espelho.html:186`, `app/ponto/services/espelho.py:856`, `app/ponto/services/espelho.py:1295`, `app/ponto/services/dia_pago.py:914`).

**Hierarquia dos totais finais:**

1. `app/relatorios/cartao_pela_celula.py::totais_da_folha(colab, mes, ano)` exige existência de `FechamentoMensal`, mas lê os números online por `fechamento_lido`; se qualquer um faltar, devolve `None` (`app/relatorios/cartao_pela_celula.py:44`, `app/relatorios/cartao_pela_celula.py:69`).
2. Sem totais da folha, `::_totais_da_lavratura(colab, mes, ano)` usa `soma_da_competencia` e datas de falta de `folha.export.datas_falta`; soma a competência, incluindo ajustes, e devolve faltas decididas em dias/horas (`app/relatorios/cartao_pela_celula.py:126`, `app/relatorios/cartao_pela_celula.py:141`, `app/relatorios/cartao_pela_celula.py:166`, `app/relatorios/cartao_pela_celula.py:183`, `app/ponto/services/dia_pago.py:548`, `app/ponto/services/dia_pago.py:753`).
3. Sem ambas, conserva o resumo do motor, carimbando `fonte_dos_totais='sem apuracao ainda'`; com lavratura usa `'lavratura'`; com folha, `'folha'` (`app/relatorios/cartao_pela_celula.py:340`). No resumo inicial de `espelho_do_colab`, não há inicialização de `dias_falta`/`horas_falta`; sua lista do motor é explicitamente `datas_furo_apurado`, enquanto as faltas decididas vêm das fontes finais (`app/ponto/services/espelho.py:1234`, `app/ponto/services/espelho.py:1253`, `app/relatorios/cartao_pela_celula.py:104`, `app/relatorios/cartao_pela_celula.py:183`).

**DATA/relógio:** a fachada recebe `hoje` e `agora`, mas a cadeia não fica presa a eles. `folha_manda` não recebe nenhum desses parâmetros, chama `ate_ontem(data_fim_pedido)` sem `hoje`, que usa `timezone.localdate()`, e consulta `turno_aberto_de(colaborador)` sem `agora`, que usa `timezone.now()` (`app/relatorios/cartao_pela_celula.py:296`, `app/relatorios/cartao_pela_celula.py:398`, `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:37`, `app/ponto/turnos.py:2247`). `fechamento_lido(colaborador, mes, ano)` só recebe competência e chama o recalculador em leitura; esse recalculador captura seu próprio `timezone.now()` e usa a janela inteira da empresa (`app/ponto/services/fechamento.py:1703`, `app/ponto/services/fechamento.py:1727`, `app/ponto/services/fechamento.py:595`, `app/ponto/services/fechamento.py:600`).

**Outras candidatas existentes, com perguntas diferentes:**

| Candidata e assinatura | O que já devolve / limite |
|---|---|
| `app/ponto/services/espelho.py::autoridade_do_periodo(colaborador, ini, fim)` | Motor apurado para intervalo explícito; devolve `Autoridade(resultado, batidas, esc, tipo_escala, feriados, motor, regua_por_fatia)`. É chamada pela fachada, mas não é sozinha o resumo final da tela (`app/ponto/services/espelho.py:982`, `app/ponto/services/espelho.py:1195`, `app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:1287`). |
| `app/ponto/services/espelho.py::somar_periodos(periodos, periodos_ft, dentro=None)` | Soma rubricas do motor em horas, com filtro opcional de intervalo; não soma faltas e não aplica a seleção folha/lavratura (`app/ponto/services/espelho.py:51`, `app/ponto/services/espelho.py:77`, `app/ponto/services/espelho.py:117`). |
| `app/ponto/services/dia_pago.py::soma_do_periodo(colaboradores, ini, fim, *, versao='motor')` | Soma todas as rubricas diárias, incluindo `horas_falta`, por colaborador, entre datas inclusivas; retorna também `sem_lavratura`/`sem_apuracao`. Exclui linhas de ajuste, DSR e banco. Não lê relógio no corpo (`app/ponto/services/dia_pago.py:34`, `app/ponto/services/dia_pago.py:750`, `app/ponto/services/dia_pago.py:754`, `app/ponto/services/dia_pago.py:782`, `app/ponto/services/dia_pago.py:828`). |
| `app/ponto/services/fechamento.py::fechamento_lido(colaborador, mes, ano)` | Totais online da competência inteira; sem corte parametrizado. Pode devolver `None`; universo inclui não desligados ou desligados com demissão a partir do início da competência da empresa (`app/ponto/services/fechamento.py:1703`, `app/ponto/services/fechamento.py:1727`, `app/ponto/services/fechamento.py:415`, `app/ponto/services/fechamento.py:423`). |
| `app/relatorios/pdf_espelho.py::_coletar_dados_espelho(colaborador, data_ini, data_fim, _turnos_juiz=None, *, agora)` | Coletor do PDF, também retorna resumo/dias; limita seu fim a ontem em relação ao `agora`, lê os dias de `espelho_do_colab`, filtra-os para o intervalo e reaplica `folha_manda` com as pontas originais (`app/relatorios/pdf_espelho.py:187`, `app/relatorios/pdf_espelho.py:286`, `app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:624`). |

**A competência contendo DATA** já vem de `app/ponto/janelas.py::janela_atual(data, empresa=None)`; usa o corte da empresa e devolve o intervalo que contém a data, inclusive depois do dia 21. `janela_fechamento(mes, ano, empresa=None)` identifica a competência pelo mês/ano do seu fim, do corte do mês anterior ao corte−1 do mês informado; corte é limitado a `[2,28]`, padrão 21 (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`).

**Consumidores não têm exatamente o mesmo recorte:** a web usa sempre `janela_fechamento(mes, ano)` escolhido no GET; por padrão usa mês/ano civis do relógio. A API usa `janela_atual(agora.date())` quando o pedido é o mês/ano civil atual, e `janela_fechamento` nos demais. Ambos chamam `espelho_do_colab` (`app/ponto/views.py:97`, `app/ponto/views.py:101`, `app/ponto/views.py:154`, `app/api/views.py:1350`, `app/api/views.py:1365`). A ficha também chama a mesma fachada, mas recorta sua resposta pública para trabalhadas, extras, turnos e fonte (`app/colaboradores/services/ficha.py:196`).

_268 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
