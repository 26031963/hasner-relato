# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 10:21** · publicado em **12/11/2026 10:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 10:21 · sessao 1c8e308c2f1a · - · - · origem=Ronald (tmux)

BLOQUEIA — Há inversão de travas entre snapshot e fechamento: snapshot trava célula e depois exclui vínculo; fechamento atualiza vínculo e depois trava célula. (`app/colaboradores/services/vinculo.py:984`; `app/colaboradores/services/vinculo.py:988`; `app/colaboradores/services/vinculo.py:992`; `app/colaboradores/services/vinculo.py:623`; `app/colaboradores/services/vinculo.py:624`; `app/colaboradores/services/vinculo.py:638`; `app/ponto/services/cadastro_futuro.py:22`.)

Contraexemplo: célula C gerada, futura e dentro da janela, nomeando vínculo E que o snapshot removerá; T1 trava C; T2 fecha E e trava E; T1 tenta excluir E e espera T2; T2 tenta travar C e espera T1. A seleção futura admite C, e o snapshot só recusa antecipadamente referências editadas. (`app/ponto/services/cadastro_futuro.py:18`; `app/ponto/services/cadastro_futuro.py:23`; `app/colaboradores/services/vinculo.py:990`.) Execução e detecção efetiva do deadlock no banco: [nao verificado].

## 12/11 10:20 · sessao 3e264079821f · - · - · origem=Ronald (tmux)

BLOQUEIA — Encerramento legítimo deixa contador positivo após o Salvar: célula de 13/11, `trabalha=True`, geradora própria; encerre o vínculo em 12/11 sem sucessor. A porta grava `trabalha=False` e DNA sem marcos, mas mantém a geradora encerrada; `vinculo_do_dia=None` e contador de cobertura=1: `app/colaboradores/services/vinculo.py:637`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:245`, `app/escala/alimentacao.py:282`, `app/ponto/services/vigia_cadastro_futuro.py:36`.

Reproduzido no harness existente, sem banco nem escrita de arquivos. Repetir o evento não zera: a cura exige `trabalha is not False`, e a célula já foi apagada logicamente: `app/ponto/portas/celula.py:239`. O teste da porta aprova esse estado sem medir o contador: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:366`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:372`.

Isso mostra incompatibilidade entre o esperado zero e o estado que a porta preserva deliberadamente; a regra de conservar a célula sem marcos já está documentada no escritor: `app/ponto/portas/celula.py:232`.

## 12/11 10:20 · sessao 1c8e308c2f1a · - · - · origem=Ronald (tmux)

BLOQUEIA — Template não alcança vínculo inativo cuja vigência ainda cobre amanhã: `_propagar_regeneracao_template` filtra `ativa=True`, mas `vinculo_do_dia` aceita geradora inativa dentro das datas. Exemplo: hoje 12/11, EC inativa até 20/11, célula de 13/11; salvar horário 09:00 deixa DNA antigo. Reproduzido com o harness em memória: juíza respondeu EC 2, propagação retornou 0, célula teve 0 saves. (`app/escala/services/cadastro_tipo.py:334`; `app/escala/alimentacao.py:276`; `app/escala/alimentacao.py:282`; harness `app/ponto/tests/test_cadastro_futuro_sem_banco.py:361`.)

BLOQUEIA — Contador de cobertura dá falso zero para geradora inativa sem `data_fim`: a cobertura real retorna falso, mas `vinculo_do_dia` aceita a geradora nomeada porque só compara datas nesse ramo; o vigia conta apenas `None`/PK diferente. Exemplo reproduzido em memória: EC inativa, início anterior a amanhã, fim nulo, célula de amanhã nomeando essa EC → `vinculo_cobre_a_data=False`, `vinculo_do_dia=EC`, contador de cobertura 0. A regeneração também recusa esse EC. (`app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:214`; `app/escala/alimentacao.py:282`; `app/ponto/services/vigia_cadastro_futuro.py:31`; `app/ponto/services/vigia_cadastro_futuro.py:36`; `app/ponto/portas/celula.py:146`.)

BLOQUEIA — Cadastro e cura futura não formam uma transação em várias portas: `executar_vinculo` termina o atomic antes da regeneração e engole a exceção; `salvar_tipo` salva antes da propagação e também engole a exceção; flag de feriado e município salvam antes do suplemento, sem atomic próprio abrangente. Contraexemplo: regeneração falha depois do commit do cadastro → cadastro novo, célula futura antiga, resposta de sucesso possível nas portas que engolem. (`app/colaboradores/services/vinculo.py:188`; `app/colaboradores/services/vinculo.py:299`; `app/colaboradores/services/vinculo.py:326`; `app/colaboradores/services/vinculo.py:331`; `app/escala/services/cadastro_tipo.py:306`; `app/escala/services/cadastro_tipo.py:320`; `app/escala/services/cadastro_tipo.py:322`; `app/colaboradores/services/vinculo.py:862`; `app/colaboradores/services/vinculo.py:878`; `app/colaboradores/services/estrutura.py:68`.)

FILA — `@transaction.atomic` na parte superior de cadastro_tipo decora `_hpd_pares`, não `salvar_tipo`; não comprova atomicidade do Salvar. (`app/escala/services/cadastro_tipo.py:41`; `app/escala/services/cadastro_tipo.py:43`; `app/escala/services/cadastro_tipo.py:140`.)

Mapa de alcance, sempre helper → porta existente em `app/ponto/services/cadastro_futuro.py:30`:

| Porta | Sítio futuro |
|---|---|
| Escala/vínculo | `app/colaboradores/services/vinculo.py:324` |
| Trabalha em feriado | `app/colaboradores/services/vinculo.py:877` |
| Município | `app/colaboradores/services/vinculo.py:927` |
| Template | `app/escala/services/cadastro_tipo.py:360` |
| Admin de template | `app/escala/services/cadastro_tipo.py:367` |
| Signal vigência | `app/escala/signals.py:77` |
| Signal âncora | `app/escala/signals.py:128` |
| Foto/lançar/alterar/lote/reparentar/aplicar calendário | `app/escala/folgas.py:298`; chamadores `:215`, `:337`, `:380`, `:418`, `:523`; `app/escala/views.py:416` |
| Editor calendário | `app/escala/folgas.py:598` |
| Troca calendário vínculo | `app/colaboradores/services/vinculo.py:454` |
| Feriado | `app/ponto/services/cadastro_futuro.py:54` |
| Fechar vigência | `app/colaboradores/services/vinculo.py:637` |
| Snapshot | `app/colaboradores/services/vinculo.py:984` |
| Correção retroativa | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:242` |

FILA — Não há RED novo individual para lançar/alterar/lote/reparentar/aplicar calendário; o teste cobre diretamente a função comum `relavrar_celulas_da_foto`. Tampouco há RED novo de aplicação/desfazimento completo AUTO. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:584`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:466`.)

FILA — AUTO continua criando faltantes além da janela corrente: `_ate` usa última célula, no mínimo início; aplicação/desfazimento chamam regeneração com horizonte habilitado. Exemplo: início depois da janela sem célula alguma → cria a célula inicial. Esse comportamento já consta do censo anterior fornecido. (`app/escala/services/escala_auto_executor.py:52`; `app/escala/services/escala_auto_executor.py:104`; `app/escala/services/escala_auto_executor.py:143`; `app/ponto/portas/celula.py:259`.)

Nada de ampliação passada identificado nos recortes alterados: folgas antes percorriam desde..máximo(ate,fim competência), agora desde..hoje; o suplemento seleciona somente `d>hoje`. (`app/escala/folgas.py:293`; `app/ponto/portas/celula.py:187`; `app/ponto/services/cadastro_futuro.py:18`.)

Nada de novo juiz de vínculo no suplemento de feriado: consulta `vinculo_do_dia`, preservando silêncio da autoridade. (`app/ponto/services/cadastro_futuro.py:54`.)

Nada de criação futura pelo suplemento fora das células existentes: seleciona geradas existentes e chama `[dia,dia]` sem horizonte; editadas ficam fora. (`app/ponto/services/cadastro_futuro.py:22`; `app/ponto/services/cadastro_futuro.py:30`.)

FILA — `None` no calendário tem sentidos documentais divergentes: porta diz “padrão do template”, mas sincronização usa FK literal e o interpretador transforma `None` em conjunto vazio. A view resolve explicitamente o padrão antes da chamada; não há herança dinâmica no interpretador. (`app/colaboradores/services/vinculo.py:441`; `app/escala/folgas.py:625`; `app/escala/folgas.py:52`; `app/colaboradores/views.py:236`.)

Nada de falta de regeneração por mudar `Posto.opera_em_feriado` comprovada: o campo decide o padrão somente de vínculos novos, explicitamente; a porta só salva esse campo. Troca de `Colaborador.posto` também não muda o posto já declarado no vínculo. (`app/escala/models.py:905`; `app/escala/models.py:910`; `app/colaboradores/services/estrutura.py:80`; `app/colaboradores/services/colaborador.py:47`.)

## 12/11 10:20 · sessao 1406670eefaa · - · - · origem=Ronald (tmux)

BLOQUEIA — contador novo não equivale a “geradora cobre o dia”: `vinculo_do_dia` aceita geradora carimbada `ativa=False,data_fim=None`, ou com `data_inicio=None`; `vinculo_cobre_a_data` rejeita esses casos. Reproduzi o primeiro com AST puro: cobertura=False; vínculo do dia=pk1. O vigia pergunta à função real, porém conta zero cobertura nesse caso. `app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:279`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/services/vigia_cadastro_futuro.py:36`.

BLOQUEIA — hoje perde cura do cron e não recebe suplemento de feriado: feriado salvo para 12/11 é descartado pelo filtro estritamente futuro; cron agora também preserva a célula existente de 12/11, diferentemente do diff-base fornecido. Contraexemplo: célula hoje trabalha=True; salva feriado hoje; segue True pelos dois caminhos. `app/ponto/services/cadastro_futuro.py:44`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:870`.

FILA — relógio fora da borda e dois referenciais possíveis: novos helpers leem `timezone.localdate`; cron/porta ainda usam `date.today`; `rejulgar` relê relógio apesar de `hoje` injetado. Configuração Django é São Paulo, USE_TZ=True; fuso do SO em produção [nao verificado]. Contraexemplo condicional: SO UTC em 13/11 01h, São Paulo ainda 12/11; suplemento trata 13/11 como futuro, cron como hoje. `app/ponto/services/cadastro_futuro.py:16`, `app/ponto/services/cadastro_futuro.py:42`, `app/escala/folgas.py:280`, `app/escala/folgas.py:616`, `app/ponto/management/commands/gerar_celulas.py:304`, `app/ponto/portas/celula.py:186`, `app/ponto/portas/celula.py:296`, `app/ponto/portas/celula.py:426`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`.

FILA — ramo fora da vigência grava trabalha/dna sem `dna_anterior/regenerada_em/regeneracoes`; dívida anterior documentada, agora também acionada pelo suplemento futuro. Contraexemplo: encerra hoje geradora de amanhã, sem sucessor; amanhã perde marcos sem os carimbos que consumidores usam para identificar reescrita. `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:243`, `app/docs/LEIS.md:163`, `app/escala/services/regeneracao.py:119`, `app/ponto/services/cartorio.py:393`.

FILA — escritor implícito continua: FK da geradora é SET_NULL; delete de EC modifica a célula fora da porta. Proteção nova recusa editadas no snapshot e no desfazer; células geradas históricas continuam sujeitas ao SET_NULL preexistente. `app/escala/models.py:1693`, `app/colaboradores/services/vinculo.py:988`, `app/colaboradores/services/vinculo.py:990`, `app/colaboradores/services/vinculo.py:992`, `app/colaboradores/services/vinculo.py:1062`, `app/colaboradores/services/vinculo.py:1064`.

Nada — segundo juiz novo nos dois serviços: feriado e vigia chamam `vinculo_do_dia`; registro vivo já nomeia essa função e a cobertura separadamente. Nada — derivação paralela nova de minutos previstos/grade no diff: comparação do vigia usa os mesmos produtores de trabalho/DNA que a regeneração. `app/ponto/services/cadastro_futuro.py:54`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/core/juizes.py:1304`, `app/core/juizes.py:1305`, `app/ponto/services/vigia_cadastro_futuro.py:39`, `app/ponto/portas/celula.py:255`.

Inventário dos campos, com complementos enviados à raiz:

| Campo | Escritores | Leitores principais |
|---|---|---|
| geradora/dna/trabalha/origem | regeneração cria/reescreve; edição humana; reversão; cron cria e enriquece DNA histórico nulo. `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:786`, `app/ponto/portas/celula.py:854`, `app/ponto/portas/celula.py:865`. | juíza; métodos EC; grade; semana; vigia. `app/escala/alimentacao.py:270`, `app/escala/models.py:1095`, `app/escala/models.py:1124`, `app/escala/models.py:1189`, `app/escala/models.py:1235`, `app/escala/services/leitor_celula.py:398`, `app/escala/services/leitor_celula.py:411`, `app/escala/services/semana_do_posto.py:85`, `app/ponto/services/vigia_cadastro_futuro.py:40`. |
| regenerada_em/regeneracoes/dna_anterior | regeneração e reversão. `app/ponto/portas/celula.py:273`, `app/ponto/portas/celula.py:787`, `app/ponto/portas/celula.py:792`. | cobrança órfã/ponte do dia, cartório, invalidadores, comandos. `app/escala/services/regeneracao.py:72`, `app/escala/services/regeneracao.py:104`, `app/ponto/services/dia_do_colab.py:94`, `app/ponto/services/cartorio.py:393`, `app/chamados/veredito_lavrado.py:193`, `app/escala/services/fase_12x36.py:189`, `app/chamados/management/commands/materializar_passivo_validado.py:220`, `app/ponto/management/commands/aplicar_col900.py:173`, `bin/r5_idempotencia_frota.py:57`. |
| FolgaDia/calendario_origem | foto automática; manual; reparentamento. `app/escala/folgas.py:180`, `app/escala/folgas.py:194`, `app/escala/folgas.py:335`, `app/escala/folgas.py:360`, `app/escala/folgas.py:515`. | calendário/manual discriminados pelo nulo; gerador usa foto mensal do vínculo; precedência aceita manual da pessoa e calendário do vínculo do dia. `app/escala/folgas.py:103`, `app/escala/folgas.py:330`, `app/escala/folgas.py:351`, `app/escala/folgas.py:484`, `app/escala/views_plano_folgas.py:113`, `app/ponto/services/declarar_dia.py:115`, `app/escala/models.py:1286`, `app/escala/models.py:1310`, `app/escala/models.py:1356`, `app/ponto/precedencia.py:237`. |

FILA — fallbacks anteriores permanecem: trabalho sem célula; marcos com DNA/marcos nulos mesmo havendo célula; pausa/extra somente sem célula; vínculo por vigência somente sem geradora. Nenhum desses foi introduzido no diff fornecido. `app/escala/models.py:1096`, `app/escala/models.py:1133`, `app/escala/models.py:1208`, `app/escala/models.py:1239`, `app/escala/alimentacao.py:285`.

Nada — inclusividade nova divergente: cobertura e geradora usam limites inclusivos; lâmpadas deslocam marcos noturnos para D+1, preservando a chave do turno; feriado usa a data recebida do dia de escala. `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`, `app/ponto/management/commands/gerar_celulas.py:67`, `app/escala/models.py:1369`, `app/escala/models.py:1395`.

O varredor puro encontrou nove operações atribuídas a CelulaDia, todas dentro da porta, além do SET_NULL explicitado acima; atribuição dinâmica não é certificada por esse censo [nao verificado]. `app/ponto/portas/celula.py:245`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:276`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:609`, `app/ponto/portas/celula.py:794`, `app/ponto/portas/celula.py:854`, `app/ponto/portas/celula.py:866`, `app/ponto/portas/celula.py:1106`.

## 12/11 10:19 · sessao 3e264079821f · - · - · origem=Ronald (tmux)

BLOQUEIA — A suíte pura não está verde: 43 testes executados, 42 passaram e `test_feriado_pergunta_a_juiza_e_respeita_o_silencio` deu ERROR em `copy.deepcopy(vars(self.cel))`, antes de chamar a porta. O dublê `EC` exige argumentos na reconstrução: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:599`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:206`. O mutante `feriado_toma_geradora` repete o mesmo ERROR; esse RED não demonstra mordida diferencial: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:316`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:599`.

BLOQUEIA — O contador com nome “geradora que não cobre o dia” mede a resposta de `vinculo_do_dia`, cujo ramo com geradora diverge da autoridade de cobertura: geradora `ativa=False`, `data_fim=None`, início anterior ao dia retorna `False` em `vinculo_cobre_a_data`, mas é aceita por `vinculo_do_dia`; o vigia devolve ambos os contadores zero se DNA/trabalho coincidirem. Reproduzido em memória usando as funções reais e dublês existentes: `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/services/vigia_cadastro_futuro.py:36`. Caso concreto: célula 13/11 nomeia EC iniciada 01/11, inativa sem fim; há geradora sem vigência declarada, mas contador de cobertura = 0: `app/escala/alimentacao.py:196`, `app/ponto/services/vigia_cadastro_futuro.py:37`.

FILA — Universo da métrica inclui **hoje**, não só `d > hoje`: células existentes `origem='gerada'`, no intervalo recebido, com `data >= hoje`; filtro opcional por colaborador. Unidade = uma célula; editadas excluídas, sem geradora separadas em `sem_geradora`, sem entrar no contador de cobertura: `app/ponto/services/vigia_cadastro_futuro.py:20`, `app/ponto/services/vigia_cadastro_futuro.py:27`, `app/ponto/services/vigia_cadastro_futuro.py:34`. Contra-exemplo: célula inválida de hoje incrementa “celula_futura…”: `app/ponto/services/vigia_cadastro_futuro.py:20`.

FILA — `MUTANTE_CADASTRO` existe apenas no módulo de teste, mas não é somente mudança de expectativa: o carregador lê e modifica AST/fonte das funções reais da produção e executa essas cópias em memória, publicadas nos módulos dublês. Não altera arquivo nem habilita mutante na produção instalada: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:25`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:67`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:92`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:305`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:320`. Os decoradores são removidos e `atomic`/`on_commit` são dublês sem transação; os testes não comprovam ordem transacional real: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:31`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:258`.

Mordidas verificadas em execução em memória:

| Porta/mutante | Resultado | RED |
|---|---|---|
| `executar_vinculo`, guarda `data_inicio <= hoje` | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:339` |
| `marcar_trabalha_em_feriado`, mesma guarda | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:352` |
| município, mesma guarda | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:356` |
| propagação template, mesma guarda | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:361` |
| correção retroativa `_aplicar`, mesma guarda | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:517` |
| `fechar_vigencia`, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:366` |
| trocar calendário, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:497` |
| salvar calendário, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:509` |
| snapshot, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:448` |
| signal âncora, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:569` |
| signal vigência, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:576` |
| relavra foto, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:584` |
| feriado save/delete, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:429`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:435` |
| esquecer data anterior feriado | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:529` |
| feriado escolher EC ativa sobreposta | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:590` |
| vigia tomar geradora sem perguntar | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:688` |
| foto sem recorte / sem relavra mensal / somente dias com célula | FAIL esperado em cada caso | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:538`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:612`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:625` |
| suplemento com `estender_horizonte=True` | FAIL; cria indevidamente 14–20/11 | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:386`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:584` |

A implementação do mutador coloca a guarda somente nos cinco nomes indicados; nas demais portas ele remove a chamada. Assim não afirmar “guarda <= hoje voltou” em todos os casos: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:73`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:80`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:87`.

RED específico “vigência amanhã + guarda <= hoje” para `lancar_folga`, `alterar_folga`, lote, reparentamento, aplicação coletiva de calendário e executor automático: não demonstrado pela nova suíte. Existem testes anteriores, mas os examinados exercitam começo da competência/FK/foto/cadastro, sem provar a troca de geradora futura sob aquele mutante: `app/escala/tests/test_bug136_plano_em_lote_relavra.py:68`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:105`, `app/escala/tests/test_folga_aplicar.py:39`, `app/escala/tests/test_escala_auto_executor.py:66`, `app/escala/tests/test_escala_auto_executor.py:117`. Execução desses testes Django: [nao verificado].

Contador/cron/copiloto: `handle` mede antes de gerar e grava os dois snapshots na mesma transação do comando; API lê o último snapshot da frota e distingue ausência de medida de zero: `app/ponto/management/commands/gerar_celulas.py:310`, `app/ponto/services/vigia_cadastro_futuro.py:47`, `app/ponto/services/vigia_cadastro_futuro.py:63`, `app/api/views_mensageria.py:647`. O endpoint está exposto em `/api/mensageria/saude/`: `app/api/urls.py:16`. Consumo efetivo pelo copiloto/HAIKU em produção: [nao verificado].

Caso mordente do contador: geradora velha termina hoje e célula amanhã ainda a nomeia; teste cobra cobertura=1, nenhuma escrita; após apontar geradora nova com DNA velho cobra divergência cadastral=1: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:243`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:247`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:658`. Ele chama autoridades reais, sem réplica local do julgamento: `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/services/vigia_cadastro_futuro.py:39`.

Cron preservado às 05:50, continua criando faltantes, preservando editadas e enriquecendo DNA nulo do passado, e agora pula existentes de hoje/futuro: `app/config/crons.py:426`, `app/ponto/portas/celula.py:853`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:870`. Os testes puros de criação, editada e cron sem reescrita passaram: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:797`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:803`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:813`.

## 12/11 10:09 · sessao 86c3a6ecfcf4 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

1. **Censo:** feriado e vigia agora perguntam a `vinculo_do_dia`. O vigia compara diretamente o carimbo com a resposta; a variável `geradora` apenas alimenta a juíza (`ponto/services/cadastro_futuro.py:54`, `ponto/services/vigia_cadastro_futuro.py:31`).

2. **Folgas:** a sincronização futura reutiliza `_gerar_foto_folgas_no_ato`, com recorte de datas. A reconciliação mensal existente mantém sua relavra (`escala/folgas.py:166`, `escala/folgas.py:637`).

3. **Feriado relâmpago:** caracteriza cron preservando a divergência e o formulário real `form_feriado` corrigindo no Salvar (`escala/tests/test_celula_dia.py:373`).

4. **Stale:** cobra contador positivo após o cron e zero após `salvar_tipo` (`escala/tests/test_celula_dia.py:328`).

5. **DNA:** cobra futuro atualizado pela porta, cron sem reescrita e DNA histórico preservado (`escala/tests/test_celula_dia.py:146`).

No fluxo alterado:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.escala_geradora` | Porta da célula (`ponto/portas/celula.py:272`) | Juíza e vigia (`escala/alimentacao.py:269`, `ponto/services/vigia_cadastro_futuro.py:36`) |
| `dna` | Porta, usando `montar_dna` (`ponto/portas/celula.py:256`) | Vigia (`ponto/services/vigia_cadastro_futuro.py:40`) |
| `trabalha` | Porta (`ponto/portas/celula.py:272`) | Vigia (`ponto/services/vigia_cadastro_futuro.py:40`) |
| `FolgaDia.calendario_origem` | Escritor existente (`escala/folgas.py:195`) | Escritor e juiz da folga (`escala/folgas.py:187`, `ponto/precedencia.py:231`) |

Comandos para a sessão. **Testes, RED na base e morte dos mutantes: [não verificado]**; não os executei, conforme solicitado. [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1
C2=ponto.tests.test_cadastro_futuro_sem_banco

python3 manage.py test \
  escala.tests.test_contract_juiz_escala \
  ponto.tests.test_catraca_folga_lancada_um_juiz \
  escala.tests.test_celula_dia

python3 -m unittest "$C2"
```

Mutantes em memória e seus testes mordentes (`ponto/tests/test_cadastro_futuro_sem_banco.py:34`, `:309`, `:647`):

```bash
MUTANTE_CADASTRO=feriado_toma_geradora python3 -m unittest "${C2}.EventosFuturosTest.test_feriado_pergunta_a_juiza_e_respeita_o_silencio"

MUTANTE_CADASTRO=feriado_escolhe_outro python3 -m unittest "${C2}.EventosFuturosTest.test_feriado_nao_escolhe_outra_geradora_sobreposta"

MUTANTE_CADASTRO=vigia_toma_geradora python3 -m unittest "${C2}.VigiaTest.test_vigia_compara_carimbo_com_resposta_da_juiza"

MUTANTE_CADASTRO=foto_no_ato_sem_recorte python3 -m unittest "${C2}.EventosFuturosTest.test_calendario_preserva_foto_manual_e_passado"

MUTANTE_CADASTRO=foto_no_ato_sem_relavra_mensal python3 -m unittest "${C2}.EventosFuturosTest.test_foto_mensal_continua_relavrando_e_poupando_manual"

MUTANTE_CADASTRO=foto_so_dias_com_celula python3 -m unittest "${C2}.EventosFuturosTest.test_foto_mensal_inclui_folga_sem_celula_fora_da_janela"

MUTANTE_CADASTRO=_propagar_regeneracao_template python3 -m unittest "${C2}.EventosFuturosTest.test_propagar_regeneracao_template"
```

## 12/11 10:08 · sessao c626fa8aa4e0 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**BLOQUEIA o pouso:** permanecem duas fontes para órfãs, regra própria no PDF e um ramo que impede a recusa nomeada (`app/relatorios/pdf_espelho.py:474`, `:517`; `app/ponto/services/espelho.py:522`, `:563`). O par financeiro obrigatório da competência 11 está **[nao verificado]**.

1. Classificação dos dez:

| Nº | Classe | Conferência |
|---|---|---|
| 1 | **H correta** | Harness substitui também atributos dos pacotes carregados; conserva contador=1 e ausência de `explicados`. Retirar a correção em memória produz falha de busca proibida. Nada. `app/ponto/tests/test_lastro_c12_sem_banco.py:89`, `:103`; `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. |
| 2 | **H correta** | Conserva posto presente, buracos=1 e explicados=0; reversão do harness falha por grade proibida. Nada. `app/ponto/tests/test_lastro_c12_sem_banco.py:109`; `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. |
| 3 | **H correta** | Conserva recusa nomeada e vizinho reconhecido explicado; reversão do harness falha por busca proibida. Nada. `app/ponto/tests/test_lastro_c12_sem_banco.py:141`; `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. |
| 4 | **C correta** | A inversão exige recusa nomeada, `sem_certidao` e veredito vazio: corresponde à leitura sem concluir precedência do aval 4; não virou sucesso genérico. Nada. `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`. Execução Django/ORM: **[nao verificado]**. |
| 5 | **C correta** | `licenca especial` normaliza para `licenca_especial`, fora do mapa; o sucesso agora usa `reciclagem`, já mapeado, e verifica palavra. O par exige recusa nomeada sem gravação: corresponde ao aval 5. Nada. `app/ponto/views_tipos_ausencia.py:92`; `app/ponto/precedencia.py:70`; `app/ponto/tests/test_tela_tipos.py:29`, `:51`. Execução HTTP/ORM: **[nao verificado]**. |
| 6 | **R correta** | A chamada do contrato voltou; o selo original passou adaptado sem banco. Ressalvas de produto no item 3 abaixo. `app/relatorios/pdf_espelho.py:474`; `app/ponto/tests/test_lastro_c13_sem_banco.py:130`. |
| 7 | **R correta** | Mesmo resultado: contrato restaurado, selo original preservado. Isso não prova ausência de regras próprias. `app/relatorios/tests/test_cartao_pela_celula.py:176`; `app/ponto/tests/test_lastro_c13_sem_banco.py:130`. |
| 8 | **R correta quanto ao sintoma** | A expectativa original continua `orfa=True`; a costura restaurada exige isso, mas preserva divergência com a tela, abaixo. `app/relatorios/tests/test_cartao_marca_espuria.py:59`; `app/ponto/tests/test_lastro_c11_sem_banco.py:383`. |
| 9 | **R correta quanto ao sintoma** | A expectativa original continua zero turno aberto por espúria; costura passou. Prova nativa do PDF binário: **[nao verificado]**. `app/relatorios/tests/test_cartao_marca_espuria.py:75`, `:84`; `app/ponto/tests/test_lastro_c11_sem_banco.py:385`. |
| 10 | **H compatível com o delta** | O registro atual aponta a consulta de feriados, e a igualdade estrita permanece. Recolocar `[1032]` em memória causa falha, sem erro de harness. Nada no instrumento atual. `app/ponto/services/espelho.py:1039`; `app/core/tests/test_medida_turno_do_par.py:542`. Identidade AST histórica c9/c12: **[nao verificado]**. |

2. Escritores, leitores e `None`:

- **Nada — escritor do dict:** A2 escreve `recusas[data]`; sem destinatário, levanta. O rótulo nasce na exceção. `app/ponto/precedencia.py:423`, `:422`, `:102`.
- **Nada — transporte:** `_marcar_ausencias` escreve `dia['recusa']` e `tipo_dia=None`; PDF lê a recusa e entrega ao compositor, que escreve palavra nomeada e veredito vazio. `app/escala/utils.py:1219`, `:1220`; `app/relatorios/pdf_espelho.py:567`; `app/ponto/services/espelho.py:339`.
- **Nada — leitores internos alcançados:** legacy preserva `None`; isenção não concede flag de dia de trabalho para esse tipo. `app/escala/utils.py:1046`, `:889`.
- **FILA — incompatibilidade latente:** `classificar_orfas({'tipo_dia': None, 'orfas': ['16:19']})` classifica como `FOLGA_DESMENTIDA`; `por_dia_da_grade` descarta esse tipo sem `ausencia_tipo`. São interpretações incompatíveis com “precedência recusada”. `app/escala/orfa_classe.py:65`; `app/ponto/services/dia_pago.py:392`. Alcance desses leitores pelo canal novo do PDF: **[nao verificado]**.
- **Nada por inspeção do delta com `recusas=None`:** retorno antecipado, condição de consulta A2 e chamada sem argumento conservam os ramos anteriores; o ramo novo fica desativado. `app/escala/utils.py:1205`, `:1213`, `:1218`, `:2114`, `:2117`, `:2365`. Equivalência dinâmica em todo chamador: **[nao verificado]**.
- **Unidades dos contadores:** cartório conta colaboradores isolados; `tipos_ativos_sem_faixa` conta códigos ativos do cadastro, inclusive sem lançamento. Dois colaboradores com um código representam 2 no primeiro universo e 1 no segundo. `app/ponto/management/commands/processar_cartorio.py:73`; `app/ponto/services/vigia_ausencia.py:155`, `:168`.

3. PDF, contrato e igualdade com a tela:

- **BLOQUEIA — duas fontes para a mesma órfã.** PDF reconstrói pelo contrato; tela lê a célula na competência. Contra-exemplo já fixado pelo teste alterado: tela tem `orfas_grade=[]`, enquanto PDF marca a batida como órfã e conta zero aberto. A igualdade `pdf['dias']==dias` compara a lista compartilhada que o próprio coletor modifica. `app/relatorios/pdf_espelho.py:474`, `:571`; `app/ponto/services/espelho.py:522`; `app/ponto/tests/test_lastro_c11_sem_banco.py:326`, `:382`, `:383`, `:384`.

- **BLOQUEIA — regra própria de turno aberto permanece.** PDF une órfãs de todas as datas por HH:MM e usa esse conjunto para excluir abertos. Contra-exemplo executado em memória: órfã em **10/11 07:02** elimina um aberto legítimo de **11/11 07:02**, mesmo sem órfã nesse segundo dia; a tela conta esse aberto. Regra herdada, ainda presente: `app/relatorios/pdf_espelho.py:174`, `:480`, `:517`; `app/ponto/services/espelho.py:436`. Contraria a testemunha sem regra própria: `CLAUDE.md:20`.

- **BLOQUEIA — rótulo não garantido no ramo de ata agregada.** O handler chama `_pela_grade()` sem `recusas`; uma ausência aprovada `fantasma_xyz` levanta `TipoSemFaixa` antes do transporte nomeado. PDF também passa por essa tela depois de obter sua grade. Contra-exemplo: ata agregada que exige o builder + ausência aprovada sem faixa. `app/ponto/services/espelho.py:495`, `:563`, `:653`; `app/relatorios/pdf_espelho.py:479`. Ocorrência produtiva: **[nao verificado]**.

- **Nada no canal coberto do rótulo:** tela e PDF transportam o mesmo `TipoSemFaixa.rotulo` ao mesmo compositor; PDF imprime a palavra resultante. `app/ponto/precedencia.py:102`; `app/ponto/services/espelho.py:880`, `:339`; `app/relatorios/pdf_espelho.py:567`, `:886`.

4. Classes de reprovação:

- **BLOQUEIA — juiz/derivação paralela:** materializações distintas de órfãs e exclusão própria de turno aberto, demonstradas acima. `app/ponto/services/espelho.py:522`; `app/relatorios/pdf_espelho.py:474`, `:517`.
- **BLOQUEIA — selo insuficiente para igualdade:** o teste aceita explicitamente respostas diferentes para a órfã; portanto verde não certifica “cartão=espelho”. `app/ponto/tests/test_lastro_c11_sem_banco.py:383`, `:384`.
- **Segundo escritor fora da porta: nada no delta inspecionado.** A grade copia a recusa da A2; cadastro continua validando antes de gravar. `app/escala/utils.py:1219`; `app/ponto/services/ausencia.py:1312`.
- **Relógio novo fora da borda: nada no delta inspecionado.** Contrato usa o `agora` recebido. `app/escala/utils.py:2359`, `:2364`.
- **Campo com dois significados:** FILA latente descrita no item 2; divergência alcançável causada pelo novo `None`: **[nao verificado]**.

5. Dinheiro:

- **Nada de caminho demonstrado recebendo o novo `None`:** `simular_folha` passa pelo fechamento; fechamento lê `grade_do_fechamento`, cuja fonte é `grade_da_celula`. `app/folha/management/commands/simular_folha.py:122`, `:130`; `app/ponto/services/fechamento.py:1015`, `:1048`; `app/folha/export.py:335`.
- **BLOQUEIA pouso — condição financeira pendente:** par `simular_folha` da competência 11, efeito `abona=0` e diferenças por rubrica: **[nao verificado]**, conforme o dado fornecido. Ausência de movimento financeiro: **[nao verificado]**. [nao verificado]

6. Fora dos dez e do mínimo:

- **Nada identificado no delta fornecido:** alterações produtivas concentram-se no transporte da recusa e na leitura do contrato pelo PDF; testes auxiliares acompanham esses caminhos. `app/escala/utils.py:1159`, `:2350`; `app/relatorios/pdf_espelho.py:474`; `app/ponto/tests/test_lastro_c13_sem_banco.py:177`. Comparação histórica integral da cópia com `86726416c`: **[nao verificado]**.
- **Limite da verificação:** passaram 72 testes sem banco e 35 da catraca; os mutantes executados ficaram vermelhos. Os builders da costura são reduzidos às chamadas extraídas, sem geometria/ORM completos. `app/ponto/tests/test_lastro_c13_sem_banco.py:73`, `:196`, `:200`; `app/core/tests/test_medida_turno_do_par.py:525`. Suite integral, HTTP/ORM e PDF binário: **[nao verificado]**.

## 12/11 10:06 · sessao 0ab2e5092d0c · - · - · origem=Ronald (tmux)

1–3 H: corretas. O defeito é o atributo do pacote já carregado: API importa pelos pais (`app/api/views_mensageria.py:2077`, `:2100`); o harness agora substitui ambos e restaura no cleanup (`app/ponto/tests/test_lastro_c12_sem_banco.py:89`). Asserções continuam exigindo 1 buraco, zero explicados e vizinhos preservados (`:103`, `:109`, `:141`). Reverti somente esse bloco em AST: três falhas de busca/grade proibidas, zero erros; a fixture adversarial está em `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. Achado: nada.

4 C: correta por inspeção; a inversão exige recusa nomeada, `sem_certidao` e veredito vazio (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`). O calendário já recebe a recusa A2 e transporta seu rótulo (`app/colaboradores/services/calendario.py:510`, `:607`, `:714`). Não apagou o caso nem substituiu por sucesso genérico (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:52`). Execução Django/ORM: [nao verificado]. Achado: nada.

5 C: correta. `licenca especial` vira `licenca_especial` pela view (`app/ponto/views_tipos_ausencia.py:92`); esse código não consta do mapa (`app/ponto/precedencia.py:52`). O novo sucesso cria `reciclagem`, cuja faixa já existe (`app/ponto/tests/test_tela_tipos.py:31`; `app/ponto/precedencia.py:70`), verifica crescimento e palavra pelo compositor (`app/ponto/tests/test_tela_tipos.py:37`, `:47`). O par exige não gravar e erro nomeado (`:51`). A porta consulta o juiz antes de gravar (`app/ponto/services/ausencia.py:1312`). Execução HTTP/ORM: [nao verificado]. Achado: nada.

6–7 R: corretas quanto à perda/restauração do contrato; os selos originais exigem o contrato (`app/escala/tests/test_f1c_contrato.py:70`; `app/relatorios/tests/test_cartao_pela_celula.py:176`) e o coletor voltou a chamá-lo (`app/relatorios/pdf_espelho.py:474`). A nova prova executa as mesmas funções dos selos (`app/ponto/tests/test_lastro_c13_sem_banco.py:130`). Achado dos testes: nada; isso sozinho não certifica ausência de regras próprias no restante do PDF (`app/escala/tests/test_f1c_contrato.py:68`).

8–9 R: corretas quanto às expectativas preservadas: espúria deve ser marcada e não produzir turno aberto (`app/relatorios/tests/test_cartao_marca_espuria.py:59`, `:75`). O substituto sem banco agora exige `orfa=True`/zero aberto (`app/ponto/tests/test_lastro_c11_sem_banco.py:384`); suprimir as órfãs restauradas é o mutante (`:68`) e ficou vermelho nesta reexecução. O teste nativo também exige o texto e a batida no PDF binário (`app/relatorios/tests/test_cartao_marca_espuria.py:84`); essa execução está [nao verificado]. Achado dos testes: nada.

10 H: atualização do registro preserva a igualdade estrita (`app/core/tests/test_medida_turno_do_par.py:542`); a ocorrência atual é a consulta de feriados em `app/ponto/services/espelho.py:1039`. Rodei os 35 testes existentes: verdes; recolocar somente `[1032]` em memória causa uma falha de igualdade, zero erros (`app/core/tests/test_medida_turno_do_par.py:541`). A identidade AST histórica c9/c12 está [nao verificado], pois esta cópia não contém `.git`; o delta fornecido não altera o produto `espelho.py` [nao verificado]. Achado: nada.

Verificação própria: os 72 testes C5/C7/C10/C11/C12/C13 passaram; os 11 mutantes C13 e os oito vizinhos executados ficaram vermelhos nas asserções/canais correspondentes (`app/ponto/tests/test_lastro_c13_sem_banco.py:45`, `:73`; `app/ponto/tests/test_lastro_c12_sem_banco.py:34`; `app/ponto/tests/test_lastro_c10_sem_banco.py:296`; `app/ponto/tests/test_lastro_c11_sem_banco.py:68`). São costuras: o teste dos builders reduz seus corpos às chamadas extraídas, portanto não executa a geometria/ORM completa (`app/ponto/tests/test_lastro_c13_sem_banco.py:196`, `:200`).

FILA — portabilidade do replay: `--red-c12` e `--mutante harness_c12` dependem de `git show`, indisponível nesta cópia sem repositório (`app/ponto/tests/test_lastro_c13_sem_banco.py:27`, `:40`). Contra-exemplo: invocar o replay nesta cópia falha antes de exercer a cura; substituí a reversão por AST em memória para verificar a mordida [nao verificado quanto à execução do replay original].

## 12/11 10:06 · sessao 38bc72b0ad37 · - · - · origem=Ronald (tmux)

- **BLOQUEIA — PDF mantém regra própria de órfã/turno aberto, preexistente e não curada.** `_entrada_espuria` compara apenas HH:MM (app/relatorios/pdf_espelho.py:174–178), depois da união das órfãs de todas as datas (:480–481), para excluir turnos abertos (:517–519). Contraexemplo executado em memória: órfã em 10/11 às 07:02; turno aberto legítimo em 11/11 às 07:02, sem órfã nesse dia → função retorna `True` e exclui o turno. Tela conta `p.turno_aberto` sem essa guarda (app/ponto/services/espelho.py:436–437). Viola testemunha sem regra própria (CLAUDE.md:20–22).

- **BLOQUEIA — c13 restaura o contrato no PDF, mas conserva duas respostas para a mesma órfã.** PDF usa builder `grade_espelho_janela` (app/relatorios/pdf_espelho.py:474); tela usa `grade_da_celula`/`remapear_na_regua` (app/ponto/services/espelho.py:522–530). O novo teste fixa concretamente `orfas_grade=[]` para tela e `batida.orfa=True`, `turnos_abertos=0` para PDF (app/ponto/tests/test_lastro_c11_sem_banco.py:383–385). O teste chama tela por `Mock` (:326) e fornece órfã pronta ao builder (:356–357); não certifica convergência dos leitores. Classificações R de 6–9 fazem sentido, mas a cura dos sintomas não fecha a classe.

- **BLOQUEIA — rótulo não está garantido no ramo de ata agregada, defeito anterior não alcançado pelo canal c13.** Tela chama contrato sem `recusas` em `_pela_grade` (app/ponto/services/espelho.py:495–496). Se `grade_da_celula` levanta `AtaAgregadaSemMarcos`, seu handler chama `_pela_grade` (:563); uma `TipoSemFaixa` levantada ali escapa ao `except Exception` irmão (:572). PDF passa pelo contrato com `recusas={}` (app/relatorios/pdf_espelho.py:474), mas depois chama essa tela (:479). Contraexemplo: ata agregada + ausência aprovada sem faixa → leitura pode levantar em vez de apresentar o rótulo. O comportamento sem destinatário está explicitamente preservado e cobrado (app/ponto/tests/test_lastro_c13_sem_banco.py:126–127). Ocorrência produtiva: [nao verificado].

- **Rótulo no canal coberto:** nasce de `TipoSemFaixa.rotulo` (app/ponto/precedencia.py:102), chega ao PDF por `dia['recusa']` (app/escala/utils.py:1219; app/relatorios/pdf_espelho.py:567) e ao compositor único (app/ponto/services/espelho.py:339–341), que escreve palavra e veredito vazio. Tela transporta o mesmo atributo (app/ponto/services/espelho.py:880). Não encontrei contraexemplo novo de apagamento do rótulo por sobreposição em :567 com mesmas entradas/janela; universalidade fora desses canais: [nao verificado].

## 12/11 10:06 · sessao e928c079842b · - · - · origem=Ronald (tmux)

- BLOQUEIA — leitura nomeada incompleta no espelho/PDF: `_pela_grade()` chama o contrato **sem `recusas`** em `app/ponto/services/espelho.py:495`; esse caminho roda no trecho anterior à competência (`app/ponto/services/espelho.py:534`) ou na ata agregada (`app/ponto/services/espelho.py:563`), antes da carga nomeada (`app/ponto/services/espelho.py:653`). Contra-exemplo reproduzido em memória: ausência aprovada `fantasma_xyz` faz `_pela_grade()` levantar `TipoSemFaixa`; o PDF também chama esse espelho (`app/relatorios/pdf_espelho.py:479`). Execução completa com ORM: [nao verificado].

- Escritor do dicionário `recusas[data]`: A2, `app/ponto/precedencia.py:423`; sem destinatário, levanta em `app/ponto/precedencia.py:422`. A recusa impede produzir fato nesse dia (`app/ponto/precedencia.py:425`).

- Escritor de `dia['recusa']` e `dia['tipo_dia']=None`: `app/escala/utils.py:1219` e `app/escala/utils.py:1220`. Leitor produtivo encontrado de **essa recusa da grade**: `app/relatorios/pdf_espelho.py:567`; o compositor imprime a palavra e deixa veredito vazio em `app/ponto/services/espelho.py:339`.

- `tipo_dia=None` também é lido internamente pelo overlay de isenção: não ganha a flag de dia de trabalho (`app/escala/utils.py:889`), chamado depois da ausência (`app/escala/utils.py:1875`); o legacy preserva `None`, pois o default de `.get()` só cobre chave ausente (`app/escala/utils.py:1046`).

- Nenhum caminho financeiro demonstrado recebe o novo `None`: o único chamador produtivo encontrado que abre `recusas` na grade é o PDF (`app/relatorios/pdf_espelho.py:474`); `grade_do_fechamento()` lê `grade_da_celula` (`app/folha/export.py:335`, `app/folha/export.py:338`). Fechamento lê essa grade em `app/ponto/services/fechamento.py:1015` e seus mapas em `app/ponto/services/fechamento.py:1048`; `simular_folha --recalcular/--lido` passa por esse fechamento (`app/folha/management/commands/simular_folha.py:122`, `app/folha/management/commands/simular_folha.py:130`).

- Leitores potencialmente incompatíveis existem, porém alcance pelo canal novo não foi demonstrado: `por_dia_da_grade` descarta `None` sem `ausencia_tipo` (`app/ponto/services/dia_pago.py:392`); `classificar_orfas` traduz `None` para folga (`app/escala/orfa_classe.py:65`). Nenhum chamador produtivo de `classificar_orfas` encontrado no censo textual: [nao verificado]. Não classificar isso como regressão alcançável da c13.

- Sem `recusas`, o delta conserva os guards e chamadas anteriores: retorno cedo em `app/escala/utils.py:1205`, condição de consulta A2 em `app/escala/utils.py:1213`, ramo novo desativado em `app/escala/utils.py:1218`; os transportes omitem o argumento quando `None` (`app/escala/utils.py:808`, `app/escala/utils.py:1010`, `app/escala/utils.py:1019`, `app/escala/utils.py:1873`, `app/escala/utils.py:2114`, `app/escala/utils.py:2117`, `app/escala/utils.py:2365`). Equivalência dinâmica de todos os chamadores: [nao verificado].

- BLOQUEIA pouso financeiro — par `simular_folha` da competência 11 e efeito `abona=0`: [nao verificado], conforme dado fornecido. O comparador mede TXT e retidos (`app/folha/management/commands/simular_folha.py:160`, `app/folha/management/commands/simular_folha.py:218`); inspeção de código não substitui esse par: [nao verificado].

## 12/11 09:57 · sessao 12930e22770f · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura deixada na cópia. Classificação dos dez vermelhos, alterações, RED, mutantes e comandos estão em [LASTRO_C13.txt](LASTRO_C13.txt:1).

Passaram Ruff, 72 testes sem banco, 35 da catraca e 8 estáticos; 22 mutantes foram rejeitados. [Evidências](LASTRO_C13.txt:156) [nao verificado]

Suite com Django, PDF real e par financeiro da competência 11: **[não verificado]**. [Limites](LASTRO_C13.txt:274) [nao verificado]

## 12/11 09:57 · sessao 28365f5c4971 · PUSH-SEM-REGRAVAR · verificador · origem=Code

**O v3 fecha as perdas do comparador anterior, mas não fecha toda a auditoria:** comparar stdout em bytes ainda depende do que o Git apresenta; a mordida de `lugar` depende do contexto; e “histórico já julgado” continua sem comprovação. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:888`)

**1) Fechamento dos achados do v2**

| Achado | Veredito |
|---|---|
| Falso positivo de lugar | **Fecha o defeito do comparador:** agora conserva cabeçalhos, contexto e posição. A fixture distingue as duas alterações, com a ressalva de não vacuidade detalhada abaixo. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:845`, `bin/tests/test_lei_akita.sh:852`) |
| LF final/binário | **Fecha a perda anterior por normalização e filtragem:** retorna `p.stdout` em bytes e acrescenta `--binary`. A comparação deixou de usar `text=True`, `splitlines()` e seleção de linhas `+/-`. Isso garante igualdade da saída capturada, condicionada à apresentação feita pelo Git. (`bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:404`) |
| Mordida C — prosa | **Fecha a ausência da prova específica:** planta a célula com `armadilha:` e reprova qualquer termo que conserve essa prosa. (`bin/tests/test_lei_akita.sh:457`, `bin/tests/test_lei_akita.sh:470`) |
| Mordida D — saída malformada | **Fecha parcialmente:** injeta `lixo` e exige `PADRAO_ERRO`; também protege os dois SHAs do mesmo pid. Porém retirar somente a validação hexadecimal, conservando `len(cs) != 2`, sobrevive aos casos novos. (`bin/tests/test_lei_akita.sh:362`, `bin/tests/test_lei_akita.sh:474`, `bin/tests/test_lei_akita.sh:479`) |
| Frase final | **Fecha a explicação das populações N/M e das dispensas. Não comprova julgamento anterior do publicado.** (`bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:426`, `bin/tests/test_lei_akita.sh:888`) |

As bordas anteriores de vocabulário permanecem: o inteiro com vírgula continua incompatível com o parser do campo; o literal completo `ADR (Architecture Decision Record)` continua sendo separado, em vez de acrescentado inteiro. (`app/docs/GLOSSARIO.md:13`, `app/docs/GLOSSARIO.md:28`, `bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:125`, `bin/tests/test_lei_akita.sh:319`)

**2) Commit novo com diff diferente pode sair como DUPLICATA?** [nao verificado]

**Sim, há um contraexemplo construído com apresentação não injetiva por `textconv`:** os dois produtores de diff permitem essa conversão; nenhum passa `--no-textconv`. A segunda comparação verifica a saída apresentada, sem comparar diretamente árvores ou blobs. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`)

Considere esta configuração e estes commits hipotéticos: [nao verificado]

```text
diff.default.textconv = sed '/^IGNORADO=/d'

Publicado P: [nao verificado]
  A: valor=0 → valor=1

Local L, sobre a mesma base:
  A: valor=0 → valor=1
  F: IGNORADO=0 → IGNORADO=1
```

A conversão conserva o patch de `A` e torna iguais as imagens de `F`, ocultando inteiramente sua alteração. Assim, `log -p` fornece o mesmo patch-id e `show --binary` apresenta os mesmos bytes; `L` encontra `P` e entra em `DUPLICATAS`, embora seu diff bruto contenha uma alteração adicional. Esse é o caminho permitido pelos dois comandos e pela dispensa. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:424`)

Execução ponta a ponta desse cenário com commits reais e ocorrência em produção: **[nao verificado]**. [nao verificado]

Quanto às configurações citadas:

| Configuração | O que se pode concluir |
|---|---|
| `diff.noprefix`, `mnemonicPrefix`, cor | Os dois lados usam o mesmo comando e repositório. Mudança de representação, isoladamente, não demonstra falsa igualdade; se resultar em bytes diferentes, a dispensa falha. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:416`) |
| `textconv` | Pode eliminar informação antes da comparação, como no contraexemplo acima; não exige configuração diferente entre os lados. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`) |
| `ext-diff` | Não há `--ext-diff` nos comandos; a simples ausência de `--no-ext-diff` não demonstra execução de um externo. Habilitação efetiva nesse ambiente: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`) |

O código também não congela configuração entre subprocessos. Uma mudança efetiva de configuração entre as chamadas que tenha causado dispensa indevida nos dados fornecidos: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:416`)

**3) `lugar` tem realmente o mesmo patch-id?**

**Sim, com contexto padrão de três linhas.** A base tem dois blocos iguais; o publicado troca o primeiro `V`, e o local troca o segundo. Reconstruindo esses patches em memória e passando-os a `git patch-id --stable`, ambos resultam em `eef41b6dc7f223182f6b375487979f6d30ec0357`. (`bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:822`, `bin/tests/test_lei_akita.sh:828`, `bin/tests/test_lei_akita.sh:845`)

Os hunks têm o mesmo corpo — contexto `a,b,c`, remoção `V`, adição `W`, contexto `d,e,f` — mas cabeçalhos diferentes: `@@ -1,7 +1,7 @@` e `@@ -9,7 +9,7 @@`. Portanto, o comparador anterior os igualava; o novo distingue seus bytes. (`bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:828`, `bin/tests/test_lei_akita.sh:845`, `bin/tests/test_lei_akita.sh:416`)

**A prova não está protegida contra vacuidade por configuração.** Com contexto quatro, `sep` passa a integrar os contextos em posições diferentes, e os pids reconstruídos divergem: `d05d6ed6fc9a394fc975b679e1d2ff95dfe49b95` e `dc2bd81c57071c4a778df9ca541ed480b4a55ecb`. O comando não fixa `-U3`, e a asserção não exige explicitamente igualdade dos pids; nesse caso, retirar a comparação adicional ainda deixaria `lugar` julgado. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:852`)

**4) Escritores, leitores e verdade da frase**

| Informação | Escritores e leitores |
|---|---|
| `DUPLICATAS` | Zerada por chamada; recebe local8/publicado12 antes do `continue`. MORDE confere o mapa; emissão normal produz `PADRAO_DUPLICATA`; shell escreve `ja julgado:` e conta os registros na frase final. (`bin/tests/test_lei_akita.sh:386`, `bin/tests/test_lei_akita.sh:424`, `bin/tests/test_lei_akita.sh:850`, `bin/tests/test_lei_akita.sh:852`, `bin/tests/test_lei_akita.sh:871`, `bin/tests/test_lei_akita.sh:879`, `bin/tests/test_lei_akita.sh:888`) |
| `JULGADOS` | Zerado com `ENUMERADOS=len(hs)`; incrementado após excluir ausência de linha e duplicatas. MORDE exige quatro; emissão produz `PADRAO_JULGADOS N de M`; shell interpola esse registro. (`bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:426`, `bin/tests/test_lei_akita.sh:852`, `bin/tests/test_lei_akita.sh:873`, `bin/tests/test_lei_akita.sh:888`) |
| Frase final | Escrita pelo `echo` quando `falhas=0`. O pre-push consome o código de saída; a régua captura a saída e a exibe na falha, sem interpretar os contadores. (`bin/tests/test_lei_akita.sh:887`, `bin/tests/test_lei_akita.sh:888`, `bin/pre-push.sh:38`, `bin/regua.sh:140`, `bin/regua.sh:142`) |

**As contagens estão descritas corretamente; “histórico já julgado” não é garantido.** Um publicado sem linha `LEI-AKITA:` pode fornecer o patch usado para dispensar um local com linha inválida, embora o próprio publicado não fosse julgado por esse instrumento. Publicação e julgamento anterior são condições diferentes no código. (`bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:424`)

A alegação das **14 duplicatas reais iguais byte a byte** consta somente do comentário; comprovação no dado fornecido: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:399`)

## 12/11 09:54 · sessao ec9bb461cbab · - · - · origem=Ronald (tmux)

Confirmação adicional: `git hash-object --stdin`, sem `-w`, confirmou os blobs usados na reconstrução da fixture (`bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:828`, `bin/tests/test_lei_akita.sh:845`).

| Contexto reconstruído | Patch-ids | Comparador antigo | Bytes completos |
|---|---|---|---|
| 3 linhas | ambos `eef41b6dc7f223182f6b375487979f6d30ec0357` | iguais | diferentes |
| 4 linhas | `d05d6ed6fc9a394fc975b679e1d2ff95dfe49b95` / `dc2bd81c57071c4a778df9ca541ed480b4a55ecb` | iguais | diferentes |

A condição de pid igual está demonstrada para contexto três; a fixture não fixa esse contexto nem o verifica expressamente (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:852`). A configuração efetiva e a execução integral do MORDE permanecem **[nao verificado]**.

## 12/11 09:54 · sessao c049468680ad · - · - · origem=Ronald (tmux)

Contraexemplo condicional mais forte: publicado altera `A`; local altera `A` identicamente e também um gitlink `S`. Com configuração que oculte completamente a alteração de submódulo, `log -p` e `show --binary` enxergam somente `A`; os pids e os bytes comparados coincidem, entrando em `DUPLICATAS`. Os comandos não fixam `--ignore-submodules=none`. Ocorrência/configuração efetiva e execução desse cenário: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:424`)

Outro contraexemplo condicional usa `textconv`: além do patch comum `A`, o local altera `F`, mas a conversão de `F` devolve o mesmo conteúdo antes/depois e elimina inteiramente seu diff. Ambos os comandos deixam de desabilitar textconv; se `F` desaparecer nos dois, a igualdade dos bytes não detectará a alteração. A omissão efetiva e a configuração são **[nao verificado]**. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`)

Para as cinco configurações pedidas:

- `diff.noprefix`, `color` e `mnemonicPrefix`: os comandos não fixam esses valores; mudança de representação não demonstra, sozinha, falsa igualdade. Não construí contraexemplo de dispensa provocado apenas por esses três. **[nao verificado]**. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:416`)
- `textconv`: candidato de perda de informação descrito acima; `--binary` não é explicitamente acompanhado de `--no-textconv`. (`bin/tests/test_lei_akita.sh:400`)
- `ext-diff`: não há `--ext-diff` nem `--no-ext-diff`; o comando, isoladamente, não prova execução de filtro externo. A habilitação padrão de externos em `show/log` seria uma afirmação sobre Git que exige qualificação **[nao verificado]** sob a restrição de fontes do usuário. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:400`)

Os dois lados de `diff_inteiro` passam pelo mesmo comando, no mesmo `repo`; o código não fornece configurações diferentes por lado nem congela a configuração entre subprocessos. Drift real entre chamadas: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:416`)

A cópia não contém repositório Git utilizável; testar `git show HEAD` falhou por ausência de `.git`. Não executei fixtures nem criei arquivos. **[nao verificado]**. [nao verificado]

## 12/11 09:53 · sessao ec9bb461cbab · - · - · origem=Ronald (tmux)

Referências abaixo em `bin/tests/test_lei_akita.sh`, salvo indicação contrária.

- **`lugar` não é mordida vazia com contexto padrão de três linhas.** A base contém dois blocos iguais (`:820`, `:822`); o publicado muda o primeiro `V` (`:828`) e o local muda o segundo (`:845`). Reconstruí os dois patches em memória e executei somente `git patch-id --stable`, sem criar arquivos: ambos deram `eef41b6dc7f223182f6b375487979f6d30ec0357`. Os hunks são `@@ -1,7 +1,7 @@` e `@@ -9,7 +9,7 @@`, ambos com ` a / b / c / -V / +W / d / e / f`. O cabeçalho `index` também difere: `7c92588..aa0c6d0` versus `7c92588..f53da47`; logo `diff_inteiro` distingue ambos (`:400`, `:416`).
- **Ressalva de configuração:** o código não fixa `-U3` no cálculo de pid (`:390`) nem afirma explicitamente a igualdade dos pids no MORDE (`:852`). Reconstruindo com contexto quatro, os pids passam a `d05d6ed6fc9a394fc975b679e1d2ff95dfe49b95` e `dc2bd81c57071c4a778df9ca541ed480b4a55ecb`: `sep` entra em posições diferentes do contexto. Nesse ambiente a mordida fica vazia. O MORDE apaga variáveis `GIT_`, mas não isola configuração global (`:69`). Configuração e execução em produção: **[nao verificado]**.
- **Lugar, LF final e binário:** a comparação conserva agora stdout em bytes e usa `--binary`, sem `text=True`, `splitlines()` ou filtro `+/-` (`:395`, `:400`, `:404`). Isso fecha os contraexemplos de perda por filtragem/normalização. A nova fixture cobre lugar (`:845`, `:852`); fixtures específicas de LF final e binário no delta: **não há** (`:813`, `:855`).
- **C ganhou mordida específica de prosa:** a fixture inclui `Ggg Hhh (armadilha: iii jjj)` (`:457`) e exige `ggg hhh`, proibindo qualquer termo contendo `armadilha` (`:470`). Reintroduzir a célula inteira com prosa faz essa condição falhar; a implementação remove parênteses (`:125`). A borda da célula com vírgula permanece: `app/docs/GLOSSARIO.md:13`, parser `:319`; o literal inteiro com `ADR (...)` continua não acrescentado por esse caminho (`:125`; `app/docs/GLOSSARIO.md:28`).
- **D ganhou mordida, mas não de cada requisito:** o parser usado pelo instrumento é o helper (`:394`); o MORDE injeta linha válida seguida de `lixo` e exige `PADRAO_ERRO` (`:474`, `:477`). Também exige conservar ambos os SHAs (`:479`). Retirar somente o teste hexadecimal e manter `len(cs) != 2` **sobrevive**: não há caso com dois tokens inválidos (`:362`, `:474`, `:479`). Completude, pertencimento à seleção e stdout vazio continuam sem verificação no helper (`:360`, `:365`). [nao verificado]
- **Frase final:** agora explica corretamente numerador, denominador e número de dispensas (`:888`): `ENUMERADOS=len(hs)` (`:406`, `:407`), pula ausência de linha (`:419`), pula duplicatas (`:423`) e só depois incrementa `JULGADOS` (`:426`). Porém **“histórico já julgado” continua uma premissa**, não uma prova de julgamento anterior: candidatos vêm de `origin/main` e piso (`:412`); não há consulta a resultado anterior (`:416`, `:424`).
- **Escritores/leitores:** `DUPLICATAS` é zerada (`:386`), acrescentada (`:424`), lida pelo MORDE (`:850`), emitida (`:871`), traduzida pelo shell para `ja julgado:` (`:879`) e contada na frase (`:888`). `JULGADOS` é zerado (`:407`), incrementado (`:426`), conferido pelo MORDE (`:852`), emitido com `ENUMERADOS` (`:873`) e interpolado na frase (`:888`). O MORDE termina antes da emissão normal/final (`:866`, `:867`); portanto não testa a veracidade da redação humana (`:888`).

## 12/11 09:53 · sessao f284376b3ac7 · - · - · origem=Ronald (tmux)

- `DUPLICATAS`: declarada global e zerada por chamada; recebe `(local[:8], publicado[:12])` antes do `continue`. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:386`, `bin/tests/test_lei_akita.sh:424`, `bin/tests/test_lei_akita.sh:425`)
- Leitores: MORDE converte em dicionário e exige mapa exato; execução normal emite `PADRAO_DUPLICATA`; shell transforma em `ja julgado:` e conta os registros na frase final. (`bin/tests/test_lei_akita.sh:850`, `bin/tests/test_lei_akita.sh:852`, `bin/tests/test_lei_akita.sh:871`, `bin/tests/test_lei_akita.sh:872`, `bin/tests/test_lei_akita.sh:879`, `bin/tests/test_lei_akita.sh:888`)
- `JULGADOS`: zerado juntamente com `ENUMERADOS=len(hs)`; incrementado depois de excluir commits sem linha e duplicatas. MORDE exige quatro; execução normal emite `PADRAO_JULGADOS N de M`; shell interpola na frase final. (`bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:423`, `bin/tests/test_lei_akita.sh:426`, `bin/tests/test_lei_akita.sh:852`, `bin/tests/test_lei_akita.sh:873`, `bin/tests/test_lei_akita.sh:888`)
- A frase final é escrita pelo `echo` do ramo `falhas=0`. O pre-push usa o código de saída; a régua captura a saída e a exibe apenas na falha; nenhum desses leitores interpreta os contadores. (`bin/tests/test_lei_akita.sh:887`, `bin/tests/test_lei_akita.sh:888`, `bin/pre-push.sh:38`, `bin/regua.sh:140`, `bin/regua.sh:142`)

**Contagem/texto:** a descrição das populações está corrigida: N conta commits com linha que não foram dispensados; M conta todos os enumerados em `head ^origin/main`, respeitado o piso; duplicatas são contadas pelos registros emitidos. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:424`, `bin/tests/test_lei_akita.sh:426`, `bin/tests/test_lei_akita.sh:872`, `bin/tests/test_lei_akita.sh:888`)

**“Histórico já julgado” continua sem prova e não é universalmente verdadeiro:** candidatos publicados são todos os patches de `origin/main` desde o piso, sem checagem de mensagem ou julgamento anterior. Um publicado sem linha `LEI-AKITA:` não seria julgado por esse instrumento, mas ainda pode dispensar um local com o mesmo patch e linha inválida; logo, “publicado” não implica “já julgado”. (`bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:419`, `bin/tests/test_lei_akita.sh:422`, `bin/tests/test_lei_akita.sh:424`)

A afirmação de “14 duplicatas reais [...] iguais byte a byte” está apenas no comentário; comprovação pelos anexos fornecidos: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:399`)

## 12/11 09:49 · sessao 06c5cff16f72 · PUSH-SEM-REGRAVAR · verificador · origem=Code

**O delta fecha a perda de associações e o diagnóstico de registros malformados; fecha parcialmente whitespace, vocabulário e contagem.** A dispensa continua sem provar equivalência de negócio, e o texto final não explicita a população contada. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:856`)

O anexo não identifica os achados por letras; adoto abaixo A=mapa, B=whitespace, C=vocabulário, D=saída malformada, E=contagem. Correspondência com uma rotulação original A–E: **[nao verificado]**. Execução do MORDE, RED/GREEN histórico e comportamento em produção: **[nao verificado]**.

| Achado | A cura fecha? |
|---|---|
| **A — dois locais com mesmo patch-id** | **Sim, para esse defeito.** Cada pid conserva todos os SHAs; a inversão inclui cada local. A busca percorre também todos os publicados daquele pid até encontrar linhas iguais. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:403`, `bin/tests/test_lei_akita.sh:406`) |
| **B — whitespace semântico dispensado** | **Fecha o exemplo `ab` versus `a b`; não fecha equivalência de negócio nem igualdade byte a byte.** Agora exige igualdade das listas filtradas, mas descarta contexto e delimitadores de linha. (`bin/tests/test_lei_akita.sh:358`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`) |
| **C — prosa/nome inteiro** | **Parcial.** Retira a prosa parentética da célula inteira; nomes internos curtos continuam entrando separadamente. Porém a célula de `sinal derivado…` mantém vírgula e continua impossível no campo. O inteiro literal `ADR (Architecture Decision Record)` também deixa de ser acrescentado por esse caminho. (`bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:125`, `bin/tests/test_lei_akita.sh:319`; `app/docs/GLOSSARIO.md:13`, `app/docs/GLOSSARIO.md:28`) |
| **D — `split()`/saída malformada** | **Sim, para quantidade e formato dos campos:** exige dois hexadecimais minúsculos de 40 caracteres e emite `PADRAO_ERRO`. Não verifica completude, pertencimento dos SHAs à seleção ou stdout vazio com sucesso. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:387`, `bin/tests/test_lei_akita.sh:390`) |
| **E — contagem/texto final** | **Parcial.** O MORDE passa a proteger a exclusão de duplicatas em `JULGADOS`; a frase final continua sem explicar essa exclusão nem que o denominador inclui commits sem linha. (`bin/tests/test_lei_akita.sh:397`, `bin/tests/test_lei_akita.sh:409`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:856`) |

**1) Existe local dispensado sem publicado de linhas iguais?** [nao verificado]

**Pelo caminho de duplicata, não.** O único `continue` dessa dispensa depende de `pub`, retornado apenas entre candidatos publicados do mesmo pid cuja lista de linhas seja igual. Local sem associação não encontra candidato e segue ao julgamento. Erro estrito de `git show` interrompe a execução; a ausência de `PADRAO_FIM` reprova no shell. (`bin/tests/test_lei_akita.sh:361`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:413`, `bin/tests/test_lei_akita.sh:848`)

**Literalmente, existe outro caminho:** commit enumerado sem linha `LEI-AKITA:` é ignorado antes de consultar publicado. Isso permanece explícito no contrato de `julgar`. (`bin/tests/test_lei_akita.sh:338`, `bin/tests/test_lei_akita.sh:409`)

“Publicado” significa alcançável de `origin/main`, após a exclusão pelo piso e sem merges; não há verificação de julgamento anterior daquele SHA. Portanto, `ja julgado` expressa uma premissa do instrumento. (`bin/tests/test_lei_akita.sh:373`, `bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:847`)

**2) Há contraexemplos e bordas restantes?**

**Sim, há falso positivo de negócio.** Contraexemplo construído: duas funções do mesmo arquivo contêm blocos suficientemente longos com contexto idêntico; o publicado troca `return 1` por `return 0` na função de pagamento, enquanto o local faz a mesma troca na função de simulação. Diferindo somente posição e identificação da função no cabeçalho do hunk, podem coincidir o patch-id e todas as linhas filtradas. O comparador não conserva a localização da alteração. Ocorrência nos commits reais: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`)

**Também há falso negativo para cherry-pick sobre outra base.** Se a base alterar uma linha de contexto, mantendo exatamente as mesmas remoções e adições, o patch-id pode mudar. Nesse caso, o comparador tolerante a contexto nunca recebe o publicado como candidato. A promessa do comentário de tolerar contexto diferente fica condicionada à igualdade prévia do pid. (`bin/tests/test_lei_akita.sh:381`, `bin/tests/test_lei_akita.sh:393`, `bin/tests/test_lei_akita.sh:406`)

**“Byte a byte” é excessivo.** `text=True` e `splitlines()` normalizam/removem delimitadores; a marca `\ No newline at end of file` também é descartada. Assim, `old\n → new\n` e `old\n → new` podem produzir a mesma lista e o mesmo pid, apesar dos bytes finais diferentes. Consequência de negócio em produção: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:358`, `bin/tests/test_lei_akita.sh:392`, `bin/tests/test_lei_akita.sh:394`)

| Borda | Comportamento |
|---|---|
| **Merge** | Excluído do cálculo de pid e impedido de usar a dispensa; com linha, incrementa `JULGADOS` e passa pelo julgamento dos hunks próprios. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:422`) |
| **Vazio** | Sem registro de pid, não encontra publicado. Com linha, é contado e julgado com lista vazia de caminhos: termo inventado reprova; `n/a (instrumento)` pode passar. Saída efetiva do Git de produção: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:319`, `bin/tests/test_lei_akita.sh:325`, `bin/tests/test_lei_akita.sh:333`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:430`) |
| **Binário** | Quando o diff só contém cabeçalhos e indicação de binário, a lista filtrada fica vazia; a distinção fica a cargo do patch-id. Não foi acrescentada comparação própria de blobs. Não se segue disso que binários diferentes sejam automaticamente duplicatas. Configuração/saída em produção: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`) |

**3) O MORDE morde cada cura?**

As mordidas abaixo são deduzidas das asserções; execução efetiva dos mutantes: **[nao verificado]**.

| Cura | Mutante que pega | Mutante que não pega |
|---|---|---|
| **A — mapa** | Voltar a guardar um único local por pid: exige `dup2` **e** `dup3` no mapa exato. (`bin/tests/test_lei_akita.sh:811`, `bin/tests/test_lei_akita.sh:813`, `bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:820`) | Consultar somente o primeiro publicado do pid: não planta dois candidatos publicados, sendo apenas o segundo exato. (`bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:799`) |
| **B — linhas** | Retirar a comparação: `a b` local seria dispensado contra `ab` publicado, contrariando as falhas esperadas. (`bin/tests/test_lei_akita.sh:799`, `bin/tests/test_lei_akita.sh:814`, `bin/tests/test_lei_akita.sh:820`) | Comparar o diff inteiro, incluindo contexto: não planta cherry-pick com contexto diferente; tampouco cobre posição semântica ou LF final. (`bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:811`, `bin/tests/test_lei_akita.sh:813`) |
| **C — prosa** | As provas existentes pegam retirar o inteiro sem parênteses e acrescentar o inteiro da segunda coluna. **Não há mordida específica da cura de prosa.** (`bin/tests/test_lei_akita.sh:445`, `bin/tests/test_lei_akita.sh:456`) | Reintroduzir a inclusão da célula com prosa parentética: a fixture não contém esse caso. Vírgulas também não são exercitadas. (`bin/tests/test_lei_akita.sh:445`, `bin/tests/test_lei_akita.sh:446`, `bin/tests/test_lei_akita.sh:459`) |
| **D — saída** | **Não há mordida específica nova:** não injeta stdout malformado de `patch-id`. (`bin/tests/test_lei_akita.sh:381`, `bin/tests/test_lei_akita.sh:795`) | Remover a validação nova mantendo o processamento da saída saudável. (`bin/tests/test_lei_akita.sh:387`, `bin/tests/test_lei_akita.sh:389`) |
| **E — contagem** | Contar duplicatas em `JULGADOS`: exige exatamente três, correspondentes à remoção, ao whitespace novo e ao outro patch. (`bin/tests/test_lei_akita.sh:812`, `bin/tests/test_lei_akita.sh:814`, `bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:820`) | Alterar ou manter a descrição humana enganosa: o MORDE termina antes da emissão final. (`bin/tests/test_lei_akita.sh:835`, `bin/tests/test_lei_akita.sh:856`) |

O cenário também pega retirada completa da dispensa, dispensa indiscriminada e associação publicada errada, pois exige simultaneamente falhas e mapa de duplicatas exatos. (`bin/tests/test_lei_akita.sh:817`, `bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:820`)

**4) Quem escreve/lê e o texto continua verdadeiro?**

| Informação | Escritores e leitores |
|---|---|
| **`DUPLICATAS`** | Zerada por chamada; recebe local8/publicado12; lida pelo MORDE e pela emissão normal; o shell transforma o registro em `ja julgado:`. (`bin/tests/test_lei_akita.sh:376`, `bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:818`, `bin/tests/test_lei_akita.sh:839`, `bin/tests/test_lei_akita.sh:847`) |
| **`JULGADOS`** | Zerado com a enumeração; incrementado após os dois filtros — sem linha e duplicata; lido pelo MORDE, emitido em `PADRAO_JULGADOS` e interpolado na frase final. (`bin/tests/test_lei_akita.sh:397`, `bin/tests/test_lei_akita.sh:409`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:841`, `bin/tests/test_lei_akita.sh:856`) |

O `pre-push` usa o código de saída; a régua também usa o código e exibe a saída quando há falha. Esses leitores não interpretam as contagens. Leitores externos: **[nao verificado]**. (`bin/pre-push.sh:38`; `bin/regua.sh:140`, `bin/regua.sh:142`)

**O número de julgados é coerente; a descrição final continua incompleta.** Em `N de M`, **N** conta commits com linha que chegaram ao julgamento, excluindo duplicatas; **M** conta todos os enumerados, inclusive sem linha e duplicatas. A frase seria precisa como: “padrão julgado em N commits; M enumerados; D duplicatas dispensadas”. (`bin/tests/test_lei_akita.sh:397`, `bin/tests/test_lei_akita.sh:409`, `bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:856`)

## 12/11 09:47 · sessao 017bd36f7dcb · - · - · origem=Ronald (tmux)

- **Nenhuma dispensa por duplicata sem comparação igual:** o único `continue` de duplicata exige `pub`, obtido entre publicados do mesmo patch-id com lista `+/-` igual. A associação local preserva todos os SHAs. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:403`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:413`) **Literalmente**, local sem `LEI-AKITA:` continua não julgado sem exigir publicado. (`bin/tests/test_lei_akita.sh:409`)

- **Falso negativo por base diferente:** a comparação tolera contexto/posição diferentes, mas só é alcançada após igualdade de patch-id. Portanto, alteração de contexto que mude o patch-id impede reconhecer um cherry-pick com as mesmas linhas adicionadas/removidas; reprodução nessa configuração Git é **[nao verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`)

- **“Byte a byte” é excessivo:** o subprocesso usa `text=True`, e o comparador usa `splitlines()`; também descarta a marca `\ No newline at end of file`. (`bin/tests/test_lei_akita.sh:358`, `bin/tests/test_lei_akita.sh:392`, `bin/tests/test_lei_akita.sh:394`) Contraexemplo candidato: publicado altera `old\n` para `new\n`; local altera `old\n` para `new` sem LF final. As listas são iguais; se o patch-id ignorar a marca, haverá dispensa apesar dos bytes diferentes. Igualdade dos patch-ids sem execução: **[nao verificado]**.

- **Merge:** excluído do cálculo por `--no-merges`, bloqueado explicitamente no bypass e contado/julgado pelos hunks próprios. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:412`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:422`)

- **Vazio:** sem registro de patch-id, não há candidato publicado; com linha, segue para julgamento e incrementa `JULGADOS`. A ausência efetiva de registro depende da saída do Git **[nao verificado]**. (`bin/tests/test_lei_akita.sh:403`, `bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:430`)

- **Binário:** se `show` emitir apenas cabeçalhos/`Binary files...`, a lista será vazia; a proteção contra binários diferentes permanece no patch-id, sem comparação própria de blobs. A saída/configuração efetiva de produção é **[nao verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:406`)

- **Lacuna do MORDE:** o gerador pesquisa todos os publicados do mesmo pid, corrigindo também “primeiro publicado tem whitespace diferente; segundo é exato”; o caso plantado tem apenas um publicado por pid e não prova essa busca além do primeiro. (`bin/tests/test_lei_akita.sh:406`, `bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:799`, `bin/tests/test_lei_akita.sh:820`)

## 12/11 09:47 · sessao 022365898ec8 · - · - · origem=Ronald (tmux)

Leitura estática, sem execução do MORDE; RED/GREEN efetivo permanece **[nao verificado]**.

| Cura | Mutante mordido pelo MORDE | Mutante/lacuna que escapa |
|---|---|---|
| Dois locais por patch-id | Voltar a conservar apenas um SHA: `dup2` e `dup3` usam o mesmo conteúdo/caminho, e a asserção exige ambos no mapa exato. (`bin/tests/test_lei_akita.sh:811`, `bin/tests/test_lei_akita.sh:813`, `bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:820`) | Procurar somente o primeiro publicado do mesmo pid: o cenário contém apenas um publicado equivalente a `dup`, sem candidato anterior de linhas diferentes. (`bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:799`, `bin/tests/test_lei_akita.sh:819`) |
| Whitespace semântico | Retirar comparação de linhas: publicado contém `ab`, local contém `a b`, e local deve permanecer entre falhas. (`bin/tests/test_lei_akita.sh:799`, `bin/tests/test_lei_akita.sh:814`, `bin/tests/test_lei_akita.sh:820`) | Comparar diff inteiro, incluindo contexto: a prova nova só recria arquivo `dup.py` com conteúdo idêntico; não planta cherry-pick com contexto alterado. (`bin/tests/test_lei_akita.sh:797`, `bin/tests/test_lei_akita.sh:811`, `bin/tests/test_lei_akita.sh:813`) |
| Prosa em parênteses | **Não há mordida específica nova.** A fixture de vocabulário usa célula sem parênteses; casos de `julgar` tampouco usam prosa do glossário real. (`bin/tests/test_lei_akita.sh:445`, `bin/tests/test_lei_akita.sh:446`, `bin/tests/test_lei_akita.sh:459`, `bin/tests/test_lei_akita.sh:482`) | Reintroduzir o loop v1 que também adicionava a célula inteira com prosa preservaria as asserções existentes de inteiro/pedaços/segunda coluna. (`bin/tests/test_lei_akita.sh:456`, `bin/tests/test_lei_akita.sh:457`) |
| Saída malformada patch-id | **Não há injeção específica de stdout malformado.** Os erros plantados são repositório raso, falta de origin e falta de corte. (`bin/tests/test_lei_akita.sh:715`, `bin/tests/test_lei_akita.sh:720`, `bin/tests/test_lei_akita.sh:829`) | Retirar a validação nova de quantidade/formato de campos não é exercitado por esses preparos saudáveis de patch-id. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:387`, `bin/tests/test_lei_akita.sh:795`) |
| Contagem | Contar duplicatas em `JULGADOS`: esperado é exatamente 3, correspondentes a remoção, whitespace novo e outro patch; as duas duplicatas ficam de fora. (`bin/tests/test_lei_akita.sh:812`, `bin/tests/test_lei_akita.sh:814`, `bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:820`) | Alterar somente a frase final para continuar sem explicar a exclusão não muda o valor validado pelo MORDE; a frase é emitida fora da execução MORDE. (`bin/tests/test_lei_akita.sh:834`, `bin/tests/test_lei_akita.sh:835`, `bin/tests/test_lei_akita.sh:856`) |

O cenário também morde dispensa indiscriminada, retirada completa da dispensa e associação publicada errada, pois exige simultaneamente conjunto exato de falhas e mapa exato de duplicatas. (`bin/tests/test_lei_akita.sh:817`, `bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:820`)

A cura de prosa retira os parênteses antes de adicionar a célula inteira; `Architecture Conformance Test (armadilha: ...)` e `Offline Fallback (network-first: ...)` deixam de entrar com prosa. O termo `Architecture Decision Record` ainda entra isoladamente pelo filtro de nome entre parênteses. (`bin/tests/test_lei_akita.sh:84`, `bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:125`; `app/docs/GLOSSARIO.md:15`, `app/docs/GLOSSARIO.md:28`, `app/docs/GLOSSARIO.md:31`)

Vírgula continua aberta: a célula `sinal derivado do estado, reconciliacao level-triggered` ainda entra inteira, mas o parser de `padrao=` não admite vírgula no valor. Não existe teste dessa célula na fixture nova. (`app/docs/GLOSSARIO.md:13`; `bin/tests/test_lei_akita.sh:125`, `bin/tests/test_lei_akita.sh:127`, `bin/tests/test_lei_akita.sh:319`, `bin/tests/test_lei_akita.sh:445`)

## 12/11 09:42 · sessao 1644ac242a95 · PUSH-SEM-REGRAVAR · verificador · origem=Code

A pré-verificação encontra um defeito concreto em (a): **dois locais com o mesmo patch-id perdem uma das associações**, podendo deixar uma duplicata sujeita a reprovação. Em (b), há ampliação para prosa e nomes inteiros que entram no vocabulário, mas continuam impossíveis de usar em `padrao=` por conterem vírgulas. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

Não executei o MORDE completo, pois ele cria arquivos e repositórios temporários; os resultados históricos RED/GREEN e “4/4 mutantes” permanecem **[não verificado]**. (`bin/tests/test_lei_akita.sh:429`, `bin/tests/test_lei_akita.sh:432`, `bin/tests/test_lei_akita.sh:642`)

1. **Aval versus implementação, por item.**

   **(a)** O aval transcrito pede dispensar o commit cujo patch-id coincide com um publicado e emitir `DUPLICATA de <sha publicado>`; a transcrição não está disponível como arquivo local **[não verificado em arquivo]**. O código implementa essa dispensa antes de `julgar`, mas somente para commits com linha `LEI-AKITA:`, com até um pai e presentes no mapa local; merges continuam pelo hunk próprio. Portanto, não cobre literalmente todo commit duplicado, e a perda de associações no mapa impede reconhecer todas as duplicatas elegíveis. (`bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:410`, `bin/tests/test_lei_akita.sh:385`)

   A saída humana fica `ja julgado: <local8> DUPLICATA de <publicado12>`, e a duplicata deixa de incrementar `JULGADOS`. (`bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:404`, `bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:822`)

   **(b)** O aval transcrito pede o nome inteiro da primeira coluna, além dos pedaços **[não verificado em arquivo]**. O código acrescenta tanto a célula inteira quanto sua versão sem parênteses, preservando a extração anterior das duas primeiras colunas; não acrescenta a segunda coluna inteira. Entretanto, o parser do campo ainda impede os nomes com vírgulas. (`bin/tests/test_lei_akita.sh:98`, `bin/tests/test_lei_akita.sh:117`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

   Os anexos também divergem no testemunho de `7313a031c`: o aval cita `072556ec8`, enquanto a mensagem do commit cita `4aa0a00fb`. Se ambos são publicados equivalentes, **[não verificado]**; o código escolhe o primeiro publicado encontrado para cada patch-id. (`bin/tests/test_lei_akita.sh:385`)

2. **Quando conteúdo diferente pode receber o mesmo patch-id.**

   A tabela descreve a semântica do comando escolhido; sua reprodução na versão/configuração Git de produção é **[não verificado]**. A chamada efetiva usa `log -p --no-merges --no-renames` seguido de `patch-id --stable`. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:379`)

   | Caso | Consequência |
   |---|---|
   | Whitespace | É ignorado no cálculo: diferenças de indentação ou espaços dentro de strings podem preservar o patch-id e mudar comportamento. O bypass não faz comparação adicional de árvore, blob ou AST. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:401`) |
   | Base/contexto | Patch-id identifica o patch normalizado, não a árvore resultante nem sua equivalência semântica em outra base; o código aceita somente essa identidade. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:401`) |
   | Modo de arquivo | Cabeçalhos de mudança de modo participam do patch; “só modo” pode produzir patch-id e ser duplicata. Não é automaticamente vazio. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:401`) |
   | Binário | Não se pode afirmar que todo binário recebe o mesmo ID: a representação binária incorpora identificação dos blobs. Não há prova dessa borda no MORDE novo. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:778`; comportamento em produção **[não verificado]**) |
   | Rename | `--no-renames` apresenta remoção e adição, incluindo caminhos e conteúdo; o bypass não se baseia apenas no rótulo “rename”. (`bin/tests/test_lei_akita.sh:378`) |
   | Commit vazio | Não produz registro de patch-id; portanto, não recebe um “ID vazio comum” que o torne duplicata. Sem associação local, segue o julgamento normal se tiver linha. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:401`) |

   **Há possibilidade de dispensar código de negócio novo sem julgá-lo:** por exemplo, patches que diferem apenas entre `return 'a b'` e `return 'ab'`. Isso decorre do critério de equivalência escolhido, além do defeito do mapa; sua ocorrência nos commits fornecidos é **[não verificado]**. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:403`)

   Um vazio com linha e termo inexistente continua vermelho; vazio com `n/a (instrumento)` pode passar por não tocar negócio. Um commit só de modo, sem correspondência publicada, continua sujeito ao julgamento dos caminhos alterados e pode reprovar `n/a` em `app/`. (`bin/tests/test_lei_akita.sh:323`, `bin/tests/test_lei_akita.sh:331`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:418`)

3. **Publicados, erros e `split()`.** [nao verificado]

   Os publicados são os commits não merge alcançáveis de `origin/main`, excluída a ancestralidade do pai do corte; é um limite de grafo, não uma data. Não há restrição a primeiro pai nem checagem de que o publicado teve efetivamente julgamento anterior. (`bin/tests/test_lei_akita.sh:371`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:394`)

   `origin/main` ausente provoca erro estrito na enumeração; repositório raso é recusado explicitamente; retorno não zero de `patch-id` produz `PADRAO_ERRO`. Esses caminhos não viram conjunto vazio silencioso. (`bin/tests/test_lei_akita.sh:359`, `bin/tests/test_lei_akita.sh:363`, `bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:388`)

   **Há conjuntos vazios legítimos:** nenhum local enumerado; nenhum publicado restante após o piso; ou nenhum patch emitido, como numa seleção só de vazios. O código também aceita stdout vazio com retorno zero sem validar sua completude. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:395`)

   Na saída normal, cada registro tem dois campos; commit vazio simplesmente não gera registro. Mas `pid, sha = ln.split()` lança `ValueError` diante de linha com quantidade diferente de campos, sem convertê-lo em `PADRAO_ERRO`; dois campos inválidos também não são validados. A ocorrência dessa saída anormal é **[não verificado]**. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:384`)

   Exceções de execução ou decodificação dos subprocessos também não têm tratamento geral. Ainda assim, uma interrupção antes do fim deixa faltar `PADRAO_FIM`, e o shell reprova: diagnóstico fora do padrão, mas não verde por esse caminho. (`bin/tests/test_lei_akita.sh:356`, `bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:817`, `bin/tests/test_lei_akita.sh:823`)

4. **Locais, merges, raízes e mapa.**

   `--no-walk=unsorted` seleciona os SHAs fornecidos sem percorrer seus ancestrais; `--no-merges` exclui merges do cálculo. Os merges permanecem em `hs` e seguem o julgamento de hunks próprios. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:405`)

   Raízes são elegíveis porque a condição é `len(pais) <= 1`. Na configuração usual, `log -p` mostra sua criação contra a árvore vazia; a configuração efetiva, inclusive `log.showRoot`, é **[não verificado]**. O julgamento posterior possui tratamento explícito de raiz. (`bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:416`)

   **O mapa fica errado para dois locais com o mesmo patch-id:** `setdefault(pid, sha)` conserva apenas o primeiro SHA; inverter esse dicionário não recupera o outro. Um local omitido será julgado mesmo sendo duplicata publicada. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:401`)

5. **Termos novos do GLOSSÁRIO real.**

   São **dez entradas novas no conjunto**, provenientes das nove células abaixo; somente sete são utilizáveis pelo parser atual de `padrao=`. A normalização remove marcação, converte para minúsculas e compacta espaços. (`bin/tests/test_lei_akita.sh:83`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

   | Célula que ganha aceite integral | Uso no campo |
   |---|---|
   | `Single Source of Truth / Aggregate Root` | Aceito. (`app/docs/GLOSSARIO.md:11`) |
   | `sinal derivado do estado, reconciliacao level-triggered (o oposto, edge-triggered, e o fio morto)` | Entra inteiro **e sem o parêntese**, mas ambas as entradas contêm vírgula e são recusadas pelo parser. (`app/docs/GLOSSARIO.md:13`; `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`) |
   | `Architecture Conformance Test (armadilha: selar a FORMA e nao o SIGNIFICADO)` | Aceito, incluindo a prosa. (`app/docs/GLOSSARIO.md:15`; `bin/tests/test_lei_akita.sh:331`) |
   | `Parallel Change / Expand-Contract` | Aceito. (`app/docs/GLOSSARIO.md:18`) |
   | `Shadow mode / Parallel run` | Aceito. (`app/docs/GLOSSARIO.md:21`) |
   | `Backfill / Reprocessing` | Aceito. (`app/docs/GLOSSARIO.md:23`) |
   | `Duplicated business logic / logic drift` | Aceito. (`app/docs/GLOSSARIO.md:25`) |
   | `ADR (Architecture Decision Record)` | Aceito; o inteiro é novo, embora seus componentes já fossem extraídos. (`app/docs/GLOSSARIO.md:28`; `bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:123`) |
   | `Offline Fallback (network-first: sem rede, a copia do cache; sem copia, o erro de rede de sempre -- nunca undefined no respondWith)` | Entra no conjunto após normalização, mas suas vírgulas impedem uso no campo. (`app/docs/GLOSSARIO.md:31`; `bin/tests/test_lei_akita.sh:83`, `bin/tests/test_lei_akita.sh:317`) |

   Portanto, **sim, prosa passa a ser aceita integralmente** no caso de `Architecture Conformance Test`. O filtro antigo para parênteses que parecem nomes não é aplicado pelo loop novo à célula inteira. (`app/docs/GLOSSARIO.md:15`; `bin/tests/test_lei_akita.sh:88`, `bin/tests/test_lei_akita.sh:123`)

6. **Quem escreve e quem lê.**

   | Informação | Escrita e leitura |
   |---|---|
   | `DUPLICATAS` | Zerada por chamada, preenchida com local8/publicado12; lida pelo MORDE novo e pela emissão normal. (`bin/tests/test_lei_akita.sh:374`, `bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:794`, `bin/tests/test_lei_akita.sh:814`) |
   | `JULGADOS` | Inicializada na enumeração e incrementada depois da exclusão de duplicatas; emitida em `PADRAO_JULGADOS`. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:404`, `bin/tests/test_lei_akita.sh:816`) |
   | `ENUMERADOS` | Total de `hs`, incluindo sem linha e duplicatas; lida na saída e nas asserções de sincronização, outra ref e piso. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:676`, `bin/tests/test_lei_akita.sh:701`, `bin/tests/test_lei_akita.sh:802`, `bin/tests/test_lei_akita.sh:816`) |
   | `PADRAO_FALHA` | Python emite; shell conta e transforma em `FALHA: PADRAO-NA-CURA`. (`bin/tests/test_lei_akita.sh:813`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:821`) |
   | `PADRAO_DUPLICATA` | Python emite; shell transforma em `ja julgado:`. (`bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:822`) |
   | `PADRAO_JULGADOS` | Consumida pelo próprio selo para compor a frase final de sucesso. (`bin/tests/test_lei_akita.sh:816`, `bin/tests/test_lei_akita.sh:831`) |
   | `PADRAO_FIM` / `PADRAO_ERRO` | O sentinela é exigido; não há parser dedicado de `PADRAO_ERRO`, cuja interrupção provoca ausência do sentinela. (`bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:823`) |

   `pre-push.sh` passa o SHA e decide pelo código de saída; `regua.sh` também decide pelo código de saída e apenas exibe a saída quando há falha. A adição da linha de duplicata não quebra esses leitores. (`bin/pre-push.sh:38`; `bin/regua.sh:140`, `bin/regua.sh:142`)

   Outro selo que grepe essa saída não foi encontrado na varredura da cópia; leitor externo, ignorado ou ausente da cópia é **[não verificado]**.

7. **MORDE e mutantes.**

   O caso sintético de vocabulário morde retirar o nome inteiro e acrescentar também o inteiro da segunda coluna; os dois casos de `julgar` mordem negar `Parallel Change / Expand-Contract` e aceitar uma composição inventada só porque contém um pedaço válido. (`bin/tests/test_lei_akita.sh:444`, `bin/tests/test_lei_akita.sh:449`, `bin/tests/test_lei_akita.sh:450`, `bin/tests/test_lei_akita.sh:471`)

   O caso de duplicata morde retirar a dispensa, dispensar indiscriminadamente o outro patch ou emitir associação publicada errada naquele cenário. Contudo, o patch diferente está em **outro arquivo**, e existe apenas um local com o patch-id publicado. (`bin/tests/test_lei_akita.sh:783`, `bin/tests/test_lei_akita.sh:789`, `bin/tests/test_lei_akita.sh:791`, `bin/tests/test_lei_akita.sh:795`)

   Permanecem lacunas concretas: dois locais com mesmo patch-id — inclusive o defeito atual —; mudança diferente no mesmo caminho; whitespace semântico; vazio/modo/binário/raiz; erro ou saída malformada de `patch-id`; prosa e vírgulas nas células. Esses casos não são plantados pela prova nova. (`bin/tests/test_lei_akita.sh:433`, `bin/tests/test_lei_akita.sh:778`)

   Também sobreviveria à asserção nova um mutante que continuasse contando duplicatas em `JULGADOS`, porque ela verifica somente falhas e associação da duplicata. Um mutante que devolvesse `{}` apenas quando `patch-id` falha não seria exercitado pelo cenário saudável; sua execução efetiva é **[não verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:795`)

8. **As três classes de reprovação citadas.**

   **Selo/RED sem mordida:** as asserções novas têm ligação com os comportamentos pretendidos, mas não provam todas as bordas; o defeito de múltiplos locais passa pela prova nova. O RED histórico alegado continua **[não verificado]**. (`bin/tests/test_lei_akita.sh:444`, `bin/tests/test_lei_akita.sh:795`, `bin/tests/test_lei_akita.sh:385`)

   **Erro operacional virando verde:** não encontrei esse caminho nas falhas explícitas examinadas; ausência de `PADRAO_FIM` reprova, e `MORDE_OK` sozinho não basta quando existe `MORDE_FALHOU`. Há, porém, exceções fora de `PADRAO_ERRO` e stdout vazio com sucesso sem validação adicional. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:823`, `bin/tests/test_lei_akita.sh:827`)

   **Campo com dois significados:** o formato `N de M` permanece, mas `JULGADOS` agora exclui duplicatas com linha. Nenhum dos dois leitores operacionais identificados depende da contagem; o próprio texto final continua dizendo “commit(s) com linha LEI-AKITA”, sem explicar a exclusão, e o MORDE novo não protege essa semântica. (`bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:795`, `bin/tests/test_lei_akita.sh:831`; `bin/pre-push.sh:38`; `bin/regua.sh:140`)

_4154 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
