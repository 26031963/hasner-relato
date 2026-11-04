# CODEX-ESTADO

> Ultima resposta processada: **03/11/2026 00:05** · publicado em **04/11/2026 00:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 03/11 00:05 -- HISTORICO (mais de 24 h) · sessao d1798f3d1359 · DEPLOY-[nome]-ELO · verificador · origem=Code

**A volta 3 melhora a proteção, mas ainda não fecha a classe.** Restam um caminho real de `docker exec` fora da admissão, um desvio do teto por `ADM_RESTART_TIMEOUT_S=00` e descarte de placar ilegível que pode produzir zero. (`bin/cron_run.sh:49`; `bin/admissao_cron.sh:55`; `bin/rc137_no_restart.sh:69`)

Executei o contrato em leitura: retornou `contrato_deploy: OK`. Não executei o selo integral, pois ele cria e remove arquivos; suíte inteira e mutantes executados permanecem **[nao verificado]**. (`bin/contrato_deploy.sh:60`; `bin/tests/test_deploy_nao_mata_elo.sh:44`)

**1. V3-1..V3-7: classe ou caso**

| Item | Conclusão |
|---|---|
| **V3-1 — fecho/reconhecimento** | **Cura os formatos nomeados, não toda a classe.** Agora há fecho transitivo com vistos, caminhos relativos, continuação por `\`, opções de compose e `$var`. Porém o classificador recebe somente o comando principal: deixa fora o próprio runner, que também executa `docker exec`. (`bin/admissao_cron.sh:89,97,99,119,128,131,208`; `bin/cron_run.sh:49,51`) |
| **V3-2 — ancestral** | **Cura o autoimpedimento síncrono do vigia; a exceção é mais ampla.** Qualquer marca ancestral recebe `segura=0`, sem examinar outros filhos ainda usando casca. O vigia real chama deploy sincronamente; um exec irmão concorrente nesse caminho atual é **[nao verificado]**. (`bin/admissao_cron.sh:189`; `bin/vigia_logins.sh:71`) |
| **V3-3 — relógio/passo** | **Cura retrocesso civil e sono do passo inteiro; não garante teto absoluto do procedimento.** As varreduras não têm prazo e, se a segunda vier vazia, o script prossegue antes de conferir o tempo esgotado. (`bin/admissao_cron.sh:66`; `bin/reinicio_sob_admissao.sh:40,44,45,48,54`) |
| **V3-4 — teto/escalada** | **Parcial: há contraexemplo concreto.** A validação rejeita apenas a grafia `0`; aceita `00` e `000`, menores que 120, e entrega esse prazo zero ao `timeout`. Conferi a função e um comando inofensivo em memória: o timeout interno permaneceu até ser interrompido pelo externo. A escalada `-k` existe, mas depende de o prazo disparar. (`bin/admissao_cron.sh:55`; `bin/reinicio_sob_admissao.sh:25,66`) |
| **V3-5 — contrato leitor** | **Cura o leitor para a linha atual com `timeout`.** Continua sendo verificação textual: uma linha executável que apenas imprima `"docker restart saas_core saas_ui mensageria"` também satisfaria essa busca. Não demonstra execução nem reunião das cascas em um único ato. (`bin/contrato_deploy.sh:25,28`) |
| **V3-6 — contador** | **Cura os casos nomeados, não toda a classe “ilegível não vira zero”.** Janela deriva da biblioteca; fim invertido e JSON quebrado contendo `137` viram desconhecido. JSON quebrado antes de aparecer `137` continua descartado e pode terminar em zero. (`bin/rc137_no_restart.sh:25,53,69,70,72,81`) |
| **V3-7 — proveniência** | **Cura a ambiguidade exit/proveniência nas novas linhas e nos dois leitores identificados.** Somente `rodou is False` exclui a corrida; saída real 70/75 continua sendo corrida com erro. Ausência do campo equivale a `true`. (`bin/cron_run.sh:20,23,29`; `app/config/crons.py:1433`; `app/core/management/commands/pipeline_placar.py:72,80`; `bin/crons_duracao.py:85`) |

O **contraexemplo com cron real** é `HOME + '/scripts/backup_all.sh'`, declarado e envelopado pelo runner. O censo o considera host. Se a admissão recusar, o comando principal não roda, mas o runner segue para `docker exec saas_core ... cron_quebrado`, sem marca e sem nova passagem pela trava. Também há esse caminho após falha do backup, cuja marca pode estar como `casca=0`. A árvore viva só pode reconhecer o exec depois que ele existir; não fecha a janela anterior ao seu nascimento. Ocorrência dessa corrida em produção: **[nao verificado]**. (`app/config/crons.py:263,1800`; `bin/tests/test_deploy_nao_mata_elo.sh:324`; `bin/cron_run.sh:16,25,48,49`; `bin/admissao_cron.sh:170,208`)

No sentido conservador, `vigia_logins.sh --auto` pode terminar pelo piso/percentual sem executar nada dentro da casca, embora nasça `casca=1` pelo ramo que contém deploy. Isso prova classificação além do caminho efetivamente executado; **recusa permanente causada por esse ramo não está demonstrada**. (`app/config/crons.py:284`; `bin/vigia_logins.sh:48,52,71`; `bin/tests/test_deploy_nao_mata_elo.sh:324`)

**2. Quem escreve e quem lê**

| Campo/artefato | Quem escreve | Quem lê |
|---|---|---|
| **`rodou` em `placar.jsonl`** | Runner inicia `false`, muda para `true` antes do comando e publica no JSON. (`bin/cron_run.sh:20,23,29`) | Predicado da configuração; pipeline e medidor perguntam a ele. Vigia e contador leem a mesma linha para outras perguntas. (`app/config/crons.py:1433`; `app/core/management/commands/pipeline_placar.py:72`; `bin/crons_duracao.py:85`; `app/core/services/crons_quebrados.py:46`; `bin/rc137_no_restart.sh:73`) |
| **`linha_nao_rodou`** | Definição única na configuração. (`app/config/crons.py:1432`) | Importação no pipeline e carregamento no medidor. (`app/core/management/commands/pipeline_placar.py:9`; `bin/crons_duracao.py:54`) |
| **`restarts.log`** | Restarter publica início antes do restart e fim nos caminhos de sucesso/falha. (`bin/reinicio_sob_admissao.sh:64,65,67,70,72`) | Contador; selo inspeciona registros. O placar de host consome a saída do contador. (`bin/rc137_no_restart.sh:45`; `bin/tests/test_deploy_nao_mata_elo.sh:359`; `bin/placar_code.sh:459`) |
| **Marca `<nome>_<emp>.<pid>`** | `adm_marcar`, chamado pelo runner e pela descoberta da corrente; publicação por temporário e `mv`. (`bin/admissao_cron.sh:203,210,211`; `bin/cron_run.sh:16`; `bin/encadeado.sh:48`) | Validação de identidade, classificação, ancestralidade e varredura. Remoção pelo desmarcador ou pela varredura de órfãs. (`bin/admissao_cron.sh:151,165,183,192,218`) |
| **`.admissao`, fd8 e flock** | Cron abre e trava antes da classificação/publicação; deploy abre e trava antes da segunda varredura e do restart. (`bin/admissao_cron.sh:206,207,208,211`; `bin/reinicio_sob_admissao.sh:28,43,44,66`) | Os mesmos participantes disputam a trava. Cron fecha fd8 antes do comando; deploy mantém a trava até terminar o restarter. (`bin/admissao_cron.sh:214`; `bin/reinicio_sob_admissao.sh:69,73`) |

**Não localizei leitor produtivo que ainda use 70/75 para decidir “não rodou”.** O vigia continua tratando saída diferente de 0/2 como quebra, deliberadamente; isso inclui recusas. O contador continua procurando 137. Nenhum desses dois distingue ausência de `rodou` de `rodou=true`. (`app/core/services/crons_quebrados.py:16,46`; `bin/rc137_no_restart.sh:73`)

**Único escritor produtivo do placar localizado na cópia: `bin/cron_run.sh`.** As outras escritas encontradas são fixtures; escritor externo à cópia ou dinâmico permanece **[nao verificado]**. (`bin/cron_run.sh:29`; `bin/tests/test_medicao_de_cron_nao_expira.sh:31`; `app/core/tests/test_crons_quebrados.py:33`)

A migração não recupera proveniência histórica: uma recusa antiga sem `rodou` será contada como corrida real, conforme a compatibilidade escolhida. Quantidade dessas linhas no dado produtivo: **[nao verificado]**. (`app/config/crons.py:1428,1433`)

**3. Relógio e ancestralidade**

O teto de espera do **restarter** agora usa `/proc/uptime`; seus `date` restantes registram início/fim civis. Entretanto, **a corrente ainda decide seu teto por `date +%s`**, podendo prolongar a espera se o relógio retroceder. (`bin/reinicio_sob_admissao.sh:37,38,63,65`; `bin/encadeado.sh:91,96,97`)

Ainda há janelas civis no medidor de sete dias, no vigia de 26 horas e no pipeline; este obtém hora e minuto em duas leituras distintas. O contador cruza timestamps civis, portanto o relógio monotônico do restarter não torna a correlação imune a ajustes civis. (`bin/crons_duracao.py:66`; `app/core/services/crons_quebrados.py:27,39`; `app/core/management/commands/pipeline_placar.py:122,124`; `bin/rc137_no_restart.sh:60,76,79`)

Quanto ao `/proc`:

- **`comm` contendo `) ` não demonstra defeito:** `${s##*) }` remove até a última ocorrência, preservando PPID e starttime. Conferi também strings com múltiplos `) ` em memória. (`bin/admissao_cron.sh:75,79,177`)
- **Reuso simples de PID não passa na marca moderna:** starttime é validado antes da isenção ancestral. Marca legada sem starttime aceita mera existência do PID e não prova identidade original. (`bin/admissao_cron.sh:154,156,159,188,189`)
- **A cadeia de ancestrais guarda somente PIDs**, sem identidade temporal nem retrato atômico com a validação posterior. Há uma lacuna contra mudanças entre essas leituras; isenção concreta de uma marca moderna não ancestral, pelo fluxo atual, permanece **[nao verificado]**. (`bin/admissao_cron.sh:173,178,185,188,189`)

**4. Mordida dos casos 13, 15 e 16**

| Caso | O que efetivamente distingue | Mutante plausível que ainda escapa |
|---|---|---|
| **13** | Docker herdando fd8; restart lento/falho; falha em gravar o início impedindo restart. A fixture de diretório no lugar de `restarts.log` agora ataca o pré-requisito. (`bin/tests/test_deploy_nao_mata_elo.sh:52,355,358,364,370,372`) | Retirar `8>&-` das varreduras/sono e preservá-lo nos Docker: o observador de fd8 está somente no fake Docker. Sobrevivência integral executada: **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:40,44,54`; `bin/tests/test_deploy_nao_mata_elo.sh:52`) |
| **15** | Diferencia corrida real 75 de recusa 75, exige os três valores de proveniência e duas amostras. (`bin/tests/test_deploy_nao_mata_elo.sh:396,397,400,407,419`) | Acrescentar `or d.get('exit') == 70` ao filtro do medidor apagaria corridas reais 70, sem atingir as amostras 0/75. O teste Django novo também só usa corrida real 75. Execução do mutante: **[nao verificado]**. (`bin/crons_duracao.py:85`; `app/chamados/tests/test_contract_crons.py:234`) |
| **16** | Ataca os formatos novos, ciclo, ancestral sequencial, relógio parado, TERM ignorado, passo maior que teto, limite alto do ambiente, leitor do contrato e erros do contador. (`bin/tests/test_deploy_nao_mata_elo.sh:423,434,444,455,460,468,472,478,488,491`) | Fixar `SEM_FIM_S=670` preserva a fixture atual: ela mata o velho 660, mas não demonstra derivação da fonte. Também falta ambiente `00` e ancestral com exec irmão vivo. Execução dos mutantes: **[nao verificado]**. (`bin/rc137_no_restart.sh:25`; `bin/tests/test_deploy_nao_mata_elo.sh:444,472,484,486`) |

O teste de restart surdo **morde a ausência de `-k`**: o fake ignora TERM antes de virar `sleep`, e o caso exige término dentro da escalada. (`bin/tests/test_deploy_nao_mata_elo.sh:55,56,460,461`)

O antigo mutante `_rr=0` agora é atacado: o caso 8 extrai a chamada real, exige a captura `$?` e cobra saída 75 com arquivo/argv. (`bin/tests/test_deploy_nao_mata_elo.sh:223,224,233`)

**5. Classes recorrentes**

| Classe | Conferência |
|---|---|
| **Segundo juiz registrado** | Não identifiquei duplicação de juiz de negócio no delta. A pergunta nova de proveniência tem um predicado compartilhado pelos dois consumidores; o registro vivo continua sendo `core/juizes.py`. Ausência global de duplicações: **[nao verificado]**. (`app/config/crons.py:1432`; `app/core/management/commands/pipeline_placar.py:9`; `bin/crons_duracao.py:54`; `app/docs/JUIZES.md:11`) |
| **Segundo escritor fora da porta** | Não localizei segundo escritor produtivo do placar. Há, contudo, **execução fora da porta de admissão** no alarme/recuperação do runner. (`bin/cron_run.sh:29,49,51`) |
| **Relógio fora da borda** | Corrigido o relógio civil da espera do restarter; permanece na espera da corrente e nas janelas dos leitores. (`bin/reinicio_sob_admissao.sh:38`; `bin/encadeado.sh:96,97`; `bin/crons_duracao.py:66`) |
| **Derivação paralela** | A janela sem fim agora deriva das constantes compartilhadas. **670 é coerente:** a primeira subida que falha encerra o procedimento, portanto não se acumulam três escaladas de KILL. A comparação das cascas literais do restart com `ADM_CASCAS` ainda depende do selo. (`bin/admissao_cron.sh:63`; `bin/reinicio_sob_admissao.sh:66,69,70`; `bin/tests/test_deploy_nao_mata_elo.sh:283,284`) |
| **Selo sem mordida** | Persistem as lacunas de 70 real, prazo `00`, derivação por valor fixo e sincronização do caso 14. (`bin/tests/test_deploy_nao_mata_elo.sh:383,397,472,484`) |
| **Ordem de trava** | Entre participantes migrados, publicação sob flock e segunda varredura sob a mesma trava fecham a janela publicação→restart. O alarme do runner não participa desse protocolo. (`bin/admissao_cron.sh:207,211`; `bin/reinicio_sob_admissao.sh:43,44,66`; `bin/cron_run.sh:49`) |
| **Pouso antes do pré-requisito** | Registro precede restart; migration precede chamada ao restarter. Suíte inteira, leitura final e pouso efetivo: **[nao verificado]**. A conduta exige suíte antes do opus e proíbe aprovação de pouso só pelo Codex. (`bin/reinicio_sob_admissao.sh:64,66`; `bin/deploy.sh:124,282`; `CLAUDE.md:370,373`) |

O comentário “fd8 não vai a filho nenhum” ainda excede o código: `date` e o `tr` das pipelines não recebem fechamento explícito. O selo demonstra fechamento no Docker. (`bin/reinicio_sob_admissao.sh:44,60,63,65,66`; `bin/tests/test_deploy_nao_mata_elo.sh:52`)

**6. Descartes da triagem**

- **D4b — envelope morto com filho vivo:** não é refutado. PID ausente torna a marca órfã e ela é removida, sem busca por descendentes reparentados. “Nenhum caminho de produção mata só o runner” permanece **[nao verificado]**; a fila não transforma isso em garantia de proteção. (`bin/admissao_cron.sh:154,192`; `bin/cron_run.sh:22`)
- **`manual` compartilhado:** a pendência permanece. Chamadas sem id compartilham destino e `.novo`; sucesso remove o arquivo desse id. O agendado tem ids próprios, portanto sua correção específica não depende de resolver o manual. (`bin/deploy.sh:52,54,285,286,361`; `bin/deploy_agendado.sh:94,104`)
- **Caso 14 apoiado em sleep:** a crítica permanece. Se o deploy atrasar antes da primeira varredura, ela já verá a marca e um mutante sem segunda varredura poderá passar. Não há confirmação da fase presumida pelo comentário. (`bin/tests/test_deploy_nao_mata_elo.sh:381,383,386,390`)

Esses descartes podem ser decisões de fila pelo julgador final; **não são demonstrações de improcedência técnica**, e a própria conduta manda encaminhar descartes ao opus. (`CLAUDE.md:368,369`)

**7. Noturno, recusa 75 e progresso do deploy** [nao verificado]

A classificação `noturno.py=1` tem fundamento: o RAW chama o noturno, este chama filhos literais, e eles executam em `mensageria`. O teto é 1500; o valor 2518 consta no dado fornecido e na configuração versionada, mas duração atual em produção permanece **[nao verificado]**. (`app/config/crons.py:575,576`; `eval/noturno.py:32,36,43,93`; `eval/gerador.py:65`; `eval/investigador.py:24`; `bin/admissao_cron.sh:35`; `app/config/crons_duracao.json:51`)

**2518 > 1500, sozinho, não implica bloqueio eterno.** Num cenário com uma corrida de 2518s começando junto da tentativa, sem outros bloqueadores, a primeira recusa após 1500s e uma segunda pode esperar os aproximadamente 1018s restantes. As hipóteses desse cenário em produção são **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:48,50`; `bin/deploy_agendado.sh:99,105`)

O agendado tem **três novas tentativas por padrão**, termina 75 se a recusa persistir e remove seu agendamento; não há repetição infinita por padrão nesse laço. Entretanto, o `flock` da régua não tem prazo: existe espera sem teto antes da tentativa. Além disso, novos crons continuam admitidos enquanto o deploy espera, permitindo recusas repetidas sem garantia de oportunidade livre. Ocorrência de bloqueio permanente: **[nao verificado]**. (`bin/deploy_agendado.sh:99,105,113,121,123`; `bin/reinicio_sob_admissao.sh:29,46`; `bin/admissao_cron.sh:207,214`)

**No caminho normal, marca moderna válida e não ancestral do noturno impede o restart; atingir 1500 não o mata.** Ainda não há garantia geral para envelope morto com filho vivo ou para a exceção ancestral ampla. Os timeouts internos do noturno também podem encerrar seus subprocessos independentemente do deploy. Morte produtiva do noturno após este delta: **[nao verificado]**. (`bin/admissao_cron.sh:154,168,189`; `bin/reinicio_sob_admissao.sh:48,50`; `eval/noturno.py:33,37,44,94`)

## 03/11 00:01 -- HISTORICO (mais de 24 h) · sessao 740c1f5f01c0 · - · - · origem=Ronald (tmux)

**V3-1 cura os formatos nomeados, mas não fecha o universo executado pelo cron.** O fecho segue arquivos transitivamente, aceita caminhos relativos e usa `vistos`; o parser junta `\`, pula opções do compose e considera `$var` como casca. Continua sendo reconhecimento lexical de arquivos nomeados, limitado a arquivos menores que 2 MB, sem interpretar imports, expansões ou controle de fluxo. (`bin/admissao_cron.sh:89`, `bin/admissao_cron.sh:97`, `bin/admissao_cron.sh:99`, `bin/admissao_cron.sh:119`, `bin/admissao_cron.sh:121`, `bin/admissao_cron.sh:128`, `bin/admissao_cron.sh:131`)

**Contraexemplo com cron real:** `HOME + '/scripts/backup_all.sh'`, declarado em `CRONS`, nasce classificado como host; porém seu envelope pode executar `docker exec saas_core ... cron_quebrado` após falha do backup ou após recusa da própria admissão. O classificador recebe somente `CMD`, deixando o código do envelope fora do fecho. Na recusa nem sequer há marca publicada; após falha do comando a marca pode existir com `casca=0`. A árvore viva só protege enquanto o Docker já estiver visível, deixando a janela anterior ao exec. (`app/config/crons.py:263`, `app/config/crons.py:1800`, `bin/admissao_cron.sh:208`, `bin/cron_run.sh:16`, `bin/cron_run.sh:25`, `bin/cron_run.sh:45`, `bin/cron_run.sh:49`, `bin/admissao_cron.sh:170`)

Verifiquei apenas os predicados em leitura: o fecho de `scripts/backup_all.sh` retornou 1; o texto de `bin/cron_run.sh` retornou classificação casca. A reprodução integral dessa corrida e sua ocorrência produtiva permanecem **[nao verificado]**. (`bin/admissao_cron.sh:126`, `bin/cron_run.sh:49`)

**Outro cron real sujeito ao mesmo caminho:** `tickets_rodape.sh --conferir` está pinado host; havendo flag anterior, seu sucesso ou rc2 pode executar a recuperação em `saas_core` pelo envelope. (`app/config/crons.py:296`, `bin/tests/test_deploy_nao_mata_elo.sh:324`, `bin/cron_run.sh:50`, `bin/cron_run.sh:51`)

**V3-2 resolve o autoimpedimento síncrono de `vigia_logins.sh --auto`, mas a exceção é mais ampla que esse caso:** toda marca cujo PID esteja na lista de ancestrais recebe `segura=0`, sem conferir se existem outros descendentes em Docker. O cron real chama deploy sincronamente; não encontrei nesse caminho um exec irmão concorrente que prove dano atual. (`app/config/crons.py:284`, `bin/vigia_logins.sh:71`, `bin/admissao_cron.sh:185`, `bin/admissao_cron.sh:189`)

**Não procede acusar o parser de quebrar por `comm` contendo `) `.** Tanto `adm_starttime` quanto `adm_ancestrais` removem até a **última** ocorrência de `) `, preservando os campos posteriores; a primeira função lê o campo 22 e a segunda o PPID. (`bin/admissao_cron.sh:75`, `bin/admissao_cron.sh:79`, `bin/admissao_cron.sh:177`)

**Reuso simples de PID não burla uma marca moderna:** a validação do starttime acontece antes da isenção ancestral. A ressalva é a marca legada sem starttime: basta existir `/proc/<pid>` para ela ser considerada viva, sem provar identidade do processo originalmente marcado. Uma corrida entre o retrato dos ancestrais e a conferência posterior não é protegida por identidade na lista, que contém somente PIDs; exploração concreta pelo fluxo atual permanece **[nao verificado]**. (`bin/admissao_cron.sh:154`, `bin/admissao_cron.sh:156`, `bin/admissao_cron.sh:159`, `bin/admissao_cron.sh:173`, `bin/admissao_cron.sh:188`, `bin/admissao_cron.sh:189`)

**Noturno:** o fecho agora encontra seus filhos literais `gerador.py`, `harness.py`, `juiz.py` e `investigador.py`; esses executam `docker exec mensageria`. A classificação `noturno.py=1` tem fundamento na cópia. (`eval/noturno.py:32`, `eval/noturno.py:36`, `eval/noturno.py:43`, `eval/noturno.py:93`, `eval/gerador.py:65`, `eval/harness.py:45`, `eval/juiz.py:241`, `eval/investigador.py:24`)

Não achei outro falso negativo no comando principal dos RAW atualmente pinados; ausência global de outros caminhos permanece **[nao verificado]**. O contraexemplo do envelope já basta para reprovar a alegação de classe fechada. (`bin/tests/test_deploy_nao_mata_elo.sh:324`, `bin/admissao_cron.sh:208`, `bin/cron_run.sh:49`)

## 03/11 00:01 -- HISTORICO (mais de 24 h) · sessao f9861db3b76e · - · - · origem=Ronald (tmux)

O selo melhorou e morde regressões específicas; ainda não prova as classes inteiras. A análise foi somente estática, pois ele cria e remove arquivos. Execução integral e sobrevivência efetivamente executada dos mutantes: **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:44`)

| Caso | O que morde | Mutante plausível sobrevivente |
|---|---|---|
| **13** | fd8 herdado pelo Docker; restart lento; falha com registro; impossibilidade de registrar início impede qualquer restart. (`bin/tests/test_deploy_nao_mata_elo.sh:354`, `:358`, `:364`, `:370`) | Remover o fechamento de fd8 das varreduras/sono, mantendo-o no Docker: o caso13 só observa `/proc/$$/fd/8` no fake Docker. Sobrevivência integral **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:52`, `:355`; `bin/reinicio_sob_admissao.sh:40`, `:44`, `:54`) |
| **15** | Distingue corrida real exit75 de recusa exit75; exige proveniência e exatamente duas amostras de duração. (`bin/tests/test_deploy_nao_mata_elo.sh:396`, `:397`, `:400`, `:407`, `:419`) | `linha_nao_rodou = rodou is False OR exit == 70`: continuaria apagando corridas reais exit70. O caso15 só produz exit0/75; o novo Django também só produz corrida real75. Sobrevivência prevista; execução **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:396`, `:397`, `:400`; `app/chamados/tests/test_contract_crons.py:233`, `:235`, `:236`) |
| **16** | Casos positivos/negativos do parser; fecho relativo com ciclo; ancestral; relógio parado; TERM ignorado; passo acima do teto; limite do env; contrato com casca faltante; truncamento137 e fim invertido. (`bin/tests/test_deploy_nao_mata_elo.sh:423`, `:434`, `:444`, `:455`, `:460`, `:468`, `:472`, `:478`, `:488`, `:491`) | Fixar `SEM_FIM_S=670`: a fixture compara comportamento no número atual, sem alterar as constantes produtoras. Portanto ela morde o velho660, mas não a derivação paralela. Sobrevivência integral **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:483`, `:484`, `:486`; `bin/admissao_cron.sh:59`, `:63`) |

O caso16 do ancestral usa um único fluxo sequencial, com `docker exec ... true` concluído antes do restart. Não testa ancestral com outro filho ainda usando a casca nem identidade temporal da cadeia de pais. (`bin/tests/test_deploy_nao_mata_elo.sh:444`; `bin/admissao_cron.sh:173`, `:189`)

O caso8 agora morde o antigo mutante `_rr=0`: extrai a própria chamada e captura do retorno e exige saída75, arquivo e argv; a extração também exige literalmente `_rr=$?`. (`bin/tests/test_deploy_nao_mata_elo.sh:223`, `:224`, `:233`)

**Descartes:**

- **Caso14:** permanece dependente de sleeps. Assume que o deploy já fez a primeira varredura antes da publicação; se o processo atrasar, a primeira varredura já verá a marca e o mutante sem segunda varredura poderá passar. A fila é decisão de escopo, não demonstração de cura. (`bin/tests/test_deploy_nao_mata_elo.sh:378`, `:380`, `:381`, `:383`, `:386`, `:390`)
- **Manual compartilhado:** continua literal `manual`, compartilhando arquivo e `.novo`; sucesso pode apagar a recusa daquele id. O agendado usa ids próprios, portanto isso não desfaz seu isolamento, mas não cura a classe concorrente manual. (`bin/deploy.sh:52`, `:54`, `:285`, `:286`, `:361`; `bin/deploy_agendado.sh:94`, `:104`)
- **D4b:** `/proc/<pid>` ausente declara marca órfã; a varredura remove-a sem procurar descendentes reparentados. A afirmação “nenhum caminho de produção mata só o envelope” permanece **[nao verificado]**; descartá-la da garantia geral de proteção não é provado pelo código. (`bin/admissao_cron.sh:154`, `:192`; `bin/cron_run.sh:22`)

**Noturno e retries:**

O RAW real chama `cd .../eval && /usr/bin/python3 noturno.py`; o comentário registra2518s, coincidindo com o dado fornecido. O teto é1500s. (`app/config/crons.py:559`, `:575`, `:576`; `bin/admissao_cron.sh:35`)

Isso sozinho não implica bloqueio eterno: sob a hipótese de uma corrida2518s iniciada junto à primeira tentativa, sem outro cron bloqueador nem atraso externo, a primeira tentativa recusa em1500s e a segunda dispõe de teto suficiente para o restante1018s. Hipóteses produtivas e duração atual: **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:48`, `:50`; `bin/deploy_agendado.sh:99`, `:105`)

As novas tentativas têm limite padrão3; recusa persistente termina75 e o agendamento é removido. Entretanto, o `flock` de `/tmp/regua.lock` não tem `-w`: há caminho de espera sem prazo antes de uma tentativa de deploy. Bloqueio produtivo dessa trava: **[nao verificado]**. (`bin/deploy_agendado.sh:78`, `:99`, `:105`, `:113`, `:121`, `:123`)

Não há timeout externo sobre o RAW completo no envelope, e a marca permanece até a saída do runner; timeout1500 do deploy não mata o noturno no caminho de recusa. Há timeouts internos dos subprocessos do noturno, independentes do deploy. (`bin/cron_run.sh:22`, `:24`; `bin/reinicio_sob_admissao.sh:48`, `:50`; `eval/noturno.py:32`, `:36`, `:43`, `:93`)

## 03/11 00:01 -- HISTORICO (mais de 24 h) · sessao de5873872725 · - · - · origem=Ronald (tmux)

- **V3-7 fecha a ambiguidade para as novas linhas:** o runner grava `rodou=false` antes da admissão e só muda para `true` antes de executar; escreve o campo no placar. O predicado exclui exclusivamente `False`; ausência do campo e `true` são equivalentes. Ambos os leitores de corrida/duração consultam esse predicado. (`bin/cron_run.sh:20,23,24,29`; `app/config/crons.py:1432,1433`; `app/core/management/commands/pipeline_placar.py:9,72`; `bin/crons_duracao.py:54,85`)

- **Nenhum leitor produtivo localizado usa 70/75 para inferir “não rodou”.** O vigia continua julgando quebra por saída diferente de 0/2, enquanto o contador busca 137; são perguntas distintas. (`app/core/services/crons_quebrados.py:46`; `bin/rc137_no_restart.sh:73`)

- **Único escritor produtivo localizado do placar:** `cron_run.sh`; as outras escritas encontradas pertencem a fixtures. Escritor externo à cópia: **[nao verificado]**. (`bin/cron_run.sh:29`; `bin/tests/test_medicao_de_cron_nao_expira.sh:31`; `app/core/tests/test_crons_quebrados.py:33`; `bin/tests/test_deploy_nao_mata_elo.sh:240`)

- **V3-6 cura os casos nomeados, não toda a classe “ilegível não é zero”.** Placar truncado antes de conter `137` continua sendo ignorado e pode resultar em zero. A verificação depende da substring literal. (`bin/rc137_no_restart.sh:69,70,71,72,81`)

- **670s não está subestimado por faltar KILL em cada casca:** cada primeira subida que falha encerra o script. O limite é restart antes de120 + duas subidas antes de180 + última subida180 com escalada10 =670. A derivação efetiva usa as constantes compartilhadas. (`bin/admissao_cron.sh:39,40,41,59,63`; `bin/reinicio_sob_admissao.sh:66,67,69,70`)

- **Persistem relógios civis decidindo outras janelas/tetos:** corrente espera carimbos com `date +%s`; medidor recorta sete dias; vigia recorta26h; pipeline lê hora/minuto separadamente. A espera do restarter usa `/proc/uptime`; seus `date` restantes registram testemunhas civis. (`bin/encadeado.sh:91,96,97`; `bin/crons_duracao.py:66`; `app/core/services/crons_quebrados.py:27,39`; `app/core/management/commands/pipeline_placar.py:122,124`; `bin/admissao_cron.sh:66,68`; `bin/reinicio_sob_admissao.sh:37,38,63,65`)

- **Mutantes sobreviventes previstos, sem execução:** acrescentar ao filtro de duração `or d.get('exit') == 70` apagaria corrida real70; o caso15 só oferece0/75. Copiar literalmente o predicado em `_nao_rodou` preservaria os efeitos e violaria fonte única; não há mutação da autoridade nessa fixture. Fixar `SEM_FIM_S=670` também conserva o caso16: ele cobra o valor atual, não a derivação. (`bin/crons_duracao.py:54,85`; `bin/tests/test_deploy_nao_mata_elo.sh:396,397,400,414,419,484,485,486`; `bin/rc137_no_restart.sh:25`)

- **Caso15 testa ausência de `rodou` apenas indiretamente fora do selo shell:** o novo teste Django fornece uma linha com `true` e outra sem campo, ambas com75, exigindo duas corridas e dois erros. Não verifica corrida real70 no medidor. (`app/chamados/tests/test_contract_crons.py:226,233,235,236,241,242`; `bin/tests/test_deploy_nao_mata_elo.sh:396,397,400`)

## 02/11 23:42 -- HISTORICO (mais de 24 h) · sessao cf0aa3782303 · DEPLOY-[nome]-ELO · verificador · origem=Code

**A volta 2 ainda não fecha P1..P7 como classes:** permanecem falhas de cobertura da fonte, teto e interpretação do placar; há também um leitor não migrado. (`bin/admissao_cron.sh:58,87`; `bin/reinicio_sob_admissao.sh:25,51`; `bin/rc137_no_restart.sh:63`; `bin/contrato_deploy.sh:23`)

Comparação Git independente dos hashes, execução integral da suíte e execução integral dos mutantes: **[nao verificado]**. A análise abaixo usa o patch fornecido e a cópia; o selo não foi executado porque cria, altera e remove arquivos. (`bin/tests/test_deploy_nao_mata_elo.sh:40,42,169`)

1. **P1..P7: classe, caso e contraexemplo**

| Item | Conclusão |
|---|---|
| **P1 — híbrido entre execs** | **Cura a sentinela e uma família de scripts literais, não toda a classe.** A sentinela contém `docker exec mensageria` e agora nasce `casca=1`. Entretanto, o reconhecimento é lexical, por linha, e segue caminhos absolutos somente um nível. `docker exec \` seguido de quebra de linha, `docker compose -f compose.yml exec…`, chamadas relativas indiretas e container por variável continuam podendo escapar entre execs. (`mensageria/bin/sentinela_diario.sh:14,16`; `bin/admissao_cron.sh:58,64,66,87,150`) |
| **P2 — teto absoluto** | **Parcial.** Passou a contar tempo decorrido e rejeita `PASSO=0`, mas aceita `ADM_PASSO_S=3600` com `ESPERA_CRON_S=1` e dorme o passo inteiro. Varreduras não têm prazo; a segunda varredura vazia permite prosseguir antes da conferência do tempo. Usa relógio civil, sujeito a retrocesso. (`bin/reinicio_sob_admissao.sh:19,21,35,36,38,42,43,46,51`) |
| **P3 — restart preso/fd herdado** | **Cura o fd no Docker e o processo que responde ao timeout; não toda espera ilimitada.** Restart e provas fecham fd 8. Porém `ADM_RESTART_TIMEOUT_S` aceita `0` ou prazo arbitrariamente alto, e `timeout` não tem escalada para KILL. Outros filhos, como os da varredura e `date`, ainda recebem fd 8. (`bin/reinicio_sob_admissao.sh:25,42,60,62,63,66`) |
| **P4 — recusa/argv** | **Cura o casamento por mtime e o argv vazio no protocolo agendado.** Há nome por tentativa, conferência do id interno e preservação do argv. A primeira passagem aceita somente o arquivo identificado, porque o rc da esteira pode estar traduzido/mascarado; as chamadas diretas seguintes exigem rc75 e arquivo. Portanto, não implementa literalmente “rc75 + arquivo” na primeira passagem. Deploys sem id continuam compartilhando `manual`, inclusive o mesmo `.novo`. (`bin/deploy.sh:48,52,54,285`; `bin/deploy_agendado.sh:74,83,94,99,101,105,107,110,111`) |
| **P5 — desconhecido/registro** | **Cura fonte ausente e registro da falha do restart, mas não a classe “erro de leitura não vira zero”.** JSON inválido do placar ainda é ignorado; restart com fim anterior ao início é aceito. O fallback fixo de 660s também pode ser menor que o prazo efetivo do escritor. (`bin/rc137_no_restart.sh:20,44,51,53,56,60,63`; `bin/reinicio_sob_admissao.sh:25,61,64,67`) |
| **P6 — “não rodou” nos leitores** | **Cura a interpretação das linhas de admissão 70/75 nos dois leitores citados.** Contudo, um comando admitido também pode executar e retornar 70/75: o runner grava o mesmo formato, e os filtros passam a apagá-lo das corridas/durações. A proveniência não está representada. (`bin/cron_run.sh:16,18,20,25`; `app/core/management/commands/pipeline_placar.py:72`; `bin/crons_duracao.py:85`) |
| **P7 — selo sem mordida** | **Melhorou substancialmente, mas não fecha a classe.** Há fixtures para intervalo entre execs, fonte ausente, tempo decorrido e segunda varredura. Ainda faltam integração do deploy real, escrita inicial impossível e prazo resistente a TERM; detalhes no item 4. (`bin/tests/test_deploy_nao_mata_elo.sh:217,248,295,324,338,355`) |

O censo pinado não elimina indireções: declara `vigia_logins.sh=0`, embora o RAW execute `--auto` e esse ramo chame `bin/deploy.sh --sem-migrate` por caminho relativo, não seguido pelo reconhecedor. Ocorrência desse ramo em produção: **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:312`; `app/config/crons.py:284`; `bin/vigia_logins.sh:71`; `bin/admissao_cron.sh:87`)

No contador real, executando apenas seu Python em memória, três entradas produziram `0`: placar truncado contendo `"exit":137`; restart `200 100` com fim de cron em epoch150; e início `100 -` com fim de cron em epoch900. São consequências, respectivamente, do descarte de JSON inválido, ausência de validação da ordem temporal e janela fixa. (`bin/rc137_no_restart.sh:20,44,53,63,70`)

2. **Quem escreve e quem lê**

| Arquivo/valor | Escritor ou produtor | Leitor/consumidor |
|---|---|---|
| `logs/crons_em_curso/<nome>_<emp>.<pid>` | `adm_marcar`, chamado pelo runner e pela descoberta da corrente; publica por temporário + `mv`. Limpeza por `adm_desmarcar` e varredura de órfãs. (`bin/admissao_cron.sh:143,153,160,134`; `bin/cron_run.sh:16`; `bin/encadeado.sh:48`) | `adm_marca_viva` lê PID/starttime/exe; `adm_segura` lê `casca` ou consulta árvore; restart consome `adm_seguram`. (`bin/admissao_cron.sh:105,119,139`; `bin/reinicio_sob_admissao.sh:38,42`) |
| `.admissao` / fd8 / flock | Cron abre e trava durante classificação/publicação; restart abre e trava durante conferência/restart/subida. (`bin/admissao_cron.sh:148,149,156`; `bin/reinicio_sob_admissao.sh:28,41,44,63,66`) | Os mesmos dois participantes disputam o mesmo arquivo. O comando do cron começa depois do fechamento do fd. (`bin/admissao_cron.sh:35,156`; `bin/cron_run.sh:20`) |
| `restarts.log` | Restart escreve `ini -` antes do ato e `ini fim` nos caminhos de sucesso/falha. (`bin/reinicio_sob_admissao.sh:59,61,62,64,67,69`) | Contador e selo; o placar consome a saída do contador. (`bin/rc137_no_restart.sh:40`; `bin/tests/test_deploy_nao_mata_elo.sh:343`; `bin/placar_code.sh:459`) |
| `recusa_cron.<id>.pendente` | `deploy.sh` escreve id e argv separados por NUL quando recebe rc75; publicação por `.novo` + `mv`. (`bin/deploy.sh:283,285,286`) | Agendado confere nome/id, lê argv, remove arquivo consumido e repete. Deploy manual bem-sucedido remove a recusa do seu id. (`bin/deploy_agendado.sh:83,99,101,102,104,113`; `bin/deploy.sh:361`) |
| rc70/75 da admissão | Literais de `adm_marcar`. (`bin/admissao_cron.sh:147,148,149,151,154`) | Runner impede execução, grava placar e alarma; descoberta traduz falha para alarme rc2. (`bin/cron_run.sh:16,18,22,25,44`; `bin/encadeado.sh:48,52,59,61`) |
| rc75 do restart/deploy | Restart recusa por espera/trava; deploy propaga e grava a recusa. (`bin/reinicio_sob_admissao.sh:41,48`; `bin/deploy.sh:283,288`) | Agendado trata arquivo e rc das novas tentativas. (`bin/deploy_agendado.sh:99,107,110,111,113`) |
| `EXITS_NAO_RODOU` | Declaração Python `(70,75)`. (`app/config/crons.py:1430`) | Pipeline importa; medidor carrega a configuração. O shell mantém literais próprios, comparados pelo selo. (`app/core/management/commands/pipeline_placar.py:9,72`; `bin/crons_duracao.py:54,85`; `bin/tests/test_deploy_nao_mata_elo.sh:372,376,377`) |
| `placar.jsonl` | Runner grava `ini/fim/dur/exit`, incluindo retorno real do comando. (`bin/cron_run.sh:20,25`) | Pipeline, medidor, vigia, contador e selo. (`app/core/management/commands/pipeline_placar.py:65`; `bin/crons_duracao.py:73`; `app/core/services/crons_quebrados.py:30`; `bin/rc137_no_restart.sh:55`; `bin/tests/test_deploy_nao_mata_elo.sh:264`) |
| `crons_duracao.json` | Medidor calcula p95 ×1,5, piso30, preserva valores antigos sem nova amostra e grava JSON. (`bin/crons_duracao.py:57,90,94,107,111`) | `config/crons.py` lê `dur_s` para `DUR_MAX_S` e horários derivados. (`app/config/crons.py:146,154,159,160,241,248`) |
| `rodou/erros/alarmes/dur_max` | Agregação de `medidos`; `dur_max` é máximo, distinto do envelope p95. (`app/core/management/commands/pipeline_placar.py:74,76,78,81,82`) | `placar` decide situação/dependência e o command apresenta. (`app/core/management/commands/pipeline_placar.py:88,94,96,102,109,125,129`) |

**Leitor não migrado confirmado:** `contrato_deploy.sh` ainda exige linha começando por `docker restart`; agora ela começa por `timeout`. Isso acusa as três cascas como ausentes e alimenta o placar de contrato. (`bin/contrato_deploy.sh:23,24,25`; `bin/reinicio_sob_admissao.sh:63`; `bin/placar_code.sh:130,131`)

Os dois leitores produtivos identificados de duração/“rodou” foram migrados. O vigia mantém 70/75 como quebra deliberadamente; o placar já trata saída desconhecida do contador como contribuição não zero. (`bin/crons_duracao.py:85`; `app/core/management/commands/pipeline_placar.py:72`; `app/core/services/crons_quebrados.py:46`; `bin/placar_code.sh:459,461`)

Duas ressalvas sobre P6:

- A espera da admissão termina **antes** de `T0`; portanto, os 900s de espera inflarem `dur` não são demonstrados pelo runner atual. As fixtures introduzem essas durações artificialmente. (`bin/cron_run.sh:16,17,24,25`; `bin/tests/test_deploy_nao_mata_elo.sh:380,381,382`)
- Se um envelope antigo estivesse contaminado e não surgisse amostra elegível, o valor antigo permaneceria. Contaminação efetiva em produção: **[nao verificado]**. (`bin/crons_duracao.py:94,107`)

3. **Relógio, laços, fd e ordem**

- **Relógio:** a política de espera lê `date` dentro de `_decorrido`, sem relógio monotônico ou instante recebido. Isso permite prolongamento por retrocesso. Pipeline e medidor também leem relógio, mas em seus pontos de entrada; no pipeline, hora e minuto vêm de duas leituras distintas. (`bin/reinicio_sob_admissao.sh:35,36`; `bin/crons_duracao.py:66,108`; `app/core/management/commands/pipeline_placar.py:122,124`)
- **Teto:** além do passo sem limitação e das varreduras sem prazo, o agendado espera `/tmp/regua.lock` sem `-w`. O limite de tentativas não fornece prazo absoluto para o fluxo inteiro. (`bin/reinicio_sob_admissao.sh:38,42,51`; `bin/deploy_agendado.sh:99,105`)
- **fd:** o comando do cron e os Docker do restart/subida não herdam fd8. A afirmação do comentário “filho nenhum” é mais ampla que a implementação: auxiliares da varredura e `date` ainda o herdam. (`bin/admissao_cron.sh:156`; `bin/reinicio_sob_admissao.sh:42,57,60,62,63,66`)
- **Janela varredura→flock:** está coberta entre os participantes migrados. Cron publica sob flock; deploy adquire o mesmo flock e varre novamente antes do restart. Não há publicação participante entre essa segunda varredura e o ato. Escritor produtivo externo a esse protocolo: **[nao verificado]**. (`bin/admissao_cron.sh:149,153,156`; `bin/reinicio_sob_admissao.sh:41,42,43,63`)

4. **Mordida do selo**

**Sim: por inspeção, o caso 8 distingue os dois mutantes perguntados; execução integral desses mutantes permanece [nao verificado].**

- Remover `[ "$rc" = 75 ] || break` faz a fixture `rc1` alcançar a mensagem falsa “rc75 sem a recusa”. O teste proíbe essa mensagem. **A mordida é no diagnóstico:** sem arquivo, o `while` já impediria terceira chamada. (`bin/deploy_agendado.sh:99,110,111`; `bin/tests/test_deploy_nao_mata_elo.sh:179,210`)
- Trocar a comparação de id dentro de `_recusou` por `true` aceita `id=outro` no nosso nome de arquivo e produz chamadas extras; o teste exige uma chamada. Trocar somente a chamada `_recusou` posterior ao rc75 por `true` conserva a proteção do `while` e não é distinguido pelos cenários atuais. (`bin/deploy_agendado.sh:87,99,111`; `bin/tests/test_deploy_nao_mata_elo.sh:180,213,214`)

| Caso novo/ampliado | Mordida e limite |
|---|---|
| **3/11** | Ataca classificação pela fonte e intervalo entre execs; testa lista Python e opções com valor. Não cobre os formatos/indireções de P1. (`bin/tests/test_deploy_nao_mata_elo.sh:103,295,300,304,306,308`) |
| **8** | Ataca argv, argv vazio, identidade, recusa persistente e rc incompatível. O trecho real executado começa **dentro** do ramo de recusa; não verifica sua seleção pelo rc recebido. (`bin/tests/test_deploy_nao_mata_elo.sh:191,200,203,207,210,214,217,220`) |
| **9** | Ataca fontes ausentes, restart ilegível e início sem fim; não testa placar truncado, fim invertido ou fallback divergente do escritor. (`bin/tests/test_deploy_nao_mata_elo.sh:242,245,248,251,253`; `bin/rc137_no_restart.sh:44,53,63`) |
| **10** | Adapta o reconhecimento da linha com `timeout`; mede somente comandos/encadeados com exit0/2, não todo RAW agora classificado pela fonte. Medida produtiva atual: **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:258,262,267,275`) |
| **12** | Ataca soma de sleeps e passo zero. Não testa passo acima do teto, varredura lenta ou relógio retrocedendo. (`bin/tests/test_deploy_nao_mata_elo.sh:60,324,328,333`) |
| **13** | Ataca fd no Docker, restart lento e registro na falha. O fake vira `sleep`, que não exercita resistência a TERM; não há fixture de registro inicial não gravável. (`bin/tests/test_deploy_nao_mata_elo.sh:48,51,338,342,348`) |
| **14** | Ataca retirada da segunda varredura, mas presume por sleeps que o deploy chegou ao flock; falta confirmação dessa fase. (`bin/tests/test_deploy_nao_mata_elo.sh:355,357,360,363,367`) |
| **15 e testes Django** | Atacam inclusão de 70/75 nas métricas e preservação do alarme. Não distinguem comando realmente executado que retorna esses códigos, nem leitor que copie literalmente a tuple atual. (`bin/tests/test_deploy_nao_mata_elo.sh:375,380,392`; `app/chamados/tests/test_contract_crons.py:207,221,223,240`) |

**Mutante plausível sobrevivente:** manter a chamada em `deploy.sh`, mas trocar `_rr=$?` por `_rr=0`. O deploy ignoraria a recusa; o selo continuaria exercitando o restarter isolado e o corpo extraído da recusa. Sobrevivência integral executada: **[nao verificado]**. (`bin/deploy.sh:282,283,290`; `bin/tests/test_deploy_nao_mata_elo.sh:217,220`)

Outro sobrevivente previsto é permitir prosseguir quando falha a escrita inicial de `restarts.log`: os casos atuais verificam registros com diretório gravável, sem atacar esse pré-requisito. (`bin/reinicio_sob_admissao.sh:61`; `bin/tests/test_deploy_nao_mata_elo.sh:338,342,348`)

5. **Classes recorrentes da casa**

| Classe | Conferência |
|---|---|
| **Segundo juiz registrado** | Não identifiquei duplicação de juiz de negócio nos trechos novos: eles classificam admissão e filtram métricas. A constatação não certifica ausência global de duplicações. (`bin/admissao_cron.sh:55,83`; `app/core/management/commands/pipeline_placar.py:72`; `app/core/juizes.py:3,7`) |
| **Segundo escritor fora da porta** | Publicação produtiva de marca passa pela biblioteca; recusa e restart têm escritores identificados. A injeção direta da marca no caso14 pertence ao teste. (`bin/cron_run.sh:16`; `bin/encadeado.sh:48`; `bin/admissao_cron.sh:153`; `bin/deploy.sh:285`; `bin/reinicio_sob_admissao.sh:61`; `bin/tests/test_deploy_nao_mata_elo.sh:363`) |
| **Relógio fora da borda** | Há leitura direta dentro da política de espera; o teto continua dependente do relógio civil. (`bin/reinicio_sob_admissao.sh:35,36,46`) |
| **Derivação paralela da fonte única** | **Procede:** 660 é derivação fixa de prazos que o escritor pode alterar. Os códigos shell/Python também são declarações separadas, embora a igualdade seja cobrada pelo selo. (`bin/rc137_no_restart.sh:13,20`; `bin/reinicio_sob_admissao.sh:25,66`; `app/config/crons.py:1430`; `bin/tests/test_deploy_nao_mata_elo.sh:376,377`) |
| **Selo sem mordida** | Persistem lacunas de integração, proveniência e falha de registro descritas acima. (`bin/tests/test_deploy_nao_mata_elo.sh:217,338,380`) |
| **Ordem de trava** | A publicação e a segunda varredura cobrem a janela solicitada; o prazo de espera continua incompleto. (`bin/admissao_cron.sh:149,153`; `bin/reinicio_sob_admissao.sh:41,42,51`) |
| **Pouso antes do pré-requisito** | O código coloca registro antes do restart e migration antes da chamada ao restarter. Suite inteira verde, leitura final e pouso efetivo: **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:61,63`; `bin/deploy.sh:124,282`; `CLAUDE.md:369,372,373`) |

6. **D1..D4**

- **D1 continua válido para o cenário descartado:** recusa superada pode terminar com sucesso; uma fatia posterior bem-sucedida não apaga falha anterior. Ressalva: `PIOR` guarda o **último não zero**, não máximo numérico ou severidade. (`bin/deploy_agendado.sh:106,114,123`)
- **D2 continua condicional à conduta declarada:** código não impede agendamento antes do pouso nem aplica janela aos retries. Cumprimento histórico do aval e ausência de novos agendamentos: **[nao verificado]**. (`bin/deploy_agendado.sh:50,55,99,105`)
- **D3 continua válido quanto à proteção do elo:** alarme falho não cria flag. Porém a repetição depende de novo disparo com rc ruim; não há retry incondicional do alarme perdido. (`bin/cron_run.sh:44,45`)
- **D4 precisa ser dividido.** Marca retida pelo vigia é conservadora, pois o trap limpa somente na saída. **Envelope morto com filho vivo não é conservador:** `/proc/<pid>` ausente torna a marca órfã, a varredura a remove e não procura o filho reparentado. Isso deixa um contraexemplo à proteção; ocorrência produtiva: **[nao verificado]**. (`bin/cron_run.sh:19,45,50`; `bin/admissao_cron.sh:108,124,131,134`)

## 02/11 23:38 -- HISTORICO (mais de 24 h) · sessao 74d8250b82c6 · - · - · origem=Ronald (tmux)

P5/P6 têm cura dos casos citados, mas restam contraexemplos de classe.

- **P5 fontes ausentes:** `FileNotFoundError` vira desconhecido, e o shell devolve `?`, rc2; `placar_code.sh` já consome desconhecido como contribuição não zero ao total. (`bin/rc137_no_restart.sh:51`, `bin/rc137_no_restart.sh:56`, `bin/rc137_no_restart.sh:75`, `bin/placar_code.sh:459`)
- **P5 fonte ilegível ainda pode virar zero:** JSON inválido no placar é descartado. Executei só o Python real em memória, substituindo `open` por `StringIO`: restarts `100 200` com placar `{"exit":137,quebrou` imprime `0`. A validação de restart também aceita fim anterior ao início. (`bin/rc137_no_restart.sh:44`, `bin/rc137_no_restart.sh:60`)
- **Fonte única duplicada:** `SEM_FIM_S=660` copia a conta `120+3×180`, mas o escritor aceita `ADM_RESTART_TIMEOUT_S` arbitrário; restart com prazo maior, sem registro final, pode ter rc137 fora da janela considerada. Em memória, restart `100 -` com rc137 em epoch900 imprime zero. (`bin/rc137_no_restart.sh:20`, `bin/rc137_no_restart.sh:53`, `bin/reinicio_sob_admissao.sh:25`, `bin/reinicio_sob_admissao.sh:66`)
- **Registro antes do ato:** o escritor grava `ini -` e aborta se isso falhar; tanto falha do restart quanto falha de subida chamam `_fim`. Essa parte cura o caso citado. (`bin/reinicio_sob_admissao.sh:61`, `bin/reinicio_sob_admissao.sh:64`, `bin/reinicio_sob_admissao.sh:67`)

**Escritores/leitores:**

| Objeto | Escritor | Leitores |
|---|---|---|
| `restarts.log` | início/fim em `bin/reinicio_sob_admissao.sh:59` | contador em `bin/rc137_no_restart.sh:40`; selo em `bin/tests/test_deploy_nao_mata_elo.sh:343` |
| `placar.jsonl` | runner grava RC e duração em `bin/cron_run.sh:25` | duração em `bin/crons_duracao.py:73`; pipeline em `app/core/management/commands/pipeline_placar.py:65`; vigia em `app/core/services/crons_quebrados.py:30`; contador em `bin/rc137_no_restart.sh:55`; selo em `bin/tests/test_deploy_nao_mata_elo.sh:264` |
| `EXITS_NAO_RODOU` | tuple declarada em `app/config/crons.py:1430` | import do pipeline em `app/core/management/commands/pipeline_placar.py:9`; carga do medidor em `bin/crons_duracao.py:54` |
| RC70/75 de admissão | literais shell em `bin/admissao_cron.sh:147` | runner em `bin/cron_run.sh:16`; descoberta em `bin/encadeado.sh:48`, traduzida para2 em `bin/encadeado.sh:59` |
| envelope `crons_duracao.json` | medidor em `bin/crons_duracao.py:111`, acionado por `bin/crons.sh:13` | `_dur_medida` e `DUR_MAX_S` em `app/config/crons.py:146`, `app/config/crons.py:159` |
| agregado `rodou/erros/dur_max` | `medidos` em `app/core/management/commands/pipeline_placar.py:74` | `placar` em `app/core/management/commands/pipeline_placar.py:88` e apresentação em `app/core/management/commands/pipeline_placar.py:125` |

Não encontrei leitor produtivo de duração/“rodou” que preserve a interpretação anterior: ambos foram migrados; o vigia deliberadamente mantém70/75 como quebra, e o contador seleciona apenas137. (`bin/crons_duracao.py:85`, `app/core/management/commands/pipeline_placar.py:72`, `app/core/services/crons_quebrados.py:46`, `bin/rc137_no_restart.sh:64`)

**P6 não fecha a proveniência:** comando admitido também pode executar e devolver70/75; o runner sobrescreve RC com o retorno real e grava o mesmo formato. Os filtros então dizem que essa corrida não ocorreu. A regra “70/75 = não executou” não é garantida pelo escritor. (`bin/cron_run.sh:18`, `bin/cron_run.sh:20`, `bin/cron_run.sh:25`, `app/core/management/commands/pipeline_placar.py:72`, `bin/crons_duracao.py:85`)

Os valores shell e Python são duas declarações mantidas iguais pelo selo via `grep`, não uma única fonte lida pelo shell. (`bin/admissao_cron.sh:147`, `app/config/crons.py:1430`, `bin/tests/test_deploy_nao_mata_elo.sh:372`)

A duração antiga permanece quando não há amostra elegível; portanto eventual envelope já contaminado não seria necessariamente corrigido só por instalar o filtro. Existência de contaminação em produção: **[nao verificado]**. (`bin/crons_duracao.py:94`, `bin/crons_duracao.py:107`)

A espera da admissão ocorre antes de `T0`: a afirmação de que os900s entram na duração não é comprovada pelo runner atual; são valores fabricados nas fixtures. (`bin/cron_run.sh:16`, `bin/cron_run.sh:17`, `bin/cron_run.sh:25`, `bin/tests/test_deploy_nao_mata_elo.sh:380`)

**D3:** válido quanto à proteção do elo: alarme está fora da admissão e flag só nasce com sucesso do `docker exec`. O retry é condicional ao próximo RC ruim; não há repetição incondicional de um alarme perdido. (`bin/cron_run.sh:44`, `bin/cron_run.sh:45`)

**D4:** não manter inteiro. Marca retida pelo vigia é conservadora porque o trap EXIT só limpa após o vigia terminar. Porém envelope morto com filho vivo não é conservador: `/proc/<pid>` ausente torna a marca órfã e ela é apagada; não existe busca pelo filho reparentado, pois a árvore só é examinada após validar a marca viva. Ocorrência produtiva: **[nao verificado]**. (`bin/cron_run.sh:19`, `bin/cron_run.sh:45`, `bin/cron_run.sh:50`, `bin/admissao_cron.sh:108`, `bin/admissao_cron.sh:124`, `bin/admissao_cron.sh:134`)

O teste antigo `test_medicao_de_cron_nao_expira.sh` não quebra pela nova constante: importa o medidor real e substitui apenas caminhos do placar/saída; `CRONS_PY` segue apontando à configuração real. A fixture sem `exit` passa como `None`. (`bin/tests/test_medicao_de_cron_nao_expira.sh:16`, `bin/tests/test_medicao_de_cron_nao_expira.sh:44`, `bin/tests/test_medicao_de_cron_nao_expira.sh:47`, `bin/crons_duracao.py:31`, `bin/crons_duracao.py:85`)

## 02/11 23:38 -- HISTORICO (mais de 24 h) · sessao 98ad748d0fa4 · - · - · origem=Ronald (tmux)

Achados por inspeção estática; execução da suíte e dos mutantes [nao verificado], pois o selo cria e remove arquivos em `bin/tests/test_deploy_nao_mata_elo.sh:40`.

- **Caso 8 mata a remoção de `[ "$rc" = 75 ] || break`.** Com o modo `rc1`, a segunda chamada retorna 1 sem arquivo (`bin/tests/test_deploy_nao_mata_elo.sh:179`). Sem a guarda de `bin/deploy_agendado.sh:110`, chega à mensagem falsa “rc 75 sem a recusa” de `bin/deploy_agendado.sh:111`; o teste a proíbe explicitamente em `bin/tests/test_deploy_nao_mata_elo.sh:210`. A mordida é no diagnóstico; o teste de arquivo já impediria uma terceira chamada (`bin/deploy_agendado.sh:99`).

- **Caso 8 mata substituir por `true` a comparação de ID dentro de `_recusou`.** A fixture põe `id=outro` no arquivo com nosso nome (`bin/tests/test_deploy_nao_mata_elo.sh:180`); sem a comparação em `bin/deploy_agendado.sh:87`, entra nas novas tentativas em `bin/deploy_agendado.sh:99`, contrariando a exigência de exatamente uma chamada em `bin/tests/test_deploy_nao_mata_elo.sh:214`. Se o mutante substituir somente a chamada `_recusou` de `bin/deploy_agendado.sh:111` por `true`, a proteção do `while` ainda existe; esse mutante sobreviveria aos cenários atuais por inspeção (`bin/deploy_agendado.sh:99`; `bin/tests/test_deploy_nao_mata_elo.sh:203`).

- **Mutante relevante que sobrevive:** manter a chamada de `bin/deploy.sh:282`, mas trocar `_rr=$?` por `_rr=0`. O deploy ignoraria a recusa e continuaria pela prova, enquanto o selo exercita o restarter isoladamente e extrai somente o corpo do ramo de recusa do deploy real (`bin/tests/test_deploy_nao_mata_elo.sh:217`, `bin/tests/test_deploy_nao_mata_elo.sh:220`). Não há execução integrada do deploy real nesse caso; sobrevivência executada [nao verificado].

- **Outro mutante que sobrevive:** trocar o tratamento de falha da escrita inicial por `|| true` em `bin/reinicio_sob_admissao.sh:61`. Os testes verificam registro em execução normal, timeout e falha do Docker, mas não simulam `restarts.log` não gravável (`bin/tests/test_deploy_nao_mata_elo.sh:338`, `bin/tests/test_deploy_nao_mata_elo.sh:342`, `bin/tests/test_deploy_nao_mata_elo.sh:348`). Sobrevivência executada [nao verificado].

- **Caso 11:** distingue fonte de árvore e ataca o intervalo entre dois execs; o intervalo usa `sleep 1.8`, sem barreira confirmando a fase exata (`bin/tests/test_deploy_nao_mata_elo.sh:295`, `bin/tests/test_deploy_nao_mata_elo.sh:299`). A fixture de lista Python e opções com valor verifica os exemplos citados (`bin/tests/test_deploy_nao_mata_elo.sh:304`, `bin/tests/test_deploy_nao_mata_elo.sh:306`, `bin/tests/test_deploy_nao_mata_elo.sh:308`).

- **Caso 12:** ataca a contagem de sleeps com sleep multiplicado por três e verifica PASSO exatamente `0`; não cobre varredura lenta, espera da trava consumindo orçamento, relógio retrocedendo ou PASSO enorme (`bin/tests/test_deploy_nao_mata_elo.sh:324`, `bin/tests/test_deploy_nao_mata_elo.sh:328`, `bin/tests/test_deploy_nao_mata_elo.sh:333`).

- **Caso 13:** verifica fd 8 no Docker de restart/prova e timeout de um processo que responde a TERM; o fake usa `exec sleep`, portanto não cobre filho que ignore TERM (`bin/tests/test_deploy_nao_mata_elo.sh:48`, `bin/tests/test_deploy_nao_mata_elo.sh:51`, `bin/tests/test_deploy_nao_mata_elo.sh:339`, `bin/tests/test_deploy_nao_mata_elo.sh:342`). O código usa `timeout` sem `--kill-after` e aceita timeout arbitrário do ambiente (`bin/reinicio_sob_admissao.sh:25`, `bin/reinicio_sob_admissao.sh:63`).

- **Caso 14:** ataca a retirada da segunda varredura: injeta marca viva enquanto o deploy deveria aguardar a trava e exige restart posterior ao fim; a sincronização é por sleeps, sem confirmação de que o deploy chegou ao flock (`bin/tests/test_deploy_nao_mata_elo.sh:355`, `bin/tests/test_deploy_nao_mata_elo.sh:360`, `bin/tests/test_deploy_nao_mata_elo.sh:363`, `bin/tests/test_deploy_nao_mata_elo.sh:367`).

- **Caso 15:** morde deixar 70/75 entrarem na duração, mas não uma derivação paralela que copie literalmente `(70,75)` no leitor: a fixture só usa os valores atuais (`bin/tests/test_deploy_nao_mata_elo.sh:375`, `bin/tests/test_deploy_nao_mata_elo.sh:380`, `bin/tests/test_deploy_nao_mata_elo.sh:392`). Sobrevivência executada [nao verificado].

- **Leitor não migrado confirmado:** `bin/contrato_deploy.sh:23` só aceita linha começando em `docker restart`, enquanto agora começa em `timeout` (`bin/reinicio_sob_admissao.sh:63`); acusa as três cascas ausentes em `bin/contrato_deploy.sh:24`. É consumido pelo placar em `bin/placar_code.sh:130`; bloqueio da régua por esse contrato [nao verificado]. O selo novo, por sua vez, entra no glob de selos da régua (`bin/regua.sh:138`).

- **D1 continua válido quanto ao cenário citado:** recusa superada pode resultar em sucesso da fatia, e sucesso de fatia posterior não limpa `PIOR`, pois a atribuição exige rc não zero (`bin/deploy_agendado.sh:114`, `bin/deploy_agendado.sh:123`). “PIOR” significa último rc não zero, não máximo numérico (`bin/deploy_agendado.sh:114`).

- **D2 continua sendo condicional de conduta:** o código permite agendar sem essa janela e repetir sem nova verificação horária (`bin/deploy_agendado.sh:50`, `bin/deploy_agendado.sh:55`, `bin/deploy_agendado.sh:99`). Cumprimento histórico de “não criar agendamento antes do pouso” [nao verificado].

## 02/11 23:37 -- HISTORICO (mais de 24 h) · sessao 912c78ee6f28 · - · - · origem=Ronald (tmux)

- **P1 cura a sentinela, mas não a classe completa.** A marca nasce `casca=1` pelo script literal e cobre os quatro passos da sentinela (`bin/admissao_cron.sh:150`; `mensageria/bin/sentinela_diario.sh:14`). O reconhecimento continua lexical, por linha, e segue somente caminhos absolutos, um nível (`bin/admissao_cron.sh:58`; `bin/admissao_cron.sh:87`). Contraexemplos: `docker exec \` + quebra de linha + `saas_core ...`, `docker compose -f compose.yml exec saas_core ...`, chamadas indiretas relativas e container por variável; fora do exec vivo, a árvore não segura (`bin/admissao_cron.sh:64`; `bin/admissao_cron.sh:66`; `bin/admissao_cron.sh:124`). Confirmei somente a função de reconhecimento, sem gravações: as duas primeiras formas retornam `casca=0`; ocorrência delas em produção [nao verificado].

- **O censo pinado não prova ausência de indireção.** Pina `vigia_logins.sh=0` (`bin/tests/test_deploy_nao_mata_elo.sh:312`), embora o RAW execute `vigia_logins.sh --auto` (`app/config/crons.py:284`) e seu ramo de recuperação chame `bin/deploy.sh --sem-migrate` por caminho relativo (`bin/vigia_logins.sh:71`). Esse deploy faz provas com `docker exec` depois de liberar a admissão (`bin/deploy.sh:290`; `bin/deploy.sh:298`; `bin/deploy.sh:314`). O ramo ter ocorrido em produção [nao verificado].

- **P2 não estabelece teto absoluto.** Substituiu soma de sleeps por tempo de calendário (`bin/reinicio_sob_admissao.sh:35`; `bin/reinicio_sob_admissao.sh:36`), mas permite `ADM_PASSO_S=3600` e dorme o passo completo mesmo com teto de 1 s (`bin/reinicio_sob_admissao.sh:19`; `bin/reinicio_sob_admissao.sh:21`; `bin/reinicio_sob_admissao.sh:51`). As varreduras também não têm timeout, e o sucesso da segunda varredura sai antes de conferir o tempo (`bin/reinicio_sob_admissao.sh:38`; `bin/reinicio_sob_admissao.sh:42`; `bin/reinicio_sob_admissao.sh:43`). Retrocesso do relógio pode prolongar a espera, pois usa `date +%s`, não relógio monotônico (`bin/reinicio_sob_admissao.sh:36`).

- **P3 fecha o fd para docker, mas não cura toda espera ilimitada.** Restart e provas fecham fd 8 (`bin/reinicio_sob_admissao.sh:63`; `bin/reinicio_sob_admissao.sh:66`). Entretanto `ADM_RESTART_TIMEOUT_S` aceita `0`, sem validação, e o `timeout` não tem escalada de TERM para KILL (`bin/reinicio_sob_admissao.sh:25`; `bin/reinicio_sob_admissao.sh:63`); comportamento concreto de processo ignorando TERM [nao verificado]. A promessa “filho nenhum” excede a implementação: a segunda varredura e os comandos `date` executam com fd 8 aberto (`bin/reinicio_sob_admissao.sh:42`; `bin/reinicio_sob_admissao.sh:60`; `bin/reinicio_sob_admissao.sh:62`).

- **A ordem de admissão cobre a janela citada em P7.** O cron adquire flock antes da publicação atômica da marca e fecha fd antes de executar o comando (`bin/admissao_cron.sh:148`; `bin/admissao_cron.sh:153`; `bin/admissao_cron.sh:156`; `bin/cron_run.sh:20`). O deploy adquire o mesmo flock e repete a varredura antes de prosseguir (`bin/reinicio_sob_admissao.sh:41`; `bin/reinicio_sob_admissao.sh:42`). Entre esses participantes, a marca não nasce depois da segunda varredura; escritores externos não participantes [nao verificado].

- **Mordida dos casos 11–14:** caso 11 cobre híbrido literal entre execs e algumas opções, mas não os formatos acima (`bin/tests/test_deploy_nao_mata_elo.sh:295`; `bin/tests/test_deploy_nao_mata_elo.sh:304`). Caso 12 usa `PASSO=1` e testa `PASSO=0`, não passo acima do teto (`bin/tests/test_deploy_nao_mata_elo.sh:60`; `bin/tests/test_deploy_nao_mata_elo.sh:328`; `bin/tests/test_deploy_nao_mata_elo.sh:333`). Caso 13 simula restart preso com `/usr/bin/sleep`, filho obediente, não processo resistente a TERM (`bin/tests/test_deploy_nao_mata_elo.sh:50`; `bin/tests/test_deploy_nao_mata_elo.sh:51`). Caso 14 deve matar a remoção integral da segunda varredura, mas sincroniza por sleeps, não confirmação de estado (`bin/tests/test_deploy_nao_mata_elo.sh:355`; `bin/tests/test_deploy_nao_mata_elo.sh:360`; `bin/tests/test_deploy_nao_mata_elo.sh:367`); execução do mutante [nao verificado].

- **Mutante plausível sobrevivente:** mudar apenas o default para `RESTART_TETO="${ADM_RESTART_TIMEOUT_S:-0}"`. O teste preso fornece explicitamente `2`, e os demais restarts falsos terminam imediatamente; nenhuma asserção cobra o default positivo (`bin/reinicio_sob_admissao.sh:25`; `bin/tests/test_deploy_nao_mata_elo.sh:51`; `bin/tests/test_deploy_nao_mata_elo.sh:342`). Sobrevivência executada [nao verificado].

- **D3/D4 permanecem conservadores no código inspecionado.** Falha do alarme não cria flag, permitindo novo disparo; o sucesso cria a flag (`bin/cron_run.sh:45`). A marca fica até o envelope sair, inclusive durante o vigia, por trap EXIT (`bin/cron_run.sh:19`; `bin/cron_run.sh:45`; `bin/cron_run.sh:50`), e um envelope vivo marcado `casca=1` segura (`bin/admissao_cron.sh:122`). Isso pode retardar deploy; ocorrência concreta em produção [nao verificado].

## 02/11 22:14 -- HISTORICO (mais de 24 h) · sessao e603127b50ca · E4CAL-L1-P3 · verificador · origem=Code

1. **Sim: o delta cura os itens 4 e 5 por inspeção dos testes. Execução verde: [nao verificado].** O teste real agora desliga o mock, congela o relógio durante a chamada inteira e passa `hoje=25/04/2026` ao censo; exige efetivamente `(fio_morto, decisao_do_dia_pendente) = (0, 1)`. (`app/chamados/tests/test_o261_vigia_fio.py:498`, `app/chamados/tests/test_o261_vigia_fio.py:503`, `app/chamados/tests/test_o261_vigia_fio.py:507`.)

   **O `freeze_time` cobre o relógio da porta curta.** `_agora` é uma variável local criada por `timezone.now()`, do `django.utils.timezone`, a cada montagem do contexto. A porta curta chama esse contexto dentro do congelamento; `hoje` deriva de `_agora`, e o mesmo instante segue para o D4. Não vem do parâmetro `hoje` do censo. (`app/colaboradores/services/calendario.py:22`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:141`, `app/colaboradores/services/calendario.py:579`.)

   Com o corte padrão 21 da empresa da fixture, em 25/04 a competência corrente começa em 21/04; o dia testado, 16/04, fica anterior a ela. Portanto, desaparece a contradição temporal apontada na volta 1. (`app/chamados/tests/test_o261_vigia_fio.py:26`, `app/chamados/tests/test_o261_vigia_fio.py:34`, `app/colaboradores/models.py:46`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`, `app/colaboradores/services/calendario.py:711`.)

   O item 5 ganha expectativa literal para **F = `('irregular', True)` na tela e `True` pela porta curta**. Isso cobre a lacuna da folga lavrada irregular; rejeição efetivamente executada de mutantes: [nao verificado]. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:67`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:75`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:77`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:83`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:86`.)

2. **Sim: F chega ao ramo lavrado do D4, com a fixture atual.** F é 19/08, criado com `trabalha=False`; `_fixar()`, chamado depois das demais preparações, grava diretamente `status_dia='irregular'`. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:38`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:40`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:54`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:100`.)

   Há um detalhe decisivo: **`dna.marcos={}` não encerra a busca de horários**. Como `if m` é falso, o código cai no template, que na fixture tem horário 07–19; o método de horários não decide trabalho/folga. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:68`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:89`, `app/escala/models.py:1099`, `app/escala/models.py:1108`, `app/escala/models.py:331`, `app/escala/models.py:343`.)

   Esse fim previsto já passou no relógio congelado em 17/09; a comparação temporal devolve encerrado. No D4, `em_curso=False` permite ler a coluna irregular e sair com `fonte='lavrado'` **antes** do ramo que classificaria a natureza como folga. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:32`, `app/escala/servico_jornada.py:92`, `app/ponto/turnos.py:2122`, `app/ponto/status_dia.py:255`, `app/ponto/status_dia.py:266`, `app/ponto/status_dia.py:268`, `app/ponto/status_dia.py:269`, `app/ponto/status_dia.py:291`.)

   **Limite da prova:** F verifica a leitura/oferta de uma coluna irregular fixada numa folga. Não verifica se a derivação produtiva da lavra produziria irregular para esses fatos, pois `_fixar()` escreve a coluna diretamente, enquanto a porta produtiva chama `status_da_lavra`/`status_do_admin`. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:100`, `app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:698`.)

3. **Para o passo 3 completo, descartar ambos apenas por serem preexistentes é insuficiente: retirar `eh_trabalho` amplia o conjunto de dias atingíveis pelos defeitos.** O código registra a retirada desse veto e mantém apenas último recurso + passado + status. O v2 cura o relógio do teste específico; a cadeia produtiva continua sem transportar `hoje` à porta. (`app/colaboradores/services/calendario.py:89`, `app/colaboradores/services/calendario.py:103`, `app/chamados/tests/test_o261_vigia_fio.py:504`, `app/chamados/services/vigia_fio.py:400`, `app/colaboradores/services/calendario.py:107`.)

   - **Exceção engolida — agravamento condicional do efeito:** uma falha depois de carregar a janela pode deixar `_perg_viva` vazio/parcial; ausência nesse mapa permite último recurso. Num dia antes vetado por `eh_trabalho=False/None`, com status que pede decisão, essa falha agora pode liberar oferta indevida. A retirada não demonstra maior probabilidade de exceção; demonstra maior alcance quando ela ocorre. Frequência real: [nao verificado]. (`app/colaboradores/services/calendario.py:436`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:451`, `app/colaboradores/services/calendario.py:711`, `app/colaboradores/services/calendario.py:103`.)

     Esse contraexemplo demonstra oferta indevida na tela/porta. Não implica automaticamente erro no contador do censo: pergunta viva já provoca desvio antes da consulta à porta. (`app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/colaboradores/services/calendario.py:115`, `app/chamados/services/vigia_fio.py:392`, `app/chamados/services/vigia_fio.py:400`.)

   - **Relógios distintos — também há ampliação condicional:** mantendo `censo(hoje=17/04)` e a porta em 25/04, o dia 16/04 pertence à competência corrente para o primeiro relógio e à anterior para o segundo. No universo sem vínculo da fixture, o veto antigo mascarava a divergência; após sua retirada, a porta pode oferecer e o censo contar decisão pendente. O v2 alinha os relógios nesse teste, sem corrigir o contrato produtivo. (`app/chamados/tests/test_o261_vigia_fio.py:40`, `app/chamados/tests/test_o261_vigia_fio.py:122`, `app/ponto/janelas.py:24`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:440`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:401`, `app/chamados/tests/test_o261_vigia_fio.py:503`.)

4. **Escritores e leitores do que muda ou passa a ser exercitado:**

   | Objeto | Quem escreve/publica | Quem lê |
   |---|---|---|
   | Tempo do teste real | O teste fixa o relógio e fornece `hoje`. (`app/chamados/tests/test_o261_vigia_fio.py:503`) | Censo lê `hoje`; contexto lê `timezone.now()`; D4 recebe `_agora`. (`app/chamados/services/vigia_fio.py:313`; `app/colaboradores/services/calendario.py:140`; `app/colaboradores/services/calendario.py:579`) |
   | Célula/status de F | `_celula` cria a célula e `_fixar` grava a coluna. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:90`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:100`) | D4 lê a coluna; calendário publica o retorno; golden lê status e oferta. (`app/ponto/status_dia.py:266`; `app/colaboradores/services/calendario.py:637`; `app/colaboradores/services/calendario.py:699`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:67`) |
   | `resolver_dia_oferecido` | Calendário carimba o resultado do predicado em memória. (`app/colaboradores/services/calendario.py:742`) | Porta curta e grade leem o campo; vigia lê a porta, compartilhada por censo e alcance. (`app/colaboradores/services/calendario.py:115`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:289`; `app/chamados/services/vigia_fio.py:400`; `app/chamados/services/vigia_fio.py:610`) |
   | Invariante do MAPA | Fonte é o contrato no módulo; gerador lê a inscrição e escreve o Markdown. (`app/colaboradores/services/calendario.py:12`; `bin/gerar_mapa.py:43`; `bin/gerar_mapa.py:110`; `bin/gerar_mapa.py:119`) | O artefato agora corresponde ao predicado; execução efetiva da regeneração e consumidores do Markdown: [nao verificado]. (`app/docs/MAPA/calendario-dia-da-tela.md:12`; `app/colaboradores/services/calendario.py:103`) |

   Os consumidores indiretos do contexto incluem view, drawer e relatório de furos. (`app/colaboradores/views.py:1946`, `app/colaboradores/services/drawer.py:121`, `app/relatorios/furos.py:195`.)

## 02/11 22:13 -- HISTORICO (mais de 24 h) · sessao 66015e8d000e · - · - · origem=Ronald (tmux)

A triagem precisa separar **delta v2** de **passo 3**: o código registra que P3 retirou o veto `eh_trabalho`, deixando último recurso + passado + status; portanto ser preexistente não basta para descartar agravamento por ampliação do domínio. (`app/colaboradores/services/calendario.py:89`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:620`.)

- **Exceção engolida: há agravamento condicional do efeito.** `_ini_comp_atual` é preenchido antes da consulta/julgamento das perguntas; uma exceção posterior é capturada, deixando `_perg_viva` vazio/parcial. O cálculo interpreta ausência no mapa como permissão. Para um dia anteriormente vetado por `eh_trabalho=False/None`, passado, de competência anterior e com status `irregular`, esse estado passa a produzir oferta; antes a perna removida ainda a barrava. Isso demonstra ampliação dos casos atingidos, **não aumento da probabilidade da exceção**; frequência real [nao verificado]. (`app/colaboradores/services/calendario.py:436`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:451`, `app/colaboradores/services/calendario.py:711`, `app/colaboradores/services/calendario.py:103`.)

  O contraexemplo seguro é a **oferta na tela/porta curta**. Não atribuir automaticamente essa mesma falha ao contador do vigia: `censo` já desvia perguntas vivas antes de consultar a oferta. (`app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/colaboradores/services/calendario.py:115`, `app/chamados/services/vigia_fio.py:392`, `app/chamados/services/vigia_fio.py:400`.)

- **Relógios distintos: também amplia os dias cujo resultado pode divergir.** A fixture documenta exatamente um novo universo: sem vínculo, anteriormente `eh_trabalho=None`, agora oferecendo `sem_certidao`. O dia é 16/04 e o `_censo` tem default 17/04; a porta, sem parâmetro temporal, chama contexto que lê `timezone.now()`. (`app/chamados/tests/test_o261_vigia_fio.py:26`, `app/chamados/tests/test_o261_vigia_fio.py:40`, `app/chamados/tests/test_o261_vigia_fio.py:122`, `app/colaboradores/services/calendario.py:107`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:140`.)

  Contraexemplo estático: manter `censo(hoje=17/04)` e porta em 25/04, corte 21. Em 17/04, 16/04 está na competência corrente e não tem último recurso; em 25/04, fica antes de 21/04 e pode oferecê-lo. Antes o veto de vínculo mascarava isso nesse universo; agora o vigia pode classificar como decisão pendente. O v2 cura o teste específico usando 25/04 em ambos, mas a API continua sem transportar `hoje`. (`app/colaboradores/models.py:46`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:711`, `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:401`, `app/chamados/tests/test_o261_vigia_fio.py:503`.)

Escritor/leitores produtivos: calendário publica `status` lido do D4, constrói `pode_ultimo_recurso` e carimba `resolver_dia_oferecido`; porta curta e grade leem esse carimbo; `dono_fora_do_fio` lê a porta e serve `censo`/`censo_alcance`. (`app/colaboradores/services/calendario.py:636`, `app/colaboradores/services/calendario.py:699`, `app/colaboradores/services/calendario.py:711`, `app/colaboradores/services/calendario.py:742`, `app/colaboradores/services/calendario.py:115`, `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:610`.)

## 02/11 22:12 -- HISTORICO (mais de 24 h) · sessao b98dec301141 · - · - · origem=Ronald (tmux)

**F chega ao ramo lavrado, por inspeção estática.**

- O R8 define F=19/08/2026, chama `_celula(... trabalha=False, status='irregular')` e aplica `_fixar()` depois das batidas dos demais casos. `_celula` grava `trabalha=False`, `dna={'marcos': {}}`, associa a geradora e guarda o status desejado; `_fixar` escreve diretamente `status_dia` e `status_rotulo`. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:38`, `:40`, `:54`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:87`, `:89`, `:90`, `:91`, `:92`, `:97`, `:100`.)

- **Detalhe decisivo:** `{}` em `dna.marcos` não significa “sem previsto” neste código: `if m` falha e `EscalaColaborador.marcos_do_dia` cai no template. O template da fixture fornece 07–19 e seu método não decide trabalho/folga; assim F ganha fim previsto em 19/08 às 19h. (`app/escala/models.py:1099`, `:1100`, `:1108`, `:326`, `:331`, `:343`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:66`, `:68`; `app/escala/servico_jornada.py:68`, `:69`, `:81`, `:89`, `:92`, `:93`.)

- A geradora está dentro da vigência: vínculo começa em D−40, com D=17/09, portanto antes de F; a seleção por geradora devolve esse vínculo quando a data está na vigência. Com relógio congelado em 17/09, o fim de F já passou e `_dia_encerrado_motivo_puro` retorna `None`; a carga usa esse mesmo julgamento. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:46`, `:82`, `:83`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:32`; `app/escala/alimentacao.py:237`, `:247`, `:250`, `:252`; `app/ponto/turnos.py:2119`, `:2122`, `:2164`, `:3004`.)

- Não há batida de F na fixture; as batidas criadas são B e E. No D4, motivo `None` dá `em_curso=False`; a coluna `irregular` entra em `if not em_curso and cel is not None and cel.status_dia`, e o `continue` impede alcançar o ramo de natureza/folga. O teste portanto exercita o ramo `fonte='lavrado'`, embora não faça asserção explícita sobre `fonte`. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:40`, `:43`, `:44`, `:47`, `:48`; `app/ponto/status_dia.py:252`, `:253`, `:255`, `:266`, `:267`, `:268`, `:269`, `:275`, `:291`.)

- A tela recebe o D4 e publica o status sem transformação; o predicado oferece para irregular/passado/último recurso. Os golden novos exigem `('irregular', True)` na tela e `True` na porta curta. Logo a lacuna do item 5 foi coberta no código do teste. Execução/verde e rejeição efetiva de mutantes: **[nao verificado]**. (`app/colaboradores/services/calendario.py:579`, `:636`, `:637`, `:699`, `:94`, `:103`, `:104`, `:711`, `:742`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:67`, `:75`, `:77`, `:83`, `:86`.)

**Escritores/leitores úteis à consolidação:**

- Escritor de fixture: `_celula` cria célula/geradora e `_fixar` escreve coluna; leitores: `CargaDaJanela` carrega as células e D4 lê `cel.status_dia`; calendário publica; golden lê tela/porta curta. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:90`, `:91`, `:100`; `app/ponto/turnos.py:2943`, `:2946`, `:2947`, `:2970`, `:2972`; `app/ponto/status_dia.py:251`, `:266`; `app/colaboradores/services/calendario.py:699`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:67`, `:86`.)

- Escritor produtivo do status: porta `_status_alvo` chama `status_da_lavra`/`status_do_admin`, `_aplicar_status` atribui coluna; a fixture contorna essa derivação ao fixar coluna diretamente. Assim F prova leitura/oferta de uma coluna irregular numa folga, sem provar que `status_da_lavra` produziria irregular para esses fatos. (`app/ponto/portas/celula.py:693`, `:694`, `:695`, `:698`, `:701`, `:705`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:100`.)

- Leitores produtivos diretos da oferta: porta curta lê o campo, template lê com `pode_veredito`, vigia chama porta curta. (`app/colaboradores/services/calendario.py:112`, `:115`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:280`, `:289`.)

- Mapa novo corresponde ao invariante já publicado no módulo. (`app/docs/MAPA/calendario-dia-da-tela.md:12`; `app/colaboradores/services/calendario.py:12`, `:13`, `:14`.)

## 02/11 22:09 -- HISTORICO (mais de 24 h) · sessao 42eeb25ed201 · E4CAL-L1-P3 · verificador · origem=Code

1. **Predicado literal: procedente.** São exatamente `pode_ultimo_recurso` + `data < hoje` + status em `('falta', 'irregular', 'aberto', 'sem_certidao')`, sem consultar `eh_trabalho`. A porta curta monta o calendário e lê seu resultado, sem repetir a regra. (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:115`.)

   **“Pela lavra” não significa exigir coluna preenchida.** Sem status lavrado, o D4 ainda decide: encerrado com batida → `sem_certidao`; sem batida, ausência descontável → `falta`, ausência abonada → `ausencia`, folga/feriado → `folga`, trabalho encerrado → `sem_certidao`, nenhum fato → `sem_dados`. Portanto, dias sem lavra podem ganhar ou perder a oferta pela regra central, conforme previsto no COMPLEMENTO. (`app/ponto/status_dia.py:270`, `app/ponto/status_dia.py:273`, `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:290`, `app/ponto/status_dia.py:295`, `app/ponto/status_dia.py:297`.)

   **Outro caminho de ganho/perda: procedente, preexistente ao patch fornecido.** O bloco de último recurso engole exceções. Se falhar antes de obter a competência, `_ini_comp_atual=None` elimina a oferta; se falhar depois, durante a leitura/julgamento das perguntas, `_perg_viva` pode permanecer vazio ou parcial e liberar indevidamente um dia com pergunta viva respondida. Ocorrência em produção: [nao verificado]. (`app/colaboradores/services/calendario.py:436`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:451`, `app/colaboradores/services/calendario.py:711`.)

2. **Escritores e leitores: contrato identificado.**

   | Campo lido | Quem publica no calendário | Fonte |
   |---|---|---|
   | `data` | Dicionário do dia | Janela escolhida pelo GET/modo, percorrida diariamente. (`app/colaboradores/services/calendario.py:154`, `:175`, `:581`, `:695`, `:727`.) |
   | `status` | `status = _st['status']`, publicado sem transformação | D4, alimentado pela mesma carga de células/turnos e pelos fatos A2. (`app/colaboradores/services/calendario.py:480`, `:579`, `:636`, `:699`.) |
   | `pode_ultimo_recurso` | Dicionário do dia | `janela_atual(hoje, empresa)` e ausência de pergunta viva **com resposta**; a vitalidade vem de `pergunta_viva`. (`app/colaboradores/services/calendario.py:440`, `:447`, `:449`, `:711`.) |
   | `resolver_dia_oferecido` | Carimbo após montar os dias | Única chamada produtiva direta ao predicado. (`app/colaboradores/services/calendario.py:742`.) |

   A coluna persistida de status é escrita pela porta da célula, que chama `status_da_lavra` ou `status_do_admin`; o método do modelo delega à porta. (`app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:705`, `app/escala/models.py:1794`.)

   **Leitores produtivos diretos encontrados:** campo → porta curta e grade; porta curta → `dono_fora_do_fio`, que classifica como `decisao_do_dia_pendente`. Esse último caminho é compartilhado por `censo` e `censo_alcance`. (`app/colaboradores/services/calendario.py:115`, `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:610`.)

   **Leitores indiretos:** view do calendário, drawer e relatório de furos montam o mesmo contexto. A grade exige também `pode_veredito`; o relatório inclui a grade com `only`, sem essa permissão, portanto não apresenta o botão. (`app/colaboradores/views.py:1946`, `app/colaboradores/views.py:1957`, `app/colaboradores/services/drawer.py:121`, `app/colaboradores/services/drawer.py:154`, `app/relatorios/furos.py:195`, `app/templates/relatorios/furos.html:42`, `app/templates/colaboradores/partials/_calendario_grade.html:125`.)

   **Testes que leem/injetam o contrato:** `test_o261_vigia_fio`, `test_o261e_f5_alcance`, `test_calendario_eh_trabalho_le_a_celula`, `test_e4cal_l1_r8_resolver_pela_lavra`, `test_calendario_mostra_o_carro` e a fixture de `test_ui_pilulas_quebram_linha`. (`app/chamados/tests/test_o261_vigia_fio.py:478`, `:897`, `:920`; `app/chamados/tests/test_o261e_f5_alcance.py:66`, `:214`, `:322`, `:374`; `app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:65`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:59`, `:79`; `app/colaboradores/tests/test_calendario_mostra_o_carro.py:142`, `:176`, `:194`; `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:55`.)

   **Leitor dependente de `eh_trabalho` esquecido: não encontrado no censo textual.** Restam injeção deliberada de `False`/`None` e testes proibindo a chave; o calendário não a publica. Acesso por chave construída dinamicamente, inclusive JS/API: [nao verificado]. (`app/chamados/tests/test_o261_vigia_fio.py:910`, `app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:80`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:86`, `app/colaboradores/services/calendario.py:694`.)

   **Documentação desatualizada: procedente.** O mapa ainda descreve “dia de trabalho” como perna do predicado. (`app/docs/MAPA/calendario-dia-da-tela.md:12`.)

3. **Conjunto paralelo ao D4: não procedente como segundo juiz.** `STATUS_QUE_PEDEM_DECISAO` pertence ao juiz da **oferta**, em `calendario.py`; o D4 define o vocabulário e decide **qual status o dia tem**. São perguntas diferentes. O patch fornecido conserva a constante, e `_TT11` já aponta para esse predicado. (`app/colaboradores/services/calendario.py:94`, `app/ponto/status_dia.py:30`, `app/ponto/status_dia.py:49`, `app/ponto/status_dia.py:200`, `app/core/juizes.py:704`, `app/core/juizes.py:718`.)

4. **Mocks: isolamento legítimo, mas com perda de integração nos cenários mockados.** O par SIM/NÃO continua testando se o vigia respeita a resposta da autoridade; isso não demonstra o comportamento do juiz real nesses mesmos cenários. No alcance, o caso SIM também permanece mockado. (`app/chamados/tests/test_o261_vigia_fio.py:46`, `app/chamados/tests/test_o261_vigia_fio.py:481`, `app/chamados/tests/test_o261e_f5_alcance.py:66`, `app/chamados/tests/test_o261e_f5_alcance.py:214`.)

   **O novo caso real morde, mas tem relógio solto: procedente.** Ele desliga o mock e exige mudança efetiva dos contadores. A fixture realmente deixa `status_dia` nulo: lavra `furo` sem `fatos_do_status`, situação em que a porta devolve status nulo. (`app/chamados/tests/test_o261_vigia_fio.py:94`, `app/chamados/tests/test_o261_vigia_fio.py:496`, `app/chamados/tests/test_o261_vigia_fio.py:500`, `app/ponto/portas/celula.py:696`.)

   Entretanto, `_censo` usa **17/04/2026**, enquanto a porta curta lê a data real da máquina. Congelando também o calendário em 17/04, o dia 16/04 estaria na competência corrente e não ofereceria Resolver, contrariando a expectativa desse teste. É um contraexemplo estático à estabilidade do selo; execução dessa falha: [nao verificado]. (`app/chamados/tests/test_o261_vigia_fio.py:26`, `app/chamados/tests/test_o261_vigia_fio.py:122`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:711`, `app/ponto/janelas.py:60`.)

   Falta, nesses testes alterados, um caso de `censo_alcance` que retire o mock e use o juiz real. O R8 cobre separadamente a porta real, mas não sua integração com esse universo do vigia. (`app/chamados/tests/test_o261e_f5_alcance.py:66`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:74`.)

5. **RED e mutantes nomeados: mordida procedente por inspeção.** O R8 exige respostas literais para trabalho sem lavra, folga com batida sem lavra, irregular lavrado sem vínculo ativo, folga sem lavra em competência anterior e lavrado `ok`; repete os cinco pela porta curta. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:65`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:77`.)

   | Mutante | Asserção que o rejeita |
   |---|---|
   | P3a: `and dia.get('eh_trabalho')` | Positivos sem a chave passam a devolver falso. (`app/chamados/tests/test_o261_vigia_fio.py:889`, `:897`.) |
   | P3b: `and dia.get('eh_trabalho', True)` | `False` e `None` explícitos devem continuar oferecendo. (`app/chamados/tests/test_o261_vigia_fio.py:909`, `:910`.) |
   | P3c: republicar a chave | `assertNotIn('eh_trabalho', dia)`. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:86`.) |

   **Variantes sobreviventes aos casos P3: possível e demonstrável por inspeção.** O teste cruza todos os status com chave ausente, mas testa `False`/`None` somente com status `falta`. Um gate seletivo para `irregular` com `eh_trabalho=False` escaparia dessas asserções. Como a chave não é publicada atualmente, essa variante isolada não alteraria o calendário real. (`app/chamados/tests/test_o261_vigia_fio.py:889`, `app/chamados/tests/test_o261_vigia_fio.py:896`, `app/chamados/tests/test_o261_vigia_fio.py:909`, `app/colaboradores/services/calendario.py:694`.)

   A variante material — voltar a consultar a célula e vetar **folga lavrada irregular** — também não é exercitada pelos cinco golden: B é sem lavra; C usa `trabalha=True`. Sobrevivência na suíte inteira: [nao verificado]. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:39`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:49`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:87`.)

   Execuções históricas RED/verde e relatórios dos mutantes P3a/P3b/P3c: [nao verificado]. [nao verificado]

6. **Classes de reprovação anteriores:**

   | Classe | Veredito |
   |---|---|
   | Segundo juiz | **Não procedente no P3:** autoridade registrada e porta curta delegante. (`app/core/juizes.py:718`; `app/colaboradores/services/calendario.py:112`.) |
   | Segundo escritor fora da porta | **Não identificado no caminho alterado:** calendário publica o retorno do D4; persistência continua na porta. (`app/colaboradores/services/calendario.py:637`, `:699`; `app/ponto/portas/celula.py:705`.) |
   | Relógio dentro do predicado | **Não procedente:** recebe `hoje`; calendário deriva `hoje` de `_agora` e passa o mesmo instante ao D4. (`app/colaboradores/services/calendario.py:97`, `:140`, `:141`, `:579`, `:742`.) |
   | Relógios distintos na cadeia do vigia | **Procedente:** `censo(hoje=...)` não transmite esse tempo à porta curta. (`app/chamados/services/vigia_fio.py:313`, `:400`, `:289`; `app/colaboradores/services/calendario.py:107`, `:140`.) |
   | Derivação paralela | **Não procedente no P3:** status é lido do D4; fatos A2 são alimentação compartilhada. (`app/colaboradores/services/calendario.py:480`, `:579`, `:636`.) |
   | Selo/RED sem mordida | **Não como acusação geral:** há expectativas literais e casos negativos; permanece o defeito temporal do novo teste real. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:65`, `:95`; `app/chamados/tests/test_o261_vigia_fio.py:122`, `:499`.) |
   | Ordem de trava | **Nenhuma inversão introduzida identificada:** expressão pura e carimbo em memória. Concorrência de toda a cadeia: [nao verificado]. (`app/colaboradores/services/calendario.py:103`, `:742`.) |
   | Pouso antes do pré-requisito | **[nao verificado].** O comando permite completar status de competência entregue, mas sua existência não prova aplicação do D5 nem ordem de deploy. (`app/ponto/management/commands/lavrar_status_competencia.py:43`, `:56`.) |

   D5 aplicado na sombra/no ar, novos dias ≤50, comparação HEAD×L1 nas competências 06–09, hashes e ordem efetiva de pouso: [nao verificado]. [nao verificado]

7. **Contraexemplos além dos golden: não encontrei inversão causada pela retirada de `eh_trabalho` nas condições abaixo.**

   | Situação adicional, passado e com último recurso | Resultado pelo código |
   |---|---|
   | Desligado/sem escala ativa, D4 devolvendo `irregular` | **Oferece.** O predicado não tem condição de situação/vínculo. (`app/colaboradores/services/calendario.py:103`.) |
   | Antes da admissão, com célula e status lavrado `irregular` | **Oferece**, se o D4 chega ao ramo lavrado; esse ramo não consulta admissão. (`app/ponto/status_dia.py:266`; `app/colaboradores/services/calendario.py:103`.) |
   | Célula `trabalha=False`, lavrada `irregular` | **Oferece**, se chega ao ramo lavrado: a coluna precede a natureza sem lavra. (`app/ponto/status_dia.py:266`, `:275`; `app/colaboradores/services/calendario.py:94`.) |
   | Mesmo cadastro, lavrado `folga` ou `ok` | **Não oferece.** Esses status estão fora do conjunto. (`app/colaboradores/services/calendario.py:94`, `:104`.) |

   Esses resultados seguem a regra de oferta pelo status; os novos golden não cobrem desligado, antes da admissão com célula ou folga **lavrada irregular**. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:76`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:38`, `:49`.)

   **Há contraexemplo condicional na cadeia completa:** dia lavrado irregular, anterior à competência corrente, com pergunta viva respondida; se a consulta/julgamento dessa pergunta lançar exceção após carregar a janela, ele pode oferecer indevidamente. É o caminho preexistente do item 1, não efeito da remoção de `eh_trabalho`; ocorrência real: [nao verificado]. (`app/colaboradores/services/calendario.py:440`, `:447`, `:451`, `:711`.)

## 02/11 22:07 -- HISTORICO (mais de 24 h) · sessao 17f5c627a035 · - · - · origem=Ronald (tmux)

1. **Mocks: isolamento procedente, cobertura integral não demonstrada.** O padrão NÃO passa pela mesma costura que os testes já usavam; os casos SIM sobrepõem o patch, e a comparação SIM/NÃO continua mordendo a classificação do vigia (`app/chamados/tests/test_o261_vigia_fio.py:39-48`, `:472-489`). Na classe de alcance, o SIM também permanece mockado (`app/chamados/tests/test_o261e_f5_alcance.py:58-68`, `:213-218`). Integração real específica de `censo_alcance` não está demonstrada por esses testes; sobrevivência na suíte completa [nao verificado].

2. **Novo caso real tem mordida, mas relógio solto — procedente.** Ele desliga o patch e exige `fio_morto=0` / `decisao_do_dia_pendente=1` (`app/chamados/tests/test_o261_vigia_fio.py:491-501`). A fixture efetivamente deixa status nulo: cria célula e lavra `furo` sem `fatos_do_status` (`:94-96`); `_status_alvo` devolve `(None,None)` quando faltam fatos e muda o veredito (`app/ponto/portas/celula.py:686-698`). Contudo, `_censo` injeta 17/04/2026 (`app/chamados/tests/test_o261_vigia_fio.py:115-123`), enquanto a porta curta chama calendário sem instante (`app/colaboradores/services/calendario.py:107-115`), que lê `timezone.now()` (`:139-141`). Isso contradiz “nenhum caminho deste selo lê relógio real” (`app/chamados/tests/test_o261_vigia_fio.py:116`). Sob relógio 17/04/2026, 16/04 permanece na competência corrente e perde último recurso (`app/colaboradores/services/calendario.py:711-712`); falha executada [nao verificado].

3. **RED pela regra: procedente por inspeção.** R8 cobre trabalho sem lavra, folga com batida sem lavra, lavrado irregular sem vínculo ativo, folga sem lavra e lavrado ok; exige os pares literais status/oferta (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:33-72`). Repete os cinco pela porta curta real (`:74-81`), exige ausência da chave (`:83-86`) e mantém competência corrente sem oferta (`:88-96`). A fixture fixa explicitamente as colunas após as batidas (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:97-100`). Execução RED/verde e relatórios P3a/P3b/P3c [nao verificado].

4. **Mutantes nomeados: mordida estática procedente.**
   
   - P3a, `and dia.get('eh_trabalho')`: morre inclusive no caso positivo sem chave (`app/chamados/tests/test_o261_vigia_fio.py:888-897`).
   - P3b, `and dia.get('eh_trabalho', True)`: morre com False/None explícitos (`:905-910`).
   - P3c, republicar a chave: morre em R8 (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:83-86`) e no teste histórico (`app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:78-80`).

5. **Variantes além das três: lacuna procedente; sobrevivência completa [nao verificado].** O teste positivo percorre todos os status, sempre sem `eh_trabalho`; o teste False/None usa somente status padrão `falta` (`app/chamados/tests/test_o261_vigia_fio.py:888-910`). Portanto, não cobre o produto status × valor do campo. Um gate seletivo para célula `trabalha=False` **lavrada irregular** não é exercitado pelos novos golden: B é sem lavra/`sem_certidao`, D é `folga`, C usa `trabalha=True` por padrão (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:38-50`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:87-92`). Os novos casos também não montam desligado ou antes da admissão com célula; `_colab` cria ativo e não informa admissão (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:76-85`).

## 02/11 22:06 -- HISTORICO (mais de 24 h) · sessao 3fc40b9d8f18 · - · - · origem=Ronald (tmux)

1. **Segundo juiz: não procedente no passo 3.** A oferta já tem autoridade registrada como `_TT11` em `app/core/juizes.py:704` e `app/core/juizes.py:718`, com corte anterior documentado em `app/docs/CORTES.md:271`. A porta curta monta o calendário e lê o campo; não repete o predicado (`app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:115`).

2. **Constante paralela ao D4: não procedente como duplicação da mesma pergunta.** `STATUS_QUE_PEDEM_DECISAO` classifica status para a oferta (`app/colaboradores/services/calendario.py:94`); o D4 define o vocabulário e responde qual status o dia tem (`app/ponto/status_dia.py:30`, `app/ponto/status_dia.py:49`, `app/ponto/status_dia.py:200`). São perguntas distintas e o patch fornecido conserva a constante preexistente [nao verificado quanto ao histórico Git local, ausente].

3. **Segundo escritor de status: não identificado no caminho alterado.** O calendário recebe a resposta do D4 e atribui `status = _st['status']`, publicando-a no dicionário (`app/colaboradores/services/calendario.py:579`, `app/colaboradores/services/calendario.py:636`, `app/colaboradores/services/calendario.py:699`). A coluna persistida é atribuída em `_aplicar_status`, dentro da porta (`app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:705`), chamada pela lavra (`app/ponto/portas/celula.py:752`, `app/ponto/portas/celula.py:773`).

4. **Relógio fora da borda do predicado: não procedente.** `resolver_dia_oferecido` recebe `hoje` (`app/colaboradores/services/calendario.py:97`); o calendário lê `_agora` e deriva `hoje` uma vez para esse caminho (`app/colaboradores/services/calendario.py:140`), passa esse `_agora` ao D4 (`app/colaboradores/services/calendario.py:579`) e usa `hoje` ao carimbar a oferta (`app/colaboradores/services/calendario.py:742`). Há outra leitura de relógio no mesmo calendário, porém exclusivamente para prazo de desfazer batida (`app/colaboradores/services/calendario.py:227`, `app/colaboradores/services/calendario.py:231`).

5. **Derivação paralela do status: não procedente no passo 3.** O antigo gate por grade foi removido e o calendário lê o D4 (`app/colaboradores/services/calendario.py:620`, `app/colaboradores/services/calendario.py:636`). Os fatos A2 são carregados por autoridade e fornecidos ao D4 como alimentação (`app/colaboradores/services/calendario.py:480`, `app/colaboradores/services/calendario.py:579`); o D4 consome o mapa fornecido sem fallback (`app/ponto/status_dia.py:279`).

6. **Caminho de ganho/perda por outra regra: procedente, preexistente.** O último recurso depende da competência atual e de pergunta viva com resposta (`app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:711`). O bloco captura qualquer exceção: falha antes de obter a competência mantém `_ini_comp_atual=None` e elimina a oferta; falha depois de obter a competência pode deixar `_perg_viva` vazio/parcial e permitir oferta apesar de pergunta viva ainda não lida (`app/colaboradores/services/calendario.py:436`, `app/colaboradores/services/calendario.py:451`, `app/colaboradores/services/calendario.py:711`). Falha da A2, diferentemente, propaga (`app/colaboradores/services/calendario.py:488`).

7. **Ordem de trava: alteração nova não identificada no trecho examinado.** O predicado é expressão pura, a porta curta chama o calendário e o carimbo altera apenas o dicionário retornado (`app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:742`). Reprodução de concorrência ou prova dinâmica de toda a cadeia [nao verificado].

8. **Pré-requisito D5/pouso: não certificável.** O comando da cópia permite competência entregue, usa `completar_status=True` e oferece `--apply` explícito (`app/ponto/management/commands/lavrar_status_competencia.py:30`, `app/ponto/management/commands/lavrar_status_competencia.py:43`, `app/ponto/management/commands/lavrar_status_competencia.py:56`). O completador grava pela porta apenas quando `apply_` é verdadeiro (`app/ponto/services/cartorio.py:889`). Execução do D5 na sombra/no ar, contagens após D5, comparação HEAD×L1 nas competências 06–09, limites de novos dias, hashes e ordem efetiva de deploy [nao verificado]. O dado fornecido contém condições normativas, sem resultados dessas medições [nao verificado].

9. **Documentação local antiga não deve contrariar o aval anexado.** O ADR ainda afirma que competência entregue não se toca (`app/docs/ADR/0002-celula-e-o-dia.md:71`); o comando já documenta a emenda do COMPLEMENTO que permite completar seu status (`app/ponto/management/commands/lavrar_status_competencia.py:10`). A execução efetiva dessa emenda permanece [nao verificado].

## 02/11 22:06 -- HISTORICO (mais de 24 h) · sessao af1a8cc5bdcf · - · - · origem=Ronald (tmux)

1. **Escritores do contrato do predicado:** `contexto_calendario` publica `data` em `app/colaboradores/services/calendario.py:695`, `status` em `:699`, `pode_ultimo_recurso` em `:711` e, depois, o resultado do único predicado em `:742`. A data vem da janela escolhida pelos modos/GET (`:154`, `:175`, `:187`), começa em `data_ini` (`:581`) e avança diariamente (`:727`). O status vem exclusivamente do retorno do D4 consultado em `:579`, lido em `:636` e `:637`.

2. **Fonte de último recurso:** início da competência atual vem de `ponto.janelas.janela_atual(hoje, empresa)`, em `app/colaboradores/services/calendario.py:439`; perguntas são filtradas por dia, com resposta não vazia, e submetidas ao juiz `pergunta_viva`, em `:447` e `:449`. O campo exige data anterior ao início atual e ausência dessa pergunta viva em `:711`.

3. **Leitores produtivos diretos completos encontrados:** predicado chamado pelo carimbo em `app/colaboradores/services/calendario.py:742`; campo lido pela porta curta em `:115` e pelo template em `app/templates/colaboradores/partials/_calendario_grade.html:125`; porta curta chamada em `app/chamados/services/vigia_fio.py:289`. O vigia prioriza essa resposta e devolve `decisao_do_dia_pendente` em `:290`, antes das ausências e demais baldes (`:291`, `:294`, `:296`).

4. **Views, drawer e relatórios:** perfil monta contexto em `app/colaboradores/views.py:1946`, acrescenta permissão em `:1957` e renderiza em `:1962`; template inclui a grade em `app/templates/colaboradores/calendario_partial.html:62`. Drawer monta calendário em `app/colaboradores/services/drawer.py:121`, publica `cal`/permissão em `:153` e `:154`, e inclui a grade em `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`. Relatórios montam `cal` em `app/relatorios/furos.py:195`; `app/templates/relatorios/furos.html:42` inclui a grade com `only`, sem passar `pode_veredito`, portanto não desenha o Resolver condicionado por essa permissão em `_calendario_grade.html:125`.

5. **Leitores em testes:** juiz direto em `app/chamados/tests/test_o261_vigia_fio.py:894`, `:900`, `:908`; campo/template em `:920`; costura da porta curta em `:478`, juiz real exercitado em `:491`. Costuras de alcance em `app/chamados/tests/test_o261e_f5_alcance.py:66`, `:214`, `:322`, `:374`. Novos testes integram campo e porta curta em `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:59`, `:76`; testes anteriores leem campo em `app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:65` e `app/colaboradores/tests/test_calendario_mostra_o_carro.py:142`. Fixtures de apresentação injetam campo em `test_calendario_mostra_o_carro.py:176` e `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:55`.

6. **Apagamento de `eh_trabalho`: não encontrei leitor executável remanescente dependente da chave.** O predicado lê apenas três pernas em `app/colaboradores/services/calendario.py:103`; o dicionário publicado não contém a chave (`:694` até `:725`). As ocorrências executáveis restantes são a injeção proposital `False`/`None` para garantir independência em `app/chamados/tests/test_o261_vigia_fio.py:910` e as proibições da chave em `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:86` e `app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:80`. JS/API/JSON não apresentaram referências ao identificador na busca integral [nao verificado quanto a acesso dinâmico por chave construída].

7. **Documentação derivada ficou velha:** `app/docs/MAPA/calendario-dia-da-tela.md:12` ainda descreve o predicado com “dia de trabalho”; `app/docs/BACKLOG.md:1785` contém trecho histórico `dia.eh_trabalho`. Isso não é leitor executável, mas o mapa contradiz o contrato atual de `app/colaboradores/services/calendario.py:103`.

8. **Outro caminho que altera a oferta sem lavra, preexistente ao diff fornecido:** o `try` do último recurso engole qualquer exceção em `app/colaboradores/services/calendario.py:451`. Se falhar a janela, `_ini_comp_atual=None` inicial de `:436` elimina oferta em `:711`; se a janela já respondeu e falhar a carga/julgamento das perguntas, `_perg_viva` pode ficar vazio ou parcial (`:437`, `:450`), permitindo oferta que pergunta viva com resposta deveria impedir (`:712`). A ocorrência real desse erro não consta dos dados [nao verificado].

## 02/11 21:29 -- HISTORICO (mais de 24 h) · sessao 9f7ef7d7568c · E4CAL-L1-P1 · verificador · origem=Code

**1. O delta fecha o item 3 específico da v1.** A lavra ocorre em T0=12h; o D5 passa às 13h e repete às 14h. Regravar `julgada_em` ou `veredito_em` com o relógio dessas passadas passa a alterar a foto. O mesmo vale para os demais timestamps fotografados, inclusive `FechamentoMensal.atualizado_em`, que é `auto_now`. (`app/ponto/tests/test_e3_status_lavrado.py:31`, `:298`, `:309–331`; `app/ponto/models.py:479`)

Isso é conclusão estática sobre o teste. O resultado do mutante citado pela triagem e sua execução nesta revisão são **[não verificado]**. (`triagem_v1.txt:2`, anexo) [nao verificado]

**Há escapes fora dessa foto:**

- `Colaborador.atualizado_em` é `auto_now`, há colaborador no fixture e essa tabela não entra na foto. Um mutante hipotético que apenas atualizasse esse campo com o relógio escaparia da comparação; ocorrência dessa escrita no D5 atual **[não verificado]**. (`app/colaboradores/models.py:381`; `app/ponto/tests/test_e3_status_lavrado.py:170`, `:309–313`)
- `EventoCelula` também fica fora. Um mutante que inserisse outbox sem alterar os campos fotografados escaparia. No código atual, salvar somente status não emite outbox: apenas `veredito` e `ata` o despertam. (`app/ponto/tests/test_e3_status_lavrado.py:309–313`; `app/ponto/portas/celula.py:623`, `:634–642`)
- `LogAuditoria` fica fora e recebe, legitimamente, uma nova linha no ramo de status. Portanto, a foto não demonstra “nenhuma outra tabela escrita”. (`app/ponto/portas/celula.py:761–765`; `app/core/observ.py:134–138`)

A igualdade das fotos demonstra igualdade do **estado observado**; não detecta um `UPDATE` que conserve o valor nem escrita fora das tabelas/linhas fotografadas. (`app/ponto/tests/test_e3_status_lavrado.py:309–331`)

**2. Os descartes têm alcances diferentes.**

**`veredito_via=NULL`: o defeito estrutural continua existindo.** Os dois gates de atualidade não exigem via; `_completar_status` chama a porta com `via='cartorio'`. Com via nula, falha a igualdade exigida pelo ramo somente status: a porta escreve `veredito_via`, `veredito_em`, veredito/protestos e emite outbox por incluir `veredito` nos campos. (`app/ponto/services/cartorio.py:1186–1189`, `:1345–1348`, `:889–891`; `app/ponto/portas/celula.py:753–775`, `:638–642`)

**Pode deixar de bloquear este apply delimitado, sem considerar o defeito corrigido.** A triagem relata sombra sobre as 35.436 células do universo exato, com zero mudanças fora do status, incluindo comparação de via e timestamp. Isso é evidência relatada no anexo; o conteúdo dos scripts e sua execução são **[não verificado]**. (`triagem_v1.txt:4`, anexo)

Para essa evidência sustentar o descarte, precisaria ser verdade:

- A sombra executou o caminho real de `--apply`, inclusive a porta, sobre o mesmo universo e estado destinados ao pouso. O dry sozinho não serve: ele não chama a lavra. (`app/ponto/services/cartorio.py:888–891`; equivalência da sombra **[não verificado]**)
- `CAMPOS_FORA` comparou cópias independentes dos valores persistidos antes/depois, sem perder células, incluindo efetivamente `veredito_via` e `veredito_em`. A implementação de `logs/sombra/e4cal_r8_d5_entregue.py:37` é **[não verificado]**.
- A prova de produção cobriu o mesmo alcance, os campos proibidos e os hashes exigidos; sua execução e resultado são **[não verificado]**. (`triagem_v1.txt:4`; `AVAL_L1_COMPLEMENTO.md:3`, `:10`, anexos)

**PAREI posterior não equivale a impedir ou desfazer a escrita.** O command não envolve o lote inteiro em uma transação; a porta tem atomicidade por chamada de salvamento. Se a prova apenas detecta a divergência depois, é necessário saber como a reversão prevista é executada. Esse comportamento dos scripts externos é **[não verificado]**. (`app/ponto/management/commands/lavrar_status_competencia.py:32–62`; `app/ponto/portas/celula.py:634–642`; `AVAL_L1_COMPLEMENTO.md:3`, anexo)

Assim: **sombra válida mais prova operacional válida sustentam o apply deste universo, deixando D5-VIA-NULA na fila; somente a menção a “zero” e “PAREI” não permite certificar isso nesta cópia.** (`triagem_v1.txt:4`, anexo; condições externas **[não verificado]**)

**O descarte da auditoria procede.** MUDA 1 restringe a alteração da célula ao status; a trilha é outra entidade e atende à exigência de registrar ação crítica. O ramo grava descrição e antes/depois por `evento`, que insere `LogAuditoria`. Isso não autoriza consumidor/outbox, que continuam excluídos do ramo somente status. (`AVAL_L1_COMPLEMENTO.md:3`, anexo; `CLAUDE.md:32–34`; `app/ponto/portas/celula.py:757–765`; `app/core/observ.py:134–138`)

**3. Escritores e leitores relevantes.**

| Campo/artefato | Quem escreve | Quem lê |
|---|---|---|
| `status_dia`, `status_rotulo` | Porta `lavrar_veredito` → `_aplicar_status` → salvamento. O teste os zera para simular legado. (`app/ponto/portas/celula.py:701–706`, `:753–760`; `app/ponto/tests/test_e3_status_lavrado.py:307`) | D5 verifica status existente; D4 lê status/rótulo; a porta compara o par. (`app/ponto/services/cartorio.py:882`; `app/ponto/status_dia.py:212–214`; `app/ponto/portas/celula.py:703`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | Porta de lavra; a precedência também escreve sua chave da ata. (`app/ponto/portas/celula.py:767–775`, `:811–822`) | D5 reutiliza veredito/protestos; a porta lê via/ata para escolher o ramo; `_foto` observa todos. (`app/ponto/services/cartorio.py:889`; `app/ponto/portas/celula.py:726–753`; `app/ponto/tests/test_e3_status_lavrado.py:309–311`) |
| `julgada_em`, `impressao`, `insumos_em` | Operação `carimbar`. (`app/ponto/portas/celula.py:800–807`) | Gates de atualidade e foto. (`app/ponto/services/cartorio.py:1186`, `:1345`; `app/ponto/tests/test_e3_status_lavrado.py:309–311`) |
| FM/ED fotografados | O fixture cria os registros. (`app/ponto/tests/test_e3_status_lavrado.py:299–302`) | `_foto` lê todas as colunas; D5 lê ED indiretamente pelo juiz de entregue. (`app/ponto/tests/test_e3_status_lavrado.py:312–313`; `app/ponto/management/commands/lavrar_status_competencia.py:43`; `app/folha/export.py:569–575`) |

**Não localizei selo que case o texto alterado dos ADRs 0002/0003 ou da docstring D5; ausência absoluta de leitores externos é [não verificado].** O selo da guarda da linha consulta AST, `_meta` e catálogo próprio, explicitamente “nunca texto”; o selo de naturezas do ADR 0003 está declarado como futuro/fila. (`app/escala/tests/test_d3_guarda_da_linha_celula.py:11–19`, `:167–173`; `app/docs/ADR/0003-celula-tres-naturezas.md:125–130`)

Existe leitor genérico de docstrings no MAPA, mas ele exige `@bolha`, ausente na docstring D5. O casamento documental da LEI-AKITA consultado lê `GLOSSARIO.md` e `PLANO-PADROES.md`. Portanto, não encontrei nesses mecanismos dependência das frases modificadas neste delta. (`app/core/mapa/contrato.py:45–48`, `:83–88`; `app/ponto/management/commands/lavrar_status_competencia.py:1–16`; `bin/tests/test_lei_akita.sh:94–108`)

## 02/11 21:27 -- HISTORICO (mais de 24 h) · sessao 0fd26c7d7b50 · - · - · origem=Ronald (tmux)

1. **O delta fecha o defeito específico de relógio na foto:** lavra em T0=12h, D5 em T1=13h e repetição em T2=14h. Regravar `julgada_em`, `veredito_em`, `insumos_em` ou outro campo fotografado com o relógio desses atos altera o estado comparado. (`app/ponto/tests/test_e3_status_lavrado.py:31`, `:298`, `:309-331`)

2. **`auto_now` de FM está coberto:** `_foto()` inclui `.values()` de todas FM; `FechamentoMensal.atualizado_em` é `auto_now=True`. (`app/ponto/tests/test_e3_status_lavrado.py:312`, `app/ponto/models.py:478-479`)

3. **Existe escape concreto fora da foto:** `Colaborador.atualizado_em` é `auto_now=True`, há colaborador no fixture e `_foto()` não fotografa essa tabela. Um mutante hipotético que regrave esse campo com o relógio do D5 passaria por essa comparação; não localizei tal escrita no caminho atual. (`app/colaboradores/models.py:380-381`, `app/ponto/tests/test_e3_status_lavrado.py:170-173`, `:309-313`; ocorrência real desse mutante [não verificado])

4. **Outras tabelas também ficam fora:** `EventoCelula.criado_em` e `processado_em`, `LogAuditoria.criado_em`, `DiaPago.lavrado_em` (`auto_now=True`). A foto observa somente células desse colaborador, FM e ED. (`app/escala/models.py:1822-1828`, `app/core/models.py:307`, `app/ponto/models.py:380`, `app/ponto/tests/test_e3_status_lavrado.py:309-313`)

5. **Trilha não é escape acidental da regra atual:** o ramo status chama `evento('veredito_celula', ...)`, que insere `LogAuditoria`; sua ausência da foto permite essa escrita prevista. (`app/ponto/portas/celula.py:761-765`, `app/core/observ.py:134-138`)

6. **Outbox mereceria testemunha própria se a afirmação for “não acorda consumidor”:** status somente não dispara evento porque `CAMPOS_QUE_ACORDAM={'veredito','ata'}`; porém mutante que inserisse um `EventoCelula` sem mudar os campos fotografados escaparia dessa foto. (`app/ponto/portas/celula.py:622-642`, `app/ponto/tests/test_e3_status_lavrado.py:309-313`)

7. **O furo `veredito_via=NULL` é estrutural e real:** gates de carimbo não exigem via; `_completar_status()` chama lavra com `via='cartorio'`; comparação da porta exige igualdade da via e, se falha, escreve via/veredito/protestos/`veredito_em`, além de acordar outbox por incluir `veredito`. O fixture normal lavra previamente pela porta e não introduz via nula. (`app/ponto/services/cartorio.py:1186-1189`, `:1345-1348`, `:889-891`, `app/ponto/portas/celula.py:753-775`, `:622-642`, `app/ponto/tests/test_e3_status_lavrado.py:189-194`, `:298-307`)

8. **Limite da prova:** igualdade das fotos prova estado final nos registros observados; não prova ausência de SQL redundante, escrita com valor igual, alteração seguida de restauração ou escrita fora do universo fotografado. (`app/ponto/tests/test_e3_status_lavrado.py:309-331`; execução SQL [não verificado])

## 02/11 21:27 -- HISTORICO (mais de 24 h) · sessao f68fd42d7647 · - · - · origem=Ronald (tmux)

Para a pergunta 3:

- **ADR 0002/0003 e docstring D5:** não localizei leitor que case o texto alterado por selo; ausência absoluta ou leitores externos **[não verificado]**. O command mantém `help` próprio, sem `help = __doc__` (`app/ponto/management/commands/lavrar_status_competencia.py:24`).

- **Selo existente do ADR 0002:** verifica AST e `_meta`, expressamente “nunca texto”; seu catálogo declara `status_dia`/`status_rotulo` como saídas escritas pela porta (`app/escala/tests/test_d3_guarda_da_linha_celula.py:11`, `:33`, `:64`). `_estado` lê AST de `ponto/portas/celula.py` e `ponto/services/cartorio.py`; o teste compara o resultado ao `CATALOGO`, sem ler ADR Markdown (`app/escala/tests/test_d3_guarda_da_linha_celula.py:75`, `:167`, `:171`).

- **ADR 0003:** o selo de naturezas está declarado como obra futura/fila, inclusive com nome provisório (`app/docs/ADR/0003-celula-tres-naturezas.md:125`, `:127`, `:130`).

- **Existe leitor genérico de docstrings:** MAPA usa `ast.get_docstring` (`app/core/mapa/contrato.py:83`, `:88`), mas devolve `{}` quando falta `@bolha` (`:45`, `:47`). A docstring D5 não contém essa inscrição (`app/ponto/management/commands/lavrar_status_competencia.py:1`, `:16`); o universo obrigatório do MAPA deriva dos registros `core/juizes.py`/`core/portas.py` (`app/core/mapa/contrato.py:157`, `:164`, `:171`, `:180`).

- **LEI-AKITA:** há casamento textual de vocabulário documental, mas lê `GLOSSARIO.md` e `PLANO-PADROES.md`, incluindo os ADRs internos deste último; não os arquivos ADR 0002/0003 (`bin/tests/test_lei_akita.sh:94`, `:102`, `:105`).

- **Teste alterado:** o executor chama D5 via `call_command` (`app/ponto/tests/test_e3_status_lavrado.py:264`, `:266`); a foto lê todas as colunas das células do colaborador, excetuando apenas status, e todas as linhas/colunas de FM/ED (`:309`, `:310`, `:312`). A conferência dos novos horários e fotos ocorre em `:318`, `:327`, `:329`, `:331`; execução efetiva nesta revisão **[não verificado]**.

## 02/11 21:04 -- HISTORICO (mais de 24 h) · sessao da8569571a0a · E4CAL-L1-P1 · verificador · origem=Code

**Parecer: a garantia irrestrita de “só status” não está fechada.** O caminho normal preserva os demais campos da célula, mas escreve auditoria; uma célula elegível com `veredito_via=NULL` pode alcançar lavra completa; e o teste deixa sobreviver uma regravação de `julgada_em` com o mesmo instante congelado. (`app/ponto/portas/celula.py:753`; `app/core/observ.py:134`; `app/escala/models.py:1739`; `app/ponto/tests/test_e3_status_lavrado.py:286`)

**1) Quem escreve, quem lê e alcance financeiro**

O escritor produtivo dos dois campos é `_aplicar_status`, dentro da porta: atribui em `app/ponto/portas/celula.py:705`, devolve os dois nomes em `:706` e persiste por `salvar_e_emitir`, cujo save real está em `:640`. O método do modelo apenas delega. (`app/escala/models.py:1794`)

As entradas são D5, cartório normal nos dois ramos e decisão administrativa. (`app/ponto/services/cartorio.py:889`; `app/ponto/services/cartorio.py:1229`; `app/ponto/services/cartorio.py:1789`; `app/ponto/services/veredito_celula.py:330`)

| Leitor produtivo | Uso e destino do valor |
|---|---|
| `app/ponto/status_dia.py:212` | Lê ambos e devolve status/rótulo/fonte em `:213–214`, sem guarda de exportada. A interface singular delega ao mesmo corpo em `:179`. |
| `app/core/services/painel_op.py:336` | Consome o D4 e coloca a resposta em `linha['dia']`, em `:338`. Termina no badge/tooltip HTML: `app/templates/core/_painel_card.html:66–67`. |
| `app/ponto/services/cartorio.py:882` | Lê `status_dia` para pular preenchimento já realizado; status ausente segue à porta em `:889`. |
| `app/ponto/portas/celula.py:697` | Preserva ambos quando faltam fatos e o veredito permanece igual; a comparação de idempotência está em `:703`. |
| `app/ponto/portas/celula.py:723` | Copia status para auditoria; descrição e JSON são escritos em `:762–765` e `:777–780`. A consulta administrativa exibe essa trilha. (`app/core/views.py:705`; `app/core/views.py:733`) |

Para esses leitores, não encontrei passagem do **valor das duas colunas** para número de folha, TXT, FM, pauta DP ou emissão de chamado/pergunta/disputa: o consumidor externo encontrado termina em HTML; o D5 retorna antes da fábrica; e a retenção do TXT consulta veredito/protestos/trabalha. (`app/core/services/painel_op.py:338`; `app/templates/core/_painel_card.html:67`; `app/ponto/services/cartorio.py:949`; `app/folha/export.py:1215`; `app/folha/export.py:1228`; `app/folha/export.py:1237`)

O painel atual consulta pares na janela de hoje menos dois dias até hoje; portanto esse caminho não varre as competências 06–09/2026 na data desta revisão. (`app/core/services/painel_op.py:327`)

O calendário ainda produz seu próprio dicionário de status/rótulo; seus templates não estão lendo essas colunas. “Minha escala” também monta rótulo de previsão. Esses homônimos não são leitores adicionais de `CelulaDia.status_rotulo`. (`app/colaboradores/services/calendario.py:895`; `app/colaboradores/services/calendario.py:896`; `app/escala/views.py:54`; `app/escala/views.py:60`)

Fora de produção, há escritas explícitas de preparação de testes e leituras de asserção; a migração apenas adiciona as colunas. (`app/ponto/tests/test_e3_status_lavrado.py:277`; `app/ponto/tests/test_e3_status_lavrado.py:307`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:52`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:80`; `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13`)

**TXT de 09:** comparando duas regenerações com todos os demais insumos fixos e somente essas duas colunas alteradas, o conteúdo permanece igual: o regerador usa `montar_linhas_export`, que lê FM para eventos e os juízes existentes para inclusão/faltas. Isso não prova igualdade com o TXT histórico já armazenado, que pode ter divergências anteriores. (`app/folha/management/commands/regerar_txt_dominio.py:85`; `app/folha/export.py:695`; `app/folha/export.py:700`; `app/folha/export.py:711`; `app/folha/export.py:721`) Igualdade real dos TXT de produção: **[nao verificado]**.

**2) O que `completar_status=True` realmente grava**

O caminho verifica impressão/carimbo/veredito nos dois ramos, chama `_completar_status` e continua antes da lavra normal. O invólucro retorna antes de reconciliação, segunda passada e fábrica. (`app/ponto/services/cartorio.py:1186–1189`; `app/ponto/services/cartorio.py:1345–1348`; `app/ponto/services/cartorio.py:948–950`)

Com via válida de cartório e veredito/protestos preservados, a porta salva **somente `status_dia/status_rotulo` na célula**. Não passa por `carimbar`, onde seriam escritos `julgada_em`, impressão e insumos; tampouco inclui campos de edição/regeneração no save. (`app/ponto/portas/celula.py:753–760`; `app/ponto/portas/celula.py:706`; `app/ponto/portas/celula.py:640`; `app/ponto/portas/celula.py:800–807`)

**Há outra escrita:** o ramo chama `evento('veredito_celula', ...)`, que tenta inserir `LogAuditoria`, com tratamento best effort. A trilha possui `criado_em` automático. Na célula decidida pelo admin, o ramo especial salva status e retorna sem essa auditoria. (`app/ponto/portas/celula.py:761–765`; `app/core/observ.py:105`; `app/core/observ.py:133–138`; `app/core/models.py:307`; `app/ponto/portas/celula.py:738–744`)

No ramo normal de status apenas, não nasce `EventoCelula`: somente `veredito` ou `ata` acordam o outbox. A auditoria do command, sem usuário humano, também não satisfaz a exceção de “admin tocou o dia”. (`app/ponto/portas/celula.py:623`; `app/ponto/portas/celula.py:638–642`; `app/core/observ.py:118`; `app/chamados/juizes.py:992–999`)

A operação usa `save`, portanto não fundamenta afirmar “nenhum signal é disparado”. Não localizei receptor de `CelulaDia` na cópia; execução do despacho de signals: **[nao verificado]**. (`app/ponto/portas/celula.py:640`)

**Ressalva estrutural:** os gates não verificam `veredito_via`, e o modelo aceita NULL. Uma célula elegível, com veredito válido de cartório e via nula, falha na igualdade de `:753` e cai em `:769–775`: grava também protestos/veredito/via/`veredito_em` e emite outbox. A existência desse estado no conjunto raspado é **[nao verificado]**. (`app/ponto/services/cartorio.py:1186`; `app/ponto/services/cartorio.py:1345`; `app/escala/models.py:1739`; `app/ponto/portas/celula.py:753`; `app/ponto/portas/celula.py:769–775`)

**3) Mordida do teste** [nao verificado]

- **“Guarda volta”: sim, por inspeção.** A saída antecipada impede as contagens e o preenchimento exigidos no dry/apply. Execução do mutante: **[nao verificado]**. (`app/ponto/tests/test_e3_status_lavrado.py:317`; `app/ponto/tests/test_e3_status_lavrado.py:321`)
- **“Também escreve `julgada_em`”: não necessariamente.** A foto inclui o campo, mas a lavra inicial e o command usam o mesmo `AGORA` congelado. Regravar `julgada_em=agora` ou `timezone.now()` pode manter exatamente o mesmo valor e passar. (`app/ponto/tests/test_e3_status_lavrado.py:189–194`; `app/ponto/tests/test_e3_status_lavrado.py:286`; `app/ponto/management/commands/lavrar_status_competencia.py:44`; `app/ponto/portas/celula.py:800`)
- A foto cobre células **do colaborador do fixture**, excetuando ambos os status, e todas as linhas de FM/ED disponíveis no teste. Não cobre auditoria, outbox, `DiaPago`, chamados, perguntas, disputas nem células de outros colaboradores. (`app/ponto/tests/test_e3_status_lavrado.py:309–313`)
- Não confere os rótulos; só exige status preenchido e os valores específicos de duas células. A última passada verifica contadores, sem nova foto. (`app/ponto/tests/test_e3_status_lavrado.py:310`; `app/ponto/tests/test_e3_status_lavrado.py:321–325`)

Para matar a regravação de carimbo, a lavra inicial precisa ocorrer em T0 e o D5 em T1 distinto, ou a prova precisa inspecionar os campos efetivamente persistidos. A igualdade temporal atual é a brecha. (`app/ponto/tests/test_e3_status_lavrado.py:189`; `app/ponto/tests/test_e3_status_lavrado.py:286`)

**4) Aval e classes reprovadas**

O caminho canônico usa o mesmo juiz e a mesma porta; a competência entregue virou informação; carimbo velho fica na fila; e o relógio funcional é capturado uma vez na borda. Não aparece segundo juiz, segundo escritor produtivo ou derivação nova nesse caminho. (`app/ponto/management/commands/lavrar_status_competencia.py:43–57`; `app/ponto/services/cartorio.py:885–891`; `app/ponto/portas/celula.py:693–698`)

Não há escrita de FM/ED nesse percurso. Há, porém, **leitura indireta de ED** por `competencia_entregue`, contrariando a frase documental de que o command não lê ED. (`app/ponto/management/commands/lavrar_status_competencia.py:13`; `app/ponto/management/commands/lavrar_status_competencia.py:43`; `app/folha/export.py:569–575`)

A auditoria não é explicitamente proibida pelo MUDA 1 fornecido; já a possibilidade de lavra completa/outbox com via nula impede certificar seu contrato para qualquer célula elegível. O selo temporal também não está fechado. (`app/ponto/portas/celula.py:761`; `app/ponto/portas/celula.py:753–775`; `app/ponto/tests/test_e3_status_lavrado.py:286`)

Não foi introduzida ordem explícita nova de locks. As células são carregadas antes e o atomic do save não revalida a impressão; segurança contra alteração concorrente permanece **[nao verificado]**. (`app/ponto/management/commands/lavrar_status_competencia.py:54`; `app/ponto/services/cartorio.py:1345`; `app/ponto/portas/celula.py:639–640`)

Os ADRs ainda descrevem a guarda retirada e precisam de reconciliação documental. (`app/docs/ADR/0002-celula-e-o-dia.md:69–71`; `app/docs/ADR/0003-celula-tres-naturezas.md:113–114`)

Apply das 06–09/2026, reversão prévia, hashes FM/TXT e cumprimento operacional dos pré-requisitos de pouso: **[nao verificado]**. [nao verificado]

**5) Outras guardas e retorno a NULL**

O cartório de lote e o julgamento unitário não têm guarda de exportada nesses caminhos. As guardas restantes identificadas protegem regeneração/reversão de DNA; seus saves não incluem status. (`app/ponto/management/commands/processar_cartorio.py:49–65`; `app/ponto/services/cartorio.py:1950–1967`; `app/ponto/portas/celula.py:238–242`; `app/ponto/portas/celula.py:264–265`; `app/ponto/portas/celula.py:520–539`)

Existe mecanismo capaz de produzir `(None, None)` quando falta `fatos_do_status` e muda o veredito, mas não é guarda de exportada. Os chamadores produtivos encontrados do cartório fornecem os fatos; o admin usa seu mapeamento próprio. (`app/ponto/portas/celula.py:694–698`; `app/ponto/services/cartorio.py:889–891`; `app/ponto/services/cartorio.py:1229–1234`; `app/ponto/services/cartorio.py:1789–1794`)

O D5 também pula qualquer `status_dia` não nulo, mesmo com `status_rotulo=NULL`; o leitor então usa o rótulo do vocabulário. Não encontrei outra guarda de exportada que anule o status. (`app/ponto/services/cartorio.py:882–884`; `app/ponto/status_dia.py:214`) Regressão ou divergência real após apply: **[nao verificado]**.

## 02/11 21:03 -- HISTORICO (mais de 24 h) · sessao 53f77a91c3e3 · - · - · origem=Ronald (tmux)

Q2 — caminho real e efeitos:

- `handle` lê a competência entregue apenas como informação, captura `agora`/`hoje`, seleciona células até ontem e chama `julgar_colab(... completar_status=True)`; não há trava de exportada nesse caminho (`app/ponto/management/commands/lavrar_status_competencia.py:43`, `:44`, `:49`, `:54`, `:56`).
- O corpo constrói a mesma grade, verifica impressão/carimbo/veredito e, nos dois ramos, chama `_completar_status` seguido de `continue`; o wrapper retorna antes de reconciliação, segunda passada, fábrica e cluster (`app/ponto/services/cartorio.py:1113`, `:1186`, `:1187`, `:1189`, `:1345`, `:1346`, `:1348`, `:949`).
- `_completar_status` pula status existente, conta carimbo velho como fila e, em apply, chama a porta com o veredito/protestos existentes e os fatos da grade (`app/ponto/services/cartorio.py:882`, `:885`, `:888`, `:889`).
- `CelulaDia.lavrar_veredito` só delega à porta (`app/escala/models.py:1787`, `:1794`).
- No ramo canônico `veredito_via='cartorio'`, a porta compara veredito/via/protestos, aplica os dois campos e executa `cel.save(update_fields=['status_dia','status_rotulo'])`; não carimba `julgada_em`, `insumos_em`, edição ou regeneração (`app/ponto/portas/celula.py:703`, `:705`, `:706`, `:753`, `:760`, `:640`). A escrita de `julgada_em` pertence a `carimbar`, que os `continue` anteriores não alcançam (`app/ponto/portas/celula.py:800`, `:805`; `app/ponto/services/cartorio.py:1189`, `:1348`).
- **Há escrita adicional de trilha:** depois do save de status, chama `evento('veredito_celula',...)`, que efetivamente executa `LogAuditoria.objects.create`, em savepoint próprio e best effort (`app/ponto/portas/celula.py:761`, `:762`, `:764`; `app/core/observ.py:105`, `:133`, `:134`). A tabela possui timestamp automático `criado_em` (`app/core/models.py:307`).
- Em célula decidida pelo admin, o ramo especial aplica status e retorna sem essa chamada de auditoria (`app/ponto/portas/celula.py:738`, `:739`, `:742`, `:744`).
- **Não cria outbox nesse ramo:** somente `veredito` ou `ata` acordam `EventoCelula`; o conjunto de campos de status não intersecta esses campos (`app/ponto/portas/celula.py:623`, `:638`, `:640`, `:641`, `:631`). Não localizei receiver de `CelulaDia` no código; dispatch interno do Django não foi inspecionado [nao verificado].

Ressalva estrutural importante:

- A promessa “só status” pressupõe via válida existente (`cartorio` ou `admin`); a elegibilidade do D5 não testa `veredito_via`, enquanto o modelo permite `NULL` (`app/ponto/services/cartorio.py:1186`, `:1345`; `app/escala/models.py:1739`).
- Se existir célula elegível com `veredito_via=NULL` e veredito válido de cartório, a comparação de igualdade falha e a porta escreve também `protestos`, `veredito`, `veredito_via`, `veredito_em`, além de emitir outbox e auditoria (`app/ponto/portas/celula.py:753`, `:769`, `:772`, `:773`, `:775`, `:777`, `:638`). Existência dessa condição no dado fornecido: [nao verificado].
- Não existe `select_for_update` no caminho examinado; as células são carregadas antes e a transação envolve o save, sem revalidar impressão/veredito sob lock (`app/ponto/management/commands/lavrar_status_competencia.py:54`; `app/ponto/portas/celula.py:639`, `:640`). Segurança contra edição concorrente no intervalo: [nao verificado]. O diff não acrescenta nem inverte locks (`diff.txt` fornecido [nao verificado como arquivo local]).

Q5 — outras guardas:

- O cron normal do cartório seleciona todas as células passadas e chama o mesmo julgamento, sem filtro de competência exportada (`app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`).
- A unidade `julgar_celula` filtra futuro, inexistência e turno ainda em andamento; depois chama o mesmo cartório sem guarda exportada (`app/ponto/services/cartorio.py:1950`, `:1952`, `:1958`, `:1962`, `:1965`).
- Há guardas de exportada nas operações de **regenerar DNA** e **reverter regeneração**, respectivamente `app/ponto/portas/celula.py:196`, `:199`, `:238`, `:242` e `:520`, `:525`, `:526`; seus saves não incluem status (`app/ponto/portas/celula.py:264`, `:265`, `:538`, `:539`). Elas não anulam status lavrado.
- A porta possui um ramo que devolve `(None,None)` quando faltam fatos e muda o veredito (`app/ponto/portas/celula.py:694`, `:697`); todos os chamadores de produção encontrados do cartório fornecem fatos (`app/ponto/services/cartorio.py:889`, `:890`, `:1229`, `:1233`, `:1789`, `:1793`), e o admin usa o mapeamento próprio (`app/ponto/services/veredito_celula.py:330`; `app/ponto/portas/celula.py:694`, `:695`).
- Não encontrei guarda “exportada não se toca” aplicada à gravação de `status_dia/status_rotulo` que os faça voltar a nulo; o alcance concreto permanece condicionado à impressão em dia e status ausente (`app/ponto/services/cartorio.py:882`, `:885`, `:1186`, `:1345`). Qualquer divergência real em produção: [nao verificado].

_1498 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
