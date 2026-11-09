# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 23:32** · publicado em **08/11/2026 23:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 23:32 · sessao 3dbe97426882 · - · - · origem=Ronald (tmux)

Censo estático: **10 chamadas produtivas diretas** a `ChamadoColaborador.reabrir`; não inclui `ponto.services.fechamento.reabrir`, que opera `FechamentoMensal` (`app/ponto/services/fechamento.py:1349`).

| Chamador | Atomic externo à migração | O que faz com False |
|---|---|---|
| `_renascer_por_premissa_viva` | Via `abrir`, sim: atomic `app/chamados/models.py:902`, chamada transitiva `:922`. Via resposta tardia, nenhum atomic local: `app/chamados/services/resposta_tardia.py:31`, `:45`; view `app/chamados/views.py:1039`, `:1053`. | Propaga False em `app/chamados/models.py:1019`. `abrir` descarta esse bool e retorna `(existente, False)`, cujo segundo elemento significa “criado”, não “reaberto” (`:922–924`). Resposta tardia faz nota/log/Pauta e devolve estado `tardia`, sem anunciar reabertura (`app/chamados/services/resposta_tardia.py:45`, `:52`, `:54`, `:58`, `:67`). |
| Desarquivar | Sem atomic local, view apenas `login_required` (`app/chamados/views_arquivar.py:51–70`). | Devolve JSON `ok=False`, mas texto atribui **qualquer recusa** a irmão vivo (`:70–72`); para universo mudou, razão exibida é errada. |
| Sincronizar adesão | Sem atomic local; comando chamador também não envolve atomic (`app/chamados/services/adesao.py:44`, `app/escala/management/commands/apurar_furos_diarios.py:6–21`). | **Conta como reaberto mesmo False**: `pai.reabrir(); r['pais_reabertos'] += 1` (`app/chamados/services/adesao.py:92`), depois imprime contador (`app/escala/management/commands/apurar_furos_diarios.py:21`). |
| Reabrir disputa admin | Sem atomic no serviço (`app/chamados/services/acoes_disputa.py:232–296`), no dispatcher (`app/chamados/services/fio.py:485`, `:583`) ou nas duas views de entrada (`app/chamados/views.py:658–674`, `:868–954`). | **Ignora False** (`app/chamados/services/acoes_disputa.py:267`); anexa ocorrência, envia pushes e responde sucesso (`:271`, `:275`, `:287`, `:296`). A mensagem fala da disputa, efetivamente reaberta, mas o chamado pode continuar encerrado. |
| Reabrir competência | Sim, atomic por chamado (`app/chamados/services/competencia_trancada.py:264`). | Só nota/log/incremento se True (`:265–272`); False pula efeitos, não levanta erro. |
| Resolver perguntas por chamado resolvido | Sim: wrapper usa atomic existente ou abre um (`app/chamados/services/disputa_emissao.py:1720–1722`). Ramo da tranca sai antes de chegar à chamada (`:1715–1716`, `:1742–1743`). | Só anexa nota de reabertura se True (`:1812–1814`). |
| Comando reabrir órfãs | Sim, atomic por pergunta (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`). | Ignora bool, mas relê estado, registra `pai_falhou` ou incrementa reabertos (`:172–177`), imprime falhas (`:185–189`). **Não desfaz** pergunta/disputa já reabertas (`:157–167`). |
| Comando ausência aberta | Sem atomic local (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:22–55`). | Só nota e incremento se True (`:49–54`). |
| Reconciliar grade | Sem atomic no caminho `_emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:488–531`); atomic `:472` é de outro método e termina antes. | **Ignora False**, retorna None (`:530–531`). Chamador interpreta None como ausência de veto e incrementa `plan['CREATE']` / `creates` (`:254–259`). |
| Detectar ausências, limbo | Sem atomic no caminho do comando (`app/ponto/management/commands/detectar_ausencias.py:61`, `:271–303`). | Ignora False (`:301`), sem contador específico nem aviso da recusa nesse chamador (`:300–304`). |

**Fronteira transacional:** `migrar_resolvedoras_vivas` abre `transaction.atomic()` com configuração padrão (`app/chamados/services/fio.py:2074`); levanta em `:2091` **antes** de executar `_migrar_sob_trava` em `:2094`. Antes da recusa há leituras e `select_for_update`; a função de trava não grava dados de negócio (`app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`). `reabrir` captura **depois da saída** desse atomic (`app/chamados/models.py:1415–1417`). Sob atomic externo saudável, a conclusão contratual é rollback ao savepoint e conexão utilizável; o próprio repositório documenta esse mecanismo em `app/core/observ.py:124–133`. Sem atomic externo, esse bloco abre uma transação e a encerra com rollback; a ocorrência da recusa entra depois em outro atomic próprio (`app/chamados/models.py:1421`, `app/core/observ.py:133`). Validação efetiva da conexão/Django em execução: **[nao verificado]**.

Configurações da cópia não ativam `ATOMIC_REQUESTS` nem desativam autocommit nos dicionários de produção/on-prem e SaaS (`app/config/settings/prod.py:9–18`, `app/config/settings/saas.py:83–92`); configuração efetiva carregada em produção: **[nao verificado]**.

**Há escrita anterior que fica:** no fluxo admin, `d_fec.reabrir_admin` salva a disputa (`app/chamados/services/acoes_disputa.py:256`, `app/chamados/models.py:2169`), abre Pergunta de conversa (`app/chamados/services/acoes_disputa.py:260`) e só depois tenta reabrir o chamado (`:267`); a recusa não desfaz essas escritas, nem bloqueia os efeitos posteriores (`:271–296`). No modal ainda há marcação de notas lidas antes do dispatcher (`app/chamados/views.py:879–880`, `:954`). Isso é parcialidade do **ato maior**, não migração pela metade (`app/chamados/services/fio.py:2091–2094`).

Mesmo com savepoint externo há parcialidade de negócio intencional/concreta: órfãs já alteraram Pergunta/Disputa quando pai recusa (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157–175`); competência grava `PeriodoFechado` antes de tentar devolver chamados, em gancho best-effort, e depois reabre fechamentos mensais (`app/ponto/services/fechamento.py:1435–1452`).

A trilha de `universo_mudou_sob_trava` é tentada pela própria porta (`app/chamados/models.py:1421–1425`), inclusive nos chamadores que ignoram False. Persistência não é garantida: `evento` é best-effort e retorna None se falhar (`app/core/observ.py:94–105`, `:133–139`); sucesso real do INSERT sem banco/log: **[nao verificado]**.

**Lacuna dos testes para esta pergunta:** o teste de `reabrir` substitui migração por `side_effect` e substitui `evento` por mock (`app/chamados/tests/test_e1_fila.py:499–505`); verifica False e argumentos (`:514–518`), mas não executa a integração rollback/savepoint → INSERT da trilha → escrita posterior do chamador. Essa garantia integrada permanece **[nao verificado]**.

**Possível ordem de trava transitiva a investigar:** atomic de `abrir` já segura um Colaborador (`app/chamados/models.py:905`), órfãs já seguram C/D/Ch/P (`app/chamados/management/commands/reabrir_orfas_enterradas.py:153–156`), resolver já trava universo e pode ter escrito Pergunta antes de reabrir pai (`app/chamados/services/disputa_emissao.py:1721`, `:1787`, `:1812`). A migração então pode adquirir conjunto adicional de Colaboradores (`app/chamados/services/fio.py:2084–2087`), inclusive de pergunta alheia, caso expressamente admitido em comentário (`:2080–2081`). A ordem local ordenada não prova a ordem da transação inteira; existência/alcançabilidade de dados que realizem esse caminho: **[nao verificado]**.

## 08/11 23:31 · sessao ec542bc3f363 · - · - · origem=Ronald (tmux)

No escopo do diff, com distinção entre defeito novo e limite já existente:

| Classe | Veredito e prova |
|---|---|
| Segundo juiz duplicando juiz registrado | **Não.** O novo teste substitui auth/sombra/casca por respostas falsas, sem reimplementar suas regras; deploy continua chamando `janela_auth.sh`. O juiz de horário permanece `proibido()`. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:54-56`; `bin/deploy.sh:145`; `bin/janela_auth.sh:53-61`. |
| Segundo escritor fora da porta | **Não introduzido.** `APLICAR_MIGRATE=0` inicializa estado local e `=1` decide apply; não há publicador novo: seguem um restart e um HUP. A exclusão de marca morta por deploy é escritor de limpeza **já existente**, além do envelope produtor/removedor. `bin/deploy.sh:61,108-109,223,326,343`; `bin/cron_run.sh:9-10`. |
| Relógio fora da borda | **Não introduzido.** O diff não acrescenta leitura de relógio; auth concentra `date` nas costuras `_dow/_hhmm`, com substituição explícita de teste. O caso 11 usa `/proc/1`, não relógio. `bin/janela_auth.sh:45-56`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`; `bin/deploy.sh:109`. |
| Derivação paralela de fonte única | **Não no patch.** A decisão de aplicar continua derivada do `--check` e modo, em um ramo; a seção 2 consome a mesma variável. Não nasce regra auth paralela. `bin/deploy.sh:184-188,204-224,298-300,325`; `bin/deploy.sh:145`. |
| Selo/RED sem mordida | **Não é inteiramente sem mordida**, mas **sim, há lacunas relevantes**. Caso 11 mata a recusa incondicional pós-apply, porém só cria `cron_falso.1`; o Docker falso não mata nem reexecuta cron. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68,76,150-152`. RED efetivamente executado nesta volta: **[nao verificado]**. |
| Ordem de trava | **Sim.** Checar cron antes de migrate e depois dispensar a checagem não impede admissão concorrente: envelope marca e executa sem coordenação com deploy. `bin/deploy.sh:299-300,325-326`; `bin/cron_run.sh:9-11`. Ausência de cron no instante observado não reserva o intervalo. |
| Pouso antes do pré-requisito | **Sim, pela regra ainda escrita “restart recusa com cron vivo”.** Depois de migrate, o novo ramo permite restart apesar da marca recém-criada, contrariando a promessa vigente. `CLAUDE.md:90`; `bin/deploy.sh:325-326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150-152`. **Outro resto do contrato literal “todo portão antes de migrate”:** selo de reciclagem continua posterior a migrate/reload e pode reprovar por configuração/crontab. `bin/deploy.sh:300,326,343,409`; `bin/selo_reciclagem.sh:46-78`. |
| Campo com dois significados | **Não demonstrado em `APLICAR_MIGRATE`, apesar da prosa incorreta.** Na escrita `=1` significa “apply planejado”; no leitor `:325`, o controle de fluxo permite inferir “apply retornou 0”, pois falha em `:300` termina por `falhou`, definido em `:92`. Portanto o novo leitor tem pré-condição válida; não lê intenção como sucesso antes de executar. `bin/deploy.sh:92,223,298-300,325`. O comentário “UM leitor” é falso: há dois, `:298` e `:325`, além de dois pontos de atribuição, `:61` e `:223`. |

Outras observações úteis para consolidar:

- O selo estrutural antigo só busca o texto `recusa_restart_com_cron` antes do restart; a linha nova mantém esse texto mesmo quando o curto-circuito dispensa sua execução. Assim, esse selo aceita a perda da garantia universal. `bin/contrato_deploy.sh:45-47`; `bin/deploy.sh:325`.
- O teste de reload gracioso verifica cron existente **no começo**; não verifica o nascido durante migrate. `bin/tests/test_deploy_reload_gracioso.sh:125-131`.
- “Todo cron é idempotente” é requisito escrito, não prova de reexecução: o envelope executa uma vez, mede RC, carimba somente 0/2 e alerta falha; não contém repetição automática do comando interrompido. `CLAUDE.md:246`; `bin/cron_run.sh:11-13,22-25,29-38`.
- Semântica dos campos novos do teste: `FAKE_CRON_NO_MIGRATE` é presença habilitadora, `FAKE_HERDA_APLICAR` injeta ambiente, `N_RST` conta linhas de chamadas restart. Seus escritores e leitores concordam nos usos fornecidos, mas `N_RST=1` não prova três cascas reiniciadas nem cron preservado. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68,90,101,146,150-151`.
- A marca de cron registra PID do **envelope do host**, não identidade do processo `docker exec`; deploy usa apenas existência de `/proc/<pid>`, sem conferir identidade ou conteúdo do arquivo. Isso admite falso “vivo” por PID reutilizado e não é novidade do diff. `bin/cron_run.sh:9-11`; `bin/deploy.sh:108-109`.

## 08/11 23:31 · sessao 73a57bb44d60 · - · - · origem=Ronald (tmux)

- **`APLICAR_MIGRATE`:** escritores executáveis em `bin/deploy.sh:61` (`0`) e `bin/deploy.sh:223` (`1`); leitores em `bin/deploy.sh:298` e `bin/deploy.sh:325`. O comentário “UM leitor” ficou falso com o segundo leitor; há também duas atribuições, embora ambas pertençam ao mesmo script (`bin/deploy.sh:59`). Fora dele, o selo remove a variável herdada e opcionalmente injeta `APLICAR_MIGRATE=1` (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`); o caso 10 ativa a injeção e exige zero migrations (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:147`). O valor herdado é sobrescrito antes dos leitores (`bin/deploy.sh:61`, `bin/deploy.sh:298`, `bin/deploy.sh:325`).

- **`RESTART`:** escritores em `bin/deploy.sh:70` (`0`) e `bin/deploy.sh:73` (`1`, somente pelo argumento `--restart`); leitores em `bin/deploy.sh:115`, `bin/deploy.sh:200`, `bin/deploy.sh:299`, `bin/deploy.sh:322` e `bin/deploy.sh:428`. O valor herdado é sobrescrito (`bin/deploy.sh:70`). Fora dele, `bin/contrato_deploy.sh:41` lê o **texto** do `if` para conferir a posição do restart; não consome o valor da variável.

- **Marcas de produção:** `bin/cron_run.sh:9` cria o diretório e deriva `<nome>_<empresa>.<PID do envelope>`; `bin/cron_run.sh:10` escreve a marca e instala sua remoção no `EXIT`. `bin/deploy.sh:108` enumera as marcas; `bin/deploy.sh:109` lê existência/PID e também **apaga** a marca cujo PID não existe. A pergunta é consumida em `bin/deploy.sh:111` e pode recusar em `bin/deploy.sh:112`; chamadas em `bin/deploy.sh:117`, `bin/deploy.sh:299` e `bin/deploy.sh:325`.

- **Marcas fora de produção:** o selo gracioso escreve uma marca com PID de processo real falso (`bin/tests/test_deploy_reload_gracioso.sh:89`, `bin/tests/test_deploy_reload_gracioso.sh:91`); o selo desta obra escreve `cron_falso.1` durante o migrate falso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`). Os diretórios falsos são criados em `bin/tests/test_deploy_reload_gracioso.sh:34` e `bin/tests/test_deploy_portoes_antes_do_migrate.sh:45`. `bin/contrato_deploy.sh:81` e `bin/contrato_deploy.sh:82` procuram a presença textual de `crons_em_curso`; não leem marcas.

- **Limites semânticos das marcas:** o escritor registra o PID do envelope no host e um timestamp (`bin/cron_run.sh:7`, `bin/cron_run.sh:9`, `bin/cron_run.sh:10`); o leitor ignora o conteúdo e confere somente `/proc/<PID>` (`bin/deploy.sh:109`). Isso não identifica a execução corrente dentro de uma casca nem protege contra reutilização de PID (`bin/cron_run.sh:9`, `bin/deploy.sh:109`). O escritor usa caminho absoluto (`bin/cron_run.sh:9`), o leitor usa `RAIZ` calculada da cópia (`bin/deploy.sh:53`, `bin/deploy.sh:108`); equivalência desses caminhos em produção: **[nao verificado]**.

- **Corrida do restart:** não existe exclusão comum entre o nascimento da marca (`bin/cron_run.sh:9`, `bin/cron_run.sh:10`) e a sequência observar → migrar → reiniciar (`bin/deploy.sh:299`, `bin/deploy.sh:300`, `bin/deploy.sh:325`, `bin/deploy.sh:326`). Com `APLICAR_MIGRATE=1`, um cron novo deixa de recusar na seção 2 (`bin/deploy.sh:325`). Se estiver executando dentro das cascas reiniciadas, o risco de morte é o que o próprio deploy documenta (`bin/deploy.sh:312`, `bin/deploy.sh:314`); ocorrência e exit concretos em produção: **[nao verificado]**.

- **“Morto e refeito” não está demonstrado:** a frase está em `bin/deploy.sh:296`; idempotência é requisito textual em `CLAUDE.md:246`. O envelope executa uma vez (`bin/cron_run.sh:11`), não carimba término em rc diferente de 0/2 (`bin/cron_run.sh:22`), tenta registrar alarme (`bin/cron_run.sh:32`, `bin/cron_run.sh:33`) e sai (`bin/cron_run.sh:38`). Não implementa retry. A corrente segue adiante registrando erro (`bin/encadeado.sh:64`, `bin/encadeado.sh:66`); seus dependentes podem esperar o carimbo até alarmar (`bin/encadeado.sh:89`, `bin/encadeado.sh:92`, `bin/encadeado.sh:94`). Reexecução efetiva e idempotência de **todos** os comandos: **[nao verificado]**.

- **Outra ordenação concebível:** impedir admissões novas de cron sob uma trava também obedecida pelo envelope, drenar os existentes e manter essa exclusão durante migrate e publicação eliminaria esta corrida específica; o código atual não tem esse protocolo (`bin/cron_run.sh:9`, `bin/cron_run.sh:11`, `bin/deploy.sh:299`, `bin/deploy.sh:326`). Para garantir ausência absoluta de código velho atendendo sobre schema novo, seria preciso ainda tratar o atendimento durante o intervalo migrate → publicação; viabilidade concreta de pausa ou compatibilidade de migrations nesta aplicação: **[nao verificado]**.

## 08/11 23:31 · sessao 73e022226ffe · - · - · origem=Ronald (tmux)

**Q3 — escritores e leitores**

- Escritores produtivos do evento: `_trilha_do_veto`, com `via='renascer'` ou `'reabrir'` e campos `veto`, `dia`, `modulo_origem`, `colaborador` (`app/chamados/models.py:1166`, `:1184`); `reabrir` recusado por admin sem fato novo (`:1396`), premissa regularizada (`:1402`) e universo alterado (`:1421`). Só o último escreve `depois['motivo']='universo_mudou_sob_trava'` (`:1424`); os outros dois usam `status_local`/`via_encerramento` (`:1398`, `:1404`).
- Leitor específico identificado: deduplicação de `_trilha_do_veto`, que filtra ação/modelo/objeto e **também** `modulo_origem`, `dia`, `veto`, `via` (`app/chamados/models.py:1172`). O novo evento não satisfaz essa chave; portanto não impede a trilha do veto (`:1172`, `:1424`).
- Não identifiquei leitor produtivo de `depois['motivo']` para essa ação; único leitor explícito desse valor encontrado está no teste (`app/chamados/tests/test_e1_fila.py:517`). Ausência absoluta de leitores dinâmicos: [nao verificado].
- A auditoria genérica consulta todas as ações, aceita filtro literal `acao` e conta registros, sem classificar motivos (`app/core/views.py:705`, `:709`, `:727`). O template exibe ação/descrição, sem `valor_depois` (`app/templates/core/log_auditoria.html:55`, `:68`, `:73`). Limitação preexistente: dropdown usa `LogAuditoria.ACOES`, que contém só sete ações genéricas e não oferece `reabertura_recusada` (`app/core/views.py:738`; `app/core/models.py:286`; `app/templates/core/log_auditoria.html:23`); GET com `acao=reabertura_recusada` continua aceito (`app/core/views.py:709`).
- `vigia_loop.escritas_por_cron` inclui a nova recusa quando o ator corresponde ao cron: conta todas as linhas por ator/modelo/objeto, sem filtro de ação ou motivo (`app/chamados/services/vigia_loop.py:86`). Seu rótulo já declara “escritas com trilha LogAuditoria”; portanto não demonstrou pressuposto incompatível de motivos (`:68`, `:96`).
- Os vigias de trocas de estado e trilha versus banco filtram exclusivamente `estado_chamado`, excluindo recusas (`app/chamados/services/vigia_loop.py:29`, `:114`). O histórico do colaborador também só inclui essa ação de estado (`app/chamados/services/historico.py:44`, `:86`).
- Há várias causas sob o mesmo evento, mas o sentido comum permanece “reabertura recusada”; **não há contrato homogêneo de `motivo` obrigatório** nos emissores (`app/chamados/models.py:1187`, `:1398`, `:1404`, `:1424`). Não encontrei prova de contador lendo conjunto antigo de motivos; comportamento em produção: [nao verificado].
- A tentativa de trilha é best-effort: erro na gravação retorna `None`, com log/contador de engolidos, em vez de assegurar linha persistida (`app/core/observ.py:105`, `:133`, `:139`, `:73`). Logo “recusa falante” prova chamada do emissor; persistência real: [nao verificado].

**Q5 — outra porta capaz de levantar a exceção**

- Único `raise UniversoMudouSobATrava` identificado no código produtivo é a conferência de `migrar_resolvedoras_vivas` (`app/chamados/services/fio.py:2088`, `:2091`).
- Único chamador produtivo nominal identificado de `migrar_resolvedoras_vivas` é o ramo `irmao_vivo` de `ChamadoColaborador.reabrir`, que captura essa exceção (`app/chamados/models.py:1411`, `:1415`, `:1417`).
- Não há outra porta direta identificada fora de `reabrir`; rotas por alias dinâmico/reflexão em execução: [nao verificado]. Chamadores indiretos que usam `reabrir` pertencem ao inventário da Q1 (`app/chamados/models.py:1415`).

**Observação útil à conferência de travas:** o conjunto inicial inclui `disputa_id` nas travas, mas a conferência posterior só compara colaboradores e PKs das perguntas. Mudança de `disputa_id` de uma pergunta já conhecida não aciona esse `raise`; se uma escrita concorrente produtiva pode realizar isso nas condições do ato: [nao verificado] (`app/chamados/services/fio.py:2086`, `:2089`).

## 08/11 23:30 · sessao 0f34790ec36d · - · - · origem=Ronald (tmux)

Mapa estático, sem execução: o selo cria e remove arquivos, o que conflita com só leitura (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:44`, `:107`); resultados reais de RED/VERDE: [nao verificado].

| Caso | Mutante que a asserção mata |
|---|---|
| 1 | Aplicar migrate antes da auth recusar, ou ignorar a recusa no normal: exige rc≠0, migrate=0 e HUP=0 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110-112`; porta real `bin/deploy.sh:145-147`). |
| 2 | Aplicar migrate antes da sombra recusar, ou ignorar a recusa: mesmas três exigências (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:114-116`; `bin/deploy.sh:162-165`). |
| 3 | Eliminar migrate; aplicá-lo duas vezes; colocá-lo antes de auth/sombra/casca; chamar sombra com operação diferente de `--conferir`; eliminar HUP: exige migrate=1, HUP=1 e primeira passagem dos três portões anterior ao primeiro apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:95-104`, `:118-120`). |
| 4 | Isentar o agendado da chamada à auth: exige N_JAN≥1. **Ignorar o retorno**, mantendo a chamada, pode sobreviver a este caso porque há pendência core e a guarda de agendado ainda recusa (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:122-124`; `bin/deploy.sh:211-214`). |
| 5 | Consultar auth/sombra em `--conferir`, migrar ou enviar HUP nesse modo: exige rc=0 e todas essas contagens zeradas (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:126-128`). |
| 6 | Adiar a prova de casca até depois do apply, ou ignorar sua recusa: exige falha sem migrate/HUP (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:130-132`; `bin/deploy.sh:286-287`). |
| 7 | Fazer `--sem-sombra` também dispensar auth ou migrar antes dela: exige falha sem migrate/HUP (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:134-136`). |
| 8 | Aplicar migrate incondicionalmente, inclusive sem pendência; eliminar o HUP no caminho sem pendência (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:138-140`). |
| 9 | Chamar auth no agendado e ignorar sua recusa: agora não há pendência para esconder a falta de mordida (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:142-144`). |
| 10 | Permitir que `APLICAR_MIGRATE=1` herdado determine apply em `--sem-migrate`; a fixture injeta explicitamente esse valor (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`, `:146-148`; inicialização real `bin/deploy.sh:61`). |
| 11 | Restaurar a recusa incondicional por cron na seção 2 depois do apply, como na volta anterior: exige rc=0, migrate=1 e restart=1 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:150-152`; decisão atual `bin/deploy.sh:325`). |

Mutantes plausíveis sobreviventes, por inspeção das fixtures e asserções; execução dos mutantes: [nao verificado].

- **Remover as recusas por cron antes do migrate**, nas linhas 117/299: nenhum caso instala cron previamente; o único cron falso nasce durante o apply (`bin/deploy.sh:117`, `:299`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `:150`).
- **Remover a guarda de migration pendente da mensageria**: o falso sempre responde sucesso a esse check (`bin/deploy.sh:200-202`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:69`).
- **Dispensar sombra/auth no `--reload-copia`**: nenhum dos 11 cenários usa esse modo (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110-152`; condições reais `bin/deploy.sh:126`, `:151`).
- **Adicionar uma segunda consulta a auth/sombra/casca depois do migrate**: os retornos dos falsos não mudam durante uma corrida; casos vermelhos já encerraram antes, casos verdes continuam verdes. Portanto não detectam um portão que muda de resposta com o tempo (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:54-56`, `:90`).
- **Reiniciar somente `saas_core`**, omitindo as outras cascas: N_RST conta chamadas, não os alvos; restart falso sempre retorna 0, e readiness/rotas também ficam verdes (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:70-76`, `:101`, `:151`; comando real `bin/deploy.sh:326`).
- **Eliminar provas de troca dos workers, readiness ou rotas**: nenhuma asserção exige essas chamadas; as fixtures já produzem troca de PID e STATUS 200 no caminho verde (`bin/deploy.sh:344-356`, `:368-370`, `:399-402`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:70`, `:73-75`, `:110-152`).
- **Ignorar falha do apply ou do restart**: o docker falso não oferece retorno de falha nessas operações, logo os respectivos `|| falhou` não são exercitados (`bin/deploy.sh:300`, `:326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `:76`).
- **Eliminar ou deslocar selo_reciclagem**: a fixture o substitui por `exit 0`; este selo não verifica a posição nem a mordida do selo real (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:51-53`; `bin/deploy.sh:409`).

O caso 11 **não prova cron morto nem cron refeito**: apenas cria `cron_falso.1`, conta uma chamada de restart e exige rc=0; o falso não executa nem reexecuta cron (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `:76`, `:101`, `:150-152`).

## 08/11 23:18 · sessao 6fa4aad98c43 · deployportao · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 08/11 23:15 · sessao 7f6b12750a43 · - · - · origem=Ronald (tmux)

Análise estática; não executei selos ou mutantes, pois criam/removem temporários. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:36`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:96`, `bin/tests/test_grep_q_sem_sigpipe.sh:30`)

| Caso | O que efetivamente morde e limite |
|---|---|
| 1 | Normal com pendência/auth recusando: exige falha, zero apply e zero HUP. Detecta apply anterior à auth nesse caminho. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:99`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:100`) |
| 2 | Mesma proteção contra sombra recusando no normal. O falso aceita qualquer argumento; quem exige `--conferir` é a medição de ordem do caso 3. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:47`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:87`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:104`) |
| 3 | Exige exatamente um apply, auth/sombra `--conferir`/casca anteriores a ele, sucesso e um HUP. Não exige ordem relativa entre os três portões. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:82`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:85`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:92`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:108`) |
| 4 | Agora confirma chamada à auth, zero apply/HUP e falha, mas **não prova respeito ao retorno da auth**: com core pendente, a guarda de agendado também recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:112`, `bin/deploy.sh:208`) |
| 5 | Exige sucesso, zero apply/HUP e ausência de chamada à sombra. Não exige ausência de chamada à auth: uma chamada cujo resultado seja ignorado passaria. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:115`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:116`) |
| 6 | Detecta apply normal anterior à casca recusando, bem como simples remoção da casca que deixe o deploy prosseguir. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:120`) |
| 7 | Protege auth antes do apply em `--sem-sombra`; não testa casca nesse modo nem um deploy verde nele. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:123`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:124`) |

Mutantes que passam, por inferência estática; execução dos mutantes: [nao verificado].

- Repor apply na seção 1 **somente em `--sem-sombra`**, conservando apply tardio no normal: caso 7 recusa antes de alcançar a seção 1; os outros casos não exercitam esse apply antecipado. O mesmo vale para `--restart`, ausente da matriz. (`bin/deploy.sh:142`, `bin/deploy.sh:201`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:99`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123`)
- Chamar auth em agendado e ignorar seu retorno: caso 4 continua vermelho pelo core pendente, com `N_JAN>=1`. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:112`, `bin/deploy.sh:208`)
- Remover o escritor `APLICAR_MIGRATE=1` e executar apply incondicional após a casca: todos os casos que chegam ao apply usam pendência; nenhum caso verde usa `PEND=0`. (`bin/deploy.sh:220`, `bin/deploy.sh:292`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:99`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:107`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123`)
- Apenas remover o escritor normalmente falha no caso 3; **com `APLICAR_MIGRATE=1` herdado**, pode passar toda a matriz: o selo não limpa essa variável, e os casos negativos encerram antes do leitor tardio. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:80`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:108`, `bin/deploy.sh:292`)

Reproduzi somente a varredura textual de `achados`, sem temporários: retornou zero linhas no conjunto efetivamente selecionado. Assim, não encontrei falso positivo efetivo atual; a regex crua casa a string legítima da própria fixture, mas o selo exclui seu próprio basename. (`bin/tests/test_grep_q_sem_sigpipe.sh:15`, `bin/tests/test_grep_q_sem_sigpipe.sh:19`, `bin/tests/test_grep_q_sem_sigpipe.sh:21`, `bin/tests/test_grep_q_sem_sigpipe.sh:25`, `bin/tests/test_grep_q_sem_sigpipe.sh:31`)

A autoprova passou a exigir os dois resultados, mas não demonstra determinismo universal: gera aproximadamente 1,29 MB com `ALVO` no início e não mede capacidade do pipe nem comportamento de leitura do `grep`. Se capacidade mais leitura inicial forem menores que a saída, o produtor não consegue terminar antes do fechamento: agendamento, sozinho, não basta para fazê-lo acertar. Premissas das implementações/host real e intermitência em produção: [nao verificado]. (`bin/tests/test_grep_q_sem_sigpipe.sh:37`, `bin/tests/test_grep_q_sem_sigpipe.sh:39`, `bin/tests/test_grep_q_sem_sigpipe.sh:44`)

Variáveis relevantes da alteração nos selos:

| Variável | Escritor | Leitor |
|---|---|---|
| `FAKE_CASCA` | Caso 6. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`) | Script falso gerado. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:48`) |
| `lc` | Posição da casca no log. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:88`) | Cálculo de ordem. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:92`) |
| `N_JAN` | Contagem do log. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:89`) | Caso 4. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:112`) |
| `N_SOM` | Contagem do log. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`) | Caso 5. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:116`) |
| `ORDEM_OK` | Inicialização e comparação. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:91`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:93`) | Diagnóstico/caso 3. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:94`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:108`) |
| `RX` | Literal da regex. (`bin/tests/test_grep_q_sem_sigpipe.sh:15`) | `grep -nE`. (`bin/tests/test_grep_q_sem_sigpipe.sh:21`) |
| `N` | Contagem dos achados da fixture. (`bin/tests/test_grep_q_sem_sigpipe.sh:32`) | Expectativa de três variantes. (`bin/tests/test_grep_q_sem_sigpipe.sh:33`) |
| `X` | `printf` + `seq`. (`bin/tests/test_grep_q_sem_sigpipe.sh:37`) | Pipeline e here-string. (`bin/tests/test_grep_q_sem_sigpipe.sh:39`, `bin/tests/test_grep_q_sem_sigpipe.sh:40`) |
| `r_pipe`, `r_here` | Ramos dos respectivos testes. (`bin/tests/test_grep_q_sem_sigpipe.sh:39`, `bin/tests/test_grep_q_sem_sigpipe.sh:40`) | Veredito e diagnóstico. (`bin/tests/test_grep_q_sem_sigpipe.sh:44`, `bin/tests/test_grep_q_sem_sigpipe.sh:45`) |

## 08/11 23:14 · sessao eca57893509f · - · - · origem=Ronald (tmux)

**Q2.** `APLICAR_MIGRATE` tem um escritor interno, `APLICAR_MIGRATE=1`, no ramo core pendente e elegível, e um leitor, no bloco 1c; não é zerada junto com os demais flags do parser. Portanto, `export APLICAR_MIGRATE=1` chega ao apply quando `PEND=0`, inclusive em `--sem-migrate`, `--reload-agendado` e `--reload-copia`. Antes, sem pendência, esses caminhos não executavam o apply da seção 1. (`bin/deploy.sh:56`, `bin/deploy.sh:67`, `bin/deploy.sh:78`, `bin/deploy.sh:201`, `bin/deploy.sh:220`, `bin/deploy.sh:292`, `bin/deploy.sh:294`)

Isso **não contorna as recusas existentes com `PEND>0`**: cópia, agendado e sem-migrate continuam encerrando antes do bloco 1c, mesmo com a variável herdada. `--conferir` também encerra antes dele. Não há, por esse vazamento, modo que passe com a mesma pendência que antes provocava recusa; há apply indevido em caminhos antes aceitos **sem apply**. (`bin/deploy.sh:205`, `bin/deploy.sh:208`, `bin/deploy.sh:212`, `bin/deploy.sh:224`, `bin/deploy.sh:226`, `bin/deploy.sh:292`)

Com ambiente limpo e portões verdes, a elegibilidade permanece normal/sem-sombra/restart. Nesses modos, casca vermelha agora impede o apply que antes a antecedia, conforme o diff fornecido; no restart, a nova verificação de cron também pode impedir o apply. (`bin/deploy.sh:220`, `bin/deploy.sh:283`, `bin/deploy.sh:293`; anexo `codex_deployportao2.diff`, localização original removida **[nao verificado]**)

**Q4.** São **três** chamadas se o restart aplicar core: primeira em `114`, nova em `293`, e a preservada em `318`. Sem apply são duas. A chamada em `318` ainda pode recusar depois do apply em `294`, se um cron começar nesse intervalo. (`bin/deploy.sh:114`, `bin/deploy.sh:292`, `bin/deploy.sh:293`, `bin/deploy.sh:294`, `bin/deploy.sh:318`)

A rotina lista marcas com PID presente em `/proc` e remove marcas cujo PID não existe; `vivos` é local. Repetir a remoção de uma marca já ausente é inócuo por `rm -f`; não há contador nem append duplicado. Mas cada chamada observa novamente o estado e pode encontrar novos crons: o envelope cria a marca antes do comando e a remove por trap. Não existe trava que reserve a janela entre a verificação e o restart. Ocorrência dessa corrida em produção: **[nao verificado]**. (`bin/deploy.sh:105`, `bin/deploy.sh:106`, `bin/deploy.sh:108`, `bin/deploy.sh:109`, `bin/cron_run.sh:9`, `bin/cron_run.sh:10`, `bin/cron_run.sh:11`)

Mapa completo enviado nas mensagens anteriores, incluindo variáveis de ambos os selos e campos dos logs/fixtures.

## 08/11 23:14 · sessao 5594ec55ebdb · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 08/11 23:08 · sessao 6ea0bd9fb4d7 · BUG-DEPLOY-PORTAO · verificador · origem=Code

**1. Auth e sombra agora antecedem o migrate, mas ainda há efeitos antes de portões e recusas depois de alterações no banco.** A sequência efetiva é esta. (`bin/deploy.sh:123`, `bin/deploy.sh:159`, `bin/deploy.sh:217`, `bin/deploy.sh:280`, `bin/deploy.sh:306`)

| Ordem | Passo e efeito | Posição dos portões |
|---|---|---|
| 1 | `arvore_sem_conflito`: lê índice e arquivos; pode encerrar qualquer modo. | Primeiro portão, antes dos demais passos abaixo. (`bin/deploy.sh:98`, `bin/arvore_sem_conflito.sh:19`, `bin/arvore_sem_conflito.sh:29`) |
| 2 | Só `--restart`: verifica crons e **apaga marcas de PID morto**. | Depois da árvore; antes de auth, sombra e pendências. A própria verificação já modifica arquivos. (`bin/deploy.sh:105`, `bin/deploy.sh:106`, `bin/deploy.sh:114`) |
| 3 | Auth: chama `janela_auth.sh` ou anuncia a dispensa por motivo. | Antes de sombra, consultas de pendências e migrate; omitido em `--conferir`. (`bin/deploy.sh:123`, `bin/deploy.sh:140`, `bin/deploy.sh:142`) |
| 4 | Sombra: chama `--conferir` ou **apenda `deploy_sem_sombra.log`**. O helper real executa também **`mkdir -p logs/sombra`**, antes de validar o carimbo. | Depois de auth; antes das recusas por mensageria/core pendentes. Agendado puro é isento; cópia não. (`bin/deploy.sh:148`, `bin/deploy.sh:157`, `bin/deploy.sh:159`, `bin/sombra.sh:84`, `bin/sombra.sh:410`) |
| 5 | Executa os dois `--check`; qualquer resposta não zero vira pendência. | Mensageria pendente recusa antes de aplicar migration, exceto em conferir/restart. (`bin/deploy.sh:181`, `bin/deploy.sh:191`, `bin/deploy.sh:197`) |
| 6 | Core pendente: executa `--plan`; conferir informa; cópia, agendado e sem-migrate recusam; demais **aplicam `migrate_schemas --tenant`**. | Árvore, primeira recusa do restart, auth, sombra e guarda de mensageria já foram tratados conforme o modo. (`bin/deploy.sh:202`, `bin/deploy.sh:203`, `bin/deploy.sh:205`, `bin/deploy.sh:208`, `bin/deploy.sh:212`, `bin/deploy.sh:217`) |
| 7 | Conferir encerra. Nos demais, `collectstatic` pode **reescrever estáticos/manifest**; falha apenas avisa. | Depois das pendências; antes da prova de casca. (`bin/deploy.sh:221`, `bin/deploy.sh:258`, `bin/deploy.sh:260`) |
| 8 | Prova de casca: **cria temporário, cria/remove container efêmero e apaga temporário**; pode recusar. App montado somente leitura, logs em tmpfs, rede desativada. | **Depois do migrate e collectstatic**, antes da publicação. Pode ser dispensada por `SEM_PROVA_DE_CASCA`. (`bin/deploy.sh:276`, `bin/deploy.sh:280`, `bin/prova_de_casca.sh:28`, `bin/prova_de_casca.sh:30`, `bin/prova_de_casca.sh:34`, `bin/prova_de_casca.sh:46`) |
| 9 | Restart: **repete a recusa por cron**, podendo novamente apagar marcas, e executa **`docker restart`**. Demais modos enviam **HUP aos três containers** e aguardam troca de workers. | A segunda recusa pode acontecer **depois de migrar**. Falha de HUP/troca também ocorre depois das escritas anteriores. (`bin/deploy.sh:306`, `bin/deploy.sh:307`, `bin/deploy.sh:324`, `bin/deploy.sh:337`) |
| 10 | O comando declarado para o boot do mensageria **aplica `migrate --noinput`**. | Efeito do ramo restart segundo o compose; configuração efetiva do container vivo: **[nao verificado]**. (`docker-compose.mensageria.yml:20`) |
| 11 | Espera os containers e executa `Client.get` em três rotas. Resposta inadequada encerra o deploy. | **Depois da publicação**. Escrita de negócio por essas requisições: **[nao verificado]**. (`bin/deploy.sh:346`, `bin/deploy.sh:362`, `bin/deploy.sh:380`, `bin/deploy.sh:383`) |
| 12 | Selo de reciclagem ainda pode recusar; contador de ImportError lê log. | Ambos depois da publicação; o contador não é portão. (`bin/deploy.sh:390`, `bin/deploy.sh:393`, `bin/importerror_na_janela.sh:13`, `bin/importerror_na_janela.sh:32`) |
| 13 | **Trunca/escreve `deploy.stamp.novo`, substitui `deploy.stamp`, cria diretório e apenda histórico**. | Depois das provas e selo. Histórico exclui agendado puro e prova de casca dispensada; stamp não tem essas exclusões. (`bin/deploy.sh:396`, `bin/deploy.sh:400`, `bin/deploy.sh:404`, `bin/deploy.sh:405`, `bin/deploy.sh:406`) |

Portanto, **sim: permanece alteração de schema/dados antes da prova de casca e da segunda recusa do restart**, sem compensação correspondente nesses ramos de falha. O HUP/restart também antecede os portões de rotas e reciclagem. A cura resolve especificamente a recusa tardia de auth/sombra; não estabelece atomicidade integral. (`bin/deploy.sh:217`, `bin/deploy.sh:281`, `bin/deploy.sh:306`, `bin/deploy.sh:324`, `bin/deploy.sh:383`, `bin/deploy.sh:390`)

“Antes de qualquer escrita” também é literalmente excessivo pelos arquivos acima. Além disso, comandos Django podem carregar settings que criam `/app/logs` e configuram arquivos de log; ocorrência efetiva dessas escritas neste ambiente: **[nao verificado]**. (`bin/deploy.sh:121`, `app/manage.py:9`, `app/config/settings/saas.py:118`, `app/config/settings/saas.py:139`, `app/config/settings/saas.py:163`)

**2. A política de inclusão dos modos foi preservada; os caminhos que recusam antecipadamente mudaram.** O parser considera somente `$1` como modo. (`bin/deploy.sh:56`, `bin/deploy.sh:60`, `bin/deploy.sh:79`, `bin/deploy.sh:83`)

| Modo | Auth, salvo motivo de dispensa | Sombra |
|---|---|---|
| Normal | Exigida | Exigida, salvo motivo herdado. (`bin/deploy.sh:140`, `bin/deploy.sh:148`) |
| `--conferir` | Não executa | Não executa. O fluxo anterior também encerrava antes do bloco removido, conforme o diff. (`bin/deploy.sh:123`, `bin/deploy.sh:221`) |
| `--sem-migrate` | Exigida | Exigida, salvo motivo herdado. (`bin/deploy.sh:61`, `bin/deploy.sh:140`, `bin/deploy.sh:148`) |
| `--sem-sombra` | Exigida | Dispensada com motivo obrigatório e tentativa de registro. (`bin/deploy.sh:83`, `bin/deploy.sh:85`, `bin/deploy.sh:140`, `bin/deploy.sh:157`) |
| `--reload-agendado` | Exigida | Isento. (`bin/deploy.sh:62`, `bin/deploy.sh:140`, `bin/deploy.sh:148`) |
| `--reload-copia` | Exigida | Exigida, salvo motivo herdado. (`bin/deploy.sh:79`, `bin/deploy.sh:140`, `bin/deploy.sh:148`) |
| `--restart` | Exigida | Exigida, salvo motivo herdado. (`bin/deploy.sh:69`, `bin/deploy.sh:140`, `bin/deploy.sh:148`) |

A resposta literal a “antes não passava e agora passa” é **sim, em caminhos recusados**: antes, mensageria pendente ou core pendente em sem-migrate/agendado/cópia encerravam antes de alcançar auth/sombra. Agora executam os portões aplicáveis primeiro. Isso altera a primeira causa/mensagem de falha e pode deixar registro de dispensa de sombra em uma tentativa posteriormente recusada ou cujo migrate falhe. (`bin/deploy.sh:140`, `bin/deploy.sh:158`, `bin/deploy.sh:197`, `bin/deploy.sh:205`, `bin/deploy.sh:208`, `bin/deploy.sh:212`, `bin/deploy.sh:217`)

**3. Os chamadores identificados são os seguintes; instalação e execução reais em produção permanecem [nao verificado].**

| Origem | Chamada e dependência observável |
|---|---|
| Cron declarado | 03:30, `--reload-agendado`, com saída em log. (`app/config/crons.py:326`) |
| Integrador | Wrapper chama o lote; lote faz `--reload-copia` após copiar, descartando seu retorno. O deploy final `--sem-sombra` tem retorno conferido antes de marcar FIM/NO_AR. (`bin/integrador.sh:52`, `app/core/integrador_lote.py:627`, `app/core/integrador_lote.py:638`, `app/core/integrador_lote.py:753`, `app/core/integrador_lote.py:754`, `app/core/integrador_lote.py:763`) |
| Molde de fatia | `--reload-copia` após desfazer e copiar; final `--sem-sombra`. Reconhece sucesso final pelo texto `^deploy: OK`. (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_fatia/cadeia.sh:89`, `bin/molde_fatia/cadeia.sh:90`) |
| Molde autocura | Mesmas três chamadas e reconhecimento textual. (`bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:45`, `bin/molde_autocura/cadeia.sh:86`, `bin/molde_autocura/cadeia.sh:87`) |
| Vigia de logins | Após revert, chama `--sem-migrate`; falha é anunciada. (`bin/vigia_logins.sh:87`) |
| Vigia da esteira | Auto-revert e revert do revert chamam `--sem-sombra`, descartando os retornos dos deploys. (`app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1275`) |
| Fatia abono | Deploy normal, depois backfill; verifica falha do pipeline do deploy. (`fatias_agendadas/abono-no-ar/esteira.sh:37`, `fatias_agendadas/abono-no-ar/esteira.sh:45`) |
| Fatia o135 | Usa `--conferir`; após merge usa `--sem-migrate` e guarda seu retorno. (`fatias_agendadas/o135-teto20/esteira.sh:80`, `fatias_agendadas/o135-teto20/esteira.sh:116`, `fatias_agendadas/o135-teto20/esteira.sh:117`) |
| Agendador genérico | Delega à `esteira.sh` da fatia. (`bin/deploy_agendado.sh:73`) |
| Fabricante | Declara que não publica e proíbe deploy; comportamento indireto de conteúdo gerado: **[nao verificado]**. (`bin/fabricante.sh:8`, `bin/fabricante.sh:17`, `bin/fabricante_prompt.md:70`) |
| Selos | Os dois testes comportamentais executam uma cópia do deploy contra docker falso. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:34`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`, `bin/tests/test_deploy_reload_gracioso.sh:36`, `bin/tests/test_deploy_reload_gracioso.sh:95`) |

Os consumos observados são retorno final, mensagem final ou descarte do retorno; não há nesses pontos uma espera por “migrate antes do carimbo”. Dependência externa ou dinâmica dessa ordem: **[nao verificado]**. (`app/core/integrador_lote.py:754`, `bin/molde_fatia/cadeia.sh:90`, `app/core/esteira_vigia.py:1267`)

**4. O selo morde a regressão principal no modo normal, mas não demonstra sua promessa universal.** Não executei os selos: eles criam e removem arquivos temporários. Os resultados abaixo são análise estática, não execução de mutantes. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:31`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:34`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:86`)

| Caso | O que efetivamente cobra |
|---|---|
| 1 — auth recusa | Normal, core pendente: exige rc não zero, zero migrate contado e zero HUP. Mover auth para depois do migrate normal quebra essa expectativa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:89`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`) |
| 2 — sombra recusa | Mesma cobrança para sombra no normal. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:93`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:94`) |
| 3 — verde | Exige um migrate, um HUP e registro dos dois portões antes do migrate. É a proteção contra passar apenas por falha antecipada. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:79`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:83`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:98`) |
| 4 — agendado | Exige apenas rc não zero e zero HUP. Não põe core pendente, não exige ausência de migrate nem confirma que auth causou a recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:101`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:102`) |
| 5 — conferir | Exige sucesso sem migrate/HUP mesmo com auth recusando. Não verifica ausência de chamada à sombra. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:105`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:106`) |

Há mutantes que a matriz deixa escapar, por inferência estática:

- **Antecipar os portões somente no normal e manter o bloco antigo nos demais modos.** Os três primeiros casos passam; o agendado ainda recusa auth sem HUP; conferir continua saindo antes. A ordem continua desprotegida em sem-sombra/restart/cópia. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:89`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:93`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:101`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:105`)
- **Trocar `sombra --conferir` por outra operação.** O falso aceita qualquer argumento; a medição de ordem procura somente `PORTAO sombra`. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:42`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:81`)
- **Acrescentar outro efeito não contado**, como restart antes do portão: o falso aceita comandos desconhecidos, enquanto as asserções contam somente a forma específica de migrate e HUP. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:60`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:77`)

Há ainda contaminação possível pelo ambiente: o selo limpa `SEM_SOMBRA_MOTIVO`, mas o deploy imediatamente a sobrescreve usando `DEPLOY_SEM_SOMBRA`. Um valor herdado nessa segunda variável dispensa o falso portão e pode produzir vermelho com a cura correta. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`, `bin/deploy.sh:82`, `bin/deploy.sh:149`)

**5. O diff contém 19 substituições, em 18 linhas — não 17.**

| Grupo | Quantidade e valores |
|---|---|
| Atomicidade | 2: `M`, `C`, saídas do varredor sobre fixtures. (`bin/tests/test_atomicidade_da_porta.sh:55`, `bin/tests/test_atomicidade_da_porta.sh:56`, `bin/tests/test_atomicidade_da_porta.sh:83`, `bin/tests/test_atomicidade_da_porta.sh:84`) |
| Import tardio | 2: `OUT`, `MORDE`, stdout/stderr do Python. (`bin/tests/test_import_tardio_contra_o_ar.sh:23`, `bin/tests/test_import_tardio_contra_o_ar.sh:25`, `bin/tests/test_import_tardio_contra_o_ar.sh:37`, `bin/tests/test_import_tardio_contra_o_ar.sh:38`) |
| Inventário | 2: `OUT`, `MORDE`, saída de `_rodar`. (`bin/tests/test_inventario_pessoal_no_commit.sh:31`, `bin/tests/test_inventario_pessoal_no_commit.sh:33`, `bin/tests/test_inventario_pessoal_no_commit.sh:61`, `bin/tests/test_inventario_pessoal_no_commit.sh:62`) |
| Akita | 3: `PADRAO_OUT` e duas consultas a `MORDE_OUT` na mesma linha. (`bin/tests/test_lei_akita.sh:730`, `bin/tests/test_lei_akita.sh:734`) |
| Raia | 10: cinco consultas a `SEM`, cinco a `COM`, ambos JSON. (`bin/tests/test_raia_declarada_no_estado.sh:63`, `bin/tests/test_raia_declarada_no_estado.sh:69`, `bin/tests/test_raia_declarada_no_estado.sh:79`) |

Nas saídas normais declaradas pelos produtores, não identifiquei valor que seja uma opção isolada de `echo`, como `-n`/`-e`: há prefixos de diagnóstico, marcadores ou JSON. Sob `echo` sem interpretação de escapes, ambos os caminhos acrescentam a quebra final ao valor capturado; a mudança pretendida é eliminar o produtor que pode receber SIGPIPE. Configuração efetiva de `echo` no ambiente de produção: **[nao verificado]**. (`bin/atomicidade_da_porta.py:97`, `bin/import_tardio_contra_o_ar.py:204`, `bin/inventario_pessoal_dos_modelos.py:85`, `bin/tests/test_lei_akita.sh:719`, `bin/tests/test_lei_akita.sh:725`, `bin/tests/test_raia_declarada_no_estado.sh:63`)

**Não há equivalência universal de bytes:** `SEM`/`COM` contêm escapes JSON `\n`, pois `linha()` produz texto multilinha. Um `echo` configurado para expandir escapes transforma esse conteúdo; a here-string o preserva. Mudança adicional de veredito nos casos concretos fornecidos: **[nao verificado]**. (`bin/raias_estado.py:153`, `bin/tests/test_raia_declarada_no_estado.sh:65`, `bin/tests/test_raia_declarada_no_estado.sh:69`, `bin/tests/test_raia_declarada_no_estado.sh:79`)

**Sobraram pipelines da mesma classe sob `pipefail`:**

- `printf ... | grep -qxF` na régua de tickets. (`bin/regua_tickets.sh:15`, `bin/regua_tickets.sh:87`)
- Quatro `printf ... | grep -q...` no selo da suite. (`bin/tests/test_suite_sh.sh:14`, `bin/tests/test_suite_sh.sh:51`, `bin/tests/test_suite_sh.sh:53`, `bin/tests/test_suite_sh.sh:55`, `bin/tests/test_suite_sh.sh:57`)
- `printf | grep | grep -q` no selo da sonda. (`bin/tests/test_codex_sonda.sh:9`, `bin/tests/test_codex_sonda.sh:91`)
- Função produtora com `git show | grep -q`. (`bin/tests/test_codex_estado_evento.sh:9`, `bin/tests/test_codex_estado_evento.sh:57`, `bin/tests/test_codex_estado_evento.sh:62`)
- Função produtora com `cat | grep -q`; nesses ramos, falha do pipeline pode anunciar “negado” apesar de encontrar o marcador. Isso é inferência do encadeamento, não reprodução dinâmica. (`bin/tests/prova_codex_pergunta_real.sh:15`, `bin/tests/prova_codex_pergunta_real.sh:142`, `bin/tests/prova_codex_pergunta_real.sh:144`, `bin/tests/prova_codex_pergunta_real.sh:147`, `bin/tests/prova_codex_pergunta_real.sh:152`)

O `grep -c` remanescente de Akita consome toda a entrada: isoladamente não tem o encerramento antecipado de `-q`. O diagnóstico com `head -2` ainda pode cortar o produtor, mas está na mensagem de uma falha já decidida. (`bin/tests/test_lei_akita.sh:729`, `bin/tests/test_lei_akita.sh:734`, `bin/tests/test_lei_akita.sh:735`)

**6. Sim: o selo textual admite falsos positivos e negativos.** A regex exige a forma estreita `echo "$NOME" | grep -...q`; a seleção de arquivos exige somente que o texto contenha `pipefail`. (`bin/tests/test_grep_q_sem_sigpipe.sh:14`, `bin/tests/test_grep_q_sem_sigpipe.sh:19`)

- **Falsos negativos:** nomes com dígitos, `${OUT}`, espaçamento diferente, continuação de linha, caminho explícito para grep, `--quiet`, produtores `printf`/`cat`/funções, subdiretórios e pipefail herdado sem essa palavra no arquivo. Os pipelines reais da resposta 5 ilustram essa lacuna. (`bin/tests/test_grep_q_sem_sigpipe.sh:14`, `bin/tests/test_grep_q_sem_sigpipe.sh:19`, `bin/tests/test_grep_q_sem_sigpipe.sh:24`, `bin/regua_tickets.sh:87`, `bin/tests/prova_codex_pergunta_real.sh:144`)
- **Falsos positivos:** `pipefail` apenas em comentário ou desativado; forma canônica dentro de string/heredoc/comentário inline. O filtro exclui somente comentários de linha inteira e não interpreta shell. (`bin/tests/test_grep_q_sem_sigpipe.sh:19`, `bin/tests/test_grep_q_sem_sigpipe.sh:20`)
- **Autoprova incompleta:** MORDE 1 cobre apenas a forma canônica. MORDE 2 calcula `r_pipe`, mas não exige que seja zero: fica verde sempre que `r_here=1`, mesmo se o pipeline antigo também acertar. (`bin/tests/test_grep_q_sem_sigpipe.sh:30`, `bin/tests/test_grep_q_sem_sigpipe.sh:31`, `bin/tests/test_grep_q_sem_sigpipe.sh:37`, `bin/tests/test_grep_q_sem_sigpipe.sh:40`)

**7. Os leitores efetivos da chave são genéricos, pelo conjunto `ISENTOS`.** Inventário de modelos e selo SQL montam a cobertura com `set(R.ISENTOS)` e comparam `tabela.coluna`; portanto passam a reconhecer a chave nova. (`bin/sombra_regras_pessoais.py:230`, `bin/inventario_pessoal_dos_modelos.py:52`, `bin/inventario_pessoal_dos_modelos.py:78`, `bin/sombra_selo_pessoal.py:25`, `bin/sombra_selo_pessoal.py:34`)

Não encontrei consumidor literal da chave antiga em `bin/`/`app/`; consumidores por nomes construídos dinamicamente: **[nao verificado]**. A ocorrência literal remanescente de `chamados_respostaanterior` no instrumento é comentário sobre o incidente. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:9`)

O **modelo antigo** permanece na criação histórica, no `RenameModel` e em um teste AST que vigia os dois nomes; isso não equivale a código corrente consultando a tabela antiga. A troca da isenção acompanha o rename declarado. (`app/chamados/migrations/0050_resposta_anterior.py:17`, `app/chamados/migrations/0051_decisao_pergunta.py:41`, `app/chamados/tests/test_reabrir_nao_destroi.py:129`, `bin/sombra_regras_pessoais.py:230`)

**8. Escritores e leitores dos estados pedidos:**

| Estado | Quem escreve | Quem lê |
|---|---|---|
| `logs/deploy.stamp` | Deploy escreve `QUANDO`, `COMMIT`, `TITULO` em `.novo` e faz `mv`, usando caminho absoluto de produção. (`bin/deploy.sh:397`, `bin/deploy.sh:398`, `bin/deploy.sh:399`, `bin/deploy.sh:400`) | Janela lê `COMMIT`, com override de caminho; import tardio lê `COMMIT`, salvo override; handoff lê `QUANDO`/`COMMIT`; vigia lê os três campos para atribuição/quarentena, auto-revert e diagnóstico de relógio. (`bin/janela_auth.sh:36`, `bin/janela_auth.sh:39`, `bin/import_tardio_contra_o_ar.py:46`, `bin/import_tardio_contra_o_ar.py:50`, `bin/handoff_sessao.sh:254`, `bin/handoff_sessao.sh:255`, `app/core/esteira_vigia.py:990`, `app/core/esteira_vigia.py:1160`, `app/

[truncado em 20000 de 22621 caracteres]

## 08/11 23:03 · sessao 351fb2a58f19 · - · - · origem=Ronald (tmux)

Q5 — há **19 substituições em 18 linhas**, pelos sítios atuais: atomicidade 2; import tardio 2; inventário 2; lei Akita 3; raias 10. A linha 734 da lei Akita contém duas substituições. (`bin/tests/test_atomicidade_da_porta.sh:56,84`; `bin/tests/test_import_tardio_contra_o_ar.sh:25,38`; `bin/tests/test_inventario_pessoal_no_commit.sh:33,62`; `bin/tests/test_lei_akita.sh:730,734`; `bin/tests/test_raia_declarada_no_estado.sh:69-73,79-83`)

Proveniência e equivalência:

- `M` e `C` são saídas do varredor Python sobre fixtures controlados; a primeira linha tem prefixo literal `select_for_update:`. Não podem ser apenas opções `-n`/`-e` nesse caminho normal. (`bin/tests/test_atomicidade_da_porta.sh:43-55,64-83`; `bin/atomicidade_da_porta.py:97-103`)
- `OUT`/`MORDE` do import vêm do Python, com stderr incluído; os caminhos normais imprimem prefixos `IMPORT_TARDIO`. (`bin/tests/test_import_tardio_contra_o_ar.sh:23,37`; `bin/import_tardio_contra_o_ar.py:153,204-217`)
- `OUT`/`MORDE` do inventário vêm de `docker run`, juntando stderr e stdout; o programa imprime `INVENTARIO_MODELOS`. Conteúdo adicional emitido pelo Docker em produção: **[nao verificado]**. (`bin/tests/test_inventario_pessoal_no_commit.sh:23-31,61`; `bin/inventario_pessoal_dos_modelos.py:48,58,85-93`)
- `PADRAO_OUT`/`MORDE_OUT` vêm do Python embutido, que imprime marcadores literais `PADRAO_*`/`MORDE_*`. (`bin/tests/test_lei_akita.sh:719-734`)
- `SEM`/`COM` vêm de `json.dumps(...)` sobre a fixture controlada. **Esses valores contêm barras invertidas reais**, pois o campo `linha` recebe texto multilinha e JSON representa as quebras como `\n`. (`bin/tests/test_raia_declarada_no_estado.sh:57-68,76-78`; `bin/raias_estado.py:153-154`)

Com `echo` Bash usual, sem expansão de escapes, a troca preserva os bytes relevantes: os dois caminhos acrescentam uma quebra final ao valor já capturado por substituição de comando; a diferença intencional é eliminar o produtor sujeito a SIGPIPE. Nenhum desses arquivos configura `xpg_echo`; configuração herdada de produção: **[nao verificado]**. Se `xpg_echo` estiver habilitado, o `echo` antigo pode transformar `\n`, `\c` etc., enquanto a here-string preserva o conteúdo literal; logo equivalência absoluta de fluxo não está demonstrada, sobretudo nos JSONs. (`bin/tests/test_raia_declarada_no_estado.sh:63-73,79-83`; `bin/tests/test_atomicidade_da_porta.sh:15,55-56`; `bin/tests/test_import_tardio_contra_o_ar.sh:20,23-25`; `bin/tests/test_inventario_pessoal_no_commit.sh:17,31-33`; `bin/tests/test_lei_akita.sh:16,727-734`)

Sobraram membros reais da mesma classe:

| Local | Pipeline restante |
|---|---|
| `bin/regua_tickets.sh:15,87` | `printf "$DECLARADOS" \| grep -qxF` sob pipefail |
| `bin/tests/test_suite_sh.sh:14,51-57` | Quatro `printf "$CORPO" \| grep -q...` |
| `bin/tests/test_codex_sonda.sh:9,91` | `printf \| grep ... \| grep -q` |
| `bin/tests/test_codex_estado_evento.sh:9,57,62` | Função que executa `git show`, canalizada para `grep -q` |
| `bin/tests/prova_codex_pergunta_real.sh:15,142-152` | `_out(){ cat ...; }` canalizado para `grep -q` |

No último exemplo, SIGPIPE no `cat` pode fazer um padrão presente cair no ramo que anuncia acesso “negado”; é inferência do encadeamento `&& ... || ...`, sem reprodução dinâmica. (`bin/tests/prova_codex_pergunta_real.sh:142-152`)

O `echo "$PADRAO_OUT" | grep -c` restante consome a entrada inteira, portanto não é o mesmo problema de encerramento antecipado; o diagnóstico com `head -2` pode cortar o produtor, mas não decide o resultado do selo. (`bin/tests/test_lei_akita.sh:729,735`)

Q6 — o selo é uma busca textual estreita, não um detector geral de SIGPIPE. A expressão só reconhece `echo "$NOME"` com letras/sublinhado, espaçamento exato e `grep -...q`; procura apenas nos dois níveis listados. (`bin/tests/test_grep_q_sem_sigpipe.sh:14,24`)

**Falsos negativos:** `$OUT2`, `${OUT}`, espaçamento alternativo, continuação de linha, `/bin/grep`, `--quiet`, `printf`, `cat`, funções produtoras e scripts em subdiretórios escapam; pipefail herdado ou ativado por `source` sem a palavra no arquivo também escapa. Os exemplos reais acima passam pela regex. (`bin/tests/test_grep_q_sem_sigpipe.sh:14,19,24`; `bin/regua_tickets.sh:87`; `bin/tests/prova_codex_pergunta_real.sh:142-152`)

**Falsos positivos:** basta a palavra `pipefail` em comentário ou em comando que o desative; só comentários de linha inteira são retirados. Uma ocorrência canônica dentro de string, heredoc ou comentário inline pode ser acusada mesmo sem ser pipeline executado. (`bin/tests/test_grep_q_sem_sigpipe.sh:19-20`)

O MORDE 1 exercita somente a forma canônica, sem contraexemplos para precisão. O MORDE 2 calcula `r_pipe`, mas **não exige que seja zero**: fica verde sempre que `r_here=1`, mesmo se o pipeline antigo também acertar. Portanto ele não comprova a reprodução do SIGPIPE no ambiente em que roda. (`bin/tests/test_grep_q_sem_sigpipe.sh:30-32,35-41`)

Não executei os selos: eles criam e removem temporários, contrariando a restrição somente leitura. (`bin/tests/test_grep_q_sem_sigpipe.sh:29-30`; `bin/tests/test_atomicidade_da_porta.sh:41-43`; `bin/tests/test_raia_declarada_no_estado.sh:31-55`)

## 08/11 23:03 · sessao 6fde343d3c37 · - · - · origem=Ronald (tmux)

Q1 — sequência e efeitos:

| Ordem | Passo / efeito | Portões que já responderam |
|---|---|---|
| 1 | `arvore_sem_conflito` lê índice/disco; pode encerrar todos os modos. | Nenhum anterior. `bin/deploy.sh:98`; `bin/arvore_sem_conflito.sh:19-35`. |
| 2 | Só `--restart`: primeira recusa de cron; durante a checagem **apaga marcas de PID morto** com `rm -f`. | Árvore; auth, sombra e pendências ainda não. `bin/deploy.sh:105-114`. |
| 3 | Exceto conferir: auth ou motivo de dispensa; motivo só vai ao stdout deste script. | Árvore e primeira recusa do restart, quando aplicável. `bin/deploy.sh:123-145`. |
| 4 | Exceto conferir/agendado: sombra ou dispensa. Dispensa **apenda arquivo de produção** `logs/deploy_sem_sombra.log`; sombra real também executa **`mkdir -p logs/sombra`** antes de conferir. | Auth já respondeu; mensageria e migration pendente ainda não. `bin/deploy.sh:148-159`; `bin/sombra.sh:81-84`. |
| 5 | Checa migrations core e mensageria por subprocessos `docker exec`; mostra plano quando core pendente. A recusa de mensageria acontece antes do migrate aplicado; restart/conferir são dispensados dessa recusa. | Árvore/auth/sombra conforme modo. `bin/deploy.sh:181-202`. |
| 6 | Core pendente: conferir só informa; cópia/agendado/sem-migrate recusam; demais aplicam `migrate_schemas --tenant`. | Todos os portões pedidos, **exceto a segunda recusa de restart**, já responderam; recusa mensageria é dispensada no restart. `bin/deploy.sh:197-217`. |
| 7 | Conferir sai; demais podem escrever estáticos/manifest por `collectstatic`. | Pendências já tratadas. `bin/deploy.sh:221-224`; `bin/deploy.sh:257-261`. |
| 8 | Prova de casca: cria temporário, cria/remove container efêmero e remove temporário; reprovação encerra. App está montado RO e logs em tmpfs. | Migration/collectstatic podem já ter ocorrido. `bin/deploy.sh:276-282`; `bin/prova_de_casca.sh:28-46`. |
| 9 | Restart: **segunda recusa de cron**, novamente podendo apagar marcas mortas; depois `docker restart`. Demais: `docker kill -s HUP` nos três e checagem de troca dos workers. | Prova de casca já respondeu; segunda recusa pode impedir restart **depois** do migrate/collectstatic. `bin/deploy.sh:304-337`. |
| 10 | Restart do mensageria executa migration no boot segundo compose; as configurações efetivas do container não foram fornecidas. | Portões anteriores passaram. `docker-compose.mensageria.yml:20`; configuração viva [não verificado]. |
| 11 | Subprocessos esperam containers e fazem `Client.get` nas três rotas. Rotas ruins encerram **depois da publicação**. | Todos os portões pedidos anteriores já responderam. `bin/deploy.sh:345-383`. |
| 12 | Selo reciclagem ainda pode reprovar depois da publicação; contador ImportError só lê arquivo. | Portões pedidos anteriores passaram. `bin/deploy.sh:390-393`; `bin/importerror_na_janela.sh:27-32`. |
| 13 | Escreve/trunca `deploy.stamp.novo`, renomeia sobre `deploy.stamp`; depois cria diretório e apenda histórico, salvo agendado puro ou prova pulada. | Inclusive rotas e selo reciclagem já passaram. `bin/deploy.sh:396-407`. |

Efeitos incidentais adicionais: os comandos Django usam por padrão settings SaaS; importar essas settings executa `os.makedirs('/app/logs')` e configura handlers de `errors.log`/`ponto.log`. Portanto não há base para prometer “zero arquivo escrito” por `--check`, `--plan` ou `--conferir`; ambiente efetivo/arquivos existentes [não verificado]. `app/manage.py:9-18`; `app/config/settings/saas.py:118-139`; `app/config/settings/saas.py:160-166`.

Provas GET não demonstram escrita de negócio: UI sem login é protegida por `@login_required`; os dois healths apenas constroem JSON. Escrita de sessão/cache nessas chamadas [não verificado]. `bin/deploy.sh:362-365`; `app/colaboradores/urls.py:47`; `app/colaboradores/views.py:1008-1009`; `app/core/views.py:1302-1306`; `mensageria/config/urls.py:7-8`.

**Conclusão Q1:** a cura põe auth e sombra antes do migrate, mas não torna deploy atômico. Migration ainda precede prova de casca, segunda recusa do restart, prova de rotas e selo reciclagem; esses passos podem recusar quando o banco já mudou. Também existe escrita de log/diretório antes dos portões de pendências. `bin/deploy.sh:157-158`; `bin/deploy.sh:217`; `bin/deploy.sh:280`; `bin/deploy.sh:306`; `bin/deploy.sh:383`; `bin/deploy.sh:390`.

Q2 — política dos modos:

| Modo | Auth | Sombra |
|---|---|---|
| normal | Sim, salvo motivo | Sim, salvo motivo herdado |
| `--conferir` | Não | Não |
| `--sem-migrate` | Sim, salvo motivo | Sim, salvo motivo herdado |
| `--sem-sombra` | Sim, salvo motivo | Dispensa, motivo obrigatório/apendado |
| `--reload-agendado` | Sim, salvo motivo | Isento |
| `--reload-copia` | Sim, salvo motivo | Exigida, salvo motivo herdado |
| `--restart` | Sim, salvo motivo | Exigida, salvo motivo herdado |

As condições dessa tabela são `bin/deploy.sh:56-86` e `bin/deploy.sh:123-164`; pelo diff fornecido, as condições internas do bloco movido não mudaram. Não existe combinação de flags: só `$1` escolhe o modo e `$2` é lido para motivos. `bin/deploy.sh:56-85`.

**Há mudança observável além de “ordem” em caminhos recusados:** antes, mensageria pendente ou core pendente com sem-migrate/agendado/cópia encerravam antes do bloco; agora esses caminhos executam auth e, quando cabível, sombra antes da recusa de pendência. Assim muda a primeira causa/mensagem de falha e pode existir registro “sem sombra” mesmo quando a migration subsequente falha ou a pendência impede publicação. `bin/deploy.sh:123-164`; `bin/deploy.sh:197-217`.

`--conferir` continua fora dos dois portões; agendado continua isento só de sombra; cópia continua sujeito aos dois. A cura não introduz outra isenção nem torna agendado sujeito à sombra. `bin/deploy.sh:123`; `bin/deploy.sh:148`; `bin/deploy.sh:221-223`.

## 08/11 23:03 · sessao 6854c0837f27 · - · - · origem=Ronald (tmux)

Q3 — Chamadores encontrados na cópia:

| Origem | Chamada / tratamento |
|---|---|
| Cron declarado | 03:30, `--reload-agendado`, saída em `reload_diario.log` (`app/config/crons.py:326`). Instalação real do cron em produção: [nao verificado]. |
| Integrador | Wrapper executa `integrador_lote.py rodar` (`bin/integrador.sh:52`); lote chama `--reload-copia` após copiar e gerar documentos, sem examinar seu retorno (`app/core/integrador_lote.py:627`, `app/core/integrador_lote.py:638`); deploy final `--sem-sombra` tem retorno conferido antes de marcar FIM/NO_AR (`app/core/integrador_lote.py:753`, `app/core/integrador_lote.py:754`, `app/core/integrador_lote.py:763`). |
| Molde de fatia | `--reload-copia` ao desfazer e após copiar; final `--sem-sombra` (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_fatia/cadeia.sh:89`). Reconhece sucesso pelo texto `^deploy: OK` (`bin/molde_fatia/cadeia.sh:90`). |
| Molde autocura | Mesmas três chamadas (`bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:45`, `bin/molde_autocura/cadeia.sh:86`); reconhece sucesso pelo texto `^deploy: OK` (`bin/molde_autocura/cadeia.sh:87`). |
| Vigia de login | Depois de `git revert`, chama `--sem-migrate`, conferindo falha da cadeia `&&` (`bin/vigia_logins.sh:87`). |
| Vigia da esteira | Auto-revert e revert do revert chamam `--sem-sombra`; retornos dos deploys são descartados (`app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1275`). |
| Fatia agendada abono | Confere sombra antes; deploy normal seguido de backfill; verifica pipeline com deploy (`fatias_agendadas/abono-no-ar/esteira.sh:30`, `fatias_agendadas/abono-no-ar/esteira.sh:37`, `fatias_agendadas/abono-no-ar/esteira.sh:45`). |
| Fatia agendada o135 | Chama `--conferir`, confere sombra separadamente; após merge chama `--sem-migrate` e guarda retorno (`fatias_agendadas/o135-teto20/esteira.sh:80`, `fatias_agendadas/o135-teto20/esteira.sh:81`, `fatias_agendadas/o135-teto20/esteira.sh:116`, `fatias_agendadas/o135-teto20/esteira.sh:117`). |
| Agendador genérico | Não chama deploy diretamente; chama a esteira de cada diretório (`bin/deploy_agendado.sh:73`). |
| Fabricante | Declara que não publica e proíbe deploy; única ocorrência de `deploy.sh` no script é comentário (`bin/fabricante.sh:8`, `bin/fabricante.sh:17`); prompt também proíbe a chamada (`bin/fabricante_prompt.md:70`). Ausência de execução indireta por conteúdo fabricado: [nao verificado]. |

Nos chamadores acima, não encontrei consumo de “migrate antes do carimbo” ou leitura intermediária do log para disparar outra ação; o consumo mostrado é retorno final, `deploy: OK`, ou descarte do retorno (`bin/molde_fatia/cadeia.sh:89`, `bin/molde_fatia/cadeia.sh:90`, `app/core/integrador_lote.py:753`, `app/core/integrador_lote.py:754`, `app/core/esteira_vigia.py:1267`). Dependência externa não incluída na cópia: [nao verificado].

Q7 — Chave e tabela:

- A chave corrente está em `ISENTOS` como `chamados_decisaopergunta.pergunta_id` (`bin/sombra_regras_pessoais.py:230`).
- Os consumidores efetivos do conjunto `ISENTOS` são o inventário de modelos e o selo SQL da sombra, ambos comparando nomes completos tabela.coluna; nenhum acessa essa chave individualmente (`bin/inventario_pessoal_dos_modelos.py:52`, `bin/inventario_pessoal_dos_modelos.py:78`, `bin/sombra_selo_pessoal.py:25`, `bin/sombra_selo_pessoal.py:34`).
- O mascarador importa as mesmas regras, mas percorre os dicionários de transformação; não percorre `ISENTOS` (`bin/sombra_mascara_pessoal.py:12`, `bin/sombra_mascara_pessoal.py:17`, `bin/sombra_mascara_pessoal.py:44`).
- Busca literal da chave antiga em `bin/` e `app/`: nenhum consumidor encontrado; ausência absoluta ou nomes construídos dinamicamente: [nao verificado]. A ocorrência literal remanescente da tabela antiga no código de instrumentos é comentário sobre o incidente (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:9`).
- O nome de modelo antigo permanece legitimamente na criação e no `RenameModel` histórico (`app/chamados/migrations/0050_resposta_anterior.py:17`, `app/chamados/migrations/0051_decisao_pergunta.py:41`); aparece ainda no tripwire AST que procura escritores dos dois nomes (`app/chamados/tests/test_reabrir_nao_destroi.py:129`) e em comentários/documentação (`app/chamados/models.py:3830`, `app/templates/chamados/partials/modal_fio.html:364`).
- A transformação textual já usa `DecisaoPergunta.ato_texto/ato_motivo` (`bin/sombra_regras_pessoais.py:139`); a alteração de `ISENTOS` alinha o FK ao rename histórico (`bin/sombra_regras_pessoais.py:230`, `app/chamados/migrations/0051_decisao_pergunta.py:41`).

Q8 — Escritores e leitores:

| Estado | Escrita | Leitura |
|---|---|---|
| `logs/deploy.stamp` | Deploy grava `QUANDO`, `COMMIT`, `TITULO` em `.novo`, depois `mv` para destino absoluto de produção (`bin/deploy.sh:396`, `bin/deploy.sh:400`). | Janela lê primeiro `COMMIT=`; caminho pode ser substituído por `JANELA_AUTH_STAMP` (`bin/janela_auth.sh:36`, `bin/janela_auth.sh:39`). Checador de import lê `COMMIT`, salvo override `COMMIT_NO_AR` (`bin/import_tardio_contra_o_ar.py:46`, `bin/import_tardio_contra_o_ar.py:50`). Handoff lê `QUANDO` e `COMMIT` (`bin/handoff_sessao.sh:253`, `bin/handoff_sessao.sh:254`, `bin/handoff_sessao.sh:255`). Vigia da esteira lê para atribuir dono/quarentena, auto-revert e diagnóstico de relógio (`app/core/esteira_vigia.py:64`, `app/core/esteira_vigia.py:1160`, `app/core/esteira_vigia.py:1255`, `app/core/esteira_vigia.py:1313`); parser usa os três campos (`app/core/esteira_vigia.py:990`). |
| `logs/deploy_sem_sombra.log` | Append somente pelo bloco de override, falha de gravação tolerada (`bin/deploy.sh:149`, `bin/deploy.sh:157`, `bin/deploy.sh:158`). | Nenhum leitor literal de execução encontrado em `bin/`/`app/`; leitores externos/dinâmicos: [nao verificado]. |
| `SEM_JANELA_AUTH_MOTIVO` | Deploy não atribui; vem do ambiente, produtor real de produção: [nao verificado]. Testes forçam vazio (`bin/tests/test_deploy_reload_gracioso.sh:94`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`). | Deploy testa não vazio, anuncia motivo e evita chamar guarda (`bin/deploy.sh:140`, `bin/deploy.sh:141`). Esteira o135 lê e anuncia o mesmo override (`fatias_agendadas/o135-teto20/esteira.sh:68`, `fatias_agendadas/o135-teto20/esteira.sh:73`). |
| `SEM_SOMBRA_MOTIVO` | Sobrescrita ao iniciar pelo valor de `DEPLOY_SEM_SOMBRA`; se primeiro argumento for `--sem-sombra`, recebe `$2` (`bin/deploy.sh:82`, `bin/deploy.sh:83`, `bin/deploy.sh:84`). Portanto exportar apenas `SEM_SOMBRA_MOTIVO` não estabelece o motivo (`bin/deploy.sh:82`). | Validação de motivo e bloco que anuncia/grava override em lugar de sombra (`bin/deploy.sh:85`, `bin/deploy.sh:149`, `bin/deploy.sh:156`, `bin/deploy.sh:157`). Teste novo força vazio no ambiente, mas essa atribuição é sobrescrita pela linha 82 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`, `bin/deploy.sh:82`). |
| `SO_CONFERIR` | Inicializa 0; muda a 1 apenas se `$1` for `--conferir` (`bin/deploy.sh:56`, `bin/deploy.sh:57`, `bin/deploy.sh:60`). | Envoltório auth/sombra, recusa mensageria, ramo de migration e retorno antecipado (`bin/deploy.sh:123`, `bin/deploy.sh:197`, `bin/deploy.sh:203`, `bin/deploy.sh:221`). |
| `AGENDADO` | Inicializa 0; vira 1 para `--reload-agendado`; `--reload-copia` também o coloca em 1 (`bin/deploy.sh:59`, `bin/deploy.sh:62`, `bin/deploy.sh:79`). | Sombra, recusa de migration, rótulo do reload e elegibilidade do histórico (`bin/deploy.sh:148`, `bin/deploy.sh:208`, `bin/deploy.sh:311`, `bin/deploy.sh:404`). Teste de orçamento injeta valor ao avaliar a condição extraída (`bin/tests/test_orcamento_contexto.sh:311`); teste auth procura `AGENDADO` no texto dos `if` que cercam a chamada (`bin/tests/test_janela_auth.sh:139`). |
| `COPIA` | Inicializa 0; vira 1 só em `--reload-copia`, junto de `AGENDADO=1` (`bin/deploy.sh:78`, `bin/deploy.sh:79`). | Sombra, recusa de migration, rótulo do reload e elegibilidade do histórico (`bin/deploy.sh:148`, `bin/deploy.sh:205`, `bin/deploy.sh:309`, `bin/deploy.sh:404`). Teste de orçamento injeta valor (`bin/tests/test_orcamento_contexto.sh:311`). |

Produtores da entrada `DEPLOY_SEM_SOMBRA`: moldes e integrador na recarga de cópia (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:45`, `app/core/integrador_lote.py:638`). O deploy usa só o primeiro argumento como modo, mantendo o risco já existente de flags posteriores ignoradas (`bin/deploy.sh:56`, `bin/deploy.sh:83`).

## 08/11 22:47 · sessao acbe2fb35c2e · FIO-E0-V4-E1-FILA · verificador · origem=Code

**O delta fecha os defeitos pontuais de dia local e dos três sítios A1, mas não fecha a garantia global do repasse:** ainda há Chamado terminal e relações com outro colaborador alcançáveis depois da trava inicial de P (`app/chamados/veredito_lavrado.py:114`; `app/chamados/services/fio.py:1022`; `app/chamados/services/disputa_emissao.py:956`; `app/chamados/reconciliador.py:1617`; `app/chamados/services/disputa_emissao.py:1747`).

A execução das suites, os resultados RED→verde citados nos anexos e a ocorrência desses caminhos em produção são **[nao verificado]**. [nao verificado]

**1. Achados marcados CURADO**

| Achado | Conferência do delta |
|---|---|
| **1 — teto do repasse** | **Fecha a omissão por dia:** seleciona toda P de C, além das costuradas nos órfãos. **Não fecha o teto transitivo:** as cascatas alcançam outras P/D e Chamados terminais fora dessa seleção; caminhos detalhados abaixo (`app/chamados/services/disputa_emissao.py:956`; `app/chamados/services/disputa_emissao.py:1747`; `app/chamados/reconciliador.py:1617`). |
| **2 — local × UTC** | **Fecha nos dois leitores apontados:** ambos chamam `relogio_virou`, que converte hoje e a lavra para dia local. Não significa que todo leitor temporal do sistema passe por ela: o backfill mantém conta própria (`app/chamados/veredito_lavrado.py:114`; `app/chamados/veredito_lavrado.py:208`; `app/chamados/services/raiox.py:41`; `app/chamados/management/commands/backfill_veredito_lavrado.py:113`). |
| **4 — A1** | **Fecha os três sítios apontados:** backfill e contador usam diretamente o FK; rechaveamento usa `p.colaborador_id` para célula e despertar. A regressão desse terceiro sítio continua fora do selo novo (`app/chamados/management/commands/backfill_decisoes_pergunta.py:199`; `app/chamados/services/fio.py:1845`; `app/chamados/services/fio.py:1022`; `app/chamados/tests/test_e1_fila.py:87`). |
| **6-F1** | **Fecha a saída vazia identificada:** acrescenta segundo órfão sem P e exige retorno não nulo. Isso não comprova todos os ramos das cascatas (`app/chamados/tests/test_e1_fila.py:261`; `app/chamados/tests/test_e1_fila.py:269`; `app/chamados/tests/test_e1_fila.py:275`). |
| **6-F5** | **Fecha a fraqueza do relógio constante:** cada leitura avança um dia e o mutante retira o instante compartilhado. O juiz real continua mockado nesse teste (`app/chamados/tests/test_e1_fila.py:359`; `app/chamados/tests/test_e1_fila.py:364`; `app/chamados/tests/test_e1_fila.py:376`). |
| **6-A3 parcial** | **Continua parcial:** agora exige a D fechada na sequência, mas pergunta e declaração permanecem no mesmo `DIA`; não exercita o reuso que entra no rechaveamento por dia divergente (`app/chamados/tests/test_e1_fila.py:185`; `app/chamados/tests/test_e1_fila.py:194`; `app/chamados/tests/test_e1_fila.py:204`; `app/chamados/models.py:2938`). |
| **8 — escritores de destino** | **A documentação passa a mencionar SET_NULL**, correspondente à política declarada no FK. A expressão “só o banco” não comprova execução por ação SQL/trigger: isso é **[nao verificado]**. A conferência da mensagem do commit, “seis” versus “sete”, também é **[nao verificado]** (`app/chamados/services/fio.py:808`; `app/chamados/services/fio.py:814`; `app/chamados/models.py:3377`). |

**2. Conjunto do repasse e caminhos que escapam**

Chamando de **C** o colaborador recebido, **O** os órfãos e **P₀** as perguntas selecionadas, o conjunto inicialmente travado é este; a chamada ocorre dentro do atomic, antes do complemento e das demais escritas (`app/chamados/services/disputa_emissao.py:984`; `app/chamados/services/disputa_emissao.py:991`):

| Classe | Conjunto inicial |
|---|---|
| **C** | Somente C; os donos das D/Ch fornecidas explicitamente não são automaticamente acrescentados (`app/chamados/services/disputa_emissao.py:954`; `app/chamados/models.py:2663`). |
| **P₀** | `P.colaborador=C OU P.chamado_destino∈O` (`app/chamados/services/disputa_emissao.py:956`). |
| **D₀** | Todas as D abertas de C, mais as D de P₀, abertas ou fechadas (`app/chamados/services/disputa_emissao.py:958`; `app/chamados/models.py:2669`). |
| **Ch₀** | O, destinos de P₀, todos os Ch vivos de C e pais das D selecionadas que estejam abertas. O recolhimento desses pais não exige que a D pertença a C (`app/chamados/services/disputa_emissao.py:959`; `app/chamados/models.py:2673`; `app/chamados/models.py:2677`; `app/chamados/models.py:2680`). |

O conjunto **alcançável** precisa incluir as expansões abaixo, repetidas enquanto outra cascata encontrar novas linhas; a seleção inicial não calcula esse fechamento (`app/chamados/services/disputa_emissao.py:956`; `app/chamados/services/disputa_emissao.py:1827`; `app/chamados/models.py:1908`):

- **Corpo e cauda:** D da P encontrada por destino, D aberta/criada, perguntas criadas ou reusadas, órfãos analisados e perguntas dos dias extraídos do contexto da D (`app/chamados/services/disputa_emissao.py:1002`; `app/chamados/services/disputa_emissao.py:1013`; `app/chamados/services/disputa_emissao.py:1028`; `app/chamados/services/disputa_emissao.py:1202`; `app/chamados/services/disputa_emissao.py:278`; `app/chamados/services/disputa_emissao.py:294`).
- **Cartório:** outros Ch vivos reconciliados, P absolvidas do colaborador inteiro e D que essas retratações podem fechar (`app/chamados/signals.py:203`; `app/ponto/services/cartorio.py:1488`; `app/ponto/services/cartorio.py:1522`; `app/ponto/services/cartorio.py:958`; `app/chamados/reconciliador.py:584`; `app/chamados/reconciliador.py:612`).
- **Resolver/S143:** P encontradas pelo destino, suas D, todas as irmãs dessas D e respectivos pais; ao fechar uma D, também o C e o universo aberto do dono dela (`app/chamados/services/disputa_emissao.py:1747`; `app/chamados/services/disputa_emissao.py:1804`; `app/chamados/services/disputa_emissao.py:1827`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/models.py:1908`).
- **Fechamento:** reconciliação síncrona do pai e carimbo de pendentes aprovadas da D. Retomada de regularizações e fechamento de becos estão em `on_commit`, portanto ficam fora desta cascata síncrona (`app/chamados/signals.py:42`; `app/chamados/signals.py:163`; `app/chamados/services/materializacao.py:1418`; `app/chamados/signals.py:144`; `app/chamados/signals.py:225`).

Há dois contraexemplos relevantes ao teto:

**a) Chamado terminal existente, com todos os donos consistentes.** A cauda pode criar P sem destino; `gerar_perguntas_disputa` chama `reatar_pergunta_orfa`, que pode reemitir o Chamado do dia. A busca não restringe a vivos; `Chamado.abrir` pode renascer um terminal existente (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/disputa_emissao.py:459`; `app/chamados/reconciliador.py:1617`; `app/chamados/reconciliador.py:838`; `app/chamados/reconciliador.py:910`; `app/chamados/models.py:906`; `app/chamados/models.py:921`).

Se esse terminal não era órfão, pai de D aberta nem destino de P preexistente, ficou fora de Ch₀; o renascimento adquire sua trava posteriormente, embora o topo já tenha travado P₀ e a cauda já tenha inserido a nova P. Seus gates podem recusar casos concretos; ocorrência no dado: **[nao verificado]** (`app/chamados/models.py:2677`; `app/chamados/models.py:2680`; `app/chamados/models.py:3009`; `app/chamados/models.py:1019`; `app/chamados/models.py:1432`; `app/chamados/models.py:290`).

**b) D/Ch de outro colaborador por costura preexistente.** Considere ChX vivo de C, fora de O, com P2 de C2 apontando para ele. ChX entra por “vivos de C”, mas P2/D2 não entram pelo predicado de P₀. O cartório pode reconciliar ChX; o S143 busca P2 pelo destino **sem filtro de colaborador**, pode carimbá-la e segue D2, suas irmãs e seu pai Ch2 (`app/chamados/services/disputa_emissao.py:956`; `app/chamados/models.py:2680`; `app/ponto/services/cartorio.py:1488`; `app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1747`; `app/chamados/services/disputa_emissao.py:1787`; `app/chamados/services/disputa_emissao.py:1804`; `app/chamados/services/disputa_emissao.py:1836`).

A reabertura de Ch2 ou o fechamento de D2 adquire C2 e seu universo depois das P já presas. O nascimento atual recusa nova costura cruzada, mas isso não elimina a leitura do acervo existente; existência dessa configuração em produção: **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1812`; `app/chamados/services/disputa_emissao.py:1888`; `app/chamados/models.py:1908`; `app/chamados/models.py:2838`).

Portanto, **há linhas alcançáveis fora do conjunto travado antes da primeira escrita**, inclusive D/Ch de outro colaborador; a ampliação para toda P de C não resolve essas expansões reversas e transitivas (`app/chamados/services/disputa_emissao.py:956`; `app/chamados/services/disputa_emissao.py:1747`; `app/chamados/models.py:1908`).

**3. `janela_expira_em`, `julgado_em` e relógio**

| Operação | Escritores/leitores |
|---|---|
| **Derivação normal** | `lavrar`: amostra `_agora`, atribui `julgado_em`, calcula a janela com o mesmo instante e salva ambos (`app/chamados/veredito_lavrado.py:251`). |
| **Outro derivador/escritor** | Backfill: calcula a janela e atribui ambos; persiste pela porta de lote. É exceção nominal no contrato (`app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`; `app/core/tests/test_contract_escritores_estado.py:74`). |
| **Restauração** | `desfazer_lavra_em_memoria` restaura a fotografia, inclusive ambos os campos (`app/chamados/veredito_lavrado.py:261`; `app/chamados/veredito_lavrado.py:268`). |
| **Persistência sem nova derivação** | Reuso, carimbo, reapontamento, rechaveamento e relavra recebem os valores de `lavrar` (`app/chamados/models.py:2996`; `app/chamados/models.py:3139`; `app/chamados/services/fio.py:923`; `app/chamados/services/fio.py:1033`; `app/chamados/services/relavra_via.py:126`). |
| **Leitores temporais principais** | `esta_stale` e varredura chamam `relogio_virou`; ambos também comparam marco com `julgado_em` (`app/chamados/veredito_lavrado.py:208`; `app/chamados/veredito_lavrado.py:212`; `app/chamados/services/raiox.py:41`; `app/chamados/services/raiox.py:44`). |
| **Outros leitores** | Fotografia de rollback, serialização de reversão do passivo e comparação persistido/recalculado no backfill (`app/chamados/veredito_lavrado.py:261`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:83`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `app/chamados/management/commands/backfill_veredito_lavrado.py:135`). |

As unidades são distintas e coerentes nos dois leitores corrigidos: `julgado_em` é **instante**, `janela_expira_em` é **dia**; para o ramo do relógio, o instante é convertido para dia local. A regra é `expira <= hoje` e `dia_local(lavra) < expira`: expiração inclusiva, lavra estritamente anterior; marco usa `marco > julgado_em` (`app/chamados/models.py:2588`; `app/chamados/models.py:2594`; `app/chamados/veredito_lavrado.py:114`; `app/chamados/veredito_lavrado.py:212`).

A janela viva inclui os dois extremos; `expira_em` procura o primeiro dia fora dela, compatível com invalidar quando `hoje >= expira` (`app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:141`; `app/chamados/veredito_lavrado.py:143`).

**Não há um único relógio para todo o ato.** O juiz ainda lê `date.today()` e é chamado antes de `lavrar` amostrar `_agora`; o teste F5 o substitui por mock. A configuração é São Paulo com timezone habilitado, mas a igualdade efetiva entre dia do processo e dia local em produção é **[nao verificado]** (`app/chamados/juizes.py:575`; `app/chamados/veredito_lavrado.py:242`; `app/chamados/veredito_lavrado.py:251`; `app/chamados/tests/test_e1_fila.py:364`; `app/config/settings/base.py:93`).

**Há outro leitor com conta própria:** o backfill lê `localdate()` e `now()` separadamente e calcula `e <= hoje`. Portanto pode atravessar meia-noite entre as amostras; ocorrência real: **[nao verificado]** (`app/chamados/management/commands/backfill_veredito_lavrado.py:94`; `app/chamados/management/commands/backfill_veredito_lavrado.py:95`; `app/chamados/management/commands/backfill_veredito_lavrado.py:113`).

Também permanece a ambiguidade anterior do campo: a descrição promete valor determinístico por evento, mas, para evento já expirado, `expira_em` devolve o dia da consulta/lavra, porque começa em `max(hoje, data_evento)` (`app/chamados/models.py:2597`; `app/chamados/veredito_lavrado.py:137`; `app/chamados/veredito_lavrado.py:142`).

**4. FILA e descartes**

- **Item 5 / A2 com chamador já travado: procedente quanto à ordem global.** O filtro de irmão/irmãs ocorre antes do atomic. Um irmão pode surgir antes da aquisição de C; depois de C/D/Ch/P presos, `pai.reabrir()` consulta novamente e entra em migração. Esta tenta adquirir todos os C das perguntas, inclusive outro colaborador, antes da recusa posterior de costura. Ocorrência em produção: **[nao verificado]** (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:153`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:172`; `app/chamados/models.py:1411`; `app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2160`).
- **`UniversoMudouSobATrava` não fecha esse caso:** a conferência de crescimento vem depois da aquisição interna dos C; ela não desfaz preventivamente a ordem já imposta pelo chamador (`app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2088`; `app/chamados/services/fio.py:2090`).
- **Fila 3/f5 é dívida real no caminho lido pela obra:** a cura de `expira_em` não torna único o relógio do julgamento completo. Pode ser adiada como desenho, mas não sustenta a afirmação de mesmo instante entre juiz e lavra (`app/chamados/juizes.py:569`; `app/chamados/juizes.py:575`; `app/chamados/veredito_lavrado.py:242`; `app/chamados/veredito_lavrado.py:251`).
- **A3 por dia diferente e A2 por crescimento continuam lacunas de prova:** o caso A3 permanece no mesmo dia; o cenário A2 monta dois colaboradores estáticos (`app/chamados/tests/test_e1_fila.py:146`; `app/chamados/tests/test_e1_fila.py:157`; `app/chamados/tests/test_e1_fila.py:185`; `app/chamados/tests/test_e1_fila.py:194`).
- **Descarte P3/P6/P7 é defensável:** os mutantes introduzem explicitamente P antes de C e exigem sequência inválida; P7 exige também três migrações. Não é necessário exigir uma mensagem textual específica para demonstrar essa mutação (`app/chamados/tests/test_e1_fila.py:311`; `app/chamados/tests/test_e1_fila.py:320`; `app/chamados/tests/test_e1_fila.py:333`; `app/chamados/tests/test_e1_fila.py:337`).
- **Rastreador:** ignorar `executemany` e trava de FK é limitação real da prova, não comprovação de outra violação concreta. Os verdes desse instrumento não demonstram todas as aquisições possíveis; um contraexemplo adicional especificamente nesses mecanismos é **[nao verificado]** (`app/chamados/tests/test_e1_v9.py:51`; `app/chamados/tests/test_e1_v9.py:69`).

A ordem exigida é por transação, inclusive entre colaboradores, com todos os C antes da primeira D; por isso a ausência de ocorrência demonstrada não elimina o caminho estático do item 5 (`app/chamados/models.py:2629`; `app/chamados/models.py:2652`; `app/chamados/tests/test_e1_v15g_portas.py:8`).

**5. Os testes mordem? Passam vazios?** [nao verificado]

| Teste | Avaliação estática | [nao verificado]
|---|---|
| **A1 por `values_list`** | Morde os dois sítios antigos: o censo real acusa chamadas com ambos os campos. **Não morde** a regressão direta `colab_id = p.disputa.colaborador_id`, pois só reconhece `or` ou chamadas com ambos os nomes (`app/chamados/tests/test_e1_fila.py:87`; `app/chamados/tests/test_e1_fila.py:98`; `app/chamados/tests/test_e1_fila.py:118`; `app/chamados/services/fio.py:1022`). |
| **Teto do repasse** | Não é vazio: cria P a vinte dias e exige sua P e D na sequência; o recorte anterior ±1 não inclui essa P. Contudo testa apenas `_trava_do_repasse`, não as cascatas nem outros colaboradores (`app/chamados/tests/test_e1_fila.py:292`; `app/chamados/tests/test_e1_fila.py:295`; `app/chamados/tests/test_e1_fila.py:297`; `app/chamados/tests/test_e1_fila.py:299`). |
| **F1 com segundo órfão** | Retira a vacuidade conhecida pelo retorno não nulo. O mutante morde a retirada da trava ampla; não demonstra, sozinho, necessidade da ampliação para qualquer dia (`app/chamados/tests/test_e1_fila.py:261`; `app/chamados/tests/test_e1_fila.py:275`; `app/chamados/tests/test_e1_fila.py:281`; `app/chamados/tests/test_e1_fila.py:285`). |
| **F5 relógio que anda** | Com evento já expirado, duas amostras produzem dias diferentes; a asserção de igualdade falha se `expira_em` voltar a ler sozinho. O mutante exige justamente essa diferença. Não prova o relógio do juiz real, que está mockado (`app/chamados/tests/test_e1_fila.py:359`; `app/chamados/tests/test_e1_fila.py:364`; `app/chamados/tests/test_e1_fila.py:372`; `app/chamados/tests/test_e1_fila.py:376`). |
| **F5 `date.today`** | Borda simulada em março contra mundo congelado em novembro distingue as fontes de relógio; o mutante deve divergir do resultado esperado (`app/chamados/tests/test_e1_fila.py:342`; `app/chamados/tests/test_e1_fila.py:350`; `app/chamados/tests/test_e1_fila.py:355`; `app/chamados/tests/test_e1_fila.py:383`). |
| **DiaLocalEstaStale** | Dois casos efetivos: antes da meia-noite local espera False; depois, com lavra na noite anterior, True. As contas UTC anteriores dão os resultados contrários (`app/chamados/tests/test_e1_fila.py:393`; `app/chamados/tests/test_e1_fila.py:401`; `app/chamados/tests/test_e1_fila.py:410`). |
| **DiaLocalVarredura** | Cria uma P, atualiza os campos e consulta sua PK; o caso esperado True impede passar com queryset vazio (`app/chamados/tests/test_e1_fila.py:420`; `app/chamados/tests/test_e1_fila.py:422`; `app/chamados/tests/test_e1_fila.py:425`; `app/chamados/tests/test_e1_fila.py:426`). |
| **A3 alterado** | A nova asserção prova presença da D fechada; não prova execução do rechaveamento por dia divergente (`app/chamados/tests/test_e1_fila.py:194`; `app/chamados/tests/test_e1_fila.py:204`; `app/chamados/models.py:2938`). |

Os mutantes falham explicitamente quando sua âncora não existe, evitando montagem silenciosamente inoperante; execução das suites nesta auditoria: **[nao verificado]** (`app/chamados/tests/test_e1_fila.py:47`; `app/chamados/tests/test_e1_fila.py:50`).

**6. Classes que já reprovaram obra**

- **Ordem de trava:** permanece nos caminhos de terminal e de outro colaborador descritos acima; é a principal insuficiência do teto alterado (`app/chamados/reconciliador.py:1617`; `app/chamados/models.py:921`; `app/chamados/services/disputa_emissao.py:1747`; `app/chamados/models.py:1908`).
- **Selo sem cobertura da cura inteira:** o novo selo morde os aliases, mas deixa passar a reversão do rechaveamento para o colaborador da D (`app/chamados/tests/test_e1_fila.py:87`; `app/chamados/tests/test_e1_fila.py:98`; `app/chamados/services/fio.py:1022`).
- **Relógio fora da borda compartilhada:** continua no juiz alcançado por `lavrar`; o delta não introduz essa dívida, mas também não a elimina (`app/chamados/juizes.py:575`; `app/chamados/veredito_lavrado.py:242`; `app/chamados/veredito_lavrado.py:251`).
- **Derivação paralela:** os três sítios A1 apontados foram corrigidos; permanece a reprodução preexistente da regra de janela em `expira_em` e no juiz (`app/chamados/management/commands/backfill_decisoes_pergunta.py:199`; `app/chamados/services/fio.py:1022`; `app/chamados/services/fio.py:1847`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/juizes.py:601`).
- **Segundo escritor:** o backfill continua atribuindo os campos fora de `lavrar`, com exc

[truncado em 20000 de 20745 caracteres]

## 08/11 22:44 · sessao 2858cbb65143 · - · - · origem=Ronald (tmux)

3. **Relógio e campos**

- `julgado_em` é `DateTimeField`; `janela_expira_em`, `DateField` (`app/chamados/models.py:2588`, `app/chamados/models.py:2594`). A porta `lavrar` atribui ambos a partir de `_agora=timezone.now()`, passando esse instante a `expira_em`; salva juntos (`app/chamados/veredito_lavrado.py:251-255`).
- Há outro atribuidor: o backfill obtém `hoje=timezone.localdate()` e `agora=timezone.now()` em leituras distintas, calcula `e=expira_em(..., agora=agora)` e atribui os dois campos; persiste por `gravar_lavras_em_lote` (`app/chamados/management/commands/backfill_veredito_lavrado.py:94-95`, `:111`, `:139-146`; `app/chamados/veredito_lavrado.py:220-230`). Essa exceção está registrada no contrato (`app/core/tests/test_contract_escritores_estado.py:74-84`).
- A reversão em memória também atribui ambos, restaurando a fotografia anterior; o chamador é o tratamento de falha de `carimbar` (`app/chamados/veredito_lavrado.py:259-269`; `app/chamados/models.py:3137-3145`).
- Persistências que recebem valores de `lavrar`, sem nova derivação: nascimento/reuso em `models.py:2996-2999`, carimbo em `models.py:3139-3142`, reapontamento em `services/fio.py:923-925`, rechaveamento em `services/fio.py:1033-1034`, relavra em `services/relavra_via.py:126-128` (todos sob `app/chamados/`).
- Os leitores temporais principais convergem: `esta_stale` chama `relogio_virou`; a varredura faz o mesmo, com um instante para o lote (`app/chamados/veredito_lavrado.py:204-212`; `app/chamados/services/raiox.py:31-44`). `relogio_virou` usa **dia local**, invalida com `expira <= hoje` e exige `dia_local(julgado_em) < expira`; portanto igualdade da lavra com expiração não invalida (`app/chamados/veredito_lavrado.py:112-114`). Os marcos usam comparação de instantes estrita `marco > julgado_em` (`app/chamados/veredito_lavrado.py:212`; `app/chamados/services/raiox.py:44`).
- Outros leitores diretos: fotografia para rollback (`app/chamados/veredito_lavrado.py:261`), serialização da reversão do passivo (`app/chamados/services/passivo_validado.py:64-66`, `:81-83`, `:367`), e backfill, que compara o campo persistido ao recalculado (`app/chamados/management/commands/backfill_veredito_lavrado.py:121`, `:135`).
- **Existe outra conta de dia no backfill:** `e <= hoje`, usando seu próprio `hoje`; não é a implementação completa de `relogio_virou`, mas impede afirmar “todo leitor temporal passa por ela” (`app/chamados/management/commands/backfill_veredito_lavrado.py:94-95`, `:111-113`). Cruzamento de meia-noite entre essas duas leituras é caminho de código; ocorrência real [nao verificado].
- A cura não alcança o relógio do juiz: `_dentro_da_janela_viva(data_evento)` não recebe instante e usa `date.today()` (`app/chamados/juizes.py:569`, `:575`). A lavra chama o juiz **antes** de amostrar seu `_agora` (`app/chamados/veredito_lavrado.py:242-253`); há ramos do juiz que consultam essa janela (`app/chamados/juizes.py:674`, `:764`, `:1217`). Igualdade do fuso efetivo do processo em produção [nao verificado]; configuração da cópia: `America/Sao_Paulo`, `USE_TZ=True` (`app/config/settings/base.py:93-95`).
- A janela viva mantém limites inclusivos, enquanto sua expiração é o primeiro dia fora dela (`app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:137-143`). `janela_expira_em` não é estritamente função fixa do evento: para evento já expirado retorna `hoje`; isso contradiz a descrição “Deterministico por data_evento”, mas precede o delta (`app/chamados/models.py:2597`; `app/chamados/veredito_lavrado.py:137-143`; `app/chamados/management/commands/backfill_veredito_lavrado.py:118`).

5. **Testes: mordida e limites** [nao verificado]

| Teste | Avaliação estática | [nao verificado]
|---|---|
| A1 por `values_list` | O detector agora acusa os dois campos na mesma chamada; o RED sobre fontes reais falha com os dois sítios antigos (`app/chamados/tests/test_e1_fila.py:97-119`). A mordida usa exemplo literal com os dois campos (`:129-138`). |
| Limite A1 | **A reversão de `rechavear` para `colab_id=p.disputa.colaborador_id` continua invisível ao selo:** não é `or` nem chamada `values` com os dois campos (`app/chamados/tests/test_e1_fila.py:87-101`; cura em `app/chamados/services/fio.py:1022`). Executei apenas os dois helpers extraídos por AST, em Python com `-B`: essa atribuição retorna `[]`; nenhuma suite Django executada. |
| Teto do repasse | Cria P vinte dias distante, exige P e sua D na sequência; falha com o recorte ±1 anterior (`app/chamados/tests/test_e1_fila.py:287-300`). Não é teste das cascatas nem de Ch/C de outro colaborador: chama somente `_trava_do_repasse` e verifica P/D (`:296-300`). |
| F1 segundo órfão | Introduz órfão sem P, passa os dois à porta e exige retorno não nulo, retirando a saída prematura que esvaziava os laços (`app/chamados/tests/test_e1_fila.py:261-277`). Seu mutante remove a trava ampla e exige violação (`:279-285`); o teste específico da ampliação por todos os dias é o do teto (`:287-300`). |
| F5 mesmo instante | Relógio avança um dia por leitura; juiz é mockado para `False`; exige expiração no dia local da lavra (`app/chamados/tests/test_e1_fila.py:357-372`). Mutante retira `agora=_agora`, e a desigualdade exigida demonstra a diferença entre duas amostragens (`:374-379`). Não exercita o relógio real do juiz, por causa do mock (`:364`). |
| F5 borda/date.today | Congela mundo em novembro, faz `timezone.now` devolver março e exige março; mutante devolve relógio `date.today()` e deve divergir (`app/chamados/tests/test_e1_fila.py:342-355`, `:381-384`). |
| DiaLocalEstaStale | Dois casos não vazios: antes da meia-noite local deve dar False; depois, com lavra na noite anterior, True. As comparações UTC antigas produzem os resultados opostos (`app/chamados/tests/test_e1_fila.py:393-411`). |
| DiaLocalVarredura | Cria P real, atualiza ambos os campos para cada caso, consulta queryset contendo sua PK e exige presença/ausência exata (`app/chamados/tests/test_e1_fila.py:420-426`). Não passa com queryset vazio no caso esperado True (`:426`). |
| A3 parcial | Exige agora D fechada na sequência, além da P e Ch (`app/chamados/tests/test_e1_fila.py:199-206`). Continua mesmo DIA: P nasce no DIA padrão e mocks devolvem DIA (`:185`, `:194-196`; padrão em `app/chamados/tests/test_e1_v9.py:145-147`); ramo de rechaveamento por dia diferente não ganhou caso (`app/chamados/services/fio.py:1012-1013`). |

Resultado de execução das suites, RED anterior e verde atual: [nao verificado]. O montador de mutantes falha explicitamente se a âncora da cura não existe, evitando erro silencioso de montagem (`app/chamados/tests/test_e1_fila.py:47-55`).

6. **Classes relacionadas ao delta**

- `relogio_virou` centraliza o predicado já duplicado nos dois leitores; não cria segundo juiz para decidir se a janela viva contém o evento, pois seu predicado é invalidação da lavra persistida (`app/chamados/veredito_lavrado.py:105-114`; `app/chamados/juizes.py:569-601`).
- Continuam, nos sítios lidos pelo delta, relógio independente no juiz, escritor excepcional do backfill e conta temporal própria desse backfill; todos já existiam, com referências acima (`app/chamados/juizes.py:575`; `app/chamados/management/commands/backfill_veredito_lavrado.py:94-113`, `:139-146`).
- O selo novo tem mordida para a regressão por `values_list`, porém não protege a terceira cura A1, a atribuição direta de `rechavear`; isso é limitação concreta de cobertura, sem inferir ocorrência de dados divergentes (`app/chamados/tests/test_e1_fila.py:87-101`; `app/chamados/services/fio.py:1022`).

## 08/11 22:44 · sessao 5d1428d82aec · - · - · origem=Ronald (tmux)

O teto mudou de “dias ±1” para `P.colaborador=C ∪ P.destino∈órfãos`; isso fecha a omissão temporal específica, mas não demonstra cobertura transitiva de todas as cascatas (`app/chamados/services/disputa_emissao.py:954-959`).

**Conjunto efetivamente travado antes da primeira escrita:**

- **C:** somente o colaborador recebido (`app/chamados/services/disputa_emissao.py:954`; `app/chamados/models.py:2660-2664`).
- **D:** todas as abertas de C, mais as disputas das P selecionadas, abertas ou fechadas (`app/chamados/services/disputa_emissao.py:956-959`; `app/chamados/models.py:2669-2674`).
- **Ch:** órfãos recebidos, destinos das P selecionadas, todos os vivos de C e pais das D selecionadas que estejam abertas; esse último recolhimento não verifica o dono da D (`app/chamados/services/disputa_emissao.py:958-959`; `app/chamados/models.py:2673-2687`).
- **P:** somente as selecionadas pelo OR acima; ordenar e travar seus próprios registros não expande irmãs nem destinos reversos (`app/chamados/services/disputa_emissao.py:956-959`; `app/chamados/models.py:2688-2691`).

**O conjunto alcançável é maior:**

- O corpo complementa D da primeira P encontrada por destino, cria ou atualiza D de C, cria/reusa P e analisa órfãos; a seleção inicial da P não exige que ela ou sua D pertençam a C (`app/chamados/services/disputa_emissao.py:1002-1017`, `1028-1064`, `1145-1155`, `1176-1188`).
- A cauda lê batidas nomeadas pelo contexto da D, gera perguntas de outros dias, modifica o contexto e pode fechar a D (`app/chamados/services/disputa_emissao.py:278-296`, `1202-1219`).
- `analisar` grava estado e dispara síncronamente o cartório; o cartório pode reconciliar outros chamados vivos do dia, retratar P de todo o colaborador e executar a fábrica (`app/chamados/models.py:800-806`, `289-296`; `app/chamados/signals.py:187-203`; `app/ponto/services/cartorio.py:1488`, `1501`, `1522`, `958-963`, `985-997`; `app/chamados/reconciliador.py:584-614`).
- O resolver por chamado segue todas as P ancoradas nele, sem filtro por dono; depois segue suas D, todas as P dessas D, seus pais e o universo aberto do dono da D quando `fechar` toma suas travas (`app/chamados/services/disputa_emissao.py:1747-1751`, `1787-1791`, `1804-1812`, `1827-1845`, `1861`, `1875-1883`; `app/chamados/models.py:1908-1910`).
- Fechar D reconcilia o pai pelo signal e materializa P aprovadas de toda a D; são chamadas síncronas. Retomar regularizações e fechar becos são `on_commit` e ficam fora desse conjunto síncrono (`app/chamados/signals.py:41-45`, `162-163`, `144`, `225`; `app/chamados/services/materializacao.py:1418-1432`).

**Caminhos que continuam fora do teto:**

1. **Ch terminal existente, mesmo colaborador e sem nenhuma divergência de dono.** Uma P nova da cauda pode nascer sem destino e chamar `reatar_pergunta_orfa`; este pode reemitir um chamado do dia já encerrado. `Chamado.abrir` busca todos os estados e renasce o encerrado, que não estava no teto se não era órfão, pai de D aberta nem destino de P preexistente (`app/chamados/services/disputa_emissao.py:444-464`; `app/chamados/reconciliador.py:1582-1591`, `1617`, `834-839`, `910-911`; `app/chamados/models.py:906-924`; teto: `2677-2687`). A aquisição de Ch ocorre depois da inserção da nova P; o renascimento chama `reabrir`, que grava estado e ressuscita perguntas (`app/chamados/models.py:3009`, `1019`, `1432-1441`, `289-291`). Os gates podem recusar um caso concreto; ocorrência no dado [nao verificado].

2. **D/Ch de outro colaborador por costura preexistente.** Considere ChX vivo de C, fora da lista de órfãos, com P2 de C2 apontando para ele: o teto inclui ChX, mas exclui P2/D2. O cartório despertado por outro órfão pode reconciliar ChX; S143 busca P2 pelo destino, carimba-a e percorre D2 e suas irmãs. Pode reabrir seu pai Ch2 e fechar D2, adquirindo C2 e seu universo depois das P já travadas/escritas (`app/chamados/services/disputa_emissao.py:956-959`; `app/chamados/models.py:2680-2687`; `app/chamados/signals.py:114-115`; `app/ponto/services/cartorio.py:1488`, `1501`, `1522`; `app/chamados/services/disputa_emissao.py:1747-1751`, `1787`, `1804-1812`, `1827-1861`, `1875-1883`; `app/chamados/models.py:1908-1910`). O nascimento atual recusa destino de outro colaborador, mas isso não elimina o caminho de leitura/escrita sobre acervo já costurado; existência dessa configuração no dado [nao verificado] (`app/chamados/models.py:2837-2838`; `app/chamados/services/disputa_emissao.py:1747-1751`).

3. **Entrada órfã de outro colaborador.** A porta não confere o dono dos chamados recebidos antes de travá-los explicitamente; um `analisar` posterior toma C e o universo do dono efetivo do chamado (`app/chamados/services/disputa_emissao.py:980-991`, `997-1017`; `app/chamados/models.py:289-291`, `800-806`). Ocorrência entre os chamadores/prod [nao verificado].

A checagem de igualdade entre `P.colaborador` e o dono da D roda apenas no nascimento; portanto ela não prova consistência de todo o acervo lido pelas cascatas (`app/chamados/models.py:3410-3425`). A cobertura exata exigiria o fechamento transitivo das relações efetivamente seguidas — destinos reversos, D das P alcançadas, irmãs, pais e universos dos novos donos — além dos terminais que a reemissão pode renascer; `_trava_do_repasse` faz somente uma expansão (`app/chamados/services/disputa_emissao.py:954-959`; caminhos: `1747-1883`; `app/chamados/reconciliador.py:910-911`; `app/chamados/models.py:906-924`).

## 08/11 22:44 · sessao 00b27372de1d · - · - · origem=Ronald (tmux)

**Descartes/FILA que merecem ressalva**

- **Item 5/A2, chamador já travado: caminho continua aberto.** O command filtra `irmao_vivo`/irmãs antes da transação; depois trava C/D/Ch/P e chama novamente `pai.reabrir()`. Se um irmão surgir entre filtro e trava, `reabrir()` entra em migração; esta coleta perguntas dos dois chamados e tenta travar seus colaboradores, inclusive outro colaborador, depois das linhas já presas pelo command. A recusa de destino cruzado na costura ocorre posteriormente e não impede essa aquisição de C. Ocorrência em produção: [nao verificado]. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `:108`, `:146`, `:153`, `:172`; `app/chamados/models.py:1390`, `:1411`; `app/chamados/services/fio.py:2037`, `:2085`, `:2086`, `:2094`, `:2160`.)
- **A recusa `UniversoMudouSobATrava` não cura o chamador previamente travado:** ela verifica crescimento entre as duas leituras internas da migração; a primeira aquisição interna de C já aconteceu antes dessa conferência. (`app/chamados/services/fio.py:2084`, `:2086`, `:2088`, `:2090`.)
- **Fila 3/f5 é dívida real preexistente:** `_dentro_da_janela_viva` continua lendo `date.today()` sem parâmetro; `expira_em` reproduz sua guarda com um dia obtido por `timezone.localdate`. O novo `relogio_virou` centraliza somente invalidação do cache; não é novo juiz de mérito. A autoridade registrada continua `pergunta_viva`. (`app/chamados/juizes.py:569`, `:575`, `:601`; `app/chamados/veredito_lavrado.py:102`, `:105`, `:114`, `:120`, `:137`, `:141`; `app/core/juizes.py:266`.)
- **A3 parcial continua parcial:** o teste exige agora a D fechada, mas prepara a pergunta e a declaração para `DIA`; não exercita o ramo de reuso com data divergente que chama rechaveamento. (`app/chamados/tests/test_e1_fila.py:185`, `:194`, `:196`, `:204`; `app/chamados/models.py:2938`, `:2945`.)
- **Não confirmei cascata efetiva A3→cartório.** Existe a ligação genérica de `post_save` de Chamado ao cartório, mas depende de mudança de status e dia forte; o pai criado pela declaração recebe somente `origem`, sem dia forte. Assim, usar essa cascata específica como reprovação exige mais caminho: [nao verificado]. (`app/chamados/signals.py:191`, `:192`, `:201`, `:202`, `:203`; `app/chamados/services/disputa_emissao.py:630`; `app/chamados/juizes.py:98`, `:117`, `:127`.)

**Testes e instrumento** [nao verificado]

- O descarte de exigir identificação textual da violação P3/P6/P7 é defensável: os mutantes introduzem explicitamente trava de P antes de C, e o contrato exige sequência inválida; P7 também exige três migrações. Isso não prova execução verde nesta auditoria: [nao verificado]. (`app/chamados/tests/test_e1_fila.py:311`, `:314`, `:319`, `:324`, `:332`, `:337`, `:338`.)
- O rastreador realmente ignora `executemany` e travas de FK. Portanto seus verdes não provam a ordem de **todas** as aquisições possíveis; não encontrei nesta subtarefa um caminho concreto adicional que explore esses dois limites: [nao verificado]. (`app/chamados/tests/test_e1_v9.py:51`, `:69`, `:72`; `app/chamados/tests/test_e1_fila.py:62`.)

**Classes/f7**

- O contrato exige ordem por transação, conjunto completo no topo e cada classe por PK; logo o caminho concorrente do command acima pertence ao escopo, apesar de a ocorrência não estar demonstrada. (`app/chamados/models.py:2629`, `:2633`, `:2634`.)
- A documentação f7 agora enumera a escrita do nascimento/reuso e a limpeza por `SET_NULL`; o campo efetivamente declara essa política. A afirmação literal “só o banco” não demonstra trigger ou `ON DELETE SET NULL` no DDL: isso está [nao verificado]. (`app/chamados/services/fio.py:808`, `:811`, `:813`; `app/chamados/models.py:3375`, `:3377`.)
- O contrato de escritores continua listando `models.py` e `fio.py`; o delta de prosa não elimina nem acrescenta escritor próprio de aplicação. (`app/core/tests/test_contract_escritores_estado.py:61`; `app/chamados/services/fio.py:808`.)

## 08/11 22:27 · sessao ec61578336e4 · FIO-E0-V4-E1-FILA · verificador · origem=Code

**A ordem global ainda não está demonstrada para o atomic inteiro:** o repasse pode alcançar perguntas fora da pré-trava, e a migração pode receber travas já mantidas pelo chamador. (`app/chamados/services/disputa_emissao.py:955`, `:1206`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:153`, `:172`)

Resultados RED/GREEN, migrations aplicadas, contagens e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

1. **ALTA — O novo “teto” do repasse não cobre tudo que a mesma transação pode escrever.** A pré-trava inclui perguntas costuradas nos órfãos e perguntas do colaborador nos dias dos órfãos ±1 ou sem dia. A cauda chama `perguntar_entradas_pendentes`, que obtém outros dias do contexto da disputa e chama a geração de perguntas. (`app/chamados/services/disputa_emissao.py:955`, `:958`, `:1206`, `:278`, `:283`, `:294`)

   Um caminho permitido pelo código é: P de pk maior pré-travada no dia do órfão; entrada pendente de outro dia, fora do recorte; reuso de P canônica antiga desse outro dia; atribuição de célula/destino e **UPDATE dessa P de pk menor depois da maior**. O reuso procura a canônica globalmente e salva as alterações. Reprodução e ocorrência no acervo: [nao verificado]. (`app/chamados/services/disputa_emissao.py:443`; `app/chamados/models.py:2905`, `:2919`, `:2958`, `:2975`, `:2999`)

   Há outra expansão pela cascata síncrona: `analisar → _gravar_estado/save → post_save → cartório`; havendo adiamentos pela lâmpada, o cartório chama a retratação das perguntas absolvidas **do colaborador inteiro**, sem recorte de dia. Ela pode atualizar P antiga que não entrou no teto. (`app/chamados/services/disputa_emissao.py:1021`; `app/chamados/models.py:806`, `:296`; `app/chamados/signals.py:203`; `app/ponto/services/cartorio.py:951`, `:959`; `app/chamados/reconciliador.py:584`, `:595`, `:627`)

2. **ALTA — A data de expiração continua com contratos de fuso incompatíveis.** O escritor usa `localdate(agora)`; `esta_stale` compara `agora.date()` e `julgado_em.date()`, e a varredura repete essas comparações. A configuração declara São Paulo e `USE_TZ=True`. Portanto, com instantes UTC, escritor e leitores podem estar em dias diferentes. É problema preexistente mantido pelo diff. (`app/chamados/veredito_lavrado.py:103`, `:126`, `:196`, `:199`; `app/chamados/services/raiox.py:36`, `:41`; `app/config/settings/base.py:93`, `:95`)

   A consequência pode ultrapassar algumas horas: se a lavra ocorrer antes da meia-noite local, mas seu dia UTC já for a data de expiração, a condição `julgado_em.date() < janela_expira_em` fica falsa. Esse ramo deixa de invalidar a lavra quando o dia local alcançar a expiração. É uma consequência das condições; ocorrência real: [nao verificado]. (`app/chamados/veredito_lavrado.py:196`, `:199`)

3. **MÉDIA — f5 une o instante do carimbo ao da expiração, mas não ao julgamento.** `lavrar` chama o juiz antes de capturar `_agora`; o juiz ainda lê `date.today()` independentemente. O backfill captura `hoje` e `agora` separadamente e executa o juiz novamente por linha. Logo, “mesmo instante” vale para `julgado_em` e `expira_em`, não para a decisão inteira. (`app/chamados/veredito_lavrado.py:234`, `:242`, `:244`; `app/chamados/juizes.py:575`; `app/chamados/management/commands/backfill_veredito_lavrado.py:94`, `:95`, `:105`)

4. **MÉDIA — A1 elimina a sintaxe escolhida, não toda derivação paralela.** Permanecem os equivalentes com aliases: `c or dc` em `fecho_cruza_dias` e no backfill de decisões. O detector AST procura diretamente `.disputa.colaborador[_id]` como alternativa de `or`, sem resolver aliases. (`app/chamados/services/fio.py:1844`, `:1847`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:199`; `app/chamados/tests/test_e1_fila.py:76`, `:86`)

   Também permanece uma identidade dividida no rechaveamento: trava/dedup usam `p.colaborador_id`, mas célula nova e despertar usam `p.disputa.colaborador_id`. A igualdade é verificada no nascimento, sem nova conferência nesse `save` de linha existente. Divergência efetiva no acervo: [nao verificado]. (`app/chamados/services/fio.py:1000`, `:1021`, `:1022`, `:1024`, `:1035`; `app/chamados/models.py:3410`, `:3423`)

5. **MÉDIA — A2 ordena sua própria chamada, não o prefixo transacional de todos os chamadores.** `reabrir_orfas_enterradas` já mantém C/D/Ch/P quando chama `pai.reabrir`; o ramo `irmao_vivo` chama a migração, cujo conjunto pode acrescentar C de outra pergunta. Isso permite aquisição de C depois de D/Ch/P já seguros. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`, `:153`, `:172`; `app/chamados/models.py:1411`, `:1414`; `app/chamados/services/fio.py:2037`, `:2085`, `:2086`)

   **A ressalva é necessária:** o pré-filtro desse comando já exclui `irmao_vivo`. O caminho exige mudança entre o pré-filtro e a trava — por exemplo, surgimento de irmão nesse intervalo — ou mudança posterior da decisão. Acervo cruzado sozinho não demonstra esse caminho; ocorrência: [nao verificado]. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `:108`, `:153`)

6. **MÉDIA — Os testes novos têm cobertura útil, mas algumas alegações excedem o que verificam.** [nao verificado]

   | Teste | O que efetivamente demonstra | [nao verificado]
   |---|---|
   | **A1** | Varre produção e detecta os exemplos sintáticos declarados; pode passar com os aliases sobreviventes do achado 4. (`app/chamados/tests/test_e1_fila.py:99`, `:109`, `:122`; `app/chamados/services/fio.py:1847`) |
   | **A2** | Não é vazio: exige os dois colaboradores e detecta a aquisição adicional fora de ordem. Não testa crescimento do universo entre leitura e trava, nem entrada por chamador já travado. (`app/chamados/tests/test_e1_fila.py:147`, `:151`, `:154`; `app/chamados/services/fio.py:2088`) |
   | **A3** | **Morde por motivo diferente do anunciado para a Disputa fechada:** fixture e pedido usam o mesmo dia; isso pula o ramo que trava a Disputa existente por rechaveamento. O cenário exercita Ch destino de pk menor depois de Ch vivo maior. Não exige a Disputa fechada na sequência. (`app/chamados/tests/test_e1_fila.py:173`, `:175`, `:183`, `:191`, `:195`; `app/chamados/tests/test_e1_v9.py:145`; `app/chamados/models.py:2938`; `app/chamados/services/fio.py:768`) |
   | **F1** | Não é vazio: verifica o complemento inicial. Porém o único chamado já tem P pendente; após complementar, ele é eliminado pelo filtro e a porta retorna `None`. Não alcança os laços principais, entradas pendentes ou fecho. O retorno é descartado. (`app/chamados/tests/test_e1_fila.py:229`, `:237`, `:254`; `app/chamados/services/disputa_emissao.py:1006`, `:1022`, `:1029`) |
   | **P3/P6/P7** | Injetam P antes de C e exigem alguma violação; P7 também exige `migradas=3`. Mordem essa inversão, mas não identificam especificamente qual violação matou o mutante. (`app/chamados/tests/test_e1_fila.py:273`, `:282`, `:295`, `:302`) |
   | **F5** | Os testes com datas diferentes distinguem o relógio da borda de `date.today()`. O teste “MESMO instante” devolve uma constante de `now`; duas leituras independentes devolveriam os mesmos valores e passariam. (`app/chamados/tests/test_e1_fila.py:307`, `:319`, `:322`, `:330`) |

   O rastreador observa **UPDATE simples e FOR UPDATE**, elimina reacessos e verifica rank/pk. Não observa `executemany` nem travas de FK; verifica a presença de algum C anterior, sem comprovar que seja o dono de cada linha. `_travar` captura somente a chamada isolada e exclui pks nascidos durante ela. (`app/chamados/tests/test_e1_v9.py:47`, `:68`, `:86`, `:95`, `:109`; `app/chamados/tests/test_e1_fila.py:57`, `:65`)

7. **INFORMATIVA — `colaborador_id=None`: possível em memória; não demonstrado em linha persistida compatível com o esquema.** O FK atual é NOT NULL; a migration preenche o acervo pela Disputa antes de impor essa restrição. Aplicação real dessa migration e estado de produção: [nao verificado]. (`app/chamados/models.py:2431`; `app/chamados/migrations/0051_decisao_pergunta.py:25`, `:101`)

   `_nascer` monta a instância final sem colaborador explícito; o preenchimento acontece em `save → _chave_de_identidade`. Já o stub consultado antes do nascimento e o proxy do juiz recebem `colaborador_id` explicitamente. Não identifiquei nesse caminho uma leitura alterada que dependa do FK ainda ausente. (`app/chamados/models.py:3006`, `:3009`, `:3410`, `:3420`, `:3422`, `:2874`, `:2884`)

   `lavrar_sem_turno_aberto` passa a usar `p.colaborador` como alternativa, mas seu chamador produtivo localizado fornece `colab` explicitamente. (`app/chamados/reconciliador.py:368`, `:1615`)

O mapa dos campos mostra estes escritores, leitores e significados:

| Campo | Quem escreve | Quem lê; significado |
|---|---|---|
| `P.colaborador_id` | `_chave_de_identidade` no nascimento; preenchimento do acervo na migration. (`app/chamados/models.py:3410`, `:3422`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Dedup/fato livre, chave, costura, migração e juiz de destino usam o FK da pergunta. É identidade por pk; os leitores pela Disputa e os aliases ainda sobrevivem. (`app/chamados/models.py:2752`, `:2764`; `app/chamados/services/fio.py:1449`, `:862`, `:2085`; `app/chamados/juizes.py:2003`) |
| `P.chamado_destino` | `_nascer` cria/reusa; `reapontar_resolvedora` recostura. `_costura_nao_mata_o_fio` atribui temporariamente e restaura. (`app/chamados/models.py:2975`, `:3006`; `app/chamados/services/fio.py:921`, `:779`, `:783`) | Materialização, crédito destino-ou-pai e guardas do fio. É FK opcional para o chamado resolvedor, sujeita ao juiz de dono/turno/estado. (`app/chamados/services/materializacao.py:339`; `app/chamados/services/raiox.py:54`, `:60`; `app/chamados/services/fio.py:906`, `:908`) |
| `P.julgado_em` | `lavrar`; backfill atribui e persiste pelo gravador em lote. (`app/chamados/veredito_lavrado.py:243`, `:221`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`) | Stale, varredura e foto da lavra. É **instante**, comparado estritamente com marcos posteriores; a conversão para dia causa a divergência do achado 2. (`app/chamados/veredito_lavrado.py:193`, `:199`, `:203`, `:252`; `app/chamados/services/raiox.py:41`, `:44`) |
| `P.janela_expira_em` | `lavrar` e backfill, mediante `expira_em`; restauração da foto apenas em memória. (`app/chamados/veredito_lavrado.py:244`, `:260`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`) | Stale, varredura e comparação do backfill. É **date**, primeiro dia inválido a partir do probe: validade termina exclusivamente nessa data; stale usa expiração inclusiva, `<= hoje`. Para fato já expirado, a função pode devolver o próprio dia do ato. (`app/chamados/veredito_lavrado.py:126`, `:130`, `:196`; `app/chamados/services/raiox.py:41`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`) |
| `P.data_evento` | Atribuição central em `gravar_dia_do_fato`; nascimento, costura que data e rechaveamento chamam esse ponto. (`app/chamados/services/fio.py:800`, `:878`, `:1027`; `app/chamados/models.py:3008`) | Dedup, identidade, materialização, juiz do chamado e expiração. **Já tem dois significados discriminados:** dia do turno para fato; dia civil local do pedido para conversa. (`app/chamados/models.py:2781`, `:2794`, `:2799`; `app/chamados/services/materializacao.py:345`; `app/chamados/juizes.py:75`; `app/chamados/veredito_lavrado.py:244`) |
| `D.fechada_em` | `fechar` carimba; reaberturas administrativa/sistêmica limpam. (`app/chamados/models.py:1930`, `:1934`, `:2147`, `:2171`) | Pré-trava, emissão e signals. É **instante de fechamento**; NULL significa aberta. (`app/chamados/models.py:2671`; `app/chamados/services/disputa_emissao.py:531`; `app/chamados/signals.py:28`, `:129`, `:153`) |
| `D.motivos` | Emissão agrega/cria; agrupador complementa; modelo apara/remove e reabertura administrativa recompõe. (`app/chamados/services/disputa_emissao.py:541`, `:589`, `:1019`, `:1191`; `app/chamados/models.py:1964`, `:2061`, `:2153`) | Emissão, complemento e remoção sob trava. É lista de identificadores de motivos; sua inclusão pode preceder as guardas que decidem se nasce pergunta. (`app/chamados/services/disputa_emissao.py:536`, `:1017`, `:825`; `app/chamados/models.py:2029`) |
| `Ch.status_local` | Nascimento e `_gravar_estado`, que salva e dispara signals. (`app/chamados/models.py:971`, `:1095`, `:228`, `:296`) | Catálogo/transições, pré-trava dos vivos, costura e signals. É estado categórico; vivo/encerrado/terminal vêm dos conjuntos do catálogo. (`app/chamados/models.py:803`, `:2681`; `app/chamados/services/fio.py:755`, `:770`; `app/chamados/signals.py:70`, `:96`) |

O mapa das funções também exige distinguir chave gravada, dia julgado e instante:

| Função | Contrato e consumidores |
|---|---|
| `PerguntaDisputa.trava_na_ordem` | Ignora None/repetidos; ordena cada tabela por pk; adquire C→D→Ch→P. Não ordena retrospectivamente as travas do chamador. É usada pelas portas e pelos dois helpers novos. (`app/chamados/models.py:2650`, `:2664`, `:2673`, `:2686`, `:2690`; `app/chamados/services/declaracao_texto.py:293`; `app/chamados/services/disputa_emissao.py:954`) |
| `fio.universo_do_fato` | Parte de `do_fato`, filtra pelo dia real do turno e fecha o componente; serve a `_nascer`, `_candidata_reusavel` e à nova pré-trava da declaração. Não é simplesmente igualdade da data gravada. (`app/chamados/services/fio.py:1784`, `:1800`, `:1822`; `app/chamados/models.py:2905`; `app/chamados/services/declaracao_texto.py:244`, `:294`) |
| `fio.chave_do_dia` | Retorna `(FK, motivo, data_evento gravada, instancia_marco)`. Seus sete usos internos alimentam identidade/cache/vizinhança; o dia real pode ser obtido depois pelo juiz. (`app/chamados/services/fio.py:1449`, `:1450`, `:1437`, `:1455`, `:1478`, `:1510`, `:1645`, `:1658`, `:1790`) |
| `chamados.juizes.dia_do_fato` | Escada: célula; catálogo/contexto; dia local da batida; menor data das perguntas não conversacionais; finalmente `.date()` do instante cru do contexto. Portanto os degraus fracos não garantem a mesma força semântica de “dia do turno”. O repasse novo usa a escada completa. (`app/chamados/juizes.py:53`, `:75`, `:98`, `:114`, `:129`, `:133`, `:166`; `app/chamados/services/disputa_emissao.py:956`) |
| `veredito_lavrado.expira_em` | Usa limites inclusivos da janela, como o juiz, e devolve o primeiro probe fora dela; consumidores são `lavrar` e backfill. Só distingue ausência de data, não conversa: conversas novas têm data civil não nula. (`app/chamados/veredito_lavrado.py:123`, `:130`, `:244`; `app/chamados/juizes.py:601`; `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/models.py:2799`) |

Para a sequência transacional, **C = Colaborador, D = DisputaSupervisao, Ch = ChamadoColaborador, P = PerguntaDisputa**. Toda aquisição agrupada abaixo usa pk crescente pelo adquirente; UPDATE de linha já pré-travada é reentrante. (`app/chamados/models.py:2650`, `:2664`, `:2673`, `:2686`, `:2690`)

| Porta/atomic | Sequência efetiva |
|---|---|
| `_trava_do_ato` / `declarar_hora_colab` | C; leitura do universo; D abertas + D do fato; Ch vivos/pais das abertas + declarado + destinos do fato; P do fato. Emissão readquire C/D/Ch e atualiza D ou insere Ch/D; depois costura/`abrir`, UPDATE P e resposta. A resposta pode ainda levantar arquivo de destino/pai pela porta de campo. (`app/chamados/services/declaracao_texto.py:293`, `:295`, `:342`, `:354`, `:364`, `:370`, `:413`; `app/chamados/services/disputa_emissao.py:530`, `:565`, `:1434`, `:1436`, `:1465`; `app/chamados/services/materializacao.py:711`, `:718`; `app/chamados/services/arquivo.py:209`) |
| `aplicar_declaracao` | Mesmo prefixo no atomic da emissão/resposta. **A validação está fora desse bloco**; abre outro atomic com C→D→Ch→UPDATE P, seguido da materialização. Sem atomic externo, são transações distintas. (`app/chamados/services/declaracao_texto.py:648`, `:650`, `:687`, `:690`; `app/chamados/services/validacao.py:425`, `:431`, `:435`) |
| `_trava_do_repasse` / agrupador | C; cálculo do teto; D explícitas+abertas; Ch órfãos/destinos/pais/vivos; P selecionadas. Depois complemento/`abrir`, UPDATE D, `analisar` e cascatas; corpo principal; entradas pendentes; possível `fechar`. O teto incompleto permite aquisições novas posteriores, conforme achado 1. (`app/chamados/services/disputa_emissao.py:954`, `:962`, `:988`, `:1017`, `:1020`, `:1021`, `:1206`, `:1220`) |
| `PerguntaDisputa.abrir/_nascer` | `_sob_contrato` abre atomic; `_nascer` readquire C. Reuso com rechaveamento toma C→D→Ch→P antes de rechavear; reuso normal pode tomar Ch pela costura e fazer UPDATE P; nascimento faz INSERT P. (`app/chamados/models.py:2817`, `:2843`, `:2938`, `:2945`, `:2973`, `:2999`, `:3009`) |
| `reapontar_resolvedora` | Sem colisão: C→Ch destino→UPDATE P. Com colisão: C→D→Ch→P do lote, costura, retratação e UPDATE P. O bloco usa `savepoint=False`; sob atomic externo conserva suas travas até o fim dele. (`app/chamados/services/fio.py:880`, `:888`, `:903`, `:908`, `:920`, `:923`) |
| `rechavear_data_evento` / `fato_livre_para` | C→D→P; `fato_livre_para` readquire C; valida destino por leitura; UPDATE P. Não adquire Ch por essa validação. (`app/chamados/services/fio.py:1000`, `:1022`, `:1029`, `:1033`; `app/chamados/models.py:2764`, `:2765`) |
| `retratar_para_canonica` | C→D→P de canônica+perdedoras; UPDATE P pela porta de identidade. Mesmo com perdedoras vazias ainda trava a canônica. (`app/chamados/services/fio.py:1326`, `:1328`, `:1331`; `app/chamados/models.py:3172`, `:3180`) |
| `migrar_resolvedoras_vivas` | Calcula conjunto; C de todos→D do conjunto→Ch velho/novo→P do conjunto; relê; recusa crescimento; reaponta para novo e retrata. A chamada isolada corrige a dupla aquisição de C; não corrige prefixos externos. (`app/chamados/services/fio.py:2084`, `:2086`, `:2088`, `:2091`, `:2160`, `:2166`) |
| Command `_migrar` / `_reverter` | `_migrar` herda atomic do `handle`; `_reverter` abre seu próprio. Ambos pré-travam o lote C→D→Ch→P; chamadas por linha e rechaveamento reencontram essas linhas. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:237`, `:374`, `:389`, `:400`, `:465`, `:470`, `:485`, `:492`) |
| `DisputaSupervisao.remover_motivo` | C; lote C→D→Ch→P; UPDATE P; UPDATE D. O eventual `fechar` ocorre depois do atomic próprio, mas continua na mesma transação se existir atomic externo. (`app/chamados/models.py:2023`, `:2026`, `:2052`, `:2061`, `:2064`) |
| `lavrar_sem_turno_aberto` | Não abre atomic global: carimba P e pode depois fechar D. Em autocommit, o atomic do save P termina antes do atomic de fechamento D; com atomic externo, depende do prefixo dele. (`app/chamados/reconciliador.py:405`, `:411`; `app/chamados/models.py:3412`, `:1903`, `:1908`) |
| `lavrar` / backfill de lavra | Fazem UPDATE P, individual ou em lote, sem pré-trava C/D/Ch própria. `expira_em`, `_hoje`, `chave_do_dia` e `_conjunto_da_migracao` não adquirem essas travas. (`app/chamados/veredito_lavrado.py:95`, `:106`, `:221`, `:246`; `app/chamados/services/fio.py:1442`, `:2032`) |

As cascatas relevantes são: Ch encerrado pode atualizar sua urgência e resolver perguntas; D fechada pode reconciliar o pai e materializar pendentes; mudança de estado de Ch pode chamar o cartório sincronamente. P agenda o fechamento do beco em `on_commit` e emite outbox sincronamente. Não se deve contar o callback pós-commit como se ainda mantivesse as travas da transação encerrada. (`app/chamados/signals.py:72`, `:115`, `:42`, `:163`, `:203`, `:225`, `:244`)

Os chamadores das declarações nas views fazem leitura comum antes da chamada, sem aquisição explícita prévia nesses corpos. A menção a `core/views_ia.py:150` é um homônimo: chama outro `_aplicar_declaracao`, que despacha declaração de dias pelo intake. Transação adicional de middleware/configuração de produção: [nao verificado]. (`app/chamados/views.py:363`, `:480`, `:1046`, `:1090`; `app/core/views_ia.py:150`, `:754`, `:775`)

Foram localizadas seis cha

[truncado em 20000 de 21940 caracteres]

## 08/11 22:24 · sessao 65246f94aba2 · - · - · origem=Ronald (tmux)

Complementos:

- `lavrar_sem_turno_aberto` não abre transação nem trava C/D/Ch/P: carimba P e depois pode fechar D (`app/chamados/reconciliador.py:405-413`). Em autocommit, o `save` de P encerra seu próprio `atomic` (`app/chamados/models.py:3412-3413`) antes do `atomic` de D (`:1903-1910`); portanto não há P→D mantidas nesse caso. Com transação externa, a ordem depende do prefixo do chamador. A emissão chama a porta dentro de `atomic` próprio após criar a pergunta (`app/chamados/services/disputa_emissao.py:459-465`); o reconciliador avulso não abre `atomic` em volta do laço (`app/chamados/reconciliador.py:1520-1525`, `:1615`).

- `remover_motivo` começa com C, depois trava C→D→Ch→P do conjunto (`app/chamados/models.py:2023-2028`); atualiza P (`:2052-2060`) e D (`:2061-2063`) já travadas. `fechar` é chamado após o `atomic` próprio (`:2064-2070`): sem transação externa é outro ato; com externa, conserva as travas anteriores.

- `_migrar` herda o `atomic` do comando (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:237-242`), trava o lote C→D→Ch→P (`:374-378`), e as travas por grupo/linha e o rechaveamento reencontram esse conjunto (`:381-400`). `_reverter` abre um `atomic`, trava o lote inteiro (`:465-473`) e depois retrava por linha/rechaveia (`:485-492`).

- Encontrei seis chamadas diretas de produção ao agrupador: `app/chamados/services/acoes_disputa.py:221`, `app/chamados/services/pergunta_no_app.py:186`, `app/chamados/services/cobrar_dia.py:47`, `app/ponto/management/commands/processar_alertas_avancados.py:158`, `app/ponto/management/commands/processar_alertas_turno.py:384`, `app/ponto/management/commands/detectar_ausencias.py:801`. `app/chamados/models.py:4072` é reexportação; as outras duas chamadas são testes (`app/tenants/tests.py:74`, `:85`). O “sétimo chamador” alegado no texto do commit é [nao verificado].

## 08/11 22:23 · sessao 1ebe59f1dec7 · - · - · origem=Ronald (tmux)

**Sequência das declarações.** Sem atomic externo, `declarar_hora_colab` abre o atomic em `app/chamados/services/declaracao_texto.py:342`; `_trava_do_ato` toma C, calcula o universo e toma D abertas + D do fato, Ch vivos/pais das abertas + destino declarado + destinos do fato, P do fato (`:293-297`). Cada grupo é ordenado por PK pelo adquirente (`app/chamados/models.py:2650-2691`). `disputa_para_declaracao` chama `_abrir_ou_agregar_disputa`, cuja aquisição C/D/Ch é reentrante para as linhas existentes; agrega motivos por UPDATE D ou cria Ch/D (`app/chamados/services/disputa_emissao.py:627-630,530-592`). Depois vêm `_candidata_reusavel`, eventual reapontamento ou `abrir`, resposta e nota (`app/chamados/services/declaracao_texto.py:357-371,413-419`). A resposta readquire C e faz UPDATE P (`app/chamados/services/disputa_emissao.py:1431-1436`); sua cauda pode levantar arquivo no destino/pai, o que toma C/D/Ch pela porta de campo e faz UPDATE Ch (`app/chamados/services/disputa_emissao.py:1464-1465`; `app/chamados/services/materializacao.py:710-722`; `app/chamados/services/arquivo.py:209-210`). O fim do atomic próprio é `app/chamados/services/declaracao_texto.py:419`; callbacks `on_commit` de P ficam para depois do commit externo (`app/chamados/signals.py:214-225`).

**`aplicar_declaracao` tem duas fases transacionais.** O primeiro atomic começa em `app/chamados/services/declaracao_texto.py:648`, toma o mesmo conjunto (`:650`), emite/reusa disputa e pergunta e responde (`:653-661,681-689`). `validar_pergunta` está **fora** desse bloco (`:690`). A validação abre outro atomic e toma C → D abertas + D da pergunta → Ch vivos/pais/destino/clones, depois UPDATE P pelo carimbo; a materialização individual ocorre dentro desse bloco (`app/chamados/services/validacao.py:425-466`). O lote da disputa é chamado **fora** desse atomic (`:469`) e percorre todas as perguntas prontas antes do fecho (`app/chamados/services/materializacao.py:1369-1379`). Se há atomic externo do chamador, os blocos continuam na mesma transação externa; sem ele, não se deve juntar essas aquisições numa única sequência contínua (`app/chamados/services/declaracao_texto.py:648-690`; `app/chamados/services/validacao.py:425-469`).

**Chamadores confirmados.** As views leem o Chamado com `get_object_or_404` e chamam diretamente as portas; não há aquisição prévia explícita nesses corpos (`app/chamados/views.py:359-363,477-481,1041-1046,1089-1091`). A referência `core/views_ia.py:150` é homônimo: despacha `declarar_folga`/`declarar_falta` para `_aplicar_declaracao(request, proposta)`, que chama `core.intake.escritor_de(acao)` com dias/tipo, não a porta de horário tocada (`app/core/views_ia.py:149-150,754-776`). Transação adicional introduzida por configuração/middleware em produção [nao verificado].

**Achados prioritários dos testes.** [nao verificado]

1. **Média — A3 acusa motivo mais amplo do que demonstra.** Com o mesmo `DIA` na fixture e no pedido, `_nascer` pula a aquisição de D existente condicionada à mudança de dia (`app/chamados/tests/test_e1_fila.py:175,183-185`; `app/chamados/tests/test_e1_v9.py:145-147`; `app/chamados/models.py:2938-2948`). O mutante morde o Ch destino de PK menor depois do Ch vivo maior, e não necessariamente D fechada depois de Ch (`app/chamados/tests/test_e1_fila.py:173-178,195-199`; `app/chamados/services/fio.py:762-769`).

2. **Média — F1 termina no complemento inicial.** A fixture fornece contexto sem `marcos_faltantes` e P viva costurada (`app/chamados/tests/test_e1_fila.py:229-246`); `cobra_marcos_da_ata` é falso (`app/chamados/services/disputa_emissao.py:732-735`; `app/ponto/services/lastro.py:74-79`). Após complementar, o filtro elimina o único Ch por já ter P não materializada e retorna `None` (`app/chamados/services/disputa_emissao.py:1004-1030`). Não cobre a cauda da porta (`:1032-1223`); `_r` é ignorado (`app/chamados/tests/test_e1_fila.py:254,264`).

3. **Alta — O teto do repasse é incompleto.** Pré-trava somente dias dos órfãos ±1/nulos e perguntas já costuradas (`app/chamados/services/disputa_emissao.py:955-963`), mas a cauda processa dias arbitrários de `entradas_pendentes` (`:1206,273-296`). Pode reusar e atualizar P canônica antiga de PK menor fora do teto, depois das P já pré-travadas (`:443-451`; `app/chamados/models.py:2905-2919,2958-2999`). Ocorrência em prod [nao verificado]; F1 não alcança essa cauda (`app/chamados/services/disputa_emissao.py:1029-1030`).

4. **Média — A1 passa com derivador equivalente sobrevivente.** O detector busca sintaxe `.disputa.colaborador` como alternativa de `or`, sem aliases (`app/chamados/tests/test_e1_fila.py:76-93`); `fecho_cruza_dias` ainda lê ambas as colunas e usa `c or dc` (`app/chamados/services/fio.py:1844-1847`).

5. **Baixa — F5 não prova uma única leitura de relógio.** O teste fixa `now` em constante e compara somente valores finais; duas leituras independentes passariam (`app/chamados/tests/test_e1_fila.py:322-328`). O código atual efetivamente repassa `_agora` (`app/chamados/veredito_lavrado.py:242-244`).

6. **Média — Cobertura de ordem não equivale à transação inteira.** `_travar` captura apenas a chamada isolada e filtra linhas novas (`app/chamados/tests/test_e1_fila.py:57-69`); A2/A3 não exercitam prefixos de chamadores, a porta irmã, nem universo crescendo entre leitura e trava (`:147-160,181-199`). O rastreador inclui UPDATE simples e FOR UPDATE, mas ignora `executemany` e travas de FK; identifica presença de algum C, sem conferir o dono da linha (`app/chamados/tests/test_e1_v9.py:47-53,68-90,95-116`).

Execução dos testes e resultado GREEN [nao verificado]. [nao verificado]

_2928 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
