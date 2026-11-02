# CODEX-ESTADO

> Ultima resposta processada: **02/11/2026 18:27** · publicado em **02/11/2026 19:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 02/11 18:27 · sessao ae4ec323f2f3 · DEPLOY-[nome]-ELO-DA-CORRENTE · leitura · origem=Code

A cópia ainda faz **“teto → aviso → restart”**, e o agendador **não repete automaticamente uma tentativa recusada**. São duas divergências diretas do contrato proposto. (`bin/deploy.sh:274–279`; `bin/deploy_agendado.sh:68–82`)

1. **Quem escreve, lê e quais execuções escapam da marca**

   `cron_run.sh` recebe nome/empresa/comando, cria `${N}_${E}.$$`, grava a data ISO e instala limpeza por `trap EXIT`. O PID é do envelope no host; a marca não identifica o container. (`bin/cron_run.sh:5–11`)

   O leitor operacional é `deploy.sh::em_curso()`: extrai o PID do nome, consulta `/proc/<pid>`, devolve nome/empresa quando encontra o processo e remove a marca quando não encontra. Não lê a data gravada no conteúdo. (`bin/deploy.sh:271–272`)

   Todos os comandos **C e RAW renderizados** passam por `_run()` → `cron_run.sh`; a execução final de cada elo também passa pelo runner. (`app/config/crons.py:1423`; `app/config/crons.py:1431–1434`; `app/config/crons.py:1789–1802`; `bin/encadeado.sh:93`)

   **Há furo comprovado:** `--corrente` é anexado depois do envelope, separado por `;`, e executa `docker exec saas_core ... crons corrente` antes de qualquer marca própria. A espera pelos carimbos também fica fora da marca; o comando final do elo recebe marca somente na linha 93. (`app/config/crons.py:1812–1813`; `bin/encadeado.sh:42`; `bin/encadeado.sh:77–93`)

   Existe ainda agendamento fora desse render: `deploy_agendado.sh` gera uma entrada em `/etc/cron.d` sem runner. A igualdade entre crontab instalado e configuração nesta produção é **[nao verificado]**. (`bin/deploy_agendado.sh:54–56`; mecanismo de comparação em `bin/crons.sh:17–22`)

2. **Como distinguir host de casca**

   Não há campo explícito `host/container/alvos` nos esquemas C, RAW ou ENC. `estagio`, `cwd` e `depende` não fornecem essa classificação. (`app/config/crons.py:29–33`; `app/config/crons.py:53–54`; `app/config/crons.py:102–109`)

   C usa `MANAGE`, definido como `docker exec saas_core`; os elos também lançam seu comando em `saas_core`. Para RAW, é necessário seguir o script chamado. (`app/config/crons.py:21`; `app/config/crons.py:1798`; `bin/encadeado.sh:93`; `app/config/crons.py:1791`)

   **RAW não significa “fora das cascas”:** a sentinela é RAW e executa em `mensageria`; `lavrar_abriu_nao_bateu.sh` coleta logs no host e executa uma etapa em `saas_core`. (`app/config/crons.py:669`; `mensageria/bin/sentinela_diario.sh:14–16`; `bin/lavrar_abriu_nao_bateu.sh:19–24`)

   **A sombra é híbrida:** o script parte do host, executa o ensaio em container separado por `docker run`, mas também chama `docker exec saas_core` para ler a lavra. Portanto, excluir a sombra inteira da proteção exige esclarecer o tratamento dessa fase interna; classificá-la apenas pelo nome ou por RAW deixa essa exposição. (`app/config/crons.py:331–333`; `bin/sombra.sh:138–144`; `bin/sombra.sh:182–191`)

3. **Marca órfã e espera eterna**

   Hoje há duas limpezas: o runner remove sua marca no `EXIT`; o deploy remove marcas cujo PID não existe em `/proc`. A limpeza de órfãs já está dentro da consulta usada pelo deploy, sem cron separado. (`bin/cron_run.sh:10`; `bin/deploy.sh:271–274`)

   A identidade é fraca: o deploy verifica somente a existência do PID, sem conferir início ou comando do processo. Por inferência, um PID reutilizado pode parecer cron vivo; também não há comprovação de que a morte do envelope encerrou todos os seus filhos. (`bin/deploy.sh:272`; `bin/cron_run.sh:9–11`)

   **Desenho proposto:** preservar a limpeza de órfãs confirmadas, fortalecer a identidade e, diante de execução ainda viva ou identidade inconclusiva, atingir um teto e **recusar sem restart**, nomeando o impedimento. Isso evita espera eterna sem transformar idade da marca em autorização para matar trabalho. Implementação desse comportamento: **[nao verificado]**; hoje o teto permite restart. (`bin/deploy.sh:274–279`)

4. **Corrida e ponto para impedir novo elo**

   A corrida permanece aberta: a última consulta está na linha 278 e o restart na 279, sem exclusão compartilhada com o nascimento da marca. Um elo pode alcançar o runner nesse intervalo. (`bin/deploy.sh:278–279`; `bin/encadeado.sh:93`; `bin/cron_run.sh:9–11`)

   Há uma **porta comum para a execução dos elos**: `cron_run.sh`, antes de registrar a marca e lançar o comando. Os C e RAW renderizados também passam ali. A trava existente em `encadeado.sh` é por comando (`/tmp/encadeado_$CMD.lock`), para deduplicar elos; o deploy não participa dela. (`bin/cron_run.sh:9–11`; `app/config/crons.py:1431–1434`; `bin/encadeado.sh:71–75`; `bin/deploy.sh:271–279`)

   **Proposta, sem cron novo:** admissão sincronizada nessa porta; o runner registra a execução sob a mesma exclusão usada pelo deploy. O deploy fecha novas admissões, espera as marcas existentes, reconsulta e mantém a exclusão até concluir o restart ou recusar. Um arquivo consultado sem exclusão atômica conservaria a corrida. Esse protocolo ainda é **[nao verificado]** na cópia. (`bin/cron_run.sh:9–11`; `bin/deploy.sh:271–279`)

   A porta precisa também abranger a descoberta hoje desmarcada em `encadeado.sh:42`. Não convém bloquear globalmente antes da espera pelos gatilhos, pois ela depende dos carimbos das outras empresas. (`bin/encadeado.sh:42`; `bin/encadeado.sh:77–90`)

5. **Outros consumidores e selos**

   No código encontrado, `ESPERA_CRON_S` é lido somente pelo ramo de restart do deploy, com padrão 600 e incrementos de cinco segundos. `deploy_agendado` consome o resultado da esteira, sem ler essa variável. (`bin/deploy.sh:274–278`; `bin/deploy_agendado.sh:73–77`)

   O selo específico atual apenas procura os textos `DEPLOY-ESPERA-CRON` e `crons_em_curso`; não demonstra recusa, filtragem host/casca ou ausência de restart. Ele é consumido pelo placar de código. (`bin/contrato_deploy.sh:47–49`; `bin/placar_code.sh:130–132`)

   Os outros testes preservam contratos distintos: conflitos/URLconfs, janela de autenticação, histórico após provas e ordem/teto dos carimbos encadeados. Esses trechos não exercitam a proteção contra restart com cron vivo. (`bin/tests/test_deploy_recusa_conflito.sh:52–60`; `bin/tests/test_janela_auth.sh:123–144`; `bin/tests/test_orcamento_contexto.sh:263–333`; `bin/tests/test_furo_encadeado_ao_cartorio.sh:24–68`)

   O molde de cadeia exige `deploy: OK` e sai com falha quando não encontra esse resultado; não contém repetição nesse trecho. Resultado de execução dos selos: **[nao verificado]**. (`bin/molde_fatia/cadeia.sh:89–90`)

6. **Furos restantes no contrato**

   **Quem chama/repete:** a promessa “recusa vira nova tentativa pelo agendador” não corresponde à implementação. Ele executa cada esteira uma vez, registra seu `rc`, gera `.done` e remove agenda e cron independentemente desse resultado. O `echo` final ainda pode fazer `rodar` terminar com sucesso após uma esteira falhar. (`contrato.txt:5`, anexo; `bin/deploy_agendado.sh:68–82`)

   **Quem espera:** “todo cron com marca” não cobre a descoberta da corrente; e excluir “cron do host” exige tratar scripts híbridos, inclusive a sombra. (`bin/encadeado.sh:42`; `bin/sombra.sh:138–144`; `mensageria/bin/sentinela_diario.sh:14–16`)

   **Falha/limite:** o contrato exige teto absoluto declarado; o número ainda está delegado ao construtor. Os 764 s do incidente não estabelecem duração máxima. Na fonte, `_espera` conta intervalos de sono, e o vencimento apenas avisa. (`contrato.txt:1` e `contrato.txt:3`, anexo; `bin/deploy.sh:273–279`)

   **Marca obrigatória:** falhas de `mkdir` ou gravação da marca não impedem explicitamente o lançamento do comando. Além disso, o runner usa caminho absoluto e o leitor usa `$RAIZ`; uma execução noutra raiz pode consultar diretórios diferentes. (`bin/cron_run.sh:9–11`; `bin/deploy.sh:45`; `bin/deploy.sh:271`)

   **Medida:** `crons_quebrados` considera a última corrida por nome/empresa, em 26 horas, e qualquer saída diferente de 0/2; não mede “137 no minuto de deploy”. Recuperação posterior pode retirar a morte desse contador. (`app/core/services/crons_quebrados.py:16–17`; `app/core/services/crons_quebrados.py:33–47`)

   O placar contém início/fim/duração/exit, mas o histórico do deploy é gravado após as provas, não no instante do restart. Falta definir a correlação temporal e o tratamento de registros ausentes; o contador atual retorna lista vazia quando não consegue abrir o placar. Valor da medida exigida e eficácia posterior à cura: **[nao verificado]**. (`bin/cron_run.sh:13`; `bin/deploy.sh:279`; `bin/deploy.sh:342–347`; `app/core/services/crons_quebrados.py:29–32`)

   **Quem desliga/modos:** `--reload-agendado` e `--reload-copia` usam HUP e não atravessam essa espera. O contrato deve delimitar a proteção ao restart destrutivo e exigir liberação automática da admissão ao terminar ou recusar, sem flag de bypass. O mutante “avisa e reinicia” precisa reprovar no selo novo; hoje ele corresponde ao comportamento da cópia. (`bin/deploy.sh:54–60`; `bin/deploy.sh:237–279`; `bin/contrato_deploy.sh:47–49`; `contrato.txt:5`, anexo)

## 02/11 18:25 · sessao bf48b9348077 · - · - · origem=Ronald (tmux)

- **Escritor, formato, conteúdo:** `bin/cron_run.sh:5` recebe nome/empresa/comando; `:9` monta `logs/crons_em_curso/${N}_${E}.$$`, com PID do envelope do host; `:10` grava instante ISO e instala `trap ... EXIT`; `:11` executa o comando. A marca não guarda alvo/container nem identidade adicional do processo (`bin/cron_run.sh:7-11`).
- **Leitor operacional e limpeza:** `bin/deploy.sh:271-272` enumera marcas, extrai o sufixo após o último ponto, considera vivo se `/proc/<pid>` existe, imprime nome/empresa e remove marca cujo PID não existe; `:274-278` usa essa lista para esperar e avisar. O conteúdo ISO da marca não é lido por essa função (`bin/deploy.sh:271-272`). O outro consumidor encontrado é estático: selo procura texto `crons_em_curso` no deploy e runner (`bin/contrato_deploy.sh:47-49`).
- **Cobertura normal:** tanto RAW quanto C renderizados passam `_run()` (`app/config/crons.py:1789-1802`), que chama runner (`:1423`, `:1431-1434`); os comandos finais dos elos também passam runner (`bin/encadeado.sh:93`). O crontab instalado de produção e sua igualdade com o render atual são **[nao verificado]**; há mecanismo para comparar, não evidência dessa execução (`bin/crons.sh:17-22`).
- **Furo sem marca comprovado:** o disparo `encadeado.sh --corrente` é anexado **após** o envelope, com `;` (`app/config/crons.py:1812-1813`). Esse caminho chama `docker exec saas_core ... crons corrente` sem runner (`bin/encadeado.sh:37-49`); portanto existe comando de casca da infraestrutura do cron sem marca. A espera de carimbos também fica sem marca e só entra no runner ao lançar efetivamente o elo (`bin/encadeado.sh:71-93`). Não significa que os management commands finais dos elos estejam sem marca (`bin/encadeado.sh:93`).
- **Fora do render:** `deploy_agendado.sh` escreve uma entrada `/etc/cron.d` que chama diretamente seu modo `rodar`, sem runner (`bin/deploy_agendado.sh:54-56`); este executa `esteira.sh` fornecido pelo chamador (`:68-74`). Quais comandos de casca essas esteiras executam: **[nao verificado]**.

- **Distinção por fonte:** C usa `MANAGE = docker exec saas_core ...` (`app/config/crons.py:21`, `:1798`); ENC lança `docker exec saas_core` (`bin/encadeado.sh:93`). Os esquemas C, RAW e ENC não declaram campo host/container/alvos (`app/config/crons.py:29-33`, `:53-54`, `:102-109`).
- **RAW não equivale a “fora das cascas”:** sentinela é RAW (`app/config/crons.py:669`) e executa dentro de `mensageria` (`mensageria/bin/sentinela_diario.sh:14-16`); negou_fogo também (`app/config/crons.py:686-687`; `mensageria/bin/negou_fogo_diario.sh:10-11`). `lavrar_abriu_nao_bateu.sh` mistura coleta no host e comando em `saas_core` (`bin/lavrar_abriu_nao_bateu.sh:19-24`).
- **Sombra é híbrida:** seu agendamento RAW chama script do host (`app/config/crons.py:331-333`), que lança container de ensaio separado por `docker run` (`bin/sombra.sh:182-191`), mas também executa uma fase de leitura de lavra dentro de `saas_core` (`bin/sombra.sh:138-144`). Para distinguir proteção efetiva pela fonte atual, é necessário seguir comando e scripts/fases; não basta RAW/C nem nome (`app/config/crons.py:102-109`, `:1789-1802`; `bin/sombra.sh:138-144`, `:182-191`).

- **Órfã:** limpeza normal é `trap EXIT` do runner (`bin/cron_run.sh:10`); limpeza de PID ausente é feita pelo deploy ao enumerar (`bin/deploy.sh:271-272`). Não há validação de identidade além da existência de `/proc/<pid>` nessa leitura, portanto reutilização de PID pode ser confundida com cron vivo — inferência direta do teste (`bin/deploy.sh:272`).
- **Tratamento proposto:** preservar a eliminação/ignorância de marca comprovadamente órfã; para marca considerada viva, a recusa com teto absoluto evita espera eterna. Validação mais forte de identidade exigiria desenho adicional: a marca atual só contém data e PID no nome (`bin/cron_run.sh:9-10`); implementação dessa cura **[nao verificado]**.

- **Ponto comum possível:** o runner é o ponto comum antes do lançamento efetivo dos C, RAW e elos (`app/config/crons.py:1431-1434`, `:1789-1802`; `bin/encadeado.sh:93`). Hoje não há coordenação com deploy nessa sequência marca→comando (`bin/cron_run.sh:9-11`; `bin/deploy.sh:271-279`). Um protocolo de exclusão compartilhado runner/deploy pode impedir novo lançamento enquanto deploy drena/reinicia; arquivo consultado sem exclusão atômica manteria a corrida — proposta, **[nao verificado]**.
- **Limites dessa proposta:** o `flock` atual é por elo (`/tmp/encadeado_$CMD.lock`), serve para deduplicar empresas e não é consultado pelo deploy (`bin/encadeado.sh:71-75`; `bin/deploy.sh:271-279`). Também seria necessário cobrir o `docker exec` de descoberta anterior ao runner (`bin/encadeado.sh:42`).

## 02/11 18:25 · sessao 8161155c5547 · - · - · origem=Ronald (tmux)

**Corrida e ponto de admissão**

- A corrida existe na fonte: deploy lê marcas em `em_curso()`, termina a espera e faz uma última leitura em `bin/deploy.sh:271-278`; o restart seguinte não está protegido por trava (`bin/deploy.sh:279`). Nesse intervalo, um elo pode terminar a espera pelos carimbos e entrar no runner (`bin/encadeado.sh:77-93`).
- A corrente executa cada elo sequencialmente, mas sem marca própria abrangendo toda a corrente (`bin/encadeado.sh:42`, `bin/encadeado.sh:53-60`). A marca de cada execução nasce apenas ao entrar em `cron_run.sh` (`bin/encadeado.sh:93`, `bin/cron_run.sh:9-11`).
- A trava existente é `/tmp/encadeado_$CMD.lock`, por comando, para impedir duplicata de elo; deploy não participa dela (`bin/encadeado.sh:71-72`, `bin/deploy.sh:271-279`).
- Há uma porta comum para **execução dos elos**: `bin/encadeado.sh:93` → `bin/cron_run.sh:9-11`. As linhas C e RAW geradas também usam esse runner (`app/config/crons.py:1423`, `app/config/crons.py:1431-1434`, `app/config/crons.py:1789-1802`).
- **Proposta, ainda não implementada:** protocolo de admissão nessa porta, antes de criar a marca e lançar o comando; deploy fecha a admissão sob a mesma trava, drena as execuções já marcadas, faz a rechecagem e mantém a admissão fechada até terminar o restart ou recusar. O ponto de inserção é `bin/cron_run.sh:9-11`; o trecho a proteger é `bin/deploy.sh:271-279`. Flag lida sem exclusão mútua não fecha a corrida; eficácia de implementação futura [nao verificado].
- Não colocar a admissão global antes da espera por gatilho: ali o elo depende de outras empresas chegarem aos carimbos (`bin/encadeado.sh:77-90`). O bloqueio deve atingir o nascimento da execução em `bin/encadeado.sh:93`.
- Limite dessa porta: o render da corrente já executa `docker exec saas_core` antes do runner (`bin/encadeado.sh:42`); logo “toda atividade dentro da casca passa pelo runner” não é verdade na fonte (`bin/encadeado.sh:42`, `bin/encadeado.sh:93`).

**Furos do contrato comprovados**

- A promessa de “recusa vira nova tentativa pelo deploy_agendado” não corresponde ao script: ele executa a esteira uma vez e registra seu rc (`bin/deploy_agendado.sh:73-77`); depois gera `.done` e remove agenda e cron independentemente desse rc (`bin/deploy_agendado.sh:79-82`). Há ainda risco de o `rodar` acabar com rc 0 pelo `echo` final mesmo após esteira falhar (`bin/deploy_agendado.sh:74`, `bin/deploy_agendado.sh:82`).
- O molde de cadeia reconhece falha de deploy e sai 1, mas não faz nova tentativa (`bin/molde_fatia/cadeia.sh:89-90`).
- O cron de higiene usa HUP, não restart, e não passa pela espera de marcas: ramificação `AGENDADO` em `bin/deploy.sh:237-265`; espera apenas no `else`, `bin/deploy.sh:266-279`. As flags que selecionam esse ramo estão em `bin/deploy.sh:54` e `bin/deploy.sh:60`.
- O selo atual do deploy só procura strings relacionadas à espera e à marca, sem demonstrar recusa no teto ou exclusão da corrida (`bin/contrato_deploy.sh:47-50`).
- A classificação RAW não significa execução exclusivamente fora das cascas: `lavrar_abriu_nao_bateu.sh` é RAW (`app/config/crons.py:271`), mas sua pipeline executa comando dentro de `saas_core` (`bin/lavrar_abriu_nao_bateu.sh:19-24`). Logo classificar todo RAW como host abriria furo (`app/config/crons.py:271`, `bin/lavrar_abriu_nao_bateu.sh:23-24`).
- O selo antigo do encadeado usa `CRON_RUN_FAKE=1` (`bin/tests/test_furo_encadeado_ao_cartorio.sh:55`), porém o runner lido não contém ramo para essa variável e executa `bash -c "$CMD"` (`bin/cron_run.sh:5-11`). Esse selo não deve ser tomado como prova de isolamento sem conferência adicional [nao verificado].

## 02/11 18:25 · sessao 6079d3f52882 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura de fonte; execução dos selos [nao verificado].

1. **`deploy_agendado` não oferece a nova tentativa prometida no contrato.** Executa cada `esteira.sh` uma vez, captura e registra `rc`, segue o laço, copia resultado para `.done`, remove agenda e remove `/etc/cron.d/...`, independentemente do `rc` da fatia. Não há laço de repetição nessa implementação. `bin/deploy_agendado.sh:68-81`. Portanto, “recusa vira nova tentativa pelo deploy_agendado” é um furo concreto do contrato contra a cópia. `bin/deploy_agendado.sh:73-81`.

2. **Consumidor de `ESPERA_CRON_S`: somente o ramo de restart de `deploy.sh` no código encontrado.** Usa default 600, espera em passos de cinco segundos, imprime aviso e reinicia mesmo se o cron permanece vivo. `bin/deploy.sh:274-279`. O agendador não lê essa variável: chama uma esteira e lê seu `rc`. `bin/deploy_agendado.sh:73-77`. Há referências documentais em `app/docs/RELATO.md:3673`, `:3676`, `:3907`, `:4336`; seus relatos de produção adicionais não devem ser usados para ampliar o dado raspado autorizado.

3. **O selo de deploy atual é fraco para este defeito.** `contrato_deploy.sh` apenas exige os textos `DEPLOY-ESPERA-CRON` e `crons_em_curso`; não prova espera, recusa, filtragem host/casca nem ausência de restart com marca viva. `bin/contrato_deploy.sh:47-49`. O mutante “teto e reinicia” já corresponde ao comportamento da cópia. `bin/deploy.sh:274-279`. Esse contrato é consumido pelo placar de código. `bin/placar_code.sh:130-132`.

4. **Outros selos que leem deploy têm contratos diferentes.** `test_deploy_recusa_conflito.sh` cobra chamada à guarda de conflitos e importação dos URLconfs. `bin/tests/test_deploy_recusa_conflito.sh:52-60`. `test_orcamento_contexto.sh` cobra histórico depois das provas, seus modos e que reprovação da prova de casca continua sendo porta; seus mutantes não exercitam cron vivo/restart. `bin/tests/test_orcamento_contexto.sh:263-333`. `test_janela_auth.sh` verifica a chamada da guarda; `bin/tests/test_janela_auth.sh:64-67`. Nenhum desses trechos é prova do novo contrato de cron vivo. `bin/contrato_deploy.sh:47-49`; `bin/tests/test_deploy_recusa_conflito.sh:52-60`; `bin/tests/test_orcamento_contexto.sh:263-333`.

5. **Há modos sem docker restart e sem essa espera.** `--reload-agendado` e `--reload-copia` definem `AGENDADO=1`; esse ramo usa HUP nas três cascas. `bin/deploy.sh:54-60`, `:237-251`. A espera por marca está no `else` do ramo. `bin/deploy.sh:266-279`. Ao especificar “todo pouso”, delimitar o contrato para a operação destrutiva de restart evita confundir HUP com restart. `bin/deploy.sh:237-279`. A configuração instalada do reload é conferida por `selo_reciclagem.sh`; não é verificada aqui no host de produção. `bin/selo_reciclagem.sh:70-75`; situação instalada [nao verificado].

6. **O contador existente não é a medida “rc137 no minuto de deploy”.** `crons_quebrados` considera a última corrida por `(nome, emp)`, nas últimas 26h, dentre as últimas 20 mil linhas, e conta qualquer exit diferente de 0/2. `app/core/services/crons_quebrados.py:16-17`, `:33-47`. O placar chama esse contador. `bin/placar_code.sh:451-455`. Assim, ele pode deixar de contar uma morte após uma recuperação e não correlaciona cron com deploy. `app/core/services/crons_quebrados.py:41-47`.

7. **Fontes para construir a medida existem, mas falta definir a correlação.** O runner registra `nome`, `emp`, `ini`, `fim`, `dur`, `exit`. `bin/cron_run.sh:13`. O histórico de deploy registra hora/head/0 somente depois das provas, não no instante do restart. `bin/deploy.sh:279`, `:342-347`. Definir log de restart, janela temporal, denominador e tratamento de ausência de registro são requisitos ainda não comprovados por um contador implementado; valor atual da medida pedida [nao verificado].

8. **O vigia acusa depois da morte; não preserva o elo.** O runner cria flag/pauta quando RC é diferente de 0/2 e recupera quando volta bem. `bin/cron_run.sh:29-35`. O carimbo de fim só nasce para RC 0/2. `bin/cron_run.sh:22-25`. A guarda nova precisa impedir a morte antes do restart; manter o vigia não satisfaz por si o contrato. `bin/deploy.sh:278-279`; `bin/cron_run.sh:22-35`.

9. **Selos mínimos a cobrar no desenho:** marca viva de casca deve impedir restart e terminar por fim do cron ou recusa; marca de host não deve segurar; órfã não deve travar; nascimento concorrente deve usar a mesma exclusão do restart; mutante “avisa e reinicia” deve reprovar. São critérios propostos a partir dos furos do ramo atual, não testes já existentes. `bin/deploy.sh:271-279`; existência/resultado desses novos selos [nao verificado].

## 02/11 18:24 · sessao b1c5436d1ca9 · E4-CALENDARIO-L1 · verificador · origem=Code

**Veredito: bloqueia.** A leitura direta do D4 entrou, mas a janela mensal pode produzir status diferente da consulta unitária; o template também conserva rótulos próprios. (`app/colaboradores/services/calendario.py:572`; `app/ponto/status_dia.py:249`; `app/ponto/turnos.py:1297`; `app/templates/colaboradores/partials/_calendario_grade.html:103`)

**1. Quem escreve e quem lê — não procede a acusação de novo escritor persistente no diff.**

| Campo/valor | Quem produz/escreve | Quem lê |
|---|---|---|
| `CelulaDia.status_dia` | Regra `status_da_lavra`; porta calcula o alvo, atribui e salva junto do veredito. (`app/ponto/status_dia.py:92`; `app/ponto/portas/celula.py:686`; `:705`; `:773`) | D4 lê a coluna; calendário copia a resposta; resumo conta esse status. O cartório também verifica se a coluna já existe. (`app/ponto/status_dia.py:266`; `app/colaboradores/services/calendario.py:653`; `:766`; `app/ponto/services/cartorio.py:882`) |
| `CelulaDia.status_rotulo` | Mesma porta e mesmo alvo do status. (`app/ponto/portas/celula.py:705`; `:773`) | D4 lê, com fallback para `ROTULO_STATUS`; calendário publica como `status_rotulo`. (`app/ponto/status_dia.py:268`; `app/colaboradores/services/calendario.py:718`) |
| Resposta transitória `status`/`rotulo` | D4 produz por `_lido` ou pela coluna lavrada; não persiste o andamento. (`app/ponto/status_dia.py:174`; `:260`; `:267`) | Calendário e painel consomem a resposta. (`app/colaboradores/services/calendario.py:653`; `:718`; `app/core/services/painel_op.py:338`) |
| `ROTULO_EM_CURSO` | Constante do núcleo. (`app/ponto/status_dia.py:61`) | D4 escolhe a frase quando o resultado já é `previsto` e `data <= hoje`; calendário apenas repassa. (`app/ponto/status_dia.py:298`; `app/colaboradores/services/calendario.py:718`) |
| `faixa` | A2 seleciona o vencedor e monta o dicionário. (`app/ponto/precedencia.py:393`; `:400`) | D4 lê para natureza; calendário fornece os fatos previamente carregados. (`app/ponto/status_dia.py:287`; `app/colaboradores/services/calendario.py:473`; `:572`) |
| `dia['ausencia']` | Cobertura seleciona a ausência; calendário monta o chip, lendo efeito e catálogo. (`app/ponto/turnos.py:2753`; `:2769`; `app/colaboradores/services/calendario.py:451`; `:663`) | Template dá prioridade ao chip. (`app/templates/colaboradores/partials/_calendario_grade.html:97`) |
| `CelulaDia.ausencia` | É outro campo: escrito pelas operações `carimbar` e `carimbar_insumos_em_lote`, dentro da porta. (`app/ponto/portas/celula.py:802`; `:838`; `:843`) | Não é a fonte do chip carregado pelo calendário. (`app/colaboradores/services/calendario.py:451`; `:650`) |
| `sem_fim` | Booleano local do D4; nenhuma persistência. (`app/ponto/status_dia.py:256`) | Ramos de andamento e previsão. (`app/ponto/status_dia.py:262`; `:295`) |
| `motivo` | Juíza temporal retorna `turno_aberto*`, `sem_previsto`, `relogio` ou `None`; carregador publica por data. (`app/ponto/turnos.py:2114`; `:2117`; `:2121`; `:2122`; `:2166`) | D4 lê e decide andamento/lavra/disponibilidade. (`app/ponto/status_dia.py:253`; `:255`; `:259`; `:266`) |

**2. Leitores restantes — fila; rótulo próprio no calendário — bloqueia.**

- **Bloqueia:** `escala_suspeita` ainda recebe tooltip escrito no template, ignorando `dia.status_rotulo` e usando a régua do vínculo ativo. O D4, por sua vez, devolve o rótulo lavrado. (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/colaboradores/services/calendario.py:265`; `:730`; `app/ponto/status_dia.py:268`)
- **Bloqueia a fidelidade do rótulo:** o rodapé afirma que todo `aberto` “não teve saída”, embora o novo D4 produza `aberto` para par já fechado antes do fim previsto. (`app/templates/colaboradores/partials/_calendario_grade.html:169`; `app/ponto/status_dia.py:262`; `:263`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:156`)
- **Fila, já declarada:** espelho, app e furos continuam classificando o dia por regras próprias. (`app/ponto/services/espelho.py:152`; `:183`; `app/api/views.py:1570`; `:1627`; `app/relatorios/furos.py:32`; `:162`; `app/core/medida_leitor_do_dia.py:323`)
- **Fila:** PDF recebe os dias desse espelho e acrescenta frases próprias de isenção e turno aberto. (`app/relatorios/pdf_espelho.py:592`; `:919`; `:921`; `:958`)
- **Não procede como duplicação do status diário:** drawer e view chamam o calendário; vigia chama a porta de oferecer resolução; painel guarda a resposta D4. (`app/colaboradores/services/drawer.py:120`; `app/colaboradores/views.py:1945`; `app/chamados/services/vigia_fio.py:280`; `:289`; `app/core/services/painel_op.py:338`)
- **Não procede como julgamento de dia encerrado:** `escala/views.py` deriva previsão e seu rótulo, papel expressamente declarado. (`app/escala/views.py:49`; `:54`; `:60`; `app/core/medida_leitor_do_dia.py:300`)
- **Não procede confundir TXT com status diário:** `classificar_export` responde entrada no TXT e consulta retenção por outra autoridade. (`app/folha/export.py:746`; `:942`; `:1212`)
- **Fila documental:** os ícones ainda citam `calendario.ROTULO_STATUS` em comentários; isso não executa import. (`app/templates/core/_icone_dia_extra.html:6`; `app/templates/core/_icone_dia_previsto.html:6`; `app/templates/core/_icone_dia_folga.html:5`) Nenhum import executável de nome morto apareceu na busca textual; garantia por importação/AST completa: **[nao verificado]**.

**3. Passado, relógio e meia-noite.**

- **Não procede:** passado com `MOTIVO_SEM_PREVISTO` não entra em `sem_fim`; com coluna lavrada, lê a coluna; sem lavra e com batida, retorna `sem_certidao`. Os dois casos possuem testes específicos. (`app/ponto/status_dia.py:256`; `:266`; `:271`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:410`; `:417`)
- **Não procede proibir todo `aberto` passado:** ele continua possível quando a própria juíza informa `turno_aberto*`; esse ramo não decide pela data. (`app/ponto/status_dia.py:259`; `app/ponto/turnos.py:2114`)
- **Não procede nova leitura de relógio no caminho do status:** calendário captura `_agora`, injeta no D4, que deriva `hoje` desse instante e o repassa à juíza. `data <= hoje` escolhe apenas o rótulo de um status já determinado. (`app/colaboradores/services/calendario.py:134`; `:572`; `app/ponto/status_dia.py:231`; `:233`; `:253`; `:298`)
- **Fila preexistente:** existe outra leitura de relógio no calendário, para permissão de desfazer retratação. Ela não alimenta o D4. (`app/colaboradores/services/calendario.py:221`; `:225`; `:572`)
- **Não procede teto somente por data no encerramento:** a juíza compara `agora >= fim` e considera turno vivo; o defeito de meia-noite encontrado está na atribuição da órfã conforme a janela, detalhado no item 7. (`app/ponto/turnos.py:2114`; `:2119`; `:2122`; `:1297`)

**4. Classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo juiz | **Ausente no classificador removido do calendário; presente como fila nos demais leitores declarados.** (`app/colaboradores/services/calendario.py:78`; `app/core/medida_leitor_do_dia.py:323`) |
| Segundo escritor fora da porta | **Não identificado no diff de produção; não procede.** As atribuições persistentes examinadas continuam na porta. (`app/ponto/portas/celula.py:705`; `:773`) |
| Relógio fora da borda | **Ausente como introdução no status; presente preexistente para desfazer, fila.** (`app/colaboradores/services/calendario.py:134`; `:221`; `:572`) |
| Derivação paralela de fonte única | **Presente: bloqueia no rótulo do template; fila nos outros leitores.** (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/core/medida_leitor_do_dia.py:323`) |
| Selo sem mordida suficiente | **Presente, fila:** paridade examina dicionários, sem render; propagação de erro A2 só exercita caminho feliz. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:202`; `app/colaboradores/tests/test_calendario_um_juiz.py:92`) |
| Ordem de trava | **[nao verificado]** |
| Pouso antes do pré-requisito | A exigência está documentada, e a cópia contém a alteração de isenção; sequência efetiva de pouso/apply: **[nao verificado]**. (`app/docs/RELATO.md:19`; `app/ponto/services/cartorio.py:520`) |

**5. Testes apagados e asserções reescritas.** [nao verificado]

- **Fila:** perdeu-se a fixture adversária explícita “lavra absolve enquanto grade/motor acusam”, dos testes apagados. Os novos casos usam `ok` com batidas exatas e `irregular` sem batida; falta acrescentar essa oposição em `CalendarioLeODoD4Test`, alternando somente a coluna lavrada. (`app/colaboradores/tests/test_calendario_ata_decide.py:23`, arquivo apagado no diff fornecido; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:117`; `:126`)
- **Não procede perda da regra de isenção:** há cobertura funcional na regra, na porta e na tela, incluindo o rótulo; o teste apagado verificava apenas texto-fonte. (`app/ponto/tests/test_e3_status_lavrado.py:62`; `:220`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:148`; `:192`; `app/colaboradores/tests/test_calendario_isencao.py:8`, arquivo apagado no diff)
- **Não procede vacuidade total do teste retargetado:** ele diferencia efetivamente `ok` de `irregular` e exige `None != ok`. O caso chamado “missing” agora injeta diretamente `irregular`; deixou de provar a antiga relação grade-missing → status, retirada do leitor. (`app/colaboradores/tests/test_t4_lei_status_none_alerta.py:114`; `:123`; `:129`; `:138`; `app/colaboradores/services/calendario.py:366`)
- **Fila:** falta injetar falha de `fatos_do_periodo` e exigir propagação; `assertNoLogs` no caminho feliz não protege o novo `raise`. (`app/colaboradores/tests/test_calendario_um_juiz.py:92`; `app/colaboradores/services/calendario.py:485`)
- **Bloqueia:** P14/P16 precisa ganhar o caso do item 7; também falta teste de render que detecte o tooltip próprio. Os testes atuais comparam apenas `status`/`status_rotulo` do contexto. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:202`; `:211`; `app/templates/colaboradores/partials/_calendario_grade.html:103`)

**6. Baixa feriado/prazo — não procede a reprovação.**

A impressão removida pertencia ao classificador apagado, e o teste nomeado existe: distingue `trabalha_em_feriado=False → folga` de `True → previsto`, com cadastros separados e comparação dos resultados. A2 efetivamente conserva trabalho quando o feriado só afeta pagamento. (`app/core/juizes.py:456`; `app/colaboradores/services/calendario.py:78`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:323`; `:333`; `:343`; `app/ponto/precedencia.py:373`; `:378`) Execução verde desse teste: **[nao verificado]**.

**7. Contraexemplo além do golden — bloqueia.**

Caso sintético: vínculo 12x36 noturno 19–07, sem intervalo, âncora 08/09/2026; única batida próxima é `S` em 09/09 às 07h; sem células em 07–10/09; existe DNA 19–07 distante, em 02/09; sem ausência ou feriado em 09/09; instante posterior ao encerramento. A alternância faz 09/09 ser folga. (`app/escala/models.py:1260`; `app/ponto/precedencia.py:380`)

| Consulta | Caminho | Resultado |
|---|---|---|
| Calendário mensal | DNA distante torna o mapa não vazio; véspera ausente no mapa impede usar o template; órfã fica em 09/09. D4 vê batida e nenhuma lavra. (`app/escala/utils.py:103`; `app/ponto/turnos.py:1297`; `:285`; `app/ponto/status_dia.py:252`; `:271`) | **`sem_certidao`** |
| D4 unitário de 09/09 | Pad 07–10/09 não contém DNA; mapa vira `None`; template atribui a órfã a 08/09, filtrada fora da consulta. Sem batida em 09/09, natureza responde folga. (`app/ponto/turnos.py:1845`; `:2993`; `:1300`; `:295`; `:1993`; `app/ponto/status_dia.py:291`) | **`folga`** |

A sonda pura do pareador, executada em memória, confirmou as duas atribuições da mesma saída: **09/09 com mapa distante; 08/09 sem mapa**. A integração completa calendário/D4 e ocorrência desse cadastro em produção permanecem **[nao verificado]**. (`app/ponto/turnos.py:1297`; `:1300`; `:1494`)

## 02/11 18:22 · sessao 17d91a766eb9 · - · - · origem=Ronald (tmux)

**Bloqueia — janela muda o status do mesmo dia.** Contraexemplo estático: vínculo 12x36 noturno 19–07, sem intervalo, âncora 08/09; única batida na região: `S` em 09/09 às 07h; nenhuma célula em 07–10/09; célula com DNA 19–07 em 02/09; agora 20/09. A natureza de 09/09 é folga pela alternância (`app/escala/models.py:1260`).

- Calendário mensal: o DNA de 02/09 torna o mapa não vazio; como 08/09 falta nele, `_vespera` retorna `None`; a saída órfã fica em 09/09. D4 recebe `tem_batida=True` e devolve `sem_certidao` (`app/escala/utils.py:103`; `app/ponto/turnos.py:1297`; `app/ponto/turnos.py:285`; `app/ponto/turnos.py:1494`; `app/ponto/status_dia.py:252`; `app/ponto/status_dia.py:271`).
- D4 unitário de 09/09: pad 07–10/09 sem DNA faz o mapa virar `None`; `_vespera` usa template 19–07; a órfã é atribuída a 08/09 e descartada do resultado unitário. Sem batida em 09/09, D4 lê natureza folga e devolve `folga` (`app/ponto/turnos.py:1845`; `app/ponto/turnos.py:2993`; `app/ponto/turnos.py:1300`; `app/ponto/turnos.py:291`; `app/ponto/turnos.py:1990`; `app/ponto/status_dia.py:291`).
- É dedução dos ramos da cópia; reprodução executada e ocorrência em produção: **[nao verificado]**.

**Não procede — passado `sem_previsto` virar aberto por data.** O predicado exclui passado; nesse ramo, com lavra lê a coluna, sem lavra e com batida entrega `sem_certidao`. Há testes específicos para ambos (`app/ponto/status_dia.py:256`; `app/ponto/status_dia.py:266`; `app/ponto/status_dia.py:271`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:410`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:417`).

**Não procede — nova leitura temporal do status fora da borda.** Calendário captura `_agora`, passa-o ao D4, que obtém `hoje` desse instante e injeta o mesmo instante na juíza. O rótulo `data <= hoje` apenas escolhe a frase depois de `s == 'previsto'` (`app/colaboradores/services/calendario.py:134`; `app/colaboradores/services/calendario.py:572`; `app/ponto/status_dia.py:231`; `app/ponto/status_dia.py:253`; `app/ponto/status_dia.py:298`).

**Fila — segunda leitura de relógio preexistente no calendário.** `_agora_retr = timezone.now()` decide prazo de desfazer; não participa do status nem foi acrescentada pelo diff (`app/colaboradores/services/calendario.py:221`; `app/colaboradores/services/calendario.py:225`).

**Não procede — meia-noite tratada só por data no encerramento.** Marcos são encadeados em instantes, avançando dia quando necessário; a juíza compara `agora >= fim` e prioriza turno aberto vivo (`app/escala/servico_jornada.py:79`; `app/escala/servico_jornada.py:89`; `app/ponto/turnos.py:2114`; `app/ponto/turnos.py:2119`).

**Fila — custo de CPU não medido pelo teto de queries.** Para cada par, D4 recalcula `dias_encerrados` da carga inteira; esse método percorre todos os dias da janela. Com uma carga mensal, há repetição quadrática em dias, embora turnos e consultas estejam memoizados (`app/ponto/status_dia.py:248`; `app/ponto/status_dia.py:253`; `app/ponto/turnos.py:3004`; `app/ponto/turnos.py:2162`).

## 02/11 18:21 · sessao 4253b846026f · - · - · origem=Ronald (tmux)

- **Fila — três classificadores paralelos persistem**, já declarados: app próprio em `app/api/views.py:1570` e `app/api/views.py:1627`–`1653`; espelho próprio em `app/ponto/services/espelho.py:152`–`171`, gravado no dicionário em `:183`; furos por missing/veredito em `app/relatorios/furos.py:32`–`83`, usado em `:162`–`171`. O inventário restante os reconhece em `app/core/medida_leitor_do_dia.py:323`–`331`.

- **Bloqueia a alegação de rótulo único no render**: `_calendario_grade.html:103` mantém tooltip próprio para `escala_suspeita`, ignorando `dia.status_rotulo`; ele usa `regua_rotulo` do vínculo ativo (`app/colaboradores/services/calendario.py:265`, `:730`). D4 copia o rótulo lavrado (`app/ponto/status_dia.py:283`–`286`), portanto template e D4 podem imprimir frases diferentes. O medidor não vê templates (`app/core/medida_leitor_do_dia.py:338`).

- **Contraexemplo visual além do golden**: declaração parcial 13–17, jornada 08–12/13–17, sem batidas, resto cobrado, lavra `irregular`: o teste confirma esse status em `app/ponto/tests/test_cobparc_v4_procedentes.py:262`–`269`; calendário mantém `ausencia` no mesmo dicionário (`app/colaboradores/services/calendario.py:650`, `:657`–`667`, `:717`), e o template escolhe o chip de ausência antes de qualquer ramo de status (`app/templates/colaboradores/partials/_calendario_grade.html:97`–`100`). A confirmação por render executado é **[nao verificado]**. O teste confere só o dicionário (`app/ponto/tests/test_cobparc_v4_procedentes.py:259`–`260`).

- **Não procede — drawer, view e vigia como novos juízes de status**: drawer chama o calendário (`app/colaboradores/services/drawer.py:120`–`121`); view também (`app/colaboradores/views.py:1945`–`1946`); vigia importa e chama apenas a porta de `resolver_dia_oferecido` (`app/chamados/services/vigia_fio.py:280`, `:289`).

- **Não procede — painel recalcular status diário**: o painel guarda a resposta do lote diretamente em `linha['dia']` (`app/core/services/painel_op.py:334`–`338`). Seu campo `status` é presença, distinguida expressamente do status diário (`app/core/medida_leitor_do_dia.py:305`–`307`). Contudo, gate usa carga `[hoje-2,hoje]` e D4 é chamado sem essa carga (`app/core/services/painel_op.py:327`, `:334`, `:336`); paridade dessas janelas permanece **[nao verificado]**.

- **Não procede como novo juiz diário — escala/views**: ainda seleciona rótulo por trabalho/folga/indefinição (`app/escala/views.py:54`, `:60`), mas é tela de previsão, explicitamente declarada (`app/escala/views.py:49`–`53`; `app/core/medida_leitor_do_dia.py:300`–`302`). O relógio é `datetime.date.today()` em `app/escala/views.py:26`.

- **Fila — PDF herda a fonte antiga**: recebe os dias do espelho (`app/relatorios/pdf_espelho.py:592`–`598`), cujo classificador próprio permanece em `app/ponto/services/espelho.py:152`–`183`. Principal palavra impressa vem de `palavra_dia` (`app/relatorios/pdf_espelho.py:919`–`920`); agrega frases próprias de isenção (`:921`–`922`) e turno aberto (`:958`–`967`).

- **Não procede — confundir status do TXT com status diário**: `classificar_export` responde entrada no TXT (`app/folha/export.py:746`–`751`), lê retenção em `:942`–`944`; retenção responde por veredito/protestos/data (`:1212`–`1238`). Não lê as novas colunas de status nesses trechos (`app/folha/export.py:1188`, `:1212`–`1238`).

- **Fila documental — nomes mortos em comentários dos ícones**: `calendario.ROTULO_STATUS` permanece em `app/templates/core/_icone_dia_extra.html:6`, `_icone_dia_espuria.html:6`, `_icone_dia_previsto.html:6`, `_icone_dia_feriado.html:6`, `_icone_dia_folga.html:5`; são comentários, não imports executáveis. Os desenhos só contêm SVG (`app/templates/core/_icone_dia_previsto.html:7`–`9`; `_icone_dia_falta.html:8`–`14`). Nenhum import Python de nome morto foi encontrado na busca textual realizada **[nao verificado por AST/importação]**.

- **Fila temporal preexistente**: calendário captura o instante principal em `app/colaboradores/services/calendario.py:134`, mas lê outro relógio para permissão de desfazer retratação em `:221`–`225`. Esse segundo instante não alimenta o status D4, enviado com `_agora` em `:573`–`574`.

## 02/11 18:21 · sessao ae0f3405deb2 · - · - · origem=Ronald (tmux)

- **Não procede — baixa feriado/prazo.** A baixa nomeia a impressão eliminada e o teste correspondente em `app/core/juizes.py:456-460`. `D4NaturezaFeriadoTest` realmente distingue quem folga de quem trabalha no feriado: esperados em `app/ponto/tests/test_e4cal_l1_d4_natureza.py:322-324`, cadastro dos dois casos em `:329-335`, comparação com a regra em `:343-344`. Execução verde: [nao verificado].

- **Não procede — apagar teste de isenção deixou regra sem cobertura.** O teste apagado verificava texto-fonte; a propriedade funcional sobrevive na regra (`app/ponto/tests/test_e3_status_lavrado.py:62-66`), no escritor real (`:220-224`) e no calendário, incluindo o rótulo (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:148-150`, `:176`, `:192`). O novo teste da tabela exige a equivalência exata `ROTULO_ISENTO ⇔ status ok e isento`: `app/ponto/tests/test_e4cal_l1_tabela_da_lavra.py:84-94`.

- **Fila — perdeu-se fixture explicitamente adversária da soberania da ata perante grade/motor.** Os casos apagados `test_MORDE_ata_concorde_vence_a_grade_que_acusa` e `...vence_o_desvio_do_motor` estão no diff fornecido (antigo arquivo, linhas 23-27). Os substitutos de tela lavram `ok` com batidas exatas 07/19 e lavram `irregular` sem batidas: `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:117-126`. A paridade tela/D4 existe (`:202-217`), mas a fixture deveria acrescentar dia com coluna `ok`, pontas/atraso que acusam, e trocar apenas a coluna para `irregular`; assim morderia a volta da derivação do leitor. A regra pura permanece coberta para `concorde→ok` e `furo→irregular`: `app/ponto/tests/test_e3_status_lavrado.py:43-49`.

- **Fila — selo “sem engolir erro” não prova propagação da falha.** `app/colaboradores/tests/test_calendario_um_juiz.py:92-99` executa somente o caminho feliz com `assertNoLogs`; não injeta exceção A2. A nova obrigação é o `raise` em `app/colaboradores/services/calendario.py:485`; sua remoção não é observada por esse caso. Falta um caso que faça `fatos_do_periodo` falhar e exija a mesma exceção no calendário.

- **Não procede — retarget de `None` ficou totalmente vazio.** O teste reescrito altera efetivamente a coluna consumida (`app/colaboradores/tests/test_t4_lei_status_none_alerta.py:114-121`) e verifica que `ok` e `irregular` produzem respostas diferentes (`:123-127`), além de `None != ok` (`:129-132`). O nome `test_celula_missing_segue_irregular` tornou-se impreciso: agora injeta diretamente `irregular`, sem célula de grade missing (`:138-139`); a antiga propriedade de missing→irregular foi deliberadamente retirada do leitor, conforme `app/colaboradores/services/calendario.py:366-368`.

- **Não procede — L1 passado sem previsto ficou sem teste.** O teste confirma chegada real a `MOTIVO_SEM_PREVISTO` em `app/ponto/tests/test_e4cal_l1_d4_natureza.py:396-402`; depois exige passado sem lavra `sem_certidao` e passado lavrado `folga_trabalhada` em `:410-421`.

- **Ordem de trava / pouso antes de pré-requisito: [nao verificado].** O desenho descreve D5 em `app/docs/ADR/0002-celula-e-o-dia.md:69-71` e permite migração E4 um leitor por pouso em `:75-77`; isso não prova execução de backfill, suite, trava ou pouso. RED anterior à cura e GREEN dos bytes auditados: [nao verificado].

## 01/11 20:45 · sessao 6bee4d54689e · COBERTURA-PARCIAL-UM-JUIZ · verificador · origem=Code

**Pré-verificação: bloqueada.** Há perda dos carimbos no legacy, emissores fora do predicado comum e perda do instante anterior em passadas do mesmo ato. Esses pontos impedem confirmar as intenções 1–3 nos caminhos completos (`app/escala/utils.py:1060`; `app/ponto/services/cluster_espurio.py:88`; `app/chamados/services/disputa_emissao.py:763`; `app/ponto/services/cartorio.py:914`).

A equivalência da cópia com o hash/base informados, a suite Django, o deploy, os hashes reais de produção e a incidência dos contracasos abaixo estão **[nao verificado]**. [nao verificado]

1. **BLOQUEIA — A/F/G: o legacy perde a identificação da ausência parcial e pode dispensar o resto indevidamente.**

   `_marcar_ausencias` escreve `cobertura_parcial` e `periodo_parcial`, mas o legacy remonta o resultado copiando apenas alguns campos: os dois carimbos ficam para trás, enquanto as células `dispensada` sobrevivem. O dispatcher continua oferecendo esse caminho quando a flag está falsa (`app/escala/utils.py:1247`; `app/escala/utils.py:1251`; `app/escala/utils.py:1058`; `app/escala/utils.py:1060`; `app/escala/utils.py:2106`).

   Contracaso: parcial 14–17, E13 ainda apagado e S17 dispensado. No resultado legacy falta `cobertura_parcial`; `dispensar_por_ausencia` interpreta essa ausência de chave como cobertura integral e acende também E13. A ata deixa de carregar os carimbos, `parcial_so_lista` retorna falso e `dia_sem_trabalho` volta a calar o dia inteiro (`app/ponto/services/cartorio.py:719`; `app/ponto/services/cartorio.py:726`; `app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:368`; `app/chamados/juizes.py:424`).

   Os testes da volta 6 não mordem esse defeito: criam ausências com `minutos=0` e verificam férias, aviso, falta e rótulo da ata, sem testar parcial e preservação dos carimbos nas duas flags (`app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:55`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:63`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:83`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:99`).

2. **BLOQUEIA — B/D/F: cluster nasce da célula e não lê `parcial_so_lista`.**

   O julgamento chama o reconciliador de cluster depois da lavra. Esse serviço lê a ata, passa por `dia_sem_trabalho`, procura duas órfãs próximas e alguma lâmpada apagada, e abre chamado diretamente. Não consulta o predicado comum (`app/ponto/services/cartorio.py:931`; `app/ponto/services/cluster_espurio.py:23`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/services/cluster_espurio.py:32`; `app/ponto/services/cluster_espurio.py:88`).

   Contracaso: colaborador ativo, vínculo ativo, ata de ausência parcial sem período, duas órfãs separadas por até 15 minutos e uma lâmpada apagada. A exceção nova faz `dia_sem_trabalho=False`; o cluster pode abrir chamado embora `parcial_so_lista=True`. A guarda global de absolvição não resolve isso: restringe-se a `MODULOS_DE_TURNO` (`app/ponto/services/cluster_espurio.py:77`; `app/ponto/services/cluster_espurio.py:79`; `app/chamados/juizes.py:424`; `app/ponto/services/cartorio.py:370`; `app/chamados/models.py:1061`).

   Isso viola a fronteira literal pedida — parcial sem período nunca vai a chamado — mesmo sendo um chamado de supervisão. Ocorrência nos quatro colaboradores apresentados: **[nao verificado]**.

3. **BLOQUEIA — B: o passivo também pergunta pelos marcos da ata sem aplicar a regra comum.**

   `pergunta_no_app.perguntar` encaminha o passivo para `criar_disputa_a_partir_de_chamados_orfaos`. Essa função abre o pai antes de consultar a ata do dia e complementar os marcos faltantes (`app/chamados/services/pergunta_no_app.py:148`; `app/chamados/services/pergunta_no_app.py:159`; `app/chamados/services/disputa_emissao.py:908`; `app/chamados/services/disputa_emissao.py:974`; `app/chamados/services/disputa_emissao.py:978`).

   A guarda da pergunta usa `_coberto_por_ausencia`, que passou a deixar passar parcial **com período**, sem consultar julgamento anterior ou fato novo. Portanto, um chamado preexistente pode ganhar perguntas para o resto de um dia que `parcial_so_lista` mandaria à lista. Na parcial **sem período**, a pergunta é barrada, mas o pai já pode ter nascido (`app/chamados/services/disputa_emissao.py:763`; `app/chamados/services/disputa_emissao.py:770`; `app/chamados/services/materializacao.py:78`; `app/chamados/services/materializacao.py:81`; `app/ponto/services/cartorio.py:372`).

   A exclusão do homônimo no censo não prova que esse fluxo esteja fora da obrigação: ele efetivamente lê os marcos da célula para criar perguntas (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:462`; `app/chamados/services/disputa_emissao.py:806`).

4. **BLOQUEIA — C: o instante anterior foi preservado na fábrica, mas não no corpo inteiro nem no c2.**

   | Leitor | Instante efetivamente usado |
   |---|---|
   | Primeira passada do corpo | `cel.julgada_em` anterior, antes do carimbo final (`app/ponto/services/cartorio.py:1618`; `app/ponto/services/cartorio.py:1733`). |
   | Fábrica dentro de `julgar_colab` | Snapshot anterior correto, inclusive quando seu valor é `None` (`app/ponto/services/cartorio.py:888`; `app/ponto/services/cartorio.py:938`; `app/chamados/services/fabrica_da_celula.py:102`). |
   | Segunda passada forçada | Mesmas instâncias, potencialmente já modificadas por `carimbar`; não recebe o snapshot (`app/ponto/services/cartorio.py:914`; `app/ponto/portas/celula.py:800`). |
   | Releitura pós-emissão | Também recebe as instâncias já carimbadas (`app/ponto/services/cartorio.py:979`). |
   | Lote | Último julgamento persistido antes da leitura do lote (`app/chamados/management/commands/disparar_perguntas_competencia.py:120`). |
   | c2 | Ata e `julgada_em` atuais do banco; pode ser o julgamento deste ato (`app/chamados/reconciliador.py:844`). |
   | Retroativo | Célula carregada antes do passe; o registro de cobertura, porém, vem do leitor que recalcula — achado 5 (`app/ponto/management/commands/emitir_furo_retroativo.py:69`; `app/ponto/management/commands/emitir_furo_retroativo.py:121`). |

   Quando o carimbo avança para `agora`, o intervalo de fato novo passa a começar nesse próprio ato. Para parcial com período, a segunda leitura pode mudar de “emite” para “só lista”; sem período, continua sempre “só lista” (`app/ponto/portas/celula.py:800`; `app/ponto/services/cartorio.py:370`; `app/ponto/services/cartorio.py:372`).

   Há um contracaso de fluxo: a ausência nova dispensa os marcos de uma cobrança antiga, deixando outro marco de fora ainda apagado. A cobrança antiga com fio é encerrada somente depois da primeira lavra; a segunda passada, que deveria poder cobrar o resto, já lê o carimbo novo. A fábrica conserva o snapshot anterior, mas o c2 volta a consultar o carimbo atual. Assim, fábrica e emissão podem decidir por instantes diferentes dentro do mesmo julgamento (`app/ponto/services/cartorio.py:1396`; `app/ponto/services/cartorio.py:899`; `app/ponto/services/cartorio.py:914`; `app/ponto/services/cartorio.py:938`; `app/chamados/reconciliador.py:845`). Reprodução integrada desse contracaso: **[nao verificado]**.

5. **BLOQUEIA — A/D: o leitor da célula continua derivando cobertura/período em vez de transportar a lâmpada lavrada.**

   A ata agora grava ambos os carimbos, mas `grade_da_celula` não os copia ao construir o dia. Depois consulta novamente a ausência atual e recalcula cobertura e período — inclusive no trecho alterado pelo diff (`app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:696`; `app/escala/services/leitor_celula.py:370`; `app/escala/services/leitor_celula.py:385`; `app/escala/services/leitor_celula.py:404`; `app/escala/services/leitor_celula.py:409`; `app/escala/services/leitor_celula.py:420`).

   Não é um segundo algoritmo de período: chama o mesmo juiz. É uma **segunda derivação em outro momento**, misturando lâmpadas persistidas com cadastro atual. Ata `sem_periodo` e cadastro já corrigido podem produzir respostas diferentes na fábrica/lote/c2 e no retroativo. A propriedade que compara grade e ata recém-produzidas não testa essa divergência temporal (`app/ponto/services/cartorio.py:346`; `app/escala/services/leitor_celula.py:404`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:247`). Incidência real dessa janela: **[nao verificado]**.

6. **FILA — B/D: a mordida dos mutantes AST é real, mas estreita; o universo não está fechado.**

   O censo reconhece as portas enumeradas, resolve `ImportFrom` e trata somente `_abrir_chamado` como chamada de método. Não reconhece emissão genérica por `ChamadoColaborador.abrir`, `PerguntaDisputa.abrir` nem chamada por módulo importado (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:311`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:353`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:388`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:394`).

   Além disso, qualquer chamada sintática ao predicado absolve a função: não se exige que seu resultado proteja a emissão. `if False: parcial_so_lista(...)` seguido de emissão continua conforme segundo esse algoritmo (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:390`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:403`).

   Os mutantes escritos verificam retirada da chamada, retirada da delegação e novo emissor pela porta conhecida. Isso sustenta essas propriedades sintáticas, não a afirmação universal sobre emissores e fluxo (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:455`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:465`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:472`).

   Outras portas exigem fronteira explícita:

   | Caminho | Situação |
   |---|---|
   | `detectar_ausencias` | Não lê o predicado; a exceção usa guarda mais restritiva que cala qualquer ausência (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:334`; `app/ponto/management/commands/detectar_ausencias.py:346`). |
   | `reconciliar_grade` | Emissão direta omitida; sua guarda cala ausência não-falta, portanto não demonstra vazamento da parcial por essa porta (`app/ponto/management/commands/reconciliar_grade.py:215`; `app/ponto/management/commands/reconciliar_grade.py:391`; `app/ponto/management/commands/reconciliar_grade.py:507`). |
   | Retroativo de turno aberto | Não lê o predicado, mas cala qualquer ausência antes da abertura (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`). |
   | Par relâmpago | Abre diretamente; origem é batida/par, exigindo distinguir essa fronteira da emissão pela ata (`app/ponto/management/commands/detectar_par_relampago.py:352`; `app/ponto/management/commands/detectar_par_relampago.py:368`). |
   | Régua degenerada | Chamado de cadastro; não é cobrança de marco (`app/escala/regua_defesa.py:36`; `app/escala/regua_defesa.py:51`). |
   | Declaração explícita | Também cria disputa/pergunta pelos marcos; há gesto humano, que precisa ser distinguido da emissão automática (`app/chamados/services/declaracao_texto.py:288`; `app/chamados/services/declaracao_texto.py:310`; `app/chamados/services/declaracao_texto.py:600`). |
   | Fio/reapontamento | O c2 dispensa a leitura de PSL ao costurar em chamado já vivo; isso é diferente de nascer/renascer (`app/chamados/reconciliador.py:805`; `app/chamados/reconciliador.py:842`). A ressuscitação de perguntas também ocorre antes do portão PSL do corpo (`app/ponto/services/cartorio.py:1536`; `app/ponto/services/cartorio.py:1618`). |

7. **FILA — A/D: ainda há leitores que descartam o resto parcial pelo rótulo `ausencia`.**

   O relatório alterado aceita ausência parcial, mas `furos_diarios` continua pulando todo dia que não seja `trabalho`; os calculadores de preenchimento e marcações fazem o mesmo. Portanto o resto pode aparecer no relatório e faltar no acompanhamento/indicadores (`app/relatorios/furos.py:154`; `app/escala/services/furos_diarios.py:39`; `app/inteligencia/calculadores.py:46`; `app/inteligencia/calculadores.py:141`).

   No acompanhamento persistido, essa seleção também alimenta a remoção de furos cuja premissa desapareceu. Não é apenas diferença visual (`app/escala/services/furos_diarios.py:89`; `app/escala/services/furos_diarios.py:98`).

8. **NOTA — E: a v4 não seleciona dias parciais; invalida impressões de toda a competência 09 em diante.**

   `versao_para` devolve a maior versão cujo horizonte alcança a competência empresarial. Assim, 09 passa de 1 para 4 e 10 passa de 3 para 4; não há filtro por ausência ou por colaborador nominal. A versão entra no hash dos dois ramos do cartório (`app/ponto/services/cartorio.py:111`; `app/ponto/services/cartorio.py:235`; `app/ponto/services/cartorio.py:238`; `app/ponto/services/cartorio.py:443`; `app/ponto/services/cartorio.py:1113`; `app/ponto/services/cartorio.py:1266`).

   Na próxima execução, células passadas com impressão antiga deixam de ser puladas sem `--forcar`. O comando percorre todos os colaboradores com célula passada da empresa, sem filtro de situação. A configuração confirma execução automática por cron `--apply`; disparo imediato pelo deploy está **[nao verificado]** (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/services/cartorio.py:1272`; `app/config/crons.py:597`).

   O rejulgamento usa **todo o código atual**: pode mover folga/trabalho, ata, protestos, veredito/status, chamados, perguntas e fios além dos dias parciais. O próprio teste cobre folga→trabalho na 09 sem forçar (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:146`; `app/ponto/services/cartorio.py:899`; `app/ponto/services/cartorio.py:935`; `app/ponto/services/cartorio.py:938`; `app/ponto/services/cartorio.py:1713`).

   Mesmo mantendo o veredito, o hash novo pode avançar `julgada_em`. Fora do diff, isso invalida lavras de perguntas e torna janelas históricas novamente elegíveis no censo de fase 12x36 (`app/ponto/portas/celula.py:796`; `app/ponto/portas/celula.py:800`; `app/chamados/veredito_lavrado.py:151`; `app/chamados/veredito_lavrado.py:187`; `app/escala/services/fase_12x36.py:188`).

   **No percurso de rejulgamento inspecionado**, a porta grava célula/outbox, e o consumidor atua em chamados/perguntas. Há guarda explícita para impedir recalcular o gravado da 09 exportada; o teste verifica FM inteiro e exportações com premissas não vazias. Isso não prova hashes reais nem execução em produção (`app/ponto/portas/celula.py:634`; `app/chamados/reconciliador.py:1310`; `app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:544`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118`). FM/TXT reais antes/depois: **[nao verificado]**.

9. **NOTA — A: mapa de escrita e leitura dos estados alterados.**

   | Campo/valor | Quem escreve | Quem lê e efeito relevante |
   |---|---|---|
   | `dia.cobertura_parcial` | Overlay da grade; também recalculado no leitor da célula (`app/escala/utils.py:1247`; `app/escala/services/leitor_celula.py:409`). | Supra, cartório, status, relatórios e predicado da lista (`app/ponto/supra_juiz.py:268`; `app/ponto/services/cartorio.py:719`; `app/ponto/status_dia.py:158`; `app/relatorios/furos.py:154`; `app/ponto/services/cartorio.py:368`). |
   | `dia.periodo_parcial` | Overlay pelo juiz de período; novamente derivado no leitor (`app/escala/utils.py:1251`; `app/escala/services/leitor_celula.py:420`). | Supra, impressão e PSL (`app/ponto/supra_juiz.py:381`; `app/ponto/services/cartorio.py:408`; `app/ponto/services/cartorio.py:370`). |
   | Carimbos correspondentes na ata | `ata_do_dia` copia somente quando há cobertura parcial; persistência pela lavra/carimbo (`app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:696`; `app/ponto/services/cartorio.py:1713`; `app/ponto/services/cartorio.py:1733`). | Fábrica, lote, c2, `dia_sem_trabalho` e calendário (`app/chamados/services/fabrica_da_celula.py:102`; `app/chamados/management/commands/disparar_perguntas_competencia.py:120`; `app/chamados/reconciliador.py:845`; `app/chamados/juizes.py:424`; `app/colaboradores/services/calendario.py:828`). |
   | `celula.dispensada` / `lampada.dispensada` | Grade dispensa missing dentro do período; cartório propaga; dispensa integral/justificativa também escreve (`app/escala/utils.py:1262`; `app/ponto/services/cartorio.py:520`; `app/ponto/services/cartorio.py:729`). | Leitor reconstrói marcador; contagens, status e cartório excluem dispensa de batida real (`app/escala/services/leitor_celula.py:127`; `app/escala/services/leitor_celula.py:91`; `app/ponto/status_dia.py:153`; `app/ponto/services/cartorio.py:1288`). |
   | `missing`, `acesa`, `luz`, `n_missing`, `n_celulas` | Grade altera missing; cartório monta lâmpadas/contagens e pode dispensá-las (`app/escala/utils.py:1263`; `app/ponto/services/cartorio.py:636`; `app/ponto/services/cartorio.py:671`; `app/ponto/services/cartorio.py:685`; `app/ponto/services/cartorio.py:726`). | Supra, lastro, fábrica, cluster e relatórios (`app/ponto/supra_juiz.py:306`; `app/ponto/services/lastro.py:129`; `app/chamados/services/fabrica_da_celula.py:49`; `app/ponto/services/cluster_espurio.py:31`; `app/relatorios/furos.py:154`). |
   | `n_ok`, `n_casadas` | Derivados da ata pelo leitor; não são novo campo persistente (`app/escala/services/leitor_celula.py:91`; `app/escala/services/leitor_celula.py:97`; `app/escala/services/leitor_celula.py:371`). | Supra prefere o número lavrado; corte de admissão/demissão e diagnóstico também mudam (`app/ponto/supra_juiz.py:229`; `app/ponto/supra_juiz.py:464`; `app/escala/services/leitor_celula.py:343`; `app/ponto/services/bordas_realizado.py:51`). |
   | `CelulaDia.ata` | Porta `lavrar_veredito`; operação própria para a chave `precedencia` (`app/ponto/portas/celula.py:709`; `app/ponto/portas/celula.py:768`; `app/ponto/portas/celula.py:811`). | Leitor da célula, juízes, lastro, perguntas, calendário e cluster — incluindo os consumidores fora do diff dos achados 2, 3 e 7 (`app/escala/services/leitor_celula.py:342`; `app/chamados/juizes.py:463`; `app/ponto/services/lastro.py:118`; `app/ponto/services/cluster_espurio.py:24`). |
   | `CelulaDia.julgada_em` | `carimbar`, com guarda de idempotência (`app/ponto/portas/celula.py:796`; `app/ponto/portas/celula.py:800`). | Impressão, PSL e seus emissores; invalidação de perguntas e fase, descritas nos achados 4 e 8 (`app/ponto/services/cartorio.py:1272`; `app/ponto/services/cartorio.py:372`; `app/chamados/veredito_lavrado.py:154`; `app/escala/services/fase_12x36.py:189`). |
   | `REGRA_VERSOES` / `regra` | Declaração no módulo; validação na carga (`app/ponto/services/cartorio.py:111`; `app/ponto/services/cartorio.py:209`). | `versao_para` → `_regra_do_dia` → impressão dos dois ramos (`app/ponto/services/cartorio.py:238`; `app/ponto/services/cartorio.py:1061`; `app/ponto/services/cartorio.py:1113`; `app/ponto/services/cartorio.py:1266`). |
   | `julgada_antes` | Snapshot local por data; sentinela diferencia omissão de `None` explícito (`app/ponto/services/cartorio.py:888`; `app/chamados/services/fabrica_da_celula.py:71`). | `perguntar_pela_celula` repassa a `motivos_a_perguntar`; não alcança corpo/c2 (`app/chamados/services/fabrica_da_celula.py:164`; `app/chamados/services/fabrica_da_celula.py:103`). |
   | `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | Continuam juntos em `carimbar`; entrada/saída vêm da ata transitória (`app/ponto/services/cartorio.py:1732`; `app/ponto/portas/celula.py:801`; `app/ponto/portas/celula.py:805`). | Pulo por impressão e leitores da célula; a obra muda a impressão por versão/período/índices dispensados (`app/ponto/services/cartorio.py:1272`; `app/ponto/services/cartorio.py:399`; `app/ponto/services/cartorio.py:443`). |
   | Veredito, prote

[truncado em 20000 de 30171 caracteres]

## 01/11 20:41 · sessao 3d0aefa7323b · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — cluster é emissor omitido e passa a alcançar parcial sem período.** `cartorio.py:931-935` chama `cluster_espurio.reconciliar_colab` após lavrar; `cluster_espurio.py:23-35` lê ATA, exige duas órfãs próximas e lâmpada apagada; `:88` abre chamado sem `parcial_so_lista`. A mudança em `chamados/juizes.py:423-426` faz `dia_sem_trabalho` devolver False para a parcial. Contracaso: colaborador ativo, EC ativa, parcial sem período, duas órfãs em até 15 min e missing → chamado de cluster apesar de PSL=True. As guardas globais não abrangem cluster: `chamados/models.py:1061`, `chamados/reconciliador.py:336`, `chamados/catalogo/furo_modulos.py:15`. Ocorrência em produção: [nao verificado].

2. **FILA/BLOQUEIA conforme fronteira exigida — passivo pergunta pela ATA sem PSL.** `pergunta_no_app.py:148-159` chama `disputa_emissao.criar_disputa_a_partir_de_chamados_orfaos`; esta cria pai em `:908-932`, lê ATA em `:974`, complementa marcos em `:978` e pergunta por `_criar_pergunta_se_couber:756-780`. A guarda `:763` usa `_coberto_por_ausencia`; parcial COM período agora devolve False (`materializacao.py:76-82`), independentemente de fato novo. Assim pode perguntar o resto de dia já julgado que PSL mandaria à lista. SEM período cala perguntas nessa guarda, mas pai pode ter sido criado antes dela. Esse caminho parte de chamado existente e foi excluído nominalmente pelo teste de homônimo (`test_cobparc5...py:454`); não está protegido pela propriedade geral anunciada. `declaracao_texto.declarar_hora_colab:288,310,325` e `aplicar_declaracao:600,608,614` também criam pai/pergunta a partir de marcos da célula sem PSL, porém mediante declaração/toque explícito. [nao verificado]

3. **FILA — censo prova presença sintática, não universo nem proteção do fluxo.** `_imports` reconhece apenas `ImportFrom` (`test_cobparc5...py:353`); chamadas por módulo não entram. `censo:394` só reconhece método `_abrir_chamado`, omitindo `ChamadoColaborador.abrir`, `PerguntaDisputa.abrir` e as portas de disputa. `violacoes:403` aceita qualquer chamada ao predicado, inclusive sob `if False`, sem exigir retorno ou domínio sobre emissão. Executei em memória os nove métodos AST reais: **9/9 OK**; os três contramutantes “via módulo”, “PSL inalcançável” e “porta genérica direta” também escaparam. Isso confirma mordida real e estreita dos mutantes escritos, sem equivaler à suite Django. Código das verificações: `test_cobparc5...py:374-413,430-484`. [nao verificado]

4. **C — snapshot correto só para fábrica; demais passadas podem ler julgamento do próprio ato.** `julgar_colab:888` captura anterior e passa-o à fábrica em `:938`; fábrica usa explicitamente esse argumento em `fabrica_da_celula.py:102-103`. Primeira passagem do corpo lê `cel.julgada_em` em `cartorio.py:1618`, antes de `carimbar:1733`; a porta escreve e modifica a instância em `portas/celula.py:800`. Segunda passagem forçada (`cartorio.py:914`) e releitura pós-emissão (`:979`) recebem as mesmas instâncias: se o primeiro carimbo mudou, PSL lê **agora**, não o anterior. COM período, sem fato posterior ao carimbo, passa a True; pode calar tentativa na segunda passagem. SEM período continua True independentemente do instante (`:370-372`). Efeito concreto em produção: [nao verificado].

5. **C — c2 lê o carimbo atual, inclusive dentro da fábrica do próprio julgamento.** `disputa_emissao.py:404-409` chama reapontamento após criar pergunta; `reconciliador.py:1531` chega ao c2, que consulta ATA e `julgada_em` atuais em `:844-845`, sem snapshot nem `agora`. Se fábrica rodou após carimbo, c2 vê o julgamento deste ato. COM período, um fato que habilitou fábrica pelo snapshot anterior pode já ter sido consumido pelo carimbo e não habilitar c2. Chamado do dia já vivo pula PSL por `_costura_no_vivo` (`:805-806,842`), o que preserva costura. O fallback temporal é relógio próprio: `cartorio.houve_fato_novo:317`. Lote lê julgamento anterior ao lote (`disparar_perguntas_competencia.py:120`); retroativo lê célula carregada antes do passe (`emitir_furo_retroativo.py:69,121`). [nao verificado]

6. **Portas adicionais, com alcance distinguido.**
   - `detectar_par_relampago.py:352-368`: abre chamado sem PSL; origem é batida/par completo, não ATA. Precisa fronteira explícita do universo.
   - `reconciliar_grade.py:250-254,488-530`: emissão direta omitida pelo AST; `_coberto:391-403`, consultado em `:216`, cala toda ausência não-falta, inclusive parcial. Não demonstra vazamento da parcial não-falta por esse caminho.
   - `reconciliar_grade.py:412-428`: pai de disputa por porta omitida.
   - `regua_defesa.py:36-78`: abre chamado de **cadastro** `regua_degenerada`, não cobrança de marco; exclusão legítima exige declarar essa fronteira.
   - `emitir_turno_aberto_retroativo.py:108-110`: não lê PSL, mas cala qualquer ausência antes de abrir em `:151`.
   - `detectar_ausencias.py:346-361`: allow tem fundamento na guarda de qualquer ausência; sua exceção enfraquece a formulação literal “todo emissor lê o mesmo predicado”, embora seja mais restritiva na emissão.

## 01/11 20:41 · sessao ebbf0da4d5d1 · - · - · origem=Ronald (tmux)

1. **Bloqueia — o leitor da célula ainda recalcula cobertura/período, embora a ata agora os grave.** `ata_do_dia` passa a transportar `cobertura_parcial` e `periodo_parcial` (`app/ponto/services/cartorio.py:689-696`), mas `grade_da_celula` não os copia ao construir o dia (`app/escala/services/leitor_celula.py:370-380`): chama `_carimbar_cobertura_parcial` (`:385`), consulta ausência atual (`:404`) e recalcula ambos (`:409-420`, trecho modificado no diff). Portanto fábrica/lote/c2 consultam a ata persistida e retroativo consulta uma composição ata+cadastro atual; uma ata com `sem_periodo` e cadastro já corrigido pode dar respostas distintas. A divergência é demonstrável pelo código; sua ocorrência nos quatro casos raspados é [nao verificado].

2. **Nota — v4 alcança todas as células de comp09 em diante, não apenas os dias parciais.** `REGRA_VERSOES` declara v4 em `'2026-09'` (`app/ponto/services/cartorio.py:111`); `versao_para` usa competência empresarial e devolve a maior versão aplicável (`:220-239`). O hash incorpora `regra:4` (`:443-444`) nos dois ramos (`:1113`, `:1267`), vencendo o pulo por impressão (`:1114`, `:1272-1278`). O comando entrega todas as células passadas da empresa, sem filtro de situação (`app/ponto/management/commands/processar_cartorio.py:49-65`). Assim o código inteiro atual pode mudar atas, vereditos/status, protestos, chamados, perguntas e fios além dos dias parciais. A configuração identifica execução automática pelo cron `--apply` (`app/config/crons.py:597`); disparo imediato pelo deploy é [nao verificado].

3. **Nota — outros leitores fora do diff observam o novo `julgada_em`.** A escrita persistente está em `carimbar`, que só poupa o relógio quando nenhum insumo/lavra mudou (`app/ponto/portas/celula.py:787-807`). O novo hash faz o relógio avançar mesmo se o veredito final permanecer igual. Fora do diff, ele compõe o marco de invalidação das perguntas (`app/chamados/veredito_lavrado.py:151-155`, `:187-190`) e reabre janelas históricas do censo de fase 12x36 (`app/escala/services/fase_12x36.py:184-193`). Mudança de ata/veredito também emite `EventoCelula` (`app/ponto/portas/celula.py:622-642`), cujo consumidor retrata chamados e encerra perguntas órfãs (`app/chamados/reconciliador.py:1306-1315`).

4. **Nota — `n_ok` muda também para dispensas integrais e justificativas antigas.** `_n_dispensadas` conta qualquer lâmpada com `dispensada`, não somente ausência parcial (`app/escala/services/leitor_celula.py:63-68`); `n_ok_lavrado` subtrai essa quantidade (`:91`) e `n_casadas_lavrado` delega (`:94-97`). A mesma alteração afeta `_teve_fato` e a exclusão por admissão/demissão (`:343-351`): no dia da demissão, uma ata apenas com dispensas deixa de sustentar “teve fato” e pode passar a `fora_vinculo`. O supra lê os números fornecidos (`app/ponto/supra_juiz.py:229-230`); o consumidor externo `bordas_realizado._teve_batida_na_ata` passa a observar `n_ok` menor (`app/ponto/services/bordas_realizado.py:49-51`), embora seu laço filtre dias de trabalho (`:74`).

5. **Nota — as escritas persistentes continuam pelas portas identificadas.** `CelulaDia.ata` nasce pela lavra (`app/ponto/portas/celula.py:709-782`); a chave `precedencia` tem operação própria que atravessa a substituição da ata (`:94`, `:732-734`, `:811-823`). `julgada_em` permanece em `carimbar` (`:800-807`). O cartório usa `lavrar_veredito(save=False)` e salva junto pelo carimbo (`app/ponto/services/cartorio.py:1713-1734`). A atribuição de ata em `escala/utils.py:1904` é a uma cópia em memória explicitamente criada em `:1903`, não uma escrita persistente nova. Não identifiquei nova aquisição de trava nesses trechos; deadlock por outro percurso é [nao verificado].

6. **Nota — o percurso de rejulgamento inspecionado não grava FM/TXT; a comprovação em produção falta.** A porta de célula salva a célula e insere outbox (`app/ponto/portas/celula.py:634-642`); o consumidor inspecionado atua em chamados/perguntas (`app/chamados/reconciliador.py:1306-1315`). A guarda de setembro está declarada (`app/ponto/services/fechamento.py:36-43`) e retira do universo de escrita colaboradores de empresas exportadas (`:542-559`). O teste verifica linha inteira de FM e todas as exportações, com premissas não vazias (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118-131`), além da mudança sem `--forcar` (`:146-151`). Execução desses testes e hashes reais antes/depois são [nao verificado].

7. **Nota — há caminho de cadastro para sair da lista; execução dos pré-requisitos não foi comprovada.** `_aplicar_periodo` escreve os campos pela porta `decidir` (`app/ponto/services/ausencia.py:875-917`); existem rota (`app/ponto/urls.py:66`), formulário de edição (`app/templates/ponto/partials/painel_ausencia.html:297-302`) e migration `app/ponto/migrations/0076_ausencia_periodo_parcial.py:1`. Migrate aplicado, smoke do Ronald, DIFF de frota e Pauta DP efetivamente produzidos são [nao verificado].

Inventário complementar: `REGRA_VERSOES` tem declaração em `cartorio.py:111`, validação em `:209-217`, único leitor produtivo `versao_para` em `:233-239`; seu chamador produtivo é `_regra_do_dia` em `:1059-1062`. `ata_do_dia` é usada nos ramos sem adesão (`:1125`), projeção para o julgamento (`:1299-1300`) e lavra final (`:1711`); `dispensar_por_ausencia` lê o carimbo da grade (`:719-720`) e escreve apenas a ata transitória (`:725-731`). `julgada_antes` é fotografada antes do primeiro corpo (`:888`), enviada à fábrica (`:938`), mas não à segunda passada (`:914`) nem à releitura (`:979`); estas podem ler o carimbo do próprio ato em `parcial_so_lista` (`:1618`). A consequência detalhada depende dos emissores e está na exploração paralela.

## 01/11 20:41 · sessao a863d9288f3c · - · - · origem=Ronald (tmux)

1. **Bloqueia — o legacy descarta os dois carimbos da parcial.** O overlay escreve `cobertura_parcial` e `periodo_parcial`, mas `_montar_grade_prevista_periodo_legacy` reconstrói o dia sem essas chaves. As células `dispensada` sobrevivem, criando combinação inconsistente: dia parcial sem a identificação da parcial. `dispensar_por_ausencia` passa a dispensar também os marcos de fora do período; `ata_do_dia` não grava os carimbos; `parcial_so_lista` retorna falso. Fontes: `app/escala/utils.py:1247`, `app/escala/utils.py:1251`, `app/escala/utils.py:1060`, `app/ponto/services/cartorio.py:719`, `app/ponto/services/cartorio.py:726`, `app/ponto/services/cartorio.py:689`, `app/ponto/services/cartorio.py:368`. Reproduzido executando somente em memória a função real extraída por AST, com carregadores e subgrade dublados: entrada com as duas chaves; saída sem ambas. Execução Django completa: [nao verificado].

2. **Fila — a bateria v6 não cobre essa perda.** O helper sempre cria ausência com `minutos=0`; os cenários cobrem férias, aviso integral, falta e folga com vínculo vivo, verificando principalmente rótulo e ata. Executar os mesmos casos nas duas flags não testa parcial nem preservação dos carimbos. Fontes: `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:55`, `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:63`, `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:83`, `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:94`, `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:99`.

3. **Nota — “uma vez por grade” vale para o montador por turno, não para o legacy inteiro.** Chamadas de produção de `_marcar_ausencias`: single em `app/escala/utils.py:807`; segmento legacy sem vínculo em `app/escala/utils.py:1029`; por turno em `app/escala/utils.py:1865`. O legacy chama o single por segmento com vínculo, portanto pode executar o overlay várias vezes na janela: `app/escala/utils.py:1021`, `app/escala/utils.py:1037`. Chamadas diretas em testes: `app/escala/tests/test_grade_segue_a_precedencia.py:40`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:60`.

4. **Nota — censo dos outros dois montadores.** `montar_grade_prevista_periodo_por_turno` é chamado pelo dispatcher (`app/escala/utils.py:2104`) e por `_score_geometria` do detector de par relâmpago (`app/ponto/management/commands/detectar_par_relampago.py:136`). Teste direto: `app/escala/tests/test_grade_por_turno_override_ponta.py:61`; testes indiretos pelo callable retornado por `_novo_or_skip`: `app/escala/tests/test_grade_por_turno.py:92`, `app/escala/tests/test_grade_por_turno.py:141`, `app/escala/tests/test_grade_por_turno.py:321`. `_montar_grade_prevista_periodo_legacy` tem o dispatcher como chamada de produção (`app/escala/utils.py:2106`); testes chegam pela flag falsa, incluindo `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:99`, `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:103`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:102`.

5. **Nota — impacto da ausência sem vínculo.** O builder por turno monta inicialmente folga com previsto zero; a ausência agora troca o rótulo e não cria jornada: `app/escala/utils.py:1818`, `app/escala/utils.py:1237`. O juiz de previsto continua retornando zero sem vínculo: `app/escala/utils.py:2277`. `EC.eh_dia_trabalho` lê `cel.trabalha`; `EC.marcos_do_dia` lê DNA ou template, sem consultar o novo rótulo da ata: `app/escala/models.py:1071`, `app/escala/models.py:1099`. O motor continua consultando esses insumos: `app/ponto/motor_calculo_v2.py:1241`, `app/ponto/motor_calculo_v2.py:2278`. A agregação de dinheiro passa a incluir o dia `ausencia`, porém com previsto zero gera abono zero e realizado limitado a zero; pode acrescentar chaves de valor zero aos mapas: `app/ponto/services/dia_pago.py:385`, `app/ponto/services/dia_pago.py:391`, `app/ponto/services/dia_pago.py:399`. Impacto monetário real na frota: [nao verificado].

6. **Nota — admissão/demissão continuam distinguindo leitores.** O overlay consulta cobertura da pessoa sem guarda por admissão, demissão ou situação: `app/escala/utils.py:1183`, `app/escala/utils.py:1205`. Já `grade_da_celula` devolve `fora_vinculo` após a demissão e antes da admissão sem fato, antes de ler o tipo da ata: `app/escala/services/leitor_celula.py:337`, `app/escala/services/leitor_celula.py:347`, `app/escala/services/leitor_celula.py:350`. Portanto a v6 não transforma universalmente esses dias em ausência em todos os leitores. Uma regra que obrigasse rejeitar a ausência já aprovada nesses casos não foi demonstrada pelo material: [nao verificado].

7. **Nota — ordem dos overlays e exceção da falta estão preservadas.** A2 declara ausência acima de folga (`app/ponto/precedencia.py:11`, `app/ponto/precedencia.py:18`); o builder aplica folga, depois ausência, depois isenção (`app/escala/utils.py:1861`, `app/escala/utils.py:1865`, `app/escala/utils.py:1867`). A condição impede falta aprovada de substituir folga: `app/escala/utils.py:1227`. A isenção usa a mesma função para quaisquer tipos, preservando o rótulo; células `None` de dia sem vínculo não são creditadas: `app/escala/utils.py:833`, `app/escala/utils.py:848`.

8. **Nota — rejeitadas e múltiplas ausências.** O overlay usa `para='folha'`, que aceita somente aprovadas: `app/escala/utils.py:1205`, `app/ponto/turnos.py:2514`. A cobertura do período desempata por maior `(data, pk)`, substituindo a ausência anterior; não combina períodos de várias ausências: `app/ponto/turnos.py:2756`, `app/ponto/turnos.py:2768`. A consulta de um dia usa a mesma ordenação: `app/ponto/turnos.py:2557`.

9. **Fila — leitor da célula recalcula os carimbos em vez de ler a ata.** `grade_da_celula` constrói o dia sem copiar `ata.cobertura_parcial`/`ata.periodo_parcial` e chama `_carimbar_cobertura_parcial`, que consulta a ausência atual e calcula novamente: `app/escala/services/leitor_celula.py:370`, `app/escala/services/leitor_celula.py:385`, `app/escala/services/leitor_celula.py:404`, `app/escala/services/leitor_celula.py:409`, `app/escala/services/leitor_celula.py:420`. Entre mudança do cadastro e nova lavra, esse caminho pode combinar lâmpadas antigas com cobertura/período atuais, enquanto fábrica/lote/c2 consultam os carimbos gravados. Ocorrência em produção: [nao verificado].

10. **Nota — escala_auto não consome diretamente esses montadores.** As propostas consultam `turnos_do_colab` e `EC.eh_dia_trabalho`; o executor altera o vínculo por sua porta: `app/escala/services/escala_auto.py:87`, `app/escala/services/escala_auto.py:144`, `app/escala/services/escala_auto_executor.py:103`. A v6 não modifica diretamente esses insumos; repercussões indiretas pela geometria/ata exigiriam execução adicional: [nao verificado].

## 01/11 18:57 -- HISTORICO (mais de 24 h) · sessao cc9e17bf9b48 · O261-E-F3 · censo · origem=Code

**O contador do F0 existe, mas mede um recorte: carimbadas, vivas pelo C6, em disputa aberta e fora das vias de identidade. Não equivale ao estoque inteiro de perguntas vivas sob destino terminal.** (`app/chamados/services/vigia_fio.py:743`, `app/chamados/services/vigia_fio.py:800`, `app/chamados/services/relavra_via.py:170`.)

Censo estático da cópia. Correspondência efetiva com o HASH/HEAD informado, quantidades atuais e ocorrência de cada classe em produção: **[nao verificado]**. As referências abaixo são relativas à raiz da cópia.

**1. Predicados e juízes**

| Pergunta | Autoridade existente e representações |
|---|---|
| “Esta PerguntaDisputa está viva?” | Juiz exato: `pergunta_viva(p)`, cuja regra está em `_pergunta_viva_calc`. Registrado como C6 na família chamado e novamente como autoridade da pergunta de tela TT8. (`app/chamados/juizes.py:799`, `app/chamados/juizes.py:1032`, `app/core/juizes.py:112`, `app/core/juizes.py:266`, `app/core/juizes.py:710`.) |
| Outras formas de consultar vida | `.viva` delega diretamente ao mesmo juiz; `perguntas_vivas_de` filtra por ele; `fio_vivo` reduz uma coleção com `any(pergunta_viva(...))`. São consumidores/composição da autoridade. (`app/chamados/models.py:2728`, `app/chamados/juizes.py:1128`, `app/chamados/juizes.py:1133`.) |
| Pré-filtro SQL | `q_vivas()` tem contrato de **continência**, não igualdade: pode trazer mortas, e quem precisa da resposta exata consulta C6. (`app/chamados/models.py:2697`, `app/chamados/models.py:2722`.) |
| “Este chamado está terminal?” | Pertencimento a `motor.TERMINAIS`: `resolvido`, `fechado`, `cancelado`, `superado`. `ENCERRADOS` contém somente `resolvido` e `fechado`; `VIVOS` contém `aberto`, `em_analise`, `registrado`. O registro C1 aponta para `motor.VIVOS`; a tabela documental explicita a família de conjuntos. (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/core/juizes.py:261`, `app/docs/JUIZES.md:51`.) |
| Outra expressão de chamado morto | `historico._morto` usa `status_local not in VIVOS`. Para os estados declarados no catálogo, corresponde ao complemento terminal; não há nesse helper outro cadastro de estados. (`app/chamados/services/historico.py:178`, `app/chamados/catalogo/motor.py:19`.) |

O lookup `JUIZES.md` não nomeia C6 no bloco de chamado: registra os conjuntos do motor; sua linha “FIO / DISPUTA” aponta para `_disputa_do_chamado`. O registro vivo nomeia explicitamente `pergunta_viva`; o próprio documento determina que o vivo prevalece. (`app/docs/JUIZES.md:10`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:51`, `app/core/juizes.py:266`.)

Duas distinções afetam o censo:

- **Carimbo não implica morte.** C6 considera validação, tranca, resposta, identidade, marco, célula, lei e testemunha do carimbo, nessa precedência. Resposta ainda por validar pode manter viva até uma pergunta carimbada por identidade. (`app/chamados/juizes.py:1032`, `app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1071`, `app/chamados/juizes.py:1107`.)
- O fallback de materialização lê **`ENCERRADOS`**, não `TERMINAIS`: cancelado/superado não satisfazem aquela testemunha. (`app/chamados/juizes.py:706`, `app/chamados/juizes.py:720`.)

**2. Contadores e vigias**

| Medidor | Unidade, universo e exclusões |
|---|---|
| `materializada_viva` / `carimbos_sem_lastro_por_dono` | Conta perguntas com carimbo, disputa **aberta**, via fora das quatro identidades e C6 viva. Divide em `aberta`/`exportada` pela competência do dia; sem data entra em `aberta`. Exclui disputas fechadas e identidades. **É exatamente o contador citado no F0 item 3**, agora na linha 800. (`app/chamados/services/vigia_fio.py:743`, `app/chamados/services/vigia_fio.py:760`, `app/chamados/services/vigia_fio.py:768`, `app/chamados/services/vigia_fio.py:800`, `app/chamados/services/relavra_via.py:166`.) |
| `censo_alcance` → `fio_morto_alcance` | Conta **dias retidos sem porta**, alimentados por `fora_da_folha`, nas empresas e competências enumeradas. Separa hoje/futuro e donos fora do fio. A causa `c` identifica respondida sob âncora terminal; o detalhe `pergunta_viva_sob_chamado_terminal` cobre outro ramo. Não é contagem de perguntas do acervo inteiro. (`app/chamados/services/vigia_fio.py:534`, `app/chamados/services/vigia_fio.py:545`, `app/chamados/services/vigia_fio.py:604`, `app/chamados/services/vigia_fio.py:630`, `app/chamados/services/vigia_fio.py:642`.) |
| `viva_em_disputa_fechada` | `q_vivas` + disputa fechada + C6 viva; retorna perguntas e disputas distintas. Não exige destino terminal nem carimbo; dono declarado é admin, sem reabertura automática pelo vigia. (`app/chamados/services/vigia_fio.py:692`, `app/chamados/services/vigia_fio.py:703`.) |
| Auditor `I2` / `I2p` | Busca **não carimbadas** com destino terminal. Retira do I2 as respondidas em disputa aberta e as publica em I2p. I2 conta chamados distintos; não consulta C6. (`app/chamados/management/commands/auditar_invariantes_chamados.py:586`.) |
| Auditor `I8b` / `I8bc` / `I8bcx` / `I8bx` | Exige destino e data presentes, C6 viva e **resolvedora inválida pelo turno**. Carimbadas vão para I8bc/I8bcx; não carimbadas sem célula e exportadas para I8bx; demais para I8b. Portanto I8bc não mede toda carimbada viva. A separação histórica de exportadas usa `_competencias_exportadas`, não `competencia_do_dia_entregue`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:885`, `app/chamados/management/commands/auditar_invariantes_chamados.py:890`, `app/chamados/management/commands/auditar_invariantes_chamados.py:904`, `app/chamados/management/commands/auditar_invariantes_chamados.py:923`.) |
| Auditor `I21` / `I21a` | Respondida, texto diferente de `''`, não validada, não carimbada, destino **e pai** fora de VIVOS. Separa ausência aprovada em I21a. Não consulta C6. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1568`, `app/chamados/management/commands/auditar_invariantes_chamados.py:156`.) |
| Auditor `I6` / pisos | Disputa fechada, recusa vazia, sem carimbo **ou** sem materialização de fato; exclui C6 viva que tenha porta para o colaborador. Divide violação/piso por via e pelo corte de `legado_sem_lastro`. Pode incluir carimbadas, mas não é um contador geral C6-viva. (`app/chamados/management/commands/auditar_invariantes_chamados.py:454`, `app/chamados/management/commands/auditar_invariantes_chamados.py:524`, `app/chamados/management/commands/auditar_invariantes_chamados.py:534`.) |
| Contraprovas `I6c` e pisos associados | Inspecionam o piso do I6 contra célula e movimento do dia, separando ausência de célula/vigência, exportação, isenção e ausência lavrada. Não aplicam simplesmente “carimbada e C6 viva”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:305`, `app/chamados/management/commands/auditar_invariantes_chamados.py:350`, `app/chamados/management/commands/auditar_invariantes_chamados.py:375`, `app/chamados/management/commands/auditar_invariantes_chamados.py:399`, `app/chamados/management/commands/auditar_invariantes_chamados.py:776`.) |

Há medidores vizinhos com outras perguntas: `I8` mede órfãs de destino NULL; `F2` do auditor exclui carimbadas; `respondida_sem_resposta` mede apenas o par texto/carimbo de resposta a partir de um marco temporal; `vigia_carimbo` parte de perguntas hora/data_hora **validadas**. Não substituem o contador do F0. (`app/chamados/management/commands/auditar_invariantes_chamados.py:853`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1760`, `app/chamados/services/vigia_fio.py:728`, `app/chamados/services/vigia_carimbo.py:97`.)

**3. Portas e escritores que podem alcançar o estoque**

Primeiro, “status da pergunta” não é uma coluna: `estado` é propriedade derivada de validação, C6, resposta e materialização. (`app/chamados/models.py:2467`.)

Para compactar os campos nas linhas seguintes:

- **Carimbo:** `materializada_em`, `materializada_por`, `via_resolucao`. (`app/chamados/models.py:2527`.)
- **Cache do julgamento:** `veredito_lavrado`, `julgado_em`, `janela_expira_em`. (`app/chamados/services/fio.py:845`.)

| Porta | Chamadores existentes | Escrita, trilha e guardas |
|---|---|---|
| `fio.reapontar_resolvedora` | Gate F1, reemissor/reatador, migração de resolvedoras, declaração textual e LIMBO. (`app/chamados/models.py:418`, `app/chamados/reconciliador.py:903`, `app/chamados/reconciliador.py:1539`, `app/chamados/services/fio.py:941`, `app/chamados/services/declaracao_texto.py:317`, `app/chamados/services/limbo_folgas.py:242`.) | Grava destino, data somente se faltava e cache. Consulta E4-c e a guarda F1 de costura, com destino relido/travado e C6 no pós-destino. Não escreve carimbo F2. A porta não cria evento próprio; gate/migração deixam `pergunta_reatada`/`resolvedoras_migradas`. (`app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:831`, `app/chamados/models.py:421`, `app/chamados/services/fio.py:947`.) |
| `PerguntaDisputa.abrir`, reutilizando identidade | Emissão e declaração textual. (`app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/declaracao_texto.py:322`.) | Também atualiza pergunta existente: célula, destino e cache. Usa a mesma guarda F1; recusa deixa `costura_recusada` idempotente. É uma exceção concreta à alegação documental de exclusividade de `reapontar_resolvedora`. (`app/chamados/models.py:2407`, `app/chamados/models.py:2423`, `app/chamados/models.py:2434`, `app/chamados/models.py:2447`.) |
| `reatar_pergunta_orfa` e `reemitir_chamado_do_dia` | Reconciliação por colaborador **e geração da pergunta**. A frase “HOJE SO O CRON” ficou desatualizada. (`app/chamados/reconciliador.py:1424`, `app/chamados/reconciliador.py:1436`, `app/chamados/services/disputa_emissao.py:404`.) | Derivam candidato, tentam histórico/c1/c2; costuram pela porta F1. c2 abre/deduplica chamado, exige retorno vivo e reaponta. Recusas de costura ficam no logger; não há evento próprio de reata nesses trechos. (`app/chamados/reconciliador.py:1454`, `app/chamados/reconciliador.py:1483`, `app/chamados/reconciliador.py:875`, `app/chamados/reconciliador.py:1539`.) |
| `lavrar_sem_turno_aberto` | Reatador. (`app/chamados/reconciliador.py:1517`.) | Só motivos c1, sem texto/carimbo/turno aberto e C6 morta. Carimba `sem_turno_aberto` pela F2 e tenta fechar disputa pela porta. (`app/chamados/reconciliador.py:332`, `app/chamados/reconciliador.py:362`, `app/chamados/reconciliador.py:405`.) |
| `retratar_perguntas_absolvidas_por_celula` | Reconciliação por colaborador e cartório. (`app/chamados/reconciliador.py:1348`, `app/ponto/services/cartorio.py:818`.) | Sem carimbo/validação, célula julgada que não acusa; inclui disputas fechadas. Pula C6 viva e dia trancado. Carimba `celula_absolveu` pela F2; fecha disputa aberta se C7 permite. Não cria evento específico nesse corpo; fechamento passa por `d.fechar`. (`app/chamados/reconciliador.py:581`, `app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:618`, `app/chamados/reconciliador.py:630`.) |
| S128 `resolver_perguntas_por_chamado_resolvido` | Signal de chamado, retratação, histórico único e serviço de becos. (`app/chamados/signals.py:75`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1545`, `app/chamados/services/becos.py:49`.) | Primeiro ramo atua no destino; segundo em disputa sem credor vivo. Pula viva pelo C6/tranca e usa F2, podendo preencher resposta/validação com texto e relavrar cache. Segundo ramo deixa `fecho_sem_credor`; pode reabrir pai com resposta pendente. (`app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1544`, `app/chamados/services/disputa_emissao.py:1565`, `app/chamados/services/disputa_emissao.py:1583`, `app/chamados/services/disputa_emissao.py:1607`.) |
| `desfazer_carimbo_sem_lastro` | Fábrica por motivo e julgamento da célula. (`app/chamados/services/fabrica_da_celula.py:121`, `app/chamados/services/fabrica_da_celula.py:148`.) | Relê linha travada, exige elegibilidade e C6 viva; limpa os três campos pela porta F2 `descarimbar`. Deixa `carimbo_desfeito_por_lastro` com antes/depois. Pode reabrir disputa; julgamento chama com `reabrir_fechada=False`. Não altera resposta/validação. (`app/chamados/services/relavra_via.py:159`, `app/chamados/services/relavra_via.py:204`, `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:229`, `app/chamados/services/relavra_via.py:243`.) |
| `relavrar` | Comando `relavrar_via_i6`; wrapper `relavrar_dia`. (`app/chamados/management/commands/relavrar_via_i6.py:102`, `app/chamados/services/relavra_via.py:140`.) | Só vias derivadas; muda `via_resolucao` e cache, com evento antes/depois. Sem testemunha nova não toca. Não escreve `materializada_em`, portanto não passa por `carimbar` F2. (`app/chamados/services/relavra_via.py:95`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:123`.) |
| `encerrar_fantasma` | Consumidor de evento da célula → `encerrar_orfas_pela_lampada`. (`app/chamados/reconciliador.py:1301`, `app/chamados/reconciliador.py:1093`.) | Exige universo específico, sem resposta, marco próprio aceso e dia encerrado; carimba `fantasma` pela F2. Evento `pergunta_fantasma_pela_lampada` somente se escreveu. (`app/chamados/reconciliador.py:993`, `app/chamados/reconciliador.py:1031`, `app/chamados/models.py:2645`, `app/chamados/reconciliador.py:1089`.) |
| `encerrar_por_fato_no_espelho` | Comando homônimo de encerramento. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`.) | Troca somente a via para `fato_materializado`; preserva carimbo, logo não chama `carimbar` F2. O comando deixa evento. (`app/chamados/models.py:2629`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:65`.) |
| `ChamadoColaborador.reabrir` / `ressuscitar_perguntas` | Cartório e desarquivamento; reabrir chama ressuscitação. (`app/ponto/services/cartorio.py:1452`, `app/chamados/views_arquivar.py:86`, `app/chamados/models.py:1380`.) | Reabre chamado pelo núcleo ou redireciona para irmão vivo, migrando resolvedoras. Ressuscitação seleciona materializadas sem texto, julga pós-estado, limpa carimbo pela F2 em lote e outros carimbos de resposta/validação; deixa `perguntas_ressuscitadas` e pode reabrir disputa. (`app/chamados/models.py:1359`, `app/chamados/models.py:1420`, `app/chamados/models.py:1483`.) |
| `ChamadoColaborador.encerrar_por_competencia_trancada` | Serviço `competencia_trancada.encerrar`. (`app/chamados/services/competencia_trancada.py:177`.) | Passa pelo gate e núcleo F1; grava estado, via, resolução temporal e urgência. Serviço deixa nota/evento. **Não carimba pergunta:** C7 mata por leitura; disputas fecham pela porta. (`app/chamados/models.py:1543`, `app/chamados/models.py:222`, `app/chamados/services/competencia_trancada.py:122`, `app/chamados/services/competencia_trancada.py:178`, `app/chamados/services/competencia_trancada.py:189`.) |

Também existem portas dependentes de resposta/decisão:

- **Responder/reabrir correção:** core web/API grava texto e `respondida_em`, desfaz validação anterior e carimbo elegível; deixa eventos de resposta/validação. Correção preserva histórico em `RespostaAnterior` e limpa resposta/validação, podendo descarimbar pela F2. (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/views.py:1283`, `app/api/views.py:2695`.)
- **Validar/materializar:** validação grava `validada_em/por` e chama a unidade de materialização. `_materializar_uma` planta dentro de transação; recusa F2 provoca rollback do plantio. Fechamento de disputa também chama `materializar_pendentes_aprovadas`, com carimbo F2. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:396`, `app/chamados/services/materializacao.py:1109`, `app/chamados/services/materializacao.py:1160`, `app/chamados/signals.py:125`, `app/chamados/services/materializacao.py:1307`.)
- **Decisão administrativa/identidade:** `fechar_por_admin` é chamado pelo veredito do dia e aceitação de folga; usa `carimbar_identidade`. Essa exceção F2 grava sem consultar C6 e admite continuar viva por resposta pendente. (`app/chamados/models.py:2484`, `app/ponto/services/veredito_celula.py:160`, `app/chamados/services/validacao.py:263`, `app/chamados/models.py:2588`.)

A antiga porta **da pergunta** `fechar_por_competencia_trancada` foi removida; F2 não aceita criar essa via. O acervo antigo continua sendo lido. (`app/chamados/models.py:2501`, `app/chamados/models.py:2546`.)

**4. Management commands existentes**

| Comando | Portas usadas e exclusões |
|---|---|
| `reabrir_orfas_enterradas --apply` | Fantasma carimbada, não validada, com data, marco False e C6 viva. Janela fixa de carimbo; exclui luz desconhecida, pai que recusa e reabertura que ressuscitaria irmãs. Exportadas vão à Pauta DP. Usa reabertura de fantasma/disputa/pai. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:35`, `:67`, `:92`, `:104`, `:145`.) |
| `encerrar_por_competencia_trancada --apply` | Usa serviço e portas de chamado/disputa. Exige tranca real; exclui sem/outra competência, módulos fora do universo, resgatados, perguntas vivas remanescentes e disputa com pai vivo fora. Não escreve perguntas. (`app/chamados/management/commands/encerrar_por_competencia_trancada.py:13`; `app/chamados/services/competencia_trancada.py:82`, `:122`, `:132`, `:174`.) |
| `encerrar_perguntas_com_fato --apply` | Só carimbadas não validadas, via vazia/NULL/`legado_sem_lastro` e marco próprio True. Usa `encerrar_por_fato_no_espelho`; fecha disputa somente sem fio vivo. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:23`, `:31`, `:65`, `:80`.) |
| `relavrar_via_i6 --apply` | Parte da violação I6; exclui vias não derivadas, IDs de `--excluir`, ausência de nova testemunha e via inalterada. Usa `relavrar`. (`app/chamados/management/commands/relavrar_via_i6.py:44`, `:58`, `:76`, `:102`.) |
| `dedup_perguntas_fato --apply` | Grupos duplicados por colaborador/motivo/dia, anteriores a `--ate`; exclui canônica e já duplicatas. Usa `retratar_duplicata`. (`app/chamados/management/commands/dedup_perguntas_fato.py:40`, `:71`, `:86`.) |
| `materializar_passivo_validado --apply` | Universo validado hora/data_hora; exclui `com_registro`, `plantio_fora_do_slot`, `slot_nao_derivavel` e recortes solicitados. Usa `passivo_validado.reparar` → `_materializar_uma`. (`app/chamados/management/commands/materializar_passivo_validado.py:49`; `app/chamados/services/passivo_validado.py:52`, `:324`, `:514`.) |
| `curar_perguntas_em_limbo --apply` | Conjunto fixo de IDs; recusa se conjunto atual divergir. Duas perguntas dependem de nota da resposta; demais passam por `reabrir_para_correcao`. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:21`, `:46`, `:55`, `:67`.) |
| `disparar_perguntas_competencia --apply` | Lote de emissão, que também pode desfazer carimbo pela fábrica. Seleciona células/marcos e chamados vivos sem pergunta viva; exclui desligados e fatos já servidos/decididos. Não percorre todo estoque carimbado: o primeiro universo deduplica perguntas existentes. Dry executa a unidade dentro de transação e força rollback. (`app/chamados/management/commands/disparar_perguntas_competencia.py:45`, `:76`, `:124`, `:155`, `:229`, `:276`, `:310`.) |

`reconciliar_chamados` já escreve, mas não oferece dry/apply: visita colaborador com chamado vivo ou órfã reatável. `reconciliar_perguntas_orfas` é vigia. Não foi identificado executor genérico F3 para todo o conjunto C6-viva/destino-terminal **[nao verificado]**. (`app/chamados/management/commands/reconciliar_chamados.py:22`, `app/ponto/management/commands/reconciliar_perguntas_orfas.py:1`.)

“Dry” não significa necessariamente ausência de escrita em arquivo: `materializar_passivo_validado` cria diretório/JSON de resultado; o auditor também pode escrever HTML. (`app/chamados/management/commands/materializar_passivo_validado.py:85`, `:114`; `app/chamados/managemen

[truncado em 20000 de 26646 caracteres]

## 01/11 18:54 -- HISTORICO (mais de 24 h) · sessao 0d1e007159c1 · - · - · origem=Ronald (tmux)

Censo Q3/Q6, somente código, sem executar comandos de aplicação.

**Premissa:** não há campo `status` em PerguntaDisputa; `estado` é property que deriva validação, C6, resposta e materialização (`app/chamados/models.py:2467-2482`). Portanto “mudar status da pergunta” significa mudar esses insumos, não escrever coluna de status (`app/chamados/models.py:2475-2482`).

| Porta / alcance | Chamadores no HEAD | Escrita / trilha / F1-F2 |
|---|---|---|
| `fio.reapontar_resolvedora(p,ch,data_evento)` | Gate F1 (`models.py:418`); reatador e reemissor (`reconciliador.py:903,1539,1551`); migração (`fio.py:941`); declaração textual (`declaracao_texto.py:317`); agrupamento LIMBO (`limbo_folgas.py:242`). Todos sob `app/chamados/`. | Escreve `chamado_destino`, opcionalmente `data_evento` somente se faltava, e `veredito_lavrado/julgado_em/janela_expira_em` (`app/chamados/services/fio.py:831-848`). Valida E4-c e F1 `_costura_nao_mata_o_fio`; esta trava destino e julga C6 com destino pós costura, barrando viva de competência aberta em terminal (`fio.py:758-780,838-844`). A porta não cria evento de auditoria próprio; migração e gate criam, conforme linhas seguintes (`fio.py:845-849,947-953`; `models.py:421-425`). Não escreve carimbo F2 (`fio.py:847`). |
| `PerguntaDisputa.abrir` ao reutilizar identidade existente | Emissor chama em `app/chamados/services/disputa_emissao.py:389-397`; declaração textual em `app/chamados/services/declaracao_texto.py:322-323`. | Além do nascimento, reaponta pergunta existente e atualiza `celula` (`app/chamados/models.py:2407-2427`). Mesma guarda F1; recusa deixa `costura_recusada` idempotente; relavra S2 no mesmo save (`models.py:2423-2450`). Exceção necessária à alegação de escritor único em `fio`: existe esta outra porta de identidade (`models.py:2374-2381,2426`). |
| `reatar_pergunta_orfa` / reemissão c2 | Cron/por batida via `reconciliar_chamados_colab` (`app/chamados/reconciliador.py:1348-1349,1424`), **e nascimento** (`app/chamados/services/disputa_emissao.py:404-409`): docstring “HOJE SO O CRON” em `reconciliador.py:1436` está desatualizada. | Deriva candidato; se não há, tenta histórico único ou c1/c2 (`reconciliador.py:1454-1457,1483-1521`). C6 viva em competência aberta sai do histórico terminal (`1491-1493`). Costura pela porta F1 (`1539-1554`). c2 usa `ChamadoColaborador.abrir`, exige retorno vivo, e reaponta (`reconciliador.py:875-905`). Não cria evento próprio de reata; recusas de costura são logadas (`1540-1542,1552-1554`). |
| `lavrar_sem_turno_aberto` c1 | Reatador (`app/chamados/reconciliador.py:1517`). | Só `MOTIVOS_C1=('orfao_14h','saida_sem_entrada')`, sem texto, sem carimbo, sem turno aberto do dia e C6 morta (`reconciliador.py:332,362-399`). Carimba `sem_turno_aberto` pela F2 e fecha disputa pela porta se `pode_fechar()` (`405-413`). |
| `retratar_perguntas_absolvidas_por_celula` | `reconciliar_chamados_colab` (`app/chamados/reconciliador.py:1348`), chamado por signal de batida (`app/ponto/signals.py:215`) e management cron (`app/chamados/management/commands/reconciliar_chamados.py:52`). | Universo: sem materialização/validação, célula julgada que não acusa; inclui disputa fechada (`reconciliador.py:581-596`; ausência de filtro de disputa em `571-573`). Pula C6 viva e dia trancado; carimba `celula_absolveu` pela F2; fecha disputa aberta quitada (`608-636`). Não há evento de auditoria específico nesta função; fechamento fica a cargo de `d.fechar()` (`618-636`). |
| `resolver_perguntas_por_chamado_resolvido` S128 | Post-save de chamado encerrado (`app/chamados/signals.py:75-94`); retratação (`reconciliador.py:536`); histórico único (`1545`); becos (`services/becos.py:49`); comando fantasmas (`app/ponto/management/commands/reconciliar_fantasmas.py:280`). | Primeiro ramo: destino igual ao chamado, disputa aberta, sem carimbo; pula C6 viva, tranca e resposta sem testemunha; F2 `carimbar(...resposta='validar',lavrar_veredito=True)` (`app/chamados/services/disputa_emissao.py:1532-1568`). Pode reabrir pai com resposta pendente (`1583-1593`). Segundo ramo: disputas abertas sem pai/destino vivo, carimba só C6 morta sem materialização/validação; deixa `fecho_sem_credor` (`1607-1645`). O chamado já passa pela F1 em sua própria porta; S128 não fecha viva por destino terminal (`1544-1549`). |
| `desfazer_carimbo_sem_lastro` | Fábrica por motivo (`app/chamados/services/fabrica_da_celula.py:121-123`) e julgamento da célula (`148`). | Relê pergunta travada, exige carimbo não identidade e C6 viva; limpa **três** campos via F2 `descarimbar`; evento `ACAO_DESFAZER` com antes/depois; opcionalmente reabre disputa pela porta (`app/chamados/services/relavra_via.py:217-245`). Julgamento chama com `reabrir_fechada=False`: exclui disputa fechada (`fabrica_da_celula.py:141-148`; `relavra_via.py:223`). Não relavra cache S2: assimetria declarada, leitores guardam carimbo (`relavra_via.py:40-49`). |
| `relavrar` | Chamador direto encontrado: comando `relavrar_via_i6` (`app/chamados/management/commands/relavrar_via_i6.py:102`); `relavrar_dia` é wrapper (`app/chamados/services/relavra_via.py:140-155`). Outro chamador de produção de `relavrar_dia`: `[nao verificado]`. | Só vias derivadas `declaracao/completude_turno`; sem destino e sem declaração pode devolver None e não tocar (`relavra_via.py:77,95-101,116-120`). Troca `via_resolucao` e relavra S2; evento `relavra_via_i6` por pergunta (`123-136`). Não escreve `materializada_em`, portanto não passa por `carimbar` F2 (`125-126`). |
| `encerrar_fantasma` | Consumidor de EventoCelula chama `encerrar_orfas_pela_lampada` (`app/chamados/reconciliador.py:1301`), que chama porta (`1093`). | Universo aberto, sem materialização/validação, dia presente, motivos orfão/saída (`993-1000`). Exige sem resposta, próprio marco aceso, C6 viva e dia encerrado; hoje/teto adia (`1031-1045`). F2 `carimbar(fantasma)` (`app/chamados/models.py:2645-2648`); evento `pergunta_fantasma_pela_lampada` só se escreveu (`reconciliador.py:1089-1101`). |
| `encerrar_por_fato_no_espelho` | Comando `encerrar_perguntas_com_fato` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`). | Só muda via para `fato_materializado`, idempotente; não muda carimbo e não passa por `carimbar` F2 (`app/chamados/models.py:2629-2638`). |
| `ChamadoColaborador.reabrir` / `ressuscitar_perguntas` | Reabrir chama ressuscitação (`app/chamados/models.py:1380-1389`); cartório (`app/ponto/services/cartorio.py:1452`); view arquivar (`app/chamados/views_arquivar.py:86`). | Reabrir pode redirecionar a irmão vivo e migrar resolvedoras/duplicatas com `resolvedoras_migradas` e `reabertura_redirecionada` (`models.py:1359-1373`; `fio.py:927-953`). Ressuscitação só materializadas sem texto, pula marco aceso e C6 morta no pós estado; limpa materialização/autor/via, validação/autor/resposta por F2 lote; `perguntas_ressuscitadas`; reabre disputa fechada (`models.py:1420-1442,1483-1516`). |
| `encerrar_por_competencia_trancada` | Serviço `competencia_trancada.encerrar` (`app/chamados/services/competencia_trancada.py:177`). | **Não carimba pergunta:** C7 a mata por leitura; `perguntas` de ação vazio por construção (`competencia_trancada.py:122-131,171-173`). Fecha chamado via F1 e núcleo, deixando estado/via/urgência/data; nota e evento com antes/depois (`app/chamados/models.py:1543-1553,222-246,275-279`; serviço `178-187`). Disputa fecha pela porta se juiz permite (`competencia_trancada.py:189-198`). Método antigo **da pergunta** `fechar_por_competencia_trancada` foi removido, zero chamador de produção declarado; F2 não aceita criar essa via (`models.py:2501-2506,2546-2547`). |

**Outras portas humanas que impedem dizer “nenhuma porta” em sentido absoluto:**

- Responder pelo core (web/API/declarador): grava texto/`respondida_em`, desfaz validação anterior, descarimba vias sem lastro, deixa `resp_pergunta_gravada` e `validacao_desfeita_por_resposta`, e tenta ressurreição por resposta (`app/chamados/services/disputa_emissao.py:1247-1291`; chamadores `app/chamados/views.py:1283`, `app/api/views.py:2695`, `app/chamados/services/declaracao_texto.py:365`). `reabrir_por_resposta` só descarimba `VIAS_SEM_LASTRO` (`app/chamados/models.py:2674-2676`).
- Validar: escritor `validada_em/por` (`app/chamados/services/validacao.py:127-133`), chama materializador, desfaz validação de hora recusada antes de eventual fechamento (`396-430`). Materializador seleciona respondida+validada sem materialização (`app/chamados/services/materializacao.py:1258-1266`); `_materializar_uma` planta dentro de atomic e F2 recusada levanta rollback da batida (`1109-1115,1160-1173`).
- Admin decide dia: `fechar_por_admin` carimba identidade (`app/chamados/models.py:2484-2498`), chamado pelo veredito/declarar dia (`app/ponto/services/veredito_celula.py:160-186`), aceitação de folga (`app/chamados/services/validacao.py:263-271,290-299`) e fechamento por veredito (`app/chamados/models.py:1839`). Identidade usa exceção F2: pode continuar viva por resposta pendente de validação, porque esse degrau precede a via (`models.py:2588-2595`).
- Reabrir correção limpa texto/resposta/validação; preserva versão em `RespostaAnterior`; se desfaz absorção também descarimba via F2 lote (`app/chamados/services/disputa_emissao.py:1358-1378`). Rejeição de ausência chama essa operação e reabre disputa (`app/chamados/services/materializacao.py:622-625`).
- `materializar_pendentes_aprovadas`, gatilhada por fechamento da disputa (`app/chamados/signals.py:125-140`), exige aprovação pelo juiz do lastro e carimba via `juiz_aprovou` com F2 (`app/chamados/services/materializacao.py:1307-1314`).

**Q6: contraexemplos verificáveis de alcance, sem inventar linha de produção:**

1. Pergunta viva com **destino terminal não NULL** fica fora do reatador automático de órfãs, porque exige NULL; se materializada também fica fora pelo carimbo (`app/chamados/reconciliador.py:1401-1406`). O gate F1 só faz plano na travessia vivo→terminal, logo não revisita espontaneamente terminal antigo (`app/chamados/models.py:398-400`). Isso demonstra falta de varredura genérica nessa porta, **não ausência de porta parametrizada**, pois `reapontar_resolvedora` aceita pergunta existente (`app/chamados/services/fio.py:784-849`).
2. Carimbada por **identidade**, ainda C6 viva por resposta não validada: é estado explicitamente permitido; desfazimento de carimbo exclui identidade (`app/chamados/models.py:2591-2595`; `app/chamados/services/relavra_via.py:179`). Limpar essa identidade automaticamente por ser viva não é regra de `desfazer_carimbo_sem_lastro`; há validação humana existente (`app/chamados/services/validacao.py:359-396`).
3. Reemissão c2 não cobre motivos fora do grupo DIA, dia ausente, desligado, vetos de célula/ausência/isenção/piso/protesto/régua; cada exclusão está no corpo e no contrato (`app/chamados/reconciliador.py:764-788,801-855`). Ligar automaticamente esses casos à emissão exigiria alterar o escopo da porta; ocorrência no estoque fornecido `[nao verificado]`.
4. Marco aceso com turno aberto sem escala/sem previsto não satisfaz encerramento por fantasma; segue viva e volta ao julgamento em evento seguinte, explicitamente (`app/chamados/reconciliador.py:1046-1052`). Transformar essa falta de prova em encerramento não é regra existente (`1048-1051`).
5. Pergunta sem dia / ambiguidade sem candidato único retorna False no reatador (`app/chamados/reconciliador.py:1465-1468,1521-1522`); escolha de destino/dia sem esses insumos não é feita por essa porta (`app/chamados/services/disputa_emissao.py:219-237`).

O dado recebido não lista linhas/pks nem atributos do estoque; portanto nenhum contraexemplo acima pode ser atribuído a uma linha real ou ter quantidade confirmada `[nao verificado]`.

## 01/11 18:54 -- HISTORICO (mais de 24 h) · sessao d3a74d0e3ea4 · - · - · origem=Ronald (tmux)

Para Q2, o contador F0 existe: `materializada_viva()['aberta']` é literalmente o tamanho do balde `aberta` devolvido por `carimbos_sem_lastro_por_dono`, não um contador geral de toda pergunta carimbada e viva (`app/chamados/services/vigia_fio.py:800-804`).

| Medidor | Universo e exclusões |
|---|---|
| `materializada_viva` / `carimbos_sem_lastro_por_dono` | Carimbo presente, disputa aberta e via fora de `VIAS_DE_IDENTIDADE`; depois exige C6 `pergunta_viva`. Particiona aberta/exportada pela competência do dia; sem data conta em aberta. Exclui disputa fechada e vias de identidade (`app/chamados/services/vigia_fio.py:743-772`; `app/chamados/services/relavra_via.py:166-182`). |
| `censo_alcance` → `fio_morto_alcance`, causa `c` e detalhe `pergunta_viva_sob_chamado_terminal` | Conta **dias retidos sem porta**, não perguntas de todo acervo; universo de empresas visíveis e competências enumeradas por `competencias_a_lavrar`, alimentado por `fora_da_folha`. Retira hoje/futuro e baldes de dono fora do fio antes de classificar causas; causa c requer pergunta respondida sob âncora terminal, detalhe e cobre outras vivas sob terminal (`app/chamados/services/vigia_fio.py:534-552`, `:604-654`). |
| `viva_em_disputa_fechada` | Pré-filtro `q_vivas`, disputa fechada, C6 viva; conta perguntas e disputas; sem filtro de destino terminal nem de carimbo (`app/chamados/services/vigia_fio.py:692-713`). |
| Auditor `I2` / `I2p` | Pergunta **não carimbada** com destino terminal; exclui do I2 respondida em disputa aberta, publicada em I2p. Conta chamados, não todas as perguntas; não consulta C6 (`app/chamados/management/commands/auditar_invariantes_chamados.py:586-598`). |
| Auditor `I8` | C6 viva, destino NULL, motivo atendível, colaborador ativo; retira piso de 2+ candidatos sem entrada no contexto (`app/chamados/management/commands/auditar_invariantes_chamados.py:853-865`, `:82-95`). |
| Auditor `I8b`, `I8bc`, `I8bcx`, `I8bx` | C6 viva com destino e data, cuja `resolvedora_valida` é falsa. Carimbadas vão I8bc/I8bcx; não carimbadas sem célula exportadas vão I8bx; restante I8b. I8bc não é o contador geral de carimbada viva (`app/chamados/management/commands/auditar_invariantes_chamados.py:890-947`). |
| Auditor `I21` / `I21a` | Respondida, não validada, não carimbada, texto diferente de `''`, destino **e pai** fora de VIVOS; retira ausência aprovada em I21a. Não consulta C6 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1568-1582`, `:156-163`). |
| Auditor `I22` | Chamado vivo com pelo menos uma resolvedora, todas inválidas; exclui cinco IDs históricos explícitos. Não mede estoque viva-terminal (`app/chamados/management/commands/auditar_invariantes_chamados.py:1040-1061`). |
| Auditor `F2` | C6 viva, disputa fechada, carimbo NULL, validação NULL e colaborador ativo; exclui carimbadas (`app/chamados/management/commands/auditar_invariantes_chamados.py:1760-1765`). |
| Auditor `S6` | Amostra aleatória de até 300 carimbadas; compara veredito lavrado com `_pergunta_materializada_de_fato`, excluindo stale e exceções. Não consulta vitalidade C6 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1153-1173`). |
| `vigia_carimbo.censo` | Perguntas de hora/data_hora validadas; classifica por materialização de fato e slot, não por C6 nem destino terminal (`app/chamados/services/vigia_carimbo.py:97-124`). |

`vigiar_fio_morto` publica `materializada_viva` aberta com esperado zero e exportada separada, sem alarme; escreve carimbo apenas no modo frota (`app/chamados/management/commands/vigiar_fio_morto.py:189-191`, `:205-224`). A cerca correta aberta/entregue é `competencia_do_dia_entregue`: janela da empresa + `folha.export.competencia_entregue`; sem dia retorna None (`app/chamados/services/vigia_fio.py:775-797`).

Para Q4, existem comandos específicos de estoque; não foi identificado um executor genérico que percorra todo o estoque C6-viva/destino-terminal e aplique reata/descarimbo [nao verificado].

| Comando | Portas e exclusões |
|---|---|
| `reabrir_orfas_enterradas --apply` | Usa `reabrir_fantasma`, `reabrir_sistema` e `pai.reabrir`; só fantasma carimbada, não validada, com data, lâmpada própria False e C6 viva. Exclui luz None, carimbo fora da janela fixa, pai que recusa ou ressuscitaria irmãs; exportada recebe Pauta DP (`app/chamados/management/commands/reabrir_orfas_enterradas.py:35-37`, `:67-118`, `:145-167`). |
| `encerrar_por_competencia_trancada --apply` | Delega a `competencia_trancada.encerrar`; usa `ch.encerrar_por_competencia_trancada()` e `d.fechar()`. Exige tranca real; não escreve perguntas desde C7. Exclui chamados sem competência, doutra competência, módulos fora da tranca, resgatados, perguntas vivas fora e disputa com pai vivo fora (`app/chamados/management/commands/encerrar_por_competencia_trancada.py:13-29`; `app/chamados/services/competencia_trancada.py:82-105`, `:122-151`, `:174-198`). |
| `encerrar_perguntas_com_fato --apply` | Usa `encerrar_por_fato_no_espelho` e depois `DisputaSupervisao.fechar`; só carimbadas não validadas, via vazia/NULL/legado_sem_lastro e marco próprio True. Preserva demais vias; fecha disputa somente sem fio vivo (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:23-43`, `:65-94`). |
| `dedup_perguntas_fato --apply` | Usa `fio.retratar_duplicata`; só grupos duplicados colab/motivo/dia, anteriores a `--ate` (default 24/08); exclui canônica e já duplicatas. Recortes opcionais colab/limite (`app/chamados/management/commands/dedup_perguntas_fato.py:40-61`, `:71-92`). |
| `materializar_passivo_validado --apply` | Usa `passivo_validado.reparar` → `_materializar_uma`; universo hora/data_hora validada do `vigia_carimbo`. Exclui `com_registro`, `plantio_fora_do_slot`, `slot_nao_derivavel` e recortes de colaborador/competência; demais recusas continuam nas guardas da unidade (`app/chamados/management/commands/materializar_passivo_validado.py:49-83`, `:110-111`; `app/chamados/services/passivo_validado.py:52-60`, `:324-350`, `:514-524`; `app/chamados/services/vigia_carimbo.py:97-116`). |
| `curar_perguntas_em_limbo --apply` | Conjunto histórico fixo de 21 IDs; recusa se conjunto atual diverge. Duas usam `carimbar_resposta_pela_trilha` somente com nota; demais `reabrir_para_correcao` (`app/chamados/management/commands/curar_perguntas_em_limbo.py:21-23`, `:46-69`). |

`reconciliar_chamados` já escreve, mas **não oferece dry/apply**: visita colaborador com chamado vivo ou pergunta órfã reatável, chama `reconciliar_chamados_colab`, rejunta célula e drena eventos (`app/chamados/management/commands/reconciliar_chamados.py:22-64`). `reconciliar_perguntas_orfas` é vigia sem escrita, com `--alarme` (`app/ponto/management/commands/reconciliar_perguntas_orfas.py:1-15`, `:24-36`).

`auditar_invariantes_chamados` não tem `--corrigir`/`--apply`; seus argumentos são `--html` e `--amostra`; `--html` grava arquivo (`app/chamados/management/commands/auditar_invariantes_chamados.py:545-550`, `:1838-1849`). Mesmo o dry de `materializar_passivo_validado` cria diretório e JSON de resultado, portanto não é execução compatível com a restrição desta tarefa (`app/chamados/management/commands/materializar_passivo_validado.py:85`, `:114-127`).

## 01/11 18:54 -- HISTORICO (mais de 24 h) · sessao 86f8555eafdd · - · - · origem=Ronald (tmux)

Q5 — leitores e efeito sobre folha/export, somente código:

- **“Status da pergunta” é `PerguntaDisputa.estado`, uma `@property`:** traduz `validada_em`, resultado de `pergunta_viva`, `respondida_em` e `materializada_em` em `VALIDADA/RESOLVIDA/PENDENTE/RESPONDIDA`; alterar apenas esse rótulo não é uma escrita de coluna de estado. A property `viva` delega ao C6; `q_vivas` é pré-filtro generoso, não resposta exata. (`app/chamados/models.py:2467-2482`, `2727-2738`.)

| Leitor | Campos/efeito |
|---|---|
| `pergunta_viva` / `_pergunta_viva_calc` | `validada_em`, tranca, resposta, via, carimbo e célula determinam vida; carimbo não basta para declarar morta. Destino terminal participa da testemunha de materialização. (`app/chamados/juizes.py:1032-1118`, `720-721`.) |
| `chamado_que_lista` | Ancora no `chamado_destino_id`; sem destino usa `disputa.chamado_id`. Reapontar muda o card ao qual pergunta é atribuída. (`app/chamados/services/fio.py:97-117`.) |
| `perguntas_do_app` e API | Seleção `q_vivas` + C6; API separa `p.estado == RESPONDIDA`, expõe `estado`, `aguarda_admin` e `chamado_destino_id`. (`app/chamados/services/pergunta_no_app.py:39-44`; `app/api/views.py:2603-2634`.) |
| `montar_chamados_fio` | Admin carrega perguntas vivas de disputas fechadas; ramo `so_janela` não acrescenta essas disputas; agrupa pela âncora canônica. (`app/chamados/services/fio.py:209-240`.) |
| `raiox.creditos_perguntas` | Credita destino vivo; senão, pai da disputa. Filtra `q_vivas`, exclui carimbadas com `veredito_lavrado=VL_SIM`, separa respondidas/não respondidas. O cache pode estar velho até ciclo do cron. (`app/chamados/services/raiox.py:49-72`, `110-149`.) |
| `qs_na_fila` | Lê `status_local` via `qs_vivos`, restringe a `COBRANCA` e exclui silêncio vigente; terminal não entra nessa fila. (`app/chamados/services/painel.py:153-155`, `158-188`.) |
| `alimentar_portas` / `porta_alcancavel_do_dia` | Cruza fila, verbos, C6, perguntas do app e âncora. Porta pela pergunta exige viva, no app, resposta não vazia quando respondida e âncora na fila. Logo reapontamento/status do chamado podem alterar existência da porta e fio/grupo retornados. (`app/chamados/juizes.py:1160-1196`, `1226-1272`.) |
| Cartório | Impressão inclui `pk:status_local` e quantidade de chamados vivos com fio. Julgamento deriva `_com_fio` por C6 e o passa ao suprajuiz; pode lavrar `cobrado`. Mudança real depende do novo julgamento. (`app/ponto/services/cartorio.py:356-410`, `1167-1186`, `1243-1246`, `1281-1282`.) |
| Signal do chamado | Criação/mudança de `status_local` chama `cartorio.julgar_celula` pelo dia forte do fato; save sem mudar status retorna. (`app/chamados/signals.py:155-180`.) |
| `furos_sem_cobranca` | Lê status do chamado e de `superado_por`; classifica vivo/coberto/decidido e exclui dia trancado. (`app/ponto/services/furos_sem_cobranca.py:66-86`.) |
| `competencia_do_chamado`, fechamento da disputa | Competência considera dias das perguntas que C6 diz vivas; `disputa_viva_do_chamado` lê existência de perguntas e `fio_vivo`. (`app/chamados/juizes.py:1909-1941`, `1955-1968`.) |

**Caminho de dinheiro:** `classificar_export` chama `motivos_retencao_espelho`; se há motivos marca pessoa `fora/furo_espelho`. O wrapper delega a `motivos_retencao_celula`. Esse retentor consulta célula por dia e lê `veredito`, `protestos`, `trabalha`, cobertura cadastral e isenções; não decide diretamente pelo carimbo, destino ou estado da pergunta. (`app/folha/export.py:942-947`, `1243-1249`, `1174-1238`.)

Assim, mudar âncora/carimbo pode mudar **fio, grupo e ação indicada** sem mudar quem entra; pode mudar quem entra quando os efeitos chegarem à célula e alterarem os motivos de retenção. O cartório lê vida/estado dos chamados e fio para julgar, e o export lê a célula resultante; o efeito efetivo neste estoque é `[nao verificado]`. (`app/chamados/juizes.py:1261-1272`; `app/ponto/services/cartorio.py:1167-1186`, `1243-1246`, `1281-1282`; `app/folha/export.py:1215-1238`.)

`fora_da_folha` toma **quem entra** de `classificar_export`; usa `motivos_retencao_celula(com_dias=True)` para dias retidos e `porta_alcancavel_do_dia` para o fio/grupo. `o_que_segura` usa as mesmas autoridades. Não existe juízo próprio de retenção por pergunta nessas ferramentas. (`app/folha/fora_da_folha.py:55-74`, `91-95`, `120-155`, `180-208`.)

A montagem do TXT consome `classificar_export`, constrói `entra_set`, restringe fechamento aos estados `aberto/aprovado` e só monta linhas dos IDs admitidos. Portanto comparar só valores não prova preservação de pertencimento ao TXT. (`app/folha/export.py:668`, `695-708`.)

**Aberta × entregue:** `competencia_do_dia_entregue` compõe `janela_atual(dia, empresa)` com `competencia_entregue`; sem dia retorna `None`. Entregue significa TXT vigente não invalidado, não apenas existência histórica de exportação. (`app/chamados/services/vigia_fio.py:775-797`; `app/folha/export.py:545-575`.)

- Na **aberta**, o alcance novo C pode operar; a cerca também exclui sem data e trancada. (`app/chamados/reconciliador.py:683-716`.)
- Na **entregue**, a cerca preserva o caminho anterior e deixa fora apenas o alcance novo C; o texto explicita O44 sem fronteira. Isso não é proibição global dos leitores ou de todas as portas. (`app/chamados/reconciliador.py:686-699`.)
- `classificar_export`/retentor continuam lendo estado atual; o juiz `txt_vigente_x_gravado` monta o atual e compara seu hash ao TXT vigente. Logo “entregue” não garante que a leitura atual permaneça igual ao documento entregue. Divergência real neste estoque é `[nao verificado]`. (`app/folha/export.py:635-660`, `746-769`, `942-955`.)
- Não confundir com `competencia_fechada_para_celula`: outro juiz combina TXT entregue **ou holerite publicado**. (`app/folha/export.py:578-609`.)

## 01/11 18:26 -- HISTORICO (mais de 24 h) · sessao 2e2239d89df6 · O261-E-F2 · verificador · origem=Code

1. **Tratamento dos itens da triagem_v8**

Nas referências abaixo, **T** é [test_o261e_f2_ramo_e_i20.py](/home/ronald/codex_ws/5c2bb8a4370998765c54bbb1926895b27a5ec1bd/app/chamados/tests/test_o261e_f2_ramo_e_i20.py) e **A** é [auditar_invariantes_chamados.py](/home/ronald/codex_ws/5c2bb8a4370998765c54bbb1926895b27a5ec1bd/app/chamados/management/commands/auditar_invariantes_chamados.py); os demais caminhos são relativos a `app/`.

- **Item 1 — seis procedentes da v7:** a resolução individual dos seis é **[nao verificado]**: o anexo não enumera os seis achados. O código atual permite confirmar restauração do juiz, leitura após o mock, comparação exata da amostra I20 e preservação da célula sem reescrita posterior. (`T:216`, `T:221`, `T:245`, `T:168`, `T:170`.)

- **Item 2a — procedente:** o novo cenário distingue as guardas. Sem resposta, a pergunta pode estar viva antes do carimbo e morta depois dele pelo BUG 117; com texto, a porta preenche `respondida_em` e o degrau da resposta decide antes desse BUG. (`chamados/juizes.py:1047`, `chamados/juizes.py:1077`, `chamados/juizes.py:1083`, `chamados/models.py:2556`.) A execução histórica de `mordida_v9_F.log` é **[nao verificado]** nesta revisão.

- **Item 2b — sustentado:** o meio-estado é uma entrada adversarial explícita: o teste escreve somente `resposta_colab`; o escritor normal grava texto e `respondida_em` juntos. (`T:156`, `T:158`, `chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1248`.)

- **Item 3 — sustentado com limite de proveniência:** resposta, veredito e lâmpadas vêm da fixture; `_fechar` relê e verifica preservação de veredito, via e lâmpadas. Não verifica preservação de todos os campos da célula ou de toda a ata. (`T:158`, `T:160`, `T:169`, `T:170`.) O cálculo dessas lâmpadas pelo cartório real é **[nao verificado]**.

- **Item 4a — correção parcial:** retirar o incremento do contador por causa viola a soma; retirar o detalhe do ramo viola a asserção específica. Entretanto, a distribuição entre `alcance_a..e` continua sem verificação individual — há mutante sobrevivente descrito no item 4 abaixo. (`chamados/services/vigia_fio.py:651`, `chamados/services/vigia_fio.py:653`, `T:196`, `T:197`.)

- **Item 4b — descarte sustentado para estes cinco testes:** uma restauração em memória inoperante não altera o banco: a porta retorna `False` antes do `save`, o fecho executa `continue` e os leitores seguintes recarregam os dados. (`chamados/models.py:2569`, `chamados/models.py:2576`, `chamados/services/disputa_emissao.py:1638`, `chamados/services/disputa_emissao.py:1655`, `T:183`.)

- **Item 5 — ressalva:** o código contém proteção explícita de relógio e mock, mas a afirmação incondicional de ausência de dependência de ordem deve considerar a possível colisão do PK gerado com a cicatriz fixa. (`T:109`, `T:213`, `T:216`, `T:265`, `A:1098`.)

2. **Caminho exato do C6 no teste da guarda sozinha** [nao verificado]

**Antes do fecho:**

- A pergunta usa `saida_sem_entrada`, resposta vazia, DNA `hi=08:00` e lâmpada E das 08:00 acesa. Esse motivo aponta para E/hi. (`T:236`, `T:147`, `T:61`, `chamados/juizes.py:845`.)
- O degrau da resposta não entra; o BUG 117 também não entra, pois `materializada_em` está nulo. O veredito `furo` então determina **viva**. (`chamados/juizes.py:1047`, `chamados/juizes.py:1077`, `chamados/juizes.py:1083`, `escala/models.py:1735`.)
- A guarda do fecho recebe esse resultado e pula a pergunta. (`chamados/services/disputa_emissao.py:1626`.)

**Retirando somente essa guarda:**

- `carimbar` aplica o carimbo em memória; como o texto está vazio, não preenche `respondida_em`. (`chamados/models.py:2555`, `chamados/models.py:2556`.)
- O C6 é chamado novamente. Agora há carimbo e o fato E/hi está provado pela lâmpada: o BUG 117 retorna **morta antes de chegar ao veredito `furo`**, permitindo o `save`. (`chamados/models.py:2565`, `chamados/juizes.py:887`, `chamados/juizes.py:893`, `chamados/juizes.py:1077`, `chamados/models.py:2576`.)

**O docstring corresponde ao código.** Com texto, a porta preencheria `respondida_em`; o degrau da resposta viria primeiro, e o dia acusador impediria a concordância, mantendo a pergunta viva e provocando recusa. (`T:229`, `chamados/models.py:2558`, `chamados/juizes.py:1059`, `chamados/juizes.py:1029`, `chamados/models.py:2569`.)

3. **Quem escreve e quem lê os campos assertados**

| Campos/resultado | Quem escreve | Quem lê/asserta |
|---|---|---|
| `veredito`, `veredito_via`, `ata.lampadas` | O teste fornece os valores à porta; a porta persiste a célula. (`T:160`, `ponto/portas/celula.py:768`, `ponto/portas/celula.py:770`.) | `_fechar` relê o banco e compara os três; C6 lê veredito e lâmpadas. (`T:169`, `T:170`, `chamados/juizes.py:1081`, `chamados/juizes.py:887`.) |
| `resposta_colab`, `respondida_em` | Fixture injeta o texto; controles escrevem ambos explicitamente. A porta pode preencher `respondida_em` durante o carimbo e restaurá-lo na recusa. (`T:158`, `T:255`, `chamados/models.py:2558`, `chamados/models.py:2583`.) | C6, I20 e a asserção de preservação do caso 2. (`chamados/juizes.py:1047`, `A:1102`, `T:264`.) |
| `materializada_em`, `via_resolucao` | A pergunta nasce sem carimbo e com via vazia; a porta escreve os campos e desfaz na recusa. (`chamados/models.py:2298`, `chamados/models.py:2529`, `chamados/models.py:2531`, `chamados/models.py:2582`.) | Os três testes do ramo verificam ausência/presença do carimbo e vivacidade; o caso 2 verifica carimbo nulo. (`T:184`, `T:185`, `T:186`, `T:218`, `T:239`, `T:264`.) |
| `pergunta_viva(...)` | C6 calcula; no MORDE o teste substitui deliberadamente sua resposta durante o fecho. (`chamados/juizes.py:816`, `T:213`.) | Premissas e selo usam o juiz real; o MORDE verifica sua restauração antes das testemunhas. (`T:202`, `T:237`, `T:260`, `T:216`, `T:220`.) |
| `alcance_universo`, `fio_morto_alcance` | Censo calcula o universo a partir de `fora_da_folha` e incrementa os dias sem porta. (`chamados/services/vigia_fio.py:550`, `chamados/services/vigia_fio.py:553`, `chamados/services/vigia_fio.py:650`.) | Selo do ramo e caso regularizado. (`T:194`, `T:268`.) |
| `alcance_dias`, incluindo célula, rota, causa e detalhe | `_listar` monta a linha produzida pelo censo. (`chamados/services/vigia_fio.py:589`, `chamados/services/vigia_fio.py:654`.) | Teste filtra pelo `cel.pk`, confere uma linha `sem_porta/e`, confere `sem_porta/c` no MORDE e ausência da linha no caso regularizado. (`T:176`, `T:191`, `T:192`, `T:224`, `T:267`.) |
| `alcance_a..e` | Censo inicializa e incrementa o contador da causa. (`chamados/services/vigia_fio.py:543`, `chamados/services/vigia_fio.py:651`.) | Teste lê somente a **soma** dos cinco. (`T:196`.) |
| `alcance_e_detalhe` | Censo incrementa a chave correspondente ao detalhe. (`chamados/services/vigia_fio.py:653`.) | Teste exige somente a chave do ramo igual a 1. (`T:197`.) |
| I20: `flag`, `total`, `amostra` | Auditor consulta os campos, calcula contagem/amostra e imprime a classificação. (`A:1102`, `A:564`, `A:565`, `A:1834`, `A:1835`.) | Teste captura, interpreta e compara a tupla exata nos dois testes I20. (`T:95`, `T:80`, `T:245`, `T:293`, `T:297`.) | [nao verificado]
| Exclusão por PK e unicidade da âncora | A cicatriz vem do auditor; o teste lê seu AST e acrescenta o PK somente à cópia mutante. (`A:1098`, `T:73`, `T:282`, `T:283`.) | Caso real exige PK fora da cicatriz; MORDE compara cópia e comando real. (`T:265`, `T:293`, `T:297`.) | [nao verificado]

**Há valores escritos pelo teste consumidos pelos leitores reais:** texto, veredito e lâmpadas. Sua origem está declarada; a lavra pela porta não demonstra que o cartório calculou esses valores a partir de batidas. (`T:156`, `T:158`, `T:160`.) Os resultados `alcance_*` e a saída I20 são obtidos das funções reais, sem preenchimento desses resultados pelo teste. (`T:175`, `T:270`, `T:297`.) [nao verificado]

4. **Mutantes de produção que ficam vivos**

**Sim, por análise estática; sobrevivência executada nesta revisão: [nao verificado].**

- **Contador na causa errada:** substituir, em `chamados/services/vigia_fio.py:651`, `d['alcance_' + causa] += 1` por `d['alcance_a'] += 1`. A soma permanece correta; linha e detalhe continuam usando a causa original. O teste não exige os valores individuais dos contadores. (`chamados/services/vigia_fio.py:653`, `chamados/services/vigia_fio.py:654`, `T:192`, `T:196`, `T:197`.) No MORDE, o helper já falha pela linha `c`, antes da soma. (`T:192`, `T:224`, `T:226`.)

- **Restauração em memória inoperante:** tornar `_desfazer_em_memoria` um no-op. A porta continua recusando sem salvar, e o fecho abandona a instância; estes testes observam dados recarregados. É precisamente a lacuna aceita na triagem 4b. (`chamados/models.py:2579`, `chamados/models.py:2569`, `chamados/services/disputa_emissao.py:1638`, `T:183`, `T:262`.)

- **Autor do carimbo perdido:** trocar a atribuição de `materializada_por_id` por `None`. Os selos verificam instante, via, vivacidade e par resposta/carimbo, mas não o autor; o C6 também não consulta esse autor. (`chamados/models.py:2530`, `T:184`, `T:218`, `T:264`, `chamados/juizes.py:788`, `chamados/juizes.py:1032`.)

5. **Relógio, mock e ordem**

- **Relógio:** a classe congela o tempo; o censo recebe `HOJE` explicitamente e o controle usa timestamp UTC literal. Não identifiquei relógio solto nesses caminhos por leitura. (`T:109`, `T:55`, `T:175`, `T:251`.)
- **Mock:** está limitado ao `with`, e a identidade da função real é assertada antes das leituras do selo e do vigia. Não há vazamento demonstrado no código. (`T:213`, `T:216`, `T:220`, `T:221`.)
- **Ordem:** cada teste cria seu cenário, usando `TestCase` e `setUpTestData`; não aparece dependência explícita de dados deixados por outro dos cinco. (`T:110`, `T:112`, `T:201`, `T:212`, `T:236`, `T:259`, `T:276`.) [nao verificado]
- **Risco condicional de sequência de PK:** o banco gera o PK, mas o teste exige que ele não coincida com IDs históricos fixos. Uma colisão faria o selo falhar por causa da sequência, não do comportamento pretendido; ocorrência efetiva **[nao verificado]**. (`T:154`, `T:265`, `A:1098`, `T:297`.) [nao verificado]

Não executei os testes: o quinto cria arquivo temporário, incompatível com a restrição de somente leitura. (`T:284`, `T:286`.) As contagens históricas de testes verdes/vermelhos dos anexos não foram reproduzidas: **[nao verificado]**. [nao verificado]

## 01/11 18:24 -- HISTORICO (mais de 24 h) · sessao dae46ff611b1 · - · - · origem=Ronald (tmux)

- Relógio fixado pela classe em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:109`; o vigia recebe `hoje=HOJE` explicitamente em `:175`, definido em `:55`; controles usam timestamp UTC literal em `:251`. Não encontrei relógio solto demonstrável nesses pontos; execução [nao verificado].
- O único patch fica no `with` em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:213`; a restauração da função real é assertada em `:216`, antes dos leitores em `:220` e `:221`. Vazamento demonstrado: nenhum; execução [nao verificado].
- Há **risco condicional de dependência da sequência de PKs**: `_cenario` deixa o banco gerar `p.pk` em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:154`, mas depois exige que ele não esteja na tupla histórica em `:265`. Essa tupla contém IDs fixos em `app/chamados/management/commands/auditar_invariantes_chamados.py:1098`. Outra sequência de execução pode produzir colisão; ocorrência real [nao verificado]. O quinto teste também exige que o comando real conte esse PK, em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:297`.
- O teste declara e injeta o meio-estado em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:156` e `:158`; o escritor normal grava o par texto/carimbo em `app/chamados/services/disputa_emissao.py:1247`. Portanto é entrada adversarial explícita, não resposta produzida pelo core.
- Veredito e lâmpadas vêm do teste em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:56` e `:160`; a porta escreve os campos em `app/ponto/portas/celula.py:768`. `_fechar` chama produção, relê o banco e verifica preservação em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:168`; ele não reescreve o resultado. Isso comprova preservação da premissa, sem comprovar cálculo real das lâmpadas.
- A classe usa `TestCase` e `setUpTestData` em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:110`; cada um dos cinco testes cria seu cenário em `:201`, `:212`, `:236`, `:259` e `:276`. Dependência de dados deixados por outro dos cinco testes não aparece no código; comportamento do framework em execução [nao verificado].
- Não executei os testes: o quinto cria arquivo temporário em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:284`, incompatível com a auditoria somente leitura. Resultados dos logs anexados [nao verificado].

## 01/11 18:24 -- HISTORICO (mais de 24 h) · sessao 0c1e939852f1 · - · - · origem=Ronald (tmux)

Auditoria estática, sem executar testes ou mutantes; resultados empíricos dos logs citados: [nao verificado]. [nao verificado]

- **Triagem 2b/3 sustentada com ressalva de proveniência:** o próprio teste injeta `resposta_colab` sem `respondida_em`; os controles também são escritos pelo teste. São entradas declaradas, não resultados produzidos pelo fecho (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:154-160`, `:251-255`). O teste verifica que o meio-estado permanece e que o I20 real o observa (`:264`, `:270-271`). A escrita normal do par pelo “core” não foi localizada nesta subtarefa: [nao verificado].

- **Preservação da célula:** o teste fornece veredito e lâmpadas pela porta (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:160`); a produção escreve `ata`, `veredito`, `veredito_via` em `app/ponto/portas/celula.py:768-773`. `_fechar` consulta novamente o banco e compara exatamente esses insumos; não os reescreve (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:168-171`). São premissas preservadas, não prova de que o cartório calculou corretamente essas premissas (`:160`, `:170-171`).

- **Quem escreve/lê alcance:** `censo_alcance` inicializa os campos em `app/chamados/services/vigia_fio.py:539-543`; calcula `alcance_universo` de `fora_da_folha` em `:548-553`; escreve `alcance_dias` em `:589`, `fio_morto_alcance` em `:650`, `alcance_a..e` em `:651`, `alcance_e_detalhe` em `:652-653`. O teste chama o censo real e filtra pelo identificador da célula (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:175-176`); lê linha, totais, soma e detalhe em `:191-197`, e a causa `c` em `:224`. Nenhum desses resultados é preenchido pelo teste nesses pontos (`:175-197`).

- **Quem escreve/lê I20:** o auditor lê `respondida_em`, `resposta_colab` e `pk` no banco e exclui a cicatriz (`app/chamados/management/commands/auditar_invariantes_chamados.py:1098-1103`); `add` calcula total e amostra (`:559-566`); o auditor calcula flag e imprime (`:1833-1837`). O teste captura saída (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:95-106`), interpreta a linha (`:80-89`) e compara flag/total/amostra (`:245`, `:293`, `:297`). A mordida injeta o `pk` apenas na cópia textual do auditor, explicitamente identificada como mutante (`:280-291`).

- **Triagem 4a parcialmente suficiente:** remover o incremento inteiro em `vigia_fio.py:651` viola a soma exigida pelo teste em `test_o261e_f2_ramo_e_i20.py:196`; remover o detalhe do ramo viola `:197`. Entretanto, o selo não garante que cada contador corresponde à causa da linha (`:192`, `:196-197`). [nao verificado]

- **Mutante vivo concreto, por análise estática:** substituir `d['alcance_' + causa] += 1` por `d['alcance_a'] += 1` em `app/chamados/services/vigia_fio.py:651`. A soma permanece correta, a linha continua com causa `e` e o detalhe permanece correto (`:652-654`); o teste não compara nenhum bucket individual, apenas a soma (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:192-197`). No MORDE, o helper já lança pela linha `c` antes de chegar à soma (`:192`, `:224-226`). A sobrevivência executada deste mutante: [nao verificado].

- **Outro mutante vivo:** omitir em `vigia_fio.py:653` o detalhe `sem_pergunta_viva`, mantendo o detalhe do ramo. O teste exige apenas `alcance_e_detalhe[CAUSA_DO_RAMO] == 1`, sem somar detalhes ou conferir o detalhe do outro dia (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:193-197`). Sobrevivência executada: [nao verificado].

## 01/11 18:24 -- HISTORICO (mais de 24 h) · sessao 853d16ba45bf · - · - · origem=Ronald (tmux)

- **Triagem 2a / pergunta 2: corresponde ao código.** O cenário cria pergunta `saida_sem_entrada`, sem texto, com DNA `hi=08:00` e lâmpada E acesa; esse motivo corresponde a E/hi. Antes do carimbo, o C6 pula o degrau da resposta, pula o BUG 117 por `materializada_em=None`, e retorna viva porque `furo` acusa. Referências: `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:61`, `:147`, `:236`; `app/chamados/juizes.py:845`, `:1047`, `:1077`, `:1081`; `app/escala/models.py:1735`.

- **Retirando apenas a guarda do fecho:** a porta aplica o carimbo em memória, mantém `respondida_em=None` porque o texto está vazio e chama novamente o C6. Agora o BUG 117 encontra carimbo + fato do marco provado e retorna morta antes de consultar o veredito do dia; a porta grava. Referências: `app/chamados/services/disputa_emissao.py:1626`, `:1638`; `app/chamados/models.py:2529`, `:2555`, `:2556`, `:2565`, `:2576`; `app/chamados/juizes.py:869`, `:887`, `:893`, `:1077`.

- **Com resposta:** a porta preenche `respondida_em` antes de perguntar ao C6. O degrau da resposta precede o BUG 117; dia `furo` impede que `_resposta_concorda_com_a_luz` retorne verdadeiro, conservando a pergunta viva e causando recusa da porta. Referências: `app/chamados/models.py:2556`; `app/chamados/juizes.py:1029`, `:1047`, `:1059`; `app/chamados/models.py:2569`. A execução histórica `mordida_v9_F.log` é **[nao verificado]**.

- **Triagem 4b: descarte sustentado para estes cinco testes.** Tornar `_desfazer_em_memoria` inoperante não grava os campos: a porta continua retornando `False` antes do `save`, o fecho executa `continue`, a avaliação final da disputa consulta novamente suas perguntas e os selos recarregam o banco. Referências: `app/chamados/models.py:2569`, `:2576`, `:2579`, `:1767`, `:1778`; `app/chamados/services/disputa_emissao.py:1638`, `:1655`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:169`, `:183`, `:217`, `:262`, `:278`. Sobrevivência desse mutante executada nesta revisão: **[nao verificado]**.

- **Outro mutante vivo dedutível:** substituir `self.materializada_por_id = autor_id` por `None` em `app/chamados/models.py:2530`. Os cinco testes não assertam o autor; suas verificações da pergunta cobrem carimbo temporal, via, vivacidade e o par resposta/carimbo, enquanto a chave e os degraus do C6 não consultam `materializada_por_id`. Referências: `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:184`, `:218`, `:264`; `app/chamados/juizes.py:788`, `:1032`. Sobrevivência por execução: **[nao verificado]**.

_1466 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
