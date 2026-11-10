# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 21:38** · publicado em **09/11/2026 21:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 21:38 · sessao 638745cb9017 · FIO-E0-V4-E4A-2 · censo · origem=Code

A cópia ainda contém o FIO MUDO escritor, inclusive um dry que executa as portas e depois faz rollback. Retirar apenas `--apply` do cron não transforma o comando em leitor. A lista tem **1 juiz por varredura**; a matriz declara **19/20 verdes**, e esvaziar a lista não altera automaticamente esse numerador. (`app/chamados/management/commands/disparar_perguntas_competencia.py:270-317`; `app/config/crons.py:2032-2048`; `app/core/contratos_estruturais.py:262-270,1053-1064`)

A identidade Git do HEAD, a ancestralidade da E4a-1 e a implantação em produção são **[nao verificado]**. Há evidência local de EXPAND/C11 aplicado, mas o dado fornecido não comprova uma rodada posterior à implantação com **FIO MUDO 0**. (`app/chamados/models.py:3017-3019`; `app/chamados/reconciliador.py:1384-1387`; prova da rodada: **[nao verificado]**)

1. **Ramos do comando, escritas e saída**

O primeiro universo, das células, apenas acumula `deixou`: células passadas da janela, com extensão até anteontem em `--comp-corrente`, excluindo ontem enquanto ainda pertence à janela do vigia de hora. Ele consulta `motivos_que_o_evento_deixou`; não chama o escritor nesse laço. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`)

O **FIO MUDO** é o segundo universo: chamado vivo ligado a célula na janela, sem pergunta viva entre suas resolvedoras. Exclui parcial só lista, ausência de motivo vencido pela ata, colaborador não ativo, `_ja_perguntou` e existência de linha do fato; deduplica por colaborador/dia/motivo. O primeiro motivo elegível de cada passagem entra em `_mudos`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141-175,215-219`)

| Condição | Comportamento |
|---|---|
| `--alarme`, inclusive combinado com `--apply` | Retorna antes do escritor. Sai 2 se houver pendência da célula ou FIO MUDO; se ambos existirem, o diagnóstico emitido é o da célula. (`app/chamados/management/commands/disparar_perguntas_competencia.py:247-260`) |
| Sem alvos FIO MUDO | Não executa `_rodar`. Com `--apply`, ainda alarma a pendência da célula; sem chave, apenas retorna. (`app/chamados/management/commands/disparar_perguntas_competencia.py:261-264`) |
| Com alvos e `--apply` | Executa `_rodar` em `transaction.atomic()`. Silencia push por padrão; `--com-push` dispensa esse silenciamento. Confirma a transação e só depois pode sair 2 pelo vigia da célula. (`app/chamados/management/commands/disparar_perguntas_competencia.py:289-304`) |
| Com alvos, sem `--apply` e sem `--alarme` | Executa o mesmo `_rodar`, com push silenciado, dentro de atomic; imprime deltas e força `RuntimeError('rollback')`. Portanto há chamadas de escrita e execução de suas consequências transacionais, seguidas de rollback. (`app/chamados/management/commands/disparar_perguntas_competencia.py:306-317`) |

A porta chamada é `fabrica_da_celula.perguntar(colab, dia, [motivo], origem='disparo_competencia', origem_carimbo='emissor fio mudo')`. Ela tenta desfazer carimbo sem lastro, cria/agrega disputa e gera perguntas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:270-283`; `app/chamados/services/fabrica_da_celula.py:133-148`)

| Efeito alcançável pela porta | O que grava e por onde |
|---|---|
| Descarimbar pergunta sem lastro | Relê sob trava, exige pergunta viva pelo C6, chama `descarimbar`, zera `materializada_em`, `materializada_por` e `via_resolucao`, registra trilha. Pode reabrir disputa fechada. (`app/chamados/services/relavra_via.py:219-261`; `app/chamados/models.py:3203-3212`) |
| Reabrir disputa | Limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; salva e anexa ocorrência ao chamado-pai. (`app/chamados/models.py:2163-2180`) |
| Agregar disputa existente | Acrescenta `motivos`; atualiza contexto com data, `qtd` e `ultima_em`, pela `_abrir_ou_agregar_disputa`. (`app/chamados/services/disputa_emissao.py:535-565,662-667`) |
| Criar disputa/pai | Abre ou reutiliza chamado `disputa_supervisao`, com contexto, urgência e SLA; pode anexar ocorrência de reuso; cria `DisputaSupervisao` e trilha `disputa_aberta`. (`app/chamados/services/disputa_emissao.py:568-599`) |
| Criar pergunta | `gerar_perguntas_disputa` chama `PerguntaDisputa.abrir`; a inserção grava identidade/dia, célula, disputa, motivo, destino e conteúdo recebido, registra assinaturas projetadas e trilha `pergunta_emitida`. (`app/chamados/services/disputa_emissao.py:444-452`; `app/chamados/models.py:3013-3027`) |
| Reutilizar/costurar pergunta | A porta pode trocar célula/destino, lavrar novamente e salvar; acorda a célula anterior. Pergunta nova órfã de motivo do dia pode passar por `reatar_pergunta_orfa`, com savepoint próprio. (`app/chamados/models.py:2962-2983,2997-3007`; `app/chamados/services/disputa_emissao.py:453-469`) |
| Notificar | Disputa nova ou motivo novo pode enviar push quando não silenciado. (`app/chamados/services/disputa_emissao.py:669-687,1748-1772`) |

O ramo `[LASTRO]` merece ressalva: em estado coerente e estável, uma pergunta materializada pertencente ao fato já tende a barrar `_mudos` antes de chegar ao descarimbo. Não é possível declará-lo absolutamente morto: seleção e escrita são consultas separadas, e seus filtros diferem — identidade usa coluna `colaborador` e instância; descarimbo busca por `disputa__colaborador`, motivo e dia. Ocorrência concreta de concorrência/inconsistência: **[nao verificado]**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:215-219`; `app/chamados/models.py:2753-2755`; `app/chamados/services/fabrica_da_celula.py:142-144`)

Se nenhum ramo escrevesse, o corpo atual ainda oferece estes diagnósticos de leitura; quais serão preservados na versão futura depende da decisão de interface. (`app/chamados/management/commands/disparar_perguntas_competencia.py:220-258`)

| Texto atual | Número/recorte |
|---|---|
| `[EMITE] FIO MUDO: N — chamado vivo, dia ACUSADO pela lei, sem uma pergunta viva` | `len(_mudos)`, antes de `--limite`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:220-222`) |
| `[BARRADO] desligado N … · ja tem pergunta viva N · FIO DECIDIDO N … · PARCIAL NA LISTA DO ADMIN N …` | Quatro contadores de barreiras do laço FIO MUDO. (`app/chamados/management/commands/disparar_perguntas_competencia.py:223-228`) |
| `ALVOS POR EMPRESA E PORTA: empE fio_mudo=N · …` ou `nenhum` | Distribuição de `_mudos`, antes do limite. (`app/chamados/management/commands/disparar_perguntas_competencia.py:229-234`) |
| `janela INI -> FIM` | Janela solicitada/calculada. (`app/chamados/management/commands/disparar_perguntas_competencia.py:239`) |
| `FABRICA-POR-EVENTO: o julgamento deixou N pergunta(s) do caminho da celula sem emitir, em C celula(s) (esperado 0; VIGIA -- este lote nao as escreve)` | Motivos em `deixou` e pares distintos colaborador/dia. (`app/chamados/management/commands/disparar_perguntas_competencia.py:240-242`) |
| `VIGIA: o evento deixou N pergunta(s) por emitir (esperado 0)` | Mesmo `len(deixou)`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:243`) |
| `ALVOS: N perguntas em C colaboradores (fio mudo)` | `falta`, depois de `--limite`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:235,244-245`) |

Os textos ligados à execução do escritor são `[LASTRO] N carimbo(s)…`, `APLICADO perguntas=N disputas=N chamados=N[ (push silenciado)]`, `DRY perguntas=N disputas=N chamados=N` e `(dry-run: nada gravado)`. Os três deltas vêm de contagens globais antes/depois, não de um relatório por alvo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:266-268,284-287,296-317`)

Os leitores e transportadores localizados são:

- **Placar:** raspa a última ocorrência de `^FABRICA-POR-EVENTO: o julgamento deixou \([0-9]*\) .*` em `logs/fio_mudo.log`; exige mtime de hoje, senão publica `?`. Expõe `perguntas_que_o_julgamento_nao_fez=N`, esperado 0, fora do TOTAL. (`bin/placar_code.sh:388-390`)
- **Corrente/cron:** `crons corrente` fornece `log<TAB>cmd gatilho empresas teto args`; `encadeado.sh` passa `ENC_LOG` e redireciona stdout/stderr do elo para o log. Não raspa os contadores para decidir o próximo elo. (`app/core/management/commands/crons.py:37-42`; `bin/encadeado.sh:45-68,99-104`)
- **`logs/fim`:** o runner usa o rc, criando arquivo de fim em 0 **ou 2**; o dependente verifica a existência desse arquivo. O conteúdo numérico do stdout não participa. (`bin/cron_run.sh:11-25`; `bin/encadeado.sh:81-94`)
- **JSONL/saúde:** o runner grava `nome,emp,ini,fim,dur,exit`; `crons_quebrados` trata 0/2 como execução não quebrada, e `pipeline_placar` distingue rc 2 como alarme. Este último deriva esperados apenas de `CRONS`, enquanto disparar está em `ENCADEADOS`. (`bin/cron_run.sh:13`; `app/core/services/crons_quebrados.py:16,24-47`; `app/core/management/commands/pipeline_placar.py:38-49,60-80`; `app/config/crons.py:1521`)
- **Testes:** há parser do prefixo `FABRICA-POR-EVENTO` e asserções sobre FIO MUDO, distribuição por empresa e dry. (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:91-95,152-184`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:195-203,226-232`)
- **ESTADO:** lê `placar_estrutural.RESULTADOS`; o R6 contém uma cópia da frase `contratos_estruturais: 19/20 verdes`. Não é leitura do stdout do disparar nesse caminho. (`bin/gerar_estado.py:60-86,98-104`; `app/core/placar_estrutural.py:433-441`)
- **Hooks:** leitor direto de `[EMITE] FIO MUDO`, `APLICADO` ou `DRY` fora dos testes: **[nao verificado]**. O documento do verificador exige auditar escritas por cron e escritas sem trilha; os hooks de compactação chamam o handoff, que raspa a frase estrutural do R6. (`.claude/agents/verificador.md:17`; `.claude/settings.json:14-30`; `bin/handoff_sessao.sh:185-208`)

2. **Cron, gatilhos, conjuntos e números**

A entrada é `ENC`, com `gatilho='apurar_furos_diarios'`, `args='--comp-corrente --apply'`, `log='fio_mudo.log'`, estágio `cobranca`, teto 900 e dependências explícitas de `processar_cartorio` e `apurar_furos_diarios`. (`app/config/crons.py:1521-1523`)

O dependente direto por gatilho é `reconciliar_perguntas_orfas --alarme`. Depois vêm `lavrar_deslogados`, `lavrar_he_pendente` e os demais elos da corrente. O primeiro lê perguntas órfãs que o evento deveria ter encerrado; não cria a pergunta faltante do FIO MUDO. (`app/config/crons.py:1526-1527,1539-1541,1554-1556`; `app/docs/CRONS.md:39-48`; `app/ponto/management/commands/reconciliar_perguntas_orfas.py:20-36`)

Não existe uma declaração Python `VIGIAS` correspondente ao nome usado no item: os vigias são uma compreensão dentro de `PAPEL_DO_CRON`. O subgrafo gerado, esse sim, chama-se `VIGIAS`. (`app/config/crons.py:2164-2169`; `app/core/management/commands/gerar_diagrama.py:393-401`)

| Registro/conjunto | Hoje | Se a mudança for coerente |
|---|---|---|
| `JUIZES_POR_VARREDURA` | Único membro: disparar; comentário justifica permanência pelo FIO MUDO. | Tupla vazia. (`app/config/crons.py:2032-2048`) |
| `PAPEL_DO_CRON` | Disparar recebe `juiz` da compreensão da tupla. | Precisa receber `vigia` na compreensão correspondente. Só retirar o nome elimina sua declaração de papel. (`app/config/crons.py:2164-2169,2340-2345`) |
| Agenda | Continua em `ENCADEADOS`, com gatilho e dependências próprios. | Reclassificação de papel não exige, por si, sair da corrente. (`app/config/crons.py:1521-1527,1787-1798,2348-2357`) |
| `juizes_por_varredura()` | **1**, derivado de todas as entradas cujo papel é `juiz`. | **0**, após retirada e reclassificação. (`app/config/crons.py:2048,2348-2357`) |
| Diagrama do tabuleiro | Versionado com **40 vigias / 1 juiz** e nó do disparar ligado ao juiz. | A mesma reclassificação isolada produz **41 / 0**, pelo gerador que conta todas as entradas. (`app/docs/ARQUITETURA.mmd:209-219`; `app/core/management/commands/gerar_diagrama.py:242-265`) |

Quem lê ou conta essa classificação:

- `bin/placar_code.sh` publica `juizes_por_varredura=N`; o valor entra no TOTAL. (`bin/placar_code.sh:482-486,555-556`)
- `vigia_loop.escritas_por_cron()` usa por padrão os juízes vivos e consulta sete dias de `LogAuditoria`, ator `sistema:<cron>`; o placar publica uma linha por cron. Retirar o nome também o retira dessa medição padrão. (`app/chamados/services/vigia_loop.py:61-99`; `bin/placar_code.sh:201-209`)
- O selo do tabuleiro exige igualdade entre lista declarada e juízes derivados. (`app/core/tests/test_contract_tabuleiro.py:25-28`)
- O selo de delegação percorre `JUIZES_POR_VARREDURA` e rejeita exceção cujo cron já saiu dela. (`bin/tests/test_papel_prazo_nao_deriva.sh:302-328`)
- A matriz estrutural referencia a lista como exceção da célula chamado/juiz; o selo exige exceções vazias para aceitar verde. (`app/core/contratos_estruturais.py:262-270`; `app/core/tests/test_selo_contratos_estruturais.py:71-96`)

**19/20 → 20/20 não é consequência automática da lista 1 → 0.** `verdes()` conta flags declaradas. A célula chamado/juiz ainda usa `verde=False` e referencia também `PENDENTES_CHAMADO`, já vazio. Esvaziar a lista mantendo a matriz intacta conserva **19/20**; declarar a célula verde e cumprir seus selos permitiria **20/20**. O denominador permanece 20. (`app/core/contratos_estruturais.py:40-42,62-73,262-270,1038-1064`; `app/core/juizes.py:465-466,1355`)

Os leitores desse N/20 são TICKETS e a ferramenta de arquitetura; ESTADO/handoff recebem a cópia do R6, cuja igualdade com a frase do juiz é selada. (`app/core/placar_tickets.py:215-217`; `app/core/arquitetura_leitura.py:55-82`; `bin/gerar_estado.py:73-83`; `bin/handoff_sessao.sh:192-208`; `app/core/tests/test_selo_r6_frase_do_juiz.py:20-25`)

3. **Cobertura dos escritores: mesma porta não prova mesmo fato**

Há outro caminho executável para a fábrica: o consumidor chama `perguntar_por_eventos` depois das retratações/lastro e antes de marcar os eventos processados; relê as células **atuais das datas**, deduplicando datas do lote. Falha da fábrica mantém eventos pendentes com tentativa/erro. O consumidor está declarado a cada minuto; execução implantada: **[nao verificado]**. (`app/chamados/reconciliador.py:1384-1401`; `app/chamados/services/fabrica_da_celula.py:259-273`; `app/config/crons.py:394`)

| Fato/efeito | Cobertura alternativa comprovada na cópia | Limite |
|---|---|---|
| Pergunta faltante de célula elegível | Cartório/evento chama `perguntar_pela_celula`, que usa o mesmo `perguntar` do lote; cobre criação/agregação, perguntas, costura e trilhas descritas acima. (`app/chamados/services/fabrica_da_celula.py:177-190`) | Exige que a data seja alcançada e passe pelas guardas da fábrica. (`app/chamados/services/fabrica_da_celula.py:85-98,259-273`) |
| Carimbo sem lastro | Julgamento coleta carimbadas e chama a mesma porta de descarimbo. (`app/chamados/services/fabrica_da_celula.py:121-129,177-180`) | Esse passe usa `reabrir_fechada=False`; o lote usa o default `True`. Portanto a cobertura de reabertura de disputa fechada não é idêntica. (`app/chamados/services/fabrica_da_celula.py:142-144,180`; `app/chamados/services/relavra_via.py:187,233-239`) |
| Nascimento de chamado de turno | `abrir`/`criar` emitem evento quando pertencem aos módulos de turno e têm célula resolvida. (`app/chamados/models.py:971-980,1093-1099`) | FIO MUDO aceita qualquer chamado vivo com célula; não aplica esse filtro de módulo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`) |
| Retorno de chamado a VIVOS | Signal emite evento antes da guarda de reentrada do cartório. (`app/chamados/signals.py:194-210`) | Emissão é best-effort com savepoint/`engolir`: pode haver retorno sem evento. Nesse caso, não há garantia desse reparo pelo consumidor. (`app/chamados/signals.py:194-207`) |
| Mudança relevante/perda de pergunta | Save relevante acorda células; ressuscitação em lote e reuso também emitem. (`app/chamados/signals.py:241-257`; `app/chamados/models.py:1563-1568,2997-3007`) | Depende das portas/gatilhos executados; estoque já existente não é automaticamente um evento novo. (`app/chamados/management/commands/consumir_eventos_celula.py:28-31`; `app/chamados/reconciliador.py:1278-1310`) |
| Estoque antigo | Existe `consumir_eventos_celula --semear`. (`app/chamados/management/commands/consumir_eventos_celula.py:14-15,28-30`) | É opção explícita; o ramo normal não semeia. (`app/chamados/management/commands/consumir_eventos_celula.py:28-31`) |
| Push opcional do lote | A fábrica do evento sempre silencia push. (`app/chamados/services/fabrica_da_celula.py:188-190`) | Não cobre o modo opcional `--com-push` do disparar. (`app/chamados/management/commands/disparar_perguntas_competencia.py:289-295`) |

A divergência de universo é comprovável: a fábrica barra `dia_sem_trabalho`, futuro e veredito que não acusa, e encaminha limbo entregue para Pauta DP. O FIO MUDO não consulta essas mesmas guardas: trabalha com chamado vivo, ata, teto e seus próprios filtros. Há teste que fixa célula `concorde` com FIO MUDO ainda escrevendo. (`app/chamados/services/fabrica_da_celula.py:85-98,185-190`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141-175,215-219`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:152-159`)

Assim, ficam sem **substituto integral demonstrado**: o reparo por varredura de alvo cujo evento se perdeu, de estoque não alcançado e de alvo aceito pelo FIO MUDO mas recusado pela fábrica. Existência e quantidade desses casos em produção: **[nao verificado]**. (`app/chamados/signals.py:194-207`; `app/chamados/services/fabrica_da_celula.py:85-98,185-190`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141-219`)

O reconciliador pode reatar **pergunta órfã existente**; isso não equivale a criar linha ausente. A criação faltante no consumidor aparece justamente pela nova chamada à fábrica. (`app/chamados/reconciliador.py:1384-1387,1432-1441,1533-1556`)

CADASTRO sem turno e limbo entregue já têm escritores próprios na fábrica; **não são escritas atuais do disparar**. CADASTRO produz pauta por colaborador sob trava; limbo entregue produz pauta por dia/motivo. (`app/chamados/services/fabrica_da_celula.py:172-186,222-255,324-343`; `app/chamados/management/commands/disparar_perguntas_competencia.py:282-283`)

A tranca atual também não substitui emissão faltante: encerra chamado/disputa, lê a morte da pergunta pelo C6 e roda com cartório calado. `trancar_periodo` não contém o bloco C11 do anexo; chama `disparar_no_ato` em best-effort. (`app/chamados/services/competencia_trancada.py:8-16,177-205`; `app/ponto/services/fechamento.py:1400-1429`)

4. **Testes atuais, selos e situação do PARQUE** [nao verificado]

Resultado de execução dos testes nesta auditoria: **[nao verificado]**. Os contratos abaixo foram identificados no fonte. [nao verificado]

| Contrato atual fixado | Teste | [nao verificado]
|---|---|
| Célula com pendência: apply não cria pergunta, contador 1, rc 2 | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:98-103` |
| FIO MUDO: apply cria 1, contador da célula 0, rc 0 | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:152-159` |
| Retratada permite renascer; limbo pode escrever FIO MUDO e terminar rc 2 | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:161-184` |
| Parcial só lista: zero perguntas e rc 0 | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:186-194` |
| Cron e argv da corrente contêm `--apply` | `app/chamados/tests/test_selo_emissor_fio_mudo.py:215-219`; `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19-27` |
| Dry não deixa pergunta persistida e imprime aviso de rollback | `app/chamados/tests/test_selo_emissor_fio_mudo.py:226-232` |
| Um ciclo `comp_corrente/apply` devolve pergunta viva ao chamado mudo | `app/chamados/tests/test_selo_emissor_fio_mudo.py:273-298` |
| Há exatamente 1 juiz por varredura | `app/ponto/tests/test_varredura_lote_prazo.py:47-62` |
| Hoje não conta no vigia; depois passa a contar; FIO MUDO respeita marco vencido | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:125-136,422-

[truncado em 20000 de 30906 caracteres]

## 09/11 21:34 · sessao a72b1f524879 · - · - · origem=Ronald (tmux)

Resultado da análise estática, somente leitura; execução real do cron, saldo FIO MUDO, banco e logs em produção: [nao verificado].

- **A escrita do disparar é o caminho FIO MUDO.** Seleciona chamados `status_local__in=VIVOS` com célula na janela; não restringe `MODULOS_DE_TURNO`. Exclui chamado com pergunta viva, parcial só lista, colaborador não ativo, `_ja_perguntou`, fato já existente; toma o primeiro motivo pela ata ainda não visto. A seleção termina em `_mudos.append(...)`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `:148`, `:154`, `:158`, `:169`, `:172`, `:215`, `:219`.)
- **O mesmo `_rodar()` escreve com e sem `--apply`.** Com apply, confirma atomic; sem apply, executa a fábrica sob atomic e força `RuntimeError('rollback')`. `--alarme` retorna antes desse trecho. (`app/chamados/management/commands/disparar_perguntas_competencia.py:253`, `:270`, `:282`, `:289`, `:306`.)

**Efeitos da porta compartilhada e cobertura alternativa**

| Efeito chamado por `_rodar` | Escritor/porta real e cobertura |
|---|---|
| Desfazer carimbo sem lastro | `perguntar` busca perguntas materializadas do motivo/dia e chama `desfazer_carimbo_sem_lastro`. Esta relê sob trava, exige C6 vivo, chama `descarimbar`, registra evento e pode reabrir disputa. O cartório/C11 também o faz por `perguntar_pela_celula`. (`app/chamados/services/fabrica_da_celula.py:142`, `:177`; `app/chamados/services/relavra_via.py:219`, `:242`, `:246`, `:248`, `:259`.) |
| Limpar campos do carimbo | `descarimbar` zera `via_resolucao`, `materializada_em`, `materializada_por`; recusa linha fora do fato. O `save` relevante também aciona outbox. (`app/chamados/models.py:3203`, `:3208`, `:3210`; `app/chamados/signals.py:251`.) |
| Reabrir disputa | `reabrir_sistema` limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; grava ocorrência no chamado da disputa. `perguntar_pela_celula`, no passe específico de carimbadas, usa `reabrir_fechada=False`; o `perguntar` usado pelo fio mudo deixa o default `True`. (`app/chamados/models.py:2163`, `:2173`, `:2177`, `:2180`; `app/chamados/services/fabrica_da_celula.py:180`; `app/chamados/services/relavra_via.py:187`.) |
| Criar/agregar disputa e contexto | `perguntar` chama `criar_ou_atualizar_disputa`; esta chama `_abrir_ou_agregar_disputa`, que agrega `motivos`, faz merge do contexto com `qtd`/`ultima_em`, ou abre/reusa chamado-pai e cria disputa. É a mesma porta chamada pelo evento da célula. (`app/chamados/services/fabrica_da_celula.py:145`, `:190`; `app/chamados/services/disputa_emissao.py:535`, `:548`, `:565`, `:568`, `:587`, `:662`.) |
| Notas e trilha de emissão | No reuso do pai anexa ocorrência; no nascimento da disputa registra `disputa_aberta`; no nascimento da pergunta registra `pergunta_emitida`. Logo retirar o cron também retira os textos com origem `disparo_competencia`, embora a emissão por célula registre sua própria origem `julgamento_celula`. (`app/chamados/services/disputa_emissao.py:584`, `:595`; `app/chamados/models.py:3020`; `app/chamados/management/commands/disparar_perguntas_competencia.py:282`; `app/chamados/services/fabrica_da_celula.py:190`.) |
| Criar pergunta do fato | A emissão chama `PerguntaDisputa.abrir`; a porta insere com `gravar_dia_do_fato`, `save(force_insert=True)` e registra atos projetados recebidos. O cartório/C11 atravessa a mesma fábrica. (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/models.py:3013`, `:3017`, `:3019`; `app/chamados/services/fabrica_da_celula.py:145`.) |
| Costurar/reapontar pergunta existente | No reuso da identidade, `_nascer` pode atualizar `chamado_destino` e lavra, salvar e emitir evento da origem anterior. Na emissão nova, se ficar órfã e o motivo for do dia sem fechar, chama `reatar_pergunta_orfa` sob savepoint; falha fica órfã com log. Essa porta pode derivar destino, costurar histórico, lavrar pergunta morta ou emitir/reabrir `batida_ausente`, respeitando gates. (`app/chamados/models.py:2981`, `:2997`, `:3003`, `:3006`; `app/chamados/services/disputa_emissao.py:459`; `app/chamados/reconciliador.py:1533`, `:1556`, `:1619`, `:1642`, `:1655`, `:910`.) |
| Push | Apply com `--com-push` pode enviar push para disputa nova/motivo novo; dry e apply padrão silenciam. O evento da célula sempre silencia. Assim o modo opcional `--com-push` não tem equivalente nesse evento. (`app/chamados/management/commands/disparar_perguntas_competencia.py:289`, `:307`; `app/chamados/services/disputa_emissao.py:669`, `:686`, `:1748`; `app/chamados/services/fabrica_da_celula.py:188`.) |

**O ramo LASTRO parece residual, mas não deve ser chamado de morto absoluto.** Em estado coerente e estável, a pergunta materializada que poderia ser descarimbada já faz `linhas_dos_fatos(...).exists()` barrar o alvo; as linhas fora do fato que passam nesse filtro são inelegíveis ao descarimbo. Contudo seleção e escrita são consultas separadas; o filtro da identidade usa coluna `colaborador` e `instancia_marco`, enquanto a busca do descarimbo usa `disputa__colaborador` e omite instância. Concorrência ou inconsistência concreta: [nao verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/models.py:2753`; `app/chamados/services/fabrica_da_celula.py:142`; `app/chamados/services/relavra_via.py:181`; `app/chamados/models.py:2510`.)

**C11 está no caminho executável da cópia.** O consumidor chama `perguntar_por_eventos` depois de retratações e lastro e antes de marcar eventos processados; falha da fábrica entra em `falhas` e mantém eventos pendentes com tentativa/erro. A fábrica deduplica datas e relê células atuais do colaborador, portanto não confia no ID antigo do evento. O cron contínuo do consumidor está declarado a cada minuto. (`app/chamados/reconciliador.py:1384`, `:1388`, `:1392`; `app/chamados/services/fabrica_da_celula.py:259`; `app/config/crons.py:394`.)

**Há gatilhos para nascimento, retorno e perda de pergunta, com garantias diferentes.**

- Nascimento por `ChamadoColaborador.abrir`/`criar`: insere evento para `MODULOS_DE_TURNO` com célula resolvida, no ato transacional. (`app/chamados/models.py:971`, `:977`, `:1093`.)
- Retorno do chamado a `VIVOS`: emite antes da guarda `cartorio.em_julgamento()`, porém é **best-effort**; falha do INSERT é engolida sob savepoint e o chamado permanece reaberto. Portanto há caminho permitido de reabertura sem evento que o apply da varredura ainda poderia reparar. (`app/chamados/signals.py:188`, `:194`, `:201`, `:205`, `:210`.)
- Alteração relevante de pergunta: o receiver emite por `CAMPOS_QUE_ACORDAM`; a falha sobe e o `save` é atomic. Ressuscitação em lote e reuso da identidade acordam células explicitamente. (`app/chamados/signals.py:251`; `app/chamados/models.py:3413`, `:1563`, `:2997`.)
- Reapontamento acorda origem, e teste exige captura antes e evento depois do save. (`app/chamados/tests/test_e4_c20.py:117`.)
- Estoque antigo pode ser semeado por `consumir_eventos_celula --semear`, mas isso é opção explícita, não o ramo normal do cron. Semeia chamados vivos de turno e órfãs que o evento encerraria. (`app/chamados/management/commands/consumir_eventos_celula.py:28`; `app/chamados/reconciliador.py:1278`, `:1289`, `:1302`.)

**A cobertura não é o MESMO universo do fio mudo.** O evento passa por `dia_sem_trabalho`, veredito que acusa, data não futura e encaminhamento do limbo entregue à Pauta DP; o fio mudo usa motivos pela ata e seus filtros próprios, sem essas mesmas guardas explícitas. Além disso, seu universo aceita qualquer chamado vivo ligado à célula, enquanto os gatilhos de nascimento/retorno exigem módulo de turno. Retirar apply elimina a varredura reparadora desses casos; se há casos reais: [nao verificado]. (`app/chamados/services/fabrica_da_celula.py:85`, `:87`, `:93`, `:185`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `:158`; `app/chamados/signals.py:201`; `app/chamados/models.py:977`.)

**CADASTRO e limbo entregue já têm outro escritor; não são escritas atuais do disparar.** A fábrica, no passado sem turno, abre pauta de cadastro por colaborador sob trava, com dedup de pauta viva ou fechada cobrindo aquele dia; o limbo entregue abre pauta DP por motivo/dia, deduplicando inclusive histórico feito/cancelado. O comando atual só observa motivos deixados e emite perguntas do fio mudo. (`app/chamados/services/fabrica_da_celula.py:172`, `:288`, `:324`, `:185`, `:222`, `:239`; `app/chamados/management/commands/disparar_perguntas_competencia.py:112`, `:282`.)

**A TRANCA proposta não está no corpo atual.** `trancar_periodo` faz get-or-create/retranca e chama `disparar_no_ato` sob `engolir`; não contém o bloco de emissão C11 por célula do anexo. O serviço atual da tranca encerra chamados/disputas e encaminha respostas ao DP; declara que não escreve nas perguntas e roda com cartório calado. Portanto não substitui a emissão de pergunta faltante pelo fio mudo, nem garante hoje evento para recalcular a fila no ato da tranca. (`app/ponto/services/fechamento.py:1400`, `:1408`, `:1420`; `app/chamados/services/competencia_trancada.py:8`, `:30`, `:177`, `:196`, `:207`.)

**O reconciliador não cria pergunta ausente por conta própria fora da fábrica.** A rotina `reconciliar_chamados_colab` retrata perguntas, reata perguntas órfãs já existentes e retrata premissa morta; criar pergunta perdida agora aparece na chamada à fábrica dentro do consumidor. Reatar uma pergunta existente não cobre um fato que não tem linha de pergunta. (`app/chamados/reconciliador.py:1432`, `:1441`, `:1533`, `:1384`.)

## 09/11 21:34 · sessao bf6e4c8791c8 · - · - · origem=Ronald (tmux)

**Cron, conjuntos e leitores**

- Entrada efetiva da cópia: `ENC('disparar_perguntas_competencia', gatilho='apurar_furos_diarios', args='--comp-corrente --apply', log='fio_mudo.log', estagio='cobranca', teto_s=900, depende=('processar_cartorio', 'apurar_furos_diarios'))` — `app/config/crons.py:1521-1523`.
- Dependente direto como gatilho: `reconciliar_perguntas_orfas --alarme`, log `perguntas_orfas.log`, estágio `reconciliacao`, teto 900 — `app/config/crons.py:1526-1527`. Próximo elo é `lavrar_deslogados --apply`, gatilho `reconciliar_perguntas_orfas`, dependência declarada de `apurar_furos_diarios` — `app/config/crons.py:1539-1541`.
- `ENC` transforma dependência omitida em `(gatilho,)`; portanto reconciliar órfãs depende do disparar também no campo `depende` — `app/config/crons.py:53-54`.
- `ENCADEADOS` continua parte do universo de crons; `todas_as_entradas()` concatena `CRONS + ENCADEADOS` — `app/config/crons.py:1787-1798`.
- `JUIZES_POR_VARREDURA` contém somente disparar; comentário diz que permanece pelo FIO MUDO e que a célula já virou vigia — `app/config/crons.py:2032-2048`, fechamento da tupla em `2098`.
- Não há conjunto Python chamado `VIGIAS` na declaração observada: o grupo de vigias é a compreensão dentro de `PAPEL_DO_CRON`, começando com `reconciliar_grade`; disparar recebe `juiz` pela compreensão anterior — `app/config/crons.py:2164-2169`. `PAPEIS` aceita oito valores — `app/config/crons.py:2004`.
- Mudança correspondente: remover da tupla de juízes e inserir na compreensão de vigias; não há razão estrutural para sair de `ENCADEADOS` ou mudar `gatilho`/`depende` — evidência da independência entre agenda/papel em `app/config/crons.py:2340-2357`. Necessidade de mudar os gatilhos por negócio: **[nao verificado]**.
- Contagem de juízes: `juizes_por_varredura()` filtra `todas_as_entradas()` por `papel(e) == 'juiz'`, não simplesmente `len(JUIZES_POR_VARREDURA)` — `app/config/crons.py:2348-2357`. Hoje 1; após reclassificação coerente, 0 — mesma função e único nome `app/config/crons.py:2048`.
- `bin/placar_code.sh` importa a função e imprime `juizes_por_varredura=N (config/crons.py, papel juiz; esperado 0; dono=Code)` — `bin/placar_code.sh:482-486`. Esse valor entra em `TOTAL`; `?` equivale a 1 — `bin/placar_code.sh:555-556`.
- `escritas_por_cron()` usa, por default, a lista viva de juízes; mede exclusivamente `LogAuditoria` do ator `sistema:<cron>` nos últimos sete dias — `app/chamados/services/vigia_loop.py:61-99`. Tirar disparar da lista também o retira desse universo de medição; não prova ausência de escrita — `app/chamados/services/vigia_loop.py:64-66`, `80-84`.
- O placar publica uma linha por membro dessa medição, formato `escritas_por_cron[<nome>]=N (objetos ..., regravados ..., top3 ...; ... esperado 0 ...)` — `bin/placar_code.sh:201-209`. O verificador pede explicitamente conferir escrita sem trilha separadamente — `.claude/agents/verificador.md:17`.

**Matriz: não há 20/20 automático**

- Célula de chamado/juiz declara duas exceções: `config.crons.JUIZES_POR_VARREDURA` e `core.juizes.PENDENTES_CHAMADO`; não declara `verde=True`, logo usa default False — `app/core/contratos_estruturais.py:62`, `262-270`.
- `PENDENTES['chamado']` já está vazio; alias `PENDENTES_CHAMADO` aponta para ele — `app/core/juizes.py:465-466`, `1355`.
- A matriz tem 18 células com `verde=True`, GLOBAL está verde; portanto 19/20. A frase atual também declara 19/20 — contagem definida em `app/core/contratos_estruturais.py:1053-1064`, GLOBAL em `81-84`, frase em `app/core/placar_estrutural.py:440-441`.
- Esvaziar a lista, sozinho, mantém **19/20**, pois `verdes()` conta flags declaradas; 20/20 exige declarar essa célula verde e cumprir os selos — `app/core/contratos_estruturais.py:1053-1055`; `app/core/tests/test_selo_contratos_estruturais.py:71-96`.
- O denominador é calculado e continua 20, não muda pela remoção do juiz — `app/core/contratos_estruturais.py:1038-1046`.
- Leitores do N/20: TICKETS gera tabela com `ce.verdes()` e `ce.total()` — `app/core/placar_tickets.py:215-217`; ferramenta HAIKU retorna número, teto e células faltantes — `app/core/arquitetura_leitura.py:55-82`.
- ESTADO lê `core/placar_estrutural.py::RESULTADOS`, não stdout do disparar — `bin/gerar_estado.py:60-86`, publicação `388`. R6 contém cópia literal da frase 19/20 — `app/core/placar_estrutural.py:440-441`; selo exige igualdade dessa frase e declaradas com juiz real — `app/core/tests/test_selo_r6_frase_do_juiz.py:20-25`.
- Handoff também lê o dado R6 e raspa `"contratos_estruturais: (\d+)/(\d+) verdes"` — `bin/handoff_sessao.sh:185-208`.
- Afirmação de que hooks leem diretamente stdout/FIO MUDO: **[nao verificado]**. O arquivo `.claude/agents/verificador.md:17` exige auditoria do contador e escritas, mas não é parser da saída.

**Transporte de stdout, rc e carimbos**

- `corrente()` retorna `(cmd, gatilho, empresas, teto_s, args, caminho_do_log(log))` — `app/config/crons.py:1824-1851`.
- `manage.py crons corrente` serializa `log<TAB>cmd gatilho empresas teto args` — `app/core/management/commands/crons.py:37-42`.
- `bin/encadeado.sh` lê a serialização, passa `ENC_LOG`, acompanha rc e guarda pior rc — `bin/encadeado.sh:45-68`.
- Um elo só roda depois da existência dos arquivos `logs/fim/<gatilho>.<empresa>.<dia>`; teto ausente gera rc 2 — `bin/encadeado.sh:74-94`.
- Saída e erro do disparar são redirecionados para seu log declarado pelo envelope — `bin/encadeado.sh:99-104`.
- `cron_run.sh` captura rc, escreve `{nome,emp,ini,fim,dur,exit}` no JSONL e cria carimbo em rc **0 ou 2**; rc 2 significa alarme de negócio, não falha de execução — `bin/cron_run.sh:11-25`.
- Outros rc abrem pauta `cron_quebrado`; 0/2 recuperam pauta anterior — `bin/cron_run.sh:27-38`. Assim reconciliar órfãs continua recebendo gatilho mesmo com alarme 2 do disparar — `app/config/crons.py:1526`; `bin/cron_run.sh:22-25`; `bin/encadeado.sh:89`.
- Parser direto do contador da célula: `sed '^FABRICA-POR-EVENTO: o julgamento deixou \([0-9]*\) .*'` sobre `logs/fio_mudo.log`, última ocorrência; se arquivo não mudou hoje, usa `?` — `bin/placar_code.sh:388-389`.
- Publicação correspondente é `perguntas_que_o_julgamento_nao_fez=N`, esperado 0, fora do TOTAL — `bin/placar_code.sh:390`.
- Parser do número `[EMITE] FIO MUDO:` fora dos testes: **[nao verificado]**. [nao verificado]

**Selos e atualização de conjuntos**

- Tabuleiro exige igualdade entre juízes derivados e tupla declarada — `app/core/tests/test_contract_tabuleiro.py:25-28`. Exige célula de chamado não verde enquanto tupla não vazia — `41-44`.
- Selo do teto exige `total() == 20`, sete famílias, três contratos e exclusão de chamado/escala sem cadastro — `app/core/tests/test_selo_contratos_estruturais.py:33-49`.
- Selos da matriz exigem teste existente e TODAS as exceções vazias para cada verde; exceção cheia proíbe verde — `app/core/tests/test_selo_contratos_estruturais.py:71-96`.
- **Outro arquivo obrigatoriamente impactado**: a exceção de delegação do disparar no selo de host tem motivo/condição de saída explícitos; condição atual é “sai quando o fio mudo for medido e o C11 pousar” — `bin/tests/test_papel_prazo_nao_deriva.sh:302-308`. O selo falha se essa exceção continuar depois que o cron sair de `JUIZES_POR_VARREDURA` — `322-326`.

**Documentação e gerador**

- CRONS não chama literalmente disparar de escritor: mostra seu lugar na corrente — `app/docs/CRONS.md:39-40`; avisa que código é fonte única e documento é consulta — `7-16`. Ausência de outra descrição de escritor: **[nao verificado]**.
- JUIZES não contém ocorrência de disparar na cópia pesquisada: descrição dele como escritor ali é **[nao verificado]**. Documento identifica a fonte viva e função de lookup — `app/docs/JUIZES.md:7-16`.
- CLAUDE não contém ocorrência literal de disparar: descrição nominal dele como escritor é **[nao verificado]**. Regra geral é “emissão, pergunta e chamado nascem do sinal da célula”, cron de varredura é vigia, e juízes são dívida — `CLAUDE.md:166-170`. Fonte dos crons e corrente por arquivo — `236-242`.
- ARQUITETURA mostra dependências do disparar — `app/docs/ARQUITETURA.mmd:105-106`, `131`; declara **40 vigias, 1 juiz** e nó do disparar ligado a juiz — `209-219`.
- Gerador do tabuleiro lê todas as entradas e papel; após reclassificação produziria **41 vigias, 0 juízes**, sem nó `j_disparar...` — `app/core/management/commands/gerar_diagrama.py:233-265`.
- **Descompasso adicional**: `bloco_vigias()` lê apenas `CRONS`, não `ENCADEADOS`; portanto disparar, mesmo reclassificado, não ganharia nó no subgrafo VIGIAS por esse gerador — `app/core/management/commands/gerar_diagrama.py:393-401`. O bloco de tabuleiro conta universo completo, mas este subgrafo exclui encadeados — mesmas linhas e `242-243`.
- A cópia literal R6 envelheceu parcialmente em sua explicação: ainda menciona 24 juízes, apesar de a tupla atual conter 1 — `app/core/placar_estrutural.py:529-534`; `app/config/crons.py:2048`. Não implica erro na frase atual 19/20, cujo selo é separado — `app/core/tests/test_selo_r6_frase_do_juiz.py:20-25`.

**Limites**

- Não houve arquivo AGENTS.md encontrado no inventário da cópia; inexistência fora desse inventário: **[nao verificado]**.
- HEAD real/ancestralidade E4a-1 e operação em produção: **[nao verificado]**; cópia não contém repositório Git acessível no cwd.
- Nenhum dado entregue prova FIO MUDO 0 em produção nem C11 implantado; a condição de saída do selo continua exigindo ambos — `bin/tests/test_papel_prazo_nao_deriva.sh:307`; prova de atendimento: **[nao verificado]**.

## 09/11 21:34 · sessao 7e8ee26731c1 · - · - · origem=Ronald (tmux)

**Testes que fixam a situação atual** [nao verificado]

- A célula é vigia mesmo com `apply=True`: nenhuma pergunta gravada, contador 1, rc 2. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:98-103`.
- FIO MUDO continua escritor: `apply=True`, nasce 1 pergunta, contador da célula 0, rc 0. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:152-159`.
- Pergunta retratada não cala o FIO MUDO: nasce outra pergunta. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:161-175`.
- Limbo pode contar pendência da célula **e** escrever FIO MUDO antes de sair rc 2. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:177-184`.
- Parcial sem período fica na lista do admin, zero perguntas, rc 0. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:186-194`.
- Cron obrigatório com `--apply`: `app/chamados/tests/test_selo_emissor_fio_mudo.py:215-219`; argv encadeado também obrigatório com `--apply`: `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19-27`.
- Dry deixa contagem persistente de perguntas inalterada e imprime “dry-run: nada gravado”. `app/chamados/tests/test_selo_emissor_fio_mudo.py:226-232`. Porém o código executa o escritor em transação e provoca rollback; não é ausência de chamadas de escrita. `app/chamados/management/commands/disparar_perguntas_competencia.py:306-317`.
- O selo do ciclo exige que o chamado mudo tenha pergunta viva depois de `comp_corrente=True, apply=True`. `app/chamados/tests/test_selo_emissor_fio_mudo.py:273-298`.
- Hoje o número de juízes é fixado em **1** pelo teste. `app/ponto/tests/test_varredura_lote_prazo.py:47-62`.
- Exceção de delegação nomeia disparar e saída “fio mudo medido e C11 pousar”; removê-lo da lista sem apagar a exceção torna o script vermelho. `bin/tests/test_papel_prazo_nao_deriva.sh:302-307`, `bin/tests/test_papel_prazo_nao_deriva.sh:322-326`.

**Selos de vigia e estrutural**

- O parser de teste raspa `^FABRICA-POR-EVENTO: o julgamento deixou (\d+) `. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:91-95`.
- Limbo com pendência exige contador 1 e rc 2; folga exige 0 e rc 0; célula já perguntada exige 0 e rc 0. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:106-124`.
- Hoje fica fora da conta; amanhã entra e exige rc 2 sem escrever. `app/chamados/tests/test_pergunta_do_dia_em_curso.py:125-136`.
- FIO MUDO de hoje só pergunta marco vencido. `app/chamados/tests/test_pergunta_do_dia_em_curso.py:422-438`.
- Teto estrutural é `total()==20`; o selo **não** exige `verdes()==20`. `app/core/tests/test_selo_contratos_estruturais.py:33-49`, `app/core/tests/test_selo_contratos_estruturais.py:209-212`.
- Verde exige todas as exceções vazias; exceção cheia impede verde. `app/core/tests/test_selo_contratos_estruturais.py:81-96`.
- Lista e papéis derivados precisam coincidir; célula “chamado × um juiz por pergunta” não pode ficar verde com `JUIZES_POR_VARREDURA` cheio. `app/core/tests/test_contract_tabuleiro.py:25-28`, `app/core/tests/test_contract_tabuleiro.py:41-44`.

**Comparação com E4A2_PARQUE fornecido**

O texto do anexo não tem numeração de arquivo verificável nesta cópia [nao verificado]. O helper atual tem somente `FONTES_MUTADAS` e `fonte_real`, sem `com_hunk`. `app/chamados/tests/e4_catracas.py:1-9`.

| Item anexado | Situação comprovável |
|---|---|
| `HunkRegistroTest...H4` | Não é um teste atual; a direção proposta contraria os selos atuais que exigem `--apply` no cron e no encadeado. `app/chamados/tests/test_selo_emissor_fio_mudo.py:215-219`; `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19-27`. O teste atual da lista exige 1. `app/ponto/tests/test_varredura_lote_prazo.py:62`. |
| `test_vigia_alarma_todos_e_cadastro_conta_colab_nao_dias` | Não corresponde ao vigia atual: alarme trata apenas `deixou` e FIO MUDO; não inclui CADASTRO nem sua mensagem/contador. `app/chamados/management/commands/disparar_perguntas_competencia.py:237-264`. O contador CADASTRO existe em teste separado da fábrica, com nome/número/esperado/dono. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51-56`. |
| `test_varredura_nao_contem_porta_de_escrita_nem_dry_escritor` | A condição proposta não bate: `_rodar` chama `perguntar`; apply usa atomic e dry também executa `_rodar` em atomic. `app/chamados/management/commands/disparar_perguntas_competencia.py:270-317`. |
| `test_C11_tranca_proposta_2x_eventos_e_erro_antes_dispatch` | Proposta ainda não aplicada: `trancar_periodo` atual não emite eventos nem envolve a operação em atomic; dispatch segue em `engolir`, com intenção best-effort declarada. `app/ponto/services/fechamento.py:1400-1429`. |
| `test_hunk_A05_falha_registrar_reverte_save_e_memoria` | Proposta fora do comportamento atual: `carimbar` salva, mas não chama `DecisaoPergunta.registrar`. `app/chamados/models.py:3128-3157`. |
| `test_A05_efeito_e_hunk_assinatura_conservam_projecao_original` + helper `carimbar` | Comparação via `com_hunk` ficou obsoleta como instrumento; helper não existe hoje. `app/chamados/tests/e4_catracas.py:1-9`. Regra atual ainda preenche resposta/validação conforme argumento, sem ato DP neste corpo. `app/chamados/models.py:3130-3157`. |
| ORM `test_A05_efeito_puro_conserva_RV` | O arquivo atual declara explicitamente A05 fora da E4a-1. `app/chamados/tests/test_e4_expand_orm.py:1-5`. O caso de `resposta=None` condiz com o corpo atual, que só preenche RV quando `resposta` é verdadeiro; execução ORM desse caso [nao verificado]. `app/chamados/models.py:3128-3157`. |
| `HunksTest` com A05/TRANCA/C11/A22/H2 | Harness `com_hunk` obsoleto; os testes atuais leem fontes aplicados via `fonte_real`. `app/chamados/tests/test_e4_expand.py:15-16`. C11 reversão, ressuscitação e reuso têm testes contra código real. `app/chamados/tests/test_e4_expand.py:284-349`. H2 tem teste de atomicidade e falha do evento. `app/chamados/tests/test_e4_expand.py:351-376`. |

**Evidência da E4a-1 na cópia**

- Testes atuais identificam explicitamente E4a-1/P1 (origem do reuso em memória), P2 (relógio propagado) e P3 (pauta CADASTRO única e contador). `app/chamados/tests/test_e4_expand.py:335-349`; `app/chamados/tests/test_e4a_relogio_injetado.py:70-87`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:1-12`.
- O teste do lote diz expressamente que H4 pertence à E4a-2 e que o lote atual somente conta perguntas da célula. `app/chamados/tests/test_fabrica_pela_ata.py:63-68`.
- Confirmação de commit/HEAD git [nao verificado]. Nenhum teste foi executado [nao verificado]. [nao verificado]

## 09/11 21:24 · sessao 5ff88b8dbfe8 · MODO-COMUM-PASSADO-SEM-TURNO · construtor · origem=Code

Arquivos entregues:

- [Selo P3/P4/P6 e casos MORDE](chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:160).
- [Fábrica: alteração somente na docstring](chamados/services/fabrica_da_celula.py:292).
- [Executor dos mutantes, com restauração em `finally`](chamados/tests/mutantes_juiz_pauta_cadastro_sem_turno.py:112).
- [Comandos e saídas completos](chamados/tests/evidencia_modo_comum_passado_sem_turno.txt:7).

Resultado isolado: **14 testes OK**, RED contra a base com **22 falhas**, **23/23 mutantes RC=1** por asserções. [Evidência](chamados/tests/evidencia_modo_comum_passado_sem_turno.txt:765).

**Ruff 0.17.4: `All checks passed!`** [Saída](chamados/tests/evidencia_modo_comum_passado_sem_turno.txt:805).

Suíte Django/ORM: **[nao verificado]**, pois Django está indisponível neste ambiente. A execução isolada não substitui a certificação do Code e do opus final. [Limites](chamados/tests/evidencia_modo_comum_passado_sem_turno.txt:828).

## 09/11 21:15 · sessao d4c533f18057 · MODO-COMUM-PASSADO-SEM-TURNO · verificador · origem=Code

**Há PROCEDENTES: emissão sem previsão, entrada alternativa de pauta, proteção insuficiente da ordem, teste incompleto do registro e explicação incorreta do fechamento.** As evidências e curas mínimas estão abaixo. Correspondência independente da cópia com `49015265a`, ocorrências reais e resultado da suíte no container: **[nao verificado]**.

1. **QUEM ESCREVE / QUEM LÊ — CONFORME nos campos; PROCEDENTE na explicação de “entregue”.**

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| Nova chave/valor de `JUIZES['chamado']` | Declaração em `app/core/juizes.py:293`. | Gerador do MAPA: `bin/gerar_mapa_juizes.py:40`; teste novo: `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`. |
| `PAUTA_SEM_TURNO` / prefixo do texto | Constante em `app/chamados/services/fabrica_da_celula.py:276`; escritor específico incorpora ao texto em `app/chamados/services/fabrica_da_celula.py:342`. | `_pautas_cadastro` interpreta o prefixo; juiz e contador usam esse seletor. `app/chamados/services/fabrica_da_celula.py:285`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:315`. |
| `Pauta.texto`, âncora e destinatário | A fábrica fornece DP, `ANCORA_COLAB` e `str(cel.colaborador_id)`; a porta comum persiste. É **pessoa**, sem vínculo nem dia na chave. `app/chamados/services/fabrica_da_celula.py:339`, `app/chamados/services/fabrica_da_celula.py:340`; `app/pautas/models.py:30`; `app/pautas/services.py:180`. | O seletor exige cabeça, DP, âncora colab e prefixo; o juiz restringe pela pessoa. `app/chamados/services/fabrica_da_celula.py:284`, `app/chamados/services/fabrica_da_celula.py:297`. |
| `feito_em` | `marcar_feita` e `marcar_feita_pelo_sistema`, com instante `timezone.now()`. `app/pautas/services.py:246`, `app/pautas/services.py:247`, `app/pautas/services.py:271`, `app/pautas/services.py:273`. | `vivas()` exclui feitas; o juiz histórico converte o instante para data local. `app/pautas/models.py:57`; `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:304`. |
| `cancelado_em` | `cancelar`, ato do autor, grava instante. `app/pautas/services.py:288`, `app/pautas/services.py:294`. | `vivas()` exclui canceladas; o juiz aceita cancelamento como fechamento e o prioriza sobre `feito_em`. `app/pautas/models.py:64`; `app/chamados/services/fabrica_da_celula.py:303`. |
| `VFM\|CADASTRO_SEM_TURNO` e carimbo JSON | Vigia lê o contador, imprime número/esperado/dono e grava número e esperado no JSON. `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:223`, `app/chamados/management/commands/vigiar_fio_morto.py:229`. | `bin/fio_estado.py:104` lê o JSON e `bin/fio_estado.py:125` compara censo com esperado; `bin/gerar_estado.py:260` incorpora a apresentação. |

Os leitores genéricos transportam esses campos sem decidir “entregue CADASTRO”: aba de pautas, API de mensageria e listagem da pessoa. A listagem da pessoa inclui histórico; a aba padrão usa vivas. São universos declaradamente diferentes, sem segundo significado para a âncora. `app/pautas/services.py:323`, `app/pautas/services.py:433`; `app/pautas/views.py:40`; `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`.

**PROCEDENTE — “o DP já olhou” não decorre do predicado.** O juiz aceita cancelamento pelo autor e não consulta `lido_em`; a docstring afirma que o DP olhou. Cura mínima: descrever “já encaminhada e posteriormente fechada/cancelada”, sem prometer leitura ou resolução pelo DP. Alterar a cobertura do cancelamento exigiria decisão normativa específica. `app/chamados/services/fabrica_da_celula.py:292`, `app/chamados/services/fabrica_da_celula.py:301`; `app/pautas/services.py:288`, `app/pautas/services.py:294`.

**CONFORME — instante versus data:** o armazenamento é de instantes; a cobertura usa `localdate(fechou) >= dia`, sendo `dia=cel.data`. A configuração base é São Paulo; fuso ativo efetivo em produção: **[nao verificado]**. `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`; `app/config/settings/base.py:93`, `app/config/settings/base.py:95`.

2. **O selo morde — CONFORME para usos diretos; PROCEDENTE na ordem e no registro.**

| Regra | O que pega | O que escapa |
|---|---|---|
| **a. Marcador/literal fora dos dois sítios** | Nome, import com alias, atributo direto e literal completo. Importar o módulo inteiro e acessar diretamente o atributo **é pego**; outra constante com o mesmo literal completo também. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:111`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:115`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:120`. | `getattr`, texto composto por soma, regex sem o literal completo e SQL parametrizado com texto composto não são resolvidos pelo varredor. Uso atual dessas técnicas para esse marcador: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:120`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:124`. |
| **b. Seletor fora do juiz/contador** | Chamadas diretas e aliases de `from … import`. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:61`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:146`. | Referência intermediária, como `f = modulo._pautas_cadastro; f()`, e reflexão. Uso atual com essa finalidade: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:47`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:124`. |
| **c. Escritor fora de `perguntar_pela_celula`** | Chamadas diretas ao escritor específico. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:148`. | Mesmas indireções de função; além disso, o **POST humano já existente** pode produzir o mesmo universo sem chamar o escritor específico. `app/pautas/views.py:76`, `app/pautas/views.py:80`, `app/pautas/views.py:81`; `app/pautas/services.py:180`. |
| **d. Trava → juiz → escrever em `atomic`** | Inversões diretas, ausência ou repetição das chamadas e os casos negativos incluídos. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:174`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:257`. | Não valida argumentos, identidade do `atomic` nem interrupções do fluxo entre os passos. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`. |
| **e. Registro** | Presença do endereço entre os valores da família. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`. | Chave errada e segundo registro do mesmo endereço passam. Cura mínima: exigir chave exata e unicidade. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:195`; `app/core/juizes.py:293`. |

**PROCEDENTE — ordem com falsa garantia.** Na execução em memória, os **11 testes novos passaram**, individualmente, com: juiz sem `cel.data`; trava de outro colaborador; `falso.atomic()`; retorno antes do bloco; retorno após a trava; comparação de fechamento invertida; contador fixado em zero. Os pontos permissivos são `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:170`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:216`.

Cura mínima: conferir argumentos da trava/juiz, identidade de `transaction.atomic` e sequência executável. Fechamento e contador precisam da proteção comportamental já existente; esses mutantes **não demonstram** que a suíte inteira passaria. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`.

**PROCEDENTE — brecha alcançável hoje pela entrada humana.** Staff autorizado pode enviar duas vezes marcador + DP + mesma âncora colab: a porta comum cria duas cabeças sem a trava/deduplicação específica, enquanto o contador `distinct` continua mostrando uma pessoa. Isso é segunda origem da pauta específica, usando a porta comum; ocorrência real: **[nao verificado]**. Cura mínima: reservar o prefixo ao fluxo específico na entrada comum, declarar essa validação no contrato do selo e testar a recusa pela entrada humana. `app/pautas/views.py:16`, `app/pautas/views.py:76`; `app/pautas/services.py:153`, `app/pautas/services.py:180`; `app/chamados/services/fabrica_da_celula.py:320`.

3. **MESMA PERGUNTA / SEGUNDO JUIZ — CONFORME no chamador atual.**

A chave registrada corresponde ao predicado efetivamente usado: cabeça DP de cadastro da pessoa, viva ou fechada em data local igual/posterior ao dia consultado. O chamador passa exatamente `(cel.colaborador_id, cel.data)`. A assinatura permite `dia=None`, caso em que responde apenas sobre vivas; essa forma não é a chamada da fábrica. `app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:288`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:299`, `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`.

O juiz não verifica sozinho “passado sem turno”; essa pré-condição vem do chamador. Logo, o contrato depende desse caminho, atualmente presente. `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`.

**Nenhum segundo juiz registrado da mesma entrega foi identificado.** `pergunta_viva`, `dia_sem_trabalho`, `tarefa_do_dia` e `dia_da_pergunta_trancado` têm outras perguntas; o contador mede somente pessoas com pendência viva. `app/core/juizes.py:278`, `app/core/juizes.py:290`, `app/core/juizes.py:291`, `app/core/juizes.py:292`; `app/chamados/services/fabrica_da_celula.py:320`.

4. **OBRIGAÇÕES POSITIVAS — cumprimento e proteção.**

| Obrigação | Código da cópia | Selo novo / cura mínima |
|---|---|---|
| **i. Passado sem turno não pergunta ao colab** | **CONFORME na fábrica**: encaminha cadastro e retorna. **CONFORME no fio mudo quanto ao silêncio**: helper devolve vazio e o ramo pula. **PROCEDENTE em `disputa_emissao`**, explicado abaixo. `app/chamados/services/fabrica_da_celula.py:175`, `app/chamados/services/fabrica_da_celula.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:160`. | O novo selo não testa essa obrigação nos emissores. Falta caso negativo completo para `disputa_emissao` e caso específico do fio mudo; falta também detectar cadastro não entregue quando esses caminhos apenas calam. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`; `app/chamados/services/fabrica_da_celula.py:213`. |
| **ii. Uma pendência por colab** | **CONFORME para escritores cooperantes**: `atomic`, trava da pessoa, seleção e escrita; a trava usa `SELECT FOR UPDATE`. Duas conexões reais: **[nao verificado]**. A entrada humana rompe a garantia global descrita no item 2. `app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:337`; `app/chamados/models.py:2666`. | Testes anteriores cobrem dois dias e repetição sequencial; não equivalem a dois processos concorrentes. Falta ensaio com conexões distintas e proteção dos argumentos no AST. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51`; `app/chamados/tests/test_e4_c20.py:95`, `app/chamados/tests/test_e4_c20.py:105`. |
| **iii. Nome/número/esperado/dono** | **CONFORME**: nome `cadastro_sem_turno`, número de pessoas distintas com pauta viva, esperado `0`, dono `CADASTRO`. Nome aparece também no rótulo VFM/chave JSON; número/esperado/dono aparecem na linha textual. O JSON não transporta dono. `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:321`; `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:229`. | O novo selo exige chamada ao seletor, sem conferir os quatro valores nem o consumo pelo vigia. O contrato dos quatro campos já tem teste anterior. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:217`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`. |
| **iv. Fechada não reabre em laço** | **CONFORME** para dia anterior ou igual à data local do fechamento. Dia posterior permite nova pauta. `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`. | Proteção comportamental anterior cobre feita, cancelada, dia coberto e posterior; o AST novo não protege essa semântica. Falta caso na fronteira UTC/data local. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:67`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:90`. |
| **v. Vigia só alarma, nunca julga** | **CONFORME no acompanhamento CADASTRO**: lê, imprime e carimba; esse contador deliberadamente não dispara alarme operacional. O modo `--alarme` de disparar retorna antes da escrita. `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`; `app/chamados/management/commands/disparar_perguntas_competencia.py:253`. | O novo selo não proíbe julgamento/escrita no vigia; exigir que seu arquivo seja varrido não protege esse papel. O fio mudo escritor está declarado separadamente como exceção de prazo. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:206`; `app/config/crons.py:2044`. |

**PROCEDENTE — caminho de emissão sem previsão.** `_tem_base` aceita batidas; `classificar_falta` pode manter `esperada=[]` e devolver motivo por batida unilateral; `_ata_do_dia` permite continuar sem célula/escala; o agrupador chama a abertura da pergunta. Portanto existe caminho de código que contraria a proibição fornecida, sem demonstrar ocorrência em produção. `app/chamados/services/disputa_emissao.py:814`; `app/escala/servico_jornada.py:396`, `app/escala/servico_jornada.py:444`, `app/escala/servico_jornada.py:446`; `app/chamados/services/disputa_emissao.py:906`, `app/chamados/services/disputa_emissao.py:1234`, `app/chamados/services/disputa_emissao.py:1241`.

Cura mínima: fazer o classificador canônico respeitar a ausência de previsão e encaminhar a pendência pelo fluxo de evento/porta autorizado, com teste do emissor completo. A fila existente chama esse caso de “falta lei”, mas a exceção “sem turno **com batida**” não está no aval fornecido: autorização dessa exceção **[nao verificado]**. `app/docs/BACKLOG.md:1876`; `app/docs/PROMPTS.md:2004`.

Quanto ao **dia posterior ao fechamento**: a implementação e os testes adotam uma nova pendência viva para uma nova ocorrência. Isso é compatível com “uma pendência viva por colab”; autorização literal específica dessa política no aval raspado: **[nao verificado]**. `app/chamados/services/fabrica_da_celula.py:295`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`.

5. **CLASSES QUE JÁ REPROVARAM OBRA.**

| Classe | Resultado |
|---|---|
| Segundo juiz que duplica registrado | **NÃO identificado para entrega CADASTRO.** `app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:288`. |
| Segundo escritor fora da porta | **NÃO demonstrado como escritor ORM externo; SIM, segunda origem que contorna a deduplicação específica**, pelo POST. `app/pautas/views.py:76`; `app/pautas/services.py:180`. |
| Relógio fora da borda | **NÃO no novo juiz**: converte instante persistido. **SIM, preexistente no classificador do emissor**, que lê agora internamente. `app/chamados/services/fabrica_da_celula.py:304`; `app/escala/servico_jornada.py:368`. |
| Derivação paralela de fonte única | **SIM para a política de emissão**: a fábrica cala sem previsão; o classificador ainda acusa unilateral sem sequência prevista. Não é segundo juiz de entrega. `app/chamados/services/fabrica_da_celula.py:38`; `app/escala/servico_jornada.py:396`, `app/escala/servico_jornada.py:444`. |
| Selo/RED sem mordida | **SIM, parcialmente**: ordem e registro aceitam os mutantes descritos; usos diretos têm mordida. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:224`. |
| Ordem de trava | **NÃO no escritor atual; SIM na proteção incompleta do selo.** `app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:337`; `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`. |
| Pouso antes do pré-requisito | **[nao verificado]**: cópia e relato não certificam execução/deploy; a exigência está em `CLAUDE.md:374`. | [nao verificado]
| Campo com dois significados | **NÃO identificado nos tipos pessoa/data/instante; SIM na explicação de “entregue = DP olhou”.** `app/chamados/services/fabrica_da_celula.py:292`; `app/pautas/models.py:94`. |

6. **VARREDURA DO FILESYSTEM — pode ler não versionado ou falhar; container efetivo [nao verificado].**

O selo percorre o filesystem, sem inventário Git, incluindo `static`, `staticfiles`, `media` e `logs`. Na montagem da cópia, `staticfiles` vem do host e é ignorado pelo Git; logs/media usam tmpfs. Portanto um `.py` não versionado nesses locais pode entrar na certificação. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:26`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`; `.gitignore:12`, `.gitignore:13`; `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:81`.

Leitura sem UTF-8, arquivo ilegível ou Python inválido pode interromper o teste; diretório inacessível pode ser omitido pelo `os.walk` sem `onerror`. Não há tratamento nesses pontos. Falha efetiva no container: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:132`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:204`.

A medição local em memória varreu **760 arquivos em aproximadamente 1,63 s**; os 11 testes levaram aproximadamente **1,67 s**. Esse custo local é pequeno; tempo no container e crescimento dos diretórios: **[nao verificado]**. O percurso também enumera arquivos que não serão analisados como Python. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`.

**PROCEDENTES finais:**

- Emissão sem previsão ainda alcançável em `disputa_emissao`; corrigir o predicado canônico e proteger o caminho completo. `app/escala/servico_jornada.py:444`; `app/chamados/services/disputa_emissao.py:1241`.
- Entrada humana produz o marcador sem deduplicação específica; reservar essa espécie ao fluxo autorizado. `app/pautas/views.py:76`; `app/pautas/services.py:180`.
- Selo de ordem aceita trava/juiz/atomic incorretos e fluxo interrompido; validar identidade, argumentos e sequência executável. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`.
- Teste de registro aceita pergunta errada e duplicação; exigir chave exata e unicidade. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`.
- Explicação “DP já olhou” excede o predicado; corrigir a documentação do fechamento/cancelamento. `app/chamados/services/fabrica_da_celula.py:292`; `app/pautas/services.py:288`.

## 09/11 21:13 · sessao d6f821d9f1e7 · - · - · origem=Ronald (tmux)

**PROCEDENTE — ordem selada apenas pela forma.** Em memória, os 11 testes novos ficaram verdes para cada mutante: juiz sem `cel.data`, trava de outro colaborador, `falso.atomic()`, retorno antes do bloco, retorno após a trava, fechamento `>=` trocado por `<=` e contador fixado em zero. `_ordem_ok` não confere argumentos ou identidade das funções e ignora instruções intermediárias que interrompem o fluxo. Cura mínima: conferir argumentos, vínculo ao `transaction.atomic` e sequência executável; proteger fechamento/contador com testes semânticos. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:170`, `:179`, `:182`, `:216`; `app/chamados/services/fabrica_da_celula.py:304`, `:320`, `:335`, `:337`)

**PROCEDENTE — registro incompleto.** Chave errada mantendo o valor e segundo registro com o mesmo valor passam no teste de registro. Cura mínima: exigir a chave exata e unicidade do endereço nessa família. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:195`; `app/core/juizes.py:293`)

**CONFORME, com limites — o selo morde usos diretos.** Nome/import alias/atributo direto/literal completo são detectados; importar módulo e chamar seu atributo também é detectado, assim como outra constante contendo o mesmo literal. Entretanto, `getattr`, referência intermediária `f = modulo._pautas_cadastro; f()`, equivalente do escritor, `Q` com prefixo composto por `+`, regex sem literal completo e SQL com parâmetro composto escapam. Esses exemplos são mutantes hipotéticos, sem ocorrência encontrada no código atual. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:47`, `:61`, `:111`, `:120`, `:124`, `:138`)

**Caminho atual alcançável — entrada humana pode produzir o marcador.** POST de staff aceita texto, destinatário e âncora arbitrários e chama a porta genérica sem deduplicação específica/trava por colaborador. Entrada adequada pode criar outra pauta CADASTRO sem passar por `pauta_cadastro_sem_turno`; ocorrência real é `[nao verificado]`. Isso continua usando a porta genérica de Pauta, portanto não comprova escritor ORM fora dela. (`app/config/urls.py:36`; `app/pautas/urls.py:9`; `app/pautas/views.py:16`, `:72`, `:76`; `app/pautas/services.py:153`, `:180`)

**Varredura — pode incluir arquivo não versionado.** Percorre filesystem, sem consultar Git; `staticfiles` do host é montado no container como diretório real, embora ignorado no Git. Logs/media ficam em tmpfs. `.py` inválido, ilegível ou sem UTF-8 pode falhar em leitura/parse; diretório inacessível pode ser omitido pelo `os.walk` sem `onerror`. Falha efetiva nesse container: `[nao verificado]`. Cura mínima: universo de fontes versionadas/materializado para o selo, distinguindo arquivos gerados. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:26`, `:36`, `:132`, `:204`; `.gitignore:13`; `bin/arvore_do_push.sh:55`, `:81`)

**Custo local:** varredura de 760 arquivos sem erros/violações em aproximadamente 1,63 s; os 11 testes em aproximadamente 1,67 s, usando `python3 -B`, AST e `unittest.TestCase` em memória. Tempo/resultado no container Django: `[nao verificado]`. O método que determina esse custo está em `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`.

**Limite importante:** os mutantes passaram apenas pelo selo novo. Testes funcionais anteriores já cobrem contador, fechamento e dia posterior; não se pode concluir que a suíte inteira os aceitaria. (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`, `:61`, `:76`, `:84`, `:88`)

## 09/11 21:12 · sessao 57bbec0b6ee6 · - · - · origem=Ronald (tmux)

- **Juiz e registro — CONFORME no caminho efetivo.** A chave nova de `JUIZES['chamado']` aponta para `pauta_cadastro_existente(colaborador_id, dia=None)`; o único chamador de produção específico passa `(cel.colaborador_id, cel.data)`. O universo é cabeça, departamento DP, âncora de pessoa e prefixo do marcador. Não há outro juiz de entrega CADASTRO registrado; `dia_sem_trabalho`, `tarefa_do_dia`, `pergunta_viva` e `dia_da_pergunta_trancado` respondem outras perguntas. (`app/core/juizes.py:278`, `app/core/juizes.py:290`, `app/core/juizes.py:291`, `app/core/juizes.py:292`, `app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:279`, `app/chamados/services/fabrica_da_celula.py:288`, `app/chamados/services/fabrica_da_celula.py:337`)

- **Escrita/leitura dos campos.** O marcador nasce como constante em `:276`, é consumido pelo filtro em `:285` e incorporado ao texto em `:342`. O escritor específico assina TI, destina a DP, usa `ANCORA_COLAB` e `str(cel.colaborador_id)`, sem vínculo ou data na chave. A porta genérica grava esses campos em `Pauta.objects.create`; o seletor e juiz leem os mesmos valores; contador deduplica por `ancora_id`. (`app/chamados/services/fabrica_da_celula.py:276`, `app/chamados/services/fabrica_da_celula.py:284`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:339`; `app/pautas/models.py:30`; `app/pautas/services.py:180`)

- **Viva/fechada e datas — CONFORME com ressalva semântica.** Viva vem da autoridade `PautaQuerySet.vivas`: não feita e não cancelada. Fechada cobre `timezone.localdate(cancelado_em or feito_em) >= dia`; sem `dia`, só viva cobre. `feito_em` é escrito por `marcar_feita` e `marcar_feita_pelo_sistema`; `cancelado_em` por `cancelar`; os três usam instantes `timezone.now()`. O juiz converte esses instantes a data local, não compara uma `date` com um instante. Fuso base São Paulo e `USE_TZ=True`; fuso ativo real do processo: `[nao verificado]`. (`app/pautas/models.py:57`, `app/pautas/models.py:64`; `app/pautas/services.py:246`, `app/pautas/services.py:247`, `app/pautas/services.py:271`, `app/pautas/services.py:272`, `app/pautas/services.py:294`; `app/chamados/services/fabrica_da_celula.py:299`, `app/chamados/services/fabrica_da_celula.py:303`, `app/chamados/services/fabrica_da_celula.py:304`; `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

- **Ressalva “DP já olhou”: não demonstrada.** A docstring diz que fechamento significa “o DP já olhou”, porém cancelamento é ato do autor e dispensa leitura; `pauta_cadastro_existente` não lê `lido_em`. Isso não muda a definição operacional declarada de “entregue”, mas não autoriza afirmar que DP leu/resolveu. A cura mínima é corrigir a explicação, se “entregue” significa encaminhada; mudar o predicado exige decidir se cancelamento deveria continuar cobrindo. (`app/chamados/services/fabrica_da_celula.py:292`, `app/chamados/services/fabrica_da_celula.py:301`, `app/pautas/models.py:94`, `app/pautas/services.py:288`, `app/pautas/services.py:294`)

- **Uma pendência pelo escritor específico — CONFORME estático; concorrência real `[nao verificado]`.** O escritor abre `atomic`, trava a pessoa, consulta o juiz e escreve. `trava_na_ordem` executa `SELECT FOR UPDATE` da linha de `Colaborador`; portanto dois escritores cooperantes do mesmo colab serializam antes da seleção. Não há chave por dia/vínculo. (`app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:335`, `app/chamados/services/fabrica_da_celula.py:337`, `app/chamados/services/fabrica_da_celula.py:339`; `app/chamados/models.py:2666`)

- **Brecha alcançável hoje: segunda origem pelo POST genérico — PROCEDENTE se o contrato “UMA por colab” incluir todas as entradas do produto.** Staff autorizado pode postar duas vezes o mesmo texto com marcador, `ancora_tipo=colab`, mesmo id e DP: rota existe, view recebe tudo do POST, service grava sem reservar prefixo, travar pessoa ou deduplicar. Isso cria duas cabeças vivas; contador por `distinct` continua mostrando 1. Não é segundo escritor ORM fora da porta, mas entrada alternativa que contorna o escritor específico. O selo AST não vê valores vindos do POST. Ocorrência em produção `[nao verificado]`. Cura mínima: reservar o prefixo à entrada específica na porta comum e acrescentar teste negativo da entrada humana; não basta procurar mais tokens em fonte. (`app/config/urls.py:36`; `app/pautas/urls.py:9`; `app/pautas/views.py:16`, `app/pautas/views.py:73`, `app/pautas/views.py:76`, `app/pautas/views.py:80`, `app/pautas/views.py:81`; `app/pautas/services.py:153`, `app/pautas/services.py:180`; `app/chamados/services/fabrica_da_celula.py:320`; `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:120`)

- **Contador: os quatro campos existem.** `nome='cadastro_sem_turno'`, número de `ancora_id` distintos vivos, `esperado=0`, `dono='CADASTRO'`. O vigia chama o contador e imprime nome no rótulo `VFM|CADASTRO_SEM_TURNO`, número, esperado e dono. JSON contém chave/número em `censo` e esperado em `esperado`, mas não transporta dono; o dono continua na função e linha textual. `bin/fio_estado.py` lê o JSON e compara genericamente esperado versus censo; `gerar_estado.py` usa essa apresentação. (`app/chamados/services/fabrica_da_celula.py:309`, `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:321`; `app/chamados/management/commands/vigiar_fio_morto.py:98`, `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:223`, `app/chamados/management/commands/vigiar_fio_morto.py:229`; `bin/fio_estado.py:104`, `bin/fio_estado.py:125`, `bin/fio_estado.py:191`; `bin/gerar_estado.py:260`)

- **Vigia do contador — CONFORME como leitor.** `vigiar_fio_morto` só chama contador/imprime/carimba; CADASTRO fica fora de `ALARMA_SEMPRE` e `ALARMA_EM_0`, portanto nem dispara `SystemExit` por essa pendência. O leitor do ESTADO mostra divergência “fora do esperado”. O aval fornecido exige contador, não exige interromper a corrente por CADASTRO. (`app/chamados/management/commands/vigiar_fio_morto.py:83`, `app/chamados/management/commands/vigiar_fio_morto.py:84`, `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:239`; `bin/fio_estado.py:191`)

- **Fechamento/dia novo — implementação e testes existentes.** Viva cobre qualquer dia; fechada cobre até a data local inclusiva. Dia posterior cria nova pauta, mantendo só uma viva pelo escritor cooperante. Isso é coerente com nova ocorrência de cadastro faltante; autorização literal específica dessa reemissão no aval raspado `[nao verificado]`; a decisão está expressa na docstring e no selo comportamental anterior. (`app/chamados/services/fabrica_da_celula.py:292`, `app/chamados/services/fabrica_da_celula.py:295`, `app/chamados/services/fabrica_da_celula.py:304`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:67`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:90`)

- **Novo selo não protege todas as obrigações positivas.** Ele exige sítios, contagens de chamadas e forma sequencial; não fixa conteúdo dos filtros, número/dono/esperado, comparação de fechamento/data local, argumentos corretos da trava/juiz ou emissão em todos os emissores. Existem testes comportamentais anteriores para dois dias/uma pauta, quatro campos, feita/cancelada, dia posterior e dia antigo; o teste isolado de ordem/duas rodadas usa dublês, não duas conexões concorrentes. Cura mínima: preservar esses testes e acrescentar concorrência real e fronteira local de meia-noite, além de identidade/argumentos do AST. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:174`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:216`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`; `app/chamados/tests/test_e4_c20.py:65`, `app/chamados/tests/test_e4_c20.py:96`, `app/chamados/tests/test_e4_c20.py:105`)

- **Leitores genéricos não são outro juiz.** A aba seleciona por destinatário e viva, serializa texto/âncora/estado; o copiloto exibe `p.texto`; API mensageria lista recebidas DP e entrega texto/âncora; `pautas_do_colab` lista pautas de pessoa/dia sem interpretar o marcador. (`app/pautas/services.py:308`, `app/pautas/services.py:322`, `app/pautas/services.py:433`; `app/pautas/views.py:40`, `app/pautas/views.py:43`; `app/templates/core/_copiloto.html:405`; `app/api/views_mensageria.py:1324`, `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`)

Não executei banco, log ou testes que escrevem arquivos. Resultado de execução da suíte e correspondência independente com o commit de produção `[nao verificado]`. [nao verificado]

## 09/11 21:11 · sessao a1ada130ce61 · - · - · origem=Ronald (tmux)

**PROCEDENTE — obrigação (i), fora da fábrica.** `disputa_emissao` pode perguntar por dia passado sem previsto: `_tem_base` aceita batidas (`app/chamados/services/disputa_emissao.py:814`); `classificar_falta` deixa a sequência prevista vazia sem escala (`app/escala/servico_jornada.py:396`) e mesmo assim acusa batida unilateral (`app/escala/servico_jornada.py:444`, `app/escala/servico_jornada.py:446`). `_ata_do_dia` autoriza continuar sem célula e sem escala (`app/chamados/services/disputa_emissao.py:899`, `app/chamados/services/disputa_emissao.py:906`); o agrupador chega à abertura (`app/chamados/services/disputa_emissao.py:1232`, `app/chamados/services/disputa_emissao.py:1241`). A porta verifica identidade/costura, mas chega à inserção sem guarda de previsão (`app/chamados/models.py:2789`, `app/chamados/models.py:2896`, `app/chamados/models.py:3017`). Ocorrência real em produção: **[nao verificado]**.

Correção mínima: fazer o juiz canônico abster-se quando não há sequência prevista, proteger o caminho completo de emissão com caso negativo e encaminhar CADASTRO pela porta/evento autorizado. O novo selo só verifica nomes/sítios e forma do escritor, portanto não detecta essa emissão (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:138`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`).

**CONFORME — fio mudo não pergunta nesse caso.** Ele passa `vencidos_no_instante` a `motivos_pela_ata` e pula quando não há motivo (`app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:160`); o helper devolve vazio para ausência de previsto (`app/chamados/services/fabrica_da_celula.py:38`). Esse ramo não encaminha ao CADASTRO; a entrega depende do evento da fábrica (`app/chamados/services/fabrica_da_celula.py:175`). O teste existente de lote sem turno monta apenas célula, sem chamado mudo, portanto não cobre especificamente esse ramo (`app/chamados/tests/test_fabrica_pela_ata.py:30`, `app/chamados/tests/test_fabrica_pela_ata.py:63`).

**CONFORME — vigia versus emissor declarado.** O ramo célula conta (`app/chamados/management/commands/disparar_perguntas_competencia.py:112`); `--alarme` retorna antes de escrever (`app/chamados/management/commands/disparar_perguntas_competencia.py:253`). O fio mudo escreve com `--apply`, mas está explicitamente declarado como exceção de prazo, não vigia (`app/config/crons.py:2044`, `app/chamados/management/commands/disparar_perguntas_competencia.py:282`). Não classificar essa escrita, isoladamente, como violação.

**CONFORME — contador CADASTRO não alarma deliberadamente.** O vigia lê o contador (`app/chamados/management/commands/vigiar_fio_morto.py:179`), publica número/esperado/dono e “não alarma” (`app/chamados/management/commands/vigiar_fio_morto.py:202`); CADASTRO está fora dos conjuntos de alarme (`app/chamados/management/commands/vigiar_fio_morto.py:83`, `app/chamados/management/commands/vigiar_fio_morto.py:84`). O aval fornecido exige contador, sem exigir alarme específico por cadastro **[nao verificado: anexo transcrito sem arquivo local]**. O selo novo não protege essa ligação ao vigia (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:206` apenas exige que o arquivo seja varrido).

**Relógio:** no juiz novo, `localdate(fechou)` converte timestamp persistido, sem ler agora (`app/chamados/services/fabrica_da_celula.py:304`). No universo ampliado, há leitura de relógio dentro de `classificar_falta` (`app/escala/servico_jornada.py:368`) e fallback em `cobra_marcos_da_ata` (`app/chamados/services/disputa_emissao.py:744`); nenhuma dessas leituras é protegida pelo selo novo (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:124`).

**Selos existentes relevantes, sem executar banco:** deduplicação sequencial e quatro campos do contador (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51`); fechada não cala dia posterior (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`); fechada cobre dias anteriores e iguais (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`); trava antes de consulta com stubs, duas chamadas sequenciais (`app/chamados/tests/test_e4_c20.py:64`). Esses casos não demonstram dois processos concorrentes (`app/chamados/tests/test_e4_c20.py:105`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:52`).

## 09/11 21:08 · sessao 0784c8db88ae · MODO-COMUM-PASSADO-SEM-TURNO · construtor · origem=Code

Arquivos entregues:

- [test_juiz_pauta_cadastro_sem_turno.py](/home/ronald/wt-codex-modo-comum-passado-sem-turno-210159/app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:194): selo AST, registro, anti-vacuidade e casos MORDE para a–d.
- [JUIZES.md](/home/ronald/wt-codex-modo-comum-passado-sem-turno-210159/app/docs/JUIZES.md:86): linha com juiz, escritor, contador e selo.

Do rascunho, corrigi:

- Varredura inclui Python em `static/media/logs`; exclui testes, migrations, dependências e links simbólicos (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:26`).
- Só a definição da constante recebe exceção no módulo; imports, aliases, atributos e literais são fiscalizados (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:96`).
- Escopos aninhados e métodos não herdam autorização; docstrings são ignoradas (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:71`).
- Ordem exige trava, guarda do juiz e escrita no mesmo bloco `atomic`, rejeitando duplicações e chamadas em blocos sem execução (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:155`).

Comando executado para base e dois mutantes reais, restaurados em `finally`. Como Django não estava disponível, o import de `SimpleTestCase` foi substituído em memória por `unittest.TestCase`; execução com Django: [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import ast
import io
import pathlib
import unittest

path = pathlib.Path('chamados/tests/test_juiz_pauta_cadastro_sem_turno.py')
tree = ast.parse(path.read_text(), str(path))
for node in tree.body:
    if isinstance(node, ast.ImportFrom) and node.module == 'django.test':
        node.module = 'unittest'
        node.names = [ast.alias(name='TestCase', asname='SimpleTestCase')]
namespace = {'__file__': str(path.resolve()), '__name__': 'selo_direto'}
exec(compile(ast.fix_missing_locations(tree), str(path), 'exec'), namespace)
classe = namespace['JuizPautaCadastroSemTurnoTest']
fabrica = pathlib.Path(namespace['FABRICA'])
original = fabrica.read_bytes()
fonte = original.decode()
fn = next(n for n in ast.parse(fonte).body
          if isinstance(n, ast.FunctionDef)
          and n.name == namespace['ESCRITOR'])
bloco = next(n for n in fn.body if isinstance(n, ast.With))
guarda, escrita = bloco.body[1:3]
assert isinstance(guarda, ast.If) and isinstance(escrita, ast.Expr)
linhas = fonte.splitlines(keepends=True)
mutante = ''.join(
    linhas[:guarda.lineno - 1]
    + linhas[escrita.lineno - 1:escrita.end_lineno]
    + linhas[guarda.lineno - 1:guarda.end_lineno]
    + linhas[escrita.end_lineno:])
ast.parse(mutante)
leitor = pathlib.Path('core/views.py')
leitor_original = leitor.read_bytes()
intruso = (
    b'\n\ndef _mutante_cadastro_sem_turno():\n'
    b'    from chamados.services.fabrica_da_celula import PAUTA_SEM_TURNO\n'
    b'    return Pauta.objects.filter(texto__startswith=PAUTA_SEM_TURNO)\n')

for nome, arquivo, alterado in (
    ('BASE', fabrica, original),
    ('M1_ESCRITA_ANTES_DO_JUIZ', fabrica, mutante.encode()),
    ('M2_SEGUNDO_LEITOR', leitor, leitor_original + intruso),
):
    antes = arquivo.read_bytes()
    try:
        if alterado != antes:
            arquivo.write_bytes(alterado)
        suite = unittest.defaultTestLoader.loadTestsFromTestCase(classe) [nao verificado]
        saida = io.StringIO()
        result = unittest.TextTestRunner(stream=saida).run(suite) [nao verificado]
        print(f'=== {nome} ===', flush=True)
        print(saida.getvalue(), end='', flush=True)
        print(f'RC={int(not result.wasSuccessful())}', flush=True)
        if nome == 'BASE':
            assert result.wasSuccessful()
        else:
            assert len(result.failures) == 1 and not result.errors
    finally:
        if arquivo.read_bytes() != antes:
            arquivo.write_bytes(antes)
    assert arquivo.read_bytes() == antes
assert fabrica.read_bytes() == original
assert leitor.read_bytes() == leitor_original
print('RESTAURACAO: fabrica e core/views.py identicos aos bytes originais')
PY
```

Saída relevante dos testes de `chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199` e `:220`:

```text
=== BASE ===
Ran 11 tests in 1.702s
OK
RC=0

=== M1_ESCRITA_ANTES_DO_JUIZ ===
AssertionError: False is not true : chamados/services/fabrica_da_celula.py::pauta_cadastro_sem_turno: esperado trava -> juiz -> escrever em atomic
Ran 11 tests in 1.695s
FAILED (failures=1)
RC=1

=== M2_SEGUNDO_LEITOR ===
'core/views.py:1310::_mutante_cadastro_sem_turno le/escreve o marcador por conta propria'
'core/views.py:1311::_mutante_cadastro_sem_turno le/escreve o marcador por conta propria'
Ran 11 tests in 1.713s
FAILED (failures=1)
RC=1
RESTAURACAO: fabrica e core/views.py identicos aos bytes originais
```

Ruff nos dois arquivos entregues (`chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:1`; `docs/JUIZES.md:86`):

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache --config ruff.toml chamados/tests/test_juiz_pauta_cadastro_sem_turno.py docs/JUIZES.md
```

```text
All checks passed!
```

## 09/11 21:00 · sessao c358ee63e42c · MODO-COMUM-PASSADO-SEM-TURNO · censo · origem=Code

Quem responde diretamente é **`pauta_cadastro_existente(colaborador_id, dia)`**: procura uma cabeça de Pauta com o marcador, destinada ao DP e ancorada no colaborador; aceita uma viva ou uma fechada cuja data local seja igual ou posterior a `dia`. O contador responde outra pergunta: quantos colaboradores têm essa pendência **viva**. (`app/chamados/services/fabrica_da_celula.py:284`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:320`)

**1. Censo das consultas e varredura AST**

Foram encontrados estes **seis sítios concretos de consulta por âncora de colaborador**:

| Sítio | Seleção | Responde à pergunta investigada? |
|---|---|---|
| `app/chamados/services/fabrica_da_celula.py:284` | `ANCORA_COLAB`, DP, cabeça, prefixo `PAUTA_SEM_TURNO`. | Seleciona exatamente o universo investigado. |
| `app/pautas/services.py:435` | `Q(ancora_tipo='colab', ...)`, unido às pautas de dias da pessoa, em `:436`. | Listagem geral; não interpreta o marcador. |
| `app/ponto/services/afastado_avisa.py:62` | `'colab'`, supervisão, cabeça viva, prefixo `TEXTO[:40]`. | Outro assunto. |
| `app/ponto/services/ausencia.py:1039` | `'colab'`, DP, viva, prefixo `[A4/SEM-DOCUMENTO]`, definido em `:1038`. | Outro assunto. |
| `app/ponto/services/ausencia.py:1229` | `'colab'`, DP, viva, prefixo `[A1-FERIAS/AG#%s]`, definido em `:1228`. | Outro assunto. |
| `app/ponto/services/pautas_dos_deslogados.py:65` | `ANCORA='colab'`, `DEPARTAMENTO='supervisao'`, marcador `[APP-401]`, definidos em `:26–28`. | Outro assunto. |

No universo específico do marcador, todas estas operações consultam ou derivam o queryset:

| Arquivo:linha | Operação |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:283` | Escolhe `Pauta.objects.vivas()` ou `Pauta.objects.all()`. |
| `app/chamados/services/fabrica_da_celula.py:284` | Aplica âncora, destinatário, cabeça e marcador. |
| `app/chamados/services/fabrica_da_celula.py:297` | Filtra colaborador e executa `exists()` nas vivas. |
| `app/chamados/services/fabrica_da_celula.py:301` | Filtra colaborador e lê os fechamentos no histórico. |
| `app/chamados/services/fabrica_da_celula.py:315` | Obtém as vivas para o contador. |
| `app/chamados/services/fabrica_da_celula.py:319` | Restringe pelas pessoas das empresas informadas. |
| `app/chamados/services/fabrica_da_celula.py:320` | Conta `ancora_id` distintos. |

Há ainda uma consulta genérica capaz de receber `'colab'`: `pendentes_por_ancora(tipo, ident)`. Ela conta pautas vivas de qualquer assunto, sem selecionar DP ou marcador; chamador efetivo com `'colab'`: **[nao verificado]**. (`app/pautas/services.py:334`, `app/pautas/services.py:336`)

O script abaixo foi executado em memória com `python3 -B`. Ele procura candidatos por AST, resolve constantes simples e apelidos importados, inclui `Q(...)`, escritas e âncoras não resolvidas para revisão manual. Reflexão, SQL externo e valores montados exclusivamente em execução: **[nao verificado]**.

```python
import ast
from pathlib import Path
mark = 'CADASTRO: PASSADO SEM TURNO PREVISTO'
count = errors = 0
for p in sorted(Path('.').rglob('*.py')):
    if set(p.parts) & {'tests','.venv','venv','node_modules','.git','__pycache__'}:
        continue
    if p.name.startswith('test_') or p.name in {'tests.py','conftest.py'}:
        continue
    count += 1
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError,UnicodeError) as e:
        errors += 1
        print('ERRO',p,e)
        continue
    env = {'ANCORA_COLAB':'colab','PAUTA_SEM_TURNO':mark}
    for n in ast.walk(t):
        if isinstance(n,ast.ImportFrom):
            for a in n.names:
                if a.name in env: env[a.asname or a.name] = env[a.name]
                if a.name == 'Pauta' and a.asname:
                    print(f'ALIAS {p}:{n.lineno} {a.asname}=Pauta')
    def val(n):
        if isinstance(n,ast.Constant): return n.value
        if isinstance(n,ast.Name): return env.get(n.id)
        if isinstance(n,ast.Attribute): return env.get(n.attr)
    for _ in range(3):
        for n in ast.walk(t):
            if isinstance(n,ast.Assign) and val(n.value) is not None:
                for dst in n.targets:
                    if isinstance(dst,ast.Name): env[dst.id] = val(n.value)
    for n in sorted(ast.walk(t),key=lambda n:getattr(n,'lineno',0)):
        if not isinstance(n,ast.Call): continue
        f = ast.unparse(n.func)
        hit = any((k.arg=='ancora_tipo' and val(k.value)=='colab') or
                  (k.arg=='texto__startswith' and isinstance(val(k.value),str)
                   and val(k.value).startswith(mark)) for k in n.keywords)
        dynamic = any(k.arg=='ancora_tipo' and val(k.value) is None for k in n.keywords)
        query = isinstance(n.func,ast.Attribute) and n.func.attr in {
            'filter','exclude','get','get_or_create','update_or_create','create'}
        if hit or (dynamic and query):
            args = ', '.join(k.arg+'='+ast.unparse(k.value) for k in n.keywords
                             if k.arg in {'ancora_tipo','texto__startswith'})
            print(f'{"ALVO" if hit else "DINAMICO"} {p}:{n.lineno} {f}({args})')
print(f'ARQUIVOS={count} ERROS={errors}')
```

Saída:

```text
DINAMICO app/chamados/management/commands/reabrir_orfas_enterradas.py:201 Pauta.objects.filter(ancora_tipo=ANCORA_COMPETENCIA, texto__startswith=MARCADOR)
DINAMICO app/chamados/services/fabrica_da_celula.py:233 Pauta.objects.filter(ancora_tipo=ANCORA_DIA, texto__startswith=PAUTA_LIMBO_EXPORTADA)
ALVO app/chamados/services/fabrica_da_celula.py:284 q.filter(ancora_tipo=ANCORA_COLAB, texto__startswith=PAUTA_SEM_TURNO)
ALVO app/chamados/services/fabrica_da_celula.py:339 escrever(ancora_tipo=ANCORA_COLAB)
DINAMICO app/chamados/services/passivo_validado.py:163 Pauta.objects.filter(ancora_tipo=ANCORA_COMPETENCIA, texto__startswith=cabeca)
DINAMICO app/chamados/services/passivo_validado.py:255 Pauta.objects.filter(ancora_tipo=ANCORA_COMPETENCIA, texto__startswith=cabeca)
DINAMICO app/pautas/management/commands/pauta_pago_x_devido_exportada.py:242 Pauta.objects.nao_canceladas().filter(ancora_tipo=ancora_tipo, texto__startswith=chave + ' ')
DINAMICO app/pautas/services.py:180 Pauta.objects.create(ancora_tipo=ancora_tipo)
DINAMICO app/pautas/services.py:336 Pauta.objects.filter(ancora_tipo=tipo)
ALVO app/pautas/services.py:436 Q(ancora_tipo='colab')
ALVO app/ponto/services/afastado_avisa.py:62 Pauta.objects.filter(ancora_tipo='colab', texto__startswith=TEXTO[:40])
ALVO app/ponto/services/afastado_avisa.py:74 escrever(ancora_tipo='colab')
ALVO app/ponto/services/ausencia.py:1039 Pauta.objects.filter(ancora_tipo='colab', texto__startswith=marcador)
ALVO app/ponto/services/ausencia.py:1055 escrever(ancora_tipo='colab')
ALVO app/ponto/services/ausencia.py:1229 Pauta.objects.filter(ancora_tipo='colab', texto__startswith=marcador)
ALVO app/ponto/services/ausencia.py:1245 escrever(ancora_tipo='colab')
ALVO app/ponto/services/pautas_dos_deslogados.py:65 Pauta.objects.vivas().filter(ancora_tipo=ANCORA, texto__startswith=MARCADOR)
ALVO app/ponto/services/pautas_dos_deslogados.py:77 escrever(ancora_tipo=ANCORA)
ARQUIVOS=1197 ERROS=0
```

Os candidatos `DINAMICO` restantes foram separados assim:

- `ANCORA_COMPETENCIA` e `ANCORA_DIA` correspondem a outros tipos de âncora. (`app/pautas/models.py:33`, `app/pautas/models.py:34`)
- Os três chamadores de `_existente(...)` passam `ANCORA_COMPETENCIA`. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:183`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:222`)
- `Pauta.objects.create(...)` é a implementação da porta de escrita, não uma consulta de entrega. (`app/pautas/services.py:153`, `app/pautas/services.py:180`)
- `pendentes_por_ancora(...)` permanece genérica, conforme descrito acima. (`app/pautas/services.py:336`)

**2. Quem escreve e quem lê o marcador**

**Escritor específico encontrado: um.** `pauta_cadastro_sem_turno` chama `pautas.services.escrever`, assinando por TI, destinando ao DP e usando o colaborador como âncora. O texto começa com `PAUTA_SEM_TURNO` e registra `cel.data` como primeiro dia observado. (`app/chamados/services/fabrica_da_celula.py:333`, `app/chamados/services/fabrica_da_celula.py:339`, `app/chamados/services/fabrica_da_celula.py:341`)

Seu único chamador de produção encontrado é `perguntar_pela_celula`, no ramo `venc is None`. Antes da deduplicação, o escritor abre transação e adquire a trava do colaborador. (`app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:175`, `app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:335`)

O endpoint humano também aceita texto e âncora arbitrários do POST, mas não contém uma referência específica ao marcador. Uso dele para escrever esse marcador: **[nao verificado]**. (`app/pautas/views.py:76`, `app/pautas/views.py:80`, `app/pautas/views.py:81`)

| Leitor | O que efetivamente lê |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:284` | Interpreta o prefixo no banco. |
| `app/chamados/services/fabrica_da_celula.py:297` e `:301` | Usa esse universo para responder “já entregue?”. |
| `app/chamados/services/fabrica_da_celula.py:315` e `:320` | Conta colaboradores com pauta viva desse universo. |
| `app/chamados/management/commands/vigiar_fio_morto.py:179` | Chama o contador; imprime em `:202` e inclui no carimbo em `:229`. |
| `bin/fio_estado.py:104` | Lê o carimbo; compara genericamente `censo` e `esperado` em `:125`, exibindo divergências em `:191`. |
| `bin/gerar_estado.py:260` | Incorpora essa apresentação ao relatório. |

O vigia publica esperado zero, mas esse contador fica fora dos conjuntos que disparam seu alarme. (`app/chamados/management/commands/vigiar_fio_morto.py:83`, `app/chamados/management/commands/vigiar_fio_morto.py:98`, `app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:202`)

Há também leitores **genéricos**, que transportam ou exibem o texto sem interpretar o marcador:

- Aba DP: seleção por departamento e pautas vivas; serialização de `p.texto`; apresentação no copiloto. (`app/pautas/services.py:308`, `app/pautas/services.py:322`, `app/pautas/views.py:40`, `app/templates/core/_copiloto.html:405`)
- API de mensageria: aba padrão DP, lista recebidas e entrega o texto. O cliente e a ferramenta de mensageria repassam esses itens. (`app/api/views_mensageria.py:1324`, `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`, `mensageria/nucleo/core_client.py:431`, `mensageria/nucleo/ferramentas.py:2241`, `mensageria/nucleo/ferramentas.py:2248`)
- Detalhe do colaborador: existe template que mostra estado e texto truncado. A alimentação efetiva do contexto `pautas_do_colab` nessa tela ficou **[nao verificado]**. (`app/templates/colaboradores/detalhe.html:113`, `app/templates/colaboradores/detalhe.html:118`, `app/templates/colaboradores/detalhe.html:119`)

**3. Decisões fora de `perguntar_pela_celula`**

A decisão explícita de **encaminhar esse caso ao CADASTRO** está em `perguntar_pela_celula`: exige passado, colaborador ativo, dia não dispensado por `dia_sem_trabalho`; quando `vencidos_no_instante` devolve `None`, escreve a pauta e retorna sem perguntas. (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:175`)

O significado de `None` vem da ausência simultânea de marcos vencidos e pendentes em uma célula passada. A ausência de marcos também pode resultar de vínculo/tipo inexistente ou de horários de início/fim ausentes. (`app/chamados/services/fabrica_da_celula.py:26`, `app/chamados/services/fabrica_da_celula.py:27`, `app/ponto/services/cartorio.py:856`, `app/escala/servico_jornada.py:72`)

| Componente | Caminho confirmado |
|---|---|
| **`disparar_perguntas_competencia` — célula** | Lê `motivos_que_o_evento_deixou`; este chama `motivos_a_perguntar`. Com `vencidos=None`, `motivos_pela_ata` retorna vazio. Não verifica nessa passagem se a pauta CADASTRO existe. (`app/chamados/management/commands/disparar_perguntas_competencia.py:112`, `app/chamados/services/fabrica_da_celula.py:213`, `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/services/fabrica_da_celula.py:38`) |
| **Mesmo comando — fio mudo** | Bypassa `perguntar_pela_celula`: chama diretamente os helpers `vencidos_no_instante` e `motivos_pela_ata`; sem motivo, pula. O escritor posterior chama `perguntar` diretamente. Portanto cala esse caso por delegação aos helpers, sem encaminhá-lo ao CADASTRO nesse ramo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:160`, `app/chamados/management/commands/disparar_perguntas_competencia.py:282`) |
| **`reconciliador` — consumidor de eventos** | Delega a `perguntar_por_eventos`, que relê as células e chama `perguntar_pela_celula`. (`app/chamados/reconciliador.py:1386`, `app/chamados/reconciliador.py:1387`, `app/chamados/services/fabrica_da_celula.py:270`, `app/chamados/services/fabrica_da_celula.py:272`) |
| **`reconciliador.veredito_orfa`** | Usa `dia_encerrado_motivo`. Sem previsto, não afirma encerramento e devolve `None`; isso conserva a pergunta, sem significar entrega ao DP. (`app/chamados/reconciliador.py:1090`, `app/chamados/reconciliador.py:1097`, `app/chamados/reconciliador.py:1104`, `app/ponto/turnos.py:2123`) |
| **`reconciliador.lavrar_sem_turno_aberto`** | Trata ausência de turno **aberto**; ainda exige que `pergunta_viva` seja falsa e carimba pela porta. Não consulta a pauta CADASTRO. (`app/chamados/reconciliador.py:371`, `app/chamados/reconciliador.py:397`, `app/chamados/reconciliador.py:405`) |
| **`fio`** | A falta de previsão deixa `esperada_prevista=''` para apresentação. A disponibilidade de ação lê o predicado de `raiox`; esses ramos não comprovam entrega ao CADASTRO. (`app/chamados/services/fio.py:325`, `app/chamados/services/fio.py:331`, `app/chamados/services/fio.py:279`, `app/chamados/services/fio.py:283`) |
| **`signals`** | Na volta a VIVOS emite evento da célula; na mudança do chamado também chama o julgamento do cartório. O cartório chama `perguntar_pela_celula`. (`app/chamados/signals.py:201`, `app/chamados/signals.py:207`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1008`) |

**`disputa_emissao` tem decisões próprias de emissão, sem essa guarda CADASTRO.** `_tem_base` aceita batidas como base; `_ata_do_dia` permite continuar sem célula quando não existe escala vigente; os agrupadores usam `_classificar_falta` e `_criar_pergunta_se_couber` diretamente. (`app/chamados/services/disputa_emissao.py:814`, `app/chamados/services/disputa_emissao.py:899`, `app/chamados/services/disputa_emissao.py:906`, `app/chamados/services/disputa_emissao.py:1197`, `app/chamados/services/disputa_emissao.py:1210`, `app/chamados/services/disputa_emissao.py:1232`)

Essa classificação pode devolver motivo usando as batidas mesmo sem sequência prevista. Portanto, o código lido não sustenta que **todo emissor** aplica “passado sem turno → CADASTRO”. Ocorrência concreta desse desvio com dados de produção: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:802`, `app/escala/servico_jornada.py:444`, `app/escala/servico_jornada.py:446`, `app/escala/servico_jornada.py:469`)

**4. Chamadores, significado de `dia` e fechamento**

| Função | Todos os chamadores de produção encontrados |
|---|---|
| `_pautas_cadastro` | `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:315` |
| `pauta_cadastro_existente` | `app/chamados/services/fabrica_da_celula.py:337` |
| `contador_cadastro_sem_turno` | `app/chamados/management/commands/vigiar_fio_morto.py:179` |
| `pauta_cadastro_sem_turno` | `app/chamados/services/fabrica_da_celula.py:175` |

O único chamador de `pauta_cadastro_existente` passa **`cel.data`**. É a data da célula que ancora os marcos; o encadeamento pode avançar os marcos posteriores para o dia seguinte. Assim, `dia` não muda para a data civil da saída noturna. (`app/chamados/services/fabrica_da_celula.py:337`, `app/chamados/services/fabrica_da_celula.py:26`, `app/escala/servico_jornada.py:81`, `app/escala/servico_jornada.py:82`)

O fechamento é interpretado assim:

- **Viva:** `feito_em IS NULL` **e** `cancelado_em IS NULL`, pelo queryset do modelo. (`app/pautas/models.py:57`, `app/pautas/models.py:64`)
- **Entregue com `dia`:** qualquer cabeça viva basta; senão, percorre o histórico e aceita `timezone.localdate(cancelado_em or feito_em) >= dia`. Se ambos estiverem preenchidos, prioriza cancelamento; não calcula o maior instante. (`app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:303`, `app/chamados/services/fabrica_da_celula.py:304`)
- **Sem `dia`:** apenas pauta viva responde verdadeiro. O chamador encontrado passa `dia` explicitamente. (`app/chamados/services/fabrica_da_celula.py:299`, `app/chamados/services/fabrica_da_celula.py:337`)
- **Contador:** não recebe dia e conta somente colaboradores distintos com pauta viva. Por isso, contador zero não implica que um dia antigo deixou de estar “entregue”. (`app/chamados/services/fabrica_da_celula.py:309`, `app/chamados/services/fabrica_da_celula.py:315`, `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:304`)

`hoje` e a data do fechamento usam `timezone.localdate`, sem fuso explícito nessas chamadas. A configuração base declara `America/Sao_Paulo` e `USE_TZ=True`; o fuso efetivamente ativo no processo de produção ficou **[nao verificado]**. (`app/chamados/services/fabrica_da_celula.py:161`, `app/chamados/services/fabrica_da_celula.py:304`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

Há uma diferença entre a explicação e a garantia operacional: o predicado considera **cancelamento** suficiente para entrega, embora cancelar seja ato do autor, e não exija `lido_em`. Logo, verdadeiro não comprova que alguém do DP leu ou resolveu o cadastro. (`app/chamados/services/fabrica_da_celula.py:303`, `app/chamados/services/fabrica_da_celula.py:304`, `app/pautas/services.py:288`, `app/pautas/services.py:294`)

**Número final medido: 1 sítio de produção responde semanticamente “este dia já está entregue ao DP como pendência CADASTRO?” — `pauta_cadastro_existente`.** Ele contém duas consultas, viva e histórica, sobre um único construtor específico do marcador. O censo mais amplo encontrou seis consultas concretas por colaborador, discriminadas acima. (`app/chamados/services/fabrica_da_celula.py:288`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:284`, `app/pautas/services.py:435`, `app/ponto/services/afastado_avisa.py:62`, `app/ponto/services/ausencia.py:1039`, `app/ponto/services/ausencia.py:1229`, `app/ponto/services/pautas_dos_deslogados.py:65`)

## 09/11 20:57 · sessao 6838b687ee95 · - · - · origem=Ronald (tmux)

Censo independente, somente leitura:

| Consulta concreta por âncora de colaborador | Filtro e finalidade |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:284` | `ANCORA_COLAB`, DP, cabeça, marcador `PAUTA_SEM_TURNO`; o marcador é definido em `:276`. |
| `app/pautas/services.py:435` | `Q(ancora_tipo='colab', ...)` em `:436`, unido a âncoras de dia; lista geral de pautas do colaborador. |
| `app/ponto/services/afastado_avisa.py:62` | Literal `'colab'`, supervisão, cabeça, feita/cancelada nulas, prefixo `TEXTO[:40]` (`:63–65`). |
| `app/ponto/services/ausencia.py:1039` | Literal `'colab'`, DP, feita/cancelada nulas; marcador `[A4/SEM-DOCUMENTO]` definido em `:1038`. |
| `app/ponto/services/ausencia.py:1229` | Literal `'colab'`, DP, feita/cancelada nulas; marcador `[A1-FERIAS/AG#%s]` definido em `:1228`. |
| `app/ponto/services/pautas_dos_deslogados.py:65` | Alias `ANCORA='colab'` (`:28`), `DEPARTAMENTO='supervisao'` (`:27`), marcador `[APP-401]` (`:26`). |

São **6 sítios concretos de consulta por colaborador**; somente o primeiro filtra o marcador objeto do censo. Os demais possuem marcadores/finalidades diferentes ou listagem geral, conforme as linhas acima.

A consulta do marcador propaga-se para operações em `app/chamados/services/fabrica_da_celula.py:297` (viva por colaborador), `:301` (histórico por colaborador), `:315` (contador), `:319` (restrição por empresas), `:320` (contagem distinta).

Há duas consultas de âncora parametrizada: `app/pautas/services.py:336`, em `pendentes_por_ancora(tipo, ident)`, e `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:242`, em `_existente(Pauta, ancora_tipo, ...)`. Os três chamadores desta última passam `ANCORA_COMPETENCIA` em `:183`, `:202`, `:222`. Chamador de `pendentes_por_ancora` não encontrado na varredura textual do repositório; uso dinâmico: [nao verificado].

O endpoint genérico aceita texto/âncora do POST em `app/pautas/views.py:76–82`; portanto pode transportar conteúdo arbitrário, mas não contém referência ao marcador. A leitura genérica da aba entrega `p.texto` em `app/pautas/views.py:40`; a lista vem de `sv.listar_por_aba` em `:33`.

O template do colaborador exibe texto e estado em `app/templates/colaboradores/detalhe.html:118–119`, usando contexto `pautas_do_colab` em `:113–115`. Não localizei produtor desse contexto fora da definição do service `app/pautas/services.py:433`; funcionamento desse painel em produção: [nao verificado].

A AST independente percorreu todos os `.py` do repositório, excluindo diretórios `tests` e nomes `test_*.py`. Encontrou as seis consultas acima e cinco chamadas de escrita com âncora colab: fábrica `:339`, afastado `:74`, ausência `:1055` e `:1245`, deslogados `:77`. Somente a fábrica escreve o marcador alvo (`app/chamados/services/fabrica_da_celula.py:341–342`). Nenhum import com alias do modelo `Pauta` ou `ANCORA_COLAB` apareceu nessa varredura; resolução por reflexão/import dinâmico: [nao verificado].

Para o número final, sugiro distinguir **6 sítios físicos de consulta concreta por colaborador**, **1 construtor de queryset específico do marcador** (`app/chamados/services/fabrica_da_celula.py:279`) e **1 predicado que responde semanticamente “este dia já foi entregue ao DP?”** (`app/chamados/services/fabrica_da_celula.py:288`), composto por consulta viva e consulta histórica (`:297`, `:301`).

## 09/11 20:57 · sessao 72b02e642af7 · - · - · origem=Ronald (tmux)

Chamadores de produção encontrados por AST em todos os `*.py`, excluindo diretórios `tests`:

```text
app/chamados/management/commands/vigiar_fio_morto.py:179: contador_cadastro_sem_turno(empresas)
app/chamados/services/fabrica_da_celula.py:315: _pautas_cadastro()
app/chamados/services/fabrica_da_celula.py:337: pauta_cadastro_existente(cel.colaborador_id, cel.data)
app/chamados/services/fabrica_da_celula.py:175: pauta_cadastro_sem_turno(cel)
app/chamados/services/fabrica_da_celula.py:297: _pautas_cadastro()
app/chamados/services/fabrica_da_celula.py:301: _pautas_cadastro(vivas=False)
```

O script executado, sem escrita:

```python
import ast, pathlib
names = {
    'pauta_cadastro_existente', '_pautas_cadastro',
    'contador_cadastro_sem_turno', 'pauta_cadastro_sem_turno',
}
for p in sorted(pathlib.Path('.').rglob('*.py')):
    if 'tests' in p.parts:
        continue
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    aliases = {
        a.asname or a.name: a.name
        for n in ast.walk(t) if isinstance(n, ast.ImportFrom)
        for a in n.names if a.name in names
    }
    for n in ast.walk(t):
        if not isinstance(n, ast.Call):
            continue
        f = n.func
        name = (
            aliases.get(f.id, f.id) if isinstance(f, ast.Name)
            else f.attr if isinstance(f, ast.Attribute) else ''
        )
        if name in names:
            print(f'{p}:{n.lineno}: {ast.unparse(n)}')
```

- `pauta_cadastro_existente` recebe `dia` somente de `cel.data`; seu único chamador é `pauta_cadastro_sem_turno`. Não há chamador de produção omitindo `dia` na saída acima. A definição permite omiti-lo e, nesse caso, consulta somente pautas vivas: `app/chamados/services/fabrica_da_celula.py:337`, `app/chamados/services/fabrica_da_celula.py:299`.
- O escritor recebe a célula de `perguntar_pela_celula`, depois da guarda `cel.data < hoje`, ativo, dia que não é sem trabalho, e `vencidos_no_instante(...) is None`: `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:175`.
- Portanto, o `dia` entregue à deduplicação é a **data da célula**, usada como âncora do turno; não é a data civil da saída de turno noturno. O teto recebe `cel.data`, e `marcos_turno` encadeia os marcos a partir desse dia, avançando ao dia seguinte quando necessário: `app/chamados/services/fabrica_da_celula.py:26`, `app/ponto/services/cartorio.py:841`, `app/escala/servico_jornada.py:62`, `app/escala/servico_jornada.py:81`.
- `hoje` padrão é `timezone.localdate(agora)`; a data do fechamento também é extraída com `timezone.localdate(fechou)`. A configuração base declara `America/Sao_Paulo` e `USE_TZ=True`; a função não fornece um fuso explícito a `localdate`: `app/chamados/services/fabrica_da_celula.py:160`, `app/chamados/services/fabrica_da_celula.py:304`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`.
- Viva significa **feito nulo e cancelado nulo**, pelo queryset do modelo. “Entregue” aceita viva independentemente de `dia`, ou histórica com `cancelado_em or feito_em` cuja data local seja `>= dia`; quando ambos existem, a expressão prioriza cancelamento. `lido_em` não entra nesse juízo: `app/pautas/models.py:57`, `app/pautas/models.py:64`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:303`.
- O contador tem significado diferente: conta colaboradores distintos com pauta **viva**, sem parâmetro `dia` e sem incluir fechadas que ainda respondem “entregue” para dias antigos. Logo, contador zero não implica ausência de entrega ao DP: `app/chamados/services/fabrica_da_celula.py:315`, `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:304`.
- O contador pode recortar colaboradores pelas empresas informadas; não altera a semântica de fechamento: `app/chamados/services/fabrica_da_celula.py:316`.
- O vigia lê o contador, imprime `VFM|CADASTRO_SEM_TURNO`, e grava número e esperado no carimbo. Esse contador não integra os conjuntos usados para disparar alarme: `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:98`, `app/chamados/management/commands/vigiar_fio_morto.py:229`, `app/chamados/management/commands/vigiar_fio_morto.py:83`.
- O relatório instrumental lê o carimbo e compara genericamente todas as chaves de `esperado`; assim pode mostrar `cadastro_sem_turno` como fora do esperado sem ler o marcador no banco. `gerar_estado` incorpora essa apresentação: `bin/fio_estado.py:104`, `bin/fio_estado.py:125`, `bin/fio_estado.py:191`, `bin/gerar_estado.py:260`.
- Tela e API exibem o texto por caminhos genéricos, sem interpretar esse marcador: listagem da aba DP em `app/pautas/services.py:304`, `app/pautas/services.py:322`; serialização em `app/pautas/views.py:40`; apresentação em `app/templates/core/_copiloto.html:405`; API mensageria com aba padrão DP em `app/api/views_mensageria.py:1324`, `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`.
- Existe template de detalhe do colaborador que mostra texto e estado, além do helper `pautas_do_colab` que consulta âncora literal `'colab'`. Não encontrei chamador Python desse helper; a alimentação efetiva dessa seção fica **[nao verificado]**: `app/templates/colaboradores/detalhe.html:118`, `app/templates/colaboradores/detalhe.html:119`, `app/pautas/services.py:433`, `app/pautas/services.py:436`.

## 09/11 20:56 · sessao 780379ccd126 · - · - · origem=Ronald (tmux)

Item 3 — decisões e delegações confirmadas:

| Sítio | Comportamento |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:172` | A guarda explícita reúne passado, colaborador ativo, dia que não é sem trabalho; consulta `vencidos_no_instante` e, se `None`, chama `pauta_cadastro_sem_turno` e retorna sem perguntas (`:173-176`). |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:158` | O caminho **fio mudo** chama diretamente `motivos_pela_ata(... vencidos=vencidos_no_instante(...))`; sem motivo, pula (`:160-161`). Não passa por `perguntar_pela_celula`, mas delega o significado de ausência de previsto aos helpers da fábrica: `app/chamados/services/fabrica_da_celula.py:27` e `:38`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:271` | O escritor do fio mudo chama `fabrica_da_celula.perguntar` diretamente (`:282-283`); essa função cria/atualiza disputa e gera perguntas sem a guarda CADASTRO de `perguntar_pela_celula`: `app/chamados/services/fabrica_da_celula.py:141`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:112` | O caminho **célula** é leitor delegado: chama `motivos_que_o_evento_deixou`; este chama `motivos_a_perguntar`, não escreve CADASTRO: `app/chamados/services/fabrica_da_celula.py:213`. |
| `app/chamados/reconciliador.py:1386` | O consumidor delega a `perguntar_por_eventos`, que relê as células atuais e chama `perguntar_pela_celula`: `app/chamados/services/fabrica_da_celula.py:270`. |
| `app/chamados/reconciliador.py:1090` | `veredito_orfa` consulta `dia_encerrado_motivo`; sem previsto/turno aberto sem escala, retorna `None`, mantendo a pergunta viva, sem enviá-la à Pauta (`:1097-1104`). É decisão diferente de “já entregue ao DP”. |
| `app/chamados/reconciliador.py:371` | `lavrar_sem_turno_aberto` consulta ausência de **turno aberto**, depois exige que `pergunta_viva` seja falsa (`:397-399`) e carimba pela porta (`:405`). Não confundir turno aberto com turno previsto; não é encaminhamento CADASTRO. |
| `app/chamados/signals.py:201` | A volta de chamado a VIVOS emite evento de célula (`:204-207`); a mudança de chamado também julga a célula pelo cartório (`:208-216`). São delegações, sem decisão própria sobre o marcador. |
| `app/chamados/services/fio.py:325` | A ausência de marco previsto aqui apenas deixa `esperada_prevista=''` para apresentação (`:327-333`); não é prova de encaminhamento CADASTRO. |
| `app/chamados/services/fio.py:1306` | O veto de competência do acervo delega a `competencia_do_dia_entregue` (`:1313-1316`); é outro significado de “entregue”, sem relação com a Pauta de falta de turno. |

Atenção ao caminho antigo de `disputa_emissao`: ele não implementa uma regra geral “passado sem previsto → CADASTRO”. `_tem_base` considera batida suficiente (`app/chamados/services/disputa_emissao.py:813`); `_ata_do_dia` permite continuar sem célula quando não há escala vigente (`:899-907`); os laços usam `_classificar_falta` e `_criar_pergunta_se_couber` diretamente (`:1197-1216`, `:1232-1247`). `_classificar_falta` delega ao serviço de jornada (`:802-803`), cujo fallback sem marcos previstos pode devolver motivo de pergunta (`app/escala/servico_jornada.py:438`, `:469-471`). Ocorrência efetiva dessa combinação em produção: [nao verificado].

Não encontrei nesses cinco módulos outra escrita do marcador nem outra consulta que responda “já existe Pauta CADASTRO deste dia”; a evidência positiva da decisão de envio permanece `app/chamados/services/fabrica_da_celula.py:174`. Exaustividade dessa negativa por AST deve ser consolidada pelo agente principal [nao verificado].

## 09/11 20:04 · sessao 2390bdb8a8f2 · FIO-E0-V4-E4-ESCRITORES · censo · origem=Code

Nos anexos, numerei as linhas desde a primeira linha de cada conteúdo, incluindo linhas vazias. Classifico o **achado específico**, distinguindo-o de outros problemas na mesma função.

| achado | classe | trecho .q | trecho .r | linha da triagem | 1 linha de motivo |
|---|---|---|---|---|---|
| **P1 — consulta desnecessária e justificativa C8** | **(e) NÃO PERGUNTADO** | “cada escritor tocado, chamado 2x, produz o mesmo estado, 1 linha de trilha e 0 duplicata?” (`preverif_e4a1.q:41`) | “dedup conserva a linha e só salva reuso com alterações” (`preverif_e4a1.r:162`) | F2 discute “re-emite EventoCelula” e “outbox at-least-once”, sem tratar da consulta de captura (`triagem_preverif_e4a1.txt:24–26`). | Faltou perguntar sobre custo SQL do reuso **sem alteração** e validade da justificativa C8; idempotência de estado não verifica consulta dispensável (`preverif_e4a1.q:41`; `app/chamados/models.py:2958,2993–3003`). |
| **P2 — ramo por `perguntar` continua sem `hoje`** | **(a) tema presente; ramo específico não visto** | “D9: o C6 […] le date.today() / relogio no caminho que este diff toca?” (`preverif_e4a1.q:14–15`) | “Há também o caminho fábrica → desfazer carimbo → `pergunta_viva` → janela, igualmente sem instante injetado” (`preverif_e4a1.r:33`) | “PROCEDENTE […] -> CURADO”; descreve a passagem direta de `hoje`, sem identificar o segundo ramo (`triagem_preverif_e4a1.txt:8–12`). | A resposta viu D9, mas não enumerou o caminho por `perguntar`; a triagem aceitou e curou parcialmente, portanto **não houve descarte desse achado** (`app/chamados/services/fabrica_da_celula.py:140–142,178,188`). |
| **P3 — ausência de contador/vigia** | **(d) tema abordado de forma insuficiente; nenhum dos dois viu** | “dia passado sem marcos previstos NAO gera pergunta por data. Algum caminho […] ainda gera pergunta para esse dia?” (`preverif_e4a1.q:36–37`) | “Pauta sem turno […] **CONFORME:** […] inclui pautas já feitas/canceladas” (`preverif_e4a1.r:165`) | E3 descarta outro problema: “ha batida de entrada, nao e o 'dia passado sem nada'” (`triagem_preverif_e4a1.txt:17–21`). | PASSADO-SEM-MARCOS foi perguntado pelo lado da emissão proibida; faltou explicitar a obrigação positiva do contador e o ciclo após fechar/cancelar a Pauta (`app/docs/PROMPTS.md:2004`; `app/chamados/services/fabrica_da_celula.py:277–304`). |
| **P4 — RED sobrescrito pelo GREEN** | **(c) fora do alcance: evidência de execução posterior** | “selo/RED sem mordida […] algum teste passa com o codigo produtivo revertido” (`preverif_e4a1.q:28–29`) | “CONFORME para os sete mutantes declarados” (`preverif_e4a1.r:107`) | A cura posterior anuncia “RED/GREEN: ponto/tests/test_porta_celula_dia.py” (`triagem_preverif_e4a1.txt:3–7`). | O achado é a perda do registro RED da cura posterior, relatada no anexo; não é o defeito de reversão que o Codex identificou (`opus_final_e4a1_1.txt:24–26`; `triagem_preverif_e4a1.txt:3–7`). |
| **P5 — “o caso comum”** | **(c) fora do alcance temporal da leitura** | “Copia lida: o commit 6a1fc54f7”; “julgue o codigo contra os avais e leis anexos” (`preverif_e4a1.q:2–4`) | “A porta física da sequência continua sendo `registrar`” (`preverif_e4a1.r:54`) | F1 descarta duplicação por replay, não vocabulário (`triagem_preverif_e4a1.txt:22–23`); as curas seguem “sem 2a volta Codex” (`:27`). | A frase foi acrescentada na cura posterior de `registrar([])`; ela não existia no corpo lido em 6a, portanto não cabe atribuir sua ausência àquela resposta (`6a1fc54f7/app/chamados/models.py:3939–3948`; `app/chamados/models.py:3943–3946`). |

A comparação temporal dos snapshots disponíveis mostra:

- **P1:** a consulta já estava em **6a1fc54f7**, exatamente antes de `upd = []`; permaneceu igual em 629f2d244. Portanto, **não nasceu nas curas posteriores**. O aumento dos tetos e a justificativa C8 são posteriores (`6a1fc54f7/app/chamados/models.py:2958–2959`; [models.py:2958](/home/ronald/codex_ws/629f2d244fa0f0c527e56a2697b4c633b263b729/app/chamados/models.py:2958); `6a1fc54f7/app/ponto/tests/test_selo_chokepoint_escrita.py:180,187`; `app/ponto/tests/test_selo_chokepoint_escrita.py:180,187`). Sua introdução especificamente no diff **f2962ab8d..6a1fc54f7**, em vez de já existir na base, permanece **[nao verificado]**: a base e o patch não estavam disponíveis nesta consulta.
- **P2:** o ramo `perguntar_pela_celula → perguntar → desfazer_carimbo_sem_lastro` já existia sem propagação de `hoje`; a cura posterior acrescentou `hoje=hoje` somente à chamada direta. O furo permaneceu, enquanto a cura parcial e seu selo nasceram depois (`6a1fc54f7/app/chamados/services/fabrica_da_celula.py:128–137,180–182`; [fabrica_da_celula.py:178](/home/ronald/codex_ws/629f2d244fa0f0c527e56a2697b4c633b263b729/app/chamados/services/fabrica_da_celula.py:178); `app/chamados/services/fabrica_da_celula.py:188`; `app/chamados/tests/test_e4a_relogio_injetado.py:61–63`).
- **P4:** o bloco de rejulgamento e os testes correspondentes foram acrescentados depois de 6a; a sobrescrita do log é sustentada pelo dado raspado, sem confirmação independente de execução (`6a1fc54f7/app/ponto/portas/celula.py:807–809`; [celula.py:809](/home/ronald/codex_ws/629f2d244fa0f0c527e56a2697b4c633b263b729/app/ponto/portas/celula.py:809); `app/ponto/tests/test_porta_celula_dia.py:257–282`; `opus_final_e4a1_1.txt:24`). Há divergência documental: o `RELATO.md:8` desta cópia contém “E4a 17:09”, não a citação RED atribuída a ele pelo opus ([RELATO.md:8](/home/ronald/codex_ws/629f2d244fa0f0c527e56a2697b4c633b263b729/app/docs/RELATO.md:8); `opus_final_e4a1_1.txt:24`).

**Contagem da tabela: (a) 1; (b) 0; (c) 2; (d) 1; (e) 1. Total: 5.**

## 09/11 19:59 · sessao 81d39397ed64 · - · - · origem=Ronald (tmux)

P3: recomendo **(d)**. O tema PASSADO-SEM-MARCOS foi perguntado em E, e a resposta examinou a Pauta, seus escritores e a deduplicação; não examinou contador/vigia nem o ciclo após fechamento/cancelamento. O código confirma que a existência de qualquer Pauta, inclusive feita/cancelada, impede nova criação (`app/chamados/services/fabrica_da_celula.py:278`; `app/chamados/services/fabrica_da_celula.py:298`). A exigência “uma por colab, com contador” está registrada em `app/docs/PROMPTS.md:2004`.

Trechos relevantes dos anexos:

- `.q`, E: “dia passado sem marcos previstos NAO gera pergunta por data”.
- `.r`, C: “Deduplicação por colaborador, departamento, cabeça e prefixo”.
- `.r`, F: “Pauta sem turno | **CONFORME:** trava, relê existência e só então escreve; inclui pautas já feitas/canceladas”.
- Triagem E3: “DESCARTADO (fila)” refere-se a entradas órfãs com Batida real; **não corresponde à falta do contador**, portanto não sustenta classe (b). Numeração física dos anexos: [nao verificado].

P5: recomendo **(e)**. Faltou pedir fiscalização textual dos termos da LEI-AKITA 1 no diff; a referência genérica às leis não explicita essa conferência. A frase está em `app/chamados/models.py:3944`; a regra textual está em `CLAUDE.md:17–19`. A resposta e a triagem fornecidas não apresentam achado sobre essa frase; numeração física dessa ausência nos anexos: [nao verificado].

Evidências adicionais:

- SILENCIAR exige prazo, tripwire e item de fila: `CLAUDE.md:194`.
- Escopo literal do aval: `CLAUDE.md:37`.
- A Pauta é a única ação nesse ramo: `app/chamados/services/fabrica_da_celula.py:300–304`.
- Busca por `PAUTA_SEM_TURNO` encontrou somente fábrica e testes: `app/chamados/services/fabrica_da_celula.py:274`; `app/chamados/tests/test_e4_c20.py:88`; `app/chamados/tests/test_orfas_pelo_evento.py:288`.

## 09/11 19:59 · sessao f390a20b0f2a · - · - · origem=Ronald (tmux)

P2: o ramo defeituoso já existia na árvore da preverificação: `/home/ronald/codex_ws/6a1fc54f7d428f08aa5e5716ccd5427189718907/app/chamados/services/fabrica_da_celula.py:128–137,180–182`. A comparação dos snapshots mostra que a cura posterior acrescentou `hoje=hoje` apenas à chamada direta atual de `desfazer_carimbo_sem_lastro`, em `app/chamados/services/fabrica_da_celula.py:178`; o ramo por `perguntar` permanece sem propagação em `:133–142,188`.

O selo D9 atual evita esse ramo: `_coleta` retorna `[]` em `app/chamados/tests/test_e4a_relogio_injetado.py:61–63`, e a chamada só ocorre quando `ms` tem conteúdo, em `app/chamados/services/fabrica_da_celula.py:186–188`.

P4: todo o bloco de cura em `app/ponto/portas/celula.py:809–821` e os testes em `app/ponto/tests/test_porta_celula_dia.py:257–282` foram acrescentados após o snapshot 6a1fc54f7. A sobrescrita do RED depende do dado raspado; sua ocorrência não é demonstrável pelo código [nao verificado].

Há divergência documental: o `app/docs/RELATO.md:8` desta cópia contém o registro “E4a 17:09”, sem a citação RED indicada pelo opus; a citação atribuída a essa linha só consta no anexo [nao verificado].

Para a classificação, P4 cabe em **(c)**. P2 não cabe literalmente em **(b)**: a triagem declarou D9 “PROCEDENTE → CURADO”, e o descarte sobre `lavrar` refere-se a outro ramo (`triagem_preverif_e4a1.txt:8–16`). Para usar **(a)**, explicite que a omissão é o ramo `perguntar`, embora a resposta tenha identificado D9 genericamente (`preverif_e4a1.r`, A3/D9; localização numérica [nao verificado]).

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

_3238 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
