# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 11:49** · publicado em **14/11/2026 11:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 14/11 10:48 · sessao 3217cb06c88f · FRAC5 · verificador · origem=Code

**NÃO: o diff acrescenta a recusa de frações de 1–4 dias no `clean()`, mas não garante a regra completa em todos os caminhos de gravação.** A edição continua salvando sem esse julgamento; o histórico também o dispensa; e a própria porta aceita um período que já contenha fração curta. (`app/ferias/models.py:387`; `app/ferias/services.py:493`; `app/ferias/services.py:693`; `app/ponto/tests/test_ferias_fracao_demais_5.py:83`)

A comparação com a base abaixo é contrafactual pelo diff fornecido. Executei somente o juiz extraído em memória; execução da suíte Django e execução real contra `f7510b95a`: **[nao verificado]**. Ocorrência desses casos em produção: **[nao verificado]**.

1. **Quem escreve e quem lê**

| Campo/valor | Quem escreve ou monta | Quem lê |
|---|---|---|
| `AgendamentoFerias.dias` | Campo inteiro; criação normal, histórico, edição e aprovação escrevem o valor. (`app/ferias/models.py:320`; `app/ferias/services.py:544`; `app/ferias/services.py:487`; `app/ferias/services.py:680`; `app/ferias/services.py:743`) | Porta carrega os dias das outras frações; aviso carrega os dias gravados. (`app/ferias/models.py:372`; `app/ferias/models.py:378`; `app/ferias/situacao.py:122`) |
| `dias_self` | Variável local calculada por `(data_fim-data_inicio).days+1`. (`app/ferias/models.py:377`) | Entra no conjunto para a exigência de uma fração ≥14; entra sozinho para o mínimo de 5. (`app/ferias/models.py:379`; `app/ferias/models.py:387`) |
| Frações do período | `clean()` consulta agendado/em_gozo/concluido e exclui o próprio PK. O aviso repete essa seleção, com exclusão opcional. (`app/ferias/models.py:368`; `app/ferias/models.py:371`; `app/ferias/situacao.py:115`) | O juiz puro recebe os números; a porta seleciona violações; o aviso apresenta os resultados. (`app/ferias/models.py:265`; `app/ferias/models.py:372`; `app/ferias/models.py:380`; `app/ferias/situacao.py:227`) |
| `FRACAO_MINIMA_DEMAIS=5` | Definição única encontrada. (`app/ferias/models.py:253`) | Comparação no juiz; interpolação dos dois avisos. (`app/ferias/models.py:277`; `app/ferias/situacao.py:249`; `app/ferias/situacao.py:359`) |
| Limites 14 e 3 | Constantes existentes. (`app/ferias/models.py:251`; `app/ferias/models.py:252`) | Comparações no mesmo juiz. (`app/ferias/models.py:273`; `app/ferias/models.py:275`) |

**PROCEDE — o conjunto julgado muda conforme a cláusula.** O caput recebe outras frações mais um marcador `[1]`; a exigência de 14 recebe outras mais a duração atual; o mínimo de 5 recebe somente a duração atual. Portanto, chamar o mesmo juiz não significa aplicar o mesmo julgamento ao período inteiro. (`app/ferias/models.py:372`; `app/ferias/models.py:379`; `app/ferias/models.py:387`)

**PROCEDE — histórico curto permanece aceito expressamente.** `[14,3]` mais uma nova fração de 13 passa pela porta, conforme o próprio teste. O período resultante continua contendo uma fração abaixo de 5. Isso limita a promessa de defesa integral feita no mapa do modelo. (`app/ponto/tests/test_ferias_fracao_demais_5.py:79`; `app/ponto/tests/test_ferias_fracao_demais_5.py:83`; `app/ferias/models.py:15`)

O grep de leitores foi:

```bash
rg -n \
'violacoes_de_fracionamento|FRACAO_MINIMA|FRACOES_MAXIMAS|checar_fracionamento|avisos_de_agendamento|fracionamento|frações|fracoes|menor.*14' \
app --glob '*.py' --glob '*.html' --glob '*.js' \
--glob '!**/migrations/**'
```

Os leitores relevantes encontrados foram:

| Leitor | Resultado |
|---|---|
| Views de criação | Delegam a `checar_fracionamento`, que instancia e chama `clean()`. **NÃO PROCEDE** outro comparador próprio nesse caminho. (`app/ferias/views.py:258`; `app/ferias/services.py:293`) |
| Preview com abono/intake | Delegam à validação existente. (`app/ferias/services.py:309`; `app/core/intake.py:34`; `app/core/intake.py:35`) |
| Aviso do período e da proposta | Chamam o juiz puro. (`app/ferias/situacao.py:227`; `app/ferias/situacao.py:348`) |
| API de mensageria | Delega à situação e aos avisos. (`app/api/views_mensageria.py:791`; `app/api/views_mensageria.py:812`; `app/api/views_mensageria.py:824`) |
| Comando de simulação | Usa `_validar_agendamento`. (`app/ferias/management/commands/listar_ferias_dp.py:107`) |
| Tela principal | Texto corrigido para 3/14/5. (`app/templates/ferias/agendar.html:10`) |
| Formulário inline | **PROCEDE** texto incompleto: continua anunciando apenas 3/14. (`app/templates/ferias/_secao_colab.html:63`) |

**NÃO PROCEDE — segundo comparador numérico de fracionamento encontrado em views/forms/services/crons/API/JS.** Os caminhos encontrados acima delegam; ausência absoluta de variantes dinâmicas ou não reconhecidas pelo grep: **[nao verificado]**. O `min="1"` da interface não constitui sozinho outro juiz, pois a criação delega ao servidor. (`app/templates/ferias/_secao_colab.html:68`; `app/ferias/views.py:259`)

**PROCEDE — aviso incompleto quando duas violações coexistem.** Para `[5,5,5,5]`, o juiz retorna `par1` e `caput`, mas o aviso usa `if par1 / elif caput` e omite o caput. É comportamento preexistente que permanece. (`app/ferias/models.py:273`; `app/ferias/models.py:275`; `app/ferias/situacao.py:228`; `app/ferias/situacao.py:237`)

**PROCEDE — zero desaparece do julgamento.** O filtro `if f` elimina zero antes do mínimo e da contagem: `[14,0] → []` e `[14,5,5,0] → []`. Não demonstra uma gravação real com zero, mas demonstra que o juiz isolado não expressa literalmente “nenhuma fração <5” para toda entrada inteira. (`app/ferias/models.py:271`; `app/ferias/models.py:275`; `app/ferias/models.py:277`)

2. **Edição, cancelamento, status e caminhos de gravação**

**NÃO PROCEDE — `clean()` só funciona para objeto novo.** Ele exclui o próprio PK e julgaria corretamente uma edição explícita para quatro dias, havendo outra fração de 14. O furo está nos chamadores que não executam `clean()`. (`app/ferias/models.py:371`; `app/ferias/models.py:377`; `app/ferias/models.py:387`)

| Caminho | Julga fracionamento antes de gravar? |
|---|---|
| Criação normal/individual/lote | Sim: trava o período, valida e depois cria. (`app/ferias/services.py:531`; `app/ferias/services.py:541`; `app/ferias/services.py:544`) |
| Aprovação de solicitação | Sim: `full_clean()` antes de `save()`. (`app/ferias/services.py:749`; `app/ferias/services.py:750`) |
| Edição regular | **PROCEDE o bypass:** aceita duração ≥1 e chama remarcação; o serviço troca dias/datas e salva sem `clean()`. (`app/ferias/views.py:712`; `app/ferias/views.py:722`; `app/ferias/services.py:680`; `app/ferias/services.py:693`) |
| Correção administrativa | **PROCEDE o bypass, documentado:** dispensa `full_clean()` e usa o mesmo escritor da edição. (`app/ferias/views.py:508`; `app/ferias/views.py:531`; `app/ferias/services.py:702`) |
| Gozo histórico | **PROCEDE o bypass:** valida datas/saldo/abono, mas dispensa fracionamento e salva. Pode admitir `<5`, ausência de 14 ou quarta fração, se as demais guardas permitirem. (`app/ferias/services.py:492`; `app/ferias/services.py:507`; `app/ferias/services.py:510`) |
| Cancelamentos | Não reavaliam as frações restantes; alteram saldo/status. (`app/ferias/services.py:616`; `app/ferias/services.py:620`; `app/ferias/services.py:643`; `app/ferias/services.py:652`) |
| Cron de status | Salva somente status, sem `clean()`. (`app/ferias/services.py:916`; `app/ferias/services.py:920`) |

**PROCEDE — cancelamento pode remover a única fração ≥14.** Cancelar o 14 de `[14,5]` deixa `[5]` no universo considerado pelo juiz; o cancelamento não consulta esse resultado. (`app/ferias/services.py:619`; `app/ferias/services.py:620`; `app/ferias/models.py:370`)

**NÃO PROCEDE — remarcação oficial ressuscita cancelado.** Ela relê o status sob trava e recusa `cancelado`. Reagendar pela criação produz outra linha e desconsidera cancelados na duplicidade. (`app/ferias/services.py:599`; `app/ferias/services.py:600`; `app/ferias/services.py:670`; `app/ferias/models.py:313`; `app/ferias/services.py:534`; `app/ferias/services.py:544`)

**PROCEDE — o status da própria instância não é filtrado pelo `clean()`.** As outras linhas são filtradas, mas `self` entra incondicionalmente. Assim, chamar `clean()` num cancelado também o conta/julga; os serviços de cancelamento normalmente não fazem essa chamada. (`app/ferias/models.py:370`; `app/ferias/models.py:372`; `app/ferias/models.py:379`; `app/ferias/models.py:387`)

**PROCEDE — `save()` não é a porta obrigatória dessa regra.** Ele executa guarda de sobreposição, persistência e espelho; não chama `clean()`. (`app/ferias/models.py:461`)

**NÃO PROCEDE — segundo escritor de produção fora do módulo registrado, no censo encontrado.** O registro permite `ferias/services.py`, e os escritores materiais acima estão nele. Há vários caminhos dentro dessa porta que pulam o juiz. (`app/core/portas.py:489`; `app/ferias/services.py:510`; `app/ferias/services.py:693`)

Grep complementar de escritores:

```bash
rg -n \
'AgendamentoFerias|_AgFer|ferias_agendamentoferias|agendamentos\.(create|update|bulk_create)' \
. --glob '*.py' --glob '!**/test*' --glob '!**/migrations/**'
```

Não apareceu `bulk_create`/`bulk_update`/`QuerySet.update` de produção para esse modelo nesse censo; mecanismos externos ou dinâmicos: **[nao verificado]**. O admin local registra somente `Afastamento`; o teste novo usa `bulk_create` deliberadamente para preparar histórico. (`app/ferias/admin.py:2`; `app/ferias/admin.py:4`; `app/ponto/tests/test_ferias_fracao_demais_5.py:69`; `app/ponto/tests/test_ferias_fracao_demais_5.py:82`)

**PROCEDE — mensagem nova contradiz um caminho real.** A correção admin coleta “este agendamento será recusado”, grava a fração curta e retorna sucesso anexando esse aviso. (`app/ferias/situacao.py:359`; `app/ferias/views.py:517`; `app/ferias/views.py:531`; `app/ferias/views.py:549`; `app/ferias/views.py:553`)

3. **Unidade e abono**

**NÃO PROCEDE — unidade diferente em `dias_self`.** Início e fim são `DateField`; a diferença mais um conta dias civis corridos inclusivos. Não há conversão por turno, escala ou fuso nessa expressão. (`app/ferias/models.py:318`; `app/ferias/models.py:319`; `app/ferias/models.py:377`)

**PROCEDE — a equivalência com `dias` depende do caminho.** `clean()` exige `dias == intervalo inclusivo`; `save()` não exige. O serviço de edição recebe datas e quantidade separadamente, embora as views atuais calculem o fim coerentemente. Divergência efetivamente gravada em produção: **[nao verificado]**. (`app/ferias/models.py:395`; `app/ferias/models.py:461`; `app/ferias/services.py:657`; `app/ferias/services.py:680`; `app/ferias/views.py:718`)

**NÃO PROCEDE — abono aumenta a duração da fração.** O gozo escreve `dias`; a venda incrementa separadamente `PeriodoAquisitivo.dias_vendidos`. O saldo desconta ambos, mas o fim é calculado somente pelos dias de gozo. Portanto, 20 gozados + 10 vendidos constituem fração de 20 nesse código. (`app/ferias/services.py:269`; `app/ferias/services.py:277`; `app/ferias/models.py:220`; `app/ferias/services.py:319`)

4. **Cada asserção morde a base?**

Todas as referências desta tabela são de `app/ponto/tests/test_ferias_fracao_demais_5.py`. “Sim” significa falha contrafactual pelo diff; execução real na base: **[nao verificado]**.

| Linha / asserção | Falharia? | Motivo |
|---|---|---|
| `:20` — `[14,4]` | Sim | Base retornaria `[]`. |
| `:21` — `[25,1]` | Sim | Base retornaria `[]`. |
| `:22` — `[4]` | Sim | Base retornaria somente `['par1']`. |
| `:26` — `[14,5]` | Não | Já retornava `[]`. |
| `:27` — `[20,5,5]` | Não | Já retornava `[]`. |
| `:28` — `[30]` | Não | Já retornava `[]`. |
| `:32` — `[10,10]` | Não | Já retornava `['par1']`. |
| `:33` — `[14,5,5,5]` | Não | Já retornava `['caput']`. |
| `:34` — `[14,3,5,5]` | Sim | Base retornaria somente `['caput']`. |
| `:41` — ausência da comparação literal | Sim | A comparação existia no texto-base. |
| `:42` — presença da chamada ao juiz | Sim | A chamada não existia nessa função-base. |
| `:70`/`:71` — `assertRaises` para quatro dias | Sim | A base aceitaria. |
| `:72` — mensagem contém `5 dias` | Sim, isoladamente | Base não produz essa mensagem; a execução já falharia no `assertRaises`. |
| `:77` — aceita cinco, implicitamente | Não | A base também aceitava. |
| `:83` — aceita treze ao lado de `[14,3]` | Não | A base também aceitava. |
| `:91` — aviso do período contém `5 dias` | Sim | Aviso novo não existia. |
| `:97` — aviso da proposta contém `5 dias` | Sim | Aviso novo não existia. |
| `:106` — remove texto antigo | Sim | Base continha a expressão. |
| `:107` — contém “ao menos uma…” | Sim, isoladamente | Base não continha; normalmente para na `:106`. |
| `:108` — contém “nenhuma…” | Sim, isoladamente | Mesmo motivo. |

**NÃO PROCEDE — conjunto inteiro sem mordida.** Há testes comportamentais que distinguem a cura da base. **PROCEDE — selo estrutural fraco:** buscar substrings não prova exclusividade nem uso efetivo do juiz. (`app/ponto/tests/test_ferias_fracao_demais_5.py:20`; `app/ponto/tests/test_ferias_fracao_demais_5.py:70`; `app/ponto/tests/test_ferias_fracao_demais_5.py:91`; `app/ponto/tests/test_ferias_fracao_demais_5.py:40`)

**PROCEDE — lacunas materiais:** os testes novos não exercitam edição pelo serviço, cancelamento, `save()` como entrada, zero ou aviso simultâneo `par1+caput`; o helper sempre fornece dias/datas coerentes e usa concluído por padrão. (`app/ponto/tests/test_ferias_fracao_demais_5.py:61`; `app/ponto/tests/test_ferias_fracao_demais_5.py:64`; `app/ponto/tests/test_ferias_fracao_demais_5.py:71`; `app/ponto/tests/test_ferias_fracao_demais_5.py:83`; `app/ponto/tests/test_ferias_fracao_demais_5.py:95`)

5. **Classes de reprovação pedidas**

| Classe | Veredito e motivo |
|---|---|
| Segundo juiz duplicando o registrado | **NÃO PROCEDE como introdução deste diff.** O puro já existia e a porta delega. O endereço vivo continua sendo `AgendamentoFerias.clean`, não o puro; o lookup manda consultar esse registro. (`app/ferias/models.py:266`; `app/ferias/models.py:380`; `app/core/juizes.py:271`; `app/docs/JUIZES.md:14`) |
| Segundo escritor fora da porta | **NÃO PROCEDE no censo de produção; PROCEDE gravação sem julgamento dentro da porta.** (`app/core/portas.py:489`; `app/ferias/services.py:693`) |
| Relógio fora da borda | **PROCEDE na árvore preexistente:** avisos têm fallback `date.today()` e cancelamento admin lê a data internamente. **NÃO PROCEDE no juiz novo**, que só recebe números. (`app/ferias/situacao.py:156`; `app/ferias/situacao.py:326`; `app/ferias/services.py:649`; `app/ferias/models.py:265`) |
| Derivação paralela da fonte única | **PROCEDE na montagem duplicada do universo**, sem divergência atual de status: porta e aviso repetem a consulta. **NÃO PROCEDE outro cálculo numérico independente encontrado.** (`app/ferias/models.py:368`; `app/ferias/situacao.py:117`; `app/ferias/situacao.py:227`) |
| Selo/RED sem mordida | **NÃO PROCEDE para o conjunto; PROCEDE a limitação dos testes por substring e a falta da edição real.** (`app/ponto/tests/test_ferias_fracao_demais_5.py:20`; `app/ponto/tests/test_ferias_fracao_demais_5.py:41`; `app/ponto/tests/test_ferias_fracao_demais_5.py:71`) |
| Ordem de trava | **NÃO PROCEDE alteração de ordem pelo diff.** Criação trava PA antes de validar; edição trava AG → PA antes dos efeitos. Deadlock real: **[nao verificado]**. (`app/ferias/services.py:531`; `app/ferias/services.py:541`; `app/ferias/services.py:599`; `app/ferias/services.py:602`; `app/ferias/services.py:688`) |
| Pouso antes do pré-requisito | **NÃO PROCEDE como achado demonstrado.** Sequência efetiva de pouso/deploy e pré-requisitos cumpridos: **[nao verificado]**. | [nao verificado]
| Campo com dois significados | **NÃO PROCEDE confusão de unidade demonstrada; PROCEDE falta de imposição universal da equivalência dias/datas.** Abono permanece separado. (`app/ferias/models.py:377`; `app/ferias/models.py:395`; `app/ferias/models.py:461`; `app/ferias/services.py:277`) |

## 14/11 10:48 · sessao 149f75f30723 · [nome]-ANDAM-JUNTO · verificador · origem=Code

**O delta fecha parte dos problemas do leitor e das provas do selo; os três achados de conteúdo continuam abertos.** A validação semântica também permanece incompleta: aceita portões sem identificação e `fecha BO` sem fonte (`bin/placar_grade.py:55`, `bin/placar_grade.py:60`, `app/docs/GRADE-MESTRA.md:119`, `app/docs/GRADE-MESTRA.md:272`, `app/docs/GRADE-MESTRA.md:853`).

**1. Volta 1: cura e caso que morde**

| Achado | Veredito na volta 2 | Cura/preservação e selo |
|---|---|---|
| Tabelas de oito/dez colunas sem deslocamento | **PROCEDE, preservado.** Esquema vem do cabeçalho; largura incompatível é rejeitada. | Leitor: `bin/placar_grade.py:76`, `bin/placar_grade.py:87`. Caso novo `RJ-LARGA`, com nove células: `bin/tests/test_placar_grade.sh:138`; rejeição exigida em `:150`. |
| Células semanticamente inválidas entram | **PROCEDE, parcialmente fechado.** `REGISTRADAxyz` foi corrigido nas dez colunas; nas oito continua reconhecimento por prefixo. `lei`, `arquivo congelado`, `colisao com` sem identificação e número sem fonte continuam aceitos. | Cura: `bin/placar_grade.py:97`; caso que morde: `bin/tests/test_placar_grade.sh:151`. Pendências: `bin/placar_grade.py:94`, `bin/placar_grade.py:55`, `bin/placar_grade.py:60`. |
| Soma dos valores informados por estado | **PROCEDE, preservado.** Soma declarações; não demonstra BO distintos nem ausência de sobreposição. | Soma: `bin/placar_grade.py:145`, `bin/placar_grade.py:149`; assert exato: `bin/tests/test_placar_grade.sh:117`. Correspondência com BO reais: **[nao verificado]**. |
| Complemento não aumenta obras | **PROCEDE, preservado.** Complementos ficam separados, participam da soma e recebem `(compl.)`. | Separação: `bin/placar_grade.py:114`; soma: `:145`; apresentação: `:264`. Mordida: total em `bin/tests/test_placar_grade.sh:113` e linha completa em `:121`. |
| `?` entra no denominador, não nas medidas | **PROCEDE, preservado.** Zero também é medido. | `bin/placar_grade.py:53`, `bin/placar_grade.py:145`, `bin/placar_grade.py:150`. Soma exige quatro medidas entre seis linhas; zero aparece explicitamente: `bin/tests/test_placar_grade.sh:116`, `:117`, `:122`. |
| Asserts existentes têm mordida | **PROCEDE; lacunas apontadas foram cobertas.** Há agora casos para `abc`, zero, largura, estado inválido, complemento inválido e sufixo. | `bin/tests/test_placar_grade.sh:121`, `:122`, `:149`, `:150`, `:151`, `:152`. O mutante que apaga a medida do complemento continua exigindo vermelho em `:160`, `:161`, `:162`. |
| DECLARADO-X-GRAVADO omite impedimento de lei | **PROCEDE, aberto.** Continua “colisao”, enquanto o RELATO retira a obra até corte de cinco questões. | Sem cura: `app/docs/GRADE-MESTRA.md:272`, `app/docs/RELATO.md:7`, `app/docs/RELATO.md:24`. Nenhum caso específico no selo apresentado: `bin/tests/test_placar_grade.sh:132`, `:165`. |
| E3 registra colisão consigo mesma | **PROCEDE, aberto.** “A propria obra em voo” continua no portão; a regra compara arquivos entre raias. | Sem cura: `app/docs/GRADE-MESTRA.md:119`, `CLAUDE.md:309`, `CLAUDE.md:310`. Nenhuma prova dessa relação no selo: `bin/tests/test_placar_grade.sh:132`, `:165`. |
| O232 aponta JUSTIFICATIVA como colisão em voo | **PROCEDE a falta de sustentação, aberto.** A referência continua; JUSTIFICATIVA está INTERROMPIDA na própria GRADE. | `app/docs/GRADE-MESTRA.md:853`, `app/docs/GRADE-MESTRA.md:269`. Colisão atual com agente vivo: **[nao verificado]**. |

Reproduzi em memória os asserts relevantes e mutantes: rejeitar zero, aceitar `abc`, aceitar `REGISTRADAxyz`, aceitar complemento inválido e retirar `(compl.)` derrubam verificações; retirar a guarda de largura provoca exceção, também detectável pelo selo (`bin/tests/test_placar_grade.sh:112`, `:121`, `:122`, `:146`, `:149`, `:151`, `:152`). Execução integral do shell: **[nao verificado]**; ele cria e remove temporários (`bin/tests/test_placar_grade.sh:21`, `:26`).

**2. Escritores, leitores e significado dos números**

| Objeto | Quem escreve | Quem lê |
|---|---|---|
| Coluna `resolve` | Escritor efetivo: **[nao verificado]**. A GRADE atribui origem à leitura Codex e determina manutenção por marco (`app/docs/GRADE-MESTRA.md:4`, `:5`). | `ler()` monta o campo pelo cabeçalho; `resolve_preenchido()` conta sua presença (`bin/placar_grade.py:39`, `:90`, `:121`). |
| `logs/GRADE-MESTRA/resolve/quadro.md` | Escritor de produção: **[nao verificado]**. O selo escreve apenas quadros de fixture (`bin/tests/test_placar_grade.sh:125`, `:128`). | `sem_obra()` escolhe caminho explícito/env/padrão, lê o arquivo e extrai `sem obra: N` (`bin/placar_grade.py:132`, `:134`, `:137`). |
| Linhas derivadas | `--cabecalho` escreve o bloco da GRADE; `--placar` escreve o documento derivado (`bin/placar_grade.py:195`, `:205`, `:260`, `:270`). | `--conferir` compara o bloco completo; o selo verifica as linhas novas (`bin/placar_grade.py:221`, `bin/tests/test_placar_grade.sh:123`, `:127`, `:159`). |

**NAO PROCEDE “456 digitado no cálculo”.** O denominador é `len(lidas)`; complementos ficam separados. A cópia registra `11/456`, e o selo também exige `1/5` na fixture, contrariando uma constante 456 (`bin/placar_grade.py:114`, `:122`, `app/docs/GRADE-MESTRA.md:28`, `bin/tests/test_placar_grade.sh:123`).

**PROCEDE exclusão incompleta de marcadores:** somente vazio, `?` e `-` são excluídos, após remoção dos espaços das extremidades. `n/a` e `[nao verificado]` seriam contados como preenchidos (`bin/placar_grade.py:48`, `:121`). Esses marcadores não aparecem na coluna desta cópia; inflação atual causada por eles: **NAO PROCEDE**, conforme os valores distintos abaixo (`app/docs/GRADE-MESTRA.md:35`, `bin/placar_grade.py:90`).

Valores distintos de `resolve`, incluindo obras e complementos:

| Valor literal | Exemplo |
|---|---|
| `-` | `app/docs/GRADE-MESTRA.md:37` |
| `HJ-074` | `app/docs/GRADE-MESTRA.md:127` |
| `HJ-092` | `app/docs/GRADE-MESTRA.md:128` |
| `casos 01, 02, 05, 06, 08` | `app/docs/GRADE-MESTRA.md:170` |
| `caso 03` | `app/docs/GRADE-MESTRA.md:267` |
| `HJ-090; HJ-091` | `app/docs/GRADE-MESTRA.md:268` |
| `HJ-093` | `app/docs/GRADE-MESTRA.md:269` |
| `HJ-091` | `app/docs/GRADE-MESTRA.md:288` |
| `casos 01–10` | `app/docs/GRADE-MESTRA.md:289` |
| `caso 01–14` | `app/docs/GRADE-MESTRA.md:431` |
| `caso 10` | `app/docs/GRADE-MESTRA.md:505` |
| `caso 13–14` | `app/docs/GRADE-MESTRA.md:686` |
| `HJ-095` | `app/docs/GRADE-MESTRA.md:687` |
| `caso 07` | `app/docs/GRADE-MESTRA.md:705` |
| `caso 09` | `app/docs/GRADE-MESTRA.md:1052` |
| `caso 02,03,05,07,08,09,10,11,12` | `app/docs/GRADE-MESTRA.md:1059` |
| `HJ-090` | `app/docs/GRADE-MESTRA.md:1351` |

O numerador mede **células de obras preenchidas**, sem deduplicar referências nem contar casos individualmente: `HJ-090; HJ-091` vale uma célula; `HJ-092` aparece em duas obras. Complementos preenchidos ficam fora desse indicador (`bin/placar_grade.py:121`, `app/docs/GRADE-MESTRA.md:268`, `:128`, `:469`, `bin/placar_grade.py:114`).

**3. Determinismo e consumidores**

**PROCEDE: `--conferir` continua independente de `logs/`.** Seu caminho lê a GRADE e deriva `cabecalho()`; `sem_obra()` só entra no PLACAR. A conferência da cópia retornou verde com 456 obras, inclusive variando `PLACAR_QUADRO_RESOLVE` (`bin/placar_grade.py:209`, `:218`, `:221`, `:181`, `:260`, `app/docs/GRADE-MESTRA.md:22`, `:24`, `:28`).

O **PLACAR completo** depende agora do quadro/env; já continha horário e commit. Portanto essa independência vale para `--conferir`, não para todo o documento produzido (`bin/placar_grade.py:132`, `:241`, `:260`).

**NAO PROCEDE quebra nos consumidores nomeados:**

| Consumidor | Leitura relevante |
|---|---|
| `relato.sh` | Gera com o leitor atualizado e transporta GRADE/PLACAR inteiros (`bin/relato.sh:101`, `:119`, `:120`). |
| Raspador | Lê texto inteiro, mascara e escreve; não interpreta esses contadores (`bin/raspar_publico.py:28`, `:31`, `:40`). |
| `hook_stop_fila1` | Lê BACKLOG e RELATO (`bin/hook_stop_fila1.py:33`, `:34`, `:186`, `:472`). |
| Pre-push | Lê referências Git, carimbo e labels da régua (`bin/pre-push.sh:8`, `:53`, `:184`; `app/core/hooks/pre-push.sh:22`, `:80`). |

O selo está ligado ao loop da régua; **não é chamado diretamente pelo pre-push**, cujo comentário explicita essa diferença (`bin/regua.sh:138`, `:140`, `:143`, `bin/pre-push.sh:14`, `:15`). Outros consumidores externos: **[nao verificado]**.

**4. Classes pedidas**

| Classe | Veredito |
|---|---|
| **Selo sem mordida** | **NAO PROCEDE** para as lacunas antigas agora cobertas. **PROCEDE uma lacuna residual:** retirar vazio da lista de exclusão de `resolve` passa pelas fixtures, pois elas exercitam referência, `?` e `-`, sem célula vazia (`bin/placar_grade.py:121`, `bin/tests/test_placar_grade.sh:90`, `:92`, `:123`, `:159`). |
| **Número sem fonte** | **PROCEDE em `fecha BO`:** a fonte é opcional no parser, embora exigida na descrição da GRADE; a fixture usa `2` sem fonte (`bin/placar_grade.py:55`, `app/docs/GRADE-MESTRA.md:8`, `bin/tests/test_placar_grade.sh:92`). **NAO PROCEDE como constante inventada em `sem obra`:** lê o quadro ou publica “nao medido”; validade do censo real: **[nao verificado]** (`bin/placar_grade.py:134`, `:137`, `:139`). |
| **Campo com dois significados** | **PROCEDE em `portao` da E3:** “a própria obra está em voo” foi registrado como colisão entre raias (`app/docs/GRADE-MESTRA.md:119`, `CLAUDE.md:309`). **NAO PROCEDE dupla semântica demonstrada em `resolve`:** o indicador implementado mede presença textual; interpretá-lo como quantidade de casos resolvidos não tem sustentação nesse cálculo (`bin/placar_grade.py:119`, `:121`, `app/docs/GRADE-MESTRA.md:268`). |

## 14/11 10:47 · sessao 54e270c3155d · COERENCIA-DAS-LEIS · censo · origem=Code

app/chamados/catalogo/motor.py::VIVOS ; o chamado esta vivo? (app/core/juizes.py:107) ; REGISTRADO-NAO-E-VIVO-DESVIO conserva registrado em VIVOS (app/docs/CORTES.md:328) ; vivo corresponde a aberto, em_analise ou registrado (app/chamados/catalogo/motor.py:24) ; nao ; nenhum achado independente buscando status_local, status_local__in e tuplas aberto/em_analise/registrado, consumidores conferidos usam VIVOS (app/ponto/services/cartorio.py:1384, app/chamados/services/historico.py:180) ; -

app/chamados/catalogo/motor.py::decidir ; pode transitar de um estado a outro? (app/core/juizes.py:108) ; nenhum texto sobre a matriz, endereco documentado em app/docs/JUIZES.md:37 ; nascimento aceita EMITIR/REGISTRAR, transicao humana consulta matriz e perfil, cancelado/superado sao terminais absolutos, eventos do sistema consultam mapa e combinacoes ausentes recusam (app/chamados/catalogo/motor.py:130, app/chamados/catalogo/motor.py:142, app/chamados/catalogo/motor.py:153, app/chamados/catalogo/motor.py:158) ; nao ; nenhum achado independente buscando TRANSICOES_PERMITIDAS, TRANSICAO_HUMANA, pode_transicionar e decidir, modelo deriva a matriz e delega a decisao (app/chamados/models.py:117, app/chamados/models.py:124) ; -

app/chamados/catalogo/premissa.py::premissa_morta ; a premissa do chamado morreu, podendo fechar? (app/core/juizes.py:109) ; L-125 para cluster (app/docs/LEIS.md:146), O169-DECIDIDO-PELA-LEI (app/docs/CORTES.md:430), RESPOSTAS-2910-MANHA aponta a celula para premissa de turno (app/docs/CORTES.md:357) ; True somente para classe reconhecida cujo fato sustentador foi coletado e e falso, classe desconhecida ou fato ausente conservam a premissa viva (app/chamados/catalogo/premissa.py:124, app/chamados/catalogo/premissa.py:128, app/chamados/catalogo/premissa.py:134, app/chamados/catalogo/premissa.py:150) ; nao ; _absolvido_pela_celula responde pela absolvição do chamado de turno usando veredito nao acusador e ausencia de marcos pendentes, autoridade expressamente nomeada pelo corte (app/chamados/reconciliador.py:1019, app/chamados/reconciliador.py:1028, app/chamados/reconciliador.py:1030, app/docs/CORTES.md:357) ; -

app/chamados/utils.py::pode_agir_chamado ; quem pode agir no chamado? (app/core/juizes.py:110) ; nenhum texto especifico em LEIS/CORTES, endereco documentado em app/docs/JUIZES.md:49 ; usa departamento do dono do modulo ou area da solicitacao, recusa anonimo, aceita gestor geral, departamento desconhecido ou usuario sem competencias declaradas, demais precisam da competencia ou perfil correspondente (app/chamados/utils.py:39, app/chamados/utils.py:42, app/chamados/utils.py:55, app/chamados/utils.py:57, app/chamados/utils.py:60, app/chamados/utils.py:65, app/chamados/utils.py:72) ; nao ; nenhum achado contextual independente buscando pode_agir_chamado, departamento_do_chamado, tem_perfil e departamento, consumidores conferidos consultam o juiz (app/chamados/views.py:1015, app/chamados/services/acoes_disputa.py:187) ; -

app/chamados/catalogo/modulos.py::data_do_chamado ; qual o dia do chamado? (app/core/juizes.py:111) ; K1-DIA-DO-CHAMADO declara chave_data=data_turno (app/docs/CORTES.md:177), lookup proibe leitura crua dessa chave (app/docs/JUIZES.md:62) ; devolve o valor da chave_data declarada no catalogo atual ou aposentado, sem modulo ou chave declarada devolve None (app/chamados/catalogo/modulos.py:222, app/chamados/catalogo/modulos.py:225) ; nao ; dias_do_fato determina o dia pela celula e depois por data_turno cru, sem consultar data_do_chamado, o singular tambem acrescenta fallback cru (app/chamados/juizes.py:184, app/chamados/juizes.py:187, app/chamados/juizes.py:118) ; -

app/chamados/juizes.py::pergunta_viva ; a pergunta da disputa esta viva? (app/core/juizes.py:112) ; L-051 (app/docs/LEIS.md:92), L-061 (app/docs/LEIS.md:95), FIO-MORTO-E-ESTRUTURAL (app/docs/CORTES.md:253), leitura da tranca (app/docs/CORTES.md:272), absolvicao pelo marco respondido por batida (app/docs/CORTES.md:413) ; delega ao calculo com memo, validacao, tranca e determinadas vias encerram, resposta preenchida permanece viva salvo decisao administrativa ou concordancia resposta/luz, depois consulta identidade, marco, celula, falta e testemunha da materializacao (app/chamados/juizes.py:894, app/chamados/juizes.py:1401, app/chamados/juizes.py:1405, app/chamados/juizes.py:1412, app/chamados/juizes.py:1417, app/chamados/juizes.py:1429, app/chamados/juizes.py:1438, app/chamados/juizes.py:1444, app/chamados/juizes.py:1450, app/chamados/juizes.py:1462, app/chamados/juizes.py:1476, app/chamados/juizes.py:1483) ; nao ; nenhum achado exato independente buscando pergunta_viva, viva, q_vivas, validada_em, materializada_em e estado, property delega e q_vivas declara somente pre-filtro por contencao (app/chamados/models.py:3392, app/chamados/models.py:3398, app/chamados/models.py:3431) ; -

app/chamados/models.py::DisputaSupervisao.pode_fechar ; a disputa pode fechar? (app/core/juizes.py:113) ; JUIZ-PODE-FECHAR-NASCE exige fio vazio e nenhuma resposta sem veredito (app/docs/CORTES.md:272) ; fechamento ordinario exige simultaneamente respostas_sem_veredito vazia e fio_vivo falso (app/chamados/models.py:1905, app/chamados/models.py:1909) ; nao ; relatorios_disputa.classificar declara PRONTA_FECHAR apenas pela ausencia de perguntas vivas, sem consultar respostas_sem_veredito (app/chamados/relatorios_disputa.py:18, app/chamados/relatorios_disputa.py:20) ; -

app/chamados/models.py::ChamadoColaborador.abrir ; o chamado e duplicado, do mesmo fato? (app/core/juizes.py:114) ; L-007 exige escritor unico e trilha (app/docs/LEIS.md:65), lookup aponta abrir como autoridade do duplicado (app/docs/JUIZES.md:62), nenhum texto especifico da chave em LEIS/CORTES ; sob trava do colaborador busca colaborador mais lookup, reutiliza existente e tenta renascer encerrado, aceita filho agrupado sob pai vivo, senao consulta o motor para nascer (app/chamados/models.py:949, app/chamados/models.py:953, app/chamados/models.py:968, app/chamados/models.py:976, app/chamados/models.py:982) ; nao ; abertura faz dedup proprio por colaborador, modulo, VIVOS, assunto, descricao, prazo e autor, retornando igual ou chamando criar, auditor tambem agrupa duplicatas por colaborador/modulo/dia (app/chamados/services/abertura.py:165, app/chamados/services/abertura.py:170, app/chamados/services/abertura.py:172, app/chamados/management/commands/auditar_invariantes_chamados.py:625, app/chamados/management/commands/auditar_invariantes_chamados.py:626) ; -

app/chamados/catalogo/motor.py::prazo_estourou ; o prazo SLA do chamado estourou? (app/core/juizes.py:115) ; nenhum texto especifico da comparacao em LEIS/CORTES, endereco documentado em app/docs/JUIZES.md:63 ; True quando prazo_sla existe e prazo_sla <= agora, sem prazo retorna False (app/chamados/catalogo/motor.py:56) ; nao ; sla.vencido calcula referencia DP/criacao mais horas por urgencia e compara com <, cobranca._cobranca_vencida usa prazo_sla ou criacao mais 30 minutos e compara com < (app/chamados/services/sla.py:30, app/chamados/services/sla.py:39, app/chamados/services/cobranca.py:253, app/chamados/services/cobranca.py:254) ; -

app/escala/services/regeneracao.py::cobranca_orfa ; a cobranca ficou orfa depois da regeneracao? (app/core/juizes.py:116) ; L-020 sobre reescrita denunciada (app/docs/LEIS.md:71), resposta especifica documentada em app/docs/JUIZES.md:41 ; sem marcos_faltantes ou dia retorna None, senao procura reescrita posterior a criacao, combinando regenerada_em/editada_em e consultando janela historica somente na ausencia de regenerada_em (app/escala/services/regeneracao.py:149, app/escala/services/regeneracao.py:154, app/escala/services/regeneracao.py:119, app/escala/services/regeneracao.py:122, app/escala/services/regeneracao.py:124) ; nao ; nenhum achado independente buscando cobranca_orfa, dia_reescrito_apos, regenerada_em, editada_em e criado_em, consumidores conferidos delegam (app/ponto/management/commands/gerar_celulas.py:257, app/ponto/management/commands/supra_juiz.py:275) ; -

app/chamados/services/silencio_ausencia.py::reconciliar_por_ausencia ; a ausencia do dia suspende, fecha ou levanta a cobranca viva? (app/core/juizes.py:117) ; L-061 (app/docs/LEIS.md:95), L-062 (app/docs/LEIS.md:96), corte S4-PENDENTE-[nome]-COBRANCA transcrito no modulo (app/chamados/services/silencio_ausencia.py:1) ; ausencia aberta silencia ou renova sem sobrepor silencio vivo alheio, aprovada fecha somente com premissa morta e celula nao acusadora, demais casos levantam apenas o marcador proprio (app/chamados/services/silencio_ausencia.py:78, app/chamados/services/silencio_ausencia.py:81, app/chamados/services/silencio_ausencia.py:94, app/chamados/services/silencio_ausencia.py:42, app/chamados/services/silencio_ausencia.py:113) ; nao ; auditor refaz ausencia aberta, marcador e silencio alheio, e acusa cobranca com ausencia aprovada sem a guarda da lampada (app/chamados/management/commands/auditar_invariantes_chamados.py:643, app/chamados/management/commands/auditar_invariantes_chamados.py:646, app/chamados/management/commands/auditar_invariantes_chamados.py:648, app/chamados/management/commands/auditar_invariantes_chamados.py:660) ; -

app/chamados/services/disputa_emissao.py::motivo_fora_da_fabrica ; a fabrica de perguntas alcanca o chamado e, se nao, por que? (app/core/juizes.py:119) ; L-061 (app/docs/LEIS.md:95), FIO-MORTO-E-ESTRUTURAL (app/docs/CORTES.md:253), DUAS-RESPOSTAS item 2 destina nunca_bateu sem adesao a C5 (app/docs/CORTES.md:433) ; retorna causa para disputa, encerrado, nunca_bateu sem adesao, pergunta materializada com celula ausente ou nao acusadora, virada resolvida ou categoria fora do mapa, retorna None para marcos nomeados passados ou categoria mapeada (app/chamados/services/disputa_emissao.py:753, app/chamados/services/disputa_emissao.py:758, app/chamados/services/disputa_emissao.py:760, app/chamados/services/disputa_emissao.py:772, app/chamados/services/disputa_emissao.py:783, app/chamados/services/disputa_emissao.py:785) ; nao ; nenhum achado independente buscando motivo_fora_da_fabrica, chamado_orfao_e_regularizavel, pergunta_do_chamado_ja_encerrada e categoria_fora_do_mapa, consumidores delegam (app/chamados/services/disputa_emissao.py:796, app/chamados/services/pergunta_no_app.py:133) ; -

app/chamados/services/pergunta_no_app.py::retencao ; por que o chamado vivo de furo nao tem pergunta no app? (app/core/juizes.py:120) ; L-061 (app/docs/LEIS.md:95), FIO-MORTO-E-ESTRUTURAL (app/docs/CORTES.md:253) ; sem dia retorna sem_dia, hoje/futuro ou presente no app retorna None, depois verifica fabrica, ata, parcial na lista administrativa, cobertura, pergunta viva em outro chamado, marco ja perguntado e fabrica nao rodada (app/chamados/services/pergunta_no_app.py:127, app/chamados/services/pergunta_no_app.py:129, app/chamados/services/pergunta_no_app.py:133, app/chamados/services/pergunta_no_app.py:136, app/chamados/services/pergunta_no_app.py:144, app/chamados/services/pergunta_no_app.py:146, app/chamados/services/pergunta_no_app.py:149, app/chamados/services/pergunta_no_app.py:151, app/chamados/services/pergunta_no_app.py:153) ; nao ; nenhum achado independente buscando retencao, fabrica_nao_rodou, pergunta_encerrada_marco_apagado e parcial_na_lista_do_admin, consumidores delegam (app/chamados/services/pergunta_no_app.py:201, app/chamados/services/pergunta_no_app.py:223) ; -

app/chamados/juizes.py::competencia_do_chamado ; qual a competencia do chamado? (app/core/juizes.py:122) ; L-026 (app/docs/LEIS.md:77), L-090 (app/docs/LEIS.md:123), K8-COMPETENCIA-NAO-E-MES-CIVIL (app/docs/CORTES.md:216) ; escolhe o fato mais recente entre dia_do_fato, ausencia e perguntas vivas, determina a competencia por janela_atual com a empresa, retorna None sem fato datado (app/chamados/juizes.py:2360, app/chamados/juizes.py:2363, app/chamados/juizes.py:2366, app/chamados/juizes.py:2368, app/chamados/juizes.py:2370, app/chamados/juizes.py:2373) ; nao ; arquivar_competencia_encerrada escolhe dia do fato ou criacao conforme escopo e calcula a competencia aritmeticamente, sem consultar competencia_do_chamado (app/chamados/management/commands/arquivar_competencia_encerrada.py:111, app/chamados/management/commands/arquivar_competencia_encerrada.py:115, app/chamados/management/commands/arquivar_competencia_encerrada.py:177, app/chamados/management/commands/arquivar_competencia_encerrada.py:233) ; -

app/chamados/juizes.py::fio_vivo ; alguma pergunta desta colecao esta viva? (app/core/juizes.py:125) ; L-002 (app/docs/LEIS.md:60), FIO-MORTO-E-ESTRUTURAL declara a autoridade da reducao da colecao (app/docs/CORTES.md:253) ; retorna any(pergunta_viva(p) for p in perguntas), colecao vazia retorna False (app/chamados/juizes.py:1513) ; nao ; Command._disputa_viva faz a reducao any(p.viva for p in d.perguntas.all()) fora do juiz, embora p.viva delegue individualmente a C6 (app/chamados/management/commands/arquivar_competencia_encerrada.py:158, app/chamados/models.py:3431) ; -

app/ponto/services/furos_vetados.py::motivo_do_veto ; este dia veta nascimento ou reabertura automatica de chamado de turno? (app/core/juizes.py:131) ; L-093 (app/docs/LEIS.md:111), VETO-EC-NONE e JUIZ-MOTIVO-DO-VETO (app/docs/CORTES.md:403) ; vinculo/template ausente ou regua degenerada retorna VETO_DEGENERADA, regua suspeita retorna VETO_REGUA, demais retornam None (app/ponto/services/furos_vetados.py:40, app/ponto/services/furos_vetados.py:42, app/ponto/services/furos_vetados.py:44) ; nao ; detectar_ausencias e reconciliar_grade recompõem os dois vetos fora do juiz, copias tambem declaradas no registro (app/ponto/management/commands/detectar_ausencias.py:601, app/ponto/management/commands/detectar_ausencias.py:605, app/ponto/management/commands/reconciliar_grade.py:495, app/ponto/management/commands/reconciliar_grade.py:499, app/core/juizes.py:129) ; -

app/chamados/juizes.py::porta_alcancavel_do_dia ; por onde alguem chega a este dia, com o verbo do painel? (app/core/juizes.py:135) ; L-002 (app/docs/LEIS.md:60), LAUDO-3010-JUIZ-DA-ACAO-DO-ADMIN (app/docs/CORTES.md:404), O261-E-F5-DESENHO (app/docs/CORTES.md:409) ; soma chamados da celula na fila e ancoras na fila de perguntas vivas no app sem resposta vazia, devolve rota, fila visivel, verbos e fio, ou None sem porta (app/chamados/juizes.py:1657, app/chamados/juizes.py:1661, app/chamados/juizes.py:1663, app/chamados/juizes.py:1667) ; nao ; nenhum achado da mesma pergunta buscando porta_alcancavel_do_dia, alimentar_portas, fila_cel, FIO_DO_VERBO e projecoes rota/fila/verbos/fio, _portas_da_tarefa restringe a portas de acoes exigiveis e declara nao reconstituir C17 (app/chamados/services/tarefa_do_dia.py:541, app/chamados/services/tarefa_do_dia.py:546) ; -

app/chamados/juizes.py::dia_sem_trabalho ; a cobranca cala porque a celula lavrada diz que o dia nao e de trabalho? (app/core/juizes.py:149) ; L-061 (app/docs/LEIS.md:95), L-125 (app/docs/LEIS.md:146), JUIZ-DIA-SEM-TRABALHO (app/docs/CORTES.md:416), excecao de ausencia parcial documentada em app/docs/JUIZES.md:67 ; True para trabalha is False ou ata.tipo_dia preenchido diferente de trabalho, na ausencia parcial somente trabalha is False cala, trabalha=None sozinho nao cala (app/chamados/juizes.py:438, app/chamados/juizes.py:454, app/chamados/juizes.py:456) ; nao ; cartorio._protesta deriva parcialmente a resposta usando trabalha is not False e marcos nao acesos, excecao expressamente declarada nos textos (app/ponto/services/cartorio.py:1458, app/ponto/services/cartorio.py:1459, app/docs/CORTES.md:416, app/docs/JUIZES.md:75) ; -

app/chamados/services/tarefa_do_dia.py::tarefa_do_dia ; quais acoes ainda sao exigiveis por instancia de marco, de quem e por quais portas? (app/core/juizes.py:155) ; L-002 (app/docs/LEIS.md:60), L-061 (app/docs/LEIS.md:95), FIO-E0-V4 (app/docs/CORTES.md:426), LEI-TRANCA-E2-JUIZ (app/docs/CORTES.md:427) ; exige agora e carga no mesmo instante, combina grade, lampadas, teto, tranca, ultima decisao e efeitos para produzir acoes com dono e porta, expondo nao_sei quando indeterminado (app/chamados/services/tarefa_do_dia.py:116, app/chamados/services/tarefa_do_dia.py:164, app/chamados/services/tarefa_do_dia.py:204, app/chamados/services/tarefa_do_dia.py:306, app/chamados/services/tarefa_do_dia.py:562, app/chamados/services/tarefa_do_dia.py:566) ; nao ; vigia_fio.dono_fora_do_fio decide separadamente se a pendencia espera decisao do dia, ausencia ou cadastro, respondedor antigo nomeado para substituicao na E3 (app/chamados/services/vigia_fio.py:306, app/chamados/services/vigia_fio.py:309, app/chamados/services/vigia_fio.py:311, app/chamados/services/vigia_fio.py:313, app/docs/CORTES.md:426) ; -

app/chamados/juizes.py::dia_da_pergunta_trancado ; o dia da pergunta esta arquivado por competencia trancada sem toque posterior do admin? (app/core/juizes.py:161) ; L-051 (app/docs/LEIS.md:92), L-090 (app/docs/LEIS.md:123), LEI-TRANCA-E2-JUIZ (app/docs/CORTES.md:427), assinatura humana do toque documentada em app/docs/JUIZES.md:88 ; consulta dias_trancados pela competencia da empresa e responde True somente sem toque administrativo posterior, conversa nao tem dia de turno a trancar (app/chamados/juizes.py:338, app/chamados/juizes.py:991, app/chamados/juizes.py:1062, app/chamados/juizes.py:1078) ; nao ; nenhum achado independente buscando dia_da_pergunta_trancado, dias_trancados, periodo_trancado, TOQUE_COBRAR_DIA e _admin_tocou, consumidores conferidos delegam (app/chamados/reconciliador.py:668, app/chamados/services/disputa_emissao.py:1970, app/chamados/services/tarefa_do_dia.py:422) ; -

app/chamados/services/fabrica_da_celula.py::pauta_cadastro_existente ; o dia passado sem turno previsto ja esta entregue ao DP como pendencia de cadastro? (app/core/juizes.py:297) ; MODO-COMUM-PASSADO-SEM-TURNO e LEI-PASSADO-SEM-MARCOS (app/docs/CORTES.md:428) ; True quando existe cabeca DP viva do marcador para o colaborador ou pauta fechada por feito/cancelado em data local igual ou posterior ao dia consultado, dia=None consulta somente viva (app/chamados/services/fabrica_da_celula.py:308, app/chamados/services/fabrica_da_celula.py:327, app/chamados/services/fabrica_da_celula.py:329, app/chamados/services/fabrica_da_celula.py:331, app/chamados/services/fabrica_da_celula.py:334) ; nao ; nenhum achado independente buscando PAUTA_SEM_TURNO, CADASTRO: PASSADO SEM TURNO PREVISTO e pauta_cadastro_existente, o escritor pergunta ao juiz (app/chamados/services/fabrica_da_celula.py:367) ; -

app/chamados/models.py::DisputaSupervisao.respostas_sem_veredito ; quais respostas do colaborador na disputa esperam veredito? (app/core/juizes.py:298) ; O168-JUIZ-DO-VEREDITO (app/docs/CORTES.md:449), tranca por leitura L-051 (app/docs/LEIS.md:92, app/chamados/models.py:1896) ; lista perguntas sem validada_em nem materializada_em, com respondida_em ou texto preenchido apos strip, excluindo dia trancado salvo ignorar_tranca=True (app/chamados/models.py:1886, app/chamados/models.py:1898, app/chamados/models.py:1902) ; nao ; auditor deriva I21 por respondida_em preenchido, validada/materializada nulas e texto diferente de vazio, restringindo a pais/destinos mortos sem chamar o juiz (app/chamados/management/commands/auditar_invariantes_chamados.py:1572, app/chamados/management/commands/auditar_invariantes_chamados.py:1575, app/chamados/management/commands/auditar_invariantes_chamados.py:1576) ; -

app/ponto/supra_juiz.py::pode_executar_csf ; o chamado tem resposta sem veredito que impede encerramento pelo sistema? (app/core/juizes.py:299) ; O168-JUIZ-CSF com predicado antigo expressamente superado (app/docs/CORTES.md:448), O168-JUIZ-DO-VEREDITO (app/docs/CORTES.md:449) ; True permite encerrar somente sem respostas pendentes de veredito nas disputas de origem e nas perguntas que resolvem o chamado, consulta sem_veredito_entre com ignorar_tranca=True nas duas metades (app/ponto/supra_juiz.py:567, app/ponto/supra_juiz.py:573, app/ponto/supra_juiz.py:574, app/ponto/supra_juiz.py:588) ; nao ; 

[truncado em 20000 de 20531 caracteres]

_5117 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
