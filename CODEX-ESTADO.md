# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 12:02** · publicado em **14/11/2026 12:02** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 12:02 · sessao eceeba4f7e5d · RMDIR-COM-GUARDA · verificador · origem=Code

**[bloqueia] A garantia irrestrita do rc ainda tem um contraexemplo: Bash em modo POSIX, arquivo do rc vazio e sinal tratado numa janela que força repetir um `wait` já concluído. O segundo `wait` pode devolver 127, que vira a saída da suíte.** O script não desliga esse modo; sua ativação no pouso é **[não verificado]**. (`bin/suite.sh:34`, `bin/suite.sh:358`, `bin/suite.sh:360`, `bin/suite.sh:364`)

1. **Caminhos do rc**

   **[fila] O caminho normal preserva o rc:** o subshell captura `_r`, tenta gravá-lo e executa `exit "$_r"`, mesmo se o `echo` falhar. Arquivo legível e não vazio prevalece; vazio ou ilegível usa `_RC_WAIT`. (`bin/suite.sh:341`, `bin/suite.sh:359`, `bin/suite.sh:360`, `bin/suite.sh:364`)

   | Sequência | Resultado |
   |---|---|
   | Sinal após instalar a trap, antes de lançar o subshell: fica enfileirado; quando houver filho, o polling tenta entregá-lo. | O sinal não determina mais o rc de reserva. (`bin/suite.sh:333`, `bin/suite.sh:341`, `bin/suite.sh:347`, `bin/suite.sh:360`) |
   | Sinal antes ou durante o polling; trava termina com 9; arquivo escrito. | Lê 9 do arquivo. A marca antiga é zerada antes da espera final. (`bin/suite.sh:341`, `bin/suite.sh:358`, `bin/suite.sh:359`) |
   | Mesma sequência, mas escrita impedida. | Usa o rc da espera final; desapareceu a fabricação de 143 pelo nome TERM. (`bin/suite.sh:358`, `bin/suite.sh:360`) |
   | Sinal realmente interrompe o `wait` final. | A trap marca `_NO_WAIT`; o laço chama `wait` novamente. Com arquivo válido, seu conteúdo ainda prevalece. (`bin/suite.sh:333`, `bin/suite.sh:358`, `bin/suite.sh:359`) |
   | **[bloqueia]** Arquivo vazio; primeiro `wait` completo devolve 9; sinal chega antes do teste da marca; Bash POSIX. | Repete uma espera já consumida, obtém 127 e devolve 127. Também há falso positivo se o sinal chegar entre o zeramento e a entrada no `wait`. Ocorrência no pouso **[não verificado]**. (`bin/suite.sh:358`, `bin/suite.sh:360`, `bin/suite.sh:364`) |

   **[fila] Há outra divergência condicional:** arquivo já contém 9, mas um sinal externo mata o próprio subshell entre o `echo` e seu `exit`; a suíte prefere o arquivo, embora o status terminal do subshell seja outro. Esse evento não aparece no dado: **[não verificado]**. (`bin/suite.sh:341`, `bin/suite.sh:359`, `bin/suite.sh:364`)

2. **Término, 127 e identidade do processo**

   - **[fila] Pode não terminar sob sinais incessantes:** basta que toda tentativa receba uma marca antes do teste. O laço não limita tentativas. Uma sequência finita, por si, não demonstra travamento permanente. (`bin/suite.sh:333`, `bin/suite.sh:334`, `bin/suite.sh:335`, `bin/suite.sh:358`)
   - **[bloqueia] Pode devolver 127 por espera repetida em Bash POSIX**, conforme a sequência acima. Em Bash padrão, recolher o filho pelo mecanismo interno do shell não significa automaticamente esquecer seu status; não há outro `wait "$_PID_RUN"` nesse trecho. Modo/versão de produção **[não verificado]**. (`bin/suite.sh:358`)
   - **[fila] PID reciclado ameaça principalmente o polling anterior:** arquivo vazio mais `kill -0` verdadeiro para o número reaproveitado pode prolongar a espera; com fila pendente, `pkill -P` pode atingir filhos desse novo processo. Arquivo não vazio rompe esse polling. (`bin/suite.sh:344`, `bin/suite.sh:347`)
   - **[fila] Reciclagem numérica não faz `wait` obter status de qualquer processo do sistema:** ele depende da tabela de filhos/status do Bash. Para devolver o rc de outro filho, seria necessária substituição da identidade guardada, por exemplo por um novo filho com o mesmo número; isso não está demonstrado pelo dado: **[não verificado]**. Os pontos envolvidos são o armazenamento do PID e sua reutilização nas consultas. (`bin/suite.sh:342`, `bin/suite.sh:344`, `bin/suite.sh:358`)

3. **Leitores e escritores**

   **[fila] O mapa atual é:** (`bin/suite.sh:329`, `bin/suite.sh:333`, `bin/suite.sh:347`, `bin/suite.sh:358`, `bin/suite.sh:359`, `bin/suite.sh:360`, `bin/suite.sh:364`)

   | Campo | Escritores | Leitores |
   |---|---|---|
   | `_NO_WAIT` | Inicialização `:329`; traps `:333–335`; zeramento `:358` | Teste em `:358` | [nao verificado]
   | `_FILA` | Inicialização `:329`; acréscimo nas traps `:333–335`; retirada da cabeça `:347` | Traps `:333–335`; teste e extração `:345–347` | [nao verificado]
   | `_RC_WAIT` | Captura de `$?` em `:358` | Reserva em `:360` |
   | `_RC_RUN` | Leitura do arquivo `:359`; reserva `:360` | Teste `:360`; aviso `:361`; saída `:364` | [nao verificado]

   **[fila] A busca em `bin/` não encontrou `_HOUVE` nem `_ULTIMO`.** A ausência é resultado da inspeção, sem uma linha individual para citar: **[não verificado por referência pontual]**. Os testes ainda leem e reescrevem textualmente âncoras de `_RC_RUN`, `_RC_WAIT` e `_FILA` para produzir mutantes. (`bin/tests/test_rm_com_guarda.sh:264`, `bin/tests/test_rm_com_guarda.sh:273`, `bin/tests/test_rm_com_guarda.sh:379`, `bin/tests/test_rm_com_guarda.sh:380`)

4. **Mordida do caso novo**

   - **[fila] Por leitura, morde dedc3e324 fora de root:** TERM executa `chmod 000` no arquivo temporário e encerra a trava com 9; o teste exige 9. O fallback antigo mostrado no delta escolheria 143 por TERM. Execução efetiva contra dedc3e324 **[não verificado]**. (`bin/tests/test_rm_com_guarda.sh:316`, `bin/tests/test_rm_com_guarda.sh:319`, `bin/tests/test_rm_com_guarda.sh:320`; delta fornecido)
   - **[fila] Não roda sob root:** o cenário inteiro está condicionado a UID diferente de zero; o sucesso geral do caso 6 não explicita essa omissão. (`bin/tests/test_rm_com_guarda.sh:314`, `bin/tests/test_rm_com_guarda.sh:322`, `bin/tests/test_rm_com_guarda.sh:386`)
   - **[fila] Há mutante que pode passar:** substituir o laço final por um único `wait`, mantendo o fallback novo. O cenário pode entregar TERM durante o polling e encontrar o run já terminado na espera final; `sleep 1` não sincroniza essa janela. Execução desse mutante **[não verificado]**. (`bin/tests/test_rm_com_guarda.sh:319`, `bin/suite.sh:344`, `bin/suite.sh:358`, `bin/suite.sh:360`)
   - **[fila] A ressalva de root alcança o selo anterior:** o caso 5 também usa `chmod 000`, sem excluir root; se a escrita continuar permitida, o mutante do fallback fixo em 1 não é exercido e pode provocar a própria reprovação do selo. Execução sob root **[não verificado]**. (`bin/tests/test_rm_com_guarda.sh:130`, `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:273`, `bin/tests/test_rm_com_guarda.sh:274`)

5. **Classes anteriores no delta**

   - **[fila] Segundo escritor, derivação paralela e ordem de trava:** não identifiquei reintrodução nas alterações fornecidas. Os alvos continuam vindo da porta, e as duas limpezas permanecem no comando executado pela trava. (`bin/suite.sh:112`, `bin/suite.sh:117`, `bin/suite.sh:293`, `bin/suite.sh:296`, `bin/suite.sh:317`, `bin/suite.sh:318`)
   - **[fila] Selo sem mordida:** o cenário novo distingue o fallback antigo fora de root, mas deixa sem cobertura garantida a interrupção da espera final e o cenário sob root. (`bin/tests/test_rm_com_guarda.sh:314`, `bin/tests/test_rm_com_guarda.sh:319`, `bin/suite.sh:358`)
   - **[bloqueia] Campo com significado ampliado:** `_NO_WAIT` registra “chegou sinal desde a marca”, mas seu leitor invalida também um `wait` que terminou corretamente. Essa diferença entre chegada e interrupção produz o contraexemplo POSIX; não é necessário PID reciclado. (`bin/suite.sh:331`, `bin/suite.sh:333`, `bin/suite.sh:358`, `bin/suite.sh:360`)

## 14/11 12:00 · sessao 62343981bf41 · - · - · origem=Ronald (tmux)

- **Fluxo normal:** não há divergência demonstrada. `_run` fornece `_r`; o subshell tenta gravá-lo e sai com o mesmo `_r`, mesmo quando o `echo` falha. Arquivo legível não vazio prevalece; vazio/ilegível aciona `_RC_WAIT`. `bin/suite.sh:341`, `bin/suite.sh:359`, `bin/suite.sh:360`, `bin/suite.sh:364`.

- **Sinal anterior ao laço ou recebido no polling:** fica em `_FILA`, segue ao filho e não define mais o rc de reserva. `_NO_WAIT` é zerado ao entrar na espera final; portanto um sinal já tratado não transforma rc 9 em 143. `bin/suite.sh:333`, `bin/suite.sh:344`, `bin/suite.sh:347`, `bin/suite.sh:358`.

- **Sinal durante a espera final:** a marca força outra chamada a `wait`; sinal depois do retorno de um `wait` completo, antes do teste da marca, também força repetição. Esse segundo caso é decisivo no Bash POSIX. `bin/suite.sh:333`, `bin/suite.sh:358`.

- **[bloqueia], condicional ao modo POSIX:** conforme a sonda informada pelo pai, nesse modo `wait "$p"` completo seguido de novo `wait "$p"` retorna respectivamente 9 e 127 [não verificado na cópia]. Sequência: arquivo do rc vazio; primeiro `wait` devolve 9; sinal chega antes do teste e marca `_NO_WAIT`; segundo `wait` devolve 127; fallback devolve 127. O script não desliga POSIX. `bin/suite.sh:34`, `bin/suite.sh:358`, `bin/suite.sh:360`. Ativação desse modo em produção: **[não verificado]**.

- **Bash padrão:** experimento local apenas em memória obteve `wait` repetido 9/9 e `wait` interrompido por TERM 143 seguido de espera 9 [não verificado na cópia]. Recolhimento do filho pelo shell, por si, não demonstra perda do status; não existe outro `wait` desse PID neste trecho. `bin/suite.sh:358`.

- **[fila], sinais incessantes:** se cada iteração receber sinal antes de testar a marca, o laço final pode continuar indefinidamente, mesmo com o filho terminado. Uma quantidade finita de sinais permite sair quando houver iteração sem marca, ressalvada a questão POSIX acima. `bin/suite.sh:333`, `bin/suite.sh:358`.

- **[fila], PID reciclado e arquivo vazio:** o polling pode continuar enquanto o número reaproveitado responder a `kill -0`; com fila pendente, pode tentar sinalizar filhos desse outro processo. Arquivo não vazio rompe esse polling. `bin/suite.sh:343`, `bin/suite.sh:344`, `bin/suite.sh:347`. O `wait` não passa a obter status arbitrário de um processo não filho apenas pelo reaproveitamento numérico [não verificado na cópia]. Substituição/perda do cache Bash por outro filho com o mesmo PID exige condições adicionais não demonstradas [não verificado].

- **Mapa:** `_FILA`: inicialização 329; leitura/escrita nas traps 333–335; leitura 345–347; remoção da cabeça 347. `_NO_WAIT`: inicialização 329; escrita nas traps 333–335; zeramento e leitura 358. `_RC_WAIT`: escrita 358, leitura 360. `_RC_RUN`: escrita 359/360, leitura 360/361/364. `bin/suite.sh:329`, `bin/suite.sh:333`, `bin/suite.sh:345`, `bin/suite.sh:347`, `bin/suite.sh:358`, `bin/suite.sh:359`, `bin/suite.sh:360`, `bin/suite.sh:361`, `bin/suite.sh:364`.

- Busca de `bin/` não encontrou `_HOUVE` nem `_ULTIMO`; os símbolos atuais aparecem nos lugares acima e nos leitores/escritores textuais dos mutantes. `bin/tests/test_rm_com_guarda.sh:264`, `bin/tests/test_rm_com_guarda.sh:265`, `bin/tests/test_rm_com_guarda.sh:272`, `bin/tests/test_rm_com_guarda.sh:273`, `bin/tests/test_rm_com_guarda.sh:379`, `bin/tests/test_rm_com_guarda.sh:380`.

- Divergência entre arquivo já escrito e status do subshell também seria possível se um agente externo matasse o subshell entre o `echo` e `exit`, ou adulterasse o arquivo; esses eventos não constam dos dados. **[fila] [não verificado]**, pontos envolvidos: `bin/suite.sh:341`, `bin/suite.sh:359`, `bin/suite.sh:364`.

## 14/11 11:59 · sessao 4539784bb97f · - · - · origem=Ronald (tmux)

- **[fila] O caso novo morde o defeito anterior fora de root:** TERM faz a trava executar `chmod 000 .../tmp.*` e sair 9; a asserção exige 9. O subshell preserva esse 9 apesar da falha do `echo`. No delta fornecido, dedc3e324 escolheria 143 pelo nome TERM quando o arquivo estivesse vazio. Referências: `bin/tests/test_rm_com_guarda.sh:314`, `:316`, `:319`, `:320`; `bin/suite.sh:341`; comportamento da versão anterior deduzido do anexo, execução **[não verificado]**.

- **[fila] O caso novo não roda sob root:** todo o cenário está dentro de `if [ "$(id -u)" != 0 ]`. Não há aviso específico de cenário pulado antes do texto de sucesso do caso 6. Referências: `bin/tests/test_rm_com_guarda.sh:314`, `:322`, `:386`.

- **[fila] Não força sinal durante o `wait` final:** usa somente `sleep 1; kill -TERM`, sem marco que confirme a entrada em `bin/suite.sh:358`. Um mutante que substitua o laço de reespera por um único `wait`, mantendo o fallback novo, pode passar quando TERM for tratado no laço de polling e o run terminar antes da espera final. Referências: `bin/tests/test_rm_com_guarda.sh:318`, `:319`; `bin/suite.sh:344`, `:352`, `:358`, `:360`. Execução do mutante **[não verificado]**.

- **[fila] Não há mutante novo que restaure o fallback por nome do sinal:** o acréscimo é apenas cenário funcional; os mutantes existentes do caso 6 cobrem caminho físico, erro de rmdir, limpeza após sinal, nome repassado e sobrescrita da fila. Referências: `bin/tests/test_rm_com_guarda.sh:312`, `:345`, `:353`, `:362`, `:370`, `:378`.

- **[fila] Ressalva já existente para root no selo inteiro:** o caso 5 tenta impedir escrita com chmod, mas não pula root; seu mutante troca somente o fallback por 1. Se root escrever normalmente, esse fallback não é exercido e o mutante pode passar, provocando `_falha`. Referências: `bin/tests/test_rm_com_guarda.sh:130`, `:231`, `:233`, `:271`, `:273`, `:274`. Execução sob root **[não verificado]**.

- **[fila] Classes do delta:** nenhuma segunda origem de limpeza, derivação paralela de alvos ou mudança na ordem da trava aparece nas alterações fornecidas; o código continua derivando alvos da porta e executando `_run` pela trava. Referências: `bin/suite.sh:293`, `:296`, `:317`, `:318`; alcance exato do histórico além do delta fornecido **[não verificado]**. A limitação concreta do selo é a cobertura omitida sob root e a ausência de sincronização com a espera final, acima.

## 14/11 11:49 · sessao 7df6ebc9fb89 · FRAC5 · sonda · origem=Code

Não reexecutei: não localizei o anexo `sonda.py` na cópia. [nao verificado]

Transcrição da saída fornecida: [nao verificado]

```text
universo_agendado_nao_iniciado 5
recusados_total 0
fracao_menor_que_5_dias (literal dias<5; o juiz do main ainda nao tem a 2a metade) 0 []
ERROS 0
```

Não há linhas `RECUSA`, contagens por recusa ou PKs no trecho fornecido. [nao verificado]

Não há `Traceback` no trecho fornecido; sua ocorrência fora desse trecho é [nao verificado].

## 14/11 11:49 · sessao 87a073847532 · RMDIR-COM-GUARDA · verificador · origem=Code

**[bloqueia] A condição (1), lida como “sempre devolver o rc do run”, não está integralmente cumprida:** sem conteúdo em `_RCF` e havendo sinal registrado, o código ignora `_RC_WAIT` e fabrica o retorno pelo último sinal. A correção específica do rc do `echo` está presente. — [bin/suite.sh:341](/home/ronald/codex_ws/dedc3e324465be37cb9faa2e2f17ac29fcbc4c1e/bin/suite.sh:341), `bin/suite.sh:354`, `bin/suite.sh:358`.

Esta é uma verificação estática. Execução dos REDs, resultados de suite/opus e efeitos em produção: **[nao verificado]**. [nao verificado]

1. **Condição (1): correção implementada; garantia absoluta incompleta.** O subshell captura `_r` imediatamente após `_run` e termina com `exit "$_r"`; falha do `echo` deixa de determinar seu status. Sem sinal e sem arquivo legível, o pai usa `_RC_WAIT`. — `bin/suite.sh:341`, `bin/suite.sh:354`, `bin/suite.sh:357`.

   O RED faz o docker falso aplicar `chmod 000` ao arquivo e exige retorno 75; o mutante substitui o fallback por 1. Pelo delta fornecido, o caso falharia no `0fae8f7f8`: falhando a escrita, o subshell antigo terminaria com o status do `echo`. Essa comparação é estrutural; execução contra o hash anterior **[nao verificado]**. — [bin/tests/test_rm_com_guarda.sh:130](/home/ronald/codex_ws/dedc3e324465be37cb9faa2e2f17ac29fcbc4c1e/bin/tests/test_rm_com_guarda.sh:130), `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:235`, `bin/tests/test_rm_com_guarda.sh:273`.

   **[bloqueia] Falta o cruzamento sinal + arquivo indisponível:** se `_RC_WAIT` for 9, `_RCF` vier vazio e `_ULTIMO=TERM`, a suite devolve 143. Os testes atuais exercitam retorno 9 após TERM e arquivo indisponível separadamente; não cobrem essa combinação. — `bin/suite.sh:355`, `bin/suite.sh:358`, `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:305`, `bin/tests/test_rm_com_guarda.sh:309`.

2. **Condição (2): cumpre para os seis alvos entregues pela porta.** A limpeza percorre os argumentos derivados da montagem; a porta escreve staticfiles, três caches, logs e media. O RED cria os seis diretórios vazios, conta os ausentes e exige exatamente 6. — `bin/suite.sh:60`, `bin/suite.sh:296`, [bin/arvore_do_push.sh:55](/home/ronald/codex_ws/dedc3e324465be37cb9faa2e2f17ac29fcbc4c1e/bin/arvore_do_push.sh:55), `bin/arvore_do_push.sh:80`, `bin/arvore_do_push.sh:81`, `bin/tests/test_rm_com_guarda.sh:153`, `bin/tests/test_rm_com_guarda.sh:157`.

   **O RED tem mordida específica:** o mutante restaura a lista antiga de três. Pelo delta, no `0fae8f7f8` sobrariam os três caches e a contagem seria 3. Execução histórica **[nao verificado]**. — `bin/tests/test_rm_com_guarda.sh:211`, `bin/tests/test_rm_com_guarda.sh:213`, `bin/tests/test_rm_com_guarda.sh:214`.

   **[fila] “Sempre seis” exige qualificação:** staticfiles só é emitido quando existe na raiz da própria porta; sem esse diretório, ela entrega cinco. A fixture garante staticfiles preenchido, portanto testa efetivamente seis, mas não prova uma saída invariável da porta. — `bin/arvore_do_push.sh:50`, `bin/arvore_do_push.sh:55`, `bin/tests/test_rm_com_guarda.sh:114`, `bin/tests/test_rm_com_guarda.sh:115`.

3. **Condição (3): cumpre no caminho normal de dois `--dir`.** Fora da trava há apenas `--so-confere`, que retorna antes do laço de remoção. `_run` chama a trava, que adquire o flock antes de executar `--na-vez`; ambas as limpezas ficam nesse comando. — `bin/suite.sh:58`, `bin/suite.sh:289`, `bin/suite.sh:317`, `bin/suite.sh:318`, `bin/suite.sh:112`, `bin/suite.sh:117`, [bin/trava_teste.sh:35](/home/ronald/codex_ws/dedc3e324465be37cb9faa2e2f17ac29fcbc4c1e/bin/trava_teste.sh:35), `bin/trava_teste.sh:42`.

   **O RED tem mordida específica:** a trava falsa usa flock; o primeiro docker falso mantém `logs`, o segundo `--dir` é lançado durante essa espera e o teste acusa seu desaparecimento. O mutante recoloca a limpeza fora da trava. Pelo delta, a limpeza prévia do `0fae8f7f8` também removeria esse diretório durante a sobreposição. Execução histórica **[nao verificado]**. — `bin/tests/test_rm_com_guarda.sh:120`, `bin/tests/test_rm_com_guarda.sh:132`, `bin/tests/test_rm_com_guarda.sh:245`, `bin/tests/test_rm_com_guarda.sh:250`, `bin/tests/test_rm_com_guarda.sh:281`.

   **[fila] O RED comprova preservação do diretório com docker falso; bind real permanece [nao verificado].** A sobreposição depende de quatro segundos e não há confirmação de que o segundo processo alcançou o ponto relevante dentro dessa janela. — `bin/tests/test_rm_com_guarda.sh:132`, `bin/tests/test_rm_com_guarda.sh:245`, `bin/tests/test_rm_com_guarda.sh:248`.

**a) Quem escreve e quem lê**

| Objeto | Escritor | Leitor |
|---|---|---|
| Nomes da montagem | `arvore_do_push.sh --montagem`, nos `printf` | A suite recebe `_m` e tokeniza `MONTAGEM`. — `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:80`, `bin/arvore_do_push.sh:81`, `bin/suite.sh:293`, `bin/suite.sh:295`. |
| `_alvos_da_montagem` | Extrai destinos de `-v` e `--tmpfs`; escreve sua saída | `ALVOS` e `_alvos_da_porta`. — `bin/suite.sh:81`, `bin/suite.sh:88`, `bin/suite.sh:92`, `bin/suite.sh:296`. |
| `_alvos_da_porta` | Chama a porta e reutiliza a mesma extração | Ramo `--limpa-montagens`, por `_al`/`_AL`. — `bin/suite.sh:90`, `bin/suite.sh:98`. |
| `ALVOS` | Inicializado vazio; preenchido pela extração da montagem | `_run` transporta; `--na-vez` divide em `_NV_AL`; pré/pós consomem esse array. — `bin/suite.sh:283`, `bin/suite.sh:296`, `bin/suite.sh:318`, `bin/suite.sh:109`, `bin/suite.sh:112`, `bin/suite.sh:117`. |
| `_RCF` | Pai cria o caminho; subshell escreve o conteúdo | Pai consulta tamanho, lê e remove. — `bin/suite.sh:338`, `bin/suite.sh:341`, `bin/suite.sh:344`, `bin/suite.sh:355`. |
| `_NV_RC` | Uma atribuição imediatamente após `"$@"` | Decide pós-limpeza e fornece o retorno. — `bin/suite.sh:115`, `bin/suite.sh:116`, `bin/suite.sh:120`. |

Não identifiquei segundo escritor **da lista usada pela limpeza normal**: ela recebe argumentos derivados da montagem. Os nomes repetidos no teste constituem o oráculo e o mutante, não outra lista operacional. — `bin/suite.sh:60`, `bin/suite.sh:296`, `bin/tests/test_rm_com_guarda.sh:153`, `bin/tests/test_rm_com_guarda.sh:213`.

**[fila] A exclusividade não é imposta à interface:** uma invocação direta de `--na-vez` fornece sua própria lista, sem consultar a porta. — `bin/suite.sh:108`, `bin/suite.sh:109`.

**b) Entradas de `--na-vez`**

| Entrada | Chega a `rmdir`? |
|---|---|
| Caminho absoluto ou fora de `app/` | Recusado pelo primeiro `case`. — `bin/suite.sh:61`. |
| Barra adicional, como `app/a/b` | Recusado pelo segundo `case`. — `bin/suite.sh:62`. |
| Qualquer `..` | Recusado, inclusive dentro do nome. — `bin/suite.sh:62`. |
| `app/` | Recusado por faltar o primeiro caractere do nome. — `bin/suite.sh:61`. |
| Lista vazia | Nenhum alvo é percorrido; o comando continua. — `bin/suite.sh:109`, `bin/suite.sh:115`. |
| Symlink final | Ignorado por `! -L`. — `bin/suite.sh:67`. |
| Symlink intermediário que saia da cópia | Barrado pela comparação física. — `bin/suite.sh:70`, `bin/suite.sh:71`. |

**[fila] Nome com espaço perde identidade:** `"app/foo bar"` vira `app/foo` e `bar`; se `app/foo` for diretório físico vazio, pode ser removido. `bar` é recusado com rc 1, que não impede o comando. Além disso, `app/[A-Za-z0-9._-]*` restringe apenas o primeiro caractere após a barra; o `*` não repete a classe. — `bin/suite.sh:109`, `bin/suite.sh:61`, `bin/suite.sh:73`, `bin/suite.sh:113`, `bin/suite.sh:115`.

**Rc 2 da limpeza anterior impede, sim, o comando.** Isso cobre cópia inválida e árvore viva; alvo malformado recebe rc 1, não 2. — `bin/suite.sh:53`, `bin/suite.sh:56`, `bin/suite.sh:61`, `bin/suite.sh:112`, `bin/suite.sh:113`.

**c) Outros caminhos de rc**

- **[fila] Trava ocupada:** retorna 75 antes de executar `--na-vez` ou docker. É “não executou”, não rc de um run realizado. — `bin/trava_teste.sh:36`, `bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`.
- **[fila] Docker ausente:** se alcançar a invocação, o status de comando ausente é 127 e será preservado. Pode haver recusa anterior com 1 em `teste_envfile`, que também pode tentar docker. Disponibilidade real **[nao verificado]**. — `bin/suite.sh:319`, `bin/suite.sh:115`, `bin/suite.sh:120`, `bin/suite.sh:276`, `bin/recursos.sh:122`.
- **[fila] Sinal na cadeia real:** o repasse alcança a trava, filha direta do subshell; ela não implementa repasse explícito aos descendentes. Os REDs usam travas falsas com handlers próprios. Portanto, rc da trava e rc do docker não são demonstrados como equivalentes sob sinal. — `bin/suite.sh:347`, `bin/trava_teste.sh:41`, `bin/trava_teste.sh:42`, `bin/tests/test_rm_com_guarda.sh:305`, `bin/tests/test_rm_com_guarda.sh:314`.

**d) Classes de reprovação**

- **Segundo escritor / derivação paralela:** não demonstrados no caminho normal; os dois consumidores reutilizam `_alvos_da_montagem`. A lista livre de `--na-vez` permanece **[fila]**. — `bin/suite.sh:92`, `bin/suite.sh:296`, `bin/suite.sh:109`.
- **[fila] Selo dependente de permissão:** o RED de `_RCF` não confirma que `chmod 000` impediu o acesso nem exclui root. Se a injeção não funcionar, o mutante sobrevive e o próprio selo acusa falta de mordida; não é verde silencioso. — `bin/tests/test_rm_com_guarda.sh:130`, `bin/tests/test_rm_com_guarda.sh:235`, `bin/tests/test_rm_com_guarda.sh:274`.
- **[fila] Ordem de trava nas portas avulsas:** `--limpa-montagens` limpa diretamente; `--na-vez` também não verifica posse da trava. Isso não invalida a ordem comprovada para dois `--dir`, mas impede afirmar “toda limpeza é travada”. — `bin/suite.sh:94`, `bin/suite.sh:99`, `bin/suite.sh:108`, `bin/suite.sh:112`.
- **[fila] Campo com dois significados:** `ALVOS` representa uma coleção, mas é transportado como texto separado por espaços, permitindo a perda de identidade descrita acima. — `bin/suite.sh:296`, `bin/suite.sh:109`.

A identificação dos **quatro `[fila]` originais**, seu registro em linhas de fila e a conclusão posterior de suite + um opus permanecem **[nao verificado]**. [nao verificado]

## 14/11 11:47 · sessao 57ec1f874399 · - · - · origem=Ronald (tmux)

- `--na-vez` recusa barra adicional e qualquer `..` antes de `rmdir`; `app/` também não passa. Lista vazia produz zero alvos e permite o comando. Referências: `bin/suite.sh:61`, `bin/suite.sh:62`, `bin/suite.sh:109`, `bin/suite.sh:115`.
- [fila] Nome com espaço é ambíguo: `app/foo bar` vira dois tokens; pode remover `app/foo`, rejeitar `bar` com rc 1 e ainda executar o comando. O padrão `app/[A-Za-z0-9._-]*` restringe somente o primeiro caractere do nome; o `*` seguinte admite outros caracteres. Referências: `bin/suite.sh:61`, `bin/suite.sh:73`, `bin/suite.sh:109`, `bin/suite.sh:112`, `bin/suite.sh:113`, `bin/suite.sh:115`.
- Symlink final é ignorado; symlink intermediário que saia da cópia falha na comparação física. Recusa rc 2 da limpeza anterior impede o comando. Referências: `bin/suite.sh:67`, `bin/suite.sh:70`, `bin/suite.sh:71`, `bin/suite.sh:112`, `bin/suite.sh:113`, `bin/suite.sh:115`.
- As duas limpezas do caminho normal `--dir` estão dentro da trava: esta prende fd 9 antes de executar `--na-vez`, que limpa antes/depois do comando. Referências: `bin/trava_teste.sh:35`, `bin/trava_teste.sh:36`, `bin/trava_teste.sh:42`, `bin/suite.sh:317`, `bin/suite.sh:318`, `bin/suite.sh:112`, `bin/suite.sh:117`.
- [fila] Isso não vale para toda entrada pública: `--limpa-montagens` chama a limpeza diretamente; `--na-vez` também não verifica posse da trava. Referências: `bin/suite.sh:94`, `bin/suite.sh:99`, `bin/suite.sh:108`, `bin/suite.sh:112`.
- O caminho normal preserva o rc do comando, inclusive se o `echo` falhar; a mudança relevante é `exit "$_r"` no subshell. Referências: `bin/suite.sh:115`, `bin/suite.sh:120`, `bin/trava_teste.sh:42`, `bin/suite.sh:341`, `bin/suite.sh:354`, `bin/suite.sh:357`.
- [fila] Há exceção explícita a “sempre rc do run”: sem conteúdo em `_RCF` e havendo sinal, o código ignora `_RC_WAIT` e devolve 143/130/129 pelo último sinal, independentemente do rc obtido. Os REDs de sinal e arquivo indisponível são separados. Referências: `bin/suite.sh:355`, `bin/suite.sh:356`, `bin/suite.sh:357`, `bin/suite.sh:358`, `bin/tests/test_rm_com_guarda.sh:233`, `bin/tests/test_rm_com_guarda.sh:305`, `bin/tests/test_rm_com_guarda.sh:317`.
- rc 75 pode vir da trava sem executar o comando: `bin/trava_teste.sh:35`, `bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`. Docker ausente no ponto da invocação resulta em status de comando ausente, preservado pelo encadeamento; a existência/disponibilidade real do Docker é [nao verificado]. Referências: `bin/suite.sh:319`, `bin/suite.sh:115`, `bin/suite.sh:120`, `bin/suite.sh:341`.
- Sinais são enviados ao filho direto do subshell, a trava; a trava real não implementa repasse explícito aos descendentes. Os REDs verificam travas falsas que recebem TERM/HUP e escolhem seus próprios rc. Referências: `bin/suite.sh:347`, `bin/trava_teste.sh:35`, `bin/trava_teste.sh:41`, `bin/trava_teste.sh:42`, `bin/tests/test_rm_com_guarda.sh:305`, `bin/tests/test_rm_com_guarda.sh:314`.
- `rc >= 128` evita a limpeza posterior, mas a chamada seguinte volta a fazer a limpeza anterior; sobrevivência efetiva de container/bind após sinal é [nao verificado]. Referências: `bin/suite.sh:112`, `bin/suite.sh:116`, `bin/suite.sh:117`.

## 14/11 11:46 · sessao 558dff52491e · - · - · origem=Ronald (tmux)

- **Escritor da montagem:** os nomes saem de `bin/arvore_do_push.sh:55` e `bin/arvore_do_push.sh:80-81`; o staticfiles é condicional à existência do diretório da própria porta (`bin/arvore_do_push.sh:50`). Portanto, “sempre seis” exige essa condição; sem ela são cinco (`bin/arvore_do_push.sh:50-56`, `bin/arvore_do_push.sh:80-82`).

- **Leitura/derivação na suite:** `_m` recebe a porta (`bin/suite.sh:293`), `MONTAGEM` tokeniza (`bin/suite.sh:295`), `_alvos_da_montagem` extrai destinos de `-v`/`--tmpfs` (`bin/suite.sh:81-88`) e escreve o valor derivado de `ALVOS` (`bin/suite.sh:296`), inicialmente vazio (`bin/suite.sh:283`). `_run` o transporta (`bin/suite.sh:318`); `--na-vez` o divide em `_NV_AL` e ambas as limpezas o leem (`bin/suite.sh:109-117`). `_alvos_da_porta` chama a mesma extração para a porta avulsa; não há implementação paralela desse cálculo nesses dois caminhos (`bin/suite.sh:90-99`).

- **Segundo escritor:** nenhuma lista de nomes de limpeza está repetida no caminho normal da suite: a função itera os argumentos (`bin/suite.sh:60`), que vêm da montagem (`bin/suite.sh:296`). Os seis nomes explícitos do selo são o oráculo independente e o mutante de três nomes (`bin/tests/test_rm_com_guarda.sh:153-156`, `bin/tests/test_rm_com_guarda.sh:210-217`). Há listas de logs/media/staticfiles fora da porta, por exemplo a sonda de montagem `:ro`, que declara outro escopo (`bin/sonda_leitura.sh:197-210`); chamá-las de segundo escritor da limpeza da suite não se sustenta nessas linhas.

- **[fila] Interface aceita outro produtor:** `--na-vez` aceita diretamente a string de alvos fornecida pelo chamador, sem consultar a porta nem adquirir trava (`bin/suite.sh:108-120`). O chamador normal a põe sob `trava_teste.sh` (`bin/suite.sh:317-318`), mas exclusividade da fonte para todas as invocações diretas é **[nao verificado]**.

- **Arquivo de rc:** o pai cria e guarda o caminho em `_RCF` (`bin/suite.sh:338`); o subshell escreve o conteúdo pelo único `echo` (`bin/suite.sh:341`); o pai lê tamanho no laço (`bin/suite.sh:344`), lê conteúdo com `cat` e remove (`bin/suite.sh:355`). Em seguida escolhe a reserva somente se o conteúdo vier vazio (`bin/suite.sh:356-359`). `_NV_RC` recebe uma vez o status de `"$@"`, serve de guarda da limpeza posterior e é devolvido (`bin/suite.sh:115-120`).

- **[fila] Campo delimitado por espaço:** `ALVOS` é serializado com espaços (`bin/suite.sh:296`) e desserializado com `read -a` (`bin/suite.sh:109`); a própria montagem também é texto tokenizado por espaços (`bin/suite.sh:295`, `bin/suite.sh:92`). Um nome com espaço perde sua identidade nesses passos; isso não constitui saída de diretório, mas impede tratar a lista como uma coleção geral de nomes (`bin/suite.sh:61-70`).

- A existência e o conteúdo das quatro linhas de fila exigidas pelo aval permanecem **[nao verificado]**; não há referência delas no dado fornecido, além da exigência de registrá-las **[nao verificado]**.

## 14/11 11:46 · sessao 3ab108696133 · - · - · origem=Ronald (tmux)

- Condição (1): o subshell agora termina com `_r`, preservando rc do `_run` mesmo se `echo` falhar; sem sinal, o fallback usa `_RC_WAIT` (`bin/suite.sh:341`, `bin/suite.sh:354-358`). O RED injeta `chmod 000` e exige rc 75 (`bin/tests/test_rm_com_guarda.sh:130`, `bin/tests/test_rm_com_guarda.sh:233-235`); o mutante força fallback 1 (`bin/tests/test_rm_com_guarda.sh:271-275`). No código anterior mostrado pelo delta, falha de escrita deixaria o subshell com rc do `echo`, portanto esse caso falharia com 1 em vez de 75 [nao verificado por execução].

- **[fila] Mordida do RC depende de permissão:** esse caso não confirma que a escrita/leitura de `_RCF` efetivamente falhou e não exclui root (`bin/tests/test_rm_com_guarda.sh:130`, `bin/tests/test_rm_com_guarda.sh:233-235`). Para outro teste baseado em chmod, o selo explicitamente exclui root (`bin/tests/test_rm_com_guarda.sh:299`, `bin/tests/test_rm_com_guarda.sh:345`). Se chmod não bloquear, o mutante de fallback sobreviverá, e o próprio selo ficará vermelho pela falta de mordida (`bin/tests/test_rm_com_guarda.sh:274`).

- Condição (2): há RED comportamental que cria seis diretórios, conta seis ausentes e exige exatamente 6 (`bin/tests/test_rm_com_guarda.sh:153-157`). O mutante troca a derivação pela lista antiga de três (`bin/tests/test_rm_com_guarda.sh:211-215`). Na versão anterior do delta, os três caches ficariam; a contagem seria 3 [nao verificado por execução].

- Condição (3): o caminho normal chama `trava_teste.sh` e só então `--na-vez`; pre/post estão nesse modo (`bin/suite.sh:317-318`, `bin/suite.sh:112-118`); a trava real adquire flock antes de executar o comando (`bin/trava_teste.sh:35-42`). O RED usa flock real numa trava falsa, segura o primeiro run com `logs` e lança segundo `--dir`, exigindo ausência de `montagem.arrancada` (`bin/tests/test_rm_com_guarda.sh:120`, `bin/tests/test_rm_com_guarda.sh:131-133`, `bin/tests/test_rm_com_guarda.sh:245-250`). O mutante recoloca limpeza fora da trava (`bin/tests/test_rm_com_guarda.sh:279-283`); a limpeza anterior do delta também removeria `logs` durante essa janela [nao verificado por execução].

- **[fila] Concorrência tem janela temporal:** o primeiro run dorme quatro segundos, e o teste não confirma que o segundo chegou à limpeza antes de essa janela acabar (`bin/tests/test_rm_com_guarda.sh:132`, `bin/tests/test_rm_com_guarda.sh:245-250`). O teste detecta o mutante sob sobreposição; a sobreposição numa execução concreta está [nao verificado].

- **[fila] Guarda da viva pode não ser exercida:** o caso e seu mutante dependem da existência da viva e de nenhum dos seis alvos estar vazio; caso contrário imprimem aviso e seguem (`bin/tests/test_rm_com_guarda.sh:170-185`, `bin/tests/test_rm_com_guarda.sh:192-197`). Isso pode deixar o mutante sem mordida; o próprio selo o acusa como falha (`bin/tests/test_rm_com_guarda.sh:196`).

## 14/11 11:42 · sessao 758c610315a2 · JANELA-NAO-SEGURA-POUSO · verificador · origem=Code

1. **[fila] A faixa está correta para todos os dias: 10.080 pares dia/minuto conferidos em memória, sem divergência.** `proibido()` retorna **0 quando barra**; `--janela` retorna **1 quando barra**. ([bin/janela_auth.sh:55](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:55), [modo horário:75](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:75))

   | Dia | HHMM | Guarda | `proxima()` |
   |---|---|---|---|
   | Segunda a domingo | `0000`–`0559` | PROIBIDA | hoje às 06:00 |
   | Segunda a domingo | `0600`–`2319` | PERMITIDA | agora |
   | Segunda a domingo | `2320`–`2359` | PROIBIDA | amanhã às 06:00 |

   **[fila] Bordas:** `0000` e `2359` barram; `0600` libera; `2320` barra inclusivamente, embora a mensagem diga “depois das 23:20”. Zeros à esquerda, inclusive `0008`, `0559`, `0600`, `0808` e `0909`, não causaram erro octal: as comparações usam `[ ... -ge/-lt ... ]`. Não encontrei contraexemplo entre horários HHMM válidos. ([bin/janela_auth.sh:58](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:58), [próxima abertura:63](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:63))

2. **[fila] Escritor e leitores da regra:**

   - A decisão horária é escrita em `proibido`; a próxima abertura, em `proxima`. ([bin/janela_auth.sh:55](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:55), [63](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:63))
   - A guarda lê os alvos declarados em `auth_sitios.txt` e consulta a decisão nos modos horário e deploy. ([bin/janela_auth.sh:84](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:84), [75](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:75), [107](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:107))
   - O deploy chama a guarda ou a atravessa quando recebe motivo não vazio; o selo chama a guarda diretamente. ([bin/deploy.sh:144](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/deploy.sh:144), [bin/tests/test_janela_auth.sh:41](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/tests/test_janela_auth.sh:41))

   **[fila] Prosa velha ainda afirmativa:** a lista de alvos continua proibindo sexta 18h+/fim de semana; O4 no BACKLOG conserva a mesma formulação. A guarda também repete a frase antiga, embora seu cabeçalho já declare a substituição. ([bin/auth_sitios.txt:3](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/auth_sitios.txt:3), [app/docs/BACKLOG.md:1257](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/app/docs/BACKLOG.md:1257), [bin/janela_auth.sh:9](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:9))

   **[fila] Há registros históricos de bloqueio por fim de semana**, que não demonstram um segundo cálculo vigente. `trava.sh` calcula outra janela, de dinheiro/backup; não reproduz a velha proibição de auth. Outro recálculo dessa proibição em `bin/` ou `app/config/`: **[nao verificado]**. ([app/docs/RELATO.md:4251](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/app/docs/RELATO.md:4251), [bin/trava.sh:25](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/trava.sh:25))

   **[bloqueia] Falta registrar o corte e a obra futura.** A busca integral não encontrou `JANELA-NAO-SEGURA-POUSO` em CORTES nem `JANELA-VIRA-SMOKE` no BACKLOG; o cabeçalho promete “Texto literal em CORTES.md”. Isso deixa incompletos os itens 3 e 4 do corte fornecido. ([bin/janela_auth.sh:4](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:4), [app/docs/CORTES.md:1](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/app/docs/CORTES.md:1), [app/docs/BACKLOG.md:1](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/app/docs/BACKLOG.md:1))

3. **[fila] O selo morde as duas regressões pedidas.** Reprodução da seção A exclusivamente em memória:

   - Regra atual: **11/11**.
   - Regra velha: **4 falhas** — sexta `1800`, sábado `1000`/`0600`, domingo `2319`.
   - Nunca barrar: **4 falhas** — sábado `2320`, domingo `0559`, quarta `2320`/`0559`.

   Os casos e expectativas estão em [bin/tests/test_janela_auth.sh:26](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/tests/test_janela_auth.sh:26).

   **[fila] Lacunas:** a seção A só confere prefixo da saída, ignorando status; `proxima()` não é exercitada pelo modo `--janela`. O selo completo não foi executado porque cria, escreve e apaga temporários. ([bin/tests/test_janela_auth.sh:41](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/tests/test_janela_auth.sh:41), [bin/janela_auth.sh:75](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:75), [temporários:79](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/tests/test_janela_auth.sh:79))

4. **[fila] Diretamente, o diff de `deploy.sh` muda só comentário. Indiretamente, o deploy passa a liberar os horários novos porque chama a guarda alterada.** O motivo do corte não é preenchido automaticamente: continua dependendo de quem chama fornecer `SEM_JANELA_AUTH_MOTIVO`. ([bin/deploy.sh:127](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/deploy.sh:127), [144](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/deploy.sh:144), [bin/janela_auth.sh:55](/home/ronald/codex_ws/2b9450e717ef6b6450a5bbd2d77c7ffa03fcaf47/bin/janela_auth.sh:55))

   **[fila] Cumprimento em produção de reversão pré-anotada, ato único, `/api/me/` 401 e linha no topo do RELATO: [nao verificado].**

## 14/11 11:41 · sessao daef3b0a03f3 · - · - · origem=Ronald (tmux)

- **[fila] Prosa antiga ainda prescritiva:** `bin/auth_sitios.txt:3-4` afirma que sexta 18h+ e fim de semana barram; `app/docs/BACKLOG.md:1257` conserva a mesma regra no item O4. `bin/janela_auth.sh:9` repete a frase antiga, embora o próprio cabeçalho `bin/janela_auth.sh:2-4` a revogue expressamente. O selo contextualiza a citação antiga com a mudança em `bin/tests/test_janela_auth.sh:4-6`; o deploy também a marca como passado em `bin/deploy.sh:128-129`.

- **[bloqueia] Registro exigido pelo corte não encontrado:** buscas literais por `JANELA-NAO-SEGURA-POUSO` e `JANELA-VIRA-SMOKE` em `app/docs` não retornaram ocorrência; logo o código promete “Texto literal em CORTES.md” sem que eu localizasse o registro (`bin/janela_auth.sh:4`). A exigência geral de registrar no mesmo turno está em `app/docs/CORTES.md:5-7`. A página é gerada de `app/docs/CORTES.json` (`app/docs/CORTES.md:3`), pelo escritor `bin/gerar_cortes.py:114-116`. A ausência em quaisquer fontes externas à cópia é **[não verificado]**.

- **[fila] Escritor e leitores:** a decisão horária mora em `bin/janela_auth.sh:55-60`; a próxima abertura em `bin/janela_auth.sh:63-69`. A guarda lê a lista declarada (`bin/auth_sitios.txt:13-22`) em `bin/janela_auth.sh:84-95`. O modo horário consulta a decisão em `bin/janela_auth.sh:75-81`; o modo de deploy em `bin/janela_auth.sh:107-114`. O consumidor operacional em `bin/` chama essa guarda, ou atravessa com motivo não vazio, em `bin/deploy.sh:144-148`. O selo chama a guarda em `bin/tests/test_janela_auth.sh:41`, `:86`, `:97`, `:116`.

- **[fila] Sem segundo cálculo encontrado no escopo:** as buscas em `bin/` e `app/config/` localizaram a comparação horária na guarda (`bin/janela_auth.sh:58-59`, `:66-67`) e a chamada pelo deploy (`bin/deploy.sh:146`). Não há diretório `config/` na raiz desta cópia **[não verificado por arquivo:linha]**.

- **[fila] Referências históricas não devem ser tratadas como regra nova:** a negativa de um deploy de outubro em domingo permanece em `app/docs/RELATO.md:3853`; o relato de fim de semana em `app/docs/RELATO.md:4251`; os estados antigos das obras Q4/Q4b em `app/docs/TICKETS.md:224-225`. São registros de atos anteriores, embora estes últimos conservem linguagem de estado corrente.

- **[fila] Consumidor fora do escopo solicitado:** `fatias_agendadas/o135-teto20/esteira.sh:64` também chama a guarda da cópia com `origin/main`, sem recalcular o horário. Outra prosa antiga fora de `bin/`, `app/docs`, `app/config` está em `app/core/management/commands/replay_credenciais.py:4-5`.

## 14/11 11:40 · sessao 94f383df6683 · - · - · origem=Ronald (tmux)

- [fila] **O selo morde ambas as mutações.** Reproduzindo somente a seção A em memória: atual passa 11/11; regra velha falha em sexta `1800`, sábado `1000`/`0600` e domingo `2319` (`bin/tests/test_janela_auth.sh:28-31`). “Nunca barrar” falha em sábado `2320`, domingo `0559`, quarta `2320`/`0559` (`bin/tests/test_janela_auth.sh:32-36`). Os resultados decorrem do prefixo esperado comparado em `bin/tests/test_janela_auth.sh:41-44`.

- [fila] **Limite do selo:** A ignora o código de saída e compara somente o prefixo da saída combinada; uma resposta textual certa com status errado passa essa seção (`bin/tests/test_janela_auth.sh:41-44`). `proxima()` também fica descoberta: `--janela` termina sem chamá-la (`bin/janela_auth.sh:75-81`); D2 verifica apenas status e `BLOQUEADO`, D3 apenas status e `nao existe` (`bin/tests/test_janela_auth.sh:105`, `bin/tests/test_janela_auth.sh:118`).

- [fila] **Não executei o selo completo:** ele cria temporários, escreve carimbos e apaga diretórios (`bin/tests/test_janela_auth.sh:79-82`, `bin/tests/test_janela_auth.sh:125`, `bin/tests/test_janela_auth.sh:160-161`). A reprodução acima usou texto extraído e substituições exclusivamente em memória, sem arquivos [não verificado por artefato persistido].

- [fila] **No arquivo `deploy.sh`, o hunk fornecido altera só comentário** (`bin/deploy.sh:127-130`); o desvio com motivo e a chamada da guarda permanecem em `bin/deploy.sh:144-148`. **O comportamento efetivo do deploy muda indiretamente**, pois a guarda chamada passa a liberar sexta à noite/fim de semana dentro da faixa diurna (`bin/janela_auth.sh:55-60`, `bin/deploy.sh:146`).

## 14/11 11:28 · sessao 5dd1b348cf18 · HJ-091 · sonda · origem=Code

Transcrição do bloco fornecido; sem confirmação independente em produção. [nao verificado]

Universo: **3001 ausências de férias; 116 agendamentos; 2943 dias vivos**. [nao verificado]

**1. Aprovadas sem agendamento vivo cobrindo: 58; 53 colaboradores. PKs das ausências:** [nao verificado]

```text
4635, 4839, 4165, 4613, 4630, 4634, 4633, 4631,
4615, 4614, 4620, 4617, 4611, 4623, 4628, 4618,
4624, 4625, 4627, 4629, 4621, 4626, 4622, 4651,
4652, 4654, 4653, 4656, 4664, 4663, 4661, 4662,
4655, 4658, 4659, 4660, 4657, 4665, 4641, 4666,
4667, 4642, 4636, 4669, 4643, 4668, 4644, 4645,
4637, 4646, 4649, 4648, 4647, 4650, 4638, 4639,
4640, 4632
```

**2. Dias de agendamento vivo sem ausência aprovada: 4; 2 colaboradores.** As tuplas identificam colaborador, data e agendamento; não fornecem PK de ausência. [nao verificado]

```text
(127, '2026-10-02', [114])
(344, '2026-11-05', [103])
(344, '2026-11-06', [103])
(344, '2026-11-07', [103])
```

**3. Rejeitadas ou canceladas dentro de agendamento vivo: 3; 1 colaborador.** Todas as linhas exibidas são `rejeitada`. [nao verificado]

```text
PK ausência | colaborador | data       | agendamento
3977        | 344         | 2026-11-07 | 103
3995        | 344         | 2026-11-06 | 103
3985        | 344         | 2026-11-05 | 103
```

**4. Backfill — sem marca #N na descrição: 58. PKs das ausências:** [nao verificado]

```text
4635, 4612, 4839, 4613, 4630, 4634, 4633, 4631,
4615, 4614, 4620, 4617, 4611, 4623, 4628, 4618,
4624, 4625, 4627, 4629, 4621, 4626, 4622, 4651,
4652, 4654, 4653, 4656, 4664, 4663, 4661, 4662,
4655, 4658, 4659, 4660, 4657, 4665, 4641, 4666,
4667, 4642, 4636, 4669, 4643, 4668, 4644, 4645,
4637, 4646, 4649, 4648, 4647, 4650, 4638, 4639,
4640, 4632
```

Nesta classe, **4612 é rejeitada; as outras 57 são aprovadas**. [nao verificado]

**5. Backfill — marca #N não corresponde a agendamento vivo cobrindo o dia: 2.** [nao verificado]

```text
PK ausência | colaborador | data       | status   | marca
4165        | 168         | 2026-10-20 | aprovada | 109
2634        | 127         | 2026-09-02 | aprovada | 77
```

**Casos pedidos:**

- **#3977 aparece na classe 3**, rejeitada, coberta pelo agendamento **103**. [nao verificado]
- **#2634 aparece na classe 5**, aprovada, com marca **77**, mas cobertura indicada pelo agendamento **114**. [nao verificado]

As categorias específicas **“rejeitadas sem origem dentro de agendamento vivo”** e **“aprovadas sem origem dentro de agendamento vivo”** não têm contagem explícita no bloco: ficam **[nao verificado]**, não zero. A marca textual é produzida em [ausencia.py:1207](/home/ronald/codex_ws/a1fddccfa8d73ab1e28470c92f7f7519ee6933f9/app/ponto/services/ausencia.py:1207), e a busca de existentes usa colaborador, tipo e janela em [ausencia.py:1183](/home/ronald/codex_ws/a1fddccfa8d73ab1e28470c92f7f7519ee6933f9/app/ponto/services/ausencia.py:1183); isso não demonstra a origem individual desses registros. [nao verificado]

**Erros da sonda anexa:** contador literal **`ERROS 0`**; **0 Tracebacks exibidos**. Execução completa e erros fora do trecho: **[nao verificado]**.

## 14/11 11:27 · sessao f1a9d220e305 · HJ-091 · sonda · origem=Code

| Campo | Valor |
|---|---|
| universo_admin_encerrado_colab_com_ferias | 845 [nao verificado] |
| com_dia_do_fato | 419 [nao verificado] |
| dia_coberto_por_ferias | 27 [nao verificado] |
| veto_admin_sem_fato_novo_ativo_hoje | 16 [nao verificado] |
| erros (valor transcrito) | 0 [nao verificado] |
| pks_veto | [3331, 3444, 3934, 4533, 4879, 5968, 6184, 7081, 7084, 7805, 7809, 8640, 11141, 11776, 12526, 12801] [nao verificado] |
| Ocorrências literais de `ERRO` no bloco raspado | 0 [nao verificado] |
| Ocorrências literais de `Traceback` no bloco raspado | 0 [nao verificado] |
| Ausência efetiva de erros na execução | [nao verificado] |

## 14/11 11:25 · sessao a0dde6e5aac0 · FRAC5 · verificador · origem=Code

**Fechamento parcial.** O delta fecha a remarcação regular de **1–4 dias declarados**, a omissão do aviso de `caput`, a frase “será recusado” na correção administrativa e o texto incompleto do formulário inline. Não faz a remarcação passar pelo julgamento completo da criação. (`app/ferias/services.py:714`; `app/ferias/situacao.py:237`; `app/ferias/situacao.py:359`; `app/templates/ferias/_secao_colab.html:63`)

**NÃO PROCEDE novo bypass de gravação demonstrado:** a nova guarda acrescenta uma recusa antes do mesmo escritor. **PROCEDE nova precedência de erro:** essa recusa ocorre antes das guardas de status, início e saldo. (`app/ferias/services.py:714`; `app/ferias/services.py:717`; `app/ferias/services.py:670`; `app/ferias/services.py:675`)

A comparação com `5d8bc6879` abaixo é contrafactual pelo delta fornecido. Execução da suíte Django, execução real na base, pouso e efeitos em produção: **[nao verificado]**.

**1. Remarcação e demais caminhos**

**PROCEDE — mesmo helper; NÃO PROCEDE — mesmo juiz registrado e mesmo julgamento.** O registro aponta para `AgendamentoFerias.clean`. A remarcação chama diretamente `violacoes_de_fracionamento([novos_dias])` e considera somente `par1_demais`. A criação considera também quantidade de frações, presença de uma ≥14 e coerência entre `dias` e datas. (`app/core/juizes.py:271`; `app/ferias/services.py:714`; `app/ferias/models.py:372`; `app/ferias/models.py:380`; `app/ferias/models.py:395`)

Dois contraexemplos delimitam a cura:

- Uma única fração **14→5** passa pela nova guarda; `clean()` recusaria `[5]` pela ausência de fração ≥14. Essa possibilidade de remarcação já existia e permanece. (`app/ferias/services.py:714`; `app/ferias/services.py:693`; `app/ferias/models.py:380`)
- Uma chamada direta com **`novos_dias=5`, mas intervalo de quatro dias**, passa pelo mínimo novo. O escritor recebe e grava quantidade e datas separadamente; não chama `clean()`. As views atuais calculam o fim coerentemente. (`app/ferias/services.py:680`; `app/ferias/services.py:693`; `app/ferias/views.py:507`; `app/ferias/views.py:718`)

| Caminho | Tratamento de fração `<5` |
|---|---|
| Remarcação regular | Recusa 1–4 em `novos_dias`. A view exige quantidade positiva e calcula o fim; o serviço não verifica equivalência dias/datas. (`app/ferias/services.py:714`; `app/ferias/views.py:712`; `app/ferias/views.py:718`; `app/ferias/services.py:680`) |
| `corrigir_datas_agendamento` | Delega diretamente a `_trocar_janela`; permite `<5` se as demais guardas permitirem. A view monta avisos e os acrescenta ao sucesso. **NÃO PROCEDE furo novo:** a exceção A3 está expressa. (`app/ferias/services.py:702`; `app/ferias/views.py:508`; `app/ferias/views.py:517`; `app/ferias/views.py:552`) |
| `_trocar_janela` diretamente | É o escritor material encontrado para datas/dias existentes. Não aplica fracionamento; verifica status/início, saldo e, no `save`, colisão. (`app/ferias/services.py:670`; `app/ferias/services.py:675`; `app/ferias/services.py:680`; `app/ferias/services.py:693`) |
| Django admin | O registro local oferece somente `Afastamento`, sem edição de `AgendamentoFerias`. (`app/ferias/admin.py:2`; `app/ferias/admin.py:4`) |
| Comando de sincronização | Muda somente status pela porta; não recusa nem corrige duração curta. (`app/ferias/management/commands/sincronizar_status_ferias.py:48`; `app/ferias/services.py:925`; `app/ferias/services.py:928`) |
| Comando `listar_ferias_dp` | Simula cancelamento e validação, depois força rollback; não remarca datas/dias. (`app/ferias/management/commands/listar_ferias_dp.py:100`; `app/ferias/management/commands/listar_ferias_dp.py:107`; `app/ferias/management/commands/listar_ferias_dp.py:109`) |
| API do colaborador | POST retorna 405; GET entrega os dias/datas existentes. (`app/api/views.py:2223`; `app/api/views.py:2244`; `app/api/views.py:2246`) |
| API de mensageria | Consulta situação e proposta hipotética; esses caminhos produzem avisos. (`app/api/views_mensageria.py:812`; `app/api/views_mensageria.py:824`; `app/api/views_mensageria.py:847`) |

Outros chamadores dinâmicos ou escritores externos: **[nao verificado]**.

**PROCEDEM, como pontos que o delta não fecha:**

- O histórico pode criar fração curta sem julgamento de fracionamento; é criação de outra linha, não alteração de agendamento existente. (`app/ferias/services.py:484`; `app/ferias/services.py:492`; `app/ferias/services.py:510`)
- Cancelar o 14 de `[14,5]` deixa `[5]`; o cancelamento não reavalia o fracionamento restante. (`app/ferias/services.py:616`; `app/ferias/services.py:619`; `app/ferias/models.py:370`)
- `save()` continua sem chamar `clean()`; portanto não é uma porta universal do fracionamento. (`app/ferias/models.py:461`)
- Zero continua desaparecendo do helper por `if f`: `[0]` e `[14,0]` não geram violações. A nova guarda herda isso, embora as duas views recusem zero. (`app/ferias/models.py:271`; `app/ferias/services.py:714`; `app/ferias/views.py:503`; `app/ferias/views.py:712`)
- Fração curta histórica continua admitida ao lado de nova fração válida; isso está declarado no próprio `clean()`. O `self` também continua entrando no julgamento independentemente do seu status. (`app/ferias/models.py:384`; `app/ferias/models.py:379`; `app/ferias/models.py:387`)

**2. Quem escreve e quem lê**

| Campo/valor | Quem escreve ou monta | Quem lê |
|---|---|---|
| `novos_dias` | As views convertem o POST para inteiro. (`app/ferias/views.py:500`; `app/ferias/views.py:709`) | Views verificam positividade e calculam fim; remarcação julga o mínimo; escritor verifica saldo, ajusta concedidos e atribui `dias`. (`app/ferias/views.py:503`; `app/ferias/views.py:718`; `app/ferias/services.py:714`; `app/ferias/services.py:676`; `app/ferias/services.py:679`; `app/ferias/services.py:680`) |
| Novas datas e `AgendamentoFerias.dias` | `_trocar_janela` atribui e persiste. (`app/ferias/services.py:680`; `app/ferias/services.py:693`) | `clean()` compara dias/datas; fracionamento lê os dias das outras linhas; situação lê dias gravados; APIs serializam. (`app/ferias/models.py:395`; `app/ferias/models.py:378`; `app/ferias/situacao.py:122`; `app/api/views.py:2244`) |
| Violações `par1`, `caput`, `par1_demais` | O helper monta `out` e retorna a lista. (`app/ferias/models.py:274`; `app/ferias/models.py:276`; `app/ferias/models.py:278`; `app/ferias/models.py:279`) | `clean()` lê as três cláusulas; remarcação lê apenas `par1_demais`; situação e proposta recebem a lista. (`app/ferias/models.py:372`; `app/ferias/models.py:380`; `app/ferias/models.py:387`; `app/ferias/services.py:714`; `app/ferias/situacao.py:227`; `app/ferias/situacao.py:348`) |
| `_v` | Variável local da situação e da proposta; não é campo persistido. (`app/ferias/situacao.py:227`; `app/ferias/situacao.py:348`) | Ramos que acrescentam cada aviso. (`app/ferias/situacao.py:228`; `app/ferias/situacao.py:237`; `app/ferias/situacao.py:244`; `app/ferias/situacao.py:349`; `app/ferias/situacao.py:355`) |
| `FRACAO_MINIMA_DEMAIS` | Constante definida como 5. (`app/ferias/models.py:253`) | Helper compara; remarcação e avisos interpolam a mensagem. (`app/ferias/models.py:277`; `app/ferias/services.py:716`; `app/ferias/situacao.py:249`; `app/ferias/situacao.py:359`) |
| Avisos `fracionamento` | `_aviso` monta `{codigo,texto,nivel,ancora}`; situação e proposta acrescentam os avisos. (`app/ferias/situacao.py:64`; `app/ferias/situacao.py:229`; `app/ferias/situacao.py:238`; `app/ferias/situacao.py:245`; `app/ferias/situacao.py:350`; `app/ferias/situacao.py:356`) | Situação indexa preservando listas; API entrega os avisos; correção administrativa filtra colisão e apresenta os textos. (`app/ferias/situacao.py:302`; `app/api/views_mensageria.py:847`; `app/api/views_mensageria.py:849`; `app/ferias/views.py:517`; `app/ferias/views.py:552`) |
| `ValidationError.messages` | Nova guarda produz mensagem do mínimo; guarda de colisão também pode produzir `ValidationError`. (`app/ferias/services.py:715`; `app/ferias/models.py:454`) | View de edição apresenta as mensagens recebidas. (`app/ferias/views.py:729`; `app/ferias/views.py:730`) |

**3. `if` no lugar de `elif`**

**NÃO PROCEDE duplicação ou contradição causada pela troca.** Os três ramos descrevem violações diferentes. Repetem `codigo='fracionamento'` e a âncora do período, mas o índice guarda uma lista, sem sobrescrever o aviso anterior. (`app/ferias/situacao.py:228`; `app/ferias/situacao.py:237`; `app/ferias/situacao.py:244`; `app/ferias/situacao.py:302`)

| Frações | Avisos de fracionamento |
|---|---|
| `[14,5]` | Nenhum. (`app/ferias/models.py:273`; `app/ferias/models.py:275`; `app/ferias/models.py:277`) |
| `[5,5]` | `par1`. (`app/ferias/models.py:273`; `app/ferias/situacao.py:228`) |
| `[14,5,5,5]` | `caput`. (`app/ferias/models.py:275`; `app/ferias/situacao.py:237`) |
| `[5,5,5,5]` | `par1` + `caput`; o segundo era omitido. (`app/ferias/situacao.py:228`; `app/ferias/situacao.py:237`) |
| `[4,5,5,5]` | `par1` + `caput` + `par1_demais`. (`app/ferias/situacao.py:228`; `app/ferias/situacao.py:237`; `app/ferias/situacao.py:244`) |
| `[14,4,5,5]` | `caput` + `par1_demais`. (`app/ferias/models.py:275`; `app/ferias/models.py:277`) |

**PROCEDE fechamento do achado da volta 1:** coexistência de violações agora produz os respectivos avisos. **PROCEDE fechamento da contradição administrativa:** o aviso deixou de prometer recusa e a correção continua podendo gravar. (`app/ferias/situacao.py:237`; `app/ferias/situacao.py:359`; `app/ferias/views.py:531`)

**4. Exceções em `editar_agendamento`**

**PROCEDE origem diferente do fracionamento:** `vivo.save()` chama `_guard_sobreposicao`, que traduz `VetoDePrecedencia` em `ValidationError`. **NÃO PROCEDE mensagem enganosa nessa captura:** “juiz do fracionamento” está somente no comentário; a resposta usa as mensagens originais da exceção. (`app/ferias/services.py:693`; `app/ferias/models.py:449`; `app/ferias/models.py:454`; `app/ferias/views.py:729`; `app/ferias/views.py:730`)

A ordem efetiva é:

1. Mínimo de cinco: `ValidationError`, antes da transação/trava. (`app/ferias/services.py:714`)
2. Status cancelado e guarda de ainda não iniciado: `AgendamentoNaoRemarcavel`, sob trava. (`app/ferias/services.py:600`; `app/ferias/services.py:670`; `app/ferias/services.py:671`)
3. Saldo: `SaldoInsuficienteParaRemarcar`. (`app/ferias/services.py:675`; `app/ferias/services.py:677`)
4. Colisão no `save`: `ValidationError`; a limpeza da janela antiga está na mesma transação. (`app/ferias/services.py:669`; `app/ferias/services.py:688`; `app/ferias/services.py:693`; `app/ferias/models.py:454`)

Os `except` aparecem como não remarcável → saldo → validação. Isso não muda a precedência acima: uma chamada de quatro dias pode receber primeiro o erro de fracionamento mesmo quando a linha já não poderia ser remarcada. (`app/ferias/views.py:725`; `app/ferias/views.py:727`; `app/ferias/views.py:729`; `app/ferias/services.py:714`)

**5. Cada asserção nova contra `5d8bc6879`**

| Asserção | Falharia na base? |
|---|---|
| `assertRaises(ValidationError)` | **Sim, contrafactualmente:** sem a guarda acrescentada, o escritor gravaria quatro dias sem `clean()`. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`; `app/ferias/services.py:680`; `app/ferias/services.py:693`; `app/ferias/services.py:714`) |
| Mensagem contém `5 dias` | **Não seria alcançada**, porque o `assertRaises` já falharia; a mensagem específica vem da guarda nova. (`app/ponto/tests/test_ferias_fracao_demais_5.py:137`; `app/ferias/services.py:716`) |
| Após refresh, `dias == 14` | **Não seria alcançada; isoladamente falharia:** a chamada anterior deixaria quatro dias. (`app/ponto/tests/test_ferias_fracao_demais_5.py:139`; `app/ferias/services.py:680`; `app/ferias/services.py:693`) |
| Fonte de `corrigir_datas_agendamento` não contém o helper | **Não:** passa antes e depois. Não prova que a correção executa nem que apresenta aviso; verifica apenas o texto do wrapper. (`app/ponto/tests/test_ferias_fracao_demais_5.py:145`; `app/ferias/services.py:700`) |
| Fonte da situação não contém `elif 'caput' in _v` | **Sim, contrafactualmente:** a expressão foi substituída. Não executa `[5,5,5,5]` nem verifica dois avisos. (`app/ponto/tests/test_ferias_fracao_demais_5.py:153`; `app/ferias/situacao.py:237`) |

**NÃO PROCEDE conjunto inteiro sem mordida:** existe teste comportamental da recusa. **PROCEDE fragilidade dos outros dois selos:** ausência de substring não demonstra a política administrativa nem a emissão simultânea anunciada pelo nome do teste. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`; `app/ponto/tests/test_ferias_fracao_demais_5.py:145`; `app/ponto/tests/test_ferias_fracao_demais_5.py:153`)

| Classe pedida | Veredito |
|---|---|
| **2º juiz duplicando o registrado** | **PROCEDE decisão parcial fora do registrado; NÃO PROCEDE novo comparador numérico duplicado.** O helper é compartilhado, mas a remarcação escolhe só uma cláusula e não chama `clean()`. (`app/core/juizes.py:271`; `app/ferias/services.py:714`) |
| **2º escritor fora da porta** | **NÃO PROCEDE no censo encontrado:** ambas as entradas convergem no escritor permitido. Escritores externos/dinâmicos: **[nao verificado]**. (`app/core/portas.py:489`; `app/ferias/services.py:702`; `app/ferias/services.py:717`) |
| **Relógio fora da borda** | **PROCEDE preexistente:** situação/proposta têm fallback interno e cancelamento admin lê a data. A guarda nova não lê relógio. (`app/ferias/situacao.py:156`; `app/ferias/situacao.py:326`; `app/ferias/services.py:649`; `app/ferias/services.py:714`) |
| **Derivação paralela de fonte única** | **PROCEDE:** criação julga duração pelas datas; remarcação julga o parâmetro. Também permanece a montagem separada das frações no modelo e no aviso. (`app/ferias/models.py:377`; `app/ferias/services.py:714`; `app/ferias/models.py:368`; `app/ferias/situacao.py:117`) |
| **Selo/RED sem mordida** | **PROCEDE limitação dos testes por substring; NÃO PROCEDE ausência total de mordida.** RED efetivamente executado contra a base: **[nao verificado]**. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`; `app/ponto/tests/test_ferias_fracao_demais_5.py:145`; `app/ponto/tests/test_ferias_fracao_demais_5.py:153`) |
| **Ordem de trava** | **NÃO PROCEDE inversão introduzida:** segue agendamento → período → ausências. A guarda nova antecede as travas e altera prioridade de recusa. (`app/ferias/services.py:599`; `app/ferias/services.py:602`; `app/ferias/services.py:688`; `app/ferias/services.py:714`) |
| **Pouso antes do pré-requisito** | **NÃO PROCEDE como ocorrência demonstrada:** sequência real de suíte, revisão e pouso **[nao verificado]**. | [nao verificado]
| **Campo com dois significados** | **NÃO PROCEDE confusão demonstrada entre gozo e abono; PROCEDE falta de equivalência universal dias/datas.** A venda permanece separada, mas o escritor aceita quantidade e intervalo independentes. (`app/ferias/services.py:277`; `app/ferias/services.py:680`; `app/ferias/models.py:395`) |

## 14/11 11:23 · sessao 0c1c22091564 · - · - · origem=Ronald (tmux)

**PROCEDE — ocultação do caput foi fechada. NÃO PROCEDE — `if` novo gera aviso duplicado ou contraditório:** cada ramo relata uma violação distinta; código e âncora coincidem, mas textos diferem, e o índice preserva uma lista. (`app/ferias/situacao.py:227`; `app/ferias/situacao.py:228`; `app/ferias/situacao.py:237`; `app/ferias/situacao.py:244`; `app/ferias/situacao.py:300`)

Tabela confirmada executando somente o juiz extraído por AST em memória; os avisos correspondentes decorrem dos três ramos independentes. (`app/ferias/models.py:265`; `app/ferias/situacao.py:228`; `app/ferias/situacao.py:237`; `app/ferias/situacao.py:244`)

| Frações | Violações/avisos |
|---|---|
| `[14,5]` | nenhum |
| `[5,5]` | `par1` |
| `[14,5,5,5]` | `caput` |
| `[5,5,5,5]` | `par1`, `caput` |
| `[4,5,5,5]` | `par1`, `caput`, `par1_demais` |
| `[14,4,5,5]` | `caput`, `par1_demais` |
| `[4]` | `par1`, `par1_demais` |

Fonte de todas as linhas: `app/ferias/models.py:271`, `app/ferias/models.py:273`, `app/ferias/models.py:275`, `app/ferias/models.py:277`.

**PROCEDE — permanece o zero ignorado:** `[14,0] → []`; isso precede o delta, pois o filtro elimina valores falsos. (`app/ferias/models.py:271`)

Produtores e consumidores:

| Valor | Quem escreve/monta | Quem lê |
|---|---|---|
| Violações | `out.append` em `app/ferias/models.py:274`, `:276`, `:278`; retorno em `:279` | `clean`: `app/ferias/models.py:372`, `:380`, `:387`; remarcação: `app/ferias/services.py:714`; situação: `app/ferias/situacao.py:227`; proposta: `:348` |
| `_v` | Situação em `app/ferias/situacao.py:227`; proposta em `:348` | Situação em `:228`, `:237`, `:244`; proposta em `:349`, `:355` |
| Avisos `fracionamento` | Formato em `app/ferias/situacao.py:64`; situação em `:229`, `:238`, `:245`; proposta em `:350`, `:356` | Situação indexa em `:300` e retorna em `:311`; API entrega em `app/api/views_mensageria.py:847`, `:849`; correção filtra em `app/ferias/views.py:517` e apresenta textos em `:552` |

**PROCEDE — contradição “será recusado” na correção foi fechada:** o texto atual apenas relata o mínimo; a correção continua exibindo esse aviso após o sucesso. (`app/ferias/situacao.py:359`; `app/ferias/views.py:549`; `app/ferias/views.py:552`)

Contrafactual dos testes contra a base, pelo delta fornecido; execução real da base e da suíte: **[nao verificado]**. [nao verificado]

| Asserção | Base 5d8bc6879 |
|---|---|
| `assertRaises(ValidationError)` | **Falharia:** o serviço anterior delegava diretamente a `_trocar_janela`, que gravava quatro dias sem `clean()`. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`; `app/ferias/services.py:680`; `app/ferias/services.py:693`; delta fornecido) |
| Mensagem contém `5 dias` | **Não alcançada na base:** a asserção anterior já falha; a guarda nova produz a mensagem. (`app/ponto/tests/test_ferias_fracao_demais_5.py:137`; `app/ferias/services.py:714`) |
| `dias == 14` após refresh | **Não alcançada na base; isoladamente falharia:** a escrita anterior deixaria quatro. (`app/ponto/tests/test_ferias_fracao_demais_5.py:139`; `app/ferias/services.py:680`; `app/ferias/services.py:693`) |
| Fonte de `corrigir_datas_agendamento` não contém o nome do juiz | **Passaria na base e agora:** wrapper permanece igual; não prova execução nem presença do aviso. (`app/ponto/tests/test_ferias_fracao_demais_5.py:145`; `app/ferias/services.py:700`) |
| Fonte da situação não contém `elif 'caput' in _v` | **Falharia na base:** o delta substitui essa expressão; não verifica comportamento ou quantidade de avisos. (`app/ponto/tests/test_ferias_fracao_demais_5.py:153`; `app/ferias/situacao.py:237`; delta fornecido) |

**NÃO PROCEDE — conjunto novo inteiro sem mordida:** há teste comportamental da recusa. **PROCEDE — selo fraco nos outros dois:** ausência de substring não prova política de correção nem os dois avisos anunciados pelo nome do teste. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`; `app/ponto/tests/test_ferias_fracao_demais_5.py:145`; `app/ponto/tests/test_ferias_fracao_demais_5.py:153`)

## 14/11 11:23 · sessao 751f412b7a4f · - · - · origem=Ronald (tmux)

**PROCEDE — `ValidationError` também pode vir de colisão, mas NÃO PROCEDE mensagem enganosa na view.** `_trocar_janela` chama `vivo.save()`, que executa `_guard_sobreposicao`; esta traduz `VetoDePrecedencia` em `ValidationError` conservando a mensagem do veto. A view apresenta `e.messages` diretamente; “juiz do fracionamento” aparece apenas no comentário do `except`. (`app/ferias/services.py:693`; `app/ferias/models.py:449`; `app/ferias/models.py:454`; `app/ferias/views.py:729`; `app/ferias/views.py:730`)

**Ordem efetiva das recusas:**

1. Novo mínimo de cinco, antes de qualquer trava: `ValidationError`. (`app/ferias/services.py:714`)
2. Dentro do `atomic`, status cancelado sob trava: `AgendamentoNaoRemarcavel`; depois guarda de ainda não iniciado, também sob trava. (`app/ferias/services.py:669`; `app/ferias/services.py:670`; `app/ferias/services.py:599`; `app/ferias/services.py:600`; `app/ferias/services.py:671`)
3. Saldo sob trava: `SaldoInsuficienteParaRemarcar`. (`app/ferias/services.py:675`)
4. Colisão no `save`: `ValidationError`; a transação engloba a limpeza da janela antiga e a gravação. (`app/ferias/services.py:688`; `app/ferias/services.py:693`; `app/ferias/models.py:454`)

**PROCEDE — o delta altera a precedência observável de erros.** Uma chamada ao serviço com quatro dias falha pelo mínimo antes de descobrir que a linha está cancelada, iniciada ou sem saldo. A ordem dos `except` na view — não remarcável, saldo, validação — não modifica essa precedência de execução. (`app/ferias/services.py:714`; `app/ferias/services.py:717`; `app/ferias/views.py:725`; `app/ferias/views.py:727`; `app/ferias/views.py:729`)

**PROCEDE — cura parcial e fonte diferente da criação.** O delta julga `[novos_dias]`; a criação calcula a duração pelas datas e verifica sua igualdade com `dias`. `_trocar_janela` grava os três argumentos sem executar `clean()`, e o `save()` também não o chama. Portanto, uma chamada direta com `novos_dias=5` e intervalo de quatro dias atravessa a nova guarda, ressalvadas as demais condições da porta. A possibilidade de divergência já existia; o delta não a fecha. (`app/ferias/services.py:714`; `app/ferias/services.py:680`; `app/ferias/services.py:693`; `app/ferias/models.py:377`; `app/ferias/models.py:395`; `app/ferias/models.py:461`)

**PROCEDE — zero atravessa a guarda isolada.** `violacoes_de_fracionamento([0])` elimina o zero por `if f`; a nova guarda não o recusa. As duas views barram valores inferiores a um e calculam o fim a partir de `novos_dias`, portanto não demonstram esse acesso pela interface. Chamadas externas e ocorrência em produção: **[nao verificado]**. (`app/ferias/models.py:271`; `app/ferias/services.py:714`; `app/ferias/views.py:503`; `app/ferias/views.py:507`; `app/ferias/views.py:712`; `app/ferias/views.py:718`)

**Outras origens inspecionadas:** o `PeriodoAquisitivo.save(update_fields=['dias_concedidos', 'status'])` não executa sua guarda de sobreposição de datas, pois ela retorna quando `update_fields` não contém datas. A validação de campo de uma Ausência recém-criada é convertida em `AusenciaInvalida`, que o espelhador captura; a criação da pauta captura exceções. O `Ausencia.save()` tem ainda uma `ValidationError` para fim anterior ao início, mas o espelhador fornece um dia por criação e deriva o fim; não há entrada invertida demonstrada nesse caminho. (`app/ferias/services.py:695`; `app/ferias/models.py:185`; `app/ponto/services/ausencia.py:387`; `app/ponto/services/ausencia.py:389`; `app/ponto/services/ausencia.py:1203`; `app/ponto/services/ausencia.py:1210`; `app/ponto/services/ausencia.py:1257`; `app/ponto/models.py:895`; `app/ponto/services/ausencia.py:197`)

## 14/11 11:23 · sessao 10ff1864ec85 · - · - · origem=Ronald (tmux)

- **PROCEDE — fechamento parcial da remarcação.** Agora `novos_dias=1..4` é recusado pelo mesmo helper numérico usado por `clean()`. Porém não chama o juiz registrado `AgendamentoFerias.clean`: deixa de julgar `par1`, `caput` e coerência dias/datas. Assim, remarcação de única fração `14→5` passa pelo serviço, embora `clean()` recusasse ausência de ≥14; datas de quatro dias com `novos_dias=14` também escapam da nova guarda. Isso já era permitido pelo escritor; o delta fecha somente o mínimo declarado em `novos_dias`. (`app/ferias/services.py:714`; `app/ferias/services.py:717`; `app/core/juizes.py:271`; `app/ferias/models.py:372`; `app/ferias/models.py:377`; `app/ferias/models.py:380`; `app/ferias/models.py:395`)

- **NÃO PROCEDE — duplicação do comparador numérico.** A remarcação chama o helper existente, sem introduzir outro `<5`. **PROCEDE — julgamento parcial fora do juiz registrado:** selecionar apenas `par1_demais` não equivale a passar por `clean()`. (`app/ferias/services.py:713`; `app/ferias/services.py:714`; `app/core/juizes.py:271`)

| Caminho de produção | Fração `<5` e escrita |
|---|---|
| Remarcação regular | Recusa `novos_dias` não zero abaixo de cinco antes do escritor; view exige ≥1 e calcula fim coerente. (`app/ferias/services.py:714`; `app/ferias/views.py:712`; `app/ferias/views.py:718`) |
| Correção administrativa | Delega diretamente ao mesmo escritor; não julga fracionamento. View monta avisos e os anexa ao sucesso. Exceção A3 expressa: não constitui furo novo. (`app/ferias/services.py:702`; `app/ferias/views.py:508`; `app/ferias/views.py:517`; `app/ferias/views.py:531`; `app/ferias/views.py:552`) |
| `_trocar_janela` | Única atribuição material encontrada para datas/dias existentes; aceita `<5` se demais guardas permitirem. `save()` não chama `clean()`. (`app/ferias/services.py:680`; `app/ferias/services.py:693`; `app/ferias/models.py:461`) |
| Django admin | Só registra `Afastamento`; não oferece edição de `AgendamentoFerias`. (`app/ferias/admin.py:2`; `app/ferias/admin.py:4`) |
| Comando de sincronização | Delega à porta que atualiza apenas status; não corrige nem rejeita duração curta. (`app/ferias/management/commands/sincronizar_status_ferias.py:48`; `app/ferias/services.py:925`; `app/ferias/services.py:928`) |
| Comando `listar_ferias_dp` | Simula cancelamento, consulta validação e força rollback; não altera datas/dias. (`app/ferias/management/commands/listar_ferias_dp.py:100`; `app/ferias/management/commands/listar_ferias_dp.py:107`; `app/ferias/management/commands/listar_ferias_dp.py:109`) |
| API do colaborador | POST retorna 405; GET serializa inclusive dias gravados. (`app/api/views.py:2223`; `app/api/views.py:2224`; `app/api/views.py:2244`; `app/api/views.py:2246`) |
| API de mensageria | Consulta situação e proposta hipotética; não escreve agendamento. (`app/api/views_mensageria.py:812`; `app/api/views_mensageria.py:824`; `app/api/views_mensageria.py:828`) |

- **NÃO PROCEDE — segundo escritor fora da porta no censo encontrado.** O registro permite `ferias/services.py`, e as duas entradas de edição usam `_trocar_janela`. Escritores dinâmicos/externos: **[nao verificado]**. (`app/core/portas.py:489`; `app/ferias/services.py:702`; `app/ferias/services.py:717`)

- **NÃO PROCEDE — mudança da ordem de travas pelo delta.** A ordem segue agendamento → período → ausências. A nova validação usa somente argumento e acontece antes dessas travas; pode mudar qual erro aparece primeiro, mas não adquire trava em ordem inversa. (`app/ferias/services.py:599`; `app/ferias/services.py:602`; `app/ferias/services.py:688`; `app/ferias/services.py:714`; `app/ferias/services.py:717`)

- **PROCEDE — precedência nova das recusas.** Pedido de quatro dias levanta `ValidationError` antes de verificar status, início e saldo da linha travada. Para pedidos que passam no mínimo: status → `pode_editar_em` → saldo → colisão no `save()`. (`app/ferias/services.py:714`; `app/ferias/services.py:717`; `app/ferias/services.py:600`; `app/ferias/services.py:671`; `app/ferias/services.py:676`; `app/ferias/services.py:693`)

- **NÃO PROCEDE — unidade nova ou confusão com abono.** `dias` continua quantidade do gozo; venda segue em `PeriodoAquisitivo.dias_vendidos`. **PROCEDE — falta preexistente de imposição universal da equivalência dias/datas:** criação deriva duração pelas datas, remarcação julga parâmetro e grava ambos independentemente. (`app/ferias/services.py:277`; `app/ferias/models.py:377`; `app/ferias/models.py:395`; `app/ferias/services.py:714`; `app/ferias/services.py:680`)

- **PROCEDE preexistente — relógio fora da borda.** Cancelamento admin usa `date.today()` internamente e avisos possuem fallback; a nova guarda não lê relógio. (`app/ferias/services.py:649`; `app/ferias/situacao.py:326`; `app/ferias/services.py:714`)

- **NÃO PROCEDE — contradição “será recusado” na correção administrativa após delta.** O aviso agora informa somente a regra; a correção segue gravando e anexando avisos conforme A3. (`app/ferias/situacao.py:358`; `app/ferias/situacao.py:359`; `app/ferias/views.py:508`; `app/ferias/views.py:552`)

## 14/11 10:58 · sessao 6801ddc748bf · [nome]-ANDAM-JUNTO · verificador · origem=Code

**O delta fecha as lacunas de formato e de `resolve`; não comprova as três correções de conteúdo.** A autocolisão literal da E3 saiu, mas entrou uma ambiguidade em `livre (...)`; DXG e O232 continuam sem prova consultável das novas alegações (`bin/placar_grade.py:58`, `:66`, `:127`; `app/docs/GRADE-MESTRA.md:119`, `:272`, `:853`).

**1. Achados da volta 2: cura e mordida**

| Achado | Veredito nesta volta | Cura/preservação e caso do selo |
|---|---|---|
| Oito/dez colunas sem deslocamento | **PROCEDE a preservação.** | Esquema pelo cabeçalho e rejeição de largura em `bin/placar_grade.py:80`, `:93`; `RJ-LARGA` e assert em `bin/tests/test_placar_grade.sh:141`, `:174`. |
| Células semanticamente inválidas | **Parcialmente fechado.** Número nu e os três portões nus agora são rejeitados. A identificação continua sendo apenas texto não vazio; oito colunas continuam aceitando estado por prefixo. | Curas em `bin/placar_grade.py:58`, `:66`, `:103`; prefixo antigo em `:100`. Casos negativos/positivos em `bin/tests/test_placar_grade.sh:143`, `:146`, `:149`; mordida em `:164`, `:167`, `:172`. |
| Soma por estado | **PROCEDE a preservação.** Soma valores declarados, sem deduplicar BOs. Correspondência com BOs reais: **[nao verificado]**. | Cálculo em `bin/placar_grade.py:151`, `:155`; assert exato em `bin/tests/test_placar_grade.sh:119`, `:120`, `:177`. |
| Complemento não aumenta obras | **PROCEDE a preservação.** | Separação em `bin/placar_grade.py:120`; participação na soma em `:151`; `(compl.)` em `:270`. Asserts em `bin/tests/test_placar_grade.sh:116`, `:124`; mutante em `:184`, `:186`. |
| `?` no denominador, zero como medida | **PROCEDE a preservação**, agora exigindo fonte também no zero. | `bin/placar_grade.py:56`, `:112`, `:154`, `:156`; casos/asserts em `bin/tests/test_placar_grade.sh:94`, `:96`, `:119`, `:125`. |
| Asserts antigos têm mordida | **NAO PROCEDE regressão demonstrada.** Permanecem verificações específicas para `abc`, largura, estado, complemento e apresentação. | `bin/tests/test_placar_grade.sh:124`, `:125`, `:173`, `:174`, `:175`, `:176`, `:186`. |
| `resolve` excluía poucos marcadores; vazio sem caso | **Fechado. NAO PROCEDE a persistência dessas lacunas.** | Exclusão dos cinco marcadores, sem distinguir caixa, em `bin/placar_grade.py:44`, `:127`; fixture inclui vazio, `n/a`, `N/A` e `[nao verificado]` em `bin/tests/test_placar_grade.sh:193`, `:196`, `:197`, `:198`; assert `1/7` em `:202`. |

Reproduzi em memória os casos principais e os mutantes de número nu, fonte só com espaços, portões nus, vazio, `n/a`, `[nao verificado]` e caixa: alteram os resultados fixados pelos asserts (`bin/tests/test_placar_grade.sh:161`, `:162`, `:164`, `:167`, `:202`). Execução integral do shell: **[nao verificado]**; ele cria e remove temporários (`bin/tests/test_placar_grade.sh:24`).

**2. As três células de conteúdo**

| Célula | Fontes disponíveis e limite | Veredito |
|---|---|---|
| **E3** | E3 em obra e a worktree têm apoio em `app/docs/RELATO.md:11`, `:13`. A regra distingue ramo ao lado da espinha (`app/docs/GRADE-MESTRA.md:8`). O hash `f569063be` e sua situação atual: **[nao verificado]**; a exclusão específica está afirmada na própria célula (`:119`). | **NAO PROCEDE** a persistência literal da autocolisão: foi removida. **PROCEDE** campo com dois significados: `livre` acompanhado de “fora da regra” (`app/docs/GRADE-MESTRA.md:119`). |
| **DECLARADO-X-GRAVADO** | `materializacao.py` existe e contém `_ts_tipo_do_plantio` (`app/chamados/services/materializacao.py:333`). O commit BUG-PAR `338fd37b0` aparece no RELATO (`app/docs/RELATO.md:15`). Porém o corte das cinco leis às 10:24, citado na célula, depende de `logs/AVAIS-1313/dxg_leis_cortadas.txt`: conteúdo **[nao verificado]** (`app/docs/GRADE-MESTRA.md:272`). As fontes consultáveis ainda registram cinco leis e retirada da ordem (`app/docs/RELATO.md:7`, `:23`, `:24`). Interseção dos diffs citados: **[nao verificado]**. | **PROCEDE**, sem fechamento demonstrado. O delta reconhece o impedimento anterior, mas sustenta sua remoção numa fonte não disponível (`app/docs/GRADE-MESTRA.md:272`). |
| **O232** | A relação de O232 com `turnos.py` existe (`app/ponto/turnos.py:913`, `:944`). JUSTIFICATIVA voltou à ordem viva (`app/docs/RELATO.md:155`), mas a GRADE continua INTERROMPIDA (`app/docs/GRADE-MESTRA.md:269`). A worktree, o hash e o `git status` novos são alegados na própria célula; prova de alteração concorrente e agente vivo: **[nao verificado]** (`:853`). | **PROCEDE** a falta de sustentação da colisão atual. Identificação mais detalhada não fecha a prova (`app/docs/GRADE-MESTRA.md:853`). |

Para esses três conteúdos, **não há caso específico que morda a veracidade da correção**: a verificação da GRADE real chama apenas `--conferir`, que valida leitura e cabeçalho derivado (`bin/tests/test_placar_grade.sh:205`; `bin/placar_grade.py:222`, `:227`).

**E3 entra na tabela oferecida à regra, mas sua seleção efetiva como vaga é [nao verificado].** O parser aceita o prefixo `livre` com texto, preservando a célula inteira; não transforma a ressalva em exclusão (`bin/placar_grade.py:66`, `:96`, `:118`). O PLACAR publica toda linha medida, sem filtro por espinha, estado ou ressalva — portanto publica E3 com `2`, esse portão e `EM VOO` (`:268`, `:270`; `app/docs/GRADE-MESTRA.md:119`).

A tabela é declarada fonte da regra (`bin/placar_grade.py:23`), mas a regra exige ramo, censo fechado e ausência de arquivo comum com agente vivo (`app/docs/GRADE-MESTRA.md:8`). **NAO PROCEDE afirmar que o código apresentado abriu efetivamente uma vaga para E3**; decisão humana ou consumidor externo: **[nao verificado]**.

**3. Consumidores e números**

| Consumidor fora de `placar_grade.py` | Leitura relevante |
|---|---|
| Publicador | Invoca o gerador e transporta GRADE/PLACAR inteiros; não calcula as três colunas (`bin/relato.sh:101`, `:119`, `:120`). |
| Raspador | Lê, mascara e grava texto integral (`bin/raspar_publico.py:28`, `:31`, `:40`). |
| Selo | Confere saídas de soma, portões e `resolve` (`bin/tests/test_placar_grade.sh:120`, `:164`, `:167`, `:202`). |
| Régua | Executa os selos pelo loop; não interpreta essas colunas (`bin/regua.sh:138`, `:140`). |

**NAO PROCEDE mudança de número nos consumidores identificados.** Aplicando em memória as regras anteriores do delta e as atuais à cópia, permanecem **456 obras, zero rejeitos, `11/456`, seis medidas entre 297 linhas e soma por estado `2; 1; 9; 0; 0; 0`** — os valores registrados no cabeçalho (`app/docs/GRADE-MESTRA.md:22`, `:24`, `:26`, `:28`; cálculo em `bin/placar_grade.py:127`, `:151`, `:155`). As seis medidas já têm fonte textual entre parênteses (`app/docs/GRADE-MESTRA.md:108`, `:119`, `:137`, `:272`, `:687`, `:853`).

**PROCEDE a preservação da independência de `--conferir` em relação a logs**: deriva o cabeçalho da GRADE; `sem_obra()` entra apenas no PLACAR (`bin/placar_grade.py:215`, `:227`, `:266`). Outros consumidores externos: **[nao verificado]**.

**4. Classes**

| Classe | Veredito |
|---|---|
| **Selo sem mordida** | **NAO PROCEDE** para os defeitos sintáticos e marcadores corrigidos. **PROCEDE para conteúdo**: trocar qualquer um dos três portões por `colisao com OBRA-INEXISTENTE (arquivo-inexistente)` mantém `--conferir` verde; aceita texto após o prefixo e não confere referências (`bin/placar_grade.py:66`, `:227`; selo vivo em `bin/tests/test_placar_grade.sh:205`). |
| **Número sem fonte** | **NAO PROCEDE número nu aceito**: fechado em `bin/placar_grade.py:58`, com mordida em `bin/tests/test_placar_grade.sh:164`. **PROCEDE ausência de verificação da fonte**: `N (texto)` basta; existência e sustentação dos valores reais: **[nao verificado]** (`bin/placar_grade.py:58`, `:59`). |
| **Campo com dois significados** | **PROCEDE na E3**: `livre` e “fora da regra” coexistem, sem exclusão executável (`app/docs/GRADE-MESTRA.md:119`; `bin/placar_grade.py:66`, `:268`). **NAO PROCEDE dupla semântica demonstrada em `resolve`**: mede células preenchidas, não quantidade de casos ou referências distintas (`bin/placar_grade.py:127`, `:128`). |

**PROCEDE ainda a limitação semântica de “identificação”:** `lei ?` e `colisao com ?` passam, pois qualquer sufixo não vazio satisfaz a guarda; os casos novos cobrem prefixos nus e exemplos nomeados, sem validar a identidade indicada (`bin/placar_grade.py:66`; `bin/tests/test_placar_grade.sh:146`, `:148`, `:149`, `:151`).

## 14/11 10:56 · sessao e404bd26221d · - · - · origem=Ronald (tmux)

**NAO PROCEDE mudança de número nos consumidores identificados.** A comparação em memória, aplicando as regras antigas e novas à cópia, manteve 456 obras, zero rejeitos, `resolve preenchido 11/456` e a soma `EM VOO 2; BRIEF PRONTO 1; REGISTRADA 9; INTERROMPIDA 0; PARADA 0; NO AR hoje 0; linhas medidas 6 de 297`. São os valores registrados no cabeçalho; as seis medidas estão nas linhas 108, 119, 137, 272, 687 e 853, todas com fonte entre parênteses e portão aceito pelas duas versões (`app/docs/GRADE-MESTRA.md:22`, `:24`, `:26`, `:28`, `:108`, `:119`, `:137`, `:272`, `:687`, `:853`; cálculo em `bin/placar_grade.py:118`, `:127`, `:151`).

| Consumidor fora do parser | Leitura efetiva |
|---|---|
| `bin/relato.sh` | Invoca `--placar`; publica GRADE e PLACAR inteiros. Não interpreta as três colunas (`bin/relato.sh:101`, `:119`, `:120`). |
| `bin/raspar_publico.py` | Lê o documento inteiro, mascara e grava texto. Não calcula os indicadores (`bin/raspar_publico.py:28`, `:31`, `:40`). |
| Selo | Interpreta as saídas esperadas de `fecha BO`, portão e `resolve`; confere GRADE e ligação com publicador (`bin/tests/test_placar_grade.sh:119`, `:121`, `:126`, `:164`, `:167`, `:177`, `:202`, `:205`, `:211`). |
| Régua | Executa o selo por varredura de `test_*.sh`, sem ler as colunas (`bin/regua.sh:138`, `:140`). |
| `hook_stop_fila1` | Seus documentos de entrada são BACKLOG e RELATO (`bin/hook_stop_fila1.py:33`, `:34`). |
| Pre-push | O loop dos selos da régua não é executado diretamente por ele, conforme comentário explícito (`bin/pre-push.sh:14`, `:15`). |

**E3: entra na tabela oferecida à regra, mas seleção efetiva como vaga é [nao verificado].** `_portao_ok()` aceita `livre (...)` pelo prefixo e preserva o texto literal. `placar()` inclui todas as linhas medidas, sem excluir espinha, `EM VOO` ou a ressalva escrita nos parênteses; logo publica E3 com `fecha BO 2` e esse portão (`bin/placar_grade.py:66`, `:118`, `:268`, `:270`; `app/docs/GRADE-MESTRA.md:119`).

A regra documental exige **ramo**, censo fechado e ausência de arquivo comum com raia de agente vivo; a mesma célula declara que E3 é espinha e está fora da regra (`app/docs/GRADE-MESTRA.md:8`, `:119`). Não há seletor automático de vaga identificado na cópia; consumidores externos ou decisões humanas: **[nao verificado]**.

Assim, **PROCEDE a ambiguidade de representação para quem tratar qualquer `livre (...)` como elegível**; **NAO PROCEDE afirmar que o código demonstrado abriu efetivamente vaga para E3**, porque o código só publica a tabela e a decisão exige condições adicionais (`bin/placar_grade.py:268`, `:270`; `app/docs/GRADE-MESTRA.md:8`).

## 14/11 10:56 · sessao 873f9314a28e · - · - · origem=Ronald (tmux)

| Célula | Veredito sobre a cura | Sustentação na cópia |
|---|---|---|
| E3 | **NAO PROCEDE** a persistência literal da autocolisão: ela foi removida. **PROCEDE** a ambiguidade residual: `livre` mais uma exceção em prosa. | O novo texto está em `app/docs/GRADE-MESTRA.md:119`. E3 em obra/raia tem apoio em `app/docs/BACKLOG.md:1902` e `app/docs/RELATO.md:7`, `:11`, `:13`, `:20`. A regra distingue ramo ao lado da espinha em `app/docs/GRADE-MESTRA.md:8`; a exclusão específica desta linha só está na própria célula (`:119`). Hash `f569063be` e situação atual dessa raia: **[nao verificado]**. |
| DECLARADO-X-GRAVADO | **PROCEDE**, não fechado pela evidência disponível: trocou a omissão do impedimento por uma declaração de corte sem fonte consultável. | `app/docs/GRADE-MESTRA.md:272` afirma corte das cinco leis às 10:24 e cita `logs/AVAIS-1313/dxg_leis_cortadas.txt`; conteúdo desse arquivo: **[nao verificado]**. As fontes disponíveis mantêm as cinco abertas e retiram DXG da ordem até o corte (`app/docs/RELATO.md:7`, `:23`, `:24`). A raia BUG-PAR e o commit `338fd37b0` têm apoio em `app/docs/RELATO.md:15`, `:20`; interseção dos diffs citados com E3: **[nao verificado]**. |
| O232 | **PROCEDE** a falta de sustentação da colisão atual. A identificação ficou mais precisa; a prova não veio junto. | `app/docs/GRADE-MESTRA.md:853` cita arquivo, raia, hash e `git status`, mas esses dados de trabalho não estão disponíveis: **[nao verificado]**. A própria GRADE conserva JUSTIFICATIVA como INTERROMPIDA (`app/docs/GRADE-MESTRA.md:269`). BACKLOG e RELATO registram retorno à ordem viva, sem demonstrar agente vivo agora (`app/docs/BACKLOG.md:1875`, `app/docs/RELATO.md:155`). A relação de O232 com `ponto/turnos.py` é sustentada por `app/docs/BACKLOG.md:1196`. |

O parser **aceita** `livre (...)` porque `_portao_ok()` permite qualquer prefixo de `PORTOES` seguido de texto, inclusive `livre` (`bin/placar_grade.py:62`, `:66`). Ele preserva o texto integral, sem extrair ou aplicar a exceção “fora da REGRA” (`bin/placar_grade.py:96`, `:118`). E3 entra na tabela numérica do PLACAR com `2`, portão literal e estado EM VOO, pois a seleção exige apenas coluna e medida numérica (`bin/placar_grade.py:267`, `:268`, `:270`).

Essa tabela é declarada como fonte da REGRA DA VAGA VAZIA (`bin/placar_grade.py:22`, `:23`), mas a regra exige **ramo**, censo fechado e ausência de arquivo comum com agente vivo (`app/docs/GRADE-MESTRA.md:8`). Portanto há **campo com dois significados — PROCEDE**: prefixo `livre` e exceção em prosa (`app/docs/GRADE-MESTRA.md:119`). Seleção automática efetiva de E3 como candidata em outro consumidor: **[nao verificado]**.

O232 é complemento sob O54, não obra do denominador (`app/docs/GRADE-MESTRA.md:838`, `:839`, `:853`; `bin/placar_grade.py:120`). Ele participa da tabela numérica e da soma, recebendo `(compl.)` (`bin/placar_grade.py:149`, `:151`, `:268`, `:270`).

_5120 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
