# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 09:17** · publicado em **09/11/2026 09:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 09:17 · sessao bbec4829eb42 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

5. Entrega no [relatório por hunk, escritores/leitores e comandos individuais](docs/BUG-PAR-INTERVALO-V2.txt:9).

- **DECISÃO-ABERTA — P2:** ligação de C7 ao predicado reservada à sessão; import e linha exata em [entrega:205](docs/BUG-PAR-INTERVALO-V2.txt:205). Teste: [test_P2_C7_nao_fecha_se_irma_sai_do_fato](chamados/tests/test_bug_par_intervalo.py:388).
- **Verificação:** `py_compile` sem persistência retornou rc 0; âncoras e sintaxe de M1–M37 conferidas. [entrega:301](docs/BUG-PAR-INTERVALO-V2.txt:301)
- **Sem execução da suíte ou dos mutantes:** RED/GREEN e mortes por asserção **[nao verificado]**. F1–F4 e produção permanecem fora da cobertura. [entrega:218](docs/BUG-PAR-INTERVALO-V2.txt:218)

## 09/11 09:10 · sessao 34f994372353 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 09:08 · sessao 790af1e10d16 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 09:08 · sessao be6e6f7b42d9 · - · - · origem=Ronald (tmux)

- **P1 ainda PROCEDENTE, fechamento parcial.** A conferência passou a contar ACL direto/PUBLIC de tabelas e colunas, sequências, schemas, banco e defaults; também membership e ownership. Porém ignora `is_grantable`: SELECT/USAGE/CONNECT com grant option continua considerado permitido, embora habilite DCL. Conserto mínimo: contar `e.is_grantable OR e.privilege_type <> '<permitido>'` nas respectivas chaves e acrescentar caso comportamental ao selo. (`bin/sonda_leitor_comum.sh:87`, `bin/sonda_leitor_comum.sh:101`, `bin/sonda_leitor_comum.sh:107`; validade em PostgreSQL 17 executado: [nao verificado])

- **Cura de P1 incompleta, com recusa segura.** O provisionador revoga ACL de tabela/sequência/schema do leitor e defaults de `saas_user`; não remove ACL de coluna, DML de PUBLIC em tabelas/sequências, defaults de outros donos nem defaults de schema. A conferência verifica essas classes mais amplas e a pós-conferência impede publicação quando restam privilégios. Conserto mínimo: explicitar que esses casos exigem saneamento separado, ou revogá-los deliberadamente pela porta, contemplando o impacto nos demais papéis. (`bin/sonda_provisionar.sh:105`, `bin/sonda_provisionar.sh:110`, `bin/sonda_provisionar.sh:129`; `bin/sonda_leitor_comum.sh:92`, `bin/sonda_leitor_comum.sh:107`)

- **P3: fechado no fluxo serial; concorrência permanece PROCEDENTE.** O arquivo só é criado/promovido depois da pós-conferência, mediante temporário único e `mv`; falhas anteriores preservam o arquivo anterior. Contudo a senha é escolhida antes das alterações no banco e não há exclusão entre duas chamadas: dois provisionamentos sem arquivo podem escolher senhas distintas e ordenar ALTER ROLE/mv de formas incompatíveis. Conserto mínimo: trava compartilhada entre destinos envolvendo leitura/geração da senha até promoção do arquivo. (`bin/sonda_provisionar.sh:63`, `bin/sonda_provisionar.sh:66`, `bin/sonda_provisionar.sh:88`, `bin/sonda_provisionar.sh:129`, `bin/sonda_provisionar.sh:140`, `bin/sonda_provisionar.sh:143`)

- **P5: correção estática presente.** A senha admite apenas caracteres alfanuméricos, entra por `\set pw` e é referenciada como `:'pw'` fora de DO; stdout é descartado e stderr é mascarado antes de voltar ao chamador. Conserto adicional para esse achado específico: nenhum identificado na análise estática. Execução e comportamento de logs do PostgreSQL real: [nao verificado]. (`bin/sonda_provisionar.sh:70`, `bin/sonda_provisionar.sh:78`, `bin/sonda_provisionar.sh:88`, `bin/sonda_provisionar.sh:92`)

- **ESMERIL-SEMANTICA: a chave `ro` tem um sentido compartilhado, mas “pronto” operacional diverge.** `ro` significa somente configuração global da role, com `setdatabase=0`; leitura e provisionamento usam o mesmo produtor/parser. Entretanto a leitura exige também `default_transaction_read_only=on` na sessão TCP, enquanto o provisionador altera apenas a configuração global e anuncia pronto sem esse login. Um override específico de banco não é inspecionado nem removido por esses trechos. Conserto mínimo: conferir/sanear o override do destino e exigir o mesmo login antes do `mv`; o fake atual devolve invariavelmente `leitor|on`. (`bin/sonda_leitor_comum.sh:64`, `bin/sonda_leitor_comum.sh:119`; `bin/sonda_provisionar.sh:93`, `bin/sonda_provisionar.sh:129`, `bin/sonda_provisionar.sh:146`; `bin/sonda_leitura.sh:205`, `bin/sonda_leitura.sh:211`; `bin/tests/test_sonda_sem_grant.sh:60`)

- **Demais chaves: identidade compartilhada, sem tradução encontrada nesses consumidores.** `role/super/poderes/login` vêm de `pg_roles`; `connect` verifica CONNECT efetivo; `usage_falta`, `tab_falta` e `seq_falta` contam ausência de leitura em `public juliani`; `default_falta` exige os quatro pares schema×tipo de defaults de `saas_user`; `esc_*` contam ACL diferente da operação permitida; `default_escrita` inspeciona defaults de qualquer dono; `membro` conta memberships diretos; `dono` conta dependências de ownership no cluster. O parser exige as 17 chaves e compara presença com 1 e ausência com 0. (`bin/sonda_leitor_comum.sh:60`, `bin/sonda_leitor_comum.sh:69`, `bin/sonda_leitor_comum.sh:80`, `bin/sonda_leitor_comum.sh:87`, `bin/sonda_leitor_comum.sh:107`, `bin/sonda_leitor_comum.sh:111`, `bin/sonda_leitor_comum.sh:119`, `bin/sonda_leitor_comum.sh:126`)

- **SQL PostgreSQL 17: nenhum erro sintático identificado nesses pontos por análise estática; execução real [nao verificado].** `printf` transforma `%%I` em `%I` para `format`; os comandos dinâmicos estão em `DO ... EXECUTE`; as variáveis psql ficam fora de corpos dollar-quoted; defaults usam `FOR ROLE saas_user`, coerente com `PGU`. O selo fake grava texto e retorna resultados preparados, sem interpretar esse SQL. (`bin/sonda_provisionar.sh:88`, `bin/sonda_provisionar.sh:92`, `bin/sonda_provisionar.sh:94`, `bin/sonda_provisionar.sh:105`, `bin/sonda_provisionar.sh:110`, `bin/sonda_provisionar.sh:120`; `bin/sonda_leitor_comum.sh:8`; `bin/tests/test_sonda_sem_grant.sh:56`, `bin/tests/test_sonda_sem_grant.sh:59`, `bin/tests/test_sonda_sem_grant.sh:71`)

## 09/11 09:08 · sessao 437ffdfdf483 · - · - · origem=Ronald (tmux)

[resposta retida: senha_atribuida -- nao se publica credencial nem caminho de segredo]

## 09/11 09:03 · sessao cc20c7413249 · CODEX-2-RAIAS-PORTAO-CPU · leitura · origem=Code

**O contrato cobre o núcleo do aval, mas deixa lacunas de execução; a cópia ainda implementa uma única vaga, sem os quatro portões da segunda.** (contrato.txt:2-6; bin/codex_pergunta.sh:228-239.)

Nas referências aos anexos, contei o cabeçalho do contrato como linha 1, os cinco itens como linhas 2–6 e o aval como linha 1. São referências ao texto fornecido; presença desses arquivos na cópia: [nao verificado].

**1. Cobertura de cada exigência literal**

Todas as exigências abaixo vêm de `aval_codex_2_raias_portao_cpu.txt:1`.

| Exigência do aval | Atendimento no contrato |
|---|---|
| “até 2 construtores Codex simultâneos” | Duas vagas nomeadas; a segunda só é tentada quando a primeira está presa. Falta definir a coordenação que garante essa condição durante a abertura. (contrato.txt:2.) |
| “2a vaga SO abre com load de 5 min < 6 (nproc 8 - 2)” | Prevê `load5 < nproc-2`; a mensagem exemplificada usa `>=6`. Equivale ao corte 6 quando `nproc=8`; valor efetivo no host: [nao verificado]. (contrato.txt:2-3.) |
| “medido no /proc/loadavg na hora da abertura” | Fonte e momento constam; ilegibilidade recusa. (contrato.txt:2,4.) |
| “fora de 06:00-07:30” | Consta, mas sem fuso ou definição da inclusão exata de 07:30. (contrato.txt:2-3.) |
| “sem arquivo em comum com a 1a raia” | Prevê interseção, `--toca` obrigatório para a segunda e reconferência final. Não define quando a primeira publica/atualiza o conjunto nem quanto tempo o preserva para a outra conferir. (contrato.txt:4.) |
| “teto total de construtores = 3 (2 Codex + 1 opus)” | Duas vagas Codex e opus 1 por conduta compõem esse teto; não há guarda conjunta prometida. A cópia também declara opus como conduta. (contrato.txt:2,4; CLAUDE.md:350-353.) |
| “Suite inteira continua serializada pela trava unica” | Preservado expressamente; o instrumento existente prende `/tmp/juliani_db_test.lock` durante o comando. (contrato.txt:4; bin/trava_teste.sh:21,35-42.) |
| “Medida (…) no RELATO 1x por dia” | Os quatro indicadores e a frequência constam. Faltam responsável, acionamento e modo de produzir média diária, ocupação e `%/h`. (contrato.txt:5.) |
| “load medio do dia passar de 7” | Mesmo limiar, avaliado na abertura da segunda vaga. Não descreve o tratamento de uma segunda vaga já em execução. (contrato.txt:6.) |
| “Codex passar de 85% antes do reset, volta a 1 sozinho” | Mesmo limiar e condição; não especifica qual janela de `used_percent`, validade da leitura ou duração do desligamento. (contrato.txt:6.) |
| “wrapper recusa a 2a vaga sem o portao”; mutante removido vermelho | Repetido expressamente, mas ainda sem esse caso no selo atual. (contrato.txt:6; bin/tests/test_codex_pergunta.sh:410-415,437-505.) |
| Custo: carga adicional e cota “~2x mais rapido” | Não repetido nas cinco cláusulas. Comprovação desse custo pelo dado fornecido: [nao verificado]. (contrato.txt:2-6.) |
| “Segue a fila; PAREI so em lei ou !” | Não repetido nas cinco cláusulas; elas tampouco descrevem alteração dessa orientação. (contrato.txt:2-6.) |

As fontes de medida precisam ser distinguidas: `ler_semanal()` entrega porcentagem e duração da janela, **sem `resets_at`**; `leituras_semanais()` já oferece histórico com hora e reset da janela semanal principal. `statusline_cota.py` grava uma fotografia de porcentagem/hora/reset, sobrescrevendo a anterior, e não calcula `%/h`. A trava registra ocupante e início atuais, sem produzir histórico de ocupação. (bin/codex_limite_estado.py:118-123,131-161; bin/statusline_cota.py:45-63; bin/trava_teste.sh:40-41.)

**2. Onde os portões entram e qual estado falta**

O sítio comum vem do `git-common-dir`, em `#G:vaga_sitio`; não é simplesmente `$R`, `$VIVO` ou `$LOGD`. A abertura atual usa fd 6 em append, seguida de `flock -n`; sua falha encerra a chamada em `#G:vaga`. **É nesse ramo de falha que o contrato coloca load, janela, arquivos e desligador, antes da admissão na vaga 2.** Não há esse ramo adicional hoje. (bin/codex_pergunta.sh:72-78,228-239; contrato.txt:2.)

- **Load e janela:** pertencem à decisão de abertura da vaga 2. Depois da vaga atual ainda há espera pelo slot geral e pela trava da resposta; portanto abertura da vaga e início do Codex são momentos distintos. (contrato.txt:2; bin/codex_pergunta.sh:341-369,394-398.)
- **Arquivo em comum:** comparação na abertura e reconferência junto ao inventário final, antes da liberação da resposta para triagem. O descarte atual por inventário já ocorre antes do `.id`. (contrato.txt:4; bin/codex_pergunta.sh:413-426,442-445.)
- **Desligador:** o automático pertence à abertura da segunda vaga. O manual tem caminho próprio no contrato; hoje o wrapper consulta apenas a pausa geral `logs/pausas/codex-construtor`. `pausar.sh` já exige dono, motivo e saída e registra trilha. (contrato.txt:6; bin/codex_pergunta.sh:191,387; bin/pausar.sh:31-47.)

**Hoje o wrapper não publica na vaga o que a raia 1 toca.** O arquivo recebe abertura e lock; `_INV0` é uma fotografia inicial obrigatoriamente vazia, `_INV1` fica em variável na revalidação, e só depois do Codex o inventário é persistido em `<resposta>.inventario`. (bin/codex_pergunta.sh:232-239,386-392,413.)

Para cumprir a comparação descrita, falta vincular ao ocupante atual: identidade da chamada, WT, base e conjunto de caminhos comparáveis, distinguindo estado ainda não publicado de conjunto vazio. Também falta preservar a informação necessária até a reconferência das duas raias; uma fotografia vazia na abertura não informa os próximos arquivos que a primeira tocará. São lacunas do mecanismo prometido, não novos critérios de admissão. (contrato.txt:4; bin/codex_pergunta.sh:237-239,413.)

O inventário existente já compara arquivos com a base, inclui status, ignorados e varredura de filesystem, e emite `TOCADO`/`PROIBIDO`. **`diff base..HEAD` sozinho não resolve:** no fluxo aceito, HEAD deve continuar igual à base; HEAD diferente já é proibido. Não existe opção `--toca` no parser atual. (bin/codex_pergunta.sh:58-66,137-178,234.)

**3. Riscos pedidos**

- **Corrida entre aberturas:** hoje não existe vaga 2. No desenho anexado, observar a vaga 1 presa e depois tomar a 2 não garante que a primeira continue presa: seu dono pode terminar no intervalo. Outra abertura pode então tomar a 1, enquanto a 2 carrega informação do ocupante anterior. O contrato não especifica coordenação dessa transição. Há ainda o intervalo entre adquirir a primeira vaga e terminar sua validação, quando o estado pode não estar pronto. (contrato.txt:2,4; bin/codex_pergunta.sh:233-242.)

- **Recusa calada:** a recusa atual fala por stderr. Entretanto, ocorre antes do trap e do registro `ini`; assim, não deixa `ini/fim` nem `.rc` dessa tentativa. Um acompanhamento limitado a esses artefatos não a verá. (bin/codex_pergunta.sh:183,233,249-262,285-286.)

- **Relógio e borda:** os `date` atuais não fixam fuso; o contrato não resolve fuso nem extremos da janela. As esperas posteriores podem atravessar 06:00 ou 07:30. Isso exige distinguir “hora da abertura”, exigida pelo aval, de “hora de iniciar o Codex”; o contrato não exige encerramento de uma execução que atravesse a janela. Relógio real do host: [nao verificado]. (contrato.txt:2; bin/codex_pergunta.sh:245,341-369,374,394.)

- **Espera bloqueante:** o contrato garante recusa imediata pelo portão fechado, mas isso não elimina as esperas existentes por slot geral e resposta depois de admitido. Se “ninguém espera” pretendia abranger a chamada inteira, há incompatibilidade. (contrato.txt:3; bin/codex_pergunta.sh:341-369.)

- **RC reaproveitado:** rc 2 já cobre uso/validação e vaga ocupada; rc 3 já cobre cópia, trava de resposta/B3 e inventário; rc 75 já cobre pausa geral e limite. Os novos rc 2/3 não identificariam a causa sozinhos: o motivo precisa sobreviver ao chamador. (contrato.txt:3-4; bin/codex_pergunta.sh:66,183,191,233,294-296,367-371,415-435.)

- **Arquivo da vaga/inode:** a proteção atual reserva somente o caminho exato de `construtor.vaga`; não reservaria automaticamente `construtor.vaga2`. Publicar estado substituindo o arquivo por rename pode separar o caminho do inode ainda preso pelo flock — justamente o acidente que a reserva atual evita. O rename de `pausar.sh` serve ao desligador separado; reutilizá-lo no arquivo travado teria esse risco. Leitores externos à cópia: [nao verificado]. (bin/codex_pergunta.sh:69-78,232-233; bin/pausar.sh:45-46.)

- **`chamadas.log`:** a quebra depende da posição do campo novo. O parser fixa obra/papel/hash/pid/tipo/sessão/rc/duração por índice; acrescentar `vaga=1` ou `vaga=2` ao fim preserva essas posições, inserir no meio pode corromper a leitura. O parser continuará sem distinguir vagas. Contagem, últimas chamadas, procedência e tokens dependem dele; o contrato não especifica a posição. (contrato.txt:5; bin/codex_limite_estado.py:55-78,82-96,185-194.) Os próprios selos fixam posições como `$3`, `$6`, `$7` e `$8`. (bin/tests/test_codex_pergunta.sh:265.)

**4. Selos existentes e mutantes**

| Cobertura existente | Caso e mutante |
|---|---|
| Vaga ocupada, inclusive com outro LOGD | `c_vaga` exige rc 2, motivo e Codex não chamado. **O mutante direto de `#G:vaga` troca `_recusa2` por `true`**, deixando prosseguir após falhar o flock. (bin/tests/test_codex_pergunta.sh:116-120,193-198,468.) |
| Sítio independente da raiz do script | `c_vaga_copia`; mutante de `#G:vaga_sitio` troca `dirname($_CDIR)` por `$R`. (bin/tests/test_codex_pergunta.sh:374-378,498.) |
| Mesmo sítio entre worktrees do repositório | `c_vaga_outra_wt`; mutante troca `dirname($_CDIR)` por `$VIVO`. (bin/tests/test_codex_pergunta.sh:351-355,499.) |
| Reserva do caminho e falha de abertura | `c_vaga_reservada` e `c_vaga_abre`, incluindo fd 6 herdado; mutantes neutralizam as recusas correspondentes. (bin/tests/test_codex_pergunta.sh:230-238,454-455.) |
| Topologia válida e leitura independente da vaga | Construtor sem sítio recusa; papéis de leitura continuam. Mutantes `vaga_topologia` e `vaga_leitura`. (bin/tests/test_codex_pergunta.sh:361-371,500-501.) |
| Inventário/descarte individual | Positivo verifica inventário, `.rc`, `.id` e log; recusa verifica rc 3 e ausência de `.id`; mutante neutraliza a decisão final. **Não comprova interseção entre raias.** (bin/tests/test_codex_pergunta.sh:122-128,245-266,410-415,487.) |
| Trava única de suite | `test_trava_teste.sh` usa lock isolado, comprova ordem entre dois chamadores e rc 75 sem vez. (bin/tests/test_trava_teste.sh:103-137.) |

O mecanismo do selo exige mutante para cada etiqueta `#G:` e acusa quando o caso correspondente continua verde após a mutação. **O mutante atual da vaga comprova a proibição antiga de qualquer segundo construtor; não comprova o portão da segunda vaga autorizado pelo aval.** (bin/tests/test_codex_pergunta.sh:193-198,468,507-527.)

Resultados de execução desses selos e do mutante novo “portão removido”: [nao verificado].

## 09/11 09:01 · sessao 692d6935cbf8 · - · - · origem=Ronald (tmux)

- **Sítio e entrada:** a vaga é derivada do `git-common-dir` do repo vivo em `bin/codex_pergunta.sh:72-78`; o construtor confere topologia em `:189`, abre o arquivo em append em `:231-232` e tenta `flock -n` em `:233`. Hoje só há uma vaga, com recusa falante rc 2; a vaga 2 e os quatro portões não existem nesse ramo (`bin/codex_pergunta.sh:228-239`).

- **Posição dos portões:** no desenho anexado, entram no ramo de falha do primeiro `flock`, hoje `#G:vaga`, antes de admitir a vaga 2 (`bin/codex_pergunta.sh:233`). Entretanto, ainda existem esperas pelo slot geral e pela trava da resposta antes do Codex; uma leitura de load/hora nesse ponto pode envelhecer antes da execução (`bin/codex_pergunta.sh:341-369`, `:394-398`).

- **Estado disponível:** o arquivo da vaga apenas é aberto, sem receber identificação ou arquivos tocados (`bin/codex_pergunta.sh:232`). O precheck exige inventário vazio; a revalidação também guarda a saída apenas em variável; somente o inventário final é persistido em `<resposta>.inventario` (`bin/codex_pergunta.sh:237-239`, `:386-392`, `:413`). Portanto o wrapper não publica hoje o que a raia 1 toca enquanto trabalha (`bin/codex_pergunta.sh:232`, `:413`).

- **Dados necessários para cumprir a comparação:** identificação da chamada/WT/base e conjunto de caminhos ligados ao ocupante atual, com indicação de publicação pronta/em curso/final. O contrato não especifica protocolo de publicação nem preservação do estado até ambas reconferirem [nao verificado]. A fotografia vazia da abertura não prevê os toques posteriores (`bin/codex_pergunta.sh:237-239`). O inventário atual produz `TOCADO<TAB>caminho<TAB>motivo`, inclui status e varredura contra a base; HEAD diferente da base é proibido (`bin/codex_pergunta.sh:137-178`). Não existe `--toca` no parser (`bin/codex_pergunta.sh:58-66`).

- **Reconferência final:** o ponto natural é junto ao inventário, antes de emitir `.id`; a recusa já vence outros rc e impede o sidecar (`bin/codex_pergunta.sh:408-426`, `:442-445`). Se a primeira raia apagar seu estado ao terminar antes da segunda, a segunda perde a comparação; a duração desse estado não foi definida no contrato [nao verificado].

- **Corrida:** hoje não há vaga 2 (`bin/codex_pergunta.sh:233`). No contrato, entre observar a vaga 1 presa e tomar a vaga 2, a primeira pode terminar; faltou tornar verificável que continua ocupada [nao verificado]. Além disso, a primeira adquire o lock antes de validar/publicar qualquer inventário, portanto a segunda pode encontrá-la antes de haver estado pronto (`bin/codex_pergunta.sh:233-242`).

- **Recusa calada:** `_recusa2` escreve motivo no stderr (`bin/codex_pergunta.sh:183`). Contudo, a recusa da vaga ocorre antes de instalar o trap e de registrar `ini`; não produz `.rc` ou `ini/fim`, o que a torna invisível para leitores que só acompanham esses artefatos (`bin/codex_pergunta.sh:233`, `:245-262`, `:285-286`).

- **RC:** 2 já significa uso/validação; 3 já cobre cópia, trava da resposta e inventário; 75 já significa pausa geral e limite. Acrescentar portão em 2 e sobreposição em 3 mantém ambiguidades que exigem motivo/artefato para distinguir (`bin/codex_pergunta.sh:66`, `:183`, `:191`, `:294-296`, `:367-369`, `:415-435`).

- **Relógio:** os `date` atuais não explicitam fuso; o contrato não define fuso nem inclusão exata das bordas [nao verificado] (`bin/codex_pergunta.sh:245`, `:252-255`, `:285`, `:343`, `:374`). As esperas posteriores à vaga podem atravessar a borda (`bin/codex_pergunta.sh:341-369`).

- **Leitores:** `codex_limite_estado.py` lê campos posicionais: tipo em `p[5]`, sessão em `p[6]`, rc em `p[7]`, duração em `p[8]`; inserir `vaga=...` antes deles quebra a leitura, enquanto acrescentar após os campos existentes preserva esse parser, mas ele não passa a distinguir vagas (`bin/codex_limite_estado.py:55-67`). Tokens e procedência dependem da sessão assim obtida (`bin/codex_limite_estado.py:185-194`; `bin/gerar_codex_estado.py:153-155`).

- **Desligador manual:** `pausar.sh` aceita caminho arbitrário e registra QUEM/QUANDO/POR_QUE/SAIDA, publica por rename e mantém trilha; isso serve ao arquivo novo descrito no contrato, mas o wrapper atual só consulta a pausa geral `logs/pausas/codex-construtor` (`bin/pausar.sh:31-47`; `bin/codex_pergunta.sh:191`, `:387`).

## 09/11 09:01 · sessao 6661071fd3d5 · - · - · origem=Ronald (tmux)

- **Não há quebra demonstrada apenas por adicionar `vaga=1|2`**: o leitor atual ignora campos finais, pois aceita `len(p)>=9`; inserir o novo campo antes das posições existentes deslocaria a interpretação. Chave `(obra,hash,pid)` em `p[1],p[3],p[4]`; tipo em `p[5]`; sessão, rc e duração em `p[6:9]`. O contrato não informa a posição da coluna. (`bin/codex_limite_estado.py:56-67`; contrato fornecido, item 4 [nao verificado como arquivo local]).

- **Leitores afetados por deslocamento**: contagem por papel (`bin/codex_limite_estado.py:71-78`), últimas chamadas/rc (`bin/codex_limite_estado.py:82-88`), origem Code (`bin/codex_limite_estado.py:92-96`; `bin/gerar_codex_estado.py:154-155`; `bin/gerar_codex_estado.py:176`), tokens por papel (`bin/codex_limite_estado.py:185-194`) e painel/disjuntor (`bin/gerar_estado.py:269`; `bin/disjuntor.py:1603`; `bin/disjuntor.py:1611`). O selo também fixa `$3`, `$6`, `$7`, `$8`. (`bin/tests/test_codex_pergunta.sh:265-266`).

- **Arquivo da vaga hoje não contém estado da raia**: o trecho abre em append e adquire lock; o inventário inicial fica em `_INV0`, e o final vai para `$RESPOSTA.inventario`. Não há publicação desses dados na vaga nesse fluxo. (`bin/codex_pergunta.sh:231-239`; `bin/codex_pergunta.sh:413`). Consumidores externos à cópia [nao verificado].

- **Identidade do inode importa**: o próprio comentário explica que usar o nome da vaga como resposta substituiria o inode preso; a proteção reserva somente o caminho exato da vaga atual. Uma futura `construtor.vaga2` não fica reservada por essa comparação atual. (`bin/codex_pergunta.sh:69-78`). Publicar estado da vaga por rename pode deixar o flock preso no inode anterior, pelo mesmo mecanismo documentado. (`bin/codex_pergunta.sh:69`; `bin/codex_pergunta.sh:232-233`).

- **`pausar.sh` atende trilha do desligador separado**: exige quem, motivo e condição de saída, escreve temporário e renomeia, registra pausa; soltura também registra trilha. (`bin/pausar.sh:25-28`; `bin/pausar.sh:31-47`). Usar esse rename no próprio arquivo de lock teria o risco anterior. (`bin/pausar.sh:46`; `bin/codex_pergunta.sh:69`).

- **rc reaproveitados**: rc 2 já significa validação/recusa anterior à chamada, não especificamente portão; rc 3 já significa inventário proibido/falho, mas também trava de resposta recusada ou B3. (`bin/codex_pergunta.sh:183`; `bin/codex_pergunta.sh:367-371`; `bin/codex_pergunta.sh:410-426`). rc 75 também serve à pausa do construtor e ao LIMITE; distinguir só por número não distingue causa. (`bin/codex_pergunta.sh:191`; `bin/codex_pergunta.sh:432-435`).

- **Recusa não é registrada em `chamadas.log` quando acontece na vaga**: a guarda da vaga está antes da instalação do trap e da linha `ini`; portanto o motivo atual sai por stderr, mas não produz par `ini/fim` nem `.rc` desse fluxo. (`bin/codex_pergunta.sh:233`; `bin/codex_pergunta.sh:249-262`; `bin/codex_pergunta.sh:285-286`). `.rc` depende de `INI=1`. (`bin/codex_pergunta.sh:251-256`).

- **Teto opus é conduta declarada, sem guarda no wrapper**. (`CLAUDE.md:350-353`).

## 09/11 09:01 · sessao 87cc07e0ba8f · - · - · origem=Ronald (tmux)

Inspeção estática apenas; veredictos de execução e situação no host: [nao verificado].

- O selo direto da vaga é `bin/tests/test_codex_pergunta.sh`: `c_vaga` prende `logs/construtor.vaga` e exige recusa também com outro `LOGD` (`:193–198`). O helper exige rc esperado, motivo e ausência de chamada ao Codex (`:116–120`).
- O mutante que morde especificamente `#G:vaga` troca `_recusa2` por `true`, preservando o `flock` que falhou e permitindo prosseguir (`bin/tests/test_codex_pergunta.sh:468`; guarda em `bin/codex_pergunta.sh:233`). O teste detecta a continuação pelo rc/motivo/chamada (`bin/tests/test_codex_pergunta.sh:116–120`).
- Dois mutantes mordem `#G:vaga_sitio`: raiz do script `$R` em vez de `dirname($_CDIR)` (`bin/tests/test_codex_pergunta.sh:498`, caso de cópia em outra raiz `:374–378`); `$VIVO` em vez do common-dir (`:499`, caso de outra worktree do mesmo repositório `:351–355`).
- Há proteção contra resposta substituir o inode da vaga (`bin/tests/test_codex_pergunta.sh:230–232`, mutante `:454`); controle de basename semelhante permitido (`:218–220`); abertura impossível com fd6 herdado deve recusar explicitamente (`:234–238`, mutante `:455`); topologia sem `<raiz>/.git` recusa construtor (`:361–364`, mutante `:500`), mas permite leitura (`:366–371`, mutante `:501`).
- O mecanismo exige mutante para cada etiqueta `#G:` (`bin/tests/test_codex_pergunta.sh:507–508`), aplica a alteração por etiqueta (`:511–520`), executa o caso correspondente e exige que fique vermelho (`:523–527`). O verde atual do host continua [nao verificado].
- Os casos e mutantes listados não contêm portão CPU da vaga 2, `--toca` ou desligador da segunda vaga (`bin/tests/test_codex_pergunta.sh:410–415`, `:437–505`). O mutante atual `vaga` prova exclusão de **todo segundo construtor** pelo comportamento antigo; não é ainda o selo “vaga 2 sem portão” do novo contrato (`bin/tests/test_codex_pergunta.sh:193–198`, `:468`).
- O inventário e descarte já têm selo: positivo exige `.rc`, `.inventario`, `.id` e log final (`bin/tests/test_codex_pergunta.sh:245–266`); recusa exige rc3, motivo, ausência de `.id` (`:122–128`), e mutante da decisão final neutraliza `if [ "$_IRC" -ne 0 ]` (`:487`). Interseção entre raias não aparece nesses casos (`:410–415`).
- A trava única tem `bin/tests/test_trava_teste.sh`: varredura de runners da suite e molde (`:39–71`), execução em cópia com lock isolado (`:103–115`), dois chamadores serializados (`:117–127`) e ausência de vez com rc75 (`:130–137`). `bin/tests/test_suite_sh.sh` cobre o comando canônico da documentação, sua passagem por `suite.sh` e por `trava_teste.sh` (`:37–58`), com fixtures nos dois sentidos (`:65–85`).
- Os selos `test_*.sh` são chamados pela régua antes da suite (`bin/regua.sh:138–145`); execução efetiva nesta produção: [nao verificado].
- Inserir `vaga=1|2` deslocando campos de `chamadas.log` quebra ao menos leitores posicionais dos próprios testes: papel `$3`, fase `$6`, thread `$7`, rc `$8` (`bin/tests/test_codex_pergunta.sh:265`) e duração `$9` (`bin/tests/test_codex_sonda.sh:96–98`). Acrescentar campo ao fim preserva essas posições específicas; compatibilidade de todos os consumidores depende da inspeção deles [nao verificado].
- `bin/tests/prova_codex_pergunta_real.sh` é prova manual que chama Codex real e verifica efeito do sandbox, não selo automático de concorrência (`:2–14`, `:85–86`, `:134–141`).

## 09/11 08:55 · sessao fb013bdd83d7 · FIO-E0-V4-E3 · construtor · origem=Code

| Hunk | ESCRITA / LEITURA |
|---|---|
| `chamados/juizes.py:1420,1448,1451` | `alimentar_portas` recebe `agora`; lê `qs_na_fila` (`chamados/services/painel.py:158`) e `particionar_verbos`. |
| `chamados/services/verbos.py:30,35` | Partição aceita relógio explícito; continua escrevendo `mapa/contagens` (`:36`). |
| `chamados/services/tarefa_do_dia.py:27,34,373,403,418` | Acrescenta `rota/fila/verbos/fio` (`:433`), selecionando portas das ações exigíveis. Lê `alimentar_portas` (`:409`). |
| `chamados/management/commands/diff_e3_sombra.py:30,41,46,60,81,101` | Lê C17 pelo chamador existente (`:117`) e tarefa (`:118`); escreve somente agregados em stdout (`:99`). Guarda SQL em `:92`. |
| `chamados/tests/e3_fixtures.py:13,16,40,65` | Fixtures ORM, `colNNN`, relógio congelado; lidas pelas três baterias novas. |
| `chamados/tests/test_e3_app_orm.py:12,20,28,47` | M1–M5 pelas views completas dos dois kernels, endpoint e dashboard. |
| `chamados/tests/test_e3_portas_tarefa.py:1,32,40,49,57` | Quatro respostas esperadas escritas antes da implementação; casos adicionais em `:67,75,84,100`. |
| `chamados/tests/test_badge_disputa_pendente.py:6` | Substitui casos antigos pelo selo `test_MORDE_pendentes_do_colaborador_nao_pode_voltar`. |
| `chamados/tests/test_esmeril2_left_join.py:10,19,27` | Migra para `consulta_do_app`: sem filhos conta zero; duas perguntas contam duas. |
| `chamados/tests/test_fio_le_o_juiz.py:40,263,267,272` | Migra preservando a prova de vitalidade C6. |
| `core/tests/test_dashboard_badge.py:10,17,40,45` | Migra para a mesma consulta lida pelo dashboard (`core/views.py:253`). |
| `chamados/tests/test_e3_pendentes_app.py:81,98,113` | Remove matriz com ORM dublado; preserva catraca M7 e contrato puro C17. |
| `chamados/tests/test_e3_diff_sombra.py:39,54,68,96,115` | Selos contra escrita SQL/arquivo e contra perda das projeções/casos. |
| `chamados/tests/e3_mutantes.py:19,69,83` | Define mutantes em memória e imprime cada comando individual (`:75`). |

Leitores externos de `alimentar_portas`: `folha/fora_da_folha.py:62`, `chamados/services/vigia_fio.py:604`, `escala/services/cadastro_realidade.py:484`, C17 em `chamados/juizes.py:1536`.

Leitores externos da partição: `chamados/services/painel.py:230`, `fila.py:55`, `na_fila.py:19`, `verbos.py:171`; `chamados/views_arquivar.py:124`, `views_cobrar.py:77`, `management/commands/arquivar_prescritos.py:50`; `colaboradores/services/ficha.py:155`, `core/context_processors.py:90`, `api/views_mensageria.py:395,453,1149`.

Os campos novos são lidos pelo instrumento (`chamados/management/commands/diff_e3_sombra.py:41,46`). Os três leitores atuais continuam chamando C17: `folha/fora_da_folha.py:67`, `chamados/services/vigia_fio.py:635`, `escala/services/cadastro_realidade.py:488`.

**DECISAO-ABERTA:** remoção do método e baixa no registro ficam com a sessão; DIFF com L-099 antes da ligação. O selo L-111 exige ausência do método (`chamados/tests/test_badge_disputa_pendente.py:6`); resultado RED ainda **[nao verificado]**.

Comandos preparados para a sessão; não executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e3_portas_tarefa chamados.tests.test_badge_disputa_pendente --settings=config.settings.ci

# Imprime o comando individual de cada mutante:
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_mutantes

# DIFF — executar somente após a sonda:
PYTHONDONTWRITEBYTECODE=1 python3 manage.py diff_e3_sombra --colab ID --inicio AAAA-MM-DD --fim AAAA-MM-DD --agora INSTANTE_COM_FUSO
```

`py_compile` executado sem gravar bytecode:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -c 'import py_compile, sys; from unittest.mock import patch; patch("importlib._bootstrap_external._write_atomic").start(); [py_compile.compile(p,cfile=p+".verificacao",doraise=True) for p in sys.argv[1:]]' $(GIT_OPTIONAL_LOCKS=0 git ls-files -m -o --exclude-standard -- '*.py')
```

Log: **14 arquivos compilados; `git diff --check` rc 0**. Suíte, RED, mutantes e DIFF não executados; comportamento **[nao verificado]**. [nao verificado]

## 09/11 08:50 · sessao 6499c93501d2 · SELOS-CANON-I1 · verificador · origem=Code

**O item 1 está curado quanto à abertura indevida da vaga.** O espelho está no predicado comum; o Agent chega nele, e o wrapper usa o mesmo predicado, embora recuse previamente os IDs com espaço. Há resíduos fora do diff e um limite na prova do PAREI, detalhados abaixo. (`bin/disjuntor.py:1686`, `:1850`, `:2248`, `:2267`.)

1. **Cura nas duas vias e caminhos alternativos**

   - O Agent chama `_censo_nega`, que consulta `_pertence_fila1`. Agora, célula `O9` contra obra `O9 (C)` ou `O9 NOTURNO-X` produz `ValueError`, convertido em negativa NAO SEI. A saída antecipada como FORA deixou de ocorrer nesses pares. (`bin/disjuntor.py:1850`, `:1704`, `:1686`, `:1691`, `:1730`.)
   - O wrapper chama `--censo` antes de prender a vaga; `main_censo` consulta `_pertence_fila1` e passa a mesma lista de células para `_censo_nega`. Os exemplos com espaço já são recusados antes dessa consulta; no wrapper real, a validação é ainda anterior, no shell. (`bin/codex_pergunta.sh:188`, `:234`, `:240`; `bin/disjuntor.py:2248`, `:2266–2275`.)
   - Nos caminhos de autorização de censo examinados, a decisão passa pelo predicado comum. A igualdade existente nele compara **obra com token**, dentro do próprio juiz. (`bin/disjuntor.py:1685`, `:1704`, `:2267`.)
   - **Existem igualdades fora dele:** o hook compara a célula crua com o alvo do marcador para localizar o item em curso e remove esse item da lista por igualdade. São decisões de marcador/ordem, distintas da autorização do censo. (`bin/hook_stop_fila1.py:223`, `:273`.)
   - **Existe uma derivação própria de fila no alarme**, descrita no item 4. Ela alimenta o disparo do alarme, sem autorizar vaga de construtor. (`bin/alarme_sessao_ociosa.py:89–116`, `:148–154`.)

2. **Quem define, quem lê e significado dos retornos**

   | Função | Definição e leitores encontrados |
   |---|---|
   | `_pertence_fila1` | Definida em `bin/disjuntor.py:1669`; leitores produtivos em `:1704` e `:2267`; chamadas diretas do selo em `bin/tests/test_censo_antes_da_vaga.sh:605` e `:611`. |
   | `_token_da_celula` | Definida em `bin/disjuntor.py:1651`; lida nas listas `exatas` e `nomeiam`, em `:1685–1686`. |
   | `_casa_id_inteiro` | Definida em `bin/hook_stop_fila1.py:110`; lida pelo predicado em `bin/disjuntor.py:1686` e pelo julgamento do PAREI em `bin/hook_stop_fila1.py:1301` e `:1313`. |

   As versões mutantes são escritas pelo mecanismo do selo em cópias de `bin/`; há mutações desses critérios em `bin/tests/test_censo_antes_da_vaga.sh:750–763`, aplicadas por `:669–678`.

   Os dois leitores produtivos de `_pertence_fila1` concordam: `False` permite seguir como FORA; `True` exige o censo; exceção gera negativa NAO SEI. (`bin/disjuntor.py:1704–1731`, `:2267–2281`.) `_token_da_celula` retorna string ou `None`; o espelho não transforma `None` em correspondência, porque `_casa_id_inteiro` rejeita ID vazio. (`bin/disjuntor.py:1660–1666`; `bin/hook_stop_fila1.py:115`.)

   A nomeação ignora caixa e usa fronteira ASCII que exclui letra, dígito e hífen; underscore é separador. A extração do token usa a mesma classe de fronteira. A igualdade exata permanece sensível à caixa, razão de `o9 × O9` resultar em ambiguidade. (`bin/hook_stop_fila1.py:117`; `bin/disjuntor.py:1664`, `:1685–1691`.)

   Alarme e handoff chegam a esses critérios indiretamente pelo hook. Para a prova de PAREI censo, ambos respeitam o contrato próprio do reconhecedor: `None` significa “não é linha de censo”; `''`, prova válida; texto, motivo de reprovação. (`bin/alarme_sessao_ociosa.py:127–140`; `bin/handoff_sessao.sh:129–134`; `bin/hook_stop_fila1.py:773–785`.) O handoff também consome os julgamentos booleanos do hook como booleanos. (`bin/handoff_sessao.sh:120–123`.)

3. **Matriz do predicado literal**

   Cada linha considera `_pertence_fila1(obra, [celula])`. A coluna anterior corresponde à retirada do espelho, exatamente a mutação declarada no selo. (`bin/disjuntor.py:1685–1692`; `bin/tests/test_censo_antes_da_vaga.sh:763`.)

   | Obra | Célula | Atual | Sem espelho | Sítio da saída atual |
   |---|---|---|---|---|
   | `O1` | `O142` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O142` | `O1` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O9` | `O9` | `True` — DENTRO | `True` | `bin/disjuntor.py:1689–1690` |
   | `O9 (C)` | `O9` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `O9 NOTURNO-X` | `O9` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `OBRA-Q (A)` | `OBRA-Q` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `09-TURNO-ABERTO-EXPOSTA` | `O9` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O67b` | `O9` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `o9` | `O9` | `ValueError` — NAO SEI | `ValueError` | `bin/disjuntor.py:1691` |

   **Há três conversões de FORA para NAO SEI**, e elas passam a negar na porta do Agent. O código as declara ambiguidades pretendidas; demonstrar que alguma corresponde a uma obra **de fato fora da fila** em produção é **[nao verificado]** com o dado fornecido. Também não há base para garantir ausência de toda negativa legítima afetada. (`bin/disjuntor.py:1674–1680`, `:1730–1731`.)

   A matriz não equivale à extração do prompt: `OBRA-Q (A)` vira `OBRA-Q` no Agent; `09-TURNO-ABERTO-EXPOSTA`, `O67b` e `o9` não satisfazem o reconhecedor de obra desse caminho. Obra ilegível é negada antes da pertença. (`bin/disjuntor.py:157`, `:602–627`, `:1847–1849`.)

4. **Classes de reprovação**

   - **Derivação paralela presente:** o alarme conta sua própria fila, excluindo `☑`, `FEITO` e `FECHAD`; o hook usa marcadores e `_item_parado`, incluindo exclusões como CONGELADA, FILA 2 e PAREI. Uma linha apenas CONGELADA/FILA 2 pode ser contada pelo alarme e excluída pelo hook. (`bin/alarme_sessao_ociosa.py:89–116`; `bin/hook_stop_fila1.py:95–97`, `:171–180`, `:231–252`.)
   - **Escritores fora da porta presentes na cópia:** `gerar_cortes.py` e `deploy_agendado.sh` regravam RELATO diretamente; o escritor canônico declara esse resíduo. A cura examinada usa `no_relato`/`no_topo`. (`bin/gerar_cortes.py:117–136`; `bin/deploy_agendado.sh:31–44`; `app/core/esteira_vigia.py:839–840`; `bin/disjuntor.py:294`, `:324`.)
   - **Segundo juiz de negócio concorrendo com autoridade registrada:** não demonstrado nesta cura; conclusão abrangente sobre todos os registros: **[nao verificado]**. O caminho examinado reutiliza o hook, o predicado, o coletor e o juiz de brief. (`bin/disjuntor.py:1637`, `:1704`, `:1710`, `:1715`.)
   - **Relógio fora da borda:** não aparece na decisão de pertença/token; a medida recebe `agora` e filtra futuro e janela. (`bin/disjuntor.py:1651–1692`, `:2066`, `:2094`, `:2127–2131`.)
   - **Ordem de trava:** o censo antecede a vaga; as portas de RELATO compartilham trava, liberada ao retornar. Não identifiquei inversão nesse percurso. (`bin/codex_pergunta.sh:234–240`; `app/core/esteira_vigia.py:843`, `:846–862`, `:892`.)
   - **Campo com dois significados:** `_censo_nega` usa `None` para “porta passa”, abrangendo FORA e aprovação; isso está documentado, e `main_censo` distingue FORA pelo predicado. Não encontrei leitor tomando esse `None` por DENTRO. (`bin/disjuntor.py:1696`, `:1704–1705`, `:1724`, `:2267–2275`.)

5. **Mordida dos casos novos e seu limite**

   - **Laço Agent:** prepara célula `O9` sem `O9 (C)` e exige negativa JSON, motivo de censo, PAREI com a obra inteira e negativa correspondente na trilha. Saída vazia reprova. Sem espelho, o predicado retorna FORA antes da validação de forma, retirando a negativa cobrada. (`bin/tests/test_censo_antes_da_vaga.sh:580`, `:520–527`, `:588–591`; `bin/disjuntor.py:1687–1688`, `:1704–1706`.)
   - **ESPELHO direto:** exige `ValueError` nos três pares e exatamente `False` nos dois controles de prefixo. Portanto, “sempre retornar FORA” e “sempre levantar” reprovam. Aceita qualquer `ValueError`, sem validar a mensagem. (`bin/tests/test_censo_antes_da_vaga.sh:603–619`.)
   - **Mutante:** remove precisamente o espelho e executa `casos_pertenca`; há exigência de âncora única, controle sem mutação e propagação de falha pelo retorno dos casos. (`bin/tests/test_censo_antes_da_vaga.sh:763`, `:675–686`, `:662–665`, `:647`.)
   - **Wrapper:** os dois casos são controles expressamente declarados, inclusive no nome; passam com ou sem a cura porque a forma é recusada antes da pertença. (`bin/tests/test_censo_antes_da_vaga.sh:585–593`; `bin/disjuntor.py:2248–2250`.)
   - **Limite material:** o laço Agent prova o PAREI **na mensagem**. Para esses IDs com espaço, `_parei_censo_no_relato` retorna antes de gravar; o laço não cobra persistência no topo. Logo, a mordida demonstra negativa/trilha, mas não demonstra PAREI gravado desses exemplos. (`bin/tests/test_censo_antes_da_vaga.sh:520–527`, `:590–591`; `bin/disjuntor.py:309–311`.)

A execução integral do selo e seu resultado verde permanecem **[nao verificado]**; o script cria e altera arquivos de teste. (`bin/tests/test_censo_antes_da_vaga.sh:27–35`, `:669–678`.) O conteúdo integral da lei citada como `CORTES.md:428` também permanece **[nao verificado]**: a cópia termina em `app/docs/CORTES.md:425`, e o anexo fornecido traz somente o início daquela linha.

## 09/11 08:48 · sessao 400ac2cb4b5d · - · - · origem=Ronald (tmux)

Não encontrei classe nova dessas no caminho da cura do censo, com os limites abaixo.

- **Juiz/derivação única:** `_fila1_viva` delega o universo ao hook; `_censo_nega` usa `_pertence_fila1`, `_briefs_do_censo` e `censo_do_brief`. Agent chama essa porta; wrapper chama `--censo`. A classificação adicional em `main_censo` repete o mesmo predicado e reutiliza a mesma lista `fila`. (`bin/disjuntor.py:1633`, `bin/disjuntor.py:1704`, `bin/disjuntor.py:1710`, `bin/disjuntor.py:1715`, `bin/disjuntor.py:1850`, `bin/disjuntor.py:2266`, `bin/disjuntor.py:2275`; `bin/codex_pergunta.sh:234`.)
- **Escritor/ordem:** a negativa passa por `ev.no_relato`; PAREI passa por `ev.no_topo` e é relido pelo reconhecedor do Stop. Ambas as portas usam o mesmo flock; leitura, classificação, idempotência e gravação do topo ficam sob a trava. O wrapper executa censo antes de tentar vaga; a trava do RELATO já foi fechada quando chega à vaga. (`bin/disjuntor.py:293`, `bin/disjuntor.py:323`; `app/core/esteira_vigia.py:842`, `app/core/esteira_vigia.py:859`, `app/core/esteira_vigia.py:877`, `app/core/esteira_vigia.py:893`; `bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`.)
- **Relógio:** pertença/token não consultam relógio; `_censo_24h` recebe `agora`, exclui futuro e aplica a janela ao casar a abertura. `trilha` consulta relógio para carimbar o registro. (`bin/disjuntor.py:1651`, `bin/disjuntor.py:1669`, `bin/disjuntor.py:2066`, `bin/disjuntor.py:2094`, `bin/disjuntor.py:2127`, `bin/disjuntor.py:198`.)
- **Campo/retorno:** `None` de `_censo_nega` significa “porta passa”, tanto FORA quanto APROVADO; `main_censo` distingue FORA pelo bool do mesmo `_pertence_fila1`, portanto não interpreta `None` como DENTRO. (`bin/disjuntor.py:1696`, `bin/disjuntor.py:1704`, `bin/disjuntor.py:1723`, `bin/disjuntor.py:2267`, `bin/disjuntor.py:2271`.)
- **Resíduo explícito fora dessa cura:** o escritor declara escritores externos que não pegam sua trava: edição manual, `gerar_cortes.py` e `deploy_agendado.sh`. O alcance desses caminhos não foi auditado por este agente. (`app/core/esteira_vigia.py:838`.) A lista usada pelo censo contém somente andáveis; a situação de obra PARADA continua dependente desse recorte. (`bin/disjuntor.py:1637`.)

Autoridade registrada para esta pergunta específica em outro lugar e efeitos atuais em produção: [nao verificado].

## 09/11 08:47 · sessao fb09f4e39450 · - · - · origem=Ronald (tmux)

Os três novos elementos têm mordida por leitura do código; execução integral do selo permanece [nao verificado].

- **Laço Agent:** a fixture remove `O9 (C)` e mantém `O9` sozinho antes dos casos (`bin/tests/test_censo_antes_da_vaga.sh:576`, `:580`). Os prompts preservam as identidades `O9 (C)` e `O9 NOTURNO-X`, pela cauda e pela identidade de id nu (`bin/disjuntor.py:164`, `:616`, `:624`). Os casos exigem JSON `deny`, `DISJUNTOR CENSO` e PAREI da obra completa (`bin/tests/test_censo_antes_da_vaga.sh:520`), além de última negativa correspondente e ausência de FORA (`:524`); saída vazia reprova (`:522`). O laço executa essas verificações nas duas obras (`:588`–`:593`).

- **Por que mata a ausência do espelho:** sem o segundo sentido, essas obras não aparecem dentro da célula `O9`, logo `nomeiam` e `exatas` ficam vazios e a função retorna `False` (`bin/disjuntor.py:1685`–`:1688`). `_censo_nega` então retorna `None` antes da validação da forma (`:1704`–`:1706`), deixando de fornecer a negativa exigida pelo caso Agent (`bin/tests/test_censo_antes_da_vaga.sh:590`). Com o espelho, há nomeação sem correspondência exata, e a função levanta `ValueError`, convertido em negativa NAO SEI (`bin/disjuntor.py:1686`, `:1691`, `:1730`).

- **Wrapper:** são controles sem poder de distinguir a cura, expressamente declarados assim no comentário e no nome do caso (`bin/tests/test_censo_antes_da_vaga.sh:585`, `:593`). `main_censo` recusa espaço antes da leitura da fila e da pertença (`bin/disjuntor.py:2248`, `:2266`). Não há controle ocultamente apresentado como mordida nova nesses dois casos (`bin/tests/test_censo_antes_da_vaga.sh:585`–`:586`).

- **ESPELHO direto:** chama o predicado efetivamente importado do diretório `$B`, exige `ValueError` nos três pares e reprova qualquer retorno, inclusive `False` (`bin/tests/test_censo_antes_da_vaga.sh:598`–`:608`). Os controles `O1/O142` e inverso exigem exatamente `False`; um predicado que sempre levante também reprova (`:609`–`:619`). A chamada direta cobre `OBRA-Q (A)`, que o Agent normalizaria para `OBRA-Q` (`bin/disjuntor.py:624`–`:627`; justificativa no selo `:596`).

- **Mutante:** remove exatamente o segundo sentido e roda `casos_pertenca` (`bin/tests/test_censo_antes_da_vaga.sh:763`). A infraestrutura exige âncora única (`:675`–`:678`), exige retorno não zero dos casos (`:681`–`:686`) e roda previamente uma cópia sem mutação nos mesmos grupos (`:662`–`:665`). `casos_pertenca` acumula `ok=1` e retorna esse valor, de modo que suas falhas chegam ao julgador do mutante (`:590`, `:619`, `:647`).

Confirmei apenas em memória, com `python3 -B`, o resultado do predicado vivo e dessa substituição: os três pares espelho dão `ValueError` no vivo e `False` no mutante; os dois controles de prefixo dão `False` em ambos. Esses resultados são explicados por `bin/disjuntor.py:1685`–`:1692` e pela substituição de `bin/tests/test_censo_antes_da_vaga.sh:763`; a suite completa e o seu resultado final são [nao verificado].

Limite: a asserção direta aceita qualquer `ValueError`, sem conferir sua mensagem (`bin/tests/test_censo_antes_da_vaga.sh:606`). Na implementação lida, para essas entradas não vazias, o `ValueError` explícito é o de pertença ambígua (`bin/disjuntor.py:1681`, `:1691`); não encontrei vacuidade concreta nesse trecho.

## 09/11 08:47 · sessao 1b91c105ad23 · - · - · origem=Ronald (tmux)

Rastreio das três funções, incluindo arquivos fora do diff:

- **`_pertence_fila1`**: definição única em `bin/disjuntor.py:1669`. Leitores produtivos encontrados: `_censo_nega`, em `bin/disjuntor.py:1704`, e `main_censo`, em `bin/disjuntor.py:2267`. Ambos interpretam `False` como FORA/passagem e exceção como NAO SEI/negativa; `True` leva ao censo (`bin/disjuntor.py:1704-1731`, `bin/disjuntor.py:2267-2281`).
- **`_token_da_celula`**: definição única em `bin/disjuntor.py:1651`; leitores produtivos encontrados somente nas listas `exatas` e `nomeiam` de `_pertence_fila1`, em `bin/disjuntor.py:1685-1686`. Retorna string ou `None`; a nomeação não aceita id vazio, portanto o `None` não casa pelo espelho (`bin/disjuntor.py:1660-1666`, `bin/hook_stop_fila1.py:115-117`).
- **`_casa_id_inteiro`**: definição única em `bin/hook_stop_fila1.py:110`. Leitores diretos encontrados: `_pertence_fila1`, em `bin/disjuntor.py:1686`, e `_parei_do_em_curso`, em `bin/hook_stop_fila1.py:1301` e `bin/hook_stop_fila1.py:1313`. Todos consomem bool; a implementação usa caixa normalizada e fronteira `[A-Z0-9-]`, inclusive permitindo underscore como separador (`bin/hook_stop_fila1.py:115-117`). A fronteira de extração da célula usa a mesma classe ASCII (`bin/disjuntor.py:1664`).

**Fora do diff:** o handoff não lê diretamente essas três funções: pede item em curso ao hook, e usa seus bools como bools, sem a antiga confusão bool/lista (`bin/handoff_sessao.sh:64`, `bin/handoff_sessao.sh:71`, `bin/handoff_sessao.sh:120-123`). Para PAREI censo, pede o reconhecedor compartilhado e distingue `None`, `''` e motivo (`bin/handoff_sessao.sh:129-134`). O alarme também não lê diretamente as três funções; pede `prova_parei_censo` e só considera provado o retorno `''` (`bin/alarme_sessao_ociosa.py:127-140`).

**Classe presente fora do diff: derivação paralela da fila.** O alarme possui `_fila` próprio: procura `## OBRAS`, conta bullets/linhas e exclui `☑`, `FEITO` ou `FECHAD` em qualquer posição (`bin/alarme_sessao_ociosa.py:89-116`). O juiz compartilhado do hook delimita por marcador e usa `_item_parado`, que também exclui CONGELADA, FILA 2, PAREI e outras marcas (`bin/hook_stop_fila1.py:95-97`, `bin/hook_stop_fila1.py:171-180`, `bin/hook_stop_fila1.py:231-252`). Assim, são respostas potencialmente divergentes à contagem da fila; o alarme usa sua resposta para decidir o disparo (`bin/alarme_sessao_ociosa.py:40-45`, `bin/alarme_sessao_ociosa.py:148-150`). Isso é herdado e não constitui outro caminho de autorização da vaga do censo (`bin/alarme_sessao_ociosa.py:144-164`).

**Igualdades fora do predicado existem, mas respondem outra pergunta:** o hook compara célula crua com o alvo declarado no marcador para localizar o item em curso (`bin/hook_stop_fila1.py:209-227`) e remove esse item por igualdade da lista de andáveis (`bin/hook_stop_fila1.py:270-273`). Não convém afirmar genericamente “não há igualdade obra × célula”; há essa igualdade, sem decidir a pertença usada pelo censo (`bin/disjuntor.py:1704`, `bin/disjuntor.py:2267`).

Leitores do selo encontrados: chamadas diretas a `_pertence_fila1` em `bin/tests/test_censo_antes_da_vaga.sh:605` e `bin/tests/test_censo_antes_da_vaga.sh:611`; mutações textuais das listas em `bin/tests/test_censo_antes_da_vaga.sh:750` e `bin/tests/test_censo_antes_da_vaga.sh:763`. Chamadas dinâmicas não identificáveis por referências nominais permanecem [nao verificado].

## 09/11 08:41 · sessao 880e3d56c067 · FIO-E0-V4-E3 · construtor · origem=Code

| Hunk | Alteração / escrita e leitura |
|---|---|
| [core/juizes.py:759](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/core/juizes.py:759) | Baixa pontual do dashboard; registro lido pelo selo em `core/tests/test_contract_juiz_tela.py:34`. |
| [chamados/models.py:1795](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/models.py:1795) | Método removido, lápide no sítio. Guarda contra retorno em `chamados/tests/test_e3_pendentes_app.py:285`. |
| [tarefa_do_dia.py:363](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/tarefa_do_dia.py:363) | Carga escreve `_verbos`, `_fila`, `_fila_cel`; tarefa lê em `:411` e `:422`. Fontes chamadas: `particionar_verbos(qs_vivos())['mapa']` e `qs_na_fila()`, também lidas por C17 em `chamados/juizes.py:1447` e `:1450`. |
| [tarefa_do_dia.py:418](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/tarefa_do_dia.py:418) | Escreve campos aditivos `rota/fila/verbos/fio`; leitores novos: DIFF `:120` e testes `test_e3_tarefa_portas.py:96`. | [nao verificado]
| [verbos.py:30](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/verbos.py:30) | Recebe `agora` da tarefa em `tarefa_do_dia.py:367`; conserva valor padrão dos leitores existentes, incluindo `painel.py:230` e `juizes.py:1447`. |
| [test_e3_pendentes_app.py:99](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_pendentes_app.py:99) | M1–M5 com fixtures ORM, relógio congelado e views completas; M6/M7 mantidos em `:306` e `:321`. |
| [test_esmeril2_left_join.py:19](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_esmeril2_left_join.py:19) | Migra para `consulta_do_app`: preserva ausência de fantasma e exige duas perguntas → duas pendências. |
| [test_fio_le_o_juiz.py:263](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_fio_le_o_juiz.py:263) | Migra leitura para `consulta_do_app`; preserva desaparecimento da pergunta absolvida. |
| [test_dashboard_badge.py:37](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/core/tests/test_dashboard_badge.py:37) | Migra comparação da tela para `consulta_do_app`. Seus leitores de produção continuam em `api/views.py:325`, `api/views_core.py:314`, `core/views.py:253` e `api/views.py:2562`. |
| `test_badge_disputa_pendente.py` — excluído | Sustentava unidade por disputa; substituição por pergunta em `test_e3_pendentes_app.py:176`. |
| [test_e3_tarefa_portas.py:1](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_tarefa_portas.py:1) | Quatro respostas escritas antes do aditivo; fixtures escrevem fatos, testes leem a tarefa. | [nao verificado]
| [diff_tarefa_portas.py:23](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/management/commands/diff_tarefa_portas.py:23) | Barreira SQL; comparação lê `_porta` e tarefa em `:118`. Escreve somente saída agregada em `:132`. |
| [test_e3_diff_portas.py:49](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_diff_portas.py:49) | Selo injeta escrita indireta e exige aborto antes do driver; identidade antes da raspagem em `:72`. |
| [e3_mutantes.py:15](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/e3_mutantes.py:15) | Catálogo de mutações; `aplicar_mutante`, em `:53`, remenda funções somente em memória e restaura após cada teste. | [nao verificado]

**DECISAO-ABERTA:** a interpretação “sem ações ⇒ sem porta” está explícita em `test_e3_tarefa_portas.py:10`. Medição na sombra por L-099/SONDA-SEM-GRANT e efeito na folha: **[nao verificado]**. C17 conserva os três chamadores em `fora_da_folha.py:67`, `vigia_fio.py:635` e `cadastro_realidade.py:488`.

Comandos preparados, **não executados**; RED, mortes dos mutantes e resultados da suíte: **[nao verificado]**. [nao verificado]

```bash
t() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    --settings=config.settings.ci "$@"
}
SELOS="chamados.tests.test_e3_pendentes_app chamados.tests.test_e3_tarefa_portas chamados.tests.test_e3_diff_portas"

t $SELOS core.tests.test_contract_juiz_tela \
  chamados.tests.test_esmeril2_left_join \
  chamados.tests.test_fio_le_o_juiz core.tests.test_dashboard_badge

# RED: retirar o aditivo.
E3_MUTANTE=sem_aditivo t chamados.tests.test_e3_tarefa_portas

# Um comando por mutante catalogado; esperado RED.
for m in $(PYTHONDONTWRITEBYTECODE=1 python3 -c \
  'from chamados.tests.e3_mutantes import MUTANTES; print(*MUTANTES, "legado_publico", "registro_dashboard", "quarto_c17_alias")')
do
  E3_MUTANTE="$m" t $SELOS
done

# DIFF: NÃO RODADO; universo a fornecer pela sombra.
PYTHONDONTWRITEBYTECODE=1 python3 manage.py diff_tarefa_portas \
  --colaborador ID --inicio AAAA-MM-DD --fim AAAA-MM-DD
```

Saída observada da compilação e auditoria estática dos arquivos acima:

```text
py_compile: OK nos 12 .py presentes; saída interceptada em memória.
Arquivos de bytecode escritos: 0.
Mutantes: 18 alvos textuais únicos, sintaxe compilável; sem execução.
C17: 3 chamadores diretos por AST.
Legado removido: 0 chamadas de produção por AST.
git diff --check: sem saída.
```

## 09/11 08:34 · sessao b4de1741aa55 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

**PROCEDENTE: a cura do par reconhecido está estruturada, mas identidade, leitores da espera e rollback do O330 ainda impedem afirmar o contrato universal.** `app/chamados/services/materializacao.py:84`, `app/chamados/services/explicador.py:97`, `app/chamados/services/validacao.py:190`.

1. **ATO ÚNICO — NAO PROCEDE para o par identificado; a universalidade não está atendida.**

   O par reconhecido entra no mesmo `atomic`, põe ambas em voo, desliga a guarda individual e julga paridade/lastro depois das duas chamadas. Carimbos e reconciliação vêm depois. Toda batida nova desse ramo passa pela porta; reuso pode dispensar `registrar_batida`. `app/chamados/services/materializacao.py:1315`, `app/chamados/services/materializacao.py:1335`, `app/chamados/services/materializacao.py:1340`, `app/chamados/services/materializacao.py:1347`, `app/chamados/services/materializacao.py:919`, `app/chamados/services/materializacao.py:987`.

   **Ainda existe aplicação individual de intervalo:** `sem_par`, `par_ambiguo` e `marcos_sem_par` caem no avulso. A irmã já aplicada é explicitamente cobrada como controle desse comportamento. Não equivale a garantir ato conjunto para todo motivo de intervalo. `app/chamados/services/materializacao.py:77`, `app/chamados/services/materializacao.py:1400`, `app/chamados/services/materializacao.py:1447`, `app/chamados/tests/test_bug_par_intervalo.py:444`.

   Chamadores encontrados por AST:

   | Função chamada | Chamadores; apelido |
   |---|---|
   | `_materializar_uma` | `validar_pergunta`, import sem alias: `app/chamados/services/validacao.py:466`, chamadas `:469`, `:474`; varredura, nome local: `app/chamados/services/materializacao.py:1565`; `_plantar_o_dia`, import sem alias: `app/chamados/services/passivo_validado.py:39`, chamada `:529`. |
   | `_materializar_resposta_hora` | `_materializar_par` e `_materializar_avulsa`, nomes locais: `app/chamados/services/materializacao.py:1336`, `app/chamados/services/materializacao.py:1447`; reexport sem chamada: `app/chamados/models.py:4077`. |
   | `registrar_batida`, neste materializador | Import sem alias; ramo hora e ramo data/hora: `app/chamados/services/materializacao.py:986`, `app/chamados/services/materializacao.py:987`, `app/chamados/services/materializacao.py:1068`. |

   **FILA — outros atos de preenchimento de intervalo também admitem resultado parcial:** `veredito_celula` captura falha por escrita e só desfaz quando nenhuma ação deu certo; regularização externa continua após uma criação recusada. São decisões do dia/regularização externa, com unidades próprias, não o aceite destas duas `PerguntaDisputa`. Cura: auditar atomicidade dessas unidades separadamente; não incluí-las numa promessa universal já cumprida. `app/ponto/services/veredito_celula.py:320`, `app/ponto/services/veredito_celula.py:327`, `app/ponto/services/veredito_celula.py:342`, `app/chamados/services/regularizacao_ext.py:172`, `app/chamados/services/regularizacao_ext.py:176`, `app/chamados/services/regularizacao_ext.py:197`.

2. **TRAVAS — NAO PROCEDE a inversão entre os dois validadores descritos.**

   `_travar_ato_materializacao` trava primeiro Colaborador; depois entrega o universo à porta que adquire Disputas → Chamados → Perguntas, cada tabela por PK. Dois validadores do mesmo colaborador serializam antes de alcançar qualquer metade. `app/chamados/services/materializacao.py:95`, `app/chamados/services/materializacao.py:104`, `app/chamados/models.py:2674`, `app/chamados/models.py:2683`, `app/chamados/models.py:2696`, `app/chamados/models.py:2700`.

   Seus quatro sítios de chamada estão dentro de transação: validação, envelope comum, competência O330 e dia O330; todos usam import sem alias ou nome local. `app/chamados/services/validacao.py:428`, `app/chamados/services/validacao.py:434`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/materializacao.py:1379`, `app/chamados/services/passivo_validado.py:371`, `app/chamados/services/passivo_validado.py:432`, `app/chamados/services/passivo_validado.py:519`, `app/chamados/services/passivo_validado.py:520`.

   **FILA — prova concorrente:** R3 executa as validações sequencialmente; não exercita duas conexões disputando locks. Deadlock executado ou ausência dele: **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:174`.

3. **ESTADO — PROCEDENTE: a espera ganhou significado que vários leitores não conhecem.**

   A primeira metade fica `VALIDADA`, sem `materializada_em`, morta para C6 e fora de `respostas_sem_veredito`. A porta nova preserva essa validação e retorna `aguardando_par`. `app/chamados/models.py:3041`, `app/chamados/juizes.py:1153`, `app/chamados/models.py:1851`, `app/chamados/services/validacao.py:192`, `app/chamados/services/validacao.py:482`.

   | Leitor | Consequência |
   |---|---|
   | Explicador/modal | Diagnostica `validada_sem_gravar`, manda corrigir escala ou TI/DP e anuncia “o sistema não conseguiu gravar”, mesmo com espera legítima. **PROCEDENTE.** `app/chamados/services/explicador.py:97`, `app/templates/chamados/partials/modal_fio.html:449`. |
   | Declaração/admin/app | Considera validação sem registro uma falha e manda refazer o horário; a projeção da API reutiliza essa família de leitores. **PROCEDENTE.** `app/chamados/services/declaracao_texto.py:221`, `app/chamados/services/declaracao_texto.py:406`, `app/chamados/services/declaracao_texto.py:677`, `app/api/views.py:1119`. |
   | Vigia do carimbo/O330 | Inclui a espera em `sem_plantio_nenhum` e `resposta_enviada_sem_gravacao`, “esperado 0”; o reparador seleciona esse universo. **PROCEDENTE.** `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/services/vigia_carimbo.py:116`, `app/chamados/services/vigia_carimbo.py:140`, `app/chamados/services/passivo_validado.py:324`. |
   | Cron desse vigia | O padrão não alarma esse passivo: usa apenas `--alarme`; o contador entra com `--alarme-passivo`. **NAO PROCEDE** alarme automático padrão por essa causa. `app/config/crons.py:1751`, `app/chamados/management/commands/vigiar_carimbo_sem_registro.py:73`. |
   | Placar | A metade sai do crédito de perguntas vivas; conta como validação humana, sem exigir aplicação. **NAO PROCEDE** chamar essa contagem de materialização. `app/chamados/services/raiox.py:72`, `app/chamados/services/verbos.py:34`, `app/chamados/services/painel.py:268`. |
   | Auditor I18 | Não a acusa como sem dono, pois exige `recusa_motivo=''`. **NAO PROCEDE** essa falsa orfandade específica. `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`. |

   **PROCEDENTE — fechamento contingente à irmã:** a espera não bloqueia C7 por si. Se a irmã sair do conjunto vivo/sem-veredito, `pode_fechar()` pode autorizar fechamento com a primeira ainda sem aplicação; há fechadores automáticos que consomem esse resultado. Cura: representar a pendência do ato no juiz de fechamento e testar retirada da irmã entre aceites. `app/chamados/models.py:1851`, `app/chamados/models.py:1861`, `app/chamados/services/becos.py:56`.

   `vigia_fio` incorpora classes do vigia do carimbo; `o_que_segura` delega à porta alcançável. Reabertura ou impacto quantitativo dessas esperas em produção: **[nao verificado]**. `app/chamados/services/vigia_fio.py:463`, `app/chamados/services/vigia_fio.py:485`, `app/folha/fora_da_folha.py:65`, `app/folha/fora_da_folha.py:221`.

   **Cura comum:** os leitores acima devem consumir um estado canônico de espera antes de diagnosticar falha, preservando separadamente a dívida real de gravação. `app/chamados/services/materializacao.py:496`.

4. **IDENTIDADE — PROCEDENTE: contar uma saída e uma volta não prova a mesma pausa.**

   O identificador consulta disputa/data/motivo e compara o mapa global, que hoje fornece sempre `S/hii` e `E/hfi`. Não consulta os slots reais do dia. Assim, uma saída remanescente da primeira pausa e uma volta da segunda passam pela identificação. As duas pausas compartilham os mesmos motivos. Cura mínima: abster quando houver múltiplas pausas reais sem identidade suficiente; solução completa: vincular os slots pela autoridade existente de marcos. `app/chamados/services/materializacao.py:70`, `app/chamados/services/materializacao.py:84`, `app/chamados/juizes.py:885`, `app/ponto/marcos.py:35`.

   `MARCO_DA_PERGUNTA`, `PARES_PAUSA_DNA`, `instancia_do_motivo` e a leitura das lâmpadas respondem perguntas diferentes; não são outro agrupador equivalente de perguntas. Não precisam chamar o agrupador novo. A divergência é ele afirmar identidade sem consumir a distinção real entre pausas. `app/chamados/juizes.py:882`, `app/escala/models.py:1652`, `app/chamados/services/declaracao_texto.py:42`, `app/chamados/juizes.py:917`.

   **PROCEDENTE — prova insuficiente de preservação de HEAD:** R7 conserva escala de uma pausa e cria quatro perguntas; prova ambiguidade por quantidade, não duas pausas no DNA nem o casal cruzado remanescente. Para quatro elegíveis há fallback avulso; preservação universal com duas pausas reais: **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:45`, `app/chamados/tests/test_bug_par_intervalo.py:255`, `app/chamados/services/materializacao.py:77`, `app/chamados/services/materializacao.py:1400`.

5. **TEXTO — parcialmente atendido.**

   **NAO PROCEDE** espera por substring: existe chave exata, reconhecida antes das regras textuais, mantendo retorno triplo. Os consumidores desempacotam esse contrato sem enumeração fechada de códigos. `app/chamados/catalogo/recusa.py:25`, `app/chamados/catalogo/recusa.py:95`, `app/chamados/services/acoes_disputa.py:34`, `app/chamados/services/fio.py:703`, `app/chamados/services/validacao.py:548`.

   **NAO PROCEDE** “colada” por vizinha distante no novo ramo: ele exige mesmo colaborador/tipo, não retratada, dentro de ±60 segundos inclusivos. **PROCEDENTE**, porém, a duplicação dessa regra fora da porta. Cura: extrair o predicado de leitura em `registro_batida.py` e reutilizá-lo nos dois consumidores. `app/chamados/services/materializacao.py:1328`, `app/chamados/services/materializacao.py:1333`, `app/ponto/registro_batida.py:106`.

   O prefixo de recusa conjunta conserva a causa para classificação; a casca de validação repassa a mensagem de espera sem anunciar ponto gravado. **NAO PROCEDE** quebra desses contratos. Permanecem as divergências de leitores do item 3. `app/chamados/services/materializacao.py:1362`, `app/chamados/catalogo/recusa.py:99`, `app/chamados/services/acoes_disputa.py:50`.

   O modal ganhou caixas de espera nos ramos validada/não validada. É renderizado pela casca modal e pelo retorno parcial compartilhado pelos POSTs das duas cascas; o GET do fio completo usa outro template. O cartão de detalhe continua apenas com “validada”. **PROCEDENTE** apresentação incompleta; cura: compartilhar a apresentação do estado. `app/templates/chamados/partials/modal_fio.html:229`, `app/templates/chamados/partials/modal_fio.html:243`, `app/chamados/views.py:927`, `app/chamados/services/fio.py:494`, `app/chamados/views.py:674`, `app/chamados/views.py:697`, `app/templates/chamados/partials/_pergunta_card.html:20`.

6. **ROLLBACK/E1 — atendido no caso normal; PROCEDENTE a exceção O330.**

   A falha da segunda escrita desfaz as novas batidas/carimbos antes da recusa; depois há releitura e devolução das validações, acrescentando decisões. Não há edição/apagamento de decisões anteriores nesse fluxo. **NAO PROCEDE** quebra append-only demonstrada pelo diff. `app/chamados/services/materializacao.py:1315`, `app/chamados/services/materializacao.py:1356`, `app/chamados/services/materializacao.py:1361`, `app/chamados/services/validacao.py:202`, `app/chamados/services/validacao.py:208`, `app/chamados/models.py:3943`.

   **PROCEDENTE — carimbo prévio sem lastro:** O330 admite esse estado; rollback restaura o carimbo anterior, e a devolução retorna sem limpar validação quando `materializada_em` permanece preenchido. Portanto não garante “zero materializada_em e ambas devolvidas” nesse universo. Cura: tratar explicitamente o descarimbo sem lastro pela porta dona antes da devolução, preservando E1. R4 não cobre essa combinação. `app/chamados/services/passivo_validado.py:531`, `app/chamados/services/materializacao.py:73`, `app/chamados/services/validacao.py:190`, `app/chamados/models.py:3203`, `app/chamados/tests/test_bug_par_intervalo.py:182`, `app/chamados/tests/test_bug_par_intervalo.py:390`.

7. **CLASSES/PROVAS — não tomar GREEN ou RED agregado como demonstração completa.**

   **NAO PROCEDE** segundo escritor novo de batida/carimbo fora das portas nesse ramo: criação, vínculo e carimbo usam as portas existentes. **PROCEDENTE** derivação paralela de proximidade e identidade insuficiente, conforme itens 4–5. `app/chamados/services/materializacao.py:987`, `app/chamados/services/materializacao.py:1008`, `app/chamados/services/materializacao.py:1256`.

   **FILA — relógio:** o helper extraído lê `now()` por metade, e a aprovação lê outro instante. A expressão já existia no corpo anterior fornecido; regressão temporal concreta: **[nao verificado]**. Cura sugerida: receber o instante da borda do ato. `app/chamados/services/materializacao.py:1223`, `app/chamados/services/materializacao.py:1348`, `app/chamados/services/materializacao.py:993`.

   Os mutantes substituem função real no módulo e executam teste-alvo; morte efetiva por asserção de M1–M18: **[nao verificado]**. M1/M10 miram rollback/devolução; M2/M3 espera/guarda individual; M4–M6 julgamento final; M7/M13 ambiguidade/contagem; M8 proximidade; M9 mapa; M11–M18 reconciliação, contagem, voo, travas e universos. `app/chamados/tests/mutantes_bug_par_intervalo.py:18`, `app/chamados/tests/mutantes_bug_par_intervalo.py:43`, `app/chamados/tests/mutantes_bug_par_intervalo.py:53`, `app/chamados/tests/mutantes_bug_par_intervalo.py:127`.

   **PROCEDENTE:** M9 só altera artificialmente o mapa; não morde identidade real de duas pausas. **FILA:** M15/M18 conferem argumentos de trava, não concorrência SQL. R9 contém controles de comportamento legado que podem falhar na base apenas pelo import novo; isso não demonstra necessidade funcional da cura. `app/chamados/tests/test_bug_par_intervalo.py:463`, `app/chamados/tests/test_bug_par_intervalo.py:332`, `app/chamados/tests/test_bug_par_intervalo.py:361`, `app/chamados/tests/test_bug_par_intervalo.py:419`, `app/chamados/tests/test_bug_par_intervalo.py:446`.

8. **ESCRITORES/LEITORES E SEMÂNTICA — inventário central, complementando os sítios acima.**

   | Campo/fonte | Quem escreve | Quem lê |
   |---|---|---|
   | `validada_em/por` | Validação/devolução: `app/chamados/services/validacao.py:134`, `:202`; nova resposta: `app/chamados/services/disputa_emissao.py:1477`; carimbo/ressuscitação: `app/chamados/models.py:3135`, `:1565`. | C6/C7/estado: `app/chamados/juizes.py:1153`, `app/chamados/models.py:1851`, `:3041`; declaração, explicador, vigias e placares: item 3; projeção E1: `app/chamados/models.py:3924`; reconciliação: `app/chamados/reconciliador.py:629`, `:1049`. |
   | `materializada_em/por`, `via_resolucao` | Porta interna e descarimbo em lote: `app/chamados/models.py:3099`, `:3266`; entradas `carimbar`, identidade, descarimbar: `:3103`, `:3168`, `:3203`. | Identificador/seletor: `app/chamados/services/materializacao.py:73`, `:1560`; fato/C6: `app/chamados/juizes.py:660`, `:1164`; relavra: `app/chamados/services/relavra_via.py:181`, `:244`; placar/veredito: `app/chamados/services/painel.py:67`, `app/chamados/veredito_lavrado.py:188`. |
   | `recusa_motivo`, `pendencia_admin` | Ramo hora/recusa/limpeza conjunta: `app/chamados/services/materializacao.py:910`, `:1163`, `:1352`; recusa humana: `app/chamados/services/validacao.py:604`; escape: `app/chamados/services/disputa_emissao.py:1494`; identidade: `app/chamados/models.py:3188`. | Devolução/catálogo: `app/chamados/services/validacao.py:193`, `app/chamados/catalogo/recusa.py:95`; explicador: `app/chamados/services/explicador.py:85`; auditorias: `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; flip: `app/ponto/services/flip_auto.py:132`; apresentação residual: `app/templates/chamados/partials/_card_atendimento.html:186`. |
   | `resposta_colab`, `respondida_em` | Resposta: `app/chamados/services/disputa_emissao.py:1464`; auto-resposta por carimbo: `app/chamados/models.py:3130`. | Prontidão/derivação: `app/chamados/services/materializacao.py:1384`, `:410`; C6/C7: `app/chamados/juizes.py:1169`, `app/chamados/models.py:1852`; E1: `app/chamados/models.py:3931`. |
   | Identidade, motivo, dia, destino, conversa | Abertura factual/conversa: `app/chamados/models.py:2784`, `:2809`, `:3016`. | Identificador/travas: `app/chamados/services/materializacao.py:67`, `:99`; derivação: `:393`; fechamento/alcance: `app/chamados/models.py:1850`, `app/chamados/juizes.py:948`. |
   | `contexto_json.marcos_faltantes` | Nascimento/reconstrução/renovação: `app/chamados/reconciliador.py:707`, `app/ponto/services/cartorio.py:1742`, `app/ponto/management/commands/reconciliar_grade.py:473`; porta de renovação: `app/chamados/services/contexto.py:102`. | Casamento/plantio: `app/chamados/juizes.py:534`, `app/chamados/services/materializacao.py:327`, `:463`; emissão: `app/chamados/services/disputa_emissao.py:1140`; lastro/grade: `app/ponto/services/lastro.py:75`, `app/escala/utils.py:1066`; regeneração: `app/escala/services/regeneracao.py:149`. |
   | Batida: `timestamp`, tipo, origem, aprovação, vínculo | Criação: `app/ponto/registro_batida.py:157`; anotação: `:254`; vínculo: `app/chamados/services/materializacao.py:1008`. | Proximidade/paridade: `app/ponto/registro_batida.py:107`, `app/chamados/services/materializacao.py:148`; lastro: `app/chamados/juizes.py:267`; competência: `app/ponto/janelas.py:84`. |

   **PROCEDENTE** a divergência semântica de `validada_em`: aceite pendente no escritor novo, terminalidade/falha em leitores antigos. **FILA** a fronteira de competência: O330 agrupa pelo dia do turno, enquanto a porta verifica o dia civil local do timestamp; são cálculos distintos já presentes, sem contraprova produtiva neste dado. A derivação overnight continua centralizada e aware; não encontrei nova conversão de fuso nesse diff. `app/chamados/services/materializacao.py:1402`, `app/chamados/juizes.py:1153`, `app/chamados/services/passivo_validado.py:113`, `app/ponto/janelas.py:84`, `app/chamados/services/materializacao.py:468`, `app/chamados/services/materializacao.py:475`.

   Exaustividade de **todos** os leitores/escritores de cada atributo incidental transitivamente alcançado pelo diff, além deste inventário: **[nao verificado]**. Execução independente de RED/GREEN, população completa e resultados dos mutantes: **[nao verificado]**.

## 09/11 08:31 · sessao 0c3a374ff426 · - · - · origem=Ronald (tmux)

Achados:

- **PROCEDENTE — explicador contradiz a espera.** A primeira metade torna-se `VALIDADA` (`app/chamados/models.py:3041`); sem batida, `explicar_pergunta` a classifica `validada_sem_gravar`, manda corrigir cadastro/escala ou TI/DP, sem verificar a espera (`app/chamados/services/explicador.py:97`, `app/chamados/services/explicador.py:103`). Com batidas no entorno, atribui intervalo deslocado ao cadastro (`app/chamados/services/explicador.py:47`, `app/chamados/services/explicador.py:58`). Esse explicador é incluído no fio administrativo (`app/chamados/services/fio.py:302`, `app/chamados/services/fio.py:312`) e o modal mostra “o sistema não conseguiu gravar”, com dono cadastro/TI (`app/templates/chamados/partials/modal_fio.html:449`). **Cura:** explicador consumir o estado canônico `aguardando_par`, antes de diagnosticar falha.

- **PROCEDENTE — declaração/app trata a espera como falha.** `_resposta_validada` lê somente `validada_em` (`app/chamados/services/declaracao_texto.py:221`). As portas de declaração devolvem `validada_sem_registro` e mandam refazer o horário pela supervisão (`app/chamados/services/declaracao_texto.py:406`, `app/chamados/services/declaracao_texto.py:677`); a consulta usada pela apresentação replica essa guarda (`app/chamados/services/declaracao_texto.py:272`). Os cards e o formulário continuam anunciando falha/refação (`app/templates/chamados/partials/_card_atendimento.html:215`, `app/templates/chamados/partials/_card_atendimento.html:290`, `app/templates/chamados/partials/modal_fio.html:146`). **Cura:** distinguir explicitamente espera legítima da validação sem gravação defeituosa.

- **PROCEDENTE — proteção de fechamento é contingente à irmã.** A metade validada fica morta para C6 (`app/chamados/juizes.py:1153`), sai de `q_vivas` (`app/chamados/models.py:3359`) e de `respostas_sem_veredito` (`app/chamados/models.py:1851`). `pode_fechar` não exige aplicação das metades validadas (`app/chamados/models.py:1861`). Portanto, se a irmã for retirada/absolvida/trancada e não restar outra pergunta viva/sem-veredito, a disputa pode fechar com aquela metade ainda pendente; o fecho automático usa esse juiz (`app/chamados/services/disputa_emissao.py:1960`, `app/chamados/services/becos.py:56`). No caso normal da fixture, a irmã continua viva e impede fechar (`app/chamados/tests/test_bug_par_intervalo.py:115`). **Cura:** representar a pendência do ato no juiz de fechamento e testar retirada da irmã entre os dois aceites.

- **PROCEDENTE — espera infla dívida de gravação.** `vigia_carimbo.classificar` seleciona todas as horas validadas, inclusive espera (`app/chamados/services/vigia_carimbo.py:97`), e classifica a metade sem batida como `sem_plantio_nenhum` (`app/chamados/services/vigia_carimbo.py:116`); o censo a soma em `resposta_enviada_sem_gravacao`, rotulado “esperado 0” (`app/chamados/services/vigia_carimbo.py:129`, `app/chamados/services/vigia_carimbo.py:140`). O reparador seleciona esse universo (`app/chamados/services/passivo_validado.py:324`). **Cura:** classe explícita de espera, preservando separadamente o contador de falhas reais.

- **NÃO PROCEDE — alarme de passivo no cron padrão.** O command só acrescenta esse contador ao alarme com `--alarme-passivo` (`app/chamados/management/commands/vigiar_carimbo_sem_registro.py:73`); o cron usa apenas `--alarme` (`app/config/crons.py:1751`). A combinação dos dois argumentos pode alarmar a espera legítima (`app/chamados/management/commands/vigiar_carimbo_sem_registro.py:77`).

- **NÃO PROCEDE — materializada no estado comum.** Sem `materializada_em`, o estado é `VALIDADA`, não `RESOLVIDA` (`app/chamados/models.py:3043`); `pergunta_sem_acao_possivel` devolve False (`app/chamados/services/raiox.py:173`). A espera fica fora do crédito de perguntas vivas, pois este lê `q_vivas` (`app/chamados/services/raiox.py:72`). Badge e verbos seguem esse crédito (`app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`); a irmã determina a ação remanescente.

- **NÃO PROCEDE — devolução automática da espera pela porta nova.** A devolução exclui exatamente `ESPERA_PAR_INTERVALO` (`app/chamados/services/validacao.py:192`); a validação retorna antes da varredura/fecho (`app/chamados/services/validacao.py:482`, `app/chamados/services/validacao.py:489`).

- **FILA — impacto efetivo em `vigia_fio`/`o_que_segura`.** `vigia_fio.fio_do_dia` delega ao C6 (`app/chamados/services/vigia_fio.py:165`); seus dias mortos e decisões pendentes recebem classes de `vigia_carimbo` (`app/chamados/services/vigia_fio.py:463`, `app/chamados/services/vigia_fio.py:485`). `o_que_segura` lê `porta_alcancavel_do_dia` e relata o papel/fila dessa porta (`app/folha/fora_da_folha.py:65`, `app/folha/fora_da_folha.py:221`). A ocorrência numérica em produção e reabertura por esses leitores permanecem **[nao verificado]**.

Inventário de escritores dos três campos de PerguntaDisputa, por código executável:

| Campo | Escritores |
|---|---|
| `validada_em` | `_carimbar_validacao` (`app/chamados/services/validacao.py:134`); devolução (`app/chamados/services/validacao.py:202`); resposta nova (`app/chamados/services/disputa_emissao.py:1477`); `carimbar` (`app/chamados/models.py:3135`); ressuscitação em lote (`app/chamados/models.py:1565`); restauração apenas em memória (`app/chamados/models.py:3164`). |
| `materializada_em` | `_escrever_carimbo` (`app/chamados/models.py:3099`), chamado pelas portas `carimbar`/identidade; descarimbo em lote (`app/chamados/models.py:3266`). `regularizacao_ext.py:198` escreve campo homônimo de outro modelo. |
| `recusa_motivo` | `_materializar_resposta_hora` (`app/chamados/services/materializacao.py:910`); `_carimbar_recusa` (`app/chamados/services/materializacao.py:1163`); limpeza do par (`app/chamados/services/materializacao.py:1352`); recusa humana (`app/chamados/services/validacao.py:604`); resposta escape (`app/chamados/services/disputa_emissao.py:1494`); `carimbar_identidade` (`app/chamados/models.py:3188`), também usado pela deduplicação (`app/chamados/services/fio.py:1351`). |

Outros leitores relevantes fora do diff:

- `validada_em`: inscrições/âncoras (`app/chamados/models.py:310`, `app/chamados/models.py:344`); C7 (`app/chamados/models.py:1851`); projeção E1 (`app/chamados/models.py:3924`); irmãs sem consumo (`app/chamados/juizes.py:1769`); reconciliação de órfãs (`app/chamados/reconciliador.py:629`, `app/chamados/reconciliador.py:1049`); resposta/correção (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/acoes_disputa.py:345`); contagem de validações da equipe (`app/chamados/services/verbos.py:240`, `app/chamados/services/painel.py:268`); procedência de batida (`app/ponto/services/procedencia_batida.py:74`); auditoria (`app/chamados/management/commands/auditar_invariantes_chamados.py:1075`).
- `materializada_em`: juiz do fato (`app/chamados/juizes.py:660`, `app/chamados/juizes.py:687`); C6 (`app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1227`); reutilização (`app/chamados/juizes.py:1245`); leitor da ação possível (`app/chamados/services/raiox.py:173`); marco do placar (`app/chamados/services/painel.py:67`); marco do veredito (`app/chamados/veredito_lavrado.py:188`); abertura de perguntas (`app/chamados/services/disputa_emissao.py:228`, `app/chamados/services/disputa_emissao.py:1067`, `app/chamados/services/disputa_emissao.py:1323`); reconciliação (`app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:1500`); desfazedor de carimbo (`app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:244`).
- `recusa_motivo`: devolução (`app/chamados/services/validacao.py:193`); explicador (`app/chamados/services/explicador.py:90`); fila de flip (`app/ponto/services/flip_auto.py:132`); auditorias de ausência de dono (`app/chamados/management/commands/auditar_invariantes_chamados.py:1075`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`); apresentação crua residual no card (`app/templates/chamados/partials/_card_atendimento.html:186`).

A espera não perde o registro de validação E1: `_carimbar_validacao` grava projeção e decisão na mesma transação, usando o mesmo instante (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:136`, `app/chamados/services/validacao.py:139`). A contagem da equipe aumenta pelo aceite mesmo sem aplicação, coerente com contar validações (`app/chamados/services/verbos.py:240`); chamá-la de aplicação seria **[nao verificado]**.

## 09/11 08:31 · sessao 1c264a102439 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura:

- **PROCEDENTE — identidade não prova “mesma pausa”.** `identificar_par_intervalo` agrupa apenas disputa/data/motivo e consulta o mapa global; não lê DNA do dia, hora do marco, destino nem `instancia_marco`. Com o mapa atual, a comparação canônica sempre recebe `S/hii + E/hfi`. Uma saída sobrevivente da primeira pausa e uma volta sobrevivente da segunda passam como par. `app/chamados/services/materializacao.py:70`; `app/chamados/services/materializacao.py:84`; `app/chamados/juizes.py:885`. As duas pausas efetivamente compartilham motivos no emissor: `app/ponto/marcos.py:35`. Cura: identificar os slots reais pela autoridade de marcos, ou abster quando o DNA tiver múltiplas pausas sem identidade suficiente; não inventar índice.

- **PROCEDENTE — a contraprova R7 não cobre duas pausas reais.** O teste cria quatro perguntas, mas conserva a escala de uma pausa e testa a ambiguidade por contagem. Não cobre duas perguntas remanescentes de pausas distintas nem duas pausas no DNA. `app/chamados/tests/test_bug_par_intervalo.py:45`; `app/chamados/tests/test_bug_par_intervalo.py:255`. A preservação universal de HEAD com duas pausas é **[nao verificado]**; para quatro perguntas elegíveis o código cai no avulso. `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:1400`.

- **PROCEDENTE — duplicação do predicado de proximidade.** O par repete consulta e tolerância ±60s inclusivos da porta. `app/chamados/services/materializacao.py:1328`; `app/ponto/registro_batida.py:106`. Cura: extrair consulta/predicado de leitura no dono `registro_batida.py`, consumido pela porta e pelo par. A mensagem “colada” tem testemunha concreta nesse novo caminho, filtrada por mesmo colaborador/tipo e não retratada. `app/chamados/services/materializacao.py:1329`; `app/chamados/services/materializacao.py:1333`. O catálogo isoladamente apenas reconhece substring, sem medir batida. `app/chamados/catalogo/recusa.py:39`; `app/chamados/catalogo/recusa.py:99`.

- **PROCEDENTE — leitor do app interpreta espera como defeito.** `declarar_hora_colab` e `aplicar_declaracao` consideram qualquer `validada_em` uma `validada_sem_registro` quando o marco falta e mandam a supervisão refazer o horário, sem distinguir a nova espera legítima. `app/chamados/services/declaracao_texto.py:210`; `app/chamados/services/declaracao_texto.py:372`; `app/chamados/services/declaracao_texto.py:406`; `app/chamados/services/declaracao_texto.py:662`; `app/chamados/services/declaracao_texto.py:677`. Cura: consultar estado de aplicação/espera antes desse retorno e devolver “falta a outra metade”, preservando a declaração.

- **NAO PROCEDE — espera depende de substring.** Há chave exata e retorno triplo compatível com `traduzir_recusa`; reconhecimento ocorre antes das regras por substring. `app/chamados/catalogo/recusa.py:25`; `app/chamados/catalogo/recusa.py:95`. Recusa conjunta preserva os marcadores de classificação porque apenas prefixa a causa. `app/chamados/services/materializacao.py:1362`; `app/chamados/catalogo/recusa.py:99`. Chamadores desempacotam três valores sem enumeração fechada: `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:172`; `app/chamados/services/fio.py:703`; `app/chamados/services/passivo_validado.py:144`; `app/chamados/services/validacao.py:548`.

- **NAO PROCEDE — casca de validação anuncia ponto gravado para espera.** Estado diferente de `validada` e `ok=True` usa a mensagem do serviço. `app/chamados/services/acoes_disputa.py:50`; `app/chamados/services/acoes_disputa.py:52`; `app/chamados/services/validacao.py:485`. `aplicar_declaracao` também repassa estado/mensagem do serviço na primeira aplicação. `app/chamados/services/declaracao_texto.py:690`.

- **PROCEDENTE — apresentação não alcança todas as telas.** O modal acrescentou caixas de espera nos ramos validada e não validada. `app/templates/chamados/partials/modal_fio.html:229`; `app/templates/chamados/partials/modal_fio.html:243`. O cartão da página de detalhe só mostra “validada” e não lê `estado_aplicacao`. `app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/detalhe_local.html:310`. Cura: compartilhar apresentação do estado de aplicação. O modal é renderizado diretamente pela casca modal e nos retornos parciais usados pelas duas cascas; a página completa do fio usa outro template, sem incluir o modal. `app/chamados/views.py:927`; `app/chamados/services/fio.py:494`; `app/chamados/views.py:674`; `app/chamados/views.py:697`. Há outro render do modal no retorno de recobrança. `app/chamados/views.py:1399`.

- **FILA — relógio fora da borda.** `_carimbar_aplicacao`, extração agora compartilhada por duas metades, lê relógio por chamada; aprovação da batida lê outro instante. Não existe instante único recebido pelo ato. `app/chamados/services/materializacao.py:1223`; `app/chamados/services/materializacao.py:1348`; `app/chamados/services/materializacao.py:993`. É extração de comportamento anterior no diff fornecido, não prova de regressão temporal. Cura sugerida: instante recebido da borda e repassado às duas escritas/carimbos. Efeito em produção **[nao verificado]**.

Mutantes e RED:

- **NAO PROCEDE quanto a mutação apenas de fonte fictícia:** o executor recompila a função real no dicionário global do módulo e chama o teste selecionado; verifica âncora única. `app/chamados/tests/mutantes_bug_par_intervalo.py:76`; `app/chamados/tests/mutantes_bug_par_intervalo.py:127`. Execução/morte efetiva M1–M18: **[nao verificado]**.
- **FILA — R3 não prova ausência de deadlock:** faz duas validações sequenciais. `app/chamados/tests/test_bug_par_intervalo.py:174`. M15 e M18 verificam os IDs passados a `trava_na_ordem`, sem duas conexões concorrentes nem observação de locks SQL. `app/chamados/tests/test_bug_par_intervalo.py:332`; `app/chamados/tests/test_bug_par_intervalo.py:361`. Cura: teste concorrente de duas transações reais.
- M1/M10 apontam ao rollback/devolução; M3 à guarda individual; M4 à paridade final; M5 à ordem temporal; M6 ao lastro; M8 à proximidade; M11 à reconciliação; M12 à contagem; M14 ao voo; M16 ao universo; M17 ao filtro de carimbo; M7/M13 à ambiguidade/contagem; M9 à tabela global. Correspondências estão declaradas no executor. `app/chamados/tests/mutantes_bug_par_intervalo.py:18`; `app/chamados/tests/mutantes_bug_par_intervalo.py:30`; `app/chamados/tests/mutantes_bug_par_intervalo.py:43`; `app/chamados/tests/mutantes_bug_par_intervalo.py:53`; `app/chamados/tests/mutantes_bug_par_intervalo.py:60`.
- **PROCEDENTE — M9 não morde identidade real de pausa:** só altera `MARCO_DA_PERGUNTA` artificialmente; não testa DNA com duas pausas/casal cruzado. `app/chamados/tests/test_bug_par_intervalo.py:463`.
- **FILA — RED por símbolo ausente não demonstra cura funcional:** controles R9 de ausência/irmã já aplicada exigem a função nova por import, embora seus resultados preservem o caminho avulso; a base pode errar no import antes de testar o comportamento. `app/chamados/tests/test_bug_par_intervalo.py:419`; `app/chamados/tests/test_bug_par_intervalo.py:431`; `app/chamados/tests/test_bug_par_intervalo.py:446`. R7 também exige a chave nova de contagem depois de conferir a recusa legada. `app/chamados/tests/test_bug_par_intervalo.py:261`; `app/chamados/tests/test_bug_par_intervalo.py:270`. R1/R2/R3, por sua vez, exigem explicitamente espera e aplicação das duas; não são meros controles de HEAD. `app/chamados/tests/test_bug_par_intervalo.py:151`; `app/chamados/tests/test_bug_par_intervalo.py:164`; `app/chamados/tests/test_bug_par_intervalo.py:174`.

Fontes paralelas existentes, com perguntas diferentes: mapa motivo→marco `app/chamados/juizes.py:882`; classificação marco→motivo `app/ponto/marcos.py:14`; posição→motivo/instância `app/chamados/services/declaracao_texto.py:42`; pares canônicos de DNA `app/escala/models.py:1652`; leitura de todas as luzes das pausas `app/chamados/juizes.py:917`; defesa que se abstém com duas pausas `app/chamados/juizes.py:1124`. Não são outro agrupador de perguntas equivalente a `identificar_par_intervalo`; exigirem chamar esse agrupador seria inverter dependências. A divergência concreta é o agrupador novo afirmar identidade sem usar os slots que essas autoridades distinguem. `app/chamados/services/materializacao.py:84`.

## 09/11 08:31 · sessao f854c2b1485a · - · - · origem=Ronald (tmux)

**ATO/TRAVAS — NAO PROCEDE para par identificado; universalidade exige ressalvas.**

- Chamadores AST de `_materializar_uma`: `validar_pergunta`, import sem alias, `app/chamados/services/validacao.py:466`, chamadas `:469`/`:474`; varredura, nome local, `app/chamados/services/materializacao.py:1565`; `_plantar_o_dia`, import sem alias `app/chamados/services/passivo_validado.py:39`, chamada `:529`.
- Chamadores AST de `_materializar_resposta_hora`: somente `_materializar_par`, nome local, `app/chamados/services/materializacao.py:1336`, e `_materializar_avulsa`, nome local, `:1447`. Existe reexport, sem chamada, em `app/chamados/models.py:4077`.
- Par identificado: ambas entram em voo antes da primeira escrita; mesmo savepoint; ramo individual ignora guarda intermediária; guarda final, lastro, carimbos e reconciliação vêm depois das duas chamadas. `app/chamados/services/materializacao.py:1315`, `:1317`, `:1336`, `:935`, `:1338`, `:1345`, `:1348`, `:1355`.
- Toda **nova criação** desse ramo usa `registrar_batida`, import sem alias; porém reuso consulta batida e pula a chamada à porta, portanto “ambas sempre chamam registrar” literalmente não é verdadeiro. `app/chamados/services/materializacao.py:919`, `:925`, `:931`, `:986`, `:987`; `app/chamados/juizes.py:235`.
- Continua havendo aplicação avulsa de pergunta de intervalo quando identidade retorna `sem_par`, `par_ambiguo` ou `marcos_sem_par`; pode criar só uma metade. Isso preserva a rota individual para esse universo. `app/chamados/services/materializacao.py:77`, `:79`, `:85`, `:1400`, `:1447`. O teste de irmã já aplicada exige esse comportamento. `app/chamados/tests/test_bug_par_intervalo.py:444`.

**TRAVAS — NAO PROCEDE para inversão entre esses dois validadores.**

- Chamadores AST de `_travar_ato_materializacao`, todos sem alias: `validacao.py:434` (import `:433`), `materializacao.py:1379` (local), `passivo_validado.py:432` (import `:431`) e `:520` (import `:513`). Transações anteriores: `validacao.py:428`, `materializacao.py:1378`, `passivo_validado.py:371`/`:519`.
- O helper pega Colaborador antes de enumerar o universo; depois trava Disputas, Chamados e Perguntas. A porta ordena cada grupo por PK. `app/chamados/services/materializacao.py:95`, `:97`, `:104`; `app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`.
- Mesmo colaborador serializa os dois validadores antes da primeira Pergunta; validar saída/volta em ordem inversa não cria a inversão de travas descrita. `app/chamados/services/validacao.py:434`, `:439`; `app/chamados/services/materializacao.py:95`. R3 é sequencial; concorrência executada **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:174`.

**ROLLBACK — NAO PROCEDE no caso normal; PROCEDENTE na promessa universal para O330.**

- Falha da segunda escrita sai do savepoint antes da recusa; releitura recupera estado anterior; recusa usa `encerrar_colisao=False`; devolução limpa validação e acrescenta decisão. Novas batidas/carimbos do par são revertidos pela estrutura transacional. `app/chamados/services/materializacao.py:1315`, `:1356`, `:1361`, `:1364`; `app/chamados/services/validacao.py:198`, `:202`, `:205`, `:208`.
- **Contraexemplo O330:** o reparador inclui perguntas com `materializada_em` anterior sem lastro; rollback restaura esse carimbo; `devolver_validacao_recusada` retorna antes de limpar validação se `materializada_em` não é nulo. Portanto não garante “zero materializada_em e devolução das duas” nesse universo. Cura sugerida: tratamento explícito pelo dono de descarimbo para carimbo sem lastro, antes da devolução, preservando E1; ou delimitar formalmente o contrato como preservação do estado anterior. `app/chamados/services/passivo_validado.py:531`; `app/chamados/services/materializacao.py:73`, `:74`, `:1361`, `:1365`; `app/chamados/services/validacao.py:188`, `:190`; porta de descarimbo `app/chamados/models.py:3203`.
- E1 do fluxo novo acrescenta `VALIDACAO`/`DEVOLUCAO`; não atualiza/apaga decisão anterior. A projeção legada é capturada antes da troca. `app/chamados/services/validacao.py:133`, `:139`, `:201`, `:208`. Modelo recusa edição/apagamento e porta usa `bulk_create`. `app/chamados/models.py:3903`, `:3908`, `:3943`.
- R4 verifica falha da segunda escrita no validador normal; não cobre esse rollback de O330 com carimbo prévio. `app/chamados/tests/test_bug_par_intervalo.py:182`, `:194`; fixtures O330 com carimbos falsos só verificam sucesso/exclusão de universo. `app/chamados/tests/test_bug_par_intervalo.py:382`, `:390`, `:403`.

**OUTROS CAMINHOS — FILA, por serem atos diferentes de aplicar respostas desse par.**

- `veredito_celula` pode preencher marcos de intervalo pela porta e depois fechar perguntas do dia; simula o conjunto antes, mas captura erro por escrita e só dá rollback quando nenhuma ação teve sucesso. Falha posterior pode conservar a primeira batida. É decisão do dia, sem chamar `_materializar_resposta_hora`/identificador de perguntas. Cura da unidade desse outro ato fica fora da correção estreita, mas impede afirmar atomicidade universal de todo preenchimento de intervalo. `app/ponto/services/veredito_celula.py:290`, `:305`, `:320`, `:322`, `:327`, `:342`, `:346`, `:179`, `:189`.
- Regularização externa também tem ramo próprio de par de intervalo; `_cria_batida` transforma recusa em retorno `None`, permitindo prosseguir com a irmã e materializar o lote. Não aplica `PerguntaDisputa`. `app/chamados/services/regularizacao_ext.py:77`, `:83`, `:88`, `:121`, `:167`, `:172`, `:176`, `:197`.

## 09/11 08:26 · sessao 2170a5030b34 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

_3088 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
