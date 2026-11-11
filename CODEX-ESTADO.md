# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 13:25** · publicado em **11/11/2026 13:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 13:25 · sessao c2682795b25e · FIO-E0-V4-E4B · construtor · origem=Code

Cura, testes, mutantes e análise dos chamadores entregues no [recibo c6](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c6.txt:6). [nao verificado]

Observado localmente: **104 testes em memória e 40 selos passaram**; RED da base e os dois mutantes falharam nas assertions esperadas ([verificações](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c6.txt:54)). [nao verificado]

ORM não executado: **[nao verificado]**. Os [comandos para a sessão](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c6.txt:88) estão registrados. Sem commit, respeitando a proibição explícita do pedido. [nao verificado]

## 11/11 13:21 · sessao da29efa3c9fc · PROPORCAO-POR-RISCO · verificador · origem=Code

**REPROVADO — há escapes de severidade ALTA que permitem declarar B com escrita direta no diff.** A detecção depende da grafia e do cabeçalho textual, e zero achados autoriza B. (`bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:48`)

Os resultados abaixo foram conferidos com diffs sintéticos fornecidos em memória ao script. Execução contra commits reais, banco, implantação e autenticidade do commit/base: **[nao verificado]**. A suíte de host completa cria e remove arquivos, portanto não foi executada nesta revisão somente leitura. (`bin/tests/test_selo_proporcao.sh:9`, `bin/tests/test_selo_proporcao.sh:18`)

1. **ALTA — escritas da lista passam com rc 0 por diferenças de grafia e de representação do diff.**

   Este diff reúne exemplos para **save, create, update, delete, bulk_create, bulk_update, get_or_create e update_or_create**. São expressões Python válidas; todas escapam porque a regex exige o nome imediatamente depois do ponto. O parser retornou **rc 0 para B**. (`bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:44`, `bin/selo_proporcao.sh:48`)

   ```diff
   --- /dev/null
   +++ b/app/x/escritor.py
   @@ -0,0 +1,8 @@
   +obj. save()
   +M.objects. create(a=1)
   +M.objects. update(a=1)
   +obj. delete()
   +M.objects. bulk_create(xs)
   +M.objects. bulk_update(xs, ["a"])
   +M.objects. get_or_create(a=1)
   +M.objects. update_or_create(a=1)
   ```

   Outras formas concretas também retornaram **rc 0**, cada uma adicionada isoladamente a um `.py` comum: a análise é linha a linha, exige a chamada literal e corta a linha no primeiro `#`. (`bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:27`, `bin/selo_proporcao.sh:36`)

   | Forma | Linhas adicionadas ao diff |
   |---|---|
   | Quebra antes do parêntese | `+(obj.save`<br>`+())` |
   | Alias | `+gravar = obj.save`<br>`+gravar()` |
   | `getattr` | `+getattr(obj, "save")()` |
   | Método entre parênteses | `+(obj.save)()` |
   | `#` dentro de string | `+tag = "#"; obj.save()` |

   **Signals:** este acréscimo em `app/x/eventos.py` retorna **rc 0**. A regex reconhece alguns nomes literais e `@receiver`; a regra por arquivo cobre somente `signals.py`. (`bin/selo_proporcao.sh:24`, `bin/selo_proporcao.sh:40`)

   ```diff
   --- a/app/x/eventos.py
   +++ b/app/x/eventos.py
   @@ -10,0 +11,2 @@
   +from django.db.models import signals as sig
   +getattr(sig, "post" + "_save").connect(gravar, sender=M)
   ```

   **Management command que escreve:** este acréscimo retorna **rc 0**. Não existe tratamento próprio para `management/commands/`; ele depende das mesmas regex de qualquer `.py`. (`bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:35`, `bin/selo_proporcao.sh:37`)

   ```diff
   --- a/app/x/management/commands/gravar.py
   +++ b/app/x/management/commands/gravar.py
   @@ -10,0 +11,2 @@
   +        escrever = M.objects.create
   +        escrever(a=1)
   ```

   **Migration por rename:** este patch retorna **rc 0**, sem sequer listar o destino. O scanner só aprende o caminho em `+++` e só aplica a regra de migrations numa linha adicionada. Conteúdo da migration e efeito no banco deste exemplo: **[nao verificado]**. (`bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:32`, `bin/selo_proporcao.sh:34`)

   ```diff
   diff --git a/app/x/pendente.py b/app/x/migrations/0002_gravar.py
   similarity index 100%
   rename from app/x/pendente.py
   rename to app/x/migrations/0002_gravar.py
   ```

   **Binário:** esta representação também retorna **rc 0**, porque não tem `+++` nem linhas adicionadas. Um `.py` textual marcado `-diff` em `.gitattributes` é o cenário relevante; inserir NUL num `.py` não demonstra Python executável. Aplicação real desse atributo neste repositório: **[nao verificado]**. (`bin/selo_proporcao.sh:19`, `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:32`)

   ```diff
   diff --git a/app/x/escritor.py b/app/x/escritor.py
   Binary files a/app/x/escritor.py and b/app/x/escritor.py differ
   ```

   **Caminhos:** os cabeçalhos abaixo, seguidos de `+obj.save()`, retornam **rc 0**. `<TAB>` representa tabulação real. O parser preserva tabulação ou aspas finais; consequentemente, o caminho não termina em `.py`. (`bin/selo_proporcao.sh:29`, `bin/selo_proporcao.sh:35`)

   ```text
   +++ b/app/x/escritor outro.py<TAB>
   +++ "b/app/x/caf\303\251.py"
   ```

   **`+++` dentro do conteúdo:** este diff cria Python sintaticamente válido e retorna **rc 0**. A segunda linha do arquivo é `++ /dev/null`, dentro da string; no patch, ela vira um falso cabeçalho que redefine `arq=None`. (`bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:29`, `bin/selo_proporcao.sh:32`)

   ```diff
   --- /dev/null
   +++ b/app/x/escritor.py
   @@ -0,0 +1,4 @@
   +DOC = """
   +++ /dev/null
   +"""
   +obj.save()
   ```

   **Modo do diff:** a chamada `bin/selo_proporcao.sh --name-only HEAD B repo` permite que o suposto argumento de revisão seja interpretado como opção do Git. Uma saída contendo somente `app/x/escritor.py` vira zero achados e **rc 0**; as revisões não são validadas separadamente. Execução completa desse cenário num repositório real: **[nao verificado]**. (`bin/selo_proporcao.sh:19`, `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:48`)

2. **ALTA — a exclusão de testes permite esconder módulo de produção que grava.** [nao verificado]

   Sim. Este diff retorna **rc 0**: a escrita é descartada pelo nome `test_*.py`, enquanto o módulo de produção importa e chama a função. O scanner não verifica consumidores nem separa fixture de módulo usado pela aplicação. (`bin/selo_proporcao.sh:25`, `bin/selo_proporcao.sh:32`)

   ```diff
   --- /dev/null
   +++ b/app/x/test_writer.py
   @@ -0,0 +1,2 @@
   +def gravar(obj):
   +    obj.save()
   --- a/app/x/views.py
   +++ b/app/x/views.py
   @@ -10,0 +11,2 @@
   +from .test_writer import gravar
   +gravar(obj)
   ```

   A variante `app/x/tests/writer.py`, importada por `from .tests.writer import gravar`, também retorna **rc 0**. A exclusão por diretório é independente do nome do arquivo e ocorre inclusive antes da regra de migrations. Existência desse uso em produção na cópia: **[nao verificado]**. (`bin/selo_proporcao.sh:25`, `bin/selo_proporcao.sh:32`, `bin/selo_proporcao.sh:34`)

3. **MÉDIA — casos do host não isolam o mecanismo anunciado.**

   | Caso | Por que pode continuar passando com parte da guarda quebrada |
   |---|---|
   | `command_escreve` | `.delete()` basta em qualquer `.py`; não prova reconhecimento de commands. (`bin/tests/test_selo_proporcao.sh:33`; `bin/selo_proporcao.sh:22`) |
   | `signal_receiver` | Contém `@receiver` **e** `post_save`; qualquer um sustenta o resultado se o outro deixar de funcionar. (`bin/tests/test_selo_proporcao.sh:31`; `bin/selo_proporcao.sh:24`) |
   | `fixture_teste` | Usa `tests/` **e** `test_a.py`; remover uma das exclusões não faz o caso falhar. (`bin/tests/test_selo_proporcao.sh:37`; `bin/selo_proporcao.sh:25`) |
   | `orm_bulk` / `orm_goc` | Exercitam somente `bulk_create` e `update_or_create`; não há casos independentes para `bulk_update` e `get_or_create`. (`bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:28`) |

   Há ainda uma fragilidade de preparação: checkout, alteração, add e commit não têm verificação individual; falta `errexit` e o commit permite árvore vazia. Uma preparação falha pode deixar o caso julgando outro diff. Ocorrência efetiva: **[nao verificado]**. (`bin/tests/test_selo_proporcao.sh:5`, `bin/tests/test_selo_proporcao.sh:17`, `bin/tests/test_selo_proporcao.sh:18`)

4. **ALTA para conteúdo omitido; o pipe interno trata corretamente as falhas perguntadas.**

   | Caminho | Resultado |
   |---|---|
   | Git retorna não zero | Selo retorna **2**. (`bin/selo_proporcao.sh:19`) |
   | Python retorna **1 ou 2** | Selo retorna **2**. (`bin/selo_proporcao.sh:46`, `bin/selo_proporcao.sh:51`) |
   | Produtor do pipe falha | `pipefail` preserva a falha; resultado fora de 0/3 vira **2**. (`bin/selo_proporcao.sh:15`, `bin/selo_proporcao.sh:20`, `bin/selo_proporcao.sh:51`) |
   | Git retorna 0 com saída vazia | Selo retorna **0**. Correto para diff realmente vazio; falso verde se conteúdo relevante foi omitido. (`bin/selo_proporcao.sh:26`, `bin/selo_proporcao.sh:44`, `bin/selo_proporcao.sh:48`) |

   Portanto, os falsos verdes demonstrados estão na interpretação/omissão do diff, não no mapeamento de Python rc 1/2. (`bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:35`, `bin/selo_proporcao.sh:51`)

   **MÉDIA, condicionada à invocação:** com `bash -e`, o pipeline que retorna 3 encerra o script antes de `r=$?`: escrita resulta em **rc 3**, tanto para A quanto B. Esse modo de chamada hoje: **[nao verificado]**. (`bin/selo_proporcao.sh:15`, `bin/selo_proporcao.sh:20`, `bin/selo_proporcao.sh:44`, `bin/selo_proporcao.sh:46`)

   Um chamador externo que use `selo ... | cat` sem `pipefail` pode observar o rc do `cat`. Uso desse encadeamento hoje: **[nao verificado]**. O teste novo captura diretamente o rc do selo. (`bin/tests/test_selo_proporcao.sh:19`)

5. **MÉDIA — alcance dos consumidores e falha preexistente em worktree.**

   - **Pre-push:** não executa o loop de `bin/tests`; chama selos específicos. Não há chamada ao selo de proporção nesse fluxo da cópia. Instalação efetiva do hook e chamada manual no pouso: **[nao verificado]**. (`bin/pre-push.sh:13`, `bin/pre-push.sh:16`, `bin/pre-push.sh:19`, `bin/pre-push.sh:38`)
   - **Régua:** o novo `test_selo_proporcao.sh` entra automaticamente pelo glob e pode barrar a suíte pelo rc. Ele julga um repositório sintético, não o diff real da obra. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`; `bin/tests/test_selo_proporcao.sh:9`, `bin/tests/test_selo_proporcao.sh:19`)
   - **Atalho da régua:** uma mudança somente em `bin/` pode ser pulada pelo “JA VERDE”: a condição compara o fingerprint de `.py`/`.html` de `app/` e retorna antes dos testes de host, sem comparar `ARVORE_FP`. (`bin/regua.sh:25`, `bin/regua.sh:33`, `bin/regua.sh:45`, `bin/regua.sh:86`, `bin/regua.sh:93`)
   - **Teste novo isolado em worktree:** não pressupõe `.git` como diretório; resolve o selo pelo próprio caminho e fornece explicitamente o repositório sintético. Execução efetiva em worktree: **[nao verificado]**. (`bin/tests/test_selo_proporcao.sh:6`, `bin/tests/test_selo_proporcao.sh:19`)
   - **Pasta inteira em worktree:** contém uma quebra independente deste selo. `test_hook_nao_e_copia.sh` exige `$RAIZ/.git/hooks/pre-push` e retorna 1 quando ausente; a própria cópia documenta que `.git` de worktree é arquivo e mostra resolução pelo diretório comum em outro teste. (`bin/tests/test_hook_nao_e_copia.sh:21`, `bin/tests/test_hook_nao_e_copia.sh:25`, `bin/tests/test_hook_nao_e_copia.sh:27`; `bin/tests/test_commit_so_o_declarado.sh:52`, `bin/tests/test_commit_so_o_declarado.sh:57`)

## 11/11 13:19 · sessao 93579e5840e6 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos derivados de `bin/selo_proporcao.sh` e simulados em memória com o script original e função `git` que fornece o diff, sem escrever arquivos:

- **ALTA — ORM direto escapa:** `+obj. save()`, `+M.objects. create(a=1)`, `+M.objects. update(a=1)`, `+obj. delete()`, `+M.objects. bulk_create([])` são Python válido (`ast.parse`) e deram rc 0. A regex exige ponto imediatamente seguido do nome. Aplica-se também a `bulk_update`, `get_or_create`, `update_or_create`. `bin/selo_proporcao.sh:22`.
- **ALTA — continuação/alias/getattr:** deram rc 0 os diffs `+obj.save\` seguido de `+()`, `+write = obj.save` seguido de `+write()`, `+getattr(obj, "save")()` e `+(obj.save)()`. `bin/selo_proporcao.sh:22`, `bin/selo_proporcao.sh:27`.
- **ALTA — `#` dentro de string oculta escrita na mesma linha:** `+label = "#"; obj.save()` deu rc 0 porque corta no primeiro `#`, sem análise léxica. `bin/selo_proporcao.sh:36`.
- **ALTA — cabeçalho forjado no conteúdo:** este diff cria Python válido e deu rc 0:

```diff
+++ b/app/writer.py
@@ -0,0 +1,4 @@
+DOC = """
+++ /dev/null
+"""
+obj.save()
```

O conteúdo real da segunda linha é `++ /dev/null` dentro de uma string. O parser considera a linha do patch um cabeçalho, põe `arq=None` e ignora a escrita. `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:29`, `bin/selo_proporcao.sh:32`.

- **ALTA — caminhos:** `+++ b/app/writer other.py<TAB>` e `+++ "b/app/caf\303\251.py"` seguidos de `+obj.save()` deram rc 0: o caminho fica com TAB ou aspas finais e falha em `.endswith(".py")`. Git real `diff --no-index` sobre memfds, com prefixos contendo espaço/Unicode, confirmou esses formatos de cabeçalho; não houve arquivos criados. `bin/selo_proporcao.sh:29`, `bin/selo_proporcao.sh:35`.
- **ALTA — migration por rename:** patch com `similarity index 100%`, `rename from app/operation.py`, `rename to app/migrations/0002_operation.py` e sem hunks deu rc 0. A regra migration só roda após encontrar linha adicionada sob `+++`. `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:32`, `bin/selo_proporcao.sh:34`. A geração real desse rename nesta cópia está [nao verificado], pois não há `.git`.
- **ALTA — binário:** patch `Binary files /dev/null and b/app/writer.py differ` deu rc 0 e lista zero arquivos. Para demonstrar escrita executável, usar arquivo Python textual marcado `-diff` por `.gitattributes`; NUL num `.py` não demonstra execução Python válida. Efeito real de `.gitattributes` no repo está [nao verificado]. `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:32`.
- **ALTA — modo do diff por argumento:** `BASE=--name-only`, `HEAD_=HEAD` entra como opção, pois não há validação de revisão nem separador de opções. `--name-only` sobrepõe `-U0`, produz nomes sem patch; o parser aceita isso como zero escrita. Git real `--no-index -U0 --name-only` confirmou saída só de nomes; execução completa em repo está [nao verificado]. `bin/selo_proporcao.sh:19`, `bin/selo_proporcao.sh:28`, `bin/selo_proporcao.sh:48`. **Não usar `--stat` como exemplo:** ele preserva o patch quando combinado com `-U0`. `diff.noprefix=true` sozinho também não basta para escapar.
- **ALTA — signal/command:** `+ps.connect(write_handler)` em `.py` fora de `signals.py`, com alias `ps` já existente, deu rc 0; `management/commands/w.py` com `+write = obj.save` / `+write()` também deu rc 0. Não há regra especial para management, apenas ORM/SQL/SIG; signal reconhece lista finita de nomes e `@receiver`. `bin/selo_proporcao.sh:24`, `bin/selo_proporcao.sh:37`, `bin/selo_proporcao.sh:40`.

Todos os rc 0 acima passam pela saída final `bin/selo_proporcao.sh:48`; os testes foram do parser e encadeamento originais, com diff sintético, não de commits existentes.

## 11/11 13:17 · sessao fb12153ec901 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos derivados da cópia; a suíte de host não foi executada porque cria e apaga diretórios temporários (`bin/tests/test_selo_proporcao.sh:9`).

- **Média — exclusões de teste não têm provas independentes.** `fixture_teste` usa simultaneamente diretório `tests/` e nome `test_a.py`; remover qualquer uma das duas condições de exclusão preservaria seu rc esperado. O caso deveria ser separado para provar ambas (`bin/tests/test_selo_proporcao.sh:37`; `bin/selo_proporcao.sh:25`).
- **Média — teste de signals tem detecção redundante.** `signal_receiver` contém `@receiver` e `post_save`; uma alternativa da regex pode deixar de funcionar e o caso continuar vermelho por causa da outra (`bin/tests/test_selo_proporcao.sh:31`; `bin/selo_proporcao.sh:24`).
- **Média — command_escreve não prova comportamento específico de commands.** Sua linha contém `.delete()`, detectável em qualquer arquivo `.py`; o scanner não consulta `management/commands/`. O caso testa a detecção genérica de ORM e não cobre comando que grava por outra forma (`bin/tests/test_selo_proporcao.sh:33`; `bin/selo_proporcao.sh:22`; `bin/selo_proporcao.sh:35`; `bin/selo_proporcao.sh:37`).
- **Média — cobertura anunciada excede os casos.** `orm_bulk` exercita somente `bulk_create`; `orm_goc` somente `update_or_create`. Não há casos independentes para `bulk_update` ou `get_or_create` na lista, embora ambos estejam na regex (`bin/tests/test_selo_proporcao.sh:27`; `bin/tests/test_selo_proporcao.sh:28`; `bin/selo_proporcao.sh:22`).
- **Média — falhas de preparação não interrompem necessariamente o host.** O script ativa `-u` e `pipefail`, mas não `-e`; checkout, alteração, add e commit não têm tratamento individual de erro, e commits vazios são permitidos. Assim, o rc aferido pode não corresponder à alteração pretendida se uma preparação falhar; ocorrência real nesta cópia: [nao verificado] (`bin/tests/test_selo_proporcao.sh:5`; `bin/tests/test_selo_proporcao.sh:17`; `bin/tests/test_selo_proporcao.sh:18`; `bin/tests/test_selo_proporcao.sh:19`).

Sobre rc:

- **Não há evidência de falso verde causado pelo pipe interno ou por Python rc 1/2.** `pipefail` está ativo; o rc do pipeline é capturado, e qualquer resultado diferente de 0 ou 3 vira rc 2. Git não zero também vira rc 2 (`bin/selo_proporcao.sh:15`; `bin/selo_proporcao.sh:19`; `bin/selo_proporcao.sh:20`; `bin/selo_proporcao.sh:46`; `bin/selo_proporcao.sh:51`).
- Simulei em memória, sem arquivos: Python rc 1 → selo rc 2; Python rc 2 → selo rc 2; Git rc 1 → selo rc 2; Python rc 3 → B rc 1/A rc 0. Um produtor grande com consumidor que termina sem ler produziu SIGPIPE 141, convertido para rc 2, coerente com os ramos citados (`bin/selo_proporcao.sh:15`; `bin/selo_proporcao.sh:46`; `bin/selo_proporcao.sh:49`; `bin/selo_proporcao.sh:51`).
- **Saída vazia com Git rc 0 é aceita.** Não há exigência de cabeçalho, arquivo ou hunk: zero achados leva a rc 0. Isso é correto para diff realmente vazio, mas permite falso verde quando o Git entrega outro formato ou omite conteúdo. O defeito é a confiança no conteúdo/formato recebido, não o `pipefail` (`bin/selo_proporcao.sh:19`; `bin/selo_proporcao.sh:26`; `bin/selo_proporcao.sh:28`; `bin/selo_proporcao.sh:44`; `bin/selo_proporcao.sh:48`).

## 11/11 13:17 · sessao 3b39b337b028 · - · - · origem=Ronald (tmux)

- **ALTA — o selo da obra não virou portão automático.** O consumidor nominal é o teste, que chama o selo com `BASE`, `HEAD`, classe e repositório sintéticos; isso verifica o instrumento, não classifica a obra real (`bin/tests/test_selo_proporcao.sh:9`, `bin/tests/test_selo_proporcao.sh:19`). O próprio pre-push informa que não executa a pasta `bin/tests`, chamando apenas selos específicos (`bin/pre-push.sh:13`, `bin/pre-push.sh:16`, `bin/pre-push.sh:19`, `bin/pre-push.sh:38`). A execução manual no pouso e sua observância em produção são **[nao verificado]**.

- **MÉDIA — o teste entra automaticamente na régua, mas pode ser pulado pelo carimbo.** A régua executa `tests/test_*.sh` e aborta antes da suite se algum retornar erro (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`). Contudo, seu atalho “JA VERDE” compara somente a impressão digital de `.py`/`.html` sob `app/` e retorna antes desse loop; uma mudança somente em `bin/` não altera essa impressão (`bin/regua.sh:25`, `bin/regua.sh:33`, `bin/regua.sh:84`, `bin/regua.sh:93`). Embora calcule `ARVORE_FP`, a condição do atalho não o compara (`bin/regua.sh:45`, `bin/regua.sh:86`).

- **MÉDIA — a pasta inteira já contém quebra em worktree.** `test_hook_nao_e_copia.sh` resolve o hook como `$RAIZ/.git/hooks/pre-push` e retorna 1 se esse caminho não for arquivo (`bin/tests/test_hook_nao_e_copia.sh:21`, `bin/tests/test_hook_nao_e_copia.sh:25`, `bin/tests/test_hook_nao_e_copia.sh:27`). A própria cópia documenta que `.git` em worktree é arquivo e mostra a solução por `--git-common-dir` em outro selo (`bin/tests/test_commit_so_o_declarado.sh:52`, `bin/tests/test_commit_so_o_declarado.sh:57`). Portanto, o loop completo pode falhar por esse selo anterior, independentemente de proporção (`bin/regua.sh:138`, `bin/regua.sh:143`).

- **O teste novo isolado não contém essa suposição de `.git` como diretório.** Resolve o executável relativamente ao próprio arquivo e fornece explicitamente o repositório temporário ao selo (`bin/tests/test_selo_proporcao.sh:6`, `bin/tests/test_selo_proporcao.sh:7`, `bin/tests/test_selo_proporcao.sh:19`). Ainda assim, não limpa variáveis `GIT_*` herdadas antes dos comandos Git; essa precaução existe em outro teste da cópia (`bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:14`, `bin/tests/test_lei_akita.sh:69`). Falha efetiva sob ambiente Git herdado é **[nao verificado]**.

Não executei os selos porque criam e removem arquivos temporários (`bin/tests/test_selo_proporcao.sh:9`). Resultado real da pasta e instalação atual dos hooks: **[nao verificado]**.

## 11/11 12:59 · sessao daf646968a80 · FIO-E0-V4-E4B · censo · origem=Code

| achado | classe | trecho da pergunta | trecho da resposta | linha de triagem | observação |
|---|---|---|---|---|---|
| **1. Recusa S143 impede S128, HX e fecho C7** (`opus_final_achados.txt:2`) | **(a)** | “Ha caminho que escapa (evento S143 x backfill, credor = destino x pai, recusa ja conhecida x nova)?” (`preverif2.q:7`) | “**Cura restritiva presente:** […] evento interrompe o resolver” (`preverif2.r:10`). | Sem descarte correspondente nas triagens fornecidas (`triagem_preverif2.txt:1`; `triagem_c4_v1.txt:1`). | O Codex descreveu a interrupção, mas não identificou a regressão sobre as demais perguntas. O retorno antecede S128, HX-RESPOSTA-VISIVEL, HX-DISPUTA-ORFA e C7 (`app/chamados/services/disputa_emissao.py:1946`, `:1950`, `:1986`, `:2009`, `:2086`). |
| **2. Eventos sobre chamados já resolvidos podem reabrir passivo da competência aberta** (`opus_final_achados.txt:3`) | **(a)** | “Idempotencia: rodar 2x o evento ou o backfill = mesmo estado, 1 linha de trilha, 0 duplicata? Laco reabre->fecha->reabre […] existe hoje na arvore?” (`preverif1.q:12`). | “A aresta direta ‘S143 → E4B → reabrir credor exportado’ fica cortada quando a consulta já enxerga a exportação” (`preverif2.r:16`). | Sem descarte correspondente; o descarte sobre “corrida com ExportacaoDominio” trata de entrega concorrente (`triagem_c4_v1.txt:10`). | A resposta examinou o corte para **exportadas**, sem identificar a reativação do passivo **aberto** por esses chamadores. Há chamadas sobre terminais e caminho para `reabrir()`/ressuscitação (`app/chamados/services/becos.py:43`, `:54`; `app/chamados/reconciliador.py:1646`; `app/chamados/services/disputa_emissao.py:1849`; `app/chamados/models.py:1451`). Ocorrência efetiva após deploy e prova de frota: **[nao verificado]**. |
| **3. Tetos 213/269 supostamente comentados** (`opus_final_achados.txt:4`) | **não procede — sem classificação** | — | — | — | O próprio achado veio “[nao verificado]”. Os valores **213** e **269** são entradas ativas de `TETOS`; `_cobrar` consulta o teto e executa `assertLessEqual` (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:292`, `:293`). |

falhas do Codex (classe a) = 2.

## 11/11 12:53 · sessao cb3c2382aea3 · LOTE-VALIDAR-REGRESSAO-0311 · sonda · origem=Code

Não executei a sonda no banco: sem acesso a banco/log, execução e transação READ ONLY permanecem [nao verificado]. Reproduzo integralmente a saída fornecida. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda_cliques.py [952087b30c6501fc] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

linhas_lidas 2684
# dia | validou_lote_tela | ia_validou_lote | val_perg_disp | val_perg_nao_gravou | materializar_batida | vincular_pergunta_origem
dia 2026-10-20 2 0 54 5 98 98
dia 2026-10-21 2 0 52 7 28 28
dia 2026-10-22 1 0 54 2 37 37
dia 2026-10-23 0 0 76 7 55 55
dia 2026-10-24 0 0 1 0 0 0
dia 2026-10-25 0 0 2 0 0 0
dia 2026-10-26 1 0 170 9 144 144
dia 2026-10-27 0 0 73 13 37 37
dia 2026-10-28 11 0 72 8 60 60
dia 2026-10-29 6 0 48 6 41 41
dia 2026-10-30 12 0 27 25 21 21
dia 2026-10-31 0 0 1 0 0 0
dia 2026-11-01 0 0 1 0 0 0
dia 2026-11-02 0 0 2 0 0 0
dia 2026-11-03 2 0 90 12 79 79
dia 2026-11-04 0 0 1 1 0 0
dia 2026-11-05 2 0 30 2 25 25
dia 2026-11-06 2 0 39 3 38 38
dia 2026-11-07 0 0 1 0 0 0
dia 2026-11-08 0 0 1 0 0 0
dia 2026-11-09 0 0 18 5 14 14
dia 2026-11-10 0 0 12 3 9 9
dia 2026-11-11 4 0 113 31 95 95
lotes_sem_trilha_mensuravel 25
# dia | n_lotes | pedidas_mediana/max | validadas_soma | seg_mediana/p90/max | seg_por_pergunta_mediana | lotes>30s
lote 2026-10-20 2 7.5/9 15 6.3/6.6/6.6 0.83 0 cadeia/val+rec=[(10, 9), (6, 6)] seg=[6.6, 6.0]
lote 2026-10-21 2 6.5/11 13 5.7/10.0/10.0 0.75 0 cadeia/val+rec=[(12, 11), (2, 2)] seg=[10.0, 1.3]
lote 2026-10-22 1 51/51 45 74.0/74.0/74.0 1.57 1 cadeia/val+rec=[(47, 51)] seg=[74.0]
lote 2026-10-26 1 125/125 107 145.5/145.5/145.5 1.3 1 cadeia/val+rec=[(112, 125)] seg=[145.5]
lote 2026-10-28 11 15/47 52 17.5/46.6/46.6 1.34 1 cadeia/val+rec=[(35, 47), (0, 15), (0, 15), (0, 15), (0, 15), (0, 15), (11, 26), (0, 15), (13, 29), (0, 16), (0, 16)] seg=[46.6, None, None, None, None, None, 14.9, None, 17.5, None, None]
lote 2026-10-29 6 12.0/31 28 28.7/34.1/34.1 1.92 1 cadeia/val+rec=[(12, 24), (18, 31), (0, 12), (0, 12), (0, 12), (0, 12)] seg=[23.2, 34.1, None, None, None, None]
lote 2026-10-30 12 29.0/33 4 19.7/19.7/19.7 0.98 0 cadeia/val+rec=[(20, 33), (0, 29), (0, 29), (0, 29), (0, 29), (0, 29), (0, 29), (0, 29), (0, 29), (0, 29), (0, 29), (0, 29)] seg=[19.7, None, None, None, None, None, None, None, None, None, None, None]
lote 2026-11-03 2 67.0/87 55 53.9/94.8/94.8 1.54 1 cadeia/val+rec=[(53, 87), (10, 47)] seg=[94.8, 12.9]
lote 2026-11-05 2 51.5/57 23 16.7/27.1/27.1 1.28 0 cadeia/val+rec=[(18, 57), (6, 46)] seg=[27.1, 6.3]
lote 2026-11-06 2 50.5/56 21 12.3/18.1/18.1 1.19 0 cadeia/val+rec=[(17, 56), (5, 45)] seg=[18.1, 6.6]
lote 2026-11-11 4 68.0/119 61 65.5/122.7/122.7 1.42 1 cadeia/val+rec=[(74, 119), (1, 59), (7, 68), (0, 68)] seg=[122.7, None, 8.3, None]
```

1. A saída não contém mensagem explícita de erro de execução nem indica universo vazio: informa `linhas_lidas 2684`. Informa também `lotes_sem_trilha_mensuravel 25`. [nao verificado] Há contagens positivas de `val_perg_nao_gravou`; no código, essa ação registra validação com ponto não gravado e retorno `ok: False`. [validacao.py:490](/home/ronald/codex_ws/e70a54f7e1050568237459da72cae78de0763d08/app/chamados/services/validacao.py:490) A causa de cada ocorrência em produção é [nao verificado].

2. Comparação por lote, na ordem fornecida; repetições consecutivas estão indicadas por `×`. [nao verificado]

| Dia | Comparações cadeia versus val+rec | Iguais | Diferentes | Evidência |
|---|---|---:|---:|---|
| 2026-10-20 | 10≠9; 6=6 | 1 | 1 | [nao verificado] |
| 2026-10-21 | 12≠11; 2=2 | 1 | 1 | [nao verificado] |
| 2026-10-22 | 47≠51 | 0 | 1 | [nao verificado] |
| 2026-10-26 | 112≠125 | 0 | 1 | [nao verificado] |
| 2026-10-28 | 35≠47; 5×(0≠15); 11≠26; 0≠15; 13≠29; 2×(0≠16) | 0 | 11 | [nao verificado] |
| 2026-10-29 | 12≠24; 18≠31; 4×(0≠12) | 0 | 6 | [nao verificado] |
| 2026-10-30 | 20≠33; 11×(0≠29) | 0 | 12 | [nao verificado] |
| 2026-11-03 | 53≠87; 10≠47 | 0 | 2 | [nao verificado] |
| 2026-11-05 | 18≠57; 6≠46 | 0 | 2 | [nao verificado] |
| 2026-11-06 | 17≠56; 5≠45 | 0 | 2 | [nao verificado] |
| 2026-11-11 | 74≠119; 1≠59; 7≠68; 0≠68 | 0 | 4 | [nao verificado] |
| **Total** | **45 lotes listados** | **2** | **43** | [nao verificado] |

3. Durações apenas dos lotes confiáveis pelo critério solicitado; segundos por pergunta = duração ÷ (validadas + recusadas). [nao verificado]

| Período | Dia | Lote na lista do dia | Perguntas | Duração do lote | Duração por pergunta | Evidência |
|---|---|---:|---:|---:|---:|---|
| Antes de 2026-11-03 | 2026-10-20 | 2º | 6 | 6,0 s | 1,00 s | [nao verificado] |
| Antes de 2026-11-03 | 2026-10-21 | 2º | 2 | 1,3 s | 0,65 s | [nao verificado] |

Nos demais dias com linha `lote`, não há lote confiável pelo critério. A partir de **2026-11-03**, nenhum dos **10 lotes listados** tem cadeia igual a validadas + recusadas; portanto, não há duração confiável por lote ou por pergunta para comparar com o período anterior. [nao verificado]

## 11/11 12:51 · sessao 63a016840d88 · BO-ROTULO-ENCERRADO · verificador · origem=Code

**BLOQUEIA — a v3 fecha os exemplos anteriores nas respostas exercitadas, mas mantém escapes das duas classes: renderização em ramo não coberto e alteração por JS.** O próprio detector admite ignorar scripts sem `novo_status`; o detalhe GLPI é testado somente com erro na abertura do cliente. (`app/chamados/tests/test_bo_rotulo_encerrado.py:230`, `:305`)

As referências abaixo pertencem à cópia solicitada. Execução integral do selo, GETs Django e resultado em navegador: **[nao verificado]**.

**1. BLOQUEIA — os dois escapes não fecharam pela classe.**

**JS sem citar o nome do controle.** Este contraexemplo pode ficar após o seletor de `app/templates/chamados/detalhe_local.html:399`:

```html
<script>
document.querySelectorAll('select option').forEach(o => {
  if (['resolvido', 'fechado'].includes(o.value))
    o.textContent = 'Encerrado';
});
</script>
```

O HTML conferido conserva os textos corretos; o detector devolve `[]` porque esse script não contém `novo_status`. A varredura de templates e `.js` continua usando esse mesmo detector. Portanto, ampliar o censo de arquivos não elimina o escape. (`app/chamados/tests/test_bo_rotulo_encerrado.py:193`, `:235`, `:345`, `:348`, `:357`)

**JS citando o controle também escapa.** Trocar a atribuição por `o['textContent'] = 'Encerrado'`, mantendo o seletor com `novo_status`, evita a regex: ela reconhece `.textContent`, mas não acesso por colchetes. O detector retornou `[]` na reprodução em memória. (`app/chamados/tests/test_bo_rotulo_encerrado.py:222`, `:237`)

**Include em ramo GLPI vivo.** Contraexemplo hipotético, sem aplicá-lo à cópia: substituir cada chamada de `resolvido` e `fechado` em `app/templates/chamados/detalhe.html:133` por esta estrutura, ajustando o destino:

```django
{% if ticket %}
  {% include "chamados/partials/_escape_virtual.html" with destino="resolvido" %}
{% else %}
  {% opcao_mover "resolvido" %}
{% endif %}
```

Partial hipotético:

```django
<option value="{{ destino }}">{{ destino|espera|first }}</option>
```

Isso preserva as chamadas originais contadas por `DESTINOS`. O partial tem valor dinâmico, ignorado pelas verificações de opções literais; `espera` devolve “Encerrado” para os dois estados. (`app/chamados/tests/test_bo_rotulo_encerrado.py:50`, `:117`, `:134`; `app/chamados/services/verbos.py:118`)

A fixture mantém `ticket=None`, pois lança `GLPIError` no `__enter__`; a view também tem o caminho que preenche `ticket` com `buscar_ticket`. Assim, o caminho testado conserva as opções corretas, enquanto o caminho vivo pode entregar as duas opções pelo filtro inadequado. **Esse condicional não existe atualmente no template; trata-se de escape demonstrativo do selo.** (`app/chamados/tests/test_bo_rotulo_encerrado.py:305`; `app/chamados/views.py:238`, `:245`, `:263`; `app/templates/chamados/detalhe.html:132`)

Na reprodução por AST, em memória, passaram **7/7 verificações puras na base e 7/7 com o include virtual**: quatro testes estáticos e três testes dos detectores. Isso demonstra a evasão dessas verificações; selo Django integral verde: **[nao verificado]**. (`app/chamados/tests/test_bo_rotulo_encerrado.py:99`, `:103`, `:121`, `:131`, `:259`, `:266`, `:270`)

**FILA — alcance adicional:** os scanners não inspecionam Python, e o parser só acompanha seletores com `name="novo_status"`. Uma oferta dinâmica construída por view/form, em outra tela não renderizada pela fixture, permanece fora dessa observação. Controle assim existente nesta cópia: **[nao verificado]**. (`app/chamados/tests/test_bo_rotulo_encerrado.py:40`, `:164`, `:310`, `:352`)

Também não lê o conteúdo apontado por `script src`: coleta o corpo inline e eventos `on*`, depois percorre `app/static/**/*.js`. Há scripts externos nas telas, inclusive HTMX e Leaflet; seus conteúdos efetivamente entregues ficam **[nao verificado]**. (`app/chamados/tests/test_bo_rotulo_encerrado.py:163`, `:170`, `:179`, `:350`; `app/templates/base.html:19`; `app/templates/chamados/fio_colaborador.html:297`)

**2. OK para o censo dos cinco seletores encontrados; FILA para a cobertura dos demais controles.**

Os cinco seletores abaixo constam em `DESTINOS`, na inspeção estática e nos GETs. A fixture usa chamados abertos e usuário simultaneamente staff/superusuário; os POSTs não são exercitados. (`app/chamados/tests/test_bo_rotulo_encerrado.py:68`, `:131`, `:287`, `:292`, `:296`, `:327`)

| Seletor | Receptor e cobertura |
|---|---|
| `app/templates/chamados/detalhe.html:130` | GET com GLPI indisponível. O POST segue para followup, cujo receptor ignora `novo_status`. `app/chamados/tests/test_bo_rotulo_encerrado.py:305`; `app/templates/chamados/detalhe.html:122`; `app/chamados/views.py:292`. |
| `app/templates/chamados/detalhe_local.html:393` | GET aberto; POST delega à resposta livre. `app/chamados/views.py:547`; `app/chamados/services/acoes_chamado.py:287`. |
| `app/templates/chamados/fio_colaborador.html:265` | GET aberto; POST usa `processar_post_fio`. `app/chamados/views.py:674`; `app/chamados/services/fio.py:502`. |
| `app/templates/chamados/partials/modal_fio.html:665` | GET aberto; POST compartilha `processar_post_fio`. `app/chamados/views.py:953`. |
| `app/templates/chamados/partials/painel_chamado_turno.html:74` | GET aberto; formulário envia ao detalhe local. `app/templates/chamados/partials/painel_chamado_turno.html:68`; `app/chamados/services/acoes_chamado.py:313`. |

**FILA — os controles de ações abaixo alcançam mudança ou reconciliação de estado, mas seus efeitos não são cobertos pelo selo**, que verifica GET, opções e scripts. (`app/chamados/tests/test_bo_rotulo_encerrado.py:327`, `:341`)

| Controle | Caminho de mudança |
|---|---|
| Aceitar/recusar escala | `app/templates/chamados/detalhe_local.html:163`, `:169` → `app/chamados/services/acoes_chamado.py:149`: `resolvido`. |
| Aceitar/recusar agenda | `app/templates/chamados/detalhe_local.html:200`, `:209` → `app/chamados/services/acoes_chamado.py:108`: `resolvido`. |
| Aprovar exceção | `app/templates/chamados/detalhe_local.html:256`; `app/templates/chamados/partials/modal_fio.html:598` → `app/chamados/services/acoes_chamado.py:70`. |
| Autorizar geofence, advertir ou recusar | `app/templates/chamados/detalhe_local.html:285`; `app/templates/chamados/partials/modal_fio.html:506`, `:517`, `:528` → `app/chamados/services/acoes_chamado.py:222`, `:233`, `:277`. |
| Aprovar/rejeitar benefício, inclusive botões em JS | `app/templates/colaboradores/painel_solicitacao_beneficio.html:23`; `app/templates/colaboradores/fila_solicitacoes_beneficio.html:135` → `app/colaboradores/views.py:2068`: `resolvido`/`fechado`. |
| Abrir ausência | `app/templates/chamados/detalhe_local.html:233` → `app/chamados/views.py:1513`: `analisar()`. |
| Reabertura administrativa de disputa | `app/templates/chamados/partials/modal_fio.html:390` → `app/chamados/services/acoes_disputa.py:270`: `reabrir(humano=True)`. |
| Resgatar chamado | `app/templates/chamados/partials/_lista_chamados.html:85` → `app/chamados/views_arquivar.py:70`: `reabrir(humano=True)`. |
| Aceitar folga sugerida | `app/templates/chamados/partials/modal_fio.html:550` → `app/chamados/services/limbo_folgas.py:101`: `reconciliar()`. |
| Validar, validar apesar de implausível ou corrigir resposta | `app/templates/chamados/partials/modal_fio.html:265`, `:281`, `:303` → `app/chamados/services/acoes_disputa.py:13`, `:302`; reconciliação em `app/ponto/services/veredito_celula.py:202`. |
| Validar inline/em lote | `app/templates/chamados/partials/_lista_chamados.html:82`; `app/templates/chamados/partials/_lote_preview.html:21` → `app/chamados/views_cobrar.py:145`, `:232`, `:372`. |
| Regularizar disputa | `app/templates/chamados/detalhe_local.html:317`; `app/templates/chamados/partials/modal_fio.html:355` → `app/chamados/services/acoes_disputa.py:139`; reconciliação em `app/chamados/services/materializacao.py:587`, `:776`. |
| Iniciar disputa de órfãos/pedir confirmação de âncora | `app/templates/chamados/partials/modal_fio.html:628`, `:644` → `app/chamados/services/disputa_emissao.py:1159`, `:1379`: `analisar()`. |
| Salvar vínculo/realocar régua | `app/templates/colaboradores/partials/painel_vinculo.html:136`; `app/templates/chamados/partials/modal_fio.html:575` → `app/colaboradores/services/vinculo.py:329`; `app/colaboradores/services/pos_vinculo.py:42`. |
| Desligar colaborador | `app/templates/colaboradores/editar.html:271` → `app/colaboradores/services/desligamento.py:155`: `encerrar()`. |
| Regularizar retorno | `app/templates/ponto/regularizar_retorno.html:23` → `app/ponto/services/regularizacao.py:137`: `analisar()`. |
| Confirmar regularização externa | `app/templates/chamados/meu_atendimento.html:18` → `app/chamados/views_regularizacao.py:217` → `app/chamados/services/regularizacao_ext.py:205`: `encerrar()`. |
| Fechar/reabrir competência | `app/ponto/services/fechamento.py:1427`, `:1447` → `app/chamados/services/competencia_trancada.py:184`, `:265`. Controle HTML exato: **[nao verificado]**. |

“Aplicar declaração escrita” existe no modal; seu efeito específico sobre `status_local` fica **[nao verificado]**. “Reabrir questionário” atua nas perguntas; mudança de estado nesse caminho fica **[nao verificado]**. (`app/templates/chamados/partials/modal_fio.html:140`; `app/chamados/views.py:1435`; `app/chamados/services/acoes_disputa.py:103`)

Não foi encontrado outro seletor montado por JS, form/view Python ou registro administrativo; ausência absoluta de construção indireta: **[nao verificado]**. `app/chamados/admin.py` está vazio, sem linha citável **[nao verificado]**. O campo possui `choices`, o que isoladamente não demonstra formulário exposto. (`app/chamados/models.py:52`)

Há ainda mecanismos automáticos fora do selo, como sinais, adesão, cartório, cobertura e feriados. Completude de todos os efeitos indiretos do repositório: **[nao verificado]**. (`app/chamados/signals.py:42`; `app/chamados/services/adesao.py:92`, `:115`; `app/ponto/services/cartorio.py:1559`; `app/ponto/services/cobertura.py:122`; `app/chamados/services/feriado_reavaliar.py:47`)

**3. OK — as causas de passagem vazia perguntadas estão guardadas, se os testes forem executados.** [nao verificado]

- **302/403:** falham na exigência de HTTP 200; também se exige o template esperado. (`app/chamados/tests/test_bo_rotulo_encerrado.py:318`, `:319`)
- **Parser não encontrar o seletor:** falha em `len(sels) == 1`. (`app/chamados/tests/test_bo_rotulo_encerrado.py:332`)
- **`subTest` sem asserção:** contém a contagem do seletor e a comparação de seus defeitos. (`app/chamados/tests/test_bo_rotulo_encerrado.py:330`, `:332`, `:333`)
- **CNPJ invisível:** a fixture usa `11444777000161`, fora de `11222333*`; `Empresa.ativa` tem default verdadeiro. (`app/chamados/tests/test_bo_rotulo_encerrado.py:285`; `app/colaboradores/queries.py:85`; `app/colaboradores/models.py:44`)
- **Censos vazios:** há exigência de cinco templates, cinco entradas em `DESTINOS` e pelo menos um `.js`. Isso não garante completude dos ramos nem das sintaxes JS. (`app/chamados/tests/test_bo_rotulo_encerrado.py:101`, `:328`, `:358`, `:230`)

**4. OK — os contratos rastreados separam código, apresentação e filtro. Há duas desconexões de FILA.**

| Valor | Quem escreve → quem lê |
|---|---|
| `value` | Templates passam o código à tag; ela escreve o mesmo código. Os serviços leem `novo_status`, validam `STATUS_CHOICES` e chamam `transicionar`; modelo/motor decidem e `_gravar_estado` grava. `app/chamados/templatetags/chamados_tags.py:77`; `app/chamados/services/acoes_chamado.py:287`, `:313`; `app/chamados/services/fio.py:502`, `:719`; `app/chamados/models.py:123`, `:199`, `:228`. |
| Rótulo da opção | `MOVER_PARA` → `rotulo_de_movimento` → texto HTML; o selo compara com o helper. Nos receptores rastreados, texto não decide a transição. `app/chamados/services/verbos.py:132`, `:146`; `app/chamados/templatetags/chamados_tags.py:76`; `app/chamados/tests/test_bo_rotulo_encerrado.py:213`; `app/chamados/services/fio.py:721`. |
| `title` | `ESTADO_TITLE` → helper/tag → atributo HTML e comparação do selo. O mesmo mapa serve aos helpers de linha e espera. `app/chamados/services/verbos.py:100`, `:146`, `:155`, `:166`; `app/chamados/templatetags/chamados_tags.py:77`; `app/chamados/tests/test_bo_rotulo_encerrado.py:213`. |
| Filtro `espera` | Delega a `rotulo_para_pessoa`, que consulta `ESPERA_COLAB`; as pílulas leem texto/title da tupla. Continua devolvendo “Encerrado” para vários estados. `app/chamados/templatetags/chamados_tags.py:53`; `app/chamados/services/verbos.py:118`, `:162`; `app/templates/chamados/detalhe.html:36`. |
| Filtro de relatório `status` | O HTML oferece códigos; a view aplica `qs.filter(status_local=f_status)`. É consulta nesse caminho. `app/templates/relatorios/historico_chamados.html:54`; `app/relatorios/views.py:920`. |

**FILA — detalhe GLPI:** oferece mudança de status, mas envia ao receptor que lê apenas conteúdo e adiciona followup. O selo novo não testa esse POST. (`app/templates/chamados/detalhe.html:122`, `:130`; `app/chamados/views.py:292`, `:304`; `app/chamados/tests/test_bo_rotulo_encerrado.py:301`)

**FILA — “Reabrir”:** significa destino `aberto`; o motor comum recusa `resolvido → aberto`, permitindo `em_analise` nesse caso. Há exceção para `admin_ti`. (`app/chamados/services/verbos.py:136`; `app/chamados/catalogo/motor.py:102`, `:153`)

**5. Classificação das classes anteriores.**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando juiz registrado | **OK — não demonstrado no delta.** A tag consulta apresentação; a decisão de estado permanece no motor registrado, chamado pelo modelo. `app/chamados/templatetags/chamados_tags.py:76`; `app/core/juizes.py:274`; `app/chamados/models.py:123`. |
| Derivação paralela da fonte única | **OK — não demonstrada para os cinco seletores atuais.** Todos usam a tag/helper; `espera` mantém seu contrato próprio de acompanhamento. `app/templates/chamados/detalhe.html:132`; `app/templates/chamados/detalhe_local.html:395`; `app/templates/chamados/fio_colaborador.html:268`; `app/templates/chamados/partials/modal_fio.html:668`; `app/templates/chamados/partials/painel_chamado_turno.html:76`; `app/chamados/templatetags/chamados_tags.py:64`, `:76`. |
| Selo sem mordida suficiente | **BLOQUEIA.** Há mordida para os exemplos e fixtures, mas os contraexemplos do item 1 continuam aceitos pelas verificações correspondentes. `app/chamados/tests/test_bo_rotulo_encerrado.py:117`, `:230`, `:305`. |
| Relógio solto em teste novo | **FILA — indireto.** O GET do modal usa `localdate()` e competência corrente; a fixture cria timestamps automáticos, sem freeze no setup/GET. Falha temporal efetiva: **[nao verificado]**. `app/chamados/views.py:896`, `:903`; `app/chamados/models.py:67`; `app/chamados/tests/test_bo_rotulo_encerrado.py:282`, `:301`. |
| Campo com dois significados | **OK — não demonstrado como introdução desta obra.** Código, texto e title seguem separados. As desconexões GLPI/Reabrir permanecem em **FILA** pelos contratos acima. `app/chamados/templatetags/chamados_tags.py:77`; `app/chamados/services/acoes_chamado.py:313`; `app/chamados/views.py:292`; `app/chamados/catalogo/motor.py:102`. |

## 11/11 12:51 · sessao 857f5e1ba79e · LOTE-VALIDAR-REGRESSAO-0311 · sonda · origem=Code

Não executei uma nova sonda: `sonda_cliques.py` não foi localizada na cópia, e não há acesso a banco ou log conforme informado. Abaixo está a saída fornecida, integralmente, sem verificação independente. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda_cliques.py [af84c9b88dc29f70] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

linhas_lidas 2684
# dia | validou_lote_tela | ia_validou_lote | val_perg_disp | val_perg_nao_gravou | materializar_batida | vincular_pergunta_origem
dia 2026-10-20 2 0 54 5 98 98
dia 2026-10-21 2 0 52 7 28 28
dia 2026-10-22 1 0 54 2 37 37
dia 2026-10-23 0 0 76 7 55 55
dia 2026-10-24 0 0 1 0 0 0
dia 2026-10-25 0 0 2 0 0 0
dia 2026-10-26 1 0 170 9 144 144
dia 2026-10-27 0 0 73 13 37 37
dia 2026-10-28 11 0 72 8 60 60
dia 2026-10-29 6 0 48 6 41 41
dia 2026-10-30 12 0 27 25 21 21
dia 2026-10-31 0 0 1 0 0 0
dia 2026-11-01 0 0 1 0 0 0
dia 2026-11-02 0 0 2 0 0 0
dia 2026-11-03 2 0 90 12 79 79
dia 2026-11-04 0 0 1 1 0 0
dia 2026-11-05 2 0 30 2 25 25
dia 2026-11-06 2 0 39 3 38 38
dia 2026-11-07 0 0 1 0 0 0
dia 2026-11-08 0 0 1 0 0 0
dia 2026-11-09 0 0 18 5 14 14
dia 2026-11-10 0 0 12 3 9 9
dia 2026-11-11 4 0 113 31 95 95
lotes_sem_trilha_mensuravel 25
# dia | n_lotes | pedidas_mediana/max | validadas_soma | seg_mediana/p90/max | seg_por_pergunta_mediana | lotes>30s
lote 2026-10-20 2 7.5/9 15 13590.3/6.6/27173.9 2264.86 1 [('validou_lote_tela', 2)]
lote 2026-10-21 2 6.5/11 13 38075.2/20257.8/55892.6 7605.02 2 [('validou_lote_tela', 2)]
lote 2026-10-22 1 51/51 45 27362.7/27362.7/27362.7 608.06 1 [('validou_lote_tela', 1)]
lote 2026-10-26 1 125/125 107 281077.6/281077.6/281077.6 2626.89 1 [('validou_lote_tela', 1)]
lote 2026-10-28 11 15/47 52 16483.0/16483.0/78325.4 1498.45 3 [('validou_lote_tela', 11)]
lote 2026-10-29 6 12.0/31 28 5711.9/34.1/11389.7 570.44 2 [('validou_lote_tela', 6)]
lote 2026-10-30 12 29.0/33 4 2215.7/2215.7/2215.7 553.92 1 [('validou_lote_tela', 12)]
lote 2026-11-03 2 67.0/87 55 3487.4/94.8/6880.0 345.05 2 [('validou_lote_tela', 2)]
lote 2026-11-05 2 51.5/57 23 16.7/6.3/27.1 1.32 0 [('validou_lote_tela', 2)]
lote 2026-11-06 2 50.5/56 21 3999.8/18.1/7981.5 798.72 1 [('validou_lote_tela', 2)]
lote 2026-11-11 4 68.0/119 61 212158.8/6699.3/417618.2 4354.81 2 [('validou_lote_tela', 4)]
```

Nenhuma linha informa erro explícito ou universo geral vazio: a saída registra `linhas_lidas 2684`. [nao verificado]

Comparação das medianas com o dia anterior que possui linha `lote`, usando 2026-10-30 como referência inicial: [nao verificado]

| Dia | Mediana por lote (s) | Mediana por pergunta (s) | Verificação |
|---|---:|---:|---|
| 2026-11-03 | 3487.4 ↑ de 2215.7 | 345.05 ↓ de 553.92 | [nao verificado] |
| 2026-11-04 | Sem duração informada | Sem duração informada | [nao verificado] |
| 2026-11-05 | 16.7 ↓ de 3487.4 | 1.32 ↓ de 345.05 | [nao verificado] |
| 2026-11-06 | 3999.8 ↑ de 16.7 | 798.72 ↑ de 1.32 | [nao verificado] |
| 2026-11-07 | Sem duração informada | Sem duração informada | [nao verificado] |
| 2026-11-08 | Sem duração informada | Sem duração informada | [nao verificado] |
| 2026-11-09 | Sem duração informada | Sem duração informada | [nao verificado] |
| 2026-11-10 | Sem duração informada | Sem duração informada | [nao verificado] |
| 2026-11-11 | 212158.8 ↑ de 3999.8 | 4354.81 ↑ de 798.72 | [nao verificado] |

## 11/11 12:49 · sessao 2efb2e12f927 · - · - · origem=Ronald (tmux)

**BLOQUEIA — os exemplos nomeados fecharam; as classes continuam abertas.** O HTML entregue é conferido contra o mapa, mas só nas cinco fixtures e o detector JS depende de literal `novo_status` e de uma lista incompleta de sintaxes de escrita. `app/chamados/tests/test_bo_rotulo_encerrado.py:213`, `:230`, `:305`, `:327`.

1. **Include em ramo não exercitado — BLOQUEIA.** No lugar de cada chamada de `resolvido` e `fechado` em `detalhe.html:133`, usar, preservando a chamada original no `else`:

```django
{% if ticket %}
  {% include "chamados/partials/_escape_virtual.html" with destino="resolvido" %}
{% else %}
  {% opcao_mover "resolvido" %}
{% endif %}
```

E um partial com:

```django
<option value="{{ destino }}">{{ destino|espera|first }}</option>
```

Repetir para `fechado`. A fonte mantém as tags reconhecidas, enquanto o partial dinâmico não entra em `ESTADOS` literais; a fixture força GLPIError e mantém `ticket=None`. O caminho vivo existe na view, por `buscar_ticket`. `app/chamados/tests/test_bo_rotulo_encerrado.py:50`, `:52`, `:117`, `:134`, `:305`; `app/chamados/views.py:238`, `:245`, `:263`.

**Reprodução:** executei em memória, por AST, os quatro testes estáticos independentes de Django (`:99`, `:103`, `:121`, `:131`) e os três testes de mordida (`:259`, `:266`, `:270`): **7/7 verdes na base e 7/7 verdes com overlay virtual do include**. O scanner dos scripts da fonte também retornou lista vazia. Nenhum arquivo foi alterado. Compilação Django, GETs e suíte integral: **[nao verificado]**. [nao verificado]

2. **JS sem nome do controle — BLOQUEIA.** Este script, anexado após o seletor de `app/templates/chamados/detalhe_local.html:399`, não altera destinos nem a fonte das opções:

```js
document.querySelectorAll('select option').forEach(o => {
  if (['resolvido', 'fechado'].includes(o.value))
    o.textContent = 'Encerrado';
});
```

`escritas_em_opcao` retornou `[]` na execução em memória, por não encontrar `novo_status`. A própria docstring admite esse escape. `app/chamados/tests/test_bo_rotulo_encerrado.py:227`, `:230`, `:232`, `:235`. Execução em navegador: **[nao verificado]**.

3. **JS com nome, mas outra sintaxe — BLOQUEIA.** Mesmo citando `novo_status`, os sinks `o['textContent']='Encerrado'` e `o.setAttribute('label','Encerrado')` retornaram `[]`: a regex reconhece acessos por ponto e não reconhece `setAttribute`. `app/chamados/tests/test_bo_rotulo_encerrado.py:222`, `:237`. Consequência visual em navegador: **[nao verificado]**.

4. **Atributo `label` ignorado — BLOQUEIA quanto à leitura completa da option.** O parser guarda apenas `value`, `title` e conteúdo textual, ignorando `label`. HTML com `label="Encerrado"` nas duas opções e textos internos corretos produziu `defeitos_do_seletor=[]` na execução em memória. `app/chamados/tests/test_bo_rotulo_encerrado.py:168`, `:193`, `:213`. Semântica visual em navegador: **[nao verificado]**.

5. **Passar vazio pelas causas perguntadas — OK.** Resposta 302/403 falha em `status_code==200`; ausência do seletor falha em `len(sels)==1`; o `subTest` contém as duas asserções. `app/chamados/tests/test_bo_rotulo_encerrado.py:318`, `:319`, `:330`, `:332`, `:333`. CNPJ fixture `11444777000161` está fora do prefixo excluído e `Empresa.ativa` tem default verdadeiro: `app/chamados/tests/test_bo_rotulo_encerrado.py:285`; `app/colaboradores/models.py:44`; `app/colaboradores/queries.py:85`.

6. **Relógio solto indireto — FILA.** Não há chamada literal `now/today/localdate` no teste novo, mas a fixture cria `criado_em` por `auto_now_add`, e o GET do modal chama `janela_atual(localdate())`, sem freeze no setup ou GET. `app/chamados/models.py:67`; `app/chamados/tests/test_bo_rotulo_encerrado.py:282`, `:298`, `:301`; `app/chamados/views.py:896`, `:903`. Falha temporal efetiva desses testes: **[nao verificado]**.

7. **Perfil real — FILA.** O usuário de teste é simultaneamente staff e superusuário; não demonstra o ramo de um supervisor comum. `app/chamados/tests/test_bo_rotulo_encerrado.py:287`; `app/chamados/utils.py:47`, `:57`.

## 11/11 12:48 · sessao f176b06ccae6 · - · - · origem=Ronald (tmux)

**Censo dos controles de escolha direta:** encontrei os cinco abaixo; todos entram em `DESTINOS`, na comparação estática e no GET da fixture aberta/superusuário. Os POSTs e os demais perfis/ramos não são exercitados pelo selo. (`app/chamados/tests/test_bo_rotulo_encerrado.py:67`, `:127`, `:282`, `:327`)

| Controle | Receptor | Cobertura |
|---|---|---|
| `templates/chamados/detalhe.html:130` | `chamados/views.py:292` ignora `novo_status`; envia followup GLPI | GET apenas com GLPI indisponível: teste `:304` |
| `templates/chamados/detalhe_local.html:393` | `chamados/views.py:547` → `services/acoes_chamado.py:287`, `:313` | GET aberto + fonte |
| `templates/chamados/fio_colaborador.html:265` | `chamados/views.py:674` → `services/fio.py:502`, `:719` | GET aberto + fonte |
| `templates/chamados/partials/modal_fio.html:665` | `chamados/views.py:953` → mesmo `processar_post_fio` | GET aberto + fonte |
| `templates/chamados/partials/painel_chamado_turno.html:74` | POST a detalhe local, template `:68` → `acoes_chamado.py:313` | GET aberto + fonte |

Os cinco destinos são montados pela tag, sem choices Python nesses caminhos; os receptores recebem o código e aceitam sua pertinência a `STATUS_CHOICES`, delegando a `transicionar`. (`app/chamados/templatetags/chamados_tags.py:74`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:719`)

**Ações que podem mudar status sem oferecer códigos:** nenhuma tem seu efeito funcional coberto neste selo, que apenas faz GET e verifica opções/scripts. (`app/chamados/tests/test_bo_rotulo_encerrado.py:327`, `:341`)

| Ação | Controle → escrita/gate |
|---|---|
| Aceitar/recusar escala | `app/templates/chamados/detalhe_local.html:163`, `:169` → `app/chamados/services/acoes_chamado.py:149`, `transicionar('resolvido')` |
| Aceitar/recusar agenda | detalhe local `:200`, `:209` → ações `:108`, `resolvido` |
| Aprovar exceção | detalhe local `:256`, modal `:598` → ações `:70`, `resolvido` |
| Autorizar geofence, autorizar/advertir, recusar | detalhe local `:285`, `:286`, `:287`; modal `:506`, `:517`, `:528` → ações `:222`, `:233`, `:277` (`registrado→em_analise` preparatório, depois `resolvido`) |
| Aprovar/rejeitar benefício | `app/templates/colaboradores/painel_solicitacao_beneficio.html:23`; JS em `fila_solicitacoes_beneficio.html:135` → `app/colaboradores/views.py:2068`, `resolvido`/`fechado` |
| Abrir ausência | detalhe local `:233` → `app/chamados/views.py:1513`, `analisar()` |
| Reabrir disputa administrativamente | modal `:390` → `app/chamados/services/acoes_disputa.py:269`, `reabrir(humano=True)` |
| Resgatar chamado | `app/templates/chamados/partials/_lista_chamados.html:85` → `app/chamados/views_arquivar.py:70`, `reabrir(humano=True)` |
| Aceitar folga sugerida | modal `:550` → `app/chamados/services/limbo_folgas.py:98`, `:101`, `reconciliar()` |
| Validar resposta; validar apesar de implausível; corrigir resposta | modal `:265`, `:281`, `:303` → `app/chamados/services/acoes_disputa.py:13`, `:302` → `services/validacao.py:373`; efeitos por materialização/reconciliação, inclusive `app/ponto/services/veredito_celula.py:202` |
| Validar inline/lote | lista `:82`; `app/templates/chamados/partials/_lote_preview.html:21` → `app/chamados/views_cobrar.py:145`, `:232`, `:372` |
| Regularizar disputa | detalhe local `:317`, modal `:355` → `app/chamados/services/acoes_disputa.py:139`, `materializar_perguntas_validadas_da_disputa`; reconciliação em `services/materializacao.py:587`, `:776` |
| Aplicar declaração escrita | modal `:140`; efeito específico sobre status fica `[nao verificado]` |
| Iniciar disputa de órfãos / pedir confirmação de âncora | modal `:628`, `:644` → `app/chamados/services/disputa_emissao.py:1159`, `:1379`, `analisar()` |
| Salvar vínculo / realocar régua | `app/templates/colaboradores/partials/painel_vinculo.html:136`, modal `:575` → `app/colaboradores/services/vinculo.py:329`, `services/pos_vinculo.py:42`, `reconciliar()` |
| Desligar colaborador | `app/templates/colaboradores/editar.html:271` → `app/colaboradores/services/desligamento.py:155`, `encerrar()` |
| Regularizar retorno | `app/templates/ponto/regularizar_retorno.html:23` → `app/ponto/views.py:3153`, `app/ponto/services/regularizacao.py:137`, `analisar()` |
| Confirmar regularização externa | `app/templates/chamados/meu_atendimento.html:18` → `app/chamados/views_regularizacao.py:216`, `:217` → `app/chamados/services/regularizacao_ext.py:205`, `reg.chamado.encerrar()` |
| Fechar/reabrir competência | `app/ponto/services/fechamento.py:1427`, `:1447` → `app/chamados/services/competencia_trancada.py:184`, `:265`, encerramento por tranca/reabertura humana; controle HTML exato `[nao verificado]` |

“Reabrir questionário” em detalhe local `:9` e modal `:368` chega a `app/chamados/views.py:1372`, que atua nas perguntas; mudança de `status_local` nesse caminho não foi demonstrada `[nao verificado]`. A outra ação homônima também apenas chama `reabrir_para_correcao(qs)`. (`app/chamados/services/acoes_disputa.py:90`, `:103`)

Não encontrei outro seletor construído por JS/form/view Python ou registro administrativo de `ChamadoColaborador`; ausência absoluta de construção indireta `[nao verificado]`. `app/chamados/admin.py` está vazio, portanto não oferece linha citável `[nao verificado]`. O modelo tem `STATUS_CHOICES` no campo, mas isso sozinho não prova formulário exposto. (`app/chamados/models.py:52`)

Há fluxos automáticos adicionais de emissão/reconciliação, fora do selo, por exemplo adesão, cartório, cobertura, feriado e sinais; são mecanismos de estado, sem controle explícito de seleção do rótulo. (`app/chamados/services/adesao.py:92`, `:115`; `app/ponto/services/cartorio.py:1559`; `app/ponto/services/cobertura.py:122`; `app/chamados/services/feriado_reavaliar.py:47`; `app/chamados/signals.py:42`)

## 11/11 12:48 · sessao 673eae144ad1 · - · - · origem=Ronald (tmux)

**OK — contrato de apresentação separado do estado.** Os cinco templates passam códigos à tag; ela escreve o mesmo código em `value`, e obtém texto/title exclusivamente de `rotulo_de_movimento`. Esse helper consulta `MOVER_PARA` e `ESTADO_TITLE`. (`app/templates/chamados/detalhe.html:132`; `app/templates/chamados/detalhe_local.html:395`; `app/templates/chamados/fio_colaborador.html:268`; `app/templates/chamados/partials/modal_fio.html:668`; `app/templates/chamados/partials/painel_chamado_turno.html:76`; `app/chamados/templatetags/chamados_tags.py:76`; `app/chamados/services/verbos.py:146`)

| Valor | Escritor → leitor | Conclusão |
|---|---|---|
| `value` | Argumentos dos templates → tag → `POST['novo_status']` → pertinência a `STATUS_CHOICES` → `transicionar` → motor → gravação. (`app/chamados/templatetags/chamados_tags.py:77`; `app/chamados/services/acoes_chamado.py:287`; `app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/fio.py:502`; `app/chamados/services/fio.py:719`; `app/chamados/models.py:123`; `app/chamados/models.py:199`; `app/chamados/models.py:228`; `app/chamados/models.py:450`) | **OK:** nos receptores rastreados, o valor significa código de estado. (`app/chamados/models.py:124`) |
| Rótulo de movimento | `MOVER_PARA` → helper → texto da option → comparação do selo. (`app/chamados/services/verbos.py:132`; `app/chamados/services/verbos.py:146`; `app/chamados/templatetags/chamados_tags.py:77`; `app/chamados/tests/test_bo_rotulo_encerrado.py:213`) | **OK:** não participa da decisão de transição rastreada. (`app/chamados/services/acoes_chamado.py:315`; `app/chamados/services/fio.py:721`) |
| `title` | `ESTADO_TITLE` → helper/tag → atributo HTML; selo compara esse atributo. O mesmo mapa alimenta apresentação de linha e espera. (`app/chamados/services/verbos.py:100`; `app/chamados/services/verbos.py:146`; `app/chamados/services/verbos.py:155`; `app/chamados/services/verbos.py:166`; `app/chamados/templatetags/chamados_tags.py:77`; `app/chamados/tests/test_bo_rotulo_encerrado.py:213`) | **OK:** não é interpretado como comando nos receptores rastreados. (`app/chamados/services/acoes_chamado.py:287`; `app/chamados/services/fio.py:502`) |
| Filtro `espera` | Filtro → `rotulo_para_pessoa` → `ESPERA_COLAB`; badge usa posição 0 como texto e posição 1 como title. (`app/chamados/templatetags/chamados_tags.py:53`; `app/chamados/services/verbos.py:162`; `app/templates/chamados/detalhe.html:36`) | **OK:** continua permitido apresentar “Encerrado” para vários estados nas pílulas de acompanhamento; esse mapa tem contrato distinto do movimento. (`app/chamados/services/verbos.py:113`; `app/chamados/services/verbos.py:118`) |
| Filtro de relatório `status` | HTML escreve códigos → view aplica `qs.filter(status_local=f_status)`. (`app/templates/relatorios/historico_chamados.html:54`; `app/relatorios/views.py:920`) | **OK:** filtro de consulta, sem transição nesse trecho. (`app/relatorios/views.py:912`) |

**FILA — seletor GLPI promete mudança que seu receptor ignora.** O formulário oferece `novo_status`, mas envia ao followup; `adicionar_followup` lê `conteudo` e chama GLPI, sem processar estado. (`app/templates/chamados/detalhe.html:122`; `app/templates/chamados/detalhe.html:130`; `app/chamados/views.py:292`; `app/chamados/views.py:304`; `app/chamados/views.py:310`)

**FILA — “Reabrir” significa `aberto`, mas o motor comum recusa `resolvido → aberto`.** O destino permitido nesse caso é `em_analise`; existe exceção para `admin_ti`. (`app/chamados/services/verbos.py:136`; `app/chamados/catalogo/motor.py:102`; `app/chamados/catalogo/motor.py:153`)

**OK — fixture não some pelo CNPJ.** Usa `11444777000161`, fora de `11222333*`; `Empresa.ativa` nasce verdadeira. O usuário é staff/superuser, aceito como gestor e autorizado pelo gate departamental. (`app/chamados/tests/test_bo_rotulo_encerrado.py:285`; `app/colaboradores/queries.py:85`; `app/colaboradores/models.py:44`; `app/chamados/tests/test_bo_rotulo_encerrado.py:286`; `app/chamados/views.py:35`; `app/chamados/utils.py:47`; `app/chamados/utils.py:57`)

**BLOQUEIA — caminho GLPI vivo não exercitado pelo selo.** O mock lança no `__enter__`, portanto `ticket` fica no `None` inicial; não exercita `buscar_ticket`/`buscar_followups`. O template atual não precisa de ticket para mostrar o seletor, mas uma inclusão condicionada a ticket vivo não seria observada por esses GETs. (`app/chamados/tests/test_bo_rotulo_encerrado.py:305`; `app/chamados/views.py:238`; `app/chamados/views.py:243`; `app/templates/chamados/detalhe.html:117`; `app/templates/chamados/detalhe.html:130`)

**OK — não demonstrado segundo juiz de estado no delta.** O helper novo consulta apresentação; a decisão permanece no motor registrado em `core/juizes`. O modelo delega ao mesmo motor. (`app/chamados/services/verbos.py:146`; `app/core/juizes.py:274`; `app/chamados/models.py:123`)

**OK — não demonstrada derivação paralela do rótulo de movimento.** Todos os cinco seletores usam a tag, que delega ao helper; `espera` continua servindo outro contrato. (`app/chamados/templatetags/chamados_tags.py:75`; `app/chamados/templatetags/chamados_tags.py:63`; referências dos cinco templates acima)

Busca em arquivos `forms.py`/`admin.py` não encontrou oferta de `status_local`, `novo_status` ou registro de `ChamadoColaborador`; `app/chamados/admin.py` está vazio. Ausência absoluta de construção indireta de controles: **[nao verificado]**.

## 11/11 12:46 · sessao c5ab17cf6348 · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

**Não aprovo o fechamento da classe pelo v1.** O diff corrige o botão escondido no erro da câmera offline, mas preserva caminhos em que a captura impede chegar ao POST/fila. Também centraliza o **aviso**, sem centralizar a decisão de admissibilidade. (`app/templates/ponto/triagem/apto.html:150`, `:182–186`, `:253–256`; Δ `app/static/pwa.js:107–118`)

Uso **Δ** para linhas resultantes do diff anexo; as demais referências são da árvore. Ocorrência dos contraexemplos em produção e smoke de clique nas duas cascas: **[nao verificado]**.

**1. Quem decide, antes e depois**

O juiz registrado responde **“a ausência já é cobrável?”**: `origem == 'app' && !tem_foto && decorrido_min >= prazo`. PWA: zero minutos; APK/canal desconhecido: 90 minutos, com sobreposição permitida para APK/desconhecido. Ele não responde “devo impedir a batida?”. (`app/core/juizes.py:1192–1195`; `app/ponto/registro_batida.py:71–91`)

| Sítio/caminho | Predicado/comportamento na árvore | Depois do diff |
|---|---|---|
| `registrar_batida` | Não condiciona criação à foto; seus retornos antecipados tratam deduplicação, competência e colisão. | Igual. (`app/ponto/registro_batida.py:94–161`) |
| API `views.py` | `not foto` consulta `selfie_ausente(...tem_foto=False, decorrido_min=0)` e acrescenta cobrança; não veta por selfie. | Igual. (`app/api/views.py:436`, `:449–465`, `:665–680`) |
| API `views_core.py` | Mesmo predicado e efeito. | Igual. (`app/api/views_core.py:423`, `:436–452`, `:657–672`) |
| POST web, `ramo_registrar_es` | Grava primeiro; `foto_sessao or request.FILES.get("foto")` só condiciona anexação. | Igual; não consulta `selfie_ausente` no ato. (`app/ponto/services/triagem_batida.py:700–713`, `:748–765`) |
| Triagem **online** | `abrirCamera`: rejeição libera botão; sucesso exige chegar ao callback de `confirmarFoto.toBlob`. `refazerFoto` oculta novamente. `handleBatida` aceita `_fotoBlob` nulo quando alcançado. | Troca aviso; mantém esses controles locais. (`app/templates/ponto/triagem/apto.html:150–195`, `:229–235`; Δ mesmo arquivo`:156–164`) |
| Triagem **offline** | Mostra confirmar antes da câmera, mas o `catch` o esconde; persistência depende do callback `toBlob`. | Mantém confirmar; captura tem saída por timeout/conversão ou `null`, seguida de enfileiramento fora do callback. (`app/templates/ponto/triagem/apto.html:242–301`; Δ mesmo arquivo`:251–320`) |
| Legado **online** | Botões começam ocultos; falha de câmera oferece câmera do sistema sem liberá-los. Arquivo recebido ou captura processada libera; captura admite conversão nula. Refazer oculta. POST aceita ausência. | `catch` também libera os botões; restante permanece local. (`app/templates/ponto/bater_ponto.html:246`, `:343–477`, `:519–543`; Δ mesmo arquivo`:366–378`) |
| Legado **offline** | Confirmar fica disponível mesmo no erro; fila depende de `toBlob` chamar callback. | Fila sai do callback e recebe Blob ou `null` após tentativa limitada. (`app/templates/ponto/bater_ponto.html:564–610`; Δ mesmo arquivo`:581–637`) |
| V2 | `garantirRequisitos`: `!_fotoBlob` retorna `false`; `v2Confirmar` interrompe. `capturarFrame` fornece Blob/arquivo anterior/`null`. | Remove veto; mantém captura aguardada e agenda aviso. `abrirCamera` só acrescenta consulta textual ao aviso. (`app/templates/ponto/bater_ponto_v2.html:235–290`, `:312`, `:320`; Δ mesmo arquivo`:249`, `:283–296`, `:310`) |
| `turno_confirmar` | POST de entrada sem campo de foto/câmera. | Igual, sem consulta ao aviso. (`app/templates/ponto/triagem/turno_confirmar.html:8–24`) |
| `registrarBatidaOffline` | Aceita ausência: `foto: fotoBlob || null`. | Igual. (`app/static/pwa.js:107–122`) |
| `sincronizarBatidas` | Só anexa foto se `b.foto`; POST segue sem arquivo caso contrário. | Igual. (`app/static/pwa.js:143–156`) |

**Contagem pelo critério explícito de caminhos da tabela:** continuam **12 caminhos de execução** — seis de interface, quatro de servidor e dois de fila. Nas três telas com câmera, continuam **cinco fluxos locais**: triagem online/offline, legado online/offline e V2. O diff acrescenta um provedor de aviso; não transforma esses controles em um juiz único. As funções internas que liberam/ocultam os controles estão discriminadas acima. (Referências da tabela; Δ `app/static/pwa.js:107–118`)

`semSelfie()` é **nova fonte textual da política**, não leitor do juiz registrado: retorna uma constante, sem receber insumos nem consultar `selfie_ausente`; seus consumidores usam a string em `textContent`, `title` ou toast. Se apresentado como autoridade de admissibilidade, seria uma autoridade nova; operacionalmente, a decisão continua distribuída. Não é correto equiparar automaticamente essa pergunta à cobrança já registrada. (Δ `app/static/pwa.js:107–118`; Δ `app/templates/ponto/bater_ponto_v2.html:249`, `:289`; `app/ponto/registro_batida.py:80–91`)

**2. Escritores, leitores e formatos**

| Campo/valor | Quem escreve → quem lê; contrato |
|---|---|
| Foto da interface | Canvas produz Blob JPEG; input produz File; dataURL é preview ou intermediário convertido para Blob. `_fotoBlob` começa nulo. (`app/templates/ponto/triagem/apto.html:138`, `:174–183`; `app/templates/ponto/bater_ponto.html:391–403`, `:430–444`; `app/templates/ponto/bater_ponto_v2.html:146`, `:250–267`) |
| `fila.foto = null` | As telas chamam `registrarBatidaOffline`; ele é o escritor IndexedDB e já normaliza ausência para `null`. O diff não inaugura esse formato. (`app/static/pwa.js:107–122`; `app/templates/ponto/bater_ponto_v2.html:333`, `:346`; Δ `app/templates/ponto/triagem/apto.html:264–320`) |
| Foto presente na fila | `sincronizarBatidas` lê e usa `form.append('foto', b.foto, 'selfie.jpg')`: espera Blob/File quando truthy. Não converte string dataURL nesse leitor; `null` é simplesmente omitido. (`app/static/pwa.js:132–152`) |
| POST sem foto | V2/legado omitem anexação quando `_fotoBlob` é falso; triagem não preenche o input. API lê ausência por `request.FILES.get('foto')`. (`app/templates/ponto/bater_ponto_v2.html:318–320`; `app/templates/ponto/bater_ponto.html:530–531`; `app/templates/ponto/triagem/apto.html:229–235`; `app/api/views.py:436`; `app/api/views_core.py:423`) |
| Foto persistente | `Batida.foto` permite nulo. `anexar_foto` escreve pela porta; conforme `registro`, também cria/atualiza `FotoBatida`. API só o chama com foto presente. (`app/ponto/models.py:78`; `app/ponto/registro_batida.py:296–312`; `app/api/views.py:781–787`; `app/api/views_core.py:776–782`) |
| `foto_ausente` | É motivo/contexto de disputa, não booleano de `Batida`. APIs acrescentam motivo após consultar o juiz; encaminham à porta de disputa. O retro também emite pelo mesmo mecanismo. (`app/api/views.py:459–465`, `:767–779`; `app/api/views_core.py:446–452`, `:762–774`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:94–104`) |
| Leitores da cobrança | Retro lê `FotoBatida existe OR Batida.foto`; catálogo gera confirmação de presença; upload posterior procura o motivo e o apara/fecha, condicionado ao vínculo com `ultima_batida`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:70–77`; `app/chamados/catalogo/perguntas.py:93–98`; `app/api/foto_recebida.py:57–85`) |
| Sincronização | SW não lê foto nem envia a fila nesse tratador: delega `SYNC_BATIDAS` às páginas; sem página, notifica. PWA recebe a mensagem, sincroniza também em `online`/inicialização e exige perfil colaborador. (`app/static/sw.js:112–129`; `app/static/pwa.js:15–16`, `:128`, `:320–328`) |
| Remoção da fila | PWA apaga após resposta bem-sucedida ou HTTP 400; rede/5xx conservam o item. Ausência de foto não é condição de descarte. (`app/static/pwa.js:158–179`) |

**Não encontrei nesses leitores exigência de foto sempre presente.** Existe exigência de **tipo** quando presente: Blob/File no multipart. Os leitores de apresentação examinados também protegem `.url` com teste de existência. Isso não certifica qualquer consumidor externo à cópia. (`app/static/pwa.js:152`; `app/core/views.py:1005`; `app/colaboradores/services/detalhe.py:87`; `app/templates/core/_d2_feed.html:6–7`; consumidores externos: **[nao verificado]**)

Há uma lacuna preexistente de unidades/transporte: a fila grava ISO corrigido, ISO bruto, gap em milissegundos, precisão e instante GPS; o sincronizador envia o ISO bruto, mas não envia gap, precisão ou instante GPS. O juiz da selfie recebe tempo em **minutos**, calculado pelo retro com `/60`. (`app/static/pwa.js:85–93`, `:109–118`, `:137–152`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:74–77`)

**3. Checklist das classes reprovadas**

| Classe | Avaliação |
|---|---|
| Segundo juiz duplicando registrado | O juiz existente cobra ausência; o proposto devolve aviso de admissibilidade. Não são o mesmo predicado, mas o comentário “resposta única” não corresponde à unificação do controle. (`app/core/juizes.py:1192–1195`; `app/ponto/registro_batida.py:80–91`; Δ `app/static/pwa.js:107–118`) |
| Segundo escritor fora da porta | Não aparece no diff: fila continua no escritor IndexedDB existente; criação/anexação de Batida continuam na porta existente. (`app/static/pwa.js:107–122`; `app/ponto/registro_batida.py:94–161`, `:296–312`) |
| Relógio fora da borda | `semSelfie` não acrescenta relógio. Selfie recebe duração; API/retro capturam `agora` na borda. Permanecem horários de feedback obtidos diretamente por `new Date()`, diferentes do timestamp corrigido da fila; esse desvio já existia. (`app/ponto/registro_batida.py:80–91`; `app/api/views_core.py:496–507`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:63–77`; `app/templates/ponto/triagem/apto.html:283–287`; `app/static/pwa.js:109–115`) |
| Derivação paralela de fonte única | Os avisos passam a compartilhar texto; disponibilidade dos controles, espera da captura e envio continuam decididos em cada tela. Não há leitura de uma decisão única de admissibilidade. (Δ `app/static/pwa.js:107–118`; `app/templates/ponto/triagem/apto.html:182–195`; `app/templates/ponto/bater_ponto_v2.html:256–268`; Δ mesmo arquivo`:283–310`) |
| Selo/RED sem mordida suficiente | Há mordida contra os mutantes nomeados, mas o oráculo verifica tokens, não execução/alcance do POST. A renderização nas duas cascas também reaplica esse oráculo; não executa cliques. (Δ `app/ponto/tests/test_l030_forma_js.py:137–255`; Δ `app/ponto/tests/test_l030_templates_renderizados.py:39–61`) |
| Campo com dois significados | `foto_ausente` significa ausência **cobrável**: APK sem foto antes de 90 minutos retorna falso. Não deve ser interpretado como teste físico imediato de existência. Blob/dataURL/arquivo persistido são representações distintas nas bordas, com conversões explícitas nos produtores. (`app/ponto/registro_batida.py:80–91`; `app/ponto/tests/test_selfie_ausente_juiz.py:27–43`; `app/templates/ponto/bater_ponto_v2.html:262–267`; `app/static/pwa.js:152`) |

Dois mutantes passaram na reprodução Python em memória dos oráculos: inserir `await new Promise(function(){});` no início de `garantirRequisitos` passou pelos verificadores de selfie, aviso e GPS; trocar o retorno de `semSelfie` por `false` passou pelo verificador de resposta única. O primeiro impede qualquer envio; o segundo demonstra que o selo não verifica o valor retornado. São compatíveis com o que os verificadores inspecionam: ausência de certos tokens, presença do nome e primeiro token `return`. (`app/ponto/tests/test_l030_forma_js.py:31–68`; Δ mesmo arquivo`:207–255`; execução da suíte integral: **[nao verificado]**)

Outro mutante plausível é `throw new Error(...)` dentro de `if(!_fotoBlob)`: a verificação desse ramo proíbe `return`, não `throw`. A espera infinita já demonstra a insuficiência sem depender desse terceiro mutante. (Δ `app/ponto/tests/test_l030_forma_js.py:225–234`; execução desse terceiro mutante: **[nao verificado]**)

**4. Contraexemplos que sobrevivem**

| Caminho após o diff | Por que não chega à gravação |
|---|---|
| Triagem online: `getUserMedia` nunca resolve/rejeita | Botão começa oculto; espera não tem timeout; nem sucesso nem `catch` liberam o fluxo nessa hipótese. (`app/templates/ponto/triagem/apto.html:93`, `:148–164`) |
| Triagem online: `toBlob` nunca chama | Liberação do botão permanece exclusivamente dentro do callback de `confirmarFoto`, não alterado pelo diff. (`app/templates/ponto/triagem/apto.html:175–186`) |
| Legado: câmera pendente | Botões começam ocultos e `abrirCameraPre` aguarda sem timeout. Alterar o `catch` não cobre pendência. O legado, porém, não é selecionado pela view atual de batida. (`app/templates/ponto/bater_ponto.html:246`, `:354–378`; `app/ponto/views.py:356–358`) |
| V2: `toBlob` não chama e `toDataURL` lança no watchdog | O argumento `cv.toDataURL(...)` é avaliado fora do `try` do conversor; erro no timer não rejeita automaticamente a Promise original. Ela pode ficar pendente; o novo `.catch` não resolve pendência. Há também `done=true` antes da conversão no callback. (`app/templates/ponto/bater_ponto_v2.html:257–267`; Δ mesmo arquivo`:287`) |
| `pwa.js` não carregou, com necessidade de fila | Todas as telas dependem de `window.JulianiPWA.registrarBatidaOffline`. Isolar o toast da V2 não disponibiliza o escritor da fila. As bases comprovam inclusão do script, não seu carregamento bem-sucedido. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/templates/ponto/bater_ponto_v2.html:333`, `:346`; `app/templates/ponto/bater_ponto.html:543`, `:597`; `app/templates/ponto/triagem/apto.html:282`) |

Esses são contraexemplos condicionais pelo código; sua incidência real no dado fornecido é **[nao verificado]**. (`app/templates/ponto/triagem/apto.html:148–186`; `app/templates/ponto/bater_ponto_v2.html:257–267`)

Telas do fluxo não tocadas pelo diff:

| Tela | Mesma classe de câmera/selfie? |
|---|---|
| `triagem/turno_confirmar.html` | Não: registra nova entrada por formulário sem câmera/foto. Não possui fila local nesse formulário. (`app/templates/ponto/triagem/turno_confirmar.html:8–24`) |
| `triagem/turno_regularizar.html` | Não: formulário de regularização de saída por horário. (`app/templates/ponto/triagem/turno_regularizar.html:19–44`) |
| `triagem/fora_horario.html` | Não: solicita anuência por justificativa. (`app/templates/ponto/triagem/fora_horario.html:20–36`) |
| `regularizar_retorno.html` | Não: correção por horário/motivo, sem captura nesse formulário. (`app/templates/ponto/regularizar_retorno.html:23–57`) |
| `materializar_saida_retroativa.html` | Não: materialização administrativa por horário/motivo. (`app/templates/ponto/materializar_saida_retroativa.html:38–58`) |

A seleção das três telas alternativas de triagem está explícita na view; elas não pertencem ao mapa `TELAS_SELFIE` proposto. (`app/ponto/views.py:225–235`; Δ `app/ponto/tests/test_l030_forma_js.py:128–132`)

**5. `estadoPermissao`: zero chamador?**

**Na árvore, não:** há uma definição em `app/templates/ponto/bater_ponto_v2.html:281–284` e **um chamador** em `:288`. A busca textual integral da cópia encontrou somente essas duas ocorrências, nenhuma em outros templates, static ou testes. (`app/templates/ponto/bater_ponto_v2.html:281–288`)

**Após o diff, zero referências textuais remanescentes:** ele remove a definição e o único chamador juntos. Isso é conclusão estática; referências dinâmicas externas à cópia são **[nao verificado]**. (Δ `app/templates/ponto/bater_ponto_v2.html:280–296`)

## 11/11 12:46 · sessao 0a93f2497f9d · LOTE-VALIDAR-REGRESSAO-0311 · leitura · origem=Code

O código demonstra **execução síncrona com trabalho que se multiplica por pergunta e alcança outras perguntas do colaborador**. Há também uma duplicação concreta: criar a batida e depois vincular `pergunta_origem` salva a mesma batida duas vezes, repetindo seus receivers. Isso explica mecanismos capazes de encarecer o lote; a regressão de **10–122 s desde 03/11 e sua causa continuam [nao verificado]**. (`app/chamados/services/validacao.py:727`, `app/chamados/services/disputa_emissao.py:1904`, `app/chamados/services/materializacao.py:951`, `app/ponto/registro_batida.py:254`)

A cópia não oferece histórico Git consultável. Portanto, **não consigo provar o hunk introduzido por cada SHA**; abaixo separo o trabalho comprovado no código atual da associação histórica aos commits, que permanece **[nao verificado]**. [nao verificado]

**1. Caminho e multiplicadores**

As três entradas aguardam a mesma porta antes de responder: tela, JSON e confirmação pelo chat. (`app/chamados/views_cobrar.py:381`, `app/chamados/views_cobrar.py:386`, `app/chamados/views_cobrar.py:261`, `app/core/views_ia.py:169`, `app/core/views_ia.py:175`)

Na tabela, “SQL próprio” exclui o custo dos descendentes e comandos de controle transacional. As quantidades são condicionais aos ramos indicados; um total fixo de queries por pergunta, válido para qualquer lote, é **[nao verificado]**. Os ramos efetivamente executados dependem de materialização, respostas e estado das disputas. (`app/chamados/services/validacao.py:461`, `app/chamados/services/materializacao.py:1374`, `app/chamados/services/disputa_emissao.py:1916`)

| Estágio | Por pergunta / ativação | Por chamado e escala |
|---|---|---|
| Entrada → `validar_lote_classe_a` | As views não agrupam a execução em job; chamam a porta diretamente. (`app/chamados/views_cobrar.py:381`, `app/core/views_ia.py:169`) | Uma execução por clique; custos seguintes repetem no laço. (`app/chamados/services/validacao.py:727`) |
| Seleção inicial do lote | **1 SELECT para todo o lote**, trazendo apenas `disputa` por `select_related`; depois uma classificação por pergunta encontrada e ainda não validada. (`app/chamados/services/validacao.py:724`, `app/chamados/services/validacao.py:732`, `app/chamados/services/validacao.py:735`) | O(N) classificações; não há consolidação por chamado. A deduplicação com `n not in refs` tem pior caso **O(N²) em CPU**. (`app/chamados/services/validacao.py:719`, `app/chamados/services/validacao.py:727`) |
| `classe_do_lote` | Para hora legível: **1 `delta_da_resposta`**; dentro do envelope: **1 `_slot_esperado`**; havendo instante: **1 `exists()` de batida próxima**. (`app/chamados/services/validacao.py:864`, `app/chamados/services/validacao.py:872`, `app/chamados/services/validacao.py:875`) | O(1) chamadas por pergunta; O(N) no lote, mesmo repetindo colaborador/chamado. (`app/chamados/services/validacao.py:735`) |
| Delta / slot / grade | Delta acessa destino e pode consultar escala/tipo; slot de `hora` deriva novamente pelo plantio, que lê grade, vínculo e marcos. Defaults `00:00`/`07:00` acrescentam leitura da régua. (`app/chamados/juizes.py:1844`, `app/chamados/juizes.py:1792`, `app/chamados/juizes.py:372`, `app/chamados/services/materializacao.py:372`, `app/chamados/services/materializacao.py:389`, `app/chamados/services/materializacao.py:425`) | O(1) derivações por pergunta; grade percorre os dias pedidos, aqui **um dia**. (`app/chamados/services/materializacao.py:283`, `app/escala/services/leitor_celula.py:377`) |
| `acao_validar_pergunta` | Casca: **1 chamada de `validar_pergunta`**, sem consulta própria nesse despacho. (`app/chamados/services/acoes_disputa.py:18`, `app/chamados/services/acoes_disputa.py:19`) | Repete por pergunta admitida; não consolida por chamado. (`app/chamados/services/validacao.py:746`) |
| `validar_pergunta`: leitura e gates | **1 novo SELECT da pergunta**, sem reutilizar a instância classificada; **1 juiz de implausibilidade**, que resolve novamente o marco e seu fallback. (`app/chamados/services/validacao.py:389`, `app/chamados/services/validacao.py:404`, `app/chamados/juizes.py:1971`, `app/chamados/juizes.py:1974`) | O(1) chamadas por pergunta; repete nos itens do mesmo chamado. (`app/chamados/services/validacao.py:746`) |
| Trava antes da validação | **3 SELECT FOR UPDATE**: colaborador; disputas abertas mais alvos; chamados vivos/pais mais alvos. A chamada não passa `perguntas`, portanto não acrescenta o quarto SELECT explícito de trava de perguntas. (`app/chamados/services/validacao.py:431`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2691`) | Quantidade de SQL O(1), mas volume bloqueado O(disputas/chamados do colaborador), repetido por pergunta. Espera efetiva por trava: **[nao verificado]**. |
| `_carimbar_validacao` / decisões | **1 SELECT de decisões anteriores**, **1 save da pergunta**, **1 bulk INSERT de decisões**, mais outra emissão de evento de célula pelo registro das decisões. (`app/chamados/services/validacao.py:133`, `app/chamados/services/validacao.py:137`, `app/chamados/services/validacao.py:139`, `app/chamados/models.py:3927`, `app/chamados/models.py:3953`, `app/chamados/models.py:3954`) | O(1) operações por pergunta; sem agregação das decisões do lote nessa porta. (`app/chamados/services/validacao.py:435`) |
| `PerguntaDisputa.save` → O-OUTBOX | O override abre `atomic`. Save existente que toca campos relevantes acrescenta **1 SELECT da pergunta + 0–2 SELECTs de célula + 0–1 INSERT de evento**; no caso normal com célula encontrada: **2 SELECTs + 1 INSERT**. Nenhum juiz nesse emissor. (`app/chamados/models.py:3413`, `app/chamados/signals.py:251`, `app/chamados/models.py:3228`, `app/chamados/models.py:3241`, `app/chamados/models.py:3244`, `app/chamados/models.py:3248`) | O(1) por save individual. `vistos` só deduplica dentro daquela emissão, não entre saves/perguntas do lote. (`app/chamados/models.py:3238`) |
| Save da pergunta → beco | **1 callback por save**; ao executar, **2 SELECTs iniciais** — disputa/pai e prefetch de perguntas — mais possíveis FKs de destino e C7/cascatas. (`app/chamados/signals.py:227`, `app/chamados/signals.py:238`, `app/chamados/services/becos.py:72`, `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:33`) | O(perguntas da disputa) por callback; não há deduplicação por disputa na lista de callbacks. A proteção existente cobre reentrada durante o fechamento. (`app/chamados/services/becos.py:63`, `app/chamados/services/becos.py:70`) |
| Recálculo de fechamento | **1 agendamento por validação**; criar batida agenda outro. Cada execução recalcula a competência daquele colaborador. (`app/chamados/services/validacao.py:446`, `app/ponto/registro_batida.py:160`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:1824`) | O(dias da competência), além das batidas/vínculos processados; não é somente o dia corrigido. Há carregamento das células do período e processamento diário. (`app/ponto/services/fechamento.py:605`, `app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:269`) |
| `_materializar_uma` | Para hora ainda não materializada: **1 passagem explícita**, derivação do plantio, busca de batida, criação/reuso, reconciliação; depois **1 juiz de lastro**, **1 C6 em `carimbar`** e **outro juiz de lastro na lavra**. (`app/chamados/services/validacao.py:463`, `app/chamados/services/materializacao.py:851`, `app/chamados/services/materializacao.py:864`, `app/chamados/services/materializacao.py:1238`, `app/chamados/models.py:3139`, `app/chamados/veredito_lavrado.py:263`) | O(1) autoridades próprias por pergunta, **mais as cascatas** sobre chamado/colaborador descritas abaixo. (`app/chamados/services/materializacao.py:979`) |
| Save de `Batida` | Por save: **1 recompute** de d−2 até d+1; **2 chamadas ao cartório**, d−1 e d; **1 reconciliação do colaborador** e **1 fechamento de adesão**. (`app/ponto/signals.py:26`, `app/ponto/signals.py:108`, `app/ponto/signals.py:135`, `app/ponto/signals.py:215`, `app/ponto/signals.py:234`) | A janela do recompute é fixa, quatro dias; reconciliação alcança perguntas e chamados do colaborador. (`app/ponto/signals.py:27`, `app/chamados/reconciliador.py:1432`, `app/chamados/reconciliador.py:1441`) |
| Cartório | Por chamada: **0–1 SELECT inicial de célula**; se admitida, **1 `julgar_colab`**, cujo corpo pode rodar **1–3 vezes**. Cada corpo recarrega batidas/grade e pode carregar chamados com perguntas. (`app/ponto/services/cartorio.py:2011`, `app/ponto/services/cartorio.py:2013`, `app/ponto/services/cartorio.py:2026`, `app/ponto/services/cartorio.py:983`, `app/ponto/services/cartorio.py:1004`, `app/ponto/services/cartorio.py:1060`, `app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:1305`) | O(dias julgados), aqui uma célula por chamada, **mais o universo de chamados/perguntas carregado para o colaborador**. Não equivale a um save barato. (`app/ponto/services/cartorio.py:2026`, `app/ponto/services/cartorio.py:1305`) |
| Gate `_vivas_ancoradas` | **1 SELECT de ancoradas não validadas**; **1 C6 por ancorada em competência aberta**. Usa identity map e memo local de competência. (`app/chamados/models.py:334`, `app/chamados/models.py:344`, `app/chamados/models.py:354`, `app/chamados/models.py:360`, `app/chamados/models.py:368`) | O(perguntas ancoradas no chamado). `_gravar_estado` pode reutilizar a lista, mas rejulga C6; ao gravar, repete as três queries de trava do universo. (`app/chamados/models.py:216`, `app/chamados/models.py:224`, `app/chamados/models.py:290`) |
| S143 | Só entra em save de chamado encerrado que escreveu status. Repete **3 queries de trava**, faz **1 SELECT de perguntas do destino**, C6 por candidata fora de voo e depois **1 SELECT de todas as disputas abertas do colaborador**, com laços de perguntas e fechamento. (`app/chamados/signals.py:96`, `app/chamados/signals.py:104`, `app/chamados/services/disputa_emissao.py:1797`, `app/chamados/services/disputa_emissao.py:1823`, `app/chamados/services/disputa_emissao.py:1843`, `app/chamados/services/disputa_emissao.py:1904`) | O(perguntas das disputas abertas do colaborador), **por ativação S143**, não apenas por chamado encerrado. Há leituras adicionais de pai, destinos, refresh e C7. (`app/chamados/services/disputa_emissao.py:1909`, `app/chamados/services/disputa_emissao.py:1912`, `app/chamados/services/disputa_emissao.py:1913`, `app/chamados/services/disputa_emissao.py:1954`, `app/chamados/services/disputa_emissao.py:1963`) |
| Cauda de materialização e retorno | Depois da unidade, cada validação faz **1 SELECT de irmãs prontas**, tenta cada uma e executa fechamento da disputa; depois relê a pergunta no service e novamente no lote. (`app/chamados/services/validacao.py:469`, `app/chamados/services/materializacao.py:1374`, `app/chamados/services/materializacao.py:1380`, `app/chamados/services/materializacao.py:1383`, `app/chamados/services/validacao.py:472`, `app/chamados/services/validacao.py:752`) | O(perguntas prontas + perguntas da disputa) **por item do lote**. Irmãs que continuam prontas após recusa podem voltar às tentativas seguintes. (`app/chamados/services/materializacao.py:1374`, `app/chamados/services/materializacao.py:1381`) |

Um exemplo estático, **condicional**, da classificação: pergunta `hora` diferente de `00:00`/`07:00`, marco no destino, célula com geradora e previsto na ata, relações frias e sem alimentação externa: **6 consultas adicionais** — destino, colaborador, célula da grade, célula com geradora, DNA da célula e conflito de batida. Fallbacks podem aumentar; `data_hora` toma outro ramo. (`app/chamados/juizes.py:1844`, `app/chamados/services/materializacao.py:283`, `app/escala/services/leitor_celula.py:362`, `app/escala/servico_jornada.py:46`, `app/escala/models.py:1122`, `app/chamados/services/validacao.py:875`, `app/chamados/juizes.py:359`)

**C6 não custa sempre o mesmo:** tem memo por instância; em avaliação efetiva pode chamar **0–1 juiz de tranca e 0–2 `classificar_falta`**, conforme os degraus alcançados. Pergunta validada retorna cedo, mas a construção da chave ainda pode carregar a FK da célula. Portanto, não cabe multiplicar todas as chamadas C6 pelo antigo comentário de “8 queries”. (`app/chamados/juizes.py:841`, `app/chamados/juizes.py:890`, `app/chamados/juizes.py:1385`, `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1453`, `app/chamados/juizes.py:698`, `app/chamados/juizes.py:790`)

**O-OUTBOX não significa consumidor dentro desse request:** pergunta e recompute emitem eventos; o consumidor é chamado pelo comando agendado. Já cartório, reconciliação e callbacks de fechamento continuam no caminho descrito. (`app/chamados/models.py:3248`, `app/ponto/nucleo.py:64`, `app/ponto/nucleo.py:92`, `app/chamados/management/commands/consumir_eventos_celula.py:31`, `app/config/crons.py:394`, `app/ponto/signals.py:135`, `app/ponto/signals.py:215`)

**2. Commits: prioridade de investigação, sem atribuir hunks inexistentes** [nao verificado]

A tabela ordena **hipóteses para investigar**. A coluna de custo descreve o código atual; sua introdução pelo SHA indicado é **[nao verificado]**.

| Prioridade / commits anexados | Sítio atual correspondente e trabalho a conferir no diff | Relação com o início relatado | [nao verificado]
|---|---|---|
| **1 — `d8be2e408`**, O261-E-F2, 01/11 | `PerguntaDisputa.carimbar`: C6 aplicado ao carimbo; materialização pede também lavra/lastro. A cascata S143 usa essa porta para cada candidata. Acréscimo candidato: novo julgamento por carimbo, potencialmente multiplicado pelos laços. (`app/chamados/models.py:3139`, `app/chamados/models.py:3149`, `app/chamados/services/materializacao.py:1282`, `app/chamados/services/disputa_emissao.py:1937`) | Entre os candidatos anteriores a 03/11, é o encaixe mais direto com “juiz novamente por pergunta”; hunk, deploy e causalidade **[nao verificado]**. |
| **2 — `fe9736178`, `b2fba08fd`**, parcial/grade, 01/11 | Conferir mudança do universo de perguntas e da circulação do carimbo da ata. O cartório atual tem passadas adicionais após reconciliação/emissão, mas o custo acrescentado por esses dois commits é **[nao verificado]**. (`app/ponto/services/cartorio.py:975`, `app/ponto/services/cartorio.py:1004`, `app/ponto/services/cartorio.py:1060`) | Podem anteceder um deploy de 03/11 pelos dados anexados; associação causal **[nao verificado]**. |
| **3 — `d224a2bdc`, `0d6dd92b7`**, 06/11 | `PerguntaDisputa.save`, receiver de outbox e `acordar_celulas`: controle transacional e, normalmente, **+2 SELECTs +1 INSERT por save elegível**, sem dedupe entre saves. (`app/chamados/models.py:3413`, `app/chamados/signals.py:251`, `app/chamados/models.py:3228`, `app/chamados/models.py:3242`, `app/chamados/models.py:3248`) | Candidato ao custo **posterior**; não explica início literal em 03/11 pelas datas anexadas. Datas efetivas de deploy **[nao verificado]**. |
| **4 — `1bb14738a`, `e8b6753f1`, `82f06e6f5`, `fe5eeb487`, `ec06aa21a`, `7950f8273`, `44ae45edc` e revisões E1**, 07–08/11 | `_carimbar_validacao`/`DecisaoPergunta.registrar`: consulta de legado, INSERT de decisões e nova emissão de célula; `trava_na_ordem`: três SELECT FOR UPDATE do universo, repetidos nas portas. (`app/chamados/services/validacao.py:133`, `app/chamados/services/validacao.py:139`, `app/chamados/models.py:3953`, `app/chamados/models.py:3954`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`) | Podem ampliar custo/contenda após 07/11; divisão do acréscimo entre os SHAs **[nao verificado]**. |
| **5 — `a7384955a`**, 06/11 | `classe_do_lote`: o comentário do reparo noturno acompanha `_slot_esperado` no teste de conflito. Isso alcança plantio, grade e vínculo antes de validar. (`app/chamados/services/validacao.py:869`, `app/chamados/services/validacao.py:872`, `app/chamados/juizes.py:372`, `app/chamados/services/materializacao.py:283`, `app/chamados/juizes.py:317`) | Possível acréscimo de consultas por classificação desde seu deploy; hunk exato **[nao verificado]**. |
| **6 — `76626871f`, `e3ef579a6`**, 05/11 | Memo da tranca/toque: o código guarda somente toque positivo; negativo volta a consultar a trilha. Pode haver repetição por pergunta/dia trancado. (`app/chamados/juizes.py:1192`, `app/chamados/juizes.py:1205`, `app/chamados/juizes.py:1210`) | Trabalho depende de competências trancadas e ausência de toque; alteração atribuível a cada SHA **[nao verificado]**. |
| **7 — `156073abd`, `790648d84`**, 05/11 | Recompute emite evento quando algum turno aberto desaparece: consulta células dos dias fechados e insere eventos. **Não demonstra chamada síncrona ao consumidor.** (`app/ponto/nucleo.py:64`, `app/ponto/nucleo.py:86`, `app/ponto/nucleo.py:92`) | Acréscimo condicionado a fechamento de turno; hunk e impacto **[nao verificado]**. |

Não há fundamento para apontar o **identity map `4892a6aba`** como acréscimo de consultas: o helper atual reutiliza instâncias e não carrega colaborador ausente. O mesmo cuidado vale para atribuir regressão ao `37575d561`, cujo título declara alimentação única; seu delta real é **[nao verificado]**. (`app/chamados/juizes.py:1159`, `app/chamados/juizes.py:1168`, `app/chamados/juizes.py:1173`)

A revisão S143 associada no código à “volta 12” **restringe** o receiver aos saves que escrevem status; não demonstra trabalho novo nesse filtro. A projeção temporal atual do C6 é comparação de campos em memória, sem consulta própria. Atribuir regressão aos demais commits E2/E3/E4/H12 sem seus diffs seria **[nao verificado]**. (`app/chamados/signals.py:98`, `app/chamados/signals.py:104`, `app/chamados/juizes.py:1369`, `app/chamados/juizes.py:1371`)

**3. Repetição e validade de memo**

- **Batida nova sofre dois saves:** nasce sem `pergunta_origem`; depois `anotar` vincula a pergunta com outro save. No ramo correspondente, são **2 recomputes, 4 chamadas ao cartório e 2 reconciliações do colaborador**, além dos demais efeitos. Eliminar uma dessas execuções preservando toda a decisão exige análise própria; essa equivalência é **[nao verificado]**. (`app/chamados/services/materializacao.py:932`, `app/chamados/services/materializacao.py:951`, `app/ponto/registro_batida.py:254`, `app/ponto/signals.py:35`, `app/ponto/signals.py:135`, `app/ponto/signals.py:215`)

- **Mesmo lastro após o plantio:** `_materializar_uma` consulta `_pergunta_materializada_de_fato`; `carimbar(...lavrar_veredito=True)` consulta esse juiz novamente na lavra. É repetição demonstrada sobre a mesma pergunta; compartilhar o resultado nesse trecho, com validade limitada ao estado consultado, é candidato de reaproveitamento. Garantia perante concorrência sem definir essa validade: **[nao verificado]**. (`app/chamados/services/materializacao.py:1238`, `app/chamados/services/materializacao.py:1282`, `app/chamados/models.py:3150`, `app/chamados/veredito_lavrado.py:263`)

- **Mesma célula lida por leitores consecutivos:** classificação lê grade; ajuste noturno busca célula/geradora; marcos podem buscar novamente DNA. Os leitores já aceitam alimentação, permitindo transportar a leitura entre chamadas sem repetir SQL. Manter essa fotografia por todo o lote escritor, sem invalidação, é **[nao verificado]**. (`app/chamados/services/materializacao.py:283`, `app/escala/servico_jornada.py:46`, `app/escala/servico_jornada.py:77`, `app/escala/models.py:1122`, `app/escala/services/leitor_celula.py:353`)

- **Mesma disputa e mesmo colaborador reaparecem:** cauda de materialização, callbacks de beco e S143 voltam a consultar seus universos. No segundo laço amplo do S143 não há o identity map usado no primeiro. Há repetição; transformá-la em cache de resultado durante o lote exige considerar as escritas intervenientes. (`app/chamados/services/validacao.py:469`, `app/chamados/signals.py:238`, `app/chamados/services/disputa_emissao.py:1832`, `app/chamados/services/disputa_emissao.py:1906`, `a

[truncado em 20000 de 28675 caracteres]

## 11/11 12:42 · sessao c4188acce3bd · - · - · origem=Ronald (tmux)

**Classificação, custo e repetição comprovados no código atual.** `validar_lote_classe_a` carrega as perguntas em uma consulta com `select_related('disputa')`, reclassifica cada referência e recarrega a pergunta após a ação; não transporta `carga` nem identity map para a classificação (`app/chamados/services/validacao.py:724`, `app/chamados/services/validacao.py:735`, `app/chamados/services/validacao.py:752`).

| Estágio | Contagem estática condicional | Complexidade / evidência |
|---|---|---|
| `classe_do_lote` | Para pergunta de hora legível: 1 `delta_da_resposta`; se dentro do envelope: 1 `_slot_esperado`; se instante existe: 1 `batidas_apuraveis(...).exists()` | O(1) por pergunta, O(N) chamadas no lote (`app/chamados/services/validacao.py:864`, `app/chamados/services/validacao.py:872`, `app/chamados/services/validacao.py:875`). |
| `delta_da_resposta` | 1 leitura lazy de `chamado_destino` se não carregado; 1 `_marco_faltante`; sem marco: 1 `_marco_esperado_da_grade` | Não tem memo. O fallback executa 1 consulta `EscalaColaborador`, seguida de acesso lazy a `tipo_escala` se houver vínculo (`app/chamados/juizes.py:1844`, `app/chamados/juizes.py:1846`, `app/chamados/juizes.py:1792`, `app/chamados/juizes.py:1797`). |
| `_slot_esperado` | Para `hora`: 1 `_ts_tipo_do_plantio`; o cache de slots só existe quando recebe `carga`, que a classificação não passa | O(1) por pergunta; `data_hora` é parse puro e retorna antes do plantio (`app/chamados/juizes.py:350`, `app/chamados/juizes.py:359`, `app/chamados/juizes.py:372`). |
| `_ts_tipo_do_plantio` | 1 `_tipo_pelo_marco`; 1 `_ajustar_saida_overnight`; resposta `00:00`/`07:00` acrescenta 1 `_marcos_da_regua` | O(1) chamadas por pergunta; o ramo de defaults repete leitura da grade (`app/chamados/services/materializacao.py:372`, `app/chamados/services/materializacao.py:389`, `app/chamados/services/materializacao.py:425`). |
| `_tipo_pelo_marco` | 1 busca em `contexto_json`; com hora/data válidas, 1 `grade_da_celula` **mesmo quando já achou tipo no chamado** | A segunda fonte é consultada para conferir divergência, portanto removê-la alteraria a observação atual (`app/chamados/services/materializacao.py:273`, `app/chamados/services/materializacao.py:280`, `app/chamados/services/materializacao.py:283`, `app/chamados/services/materializacao.py:301`). |
| `grade_da_celula` | Sem alimentação: 1 consulta CelulaDia; mais 1 consulta de todos os vínculos se dia trabalha e ata não tem `minutos_previstos` | O(D) no intervalo solicitado; aqui D=1. O vínculo extra é carregado uma vez por chamada, não por lote (`app/escala/services/leitor_celula.py:362`, `app/escala/services/leitor_celula.py:377`, `app/escala/services/leitor_celula.py:406`, `app/escala/services/leitor_celula.py:408`, `app/escala/services/leitor_celula.py:444`). |
| `_ajustar_saida_overnight` | 1 `escala_vigente`; se vínculo/tipo existem, 1 `marcos_turno` | `escala_vigente` faz 1 consulta CelulaDia com geradora/tipo; sem geradora consulta também vínculos. Depois `marcos_turno` chama `ec.marcos_do_dia` sem repassar as células carregadas, custando outra consulta DNA fora do contexto alimentado (`app/chamados/juizes.py:317`, `app/chamados/juizes.py:320`, `app/escala/servico_jornada.py:46`, `app/escala/servico_jornada.py:64`, `app/escala/servico_jornada.py:77`, `app/escala/models.py:1122`). |
| Gate implausibilidade da validação | 1 pergunta `.get`; repete resolução de marco feita pelo delta da classe: `_marco_faltante` + fallback `_marco_esperado_da_grade` | Outra instância da pergunta, sem reaproveitamento do delta; O(1) por pergunta (`app/chamados/services/validacao.py:389`, `app/chamados/services/validacao.py:404`, `app/chamados/juizes.py:1971`, `app/chamados/juizes.py:1974`). |
| `data_do_fato_de` | Tem memo `_dfd_memo` **por instância**; reutiliza destino se já é o alvo, senão consulta chamado; `dia_do_fato(fortes=True)` pode carregar célula lazy | Não é fonte de repetição ilimitada na mesma instância; instâncias novas perdem esse memo (`app/chamados/services/fio.py:74`, `app/chamados/services/fio.py:80`, `app/chamados/services/fio.py:83`, `app/chamados/services/fio.py:86`, `app/chamados/services/fio.py:93`, `app/chamados/juizes.py:98`). |

**Exemplo de contagem concreta, condicional:** na classificação de uma pergunta classe A, `hora` diferente de `00:00`/`07:00`, com marco no destino, geradora na célula, ata com previsto, sem contexto alimentado: **6 consultas** adicionais à consulta inicial do lote — destino, colaborador, grade/CelulaDia, CelulaDia com geradora, DNA CelulaDia e conflito/exists. Não é total universal: defaults/fallbacks acrescentam consultas; `data_hora` não executa o plantio no `_slot_esperado` (`app/chamados/services/validacao.py:724`, `app/chamados/juizes.py:1844`, `app/chamados/services/materializacao.py:283`, `app/escala/services/leitor_celula.py:362`, `app/escala/servico_jornada.py:46`, `app/escala/models.py:1122`, `app/chamados/services/validacao.py:875`, `app/chamados/juizes.py:359`).

**Memo seguro não deve ser prometido indiscriminadamente.** Leitores de células e batidas podem observar alterações feitas pelas próprias validações entre itens; o código declara que o recalculo roda duas vezes de propósito quando nasce batida, e guarda somente “SIM” no memo de toque porque “NÃO” pode mudar no mesmo request (`app/chamados/services/validacao.py:439`, `app/chamados/juizes.py:1192`). É demonstrável o trabalho repetido de leitura da mesma célula dentro da classificação antes de qualquer escrita local; preservar decisões perante concorrência e ao longo do lote inteiro exige delimitar validade/invalidação [nao verificado] (`app/chamados/services/materializacao.py:283`, `app/chamados/juizes.py:317`, `app/escala/models.py:1122`).

**História disponível, sem hunks verificáveis.** Não há `.git` nesta cópia; atribuir exatamente cada alteração atual ao SHA dos anexos permanece [nao verificado].

- O helper N1 atual apenas troca instâncias de mesmo PK; não carrega colaborador ausente e tem custo zero de query próprio. Seu objetivo é reaproveitar memo da tranca fora de request; não é candidato demonstrado a acréscimo de custo por pergunta (`app/chamados/juizes.py:1159`, `app/chamados/juizes.py:1163`, `app/chamados/juizes.py:1168`, `app/chamados/juizes.py:1173`). O registro documental associa `4892a6ab` ao corte de consultas na sombra e deploy em 04/11 20:2x; isso não é medição atual de prod (`app/docs/RELATO.md:1063`, `app/docs/RELATO.md:1068`).
- A classificação atual tem comentário explícito BUG-PLANTIO-NOTURNO-MAIS-1 junto da chamada nova de `_slot_esperado`, candidata a acréscimo de derivação/consultas por pergunta; atribuição ao hunk de `a7384955a` é [nao verificado] (`app/chamados/services/validacao.py:869`, `app/chamados/services/validacao.py:872`).
- O registro documental diz que cascata `signals → becos → disputa_emissao → materializacao` antecedia a E1 e cita `d8be2e40` entre seus ancestrais; não prova sozinho qual hunk nem quanto custava em 03/11 (`app/docs/RELATO.md:517`).
- Registro documental do pouso O261-E-F2 aponta 01/11 18:50; relacioná-lo a duração desde 03/11 exige dado de deploy e latência não fornecido [nao verificado] (`app/docs/RELATO.md:7191`).
- Documento da LASTRO-PARTE-3 relaciona `d224a2bd` ao `post_save` filtrado por campos, com emissão de evento conservadora para `update_fields=None`; confirmar diff SHA exato permanece [nao verificado] (`app/docs/RELATO.md:855`).

**Predicados de contagem para separar o custo da classificação:** no conjunto `PerguntaDisputa.objects.filter(pk__in=refs)` medir grupos `(disputa__colaborador_id, data_evento, motivo, tipo_resposta)` e contar `N`, `N_distinct_colab`, `N_distinct_colab_dia`, defaults `resposta_colab__in=['00:00','07:00']`; medir destino nulo e destinos cujo `contexto_json.marcos_faltantes` não resolve o motivo pela função real, pois filtro JSON aproximado não reproduz o fallback legado (`app/chamados/services/validacao.py:724`, `app/chamados/services/materializacao.py:372`, `app/chamados/juizes.py:539`). Nos mesmos pares `(colaborador_id,data_evento)`, contar `CelulaDia` sem `escala_geradora_id` e atas sem `minutos_previstos`; são os ramos que exigem alimentação adicional (`app/escala/servico_jornada.py:62`, `app/escala/servico_jornada.py:64`, `app/escala/services/leitor_celula.py:399`, `app/escala/services/leitor_celula.py:408`).

## 11/11 12:42 · sessao e622ae7e1ea2 · - · - · origem=Ronald (tmux)

- **Pergunta.save:** 1 `UPDATE` + controle transacional; não há juiz diretamente no override (`app/chamados/models.py:3413`). Cada save existente dispara callback de beco; se toca `CAMPOS_QUE_ACORDAM`, também emite outbox (`app/chamados/signals.py:227`, `app/chamados/signals.py:251`).

- **Outbox por save elegível:** 1 SELECT de `(celula_id,colaborador_id,data_evento)` + 1 SELECT da célula por FK, eventualmente outro pelo par colaborador/dia + 0–1 INSERT de evento. Portanto **2–3 SELECTs + 0–1 INSERT**, além do UPDATE da pergunta; sem chamada de juiz nesse trecho (`app/chamados/models.py:3225`, `app/chamados/models.py:3238`, `app/ponto/portas/celula.py:895`). O conjunto `vistos` dura apenas essa chamada; dois saves da mesma pergunta podem inserir dois eventos da mesma célula (`app/chamados/models.py:3238`).

- **Consumidor não roda no request por esse mecanismo:** `recompute_turnos` apenas calcula os turnos fechados e chama `_emitir_eventos_dos_turnos_fechados`, que lê células e insere eventos (`app/ponto/nucleo.py:64`, `app/ponto/nucleo.py:86`). A chamada de produção a `consumir_eventos_celula()` está no management command, agendado por minuto (`app/chamados/management/commands/consumir_eventos_celula.py:31`, `app/config/crons.py:394`). O título de `156073abd` não demonstra execução síncrona do consumidor; o código atual demonstra emissão de evento (`app/ponto/nucleo.py:60`).

- **Beco por save de pergunta:** callback `on_commit` chama `fechar_beco_da_disputa`; ela relê Disputa com chamado e prefetch de perguntas: **2 SELECTs iniciais** quando a disputa existe (`app/chamados/signals.py:233`, `app/chamados/services/becos.py:72`). `classificar` percorre todas as perguntas; para cada não respondida, acessar `chamado_destino` pode gerar consulta adicional, porque esse relacionamento não é prefetched (`app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:73`). Se o beco for fechável, entram C7, resolver, travas e fechamento; custo variável **O(perguntas da disputa)** e cascatas (`app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:51`).

- **Carimbar:** chama C6 uma vez após escrever o carimbo em memória; se C6 disser viva, desfaz e não salva (`app/chamados/models.py:3128`, `app/chamados/models.py:3139`). Com `lavrar_veredito=True`, chama também `_pergunta_materializada_de_fato` via `lavrar(salvar=False)` e faz **um único save conjunto**, que paga signals/outbox/beco (`app/chamados/models.py:3149`, `app/chamados/models.py:3152`, `app/chamados/veredito_lavrado.py:262`). `lavrar(salvar=True)` isolado faz um save apenas dos três campos da lavra: dispara beco, mas o filtro do outbox não aceita esses campos (`app/chamados/veredito_lavrado.py:274`, `app/chamados/signals.py:255`).

- **Duplicação confirmada no plantio:** depois de criar/reusar Batida, o materializador chama `anotar(... pergunta_origem ...)` quando a FK difere (`app/chamados/services/materializacao.py:951`). `anotar` chama **`batida.save(update_fields=...)`**, exceto quando todos os campos já têm os valores pedidos (`app/ponto/registro_batida.py:248`, `app/ponto/registro_batida.py:254`). Logo, uma Batida nova sem a FK sofre INSERT e depois UPDATE, com os receivers em ambas as vezes (`app/chamados/services/materializacao.py:935`, `app/chamados/services/materializacao.py:951`).

- **Cada save de Batida:** um recompute na janela inclusiva **d−2..d+1**, duas chamadas `cartorio.julgar_celula` (**d−1 e d**), uma chamada `reconciliar_chamados_colab` e uma `fechar_adesao_com_batida` (`app/ponto/signals.py:26`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:214`, `app/ponto/signals.py:233`). Portanto o INSERT seguido de `anotar` pode pagar **2 recomputes, 4 chamadas ao cartório e 2 reconciliações do colaborador**, antes de somar chamadas explícitas posteriores (`app/ponto/registro_batida.py:254`, `app/ponto/signals.py:35`).

- **Cartório por chamada:** 1 SELECT de célula; pode retornar sem julgar por futuro, inexistência ou turno atual ainda aberto (`app/ponto/services/cartorio.py:2011`). Se prossegue, chama `julgar_colab` uma vez e depois precedência (`app/ponto/services/cartorio.py:2026`, `app/ponto/services/cartorio.py:2032`). `julgar_colab` executa o corpo **1–3 vezes**: inicial, segunda passada para reconciliadas e releitura após emissão (`app/ponto/services/cartorio.py:983`, `app/ponto/services/cartorio.py:1004`, `app/ponto/services/cartorio.py:1060`). Cada corpo refaz batidas, grade e primeira batida; o ramo com adesão ainda lê **todos os chamados de FURO_MODULES do colaborador + prefetch de perguntas**, sem filtro temporal nessa carga (`app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:1189`, `app/ponto/services/cartorio.py:1305`, `app/ponto/services/cartorio.py:117`). O skip por impressão ocorre depois dessas cargas e do cálculo de fio vivo (`app/ponto/services/cartorio.py:1375`, `app/ponto/services/cartorio.py:1399`).

- **Gate do chamado:** `_ancoradas()` faz **1 SELECT** das perguntas não validadas ancoradas; `_vivas_entre()` chama C6 por pergunta em competência aberta, com identity map e memo de competência locais (`app/chamados/models.py:344`, `app/chamados/models.py:354`). `_gravar_estado` pode reutilizar a lista, mas rejulga C6; ao gravar, toma **3 SELECT FOR UPDATE** do universo do colaborador, além da trilha e save (`app/chamados/models.py:216`, `app/chamados/models.py:224`, `app/chamados/models.py:289`, `app/chamados/models.py:2665`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`). Ao cruzar a fronteira e existir alvo, relê perguntas de destino e lavra uma a uma (`app/chamados/models.py:303`).

- **S143 por save de status encerrado:** só roda quando o save escreveu status (`app/chamados/signals.py:96`, `app/chamados/signals.py:104`). Toma novamente as **3 SELECT FOR UPDATE**; lê perguntas não materializadas do destino com disputa aberta; chama C6 nas não em voo; depois lê **todas as disputas abertas do colaborador** e percorre suas perguntas, inclusive com refresh e C7 por disputa (`app/chamados/services/disputa_emissao.py:1797`, `app/chamados/services/disputa_emissao.py:1823`, `app/chamados/services/disputa_emissao.py:1832`, `app/chamados/services/disputa_emissao.py:1904`, `app/chamados/services/disputa_emissao.py:1912`, `app/chamados/services/disputa_emissao.py:1951`). Seu custo variável alcança **perguntas/disputas do colaborador**, além do chamado fechado (`app/chamados/services/disputa_emissao.py:1904`).

## 11/11 12:42 · sessao c36657fac3ae · - · - · origem=Ronald (tmux)

Achados confirmados para consolidar:

| Estágio | Queries/chamadas e escala |
|---|---|
| `ChamadoColaborador._vivas_ancoradas(novo)` | Faz `_ancoradas()` uma vez: **1 SELECT** de perguntas não validadas ancoradas por destino ou pai, com disputa/colaborador/empresa/célula em `select_related`; depois **1 C6 por pergunta cuja competência está aberta**. Escala O(perguntas ancoradas no chamado), não O(1) por chamado. `app/chamados/models.py:334`, `:344`, `:346`, `:360`, `:364`, `:368`. |
| Cerca de competência | `competencia_do_dia_entregue` tem memo **local à invocação de `_vivas_entre`**, por empresa/mês/ano; uma chamada efetiva a `competencia_entregue` por chave nessa invocação. O memo reinicia no gate seguinte. `app/chamados/models.py:354`; `app/chamados/services/vigia_fio.py:839`, `:842`. |
| Tripwire `_gravar_estado` | Pode reutilizar lista `_f1_ancoradas` do gate e **rejulgar C6 sem rebuscar**. Se não recebeu lista, rebusca. Se memória diz terminal, ainda pode fazer **1 SELECT de status atual**. `app/chamados/models.py:216`, `:219`, `:223`, `:224`. |
| `_gravar_estado` travas | Chama `trava_na_ordem(...abertas_do_colaborador=True)` antes de salvar: **1 SELECT FOR UPDATE Colaborador + 1 Disputas abertas + 1 Chamados vivos/pais/alvo**, independentemente da quantidade de linhas; volume travado O(disputas/chamados do colaborador). Reentrância ainda executa SQL. `app/chamados/models.py:290`, `:2666`, `:2675`, `:2688`. |
| Relavra S2 do chamado | Só se cruzou fronteira encerrado e existe alvo ou não veio lista do gate; **1 SELECT** de perguntas não validadas cujo destino é o chamado, **1 `_lavrar_s2` por pergunta**. `app/chamados/models.py:273`, `:303`, `:309`, `:312`. |
| Receiver S143 | Roda somente se chamado encerrado e save escreveu `status_local`; usa savepoint e chama resolver. Saves só de campo com `update_fields` sem status não entram. `app/chamados/signals.py:96`, `:104`, `:114`, `:115`. |
| Resolver S143, primeiro laço | Repete as **3 queries de trava do universo do colaborador**, faz **1 SELECT** das perguntas do destino não materializadas em disputa aberta; **1 C6 por pergunta fora de voo**, depois até **1 tranca adicional**, até **1 `_pergunta_materializada_de_fato`** para resposta e até **1 C6 dentro de `carimbar`**. Identity map existe neste laço. `app/chamados/services/disputa_emissao.py:1797`, `:1823`, `:1832`, `:1837`, `:1843`, `:1847`, `:1856`, `:1863`; `app/chamados/models.py:3139`. |
| Resolver S143, varredura ampliada | **1 SELECT de todas disputas abertas do colaborador**; por candidata com pai não vivo, **1 SELECT de todas perguntas** e até **1 FK destino por pergunta** até o `any` encerrar; para perguntas abertas fora de voo **1 C6 cada** e possível novo C6 no carimbo. **Este laço não usa identity map**. Escala O(perguntas das disputas abertas do colaborador), a cada S143. `app/chamados/services/disputa_emissao.py:1904`, `:1906`, `:1909`, `:1912`, `:1913`, `:1921`, `:1925`, `:1937`. |
| Resolver S143, fecho de candidatas | Por disputa candidata: **1 refresh**, se aberta **1 SELECT perguntas para voo**, depois `pode_fechar`: **1 SELECT perguntas para respostas sem veredito** e, se não encontrou resposta impeditiva, **1 SELECT perguntas + C6 até primeira viva**. `fechar` pode repetir esses juízos. `app/chamados/services/disputa_emissao.py:1951`, `:1954`, `:1961`, `:1963`, `:1964`; `app/chamados/models.py:1842`, `:1853`, `:1886`. |

C6 não está “sem memo”: `pergunta_viva` memoriza **por instância da pergunta**, com chave de campos mutáveis, data e instante; carga explícita pula esse memo. Instância nova perde reutilização; alteração da chave pede julgamento novo. Até a chave lê `p.celula`, podendo custar FK quando fria, mesmo se o corpo retornará por `validada_em`. `app/chamados/juizes.py:841`, `:851`, `:888`, `:890`, `:902`, `:1385`.

No corpo C6, as chamadas de autoridade são condicionais: 0–1 juiz da tranca; 0–1 `classificar_falta` no degrau do dia; se continuar até testemunha, mais 0–1 `classificar_falta` nos ramos de via sem testemunha ou slot não derivável. Portanto **0–2 CF por avaliação efetiva de C6**, não número fixo; CF tem consultas de ausência, vínculo e turnos de janelas locais, não laço de dias da competência. `app/chamados/juizes.py:1389`, `:1453`, `:1467`, `:698`, `:790`; `app/escala/servico_jornada.py:349`, `:362`, `:249`, `:403`.

A tranca já tem memo por **request**, ou colaborador fora de request; `periodo_trancado` é 1 SELECT por chave fria empresa/competência. Com tranca, o toque humano negativo pode executar até **5 SELECTs**: logs “Cobrar”, células, logs “Resolver”, chamados, logs “desarquivar”; o usuário de sistema é buscado uma vez por memo. Guardam somente toque **positivo**; guardar negativo é explicitamente incorreto porque o próprio request pode escrever o toque depois. `app/chamados/juizes.py:1092`, `:1104`, `:1108`, `:1063`, `:1066`, `:1213`, `:1255`, `:1270`, `:1281`, `:1294`, `:1304`, `:1192`; `app/ponto/services/fechamento.py:1470`.

Repetição evitável demonstrável sem mudar o julgamento: transportar/alimentar dados já lidos entre consumidores consecutivos **sem escrita interveniente**, como vínculo diário já alimentado em CF, e manter identity map dos colaboradores nas leituras da mesma rodada. Não afirmar que memo de C6 por colaborador/chamado preserva decisão: C6 depende da pergunta, resposta, motivo, slot, célula e estado; o lote materializa fatos entre iterações. Preservação por um cache amplo durante todo lote escritor é **[nao verificado]**. `app/escala/servico_jornada.py:358`, `:362`, `:364`; `app/chamados/juizes.py:1159`, `:1173`, `:851`, `:941`; `app/chamados/models.py:3139`.

Predicados úteis para a sonda, **somente contar**:

- Ancoradas por chamado `ch`: `PerguntaDisputa.objects.filter(validada_em__isnull=True).filter(Q(chamado_destino_id=ch.pk) | Q(chamado_destino__isnull=True, disputa__chamado_id=ch.pk))`; agrupar por `disputa__colaborador_id`, `data_evento`, `chamado_destino_id`, `disputa__chamado_id`. É exatamente o universo de `_ancoradas`. `app/chamados/models.py:344`.
- Primeiro universo S143: `PerguntaDisputa.objects.filter(chamado_destino_id=ch.pk, materializada_em__isnull=True, disputa__fechada_em__isnull=True)`; agrupar `disputa_id`, `validada_em__isnull`, `respondida_em__isnull`, `tipo_resposta`, `celula_id`. `app/chamados/services/disputa_emissao.py:1823`.
- Amplificação S143 por colaborador `cid`: `DisputaSupervisao.objects.filter(colaborador_id=cid, fechada_em__isnull=True)`; contar por `chamado__status_local`; para os pais fora de `VIVOS` ou nulos, contar `PerguntaDisputa.objects.filter(disputa_id__in=...)` por `disputa_id`, `materializada_em__isnull`, `validada_em__isnull`, `chamado_destino__status_local`. A presença de um destino vivo faz aquele ramo parar antes de C6; contar separadamente disputas com e sem algum destino vivo. `app/chamados/services/disputa_emissao.py:1904`, `:1909`, `:1913`, `:1916`.
- Trava por colaborador: contar disputas abertas e chamados cuja condição é `Q(colaborador_id=cid,status_local__in=VIVOS) | Q(pk__in=pais_das_abertas_e_alvos)`. Esse volume diferencia aquisição de muitas linhas de custo apenas de roundtrips; espera real por lock não é inferível de contagens **[nao verificado]**. `app/chamados/models.py:2673`, `:2679`, `:2683`.
- Toque: primeiro classificar os pares empresa/competência por `PeriodoFechado.objects.filter(empresa_id=..., mes=..., ano=..., reaberto_em__isnull=True)`; só trancados consultam toque. O predicado humano exclui `usuario_id=None` e sistema, usa `criado_em__gt=fechado_em` e as três ações/modelos da fonte. `app/ponto/services/fechamento.py:1470`; `app/chamados/juizes.py:1069`, `:1247`, `:1256`, `:1281`, `:1305`.

A cópia sem `.git` impede provar o hunk introduzido por cada hash; comentários históricos apontam intenção/datas, mas atribuição de custo a commit específico sem diff continua **[nao verificado]**. [nao verificado]

## 11/11 12:40 · sessao 3898b92fbd5f · FIO-E0-V4-E4B · sonda · origem=Code

Não reexecutei a sonda: seu fonte não foi localizado na cópia. Abaixo está a saída raspada fornecida, integralmente; execução, contagens e PKs sem verificação independente. [não verificado]

```text
sonda_leitura: sonda_exportada_viva.py [75c00e700ea78f78] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

ancoradas_nao_validadas_em_chamado_encerrado n 22246
perguntas ('aberta', 'morta') 9655
perguntas ('aberta', 'viva') 1913
perguntas ('comp', 'aberta', '2026-04') 20
perguntas ('comp', 'aberta', '2026-05') 35
perguntas ('comp', 'aberta', '2026-06') 3
perguntas ('comp', 'aberta', '2026-09') 446
perguntas ('comp', 'aberta', '2026-10') 1134
perguntas ('comp', 'aberta', '2026-11') 275
perguntas ('comp', 'exportada', '2026-05') 3
perguntas ('comp', 'exportada', '2026-06') 17
perguntas ('comp', 'exportada', '2026-08') 420
perguntas ('comp', 'exportada', '2026-09') 706
perguntas ('exportada', 'morta') 9532
perguntas ('exportada', 'viva') 1146
chamados_encerrados_com_viva exportada 422 [1225, 1367, 1544, 1583, 1620, 1693, 1694, 1695, 1712, 1836, 2385, 2388, 2586, 2587, 2588, 2765, 5037, 5623, 5715, 8986]
chamados_encerrados_com_viva aberta 745 [1203, 2350, 2525, 2765, 2834, 5037, 5894, 8591, 9185, 9535, 9551, 9687, 9748, 10072, 10970, 12920, 14296, 15630, 16014, 16025]
chamados_encerrados_com_viva indecidivel 0 []
```

EXPORTADA: 422 chamados encerrados com pergunta viva, conforme dado fornecido. [não verificado]  
ABERTA: 745 chamados encerrados com pergunta viva, conforme dado fornecido. [não verificado]  
INDECIDÍVEL: 0 chamados, conforme dado fornecido. [não verificado]  
EXPORTADA — meses de `data_evento`: 2026-05, 2026-06, 2026-08, 2026-09. [não verificado]  
ABERTA — meses de `data_evento`: 2026-04, 2026-05, 2026-06, 2026-09, 2026-10, 2026-11. [não verificado]

## 11/11 12:39 · sessao 73d538fff093 · - · - · origem=Ronald (tmux)

Q4 — contraexemplos condicionais que permanecem após o diff:

- **Triagem online, `getUserMedia` pendente:** o confirmar a batida nasce oculto (`app/templates/ponto/triagem/apto.html:93`); `abrirCamera()` oculta abrir câmera e aguarda `getUserMedia` sem timeout (`:148-150`); a liberação ocorre depois da captura (`:182-185`) ou no catch (`:156-163`). Uma promessa que nunca resolve/rejeita não alcança nenhum desses caminhos. Ocorrência em produção: [nao verificado].
- **Triagem online, `toBlob` sem callback:** `confirmarFoto()` esconde vídeo/confirmar selfie (`app/templates/ponto/triagem/apto.html:175-179`), mas libera o botão de batida somente no callback `toBlob` (`:182-186`). O diff altera `confirmarFotoOffline`, mantendo esse trecho online. Ocorrência em produção: [nao verificado].
- **Legado, câmera pendente:** `bloco-botoes` começa `display:none` (`app/templates/ponto/bater_ponto.html:246`); `abrirCameraPre` aguarda câmera sem timeout (`:354-356`). O novo catch não cobre promessa pendente. Contudo, a view atual renderiza v2 ou triagem, sem selecionar esse legado (`app/ponto/views.py:356-358`; seleção da triagem em `:225-235`).
- **v2, falha assíncrona da conversão:** `capturarFrame` põe `done=true` antes de tentar `toDataURL` no callback (`app/templates/ponto/bater_ponto_v2.html:265`), e chama `toDataURL` também fora de proteção no watchdog (`:267`). Se callback assíncrono recebe `null` e `toDataURL` lança, a promessa pode permanecer pendente, e `done=true` impede o watchdog de resolvê-la (`:263-267`). O novo `.catch()` só cobre rejeição. Ocorrência real desse cenário: [nao verificado].
- **`pwa.js` indisponível:** ambas as cascas incluem o arquivo com `defer`, o que comprova inclusão, sem comprovar carregamento (`app/templates/base.html:20`; `app/templates/base_app.html:20`). A persistência offline depende de `window.JulianiPWA.registrarBatidaOffline` na v2 (`app/templates/ponto/bater_ponto_v2.html:333`, `:346`), legado (`app/templates/ponto/bater_ponto.html:543`, `:597`) e triagem (`app/templates/ponto/triagem/apto.html:282`). O diff mantém essa dependência; seu isolamento do aviso da v2 não fornece outra implementação da fila. Falha de carregamento efetiva: [nao verificado].

Telas de fluxo não tocadas:

| Tela | Função e classe |
|---|---|
| `app/templates/ponto/triagem/turno_confirmar.html:8` | POST de nova entrada, `tipo=E`, `confirmar=sim`; sem câmera/foto nesse formulário (`:10-17`). Sem a barreira de selfie; envio depende da rede, sem fila local no template (`:8-24`). |
| `app/templates/ponto/triagem/turno_regularizar.html:19` | Regularização, `tipo=regularizar_saida` (`:21`), horários de saída (`:29-31`); sem câmera. Selecionada por `app/ponto/views.py:229-230`. |
| `app/templates/ponto/triagem/fora_horario.html:20` | Solicita anuência (`:22`), não captura selfie. Selecionada por `app/ponto/views.py:231-232`. |
| `app/templates/ponto/regularizar_retorno.html:23` | Regularização com horário e motivo (`:28`, `:35`), sem câmera nesse formulário (`:23-57`). |
| `app/templates/ponto/materializar_saida_retroativa.html:38` | Criação administrativa de saída retroativa, horário e motivo (`:43`, `:50`), sem câmera nesse formulário (`:38-58`). |

Nas buscas por `getUserMedia`, `registrarBatidaOffline`, `name="foto"` e `capture=` em `templates/ponto/**`, só as três telas tocadas têm captura de selfie; completude diante de nomes dinâmicos não identificáveis por esses termos: [nao verificado]. A view encaminha diretamente a confirmação para `turno_confirmar.html` (`app/ponto/views.py:233-234`), que não entra no mapa `TELAS_SELFIE` do anexo; esse mapa não é um censo de todos os formulários de batida [nao verificado].

Fora do escopo estrito de telas de batida, existe o gate de onboarding: colaborador incompleto é redirecionado antes da tela (`app/ponto/views.py:243-244`); onboarding tem selfie (`app/templates/core/onboarding.html:53-63`), câmera sem timeout (`:154`), `toBlob` sem watchdog (`:190-200`) e `salvarFoto()` retorna sem foto (`:212-213`). A inclusão disso no aval é [nao verificado].

Q5 — **na árvore há um chamador, não zero:** definição de `estadoPermissao` em `app/templates/ponto/bater_ponto_v2.html:281`, chamada em `:288`. Busca `rg -n --hidden 'estadoPermissao' .` encontrou somente essas duas ocorrências; nenhuma em static/testes [nao verificado]. O diff remove definição e esse único chamador; zero remanescentes após aplicação é previsão estática, aplicação/execução [nao verificado].

## 11/11 12:39 · sessao e28afac356d2 · - · - · origem=Ronald (tmux)

Auditoria de fila/foto, somente leitura:

- **Foto nula já é contrato da fila antes do diff.** `registrarBatidaOffline` recebe `fotoBlob` e grava `foto: fotoBlob || null`; a transação IndexedDB conclui antes do retorno. Portanto o diff muda os produtores das telas, sem inaugurar `null` no formato armazenado. `app/static/pwa.js:107`, `app/static/pwa.js:113`, `app/static/pwa.js:118`, `app/static/pwa.js:120`.
- **Leitor da fila tolera ausência.** `sincronizarBatidas` lê todos os registros, ordena pelo `timestamp`, e só acrescenta multipart `foto` quando `b.foto` é truthy. O POST sem foto segue ao mesmo endpoint `/api/ponto/bater/`. `app/static/pwa.js:126`, `app/static/pwa.js:132`, `app/static/pwa.js:137`, `app/static/pwa.js:152`, `app/static/pwa.js:153`.
- **Formato exigido quando a foto existe:** `form.append('foto', b.foto, 'selfie.jpg')` usa o valor como Blob/File, sem conversão de dataURL. Um dataURL truthy armazenado nessa propriedade não tem adaptador nesse leitor; `null` é pulado. Os produtores atuais escrevem Blob de `canvas.toBlob`, File do input, ou Blob convertido de dataURL, e não a string do preview. `app/static/pwa.js:152`; `app/templates/ponto/triagem/apto.html:174`, `app/templates/ponto/triagem/apto.html:182`, `app/templates/ponto/triagem/apto.html:183`; `app/templates/ponto/bater_ponto.html:395`, `app/templates/ponto/bater_ponto.html:430`, `app/templates/ponto/bater_ponto.html:443`; `app/templates/ponto/bater_ponto_v2.html:252`, `app/templates/ponto/bater_ponto_v2.html:262`, `app/templates/ponto/bater_ponto_v2.html:265`.
- **POST direto também é condicional.** V2 só adiciona `_fotoBlob` ao FormData quando existe; a tela antiga faz o mesmo; apto só preenche input de arquivo quando `_fotoBlob` existe, e depois submete o formulário. `app/templates/ponto/bater_ponto_v2.html:320`, `app/templates/ponto/bater_ponto.html:531`, `app/templates/ponto/triagem/apto.html:229`, `app/templates/ponto/triagem/apto.html:235`.
- **Servidor recebe ausência como ausência de arquivo**, via `request.FILES.get('foto')`; o modelo admite foto opcional. API usa `if not foto` para consultar `selfie_ausente` e acrescentar motivo/contexto, enquanto anexação só ocorre com foto. Não há veto por ausência nesse ramo. `app/api/views_core.py:423`, `app/api/views_core.py:436`, `app/api/views_core.py:448`, `app/api/views_core.py:451`, `app/api/views_core.py:776`; gêmeo `app/api/views.py:436`, `app/api/views.py:449`, `app/api/views.py:461`, `app/api/views.py:464`, `app/api/views.py:781`; `app/ponto/models.py:78`.
- **Escritor persistente de foto é a porta existente:** `anexar_foto` escreve `Batida.foto` por `anotar`, opcionalmente cria/atualiza `FotoBatida`. Ausência não chama essa função no endpoint de bater. Upload separado exige arquivo e, depois de anexá-lo, resolve disputas. `app/ponto/registro_batida.py:296`, `app/ponto/registro_batida.py:305`, `app/ponto/registro_batida.py:307`, `app/ponto/registro_batida.py:311`; `app/api/views_core.py:835`, `app/api/views_core.py:844`, `app/api/views_core.py:854`.
- **Leitores posteriores conhecidos toleram ausência:** cron compõe `tem_foto = bool(FotoBatida existente ou b.foto)` para o juiz; dashboard e detalhe usam condicionais antes de `.url`. `app/ponto/management/commands/marcar_foto_ausente_retro.py:70`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:74`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:75`; `app/core/views.py:1005`; `app/colaboradores/services/detalhe.py:87`; `app/templates/core/_d2_feed.html:6`.
- **`foto_ausente` é motivo de disputa, não campo booleano da batida.** As APIs acrescentam a string em `motivos_anomalia` e um objeto em `contexto_anomalia`; cron cria/atualiza disputa com esse motivo; upload separado procura disputas abertas contendo a string e a remove/fecha. `app/api/views_core.py:451`, `app/api/views_core.py:452`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/api/foto_recebida.py:61`, `app/api/foto_recebida.py:70`, `app/api/foto_recebida.py:72`, `app/api/foto_recebida.py:80`.

Sincronização e unidades:

- SW **não lê foto nem IndexedDB** no tratador de sync: se há clientes, transmite `SYNC_BATIDAS`; sem clientes, oferece notificação para abrir o app. A página recebe a mensagem e chama `sincronizarBatidas`. `app/static/sw.js:112`, `app/static/sw.js:116`, `app/static/sw.js:119`, `app/static/sw.js:122`; `app/static/pwa.js:15`.
- Outros gatilhos de sync são evento `online` e inicialização com navegador online; exige meta `user-perfil=colaborador`. `app/static/pwa.js:128`, `app/static/pwa.js:320`, `app/static/pwa.js:328`.
- `resp.ok` apaga o item; HTTP 400 também apaga como erro definitivo; falha de rede/5xx deixa para nova tentativa. Ausência de foto em si não é condição de descarte no cliente. `app/static/pwa.js:158`, `app/static/pwa.js:162`, `app/static/pwa.js:166`, `app/static/pwa.js:172`, `app/static/pwa.js:177`.
- Relógio/gap: escritor calcula ISO corrigido com `Date.now()+gap`, grava também ISO bruto, `timestamp_gap_ms`, coordenadas, `gps_accuracy`, `gps_captured_at`. Gap é calculado em milissegundos. Sync envia somente o ISO bruto dentre os timestamps; `timestamp` serve para ordenar, e gap/accuracy/captured_at não são acrescentados ao formulário. Essa lacuna preexiste ao diff. `app/static/pwa.js:85`, `app/static/pwa.js:91`, `app/static/pwa.js:109`, `app/static/pwa.js:115`, `app/static/pwa.js:117`, `app/static/pwa.js:137`, `app/static/pwa.js:143`, `app/static/pwa.js:147`.
- API passa o timestamp bruto ao juiz `hora_do_aparelho`, com `agora` injetado; o juiz aceita passado até 48h e, fora disso, usa `agora`, conservando a hora lida em outro retorno. Portanto o ISO corrigido salvo no IndexedDB não é a hora enviada por esse sincronizador. `app/api/views_core.py:496`, `app/api/views_core.py:505`, `app/api/views_core.py:507`; `app/api/hora_do_aparelho.py:124`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:146`, `app/api/hora_do_aparelho.py:153`.

Contraexemplos estáticos que sobrevivem ao diff:

- **Apto online, callback `toBlob` nunca chamado:** botão principal só é liberado dentro do callback de `confirmarFoto`, trecho não alterado pelo diff. Execução com esse callback pendente deixa o fluxo sem botão liberado; incidência concreta [nao verificado]. `app/templates/ponto/triagem/apto.html:182`, `app/templates/ponto/triagem/apto.html:185`.
- **V2, timeout de captura também pode não resolver:** em `capturarFrame`, timeout avalia `cv.toDataURL(...)` antes de chamar o conversor protegido por `try`. Se `toBlob` não chama e `toDataURL` lança no callback do timeout, a promessa não resolve/rejeita naquele callback; o `.catch` proposto em `garantirRequisitos` não resolve uma promessa ainda pendente. Incidência concreta [nao verificado]. `app/templates/ponto/bater_ponto_v2.html:257`, `app/templates/ponto/bater_ponto_v2.html:262`, `app/templates/ponto/bater_ponto_v2.html:265`, `app/templates/ponto/bater_ponto_v2.html:267`.
- **PWA ausente, fila indisponível:** todos os caminhos de gravação offline chamam diretamente `window.JulianiPWA.registrarBatidaOffline`; timeout do aviso da v2 isola o aviso, mas não oferece um escritor alternativo da fila. `app/templates/ponto/bater_ponto_v2.html:333`, `app/templates/ponto/bater_ponto_v2.html:346`; `app/templates/ponto/bater_ponto.html:543`, `app/templates/ponto/bater_ponto.html:597`; `app/templates/ponto/triagem/apto.html:282`.

_3865 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
