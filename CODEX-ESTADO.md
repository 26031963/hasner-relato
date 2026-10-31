# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 16:54** · publicado em **31/10/2026 16:55** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 16:54 · sessao bab72768ff86 · DISJUNTOR-B1-ESPERA-OCIOSA · leitura · origem=Code

**O v1 tem dois problemas centrais: E2 continuaria sem detectar um Bash único já esperando, e seu critério confundiria trabalho legítimo sem escrita com ócio.** A medição só ocorre no PreToolUse do próprio agente; o Stop apenas consulta disparos gravados. O medidor não consulta o processo em execução (`bin/disjuntor.py:413`, `bin/disjuntor.py:478`, `bin/disjuntor.py:1435`, `bin/hook_stop_fila1.py:1053`).

Os contraexemplos abaixo são cenários construídos para avaliar o contrato; sua ocorrência em produção é **[nao verificado]**. Resultado de execução dos testes: **[nao verificado]**; o script cria, altera e apaga arquivos, contrariando a restrição desta análise (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:50`).

**Respostas diretas**

- **O payload real de PreToolUse de subagente traz `agent_id`? [nao verificado].** O código o lê em `bin/disjuntor.py:1429` e também o usa na identificação da negativa em `bin/disjuntor.py:216`. O teste fabrica esse campo; isso comprova a expectativa do código, não o envio pelo harness (`bin/tests/test_disjuntor.sh:242`, `bin/tests/test_disjuntor.sh:85`).
- **Uma suite em primeiro plano, sem `sleep`, seria considerada ociosa pelo E2? Sim, quando medida**, se o agente não for isento, tiver idade suficiente, cópia citada e nenhum arquivo recente. O medidor não distingue suite, RED, ruff, git ou espera (`bin/disjuntor.py:418`, `bin/disjuntor.py:425`, `bin/disjuntor.py:450`, `bin/disjuntor.py:454`).
- **Isso não significa disparar durante o Bash no minuto 10.** Sem outra chamada do agente, não há nova medição; os 60 segundos são intervalo mínimo entre medições solicitadas, não um agendamento (`bin/disjuntor.py:478`, `bin/disjuntor.py:480`, `bin/disjuntor.py:1436`). O Stop não supre essa ausência (`bin/hook_stop_fila1.py:1053`).

**Confronto por cláusula**

| Cláusula do contrato | Suporte atual e o que falta | Contraexemplo ou consequência concreta |
|---|---|---|
| Cabeçalho: aval, instrumento e pouso posterior | Aval específico e ordem efetiva de pouso: **[nao verificado]**. O código continua declarando B1 com 15 minutos e consumo de tokens (`bin/disjuntor.py:7`, `bin/disjuntor.py:68`, `bin/disjuntor.py:452`). | O cabeçalho não estabelece uma decisão classificatória; a divergência verificável está nos critérios seguintes (`bin/disjuntor.py:450`). |
| **E1 — sleep, laços, follow e timeout** | Não há ramo E1 no PreToolUse atual. Bash comum de agente passa apenas pela medição genérica B1, sem classificação dessas esperas (`bin/disjuntor.py:1426`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1440`). Falta reconhecimento, negativa específica, instrução comando+log e emissão `B1-ESPERA`. | **FN atual:** agente jovem executando `until grep -q rc= x.log; do sleep 60; done` passa pelo B1 por idade insuficiente (`bin/disjuntor.py:450`). **FP do texto proposto:** `while false; do sleep 60; done` contém laço com sleep, mas executa zero iterações; a precisão da futura regex é **[nao verificado]**. |
| **E1 — leitura de log “EM CURSO”** | O código atual não examina caminho, conteúdo `^rc=` ou mtime do log para decidir sobre Bash (`bin/disjuntor.py:1430`, `bin/disjuntor.py:1435`). | **FP do contrato:** `tail -n 40 /tmp/wt-A/red.out` logo após RED concluído, sem `rc=` e com mtime recente. O runner RED grava saída, mas não acrescenta esse marcador (`bin/molde_autocura/rodar.sh:18`, `bin/molde_autocura/rodar.sh:24`). A própria suite também termina diretamente no processo de teste (`bin/suite.sh:96`). |
| **E1 — só subagente; principal fora** | Há leitura de `agent_id`; Bash comum sem ele retorna antes de I/O. A negativa e a exceção para Handback já têm molde B1 (`bin/disjuntor.py:1429`, `bin/disjuntor.py:1431`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1395`). | **FN condicionado:** se o harness omitir `agent_id`, `sleep 600` de subagente será tratado como Bash sem agente. O envio real desse campo é **[nao verificado]** (`bin/disjuntor.py:1431`). |
| **E2 — 15→10 minutos; retirar tokens** | As duas mudanças faltam: constante ainda 15 e `tokens <= 0` ainda impede disparo (`bin/disjuntor.py:68`, `bin/disjuntor.py:452`). | **FP do contrato:** agente `general-purpose`, vivo há 12 minutos, cópia `/tmp/wt-A` citada, arquivos antigos, pede `git -C /tmp/wt-A diff` ou inicia a suite. E2 nega pelo estado, sem avaliar o trabalho solicitado; o molde atual aplica a negativa a qualquer ferramenta exceto Handback (`bin/disjuntor.py:1435`). |
| **E2 — “vivo” e “arquivo novo”** | “Vivo” deriva do menor timestamp legível do transcript; “novo” significa arquivo regular com mtime recente, inclusive arquivo antigo modificado. Não mede criação nem execução produtiva (`bin/disjuntor.py:405`, `bin/disjuntor.py:441`, `bin/disjuntor.py:450`). | **FN:** agente esperando, mas um arquivo da cópia recebe atualização periódica: a medição encontra mtime recente e libera. Não exige que a atualização venha do agente ou represente avanço (`bin/disjuntor.py:405`, `bin/disjuntor.py:458`). |
| **E2 — PreToolUse nega; Stop pede TaskStop** | Existe, mas Stop só considera disparos já gravados e IDs reconhecidos como agentes async sem entrega; não mede novamente nem encerra o agente (`bin/disjuntor.py:489`, `bin/disjuntor.py:376`, `bin/hook_stop_fila1.py:1053`). O recado ocorre no caminho final do Stop, depois de possíveis saídas antecipadas (`bin/hook_stop_fila1.py:1207`, `bin/hook_stop_fila1.py:1297`, `bin/hook_stop_fila1.py:1300`). | **FN:** agente entra em Bash antes do limiar e permanece esperando sem nova ferramenta; não nasce disparo para o Stop consultar (`bin/disjuntor.py:1435`, `bin/hook_stop_fila1.py:1054`). |
| **QUEM CHAMA — mesmos hooks** | A configuração aponta para os dois comandos descritos (`.claude/settings.json:9`, `.claude/settings.json:68`). Porém são caminhos absolutos em `/home/ronald/saas-hasner`, externos à cópia auditada; identidade com os arquivos realmente executados é **[nao verificado]**. | Manter os mesmos chamadores, sem acrescentar medição ao Stop, preserva o FN do Bash único (`bin/hook_stop_fila1.py:1054`). |
| **QUEM ESPERA — síncrono, memória 1/min** | Decisão e cache existem; não há medição periódica independente. Há também espera limitada por contenção da trava do estado (`bin/disjuntor.py:251`, `bin/disjuntor.py:256`, `bin/disjuntor.py:478`). | **FN concreto sem sleep aparente:** `bash bin/suite.sh --only "ponto core"` pode ficar em `flock` por até 3600 segundos. A espera está dentro do script chamado (`bin/suite.sh:96`, `bin/trava_teste.sh:22`, `bin/trava_teste.sh:36`). |
| **FALHA — ilegível não dispara; painel `nao_sei`** | Meta/transcript/cópia ilegíveis podem produzir motivo “nao sei” sem disparo. Não existe contador B1 `nao_sei` no estado nem publicação desse motivo no painel (`bin/disjuntor.py:417`, `bin/disjuntor.py:424`, `bin/disjuntor.py:457`, `bin/disjuntor.py:481`, `bin/disjuntor.py:1615`). Parser específico E1 ainda falta (`bin/disjuntor.py:1426`). | **FN assumido pela política:** espera com meta ilegível passa (`bin/disjuntor.py:417`). **Ressalva:** disparo já gravado retorna antes de reler meta/transcript/cópia; portanto “ilegível = não dispara” não libera um B1 anteriormente disparado (`bin/disjuntor.py:476`). |
| **LIMITE — script próprio invisível ao E1; E2 pega em 10 minutos** | A segunda promessa não decorre dos chamadores atuais: B1 só mede no PreToolUse do agente (`bin/disjuntor.py:1436`). | **FN:** `bash x.sh`, cujo conteúdo seja `sleep 3600`, iniciado antes do limiar e sem chamada posterior. Mesmo retirar tokens não cria a medição ausente (`bin/disjuntor.py:478`, `bin/hook_stop_fila1.py:1054`). |
| **DIFF/apply suspende E2; E1 não** | PreToolUse suspende B1 quando `sombra.lock` está tomado ou ilegível; mera existência do arquivo não basta (`bin/disjuntor.py:307`, `bin/disjuntor.py:318`, `bin/disjuntor.py:1435`). Falta E1 independente dessa guarda. O Stop não verifica DIFF antes do recado B1 (`bin/hook_stop_fila1.py:1053`). | **FP/interrupção indevida possível:** B1 dispara; depois começa DIFF com lock tomado; Stop ainda pede TaskStop, pois consulta o disparo persistido sem essa guarda (`bin/disjuntor.py:476`, `bin/hook_stop_fila1.py:1054`). |
| **MEDIDA — disparos, negações, `nao_sei`, enumeração** | Leitor já agrega disparos pelo identificador, mas enumeração não inclui `B1-ESPERA`; negações são agregadas, sem separação por regra; falta `nao_sei` B1. Janela é móvel de 24 horas, não dia civil (`bin/disjuntor.py:1548`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:1579`, `bin/disjuntor.py:1679`). | Se surgisse uma linha `DISJUNTOR B1-ESPERA ...`, o leitor a contaria, mas a enumeração atual não a exibiria. O painel também não conta uma espera que nunca foi medida (`bin/disjuntor.py:1579`, `bin/disjuntor.py:1679`, `bin/disjuntor.py:1436`). |
| **QUEM DESLIGA — chave global do Ronald** | A chave é respeitada por PreToolUse e Stop; a proteção da própria chave ocorre antes do desligamento (`bin/disjuntor.py:144`, `bin/disjuntor.py:1486`, `bin/hook_stop_fila1.py:1039`). | **FP existente sobre git legítimo:** `git log --all --oneline --grep=DISJUNTOR_DESLIGADO` é negado por simplesmente mencionar a chave, inclusive com os disjuntores desligados (`bin/disjuntor.py:160`, `bin/disjuntor.py:1486`). |

**Os quatro trabalhos legítimos pedidos**

Considere agente `general-purpose` com cópia citada e sem mtime recente. Esses comandos não precisam produzir arquivo novo na cópia; E2 não contém um discriminador que os separe de espera (`bin/disjuntor.py:418`, `bin/disjuntor.py:454`, `bin/disjuntor.py:1435`).

| Trabalho | Comando/forma concreta e evidência |
|---|---|
| Suite `--only` em primeiro plano | `bash bin/suite.sh --dir /tmp/wt-A --only "ponto core" --espera 1`. Opções suportadas; execução por `exec` e Docker sem `-d`, com bytecode desativado (`bin/suite.sh:46`, `bin/suite.sh:96`, `bin/suite.sh:99`). |
| Docker do RED | A chamada `teste $T/orig/app test_juliani_esacred $T/red.out $IDS` executa Docker em primeiro plano e grava log fora de `orig/app` (`bin/molde_autocura/rodar.sh:20`, `bin/molde_autocura/rodar.sh:24`). |
| Ruff | `docker run --rm -v /tmp/wt-A/app:/app:ro -w /app saas-hasner-core:latest ruff check . --no-cache`, seguindo a forma existente com montagem somente leitura e cache desativado (`bin/molde_autocura/rodar.sh:13`). |
| Git | `git -C /tmp/wt-A diff`. B1 não examina o comando antes de negar. O exemplo usa `general-purpose`: `verificador` e os demais tipos de `SO_LEITURA` são isentos no medidor atual (`bin/disjuntor.py:91`, `bin/disjuntor.py:418`, `bin/disjuntor.py:1435`). |

**Os cinco MORDE**

| Item | Situação atual e lacuna |
|---|---|
| **1 — until/sleep negado; mutante sem E1 vermelho** | Falta ramo e caso E1. Os casos B1 atuais são `b1_nega`, `b1_nao_nega`, `b1_recado`; mutantes diretos cobram mtime e Handback (`bin/tests/test_disjuntor.sh:1468`, `bin/tests/test_disjuntor.sh:1577`). Um agente jovem com o laço passa hoje (`bin/disjuntor.py:450`). |
| **2 — sleep 5 e principal sleep 600 passam** | Principal sem `agent_id` já passa. Entretanto agente com B1 disparado tem até `sleep 5`, ruff ou git negados. O contrato precisa delimitar que o controle positivo de E1 usa E2 não disparado (`bin/disjuntor.py:1431`, `bin/disjuntor.py:1435`). |
| **3 — sem tokens por 10 minutos dispara; mutante vermelho** | Hoje ausência de tokens impede disparo. A fixture injeta 510 tokens no último turno; falta caso específico e mutante da condição (`bin/disjuntor.py:452`, `bin/tests/test_disjuntor.sh:237`, `bin/tests/test_disjuntor.sh:1577`). O teste ainda exige que agente com 10 minutos passe (`bin/tests/test_disjuntor.sh:258`). |
| **4 — arquivo recente não dispara** | Já existe controle com arquivo de três minutos (`bin/tests/test_disjuntor.sh:254`). Mas, após `disparado=true`, arquivo novo não rearma: o retorno antecede a medição. É contraexemplo ao item se escrito sem restringir o estado inicial (`bin/disjuntor.py:476`). |
| **5 — log com rc final não negado** | Falta caso específico E1 (`bin/tests/test_disjuntor.sh:1468`). O texto precisa resolver precedência com `tail -f` e distinguir rc da execução atual: log ativo contendo `rc=0` de execução anterior escaparia da heurística proposta; ocorrência em produção **[nao verificado]**. |

**Quem escreve e quem lê o estado**

Os caminhos são `logs/sessao/disjuntor_estado.json` e `logs/sessao/disjuntor.log` (`bin/disjuntor.py:61`). No JSON, a gravação física passa por `_com_estado`, com flock, temporário e substituição; `ler_estado` faz a leitura direta (`bin/disjuntor.py:237`, `bin/disjuntor.py:260`, `bin/disjuntor.py:271`, `bin/disjuntor.py:291`).

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `b1[agent_id]` | `b1_do_agente` cria/atualiza a entrada, via `_com_estado` (`bin/disjuntor.py:474`, `bin/disjuntor.py:481`, `bin/disjuntor.py:486`). | O próprio `b1_do_agente`; `b1_disparados` percorre as entradas (`bin/disjuntor.py:475`, `bin/disjuntor.py:490`). |
| `.checado` | `b1_do_agente`, com `agora` após medir (`bin/disjuntor.py:481`). | Mesmo método, para intervalo de 60 segundos (`bin/disjuntor.py:478`). |
| `.disparado` | `b1_do_agente`, com `bool(med['disparo'])` (`bin/disjuntor.py:481`). | Mesmo método e `b1_disparados`; este último alimenta o Stop (`bin/disjuntor.py:476`, `bin/disjuntor.py:490`, `bin/hook_stop_fila1.py:1054`). |
| `.motivo` | `b1_do_agente`, a partir do medidor (`bin/disjuntor.py:481`). | Retorno do disparo/cache e composição da negativa PreToolUse (`bin/disjuntor.py:477`, `bin/disjuntor.py:479`, `bin/disjuntor.py:1438`). |
| `tokens`, `copias`, contador `nao_sei` | Tokens/cópias são resultados transitórios do medidor; não são campos persistidos em B1. “Nao sei” pode ficar apenas no texto de `.motivo`; não há contador correspondente nessa gravação (`bin/disjuntor.py:417`, `bin/disjuntor.py:460`, `bin/disjuntor.py:481`). | O painel não lê esses motivos B1 para contá-los (`bin/disjuntor.py:1615`). |

No log, **o escritor físico é `trilha`**, que acrescenta carimbo, evento e detalhe achatado/truncado em 300 caracteres (`bin/disjuntor.py:128`).

| Registro/campo do log | Quem produz | Quem lê |
|---|---|---|
| Carimbo e evento | `trilha` (`bin/disjuntor.py:133`). | `ler`, que separa tokens e interpreta o instante (`bin/disjuntor.py:1559`, `bin/disjuntor.py:1563`). |
| `DISJUNTOR B1 <id>: medida \| ação` | `b1_do_agente` → `dispara` → `trilha` (`bin/disjuntor.py:483`, `bin/disjuntor.py:139`). | `ler` conta pelo quarto token, `B1`; não usa o ID para a contagem (`bin/disjuntor.py:1579`). |
| `DISJUNTOR B1-ESPERA ...` | Falta produtor no ramo atual de PreToolUse (`bin/disjuntor.py:1426`). | O leitor genérico comporta o identificador; falta inclusão na enumeração de `linha` (`bin/disjuntor.py:1579`, `bin/disjuntor.py:1679`). |
| `NEGOU` e motivo | `main_pretool`, antes da resposta deny (`bin/disjuntor.py:1489`). | `ler` incrementa total agregado de negativas (`bin/disjuntor.py:1570`). |
| `FALHA` | Falhas de persistência, PreToolUse e registro no RELATO; Stop também chama `DJ.trilha` (`bin/disjuntor.py:281`, `bin/disjuntor.py:232`, `bin/disjuntor.py:1506`, `bin/hook_stop_fila1.py:1061`). | `ler` conta especificamente `relato_negacao` como negativa sem RELATO; isso não constitui contador de medições B1 desconhecidas (`bin/disjuntor.py:1572`). |
| `ALVO`, hash e comando | `relato_negacao` (`bin/disjuntor.py:211`). | Não integra os eventos lidos pelo painel (`bin/disjuntor.py:1554`). |

O publicador do ESTADO chama `disjuntor.ler/linha`; **o Stop decide seu recado B1 pelo JSON, não pelo log** (`bin/gerar_estado.py:280`, `bin/hook_stop_fila1.py:1054`).

## 31/10 16:52 · sessao 6de02a40a9c1 · - · - · origem=Ronald (tmux)

- **`agent_id` real no payload de subagente: [nao verificado].** O código espera esse campo em `pretool`, lendo `payload.get('agent_id')`; sem ele, um Bash comum sai antes de qualquer I/O (`bin/disjuntor.py:1429–1432`). A evidência disponível é sintética: `_pl` fabrica JSON com `agent_id` (`bin/tests/test_disjuntor.sh:242`), e `_pre` entrega o JSON ao hook (`bin/tests/test_disjuntor.sh:85`). Não há prova local de uma captura real do harness [nao verificado]. O campo também identifica o agente na trilha do RELATO (`bin/disjuntor.py:216`).

- **E2 como escrito confundiria suite produtiva com ociosidade quando medido.** `medir_b1` conhece tipo do agente, texto do transcript, timestamps, tokens e mtime de arquivos; não examina comando em execução nem distingue trabalho CPU/DB de espera (`bin/disjuntor.py:413–461`). O tipo `general-purpose` não é isento; isentos são `verificador`, `Explore`, `Plan`, `claude-code-guide`, `statusline-setup` (`bin/disjuntor.py:90–91`, `bin/disjuntor.py:418–419`). Remover o requisito de tokens e usar janela de 10 min classificaria como disparada uma suite em primeiro plano de agente com cópia citada e sem escrita nessa cópia em 10 min; a duração real de qualquer suite do dado é [nao verificado].

- **Mas não dispararia automaticamente no minuto 10 de UM Bash já em execução.** `b1_do_agente` só é chamado pelo PreToolUse daquele agente (`bin/disjuntor.py:1435–1436`). Os 60 s são cache de medição sob demanda, sem agendador (`bin/disjuntor.py:478–481`). O Stop não mede: lê `b1_disparados` e cruza com agentes de fundo ainda sem entrega (`bin/hook_stop_fila1.py:1053–1054`; `bin/disjuntor.py:489–490`). Portanto, Bash único iniciado antes do limiar, sem chamadas posteriores, pode continuar além de 10 min sem novo disparo; remover tokens não fecha esse buraco (`bin/disjuntor.py:1435–1436`, `bin/hook_stop_fila1.py:1054`).

- **Contra-exemplo concreto, sem executar:** estado de transcript com `agentType=general-purpose`, primeiro timestamp há 20 min, cópia absoluta `/tmp/wt-a` citada, arquivos sem mtime recente, último turno assistente há 11 min, e ferramenta Bash `bash bin/suite.sh --dir /tmp/wt-a --only "ponto core"` ainda rodando. Hoje, em medição aos 20 min, falta token nos últimos 15 min pode livrar; com E2 e medição, a ausência de arquivos dispara, mesmo se a suite estiver calculando (`bin/disjuntor.py:425–453`, `bin/disjuntor.py:454–461`). A sintaxe `--dir`/`--only` é suportada (`bin/suite.sh:46–47`); a execução final é foreground `docker run ... python manage.py test` com `PYTHONDONTWRITEBYTECODE=1` (`bin/suite.sh:96–103`). Se nenhuma ferramenta nova for chamada, não há medição aos 20 min (`bin/disjuntor.py:1435–1436`).

- **Outro FN concreto de espera sem `sleep`:** a mesma chamada `bash bin/suite.sh --only "ponto core"` pode aguardar a trava. Suite sempre chama `trava_teste.sh` inclusive com `--only` (`bin/suite.sh:91–96`); a trava faz `flock -w "$ESPERA"` com default 3600 s (`bin/trava_teste.sh:22`, `bin/trava_teste.sh:35–38`). Isso escapa de regex que olha apenas o Bash externo e dos padrões E1 descritos; E2 não o captura no meio de um único Bash pelo motivo acima (`bin/disjuntor.py:1435–1436`, `bin/hook_stop_fila1.py:1054`).

- **“Vivo” hoje não consulta processo:** deriva da menor data de qualquer linha JSON com timestamp; JSON quebrado e timestamp inválido são pulados (`bin/disjuntor.py:430–451`). **“Arquivo novo” inclui arquivo antigo modificado:** basta qualquer arquivo regular com `mtime >= janela`, fora dos diretórios ignorados (`bin/disjuntor.py:402–406`). **Disparo é persistente:** `disparado=true` retorna sem reavaliar, mesmo que apareça arquivo posterior (`bin/disjuntor.py:476–477`).

## 31/10 16:52 · sessao 7ba21874f2bc · - · - · origem=Ronald (tmux)

Auditoria estática; não executei testes, pois o script cria árvore temporária, escreve fixtures e apaga arquivos no EXIT (`bin/tests/test_disjuntor.sh:39-55`). Resultado real da suíte: [nao verificado].

- Os testes atuais certificam **15 minutos e tokens positivos**: fixture sempre injeta 510 tokens (`bin/tests/test_disjuntor.sh:227-239`), a negativa exige texto “15 min” (`bin/tests/test_disjuntor.sh:245-250`), e agente com exatamente 10 minutos deve passar (`bin/tests/test_disjuntor.sh:258-259`). Reduzir para 10 minutos exige atualizar essa expectativa.
- Já existem controles para arquivo recente, verificador, DIFF, sessão principal e ilegibilidade (`bin/tests/test_disjuntor.sh:252-270`), além de pedido de TaskStop e retirada do pedido após killed (`bin/tests/test_disjuntor.sh:272-280`).
- Os cinco MORDE novos não estão implementados como casos E1/E2: os casos B1 continuam `b1_nega`, `b1_nao_nega`, `b1_recado` (`bin/tests/test_disjuntor.sh:1468`). Os mutantes diretos de B1 removem mtime ou exceção de Handback (`bin/tests/test_disjuntor.sh:1577-1578`), e os outros testam cópia/meta ilegíveis (`bin/tests/test_disjuntor.sh:1731-1735`).

Contraexemplos concretos, condicionados ao estado sintético E2: `general-purpose`, vivo há 12 minutos, `/tmp/wt-A` citada, nenhum arquivo recente. Ocorrência desse estado em produção: [nao verificado].

1. `bash bin/suite.sh --dir /tmp/wt-A --only "ponto core" --espera 1`: opções legítimas documentadas (`bin/suite.sh:24-27`), execução foreground por `exec`/docker sem `-d`, com `PYTHONDONTWRITEBYTECODE=1` (`bin/suite.sh:96-103`). E2 não distingue esse trabalho de espera, pois seu molde mede apenas idade/tokens/mtime (`bin/disjuntor.py:428-461`).
2. RED foreground: o molde executa docker contra a árvore anterior e grava `red.out` fora de `orig/app` (`bin/molde_autocura/rodar.sh:18-24`). É trabalho útil sem necessidade de escrever na árvore examinada.
3. Ruff: `docker run ... -v /tmp/wt-A/app:/app:ro ... sh -c "ruff check . --output-format concise --no-cache"` segue execução existente, read-only e sem cache (`bin/molde_autocura/rodar.sh:13`).
4. `git -C /tmp/wt-A diff`: a negativa de B1 não examina o comando, logo também alcança leitura legítima de git (`bin/disjuntor.py:1428-1439`). Para verificador há isenção explícita, portanto usar `general-purpose` neste contraexemplo (`bin/disjuntor.py:418-419`).

E1 também confunde “sem `^rc=`” com “em curso”: `tail -n 40 /tmp/wt-A/red.out`, logo após RED concluído, satisfaz a heurística proposta, embora o runner RED não grave `rc=` (`bin/molde_autocura/rodar.sh:18-24`). `suite.sh` termina diretamente no comando de teste, também sem acrescentar marcador (`bin/suite.sh:96-103`). O protocolo documental verifica veredito, `Ran` e rc **do processo**, sem impor `rc=` no arquivo (`CLAUDE.md:191-195`). Um log ainda ativo contendo `rc=0` de execução anterior constitui o falso negativo inverso; estado hipotético, [nao verificado].

O `agent_id` dos testes é **fabricado pelo teste**, não prova de payload real do harness: `_pl` imprime o JSON com esse campo (`bin/tests/test_disjuntor.sh:242`); o hook de teste apenas encaminha esse JSON ao stdin (`bin/tests/test_disjuntor.sh:85-86`).

## 31/10 16:52 · sessao 2b38ef829f7c · - · - · origem=Ronald (tmux)

Mapa de estado e integração, apenas por inspeção de código:

| Campo/arquivo | Quem escreve | Quem lê | Semântica e lacuna |
|---|---|---|---|
| `disjuntor_estado.json` | `_com_estado`: carrega JSON, executa transição, grava temporário e `os.replace`, sob `flock` (`bin/disjuntor.py:237`, `:247`, `:260`, `:268`, `:271`). | `_com_estado` e `ler_estado` (`bin/disjuntor.py:260`, `:291`). | JSON ilegível levanta; ausência vira `{}`; falha de gravação preserva a decisão já calculada e gera `FALHA` (`bin/disjuntor.py:262`, `:265`, `:281`, `:296`). |
| `b1` | `b1_do_agente`, via `est.setdefault('b1', {})` (`bin/disjuntor.py:473`). | Próprio `b1_do_agente` e `b1_disparados` (`bin/disjuntor.py:475`, `:489`). | Chaveado somente por `agent_id`, não por sessão (`bin/disjuntor.py:475`, `:481`). |
| `b1[agent_id].checado` | `b1_do_agente` grava `agora` ao medir (`bin/disjuntor.py:481`). | Mesmo método compara idade com `B1_REAVALIA_S=60` (`bin/disjuntor.py:69`, `:478`). | É intervalo mínimo entre chamadas; não é timer/autonomia de reavaliação (`bin/disjuntor.py:478`, `:1436`). |
| `b1[agent_id].disparado` | `bool(med['disparo'])` (`bin/disjuntor.py:481`). | `b1_do_agente` e `b1_disparados` (`bin/disjuntor.py:476`, `:490`). | Disparo fica latched: retorna antes de medir novamente; arquivo novo posterior não o desarma (`bin/disjuntor.py:476`, `:480`). |
| `b1[agent_id].motivo` | Copiado de `medir_b1` (`bin/disjuntor.py:481`). | Retorno de disparo/cache e mensagem de negativa (`bin/disjuntor.py:477`, `:479`, `:1438`). | `nao sei` existe como texto do motivo; não há contador B1 `nao_sei` no estado gravado (`bin/disjuntor.py:417`, `:424`, `:457`, `:481`). |
| `disjuntor.log`: carimbo, evento, detalhe | Escritor físico `trilha`: append com carimbo local, detalhe achatado e truncado em 300 caracteres (`bin/disjuntor.py:128`). | `ler` divide tokens, interpreta carimbo/evento (`bin/disjuntor.py:1554`, `:1559`, `:1563`). | Trilha ilegível torna disparos/negações/liberações `None`; arquivo ausente conta zero (`bin/disjuntor.py:1584`, `:1590`). |
| Evento `DISJUNTOR B1 <id>: medida \| ação` | `b1_do_agente` chama `dispara`, que chama `trilha` (`bin/disjuntor.py:483`, `:139`). | `ler` agrupa pelo quarto token, portanto por `B1`; `linha` renderiza enumeração fixa (`bin/disjuntor.py:1578`, `:1677`). | Sem ramo/escritor `B1-ESPERA`; enumeração atual não o inclui (`bin/disjuntor.py:483`, `:1679`). |
| Evento `NEGOU` | `main_pretool` antes de emitir resposta deny (`bin/disjuntor.py:1489`). | `ler` incrementa total agregado `negou` (`bin/disjuntor.py:1570`). | Não agrupa negações por regra/agente (`bin/disjuntor.py:1570`). |
| Eventos `FALHA`, `ALVO` | Falhas de estado/PreToolUse/RELATO e `relato_negacao`; Stop também chama `DJ.trilha('FALHA', ...)` (`bin/disjuntor.py:212`, `:232`, `:281`, `:1506`; `bin/hook_stop_fila1.py:1061`). | Painel lê `FALHA` para contar apenas `relato_negacao`; ignora `ALVO` (`bin/disjuntor.py:1554`, `:1572`). | Falha de medição B1 representada somente por motivo `nao sei` não vira essa contagem (`bin/disjuntor.py:417`, `:481`, `:1572`). |

`tokens` e `copias` são retornados por `medir_b1` quando dispara, mas não são persistidos como campos B1; persistem apenas `checado`, `disparado`, `motivo` (`bin/disjuntor.py:460`, `:481`).

Integração:

- `.claude/settings.json` liga `Stop` a `/home/ronald/saas-hasner/bin/hook_stop_fila1.py` e `PreToolUse` a `/home/ronald/saas-hasner/bin/disjuntor.py pretool`, com matcher vazio (`.claude/settings.json:3`, `:9`, `:62`, `:68`). Esses caminhos são absolutos e externos à cópia auditada; equivalência com arquivos atualmente executados em produção é **[nao verificado]**.
- PreToolUse lê `payload.agent_id` em `bin/disjuntor.py:1429`; ausência permite saída imediata para Bash comum (`bin/disjuntor.py:1431`). A cópia prova que o código espera esse campo; não prova que o harness realmente o envia **[nao verificado]**.
- B1 mede exclusivamente quando PreToolUse de agente chama `b1_do_agente`; `SubagentHandback` é excluído e DIFF suspende esse ramo (`bin/disjuntor.py:1435`).
- Stop **não mede B1**: lê `b1_disparados`, cruza com `agentes_sem_entrega` e apenas compõe texto `TaskStop` (`bin/hook_stop_fila1.py:1053`). `agentes_sem_entrega` reconhece só agentes lançados async e notificações de fim no transcript principal (`bin/disjuntor.py:366`, `:376`).
- Stop lê apenas `transcript_path` do payload; não distingue por `agent_id` (`bin/hook_stop_fila1.py:1184`). O recado B1 só é acrescentado no caminho final que imprime `decision:block`; saídas antecipadas podem evitá-lo (`bin/hook_stop_fila1.py:1207`, `:1297`, `:1300`).
- Consequência para E2: remover tokens e mudar 15→10 **não detecta um Bash já rodando sem nova chamada**. A memória de 60 s não agenda medidas; Stop não as produz (`bin/disjuntor.py:478`, `:1436`; `bin/hook_stop_fila1.py:1054`).
- DIFF usa tentativa de `flock` compartilhado em `logs/sombra.lock`; lock ocupado ou ilegível suspende, arquivo inexistente não suspende (`bin/disjuntor.py:307`). Porém Stop não consulta essa guarda antes de pedir TaskStop de B1 já disparado: estado latched anterior + lock adquirido depois ainda gera pedido (`bin/hook_stop_fila1.py:1039`, `:1054`).
- Chave: `desligado` testa `lexists` (`bin/disjuntor.py:144`); PreToolUse chama guarda da chave antes de respeitar desligamento (`bin/disjuntor.py:1486`); Stop respeita desligamento (`bin/hook_stop_fila1.py:1039`). Comandos Bash que apenas mencionam `DISJUNTOR_DESLIG` também são negados, inclusive leitura/git legítimos (`bin/disjuntor.py:160`).
- Painel: `gerar_estado.py` chama `disjuntor.ler/linha` (`bin/gerar_estado.py:279`); `ler` conta janela móvel de 86.400 s, não dia civil (`bin/disjuntor.py:1548`); `linha` enumera B1/B2/B3/B4/B4-CODEX/ANEXO-ENXUTO (`bin/disjuntor.py:1679`).
- Hook Stop consulta `stat` desses arquivos para impedir que atualizações do próprio painel contem como prova de espera; não lê o conteúdo nessa função (`bin/hook_stop_fila1.py:585`, `:590`, `:598`).

## 31/10 16:40 · sessao 0449d96ad80d · PADRAO-DE-CIMA · leitura · origem=Code

1. **Parecer geral: há infraestrutura reaproveitável, mas o PADRAO-DE-CIMA ainda precisa de uma decisão própria.** O B3 atual agrupa por **obra**, conta reprovações finais/triagens procedentes e dispara em limiares diferentes dos desenhos por entidade; sua transição não implementa `visto_em`, janela de desenhos ou episódios nomeados. (`bin/disjuntor.py:878`, `bin/disjuntor.py:891`, `bin/disjuntor.py:896`, `bin/disjuntor.py:912`)

   Duas premissas do anexo estão desatualizadas: **o parser já extrai `CODEX_OBRA`**, e **exceções do envelope já negam no ramo B3**. (`bin/disjuntor.py:1337`, `bin/disjuntor.py:1496`)

   Os anexos fornecidos não contêm linhas reais do RELATO nem conteúdo de `disjuntor_estado.json`; todos os cenários abaixo são sintéticos, e sua ocorrência em produção é **[nao verificado]**.

2. **Linha “quem chama”: pontos de entrada existem; falta conectar a nova decisão.**

   O PreToolUse chama `disjuntor.py pretool`; o Stop consulta `julga_b3`; o wrapper consulta diretamente `--b3-parada`, depois de esperar pelo slot e obter a trava da resposta. Esses são os pontos existentes para integrar o instrumento. (`.claude/settings.json:68`, `bin/hook_stop_fila1.py:1048`, `bin/codex_pergunta.sh:156`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:165`)

   **Contraexemplo:** entidade `Batida` disparada para `OBRA-A`; chamada feita por `export CODEX_OBRA=OBRA-A; CODEX_PAPEL=verificador bin/codex_pergunta.sh …`. O parser da invocação não recupera a obra exportada no trecho anterior; implementar PADRAO somente no PreToolUse deixaria essa passagem descoberta. O wrapper recebe a obra efetiva e hoje protege apenas pelo B3. Cenário **[nao verificado]**; comportamento fundamentado em `bin/disjuntor.py:1292`, `bin/disjuntor.py:1335`, `bin/disjuntor.py:1443`, `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:165`.

3. **Linha “quem espera” + regra BLOQUEIO: implementação parcial; o alcance atual é menor.**

   `_obra_de()` já identifica obra em prompt, e `_nega()` já emite a negativa de ferramenta. Entretanto, o ramo B3 de Agent exige **verificador e leitura final**: não alcança construtor nem verificador de desenho. (`bin/disjuntor.py:504`, `bin/disjuntor.py:1395`, `bin/disjuntor.py:1454`)

   **Contraexemplo:** estado pretendido `Batida.disparado=true`, obras alcançadas `{OBRA-A}`. `Agent(prompt="Construa a obra OBRA-A")` e verificador com “leitura de desenho da obra OBRA-A” passam pelo filtro B3 atual; uma nova decisão precisa alcançá-los, supondo os demais disjuntores livres. Cenário **[nao verificado]**; filtros em `bin/disjuntor.py:114`, `bin/disjuntor.py:499`, `bin/disjuntor.py:1454`.

   **Limite oposto:** bloquear toda obra do programa alcançaria `OBRA-C`, que nunca produziu evento na entidade, contrariando o escopo declarado. Esse estado e essa associação são **[nao verificado]**; o código atual não fornece agrupamento por entidade, apenas `b3[obra]`. (`bin/disjuntor.py:891`, `bin/disjuntor.py:897`)

4. **Linha FALHA/LIMITE + regra NAO_SEI: não se pode copiar o comportamento do B3 sem restringi-lo.**

   Estado ilegível já levanta erro; `julga_b3()` transforma a exceção em motivo de desconhecimento, e `estado_b3()` devolve `NAO_SEI` para **qualquer obra consultada** quando existe esse motivo. Isso é mais amplo que “somente as obras das entidades disparadas no último estado legível”. (`bin/disjuntor.py:265`, `bin/disjuntor.py:291`, `bin/disjuntor.py:977`, `bin/disjuntor.py:986`)

   **Contraexemplo:** último estado legível alcançava apenas `{OBRA-A}`; o JSON vira `{lixo`; consulta de `OBRA-B` também é recusada pelo B3 atual. Para o novo ramo, seria bloqueio indevido. Cenário **[nao verificado]**; decisão em `bin/disjuntor.py:982`, `bin/disjuntor.py:992`, `bin/disjuntor.py:1046`.

   Falta uma fonte recuperável do **último escopo legível**: os leitores atuais retornam o estado presente ou levantam erro; não recuperam uma fotografia anterior. Guardar essa fotografia apenas dentro do JSON que pode ficar ilegível não resolve a recuperação. (`bin/disjuntor.py:259`, `bin/disjuntor.py:291`)

   RELATO ilegível também exige adaptação: `_topo_consistente()` retorna `None`, mas a decisão B3 apenas impede liberação nesse caso; o motivo explícito de `NAO_SEI` nessa transição vem da triagem ilegível. (`bin/disjuntor.py:681`, `bin/disjuntor.py:884`, `bin/disjuntor.py:925`, `bin/disjuntor.py:964`)

   A garantia de **consumir antes de liberar** já tem molde utilizável: gravação sob trava, confirmação de persistência e cancelamento da liberação quando a escrita falha. (`bin/disjuntor.py:237`, `bin/disjuntor.py:274`, `bin/disjuntor.py:958`)

5. **Linha MEDIDA: painel e ESTADO existem; as medidas novas não.**

   Há escritor de disparos, agregador de trilha e formatador do painel; o gerador de ESTADO importa esse mesmo agregador/formatador. (`bin/disjuntor.py:139`, `bin/disjuntor.py:1546`, `bin/disjuntor.py:1677`, `bin/gerar_estado.py:280`)

   Faltam desenhos por entidade, liberações do novo instrumento e eventos `entidade=None`: o retorno atual expõe medidas B3, e a apresentação enumera IDs fixos. (`bin/disjuntor.py:1615`, `bin/disjuntor.py:1620`, `bin/disjuntor.py:1679`)

   **Contraexemplo:** acrescentar somente `DISJUNTOR PADRAO-DE-CIMA` à trilha permitiria ao agregador contar o ID, mas ele continuaria ausente da enumeração apresentada; dois eventos sem entidade também não apareceriam como tal. Cenário **[nao verificado]**; agregação/apresentação em `bin/disjuntor.py:1578`, `bin/disjuntor.py:1679`.

6. **Linha “quem desliga” + regra DESLIGAMENTO: a chave existe, mas há uma falha na porta do wrapper.**

   `desligado()` consulta `DISJUNTOR_DESLIGADO` por `lexists`; PreToolUse e Stop respeitam essa chave. A guarda também impede ferramentas do Code que nomeiem a chave ou removam o hook por determinadas edições. (`bin/disjuntor.py:144`, `bin/disjuntor.py:152`, `bin/disjuntor.py:1486`, `bin/hook_stop_fila1.py:1039`)

   Porém o wrapper chama `--b3-parada` incondicionalmente para verificador; o caminho CLI → `b3_parada()` → `estado_b3()` não consulta a chave. (`bin/codex_pergunta.sh:164`, `bin/disjuntor.py:1709`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:989`)

   **Contraexemplo:** chave presente, `b3["OBRA-A"].disparado=true`, fontes legíveis. O hook fica desligado, mas o wrapper ainda recusa a chamada. Reutilizar essa porta para PADRAO exige corrigir também o desligamento nela. Cenário **[nao verificado]**; `bin/disjuntor.py:1487`, `bin/disjuntor.py:1043`, `bin/codex_pergunta.sh:165`.

7. **Regra EVENTO: falta o reconhecedor de eventos do RELATO vivo.**

   O B3 atual obtém reprovações de transcripts e triagens; seu reconhecedor de linhas do RELATO serve à **liberação**, aceitando `DESENHO` e `ERRO-BOBO`, não às três formas de evento contratadas. (`bin/disjuntor.py:736`, `bin/disjuntor.py:883`, `bin/disjuntor.py:626`)

   `_topo_consistente()` lê os bytes completos, mas devolve somente as duas primeiras linhas úteis. Reutilizar diretamente sua saída para contar desenhos perderia eventos no restante do RELATO. (`bin/disjuntor.py:662`, `bin/disjuntor.py:679`, `bin/disjuntor.py:686`, `bin/hook_stop_fila1.py:439`)

   **Contraexemplo sintético:** estas duas linhas, no corpo depois do topo, deveriam produzir dois eventos `Batida`, mas não seriam vistas por esse recorte **[nao verificado]**:

   ```text
   DESENHO OBRA-A: pergunta=x app/ponto/registro_batida.py:152
   PERGUNTA DE DESENHO OBRA-B: pergunta=y app/ponto/registro_batida.py:157
   ```

   Os dois sítios são escritas explícitas de `Batida`. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`)

   Para evitar falsos eventos, o reconhecedor novo precisa distinguir as formas válidas de `~~DESENHO…~~`, `DESENHO <obra>: …` e prosa como `resultado = DESENHO`. Não há esse reconhecedor na decisão atual. (`bin/disjuntor.py:878`, `bin/disjuntor.py:930`)

8. **Regra TEMPO: hash reaproveitável como mecanismo; identidade e janela ainda precisam ser implementadas.**

   `_sha_b3()` calcula SHA da linha com normalização própria do topo; não registra primeira observação. A janela de 24 horas existente no painel usa timestamps da **trilha**, não `visto_em` de desenhos. (`bin/disjuntor.py:571`, `bin/disjuntor.py:1548`, `bin/disjuntor.py:1563`)

   **Contraexemplo de retorno:** evento com `visto_em=T−90000` some e volta em `T`; recriar seu registro com `visto_em=T` faria um evento antigo voltar à janela. É necessário preservar a identidade mesmo fora da janela. Estado sintético **[nao verificado]**; esse histórico não pertence ao estado B3 hoje manipulado. (`bin/disjuntor.py:891`, `bin/disjuntor.py:936`)

   **Contraexemplo de normalização:** acrescentar `RESPONDIDA` à mesma linha muda o hash de `_sha_b3()`. Usá-lo diretamente pode transformar uma atualização do mesmo desenho em segundo evento. (`bin/disjuntor.py:573`) A normalização contratual que impediria essa duplicação não foi especificada **[nao verificado]**.

9. **Regra ENTIDADE: o censo fornece insumos, mas não o resolvedor completo.**

   `varrer_arvore()` expõe `arquivo`, `linha`, `entidade`, `op` e `expr`, com caminhos relativos a `app/`. Não expõe função/intervalo nem flag estruturada `DIRETA`; esses vínculos precisam de adaptação. (`app/core/censo_escritas.py:361`, `app/core/censo_escritas.py:469`, `app/core/censo_escritas.py:489`)

   Não basta aceitar `entidade != None`: o censo também atribui entidades por inferência de campo, relacionamento e nome da variável. (`app/core/censo_escritas.py:387`, `app/core/censo_escritas.py:413`, `app/core/censo_escritas.py:420`, `app/core/censo_escritas.py:424`)

   **Contraexemplo:** `app/ponto/registro_batida.py:254` contém `batida.save(update_fields=list(campos))`; a atribuição por nome não pode ser promovida automaticamente a DIRETA. Agrupá-la com um evento direto de `Batida` pode provocar disparo indevido e impedir o fallback ao programa. Cenário **[nao verificado]**; escrita e inferência em `app/ponto/registro_batida.py:254`, `app/core/censo_escritas.py:424`.

   `modelos()` atende literalmente à busca por nomes de classes, mas inclui qualquer classe de topo com bases, exceto nomes terminados em `QuerySet`/`Manager`. Por exemplo, inclui a exceção `ResolvedoraCruzada(ValueError)`: duas citações poderiam agrupar uma “entidade” que não é modelo Django. (`app/core/censo_escritas.py:89`, `app/core/censo_escritas.py:108`, `app/chamados/models.py:2179`, `app/core/tests/test_censo_escritas.py:264`)

   O fallback BACKLOG → PROGRAMA ainda carece de resolvedor: as relações documentais aparecem como “1o do programa”, “complemento” e “linha da E4”. Uma busca simples por token não cobre essas formas. (`app/docs/BACKLOG.md:771`, `app/docs/BACKLOG.md:773`, `app/docs/BACKLOG.md:774`)

10. **Regra GATILHO: exige contador por entidade e nomeação por associação evento–entidade.**

    O contador atual mede reprovações e voltas Codex por obra; não implementa `>=2 eventos na janela` e `>=1 não nomeado`. (`bin/disjuntor.py:909`, `bin/disjuntor.py:912`)

    **Contraexemplo de redisparo:** `e1` e `e2` são `Batida`, continuam na janela e foram nomeados; chega `e3`, também `Batida`, não nomeado. Um simples marcador permanente “entidade liberada” deixaria passar o terceiro evento; o contrato exige novo disparo. Estado sintético **[nao verificado]**; o mecanismo atual de exclusão é por IDs liberados/rearmados do B3. (`bin/disjuntor.py:898`, `bin/disjuntor.py:905`)

    **Contraexemplo com múltiplas entidades:** `e1` pertence a `{Batida, CelulaDia}`. Liberar `Batida` e escrever um único booleano global `e1.nomeado=true` também nomearia indevidamente sua participação em `CelulaDia`. O registro precisa distinguir essas associações. Proposta e cenário **[nao verificado]**.

    A permanência de um disparo depois de todos os eventos saírem da janela não está definida no anexo; copiar o travamento persistente do B3 escolheria esse comportamento implicitamente. **[nao verificado]**; persistência atual em `bin/disjuntor.py:912`, `bin/disjuntor.py:922`, `bin/disjuntor.py:938`.

11. **Regra LIBERAÇÃO: atomicidade aproveitável; sintaxe e episódio são novos.**

    `linhas_libera_b3()` não reconhece `PADRAO-DE-CIMA <entidade>: classe=… padrao=… armadilha=… obra=…`; aceita somente o desenho B3 ou erro bobo. (`bin/disjuntor.py:626`, `bin/disjuntor.py:639`, `bin/disjuntor.py:650`)

    Há um extrator de vocabulário em `bin/tests/test_lei_akita.sh`, dentro de heredoc Python. Ele consulta GLOSSARIO/PLANO por `git show`, compara termos completos e não oferece uma regra independente para “nome clássico declarado”. (`bin/tests/test_lei_akita.sh:65`, `bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:89`, `bin/tests/test_lei_akita.sh:138`) A forma dessa declaração continua **[nao verificado]**.

    **Contraexemplo de liberação insuficiente:** entidade disparada com eventos de `{OBRA-A, OBRA-B}`; aplicar o consumo atual apenas a `b3[OBRA-A]` não nomeia o episódio da entidade nem libera coerentemente as duas obras. O consumo atual pertence a cada obra. Cenário **[nao verificado]**; `bin/disjuntor.py:909`, `bin/disjuntor.py:932`, `bin/disjuntor.py:939`.

    **Contraexemplo de persistência:** se a gravação do consumo falhar, responder “liberado” permitiria reutilizar a mesma linha na chamada seguinte. O B3 já oferece o tratamento correto dessa falha, restaurando a negativa após escrita malsucedida. (`bin/disjuntor.py:958`, `bin/disjuntor.py:960`)

12. **Campos de estado: hoje não há escritores/leitores dos campos novos; há estes donos concretos da infraestrutura existente.**

    | Campo/artefato existente | Quem escreve | Quem lê |
    |---|---|---|
    | `disjuntor_estado.json` físico | `_com_estado`, temporário e substituição: `bin/disjuntor.py:237`, `bin/disjuntor.py:271` | `_com_estado` e `ler_estado`: `bin/disjuntor.py:260`, `bin/disjuntor.py:291` |
    | `b3[obra].reprovados`, `codex`, `disparado` | `decide_b3`: `bin/disjuntor.py:900`, `bin/disjuntor.py:907`, `bin/disjuntor.py:913` | `decide_b3`: `bin/disjuntor.py:909`; resultado por `estado_b3`: `bin/disjuntor.py:989` |
    | `disparado_em`, `topo_no_disparo` | `_fotografa`: `bin/disjuntor.py:727`, `bin/disjuntor.py:730` | `decide_b3`: `bin/disjuntor.py:927`, `bin/disjuntor.py:934` |
    | `consumidas`, `liberado` | `decide_b3`: `bin/disjuntor.py:936`, `bin/disjuntor.py:939` | `decide_b3`: `bin/disjuntor.py:898`, `bin/disjuntor.py:932` |
    | `<resposta>.id`: obra/papel/hash/pid/fim/sha | Wrapper: `bin/codex_pergunta.sh:198` | `_le_id` e triagem: `bin/disjuntor.py:753`, `bin/disjuntor.py:795` |
    | `triagem.log`; `b3_triagem_linhas` | `_triagem_sob_trava`: `bin/disjuntor.py:812`, `bin/disjuntor.py:815`; contador também atualizado em `bin/disjuntor.py:853` | `_triagens`: `bin/disjuntor.py:823` |
    | `disjuntor.log` | `trilha`/`dispara`: `bin/disjuntor.py:128`, `bin/disjuntor.py:139` | `ler` → `linha` → ESTADO: `bin/disjuntor.py:1554`, `bin/disjuntor.py:1677`, `bin/gerar_estado.py:280` |

    Para os campos novos, o mapa necessário seria o seguinte; **nomes de estruturas e donos futuros são proposta, sem implementação [nao verificado]**:

    | Novo estado necessário | Escritor proposto | Leitores propostos |
    |---|---|---|
    | SHA, `visto_em`, obra e entidades de cada evento | Varredura na decisão única | Janela, agrupamento e painel |
    | Nomeação por `(SHA, entidade)`/episódio | Transição de liberação | Gatilho e redisparo |
    | Entidade disparada e obras alcançadas | Transição de disparo/liberação | PreToolUse, wrapper e Stop |
    | SHA de nomeação consumido | Liberação, antes de autorizar | Próximas liberações |
    | Último escopo legível recuperável | Persistência da decisão válida | Tratamento restrito de `NAO_SEI` |
    | Contagens de desenhos, disparos, liberações e `None` | Decisão/registro de transições | Painel e ESTADO |

13. **Parser de CODEX_OBRA: mora hoje em `disjuntor.py`; é diferente do parser de prompts.**

    **Comando Bash:** `_trechos()` separa invocações; `_atribuicoes()` usa `shlex.split()`, remove aspas e conserva a última atribuição; `_codex_cmds()` retorna `obra`. Não aplica a regex de nomes de `_obra_de()`. (`bin/disjuntor.py:1251`, `bin/disjuntor.py:1309`, `bin/disjuntor.py:1325`, `bin/disjuntor.py:1337`)

    **Wrapper:** recebe o valor efetivo de `CODEX_OBRA` e exige apenas que não esteja vazio; assim aceita `obra_teste` e `O9 (C)`, embora o primeiro não seja reconhecido por `_obra_de()`. (`bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/disjuntor.py:97`, `bin/disjuntor.py:521`)

    **Prompt de Agent:** aceita âncoras como `obra X`, `Obra: X`, Markdown e `leitura … da X`; escolhe a primeira ocorrência válida. IDs aceitam formas como `PADRAO-DE-CIMA`, `OBRA-x` e `O261`; IDs sem hífen podem carregar cauda: `O9 (c)` vira `O9 (C)`. Um ID com hífen descarta essa cauda: `OBRA-X (C)` vira `OBRA-X`. (`bin/disjuntor.py:97`, `bin/disjuntor.py:104`, `bin/disjuntor.py:504`, `bin/disjuntor.py:518`, `bin/disjuntor.py:526`)

    Há colisão por ausência de borda direita: `obra OBRA-X_Y` é lida como `OBRA-X`. Também há risco quando um prompt menciona outra obra antes da obra executada. (`bin/disjuntor.py:103`, `bin/disjuntor.py:104`, `bin/disjuntor.py:514`, `bin/tests/test_disjuntor.sh:693`)

    **Limites concretos do parser Bash:**

    | Forma | Resultado atual |
    |---|---|
    | `CODEX_OBRA="O9 (C)" CODEX_PAPEL=verificador …` | Extrai `O9 (C)`. (`bin/disjuntor.py:1314`, `bin/disjuntor.py:1337`) |
    | `env CODEX_OBRA=OBRA-A CODEX_PAPEL=verificador …` | Não reconhece essa disposição: atribuições são previstas antes dos prefixos. (`bin/disjuntor.py:1243`) |
    | `export CODEX_OBRA=OBRA-A; CODEX_PAPEL=verificador …` | Não transporta a obra entre trechos. (`bin/disjuntor.py:1292`, `bin/disjuntor.py:1335`) |
    | `CODEX_OBRA="$OBRA" CODEX_PAPEL=verificador …` | Extrai o literal `$OBRA`, sem expansão. (`bin/disjuntor.py:1314`, `bin/disjuntor.py:1321`) |

    Mudar a identidade de obra afeta contagem de transcripts, seleção de Agent, classificação de exceções e reconhecimento de liberação. As chaves persistidas e a triagem usam a obra como texto; uma nova normalização pode separar histórico antigo e chamadas novas. (`bin/disjuntor.py:557`, `bin/disjuntor.py:637`, `bin/disjuntor.py:850`, `bin/disjuntor.py:897`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1455`)

14. **Regra MORDE: os casos precisam testar a decisão nova, com agrupamento explícito.**

    As escritas abaixo permitem montar casos com entidades distintas sem banco: `registro_batida.py:152` e `:157` escrevem `Batida`; `portas/celula.py:247` escreve `CelulaDia`. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/portas/celula.py:247`)

    Com SHAs distintos, `visto_em=T−60/T−30` e ambos não nomeados, os resultados contratuais esperados seriam estes; execução do instrumento/mutante **[nao verificado]**:

    | Eventos | Programa | Esperado |
    |---|---|---|
    | `Batida`, `Batida` | Qualquer | Dispara |
    | `Batida`, `CelulaDia` | Diferentes | Não dispara |
    | `Batida`, `CelulaDia` | Mesmo programa | Não dispara |
    | `None`, `None` | Não resolvido | Mede dois; não agrupa |
    | Dois `Batida` nomeados + terceiro novo | Qualquer | Redispara |

    O mutante “conta global” deve falhar nos dois casos de entidades distintas; o terceiro caso também detecta uma implementação que soma programa apesar de entidade resolvida. Resultado efetivamente VERMELHO **[nao verificado]**.

    Os selos existentes criam arquivos e mutantes; sua execução não cabe na restrição de somente leitura desta auditoria. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:1478`, `bin/tests/test_disjuntor.sh:1487`)

## 31/10 16:38 · sessao eb6a9f7f9f33 · F1C4-3-EMISSORES · verificador · origem=Code

A delegação do ramo do supra está implementada, e os dois ramos do cartório entregam atas montadas no julgamento corrente. A exceção `_protesta` continua real: ela impede que a primeira cláusula do juiz seja alcançada pelo cartório quando `trabalha=False`. (`app/ponto/supra_juiz.py:65-71`; `app/ponto/services/cartorio.py:1012-1029`, `:1184-1214`)

Há também um defeito adicional no delta: a nova deduplicação de Pauta permite duplicatas sob concorrência, por intercalação entre a consulta e a criação. Ocorrência efetiva em produção: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:180-185`; `app/pautas/services.py:152-185`; `app/pautas/models.py:113-133`)

**1. Escritores, leitores e autoridades**

O predicado efetivo do juiz é:

```python
cel.trabalha is False or bool(
    ata.get('tipo_dia') and ata.get('tipo_dia') != 'trabalho'
)
```

Portanto, a segunda cláusula exige um valor verdadeiro: `None` e `''` não bastam; `trabalha=None` pode ser calado por uma ata preenchida com tipo diferente de trabalho. (`app/chamados/juizes.py:407-408`)

| Campo/valor | Quem escreve | Quem lê e como decide |
|---|---|---|
| `CelulaDia.trabalha` | Geração/regeneração pela porta, a partir do calendário calculado; há também limpeza para `False` e reversão pelo DNA anterior. (`app/ponto/portas/celula.py:231-260`, `:532`, `:590-615`) | O juiz lê `is False`; fábrica, materialização, cluster e intervalo passaram a consultá-lo. (`app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:80`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `ata.tipo_dia` | `ata_do_dia` copia o tipo da grade; a persistência passa por `lavrar_veredito`. (`app/ponto/services/cartorio.py:568`; `app/escala/models.py:1787-1796`; `app/ponto/portas/celula.py:726-768`) | O juiz aplica a segunda cláusula. `grade_da_celula` mantém projeção própria de tipo; declaração mantém veto próprio por tipo. (`app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:345-346`; `app/chamados/services/declaracao_texto.py:95`) |
| `ata.minutos_previstos` | Cartório grava `int(dia.get('minutos_previstos') or 0)` pela mesma porta. (`app/ponto/services/cartorio.py:569`; `app/ponto/portas/celula.py:726-768`) | O leitor usa o número da ata; se ausente, consulta `minutos_previstos_do_dia` somente quando `cel.trabalha` é verdadeiro, senão usa zero. O supra lê a projeção em `prev`. Esses passos não consultam `dia_sem_trabalho`. (`app/escala/services/leitor_celula.py:347-354`; `app/ponto/supra_juiz.py:223`) |
| `ata.lampadas` | `lampadas_do_dia` monta; `ata_do_dia` incorpora; dispensa por ausência/justificativa e teto podem alterar as luzes antes da lavra. (`app/ponto/services/cartorio.py:541-569`, `:611-624`, `:707-717`) | Materialização lê slots/luzes; cluster conta apagadas; intervalo indexa lâmpadas; leitor projeta células; `lastro` procura os marcos cobrados. São perguntas sobre os marcos, adicionais ao gate de dia sem trabalho. (`app/chamados/services/materializacao.py:158-164`; `app/ponto/services/cluster_espurio.py:29-31`; `app/ponto/management/commands/detectar_intervalo_ausente.py:64`; `app/escala/services/leitor_celula.py:92-121`; `app/ponto/services/lastro.py:82-95`) |
| `INSUMO_DIA_SEM_TRABALHO` | Constante do supra; escrita pelo adaptador e por `grade_da_celula`. Cartório usa o adaptador nos dois ramos. (`app/ponto/supra_juiz.py:57-62`; `app/escala/services/leitor_celula.py:370`; `app/ponto/services/cartorio.py:1029`, `:1213`) | `_dia_sem_trabalho_pelo_juiz` exige a chave e entrega `trabalha` e `ata` ao juiz, sem predicado substituto. (`app/ponto/supra_juiz.py:65-71`) |
| `chamados_vivos` | Commands passam `len(vivos)`, selecionados por `VIVOS`; cartório normal passa `len(_protestam)`; `nunca_bateu` passa zero. (`app/chamados/catalogo/motor.py:24`; `app/ponto/management/commands/supra_juiz.py:194-208`; `app/ponto/management/commands/emitir_furo_retroativo.py:92-101`; `app/ponto/services/cartorio.py:1029`, `:1206-1214`) | O supra exige contagem verdadeira nesse ramo. A contagem dos commands não consulta o juiz de dia; a do cartório passa pelo predicado próprio `_protesta`. (`app/ponto/supra_juiz.py:470-472`; `app/ponto/services/cartorio.py:1206-1208`) |
| `cobertura_status` | Vem de `ausencia_cobre(..., para='cobranca')`, por `getattr(cob, 'status', None)`, nos commands e no cartório. (`app/ponto/management/commands/supra_juiz.py:199-209`; `app/ponto/management/commands/emitir_furo_retroativo.py:97-102`; `app/ponto/services/cartorio.py:982`, `:1030`, `:1108`, `:1216`) | O ramo exige **exatamente `None`**, não apenas “não aprovada”. É guarda de cobertura, não outro juiz de dia sem trabalho. (`app/ponto/supra_juiz.py:471`) |
| Novos `Pauta.texto`/`ancora_id` | Fábrica constrói `colaborador_id:AAAA-MM-DD`, marcador e tags; chama `escrever`, que normaliza, acrescenta contexto e persiste. Não há criação direta de Pauta pela fábrica. (`app/chamados/services/fabrica_da_celula.py:175-190`; `app/pautas/services.py:163-185`) | A fábrica relê âncora/texto para deduplicar; API expõe ambos; painel mostra texto e tenta navegar pela âncora; chip consulta a âncora. Não são decisões de cobrança. (`app/chamados/services/fabrica_da_celula.py:180-183`; `app/pautas/views.py:40-41`; `app/templates/core/_copiloto.html:405`, `:564-573`; `app/pautas/services.py:334-338`) |

A fábrica consulta o juiz na entrada, mas depois passa por `motivos_pela_ata → _ata_do_dia → _marcos_faltantes`, onde sobrevive o veto próprio `tipo_dia != 'trabalho'`. Portanto, delegar o primeiro gate não eliminou todos os predicados do caminho. (`app/chamados/services/fabrica_da_celula.py:49`, `:80`, `:111`; `app/chamados/services/declaracao_texto.py:79-96`)

Os leitores anteriores `_celula_furada`, `ChamadoColaborador` e `reconciliar_grade` também consultam o juiz; possuem seus próprios universos e guardas ao redor da consulta. (`app/chamados/juizes.py:411-443`; `app/chamados/models.py:1227-1232`; `app/ponto/management/commands/reconciliar_grade.py:274-282`)

Nos testes, há escrita direta de `trabalha/ata` para construir as situações adversariais; isso não representa um novo escritor de produção. (`app/chamados/tests/test_f1c4_3_emissores.py:65-66`; `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:43`, `:100-101`)

**2. Produtores do dict, ausência de insumo e idade da ata**

| Caminho | Produção do dict e ata entregue |
|---|---|
| Cron/command `supra_juiz` | Usa `grade_da_celula`: insumo com `cel.trabalha` e **ata já gravada**. (`app/ponto/management/commands/supra_juiz.py:116`, `:206-209`; `app/escala/services/leitor_celula.py:332`, `:370`) |
| `emitir_furo_retroativo` | Mesmo leitor e **ata já gravada**. (`app/ponto/management/commands/emitir_furo_retroativo.py:59`, `:99-102`; `app/escala/services/leitor_celula.py:332`, `:370`) |
| Cartório, `nunca_bateu` | Monta `_ata_nb` neste julgamento, entrega-a ao helper e depois à lavra. (`app/ponto/services/cartorio.py:1012`, `:1029`, `:1044-1045`) |
| Cartório, normal | Monta `_ata_agora.ata`, publica em `_tl.atas` e entrega esse objeto ao helper antes da lavra final. (`app/ponto/services/cartorio.py:1184-1188`, `:1213`, `:1563-1565`) |
| Evento `julgar_celula` | Carrega a célula e chama `julgar_colab`; desemboca nos mesmos dois ramos, não cria outro adaptador de dict. (`app/ponto/services/cartorio.py:1708-1723`, `:777`) |

O leitor produz `fora_vinculo` e `sem_celula` **sem insumo**, mas ambos são barrados por `SEM_JURISDICAO` antes do acesso obrigatório. Com célula no ramo normal do leitor, a chave é acrescentada. (`app/escala/services/leitor_celula.py:340-370`; `app/ponto/supra_juiz.py:48`, `:470-472`)

Nos caminhos de produção identificados, não encontrei um dict sem insumo que alcance esse acesso: são os dois commands e as duas chamadas do cartório. A catraca declara esses mesmos arquivos. Ausência efetiva de `KeyError` em produção: **[não verificado]**. (`app/ponto/management/commands/supra_juiz.py:206`; `app/ponto/management/commands/emitir_furo_retroativo.py:99`; `app/ponto/services/cartorio.py:1028`, `:1212`; `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:161-165`)

Nos testes existem estes modos de produção do dict: [nao verificado]

- Pelo leitor real, após alteração da célula: recebe ata gravada na fixture e insumo do leitor. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:39-45`)
- Pelo cartório real, com espião no adaptador: recebe ata corrente. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:89-104`)
- Pelo adaptador em fixtures de folga/indefinido. (`app/ponto/tests/test_f1c4_3_supra_indefinido_nao_retrata.py:17-24`; `app/ponto/tests/test_supra_juiz.py:231-235`; `app/ponto/tests/test_supra_juiz_cobranca.py:102-105`)
- Por dict manual com insumo explícito, inclusive tipo externo diferente do tipo na ata. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:147-155`)
- Por dict manual sem insumo: deliberadamente para exigir `KeyError`, testar `SEM_JURISDICAO` ou exercitar caminhos barrados por previsto/chamados/cobertura. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:135-145`; `app/ponto/tests/test_supra_juiz_cobranca.py:107-119`; `app/ponto/tests/test_supra_juiz_indefinido.py:21-48`)

**`tipo_dia='trabalho'` não protege mais um dict sem insumo**: previsto zero, chamados vivos e cobertura `None` fazem esse dict chegar ao acesso obrigatório. A proteção por tipo agora cobre somente `SEM_JURISDICAO`. (`app/ponto/supra_juiz.py:470-472`)

**3. Sítios que conservam outra decisão**

| Sítio | Predicado e universo | Equivalência ao juiz |
|---|---|---|
| Cartório `_protesta` | `trabalha is not False and marcos_acesos(...) is not True`, sobre chamados vivos; também participa de `cobrado`, adiamento e reconciliação. (`app/ponto/services/cartorio.py:1206-1214`, `:1250`, `:1281-1287`) | **Não**: ignora `ata.tipo_dia` e considera satisfação dos marcos. (`app/ponto/services/cartorio.py:1207`; `app/chamados/juizes.py:408`) |
| Cartório, emissão total | Exige `ec.eh_dia_trabalho(data) is True`, depois dos gates de furo, situação, piso e afastamento. (`app/ponto/services/cartorio.py:1492-1507`) | **Não**: com célula, essa autoridade lê `trabalha`, sem a segunda cláusula da ata. (`app/escala/models.py:1066-1073`) |
| Cartório `_marcos_do_furo_parcial` | Recusa vínculo ausente ou `eh_dia_trabalho is not True`. (`app/ponto/services/cartorio.py:1604-1609`) | **Não**, pela mesma distinção entre previsão e ata. (`app/escala/models.py:1071-1073`) |
| Declaração `_marcos_faltantes` | Só aceita tipo projetado `trabalho`; lê dia e véspera, desta aproveitando somente saída. (`app/chamados/services/declaracao_texto.py:79-96`, `:116-123`) | **Não**: não aplica a cláusula soberana quando a ata diz trabalho; universo inclui véspera. (`app/chamados/services/declaracao_texto.py:88-96`; `app/chamados/juizes.py:408`) |
| Disputa, identificação do dia | Só aceita `tipo_dia == 'trabalho'` na grade da data da entrada aberta. (`app/chamados/services/disputa_emissao.py:56-70`) | **Não**: decide pelo tipo projetado, sem consultar o juiz. (`app/chamados/services/disputa_emissao.py:65-68`) |
| Disputa `_tem_base` | Aceita batida primeiro; senão previsão `is not False`. (`app/chamados/services/disputa_emissao.py:749-753`) | **Não**: aceita `None` e não consulta o tipo da ata nesse gate. (`app/chamados/services/disputa_emissao.py:750-753`) |
| Lote de perguntas | Pré-seleciona somente `trabalha=True` na janela. (`app/chamados/management/commands/disparar_perguntas_competencia.py:74-76`) | **Não**: `None/trabalho` fica fora antes dos demais filtros. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`; `app/chamados/juizes.py:408`) |
| Fila por causa | Dias previstos são células `trabalha=True` dos colaboradores da fila. (`app/chamados/services/fila_por_causa.py:191-200`) | **Não**: considera somente o campo, sem ata. (`app/chamados/services/fila_por_causa.py:192-194`) |
| Vigia de hora | Candidatas `trabalha=True`, empresa, hoje/ontem e janela do turno. (`app/ponto/services/vigia_de_hora.py:15-26`) | **Não**: `None` não chega ao julgamento por essa seleção. (`app/ponto/services/vigia_de_hora.py:21-23`) |
| Vigia de entrada | Somente `trabalha=True` e ata com lâmpadas, na janela ou data consultada. (`app/ponto/services/vigia_entrada.py:73-85`, `:120-125`) | **Não**: mesmo recorte por `True`; é instrumento de leitura. (`app/ponto/services/vigia_entrada.py:79`, `:120`) |
| `dia_do_colab` | `not cel.trabalha` produz “Dia sem trabalho previsto”. (`app/ponto/services/dia_do_colab.py:121-123`) | **Não**: inclui `None` nessa descrição. (`app/ponto/services/dia_do_colab.py:121`; `app/chamados/juizes.py:408`) |
| `passivo_s84` | Célula truthy vira TRABALHO; `False/None` viram FOLGA, sobre batidas atribuídas ao dia-turno. (`app/ponto/services/passivo_s84.py:88-96`) | **Não**: colapsa o indeterminado em folga; a classificação participa da seleção posterior. (`app/ponto/services/passivo_s84.py:96`, `:108-112`) |
| Veto em `ponto.marcos` | `trabalha is False` dá FOLGA; depois considera vereditos vetados. (`app/ponto/marcos.py:155-163`) | Só coincide com a primeira cláusula; responde ao veto, com outro conjunto de condições. (`app/ponto/marcos.py:159-162`) |
| `flip_auto` | Célula `False` recusa dia diurno; no noturno exige véspera `True` para recuar. (`app/ponto/services/flip_auto.py:201-215`) | **Não**: decisão geométrica entre dia/véspera, sem tipo da ata. (`app/ponto/services/flip_auto.py:208-215`) |
| Worklist | Tipo projetado diferente de trabalho retorna gap indeterminado antes da leitura das lâmpadas. (`app/ponto/worklist.py:18-26`) | **Não**: não aplica a primeira cláusula soberana. (`app/ponto/worklist.py:19-22`) |
| Bordas do realizado | Só tipo projetado `trabalho`, entre vínculos ativos e dentro da janela. (`app/ponto/services/bordas_realizado.py:62-75`) | **Não**: universo de auditoria do realizado. (`app/ponto/services/bordas_realizado.py:76-87`) |
| Outros ramos do supra | Conservam gates por tipo; `dia_de_cobranca` admite ausência parcial. (`app/ponto/supra_juiz.py:281`, `:304-308`, `:402-420`, `:443`, `:476-491`) | **Não**: não foram todos migrados; ausência parcial pode ser cobrável ali enquanto o juiz cala ata `ausencia`. (`app/ponto/supra_juiz.py:299-305`; `app/chamados/juizes.py:408`) |

Há ainda recortes próprios nos instrumentos: auditor de chamados (`not trabalha`/`is not True`), esmeril (`is False`/truthy), tripwire (`True`, ou `False/None` mais ata folga), `dna_x_batida_real` e `plano_b_no_dinheiro` (`True`). Seus universos são invariantes e diagnósticos, não a mesma pergunta de cobrança do juiz. (`app/chamados/management/commands/auditar_invariantes_chamados.py:376`, `:1443`, `:1542`; `app/ponto/services/esmeril_espelho.py:71`, `:121-124`; `app/ponto/management/commands/tripwire_celulas.py:98`, `:257`, `:338-344`; `app/ponto/management/commands/dna_x_batida_real.py:58-65`; `app/ponto/management/commands/plano_b_no_dinheiro.py:142-144`)

Os leitores de **previsão** também precisam permanecer distinguidos: alerta avançado usa resultado falsey; retroativo exige `True`; detector de anomalias trata `None` como indeterminação e `False` como folga; faltas de hoje aceita `True` ou defeito cadastral; plantio overnight verifica `False`. Eles consultam a autoridade prevista, que, havendo célula, retorna `cel.trabalha`. (`app/ponto/management/commands/processar_alertas_avancados.py:34-50`; `app/ponto/management/commands/emitir_furo_retroativo.py:117-129`; `app/chamados/detector_anomalias.py:164-170`; `app/ponto/services/faltas_de_hoje.py:32-42`; `app/ponto/management/commands/vigia_plantio_overnight.py:38-50`; `app/escala/models.py:1071-1073`)

`detectar_ausencias` conserva tratamento específico do limbo previsto; propositor exige tipo trabalho em dicts; simulação e `veredito_celula` julgam grade remontada. Não são consultas equivalentes ao juiz da cobrança lavrada. (`app/ponto/management/commands/detectar_ausencias.py:136-159`; `app/ponto/propositor.py:34-37`, `:201-211`; `app/ponto/services/simular_dia.py:5-12`; `app/ponto/services/veredito_celula.py:216-222`)

A documentação reconhece parte dessa dívida em `F1C4-3-NONE-LEITORES` e `F1C4-3-EMISSORES-FILA`; não sustenta que todos esses leitores tenham migrado. (`app/docs/BACKLOG.md:805`, `:823`)

**4. Os dois testes novos** [nao verificado]

| Teste | Mutação mínima no cartório que deve deixá-lo vermelho, por análise estática | [nao verificado]
|---|---|
| `test_MORDE_ramo_nunca_bateu_entrega_a_ata_deste_julgamento` | Trocar `ata=_ata_nb` por `ata=cel.ata` em `app/ponto/services/cartorio.py:1029`: entrega a sentinela e tipo `folga`, contrariando as asserções. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-111`, `:116-119`) |
| `test_MORDE_ramo_normal_entrega_a_ata_deste_julgamento` | Trocar `ata=_ata_agora.ata` por `ata=cel.ata` em `app/ponto/services/cartorio.py:1213`: falha pelas mesmas verificações e pela identidade com a ata em curso. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-111`, `:121-132`) |

**Nenhum passa se o espião nunca for chamado**: `assertTrue(vistos)` antecede o laço. **Nenhum passa recebendo a ata anterior intacta**: ambos rejeitam a sentinela e exigem tipo `trabalho`. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-114`)

Não passam por fixture vazia quanto ao **transporte observado**: exigem chamada, conteúdo e substituição da ata gravada; o normal também exige identidade com `_tl.atas`. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:107-114`, `:132`)

Entretanto, não demonstram a execução do ramo de cobrança nem a chamada efetiva ao juiz: `nunca_bateu` passa zero vivos; a fixture normal começa com célula de trabalho e jornada prevista. (`app/ponto/services/cartorio.py:1029`; `app/ponto/supra_juiz.py:471-472`; `app/ponto/tests/test_bo_inss_nunca_bateu_le_a_precedencia.py:72-90`)

O teste `nunca_bateu` é mais fraco quanto à integridade da ata: trocar seu argumento por `{'tipo_dia': 'trabalho'}` satisfaz as verificações de conteúdo, mesmo omitindo minutos e lâmpadas. O normal exige identidade com a ata em curso, mas nenhum dos dois compara integralmente o insumo com a ata persistida. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:106-114`, `:132`)

Execução dos testes Django e dos mutantes completos: **[não verificado]**. [nao verificado]

**5. Correspondência entre documentos e código**

- **Correto:** migração do ramo no mesmo pouso, guarda `SEM_JURISDICAO` antes do juiz e transporte pelo insumo. (`app/docs/CORTES.md:415`; `app/docs/JUIZES.md:58-59`; `app/ponto/supra_juiz.py:470-472`)
- **Correto:** ata deste julgamento nos dois ramos do cartório. (`app/docs/CORTES.md:415`; `app/ponto/services/cartorio.py:1012-1029`, `:1184-1213`)
- **Correto com qualificação:** `_protesta` conserva somente a primeira cláusula **sobre a natureza do dia**; a função inteira também pergunta por marcos acesos. “Zera chamados vivos” vale para `trabalha=False`, não incondicionalmente. (`app/docs/BACKLOG.md:807`; `app/docs/JUIZES.md:60-61`; `app/core/juizes.py:144-146`; `app/ponto/services/cartorio.py:1206-1214`)
- **Imprecisão contradita pelos casos vazios:** a fórmula documental `ata.tipo_dia != trabalho` omite a guarda de preenchimento. Para `True/None` com tipo `None/''`, o juiz não cala. (`app/docs/CORTES.md:415`; `app/docs/JUIZES.md:56`; `app/core/juizes.py:138`; `app/chamados/juizes.py:408`)
- **Imprecisão se lida literalmente:** “`trabalha=None` COBRA” não vale quando a segunda cláusula é verdadeira. `None` com ata `folga`, `ausencia`, `falta` ou `indefinido` é dia sem trabalho para o juiz. (`app/docs/JUIZES.md:57`; `app/core/juizes.py:139`; `app/chamados/juizes.py:408`)
- **Ressalva temporal ausente em `_C18`:** “célula JÁ LAVRADA” não descreve esse leitor do cartório, que consulta a ata corrente antes da persistência final; `CORTES` descreve essa exceção corretamente. (`app/core/juiz

[truncado em 20000 de 24186 caracteres]

## 31/10 16:38 · sessao 1d6ef70ea0d8 · - · - · origem=Ronald (tmux)

1. **Situação do instrumento.** O código atual é B3 por **obra**, alimentado por veredictos de transcripts e triagens Codex; o disparo usa `n>=1` reprovação final ou `nc>=3` voltas procedentes, sem entidades ou janela de desenhos (`bin/disjuntor.py:736`, `bin/disjuntor.py:862`, `bin/disjuntor.py:896`, `bin/disjuntor.py:912`). Não existem nessa transição os campos `visto_em`, entidade, eventos nomeados ou consumo de `PADRAO-DE-CIMA`; o estado manipulado é `b3`, `b3_sem_obra`, reprovações, Codex e liberações por obra (`bin/disjuntor.py:891`, `bin/disjuntor.py:936`). Resultado executado do instrumento novo: [não verificado].

2. **Quem chama / quem espera.** Há pontos de integração: PreToolUse chama `main_pretool` pela configuração; Stop chama `julga_b3` para acrescentar recados; wrapper pergunta diretamente `--b3-parada` depois de obter slot e trava da resposta (`.claude/settings.json:68`, `bin/hook_stop_fila1.py:1048`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:165`). Hoje o Agent alcançado por B3 é somente `subagent_type=verificador` cujo prompt passa `_eh_final`; construtor e verificador de desenho não são alcançados por esse ramo (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:499`). Portanto reutilizar o ramo sem ampliá-lo deixa passar os Agents que o contrato novo pretende bloquear (`bin/disjuntor.py:1454`).

3. **Evento / tempo.** `_topo_consistente` já lê o RELATO inteiro duas vezes e verifica bytes/stat, mas devolve somente as duas primeiras linhas úteis recortadas por `_bloco_do_topo`; serve à liberação atual, não à varredura de todos os eventos vivos (`bin/disjuntor.py:662`, `bin/disjuntor.py:679`, `bin/disjuntor.py:686`, `bin/hook_stop_fila1.py:415`, `bin/hook_stop_fila1.py:439`). `_sha_b3` fornece SHA-256 truncado a 16 hex após `strip().lstrip('*_`> ')`; é normalização específica do topo, não definição pronta para identidade imutável de evento (`bin/disjuntor.py:571`). A janela de 24h atual é apenas painel/trilha, baseada no timestamp da trilha, não `visto_em` de desenho (`bin/disjuntor.py:1548`, `bin/disjuntor.py:1563`). Contraexemplo hipotético: dois eventos válidos na terceira e quarta linhas do RELATO seriam invisíveis se o instrumento usar diretamente a saída de `_topo_consistente` (`bin/hook_stop_fila1.py:439`). Contraexemplo hipotético de identidade: `DESENHO OBRA-X: ...` e `> DESENHO OBRA-X: ...` têm o mesmo hash nessa função; se `>` deveria ser semanticamente diferente, a normalização precisa ser especificada (`bin/disjuntor.py:573`).

4. **Disparo / liberação.** Existem a trava única, gravação por temporário+`os.replace`, anúncio após gravação e reversão da liberação se não gravou; são infraestrutura reutilizável (`bin/disjuntor.py:237`, `bin/disjuntor.py:271`, `bin/disjuntor.py:274`, `bin/disjuntor.py:948`, `bin/disjuntor.py:958`). O reconhecedor existente só aceita `DESENHO <obra>: lei=... pergunta=... <arquivo>:<linha>` ou `ERRO-BOBO`; não valida classe/padrão/armadilha/obra nem consulta GLOSSÁRIO/PLANO (`bin/disjuntor.py:626`, `bin/disjuntor.py:639`, `bin/disjuntor.py:650`). O consumo atual é por **obra**: `b3[obra].consumidas`; não é consumo global do SHA de um episódio por entidade (`bin/disjuntor.py:909`, `bin/disjuntor.py:932`, `bin/disjuntor.py:939`). Contraexemplo hipotético: se uma mesma linha de liberação nomear várias obras, o consumidor atual pode consumi-la uma vez em cada `b3[obra]`, porque o histórico consumido pertence a cada obra (`bin/disjuntor.py:637`, `bin/disjuntor.py:939`).

5. **Falha/NAO_SEI.** O contrato está desatualizado ao afirmar que a exceção do envelope hoje libera todo ramo: o B3 já fecha a exceção com negativa por `_ramo_b3`; outros ramos ainda falham abertos (`bin/disjuntor.py:1496`, `bin/disjuntor.py:1498`, `bin/disjuntor.py:1506`). O comportamento B3 atual excede o escopo proposto: `julga_b3` retorna `{}`+motivo em qualquer exceção e `estado_b3` devolve `NAO_SEI` para **qualquer obra consultada** se há motivo; não restringe a obras do último estado legível (`bin/disjuntor.py:977`, `bin/disjuntor.py:982`, `bin/disjuntor.py:989`, `bin/disjuntor.py:992`). `ler_estado` não mantém cópia anterior recuperável e `_com_estado` preserva o JSON ilegível, levantando erro (`bin/disjuntor.py:259`, `bin/disjuntor.py:265`, `bin/disjuntor.py:291`). Contraexemplo hipotético: último estado legível disparou apenas `OBRA-X`; JSON vira `{lixo`; consulta verificador de `OBRA-Y` também recebe NAO_SEI/recusa pelo B3 atual (`bin/disjuntor.py:982`, `bin/disjuntor.py:993`, `bin/disjuntor.py:1046`). Para implementar o limite novo falta uma fonte durável recuperável de `obras_em_entidades_disparadas`; seu escritor/leitor ainda não existe nas funções de estado examinadas (`bin/disjuntor.py:237`, `bin/disjuntor.py:291`).

6. **Medida.** Há `dispara` para trilha padronizada, `ler` para agregações 24h e `linha` para painel; ESTADO importa ambos diretamente (`bin/disjuntor.py:139`, `bin/disjuntor.py:1546`, `bin/disjuntor.py:1677`, `bin/gerar_estado.py:280`). O painel enumera IDs fixos B1/B2/B3/B4/B4-CODEX/ANEXO-ENXUTO; adicionar só um novo evento à trilha não o faz aparecer nessa enumeração, e não há desenhos por entidade ou eventos None (`bin/disjuntor.py:1679`, `bin/disjuntor.py:1615`). Escritores atuais de métricas: `trilha`/`dispara` e anúncio da decisão; leitores: `ler`/`linha` e gerador de ESTADO (`bin/disjuntor.py:128`, `bin/disjuntor.py:139`, `bin/disjuntor.py:948`, `bin/disjuntor.py:1554`, `bin/gerar_estado.py:281`).

7. **Desliga — falha concreta na porta atual.** `desligado` já usa `lexists` para a chave global; PreToolUse e Stop respeitam-na (`bin/disjuntor.py:144`, `bin/disjuntor.py:1487`, `bin/hook_stop_fila1.py:1039`). Porém o wrapper sempre consulta B3 para verificador, e o caminho CLI → `b3_parada` → `estado_b3` → `decide_b3` não consulta a chave (`bin/codex_pergunta.sh:164`, `bin/disjuntor.py:1709`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:989`, `bin/disjuntor.py:877`). Contraexemplo hipotético: chave existe, `b3["OBRA-X"]={"reprovados":["a1"],"disparado":true}`, fontes legíveis; o hook fica desligado, mas wrapper recusa OBRA-X com rc3 (`bin/disjuntor.py:1487`, `bin/disjuntor.py:1043`, `bin/codex_pergunta.sh:165`). A proteção da chave é textual e muito ampla: até Bash meramente contendo `DISJUNTOR_DESLIG` é negado; não autentica autoria Ronald por credencial (`bin/disjuntor.py:160`). Não prometer “só Ronald” como propriedade de segurança forte (`bin/disjuntor.py:152`).

8. **Mapa de estado existente e proposta explicitamente ausente.**
   - Estado físico: leitura/escrita exclusivamente pela infraestrutura `_com_estado` e leitura independente `ler_estado`; trava `.lock`, temporário e substituição atômica (`bin/disjuntor.py:247`, `bin/disjuntor.py:249`, `bin/disjuntor.py:260`, `bin/disjuntor.py:271`, `bin/disjuntor.py:291`).
   - `b3_limiar`: `_migra_limiar` lê e escreve (`bin/disjuntor.py:699`, `bin/disjuntor.py:710`).
   - `b3_sem_obra`: `decide_b3` acumula; `_b3_sem_obra` lê para painel (`bin/disjuntor.py:892`, `bin/disjuntor.py:895`, `bin/disjuntor.py:1624`).
   - `b3[obra].reprovados`: `decide_b3` acumula, mede e zera ao liberar; rearme também zera (`bin/disjuntor.py:900`, `bin/disjuntor.py:910`, `bin/disjuntor.py:938`, `bin/disjuntor.py:1020`).
   - `codex`: decisão recalcula da triagem, mede e zera; `_triagens` lê os IDs existentes para impedir redução da fonte (`bin/disjuntor.py:832`, `bin/disjuntor.py:907`, `bin/disjuntor.py:911`, `bin/disjuntor.py:938`).
   - `disparado`: decisão e rearme escrevem; decisão lê (`bin/disjuntor.py:913`, `bin/disjuntor.py:922`, `bin/disjuntor.py:938`, `bin/disjuntor.py:1020`).
   - `disparado_em`, `topo_no_disparo`: `_fotografa` escreve; decisão lê (`bin/disjuntor.py:727`, `bin/disjuntor.py:730`, `bin/disjuntor.py:927`, `bin/disjuntor.py:934`).
   - `consumidas`: decisão lê e acrescenta SHA por obra (`bin/disjuntor.py:932`, `bin/disjuntor.py:939`).
   - `liberado`, `rearmado`: decisão/liberação, migração e rearme escrevem; acumulação e recálculo leem para excluir IDs antigos (`bin/disjuntor.py:708`, `bin/disjuntor.py:898`, `bin/disjuntor.py:905`, `bin/disjuntor.py:936`, `bin/disjuntor.py:1018`).
   - `b3_triagem_linhas`: escritor de triagem incrementa, `_triagens` lê e atualiza (`bin/disjuntor.py:815`, `bin/disjuntor.py:829`, `bin/disjuntor.py:853`).
   - Campos **novos propostos**, ainda sem escritor/leitor: registro por SHA com `visto_em/obra/entidades/nomeado_em_episodio`; episódios por entidade com obras alcançadas; consumo global de SHA de nomeação; contadores por entidade/None; cópia recuperável de último escopo legível. Nenhuma dessas estruturas pertence ao schema B3 manipulado na decisão atual; funções futuras/nomes exatos: [não verificado] (`bin/disjuntor.py:881`, `bin/disjuntor.py:891`, `bin/disjuntor.py:936`).

9. **MORDE.** A suíte atual declara casos B3/Codex e mutantes de reconhecimento, atomicidade, NAO_SEI e porta, mas sua lista de execução não inclui casos PADRAO-DE-CIMA por entidade (`bin/tests/test_disjuntor.sh:1468`, `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1757`). Os testes criam diretórios/arquivos e mutantes: não executá-los sob a exigência de somente leitura (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:1478`, `bin/tests/test_disjuntor.sh:1487`). Resultado de teste e mutante global do instrumento novo: [não verificado]. Os casos necessários do contrato devem fixar estados explícitos: entidades E1/E2, mesmo programa P1, SHA distintos, dois `visto_em` dentro de 86400s, pelo menos um `nomeado=false`; com E1/E2 não dispara, com E1/E1 dispara e mutação que remove chave entidade deve tornar primeiro caso vermelho. Implementação desses testes hoje: [não verificado].

10. **Observação adicional para consolidação.** O parser já extrai `CODEX_OBRA` da atribuição imediata em `_codex_cmds`, contrariando o anexo; o docstring da função ficou desatualizado ao listar retorno sem `obra` (`bin/disjuntor.py:1325`, `bin/disjuntor.py:1326`, `bin/disjuntor.py:1337`). Não há AGENTS.md na árvore auditada pelo comando de localização; aplicabilidade de instruções externas: [não verificado].

## 31/10 16:38 · sessao 657183ad6699 · - · - · origem=Ronald (tmux)

1. **Entidade: material reutilizável existe, resolvedor do contrato ainda precisa ser construído.** `varrer_arvore()` entrega `{arquivo, linha, entidade, op, expr}`; caminhos são relativos a `app/`. Não entrega nome da função, intervalo da função, `end_lineno` ou atributo `DIRETA`; `_anotar()` guarda apenas linha, entidade, operação e expressão. A resolução de “arquivo:linha → entidade direta naquela linha/função” exige adaptador e, para referência dentro de função fora da linha de escrita, outro mapeamento AST. (`app/core/censo_escritas.py:361`, `app/core/censo_escritas.py:469`, `app/core/censo_escritas.py:489`)

2. **Separar atribuição direta de inferência é obrigatório.** Gerente explícito `Modelo.objects...` gera entidade diretamente; amarração a modelo e `self` também podem gerar expressão sem marcador. Inferências aparecem em `expr` como `[campo]`, `[relacionado]`, `[update_fields]`, `[nome]`; não existe flag estruturada de proveniência. Tratar todo `entidade != None` como DIRETA viola esse corte do contrato. (`app/core/censo_escritas.py:377`, `app/core/censo_escritas.py:406`, `app/core/censo_escritas.py:413`, `app/core/censo_escritas.py:420`, `app/core/censo_escritas.py:424`)

3. **Contraexemplo sintético de agrupamento direto correto.** Com `visto_em=agora−60/−30`, ambos ainda não nomeados:
   ```
   DESENHO OBRA-A: pergunta=x app/ponto/registro_batida.py:152
   DESENHO OBRA-B: pergunta=y app/ponto/registro_batida.py:157
   ```
   As duas escritas são `Batida.objects.create(...)`; a entidade correta é Batida para ambas e o contrato exige disparo. Trocar a segunda referência por `app/ponto/portas/celula.py:247` dá CelulaDia e exige ausência de disparo; um programa comum hipotético não muda isso. As escritas e a atribuição direta estão em `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/portas/celula.py:247`, `app/core/censo_escritas.py:377`. Estado e linhas de RELATO acima são sintéticos, **[não verificado]** em produção.

4. **Contraexemplo sintético para DIRETA.** `DESENHO OBRA-A: pergunta=x app/ponto/registro_batida.py:254` referencia `batida.save(update_fields=list(campos))`; o censo a atribui por nome da variável (`[nome]`), pois `update_fields` não é literal. Usar essa entidade como DIRETA pode agrupar indevidamente com outro evento Batida e impedir o fallback para programa; o filtro correto precisa descartar essa proveniência. (`app/ponto/registro_batida.py:254`, `app/core/censo_escritas.py:433`, `app/core/censo_escritas.py:424`) Ocorrência em RELATO/produção **[não verificado]**.

5. **`modelos()` satisfaz literalmente o vocabulário de classes do contrato, com uma limitação já demonstrada pelos testes.** Aceita classe de topo com alguma base em `*/models.py` ou `*/models/*.py`, excluindo nomes terminados em `QuerySet`/`Manager`; não verifica registro Django. Assim inclui `ResolvedoraCruzada(ValueError)`. Duas linhas sintéticas que a citem podem disparar uma entidade que é exceção, não modelo Django; isso decorre do próprio critério literal `modelos()` escolhido no contrato. (`app/core/censo_escritas.py:89`, `app/core/censo_escritas.py:95`, `app/core/censo_escritas.py:108`, `app/chamados/models.py:2179`, `app/core/tests/test_censo_escritas.py:264`, `app/core/tests/test_censo_escritas.py:289`)

6. **Fallback obra → PROGRAMA carece de resolvedor e gramática única.** O BACKLOG contém relações em prosa diferentes: “complemento”, “linha da E4 do PROGRAMA…” e itens com nome do programa. “CALENDARIO-HOJE-MINIMO” diz apenas “1o do programa”; extrair associação só por busca de token `PROGRAMA-...` perde essa ligação. A obra PADRAO-DE-CIMA está registrada como construção futura sobre B3. (`app/docs/BACKLOG.md:771`, `app/docs/BACKLOG.md:772`, `app/docs/BACKLOG.md:773`, `app/docs/BACKLOG.md:774`, `app/docs/BACKLOG.md:787`) Associação em estado de produção **[não verificado]**.

7. **Há validador de vocabulário aproveitável, mas dentro de selo shell, sem API runtime do instrumento.** `vocabulario()` está no heredoc Python de `bin/tests/test_lei_akita.sh`; lê as duas primeiras colunas do GLOSSARIO, negritos do NORTE e nomes em itálico dos ADRs do PLANO. Normaliza caixa, espaços e Markdown, compara termos inteiros, separa alternativas e alguns nomes em parênteses. Sua leitura usa `git show` do commit julgado, não o arquivo vivo; reutilizá-lo sem decidir essa diferença pode rejeitar padrão já editado no arquivo e ainda não commitado. (`bin/tests/test_lei_akita.sh:65`, `bin/tests/test_lei_akita.sh:74`, `bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:80`, `bin/tests/test_lei_akita.sh:89`, `bin/tests/test_lei_akita.sh:102`, `bin/tests/test_lei_akita.sh:112`, `bin/tests/test_lei_akita.sh:138`)

8. **“Nome clássico declarado” requer definição adicional no instrumento.** O validador existente só aceita `VOCAB` ou exceções `n/a`; não tem regra independente para declaração de nome clássico. O texto documental atual também restringe padrão novo a corte do Ronald; o contrato anexo introduz uma alternativa cuja forma de declaração não foi fornecida **[não verificado]**. (`bin/tests/test_lei_akita.sh:120`, `bin/tests/test_lei_akita.sh:124`, `bin/tests/test_lei_akita.sh:138`, `app/docs/PLANO-PADROES.md:46`, `app/docs/PLANO-PADROES.md:53`)

9. **MORDE do censo não prova MORDE do novo instrumento.** Os testes existentes verificam distinção entre escrita direta, inferência e entidade desconhecida; são úteis como suporte do resolvedor. O teste “duas entidades distintas no mesmo programa não dispara” ainda pertence ao novo instrumento, cujo registro está como obra futura. (`app/core/tests/test_censo_escritas.py:25`, `app/core/tests/test_censo_escritas.py:80`, `app/core/tests/test_censo_escritas.py:92`, `app/core/tests/test_censo_escritas.py:110`, `app/docs/BACKLOG.md:787`) Nenhum teste foi executado porque os selos existentes podem criar arquivos; fiz somente leitura e uma consulta Python `-B` em memória **[não verificado]** quanto a execução dos selos.

## 31/10 16:38 · sessao 11a4f092d15a · - · - · origem=Ronald (tmux)

1. **Premissa desatualizada no contrato:** a cópia **já extrai `CODEX_OBRA`**. `_codex_cmds()` lê as atribuições anteriores à invocação por `_atribuicoes()` e retorna `obra`; o PreToolUse usa esse valor para negar a volta do verificador pelo B3. `bin/disjuntor.py:1309`, `bin/disjuntor.py:1325`, `bin/disjuntor.py:1335`, `bin/disjuntor.py:1337`, `bin/disjuntor.py:1440`.

2. **Há dois parsers com contratos distintos.**

   - **Comando Bash/CODEX_OBRA:** aceita qualquer valor textual em atribuição reconhecida, remove aspas por `shlex.split()`, mantém a última atribuição repetida e não aplica regex de nome de obra. Ex.: `CODEX_OBRA="O9 (C)"`, `CODEX_OBRA="qualquer nome / ?"` são extraídos literalmente. `bin/disjuntor.py:1309`, `bin/disjuntor.py:1314`, `bin/disjuntor.py:1318`, `bin/disjuntor.py:1337`.
   - **Ambiente efetivo do wrapper:** `OBRA="${CODEX_OBRA:-}"`, recusando apenas valor vazio; papel deve ser leitura/censo/verificador/sonda. Portanto o wrapper aceita nomes que `_obra_de()` não reconhece. `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:48`.
   - **Prompt de Agent:** `_obra_de()` reconhece `obra X`, `Obra: X`, markdown em torno do nome, ou `leitura ... da X` com até 80 caracteres antes de `da`, escolhendo a primeira ocorrência válida. `bin/disjuntor.py:104`, `bin/disjuntor.py:105`, `bin/disjuntor.py:504`, `bin/disjuntor.py:514`.
   - O ID deve ser `[A-Z][A-Z0-9]*(?:-[A-Za-z0-9]+)+` ou `[A-Z]+\d+[A-Z0-9]*`. Assim aceita `PADRAO-DE-CIMA`, `OBRA-x`, `O9`, `A1B`, mas não `Obra-X`, `ABC`, `O9a`, `A1b`. Busca em `re.I` é seguida por validação sensível à caixa. `bin/disjuntor.py:97`, `bin/disjuntor.py:104`, `bin/disjuntor.py:518`, `bin/disjuntor.py:521`.
   - ID sem hífen preserva cauda `(letra)` ou outro ID com hífen, normalizada para maiúsculas: `O9 (c)` → `O9 (C)`; `O9 NOTURNO-FOO` é identidade completa. ID com hífen ignora essa cauda: `OBRA-X (C)` → `OBRA-X`. `bin/disjuntor.py:99`, `bin/disjuntor.py:518`, `bin/disjuntor.py:526`, `bin/disjuntor.py:529`.
   - Não há borda direita no token: `obra O9_A` vira `O9`, e `obra OBRA-X_Y` vira `OBRA-X`. É risco de colisão se o instrumento receber esses nomes. `bin/disjuntor.py:97`, `bin/disjuntor.py:103`, `bin/disjuntor.py:104`, `bin/disjuntor.py:515`.

3. **Limites concretos do parser de comando.**

   - `env CODEX_OBRA=OBRA-X CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` não é reconhecido: a regex permite atribuições **antes** dos prefixos; depois de `env` espera o caminho do wrapper. `bin/disjuntor.py:1243`, `bin/disjuntor.py:1244`, `bin/disjuntor.py:1245`.
   - `export CODEX_OBRA=OBRA-X; CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` é reconhecido com `obra=''`: o parser usa só atribuições do mesmo trecho da invocação, sem carregar ambiente anterior. `bin/disjuntor.py:1248`, `bin/disjuntor.py:1292`, `bin/disjuntor.py:1327`, `bin/disjuntor.py:1335`, `bin/disjuntor.py:1337`.
   - `CODEX_OBRA="$OBRA"` retorna o literal `$OBRA`, sem expansão de variável; o wrapper recebe o valor expandido pelo shell. Isso permite divergência entre a comparação do PreToolUse e a obra efetiva. `bin/disjuntor.py:1314`, `bin/disjuntor.py:1321`, `bin/codex_pergunta.sh:44`.
   - `bash -c "..."`, `$(...)`, script posterior e `codex exec` direto são resíduos explicitamente declarados pelo próprio código. `bin/disjuntor.py:1240`, `bin/disjuntor.py:1241`.
   - Hoje esses desvios não contornam **B3 pelo wrapper**, pois todo `PAPEL=verificador` consulta `--b3-parada "$OBRA"` depois de aguardar slot e trava da resposta. O instrumento novo precisa entrar também nessa porta efetiva, ou poderá ser contornado por ambiente/export/env. `bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:164`, `bin/codex_pergunta.sh:165`.

4. **Contraexemplos para ação/escopo do contrato.**

   - Estado hipotético PADRAO: entidade `E` disparada, obras dos eventos `{OBRA-X}`; `Agent` construtor com `prompt="construa a obra OBRA-X"` passa hoje pelo ramo B3, que exige `subagent_type=verificador` e leitura final. Falta decisão PADRAO sobre construtor. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`.
   - Mesmo estado; `Agent` verificador de leitura curta passa pelo B3 porque `_eh_final()` exclui leitura de desenho; o contrato PADRAO não restringe verificador à final. `bin/disjuntor.py:114`, `bin/disjuntor.py:499`, `bin/disjuntor.py:1454`.
   - Mesmo estado; `export CODEX_OBRA=OBRA-X; CODEX_PAPEL=verificador ...` não oferece obra ao gate do PreToolUse: `_nega_b3()` só é chamado se `c.get('obra')`. Se PADRAO for implementado apenas nesse ponto, deixará passar a obra disparada. `bin/disjuntor.py:1337`, `bin/disjuntor.py:1442`, `bin/disjuntor.py:1443`.
   - `prompt="Compare obra OBRA-Y antes de construir obra OBRA-X"` resolve `OBRA-Y`; com `{OBRA-X}` disparada deixa passar a execução pretendida de X, com `{OBRA-Y}` disparada pode bloquear por mera referência. A primeira ocorrência manda. `bin/disjuntor.py:504`, `bin/disjuntor.py:510`, `bin/disjuntor.py:514`.

5. **O que quebra ao mudar o parser de nomes.**

   - `_obra_de()` alimenta a identificação dos verificadores finais nos transcripts, o gate de Agent e a classificação do ramo de exceção B3; uma mudança pode alterar tanto contagem quanto bloqueio e NAO_SEI. `bin/disjuntor.py:546`, `bin/disjuntor.py:557`, `bin/disjuntor.py:736`, `bin/disjuntor.py:744`, `bin/disjuntor.py:1415`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1455`.
   - `_OBRA_TOK`, `_obra_lida()` e `_identidade()` são compartilhados com `obras_da_linha()`, que valida a identidade no cabeçalho das linhas de liberação B3. Modificar só uma ponta pode contar uma identidade e liberar outra. `bin/disjuntor.py:100`, `bin/disjuntor.py:103`, `bin/disjuntor.py:518`, `bin/disjuntor.py:526`, `bin/disjuntor.py:533`, `bin/disjuntor.py:637`.
   - Estado e triagem usam a obra como chave literal; trocar normalização pode separar históricos antigos da identidade nova. O Codex mantém a identidade literal de `CODEX_OBRA`, e o Agent aplica `_identidade()`. `bin/disjuntor.py:808`, `bin/disjuntor.py:813`, `bin/disjuntor.py:850`, `bin/disjuntor.py:1013`, `bin/codex_pergunta.sh:199`.
   - Existem testes dessas diferenças: `O9` versus `O9 (C)`, markdown, caixa da cauda, exclusão de `O9a`, hífen sem cauda, concordância contador/topo, última atribuição e atribuição dentro de outro valor. Não foram executados porque os scripts criam arquivos; só exercitei funções puras com `python3 -B`. `bin/tests/test_disjuntor.sh:686`, `bin/tests/test_disjuntor.sh:688`, `bin/tests/test_disjuntor.sh:696`, `bin/tests/test_disjuntor.sh:697`, `bin/tests/test_disjuntor.sh:698`, `bin/tests/test_disjuntor.sh:699`, `bin/tests/test_disjuntor.sh:743`, `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1260`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1262`, `bin/tests/test_disjuntor.sh:1265`.

6. **Escritores/leitores relacionados ao wrapper e seus campos.**

   | Estado/artefato | Escritor | Leitor |
   |---|---|---|
   | `chamadas.log`: hora, obra, papel, hash12, pid, ini, pergunta raspada | `bin/codex_pergunta.sh:88` | `_linhas_chamadas`: `bin/codex_limite_estado.py:49`, `bin/codex_limite_estado.py:55`, `bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:65` |
   | `chamadas.log`: fim, thread_id, rc, duração | `_saida`: `bin/codex_pergunta.sh:56`, `bin/codex_pergunta.sh:59` | `bin/codex_limite_estado.py:66`, `bin/codex_limite_estado.py:67`; últimas chamadas `bin/codex_limite_estado.py:82`; origem Code `bin/codex_limite_estado.py:92` |
   | `limite.log`: hora, obra, mensagem | `bin/codex_pergunta.sh:188` | `bin/codex_limite_estado.py:33`, `bin/codex_limite_estado.py:34` |
   | `<resposta>.id`: obra, papel, hash, pid, fim, sha | `bin/codex_pergunta.sh:198`, `bin/codex_pergunta.sh:199`, `bin/codex_pergunta.sh:201` | `_le_id`: `bin/disjuntor.py:753`, `bin/disjuntor.py:756`, `bin/disjuntor.py:760`; `_triagem_sob_trava`: `bin/disjuntor.py:795`, `bin/disjuntor.py:798`, `bin/disjuntor.py:800`, `bin/disjuntor.py:802` |
   | Trilha de triagem: hora, obra, volta, veredito | `_triagem_sob_trava`: `bin/disjuntor.py:810`, `bin/disjuntor.py:812`, `bin/disjuntor.py:813` | `_triagens`: `bin/disjuntor.py:823`, `bin/disjuntor.py:834`, `bin/disjuntor.py:846`, `bin/disjuntor.py:850` |
   | `disjuntor_estado.json:b3_triagem_linhas` | `_triagem_sob_trava`: `bin/disjuntor.py:815`; `_triagens`: `bin/disjuntor.py:853` | `_triagens`: `bin/disjuntor.py:829`, `bin/disjuntor.py:843` |
   | Campos novos PADRAO | Nenhum escritor apresentado nesses caminhos; definição/integração futura [não verificado] | Nenhum leitor apresentado nesses caminhos; definição/integração futura [não verificado] |

   O ESTADO agrega chamadas por `disjuntor.ler()` → `codex_limite_estado.ler_chamadas()` e publica o texto pelo gerador. `bin/disjuntor.py:1611`, `bin/disjuntor.py:1615`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`.

## 31/10 16:35 · sessao f0bf8853260b · - · - · origem=Ronald (tmux)

Base para as questões 3 e 5, somente por leitura estática.

O juiz efetivo é `cel.trabalha is False or bool(ata.get('tipo_dia') and ata.get('tipo_dia') != 'trabalho')`: `None` e `''` na ata não bastam para calar; `trabalha=None` com ata não vazia diferente de trabalho cala. (`app/chamados/juizes.py:398-408`)

| Sítio | Predicado próprio e universo | Igual ao juiz? |
|---|---|---|
| `cartorio._protesta` | `trabalha is not False AND marcos_acesos(chamado, ata_atual) is not True`; aplicado a `vivos`, `_com_fio`, adiamento e reconciliação. (`app/ponto/services/cartorio.py:1206-1214`, `:1250`, `:1281-1287`) | Não: ignora a segunda cláusula e considera satisfação dos marcos. Para `trabalha=False`, todos os chamados deixam de protestar e `chamados_vivos=0`. (`app/ponto/services/cartorio.py:1207-1214`) |
| `cartorio` emissão total | `ec.eh_dia_trabalho(data) is True`, após código de furo/morta/limbo atual, situação, piso e afastamento. (`app/ponto/services/cartorio.py:1492-1507`) | Não: `eh_dia_trabalho` devolve `cel.trabalha` sem ler ata quando existe célula; sem célula calcula previsão. (`app/escala/models.py:1066-1073`) |
| `cartorio._marcos_do_furo_parcial` | Ausência de vínculo ou `eh_dia_trabalho is not True` impede nomear marcos apagados. (`app/ponto/services/cartorio.py:1604-1625`) | Não, pela mesma distinção entre previsão e ata. (`app/escala/models.py:1071-1073`) |
| `declaracao_texto._marcos_faltantes` | Grade lavrada do dia e véspera; `tipo_dia != 'trabalho'` pula o dia, inclusive indefinido; véspera contribui só saída. (`app/chamados/services/declaracao_texto.py:79-96`, `:116-123`) | Não: não vê `trabalha=False` quando ata diz trabalho; indefinido explícito fica calado; universo inclui véspera. (`app/chamados/services/declaracao_texto.py:88-96`, `app/chamados/juizes.py:408`) |
| `disputa_emissao` identificação do dia | Grade da data da entrada aberta, aceita só `tipo_dia == 'trabalho'`. (`app/chamados/services/disputa_emissao.py:56-70`) | Não: decide pelo tipo projetado da ata, sem primeira cláusula soberana. (`app/chamados/services/disputa_emissao.py:65-68`, `app/escala/services/leitor_celula.py:345-346`) |
| `disputa_emissao._tem_base` | Primeiro aceita batidas do turno; senão vínculo cujo `eh_dia_trabalho is not False`. (`app/chamados/services/disputa_emissao.py:749-753`) | Não: batida vence e previsão sem ata aceita None. (`app/chamados/services/disputa_emissao.py:750-753`, `app/escala/models.py:1071-1073`) |
| `disparar_perguntas_competencia` | Consulta só `trabalha=True` na janela; depois exige ativo e aplica outros filtros de emissão. (`app/chamados/management/commands/disparar_perguntas_competencia.py:74-76`, `:104-110`) | Não: exclui None/trabalho e consulta aceita True/ausência antes dos demais filtros. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/juizes.py:408`) |
| `fila_por_causa` | Dias previstos são células `trabalha=True`, desde `ini`, para colaboradores da fila. (`app/chamados/services/fila_por_causa.py:191-200`) | Não: fonte só trabalha, sem ata. (`app/chamados/services/fila_por_causa.py:192-194`) |
| `vigia_de_hora._candidatas` | `trabalha=True`, empresa, hoje e ontem; ontem ainda passa pela janela do turno. (`app/ponto/services/vigia_de_hora.py:15-26`) | Não: None não chega ao cartório por esta seleção. (`app/ponto/services/vigia_de_hora.py:21-23`) |
| `vigia_entrada` | Só `trabalha=True` e ata com lâmpadas, em janela passada ou data pedida. (`app/ponto/services/vigia_entrada.py:73-85`, `:120-125`) | Não: mesma divergência do filtro True, embora seja instrumento de leitura. (`app/ponto/services/vigia_entrada.py:79`, `:120`) |
| `dia_do_colab._veredito_do_dia` | `not cel.trabalha` produz “Dia sem trabalho previsto”; também compara faixa de precedência com trabalha truthy. (`app/ponto/services/dia_do_colab.py:117-136`) | Não: None é descrito como sem trabalho e ata não decide esse ramo. (`app/ponto/services/dia_do_colab.py:121-123`) |
| `passivo_s84` | Batidas vivas atribuídas ao dia-turno; célula truthy vira TRABALHO, False/None viram FOLGA. (`app/ponto/services/passivo_s84.py:70-96`) | Não: colapsa None em folga; essa classe alimenta seleção de retratação. (`app/ponto/services/passivo_s84.py:96`, `:108-112`) |
| `ponto.marcos` veto | `trabalha is False` dá FOLGA; em seguida considera vereditos vetados. (`app/ponto/marcos.py:155-163`) | Só a primeira cláusula; não ata. (`app/ponto/marcos.py:159-162`) |
| `flip_auto` | Para célula False, recusa folga diurna; no noturno pede véspera True e recua ao dia anterior. (`app/ponto/services/flip_auto.py:201-215`) | Não: problema geométrico com célula/véspera, sem ata tipo. (`app/ponto/services/flip_auto.py:208-215`) |
| `worklist.linha_da_ata` | Grade lavrada de um dia: `tipo_dia != trabalho` devolve gap indeterminado; só então lê lâmpadas. (`app/ponto/worklist.py:18-26`) | Não: tipo da ata sem cláusula trabalha. (`app/ponto/worklist.py:19-22`) |
| `bordas_realizado` | Grade lavrada dos vínculos ativos, só `tipo_dia == trabalho` em intervalo. (`app/ponto/services/bordas_realizado.py:62-75`) | Não; universo de auditoria do realizado, não emissão de cobrança. (`app/ponto/services/bordas_realizado.py:76-87`) |
| `supra_juiz` outros ramos | Mantém tipo trabalho nos ramos `:281`, `:402`, `:419`, `:443`, `:476`, `:491`; dia de cobrança inclui ausência parcial em `:304`. (`app/ponto/supra_juiz.py:281`, `:304-308`, `:402-420`, `:443`, `:476-491`) | Não são todos a pergunta migrada; ausência parcial é explicitamente cobrável ali, enquanto juiz cala ata ausência. (`app/ponto/supra_juiz.py:299-305`, `app/chamados/juizes.py:408`) |

Outros instrumentos sobre células também conservam recortes próprios: auditor de chamados `not trabalha`/`is not True` para invariantes, esmeril `is False` para A8/A2 e truthy para A4, tripwire filtro True para DNA/ata e filtro False/None mais ata folga para “folga mentindo”, sondas `dna_x_batida_real` e `plano_b_no_dinheiro` filtro True. São universos de diagnóstico, não equivalentes ao juiz de cobrança. (`app/chamados/management/commands/auditar_invariantes_chamados.py:376`, `:1443`, `:1542`; `app/ponto/services/esmeril_espelho.py:71-75`, `:121-124`; `app/ponto/management/commands/tripwire_celulas.py:96-98`, `:256-264`, `:338-344`; `app/ponto/management/commands/dna_x_batida_real.py:58-65`; `app/ponto/management/commands/plano_b_no_dinheiro.py:142-144`)

Há leitores da autoridade **prevista** que não devem ser apresentados como segundo juiz da ata: alerta avançado pula previsão falsey de ontem; retroativo exige previsão True; detector de anomalias devolve ok para previsão None e alerta folga para False; faltas de hoje aceita previsão True ou defeito de folga; plantio overnight acusa previsão False. Eles discordam do juiz de cobrança em células com ata contrária ou em None, mas o método chamado é a autoridade prevista registrada. (`app/ponto/management/commands/processar_alertas_avancados.py:34-50`; `app/ponto/management/commands/emitir_furo_retroativo.py:117-129`; `app/chamados/detector_anomalias.py:164-170`; `app/ponto/services/faltas_de_hoje.py:32-42`; `app/ponto/management/commands/vigia_plantio_overnight.py:38-50`; `app/escala/models.py:1048-1051`, `:1071-1073`)

`detectar_ausencias` também mantém tratamento próprio do limbo de previsão e defeito cadastral nas passadas de hoje/ontem; não resume sua regra a dia sem trabalho lavrado. (`app/ponto/management/commands/detectar_ausencias.py:136-159`)

`propositor.dia_degenerado` e `template_suspeito` exigem tipo trabalho, mas recebem dicts de diferentes montadores; `simular_dia.dia_perfeito` e o gate de `veredito_celula` leem uma grade simulada remontada, portanto não são leitura exclusiva da ata persistida. (`app/ponto/propositor.py:34-37`, `:201-211`; `app/ponto/services/simular_dia.py:5-12`; `app/ponto/services/veredito_celula.py:216-222`)

Conferência documental:

- A exceção `_protesta` está declarada e corresponde à omissão da segunda cláusula; “zera chamados_vivos” vale quando trabalha é False, não incondicionalmente. Há também o veto independente `marcos_acesos`. (`app/docs/BACKLOG.md:807`; `app/core/juizes.py:144-146`; `app/docs/JUIZES.md:60-61`; `app/ponto/services/cartorio.py:1206-1214`)
- A primeira cláusula do juiz realmente não é alcançada pelo cartório normal com trabalha False, porque `chamados_vivos=0`; nunca bateu já passa zero explicitamente. (`app/ponto/services/cartorio.py:1029`, `:1207-1214`; `app/ponto/supra_juiz.py:470-472`)
- “Ata DESTE julgamento” está correto nos dois ramos: `_ata_nb` recém-montada e `_ata_agora.ata` recém-montada; a segunda fica em `_tl.atas` antes da chamada. (`app/docs/CORTES.md:415`; `app/ponto/services/cartorio.py:1012`, `:1029`, `:1184-1188`, `:1213`)
- “Célula JA LAVRADA” de `_C18` exige ressalva para o cartório: esse leitor usa lavra em curso antes da persistência final. (`app/core/juizes.py:137`; `app/ponto/services/cartorio.py:1184-1188`, `:1213`, `:1563-1565`)
- A fórmula textual `ata.tipo_dia != trabalho` omite a guarda de valor verdadeiro: None/'' não calam para trabalha True/None. (`app/docs/CORTES.md:415`; `app/docs/JUIZES.md:56`; `app/core/juizes.py:138`; `app/chamados/juizes.py:408`)
- “trabalha=None COBRA” é condicional: ata folga/ausência/falta/indefinido cala mesmo com None. (`app/docs/JUIZES.md:57`; `app/core/juizes.py:139`; `app/chamados/juizes.py:408`)
- O fechamento SUPRA-DELEGA descreve corretamente a chamada ao juiz nesse ramo, mantendo a exceção SEM_JURISDICAO. A divergência para ata explícita indefinido continua existindo pela guarda autorizada: o juiz isolado diria sem trabalho, mas supra nem o chama. (`app/docs/BACKLOG.md:806`; `app/docs/CORTES.md:415`; `app/ponto/supra_juiz.py:48`, `:470-472`; `app/chamados/juizes.py:408`)
- Os números de produção citados em BACKLOG permanecem declarações documentais; sem banco/log não os confirmei. [não verificado]
- BACKLOG NONE-LEITORES conserva endereços antigos do cartório; os gates equivalentes atuais estão em `1504` e `1608`, não nos quatro números registrados. (`app/docs/BACKLOG.md:805`; `app/ponto/services/cartorio.py:1504`, `:1608`)
- A fila já admite outros predicados próprios e ausência parcial divergente, portanto a documentação não sustenta “todos os leitores de chamados/ponto migraram”. (`app/docs/BACKLOG.md:823`)

Não identifiquei defeito adicional comprovado no delta além dessas limitações/documentação; os outros predicados acima são majoritariamente preexistentes e têm universos diferentes. [não verificado]

## 31/10 16:34 · sessao cdc88f1201ca · - · - · origem=Ronald (tmux)

Escritores e leitores confirmados, sem acesso a banco/log nem alterações:

| Valor | Escritor / porta | Leitores e autoridade |
|---|---|---|
| `CelulaDia.trabalha` | Geração calcula `ec.eh_dia_trabalho_calculado(d)` e cria/atualiza pela porta; regeneração também, inclusive limpa para `False` fora de vínculo; reversão deriva do DNA anterior. `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:532`, `app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:612`. | Juiz lê `is False`, mais tipo verdadeiro diferente de trabalho na ata: `app/chamados/juizes.py:407`. Fábrica delega em `app/chamados/services/fabrica_da_celula.py:80`; materialização em `app/chamados/services/materializacao.py:160`; cluster em `app/ponto/services/cluster_espurio.py:28`; intervalo em `app/ponto/management/commands/detectar_intervalo_ausente.py:51`. |
| `ata.tipo_dia` | `ata_do_dia` copia do dict da grade em `app/ponto/services/cartorio.py:568`; `CelulaDia.lavrar_veredito` apenas delega em `app/escala/models.py:1794`; persistência na porta em `app/ponto/portas/celula.py:726`, `app/ponto/portas/celula.py:768`. | Juiz em `app/chamados/juizes.py:408`; grade lê e tem fallback próprio em `app/escala/services/leitor_celula.py:345`; declaração tem veto próprio `tipo_dia != 'trabalho'` em `app/chamados/services/declaracao_texto.py:95`. |
| `ata.minutos_previstos` | Cartório converte `int(dia.get(...) or 0)` em `app/ponto/services/cartorio.py:569`; mesma porta de persistência acima. | Grade lê em `app/escala/services/leitor_celula.py:347`; sem número, consulta minutos do dia apenas se `cel.trabalha` for verdadeiro, senão põe zero em `app/escala/services/leitor_celula.py:354`; supra usa o valor projetado em `app/ponto/supra_juiz.py:223`. Nenhum desses cálculos consulta `dia_sem_trabalho`. |
| `ata.lampadas` | `lampadas_do_dia` monta os itens em `app/ponto/services/cartorio.py:540`, `app/ponto/services/cartorio.py:552`; `ata_do_dia` inclui em `app/ponto/services/cartorio.py:566`; ausência/justificativa dispensa luzes em `app/ponto/services/cartorio.py:611`; teto transforma apagada em `None` em `app/ponto/services/cartorio.py:707`. | Materialização lê slots/luzes em `app/chamados/services/materializacao.py:158`, `app/chamados/services/materializacao.py:164`; cluster conta apagadas em `app/ponto/services/cluster_espurio.py:31`; intervalo indexa em `app/ponto/management/commands/detectar_intervalo_ausente.py:64`; grade projeta células em `app/escala/services/leitor_celula.py:92`; Pauta informa apagadas em `app/pautas/services.py:422`. São leituras da lâmpada para outras perguntas; os quatro emissores já delegaram o gate de dia sem trabalho nos locais acima. |
| `INSUMO_DIA_SEM_TRABALHO` | Constante em `app/ponto/supra_juiz.py:57`; adaptador escreve `{'trabalha':..., 'ata':...}` em `app/ponto/supra_juiz.py:62`; grade escreve em `app/escala/services/leitor_celula.py:370`; cartório injeta ata atual em `app/ponto/services/cartorio.py:1029`, `app/ponto/services/cartorio.py:1213`. | Único leitor funcional é `_dia_sem_trabalho_pelo_juiz`: acesso obrigatório e delegação, sem predicado paralelo, `app/ponto/supra_juiz.py:65`. |
| `Pauta.texto/ancora_id` novos | Fábrica constrói âncora `colab:data`, marcador e tags em `app/chamados/services/fabrica_da_celula.py:175`, `app/chamados/services/fabrica_da_celula.py:189`; escreve pela porta em `app/chamados/services/fabrica_da_celula.py:185`; porta normaliza/corta texto e grava em `app/pautas/services.py:163`, `app/pautas/services.py:180`. | Dedup relê ambas com filtro por cabeça/DP/marcador e procura tags em `app/chamados/services/fabrica_da_celula.py:180`; API expõe em `app/pautas/views.py:40`; painel mostra texto em `app/templates/core/_copiloto.html:405`; chip consulta âncora em `app/pautas/services.py:334`; contexto parseia âncora em `app/pautas/services.py:381`. Não respondem “dia sem trabalho”; contexto usa `trabalha is False` para rótulo `folga`, sem juiz, em `app/pautas/services.py:418`. |

Exceções a “um escritor da ata”: `carimbar_precedencia` escreve outra chave da mesma ata pela mesma porta em `app/ponto/portas/celula.py:811`; projeções em memória copiam ata sem salvar em `app/ponto/portas/celula.py:667` e `app/escala/utils.py:1826`. Os testes novos escrevem `trabalha/ata` diretamente para formar fixture em `app/chamados/tests/test_f1c4_3_emissores.py:65`.

A fábrica delega primeiro ao juiz, mas seu caminho efetivo ainda passa pelo veto próprio da declaração: `motivos_pela_ata` chama `_ata_do_dia` em `app/chamados/services/fabrica_da_celula.py:49`; a leitura da declaração rejeita qualquer tipo projetado diferente de `trabalho` em `app/chamados/services/declaracao_texto.py:95`. A grade também mantém projeção própria de tipo e de minutos em `app/escala/services/leitor_celula.py:345`.

Defeito adicional inferido: dedup nova de Pauta não é segura sob concorrência. O SELECT da fábrica acontece antes da transação de `escrever`; dois julgamentos podem obter `ja` vazio e ambos criar Pauta: `app/chamados/services/fabrica_da_celula.py:180`, `app/chamados/services/fabrica_da_celula.py:185`, `app/pautas/services.py:152`, `app/pautas/services.py:180`. Não há unicidade por dia/motivo no modelo, apenas índice de âncora e check de destinatário: `app/pautas/models.py:113`, `app/pautas/models.py:123`. Os caminhos lidos não serializam esse trecho: batch chama diretamente em `app/ponto/management/commands/processar_cartorio.py:59`; evento lê célula sem trava em `app/ponto/services/cartorio.py:1708` e chama o mesmo julgamento em `app/ponto/services/cartorio.py:1721`; fábrica roda após o corpo em `app/ponto/services/cartorio.py:824`. `em_julgamento` é thread-local, não trava entre processos: `app/ponto/services/cartorio.py:34`, `app/ponto/services/cartorio.py:770`. Duplicação efetivamente ocorrida em produção e eventual serialização externa: [não verificado].

Limitação já declarada do produto: âncora `dia` não abre objeto no painel; ROTA contém só colab/chamado/posto e dá toast para dia em `app/templates/core/_copiloto.html:564`. O link do calendário está no fim do contexto em `app/pautas/services.py:426`, podendo cair com o corte novo de contexto em `app/pautas/services.py:177`.

## 31/10 16:34 · sessao e08f301f9bb6 · - · - · origem=Ronald (tmux)

**Q2 — produtores e risco de `KeyError`.**

| Caminho | Dict e ata entregues | Insumo ausente? |
|---|---|---|
| Command `supra_juiz` | Usa `grade_da_celula`; esta lê `cel.ata` já gravada e põe `cel.trabalha` + essa ata no insumo. Referências: `app/ponto/management/commands/supra_juiz.py:116`, `:206`; `app/escala/services/leitor_celula.py:332`, `:370`. | Dias `fora_vinculo` e `sem_celula` são produzidos sem insumo, mas ficam barrados por `SEM_JURISDICAO` antes da chamada ao juiz. Referências: `app/escala/services/leitor_celula.py:340`, `:343`; `app/ponto/supra_juiz.py:470`. |
| Command `emitir_furo_retroativo` | Mesmo produtor e mesma ata já gravada. Referências: `app/ponto/management/commands/emitir_furo_retroativo.py:59`, `:99`; `app/escala/services/leitor_celula.py:332`, `:370`. | Mesma proteção para os dois dias sem insumo. Referências: `app/escala/services/leitor_celula.py:340`, `:343`; `app/ponto/supra_juiz.py:470`. |
| Cartório, `nunca_bateu` | Monta `_ata_nb` neste julgamento e entrega exatamente essa variável ao helper, antes da lavra. Referências: `app/ponto/services/cartorio.py:1012`, `:1029`, `:1045`. | Helper sempre acrescenta a chave; neste ramo `chamados_vivos=0` também impede alcançar a leitura do insumo. Referências: `app/ponto/supra_juiz.py:62`, `:471`; `app/ponto/services/cartorio.py:1029`. |
| Cartório, normal | Monta `_ata_agora.ata`, publica-a em `_tl.atas` e passa essa ata ao helper. Referências: `app/ponto/services/cartorio.py:1184`, `:1188`, `:1213`. | Helper acrescenta a chave antes da classificação. Referências: `app/ponto/supra_juiz.py:62`; `app/ponto/services/cartorio.py:1213`. |
| Evento `julgar_celula` | Não monta um quinto produtor: carrega a célula e chama `julgar_colab`, chegando aos mesmos dois ramos. Referências: `app/ponto/services/cartorio.py:1708`, `:1721`, `:777`. | Mesmas garantias dos dois ramos; célula inexistente retorna antes. Referências: `app/ponto/services/cartorio.py:1709`, `:1721`. |

Na varredura dos chamadores de produção, os únicos sítios de chamada ao classificador do supra são os dois commands e os dois ramos do cartório; os usos em `relatorios/furos.py` e `inteligencia/calculadores.py` pertencem a outra `classificar_dia`. Referências: `app/ponto/management/commands/supra_juiz.py:206`; `app/ponto/management/commands/emitir_furo_retroativo.py:99`; `app/ponto/services/cartorio.py:1028`, `:1212`; `app/relatorios/furos.py:55`; `app/inteligencia/calculadores.py:26`.

Não encontrei nesses caminhos um dict sem insumo que alcance a leitura obrigatória; afirmar ocorrência ou ausência real de `KeyError` em produção permanece **[não verificado]**. A leitura obrigatória lança se a chave faltar e só acontece depois de `tipo_dia not in SEM_JURISDICAO`, previsto falso, chamados vivos e cobertura exatamente `None`. Referências: `app/ponto/supra_juiz.py:70`, `:470`.

Os testes também produzem dicts sem insumo deliberadamente: um exige `KeyError`, outro verifica a guarda `SEM_JURISDICAO`; testes antigos sem chamado ou com cobertura aprovada continuam barrados antes da leitura. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:136`, `:141`; `app/ponto/tests/test_supra_juiz_cobranca.py:107`, `:111`; `app/ponto/supra_juiz.py:471`.

**Q4 — os dois testes novos.** [nao verificado]

| Teste | Mutação mínima no cartório que o torna vermelho, por leitura estática | [nao verificado]
|---|---|
| `test_MORDE_ramo_nunca_bateu_entrega_a_ata_deste_julgamento` | Em `app/ponto/services/cartorio.py:1029`, substituir `ata=_ata_nb` por `ata=cel.ata`: o espião recebe a ata semeada com a sentinela e `tipo_dia='folga'`, contrariando as asserções. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100`, `:109`, `:111`, `:116`. |
| `test_MORDE_ramo_normal_entrega_a_ata_deste_julgamento` | Em `app/ponto/services/cartorio.py:1213`, substituir `ata=_ata_agora.ata` por `ata=cel.ata`: falha pelas mesmas asserções e também pela identidade com `_tl.atas`. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100`, `:109`, `:111`, `:132`. |

Nenhum passa se o espião nunca for chamado: `_conferir` começa exigindo `vistos` não vazio. Referência: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:107`.

Nenhum passa recebendo a ata anterior intacta, pois ela contém sentinela e `tipo_dia='folga'`; ambos exigem ausência da sentinela e tipo `'trabalho'`. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100`, `:109`, `:111`.

Não são fixtures vazias quanto ao **transporte da ata**: exigem chamada observada, conteúdo esperado e que a lavra substitua a ata anterior; o teste normal exige ainda identidade do objeto com a ata em curso. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:107`, `:111`, `:112`, `:132`.

Entretanto, **não provam execução do ramo `COBRANCA_EM_DIA_SEM_TRABALHO` nem chamada efetiva a `dia_sem_trabalho`**: o ramo `nunca_bateu` passa `chamados_vivos=0`; a fixture normal começa com célula de trabalho e jornada prevista, sem criar chamado no cadastro. Referências: `app/ponto/services/cartorio.py:1029`; `app/ponto/supra_juiz.py:471`; `app/ponto/tests/test_bo_inss_nunca_bateu_le_a_precedencia.py:72`, `:80`, `:89`.

Há uma diferença de força entre os testes: o normal exige identidade com a ata em curso; o `nunca_bateu` só compara sentinela e `tipo_dia`, então uma ata sintética `{'tipo_dia': 'trabalho'}` pode satisfazer suas asserções mesmo perdendo outros campos. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:106`, `:113`, `:132`.

Execução dos testes e dos mutantes completos: **[não verificado]**; as conclusões acima são estáticas, sem banco e sem escrita. [nao verificado]

## 31/10 16:28 · sessao ffcc8c5e890a · E4-COMBOIO · verificador · origem=Code

Há achados procedentes no entorno e provas mais estreitas que as promessas dos selos: atividade crua sobrevivente no painel, rótulo `previsto` invisível no HTML e escritor de `Batida` fora da porta. — `app/core/services/painel_op.py:239`; `app/templates/escala/minha_escala.html:76`; `app/chamados/services/regularizacao_ext.py:153`.

Execução dos TestCase Django, RED/GREEN histórico, correspondência com a base `5693e462` e impacto em produção: **[nao verificado]**. Os números “11” e “4732” aparecem nas docstrings; sua confirmação por registros de produção permanece **[nao verificado]**. — `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:5`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:4`.

**1. Escritores e leitores dos campos/valores**

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `empresa_id`, `filtro_status` do painel | GET das views, passado ao construtor. — `app/core/views.py:607`; `app/core/views.py:626` | Recorte de colaboradores/postos e filtro das linhas. — `app/core/services/painel_op.py:41`; `app/core/services/painel_op.py:179`; `app/core/services/painel_op.py:246` |
| `agora`, `hoje`, `_14h_atras` | `timezone.now()`, conversão local e subtração de **24h**. — `app/core/services/painel_op.py:32`; `app/core/services/painel_op.py:46` | Consulta de entradas, candidatos, juiz de turno, previsão e duração. — `app/core/services/painel_op.py:53`; `app/core/services/painel_op.py:60`; `app/core/services/painel_op.py:65`; `app/core/services/painel_op.py:125`; `app/core/services/painel_op.py:152` |
| `Batida.colaborador`, `timestamp`, `tipo`, coordenadas | Criação pela porta `registrar_batida`, com sanitização geo; **há também correção direta de `tipo/timestamp` na regularização externa**. — `app/ponto/registro_batida.py:105`; `app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:157`; `app/chamados/services/regularizacao_ext.py:153` | Helper recorta datas/retratação; painel seleciona E, chaveia pelo colaborador e publica hora/coordenadas. — `app/ponto/turnos.py:2845`; `app/core/services/painel_op.py:51`; `app/core/services/painel_op.py:188`; `app/core/services/painel_op.py:195` |
| `Batida.retratada_em` | `retratar_batida → anotar`; desfazer humano escreve `None`; fixture E4-7 usa `update` direto. — `app/ponto/registro_batida.py:252`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:57` | `batidas_apuraveis`, candidatos do painel e carregamento do juiz. — `app/ponto/turnos.py:2845`; `app/core/services/painel_op.py:61`; `app/ponto/turnos.py:2192` |
| `entradas_hoje` | Dict local: última E, na ordem de timestamp, por colaborador. — `app/core/services/painel_op.py:51`; `app/ponto/turnos.py:2852` | Lista de IDs para pings e seleção de `entrada` por linha. — `app/core/services/painel_op.py:102`; `app/core/services/painel_op.py:143` |
| `entrada → status` | Sem turno vivo, entrada existente produz `encerrado`; trabalho sem entrada produz `ausente`; restante produz `folga`. — `app/core/services/painel_op.py:169` | Filtro, contadores, quantidade de ausentes e ordenação dos colaboradores. — `app/core/services/painel_op.py:179`; `app/core/services/painel_op.py:202`; `app/core/services/painel_op.py:254`; `app/core/services/painel_op.py:263` |
| `entrada`, `entrada_id` | Timestamp local e PK da entrada selecionada. — `app/core/services/painel_op.py:188` | Card mostra a hora; ID abre detalhe geo por HTMX; view do detalhe reabre a Batida por PK. — `app/templates/core/_painel_card.html:64`; `app/templates/core/_painel_card.html:68`; `app/ponto/views.py:3236` |
| `presenca_geo` | `classificar_presenca_turno(entrada, pings)`; pings carregados para as entradas selecionadas. — `app/core/services/painel_op.py:102`; `app/core/services/painel_op.py:183`; `app/ponto/presenca.py:50` | Classe e título da bolinha geo. — `app/templates/core/_painel_card.html:68` |
| `data_turno`, `lat`, `lng` | Derivados do turno vivo/entrada e das coordenadas da entrada. — `app/core/services/painel_op.py:190`; `app/core/services/painel_op.py:195` | `data_turno` é conferido por testes antigos; consumidor produtivo desses três campos do retorno: **[nao verificado]**. — `app/core/tests/test_painel_op_turno_aberto_ancora.py:50`; `app/core/tests/test_painel_op_turno_pelo_juiz.py:80` |
| `bateu_hoje_ids → _teve_ativ → cobertura` | **Consulta independente**, crua, de qualquer Batida na data local; não deriva de `entradas_hoje`. — `app/core/services/painel_op.py:239`; `app/core/services/painel_op.py:258` | `classificar_cobertura`, mapa do painel, agregação e ordenação dos postos. — `app/core/services/painel_op.py:259`; `app/core/services/painel_op.py:273` |
| Linhas, contadores e postos do painel | Construtor. — `app/core/services/painel_op.py:185`; `app/core/services/painel_op.py:202`; `app/core/services/painel_op.py:261` | Página, fragmento e lote; card/stats; JS lê atributos de cobertura/geofence/aberto/ausente. — `app/core/views.py:607`; `app/core/views.py:632`; `app/core/views.py:679`; `app/templates/core/_painel_card.html:2`; `app/templates/core/_painel_stats.html:32`; `app/templates/core/painel_operacional.html:177` |

Para `minha_escala` e o medidor:

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| Vínculo selecionado, `tipo_ciclo`, âncora | View escolhe o primeiro vínculo ativo e lê seu cadastro. — `app/escala/views.py:17`; `app/escala/views.py:25`; `app/escala/views.py:32` | Cálculo de `ancora_ausente`, previsão e cabeçalho da tela. — `app/escala/views.py:32`; `app/escala/views.py:40`; `app/templates/escala/minha_escala.html:24`; `app/templates/escala/minha_escala.html:49` |
| `CelulaDia.trabalha`, `dna.marcos` | Porta gera/regera a célula a partir do vínculo e DNA. — `app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:260` | `EC.eh_dia_trabalho` lê `trabalha`, com fallback calculado; `EC.marcos_do_dia` lê DNA, com fallback no template. — `app/escala/models.py:1069`; `app/escala/models.py:1072`; `app/escala/models.py:1099`; `app/escala/models.py:1108` |
| `hoje`, `dia`, `trabalho`, horários | View lê `date.today()`, gera 14 datas, consulta os dois métodos do vínculo. — `app/escala/views.py:26`; `app/escala/views.py:38`; `app/escala/views.py:40`; `app/escala/views.py:43` | Ternário `_st`, contexto e template. — `app/escala/views.py:54`; `app/escala/views.py:55`; `app/templates/escala/minha_escala.html:63`; `app/templates/escala/minha_escala.html:78` |
| `_st`: `folga/escala_suspeita/previsto` | Ternário local; não grava estado persistido. — `app/escala/views.py:54`; `app/escala/views.py:55` | Indexação de `ROTULO_STATUS`; detector AST examina esses literais. — `app/escala/views.py:60`; `app/core/medida_leitor_do_dia.py:340` |
| `ROTULO_STATUS` | Dicionário canônico em `ponto/status_dia.py`, reexportado pelo calendário. — `app/ponto/status_dia.py:28`; `app/colaboradores/services/calendario.py:45` | View, testes e leitura AST do vocabulário. — `app/escala/views.py:36`; `app/escala/views.py:60`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:56`; `app/core/medida_leitor_do_dia.py:159` |
| `dias[].status_rotulo` | View publica **a frase**, sem publicar a chave `_st`. — `app/escala/views.py:55`; `app/escala/views.py:60` | Template somente nos ramos dúvida/folga; testes leem também o contexto de trabalho. — `app/templates/escala/minha_escala.html:75`; `app/templates/escala/minha_escala.html:84`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:53` |
| `CATEGORIAS`, `PALAVRAS_DA_PREVISAO`, `NAO_E_LEITOR`, teto, `DECLARADOS` | Constantes do medidor; previsão tem as três palavras e uma entrada nominal para `minha_escala`. — `app/core/medida_leitor_do_dia.py:259`; `app/core/medida_leitor_do_dia.py:262`; `app/core/medida_leitor_do_dia.py:285`; `app/core/medida_leitor_do_dia.py:289`; `app/core/medida_leitor_do_dia.py:296` | `censo` usa exclusões, categoria, palavras e declarados; **categorias válidas/teto são cobrados pelos testes**, não por um guard de execução do censo. — `app/core/medida_leitor_do_dia.py:348`; `app/core/medida_leitor_do_dia.py:358`; `app/core/tests/test_medida_leitor_do_dia.py:73` |
| `chave/metade/linhas/palavras/sitios/categoria/motivo` | Detector e montagem dos registros do censo. — `app/core/medida_leitor_do_dia.py:221`; `app/core/medida_leitor_do_dia.py:346`; `app/core/medida_leitor_do_dia.py:351` | Contagem, novos/sumidos/exclusões mortas, testes e publicação no ESTADO. — `app/core/medida_leitor_do_dia.py:368`; `app/core/tests/test_medida_leitor_do_dia.py:39`; `bin/leitor_dia_estado.py:46`; `bin/leitor_dia_estado.py:51` |

`bin/leitor_dia_estado.py` carrega o mesmo módulo e chama `censo`; `gerar_estado` usa esse leitor. O selo S3 importa o detector de **valor**, sem duplicá-lo. — `bin/leitor_dia_estado.py:32`; `bin/leitor_dia_estado.py:35`; `bin/gerar_estado.py:290`; `app/ponto/tests/test_s3_placar_exercicio.py:38`.

Para o E4-1:

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `CelulaDia.status_dia/status_rotulo` | Produção: aplicação do status dentro da porta de lavratura; fixture injeta ambos por `update`. — `app/ponto/portas/celula.py:701`; `app/ponto/portas/celula.py:709`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:52`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:67` | Leitor D4, controle de preenchimento do cartório e consulta/assert do teste; o service atual decide pelos fatos. — `app/ponto/status_dia.py:171`; `app/ponto/status_dia.py:181`; `app/ponto/services/cartorio.py:726`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:80`; `app/chamados/services/feriado_reavaliar.py:33` |
| `FolgaDia.escala_colaborador/data/calendario_origem` | Produção materializa calendário/cria e reparenta folga manual; fixture cria calendário no velho ou manual no novo. — `app/escala/folgas.py:173`; `app/escala/folgas.py:259`; `app/escala/folgas.py:405`; `app/escala/folgas.py:435`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:88`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:104` | Precedência lê por pessoa/data e descarta calendário de outro vínculo; overlay da grade lê por vínculo. — `app/ponto/precedencia.py:236`; `app/ponto/precedencia.py:313`; `app/escala/utils.py:1109` |
| `faixa`, `era_dia_previsto`, `fatos.folga_lancada` | `fatos_do_dia → fatos_do_periodo`. — `app/ponto/precedencia.py:209`; `app/ponto/precedencia.py:355` | Seleção de espúrios e diagnóstico/asserts da fixture. — `app/chamados/services/feriado_reavaliar.py:33`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:73`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:94` |
| `modulo_origem`, `subcategoria`, `contexto_json.data_turno` | Fixture cria `batida_ausente/atraso/data_turno`; catálogo declara a chave do dia. — `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:59`; `app/chamados/catalogo/modulos.py:105` | Universo, juiz do dia do fato e premissa. — `app/chamados/services/feriado_reavaliar.py:17`; `app/chamados/services/feriado_reavaliar.py:30`; `app/chamados/services/feriado_reavaliar.py:38`; `app/chamados/juizes.py:104` |
| `FATO_FALTA_DESCOBERTA=False` | `espurios` fornece ao catálogo após consultar o juiz. — `app/chamados/services/feriado_reavaliar.py:36` | `premissa_morta → avaliar`. — `app/chamados/catalogo/premissa.py:144` |
| `Chamado.status_local` | Fixture nasce aberto; fechamento produtivo usa `reconciliar → _gravar_estado`. — `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:58`; `app/chamados/models.py:615`; `app/chamados/models.py:618` | Universo VIVOS, motor/gates e asserts aberto/resolvido. — `app/chamados/services/feriado_reavaliar.py:17`; `app/chamados/models.py:637`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:97`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:110` |

Os cadastros auxiliares das fixtures — Empresa/Praca/Posto/TipoEscala/Colaborador/vínculos/usuário — são escritos nos respectivos `setUp`/`setUpTestData`; sustentam os seletores, ciclos e relações acima. Consumidores adicionais de cada identificador auxiliar, como CNPJ/nome de fixture: **[nao verificado]**. — `app/core/tests/test_e4_7_painel_entrada_retratada.py:35`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:43`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:23`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:32`.

**2. Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz | **Procedente no entorno:** a grade ainda transforma FolgaDia em folga sem verificar a titularidade pelo vínculo do dia, enquanto a precedência descarta calendário de outro vínculo. O próprio RELATO nomeia essa divergência. — `app/escala/utils.py:1109`; `app/escala/utils.py:1118`; `app/ponto/precedencia.py:307`; `app/ponto/precedencia.py:313`; `app/docs/RELATO.md:63` |
| Segundo escritor fora da porta | **Procedente, anterior ao diff:** `alvo` é FK de Batida; regularização externa grava `tipo/timestamp` por `save` direto e remove por `delete`. O caminho é alcançável pela confirmação web e APIs. — `app/chamados/models.py:3098`; `app/chamados/services/regularizacao_ext.py:127`; `app/chamados/services/regularizacao_ext.py:136`; `app/chamados/services/regularizacao_ext.py:153`; `app/chamados/services/regularizacao_ext.py:356`; `app/chamados/views_regularizacao.py:216`; `app/api/views.py:3061`; `app/api/views_core.py:1327` |
| Selo cego ao escritor | **Procedente:** o censo puro classificou essas escritas por `alvo` sem entidade resolvida; o gate ignora `entidade=None`. Portanto seu verde não prova escritor único nesse caminho. — `app/core/censo_escritas.py:43`; `app/core/censo_escritas.py:469`; `app/ponto/tests/test_chokepoint_batida_gate.py:29` |
| Relógio fora da borda | E4-7 não acrescenta relógio: helper recebe a janela e painel passa `agora` ao juiz. A view mantém `date.today()`; congelar testes não altera essa leitura real. **Dúvida concreta no selo E4-1:** setup alcança cartório com relógio real, sem freeze, e o cartório ignora datas futuras. — `app/core/services/painel_op.py:32`; `app/core/services/painel_op.py:65`; `app/ponto/turnos.py:2844`; `app/escala/views.py:26`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:26`; `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1691`; `app/ponto/services/cartorio.py:1700` |
| Derivação paralela | **Procedente, dívida mantida:** painel ainda escolhe status próprio e permanece declarado no medidor. E4-7 corrige sua entrada, sem concluir essa migração. — `app/core/services/painel_op.py:148`; `app/core/services/painel_op.py:169`; `app/core/medida_leitor_do_dia.py:312` |
| Exclusão sem predicado | A exclusão nova **tem predicado lexical**, mas não comprova futuro nem consulta à grade: testa apenas o subconjunto de palavras detectadas. — `app/core/medida_leitor_do_dia.py:348` |
| Pouso antes do pré-requisito | **Dúvida documental:** RELATO manda E4-1 esperar cura da grade e sombra com 0044; cumprimento: **[nao verificado]**. A saída ii fornecida é caracterização, e o cron permanece na lista de juízes; o diff não demonstra rebaixamento prematuro. — `app/docs/RELATO.md:63`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:1`; `app/config/crons.py:1983` |

**Mordida dos selos:**

- **E4-7, estática procedente:** restaurar a consulta crua faz A recuperar sua única entrada retratada e C escolher 07:30; isso contradiz os asserts. Zerar todas as entradas contradiz B. O teste não afirma atividade/cobertura, apesar da promessa da docstring. Execução: **[nao verificado]**. — `app/core/tests/test_e4_7_painel_entrada_retratada.py:3`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:64`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:69`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:78`.
- **E4-6:** o teste de contexto rejeita `previsto → ok`; o teste HTML das duas cascas não rejeita essa troca por si só, pois o ramo trabalho não imprime o rótulo. — `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:53`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:66`; `app/templates/escala/minha_escala.html:76`.
- **Medidor:** censo executado em memória contou **6 status, 0 valor**, sem novos/sumidos/sítios alterados/exclusões mortas/ilegíveis; a mutação literal para `ok` produziu **7** e recolocou `minha_escala` em `novos`. Isso verifica a mordida lexical prevista pelo código. — `app/core/medida_leitor_do_dia.py:348`; `app/core/medida_leitor_do_dia.py:370`; `app/core/tests/test_medida_leitor_do_dia.py:110`.
- **E4-1, dúvida forte na mordida funcional:** o mutante literal é pego pelo AST, mas “selecionar erroneamente” não garante “fechar”. O nascimento do chamado pode lavrar ATA com lâmpadas apagadas; `_lampada_folga` preserva essa ATA. Então `_celula_furada=True` faz `premissa_regularizada=False` antes do fallback para `espurios`, e o gate posterior recusa o fechamento. Nesse caminho, os asserts aberto/folga sobrevivem ao mutante. Execução efetiva: **[nao verificado]**. — `app/chamados/signals.py:164`; `app/ponto/services/cartorio.py:1011`; `app/ponto/services/cartorio.py:1042`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:67`; `app/chamados/juizes.py:454`; `app/chamados/models.py:1247`; `app/chamados/models.py:646`; `app/chamados/models.py:709`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:97`.

Para prender a seleção, falta observar diretamente que A não pertence a `espurios`, além de observar o estado final; hoje o teste verifica apenas faixa inicial, fechamento e coluna. — `app/chamados/services/feriado_reavaliar.py:23`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:93`.

**3. `painel_op`: mudança de universo**

No trecho substituído, a condição adicional é **`retratada_em IS NULL`**. Permanecem o limite inferior `agora−24h`, ausência de limite superior, todos os colaboradores na pré-carga, filtro `tipo='E'`, `select_related` e ordenação por timestamp; o dict continua escolhendo a última E por colaborador. Não surge filtro por `aprovada`. — `app/core/services/painel_op.py:46`; `app/core/services/painel_op.py:51`; `app/ponto/turnos.py:2845`.

O comentário/nome “14h” continua divergente das **24h** efetivas; isso antecede a troca. “Mesmo universo do juiz” vale para retratação, sem igualdade das janelas: o juiz carrega três datas locais, `hoje−2..hoje`. — `app/core/services/painel_op.py:45`; `app/ponto/turnos.py:2186`; `app/ponto/turnos.py:2190`.

Continuam dois acessos diretos a Batida no arquivo:

- `_cids_pn` contém explicitamente o filtro de retratação. — `app/core/services/painel_op.py:60`.
- `bateu_hoje_ids` não contém esse filtro e admite qualquer tipo na data local. **Procedente:** a afirmação de que ele foi corrigido junto de `entradas_hoje` excede o patch. — `app/core/services/painel_op.py:239`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:3`.

Não há base para afirmar mudança visual de cobertura causada somente por esse segundo cru: atividade distingue `encerrado/inativo` quando previsto/presente são zero, mas o painel converte ambos em `encerrado`. — `app/ponto/selecao_periodo.py:237`; `app/core/services/painel_op.py:242`.

**4. `minha_escala`: `previsto` nas duas cascas**

**Procedente: o contexto muda; a apresentação do trabalho não consome a mudança.** A view publica `ROTULO_STATUS['previsto']`, porém o ramo trabalho mostra somente início/fim. Cor depende de hoje e `trabalho`; não há decisão por `previsto`, título de previsão ou ícone específico desse status. Portanto não cai em default de status desconhecido: a string não participa da renderização desse ramo. — `app/escala/views.py:54`; `app/escala/views.py:60`; `app/templates/escala/minha_escala.html:63`; `app/templates/escala/minha_escala.html:76`.

As duas cascas compartilham esse conteúdo: template estende `base_tpl`, e `app_shell` escolhe `base.html/base_app.html`. Renderização efetiva no navegador: **[nao verificado]**. — `app/templates/escala/minha_escala.html:1`; `app

[truncado em 20000 de 23911 caracteres]

## 31/10 16:27 · sessao e6629a237889 · V3-RETOMA · verificador · origem=Code

**BLOQUEIA — permanecem pausas perdidas no split e a fusão das pontas altera a prorrogação noturna.** A mistura entre os totais de HE de CCTs diferentes foi corrigida, mas esses dois problemas impedem aprovação (`app/ponto/calculador/alimentacao.py:539`; `app/ponto/calculador/regras.py:840`; `app/ponto/calculador/regras.py:852`).

**BLOQUEIA — os intervalos internos do período continuam fora do split.** A porta conserva o envelope inteiro em `he_pontas`, desconta os intervalos dos segmentos trabalhados, mas chama `do_dia` com `intervalos=()`. O novo split acrescenta somente lacunas **entre** períodos; não recupera as pausas internas (`app/ponto/calculador/alimentacao.py:99`; `app/ponto/calculador/alimentacao.py:118`; `app/ponto/calculador/alimentacao.py:124`; `app/ponto/calculador/alimentacao.py:539`; `app/ponto/calculador/regras.py:851`).

Contraprova: feriado **21h–05h**, pausa **23h–00h**, trabalhados=420, previstos=360, intervalo declarado=60, dobra ativa e prorrogação desativada. Há excedente positivo de 60 minutos, substituído pela dobra: HE50=0 e HE100=7h. O split atual recua sete horas sem pausa, toma **22h–05h** e afirma **7h noturnas**. Com o intervalo julgado, o trabalho noturno é **22h–23h + 00h–05h = 6h**. A função compartilhada descontaria essa pausa se a recebesse; o motor a entrega, a porta diária não (`app/ponto/calculador/regras.py:767`; `app/ponto/calculador/regras.py:779`; `app/ponto/calculador/regras.py:841`; `app/ponto/motor_calculo_v2.py:390`; `app/ponto/motor_calculo_v2.py:392`; `app/ponto/motor_calculo_v2.py:956`). Ocorrência em produção: **[nao verificado]**.

**BLOQUEIA — fundir as pontas muda a janela de prorrogação.** A chamada única usa a primeira entrada e a última saída; `minutos_noturnos` decide a extensão depois das 05h pelo início/fim dessa chamada. Portanto, descontar as lacunas depois não equivale a julgar cada período separadamente (`app/ponto/calculador/regras.py:846`; `app/ponto/calculador/regras.py:852`; `app/ponto/motor_calculo_v2.py:109`; `app/ponto/motor_calculo_v2.py:145`).

Contraprova: mesmo dia, períodos **00h–01h e 06h–08h**, feriado em dobra, trabalhados=previstos=180 e prorrogação ativa. A chamada fundida conta 00h–08h como noturno prorrogado e desconta a lacuna 01h–06h: **HE100 noturna=3h**. Separadamente, o primeiro período dá 1h e o segundo zero. O próprio adicional noturno diário continua calculado por pares e reconhece somente 1h de relógio (`app/ponto/calculador/regras.py:82`; `app/ponto/calculador/regras.py:85`; `app/ponto/calculador/regras.py:841`; `app/ponto/calculador/regras.py:851`; `app/ponto/motor_calculo_v2.py:110`). Ocorrência em produção: **[nao verificado]**.

Além disso, a regra escrita exige ancoramento no fim de **cada período** e mantém “último par do dia” como lei aberta; o novo código assume o último fim global. Autorização específica para essa mudança: **[nao verificado]** (`app/docs/PROMPTS.md:344`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:255`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:261`; `app/ponto/calculador/regras.py:825`).

1. **Destino dos achados anteriores**

| Achado anterior | Resultado no diff atual | Prova |
|---|---|---|
| **BLOQUEIA — HE principal e split com CCTs diferentes** | **Resolve o defeito específico.** O split usa `_dobra/extra` do dia; extras do motor ficam apenas no detalhe. A porta também entrega períodos sem extras. | `app/ponto/calculador/regras.py:840`; `app/ponto/calculador/regras.py:862`; `app/ponto/calculador/alimentacao.py:99` |
| **NAO-BLOQUEIA — consultas por dia sem medida/cache equivalente** | **Resolve em parte.** Acrescenta instrumento e teto, mas mantém a fábrica por dia; não há resultado medido fornecido. | `app/ponto/calculador/alimentacao.py:403`; `app/ponto/calculador/alimentacao.py:517`; `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`; **[nao verificado: contagem real]** |
| **NAO-BLOQUEIA — testes apresentados como RED que passariam antes** | **Resolve em parte.** Nacional e vigência interna viram `GUARDA`; a fixture de praça passa a cobrir 21/09; surge vigência começando no meio. A dobra básica continua sendo guarda. | `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:45`; `:217`; `:270`; `:313`; `:337` |
| **NAO-BLOQUEIA — cobertura incompleta** | **Resolve em parte.** Acrescenta excedente positivo com dobra e HE50 noturna. Os casos novos continuam sem assert de saída antecipada e sem mudança real dos parâmetros noturnos por praça/vigência. | `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:48`; `:125`; `:258`; `:289`; `:320` |

Os testes existentes dos outros parâmetros verificam troca de **template**, usando fábrica simulada; isso não demonstra mudança dos parâmetros noturnos pela vigência ou praça da CCT real (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:158`; `:176`; `:187`; `:193`).

2. **Quem escreve e quem lê**

| Campo/valor | Escritor/origem | Leitor/destino |
|---|---|---|
| Entrada/saída efetivas e intervalos julgados | O motor carimba `efetivo_entrada`, `efetivo_saida` e `intervalos_do_motor`. | A porta lê as pontas, monta segmentos e guarda o envelope em `he_pontas`. **O intervalo não acompanha esse envelope até o split.** (`app/ponto/motor_calculo_v2.py:1778`; `app/ponto/calculador/alimentacao.py:75`; `:99`; `:124`; `:539`) |
| Dia da jornada | O motor publica o mapa por identidade do período. | A porta chaveia pontas/segmentos pelo mapa; o núcleo soma pelo terceiro elemento do par; o laço consulta a CCT pelo mesmo `k`. (`app/ponto/motor_calculo_v2.py:2398`; `app/ponto/calculador/alimentacao.py:70`; `:99`; `app/ponto/calculador/nucleo.py:134`; `:141`; `app/ponto/calculador/alimentacao.py:517`) |
| Extras antigas dos períodos | O motor grava dobra/excedente nos períodos; há também distribuição diária nos ramos partido/comercial. | A porta copia para a tupla; agora `do_dia` só as soma em `he_motor_*_min`. (`app/ponto/motor_calculo_v2.py:2749`; `:2789`; `:3032`; `:3080`; `:3278`; `:3332`; `app/ponto/calculador/alimentacao.py:94`; `app/ponto/calculador/regras.py:862`) |
| HE50/HE100 principal do dia | `do_dia` calcula `extra`, pergunta a dobra e escreve os totais. | O split **repete os ramos/faixas** para produzir `_x50/_x100`; não lê diretamente `val['horas_extras_*']`. (`app/ponto/calculador/regras.py:769`; `:776`; `:780`; `:795`; `:810`; `:840`) |
| HE50/HE100 noturna | `he_noturna` devolve os minutos; `do_dia` grava horas em `val`. | Fechamento e comparador recebem as rubricas pela mesma porta. (`app/ponto/motor_calculo_v2.py:359`; `app/ponto/calculador/regras.py:852`; `:856`; `app/ponto/services/fechamento.py:1239`; `app/ponto/management/commands/diff_calculador.py:417`) |
| Persistência das rubricas | Fechamento entrega `rubricas` ao serviço existente, com versão `oraculo`; `lavrar` substitui os campos e grava `DiaPago`. | A soma da competência lê os campos persistidos, com versão padrão `motor`. (`app/ponto/services/fechamento.py:1245`; `:1256`; `:1262`; `app/ponto/services/dia_pago.py:328`; `:358`; `:548`; `:557`) |
| `he_calc_*`, `he_motor_*`, quantidade de pontas | `do_dia` escreve em `Rubricas.detalhe`. | Teste lê `he_motor_*`; comparador imprime o detalhe com `--colab`. A lavratura não copia esse detalhe: copia valores e causas de `nao_decide`. (`app/ponto/calculador/regras.py:859`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:322`; `app/ponto/management/commands/diff_calculador.py:545`; `app/ponto/services/dia_pago.py:322`; `:329`) |
| Motor CCT do dia `k` | `_cct_de` chama a fábrica com template diário e `k`; a fábrica resolve praça/vigência e prepara os parâmetros. | `cct_do_motor` transcreve os atributos para `CCT`; a porta entrega dobra, calendário, prorrogação e hora reduzida a `do_dia`. (`app/ponto/calculador/alimentacao.py:403`; `:517`; `app/core/regua_cct.py:329`; `:366`; `:466`; `:470`; `:477`; `app/ponto/calculador/nucleo.py:347`; `app/ponto/calculador/alimentacao.py:518`; `:534`; `:540`) |

A exportação examinada emite HE principal e adicional noturno; os splits não viram eventos separados no TXT. Assim, o defeito demonstrado no split não prova alteração do TXT atual (`app/folha/export.py:413`; `:430`; `:436`). Alteração efetiva exportada: **[nao verificado]**.

3. **Classes de reprovação e mordida dos testes** [nao verificado]

| Classe procurada | Resultado |
|---|---|
| Segundo juiz duplicando juiz registrado | Não identificado na resolução de vínculo/aplicação: a fábrica chega à mesma `vinculo_do_dia`, e praça/vigência continuam em `_aplicacao_vigente`. A composição global das pontas, porém, altera a pergunta temporal conforme o segundo bloqueio acima. (`app/escala/alimentacao.py:298`; `:301`; `app/core/regua_cct.py:253`; `:261`; `app/ponto/calculador/regras.py:852`) |
| Segundo escritor fora da porta | Nenhum novo identificado neste fluxo: a versão `oraculo` continua entrando pelo mesmo `lavrar`. (`app/ponto/services/fechamento.py:1245`; `:1262`; `app/ponto/services/dia_pago.py:328`; `:358`) |
| Relógio fora da borda | Nenhuma nova leitura do relógio identificada no diff; a decisão diária recebe `k`. O fechamento captura seu instante na borda da passada. (`app/ponto/calculador/alimentacao.py:481`; `:517`; `app/ponto/services/fechamento.py:595`) |
| Derivação paralela de fonte única | Há repetição interna da escolha dobra/excedente e das faixas no split. Divergência numérica causada somente por essa repetição: **[nao verificado]**. A divergência temporal da fusão está demonstrada acima. (`app/ponto/calculador/regras.py:779`; `:794`; `:840`; `:843`; `:852`) |
| Selo sem mordida | O `>0` do contador não comprova consultas diárias da CCT: outras consultas diárias continuam dentro da captura. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`; `:370`; `app/ponto/calculador/alimentacao.py:528`; `app/escala/utils.py:2120`) |
| Pouso antes do pré-requisito | Não demonstrado. O relato declara ausência de pouso e exige suite, Codex, opus e condições financeiras; o backlog mantém troca dependente de `!`. Cumprimento efetivo para este hash: **[nao verificado]**. (`app/docs/RELATO.md:14`; `app/docs/BACKLOG.md:816`) |

Simulação mental dos testes novos/alterados contra **7e0fc9bc**, usando o diff fornecido; execução da suíte nessa base: **[nao verificado]**.

| Teste/caso | Passaria na base? | [nao verificado]
|---|---|
| S5b invertido para HE diária | **Não**: a base usaria extras das pontas e não produziria os novos detalhes. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:292`; `:315`; `:322`) |
| Dobra com excedente positivo | **Sim**: o ramo dobra/`elif` já existia. É guarda contra `elif` virar `if`. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:48`; `app/ponto/calculador/regras.py:779`; `:794`) |
| CCT diária sem dobra × motor com dobra | **Não**: exige eliminar o split antigo positivo. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:111`) |
| CCT diária com dobra × motor zerado | **Não**: exige criar o split diário de 6h. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:118`) |
| Excedente diário × dobra do motor | **Não**: exige HE50 noturna=1h e HE100 noturna=0. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:125`) |
| Sem calendário | **Não**: exige declarar as duas parcelas, sem valores afirmados. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:134`; `app/ponto/calculador/regras.py:830`) |
| Ponta sem extras entra como geometria | **Não**: o filtro removido descartava essa ponta. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:150`; `app/ponto/calculador/alimentacao.py:99`) |
| Nacional e vigência interna renomeados | **Sim**, corretamente descritos como guardas. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:217`; `:313`) |
| Praça externa com fixture corrigida; vigência começando no meio | **Sim contra 7e0**, que já tinha resolução diária segundo o anexo. Distinguem a implementação anterior por `ini`, não esta troca do split. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:270`; `:284`; `:337`; `app/ponto/calculador/alimentacao.py:517`) |
| Contador | O diff não altera o caminho ORM capturado; não é RED da mudança do split. Resultado numérico: **[nao verificado]**. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`; `:368`) |

4. **Contraexemplos solicitados**

- **HE positiva + noturno + dobra:** o caso 21h–05h com pausa 23h–00h demonstra o primeiro bloqueio. Sem pausa, o novo teste protege a substituição do excedente, mas não afirma o split desse mesmo caso (`app/ponto/calculador/alimentacao.py:539`; `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:54`; `:61`).

- **CCT muda entre a fatia e `k`:** o motor antigo continua nascendo em `fatia_ini`; o calculador consulta `k`. Agora, ponta com 360 minutos de HE100 e CCT diária sem dobra produz total/split zero; no espelho, CCT diária com dobra e ponta zerada produz 6h/6h. Esse defeito específico está corrigido (`app/ponto/services/fechamento.py:711`; `app/ponto/calculador/alimentacao.py:517`; `app/ponto/calculador/regras.py:840`; `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:111`; `:118`).

- **Turno dividido atravessando 20→21:** se o juiz atribuir ambas as partes ao dia 20, pontas, minutos e CCT permanecem no dia 20. A busca inclui a saída do dia seguinte; o motor filtra pelo dia da jornada, e o núcleo conserva esse dia nos segmentos. Teste novo específico combinando split e virada: **[nao verificado]** (`app/ponto/services/fechamento.py:606`; `:610`; `app/ponto/motor_calculo_v2.py:2367`; `:2398`; `app/ponto/calculador/nucleo.py:134`; `:141`; `app/ponto/calculador/alimentacao.py:517`).

- **Intermitente em feriado municipal:** o motor esvazia o calendário injetado; esse conjunto chega à regra diária. A consulta de data ausente usa o calendário sem município, preservando somente o nacional nesse caminho. O teste municipal exige zero e o nacional é guarda (`app/ponto/motor_calculo_v2.py:3495`; `app/ponto/calculador/nucleo.py:350`; `app/ponto/calculador/alimentacao.py:534`; `app/ponto/motor_calculo_v2.py:901`; `app/core/feriados.py:91`; `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:211`; `:217`).

5. **NAO-BLOQUEIA — contador e teto 99 não constituem a medida prometida da CCT.**

`q2/q4` contam **todas as consultas de `rubricas_por_dia`**; `(q4-q2)/2` mede o custo marginal da porta nessa fixture. Previsto e intervalo também consultam vínculos por dia, portanto o `>0` pode continuar passando mesmo com cache da CCT (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`; `:368`; `:370`; `app/ponto/calculador/alimentacao.py:528`; `:529`; `app/escala/utils.py:2152`; `:2200`; `:2120`).

**99 é trava provisória sobre esse custo marginal, não valor medido.** O literal é escrito na constante e lido pelo `assertLessEqual` e sua mensagem; `q2`, `q4` e `por_dia` são locais, impressos e consumidos pelos asserts. Nenhum leitor produtivo desse contador foi identificado (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:366`; `:369`; `:370`; `:372`; `:377`; `:379`). Contagem efetiva, latência e impacto em produção: **[nao verificado]**.

## 31/10 16:26 · sessao 7c8017290e23 · - · - · origem=Ronald (tmux)

**BLOQUEIA — a fusão das pontas muda a prorrogação noturna.** O novo split chama `he_noturna` uma vez para o envelope inteiro; a prorrogação depende do início/fim dessa chamada (`app/ponto/calculador/regras.py:846`; `app/ponto/calculador/regras.py:852`; `app/ponto/motor_calculo_v2.py:109`).

Contraexemplo: mesmo dia, períodos 00–01 e 06–08, feriado em dobra, trabalhados=previstos=180, prorrogação ativa. A versão atual produz HE100 noturna=3h; a soma por período produz 1h. O próprio adicional noturno continua julgando os pares separadamente, produzindo 1h (`app/ponto/calculador/regras.py:82`; `app/ponto/calculador/regras.py:85`; `app/ponto/calculador/regras.py:840`). Executei as funções reais extraídas por AST em memória, sem arquivos/banco, com fuso fixo UTC−3: fundidas `(0,180)`; separadas `(0,60)+(0,0)` (`app/ponto/motor_calculo_v2.py:359`; `app/ponto/motor_calculo_v2.py:390`). Ocorrência em produção: [nao verificado].

**BLOQUEIA — mudança de ancoramento sem corte demonstrado.** A regra escrita exige fim de **cada período**, deixando explicitamente o último par como lei aberta; o novo código ancora no último fim global (`app/docs/PROMPTS.md:344`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:255`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:261`; `app/ponto/calculador/regras.py:825`). Contraexemplo sem divergência de CCT: p1=08–13 com HE50=90; p2=18–23:30 com HE50=30; trabalhados630, previstos510, pausa300, intervalo declarado0. HE50 total=120; antes HE50 noturna=30 minutos, agora90 (`app/ponto/calculador/regras.py:769`; `app/ponto/calculador/regras.py:795`; `app/ponto/calculador/regras.py:843`; `app/ponto/calculador/regras.py:852`). Aval específico para alterar essa distribuição: [nao verificado]. O registro de retomada permite construção em cópia e mantém a troca dependente de `!` (`app/docs/BACKLOG.md:816`).

**BLOQUEIA — a porta continua descartando os intervalos do split.** A ponta conserva o envelope inteiro, os pares descontam o intervalo, mas `rubricas_por_dia` passa `intervalos=()` (`app/ponto/calculador/alimentacao.py:99`; `app/ponto/calculador/alimentacao.py:118`; `app/ponto/calculador/alimentacao.py:124`; `app/ponto/calculador/alimentacao.py:539`). Contraexemplo: feriado21–05, pausa23–00, trabalhados420, previstos360, intervalo declarado60. HE100=7h; split sem intervalo=7h noturnas; com o intervalo julgado=6h (`app/ponto/calculador/regras.py:780`; `app/ponto/calculador/regras.py:841`; `app/ponto/motor_calculo_v2.py:390`; `app/ponto/motor_calculo_v2.py:392`). A execução isolada confirmou420 contra360 minutos. O defeito já existia no caminho anterior; a nova distribuição também o recebe (`app/ponto/calculador/alimentacao.py:539`).

O achado anterior de **mistura entre HE diária e extras da CCT da fatia foi resolvido no split**, pois os novos minutos vêm de `_dobra/extra` do dia; as extras antigas ficam apenas no detalhe (`app/ponto/calculador/regras.py:840`; `app/ponto/calculador/regras.py:862`). A ponta sem extras também entra agora (`app/ponto/calculador/alimentacao.py:93`; `app/ponto/calculador/alimentacao.py:99`).

Escrita/leitura dos valores:

- HE diária: `extra` e `_dobra` são julgados em `regras`; totais são escritos em `val`, relidos para montar `_x50/_x100`, e o juiz `he_noturna` devolve os splits (`app/ponto/calculador/regras.py:769`; `app/ponto/calculador/regras.py:776`; `app/ponto/calculador/regras.py:780`; `app/ponto/calculador/regras.py:795`; `app/ponto/calculador/regras.py:840`; `app/ponto/calculador/regras.py:856`).
- Geometria/extras antigas: porta lê os campos do período, escreve `he_pontas_por_dia`, depois entrega `he_pontas` ao dia (`app/ponto/calculador/alimentacao.py:94`; `app/ponto/calculador/alimentacao.py:99`; `app/ponto/calculador/alimentacao.py:465`; `app/ponto/calculador/alimentacao.py:538`).
- Persistência: fechamento chama a mesma porta e `lavrar`, com versão `oraculo`; `lavrar` substitui os campos e grava `DiaPago` (`app/ponto/services/fechamento.py:1239`; `app/ponto/services/fechamento.py:1245`; `app/ponto/services/fechamento.py:1262`; `app/ponto/services/dia_pago.py:320`; `app/ponto/services/dia_pago.py:328`; `app/ponto/services/dia_pago.py:358`).
- Detalhes novos: escritos em `regras`; leitor genérico é a impressão `--colab` do comparador. `lavrar` copia valores e `nao_decide`, mas não copia `Rubricas.detalhe` (`app/ponto/calculador/regras.py:859`; `app/ponto/management/commands/diff_calculador.py:545`; `app/ponto/services/dia_pago.py:322`; `app/ponto/services/dia_pago.py:329`).
- Exportação: não emite os splits como eventos próprios; emite HE principal e adicional noturno. A porta consulta `soma_da_competencia`, cujo padrão é versão `motor` (`app/folha/export.py:413`; `app/folha/export.py:430`; `app/folha/export.py:436`; `app/folha/porta_export.py:267`; `app/ponto/services/dia_pago.py:548`; `app/ponto/services/dia_pago.py:557`).

Na virada20→21, a alimentação conserva o dia do mapa do motor e o núcleo soma pelo terceiro elemento; o filtro de competência e a consulta CCT usam esse mesmo `k` (`app/ponto/calculador/alimentacao.py:70`; `app/ponto/calculador/alimentacao.py:99`; `app/ponto/calculador/nucleo.py:134`; `app/ponto/calculador/nucleo.py:141`; `app/ponto/calculador/alimentacao.py:481`; `app/ponto/calculador/alimentacao.py:517`). A busca inclui a saída do dia21 e o motor filtra pelo dia da jornada (`app/ponto/services/fechamento.py:606`; `app/ponto/services/fechamento.py:610`; `app/ponto/motor_calculo_v2.py:2367`). Regressão adicional nessa classe: [nao verificado].

## 31/10 16:26 · sessao e69e5490d2c0 · - · - · origem=Ronald (tmux)

Complemento concluído.

- **Procedente — escritor produtivo de `Batida.tipo`/`timestamp` fora da porta, anterior ao diff:** `materializar_regularizacao_externa` obtém `alvo = item.batida_alvo` e grava `alvo.tipo`, `alvo.timestamp`, `alvo.save(update_fields=['tipo','timestamp'])`; o FK do alvo aponta expressamente para `ponto.Batida`. Também remove por `alvo.delete()`. — `app/chamados/services/regularizacao_ext.py:127`, `app/chamados/services/regularizacao_ext.py:136`, `app/chamados/services/regularizacao_ext.py:153`, `app/chamados/models.py:3098`.
- **Caminho alcançável pelas três cascas:** confirmação web chama `confirmar_e_materializar`, assim como as duas APIs; o helper chama o materializador. Não é fixture nem legado desabilitado; ocorrência efetiva em produção **[nao verificado]**. — `app/chamados/views_regularizacao.py:216`, `app/api/views.py:3061`, `app/api/views_core.py:1327`, `app/chamados/services/regularizacao_ext.py:347`; rotas em `app/chamados/urls.py:54`, `app/api/urls.py:91`, `app/api/urls_core.py:16`.
- **Procedente — gate cego a esse escritor:** rodei somente o censo AST puro, com `python3 -B`, sem carregar Django/banco e sem escrever arquivos. Ele classifica `alvo.delete()` e `alvo.save()` como `entidade=None`; o gate pula entidades não atribuídas. Assim, “nenhum `Batida` atribuído fora da porta” não significa escritor único real. — implementação do censo em `app/core/censo_escritas.py:469`; limitação declarada em `app/core/censo_escritas.py:43`; descarte no gate em `app/ponto/tests/test_chokepoint_batida_gate.py:29`; escritas em `app/chamados/services/regularizacao_ext.py:136`, `app/chamados/services/regularizacao_ext.py:155`.
- **Outros candidatos revisados não são escritas persistentes:** normalização de timestamp em cópia no pareador e normalização em memória no motor, sem `save` nesses trechos. — `app/ponto/turnos.py:1524`, `app/ponto/motor_calculo_v2.py:409`.
- **Para `colaborador_id`, latitude, longitude e `retratada_em`:** o rg curto não encontrou outro escritor produtivo persistente fora da porta; exaustividade por alias/indireção **[nao verificado]**. Criação comum permanece `registrar_batida`; `retratada_em` passa por `anotar`/`retratar_batida` e desfazer humano. — `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:252`, `app/ponto/registro_batida.py:274`, `app/ponto/services/retratar_humano.py:207`.

**Leituras do retorno:** não apareceu consumidor de `lat`, `lng` ou `data_turno` do retorno do painel nos templates/cards/JS ou views que usam o construtor; eles são publicados no dicionário. `data_turno` tem consumidores claros nos dois testes antigos; `lat/lng` ficam sem consumidor encontrado, não certificar “inexistente” universalmente. — publicação em `app/core/services/painel_op.py:190`, `app/core/services/painel_op.py:195`; entrega pelas views em `app/core/views.py:610`, `app/core/views.py:635`; consumidores de teste em `app/core/tests/test_painel_op_turno_aberto_ancora.py:50`, `app/core/tests/test_painel_op_turno_pelo_juiz.py:80`; card usa entrada/ID/geo/status em `app/templates/core/_painel_card.html:64`.

## 31/10 16:25 · sessao db1652df3a91 · - · - · origem=Ronald (tmux)

Revisão estática dos testes; execução contra 7e0fc9bc: **[nao verificado]**.

| Teste novo/alterado | Resultado mental contra 7e0fc9bc |
|---|---|
| `test_MORDE_com_as_PONTAS_o_split_SAI_e_reparte_a_HE_DO_DIA` | **Falharia**: a base usava extras das pontas; o caso exige HE100 noturna zero e HE50 noturna 1,5, além dos novos detalhes (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:292`, `:315`, `:322`). |
| `test_MORDE_dobra_com_EXCEDENTE_nao_soma_HE50` | **Passaria**: dobra/`elif` já existiam e o diff não os altera. É guarda contra mutação `elif`→`if`, não RED desta alteração (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:48`; `app/ponto/calculador/regras.py:779`, `:794`). |
| CCT diária sem dobra × ponta com dobra | **Falharia**: base produziria parcela noturna positiva; o teste exige zero (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:111`). |
| CCT diária com dobra × ponta zerada | **Falharia**: base conservaria split zero; teste exige seis horas (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:118`). |
| Excedente diário × dobra na ponta | **Falharia**: base usaria HE100 da ponta; teste exige HE50 noturna=1 e HE100 noturna=0 (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:125`). |
| Sem calendário | **Falharia**: teste exige declaração das duas parcelas, ausência nos valores e causa; esse ramo é novo (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:134`; `app/ponto/calculador/regras.py:830`). |
| Ponta sem extra entra como geometria | **Falharia**: filtro anterior descartava a ponta zerada; teste exige sua presença (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:150`). |
| Nacional e vigência interna renomeados `GUARDA` | **Passariam**, agora corretamente descritos como guardas (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:217`, `:313`). |
| Praça externa com vínculo começando em setembro | **Passaria em 7e0fc9bc**, que já resolve CCT diariamente; a fixture corrigida distingue a implementação anterior que consultava `ini` (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:269`, `:284`; `app/ponto/calculador/alimentacao.py:517`). |
| Vigência começando no meio da janela | **Passaria em 7e0fc9bc** pelo mesmo motivo; é adversarial válido contra a implementação anterior por `ini` (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:320`, `:337`; `app/ponto/calculador/alimentacao.py:517`). |
| Contador de consultas | O diff não modifica o caminho ORM capturado; não constitui RED da troca do split. Resultado numérico efetivo: **[nao verificado]** (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`, `:368`). |

**NAO-BLOQUEIA — o contador mede o custo marginal da porta inteira e não comprova consultas diárias da CCT.** O contexto envolve toda `rubricas_por_dia`; seu `>0` pode continuar verdadeiro após introduzir cache na CCT, pois previsto e intervalo também consultam vínculos por dia sem receber `escalas` (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`, `:370`; `app/ponto/calculador/alimentacao.py:528`, `:529`; `app/escala/utils.py:2152`, `:2200`, `:2119`).

O **99 é teto provisório**, explicitamente declarado assim, lido pelo assert superior e pela mensagem. Não é valor medido fornecido; os valores `q2`, `q4` e `por_dia` são locais e impressos pelo teste (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:363`, `:368`, `:369`, `:372`, `:377`, `:379`).

O achado anterior de cobertura foi **resolvido em parte**: há excedente positivo em dobra e HE50 noturna explícitos; continuam ausentes nesses casos novos saída antecipada e mudança dos parâmetros noturnos por praça/vigência (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:48`, `:125`, `:258`, `:289`, `:320`).

**Pouso prematuro não demonstrado.** A documentação disponível diz “nada pousou, v2 intocada” e registra suite, Codex, opus e condições financeiras como pré-requisitos; o backlog mantém troca somente com `!` (`app/docs/RELATO.md:14`; `app/docs/BACKLOG.md:816`; `CLAUDE.md:710`). Cumprimento efetivo para 949baf5c: **[nao verificado]**.

## 31/10 16:25 · sessao da5f3e67fd21 · - · - · origem=Ronald (tmux)

- **NAO-BLOQUEIA — custo segue sem medida fornecida e o teste captura mais que CCT.** `CaptureQueriesContext` envolve toda `rubricas_por_dia`, contando todas as consultas dessa porta (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`). A porta também pede previsto e intervalo por dia sem alimentar `escalas=` (`app/ponto/calculador/alimentacao.py:528`), acionando novas consultas de vínculo (`app/escala/utils.py:2152`, `app/escala/utils.py:2200`, `app/escala/utils.py:2120`). Portanto `(q4-q2)/2` é custo marginal da porta nessa fixture; atribuí-lo exclusivamente à CCT excede o que o instrumento separa (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:368`).

- **99 é trava provisória, não medida.** Escritor: constante literal `CUSTO_POR_DIA_MEDIDO = 99`; leitor: `assertLessEqual` e sua mensagem. `q2`, `q4` e `por_dia` são locais do teste, impressos e consumidos pelos dois asserts; não há consumidor produtivo encontrado (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:366`, `:369`, `:370`, `:372`, `:379`). Valor efetivamente medido e latência: **[nao verificado]**.

- **CCT diária: fluxo sem segundo resolvedor identificado.** `_cct_de` chama `get_motor_cct(c,k,...)`; o laço entrega o template do vínculo diário e a data `k` (`app/ponto/calculador/alimentacao.py:403`, `:500`, `:517`). A fábrica lê switch e chama `regua_para`, que usa `vinculo_do_dia_carregado`; este alimenta a mesma juíza `vinculo_do_dia` (`app/core/regua_cct.py:444`, `:458`, `:329`; `app/escala/alimentacao.py:298`, `:301`). Aplicação e vigência são julgadas em `_aplicacao_vigente`, sem derivação disso na porta (`app/core/regua_cct.py:253`, `:261`, `:265`).

- **Escritores/leitores dos parâmetros.** `get_motor_cct` prepara prorrogação, afastamento da hora reduzida e exceção de dobra (`app/core/regua_cct.py:466`, `:470`, `:477`). `get_motor` resolve a dobra padrão pelo ciclo e entrega os argumentos ao construtor (`app/ponto/motor_calculo_v2.py:3607`). `MotorBase` grava atributos e calendário; `MotorIntermitente` substitui seu calendário por vazio (`app/ponto/motor_calculo_v2.py:712`, `:719`, `:737`, `:740`, `:3495`). `cct_do_motor` os transcreve para a dataclass congelada; a porta extrai e entrega a `do_dia` (`app/ponto/calculador/nucleo.py:281`, `:344`, `:347`; `app/ponto/calculador/alimentacao.py:518`, `:534`, `:540`).

- **Intermitente municipal: não dobra pelo caminho examinado.** O dispatch escolhe `MotorIntermitente`, que esvazia o calendário; esse conjunto vazio chega à regra diária (`app/ponto/motor_calculo_v2.py:3563`, `:3495`; `app/ponto/calculador/nucleo.py:350`; `app/ponto/calculador/alimentacao.py:534`). A regra empresta os métodos de `MotorBase`; ausência no conjunto consulta `is_feriado(data)`, sem município, alcançando somente nacional (`app/ponto/calculador/regras.py:290`, `:344`, `:347`; `app/ponto/motor_calculo_v2.py:901`; `app/core/feriados.py:91`). O comportamento histórico do intermitente já está registrado como pendência, não nasce neste diff (`app/core/juizes.py:437`). O teste municipal exige zero; o nacional é explicitamente guarda (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:211`, `:217`).

- **Relógio e escrita fora da porta:** nenhum `now`, `today` ou `localdate` encontrado nos módulos diretamente rastreados de CCT/alimentação/regras/núcleo/motor; a data é argumento explícito (`app/ponto/calculador/alimentacao.py:481`, `:517`; `app/core/regua_cct.py:317`). Nenhum segundo escritor persistente novo identificado no trecho examinado; a lavratura continua pelo serviço existente (`app/ponto/services/fechamento.py:1245`, `:1256`, `:1262`). Censo integral executado dos contratos: **[nao verificado]**.

- **RED contra 7e0:** o contador e o novo teste de vigência começando no meio não são RED do split desta volta: 7e0 já possuía resolução CCT diária segundo o anexo anterior, e o diff949 não a altera; passariam por inspeção, execução **[nao verificado]**. São guardas da resolução diária (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:337`, `:343`; anexo `diff_949b.patch` **[nao verificado: sem numeração local do anexo]**).

- **Pouso:** não há prova fornecida de deploy irregular. O relato da cópia declara a cura anterior sem pouso e sujeita a suite, Codex, opus e gates de dinheiro (`app/docs/RELATO.md:14`); cumprimento real desses requisitos: **[nao verificado]**.

## 31/10 16:24 · sessao 53afcb528e7e · - · - · origem=Ronald (tmux)

**E4-1 — achados**

- **Procedente: o selo AST não prova o “caminho do cron” inteiro.** Varre somente os dois arquivos literais de `SITIOS`, inteiros, sem seguir imports/chamadas; coleta atributos, nomes, aliases, keywords e strings, depois intersecta quatro nomes proibidos. A presença de `fatos_do_dia` é apenas nominal, sem exigir chamada ou uso do retorno. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:116`, `:120`, `:136`)
- **Procedente: há falsos negativos e positivos estruturais.** Não vê helper transitivo, nome montado por concatenação ou `filter(celula__status_dia=...)`/`values('celula__status_dia')`, pois guarda somente o prefixo anterior a `__`; pode reprovar string/docstring inocente exatamente igual a nome proibido. Comentários não entram no AST; código morto entra. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:122`, `:130`, `:132`)
- **O mutante literal descrito é morto pelo AST:** `celula.status_dia` adiciona `status_dia` ao conjunto proibido; os exemplos MORDE verificam detecção, não aplicam mutação ao service. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:123`, `:140`, `:144`)
- **Dúvida forte: o funcional pode sobreviver ao mutante descrito.** A docstring afirma que trocar o teste de faixa pela coluna fecha o primeiro caso, mas `encerrar` ainda atravessa `reconciliar` e o gate da premissa. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:15`; `app/chamados/services/feriado_reavaliar.py:47`; `app/chamados/models.py:615`, `:646`, `:699`)
- O nascimento do chamado dispara o cartório, que lavra ATA; `_lampada_folga` altera somente `status_dia/status_rotulo`, preservando ATA. Com ATA de trabalho e lâmpadas apagadas, `_celula_furada` responde `True`; `premissa_regularizada` retorna `False` **antes** do fallback para `espurios`, e o gate recusa o fechamento do único chamado. Nesse caminho, as asserções de permanecer aberto/folga passam mesmo com a seleção errada do mutante. Execução efetiva do TestCase/mutante: **[nao verificado]**. (`app/chamados/signals.py:164`; `app/ponto/services/cartorio.py:1011`, `:1042`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:67`, `:97`; `app/chamados/juizes.py:454`; `app/chamados/models.py:1247`, `:1252`, `:709`)
- A fixture A escolhe o vínculo novo para 15/10; o antigo encerra em 30/09, e o overlay da grade só percorre vínculos selecionados na janela. Isso sustenta a ATA de trabalho no julgamento unitário que precede `_lampada_folga`; falta asserção explícita da ATA/lâmpadas e da exclusão do chamado em `espurios`. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:44`, `:49`, `:88`; `app/escala/utils.py:1254`, `:1261`, `:1548`, `:1785`)
- **Dúvida de relógio no teste:** E4-1 não congela o tempo; criar o chamado alcança `julgar_celula`, que lê `localdate()` e abandona dias futuros. Antes de 15/10/2026, a preparação da ATA segue outro caminho. As condições temporais de execução do selo fornecidas: **[nao verificado]**. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:18`, `:26`, `:30`; `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1691`, `:1700`)
- **Dúvida de pré-requisito:** o RELATO manda E4-1 esperar a cura da grade e a sombra com 0044; a cópia mantém `_aplicar_folgas_foto` sem o descarte por vínculo do dia. A realização da cura/sombra exigida não consta do dado fornecido **[nao verificado]**. O cron continua JUIZ, portanto o diff não o rebaixa prematuramente. (`app/docs/RELATO.md:61`, `:63`; `app/escala/utils.py:1109`, `:1118`; `app/config/crons.py:1983`)

**Escritores/leitores relevantes**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.status_dia/status_rotulo` | Produção: `_aplicar_status`, chamada por `lavrar_veredito`; fixture injeta com `update`, deliberadamente fora da porta para fabricar divergência. (`app/ponto/portas/celula.py:701`, `:709`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:52`, `:67`) | Leitor canônico D4; cartório verifica ausência de lavra; fixture consulta `status_dia`. O cron/service atuais não leem essas colunas. (`app/ponto/status_dia.py:171`, `:181`; `app/ponto/services/cartorio.py:726`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:80`; `app/chamados/services/feriado_reavaliar.py:33`) |
| `faixa`, `era_dia_previsto`, `fatos.folga_lancada` | `fatos_do_dia` delega ao período, que produz os dicionários. (`app/ponto/precedencia.py:195`, `:209`, `:355`) | `espurios` exige previsão conhecida e faixa feriado/folga; teste verifica faixa trabalho e usa demais campos no diagnóstico. (`app/chamados/services/feriado_reavaliar.py:33`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:73`, `:94`) |
| `FolgaDia.escala_colaborador/data/calendario_origem` | Fixture cria calendário antigo ou folga manual no novo; produção materializa calendário, cria manual e reparenta só manual. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:88`, `:104`; `app/escala/folgas.py:173`, `:259`, `:405`, `:435`) | Precedência lê por pessoa/data e descarta calendário de vínculo diferente do escolhido; grade lê por vínculo e aplica overlay. (`app/ponto/precedencia.py:236`, `:307`, `:313`; `app/escala/utils.py:1109`, `:1118`) |
| `Chamado.status_local` | Fixture nasce `aberto`; fechamento real passa por `_gravar_estado`, com save e trilha. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:58`; `app/chamados/models.py:618`, `:222`, `:274`) | Universo seleciona `VIVOS`; motor/gates consultam; testes recarregam e exigem aberto/resolvido. (`app/chamados/services/feriado_reavaliar.py:17`; `app/chamados/models.py:637`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:96`, `:109`) |
| `modulo_origem/subcategoria/contexto_json.data_turno` | Fixture cria `batida_ausente/atraso/data_turno`; catálogo declara a chave de data desse módulo. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:59`; `app/chamados/catalogo/modulos.py:105`) | Universo/`premissa_morta`; dia vem de `dia_do_fato`, que consulta catálogo e fallback `data_turno`. (`app/chamados/services/feriado_reavaliar.py:17`, `:30`, `:38`; `app/chamados/juizes.py:89`, `:104`, `:109`) |
| `FATO_FALTA_DESCOBERTA=False` | Coletor `espurios` fornece o fato ao catálogo depois de ler o juiz. (`app/chamados/services/feriado_reavaliar.py:36`, `:38`) | `premissa_morta` delega a `avaliar`; catálogo responde escopo e vida da premissa. (`app/chamados/catalogo/premissa.py:115`, `:127`, `:144`) |

O juiz de precedência já está registrado; o service delega a ele, e o diff E4-1 apenas acrescenta testes. O fechamento usa a porta existente, sem escritor novo de produção nesse trecho. (`app/core/juizes.py:160`; `app/chamados/services/feriado_reavaliar.py:27`, `:47`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:1`)

## 31/10 16:24 · sessao d8fb872ec851 · - · - · origem=Ronald (tmux)

E4-6 — conclusões por leitura estática:

- **Procedente: o novo rótulo de trabalho não chega ao HTML.** `minha_escala` produz `_st='previsto'` e publica somente a frase em `dias[].status_rotulo`, sem publicar `_st`/`status` (`app/escala/views.py:54`, `:60`). No template, `status_rotulo` só é lido nos ramos `trabalho is None` e folga; o ramo trabalho imprime exclusivamente horários (`app/templates/escala/minha_escala.html:74`, `:76`, `:78`, `:81`, `:84`). Assim, a troca corrige o contexto Python, mas não altera a apresentação de um dia de trabalho (`app/escala/views.py:54`; `app/templates/escala/minha_escala.html:76`).

- **Procedente: não existe tratamento de `previsto` por palavra, cor ou ícone nessa tela.** As cores usam `dia.data == hoje` e o booleano `dia.trabalho`; o dia de trabalho mostra horários, sem ícone específico nem `title` de previsto (`app/templates/escala/minha_escala.html:63`, `:65`, `:66`, `:76`). Isso não cai em um default de status desconhecido: o status nem participa da decisão visual (`app/templates/escala/minha_escala.html:63`, `:74`). Quebra observada em navegador: **[nao verificado]**.

- **Duas cascas, mesmo comportamento.** O template único estende `base_tpl`, cuja fonte é `app_shell`; esta usa `modo_app` para selecionar `base_app.html`/`base.html` (`app/templates/escala/minha_escala.html:1`; `app/core/context_processors.py:194`, `:205`, `:210`). O processador está instalado em `app/config/settings/base.py:82`. O teste injeta `via_app` e `plataforma_sessao`; `carimbo_app` lê esses campos e `modo_app` o chama (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:42`; `app/core/utils.py:52`, `:54`, `:67`).

- **Procedente: a prova HTML das cascas não morde o mutante descrito.** O teste das cascas verifica presença do template base, ausência da frase de `ok` e presença do `title` de folga; não exige a frase de previsto nem sua aparência (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:66`, `:70`, `:72`, `:73`). A frase de `ok` também ficaria invisível no ramo trabalho original, porque esse ramo não lê `status_rotulo` (`app/templates/escala/minha_escala.html:76`). O teste de **contexto**, sim, rejeita `previsto -> ok`, ao exigir o rótulo de previsto nos futuros de trabalho (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:50`, `:53`, `:56`, `:57`). Execução Django/RED histórico: **[nao verificado]**.

- **Dúvida de enquadramento: a categoria diz futuro, a janela inclui hoje.** A view lê `datetime.date.today()`, percorre `range(14)` a partir de zero e dá `previsto` a todo dia de trabalho, inclusive hoje, independentemente de batidas/turno (`app/escala/views.py:26`, `:38`, `:39`, `:54`). O rótulo reutilizado diz literalmente “Dia futuro, ainda sem batida” (`app/ponto/status_dia.py:39`), enquanto a própria autoridade anterior distingue “Dia ainda nao encerrado, sem batida” (`app/colaboradores/services/calendario.py:46`). O selo testa futuros com `data > HOJE`, deixando hoje fora da exigência de rótulo (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:52`, `:53`). Como o rótulo de trabalho está invisível no template, impacto humano atual desse erro de contexto: **[nao verificado]**.

- **Relógio: existente na view, congelado apenas nos testes.** A leitura real continua `datetime.date.today()`; o diff não a introduziu nem a levou a um parâmetro (`app/escala/views.py:26`). O teste usa `freeze_time` (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:20`). A API usa data local de Django, diferente da construção da view; divergência em produção depende do fuso do processo, ausente do dado **[nao verificado]** (`app/api/views.py:926`; `app/config/settings/base.py:93`).

Fontes e consumidores:

| Campo/valor | Quem escreve / fonte | Quem lê |
|---|---|---|
| `trabalho` | Retorno de `ec.eh_dia_trabalho(dia)` (`app/escala/views.py:40`); juiz lê `CelulaDia.trabalha` ou cálculo do vínculo (`app/escala/models.py:1069`, `:1072`, `:1073`) | Escolha de marcos e `_st`, contexto (`app/escala/views.py:42`, `:54`, `:57`); cor e conteúdo (`app/templates/escala/minha_escala.html:65`, `:66`, `:74`, `:76`) |
| `CelulaDia.trabalha` e `dna` | Porta gera/regera a partir de `eh_dia_trabalho_calculado` e `montar_dna` (`app/ponto/portas/celula.py:243`, `:247`, `:260`, `:264`); também restauração/geração diária (`:532`, `:595`, `:612`) | Juízes do vínculo (`app/escala/models.py:1072`, `:1099`) |
| Horários do dia | `ec.marcos_do_dia` (`app/escala/views.py:43`), que lê DNA e só sem marcos usa template (`app/escala/models.py:1097`, `:1099`, `:1106`, `:1108`) | Contexto (`app/escala/views.py:58`, `:59`), template (`app/templates/escala/minha_escala.html:78`, `:81`) |
| `_st` | Ternário da view (`app/escala/views.py:54`) | Indexação de `ROTULO_STATUS` (`app/escala/views.py:60`); também medidor AST (`app/core/medida_leitor_do_dia.py:340`) |
| `ROTULO_STATUS` | Dicionário canônico (`app/ponto/status_dia.py:28`) | Calendário reexporta (`app/colaboradores/services/calendario.py:45`); view importa e indexa (`app/escala/views.py:36`, `:60`); medidor lê AST da autoridade (`app/core/medida_leitor_do_dia.py:124`, `:159`, `:164`); testes comparam (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:15`, `:56`) |
| `dias[].status_rotulo` | Somente contexto local da view; não grava `CelulaDia.status_rotulo` (`app/escala/views.py:55`, `:60`, `:64`) | Template, ramos dúvida/folga (`app/templates/escala/minha_escala.html:75`, `:84`); testes de contexto (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:56`, `:57`, `:64`) |
| `ancora_ausente` | `tipo_ciclo` e `data_ancora_colaborador` do vínculo (`app/escala/views.py:32`) | Contexto e aviso no template (`app/escala/views.py:68`; `app/templates/escala/minha_escala.html:49`) |

A rota HTML é `config/urls.py:27` + `escala/urls.py:11`, com retorno `render` em `escala/views.py:64`. Não há JS de página nem serialização desse contexto no template (`app/templates/escala/minha_escala.html:1`, `:100`). A API **não é consumidora** da string da view: `/api/escala/v2/` tem implementação independente, retorna `dias` sem `status`/`status_rotulo` (`app/api/urls.py:72`; `app/api/views.py:908`, `:945`, `:956`). A API v1 retorna 410 (`app/api/views.py:878`, `:883`). API v2 chama o mesmo juiz de trabalho, mas usa marcos do **template**, ao contrário da view HTML (`app/api/views.py:934`, `:939`; `app/escala/views.py:43`); autoridade registrada dos marcos do dia é `EscalaColaborador.marcos_do_dia` (`app/core/juizes.py:182`). Divergência concreta em dado de produção: **[nao verificado]**.

Medidor:

- `CATEGORIAS`, `PALAVRAS_DA_PREVISAO`, `NAO_E_LEITOR`, teto e `DECLARADOS` são escritos como constantes no próprio módulo (`app/core/medida_leitor_do_dia.py:259`, `:262`, `:263`, `:285`, `:289`, `:296`). O leitor de produção das exclusões é `censo`, que lê categoria/vocabulário permitido e distribui registros entre `excluidos` e `status` (`app/core/medida_leitor_do_dia.py:348`, `:350`, `:351`, `:353`). Os testes leem categorias/teto/motivos/disjunção (`app/core/tests/test_medida_leitor_do_dia.py:73`) e especificamente a exceção de previsão (`:85`).

- **Procedente: previsão tem predicado lexical; não é exclusão incondicional.** `pal ⊆ PALAVRAS_DA_PREVISAO` decide exclusão; `ok` devolve o item ao status, e `novos` usa diferença contra `DECLARADOS` (`app/core/medida_leitor_do_dia.py:348`, `:349`, `:353`, `:377`). Fiz apenas sondagem Python `-B`, em memória: detector retornou base `['escala_suspeita','folga','previsto']`, mutante `['escala_suspeita','folga','ok']`, três sítios em ambos; o predicado aceita base e rejeita mutante, conforme `app/core/medida_leitor_do_dia.py:229`, `:349`.

- **Limite procedente: o predicado não prova “futuro” nem “pergunta à grade”.** Inspeciona apenas os literais já reconhecidos; não valida data, ausência de batidas ou autoridade consultada (`app/core/medida_leitor_do_dia.py:340`, `:348`). Indireção sem literal, booleanos/relógio e HTML/JS estão explicitamente fora da visão (`app/core/medida_leitor_do_dia.py:317`). Uma exclusão que desapareça vira `exclusoes_mortas`, cobrada pela suíte (`app/core/medida_leitor_do_dia.py:384`; `app/core/tests/test_medida_leitor_do_dia.py:74`).

- Os consumidores externos do resultado são a suíte (`app/core/tests/test_medida_leitor_do_dia.py:29`) e `bin/leitor_dia_estado.py`, que carrega o módulo e chama `censo` (`bin/leitor_dia_estado.py:25`, `:32`, `:35`); `gerar_estado` importa esse leitor (`bin/gerar_estado.py:290`). O selo S3 importa daqui o detector de **valor**, não o predicado de previsão (`app/ponto/tests/test_s3_placar_exercicio.py:38`).

As fixtures E4-6 são escritas por `setUpTestData` e pela sessão do cliente (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:23`, `:26`, `:27`, `:28`, `:29`, `:30`, `:32`, `:33`, `:36`, `:43`); os leitores verificadores estão nos testes de contexto e render (`:50`, `:59`, `:66`). Não executar esses testes preservou a exigência de nenhum arquivo/banco criado; RED de execução continua **[nao verificado]**.

_1152 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
