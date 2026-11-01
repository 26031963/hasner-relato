# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 21:56** · publicado em **31/10/2026 21:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 21:56 · sessao 3ff5b8f0a0eb · - · - · origem=Ronald (tmux)

Não localizei teste que exercite **`sem_ancora=True` com ausência da chave `(colab, dia)` em `celulas_do_periodo`**. A lacuna está registrada explicitamente como obra aberta em `app/docs/BACKLOG.md:832`: “dia sem ancora e sem celula deixou de ser morto sem caso na bateria”.

Os testes próximos usam **célula existente**: [nao verificado]

- `test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py` simula grade indefinida (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:28`–34), mas `_rodar` sempre cria `CelulaDia(trabalha=None, ata=ata)` (`:55`) antes de executar `handle` (`:58`). Os testes cobrem ata de trabalho (`:61`–65), `ata=None` (`:67`–70) e ata de folga (`:72`–74). **“Sem ata” não significa “sem CelulaDia”** (`:55`, `:68`).
- `test_cobranca_em_dia_indefinido_fica_viva_limbo_cobra` também simula `sem_ancora=True` (`app/ponto/tests/test_a2_marco_morto.py:26`–32), mas cria expressamente `CelulaDia(trabalha=None)` (`:46`). Com `apply=True` (`:50`), verifica que `retratar` nem foi chamado (`:52`) e que o chamado permanece vivo (`:54`–55).
- `test_plano_declara_sem_ancora` apenas inspeciona o texto do comando e exige `SEM_ANCORA`/`sem_ancora_dias`; não monta um caso com chamado e ausência de célula (`app/ponto/tests/test_reconciliar_sem_ancora_visivel.py:11`–18).

Em `app/chamados/tests`, os casos encontrados ligados ao comando não exercitam esse cenário:

- O teste de `_cham_dts` cria célula com `trabalha=True` e verifica a data recuperada do vínculo do chamado com ela (`app/chamados/tests/test_c5_emissores.py:39`–51).
- O selo do cron verifica ausência de `--apply`, sem executar o julgamento de dia indefinido (`app/chamados/tests/test_hx_um_emissor.py:5`–9).
- Os testes das portas chamam auxiliares `_relavrar_orfa` e `_disputa`, não `Command.handle` (`app/chamados/tests/test_porta_chamado_e2.py:62`–72; `app/chamados/tests/test_porta_chamado_e3.py:92`–94).

Há teste de **ausência de célula em outro escopo**: `GateCelulaTest` deixa `d_sem` sem criação correspondente (`app/ponto/tests/test_t31_gate_celula.py:16`–25) e verifica `celula_veta_cobranca(..., d_sem) is None` (`:32`–35). Ele não executa `reconciliar_grade.handle` nem testa `_indef`/`mortos` (`:32`–35).

O comentário dos testes descreve a regra anterior como `sem_ancora -> mortos -> RETRACT_INDEFINIDO` (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:4`–7); a correspondência byte a byte com o **pai de `9651ef38`** continua `[nao verificado]`, pois não encontrei histórico Git ou reprodução desse pai na cópia.

## 31/10 21:56 · sessao c686af4f9bc0 · - · - · origem=Ronald (tmux)

Não encontrei a versão histórica nem referência a `9651ef38` na cópia; o conteúdo exato e as linhas do pai de `9651ef38` ficam **[nao verificado]**.

Há duas evidências locais sobre a mudança:

- `app/docs/BACKLOG.md:832`: registra explicitamente **“reconciliar_grade.py:281-282, dia sem ancora e sem celula deixou de ser morto sem caso na bateria”**.
- `app/ponto/management/commands/reconciliar_grade.py:277`: comentário histórico informa **“Ate aqui `sem_ancora` ia inteiro para `mortos`”**.

Na cópia atual, `_indef` exige célula existente antes de perguntar ao juiz: `app/ponto/management/commands/reconciliar_grade.py:281-282`. Sem chave no mapa, `.get(...)` retorna `None`, o primeiro operando falha e o dia não entra em `_indef`; portanto não entra em `mortos` **por esse caminho** (`app/ponto/management/commands/reconciliar_grade.py:283`).

O teste citado pelo comentário não cobre ausência de célula: `_rodar` sempre cria uma `CelulaDia`, inclusive quando `ata=None` (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:55`, `:67-68`).

## 31/10 21:56 · sessao 31b27f047daf · - · - · origem=Ronald (tmux)

Leitores e fluxo do resultado na cópia:

- `RETRACT_INDEFINIDO` incrementa quando todas as datas estão em `mortos` e `_indef` é não vazio, **antes** do veto por `premissa_morta`; portanto pode subir sem gerar `RETRACT`. `RETRACT` e `retracts.append` só acontecem após esse veto. (`app/ponto/management/commands/reconciliar_grade.py:292–304`)
- Com `--apply`, chama `chamados.reconciliador.retratar`; a mensagem depende de `_indef`. (`app/ponto/management/commands/reconciliar_grade.py:305–316`)
- O stdout imprime todos os contadores em `PLANO`, o tamanho de `retracts` e até 40 itens da lista. (`app/ponto/management/commands/reconciliar_grade.py:318–325`)
- O tripwire usa **`len(creates) + len(retracts)`**, não soma diretamente `RETRACT_INDEFINIDO`; imprime também `create`, `retract`, `orfa_dry`, esperado e dono. (`app/ponto/management/commands/reconciliar_grade.py:337–342`)
- O cron configurado roda às **06:20**, com `--empresa 2,3,4`, sem `--apply`; o log padrão é `reconciliar_grade.log`, com stdout/stderr redirecionados pelo renderizador. (`app/config/crons.py:443`, `app/config/crons.py:32`, `app/config/crons.py:1801–1802`)
- O leitor do contador é `bin/placar_code.sh`: extrai o último `divergencia_grade_cartorio` do log, exige log modificado hoje e publica `divergencia_grade_x_cartorio`; esse valor não participa do `TOTAL` do Code. (`bin/placar_code.sh:528–532`)
- O envelope do cron acompanha o **exit code**, não interpreta `RETRACT`/tripwire; registra execução e aciona `cron_quebrado` nas falhas. (`bin/cron_run.sh:11–13`, `bin/cron_run.sh:27–38`)
- Selos que leem o stdout: `test_reconciliar_grade_sombra` verifica existência e contrato completo do tripwire; `test_f1c4_3_reconciliar_grade_pergunta_ao_juiz` verifica ausência/presença de `RETRACT_INDEFINIDO` e divergência zero. (`app/ponto/tests/test_reconciliar_grade_sombra.py:71–88`, `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:61–74`)
- O selo do cron cobra ausência de `--apply`. (`app/chamados/tests/test_hx_um_emissor.py:5–9`)

Execução real do cron, conteúdo de log e valor do contador em produção: `[nao verificado]`.

## 31/10 21:56 · sessao 8dc87397ccfa · - · - · origem=Ronald (tmux)

Leitura concluída sem alterações. Não encontrei `AGENTS.md` no resultado de `rg --files -g AGENTS.md`; instruções externas a esse conjunto [não verificado].

**Chamadores e consumidores de rc/stdout**

| Chamador | Fluxo atual e dependência |
|---|---|
| `suite.sh` | Faz `exec env ESTEIRA_QUEM=... bash trava_teste.sh`, herdando stdout/stderr e devolvendo o rc pelo próprio processo: `bin/suite.sh:96`. Não declara obra/volta nesse ponto: `bin/suite.sh:96`. |
| Régua | Suite escreve em `$LOG`; lê `$?`; só permite 0/1 julgar e devolve outros códigos sem carimbo; juiz lê log+rc: `bin/regua.sh:165`, `bin/regua.sh:170`, `bin/regua.sh:175`, `bin/regua.sh:180`. |
| Pre-push | Captura stdout/stderr em arquivo; guarda rc verdadeiro; imprime filtro só depois de terminar; devolve `_rc`: `bin/pre-push.sh:142`, `bin/pre-push.sh:145`, `bin/pre-push.sh:146`, `bin/pre-push.sh:157`. Portanto não exige stdout ao vivo nesse trecho. |
| Vigia da árvore | Captura stdout/stderr em log; lê rc e passa ambos ao juiz nos três runs: `bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:86`, `bin/vigia_arvore.sh:99`, `bin/vigia_arvore.sh:113`, `bin/vigia_arvore.sh:132`, `bin/vigia_arvore.sh:137`. |
| Régua relógio | Pipeline `trava ... 2>&1 | tee ... | grep ...`; captura `PIPESTATUS[0]` e julga log+rc: `bin/regua_relogio.sh:51`, `bin/regua_relogio.sh:56`, `bin/regua_relogio.sh:57`, `bin/regua_relogio.sh:62`. É leitor de fluxo incremental via `tee`/filtro; necessidade operacional de latência específica [não verificado]. |
| Régua calendário | Cada run escreve log; lê rc; 75 retorna 75; restantes passam pelo juiz: `bin/regua_calendario.sh:46`, `bin/regua_calendario.sh:50`, `bin/regua_calendario.sh:51`, `bin/regua_calendario.sh:56`. |
| Isolamento | Só preserva 75; para qualquer outro rc decide pelo último `OK` do log: `bin/isolamento.sh:33`, `bin/isolamento.sh:37`, `bin/isolamento.sh:38`. Contraexemplo: log contém `OK`, job morre 143; `_iso_roda` pode devolver 0 e chamador declarar “verdes sem a fatia”: `bin/isolamento.sh:49`, `bin/isolamento.sh:50`. |
| Placar | `trava ... | sed ... | head -1` dentro de substituição; ignora rc e lê contador; ausente vira `nao_medido`: `bin/placar_code.sh:125`, `bin/placar_code.sh:132`, `bin/placar_code.sh:133`. `head` pode encerrar leitura antes do fim; preservação do job exige relay tolerar consumidor fechado [não verificado quanto à implementação proposta]. |
| Molde fatia | Chamadas em `bin/molde_fatia/rodar.sh:30` e `bin/molde_fatia/rodar.sh:44`; só distingue 75, depois julga `OK` no arquivo: `bin/molde_fatia/rodar.sh:34`, `bin/molde_fatia/rodar.sh:35`, `bin/molde_fatia/rodar.sh:53`, `bin/molde_fatia/rodar.sh:55`, `bin/molde_fatia/rodar.sh:56`. |
| Molde autocura | Chamada em `bin/molde_autocura/rodar.sh:22`; só distingue 75; publica fim para outros rc: `bin/molde_autocura/rodar.sh:28`, `bin/molde_autocura/rodar.sh:32`, `bin/molde_autocura/rodar.sh:33`. |
| `_rodar_testes` | `subprocess.run` espera processo, escreve saída em arquivo e usa rc+conteúdo no juiz: `app/core/esteira_vigia.py:794`, `app/core/esteira_vigia.py:802`, `app/core/esteira_vigia.py:803`, `app/core/esteira_vigia.py:805`, `app/core/esteira_vigia.py:807`. |
| Integrador | Wrapper herda locks fd7/fd8 e chama Python: `bin/integrador.sh:41`, `bin/integrador.sh:44`, `bin/integrador.sh:51`. `_suite` espera régua, lê rc, busca caminho anunciado no trecho novo de stdout e julga log específico: `app/core/integrador_lote.py:379`, `app/core/integrador_lote.py:380`, `app/core/integrador_lote.py:385`, `app/core/integrador_lote.py:388`, `app/core/integrador_lote.py:390`. Não usa mais “log mais novo”. |
| Cron | Geração envelopa comando em `cron_run.sh` e redireciona saída para log: `app/config/crons.py:1431`, `app/config/crons.py:1791`. Runner espera `bash -c`, mede duração/rc, grava fim só em 0/2 e alarma outros rc: `bin/cron_run.sh:11`, `bin/cron_run.sh:13`, `bin/cron_run.sh:22`, `bin/cron_run.sh:32`. Régua relógio/calendário no crontab instalado [não verificado]; entrada de vigia existe em `app/config/crons.py:277`. |

**Quebras concretas/condicionais por destaque**

1. **Selo atual quebra diretamente.** Régua executa todos os selos de host antes da suite e aborta se qualquer falhar: `bin/regua.sh:138`, `bin/regua.sh:140`. Selo exige que TERM no PID da chamada mate container e produza rc143: `bin/tests/test_trava_dono_para_filho.sh:54`, `bin/tests/test_trava_dono_para_filho.sh:60`, `bin/tests/test_trava_dono_para_filho.sh:62`. Também exige KILL da chamada deixar ORFAO: `bin/tests/test_trava_dono_para_filho.sh:68`, `bin/tests/test_trava_dono_para_filho.sh:74`, `bin/tests/test_trava_dono_para_filho.sh:77`. Contrato faz esse PID virar esperador cuja morte preserva job; selo precisa mudar junto, ou régua fica vermelha antes de testar.

2. **Recursos temporários seguem pertencendo ao chamador.** Pre-push cria archive e registra `rm -rf` no EXIT: `bin/pre-push.sh:39`, `bin/pre-push.sh:40`; job monta aquele archive: `bin/pre-push.sh:144`. Se esperador morrer antes da aquisição, hook sair pelo rc/sinal e executar EXIT, archive pode sumir antes do `docker run`; execução desse sinal/EXIT específico em produção [não verificado]. Mesmo problema estrutural para calendário com `$TMP/app` (`bin/regua_calendario.sh:26`, `bin/regua_calendario.sh:27`, `bin/regua_calendario.sh:48`), relógio com shim em `$TMP` (`bin/regua_relogio.sh:31`, `bin/regua_relogio.sh:32`, `bin/regua_relogio.sh:54`) e fallback efêmero do vigia (`bin/vigia_arvore.sh:23`, `bin/vigia_arvore.sh:58`, `bin/vigia_arvore.sh:85`).

3. **Locks extras não podem ser herdados sem decisão.** Integrador segura fd7 `/tmp/regua.lock`, fd8 temporário de push e locks da árvore via `trava.sh`: `bin/integrador.sh:32`, `bin/integrador.sh:41`, `bin/integrador.sh:51`; árvore é fd8 e raia fd6: `bin/trava.sh:60`, `bin/trava.sh:64`. Destaque preservando FDs pode manter locks depois do dono morrer; fechando-os pode deixar job lendo árvore já livre para outro escritor. Política do contrato para esses FDs [não verificado].

4. **Restauração da árvore é ligada ao chamador.** Cadeia instala trap EXIT que faz checkout/remove arquivos e recarrega aplicação: `bin/molde_fatia/cadeia.sh:25`, `bin/molde_fatia/cadeia.sh:27`, `bin/molde_fatia/cadeia.sh:29`, `bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:35`; régua é chamada depois: `bin/molde_fatia/cadeia.sh:51`. Se cadeia sai enquanto teste destacado continua, a árvore montada pode mudar sob o job; proteção contratual [não verificado].

5. **Matador da esteira não alcança nova sessão automaticamente.** Vigia descobre grupos dos detentores do lock e manda TERM/KILL para eles: `app/core/esteira_vigia.py:1026`, `app/core/esteira_vigia.py:1031`, `app/core/esteira_vigia.py:1034`, `app/core/esteira_vigia.py:1037`. Job criado em nova sessão/grupo pode sobreviver a esse desligamento; se herdar o lock, o `fuser` pode descobri-lo também. FDs herdados pelo contrato [não verificado].

6. **Unidade do integrador já permite descendentes sobreviverem ao principal.** `KillMode=process`: `bin/hasner-integrador.service:10`. Portanto `setsid` não resolve por si só protocolo de fim, recursos e locks; serviço instalado [não verificado].

7. **Consumidores antigos ainda aceitam resultado textual sem rc.** Cadeias ignoram rc da régua e escolhem `/tmp/regua_*.log` mais novo, verificando `Ran`/`OK`: `bin/molde_fatia/cadeia.sh:51`, `bin/molde_fatia/cadeia.sh:53`, `bin/molde_fatia/cadeia.sh:54`, `bin/molde_fatia/cadeia.sh:55`; autocura idem `bin/molde_autocura/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:50`. Rc143 correto do relay não basta para esses leitores.

**Leitores complementares**

- Handoff mede somente `flock`, não `.quem` nem fila: `bin/handoff_sessao.sh:223`, `bin/handoff_sessao.sh:230`. Fila existente com lock momentaneamente livre pode aparecer LIVRE; adaptação prevista [não verificado].
- Placar lê contador de esperas longas da sombra: `bin/placar_code.sh:155`, `bin/placar_code.sh:158`.
- Leitor direto externo de `.quem` não foi encontrado; na trava ele é lido em `bin/trava_teste.sh:63`, `bin/trava_teste.sh:80`, `bin/trava_teste.sh:98` e escrito em `bin/trava_teste.sh:102`, `bin/trava_teste.sh:103`. Escritor deve passar ao job para o esperador morto não publicar “livre” enquanto ele roda.

## 31/10 21:56 · sessao 7f44a2164616 · - · - · origem=Ronald (tmux)

Achados da sombra, somente por leitura:

- `trancar_sombra` é uma função carregada por `source`; ela abre fd 8 no próprio chamador, espera `flock`, exporta `SOMBRA_TRAVA_HERDADA=1` e `SOMBRA_TRAVA_ID`, e retorna 0 para o chamador executar a continuação. Ela não recebe um comando que possa destacar: `bin/sombra_trava.sh:21-39`. Os chamadores carregam a função em `bin/sombra.sh:87`, `bin/simular_folha.sh:45`, `bin/diff_janela_he_total.sh:17`.
- O caminho herdado retorna 0 apenas pela variável `SOMBRA_TRAVA_HERDADA=1`, sem conferir fd 8, identidade ou posse: `bin/sombra_trava.sh:22`. Logo, uma adaptação que preserve a variável mas feche/perca fd 8 executaria continuação sem adquirir a trava; uma adaptação que remova a herança tentaria adquirir novamente enquanto o pai ainda detém a trava: consequências condicionais dessa lógica, `bin/sombra_trava.sh:22-28`.
- Destacar somente a função que adquire a trava não basta: os trabalhos protegidos permanecem nas continuações `bin/sombra.sh:438-450`, `bin/sombra.sh:451`, `bin/sombra.sh:456-461`, `bin/sombra.sh:487-492`, `bin/simular_folha.sh:67-77`, `bin/simular_folha.sh:96-101`, `bin/diff_janela_he_total.sh:18-42`. O contrato não especifica como transferir essas continuações para o job destacado: `[nao verificado]`.

Dependências de herança e identidade:

- `simular_folha par` adquire uma trava e executa, sequencialmente, as duas fotos e a comparação; cada falha de foto interrompe o par e o processo retorna o rc da comparação: `bin/simular_folha.sh:96-101`.
- Cada foto escreve `${SOMBRA_TRAVA_ID}` no arquivo `<rotulo>.trava` após sucesso do container: `bin/simular_folha.sh:73-77`. A comparação lê os dois arquivos e sai 5 se os IDs divergem ou são `?`: `bin/simular_folha.sh:82-85`. Duas fotos destacadas independentemente, com IDs novos, quebrariam esse contrato mesmo se executadas sem concorrência: consequência de `bin/sombra_trava.sh:37-38` e `bin/simular_folha.sh:82-85`.
- `diff_janela_he_total` também grava e imprime o ID da trava: `bin/diff_janela_he_total.sh:41-42`; não localizei outro leitor desse arquivo na árvore pesquisada `[nao verificado]`.
- `sombra.sh --com-a-sombra` executa o comando arbitrário e devolve seu rc explicitamente: `bin/sombra.sh:452-461`. `sombra.sh --rodar` passa saída por `grep` e `tee`, escreve `logs/<rotulo>.out`, depois imprime o caminho: `bin/sombra.sh:490-493`. Seu último `echo` pode encobrir o rc da pipeline atual: `bin/sombra.sh:492-493`.

Cancelamento e containers:

- A implementação atual da sombra não associa seus containers a um rótulo de dono nem define `_parar`: receitas `docker run` em `bin/sombra.sh:182-192`, `bin/simular_folha.sh:47-57`, `bin/diff_janela_he_total.sh:30-40`; a trava atual só abre fd/espera/retorna: `bin/sombra_trava.sh:21-39`.
- Portanto, “sombra idem” do contrato 5 exige nova associação dos containers ao job e parada efetiva deles; matar apenas o grupo do shell não está demonstrado como suficiente `[nao verificado]`. A distinção de parentesco do container com o daemon já está documentada e testada para a outra trava em `bin/tests/test_trava_dono_para_filho.sh:4-14`.
- O bloco executa múltiplos comandos dentro de um container, escreve rc/duração individual em `resultado.tsv`, e só depois consolida e carimba o resultado: `bin/sombra.sh:351-407`. Um cancelamento antes da consolidação deixa esse trabalho incompleto por esse fluxo: `bin/sombra.sh:385-407`.

Leitores que precisam preservar:

- A trava atual escreve `logs/sombra_espera.jsonl` com `ts`, `quem`, `espera_s`, `ok`, `timeout_s`, `pid`: `bin/sombra_trava.sh:15-18`, `bin/sombra_trava.sh:30-31`. `--esperas-longas` lê esse formato, conta ilegíveis como problema e imprime `?` se o arquivo não existe: `bin/sombra_trava.sh:42-60`.
- `placar_code.sh` consome essa saída, extrai o número ou `?`, e converte `?` em 1: `bin/placar_code.sh:155-160`. Substituir esse log por `<trava>.fila.jsonl` sem manter/migrar o leitor altera a medição: consequência desses sítios.
- `handoff_sessao.sh` pergunta diretamente ao `flock` das duas travas e imprime LIVRE/TOMADA: `bin/handoff_sessao.sh:223-234`.
- `disjuntor.diff_apply_em_curso` pergunta ao flock da sombra; inexistente = livre; erro = em curso: `bin/disjuntor.py:307-326`. Essa decisão é lida em `bin/disjuntor.py:1186`, `bin/disjuntor.py:1211`, `bin/disjuntor.py:1435`.
- O hook de Stop usa o mesmo leitor para impedir `/clear` durante DIFF/apply: `bin/hook_stop_fila1.py:785-795`. Perder fd 8 antes do fim do trabalho faria esses leitores observarem uma trava livre durante trabalho ainda ativo: consequência dos sítios acima.

Cron e destacamento:

- O cron da sombra encadeia `--refazer && --bloco`: `app/config/crons.py:331-333`. Um esperador que continue vivo, espere o trabalho e retorne o mesmo rc preserva esse encadeamento; a implementação futura disso é `[nao verificado]`.
- O envelope cron cria a marca de “em curso”, remove-a no EXIT, espera `bash -c`, e mede seu fim/rc: `bin/cron_run.sh:9-13`; publica arquivo de fim apenas para rc 0 ou 2: `bin/cron_run.sh:22-25`; abre/fecha alarme pelos mesmos resultados: `bin/cron_run.sh:29-35`.
- Se o esperador/shell morrer e o job de `--refazer` sobreviver, o trabalho prossegue fora da duração/marca do envelope; o `&& --bloco` do shell morto não se executa: consequência de `app/config/crons.py:332`, `bin/cron_run.sh:10-13` e da sobrevivência proposta no contrato 2. O estado real do cron de produção é `[nao verificado]`.

Não encontrei AGENTS.md nos ancestrais consultados nem na listagem da cópia; presença fora desses locais `[nao verificado]`.

## 31/10 21:55 · sessao 901e2a5609d1 · - · - · origem=Ronald (tmux)

Achados só por leitura; não executei selos, pois eles criam/removem arquivos (`bin/tests/test_trava_teste.sh:156-182`, `bin/tests/test_trava_dono_para_filho.sh:33-43`).

- **PreToolUse atual:** já extrai o texto de todo Bash em `bin/disjuntor.py:1430`, mas retorna imediatamente para sessão sem `agent_id`, ferramenta diferente de Agent e comando sem `codex_pergunta` (`bin/disjuntor.py:1431-1432`). Os ramos posteriores só fazem B1, Codex/B3/B4-CODEX e Agent/B4; não há regra para Docker/suite (`bin/disjuntor.py:1434-1475`). O teste atual inclusive declara que Bash da sessão principal passa (`bin/tests/test_disjuntor.sh:265`).
- **Mudança necessária nas linhas 1/3 do contrato:** a checagem suite deve executar antes daquele retorno; apenas acrescentá-la depois deixa escapar a sessão principal (`bin/disjuntor.py:1431-1432`). O matcher já é vazio, portanto a configuração chama o hook sem selecionar somente Bash (`.claude/settings.json:62-68`, `bin/disjuntor_settings.json:5-11`). A configuração aponta para `/home/ronald/saas-hasner`, não para esta cópia; qual código foi efetivamente carregado na produção [não verificado].
- **Desligamento e falhas:** `guarda_chave` executa antes de tudo; com chave presente, `pretool` inteiro é pulado (`bin/disjuntor.py:1486-1488`). Exceção fora do ramo B3 libera a ferramenta (`bin/disjuntor.py:1496-1506`). A proposta não define como o novo bloqueio interage com esses caminhos [não verificado].
- **Leitor indireto da sombra:** `diff_apply_em_curso` lê `logs/sombra.lock` e tenta `flock` compartilhado, sem ler `.quem` (`bin/disjuntor.py:66`, `bin/disjuntor.py:307-326`); B1 só age quando esse leitor diz livre (`bin/disjuntor.py:1434-1439`). Se a implementação destacada soltar a trava antes do trabalho efetivo acabar, esse leitor passa a considerar o DIFF terminado — inferência de `bin/disjuntor.py:318-324`.

O selo estático já trabalha por comando lógico Shell e por unidade AST Python; ele é executado pela régua, que percorre todos os `bin/tests/test_*.sh` e interrompe ao primeiro erro (`bin/tests/test_trava_teste.sh:48-62`, `bin/tests/test_trava_teste.sh:84-122`, `bin/regua.sh:138-143`). O universo inclui `.sh` recursivamente em `bin/` e Python em `bin/`/`app/`, excluindo diretórios `tests` e arquivos `test_*`; outras extensões e comandos interativos não entram nesse scanner (`bin/tests/test_trava_teste.sh:125-133`).

Limites demonstráveis por inspeção do selo, sem afirmar exploração real:

- Em Shell, uma função é considerada travada se seu corpo **contém o texto** `trava_teste.sh`; uma chamada cujo primeiro token seja essa função é aprovada (`bin/tests/test_trava_teste.sh:83-91`). Não demonstra que todos os caminhos da função executam a trava.
- Em Python, basta uma constante terminando em `trava_teste.sh` na mesma unidade da constante com `manage.py test`; não verifica fluxo entre elas (`bin/tests/test_trava_teste.sh:114-122`).
- O reconhecedor exige o literal `manage.py test` com um espaço; composição dinâmica do comando ou espaçamento diferente não tem cobertura demonstrada (`bin/tests/test_trava_teste.sh:44`, `bin/tests/test_trava_teste.sh:85`, `bin/tests/test_trava_teste.sh:117`).
- Uma futura regra que só procure nomes no texto inteiro do Bash pode aprovar `echo suite.sh; docker run … manage.py test …`; isso é contraexemplo da interpretação textual do contrato, implementação futura [não verificado]. [nao verificado]

**Incompatibilidade concreta com destaque:** o selo atual lança a trava em background, dá TERM ao PID retornado por `$!` e exige container parado e rc 143 (`bin/tests/test_trava_dono_para_filho.sh:54-63`). Aquele PID virar ESPERADOR cuja morte não mata o job contradiz o teste. Sem adaptar o selo, a régua reprovaria a mudança, pois executa esse arquivo (`bin/regua.sh:138-143`). O teste de KILL também espera que o mesmo PID seja o dono e que sua morte produza container órfão (`bin/tests/test_trava_dono_para_filho.sh:68-80`).

## 31/10 21:53 · sessao d3273d2284fb · PADRAO-DE-CIMA · verificador · origem=Code

**REPROVADO.** Permanecem caminhos de mordida falsa, atribuição falsa ao prazo e dois leitores fora da cura: o primeiro leitor de modelos e o consumidor de prova de `ESPERANDO`. (`bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:293`; `app/core/censo_escritas.py:105`; `bin/hook_stop_fila1.py:585`)

Parecer por leitura estática. Execução dos selos, ocorrência em produção e autenticação do hash pelo histórico Git: **[nao verificado]**.

**1. Escritores e leitores dos estados**

| Estado | Quem escreve | Quem lê; migração pendente |
|---|---|---|
| `logs/sessao/padrao_de_cima_geracao` | `_pdc_geracao_grava`, chamada pelo callback PDC antes da tentativa de gravar o estado. (`bin/disjuntor.py:1454`; `bin/disjuntor.py:1487`; `bin/disjuntor.py:1491`) | `_pdc_geracao_lida`, usada no fallback. Há também o leitor genérico de **mtime** `_prova_arquivo`, cuja exclusão dos arquivos do próprio instrumento não recebeu o arquivo novo. (`bin/disjuntor.py:1499`; `bin/disjuntor.py:1548`; `bin/hook_stop_fila1.py:578`; `bin/hook_stop_fila1.py:585`) |
| `escopo.geracao` | `_pdc_grava_escopo`, depois de estado gravado, pelo anúncio ainda sob a trava. (`bin/disjuntor.py:292`; `bin/disjuntor.py:1462`; `bin/disjuntor.py:1514`) | `_pdc_escopo_lido` carrega; `estado_pdc` compara com o marcador. Não identifiquei outro escritor desse campo na cópia. (`bin/disjuntor.py:1529`; `bin/disjuntor.py:1549`) |
| Arquivo `disjuntor_estado.json` | Porta produtiva `_com_estado`: lê, executa callback e substitui por temporário. Fixtures também escrevem diretamente. (`bin/disjuntor.py:267`; `bin/disjuntor.py:275`; `bin/disjuntor.py:281`; `bin/tests/test_padrao_de_cima.sh:576`; `bin/tests/test_padrao_de_cima.sh:620`) | `_com_estado` e `ler_estado`; o painel consome via `_pdc_painel`, inclusive pelo gerador do ESTADO. (`bin/disjuntor.py:302`; `bin/disjuntor.py:1636`; `bin/disjuntor.py:2236`; `bin/gerar_estado.py:281`) |
| `pdc.eventos`, `nomeados`, `libera_vistas`, `fotos`, `disparadas` | Callback de `decide_pdc`. Fixtures alteram eventos, versão e agrupamentos sem atualizar geração. (`bin/disjuntor.py:1385`; `bin/disjuntor.py:1402`; `bin/disjuntor.py:1440`; `bin/disjuntor.py:1445`; `bin/disjuntor.py:1449`; `bin/tests/test_padrao_de_cima.sh:574`) | A própria decisão, janela, agrupamento e painel. O painel não consulta geração; apresenta contagens, sem decidir liberação. (`bin/disjuntor.py:1391`; `bin/disjuntor.py:1303`; `bin/disjuntor.py:1308`; `bin/disjuntor.py:1633`) |
| `<raiz>.proc`: `INI/FIM/SINAL/PRAZO` | `_mordida_sondado` trunca; `sitecustomize` escreve INI, FIM e SINAL; a função shell escreve PRAZO; o controle copia a linha de base. São escritores distintos deliberados. (`bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:55`; `bin/tests/_mordida.sh:59`; `bin/tests/_mordida.sh:65`; `bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:210`) | `abertos` interpreta os registros de mutante e controle. O `timeout` do Bash separado do wrapper continua sem produzir PRAZO. (`bin/tests/_mordida.sh:244`; `bin/tests/_mordida.sh:261`; `bin/tests/test_padrao_de_cima.sh:289`; `bin/codex_pergunta.sh:177`) |
| `.exc` | Truncamento, callback de toda exceção RAISE da cópia e cópia da linha de base. (`bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:114`; `bin/tests/_mordida.sh:209`) | Controle procura cinco tipos de erro de programação; verificador lê para declarações e para a nova regra de `SystemExit`. (`bin/tests/_mordida.sh:206`; `bin/tests/_mordida.sh:258`; `bin/tests/_mordida.sh:285`; `bin/tests/_mordida.sh:314`) |
| `.esc` | Truncamento, `sys.excepthook` e cópia da linha de base. (`bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:125`; `bin/tests/_mordida.sh:211`) | Verificador desconta declaradas e compara multiplicidade das assinaturas restantes com o controle. (`bin/tests/_mordida.sh:259`; `bin/tests/_mordida.sh:319`; `bin/tests/_mordida.sh:320`) |
| `.novo` | `_mordida_muta` trunca e grava textos de substituição separados por NUL. (`bin/tests/_mordida.sh:169`; `bin/tests/_mordida.sh:192`) | Regra de `SystemExit` e conferência das exceções declaradas. (`bin/tests/_mordida.sh:269`; `bin/tests/_mordida.sh:284`; `bin/tests/_mordida.sh:310`) |
| `.orig.d` | `_mordida_muta` apaga a coleção anterior e guarda o original do arquivo mutado. (`bin/tests/_mordida.sh:169`; `bin/tests/_mordida.sh:176`; `bin/tests/_mordida.sh:178`) | As duas conferências anteriores; se o original estiver ausente, ambas usam o arquivo **atual** como substituto da prova original. (`bin/tests/_mordida.sh:291`; `bin/tests/_mordida.sh:292`; `bin/tests/_mordida.sh:308`; `bin/tests/_mordida.sh:309`) |
| `_MORDIDA_PROC` | `_mordida_sondado` atribui e depois esvazia; não é local nem restaura valor anterior. (`bin/tests/_mordida.sh:157`; `bin/tests/_mordida.sh:162`) | A função `timeout`, para escolher onde apendar PRAZO. (`bin/tests/_mordida.sh:135`) |
| Função shell `timeout` | Definição global ao carregar o harness nos dois selos. (`bin/tests/_mordida.sh:131`; `bin/tests/test_padrao_de_cima.sh:34`; `bin/tests/test_disjuntor.sh:45`) | Chamadas dos casos passam a usá-la, inclusive chamadas antigas do selo disjuntor. Ela não é exportada para o Bash novo do wrapper. (`bin/tests/test_padrao_de_cima.sh:601`; `bin/tests/test_disjuntor.sh:579`; `bin/tests/test_disjuntor.sh:661`; `bin/tests/test_padrao_de_cima.sh:289`) |

Há vários callbacks escritores do **arquivo** de estado, mas passam pela mesma porta: B1, triagem, B3, rearme, B2 e B4. Esses callbacks alteram outras chaves; não constituem outro escritor produtivo de `pdc.eventos` que precisasse incrementar a geração PDC. (`bin/disjuntor.py:494`; `bin/disjuntor.py:845`; `bin/disjuntor.py:986`; `bin/disjuntor.py:1059`; `bin/disjuntor.py:1782`; `bin/disjuntor.py:1808`; `bin/disjuntor.py:2058`; `bin/disjuntor.py:2066`)

**2. Classes que já reprovaram obra**

| Classe | Julgamento neste diff |
|---|---|
| 1. Segundo juiz registrado | **Violação não evidenciada.** PDC continua decidida por `decide_pdc`/`estado_pdc`; o caminho novo da chave consulta a mesma `desligado`. O lookup declara que o registro vivo é `core/juizes.py`. (`bin/disjuntor.py:1360`; `bin/disjuntor.py:1537`; `bin/disjuntor.py:2096`; `app/docs/JUIZES.md:10`) |
| 2. Segundo escritor fora da porta | **Não encontrei segundo escritor produtivo de PDC.** Existem escritores diretos nas fixtures, sem geração; isso impede tratar a geração como detector universal de qualquer alteração do JSON, mas não demonstra outro escritor produtivo. (`bin/disjuntor.py:1385`; `bin/disjuntor.py:1454`; `bin/tests/test_padrao_de_cima.sh:576`; `bin/tests/test_padrao_de_cima.sh:620`) |
| 3. Relógio fora da borda | **Violação nova não evidenciada.** A geração usa aleatoriedade; a decisão continua recebendo ou capturando `agora`, repassado ao escopo. (`bin/disjuntor.py:1369`; `bin/disjuntor.py:1462`; `bin/disjuntor.py:1486`) |
| 4. Derivação paralela de fonte única | **Violação nova não evidenciada.** O escopo permanece uma projeção da decisão, com geração adicionada. A falsa prova de prazo abaixo é um problema de evidência, não demonstração de juiz de negócio duplicado. (`bin/disjuntor.py:1514`; `bin/disjuntor.py:1515`; `bin/tests/_mordida.sh:135`) |
| 5. Selo/RED sem mordida | **Sim, bloqueia.** Há mordida aceita sem execução da decisão e autorização testada apenas por ausência de saída. (`bin/tests/_mordida.sh:293`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/test_padrao_de_cima.sh:329`) |
| 6. Ordem de trava | **Inversão nova não demonstrada.** Geração e escopo ficam na mesma transição protegida. Há, entretanto, leitura bloqueante remanescente dentro dessa seção crítica, detalhada abaixo. (`bin/disjuntor.py:1454`; `bin/disjuntor.py:1462`; `bin/disjuntor.py:1466`; `app/core/censo_escritas.py:105`) |
| 7. Pouso antes do pré-requisito | **[nao verificado].** A exigência é suíte antes do opus e opus antes do pouso; os anexos não demonstram sua execução. (`CLAUDE.md:549`; `CLAUDE.md:553`) | [nao verificado]

**3. O que o harness prova — e os caminhos de veredito errado**

A aceitação exige: mutação com âncoras únicas e sintaxe válida; caso com rc não zero atribuído ao último assert reconhecido; linha de base existente; ausência de contaminação adicional segundo os contadores; declarações presentes e admitidas; ausência de exceção escapada adicional. A checagem sintática prova compilação, não execução do módulo nem passagem pela decisão. (`bin/tests/_mordida.sh:181`; `bin/tests/_mordida.sh:185`; `bin/tests/_mordida.sh:197`; `bin/tests/_mordida.sh:218`; `bin/tests/_mordida.sh:220`; `bin/tests/_mordida.sh:226`; `bin/tests/_mordida.sh:262`; `bin/tests/_mordida.sh:314`; `bin/tests/_mordida.sh:323`)

“Mordeu pelo prazo” significa apenas: houve mais processos sem FIM, com SINAL cujo **grupo** aparece numa linha PRAZO, do que no controle. PRAZO, por sua vez, significa que `wait` devolveu 124. Não há registro independente de expiração nem identificação da origem do sinal. (`bin/tests/_mordida.sh:134`; `bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:253`; `bin/tests/_mordida.sh:254`; `bin/tests/_mordida.sh:266`)

- **B1 — bloqueia: saída antecipada Python aceita como mordida.** Mutar `return padrao_parada(argv[1])` para `return 1` faz `i14_mesma` retornar pelo assert 1, sem chamar a decisão. INI/FIM fecham normalmente. `SystemExit` nasce em `sys.exit(main(...))`, linha original e ausente do texto novo; a guarda não acusa. O caso foi derrubado por uma saída antecipada que o harness afirma excluir. (`bin/disjuntor.py:2333`; `bin/disjuntor.py:2343`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:59`; `bin/tests/_mordida.sh:293`)

- **B2 — bloqueia: mutante shell que não roda o instrumento também é aceito.** Trocar `set -uo pipefail` do wrapper por `exit 1` passa `bash -n`; `wrapper` retorna seu assert 1. Nenhum Python do wrapper precisa executar. O verificador penaliza processos abertos adicionais, mas não exige execução nem quantidade mínima de processos; a regra de saída plantada só examina `SystemExit` Python. (`bin/codex_pergunta.sh:27`; `bin/tests/_mordida.sh:197`; `bin/tests/test_padrao_de_cima.sh:294`; `bin/tests/_mordida.sh:262`; `bin/tests/_mordida.sh:285`)

- **B3 — bloqueia: 124 e grupo não provam prazo.** Como contraexemplo estático, a substituição abaixo em `main` cria um filho que se mata, espera por ele e devolve 124 imediatamente; o pai sai pela linha original de `sys.exit`. O filho herda a sonda e o grupo do comando temporizado. A função grava PRAZO, `resolvedor_fifo` retorna assert 1 e o verificador classifica o filho como morto pelo prazo, embora o caminho não espere os 20 segundos. (`bin/disjuntor.py:2333`; `bin/disjuntor.py:2343`; `bin/tests/_mordida.sh:65`; `bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:254`; `bin/tests/test_padrao_de_cima.sh:601`; `bin/tests/test_padrao_de_cima.sh:603`)

  ```python
  return __import__('subprocess').call([
      __import__('sys').executable, '-c',
      'import os; os.kill(os.getpid(), 15)'
  ]) and 124
  ```

  Também não se distingue um SIGTERM externo recebido por um descendente antes do vencimento de um timeout que expire depois: a associação considera somente o grupo, sem instante ou emissor do sinal. (`bin/tests/_mordida.sh:65`; `bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:254`)

- **F1 — fila: falso VERMELHO por `SystemExit` capturada.** `.exc` registra toda ocorrência, mas o filtro novo acusa qualquer `SystemExit` em linha considerada plantada, mesmo capturada e seguida da decisão real. Além disso, `plant` contém todas as linhas dos blocos novos, inclusive contexto inalterado, sem distinguir posição ou arquivo. Isso pode recusar uma mordida válida; não demonstra liberação indevida do instrumento produtivo. (`bin/tests/_mordida.sh:115`; `bin/tests/_mordida.sh:284`; `bin/tests/_mordida.sh:285`; `bin/tests/_mordida.sh:293`)

- **F2 — fila: contaminações podem se compensar por quantidade.** O controle não rejeita processos sem FIM ou SIGTERM externo; esses totais viram tolerância. Um aborto do mutante pode substituir outro aborto do controle sem aumentar o contador. A comparação não demonstra correspondência entre operações equivalentes. (`bin/tests/_mordida.sh:205`; `bin/tests/_mordida.sh:206`; `bin/tests/_mordida.sh:255`; `bin/tests/_mordida.sh:262`; `bin/tests/_mordida.sh:264`)

**4. Cada caso e mutante novo morde o quê**

Os caminhos abaixo são deduções do código; execução e tempo observado são **[nao verificado]**.

| Caso/mutante | Julgamento |
|---|---|
| Extensão `resolvedor_fonte` + `v9-resolvedor-v-parado` | **Morde a versão 2:** o vazio plantado com `res=2` é aceito quando V=2; não redispara, produzindo `return 16`. Porém o trecho novo também passaria contra 5b39ae82: V=3 e a condição de reclassificação já eram código anterior no diff fornecido. É cobertura adicional, não RED desta cura. (`bin/tests/test_padrao_de_cima.sh:574`; `bin/tests/test_padrao_de_cima.sh:578`; `bin/tests/test_padrao_de_cima.sh:805`; `bin/disjuntor.py:1105`; `bin/disjuntor.py:1392`) |
| `resolvedor_fifo` + `v9-modelos-fifo` | O primeiro FIFO prende o `open` reintroduzido; rc124 leva a `return 1`. Morde a remoção de `_abre` nesse leitor, condicionado às limitações da prova de prazo. Não cobre o leitor anterior `CE.modelos`. (`bin/tests/test_padrao_de_cima.sh:600`; `bin/tests/test_padrao_de_cima.sh:603`; `bin/tests/test_padrao_de_cima.sh:806`; `bin/disjuntor.py:1222`) |
| `resolvedor_fifo` + `v9-sitio-fifo` | Primeiro subcaso passa; o segundo FIFO prende o `open` do sítio e leva a `return 2`. O caso exige rc4 antes do limite, mas não mede a alegação “no ato”: uma espera inferior a 20 segundos seguida de rc4 passa. (`bin/tests/test_padrao_de_cima.sh:604`; `bin/tests/test_padrao_de_cima.sh:607`; `bin/tests/test_padrao_de_cima.sh:808`) |
| `escopo_velho` + `v9-escopo-de-outra-geracao` | **Morde a comparação removida:** escopo antigo omite A; estado ilegível e RELATO neutro permitem LIVRE, derrubando o assert final. O caso não confere token, ordem de escrita nem recriação. Uma cura incorreta que sempre recuse o fallback também passa **esse caso**; `escopo_duvida` oferece o contraponto LIVRE na suíte. (`bin/tests/test_padrao_de_cima.sh:612`; `bin/tests/test_padrao_de_cima.sh:621`; `bin/tests/test_padrao_de_cima.sh:810`; `bin/tests/test_padrao_de_cima.sh:513`; `bin/disjuntor.py:1567`) |
| Extensão `desligado` + `v9-cru-ignora-chave` | **Morde a negativa que reaparece** nos arrays com a chave presente, por `return 6`. **B4 — bloqueia:** não confere rc. Trocar o corpo do ramo novo por `raise SystemExit(1)` deixa stdout vazio e faz esse caso passar, embora o processo tenha encerrado com erro. (`bin/tests/test_padrao_de_cima.sh:328`; `bin/tests/test_padrao_de_cima.sh:329`; `bin/tests/test_padrao_de_cima.sh:331`; `bin/tests/test_padrao_de_cima.sh:811`; `bin/disjuntor.py:2099`) |
| Meta `mutante-que-nao-compila` | Exercita a recusa de sintaxe Python antes do caso. **Fila:** não há meta correspondente para o novo `bash -n`. (`bin/tests/test_padrao_de_cima.sh:831`; `bin/tests/_mordida.sh:187`; `bin/tests/_mordida.sh:197`) |
| Meta `saida-plantada` | Exercita um `raise SystemExit` diretamente plantado; não cobre retorno antecipado através da linha original de saída nem `exit` shell — B1/B2. (`bin/tests/test_padrao_de_cima.sh:833`; `bin/tests/test_padrao_de_cima.sh:834`; `bin/tests/_mordida.sh:293`) |
| Meta `sigterm-sem-prazo` | Exercita automorte direta, sem timeout. Não cobre 124 produzido pelo filho nem sinal externo dentro de grupo posteriormente marcado PRAZO — B3. (`bin/tests/test_padrao_de_cima.sh:836`; `bin/tests/test_padrao_de_cima.sh:837`; `bin/tests/_mordida.sh:254`) |

Os mutantes antigos tocados de sítio/modelos continuam retirando o tratamento que transforma fonte ilegível em dúvida, e o de escopo continua deslocando a gravação para fora da trava; suas âncoras foram adaptadas ao novo código. As alterações dos chamadores nos dois selos propagam a mensagem de falha do mutador. (`bin/tests/test_padrao_de_cima.sh:738`; `bin/tests/test_padrao_de_cima.sh:785`; `bin/tests/test_padrao_de_cima.sh:793`; `bin/tests/test_padrao_de_cima.sh:698`; `bin/tests/test_disjuntor.sh:1501`; `bin/tests/test_disjuntor.sh:1510`)

**Fila:** os metas exigem uma falha contendo determinado trecho; não exigem que aquela seja a única causa dentro da mensagem. Um meta pode passar com contaminação adicional agregada na mesma falha. (`bin/tests/_mordida.sh:326`; `bin/tests/_mordida.sh:335`; `bin/tests/_mordida.sh:336`)

**5. Demais erros e fragilidades**

- **B5 — bloqueia: o primeiro leitor de modelos não foi migrado.** `_prepara` chama `CE.modelos` **antes** de `_so_modelos`. Ali permanece `isfile` seguido de `open(...).read()`: trocar regular por FIFO entre essas operações prende o resolvedor dentro do flock do estado. O FIFO estático do caso é rejeitado pelo `isfile` e não exercita essa janela. A cura protege o segundo leitor, não toda a cadeia. (`bin/disjuntor.py:1222`; `bin/disjuntor.py:1224`; `app/core/censo_escritas.py:102`; `app/core/censo_escritas.py:105`; `bin/disjuntor.py:260`; `bin/disjuntor.py:275`; `bin/tests/test_padrao_de_cima.sh:600`)

- **B6 — bloqueia: geração nova pode virar prova falsa de `ESPERANDO`.** Declare `ESPERANDO: agente-x | arquivo=logs/sessao/padrao_de_cima_geracao`; a primeira avaliação memoriza o mtime. Uma decisão PDC reescreve o arquivo mesmo sem evento novo. Na próxima avaliação, a exclusão não reconhece o arquivo como produto do próprio instrumento, o mtime avançado satisfaz a prova e o Stop pode liberar. O escopo preexistente também está ausente dessa exclusão. (`bin/hook_stop_fila1.py:658`; `bin/hook_stop_fila1.py:679`; `bin/hook_stop_fila1.py:585`; `bin/hook_stop_fila1.py:623`; `bin/hook_stop_fila1.py:640`; `bin/hook_stop_fila1.py:1233`; `bin/disjuntor.py:1454`; `bin/disjuntor.py:1521`)

- **Fila — o prazo não garante encerramento.** O wrapper não acrescenta escalada para SIGKILL; se o código ignorar SIGTERM, `wait` pode permanecer preso. Isso limita a robustez do harness contra outros mutantes, embora os dois novos mutantes FIFO mantenham o handler da sonda. (`bin/tests/_mordida.sh:63`; `bin/tests/_mordida.sh:133`; `bin/tests/_mordida.sh:134`)

- **Fila — prazo do wrapper ficou fora do protocolo.** `_xw` inicia outro Bash; a função não é exportada, e o `timeout` interno continua sem PRAZO. Uma morte legítima nesse caminho pode ser classificada como SIGTERM sem prazo, em vez de receber a atribuição pretendida. (`bin/tests/test_padrao_de_cima.sh:289`; `bin/codex_pergunta.sh:177`; `bin/tests/_mordida.sh:131`; `bin/tests/_mordida.sh:264`)

- **Fila — sem suporte a casos paralelos no mesmo selo.** Os casos compartilham `A/EST`; `_arvore` remove a árvore inteira. Linhas de base por nome e `meta.out` também são compartilhados. Os loops atuais são sequenciais e execuções independentes recebem `mktemp` distintos; paralelizar casos internamente pode produzir falhas de fixture aceitas como mordida. (`bin/tests/test_padrao_de_cima.sh:29`; `bin/tests/test_padrao_de_cima.sh:35`; `bin/tests/test_padrao_de_cima.sh:39`; `bin/tests/test_padrao_de_cima.sh:683`; `bin/tests/_mordida.sh:209`; `bin/tests/_mordida.sh:335`; `bin/tests/test_disjuntor.sh:39`)

- **Fila — sonda não reentrante.** Uma chamada aninhada de `_mordida_sondado` sobrescreve os globais e termina esvaziando `_MORDIDA_PROC`, sem restaurar o contexto exterior. Não há chamada aninhada demonstrada pelos chamadores atuais. (`bin/tests/_mordida.sh:156`; `bin/tests/_mordida.sh:157`; `bin/tests/_mordida.sh:162`; `bin/tests/_mordida.sh:204`; `bin/tests/_mordida.sh:216`)

- **Fila — VERMELHO por premissa de permissões.** `escopo_velho` depende de `chmod 555` realmente impedir a substituição no diretório. Se o executor tiver privilégio para escrever ali, falta a falha esperada e o caso reprova código intacto pelo `grep`. Identidade e privilégios do executor real: **[nao verificado]**. (`bin/tests/test_padrao_de_cima.sh:615`; `bin/tests/test_padrao_de_cima.sh:619`)

- **Fila — temporário do escopo sem limpez

[truncado em 20000 de 21588 caracteres]

## 31/10 21:49 · sessao af7b8582d461 · O261-E-F1C4-MEMO-C6 · censo · origem=Code

**A chave não cobre todos os insumos do C6. Há também um caminho condicional no código de produção que pode mudar batidas entre duas consultas às mesmas perguntas. Sua ocorrência e uma divergência efetiva em produção são [nao verificado].** (`app/chamados/juizes.py:788–796`; `app/ponto/services/cartorio.py:1141,1394`; `app/chamados/services/regularizacao_ext.py:136,155,181`)

1) **Quem escreve e quem lê `_pv_memo`**

Os acessos executáveis ao atributo encontrados na cópia são estes. (`app/chamados/juizes.py:813–824`; `app/chamados/models.py:1439`)

| Operação | Sítio |
|---|---|
| **Escreve/substitui** `(chave, resultado)` | `pergunta_viva`, depois do cálculo; recalcula a chave para incluir relações carregadas durante a avaliação. `app/chamados/juizes.py:816–824` |
| **Lê** o atributo, compara a chave e devolve o resultado armazenado | `pergunta_viva`: `getattr`, comparação de `_m[0]` e retorno de `_m[1]`. `app/chamados/juizes.py:813–815` |
| **Remove** o atributo da cópia da pergunta | `_universo_da_ressuscitacao`, antes de consultar C6 sobre o estado projetado da ressuscitação. `app/chamados/models.py:1427–1445` |

Os testes do memo exercitam o atributo **indiretamente**, chamando `pergunta_viva`: repetição na mesma instância, comparação com instância recém-carregada, alterações de campos da chave, alteração sem `save` e repetição sem queries. Não acessam diretamente `_pv_memo` nesses casos. (`app/chamados/tests/test_selo_memo_pergunta_viva.py:81–114,117–178`)

Os demais registros textuais são comentários/docstrings sobre sua duração e uma referência ao nome da função da chave; não são acessos executáveis ao atributo. (`app/chamados/juizes.py:905,943`; `SERIE_T.md:2106`)

2) **Insumos lidos que ficam fora da chave**

A chave contém somente:

```text
p.validada_em, respondida_em, resposta_colab, via_resolucao,
materializada_em, data_evento, tipo_resposta, celula_id,
p.celula.veredito, p.celula.veredito_via,
p.chamado_destino_id, status_local do destino já carregado
```

O status do destino entra como `'?'` quando a relação ainda não está no cache. (`app/chamados/juizes.py:778–796`)

As dependências abaixo ficam fora dessa tupla; sua utilização depende do ramo alcançado pelo cálculo. (`app/chamados/juizes.py:1032–1118`)

| Insumo omitido | Leitura no cálculo ou nos chamados |
|---|---|
| **`p.motivo`** | Escolhe o marco cobrado, participa da concordância com a luz e seleciona dados do contexto. `app/chamados/juizes.py:863,1000,362`; `app/chamados/services/materializacao.py:327–333` |
| **`cel.dna['marcos']`** | Horários do marco e dos pares de pausa, inclusive existência de segunda pausa. `app/chamados/juizes.py:870–884,1002–1007` |
| **`cel.ata['lampadas']`** | `hora`, `tipo`, `acesa` e `luz` determinam fato provado e concordância com a resposta. `app/chamados/juizes.py:887–893,1008–1029` |
| **Marcos pendentes da ata** | C6 chama `marcos_pendentes`; ela lê `ata.lampadas[*].pendente`. Aqui **lastro é o módulo**, não uma leitura de campo `cel.lastro`. `app/chamados/juizes.py:1094–1096`; `app/ponto/services/lastro.py:138–150` |
| **`cel.trabalha`** | `_celulas_da_pergunta` fornece a própria FK ao classificador; `classificar_falta` cala quando `trabalha is False`. `app/chamados/juizes.py:598–606`; `app/escala/servico_jornada.py:302–304` |
| **Identidade e disponibilidade da célula** | `cel.data`, `cel.colaborador_id`, `colab.pk` e campos diferidos determinam se a FK será aproveitada ou se haverá outra consulta. `app/chamados/juizes.py:602–606`; `app/escala/servico_jornada.py:267–271` |
| **Disputa, colaborador e empresa** | A tranca usa `p.disputa.colaborador`, `empresa_id` e empresa; classificação, slot e turnos também dependem do colaborador. `app/chamados/juizes.py:909–921,1100–1102,344,369` |
| **Competência e tranca viva** | Corte da empresa, existência da linha `PeriodoFechado`, empresa/mês/ano, `reaberto_em` e `fechado_em`. `app/ponto/janelas.py:50–63`; `app/ponto/services/fechamento.py:1432–1438`; `app/chamados/juizes.py:918–928` |
| **Toque humano posterior à tranca** | Trilhas, usuário, instante, ação, modelo, objeto, descrição e células/chamados relacionados ao dia. `app/chamados/juizes.py:971–978` |
| **Batidas e retratações** | Existência, tipo, timestamp, colaborador, vínculo à pergunta e `retratada_em`; afetam plantio comprovado, slot e completude do turno. `app/chamados/juizes.py:233–237,258–266,695–702,751–755`; `app/ponto/turnos.py:1861–1864` |
| **Trilha de retratação da pergunta** | Consulta `LogAuditoria` pelo ato e pelo `p.pk`. `app/chamados/juizes.py:275–280` |
| **Ausências** | Existência/cobertura, status, tipo, datas e minutos; a classificação consulta ausências e distingue cobertura parcial. `app/escala/servico_jornada.py:340–345`; `app/ponto/turnos.py:2509–2533,2595–2600` |
| **Contextos da disputa e do destino** | `disputa.contexto[motivo].data/hora_tentativa` e `chamado_destino.contexto_json.marcos_faltantes`, incluindo motivo/hora/tipo. `app/chamados/juizes.py:359–370,498–529`; `app/chamados/services/materializacao.py:329–333,394–406` |
| **Vínculos, geradora, vigência e template de escala** | Determinam escala e marcos esperados, inclusive fallback quando não há DNA utilizável. `app/escala/servico_jornada.py:46–58,354,427–435`; `app/escala/alimentacao.py:237–254`; `app/escala/models.py:1094–1108` |
| **Outros dados utilizados pela grade do slot** | Admissão, demissão, isenção, ata e `trabalha`, por chamadas a `grade_da_celula`. `app/chamados/services/materializacao.py:218–227,267–268`; `app/escala/services/leitor_celula.py:321–372` |
| **Relógio** | `date.today()` muda o alcance da janela; `timezone.now()` participa do teto do turno. `app/chamados/juizes.py:539–565`; `app/escala/servico_jornada.py:375–378` |

**Cartório em curso exige uma distinção:** C6 lê diretamente `p.celula`; ele não consulta automaticamente `ata_deste_julgamento` ou `veredito_deste_julgamento`. Quem consulta esses mapas é `celula_deste_julgamento`, que produz uma projeção da célula. Na ressuscitação, essa projeção é entregue a uma cópia da pergunta e seu memo é removido antes da avaliação. (`app/chamados/juizes.py:1044,866,392–395`; `app/ponto/portas/celula.py:675–683`; `app/chamados/models.py:1427–1445`)

A tranca possui **outro memo**, no request ou, fora dele, no colaborador. Ele guarda a consulta da tranca inclusive quando resulta em `None`; o memo de toque humano guarda somente o resultado positivo. Portanto, invalidar apenas `_pv_memo` não garante nova consulta da tranca. (`app/chamados/juizes.py:914–923,944–948,965–980`)

3) **Tempo de vida e objetos que o preservam**

O código executável não impõe duração por request: o atributo permanece na instância e o resultado é reutilizado enquanto a chave coincidir. “Morre com o request” é a descrição da intenção; não há identificador de request ou prazo no mecanismo. (`app/chamados/juizes.py:799–824`)

Há preservação concreta das mesmas perguntas nestes contêineres:

| Contêiner | Sobrevida demonstrada |
|---|---|
| **Listas do fio/modal** | As perguntas carregadas entram em `perguntas_por_chamado`, são anexadas a `ch.perguntas_disputa` e passadas ao explicador. `.viva`, `estado` e `fio_vivo` consultam C6 sobre esses objetos. `app/chamados/services/fio.py:222–249,279–281,308–309`; `app/chamados/services/explicador.py:124,133–137`; `app/chamados/models.py:2479,2641` |
| **Prefetch do cartório** | Os chamados guardam perguntas prefetched; o mesmo manager é usado em duas consultas ao fio dentro do corpo. `app/ponto/services/cartorio.py:96–107,1141,1393–1394` |
| **Lista temporária do gate de chamado** | `_f1_ancoradas` conserva perguntas entre o plano e `_gravar_estado`, que as julga novamente. `app/chamados/models.py:362–364,210–218` |
| **Prefetch do relatório** | `classificar` recebe perguntas do prefetch e chama C6; o fluxo monta resultados sem mutação desses insumos demonstrada. `app/chamados/relatorios_disputa.py:11–17,39–52` |

O command do cartório conserva as células em `cels`, e o serviço pode reutilizá-las nas passadas seguintes. Entretanto, cada entrada no corpo recarrega os chamados e suas perguntas: isso **não demonstra sobrevivência do mesmo `p._pv_memo` entre passadas**, nem identidade entre a célula do loop e `p.celula`. (`app/ponto/management/commands/processar_cartorio.py:59–65`; `app/ponto/services/cartorio.py:777,802,867,1071,1105`; `app/chamados/juizes.py:598–606`)

4) **Caminhos de produção: chamada → mutação → nova chamada**

**Existe uma família condicional pelo cartório e pelos signals de fechamento.** A identidade preservada é a pergunta prefetched de um chamado `y`; a mutação intermediária pode ocorrer ao reconciliar outro chamado `x`. Existência desses registros no ambiente produtivo: **[nao verificado]**. (`app/ponto/services/cartorio.py:96–107,1141,1390–1394`)

A sequência codificada é:

1. O command chama `julgar_colab`; o corpo carrega os chamados com perguntas prefetched e avalia `_fio_vivo` dessas perguntas. (`app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:1071,96–97,1141`)
2. Um chamado `x` passa por `reconciliar`, nos ramos de marcos satisfeitos, marcos inexistentes ou cobrança indevida. A porta grava seu estado dentro de transação. (`app/ponto/services/cartorio.py:1287,1300,1321`; `app/chamados/models.py:614–619,279`)
3. O signal S143 recebe o fechamento e chama `resolver_perguntas_por_chamado_resolvido`. (`app/chamados/signals.py:75–92`)
4. O resolvedor inclui **todas as disputas abertas do colaborador** em `_cands` e fecha aquelas liberadas pelo C7. A guarda de credores vivos pertence ao laço anterior de carimbo; não elimina candidatos desse laço final. (`app/chamados/services/disputa_emissao.py:1621–1633,1672–1679`)
5. O fechamento salva a disputa e agenda `retomar_regularizacoes_confirmadas` no commit. (`app/chamados/models.py:1846–1850`; `app/chamados/signals.py:97–121`)
6. A retomada busca regularizações confirmadas e não materializadas. Sem disputa aberta restante, pode **adicionar**, **remover** ou **corrigir tipo/timestamp** de batidas. (`app/chamados/services/regularizacao_ext.py:218–224,51–65,77,136,153–155,181–185`)
7. Sem transação externa abrangendo o julgamento, o callback pode executar na saída da transação de `x.reconciliar`. O command e `julgar_colab` não introduzem tal transação externa nesses trechos. (`app/chamados/models.py:614–619`; `app/ponto/management/commands/processar_cartorio.py:59–65`; `app/ponto/services/cartorio.py:770–777`)
8. O corpo volta a consultar as perguntas do prefetch de `y`, se ele continuar vivo. O chamado `x` encerrado é pulado. Batidas não participam da chave, permitindo reutilizar o memo anterior de `p`. (`app/ponto/services/cartorio.py:1390–1394`; `app/chamados/juizes.py:788–796,813–815`)

A guarda `em_julgamento()` evita reentrada do cartório pelo signal da batida; ela não impede a gravação da batida. (`app/ponto/signals.py:85–93`; `app/ponto/registro_batida.py:157–161`)

Para essa sequência afetar um insumo efetivamente consultado, C6 pode ter alcançado `_turno_tem_saida` numa pergunta textual materializada: remover/corrigir a saída altera a testemunha externa sem alterar a tupla. Essa combinação concreta e a mudança efetiva de resultado em produção permanecem **[nao verificado]**. (`app/chamados/juizes.py:1097–1114,624–649,754–755,359–370`; `app/chamados/services/regularizacao_ext.py:136,155`)

Os outros candidatos principais não demonstram, por si, esse problema:

| Candidato | Por que não comprova reutilização após mutação omitida |
|---|---|
| **Publicar ata/veredito em curso** | Os mapas mudam entre as consultas, mas não atualizam automaticamente a FK das perguntas. A lavra da célula do loop ocorre depois da segunda consulta. `app/ponto/services/cartorio.py:1188,1259,1394,1565–1586`; `app/chamados/juizes.py:392–395,1044` |
| **Desarquivar e registrar toque humano** | Há ressuscitação antes e depois da trilha, mas cada chamada faz nova consulta, copia a pergunta e remove seu memo. `app/chamados/views_arquivar.py:70,77–86`; `app/chamados/models.py:1420–1445` |
| **Responder pelo PWA/API** | As consultas sobre `p` precedem a escrita; o core altera campos da chave. Não aparece nova consulta ao C6 sobre o `p` original depois da criação de ausência. `app/chamados/views.py:1242,1257,1283`; `app/api/views.py:2683,2695–2702`; `app/chamados/services/disputa_emissao.py:1247–1266,1295,1324` |
| **Validar/materializar** | A validação carrega por ID; a cauda consulta outro queryset. O signal transmite `disputa_id` e recarrega disputa/perguntas. `app/chamados/services/validacao.py:359,424–433`; `app/chamados/services/materializacao.py:1233`; `app/chamados/juizes.py:1130`; `app/chamados/signals.py:195–202`; `app/chamados/services/becos.py:67–68` |
| **Gate de chamado / desfazer carimbo** | O gate altera destino/status, presentes na chave; desfazer carimbo relê a pergunta e altera materialização/via, também presentes. `app/chamados/models.py:339–352`; `app/chamados/services/relavra_via.py:221–235`; `app/chamados/juizes.py:790–795` |

O anexo fornecido não comprova uma ocorrência dessas sequências em produção, nem quantidade de memos incorretos: **[nao verificado]**.

## 31/10 21:49 · sessao f4f7b1e2fa91 · - · - · origem=Ronald (tmux)

Achados dos casos/mutantes, por leitura estática; execução dos selos não verificada.

- **Bloqueia — saída antecipada ainda dá falsa mordida.** Inserir `return 1` no começo de `main` deixa `sys.exit(main(...))` na linha original; `i14_mesma` retorna pelo assert, sem chamar a decisão. A verificação de `SystemExit` aceita essa linha original. Também inserir `exit 1` no começo do wrapper passa `bash -n`, derruba `wrapper` no assert e não executa Python da cópia. (`bin/disjuntor.py:2320`, `bin/disjuntor.py:2343`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/test_padrao_de_cima.sh:294`, `bin/tests/_mordida.sh:197`, `bin/tests/_mordida.sh:285`, `bin/tests/_mordida.sh:293`.)

- **Bloqueia — stdout vazio não prova liberação no caso ampliado.** As duas execuções novas de `pretool` ignoram rc. Substituir o corpo de `if chave` por `raise SystemExit(1)` deixa stdout vazio, mantém as portas anteriores funcionando e faz `desligado` terminar com sucesso, apesar da saída antecipada. O mutante declarado verifica somente a volta da negativa. (`bin/tests/test_padrao_de_cima.sh:321`, `bin/tests/test_padrao_de_cima.sh:328`, `bin/tests/test_padrao_de_cima.sh:329`, `bin/tests/test_padrao_de_cima.sh:331`, `bin/tests/test_padrao_de_cima.sh:811`, `bin/disjuntor.py:2099`.)

| Mutante novo | Caminho da mordida / limite |
|---|---|
| `v9-resolvedor-v-parado` | V=2 aceita o vazio plantado com res=2; não redispara e o caso retorna16. A extensão passa também contra 5b39ae82: V=3 e a guarda de reclassificação já existiam no diff fornecido. (`bin/tests/test_padrao_de_cima.sh:574`, `bin/tests/test_padrao_de_cima.sh:578`, `bin/tests/test_padrao_de_cima.sh:805`, `bin/disjuntor.py:1105`, `bin/disjuntor.py:1392`.) |
| `v9-modelos-fifo` | O primeiro FIFO prende `open`; timeout retorna124 e o caso retorna1. Exige a prova de prazo do harness, cujas fragilidades foram enviadas pelo outro explorer. (`bin/tests/test_padrao_de_cima.sh:600`, `bin/tests/test_padrao_de_cima.sh:601`, `bin/tests/test_padrao_de_cima.sh:603`, `bin/tests/test_padrao_de_cima.sh:806`.) |
| `v9-sitio-fifo` | Primeiro subcaso passa; segundo FIFO prende `open`, timeout retorna124 e o caso retorna2. (`bin/tests/test_padrao_de_cima.sh:604`, `bin/tests/test_padrao_de_cima.sh:605`, `bin/tests/test_padrao_de_cima.sh:607`, `bin/tests/test_padrao_de_cima.sh:808`.) |
| `v9-escopo-de-outra-geracao` | Sem comparação, escopo antigo omite A; estado corrompido e relato neutro chegam a LIVRE, disparando return4 do caso. Trocar a comparação por `if True` também passa esse caso; a suíte completa o rejeitaria pela exigência de LIVRE em `escopo_duvida`. (`bin/tests/test_padrao_de_cima.sh:620`, `bin/tests/test_padrao_de_cima.sh:621`, `bin/tests/test_padrao_de_cima.sh:810`, `bin/tests/test_padrao_de_cima.sh:513`, `bin/disjuntor.py:1549`, `bin/disjuntor.py:1567`.) |
| `v9-cru-ignora-chave` | Desativa a guarda; arrays com chave voltam a produzir negativa, disparando return6. Não distingue liberação normal de crash silencioso. (`bin/tests/test_padrao_de_cima.sh:329`, `bin/tests/test_padrao_de_cima.sh:811`, `bin/disjuntor.py:2102`, `bin/disjuntor.py:2111`.) |

- **Fila — precisão dos novos casos.** FIFO só exige rc4 antes de20s, sem demonstrar “no ato”; não cobre troca concorrente regular→FIFO. `escopo_velho` não inspeciona geração, ordem da gravação, falha de geração, recriação/restauração ou concorrência. (`bin/tests/test_padrao_de_cima.sh:601`, `bin/tests/test_padrao_de_cima.sh:607`, `bin/tests/test_padrao_de_cima.sh:612`, `bin/tests/test_padrao_de_cima.sh:621`.)

- **Bloqueia — leitor anterior ainda pode esperar FIFO.** Antes do novo `_abre`, o resolvedor chama `CE.modelos`, que faz `isfile` seguido de `open`; troca concorrente regular→FIFO pode bloquear ali. O FIFO estático do selo não exercita essa janela. (`bin/disjuntor.py:1222`, `bin/disjuntor.py:1224`, `app/core/censo_escritas.py:102`, `app/core/censo_escritas.py:105`, `bin/tests/test_padrao_de_cima.sh:600`.)

- **Fila — metas estreitos.** Sintaxe exercita somente `.py`, deixando novo `bash -n` sem meta; SystemExit exercita somente `raise` plantado direto; SIGTERM exercita somente sinal próprio fora de timeout. Cada meta aceita incremento de uma falha contendo substring, mesmo se houver causas adicionais na mesma mensagem. (`bin/tests/test_padrao_de_cima.sh:831`, `bin/tests/test_padrao_de_cima.sh:833`, `bin/tests/test_padrao_de_cima.sh:836`, `bin/tests/_mordida.sh:197`, `bin/tests/_mordida.sh:335`, `bin/tests/_mordida.sh:336`.)

Classes: classe5 evidenciada acima; classes1/3/4/6 não tiveram violação específica demonstrada nesta leitura — permanecem juiz PDC único, relógio na borda e geração/escopo dentro de `_com_estado`. Pouso antes de pré-requisito é **[não verificado]**; a exigência consta em `CLAUDE.md:549` e `CLAUDE.md:553`, mas os anexos não trazem execução/pouso. (`bin/disjuntor.py:1360`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1462`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:1537`.)

## 31/10 21:48 · sessao 5dca7ee72020 · - · - · origem=Ronald (tmux)

Há **cadeia de produção condicional alcançável**, inclusive sem passar por `becos`; ocorrência concreta no dado fornecido: **[nao verificado]**.

Sequência:

1. O cartório avalia o fio dos chamados carregados: `app/ponto/services/cartorio.py:1141`.
2. Um chamado `x` reconcilia em `:1287`, `:1300` ou `:1321`.
3. `reconciliar` abre transação e chama `_gravar_estado`: `app/chamados/models.py:614-618`; esta salva o chamado em `:279`.
4. S143 recebe o save terminal e chama o resolvedor: `app/chamados/signals.py:75-92`.
5. O resolvedor inclui **todas as disputas abertas do colaborador** em `_cands`: `app/chamados/services/disputa_emissao.py:1621-1623`; no laço final, faz `refresh`, consulta C7 e chama `d.fechar`: `:1672-1679`.
6. `d.fechar` salva a disputa fechada: `app/chamados/models.py:1846-1850`.
7. O signal agenda retomada da regularização: `app/chamados/signals.py:97-121`.
8. Ao commit de `reconciliar`, regularizações confirmadas ainda não materializadas são retomadas: `app/chamados/services/regularizacao_ext.py:218-224`.
9. Essa porta pode criar batida (`:70-82`), remover (`:131-140`) ou corrigir tipo/timestamp (`:149-155`).
10. O cartório volta a avaliar `_fio_vivo` em perguntas de **outro chamado `y` ainda vivo**: `app/ponto/services/cartorio.py:1390-1394`. `x` terminal é pulado em `:1391`.

Guardas e limitações:

- A regularização exige `status='confirmada'` (`app/chamados/services/regularizacao_ext.py:51-54`) e **nenhuma disputa aberta do colaborador** (`:58-65`): a disputa fechada precisa ser a última aberta.
- C7 pode fechar disputa com chamado `y` ainda vivo, desde que suas perguntas estejam mortas: o laço final `app/chamados/services/disputa_emissao.py:1672-1679` não consulta credores; o guarda de credor vivo em `:1628-1633` protege apenas o bloco anterior.
- `julgar_colab` não envolve o corpo em `atomic`; apenas ativa `_tl.ativo` e executa o corpo (`app/ponto/services/cartorio.py:770-777`). Portanto, no command sem transação externa, o callback pode executar antes de retornar a `:1394`; transação externa em uma invocação específica: **[nao verificado]**.
- `em_julgamento()` impede reentrada do cartório pelo signal da batida (`app/ponto/signals.py:85-93`); não impede a gravação pelo chokepoint (`app/ponto/registro_batida.py:157-161`).

Testemunha lógica para mudança de insumo efetivamente lido: pergunta de `y`, tipo texto, materializada, não validada, via sem encerramento por identidade, célula sem veredito, dia fora da janela viva. C6 alcança `_pergunta_materializada_de_fato` (`app/chamados/juizes.py:1097-1114`), cujo ramo texto consulta `_turno_tem_saida` (`:754-755`, `:359-370`). Uma regularização que remove a saída pode mudar esse resultado sem mudar qualquer componente da chave (`:788-796`). Essa combinação de registros em produção permanece **[nao verificado]**.

## 31/10 21:48 · sessao 8b45f086c323 · - · - · origem=Ronald (tmux)

Cartório contém reutilização comprovada de `PerguntaDisputa`: `chamados_que_falam_de_batida` carrega perguntas com prefetch e guarda os chamados em listas (`app/ponto/services/cartorio.py:96-107`); o corpo consulta essas perguntas em `:1141` e volta a consultar o mesmo manager em `:1393-1394`, que delega a `pergunta_viva` (`app/chamados/juizes.py:1142`).

A publicação de ata/veredito correntes entre essas consultas (`app/ponto/services/cartorio.py:1188`, `:1259`) não altera `p.celula`: o C6 lê a FK diretamente (`app/chamados/juizes.py:1044`, `:866`), e a alimentação de `classificar_falta` devolve essa mesma FK (`:598-606`). A projeção do julgamento corrente aparece explicitamente na cópia de ressuscitação, com remoção do memo (`app/chamados/models.py:1427-1445`). A lavra da célula do loop ocorre depois da segunda consulta (`app/ponto/services/cartorio.py:1565-1586`).

Há, porém, uma cadeia condicional de produção capaz de mudar batidas entre as duas consultas:

1. Primeiro `_fio_vivo` sobre perguntas prefetched: `app/ponto/services/cartorio.py:1141`.
2. Um chamado `x` é reconciliado: `:1287`, `:1300` ou `:1321`; a porta usa transação e grava seu estado (`app/chamados/models.py:614-619`, `:279`).
3. O signal S143 chama o resolver (`app/chamados/signals.py:75-92`).
4. Esse resolver inclui **todas** as disputas abertas do colaborador em `_cands` (`app/chamados/services/disputa_emissao.py:1621-1623`) e fecha as que passam pelo C7 (`:1672-1679`). As guardas de credores vivos em `:1627-1633` pertencem ao laço anterior de carimbo; não eliminam candidatos do laço final.
5. Fechar a disputa salva seu estado (`app/chamados/models.py:1846-1850`) e agenda retomada de regularizações no commit (`app/chamados/signals.py:97-121`).
6. A retomada busca regularizações confirmadas e não materializadas (`app/chamados/services/regularizacao_ext.py:218-224`). A materialização exige nenhuma disputa aberta (`:58-65`) e pode adicionar batidas (`:77`, `:181-185`), removê-las (`:136`) ou corrigi-las (`:153-155`).
7. Ao sair da transação de `x.reconciliar`, a execução volta ao corpo do cartório (`app/chamados/models.py:614-619`), que depois pode consultar novamente as perguntas prefetched de outro chamado `y` ainda vivo (`app/ponto/services/cartorio.py:1390-1394`). O ramo pula `x` se ele se tornou terminal (`:1391`).

Condições necessárias: o fechamento deve deixar nenhuma disputa aberta; precisa existir regularização confirmada aplicável; outro chamado `y` deve continuar vivo e reter perguntas já avaliadas no prefetch. A existência desses registros em produção é **[nao verificado]**. A cadeia é alcançável pelo código; ocorrência ou divergência efetiva em produção é **[nao verificado]**.

As passadas do cartório reutilizam as células de `cels` (`app/ponto/services/cartorio.py:777`, `:802`, `:867`), mas recarregam os chamados/perguntas a cada entrada no corpo (`:1071`, `:96-97`). Portanto a repetição de passadas não comprova, sozinha, sobrevivência do mesmo `p._pv_memo`; tampouco comprova que a célula do loop seja a mesma instância de `p.celula` (`app/ponto/services/cartorio.py:1105`; `app/chamados/juizes.py:598-606`).

## 31/10 21:48 · sessao 538c525ade29 · - · - · origem=Ronald (tmux)

Achados do harness, por leitura estática; os selos não foram executados. [não verificado]

1. **Bloqueia — saída antecipada ainda vira mordida VERDE.** Trocar `return padrao_parada(argv[1])` por `return 1` faz o caso `i14_mesma` retornar seu assert 1 sem chamar a decisão. O processo fecha normalmente; o `SystemExit` nasce na linha original `sys.exit(main(...))`, ausente do texto novo, e escapa da nova guarda. Referências: `bin/disjuntor.py:2333`, `bin/disjuntor.py:2343`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:220`, `bin/tests/_mordida.sh:285`, `bin/tests/_mordida.sh:293`.

2. **Bloqueia — `.sh` que sai antes de executar o instrumento também morde.** Trocar `set -uo pipefail` do wrapper por `exit 1` passa `bash -n`; o caso `wrapper` retorna assert 1 antes de executar qualquer Python do wrapper. `.proc/.exc/.esc` vazios não impedem aprovação: o checker penaliza excesso de processos abertos, nunca exige processo ou execução do código mutado. Referências: `bin/codex_pergunta.sh:27`, `bin/tests/_mordida.sh:197`, `bin/tests/test_padrao_de_cima.sh:289`, `bin/tests/test_padrao_de_cima.sh:294`, `bin/tests/_mordida.sh:262`, `bin/tests/_mordida.sh:327`.

3. **Bloqueia — rc 124 não prova expiração.** Exemplo de mutação em `disjuntor.py:2333`:
   ```python
   return __import__('subprocess').call([
       __import__('sys').executable, '-c',
       'import os; os.kill(os.getpid(), 15)'
   ]) and 124
   ```
   O filho herda a sonda, registra `SINAL` no grupo do timeout; o pai fecha normalmente e retorna 124 imediatamente pela linha original `sys.exit`. A função grava `PRAZO`; `resolvedor_fifo` retorna assert 1; o checker aceita e anuncia mordida pelo prazo sem que tenham passado 20 segundos. Caminho: `bin/tests/_mordida.sh:63`, `bin/tests/_mordida.sh:133`, `bin/tests/_mordida.sh:135`, `bin/tests/_mordida.sh:254`, `bin/tests/_mordida.sh:266`, `bin/tests/test_padrao_de_cima.sh:601`, `bin/tests/test_padrao_de_cima.sh:603`, `bin/disjuntor.py:2343`. Caminho estático, não executado. [não verificado]

4. **Fila — `SystemExit` capturada também causa VERMELHO.** A sonda registra toda exceção; a nova prova não consulta se `SystemExit` encerrou o processo. Um mutante válido que levante e capture `SystemExit`, depois efetivamente execute a decisão, é recusado como saída cedo. Além disso, `plant` compara texto global, sem distinguir arquivo, posição ou linha original repetida no bloco novo. Referências: `bin/tests/_mordida.sh:115`, `bin/tests/_mordida.sh:284`, `bin/tests/_mordida.sh:285`, `bin/tests/_mordida.sh:293`.

5. **Fila — comparação de contaminação por totais permite compensação.** O controle pode conter um processo sem fechamento e o mutante trocar esse por outro processo abortado; totais iguais passam. Também aceita sinal externo até a quantidade já encontrada no controle. O controle não rejeita essas mortes, apenas rc e cinco tipos de exceção de programação. Referências: `bin/tests/_mordida.sh:205`, `bin/tests/_mordida.sh:206`, `bin/tests/_mordida.sh:255`, `bin/tests/_mordida.sh:262`, `bin/tests/_mordida.sh:264`.

6. **Fila — prazo não impõe limite de encerramento.** A função não usa `--kill-after`; um mutante que ignore SIGTERM pode manter `wait` indefinidamente. Referências: `bin/tests/_mordida.sh:133`, `bin/tests/_mordida.sh:134`.

Mapa dos estados:

| Estado | Escritores | Leitores |
|---|---|---|
| `.proc` | Truncamento em `_mordida_sondado`; sonda escreve INI/FIM/SINAL; função `timeout` escreve PRAZO; controle copia baseline. `bin/tests/_mordida.sh:155`, `:55`, `:59`, `:65`, `:135`, `:210` | `abertos`, chamado para mutante e controle. `bin/tests/_mordida.sh:247`, `:261` |
| `.exc` | Truncamento; callback RAISE; cópia do controle. `bin/tests/_mordida.sh:155`, `:114`, `:209` | Controle procura cinco tipos; `regs` lê eventos do mutante para declarações e SystemExit. `bin/tests/_mordida.sh:206`, `:236`, `:258`, `:285`, `:314` |
| `.esc` | Truncamento; excepthook; baseline. `bin/tests/_mordida.sh:155`, `:125`, `:211` | Checker conta diferenças e desconta exceções declaradas. `bin/tests/_mordida.sh:259`, `:319`, `:320` |
| `.novo` | `_mordida_muta` trunca e depois grava os textos novos separados por NUL. `bin/tests/_mordida.sh:169`, `:192` | Guarda de SystemExit e validação das declarações. `bin/tests/_mordida.sh:269`, `:284`, `:310` |
| `.orig.d` | `_mordida_muta` remove o anterior e grava original do arquivo. `bin/tests/_mordida.sh:169`, `:176`, `:178` | Guarda de SystemExit e declarações; ausência cai para arquivo atual. `bin/tests/_mordida.sh:291`, `:292`, `:308`, `:309` |
| `_MORDIDA_PROC` | `_mordida_sondado` atribui e depois esvazia, sem restaurar valor anterior. `bin/tests/_mordida.sh:157`, `:162` | Função `timeout`. `bin/tests/_mordida.sh:135` |
| Função `timeout` | Definição global ao dar source no harness. `bin/tests/_mordida.sh:131`, `bin/tests/test_padrao_de_cima.sh:34`, `bin/tests/test_disjuntor.sh:45` | Casos PDC e disjuntor. `bin/tests/test_padrao_de_cima.sh:601`, `:605`, `bin/tests/test_disjuntor.sh:579`, `:661` |

A função não é exportada para o Bash novo do wrapper; seu `timeout` interno continua binário, sem anotação PRAZO. Referências: `bin/tests/_mordida.sh:131`, `bin/tests/test_padrao_de_cima.sh:289`, `bin/codex_pergunta.sh:177`.

Casos do mesmo processo de selo não suportam paralelismo: compartilham árvore `A`, arquivo `EST`, variáveis globais da sonda, baseline por nome do caso e `meta.out` único. Os loops atuais são sequenciais e duas execuções independentes usam diretórios `mktemp` separados. Referências: `bin/tests/test_padrao_de_cima.sh:29`, `:35`, `:36`, `:39`, `:683`, `bin/tests/test_disjuntor.sh:39`, `bin/tests/_mordida.sh:156`, `:209`, `:335`.

## 31/10 21:48 · sessao 00c6433b6e74 · - · - · origem=Ronald (tmux)

Achados PDC, por leitura estática:

1. **Bloqueia — leitor de modelos não migrado mantém janela de bloqueio em FIFO.** `_prepara` chama `CE.modelos` antes do novo `_so_modelos` (`bin/disjuntor.py:1222`, `bin/disjuntor.py:1224`). Esse primeiro leitor faz `isfile(c)` e depois `open(c).read()` sem `O_NONBLOCK` (`app/core/censo_escritas.py:102`, `app/core/censo_escritas.py:105`). Se um arquivo regular virar FIFO entre as duas operações, prende dentro da transição que já detém o flock (`bin/disjuntor.py:260`, `bin/disjuntor.py:275`). O novo caso planta FIFO antes da chamada, quando `isfile` já o rejeita; não cobre a troca concorrente (`bin/tests/test_padrao_de_cima.sh:600`, `bin/tests/test_padrao_de_cima.sh:601`). O `_abre` novo protege o segundo leitor e o sítio citado: abre com `O_NONBLOCK|O_NOFOLLOW`, valida `fstat` e recusa não regular (`bin/disjuntor.py:1250`, `bin/disjuntor.py:1277`, `bin/orcamento_contexto.py:74`, `bin/orcamento_contexto.py:76`).

2. **Bloqueia — arquivo novo do próprio instrumento pode provar ESPERANDO.** O disjuntor reescreve `padrao_de_cima_geracao` em toda decisão PDC, mesmo sem alteração de eventos (`bin/disjuntor.py:1454`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1491`). O consumidor `_prova_arquivo` exclui os arquivos do próprio disjuntor por lista e inode, mas a lista não inclui geração (`bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:598`). Caminho: topo declara `ESPERANDO: produtor | arquivo=logs/sessao/padrao_de_cima_geracao`; a primeira avaliação memoriza mtime, uma chamada PDC o altera, a seguinte considera a espera provada (`bin/hook_stop_fila1.py:658`, `bin/hook_stop_fila1.py:679`, `bin/hook_stop_fila1.py:620`, `bin/hook_stop_fila1.py:640`, `bin/hook_stop_fila1.py:642`). Isso libera o Stop por atividade do instrumento, sem prova da entrega esperada (`bin/hook_stop_fila1.py:1231`, `bin/hook_stop_fila1.py:1233`). O escopo preexistente também está ausente dessa exclusão (`bin/disjuntor.py:1521`, `bin/hook_stop_fila1.py:585`).

3. **Fila — temporário de escopo vaza na falha posterior à criação.** `_pdc_grava_escopo` cria temporário, serializa e faz `replace`; o `except` apenas registra falha, sem `unlink(tmp)` (`bin/disjuntor.py:1518`, `bin/disjuntor.py:1521`, `bin/disjuntor.py:1522`). O escritor de estado vizinho tem limpeza explícita, portanto o comportamento é inconsistente (`bin/disjuntor.py:283`, `bin/disjuntor.py:286`). Falha de `json.dump`, fechamento ou `replace` deixa `.padrao_de_cima_escopo.*`; ocorrência no ambiente fornecido é [não verificado].

4. **Fila — função com parâmetro `raiz` mistura árvores.** `decide_pdc(raiz)` usa a raiz explícita para resolvedor, geração e estado, mas `_relato_consistente()` ignora esse parâmetro e lê `hook_stop_fila1.RELATO` global (`bin/disjuntor.py:1370`, `bin/disjuntor.py:1372`, `bin/disjuntor.py:1376`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:707`). A CLI normal alinha por ambiente; consumidor Python que fornece outra raiz pode gravar eventos de outra árvore (`bin/disjuntor.py:67`, `bin/disjuntor.py:1360`, `bin/disjuntor.py:1402`). É preexistente, mas está no escopo de leitores da função tocada (`bin/disjuntor.py:1376`).

5. **Fila — escrita da geração tem validação depois da operação destrutiva e não verifica escrita completa.** `O_TRUNC` ocorre antes de validar arquivo regular e o retorno de `os.write` é ignorado (`bin/disjuntor.py:1487`, `bin/disjuntor.py:1489`, `bin/disjuntor.py:1491`). Escrita parcial faz escopo e geração discordarem, causando recusa posterior; não identifiquei liberação indevida por essa falha isolada, porque o fallback compara igualdade e recusa geração vazia (`bin/disjuntor.py:1501`, `bin/disjuntor.py:1549`). Não há `fsync` entre geração, estado e escopo; comportamento após queda do host é [não verificado] (`bin/disjuntor.py:1491`, `bin/disjuntor.py:281`, `bin/disjuntor.py:1521`).

Mapa:

| Estado | Escritores | Leitores e observação |
|---|---|---|
| `PDC_GERACAO` | `_pdc_geracao_grava`, invocado no callback PDC sob flock (`bin/disjuntor.py:1454`, `bin/disjuntor.py:1487`). | `_pdc_geracao_lida`, somente no fallback de `estado_pdc` (`bin/disjuntor.py:1499`, `bin/disjuntor.py:1548`); leitor indireto de mtime do Stop não foi migrado (`bin/hook_stop_fila1.py:578`, `bin/hook_stop_fila1.py:585`). |
| `escopo.geracao` | `_pdc_grava_escopo`, após estado gravado e ainda sob flock (`bin/disjuntor.py:1462`, `bin/disjuntor.py:1514`, `bin/disjuntor.py:292`). | `_pdc_escopo_lido` carrega JSON, `estado_pdc` compara com geração (`bin/disjuntor.py:1529`, `bin/disjuntor.py:1549`). |
| Arquivo de estado | Porta produtiva `_com_estado`, com arquivo temporário e `replace` (`bin/disjuntor.py:244`, `bin/disjuntor.py:278`, `bin/disjuntor.py:281`). Também fixtures gravam diretamente, sem geração (`bin/tests/test_padrao_de_cima.sh:103`, `bin/tests/test_padrao_de_cima.sh:565`, `bin/tests/test_padrao_de_cima.sh:576`, `bin/tests/test_padrao_de_cima.sh:620`). | `_com_estado` lê para transição, `ler_estado` lê para consultas/painel (`bin/disjuntor.py:267`, `bin/disjuntor.py:302`, `bin/disjuntor.py:1636`). |
| `pdc.eventos`, `nomeados`, `libera_vistas`, `fotos`, `disparadas` | Callback de `decide_pdc`; não encontrei segundo escritor produtivo no censo textual, mas testes editam diretamente (`bin/disjuntor.py:1385`, `bin/disjuntor.py:1402`, `bin/disjuntor.py:1440`, `bin/disjuntor.py:1445`, `bin/disjuntor.py:1449`, `bin/tests/test_padrao_de_cima.sh:560`, `bin/tests/test_padrao_de_cima.sh:572`). | Decisão, agrupamento, janela e painel (`bin/disjuntor.py:1391`, `bin/disjuntor.py:1304`, `bin/disjuntor.py:1311`, `bin/disjuntor.py:1636`). |

Os demais callbacks de `_com_estado` continuam escritores do **mesmo arquivo**, mas alteram outras chaves e preservam PDC; não precisam incrementar geração quando só muda B1/B3/B4/triagem (`bin/disjuntor.py:481`, `bin/disjuntor.py:831`, `bin/disjuntor.py:908`, `bin/disjuntor.py:1033`, `bin/disjuntor.py:1778`, `bin/disjuntor.py:1792`, `bin/disjuntor.py:2058`, `bin/disjuntor.py:2060`). O painel continua lendo PDC sem geração, mas sua função é apresentar contagem, não liberar ferramenta (`bin/disjuntor.py:1633`, `bin/disjuntor.py:1644`).

A cura de `DISJUNTOR_DESLIGADO` no ramo de exceção está conectada à mesma autoridade `desligado()` usada no caminho normal e no wrapper; não cria juiz concorrente (`bin/disjuntor.py:151`, `bin/disjuntor.py:1600`, `bin/disjuntor.py:2085`, `bin/disjuntor.py:2096`). O caso novo verifica stdout vazio, sem verificar rc ou conteúdo JSON da negativa; qualquer saída antecipada sem stdout também satisfaz essa parte isolada (`bin/tests/test_padrao_de_cima.sh:328`, `bin/tests/test_padrao_de_cima.sh:329`).

## 31/10 21:47 · sessao d420147522b3 · - · - · origem=Ronald (tmux)

Não comprovei, nos fluxos de views/services/signals/models examinados, sequência de produção que altere insumo omitido da chave e volte ao C6 sobre a mesma `PerguntaDisputa` ou mesma `p.celula`; os candidatos abaixo reutilizam objetos somente em leitura, alteram campos da chave ou recarregam as perguntas. (`app/chamados/services/fio.py:222`, `app/chamados/models.py:1420`, `app/chamados/services/validacao.py:359`)

| Fluxo | Sequência e conclusão |
|---|---|
| Render do fio/modal | Carrega `p` com `select_related` e `prefetch_related` → guarda as mesmas instâncias em listas → anexa a `ch.perguntas_disputa` → chama `.viva` → passa a lista ao explicador. Este preserva os objetos e chama `p.estado` e depois `fio_vivo(ps)`. Há reutilização real do memo, sem alteração dos insumos do C6 comprovada nesse trecho. (`app/chamados/services/fio.py:222`, `app/chamados/services/fio.py:239`, `app/chamados/services/fio.py:249`, `app/chamados/services/fio.py:279`, `app/chamados/services/fio.py:308`, `app/chamados/services/explicador.py:124`, `app/chamados/services/explicador.py:133`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:2479`) |
| Gate de fechamento do chamado | `_plano_pergunta_viva` guarda as perguntas em `self._f1_ancoradas`; `_gravar_estado` recupera a lista e chama `_vivas_entre` novamente. O julgamento temporário muda `status_local` e aponta `p.chamado_destino` para `self`; destino e status do destino estão na chave. (`app/chamados/models.py:362`, `app/chamados/models.py:210`, `app/chamados/models.py:218`, `app/chamados/models.py:339`, `app/chamados/models.py:348`, `app/chamados/juizes.py:794`) |
| Desarquivar / toque humano | `ch.reabrir()` chama `ressuscitar_perguntas`; depois `arquivo.levantar` salva e grava a trilha do toque; a view chama `ressuscitar_perguntas` novamente. Entretanto, cada chamada cria queryset novo, copia cada `p`, copia `_state.fields_cache`, substitui a célula pela projeção do julgamento e remove `_pv_memo` antes do C6. Não é prova de reutilização do memo anterior. (`app/chamados/views_arquivar.py:70`, `app/chamados/models.py:1389`, `app/chamados/views_arquivar.py:77`, `app/chamados/services/arquivo.py:208`, `app/chamados/services/arquivo.py:209`, `app/chamados/views_arquivar.py:86`, `app/chamados/models.py:1420`, `app/chamados/models.py:1427`, `app/chamados/models.py:1430`, `app/chamados/models.py:1434`, `app/chamados/models.py:1439`) |
| Responder pelo PWA | C6 sobre `p` → eventual segunda consulta ao C6 **antes** da escrita → core altera resposta, respondida, validação e carimbos da chave → pode criar ausência → retorna sem nova chamada ao C6 sobre o `p` original. As perguntas da absorção são obtidas por novo filtro. (`app/chamados/views.py:1242`, `app/chamados/views.py:1257`, `app/chamados/views.py:1283`, `app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1265`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/materializacao.py:501`, `app/chamados/services/materializacao.py:509`, `app/chamados/services/disputa_emissao.py:1324`) |
| Responder pela API | Carrega e julga `p` → chama o mesmo core → devolve resposta HTTP; não volta ao C6 sobre `p`. (`app/api/views.py:2680`, `app/api/views.py:2683`, `app/api/views.py:2695`, `app/api/views.py:2702`) |
| Resgate pela resposta | Usa o destino já acessível pela pergunta, mas escreve arquivo/silêncio e trilha `resgatar_por_resposta`; essa ação não está entre os três toques humanos reconhecidos pelo C6. (`app/chamados/services/materializacao.py:658`, `app/chamados/services/materializacao.py:665`, `app/chamados/juizes.py:935`) |
| Validar pergunta | Recebe ID e faz `.get()` de nova instância; carimba validação, materializa e atualiza os carimbos com `refresh_from_db`. Não há consulta posterior ao C6 sobre `perg` nessa função. A cauda consulta `perguntas_vivas_de`, que usa queryset com `order_by`; o signal da pergunta transmite somente `disputa_id` e o serviço de becos recarrega disputa e perguntas. (`app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:424`, `app/chamados/services/validacao.py:430`, `app/chamados/services/validacao.py:433`, `app/chamados/services/materializacao.py:1233`, `app/chamados/juizes.py:1130`, `app/chamados/signals.py:195`, `app/chamados/signals.py:201`, `app/chamados/services/becos.py:67`) |
| Desfazer carimbo sem lastro | Recebe perguntas, mas substitui cada recebida por releitura com `select_for_update`; chama C6 e só depois muda `materializada_em`/`via_resolucao`, presentes na chave. (`app/chamados/services/relavra_via.py:213`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/relavra_via.py:231`, `app/chamados/juizes.py:790`) |
| Relatório de disputas | Prefetch guarda perguntas/célula/destino e `classificar` chama C6 nas perguntas desse cache; o fluxo apresentado somente monta e ordena resultados. (`app/chamados/relatorios_disputa.py:39`, `app/chamados/relatorios_disputa.py:42`, `app/chamados/relatorios_disputa.py:11`, `app/chamados/relatorios_disputa.py:17`, `app/chamados/relatorios_disputa.py:44`, `app/chamados/relatorios_disputa.py:51`) |

O memo não tem prazo baseado em request no corpo executável: ele fica no atributo da instância e é reutilizado enquanto a chave coincidir; a morte com o request é descrita no comentário, não implementada por um encerramento ou limpeza ali. (`app/chamados/juizes.py:809`, `app/chamados/juizes.py:812`, `app/chamados/juizes.py:824`)

## 31/10 21:46 · sessao 5dca7ee72020 · - · - · origem=Ronald (tmux)

A tupla **não cobre todos os insumos lidos pelo cálculo**: compara apenas os campos declarados em `app/chamados/juizes.py:788-796`; o hit retorna o resultado sem reexecutar os leitores abaixo (`app/chamados/juizes.py:812-816`).

| Insumo fora da chave | Leitura comprovada |
|---|---|
| `p.motivo` | Seleciona marco em `fato_do_marco_no_espelho` (`app/chamados/juizes.py:863`), concordância (`:1000`), data contextual do turno (`:362`) e derivação do plantio (`app/chamados/services/materializacao.py:327-329`). |
| `p.celula.dna.marcos` — entrada, saída e dois pares de pausas | `app/chamados/juizes.py:870-884`, `:1002-1007`; pares declarados em `app/escala/models.py:1620`. |
| `p.celula.ata.lampadas`: `hora`, `tipo`, `acesa` | `app/chamados/juizes.py:887-893`. |
| `p.celula.ata.lampadas.luz` | Concordância resposta/batida em `app/chamados/juizes.py:1008-1028`. |
| `p.celula.ata.lampadas.pendente` | C6 chama `marcos_pendentes` em `app/chamados/juizes.py:1094-1096`; implementação lê `cel.ata.lampadas[*].pendente` em `app/ponto/services/lastro.py:148-150`. |
| `p.celula.trabalha` | `_celulas_da_pergunta` repassa a mesma célula, mediante identidade e campos carregados (`app/chamados/juizes.py:598-606`); `classificar_falta` lê `trabalha` (`app/escala/servico_jornada.py:302-304`). |
| Célula/data/colaborador e estado de campos diferidos | Definem se o cálculo recebe `p.celula` ou consulta outra célula: `app/chamados/juizes.py:602-605`; consulta alternativa em `app/escala/servico_jornada.py:267-271`. |
| Disputa e colaborador, inclusive `empresa_id` e empresa | `dia_da_pergunta_trancado` em `app/chamados/juizes.py:909-921`; colaborador usado em classificação e testemunhas em `:1100-1102`, `:729-731`, `:751`. |
| Corte de competência `empresa.dia_inicio_competencia` | Janela da tranca em `app/chamados/juizes.py:918`; autoridade lê o campo em `app/ponto/janelas.py:50-51`, `:60-63`. |
| Tranca viva: existência de `PeriodoFechado`, empresa/mês/ano, `reaberto_em`; data `fechado_em` | `app/ponto/services/fechamento.py:1432-1438`; utilização e toque posterior em `app/chamados/juizes.py:919-928`. |
| Trilhas humanas posteriores à tranca; existência das células/chamados relacionados | `app/chamados/juizes.py:971-978`: `LogAuditoria.criado_em`, usuário, ação, modelo, objeto e descrição. |
| Relógio/data atual | `_dentro_da_janela_viva` usa `date.today` e janelas corrente/anterior (`app/chamados/juizes.py:539-565`); classificação compara `timezone.now()` ao fim previsto (`app/escala/servico_jornada.py:375-378`). |
| Ausências: existência, colaborador, `data`, `data_fim`, `status`, `tipo`, `minutos`; cadastro de tipos parciais | Classificação em `app/escala/servico_jornada.py:340-345`; seleção em `app/ponto/turnos.py:2509-2533`; parcialidade em `:2595-2600`, que consulta `parciais_vigentes`. |
| Batidas plantadas relacionadas à pergunta: existência, `timestamp`, `retratada_em` | `app/chamados/juizes.py:695-702`; testes de horário em `:258-266`. |
| Outras batidas: existência, colaborador, tipo, timestamp e retratação | Consulta da testemunha em `app/chamados/juizes.py:225-237`; retratação em `:751-753`; universo de turnos em `app/ponto/turnos.py:1859-1864`. |
| Trilha de retratação da própria pergunta, inclusive `p.pk` | `app/chamados/juizes.py:275-280`. |
| `disputa.contexto[motivo].data` e `.hora_tentativa` | Completude do turno em `app/chamados/juizes.py:359-370`; derivação do slot em `app/chamados/services/materializacao.py:329-333`, `:406`. |
| `chamado_destino.contexto_json.marcos_faltantes`, incluindo motivo/hora/tipo | `app/chamados/juizes.py:498-529`; usado na derivação em `app/chamados/services/materializacao.py:259-261`, `:394-397`. O status do destino já carregado **está** na chave; o JSON não (`app/chamados/juizes.py:794-795`). |
| Vínculos, geradora da célula, vigência e template de escala | Classificação chama `escala_vigente` (`app/escala/servico_jornada.py:354`), cujo carregador lê geradora/vínculos (`:46-58`); escolha depende de geradora, datas e atividade (`app/escala/alimentacao.py:237-254`). |
| Marcos congelados ou template quando não há DNA utilizável | `app/escala/servico_jornada.py:427-435`; implementação em `app/escala/models.py:1094-1108`. Overnight usa esses marcos em `app/chamados/juizes.py:300-311`. |
| Ata/DNA e escalas da janela de turnos | `_turno_tem_saida` chama `turnos_do_colab` (`app/chamados/juizes.py:369-370`); carregamento em `app/ponto/turnos.py:1882-1886`, `:1907-1913`; papel da ata em `:1937`; marcos e pareamento em `:1982-1993`. |
| Dados adicionais do colaborador e célula, por `grade_da_celula` no cálculo do slot | `_marcos_da_regua` e `_tipo_pelo_marco` chamam a grade (`app/chamados/services/materializacao.py:218-227`, `:267-268`); ela lê `data_admissao`, `data_demissao`, `isento_ponto`, `ata` e `trabalha` (`app/escala/services/leitor_celula.py:321-372`; flag de isenção em `app/core/constants.py:18`). |

Precisões para a consolidação:

- **“Lastro” aqui é módulo, não uma leitura de campo `cel.lastro`**: `marcos_pendentes` lê exclusivamente as lâmpadas da ata (`app/ponto/services/lastro.py:138-150`). “Marcos” são lidos principalmente de `dna['marcos']` e de `ata['lampadas']` (`app/chamados/juizes.py:870`, `:888`).
- **C6 não consulta automaticamente a ata/veredito em curso do cartório.** O leitor que faz isso é `celula_deste_julgamento`, que consulta os dois mapas (`app/chamados/juizes.py:392-395`); estes vêm de `_tl.atas` e `_tl.vereditos` (`app/ponto/services/cartorio.py:57`, `:66`).
- Essa projeção faz **cópia rasa da célula**, substitui ata e eventualmente o par de veredito (`app/ponto/portas/celula.py:675-683`). Portanto, distinguir “estado do cartório mudou” de “a mesma `p.celula` passou a refletir esse estado” é necessário; o último não acontece implicitamente nesses leitores (`app/chamados/juizes.py:394-395`; `app/ponto/portas/celula.py:678-683`).
- A tranca tem **outro memo**, fora de `_pv_memo`: fica no request ou colaborador (`app/chamados/juizes.py:944-948`), guarda resultado da consulta de tranca inclusive `None` (`:920-923`), e guarda apenas o resultado positivo do toque humano (`:965-966`, `:979-980`). Invalidar `_pv_memo` isoladamente não prova nova consulta da tranca (`app/chamados/juizes.py:914-923`).

## 31/10 21:33 · sessao a047dbbf9d5a · O261-E-F1C4-LEN-VIVOS · censo · origem=Code

Há **quatro sítios de chamada, em três arquivos produtivos**: dois no cartório e um em cada comando. As chamadas de relatórios/inteligência usam outro `classificar_dia`. (`app/ponto/services/cartorio.py:1028`, `app/ponto/services/cartorio.py:1212`, `app/ponto/management/commands/supra_juiz.py:206`, `app/ponto/management/commands/emitir_furo_retroativo.py:99`, `app/relatorios/furos.py:55`, `app/inteligencia/calculadores.py:26`)

O censo abaixo descreve a cópia local. Ocorrências, quantidades e efeitos efetivamente observados em produção nesta obra: **[nao verificado]**.

**1) Onde o classificador lê os parâmetros**

O contrato distingue explicitamente `chamados_vivos`/`chamados_total`, relativos ao **chamado**, de `cobrancas_vivas`, relativo ao **fio vivo**. Se `isento_ponto=True`, a função retorna lista vazia antes desses ramos. (`app/ponto/supra_juiz.py:204`, `app/ponto/supra_juiz.py:213`, `app/ponto/supra_juiz.py:220`)

| Parâmetro lido | Código acrescentado | Condições |
|---|---|---|
| `chamados_vivos` | `COBRANCA_SEM_FURO` | `tipo_dia == 'trabalho'`, previsto não zero, realizado ≥ previsto, `chamados_vivos` verdadeiro e `todas_casadas`. Esta última condição exige células não vazias e contagem de casadas igual ao tamanho da lista; prefere `n_casadas` lavrado e, sem ele, conta células com hora. (`app/ponto/supra_juiz.py:439`) |
| `chamados_vivos` | `COBRANCA_EM_DIA_SEM_TRABALHO` | Tipo fora de `SEM_JURISDICAO`, previsto zero, `chamados_vivos` verdadeiro, `cobertura_status is None` e resposta positiva de `_dia_sem_trabalho_pelo_juiz`. (`app/ponto/supra_juiz.py:470`) |
| `chamados_vivos` | `COBRANCA_EM_DIA_COBERTO` | Cobertura com status exatamente `'aprovada'` e `chamados_vivos` verdadeiro; esse ramo não exige cobertura integral. (`app/ponto/supra_juiz.py:255`, `app/ponto/supra_juiz.py:264`, `app/ponto/supra_juiz.py:501`) |

`SEM_JURISDICAO` contém `fora_vinculo`, `sem_celula` e `indefinido`. O juiz de dia sem trabalho responde `trabalha is False` **ou** `ata.tipo_dia` preenchido e diferente de `'trabalho'`; o classificador recebe esses insumos pelo dicionário, sem fallback quando a chave falta. (`app/ponto/supra_juiz.py:48`, `app/ponto/supra_juiz.py:65`, `app/chamados/juizes.py:398`)

Esses três ramos não exigem `turno_encerrado`; essa guarda pertence aos ramos de furo/fato indicados no código. (`app/ponto/supra_juiz.py:278`, `app/ponto/supra_juiz.py:443`, `app/ponto/supra_juiz.py:470`, `app/ponto/supra_juiz.py:501`)

`chamados_total` é lido **uma vez**, no `elif` que escolhe `FURO_COM_COBRANCA_MORTA`; se falso, o `else` escolhe `FURO_SEM_COBRANCA`. Para alcançar essa escolha, precisam valer todas estas condições: dia de cobrança — trabalho ou ausência parcial —, previsto não zero, realizado zero, nenhuma célula casada, nenhuma batida no dia, nenhuma `cobrancas_vivas`, ausência sem cobrir integralmente o dia e turno encerrado. (`app/ponto/supra_juiz.py:304`, `app/ponto/supra_juiz.py:352`)

Antes de consultar `chamados_total`, o mesmo bloco dá precedência a:

- pré-adoção → `FURO_PRE_ADOCAO`;
- sem vínculo → `PREVISTO_COM_DIA_INDEFINIDO`;
- sem âncora e sem fase → `FASE_NAO_DECLARADA_COM_PREVISTO`;
- régua degenerada → `FURO_COM_REGUA_DEGENERADA`;
- sem âncora → `PREVISTO_COM_DIA_INDEFINIDO`.  
  (`app/ponto/supra_juiz.py:326`, `app/ponto/supra_juiz.py:329`, `app/ponto/supra_juiz.py:331`, `app/ponto/supra_juiz.py:340`, `app/ponto/supra_juiz.py:348`)

Se `dia['justificado']` for verdadeiro, o bloco remove os códigos recém-acrescentados cujo nome começa por `FURO_`. Portanto, os dois resultados dependentes de `chamados_total` podem ser removidos antes do retorno. (`app/ponto/supra_juiz.py:363`)

**2–3) Todos os chamadores, seus universos e o consumo dos códigos**

Nos universos abaixo, `VIVOS` significa `aberto`, `em_analise` e **`registrado`**. Não equivale a `COBRANCA`, que contém somente `aberto` e `em_analise`. `FURO_MODULES` contém `batida_ausente`, `turno_aberto`, `intervalo_pendente` e `entrada_bloqueada_turno_aberto`. (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/furo_modulos.py:15`)

**Cartório, ramo normal — chamada em `app/ponto/services/cartorio.py:1212`.**

- `lista`: chamados do colaborador em `FURO_MODULES`, **todos os status**, agrupados pelo valor de `data_do_chamado(modulo, contexto)` convertido em string; seleciona a chave do dia da célula. O agrupamento não usa `ch.celula.data`. (`app/ponto/services/cartorio.py:81`, `app/ponto/services/cartorio.py:96`, `app/ponto/services/cartorio.py:104`, `app/ponto/services/cartorio.py:1107`)
- `vivos`: elementos de `lista` com status em `VIVOS`. `_com_fio`: esses vivos para os quais `fio_vivo(x.perguntas_que_resolvem.all())` responde verdadeiro. (`app/ponto/services/cartorio.py:1136`, `app/ponto/services/cartorio.py:1141`)
- `_protestam`: vivos para os quais **`cel.trabalha is not False` e `marcos_acesos(x, ata_deste_julgamento) is not True`**. Portanto, `trabalha=None` permanece nesse universo; resultado desconhecido de marcos também permanece. (`app/ponto/services/cartorio.py:1184`, `app/ponto/services/cartorio.py:1206`)
- Passa **`chamados_vivos=len(_protestam)`**, **`chamados_total=len(lista)`** e **`cobrancas_vivas=len(_com_fio)`**. O filtro `_protesta` não é aplicado a `_com_fio` nessa passagem. (`app/ponto/services/cartorio.py:1212`)
- Julga as células recebidas; pode pular impressão já julgada e pode recortar o dia pelos marcos vencidos quando opera com teto. (`app/ponto/services/cartorio.py:1105`, `app/ponto/services/cartorio.py:1115`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/cartorio.py:1177`)

O destino dos resultados é:

| Resultado | O que o cartório faz |
|---|---|
| **Todos os códigos** | Guarda em `out['cods']`, conta dia com protesto e, com `apply_`, entrega códigos e ata à lavra da célula. (`app/ponto/services/cartorio.py:1224`, `app/ponto/services/cartorio.py:1448`, `app/ponto/services/cartorio.py:1565`) |
| `COBRANCA_SEM_FURO`, `COBRANCA_EM_DIA_SEM_TRABALHO` | Além da lavra, com `apply_`, tenta `reconciliar()` nos vivos; contabiliza fechamentos e pede rejulgamento. Aqui chama **`reconciliar` diretamente**, não `retratar`. (`app/ponto/services/cartorio.py:1313`) |
| `COBRANCA_EM_DIA_COBERTO` | Entra na lavra/derivação de veredito; não tem o executor específico dos dois códigos anteriores. (`app/ponto/services/cartorio.py:1313`, `app/ponto/services/cartorio.py:1565`, `app/escala/models.py:1582`) |
| `FURO_SEM_COBRANCA`, `FURO_COM_COBRANCA_MORTA` | Podem acionar emissão, com guardas de chamado vivo, horizonte/toque, desligamento, afastamento, vínculo, dia de trabalho conhecido e horário inicial. O código de cobrança morta também procura nomear os marcos apagados. (`app/ponto/services/cartorio.py:1475`, `app/ponto/services/cartorio.py:1483`, `app/ponto/services/cartorio.py:1488`, `app/ponto/services/cartorio.py:1495`, `app/ponto/services/cartorio.py:1500`) |
| `FURO_PARCIAL` | Conta emissão potencial; com `apply_`, abre chamado se não há vivos ou atualiza os marcos do `batida_ausente` existente, respeitando as guardas mostradas nesse bloco. (`app/ponto/services/cartorio.py:1537`) |
| `PREVISTO_COM_DIA_INDEFINIDO` | Também pode participar da emissão no caso específico `_limbo_de_hoje`. (`app/ponto/services/cartorio.py:1492`) |
| Demais códigos | Seguem para contagem/lavra e para o mapa de veredito; não têm outros executores por código nesse corpo. (`app/ponto/services/cartorio.py:1448`, `app/ponto/services/cartorio.py:1488`, `app/ponto/services/cartorio.py:1537`, `app/ponto/services/cartorio.py:1565`) |

Há ainda reconciliação **independente dos códigos** para chamados que não protestam e para marcos inexistentes. Quando necessário, o fluxo adia o fechamento e executa **lavra → E2 → reconciliar → rejulgamento**. (`app/ponto/services/cartorio.py:1281`, `app/ponto/services/cartorio.py:1298`, `app/ponto/services/cartorio.py:780`)

Sem códigos, pode lavrar `cobrado` se houver `_com_fio` que também satisfaça `_protesta`; caso contrário, o mapa vazio produz `concorde`. (`app/ponto/services/cartorio.py:1227`, `app/ponto/services/cartorio.py:1250`, `app/escala/models.py:1594`)

**Cartório, ramo `nunca_bateu` — chamada em `app/ponto/services/cartorio.py:1028`.**

Esse ramo é alcançado quando não existe primeira batida não retratada. Passa **zero literal** para `chamados_vivos`, `chamados_total` e `cobrancas_vivas`; não realiza um censo de chamados para esses argumentos. A chamada ocorre dentro de `if apply_`. (`app/ponto/services/cartorio.py:957`, `app/ponto/services/cartorio.py:965`, `app/ponto/services/cartorio.py:1009`, `app/ponto/services/cartorio.py:1029`)

Deriva o veredito dos códigos retornados; sem cobertura integral, substitui-o por `nunca_bateu`. Lava o resultado com **`codigos=[]`**, mesmo que a classificação tenha retornado códigos, e retorna antes do fluxo normal de emissão. Os três códigos que exigem `chamados_vivos` não podem nascer nessa chamada. (`app/ponto/services/cartorio.py:1033`, `app/ponto/services/cartorio.py:1043`, `app/ponto/services/cartorio.py:1044`, `app/ponto/services/cartorio.py:1070`, `app/ponto/supra_juiz.py:443`, `app/ponto/supra_juiz.py:470`, `app/ponto/supra_juiz.py:501`)

**Comando `supra_juiz` — chamada em `app/ponto/management/commands/supra_juiz.py:206`.**

Seleciona colaboradores ativos da empresa, pula quem não tem batida na janela carregada e julga dias de `grade_da_celula` dentro do intervalo solicitado, corrente ou anterior. (`app/ponto/management/commands/supra_juiz.py:74`, `app/ponto/management/commands/supra_juiz.py:97`, `app/ponto/management/commands/supra_juiz.py:116`, `app/ponto/management/commands/supra_juiz.py:188`)

Busca chamados em `FURO_MODULES`, todos os status, e agrupa pelo catálogo `data_do_chamado`. Passa **`len(vivos)`**, **`len(lista)`** e **`len(_com_fio)`**; não aplica ao contador de vivos o filtro de `trabalha` ou de marcos acesos do cartório. (`app/ponto/management/commands/supra_juiz.py:169`, `app/ponto/management/commands/supra_juiz.py:182`, `app/ponto/management/commands/supra_juiz.py:194`, `app/ponto/management/commands/supra_juiz.py:206`)

- **Todos os códigos** alimentam placar, contagem por colaborador e exemplos. (`app/ponto/management/commands/supra_juiz.py:256`)
- **`COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO`** acionam o executor: percorre todos os `vivos`, preserva conversa respondida e cobrança órfã de regeneração; com `--executar` e trava desligada, chama `retratar`. A trava está em `False`. A chamada ao retratador não garante fechamento, pois a porta ainda pode recusar. (`app/ponto/management/commands/supra_juiz.py:41`, `app/ponto/management/commands/supra_juiz.py:218`, `app/ponto/supra_juiz.py:509`, `app/chamados/reconciliador.py:520`)
- **`COBRANCA_EM_DIA_COBERTO`** apenas entra nas contagens desse comando. (`app/ponto/management/commands/supra_juiz.py:218`, `app/ponto/management/commands/supra_juiz.py:256`)
- `--alarme` examina somente `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`, podendo sair com código 2. (`app/ponto/management/commands/supra_juiz.py:268`)

**Comando `emitir_furo_retroativo` — chamada em `app/ponto/management/commands/emitir_furo_retroativo.py:99`.**

Seleciona colaboradores ativos da empresa, exige alguma batida na janela carregada e lê os dias pela célula. Busca chamados em `FURO_MODULES`, todos os status, mas agrupa por **`dia_do_fato(ch, fortes=True)`**: célula primeiro, depois data declarada no catálogo/fallback `data_turno`. (`app/ponto/management/commands/emitir_furo_retroativo.py:51`, `app/ponto/management/commands/emitir_furo_retroativo.py:59`, `app/ponto/management/commands/emitir_furo_retroativo.py:77`, `app/chamados/juizes.py:88`, `app/chamados/juizes.py:104`)

Passa **`len(vivos)`**, **`len(lista)`** e **`len(_com_fio)`**, sem filtro de marcos acesos ou `trabalha` na lista de vivos. (`app/ponto/management/commands/emitir_furo_retroativo.py:92`)

**Consome exclusivamente `FURO_SEM_COBRANCA`. Portanto, ignora todos os três códigos dependentes de `chamados_vivos`.** Também ignora `FURO_COM_COBRANCA_MORTA`, embora `chamados_total` possa fazer o juiz escolhê-lo no lugar do código consumido. (`app/ponto/management/commands/emitir_furo_retroativo.py:109`, `app/ponto/supra_juiz.py:352`)

Para emitir, ainda exige vínculo, `eh_dia_trabalho is True`, horário inicial e `lista` vazia; com `--apply`, chama o emissor canônico `_abrir_chamado`. Não lavra diretamente veredito da célula nessa chamada. (`app/ponto/management/commands/emitir_furo_retroativo.py:118`, `app/ponto/management/commands/emitir_furo_retroativo.py:127`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/management/commands/emitir_furo_retroativo.py:136`, `app/ponto/management/commands/emitir_furo_retroativo.py:142`)

**4) Escritores, leitores e textos normativos**

O mapa converte os códigos de furo em `furo`; os códigos `COBRANCA_SEM_FURO`/`COBRANCA_EM_DIA_SEM_TRABALHO` em `cobranca_indevida`; e `COBRANCA_EM_DIA_COBERTO` em `coberta`. Havendo vários códigos, vence o primeiro grupo correspondente no mapa. (`app/escala/models.py:1579`, `app/escala/models.py:1597`)

| Estado/campo | Quem escreve | Quem lê no circuito dos efeitos |
|---|---|---|
| `ChamadoColaborador.status_local` | Nascimento por `abrir`; transição por `_gravar_estado`, chamada por `reconciliar`. `RECONCILIAR` leva os estados vivos a `resolvido`, sujeito aos gates. (`app/chamados/models.py:1046`, `app/chamados/models.py:222`, `app/chamados/models.py:605`, `app/chamados/catalogo/motor.py:109`) | Os contadores dos três chamadores; signals de encerramento/rejulgamento; painel/fila; fábrica de disputa. (`app/ponto/services/cartorio.py:1136`, `app/ponto/management/commands/supra_juiz.py:195`, `app/ponto/management/commands/emitir_furo_retroativo.py:93`, `app/chamados/signals.py:68`, `app/chamados/signals.py:155`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:186`, `app/chamados/services/disputa_emissao.py:690`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `CelulaDia.lavrar_veredito` delega à porta `ponto.portas.celula.lavrar_veredito`; esta atribui os campos. O cartório fornece códigos/ata; a decisão humana usa a mesma porta. (`app/escala/models.py:1787`, `app/ponto/portas/celula.py:709`, `app/ponto/portas/celula.py:768`, `app/ponto/services/cartorio.py:1565`, `app/ponto/services/veredito_celula.py:330`) | Leitor da célula, juiz de pergunta viva, fábrica, reconciliador e retenção da folha. (`app/escala/services/leitor_celula.py:345`, `app/chamados/juizes.py:1073`, `app/chamados/services/fabrica_da_celula.py:78`, `app/chamados/reconciliador.py:949`, `app/folha/export.py:1215`) |
| `ata.lampadas` / `acesa` | Produzidas do casamento das células por `lampadas_do_dia`, agregadas por `ata_do_dia`; dispensa por ausência/justificativa também pode acender lâmpadas. Persistem pela lavra. **Os códigos não atribuem `acesa` diretamente.** (`app/ponto/services/cartorio.py:429`, `app/ponto/services/cartorio.py:541`, `app/ponto/services/cartorio.py:560`, `app/ponto/services/cartorio.py:611`, `app/ponto/services/cartorio.py:1565`) | `_protesta` via `marcos_acesos`, juiz de pergunta viva, `_celula_furada` e leitor da célula. (`app/ponto/services/cartorio.py:1206`, `app/ponto/services/lastro.py:153`, `app/chamados/juizes.py:1073`, `app/chamados/juizes.py:442`, `app/escala/services/leitor_celula.py:63`) |
| `status_dia` / `status_rotulo` | A mesma porta de lavra calcula e aplica o status com o veredito dessa lavra. (`app/ponto/portas/celula.py:686`, `app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:773`) | A autoridade de leitura do status lavrado é `status_do_dia_lavrado`, registrada na tabela de juízes. (`app/docs/JUIZES.md:81`) |
| Estado das perguntas alcançadas pelo fechamento | `retratar` chama `reconciliar`, resolve perguntas, pode rejeitar justificativa pendente vinculada e anexa ocorrência; no fluxo adiado do cartório, E2 carimba as perguntas absolvidas após a lavra. (`app/chamados/reconciliador.py:505`, `app/chamados/reconciliador.py:531`, `app/chamados/reconciliador.py:538`, `app/ponto/services/cartorio.py:787`) | `pergunta_viva` e `fio_vivo`; este último realimenta `cobrancas_vivas` nos três chamadores. (`app/chamados/juizes.py:1073`, `app/chamados/juizes.py:1133`, `app/ponto/services/cartorio.py:1141`, `app/ponto/management/commands/supra_juiz.py:198`, `app/ponto/management/commands/emitir_furo_retroativo.py:96`) |

A porta preserva veredito humano diante da lavra do cartório, podendo atualizar ata/status; a lavra humana conserva os protestos existentes. `furo` e `cobranca_indevida` estão em `VEREDITOS_ACUSAM`; `coberta` não está. A folha lê esse conjunto e os protestos para decidir retenção. (`app/ponto/portas/celula.py:737`, `app/ponto/portas/celula.py:751`, `app/escala/models.py:1735`, `app/folha/export.py:1215`)

A tabela acima cobre o circuito rastreado desses efeitos; exaustividade de **todo** leitor genérico desses campos em telas, APIs, relatórios e vigias: **[nao verificado]**.

Há textos normativos relevantes, com alcances distintos:

- **Lei geral:** “O CONSUMIDOR LE A LAMPADA, nunca re-julga: chamado, pergunta, tela e folha leem a celula/ata”. (`app/docs/LEIS.md:95`)
- **Corte que nomeia os três alimentadores:** “todo leitor de chamados_vivos (disputa_emissao.py:670, supra_juiz.py:163-168, core/juizes.py _C1, cartorio.py:780, commands/supra_juiz.py:182, emitir_furo_retroativo.py:79)” e “os leitores leem a lampada”. A numeração interna dessa citação é histórica. (`app/docs/CORTES.md:255`)
- **Corte sobre marcos acesos no julgamento:** “com todos os marcos acesos NESTE julgamento, o chamado nao conta como cobranca viva e o veredito nao le o proprio chamado vivo (4a).” (`app/docs/CORTES.md:405`)
- **Extensão:** “o veredito nunca le o proprio chamado vivo vale para TODA lampada, inclusive 'cobrado' em dia sem trabalho”. (`app/docs/CORTES.md:412`)
- **Separação das perguntas:** “`VIVOS` responde so a primeira metade”, distinguindo chamado vivo de fio; o registro dá autoridade própria a “alguma pergunta desta colecao esta viva?”. (`app/core/juizes.py:103`, `app/core/juizes.py:125`)
- **Limite explícito de `_protesta`:** “NAO MIGRADO”; lê “so a 1a clausula (`trabalha is not False`, alem dos marcos acesos”; “com trabalha=False zera chamados_vivos antes de classificar_dia”; a segunda cláusula, `ata.tipo_dia`, “seria predicado novo”. (`app/core/juizes.py:145`, `app/core/juizes.py:146`, `app/core/juizes.py:147`, `app/core/juizes.py:148`)
- **Vocabulário/dia:** “VIVOS/COBRANCA/TERMINAIS/NAO_FECHADOS, nunca tupla literal” e dia pelo catálogo, “nunca .get('data_turno') cru”. (`app/docs/JUIZES.md:52`, `app/docs/JUIZES.md:53`)

Assim, existe corte abrangente nomeando os três alimentadores e especificação explícita do recorte atual de `_protesta`. Uma especificação documental individual completa de todos os filtros para cada comando: **[nao verificado]**. (`app/docs/CORTES.md:255`, `app/core/juizes.py:145`)

**5) Existe função única compartilhada para esse universo?**

**Não há uma seleção compartilhada que produza o universo completo passado como `chamados_vivos` pelos três arquivos.** Os comandos montam `vivos` próprios; o cartório acrescenta seu `_protesta` local. (`app/ponto/management/commands/supra_juiz.py:173`, `app/ponto/management/commands/supra_juiz.py:195`, `app/ponto/management/commands/emitir_furo_retroativo.py:80`, `app/ponto/management/commands/emitir_furo_retroativo.py:93`, `app/ponto/services/cartorio.py:1206`)

Existem autoridades compartilhadas de alcance parcial:

| Função | Resposta efetiva e reutilização |
|---|---|
| `fio_vivo(perguntas)` | Responde se há pergunta viva na coleção recebida. O contrato diz literalmente **“O UNIVERSO e do chamador”**; é usada nos três alimentadores, mas não seleciona chamados do dia. (`app/chamados/juizes.py:1133`, `app/ponto/services/cartorio.py:1141`, `app/ponto/management/commands/supra_juiz.py:198`, `app/ponto/manageme

[truncado em 20000 de 21058 caracteres]

## 31/10 21:32 · sessao 3c83aaaa49cc · - · - · origem=Ronald (tmux)

Censo dos chamadores: quatro sítios produtivos em três arquivos — `app/ponto/services/cartorio.py:1028`, `app/ponto/services/cartorio.py:1212`, `app/ponto/management/commands/supra_juiz.py:206` e `app/ponto/management/commands/emitir_furo_retroativo.py:99`. As chamadas de `app/inteligencia/calculadores.py:60` e `app/relatorios/furos.py:45,222,244` pertencem ao homônimo definido em `app/relatorios/furos.py:55`; a importação de inteligência confirma isso em `app/inteligencia/calculadores.py:26`.

**Comando `supra_juiz`:**

- Universo de colaboradores: empresa pedida, `situacao='ativo'`; pula o colaborador se nenhuma batida no intervalo de timestamps entre início−1 dia e fim+3 dias. Janela de dias pode ser explícita, anterior ou atual. (`app/ponto/management/commands/supra_juiz.py:74-105`)
- Lê os dias da célula por `grade_da_celula`, restringindo à janela. (`app/ponto/management/commands/supra_juiz.py:116`, `:188-191`)
- Busca chamados do colaborador em `FURO_MODULES`, **sem filtro de status nessa busca**, agrupa pelo retorno truthy de `data_do_chamado(modulo, contexto)` convertido em string. Não pergunta pela célula do chamado nesse agrupamento. (`app/ponto/management/commands/supra_juiz.py:169-185`)
- `lista = chs[iso]`; `vivos = status_local in VIVOS`; `_com_fio = vivos` com `fio_vivo(perguntas_que_resolvem)`. Passa `chamados_vivos=len(vivos)`, `chamados_total=len(lista)` e `cobrancas_vivas=len(_com_fio)`. Não restringe essas listas por `celula.trabalha` nem por marcos acesos. (`app/ponto/management/commands/supra_juiz.py:194-208`)
- Todo código retornado incrementa placar, placar por colaborador e exemplos; todos podem aparecer no relatório. (`app/ponto/management/commands/supra_juiz.py:256-267`, `:275-283`)
- Só `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO` entram no executor. Para esses códigos percorre **todos os `vivos`**, preserva conversa respondida por `pode_executar_csf` e cobrança órfã de regeneração; com `--executar` e trava desligada chama `chamados.reconciliador.retratar`, usando `motivo_retratacao`. A trava é literalmente `False`. (`app/ponto/management/commands/supra_juiz.py:41`, `:218-255`)
- `COBRANCA_EM_DIA_COBERTO` só entra nas contagens; não tem executor próprio. (`app/ponto/management/commands/supra_juiz.py:218`, `:256-259`)
- `--alarme` considera exclusivamente `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`, podendo sair com código 2. (`app/ponto/management/commands/supra_juiz.py:268-274`)
- O gate `pode_executar_csf` recusa qualquer pergunta respondida tanto em `disputa_origem` quanto em `perguntas_que_resolvem`. (`app/ponto/supra_juiz.py:509-520`)

**Comando `emitir_furo_retroativo`:**

- Janela explícita; colaboradores da empresa ativos; pula quem não tem batida na janela de timestamps início−1 até fim+3. Lê `grade_da_celula`. (`app/ponto/management/commands/emitir_furo_retroativo.py:40-59`)
- Busca todos os status dos chamados do colaborador em `FURO_MODULES`; agrupa por `dia_do_fato(ch, fortes=True)`, diferentemente do agrupamento direto pelo catálogo do comando anterior. (`app/ponto/management/commands/emitir_furo_retroativo.py:77-84`)
- `dia_do_fato(..., fortes=True)` prefere `celula.data`; depois a chave declarada no catálogo, com fallback para `contexto.data_turno`; interpreta como data e retorna `None` sem esses degraus. (`app/chamados/juizes.py:78-119`)
- Para cada dia da janela: `lista`, `vivos=VIVOS`, `_com_fio` por `fio_vivo`; passa os respectivos comprimentos. Sem filtro de marcos acesos nem de `trabalha` na formação de `vivos`. (`app/ponto/management/commands/emitir_furo_retroativo.py:88-108`)
- **Consome exclusivamente `FURO_SEM_COBRANCA`**; todos os outros códigos são descartados pelo `continue`. Logo ignora os três códigos que dependem diretamente de `chamados_vivos`. (`app/ponto/management/commands/emitir_furo_retroativo.py:109-110`)
- Encontrando esse código, exige vínculo vigente, `eh_dia_trabalho(data) is True`, primeiro marco presente e `lista` vazia; conta descartes nas categorias correspondentes. Com `--apply` chama `detectar_ausencias.Command._abrir_chamado`; sem a flag apenas imprime. (`app/ponto/management/commands/emitir_furo_retroativo.py:111-146`)
- `chamados_total` continua relevante: pode fazer o classificador escolher `FURO_COM_COBRANCA_MORTA` em vez de `FURO_SEM_COBRANCA`; o retroativo ignora o primeiro. (`app/ponto/supra_juiz.py:352-362`, `app/ponto/management/commands/emitir_furo_retroativo.py:109-110`)
- A chamada ao emissor não lavra célula diretamente; `_abrir_chamado` é documentado como criador de chamado com push, e recebe sem `marcos_faltantes` nessa via. (`app/ponto/management/commands/emitir_furo_retroativo.py:142`, `app/ponto/management/commands/detectar_ausencias.py:570-579`)

**Vocabulários comuns:**

- `FURO_MODULES`: `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto`. (`app/chamados/catalogo/furo_modulos.py:15-16`)
- `VIVOS`: `aberto`, `em_analise`, `registrado`; `COBRANCA` só `aberto`, `em_analise`. Portanto os contadores descritos incluem `registrado`. (`app/chamados/catalogo/motor.py:24-29`)
- `data_do_chamado` retorna o valor da chave declarada no catálogo, sem interpretar a data, e devolve `None` sem módulo/chave declarada. (`app/chamados/catalogo/modulos.py:219-225`)

**Questão 5 — distinguir funções existentes da função exata pedida:**

- Não há uma função compartilhada pelos três arquivos para produzir o universo de `chamados_vivos` passado ao classificador: os comandos montam suas próprias listas; o cartório monta `_protestam`. (`app/ponto/management/commands/supra_juiz.py:173-208`, `app/ponto/management/commands/emitir_furo_retroativo.py:80-101`, `app/ponto/services/cartorio.py:1206-1214`)
- Há `fio_vivo(perguntas)`, compartilhada, mas ela só responde se existe pergunta viva na lista recebida; seu próprio contrato diz literalmente: **“O UNIVERSO e do chamador”**. Não seleciona chamados do dia nem filtra módulo, marcos ou `trabalha`. (`app/chamados/juizes.py:1133-1142`)
- Há `cobranca_viva_do_dia(colaborador, modulo_origem, dia, excluir=None)`: retorna o chamado VIVOS mais novo dos módulos recebidos que bate com o dia forte; não verifica fio vivo, marcos acesos ou `trabalha`. (`app/chamados/juizes.py:1771-1787`)
- Essa função é reutilizada por cartório, triagem, entrada sincronizada e gates do modelo, mas **não monta os contadores dos chamadores do classificador**. (`app/ponto/services/cartorio.py:1382`, `app/ponto/services/triagem_batida.py:588-589`, `app/ponto/services/entrada_sincronizada.py:24-28`, `app/chamados/models.py:368`, `:690`)
- Há ainda `chamados_vivos_do_dia` em `veredito_celula`: une chamados VIVOS por `contexto.data_turno` e por relação com a célula, sem filtro de módulo, fio, marcos ou `trabalha`. Tem um uso direto, dentro de `fechar_o_fio_do_dia`. (`app/ponto/services/veredito_celula.py:136-157`, `:182`)
- `fechar_o_fio_do_dia` é reutilizada pelo veredito, pela declaração do dia e pela validação; isso não significa reutilização daquele universo na classificação do cartório/comandos. (`app/ponto/services/veredito_celula.py:325`, `app/ponto/services/declarar_dia.py:198`, `app/chamados/services/validacao.py:271`)
- Outra seleção particular é `_chamados_vivos_do_dia` de cobertura: todos os módulos VIVOS, preferindo célula e depois catálogo, usada só pela própria cobertura. (`app/ponto/services/cobertura.py:218-231`, `:309`)

## 31/10 21:32 · sessao 2fe618724668 · - · - · origem=Ronald (tmux)

Censo do circuito de estados, somente código local.

- **Código → estado:** `_MAPA_VEREDITO` mapeia `FURO_SEM_COBRANCA` e `FURO_COM_COBRANCA_MORTA` para `furo`; `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO` para `cobranca_indevida`; `COBRANCA_EM_DIA_COBERTO` para `coberta`. A ordem do mapa decide a precedência quando há vários códigos; lista vazia vira `concorde`. (`app/escala/models.py:1579-1600`)
- **Escritor de veredito/protestos/ata:** `CelulaDia.lavrar_veredito` delega à porta `ponto.portas.celula.lavrar_veredito`. A porta atribui `ata`, `protestos`, `veredito`, `veredito_via`, `veredito_em` e salva; o cartório lhe entrega todos os `cods` e a ata da passada. (`app/escala/models.py:1787-1796`; `app/ponto/portas/celula.py:709-775`; `app/ponto/services/cartorio.py:1565-1586`)
- **Outros chamadores produtivos da mesma porta:** completar status de célula já lavrada; ramo nunca-bateu; julgamento comum do cartório; decisão humana pelo serviço `veredito_celula`. (`app/ponto/services/cartorio.py:734`; `app/ponto/services/cartorio.py:1044`; `app/ponto/services/cartorio.py:1565`; `app/ponto/services/veredito_celula.py:330`)
- **Admin prevalece:** lavra do cartório preserva o veredito humano, mas pode atualizar ata e status; lavra humana conserva os protestos existentes. (`app/ponto/portas/celula.py:737-751`)
- **Lâmpadas não são o efeito direto desses códigos:** `lampadas_do_dia` produz `acesa/luz/tipo_real` a partir das células casadas; `ata_do_dia` inclui a lista na ata; dispensa por ausência/justificativa aprovada pode ligar lâmpadas sem luz e marcá-las dispensadas. Os códigos seguem em `protestos`, enquanto lâmpadas seguem na ata entregue à mesma lavra. (`app/ponto/services/cartorio.py:429-443`; `app/ponto/services/cartorio.py:541-568`; `app/ponto/services/cartorio.py:611-624`; `app/ponto/services/cartorio.py:1565`)
- **Escritor de `status_local` nas retratações:** `reconciliar` consulta a decisão/gates e chama `_gravar_estado`; este atribui `status_local`, ajusta resolução/via/urgência, registra evento e salva. `RECONCILIAR` leva `aberto/em_analise/registrado` a `resolvido`. (`app/chamados/models.py:195-279`; `app/chamados/models.py:605-619`; `app/chamados/catalogo/motor.py:109-117`)
- **Nascimento de chamado:** a porta `abrir` constrói e cria `ChamadoColaborador` com `status_local=status`; é outro momento de escrita do campo, além das transições. (`app/chamados/models.py:1029`; `app/chamados/models.py:1046`)
- **Retratação completa:** `retratar → _retratar` testa retenção, chama `ch.reconciliar()`, pode rejeitar justificativa pendente vinculada, resolve perguntas e anexa ocorrência. O cartório, nos códigos CSF/SEM_TRABALHO, chama diretamente `x.reconciliar()`, não a unidade `retratar`. (`app/chamados/reconciliador.py:505-548`; `app/ponto/services/cartorio.py:1313-1325`)
- **Leitores síncronos de `status_local`:** signals rebaixam urgência e resolvem perguntas quando o chamado está em `ENCERRADOS`; outro signal compara status anterior/atual e chama `cartorio.julgar_celula` quando houve nascimento/mudança, exceto reentrada do próprio julgamento. (`app/chamados/signals.py:68-94`; `app/chamados/signals.py:155-180`)
- **Leitores de fila:** `qs_vivos` filtra `status_local__in=VIVOS`; `qs_na_fila` acrescenta `COBRANCA` e exclui silêncio futuro. A fábrica também lê `status_local in ENCERRADOS` para recusar chamado já encerrado. (`app/chamados/services/painel.py:153-155`; `app/chamados/services/painel.py:186-188`; `app/chamados/services/disputa_emissao.py:690-692`)
- **Leitor C6/pergunta viva:** lê a lâmpada do marco perguntado antes do veredito geral; depois considera se `cel.veredito` pertence a `VEREDITOS_ACUSAM`, com ressalva dos marcos pendentes. (`app/chamados/juizes.py:1073-1095`)
- **Leitor de premissa/furo:** `_celula_furada` consulta `dia_sem_trabalho`, exige chave `lampadas` e lê lâmpadas `acesa is False` e órfãs; `dia_sem_trabalho` lê `cel.trabalha is False` ou `ata.tipo_dia` preenchido diferente de trabalho. (`app/chamados/juizes.py:398-408`; `app/chamados/juizes.py:442-467`)
- **Leitor por marco:** `lastro.marcos_acesos` usa os marcos cobrados pelo chamado e cada lâmpada da ata; devolve `True`, `False` ou `None`. É usado pelo cartório e pela rede horária, conforme contrato do próprio método. (`app/ponto/services/lastro.py:153-181`)
- **Leitores emissores:** `fabrica_da_celula` cala quando célula tem veredito fora de `VEREDITOS_ACUSAM`; `disputa_emissao` usa a mesma lista para saber se pergunta encerrada retira chamado da fábrica. (`app/chamados/services/fabrica_da_celula.py:78-89`; `app/chamados/services/disputa_emissao.py:695-706`)
- **Leitor folha:** `motivos_retencao_celula` lê `VEREDITOS_ACUSAM` e protestos; retém `espelho_<veredito>` salvo protestos todos não cobráveis ou dispensa de pausa. `coberta` fica fora de `VEREDITOS_ACUSAM`, enquanto `furo` e `cobranca_indevida` estão dentro. (`app/folha/export.py:1215-1238`; `app/escala/models.py:1735-1736`)
- **Leitor/retratador assíncrono:** `_absolvido_pela_celula` aceita célula com veredito preenchido fora de `VEREDITOS_ACUSAM`, sem marcos pendentes; `retratar_chamados_absolvidos_por_celula` percorre chamados VIVOS de módulos de turno e retrata pela unidade canônica. `consumir_eventos_celula` chama esse leitor e encerra órfãs pela lâmpada. (`app/chamados/reconciliador.py:949-986`; `app/chamados/reconciliador.py:1300-1308`)
- **Evento da lavra:** salvar célula e inserir `EventoCelula` ocorrem no mesmo `atomic` quando campos relevantes mudam. (`app/ponto/portas/celula.py:626-642`)
- **Leitor que realimenta o juiz:** `grade_da_celula` leva o veredito, contagens lavradas, lâmpadas convertidas em células e insumo de `dia_sem_trabalho` para o dicionário `dia`. (`app/escala/services/leitor_celula.py:63-101`; `app/escala/services/leitor_celula.py:345-370`)

Não fiz afirmação de que esta lista seja o censo exaustivo de **todo** leitor de `status_local`/ata/veredito da aplicação; ela é o circuito produtivo diretamente relacionado aos efeitos em questão. A busca encontrou leitores adicionais em relatórios, APIs, calendários, vigias e comandos, cujo comportamento completo não foi inspecionado. [não verificado]

## 31/10 21:31 · sessao f063b36dd0dc · - · - · origem=Ronald (tmux)

Censo documental, somente leitura.

- **Regra geral de leitura da lâmpada:** literal “O CONSUMIDOR LE A LAMPADA, nunca re-julga: chamado, pergunta, tela e folha leem a celula/ata” (`app/docs/LEIS.md:95`); equivalente “CONSUMIDOR (chamado, pergunta, tela, folha) le a lampada, nunca re-julga” (`CLAUDE.md:245`).

- **Corte que nomeia os três alimentadores:** o aval literal inclui “todo leitor de chamados_vivos (disputa_emissao.py:670, supra_juiz.py:163-168, core/juizes.py _C1, cartorio.py:780, commands/supra_juiz.py:182, emitir_furo_retroativo.py:79)” e determina “FASE 2 direto pela CLAUDE.md 4a: cada escritor passa a fechar a pergunta APAGANDO a lampada pela porta da celula, ou se aposenta (L-111); lampada que reacende reabre o fio; os leitores leem a lampada.” As linhas internas citadas pelo documento são históricas, mas o texto literal está em `app/docs/CORTES.md:255`.

- **Separação chamado vivo × cobrança viva:** o registro diz literalmente “`VIVOS` responde so a primeira metade” e explica que ler `VIVOS` para saber se alguém cobra confundia chamado vivo com fio morto (`app/core/juizes.py:100-106`). A pergunta “o chamado esta vivo?” é `_C1` (`app/core/juizes.py:107`), registrada em `chamados/catalogo/motor.py::VIVOS` (`app/core/juizes.py:256`); “alguma pergunta desta colecao esta viva?” é `_C15` (`app/core/juizes.py:123-125`), registrada em `chamados/juizes.py::fio_vivo` (`app/core/juizes.py:270`).

- **Corte F1 específico sobre universo no julgamento:** “o corte das 22:1x vale tambem para 'cobranca_indevida' (COBRANCA_EM_DIA_SEM_TRABALHO e COBRANCA_SEM_FURO): com todos os marcos acesos NESTE julgamento, o chamado nao conta como cobranca viva e o veredito nao le o proprio chamado vivo (4a).” (`app/docs/CORTES.md:405`)

- **Extensão classe 4:** “o veredito nunca le o proprio chamado vivo, cura na origem sem predicado novo nem excecao” e “cobranca_indevida deixa de ler o chamado vivo do proprio dia” (`app/docs/CORTES.md:409`). A versão seguinte amplia: “o veredito nunca le o proprio chamado vivo vale para TODA lampada, inclusive 'cobrado' em dia sem trabalho” (`app/docs/CORTES.md:412`).

- **Universo de `_protesta` expressamente documentado:** “NAO MIGRADO (declarado, BACKLOG CARTORIO-PROTESTA-1A-CLAUSULA)” (`app/core/juizes.py:145`); lê “so a 1a clausula (`trabalha is not False`, alem dos marcos acesos; corte O261-E-F1-CLASSE-4 31/10 00:0x/00:4x)” (`app/core/juizes.py:146`); “com trabalha=False zera chamados_vivos antes de classificar_dia” (`app/core/juizes.py:147`); “a 2a clausula (ata.tipo_dia) seria predicado novo -- pergunta com numeros, nao cura calada” (`app/core/juizes.py:148`). O mesmo consta do anexo/local `app/docs/JUIZES.md:60-62` e do corte `app/docs/CORTES.md:415`.

- **O ramo COBRANCA_EM_DIA_SEM_TRABALHO tem juiz diverso de `_protesta`:** o corte diz literalmente “`trabalha is False` ou `ata.tipo_dia` PREENCHIDO e != trabalho, L-061; `trabalha=None` sem ata de outro tipo COBRA” (`app/docs/CORTES.md:415`). O registro especifica ata **deste julgamento, antes da lavra**, e distingue a pergunta de `eh_dia_trabalho` do vínculo, que responde pelo previsto (`app/core/juizes.py:136-140`). O corte preserva `SEM_JURISDICAO` como guarda anterior e declara o transporte de `trabalha` + `ata` pelo produtor do dict (`app/docs/CORTES.md:415`; `app/core/juizes.py:143-144`).

- **Histórico que alerta contra estreitar `vivos` indiscriminadamente:** o texto explica “no `cartorio.py:907` o MESMO `vivos` decide EMISSAO, logo a cura separa predicados (*ha chamado vivo* x *ha cobranca viva*) e nao estreita `vivos`” (`app/docs/CORTES.md:64`; repetido em `:67`). Trata-se de registro histórico, não evidência da numeração ou da implementação atual (`app/docs/CORTES.md:64`).

- **Declarações de autoridade complementares:** “VIVOS/COBRANCA/TERMINAIS/NAO_FECHADOS, nunca tupla literal, nem em aspas duplas” (`app/docs/JUIZES.md:51-52`) e dia por `catalogo/modulos.py::data_do_chamado`, “nunca .get('data_turno') cru” (`app/docs/JUIZES.md:53-54`).

Não localizei nesses três arquivos uma especificação individual completa de filtros de módulo/status/dia para **cada** comando; a ausência absoluta de outro texto normativo no repositório é **[não verificado]**. O que há documentalmente é a ordem abrangente que nomeia todos os alimentadores (`app/docs/CORTES.md:255`) e a especificação recente e explícita de `_protesta`/`dia_sem_trabalho` (`app/core/juizes.py:136-148`; `app/docs/CORTES.md:415`).

## 31/10 21:18 · sessao f0d8537b15e0 · GRADE-FOLGA-JUIZ · verificador · origem=Code

**Pré-veredito: não liberaria o pouso ainda.** A catraca deixa passar uma cópia literal do predicado que deveria proteger; há também uma diferença de alimentação em `fase_conflitante` que impede afirmar equivalência geral entre os sítios. O ensaio exigido antes do pouso não está demonstrado nos anexos: **[nao verificado]**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:75`, `app/ponto/precedencia.py:237`, `app/escala/services/fase_conflitante.py:82`, `app/docs/BACKLOG.md:796`)

As referências abaixo são à cópia indicada pelo usuário como `1093275a`; a identidade Git da cópia é **[nao verificado]**. RED histórico, suíte Django e resultados de produção permanecem **[nao verificado]**.

**1. Quem escreve e quem lê**

O novo juiz está autorizado e registrado: a pergunta é de `folga_lancada_do_dia`; o carregador preserva todas as candidatas, ordenadas por PK, e o seletor consulta um único predicado. (`app/docs/CORTES.md:416`, `app/core/juizes.py:192`, `app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:212`)

| Função/campo | Quem produz ou escreve | Quem lê |
|---|---|---|
| `FolgaDia.escala_colaborador`, `data`, `calendario_origem` | `gerar_foto_folgas` cria a foto do calendário; preserva manuais e apaga a foto anterior de calendário. | Carregador novo, alimentação mensal, gerador, cadastro e auditor C5. (`app/escala/folgas.py:165`, `app/escala/folgas.py:173`, `app/ponto/precedencia.py:206`, `app/escala/alimentacao.py:66`, `app/escala/models.py:1257`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`) |
| Avulsa: vínculo, data e `tipo` | `lancar_folga` e `_foto_do_dia`; este também altera `tipo` e remove a manual. | Juiz lê vínculo/origem; `tipo` é rótulo informativo, não condição de validade. (`app/escala/folgas.py:254`, `app/escala/folgas.py:259`, `app/escala/folgas.py:274`, `app/escala/folgas.py:284`, `app/escala/folgas.py:288`, `app/escala/models.py:1564`, `app/ponto/precedencia.py:237`) |
| Reparentamento da avulsa | `reparentar_folga` altera `escala_colaborador`; calendário não é reparentado. | Os mesmos leitores da foto. (`app/escala/folgas.py:405`, `app/escala/folgas.py:434`) |
| Escrita implícita das FKs | Exclusão do vínculo usa `CASCADE`; exclusão do calendário usa `SET_NULL`. | O predicado interpreta origem nula como avulsa: essa escrita implícita também afeta a classificação. (`app/escala/models.py:1553`, `app/escala/models.py:1557`, `app/ponto/precedencia.py:237`) |
| `{data: [FolgaDia...]}` | `folgas_lancadas_do_periodo`; somente memória, sem persistência. | `fatos_do_periodo`, veto, declaração, comando de rejulgamento, medidor de fase e overlay da grade. (`app/ponto/precedencia.py:205`, `app/ponto/precedencia.py:281`, `app/ponto/precedencia.py:501`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64`, `app/escala/services/fase_conflitante.py:80`, `app/escala/utils.py:1119`) |
| Folga selecionada / validade | `folga_lancada_do_dia` retorna a primeira candidata aceita; `folga_lancada_vale_no_dia` decide origem e vínculo. | Os consumidores acima; só o seletor chama diretamente o predicado. (`app/ponto/precedencia.py:216`, `app/ponto/precedencia.py:222`) |
| `faixa`, `tipo_do_dia`, `era_dia_previsto`, `suprimidos`, `fatos.folga_lancada` | `fatos_do_periodo`; `fatos_do_dia` é sua entrada para um dia. | Calendário, fatos do status e consumidores do juiz, incluindo a coleta de chamados espúrios. (`app/ponto/precedencia.py:256`, `app/ponto/precedencia.py:397`, `app/colaboradores/services/calendario.py:807`, `app/ponto/status_dia.py:139`, `app/chamados/services/feriado_reavaliar.py:33`) |
| Grade: `tipo_dia`, `celulas`, `orfas` | Builders montam; `_aplicar_folgas_foto` aplica o resultado do juiz; ausência e isenção ainda fazem seus overlays. | Cartório e testemunhas da grade. (`app/escala/utils.py:1124`, `app/escala/utils.py:1794`, `app/escala/utils.py:1797`, `app/ponto/services/cartorio.py:946`, `app/ponto/services/cartorio.py:582`) |
| Calendário: `status`, `eh_trabalho` | `_classificar` e `trabalho_do_dia`, sobre o mesmo dict, publicados por `contexto_calendario`. | Template, contagens, `status_do_dia`, `contar_por_status` e oferta de “Resolver dia”. (`app/colaboradores/services/calendario.py:818`, `app/colaboradores/services/calendario.py:839`, `app/colaboradores/services/calendario.py:903`, `app/colaboradores/services/calendario.py:202`, `app/colaboradores/services/calendario.py:226`, `app/colaboradores/services/calendario.py:237`, `app/templates/colaboradores/partials/_calendario_grade.html:69`) |
| `REGRA_VERSOES` | Declaração em código, agora com versão 3. | `versao_para`; a versão entra na impressão dos insumos. (`app/ponto/services/cartorio.py:111`, `app/ponto/services/cartorio.py:193`, `app/ponto/services/cartorio.py:367`) |

Os demais campos do dict do calendário — `ausencia_desconta`, `futuro`, `isento`, `regua_suspeita`, `tem_batida`, `turno_aberto`, `desvio`, `celula`, `celula_trabalha`, `veredito`, `veredito_via`, `cobertura`, `cobertura_desconta` e `retido_folha` — são montados em `contexto_calendario` e consumidos por `_classificar`; o diff não os transforma em campos persistidos. (`app/colaboradores/services/calendario.py:818`, `app/colaboradores/services/calendario.py:122`, `app/colaboradores/services/calendario.py:169`)

Fora do diff, há consumidores relevantes das funções alteradas:

- `o_dia_ja_e_folga` participa do aceite idempotente e da repetição após corrida em validação. (`app/chamados/services/validacao.py:261`, `app/chamados/services/validacao.py:316`)
- O veto afeta ausência, férias e as portas de folga. (`app/ponto/services/ausencia.py:380`, `app/ferias/models.py:428`, `app/ferias/views.py:253`, `app/escala/folgas.py:258`, `app/escala/folgas.py:283`)
- O medidor entrega `total`, `vinculos` e detalhes; o comando grava `MetricaSnapshot`, e a API lê o snapshot. (`app/escala/services/fase_conflitante.py:100`, `app/escala/management/commands/lavrar_fase_conflitante.py:35`, `app/api/views_mensageria.py:643`)

**Sim, permanecem leitores com resposta própria.** O C5 aceita toda `FolgaDia` cujo vínculo cubra o dia **ou** para a qual encontre outro vínculo cobrindo; não distingue origem. Calendário velho inválido continua podendo virar “contradição” contra célula de trabalho. Está expressamente declarado como fila `GRADE-FOLGA-C5`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1530`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1542`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:15`)

Também permanece a foto própria do gerador, discutida no item 6. As consultas de Plano de Folgas, diagnóstico e limbo são por vínculo/mês, portanto sua mera existência não prova segundo juiz da pergunta da pessoa. (`app/escala/models.py:1257`, `app/escala/views_plano_folgas.py:60`, `app/ponto/services/diagnostico_escala.py:102`, `app/relatorios/furos.py:112`)

**2. Antes, depois e contraexemplos**

Considere velho encerrado em 09/10, novo desde 10/10, célula de trabalho de 15/10 pertencente ao novo; sem outra ausência ou feriado prevalecente. Esse é o desenho dos testes nominais. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:46`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:49`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:51`)

| Sítio | Calendário do velho em 15/10: antes → depois | Avulsa no velho em 15/10: antes → depois |
|---|---|---|
| `_folga_avulsa` / veto | Encontrava e vetava → não veta por essa folga. | Encontrava e vetava → continua vetando folga/atestado. Férias atravessam folga. (`app/ponto/precedencia.py:161`, `app/ponto/precedencia.py:491`, `app/ponto/precedencia.py:505`) |
| `_folga_do_dia` / declaração | Não encontrava no vínculo novo → continua não encontrando folga válida. | Não encontrava → encontra; `o_dia_ja_e_folga=True` e recusa “já é folga”. (`app/ponto/services/declarar_dia.py:69`, `app/ponto/services/declarar_dia.py:95`, `app/ponto/services/declarar_dia.py:112`) |
| `rejulgar_ausencia_em_folga` | Entrava no cruzamento empresa/data → é descartado pelo juiz. | Entrava → continua entrando. Só vira achado se houver ausência aprovada não-falta cobrindo o dia. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:24`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64`) |
| `fase_conflitante.medir` | Foto presa ao velho não entrava na foto do novo → continua não entrando. | Não entrava → passa a entrar, dentro da vigência carregada do novo. (`app/escala/services/fase_conflitante.py:75`, `app/escala/services/fase_conflitante.py:80`) |
| Precedência | Já descartava calendário alheio, mas só examinava a primeira linha → percorre todas até achar uma válida. | Continua reconhecendo a avulsa da pessoa. (`app/ponto/precedencia.py:201`, `app/ponto/precedencia.py:216`, `app/ponto/precedencia.py:357`) |

Um contraexemplo importante corrigido: calendário inválido com PK menor seguido de calendário válido no mesmo dia. Antes, a primeira candidata escondia a segunda; agora ambas chegam ao seletor. (`app/ponto/precedencia.py:201`, `app/ponto/precedencia.py:206`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`)

**Sem vínculo**, o juiz rejeita calendário e aceita avulsa. `o_dia_ja_e_folga` passa a reconhecer essa avulsa, mas `_recusa_do_dia` continua recusando **nova declaração** por “sem vínculo” antes de consultar a folga. Existência do fato e autorização para declarar são perguntas diferentes. (`app/ponto/precedencia.py:235`, `app/ponto/services/declarar_dia.py:95`, `app/ponto/services/declarar_dia.py:109`)

O comando de rejulgamento também pode selecionar essa avulsa sem vínculo, pois usa `escala_vigente(...)=None`; a realização posterior depende de haver célula julgável. O medidor de fase, por sua vez, percorre somente vínculos ativos. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78`, `app/ponto/services/cartorio.py:1722`, `app/escala/services/fase_conflitante.py:72`)

**Na fronteira**, início e fim são inclusivos. Sem geradora, vence o vínculo ativo e de início mais recente entre os que cobrem a data. Com geradora declarada, ela manda enquanto vigente; se estiver fora da vigência, a juíza retorna `None`, sem escolher substituto. Portanto “dia da troca” exige informar a célula geradora, além das datas dos vínculos. (`app/escala/alimentacao.py:182`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:254`)

Há uma **ressalva no sítio nominal `fase_conflitante`**: ele entrega ao juiz o `ec` iterado, não o vínculo efetivo do dia. Contraexemplo: velho vigente até 20/10, novo ativo desde 10/10, célula de 15/10 gerada pelo velho, calendário preso ao novo. O medidor aceita essa candidata; a precedência, alimentada com o velho, rejeita. Isso é divergência de alimentação, não cópia do predicado. Para declarar os sítios equivalentes na pergunta do colaborador, precisa fechar esse caso ou explicitar que o contador responde outra pergunta. (`app/escala/services/fase_conflitante.py:80`, `app/escala/services/fase_conflitante.py:82`, `app/ponto/precedencia.py:350`, `app/ponto/precedencia.py:357`, `app/escala/alimentacao.py:247`)

**3. Catraca AST e mordida dos mutantes**

Executei em memória as funções da catraca: o censo atual encontrou **21 chaves, iguais às 21 da allowlist**, e os quatro testes sintéticos passaram. Isso verifica o scanner e suas amostras; execução da suíte Django permanece **[nao verificado]**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:91`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:110`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:158`)

| Forma sondada em memória | Resultado |
|---|---|
| `from escala.models import FolgaDia as FD` | Pega o filtro. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:47`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`) |
| Import direto local dentro da função | Pega; os apelidos são coletados na árvore inteira. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:46`) |
| `import escala.models as m; m.FolgaDia...` | Escapa: a raiz é `m`, não um apelido reconhecido de `FolgaDia`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:48`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`) |
| `getattr(fd, 'calendario_origem_id', None)` ou manager via `getattr` | Escapa. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:77`) |
| Método `__init__` ou função em arquivo `__init__.py` | Pega, se estiver numa forma reconhecida; não existe exclusão especial desses nomes. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:81`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:97`) |
| `from .models import FolgaDia as FD` | Pega. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:47`) |
| `from . import models as m; m.FolgaDia...` | Escapa pelo mesmo motivo do import de módulo. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:48`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:62`) |
| `Q(...)`, `**kwargs`, teste booleano `not fd.calendario_origem_id`, código de módulo | Escapam às formas examinadas. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:33`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:75`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:81`) |

**O escape decisivo é literal:** copiar `folga_lancada_vale_no_dia` para outro arquivo produziu zero achados na sonda. O predicado verdadeiro usa `getattr`, enquanto a catraca procura comparação de atributo direto com `None`. Além disso, todo `precedencia.py` é excluído: um segundo juiz ali, com outro nome, também não seria barrado por esse censo. (`app/ponto/precedencia.py:235`, `app/ponto/precedencia.py:237`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:77`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:99`)

Os mutantes do **scanner** mordem parcialmente: removendo reconhecimento de apelido, falhou o teste de apelido; removendo comparação de origem, falhou o de origem; removendo emissão de achado de filtro, falharam dois testes. Esses são os comportamentos assertados pelas amostras existentes. O mutante “segundo predicado literal” sobrevive. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:158`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:163`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:169`)

“Só encolhe” significa, mecanicamente, **igualdade entre código atual e allowlist atual**. Não há comparação com uma allowlist anterior: adicionar uma cópia reconhecida e sua autorização no mesmo patch passa nessa igualdade. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:144`)

Os testes comportamentais têm pares úteis para veto, declaração, rejulgamento e fase; porém a execução do RED contra o main indicado e a morte de mutantes desses caminhos são **[nao verificado]**. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:76`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:90`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:122`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:162`)

**4. O portão trata igual ao juiz?**

**Depois de relavrar, os testes cobrem os dois lados corretos:** avulsa velha permite fechar; calendário velho inválido não permite. Ambos passam antes por `julgar_celula(..., forcar=True)`. Não demonstram igualdade incondicional antes da relavra. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:196`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:202`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:209`)

O portão consulta `premissa_regularizada(para='folha')`; esse coletor prioriza célula/ata. No cartório, recebe a projeção deste julgamento; fora dele, lê o gravado. (`app/chamados/models.py:699`, `app/chamados/models.py:1227`, `app/chamados/juizes.py:381`)

Contraexemplos à afirmação “sempre igual”:

- Avulsa válida, mas ata antiga de trabalho com lâmpada apagada: pode continuar recusando fechamento. (`app/chamados/juizes.py:454`, `app/chamados/juizes.py:457`, `app/chamados/models.py:1247`)
- Calendário velho inválido, mas ata antiga `tipo_dia='folga'`: pode regularizar pela ata. (`app/chamados/juizes.py:408`, `app/chamados/models.py:1232`)
- Sem célula julgável, a consulta ao juiz ainda exige `era_dia_previsto` conhecido para considerar o chamado espúrio. Avulsa existente, sozinha, não garante fechamento. (`app/chamados/services/feriado_reavaliar.py:33`)

Isso **não prova um segundo juiz de folga no portão**: o contrato distingue cobrança pela célula/ata da pergunta do previsto. O que não procede é vender o par de testes como prova de todos os estados anteriores à relavra. (`app/docs/JUIZES.md:55`, `app/chamados/models.py:1203`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:199`)

**5. As classes de reprovação**

| Classe | Verificação |
|---|---|
| Segundo juiz duplicando autoridade registrada | As cópias nominais foram substituídas por chamadas. C5 e gerador permanecem declarados; a catraca permite nascer outra cópia literal. (`app/ponto/precedencia.py:505`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:15`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:128`) |
| Segundo escritor fora da porta | Não demonstrado como introdução deste diff: `FolgaDia` continua na porta registrada, e ata/veredito/carimbo passam pelas operações existentes da célula. (`app/core/portas.py:466`, `app/ponto/services/cartorio.py:1579`, `app/ponto/services/cartorio.py:1599`, `app/ponto/portas/celula.py:709`) |
| Relógio fora da borda | Juiz, carregador e overlay novos não leem relógio. Permanecem leituras próprias em `medir` e `julgar_celula`, inclusive `localdate` mesmo recebendo `agora`; não são introduções mostradas pelo diff. (`app/ponto/precedencia.py:194`, `app/escala/utils.py:1103`, `app/escala/services/fase_conflitante.py:68`, `app/ponto/services/cartorio.py:1711`, `app/ponto/services/cartorio.py:1732`) |
| Derivação paralela de fonte única | Reduzida na grade/calendário; ainda existe no gerador e no C5. (`app/escala/utils.py:1124`, `app/colaboradores/services/calendario.py:820`, `app/escala/models.py:1281`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`) |
| Selo/RED sem mordida | Há mordida nas formas sintéticas, mas o predicado literal escapa; RED histórico e integração: **[nao verificado]**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:158`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:77`, `app/ponto/precedencia.py:237`) |
| Ordem de trava | Não há inversão nova demonstrada pelos trechos alterados; o overlay só consulta e altera memória. Persistência de célula/evento mantém o mesmo `atomic`. Concorrência real: **[nao verificado]**. (`app/escala/utils.py:1115`, `app/ponto/portas/celula.py:639`, `app/escala/folgas.py:257`) |
| Pouso antes do pré-requisito | O pré-requisito é explícito: sombra `--apply` sobre main, reversão e frota antes do opus. Cumprimento: **[nao verificado]**. (`app/docs/BACKLOG.md:796`, `app/ponto/services/cartorio.py:174`) |

Há ainda correção documental devida no mesmo commit: `JUIZES.md` manda acompanhar o registro vivo, mas sua tabela omite o novo juiz. Não procede “juiz novo sem corte”; procede “lookup desatualizado”. (`app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:72`, `app/core/juizes.py:192`, `app/docs/CORTES.md:416`)

**6. Grade, gerador persistente e competência exportada**

Em `escala/utils.py`, a mudança é no **builder da grade**: uma carga da pessoa e aplicação contra o vínculo do dia. Por turno, usa `esc_por_dia`; no legacy, os segmentos vêm da mesma juíza, e o builder individual usa o vínculo daquele segmento. A aplicação escreve somente os dicts `tipo_dia`,

[truncado em 20000 de 23379 caracteres]

_1258 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
