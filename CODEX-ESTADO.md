# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 13:52** · publicado em **09/11/2026 13:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 13:52 · sessao 3b20bcb6caff · SONDA-SEM-GRANT · leitura · origem=Code

**A matriz está ABERTA.** Existe juiz único da conferência de catálogo, mas falta uma transição completa com contrato único de prontidão, falha e idempotência. A porta confirma alterações em duas transações e publica a credencial depois delas. (`bin/sonda_leitor_comum.sh:158`; `bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:168`; `bin/sonda_provisionar.sh:196`)

Referências abreviadas abaixo são **arquivo:linha**, com esta expansão:

| Sigla | Arquivo |
|---|---|
| C | `bin/sonda_leitor_comum.sh` |
| P | `bin/sonda_provisionar.sh` |
| L | `bin/sonda_leitura.sh` |
| T | `bin/tests/test_sonda_sem_grant.sh` |
| G | `bin/tests/test_sonda_sem_grant_pg.sh` |
| S | `bin/sombra.sh` |

Execuções, RED/GREEN, mortes efetivas dos mutantes e estado atual de PROD: **[nao verificado]**. A coluna PROVA identifica casos e asserções existentes; não homologa os relatos dos anexos.

**1. Estados e fechamento**

O estado deve ser tratado como vetor: destino × existência/atributos do leitor × credencial × ACL vigente × defaults × autenticação × fase da transição. Esses componentes são consultados ou alterados separadamente no vivo. (`C:26`; `C:79`; `C:132`; `C:191`; `P:105`; `P:127`; `P:193`)

| Dimensão | Estados encontrados/contemplados no vivo |
|---|---|
| Destino | PROD=`saas_db/saas_hasner`; sombra=`sombra_db/sombra`; credencial compartilhada. (`C:22`; `C:28`; `C:29`) |
| Role | Ausente; sem LOGIN; super/poderes; membro; dono; configuração global ou por banco divergente. (`C:80`; `C:87`; `C:146`; `C:147`) |
| Credencial | Ausente; sem senha; modo diferente de 600; formato antigo com HOST; senha divergente; senha conhecida apenas pelo cluster. (`L:173`; `L:178`; `L:194`; `P:79`) |
| Relações | Leitura faltante; ACL aberta em tabela/view/partição/foreign table, coluna ou sequência; GRANT OPTION. (`C:97`; `C:112`; `C:122`) |
| Fora de relação | LO nomeados; SECURITY DEFINER não-sistema; CONNECT em outros bancos; TEMP/CREATE; CREATE/GRANT OPTION em schema. (`C:126`; `C:129`; `C:136`; `C:139`; `C:143`) |
| Defaults | De `saas_user`; do leitor; de outro concedente; destinados ao leitor ou PUBLIC, nos tipos consultados `r/S/n`. (`C:105`; `C:132`; `P:133`; `P:150`) |
| Alteração pelo leitor | `read_only` global/por banco, senha e defaults próprios alterados com sucesso antes da detecção seguinte. (`G:365`; `G:376`; `G:379`) |
| Instrumento/transição | Conferência muda, incompleta ou com erro; login recusado; cluster inacessível; trava ocupada; falha após cada COMMIT, na publicação ou na trilha. (`L:185`; `C:160`; `P:69`; `P:118`; `P:168`; `P:196`; `P:55`) | [nao verificado]

**Não está FECHADA por catálogo.** Há enumeração de relações/ACL, mas funções são limitadas por nomes e `prosecdef`; defaults consultados excluem tipos diferentes de `r/S/n`. A leitura de tabelas/sequências usa `information_schema`, sem prova fornecida de visibilidade integral do executor real. (`C:98`; `C:102`; `C:112`; `C:132`; `C:136`; `C:139`)

| Prova necessária de fechamento | Situação |
|---|---|
| Inventário completo dos objetos, ACL, defaults e capacidades, incluindo o complemento das consultas atuais | **FALTA**; fronteiras atuais em `C:112`, `C:132`, `C:136`, `C:139`. |
| Cada item inventariado classificado como permitido, recusado ou fora do contrato, sem resto sem classificação | **FALTA**; lista LO nominal e defaults filtrados. (`C:19`; `C:134`) |
| Cobertura de combinações, concorrência e sessões anteriores | **FALTA**; conferir, autenticar e executar são etapas separadas; CONNECT é revogado sem operação de encerramento de sessões. (`L:187`; `L:194`; `L:233`; `P:117`) |

**2. Matriz estado × role**

Convenções com expansão contratual:

| Símbolo | Significado existente |
|---|---|
| **J(k)** | `_q_conferencia` mede a chave; `_conferencia_pronta` decide. Porta e leitura delegam ao mesmo predicado. (`C:76`; `C:158`; `P:173`; `L:187`) |
| **A** | `_login_leitor` mede autenticação/sessão; a comparação `leitor\|on` está repetida nos dois chamadores. (`C:191`; `P:187`; `L:196`) |
| **E+T** | Em sucesso, promoção de `.env_leitor` e tentativa de trilha da porta; em falha, vale a matriz de fases adiante. (`P:193`; `P:196`; `P:60`) |
| **R(k)** | Leitura recusa rc **5**, nomeando a falta e a porta; registra trilha após instalação do trap. (`L:126`; `L:129`; `L:187`) |
| **✓** | Porta rc **0**, mensagem `pronto`, após conferência, login e publicação; leitura executa a sonda e devolve seu rc, remapeando 5→1. (`P:173`; `P:187`; `P:199`; `L:233`; `L:244`) |
| **F** | Porta rc **1**, sem anúncio regular de pronto, por falha SQL, conferência ou login. (`P:93`; `P:174`; `P:184`) |
| **N/A** | Dimensão inaplicável: PUBLIC é representado como grantee `0`, não como a role de LOGIN examinada. (`C:74`; `C:149`) |

“Role” abaixo identifica o titular afetado ou concedente. `saas_user` também é o executor administrativo fixo; seus próprios atributos/capacidades não são objeto do juiz atual. (`C:10`; `C:109`; `P:51`; `L:166`)

| Estado | Role | JUIZ → OPERAÇÃO | PERSISTIDO | RETORNO | PROVA / mutante |
|---|---|---|---|---|---|
| Role ausente | leitor | J(role) → CREATE/ALTER. (`C:80`; `P:107`) | Role, senha/configuração; E+T. (`P:108`; `P:109`) | R(role); porta ✓ se completar. | Recusa falsa `T:139`; criação `G:222`. Mutante isolante da ausência: **FALTA**. |
| Role ausente | saas_user | **FALTA** bootstrap/juiz; conexão já exige esse usuário. (`P:51`; `L:166`) | Sem restauração prevista desse executor. (`P:107`) | Falha SQL F / conferência R. (`P:93`; `L:185`) | **FALTA**. |
| Role ausente | PUBLIC | N/A: grantee `0`. (`C:74`) | N/A. | N/A. | N/A. |
| Presente, senha desconhecida/divergente | leitor | A; porta usa senha do arquivo ou gera outra e altera role. (`C:191`; `P:77`; `P:79`; `P:108`) | Senha do destino; E+T. | R(login); porta ✓/F. (`L:194`; `P:184`) | Divergência real `G:287`; ignorar login morderia `G:289`. Caso “role existente + arquivo ausente”: **FALTA**. |
| Presente, senha desconhecida/divergente | saas_user | **FALTA** juiz da sua senha; conexão administrativa usa socket sem senha no script. (`C:10`; `P:51`) | Nenhuma rotação desse usuário prevista. (`P:108`) | Dependente da conexão administrativa. (`P:93`) | **FALTA**. |
| Presente, senha desconhecida/divergente | PUBLIC | N/A. (`C:74`) | N/A. | N/A. | N/A. |
| `.env_leitor` ausente/desatualizado/inválido | leitor | Guardas locais + A; porta normaliza arquivo ao concluir. (`L:173`; `L:179`; `P:194`) | E+T; cluster pode mudar antes do arquivo. (`P:108`; `P:196`) | R(credencial/HOST/login); ✓/F. | Ausência/HOST/sem senha `T:147`, `T:150`, `T:252`; falhas mktemp/chmod/mv: **FALTA**. |
| `.env_leitor` ausente/desatualizado/inválido | saas_user | N/A como credencial desse usuário: arquivo é consumido pelo login leitor. (`L:194`) | Arquivo não publica senha de `saas_user`. (`P:194`) | **FALTA** garantia de disponibilidade do executor. (`P:51`) | **FALTA**. |
| `.env_leitor` ausente/desatualizado/inválido | PUBLIC | N/A. (`C:74`; `C:22`) | N/A. | N/A. | N/A. |
| ACL de relação/coluna/sequência aberta | leitor | J(esc_tab/esc_seq) → REVOKE relações, depois SELECT. (`C:112`; `C:122`; `P:129`; `P:165`) | ACL; E+T. Revogação explícita de ACL de coluna: **FALTA** no bloco. (`P:128`) | R(chave); ✓ se zerar, senão F. (`P:173`) | Estragos `G:250`–`G:255`; mutante específico por subtipo: **FALTA**. |
| ACL de relação/coluna/sequência aberta | saas_user | **FALTA** juiz de seus privilégios; consulta filtra leitor/PUBLIC. (`C:116`; `C:125`) | Porta concede leitura ao leitor, sem contrato de preservação do executor. (`P:165`) | **FALTA** retorno sobre esse sujeito. | **FALTA**. |
| ACL de relação/coluna/sequência aberta | PUBLIC | J detecta; operação de revogação relacional de PUBLIC: **FALTA**. (`C:116`; `P:129`) | ACL aberta pode permanecer; fases anteriores já confirmadas. (`P:118`; `P:173`) | R(chave); porta F. | Só cobrança textual `T:217`; estrago PG PUBLIC relacional e mutante: **FALTA**. |
| ACL LO aberta | leitor | J(esc_lo) → REVOKE das famílias nomeadas. (`C:136`; `P:156`) | `pg_proc.proacl`; E+T. | R(esc_lo); ✓/F. | Tentativas `G:327`–`G:329`; M1 atingiria primeiro `G:223`. Sete famílias sem tentativa: **FALTA** frente a `C:19`. |
| ACL LO aberta | saas_user | **FALTA** juiz de capacidades/impacto; filtro não o inclui. (`C:138`) | REVOKE não o nomeia. (`P:156`) | **FALTA** garantia de acesso preservado. | **FALTA**. |
| ACL LO aberta | PUBLIC | J(esc_lo) → REVOKE EXECUTE. (`C:136`; `P:156`) | `proacl`; E+T. | R(esc_lo); ✓/F. | Estrago `G:267`; retirar chave, M4, morderia `G:242`. |
| SECURITY DEFINER aberta/nova | leitor | J(esc_func) → REVOKE não-sistema. (`C:139`; `P:157`) | `proacl`; E+T. | R(esc_func); ✓/F. | Escrita autenticada `G:336`; M2 atingiria primeiro `G:223`. |
| SECURITY DEFINER aberta/nova | saas_user | **FALTA** juiz de impacto no dono/app. (`C:142`; `P:157`) | ACL do objeto é alterada, sem prova de clientes preservados. (`P:157`) | **FALTA** retorno sobre impacto. | **FALTA**. |
| SECURITY DEFINER aberta/nova | PUBLIC | J(esc_func) → REVOKE EXECUTE. (`C:139`; `P:157`) | `proacl`; E+T. | R(esc_func); ✓/F. | Função nova `G:266`; retirar chave, M5, morderia `G:242`. Concorrência: **FALTA**. |
| CONNECT em outro banco | leitor | J(outros_bancos) → REVOKE ALL nos outros bancos conectáveis. (`C:143`; `P:117`) | `pg_database.datacl`; E+T. | R(outros_bancos); ✓/F. | Novas conexões `G:345`; M3 atingiria primeiro `G:223`. Sessão anterior: **FALTA**. |
| CONNECT em outro banco | saas_user | **FALTA** juiz de continuidade de acesso; não é sujeito da contagem. (`C:145`) | Efeito indireto via PUBLIC depende de seus privilégios reais. (`P:117`) | **FALTA** retorno de impacto. | Superuser real/acesso preservado: **[nao verificado]**. |
| CONNECT em outro banco | PUBLIC | J(outros_bancos) → REVOKE CONNECT em todos os outros `datallowconn`. (`C:143`; `P:117`) | `datacl`; E+T. | R(outros_bancos); ✓/F. | Estrago `G:268`; retirar chave, M6, morderia `G:242`. |
| TEMP/CREATE/GRANT OPTION em banco/schema | leitor | J(esc_banco/esc_schema) → REVOKE ALL; repõe CONNECT/USAGE. (`C:126`; `P:131`; `P:158`; `P:162`) | ACL banco/schema; E+T. | R(chave); ✓/F. | Estragos `G:256`–`G:258`; tentativas `G:333`–`G:335`; mutante específico: **FALTA**. |
| TEMP/CREATE/GRANT OPTION em banco/schema | saas_user | **FALTA** juiz de impacto; excluído do filtro. (`C:128`; `C:131`) | Possível perda do que dependia de PUBLIC. (`P:132`; `P:159`) | **FALTA** retorno de impacto. | **FALTA**. |
| TEMP/CREATE/GRANT OPTION em banco/schema | PUBLIC | J → revoga CREATE schema e TEMP/CREATE banco; não toda ACL grantable restante. (`C:128`; `C:131`; `P:132`; `P:159`) | ACL; eventual resíduo permanece detectável. (`P:173`) | R(chave); ✓ ou F. | TEMP `G:259`; demais variantes PUBLIC e mutantes: **FALTA**. |
| Defaults do leitor abertos | leitor | J(default_escrita/dono) → revoga defaults próprios destinados a PUBLIC. (`C:132`; `C:147`; `P:136`; `P:150`) | `pg_default_acl`; E+T. | R(chave); ✓/F. | ALTER próprio `G:379`; M7 morderia reprovisionamento `G:372`. |
| Defaults do leitor abertos | saas_user | N/A como concedente dessa célula; operação distingue os dois donos. (`P:133`; `P:136`) | Nenhuma garantia própria adicional. | N/A. | **FALTA** prova de impacto combinado. |
| Defaults do leitor abertos | PUBLIC | J detecta destinatário; porta revoga tabelas/sequências por schema e `r/S/n` globais. (`C:133`; `P:136`; `P:150`) | `pg_default_acl`; E+T. | R(default_escrita); ✓/F. | Tabela global `G:379`; sequência/schema/por-schema autenticados: **FALTA**. |
| Defaults de `saas_user` abertos | leitor | J(default_escrita/default_falta) → remove grants e repõe SELECT. (`C:105`; `C:132`; `P:133`; `P:163`) | `pg_default_acl`; E+T. | R(chave); ✓/F. | Estragos `G:261`, `G:262`, `G:265`; mutantes específicos: **FALTA**. |
| Defaults de `saas_user` abertos | saas_user | J examina seus defaults **para leitor**, não seus poderes gerais. (`C:109`; `C:133`) | Defaults alterados como concedente. (`P:133`; `P:143`; `P:163`) | ✓/F segundo prontidão do leitor. (`P:173`) | Tabela futura legível `G:230`; preservação de outros destinatários: **FALTA**. |
| Defaults de `saas_user` abertos | PUBLIC | J detecta `r/S/n`; operação para revogar esses grants de PUBLIC: **FALTA**. (`C:132`; `P:133`; `P:150`) | Default aberto pode permanecer. (`P:175`) | R(default_escrita); porta F. | Caso PG e mutante: **FALTA**. |
| Defaults de outro concedente abertos | leitor | J detecta; cura automática: **FALTA**. (`C:132`; `P:175`) | Default permanece; outras fases podem estar confirmadas. (`P:118`; `P:168`) | R(default_escrita); F. | Caso PG/mutante: **FALTA**. |
| Defaults de outro concedente abertos | saas_user | **FALTA** avaliação de dependências desse executor. (`C:133`) | Sem operação específica. (`P:133`; `P:150`) | **FALTA** retorno próprio. | **FALTA**. |
| Defaults de outro concedente abertos | PUBLIC | J detecta; cura automática: **FALTA**. (`C:133`; `P:175`) | Default permanece. | R(default_escrita); F. | Caso PG/mutante: **FALTA**. |
| Leitor altera `read_only`/senha | leitor | J(ro/ro_banco)+A → próxima leitura recusa; porta realinha. (`C:84`; `C:87`; `P:108`; `P:111`) | Alteração do leitor persiste até cura; depois E+T. (`G:365`; `G:371`) | ALTER deve dar 0 no selo; leitura seguinte R; porta ✓/F. (`G:366`; `G:368`) | `G:376`–`G:378`; mutante de **impedimento**: **FALTA**, pois selo exige aceitação. |
| Leitor altera `read_only`/senha | saas_user | N/A como titular; atua administrativamente na cura. (`P:51`; `P:108`) | Estado alterado é do leitor. (`P:109`) | N/A. | Prova de preservação do executor: **FALTA**. |
| Leitor altera `read_only`/senha | PUBLIC | N/A; default próprio para PUBLIC está na célula anterior de defaults. (`G:379`) | N/A nesta célula. | N/A. | N/A. |
| Super/poderes/membership/ownership | leitor | J; cura atributos/membros; reatribuição de propriedade: **FALTA** na porta. (`C:81`; `C:146`; `P:108`; `P:112`; `P:175`) | Atributos/membros alterados; propriedade pode permanecer. | R(chave); ✓ ou F. (`P:173`) | Membership/super `G:260`, `G:264`; dono falso `T:195`; ownership real/mutante: **FALTA**. |
| Super/poderes/membership/ownership | saas_user | **FALTA** juiz de seus atributos/heranças. (`C:149`) | Não normalizados pela porta. (`P:108`) | **FALTA** retorno próprio. | **FALTA**. |
| Super/poderes/membership/ownership | PUBLIC | N/A como role proprietária/membro examinada. (`C:146`; `C:147`) | ACL PUBLIC continua avaliada nas células próprias. (`C:116`) | N/A. | N/A. |
| Leitura/default de leitura faltante | leitor | J(usage/tab/seq/default_falta) → CONNECT, USAGE, SELECT e defaults. (`C:92`; `C:105`; `P:160`; `P:166`) | ACL/defaults; E+T. | R(chave); ✓/F. | Falta falsa `T:137`; tabela futura `G:230`; mutantes por dimensão: **FALTA**. |
| Leitura/default de leitura faltante | saas_user | Seu OID é o concedente exigido; visibilidade integral do inventário: **FALTA** prova. (`C:98`; `C:109`) | Defaults desse concedente. (`P:163`) | ✓/F segundo leitor. (`P:173`) | Fixture `G:145`; equivalência ao executor real: **[nao verificado]**. |
| Leitura/default de leitura faltante | PUBLIC | SELECT/USAGE/CONNECT simples são admitidos, sem exigência de que PUBLIC os possua. (`C:116`; `C:128`; `C:131`) | Porta não concede leitura a PUBLIC. (`P:160`; `P:165`) | Não decide presença de leitura por PUBLIC. (`C:92`) | Caso isolante: **FALTA**. |
| Conferência ilegível/erro | leitor | J rejeita prefixo/chaves; falha SQL recusa. Porta tolera erro **antes**, rejeita **depois**. (`C:160`; `C:164`; `L:185`; `P:72`; `P:173`) | Leitura: trilha; porta: alterações já confirmadas podem ficar. (`L:121`; `P:168`) | L5; P1 após conferência final. | Mudo/erro/chave ausente `T:141`, `T:143`, `T:199`; duplicata/valor contraditório: **FALTA**. |
| Conferência ilegível/erro | saas_user | Executor da consulta; diagnóstico próprio: **FALTA**. (`L:166`; `P:51`) | Trilha tenta registrar erro genérico. (`P:72`; `P:60`) | L5/P1; sem distinguir causa administrativa. (`L:185`; `P:174`) | Caso separado por causa: **FALTA**. |
| Conferência ilegível/erro | PUBLIC | A conferência inteira é recusada; sem decisão parcial confiável de suas ACL. (`C:158`; `L:187`) | ACL não é curada pela leitura. (`L:166`) | L5/P1. (`L:185`; `P:174`) | Caso específico PUBLIC: **FALTA**. |
| Cluster inacessível | leitor | PROD: consulta recusa; sombra tenta subir cluster primeiro. (`L:143`; `L:185`) | Sombra pode alterar estado Docker; porta tenta trilha. (`L:144`; `P:60`) | PROD L5; sombra pode L1; porta falha SQL P1. (`L:144`; `P:93`) | Simulação de consulta `T:143`; indisponibilidade real/mutante: **FALTA**. |
| Cluster inacessível | saas_user | Conexão administrativa não disponível; reparo: **FALTA**. (`P:51`) | Estado do banco não conferido. (`P:72`) | Falha SQL P1. (`P:93`) | **FALTA**. |
| Cluster inacessível | PUBLIC | Estado ACL não observável nessa execução. (`C:143`; `L:185`) | **[nao verificado]**. | Recusa por conferência, não por ACL medida. (`L:185`) | **FALTA**. |
| Pronto; PROD × sombra | leitor | J+A separados; destino fixo; garantia conjunta dos dois clusters: **FALTA**. (`C:28`; `C:29`; `P:173`; `P:187`) | ACL do destino + E+T; sombra só carimba depois da porta. (`P:169`; `S:340`; `S:341`) | ✓ no destino escolhido. (`P:199`) | Ambos `G:220`, `G:309`; comparação de estado 2×: **FALTA**. |
| Pronto; PROD × sombra | saas_user | **FALTA** juiz da disponibilidade/capacidade do app nos dois clusters. (`C:149`) | Defaults alterados; impacto via PUBLIC não certificado. (`P:117`; `P:163`) | Nenhuma pós-condição própria. (`P:173`) | **FALTA**. |
| Pronto; PROD × sombra | PUBLIC | J considera ACL do destino e CONNECT dos demais bancos do cluster escolhido. (`C:129`; `C:143`) | Revogações amplas, inclusive em outros bancos. (`P:117`; `P:157`; `P:159`) | ✓ não certifica clientes legítimos dependentes de PUBLIC. (`C:145`) | `postgres/template1` em `G:345`; inventário real/clientes: **[nao verificado]**. |

**3. Contrato de saída e falha**

O predicado de catálogo existente é:

```text
role = login = ro = connect = 1
todas as chaves de CONF_AUSENCIA = 0
```

Ele é definido em `C:154`–`C:168`. Além dele, a porta cobra login `leitor|on` e arquivo promovido com modo 600; esses últimos não são verificáveis apenas por catálogo. (`P:183`; `P:187`; `P:196`; `P:198`)

| Saída/fase | Garantia atual e estado que pode ficar |
|---|---|
| **rc 0** | Conferência aprovada, depois login aprovado, depois arquivo publicado. **FALTA** garantia de que o catálogo continua igual no instante do retorno, diante de concorrência entre essas etapas. (`P:172`; `P:183`; `P:196`) | [nao verificado]
| Falha antes da transação da role | Sem alterações SQL enviadas por essa execução; lock/log podem existir. Argumento/sudo: rc2; trava/senha inválida: rc1. (`P:45`; `P:47`; `P:68`; `P:70`; `P:84`) |
| Falha dentro da transação da role | BEGIN/COMMIT com `ON_ERROR_STOP`; não há COMMIT intermediário do laço de bancos. Resultado concreto após falha: **[nao verificado]**. (`P:51`; `P:105`; `P:117`; `P:118`) | [nao verificado]
| Falha após primeiro COMMIT | Role/senha/configuração/membership/CONNECT podem estar novos; ACL e arquivo podem continuar antigos. **Não há atomicidade global.** (`P:118`; `P:127`; `P:196`) | [nao verificado]
| Falha após segundo COMMIT | Role e ACL podem estar novos, arquivo ainda antigo; conferência/login recusam a publicação. (`P:168`; `P:174`; `P:184`) | [nao verificado]
| Falha depois do `mv` | Arquivo já pode ser novo: verificação final ocorre após a promoção. (`P:196`; `P:198`) |
| Falha na trilha | `pronto` pode já ter sido emitido; o trap tenta gravar depois, sem tratamento específico da falha de append. **FALTA** contrato/selo de coerência mensagem–persistência–rc. (`P:199`; `P:55`; `P:61`) |
| Falha da porta dentro de `--refazer` | Carimbo já invalidado; falha impede publicação final. Esse caminho não mantém o verde anterior. (`S:288`; `S:265`; `S:340`; `S:341`) | [nao verificado]

**Idempotência:** `2× = mesmo estado total

[truncado em 20000 de 23724 caracteres]

## 09/11 13:48 · sessao f59a012f0887 · - · - · origem=Ronald (tmux)

| Contrato/célula | Evidência no vivo |
|---|---|
| Juiz único existente de prontidão de catálogo | `_conferencia_pronta`, `bin/sonda_leitor_comum.sh:158`, exige presenças=1/ausências=0 (`:154-168`); chamado pela porta `bin/sonda_provisionar.sh:173` e leitura `bin/sonda_leitura.sh:187`. |
| Juiz de autenticação distinto, pergunta distinta | `_login_leitor`, `bin/sonda_leitor_comum.sh:191`; porta compara `leitor\|on` em `bin/sonda_provisionar.sh:187`; leitura em `bin/sonda_leitura.sh:196`. Não é duplicação da pergunta de catálogo. |
| `saas_user` | É executor administrativo fixo (`bin/sonda_leitor_comum.sh:10`; `bin/sonda_provisionar.sh:51`; `bin/sonda_leitura.sh:166`) e concedente dos defaults (`bin/sonda_leitor_comum.sh:109`). **FALTA juiz de seus atributos, disponibilidade e capacidades**; ser superuser real: **[nao verificado]**. |
| rc 0 da porta | Conferência pós-alteração aprovada, login `leitor\|on`, envfile promovido e modo 600 verificado (`bin/sonda_provisionar.sh:172-199`). Predicado é instantâneo; não certifica estabilidade depois das consultas. |
| rc !=0 antes da transação 1 | Argumento/sudo recusam rc2 (`bin/sonda_provisionar.sh:45-47`); trava/senha inválida rc1 (`:69-84`). Sem SQL de alteração antes disso, mas lock/log podem ter sido criados (`:59-68`). |
| rc !=0 na transação 1 | Role/atributos/senha/CONNECT dos outros bancos estão num único BEGIN/COMMIT, com ON_ERROR_STOP (`bin/sonda_provisionar.sh:51,105,118`). Estado final executado: **[nao verificado]**. |
| rc !=0 depois da transação 1 | **Pode ficar parcialmente aplicado**: COMMIT role/senha/CONNECT em `bin/sonda_provisionar.sh:118`; ACL só começa `:127`; falhas de ACL, conferência/login/publicação não desfazem a primeira transação (`:169-196`). |
| rc !=0 depois da transação 2 | Role e ACL já confirmados, env velho ainda intacto até `mv` (`bin/sonda_provisionar.sh:168,174,184,196`). **Não garante rollback global**. |
| rc !=0 depois do `mv` | Env já pode ser novo: verificação de modo ocorre depois de sua promoção (`bin/sonda_provisionar.sh:196-198`). |
| Trilha falha | `_fim` tenta log sem guarda e está sob `set -e`; falha de mkdir/append pode interromper antes do `exit "$rc"` (`bin/sonda_provisionar.sh:40,55-62`). O `pronto` já pode ter sido emitido (`:199`). **FALTA contrato/selo de coerência dessa célula**. |
| `.env_leitor` ausente/sem senha | Porta gera senha (`bin/sonda_provisionar.sh:76-79`), alinha cluster (`:108`), só promove após prova (`:193-196`); leitura recusa rc5 (`bin/sonda_leitura.sh:173,179`; `_recusa :129-135`). |
| `.env_leitor` antigo/inválido | Leitura recusa modo ≠600/HOST/sem senha (`bin/sonda_leitura.sh:174-179`); porta reaproveita somente primeira linha da senha, admite alfanumérica, regrava só senha (`bin/sonda_provisionar.sh:77,83-84,194`). |
| Prod × sombra | Destinos fixos (`bin/sonda_leitor_comum.sh:28-29`), **um env compartilhado** (`:22`). Porta lê/gera uma senha (`bin/sonda_provisionar.sh:77-79`) e altera apenas o destino escolhido (`:108`). **FALTA garantia conjunta dos dois clusters após uma execução**. |
| Sombra/carimbo | `--refazer` invalida antes do drop (`bin/sombra.sh:288-289`), chama porta antes do carimbo final (`:340-344`). A própria porta não escreve stamp: sua publicação é `.env_leitor` + log (`bin/sonda_provisionar.sh:60-61,193-199`). |
| Idempotência | Mesmo segredo é reutilizado (`bin/sonda_provisionar.sh:77-79`), mas cada execução reaplica ALTER/REVOKE/GRANT e promove novo arquivo (`:108-168,193-196`), acrescentando trilha com data (`:60-61`). **2x = mesmo estado total é falso pelo log**; equivalência semântica ACL/configuração e caso de selo completo **FALTA**. |
| Conferência ilegível | Leitura recusa consulta falha e parser inválido (`bin/sonda_leitura.sh:185-187`; `bin/sonda_leitor_comum.sh:160`). Porta registra erro na conferência inicial e continua tentando provisionar (`bin/sonda_provisionar.sh:72`); pós-conferência inválida recusa rc1 (`:172-176`). |
| Cluster inacessível | Porta não trata conferência inicial inválida como parada (`bin/sonda_provisionar.sh:72`), mas escrita falha leva rc1 mascarado (`:93-97`). Leitura prod recusa consulta rc5 (`bin/sonda_leitura.sh:185`); sombra tenta subir cluster e pode sair rc1 antes (`:143-145`). |

**Lista não fechada**, pelas seguintes fronteiras do catálogo:

| Fronteira | Referência e consequência |
|---|---|
| Privilégios de relação cobrem todas as ACLs previstas, porém não todo objeto/capacidade PostgreSQL | Relações `r,p,v,m,f`, colunas e sequências não-sistema em `bin/sonda_leitor_comum.sh:112-125`; funções só LO nomeadas/SECURITY DEFINER não-sistema em `:136-141`; **FALTA enumeração das capacidades restantes**. |
| Defaults `f` ignorados | `default_escrita` só examina tipos `r,S,n` (`bin/sonda_leitor_comum.sh:132-135`); default EXECUTE de funções **FALTA célula/juiz**, embora futuras SECURITY DEFINER sejam detectadas depois de criadas (`:139-142`). |
| Inventário de leitura filtrado pelo executor | Tabelas/sequências vêm de `information_schema` (`bin/sonda_leitor_comum.sh:98-104`); comprovação de que saas_user enxerga universo inteiro **[nao verificado]**. |
| PUBLIC DML detectado, não curado | Juiz conta leitor/PUBLIC (`bin/sonda_leitor_comum.sh:112-125`); REVOKE em relações revoga só leitor (`bin/sonda_provisionar.sh:129-130`). Porta pode terminar rc1 sem pronto (`:173-176`). |
| Outros concedentes/defaults | Juiz cobre qualquer dono para `r/S/n` (`bin/sonda_leitor_comum.sh:132-135`); porta cura defaults de saas_user→leitor e leitor→PUBLIC (`bin/sonda_provisionar.sh:133-150`). **FALTA operação para demais concedentes/PUBLIC**, com recusa explícita (`:175`). |
| Membro/dono | Juiz mede membership e ownership do cluster (`bin/sonda_leitor_comum.sh:146-148`); porta revoga memberships (`bin/sonda_provisionar.sh:112`), não reatribui objetos; informa REASSIGN/REVOKE manual (`:175`). |
| Mutação concorrente e sessões anteriores | A porta só executa REVOKE CONNECT, sem encerrar sessões (`bin/sonda_provisionar.sh:117`); leitura confere, faz login e executa depois (`bin/sonda_leitura.sh:187,194,233`). **FALTA predicado de garantia contínua/caso concorrente**. |

**Separação Ronald × técnico:**

- **Decisão já estabelecida:** leitura não pode alterar role; só porta escreve (`bin/sonda_leitura.sh:28-33`); nada sem raspar/credencial no argv/prompt, escrita prod ou segundo envelope (`app/docs/CORTES.md:375`).
- **`!` operacional prod:** comentário reserva porta prod ao Ronald; único bloqueio técnico local é `SUDO_USER`, não identidade Ronald (`bin/sonda_provisionar.sh:5,45-48`). Impacto de REVOKE PUBLIC em clientes externos **[nao verificado]**; operações amplas estão em `:117,156-159`.
- **Lei já decide, sem novo corte:** um escritor com tripwire/trilha (`CLAUDE.md:32-34`), leitor sem regra própria (`:20-22`), risco medido antes (`:23-24`), lei existente antes de novo corte (`:25-27`).
- **Se quiser enfraquecer garantia para aceitar alterações próprias detectadas depois:** isso altera promessa de “impossibilidade de acidente” (`bin/sonda_leitura.sh:8-15`) e escritor único (`CLAUDE.md:32`); não é mero ajuste técnico. Existência desses ALTER próprios está no selo (`bin/tests/test_sonda_sem_grant_pg.sh:365-376`).
- **Técnico:** preencher enumeração, retornos por fase, atomicidade observável, projeção explícita de idempotência, cobertura das três identidades e mutantes específicos; não depende de inventar lei (`CLAUDE.md:20-32`).

## 09/11 13:47 · sessao 9ad35765ebd1 · - · - · origem=Ronald (tmux)

Execuções, RED/GREEN e mortes reais de mutantes: **[nao verificado]**. “Morderia” abaixo identifica asserção, sem execução.

| Estado / role | Caso do selo e mordida identificável | FALTA |
|---|---|---|
| Limpo / leitor | Dois destinos, rc0, SELECT e env600: `bin/tests/test_sonda_sem_grant_pg.sh:219`; autenticação efetiva: `:301`, `:311`. | Comparação de catálogo/env após **duas** portas: FALTA (`:219`, `:308`). |
| Role ausente / leitor | Conferência falsa `role=0`; rc5, mensagem, sem sonda, sem SQL escrito e trilha: `bin/tests/test_sonda_sem_grant.sh:127`, `:139`. | Recusa com role ausente em PG real: FALTA; primeira criação real é caso positivo (`bin/tests/test_sonda_sem_grant_pg.sh:222`). |
| Senha desconhecida/divergente / leitor | Login falso falho: `bin/tests/test_sonda_sem_grant.sh:145`; senha errada real: `bin/tests/test_sonda_sem_grant_pg.sh:285`. | Role existente + env ausente + senha desconhecida explicitamente ensaiados: FALTA. |
| Env ausente/antigo/sem senha / leitor | Recusa rc5 nomeada: `bin/tests/test_sonda_sem_grant.sh:147`, `:150`, `:252`. | Falhas de promoção `mktemp/chmod/mv` do env: FALTA nos casos da porta (`:255`, `:370`). |
| Conferência muda/erro / leitor | `FAKE_CONF=''` e rc2; rc5 e sonda impedida: `bin/tests/test_sonda_sem_grant.sh:141`, `:143`, `:127`. | Cluster indisponível real: FALTA; rc2 é simulação de consulta (`:143`). |
| Relação/coluna/sequência / leitor | Estragos de INSERT, UPDATE coluna, GRANT OPTION e USAGE: `bin/tests/test_sonda_sem_grant_pg.sh:250`; arnês exige chave positiva, rc5, cura e nova leitura: `:238`. | `nextval/setval` autenticados: FALTA na lista `:327`–`:341`. |
| Relação/coluna/sequência / PUBLIC | Consulta textual exige `e.grantee=0`: `bin/tests/test_sonda_sem_grant.sh:217`. | Estrago PG concedido a PUBLIC: FALTA na lista `bin/tests/test_sonda_sem_grant_pg.sh:250`–`:268`; casos relacionais concedem ao leitor. |
| TEMP/CREATE / leitor e PUBLIC | TEMP PUBLIC: `bin/tests/test_sonda_sem_grant_pg.sh:259`; negativas autenticadas TEMP/CREATE: `:333`–`:335`. | Negativa autenticada após retirar especificamente REVOKE PUBLIC: mutante executado [nao verificado]. |
| LO / leitor e PUBLIC | Estrago PUBLIC: `bin/tests/test_sonda_sem_grant_pg.sh:267`; tentativas autenticadas: `:327`–`:329`. | Sete famílias de `bin/sonda_leitor_comum.sh:19` sem tentativa autenticada. |
| SECURITY DEFINER / PUBLIC → leitor | Função nova: `bin/tests/test_sonda_sem_grant_pg.sh:266`; execução que escreve recusada: `:336`. | Concorrência criação após conferência: FALTA; tentativa é sequencial (`:238`, `:266`). |
| Outros bancos / PUBLIC → leitor | Estrago CONNECT PUBLIC: `bin/tests/test_sonda_sem_grant_pg.sh:268`; negativas em postgres/template1, efeito LO zero: `:345`–`:354`. | Sessões já abertas e outros bancos enumerados dinamicamente no selo: FALTA; lista é postgres/template1 (`:345`). |
| Defaults / saas_user → leitor | INSERT, SELECT grantable, schema grantable: `bin/tests/test_sonda_sem_grant_pg.sh:261`, `:262`, `:265`. | saas_user→PUBLIC e outro concedente→leitor/PUBLIC: FALTA na lista `:250`–`:268`. |
| Defaults / leitor → PUBLIC | O leitor executa ALTER DEFAULT PRIVILEGES; exige sucesso, recusa posterior e cura: `bin/tests/test_sonda_sem_grant_pg.sh:365`–`:379`. | Sequência/schema e variante por schema autenticadas: FALTA; caso é tabela global (`:379`). |
| ALTER próprio / leitor | Sucesso obrigatório do ALTER, rc5 posterior e cura: `bin/tests/test_sonda_sem_grant_pg.sh:365`–`:378`. | “Nenhuma escrita persistente pelo leitor” é incompatível com a asserção de sucesso (`:366`). |
| Login barrado / leitor | PG_hba reject; porta não anuncia pronto, env preservado por SHA, sem temporário: `bin/tests/test_sonda_sem_grant_pg.sh:275`–`:281`. | Snapshot do catálogo após falha: FALTA nessas asserções. |
| Impacto / saas_user | Role usada como administrador das fixtures: `bin/tests/test_sonda_sem_grant_pg.sh:162`; bootstrap é POSTGRES_USER: `:145`. | Prova de preservação de acesso legítimo **não-superuser**, ACL/defaults e estado de saas_user: FALTA; fixture não representa esse inventário. |

Mutantes e limites de mordida:

| Mutante | Asserção que morderia |
|---|---|
| Retirar REVOKE LO/definer/CONNECT (M1–M3) | Conferência pós-porta falha antes da credencial (`bin/sonda_provisionar.sh:172`–`:176`); selo acusa provisionamento/env (`bin/tests/test_sonda_sem_grant_pg.sh:223`–`:225`). A morte específica posterior “ESCRITA ACEITA” exige arnês adicional **[nao verificado]**. |
| Retirar chaves esc_lo/esc_func/outros_bancos de CONF_AUSENCIA (M4–M6) | Estragos deixam de ser recusados: cobrança rc5 e ausência de sonda (`bin/tests/test_sonda_sem_grant_pg.sh:242`–`:244`, `:266`–`:268`). |
| Retirar ADP leitor→PUBLIC (M7) | Reprovisionamento após default próprio precisa voltar rc0 (`bin/tests/test_sonda_sem_grant_pg.sh:372`, `:379`). |
| Reintroduzir SQL escrito na leitura | Injeta GRANT, SELECT INTO, CTE DELETE; cobra rc5, recusa falante e SQL impedido antes do psql (`bin/tests/test_sonda_sem_grant.sh:237`–`:248`). |
| Segredo em externo fora da lista/PATH absoluto | **FALTA** mordida universal; espiação enumera nomes e delega pelo PATH (`bin/tests/test_sonda_sem_grant.sh:306`–`:312`). |

Atomicidade global e idempotência de catálogo não são provadas pelos controles de env preservado: a role/CONNECT confirma em uma transação e o ACL em outra (`bin/sonda_provisionar.sh:118`, `:127`, `:168`); o selo preserva SHA do env, sem comparar o catálogo anterior (`bin/tests/test_sonda_sem_grant.sh:281`–`:300`).

## 09/11 13:41 · sessao b63b62a74363 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

**P1–P5 da triagem v3 estão corrigidos nos sítios apontados, mas restam dois PROCEDENTES funcionais: fechamento do chamado por S127 e perda do motivo de `pendencia_admin`. M38 tem mordida estrutural, sem demonstrar diferença comportamental no cenário vazio.** Referências abaixo relativas a `app/`. (`chamados/models.py:1927`; `chamados/juizes.py:2138`; `chamados/services/materializacao.py:1375`; `chamados/tests/test_bug_par_intervalo.py:724`.)

**0. P1–P5 v3 — NAO PROCEDE que continuem abertos nesses pontos.**

| Item | Cura efetiva | Asserção discriminante |
|---|---|---|
| P1: retratadas | Vizinhança usa universo apurável. `chamados/services/materializacao.py:178`; `ponto/turnos.py:2866` | Exige aplicação e sequência canônica preservando retratada. `chamados/tests/test_bug_par_intervalo.py:176` |
| P2: descarimbo vazio | Compara os três campos antes de salvar. `chamados/models.py:3218` | Zero saves e agendamentos, duas vezes. `chamados/tests/test_bug_par_intervalo.py:492` |
| P3: fechamento | Conserva modalidade original e consulta C7 após trava. `chamados/models.py:1908`; `chamados/models.py:1927` | Chama `fechar`, exige consulta após trava e estado intacto. `chamados/tests/test_bug_par_intervalo.py:517`; `chamados/tests/test_bug_par_intervalo.py:523` |
| P4: espera prematura | Exige validação própria. `chamados/services/materializacao.py:109` | Cobra predicado, declaração e validação pelo administrador. `chamados/tests/test_bug_par_intervalo.py:533`; `chamados/tests/test_bug_par_intervalo.py:550`; `chamados/tests/test_bug_par_intervalo.py:566` |
| P5: desaparecimento | Detalhe inclui C6 viva **ou espera**. `chamados/services/acoes_chamado.py:349` | GET real cobra pergunta no contexto e aviso HTML. `chamados/tests/test_bug_par_intervalo.py:636`; `chamados/tests/test_bug_par_intervalo.py:641` |

Execução independente dos testes e causa efetiva das 44 mortes informadas: **[nao verificado]**. [nao verificado]

**1. Ato único — NAO PROCEDE para par identificado; a garantia não abrange todo motivo de intervalo.**

Validação, varredura e O330 convergem em `_materializar_uma`. O par entra inteiro em voo e no mesmo savepoint; ambas as aplicações precedem julgamento temporal, vizinhança, lastro, carimbos e reconciliação. (`chamados/services/validacao.py:474`; `chamados/services/materializacao.py:1593`; `chamados/services/passivo_validado.py:532`; `chamados/services/materializacao.py:1341`; `chamados/services/materializacao.py:1358`; `chamados/services/materializacao.py:1361`; `chamados/services/materializacao.py:1378`.)

O rastreamento AST com imports, apelidos e atribuições simples encontrou os plantadores de perguntas nos ramos hora/data-hora; hora é chamada pelo par ou pela avulsa. Chamadas dinâmicas por reflexão: **[nao verificado]**. (`chamados/services/materializacao.py:1014`; `chamados/services/materializacao.py:1095`; `chamados/services/materializacao.py:1359`; `chamados/services/materializacao.py:1475`; `chamados/models.py:4091`.)

**Há plantio individual:** `sem_par`, `par_ambiguo` e `marcos_sem_par` seguem pela avulsa; o teste exige plantio da metade cuja irmã já foi aplicada. Isso preserva esse comportamento explícito, não prova equivalência irrestrita com o HEAD anterior. (`chamados/services/materializacao.py:1428`; `chamados/tests/test_bug_par_intervalo.py:940`.)

**FILA:** regularização externa e veredito da célula admitem aplicação parcial em unidades próprias; não são o aceite deste par de perguntas. (`chamados/services/regularizacao_ext.py:172`; `chamados/services/regularizacao_ext.py:176`; `ponto/services/veredito_celula.py:322`; `ponto/services/veredito_celula.py:342`.)

**2. Rollback — comportamento conferido, com limite de prova após commit.** [nao verificado]

Após desfazer o savepoint, cada pergunta é relida. O resultado depende do lastro restaurado. (`chamados/services/materializacao.py:1384`; `chamados/services/materializacao.py:1388`.)

| Condição após rollback | Estado resultante |
|---|---|
| Sem lastro, dentro do fato | `materializada_em=None`, autor `None`, via `''`; recusa conjunta gravada; devolução limpa validação/autor para resposta de hora elegível. `chamados/models.py:3222`; `chamados/services/materializacao.py:1392`; `chamados/services/validacao.py:190`; `chamados/services/validacao.py:202` |
| Com lastro | Carimbo anterior preservado; recusa conjunta ainda é registrada; materialização preenchida impede devolução. `chamados/services/materializacao.py:1388`; `chamados/services/materializacao.py:1392`; `chamados/services/validacao.py:190` |
| Fora do fato | Excluída da identificação e protegida novamente no descarimbo. `chamados/services/materializacao.py:72`; `chamados/models.py:3215` |

Descarimbo vazio agora não salva. Descarimbo efetivo salva somente os três campos: agenda beco e emite evento da célula, mas não relavra veredito nem cria diretamente auditoria; a devolução acrescenta decisão e evento quando muda o estado. (`chamados/models.py:3224`; `chamados/signals.py:225`; `chamados/signals.py:244`; `ponto/portas/celula.py:891`; `chamados/services/validacao.py:208`; `chamados/services/validacao.py:224`.)

O censo estático continua vendo o sítio `save`; ausência de execução desse save é outra medida. (`core/censo_escritas.py:364`; `chamados/models.py:3226`.)

O teste cobra primeira tentativa com quatro saves/dois eventos e segunda com zero saves e foto idêntica. Entretanto, captura callbacks com `execute=False`: idempotência dos efeitos após commit real permanece **[nao verificado]**. (`chamados/tests/test_bug_par_intervalo.py:721`; `chamados/tests/test_bug_par_intervalo.py:726`; `chamados/tests/test_bug_par_intervalo.py:738`; `chamados/tests/test_bug_par_intervalo.py:745`.)

**FILA:** descarimbo/devolução não relavram o veredito persistido; remover timestamps não garante avanço do marco de staleness. Consequência concreta neste par e perda de carimbo legítimo: **[nao verificado]**. (`chamados/models.py:3226`; `chamados/services/validacao.py:205`; `chamados/veredito_lavrado.py:187`; `chamados/veredito_lavrado.py:212`.)

**3. Travas — NAO PROCEDE inversão entre os dois validadores pelo fluxo conferido.**

Ambos começam pelo Colaborador; depois travam Disputas, Chamados e Perguntas em ordem de PK. O330 inclui previamente as disputas fechadas dos itens. **FILA:** demonstração concorrente com duas conexões, **[nao verificado]**. (`chamados/services/materializacao.py:125`; `chamados/services/materializacao.py:134`; `chamados/models.py:2683`; `chamados/models.py:2692`; `chamados/models.py:2705`; `chamados/models.py:2709`; `chamados/services/passivo_validado.py:435`.)

**4. Leitores — PROCEDENTE: S127 libera o chamado enquanto C7 retém a disputa.**

Com metade aceita em espera e irmã fora do fato, C6 considera ambas mortas; C7 retém a disputa. Porém `disputa_viva_do_chamado` só consulta vazio/C6, e `transicionar` usa essa resposta. As guardas seguintes também excluem a validada. Assim, o caminho para `fechado` pode passar; a guarda adicional de premissa vale apenas para `resolvido`. (`chamados/juizes.py:1153`; `chamados/models.py:1864`; `chamados/juizes.py:2138`; `chamados/models.py:154`; `chamados/models.py:344`; `chamados/models.py:181`; `chamados/catalogo/motor.py:100`.)

**Cura:** S127 deve consumir C7, preservando sua exceção de disputa vazia; RED pela chamada real a `transicionar`, usando o cenário da irmã retratada. (`chamados/juizes.py:2138`; `chamados/tests/test_bug_par_intervalo.py:449`.)

Os demais leitores conferidos:

- Explicador, declaração, devolução, vigia de carimbo e O330 distinguem espera de falha. (`chamados/services/explicador.py:74`; `chamados/services/declaracao_texto.py:213`; `chamados/services/validacao.py:193`; `chamados/services/vigia_carimbo.py:109`; `chamados/services/passivo_validado.py:326`.)
- Painel/badge não creditam a metade validada; a irmã determina Validar/Cobrar, ou sobra Decidir no chamado acionável. (`chamados/services/raiox.py:72`; `chamados/services/raiox.py:137`; `chamados/catalogo/natureza.py:48`; `chamados/catalogo/natureza.py:61`.)
- Fio conserva perguntas da disputa aberta; beco consulta C7. (`chamados/services/fio.py:221`; `chamados/services/fio.py:253`; `chamados/services/becos.py:56`.)
- `o_que_segura` lê a porta alcançável. Vigia de fio continua julgando vitalidade por C6 e só depois abre `porta_aguardando_par`; portanto essa classe não elimina, por si, eventual alarme de fio morto. Contracaso executado: **[nao verificado]**. (`folha/fora_da_folha.py:77`; `chamados/services/vigia_fio.py:416`; `chamados/services/vigia_fio.py:430`; `chamados/services/vigia_fio.py:448`; `chamados/services/vigia_fio.py:462`.)

**5. Identidade — NAO PROCEDE derivação paralela.**

O identificador concentra elegibilidade, multiplicidade e abstenção por duas pausas; lê pausas da autoridade existente. A tag apenas delega ao predicado de espera. `par_ambiguo` segue avulsa; o teste cobra irmãs intactas nesse cenário. (`chamados/services/materializacao.py:70`; `chamados/services/materializacao.py:77`; `chamados/services/materializacao.py:87`; `escala/models.py:1189`; `escala/models.py:1197`; `chamados/templatetags/chamados_tags.py:19`; `chamados/tests/test_bug_par_intervalo.py:329`.)

**6. Texto/tela — NAO PROCEDE nos caminhos conferidos.**

“Colada” exige testemunha viva de mesmo tipo em ±60 segundos inclusivos, pela mesma função usada na porta. Espera tem entrada própria de catálogo. Detalhe e modal incluem o mesmo partial; os testes agora cobram GET real das duas telas. Renderização executada nesta revisão: **[nao verificado]**. (`ponto/registro_batida.py:94`; `ponto/registro_batida.py:115`; `chamados/services/materializacao.py:1354`; `chamados/catalogo/recusa.py:25`; `templates/chamados/partials/_pergunta_card.html:29`; `templates/chamados/partials/modal_fio.html:160`; `chamados/tests/test_bug_par_intervalo.py:623`.)

**7. Classes estruturais e semântica.**

**PROCEDENTE — espera e limpeza destroem motivo de pendência técnica.** O ramo hora grava `pendencia_admin` junto de seu motivo e pode continuar aplicando. Depois, o par apaga qualquer motivo, deixando exatamente o estado acusado por I19. A primeira espera também pode sobrescrever motivo anterior. Ocorrência em produção: **[nao verificado]**. (`chamados/services/materializacao.py:477`; `chamados/services/materializacao.py:937`; `chamados/services/materializacao.py:1375`; `chamados/services/materializacao.py:1433`; `chamados/services/materializacao.py:1190`; `chamados/management/commands/auditar_invariantes_chamados.py:1112`.)

**Cura:** preservar o par pendência/motivo tanto ao registrar quanto ao retirar espera; não zerar pendência automaticamente. RED deve aplicar par com pendência anterior e com pendência produzida durante plantio, cobrando motivo preservado e I19 vazio. (`chamados/services/materializacao.py:937`; `chamados/services/materializacao.py:1375`; `chamados/services/materializacao.py:1433`.)

M7 agora isola multiplicidade com DNA de uma pausa; M35 cobra o HTML específico do include. **NAO PROCEDE** a vacuidade anterior desses dois. (`chamados/tests/test_bug_par_intervalo.py:307`; `chamados/tests/test_bug_par_intervalo.py:315`; `chamados/tests/test_bug_par_intervalo.py:664`.)

**PROCEDENTE como limitação de prova:** M38 morre pela chamada observada a `descarimbar`. Com os três campos vazios, a guarda antiga produz o mesmo estado/trilha após P2. Para sustentar cura funcional, falta contracaso em que a guarda antiga deixe estado errado; alternativamente, rotular esse selo como estrutural. (`chamados/tests/mutantes_bug_par_intervalo.py:134`; `chamados/tests/test_bug_par_intervalo.py:724`; `chamados/models.py:3218`.)

**FILA:** relógio continua lido por metade na aprovação e no carimbo; não há instante único propagado pelo ato. Também permanece a diferença entre competência pelo dia do turno no O330 e pelo dia civil na porta. (`chamados/services/materializacao.py:1020`; `chamados/services/materializacao.py:1250`; `chamados/services/passivo_validado.py:113`; `ponto/janelas.py:84`.)

**8. Mapa adicional de escritores/leitores.**

| Campo/valor | Escritores conferidos | Leitores conferidos |
|---|---|---|
| Resposta/instante | `chamados/services/disputa_emissao.py:1464`; `chamados/models.py:3141` | `chamados/services/materializacao.py:110`; `chamados/services/raiox.py:136` |
| Validação/autor | `chamados/services/validacao.py:134`; `chamados/services/validacao.py:202`; `chamados/services/disputa_emissao.py:1477`; `chamados/models.py:3144` | `chamados/juizes.py:1153`; `chamados/models.py:1851`; `chamados/services/painel.py:67` |
| Materialização/autor/via | `chamados/models.py:3108`; `chamados/models.py:3194`; `chamados/models.py:3222`; `chamados/models.py:3280`; `chamados/services/relavra_via.py:125` | `chamados/juizes.py:660`; `chamados/services/validacao.py:190`; `chamados/veredito_lavrado.py:187` |
| Recusa/pendência | `chamados/services/materializacao.py:937`; `chamados/services/materializacao.py:1190`; `chamados/services/materializacao.py:1375`; `chamados/services/validacao.py:605`; `chamados/services/disputa_emissao.py:1494`; `chamados/models.py:3197` | `chamados/services/materializacao.py:112`; `chamados/services/validacao.py:195`; `chamados/services/explicador.py:98`; `chamados/management/commands/auditar_invariantes_chamados.py:1112` |
| Dia/contexto/marcos | `chamados/services/fio.py:800`; `chamados/models.py:1123` | `chamados/services/materializacao.py:425`; `chamados/services/materializacao.py:428`; `chamados/services/materializacao.py:490`; `chamados/services/declaracao_texto.py:468` |
| Batida: instante/tipo/origem/vínculo/retração | `ponto/registro_batida.py:159`; `chamados/services/materializacao.py:1035`; `ponto/registro_batida.py:276` | `ponto/registro_batida.py:99`; `ponto/turnos.py:2866`; `chamados/juizes.py:734` |
| Espera/projeção | `chamados/services/materializacao.py:1433`; `chamados/services/declaracao_texto.py:485` | `chamados/models.py:1864`; `chamados/templatetags/chamados_tags.py:19`; `api/views.py:1126`; `templates/chamados/partials/_card_atendimento.html:215` |
| Lavra/trilha/eventos | `chamados/veredito_lavrado.py:250`; `chamados/services/validacao.py:208`; `ponto/portas/celula.py:891` | `chamados/veredito_lavrado.py:204`; `chamados/services/raiox.py:133`; `chamados/signals.py:244` |

Exaustividade transitiva de todos os acessos dinâmicos, suite completa, concorrência real e efeitos em produção: **[nao verificado]**. [nao verificado]

## 09/11 13:39 · sessao f2e59b370272 · - · - · origem=Ronald (tmux)

**PROCEDENTE — limpeza do par deixa pendência sem motivo.** O ramo hora pode gravar `pendencia_admin` e `recusa_motivo` juntos e continuar aplicando; após sucesso, o par apaga qualquer `recusa_motivo`, sem limpar nem resolver a pendência. O resultado satisfaz exatamente o filtro de violação I19. (`app/chamados/services/materializacao.py:468`; `:477`; `:503`; `:934`; `:937`; `:1359`; `:1374`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1111`.)

Também alcança pendência anterior: identidade e prontidão não a excluem. Além disso, a primeira metade em espera já pode sobrescrever seu motivo com `ESPERA_PAR_INTERVALO`. (`app/chamados/services/materializacao.py:67`; `:77`; `:1412`; `:1431`; `app/chamados/services/materializacao.py:1180`; `:1190`.)

**Cura sugerida:** preservar o motivo pertencente a `pendencia_admin` tanto ao registrar espera quanto ao limpar espera após aplicação; retirar apenas o marcador de espera sem apagar pendência legítima. RED: par com pendência/motivo anteriores ou produzidos durante plantio; exigir preservação do motivo e ausência de I19 após aplicar. (`app/chamados/services/materializacao.py:937`; `:1374`; `:1433`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`.)

Ocorrência em produção: **[nao verificado]**.

## 09/11 13:39 · sessao ae987bd8c0fe · SONDA-SEM-GRANT · verificador · origem=Code

**Não fecharia V7 como garantia de nenhuma escrita, nem V7b como preservação integral de ordem e origem.** O selo aceita alterações persistentes feitas pelo próprio leitor; o transporte ainda separa stdout direto de stderr processado. (`bin/tests/test_sonda_sem_grant_pg.sh:365`; `bin/tests/test_sonda_sem_grant_pg.sh:376`; `bin/codex_pergunta.sh:322`)

Resultados executados dos selos, RED/GREEN e mortes reais dos mutantes: **[nao verificado]**. Abaixo avalio código e asserções, sem usar entrega ou triagem como prova.

1. **V7a: capacidades e mordida do selo**

O selo agora constrói uma conexão TCP autenticada como `leitor`, com `ON_ERROR_STOP`, e exige leitura bem-sucedida nos dois destinos antes das negativas. Isso fecha a lacuna anterior de testar apenas como administrador. (`bin/tests/test_sonda_sem_grant_pg.sh:301`; `bin/tests/test_sonda_sem_grant_pg.sh:305`; `bin/tests/test_sonda_sem_grant_pg.sh:309`; `bin/tests/test_sonda_sem_grant_pg.sh:312`)

| Via | O que o código e o selo sustentam |
|---|---|
| Large objects | A porta revoga todas as assinaturas das **dez famílias nomeadas**. O selo tenta apenas `lo_create`, `lo_from_bytea` e `lo_creat`; faltam tentativas das outras sete famílias. (`bin/sonda_leitor_comum.sh:19`; `bin/sonda_provisionar.sh:156`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| SECURITY DEFINER | Revogação e conferência cobrem rotinas fora dos schemas de sistema. Há tentativa autenticada de função que insere. Isso não certifica funções de sistema nem qualquer função com efeito colateral. (`bin/sonda_provisionar.sh:157`; `bin/sonda_leitor_comum.sh:139`; `bin/tests/test_sonda_sem_grant_pg.sh:336`) |
| Função volátil invoker | Há uma tentativa de INSERT por função invoker. A conferência não classifica rotinas por `provolatile`; esse caso não prova ausência de outros efeitos de funções voláteis. (`bin/tests/test_sonda_sem_grant_pg.sh:177`; `bin/tests/test_sonda_sem_grant_pg.sh:337`; `bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:139`) |
| DML, TEMP e CREATE | Existem tentativas autenticadas de INSERT/UPDATE/DELETE, TEMP e criação nos dois schemas. (`bin/tests/test_sonda_sem_grant_pg.sh:330`; `bin/tests/test_sonda_sem_grant_pg.sh:333`; `bin/tests/test_sonda_sem_grant_pg.sh:335`) |
| COPY e arquivo do servidor | Há casos de COPY TO PROGRAM, COPY FROM arquivo e `pg_read_file`; não é cobertura de todas as formas de COPY. (`bin/tests/test_sonda_sem_grant_pg.sh:339`; `bin/tests/test_sonda_sem_grant_pg.sh:341`) |
| `set_config` / ALTER próprio | O caso `set_config` cobra a recusa do **INSERT posterior**, não a recusa da configuração. A seção seguinte exige sucesso de ALTER próprio e só depois detecção/cura. (`bin/tests/test_sonda_sem_grant_pg.sh:338`; `bin/tests/test_sonda_sem_grant_pg.sh:365`; `bin/tests/test_sonda_sem_grant_pg.sh:368`) |
| Outros bancos | Há teste de recusa de nova conexão em `postgres` e `template1`, nos dois clusters. O provisionamento revoga privilégios, sem encerrar sessões anteriores; existência dessas sessões: **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:345`; `bin/tests/test_sonda_sem_grant_pg.sh:350`; `bin/sonda_provisionar.sh:117`) |
| Sequências | A conferência rejeita privilégios diferentes de SELECT; há estrago administrativo concedendo USAGE. Não há tentativa autenticada de `nextval` ou `setval`. (`bin/sonda_leitor_comum.sh:122`; `bin/tests/test_sonda_sem_grant_pg.sh:255`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| NOTIFY, LISTEN, advisory lock, prepared transaction, dblink/extensões, vacuum/analyze | Não aparecem nas tentativas autenticadas. Aceitação, recusa e configuração dessas vias nos clusters fornecidos: **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:327`; `bin/tests/test_sonda_sem_grant_pg.sh:341`; `bin/tests/test_sonda_sem_grant_pg.sh:376`) |

As negativas existentes cobram rc diferente de zero, ausência de `ESCREVEU`, mensagem `permission denied` e efeito persistente zero quando há consulta de efeito. É mordida útil para esses casos, não prova das vias omitidas. (`bin/tests/test_sonda_sem_grant_pg.sh:317`; `bin/tests/test_sonda_sem_grant_pg.sh:319`; `bin/tests/test_sonda_sem_grant_pg.sh:324`)

O classificador `_sql` não fecha essas omissões: ele classifica o SQL administrativo do envelope; o Python da sonda executa separadamente, com conexão própria. (`bin/sonda_leitura.sh:158`; `bin/sonda_leitura.sh:166`; `bin/sonda_leitura.sh:233`; `bin/sonda_leitura.sh:239`)

| Mutantes relatados | Asserção identificável; morte executada **[nao verificado]** |
|---|---|
| M1–M3: retirar REVOKE LO/definer/CONNECT | A conferência da porta já deve reprovar **antes de publicar a credencial**. No selo fresco, isso atinge primeiro as cobranças de provisionamento e arquivo. Não comprova a morte específica posterior por “ESCRITA ACEITA”; seria necessário demonstrar o arnês que permite chegar ali autenticado. (`bin/sonda_provisionar.sh:173`; `bin/sonda_provisionar.sh:196`; `bin/tests/test_sonda_sem_grant_pg.sh:223`; `bin/tests/test_sonda_sem_grant_pg.sh:225`; `bin/tests/test_sonda_sem_grant_pg.sh:303`) |
| M4–M6: retirar chaves de `CONF_AUSENCIA` | Os estragos correspondentes cobram rc 5, chave positiva e ausência de `SONDA_RODOU`. (`bin/tests/test_sonda_sem_grant_pg.sh:242`; `bin/tests/test_sonda_sem_grant_pg.sh:244`; `bin/tests/test_sonda_sem_grant_pg.sh:266`) |
| M7: retirar cura global do default próprio | Há caso que concede INSERT a PUBLIC e cobra sucesso do reprovisionamento. Um mutante semanticamente válido deve atingir essa cobrança. (`bin/tests/test_sonda_sem_grant_pg.sh:372`; `bin/tests/test_sonda_sem_grant_pg.sh:379`; `bin/sonda_provisionar.sh:150`) |
| M8: stderr novamente colado no fim | A comparação da sequência e a exigência de `[stderr]` atingem essa regressão. (`bin/tests/test_codex_sonda.sh:105`; `bin/tests/test_codex_sonda.sh:107`) |

2. **Conferência: escritores, leitores e significado**

É preciso distinguir **quem produz a chave textual** de **quem modifica o estado consultado**. (`bin/sonda_leitor_comum.sh:79`; `bin/sonda_provisionar.sh:117`; `bin/sonda_provisionar.sh:156`)

| Elemento | Quem escreve/define | Quem lê |
|---|---|---|
| `LO_ESCRITA` | Definição única na biblioteca. (`bin/sonda_leitor_comum.sh:19`) | Consulta e revogação. (`bin/sonda_leitor_comum.sh:77`; `bin/sonda_provisionar.sh:156`) |
| `esc_lo` | `_q_conferencia` produz o valor; a porta revoga o ACL correspondente. (`bin/sonda_leitor_comum.sh:136`; `bin/sonda_provisionar.sh:156`) | `CONF_AUSENCIA` → `_conferencia_pronta`. (`bin/sonda_leitor_comum.sh:155`; `bin/sonda_leitor_comum.sh:161`) |
| `esc_func` | `_q_conferencia` produz o valor; a porta revoga EXECUTE. (`bin/sonda_leitor_comum.sh:139`; `bin/sonda_provisionar.sh:157`) | Mesmo parser/predicado. (`bin/sonda_leitor_comum.sh:155`; `bin/sonda_leitor_comum.sh:168`) |
| `outros_bancos` | `_q_conferencia` produz o valor; a porta revoga CONNECT de PUBLIC e privilégios do leitor. (`bin/sonda_leitor_comum.sh:143`; `bin/sonda_provisionar.sh:117`) | Mesmo parser/predicado. (`bin/sonda_leitor_comum.sh:155`; `bin/sonda_leitor_comum.sh:168`) |
| `CONF_AUSENCIA` | Uma definição. (`bin/sonda_leitor_comum.sh:155`) | `_conferencia_pronta`, exigindo cada chave presente e igual a zero. (`bin/sonda_leitor_comum.sh:161`; `bin/sonda_leitor_comum.sh:164`; `bin/sonda_leitor_comum.sh:168`) |

Envelope e porta importam a mesma biblioteca e usam a mesma consulta; a leitura exige prontidão antes da execução, e a porta exige prontidão depois das alterações. (`bin/sonda_leitura.sh:48`; `bin/sonda_leitura.sh:184`; `bin/sonda_leitura.sh:187`; `bin/sonda_provisionar.sh:42`; `bin/sonda_provisionar.sh:172`; `bin/sonda_provisionar.sh:173`)

O parser próprio localizado é `_chave`, **instrumentação do selo**; o predicado de prontidão desse selo delega à biblioteca. Não identifiquei outro julgador operacional dessa conferência. (`bin/tests/test_sonda_sem_grant_pg.sh:216`; `bin/tests/test_sonda_sem_grant_pg.sh:217`)

O significado é igual nos dois destinos: `_destino` troca cluster/banco, e as consultas usam `current_database()`. As três novas chaves contam **itens ACL**, podendo contar PUBLIC e leitor separadamente; não são números distintos de funções ou bancos. (`bin/sonda_leitor_comum.sh:28`; `bin/sonda_leitor_comum.sh:29`; `bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:143`)

A frase “só a porta escreve todo esse estado” seria excessiva: o próprio selo exige que o leitor altere senha, configurações e default privileges. (`bin/tests/test_sonda_sem_grant_pg.sh:365`; `bin/tests/test_sonda_sem_grant_pg.sh:376`)

3. **Laço noturno e estado parcialmente aplicado** [nao verificado]

Pelo censo fornecido, o laço alcança `juliani_ref_1810`, `postgres` e `template1`, excluindo `sombra`; não há outro LOGIN não-superuser informado além do leitor. Assim, **dano atual a outro login legítimo não está demonstrado**. Censo independentemente conferido nesta cópia: **[nao verificado]**. (`bin/sonda_provisionar.sh:117`)

O laço não contém exclusão de `juliani_ref_*` nem preservação de CONNECT herdado de PUBLIC para outros usuários. Portanto, em outro inventário, pode retirar acesso legítimo dependente de PUBLIC; uso de roles NOLOGIN por `SET ROLE` e clientes externos: **[nao verificado]**. (`bin/sonda_provisionar.sh:117`)

Há duas granularidades de atomicidade:

- **Dentro do laço:** DO inteiro em BEGIN/COMMIT, com `ON_ERROR_STOP`; falha não deixa metade desses REVOKEs confirmada. (`bin/sonda_provisionar.sh:51`; `bin/sonda_provisionar.sh:105`; `bin/sonda_provisionar.sh:117`; `bin/sonda_provisionar.sh:118`)
- **No provisionamento completo:** role/CONNECT confirmam antes da transação de ACL. Falha posterior pode conservar as primeiras alterações, sem promoção do envfile ou anúncio de pronto. (`bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:127`; `bin/sonda_provisionar.sh:169`; `bin/sonda_provisionar.sh:183`; `bin/sonda_provisionar.sh:196`)

O cron chama `--refazer && --bloco`. O refazer invalida o carimbo antes do drop, e erro da porta interrompe antes do carimbo final: **pode deixar a sombra vermelha e impedir o bloco, mas esse caminho não conserva o verde anterior**. (`app/config/crons.py:351`; `bin/sombra.sh:288`; `bin/sombra.sh:265`; `bin/sombra.sh:340`; `bin/sombra.sh:341`)

Conferência verde também não certifica disponibilidade de outros usuários: suas contagens verificam privilégios de leitor/PUBLIC. (`bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:143`)

4. **V7b: ordem, origem, rc e segredo**

**Ordem integral: não.** A sincronização garante que a pipeline termine antes da raspagem, mas stdout escreve diretamente enquanto stderr atravessa `tee` e `sed`. A ordem entre esses caminhos pode inverter; linhas parciais também podem se concatenar. O selo apenas intercala linhas completas com pausas de 0,3 segundo. (`bin/codex_pergunta.sh:322`; `bin/codex_pergunta.sh:333`; `bin/tests/test_codex_sonda.sh:53`; `bin/tests/test_codex_sonda.sh:54`)

**Origem integral: não.** O próprio envelope já mistura stdout/stderr do processo da sonda com `2>&1`. Ambos chegam ao wrapper como stdout; `[stderr]` identifica apenas stderr recebido na fronteira do envelope. (`bin/sonda_leitura.sh:240`; `bin/codex_pergunta.sh:322`)

**Rc do envelope: sim, nas execuções normais da pipeline.** A captura imediata de `PIPESTATUS[0]` preserva 0, 5 e demais retornos; o wrapper não usa `errexit`. Porém, ignora os retornos de `tee` e `sed`: envelope rc 0 pode continuar após falha desses estágios. (`bin/codex_pergunta.sh:54`; `bin/codex_pergunta.sh:322`; `bin/codex_pergunta.sh:326`)

**Rc 5 continua impedindo o bruto de chegar ao Codex:** sai antes da raspagem e da inclusão do dado no prompt. A sonda real que retorna 5 é remapeada para 1 pelo envelope. (`bin/codex_pergunta.sh:325`; `bin/codex_pergunta.sh:333`; `bin/codex_pergunta.sh:359`; `bin/sonda_leitura.sh:244`)

**Segredo não tem garantia geral de retenção:**

- `tee` grava stderr cru em `envelope.err`; o diretório temporário recebe modo 700 e há tentativa de limpeza no EXIT. (`bin/codex_pergunta.sh:322`; `bin/codex_pergunta.sh:273`; `bin/codex_pergunta.sh:267`)
- No rc 5, qualquer linha iniciada por `sonda_leitura:` sai **sem raspagem**. Um segredo nessa linha passaria pelo filtro; ocorrência real: **[nao verificado]**. (`bin/codex_pergunta.sh:324`)
- No rc 0, a máscara trata nomes/CPF, não senhas arbitrárias. O selo de recusa só coloca os segredos fora das linhas prefixadas. (`bin/nome_no_texto.py:87`; `bin/nome_no_texto.py:110`; `bin/tests/test_codex_sonda.sh:42`; `bin/tests/test_codex_sonda.sh:44`)

5. **As quatro decisões abertas**

| Decisão | Classificação |
|---|---|
| **1 — aceitar ALTER próprio e detectar depois** | **PROCEDENTE; bloqueia a garantia absoluta.** Conserva escritor do estado fora da porta; detecção posterior não é impedimento. O selo exige essa escrita bem-sucedida, enquanto o envelope promete impossibilidade de acidente. (`CLAUDE.md:32`; `bin/tests/test_sonda_sem_grant_pg.sh:366`; `bin/sonda_leitura.sh:8`) |
| **2 — SECURITY DEFINER criada depois** | **DESCARTE como falso pronto apenas por nascer entre provisionamento e próxima conferência:** há caso exigindo recusa. **Permanece lacuna de garantia contínua:** conferir antes de executar não impede mudança depois da conferência; essa concorrência não é ensaiada. (`bin/tests/test_sonda_sem_grant_pg.sh:242`; `bin/tests/test_sonda_sem_grant_pg.sh:266`; `bin/sonda_leitura.sh:187`; `bin/sonda_leitura.sh:233`) |
| **3 — lista LO dependente da versão** | **DESCARTE como defeito futuro já demonstrado.** Função futura omitida: **[nao verificado]**. **PROCEDENTE a cobertura atual incompleta:** sete famílias já revogadas não têm tentativa autenticada. (`bin/sonda_leitor_comum.sh:19`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| **4 — CONNECT amplo na sombra** | **DESCARTE do dano atual alegado a outro LOGIN**, pelo censo fornecido; dano real: **[nao verificado]**. **PROCEDENTE o efeito amplo e a aplicação global não atômica**, sem demonstração de falso verde nesse caminho. (`bin/sonda_provisionar.sh:117`; `bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:127`; `bin/sombra.sh:288`) |

Não identifiquei nessas quatro decisões derivação paralela ou relógio fora da borda. Os problemas demonstráveis pertencem à escrita fora da porta, cobertura insuficiente e fidelidade incompleta do instrumento; essas classes exigem tratamento, não mera aceitação como decisão aberta. (`CLAUDE.md:28`; `CLAUDE.md:32`; `CLAUDE.md:370`; `CLAUDE.md:373`)

6. **Lista final**

| Item | Veredito |
|---|---|
| “Leitor impossibilitado de qualquer escrita/efeito” | **PROCEDENTE manter aberto:** ALTER próprio é aceito pelo contrato do selo e várias vias não são medidas. (`bin/tests/test_sonda_sem_grant_pg.sh:366`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| “Cada via fechada tem tentativa e mutante específico comprovado” | **DESCARTE:** cobertura LO parcial; M1–M3 falham antes da credencial; execuções **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:223`; `bin/tests/test_sonda_sem_grant_pg.sh:303`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| “Há outro juiz operacional da conferência” | **DESCARTE no código localizado:** consumidores delegam ao predicado comum; `_chave` é instrumentação. (`bin/sonda_leitura.sh:187`; `bin/sonda_provisionar.sh:173`; `bin/tests/test_sonda_sem_grant_pg.sh:217`) |
| “Laço pode deixar metade dos bancos confirmada” | **DESCARTE dentro da mesma transação; PROCEDENTE para o provisionamento completo em duas transações.** (`bin/sonda_provisionar.sh:105`; `bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:127`) |
| “Falha da porta mantém verde anterior da sombra” | **DESCARTE nesse caminho:** invalidação precede a mudança e publicação final depende da porta. (`bin/sombra.sh:288`; `bin/sombra.sh:340`; `bin/sombra.sh:341`) |
| “V7b preserva ordem e origem de cada linha” | **PROCEDENTE a reprovação:** caminhos concorrentes e mistura anterior no envelope. (`bin/codex_pergunta.sh:322`; `bin/sonda_leitura.sh:240`) |
| “Rc 5 entrega bruto ao Codex” | **DESCARTE:** saída anterior à raspagem/prompt. (`bin/codex_pergunta.sh:325`; `bin/codex_pergunta.sh:333`) |
| “Falha de transporte ou segredo sempre fecha a passagem” | **PROCEDENTE a lacuna:** rc de filtros ignorado e linhas prefixadas emitidas cruas. Vazamento ocorrido: **[nao verificado]**. (`bin/codex_pergunta.sh:322`; `bin/codex_pergunta.sh:324`) |

## 09/11 13:37 · sessao 80205edd1ecf · - · - · origem=Ronald (tmux)

**PROCEDENTE — S127 ainda usa C6 e permite encerrar o chamado-pai com intervalo em espera.** `ChamadoColaborador.transicionar` chama `disputa_viva_do_chamado`; esse juiz só retém disputa vazia ou com `fio_vivo`, enquanto C6 mata imediatamente a pergunta validada. Com a irmã fora do fato, C7 retém a disputa, mas S127 devolve `None` e o gate seguinte de ancoradas também só procura C6 vivas. Cura: S127 consultar a condição canônica C7, preservando o caso da disputa vazia; RED chamar `transicionar` sobre o cenário da irmã retratada. `app/chamados/models.py:148`, `app/chamados/models.py:165`, `app/chamados/models.py:344`, `app/chamados/models.py:368`, `app/chamados/juizes.py:1153`, `app/chamados/juizes.py:2134`, `app/chamados/juizes.py:2138`, `app/chamados/models.py:1863`.

**NAO PROCEDE — leitores principais migram a espera antes de diagnosticar falha/devolver.** Explicador retorna `aguardando_par`; declaração retorna o mesmo estado antes da guarda de validação; devolução sai sem apagar aceite; vigia separa espera de dívida; O330 exclui espera do alvo. `app/chamados/services/explicador.py:74`, `app/chamados/services/declaracao_texto.py:213`, `app/chamados/services/declaracao_texto.py:382`, `app/chamados/services/declaracao_texto.py:678`, `app/chamados/services/validacao.py:193`, `app/chamados/services/vigia_carimbo.py:109`, `app/chamados/services/vigia_carimbo.py:133`, `app/chamados/services/passivo_validado.py:327`.

**Leitores restantes, alcance preciso.**

- C6 continua dizendo “morta”; propriedade `estado` diz `VALIDADA` enquanto não materializada; C7 acrescenta a retenção da espera. `app/chamados/juizes.py:1153`, `app/chamados/models.py:3050`, `app/chamados/models.py:1864`.
- Painel/badge creditam apenas `q_vivas`, portanto a metade aceita não conta: irmã respondida gera Validar; irmã sem resposta gera Cobrar; sem outra viva, chamado acionável vira Decidir. Isso preserva uma ação no chamado vivo, mas não produz um contador específico de espera. `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:137`, `app/chamados/services/verbos.py:34`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/catalogo/natureza.py:54`, `app/chamados/catalogo/natureza.py:61`.
- Fio carrega todas as perguntas de disputas abertas, inclusive aceita; de disputa fechada carrega só C6 vivas. Não reabre nesse carregamento. `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:224`, `app/chamados/services/fio.py:238`, `app/chamados/services/fio.py:253`.
- `o_que_segura` lê a porta alcançável e seu `fio`; não lê C7 nem espera diretamente. Vigia antigo usa C6 para fio vivo; pode classificar dia sem C6 viva como `fio_morto`, mesmo tendo `porta_aguardando_par`, se faltarem as outras saídas (resolver dia/ausência/lista admin). Não é contraprova de produção: depende desses insumos. `app/folha/fora_da_folha.py:221`, `app/folha/fora_da_folha.py:77`, `app/chamados/services/vigia_fio.py:416`, `app/chamados/services/vigia_fio.py:430`, `app/chamados/services/vigia_fio.py:448`, `app/chamados/services/vigia_fio.py:462`, `app/chamados/services/vigia_fio.py:305`.
- Beco/evento e cron respeitam C7; vigias de fio/carimbo estão declarados como vigias. `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:56`, `app/config/crons.py:2044`, `app/config/crons.py:2213`, `app/config/crons.py:2226`.

**NAO PROCEDE — duas cascas administrativas recebem o aviso.** Detalhe conserva a aceita por `C6 ou espera`; modal carrega perguntas da disputa aberta; ambos incluem o mesmo partial que chama diretamente o predicado. GETs reais estão nos testes; execução nesta revisão: [nao verificado]. `app/chamados/services/acoes_chamado.py:349`, `app/chamados/views.py:549`, `app/templates/chamados/detalhe_local.html:310`, `app/chamados/services/fio.py:253`, `app/templates/chamados/partials/modal_fio.html:160`, `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/_espera_par_intervalo.html:2`, `app/chamados/templatetags/chamados_tags.py:19`, `app/chamados/tests/test_bug_par_intervalo.py:623`.

**Mapa adicional de escrita/leitura fora do diff.**

| Campo | Escritores conferidos | Leitores conferidos |
|---|---|---|
| `resposta_colab/respondida_em` | resposta core `disputa_emissao.py:1464`; complemento de carimbo `models.py:3140`; reabertura `disputa_emissao.py:1590` | C6 `juizes.py:1169`; estado `models.py:3053`; espera `materializacao.py:110`; crédito `raiox.py:136` |
| `validada_em/por` | aceite `validacao.py:134`; devolução `validacao.py:202`; nova resposta `disputa_emissao.py:1477`; complemento `models.py:3144`; descarimbo em lote `models.py:1565` | C6 `juizes.py:1153`; C7 `models.py:1851`; estado `models.py:3050`; espera `materializacao.py:109`; vigia `vigia_carimbo.py:100`; placar `painel.py:67` |
| materialização/via | `models.py:3108`; identidade `models.py:3192`; descarimbo `models.py:3222`; lote `models.py:3280`; relavra via `relavra_via.py:125` | lastro `juizes.py:660`; C6 `juizes.py:1164`; estado `models.py:3052`; devolução `validacao.py:190`; apresentação `materializacao.py:515` |
| `recusa_motivo` | `materializacao.py:1190`; limpeza do par `materializacao.py:1375`; recusa humana `validacao.py:605`; escape `disputa_emissao.py:1494`; identidade `models.py:3197` | espera `materializacao.py:112`; devolução `validacao.py:195`; explicador `explicador.py:98`; auditor I18 `auditar_invariantes_chamados.py:1075` |
| `contexto_json/data_evento/timestamp` | contexto nascimento `models.py:1123`; batida porta `registro_batida.py:155` | data plantio `materializacao.py:425`; fallback contexto disputa `materializacao.py:428`; datetime aware `materializacao.py:495`; ajuste overnight `materializacao.py:502`; tolerância `registro_batida.py:99` |

Caminhos acima são os conferidos; exaustividade transitiva de todos os escritores/leitores da árvore: **[nao verificado]**.

## 09/11 13:37 · sessao f4d5aba2cdbb · - · - · origem=Ronald (tmux)

- **NAO PROCEDE — par identificado dividido:** validação chama `_materializar_uma`; varredura e O330 usam a mesma porta. Ambas as perguntas entram em voo antes do plantio, no mesmo `atomic`; ordem temporal, vizinhança e lastro são julgados após ambas; carimbos e reconciliação seguem dentro dele. `app/chamados/services/validacao.py:474`; `app/chamados/services/materializacao.py:1593`; `app/chamados/services/passivo_validado.py:532`; `app/chamados/services/materializacao.py:1341`; `app/chamados/services/materializacao.py:1358`; `app/chamados/services/materializacao.py:1361`; `app/chamados/services/materializacao.py:1370`.

- **Ressalva literal:** nem toda pergunta com motivo de intervalo recebe esse tratamento: `sem_par`, `par_ambiguo` e `marcos_sem_par` seguem pela avulsa. Uma metade pode ser plantada quando a irmã já está aplicada; o teste exige isso. Portanto “nenhum caminho planta metade sozinha” seria falso; “nenhum par identificado é dividido” corresponde ao código. `app/chamados/services/materializacao.py:1428`; `app/chamados/services/materializacao.py:1475`; `app/chamados/tests/test_bug_par_intervalo.py:940`.

- **NAO PROCEDE — inversão entre os dois validadores:** ambos adquirem Colaborador antes de montar/travar o universo; depois Disputas, Chamados e Perguntas, por PK. O330 trava também as disputas fechadas dos itens antes do primeiro dia. `app/chamados/services/validacao.py:434`; `app/chamados/services/materializacao.py:125`; `app/chamados/services/materializacao.py:134`; `app/chamados/models.py:2683`; `app/chamados/models.py:2692`; `app/chamados/models.py:2705`; `app/chamados/models.py:2709`; `app/chamados/services/passivo_validado.py:435`. **FILA:** execução concorrente em duas conexões [nao verificado].

- **NAO PROCEDE — identidade paralela/tag:** identidade concentra-se em `identificar_par_intervalo`; exclui fora do fato, distingue carimbo normal/reparo, recusa multiplicidade e mais de uma pausa. Lê DNA pela autoridade existente; a tag apenas delega ao predicado da espera. `app/chamados/services/materializacao.py:70`; `app/chamados/services/materializacao.py:73`; `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:87`; `app/escala/models.py:1189`; `app/escala/models.py:1197`; `app/chamados/templatetags/chamados_tags.py:18`.

- **NAO PROCEDE — escolha arbitrária em `par_ambiguo`:** encaminha à avulsa; o teste exige recusa individual, irmã intacta e ausência de batidas novas. Equivalência irrestrita com HEAD anterior [nao verificado]; o encaminhamento e as asserções são verificáveis. `app/chamados/services/materializacao.py:1428`; `app/chamados/tests/test_bug_par_intervalo.py:315`; `app/chamados/tests/test_bug_par_intervalo.py:320`; `app/chamados/tests/test_bug_par_intervalo.py:329`.

- **Censo AST com imports/apelidos/atribuições simples:** encontrou os dois plantadores de perguntas em `materializacao.py`; o de hora só é chamado pela unidade par e pela avulsa; `data_hora` também passa pela avulsa. O reexport em models não acrescenta chamador. `app/chamados/services/materializacao.py:1014`; `app/chamados/services/materializacao.py:1095`; `app/chamados/services/materializacao.py:1359`; `app/chamados/services/materializacao.py:1475`; `app/chamados/services/materializacao.py:1477`; `app/chamados/models.py:4091`. Chamadas dinâmicas por reflexão [nao verificado].

- **FILA — unidades próprias já triadas:** regularização externa pode conservar uma metade e continuar; veredito da célula acumula erros e só força rollback quando não houve ação. Não são aplicação das duas PerguntaDisputa pelo aceite. `app/chamados/services/regularizacao_ext.py:172`; `app/chamados/services/regularizacao_ext.py:176`; `app/chamados/services/regularizacao_ext.py:188`; `app/ponto/services/veredito_celula.py:320`; `app/ponto/services/veredito_celula.py:327`; `app/ponto/services/veredito_celula.py:342`.

## 09/11 13:37 · sessao f2e59b370272 · - · - · origem=Ronald (tmux)

**NAO PROCEDE — P1–P5 v3 continuam abertos:** todos têm cura na árvore e asserção discriminante: P1 usa universo apurável e cobra aplicação/alternância (`app/chamados/services/materializacao.py:178`; `app/chamados/tests/test_bug_par_intervalo.py:176`); P2 cobra zero saves/agendamentos (`app/chamados/models.py:3218`; `app/chamados/tests/test_bug_par_intervalo.py:492`); P3 consulta C7 sob trava e testa a porta real (`app/chamados/models.py:1927`; `app/chamados/tests/test_bug_par_intervalo.py:517`); P4 exige validação própria (`app/chamados/services/materializacao.py:109`; `app/chamados/tests/test_bug_par_intervalo.py:533`); P5 inclui espera no contexto e cobra GET real (`app/chamados/services/acoes_chamado.py:350`; `app/chamados/tests/test_bug_par_intervalo.py:641`).

**Rollback:** o savepoint desfaz plantios/carimbos internos; depois cada pergunta é relida. Sem lastro, `descarimbar()` limpa conjuntamente `materializada_em/por/via`; a recusa é gravada e a devolução limpa `validada_em/por`, preservando declaração e decisões anteriores. Com lastro verdadeiro, preserva carimbo anterior; com `materializada_em` preenchido, a devolução não ocorre. (`app/chamados/services/materializacao.py:1341`; `:1384`; `:1388`; `:1392`; `app/chamados/models.py:3222`; `app/chamados/services/validacao.py:190`; `:202`; `:208`.)

**NAO PROCEDE — descarimbo vazio ainda produz escrita:** os três vazios retornam antes de `save`; fora do fato também retorna. Um descarimbo efetivo salva somente os três campos; não relavra veredito nem cria diretamente `LogAuditoria`. O save agenda beco e emite evento da célula; a devolução produz decisão append-only e auditoria quando muda estado. (`app/chamados/models.py:3215`; `:3218`; `:3224`; `app/chamados/signals.py:225`; `:244`; `app/ponto/portas/celula.py:891`; `app/chamados/services/validacao.py:208`; `:224`; `:226`.)

**NAO PROCEDE — perde carimbo de linha fora do fato:** a identidade exclui essas linhas e a porta protege novamente; vias de identidade com carimbo são absolvidas pelo próprio juiz do lastro. Contracaso de via legítima reprovada e apagada: **[nao verificado]**. (`app/chamados/services/materializacao.py:72`; `app/chamados/models.py:3215`; `app/chamados/juizes.py:660`; `:670`; `:672`.)

**Selos:**

- M7 agora isola multiplicidade com DNA de uma pausa e exige ambiguidade para as quatro perguntas; não depende da segunda guarda do DNA. (`app/chamados/tests/test_bug_par_intervalo.py:307`; `:315`.)
- M35 cobra o HTML exclusivo do include, evitando aprovação por texto em nota/trilha. (`app/chamados/tests/test_bug_par_intervalo.py:664`.)
- M38 restaura a guarda antiga, mas morre pela quantidade de chamadas à porta. Após P2, pular `descarimbar()` dos três campos vazios produz o mesmo estado/trilha; portanto sua morte comprova passagem pela porta, não diferença comportamental. (`app/chamados/tests/mutantes_bug_par_intervalo.py:134`; `app/chamados/tests/test_bug_par_intervalo.py:724`; `app/chamados/models.py:3218`.)
- O teste 2x cobra quatro saves/agendamentos e dois eventos na primeira tentativa; zero saves/agenda e foto idêntica na segunda. Os callbacks são capturados com `execute=False`; seus efeitos após commit real permanecem **[nao verificado]**. (`app/chamados/tests/test_bug_par_intervalo.py:721`; `:726`; `:728`; `:738`; `:743`; `:745`.)

**FILA — relavra persistida após descarimbo:** nenhuma chamada de lavra no descarimbo/devolução; a validade da lavra compara relógio e máximos de timestamps, inclusive `materializada_em/respondida_em`. Remover carimbo não garante, sozinho, avanço desses máximos. Consequência concreta neste par: **[nao verificado]**; merece confronto com os leitores persistidos, sem atribuir ao diff prova que ele não fornece. (`app/chamados/models.py:3222`; `app/chamados/services/validacao.py:205`; `app/chamados/veredito_lavrado.py:187`; `:198`.)

## 09/11 13:36 · sessao b19adcb2e1b8 · - · - · origem=Ronald (tmux)

V7a não está certificado como “nenhuma escrita ou efeito colateral”: o selo exige explicitamente sucesso de alterações do próprio leitor e só verifica recusa na leitura posterior; isso contradiz uma promessa absoluta de impedimento. (`bin/tests/test_sonda_sem_grant_pg.sh:365`; `bin/tests/test_sonda_sem_grant_pg.sh:366`; `bin/tests/test_sonda_sem_grant_pg.sh:368`)

- A autenticação do teste é efetivamente construída como `leitor`, por TCP, com senha por stdin e `ON_ERROR_STOP`; há controle positivo de leitura nos dois bancos. (`bin/tests/test_sonda_sem_grant_pg.sh:301`; `bin/tests/test_sonda_sem_grant_pg.sh:305`; `bin/tests/test_sonda_sem_grant_pg.sh:309`; `bin/tests/test_sonda_sem_grant_pg.sh:312`)
- As negativas cobram rc, ausência de `ESCREVEU`, mensagem de privilégio e efeito persistente igual a zero quando fornecido. (`bin/tests/test_sonda_sem_grant_pg.sh:317`; `bin/tests/test_sonda_sem_grant_pg.sh:319`; `bin/tests/test_sonda_sem_grant_pg.sh:324`)
- A lista cobre somente três das dez famílias LO revogadas: `lo_create`, `lo_from_bytea`, `lo_creat`; as outras sete não têm tentativa autenticada correspondente. (`bin/sonda_leitor_comum.sh:19`; `bin/tests/test_sonda_sem_grant_pg.sh:327`; `bin/tests/test_sonda_sem_grant_pg.sh:329`)
- Existem tentativas autenticadas de DML, TEMP, CREATE, SECURITY DEFINER, invoker volátil, `set_config` seguido de INSERT, COPY PROGRAM/FROM arquivo e `pg_read_file`. (`bin/tests/test_sonda_sem_grant_pg.sh:330`; `bin/tests/test_sonda_sem_grant_pg.sh:341`)
- A lista não contém NOTIFY, LISTEN, advisory locks, prepared transaction, dblink/extensões, vacuum/analyze ou execução autenticada de `nextval`/`setval`; `USAGE` de sequência aparece como estrago aplicado pelo administrador, depois conferido. (`bin/tests/test_sonda_sem_grant_pg.sh:238`; `bin/tests/test_sonda_sem_grant_pg.sh:255`; `bin/tests/test_sonda_sem_grant_pg.sh:327`; `bin/tests/test_sonda_sem_grant_pg.sh:341`)
- Os quatro casos de alteração própria só rodam em sombra e cobram sucesso da escrita, depois detecção/cura: defaults global/por banco, senha e default privileges. (`bin/tests/test_sonda_sem_grant_pg.sh:365`; `bin/tests/test_sonda_sem_grant_pg.sh:376`; `bin/tests/test_sonda_sem_grant_pg.sh:379`)
- O classificador `_sql` protege o SQL produzido pelo envelope; o Python da sonda executa separadamente como código arbitrário. (`bin/sonda_leitura.sh:158`; `bin/sonda_leitura.sh:164`; `bin/sonda_leitura.sh:233`; `bin/sonda_leitura.sh:239`)
- A conferência de funções cobre somente os nomes LO declarados e SECURITY DEFINER de schemas não-sistema; não certifica outras rotinas voláteis com efeitos externos ou de sessão. (`bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:142`)
- O fechamento de outros bancos só emite REVOKE; não há encerramento de sessões anteriores nesse bloco. A existência e capacidade dessas sessões: [nao verificado]. (`bin/sonda_provisionar.sh:117`)

A execução concreta das capacidades omitidas e os resultados reais dos selos/mutantes: **[nao verificado]**.

| Mutante | Mordida demonstrável no código |
|---|---|
| M1/M2/M3: retirar REVOKE LO/definer/CONNECT | Morde a conferência inicial da porta e a cobrança de provisionamento, antes de existir credencial publicada. A morte específica por “ESCRITA ACEITA” não está demonstrada. (`bin/sonda_provisionar.sh:172`; `bin/sonda_provisionar.sh:176`; `bin/sonda_provisionar.sh:193`; `bin/tests/test_sonda_sem_grant_pg.sh:223`; `bin/tests/test_sonda_sem_grant_pg.sh:225`) |
| M4/M5/M6: retirar chaves de CONF_AUSENCIA | Estragos definidos para cada chave cobram rc 5, chave positiva e ausência de execução da sonda. (`bin/tests/test_sonda_sem_grant_pg.sh:242`; `bin/tests/test_sonda_sem_grant_pg.sh:244`; `bin/tests/test_sonda_sem_grant_pg.sh:266`; `bin/tests/test_sonda_sem_grant_pg.sh:268`) |
| M7: retirar cura global de default privilege próprio | O caso dá INSERT global a PUBLIC e cobra sucesso da porta de cura; retirar essa cura deve atingir essa cobrança. Execução real: [nao verificado]. (`bin/sonda_provisionar.sh:150`; `bin/tests/test_sonda_sem_grant_pg.sh:372`; `bin/tests/test_sonda_sem_grant_pg.sh:379`) |

A ressalva M1–M3 importa: a raiz temporária nasce sem `.env_leitor`; a porta só publica o arquivo depois da conferência e do login. Retirar uma revogação mantém a chave positiva e impede essa publicação. O teste autenticado posterior depende de ler a senha desse arquivo. Portanto a alegação de morte especificamente na tentativa de escrita exige evidência de um arnês adicional ou de uma execução sem essa falha inicial. (`bin/tests/test_sonda_sem_grant_pg.sh:182`; `bin/tests/test_sonda_sem_grant_pg.sh:185`; `bin/tests/test_sonda_sem_grant_pg.sh:303`; `bin/sonda_provisionar.sh:173`; `bin/sonda_provisionar.sh:196`)

Quanto às decisões abertas: aceitar escrita própria do papel como “somente leitura” conserva um segundo escritor de estado fora da porta; a regra exige escritor canônico com tripwire e o cabeçalho do envelope promete impossibilidade de acidente de escrita. Detectar posteriormente não prova impedir. (`CLAUDE.md:32`; `CLAUDE.md:34`; `bin/sonda_leitura.sh:8`; `bin/sonda_leitura.sh:31`; `bin/tests/test_sonda_sem_grant_pg.sh:366`)

A lista LO futura é limitação de cobertura, e a criação posterior de SECURITY DEFINER é alteração do universo conferido; não constituem, sozinhas, derivação paralela ou relógio fora da borda. O contrato exige, porém, cura da classe e selo que morde, e enumera explicitamente segundo escritor e selo sem mordida entre as classes de reprovação. (`bin/sonda_leitor_comum.sh:19`; `bin/sonda_leitor_comum.sh:139`; `CLAUDE.md:66`; `CLAUDE.md:370`; `CLAUDE.md:372`)

## 09/11 13:36 · sessao 298ff21ca46b · - · - · origem=Ronald (tmux)

**V7b ainda não garante ordem e origem de cada linha.** O fluxo direto de stdout e o fluxo de stderr via `tee|sed` concorrem; esperar a pipeline terminar garante conclusão antes da raspagem, sem garantir a ordem original entre os descritores. Sem newline no stdout, a marca do stderr também pode entrar no meio da mesma linha. (`bin/codex_pergunta.sh:322,333`)

Há perda anterior de origem: o próprio envelope junta stdout e stderr do `docker run` com `2>&1` antes do filtro; ambos chegam ao wrapper como stdout, sem `[stderr]`. O carimbo identifica somente stderr do envelope nessa fronteira. (`bin/sonda_leitura.sh:233,240`; `bin/codex_pergunta.sh:322`)

- **RC normal: correto quanto ao envelope.** O wrapper habilita `nounset` e `pipefail`, sem `errexit`; captura imediatamente `PIPESTATUS[0]`, preservando os retornos 0, 5 e demais falhas do envelope. (`bin/codex_pergunta.sh:54,322,323,326`)
- **Falhas do transporte: lacuna.** Os retornos de `tee` e `sed` são ignorados. Envelope com rc 0 permite continuar mesmo que outro estágio falhe e perca stderr; o retorno da atribuição substitui o resultado agregado da pipeline. (`bin/codex_pergunta.sh:322,326,333`)
- **Ramo 5 impede o bruto de chegar ao Codex.** Sai antes da raspagem, da construção do prompt e da chamada ao Codex. A limpeza tenta remover o diretório temporário no EXIT. (`bin/codex_pergunta.sh:325,333,359,423,267`)
- **Filtro de rc 5 não é raspagem.** Qualquer linha de stderr iniciada por `sonda_leitura:` é emitida crua; um segredo nessa linha passa pelo filtro textual. A ocorrência real disso está **[nao verificado]**. (`bin/codex_pergunta.sh:324`)
- **Ressalva ao cenário anterior:** o envelope real remapeia rc 5 da sonda para 1; portanto, não está demonstrado que o Python arbitrário consiga selecionar o ramo de recusa imprimindo o prefixo e saindo 5. As recusas normais são produzidas antes da execução pela função `_recusa`. (`bin/sonda_leitura.sh:129,135,243,244`)
- **`tee` mantém cópia crua local**, enquanto sua saída segue pelo `sed` para o bruto; não imprime diretamente o conteúdo no stderr do chamador. A raspagem posterior trata nomes e CPF, sem filtro geral de senhas. Senha arbitrária impressa com envelope rc 0 pode permanecer no texto raspado; vazamento ocorrido **[nao verificado]**. (`bin/codex_pergunta.sh:322,333`; `bin/raspar_publico.py:31,36`; `bin/nome_no_texto.py:87,110,121`)

O selo cobra a sequência espaçada por dois `sleep 0.3`, a presença da marca em uma linha específica e ausência da marca nos sentinelas de stdout. Não cobre rajada, linhas parciais, stderr original do processo dentro do envelope nem falha de `tee/sed`. (`bin/tests/test_codex_sonda.sh:53,54,55,104,105,107,108`; `bin/sonda_leitura.sh:240`)

O caso rc 5 cobra ausência de chamada ao Codex e ausência dos segredos sem prefixo; as duas linhas prefixadas da fixture não contêm segredo, deixando o filtro textual sem teste dessa condição. (`bin/tests/test_codex_sonda.sh:42,43,44,45,96,97,100`)

O mutante “colar stderr no fim” atingiria a comparação da ordem e a cobrança da marca. Sua execução e morte efetiva, assim como os resultados RED/GREEN relatados: **[nao verificado]**. (`bin/tests/test_codex_sonda.sh:105,106,107`)

## 09/11 13:36 · sessao 30ac7db3ccbc · - · - · origem=Ronald (tmux)

Conferência e consumidores:

- Os valores das chaves são produzidos por `_q_conferencia`; `esc_lo` conta itens ACL de leitor/PUBLIC nas famílias da lista, `esc_func` conta itens ACL em SECURITY DEFINER fora dos schemas de sistema, e `outros_bancos` conta itens CONNECT nos bancos conectáveis diferentes do destino. Não são contagens distintas de funções/bancos. (`bin/sonda_leitor_comum.sh:79`; `bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:139`; `bin/sonda_leitor_comum.sh:143`)
- `CONF_AUSENCIA` é definida uma vez e lida pelo parser/predicado `_conferencia_pronta`, que exige presença das chaves e valor textual zero. (`bin/sonda_leitor_comum.sh:155`; `bin/sonda_leitor_comum.sh:158`; `bin/sonda_leitor_comum.sh:161`; `bin/sonda_leitor_comum.sh:168`)
- A leitura importa a biblioteca e usa query/parser compartilhados; a porta importa a mesma biblioteca, consulta antes/depois e só exige prontidão depois. (`bin/sonda_leitura.sh:48`; `bin/sonda_leitura.sh:184`; `bin/sonda_leitura.sh:187`; `bin/sonda_provisionar.sh:42`; `bin/sonda_provisionar.sh:72`; `bin/sonda_provisionar.sh:172`; `bin/sonda_provisionar.sh:173`)
- Os dois destinos têm o mesmo algoritmo/significado: `_destino` apenas escolhe cluster e banco; as consultas usam `current_database()`. (`bin/sonda_leitor_comum.sh:28`; `bin/sonda_leitor_comum.sh:29`; `bin/sonda_leitor_comum.sh:130`; `bin/sonda_leitor_comum.sh:144`)
- O parser próprio localizado é de instrumentação do selo, `_chave`; sua decisão de pronto ainda delega ao predicado comum. As linhas PRONTO do selo shell são fixtures. (`bin/tests/test_sonda_sem_grant_pg.sh:216`; `bin/tests/test_sonda_sem_grant_pg.sh:217`; `bin/tests/test_sonda_sem_grant.sh:69`; `bin/tests/test_sonda_sem_grant.sh:136`)

Escritores do estado subjacente:

- A porta revoga CONNECT dos outros bancos, EXECUTE dos LO, EXECUTE das SECURITY DEFINER e defaults próprios do leitor. (`bin/sonda_provisionar.sh:117`; `bin/sonda_provisionar.sh:136`; `bin/sonda_provisionar.sh:150`; `bin/sonda_provisionar.sh:156`; `bin/sonda_provisionar.sh:157`)
- Não é correto dizer que todo esse estado tem apenas a porta como escritor: o selo explicitamente exige que o próprio leitor consiga alterar seus defaults/senha/default privileges e depois mede recusa/cura. Execução efetiva desses testes: [nao verificado]. (`bin/tests/test_sonda_sem_grant_pg.sh:362`; `bin/tests/test_sonda_sem_grant_pg.sh:376`; `bin/tests/test_sonda_sem_grant_pg.sh:379`)

Laço dos outros bancos:

- O laço está inteiro em um DO dentro de BEGIN/COMMIT, com `ON_ERROR_STOP`; falha em uma revogação não implica persistência das revogações anteriores dessa mesma transação. (`bin/sonda_provisionar.sh:51`; `bin/sonda_provisionar.sh:105`; `bin/sonda_provisionar.sh:117`; `bin/sonda_provisionar.sh:118`)
- A operação completa não é atômica: role+CONNECT dão COMMIT antes da transação do ACL. Uma falha posterior pode conservar o CONNECT já revogado e deixar ACL antigo. (`bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:127`; `bin/sonda_provisionar.sh:168`; `bin/sonda_provisionar.sh:169`)
- Falha de SQL/checagem/login impede anúncio de pronto e promoção do envfile; não há rollback da primeira transação. (`bin/sonda_provisionar.sh:93`; `bin/sonda_provisionar.sh:97`; `bin/sonda_provisionar.sh:173`; `bin/sonda_provisionar.sh:183`; `bin/sonda_provisionar.sh:196`; `bin/sonda_provisionar.sh:199`)
- O refazer invalida o stamp antes de destruir a sombra; falha da porta encerra o refazer antes da publicação final. Portanto pode deixar sombra vermelha com estado parcialmente aplicado, mas esse caminho não conserva o verde anterior. (`bin/sombra.sh:288`; `bin/sombra.sh:289`; `bin/sombra.sh:265`; `bin/sombra.sh:266`; `bin/sombra.sh:340`; `bin/sombra.sh:341`)
- O cron configurado encadeia refazer e bloco com `&&`, portanto falha do refazer impede o bloco subsequente. (`app/config/crons.py:351`)
- Em geral o REVOKE de PUBLIC afeta qualquer login não-superuser dependente desse privilégio; não há exclusão para `juliani_ref_*`. No inventário fornecido pelo usuário, só há saas_user super e leitor: outro login legítimo atual atingido não está demonstrado. Inventário e aplicação real: [nao verificado]. (`bin/sonda_provisionar.sh:117`)
- Conferência verde só responde aos poderes do leitor; ela não certifica disponibilidade para outros usuários. Assim, poderia estar verde mesmo que o amplo REVOKE prejudicasse outro login existente fora do inventário dado. (`bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:143`; `bin/sonda_leitor_comum.sh:145`)

## 09/11 13:27 · sessao 83f938655465 · - · - · origem=Ronald (tmux)

1. Tentativa em `INTRUSO_COPIA.txt` — saída literal; código de saída `1`. [nao verificado]

```text
/bin/bash: line 1: /home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/INTRUSO_COPIA.txt: Read-only file system
```

2. Tentativa em `INTRUSO_VIVA.txt` — saída literal; código de saída `1`. [nao verificado]

```text
/bin/bash: line 1: /home/ronald/saas-hasner/INTRUSO_VIVA.txt: Read-only file system
```

3. Tentativa em `INTRUSO_TMP.txt` — saída literal; código de saída `1`. [nao verificado]

```text
/bin/bash: line 1: /tmp/INTRUSO_TMP.txt: Read-only file system
```

## 09/11 13:27 · sessao 0c99726e989d · - · - · origem=Ronald (tmux)

ok.

## 09/11 13:11 · sessao 84f2029d65e7 · FIO-E0-V4-E3 · construtor · origem=Code

P1–P5 implementados. RED, execução ORM e mordida dos mutantes **[nao verificado]**; suite não executada. [nao verificado]

| Item | Hunks e resposta esperada |
|---|---|
| P1 | `chamados/services/aviso_pendencia.py:7` e `cobranca.py:45` leem `consulta_do_app()['pendentes']`. Testes: fechada viva não avisa; aberta coincide com ambos os badges (`chamados/tests/test_e3_v4_orm.py:20`, `:37`). |
| P2 | Seleção central em `api/views.py:1021`, `:1039`, `:1058`; preserva respondida viva da disputa aberta. ORM exige excluir pergunta sem carimbo terminal que C6 absolveu (`chamados/tests/test_e3_v4_orm.py:42`). |
| P3 | Instrumento resolve `sistema` por leitura e fornece seu pk (`chamados/management/commands/diff_e3_sombra.py:94`; `chamados/juizes.py:1106`). Testes exigem erro explícito sem criação e envenenam `get_user_sistema` no ramo trancado (`chamados/tests/test_e3_diff_sombra.py:26`, `:41`). |
| P4 | AST resolve atribuições e `getattr`; varredura inclui models/registro (`chamados/tests/test_e3_pendentes_app.py:64`, `:75`, `:82`, `:96`). Catraca permanece `=3` (`:144`); novos casos em `:119`, `:124`. |
| P5 | Lote pela autoridade existente, com contexto descartado por chamada (`chamados/juizes.py:946`, `:1058`, `:1080`, `:1084`; `chamados/services/pergunta_no_app.py:44`, `:55`, `:69`). Selo migrado em `chamados/tests/test_contract_pergunta_viva.py:75`. ORM exige **9 queries para N=1 e N=5**, além de frescor após tranca/toque/reabertura (`chamados/tests/test_e3_v4_orm.py:80`, `:96`). |
| F4 | Uma linha declara o limite de `date.today` (`chamados/management/commands/diff_e3_sombra.py:105`); leitor da saída ajustado (`chamados/tests/test_e3_diff_sombra.py:21`). |

Escrita/leitura dos campos e funções alterados:

| Escritor/produtor | Leitor |
|---|---|
| `pendentes/historico`: `chamados/services/pergunta_no_app.py:71`, `:73` | Aviso `aviso_pendencia.py:9`, `:13`; cobrança `cobranca.py:48`, `:82`, `:163`; kernels `api/views.py:325`, `api/views_core.py:314`; dashboard `core/views.py:253`; endpoints `api/views.py:1023`, `:2561`. |
| Índices locais: `api/views.py:1064`, `:1065` | Serialização `api/views.py:1173`. |
| `sistema_pk/memo`: `diff_e3_sombra.py:98` → `chamados/juizes.py:1111` | `_admin_tocou_os_dias_depois`, `chamados/juizes.py:1173`; carga da tarefa `chamados/services/tarefa_do_dia.py:370`. Chamador anterior do contexto: `chamados/management/commands/vigiar_fio_morto.py:171`. |
| Contexto, memo e mapa de dias: `chamados/juizes.py:1091`, `:1096`, `:1099` | `_memo_da_tranca` `:1058`, lote `:992`, toque `:1165`, degrau C6 `:948`, `:1327`; descarte `:1102`. |
| `em_lote`: `chamados/services/pergunta_no_app.py:69` | Seleção em `:56`; demais leitores mantêm chamada sem argumento em `:163`, `:186` e `chamados/juizes.py:1519`. |
| Aliases/fontes/chamadas AST: `chamados/tests/test_e3_pendentes_app.py:80`, `:86`, `:114`, `:90` | Resolução `:66`, catraca `:145`; mutantes em `chamados/tests/e3_mutantes.py:36`, `:123`, executor `:138`. |

Comandos dos testes, **não executados**: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e3_v4_orm.LeitoresPendentesV4Test --settings=config.settings.ci
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e3_diff_sombra --settings=config.settings.ci
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_pendentes_app.CatracaC17Test
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e3_v4_orm.CustoConsultaV4Test chamados.tests.test_contract_pergunta_viva.ExibicaoUsaADerivacaoTest --settings=config.settings.ci
```

Mutantes novos/ajustados, **mordida [nao verificado]** (`chamados/tests/e3_mutantes.py:34`, `:123`):

```bash
m() { PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_mutantes --mutante "$1"; }
m aviso_inclui_historico
m cobranca_inclui_historico
m chamados_v2_sem_C6
m chamados_v2_perde_respondida
m diff_cria_sistema
m diff_nao_alimenta_sistema
m diff_omite_limite_relogio
m consulta_sem_lote
m consulta_memo_persistente
m consulta_ignora_tranca
m consulta_vazia
m catraca_ignora_atribuicao
m catraca_ignora_getattr
m catraca_exclui_models
m catraca_exclui_registro
m quarto_c17_alias
m quarto_c17_atribuicao
m quarto_c17_atributo
m quarto_c17_getattr
m quarto_c17_getattr_atribuido
```

`py_compile`: **11 arquivos, rc 0**, interceptando bytecode; `git diff --check`: **rc 0**. Comando reproduzível:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import subprocess, py_compile
from unittest.mock import patch
paths = subprocess.check_output(
    ['git', 'diff', '--name-only', '--relative'], text=True).splitlines()
paths += ['chamados/tests/test_e3_v4_orm.py']
with patch('importlib._bootstrap_external._write_atomic'):
    for path in sorted(set(paths)):
        if path.endswith('.py'):
            py_compile.compile(path, cfile=path + '.verificacao', doraise=True)
PY
GIT_OPTIONAL_LOCKS=0 git diff --check
```

**DECISAO-ABERTA:** ligação da fachada continua aguardando DIFF por chamador/caso, L-099 e SONDA-SEM-GRANT; resultado de produção **[nao verificado]**. C17 permanece em `chamados/juizes.py:1541`; os três chamadores permanecem em `folha/fora_da_folha.py:67`, `chamados/services/vigia_fio.py:635` e `escala/services/cadastro_realidade.py:488`.

## 09/11 13:07 · sessao 07a8155e702d · SELOS-CANON-I3 · verificador · origem=Code

**REPROVADO: T1, T2, T4 e T8 ainda não fecham a classe.** Há casamento com tentativa errada, escolha incorreta entre fotografias, carimbo concorrente e perda definitiva de eventos do placar. (`bin/disjuntor.py:932`, `bin/disjuntor.py:969`, `bin/disjuntor.py:916`, `bin/disjuntor.py:1316`)

RED de 45 falhas, GREEN, morte executada dos 38 mutantes, condição dos hooks instalados e ocorrência em produção: **[nao verificado]**.

**1. T1…T8**

| Item | Classificação e conclusão |
|---|---|
| **T1** | **PROCEDENTE.** Obra+pid+hash12+janela não identifica a tentativa. Uma tentativa fotografa, perde a vaga e termina; outra, com PID reciclado e mesma obra/hash, abre DESLIGADA dentro dos 600 s. Como essa abertura não produz fotografia, seu `ini` pode efetivar a fotografia recusada anterior. O fotógrafo considera somente SNAPSHOT; o censo considera também DESLIGADO/FORA: compartilhar a função não compartilha o casamento. **Cura:** identificador único da tentativa, ou publicação da fotografia depois da vaga. (`bin/disjuntor.py:932`, `bin/disjuntor.py:956`, `bin/disjuntor.py:964`, `bin/disjuntor.py:2467`, `bin/disjuntor.py:2487`) |
| **T1 — desempate** | **PROCEDENTE.** Duas fotografias Agent no mesmo segundo, primeiro N=5 e depois N=3, resultam em N=5: `sort()` ordena `(instante,N,via)` e o leitor escolhe a última tupla. **Cura:** ordenar pela abertura identificada, sem usar N como desempate. (`bin/disjuntor.py:958`, `bin/disjuntor.py:969`, `bin/disjuntor.py:977`) |
| **T2** | **PROCEDENTE.** O carimbo nasce por leitura seguida de append, sem exclusão comum: dois processos podem ler ausência e gravar dois carimbos. É idempotente sequencialmente, não sob corrida. **Cura:** leitura/criação sob exclusão única, preservando um marco confiável. (`bin/disjuntor.py:907`, `bin/disjuntor.py:916`, `bin/disjuntor.py:2028`, `bin/disjuntor.py:2690`) |
| **T3** | **NAO PROCEDE quanto ao N paralelo.** B3 e `--etapas` consultam `_fotografia_efetiva`; a configuração manda consumir esse N. Os defeitos dessa autoridade continuam sendo T1. (`bin/disjuntor.py:1063`, `bin/disjuntor.py:2719`, `.claude/agents/verificador.md:18`) |
| **T4** | **PROCEDENTE, cura parcial.** `_obra_de` participa das duas pontas, mas não é a representação única: entrada CLI aceita ID sem espaços; fotografia e medida usam `\S+`; B3 reconhece `O9 (C)`. A guarda impede alguns IDs incompatíveis, porém uma identidade qualificada reconhecida no final não pode ser fotografada pela porta. **Cura:** contrato canônico de identidade em abertura, fotografia, consulta e final, preservando caudas significativas. (`bin/disjuntor.py:178`, `bin/disjuntor.py:652`, `bin/disjuntor.py:884`, `bin/disjuntor.py:886`, `bin/disjuntor.py:2031`, `bin/disjuntor.py:2035`) |
| **T5** | **NAO PROCEDE quanto à DIETA.** O helper lê vivo+arquivo. Há **dois consumidores**: opus confirma K e B3 conta K por reconhecimento textual; portanto “leitor único” só descreve o helper de código. `n` confirmado e `n_rel` reconhecido são cruzados pelo mínimo; isso, isoladamente, não prova duplicação de juiz cadastrado. (`bin/disjuntor.py:989`, `bin/disjuntor.py:992`, `bin/disjuntor.py:1004`, `bin/disjuntor.py:1090`, `.claude/agents/verificador.md:18`) |
| **T6** | **NAO PROCEDE quanto à mistura original.** Cabeças e contadores foram separados; ETAPAS não entra no reconhecedor que escreve PAREI censo. Não existe PAREI ETAPAS nesse caminho. (`bin/disjuntor.py:1789`, `bin/disjuntor.py:1792`, `bin/disjuntor.py:334`, `bin/disjuntor.py:2406`, `bin/disjuntor.py:2502`) |
| **T7** | **NAO PROCEDE no recorte corrigido.** Harness exige rc≠0 **e** a FALHA específica; há guarda n≤N e casos para resultado `?` e K fora de 1…N. Isso não certifica toda a classe de fotografia; faltas abaixo. (`bin/tests/test_etapas_no_brief.sh:446`, `bin/disjuntor.py:1090`, `bin/tests/test_etapas_no_brief.sh:397`, `bin/tests/test_etapas_no_brief.sh:403`) |
| **T8** | **PROCEDENTE.** SEM-FOTO entrou no denominador e ganhou esperado zero, mas o memo é persistido **antes** do anúncio não estrito. Se o JSON gravar e o append falhar, o evento desaparece definitivamente: o memo impede nova emissão, enquanto ESTADO lê somente a trilha. **Cura:** evento persistido com o memo e publicação idempotente por aid, ou medição pelo estado com instante canônico. (`bin/disjuntor.py:2531`, `bin/disjuntor.py:2535`, `bin/disjuntor.py:397`, `bin/disjuntor.py:407`, `bin/disjuntor.py:1316`, `bin/disjuntor.py:219`, `bin/disjuntor.py:1059`) |

**Sobre o `ini`: NAO PROCEDE a hipótese de escrita anterior ao flock.** Ele está depois da aquisição da vaga. Entretanto inventário e raspagem ficam no intervalo; `flock -n` não prova duração máxima até o `ini`. (`bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:244`, `bin/codex_pergunta.sh:283`, `bin/codex_pergunta.sh:292`)

**Sobre a isenção: PROCEDENTE.** Ela consulta trilha e chamadas, sem depender do mtime do RELATO; porém aceita SNAPSHOT/CENSO APROVADO pré-vaga como prova de “abertura”, inclusive tentativa que não efetivou. **Cura:** provar a primeira abertura efetiva pela mesma identidade da tentativa. (`bin/disjuntor.py:889`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1027`, `bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`)

**SEM-FOTO realmente entra como reprovação no B3**, independentemente da via que originou a abertura. A universalidade falha antes: fotografia indevidamente aceita, isenção indevida ou classificação FORA evitam chegar a SEM-FOTO. (`bin/disjuntor.py:1063`, `bin/disjuntor.py:1065`, `bin/disjuntor.py:1071`, `bin/disjuntor.py:1077`, `bin/disjuntor.py:1254`)

**2. As 12 decisões**

| Nº | Classificação |
|---|---|
| **1** | **NAO PROCEDE como estado mentiroso:** ausência de PAREI próprio não transforma a negativa em CENSO; há recusa e registro no RELATO. (`bin/disjuntor.py:2071`, `bin/disjuntor.py:2263`, `bin/disjuntor.py:2702`) |
| **2** | **PROCEDENTE:** o marco representa primeiro uso bem-sucedido, não deploy. DESLIGADO engole falha do carimbo e permite abertura; um carimbo posterior pode chamar essa abertura do código novo de legado. **Cura:** marco confiável, concorrência resolvida e ausência sem fabricação de isenção. (`bin/disjuntor.py:916`, `bin/disjuntor.py:2668`, `bin/disjuntor.py:2672`, `bin/disjuntor.py:1071`) |
| **3** | **NAO PROCEDE:** `-1` é sentinela interna, convertido em mensagem própria; entra na fila existente de reprovações. (`bin/disjuntor.py:1077`, `bin/disjuntor.py:1254`, `bin/disjuntor.py:1278`) |
| **4** | **NAO PROCEDE para obra comprovadamente externa.** A prova histórica insuficiente está na decisão 6. (`bin/disjuntor.py:1036`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:1065`) |
| **5** | **NAO PROCEDE mecanicamente:** sujeita, sem foto e sem abertura anterior provada termina SEM-FOTO. A cronologia histórica alegada “pré-I1/pós-carimbo”: **[nao verificado]**. (`bin/disjuntor.py:1065`, `bin/disjuntor.py:1071`, `bin/disjuntor.py:1077`) |
| **6** | **PROCEDENTE:** abertura DESLIGADA na fila 1, depois retirada da fila, pode virar FORA. O histórico de sujeição não reconhece DESLIGADO. **Cura:** preservar a sujeição da abertura, inclusive desligada ou indeterminada. (`bin/disjuntor.py:891`, `bin/disjuntor.py:1036`, `bin/disjuntor.py:1045`) |
| **7** | **PROCEDENTE:** wrapper exige carimbo antes de dispensar FORA; Agent dispensa FORA antes do carimbo. Amplia escopo numa única via. **Cura:** manter a mesma condição de aplicação nas duas entradas. (`bin/disjuntor.py:2025`, `bin/disjuntor.py:2028`, `bin/disjuntor.py:2690`, `bin/disjuntor.py:2693`; `CLAUDE.md:37`) |
| **8** | **NAO PROCEDE como aprovação indevida:** não empresta foto da identidade nua. Permanece a incompatibilidade de representações de T4. (`bin/disjuntor.py:652`, `bin/disjuntor.py:1063`, `bin/disjuntor.py:1077`) |
| **9** | **FILA — mensagem:** prefixo externo ainda diz CENSO para negativa ETAPAS; detalhe e eventos distinguem os motivos. Não é mistura dos contadores. (`bin/codex_pergunta.sh:234`, `bin/disjuntor.py:2406`, `bin/disjuntor.py:2502`) |
| **10** | **FILA — validação:** execução de ruff/AST/bash-n alegada **[nao verificado]**. |
| **11** | **FILA — ambiente:** causa e exclusividade ambiental do vermelho **[nao verificado]**. |
| **12** | **PROCEDENTE:** janela e consumo único não substituem identidade da tentativa; sobrevivem os contraexemplos T1. (`bin/disjuntor.py:932`, `bin/disjuntor.py:936`, `bin/disjuntor.py:2409`) |

**3. Demais classes**

- **PROCEDENTE — relógio fora da borda:** o instante interno de `trilha_linhas` passou a decidir isenção. Fotografias também aceitam futuro: não recebem `agora`, enquanto o censo o aplica. **Cura:** propagar o instante da borda e usar identidade/ordem persistida para distinguir aberturas. (`bin/disjuntor.py:213`, `bin/disjuntor.py:1071`, `bin/disjuntor.py:958`, `bin/disjuntor.py:964`, `bin/disjuntor.py:2456`, `bin/disjuntor.py:2251`)
- **NAO PROCEDE como segundo escritor introduzido:** append converge em `trilha_linhas`; estado converge em `_com_estado`; ESTADO consome `ler/linha`. Isso não resolve atomicidade carimbo/evento/memo. (`bin/disjuntor.py:218`, `bin/disjuntor.py:226`, `bin/disjuntor.py:397`, `bin/gerar_estado.py:280`)
- **NAO PROCEDE quanto a N paralelo; duplicação de juiz registrado não demonstrada.** Existe uma autoridade consultada pelas duas entradas de N; há duas verificações de n com significados explícitos acima. (`bin/disjuntor.py:1063`, `bin/disjuntor.py:2719`, `bin/disjuntor.py:1090`)
- **FILA — pouso antes do pré-requisito:** Bash principal continua podendo retornar antes do B3; é o caminho herdado apontado em D2. (`bin/disjuntor.py:2174`, `bin/disjuntor.py:2202`)
- **Semântica efetiva:** `hash12` do wrapper identifica o commit; o hash de cada brief é outro campo. N é máximo dos briefs; `n_v` é confirmação do opus e `n_rel` é reconhecimento textual. Casamento usa `[ini−600,ini]`; medição usa `[agora−86400,agora]`, ambas inclusivas. O rótulo “pousos” mede finais conferidos, não merges realizados. (`bin/codex_pergunta.sh:292`, `bin/disjuntor.py:2055`, `bin/disjuntor.py:2059`, `bin/disjuntor.py:1090`, `bin/disjuntor.py:933`, `bin/disjuntor.py:2525`, `bin/disjuntor.py:2552`)

**4. Selos e quatro reancoragens**

| Mutante | Mesma propriedade? |
|---|---|
| `abertura-sem-janela` | **NAO PROCEDE alteração de propriedade:** remove o limite inferior; fixture separa aprovação/ini por uma hora. (`bin/tests/test_censo_antes_da_vaga.sh:723`, `bin/tests/test_censo_antes_da_vaga.sh:470`, `bin/tests/test_censo_antes_da_vaga.sh:480`, `bin/tests/test_censo_antes_da_vaga.sh:486`) |
| `sem-consumo-unico` | **NAO PROCEDE:** uma aprovação para dois ini deve continuar dando com=1/sem=1. (`bin/tests/test_censo_antes_da_vaga.sh:731`, `bin/tests/test_censo_antes_da_vaga.sh:503`, `bin/tests/test_censo_antes_da_vaga.sh:507`, `bin/tests/test_censo_antes_da_vaga.sh:510`) |
| `ini-do-futuro` | **NAO PROCEDE:** remove somente a exclusão do ini futuro. (`bin/tests/test_censo_antes_da_vaga.sh:747`, `bin/tests/test_censo_antes_da_vaga.sh:507`, `bin/tests/test_censo_antes_da_vaga.sh:511`) |
| `pertenca-wrapper-propria` | **NAO PROCEDE:** continua contrapondo pertencimento canônico à célula crua. Esta âncora vai para `if dentro is False:`, não para `_casa_wrapper_ini`. (`bin/tests/test_censo_antes_da_vaga.sh:753`, `bin/tests/test_censo_antes_da_vaga.sh:535`, `bin/tests/test_censo_antes_da_vaga.sh:536`) |

**PROCEDENTE — cobertura faltante:** T1 fabrica `ini`, usa PIDs distintos e não disputa o flock. Faltam os contraexemplos acima, carimbo concorrente e perda de anúncio após memo. Falta também positivo Agent→fotografia→B3/`--etapas`: retirar a ingestão Agent não encontra esse teste. **Cura:** acrescentar esses casos e seus mutantes específicos; sobrevivência executada **[nao verificado]**. (`bin/tests/test_etapas_no_brief.sh:72`, `bin/tests/test_etapas_no_brief.sh:320`, `bin/tests/test_etapas_no_brief.sh:322`, `bin/tests/test_etapas_no_brief.sh:148`, `bin/tests/test_etapas_no_brief.sh:176`, `bin/disjuntor.py:958`)

**FILA — harness vizinho:** ainda aceita qualquer rc≠0; portanto suas mensagens não certificam morte pela FALHA correta. Casos `?` e K fora de N já verdes antes podem legitimamente matar mutantes novos. (`bin/tests/test_censo_antes_da_vaga.sh:685`, `bin/tests/test_etapas_no_brief.sh:513`, `bin/tests/test_etapas_no_brief.sh:514`)

**5. Quem escreve / quem lê**

| Arquivo/campo | Escritor | Leitor |
|---|---|---|
| Briefs: CENSO, ETAPAS, K | Autor real **[nao verificado]** | Coletor e validadores: `bin/disjuntor.py:1931`, `bin/disjuntor.py:2047`, `bin/disjuntor.py:2050` |
| BACKLOG / fila | Autor real **[nao verificado]** | Leitor delegado e pertencimento: `bin/disjuntor.py:1958`, `bin/disjuntor.py:1990` |
| `disjuntor.log`: instante/evento/detalhe | Append e limite de 300: `bin/disjuntor.py:213`, `bin/disjuntor.py:218` | Fotografia, abertura, sujeição, medidores: `bin/disjuntor.py:951`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:2449`, `bin/disjuntor.py:2518` |
| INSTALADO | `bin/disjuntor.py:916` | `bin/disjuntor.py:909`; efeito na isenção: `bin/disjuntor.py:1071` |
| SNAPSHOT: obra/N/via/pid/hash/briefs | Porta CLI: `bin/disjuntor.py:2059`, `bin/disjuntor.py:2064`; Agent: `bin/disjuntor.py:2068`, `bin/disjuntor.py:2162` | `bin/disjuntor.py:951`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1042` |
| CENSO APROVADO/FORA/DESLIGADO | `bin/disjuntor.py:2064`, `bin/disjuntor.py:2162`, `bin/disjuntor.py:2694`, `bin/disjuntor.py:2670`, `bin/disjuntor.py:2238` | `bin/disjuntor.py:1020`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:2449` |
| `chamadas.log`: ini/fim, obra/papel/hash12/pid | Wrapper: `bin/codex_pergunta.sh:292`, `bin/codex_pergunta.sh:259` | Parser: `bin/codex_limite_estado.py:49`; consumidores novos: `bin/disjuntor.py:964`, `bin/disjuntor.py:1027`; censo: `bin/disjuntor.py:2483` |
| Transcript/meta/handback/aid | Harness real **[nao verificado]**; fixture: `bin/tests/test_etapas_no_brief.sh:160` | `bin/disjuntor.py:868`, `bin/disjuntor.py:872`, `bin/disjuntor.py:681`, `bin/disjuntor.py:687` |
| `ETAPAS: n/N` | Instrução ao verificador: `.claude/agents/verificador.md:18`; execução **[nao verificado]** | `bin/disjuntor.py:1088` |
| RELATO + RELATO-ARQUIVO / linhas ETAPA | Autor e executor da DIETA **[nao verificado]**; fixtures: `bin/tests/test_etapas_no_brief.sh:47`, `bin/tests/test_etapas_no_brief.sh:381` | `bin/disjuntor.py:989`, `bin/disjuntor.py:992`, `bin/disjuntor.py:1004`; opus: `.claude/agents/verificador.md:18` |
| `disjuntor_estado.json`: memo, reprovações, metade | Conferência/incorporação: `bin/disjuntor.py:1091`, `bin/disjuntor.py:1253`; flock/replace: `bin/disjuntor.py:376`, `bin/disjuntor.py:397` | Memo e decisão: `bin/disjuntor.py:1059`, `bin/disjuntor.py:1269` |
| ETAPAS finais: FORA/isenta/SEM-FOTO/METADE/PROVADAS | Prepara: `bin/disjuntor.py:1067`, `bin/disjuntor.py:1073`, `bin/disjuntor.py:1078`, `bin/disjuntor.py:1097`, `bin/disjuntor.py:1099`; publica: `bin/disjuntor.py:1316` | `bin/disjuntor.py:2518` |
| NEGOU / ALVO / alarme no RELATO | `bin/disjuntor.py:2260`, `bin/disjuntor.py:2700`, `bin/disjuntor.py:302`, `bin/disjuntor.py:319` → `app/core/esteira_vigia.py:843` | Sujeição e contadores: `bin/disjuntor.py:1042`, `bin/disjuntor.py:2449`, `bin/disjuntor.py:2518` |
| PAREI censo | `bin/disjuntor.py:349` → `app/core/esteira_vigia.py:865`; trava: `app/core/esteira_vigia.py:854` | Stop/handoff/alarme: `bin/hook_stop_fila1.py:773`, `bin/handoff_sessao.sh:130`, `bin/alarme_sessao_ociosa.py:137` |
| Hooks / linha ETAPAS do ESTADO | Configuração: `.claude/settings.json:68`, `.claude/settings.json:9`; composição: `bin/disjuntor.py:2402`, `bin/disjuntor.py:2649`; gravação: `bin/gerar_estado.py:390` | B3 pelos hooks: `bin/disjuntor.py:2186`, `bin/disjuntor.py:2202`, `bin/hook_stop_fila1.py:1204`; gerador/publicação: `bin/gerar_estado.py:281`, `bin/relato.sh:38`, `bin/relato.sh:105` |

## 09/11 13:05 · sessao 8ac92c7e3053 · - · - · origem=Ronald (tmux)

T1/T2 **não fechados na classe**:

- **PROCEDENTE — tentativa errada aceita.** Snapshot recusado de `(obra,pid,hash12)` pode casar com `ini` de PID reciclado cuja nova abertura foi DESLIGADA/FORA, sem produzir snapshot próprio. `_fotografias` casa somente snapshots; `_censo_24h` casa também DESLIGADO/FORA, portanto o mesmo helper pode selecionar tentativas diferentes. Demonstrado em memória: snapshot t100 + `ini` t210 → casamento aceito. Cura: identificador único da tentativa compartilhado ou fotografia após a vaga. (`bin/disjuntor.py:932`, `bin/disjuntor.py:956`, `bin/disjuntor.py:964`, `bin/disjuntor.py:2467`, `bin/disjuntor.py:2487`)
- **PROCEDENTE — empate temporal troca N.** `v.sort()` ordena `(t,N,via)`; N5 seguido de N3 no mesmo segundo retorna N5. Demonstrado executando somente funções extraídas por AST, com arquivos simulados em memória. Cura: ordem/identidade inequívoca da abertura; timestamp em segundos não desempata. (`bin/disjuntor.py:954`, `bin/disjuntor.py:969`, `bin/disjuntor.py:977`)
- **PROCEDENTE — carimbo concorrente.** Dois processos podem ler ausência e ambos escrever; ausência de trava entre leitura e append. Demonstrado em memória com duas leituras intercaladas: dois carimbos. A trava B3 não protege chamadas independentes do wrapper/Agent. Cura: criação/leitura idempotente sob exclusão comum. (`bin/disjuntor.py:907`, `bin/disjuntor.py:916`, `bin/disjuntor.py:2028`, `bin/disjuntor.py:2690`)
- **PROCEDENTE — relógio decide fora da borda.** Carimbo usa `datetime.now()` dentro do escritor; essa hora agora decide isenção. Fotos futuras são aceitas, inclusive wrapper chamando casamento sem `agora`; censo exclui futuro. Cura: passar instante da borda e ordenar por identidade persistida da abertura. (`bin/disjuntor.py:213`, `bin/disjuntor.py:958`, `bin/disjuntor.py:964`, `bin/disjuntor.py:1071`, `bin/disjuntor.py:2251`, `bin/disjuntor.py:2456`)
- **NAO PROCEDE — `ini` antes da vaga.** O escritor fica depois do `flock`; entretanto há inventário e raspagem entre ambos, sem garantia de “segundos”. (`bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:244`, `bin/codex_pergunta.sh:283`, `bin/codex_pergunta.sh:292`)

Decisões:

- **2 PROCEDENTE:** carimbo representa primeiro uso bem sucedido, não instalação/deploy. Wrapper DESLIGADO engole falha do carimbo e ainda pode registrar `ini`; carimbo posterior permite chamar essa abertura do código novo de legado. Cura: marco de instalação persistido/confiável, com falha e concorrência tratadas. (`bin/disjuntor.py:916`, `bin/disjuntor.py:2668`, `bin/codex_pergunta.sh:292`, `bin/disjuntor.py:1071`)
- **3 NAO PROCEDE:** SEM-FOTO entra na mesma fila B3 e dispara pelo limiar existente; não é simples aviso. (`bin/disjuntor.py:1077`, `bin/disjuntor.py:1254`, `bin/disjuntor.py:1272`)
- **4 NAO PROCEDE, condicional:** FORA é dispensado quando pertencimento atual e eventos reconhecidos não demonstram sujeição. A limitação histórica está na decisão 6. (`bin/disjuntor.py:1036`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1065`)
- **5 NAO PROCEDE:** comprovadamente sujeita, sem foto e sem abertura anterior ao carimbo, entra SEM-FOTO. Possibilidade histórica de “pré-I1, pós-I3” real: **[nao verificado]**. (`bin/disjuntor.py:1065`, `bin/disjuntor.py:1071`, `bin/disjuntor.py:1077`)
- **6 PROCEDENTE:** fila1 aberta DESLIGADA após carimbo e removida antes do final vira FORA: `_SUJEITA_RE` não reconhece DESLIGADO, e BACKLOG atual não preserva pertencimento da abertura. Cura: registrar sujeição da abertura, inclusive desligada/indeterminada. (`bin/disjuntor.py:891`, `bin/disjuntor.py:1036`, `bin/disjuntor.py:1065`)
- **7 PROCEDENTE:** wrapper fora da fila1 pode ser recusado só porque carimbo não grava; Agent fora retorna antes dessa escrita. Cura: preservar escopo fila1 nas duas vias. (`bin/disjuntor.py:2025`, `bin/disjuntor.py:2028`, `bin/disjuntor.py:2690`, `bin/disjuntor.py:2693`)
- **12 PROCEDENTE:** janela inclusiva `[ini−600,ini]` e consumo único não fornecem identidade da tentativa; os contraexemplos acima sobrevivem. (`bin/disjuntor.py:933`, `bin/disjuntor.py:936`, `bin/disjuntor.py:2409`)

A isenção consulta trilha e `chamadas.log`, sem mtime do RELATO; mas inclui snapshot não efetivado e CENSO APROVADO pré-vaga como prova de “abertura”. Cura: primeira abertura efetiva pelo mesmo identificador canônico. (`bin/disjuntor.py:889`, `bin/disjuntor.py:1022`, `bin/disjuntor.py:1027`, `bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`)

## 09/11 13:05 · sessao ac5e5ec8886f · - · - · origem=Ronald (tmux)

- **T3 — NAO PROCEDE quanto a N paralelo:** B3 e `--etapas` usam `_fotografia_efetiva`; verificador recebe ordem explícita de consumir esse N. (`bin/disjuntor.py:1063`, `bin/disjuntor.py:2719`, `.claude/agents/verificador.md:18`)
- **T4 — parcial:** `_obra_de` está na abertura Agent, guarda da abertura CLI, final B3 e consulta `--etapas`; o wrapper ainda recebe ID literal, e snapshots/medidas usam `\S+`, enquanto `_obra_de` aceita identidade com cauda/espaço. Final `O9 (C)` é recusado por falta da fotografia própria; não houve unificação integral das representações. (`bin/disjuntor.py:2207`, `bin/disjuntor.py:2035`, `bin/disjuntor.py:681`, `bin/disjuntor.py:2714`, `bin/codex_pergunta.sh:188`, `bin/disjuntor.py:884`, `bin/disjuntor.py:886`, `bin/disjuntor.py:649`)
- **T5 — cura da DIETA presente, “leitor único” absoluto falso:** um helper de código reúne vivo+arquivo; porém verificador conta K separadamente e B3 aplica seu próprio parser/filtro para n. Divergência humana efetivamente ocorrida: **[nao verificado]**. Cura para centralizar contagem: expor Ks/n pelo leitor canônico e reservar ao opus a confirmação do conteúdo. (`bin/disjuntor.py:989`, `bin/disjuntor.py:992`, `bin/disjuntor.py:1004`, `.claude/agents/verificador.md:18`, `app/docs/LEIS.md:60`)
- **T6 — cabeças/placares separados:** ETAPAS possui literal próprio; `_obra_da_negativa` só reconhece CENSO, portanto ETAPAS não dispara PAREI censo. O placar ETAPAS reconhece negativa própria e CENSO permanece restrito à sua cabeça. (`bin/disjuntor.py:1789`, `bin/disjuntor.py:1792`, `bin/disjuntor.py:333`, `bin/disjuntor.py:2502`, `bin/disjuntor.py:2406`)
- **T8 — PROCEDENTE, perda permanente da medida:** memo é gravado antes do anúncio; anúncio usa trilha não estrita, que engole erro. Falha de append com JSON gravável perde PROVADAS/METADE/SEM-FOTO definitivamente, pois o memo impede repetir; ESTADO lê só a trilha e poderá publicar zero. Cura: evento persistido com memo/outbox, entrega idempotente por aid, ou medição pelo estado com instante canônico. (`bin/disjuntor.py:1091`, `bin/disjuntor.py:396`, `bin/disjuntor.py:407`, `bin/disjuntor.py:1315`, `bin/disjuntor.py:225`, `bin/disjuntor.py:219`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:2516`, `bin/disjuntor.py:2538`)
- **T8 — conta implementada:** inclui SEM-FOTO no denominador e contador próprio esperado zero; exclui FORA/isentos. Janela inclusiva nas duas bordas. Rótulo continua dizendo “pousos”, mas universo é finais APROVADOS conferidos. (`bin/disjuntor.py:2525`, `bin/disjuntor.py:2528`, `bin/disjuntor.py:2531`, `bin/disjuntor.py:2535`, `bin/disjuntor.py:2552`)

Inventário consolidável:

| Arquivo/campo | Escritor | Leitor |
|---|---|---|
| Briefs / CENSO / ETAPAS | Autor real **[nao verificado]**; fixture em `bin/tests/test_etapas_no_brief.sh:65` | Coletor `bin/disjuntor.py:1931`; validadores `bin/disjuntor.py:1818`, `bin/disjuntor.py:1875`; aplicação `bin/disjuntor.py:2047` |
| BACKLOG / fila | Autor real **[nao verificado]** | `bin/disjuntor.py:1954` delega `_itens_andaveis` em `bin/disjuntor.py:1958`; pertinência `bin/disjuntor.py:1990` |
| `disjuntor.log` / instante / evento / detalhe | Append central `bin/disjuntor.py:209`; relógio interno `bin/disjuntor.py:213`; truncamento `bin/disjuntor.py:218` | Fotos `bin/disjuntor.py:951`; abertura `bin/disjuntor.py:1018`; sujeição `bin/disjuntor.py:1040`; CENSO `bin/disjuntor.py:2447`; ETAPAS `bin/disjuntor.py:2516` |
| INSTALADO | `_instalacao_etapas`, `bin/disjuntor.py:916` | Mesma função, `bin/disjuntor.py:909` |
| SNAPSHOT / obra / N / via / pid / hash12 / briefs | Porta CLI `bin/disjuntor.py:2058`, `bin/disjuntor.py:2064`; Agent acumula `bin/disjuntor.py:2068`, publica `bin/disjuntor.py:2162` | `_fotografias` `bin/disjuntor.py:951`; `_primeira_abertura` `bin/disjuntor.py:1020`; `_sujeita` `bin/disjuntor.py:1042` |
| CENSO APROVADO/FORA/DESLIGADO | `bin/disjuntor.py:2064`, `bin/disjuntor.py:2162`, `bin/disjuntor.py:2694`, `bin/disjuntor.py:2670`, `bin/disjuntor.py:2238` | `bin/disjuntor.py:1020`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:2449` |
| `chamadas.log` / ini / obra / papel / hash12 / pid | Wrapper `bin/codex_pergunta.sh:292`, depois do flock `bin/codex_pergunta.sh:240`; fim `bin/codex_pergunta.sh:259` | Parser `bin/codex_limite_estado.py:49`; chamado por `bin/disjuntor.py:964`, `bin/disjuntor.py:1027`, `bin/disjuntor.py:2483` |
| Transcript/meta / handback / aid / obra / veredito | Harness real **[nao verificado]**; fixture `bin/tests/test_etapas_no_brief.sh:160` | `bin/disjuntor.py:868`, `bin/disjuntor.py:869`, `bin/disjuntor.py:872`, `bin/disjuntor.py:681`, `bin/disjuntor.py:687` |
| ETAPAS n/N | Verificador instruído em `.claude/agents/verificador.md:18`; execução **[nao verificado]** | `bin/disjuntor.py:1088` |
| ETAPA obra K / resultado no vivo+arquivo | Autor real e executor da DIETA **[nao verificado]**; fixture `bin/tests/test_etapas_no_brief.sh:47`, `bin/tests/test_etapas_no_brief.sh:381` | `bin/disjuntor.py:989`, `bin/disjuntor.py:992`, `bin/disjuntor.py:1004`; também `.claude/agents/verificador.md:18` |
| `disjuntor_estado.json` / memo / b3.reprovados / metade | Conferência `bin/disjuntor.py:1056`, `bin/disjuntor.py:1091`; incorporação B3 `bin/disjuntor.py:1253`; persistência sob flock `bin/disjuntor.py:376`, `bin/disjuntor.py:396` | Memo `bin/disjuntor.py:1059`; decisão B3 `bin/disjuntor.py:1264` |
| Eventos ETAPAS finais | Prepara `bin/disjuntor.py:1067`, `bin/disjuntor.py:1073`, `bin/disjuntor.py:1078`, `bin/disjuntor.py:1097`, `bin/disjuntor.py:1099`; publica `bin/disjuntor.py:1315` | `bin/disjuntor.py:2518` |
| NEGOU ETAPAS/CENSO / ALVO / RELATO | `bin/disjuntor.py:2260`, `bin/disjuntor.py:2271`, `bin/disjuntor.py:2700`; RELATO via `bin/disjuntor.py:319` → `app/core/esteira_vigia.py:843`; ALVO `bin/disjuntor.py:302` | Sujeição `bin/disjuntor.py:1042`; contadores `bin/disjuntor.py:2449`, `bin/disjuntor.py:2518` |
| PAREI censo | `bin/disjuntor.py:340` → `app/core/esteira_vigia.py:865`, trava comum `app/core/esteira_vigia.py:846` | Stop `bin/hook_stop_fila1.py:773`; handoff `bin/handoff_sessao.sh:130`; alarme `bin/alarme_sessao_ociosa.py:137` |
| Hook PreToolUse / Stop | Configurações `.claude/settings.json:68`, `.claude/settings.json:9` | Agent final `bin/disjuntor.py:2202`; Codex verificador `bin/disjuntor.py:2186`; Stop delega `bin/hook_stop_fila1.py:1204` |
| Linha ETAPAS no ESTADO | `ler` em `bin/disjuntor.py:2402`; composição `bin/disjuntor.py:2649`; gerador delega `bin/gerar_estado.py:280`, grava `bin/gerar_estado.py:390`; publicação `bin/relato.sh:38`, `bin/relato.sh:105` | Consumidores reais posteriores **[nao verificado]** |

## 09/11 13:04 · sessao c81d9af71fbe · - · - · origem=Ronald (tmux)

T7 **NAO PROCEDE no recorte corrigido**: harness exige simultaneamente rc≠0 e `FALHA (<caso>)`; guarda n≤N e casos `?`/K fora do intervalo existem. (`bin/tests/test_etapas_no_brief.sh:446`, `bin/disjuntor.py:1090`, `bin/tests/test_etapas_no_brief.sh:397`, `bin/tests/test_etapas_no_brief.sh:403`, `bin/tests/test_etapas_no_brief.sh:512`)

| Reancoragem | Mesma propriedade por inspeção |
|---|---|
| abertura-sem-janela | Sim: retira apenas limite inferior dos 600s; fixture G tem aprovação uma hora anterior ao ini e deve aumentar “sem”. (`bin/tests/test_censo_antes_da_vaga.sh:723`, `bin/tests/test_censo_antes_da_vaga.sh:470`, `bin/tests/test_censo_antes_da_vaga.sh:480`, `bin/tests/test_censo_antes_da_vaga.sh:486`) |
| sem-consumo-unico | Sim: permite reaproveitar aprovação para dois ini da mesma chave; fixture/asserções exigem com=1, sem=1. (`bin/tests/test_censo_antes_da_vaga.sh:731`, `bin/tests/test_censo_antes_da_vaga.sh:503`, `bin/tests/test_censo_antes_da_vaga.sh:507`, `bin/tests/test_censo_antes_da_vaga.sh:510`) |
| ini-do-futuro | Sim: retira somente limite superior; ini T futuro passa a casar e viola com=1. (`bin/tests/test_censo_antes_da_vaga.sh:747`, `bin/tests/test_censo_antes_da_vaga.sh:504`, `bin/tests/test_censo_antes_da_vaga.sh:507`, `bin/tests/test_censo_antes_da_vaga.sh:511`) |
| pertenca-wrapper-propria | Sim: troca pertencimento decidido pelo teste da célula crua; obra qualificada volta a FORA. (`bin/tests/test_censo_antes_da_vaga.sh:753`, `bin/tests/test_censo_antes_da_vaga.sh:535`, `bin/tests/test_censo_antes_da_vaga.sh:536`, `bin/disjuntor.py:2693`) |

São três reancoragens para `_casa_wrapper_ini` e uma para `if dentro is False:`. (`bin/tests/test_censo_antes_da_vaga.sh:723`, `bin/tests/test_censo_antes_da_vaga.sh:731`, `bin/tests/test_censo_antes_da_vaga.sh:747`, `bin/tests/test_censo_antes_da_vaga.sh:753`)

**PROCEDENTE — cobertura incompleta da classe fotografia:** retirar a aceitação da fotografia Agent em `_fotografias` não encontra caso que confira esse N no B3 ou `--etapas`; os positivos abrem pelo wrapper fabricando ini, enquanto os testes Agent verificam escrita/negação. Cura: positivo ponta a ponta com única fotografia Agent, mais mutante removendo sua ingestão. Sobrevivência executada [nao verificado]. (`bin/disjuntor.py:958`, `bin/tests/test_etapas_no_brief.sh:67`, `bin/tests/test_etapas_no_brief.sh:77`, `bin/tests/test_etapas_no_brief.sh:148`, `bin/tests/test_etapas_no_brief.sh:153`, `bin/tests/test_etapas_no_brief.sh:176`, `bin/tests/test_etapas_no_brief.sh:359`)

**PROCEDENTE — T1 não certificado na classe:** o caso usa PIDs distintos e fabrica ini; não disputa o flock real. Cura: identidade única da abertura e casos mesma chave/PID reciclado, duas aberturas no mesmo segundo, relógio invertido e carimbo concorrente. (`bin/tests/test_etapas_no_brief.sh:72`, `bin/tests/test_etapas_no_brief.sh:320`, `bin/tests/test_etapas_no_brief.sh:322`, `bin/tests/test_etapas_no_brief.sh:323`, `bin/disjuntor.py:931`, `bin/disjuntor.py:969`)

**FILA — harness vizinho herdado:** `test_censo_antes_da_vaga` continua aceitando qualquer rc≠0, sem exigir a falha específica. As quatro propriedades permanecem verificáveis por inspeção; morte executada pelo motivo certo [nao verificado]. (`bin/tests/test_censo_antes_da_vaga.sh:685`)

Casos `?` e K fora do intervalo podem passar antes da cura e continuar úteis como controles/matadores dos mutantes adicionados; isso sozinho **NAO PROCEDE** como defeito. (`bin/tests/test_etapas_no_brief.sh:397`, `bin/tests/test_etapas_no_brief.sh:403`, `bin/tests/test_etapas_no_brief.sh:513`, `bin/tests/test_etapas_no_brief.sh:514`)

RED 45, GREEN e mortes dos 38: **[nao verificado]**; não executei suites nem consultei logs de produção. [nao verificado]

## 09/11 12:24 · sessao eab8c15e7201 · FIO-E0-V4-E3 · verificador · origem=Code

**Há achados PROCEDENTES de unidade residual, custo e leitura indireta na sombra; os quatro consumidores principais estão unificados.** (`app/chamados/services/aviso_pendencia.py:13`; `app/chamados/juizes.py:1149`; `app/core/utils.py:18`; `app/api/views.py:325,2562`; `app/api/views_core.py:314`; `app/core/views.py:253`)

1. **UNIDADE — NAO PROCEDE nos quatro consumidores; PROCEDENTE no restante do app.**

   Badge dos dois kernels, dashboard e `api_disputa_perguntas` leem `consulta_do_app`. Ela aplica C6 e separa pendentes por **sem resposta + disputa aberta**. Fechada viva vai ao `historico`; só entra também em `respondidas` quando possui `respondida_em`. (`app/chamados/services/pergunta_no_app.py:54,67`; `app/api/views.py:2588,2600`)

   Permanecem fontes paralelas:

   - **PROCEDENTE:** aviso após mensagem recompõe C6 + sem resposta, incluindo fechada viva, e informa “AGUARDANDO sua resposta”. A cobrança reutiliza esse leitor. **Cura:** ambos consumirem `consulta_do_app()['pendentes']`. (`app/chamados/services/aviso_pendencia.py:9,13,21`; `app/api/views.py:1013,2236`; `app/chamados/services/cobranca.py:47`)
   - **PROCEDENTE:** `api_chamados_v2` carrega todas as perguntas das disputas abertas, sem C6, e entrega outra lista em `perguntas_disputa`. Pode servir pergunta morta. **Cura:** consumir a seleção/classificação central. (`app/api/views.py:1044,1059,1174`)
   - **FILA — universos internos distintos:** placar/copiloto e `_no_app` continuam considerando todas as C6 vivas, inclusive fechadas agora disponíveis apenas no histórico. Definir se histórico satisfaz “pergunta no app”. (`app/chamados/juizes.py:1489`; `app/chamados/services/pergunta_no_app.py:160,183`; `bin/placar_code.sh:317`)
   - **FILA — dívida preexistente:** há contagens por carimbo cru na cobrança, ações da disputa, censo da folha e contador com cron. Não certificam pendência apresentada pelo app. (`app/chamados/services/cobranca.py:229`; `app/chamados/services/acoes_disputa.py:95`; `bin/censo_folha.py:28`; `app/chamados/services/furo_sem_canal.py:80`; `app/config/crons.py:531`)

   Fechada viva também pode aparecer no fio administrativo e seus templates: esse caminho aplica C6 às fechadas; o fio do colaborador usa outro recorte. (`app/chamados/services/fio.py:216,238,253`; `app/templates/chamados/partials/modal_fio.html:156`)

2. **JUIZ ÚNICO C17 — NAO PROCEDE a acusação de cópia da partição ou ligação antecipada.**

   A tarefa chama `alimentar_fila`, que chama as autoridades existentes `particionar_verbos(qs_vivos(), agora=...)` e `qs_na_fila(agora=...)`. A partição chama `creditos_perguntas` e `verbo_do_chamado`; `_portas_da_tarefa` compõe portas a partir das ações já julgadas pelo C19. (`app/chamados/services/tarefa_do_dia.py:409,425`; `app/chamados/juizes.py:1429,1432`; `app/chamados/services/verbos.py:34,47`; `app/core/juizes.py:291`)

   C17 **ainda não virou fachada delegante**: conserva seu julgamento. Os três chamadores permanecem folha, vigia e cadastro; seus corpos não constam como alterados no diff fornecido. Comparação histórica independente com a base: **[nao verificado]**. (`app/chamados/juizes.py:1545`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:488`)

   Os campos antigos da tarefa são preservados; entram quatro chaves aditivas. A igualdade do dicionário inteiro muda, mas não foi localizado consumidor anterior de produção sendo migrado; o novo consumidor é o DIFF. Chamadas dinâmicas não localizáveis: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:177,433`; `app/chamados/management/commands/diff_e3_sombra.py:118`)

3. **L-111 — NAO PROCEDE a permanência do método.**

   Método removido, lápide no sítio; busca AST/textual não encontrou chamada, alias importado, `getattr` literal ou template restante. Construção dinâmica do nome: **[nao verificado]**. (`app/chamados/models.py:1795`; `app/chamados/tests/test_badge_disputa_pendente.py:8`)

   Os testes migrados ainda exigem: zero sem filhos, duas perguntas → duas pendências, desaparecimento após absolvição e concordância entre endpoint, dashboard e dois kernels. (`app/chamados/tests/test_esmeril2_left_join.py:19,27`; `app/chamados/tests/test_fio_le_o_juiz.py:267,272`; `app/chamados/tests/test_e3_app_orm.py:28,39,47`)

4. **SOMBRA — PROCEDENTE a objeção à cadeia estritamente só de leitura.**

   O comando não contém escritor ORM direto e envolve todos os aliases na guarda SQL. Porém `dias_trancados` chega a `get_user_sistema`, que executa **`get_or_create`**. Se o usuário faltar, haverá tentativa de criação; a guarda impede que o SQL escritor alcance o cursor. **Cura:** resolver por leitura `.get`, falhar explicitamente na ausência e testar esse ramo; a fixture atual sempre cria `sistema`. (`app/chamados/management/commands/diff_e3_sombra.py:34,92,108`; `app/chamados/services/tarefa_do_dia.py:370`; `app/chamados/juizes.py:1033,1146`; `app/core/utils.py:18`; `app/chamados/tests/e3_fixtures.py:26`)

   **PROCEDENTE, no caminho de erro:** C6 pode registrar exceção/traceback; a configuração SaaS inclui arquivo de log. A sanitização final do comando não desfaz essa emissão. **Cura:** tratamento sanitizado durante o instrumento e teste das dependências. Configuração efetiva e ocorrência em produção: **[nao verificado]**. (`app/chamados/juizes.py:1366,1377`; `app/core/observ.py:79`; `app/config/settings/saas.py:138,183`; `app/chamados/management/commands/diff_e3_sombra.py:98`)

   **NAO PROCEDE — agendamento:** `FORA_DE_PIPELINE` apenas declara; geração do cron percorre `CRONS`, e inventário executável reúne `CRONS + ENCADEADOS`. O diagrama apenas baixa tela de 110 para 109 sítios; não liga fachada nem acrescenta execução. (`app/config/crons.py:769,1798,1865`; `app/docs/ARQUITETURA.mmd:356`; `app/docs/MAPA.md:209`)

5. **REGISTRO DE TELA — NAO PROCEDE novo juiz ou alteração adicional nesse arquivo.**

   No diff fornecido, `core/juizes.py` perde somente o pendente do dashboard; C17 e C19 continuam registrados. A leitura curada está no dashboard. (`app/core/juizes.py:289,291,756`; `app/core/views.py:253`)

   O selo de tela sozinho verifica impressões remanescentes, portanto não prova a cura funcional. O mérito vem da matriz ORM que exige duas perguntas na mesma disputa → duas pendências nos consumidores completos. (`app/core/tests/test_contract_juiz_tela.py:34`; `app/chamados/tests/test_e3_app_orm.py:28,39,47`)

6. **CLASSES DE REPROVAÇÃO.**

   - **PROCEDENTE — catraca incompleta:** resolve aliases de importação, mas não `p = porta_alcancavel_do_dia` nem `getattr(j, 'porta_alcancavel_do_dia')(...)`; também exclui `models.py` e `core/juizes.py`. Esses desvios escapam da contagem. **Cura:** ampliar resolução/escopo e acrescentar mutantes dessas formas. Isso não demonstra quarto chamador atual. (`app/chamados/tests/test_e3_pendentes_app.py:49,67,98,113`)
   - **NAO PROCEDE — quatro casos sem mordida:** há expectativas próprias para só fila, A/B, verbos do universo e isento, com mutantes correspondentes. O verbo inventado encontra ainda expectativa explícita de `validar`. Nem todo teste novo precisa falhar sem a cura: o contrato preservado de C17 é regressão. Causa individual das 30 mortes apenas por `rc=1`: **[nao verificado]**. (`app/chamados/tests/test_e3_portas_tarefa.py:32,40,49,57,75`; `app/chamados/tests/e3_mutantes.py:49,127`; `app/chamados/tests/test_e3_pendentes_app.py:126`)
   - **NAO PROCEDE — nova inversão de travas:** a extensão escreve mapas em memória, sem introduzir aquisição de trava; a tentativa escritora indireta está no item 4. (`app/chamados/juizes.py:1428`; `app/chamados/services/tarefa_do_dia.py:425`)
   - **FILA — relógio integral da comparação:** o comando recebe instante explícito e o propaga, mas C17 ainda chama C6, cujo fallback lê `date.today()`. Portanto `--agora` não controla toda a comparação histórica. **Cura:** explicitar esse limite ou alimentar o horizonte dessa dependência antes de alegar reprodução integral. (`app/chamados/management/commands/diff_e3_sombra.py:84,107`; `app/chamados/juizes.py:1472,1357,575`)
   - **NAO PROCEDE — ambiguidade nova demonstrada:** tarefa exclui conversa ao agrupar `data_evento`; conversa continua gravando dia civil, fato usa dia do turno, e `data_civil` da instância deriva do instante do marco. Silêncio usa `> agora`; igualdade retorna à fila/histórico. Decisões e efeitos usam `<= agora`; janela de dias inclui as duas pontas. (`app/chamados/models.py:2786,2798`; `app/chamados/services/tarefa_do_dia.py:102,325,339,358`; `app/chamados/services/painel.py:190`; `app/chamados/services/verbos.py:51`; `app/chamados/management/commands/diff_e3_sombra.py:114`)

   `rota='porta_fila'` pode coexistir com `fila=()` quando o verbo é histórico: rota usa a âncora exigível na célula; `fila` exclui verbos fora da lista padrão. O caso está explicitamente testado. (`app/chamados/services/tarefa_do_dia.py:428,429,433`; `app/chamados/tests/test_e3_portas_tarefa.py:109`)

7. **CUSTO — PROCEDENTE: não há número constante universal no badge.**

   A consulta inicial é uma, mas C6 consulta tranca por pergunta; competência é memoizada, toque negativo não. **Cura:** alimentar tranca/toque em lote por chamada, preservando frescor entre chamadas. (`app/chamados/services/pergunta_no_app.py:47,54`; `app/chamados/juizes.py:948,1020,1137,1151`)

   Contagem estática incremental de `consulta_do_app`, não de `api_me` inteiro:

   | Cenário | SELECTs |
   |---|---:|
   | Nenhuma pergunta candidata | **1** (`pergunta_no_app.py:47`) |
   | Sem resposta, célula acusando, competências abertas | **1 + E + K** (`juizes.py:1003,1023,1342`) |
   | N perguntas, uma competência trancada, nenhum toque humano, células/chamados existentes, `sistema` existente e memo frio | **3 + E + 5N** (`juizes.py:1146,1185,1200,1211,1224,1234`) |

   `E=1` se Empresa precisar ser carregada; `K` é o número de competências ainda não memoizadas. Empresa fria no último cenário: **4 + 5N**. Outros ramos de C6 variam; quantidade observada em produção: **[nao verificado]**. (`app/chamados/juizes.py:1003,1020,1361,1374`)

8. **QUEM ESCREVE / QUEM LÊ — rastro principal, inclusive fora do diff.**

   | Campo/função | Produtor/escritor | Leitores |
   |---|---|---|
   | `pendentes/historico` | `pergunta_no_app.py:65` — memória | `api/views.py:325,2562`; `api/views_core.py:314`; `core/views.py:253` |
   | `respondida_em/resposta_colab` | `disputa_emissao.py:1464,1650,1671`; reabertura `:1590,1629`; carimbo `models.py:3121` | Consulta `pergunta_no_app.py:67`; C6 `juizes.py:1309`; crédito `raiox.py:139` |
   | `fechada_em` | Fecha `models.py:1929`; reabre `:2146,2170` | Consulta `pergunta_no_app.py:67`; fio `fio.py:216`; lista paralela `api/views.py:1046` |
   | `validada_em/materializada_em/via_resolucao` | `validacao.py:134,199`; `models.py:3088,3179,3253` | C6 `juizes.py:1293,1304,1330`; crédito `raiox.py:135` |
   | Âncora/data da pergunta | Nascimento `models.py:2786,2872`; portas `fio.py:804,970` | `chamado_que_lista`, `fio.py:116`; tarefa `tarefa_do_dia.py:329,362`; C17 `juizes.py:1549` |
   | `veredito_lavrado/julgado_em/janela_expira_em` | `veredito_lavrado.py:250,270` | Crédito `raiox.py:135`; tarefa `tarefa_do_dia.py:350` |
   | Decisões, `ato_em`, efeitos | Sequência `models.py:3934`; resposta/validação `disputa_emissao.py:1653`, `validacao.py:139` | Tarefa `tarefa_do_dia.py:337,346,358` |
   | `verbo/fila_ids/fila_cel` | `juizes.py:1429` — memória, autoridades painel/partição | `juizes.py:1465`; `tarefa_do_dia.py:409,426` |
   | `rota/fila/verbos/fio` | Tarefa `tarefa_do_dia.py:433`; C17 separado `juizes.py:1556` | DIFF `diff_e3_sombra.py:43,50`; atuais C17: `fora_da_folha.py:78,225`, `vigia_fio.py:643`, `cadastro_realidade.py:488` |
   | `status_local/silenciado_ate` | `models.py:228,296,1095`; `services/arquivo.py:166,202` | `painel.py:155,189`; `verbos.py:45,51` |
   | `modulo_origem/urgencia/criado_em` | `models.py:1095,550,67` | `verbos.py:45,47,57,60` |
   | `dia_do_turno/agora/data_civil` | Entrada `tarefa_do_dia.py:442`; parsing `diff_e3_sombra.py:83`; saída `tarefa_do_dia.py:102,177` | Carga/horizontes `tarefa_do_dia.py:317,339,358,411`; partição `verbos.py:51` |

   Caminhos dessa tabela são relativos a `app/`. Censo recursivo exaustivo de **todos** os escritores/leitores dinâmicos de cada insumo transitivo: **[nao verificado]**. DIFF medido por chamador/caso em produção e liberação da fachada: **[nao verificado]**.

_3154 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
