# CODEX-ESTADO

> Ultima resposta processada: **21/11/2026 18:37** · publicado em **21/11/2026 18:37** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 21/11 18:37 · sessao 528e4946c43e · E4C-F5-CONSTROI · verificador · origem=Code

**BLOQUEIA — E4C-F5 item 1.2.**

1. A validação pública ainda carimba e planta por uma entrada que contorna `travar_o_ato_do_plantio`. `validacao.py:697–760,786–805`
2. O passivo ainda usa dados decisórios lidos antes da trava, incluindo competência, exportação e a conferência de data das perguntas avulsas. `passivo_validado.py:334–396,462–478,549–557` · `materializacao.py:1922–1928,1947–1970`
3. O caminho do flip pode adquirir Batida → Colaborador, enquanto o plantio pode adquirir Colaborador → Batida. Deadlock observado **[nao verificado]**. `flip_automatico.py:62–79` · `ponto/signals.py:206–218` · `chamados/reconciliador.py:635–646,1486–1487` · `materializacao.py:1418,1586–1590,2145–2146`
4. O censo AST da cópia diverge em quatro entradas da catraca de linhas. O RED, os mutantes e a suíte inteira deste hash, com veredito terminal, `Ran` e RC, estão **[nao verificado]**. `core/medida_turno_do_par.py:448–454` · `core/tests/test_medida_turno_do_par.py:271–274,558–562` · `CLAUDE.md:119–123,372–381`

### 1. P1, P3, P5, P7 e descartados

A correspondência individual de **P1, P3, P5 e P7** e a lista nominal dos descartados estão **[nao verificado]**. O `r.txt` fornecido descreve os achados por seção, sem a triagem numerada. Não localizei o diff integral `b0935c1f1..6a5ab520c` na cópia exportada. `bin/codex_pergunta.sh:311–319`

| Achado identificável no `verif1` | Situação na cópia |
|---|---|
| Candidatas fora da trava | **Fechado para as linhas de `PerguntaDisputa` examinadas.** Todas as candidatas da consulta entram na trava, inclusive sem resposta ou com eleição ambígua. O helper relê a eleição e o conjunto, e recusa mudança ou PK faltante. As linhas de `DecisaoPergunta` vêm por `prefetch` e ficam fora do `FOR UPDATE OF self`. `materializacao.py:139–157,2123–2158` · `models.py:2900–2904` |
| Foto velha da principal no passivo | **Fechado para mudanças nos cinco valores da composta.** A comparação ocorre antes da unidade de trabalho. Os demais insumos continuam com as lacunas do item 3. `passivo_validado.py:360–362,382–401` · `models.py:4382–4390` |
| Barreira, ordem e selos | **Corrigidos no código dos testes.** A barreira rompida agora sobe como falha. O teste inverso usa o mapa retornado, verifica o SQL das quatro travas e cobra duas batidas com uma trilha por batida. Há casos dirigidos para as guardas e para o reset do contexto. A execução neste hash está **[nao verificado]**. `test_e4c_f5_plantio_concorrente.py:63–132,189–263` | [nao verificado]
| Entrada pública e fila inicial | **Continuam abertas.** A entrada pública escreve antes de chegar ao laço protegido. O laço ainda seleciona pela face SQL da resposta e pela coluna física `validada_em`. O passivo também começa pela coluna física. Uma principal excluída nessa etapa fica fora da releitura. `validacao.py:743–760,786–805` · `materializacao.py:2194–2225` · `vigia_carimbo.py:94–116` |

O uso do derivador pela tela pode ficar em fila, por ser leitura. O uso da mesma informação para escolher travas, validar e plantar faz parte do ato coberto pelo aval. A inversão com o flip também permanece pertinente à verificação da ordem. `aval.txt:1` · `materializacao.py:795–799` · `validacao.py:714–723,743–760,786–803` · `flip_automatico.py:62–79` · `chamados/reconciliador.py:635–646`

### 2. Quem escreve, quem lê e para onde vai o erro

- **`estados_classificados`.** `reparar` cria o dicionário local e é seu único escritor. Em `apply`, guarda a composta por PK após a classificação e os recortes. O único leitor é a comparação em `:392`. Depois de travar, `reparar` carrega novas instâncias com `prefetch_related('decisoes')`. As instâncias de `atuais` servem à comparação. `_trabalhar_o_par` recebe os `itens` anteriores. `passivo_validado.py:332–362,388–396`
- **Fontes da composta.** `DecisaoPergunta.composta` lê os instantes de resposta e validação pela sequência, o texto da projeção e o carimbo e via das colunas. Resposta humana, automática e pela trilha alimentam resposta e sequência. Validação e devolução assinam ou limpam a validação e registram o ato. `registrar` insere a sequência. O modelo escreve e desfaz carimbo e via. `models.py:24–25,3324–3328,3363–3374,3394–3405,3451–3456,4361–4390,4428–4453` · `disputa_emissao.py:1499–1537,1707–1737` · `validacao.py:161–175,269–296`
- **Quem levanta.** O helper levanta `AtoDoPlantioMudou` por mudança de escopo da principal, troca da irmã ou inconsistência no conjunto de candidatas. O passivo levanta a mesma exceção quando a composta da principal difere da foto. `materializacao.py:2112–2113,2150–2158` · `passivo_validado.py:388–393`
- **Quem captura no passivo.** `reparar` captura após sair do `atomic`, recusa o grupo inteiro no balde `escopo_do_plantio_mudou`, guarda a mensagem e segue. O comando salva o resultado e imprime o balde e `ERRO`. A exceção fica absorvida, com diagnóstico. Esse caminho termina normalmente se o restante do comando completar. RC observado **[nao verificado]**. `passivo_validado.py:382–401,437–440` · `materializar_passivo_validado.py:100–141,241–249,272–274`
- **Quem captura no laço da disputa.** O laço captura antes da escrita, acrescenta a mensagem a `erros` e passa à próxima pergunta. A validação pública distingue erros próprios e de irmãs. As cascas de regularização podem retornar antes de detalhar o motivo. O backfill descarta o retorno do laço. `materializacao.py:2210–2236` · `validacao.py:803–854` · `acoes_disputa.py:143–149,164–179` · `fio.py:722–728,745–758` · `backfill_fio_morto_e4b.py:153–159`

### 3. TOCTOU

- **Validação pública.** A composta, os gates de resposta e implausibilidade e a irmã inicial são lidos antes da trava. `trava_na_ordem` recebe os chamados dessa irmã, sem os PKs das perguntas. O código carimba a principal antiga e `_validar_o_par` pode eleger outra irmã e plantar antes da varredura protegida. `validacao.py:648–659,693–760,786–805` · `models.py:2900–2903`
- **Passivo.** `classe` e `ts` são obtidos antes da trava. O `ts` pode determinar o dia quando falta `data_evento`. Dia, competência, corte e `exportada` ficam no agrupamento usado para decidir geometria, regeneração e pauta. A unidade pode recalcular a hora da batida e ainda trabalhar com aquele agrupamento. A exportação tem escritor independente da trava do colaborador. A foto de reversão também é anterior à trava e permanece no resultado mesmo quando o grupo é recusado. `vigia_carimbo.py:97–116` · `passivo_validado.py:102–132,334–401,462–478,495–505,547–577` · `materializacao.py:1404–1418` · `folha/views.py:297–302` · `folha/models.py:174–188`
- **Contraexemplo na principal avulsa.** Uma confirmação de data pode gravar `data_conferida_em` entre a classificação e a trava, sem alterar a composta ou a foto estrutural. O passivo atualiza somente carimbo, via e autor naquela instância. O refresh total da unidade é restrito às metades de intervalo. A avulsa pode então gravar uma recusa `DATA_EM_DISPUTA` baseada na conferência antiga. `validacao.py:1008–1016` · `models.py:4382–4390` · `passivo_validado.py:549–557` · `materializacao.py:1922–1925,1947–1970,2116–2120`
- **Outras leituras da escrita.** O fecho do destino e dos clones consulta outras perguntas creditadas, e o par consulta batidas, vizinhança e célula. As quatro travas cobrem as tabelas e os PKs enumerados pelo helper. A proteção integral dessas outras leituras está **[nao verificado]**. `materializacao.py:219–247,447–474,1302–1322,2135–2146` · `juizes.py:2121–2126` · `models.py:2875–2904`

### 4. RED de `6a5ab520c`

- **Outra conexão, commit antes da trava.** O teste usa `TransactionTestCase`, chama a porta real de devolução em outra thread e abre `atomic(durable=True)`. A saída do `atomic` efetiva o commit antes de o `Future` devolver o PK. Só então o passivo chama a trava real. A corrida reproduzida é a devolução já confirmada entre classificação e trava. O PID desse RED específico está **[nao verificado]**. `test_e4c_f5_guarda_plantio_s144.py:101–111,298–327` · `validacao.py:269–296` [nao verificado]
- **`if False` em `:392` deve ficar VERMELHO por leitura estática.** A fixture usa `orfao_14h`, sem eleição de irmã, e a devolução muda a validação e a sequência sem mudar a foto estrutural. Retirada a reconfirmação, o único produtor do balde esperado deixa de ser acionado. O teste exige exatamente `{'escopo_do_plantio_mudou': 1}` e nenhuma tentativa de carimbo. O teste isolado também exige que `_trabalhar_o_par` fique sem chamada. Execução do mutante **[nao verificado]**. `passivo_validado.py:389–401` · `materializacao.py:139–145,2116–2127` · `test_e4c_f5_guarda_plantio_s144.py:44–58,113–121,165–202` [nao verificado]
- **Limpeza e relógio.** O worker fecha a conexão em `finally`, e o executor aguarda a thread. A devolução fica commitada para a asserção por PK. A limpeza das linhas cabe ao `flush` do `TransactionTestCase` no banco de teste. Teardown efetivo **[nao verificado]**. Nesse RED o relógio de negócio está congelado na thread principal e no worker, e `hoje` é explícito. O worker fixa `lock_timeout` de 4 s. O limite de 15 s do `Future` limita a espera e não cancela a thread. `test_e4c_f5_guarda_plantio_s144.py:30–58,110–111,133–155,298–327` · `config/settings/ci.py:6–17`

### 5. Checklist 5(a)

| Classe | Resultado |
|---|---|
| `except` e fallback | **Total contra a base [nao verificado].** No delta fornecido há **2 `except` acrescentados em teste, 1 removido e 0 acrescentados em produção**. `BrokenBarrierError` sobe pelo `Future` como `AssertionError` com causa. `AtoDoPlantioMudou` vira retorno do worker. Em produção, a nova reconfirmação alimenta a captura do passivo, que registra a recusa e segue. O delta fornecido não acrescenta plantio alternativo em caso de erro. `test_e4c_f5_plantio_concorrente.py:75–103` · `passivo_validado.py:388–401` | [nao verificado]
| Segundo juiz | A eleição continua chamando o C21 registrado sobre a composta. Há uma divergência no leitor de exportação do passivo. `_esta_exportada` aceita o último TXT marcado vigente mesmo quando invalidado. O K6 registrado exclui esse caso. A precedência desse achado em relação à base está **[nao verificado]**. `core/juizes.py:305,342` · `materializacao.py:146–154` · `passivo_validado.py:120–132` · `folha/haiku_export.py:38–54,59–75` · `folha/models.py:203–225` · `folha/export.py:580–594` |
| Segundo escritor fora da porta | A validação pública reutiliza os escritores de validação, batida e carimbo. A segunda entrada para esses escritores continua sem a mesma porta de trava. `validacao.py:648–659,743–760,786–803` · `materializacao.py:1531–1539,2042–2046` |
| Relógio fora da borda | A reconfirmação não lê relógio, e o `hoje` do passivo chega do comando. O fluxo de escrita ainda lê instantes distintos na validação, aprovação da batida e carimbo. O teste inverso congela a resposta, mas valida e materializa fora desse congelamento. `models.py:4348–4390` · `materializar_passivo_validado.py:85–89,103–114` · `validacao.py:161–175` · `materializacao.py:1531–1539,2009` · `test_e4c_f5_plantio_concorrente.py:31–39,75–89` | [nao verificado]
| Derivação paralela | Para `hora`, classificação e plantio usam `_ts_tipo_do_plantio`. Para `data_hora`, a cópia tem dois pontos de parse do slot. A novidade contra a base está **[nao verificado]**. `juizes.py:344–379` · `materializacao.py:1404–1405,1634–1650` |
| Selo e ordem de trava | O teste inverso inspeciona SQL real em `atomic`, exige PIDs distintos e `ASC` nas quatro tabelas, uma tentativa vencedora e uma trilha por batida. Ele chama a unidade diretamente e não cruza com o flip. A rodada com mutante real da ordem está **[nao verificado]**. `models.py:2875–2904` · `test_e4c_f5_plantio_concorrente.py:41–47,63–132` · `flip_automatico.py:62–79` | [nao verificado]
| Campo com dois significados | O `ContextVar` faz dois papéis. Guarda a eleição por PK e sinaliza que a principal deve ser relida. O `.get(p.pk)` produz o mesmo `None` para PK ausente e eleição confirmada sem irmã. O token é restaurado em `finally`. `materializacao.py:202–207,1832–1842,1922–1925` |

**Catraca medida em memória.** O censo da cópia encontrou `[448, 469]` em `_plantio_preenche_furo_na_data`, `[117]` em `_dia_do_plantio`, `[567, 569]` em `_plantar_o_dia` e `[355]` em `reparar`. O selo ainda exige `[438, 459]`, `[113]`, `[550, 552]` e `[351]`, respectivamente. Essa comparação exata já diverge na cópia examinada. `core/tests/test_medida_turno_do_par.py:271–274,558–562` · `materializacao.py:448,469` · `passivo_validado.py:117,355,567–569`

## 21/11 18:36 · sessao 4bb167c1a815 · - · - · origem=Ronald (tmux)

- `estados_classificados` nasce e é preenchido apenas em `reparar` com `apply`, após classificar e aplicar os recortes. O único leitor compara a composta atual após a trava. A nova consulta não substitui as instâncias de `itens`. A composta inclui validação e resposta do ato, texto da projeção, carimbo e via. `passivo_validado.py:332–362,382–396` · `models.py:24–25,4361–4390`
- `AtoDoPlantioMudou` sai do helper por mudança da principal, eleição ou composição do conjunto e do passivo quando a composta muda. O passivo captura fora do `atomic`, conta o grupo inteiro, conserva o motivo em `erros` e segue. O comando grava e imprime o erro sem convertê-lo em `CommandError`. No laço da disputa a captura ocorre antes da escrita e o erro vira item da lista retornada. `materializacao.py:2112–2113,2152–2158,2210–2236` · `passivo_validado.py:382–401,429–440` · `materializar_passivo_validado.py:100–141,248–249,272–274`
- Validação, ações da disputa e fio consomem a lista retornada. Validação separa erros próprios e de irmãs. Ações e fio registram a contagem, mas retornam avisos ou sucesso sem detalhar erros em certos ramos. A validação direta elege a irmã antes de travar, não fornece os PKs de perguntas à trava e pode plantar antes do laço. `validacao.py:743–760,786–854` · `acoes_disputa.py:143–149,164–179` · `fio.py:722–728,745–758`
- A conferência de cinco atributos não cobre classe, dia, competência, exportada, foto de reversão ou campos do gate de data. `exportada` fica em cache antes da trava e decide se haverá regeneração e pauta. A folha escreve exportação concorrente por sua própria porta. Para uma principal avulsa, `data_conferida_em` pode permanecer antigo mesmo com composta igual, pois o refresh total da unidade é restrito às metades de intervalo. `passivo_validado.py:102–132,334–379,390–396,462–478,545–577` · `folha/views.py:297–302` · `folha/models.py:174–188` · `materializacao.py:1922–1928,1947–1970,2116–2120` · `validacao.py:1008–1016`
- O RED usa a porta de devolução em thread separada, com `atomic(durable=True)` e fechamento da conexão em `finally`. A `Future` termina antes da trava real. A mutação em `passivo_validado.py:392` deve falhar estaticamente porque o `orfao_14h` não tem eleição de irmã e a devolução não muda as coordenadas da foto. O teste exige a recusa exata e nenhuma chamada de carimbo. Execução, PID desse RED, limpeza efetiva e RC **[nao verificado]**. `test_e4c_f5_guarda_plantio_s144.py:51–57,101–121,298–327` · `materializacao.py:143–145,2116–2120` · `validacao.py:269–296` [nao verificado]
- Os cinco blobs coincidem com o lado novo do delta fornecido. Diff inteiro contra a base e triagem numerada **[nao verificado]**. O delta fornecido acrescenta duas linhas `except` de teste e remove um `except RuntimeError`. A barreira sobe como `AssertionError` por `Future`. A recusa esperada vira retorno do worker. `bin/codex_pergunta.sh:311–319` · `test_e4c_f5_plantio_concorrente.py:75–103`

## 21/11 18:35 · sessao 2fc389242459 · FERIADO-POR-POSTO · verificador · origem=Code

**BLOQUEIA.** A volta 4 corrige o `rc 0` quando `MarcaInvalida` sai da porta. Na comparação com `main`, seguem três achados **NOVOS**: `--apply` sem motivo ainda imprime erro e retorna normalmente, a prévia classifica um par antes do encerramento do turno e a API da prévia lê o relógio quando o chamador omite `agora`. A releitura em `celula.py:464` é **PREEXISTENTE**, com prova pareada em memória. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:118–130` · `app/colaboradores/services/feriado_do_posto.py:370–385,449–474` · `app/ponto/portas/celula.py:460–474` nas duas árvores · aval, item 2.

### 1. Erros e saídas do comando

- **O item nominal está fechado.** Na volta 3, o `except MarcaInvalida` imprimia e retornava. Na volta 4, o mesmo `except` levanta `CommandError` com a causa original, tanto no `--apply` quanto no dry-run na sombra. `PreviaForaDaSombra` também vira `CommandError`. Exercitei os dois ramos em memória e confirmei que um `RuntimeError` saído de qualquer uma das portas sobe intacto. `reaplicar_marca` delega diretamente e a prévia só captura sua sentinela de rollback. `b95a383d6` e `d72b72a76`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:118–130` · `app/colaboradores/services/feriado_do_posto.py:357–359,397–406`.
- **Ainda há três retornos normais com mensagem de erro antes da porta.** `--fecha` e `--opera` iguais, e PK inexistente, são **PREEXISTENTES**. `--apply` sem motivo, inclusive motivo só com espaços, é **NOVO** na obra. Essa guarda impede que a porta levante a `MarcaInvalida` prevista para o mesmo caso. A comparação em memória confirmou os três retornos normais. Cópia, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:70–75,111–124` e `app/colaboradores/services/feriado_do_posto.py:305–306`. Main, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:57–60,76–80,124–137`.
- **Há erros consumidos dentro das dependências.** Uma falha ao criar `LogAuditoria` é registrada e consumida por `evento`, com savepoint próprio. Uma falha no julgamento ou recálculo da célula também pode ser registrada e consumida. Nesses casos, a porta pode devolver resultado e o comando terminar normalmente. A obra também passou a chamar o helper best-effort `registrar_log` para a nova trilha da chave do vínculo. `app/colaboradores/services/feriado_do_posto.py:318–323,344–354` · `app/core/observ.py:85–98,124–139` · `app/ponto/portas/celula.py:460–500` · `app/colaboradores/services/vinculo.py:896–904` · `app/core/auditoria.py:17–30`.

### 2. Quem levanta, captura e lê o retorno

- **Seis pontos levantam `MarcaInvalida`.** `plano_da_marca` levanta por valor diferente de `True`/`False`, `hoje=None`, FECHA sem município e retomada sem um posto já FECHA. `aplicar_marca` levanta por motivo vazio e reaplicação de posto sem marca. A única captura produtiva localizada dessa classe é a do comando. Os testes da porta capturam a superclasse `ValueError`. O teste novo captura o `CommandError` convertido. `app/colaboradores/services/feriado_do_posto.py:257–275,305–315` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–130` · `app/colaboradores/tests/test_feriado_por_posto.py:171–176,249–274,356–362` · `app/colaboradores/tests/test_feriado_volta4.py:17–25`.
- **Zero leitores de `rc` específicos localizados na cópia.** O comando está em `FORA_DE_PIPELINE`. `bin/cron_run.sh` lê `$?` genericamente. Os testes localizados chamam `call_command` e leem exceção, saída ou estado. Nenhum exige retorno normal para `MarcaInvalida`. O teste de `--apply` sem motivo depende de retorno normal para chegar à asserção de que a marca segue vazia. O `manage.py` delega ao Django, e a expectativa registrada é `rc 1` para `CommandError`. O `rc` numérico em subprocesso real, o crontab instalado e consumidores externos ficam [nao verificado]. `app/config/crons.py:769,1350–1355` · `bin/cron_run.sh:4–13` · `app/manage.py:7–18` · `app/colaboradores/tests/test_feriado_por_posto.py:629–662` · `app/colaboradores/tests/test_feriado_previa_da_marca.py:274–288` · `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83–101`.

### 3. O teste novo morde [nao verificado]

**VERDE na cópia e VERMELHO com o mutante em memória.** Executei o corpo do teste com o comando e a validação reais, usando substitutos para Django, ORM e relógio. Restaurar `w(ERROR); return` reproduziu exatamente o fonte da volta 3 e falhou com `AssertionError: CommandError not raised`. O teste fixa o relógio com `freeze_time`, usa a fixture `_posto` existente e chama `call_command` e `refresh_from_db`, APIs já usadas nos testes vizinhos. A execução do `TestCase` com Django e banco reais nesta árvore fica [nao verificado]. `app/colaboradores/tests/test_feriado_volta4.py:8–25` · `app/colaboradores/tests/test_feriado_por_posto.py:51–57,82–86,627–649` · `app/requirements.txt:13,43` · `b95a383d6`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:129–131`.

### 4. Checklist do delta `b95a383d6..d72b72a76`

- **`except` adicionados: 0. Fallbacks adicionados: 0.** O corpo de um handler existente trocou o retorno normal por `raise ... from e`. O teste acrescentado exige a exceção. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:127–130` nas duas voltas · `app/colaboradores/tests/test_feriado_volta4.py:15–25`.
- **Segundo juiz e derivação paralela.** Zero introduzidos pelo delta. Na obra, a mesma divergência cobre as duas classes: a prévia usa entrada e saída para incluir em `(a) PASSAM A PAGAR`, enquanto o juiz registrado também exige dia encerrado. A seleção dos vínculos da marca usa o mesmo universo na porta e no A4. `app/colaboradores/services/feriado_do_posto.py:203–234,283–285,449–474` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:78–85` · `app/core/juizes.py:243–248`.
- **Segundo escritor e duplo significado.** O censo AST localizou um escritor da marca e um chamador. `opera_em_feriado` é a declaração do posto. `trabalha_em_feriado` é a chave efetiva do vínculo, lida com as condições de ciclo e feriado. `alvo` conta candidatos e `retratados` conta o efeito aceito pelo reconciliador. `app/colaboradores/services/estrutura.py:90–95` · `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:226–238` · `app/escala/models.py:1384–1406` · `app/colaboradores/services/feriado_do_posto.py:277–285,318–341`.
- **Relógio e selo.** Zero leituras acrescentadas pelo delta. O selo novo morde a regressão pedida. O selo anterior de meia-noite observa os argumentos entregues à porta do vínculo, e a releitura na célula fica abaixo dessa observação. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:41–50,75,117–130` · `app/colaboradores/tests/test_feriado_volta4.py:15–25` · `app/colaboradores/tests/test_feriado_a5_volta3.py:243–266` · `app/ponto/portas/celula.py:460–474`.

### 5. Achados da volta 3 contra `main ab351aa9b`

| Achado aberto | Classificação e evidência nas duas árvores |
|---|---|
| Releitura em `_o_chao_do_dia_mudou` e no cartório | **PREEXISTENTE.** Rodei o mesmo caso em memória com os fontes das duas árvores. Sem virada, a célula de 16/12 foi tratada como futura e ficou fora do julgamento. Com virada de 23h59 para 00h01, a regeneração ainda usou 15/12 e o cartório recebeu 16/12 nas duas. A porta do vínculo no main faz uma leitura inicial própria, que a cópia substitui pela injeção. **Sai do bloqueio pelo aval, item 2.** Main e cópia, `app/ponto/services/cadastro_futuro.py:43–59`, `app/ponto/portas/celula.py:306–315,390–394,460–474` e `app/ponto/services/cartorio.py:2059–2089`. Porta do vínculo, main `app/colaboradores/services/vinculo.py:895–913`, cópia `:905–924`. |
| Outros relógios | **PREEXISTENTE:** presença sem limites e sincronizador sem `hoje`. Main `app/colaboradores/services/feriado_do_posto.py:74–88`, cópia `:74–89`. Main `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53–57` → `app/colaboradores/services/vinculo.py:895–897`, cópia `:68–72` → `:905–908`. **NOVO:** `previa_da_marca` lê `now()` sem `agora`; a casca passa o valor e os testes diretos exercitam a omissão. Main `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–126`; cópia `app/colaboradores/services/feriado_do_posto.py:370–385`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–126` e `app/colaboradores/tests/test_feriado_previa_da_marca.py:99`. O A4 lê na própria borda, em `app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:14–18`, comando ausente no main. |
| Segundo juiz da prévia | **NOVO.** Com saída às 16h55 e turno previsto até 17h, o cenário em memória deu um item em `(a)` às 16h56 na cópia. O juiz registrado devolveu `False` às 16h56 nas duas árvores. O main tinha apenas a impressão do plano no dry-run. Main e cópia `app/core/juizes.py:243–248` e `app/ponto/turnos.py:2113–2130,2185–2214`. Main `app/colaboradores/management/commands/marcar_feriado_do_posto.py:117–126`; cópia `app/colaboradores/services/feriado_do_posto.py:449–474`. |
| Janela do sincronizador | **PREEXISTENTE.** Em memória, com chave inicialmente `False`, o sincronizador enfileirou `True`, a marca passou a FECHA antes da escrita e o lote gravou `True` nas duas árvores. O filtro novo protege quando a marca já está declarada na seleção. Main `app/colaboradores/management/commands/sincronizar_feriado_admin.py:42–57` e `app/colaboradores/management/commands/marcar_feriado_do_posto.py:128–137`; cópia `app/colaboradores/management/commands/sincronizar_feriado_admin.py:43–71` e `app/colaboradores/services/feriado_do_posto.py:318–323,344–352`. |
| Auditoria best-effort | **PREEXISTENTE** para o evento da marca e para julgamento/recálculo da célula. **NOVO alcance** na trilha da chave, que passa a usar o helper existente. Main `app/colaboradores/management/commands/marcar_feriado_do_posto.py:130–137` e `app/colaboradores/services/vinculo.py:890–897`; cópia `app/colaboradores/services/feriado_do_posto.py:318–323` e `app/colaboradores/services/vinculo.py:896–904`; nas duas `app/core/observ.py:85–98,124–139`, `app/core/auditoria.py:17–30` e `app/ponto/portas/celula.py:460–500`. |

**F1–F5.** Reexecutei as varreduras estáticas de F1 e F2, com zero ocorrências abertas. A forma antiga `type(posto)` voltou a acusar `?type` em memória. Na retratação a trava de `Posto` vem antes do reconciliador, que usa a ordem canônica das outras quatro entidades. F3/F4 mantêm os testes adversariais `1 | 1` e `0 | 0` com o rótulo novo. Em F5, `CLASSIFICAR` bate e os contadores seguem em 9 e 85. O censo B2 exige operando explícito e deixa de fora as releituras acima. Contenção real, suíte inteira, `sombra_c2b`, opus final e produção ficam [nao verificado]. `app/ponto/tests/test_contract_no_except_pass.py:50–78` · `app/chamados/tests/test_e1_v7_catracas.py:385–415` · `app/colaboradores/services/feriado_do_posto.py:307–309,325–335` · `app/chamados/models.py:754–770,2816–2825` · `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83–101` · `app/core/tests/test_medida_turno_do_par.py:100,389–390,548–571` · `app/core/medida_turno_do_par.py:217–226` · `app/docs/BACKLOG.md:1966` · `CLAUDE.md:372–381`.

## 21/11 18:35 · sessao 31469f47b34d · - · - · origem=Ronald (tmux)

**BLOQUEIA.** A validação pública ainda carimba e planta diretamente, usando a irmã e os gates calculados antes do `atomic` e sem passar PKs de perguntas na chamada da trava. A varredura que usa o helper comum vem depois da escrita. `validacao.py:697–744,746–760,786–805,648–659`

- **P1/P3/P5/P7 e descartes.** O `r.txt` descreve os achados sem ligar os quatro IDs aos respectivos textos ou listar a triagem dos descartes. Vínculo nominal e descarte por ID [nao verificado]. A correção das candidatas cobre os PKs examinados, inclusive candidatas inelegíveis, e compara a eleição, o conjunto e a inclusão nas travas. As linhas de decisão ainda vêm por prefetch separado e as demais leituras decisórias continuam com cobertura parcial. `materializacao.py:139–157,2116–2158` · `models.py:2875–2904,4382–4390`
- **TOCTOU no passivo.** O `exportada` obtido antes da trava controla regen e pauta depois dela. O exportador registra a entrega fora dessa trava. Para perguntas avulsas, `data_conferida_em` fica na instância antiga e pode levar ao carimbo indevido de recusa após a confirmação concorrente do dia. A reversão do grupo recusado ainda conserva a foto anterior no resultado. `passivo_validado.py:351–401,462–474,549–557` · `folha/views.py:297–302` · `folha/models.py:174–188` · `validacao.py:1008–1016` · `materializacao.py:1922–1925,1947–1970` · `materializar_passivo_validado.py:117–130`
- **Capturas e fallback.** No delta fornecido `f3e48a445..6a5ab520c`, há dois `except` adicionados em teste e um removido, o `RuntimeError` amplo. `BrokenBarrierError` sobe como `AssertionError` encadeado e `AtoDoPlantioMudou` vira resultado do worker. O delta não adiciona captura de produção nem caminho alternativo de plantio em caso de erro. `test_e4c_f5_plantio_concorrente.py:79–103` · `materializacao.py:2150–2160` · `passivo_validado.py:389–401`. O total de `b0935c1f1..6a5ab520c` [nao verificado]. A cópia é compatível com o `git archive` do wrapper e a busca não encontrou a base, `.git` ou o diff integral. `bin/codex_pergunta.sh:311–319`
- **Chamadores.** O passivo transforma a exceção em recusa de grupo e o comando imprime `ERRO`, mantém o resultado no JSON e termina pelo fluxo normal. O laço da disputa agrega a mensagem em `erros`; a validação pública traduz esse retorno. As duas cascas de regularização podem retornar aviso ou sucesso antes de anexar o motivo, e o backfill descarta o retorno. `passivo_validado.py:397–401,437–440` · `materializar_passivo_validado.py:117–141,272–274` · `materializacao.py:2210–2236` · `validacao.py:803–848` · `acoes_disputa.py:143–149,164–179` · `fio.py:722–728,745–758` · `backfill_fio_morto_e4b.py:153–159`
- **Ordem.** O teste atual verifica as quatro tabelas em sequência, SQL ascendente para cada uma, PIDs distintos e ausência de duplicação das duas batidas/trilhas no cenário. A inversão `Batida → Colaborador` do flip contra `Colaborador → Batida` continua possível no código, com execução [nao verificado]. `test_e4c_f5_plantio_concorrente.py:58–132` · `flip_automatico.py:62–79` · `flip_batida.py:25–33` · `registro_batida.py:264–274` · `ponto/signals.py:206–218` · `reconciliador.py:635–646,1486–1487` · `materializacao.py:1586–1590,2145–2146`

## 21/11 18:34 · sessao fb9068025542 · - · - · origem=Ronald (tmux)

O RED reproduz a janela do passivo por leitura estática. A devolução usa a porta real em outra thread, grava em `atomic(durable=True)` e fecha a conexão antes de entregar seu PK. O passivo aguarda o `Future` antes de chamar a trava real. O cenário específico usa a conexão Django vinculada à thread e não mede PIDs. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:101–117,298–327` · `app/chamados/services/validacao.py:269–296,309–321`

O mutante `if False` em `passivo_validado.py:392` deve ficar VERMELHO. A principal avulsa tem motivo `orfao_14h` e a devolução altera a validação e a sequência, que ficam fora da foto estrutural do helper. Sem a comparação da composta, a execução alcança a unidade e o balde exato exigido pelo teste não é produzido. O teste isolado também exige que `_trabalhar_o_par` não seja chamado. Execuções do RED, do mutante e o trio veredito, `Ran` e RC neste hash [nao verificado]. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:44–58,113–119,165–202` · `app/chamados/services/materializacao.py:139–147,2116–2127,2152–2158` · `app/chamados/services/passivo_validado.py:389–401` · `CLAUDE.md:119–123`

O executor espera o worker e o worker fecha sua conexão em `finally`. O commit da devolução persiste durante o teste e é identificado pelo PK. A limpeza dessas linhas cabe ao `flush` de `TransactionTestCase`, com banco de ensaio configurado. A execução do teardown [nao verificado]. O relógio de negócio está congelado, inclusive na devolução, e `hoje` chega explicitamente. O worker tem `lock_timeout` de 4s e o `Future` aguarda 15s. Esse timeout não cancela o worker. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:30–58,110–111,133–144,298–327` · `app/config/settings/ci.py:6–17`

O teste inverso assertou por código PIDs distintos, as quatro classes de SQL em ordem, `atomic`, `ASC`, uma tentativa vencedora, duas batidas e uma trilha por batida. O worker instala o contexto retornado, mas chama diretamente a unidade. `BrokenBarrierError` agora vira `AssertionError` com causa e escapa pelo `Future`. Um `order_by('-pk')` real nas quatro tabelas deve falhar por análise estática. Retirar simplesmente a ordenação explícita de Pergunta pode preservar o resultado porque `Meta.ordering = ['id']`. Execução e mutação [nao verificado]. O risco de inversão com flip permanece no código: flip adquire Batida e pode pedir Colaborador no reconciliador, enquanto plantio detém Colaborador e pode atualizar a mesma Batida reutilizada. Deadlock observado [nao verificado]. `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:41–47,63–132` · `app/chamados/models.py:2875–2903,3813` · `app/ponto/management/commands/flip_automatico.py:62–79` · `app/ponto/services/flip_batida.py:25–33` · `app/ponto/registro_batida.py:264–274` · `app/ponto/signals.py:206–218` · `app/chamados/reconciliador.py:635–646,1486–1487` · `app/chamados/services/materializacao.py:1418,1586–1590,2145–2146`

## 21/11 18:34 · sessao 109c7d33a892 · E4C-F5-CONSTROI · construtor · origem=Code

Corrigi `_slot_esperado` para usar `texto_sem_carimbo(p)` na guarda do legado de hora. A catraca continua exigindo exatamente uma leitura declarada no slot, a do veto temporal. Reproduzi as cinco falhas da base. Após a cura, passaram os 47 testes do recorte local sem banco. [chamados/juizes.py:374](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/juizes.py:374) · [chamados/juizes.py:411](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/juizes.py:411) · [chamados/tests/test_e3_catraca_respondedores.py:544](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/tests/test_e3_catraca_respondedores.py:544) [nao verificado]

### 1. O que o selo mede

`(2, 1)` significa **duas ocorrências medidas pelo censo AST e uma declarada**. A chave identifica a função. O censo registra cada ocorrência classificada como `DECLARADO`, agrupa por `arquivo::função` e compara o total exato com `LEITURAS_DECLARADAS`. `chamados/tests/test_e3_catraca_respondedores.py:81–85`, `:393–395`, `:422–430`, `:444–458`.

### 2. Por que a cura ficou no código

O delta foi **`1 → 2`**. O censo de `93ab82d08` encontrou o veto temporal em `_lido.respondida_em`. O de `aeb326255` encontrou também `p.respondida_em is None`, acrescentado na guarda do legado. A declaração existente cobre a pergunta temporal. A segunda leitura veio de outra pergunta: se há texto de hora com o instante físico ausente. `93ab82d08:app/chamados/juizes.py:374` · `chamados/juizes.py:374`, `:409–416` · `chamados/tests/test_e3_catraca_respondedores.py:67–69`.

O `r.txt` fornecido exige a ausência do instante físico para usar o texto legado na busca da batida. `texto_sem_carimbo(p)` aplica essa condição junto com a presença de texto, sobre a própria instância. O C21 segue julgando o par composto, e o juiz de fato exige a testemunha. Em memória, a batida determinou o resultado com o legado elegível. Preencher só a coluna física bloqueou o uso do texto mesmo com o instante composto vazio. O texto integral da FICHA e do aval está [nao verificado]. `chamados/juizes.py:371–382`, `:409–416`, `:860–863`, `:1587–1603` · `chamados/tests/test_fechar_disputa_materializada.py:38–52`.

### 3. As quatro falhas `MORDE`

| Teste | Causa | [nao verificado]
|---|---|
| `leitura_booleana_nova_DENTRO_de_um_declarado` | Injeta um teste de nulo no slot. O censo sobe de dois para três e devolve `(3, 1)`, enquanto a prova espera `(2, 1)`. `chamados/tests/test_e3_catraca_respondedores.py:588–595` |
| `volta4_a_corpo_diferido_nao_e_uso_de_VALOR` | O `isoformat()` na lambda diferida deixa o teste de nulo externo contar. A injeção também produz `(3, 1)`. `chamados/tests/test_e3_catraca_respondedores.py:792–798` |
| `volta4_b_disjuncao_no_ORM_nao_e_guarda` | O lookup de ordem no outro ramo do `\|` deixa o teste `isnull` contar. Dentro do slot, a injeção produz `(3, 1)`. `chamados/tests/test_e3_catraca_respondedores.py:339–357`, `:824–839` |
| `volta4_c_compensacao_entre_declarados` | Adiciona uma leitura no slot e retira uma no modelo. A troca conserva o total de partida, que já era seis, e falha na asserção de total cinco. `chamados/tests/test_e3_catraca_respondedores.py:866–880` |

### Diff

```diff
--- a/app/chamados/juizes.py
+++ b/app/chamados/juizes.py
@@ -411 +411 @@
-    carimbo_legado = (materializada is not None and not via and p.respondida_em is None
+    carimbo_legado = (materializada is not None and not via and texto_sem_carimbo(p)
--- a/app/chamados/tests/e3_c8b_apoio.py
+++ b/app/chamados/tests/e3_c8b_apoio.py
@@ -117 +117 @@
-                 'texto_vazio', 'pergunta_respondida', 'ausencia_parcial_cobra_o_resto',
+                 'texto_vazio', 'texto_sem_carimbo', 'pergunta_respondida', 'ausencia_parcial_cobra_o_resto',
```

### Porta do Code e prova RED

```bash
bash bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-constroi-141636 --only chamados --espera 7200 -- -k test_e3_catraca_respondedores
```

Da pasta `app`, este comando executa o selo e os `MORDE` com as injeções existentes. Na base, o módulo deu cinco falhas em 18 testes. Com a cura, os 18 passaram no recorte local. `chamados/tests/test_e3_catraca_respondedores.py:544–574`, `:588–595`, `:792–880`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e3_catraca_respondedores
```

O mutante abaixo restaura a leitura crua em memória. O próprio selo fica vermelho na igualdade de contagem, com `(2, 1)` e `rc 1`. `chamados/juizes.py:411` · `chamados/tests/test_e3_catraca_respondedores.py:556`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import unittest
from unittest.mock import patch
from chamados.tests import test_e3_catraca_respondedores as selo

arquivo = 'chamados/juizes.py'
fontes = selo.production_sources()
antes = 'and texto_sem_carimbo(p)'
assert fontes[arquivo].count(antes) == 1
fontes[arquivo] = fontes[arquivo].replace(antes, 'and p.respondida_em is None', 1)

teste = selo.ContadorRespondedoresTest('test_os_quatro_declarados_pela_pergunta') [nao verificado]
with patch.object(selo, 'production_sources', return_value=fontes):
    resultado = unittest.TextTestRunner().run(unittest.TestSuite([teste])) [nao verificado]
raise SystemExit(2 if resultado.errors else int(not resultado.wasSuccessful()))
PY
```

O recorte local mais amplo encontrou dois erros de importação de `django.utils` nos subcasos do teste de hash legado. Reproduzi ambos com a fonte da base. A execução ORM e a certificação pelo Code e pelo Opus final estão [nao verificado]. `chamados/tests/test_e3_pos_suite_sem_banco.py:175` · `core/fmt.py:8`.

**Autoconferência — except/fallback novos no diff: 0, esperado 0.** `chamados/juizes.py:411` · `chamados/tests/e3_c8b_apoio.py:117`.

## 21/11 18:28 · sessao 24f049b85777 · FERIADO-POR-POSTO · leitura · origem=Code

**Leitura do código: a releitura é preexistente na porta da célula, mas a marca do posto ganha um caminho até ela na cópia.** A linha 464 é idêntica nas duas árvores. No main, o comando grava a marca do posto e pode retratar chamados. Na cópia, ele também propaga a chave aos vínculos e, por essa propagação, pode julgar uma célula que ainda é futura para o `hoje` do ato. A igualdade no caminho comum, por si só, não demonstra igualdade no caminho da marca. `app/ponto/portas/celula.py:460–478` (ambas) · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–149` (main) · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:117–126` e `app/colaboradores/services/feriado_do_posto.py:343–354` (cópia)

Conferi as duas árvores locais e compilei em memória o texto do teste abaixo. O Python disponível em ambas falhou ao importar `freezegun`, declarado em `app/requirements.txt:43`. A execução Django, as linhas `PAREADA:` efetivamente emitidas e o efeito em produção ficam **[nao verificado]**.

## 1. Caminhos até a releitura

| Salto | Main `ab351aa9b` | Cópia `b95a383d6` |
|---|---|---|
| `Command.handle` → `_marcar` | `handle` chama `_marcar(o, w)`. `_marcar` lê `localdate()` para a janela de feriados usada na seleção dos chamados. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:33–37,57–67,93–116` | `handle` lê `agora`, deriva `hoje` e passa os dois a `_marcar`. Sugestões recebem `hoje` e leem a presença. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:41–50,75` · `app/colaboradores/services/feriado_do_posto.py:74–97,137–144` |
| `_marcar` → escrita ou prévia | O dry-run retorna. `--apply` chama `marcar_opera_em_feriado`, que grava só o campo do posto, e percorre o alvo da retratação. O corpo termina aí, sem chamada à porta do vínculo. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–149` · `app/colaboradores/services/estrutura.py:83–86` | `--apply` chama `aplicar_marca(hoje=...)`. O dry-run passa `hoje` e `agora` a `previa_da_marca`, que, somente na sombra, chama a mesma `aplicar_marca` com `hoje` dentro do savepoint. `agora` segue para a classificação da prévia. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:117–126` · `app/colaboradores/services/feriado_do_posto.py:370–385,397–406` |
| `aplicar_marca` → `marcar_trabalha_em_feriado` | O serviço da marca ainda não existe nessa árvore. A porta do vínculo já existe, com `hoje` lido dentro dela. O chamador de produção localizado é `sincronizar_feriado_admin --apply`. `app/colaboradores/services/feriado_do_posto.py:146–158` · `app/colaboradores/services/vinculo.py:869,890–914` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53–57` | O plano seleciona vínculos vigentes em `hoje`. Sob a trava do posto, a propagação reconfirma a marca e o vínculo divergente e chama `marcar_trabalha_em_feriado(hoje=...)`. `reaplicar_marca` também delega à mesma porta. `app/colaboradores/services/feriado_do_posto.py:203–219,283–285,307–315,343–359` |
| Porta do vínculo → regeneração | Usa o `hoje` que acabou de ler nos dois ramos. Regenera até `hoje` diretamente e chama `regenerar_futuras_do_cadastro` para o restante. `app/colaboradores/services/vinculo.py:895–914` | Usa o `hoje` recebido da marca nos mesmos dois ramos. A leitura local fica para os chamadores que omitem o argumento, como o sincronizador. `app/colaboradores/services/vinculo.py:905–925` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:68–72` |
| Regeneração → linha 464 → cartório | Em ambas, o suplemento seleciona células **geradas já existentes** com `data > hoje` e chama `regenerar_celulas_vinculo` para cada dia, com esse mesmo `hoje`. A porta usa o dia recebido para desfazer o julgamento anterior da futura reescrita. Em seguida chama `_o_chao_do_dia_mudou` sem `hoje`. Havendo dias tocados e recálculo, a linha 464 relê o dia e a linha 472 despacha ao cartório. | Mesmos arquivos e linhas nas duas árvores. `app/ponto/services/cadastro_futuro.py:8–25,43–59` · `app/ponto/portas/celula.py:112–114,193–199,306–318,390–394,460–478` · `app/ponto/services/cartorio.py:2059–2089` |

A retratação de `--fecha` tem outro caminho nas duas árvores. `transicionar_com_pauta` chama `reconciliar`. Se o fechamento passar pelo gate, `_gravar_estado` salva o chamado e o `post_save` pode chamar `cartorio.julgar_celula` diretamente. Esse salto contorna a linha 464 e não carrega o dia da marca. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:139–147` (main) · `app/colaboradores/services/feriado_do_posto.py:325–341` (cópia) · `app/chamados/services/fecho_runtime.py:146–169`, `app/chamados/models.py:654–668,286–302` e `app/chamados/signals.py:189–218` (ambas)

## 2. `app/colaboradores/tests/test_pareada_hoje_injetado.py`

Um único método mede o comando real com a virada após `sugestoes` e, em outro vínculo, o caminho comum mais próximo com `hoje` explícito. No comando, a chamada direta a `handle(**opções)` usa a assinatura das duas árvores e fornece os campos que a cópia lê. No caminho comum, a única escrita direta da chave prepara a entrada da fixture. A seleção das futuras, a regeneração e o julgamento rodam pelo código real. A observação separa datas enviadas pela chamada forçada da linha 472 de datas para as quais o cartório devolveu resultado. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:33–37,60–62` (main), `:41–50,117–126` (cópia) · `app/colaboradores/tests/test_feriado_a5_volta3.py:243–266` (cópia) · `app/ponto/services/cadastro_futuro.py:43–59`, `app/ponto/portas/celula.py:460–478` e `app/ponto/services/cartorio.py:2073–2102` (ambas)

| Símbolo importado | Main | Cópia |
|---|---|---|
| `datetime`, `StringIO`, `TestCase`, `User` | `app/colaboradores/tests/test_feriado_admin_sync.py:1–5` | Mesmas linhas |
| `mock`, `freeze_time` | `app/colaboradores/tests/test_relogio_riscado_calendario.py:27–30` · `app/requirements.txt:43` | Mesmas linhas |
| `timezone` | `app/ponto/tests/test_porta_celula.py:9` | Mesma linha |
| `Command` | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:19,33` | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:23,41` |
| `Empresa`, `Praca`, `Posto`, `Colaborador`, `Municipio` | `app/colaboradores/models.py:40,177,219,283,962` | Mesmas linhas |
| `feriado_do_posto` | `app/colaboradores/tests/test_feriado_do_posto.py:108` · `app/colaboradores/services/feriado_do_posto.py:136` | `app/colaboradores/tests/test_feriado_do_posto.py:108` · `app/colaboradores/services/feriado_do_posto.py:137` |
| `Feriado` | `app/core/models.py:5` | Mesma linha |
| `CelulaDia`, `EscalaColaborador`, `TipoEscala` | `app/escala/models.py:1635,871,24` | `app/escala/models.py:1646,871,24` |
| `celula` | `app/ponto/tests/test_porta_celula_dia.py:147` · `app/ponto/portas/celula.py:112` | Mesmas linhas |
| `cartorio` | `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:178–182` · `app/ponto/services/cartorio.py:2059` | Mesmas linhas |
| `regenerar_futuras_do_cadastro` | `app/ponto/services/cadastro_futuro.py:28` | Mesma linha |

```python
import datetime
from io import StringIO
from unittest import mock

from django.contrib.auth.models import User
from django.test import TestCase
from django.utils import timezone
from freezegun import freeze_time

from colaboradores.management.commands.marcar_feriado_do_posto import Command
from colaboradores.models import Colaborador, Empresa, Municipio, Posto, Praca
from colaboradores.services import feriado_do_posto
from core.models import Feriado
from escala.models import CelulaDia, EscalaColaborador, TipoEscala
from ponto.portas import celula
from ponto.services import cartorio
from ponto.services.cadastro_futuro import regenerar_futuras_do_cadastro


D = datetime.date(2026, 12, 16)
UM_DIA = datetime.timedelta(days=1)
HOJE = D - UM_DIA
DIAS = {HOJE, D, D + UM_DIA}


def datas(dias):
    return [dia.isoformat() for dia in sorted(dias)]


class PareadaHojeInjetadoTest(TestCase):
    def _cenario(self, empresa, municipio, praca, tipo, nome, cpf, matricula):
        posto = Posto.objects.create(
            nome=nome, empresa=empresa, municipio=municipio, praca=praca,
            opera_em_feriado=True,
        )
        colab = Colaborador.objects.create(
            nome=nome, cpf=cpf, matricula_hasner=matricula, empresa=empresa,
            posto=posto, situacao='ativo', data_admissao=HOJE, cargo='Vigilante',
        )
        ec = EscalaColaborador.objects.create(
            colaborador=colab, posto=posto, tipo_escala=tipo, ativa=True,
            data_inicio=HOJE, trabalha_em_feriado=True,
        )
        self.assertEqual(celula.regenerar_celulas_vinculo(
            ec, HOJE, max(DIAS), hoje=HOJE, estender_horizonte=False, recalculo=False,
        ), len(DIAS))
        self.assertEqual(
            dict(self._dias(ec).values_list('data', 'trabalha')),
            dict.fromkeys(DIAS, True),
        )
        return posto, ec

    def _dias(self, ec):
        return CelulaDia.objects.filter(
            colaborador_id=ec.colaborador_id, data__in=DIAS, origem='gerada',
        )

    def _observar(self, nome, ec, ato):
        enviados = set()
        julgados = set()
        julgar_real = cartorio.julgar_celula

        def julgar(colab, dia, **opcoes):
            alvo = colab.pk == ec.colaborador_id and dia in DIAS and opcoes.get('forcar')
            if alvo:
                enviados.add(dia)
            resultado = julgar_real(colab, dia, **opcoes)
            if alvo and resultado is not None:
                julgados.add(dia)
            return resultado

        with mock.patch.object(cartorio, 'julgar_celula', side_effect=julgar), \
                mock.patch.object(celula, 'regenerar_celulas_vinculo',
                                  wraps=celula.regenerar_celulas_vinculo) as regenerar:
            ato()

        alterados = set(self._dias(ec).filter(trabalha=False).values_list('data', flat=True))
        hoje_porta = sorted({c.kwargs['hoje'].isoformat() for c in regenerar.call_args_list})
        print(
            f'PAREADA: {nome} hoje_porta={hoje_porta} alterados={datas(alterados)} '
            f'enviados={datas(enviados)} julgados={datas(julgados)} '
            f'pulados={datas(alterados - enviados)} intactos={datas(DIAS - alterados)}',
            flush=True,
        )
        return alterados, enviados, julgados

    def test_hoje_da_borda_governa_a_regeneracao(self):
        with timezone.override('America/Sao_Paulo'), freeze_time(
            '2026-12-15 23:59:00-03:00',
        ) as relogio:
            User.objects.get_or_create(username='sistema')
            empresa = Empresa.objects.create(
                razao_social='Pareada', cnpj='19131243000359', ativa=True,
                dia_inicio_competencia=21,
            )
            municipio, _ = Municipio.objects.get_or_create(
                codigo_ibge=4113700, defaults={'nome': 'Londrina', 'uf': 'PR'},
            )
            praca = Praca.objects.create(nome='Pareada', cidade='Londrina', estado='PR')
            tipo = TipoEscala.objects.create(
                codigo='PAREADA-5X2', descricao='Pareada 5x2', tipo_ciclo='5x2',
                tipo_base='comercial', hora_inicio=datetime.time(8),
                hora_fim=datetime.time(17), folga_dia_semana='5,6',
            )
            for dia in sorted(DIAS):
                Feriado.objects.get_or_create(
                    data=dia, abrangencia='nacional', uf='', municipio='',
                    defaults={'nome': 'Feriado sintetico', 'fonte': 'teste'}, [nao verificado]
                )
            posto, ec_marca = self._cenario(
                empresa, municipio, praca, tipo, 'Marca', '[cpf]', 993001,
            )
            _, ec_comum = self._cenario(
                empresa, municipio, praca, tipo, 'Comum', '[cpf]', 993002,
            )
            self.assertEqual(timezone.localdate(), HOJE)

            sugerir_real = feriado_do_posto.sugestoes

            def sugerir_e_virar_dia(**opcoes):
                resultado = sugerir_real(**opcoes)
                relogio.tick(delta=datetime.timedelta(minutes=2))
                self.assertEqual(timezone.localdate(), D)
                return resultado

            def marcar():
                Command(stdout=StringIO()).handle(
                    sugestoes=True, posto=posto.pk, fecha=True, opera=False,
                    por='sistema', apply=True, motivo='Prova pareada de relogio',
                    retomar_retratacao=False,
                )

            with mock.patch.object(feriado_do_posto, 'sugestoes',
                                   side_effect=sugerir_e_virar_dia) as sugestoes:
                marca = self._observar('marca', ec_marca, marcar)

            sugestoes.assert_called_once()
            self.assertEqual(timezone.localdate(), D)
            posto.refresh_from_db()
            self.assertIs(posto.opera_em_feriado, False)

            def regenerar():
                EscalaColaborador.objects.filter(pk=ec_comum.pk).update(trabalha_em_feriado=False)
                ec_comum.refresh_from_db()
                celula.regenerar_celulas_vinculo(
                    ec_comum, HOJE, HOJE, hoje=HOJE, estender_horizonte=False,
                )
                regenerar_futuras_do_cadastro(ec_comum, hoje=HOJE)

            comum = self._observar('comum', ec_comum, regenerar)

        self.assertEqual(comum[0], DIAS, 'a prova precisa reescrever os tres dias')
        for nome, (alterados, enviados, julgados) in (('marca', marca), ('comum', comum)):
            with self.subTest(caminho=nome):
                self.assertEqual(julgados, enviados, 'o cartorio precisa concluir o julgamento enviado')
                self.assertEqual(
                    julgados, alterados & {HOJE},
                    'Clock Injection: D e D+1 ainda sao futuros para o ato',
                )
```

## 3. Como decidir pela saída

Com `hoje=15/12` e relógio congelado em `16/12` na regeneração, a regra permite julgar **15/12** e manda pular **16/12 e 17/12**, mesmo quando as três células são reescritas. O cenário usa `FECHA` para que as células reescritas virem folga e a guarda pelo fim do turno não mascare a releitura de `16/12`. `app/docs/PLANO-PADROES.md:97–103` (ambas) · `app/escala/models.py:1369–1395` (main), `:1380–1406` (cópia) · `app/ponto/services/cartorio.py:2073–2089` (ambas)

| Linha | Main, previsão pela leitura | Cópia, previsão pela leitura |
|---|---|---|
| `PAREADA: comum` | `hoje_porta=15/12`. Altera os três dias, julga `15/12` e `16/12`, pula `17/12`. | Mesma previsão. `app/ponto/services/cadastro_futuro.py:43–59` · `app/ponto/portas/celula.py:306–318,390–394,460–478` (ambas) |
| `PAREADA: marca` | `hoje_porta=[]`. Os três dias ficam intactos, sem despacho pela regeneração. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–149` (main) | `hoje_porta=15/12`. Altera os três dias, julga `15/12` e `16/12`, pula `17/12`. `app/colaboradores/services/feriado_do_posto.py:343–354` · `app/colaboradores/services/vinculo.py:905–925` (cópia) · `app/ponto/portas/celula.py:460–478` (ambas) |

- **MESMO na porta compartilhada:** as duas linhas `comum` coincidem, com regeneração efetiva dos três dias, `15/12` julgado como controle positivo e o mesmo destino para `16/12` e `17/12`. Se ambas julgarem `16/12`, a violação é preexistente nessa porta. `app/ponto/services/cadastro_futuro.py:43–59` · `app/ponto/portas/celula.py:460–478` (ambas)
- **NOVO/PIORA no caminho da marca:** `16/12` fica intacto no main e é julgado na cópia na linha `marca`, embora `hoje_porta` registre `15/12`. Esse par demonstra que o ato da marca passou a expor a releitura preexistente. Se a divergência aparecer também na linha `comum`, com `16/12` pulado no main e julgado na cópia, a piora aparece também no caminho compartilhado. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124–149` (main) · `app/colaboradores/services/feriado_do_posto.py:343–354` (cópia) · `app/ponto/portas/celula.py:460–478` (ambas)

Para o item 2 do aval, o resultado previsto é **mesmo defeito na porta comum e exposição nova no ato da marca**. A prova pareada executada continua **[nao verificado]**.

## 21/11 18:25 · sessao e6269bff1d70 · E4C-F5-CONSTROI · construtor · origem=Code

A falha da volta 2 vem da simulação. O teste agora confirma a devolução em outra conexão antes de o passivo adquirir a trava. O diff da volta 3 está em dois arquivos de teste. [guarda_plantio_s144.py:298](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:298) · [cortes_f2_f9_apoio.py:37](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/cortes_f2_f9_apoio.py:37) [nao verificado]

### Respostas

1. **Por que o par volta a `(True, True)`.** Na base, o gancho chamava `devolver_validacao_recusada` na própria conexão do passivo, já dentro do `atomic()` da competência. A devolução zerava `validada_em` e `validada_por` e inseria `DEVOLUCAO` em savepoints aninhados. Depois da trava, a composta relida diferia da foto classificada. O `raise` saía do `atomic()` da competência e desfazia a limpeza e a inserção. O `except` externo contabilizava `escopo_do_plantio_mudou`. Assim, a leitura do ato e a coluna voltavam a mostrar a validação original. Em `f3e48a445`, o fluxo seguia da trava direto para `_trabalhar_o_par`. `1d0b5058b:app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:96` · `f3e48a445:app/chamados/services/passivo_validado.py:385` · [validacao.py:284](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/validacao.py:284) · [models.py:4437](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:4437) · [passivo_validado.py:383](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:383) · [passivo_validado.py:397](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:397) · [models.py:4399](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:4399)

2. **Onde está o defeito.** O rollback da competência alcançava a devolução injetada porque ambas as escritas pertenciam àquela transação. Com o outro ato já confirmado, a releitura ocorre após a trava e a divergência encerra a tentativa antes de `_trabalhar_o_par`. A captura altera apenas o resultado em memória. O commit da outra conexão fica preservado. [passivo_validado.py:361](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:361) · [passivo_validado.py:389](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:389) [nao verificado]

3. **O que fica gravado.** Pela regra, `validada_em` e `validada_por_id` ficam nulos, `recusa_motivo` continua `'Recusa anterior do plantio'` e há três decisões. A fixture grava `RESPOSTA` e `VALIDACAO`. A devolução acrescenta `DEVOLUCAO` e copia o motivo preexistente para `ato_motivo`. Nesse cenário, a fixture é quem preenche `recusa_motivo`. `_carimbar_recusa` é o escritor desse campo no caminho de materialização, que a recusa de escopo antecede. [guarda_plantio_s144.py:52](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:52) · [models.py:4313](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:4313) · [validacao.py:287](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/validacao.py:287) · [materializacao.py:1733](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/materializacao.py:1733) · [passivo_validado.py:392](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:392)

### Diff e RED

- [cortes_f2_f9_apoio.py:37](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/cortes_f2_f9_apoio.py:37) extrai a criação da empresa e do colaborador para reuso entre `TestCase` e `TransactionTestCase`.
- [guarda_plantio_s144.py:101](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:101) intercala a devolução antes da trava real. O RED usa um worker com relógio congelado e `atomic(durable=True)`. O passivo espera o worker retornar após o commit. [guarda_plantio_s144.py:305](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:305) [nao verificado]
- A asserção de uma devolução bem-sucedida agora confere `True` dentro do worker e o PK exato da decisão depois do rollback do passivo. O RED exige o balde `escopo_do_plantio_mudou`, zero plantio, zero tentativa de carimbo e as três decisões. Em seguida, a mesa valida e planta pelo mesmo PK. [guarda_plantio_s144.py:113](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:113) · [guarda_plantio_s144.py:138](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:138) · [guarda_plantio_s144.py:143](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:143) · [guarda_plantio_s144.py:311](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:311)

### Porta para o Code

O recorte foi reconstruído a partir do anexo e do delta da volta 2. A seleção estática encontrou três testes do passivo e oito do plantio concorrente. Execução pela porta e veredito [nao verificado]. [guarda_plantio_s144.py:159](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:159) · [guarda_plantio_s144.py:317](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:317) · [plantio_concorrente.py:98](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:98) · [plantio_concorrente.py:189](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:189) [nao verificado]

```bash
wt=/home/ronald/wt-codex-e4c-f5-constroi-160129
bash bin/suite.sh --dir "$wt" --only chamados -- \
  -k '*test_e4c_f5_guarda_plantio_s144.*.test_RED_passivo*' \
  -k '*test_e4c_f5_plantio_concorrente.*.test_RED_*' \
  -k '*test_e4c_f5_plantio_concorrente.*.test_MORDE_*' \
  -k test_duas_materializacoes_inversas_travam_o_mesmo_par_em_ordem
```

### Mutantes para o Code

- **M1, devolução na conexão do passivo.** Reintroduz a simulação da volta 2. A mordida esperada é `(True, True)` na leitura após o rollback. [guarda_plantio_s144.py:126](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:126)
- **M2, worker sem commit.** O worker executa a devolução e faz rollback. O RED deve acusar que o passivo deixou de recusar pelo escopo mudado. [guarda_plantio_s144.py:114](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:114) · [guarda_plantio_s144.py:313](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:313) [nao verificado]
- **M3, P3 sem reconfirmação.** Mantém a devolução confirmada e suprime a recusa da foto velha. O RED deve acusar o desvio no resultado do passivo. [passivo_validado.py:392](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:392) · [guarda_plantio_s144.py:114](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:114)

Os três mutantes têm âncora única e compilam em memória. Mordidas pela porta [nao verificado]. O bloco abaixo restaura o arquivo após cada chamada.

```bash
red=test_RED_passivo_foto_velha_recusa_escopo_e_mesa_valida_mesmo_pk
teste=chamados/tests/test_e4c_f5_guarda_plantio_s144.py [nao verificado]

mutar() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$wt" "$red" "$@" <<'PY'
from pathlib import Path
import subprocess
import sys

wt, red, rel, antes, depois = sys.argv[1:]
arquivo = Path(wt) / 'app' / rel
original = arquivo.read_bytes()
assert original.count(antes.encode()) == 1, rel
mutado = original.replace(antes.encode(), depois.encode())
compile(mutado, str(arquivo), 'exec')
try:
    arquivo.write_bytes(mutado)
    resultado = subprocess.run(
        ['bash', 'bin/suite.sh', '--dir', wt, '--only', 'chamados', '--', '-k', red] [nao verificado]
    )
finally:
    arquivo.write_bytes(original)
raise SystemExit(resultado.returncode)
PY
}

# M1
mutar "$teste" \ [nao verificado]
  'return executor.submit(self._devolver_em_outra_transacao, p.pk).result(timeout=15)' \
  $'with freeze_time(DEVOLUCAO_EM):\n                    atual = PerguntaDisputa.objects.get(pk=p.pk)\n                    self.assertIs(validacao.devolver_validacao_recusada(atual, self.admin), True)\n                    return atual.decisoes.get(ato=D.DEVOLUCAO).pk'

# M2
mutar "$teste" 'return devolucao.pk' \ [nao verificado]
  $'transaction.set_rollback(True)\n                return devolucao.pk'

# M3
mutar chamados/services/passivo_validado.py \
  'if DecisaoPergunta.composta(atuais[p.pk]) != estados_classificados[p.pk]:' \
  'if False:'
```

**LEI-AKITA** `origem=validacao.py:289` · `testemunha=models.py:4393` · `quem-mais-le=passivo_validado.py:390` · `RED=test_e4c_f5_guarda_plantio_s144.py:317` · `juizes novos=0` · `padrao=Single Writer Principle`

**AUTOCONFERÊNCIA** · except/fallback novos no diff: **0**. `git diff | grep -n except` sem ocorrências. O `finally` novo fecha a conexão do worker e deixa o erro subir. [guarda_plantio_s144.py:305](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:305)

## 21/11 18:13 · sessao 82fd2448f20e · E4C-F5-CONSTROI · construtor · origem=Code

Apliquei S1, S2, S2b e as duas curas de S3 na cópia `93ab82d08`. As provas em memória passaram com a cura, e os oito mutantes falharam por asserção. O recorte sem banco de C8b e do veto temporal passou com 29 testes. Execução ORM e suíte certificadora [nao verificado]. `chamados/tests/test_e3_c8b_sem_banco.py:369` · `chamados/tests/test_e3_pos_suite_sem_banco.py:103`

## 1. Alterações e mordidas

| Item | Alteração e mutante que o teste mata | [nao verificado]
|---|---|
| **S1** | Removi a captura da consulta de conflito. O teste exige `DatabaseError`, a mesma instância injetada e uma chamada a `.exists()`. Restaurar a conversão para `LOTE_B` falha no `assertRaises`. `chamados/services/validacao.py:1230` · `chamados/tests/test_fila_validar_lote.py:110` |
| **S2a** | O explicador recebe uma foto com `REABERTURA` na sequência e texto ainda na coluna. O teste exige `sem resposta vigente` e verifica a ausência da data e hora antigas. Acrescentar `p.resposta_colab` à mensagem falha no `assertNotIn`. `chamados/services/explicador.py:61` · `chamados/tests/test_explicador_disputa.py:80` |
| **S2, vínculo** | A pergunta tem `SEM_RESPOSTA_VIGENTE` e uma batida viva vinculada às 08h, com destino ausente. Deixar esse motivo passar pela guarda reabre o atalho do vínculo e muda o veredito para `True`. `chamados/juizes.py:799` · `chamados/juizes.py:805` · `chamados/tests/test_marco_por_motivo.py:231` |
| **S2, destino** | A pergunta tem `SEM_RESPOSTA_VIGENTE` e destino resolvido, com vínculo ausente. Deixar esse motivo passar pela guarda reabre o atalho do destino e muda o veredito para `True`. `chamados/juizes.py:799` · `chamados/juizes.py:827` · `chamados/tests/test_marco_por_motivo.py:245` |
| **S2, retratação** | Sob `SEM_RESPOSTA_VIGENTE`, carimbo mais trilha absolvem na leitura avulsa e na carga. Os controles com apenas uma das duas testemunhas reprovam. Trocar o retorno antecipado por `False` derruba o controle positivo. `chamados/juizes.py:293` · `chamados/juizes.py:800` · `chamados/tests/test_marco_por_motivo.py:253` |
| **S2b** | A fixture afirma `PLANTIO_SEM_TS` antes de cada julgamento da lei. Trocar esse motivo por `DATA_HORA_INVALIDA` deixa a asserção vermelha mesmo quando os dois motivos seguem o mesmo caminho no juiz de fato. `chamados/juizes.py:419` · `chamados/tests/test_hx_fallback_sem_lei.py:62` |

## 2. S3 — origem, cura e mutantes

**(i) Passivo com foto velha.** A linha do delta é `chamados/services/vigia_carimbo.py:104`, commit `93ab82d08`. O `prefetch_related('decisoes')` passou a acompanhar a instância entregue ao passivo. A devolução gravada entre classificação e trava entra por outra instância. O refresh parcial do passivo atualiza o carimbo e a via, e a composta ainda encontra a sequência antiga na instância classificada. É uma causa de classe de **reuso da foto após a classificação**. `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:95` · `chamados/services/passivo_validado.py:382` · `chamados/services/passivo_validado.py:538` · `chamados/models.py:4361` · `chamados/models.py:4446`

A cura mantém o carregamento em lote e descarta `decisoes` em cache antes de devolver a tripla. Remover esse descarte reproduziu `((True, True), True, 'validar', True, None)` em memória. Com a cura, a leitura reproduziu `((False, True), True, 'validar', False, 'par_ato_coluna')`. O teste existente também exige a recusa no placar e a validação posterior pela mesa no mesmo PK. `chamados/services/vigia_carimbo.py:104` · `chamados/services/vigia_carimbo.py:122` · `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:111` · `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:132`

**(ii) Fechamento com uma pergunta já materializada.** A linha do delta é `chamados/juizes.py:410` em `93ab82d08`. A foto de `hora` passou a entregar texto vazio ao derivador quando o C21 recusava o par. A fixture tem `07:00`, carimbo de materialização e uma batida `E` às 07h, com os instantes de resposta e validação vazios. O texto vazio leva a `PLANTIO_SEM_TS`, o juiz de fato perde a comparação com a batida, e a pergunta volta viva ao gate. É uma causa de classe de **leitura do legado de hora no fechamento**. Nos recortes em memória, as duas regressões passaram em `c8eb8eddf` e `41d21a249` e falharam em `93ab82d08`. `chamados/tests/test_fechar_disputa_materializada.py:38` · `chamados/services/materializacao.py:695` · `chamados/juizes.py:1537` · `chamados/services/acoes_disputa.py:134` · `chamados/models.py:2035`

A cura usa o texto legado para localizar a batida somente no ramo `hora`, com carimbo visível no horizonte, via vazia, instante físico de resposta ausente e validação ausente na coluna e na composta. O C21 continua exigindo o par, e a materialização ainda exige testemunha. O mutante que restaura `resposta if vigente else ''` deixou **uma pergunta viva** e o C7 devolveu `False` em memória. A asserção ORM correspondente exige `fechada_em` preenchido. `chamados/juizes.py:411` · `chamados/juizes.py:860` · `chamados/juizes.py:1603` · `chamados/tests/test_fechar_disputa_materializada.py:64`

## 3. Quem escreve e quem lê

| Campo ou valor | Escrita e leitura |
|---|---|
| `resposta_colab`, `respondida_em`, `validada_em` e sequência de atos | A porta de resposta escreve o par. A resposta automática também o escreve, e a recuperação pela trilha completa o instante. A validação grava `VALIDACAO`, a devolução grava `DEVOLUCAO`, e a reabertura registra o ato e limpa par e carimbo pela porta em lote. `DecisaoPergunta.registrar` persiste as linhas. `leitura` ordena e aplica os atos, `composta` combina instantes do ato com texto da coluna, e slot, C21, lote e explicador consomem essa leitura. `chamados/services/disputa_emissao.py:1499` · `chamados/services/disputa_emissao.py:1664` · `chamados/services/disputa_emissao.py:1709` · `chamados/services/disputa_emissao.py:1732` · `chamados/services/validacao.py:168` · `chamados/services/validacao.py:287` · `chamados/models.py:4361` · `chamados/models.py:4388` · `chamados/models.py:4438` · `chamados/juizes.py:371` · `chamados/services/validacao.py:1224` · `chamados/services/explicador.py:52` |
| `materializada_em`, `via_resolucao`, `agora` e `slots[pk]` | `_escrever_carimbo` atribui carimbo e via. A projeção lê ambos e aplica o horizonte. A tarefa fornece `agora`, a carga copia o horizonte e grava resultado ou exceção por PK, e o slot lê o cache. O C6 lê a materialização projetada antes de consultar o juiz de fato. `chamados/models.py:3324` · `chamados/juizes.py:1434` · `chamados/juizes.py:1525` · `chamados/services/tarefa_do_dia.py:525` · `chamados/services/carga_pergunta_viva.py:88` · `chamados/services/carga_pergunta_viva.py:119` · `chamados/juizes.py:383` |
| Identidade da pergunta e insumos do derivador | O nascimento fornece `tipo_resposta`, `motivo`, `disputa` e destino. `gravar_dia_do_fato` atribui `data_evento`, e a costura pode atualizar o destino. A transição do chamado atribui `status_local`. O slot lê a identidade para montar a foto, o derivador lê também o contexto da disputa e o marco, e o juiz de fato lê o estado do destino. `chamados/services/disputa_emissao.py:448` · `chamados/services/fio.py:853` · `chamados/services/fio.py:978` · `chamados/models.py:240` · `chamados/juizes.py:413` · `chamados/services/materializacao.py:556` · `chamados/services/materializacao.py:625` · `chamados/juizes.py:827` |
| Batida e trilha de retratação | `registrar_batida` grava instante e tipo, o materializador anota `pergunta_origem`, e `retratar_batida` grava `retratada_em`. Juiz e vigia leem o vínculo vivo, o juiz lê a janela e o lote consulta `batidas_apuraveis`. A chave de auditoria `retratar_pergunta_obsoleta` é gravada pela fixture e lida pelo juiz avulso e pela carga. Escritor de produção dessa chave literal [nao verificado]. `ponto/registro_batida.py:177` · `ponto/registro_batida.py:294` · `chamados/services/materializacao.py:1578` · `chamados/juizes.py:803` · `chamados/juizes.py:860` · `chamados/services/vigia_carimbo.py:117` · `chamados/services/validacao.py:1230` · `chamados/tests/test_marco_por_motivo.py:262` · `chamados/juizes.py:296` · `chamados/services/carga_pergunta_viva.py:110` |
| `slot.motivo`, `slot.ts`, `slot.tipo`, classe e detalhe | `_slot_esperado` produz o resultado. Leem o juiz de fato, a carga, o lote, o explicador e o vigia. `classe_do_lote` produz classe e detalhe. `classificar` produz `(pergunta, classe, ts)` e o consomem o passivo e os dois vigias. `chamados/juizes.py:360` · `chamados/juizes.py:798` · `chamados/services/carga_pergunta_viva.py:119` · `chamados/services/validacao.py:1226` · `chamados/services/explicador.py:52` · `chamados/services/vigia_carimbo.py:112` · `chamados/services/vigia_carimbo.py:123` · `chamados/services/vigia_carimbo.py:131` · `chamados/services/passivo_validado.py:330` · `chamados/services/vigia_fio.py:415` |
| Cache de `decisoes`, `recusa_do_carimbo` e `fechada_em` | O prefetch do vigia preenche o cache, `leitura` o consome e `classificar` o descarta antes do `yield`. `carimbar` lê o par e grava a recusa em memória, que o passivo traduz em balde. C6 e C7 alimentam o fechamento e `DisputaSupervisao.fechar` grava `fechada_em`. `chamados/services/vigia_carimbo.py:104` · `chamados/models.py:4361` · `chamados/services/vigia_carimbo.py:122` · `chamados/models.py:3388` · `chamados/services/passivo_validado.py:145` · `chamados/models.py:2035` · `chamados/models.py:2118` |

## 4. Censo S1

`git grep -n -E 'classe_do_lote\(' -- api chamados/services` encontrou três consumidores. Dependência específica de `LOTE_B` **na falha** encontrada no censo estático: zero. O executor recusa qualquer classe diferente de A, a fila agrupa a classe devolvida e a API serializa a classe. No `93ab82d08`, os três consumiam genericamente o B produzido pela captura, e `git grep -n 'CONSULTA_CONFLITO_INDISPONIVEL' 93ab82d08 -- '*.py'` encontrou apenas o produtor. `chamados/services/validacao.py:1085` · `chamados/services/validacao.py:1277` · `api/views_mensageria.py:546` · `93ab82d08:app/chamados/services/validacao.py:1234`

## Comandos para o Code

O executor abaixo carrega cada mutante em memória. Os comandos ORM abaixo seguem [nao verificado]. Os alvos são as asserções de S1, S2, S2b e S3 listadas acima.

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings_test "$@"
}

mutante() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
from importlib.machinery import SourceFileLoader
from pathlib import Path
import runpy
import sys

arquivo, teste, antes, depois = sys.argv[1:] [nao verificado]
alvo = Path(arquivo).resolve()
get_code = SourceFileLoader.get_code

def com_mutante(loader, nome):
    if Path(loader.path).resolve() != alvo:
        return get_code(loader, nome)
    fonte = alvo.read_text()
    if fonte.count(antes) != 1:
        raise ValueError(f'{alvo}: trecho do mutante não é único')
    return compile(fonte.replace(antes, depois), loader.path, 'exec')

SourceFileLoader.get_code = com_mutante
sys.argv = ['manage.py', 'test', '--settings=config.settings_test', teste] [nao verificado]
runpy.run_path('manage.py', run_name='__main__')
PY
}

t=chamados.tests
j=chamados/juizes.py
v=chamados/services/validacao.py
e=chamados/services/explicador.py
c=chamados/services/vigia_carimbo.py

s1=$t.test_fila_validar_lote.FilaValidarLoteTest.test_RED_lote_propaga_falha_da_consulta_de_conflito
s2a=$t.test_explicador_disputa.ExplicadorDisputaTest.test_RED_explicador_nao_cita_texto_residual_como_resposta_vigente
marco=$t.test_marco_por_motivo.MarcoPorMotivoTest
vinculo=$marco.test_RED_sem_resposta_vigente_nao_absolve_so_pelo_vinculo
destino=$marco.test_RED_sem_resposta_vigente_nao_absolve_so_pelo_destino_encerrado
trilha=$marco.test_RED_sem_resposta_vigente_preserva_retratacao_com_carimbo_e_trilha
s2b=$t.test_hx_fallback_sem_lei.FallbackSemLeiTest
s3i=$t.test_e4c_f5_guarda_plantio_s144.GuardaPlantioS144Test.test_RED_passivo_foto_velha_recusa_par_e_mesa_valida_mesmo_pk
s3ii=$t.test_fechar_disputa_materializada.FecharDisputaPerguntaMaterializadaTest.test_pergunta_materializada_nao_bloqueia_fechamento

prova "$s1" "$s2a" "$vinculo" "$destino" "$trilha" "$s2b" "$s3i" "$s3ii"

mutante "$v" "$s1" \
  '    if batidas_apuraveis(perg.disputa.colaborador, slot.ts - perto, slot.ts + perto).exists():' \
  $'    from django.db import DatabaseError\n    try:\n        conflito = batidas_apuraveis(perg.disputa.colaborador, slot.ts - perto, slot.ts + perto).exists()\n    except DatabaseError:\n        return LOTE_B, dict(d, porque="CONSULTA_CONFLITO_INDISPONIVEL")\n    if conflito:'

mutante "$e" "$s2a" $' % motivo\n' $' % motivo + " " + p.resposta_colab\n'

guarda="if slot.motivo in ('VETO_TEMPORAL', 'SEM_RESPOSTA_VIGENTE'):"
mutante "$j" "$vinculo" "$guarda" \
  "if slot.motivo == 'VETO_TEMPORAL' or (slot.motivo == 'SEM_RESPOSTA_VIGENTE' and p.chamado_destino is not None):"
mutante "$j" "$destino" "$guarda" \
  "if slot.motivo == 'VETO_TEMPORAL' or (slot.motivo == 'SEM_RESPOSTA_VIGENTE' and p.chamado_destino is None):"
mutante "$j" "$trilha" \
  "            return _retratada_com_trilha(p) if carga is None else carga.retratada(p)" \
  '            return False'

mutante "$j" "$s2b" "return _ResultadoSlot('PLANTIO_SEM_TS')" "return _ResultadoSlot('DATA_HORA_INVALIDA')"
mutante "$c" "$s3i" $'        getattr(p, \'_prefetched_objects_cache\', {}).pop(\'decisoes\', None)\n' ''
mutante "$j" "$s3ii" 'resposta if vigente or carimbo_legado' 'resposta if vigente'
```

## 21/11 18:09 · sessao c8af26ea4af5 · MEDIDA-FOLHA-[nome] · sonda · origem=Code

## Item (d) — segunda medida

| Chave da saída | Valor devolvido | Referência no código |
|---|---:|---|
| `gravado.badge_chamados` | 1420 | `app/core/context_processors.py:84,110-111` |
| `gravado.lavrado_em` | 21/11 18:06:10 | `app/core/context_processors.py:110-111` |
| `agora` | 21/11 18:08:29 | Relógio informado na saída fornecida. Linha da sonda [nao verificado]. |
| `vivo.exige_o_admin` | 1420 | `app/chamados/services/verbos.py:22-27` |
| `vivo.arquivado.n` | 136 | `app/chamados/services/painel.py:229-231,284-292` |
| `vivo.cobrar.n` | 2553 | `app/chamados/services/painel.py:229-231,284-292` |
| `vivo.decidir.n` | 712 | `app/chamados/services/painel.py:229-231,284-292` |
| `vivo.historico.n` | 8 | `app/chamados/services/painel.py:229-231,284-292` |
| `vivo.validar.n` | 708 | `app/chamados/services/painel.py:229-231,284-292` |

**1. O badge gravado bate com o vivo.** Na saída fornecida, `1420 = 708 + 712`. O carimbo tinha **2 min 19 s** de idade e a diferença era **0**. A navbar lê `PlacarNavbar.badge_chamados`. A lavra e a medida viva aplicam `exige_o_admin` às contagens de `montar_contexto_painel` sem filtros. A unidade é o chamado vivo classificado como Validar ou Decidir. Cobrar, Arquivado e Histórico ficam fora da soma. A composição por verbo na hora da lavra e as variações durante o intervalo ficam [nao verificado]. `app/core/context_processors.py:68-84,110-111` · `app/core/management/commands/lavrar_badge_navbar.py:39-41,72-74,91-93` · `app/chamados/services/painel.py:153-155,229-231` · `app/chamados/services/verbos.py:22-27,44-56`.

**2. Laudo versus sonda.** Pelos valores fornecidos, Validar foi de **707 para 708** (+1), Decidir de **705 para 712** (+7) e o badge de **1413 para 1420** (+7). No laudo, as pílulas somavam **1412**, um abaixo do badge. Na sonda, a soma e o badge fecham em **1420**. O código permite que as pílulas usem um recorte filtrado e que o badge venha de uma lavra anterior. Os filtros efetivos, as fontes que abasteceram as pílulas, o `lavrado_em` do laudo e a causa daquele 1 ficam [nao verificado]. `app/chamados/views.py:727-739,813-819,840-861` · `app/chamados/services/painel.py:117-130,229-231` · `app/core/context_processors.py:68-84`.

**3. Sim, há caminhos com fontes diferentes mesmo sem filtro.**

- **No GET inicial**, o cabeçalho renderiza `badge_chamados` da lavra, quando disponível, e as pílulas renderizam `verbos_contagens` calculadas ao vivo pelo GET. O recorte sem filtro segue a mesma regra de classificação em leituras de instantes distintos. `app/core/context_processors.py:68-84,154` · `app/chamados/views.py:727-739,767` · `app/templates/chamados/painel_gestao.html:42,79,191`.
- **Na atualização por JSON**, sem filtro e com `payload_placar`, as pílulas passam a ler o payload lavrado. Sem payload, a rota calcula ao vivo. O JavaScript atualiza as pílulas e mantém no cabeçalho o valor do render inicial. Se houver uma lavra entre o GET e a atualização, cabeçalho e pílulas podem exibir lavras distintas. `app/chamados/views.py:840-862` · `app/templates/chamados/painel_gestao.html:191,302-325`.
- **Se o cálculo vivo da navbar falhar**, o último fallback conta `qs_na_fila()`, que usa status de cobrança e exclui silenciados no futuro. Esse ramo tem outro critério. Sua ocorrência no laudo fica [nao verificado]. `app/core/context_processors.py:87-93` · `app/chamados/services/painel.py:188-190` · `app/chamados/services/verbos.py:22-27`.

A confirmação independente do hash da cópia e do isolamento da transação em produção fica [nao verificado].

## 21/11 18:07 · sessao c38aa6f720a9 · MEDIDA-FOLHA-[nome] · sonda · origem=Code

**Na sonda, “Espelho pendente” soma 207 em outubro e 298 em novembro na prévia. Em novembro, a `emp2` tem a única defasagem observada entre o retrato gravado e a prévia. Na Central, a diferença de 1 é aritmética confirmada pelo aval, e a igualdade de universo e instante exigida para provar pílula dissonante está [nao verificado].** `C.<competência>.<empresa>.furo_espelho_colabs`, `E.2026-11.emp2.retrato_gravado_x_previa.mudaram`, `E.2026-11.emp2.retrato_em_memoria_x_previa.mudaram`, `aval.txt:1`, `app/chamados/services/painel.py:230`, `app/core/context_processors.py:68`

Nos quadros, a primeira coluna dá o prefixo `P = competência.empresa`. Cada legenda indica como compor a chave da saída para cada número. As referências ao aval e à linha do log apontam para os anexos fornecidos. O carimbo de lavratura das fotos lidas está [nao verificado].

## (a) Termômetro R1

O E6 compara o espelho com o oráculo por dia-colab e atribui um dono a cada divergência. O universo é o de empresas ativas com fechamentos na competência, sem filtro de aptidão para o TXT. A comparação exclui dia sem linha, isento ou com ausência, sem valor lavrado, zero dos dois lados ou diferença explicada pela pausa declarada. Diferenças de até 10 minutos contam como concordância. `app/ponto/management/commands/e6_oraculo.py:303`, `app/ponto/management/commands/e6_oraculo.py:316`, `app/ponto/management/commands/e6_oraculo.py:331`, `app/ponto/management/commands/e6_oraculo.py:354`, `app/ponto/management/commands/e6_oraculo.py:366`, `app/ponto/management/commands/e6_oraculo.py:378`

Chave `A.P.<coluna>`. Nas colunas dos donos, `dias/colabs` corresponde a `A.P.<dono>.dias` e `A.P.<dono>.colabs`. A união conta pessoas distintas e as duas últimas colunas mostram a sobreposição exata. `app/ponto/management/commands/e6_oraculo.py:378`, `app/ponto/management/commands/e6_oraculo.py:401`

| P | `universo_colabs` | `dias_divergentes` | `ESTRUTURA` dias/colabs | `CADASTRO` dias/colabs | `BATIDA` dias/colabs | `uniao_colabs` | `em_exatamente_2_donos` | `em_exatamente_3_donos` |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| 2026-10.emp1 | 4 | 4 | 0/0 | 0/0 | 4/1 | 1 | 0 | 0 |
| 2026-10.emp2 | 477 | 265 | 18/10 | 68/28 | 179/94 | 117 | 11 | 2 |
| 2026-10.emp3 | 119 | 40 | 6/5 | 7/3 | 27/16 | 21 | 1 | 1 |
| 2026-10.emp4 | 24 | 9 | 2/1 | 0/0 | 7/6 | 7 | 0 | 0 |
| 2026-11.emp1 | 3 | 4 | 1/1 | 0/0 | 3/1 | 1 | 1 | 0 |
| 2026-11.emp2 | 460 | 288 | 24/16 | 63/30 | 201/101 | 128 | 15 | 2 |
| 2026-11.emp3 | 115 | 92 | 11/7 | 16/7 | 65/22 | 28 | 8 | 0 |
| 2026-11.emp4 | 24 | 9 | 1/1 | 0/0 | 8/5 | 6 | 0 | 0 |

## (b) Prévia e retrato × donos do R1

A prévia usa `classificar_export` sobre os fechamentos da empresa e competência, sem filtro de status. O retrato lê as linhas já gravadas. A ponte conta colabs por competência. As colunas dos donos são interseções com o R1 e podem se sobrepor. `app/folha/views.py:350`, `app/folha/export.py:782`, `app/folha/retrato_folha.py:98`, `app/ponto/management/commands/e6_oraculo.py:305`, `app/ponto/management/commands/e6_oraculo.py:378`

Cada célula segue a ordem **`no_r1 / sem_divergencia_r1 / ESTRUTURA / CADASTRO / BATIDA / so_no_leitor`**. A chave é `B.P.<leitor>.<coluna>.<métrica>`. Nas linhas sem leitor indicado, o valor vale para as duas chaves, `previa` e `retrato`.

| P | Leitor | `universo` | `entram` | `fora_txt` | `furo_espelho` | `sem_codigo_dominio` | `ferias_com_batida` |
|---|---|---|---|---|---|---|---|
| 2026-10.emp1 | Ambos | 4/3/0/0/1/0 | 0/0/0/0/0/0 | 4/3/0/0/1/0 | 2/1/0/0/1/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |
| 2026-10.emp2 | Ambos | 477/360/10/28/94/0 | 244/196/4/9/41/0 | 233/164/6/19/53/0 | 173/106/6/19/51/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |
| 2026-10.emp3 | Ambos | 119/98/5/3/16/0 | 82/71/1/0/11/0 | 37/27/4/3/5/0 | 24/17/3/2/4/0 | 1/1/0/0/0/0 | 1/1/0/0/0/0 |
| 2026-10.emp4 | Ambos | 24/17/1/0/6/0 | 14/12/0/0/2/0 | 10/5/1/0/4/0 | 8/4/1/0/3/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |
| 2026-11.emp1 | Ambos | 3/2/1/0/1/0 | 0/0/0/0/0/0 | 3/2/1/0/1/0 | 2/1/1/0/1/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |
| 2026-11.emp2 | `previa` | 460/332/16/30/101/0 | 191/157/5/4/28/0 | 269/175/11/26/73/0 | 240/146/11/26/73/0 | 1/1/0/0/0/0 | 1/1/0/0/0/0 |
| 2026-11.emp2 | `retrato` | 460/332/16/30/101/0 | 192/158/5/4/28/0 | 268/174/11/26/73/0 | 239/145/11/26/73/0 | 1/1/0/0/0/0 | 1/1/0/0/0/0 |
| 2026-11.emp3 | Ambos | 115/87/7/7/22/0 | 51/47/0/1/3/0 | 64/40/7/6/19/0 | 51/28/7/6/18/0 | 0/0/0/0/0/0 | 1/1/0/0/0/0 |
| 2026-11.emp4 | Ambos | 24/18/1/0/5/0 | 16/14/1/0/1/0 | 8/4/0/0/4/0 | 5/1/0/0/4/0 | 1/1/0/0/0/0 | 0/0/0/0/0/0 |

A barra do retrato separa `bloqueado` de `modulo_proprio`, que recebe as rescisões na janela. A prévia inclui essas rescisões em `fora_txt`. Mesma ordem de métricas, com chave `B.P.retrato.<coluna>.<métrica>`. `app/folha/retrato_folha.py:27`, `app/folha/retrato_folha.py:89`, `app/folha/views.py:351`

| P | `bloqueado` | `modulo_proprio` |
|---|---|---|
| 2026-10.emp1 | 3/2/0/0/1/0 | 1/1/0/0/0/0 |
| 2026-10.emp2 | 174/107/6/19/51/0 | 59/57/0/0/2/0 |
| 2026-10.emp3 | 26/19/3/2/4/0 | 11/8/1/1/1/0 |
| 2026-10.emp4 | 9/5/1/0/3/0 | 1/0/0/0/1/0 |
| 2026-11.emp1 | 3/2/1/0/1/0 | 0/0/0/0/0/0 |
| 2026-11.emp2 | 241/147/11/26/73/0 | 27/27/0/0/0/0 |
| 2026-11.emp3 | 52/29/7/6/18/0 | 12/11/0/0/1/0 |
| 2026-11.emp4 | 6/2/0/0/4/0 | 2/2/0/0/0/0 |

- **Mesmo universo de pessoas, sim**, em cada linha do quadro. Em ambos os leitores, `B.P.<leitor>.r1_sem_linha = 0` e `B.P.<leitor>.universo.so_no_leitor = 0`. Para cada `P`, `B.P.empresa_no_seletor = 1` e `B.P.retrato_presente = 1`. `app/ponto/management/commands/e6_oraculo.py:305`, `app/folha/export.py:782`, `app/folha/retrato_folha.py:98`
- **Mesma competência, sim.** R1 e prévia foram lidos na mesma rodada. A leitura do retrato nessa rodada usa a foto gravada, cujo `lavrado_em` está [nao verificado]. A coincidência de datas individuais entre o dia divergente do R1 e o dia que reteve na folha também está [nao verificado]. Em outubro, a igualdade acima é dos agregados da ponte. `app/ponto/janelas.py:54`, `app/ponto/management/commands/e6_oraculo.py:284`, `app/folha/fora_da_folha.py:146`, `app/folha/models.py:356`, `app/folha/retrato_folha.py:1237`

## (c) Espelho pendente por motivo diário e rota

A sonda abre o detalhe diário dos retidos por `furo_espelho` no universo comum com o R1. O leitor respeita a janela do vínculo e as dispensas por isenção, desligamento e vereditos dispensados. A unidade do detalhe é dia-colab, com pessoas distintas por célula da tabela. Uma pessoa pode aparecer em vários motivos ou rotas. `app/folha/fora_da_folha.py:115`, `app/folha/fora_da_folha.py:146`, `app/folha/export.py:1205`, `app/folha/export.py:1238`, `app/folha/export.py:1264`, `app/folha/export.py:1293`, `app/folha/retrato_folha.py:1024`

| P | `furo_espelho_colabs` | `retidos_sem_dia_detalhado` |
|---|---:|---:|
| 2026-10.emp1 | 2 | 0 |
| 2026-10.emp2 | 173 | 0 |
| 2026-10.emp3 | 24 | 0 |
| 2026-10.emp4 | 8 | 0 |
| 2026-11.emp1 | 2 | 0 |
| 2026-11.emp2 | 240 | 0 |
| 2026-11.emp3 | 51 | 0 |
| 2026-11.emp4 | 5 | 0 |

O quadro acima usa `C.P.<coluna>`. No próximo, cada par é `dias/colabs`. A chave do Total é `C.P.<motivo>.<dias ou colabs>`. Nas outras colunas é `C.P.<motivo>.<rota>.<dias ou colabs>`. `—` indica chave ausente no bloco fornecido. As rotas são `chamados:modal_fio` para o fio, `colaboradores:calendario_ponto` para Resolver dia e `ponto:painel_ausencia` para Decidir ausência. A sonda identificou as rotas publicadas. A abertura e a resolução pela tela estão [nao verificado]. `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1016`, `app/chamados/urls.py:39`, `app/colaboradores/urls.py:24`, `app/ponto/urls.py:70`

| P | Motivo | Total | Fio | Calendário | Ausência | **`sem_porta`** | **`sem_leitura`** |
|---|---|---:|---:|---:|---:|---:|---:|
| 2026-10.emp1 | `espelho_furo` | 1/1 | — | 1/1 | — | — | — |
| 2026-10.emp1 | `espelho_nunca_bateu` | 14/1 | 10/1 | — | — | **4/1** | — |
| 2026-10.emp1 | `sem_celula` | 24/2 | — | 15/2 | — | **8/2** | **1/1** |
| 2026-10.emp2 | `espelho_cobrado` | 212/68 | 206/65 | 1/1 | 5/4 | — | — |
| 2026-10.emp2 | `espelho_cobranca_indevida` | 5/1 | — | 2/1 | — | **3/1** | — |
| 2026-10.emp2 | `espelho_discordante` | 13/10 | 2/2 | 7/7 | — | — | **4/2** |
| 2026-10.emp2 | `espelho_fato_em_ausencia` | 17/15 | 3/3 | 11/10 | — | **1/1** | **2/2** |
| 2026-10.emp2 | `espelho_furo` | 197/84 | 179/78 | 14/8 | 4/2 | — | — |
| 2026-10.emp2 | `espelho_indefinida` | 173/40 | 153/32 | 5/1 | — | **13/8** | **2/1** |
| 2026-10.emp2 | `espelho_nunca_bateu` | 157/6 | 96/6 | — | 1/1 | **60/4** | — |
| 2026-10.emp3 | `espelho_cobrado` | 50/16 | 50/16 | — | — | — | — |
| 2026-10.emp3 | `espelho_discordante` | 3/1 | 2/1 | 1/1 | — | — | — |
| 2026-10.emp3 | `espelho_furo` | 58/13 | 58/13 | — | — | — | — |
| 2026-10.emp3 | `espelho_indefinida` | 16/7 | 12/6 | — | — | **1/1** | **3/1** |
| 2026-10.emp3 | `espelho_nunca_bateu` | 30/1 | 15/1 | — | — | **15/1** | — |
| 2026-10.emp4 | `espelho_cobrado` | 5/4 | 5/4 | — | — | — | — |
| 2026-10.emp4 | `espelho_fato_em_ausencia` | 1/1 | — | 1/1 | — | — | — |
| 2026-10.emp4 | `espelho_furo` | 6/5 | 6/5 | — | — | — | — |
| 2026-11.emp1 | `espelho_cobrado` | 1/1 | 1/1 | — | — | — | — |
| 2026-11.emp1 | `sem_celula` | 41/2 | — | 22/2 | — | **19/2** | — |
| 2026-11.emp2 | `espelho_cobrado` | 505/128 | 484/121 | 17/9 | 3/3 | — | **1/1** |
| 2026-11.emp2 | `espelho_cobranca_indevida` | 11/6 | 9/4 | — | — | **2/2** | — |
| 2026-11.emp2 | `espelho_discordante` | 12/9 | 5/5 | 7/4 | — | — | — |
| 2026-11.emp2 | `espelho_fato_em_ausencia` | 12/9 | 5/3 | 7/6 | — | — | — |
| 2026-11.emp2 | `espelho_furo` | 222/117 | 206/111 | 12/6 | 1/1 | **1/1** | **2/2** |
| 2026-11.emp2 | `espelho_indefinida` | 250/58 | 227/52 | 1/1 | — | **22/7** | — |
| 2026-11.emp2 | `espelho_nunca_bateu` | 119/9 | 51/7 | 68/9 | — | — | — |
| 2026-11.emp3 | `espelho_cobrado` | 103/30 | 103/30 | — | — | — | — |
| 2026-11.emp3 | `espelho_discordante` | 4/2 | 3/1 | 1/1 | — | — | — |
| 2026-11.emp3 | `espelho_fato_em_ausencia` | 1/1 | — | 1/1 | — | — | — |
| 2026-11.emp3 | `espelho_furo` | 80/30 | 79/29 | 1/1 | — | — | — |
| 2026-11.emp3 | `espelho_indefinida` | 34/11 | 34/11 | — | — | — | — |
| 2026-11.emp3 | `espelho_nunca_bateu` | 15/1 | 7/1 | 8/1 | — | — | — |
| 2026-11.emp4 | `espelho_cobrado` | 7/3 | 5/3 | 2/1 | — | — | — |
| 2026-11.emp4 | `espelho_furo` | 5/3 | 5/3 | — | — | — | — |

| Motivo | O que retém o dia |
|---|---|
| `sem_celula` | Dia passado com vínculo sem célula, ou célula com previsão definida e sem veredito. `app/folha/export.py:1264`, `app/folha/export.py:1293` |
| `espelho_furo` | Furo integral ou parcial com veredito ainda acusador. `app/escala/models.py:1612`, `app/escala/models.py:1767` |
| `espelho_cobrado` | A cobrança foi reconhecida e o dia segue acusado. `app/ponto/services/cartorio.py:1534`, `app/escala/models.py:1767` |
| `espelho_cobranca_indevida` | Cobrança em dia sem trabalho ou sem furo. `app/escala/models.py:1613` |
| `espelho_discordante` | Batida órfã fora da tolerância, realizado inflado ou realizado zero com turno. `app/escala/models.py:1616` |
| `espelho_fato_em_ausencia` | Trabalho em dia coberto por ausência. `app/escala/models.py:1617`, `app/escala/models.py:1754` |
| `espelho_indefinida` | Dia previsto indefinido, fase por declarar ou ausência parcial sem período. `app/escala/models.py:1615` |
| `espelho_nunca_bateu` | Falta de adesão ao ponto em dia de trabalho sem cobertura integral. `app/ponto/services/cartorio.py:1312`, `app/ponto/services/cartorio.py:1318` |

**Achado de desenho para o BO:** as células `sem_porta` registram ocorrências em que o motivo reteve o dia e o mapa deixou o admin sem rota publicada para a ocorrência. `sem_leitura` registra separadamente os dias sem rota em que o fio ficou `nao_sei`. `C.P.<motivo>.sem_porta.{dias,colabs}`, `C.P.<motivo>.sem_leitura.{dias,colabs}`, `app/folha/fora_da_folha.py:86`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1024`

| Competência | Aval `aval.txt:1` | Prévia `Σ C.P.furo_espelho_colabs` | Retrato `Σ B.P.retrato.furo_espelho.no_r1` |
|---|---:|---:|---:|
| 2026-10 | 207 | 2 + 173 + 24 + 8 = **207** | 2 + 173 + 24 + 8 = **207** |
| 2026-11 | 298 | 2 + 240 + 51 + 5 = **298** | 2 + 239 + 51 + 5 = **297** |

As somas usam `emp1` a `emp4` na competência da linha. Em outubro, o 207 bate nos dois leitores. Em novembro, o 298 bate na prévia e o retrato fica um abaixo, na `emp2`. `C.2026-11.emp2.furo_espelho_colabs`, `B.2026-11.emp2.retrato.furo_espelho.no_r1`, `aval.txt:1`

## (d) Central

| Contador | Valor citado | Fonte, unidade e universo |
|---|---:|---|
| **Validar** | 707 · `aval.txt:1` | `verbos_contagens.validar.n`. Conta chamados vivos classificados nesse verbo, dentro do recorte efetivo da Central. `app/templates/chamados/painel_gestao.html:42`, `app/chamados/services/painel.py:198`, `app/chamados/services/painel.py:230`, `app/chamados/services/verbos.py:30` |
| **Decidir** | 705 · `aval.txt:1` | `verbos_contagens.decidir.n`. Conta chamados do mesmo recorte classificados no verbo Decidir. O juiz atribui um verbo por chamado e remapeia os silenciados. `app/templates/chamados/painel_gestao.html:79`, `app/chamados/services/painel.py:230`, `app/chamados/services/verbos.py:46` |
| **Pra decidir** | 1413 · `aval.txt:1` | `badge_chamados` de `PlacarNavbar`. O cron computa Validar + Decidir para o tenant sem filtros e grava a soma com o `payload_placar` da mesma rodada. `app/templates/chamados/painel_gestao.html:191`, `app/core/context_processors.py:68`, `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/core/management/commands/lavrar_badge_navbar.py:91` |

- **Mesmo universo e mesmo instante.** Para o mesmo payload gravado, `badge_chamados = Validar + Decidir` por construção. O aval traz `707 + 705 = 1412`, contra `1413` no cabeçalho. A igualdade de universo e instante para esses valores está [nao verificado]. A diferença observada, portanto, **não prova pílula dissonante** no sentido exigido pelo aval. `aval.txt:1`, `app/chamados/services/verbos.py:22`, `app/chamados/services/verbos.py:25`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/core/management/commands/lavrar_badge_navbar.py:91`
- **Diferença de universo ou instante.** O código permite pílulas ao vivo com filtro e cabeçalho global lavrado. Mesmo sem filtro, a renderização inicial pode contar ao vivo enquanto o cabeçalho lê a lavra. O refresh sem filtros pode buscar as pílulas no payload lavrado. Se faltar lavra, o cabeçalho tenta o mesmo juiz ao vivo para o tenant inteiro e, em caso de exceção, usa `qs_na_fila().count()`, de outro recorte. Qual desses caminhos produziu a diferença de 1 está [nao verificado]. `app/chamados/views.py:727`, `app/chamados/views.py:840`, `app/chamados/views.py:849`, `app/templates/chamados/painel_gestao.html:302`, `app/core/context_processors.py:68`, `app/core/context_processors.py:87`, `app/chamados/services/painel.py:188`, `aval.txt:1`

## (e) Competência 11 — gravado × prévia × reconstrução em memória

O retrato gravado vem de `linhas_do_retrato`. A reconstrução usa `montar_linhas`, chamada pelo lavrador antes de persistir, e deriva `status` e `motivo` do mesmo classificador da prévia. A sonda compara presença e o par `status/motivo` por colab-competência, na mesma leitura e com relógio comum para os leitores atuais. Datas dos dias, ações e fios ficam fora dessa comparação. `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:1212`, `app/folha/retrato_folha.py:1222`, `app/folha/retrato_folha.py:1237`, `app/folha/fora_da_folha.py:146`, `app/folha/management/commands/lavrar_retrato_folha.py:65`, `app/folha/management/commands/lavrar_retrato_folha.py:77`

Cada célula segue a ordem **`so_esquerda / so_direita / mudaram`**, com chave `E.P.<coluna>.<métrica>`.

| P | `retrato_em_memoria_x_previa` | `retrato_gravado_x_previa` | `retrato_gravado_x_em_memoria` | Leitura |
|---|---:|---:|---:|---|
| 2026-11.emp1 | 0/0/0 | 0/0/0 | 0/0/0 | Coincidem no recorte medido |
| 2026-11.emp2 | 0/0/0 | 0/0/1 | 0/0/1 | **Defasagem** |
| 2026-11.emp3 | 0/0/0 | 0/0/0 | 0/0/0 | Coincidem no recorte medido |
| 2026-11.emp4 | 0/0/0 | 0/0/0 | 0/0/0 | Coincidem no recorte medido |

Na `emp2`, a mudança é de `entra` na foto para `fora` por `furo_espelho` nos dois leitores atuais. É defasagem do veredito gravado, com o mesmo conjunto de pessoas. A comparação em memória coincide com a prévia em todas as empresas nos campos medidos. A idade da defasagem e o evento que a produziu estão [nao verificado]. `E.2026-11.emp2.retrato_gravado_x_previa.entra.-__fora.furo_espelho`, `E.2026-11.emp2.retrato_gravado_x_em_memoria.entra.-__fora.furo_espelho`, `E.P.retrato_em_memoria_x_previa.{so_esquerda,so_direita,mudaram}`, `app/folha/models.py:356`

**Onde batem 302 e 303.** A tabela usa as chaves `B.P.<leitor>.<balde>.no_r1`. A última coluna aplica à prévia a regra de **Bloqueados** do retrato. Ela subtrai `retrato.modulo_proprio` de `previa.fora_txt` porque os pares de `status/motivo` preservam esse módulo na comparação E. A única mudança foi de `entra` para `fora.furo_espelho`. Pela igualdade memória × prévia nesses campos, a última coluna também é o valor derivado para a reconstrução em memória. `app/folha/retrato_folha.py:27`, `app/folha/retrato_folha.py:89`, `app/folha/views.py:351`, `E.P.retrato_em_memoria_x_previa.mudaram`, `E.2026-11.emp2.retrato_gravado_x_previa.entra.-__fora.furo_espelho`

| P | `retrato.bloqueado` | `retrato.modulo_proprio` | `retrato.fora_txt` | `previa.fora_txt` | Prévia/memória no recorte de Bloqueados |
|---|---:|---:|---:|---:|---:|
| 2026-11.emp1 | 3 | 0 | 3 | 3 | 3 − 0 = 3 |
| 2026-11.emp2 | 241 | 27 | 268 | 269 | 269 − 27 = 242 |
| 2026-11.emp3 | 52 | 12 | 64 | 64 | 64 − 12 = 52 |
| 2026-11.emp4 | 6 | 2 | 8 | 8 | 8 − 2 = 6 |
| Soma das chaves acima | **302** | **41** | **343** | **344** | **303** |

O **302** do aval bate com **Bloqueados** do retrato. O **303** bate com a prévia e a memória quando se aplica esse mesmo recorte. Se a referência for o contador literal **Retidos do TXT** da prévia, a sonda soma **344**. No recorte equivalente do retrato, `fora_txt` soma **343**. A tela, os filtros e o carimbo que deram origem aos números citados no aval estão [nao verificado]. `aval.txt:1`, `B.P.retrato.bloqueado.no_r1`, `B.P.retrato.modulo_proprio.no_r1`, `B.P.retrato.fora_txt.no_r1`, `B.P.previa.fora_txt.no_r1`, `app/folha/views.py:408`, `app/folha/retrato_folha.py:89`

## (f) Porta do export

**Linha fornecida, com o identificador da tentativa omitido.** `f_linhas_export.txt:1`

```text
2026-11-21 07:04:59,754 WARNING ponto.recusa_critica RECUSA-ACAO-CRITICA porta=porta_export […] objeto=emp3:2026-12:[…] tipo=erro motivo=antes=em_curso depois=concluido origem=cron […] os leitores discordam em 1 ponto(s)
```

- **Contagem e causa.** A linha fornecida corresponde a uma tentativa da `emp3`, competência **12/2026**, concluída pelo cron com resultado divergente e `n=1`. O ESTADO conta recusas dessa porta pela data local das linhas de `ponto.log` e dos rotados. O anexo sustenta uma ocorrência. O total nos arquivos completos e qual teste compôs o `1` estão [nao verificado]. `aval.txt:1`, `f_linhas_export.txt:1`, `bin/recusa_estado.py:60`, `app/core/recusa_critica.py:95`, `app/core/recusa_critica.py:108`, `app/core/recusa_critica.py:133`, `app/config/crons.py:1809`, `app/folha/porta_export.py:1235`, `app/folha/porta_export.py:1273`
- **Impacto.** A tentativa do cron mediu e gravou o carimbo. Com esse carimbo vigente, a porta retorna `DIVERGENTE` e o POST é recusado antes da montagem do TXT. Exportação anterior ou posterior dessa empresa e competência está [nao verificado]. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:218`, `app/folha/porta_export.py:1273`, `app/folha/porta_export.py:1388`, `app/folha/views.py:130`, `app/folha/views.py:270`

## (g) «Gerar TXT para conferir»

**Achado de desenho.** O GE

[truncado em 20000 de 21098 caracteres]

## 21/11 18:03 · sessao e19f8d4e02f8 · FERIADO-POR-POSTO · verificador · origem=Code

1. **Bloqueia — F5 corrige o registro do censo. O caminho da marca ainda relê o relógio.** `Command.handle` lê `timezone.now()` uma vez e passa `hoje = timezone.localdate(agora)` às sugestões, à marca e à prévia. A prévia também recebe `agora`. Esse é o dia civil da execução. O dia de cada chamado vem de `dias_do_fato`, e o dia de cada turno vem de `t.data_turno` entregue pelo pareador. A entrada em `B2_menos_B1` descreve corretamente a conversão da borda. Rodei o censo estático em memória: `CLASSIFICAR` bate integralmente, `LinhaTransporte.custo_mensal` continua na lista e os contadores com catraca continuam em 9 e 85. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:41–50,75,117–126` · `app/colaboradores/services/feriado_do_posto.py:248–253,449–459` · `app/core/medida_turno_do_par.py:217–226,432–440` · `app/core/tests/test_medida_turno_do_par.py:389–390,548–571`

   **Contraexemplo de meia-noite.** A marca começa às 23h59 de D. Uma célula gerada de D+1 muda na propagação, que ainda usa D e trata a célula como futura. Se o relógio virar antes de `_o_chao_do_dia_mudou`, `_hoje_j = timezone.localdate()` já vale D+1 e a mesma célula fica elegível para julgamento naquele ato. `julgar_celula` consulta o dia novamente e, conforme o ramo, consulta também o instante. O RED existente confere os dois argumentos `hoje` entregues à porta do vínculo. A releitura ocorre abaixo desse ponto, em conflito com a injeção de relógio da L-128. `app/colaboradores/services/feriado_do_posto.py:344–352` · `app/colaboradores/services/vinculo.py:905–924` · `app/ponto/services/cadastro_futuro.py:43–59` · `app/ponto/portas/celula.py:306–314,390–394,460–478` · `app/ponto/services/cartorio.py:2059–2089` · `app/colaboradores/tests/test_feriado_a5_volta3.py:244–266` · `app/docs/PLANO-PADROES.md:97–103`

   **Outros pontos fora dessa classificação.** O vigia A4 lê `localdate()` na própria borda. `presenca_em_feriado` lê `localdate()` quando faltam `hoje` e algum limite. `previa_da_marca` lê `now()` se chamada sem `agora`. `sincronizar_feriado_admin --apply` chama a porta do vínculo sem `hoje`, permitindo uma leitura por vínculo alterado. O censo B2 exige um operando explícito em `localdate` e não mede chamadas isoladas a `now()`. `app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:14–18` · `app/colaboradores/services/feriado_do_posto.py:74–89,378–385` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:68–71` · `app/colaboradores/services/vinculo.py:896–908` · `app/core/medida_turno_do_par.py:217–226`

2. **Fila — F1 cura o `except: pass` na origem.** Na saída normal, a prévia monta o resultado, lança `_DesfazAPrevia` dentro do `atomic` e devolve o dicionário no `except`, depois de sair do bloco transacional. Fora da sombra, levanta `PreviaForaDaSombra` antes da porta. Se `_classificar_previa` levantar `ValueError`, por exemplo, o `atomic` é desfeito e o erro sobe. No corpo da prévia, todo retorno é um dicionário e a única exceção consumida é a sentinela. A varredura estática de F1 retornou zero infrações novas. A mutação `pos_rollback` ainda compila e ainda move a classificação para depois do rollback. `app/colaboradores/services/feriado_do_posto.py:366–406` · `app/ponto/tests/test_contract_no_except_pass.py:50–78` · `app/colaboradores/tests/test_feriado_a5_volta3.py:197–229,323–328,372–379`

   Há supressão de erros **nas dependências**. Por exemplo, uma falha ao gravar `LogAuditoria` é registrada e consumida por `evento`, que tem savepoint próprio. A marca pode continuar e a prévia pode devolver o plano. Os testes da prévia conferem a foto do banco e os callbacks `on_commit`, inclusive no ramo da retratação. `app/core/observ.py:85–98,124–139` · `app/colaboradores/services/feriado_do_posto.py:318–323,397–406` · `app/colaboradores/tests/test_feriado_previa_da_marca.py:157–205,246–270`

3. **Fila — F2 resolve a identificação do modelo pela catraca. A ordem de aquisição permanece.** O mesmo `.get(pk=posto.pk)` avalia a consulta dentro de cada `atomic`. Na retratação, a aquisição de `Posto` precede o reconciliador, que chama `trava_na_ordem` para `Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa`. `Posto` fica fora desse conjunto. Reapliquei em memória a expressão antiga e a catraca acusou `?type`. Com `Posto` nomeado, a varredura da cópia retornou zero ocorrências. `app/colaboradores/services/feriado_do_posto.py:307–315,325–351` · `app/chamados/models.py:663–667,684–686,754–770,2816–2825,2852–2880` · `app/chamados/tests/test_e1_v7_catracas.py:70–72,116–132,385–390`

   A busca na cópia localizou o `select_for_update` explícito de `Posto` em `aplicar_marca`. Os outros escritores localizados adquirem a linha pelo `UPDATE`: `atualizar_posto`, `definir_municipio_posto` e `gravar_posto_ativo`. Por exemplo, a troca de município grava o posto antes de disparar a reavaliação dos vínculos. A troca de `type(posto)` por `Posto` conserva essa relação. Contenção efetiva e travas indiretas no banco ficam [nao verificado]. `app/colaboradores/services/feriado_do_posto.py:307–309` · `app/colaboradores/services/estrutura.py:53–86,98–107`

4. **Fila — F3/F4 corrigem os consumidores do rótulo e preservam a prova do universo.** Os testes continuam exigindo `1 | 1` para vínculo do dia no posto que fecha, com cadastro em outro posto, e `0 | 0` para o inverso. Além deles, o teste da casca lê o literal novo. O comando imprime a mesma linha no dry-run e no apply. A ocorrência localizada do literal antigo está na base histórica congelada da catraca C12, lida pelo verificador como código-fonte. A busca na cópia não encontrou consumidor do literal em documentação, outro comando ou template. Consumidores externos ficam [nao verificado]. `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83–101` · `app/colaboradores/tests/test_feriado_por_posto.py:629–639` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:132–152` · `app/chamados/tests/e4b_c12_base.json:13` · `app/chamados/tests/e4b_c12_verificar.py:13–18`

   O novo rótulo atende à LEI-AKITA 8: a saída imprime `len(plano['alvo'])`, produzido pela porta real. `alvo` contém os vivos do posto do dia do fato em feriado alcançado, quando o ato muda a marca para FECHA ou retoma explicitamente uma retratação. Um chamado vivo em feriado dá `1 | 0` numa operação OPERA. Numa operação FECHA ele pode entrar como candidato e a premissa ainda recusar a retratação. O teste R3 cobre essa recusa. `CLAUDE.md:35–36` · `app/colaboradores/services/feriado_do_posto.py:237–254,272–285,325–341,397–406` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:136–138` · `app/colaboradores/tests/test_feriado_por_posto.py:231–247`

5. **Fila — Escritores e leitores dos valores exercitados pelo delta.**

   | Campo ou valor | Quem escreve ou produz | Quem lê |
   |---|---|---|
   | `Posto.pk`, `nome`, `municipio_id`, `opera_em_feriado` | A criação e a edição do cadastro passam por `estrutura`. O backfill escreve `municipio`. `marcar_opera_em_feriado` grava a marca a pedido de `aplicar_marca`. `app/colaboradores/views.py:852–884` · `app/colaboradores/services/estrutura.py:43–45,67–79,90–107` · `app/colaboradores/services/feriado_do_posto.py:318–323` | A porta relê a linha pelo PK e usa município e marca no plano. A casca imprime PK, nome, município e marca. Sugestões, veto de cobrança, vigia A4, sincronizador e juiz do vínculo novo também leem a marca. `app/colaboradores/services/feriado_do_posto.py:47–71,124–139,188–200,222–234,269–285,307–315` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:111–117,132–147` · `app/ponto/management/commands/detectar_ausencias.py:624–630` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:50–61` · `app/escala/models.py:905–928` |
   | `EscalaColaborador.posto_id`, vigência, ciclo, cargo e `trabalha_em_feriado` | A porta de vínculo grava posto, vigência e template. No nascimento consulta o juiz da chave, e os copiadores chamam `chave_ao_nascer`. A troca da chave em vínculo existente passa por `marcar_trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:259–299,870–904,1118–1127,1145–1153` | Plano e A4 usam `vinculos_que_seguem_a_marca`. O plano usa `q_vinculo_cobre_a_data` e o predicado de ciclo e cargo. A grade, a precedência e o fechamento leem a chave por `feriado_vira_folga`. `app/colaboradores/services/feriado_do_posto.py:175–185,203–234,283–285` · `app/escala/alimentacao.py:219–225` · `app/escala/models.py:1380–1406` · `app/ponto/precedencia.py:377` · `app/ponto/services/fechamento.py:949` · `app/docs/JUIZES.md:114–118,142–146` |
   | `ChamadoColaborador.status_local`, `colaborador_id`, `modulo_origem`, dia do fato e feriado | O emissor entrega identidade e `contexto_json.data_turno` a `ChamadoColaborador.abrir`. O reconciliador passa a mudança de status a `_gravar_estado`. O cadastro de feriados grava data, alcance e nome. `app/ponto/management/commands/detectar_ausencias.py:694–710` · `app/chamados/models.py:207–235,654–668` · `app/core/views_config.py:41–89,144–153` | O plano usa `qs_vivos`/`VIVOS`, `dias_do_fato` e `vinculos_dos_dias` para achar o posto da data. `core.feriados` decide o alcance e fornece o nome. A casca lê o módulo para listar e agrupar o alvo. `app/colaboradores/services/feriado_do_posto.py:47–57,237–254,277–285,462` · `app/chamados/services/painel.py:135–155` · `app/chamados/juizes.py:175–208` · `app/escala/alimentacao.py:334–358` · `app/core/feriados.py:70–112` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:139–153` · `app/docs/JUIZES.md:62–65,127–130,142–146` |
   | Foto da célula, `data_turno`, batidas e `a/b/outras` | A porta da célula grava `trabalha`, `dna` e `escala_geradora`; a lavra escreve `ata`, `veredito` e `veredito_em`; `carimbar` grava `julgada_em` e `impressao`. O pareador produz `data_turno`, entrada, saída e batidas. `_classificar_previa` produz as três listas. `app/ponto/portas/celula.py:289–314,1089–1097,1181–1202` · `app/ponto/turnos.py:312–324` · `app/colaboradores/services/feriado_do_posto.py:420–475` | A prévia captura as fotos por `(colaborador_id, data)`, usa os juízes de vínculo, janela e turno e devolve as listas. A casca só as formata e conta. `app/colaboradores/services/feriado_do_posto.py:390–417,425–475` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:77–100,148–150` · `app/docs/JUIZES.md:20–25,114–121` |
   | `plano`, contagens, trilha e nota | `plano_da_marca` produz `marca_antes`, `marca_muda`, `candidatos`, `sem_dia`, `vivos`, `alvo` e `propagar`. `aplicar_marca` conta `retratados` e `propagados`. `evento` escreve autor, motivo e antes/depois na trilha; `nota.anexar_ocorrencia` grava a nota no fio após a retratação aceita. `app/colaboradores/services/feriado_do_posto.py:261–285,318–354` · `app/core/observ.py:94–102,133–138` · `app/chamados/services/nota.py:75–109` | A prévia lê `propagar` e `alvo` para definir quem fotografar. A casca lê o plano e as contagens para imprimir. A auditoria lê a trilha e os templates do fio leem os follow-ups. `app/colaboradores/services/feriado_do_posto.py:390–406` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:132–154` · `app/core/views.py:705` · `app/templates/chamados/fio_colaborador.html:235` |

6. **Fila — Outras classes, fora das mudanças do patch.**

   - **Segundo juiz.** O classificador da prévia usa entrada e saída presentes como condição para `(a) PASSAM A PAGAR`. O juiz registrado de par completo exige também dia encerrado. Contraexemplo: o pareador já devolveu entrada e saída às 16h55, a prévia roda às 16h56 e o turno previsto termina às 17h. A condição local aceita o par e o juiz ainda recusa o encerramento. A divergência está no classificador já presente, fora da linha alterada por F1. `app/colaboradores/services/feriado_do_posto.py:449–474` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:78–85` · `app/core/juizes.py:243–248` · `app/ponto/turnos.py:2113–2130,2185–2214`
   - **Segundo escritor fora da porta.** O delta conserva o escritor único da marca. Na chave do vínculo há uma janela concorrente anterior ao patch: o sincronizador pode selecionar um vínculo quando o posto está sem marca, a porta pode então gravar FECHA e propagar `False`, e o sincronizador pode em seguida gravar o `True` já calculado. Os dois usam a porta da chave. O sincronizador toma a decisão sobre a marca antes do lote de escritas, fora da trava do posto. `app/colaboradores/services/estrutura.py:90–95` · `app/colaboradores/services/feriado_do_posto.py:344–352` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:43–71` · `app/colaboradores/services/vinculo.py:896–904`
   - **Selo, trava e significado dos campos.** F1 e F2 preservam as catracas existentes, F3/F4 preservam os dois casos adversariais e F5 atualiza a publicação informativa, com a lacuna de relógio do item 1. O rótulo novo distingue a candidatura do efeito, e a marca do posto continua separada da chave efetiva do vínculo, inclusive nos casos em que a propagação é excluída por ciclo ou cargo. `app/ponto/tests/test_contract_no_except_pass.py:64–78` · `app/chamados/tests/test_e1_v7_catracas.py:387–415` · `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:91–101` · `app/core/tests/test_medida_turno_do_par.py:12–16,565–571` · `app/colaboradores/services/feriado_do_posto.py:175–185,272–285,325–354`
   - **Pré-requisitos de pouso.** O registro exige `sombra_c2b = 0`, e o processo exige suíte inteira verde na árvore da obra antes do opus final. Na cópia pude conferir os trechos do patch e rodar as verificações estáticas descritas acima. Hash Git, suíte inteira pós-delta, `sombra_c2b`, opus, pouso e efeito em produção ficam [nao verificado]. `app/docs/BACKLOG.md:1965–1966` · `CLAUDE.md:372–381`

## 21/11 17:56 · sessao 16374add9c62 · FERIADO-POR-POSTO · construtor · origem=Code

F1–F5 estão na cópia em três arquivos. Os módulos estáticos de F1, F2 e F5 passaram isoladamente. Em F3 e F4, as duas asserções do rótulo passaram em memória. O módulo Django parou na importação de `django.core`, então os dois testes completos seguem [nao verificado]. [manage.py:11](/home/ronald/wt-codex-feriado-por-posto-120132/app/manage.py:11) [nao verificado]

| Item | Mudança | Mesmo teste do log | [nao verificado]
|---|---|---|
| F1 | `previa_da_marca` retorna dentro do `except _DesfazAPrevia`, após a sentinela sair do `atomic`. [feriado_do_posto.py:398](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:398) | `test_sem_except_pass_novo` verde, **1/1** no módulo. O mutante que restaura `except: pass` falhou. [test_contract_no_except_pass.py:73](/home/ronald/wt-codex-feriado-por-posto-120132/app/ponto/tests/test_contract_no_except_pass.py:73) | [nao verificado]
| F2 | `aplicar_marca` nomeia `Posto` no `select_for_update`. [feriado_do_posto.py:307](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:307) | `test_zero_trava_fora_de_trava_na_ordem` verde, **22/22** no módulo. O mutante com `type(posto)` voltou a acusar `?type`. [test_e1_v7_catracas.py:387](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:387) | [nao verificado]
| F3 | A asserção de vínculo em P espera o rótulo atual e conserva `1/1`. [test_jv1_volta5_posto_do_dia_nos_vetos.py:94](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:94) | `test_RED_vinculo_em_P_cadastro_em_Maringa_entra_no_universo`. A asserção passou em memória e falhou com o literal antigo. Teste completo [nao verificado]. | [nao verificado]
| F4 | A asserção de cadastro em P espera o rótulo atual e conserva `0/0`. [test_jv1_volta5_posto_do_dia_nos_vetos.py:100](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:100) | `test_RED_cadastro_em_P_vinculo_em_Maringa_fica_fora`. A asserção passou em memória e falhou com o literal antigo. Teste completo [nao verificado]. | [nao verificado]
| F5 | Registrei `Command.handle` em `B2_menos_B1`, imediatamente antes de `LinhaTransporte.custo_mensal`, que permaneceu na lista. [test_medida_turno_do_par.py:389](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:389) | `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` verde, **35/35** no módulo. O mutante que omite só a entrada nova falhou na igualdade exata. [test_medida_turno_do_par.py:565](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:565) | [nao verificado]

### F2 — posição da trava

O modelo é `colaboradores.models.Posto`. Ele é travado primeiro em cada segmento de `aplicar_marca`, inclusive antes de chamar a retratação. A ordem de `PerguntaDisputa.trava_na_ordem` abrange **Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa**. A catraca cobra essas quatro classes e raízes ilegíveis. Nomear `Posto` preserva a trava que já existia. [feriado_do_posto.py:307](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:307) · [feriado_do_posto.py:325](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:325) · [chamados/models.py:2816](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/models.py:2816) · [test_e1_v7_catracas.py:71](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:71)

### F5 — fonte do dia e do par

A única entrada nova foi `colaboradores/management/commands/marcar_feriado_do_posto.py::Command.handle`, em **B2−B1**, o critério largo de dia. O detector marca `localdate(agora)`. O comando obtém `agora` de `timezone.now()` e passa o dia civil da execução à porta. O **dia do fato** vem de `dias_do_fato`, o **vínculo desse dia** de `vinculos_dos_dias`, e os **turnos da prévia** de `turnos_do_colab`. A prévia agrupa os turnos pelo `data_turno` já retornado e lê `entrada` e `saida` dos pares entregues. [core/medida_turno_do_par.py:220](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/medida_turno_do_par.py:220) · [marcar_feriado_do_posto.py:46](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/management/commands/marcar_feriado_do_posto.py:46) · [feriado_do_posto.py:248](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:248) · [feriado_do_posto.py:453](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:453)

### Escritores e leitores

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `Posto.pk`, `nome`, `municipio`, `opera_em_feriado` | `criar_posto` insere a linha e `definir_municipio_posto` grava a FK. `marcar_opera_em_feriado` grava a marca. O comando lê o posto pelo `pk` e imprime nome, município e marca anterior. `aplicar_marca` relê a marca sob trava para planejar e para interromper segmentos que tenham sido superados por outra marca. `eh_feriado_do_posto` usa o município ao consultar `feriados_de`. `colaboradores/services/estrutura.py:43`, `:90`, `:99` · `colaboradores/management/commands/marcar_feriado_do_posto.py:111`, `:134` · `colaboradores/services/feriado_do_posto.py:55`, `:309`, `:328`, `:346` |
| `EscalaColaborador.posto`, `trabalha_em_feriado`, `ChamadoColaborador.status_local` e dia do fato | A porta de vínculo grava o posto ao criar ou reaproveitar o vínculo e grava a chave de feriado em `marcar_trabalha_em_feriado`. `_chamados_do_posto` lê o posto do vínculo devolvido para o dia do fato e recebe os vivos de `qs_vivos`, que filtra `status_local`. A fixture grava `aberto` e `contexto_json.data_turno`. `dias_do_fato` lê `celula.data` ou, no degrau seguinte, `contexto_json.data_turno`. No ato de retratar, `reconciliar` delega a escrita do estado a `_gravar_estado`. `colaboradores/services/vinculo.py:274`, `:287`, `:898` · `colaboradores/services/feriado_do_posto.py:244`, `:283`, `:350` · `chamados/services/painel.py:155` · `chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:96` · `chamados/juizes.py:187` · `chamados/models.py:234`, `:667` |
| `plano` e totais da aplicação | `plano_da_marca` produz `candidatos`, `sem_dia`, `vivos`, `alvo`, `propagar`, `marca_antes` e `marca_muda`. `aplicar_marca` consome o plano e devolve `marca_muda`, `retratados`, `propagados` e `plano`. A prévia repassa esses valores, e o comando lê o plano e imprime os totais do `--apply`. `colaboradores/services/feriado_do_posto.py:277`, `:318`, `:325`, `:343`, `:354`, `:406` · `colaboradores/management/commands/marcar_feriado_do_posto.py:132`, `:152` |
| Foto da célula e `a`, `b`, `outras` | A porta da célula grava `colaborador_id`, `data`, `trabalha`, `escala_geradora` e `dna` na geração, `ata`, `veredito` e `veredito_em` na lavra, e `julgada_em` e `impressao` no carimbo. `_foto_das_celulas` lê a foto, `_classificar_previa` produz as três listas, e `_imprimir_previa` as lê. `ponto/portas/celula.py:293`, `:306`, `:1090`, `:1194` · `colaboradores/services/feriado_do_posto.py:403`, `:409`, `:431`, `:462` · `colaboradores/management/commands/marcar_feriado_do_posto.py:78` |
| Rótulo e `CLASSIFICAR` | O comando escreve o rótulo em `stdout`; `_dry` seleciona a linha, e as duas asserções passam a lê-la com o literal atual. O censo publica `a_classificar_pela_regra`; a lista manual `CLASSIFICAR` guarda a expectativa e o teste compara as duas por igualdade. `colaboradores/management/commands/marcar_feriado_do_posto.py:137` · `chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83`, `:94`, `:100` · `core/medida_turno_do_par.py:432` · `core/tests/test_medida_turno_do_par.py:389`, `:566` |

**LEI-AKITA 8.** O rótulo `candidatos a retratacao no feriado` descreve `len(plano['alvo'])`. O plano inclui chamados vivos no posto do dia do fato, alcançados pelo feriado, numa mudança para FECHA ou na retomada explícita. O total efetivamente retratado é contado depois de passar pelo gate de reconciliação. [feriado_do_posto.py:277](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:277) · [feriado_do_posto.py:331](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:331) · [marcar_feriado_do_posto.py:137](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/management/commands/marcar_feriado_do_posto.py:137)

**Vizinho do rótulo antigo.** O `git grep` na wt inteira encontrou somente `chamados/tests/e4b_c12_base.json:13` após a correção. Ali está o **emissor histórico** congelado para os testes c12. O verificador lê a fonte congelada, e o manifesto aponta nela o sítio S19 da retratação. O outro teste de saída já espera o rótulo atual. [e4b_c12_base.json:13](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_base.json:13) · [e4b_c12_verificar.py:18](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_verificar.py:18) · [e4b_c12_sitios.json:148](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_sitios.json:148) · [test_feriado_por_posto.py:636](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/tests/test_feriado_por_posto.py:636)

### Comandos

De `app/`, a função abaixo reproduz as provas em memória. Nas catracas estáticas, `unittest.TestCase` serve de base para executar os módulos originais. Em F3 e F4, a prova executa a linha de saída e a asserção original com as contagens do anexo. [test_contract_no_except_pass.py:2](/home/ronald/wt-codex-feriado-por-posto-120132/app/ponto/tests/test_contract_no_except_pass.py:2) · [test_e1_v7_catracas.py:51](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:51) · [test_medida_turno_do_par.py:95](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:95)

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
from importlib import import_module
from io import StringIO
from pathlib import Path
import sys
from textwrap import dedent
from types import ModuleType
import unittest
from unittest.mock import patch

case, mode, *targets = sys.argv[1:]
command = 'colaboradores/management/commands/marcar_feriado_do_posto.py'

if case in ('F3', 'F4'):
    line, count = {'F3': (94, 1), 'F4': (100, 0)}[case]
    rows, out = [None] * count, []
    reader = Path('chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py')
    expr = dedent(''.join(reader.read_text().splitlines(True)[line-1:line+1]))
    if mode == 'mutante':
        expr = expr.replace('candidatos a retratacao no feriado', 'em feriado que o alcanca')
    emit = dedent(''.join(Path(command).read_text().splitlines(True)[136:138]))
    exec(emit, dict(w=out.append, plano={'vivos': rows}, alvo=rows))
    check = unittest.TestCase()
    check._dry = lambda: out[0]
    exec(compile('\n' * (line - 1) + expr, str(reader), 'exec'), {'self': check})
    print(out[0])
else:
    django, test = ModuleType('django'), ModuleType('django.test')
    test.SimpleTestCase = unittest.TestCase
    sys.modules.update({'django': django, 'django.test': test})
    path = Path('colaboradores/services/feriado_do_posto.py').resolve()
    source = path.read_text().splitlines(True)
    if mode == 'mutante' and case == 'F1':
        source[405] = '        pass\n' + source[405][4:]
    if mode == 'mutante' and case == 'F2':
        source[306] = source[306].replace('Posto.objects', 'type(posto).objects')
    if mode == 'mutante' and case == 'F5':
        module = import_module('core.tests.test_medida_turno_do_par')
        module.CLASSIFICAR['B2_menos_B1'].remove(command + '::Command.handle')
    real_open = open

    def read(name, *args, **kwargs):
        if isinstance(name, (str, Path)) and Path(name).resolve() == path:
            return StringIO(''.join(source))
        return real_open(name, *args, **kwargs)

    with patch('builtins.open', read):
        unittest.main(module=None, argv=['unittest', *targets], warnings='ignore')
PY
}

prova F1 cura ponto.tests.test_contract_no_except_pass
prova F2 cura chamados.tests.test_e1_v7_catracas
prova F3 cura
prova F4 cura
prova F5 cura core.tests.test_medida_turno_do_par

prova F1 mutante ponto.tests.test_contract_no_except_pass.ContractNoExceptPassTest.test_sem_except_pass_novo
prova F2 mutante chamados.tests.test_e1_v7_catracas.CatracaTravaTest.test_zero_trava_fora_de_trava_na_ordem
prova F3 mutante
prova F4 mutante
prova F5 mutante core.tests.test_medida_turno_do_par.CatracaTurnoDoParTest.test_a_classificar_publicado_sem_entrar_como_criterio_amplo

git grep -ni -F -e 'em feriado que o alcanca' -e 'em feriado que o alcança' -- :/
```

Comando do módulo F3/F4, que parou na importação local. [manage.py:11](/home/ronald/wt-codex-feriado-por-posto-120132/app/manage.py:11)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_jv1_volta5_posto_do_dia_nos_vetos --settings=config.settings_test
```

## 21/11 17:49 · sessao e72315cf8067 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei P1, P3, P5 e P7 na cópia `f3e48a4458e343075cc3873183ddc8c9cf03c533`. Confira o diff abaixo.

- **P1.** A eleição entrega ao plantio todas as candidatas que leu, inclusive as ambíguas e as inelegíveis. A trava existente recebe seus PKs e vínculos. A releitura compara a eleição, o conjunto e os PKs efetivamente travados, com `AtoDoPlantioMudou` em caso de divergência. [materializacao.py:139](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/materializacao.py:139) · [materializacao.py:2123](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/materializacao.py:2123) · [models.py:2875](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:2875)
- **P3.** O passivo guarda a composta da principal ao montar o grupo e relê as compostas em lote sob a trava, antes do plantio. Alteração recusa o grupo no balde `escopo_do_plantio_mudou`. [passivo_validado.py:360](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:360) · [passivo_validado.py:383](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:383)
- **P5 e P7.** O teste concorrente confere a ordem das quatro tabelas e o `ASC` do SQL enviado, e usa a eleição confirmada no plantio. A barreira rompida chega ao teste como `AssertionError` com `BrokenBarrierError` como causa. Há selos separados para as duas cláusulas da guarda da principal e para o reset em exceção. [test_e4c_f5_plantio_concorrente.py:58](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:58) · [test_e4c_f5_plantio_concorrente.py:230](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:230) [nao verificado]

### REDs na base

| Caso | Asserção que cai por leitura da base |
|---|---|
| P1, ambiguidade | O `assertEqual` dos PKs enviados ao `FOR UPDATE` recebe só a principal. A eleição ambígua retorna `None`, e a base só acrescenta a irmã eleita. `test_e4c_f5_plantio_concorrente.py:202` · `materializacao.py:2128` na base |
| P1, saída de candidata | O `assertRaisesRegex` espera `AtoDoPlantioMudou`. Uma candidata sem resposta sai do conjunto e a irmã eleita continua igual, então a base aceita. `test_e4c_f5_plantio_concorrente.py:215` · `materializacao.py:2148` na base |
| P3, composta | O `assertEqual` espera `{'escopo_do_plantio_mudou': 1}` nos três subcasos, que alteram isoladamente resposta, validação e materialização. A base chama `_trabalhar_o_par` e o balde fica vazio com o dublê do teste. O cenário com devolução real também passa a exigir esse balde e nenhuma tentativa de carimbo. `test_e4c_f5_guarda_plantio_s144.py:114` · `test_e4c_f5_guarda_plantio_s144.py:166` · `test_e4c_f5_guarda_plantio_s144.py:200` | [nao verificado]
| P7, barreira rompida | O `assertRaisesRegex` espera a falha do worker. O `except RuntimeError` da base converte a exceção da barreira em retorno de recusa. `test_e4c_f5_plantio_concorrente.py:101` · `test_e4c_f5_plantio_concorrente.py:78` na base |

### Mutantes para a sessão

Cada `sed` abaixo é independente e usa as linhas do diff entregue. Aplique um por vez em uma cópia descartável. As falhas previstas são o regex de SQL para m7/m7b, as duas asserções de `AtoDoPlantioMudou` para m8 e o `assertIs` do contexto externo para m9. `test_e4c_f5_plantio_concorrente.py:116` · `test_e4c_f5_plantio_concorrente.py:236` · `test_e4c_f5_plantio_concorrente.py:249` · `test_e4c_f5_plantio_concorrente.py:261`

```bash
obra_wt=/caminho/da/worktree-descartavel
obra_mat="$obra_wt/app/chamados/services/materializacao.py"
obra_passivo="$obra_wt/app/chamados/services/passivo_validado.py"
obra_modelos="$obra_wt/app/chamados/models.py"
obra_teste="$obra_wt/app/chamados/tests/test_e4c_f5_plantio_concorrente.py"

# m7 Pergunta
sed -i "2903s/order_by('pk')/order_by('-pk')/" "$obra_modelos"

# m7b Colaborador, Disputa, Chamado, respectivamente
sed -i "2876s/order_by('pk')/order_by('-pk')/" "$obra_modelos"
sed -i "2885s/order_by('pk')/order_by('-pk')/" "$obra_modelos"
sed -i "2898s/order_by('pk')/order_by('-pk')/" "$obra_modelos"

# m8 remove cada cláusula isoladamente
sed -i '2152s/pk not in travadas or //' "$obra_mat"
sed -i '2152s/ or _dados_da_trava_do_plantio(p) != dados_p//' "$obra_mat"

# m9 restaura o contexto somente no caminho normal
sed -i '1839,1842c\    yield\n    _IRMAS_CONFIRMADAS_NO_PLANTIO.reset(tok)' "$obra_mat"

# P1 só registra a eleita / ignora mudança do conjunto / ignora candidata sem trava
sed -i '2126c\    candidatas = {irma.pk: _dados_da_trava_do_plantio(irma)} if irma is not None else {}' "$obra_mat"
sed -i '2157s/candidatas_atuais != candidatas or //' "$obra_mat"
sed -i '2157s/ or candidatas_atuais\.keys() - travadas//' "$obra_mat"

# P3 pula a reconfirmação composta
sed -i '390,393d' "$obra_passivo"

# P7 volta a capturar RuntimeError como recusa
sed -i '81,83c\                    except RuntimeError as exc:' "$obra_teste"
```

### Porta de cada teste [nao verificado]

`--dir` recebe a raiz da worktree, que contém `app/`. Para cada mutante, a sessão usa o mesmo seletor na cópia descartável. `bin/suite.sh:285` · `bin/suite.sh:324`

```bash
obra_wt=/home/ronald/wt-codex-e4c-f5-constroi-160129

bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_duas_materializacoes_inversas_travam_o_mesmo_par_em_ordem
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_MORDE_guarda_da_principal_sem_pk_travado
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_MORDE_guarda_da_principal_com_dados_mudados
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_MORDE_contexto_do_plantio_restaurado_em_excecao
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_trava_todas_as_candidatas_mesmo_sem_irma_eleita
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_conjunto_de_candidatas_muda_com_eleicao_igual
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_MORDE_guarda_recusa_candidata_sem_pk_travado
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_passivo_reconfirma_a_composta_da_principal
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_passivo_foto_velha_recusa_escopo_e_mesa_valida_mesmo_pk
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_passivo_b_persistido_recusa_par_e_mesa_valida_mesmo_pk
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_barreira_rompida_escapa_do_worker
```

`git diff --check` e o parse dos quatro arquivos passaram. A prova isolada da barreira e do `ContextVar`, com dublê de conexão, terminou com `Ran 2 tests`, `OK`, `rc=0`. Suite em PostgreSQL, REDs na base, mutantes e certificação pelo Code e pelo Opus final **[nao verificado]**. `test_e4c_f5_plantio_concorrente.py:98` · `test_e4c_f5_plantio_concorrente.py:253` [nao verificado]

**LEI-AKITA** · origem=`materializacao.py:2130` + `passivo_validado.py:383` · testemunha=`PerguntaDisputa.trava_na_ordem` (`models.py:2875`) + `DecisaoPergunta.composta` (`models.py:4382`) · RED=`test_e4c_f5_plantio_concorrente.py:189,204,98` + `test_e4c_f5_guarda_plantio_s144.py:166` · quem-mais-le=laço da disputa (`materializacao.py:2212`) + passivo (`passivo_validado.py:389`) · juízes novos=0 · padrao=Single Writer Principle com ordem canônica de travas.

## 21/11 17:41 · sessao 7598a4119613 · MEDIDA-FOLHA-[nome] · leitura · origem=Code

# MEDIDA-FOLHA-[nome]-2111-1740 · volta 1

Revisei a cópia no diretório `ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5`. A cópia veio sem `.git`, portanto a identidade do commit está [nao verificado]. Conferi a sintaxe da sonda e exercitei a captura do comando E6 com dados simulados. A execução com Django e os resultados de produção estão [nao verificado].

A unidade comum da folha é **colab-competência por empresa**. A janela começa no `dia_inicio_competencia` do mês anterior e termina na véspera desse dia no mês da competência. O juiz usa 21 como padrão e limita o corte a 2–28. A sonda usa o juiz para 10 e 11/2026. `app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/folha/export.py:47`

## (a) Termômetro R1

| Contrato | Leitura |
|---|---|
| **Fonte** | O ESTADO exibe o R1 declarado em `placar_estrutural.placar()`. A medição vem de `e6_oraculo.Command.handle`, que chama `InsumosDoDono.dono()` → `dono_da_divergencia()`. O comando lê `espelho_do_colab`, a função que `ponto:espelho` chama em `espelho_ponto` antes de renderizar `ponto/espelho.html`. `bin/gerar_estado.py:60`, `bin/gerar_estado.py:105`, `app/core/placar_estrutural.py:123`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:316`, `app/ponto/urls.py:10`, `app/ponto/views.py:161`, `app/ponto/views.py:195` |
| **Unidade** | Um dono por dia-colab divergente. A sonda conta dias e pessoas distintas por dono, a união de pessoas e quantas aparecem em exatamente dois ou três donos. A precedência efetiva é lacuna de cadastro por folga/vínculo, batida aberta com órfã real ou contagem ímpar, fase deslocada ou Cadastro × Realidade, depois o resíduo `ESTRUTURA`. `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:198`, `app/ponto/management/commands/e6_oraculo.py:378` |
| **Universo** | Empresas ativas e colaboradores com `FechamentoMensal` na empresa e competência, sem filtro de aptidão para o TXT. O espelho fornece `minutos_realizados` ou, na sua ausência, `pago_h` em minutos. `app/ponto/management/commands/e6_oraculo.py:263`, `app/ponto/management/commands/e6_oraculo.py:303`, `app/ponto/models.py:485` |
| **Exclusões** | Batidas retratadas ficam fora do insumo. O comando pula dia sem linha, isento ou com ausência, sem valor lavrado, zero dos dois lados ou diferença explicada pela pausa declarada. Diferença absoluta de até 10 min conta como concordância. O comando pula exceções do espelho e soma erros. A sonda devolve o primeiro erro pelo formato pedido para evitar publicar totais parciais. `app/ponto/turnos.py:2872`, `app/ponto/management/commands/e6_oraculo.py:149`, `app/ponto/management/commands/e6_oraculo.py:215`, `app/ponto/management/commands/e6_oraculo.py:315`, `app/ponto/management/commands/e6_oraculo.py:331` |

**R1 de 21/11 por empresa, união e sobreposição** [nao verificado]. O número existente no placar é a medição de **06/11 na sombra**. `app/core/placar_estrutural.py:130`

## (b) Retrato, prévia e ponte com o R1

| Contrato | Leitura |
|---|---|
| **Fonte da prévia** | O seletor em `folha/exportar.html` chama `folha:preview_exportacao`. A view chama `classificar_export`, conta `entra` e `fora` e agrupa os retidos por `motivo`. `app/templates/folha/exportar.html:26`, `app/folha/urls.py:19`, `app/folha/views.py:333`, `app/folha/views.py:350`, `app/folha/views.py:406` |
| **Fonte do Retrato** | `folha:retrato_folha` lê `RetratoFolha` por empresa e competência via `linhas_do_retrato`. A barra usa `quadros_barra_retrato` → `recortar`. O template inclui a barra e a lista. `app/folha/urls.py:24`, `app/folha/views_retrato.py:194`, `app/folha/views_retrato.py:324`, `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:840`, `app/templates/folha/retrato_folha.html:32`, `app/templates/folha/retrato_folha.html:53` |
| **Unidade e universo** | Uma pessoa por fechamento. A prévia consulta os fechamentos gravados da empresa e competência, de qualquer status. Essa base coincide com a do R1 para a mesma empresa ativa no mesmo estado do banco. O Retrato lê a lista guardada na lavratura. A ponte usa a interseção com o R1 e conta também as pessoas presentes em apenas um dos lados. `app/ponto/management/commands/e6_oraculo.py:303`, `app/folha/export.py:782`, `app/folha/retrato_folha.py:110`, `app/folha/retrato_folha.py:1212` |
| **Exclusões e baldes** | A prévia põe todos os `fora` em **Retidos**. O Retrato exibe a rescisão em **Módulo próprio**, separada de **Bloqueados**. `sem_codigo_dominio`, `ferias_com_batida` e `furo_espelho` são motivos atribuídos por precedência, depois da rescisão. A batida em férias usa agendamento na janela e batida apurável. Filtros da barra e o conjunto de empresas do seletor também podem mudar o total exibido. `app/folha/views.py:351`, `app/folha/retrato_folha.py:27`, `app/folha/retrato_folha.py:89`, `app/folha/export.py:464`, `app/folha/export.py:866`, `app/folha/export.py:902`, `app/folha/export.py:968`, `app/folha/views_retrato.py:216`, `app/colaboradores/queries.py:78` |

O **GET do Retrato com `empresa=` é não mensurável sem gravar por essa view**. O recorte chama `PreferenciaUI.set_pref`, que pode criar a preferência ou salvar seu novo valor. Os números são mensuráveis pelo leitor `linhas_do_retrato`, usado na sonda. A ponte cruza os baldes com os donos do **R1**, que classificam divergência de minutos. O campo `dono` do Retrato guarda o fio de resolução. `app/folha/views_retrato.py:155`, `app/colaboradores/models.py:212`, `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:1246`, `app/ponto/management/commands/e6_oraculo.py:378`

| Número citado | Dado recebido | Apuração nesta volta |
|---|---:|---|
| Espelho pendente, 10/2026 | 207 · `aval.txt:1` | Por empresa, motivo e dono [nao verificado] |
| Espelho pendente, 11/2026 | 298 · `aval.txt:1` | Por empresa, motivo e dono [nao verificado] |
| Entram, retidos, sem código, batida em férias | [nao verificado] | Contagens e ponte [nao verificado] |

## (c) Espelho pendente por motivo e porta

**Fonte e unidade.** A prévia exibe `furo_espelho`, cujo `motivos_espelho` sai de `motivos_retencao_celula`. A face `com_dias=True` devolve um motivo por dia retido. A sonda conta dias e colabs distintos por motivo entre os retidos por `furo_espelho` no universo do R1. Uma pessoa pode aparecer em vários motivos. O Retrato guarda o motivo geral, datas, tipo e ações. A quebra por motivo diário vem da leitura atual da autoridade. `app/folha/views.py:409`, `app/folha/export.py:968`, `app/folha/export.py:1176`, `app/folha/export.py:1259`, `app/folha/export.py:1301`, `app/folha/fora_da_folha.py:125`, `app/folha/retrato_folha.py:1237`

**Universo e exclusões.** A retenção por célula dispensa isentos e desligados, respeita admissão/demissão, ignora falta de célula em dia sem vínculo e só acusa ausência de célula ou de veredito nas condições do ramo de dia passado. Um veredito acusador segue seu próprio ramo, com as dispensas de protestos integralmente não cobraveis e de furo só de pausa. `app/folha/export.py:1205`, `app/folha/export.py:1238`, `app/folha/export.py:1264`, `app/folha/export.py:1293`

| Motivo devolvido | O que falta ou precisa ser decidido |
|---|---|
| `sem_celula` | Dia passado com vínculo sem célula, ou célula com previsão definida sem veredito. A tarefa marca falta de célula como `nao_sei`. O motivo tem **sem porta própria** no mapa. Uma oferta efetiva de Resolver dia depende do calendário. `app/folha/export.py:1264`, `app/folha/export.py:1295`, `app/chamados/services/tarefa_do_dia.py:236`, `app/chamados/services/tarefa_do_dia.py:697`, `app/folha/retrato_folha.py:1016` |
| `espelho_furo` | Furo integral ou parcial ainda acusador. O mapa inclui combinações em que falta cobrança, régua válida ou resolução. `app/escala/models.py:1611`, `app/folha/export.py:1293` |
| `espelho_discordante` | Batida órfã fora da tolerância, realizado inflado ou realizado zero com turno. `app/escala/models.py:1616` |
| `espelho_indefinida` | Dia previsto indefinido, fase por declarar ou ausência parcial sem período. `app/escala/models.py:1615` |
| `espelho_cobranca_indevida` | Cobrança em dia sem trabalho ou sem furo, ainda com veredito acusador. `app/escala/models.py:1613`, `app/escala/models.py:1767` |
| `espelho_cobrado` | O dia continua acusado depois de haver cobrança reconhecida no julgamento. `app/ponto/services/cartorio.py:1534`, `app/escala/models.py:1767` |
| `espelho_nunca_bateu` | Falta de adesão ao ponto no ramo de trabalho sem cobertura integral. `app/ponto/services/cartorio.py:1312`, `app/ponto/services/cartorio.py:1318` |
| `espelho_fato_em_ausencia` | Houve trabalho em dia coberto por ausência e o conflito segue acusado. `app/escala/models.py:1617`, `app/escala/models.py:1754`, `app/chamados/services/tarefa_do_dia.py:358` |

**Portas por ocorrência.** Para os motivos da tabela, a rota depende da ação publicada para o dia. O drawer chama `portas_da_linha` para traduzir a ação em link, com alternativas pelo fio e pelo tipo já gravados. A sonda cruza cada motivo diário com essa tradução e conta `sem_porta` e `sem_leitura` separadamente. `app/chamados/services/tarefa_do_dia.py:719`, `app/chamados/services/tarefa_do_dia.py:804`, `app/folha/fora_da_folha.py:278`, `app/folha/retrato_folha.py:1024`, `app/colaboradores/views.py:996`, `app/templates/folha/_retrato_o_que_fazer.html:42`, `app/templates/folha/_retrato_o_que_fazer.html:66`

| Ação ou situação publicada | Porta de tela para o admin | [nao verificado]
|---|---|
| Validar, decidir, abrir chamado ou cobrar pelo fio | `chamados:modal_fio` → `modal_fio_colaborador`. Cobrar deixa a resposta com o colaborador. **Abrir esse GET é não mensurável sem gravar** por essa view, que marca notas do DP como lidas. A sonda resolve apenas o nome da URL. `app/chamados/urls.py:39`, `app/chamados/views.py:869`, `app/chamados/views.py:877`, `app/chamados/services/nota.py:126`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1016` |
| Resolver dia quando oferecido | `colaboradores:calendario_ponto` → `calendario_ponto`. O formulário envia para `ponto:veredito_celula` → `veredito_celula_view`. A oferta exige dia passado, status elegível e último recurso. A tela exige permissão. `app/colaboradores/urls.py:24`, `app/colaboradores/views.py:1976`, `app/colaboradores/services/calendario.py:97`, `app/colaboradores/services/calendario.py:117`, `app/templates/colaboradores/partials/_calendario_grade.html:131`, `app/templates/ponto/partials/_veredito_dia.html:17`, `app/ponto/urls.py:58`, `app/ponto/views.py:3466` |
| Decidir ausência com registro identificado | `ponto:painel_ausencia` → `painel_ausencia`. O drawer recebe o ID da ausência e `somente_decisao=1`. `app/folha/retrato_folha.py:1050`, `app/ponto/urls.py:70`, `app/ponto/views.py:2070` |
| Tipo gravado `escala`, `folga` ou `sem_escala` | Link complementar `colaboradores:painel_vinculo` → `painel_vinculo`. `app/folha/retrato_folha.py:1003`, `app/colaboradores/urls.py:12`, `app/colaboradores/views.py:262` |
| Sem ação ou fio publicado | `nao_sei` e `ninguem` têm **sem porta** pelo fio. `nao_bate`, `falta`, `ferias` e `sem_causa` têm **sem porta** complementar pelo tipo. A rota `chamados:ir_para_ausencia` exige um chamado, e o mapa do tipo não oferece esse link. `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:996`, `app/chamados/urls.py:44`, `app/chamados/views.py:1490` |

**Quantos casos de cada motivo têm rota funcional, estão sem porta ou sem leitura em produção** [nao verificado].

## (d) Central

**Fonte, unidade e universo.** `chamados:painel_gestao` renderiza a Central. As duas pílulas contam **chamados**, uma vez por verbo, sobre os vivos do recorte. `particionar_verbos` usa créditos de perguntas. Suprime a contagem em Validar/Decidir para registros sem verbo e remapeia os silenciados para Arquivado ou Histórico. A competência da folha não entra no queryset dessas pílulas. `app/chamados/urls.py:22`, `app/chamados/views.py:721`, `app/chamados/views.py:767`, `app/chamados/services/painel.py:96`, `app/chamados/services/painel.py:135`, `app/chamados/services/painel.py:227`, `app/chamados/services/verbos.py:30`, `app/chamados/services/raiox.py:51`

| Contador | Valor do anexo | Sítio |
|---|---:|---|
| **Validar** | 707 · `aval.txt:1` | `verbos_contagens.validar.n`. Resposta creditada ou módulo de validação direta, conforme o juiz do verbo. `app/templates/chamados/painel_gestao.html:42`, `app/chamados/services/painel.py:230`, `app/chamados/catalogo/natureza.py:41` |
| **Decidir** | 705 · `aval.txt:1` | `verbos_contagens.decidir.n`. Chamado acionável sem pergunta viva ou de cadastro, depois das regras anteriores do verbo. `app/templates/chamados/painel_gestao.html:79`, `app/chamados/services/painel.py:230`, `app/chamados/catalogo/natureza.py:41` |
| **Pra decidir (validar + decidir)** | 1413 · `aval.txt:1` | `badge_chamados`, lido por `navbar_context` de `PlacarNavbar`. O cron grava a soma dos dois verbos do tenant sem filtros e o payload da mesma rodada. `app/templates/chamados/painel_gestao.html:191`, `app/core/context_processors.py:59`, `app/core/context_processors.py:102`, `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/core/management/commands/lavrar_badge_navbar.py:91` |

| Pílulas | Cabeçalho |
|---|---|
| `particionar_verbos(qs_vivos(_base=montar_qs_filtrado(urgencia, q, colab, empresa, praca)))['contagens']`. No refresh `chamados:placar_json`, sem filtros, pode ler `PlacarNavbar.objects.filter(pk=1).first().payload_placar`. O JavaScript atualiza as pílulas. `app/chamados/services/painel.py:198`, `app/chamados/services/painel.py:230`, `app/chamados/views.py:840`, `app/templates/chamados/painel_gestao.html:302` | `PlacarNavbar.objects.only('badge_chamados', 'payload_placar', 'lavrado_em').first()`. Sem lavra, `exige_o_admin(particionar_verbos(qs_vivos())['contagens'])`, sem filtros. Se essa partição levantar exceção, `qs_na_fila().count()` conta abertos/em análise não silenciados no futuro, incluindo possíveis casos de Cobrar. `app/core/context_processors.py:68`, `app/core/context_processors.py:87`, `app/core/context_processors.py:110`, `app/chamados/services/painel.py:188`, `app/chamados/catalogo/motor.py:29` |

**Contraexemplo de código.** Se o tenant tem um único chamado Validar, da empresa B, a Central filtrada pela empresa A mostra Validar 0 e Decidir 0, com cabeçalho 1. Mesmo sem filtro, um chamado que passou de Decidir para encerrado depois da lavra pode produzir a mesma diferença entre pílulas ao vivo e cabeçalho gravado. Para o mesmo payload gravado, vale a identidade `badge_chamados = Validar + Decidir`. A dissonância de apresentação é demonstrável. No anexo, `707 + 705 = 1412`, um abaixo de 1413. Filtros, carimbo e caminho de leitura que produziram essa diferença específica [nao verificado]. `app/chamados/services/painel.py:127`, `app/chamados/services/painel.py:155`, `app/chamados/services/verbos.py:22`, `app/core/context_processors.py:68`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `aval.txt:1`

## (e) Retrato 11 × prévia 11

| Contrato | Leitura |
|---|---|
| **Fonte e unidade** | Colab-competência por empresa. A comparação usa `linhas_do_retrato` sobre a foto existente e os itens atuais de `classificar_export`, a fonte da prévia. A terceira leitura chama `montar_linhas` em memória, a derivação usada pelo lavrador antes do `update_or_create`. `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:1174`, `app/folha/views.py:350`, `app/folha/management/commands/lavrar_retrato_folha.py:65`, `app/folha/management/commands/lavrar_retrato_folha.py:77` |
| **Universo e exclusões** | Competência 11 por empresa, com comparação de presença, `status` e `motivo`. A foto pode conter uma população anterior. A reconstrução recebe os mesmos itens do juiz da prévia, via `fora_da_folha` → `dias_que_seguram`. Casos sem foto aparecem no indicador `retrato_presente`. `app/folha/views_retrato.py:194`, `app/folha/retrato_folha.py:1212`, `app/folha/retrato_folha.py:1237`, `app/folha/fora_da_folha.py:146`, `app/folha/fora_da_folha.py:200` |

**Diferenças de critério verificadas no código**

- Para `status` e `motivo`, a derivação atual do Retrato copia as saídas do **mesmo** `classificar_export` que alimenta a prévia. A foto materializa essas saídas em uma rodada anterior. A sonda compara também a reconstrução em memória com a prévia na mesma transação, com um relógio comum, incluindo o `localdate()` interno do classificador. `app/folha/views.py:350`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:146`, `app/folha/retrato_folha.py:1222`, `app/folha/retrato_folha.py:1237`, `app/folha/export.py:1238`, `app/folha/management/commands/lavrar_retrato_folha.py:74`
- **Retidos** da prévia inclui a rescisão. **Bloqueados** do Retrato exclui a rescisão e a exibe em módulo próprio. A barra do Retrato também usa o recorte da tela. Para comparar elegibilidade, a sonda usa `fora_txt` dos dois leitores e também imprime `bloqueado` e `modulo_proprio` do Retrato. `app/folha/views.py:351`, `app/folha/retrato_folha.py:27`, `app/folha/retrato_folha.py:89`, `app/folha/views_retrato.py:225`
- Há uma rota distinta, `folha:previa_proposta`, cujo leitor `montar_previa` usa a classificação para montar propostas de marcos. Ela exclui isentos, dias a partir de hoje e dias fora de vínculo, e põe mês sem batida em seção própria. A prévia que contém a mensagem **Gerar TXT para conferir** é `folha:preview_exportacao`. `app/folha/urls.py:19`, `app/folha/urls.py:21`, `app/folha/views.py:386`, `app/folha/views.py:744`, `app/folha/previa.py:41`, `app/folha/previa.py:69`, `app/folha/previa.py:107`

O aval cita **302 × 303**. A atribuição dessa diferença a defasagem, recorte ou baldes está [nao verificado]. O código guarda uma única foto por empresa e competência. O indicador de frescor compara o carimbo dessa foto ao fechamento mais recente, sem reconstituir historicamente a classificação. `aval.txt:1`, `app/folha/models.py:356`, `app/folha/models.py:365`, `app/folha/retrato_folha.py:1413`

## (f) Porta do export às 07:04

**Unidade, universo e exclusões.** O ESTADO conta linhas de recusa por data local do log e porta `porta_export`, consultando `ponto.log` e rotados. O escritor emite a recusa da tentativa que concluiu divergente ou falhou. O anexo curto repete a mesma linha que está no contexto. Há **uma tentativa identificável** no material fornecido. A contagem do arquivo completo está [nao verificado]. `bin/recusa_estado.py:60`, `app/core/recusa_critica.py:95`, `app/core/recusa_critica.py:108`, `app/core/recusa_critica.py:133`, `app/folha/porta_export.py:1074`, `f_linhas_export.txt:1`, `f_contexto_0703_0705.txt:9`

```text
2026-11-21 07:04:59,754 WARNING ponto.recusa_critica RECUSA-ACAO-CRITICA porta=porta_export usuario=- colab=- dia=2026-11-21 objeto=emp3:2026-12:e13c73fcee9c44349e48621754bdc6d6 tipo=erro motivo=antes=em_curso depois=concluido origem=cron ator=-: os leitores discordam em 1 ponto(s)
```

- **Quem escreveu.** O cron chama `selo_leitores_no_mesmo_numero --rollout --origem cron` para a competência corrente de cada empresa em rollout. O comando chama `medir` → `concluir`, que grava o carimbo e o estado. `concluir` chama `_recusa`, e `recusa_critica.registrar` emite o `WARNING` pelo logger encaminhado a `ponto.log`. `app/config/crons.py:1809`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:43`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:218`, `app/folha/porta_export.py:1273`, `app/folha/porta_export.py:1074`, `app/core/recusa_critica.py:85`, `app/config/settings/saas.py:160`, `app/config/settings/saas.py:181`
- **Causa confirmada.** Para a empresa 3, competência **12/2026**, a medição terminou `concluido` com resultado `divergente`, `n=1`. A linha traz a soma do ponto divergente. Qual teste compôs esse `1` está [nao verificado].

[truncado em 20000 de 34488 caracteres]

## 21/11 17:40 · sessao 66840d695d92 · E4C-F5-CONSTROI · verificador · origem=Code

# BLOQUEIA o pouso [nao verificado]

1. **Erro de banco virou classe B comum.** A captura é nova, não isola a consulta numa transação e descarta a exceção. Nas rotas verificadas, a classificação ocorre antes da transação de escrita. O risco de continuar numa transação abortada existe se o serviço for chamado dentro de um `atomic()` externo. O teste injeta uma exceção em Python e não exercita esse efeito no Postgres. `app/chamados/services/validacao.py:1082–1096,1230–1237,746–760` · `app/chamados/tests/test_fila_validar_lote.py:110–126` · `CLAUDE.md:17–19`
2. **Faltam mordidas exigidas pelo aval.** Falta o teste do explicador contra texto residual, o controle de retratação que atravesse o novo retorno antecipado e o caso em que `SEM_RESPOSTA_VIGENTE` barra vínculo e destino encerrado. Os dois testes dos atalhos exercitam `VETO_TEMPORAL`. `app/chamados/tests/test_explicador_disputa.py:53–102,143–156` · `app/chamados/tests/test_marco_por_motivo.py:184–241` · `app/chamados/tests/test_juiz_le_retratacao.py:49–61` · `app/chamados/juizes.py:793–795`
3. **O PRONTO está aberto.** A sonda fornecida ficou abaixo do limite. Publicação no RELATO, registro dos dois SHAs, censo **em produção** anterior à cura, integração sobre a volta 3, medição ORM do vigia e suíte inteira verde no hash final ficam [nao verificado]. A ordem exigida para o pouso está em `CLAUDE.md:35–38,372–381` e no anexo `aval.txt:1`. [nao verificado]

## (a) Aval e rascunho

| Item | Conferência |
|---|---|
| Registrar `44f7c468aeb7` e `566c3b225f0c` | Os dois constam de `aval.txt:1`. Registro no RELATO e validação independente dos SHAs [nao verificado]. |
| Escopo nominal | O patch fornecido mostra as cinco funções de produção e o tipo de retorno nos cinco arquivos previstos. Os hashes de conteúdo dos 12 arquivos listados no patch coincidem com os prefixos `index` anexados. A cópia veio sem metadados Git, portanto a completude do delta entre `5cc451ac3` e `93ab82d08` fica [nao verificado]. O aval diz “8”, mas os sete itens de `rascunho.txt:25–31` nomeiam nove caminhos de teste. O patch mostra sete desses caminhos, a cópia contém testes de lote em `app/chamados/tests/test_fila_validar_lote.py:79–126` e falta o caso pedido em `app/chamados/tests/test_explicador_disputa.py:53–102,143–173`. |
| Sonda A→B antes do pouso | `7:2` → **1**, `7:3` → **0**, `7:4` → **0**, total **1**, abaixo de 50. O trecho fornecido mostra as contagens. A função executada, a cobertura da frota e a publicação no RELATO ficam [nao verificado]. `r_sonda.txt:3–7` · `aval.txt:1` | [nao verificado]
| Censo de horizonte antes do `ValueError` | Na cópia, `carga_da_vida` chama só com `carga=self`. O caminho de produção com **ambos** vem da tarefa, que copia `self.agora` para a carga e passa o mesmo valor ao C6. Os três outros chamadores diretos consultam a foto atual sem carga nem horizonte. Divergências em produção e a realização do censo **antes** da cura ficam [nao verificado]. `app/chamados/services/tarefa_do_dia.py:525,630–637` · `app/chamados/services/carga_pergunta_viva.py:88,119` · `app/chamados/juizes.py:793,925–932,1531–1537` · `app/chamados/services/validacao.py:1227` · `app/chamados/services/explicador.py:52` · `app/chamados/services/vigia_carimbo.py:111` |
| Contrato e ordem | O resultado é imutável, sem interface de par, e exige `ts` e `tipo` para `SLOT`. As seis saídas estão implementadas. Horizonte divergente falha antes da composta e do cache. O veto preserva o empate inclusivo. C21 lê o par projetado antes do cache, e `hora` passa uma foto ao derivador existente, com texto vazio se inelegível. O marco fixo continua disponível. `app/chamados/juizes.py:346–415,1427–1441` · `app/chamados/services/materializacao.py:691–710` |
| Materialização e cache | Veto e resposta desfeita em `data_hora` fecham os dois atalhos e conservam a retratação independente. Os demais retornos sem slot mantêm os controles de destino e lei. `SLOT` conserva as provas por vínculo e janela. A carga guarda o resultado ou a própria exceção, amplia os limites apenas para `SLOT` e sempre inclui a busca por origem. O degrau 6 conserva a captura. `app/chamados/juizes.py:793–840,855–858,1531–1538` · `app/chamados/services/carga_pergunta_viva.py:113–145,186–195` |
| Lote, explicador e vigia | O lote preserva as guardas e exige C21, `SLOT` e ausência de conflito. O `except DatabaseError` acrescenta a política de falha descrita em (c). O explicador traduz o motivo e preserva os caminhos de ação e dono. O vigia conserva universo, classes, tripla e soma. O `prefetch` agrupa decisões em blocos de 500, mas continua havendo `.exists()` de plantadas por pergunta no ramo com slot. `app/chamados/services/validacao.py:1204–1237` · `app/chamados/services/explicador.py:48–95,138–150` · `app/chamados/services/vigia_carimbo.py:94–134` |
| Volta 3, REDs e PRONTO | A cópia conserva as cinco expectativas do C8b nos dois caminhos e legaliza a fixture noturna. O caso C8b e o mutante do aval foram executados em memória. O recorte C6 de N=1 e N=4 e as catracas A1 e de respondedores passaram. A identidade da árvore com a entrega da volta 3, RED antes da cura, os demais mutantes isolados no hash final e suíte inteira ficam [nao verificado]. `app/chamados/tests/test_e3_c8b_sem_banco.py:282–299,369–433` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:188–193,251–278` · `app/chamados/tests/test_e3_catraca_respondedores.py:482–556` · `CLAUDE.md:372–381` |

## (b) Escritores, leitores e fonte única

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `slot.motivo`, `slot.ts`, `slot.tipo` | `_slot_esperado` produz. Cinco leitores diretos em produção: juiz de fato, carga, lote, explicador e vigia. Todos migraram do par para o resultado. `models.py` reexporta o símbolo, e `test_p71` chama sem consumir o retorno. `app/chamados/juizes.py:346–415,793–803` · `app/chamados/services/carga_pergunta_viva.py:119–128` · `app/chamados/services/validacao.py:1227–1236` · `app/chamados/services/explicador.py:50–62` · `app/chamados/services/vigia_carimbo.py:94–117` · `app/chamados/models.py:4580` · `app/chamados/tests/test_p71_marco_sem_hora.py:49` |
| `carga.agora`, `carga.slots[pk]` e testemunhas da carga | A tarefa escreve `agora`, a carga copia e escreve o mapa por PK. `_slot_esperado` lê o mapa e relança a mesma instância. O juiz de fato lê as testemunhas carregadas por `plantadas`, `batida_na_janela` e `retratada`. A tarefa e o C6 encaminham o horizonte. `app/chamados/services/tarefa_do_dia.py:520–539,630–637` · `app/chamados/services/carga_pergunta_viva.py:88–95,110–145,186–195` · `app/chamados/juizes.py:362–387,793–803,855–858,1531–1537` |
| Resposta, atos e carimbos | A porta do colaborador escreve `resposta_colab` e `respondida_em`. As portas automática e de reparo pela trilha também escrevem o par ou completam o instante. A reabertura registra o ato e descarimba pela porta do modelo. `_carimbar_validacao` escreve `validada_em` e seu ato. `PerguntaDisputa.carimbar` escreve o carimbo de materialização e a via, com preenchimento condicional dos instantes. `DecisaoPergunta.registrar` persiste os atos e `composta` fornece os instantes da sequência, o texto da coluna e os carimbos físicos. Leitores relevantes incluem C6, C21, lote, vigia, explicador e o materializador pela guarda de anuência. `app/chamados/services/disputa_emissao.py:1499–1514,1664–1684,1707–1737` · `app/chamados/services/validacao.py:161–175,1225` · `app/chamados/models.py:3259–3268,3324–3374,4348–4390,4428–4438` · `app/chamados/juizes.py:369–380,1427–1441,1462–1466,1589–1598` · `app/chamados/services/vigia_carimbo.py:97–107` · `app/chamados/services/explicador.py:98–123` · `app/chamados/services/materializacao.py:1332–1341` |
| Identidade, dia, destino e batidas | As portas de nascimento fornecem `tipo_resposta`, `motivo`, `data_evento`, `disputa` e `chamado_destino`. A costura grava o destino e a porta do fio grava o dia do fato. O slot os lê para a foto; o derivador lê também `disputa.contexto`, o marco do destino e a grade. O plantio escreve batida pela porta `registrar_batida`. O juiz e o vigia leem as vinculadas vivas, e o lote lê o universo de `batidas_apuraveis`. Para a prova histórica `retratar_pergunta_obsoleta`, localizei o leitor avulso e o da carga. Escritor dessa chave literal [nao verificado]. `app/chamados/services/disputa_emissao.py:448–461` · `app/chamados/services/fio.py:843–855,975–981` · `app/chamados/juizes.py:287–298,408–412,798–840` · `app/chamados/services/materializacao.py:556–592,615–645,691–710,1638–1651` · `app/ponto/registro_batida.py:118,170–180,287` · `app/ponto/turnos.py:2872–2897` · `app/chamados/services/carga_pergunta_viva.py:110–112` |
| Classe, detalhe e `porque` do lote | `delta_da_resposta` produz `resp`, `esperada`, `delta_min`, `ampm`, `sinal_min` e `tipo`. A tolerância vem da folha; a janela de conflito vem de `CONFLITO_LOTE_MIN`. `classe_do_lote` produz classe e detalhe. A fila carrega `detalhe`, mas executor e API consomem só a classe. A proposta e a prévia exibem detalhes da A e agregam as exclusões. O motivo novo existe no serviço e não chega à recusa exibida pelas portas verificadas. `app/chamados/juizes.py:1975–2007,2017–2023` · `app/chamados/services/validacao.py:1028–1042,1085–1087,1128,1147–1154,1204–1237,1279–1294` · `app/chamados/views_cobrar.py:289–300,345–370` · `app/api/views_mensageria.py:542–575` · `app/templates/chamados/partials/_lote_preview.html:24` |
| Saídas indiretas do juiz, do explicador e do vigia | O booleano de materialização alimenta C6, lavra, backfill, raio-X, emissão, explicador, vigia e ambos os caminhos do materializador. O explicador passa o texto ao item de `por_que_nao_fecha`. O vigia produz a tripla que o censo, o passivo e o vigia do fio leem. O passivo já exclui `slot_nao_derivavel` antes de usar `ts`. `app/chamados/juizes.py:1531–1538` · `app/chamados/veredito_lavrado.py:263` · `app/chamados/management/commands/backfill_veredito_lavrado.py:105` · `app/chamados/services/raiox.py:194` · `app/chamados/services/disputa_emissao.py:2052` · `app/chamados/services/materializacao.py:1971,2216` · `app/chamados/services/explicador.py:123,144–150,179` · `app/chamados/services/vigia_carimbo.py:107–134` · `app/chamados/services/passivo_validado.py:330–347` · `app/chamados/services/vigia_fio.py:415–418` |

**Regra antiga que sobrevive.** O parser de `data_hora` continua duplicado no materializador, que lê a coluna depois de passar pelo C21. A cópia já existia no lado removido do patch, e o aval proíbe tocar o materializador. Em `hora`, ambos delegam a `_ts_tipo_do_plantio`. As guardas antigas do lote ainda leem texto físico para escape, legibilidade e delta antes da guarda de vigência para A. A API ainda calcula os detalhes pela classificação anterior e substitui só a classe pela do lote. `app/chamados/juizes.py:389–412,1975–1991` · `app/chamados/services/materializacao.py:1395,1624–1639` · `app/chamados/services/validacao.py:1204–1229` · `app/api/views_mensageria.py:544–546,573–575` · `CLAUDE.md:20–22`

**Dois significados.** Nenhum campo persistido ganha uma segunda função no patch. `slots` conserva a união entre resultado e exceção. Há mistura de formatos em `detalhe['porque']`, com frases antigas e códigos novos, e `LOTE_B` passa a agrupar também indisponibilidade técnica. É essa última mistura que faz uma falha de banco parecer uma exclusão comum na porta. `app/chamados/services/carga_pergunta_viva.py:119–128` · `app/chamados/services/validacao.py:1085–1087,1204–1237`

## (c) `DatabaseError` e `atomic()`

Há três chamadores diretos de `classe_do_lote` na cópia:

- `validar_lote_classe_a`, acionado pela rota de lote, pela tela e pela confirmação da IA. A reclassificação precede `acao_validar_pergunta`, que chama o escritor. O `atomic()` e as travas do escritor vêm depois. `app/chamados/services/validacao.py:1074–1096,746–760` · `app/chamados/services/acoes_disputa.py:13–22` · `app/chamados/views_cobrar.py:234–263,373–385` · `app/core/views_ia.py:142–169,859–874`
- `fila_validar_classificada`, chamada pela proposta, contagem e prévia, em caminhos de leitura sem `atomic()` visível. `app/chamados/services/validacao.py:1272–1294` · `app/chamados/views_cobrar.py:277–300,345–370`
- `api_mensageria_validar`, GET que lê a fila e serializa a classe, sem `atomic()` visível. `app/api/views_mensageria.py:523–552`

**A hipótese de transação quebrada no fluxo normal verificado não procede.** O `atomic()` identificado protege a escrita posterior, e os dicionários PostgreSQL de produção e SaaS na cópia não configuram `ATOMIC_REQUESTS`. Configuração efetiva fora da cópia [nao verificado]. **Sob um `atomic()` externo**, se o Postgres abortar a transação durante o SELECT, o `except` não faz rollback nem delimita savepoint. O executor pode fazer `continue` e tentar outra consulta, ou devolver uma recusa genérica com a transação ainda abortada. `app/config/settings/prod.py:9–18` · `app/config/settings/saas.py:83–92` · `app/chamados/services/validacao.py:1082–1096,1230–1237`

**A captura é nova.** O lado removido do patch deixava a exceção subir. A regra de barrar a classe A quando não há confirmação de ausência de conflito é coerente com o aval. O fallback novo é converter falha de infraestrutura em classificação normal, com um motivo que os chamadores verificados descartam. Isso fere a disciplina de tratar na origem da LEI-AKITA 1. O teste só prova que um `DatabaseError` sintético vira B com algum motivo. Até trocar `CONSULTA_CONFLITO_INDISPONIVEL` por qualquer string não vazia deixa essa asserção satisfeita. `diff.patch`, hunk `app/chamados/services/validacao.py:1220` · `app/chamados/services/validacao.py:1085–1087,1230–1237` · `app/api/views_mensageria.py:546` · `app/chamados/tests/test_fila_validar_lote.py:110–126` · `CLAUDE.md:17–19`

## (d) Classes de reprovação

| Classe | Achado |
|---|---|
| Segundo juiz registrado | Zero novos no patch. O lote e o slot chamam o C21 registrado. São duas chamadas, com finalidades diferentes, ao mesmo predicado. `app/core/juizes.py:305` · `app/chamados/services/validacao.py:1225–1229` · `app/chamados/juizes.py:379–382` |
| Segundo escritor fora da porta | Nenhum novo escritor persistente nos hunks. A foto é local, o cache pertence à carga e a escrita do lote continua na porta. `app/chamados/juizes.py:408–415` · `app/chamados/services/carga_pergunta_viva.py:117–128` · `app/chamados/services/validacao.py:1094–1096` |
| Relógio fora da borda | Nenhuma leitura nova de relógio no slot. Permanece o `date.today()` preexistente da janela viva quando `hoje` não é fornecido. `app/chamados/juizes.py:360–415,625–634,828–835` |
| Derivação paralela | Sobrevive o parser de `data_hora` no materializador. A missão de `hora` segue delegada. `app/chamados/juizes.py:389–412` · `app/chamados/services/materializacao.py:1395,1627–1637` |
| Selo ou RED sem mordida | Falta a mordida do explicador e a dos dois atalhos sob `SEM_RESPOSTA_VIGENTE`. O teste de banco simulado deixa sobreviver qualquer motivo não vazio. O C8b morde, conforme (e). `app/chamados/tests/test_explicador_disputa.py:53–102` · `app/chamados/tests/test_marco_por_motivo.py:184–241` · `app/chamados/tests/test_fila_validar_lote.py:124–126` |
| Ordem de trava | O patch não altera a sequência de travas. A leitura de conflito continua anterior à seção que trava e escreve. Isolamento concorrente em produção [nao verificado]. `app/chamados/services/validacao.py:746–760,1085–1096,1230–1236` |
| Pouso antes do pré-requisito | Pouso efetivo [nao verificado]. Os pré-requisitos pendentes constam de (a). `aval.txt:1` · `CLAUDE.md:372–381` | [nao verificado]
| Campo com dois significados | Nenhum novo significado em coluna persistida. A mistura entre classe de negócio e indisponibilidade técnica está em `LOTE_B` e seu detalhe. `app/chamados/services/validacao.py:1128,1225–1237` |

## (e) Testes e mutantes [nao verificado]

| Teste | Mutante de produção e asserção | [nao verificado]
|---|---|
| `test_marco_por_motivo` | Marco errado, veto perdido ou empate vetado → valores de hora, tipo e motivo em `:79–109,126–146`. Reabrir o vínculo → `assertFalse` em `:200`; reabrir o destino → `assertFalse` em `:213`. Usar texto bruto desfeito → motivo e campos em `:227–241`. Antecipar o cache a ponto de relançar erro inelegível, perder o motivo armazenado ou trocar a instância → `:254–265`. Aceitar horizonte divergente ou consultar antes de rejeitá-lo → `:279–283`. Alvos em `app/chamados/juizes.py:362–415,793–840,1435–1436` e `app/chamados/services/carga_pergunta_viva.py:119–128`. Execução ORM desses mutantes [nao verificado]. |
| `test_e3_c8b_sem_banco` e `e3_c8b_apoio` | Confundir vínculo com prova no slot → falhou em `:396,401` com o mutante em `app/chamados/juizes.py:803`. Alterar o tipo armazenado → falhou em `:427` com o mutante em `app/chamados/services/carga_pergunta_viva.py:126`. A mesma asserção fixa D+1 e `S`; `:428–432` fixa veredito e leituras. O apoio carrega o contrato real por AST em `app/chamados/tests/e3_c8b_apoio.py:94–97,116–124`. |
| `test_e3_pos_suite_sem_banco` | Trocar o motivo do veto → falhou em `:115`. Excluir o empate → falhou em `:111`. O cache sintético e a igualdade de motivo e tipo cobrem `:117–120`. Alvos em `app/chamados/juizes.py:374–387,1435–1436`. |
| `test_hx_fallback_sem_lei` | Absolver com lei acusando, perder a absolvição quando a lei cala ou estender a janela ao dia morto → asserções em `:68–85`, sobre `app/chamados/juizes.py:822–840`. Remover o mock do slot fortalece o teste. Falta assertar que a fixture chegou como `PLANTIO_SEM_TS` antes dos controles. `app/chamados/tests/test_hx_fallback_sem_lei.py:40–45,61–85` |
| `test_o306_vigia_carimbo` e `test_s3f4_data_e_ancora_pela_autoridade` | Absolver a vinculada fora do slot ou contá-la na classe errada → `app/chamados/tests/test_o306_vigia_carimbo.py:111–121`; somas em `:149–161`. Divergir do tipo ou instante do plantio → igualdade em `app/chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:96–110`. Alvos em `app/chamados/juizes.py:408–415,798–804` e `app/chamados/services/vigia_carimbo.py:107–117`. Execução ORM [nao verificado]. |
| `test_fila_validar_lote` | Remover C21 com marco fixo → exige B e `SEM_RESPOSTA_VIGENTE` em `:88–99`. Admitir data impossível → exige B e `DATA_HORA_INVALIDA` em `:104–108`. Eliminar a consulta de conflito → `.exists()` uma vez em `:124`; aceitar batida próxima → B e `conflito` em `:70–77`. Alvos em `app/chamados/services/validacao.py:1225–1237`. A seleção de refs em `:128–133` usa uma discrepância de hora como exclusão; falta nela um caso novo sem slot. Execução ORM [nao verificado]. |
| `test_explicador_disputa` | **Falta** um caso que fique vermelho ao voltar a citar `p.resposta_colab` como vigente em `app/chamados/services/explicador.py:52–61`. O teste de resposta sem veredito já presente verifica C7 e fechamento. `app/chamados/tests/test_explicador_disputa.py:65–102,143–156` |

**C8b com `sem_veredito` fica VERMELHO.** Executei o caso original e ele passou. Zerei `celula.veredito` na testemunha em memória. Os dez subcasos falharam por zero chamadas ao juiz de fato, nas asserções de carga e avulso. Sem a decisão admin, o C6 retorna pelo degrau da resposta antes de chegar ao degrau 6. `app/chamados/tests/e3_c8b_apoio.py:190–205` · `app/chamados/tests/test_e3_c8b_sem_banco.py:369–401` · `app/chamados/juizes.py:1462–1478,1531–1534`

**Comparação com a base anexada.** O lado removido já barrava a vinculada no veto explícito e já deixava `data_hora` desfeita sem slot. Contra essa base, nesses casos escurece a asserção do **motivo**, além da cobertura do novo contrato. O destino encerrado tinha o atalho, e `hora` avulsa podia alimentar o plantio com texto cru. Nos hunks exibidos, não identifiquei remoção de asserção nem troca de expectativa fora do aval. A fixture noturna passa de par ilegal para par legal e conserva D+1, tipo e leituras. `diff.patch`, hunks `app/chamados/juizes.py:341,762` e `app/chamados/tests/test_e3_c8b_sem_banco.py:407,424` · `a

[truncado em 20000 de 21471 caracteres]

## 21/11 17:38 · sessao 14c8ec388ea2 · E4C-F5-CONSTROI · verificador · origem=Code

**CORRIGE.** A trava nova ordena por PK a principal e a irmã eleita nos dois laços. A validação pública ainda grava o par por um caminho que contorna essa trava, e a eleição ainda usa candidatas que ficam fora do conjunto travado. O teste concorrente prova o caso de duas chamadas ao helper, com a trava do colaborador serializando as duas. A ordem sob concorrência com a porta de batida continua sem prova. `materializacao.py:2128–2152,2202–2217` · `passivo_validado.py:379–388` · `validacao.py:743–760,786–803` · `test_e4c_f5_plantio_concorrente.py:52–113` [nao verificado]

### (a) Condições do aval

| Condição | Resultado |
|---|---|
| **(i) Ordem canônica, mesma transação, conjunto completo** | **Falta completar o conjunto.** A ordem existente é Colaborador → Disputa → Chamado → Pergunta, cada grupo por PK crescente. Os dois laços chamam a trava e plantam no mesmo `atomic`. A irmã única elegível pela composta entra mesmo com a projeção divergente. Porém, o helper lê todas as candidatas para decidir unicidade e só trava a irmã retornada. Se houver zero elegíveis ou ambiguidade, trava apenas a principal, salvo candidatas que também sejam principais do lote. As linhas de `DecisaoPergunta` são lidas por `prefetch` e ficam fora do `FOR UPDATE OF self`. `models.py:2875–2904` · `materializacao.py:148–154,2128–2152,2202–2217` · `passivo_validado.py:379–388` |
| **(ii) RED concorrente em ordem inversa, sem deadlock e sem dupla gravação** | **O cenário existe.** Dois workers começam por PKs opostos, sincronizam antes de adquirir o colaborador, exigem PIDs distintos e verificam o SQL do par, uma tentativa vencedora, duas batidas e uma trilha por batida. O worker descarta o mapa retornado pela trava e chama `_materializar_uma` diretamente. A passagem pelo fluxo completo, a execução do RED no commit indicado e a ausência de carimbo duplicado ficam **[nao verificado]**. `test_e4c_f5_plantio_concorrente.py:52–113` | [nao verificado]
| **(iii) Passivo pelo mesmo caminho e preservação do restante** | O passivo usa o mesmo helper e o mesmo contexto, e contabiliza a mudança detectada como recusa do grupo. Ainda usa `classe`, `ts`, dia, competência, `exportada` e a foto de reversão obtidos antes da trava. A releitura dentro da unidade só se aplica a motivos de intervalo e ocorre depois do agrupamento e da decisão de chamar a unidade. A sequência das quatro travas permanece no helper. `passivo_validado.py:333–393,443–466,537–555` · `materializacao.py:1919–1925` · `models.py:2875–2904` |

### (b) Leituras no ato e TOCTOU

| Sítio | Cobertura |
|---|---|
| `travar_o_ato_do_plantio` e `metade_irma_do_intervalo` | A eleição inicial ocorre antes da primeira trava. Depois, a principal é relida e a eleição é refeita. Mudança no escopo ou irmã eleita ausente dos PKs travados recusa. A segunda eleição ainda consulta candidatas e decisões fora do conjunto. O teste de ambiguidade demonstra esse limite: a terceira pergunta é lida depois da trava e só então recusada pelo mapa. `materializacao.py:139–154,2113–2152` · `test_e4c_f5_plantio_concorrente.py:156–168` | [nao verificado]
| `par_do_intervalo` pela guarda de espera e pelo plantio | O par aplica C21 à principal e às candidatas, confere a eleição, lê a validação composta da irmã e só então deriva e autoriza o par. No laço e no passivo, a trava cobre a principal e a irmã confirmada, com a ressalva sobre as demais candidatas. A guarda de espera chega pela chamada em `935`, e a tentativa de gravar a irmã chega pela chamada em `1773`. A chamada da tela ao mesmo derivador é leitura; a chamada pela guarda de escrita é parte do ato. `materializacao.py:193–245,784–796,876–901,931–950,1442–1447,1773–1785,1972` |
| `validar_pergunta` e `_validar_o_par` | A principal é carregada e julgada pela composta antes do `atomic`, assim como a implausibilidade e `_irma`. Depois da trava, `_irma` antiga ainda alimenta K6, e a principal antiga é carimbada sem repetir os gates. `_validar_o_par` elege uma irmã nova sob a trava do colaborador, mas o `FOR UPDATE` anterior recebeu os destinos/clones da irmã antiga e **nenhum PK de pergunta**. A irmã nova pode ser validada e plantada, e o caminho avulso também chama a unidade sem instalar o contexto. A varredura posterior ocorre depois dessas escritas. `validacao.py:693–744,746–760,786–806,648–659` · `models.py:2900–2903` |
| Anuência, retenção do destino/clones e fechamento | A anuência usa C21 composto na unidade. O fecho de cada destino/clone consulta outras perguntas creditadas ao chamado e lê suas compostas, sem incluir seus PKs no conjunto do plantio. A cauda consulta C6/C7 depois dos `atomic` de plantio. O ramo de ausência é outro ato: faz seleção inicial e releitura por candidata sob a trava do colaborador. `materializacao.py:1296–1319,1339–1347,2084–2105,2219–2220,1043–1075` · `juizes.py:2121–2126` · `models.py:2017–2035` |
| Espelho, escala/célula e competência | As consultas a batida existente, vizinhança e colisão, os insumos da escala/célula e K6/F6 também decidem a gravação. As quatro travas não incluem essas linhas. Para K6 há um escritor independente verificável: a exportação registra a entrega sem tomar a trava do colaborador. A releitura de K6 na validação pública reduz a janela; o laço e o passivo não repetem o K6 do dia do fato depois da escrita. `materializacao.py:109–117,216–244,799–858,1517–1536,1924–1928` · `juizes.py:238–250,2064–2084` · `validacao.py:794–797` · `folha/models.py:174–188` |

**Escopo da validação pública.** A leitura pura para exibir o estado cabe em fila. A leitura usada para escolher as travas e depois efetivamente validar e plantar pertence à condição 1.2, pela redação do aval sobre tudo o que o ato lê para decidir. A pré-checagem pode ficar onde está se a decisão e o conjunto forem confirmados sob a trava antes da escrita. `aval.txt:1` · `validacao.py:743–760,786–803` · `materializacao.py:904–907,931–950`

### (c) `ContextVar`

O único `set` é `usar_irmas_confirmadas_no_plantio`, chamado pelo laço da disputa e pelo passivo. `par_do_intervalo` lê a eleição por PK, e `_materializar_uma` usa a presença do mapa para ativar a releitura da principal. O `finally` restaura o token anterior inclusive em exceção. Nas chamadas síncronas examinadas, a variável é local ao contexto e o mapa fica restrito ao bloco. O helper, por si, aceita instalação fora de `atomic`. `materializacao.py:202–204,1829–1839,1919–1922,2216–2217` · `passivo_validado.py:385–388`

Há dois usos do estado, **eleição confirmada** e **sinal para reler a principal**. O `.get(p.pk)` também dá o mesmo resultado para chave ausente e irmã confirmada como `None`. Isso rejeita uma irmã concreta nos dois casos, mas o mapa sozinho não prova que o PK atual pertence ao ato. Nenhuma coluna persistida foi criada para esse estado. `materializacao.py:199–204,1829–1839,1919–1922,2144–2152`

### (d) Escritores, leitores e regra antiga

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| PK, disputa, colaborador da disputa, motivo, tipo e dia | A emissão cria a disputa com colaborador e pai e abre a pergunta com motivo, tipo e dia. `_nascer` insere a pergunta e o banco atribui o PK. A porta de re-chaveamento muda o dia. A eleição e a foto de trava leem esses valores, e o gate de data e o derivador voltam a usá-los. `disputa_emissao.py:601–606,837–860` · `models.py:3224–3234,3766–3776` · `fio.py:1032–1096` · `materializacao.py:142–154,168–175,631–641,2113–2117` |
| Pai, destino, clones e contexto | A emissão atribui pai, contexto e clones. Nascimento/reuso e `reapontar_resolvedora` atribuem destino. O mapa de trava lê os três vínculos como conjunto de PKs. Derivação, guarda de data, conciliação e trilha voltam a lê-los nos seus papéis próprios. O `disputa` passado pelo chamador pode ter sido carregado antes da trava; reler `p` não atualiza esse argumento independente. `disputa_emissao.py:576–606,848–859` · `models.py:3195–3232` · `fio.py:74–88,975–981` · `materializacao.py:631–639,1311–1319,1597–1608,1919–1925,2216–2217` |
| `respondida_em`, `resposta_colab`, `validada_em/por` e sequência | Resposta humana, automática e pela trilha escrevem a projeção e registram os atos. Validação assina; devolução e reabertura limpam; `carimbar` também pode preencher colunas vazias. `DecisaoPergunta.registrar` escreve a sequência e invalida o cache da instância recebida. A composta entrega instante e validação do ato, texto da projeção e carimbo/via da coluna. C21, eleição, validação da irmã, anuência e retenção consomem essa leitura. `disputa_emissao.py:1499–1537,1630–1684,1707–1737` · `validacao.py:161–175,269–296,656–657` · `models.py:3363–3374,4382–4390,4428–4453,2017–2027` · `materializacao.py:148–154,195–208,1339–1343` |
| `materializada_em/por`, `via_resolucao`, `pendencia_admin`, `recusa_motivo` e conferência de data | O modelo grava/desfaz carimbo e via. O ramo hora grava pendência com motivo, `_carimbar_recusa` grava motivo e anexa nota, e `conferir_data_do_fato` grava a conferência. Eleição, par, filtros do laço, passivo, testemunha e reversão os leem. `models.py:3324–3328,3394–3405,3416–3455` · `disputa_emissao.py:1682–1684` · `materializacao.py:149–153,205–206,886–900,1401–1407,1724–1748,1944–1946,2191–2215` · `validacao.py:1008–1016` · `passivo_validado.py:65–86,541–557` |
| Batidas, trilha e retorno do lote | `registrar_batida` cria ou reutiliza. `anotar` altera `pergunta_origem` e também é usado pelo flip e pela retratação. A unidade escreve a trilha de materialização; juízes, vigia e testes leem o espelho e o vínculo. O laço agrega `erros`/`materializadas` para a validação pública. O passivo escreve o balde `escopo_do_plantio_mudou`, e o comando imprime os baldes no placar. `registro_batida.py:118–181,255–301` · `flip_batida.py:25–33` · `materializacao.py:1583–1612,2204–2228` · `vigia_carimbo.py:97–116` · `validacao.py:803–805` · `passivo_validado.py:389–393` · `materializar_passivo_validado.py:241–249` | [nao verificado]

**Leitores antigos encontrados.** A fila inicial da disputa ainda usa as faces SQL do carimbo físico e `validada_em` físico. A releitura composta posterior não recupera uma principal excluída pela seleção inicial. O caso de carimbo de resposta apagado que passa como **irmã** pode ficar invisível como **principal**. O passivo também seleciona por `validada_em` físico. `_situacao_da_irma` já usa C21 composto, mas escolhe a primeira complementar por PK para diagnóstico, enquanto o plantio recusa ambiguidade. `materializacao.py:148–154,291–303,2186–2215` · `juizes.py:1573–1588` · `vigia_carimbo.py:94–110` · `test_bug_par_projetado.py:1384–1423`

### (e) Classes de reprovação

- **Juiz duplicado, derivação paralela e escritor fora da porta.** A eleição chama o C21 registrado sobre a composta. O par continua usando o derivador de hora existente, e as escritas continuam pelas portas de batida, validação e carimbo. O defeito é a segunda entrada para o mesmo escritor sem a mesma trava. `core/juizes.py:305` · `materializacao.py:145–154,223–231,1528–1536,1785,2039–2040` · `validacao.py:648–659,786–792`
- **Ordem com a batida.** Há um caminho inverso no código: `flip_automatico --apply` abre um `atomic` e faz `UPDATE` de Batida; o `post_save` pode entrar no reconciliador e pedir a trava do Colaborador. O plantio mantém essa trava e pode tentar `UPDATE` da mesma batida reutilizada para vincular `pergunta_origem`. Se o flip já tiver a batida e seu reconciliador precisar do colaborador, há espera circular possível. A materialização dupla do teste serializa no colaborador e não exercita esse cruzamento. Ocorrência em execução **[nao verificado]**. `flip_automatico.py:62–79` · `flip_batida.py:25–33` · `registro_batida.py:264–274` · `ponto/signals.py:206–215` · `reconciliador.py:635–646,1486–1487` · `materializacao.py:1413–1415,1583–1587,2139–2140` · `test_e4c_f5_plantio_concorrente.py:62–82`
- **Relógio e significado de campo.** A eleição e a composta não introduzem relógio. A aprovação e o carimbo ainda leem instantes em pontos distintos da escrita. O `hoje` do passivo vem da borda do comando. O ponto de dupla função introduzido é o mapa do contexto, descrito em (c). `materializacao.py:148–154,1535,1829–1839,1919–1922,2006` · `models.py:4357–4359` · `materializar_passivo_validado.py:85–89,103–114`
- **Pré-requisito de pouso.** O enunciado informa `OK, Ran 8`. A linha terminal e o RC desse GREEN, a suite inteira, os resultados dos REDs nos commits indicados e o hash/base Git estão **[nao verificado]**. A cópia é compatível com o formato de arquivo exportado descrito na pista, e a checagem Git local retornou ausência de repositório. `CLAUDE.md:119–123` · `app/docs/PISTA.md:72–86,102–117`

### (f) Mutantes

- **m1, m2 e m4** alteram produção. O anexo registra falha para a troca pela coluna crua, a omissão da irmã dos PKs e a remoção da guarda de mudança da irmã. m4 elimina a comparação e a checagem de pertencimento juntas; não demonstra a mordida independente de cada cláusula. `mutantes_def.txt:8–11` · `mutantes.txt:1–4` · `materializacao.py:153,2138,2149`
- **m3** altera só o SQL observado pela asserção; o banco recebe o SQL original. Prova a sensibilidade do regex ao texto `DESC`. Uma mutação real de `order_by('pk')` para `order_by('-pk')` **na query de Pergunta** deveria cair nesse mesmo regex, por leitura estática. Um mutante de produção sobrevivente nessa linha está **[nao verificado]**. Nas outras três tabelas, a ordem por PK pode ser retirada sem que a asserção de ordenação a examine. `mutantes_def.txt:10` · `test_e4c_f5_plantio_concorrente.py:62–70,92–98` · `models.py:2875–2903`
- **Faltam provas dirigidas** para o mutante de produção da ordem, para a guarda da principal, para o reset em exceção e para o caminho real do passivo e da validação pública. O teste concorrente principal ignora o valor retornado pela trava; os dois últimos testes exercitam o contexto diretamente. m5/m6 foram definidos, com resultado **[nao verificado]**. `mutantes_def.txt:8–13` · `materializacao.py:1833–1839,2144–2151` · `test_e4c_f5_plantio_concorrente.py:75–82,156–185` · `passivo_validado.py:385–388` · `validacao.py:786–794` [nao verificado]

### (g) Flakiness

A sincronização usa `Barrier` e `Event`, sem `sleep` e sem depender de qual worker vence. Cada worker fecha sua conexão em `finally`, e o executor espera os workers antes do teardown. Há risco de falso vermelho sob carga pelos limites de 4 s de lock, 10 s de sincronização e 25 s por `Future`. O `Future` estourar não cancela o worker, e a conexão principal do teste de reabertura não aplica os limites SQL do worker. O `except RuntimeError` também pode mascarar uma barreira rompida como recusa da aplicação. `test_e4c_f5_plantio_concorrente.py:35–41,52–104,115–144` [nao verificado]

O relógio da fixture é congelado e os instantes de resposta e reabertura são explícitos. A fixture cria seus próprios registros e o runner aponta para um banco de teste configurado. Interferência com outra execução que use o mesmo banco e estabilidade da classe na suite inteira ficam **[nao verificado]**. `test_e4c_f5_plantio_concorrente.py:18–33,133–142,170–176` · `test_bug_par_projetado.py:44–87` · `config/settings/ci.py:9–17` · `bin/suite.sh:129,324`

### Correções para o pouso [nao verificado]

1. **Unificar a trava do caminho que realmente grava.** Fazer a validação pública confirmar principal, irmã e vínculos sob a mesma trava antes de carimbar; cobrir as candidatas que decidem unicidade e explicitar a proteção das demais leituras decisórias. `validacao.py:693–760,786–803` · `materializacao.py:148–154,2113–2152`
2. **Fechar a janela das fotos antigas.** Revalidar sob a trava os dados do passivo que ainda decidem o trabalho e tratar a seleção inicial da principal pela coluna física, preservando a recusa com dono para instante sem texto. `passivo_validado.py:333–387,537–555` · `materializacao.py:2186–2217`
3. **Provar a ordem com a batida e a mordida do RED.** Cobrir o caminho real de plantio contra o flip em transação, executar o mutante de produção da ordem e comprovar os resultados pendentes com veredito terminal, `Ran` e RC. `flip_automatico.py:62–79` · `materializacao.py:1583–1587` · `test_e4c_f5_plantio_concorrente.py:52–113` · `mutantes_def.txt:10–13` · `CLAUDE.md:119–123`

## 21/11 17:30 · sessao 163ca4271953 · E4C-F5-CONSTROI · sonda · origem=Code

| Empresa | Contagem | Fonte |
|---|---:|---|
| `7:2` | 1 | `sonda/anexos:4` |
| `7:3` | 0 | `sonda/anexos:5` |
| `7:4` | 0 | `sonda/anexos:6` |

`SOMA	1	PAREI_SE_MAIOR_QUE	50` (`sonda/anexos:7`)

Erro ou traceback ausentes no trecho. Há três linhas de contagem. (`sonda/anexos:1–7`)

A soma de 1 não passa de 50. (`sonda/anexos:7`)

## 21/11 17:25 · sessao a4e8a30e5634 · E4C-F5-CONSTROI · construtor · origem=Code

**Cura aplicada na cópia E4C-F5-CONSTROI**, sobre `5cc451ac397216a84a70aaebafb326edc2cfbf37`. Aval `44f7c468aeb7` e rascunho `566c3b225f0c`, conforme `aval.txt:1`. Conferência independente dos hashes dos anexos [nao verificado].

Os dez casos do RED mantêm a mesma AST da base. Nas checagens locais, passaram os 28 testes do C8b, os 28 das duas catracas e o controle avulso de veto e empate. O mutante do C8b saiu com `rc 1`, com dez falhas de asserção por zero chamadas ao juiz de fato. O teste vizinho de hash legado teve dois erros de importação de `django.utils`, reproduzidos com a fonte da base. A rodada ORM, a suíte inteira e a certificação pelo Code e pelo opus final [nao verificado]. `red_tail.txt:1–31` · `chamados/tests/test_e3_c8b_sem_banco.py:369–433` · `chamados/tests/test_e3_pos_suite_sem_banco.py:103–120,163–181` · `chamados/tests/test_e4c_f5_selo_chamada.py:188–193,251–278` · `chamados/tests/test_e3_catraca_respondedores.py:544–556`

**Pouso pendente da sonda.** Contagem A → B por empresa e publicação no RELATO [nao verificado]. Se a soma passar de 50, o aval manda parar com a tabela antes do pouso. `aval.txt:1` [nao verificado]

### Diff por arquivo

| Arquivo e linhas | Alteração |
|---|---|
| [chamados/juizes.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/juizes.py:346), 346–415 e 793–858 | Resultado imutável com `motivo`, `ts` e `tipo`. A incompatibilidade de horizonte é recusada antes da composta e do cache. O veto precede o C21 e o cache. `hora` entrega ao plantio uma foto com o texto elegível ou vazio. O juiz de fato usa o motivo para bloquear os dois atalhos e conserva a retratação com carimbo e trilha. |
| [chamados/services/carga_pergunta_viva.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/services/carga_pergunta_viva.py:113), 113–145 | Guarda por PK o resultado completo ou a própria exceção. Só `SLOT` entra no cálculo dos limites. A busca por origem continua cobrindo as perguntas do lote. |
| [chamados/services/validacao.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/services/validacao.py:1196), 1196–1237 | Depois das guardas anteriores, A exige C21, `SLOT` e consulta de conflito concluída sem batida próxima. Usei `CONSULTA_CONFLITO_INDISPONIVEL` para `DatabaseError` e `BATIDA_PROXIMA` para o conflito confirmado, conservando `conflito=True`. |
| [chamados/services/explicador.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/services/explicador.py:48), 48–62 e 138–150 | Traduz o motivo do slot em texto para o admin. O ramo sem slot deixa de exibir a resposta física. Ações e donos continuam definidos em `explicar_pergunta`. |
| [chamados/services/vigia_carimbo.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/services/vigia_carimbo.py:94), 94–133 | Alimenta as decisões por bloco de 500 e consome o motivo na foto atual. A tripla conserva `ts` para os dois resultados sem registro que têm slot. O censo usa as mesmas classes e somas. |

Nos testes existentes, cada ajuste corresponde a um consumidor do contrato anterior. `git diff --relative --unified=0` mostra os hunks da cópia. [nao verificado]

| Arquivo e linhas | Por que o teste precisava do ajuste | [nao verificado]
|---|---|
| `chamados/tests/test_marco_por_motivo.py:79–100` | Desempacotamento, igualdade do cache e comparações com `(None, None)` passam a ler o resultado. As testemunhas de 13h, cache e veto nos dois caminhos permanecem. |
| `chamados/tests/test_marco_por_motivo.py:108–109,126–127,131–142` | Empate, ato desfeito com marco fixo, saída de intervalo e formato legado passam a conferir os campos. Os horários e tipos exigidos foram preservados. |
| `chamados/tests/e3_c8b_apoio.py:6–16,94–97` | O harness já carrega funções reais por AST. Agora carrega também o tipo real que essas funções instanciam. |
| `chamados/tests/test_e3_c8b_sem_banco.py:410–411,427` | O noturno conserva o instante e a decisão admin da fixture para ter resposta legal e chegar ao juiz. O cache exige `SLOT`, D+1 e `S`. As asserções de leituras continuam em `:428–433`. |
| `chamados/tests/test_e3_pos_suite_sem_banco.py:114–120` | A entrada sintética do cache usa o tipo real. As duas consultas anteriores ao ato exigem `VETO_TEMPORAL`, e o empate exige `SLOT` com tipo `E`. |
| `chamados/tests/test_hx_fallback_sem_lei.py:41–45,61–85` | A fixture de `hora` sem texto já produz `PLANTIO_SEM_TS` pelo caminho real. Retirei o stub de par. Os três controles de destino, lei e janela continuam. |
| `chamados/tests/test_o306_vigia_carimbo.py:112–121` | O controle lê `.motivo` e `.ts` no lugar de desempacotar o par. A expectativa de batida fora do slot permanece. |
| `chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:104–110` | A igualdade com o plantio compara `ts` e `tipo`, com `SLOT` explícito. |

Os casos de `test_fila_validar_lote.py:79–126` já exigiam o contrato novo. Os controles de explicador existentes usam respostas válidas e mantêm as mesmas expectativas. A checagem local de texto residual e retratação usou os corpos reais no harness, em memória. `chamados/tests/test_explicador_disputa.py:53–102` · `chamados/tests/e3_c8b_apoio.py:94–124` · `chamados/services/explicador.py:48–62` · `chamados/juizes.py:287–298,793–795`

### REDs e comandos para o Code

| Caso | Sítio que satisfaz a asserção |
|---|---|
| a1 | O veto do ato posterior protege o juiz contra o vínculo das 08h. No empate, a batida ainda precisa comprovar o slot das 13h. `chamados/juizes.py:371–375,793–804` · `chamados/tests/test_marco_por_motivo.py:184–203` |
| a2 | A saída sob veto ocorre antes da leitura do destino e admite a testemunha independente de retratação. `chamados/juizes.py:793–795,822–840` · `chamados/tests/test_marco_por_motivo.py:205–217` |
| b1 | O C21 sobre o par projetado retorna `SEM_RESPOSTA_VIGENTE` antes do parser e do cache, com horizonte e na leitura avulsa. `chamados/juizes.py:368–393` · `chamados/tests/test_marco_por_motivo.py:219–228` |
| b2 | O texto desfeito chega vazio ao plantio. A autoridade existente ainda pode obter a hora do marco fixo. `chamados/juizes.py:379–415` · `chamados/services/materializacao.py:691–710` · `chamados/tests/test_marco_por_motivo.py:230–241` |
| c | A carga retém motivo e identidade da exceção. A guarda de `data_hora` ocorre antes do cache. O ramo que alcança o erro relança aquela instância. `chamados/services/carga_pergunta_viva.py:117–128` · `chamados/juizes.py:379–387,1531–1538` · `chamados/tests/test_marco_por_motivo.py:243–265` |
| d | A comparação de horizontes precede composta e cache. `chamados/juizes.py:360–373` · `chamados/tests/test_marco_por_motivo.py:267–283` |
| e1 | C21 tira de A a resposta desfeita mesmo quando o marco fixo pode fornecer slot. `chamados/services/validacao.py:1225–1229` · `chamados/tests/test_fila_validar_lote.py:79–99` |
| e2 | O parser devolve `DATA_HORA_INVALIDA`, que o lote carrega para B. `chamados/juizes.py:389–402` · `chamados/services/validacao.py:1227–1229` · `chamados/tests/test_fila_validar_lote.py:101–108` |
| e3 | A falha em `.exists()` leva a B com motivo. A consulta bem-sucedida sem conflito permite A. `chamados/services/validacao.py:1230–1237` · `chamados/tests/test_fila_validar_lote.py:110–126` |
| f | O par legal e o veredito admin levam as cinco testemunhas ao juiz real, nos dois caminhos. O RED exige a chamada antes de conferir o resultado. `chamados/tests/e3_c8b_apoio.py:189–208` · `chamados/tests/test_e3_c8b_sem_banco.py:369–402` |

Comandos a partir de `app/`, pela configuração do Code. O selo C6 conta `ausencias`, `trilha` e `batidas` para N=1 e N=4. No ORM, `decisoes` é pré-carregado na tarefa. A medição ORM do vigia está no comando `medir-vigia` abaixo [nao verificado]. `config/settings/ci.py:6–18,38–41` · `chamados/tests/test_e3_c8b_sem_banco.py:282–299` · `chamados/services/tarefa_do_dia.py:532–545` · `chamados/services/vigia_carimbo.py:103–106`

```bash
t() { PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci "$@"; }

t chamados.tests.test_marco_por_motivo chamados.tests.test_fila_validar_lote \
  chamados.tests.test_e3_c8b_sem_banco \
  -k test_RED_ -k test_testemunhas_de_materializacao_com_e_sem_carga

t chamados.tests.test_marco_por_motivo chamados.tests.test_e3_c8b_sem_banco \
  chamados.tests.test_e3_pos_suite_sem_banco chamados.tests.test_hx_fallback_sem_lei \
  chamados.tests.test_o306_vigia_carimbo chamados.tests.test_s3f4_data_e_ancora_pela_autoridade \
  chamados.tests.test_fila_validar_lote chamados.tests.test_explicador_disputa -k test_

t chamados.tests.test_juiz_le_retratacao -k retratada
t chamados -k chamados.tests

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_e4c_f5_selo_chamada chamados.tests.test_e3_catraca_respondedores
```

### Mutantes isolados

A mudança **(3)** zera só o veredito admin em memória. O `rc 1` local veio de `assert_called_once_with` nos dois caminhos, com zero chamadas em cada uma das cinco testemunhas. `chamados/tests/e3_c8b_apoio.py:189–204` · `chamados/tests/test_e3_c8b_sem_banco.py:377–402`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import unittest
from unittest.mock import patch
from chamados.tests import e3_c8b_apoio

original = e3_c8b_apoio.testemunha

def sem_veredito(*args, **kwargs):
    caso = original(*args, **kwargs)
    for pergunta in caso[2]:
        pergunta.celula.veredito = None
    return caso

nome = (
    'chamados.tests.test_e3_c8b_sem_banco.AlimentacaoC6.'
    'test_testemunhas_de_materializacao_com_e_sem_carga'
)
with patch.object(e3_c8b_apoio, 'testemunha', sem_veredito):
    resultado = unittest.TextTestRunner().run(unittest.defaultTestLoader.loadTestsFromName(nome))
raise SystemExit(0 if resultado.wasSuccessful() else 1)
PY
```

Os demais comandos recompilam um corpo por execução na memória do processo de teste. `c-ordem` deve expor o `ValueError` injetado no subteste que espera o motivo. `medir-vigia` verifica um SELECT de decisões por chamada ao censo para N=1 e N=2. `vigia` retira o prefetch sob essa mesma medição. Resultados pela porta [nao verificado]. `chamados/tests/test_marco_por_motivo.py:243–265` · `chamados/tests/test_fila_validar_lote.py:70–126` · `chamados/tests/test_o306_vigia_carimbo.py:100–121,149–162` · `chamados/services/vigia_carimbo.py:103–106`

```bash
mutante() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - "$1" <<'PY'
import inspect
import sys

import django
django.setup()

from django.core.management import execute_from_command_line
from chamados import juizes, models
from chamados.services import carga_pergunta_viva, explicador, validacao, vigia_carimbo

id_mutante = sys.argv[1]
marco = 'test_marco_por_motivo'
fila = 'test_fila_validar_lote'
vigia = 'test_o306_vigia_carimbo'
pm = '_pergunta_materializada_de_fato'
sl = '_slot_esperado'
lo = 'classe_do_lote'
veto = "        if slot.motivo in ('VETO_TEMPORAL', 'SEM_RESPOSTA_VIGENTE'):\n"
guarda = ("    if tipo_resposta == 'data_hora' and not vigente:\n"
          "        return _ResultadoSlot('SEM_RESPOSTA_VIGENTE')\n")
cache = ("    if carga is not None and p.pk in carga.slots:\n"
         "        resultado = carga.slots[p.pk]\n"
         "        if isinstance(resultado, Exception):\n"
         "            raise resultado\n"
         "        return resultado\n")
c21 = ("    if not pergunta_respondida(DecisaoPergunta.composta(perg)):\n"
       "        return LOTE_B, dict(d, porque='SEM_RESPOSTA_VIGENTE')\n")
exp = " % motivo\n"

casos = {
    'a1': (juizes, pm, veto, veto +
           "            if p.batidas_plantadas.filter(retratada_em__isnull=True).exists():\n"
           "                return True\n", marco, 'ato_posterior_nao_absolve'),
    'a2': (juizes, pm, veto, veto +
           "            if getattr(p.chamado_destino, 'status_local', None) in ENCERRADOS:\n"
           "                return True\n", marco, 'veto_temporal_nao_absolve'),
    'b1': (juizes, sl, guarda,
           "    if tipo_resposta == 'data_hora' and not vigente:\n        resposta = p.resposta_colab\n",
           marco, 'data_hora_desfeita_nao_fornece'),
    'b2': (juizes, sl, "resposta_colab=resposta if vigente else ''",
           'resposta_colab=p.resposta_colab', marco, 'hora_desfeita_depende'),
    'c-ordem': (juizes, sl, guarda + cache, cache + guarda, marco, 'nao_relanca_erro_cacheado'),
    'c-motivo': (carga_pergunta_viva, 'carga_da_vida', '                self.slots[p.pk] = slot\n',
                 "                self.slots[p.pk] = (type(slot)('TIPO_SEM_SLOT')\n"
                 "                                    if slot.motivo == 'SEM_RESPOSTA_VIGENTE' else slot)\n",
                 marco, 'nao_relanca_erro_cacheado'),
    'c-identidade': (juizes, sl, '            raise resultado\n',
                     '            raise type(resultado)(*resultado.args)\n',
                     marco, 'nao_relanca_erro_cacheado'),
    'd': (juizes, sl,
          "        elif agora != carga.agora:\n            raise ValueError('agora diverge do horizonte da carga C6')\n",
          '', marco, 'horizonte_divergente_e_recusado'),
    'e1': (validacao, lo, c21, '', fila, 'lote_sem_resposta_vigente'),
    'e2': (validacao, lo, 'return LOTE_B, dict(d, porque=slot.motivo)',
           'return LOTE_A, d', fila, 'lote_com_data_impossivel'),
    'e3': (validacao, lo, "return LOTE_B, dict(d, porque='CONSULTA_CONFLITO_INDISPONIVEL')",
           'return LOTE_A, d', fila, 'lote_sem_consulta_de_conflito'),
    'conflito': (validacao, lo, "return LOTE_B, dict(d, conflito=True, porque='BATIDA_PROXIMA')",
                 'return LOTE_A, d', fila, 'batida_perto_tira_do_lote'),
    'explicador': (explicador, '_explicar_validada', exp,
                   " % (motivo + repr(p.resposta_colab))\n", marco, 'data_hora_desfeita_nao_fornece'),
    'vigia': (vigia_carimbo, 'classificar', ".prefetch_related('decisoes')", '', vigia,
              'test_as_PARTES_SOMAM'),
}

if id_mutante == 'medir-vigia':
    arquivo, seletor = vigia, 'test_o_plantio_NO_slot_e_o_controle_POSITIVO'
else:
    modulo, nome, antes, depois, arquivo, seletor = casos[id_mutante]
    real = getattr(modulo, nome)
    fonte = inspect.getsource(real)
    assert fonte.count(antes) == 1, (id_mutante, nome)
    ns = {}
    exec(compile(fonte.replace(antes, depois, 1), inspect.getfile(real), 'exec'), vars(modulo), ns)
    setattr(modulo, nome, ns[nome])
    if getattr(models, nome, None) is real:
        setattr(models, nome, ns[nome])

if id_mutante == 'explicador':
    from chamados.tests.test_marco_por_motivo import MarcoPorMotivoTest
    reabrir = MarcoPorMotivoTest._registrar_reabertura_sem_limpar_colunas

    def com_explicacao(self, p):
        horizonte = reabrir(self, p)
        mensagem = explicador._explicar_validada(p, [])
        self.assertIn('sem resposta vigente', mensagem)
        self.assertNotIn(p.resposta_colab, mensagem)
        return horizonte

    MarcoPorMotivoTest._registrar_reabertura_sem_limpar_colunas = com_explicacao

args = ['manage.py', 'test', f'chamados.tests.{arquivo}', '-k', seletor]
if id_mutante in {'vigia', 'medir-vigia'}:
    from django.db import connection
    from django.test.utils import CaptureQueriesContext
    from chamados.tests.test_o306_vigia_carimbo import VigiaCarimboSemRegistroTest

    censo = VigiaCarimboSemRegistroTest._censo
    tabela = connection.ops.quote_name(models.DecisaoPergunta._meta.db_table)

    def com_medicao(self, *args, **kwargs):
        with CaptureQueriesContext(connection) as consultas:
            resultado = censo(self, *args, **kwargs)
        n = sum(tabela in q['sql'] and q['sql'].lstrip().upper().startswith('SELECT')
                for q in consultas.captured_queries)
        print(f"vigia N={resultado['universo']} SELECT decisoes={n}")
        self.assertEqual(n, 1)
        return resultado

    VigiaCarimboSemRegistroTest._censo = com_medicao
    if id_mutante == 'medir-vigia':
        args += ['-k', 'test_as_PARTES_SOMAM']

execute_from_command_line(args)
PY
}

mutante a1
mutante a2
mutante b1
mutante b2
mutante c-ordem
mutante c-motivo
mutante c-identidade
mutante d
mutante e1
mutante e2
mutante e3
mutante conflito
mutante explicador
mutante medir-vigia
mutante vigia
```

### Cinco perguntas por campo

| Campo | Fonte | Escritor | Instante ou borda | Leitores, inclusive fora do diff | Significado único |
|---|---|---|---|---|---|
| `motivo` | Composta, projeção e C21 para elegibilidade. Parser existente e plantio para derivação. `chamados/models.py:4382–4390` · `chamados/juizes.py:368–415` | `_slot_esperado` constrói o valor imutável. `chamados/juizes.py:346–357,374–415` | Veto antes de elegibilidade e cache. Com slot elegível, o resultado é emitido ao concluir a derivação. `chamados/juizes.py:360–415` | Juiz de fato, carga, lote, explicador e vigia. A tradução chega a `explicar_pergunta`. `chamados/juizes.py:793–808` · `chamados/services/carga_pergunta_viva.py:119–128` · `chamados/services/validacao.py:1227–1229` · `chamados/services/explicador.py:52–62,144–150` · `chamados/services/vigia_carimbo.py:110–117` | Uma condição do slot na foto consultada. Categoria sem unidade ou fuso. `SLOT` exige `ts` e `tipo`. Os demais motivos têm ambos vazios. `chamados/juizes.py:346–357` |
| `ts` | Data e hora declaradas em `data_hora`. Derivador do plantio em `hora`, com a cadeia do turno podendo chegar a D+1. `chamados/juizes.py:315–335,389–415` · `chamados/services/materializacao.py:691–710` | `_slot_esperado` escreve ou repassa o instante em `SLOT`. `chamados/juizes.py:399–415` | Após as guardas e ao concluir a derivação. Em cache, pertence à mesma foto. `chamados/juizes.py:362–415` · `chamados/services/carga_pergunta_viva.py:88,117–128` | Os cinco leitores diretos. Fora do diff, a busca de janela, a comparação da batida de origem e o passivo que deriva o dia do plantio. `chamados/juizes.py:240–284,796–858` · `chamados/services/carga_pergunta_viva.py:127–144` · `chamados/services/validacao.py:1230–1232` · `chamados/services/explicador.py:62–95` · `chamados/services/vigia_carimbo.py:111–117` · `chamados/services/passivo_validado.py:330–351` | `datetime` com fuso, criado no fuso ativo do Django, com padrão `America/Sao_Paulo`. As janelas pertencem aos leitores. Juiz e carga usam ±20 min inclusivos, conflito usa ±60 min inclusivos, explicador usa ±20 min inclusivos e contexto diário `[−6h, +30h)`. `config/settings/base.py:93–95` · `chamados/juizes.py:240–284,399` · `chamados/services/carga_pergunta_viva.py:141–145` · `chamados/services/validacao.py:1152,1230–1232` · `ponto/turnos.py:2890–2896` · `chamados/services/explicador.py:64–85` |
| `tipo` | `E` no ramo `data_hora`. Em `hora`, o tipo entregue pelo plantio. `chamados/juizes.py:399–415` · `chamados/services/materializacao.py:624–626,672–710` | `_slot_esperado` escreve no resultado, que a carga preserva inteiro. `chamados/juizes.py:402–415` · `chamados/services/carga_pergunta_viva.py:126–128` | Junto de `ts` na conclusão de `SLOT`. `chamados/juizes.py:346–357,399–415` | Juiz de fato, janela de batidas e explicador. `chamados/juizes.py:240–251,796–858` · `chamados/services/explicador.py:62,86–95` | `E` ou `S` da batida. Categoria sem unidade, fuso ou limite temporal. `chamados/juizes.py:240–251,402–415` |
| `carga.agora` | Argumento `agora` de `CargaDaTarefa`. `chamados/services/tarefa_do_dia.py:520–525,826–839` | `CargaDaTarefa` guarda o argumento e `CargaDaVida` o copia. `chamados/services/tarefa_do_dia.py:525` · `chamados/services/carga_pergunta_viva.py:83–88` | Abertura da carga, antes do primeiro slot. `chamados/services/carga_pergunta_viva.py:83–119` | `_slot_esperado`, juiz de fato e `pergunta_viva`. Fora do diff, a tarefa repassa o mesmo atributo a C6. `chamados/juizes.py:362–366,718–719,925–932` · `chamados/services/tarefa_do_dia.py:629–637` | Horizonte da foto em `datetime` com fuso. Ato posterior é excluído e o empate incluído. `chamados/juizes.py:1428–1441` |
| `carga.slots` | Resultado ou falha da leitura antecipada 

[truncado em 20000 de 28245 caracteres]

## 21/11 17:11 · sessao 744498d1387b · E4C-F5-CONSTROI · construtor · origem=Code

**E4C-F5-CONSTROI · 3.3 · volta 3.** A cura Y está na cópia pedida. O RED da base falhou no trio do carimbo e na contagem, conforme `red_tail.txt:7–30`. A sintaxe dos três arquivos, o `git diff --check` e a conferência local por AST passaram. GREEN, A2 em execução, mutante e medição SQL por verbo **[nao verificado]**. A certificação cabe ao Code e ao opus final. `app/chamados/reconciliador.py:644–661` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:200–247` · `app/ponto/tests/test_selo_chokepoint_escrita.py:205–209,232–236` · `bin/suite.sh:316–325`.

### Diff por arquivo

- **Reconciliador.** Depois da trava existente do colaborador, reaplica `qs_perguntas_absolviveis` ao PK e à disputa original, com prefetch de `decisoes`. A candidata que sai do universo é descartada. A que permanece passa pela conferência do dono e pelo veto C20 existente. A porta recebe a instância relida e consulta o C6. Só o carimbo gravado conta e pode enfileirar o fecho. `validada_em__isnull` continua definido uma vez no seletor. [reconciliador.py](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/reconciliador.py:644) · `app/chamados/reconciliador.py:681–703` · `app/chamados/models.py:3375–3383`.
- **A2.** A variante `celula_absolveu` agora exige zero chamadas à porta, trio vazio, contagem zero, `validada_em == AGORA`, ato `VALIDACAO`, par `(True, True)`, `excluida` com par `(False, True)` e evento processado. Parametrizei o helper. A variante invertida chama diretamente o consumidor real para ler `out['perguntas']`. O gêmeo órfão mantém a chamada pelo comando e as asserções originais. [test_e4c_f5_guarda_reconciliador.py](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:200) · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:268–274` · `app/chamados/reconciliador.py:1429,1465–1473`.
- **Selo.** Somente os tetos dos dois cenários de fecho foram ajustados, **234 → 236** e **298 → 300**. Cada chave anota `+1 SELECT` da releitura e `+1 SELECT` do prefetch, com o commit a preencher pela sessão. A projeção assume uma candidata relida e elegível por cenário. A medição pela porta e o delta de `INSERT`/`UPDATE`/`DELETE` estão **[nao verificado]**. Qualquer delta de escrita medido aciona o PAREI com o número. [test_selo_chokepoint_escrita.py](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/ponto/tests/test_selo_chokepoint_escrita.py:205) · `app/ponto/tests/test_selo_chokepoint_escrita.py:232–236,444–481` · `app/chamados/reconciliador.py:647–651` · `f5_chokepoint_teto_medido_2011_0110.txt:4–5`.

### RED, GREEN e mutante

O primeiro comando seleciona o RED e seu controle tanto na base quanto após a cura. O segundo roda os A2, incluindo o gêmeo órfão e as guardas AST. O terceiro roda o selo. `bin/suite.sh:197–209,316–325` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:115–167` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:239–294`.

```bash
WT=/home/ronald/wt-codex-e4c-f5-constroi-165618
BASE=daba956c8e4c33df62acb8de099fb5e09a85d536

PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_f5_e2_absolvicao [nao verificado]
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_A2_ [nao verificado]
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only ponto -- -k test_selo_chokepoint_escrita [nao verificado]
```

**Mutante `instancia_pre_trava`.** O bloco repõe temporariamente só o reconciliador da base, que pula a releitura e entrega `p` à porta. Mantém o A2 invertido e restaura o reconciliador da cura ao sair. O RED 3.3 deve falhar no trio e na contagem. O A2 invertido deve falhar na primeira asserção, que exige zero chamadas à porta. O controle sem intercalação deve passar. Execução **[nao verificado]**. `volta2_parei.txt` **[nao verificado: linha do anexo]** · `app/chamados/reconciliador.py:644–661` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:153–167` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:227–230`.

```bash
(
  set -e
  alvo="$WT/app/chamados/reconciliador.py"
  cura=$(cat "$alvo" && printf '.')
  base=$(git -C "$WT" show "$BASE:app/chamados/reconciliador.py" && printf '.')
  trap 'printf "%s" "${cura%.}" > "$alvo"' EXIT
  printf '%s' "${base%.}" > "$alvo"

  rc_red=0
  rc_a2=0
  PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_f5_e2_absolvicao || rc_red=$? [nao verificado]
  PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_A2_evento_celula_absolveu_exclui_validada_apos_selecao || rc_a2=$? [nao verificado]
  printf 'RED 3.3 rc=%d | A2 invertido rc=%d\n' "$rc_red" "$rc_a2"
)
```

`LEI-AKITA origem=retratar_perguntas_absolvidas_por_celula, testemunha=seletor unico relido sob a trava + C6 da porta sobre a fresca, RED=test_f5_e2_absolvicao_resposta_entre_prefetch_e_trava_preserva_pergunta_viva, quem-mais-le=consumidor do EventoCelula (subtotal) + cartório (carimbo antes do gate) + reconciliação do colaborador (subtotal) + vigia (emissões do gerador), juizes novos=0, padrao=Single Source of Truth`. `app/chamados/reconciliador.py:644–661,681–703,1429,1496–1506` · `app/chamados/models.py:3375–3383` · `app/core/juizes.py:290` · `app/ponto/services/cartorio.py:1018–1033` · `app/chamados/management/commands/reconciliar_chamados.py:89–93`.

### Autoconferência · campos=26

`R` = `app/chamados/reconciliador.py`, `M` = `app/chamados/models.py`, `J` = `app/chamados/juizes.py`, `T` = `app/chamados/tests/test_e2_pergunta_so_ao_c6.py`. Os caminhos `services/` nesta matriz são relativos a `app/chamados/`.

| Campo ou valor | (a) Fonte | (b) Escritor | (c) Instante | (d) Leitores | (e) Significado único |
|---|---|---|---|---|---|
| 1. Candidata e PK | Universo do seletor `R:681–703` | Nascimento `_nascer` `M:3224–3234` | Emissão antes da trava, releitura por PK e disputa sob a trava `R:636,644–651` | E2 e vigia `R:647–661`, `app/chamados/management/commands/reconciliar_chamados.py:91–93` | O PK identifica a pergunta. A releitura confirma a elegibilidade SQL `R:647–651,688–703` |
| 2. Colaborador, `_cid` e dono | Argumento e FK da disputa `R:635,689`, `M:1892–1896` | `_cid` local e vínculo da pergunta no nascimento `R:635`, `M:3229–3232` | Mesmo PK de trava, com conferência do dono na relação relida `R:644–653` | Trava, C20 e C6 `R:645,652–654`, `J:991–997,1478–1482` | A associação em memória usa o colaborador do mesmo PK e seu memo `R:647,652–653,689`, `J:1174–1195` |
| 3. Trava e registro do dono | Linha e registro transacional `M:2830–2848,2874–2878` | `trava_na_ordem` pela porta existente `M:2830–2839` | Colaborador primeiro no `atomic`, antes da releitura `R:644–649` | Guarda E2 e porta da resposta `R:652`, `services/disputa_emissao.py:1478–1485` | O registro identifica o dono já travado `services/trava_do_colaborador.py:29–47,55–63` |
| 4. `decisoes` | Sequência e complemento legado `M:4352–4379` | `DecisaoPergunta.registrar` `M:4428–4453` | Prefetch da instância entregue à porta, sob a trava `R:644–656` | Composta, C6 e C7 `M:4382–4390,2017–2027`, `J:1406–1409` | A sequência relida inclui o ato interposto no RED `T:131–147` |
| 5. `respondida_em` | Instante vigente recomposto do ato `M:4368–4390` | Portas de resposta e reabertura `services/disputa_emissao.py:1499–1537,1630–1737` | Composta lê os atos carregados sob a trava `R:644–656`, `J:1406–1409` | C6, C21 e C7 `J:1426–1428`, `M:2022–2027` | O instante representa a resposta vigente. A reabertura pode limpá-lo `M:4368–4379` |
| 6. `resposta_colab` | Texto da projeção `M:4382–4390` | Portas de resposta e reabertura `services/disputa_emissao.py:1499–1502,1630–1715` | Coluna relida sob a trava `R:644–649` | Composta, C21 e comparação com a luz `M:4388–4390`, `J:1359–1364,1426–1440` | Texto da resposta. No RED, `19:45` discorda da luz de `17:02` `T:137–146` |
| 7. `validada_em`, `validada_por_id` | Ato vigente e colunas físicas `M:3683–3689,4382–4399` | Validação escreve o ato. As colunas também passam pelas portas de devolução, resposta e carimbo no modo validar `services/validacao.py:161–175,284–296`, `services/disputa_emissao.py:1503–1514`, `M:3367–3374` | O seletor reaplica o filtro físico sob a trava. Para quem passa, os atos são carregados nesse ponto `R:644–651,691` | Seletor, C6, C7 e guarda do par `R:691`, `J:1408–1411`, `M:2022–2027,3384–3393` | Coluna nula delimita o universo SQL. A composta responde pela validação vigente. O A2 verifica a preservação de ambas `R:691`, `M:4385–4399`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:227–237` |
| 8. `celula_id` | FK do fato `M:2623–2625` | Nascimento e rechaveamento `M:3229–3232`, `services/fio.py:1087–1092` | FK e relação relidas sob a trava `R:644–649` | Seletor, C6 e outbox `R:692–702`, `J:1423–1425`, `M:3472–3496` | Identifica a célula vinculada. FK nula exclui a candidata `R:692` |
| 9. Veredito e via da célula | Lavra da célula `app/ponto/portas/celula.py:1032–1045` | `lavrar_veredito` `app/ponto/portas/celula.py:1090–1098` | Relação relida sob a trava `R:644–649` | Seletor, C6 e lastro `R:698–702`, `J:1423–1440,1458–1474`, `app/ponto/services/lastro.py:145` | Veredito registra o julgamento e via identifica a origem. O C6 interpreta a pendência `J:1423–1440,1458–1474` |
| 10. DNA, ata e lâmpada | DNA e ata da célula `J:935–953,1351–1360` | Portas da célula `app/ponto/portas/celula.py:294–306,1032–1067,1090–1098` | Célula relida sob a trava `R:644–649` | C6 e juiz do marco `J:935–975,1436–1440,1463–1474` | DNA dá o marco e ata dá a luz. Só `True` confirma o marco `J:935–953,1472–1474` |
| 11. `data_evento`, `conversa` | Dia e discriminador da pergunta `M:2633–2645` | `gravar_dia_do_fato` recebe o dia. O nascimento fixa `conversa` `services/fio.py:846–854`, `M:3004–3013,3224–3232` | Colunas relidas sob a trava `R:644–649` | C20, C6 e outbox `J:337–341,991–997,1475–1482`, `M:3472–3476` | O discriminador identifica quando o dia gravado é civil de conversa ou dia de turno `J:337–341`, `M:3004–3013` |
| 12. `motivo`, `tipo_resposta` | Colunas da pergunta `M:2631,2652` | Porta de nascimento `M:3035–3036,3229–3232` | Colunas relidas sob a trava `R:644–649` | C6, comparação com a luz e porta de resposta `J:927–946,1347–1352`, `services/disputa_emissao.py:1449–1473` | Motivo identifica a cobrança. Tipo identifica o formato da resposta `J:927–946,1347–1352` |
| 13. Destino e `status_local` | FK e estado do chamado `M:3691–3698`, `J:846–864` | FK pelas portas de nascimento/reuso e costura, com `SET_NULL` na exclusão. Estado por `_gravar_estado` `services/fio.py:857–876`, `M:213` | Relação relida sob a trava `R:644–649` | Memo C6 e outbox `J:846–864`, `M:3463–3470` | FK identifica a resolvedora quando presente. Estado pertence ao chamado `M:3691–3698`, `J:862–864` |
| 14. Veto C20 | Tranca e trilha de toque `J:979–997,1067–1085` | Portas de trancar e reabrir para a tranca. Veto calculado por leitura `app/ponto/services/fechamento.py:1400,1432`, `J:979–989` | Veto consultado também sob a trava com a pergunta relida e o memo existente `R:652–656,725–730`, `J:1101–1117` | Gerador, E2, C6 e C7 `R:654,729`, `J:1414`, `M:2022–2027` | Tranca aplicável barra o carimbo definitivo `R:654–655,725–729` |
| 15. C6 e `_pv_memo` | Juiz registrado sobre a composta `app/core/juizes.py:290`, `J:1406–1409` | Memo calculado pelo juiz por instância `J:895–908` | Gerador antes da trava. Porta julga a instância relida com o carimbo em memória `R:644–656,723`, `M:3361–3383` | Gerador, porta e C7 `R:723`, `M:3375–3383,2029–2035` | C6 decide vitalidade. O RED exige `RECUSA_C6` após a resposta interposta `T:137–157` |
| 16. Autor `_user` | Usuário `sistema` `app/core/utils.py:6–18` | Inicialização local pelo helper `R:633,638–639` | Carregado antes da trava `R:638–645` | Carimbo e fecho `R:656,673–675` | Autor das ações automatizadas do E2 `app/core/utils.py:6–18`, `R:656,673` |
| 17. Borda do relógio | `timezone.now()` das portas e horizonte do C6 `M:3361,2118`, `services/disputa_emissao.py:1494–1501`, `J:880–899,1383–1403` | Cada porta atribui o instante do próprio ato `M:3361–3366,2118–2122`, `services/disputa_emissao.py:1499–1501,1535–1537` | Resposta e carimbo sob a trava de seus atos `services/disputa_emissao.py:1478–1499`, `R:644–656` | Sequência, carimbo, janela viva e fecho `M:4363–4374,3361–3366,2118–2122`, `J:1475–1476` | Instante do ato, instante do carimbo e horizonte têm papéis próprios `J:1383–1403`, `M:3361–3366`, `T:137–147` |
| 18. Trio do carimbo | Colunas e via `VIA_CELULA_ABSOLVEU` `M:2694,3324–3328` | `_escrever_carimbo` pela porta `M:3324–3328,3365–3405` | Ausência de `materializada_em` reaplicada sob a trava. A porta salva após o C6 `R:644–656,690`, `M:3375–3405` | Seletor, composta, lastro e outbox `R:690`, `M:4388–4390,3468–3476`, `J:684–696` | Carimbo já existente exclui a candidata. Via e autor identificam o registro `R:690`, `M:3324–3328` |
| 19. `_gravou` e `recusa_do_carimbo` | Resultado da porta `M:3317–3322,3353–3405` | `carimbar` `M:3330–3405` | Julgamento da relida dentro do `atomic` `R:644–657`, `M:3375–3405` | Bool no E2. Motivo na materialização e no RED `R:656–659`, `services/materializacao.py:2222–2228`, `T:126–129,157` | Bool indica gravação. `RECUSA_C6` identifica a recusa por vitalidade `M:3375–3383,3405` |
| 20. Contagem `n` | Carimbos gravados `R:632,656–659,678` | Incremento local `R:659` | Após sair do `atomic` com sucesso da porta `R:644–659` | Evento e reconciliação `R:1429,1496–1506` | Exclusão ou recusa soma zero `R:650–659`, `T:153–163`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:227–230` |
| 21. Disputa, PK e `_disputas` | FK e mapa local `M:2626–2629`, `R:634,660–669` | Laço dos carimbos bem-sucedidos `R:657–661` | A disputa relida entra após a gravação `R:647–661` | Fecho e log `R:669–677` | PK deduplica a fila. Troca de disputa exclui a candidata desta passagem `R:647,660–669` |
| 22. `disputa.fechada_em` | Coluna da disputa `M:2118–2122,2335–2363` | Fecho e portas de reabertura `M:2118–2122,2312–2363` | Triagem pela relação relida sob a trava e nova leitura antes de C7 `R:647–661,669–672` | E2, porta de fecho e devolução `R:660–672`, `M:2061–2065`, `services/validacao.py:298–303` | `None` indica disputa aberta, inclusive após reabertura `M:2335–2363` |
| 23. Veredito C7 | Respostas sem veredito e `fio_vivo` `M:2022–2035` | `pode_fechar` calcula. `fechar` aplica a guarda `M:2029–2048,2071–2088` | Depois do laço de carimbos. A guarda da porta precede a trava do fecho `R:669–675`, `M:2071–2097` | E2 e porta de fecho `R:672–675`, `M:2071–2088` | Bool autoriza o fecho quando ambos os impedimentos estão ausentes `M:2029–2035` |
| 24. `fechada_por`, `resolucao`, `motivo_fechamento` | Autor, texto do E2 e motivo escolhido pela porta `R:673–675`, `M:2088,2118–2122` | `fechar` grava. Reaberturas limpam `M:2118–2122,2335–2363` | Transação própria do fecho, após o laço `R:669–675`, `M:2090–2097,2118–2122` | Sinal do pai e auditoria `M:2122–2126`, `app/chamados/signals.py:42–43` | Campos registram autor, texto e motivo do fecho `M:2119–2126` |
| 25. Foto da lavra | `veredito_lavrado`, `julgado_em`, `janela_expira_em` `app/chamados/veredito_lavrado.py:279–281` | Porta da lavra. O E2 usa o padrão `lavrar_veredito=False` `app/chamados/veredito_lavrado.py:270–275`, `M:3330,3394–3403`, `R:656` | Foto em memória após C6 e antes do save `M:3394–3403` | Restauração em falha e leitura do veredito lavrado `app/chamados/veredito_lavrado.py:284–289`, `M:2776–2805` | Vazio indica ausência de lavra. `indeterminado` é veredito próprio `M:2783–2798` |
| 26. Falhas e PK no log | Exceção por pergunta ou disputa `R:662–663,676–677` | `_log.exception` nos dois tratadores `R:663,677` | Após falha do ato correspondente `R:644–677` | O laço segue e retorna a contagem `R:662–678`. Leitores externos do log **[nao verificado]** | As mensagens distinguem falha de carimbo e de fecho pelo PK `R:663,677` |

_7731 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
