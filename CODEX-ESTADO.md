# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 13:25** · publicado em **07/11/2026 13:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 13:25 · sessao 8e6391171c0e · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

**Não liberaria esta volta:** o inventário ainda deixa passar hardlink idêntico na revalidação/final, e dois mutantes ficam vermelhos sem demonstrar a proteção alegada. (`bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:158`, `bin/tests/test_codex_pergunta.sh:393`, `bin/tests/test_codex_pergunta.sh:396`)

1. **Informativo — fechamento dos achados do opus fornecido.**

   | Achado | Aval desta volta |
   |---|---|
   | `.git` aninhado invisível | **Fecha os contraexemplos apresentados:** o percurso encontra diretórios e arquivos `.git`, e a classificação os proíbe. Persistem limites gerais da varredura abaixo. (`bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:135`, `bin/codex_pergunta.sh:154`) |
   | Selo só criava `.gitignore` | **Fecha em parte:** acrescenta `.git/config`, diretório aninhado e arquivo `gitdir`, mas há mortes de mutantes pelo motivo errado. (`bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:295`, `bin/tests/test_codex_pergunta.sh:396`) |
   | Ignorados/autoload saíam `TOCADO` | **Fecha a classificação dos casos apresentados:** agora recebem motivo proibitivo; a execução de autoload pelo próprio processo inventariante continua `[nao verificado]`. (`bin/codex_pergunta.sh:107`, `bin/codex_pergunta.sh:155`, `bin/codex_pergunta.sh:393`) |
   | Vaga pela raiz do script | **Fecha mantendo o mesmo `CODEX_VIVO`:** copiar `bin/` já não muda a vaga. **Parcial como exclusão por repositório**, conforme item 4. (`bin/codex_pergunta.sh:71`, `bin/tests/test_codex_pergunta.sh:314`) |
   | Prova baseada no `.out` do Codex | **Fecha em parte:** TCP local/Unix ganham ouvinte externo; Docker, docker.sock e TCP externo continuam autodeclarados. (`bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:104`, `bin/tests/prova_codex_pergunta_real.sh:117`, `bin/tests/prova_codex_pergunta_real.sh:124`) |
   | Texto “só LE”/“ÚNICA escrita” | **Fecha:** distingue quatro papéis de leitura e duas permissões de escrita. (`CLAUDE.md:341`, `CLAUDE.md:346`) |
   | Opus/teto apresentados como guarda | **Fecha a rotulagem:** declara expressamente que são conduta, sem guarda no wrapper. (`CLAUDE.md:351`, `CLAUDE.md:352`) |

2. **Alta — hardlink idêntico escapa durante a espera e no inventário final.**

   Substituir um arquivo rastreado por hardlink com os mesmos bytes e modo não o inclui em `tocados`; a consulta a `st_nlink` acontece somente depois, dentro desse conjunto. O `find -links +1` inicial não se repete na revalidação. Assim, a troca durante a espera passa pela nova guarda; no final também pode produzir inventário vazio. A simulação em memória confirmou rc 0 e saída vazia; reprodução em filesystem real: `[nao verificado]`. (`bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:152`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:220`, `bin/codex_pergunta.sh:366`)

3. **Média — o inventário não cobre integralmente entradas, identidade e permissões.**

   | Classe | Resultado da leitura |
   |---|---|
   | Permissão de arquivo | Só compara execução pelo proprietário. `0644→0666` ou `0644→0600` pode desaparecer do inventário; a simulação de `0666` deu rc 0 vazio. (`bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:143`) |
   | Diretório existente | Não compara suas permissões; só verifica se o caminho pertence às pastas da base. (`bin/codex_pergunta.sh:139`) |
   | FIFO/socket/dispositivo novo | É reportado, mas pode sair **`TOCADO`, rc 0**, por faltar proibição por tipo. FIFO foi confirmado em memória. (`bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:153`, `bin/codex_pergunta.sh:160`) |
   | Diretório vazio novo | É reportado como `caminho/`; sob caminho permitido, é aceito. Diretórios vazios não têm representação nessa base `ls-tree`. (`bin/codex_pergunta.sh:125`, `bin/codex_pergunta.sh:140`) |
   | Symlink novo/modificado | Entra na comparação e recebe `PROIBIDO`. Symlink já rastreado e inalterado não recebe essa classificação. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:157`) |
   | Bytes não UTF-8 | Git usa `"replace"`, enquanto o percurso conserva outra representação dos bytes. Pode duplicar caminhos e recusar arquivo rastreado limpo. Destino não UTF-8 de symlink pode falhar no `.encode()`. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:122`, `bin/codex_pergunta.sh:128`) |
   | Case | A comparação é literal, sem conversão de caixa. Comportamento em filesystem que ignore caixa: `[nao verificado]`. (`bin/codex_pergunta.sh:128`, `bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:143`) |
   | Ramo não percorrido | `os.walk` não recebe `onerror`; uma abertura recusada pode omitir descendentes novos sem produzir `FALHA`. Rastreado não visto vira remoção, mas isso não enumera os novos ocultos. (`bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:146`) |

   Há também recusa potencial de checkout legítimo com transformação CRLF/smudge: compara bytes físicos diretamente ao blob armazenado. Configuração ou ocorrência disso nesta obra: `[nao verificado]`. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:125`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:217`)

   A poda da raiz exclui `.git` independentemente do tipo; a exigência inicial “arquivo sem symlink” não é repetida no final, que compara conteúdo. Aceitação real pelo Git de uma troca por symlink de conteúdo idêntico: `[nao verificado]`. (`bin/codex_pergunta.sh:135`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:394`)

4. **Alta — a vaga ainda depende da árvore escolhida como `CODEX_VIVO`.**

   Dois valores de `CODEX_VIVO` apontando para worktrees distintas do mesmo repositório produzem dois caminhos de vaga. A validação de common-dir confere a origem com o destino de cada chamada, mas não deriva a vaga desse common-dir. Portanto, o delta resolve a cópia do script com origem fixa; não estabelece exclusão única para todas as árvores do mesmo repositório. Execução concorrente desse cenário: `[nao verificado]`. (`bin/codex_pergunta.sh:56`, `bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:200`, `bin/codex_pergunta.sh:212`)

5. **Alta — a prova ainda pode ficar verde sem executar cada tentativa.** [nao verificado]

   O controle positivo exige apenas `INTRUSO_WT.txt`; a evidência individual continua sendo `.out` gravado pelo próprio Codex. Preencher esses arquivos com `rc=` e negativas fabricadas satisfaz as verificações documentais; TCP local/Unix nem exigem assinatura de negação. Zero conexões prova ausência de conexão registrada, não execução de tentativa bloqueada. Contraexemplo completo executado: `[nao verificado]`. (`bin/tests/prova_codex_pergunta_real.sh:60`, `bin/tests/prova_codex_pergunta_real.sh:90`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:105`, `bin/tests/prova_codex_pergunta_real.sh:107`)

   Os rótulos `AUTO-DECLARADO` de Docker/socket/TCP externo não tornam o resultado inconclusivo: essas negativas ainda podem terminar com rc 0. (`bin/tests/prova_codex_pergunta_real.sh:82`, `bin/tests/prova_codex_pergunta_real.sh:117`, `bin/tests/prova_codex_pergunta_real.sh:120`, `bin/tests/prova_codex_pergunta_real.sh:125`, `bin/tests/prova_codex_pergunta_real.sh:135`)

6. **Média — o ouvinte não tem controle posterior e `$EV` pode colidir.**

   O script testa o ouvinte antes da chamada; depois só compara contagens. Não confirma sobrevivência nem faz controle posterior. Se ele morrer após os controles, ainda pode imprimir “OK MEDIDO”. (`bin/tests/prova_codex_pergunta_real.sh:44`, `bin/tests/prova_codex_pergunta_real.sh:49`, `bin/tests/prova_codex_pergunta_real.sh:103`, `bin/tests/prova_codex_pergunta_real.sh:105`)

   `$EV` usa precisão de segundos e `mkdir -p`; chamadas no mesmo segundo podem compartilhar socket, porta e evidências. Além disso, `CONECTOU` no `.out` ainda reprova independentemente do ouvinte, apesar do comentário que lhe atribui o julgamento. Ocorrências reais: `[nao verificado]`. (`bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:34`, `bin/tests/prova_codex_pergunta_real.sh:45`, `bin/tests/prova_codex_pergunta_real.sh:92`, `bin/tests/prova_codex_pergunta_real.sh:125`)

7. **Média — dois mutantes morrem por motivo diferente do declarado.**

   - **`fs_walk`:** remover o percurso também afeta o pre-check. Todos os blobs ficam “não vistos”, a chamada é recusada com rc 2 antes do falso Codex, e o caso fica vermelho porque esperava rc 3 final. Não demonstra a mordida final contra `.git/config`. (`bin/tests/test_codex_pergunta.sh:393`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:217`, `bin/tests/test_codex_pergunta.sh:105`)
   - **`git_aninhado`:** remove apenas o motivo suplementar; `.git*` continua proibindo o caminho. Morre pela ausência da mensagem exigida, mantendo a proteção. (`bin/tests/test_codex_pergunta.sh:396`, `bin/codex_pergunta.sh:103`, `bin/codex_pergunta.sh:154`, `bin/tests/test_codex_pergunta.sh:106`)
   - **Sentinela FSMON:** o caso planta configuração, mas não exercita o hook com Git no diretório plantado. Prova recusa do caminho; a ausência da sentinela não prova bloqueio de execução do hook. (`bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:292`, `bin/codex_pergunta.sh:110`)

   Os mutantes `poda_so_raiz` retiram justamente a visibilidade de `.git` em diretório e arquivo; não encontrei a mesma morte espúria neles. Resultado executado de todos os mutantes: `[nao verificado]`. (`bin/tests/test_codex_pergunta.sh:295`, `bin/tests/test_codex_pergunta.sh:296`, `bin/tests/test_codex_pergunta.sh:394`, `bin/tests/test_codex_pergunta.sh:395`)

8. **Informativo — escritores e leitores pedidos.**

   | Arquivo/variável | Escritor → leitor |
   |---|---|
   | `VAGA` | Shell deriva de `VIVO` → reserva da resposta e abertura da trava. (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:212`) |
   | `construtor.vaga` | Wrapper cria/abre em append → `flock` usa fd 6. O selo também cria, prende, move e restaura sua fixture. (`bin/codex_pergunta.sh:212`, `bin/codex_pergunta.sh:213`, `bin/tests/test_codex_pergunta.sh:175`, `bin/tests/test_codex_pergunta.sh:217`) |
   | `_INVENTARIO_PY` | Uma definição no shell → três chamadas Python. Não são três implementações independentes do juiz. (`bin/codex_pergunta.sh:90`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:366`, `bin/codex_pergunta.sh:393`) |
   | `_INV0` | Captura stdout/stderr do Python → exige sucesso e saída vazia. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:218`) |
   | `_INV1` | Captura stdout/stderr, acrescentando `FALHOU` em erro → exige saída vazia. (`bin/codex_pergunta.sh:366`, `bin/codex_pergunta.sh:369`) |
   | `.inventario`/`.recusa` | Python escreve inventário; shell acrescenta guarda de `.git` e gera recusa → testes conferem recusa e a prova copia evidências. (`bin/codex_pergunta.sh:393`, `bin/codex_pergunta.sh:394`, `bin/codex_pergunta.sh:398`, `bin/tests/test_codex_pergunta.sh:106`, `bin/tests/prova_codex_pergunta_real.sh:80`) |
   | `$EV/escuta` | Shell cria/inicia processo; ouvinte publica `porta` e acrescenta `conexoes` → shell lê porta e contagens; trap encerra o ouvinte. (`bin/tests/prova_codex_pergunta_real.sh:29`, `bin/tests/prova_codex_pergunta_real.sh:35`, `bin/tests/prova_codex_pergunta_real.sh:41`, `bin/tests/prova_codex_pergunta_real.sh:44`, `bin/tests/prova_codex_pergunta_real.sh:103`) |
   | `$EV/p` e `prova/*.out` | Shell escreve pergunta → wrapper lê; Codex é instruído a escrever `.out` → prova copia e julga. (`bin/tests/prova_codex_pergunta_real.sh:53`, `bin/tests/prova_codex_pergunta_real.sh:61`, `bin/tests/prova_codex_pergunta_real.sh:77`, `bin/tests/prova_codex_pergunta_real.sh:81`, `bin/tests/prova_codex_pergunta_real.sh:89`) |
   | Texto do `CLAUDE.md` | Autor da edição: `[nao verificado]`. Selo lê nomes/ponteiros; hashes cobrem blocos de **PISTA**, não as frases alteradas do item 1. O caso `c_p_claudemd` escreve outra cópia na fixture. (`bin/tests/test_claude_md_regras_preservadas.sh:44`, `bin/tests/test_claude_md_regras_preservadas.sh:72`, `bin/tests/test_claude_md_regras_preservadas.sh:83`, `bin/tests/test_codex_pergunta.sh:259`) |

9. **Informativo — classes históricas e papéis de leitura.**

   Aparecem **RED sem mordida pelo motivo alegado** e **fragmentação da exclusão de escritor**, conforme itens 4 e 7. Também há diferença material entre “blob Git” e “arquivo físico”, conforme item 3. Não identifiquei no delta clone de juiz de negócio registrado, relógio de negócio fora da borda, inversão nova da ordem de travas ou operação de pouso; a vaga precede slot, trava da resposta e revalidação. (`bin/codex_pergunta.sh:96`, `bin/codex_pergunta.sh:213`, `bin/codex_pergunta.sh:324`, `bin/codex_pergunta.sh:349`, `bin/codex_pergunta.sh:366`)

   Não identifiquei regressão no sandbox ou execução do inventário nos quatro papéis de leitura: permanecem no ramo `read-only`, com testes para argv, diretório e cabeçalho. A alteração compartilhada é a nova reserva do caminho da vaga. Execução efetiva desses testes: `[nao verificado]`. (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:381`, `bin/tests/test_codex_pergunta.sh:327`)

10. **Informativo — limite da verificação.**

    A suite e a prova real não foram executadas: criam arquivos e a prova chama o Codex. Resultados completos desta volta, isolamento real e dados de produção: `[nao verificado]`. As confirmações mencionadas foram simulações exclusivamente em memória do inventário. (`bin/tests/test_codex_pergunta.sh:18`, `bin/tests/test_codex_pergunta.sh:19`, `bin/tests/prova_codex_pergunta_real.sh:21`, `bin/tests/prova_codex_pergunta_real.sh:77`)

## 07/11 13:23 · sessao cd2630cc4e18 · - · - · origem=Ronald (tmux)

1. **Alta — hardlink idêntico escapa da revalidação e do inventário final.** `st_nlink` só é consultado para caminhos já em `tocados`; substituir um arquivo rastreado por hardlink de mesmos bytes/modo não o inclui nesse conjunto. O `find -links +1` ocorre apenas inicialmente. Simulação em memória do código original: `nlink=2`, mesmo blob → rc 0, saída vazia. (`bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:220`, `bin/codex_pergunta.sh:366`, `bin/codex_pergunta.sh:393`)

2. **Média — permissões não são inventariadas integralmente.** Só o bit executável do proprietário entra em `modo`; `0644→0666`, `0644→0600`, setuid/setgid e permissões de diretórios existentes ficam fora da comparação. Simulação em memória: `0644→0666` → rc 0, saída vazia. ACL/xattrs também não são lidos pelo inventário. (`bin/codex_pergunta.sh:114`, `bin/codex_pergunta.sh:116`, `bin/codex_pergunta.sh:139`, `bin/codex_pergunta.sh:143`)

3. **Média — FIFO/socket/device são reportados, mas aceitos.** Tipos não regulares entram em `tocados`, porém não recebem motivo por tipo especial. Simulação: FIFO novo `app/pipe` → `TOCADO`, rc 0. Diretório vazio novo também é reportado e aceito sob caminho permitido; permissões de diretórios existentes não são reportadas. (`bin/codex_pergunta.sh:139`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:153`, `bin/codex_pergunta.sh:160`)

4. **Média — varredura incompleta pode não ser tratada como falha.** `os.walk(wt)` não recebe `onerror`; erro de abertura de diretório pode omitir sua descendência. Blobs rastreados não vistos aparecem como removidos, mas novas entradas ocultas naquele ramo não são individualmente reportadas. Condições efetivas de permissão/Git em produção: `[nao verificado]`. (`bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:146`)

5. **Média — nomes legítimos não UTF-8 recebem tratamento incompatível.** Git é decodificado com `"replace"`; o percurso de filesystem usa nomes reais, com outra representação para bytes inválidos. Um arquivo rastreado limpo pode aparecer simultaneamente novo/removido e reprovar o pre-check. Destino de symlink com bytes inválidos pode falhar no `.encode()` e produzir `FALHA`. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:122`, `bin/codex_pergunta.sh:128`, `bin/codex_pergunta.sh:132`, `bin/codex_pergunta.sh:217`)

6. **Média — Git limpo não implica blob físico idêntico.** Comparar bytes físicos diretamente com `ls-tree` recusa checkout legítimo transformado por CRLF/smudge/encoding. Não encontrei `.gitattributes` nesta cópia; configuração efetiva do Git, filtros ou ocorrência concreta: `[nao verificado]`. (`bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:125`, `bin/codex_pergunta.sh:143`, `bin/codex_pergunta.sh:217`)

7. **Média — identidade de tipo de `.git` não é repetida.** A poda exclui `.git` da raiz tanto de `dirs` como de `arqs`; pre-check exige arquivo sem symlink, mas revalidação/final comparam conteúdo e resolução Git. Troca posterior por symlink de mesmos bytes não tem guarda explícita de tipo. Aceitação desse cenário pelo Git real: `[nao verificado]`. (`bin/codex_pergunta.sh:135`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:368`, `bin/codex_pergunta.sh:394`)

8. **Alta, condicional — o próprio inventário pode carregar autoload antes de recusá-lo.** As chamadas `python3 -c` não usam isolamento nem mudam o diretório corrente. Se o wrapper for iniciado em `WT/app`, um `sitecustomize.py` ali plantado pode executar na inicialização do Python do host antes da regra de proibição. Ambiente efetivo de inicialização Python: `[nao verificado]`. (`bin/codex_pergunta.sh:107`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:366`, `bin/codex_pergunta.sh:393`)

## 07/11 13:22 · sessao 6609b154c603 · - · - · origem=Ronald (tmux)

1. **Média — mutante `fs_walk` morre antes do comportamento que pretende provar.** A substituição desliga a varredura também no pre-check; todos os blobs ficam “não vistos”, gerando inventário não vazio e recusa rc 2 antes do Codex. O caso espera rc 3 final, portanto será contado como morto por antecipação: `bin/tests/test_codex_pergunta.sh:393`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:217`, `bin/tests/test_codex_pergunta.sh:105`. Execução efetiva da suite: `[nao verificado]`.

2. **Média — mutante `git_aninhado` mede mensagem, mantendo a proteção.** Remove somente o motivo suplementar `.git aninhado`; a regex `.git*` continua produzindo PROIBIDO e rc 3. O caso fica vermelho porque exige aquela substring: `bin/tests/test_codex_pergunta.sh:396`, `bin/codex_pergunta.sh:103`, `bin/codex_pergunta.sh:154`, `bin/tests/test_codex_pergunta.sh:106`, `bin/tests/test_codex_pergunta.sh:295`.

3. **Baixa — sentinela FSMON sem controle positivo de execução do hook.** O caso planta `.git/config` e verifica ausência de FSMON, mas sua ação não chama Git após plantar; o inventário chama Git na raiz da WT. Prova recusa do caminho, sem provar execução potencial do hook: `bin/tests/test_codex_pergunta.sh:291`, `bin/tests/test_codex_pergunta.sh:292`, `bin/tests/test_codex_pergunta.sh:76`, `bin/codex_pergunta.sh:110`.

4. **Sem defeito identificado nos mutantes `poda_so_raiz`.** `if True` poda `.git` tanto de diretórios quanto de arquivos; os dois casos perdem justamente a entrada proibida e ficam com diretórios comuns TOCADO. A expectativa rc 3 deixa de ser satisfeita pelo motivo declarado: `bin/tests/test_codex_pergunta.sh:394`, `bin/tests/test_codex_pergunta.sh:395`, `bin/tests/test_codex_pergunta.sh:295`, `bin/tests/test_codex_pergunta.sh:296`, `bin/codex_pergunta.sh:135`. Execução efetiva: `[nao verificado]`.

5. **Leitor do CLAUDE: nomes e referências, sem guarda das frases alteradas.** O selo preserva nomes de regras, âncoras e hashes dos blocos **PISTA**; não compara literalmente o texto CODEX item 1 do CLAUDE. Logo, verde nesse selo não certifica “DUAS escritas” ou “CONDUTA”: `bin/tests/test_claude_md_regras_preservadas.sh:43`, `bin/tests/test_claude_md_regras_preservadas.sh:55`, `bin/tests/test_claude_md_regras_preservadas.sh:72`, `bin/tests/test_claude_md_regras_preservadas.sh:83`, `CLAUDE.md:346`, `CLAUDE.md:352`. Outros leitores examinados julgam seção 3, seção 0 e senha no argv: `bin/tests/test_suite_sh.sh:22`, `bin/tests/test_lei_akita.sh:24`, `bin/tests/test_lei_akita.sh:35`, `bin/tests/test_segredo_fora_do_argv.sh:33`.

6. **Leitura: cobertura estática preservada; resultado executado não verificado.** Existem casos para os quatro papéis verificando sandbox, cwd, cabeçalho e ausência de sinais do construtor; o verificador usa B3 falso sempre livre. Os testes de sonda cobrem falha do envelope e lista vazia. `bin/tests/test_codex_pergunta.sh:327`, `bin/tests/test_codex_pergunta.sh:337`, `bin/tests/test_codex_pergunta.sh:57`, `bin/tests/test_codex_sonda.sh:71`, `bin/tests/test_codex_sonda.sh:77`. Não executei: a suite cria e remove arquivos temporários, contrariando a restrição de leitura (`bin/tests/test_codex_pergunta.sh:18`, `bin/tests/test_codex_pergunta.sh:19`).

## 07/11 13:21 · sessao 5bdc20a26151 · - · - · origem=Ronald (tmux)

1. **Alta — achado da prova fechado em parte.** TCP local e Unix ganharam medição fora do sandbox: o ouvinte grava conexões e o script compara as contagens antes/depois. Docker, docker.sock e TCP externo continuam julgados pelos `.out` escritos pelo Codex; o delta apenas rotula essa negativa como `AUTO-DECLARADO`, permitindo ainda rc 0. Referências: `bin/tests/prova_codex_pergunta_real.sh:30`, `:41`, `:49`, `:52`, `:101`, `:103`, `:117`, `:120`, `:124`.

2. **Alta — arquivo com `rc=` não comprova execução da tentativa.** A condição aceita qualquer `.out` não vazio contendo `rc=`; TCP local/Unix dispensam inclusive assinatura de erro. Criar o controle positivo e preencher os `.out` com negativas fabricadas satisfaz essas condições sem executar as tentativas; os demais controles externos precisam estar disponíveis para o conjunto retornar 0. Referências: `bin/tests/prova_codex_pergunta_real.sh:60`, `:88`, `:90`, `:92`, `:93`, `:98`, `:106`, `:107`, `:135`. Execução desse contraexemplo: `[nao verificado]`.

3. **Média — zero conexões não é prova suficiente de bloqueio durante toda a chamada.** O script confirma disponibilidade do ouvinte antes do Codex, mas depois apenas lê a contagem; não verifica que o processo continua vivo nem realiza controle posterior ou sincronização do processamento. Se o ouvinte morrer após os controles, permanece possível imprimir `OK MEDIDO ... 0 conexao`. Referências: `bin/tests/prova_codex_pergunta_real.sh:44`, `:49`, `:52`, `:77`, `:101`, `:103`, `:105`. Ocorrência real: `[nao verificado]`.

4. **Média — dois julgamentos continuam ativos para TCP local/Unix.** O comentário atribui o julgamento ao ouvinte, mas `CONECTOU` no `.out` ainda impõe rc 1 independentemente da medição externa. Uma saída fabricada pode produzir “VERMELHO conexão abriu” com contagem externa zero. É duplicação de julgamento da prova; relação com autoridade registrada em `app/core/juizes.py`: `[nao verificado]`. Referências: `bin/tests/prova_codex_pergunta_real.sh:92`, `:104`, `:105`, `:124`, `:125`.

5. **Média — `$EV` não identifica exclusivamente a execução.** O nome usa data até segundos e `mkdir -p` reutiliza o diretório. Duas execuções no mesmo segundo compartilham socket, `porta`, `conexoes`, prompt e evidências; não há recusa de diretório preexistente. O segundo ouvinte pode falhar no `bind` e consumir `porta` do primeiro. Referências: `bin/tests/prova_codex_pergunta_real.sh:21`, `:29`, `:30`, `:34`, `:35`, `:45`, `:46`, `:53`, `:80`, `:81`. Reprodução: `[nao verificado]`.

6. **Informativo — escritores/leitores da prova.** O shell cria `$EV/escuta`, inicia e encerra o ouvinte; o Python escreve `porta.novo`, renomeia para `porta` e acrescenta `conexoes`; o shell lê `porta` e as contagens. O shell escreve `$EV/p`; o wrapper o lê. O Codex recebe instrução para escrever `<wt>/app/prova/*.out`, e o shell os copia/lê para produzir o veredito. Referências: `bin/tests/prova_codex_pergunta_real.sh:21`, `:29`, `:30`, `:35`, `:41`, `:44`, `:46`, `:52`, `:53`, `:61`, `:77`, `:81`, `:89`, `:103`, `:115`.

7. **Informativo — limites desta auditoria.** O ramo de leitura da prova permanece separado após o término do ramo construtor; o ouvinte está somente no construtor. Referências: `bin/tests/prova_codex_pergunta_real.sh:18`, `:135`, `:137`. Não executei o script, que cria arquivos e chama o wrapper (`:21`, `:53`, `:77`); resultados reais e mutantes executados: `[nao verificado]`.

## 07/11 12:59 · sessao a1c39fc87f85 · FIO-E0-V4-E2 · verificador · origem=Code

**A paridade no presente foi restaurada para o predicado de 57acffd2 informado no pedido. Permanece um conflito com a regra escrita: a mesma pergunta retratada pode voltar viva após ressurreição e resposta posterior.** ([seleção atual](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211), [regra de identidade](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2321))

Identidade Git da cópia, comparação direta com a árvore de 57acffd2, execução integrada Django, SQL medido e ocorrência produtiva dos estados abaixo: **[nao verificado]**.

1. **Classe: equivalência da seleção no presente — procedente quanto ao predicado fornecido.**

   Se `agora >= todos os atos`, nenhuma retratação alimenta `_retratada_depois`. Portanto, a seleção reduz-se exatamente a: excluir via em `VIAS_FORA_DO_FATO`; depois excluir última decisão `retratacao`. ([carga](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:321), [seleção](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211))

   Na tabela, cada célula vale **57 conforme descrito / atual**; “entra” significa passar pela seleção, antes das demais guardas da tarefa. As classes de sequência são exaustivas porque o predicado consulta somente sua existência e seu último ato. ([tarefa_do_dia.py:214](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:214))

   | Via atual | Sem decisões | Última = retratação | Sem retratação; última diferente | Retratação antiga; última diferente |
   |---|---|---|---|---|
   | `duplicata` | exclui / exclui | exclui / exclui | exclui / exclui | exclui / exclui |
   | `motivo_retratado` | exclui / exclui | exclui / exclui | exclui / exclui | exclui / exclui |
   | Vazia ou qualquer outra via | entra / entra | exclui / exclui | entra / entra | entra / entra |

   As duas vias excluídas são exatamente as declaradas pelo modelo; os resultados atuais da tabela seguem dos dois `continue`. ([models.py:2323](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2323), [tarefa_do_dia.py:211](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211), [tarefa_do_dia.py:215](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:215))

2. **Classe: horizonte temporal versus identidade retratada — corte implementado; conflito normativo da ressuscitada permanece.**

   | Estado em relação a `agora` | Resultado atual | Procedência |
   |---|---|---|
   | `motivo_retratado`; resposta anterior; única retratação futura | Entra; `aguarda_validacao` | A retratação futura libera a via; a resposta é a última decisão retida. ([seleção:212](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:212), [teste:761](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:761)) |
   | `motivo_retratado`; retratação anterior ou igual; nenhuma futura | Excluída, mesmo havendo resposta posterior | A via vence a resposta. ([models.py:2327](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2327), [seleção:211](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211)) |
   | `motivo_retratado`; nenhuma retratação | Excluída | A porta genérica aceita a via sem exigir linha correspondente; a seleção conserva a exclusão. ([porta:2736](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2736), [teste:777](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:777)) |
   | Via vazia; retratação antiga; resposta posterior, ambas até `agora` | Entra com a mesma PK; `aguarda_validacao` | É o comportamento exigido pelo teste novo. ([teste:778](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:778), [teste:783](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:783)) |
   | Há retratação futura, mas a última decisão **até agora** também é retratação | Excluída | Liberar a via não elimina a segunda guarda. ([seleção:212](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:212), [seleção:215](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:215)) |

   **O contraexemplo à regra escrita é a ressuscitada.** `remover_motivo` e `VIAS_FORA_DO_FATO` dizem expressamente que o fato que volta ganha pergunta nova. A tarefa e o teste novo aceitam reutilizar a antiga após `[retratacao, resposta]`. Paridade com o comportamento anterior não resolve essa divergência. ([models.py:1979](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1979), [models.py:2321](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2321), [teste:783](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:783))

   **Origem: conflito preexistente na ressurreição, novamente aceito pela seleção neste delta.** O universo busca carimbadas sem texto, descarimba a cópia antes do C6 e a porta descarimba o registro; a decisão antiga continua imutável. ([universo:1440](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1440), [simulação:1461](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1461), [escrita:1515](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1515), [imutabilidade:3393](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:3393))

   Há ainda uma limitação precisa: `_retratada_depois` identifica **alguma** retratação futura, sem ligar a via atual ao ato específico que a escreveu. Com retratação antiga, resposta aplicável e nova retratação futura, a pergunta entra. A garantia de correspondência entre essa via e essa retratação específica: **[nao verificado]**. ([carga:323](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:323), [seleção:212](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:212))

3. **Classe: vazamento de ato futuro e queries por N — não achei vazamento pelo `_dec` nem N+1 introduzido nesse delta.**

   | Objeto | Quem escreve | Quem lê |
   |---|---|---|
   | `_retratada_depois` | `CargaDaTarefa.__init__`: inicializa e acrescenta apenas `pergunta_id` de retratação futura. ([314](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:314), [324](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:324)) | `.tarefa` encaminha; `_tarefa_pura` consulta pertinência para liberar a via. ([380](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:380), [212](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:212)) |
   | `_dec` | A carga acrescenta objetos somente sob `d.ato_em <= agora`. ([321](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:321)) | Seleção consulta a última; `_pela_decisao` recebe a lista da pergunta. ([214](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:214), [267](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:267)) |

   Para instantes válidos e comparáveis, o corte Python conserva o significado inclusivo de `ato_em__lte=agora`, incluindo a igualdade e preservando a ordem `(ato_em, pk)` dentro de cada pergunta. `ato_em` não admite nulo no modelo e `USE_TZ=True`. Execução comparativa contra SQL: **[nao verificado]**. ([modelo:3358](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:3358), [ordenação/corte:320](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:320), [configuração:95](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/config/settings/base.py:95))

   | Caminho examinado | Conclusão |
   |---|---|
   | `absorcao_ausencia` | Lê somente a última decisão retida; o `select_related` continua presente. O **status atual** da ausência permanece um limite declarado, não um ato futuro entrando em `_dec`. ([carga:320](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:320), [leitor:145](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:145), [limite:290](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:290)) |
   | Efeitos | Carga independente de `_dec`, com `criado_em__lte=agora`; conserva o universo atual de Batidas apuráveis. ([340](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:340)) |
   | Passivo | Carga independente; correspondência exata de pergunta, ato e instante, seguida de corte `<= agora`. Uma linha futura não corresponde a um carimbo anterior. ([models.py:3412](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:3412), [carga:328](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:328)) |
   | Lavra | Carga independente, com `marcos_do_lote(ate=agora)`; `esta_stale` rejeita julgamento futuro. ([332](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:332), [veredito_lavrado.py:196](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/veredito_lavrado.py:196)) |

   Remover o filtro SQL aumenta as **linhas transportadas**, incluindo decisões futuras e suas ausências relacionadas; não acrescenta consulta por pergunta nesse caminho. O selo de constância inclui validações, mas não monta absorções; sua execução e contagem efetiva permanecem **[nao verificado]**. ([query:319](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:319), [teste:920](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:920), [comparação:937](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:937))

4. **Classes de reprovação — consolidação.**

   | Classe | Procedência e alcance |
   |---|---|
   | Segundo juiz | **Não achei introdução no delta.** A tarefa possui corte explícito e a coexistência até E3 está declarada. Isso não sana o conflito de identidade do item 2. ([CORTES.md:424](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/docs/CORTES.md:424)) |
   | Segundo escritor | **Não achei escritor persistente novo.** As coleções novas são memória; `remover_motivo` continua registrando decisão e carimbando identidade no mesmo `atomic`. ([carga:313](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:313), [models.py:1986](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:1986)) |
   | Relógio fora da borda | **Procedente, preexistente; já encaminhado como fila.** Cobertura consulta tipos em aberto, cujo conjunto vivo chega a `vigente → monotonic`, com mutação do cache. ([turnos.py:2762](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/ponto/turnos.py:2762), [ausencias.py:433](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/ponto/catalogo/ausencias.py:433), [ausencias.py:275](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/ponto/catalogo/ausencias.py:275)) |
   | Derivação paralela | **Não achei nova nos caminhos alterados:** lista de identidade do modelo, autoridade E1 do passivo, porta das Batidas e regra compartilhada de posição continuam sendo lidas. ([211](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:211), [328](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:328), [340](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:340), [55](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:55)) |
   | Campo com dois significados | **Não achei significado novo no delta.** O conjunto guarda existência de retratação futura; não guarda seu instante nem comprova qual ato escreveu a via. `manter_carimbo` já preservava instante anterior ao trocar via. ([324](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:324), [models.py:2738](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2738)) |

   **RED sem mordida: não procede para o teste novo, na comparação dos corpos executados em memória com autoridades alimentadas.** O predicado sem delta foi reconstruído do anexo; histórico Git e Django integrado: **[nao verificado]**. ([corpo atual:157](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/services/tarefa_do_dia.py:157))

   | Cenário | Sem delta | Atual / exigido pelo teste | [nao verificado]
   |---|---|---|
   | `motivo_retratado`, somente resposta | `aguarda_validacao`, mesma PK | `sem_pergunta`, sem PK. ([asserção:777](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:777)) |
   | Via vazia, retratação antiga e resposta posterior | `sem_pergunta`, sem PK | `aguarda_validacao`, mesma PK. ([asserção:783](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:783)) |

   Sem delta, o teste para na primeira asserção; o segundo cenário também discrimina quando isolado. **A mordida existe, mas a segunda expectativa certifica a paridade restaurada, permanecendo contrária à regra escrita de pergunta nova.** ([teste:777](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:777), [teste:783](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/tests/test_e2_tarefa_do_dia.py:783), [models.py:2321](/home/ronald/codex_ws/e2dc08e221c00f3c2e5d1f589bf37c1126363ddc/app/chamados/models.py:2321))

## 07/11 12:57 · sessao a97064b5e654 · - · - · origem=Ronald (tmux)

1. **RED sem mordida: não procede para o teste novo, por execução dos corpos em memória.** Extraí `_acao`, `_pela_decisao` e `_tarefa_pura` via AST, retirei imports e alimentei as autoridades; reconstruí o predicado antigo conforme o delta fornecido. Resultado: (`app/chamados/services/tarefa_do_dia.py:105`, `app/chamados/services/tarefa_do_dia.py:109`, `app/chamados/services/tarefa_do_dia.py:157`)

   | Caso | Sem delta | Atual | Asserção |
   |---|---|---|---|
   | `motivo_retratado`, apenas resposta | `p.pk, aguarda_validacao` | `None, sem_pergunta` | Atual exigido em `app/chamados/tests/test_e2_tarefa_do_dia.py:777` |
   | Via vazia, resposta → retratação → resposta | `None, sem_pergunta` | `p.pk, aguarda_validacao` | Atual exigido em `app/chamados/tests/test_e2_tarefa_do_dia.py:783` |

   **O teste completo sem delta falha já na primeira asserção; seu segundo cenário não chega a executar nessa rodada.** Isoladamente, o segundo cenário também discrimina os predicados, conforme execução em memória. Django integrado, RED/GREEN históricos e identidade Git: **[nao verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:775`, `app/chamados/tests/test_e2_tarefa_do_dia.py:777`, `app/chamados/tests/test_e2_tarefa_do_dia.py:778`)

2. **Segundo juiz: não achei introdução no delta fornecido.** O predicado atual lê a lista única `VIAS_FORA_DO_FATO`; o juiz da tarefa tem registro e corte explícitos, com coexistência dos cinco respondedores até E3 declarada. (`app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/models.py:2323`, `app/core/juizes.py:285`, `app/docs/CORTES.md:424`)

3. **Segundo escritor persistente: não achei introdução nos trechos alterados.** `_dec` e `_retratada_depois` são coleções em memória; a carga só as alimenta e encaminha. As portas persistentes continuam sendo `carimbar_identidade` e `DecisaoPergunta.registrar`; `remover_motivo` chama ambas no mesmo `atomic`. (`app/chamados/services/tarefa_do_dia.py:313`, `app/chamados/services/tarefa_do_dia.py:322`, `app/chamados/services/tarefa_do_dia.py:324`, `app/chamados/services/tarefa_do_dia.py:378`, `app/chamados/models.py:1986`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`, `app/chamados/models.py:2746`, `app/chamados/models.py:3428`)

4. **Relógio fora da borda: procedente, preexistente e já declarado como fila pelo usuário.** Caminho efetivamente lido: tarefa → `cobertura` → `_q_cobertura` → `Ausencia.TIPOS_EM_ABERTO` → conjunto vivo → `em_aberto_vigentes` → `vigente` → `monotonic`. Também há mutação do cache de processo. (`app/chamados/services/tarefa_do_dia.py:378`, `app/ponto/turnos.py:3072`, `app/ponto/turnos.py:2762`, `app/ponto/models.py:689`, `app/ponto/catalogo/ausencias.py:464`, `app/ponto/catalogo/ausencias.py:433`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/catalogo/ausencias.py:279`)

   O veneno dinâmico intercepta apenas `timezone.now`; o selo estático examina apenas tarefa e lavra, deixando catálogo fora. (`app/chamados/tests/test_e2_tarefa_do_dia.py:680`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1010`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1011`)

5. **Derivação paralela: não achei nova nos caminhos alterados.** A seleção lê a lista de identidade do modelo; passivo continua usando a autoridade E1; efeito usa `batidas_apuraveis`; posição chama a regra compartilhada; cobertura usa filtro e corpo compartilhados. (`app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:328`, `app/chamados/services/tarefa_do_dia.py:340`, `app/chamados/services/tarefa_do_dia.py:55`, `app/ponto/turnos.py:3072`, `app/ponto/turnos.py:3076`)

6. **Campo com dois significados: não achei significado novo no delta.** `_retratada_depois` significa existência de **alguma** retratação com `ato_em > agora`, independentemente do último ato futuro; não carrega a própria decisão. `ato_em` mantém seu papel temporal. (`app/chamados/services/tarefa_do_dia.py:321`, `app/chamados/services/tarefa_do_dia.py:323`, `app/chamados/services/tarefa_do_dia.py:324`)

   Permanecem duas ressalvas anteriores: `materializada_em` pode conservar instante anterior ao trocar a via com `manter_carimbo`; resposta/validação projetadas podem receber instante do fechamento por `carimbar`. Não são introduções desta volta. (`app/chamados/models.py:2738`, `app/chamados/models.py:2739`, `app/chamados/models.py:2667`)

7. **Docstring: correções solicitadas presentes.** A abertura agora limita o corte aos fatos datados da pergunta; enumeração inclui cadastro e catálogo condicionado ao TTL. (`app/chamados/tests/test_e2_tarefa_do_dia.py:702`, `app/chamados/services/tarefa_do_dia.py:282`, `app/chamados/services/tarefa_do_dia.py:283`)

Não existe repositório Git utilizável no diretório consultado; comparação histórica direta `53db08a0..e2dc08e2`: **[nao verificado]**. A comparação acima usa o delta fornecido e os arquivos da cópia; nenhuma suíte Django foi executada: **[nao verificado]**.

## 07/11 12:56 · sessao e4e5706c2a05 · - · - · origem=Ronald (tmux)

- **Paridade no presente: procede para o predicado informado de 57acffd2.** Quando `agora >= todos os atos`, `_retratada_depois` fica vazio; a seleção atual se reduz a “via fora do fato exclui; última decisão retratação exclui”. (`app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:215`, `app/chamados/services/tarefa_do_dia.py:321`)

- **Regra escrita versus ressuscitada: permanece conflito, apesar da paridade.** A lei na cópia diz explicitamente que a retratada morre por identidade e, se o fato voltar, nasce pergunta nova. A tarefa admite a mesma pergunta com via vazia e sequência `[retratacao, resposta]`, atribuindo `aguarda_validacao`. O teste novo exige justamente esse comportamento. (`app/chamados/models.py:1979`, `app/chamados/models.py:2320`, `app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:215`, `app/chamados/services/tarefa_do_dia.py:129`, `app/chamados/tests/test_e2_tarefa_do_dia.py:766`)

- **Não é só construção impossível pela API:** a ressurreição busca carimbadas sem texto, limpa via na cópia antes de consultar C6 e depois limpa via real; não remove a retratação imutável. C6 aceita célula acusando depois desse descarimbo. Esse caminho não exclui `motivo_retratado`; o cartório exclui apenas o caso em que todas são duplicatas. Uma resposta posterior pode entrar na sequência pela porta normal. Ocorrência produtiva **[nao verificado]**. (`app/chamados/models.py:1440`, `app/chamados/models.py:1461`, `app/chamados/models.py:1515`, `app/chamados/models.py:3393`, `app/chamados/juizes.py:1183`, `app/ponto/services/cartorio.py:1610`, `app/ponto/services/cartorio.py:1633`, `app/chamados/services/disputa_emissao.py:1369`)

- **Há testemunha de regra oposta, já existente:** `test_RED_motivo_retratado_que_volta_nasce_pergunta_nova` exige PK nova e preservação da via retratada na antiga. Embora esse caso seja motivo sem data e não o marco da tarefa, confirma a intenção documental sobre identidade; não prova que a ressurreição de marco obedeça à regra. (`app/chamados/tests/test_e1_volta2.py:244`, `app/chamados/tests/test_e1_volta2.py:260`, `app/chamados/tests/test_e1_volta2.py:261`)

| Via/sequência no horizonte | Seleção atual | Procedência |
|---|---|---|
| `motivo_retratado`, única retratação futura | Via cede; vive se última decisão até `agora` não for retratação | Corte temporal implementado. (`app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:323`) |
| `motivo_retratado`, retratação anterior, sem retratação futura | Excluída pela via, mesmo com resposta posterior | Compatível com via que vence resposta. (`app/chamados/models.py:2327`, `app/chamados/services/tarefa_do_dia.py:211`) |
| `motivo_retratado`, nenhuma retratação | Excluída pela via | Porta genérica aceita essa combinação, sem exigir decisão. (`app/chamados/models.py:2736`, `app/chamados/models.py:2746`, `app/chamados/services/tarefa_do_dia.py:211`) |
| Via vazia, retratação antiga + resposta anterior a `agora` | Viva, mesma PK | Paridade restaurada; conflito com “nasce pergunta nova” permanece. (`app/chamados/services/tarefa_do_dia.py:215`, `app/chamados/models.py:2321`) |
| `motivo_retratado`, retratação antiga + resposta até `agora` + nova retratação futura | Vive: qualquer retratação futura libera a via, e a última decisão aplicável é resposta | O conjunto não identifica qual retratação originou a via atual; resta a mesma questão de reutilização de identidade. (`app/chamados/services/tarefa_do_dia.py:323`, `app/chamados/services/tarefa_do_dia.py:211`, `app/chamados/services/tarefa_do_dia.py:215`) |

**Classificação sugerida:** conflito preexistente entre porta de ressurreição e regra de identidade, que o delta volta a aceitar na seleção; não classificar a equivalência com 57 como prova de conformidade à lei. (`app/chamados/models.py:1515`, `app/chamados/models.py:2321`, `app/chamados/services/tarefa_do_dia.py:215`)

## 07/11 12:56 · sessao f420c0cfdb6b · - · - · origem=Ronald (tmux)

**Classe: vazamento de ato futuro — não achei no `_dec` nem nos seus leitores examinados.** A retirada do filtro SQL amplia o que a query transporta, mas o append continua sob `d.ato_em <= agora`; o ramo futuro só guarda o ID quando o ato é `RETRATACAO`. A ordem continua `(pergunta_id, ato_em, pk)`. (`app/chamados/services/tarefa_do_dia.py:319`, `app/chamados/services/tarefa_do_dia.py:321`, `app/chamados/services/tarefa_do_dia.py:323`)

| Objeto | Escritor | Leitor |
|---|---|---|
| `self._retratada_depois` | Inicialização vazia e `add(pergunta_id)` exclusivamente para retratação futura. (`app/chamados/services/tarefa_do_dia.py:314`, `app/chamados/services/tarefa_do_dia.py:323`) | `.tarefa` passa para `_tarefa_pura`; esta usa apenas pertinência do ID para a exceção da via `motivo_retratado`. (`app/chamados/services/tarefa_do_dia.py:380`, `app/chamados/services/tarefa_do_dia.py:211`) |
| `self._dec` | Inicialização vazia e append exclusivamente no ramo `<= agora`. (`app/chamados/services/tarefa_do_dia.py:313`, `app/chamados/services/tarefa_do_dia.py:321`) | `.tarefa` passa o dict; seleção lê a última decisão e `_pela_decisao` recebe a lista dessa pergunta. (`app/chamados/services/tarefa_do_dia.py:379`, `app/chamados/services/tarefa_do_dia.py:214`, `app/chamados/services/tarefa_do_dia.py:267`) |

**Classe: equivalência do corte temporal — sustentada pelo código para instantes comparáveis válidos; execução SQL/Django [nao verificado].** `ato_em` é `DateTimeField` sem `null=True`; tanto `ato_em__lte=agora` quanto `d.ato_em <= agora` incluem igualdade, e filtrar a sequência já ordenada conserva a ordem relativa retida. (`app/chamados/models.py:3358`, `app/chamados/services/tarefa_do_dia.py:320`, `app/chamados/services/tarefa_do_dia.py:321`)

| Leitor | Conclusão examinada |
|---|---|
| `absorcao_ausencia` | O `select_related` permanece; `_pela_decisao` lê o status somente da última decisão retida. O status é **atual**, conforme limite declarado, e não fotografia histórica da ausência. Nenhum ato futuro passa por esse ramo via `_dec`. (`app/chamados/services/tarefa_do_dia.py:320`, `app/chamados/services/tarefa_do_dia.py:125`, `app/chamados/services/tarefa_do_dia.py:145`, `app/chamados/services/tarefa_do_dia.py:290`) |
| Efeitos | Não deriva de `_dec`: vem de query própria de Batidas apuráveis com `criado_em__lte=agora`. A retratação atual da Batida continua excluindo-a, inclusive quando posterior a `agora`, limite já declarado. (`app/chamados/services/tarefa_do_dia.py:340`, `app/ponto/turnos.py:2876`, `app/chamados/services/tarefa_do_dia.py:338`) |
| Passivo | Não deriva de `_dec`: a autoridade consulta correspondência exata `(pergunta_id, ato, ato_em)` e a carga corta as linhas devolvidas por `<= agora`. Uma decisão futura não pode satisfazer correspondência com um carimbo anterior porque o instante deve ser idêntico. (`app/chamados/models.py:3412`, `app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:328`) |
| Lavra | Não deriva de `_dec`: usa `marcos_do_lote(ate=agora)` e `esta_stale(..., agora)`. `julgado_em > agora` é recusado antes dos demais motivos. (`app/chamados/services/tarefa_do_dia.py:332`, `app/chamados/services/tarefa_do_dia.py:333`, `app/chamados/veredito_lavrado.py:196`) |

**Classe: queries por N — não achei introdução de N+1 nesse delta; volume de linhas transportadas aumenta.** A decisão segue em uma query de lote com ausência relacionada; o passivo faz outra query de lote; o acesso à âncora usa IDs e a disputa já relacionada. Remover `ato_em__lte` carrega também todas as decisões futuras das perguntas selecionadas, sem acrescentar uma query por pergunta. (`app/chamados/services/tarefa_do_dia.py:309`, `app/chamados/services/tarefa_do_dia.py:319`, `app/chamados/models.py:3412`, `app/chamados/services/fio.py:116`)

O teste de constância compara uma carga com dois colaboradores/três dias contra um colaborador/um dia, incluindo decisões até validação; não inclui absorções nessa montagem. Seu resultado executado e contagem efetiva: **[nao verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:912`, `app/chamados/tests/test_e2_tarefa_do_dia.py:920`, `app/chamados/tests/test_e2_tarefa_do_dia.py:937`)

## 07/11 12:41 · sessao 216139486a7d · FIO-E0-V4-E2 · verificador · origem=Code

**O delta corrige os dois contraexemplos anteriores, mas não é incondicionalmente neutro no presente.** A correspondência entre via e retratação não é imposta pela porta genérica; também achei um caminho de ressurreição que limpa a via e conserva a retratação antiga. (`app/chamados/models.py:2727`, `app/chamados/models.py:2746`, `app/chamados/models.py:1515`, `app/chamados/services/tarefa_do_dia.py:210`)

Ocorrência desses estados em produção, execução integrada Django, RED/GREEN históricos e identidade Git da cópia: **[nao verificado]**.

1. **Classe: correspondência entre via e sequência — garantia estrutural incompleta; procedente na API, ocorrência produtiva [nao verificado].**

   | Objeto | Escritores encontrados |
   |---|---|
   | Via `motivo_retratado` | `remover_motivo` registra `RETRATACAO` e chama `carimbar_identidade` no mesmo `atomic`. Foi o único chamador produtivo explícito dessa via que achei. (`app/chamados/models.py:1986`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`) |
   | Via `duplicata` | `retratar_duplicata → carimbar_identidade`; chamadores encontrados: deduplicação e migração de resolvedoras vivas. Não registra decisão `retratacao`. (`app/chamados/services/fio.py:939`, `app/chamados/services/fio.py:952`, `app/chamados/services/fio.py:1004`, `app/chamados/management/commands/dedup_perguntas_fato.py:88`) |
   | Atribuição física das vias | `_escrever_carimbo`; a porta de identidade salva os três campos. `manter_carimbo=True` pode trocar a via preservando instante e autor anteriores. (`app/chamados/models.py:2658`, `app/chamados/models.py:2738`, `app/chamados/models.py:2746`) |
   | Decisão `retratacao` | Chamador produtivo encontrado: `remover_motivo`, pela porta `DecisaoPergunta.registrar`, cujo corpo faz o `bulk_create`. (`app/chamados/models.py:1989`, `app/chamados/models.py:3428`) |

   **Existe testemunha na cópia de via sem decisão:** `test_RED_retratada_nao_esta_no_dominio_do_lote` chama somente `carimbar_identidade(VIA_MOTIVO_RETRATADO, ...)`. A porta aceita a via e não registra retratação. Isso demonstra possibilidade pela API e uso em fixture, sem demonstrar dado produtivo. (`app/chamados/tests/test_e1_volta2.py:128`, `app/chamados/tests/test_e1_volta2.py:131`, `app/chamados/models.py:2736`, `app/chamados/models.py:2746`)

   Não achei migração/backfill que produza essa combinação: a 0051 acrescenta a escolha, converte decisões antigas em `reabertura` e preenche a chave; o backfill de decisões é DRY e recupera resposta/validação. (`app/chamados/migrations/0051_decisao_pergunta.py:22`, `app/chamados/migrations/0051_decisao_pergunta.py:49`, `app/chamados/migrations/0051_decisao_pergunta.py:111`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:7`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:29`)

   Os leitores do domínio do fato e da emissão excluem ambas as vias; `q_vivas` e C6 leem via/carimbo atuais; o juiz do lastro lê o conjunto de identidade. A tarefa passa a excluir duplicata pela via e retratada pela sequência datada. (`app/chamados/models.py:2450`, `app/chamados/services/disputa_emissao.py:323`, `app/chamados/models.py:2888`, `app/chamados/juizes.py:1147`, `app/chamados/juizes.py:658`, `app/chamados/services/tarefa_do_dia.py:208`)

   **Sem nenhuma linha `retratacao`, a via `motivo_retratado` deixa de excluí-la em qualquer `agora`.** Se a pergunta for escolhida para um marco vencido/apagado e superar as guardas anteriores, seguirá `_pela_decisao`: sem decisões, `aguarda_resposta/responder`; com resposta, `aguarda_validacao/validar`; com passivo aplicável, `passivo_sem_linha/nao_sei`. Não existe tratamento específico de inconsistência via/ato. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:121`, `app/chamados/services/tarefa_do_dia.py:129`)

2. **Classe: mudança de comportamento no presente — procedente; não significa que o comportamento anterior fosse correto.**

   Reconstituí em memória o ramo anterior fornecido e comparei os corpos, com `agora` posterior aos atos e marco exigível: (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:262`)

   | Via atual / sequência | Antes do delta | Atual |
   |---|---|---|
   | `duplicata`, qualquer sequência | Excluída | Excluída |
   | `motivo_retratado`, com retratação aplicável | Excluída pela via | Excluída pela decisão |
   | `motivo_retratado`, somente resposta | `sem_pergunta/perguntar` | `aguarda_validacao/validar` |
   | Via vazia, `[retratacao, resposta]` | `aguarda_validacao/validar` | `sem_pergunta/perguntar` |

   Os resultados decorrem da seleção e dos ramos preservados de resposta/ausência de pergunta. (`app/chamados/services/tarefa_do_dia.py:129`, `app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:256`)

   **O último estado tem caminho no código de produção:**

   - O cartório considera todas as perguntas do destino; sem fio vivo nem texto, e não sendo todas duplicatas, chama `ressuscitar_perguntas`. Não exclui motivo retratado nesse bloco. (`app/ponto/services/cartorio.py:1605`, `app/ponto/services/cartorio.py:1608`, `app/ponto/services/cartorio.py:1610`, `app/ponto/services/cartorio.py:1633`)
   - O universo da ressurreição aceita carimbadas sem texto e **limpa a via na cópia antes de consultar C6**. Com célula acusando e marco sem fato, essa cópia pode ser julgada viva. Confirmei essa seleção executando os corpos em memória com dependências alimentadas. (`app/chamados/models.py:1440`, `app/chamados/models.py:1461`, `app/chamados/models.py:1464`, `app/chamados/juizes.py:1183`)
   - O lote limpa a via real e reabre a disputa; não apaga a decisão imutável antiga. A API pode então aceitar nova resposta pela vitalidade atual e registrar `resposta` posterior. (`app/chamados/models.py:1515`, `app/chamados/models.py:2801`, `app/chamados/models.py:1542`, `app/api/views.py:2647`, `app/api/views.py:2659`, `app/chamados/services/disputa_emissao.py:1369`)

   Portanto, não confirmei equivalência no presente pelos dois caminhos. Entretanto, a regra documentada determina que a retratada saia da identidade e que o fato que volte ganhe pergunta nova; o `any` é compatível com essa intenção. O caminho de ressurreição é que não preserva essa separação. (`app/chamados/models.py:2320`, `app/chamados/models.py:2323`, `app/chamados/models.py:1461`, `app/chamados/services/tarefa_do_dia.py:210`)

3. **Classe: corte temporal do passivo — não achei defeito novo; equivalência de instantes sustentada pelo código.**

   O dict devolve `ato_em=respondida_em` para resposta e `ato_em=validada_em` para validação. A correspondência exige exatamente `(pergunta_id, ato, instante)`. Assim, `<= agora` tem a mesma inclusão temporal de `ato_em__lte=agora`; são campos `DateTimeField`, com configuração timezone-aware. Consistência de cada registro produtivo: **[nao verificado]**. (`app/chamados/models.py:3412`, `app/chamados/models.py:3418`, `app/chamados/models.py:3422`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:316`, `app/config/settings/base.py:93`)

   O censo de escritores dos carimbos encontrou: (`app/chamados/models.py:2257`, `app/chamados/models.py:2258`)

   | Operação | Escrita |
   |---|---|
   | Resposta normal | Projeção e decisão recebem o mesmo `_agora`. (`app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/disputa_emissao.py:1346`, `app/chamados/services/disputa_emissao.py:1369`) |
   | Resposta automática / recuperação pela trilha | Recebem instante externo, também usado na decisão. (`app/chamados/services/disputa_emissao.py:1516`, `app/chamados/services/disputa_emissao.py:1519`, `app/chamados/services/disputa_emissao.py:1537`, `app/chamados/services/disputa_emissao.py:1540`) |
   | Validação | Projeção recebe `timezone.now()`; decisão recebe exatamente esse valor. (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:139`) |
   | `carimbar` | Preenche campos vazios com `quando`, havendo texto. (`app/chamados/models.py:2683`, `app/chamados/models.py:2691`, `app/chamados/models.py:2694`) |
   | Nova resposta / devolução | Zeram validação anterior. (`app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/validacao.py:199`) |
   | Reabertura / ressurreição | Zeram os carimbos pelas portas existentes. (`app/chamados/services/disputa_emissao.py:1462`, `app/chamados/services/disputa_emissao.py:1493`, `app/chamados/models.py:1515`) |
   | Restauração / simulação | Restauram fotografia em memória ou alteram somente a cópia. (`app/chamados/models.py:2722`, `app/chamados/models.py:1462`) |

   Os leitores relevantes incluem estado/vitalidade, seleção para materialização, correspondência do passivo e invalidação da lavra. O novo `_pela_decisao` consulta o conjunto de passivo, sem julgar diretamente esses carimbos. (`app/chamados/models.py:2601`, `app/chamados/juizes.py:1137`, `app/chamados/services/materializacao.py:1361`, `app/chamados/models.py:3416`, `app/chamados/veredito_lavrado.py:180`, `app/chamados/services/tarefa_do_dia.py:121`)

   Os consumidores do dict são resposta, resposta automática, validação, devolução, ressurreição, DRY do backfill e carga da tarefa. Os escritores encaminham as linhas à sequência; a tarefa usa `pergunta_id/ato_em`. (`app/chamados/services/disputa_emissao.py:1344`, `app/chamados/services/disputa_emissao.py:1514`, `app/chamados/services/validacao.py:133`, `app/chamados/services/validacao.py:198`, `app/chamados/models.py:1513`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`, `app/chamados/services/tarefa_do_dia.py:316`)

   **Resposta às 19h, validação às 22h, `agora=21h`:**

   - Com linha correspondente de resposta: validação futura não entra no passivo nem nas decisões; resultado `aguarda_validacao/validar`. (`app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:317`, `app/chamados/services/tarefa_do_dia.py:129`)
   - Sem linha da resposta: o passivo das 19h permanece; resultado `passivo_sem_linha/nao_sei`. (`app/chamados/models.py:3416`, `app/chamados/services/tarefa_do_dia.py:121`)
   - Consultar a sequência inteira para correspondência não introduz ato futuro nesse caso: uma linha correspondente ao carimbo anterior tem necessariamente o mesmo instante anterior. (`app/chamados/models.py:3412`, `app/chamados/models.py:3416`)

   **Ressalva semântica preexistente:** `carimbar` pode usar o instante de fechamento para preencher resposta/validação. Portanto, esses campos não garantem recuperar o instante original da fala ou validação humana. Isso não foi introduzido pelo novo `<=`. (`app/chamados/models.py:2667`, `app/chamados/models.py:2691`, `app/chamados/models.py:2694`)

4. **Classe: documentação do horizonte — melhorou, mas não está inteiramente exata.**

   A carga agora declara corretamente os estados atuais anteriormente omitidos: identidade/âncora, ausência e absorção, fila, cadastro, vínculo e catálogo. Os cortes descritos de decisão, passivo, lavra e criação da Batida correspondem aos filtros. (`app/chamados/services/tarefa_do_dia.py:278`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:328`)

   Ainda sobra a abertura do selo, **“corta TODO fato que a tarefa lê”**, contrariada pelos próprios limites seguintes. A enumeração de queries também omite a consulta de cadastro e a consulta condicional do catálogo. (`app/chamados/tests/test_e2_tarefa_do_dia.py:702`, `app/chamados/tests/test_e2_tarefa_do_dia.py:709`, `app/chamados/services/tarefa_do_dia.py:276`, `app/chamados/services/tarefa_do_dia.py:295`, `app/ponto/catalogo/ausencias.py:278`)

   Declarar catálogo atual não resolve a promessa de relógio somente pela borda: o caminho transitivo continua lendo `monotonic`. (`app/chamados/services/tarefa_do_dia.py:284`, `app/ponto/catalogo/ausencias.py:275`)

5. **Classes de reprovação — consolidação do delta.**

   | Classe | Procedência |
   |---|---|
   | **Segundo juiz** | **Não achei novo no delta.** Há corte/registro para a tarefa e coexistência declarada até E3. (`app/core/juizes.py:150`, `app/docs/CORTES.md:424`) |
   | **Segundo escritor fora da porta** | **Não achei introdução no delta.** A lacuna via/ato está na API existente; ressurreição também usa a porta existente de descarimbo. (`app/chamados/models.py:2727`, `app/chamados/models.py:1515`) |
   | **Relógio fora da borda** | **Procedente, permanece:** tarefa → cobertura → conjunto vivo → catálogo → `monotonic`, com atualização de cache. O AST cobre dois módulos e o veneno intercepta `timezone.now`, deixando esse caminho fora. (`app/chamados/services/tarefa_do_dia.py:366`, `app/ponto/turnos.py:2762`, `app/ponto/catalogo/ausencias.py:433`, `app/ponto/catalogo/ausencias.py:275`, `app/chamados/tests/test_e2_tarefa_do_dia.py:680`, `app/chamados/tests/test_e2_tarefa_do_dia.py:991`) |
   | **Derivação paralela de fonte única** | **Não achei nos caminhos alterados.** Passivo usa a autoridade E1; efeito mantém `batidas_apuraveis`; teto e posição mantêm os corpos compartilhados. (`app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:328`, `app/ponto/turnos.py:3059`, `app/chamados/services/tarefa_do_dia.py:55`) |
   | **RED sem mordida** | **Não procede para os dois testes novos, em memória.** Sem o delta, às 21h o primeiro recebe `sem_pergunta` e o segundo `passivo_sem_linha`, contrariando suas asserções; o atual recebe os resultados exigidos. Django integrado: **[nao verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:761`, `app/chamados/tests/test_e2_tarefa_do_dia.py:774`) |
   | **Campo com dois significados** | **Não achei significado novo introduzido pelo corte.** Permanecem as ressalvas de carimbo de fechamento e `materializada_em` preservado ao trocar via; `ato_em` representa o ato, não o nascimento físico da linha. (`app/chamados/models.py:2691`, `app/chamados/models.py:2738`, `app/chamados/services/disputa_emissao.py:1540`) |
   | **Queries crescendo com N** | **Não achei SELECT por pergunta nos ramos novos.** Perguntas/decisões vêm relacionadas e a correspondência usa consulta de lote. O teste inclui validações, mas não ausências; catálogo depende de TTL. Constância absoluta e SQL medido: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:302`, `app/chamados/services/tarefa_do_dia.py:311`, `app/chamados/models.py:3412`, `app/chamados/tests/test_e2_tarefa_do_dia.py:893`, `app/ponto/catalogo/ausencias.py:276`) |

## 07/11 12:38 · sessao 67c07a42d4d7 · - · - · origem=Ronald (tmux)

**Há caminho produtivo que permite diferença no presente pelo `any`, sem depender apenas da porta genérica. Ocorrência em produção [nao verificado].** (`app/ponto/services/cartorio.py:1633`, `app/chamados/services/tarefa_do_dia.py:210`)

A sequência alcançável por leitura é:

1. `remover_motivo` grava retratação e via `motivo_retratado`; a pergunta sem resposta fica conservada e carimbada. (`app/chamados/models.py:1987`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`)
2. O cartório carrega **todas** as perguntas que resolvem o chamado. Se não há fio vivo nem resposta com texto e o conjunto não é inteiramente duplicata, chama `ressuscitar_perguntas`; não há guarda de motivo retratado nesse bloco. (`app/ponto/services/cartorio.py:1605`, `app/ponto/services/cartorio.py:1606`, `app/ponto/services/cartorio.py:1608`, `app/ponto/services/cartorio.py:1610`, `app/ponto/services/cartorio.py:1633`)
3. O universo da ressurreição filtra carimbo presente e resposta vazia, sem excluir essa via. Na cópia usada para consultar C6, **descarimba antes de julgar**, removendo o insumo que faria a via matar a pergunta. Com célula acusando e marco sem fato, C6 pode devolvê-la viva. (`app/chamados/models.py:1440`, `app/chamados/models.py:1443`, `app/chamados/models.py:1461`, `app/chamados/models.py:1464`, `app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1186`)
4. O lote limpa a via real; a decisão imutável de retratação permanece. A mesma porta reabre a disputa fechada. (`app/chamados/models.py:1515`, `app/chamados/models.py:2801`, `app/chamados/models.py:1542`, `app/chamados/models.py:1543`)
5. A API aceita a pergunta pela vitalidade atual e chama o escritor da resposta; ele grava uma decisão `resposta` posterior para a mesma pergunta. (`app/api/views.py:2644`, `app/api/views.py:2647`, `app/api/views.py:2659`, `app/chamados/services/disputa_emissao.py:1369`)

Assim, para um estado com `via=''`, sequência `[retratacao, resposta]`, `agora` posterior aos dois atos e marco exigível sem outra pergunta escolhida:

- O código anterior do delta não a excluía pela via nem pela última decisão; chegaria a `aguarda_validacao/validar`. (**Anexo `e2_b3v3_delta.diff`**, contexto da alteração em `_tarefa_pura`; ramo preservado em `app/chamados/services/tarefa_do_dia.py:129`.)
- O código atual a exclui por existir **qualquer** retratação aplicável, chegando a `sem_pergunta/perguntar`. (`app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:256`, `app/chamados/services/tarefa_do_dia.py:258`)

**Classe: mudança de comportamento no presente; procedente por caminho de código.** Isso não estabelece que o comportamento anterior era correto: a regra documentada diz que a retratada saiu da identidade e o fato que volta ganha pergunta nova. A ressurreição acima pode estar apagando indevidamente essa identidade antes de chamar C6. (`app/chamados/models.py:2320`, `app/chamados/models.py:2323`, `app/chamados/models.py:1461`)

As portas específicas `reabrir_fantasma`, `reabrir_por_competencia` e `reabrir_por_resposta` têm guardas que não contemplam motivo retratado; `desfazer_carimbo_sem_lastro` exclui todas as vias de identidade. O caminho encontrado é a ressurreição por chamado/cartório. (`app/chamados/models.py:2832`, `app/chamados/models.py:2849`, `app/chamados/models.py:2856`, `app/chamados/services/relavra_via.py:179`)

Na busca adicional em Python **fora de `app`**, excluídos testes e logs, não achei escritor das duas vias nem `RETRATACAO`; essa conclusão limita-se ao texto pesquisado. **[nao verificado]** [nao verificado]

## 07/11 12:38 · sessao 3c3407e2311a · - · - · origem=Ronald (tmux)

**Corte do passivo: não achei defeito novo no caso resposta antes/validação depois.** Ele compara inclusivamente o instante que a autoridade devolve; a sequência também usa comparação inclusiva sobre `ato_em`. (`app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/models.py:3418`, `app/chamados/models.py:3422`)

**Escritores encontrados em código de produção**, por `rg` e AST em `app/**/*.py`, excluídos testes e migrações: [nao verificado]

| Campo | Escritor |
|---|---|
| `respondida_em` | Resposta normal: `_agora = _tz.now()` e atribuição; decisão recebe exatamente `_agora`. (`app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/disputa_emissao.py:1346`, `app/chamados/services/disputa_emissao.py:1369`) |
| `respondida_em` | Resposta automática: recebe `instante`; decisão recebe o mesmo valor. (`app/chamados/services/disputa_emissao.py:1516`, `app/chamados/services/disputa_emissao.py:1519`) |
| `respondida_em` | Recuperação pela trilha: recebe o instante externo da nota; decisão recebe o mesmo valor. (`app/chamados/services/disputa_emissao.py:1528`, `app/chamados/services/disputa_emissao.py:1537`, `app/chamados/services/disputa_emissao.py:1540`) |
| `validada_em` | Validação: `timezone.now()` e decisão com exatamente `perg.validada_em`. (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:139`) |
| Ambos | `carimbar`: preenche os campos vazios, havendo texto, com `quando`; `quando` ausente vira `_tz.now()`. (`app/chamados/models.py:2683`, `app/chamados/models.py:2689`, `app/chamados/models.py:2691`, `app/chamados/models.py:2694`) |
| Ambos | Reabertura zera os campos por `campos`, persistidos por `descarimbar_em_lote` ou `qs.update`. (`app/chamados/services/disputa_emissao.py:1462`, `app/chamados/services/disputa_emissao.py:1493`, `app/chamados/services/disputa_emissao.py:1495`) |
| Ambos | Ressuscitação zera pela porta de lote. (`app/chamados/models.py:1515`, `app/chamados/models.py:2801`) |
| `validada_em` | Nova resposta zera a validação anterior; devolução também zera e registra ato. (`app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1366`, `app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:202`, `app/chamados/services/validacao.py:205`) |
| Ambos | Desfazer carimbo em memória restaura a fotografia; a simulação de ressuscitação zera somente uma cópia. (`app/chamados/models.py:2722`, `app/chamados/models.py:2723`, `app/chamados/models.py:1449`, `app/chamados/models.py:1462`, `app/chamados/models.py:1463`) |

A migração encontrada apenas declara os dois `DateTimeField`, sem preenchê-los. (`app/chamados/migrations/0014_perguntadisputa.py:25`, `app/chamados/migrations/0014_perguntadisputa.py:26`)

**Há uma ressalva de significado preexistente:** `carimbar` pode preencher `respondida_em/validada_em` com o instante do fechamento, e não recuperar o instante original de fala/validação humana. Seus chamadores incluem absorção, materialização, encerramento por fato e encerramento sem credor. Isso não é introduzido pelo delta do passivo. (`app/chamados/models.py:2667`, `app/chamados/models.py:2691`, `app/chamados/models.py:2694`, `app/chamados/services/materializacao.py:558`, `app/chamados/services/materializacao.py:1268`, `app/chamados/services/disputa_emissao.py:1707`, `app/chamados/services/disputa_emissao.py:1781`)

Na absorção, especificamente, o chamador registra `ABSORCAO`, enquanto `carimbar(resposta='validar')` pode preencher os dois carimbos. A autoridade de passivo procura correspondência por `RESPOSTA/VALIDACAO`, portanto esses carimbos podem aparecer como passivo; esse comportamento já decorre do código anterior ao delta. (`app/chamados/services/materializacao.py:558`, `app/chamados/services/materializacao.py:560`, `app/chamados/models.py:3412`, `app/chamados/models.py:3416`, `app/chamados/models.py:3420`)

**Quem escreve/lê o dict:** `linhas_da_projecao_sem_linha` lê a projeção em memória e uma query da sequência, exigindo correspondência exata `(pergunta_id, ato, ato_em)`. Devolve resposta com `ato_em=respondida_em` e validação com `ato_em=validada_em`. (`app/chamados/models.py:3407`, `app/chamados/models.py:3412`, `app/chamados/models.py:3418`, `app/chamados/models.py:3422`)

Os consumidores de produção encontrados são: resposta normal, resposta automática, validação, devolução, ressuscitação, o DRY do backfill e a carga da tarefa. Os cinco primeiros encaminham linhas a `registrar`; o backfill classifica por pergunta/ato; a tarefa usa `pergunta_id` e `ato_em`. (`app/chamados/services/disputa_emissao.py:1344`, `app/chamados/services/disputa_emissao.py:1369`, `app/chamados/services/disputa_emissao.py:1514`, `app/chamados/services/disputa_emissao.py:1519`, `app/chamados/services/validacao.py:133`, `app/chamados/services/validacao.py:139`, `app/chamados/services/validacao.py:198`, `app/chamados/services/validacao.py:205`, `app/chamados/models.py:1513`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:58`, `app/chamados/services/tarefa_do_dia.py:316`)

**Caso resposta19, validação22, agora21**, executado em memória com o corpo AST exato da autoridade e ORM falso, sem banco:

- Sem nenhuma linha: a autoridade devolve resposta19 e validação22; o corte mantém o passivo da resposta19. A tarefa responde `passivo_sem_linha`, não inventa validação. (`app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:121`)
- Com linha correspondente de resposta19: só validação22 é devolvida e descartada pelo corte; a sequência cortada contém resposta19, logo `aguarda_validacao/validar`. (`app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:126`, `app/chamados/services/tarefa_do_dia.py:129`)
- Com ambas as linhas: a autoridade devolve vazio; a validação22 sai pelo filtro SQL e novamente vale resposta19. (`app/chamados/models.py:3412`, `app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:310`)

Os três campos são instantes (`DateTimeField`), os escritores normais compartilham o valor entre projeção e sequência e a configuração usa timezone-aware com São Paulo. Isso sustenta a equivalência temporal do `<=` Python e do `__lte`; integridade/fuso de cada registro produtivo e execução SQL/Django permanecem **[nao verificado]**. (`app/chamados/models.py:2257`, `app/chamados/models.py:2258`, `app/chamados/models.py:3358`, `app/chamados/services/disputa_emissao.py:1369`, `app/chamados/services/validacao.py:139`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

## 07/11 12:37 · sessao cef5096f5ef1 · - · - · origem=Ronald (tmux)

1. **RED sem mordida — não procede para os dois testes novos, por execução dos corpos em memória.** Reconstituí o ramo anterior a partir do delta fornecido, sem Django ou banco. Antes do delta, o teste de retratação recebe `sem_pergunta/perguntar` às 21h; o atual recebe `aguarda_validacao/validar`. Antes do delta, o teste do passivo recebe `passivo_sem_linha/nao_sei` às 21h; o atual recebe `aguarda_resposta/responder`. Os resultados anteriores contradizem as primeiras asserções dos respectivos testes. Ambos preservam os resultados posteriores às 23h. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:121`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/tests/test_e2_tarefa_do_dia.py:761`, `app/chamados/tests/test_e2_tarefa_do_dia.py:774`) Execução integrada Django e RED/GREEN históricos: **[nao verificado]**.

2. **Docstrings — limites principais agora declarados; ainda sobra uma formulação absoluta.** A carga distingue fatos cortados de estado atual, incluindo ausência/absorção, fila, cadastro, vínculo, âncora, identidade e catálogo. Isso corresponde aos caminhos lidos. (`app/chamados/services/tarefa_do_dia.py:278`, `app/chamados/services/tarefa_do_dia.py:283`, `app/chamados/services/tarefa_do_dia.py:295`, `app/chamados/services/tarefa_do_dia.py:310`, `app/chamados/services/tarefa_do_dia.py:328`, `app/chamados/services/tarefa_do_dia.py:343`, `app/chamados/services/tarefa_do_dia.py:366`) O selo ainda abre dizendo “corta TODO fato”, embora depois explicite os limites atuais; a frase inicial, isoladamente, excede o código. (`app/chamados/tests/test_e2_tarefa_do_dia.py:702`, `app/chamados/tests/test_e2_tarefa_do_dia.py:709`)

   A enumeração de queries da carga também não é completa: falta mencionar a consulta de cadastro e a consulta condicional do catálogo. (`app/chamados/services/tarefa_do_dia.py:276`, `app/chamados/services/tarefa_do_dia.py:295`, `app/ponto/catalogo/ausencias.py:278`)

3. **Relógio transitivo fora da borda — procedente, permanece.** `tarefa → cobertura → TIPOS_EM_ABERTO → vigente()` continua alcançando `time.monotonic()` e atualização do cache do catálogo. Declarar catálogo atual não remove essa leitura de relógio. (`app/chamados/services/tarefa_do_dia.py:366`, `app/ponto/turnos.py:2762`, `app/ponto/turnos.py:2774`, `app/ponto/models.py:689`, `app/ponto/catalogo/ausencias.py:403`, `app/ponto/catalogo/ausencias.py:433`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/catalogo/ausencias.py:279`) O selo AST limita-se aos módulos da tarefa e da lavra; seu veneno dinâmico intercepta `timezone.now`, deixando esse helper fora. (`app/chamados/tests/test_e2_tarefa_do_dia.py:929`, `app/chamados/tests/test_e2_tarefa_do_dia.py:991`, `app/chamados/tests/test_e2_tarefa_do_dia.py:678`)

4. **Segundo juiz, segundo escritor e derivação paralela — não achei no delta examinado.** As mudanças são filtros de leitura, com passivo pela autoridade E1; o efeito mantém `batidas_apuraveis`, o teto mantém o corpo do cartório e a posição mantém a função compartilhada. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/models.py:3399`, `app/chamados/services/tarefa_do_dia.py:328`, `app/ponto/turnos.py:3059`, `app/ponto/services/cartorio.py:858`, `app/chamados/services/tarefa_do_dia.py:55`, `app/chamados/services/declaracao_texto.py:75`) A existência do novo juiz e a coexistência provisória dos cinco respondedores têm corte explícito. (`app/core/juizes.py:150`, `app/docs/CORTES.md:424`)

5. **Queries crescendo com N — não achei SELECT por pergunta nos ramos novos; constância absoluta não certificada.** Perguntas carregam disputa, decisões carregam ausência e a correspondência do passivo usa consulta de lote. (`app/chamados/services/tarefa_do_dia.py:302`, `app/chamados/services/tarefa_do_dia.py:311`, `app/chamados/models.py:3412`) O teste compara lotes e inclui validações, mas não exercita ausências. (`app/chamados/tests/test_e2_tarefa_do_dia.py:893`, `app/chamados/tests/test_e2_tarefa_do_dia.py:904`, `app/chamados/tests/test_e2_tarefa_do_dia.py:918`) O corpo da cobertura consulta o conjunto vivo por ausência, cujo acesso ao banco depende do TTL; não medi SQL nem confirmei crescimento real. (`app/ponto/turnos.py:2774`, `app/ponto/catalogo/ausencias.py:276`, `app/ponto/catalogo/ausencias.py:278`) Medição produtiva: **[nao verificado]**.

A cópia não contém repositório Git utilizável; a reconstrução anterior usou exclusivamente o delta fornecido. Identidade histórica dos commits: **[nao verificado]**. [nao verificado]

## 07/11 12:37 · sessao 67c07a42d4d7 · - · - · origem=Ronald (tmux)

**Classe: correspondência via/ato; produção sem linha não demonstrada.** O único chamador produtivo explícito que achei para `VIA_MOTIVO_RETRATADO` foi `DisputaSupervisao.remover_motivo`: registra `RETRATACAO` e carimba a via no mesmo `transaction.atomic`. A busca percorreu Python da cópia, excluindo testes, migrações e logs; ocorrência produtiva de via sem linha **[nao verificado]**. (`app/chamados/models.py:1986`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`)

**A porta genérica permite a combinação sem linha, e existe testemunha de teste.** `carimbar_identidade` valida a participação na lista de vias e salva o carimbo, sem registrar decisão. `test_RED_retratada_nao_esta_no_dominio_do_lote` chama diretamente essa porta com `VIA_MOTIVO_RETRATADO`, sem criar a retratação. Isso prova possibilidade pela API e uso em teste; não prova segundo escritor produtivo. (`app/chamados/models.py:2727`, `app/chamados/models.py:2736`, `app/chamados/models.py:2746`, `app/chamados/tests/test_e1_volta2.py:128`, `app/chamados/tests/test_e1_volta2.py:131`)

Para essa pergunta **sem qualquer linha `retratacao`**, a via `motivo_retratado` não a exclui mais em nenhum `agora`: o filtro atual exclui apenas `VIA_DUPLICATA` pela via e procura retratação na sequência cortada. Se ela for a pergunta escolhida para um marco vencido/apagado e passar pelas guardas anteriores, seguirá `_pela_decisao`: sem decisões/passivo, `aguarda_resposta/responder`; com resposta, `aguarda_validacao/validar`; com passivo, `passivo_sem_linha/nao_sei`. A aplicação concreta depende das demais guardas e perguntas concorrentes. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:121`, `app/chamados/services/tarefa_do_dia.py:123`, `app/chamados/services/tarefa_do_dia.py:129`)

**Migração/backfill: não achei criação dessa inconsistência.** A 0051 adiciona a escolha `motivo_retratado`; as decisões anteriores recebem `ato='reabertura'`. O preenchimento de dados da migração altera colaborador/instância, não essa via. `backfill_decisoes_pergunta` é DRY e calcula somente respostas/validações faltantes. (`app/chamados/migrations/0051_decisao_pergunta.py:22`, `app/chamados/migrations/0051_decisao_pergunta.py:49`, `app/chamados/migrations/0051_decisao_pergunta.py:61`, `app/chamados/migrations/0051_decisao_pergunta.py:111`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:7`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`)

**Escritor de duplicata:** `fio.retratar_duplicata → carimbar_identidade → _escrever_carimbo`; o helper não cria decisão `retratacao`. Os dois chamadores produtivos explícitos achados são `dedup_perguntas_fato` e `migrar_resolvedoras_vivas`. `manter_carimbo=True` pode preservar instante/autor antigos enquanto troca a via. (`app/chamados/services/fio.py:939`, `app/chamados/services/fio.py:952`, `app/chamados/models.py:2658`, `app/chamados/models.py:2738`, `app/chamados/management/commands/dedup_perguntas_fato.py:88`, `app/chamados/services/fio.py:1004`)

**Escritor de decisão `retratacao`:** achei o chamador produtivo `remover_motivo`, que passa por `DecisaoPergunta.registrar`, único corpo que realiza o `bulk_create` da sequência. O teste novo escreve uma retratação diretamente por `registrar` e muda a via por `update`, como fixture; esse segundo caminho é de teste. (`app/chamados/models.py:1989`, `app/chamados/models.py:3430`, `app/chamados/tests/test_e2_tarefa_do_dia.py:757`, `app/chamados/tests/test_e2_tarefa_do_dia.py:759`)

**Leitores relevantes das vias:**

| Leitor | Uso |
|---|---|
| Domínio do fato | Exclui ambas por `VIAS_FORA_DO_FATO`. (`app/chamados/models.py:2450`, `app/chamados/models.py:2467`) |
| Emissão e censo de colisões | Excluem ambas pela mesma lista. (`app/chamados/services/disputa_emissao.py:323`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:74`) |
| `q_vivas` e juiz de vida | Leem via/carimbo atuais; motivo retratado vence a resposta. (`app/chamados/models.py:2888`, `app/chamados/juizes.py:1147`) |
| Juiz do lastro | Lê via+carimbo e conjunto de identidade. (`app/chamados/juizes.py:648`, `app/chamados/juizes.py:658`) |
| Fábrica da célula e cartório | Têm leitores específicos de duplicata. (`app/chamados/services/fabrica_da_celula.py:119`, `app/ponto/services/cartorio.py:1610`) |
| Tarefa nova | Exclui duplicata pela via atual e retratação por qualquer decisão aplicável. (`app/chamados/services/tarefa_do_dia.py:208`, `app/chamados/services/tarefa_do_dia.py:210`) |

`respostas_anteriores` percorre a sequência, mas só trata `resposta` e atos com foto (`reabertura/recusa`); não transforma retratação em resposta anterior. (`app/chamados/models.py:3354`, `app/chamados/models.py:2993`, `app/chamados/models.py:2998`)

## 07/11 12:22 · sessao 17aa803bf615 · FIO-E0-V4-E1 · verificador · origem=Code

**Há falhas procedentes em identidade, horizonte, travas, backfill e cobertura dos testes.** As principais são a seleção anterior à canônica, o tratamento diferente de `NULL` e as travas adquiridas por caminhos indiretos depois da Pergunta. (`app/chamados/services/declaracao_texto.py:256`; `app/chamados/services/fio.py:1030`; `app/chamados/services/validacao.py:284`)

Execução dos testes, migrações efetivamente aplicadas e ocorrência ou quantidade desses casos em produção: **[nao verificado]**. [nao verificado]

**A. PROCEDENTE — os campos não são interpretados uniformemente por todos os caminhos.**

O mapa confirmado de escritores e leitores é este; exaustividade nominal de cada ocorrência indireta no repositório: **[nao verificado]**.

| Campo/valor | Quem escreve | Quem lê / significado |
|---|---|---|
| `data_evento` | Nascimento em `_nascer`; datação e rechaveamento no fio. (`app/chamados/models.py:2687`; `app/chamados/services/fio.py:841`; `app/chamados/services/fio.py:948`) | Identidade, horizonte, plantio, C6 e apresentação. (`app/chamados/models.py:2549`; `app/chamados/services/fio.py:1030`; `app/chamados/services/materializacao.py:344`; `app/chamados/juizes.py:1200`; `app/api/views_mensageria.py:568`) |
| `instancia_marco` | `_chave_de_identidade`, derivada do motivo e conferida na inserção; acervo preenchido pela 0051. (`app/chamados/models.py:3084`; `app/chamados/models.py:3100`; `app/chamados/migrations/0051_decisao_pergunta.py:27`) | Dedup deriva a instância; índice e agrupamento leem a coluna persistida. (`app/chamados/models.py:2549`; `app/chamados/models.py:3147`; `app/chamados/services/fio.py:1016`) |
| `batida_origem` | Nascimento, derivando `ultima_batida.id` e conferindo o colaborador. (`app/chamados/models.py:3112`; `app/chamados/models.py:3127`; `app/chamados/models.py:3131`) | Emissor, dedup por episódio, índice, agrupamento e contador. (`app/chamados/services/disputa_emissao.py:326`; `app/chamados/models.py:2518`; `app/chamados/models.py:3153`; `app/chamados/services/fio.py:1017`; `app/chamados/services/fio.py:1099`) |
| `VIA_DUPLICATA` | `retratar_duplicata` → `carimbar_identidade` → escritor do carimbo. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2770`) | Exclusão do fato, C6, `q_vivas` e índice. (`app/chamados/models.py:2551`; `app/chamados/juizes.py:1148`; `app/chamados/models.py:3016`; `app/chamados/models.py:3149`) |
| `VIA_MOTIVO_RETRATADO` | `remover_motivo`, pela porta do carimbo. (`app/chamados/models.py:2012`) | As mesmas tuplas de exclusão e terminalidade. (`app/chamados/models.py:2363`; `app/chamados/models.py:2373`; `app/chamados/models.py:2551`) |
| `SILENCIO` | É ato de `DecisaoPergunta`, escrito por `registrar`, nos fechamentos e no backfill. (`app/chamados/models.py:3522`; `app/chamados/models.py:3607`; `app/chamados/models.py:1888`; `app/ponto/services/veredito_celula.py:192`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:148`) | O backfill o consulta para idempotência; o leitor de respostas anteriores não o transforma em resposta vigente. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:90`; `app/chamados/models.py:3172`) |
| `conversa` | As portas passam explicitamente `False` ou `True` à inserção comum; a 0051 não distingue as conversas antigas. (`app/chamados/models.py:2582`; `app/chamados/models.py:2593`; `app/chamados/models.py:2688`; `app/chamados/migrations/0051_decisao_pergunta.py:5`) | Dedup e índice excluem conversa; os filtros remanescentes examinados em E não fazem isso. (`app/chamados/models.py:2550`; `app/chamados/models.py:3148`; `app/chamados/services/declaracao_texto.py:236`; `app/chamados/services/disputa_emissao.py:1302`) |
| `chamado_destino` | Nascimento, reapontamento do dedup e porta de costura. (`app/chamados/models.py:2688`; `app/chamados/models.py:2659`; `app/chamados/services/fio.py:869`) | Juiz da costura, C6, materializador, resolver e API. (`app/chamados/juizes.py:1966`; `app/chamados/juizes.py:744`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/disputa_emissao.py:1693`; `app/api/views.py:2590`) |
| `DisputaSupervisao.aberta_em` | `auto_now_add`: nascimento da disputa. (`app/chamados/models.py:1709`) | Ordenação das disputas e substituto do nascimento da pergunta no contador novo. (`app/chamados/models.py:1783`; `app/chamados/services/fio.py:1100`) |
| `HORIZONTE_DO_FATO` | Literal no modelo e cópia congelada na 0053. (`app/chamados/models.py:2253`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:28`) | Índices, agrupamento, backfill, contador e diagnóstico por empresa. (`app/chamados/models.py:3150`; `app/chamados/services/fio.py:1030`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:54`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:195`) |
| Registro aplicado da 0052 | O arquivo declara a criação da coluna; escritor efetivo do registro nesta instalação: **[nao verificado]**. (`app/chamados/migrations/0052_pergunta_batida_origem.py:18`) | O contador lê `django_migrations.applied` e compara com a abertura da disputa. (`app/chamados/services/fio.py:1097`; `app/chamados/services/fio.py:1101`) |

`SILENCIO` não é valor de `via_resolucao`: pertence a outra entidade e outro vocabulário. (`app/chamados/models.py:2410`; `app/chamados/models.py:3522`)

`data_evento` tem dois domínios declarados: dia do turno para fato e dia civil da emissão para conversa. O discriminador é `conversa`; o defeito aparece quando leitores ignoram esse discriminador, como em E. (`app/chamados/models.py:2576`; `app/chamados/models.py:2588`; `app/chamados/services/declaracao_texto.py:236`)

A configuração usa `America/Sao_Paulo` e `USE_TZ=True`; não há troca explícita entre segundos e dias na comparação de instantes do contador. A divergência demonstrada é usar o nascimento da **disputa** como nascimento da **pergunta**. (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/chamados/services/fio.py:1100`)

**B. PROCEDENTE — o recorte não é o mesmo em todos os casos.**

- **Datas preenchidas: NAO PROCEDE a divergência de operador.** Modelo, migration e fio usam `>= 2026-09-21`. (`app/chamados/models.py:3150`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/services/fio.py:1030`)
- **`NULL`: PROCEDENTE.** O índice exclui a data nula pelo `>=`; `q_do_horizonte` a inclui expressamente. Para BATIDA com origem e sem dia, `grupos_de_colisao` pode inclusive formar grupo e encaminhá-lo à retratação, embora o índice não alcance essas linhas. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1060`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:166`)
- **O desvio chega à escrita.** `admin_decidiu_sem_ato` inclui pergunta sem dia; competência desconhecida é tratada como não exportada; `aplicar` grava seu SILENCIO. Não fica apenas contada fora do horizonte. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:94`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:105`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:239`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:147`)
- **Corte por empresa: PROCEDENTE.** A autoridade aceita corte por empresa, mas o índice fixa 21/09 para todas. `_horizonte_por_empresa` imprime `DIVERGE` e o comando continua, apesar de sua docstring anunciar PAREI. Existência de empresa divergente em produção: **[nao verificado]**. (`app/ponto/janelas.py:36`; `app/ponto/janelas.py:60`; `app/chamados/models.py:2253`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:189`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:200`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:204`)
- **Porta: NAO PROCEDE recorte indevido.** `abrir`, `do_fato` e `q_dos_fatos` continuam vendo o fato antigo sem aplicar esse horizonte. (`app/chamados/models.py:2513`; `app/chamados/models.py:2549`; `app/chamados/models.py:2639`)

**C. PROCEDENTE — a trava antecipada não cobre todos os caminhos nem todas as linhas.**

| Sítio | Violação demonstrada |
|---|---|
| `DisputaSupervisao.fechar` | Antecipa somente a própria disputa, pai e destinos. Depois de atualizar Pergunta, salva Disputa; S88 reconcilia o pai, S143 chama o resolver, que trava todas as outras disputas abertas. Outra disputa pode entrar depois de Pergunta/Chamado. (`app/chamados/models.py:1874`; `app/chamados/models.py:1884`; `app/chamados/models.py:1899`; `app/chamados/signals.py:42`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1663`) |
| `fechar_o_fio_do_dia` | A lista promete pais, mas acrescenta apenas chamados vivos do dia e destinos. Após carimbar perguntas, o reconciliar pode alcançar S143 e adquirir pais ou outras disputas ausentes da lista. (`app/ponto/services/veredito_celula.py:179`; `app/ponto/services/veredito_celula.py:184`; `app/ponto/services/veredito_celula.py:186`; `app/ponto/services/veredito_celula.py:199`; `app/chamados/services/disputa_emissao.py:1663`) |
| Validação | O aceite de folga desvia antes da trava nova e carimba Pergunta antes do fechamento do fio. No ramo normal, clones de `chamados_relacionados_ids` ficam fora da lista pai+destino e são reconciliados depois do carimbo. (`app/chamados/services/validacao.py:404`; `app/chamados/services/validacao.py:284`; `app/chamados/services/validacao.py:292`; `app/chamados/services/validacao.py:423`; `app/chamados/services/validacao.py:425`; `app/chamados/services/materializacao.py:766`) |
| `fechar_beco` | Trava somente `d` antes dos chamados; em seguida chama resolver, que trava todas as disputas abertas. Uma segunda disputa entra depois de Chamado. (`app/chamados/services/becos.py:51`; `app/chamados/services/becos.py:53`; `app/chamados/services/disputa_emissao.py:1663`) |
| Resolver | Via S143, o Chamado já foi travado e salvo quando o wrapper pede Disputa. Além disso, coleta `_disp` antes de travar Colaborador e depois o corpo consulta novamente todas as abertas. (`app/chamados/models.py:688`; `app/chamados/models.py:291`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1773`) |

A validação ainda chama o materializador geral fora de seu atomic local; esse materializador percorre irmãs cujos destinos não constam da lista antecipada da pergunta original. (`app/chamados/services/validacao.py:459`; `app/chamados/services/materializacao.py:1360`)

**BUG 120/S143: NAO PROCEDE como regressão de commit parcial demonstrada.** S143 já envolve o resolver inteiro em savepoint e captura a exceção fora dele; o novo atomic não remove essa proteção. Dependência de sucesso parcial nos outros chamadores: **[nao verificado]**. (`app/chamados/signals.py:83`; `app/chamados/signals.py:91`; `app/ponto/management/commands/reconciliar_fantasmas.py:280`)

**D. PROCEDENTE — há nascimento repetido fora do caminho nominal de episódio.**

Com `ultima_batida.id` apontando para batida retratada, o derivador de origem ainda devolve seu ID, então o filtro novo pula o dedup `(disputa,motivo)`. Entretanto, o juiz do dia exclui essa batida e devolve `None`. Para uma escolha como `gps_ausente`, sem outra data no contexto, o gerador cai em `abrir_conversa`, que nasce sempre e não persiste origem. Repetir a emissão pode criar duas conversas referentes ao mesmo episódio. (`app/chamados/models.py:3131`; `app/chamados/services/disputa_emissao.py:326`; `app/ponto/turnos.py:2905`; `app/ponto/turnos.py:2917`; `app/chamados/catalogo/perguntas.py:88`; `app/chamados/services/disputa_emissao.py:429`; `app/chamados/models.py:2586`; `app/chamados/models.py:3112`)

**Sem episódio conhecido, NAO PROCEDE a perda do dedup sequencial:** o filtro `(disputa,motivo)` permanece. Ele não serializa duas emissões concorrentes de conversa: a conversa não toma a trava do colaborador nem passa pelo índice de fato. Reprodução concorrente executada: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:325`; `app/chamados/models.py:2607`; `app/chamados/models.py:3148`)

O teste novo cobre o caminho nominal: dois episódios distintos, mesma disputa e repetição do segundo sem terceira pergunta. Não cobre batida retratada ou conversa sem origem. (`app/chamados/tests/test_e1_v6_horizonte.py:245`)

**E. PROCEDENTE — os remanescentes ainda divergem da identidade e da canônica.**

1. **A primeira reutilizável ainda vence a canônica.** Abaixo do horizonte, considere `p1` muda sem carimbo e `p2` respondida sem carimbo. Ambas são reutilizáveis; `_segura_da_lista` escolhe `p1`, enquanto `abrir` escolhe `p2` pela resposta. A troca para canônica só acontece no fallback. (`app/chamados/juizes.py:1228`; `app/chamados/services/declaracao_texto.py:256`; `app/chamados/models.py:2639`; `app/chamados/services/fio.py:985`)

2. **Conversa administrativa pode ser reutilizada como pergunta de marco.** A reabertura aceita motivo MARCO e cria conversa com esse motivo. `_candidata_reusavel` filtra somente disputa/motivo/dia e aceita a conversa sem carimbo; a declaração então a reutiliza e não chama `abrir`. O domínio do índice exclui essa mesma conversa. (`app/chamados/services/acoes_disputa.py:247`; `app/chamados/services/acoes_disputa.py:260`; `app/chamados/services/declaracao_texto.py:236`; `app/chamados/services/declaracao_texto.py:326`; `app/chamados/services/declaracao_texto.py:336`; `app/chamados/models.py:2550`)

3. **`_ja_perguntou` também inclui conversa.** `fio_vivo` julga a coleção recebida; não corrige o domínio escolhido pelo chamador. Já a tela em lote usa `linhas_dos_fatos` e exclui conversa. (`app/chamados/services/disputa_emissao.py:1302`; `app/chamados/juizes.py:1242`; `app/chamados/services/declaracao_texto.py:550`)

4. **Há outros filtros próprios:** o gerador conserva dedups por `(disputa,motivo,data)`, sem excluir conversa nem conferir instância, e `do_fato` conserva uma união deliberada com legado sem origem do mesmo dia, além da chave do episódio. (`app/chamados/services/disputa_emissao.py:399`; `app/chamados/services/disputa_emissao.py:416`; `app/chamados/models.py:2516`)

**NAO PROCEDE a objeção específica sobre retratadas válidas nesses remanescentes:** ambas as retratações recebem carimbo e morrem pelo juiz; `_derivar_chamado_destino` exclui as materializadas. Essa justificativa não resolve a inclusão de conversa nos demais filtros. (`app/chamados/models.py:2012`; `app/chamados/services/fio.py:1002`; `app/chamados/juizes.py:1148`; `app/chamados/services/disputa_emissao.py:228`)

**F. PROCEDENTE para a garantia “em todo caminho”; NAO PROCEDE no CLI sem alteração concorrente.**

No CLI, mérito e competência exportada são recusados antes de chamar `aplicar`; dentro da aplicação, SILENCIO precede as retratações. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:283`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:287`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:139`)

A função `aplicar` diretamente, porém, grava SILENCIO em atomics separados **antes** de examinar mérito; quando encontra mérito, apenas acrescenta `merito_recusado` e continua. Ela também não verifica exportada. O teste chama essa função diretamente, mas com `sem_ato=[]`, deixando a escrita anterior à recusa sem cobertura. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:139`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:160`; `app/chamados/tests/test_e1_v6_horizonte.py:196`)

Portanto, o helper pode tocar pergunta de competência exportada quando chamado diretamente; uso desse caminho em produção: **[nao verificado]**. O CLI também não relê a condição de exportada sob a trava da aplicação. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:125`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:238`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:291`)

**G. PROCEDENTE — a partição fecha, mas “nova” não mede nascimento de pergunta.**

A função particiona as linhas em `novas` e `acervo`; assim, `n + len(acervo) == universo`. Entretanto, o universo inclui data nula, e `n` depende da abertura da disputa. (`app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:1099`; `app/chamados/services/fio.py:1101`; `app/chamados/services/fio.py:1104`)

**Pergunta nova em disputa velha é caminho existente:** a emissão reutiliza disputa aberta e gera perguntas depois. Essa pergunta entra em `acervo`, podendo aumentar a suposta linha de base que “só desce”. (`app/chamados/services/disputa_emissao.py:524`; `app/chamados/services/disputa_emissao.py:659`; `app/chamados/services/fio.py:1093`)

Pergunta antiga transferida para disputa nova: **[nao verificado]**. As portas examinadas de reapontamento alteram destino/célula, preservando a disputa. (`app/chamados/services/fio.py:869`; `app/chamados/models.py:2687`)

Se o registro da 0052 não existir, nenhuma linha será classificada como nova: tudo cai em acervo e `n=0`. O código não distingue essa ausência de uma medição válida sem novas. (`app/chamados/services/fio.py:1097`; `app/chamados/services/fio.py:1101`)

O placar lê a função real, mas imprime somente as primeiras dez novas e apenas a quantidade do acervo; a função conserva as listas completas. (`bin/placar_code.sh:198`; `app/chamados/services/fio.py:1105`)

**H. PROCEDENTE — há selos com alcance menor que o anunciado e relógios soltos.**

- **Travas:** o rastreador elimina toda segunda aquisição da mesma tabela. Não detecta outro Chamado ou outra Disputa adquiridos depois da Pergunta quando a tabela já apareceu. (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`; `app/chamados/tests/test_e1_v5_ordem_trava.py:45`)
- **Resolver:** o teste fecha por `.update()` fora da observação, chama diretamente e exige apenas Disputa/Chamado. Pode passar sem carimbar Pergunta e não exercita S143. (`app/chamados/tests/test_e1_v5_ordem_trava.py:331`; `app/chamados/tests/test_e1_v5_ordem_trava.py:335`)
- **Canônica da tela:** o novo teste materializa e valida ambas as perguntas, forçando o fallback. Não alcança o contraexemplo da primeira reutilizável de E. (`app/chamados/tests/test_e1_v6_horizonte.py:156`; `app/chamados/services/declaracao_texto.py:256`)
- **Relógio:** a declaração “todo instante é literal” não se sustenta. A fixture usa `aberta_em=auto_now_add`, os casos chamam `fechar_por_admin` que lê `now()`, e o contador compara abertura e `applied` sem fixar essa fronteira. (`app/chamados/tests/test_e1_v6_horizonte.py:14`; `app/chamados/tests/test_e1_v6_horizonte.py:74`; `app/chamados/models.py:1709`; `app/chamados/models.py:2733`; `app/chamados/tests/test_e1_v6_horizonte.py:210`)
- **Fronteira do índice:** o arquivo testa banco abaixo/no horizonte para MARCO; a inspeção das condições verifica folhas do `Q`, sem provar sua composição inteira nem a fronteira BATIDA equivalente. (`app/chamados/tests/test_e1_v6_horizonte.py:47`; `app/chamados/tests/test_e1_v6_horizonte.py:94`)

**NAO PROCEDE vacuidade generalizada:** porta antiga, canônica de `abrir`, SILENCIO, mérito, emissor e fábrica verificam resultados concretos; o RED do fio mudo exige uma pergunta antes e duas depois, com relógio congelado. (`app/chamados/tests/test_e1_v6_horizonte.py:113`; `app/chamados/tests/test_e1_v6_horizonte.py:146`; `app/chamados/tests/test_e1_v6_horizonte.py:171`; `app/chamados/tests/test_e1_v6_horizonte.py:245`; `app/chamados/tests/test_e1_v6_horizonte.py:272`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:44`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:169`)

A API continua verificando a irmã materializada e agora cria motivos distintos; não prova o índice do horizonte, pois usa maio de 2026. (`app/api/tests/test_falta_na_resposta.py:10`; `app/api/tests/test_falta_na_resposta.py:29`; `app/api/tests/test_falta_na_resposta.py:62`)

**I. PROCEDENTE — classes ante

[truncado em 20000 de 21495 caracteres]

## 07/11 12:20 · sessao bef057c991f3 · - · - · origem=Ronald (tmux)

**Sim, confirmado.** Com `p1` muda sem carimbo e `p2` respondida sem carimbo, ambas passam `_reusavel` porque `materializada_em is None` (`app/chamados/juizes.py:1228`). `_segura_da_lista` devolve imediatamente `p1`, a primeira da lista (`app/chamados/services/declaracao_texto.py:256`); `abrir` consulta a mesma lista e escolhe `p2` por `canonica_do_grupo` (`app/chamados/models.py:2639`; `app/chamados/services/fio.py:985`).

O teste novo carimba **e valida ambas**, fazendo `_reusavel=False` e exercitando apenas o fallback canônico (`app/chamados/tests/test_e1_v6_horizonte.py:156`; `app/chamados/services/declaracao_texto.py:259`). Ele não detecta o contraexemplo (`app/chamados/tests/test_e1_v6_horizonte.py:161`).

Precisão: `declarar_hora_colab` também prefere a primeira reutilizável, por `_candidata_reusavel`; portanto a divergência demonstrada é entre a seleção reutilizável da declaração/tela e a seleção de `PerguntaDisputa.abrir` (`app/chamados/services/declaracao_texto.py:236`; `app/chamados/services/declaracao_texto.py:326`).

## 07/11 12:19 · sessao bef057c991f3 · - · - · origem=Ronald (tmux)

**D — PROCEDENTE, caminho de duplicação com episódio conhecido mas retratado.**

`batida_origem_do_nascimento` aceita a batida pelo ID e colaborador, sem excluir `retratada_em` (`app/chamados/models.py:3131`). O novo filtro pula o dedup `(disputa,motivo)` quando essa origem existe (`app/chamados/services/disputa_emissao.py:325`). Já a derivação do dia usa `_T1`, que exclui batidas retratadas e devolve `None` (`app/chamados/services/disputa_emissao.py:388`; `app/ponto/turnos.py:2905`; `app/ponto/turnos.py:2917`). Para `gps_ausente`, pergunta de escolha sem `{data}`, a execução então entra em `abrir_conversa` (`app/chamados/catalogo/perguntas.py:88`; `app/chamados/services/disputa_emissao.py:429`). Essa porta nasce sempre, sem dedup, e não grava origem para conversa (`app/chamados/models.py:2586`; `app/chamados/models.py:3112`). Repetir o gerador com esse contexto pode criar duas conversas referentes ao mesmo episódio; ocorrência em produção [nao verificado].

Sem episódio conhecido, o dedup sequencial `(disputa,motivo)` continua presente e exclui as duas vias retratadas (`app/chamados/services/disputa_emissao.py:325`). Sua proteção contra concorrência não está no filtro: o gerador é chamado depois de `_abrir_ou_agregar_disputa` retornar, fora do `atomic` interno dessa função, e conversa não trava colaborador (`app/chamados/services/disputa_emissao.py:512`; `app/chamados/services/disputa_emissao.py:651`; `app/chamados/services/disputa_emissao.py:659`; `app/chamados/models.py:2607`). Dois emissores simultâneos sem data/origem podem ambos passar no `.exists()` e nascer como conversa; reprodução executada [nao verificado].

A porta também junta deliberadamente o episódio conhecido com qualquer linha legada sem origem do mesmo motivo/dia: `_dia_sem_episodio` entra no `OR` de `do_fato` (`app/chamados/models.py:2516`). Assim, dois episódios distintos podem ser ambos retidos pela mesma linha legada sem origem; é coalescência expressamente documentada, não equivalência estrita com a chave `(batida_origem,motivo)` do índice (`app/chamados/models.py:2511`; `app/chamados/models.py:3153`).

**E — PROCEDENTE, conversa administrativa continua atravessando os dedups antigos.**

Há caminho normal, sem dados corrompidos: `acao_reabrir_disputa_admin` aceita um motivo MARCO e cria conversa com esse mesmo motivo, `tipo_resposta='escolha'` (`app/chamados/services/acoes_disputa.py:247`; `app/chamados/services/acoes_disputa.py:260`). No mesmo dia, `_candidata_reusavel` filtra só disputa/motivo/data e aceita a conversa não carimbada (`app/chamados/services/declaracao_texto.py:236`; `app/chamados/juizes.py:1228`). `declarar_hora_colab` a reutiliza e só chama `abrir` se não houver candidata (`app/chamados/services/declaracao_texto.py:326`; `app/chamados/services/declaracao_texto.py:336`). A conversa passa a receber declaração destinada ao marco, embora `q_dos_fatos` e índice a excluam (`app/chamados/models.py:2550`; `app/chamados/models.py:3148`).

A mesma conversa viva faz `_ja_perguntou` calar o fato: sua consulta inclui conversa, e `fio_vivo` reduz o universo recebido pelo C6, sem excluir conversa (`app/chamados/services/disputa_emissao.py:1302`; `app/chamados/juizes.py:1236`). Já a tela em lote consulta `linhas_dos_fatos`, portanto exclui essa conversa (`app/chamados/services/declaracao_texto.py:550`). É divergência concreta de domínio entre leitor e escritor (`app/chamados/models.py:2550`).

A justificativa específica sobre **retratadas válidas** se sustenta: remoção grava carimbo junto com a via; duplicata também passa pela porta de carimbo, e ambas morrem antes da resposta no C6 (`app/chamados/models.py:2012`; `app/chamados/services/fio.py:1002`; `app/chamados/models.py:2850`; `app/chamados/juizes.py:1148`). `_derivar_chamado_destino` exclui as materializadas; isso exclui essas retratadas, mas não resolve o domínio de conversa dos outros dois leitores (`app/chamados/services/disputa_emissao.py:227`).

Outros filtros próprios ainda existem em `gerar_perguntas_disputa`: `(disputa,motivo,data)` em `app/chamados/services/disputa_emissao.py:399` e `app/chamados/services/disputa_emissao.py:416`; faltam `conversa=False` e a coluna de instância, diferentemente de `app/chamados/models.py:2549`. O problema de conversa acima também alcança esses filtros quando as datas coincidem; ocorrência em produção [nao verificado].

**A — mapa dos escritores/leitores e significados.**

| Campo/valor | Escritores | Leitores relevantes |
|---|---|---|
| `data_evento` | Nascimento de fato em `app/chamados/models.py:2569`, conversa com `localdate()` em `app/chamados/models.py:2593`, inserção comum em `app/chamados/models.py:2687`; datação/rechaveamento em `app/chamados/services/fio.py:841` e `app/chamados/services/fio.py:948`. | Identidade em `app/chamados/models.py:2549`; horizonte/agrupamento em `app/chamados/services/fio.py:1024`; plantio em `app/chamados/services/materializacao.py:344`; C6 em `app/chamados/juizes.py:1200`; apresentação/API em `app/api/views_mensageria.py:568` e `app/api/views.py:1184`; calendário em `app/colaboradores/services/calendario.py:473`. |
| `instancia_marco` | Derivada e conferida só na inserção em `app/chamados/models.py:3084` e `app/chamados/models.py:3100`; migração de acervo em `app/chamados/migrations/0051_decisao_pergunta.py:27`. | Derivador em `app/chamados/services/declaracao_texto.py:55`; dedup em `app/chamados/models.py:2549`; chave persistida do índice em `app/chamados/models.py:3147`; agrupamento em `app/chamados/services/fio.py:1016`. |
| `batida_origem` | Na inserção, pelo contexto `ultima_batida.id`, em `app/chamados/models.py:3112` e `app/chamados/models.py:3127`; contexto alimentado pelas duas APIs em `app/api/views.py:788` e `app/api/views_core.py:788`. | Dedup de episódio em `app/chamados/models.py:2518`; filtro novo do emissor em `app/chamados/services/disputa_emissao.py:326`; índice em `app/chamados/models.py:3153`; agrupamento/contador em `app/chamados/services/fio.py:1017` e `app/chamados/services/fio.py:1099`. |
| `conversa` | As duas portas passam `False`/`True` à inserção comum (`app/chamados/models.py:2582`; `app/chamados/models.py:2593`; `app/chamados/models.py:2688`); legado inteiro recebe default `False`, sem distinguir conversas antigas (`app/chamados/migrations/0051_decisao_pergunta.py:5`). | Domínio de fato em `app/chamados/models.py:2550`; índices em `app/chamados/models.py:3148`; migração de resolvedoras em `app/chamados/services/fio.py:1171`; falta nos filtros defeituosos citados em E. |
| `chamado_destino` | Nascimento em `app/chamados/models.py:2688`, reapontamento da identidade em `app/chamados/models.py:2659`, porta de costura em `app/chamados/services/fio.py:869`. | Juiz da costura em `app/chamados/juizes.py:1966`; âncora do fio em `app/chamados/services/fio.py:97`; C6 em `app/chamados/juizes.py:744`; API em `app/api/views.py:2590`; filtros próprios em `app/chamados/services/disputa_emissao.py:227` e `app/chamados/services/disputa_emissao.py:968`. |
| `VIA_DUPLICATA` | `retratar_duplicata` → `carimbar_identidade` → `_escrever_carimbo` (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2851`; `app/chamados/models.py:2770`). | Domínio de fato em `app/chamados/models.py:2551`; C6 em `app/chamados/juizes.py:1148`; SQL `q_vivas` em `app/chamados/models.py:3016`; índice em `app/chamados/models.py:3149`; tratamento externo no cartório em `app/ponto/services/cartorio.py:1599`. |
| `VIA_MOTIVO_RETRATADO` | `remover_motivo` → porta do carimbo (`app/chamados/models.py:2012`). | Mesmas tuplas de exclusão e terminalidade acima (`app/chamados/models.py:2360`; `app/chamados/models.py:2373`; `app/chamados/models.py:2551`). |
| `SILENCIO` | É **ato de `DecisaoPergunta`**, não valor de `via_resolucao`: declarado em `app/chamados/models.py:3522`; escrito por `registrar` em `app/chamados/models.py:3607`, chamado no fechamento da disputa (`app/chamados/models.py:1888`), fechamento do dia (`app/ponto/services/veredito_celula.py:192`) e backfill (`app/chamados/management/commands/backfill_decisoes_pergunta.py:148`). | Idempotência do backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:90` e `app/chamados/management/commands/backfill_decisoes_pergunta.py:145`; `respostas_anteriores` só trata RESPOSTA e ATOS_COM_FOTO, sem transformar SILENCIO em resposta vigente (`app/chamados/models.py:3172`). |

`data_evento` possui dois significados declarados e discriminados por `conversa`: dia do turno para fato, dia civil da emissão para conversa (`app/chamados/models.py:2576`; `app/chamados/models.py:2588`). A falha demonstrada em E é justamente um leitor/escritor que ignora esse discriminador (`app/chamados/services/declaracao_texto.py:236`). Datas são `DateField`; instantes usam configuração `America/Sao_Paulo`, `USE_TZ=True` (`app/chamados/models.py:2273`; `app/config/settings/base.py:93`). O backfill aplica `q_do_horizonte` também às conversas, sem filtro próprio, portanto o mesmo corte lê o domínio civil delas (`app/chamados/management/commands/backfill_decisoes_pergunta.py:52`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:92`).

Não encontrei reparent de pergunta entre disputas nos caminhos examinados; não afirmo censo negativo fechado [nao verificado]. O nascimento fixa a disputa e o reapontamento muda destino/célula, sem mudar disputa (`app/chamados/models.py:2687`; `app/chamados/services/fio.py:869`). Portanto o contraexemplo de G sustentado pelo código é pergunta **nova em disputa velha**, explicitamente admitido no contador; pergunta antiga transferida para disputa nova [nao verificado].

`abrir(..., batida_origem=b)` aceita o objeto via `**campos`, mas o dedup só consulta `campos.get('batida_origem_id')`; esse formato não alimenta `_bo` (`app/chamados/models.py:2569`; `app/chamados/models.py:2632`). Não encontrei chamador de produção usando essa forma; alcance prático [nao verificado].

## 07/11 12:18 · sessao 5184053a97bf · - · - · origem=Ronald (tmux)

C — **PROCEDENTE: a ordem global ainda quebra em caminhos alcançados pelos cinco sítios.**

1. **`DisputaSupervisao.fechar`:** o corpo antecipa `self`, pai e destinos das vivas (`app/chamados/models.py:1874`). Porém `save()` dispara S88 (`app/chamados/models.py:1899`; `app/chamados/signals.py:42`), que reconcilia pai e dispara S143 (`app/chamados/signals.py:92`). O resolver trava **todas** as disputas abertas do colaborador, incluindo outras ausentes da lista inicial (`app/chamados/services/disputa_emissao.py:1663`). Havendo outra disputa aberta, ela entra depois do Chamado e possivelmente depois da Pergunta já atualizada (`app/chamados/models.py:1884`). O modelo permite múltiplas disputas abertas: seu `Meta` só declara índice, sem unicidade (`app/chamados/models.py:1765`).

2. **`fechar_o_fio_do_dia`:** comentário promete pais, mas a lista contém somente chamados vivos do dia e destinos; pais das disputas não são acrescentados (`app/ponto/services/veredito_celula.py:179`; `app/ponto/services/veredito_celula.py:184`). Depois do UPDATE das perguntas (`app/ponto/services/veredito_celula.py:186`), `reconciliar()` pode alcançar S143 e acrescentar pais e outras disputas (`app/ponto/services/veredito_celula.py:199`; `app/chamados/services/disputa_emissao.py:1663`).

3. **Validação:** o ramo `FOLGA_CONTESTA` desvia antes da trava nova (`app/chamados/services/validacao.py:404`). Folga pronta carimba Pergunta antes de chamar o fechamento do fio (`app/chamados/services/validacao.py:284`; `app/chamados/services/validacao.py:292`); folga nova também carimba primeiro (`app/chamados/services/validacao.py:312`). No ramo normal, a lista só inclui pai e destino (`app/chamados/services/validacao.py:423`), mas o materializador reconcilia clones de `chamados_relacionados_ids`, ausentes dela, depois do carimbo da Pergunta (`app/chamados/services/validacao.py:425`; `app/chamados/services/materializacao.py:965`; `app/chamados/services/materializacao.py:766`). O materializador geral roda após o atomic local e varre irmãs, sem cobertura da lista antecipada (`app/chamados/services/validacao.py:459`; `app/chamados/services/materializacao.py:1360`).

4. **`fechar_beco`:** trava somente `d` e os chamados coletados (`app/chamados/services/becos.py:51`), depois chama resolver (`app/chamados/services/becos.py:53`), que trava todas as disputas abertas (`app/chamados/services/disputa_emissao.py:1663`). Outra disputa aberta entra depois de Chamado.

5. **Resolver:** via S143, a nova trava chega tarde. `reconciliar()` já trava Chamado (`app/chamados/models.py:628`; `app/chamados/models.py:688`), salva (`app/chamados/models.py:291`) e executa S143 (`app/chamados/signals.py:92`); só então o wrapper trava Disputa (`app/chamados/services/disputa_emissao.py:1665`). Há também janela concorrente: `_disp` é coletada antes da trava do colaborador, enquanto o corpo posteriormente consulta novamente todas as abertas (`app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1773`).

**Commit parcial / BUG120 — NÃO PROCEDE como regressão demonstrada.** S143 já envolve o resolver inteiro em savepoint próprio e captura a exceção fora dele; o atomic adicional não remove essa proteção (`app/chamados/signals.py:83`). `_retratar` e `fechar_beco` já envolviam o ato inteiro em atomic (`app/chamados/reconciliador.py:522`; `app/chamados/services/becos.py:41`). Existe chamador direto no fallback de `reconciliar_fantasmas`, mas dependência de sucesso parcial é [nao verificado] (`app/ponto/management/commands/reconciliar_fantasmas.py:280`).

H — **PROCEDENTE: cobertura insuficiente da ordem.** O instrumento memoriza tabelas, não linhas: qualquer segunda trava da mesma tabela desaparece (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`; `app/chamados/tests/test_e1_v5_ordem_trava.py:45`). Assim não percebe clone ou segunda disputa travados após Pergunta. O teste do resolver fecha o chamado por `.update()` fora do trecho observado, chama o resolver diretamente e exige apenas Disputa e Chamado; pode passar sem escrita de Pergunta e não exercita S143 (`app/chamados/tests/test_e1_v5_ordem_trava.py:331`; `app/chamados/tests/test_e1_v5_ordem_trava.py:335`). O mutante cobre apenas `admin_encerra` (`app/chamados/tests/test_e1_v5_ordem_trava.py:337`).

I — **PROCEDENTE: ordem de trava e retorno contra a lâmpada permanecem.** Os caminhos síncronos Chamado → Pergunta, Disputa → materialização e Chamado → cartório seguem dentro de `post_save`, sem `on_commit` (`app/chamados/signals.py:92`; `app/chamados/signals.py:140`; `app/chamados/signals.py:180`). Beco e retomada de regularizações usam `on_commit`, portanto o cumprimento é parcial (`app/chamados/signals.py:121`; `app/chamados/signals.py:202`).

## 07/11 12:18 · sessao 52e5ef2b6986 · FIO-E0-V4-E2 · verificador · origem=Code

**Há achados procedentes de horizonte incompleto e de relógio transitivo fora da borda. Não achei violação do teto no ramo novo `nao_venceu`, nem segundo escritor no caminho da calibração da âncora.** (`app/chamados/services/tarefa_do_dia.py:202`, `app/chamados/services/tarefa_do_dia.py:305`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/services/cartorio.py:858`, `app/escala/signals.py:161`)

A atribuição histórica exata de cada alteração aos três commits, os RED/GREEN originais, a suíte Django e a ocorrência dos contraexemplos em produção estão **[nao verificado]**. [nao verificado]

1. **Retratação posterior ainda muda a tarefa do passado — classe: horizonte incompleto; procedente.**

   O filtro de decisões corta `ato_em > agora`, mas `_tarefa_pura` descarta primeiro a pergunta pela **`via_resolucao` atual**. Portanto, cortar a decisão de retratação não basta para recuperar a pergunta anterior. (`app/chamados/services/tarefa_do_dia.py:301`, `app/chamados/services/tarefa_do_dia.py:202`)

   A porta `remover_motivo` registra a decisão de retratação e muda a via para motivo retratado. Se esse ato aconteceu depois do `agora` consultado, a via atual continua sendo lida: com marco vencido e apagado, a pergunta anterior desaparece da seleção e nasce `sem_pergunta/perguntar`. (`app/chamados/models.py:1989`, `app/chamados/models.py:1994`, `app/chamados/services/tarefa_do_dia.py:251`)

   Confirmei, executando os corpos extraídos em memória, a mudança de `aguarda_validacao/validar` para `sem_pergunta/perguntar`, mantendo a mesma sequência anterior a `agora` e trocando apenas a via atual. Esses resultados decorrem dos ramos citados; ocorrência produtiva **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:129`, `app/chamados/services/tarefa_do_dia.py:202`, `app/chamados/services/tarefa_do_dia.py:253`)

   Há ainda uma incompatibilidade com usar `materializada_em` como relógio da última alteração da via: `manter_carimbo=True` pode trocar a via preservando o instante antigo. Esse campo é o instante do **carimbo preservado**, não necessariamente da última mudança de identidade; `marcos_do_lote` agrega somente esse instante. (`app/chamados/models.py:2738`, `app/chamados/veredito_lavrado.py:176`)

2. **Passivo posterior atravessa o horizonte — classe: horizonte incompleto; procedente.**

   A carga chama `linhas_da_projecao_sem_linha(_ps)` sem `agora`. A autoridade lê os carimbos atuais `respondida_em/validada_em` e procura correspondência na sequência inteira, também sem corte temporal. (`app/chamados/services/tarefa_do_dia.py:305`, `app/chamados/models.py:3409`, `app/chamados/models.py:3412`)

   Contraexemplo: projeção respondida às 22h, sem linha correspondente, consulta às 21h. A função devolve passivo com `ato_em=22h`; `_pela_decisao` consulta esse passivo antes de verificar se havia decisão e responde `passivo_sem_linha/nao_sei`, em vez de `aguarda_resposta/responder`. Confirmei esses dois resultados nos corpos extraídos em memória. (`app/chamados/models.py:3416`, `app/chamados/models.py:3418`, `app/chamados/services/tarefa_do_dia.py:121`, `app/chamados/services/tarefa_do_dia.py:123`)

   Esse limite não está entre os três declarados no docstring do horizonte. (`app/chamados/services/tarefa_do_dia.py:273`)

3. **Ausência e absorção usam status atual — classe: horizonte incompleto; procedente.**

   A cobertura não corta o nascimento da ausência por `criado_em`, embora esse instante exista. Ela seleciona pelo período e pelo status atual — para cobrança, exclui somente `rejeitada`. Uma ausência criada depois de `agora` pode, assim, suspender a cobrança daquele instante passado. (`app/ponto/turnos.py:3072`, `app/ponto/turnos.py:2761`, `app/ponto/turnos.py:2519`, `app/ponto/models.py:813`)

   A decisão de absorção é cortada por `ato_em`, mas sua FK é carregada com o estado atual da ausência. `_pela_decisao` lê diretamente `ult.absorcao_ausencia.status`; uma aprovação, rejeição ou entrega de documento posterior pode mudar o dono da tarefa histórica. (`app/chamados/services/tarefa_do_dia.py:302`, `app/chamados/services/tarefa_do_dia.py:145`)

   O status efetivamente muda pela porta `mudar_estado`; a leitura da tarefa não usa `aprovada_em` nem reconstrói o status anterior. Não achei corte temporal nesses caminhos. (`app/ponto/services/ausencia.py:808`, `app/ponto/services/ausencia.py:824`, `app/ponto/models.py:810`, `app/chamados/services/tarefa_do_dia.py:233`)

4. **Fila, cadastro e identidade continuam atuais — classe: horizonte incompleto; procedente.**

   | Fato lido | O que ainda não é cortado por `agora` |
   |---|---|
   | **Fila** | `agora` entra somente na comparação com `silenciado_ate`. `status_local` continua atual, e não há corte por nascimento do chamado nessa seleção. Resolver ou silenciar posteriormente pode alterar a porta histórica. (`app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:188`, `app/chamados/services/tarefa_do_dia.py:321`) |
   | **Âncora da pergunta** | `chamado_que_lista` usa o destino/container atual; a tarefa não recupera o vínculo anterior da pergunta com o chamado. (`app/chamados/services/fio.py:116`, `app/chamados/services/tarefa_do_dia.py:320`) |
   | **Cadastro** | A carga lê o colaborador atual. A grade aplica admissão, demissão e isenção atuais; não recebe `agora`. Demissão retroativa ou alteração posterior de isenção podem mudar a resposta histórica. (`app/chamados/services/tarefa_do_dia.py:286`, `app/escala/services/leitor_celula.py:360`, `app/escala/services/leitor_celula.py:365`, `app/escala/services/leitor_celula.py:389`) |
   | **Identidade/data da pergunta** | A seleção usa `conversa` e `data_evento` atuais. Existe porta que altera `data_evento`; cortar decisões não recupera a identidade anterior. (`app/chamados/services/tarefa_do_dia.py:290`, `app/chamados/services/fio.py:859`, `app/chamados/services/fio.py:898`) |
   | **Vínculo e catálogo de ausência** | Os vínculos são carregados do cadastro atual; o conjunto de tipos em aberto vem do catálogo vigente, sem versão em `agora`. (`app/ponto/turnos.py:2990`, `app/ponto/turnos.py:2762`, `app/ponto/catalogo/ausencias.py:267`) |

   Portanto, “todo fato carregado é o que existia em `agora`” é mais amplo que o comportamento implementado; os limites excedem nascimento da pergunta, retratação de Batida e história da ata. (`app/chamados/services/tarefa_do_dia.py:273`)

5. **79315d64: relógio real escapa pelos helpers — classe: relógio fora da borda / selo incompleto; procedente.**

   O caminho encontrado é:

   `tarefa → cobertura → TIPOS_EM_ABERTO → _SetVivo → em_aberto_vigentes → vigente → time.monotonic()`.

   A tarefa chama a cobertura; o filtro e o corpo da cobertura usam o conjunto vivo; esse conjunto chega a `vigente()`, que lê `monotonic` e atualiza o cache. (`app/chamados/services/tarefa_do_dia.py:354`, `app/ponto/turnos.py:2762`, `app/ponto/turnos.py:2774`, `app/ponto/catalogo/ausencias.py:403`, `app/ponto/catalogo/ausencias.py:433`, `app/ponto/catalogo/ausencias.py:275`)

   É relógio para TTL do catálogo, não para julgar diretamente o vencimento do marco. Entretanto, o próprio selo inclui expressamente `time.monotonic` entre as leituras proibidas. (`app/ponto/catalogo/ausencias.py:276`, `app/chamados/tests/test_e2_tarefa_do_dia.py:890`)

   O detector AST examina apenas os módulos da tarefa e da lavra. O veneno dinâmico intercepta somente `django.utils.timezone.now`. Ambos deixam passar esse helper real. Confirmei também que `vigente()` lê `monotonic` mesmo com cache preenchido. (`app/chamados/tests/test_e2_tarefa_do_dia.py:958`, `app/chamados/tests/test_e2_tarefa_do_dia.py:680`, `app/ponto/catalogo/ausencias.py:275`)

   **“Não morde os mutantes declarados” não procede para o detector isolado:** as 16 formas declaradas foram identificadas na execução em memória. A falha é de alcance transitivo. Não achei `timezone.now/localdate` sem instante ou `date.today` no caminho efetivo da tarefa que rastreei; achei o `monotonic` acima. (`app/chamados/tests/test_e2_tarefa_do_dia.py:966`, `app/escala/services/leitor_celula.py:350`, `app/escala/utils.py:2256`, `app/escala/alimentacao.py:269`, `app/chamados/services/painel.py:190`)

6. **57acffd2: `nao_venceu` sem lâmpadas — classe: silenciamento indevido por teto; não procedente no ramo examinado.**

   A lei diz comparar **INSTANTE OU exigir FATO ENCERRADO**. Não exige os dois simultaneamente. O corpo do cartório compara `quando + teto <= agora`, preservando o instante do marco. (`CLAUDE.md:185`, `app/ponto/services/cartorio.py:858`)

   O ramo novo exige marcos **não vazios** e nenhum vencido. Se houver qualquer vencido, ou se os marcos forem `[]`/`None`, responde `ata_sem_lampadas/nao_sei`. Não achei marco vencido silenciado por esse ramo. (`app/chamados/services/tarefa_do_dia.py:187`, `app/chamados/services/tarefa_do_dia.py:193`)

   No cross-meia-noite, `marcos_turno` encadeia datetimes, avançando o dia quando necessário. A saída conserva o instante do dia seguinte; `data_civil` só é extraída depois. O vencimento é inclusivo na igualdade com o teto. (`app/escala/servico_jornada.py:61`, `app/escala/servico_jornada.py:81`, `app/ponto/services/cartorio.py:858`, `app/chamados/services/tarefa_do_dia.py:96`)

7. **d912345b: calibração da âncora — classe: segundo escritor fora da porta; não achei.**

   O signal reconhece `None → valor` e chama `DisputaSupervisao.remover_motivo`. A consequência **é escrita**: registra retratação, carimba identidade, remove motivo/contexto e, quando cabível, fecha pela porta do ciclo. (`app/escala/signals.py:143`, `app/escala/signals.py:161`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`, `app/chamados/models.py:1996`, `app/chamados/models.py:2005`)

   A sequência passa por `DecisaoPergunta.registrar`; o carimbo passa por `carimbar_identidade → _escrever_carimbo`. Não achei escrita paralela no signal. (`app/chamados/models.py:3428`, `app/chamados/models.py:2658`, `app/chamados/models.py:2738`)

   O trecho do diff da âncora altera a testemunha: exige uma pergunta conservada/retratada e uma decisão de retratação, substituindo a expectativa de exclusão. Esse teste não cria escritor de produção. (`app/escala/tests/test_signal_ancora.py:52`, `app/escala/tests/test_signal_ancora.py:59`, `app/escala/tests/test_signal_ancora.py:62`)

8. **Selos, fonte única e consultas — resultado por classe.**

   | Classe | Procedência |
   |---|---|
   | **Segundo juiz duplicando autoridade registrada** | **Não achei nos caminhos examinados.** Há corte e registro para `_C19`; a coexistência dos cinco respondedores até E3 está declarada. Teto e vínculo delegam às autoridades existentes. (`app/docs/CORTES.md:424`, `app/core/juizes.py:285`, `app/core/juizes.py:153`, `app/ponto/turnos.py:3052`, `app/ponto/turnos.py:3059`) |
   | **Universo de `batidas_apuraveis` rederivado** | **Não achei.** O efeito chama a porta e acrescenta origem/horizonte. As agregações de invalidação usam Batida crua porque precisam incluir retratações; não constituem o universo de apuração. (`app/chamados/services/tarefa_do_dia.py:316`, `app/ponto/turnos.py:2876`, `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:154`) |
   | **Selo do horizonte sem mordida sobre os dois filtros** | **Não procede por leitura contrafactual.** Sem cortar decisões, a resposta/validação futura contraria os resultados exigidos; sem cortar criação da Batida, o efeito futuro contraria `encerrada_pelo_juiz`. Execução desses testes com Django: **[nao verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:710`, `app/chamados/tests/test_e2_tarefa_do_dia.py:720`, `app/chamados/tests/test_e2_tarefa_do_dia.py:730`) |
   | **Selo T sem mordida sobre a cura nova** | **Não procede.** Remover o ramo em memória faz 08h10 voltar a `ata_sem_lampadas/nao_sei`, contrariando a asserção. U/C/L testam mecanismos distintos e não precisam falhar sem essa cura. (`app/chamados/tests/test_e2_tarefa_do_dia.py:793`, `app/chamados/tests/test_e2_tarefa_do_dia.py:773`, `app/chamados/tests/test_e2_tarefa_do_dia.py:797`, `app/chamados/tests/test_e2_tarefa_do_dia.py:815`) |
   | **Queries crescem com N** | **Não achei prova de crescimento nos caminhos inspecionados.** O teste inclui validações e compara lote maior com menor; não exercita ausência, absorção, âncora ou ata sem lâmpadas. Não certifica esses cenários. (`app/chamados/tests/test_e2_tarefa_do_dia.py:861`, `app/chamados/tests/test_e2_tarefa_do_dia.py:869`, `app/chamados/tests/test_e2_tarefa_do_dia.py:886`) |

   A leitura do conjunto vivo por ausência não prova uma query por ausência: `vigente()` tem cache TTL de 60 segundos. A contagem pode depender desse cache; não medi SQL nesta cópia. (`app/ponto/turnos.py:2774`, `app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:276`)

9. **Censo dos campos e significados usados pelo diff.**

   | Campo/valor | Quem escreve | Quem lê e significado |
   |---|---|---|
   | **`DecisaoPergunta.ato_em`** | Porta `registrar`; recebe resposta, reabertura/recusa, validação, devolução, absorção, silêncio, retratação e recuperação histórica. (`app/chamados/models.py:3428`, `app/chamados/services/disputa_emissao.py:1369`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/validacao.py:139`, `app/chamados/services/validacao.py:205`, `app/chamados/services/materializacao.py:560`, `app/chamados/models.py:1870`, `app/chamados/models.py:1990`, `app/chamados/models.py:3418`) | Ordenação da sequência, histórico da tela, correspondência do passivo e tarefa. É **instante do ato**, podendo ser recuperado de fato antigo; não é nascimento físico da linha. (`app/chamados/models.py:3374`, `app/chamados/models.py:2993`, `app/chamados/models.py:3413`, `app/chamados/services/tarefa_do_dia.py:301`, `app/chamados/services/disputa_emissao.py:1524`) |
   | **`Batida.criado_em`** | `auto_now_add`; inserção pela porta de registro. (`app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`) | Tarefa e invalidadores usam o nascimento da linha, com `<=agora/ate`; não usam o instante da marcação `timestamp`. (`app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/veredito_lavrado.py:151`) |
   | **`Batida.retratada_em`** | `retratar_batida → anotar`; desfazer retratação também passa por anotação. (`app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:252`, `app/ponto/services/retratar_humano.py:207`) | `batidas_apuraveis` lê participação **atual**, exigindo NULL; `marcos_do_lote` lê o instante da retratação até `ate`. São usos distintos e o limite histórico da porta está declarado. (`app/ponto/turnos.py:2876`, `app/chamados/veredito_lavrado.py:154`, `app/chamados/services/tarefa_do_dia.py:313`) |
   | **`PerguntaDisputa.veredito_lavrado`** | `lavrar` pergunta ao juiz do fechamento; backfill também calcula/atribui e persiste pela porta de lote. (`app/chamados/veredito_lavrado.py:236`, `app/chamados/veredito_lavrado.py:244`, `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/veredito_lavrado.py:224`) | Tarefa aceita somente `sim/nao` vigentes; `esta_stale`, varredura e auditoria verificam confiança na lavra. (`app/chamados/services/tarefa_do_dia.py:309`, `app/chamados/veredito_lavrado.py:196`, `app/chamados/services/raiox.py:40`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`) |
   | **`PerguntaDisputa.julgado_em`** | Mesmos escritores da lavra; instante em que foi lavrada. (`app/chamados/veredito_lavrado.py:245`, `app/chamados/management/commands/backfill_veredito_lavrado.py:139`) | Lavra futura é rejeitada; insumo invalida quando estritamente posterior, `marco > julgado_em`. A data usada na expiração é local. (`app/chamados/veredito_lavrado.py:196`, `app/chamados/veredito_lavrado.py:202`, `app/chamados/veredito_lavrado.py:206`) |
   | **`PerguntaDisputa.janela_expira_em`** | `expira_em(data_evento, hoje)` alimenta lavra/backfill. (`app/chamados/veredito_lavrado.py:95`, `app/chamados/veredito_lavrado.py:246`, `app/chamados/management/commands/backfill_veredito_lavrado.py:140`) | É **data de competência**, não instante nem dia do turno. Expira com `<= data local de agora`, quando a lavra é de data local anterior. (`app/chamados/models.py:2399`, `app/chamados/veredito_lavrado.py:198`, `app/chamados/veredito_lavrado.py:202`) |
   | **`PerguntaDisputa.via_resolucao/materializada_em`** | `_escrever_carimbo`, usado por `carimbar`, `carimbar_identidade` e descarimbo. (`app/chamados/models.py:2658`, `app/chamados/models.py:2664`, `app/chamados/models.py:2727`, `app/chamados/models.py:2748`) | Juiz lê via+carimbo; tarefa lê via atual; invalidadores leem o instante. **Não equivale necessariamente à última alteração da via**, conforme achado 1. (`app/chamados/juizes.py:648`, `app/chamados/services/tarefa_do_dia.py:202`, `app/chamados/veredito_lavrado.py:176`, `app/chamados/models.py:2738`) |
   | **`PerguntaDisputa.data_evento`** | Abertura, preenchimento na costura e porta `rechavear_data_evento`. (`app/chamados/models.py:2578`, `app/chamados/services/fio.py:838`, `app/chamados/services/fio.py:898`) | Tarefa e ajuste overnight assumem **dia do turno**; a data civil do marco resulta da cadeia de instantes. Não achei releitura como dia civil no código novo; migração efetiva do acervo **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:197`, `app/chamados/juizes.py:301`, `app/chamados/juizes.py:319`) |
   | **`CelulaDia.veredito_em`** | `lavrar_veredito`; não anda em toda operação sobre a ata. (`app/ponto/portas/celula.py:1032`, `app/ponto/portas/celula.py:997`) | `marcos_do_lote` agrega esse instante até `ate`; ele não é histórico completo da ata. (`app/chamados/veredito_lavrado.py:165`, `app/ponto/portas/celula.py:1001`) |
   | **`CelulaDia.ata`** | Lavra e operação de precedência dentro da porta da célula. (`app/ponto/portas/celula.py:1028`, `app/ponto/portas/celula.py:1071`) | O leitor único transporta fotografia atual, lâmpadas e insumos; a tarefa recebe essa grade. Não há reconstrução da ata em `agora`. (`app/escala/services/leitor_celula.py:379`, `app/escala/services/leitor_celula.py:413`, `app/chamados/services/tarefa_do_dia.py:331`) |
   | **`marcos_com_teto: tipo, instante, vencido`** | Corpo do cartório usa `marcos_turno` e calcula `instante+teto<=agora`. (`app/ponto/services/cartorio.py:849`, `app/ponto/services/cartorio.py:858`) | `marcos_vencidos` separa/ formata; carga delega; tarefa casa tipo+hora consumindo na ordem e lê vencimento/data civil. `tipo` é E/S, instante é datetime e vencimento é inclusivo. (`app/ponto/services/cartorio.py:843`, `app/ponto/turnos.py:3056`, `app/chamados/services/tarefa_do_dia.py:63`, `app/chamados/services/tarefa_do_dia.py:96`) |

   **Resposta direta sobre os dois cortes:** ambos usam comparação inclusiva de instantes, como `marcos_do_lote(ate=)`. Porém, `ato_em` é tempo do ato e `criado_em` é nascimento da Batida; não representam o mesmo fato. Além disso, `marcos_do_lote` **não lê `DecisaoPergunta.ato_em`**: agrega criação/retratação de Batida, carimbos da célula e materialização/resposta da projeção da pergunta. (`app/chamados/services/tarefa_do_dia.py:301`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:165`, `app/chamados/veredito_lavrado.py:176`, `app/chamados/veredito_lavrado.py:180`)

   A configuração usa `USE_TZ=True` e fuso padrão `America/Sao_Paulo`; a correção nova usa datas locais explicitamente. Fuso e consistência de cada registro produtivo **[nao verificado]**. (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `app/chamados/veredito_lavrado.py:198`)

Os “337 dias” e a correspondência das “27 classes” com linhas produtivas permanecem **[nao verificado]**: aparecem como descrição dos casos, sem as linhas da sonda no dado fornecido. (`app/chamados/tests/test_e2_tarefa_do_dia.py:7

[truncado em 20000 de 20055 caracteres]

## 07/11 12:18 · sessao a014a70d2d37 · - · - · origem=Ronald (tmux)

**F — PROCEDENTE para “em todo caminho”; NÃO PROCEDE no caminho CLI estável.**

- O CLI mede mérito/exportada e recusa antes de chamar `aplicar`: `app/chamados/management/commands/backfill_decisoes_pergunta.py:283-291`. O SILENCIO vem antes das retratações: `:139-151`.
- `aplicar(grupos, sem_ato)` direto escreve e conclui os atomics de SILENCIO antes de examinar mérito; depois apenas acrescenta `merito_recusado` e continua outros grupos: `:139-163`. O teste legitima chamada direta, mas passa `sem_ato=[]`, ocultando precisamente esse cenário: `app/chamados/tests/test_e1_v6_horizonte.py:196-203`.
- Uma perdedora `admin_decidiu` com resposta e sem validação entra em `admin_decidiu_sem_ato`: `backfill_decisoes_pergunta.py:90-95`; assim, chamada direta com a lista real pode gravar SILENCIO antes de recusar seu grupo: `:147-163`.
- A função `aplicar` não verifica competência exportada; a guarda existe só em `handle`: `:125-172`, `:238-243`, `:283-286`. Uso direto em produção e mudança concorrente entre medição e aplicação: **[nao verificado]**.
- O CLI não conserva uma transação que una preflight e aplicação; mérito é relido sob trava, exportada não: `:245-249`, `:283-291`, `:152-163`. Portanto, a garantia de recusa inteira não cobre alteração concorrente após o preflight: mesmos pontos; ocorrência **[nao verificado]**.

**H — PROCEDENTE: há lacunas nos selos e relógios soltos.**

- O rastreador registra somente a **primeira ocorrência por tabela** e ignora toda trava posterior daquela tabela: `app/chamados/tests/test_e1_v5_ordem_trava.py:38-47`. Logo, não detecta outro Chamado/Disputa adquirido depois da Pergunta se a tabela já apareceu antes: `:45-47`.
- O RED de `resolver_perguntas_por_chamado_resolvido` só exige Disputa e Chamado, sem exigir Pergunta, alteração persistida ou retorno: `:326-335`. O wrapper adquire essas tabelas antecipadamente: `app/chamados/services/disputa_emissao.py:1662-1667`; o corpo pode pular pergunta viva: `:1712-1713`. O teste admite passagem sem exercitar o fechamento anunciado: esses pontos.
- O mutante prova a remoção completa da subordem em um caso de `admin_encerra`, mas não cobre aquisição tardia de outra linha da mesma tabela: `test_e1_v5_ordem_trava.py:337-346`, combinado com `:38-47`.
- A declaração “Sem relógio solto: todo instante é literal” é falsa: `app/chamados/tests/test_e1_v6_horizonte.py:14`. A fixture cria disputa com `aberta_em=auto_now_add`: `:73-74`, `app/chamados/models.py:1709`; chama `fechar_por_admin` sem congelamento: teste `:174`, `:185`, `:201`, método lê `timezone.now()` em `models.py:2733`.
- Os testes do contador dependem da hora real de abertura e do `applied` real da migration sem fixar essa fronteira: `test_e1_v6_horizonte.py:210-224`, `app/chamados/services/fio.py:1097-1103`. O caso “2020 = acervo” também supõe que a 0052 foi aplicada depois de 2020: teste `:221-224`, comparação `fio.py:1101`.
- Os testes herdados de fábrica passam HOJE/AGORA literais ao leitor, mas a fixture lavra a célula sem `agora`, usando relógio real para `veredito_em`: `test_e1_v6_horizonte.py:262-294`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:65-77`, `app/ponto/portas/celula.py:1032`.
- O teste de condição inspeciona presença de folhas no `Q` do modelo; não comprova a estrutura booleana inteira nem executa a condição congelada da migration: `test_e1_v6_horizonte.py:47-60`. Os testes reais de banco abaixo/no horizonte exercitam MARCO: `:94-102`; BATIDA não recebe o mesmo teste de fronteira nesse arquivo: `:132-139`, `:226-232`. [nao verificado]

**H — NÃO PROCEDE quanto a vacuidade dos demais REDs examinados.**

- Horizonte, porta antiga, canônica e backfill têm testemunhas positivas e verificam identidade/contagens concretas: `test_e1_v6_horizonte.py:94-130`, `:146-161`.
- SILENCIO verifica a linha exata e a via duplicata; mérito verifica `SystemExit(4)` e preservação da via: `:171-194`. Esses testes não cobrem o preflight concorrente nem a lista `sem_ato` não vazia no helper direto: `:196-203`. [nao verificado]
- Emissor verifica mesma disputa, dois IDs distintos de batida e repetição sem terceira pergunta: `:245-259`. O caso sem origem não é exercitado nesse teste: `:250-259`. [nao verificado]
- Fábrica e marco perguntado têm controles positivos e negativos: `:272-294`.
- O RED do fio mudo exige primeira pergunta e, após retratação, segunda pergunta e saída `FIO MUDO: 1`: `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:169-175`; a classe congela o relógio: `:44`.
- A API passa a criar perguntas de motivos/instâncias diferentes e continua verificando materialização da irmã: `app/api/tests/test_falta_na_resposta.py:29-41`, `:62-67`. Não prova o índice do horizonte, porque usa `2026-05-28`: `:10`.

Execução dos testes, resultados e ocorrência em produção: **[nao verificado]**. [nao verificado]

## 07/11 12:14 · sessao f03c646da6ac · - · - · origem=Ronald (tmux)

- **Teto temporal — não procedente como defeito do ramo novo.** A lei permite comparar **instante OU exigir fato encerrado**, não exige ambos (`CLAUDE.md:185`). O teto compara `quando + 30 minutos <= agora`, inclusivo na igualdade (`app/ponto/services/cartorio.py:828`, `:858`). Sem lâmpadas, `nao_venceu` exige marcos **não vazios** e nenhum vencido; qualquer vencido, `[]` ou `None` resulta em `ata_sem_lampadas/nao_sei` (`app/chamados/services/tarefa_do_dia.py:187`, `:193`). Não achei silenciamento de marco vencido nesse ramo.

- **Cross-meia-noite — não achei corte por data.** `marcos_turno` constrói datetime aware e avança o dia enquanto o próximo instante não for posterior ao anterior (`app/escala/servico_jornada.py:61`, `:81`). `marcos_com_teto` preserva esse instante; o leitor transforma em data civil apenas para a saída (`app/ponto/services/cartorio.py:858`; `app/chamados/services/tarefa_do_dia.py:96`). Executei esses corpos extraídos por AST, em memória e com dependências simuladas: turno 21/10 22h–22/10 06h mantém a saída em 22/10; vence às 06h30 inclusive. Execução da suíte Django: **[nao verificado]**.

- **Selo T sem mordida — não procedente para a cura nova.** O caso exige `(instancias=[], nao_sei=False, razao='nao_venceu')` às 08h10 e `nao_sei` às 09h05 (`app/chamados/tests/test_e2_tarefa_do_dia.py:788`, `:793`, `:795`). Removi somente o ramo novo em AST na memória: o primeiro resultado virou `ata_sem_lampadas/True`, contrariando a asserção. As classes U/C/L não dependem dessa cura; exercitam outros mecanismos (`:773`, `:797`, `:815`). A atribuição das “337” linhas de produção ao ramo: **[nao verificado]**, pois só consta como prosa (`:756`).

- **Segundo juiz / universo paralelo de batidas — não achei nos trechos inspecionados.** O novo juiz tem corte explícito e registro `_C19`; a coexistência dos cinco respondedores até E3 é declarada (`app/docs/CORTES.md:424`; `app/core/juizes.py:150`, `:285`). Teto e vínculo delegam às autoridades existentes (`app/ponto/turnos.py:3052`, `:3059`); a leitura do efeito chama `batidas_apuraveis(None)` e acrescenta identificação/horizonte, preservando o predicado único de retratação (`app/chamados/services/tarefa_do_dia.py:316`; `app/ponto/turnos.py:2876`). Não confundir esse efeito com os marcos de invalidação, que legitimamente incluem batidas retratadas (`app/chamados/veredito_lavrado.py:151`, `:154`).

- **Queries crescem com N — não achei prova.** O selo compara 2 colaboradores × 3 dias × 3 perguntas com 1 × 1 × 1, incluindo decisões de validação (`app/chamados/tests/test_e2_tarefa_do_dia.py:861`, `:869`, `:879`, `:886`). Os caminhos novos fornecem células e escalas completas, carregam disputa e ausência por `select_related`, e usam IDs para a âncora (`app/chamados/services/tarefa_do_dia.py:293`, `:302`, `:331`; `app/chamados/services/fio.py:116`). Os helpers só consultam células/escalas quando a alimentação é `None` (`app/escala/utils.py:2179`, `:2189`; `app/escala/models.py:1117`, `:1182`). O selo **não exercita ausências, absorção, âncoras nem ata sem lâmpadas**: sua preparação contém lâmpadas e três validações, sem esses fatos (`app/chamados/tests/test_e2_tarefa_do_dia.py:864`, `:869`). Contagem efetiva da suíte: **[nao verificado]**.

- **Falsa pista N+1 / dependência real do relógio — procedente apenas a segunda.** `_cobertura_pura` consulta o conjunto vivo para cada ausência sem `data_fim` (`app/ponto/turnos.py:2774`); `_SetVivo.__contains__` chama o catálogo (`app/ponto/catalogo/ausencias.py:403`, `:430`). Entretanto, `vigente()` tem cache TTL de 60 segundos, portanto não há fundamento para afirmar uma query por ausência (`:257`, `:267`, `:276`). Há leitura transitiva real de `time.monotonic()` e dependência do cache (`:274`, `:275`), inclusive pelo filtro `_q_cobertura`, que consome o mesmo conjunto vivo (`app/ponto/turnos.py:2762`). A designação “corpo puro” da cobertura é inexata nessa dependência (`:2765`).

_2725 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
