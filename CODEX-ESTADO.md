# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 18:30** · publicado em **09/11/2026 18:35** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 18:30 · sessao 57a6d3dbea05 · FIO-E0-V4-E4-ESCRITORES · verificador · origem=Code

**PROCEDENTE — bloqueia**, pelos cinco achados consolidados ao final: relógio no C6, reversão sem relavra, emissão residual para passado sem turno e duas falhas de idempotência nos escritores tocados (`app/chamados/juizes.py:575`; `app/ponto/portas/celula.py:785`; `app/chamados/services/disputa_emissao.py:288`; `app/chamados/models.py:3945`; `app/chamados/services/fio.py:925`).

A comparação Git integral com `f2962ab8d` ficou **[nao verificado]**: esta cópia não contém `.git`. O julgamento abaixo usa o patch produtivo fornecido e os arquivos locais; execução com banco, concorrência e ocorrência dos contracasos em produção permanecem **[nao verificado]**.

**A1 — CONFORME: A03, A17 e A18 conservam a projeção.**

| Caso | Informação depois do patch | Escrita e leitura |
|---|---|---|
| A03, `reabrir_admin` | Continua nos campos da `DisputaSupervisao`: fechamento, resolução e contador de reaberturas. Não recebeu DP próprio. | Escrita em `app/chamados/models.py:2149–2161`; chamada e leitura do contador em `app/chamados/services/acoes_disputa.py:255–259,298`. |
| A17, pendência técnica | Continua em `pendencia_admin` e `recusa_motivo`. | Escrita em `app/chamados/services/materializacao.py:850–851`; leitura em `app/chamados/services/explicador.py:85–90` e `app/templates/chamados/partials/modal_fio.html:221–225`. |
| A18, recusa técnica | Continua em `recusa_motivo`; colisão também pode receber carimbo de identidade. | Escrita em `app/chamados/services/materializacao.py:1099–1107`; leitura em `app/templates/chamados/partials/_card_atendimento.html:186` e `app/chamados/services/explicador.py:85–90`. |

O catálogo de atos não fornece substitutos próprios para esses três casos; seus leitores continuam dependentes da projeção (`app/chamados/models.py:3853–3863`). Não encontrei perda dessas informações causada pelos hunks fornecidos; diff integral **[nao verificado]**.

**A2 / D8 — CONFORME quanto à preservação da visibilidade existente.**

As telas e os leitores de apresentação continuam usando `recusa_motivo`:

- **Modal administrativo:** leitura direta sob resposta/pendência, incluindo advertência (`app/templates/chamados/partials/modal_fio.html:216–225`).
- **Atendimento do colaborador:** cartão que apresenta o motivo após as alternativas por `via_resolucao`; incluído nas duas listas do atendimento (`app/templates/chamados/partials/_card_atendimento.html:176–186`; `app/templates/chamados/meu_atendimento.html:127,144`).
- **Explicação do fio:** lê pendência e motivo, é montada pelo fio e renderizada pelo modal (`app/chamados/services/explicador.py:85–90`; `app/chamados/services/fio.py:302–314`; `app/templates/chamados/partials/modal_fio.html:433–442`).

A preservação é relativa ao comportamento anterior: a exibição direta no modal tem condicionais, e o cartão dá preferência às vias. Portanto, “toda recusa aparece incondicionalmente” não está demonstrado (`app/templates/chamados/partials/modal_fio.html:221–225`; `app/templates/chamados/partials/_card_atendimento.html:176–186`).

Outros leitores conservados: validação/devolução, reversão do passivo, auditoria e interpretação de identidade de batida (`app/chamados/services/validacao.py:147,188–208`; `app/chamados/services/passivo_validado.py:66`; `app/chamados/management/commands/auditar_invariantes_chamados.py:536,1074–1075,1111–1112`; `app/ponto/services/flip_auto.py:132`).

**A3 / D9 — PROCEDENTE, bloqueia: o instante não chega ao C6.**

O caminho tocado é:

`reapontar_resolvedora` → `lavrar` → `_pergunta_materializada_de_fato` → `_dentro_da_janela_viva` → `date.today()` (`app/chamados/services/fio.py:926`; `app/chamados/veredito_lavrado.py:262–264`; `app/chamados/juizes.py:674,764,575`).

Há também o caminho fábrica → desfazer carimbo → `pergunta_viva` → janela, igualmente sem instante injetado (`app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/relavra_via.py:242`; `app/chamados/juizes.py:1387`).

`lavrar` não aceita `agora` e consulta `timezone.now()` **depois** do juiz; esse instante apenas carimba o resultado já calculado (`app/chamados/veredito_lavrado.py:254,262–273`).

**Correção mínima:** propagar o instante da borda pelos caminhos tocados até o juiz da janela, derivando dele o dia local, conforme a conversão já existente em `_hoje` (`app/chamados/veredito_lavrado.py:95–104`).

**B — CONFORME no patch produtivo fornecido: não há migração de leitores da projeção para DP.**

O nascimento acrescenta atos preservando os campos recebidos; ressuscitação e reapontamento acrescentam acordares. A preservação de R/V continua lendo a própria projeção (`app/chamados/models.py:3010–3015,1563–1568,3910–3936`; `app/chamados/services/fio.py:924–929`).

O leitor fora do Python permanece:

- **Antes/depois:** modal lê `data_conferida_em` e `data_conferida_por`, exibindo nome ou username (`app/templates/chamados/partials/modal_fio.html:169–170`).
- **Escritor mantido:** `conferir_data_do_fato` grava ambos os campos (`app/chamados/services/validacao.py:664–666`).

A retirada do fallback de classificação na fábrica está em `motivos_pela_ata`; sua conformidade com PASSADO-SEM-MARCOS é julgada no item E (`app/chamados/services/fabrica_da_celula.py:32–46`).

O PRONTO integral continua pendente: o placar puro retornou **19/20**, e o cron permanece em `JUIZES_POR_VARREDURA`. Isso é compatível com a divisão E4a-1/E4a-2 informada, mas não comprova entrega de E4a completa (`app/core/contratos_estruturais.py:261–270,1063`; `app/config/crons.py:2044–2048`).

**C — Escritores, leitores e significado dos campos.**

A porta física da sequência continua sendo `registrar`; seus campos e restrições estão no modelo (`app/chamados/models.py:3835–3897,3939–3948`).

| Ato DP | Quem solicita a escrita |
|---|---|
| Resposta | `app/chamados/services/disputa_emissao.py:1508–1509,1653–1654,1674–1675` |
| Validação | `app/chamados/services/validacao.py:139–141` |
| Devolução | `app/chamados/services/validacao.py:205–208` |
| Reabertura / recusa humana | `app/chamados/services/disputa_emissao.py:1612–1625`; `app/chamados/services/validacao.py:593–594` |
| Silêncio | `app/chamados/models.py:1924–1926`; `app/ponto/services/veredito_celula.py:194–196`; legado em `app/chamados/management/commands/backfill_decisoes_pergunta.py:324–326` |
| Retratação | `app/chamados/models.py:2056–2060` |
| Absorção | `app/chamados/services/materializacao.py:563–566` |
| Preservação de R/V sem ato correspondente | `app/chamados/models.py:3910–3936`; usada nos hunks em `app/chamados/models.py:1563–1564,3015` |

Os leitores da sequência são o histórico, seu carregamento pelo fio, a tarefa do dia e o backfill (`app/chamados/models.py:3504–3512`; `app/chamados/services/fio.py:231–232`; `app/chamados/services/tarefa_do_dia.py:337–347`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:99–101,315`).

| Projeção / valor | Escritores persistentes | Leitores relevantes |
|---|---|---|
| R: `resposta_colab`, `respondida_em` | Resposta, automática e trilha em `app/chamados/services/disputa_emissao.py:1464–1508,1649–1654,1671–1675`; limpeza em `:1627–1629`; complemento do carimbo em `app/chamados/models.py:3127–3129`; ressuscitação em `:1566`; nascimento em `:3010–3015`. | Estado/C6 em `app/chamados/models.py:3037–3044` e `app/chamados/juizes.py:1339–1351`; materializador em `app/chamados/services/materializacao.py:432,468`; histórico em `app/chamados/services/historico.py:98–109`; preservação DP em `app/chamados/models.py:3920–3930`. |
| V: `validada_em/por` | Validação/devolução em `app/chamados/services/validacao.py:134–141,199–208`; resposta nova limpa em `app/chamados/services/disputa_emissao.py:1477–1479`; reabertura em `:1627–1629`; carimbo com `resposta='validar'` em `app/chamados/models.py:3130–3133`; ressuscitação em `:1566`. | C6 em `app/chamados/juizes.py:1323`; fila em `app/chamados/models.py:3344–3355`; procedência em `app/ponto/services/procedencia_batida.py:71–74`; preservação DP em `app/chamados/models.py:3931–3935`. |
| `materializada_em/por`, `via_resolucao` | Atribuição do carimbo em `app/chamados/models.py:3093–3097`; portas em `:3099,3164,3199`; lote em `:3260–3262`; troca de via em `:3271–3272` e `app/chamados/services/relavra_via.py:125–128`; nascimento em `app/chamados/models.py:3010–3013`. | C6 em `app/chamados/juizes.py:660–687,1334–1404`; reconciliação em `app/chamados/reconciliador.py:366,1049,1504`; materialização em `app/chamados/services/materializacao.py:434–438,625–632,1093`; apresentação/cache em `app/chamados/services/raiox.py:175–187`; procedência em `app/ponto/services/procedencia_batida.py:71–74`. |
| `pendencia_admin`, `recusa_motivo` | `app/chamados/services/materializacao.py:850–851,1099–1103`; `app/chamados/services/validacao.py:584–590`; escape em `app/chamados/services/disputa_emissao.py:1494–1505`; identidade em `app/chamados/models.py:3183–3185`, chamada pela duplicata em `app/chamados/services/fio.py:1355`. | Telas e leitores discriminados em A2; interpretação textual adicional em `app/ponto/services/flip_auto.py:132`. |
| `data_conferida_em/por` | `app/chamados/services/validacao.py:664–666`. | Gate em `app/chamados/services/validacao.py:662`; exibição em `app/templates/chamados/partials/modal_fio.html:169–170`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | Lavra em `app/chamados/veredito_lavrado.py:254–275`; gravação em lote em `:240–250`; restauração em memória em `:284–289`. Os hunks salvam a lavra em `app/chamados/models.py:3000–3002` e `app/chamados/services/fio.py:926–928`. | Validade em `app/chamados/veredito_lavrado.py:107–116,213–234`; tarefa em `app/chamados/services/tarefa_do_dia.py:349–352`; raio X em `app/chamados/services/raiox.py:34–35,135–136,184–185`. |
| Identidade: colaborador, disputa, destino, célula e dia | Nascimento/reuso em `app/chamados/models.py:2845–2896,2958–3002,3010–3013`; reapontamento/rechaveamento em `app/chamados/services/fio.py:879,925–929,1038`. | Domínio do fato em `app/chamados/models.py:2697–2755`; C6 em `app/chamados/juizes.py:1336–1392`; coleta do evento em `app/chamados/models.py:3221–3244`. |
| DNA e `trabalha` | Produtor `app/ponto/management/commands/gerar_celulas.py:73–104`; gravações da porta em `app/ponto/portas/celula.py:243–244,259–273,600–604,786–795,850–871`. | Marcos em `app/escala/models.py:1117–1133`; teto em `app/ponto/services/cartorio.py:831–859`; C6 em `app/chamados/juizes.py:634–642,1369–1392`; fábrica em `app/chamados/services/fabrica_da_celula.py:87–107`. |
| `acordar_celulas` / `EventoCelula` | Coleta e emissão em `app/chamados/models.py:3221–3244`; INSERT único em `app/ponto/portas/celula.py:883–892`; novos chamadores em `app/chamados/models.py:1568,3003,3946`, `app/chamados/services/fio.py:929`, `app/chamados/signals.py:207` e `app/ponto/portas/celula.py:796`. | Consumidor em `app/chamados/reconciliador.py:1343–1401`; célula atual por colaborador/data em `app/chamados/services/fabrica_da_celula.py:260–264`; semeadura em `app/chamados/reconciliador.py:1289–1311`. |
| Pauta de cadastro sem turno | Porta em `app/chamados/services/fabrica_da_celula.py:279–298`, delegando a `app/pautas/services.py:153–186`. | Deduplicação por colaborador, departamento, cabeça e prefixo em `app/chamados/services/fabrica_da_celula.py:271–276,292–293`. |

Outros leitores de R/V/C encontrados na cópia, inclusive fora dos hunks:

- **API e telas:** `app/api/views.py:1188–1202,2584–2600,2643,2660`; `app/api/views_mensageria.py:567–570`; `app/templates/chamados/partials/_pergunta_card.html:20–42`; `app/templates/chamados/partials/_card_atendimento.html:94–100,135–186,384–385`; `app/templates/chamados/partials/_lote_preview.html:24`.
- **Apresentação e indicadores:** `app/chamados/views.py:1280,1416`; `app/chamados/views_cobrar.py:169,176,291`; `app/chamados/services/painel.py:67,262–273`; `app/chamados/services/verbos.py:240–246,311–321`; `app/chamados/templatetags/chamados_tags.py:128`; `app/chamados/relatorios_disputa.py:21`.
- **Fluxos auxiliares:** `app/chamados/canal.py:65`; `app/chamados/services/cobranca.py:229`; `app/chamados/services/furo_sem_canal.py:81`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/vigia_fio.py:675,783`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/declaracao_texto.py:227,417–419,687`; `app/chamados/services/becos.py:30`; `app/chamados/services/acoes_disputa.py:95,130,345,353`.
- **Outros domínios e comandos:** `app/ponto/supra_juiz.py:545,548`; `app/ponto/services/cartorio.py:1601`; `app/ponto/services/dia_do_colab.py:70`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1572–1575,1602–1603,1655,1766,1778`; `app/ponto/management/commands/reconciliar_grade.py:461`.

A exaustividade diante de acessos dinâmicos adicionais permanece **[nao verificado]**.

**C — Semântica: PROCEDENTE nos desacordos temporal e de época já identificados.**

- DP ordena por `ato_em, pk`; a tarefa considera o ato existente inclusivamente em `ato_em <= agora`. Resposta e validação preservam o instante da projeção (`app/chamados/models.py:3885,3927–3935`; `app/chamados/services/tarefa_do_dia.py:337–347`).
- A célula representa **dia do turno**; a batida resolve esse dia pelo pareamento. Conversa conserva a exceção explícita de dia civil (`app/ponto/turnos.py:2927–2943`; `app/chamados/services/fio.py:1612,1626–1627`).
- O teto vence inclusivamente quando `marco + 30 minutos <= agora`; o evento registra a data da célula e o instante automático de criação (`app/ponto/services/cartorio.py:828,858–859`; `app/ponto/portas/celula.py:892`; `app/escala/models.py:1857–1863`).
- A configuração usa São Paulo e datas conscientes de fuso, mas o C6 ainda usa a data do processo. A equivalência do fuso efetivo do processo em produção está **[nao verificado]** (`app/config/settings/base.py:93–95`; `app/chamados/juizes.py:575`).
- Na reversão, DNA atual e ata/veredito anteriores podem ser lidos juntos: desacordo comprovado pelo código, detalhado em E (`app/ponto/portas/celula.py:786–796`; `app/chamados/juizes.py:1369–1385`).

**D — Classes que já reprovaram.**

| Classe | Julgamento |
|---|---|
| Segundo juiz duplicando autoridade | **CONFORME nos hunks:** fábrica delega ao C6 existente; o registro conserva sua autoridade (`app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/relavra_via.py:242`; `app/core/juizes.py:278`). |
| Segundo escritor fora da porta | **CONFORME nos hunks:** DP continua no `registrar`; evento continua no emissor único. As portas da pergunta estão declaradas (`app/chamados/models.py:3945`; `app/ponto/portas/celula.py:892`; `app/core/portas.py:393–400`). |
| Relógio fora da borda | **PROCEDENTE:** D9 acima (`app/chamados/juizes.py:575`). |
| Derivação paralela de fonte única | **CONFORME quanto às autoridades reutilizadas; PROCEDENTE quanto ao caminho de emissão que escapa da guarda**, detalhado em E (`app/ponto/services/cartorio.py:831–859`; `app/chamados/services/disputa_emissao.py:288–294`). |
| Selo/RED sem mordida | **CONFORME para os sete mutantes declarados; cobertura insuficiente para afirmar correção do consumo de DNA** (`app/chamados/tests/e4_verificar.py:16–45`; `app/chamados/tests/test_e4_expand.py:284–293`). |
| Ordem de trava | **CONFORME nos novos pontos de aquisição:** pauta trava antes de selecionar/escrever; ressuscitação usa a ordem central. Prova concorrente global **[nao verificado]** (`app/chamados/services/fabrica_da_celula.py:289–294`; `app/chamados/models.py:1562,2629–2693`). |
| Pouso antes do pré-requisito | **CONFORME quanto à ausência de migração de leitores no patch; PRONTO integral não demonstrado**, com cron ainda escritor (`app/chamados/models.py:3010–3015`; `app/config/crons.py:2044–2048`). |
| Campo com dois significados | `recusa_motivo` já reúne conteúdos de donos diferentes, explicitamente declarados. O parser textual persiste; não foi introduzido pelo patch fornecido (`app/core/portas.py:385–392`; `app/ponto/services/flip_auto.py:132`). |

Nos testes sem banco, foram obtidos **29 verdes** e **RC=1 nos sete mutantes**. Eles leem e alteram em memória o fonte aplicado (`app/chamados/tests/e4_verificar.py:14–45,58–97`; `app/chamados/tests/e4_catracas.py:8–9`; `app/chamados/tests/e4_apoio.py:8–16`).

Retirando em memória os hunks executáveis fornecidos, **17 testes passam, 8 falham e 4 dão erro**. Os sobreviventes são oito testes de portas anteriores, sintaxe, um efeito canônico, seis controles E3 e uma borda E2; os doze testes de eventos/C20 detectam a retirada (`app/chamados/tests/test_e4_expand.py:127–245,254–370`; `app/chamados/tests/test_e4_efeitos.py:8–21`; `app/chamados/tests/e4_verificar.py:14,48–55`; `app/chamados/tests/test_e4_c20.py:20–174`). Reversão integral ao commit exato: **[nao verificado]**.

**E1 — PROCEDENTE, bloqueia: reversão emite evento, mas falta relavra anterior ao consumo.**

A reversão restaura DNA/trabalho e emite o evento no mesmo atomic; não atualiza ata/veredito. O consumidor segue para C6 e fábrica sem julgar previamente o DNA restaurado (`app/ponto/portas/celula.py:785–796`; `app/chamados/reconciliador.py:1364–1387`).

C6 pode retornar pelo veredito antigo antes de consultar a classificação do dia. A fábrica também pode recusar emissão pelo veredito antigo, embora o teto tenha sido calculado com o DNA atual (`app/chamados/juizes.py:1369–1392`; `app/chamados/services/fabrica_da_celula.py:87–107`; `app/ponto/services/cartorio.py:840–844`).

Contracaso executado em memória com corpos reais: restaurar DNA de trabalho sobre célula com `veredito='concorde'` conservou esse veredito, emitiu um evento e produziu C6=False; substituir o veredito por `furo` mudou C6=True. Ocorrência em produção **[nao verificado]** (`app/ponto/portas/celula.py:740–796`; `app/chamados/juizes.py:1322–1392`).

**Correção mínima:** no consumidor, julgar a célula restaurada pelo cartório antes dos leitores de C6/ata, reutilizando o juiz já chamado pela regeneração (`app/ponto/portas/celula.py:422–440`).

**E2 — CONFORME: falha ordinária do INSERT na volta a VIVOS.**

O savepoint está **dentro** de `engolir`: a exceção sai do atomic antes de ser absorvida. O observador incrementa contador e emite ERROR com contexto e causa (`app/chamados/signals.py:205–207`; `app/core/observ.py:73–91`).

Assim, esse caminho prevê perda do evento com rastro, preservando a transição externa. O teste modela justamente rollback, continuidade do cartório e registro da falha; comprovação PostgreSQL permanece **[nao verificado]** (`app/chamados/tests/test_e4_c20.py:117–174`).

A cobertura do novo acordar é limitada aos módulos de `MODULOS_DE_TURNO`; essa tupla não inclui `batida_ausente`. A abrangência de todos os renascimentos do FIO MUDO não está demonstrada (`app/chamados/signals.py:200–202`; `app/chamados/reconciliador.py:336–337`).

**E3 — PROCEDENTE, bloqueia: passado sem marcos ainda pode gerar pergunta.**

O caminho direto e o FIO MUDO do cron respeitam o novo significado de `None` (`app/chamados/services/fabrica_da_celula.py:27–41,164–168`; `app/chamados/management/commands/disparar_perguntas_competencia.py:157–161`).

Entretanto, o próprio caminho C20 alcança:

`perguntar` → `criar_ou_atualizar_disputa` → `perguntar_entradas_pendentes` → `classificar_falta` → `gerar_perguntas_disputa` → `abrir` (`app/chamados/services/fabrica_da_celula.py:138`; `app/chamados/services/disputa_emissao.py:682–685,282–294,443–452`).

O merge conserva `entradas_pendentes` anteriores; o classificador ainda devolve motivo sem marcos esperados. Portanto, o evento de um dia pode emitir pergunta para outro dia passado sem turno (`app/chamados/services/disputa_emissao.py:548–559`; `app/escala/servico_jornada.py:469–471`).

Também permanece o repasse por data, cujo gate admite ausência de escala e cuja porta auxiliar verifica ausência/dedup sem exigir turno previsto (`app/chamados/services/disputa_emissao.py:905–907,

[truncado em 20000 de 24374 caracteres]

## 09/11 18:27 · sessao bcec07067663 · - · - · origem=Ronald (tmux)

**E — Reversão de DNA: PROCEDENTE, bloqueia.** O evento é emitido no mesmo atomic da restauração, mas a restauração conserva ata/veredito anteriores (`app/ponto/portas/celula.py:785-796`). O consumidor chama C6 e fábrica sem relavrar a célula (`app/chamados/reconciliador.py:1364-1387`). C6 retorna pelo veredito antigo antes de consultar a classificação do dia (`app/chamados/juizes.py:1369-1392`). A fábrica combina teto do DNA atual com ata persistida (`app/ponto/services/cartorio.py:840-844`; `app/chamados/services/fabrica_da_celula.py:43-49`).

Contracaso executado somente em memória, por extração AST das funções reais: restaurar DNA de trabalho sobre célula antes em folga deixou `trabalha=True`, `veredito='concorde'`, um evento e C6=False; simular a lavra faltante com `veredito='furo'` mudou C6=True. Isso demonstra a dependência do veredito antigo; ocorrência em produção [nao verificado]. Fontes executadas: `app/ponto/portas/celula.py:740`; `app/chamados/juizes.py:1322`.

Correção mínima: o consumidor deve acionar o cartório para julgar o DNA restaurado antes das operações que leem C6/ata; reutilizar o juiz existente, como já faz o rabo compartilhado da regeneração (`app/ponto/portas/celula.py:422-440`).

**E — Falha do evento na volta a VIVOS: CONFORME no caminho ordinário.** O savepoint fica dentro de `engolir`, permitindo seu rollback antes de engolir a exceção (`app/chamados/signals.py:205-207`). A falha deixa contador e ERROR com contexto/causa (`app/core/observ.py:73-91`). Não há perda sem rastro nesse caminho; comprovação com transação PostgreSQL [nao verificado]. O teste existente modela a transação com stub (`app/chamados/tests/test_e4_c20.py:117-174`).

**E — PASSADO-SEM-MARCOS: PROCEDENTE, bloqueia.** O caminho principal da fábrica e o FIO MUDO do cron respeitam a nova guarda (`app/chamados/services/fabrica_da_celula.py:38-41`; `app/chamados/management/commands/disparar_perguntas_competencia.py:157-161`). Entretanto, o próprio C20 alcança um desvio:

`perguntar` → `criar_ou_atualizar_disputa` → `perguntar_entradas_pendentes` → `classificar_falta` → `gerar_perguntas_disputa` → `PerguntaDisputa.abrir` (`app/chamados/services/fabrica_da_celula.py:138`; `app/chamados/services/disputa_emissao.py:682-685`; `app/chamados/services/disputa_emissao.py:282-294`; `app/chamados/services/disputa_emissao.py:443-452`).

O contexto antigo pode conservar `entradas_pendentes` quando a fábrica atualiza apenas `data` (`app/chamados/services/disputa_emissao.py:548-559`). Sem escala ou marcos esperados, `classificar_falta` ainda retorna `orfao_14h` (`app/escala/servico_jornada.py:469-471`). Assim, evento de um dia com marcos pode emitir pergunta para outro dia passado sem turno previsto, contrariando `app/docs/PROMPTS.md:2004`.

Também permanece o repasse por data: `_classificar_falta` seguido de `_criar_pergunta_se_couber`; `_ata_do_dia` permite continuar sem escala (`app/chamados/services/disputa_emissao.py:1234-1244`; `app/chamados/services/disputa_emissao.py:905-907`). A porta auxiliar verifica ausência e dedup, sem exigir turno previsto (`app/chamados/services/disputa_emissao.py:827-842`).

Contracaso executado em memória: `classificar_falta` real, sem escala/marcos/batidas, retornou `('S','orfao_14h','SAIU')`; `perguntar_entradas_pendentes` real chamou a geração para o dia passado derivado da batida simulada. Ocorrência desse contexto em produção [nao verificado]. Fontes: `app/escala/servico_jornada.py:270`; `app/chamados/services/disputa_emissao.py:257`.

Correção mínima: aplicar a guarda compartilhada de passado sem turno nos caminhos de emissão datada, incluindo entradas pendentes e repasse, encaminhando à pauta de cadastro; preservar a classificação global usada por outros leitores (`app/chamados/services/fabrica_da_celula.py:164-168`; `app/chamados/services/fabrica_da_celula.py:279-298`).

**E — FABRICA-POR-EVENTO: cobertura parcial; zeragem [nao verificado].** Os emissores são cartório e consumidor (`app/ponto/services/cartorio.py:1006-1008`; `app/chamados/reconciliador.py:1386-1387`). O cron continua apenas contando esse universo, enquanto seu ramo FIO MUDO escreve (`app/chamados/management/commands/disparar_perguntas_competencia.py:109-113`; `app/chamados/management/commands/disparar_perguntas_competencia.py:270-295`). C20 examina somente datas presentes no lote de eventos; não recupera automaticamente todo estoque sem evento (`app/chamados/services/fabrica_da_celula.py:260-264`). A semeadura existente cobre chamados vivos e órfãs específicas, sem varrer todas as células sem pergunta (`app/chamados/reconciliador.py:1289-1311`). Portanto, o código não garante estancar a contagem; resultado no ar [nao verificado].

**F — Reapontamento: PROCEDENTE contra a exigência literal de mesmo estado 2×.** Não existe no-op para destino/dia efetivamente iguais: o caminho sempre lavra, salva e acorda a origem (`app/chamados/services/fio.py:866`; `app/chamados/services/fio.py:925-929`). A lavra sempre avança `julgado_em` (`app/chamados/veredito_lavrado.py:270-273`). Além disso, o save acorda a célula atual e a chamada explícita acorda a anterior, que podem coincidir (`app/chamados/signals.py:251-257`; `app/chamados/services/fio.py:929`). Duplicatas na fila são admitidas pelo desenho; não equivalem automaticamente a duplicação de efeito/DP (`app/escala/models.py:1848-1850`).

Correção mínima: retornar sem escrita quando destino e identidade efetivos não mudarem; emitir os acordares necessários por célula distinta no ato (`app/chamados/services/fio.py:924-929`; `app/chamados/models.py:3234-3244`).

**F — Reversão sequencial: CONFORME quanto ao estado e evento.** Limpar `dna_anterior` faz a segunda chamada retornar False e não emitir novamente (`app/ponto/portas/celula.py:777-778`; `app/ponto/portas/celula.py:787`). Confirmado no contracaso em memória: primeira=True, segunda=False, um evento. Concorrência e uma trilha efetivamente persistida [nao verificado]; a leitura antecede o atomic e não usa trava de linha (`app/ponto/portas/celula.py:776`; `app/ponto/portas/celula.py:785`).

**C — Célula/DNA e evento, mapa para consolidação.**

| Elemento | Escritores | Leitores / significado |
|---|---|---|
| DNA | Produtor `app/ponto/management/commands/gerar_celulas.py:73`; atribuições na porta em `app/ponto/portas/celula.py:243`, `:259`, `:272`, `:600`, `:786`, `:850` | Marcos congelados em `app/escala/models.py:1117`; teto em `app/ponto/services/cartorio.py:840`; C6 em `app/chamados/juizes.py:634` e `:1369` |
| `EventoCelula` | INSERT único `app/ponto/portas/celula.py:892`; acordares por pergunta `app/chamados/models.py:3227` | Consumidor `app/chamados/reconciliador.py:1315`; fábrica relê célula atual por colaborador/data em `app/chamados/services/fabrica_da_celula.py:262` |
| Dia | Célula declara dia do turno em `app/ponto/turnos.py:2939`; batida resolve pelo pareamento em `app/ponto/turnos.py:2927`; atribuição única da pergunta em `app/chamados/services/fio.py:790` | Conversa conserva exceção explícita de dia civil em `app/chamados/services/fio.py:1612` e `:1626`; evento grava a data da célula em `app/ponto/portas/celula.py:892` |
| Tempo | Instante do evento automático em `app/escala/models.py:1860`; consumidor captura agora em `app/chamados/reconciliador.py:1337` | Fábrica recebe esse instante e deriva dia local em `app/chamados/services/fabrica_da_celula.py:264`; teto por marco +30 min em `app/ponto/services/cartorio.py:828` |

Procedentes encontrados: reversão sem relavra anterior ao consumo; desvio de emissão para passado sem turno; reapontamento sem idempotência de estado (`app/ponto/portas/celula.py:785`; `app/chamados/services/disputa_emissao.py:288`; `app/chamados/services/fio.py:925`).

## 09/11 18:25 · sessao c723709f6c87 · - · - · origem=Ronald (tmux)

Complemento C — escritores R/V/C verificados por AST e leitura dos sítios, sem importar a aplicação:

| Campos | Escritores persistentes |
|---|---|
| `resposta_colab/respondida_em` | `app/chamados/services/disputa_emissao.py:1464-1465`, save `:1505`; resposta automática `:1649-1652`; pela trilha `:1671-1673`; reabertura limpa via dict `:1590` e update `:1627-1629`; nascimento recebe campos em `app/chamados/models.py:3008-3015`; `carimbar` completa resposta em `:3127-3129`; ressuscitação zera em `:1566`. |
| `validada_em/por` | `app/chamados/services/validacao.py:134-137`, devolução `:199-202`; resposta nova limpa em `app/chamados/services/disputa_emissao.py:1477-1479`; reabertura `:1590`, `:1627-1629`; `carimbar(resposta='validar')` em `app/chamados/models.py:3130-3133`; ressuscitação em `:1566`; nascimento `:3008-3015`. |
| `pendencia_admin` | `app/chamados/services/materializacao.py:850-851`. |
| `recusa_motivo` | `app/chamados/services/materializacao.py:850-851`, `:1102-1103`; via identidade em `app/chamados/models.py:3183-3185`, chamada pela colisão em `app/chamados/services/materializacao.py:1099-1100` e duplicata em `app/chamados/services/fio.py:1355`; recusa humana `app/chamados/services/validacao.py:584-590`; escape em `app/chamados/services/disputa_emissao.py:1494-1505`. |
| `data_conferida_em/por` | `app/chamados/services/validacao.py:664-666`. |

As atribuições em `app/chamados/models.py:1507-1508` pertencem a cópia de julgamento (`:1494`), e `:3159-3161` desfazem memória; não são escritores persistentes adicionais.

Leitores de produção agrupados:

- API: `app/api/views.py:1188-1190`, `:1202`, `:2584-2586`, `:2600`, `:2643`, `:2660`; mensageria `app/api/views_mensageria.py:567-570`.
- Templates: `app/templates/chamados/partials/modal_fio.html:169-170`, `:184`, `:216-227`; `_pergunta_card.html:20-32`, `:42`; `_card_atendimento.html:94-100`, `:135-155`, `:176-186`, `:384-385`; `_lote_preview.html:24`.
- Juiz/estado: `app/chamados/juizes.py:347`, `:827-828`, `:1300`, `:1323`, `:1339`, `:1447`, `:1592-1600`, `:1628`, `:1775`, `:1879-1901`, `:1949-1953`; `app/chamados/models.py:1843-1845`, `:3037-3044`, `:3344-3355`, `:3494`, `:3517-3539`, `:3920-3934`.
- Reconciliação/emissão: `app/chamados/reconciliador.py:364`, `:630`, `:1049`, `:1082`; `app/chamados/services/disputa_emissao.py:1467-1468`, `:1616-1618`, `:1644`, `:1667`, `:1710`, `:1853`, `:1914`, `:1926`, `:1931`, `:1943`.
- Materialização/validação: `app/chamados/services/materializacao.py:278`, `:356`, `:407`, `:432`, `:468`, `:627`, `:668-670`, `:829`, `:995`, `:1092`, `:1193`, `:1370-1371`; `app/chamados/services/validacao.py:139-147`, `:188-208`, `:236`, `:393-396`, `:573-582`, `:662`, `:732`, `:753`, `:851-866`, `:924`.
- Tela/fio: `app/chamados/views.py:1280`, `:1416`; `app/chamados/views_cobrar.py:169`, `:176`, `:291`; `app/chamados/services/fio.py:340`, `:660`, `:1227`, `:1239`; `app/chamados/services/explicador.py:32`, `:79-90`; `app/chamados/templatetags/chamados_tags.py:128`; `app/chamados/relatorios_disputa.py:21`.
- Histórico/indicadores: `app/chamados/services/historico.py:98-109`; `app/chamados/services/painel.py:67`, `:262-273`; `app/chamados/services/verbos.py:240-246`, `:311-321`; `app/chamados/veredito_lavrado.py:206-209`.
- Outros caminhos: `app/chamados/canal.py:65`; `app/chamados/services/cobranca.py:229`; `furo_sem_canal.py:81`; `vigia_carimbo.py:98`; `vigia_fio.py:675`, `:783`; `pergunta_no_app.py:70`; `declaracao_texto.py:227`, `:417-419`, `:687`; `becos.py:30`; `acoes_disputa.py:95`, `:130`, `:345`, `:353`; `raiox.py:138`; `passivo_validado.py:66`.
- Fora de chamados: `app/ponto/supra_juiz.py:545`, `:548`; `app/ponto/services/cartorio.py:1601`; `dia_do_colab.py:70`; `procedencia_batida.py:71-74`; `flip_auto.py:130-132`; `veredito_celula.py:196`; `app/colaboradores/services/calendario.py:474`; `ficha.py:171`; `app/folha/previa.py:142`.
- Comandos/sondas: `app/chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1075-1076`, `:1105-1112`, `:1572-1575`, `:1602-1603`, `:1655`, `:1766`, `:1778`; `curar_perguntas_em_limbo.py:32`, `:46`, `:69`; `encerrar_perguntas_com_fato.py:32`; `validar_classe_a.py:80`; `reabrir_orfas_enterradas.py:68`; `backfill_decisoes_pergunta.py:56`, `:102`, `:326`; `dedup_perguntas_fato.py:75`; `app/ponto/management/commands/reconciliar_grade.py:461`; `bin/censo_folha.py:28-29`; `bin/sombra_regras_pessoais.py:85`.

Censo acima combina atributos AST, kwargs e leituras conferidas; exaustividade diante de acesso dinâmico adicional: [nao verificado].

`recusa_motivo` tem múltiplos conteúdos preexistentes e explicitamente declarados como donos diferentes em `app/core/portas.py:385-392`; além da apresentação, `app/ponto/services/flip_auto.py:132` interpreta estrutura textual para extrair uma batida. Esse risco é legado, sem alteração desses escritores/leitores no patch fornecido.

Para F: o reenvio da mesma resposta sempre muda `respondida_em` (`app/chamados/services/disputa_emissao.py:1459-1465`) e acrescenta DP (`:1508`); o próprio contrato declara essa não idempotência em `app/core/contratos_estruturais.py:563-564`. `DecisaoPergunta.registrar`, agora tocado, continua fazendo `bulk_create` incondicional (`app/chamados/models.py:3942-3945`), sem unicidade do ato nas constraints (`:3886-3897`).

## 09/11 18:25 · sessao 5d56fd863bda · - · - · origem=Ronald (tmux)

D9 — **PROCEDENTE bloqueante**. O caminho tocado continua lendo relógio dentro do juiz: `fio.reapontar_resolvedora` chama `_lavrar` (`app/chamados/services/fio.py:926`); `lavrar` chama `_pergunta_materializada_de_fato` (`app/chamados/veredito_lavrado.py:262`); este alcança `_dentro_da_janela_viva` (`app/chamados/juizes.py:674`, `app/chamados/juizes.py:764`), que executa `date.today()` (`app/chamados/juizes.py:575`). `lavrar` não recebe instante e só lê `timezone.now()` depois de consultar o juiz (`app/chamados/veredito_lavrado.py:254`, `app/chamados/veredito_lavrado.py:271`). Correção mínima: propagar `agora` desde a borda nesses caminhos, inclusive ao juiz da janela; conservar a semântica de dia local (`app/chamados/veredito_lavrado.py:98`).

Também há caminho pelo C6: a fábrica chama `desfazer_carimbo_sem_lastro` (`app/chamados/services/fabrica_da_celula.py:172`), que chama `pergunta_viva` (`app/chamados/services/relavra_via.py:242`); seu cálculo consulta a janela sem instante (`app/chamados/juizes.py:1387`).

D, selos/RED — **CONFORME quanto aos sete mutantes declarados**. Executei o runner sem escrita de bytecode: 29 testes verdes; os sete mutantes produziram RC=1. Eles alteram em memória o texto lido do código aplicado (`app/chamados/tests/e4_verificar.py:16`, `app/chamados/tests/e4_verificar.py:58`, `app/chamados/tests/e4_catracas.py:8`); os corpos são compilados desse texto (`app/chamados/tests/e4_apoio.py:8`). A reversão é descartada em memória (`app/chamados/tests/e4_verificar.py:69`).

Reversão em memória dos **hunks executáveis** dos seis arquivos do anexo: 29 testes, **17 passam, 8 falham, 4 dão erro**. As docstrings novas da fábrica foram mantidas; identidade integral com o commit f2962ab8d fica **[nao verificado]**, pois não há repositório Git nesta cópia. Os 17 sobreviventes são:

- Oito testes de portas fora dos hunks anexados, A07/A09/A10/A11/A12/A13-A08/A14/A15 (`app/chamados/tests/test_e4_expand.py:127`, `app/chamados/tests/test_e4_expand.py:223`).
- Um de sintaxe (`app/chamados/tests/test_e4_expand.py:241`).
- Um de estado canônico do chamado (`app/chamados/tests/test_e4_efeitos.py:8`).
- Seis controles E3/C17 incluídos no runner (`app/chamados/tests/e4_verificar.py:14`).
- Um controle E2 de bordas, que testa `esta_stale`, não a injeção no C6 (`app/chamados/tests/test_e2_tarefa_do_dia.py:1409`).

Os 12 testes novos de eventos/C20 detectaram a retirada dos hunks; portanto os 17 sobreviventes não demonstram, por si, selo novo sem mordida (`app/chamados/tests/test_e4_expand.py:254`, `app/chamados/tests/test_e4_c20.py:20`). Integração ORM, concorrência e falha real PostgreSQL: **[nao verificado]**; o próprio selo declara instrumentação sem banco (`app/chamados/tests/test_e4_expand.py:1`, `app/chamados/tests/test_e4_expand_orm.py:1`).

F, `DecisaoPergunta.registrar` — **PROCEDENTE pela exigência literal de replay da porta**. A porta faz `bulk_create` incondicional e emite evento em toda chamada (`app/chamados/models.py:3942`); não há identidade única do ato nas constraints, apenas checks de formato (`app/chamados/models.py:3884`). Executando o corpo extraído por AST duas vezes com a mesma linha e ORM instrumentado: duas linhas e dois eventos. O teste H2 cobre uma chamada de sucesso e outra de falha, sem replay (`app/chamados/tests/test_e4_expand.py:359`, `app/chamados/tests/test_e4_expand.py:368`). Correção mínima: identidade idempotente do ato, deduplicação sob trava/constraint correspondente, evento condicionado à criação. Evitar deduplicar indiscriminadamente texto: a sequência admite atos posteriores distintos (`app/chamados/models.py:3817`).

D, juiz/escritor paralelo — não identifiquei novo clone do C6 nos hunks: a fábrica delega a porta existente e esta consulta `pergunta_viva` (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/relavra_via.py:242`); o registro mantém C6 na mesma autoridade (`app/core/juizes.py:278`). A escrita de DP continua na porta `registrar` (`app/chamados/models.py:3945`); censo completo dos demais escritores fica com a consolidação **[nao verificado]**.

PRONTO E4a integral — **não atingido**: módulo puro retorna `contratos_estruturais: 19/20 verdes`; a única célula não verde é chamado/juiz, dependente das exceções de varredura (`app/core/contratos_estruturais.py:261`, `app/core/contratos_estruturais.py:1063`). `disparar_perguntas_competencia` segue em `JUIZES_POR_VARREDURA` pelo fio mudo (`app/config/crons.py:2044`). Isso é compatível com a divisão E4a-1/E4a-2 fornecida no anexo; não deve ser apresentado como 20/20 (`app/core/contratos_estruturais.py:264`).

## 09/11 18:23 · sessao c723709f6c87 · - · - · origem=Ronald (tmux)

A1 — CONFORME: os três casos continuam na projeção; não foi criado fato equivalente nem migrado leitor no patch fornecido. A03 escreve `fechada_em/por`, `resolucao`, `motivo_fechamento`, `reaberturas_admin` e eventualmente `motivos` em `app/chamados/models.py:2149-2161`; a casca continua chamando essa porta em `app/chamados/services/acoes_disputa.py:255-259`, lê o contador em `:298`. A17 escreve conjuntamente `pendencia_admin` e `recusa_motivo` em `app/chamados/services/materializacao.py:850-851`; A18 escreve `recusa_motivo` em `:1102-1103`, ou pela porta de identidade em `:1099-1100` quando a colisão encerra por fato ocupado. O chamado recebe ocorrência em `:1105-1107`, mas isso não substitui a projeção como fonte dos leitores. O catálogo DP não contém atos próprios para esses três casos (`app/chamados/models.py:3853-3863`).

A2/D8 — CONFORME quanto à preservação antes/depois: os leitores continuam lendo a projeção. Leituras diretas de tela encontradas:

- Modal administrativo: `app/templates/chamados/partials/modal_fio.html:225`, sob `respondida_em` e `pendencia_admin` (`:216-221`); preserva advertência em `:222-225`.
- Atendimento do colaborador: `app/templates/chamados/partials/_card_atendimento.html:186`, com preferência anterior por `via_resolucao`; incluído por `app/templates/chamados/meu_atendimento.html:127` e `:144`.
- Explicador: `app/chamados/services/explicador.py:85-90` lê `pendencia_admin/recusa_motivo`; o fio o monta em `app/chamados/services/fio.py:302-314`. O modal renderiza seus itens em `app/templates/chamados/partials/modal_fio.html:433-442`.

Não afirmar visibilidade incondicional de toda recusa: o modal exige pendência para imprimir diretamente o campo (`app/templates/chamados/partials/modal_fio.html:221-225`), e o cartão só o mostra depois de esgotar as vias (`app/templates/chamados/partials/_card_atendimento.html:186`). Essas condições não foram alteradas pelo patch fornecido [nao verificado quanto ao diff completo ausente].

Outros leitores de `recusa_motivo`: `app/chamados/services/validacao.py:147`, `:188-208`; `app/chamados/services/passivo_validado.py:66`; `app/chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1074-1075`, `:1111-1112`; `app/ponto/services/flip_auto.py:132` extrai identidade de batida pelo texto. Os campos de formulário em `modal_fio.html:60`, `:294` e `_pergunta_card.html:114` são entradas da recusa humana, não leituras da projeção.

B/modal — CONFORME: segue lendo `data_conferida_em/por`, nome completo ou username, em `app/templates/chamados/partials/modal_fio.html:169-170`; segue sendo escrito por `app/chamados/services/validacao.py:664-666`. O patch fornecido não altera esses sítios. A leitura do dia continua comparando a data da pergunta e a antiga do chamado (`app/templates/chamados/partials/modal_fio.html:161-167`).

C — sequência DP:

| Ato/campo | Escritor/chamador da porta | Leitor |
|---|---|---|
| Porta física DP | `app/chamados/models.py:3939-3948` | — |
| `resposta` | `app/chamados/services/disputa_emissao.py:1508-1509`, `:1653-1654`, `:1674-1675` | histórico `app/chamados/models.py:3504-3512`; tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `validacao` | `app/chamados/services/validacao.py:139-141` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `devolucao` | `app/chamados/services/validacao.py:205-208` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `reabertura/recusa` | `app/chamados/services/disputa_emissao.py:1612-1625`; recusa humana chama em `app/chamados/services/validacao.py:593-594` | histórico `app/chamados/models.py:3509-3512`; modal `app/templates/chamados/partials/modal_fio.html:204-211` |
| `silencio` | `app/chamados/models.py:1924-1926`; legado `app/chamados/management/commands/backfill_decisoes_pergunta.py:324-326` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347`; seleção legado `app/chamados/management/commands/backfill_decisoes_pergunta.py:99-101`, `:315` |
| `retratacao` | `app/chamados/models.py:2056-2060` | tarefa `app/chamados/services/tarefa_do_dia.py:339-342` |
| `absorcao` | `app/chamados/services/materializacao.py:563-566` | carga com ausência `app/chamados/services/tarefa_do_dia.py:337-338` |
| Preservação R/V sem linha | `app/chamados/models.py:3910-3936`; nascimento `:3015`; ressuscitação `:1563-1564` | mesma função lê projeção; tarefa a usa em `app/chamados/services/tarefa_do_dia.py:343-347` |

O instante de resposta é o mesmo na projeção e DP (`app/chamados/services/disputa_emissao.py:1459-1465`, `:1508`); idem validação (`app/chamados/services/validacao.py:134-141`), silêncio (`app/chamados/models.py:1925`) e absorção (`app/chamados/services/materializacao.py:561-566`). A tarefa limita atos inclusivamente por `ato_em <= agora` (`app/chamados/services/tarefa_do_dia.py:339`); a ordem total DP é `ato_em, pk` (`app/chamados/models.py:3885`).

R/V/materialização mantêm leitura antiga: estado em `app/chamados/models.py:3037-3044`; carimbo físico único `_escrever_carimbo` em `:3093-3097`; `carimbar` em `:3120-3148`; volta avulsa em `:3200-3208`; volta em lote em `:3260-3262`; campos que acordam mantêm R/V/C em `:3216-3218`.

Observação útil sobre “campo com dois significados”: `recusa_motivo` já contém recusa técnica (`app/chamados/services/materializacao.py:1102`), advertência de tipo (`:850`), recusa humana (`app/chamados/services/validacao.py:584`), explicação de escape (`app/chamados/services/disputa_emissao.py:1494-1496`) e identidade da duplicata (`app/chamados/services/fio.py:1355`). O patch fornecido não cria nem altera esses usos; classificar como nova infração E4a seria incorreto [nao verificado quanto a exigência de sanar o legado].

Procedentes encontrados nesta subverificação: nenhum comprovado.

## 09/11 18:18 · sessao 87e4233cdf18 · FIO-E0-V4-E4-ESCRITORES · construtor · origem=Code

Entrega em [E4A_V3_ENTREGA.txt](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_ENTREGA.txt:1), com parque integral, mapa de campos e comando ORM.

29 testes puros passaram; os sete mutantes morderam. [Resultados e comandos](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_TESTES.txt:1). [nao verificado]

Não encontrei bypass no disparar; nenhuma cura produtiva foi necessária. [Análise](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_ENTREGA.txt:90).

Resultados ORM e certificação final: [nao verificado].

## 09/11 18:11 · sessao 06071fff5ff7 · - · - · origem=Ronald (tmux)

Não encontrei caminho do `disparar` que ainda pergunte por DATA sem previsão fora da fábrica. Não recomendo alteração produtiva.

O universo célula lê `motivos_que_o_evento_deixou` em `chamados/management/commands/disparar_perguntas_competencia.py:112`, que passa por `motivos_a_perguntar` em `chamados/services/fabrica_da_celula.py:205`, calcula o teto em `:87` e entrega esse teto a `motivos_pela_ata` em `:107`. O universo FIO MUDO também passa explicitamente o teto em `chamados/management/commands/disparar_perguntas_competencia.py:158`. `vencidos=None` retorna `[]` em `chamados/services/fabrica_da_celula.py:38`. Só os motivos selecionados chegam ao escritor em `chamados/management/commands/disparar_perguntas_competencia.py:270`.

Ajustes identificados por leitura, sem executar ORM:

- `test_fabrica_pela_ata.py::FabricaPelaAta.test_05_sem_turno_previsto_vai_ao_cadastro_sem_pergunta` exige uma saída `CADASTRO-SEM-TURNO` ausente no comando HEAD; retirar essa exigência futura, conservando zero perguntas. `chamados/tests/test_fabrica_pela_ata.py:63`.
- `test_selo_emissor_fio_mudo.py::EmissorPedeDoChamadoMudoTest` tem seis casos afetados: `test_MORDE_o_emissor_ACHA_o_chamado_mudo`, `test_MORDE_desligado_e_barrado_e_NOMEADO`, `test_MORDE_competencia_exportada_VOLTA_a_ser_pedida`, `test_MORDE_pergunta_MORTA_do_MESMO_FATO_e_piso_F1t`, `test_MORDE_o_cron_leva_os_dois_caminhos_e_diz_por_porta`, `test_o_dry_nao_grava_nada`. As fixtures não têm vínculo nem DNA; o helper injeta ata, mas não previsão. `chamados/tests/test_selo_emissor_fio_mudo.py:36`, `:62`.
- No mesmo módulo, `FioMudoNaoSobreviveAUmCicloTest.test_MORDE_o_chamado_mudo_nao_sobrevive_a_um_ciclo` usa a mesma forma sem vínculo. `chamados/tests/test_selo_emissor_fio_mudo.py:256`, `:291`. Ajuste menor: dar previsão explícita no mock `marcos_vencidos` dentro de `_lei_e_ata`, acompanhado de comentário da LEI-PASSADO-SEM-MARCOS. Acrescentar vínculo antes de plantar o Chamado pode fazer o signal emitir antes do exercício; essa consequência precisa ser considerada. `chamados/signals.py:210`.
- `test_o261_julgamento_desfaz_carimbo.py::_Cenario._cenario` tem DNA, mas nenhum vínculo. Sem EC, `marcos_com_teto` retorna `[]`; a guarda nova manda a Pauta antes da coleta de carimbos. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:53`, `ponto/services/cartorio.py:856`, `chamados/services/fabrica_da_celula.py:164`.
- Quatro casos O261 inferidos vermelhos: `JulgamentoDesfazCarimboSemLastroTest.test_MORDE_o_julgamento_desfaz_o_carimbo_da_pergunta_viva`, `.test_julgar_duas_vezes_e_idempotente`, `VigiaMaterializadaVivaTest.test_MORDE_o_vigia_conta_e_zera_depois_do_julgamento`, `.test_MORDE_exportada_conta_no_balde_do_DP_e_o_julgamento_nao_tem_fronteira`. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:88`, `:122`, `:208`, `:221`.
- O cenário O261 também alimenta `test_e1_v6_horizonte.py::FabricaLeOFatoTest.test_RED_pergunta_retratada_nao_cala_o_fato_que_volta_na_celula`, cuja expectativa passa de `['saida_sem_entrada']` a `[]`. `chamados/tests/test_e1_v6_horizonte.py:459`, `:467`. Acrescentar vínculo ao cenário compartilhado conserva a intenção desses testes sob a lei; existe `dar_vinculo` em `chamados/tests/_vinculo_do_dia.py:18`.

Nos outros apps com referências explícitas, não identifiquei expectativa alterada: F3 tem vínculo/DNA (`escala/tests/test_f3_beco1_cadastro_entrou.py:54`, `:72`); `test_cods_do_ato` e `test_classificar_falta_escala` substituem a fábrica (`ponto/tests/test_cods_do_ato.py:67`, `:95`; `ponto/tests/test_classificar_falta_escala.py:24`). Exaustividade sobre caminhos indiretos de todos os signals/cartório da árvore: [nao verificado]. Resultados ORM: [nao verificado].

## 09/11 18:10 · sessao 85fcb794b0e7 · - · - · origem=Ronald (tmux)

Inventário dos campos/valores do diff produtivo `f2962ab8d → 46d313581`:

| Campo/valor | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| DP `pergunta/pergunta_id`, `ato`, `ato_em`, `ato_por/id`, `ato_texto`, `ato_motivo` | `DecisaoPergunta.registrar`: `chamados/models.py:3945`; novas assinaturas de nascimento: `:3015`; conversão da projeção em linhas RESPOSTA/VALIDACAO: `:3929`, `:3933` | Correspondência para preservar legado: `chamados/models.py:3923`; histórico: `:3504`; tarefa por sequência/horizonte: `chamados/services/tarefa_do_dia.py:131`, `:337` |
| DP fotos `respondida_original_em`, `validada_original_em`, `validada_original_por_id` e vínculos de absorção | Portas existentes, gravadas pelo mesmo executor `chamados/models.py:3945`; campos e restrições: `:3873`, `:3894` | Histórico distingue atos com foto: `chamados/models.py:3509`; tarefa lê ato final: `chamados/services/tarefa_do_dia.py:136`; constraints: `chamados/models.py:3894` |
| Pergunta `respondida_em/resposta_colab` | Porta de resposta existente: `chamados/services/disputa_emissao.py:1464`; nascimento recebe `**campos`: `chamados/models.py:3010`; ressuscitação limpa instante via `:1566` | Nova conversão para DP: `chamados/models.py:3920`, `:3929`; C6 mantém projeção: `chamados/juizes.py:1339`; app: `chamados/services/pergunta_no_app.py:70` |
| Pergunta `validada_em/validada_por/id` | Validação existente: `chamados/services/validacao.py:134`; resposta invalida: `chamados/services/disputa_emissao.py:1477`; ressuscitação: `chamados/models.py:1566` | Conversão para DP: `chamados/models.py:3931`; C6: `chamados/juizes.py:1323` |
| Pergunta `materializada_em/materializada_por/id/via_resolucao` | Porta de carimbo: `chamados/models.py:3093`; limpeza por lote: `:3260`; ressuscitação usa a porta: `:1566` | C6: `chamados/juizes.py:1334`, `:1360`, `:1366`; fábrica busca candidatas com carimbo: `chamados/services/fabrica_da_celula.py:136` |
| Pergunta `pk`, `disputa_id`, `disputa__colaborador_id`, `celula/id`, `data_evento`, `chamado_destino/id`, `conversa`, `motivo` | Nascimento conserva campos: `chamados/models.py:3010`; reuso: `:2962`, `:2978`; reapontamento: `chamados/services/fio.py:925`; data canônica: `chamados/services/fio.py:790` | Captura de origem do evento: `chamados/models.py:3224`; reuso: `:2958`; ressuscitação: `:1565`; reapontamento: `chamados/services/fio.py:924`; fábrica/dedup: `chamados/services/fabrica_da_celula.py:113`, `:116` |
| Pergunta `veredito_lavrado/julgado_em/janela_expira_em` | Lavrador existente: `chamados/veredito_lavrado.py:270`; reuso salva os mesmos campos: `chamados/models.py:3001`; fio: `chamados/services/fio.py:927` | Validade temporal: `chamados/veredito_lavrado.py:228`; tarefa mantém seu leitor: `chamados/services/tarefa_do_dia.py:351` |
| Chamado `status_local`, `_b5_status_antes`, `modulo_origem`, `celula_id`, `pk`, `colaborador` | Estado pela porta existente: `chamados/models.py:228`; snapshot do status persistido: `chamados/signals.py:184` | Novo evento de volta a VIVOS: `chamados/signals.py:191`, `:201`; VIVOS segue catálogo: `chamados/catalogo/motor.py:24`; julgamento seguinte: `chamados/signals.py:214` |
| EventoCelula `celula_id/colaborador_id/data` | Escritor único: `ponto/portas/celula.py:892`; novos emissores: `chamados/models.py:1568`, `:3003`, `:3946`; `chamados/services/fio.py:929`; `chamados/signals.py:207`; reversão: `ponto/portas/celula.py:796` | Consumidor agrupa por colaborador e IDs: `chamados/reconciliador.py:1346`, `:1350`, `:1352`; fábrica lê datas do lote e relê célula atual: `chamados/services/fabrica_da_celula.py:260`, `:262` |
| EventoCelula `processado_em/tentativas/erro` | Consumidor existente, depois da nova fábrica: `chamados/reconciliador.py:1395`, `:1401` | Seleção pendente: `chamados/reconciliador.py:1343`, `:1393`; retenção: `:1408` |
| CelulaDia `pk/data/colaborador_id`, `trabalha`, `ata`, `veredito`, `julgada_em`; Colaborador `situacao='ativo'` | Portas existentes da célula/cartório: `ponto/portas/celula.py:900`, `:1002`; cadastro define situação: `colaboradores/models.py:323` | Fábrica mantém leitura da célula: `chamados/services/fabrica_da_celula.py:80`, `:88`, `:91`; novo encaminhamento CADASTRO: `:164`; consumidor relê atual por colaborador/data: `:262` |
| Retorno `vencidos=None`, `set()`, pares `(tipo, HH:MM)` | Adaptador do teto: `chamados/services/fabrica_da_celula.py:26`, `:28`; a autoridade continua `cartorio.marcos_vencidos` | `None`/vazio deixam de autorizar pergunta: `chamados/services/fabrica_da_celula.py:38`; `None` passado encaminha CADASTRO: `:166`; pares limitam motivos: `:49` |
| Pauta `ancora_tipo=ANCORA_COLAB`, `ancora_id`, `destinatario_departamento='dp'`, `pai=None`, marcador `CADASTRO: PASSADO SEM TURNO PREVISTO`, `autor_user` sistema `ti`, `remetente_informado='ti'`, `texto` | Novo chamador: `chamados/services/fabrica_da_celula.py:294`; escritor único efetivo: `pautas/services.py:180` | Guarda histórica por colaborador/cabeça/DP/marcador: `chamados/services/fabrica_da_celula.py:274`; `ANCORA_COLAB` é importado, sem literal inventado: `:273` |
| Celula `dna/dna_anterior/trabalha/escala_geradora_id/regenerada_em/regeneracoes` | Reversão existente conserva sua escrita: `ponto/portas/celula.py:786`, `:794`; diff acrescenta somente evento `:796` | Própria reversão lê DNA anterior: `ponto/portas/celula.py:777`, `:783`; leitor de reescrita: `escala/services/regeneracao.py:84`, `:104`; fábrica continua lendo ata/célula, `chamados/services/fabrica_da_celula.py:91`, `:107` |

O diff produtivo não altera escritores de cron, registro de juízes, campos de conferência, pendência administrativa ou recusa técnica; os novos testes podem compará-los, mas isso não acrescenta escrita produtiva. Evidência: `git diff f2962ab8d 46d313581 -- chamados/models.py chamados/reconciliador.py chamados/services/fabrica_da_celula.py chamados/services/fio.py chamados/signals.py ponto/portas/celula.py`; cada bloco acima referencia os fontes reais.

Comando ORM principal e vizinhos diretamente relacionados às funções alteradas, para a sessão executar, sem execução aqui:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings_test \
chamados.tests.test_e4_expand_orm \
chamados.tests.test_e1_entradas \
chamados.tests.test_e1_volta2 \
chamados.tests.test_e1_v12 \
chamados.tests.test_e1_v13_cadeia \
chamados.tests.test_e1_v15 \
chamados.tests.test_e1_v15b \
chamados.tests.test_e1_v15d_um_ponto_de_escrita \
chamados.tests.test_e1_v15f \
chamados.tests.test_e1_v15g_portas \
chamados.tests.test_e1_v5_ordem_trava \
chamados.tests.test_e1_v5_portas_backfill \
chamados.tests.test_e1_v6_horizonte \
chamados.tests.test_e1_v9 \
chamados.tests.test_fabrica_pela_ata \
chamados.tests.test_fabrica_por_evento \
chamados.tests.test_f1c4_3_emissores \
chamados.tests.test_cobparc5_parcial_sem_periodo_so_lista \
chamados.tests.test_pergunta_do_dia_em_curso \
chamados.tests.test_o261_julgamento_desfaz_carimbo \
chamados.tests.test_jv4a_disparar_vigia_da_celula \
chamados.tests.test_c20_juiz3_outbox \
chamados.tests.test_jv3_reconciliar_vira_vigia \
chamados.tests.test_orfas_pelo_evento \
chamados.tests.test_orfas_nao_morre_na_guarda_da_porta \
chamados.tests.test_porta_reapontamento_pergunta \
chamados.tests.test_o261e_f1_codex_v5 \
chamados.tests.test_o261e_f1_porta_encerramento \
chamados.tests.test_reconciliar_pergunta_a_premissa \
chamados.tests.test_e2_celula_regenerada_reavalia \
ponto.tests.test_porta_celula_dia \
ponto.tests.test_hx_cartorio_ressuscita \
ponto.tests.test_d3_impressao_cobre_insumos \
ponto.tests.test_c3_ordem_dos_receivers_da_batida \
ponto.tests.test_lastro_parte2_fecho_no_evento \
ponto.tests.test_lastro_parte3_fecho_na_pergunta \
ponto.tests.test_o_outbox_celula \
ponto.tests.test_bug114_fechador_de_dois_dias \
ponto.tests.test_bug115_lampada_do_marco \
ponto.tests.test_orfas_vigia
```

Evidência de vizinhança: imports da fábrica em `chamados/tests/test_fabrica_por_evento.py:35`, `test_f1c4_3_emissores.py:76`, `test_o261_julgamento_desfaz_carimbo.py:82`; consumidor em `test_c20_juiz3_outbox.py:241`, `test_jv3_reconciliar_vira_vigia.py:141`, `ponto/tests/test_o_outbox_celula.py:165`; fio em `chamados/tests/test_porta_reapontamento_pergunta.py:271`; signal em `chamados/tests/test_reconciliar_pergunta_a_premissa.py:269` e `ponto/tests/test_d3_impressao_cobre_insumos.py:369`; reversão em `ponto/tests/test_porta_celula_dia.py:184`; ressuscitação em `chamados/tests/test_e1_v13_cadeia.py:140`.

Todos os resultados ORM permanecem `[nao verificado]`.

## 09/11 17:58 · sessao 323aebf8a713 · FIO-E0-V4-E4-ESCRITORES · construtor · origem=Code

Entrega: [E4A_V2_ENTREGA.txt](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_V2_ENTREGA.txt:1).

- 35 testes sem banco passaram; oito mutantes morderam e foram restaurados. [Evidências](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_V2_TESTES.txt:29). [nao verificado]
- ORM não executado; comando preparado para a sessão. [Referência](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_V2_ENTREGA.txt:175).
- O pouso ainda depende dos hunks e das decisões explícitas sobre A05 e C11. [Pendências](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_V2_ENTREGA.txt:222). [nao verificado]

## 09/11 17:42 · sessao c84017ed8f71 · - · - · origem=Ronald (tmux)

C11 por ato:

| Ato | Evento demonstrável |
|---|---|
| Abertura `batida_ausente` | `abrir`: `chamados/models.py:977–980`; `criar`: `:1093–1099`. Ambos inserem evento no atomic do nascimento. |
| Reabrir chamado/transição para VIVOS | `chamados/signals.py:194–203`, antes do best-effort/cartório. `_gravar_estado` salva dentro de atomic em `chamados/models.py:280–296`. |
| Retratar duplicata | `chamados/services/fio.py:1354` → `carimbar_identidade` → save; receiver de pergunta em `chamados/signals.py:237–245`; save atômico em `chamados/models.py:3402–3413`. |
| Validação | `chamados/services/validacao.py:136–139` salva e registra DP no mesmo atomic; evento pelo receiver acima. |
| Morte pela leitura da célula | `ponto/portas/celula.py:894–902`: célula e evento no mesmo atomic. |
| Reapontar pela porta fio | Captura origem em `chamados/services/fio.py:924`; salva em `:927`; acorda origem em `:929`, no atomic aberto em `:881`. |

Lacunas adicionais:

1. **Tranca de competência:** C6 morre por leitura da tranca em `chamados/juizes.py:1325–1328`; `trancar_periodo` não emite evento nem possui atomic próprio (`ponto/services/fechamento.py:1400–1430`). Construção permitida sugerida: atomic envolvendo criação/retranca e evento por `CelulaDia` da empresa dentro de `janela_fechamento(mes, ano, empresa_id)`. Essa janela aceita ID e consulta o corte canônico (`ponto/janelas.py:36–57`). Manter `disparar_no_ato` fora do atomic: pode abrir subprocesso (`chamados/services/competencia_trancada.py:290–298`). Segunda chamada com `ato=False` não emite novamente (`ponto/services/fechamento.py:1411`).

2. **Reuso em `_nascer`:** muda célula/destino e salva, sem acordar origem (`chamados/models.py:2955–2998`). Hunk textual: capturar `cls.linhas_do_evento(cls.objects.filter(pk=ex.pk))` antes de `upd=[]`; após `ex.save(update_fields=upd)`, chamar `cls.acordar_celulas(linhas_antes)` no atomic existente (`chamados/models.py:2839`).

3. **Reversão de regeneração:** save cru em `ponto/portas/celula.py:794–795`; termina em `:808`, sem emissão e sem `_o_chao_do_dia_mudou`. Edição chama esse rabo em `:627`; regeneração em `:354`. Hunk mínimo textual: `emitir_evento_celula(cel)` depois desse save, ainda no atomic de `:785`. Entretanto, o INSERT sozinho não relavra ATA/veredito: fábrica lê os motivos da célula em `chamados/services/fabrica_da_celula.py:170`. Não afirmar cobertura completa da reversão com esse hunk isolado.

PERGUNTAS para sessão:

- Aceita-se que o erro de outbox impeça a transição para VIVOS? O caminho alterado é o `post_save` de **qualquer save existente com mudança de status para VIVOS**, módulo de turno e célula (`chamados/signals.py:194–203`). Pela porta `_gravar_estado`, participa do mesmo atomic (`chamados/models.py:280–296`). Saves diretos não ganham atomic genérico: `ChamadoColaborador` não contém wrapper equivalente ao de PerguntaDisputa (`chamados/models.py:3402–3413`).
- Se construir tranca+evento atomicamente, falha da outbox passa a desfazer a tranca. O comentário atual promete que a tranca nunca cai por causa da fila (`ponto/services/fechamento.py:1423`). Não resolver silenciosamente essa mudança.

H2 continua aplicável à implementação atual: `registrar` só executa `bulk_create` (`chamados/models.py:3932–3934`); o texto acrescenta atomic+outbox (`docs/E4A_HUNKS_SESSAO.txt:66`). H4 corresponde às três mudanças necessárias: remover `--apply` do ENC (`config/crons.py:1521`), retirar nome da lista de juízes (`:2048`) e declarar papel vigia (`:2164–2169`). Sem H4 aplicado, cron chama opção ausente no comando desta cópia (`docs/E4A_HUNKS_SESSAO.txt:123`).

## 09/11 17:42 · sessao 760a6e6cb3db · - · - · origem=Ronald (tmux)

Mapa de provas existentes, sem executar ORM:

| Porta | Prova aproveitável | Lacuna para selo EXPAND |
|---|---|---|
| A01 | Estado canônico + trilha: `chamados/tests/test_trilha_na_ordem_da_escrita.py:67`; porta válida: `chamados/tests/test_selo_e4_gates.py:59` | Comparar toda projeção das perguntas antes/depois, sem exigir DP para efeito Chamado. |
| A02 | SILENCIO com autor: `chamados/tests/test_e1_entradas.py:121` | Exigir F/C preservados, instante correspondente e 2x sem novo DP. |
| A04 | Exercício indireto na devolução: `chamados/tests/test_o261_recusa_fala.py:164` | Caso direto `reabrir_sistema`, F completo e 2x com uma nota; não possui DP próprio. |
| A05 efeito puro | Carimbo aceito: `chamados/tests/test_o261e_f2_porta_carimbo.py:676`; recusa restaura memória/banco: `:713` | Caracterizar C completo com `resposta=None`; assinatura segue exceção conflitante do pedido. |
| A07 | Três campos C limpos juntos: `chamados/tests/test_o261e_f2_porta_carimbo.py:754` | Exigir R/V intactos e nenhum DP inventado pelo efeito técnico. |
| A08 | Exercício por correção com absorção: `chamados/tests/test_e1_volta2.py:200`; 2x = 1 DP: `:217` | Caso direto do lote, incluindo `outros` e C/R/V completo. |
| A09 | 2x = mesmo estado e zero query na segunda: `chamados/tests/test_porta_chamado_e3.py:70` | Exigir demais projeções intactas e nenhum DP humano fabricado. |
| A10 | DP VALIDACAO com autor e mesmo instante de V: `chamados/tests/test_e1_entradas.py:94` | Asserção explícita da projeção completa. `_carimbar_validacao` não declara idempotência própria. |
| A11 | Conserva resposta/recusa, limpa V e reabre F: `chamados/tests/test_o261_recusa_fala.py:164`; conserva V legada em DP: `chamados/tests/test_e1_volta2.py:101` | Exigir exatamente um novo DP DEVOLUCAO e repetir a porta. |
| A12 | Cada resposta é uma linha; anterior imutável; mesmo instante R/DP: `chamados/tests/test_e1_entradas.py:64`; invalida V: `chamados/tests/test_porta_resposta_pergunta.py:224` | Comparar toda projeção. Repetição não é idempotente: a própria prova caracteriza duas trilhas em `test_porta_resposta_pergunta.py:216`. |
| A13 | Sem dados não fabrica DP: `chamados/tests/test_e1_entradas.py:115`; foto da absorção e 2x: `chamados/tests/test_e1_volta2.py:200`, `:217` | Reabertura ordinária com foto R/V, F intacto e exatamente 1 DP. |
| A14 | 2x sem query; voz não sobrescrita: `chamados/tests/test_porta_chamado_e3.py:43`, `:50` | Exigir 1 DP RESPOSTA, instante injetado e R completo. |
| A15 | Testemunha preservada e 2x sem mudar instante/trilha: `chamados/tests/test_porta_resposta_pergunta.py:241` | Exigir exatamente 1 DP RESPOSTA com texto/instante testemunhados. |
| A16 | Via e L no mesmo save: `chamados/tests/test_porta_veredito_pergunta.py:343`; 2x = uma trilha: `:372` | Exigir R/V/C restante preservado e nenhum DP de decisão inventado. |
| A19 | Uma RECUSA, autor/motivo e texto retirado: `chamados/tests/test_e1_entradas.py:103` | Exigir motivo projetado e R/V zerados; 2x = mesma projeção e um DP RECUSA. |
| A21 | RETRATACAO, via terminal, resposta preservada: `chamados/tests/test_e1_entradas.py:142` | Repetir remoção e comparar motivos/contexto/projeção e quantidade de DP. |
| A22 | 2x mesmo fato = uma pergunta e uma trilha: `chamados/tests/test_porta_pergunta_identidade.py:167`; reuso sem escrita: `chamados/tests/test_e1_v15.py:44` | Nascimento/defaults R/V/C e reuso conservando DP/projeções existentes. |
| A23 | Limpa resposta e conta: `chamados/tests/test_porta_disputa_ciclo.py:283`; 2x não conta/trilha de novo: `:297` | Exigir exatamente um DP REABERTURA com autor/foto, além do contador/projeção já provados. |

A fixture mais simples para um novo módulo ORM é `_Base` de `chamados/tests/test_e1_entradas.py:30`: cria Empresa/Colaborador/User; `_pergunta` nasce pela porta em `:37`; `_responder` executa o core e recarrega em `:44`. O relógio desse módulo é fixado por `RELOGIO` em `:26` e `freeze_time` em `:51`. Para efeitos que dependem de chamado/célula e lavra, a fixture de `chamados/tests/test_porta_veredito_pergunta.py:223` já prepara esse universo.

Não recomendo afirmar “1 DP” para A01/A04/A05 puro/A07/A08 direto/A09/A16/A22: o vocabulário atual representa decisões humanas ou atos definidos, enquanto esses corpos executam efeitos/identidade. O requisito permite “DP ou efeito canônico”; adicionar RESPOSTA/REABERTURA a efeitos técnicos alteraria significado. (`chamados/models.py:3846`; `chamados/services/relavra_via.py:106`; `chamados/models.py:3192`; `chamados/models.py:2822`)

Resultados dessas provas no banco desta cópia: **[nao verificado]**.

## 09/11 17:42 · sessao f9abd31ce653 · - · - · origem=Ronald (tmux)

Leitura concluída, sem escrita.

| Porta | Evidência / classificação |
|---|---|
| A01 `_gravar_estado` | Efeito canônico Chamado + trilha no mesmo atomic: `chamados/models.py:280`, `:292`, `:296`. Não corresponde a um ato DP por pergunta. |
| A02 `fechar` | Exceções já gravam `SILENCIO`: `chamados/models.py:1921`; F persiste no mesmo atomic: `:1934`. Fechamento normal conserva efeito/quitação, sem inventar decisão. |
| A04 `reabrir_sistema` | Efeito no continente e nota existente: `chamados/models.py:2174`, `:2177`. `REABERTURA` disponível significa correção da pergunta, não reabertura do continente: `:3853`. |
| A05 `carimbar`, ramo puro | Efeito/carimbo e L: `chamados/models.py:3118`, `:3140`, `:3141`. |
| A05 assinatura | **Lacuna confirmada**: atribui R/V em `chamados/models.py:3121`, `:3124`; salva sem DP em `:3141`. |
| A07 `descarimbar` | Limpeza do carimbo: `chamados/models.py:3197`, `:3201`; caller de lastro executa consequência do juiz: `chamados/services/relavra_via.py:242`, `:246`. |
| A08 `descarimbar_em_lote` | Executor de limpeza: `chamados/models.py:3253`; correção registra DP antes do lote: `chamados/services/disputa_emissao.py:1612`, `:1627`. Ressuscitação conserva decisões antigas antes da limpeza: `chamados/models.py:1563`, `:1565`. |
| A09 `encerrar_por_fato_no_espelho` | Efeito já existente do espelho, via idempotente: `chamados/models.py:3262`, `:3264`, `:3265`. |
| A10 `_carimbar_validacao` | Já grava `VALIDACAO` no atomic do save: `chamados/services/validacao.py:136`, `:139`. |
| A11 `devolver_validacao_recusada` | Já grava `DEVOLUCAO`: `chamados/services/validacao.py:201`, `:205`; segunda chamada não registra novamente quando V já nulo: `:195`. |
| A12 resposta core | Já grava `RESPOSTA` no atomic: `chamados/services/disputa_emissao.py:1500`, `:1505`, `:1508`. Advertência permanece em `recusa_motivo`: `:1494`. |
| A13 correção | Já grava `REABERTURA`/`RECUSA` antes da limpeza: `chamados/services/disputa_emissao.py:1602`, `:1612`, `:1624`, `:1629`. |
| A14 automática | Já grava `RESPOSTA`: `chamados/services/disputa_emissao.py:1651`, `:1653`; guarda do texto mantém idempotência existente: `:1644`. |
| A15 trilha | Já grava `RESPOSTA` no instante testemunhado: `chamados/services/disputa_emissao.py:1671`, `:1674`; guarda idempotente: `:1667`. |
| A16 relavra | Efeito derivado + L pelo juiz existente: `chamados/services/relavra_via.py:120`, `:125`, `:127`. Não representa nova decisão humana. |
| A19 recusa | Já delega DP `RECUSA` dentro do atomic da projeção: `chamados/services/validacao.py:588`, `:590`, `:593`; executor DP em `chamados/services/disputa_emissao.py:1612`. |
| A21 remover motivo | Já grava `RETRATACAO` e carimbo no atomic: `chamados/models.py:2023`, `:2053`, `:2059`. |
| A22 nascer/reuso | Reuso altera identidade/costura/L, sem nova escrita R/V/C: `chamados/models.py:2955`, `:2997`, `:2998`; nascimento aceita `campos` e persiste em `:3005`, `:3008`. |
| A23 colaborador | Já delega correção com DP dentro do atomic do contador: `chamados/models.py:2232`, `:2234`, `:2236`; executor DP em `chamados/services/disputa_emissao.py:1612`. |

A contradição precisa constar em PERGUNTAS: o pedido exclui expressamente A05 **ramo de assinatura**, mas manda começar pelo RED que exige DP precisamente nesse ramo. A falha é real (`chamados/models.py:3121`, `:3124`, `:3141`); qual instrução prevalece é **[nao verificado]**.

Se autorizado pela sessão, o hunk mínimo de A05 deve acumular `RESPOSTA` somente ao preencher R ausente, `VALIDACAO` somente ao preencher V ausente, com texto/instante/autor das atribuições existentes; depois envolver `save` + `DecisaoPergunta.registrar` no mesmo atomic dentro do `try` atual. O rollback em memória existente precisa continuar cobrindo falha do registrar (`chamados/models.py:3137`, `:3143`, `:3144`). Não inserir DP no ramo puro nem repetir assinatura já presente (`:3120`, `:3123`).

`registrar` não deduplica: faz apenas `bulk_create`, então a precondição deve continuar na porta (`chamados/models.py:3934`).

O vocabulário disponível é fechado em oito atos (`chamados/models.py:3846`, `:3847`); classificar indiscriminadamente limpeza técnica, relavra, nascimento ou reabertura de continente como `REABERTURA` alteraria o significado documentado em `:3853`.

Não encontrei `Parallel Change`/`Expand-Contract` nos dois catálogos consultados; resultado da busca local, não confirmação universal **[nao verificado]**. Termos efetivamente encontrados: `Single Writer Principle` em `docs/GLOSSARIO.md:12` e `Architecture Conformance Test` em `docs/GLOSSARIO.md:15`.

## 09/11 17:35 · sessao 98f00f1fa6ec · FIO-E0-V4-E4-ESCRITORES · construtor · origem=Code

**E4a não está pronta:** o RED final mantém 9 falhas, incluindo identidade, fatos ausentes e registro C20. [Resultado](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_TESTES.txt:784).

- [Entrega: diff, escritores/leitores, lacunas e pendências](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_ENTREGA.txt:1).
- [Hunks textuais para a sessão](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_HUNKS_SESSAO.txt:1).
- [Provas: 24 testes verdes e comandos dos 18 mutantes que falharam e foram restaurados](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_TESTES.txt:1). [nao verificado]

Sem commit, pela proibição de escrever em `.git*`. [Registro](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_ENTREGA.txt:292). [nao verificado]

## 09/11 17:13 · sessao af1c0c27e636 · - · - · origem=Ronald (tmux)

C20 precisa destes quatro movimentos coordenados:

1. O comando vira vigia puro: retirar `_rodar` e seus blocos de escrita, hoje em [disparar_perguntas_competencia.py:266](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/management/commands/disparar_perguntas_competencia.py:266), e fazer a leitura usar `motivos_que_o_evento_deixou`, cuja composição já reúne os gates da fábrica e a Pauta do limbo. [fabrica_da_celula.py:201](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:201)

2. O consumidor passa a chamar `perguntar_pela_celula` depois do lastro. O `lote` já oferece as datas; carregar as células atuais por colaborador e `data__in={e.data for e in lote}` evita depender do identificador da célula anterior à regeneração. Reutilizar `agora` da rodada e manter as exceções dentro do bloco que deixa eventos pendentes. [reconciliador.py:1337](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1337), [reconciliador.py:1348](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1348), [reconciliador.py:1383](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1383)

3. As fontes precisam permanecer completas após cessar os saves das projeções: retorno do chamado a VIVOS, registro de DP e reapontamento com origem e destino devem emitir o evento no mesmo atomic. Hoje o signal de pergunta depende do save e observa somente a situação posterior; `_gravar_estado` não emite explicitamente o evento. [signals.py:237](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/signals.py:237), [models.py:294](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/models.py:294), [fio.py:922](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fio.py:922)

4. Passado sem marcos já tem corte: não pergunta ao colaborador; cria pendência CADASTRO, uma por colaborador, com contador. O emissor antigo ainda retorna `None` por comparação de datas e alcança `classificar_falta`. [PROMPTS.md:2004](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/PROMPTS.md:2004), [fabrica_da_celula.py:29](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:29), [fabrica_da_celula.py:55](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:55)

A Pauta cabe pela porta existente `pautas.services.escrever`, usando `ANCORA_COLAB`, destinatário DP e marcador estável. Sob atomic e trava do colaborador, consultar toda a história das cabeças e escrever somente se nenhuma existir; o contador deve contar colaboradores, não dias. Essa implementação é proposta, ainda [nao verificado]. A porta e a âncora existem em [services.py:152](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/pautas/services.py:152) e [models.py:30](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/pautas/models.py:30).

O risco de loop vem do consumidor reler eventos novos na mesma execução. A fábrica deve escrever apenas diante de motivos efetivamente ausentes; sua seleção já exclui identidades existentes, e o nascimento da pergunta não aciona o outbox. Uma segunda passagem deve resultar em `motivos=[]`, sem save nem relavra. [reconciliador.py:1343](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1343), [fabrica_da_celula.py:124](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:124), [signals.py:239](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/signals.py:239)

Hunk proibido necessário em `config/crons.py`:

```diff
@@ ENC('disparar_perguntas_competencia', ...)
-    ENC('disparar_perguntas_competencia', gatilho='apurar_furos_diarios', args='--comp-corrente --apply',
+    ENC('disparar_perguntas_competencia', gatilho='apurar_furos_diarios', args='--comp-corrente --alarme',
@@ JUIZES_POR_VARREDURA = (
-    # 'disparar_perguntas_competencia' FICA, e so pelo FIO MUDO: no JUIZ-VARREDURA-4 parte A o caminho da CELULA virou
-    # VIGIA (conta o que o evento `fabrica_da_celula.perguntar_pela_celula` deixou, esperado 0, SystemExit(2)); o fio
-    # mudo (chamado vivo sem pergunta viva) segue ESCREVENDO -- nao ha evento dele e a lei esta aberta (CORTES.md,
-    # PAPEL-PRAZO-DISPARAR-PERGUNTAS 27/10).
-    'disparar_perguntas_competencia',
@@ PAPEL_DO_CRON = dict(
     **{n: 'vigia' for n in (
+                            'disparar_perguntas_competencia',
```

Âncoras reais: [crons.py:1521](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:1521), [crons.py:2044](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:2044), [crons.py:2169](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:2169). Não encontrei registro desse comando em `core/juizes.py`; alteração ali para C20: [nao verificado].

A condição de saída permanece literalmente “fio mudo medido + C11 pousado”. Esta exploração não comprova nenhuma dessas duas condições, nem `20/20`; o contrato estrutural consulta as exceções reais e não deve ganhar `verde=True` artificial. [CORTES.md:305](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/CORTES.md:305), [contratos_estruturais.py:264](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/core/contratos_estruturais.py:264) [nao verificado]

## 09/11 17:13 · sessao e00ecb618eb2 · - · - · origem=Ronald (tmux)

Leitura concluída sem editar arquivos. O corte completo de identidade e continente permanece dependente de representação factual ausente; hunks apenas de leitura R/V podem ser aplicados separadamente. (`chamados/models.py:2752`, `chamados/models.py:3474`, `chamados/models.py:3480`, `chamados/models.py:3860`)

**Hunk de apresentação**, arquivo `chamados/models.py`, âncora imediatamente anterior a `PerguntaDisputa.estado` (`chamados/models.py:3022`):

```diff
@@
-    @property
-    def estado(self):
+    @property
+    def atos(self):
+        """Leitura mecanica da sequencia; nao julga tarefa nem efeito."""
+        from chamados.services.atos_pergunta import leitura
+        return leitura(self)
+
+    @property
+    def resposta_do_ato(self):
+        return self.atos.resposta
+
+    @property
+    def respondida_no_ato(self):
+        return self.atos.respondida_em
+
+    @property
+    def validada_no_ato(self):
+        return self.atos.validada_em
+
+    @property
+    def validada_pelo_ato(self):
+        return self.atos.validada_por
+
+    @property
+    def recusa_humana_do_ato(self):
+        return self.atos.recusa_humana
+
+    @property
+    def estado(self):
```

Esse bloco não corrige `estado`: a propriedade ainda contém julgamento por V/C/R e precisa ser substituída por apresentação da autoridade da tarefa. Não propor atribuições aos campos antigos para alimentar o C6: isso perpetuaria a projeção em memória. (`chamados/models.py:3030`, `chamados/models.py:3034`, `chamados/models.py:3035`; `chamados/juizes.py:1323`, `chamados/juizes.py:1339`)

**Hunk de presença da resposta**, arquivo `chamados/models.py`, âncora `PerguntaDisputa.respondida` (`chamados/models.py:3486`):

```diff
@@
     @property
     def respondida(self):
-        return self.respondida_em is not None
+        return self.atos.respondida_em is not None
```

**Hunk de conteúdo discriminado**, arquivo `chamados/models.py`, âncora `PerguntaDisputa.resposta_display` (`chamados/models.py:3508`):

```diff
@@
     def resposta_display(self):
         # S80 - resolve ID para nome legivel quando tipo_resposta e dropdown
-        if not self.resposta_colab:
+        resposta = self.atos.resposta
+        if not resposta:
             return ''
         if self.tipo_resposta == 'posto_dropdown':
             try:
                 from colaboradores.models import Posto
-                p = Posto.objects.filter(pk=int(self.resposta_colab)).first()
+                p = Posto.objects.filter(pk=int(resposta)).first()
@@
-                logger.warning('resposta_display: pk de posto invalido (%r)', self.resposta_colab)
+                logger.warning('resposta_display: pk de posto invalido (%r)', resposta)
@@
-                e = EscalaColaborador.objects.filter(pk=int(self.resposta_colab)).select_related('tipo_escala').first()
+                e = EscalaColaborador.objects.filter(pk=int(resposta)).select_related('tipo_escala').first()
@@
-                logger.warning('resposta_display: pk de escala invalido (%r)', self.resposta_colab)
+                logger.warning('resposta_display: pk de escala invalido (%r)', resposta)
@@
-                if _o.get('valor') == self.resposta_colab:
-                    return _o.get('rotulo', self.resposta_colab)
-        return self.resposta_colab
+                if _o.get('valor') == resposta:
+                    return _o.get('rotulo', resposta)
+        return resposta
```

**Falta fato — A03.** Falta o ato de reabertura do continente, com disputa, autor, instante, motivo e identidade da operação. `REABERTURA` atual representa correção da resposta; DP exige uma pergunta, portanto uma disputa vazia não pode receber esse ato sem inventar pergunta. Acrescentar uma opção em `ATOS` sozinho não resolve. (`chamados/models.py:2123`, `chamados/models.py:2193`, `chamados/models.py:3860`)

Proposta textual **somente para decisão da sessão**, arquivo `chamados/models.py`, âncora `DecisaoPergunta`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    REABRIR_DISPUTA = 'reabrir_disputa'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (REABRIR_DISPUTA, 'Continente reaberto pelo admin'),
```

Para esse ato ser utilizável em continente vazio ainda falta decidir o sujeito factual: tornar `pergunta` opcional e acrescentar `disputa`, com constraint “exatamente um sujeito”, implicaria schema. Não construir essa mudança silenciosamente nesta E4a. (`chamados/models.py:3860`, `chamados/models.py:3877`)

**Falta fato — A17/A18.** Faltam resultado técnico, código, motivo completo, pergunta, instante e autor operacional. Pendência nasce em campos; recusa técnica nasce em campos e nota do chamado. Nenhuma é DP `RECUSA` humana. (`chamados/services/materializacao.py:850`, `chamados/services/materializacao.py:1089`, `chamados/services/materializacao.py:1107`)

Proposta textual, arquivo `chamados/models.py`, âncora `DecisaoPergunta.ATOS`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    PENDENCIA_TECNICA, RECUSA_TECNICA = 'pendencia', 'recusa_tecnica'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (PENDENCIA_TECNICA, 'Materializacao exige atendimento tecnico'),
+        (RECUSA_TECNICA, 'Materializacao recusada tecnicamente'),
```

Só o enum não preserva todo o resultado técnico: `ato_motivo` admite 240 caracteres e `ato_texto` está documentado como resposta. A sessão precisa decidir representação dos dados técnicos; reutilizar texto da resposta para payload quebraria o significado declarado. (`chamados/models.py:3837`, `chamados/models.py:3864`, `chamados/models.py:3865`)

**Falta fato — A20.** Faltam fato durável da conferência, par dos dias, autor, instante e justificativa integral. O log não garante existência: `registrar_log` captura qualquer exceção. (`chamados/services/validacao.py:664`, `chamados/services/validacao.py:667`, `core/auditoria.py:17`, `core/auditoria.py:25`)

Proposta textual, arquivo `chamados/models.py`, âncora `DecisaoPergunta.ATOS`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    CONFERENCIA_DATA = 'conferencia_data'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (CONFERENCIA_DATA, 'Par de dias conferido pelo admin'),
```

A opção tem 16 caracteres e cabe no campo atual, mas não dá campos próprios ao par e à justificativa. Proposta adicional para a sessão decidir, sem aplicação: `ato_dados = models.JSONField(default=dict, blank=True)`, com validação obrigatória específica por ato; isso exige schema e não é hunk executável isoladamente nesta entrega. (`chamados/models.py:3861`, `chamados/models.py:3865`)

**Identidade — impedimento material para N=0 seguro.** As duas constraints parciais excluem linhas através de `via_resolucao`; a porta de retratação escreve essa via e a duplicata nem possui DP correspondente. Cessar a via deixa a linha retratada dentro do índice: um nascimento legítimo posterior colide com ela. Não remover a constraint, substituir por dedup apenas em Python ou fingir que subquery DP cabe na condição de índice. (`chamados/models.py:2053`, `chamados/models.py:2058`, `chamados/models.py:2752`, `chamados/models.py:3471`, `chamados/models.py:3477`)

O teste E5 deve permanecer RED enquanto não existir representação factual de terminalidade consultável pelo banco e tratamento das constraints pela sessão; retirar fisicamente campo não é necessário para demonstrar esse impedimento. (`chamados/models.py:3474`, `chamados/models.py:3480`)

**A23 — idempotência sob trava.** Além de substituir o filtro R em `perguntas_corrigiveis`, a sessão deve mover a seleção corrigível e leitura do contador para dentro do atomic, após trava C/D e releitura. Hoje a guarda e `usadas` são calculados antes da trava: duas chamadas concorrentes podem selecionar a mesma resposta antes de qualquer DP de correção. (`chamados/models.py:2220`, `chamados/models.py:2225`, `chamados/models.py:2232`, `chamados/models.py:2236`; `chamados/juizes.py:1592`)

**A05 — assinatura.** Não reduzir `carimbar` a “não salva”: os ramos `responder` e `validar` hoje completam assinaturas sem DP. O hunk deve registrar DP `RESPOSTA`/`VALIDACAO` no mesmo atomic e no instante `quando`, antes de produzir a consequência, conservando o gate da autoridade e a lavra L. Os testes E4/E9 precisam morder perda desse ato e troca de `quando` pelo relógio da reparação. (`chamados/models.py:3113`, `chamados/models.py:3119`, `chamados/models.py:3123`, `chamados/models.py:3138`)

Não produzi hunk integral de A01–A09/A21–A23 que alegasse fechar o corte: A03 e identidade acima impedem um patch seguro e aplicável isoladamente sem decisão de representação da sessão. [nao verificado]

## 09/11 17:12 · sessao 02db88593fc2 · - · - · origem=Ronald (tmux)

Exploração somente leitura concluída; nenhuma edição.

- Vizinho executado: `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e3_pendentes_app` → rc 0, `Ran 6 tests`, `OK`. O módulo usa `unittest`, AST e stubs de módulos; o relógio é explícito. (`chamados/tests/test_e3_pendentes_app.py:2`, `:16`, `:24`, `:118`)

- Infra segura para novos selos: reproduzir a extração AST de funções produtivas sem importar Django ou settings. `config/settings/ci.py` cria diretórios temporários fora de `app/` e configura PostgreSQL; portanto sua importação viola as restrições desta volta. (`config/settings/ci.py:6`, `:25`, `:29`, `:33`)

- E1/escritores: o detector existente usa contaminação por ponto fixo para atingir atributos, `setattr`, construtor com alias, dicionários/defaults, uniões e `bulk_update`. Sua varredura inclui `models.py`. Porém não resolve alias de métodos, como `gravar = qs.update`, e declara não alcançar `__dict__`; ampliar para o contrato novo. (`chamados/tests/test_o261e_f2_porta_carimbo.py:109`, `:133`, `:173`, `:192`, `:204`, `:294`)

- Resolução de alias aproveitável: `c17_callers` resolve imports de símbolo/módulo, atribuições locais, escopo lexical e `getattr`. Há contraprovas sobre `models.py` e registro. (`chamados/tests/test_e3_pendentes_app.py:33`, `:64`, `:75`, `:124`, `:149`)

- E9 puro: extrair `_hoje`, `relogio_virou` e `esta_stale`; fornecer timezone com `ZoneInfo('America/Sao_Paulo')` e pergunta `SimpleNamespace`. Casos relevantes matam `<= → <`, `> → >=`, data UTC em lugar da local, lavra ausente e lavra futura. (`chamados/veredito_lavrado.py:102`, `:107`, `:116`, `:213`, `:224`, `:232`)

- E2 existente depende de Django e `TestCase` com banco. Seus casos de referência para bordas são meia-noite local, instante futuro, relógio envenenado e horizonte de decisões. Escrever novos casos puros no arquivo e executar apenas a classe extraída por AST evita inicializar a infraestrutura proibida. (`chamados/tests/test_e2_tarefa_do_dia.py:59`, `:71`, `:640`, `:657`, `:667`, `:714`)

- E17 render real existente depende de banco: a matriz usa views inteiras, `Client`, autenticação e os dois kernels. Não declarar render admin/app certificado com mera inspeção AST. (`chamados/tests/test_e3_app_orm.py:1`, `:12`, `:28`, `:33`, `:38`)

- Conferência: os testes antigos medem apenas timestamp/autor e idempotência; não provam validade para par de dias alterado. O log é explicitamente best effort e pode devolver `None`; não serve como substituto obrigatório do fato. (`chamados/tests/test_data_do_fato_divergente.py:185`, `chamados/tests/test_porta_validacao_pergunta.py:317`, `core/observ.py:98`, `:139`) Falta ato de conferência em `ATOS`. (`chamados/models.py:3846`)

- Padrões existentes adequados à entrega: `Single Writer Principle`, `Architecture Conformance Test`, `Immutable event log + compensating event`; a linha `padrao=` deve refletir o diff realizado. (`docs/GLOSSARIO.md:12`, `:15`, `:25`; `docs/PLANO-PADROES.md:49`, `:53`)

Recomendação para mutantes: executar a função produtiva extraída de fonte com alteração em memória, mostrar a falha e descartar a fonte mutada. Informar explicitamente essa modalidade; os testes de snippets sintéticos provam o detector, mas não substituem os mutantes comportamentais de produção. (`chamados/tests/test_e3_pendentes_app.py:24`, `:149`; equivalência dessa modalidade ao requisito operacional: [nao verificado])

## 09/11 17:07 · sessao 08f5868560fb · FIO-E0-V4-E4-ESCRITORES · leitura · origem=Code

**É (b): leitor a migrar para o fato antes de cessar a escrita legada.** O template consome campos cuja escritora já está no censo; não acrescenta porta de escrita. (`app/templates/chamados/partials/modal_fio.html:169`, `:170`; `app/chamados/services/validacao.py:664`, `:665`; anexo `censo_v2.r:407`)

| Item | Resultado e evidência |
|---|---|
| **1 — Template: instante** | Dentro do ramo de divergência dos dias, `if perg.data_conferida_em` escolhe mostrar “Conferida em”, formatada `d/m/Y H:i`; sem instante, oferece formulário conforme permissão ou informa bloqueio. (`app/templates/chamados/partials/modal_fio.html:162`, `:169`, `:170`, `:171`, `:179`) |
| **1 — Template: autor** | `if perg.data_conferida_por` decide mostrar autoria; `get_full_name\|default:perg.data_conferida_por.username` apresenta nome completo ou username. (`app/templates/chamados/partials/modal_fio.html:170`) |
| **1 — Contexto/apelidos** | O objeto percorre `p → perguntas_por_chamado → ch.perguntas_disputa → perg`; os campos chegam pelo próprio objeto. (`app/chamados/services/fio.py:225`, `:243`, `:253`; `app/templates/chamados/partials/modal_fio.html:157`) |
| **1 — Views/contextos** | Render completo e parcial chamam o mesmo montador e passam `chamados`; o parcial também fornece `pode_conferir_data` pela permissão `gerir_chamados`. A view de conferência delega autor/justificativa ao serviço. (`app/chamados/views.py:889`, `:927`, `:931`, `:1312`; `app/chamados/services/fio.py:446`, `:465`, `:474`) |
| **1 — Python** | Fora da escritora, `_materializar_uma` lê somente `data_conferida_em`: divergência com instante nulo bloqueia materialização. Dentro da escritora, instante não nulo impede repetir a conferência. (`app/chamados/services/materializacao.py:1193`, `:1199`; `app/chamados/services/validacao.py:662`, `:663`) |
| **1 — JS estático** | Não localizei leitor literal desses valores **[nao verificado]**. O formulário encontrado aciona a rota por HTMX, sem transportar os campos de conferência. (`app/templates/chamados/partials/modal_fio.html:172`, `:174`) |
| **1 — Leituras de teste** | Conferem presença/ausência, autoria por `data_conferida_por_id` e preservação do instante no segundo pedido; `em1` é o apelido local. (`app/chamados/tests/test_data_do_fato_divergente.py:191`, `:192`, `:203`, `:211`; `app/chamados/tests/test_porta_validacao_pergunta.py:292`, `:293`, `:306`, `:315`, `:322`, `:327`) |
| **2 — Escritores** | Porta semântica produtiva: `conferir_data_do_fato`, que atribui instante/autor e persiste ambos; `save` é executor. Há escrita direta de preparação nos testes. A FK ainda declara `SET_NULL` quando o usuário é excluído. (`app/chamados/services/validacao.py:664`, `:665`, `:666`; `app/chamados/models.py:3402`, `:3412`, `:2457`; `app/chamados/tests/test_data_do_fato_divergente.py:155`, `:166`) |
| **2 — Ato correspondente** | A porta emite **`LogAuditoria`: ação `conferir_data`, modelo `PerguntaDisputa`, objeto `p.pk`**, com dia anterior, dia válido e justificativa; a trilha recebe usuário e tem `criado_em` próprio. **Nenhuma DP de conferência** consta em `ATOS`. (`app/chamados/services/validacao.py:667`, `:668`, `:669`, `:670`, `:671`, `:672`; `app/core/auditoria.py:22`, `:24`; `app/core/models.py:295`, `:307`; `app/chamados/models.py:3848`) |
| **3 — E4** | **(b), sem aumentar 26 (+27 informado):** migrar a leitura do instante/autoria para o ato correspondente antes de abandonar suas escritas. O candidato factual existente é o log acima; sua gravação admite falha sem impedir o fluxo. Cobertura histórica por pergunta: **[nao verificado]**. Onde não houver registro equivalente, **falta fato**: conferência daquele par de dias, autor, instante e justificativa. (`app/templates/chamados/partials/modal_fio.html:169`, `:170`; `app/chamados/services/validacao.py:664`, `:667`; `app/core/observ.py:98`, `:139`) |

| 4 — Outros leitores de template ausentes do `censo_v2` fornecido | Ocorrências confirmadas |
|---|---|
| Modal: R/V/F, incluindo indiretos | `app/templates/chamados/partials/modal_fio.html:184`, `:216`, `:218`, `:219`, `:227`, `:378`; `aberta`: `:75`, `:325`; `leitura_da_resposta`: `:249`; `todas_respondidas`: `:333`. |
| Card: R/via/estado/F | `app/templates/chamados/partials/_card_atendimento.html:94`, `:100`, `:108`, `:135`, `:137`, `:142`, `:148`, `:155`, `:176`, `:177`, `:384`, `:385`; via `:186`; estado `:183`; aberta `:10`, `:15`; correção `:190`. |
| Card de pergunta: R/V | `app/templates/chamados/partials/_pergunta_card.html:20`, `:23`, `:24`, `:29`, `:31`, `:32`, `:42`, `:43`. |
| Preview: R | `app/templates/chamados/partials/_lote_preview.html:24`. |
| F por contexto/propriedade | `app/templates/chamados/fio_colaborador.html:164`, `:169`; `app/templates/chamados/detalhe_local.html:6`, `:296`. |
| Correspondência dos indiretos | `aberta → fechada_em`: `app/chamados/models.py:1821`; `estado → V/C/R`: `:3030`, `:3032`, `:3034`, `:3037`; filtro → R: `app/chamados/templatetags/chamados_tags.py:128`; flags → R/F: `app/chamados/services/fio.py:340`, `:341`, `:342`; contexto → F: `app/chamados/services/acoes_chamado.py:336`. |

Exaustividade por reflexão, SQL ou nomes montados dinamicamente: **[nao verificado]**.

## 09/11 17:06 · sessao a086c7213e12 · - · - · origem=Ronald (tmux)

| Template | Leitores adicionais ao `censo_v2` fornecido |
|---|---|
| `app/templates/chamados/partials/modal_fio.html` | R: `:184`, `:216`, `:218`, `:219`; V: `:227`; F: `:378`, e por `disputa_obj.aberta`: `:75`, `:325`; R indireto por `leitura_da_resposta`: `:249`; R indireto por `todas_respondidas`: `:333`. |
| `app/templates/chamados/partials/_card_atendimento.html` | R: `:94`, `:100`, `:108`, `:135`, `:137`, `:142`, `:148`, `:155`, `:176`, `:177`, `:384`, `:385`; via: `:186`; V/C/R por `p.estado`: `:183`; F por `disputa_obj.aberta`: `:10`, `:15`; R/F por `pode_solicitar_correcao`: `:190`. |
| `app/templates/chamados/partials/_pergunta_card.html` | R: `:24`, `:29`, `:31`, `:32`; V: `:20`, `:23`, `:42`; R indireto por `leitura_da_resposta`: `:43`. |
| `app/templates/chamados/partials/_lote_preview.html` | R: `:24`. |
| `app/templates/chamados/fio_colaborador.html` | F por `disputa_obj.aberta`: `:164`, `:169`. |
| `app/templates/chamados/detalhe_local.html` | F por contexto `disputa_aberta`: `:6`, `:296`; seleção por `fechada_em__isnull=True` em `app/chamados/services/acoes_chamado.py:336`. |

| Comprovação dos aliases | Fonte |
|---|---|
| `aberta` lê `fechada_em`. | `app/chamados/models.py:1821` |
| `estado` lê V/C/R. | `app/chamados/models.py:3030`, `:3032`, `:3034`, `:3035`, `:3037` |
| `resposta_display` lê `resposta_colab`. | `app/chamados/models.py:3510`, `:3515`, `:3523`, `:3530`, `:3532` |
| `leitura_da_resposta` lê R por `getattr`. | `app/chamados/templatetags/chamados_tags.py:128` |
| `todas_respondidas` deriva de `all(p.respondida_em)`; `pode_solicitar_correcao` combina isso com `d.aberta`. | `app/chamados/services/fio.py:340`, `:341`, `:342` |

Correção da mensagem anterior: não há ocorrência de `pode_solicitar_correcao` no modal; a ocorrência encontrada é apenas `app/templates/chamados/partials/_card_atendimento.html:190`.

## 09/11 17:06 · sessao d2ea68ef7256 · - · - · origem=Ronald (tmux)

| Caminho | Evidência e uso |
|---|---|
| Transporte para template | `montar_chamados_fio` busca objetos `PerguntaDisputa` como `p`; agrupa em `perguntas_por_chamado`, anexa em `ch.perguntas_disputa`; template percorre como `perg`. Logo, `p → ch.perguntas_disputa → perg` é o apelido relevante; não há cópia separada dos dois campos. (`app/chamados/services/fio.py:225`, `:243`, `:253`; `app/templates/chamados/partials/modal_fio.html:157`) |
| Seleção/preload | Perguntas das disputas abertas; nas fechadas aplica `q_vivas` e depois `pergunta_viva`. `select_related` inclui disputa/célula/destino; prefetch inclui decisões com `validada_original_por`/`ato_por`; não inclui `data_conferida_por`. (`app/chamados/services/fio.py:221`, `:224`, `:225`, `:226`, `:227`, `:231`, `:238`) |
| Contexto completo | `_resp_modal_full` chama `montar_chamados_fio(..., com_explicacao=True)` e passa `'chamados': chamados` para `modal_fio.html`. Não calcula outro valor de conferência. (`app/chamados/views.py:889`, `:927`, `:931`) |
| Contexto parcial | `contexto_parcial_fio` chama o mesmo montador, passa `chamados` e define `pode_conferir_data` pela ação `gerir_chamados`. Consumido no re-render pós-ação de `processar_post_fio` e na reabertura de disputa. (`app/chamados/services/fio.py:446`, `:465`, `:474`, `:496`, `:497`; `app/chamados/views.py:1399`, `:1400`) |
| Leitura do par no modal | Dentro do ramo de divergência entre dias, `if perg.data_conferida_em` decide exibir conferência em vez do formulário/bloqueio. Mostra timestamp `d/m/Y H:i`; `if perg.data_conferida_por` controla a autoria; usa `get_full_name|default:...username`. Sem timestamp, oferece formulário se `pode_conferir_data`, senão informa bloqueio. (`app/templates/chamados/partials/modal_fio.html:162`, `:169`, `:170`, `:171`, `:172`, `:179`) |
| Casca HTTP | `conferir_data_pergunta` valida método/permissão e delega ao serviço com autor e justificativa; não lê os campos. (`app/chamados/views.py:1308`, `:1310`, `:1312`, `:1313`) |
| JS | Busca literal por `data_conferida`/`conferir_data` em `app/**/*.js` não encontrou ocorrência; cobertura dinâmica: **[nao verificado]**. O acionamento concreto está no atributo HTMX do template, `hx-post` para `conferir_data_pergunta`, `hx-swap="none"`. (`app/templates/chamados/partials/modal_fio.html:172`) |

Detalhe potencialmente útil: o contexto **completo** não fornece `pode_conferir_data`; o parcial fornece. Portanto a diferença comprovável é ausência/presença dessa chave nos dois construtores, sem inferir resultado produtivo. (`app/chamados/views.py:927`; `app/chamados/services/fio.py:474`; comportamento produtivo: **[nao verificado]**)

## 09/11 16:29 · sessao f6ee62474778 · FIO-E0-V4-E4-ESCRITORES · leitura · origem=Code

**O pré-desenho precisa incluir a troca dos leitores junto dos escritores.** Nesta cópia, a consulta central do app ainda depende de R/V/C/F; a tarefa ainda depende da via atual e consulta R/V para reconhecer passivo sem decisão. Cortar apenas as escritas deixaria consumidores usando projeções abandonadas. (`app/chamados/services/pergunta_no_app.py:48`, `:57`, `:70`; `app/chamados/services/tarefa_do_dia.py:227`, `:346`)

**Alvo declarado: N=0 portas de escrita de estado derivado da pergunta**, conservando decisões imutáveis, efeitos canônicos e L como cache declarado. Isso é alvo do desenho, ainda não resultado certificado. A própria definição de `DecisaoPergunta` declara transitórias as projeções até a E4. (`app/chamados/models.py:3832`; resultado alcançado: **[nao verificado]**)

Não cabe prometer uma única construção Codex: portas e leitores em `models.py`, migrations e eventuais ajustes em `bin/` são proibidos ao construtor. A quantidade de cópias/voltas necessária é **[nao verificado]**. (`bin/codex_pergunta.sh:108`, `:112`, `:113`, `:115`, `:330`)

Para compactar os endereços abaixo:

- `models.py` significa `app/chamados/models.py`.
- Arquivos de serviços sem prefixo significam `app/chamados/services/<arquivo>`.
- `juizes.py` significa `app/chamados/juizes.py`.
- Os demais caminhos aparecem completos.

Os números produtivos abaixo são exclusivamente os do anexo fornecido; sua reprodução por banco ou arquivo local é **[nao verificado]**. A identidade criptográfica independente da cópia também é **[nao verificado]**.

**A. MATRIZ DAS PORTAS**

R = resposta/texto/instante; V = validação/autor; C = materialização/autor/via; F = fechamento/autor/resolução/motivo; L = veredito/julgamento/expiração. Esses grupos não têm significado uniforme de “aconteceu”: isso está discriminado em B. (`models.py:2444`, `:3023`, `:3092`, `:3366`; `app/chamados/veredito_lavrado.py:213`)

As referências **H-R, H-V, H-C, H-F, H-S, H-L** remetem aos leitores discriminados na tabela H. “DIVERGE” significa dependência produtiva existente que a transição precisa retirar; não afirma uma ocorrência produtiva nova além do anexo. (`models.py:3315`; `pergunta_no_app.py:70`; `materializacao.py:1369`)

| Célula / porta em 05e6aa147 | Classificação e escrita atual | Fato substituto já existente | Leitores atuais / E3 | Resultado esperado |
|---|---|---|---|---|
| **A01 — `models.py:201 _gravar_estado`** | Efeito/decisão: `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`; prioridade/atualização; relavra perguntas. | **Chamado**, transição e trilha do ato. (`models.py:294`) | H-S; fila ainda lê status do chamado. (`painel.py:155`; `tarefa_do_dia.py:363`) | Conservar a porta do efeito chamado; retirar consequências que perpetuem estado derivado da pergunta. Estado canônico do chamado não entra no alvo zero da pergunta. (`models.py:201`; `models.py:3832`) |
| **A02 — `models.py:1852 fechar`** | Efeito no fechamento normal; decisão nas exceções. Grava F e delega C. | Exceção: DP `SILENCIO`. Normal: quitação pelos efeitos; F e trilha registram o fechamento atual. (`models.py:1921`, `:1929`, `:1937`) | H-F/H-C. App ainda particiona por F. **DIVERGE**. (`pergunta_no_app.py:70`) | Fechamento derivado deixa de comandar exigibilidade. Preservar o ato excepcional e a história antes de retirar projeções. (`tarefa_do_dia.py:149`; `models.py:3832`) |
| **A03 — `models.py:2123 reabrir_admin`** | Decisão: limpa F; incrementa contador/agrega motivos. | **Nenhum DP próprio dessa reabertura do continente**; há gesto, contador e trilha. (`models.py:2135`, `:2146`, `:2158`) | H-F. **DIVERGE**: DP `REABERTURA` da resposta não representa automaticamente esse gesto. (`disputa_emissao.py:1612`) | Preservar decisão, autor, limite e motivo; retirar F como cópia de exigibilidade somente com substituição factual do gesto. (`models.py:2123`; `app/docs/LEIS.md:65`) |
| **A04 — `models.py:2160 reabrir_sistema`** | Efeito: limpa F. | Pergunta voltou a viver; nota de sistema existente. (`models.py:2170`, `:2177`) | H-F. **DIVERGE**. (`models.py:1820`; `pergunta_no_app.py:70`) | Exigibilidade vem da tarefa; não depender de “reabrir F” para tornar ação alcançável. (`tarefa_do_dia.py:442`) |
| **A05 — `models.py:3092 carimbar`** | Efeito no carimbo; decisão nos ramos de assinatura. C; completa R/V; pode gravar L. | Batida/Ausência/chamado/lavra conforme ramo. Assinaturas R/V desta porta não ganham DP automaticamente. (`models.py:3119`, `:3123`, `:3140`) | H-R/H-V/H-C/H-L. **DIVERGE**. (`models.py:3023`; `juizes.py:1323`) | C deixa de ser escrito; assinaturas passam pelo fato de decisão; L permanece explicitamente cache. (`models.py:3832`; `app/chamados/veredito_lavrado.py:254`) |
| **A06 — `models.py:3157 carimbar_identidade`** | Efeito; decisão humana depende do chamador. C e recusa opcional. | Retratação de motivo tem DP; duplicata e outras vias não possuem representação factual uniforme equivalente. (`models.py:2053`; `tarefa_do_dia.py:220`) | H-C; **a própria tarefa lê via atual**. **DIVERGE de identidade**. (`tarefa_do_dia.py:227`; `models.py:2743`) | Não abandonar via enquanto dedup, índice e tarefa dependem dela. Factualizar terminalidade antes de substituir esse discriminador. (`models.py:3474`, `:3480`; `tarefa_do_dia.py:216`) |
| **A07 — `models.py:3192 descarimbar`** | Efeito: limpa C; também preparação em memória. | Fato/decisão que invalida carimbo, conforme chamador. A ausência de carimbo não é o fato. (`models.py:3192`; `materializacao.py:639`) | H-C. **DIVERGE**. (`models.py:3315`; `relavra_via.py:181`) | Retirar a escrita da projeção junto dos consumidores; manter a consequência factual e invalidação de L necessária. (`models.py:3832`; `app/chamados/veredito_lavrado.py:213`) |
| **A08 — `models.py:3241 descarimbar_em_lote`** | Efeito: limpa C e grava campos recebidos em `outros`, inclusive R/V. | DP da correção/recusa quando chamada por esse fluxo. (`disputa_emissao.py:1612`) | H-R/H-V/H-C. **DIVERGE**. (`models.py:3253`; `materializacao.py:1369`) | Nenhum campo derivado pode continuar entrando por `outros`; preservar atos e execução em lote. (`models.py:3241`; `models.py:3832`) |
| **A09 — `models.py:3257 encerrar_por_fato_no_espelho`** | Efeito: `via_resolucao=fato_materializado`. | Fato do marco no espelho/lavra. (`models.py:3258`; `juizes.py:645`) | H-C. **DIVERGE** nos consumidores de via. (`explicador.py:68`; `models.py:3182`) | Não escrever via derivada; ler confirmação do efeito pela autoridade. (`tarefa_do_dia.py:140`) |
| **A10 — `validacao.py:127 _carimbar_validacao`** | Decisão: V + DP `VALIDACAO`. | DP no mesmo instante da validação. (`validacao.py:134`, `:139`) | H-V. **DIVERGE**; tarefa já interpreta DP, mas outros consumidores ainda usam V. (`tarefa_do_dia.py:140`; `juizes.py:1323`) | V projetado deixa de ser escrito; o ato permanece. (`models.py:3832`) |
| **A11 — `validacao.py:163 devolver_validacao_recusada`** | Decisão: limpa V, preserva resposta; DP `DEVOLUCAO`. | DP `DEVOLUCAO`. (`validacao.py:205`) | H-V/H-R. **DIVERGE**. Tarefa já lê o ato. (`tarefa_do_dia.py:138`) | Nenhuma limpeza de V projetado; atos anteriores continuam imutáveis. (`models.py:3818`; `models.py:3832`) |
| **A12 — `disputa_emissao.py:1422 responder_pergunta_disputa_core`** | Decisão: R, invalida V/C, pode escrever recusa explicativa. | DP `RESPOSTA`, inclusive texto/escape. (`disputa_emissao.py:1494`, `:1508`) | H-R/H-V/H-C. **DIVERGE**. (`pergunta_no_app.py:70`; `materializacao.py:1370`) | Conteúdo e instante vêm da sequência; eliminar manutenção de R/V/C projetados. (`models.py:3832`) |
| **A13 — `disputa_emissao.py:1559 reabrir_para_correcao`** | Decisão: limpa R/V e opcionalmente C; **não escreve F**. | DP `REABERTURA` ou `RECUSA`, com foto anterior. (`disputa_emissao.py:1612`, `:1625`) | H-R/H-V/H-C. **DIVERGE**. (`models.py:3903`; `models.py:3315`) | Registrar ato seguinte sem apagar resposta anterior; consumidores deixam de depender da limpeza. (`models.py:3811`; `models.py:3832`) |
| **A14 — `disputa_emissao.py:1635 responder_automaticamente`** | Decisão de sistema: R sem sobrescrever texto existente. | DP `RESPOSTA`. (`disputa_emissao.py:1653`) | H-R; a própria guarda usa texto projetado. **DIVERGE**. (`disputa_emissao.py:1644`) | Preservar voz existente e idempotência pela sequência, sem escrever R. (`models.py:3832`) |
| **A15 — `disputa_emissao.py:1658 carimbar_resposta_pela_trilha`** | Decisão reconstruída: `respondida_em`. | Testemunha da resposta + DP `RESPOSTA`. (`disputa_emissao.py:1674`) | H-R; guarda ainda usa projeção. **DIVERGE**. (`disputa_emissao.py:1667`) | Conservar instante testemunhado; não substituí-lo pelo instante do reparo. (`disputa_emissao.py:1658`) |
| **A16 — `relavra_via.py:106 relavrar`** | Efeito: via derivada + L. | Autoridade do efeito e lavra. (`relavra_via.py:95`, `:118`) | H-C/H-L. **DIVERGE** na via. (`relavra_via.py:118`; `explicador.py:68`) | Encolher para manutenção declarada de L; retirar derivação de via sem consumidores sobreviventes. (`app/chamados/veredito_lavrado.py:254`; `app/docs/LEIS.md:137`) |
| **A17 — `materializacao.py:784 _materializar_resposta_hora`** | Efeito: `pendencia_admin`, `recusa_motivo`; plantio/carimbos delegados. | Sucesso: Batida. Pendência técnica: **nenhum DP próprio**. (`materializacao.py:850`, `:859`) | H-P/H-REC, além de R/C. Template também lê pendência. **DIVERGE**. (`explicador.py:85`; `app/templates/chamados/partials/modal_fio.html:221`) | Pendência deixa de ser estado paralelo; apresentar resultado técnico pela autoridade real, preservando trilha do insucesso. Não convertê-lo em recusa humana. (`app/docs/LEIS.md:60`, `:65`; `materializacao.py:1107`) |
| **A18 — `materializacao.py:1083 _carimbar_recusa`** | Efeito técnico: recusa; colisão delega C de identidade. | Colisão/resultado técnico + nota `recusa_materializacao`; **nenhum DP `RECUSA` humano**. (`materializacao.py:1099`, `:1107`) | H-REC/H-C; própria idempotência lê recusa/C. **DIVERGE**. (`materializacao.py:1089`, `:1092`) | Não usar string ou C como prova de decisão; conservar motivo técnico sem criar ato humano inexistente. (`models.py:3849`; `materializacao.py:1107`) |
| **A19 — `validacao.py:533 recusar_pergunta`** | Decisão: recusa, limpeza R/V delegada. | DP `RECUSA`. (`validacao.py:584`, `:593`; `disputa_emissao.py:1612`) | H-REC/H-R/H-V; guarda usa R. **DIVERGE**. (`validacao.py:576`) | Motivo e foto vêm do ato; nenhuma manutenção dos campos derivados. (`models.py:3840`) |
| **A20 — `validacao.py:625 conferir_data_do_fato`** | Decisão: `data_conferida_em/por`. | **Outro fato existente:** `LogAuditoria` com os dois dias e justificativa. Não há ato DP de conferência. (`validacao.py:664`, `:667`; `models.py:3846`) | H-DATA. Materializador exige timestamp; modal exibe instante **e autor**. **DIVERGE**. (`materializacao.py:1193`; `app/templates/chamados/partials/modal_fio.html:170`) | Substituir o sinalizador por leitura inequívoca do ato de conferência; preservar o par de dias, autor e justificativa. (`validacao.py:667`; `app/docs/LEIS.md:65`) |
| **A21 — `models.py:1968 remover_motivo`** | Decisão; motivos/contexto próprios; C delegado. | DP `RETRATACAO`. (`models.py:2053`) | Tarefa lê DP e ainda via; C6 lê via/C. **DIVERGE**. (`tarefa_do_dia.py:227`, `:230`; `juizes.py:1334`) | Entra apesar do “fica” parcial: retirar a dependência da consequência C, preservando retratação e metadados. (`models.py:2053`, `:2058`) |
| **A22 — `models.py:2822 _nascer`** | Nascimento/reuso: identidade, defaults projetados, costura e L. | Identidade factual e decisões já existentes no reuso. (`models.py:2822`, `:2997`) | H-R/H-V/H-C; identidade continua usando via. **DIVERGE** se defaults/reuso mantiverem campos abolidos. (`models.py:2743`, `:3474`) | Entra por obrigação do diff: nascimento e reuso não podem reintroduzir projeções retiradas. Identidade e L permanecem. (`models.py:3832`) |
| **A23 — `models.py:2179 reabrir_por_colab`** | Contador próprio; decisão delegada. | DP da correção + contador factual do pedido. (`models.py:2234`, `:2236`) | Gate lê F; idempotência atual depende de R ser limpo. **DIVERGE**. (`models.py:2201`, `:2221`) | Preservar teto/contador; migrar gate e idempotência para atos, sem repetir incremento na segunda chamada. (`models.py:2223`, `:2236`) |
| **A24 — `models.py:2247 limpar_resolucao_residual`** | Limpeza de `resolucao` em aberta. | Trilha com texto anterior. (`models.py:2259`, `:2266`) | H-F. **DIVERGE** se F/resolução forem retirados. (`models.py:2259`) | Entra por obrigação do diff; depois da conservação histórica, perde trabalho próprio e deve sair com seus consumidores. (`app/docs/LEIS.md:137`) |

As **20 portas centrais de alteração** conservam os endereços apresentados no censo; os seis nascimentos continuam em `models.py:833`, `:1054`, `:2822` e `disputa_emissao.py:474`, `:1024`, `:1299`. Isso não comprova identidade de commits sem metadados Git. (`models.py:201`, `:1852`, `:2123`, `:2160`, `:3092`, `:3157`, `:3192`, `:3241`, `:3257`; `validacao.py:127`, `:163`, `:533`, `:625`; `disputa_emissao.py:1422`, `:1559`, `:1635`, `:1658`; `relavra_via.py:106`; `materializacao.py:784`, `:1083`; identidade criptográfica: **[nao verificado]**) [nao verificado]

Duas referências do censo são **ocorrências de leitura**, não início de função: `cobranca.py:229` fica dentro de `_calados`, definido em `:221`; `fabrica_da_celula.py:128` fica dentro de `motivos_a_perguntar`, definido em `:74`. Não são evidência de deslocamento entre as árvores. (`cobranca.py:221`, `:229`; `fabrica_da_celula.py:74`, `:128`)

As referências antigas do aval a badge em `models.py:1729/1736` não apontam o leitor atual: há lápide em `models.py:1795`; os consumidores atuais chamam a consulta central em `app/api/views.py:325` e `app/api/views_core.py:314`. (`models.py:1729`, `:1736`, `:1795`; `app/api/views.py:325`; `app/api/views_core.py:314`)

**Conta:** o “16” é o inventário inicial de alterações; o censo corrigido conta **16 + 4 alterações adicionais = 20**, e depois **20 + 6 nascimentos = 26**. Não são o mesmo universo. As quatro adicionais são A17–A20; executores `save`/`registrar` não devem ser somados novamente às portas que executam. (`materializacao.py:784`, `:1083`; `validacao.py:533`, `:625`; `models.py:3402`, `:3932`; número histórico do censo anexado: **[nao verificado]**)

**RESULTADO 11, com unidade explícita: 16 → N=0 portas de escrita de estado derivado da pergunta; lista residual: vazia.** No censo corrigido, a comparação correspondente é **20 → 0**, fiscalizando também os nascimentos. Separadamente permanecem o executor factual `DecisaoPergunta.registrar`, as portas dos efeitos e os escritores declarados de L. Não seria comparável anunciar “16→4” somando três famílias de efeitos a um executor de decisões. (`models.py:3832`, `:3932`; `app/chamados/veredito_lavrado.py:240`, `:254`; realização desse alvo: **[nao verificado]**)

**B. CAMPO A CAMPO — ESMERIL-SEMÂNTICA**

| Célula | Significado e divergência comprovável | Onde morde A |
|---|---|---|
| **B01 — R: presença** | `respondida` testa instante não nulo; SQL combina instante/texto; C6 usa texto com `strip`; tarefa usa última DP e trata projeção sem linha como desconhecimento. São critérios distintos. (`models.py:3486`, `:3337`; `juizes.py:1339`; `tarefa_do_dia.py:127`, `:132`) | A05, A08, A12–A15, A22–A23. |
| **B02 — R: conteúdo** | Resposta pode ser hora, data/hora, escolha, identificador ou escape. Intervalo pode plantar pela hora do marco. Não transformar qualquer texto em instante. (`models.py:3508`; `materializacao.py:356`, `:400`, `:978`) | A05, A12–A15, A17. |
| **B03 — V** | V é assinatura, não confirmação de efeito. C6 encerra por V não nulo; tarefa com DP `VALIDACAO` exige L vigente e distingue materializar, rejulgar e encerrar. Ramo de carimbo também preenche V. (`juizes.py:1323`; `tarefa_do_dia.py:140`; `models.py:3123`) | A05, A10–A13. |
| **B04 — C** | `materializada_em` não significa uniformemente “Batida criada”: identidade/colisão também carimbam; identidade pode conservar instante e autor antigos. Nulo pode resultar de limpeza posterior. (`models.py:3157`, `:3171`, `:3192`; `materializacao.py:1099`) | A05–A09, A16–A18. |
| **B05 — via/identidade** | Via mistura resultado derivado com exclusão da identidade do fato. Retratação tem DP datada; duplicata não tem equivalente datado na tarefa atual. Índices e seleção ainda dependem da via. (`tarefa_do_dia.py:216`, `:220`, `:227`; `models.py:3474`, `:3480`) | A06–A08, A16, A21–A22. |
| **B06 — F** | F significa fechamento do continente, não inexistência de tarefa. App manda viva em fechada ao histórico; pedido de correção mantém gate de fechada. Reabertura da resposta não é reabertura do continente. (`pergunta_no_app.py:70`; `models.py:2221`; `disputa_emissao.py:1559`) | A02–A04, A13, A23–A24. |
| **B07 — pendência** | Pendência técnica precede o rótulo: resposta pode parecer validada/resolvida e ainda exigir admin. Explicador e templates dependem disso. (`materializacao.py:850`; `explicador.py:85`; `app/templates/chamados/partials/modal_fio.html:221`; `app/templates/chamados/partials/_card_atendimento.html:183`) | A17; saída dos leitores de A05/A10. |
| **B08 — recusa** | Humana gera DP `RECUSA`; técnica grava campo/nota; advertência de escape acompanha DP `RESPOSTA`. Não reconstruir recusa humana de qualquer string nesse campo. (`validacao.py:593`; `materializacao.py:1103`, `:1107`; `disputa_emissao.py:1494`, `:1508`) | A06, A12–A13, A17–A19. |
| **B09 — conferência** | `data_conferida_em/por` representa autorização humana diante de dois dias divergentes. Materializador usa apenas presença do timestamp; trilha guarda o par/justificativa; modal usa também autor. (`materializacao.py:1193`; `validacao.py:667`; `app/templates/chamados/partials/modal_fio.html:170`) | A20. Validade da conferência após alteração do par de dias: **[nao verificado]**. |
| **B10 — dia versus instante** | `data_evento` é dia do turno no fato e dia civil na conversa. Plantio combina dia/hora e ajusta overnight pelo envelope inclusivo. O instante da Batida não é esse dia. (`models.py:2791`; `juizes.py:296`, `:332`; `materializacao.py:413`) | A17, A20, A22; identidade de A06/A21. |
| **B11 — L e bordas** | `julgado_em` é instante; expiração é dia local. Expira por `<=`; marco posterior invalida por `>`. Ausente/indeterminado não equivale a “não materializado”. C6 ainda usa janela inclusiva e relógio que `--agora` não governa integralmente. (`app/chamados/veredito_lavrado.py:116`, `:224`, `:232`; `juizes.py:575`, `:601`; `app/chamados/management/commands/diff_e3_sombra.py:105`) | A05/A16; medidas C e leitores H-L. Divergência produtiva efetiva nessa borda: **[nao verificado]**. |
| **B12 — chamado** | `VIVOS`, `COBRANCA`, `ENCERRADOS`, `TERMINAIS` e `NAO_FECHADOS` respondem perguntas diferentes. “Tem tarefa” não substitui globalmente esses conjuntos. (`app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`, `:32`) | A01 e nascimentos do chamado; classificação C. |
| **B13 — dois silêncios** | Véu temporário vigora com `silenciado_ate > agora`, final exclusivo. DP `SILENCIO` é encerramento excepcional da pergunta. Não são o mesmo ato. (`models.py:511`, `:1921`; `arquivo.py:154`) | A02; filtros de fila usados por A01 e pela tarefa. |
| **B14 — instante histórico** | Reconstrução usa instante da testemunha; reparação ocorre depois. Ordenação das decisões é por `ato_em, pk`; carga considera atos `<= agora`. (`disputa_emissao.py:1658`; `models.py:3878`; `tarefa_do_dia.py:338`) | A15; backfill C3. |

Logo, **não**, escritores e leitores não supõem todos o mesmo significado dos campos. A retirada precisa conservar essas distinções na leitura factual, sem uma propriedade local voltar a ser segundo juiz. (`app/docs/LEIS.md:60`; evidências B01–B14)

**C. O FIO MORTO E O PASSIVO**

**C1 — contadores e universos**

| Contador | Definição efetiva |
|---|---|
| `fio_morto` | Células acusadoras alcançadas por chamados vivos de `FURO_MODULES

[truncado em 20000 de 59200 caracteres]

_3221 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
