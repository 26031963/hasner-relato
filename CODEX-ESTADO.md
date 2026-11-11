# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 13:41** · publicado em **11/11/2026 13:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 13:41 · sessao b08d61d550d2 · - · - · origem=Ronald (tmux)

O censo não encontrou uso vivo do template apagado `bater_ponto.html`: a view renderiza V2 ou o seletor de triagem; as rotas conservam nomes `bater_ponto`/`bater_ponto_v2`, que não são referências ao arquivo removido. (`app/ponto/views.py:225`, `app/ponto/views.py:356`; `app/ponto/urls.py:11`)

As referências restantes ao arquivo removido são comentário de exclusão, comentário histórico dos mutantes e ticket “MATAR”; o verificador de URLs já não o inclui. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:379`; `app/docs/TICKETS.md:693`; `app/tools/check_frontend_urls.py:25`)

No front de batida, as chamadas reais a câmera/conversão estão concentradas em PWA; a outra ocorrência é onboarding, declarado como foto de referência no censo fechado. (`app/static/pwa.js:137`, `app/static/pwa.js:148`, `app/static/pwa.js:163`; `app/templates/core/onboarding.html:154`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:476`)

As telas continuam interpretando a resposta para apresentação e transporte, mas não retornam recusando por foto nula: triagem libera sem stream, aguarda captura protegida e envia; offline mantém confirmar e entrega Blob/null à fila; V2 aguarda requisitos e anexa somente se houver foto. (`app/templates/ponto/triagem/apto.html:154`, `app/templates/ponto/triagem/apto.html:231`, `app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`; `app/templates/ponto/bater_ponto_v2.html:273`, `app/templates/ponto/bater_ponto_v2.html:297`, `app/templates/ponto/bater_ponto_v2.html:305`)

Persistem textos contraditórios próprios: “Tire sua selfie para liberar o ponto”, “Selfie obrigatória”, o texto de refazer e “Tire a selfie para registrar”; não constituem veto técnico, mas a linguagem de obrigatoriedade não foi centralizada. (`app/templates/ponto/triagem/apto.html:66`, `app/templates/ponto/triagem/apto.html:124`, `app/templates/ponto/triagem/apto.html:194`, `app/templates/ponto/triagem/apto.html:250`; `app/templates/ponto/bater_ponto_v2.html:100`)

As duas cascas carregam o mesmo PWA e diálogo global; diferem na estrutura de navegação e outros scripts, sem outra política de câmera localizada. Os testes trocam o loader de `base.html` pela casca real, portanto verificam composição nas duas cascas; seleção real da casca e execução dos testes: [nao verificado]. (`app/templates/base.html:20`, `app/templates/base.html:385`, `app/templates/base.html:388`; `app/templates/base_app.html:20`, `app/templates/base_app.html:142`, `app/templates/base_app.html:145`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:185`)

A confirmação de turno aberto continua podendo enviar formulário sem foto, fora do juiz visual da câmera; não contém captura nem fila local. (`app/templates/ponto/triagem/turno_confirmar.html:8`)

Continuam riscos fora do teto da câmera: V2 não enfileira HTTP de erro resolvido, apenas rejeição de `fetch`; triagem online usa submit nativo; todos os caminhos offline dependem do escritor PWA. (`app/templates/ponto/bater_ponto_v2.html:307`, `app/templates/ponto/bater_ponto_v2.html:313`, `app/templates/ponto/bater_ponto_v2.html:316`; `app/templates/ponto/triagem/apto.html:241`, `app/templates/ponto/triagem/apto.html:289`; `app/templates/ponto/bater_ponto_v2.html:331`)

## 11/11 13:41 · sessao 8bdba0451286 · PROPORCAO-POR-RISCO · verificador · origem=Code

A v3 fecha vários exemplos da v2, mas ainda permite escrita com **rc 0 deduzido por leitura**. A seleção continua dependendo do nome imediato da chamada e da interseção dos nós reconhecidos com linhas adicionadas. (`bin/selo_proporcao.py:64`, `bin/selo_proporcao.py:69`, `bin/selo_proporcao.py:130`, `bin/selo_proporcao.py:132`)

Não executei o selo, testes ou exemplos; os retornos abaixo são deduções. Execução efetiva, efeitos persistidos e situação em produção: **[nao verificado]**. [nao verificado]

**1. Fechamento dos achados da volta 2**

| Achado da v2 | Situação na v3 e cura | Caso que o morde |
|---|---|---|
| Continuação `()` sobre atributo antigo | **Fecha o exemplo direto:** a `Call` reconhecida cobre sua extensão inteira. (`bin/selo_proporcao.py:69`, `bin/selo_proporcao.py:132`) | `continuacao_sobre_attr_antigo`. (`bin/tests/test_selo_proporcao.sh:60`) |
| Argumento novo em chamada escritora antiga | **Fecha quando a função é `Name`/`Attribute` reconhecido.** A chamada aninhada continua escapando, demonstrada abaixo. (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`) | `argumento_novo_em_update_antigo`, sobre preparo antigo. (`bin/tests/test_selo_proporcao.sh:16`, `bin/tests/test_selo_proporcao.sh:61`) |
| `exec("obj.save()")` | **Fecha o literal direto**, por `OPACO`. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:82`) | `exec_string`. (`bin/tests/test_selo_proporcao.sh:62`) |
| `eval("obj.save()")` | **Fecha o literal direto**, pelo mesmo mecanismo. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`) | `eval_string`. (`bin/tests/test_selo_proporcao.sh:63`) |
| Decorador da v2 com `@eval(...)` | **Fecha aquele exemplo**, pois contém `eval`; não fecha decoradores em geral. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:82`) | `eval_string` cobre o gatilho, mas não reproduz o decorador. (`bin/tests/test_selo_proporcao.sh:63`) |
| Nome de método em f-string | **Fecha o exemplo:** segundo argumento de `getattr` não é `Constant`. (`bin/selo_proporcao.py:73`) | `fstring_nome`. (`bin/tests/test_selo_proporcao.sh:66`) |
| `setattr` instala alias escritor | **Fecha o exemplo direto**, conservadoramente pelo próprio `setattr`. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`) | `setattr_alias`; não instala nem invoca um escritor real, mas isola esse gatilho. (`bin/tests/test_selo_proporcao.sh:64`) |
| QuerySet com método calculado | **Fecha o exemplo**, sem precisar resolver o tipo de `qs`. (`bin/selo_proporcao.py:73`) | `getattr_calculado`. (`bin/tests/test_selo_proporcao.sh:65`) |
| Alias novo de cursor e SQL fragmentado | **Fecha aquele diff** pela referência nova a `.execute`. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:80`) | `cursor_execute` contém somente a referência, suficiente para provar esse mecanismo. (`bin/tests/test_selo_proporcao.sh:68`) |
| SQL na f-string `UPDATE {tabela} SET ...` | **Fecha:** substitui expressões por ` x ` antes de procurar SQL; `.execute` também passa a ser reconhecido. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:76`) | `sql_fstring` isola a f-string, sem cursor. (`bin/tests/test_selo_proporcao.sh:67`) |
| Payload `.pyw` carregado por `runpy.run_path` | **Fecha o executor apresentado**, por `run_path`; `.pyw` continua fora da análise. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:124`) | `import_dinamico` testa `import_module`, **não** `run_path` nem o payload `.pyw`. (`bin/tests/test_selo_proporcao.sh:70`) |
| Arquivo `.sql` e executor | **Fecha o exemplo:** destino `.sql` é A, e `.execute` novo também seria detectado. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:122`) | `arquivo_sql` prova a regra por caminho. (`bin/tests/test_selo_proporcao.sh:69`) |
| Escritor novo em teste importado por linha antiga | **Não fecha; passou a limite declarado.** O arquivo continua excluído. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:124`) | Nenhum caso reproduz essa costura; `prod_importa_teste` adiciona somente o import. (`bin/tests/test_selo_proporcao.sh:49`) |
| `import *` via reexportação antiga de teste | **Não fecha e não está explicitamente declarado no docstring.** Import de módulo comum não revela a origem reexportada. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:124`) | Sem caso correspondente. (`bin/tests/test_selo_proporcao.sh:29`, `bin/tests/test_selo_proporcao.sh:70`) |
| Template chama método escritor | **Não fecha; limite declarado.** HTML continua excluído. (`bin/selo_proporcao.py:23`, `bin/selo_proporcao.py:124`) | `tela_pura` só muda texto; não chama método. (`bin/tests/test_selo_proporcao.sh:29`) |
| Argumentos excedentes | **Fecha:** exige três ou quatro argumentos. (`bin/selo_proporcao.py:94`) | `arg_a_mais`. (`bin/tests/test_selo_proporcao.sh:78`) |
| Retorno de `bash -e` não afirmado | **Fecha a ausência da asserção**, mantendo problema no preparo especial. (`bin/tests/test_selo_proporcao.sh:76`, `bin/tests/test_selo_proporcao.sh:77`) | `rc_bash_e` agora exige exatamente 1. (`bin/tests/test_selo_proporcao.sh:77`) |
| Normalização universal de erros | **Não fecha:** wrapper permanece um `exec`, sem normalização de falhas de lançamento. (`bin/selo_proporcao.sh:4`) | Sem caso de Python ausente, execução recusada ou sinal. (`bin/tests/test_selo_proporcao.sh:71`, `bin/tests/test_selo_proporcao.sh:79`) |

As curas anteriores permanecem: oito métodos ORM, espaço após ponto, alias por referência, `getattr` literal, `#` em string, signals, caminhos NUL, migration por rename, `--text` e revisão iniciada por opção. Os mecanismos estão em `bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:102`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:118` e `bin/selo_proporcao.py:56`; os respectivos casos estão em `bin/tests/test_selo_proporcao.sh:30`, `bin/tests/test_selo_proporcao.sh:33`, `bin/tests/test_selo_proporcao.sh:35`, `bin/tests/test_selo_proporcao.sh:36`, `bin/tests/test_selo_proporcao.sh:37`, `bin/tests/test_selo_proporcao.sh:39`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:46`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:48` e `bin/tests/test_selo_proporcao.sh:73`.

Continuam sem reprodução específica: `(obj.save)()`, caminho com tabulação e conteúdo falso `+++ /dev/null`; os casos próximos não têm essas formas. (`bin/tests/test_selo_proporcao.sh:33`, `bin/tests/test_selo_proporcao.sh:34`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:48`)

Quanto ao custo dos limites:

- **Helper que já grava:** está compreensível: chamada comum pode passar, e o texto atribui a detecção ao smoke. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:26`)
- **Template escritor:** a exclusão está explícita, mas deveria dizer diretamente que renderizar o template pode causar escrita real não detectada; existe método `rebaixar()` que chega a `self.save()`. Renderização e objeto no contexto: **[nao verificado]**. (`bin/selo_proporcao.py:23`, `app/chamados/models.py:488`, `app/chamados/models.py:498`)
- **Teste importado por linha antiga:** explica a exclusão técnica, mas chamar isso de “custo do aval, não fuga” omite a consequência: **escrita nova no corpo do teste pode alcançar produção com B verde**. Isso é mais amplo que apenas chamar helper que já escrevia. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:124`)
- **`import *`:** falta declaração expressa sobre reexportação e seu custo. Aceitação específica desse limite pelo Ronald: **[nao verificado]**. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:85`)

**2. Novos contraexemplos à v3**

Cada diff abaixo é independente e declarado B. Os `.py` ficam fora dos caminhos especiais e de testes; os objetos mencionados precisam existir no contexto. Os **rc 0 são deduzidos** das regras de seleção; funcionamento desses contextos em produção: **[nao verificado]**. (`bin/selo_proporcao.py:118`, `bin/selo_proporcao.py:124`, `bin/selo_proporcao.py:132`, `bin/selo_proporcao.py:145`)

**a) Chamada aninhada, alterando somente argumento.** Em um `ModelForm`, o exemplo muda a opção de persistência; presença desse formulário na aplicação: **[nao verificado]**.

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,3 +1,3 @@
 getattr(form, "save")(
-    commit=False, [nao verificado]
+    commit=True, [nao verificado]
 )
```

O `Call` externo cruza a linha adicionada, mas sua função é outro `Call`; `nome_chamado` devolve `None`. O `getattr` e a constante `"save"` estão inteiramente na linha antiga. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:132`)

**b) Lambda antiga passa a ser executada.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,2 +1,2 @@
 (lambda: obj.save()
-)
+)()
```

A chamada tem `Lambda` como função; o atributo escritor permanece na linha antiga. A alteração passa a invocar a expressão que antes apenas construía a função. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:132`)

**c) Decorador passa a executar a lambda escritora.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,5 +1,5 @@
 @(
     lambda f, acao=(lambda: obj.save()):
-        f
+        (acao(), f)[1]
 )
 def tela(): pass
```

A referência `.save` está na linha antiga; a chamada nova chama `acao`, nome comum. Nem as lambdas nem a tupla são classificadas como escrita. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:132`)

**d) `__dict__` e `vars()`, com nome fragmentado.** São alternativas independentes para um contexto com `Model` e `obj` disponíveis. Disponibilidade concreta: **[nao verificado]**.

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+Model.__dict__["sa" + "ve"](obj)
```

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+vars(Model)["sa" + "ve"](obj)
```

`__dict__`, `vars` e as constantes separadas não pertencem aos conjuntos; a função externa é um `Subscript`. **Rc 0 em ambas.** Usar `"save"` inteiro seria detectado. (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:83`)

**e) `operator.methodcaller`.**

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+import operator
+operator.methodcaller("sa" + "ve")(obj)
```

`methodcaller` não está nos conjuntos, o nome é fragmentado e a chamada externa tem outra `Call` como função. **Rc 0.** (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`)

**f) Reflexão por `type(...)`.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+type(obj).__getattribute__(obj, "sa" + "ve")()
```

`type` e `__getattribute__` não são reconhecidos; a chamada final tem uma `Call` como função. **Rc 0.** (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:83`)

**g) Alias de `exec` por import.**

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+from builtins import exec as rodar
+rodar("obj.save()")
```

O import só verifica origens de teste/signals; não aplica `OPACO` aos nomes importados. A chamada vê `rodar`, e a string `"obj.save()"` não é um nome exato nem SQL. **Rc 0.** Em contraste, `rodar = exec` seria detectado pelo `Name exec`. (`bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`)

**h) `.sql` renomeado para sufixo ignorado.**

```diff
diff --git a/app/x/q.sql b/app/x/q.txt
rename from app/x/q.sql
rename to app/x/q.txt
```

Considere `q.sql` contendo escrita e um executor antigo que só consome `q.txt` quando esse arquivo existe. A renomeação pode ativar o payload sem mudar Python; existência desse executor: **[nao verificado]**.

O selo considera o destino; `.txt` é ignorado. Mesmo que Git reporte deleção e adição, a deleção é ignorada e o destino continua fora. **Rc 0.** Rename mantendo destino `.sql` dá **rc 1**, portanto não constitui escape. (`bin/selo_proporcao.py:110`, `bin/selo_proporcao.py:116`, `bin/selo_proporcao.py:122`, `bin/selo_proporcao.py:124`)

**i) F-string sem SQL nas partes constantes.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1 +1,2 @@
 run = cur.execute
+run(f"{'DE' + 'LETE'} {'FR' + 'OM'} tabela")
```

A referência `.execute` é antiga; `run` não é resolvido como alias. O `JoinedStr` vira aproximadamente ` x   x  tabela`, e nenhuma constante contém SQL completo. **Rc 0.** (`bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:76`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:132`)

**3. Falsos positivos e controles**

| Código adicionado em `.py` comum | Rc B deduzido |
|---|---|
| `getattr(obj, "x")` | **0**: segundo argumento continua sendo `Constant`, sem nome reconhecido. (`bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:83`) |
| `getattr(obj, nome)` usado apenas para leitura | **1, novo falso positivo:** nome calculado basta. (`bin/selo_proporcao.py:73`) |
| `motor_de_template.execute()` sem persistência | **1, novo falso positivo:** não resolve o tipo do receptor. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`) |
| `texto = f"UPDATE {nome} SET rótulo"` apenas para exibição | **1, novo falso positivo:** o texto montado satisfaz a regex SQL. (`bin/selo_proporcao.py:41`, `bin/selo_proporcao.py:76`) |
| `texto = f"update {nome}"` | **0**: não contém `SET`, e a constante `"update "` não é o nome exato `"update"`. (`bin/selo_proporcao.py:41`, `bin/selo_proporcao.py:76`, `bin/selo_proporcao.py:83`) |
| `texto = f"update{nome}"` | **1, já ocorria na v2:** a constante exata `"update"` basta. (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:83`) |
| Palavra `update` em template `.html` | **0**: HTML permanece fora. (`bin/selo_proporcao.py:124`) |
| Referência a `exec`, `eval` ou `compile` sem execução | **1:** `Name` em `OPACO` basta. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:82`) |
| `.sql` contendo apenas `SELECT`, comentário ou texto | **1:** regra incondicional por caminho. (`bin/selo_proporcao.py:122`) |

Pelo critério fornecido, esses falsos positivos que empurram para A não são bloqueantes; o código também declara essa direção conservadora. (`bin/selo_proporcao.py:27`)

**4. Retornos**

- **Argumento a mais agora dá 2**, inclusive na chamada testada com `repo extra`. (`bin/selo_proporcao.py:94`, `bin/tests/test_selo_proporcao.sh:78`)
- **Argumentos inválidos, revisão inválida, Git não zero, parse inválido e exceções ordinárias alcançadas pela fronteira Python dão 2.** (`bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:102`, `bin/selo_proporcao.py:134`, `bin/selo_proporcao.py:152`)
- **Não são todos os caminhos de erro:** falha ao lançar Python pode preservar 126/127; término por sinal não passa pela captura de `Exception`. Ocorrência concreta: **[nao verificado]**. O wrapper permanece sem normalização. (`bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:152`)
- **`bash -e` recebe o rc 1 inteiro**, se o selo o produzir: `exec` preserva o retorno, e o shell externo captura a saída do shell com `-e`. O teste agora afirma exatamente 1. (`bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:143`, `bin/tests/test_selo_proporcao.sh:77`)
- **Rc 1 continua significando classificação conservadora**, incluindo falsos positivos; não comprova escrita efetiva. (`bin/selo_proporcao.py:141`, `bin/selo_proporcao.py:143`)

**5. Qualidade dos casos**

| Caso ou anúncio | Avaliação |
|---|---|
| `so_remocao` | **Corrigido:** base contém `a = 1` e `o.save()`; `sed` só remove a segunda linha. O arquivo permanece Python válido, sem linha adicionada. (`bin/tests/test_selo_proporcao.sh:16`, `bin/tests/test_selo_proporcao.sh:51`) |
| `apagou_migration` | **Corrigido:** migration existe na base e é realmente removida. Exercita o ramo `D`. (`bin/tests/test_selo_proporcao.sh:15`, `bin/tests/test_selo_proporcao.sh:56`, `bin/selo_proporcao.py:116`) |
| `sql_escrita` | **Ganhou duplicidade na v3:** `.execute` e a constante SQL bastam independentemente. Não isola a regex SQL. (`bin/tests/test_selo_proporcao.sh:38`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`, `bin/selo_proporcao.py:83`) |
| `signal_nome` | **Duplicidade permanece:** módulo terminado em `signals` ou nome importado `post_save` bastam independentemente. (`bin/tests/test_selo_proporcao.sh:40`, `bin/selo_proporcao.py:88`) |
| Chamadas diretas ORM | Agora podem disparar por `Attribute` e por `Call`; não isolam a nova regra de extensão. Os dois casos com chamada antiga isolam melhor essa cura. (`bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:60`, `bin/tests/test_selo_proporcao.sh:61`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`) |
| `command_alias` | Continua provando atributo genérico `.create`, sem invocar alias e sem provar mecanismo específico de command. (`bin/tests/test_selo_proporcao.sh:45`, `bin/selo_proporcao.py:80`) |
| `prod_importa_teste` | Continua sem criar módulo escritor: prova a regra conservadora do import, não a costura produção→teste→escrita. (`bin/tests/test_selo_proporcao.sh:49`) |
| Preparo comum | Checkout, resultado final do script, commit e mudança de HEAD são conferidos; comandos anteriores separados por `;` ainda podem falhar e ser mascarados pelo último. (`bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:23`, `bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:46`) |
| Preparo de `rc_bash_e` | **Continua sem conferência:** checkout, escrita, add e commit estão encadeados por `;`. A asserção do rc existe, mas pode julgar outra árvore se esse preparo falhar. (`bin/tests/test_selo_proporcao.sh:76`, `bin/tests/test_selo_proporcao.sh:77`) |
| “Cada escape” e “um mecanismo por caso” | O anúncio continua mais forte que a cobertura: persistem duplicidades e ausências específicas descritas acima. (`bin/tests/test_selo_proporcao.sh:3`, `bin/tests/test_selo_proporcao.sh:4`, `bin/tests/test_selo_proporcao.sh:38`, `bin/tests/test_selo_proporcao.sh:40`) |

**6. Quem escreve, chama e lê**

- **Produz o veredito:** Python imprime achados e retorna 0/1/2; o wrapper executa Python. (`bin/selo_proporcao.py:137`, `bin/selo_proporcao.py:141`, `bin/selo_proporcao.py:145`, `bin/selo_proporcao.py:152`, `bin/selo_proporcao.sh:4`)
- **Chamador concreto encontrado:** `test_selo_proporcao.sh` define `SELO`, chama sobre repositório sintético, captura `$?` e compara com o esperado. (`bin/tests/test_selo_proporcao.sh:8`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:25`, `bin/tests/test_selo_proporcao.sh:26`)
- **Outros leitores diretos:** as verificações de argumentos/revisões e as duas verificações de propagação do rc. (`bin/tests/test_selo_proporcao.sh:71`, `bin/tests/test_selo_proporcao.sh:77`, `bin/tests/test_selo_proporcao.sh:78`, `bin/tests/test_selo_proporcao.sh:79`)
- **Leitor indireto:** a régua chama o teste pelo glob e transforma falha do teste em rc 1 da régua. Isso testa cenários sintéticos; não classifica o diff real de pouso. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_selo_proporcao.sh:25`)
- **Consumidor do pouso real:** o contrato exige que merge+deploy espere o rc; chamada operacional, leitor efetivo e registro no RELATO continuam **[nao verificado]**. (`contrato.txt:2`, `contrato.txt:3`, anexo) [nao verificado]
- **CORTES/RELATO:** não encontrei o registro específico PROPORCAO-POR-RISCO na busca textual desses documentos; atendimento desses itens do aval: **[nao verificado]**.
- **Pre-push:** o código declara que não roda o loop de host e chama outros selos individualmente; uso deste selo pelo hook instalado: **[nao verifi

[truncado em 20000 de 21776 caracteres]

## 11/11 13:40 · sessao eb2c0d0b2703 · - · - · origem=Ronald (tmux)

- **Juiz do servidor = cobrança, não veto.** `selfie_ausente` retorna `origem in ('app',) and not tem_foto and decorrido_min >= prazo`; prazo PWA 0 min, APK/canal desconhecido 90 min, sobreposição só APK/desconhecido. Não lê banco nem relógio. Registro vivo identifica essa autoridade. (`app/ponto/registro_batida.py:71`, `:75`, `:80`; `app/core/juizes.py:1192`)

- **Três chamadores operacionais encontrados:** API principal e API core sob `if not foto`, com `tem_foto=False, decorrido_min=0`, acrescentando motivo/contexto sem impedir escrita; retro usa canal gravado, existência `FotoBatida OR Batida.foto` e duração em minutos. (`app/api/views.py:449`, `:461`; `app/api/views_core.py:436`, `:448`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:70`, `:74`)

- **As APIs usam a mesma porta com ou sem foto:** `registrar_batida` recebe campos da batida sem exigir foto; anexação ocorre depois, somente `if foto`, por `anexar_foto(...registro='criar')`. A porta cria `Batida` sem predicado de foto; seus vetos próprios são competência/colisão, além dos retornos de deduplicação. (`app/api/views.py:665`, `:781`; `app/api/views_core.py:657`, `:776`; `app/ponto/registro_batida.py:94`, `:113`, `:132`, `:145`, `:157`)

- **POST triagem web:** escreve por `registrar_batida` antes de ler/anexar foto; `foto_sessao or request.FILES.get("foto")` só controla anexação. Não consulta `selfie_ausente` nesse ato; a cobrança poderá vir do retro, que lê o canal gravado e ambas as fontes de foto. Isso preserva diferença temporal entre casca web e APIs: web sem foto não cobra inline, API PWA cobra inline. (`app/ponto/services/triagem_batida.py:700`, `:748`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:74`; `app/api/views.py:461`; `app/api/views_core.py:448`)

- **Escritor persistente de foto:** `anexar_foto` escreve `Batida.foto` pela porta `anotar`; pode também criar/atualizar `FotoBatida` conforme `registro`. Modelo admite foto nula. Upload separado exige foto como insumo daquela operação, sem impor foto à criação da batida. (`app/ponto/registro_batida.py:296`; `app/ponto/models.py:78`; `app/api/views.py:847`; `app/api/views_core.py:832`; `app/ponto/views.py:2895`)

- **`foto_ausente` é motivo de cobrança**, não campo booleano de `Batida`: APIs e retro o entregam a `criar_ou_atualizar_disputa`; catálogo transforma em confirmação de presença; upload posterior procura disputas desse motivo, vincula por `contexto.ultima_batida.id`, apara/fecha pelas portas. (`app/api/views.py:464`, `:767`; `app/api/views_core.py:451`, `:762`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/chamados/catalogo/perguntas.py:93`; `app/api/foto_recebida.py:57`)

- **Fila admite null e sync omite arquivo:** escritor normaliza `foto: fotoBlob || null`; leitor só faz multipart de foto se `b.foto` truthy, mantendo exigência de Blob/File quando presente. Modelo, retro e leitores de apresentação examinados tratam ausência; não encontrei exigência de foto sempre presente nesses leitores. Consumidores externos à cópia: [nao verificado]. (`app/static/pwa.js:169`, `:180`, `:214`; `app/ponto/models.py:78`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:75`; `app/core/views.py:1005`; `app/colaboradores/services/detalhe.py:87`; `app/templates/core/_d2_feed.html:6`)

- **Possível bloqueante remanescente real na fila:** `registrarBatidaOffline` aguarda `obterGap()` antes de abrir/gravar `batidas_pendentes`; gap ausente/velho chama `calcularESalvarGap`, cujo `fetch('/hora/')` não tem timeout. Se esse fetch permanecer pendente, a batida não chega ao `add`, embora a captura já tenha resolvido pelo teto. Incidência em produção: [nao verificado]. O Chromium não prova persistência porque substitui `registrarBatidaOffline` por espia que resolve imediatamente. (`app/static/pwa.js:85`, `:100`, `:169`, `:175`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:116`)

- **Sync não pressupõe foto:** lê fila, envia mesmo endpoint `/api/ponto/bater/`, remove após sucesso ou HTTP 400 e conserva em rede/5xx; SW delega às páginas e, sem página, notifica. Sucesso de chamada da espia não comprova esse ciclo. (`app/static/pwa.js:188`, `:214`, `:220`, `:228`, `:239`, `:384`; `app/static/sw.js:112`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:118`)

- **Lacuna anterior de transporte permanece:** fila armazena timestamp corrigido, timestamp bruto, gap, precisão e instante GPS; sincronizador envia timestamp bruto, mas não envia gap, precisão nem instante GPS. Isso não é campo de foto com dois significados; é perda de insumos no transporte. (`app/static/pwa.js:171`, `:176`; `app/static/pwa.js:205`)

- **Testes de servidor existentes:** sem foto cria batida e disputa, com foto não cria motivo `foto_ausente`; juiz testa limiares e retro lê ambas as fontes. Execução nesta revisão: [nao verificado]. (`app/api/tests/test_api_bater_pwa.py:64`, `:95`; `app/ponto/tests/test_selfie_ausente_juiz.py:27`, `:34`, `:103`, `:111`)

## 11/11 13:36 · sessao 578de191b847 · - · - · origem=Ronald (tmux)

- **Quem escreve o veredito:** Python imprime achados e devolve 1 para B com achados, 0 para restante julgado e 2 para erros tratados; o wrapper faz `exec python3`. (`bin/selo_proporcao.py:134`, `bin/selo_proporcao.py:137`, `bin/selo_proporcao.py:141`, `bin/selo_proporcao.py:145`, `bin/selo_proporcao.py:152`, `bin/selo_proporcao.sh:4`)
- **Chamador concreto encontrado:** o teste define `SELO`, chama o wrapper para o repositório sintético, captura `$?` e compara com o esperado. (`bin/tests/test_selo_proporcao.sh:8`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:25`, `bin/tests/test_selo_proporcao.sh:26`)
- **Leitores adicionais no teste:** argumentos/classes/revisões são comparados com 2; a chamada com `bash -e` agora tem resultado comparado explicitamente com 1, assim como a chamada direta. (`bin/tests/test_selo_proporcao.sh:71`, `bin/tests/test_selo_proporcao.sh:72`, `bin/tests/test_selo_proporcao.sh:73`, `bin/tests/test_selo_proporcao.sh:74`, `bin/tests/test_selo_proporcao.sh:77`, `bin/tests/test_selo_proporcao.sh:78`, `bin/tests/test_selo_proporcao.sh:79`)
- **Leitor indireto:** a régua chama todos os `tests/test_*.sh`, portanto inclui este teste quando alcança o loop; falha do teste vira rc 1 da régua. O repositório julgado pelo teste é sintético, não o diff de um pouso real. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:25`)
- **Pouso real:** não encontrei chamada operacional, captura do rc ou registro PROPORCAO em CORTES/RELATO na busca textual do código/docs da cópia, excluindo `.git`, `logs/` e `app/logs/`; integração operacional/manual **[nao verificado]**. As únicas referências literais encontradas a `selo_proporcao`/`PROPORCAO-POR-RISCO` pertencem aos três arquivos do instrumento; exaustividade frente a chamada construída dinamicamente **[nao verificado]**. [nao verificado]
- **Pre-push:** declara que não executa o loop de host e chama individualmente outros selos; instalação efetiva e uso deste selo em produção **[nao verificado]**. (`bin/pre-push.sh:14`, `bin/pre-push.sh:15`, `bin/pre-push.sh:16`, `bin/pre-push.sh:19`, `bin/pre-push.sh:38`)
- **Atalho da régua permanece:** `FP` considera apenas `.py`/`.html` em `app/`; `ARVORE_FP` é calculado, mas o retorno “JA VERDE” compara somente `FP` e status e ocorre antes do loop de host. Assim, mudar somente `bin/` pode conservar esse atalho, condicionado a carimbo anterior OK. (`bin/regua.sh:26`, `bin/regua.sh:33`, `bin/regua.sh:38`, `bin/regua.sh:45`, `bin/regua.sh:84`, `bin/regua.sh:86`, `bin/regua.sh:93`, `bin/regua.sh:138`)
- **Worktree:** o próprio teste resolve seu diretório e passa o repo sintético explicitamente; execução em worktree **[nao verificado]**. O problema independente de outro teste permanece: `test_hook_nao_e_copia.sh` exige `.git/hooks/pre-push` e falha se ausente; o teste vizinho já consulta `core.hooksPath`/diretório comum. (`bin/tests/test_selo_proporcao.sh:7`, `bin/tests/test_selo_proporcao.sh:25`, `bin/tests/test_hook_nao_e_copia.sh:21`, `bin/tests/test_hook_nao_e_copia.sh:25`, `bin/tests/test_hook_nao_e_copia.sh:27`, `bin/tests/test_commit_so_o_declarado.sh:56`, `bin/tests/test_commit_so_o_declarado.sh:57`)

## 11/11 13:33 · sessao 3d47551f7e4d · O168 · censo · origem=Code

O buraco está confirmado na cópia: o supra consulta `pode_executar_csf`; o cartório reconcilia sem essa guarda, inclusive antes do bloco que protege respostas textuais. A retirada do `--executar` continua condicionada à guarda no evento e à prova de `CSF_RETRATADO=0`. (`app/ponto/management/commands/supra_juiz.py:261`; `app/ponto/services/cartorio.py:1525`; `app/ponto/services/cartorio.py:1559`; `app/ponto/services/cartorio.py:1623`; `app/docs/BACKLOG.md:1395`.)

O raspado fornecido não contém a medição atual nem resultados de testes: **produção atual, zero e aprovação da suíte [nao verificado]**. [nao verificado]

1. **Guarda e predicados de conversa**

`pode_executar_csf(ch)` permite executar exatamente quando **nenhuma** pergunta das duas relações abaixo tem `respondida_em is not None`:

- Todas as perguntas de **todas** as disputas em `ch.disputa_origem.all()`.
- Todas as perguntas de `ch.perguntas_que_resolvem.all()`.

Relações vazias permitem executar. Não exige texto, pergunta viva, disputa aberta, validação, materialização, data ou `PerguntaDisputa.conversa=True`; também não consulta mensagens de `FollowupChamado`. (`app/ponto/supra_juiz.py:539`.)

O único chamador de produção encontrado dessa função é o comando supra; a importação está em `:257` e a chamada em `:261`. (`app/ponto/management/commands/supra_juiz.py:257`.)

Há outra implementação de proteção por resposta no cartório:

| Função | Predicado | Universo e uso |
|---|---|---|
| `pode_executar_csf(ch)` | Ausência de qualquer `respondida_em != None`. | Duas relações completas acima; protege o retratador do supra. (`app/ponto/supra_juiz.py:543`; `app/ponto/management/commands/supra_juiz.py:261`.) |
| `_tem_resposta_do_colab(perguntas)` | `any((q.resposta_colab or '').strip() for q in perguntas)`. | Nos dois leitores recebe somente `perguntas_que_resolvem`; protege FIO-DUP/ressuscitação, depois dos fechamentos CSF. Ignora timestamp e `disputa_origem`. (`app/ponto/services/cartorio.py:1623`; `:1631`; `:1634`; `:1651`.) |

Os seguintes predicados relacionados **não respondem à mesma pergunta com o mesmo universo**:

| Função | Predicado e universo |
|---|---|
| `PerguntaDisputa.respondida` | Timestamp presente; uma pergunta. (`app/chamados/models.py:3496`.) |
| `fio_vivo(perguntas)` | Alguma pergunta viva segundo `pergunta_viva`; coleção fornecida pelo chamador. (`app/chamados/juizes.py:1488`.) |
| `DisputaSupervisao.respostas_sem_veredito` | Perguntas da disputa sem validação/materialização, com timestamp **ou** texto, fora da tranca salvo opção explícita. (`app/chamados/models.py:1826`; `:1842`.) |
| `DisputaSupervisao.pode_fechar` | Nenhuma resposta sem veredito e nenhum fio vivo; uma disputa. (`app/chamados/models.py:1848`.) |
| `irmas_respondidas_sem_consumo(ch)` | Timestamp e texto presentes, sem validação/materialização. Crédito ao destino `ch`, ou ao pai `ch` quando o destino não está vivo. (`app/chamados/juizes.py:2012`.) |
| `retencao_da_retratacao(ch)` | Irmãs anteriores ou justificativa alheia pendente durante outra aprovação. Não protege toda resposta histórica. (`app/chamados/reconciliador.py:437`.) |
| `disputa_viva_do_chamado(ch)` | Primeira disputa direta aberta sem perguntas ou com fio vivo. (`app/chamados/juizes.py:2371`.) |
| Arquivador `Command._disputa_viva(ch)` | Uma disputa aberta resolvida pelo fio, com alguma pergunta viva; erro retorna sentinela conservadora. (`app/chamados/management/commands/arquivar_competencia_encerrada.py:148`.) |
| `_disputa_do_chamado(ch)` | Localiza uma disputa aberta, primeiro direta, depois por pergunta de destino; não mede resposta. (`app/chamados/services/fio.py:120`.) |
| `canonica_do_grupo(perguntas)` | Prioriza texto não branco e depois menor PK; universo de perguntas do mesmo fato. (`app/chamados/services/fio.py:1225`.) |

O registro vivo distingue pergunta viva, disputa, fio e outras autoridades; não declara `pode_executar_csf` como autoridade da família chamado nesse bloco. (`app/core/juizes.py:272`.) Ausência de qualquer outro respondedor indireto ou dinâmico em toda a árvore: [nao verificado].

2. **EVENTO: portas do cartório**

A entrada por batida chama `_julgar_dias`; `julgar_celula` entrega o julgamento a `julgar_colab`. (`app/ponto/signals.py:132`; `app/ponto/services/cartorio.py:1997`; `:2026`.)

O cartório usa a ata deste julgamento. Seu `_protesta` exige `cel.trabalha is not False` e `marcos_acesos(...) is not True`; `classificar_dia` recebe `len(_protestam)`, não simplesmente todos os chamados vivos. Por isso proteger somente o `if COBRANCA_SEM_FURO` deixa de fora o fechamento por marco que pode ocorrer antes. (`app/ponto/services/cartorio.py:1422`; `:1444`; `:1450`; `:1525`.)

Os seis sítios diretos de reconciliação são:

| Porta | Quando fecha | Guarda de conversa hoje |
|---|---|---|
| Adiados após lavra → E2 | Reconcilia o lote adiado porque havia fio vivo. | Não consulta `pode_executar_csf`. (`app/ponto/services/cartorio.py:987`; `:993`; `:998`.) |
| RECONCILIA-MARCO | Não protesta: marcos acesos ou `trabalha=False`, descontados os adiados. | Não consulta a guarda. (`app/ponto/services/cartorio.py:1519`; `:1525`.) |
| MARCO-INEXISTENTE | Marcos cobrados deixaram de existir na ata atual. | Não consulta a guarda; anexa nota própria. (`app/ponto/services/cartorio.py:1536`; `:1538`; `:1542`.) |
| RECONCILIA por código | `COBRANCA_SEM_FURO` **ou** `COBRANCA_EM_DIA_SEM_TRABALHO`. | Não consulta a guarda. (`app/ponto/services/cartorio.py:1551`; `:1559`.) |
| FIO-DUP | Todas as perguntas diretas têm via duplicata. | Guarda textual local, diferente da do supra. (`app/ponto/services/cartorio.py:1634`; `:1636`; `:1637`.) |
| FIO-DUP-C8 | Fecha outros duplicados apoiados no sobrevivente escolhido pelo gate. | Mesma guarda textual local. (`app/ponto/services/cartorio.py:1647`; `:1651`; `:1652`.) |

As quatro primeiras não têm guarda de conversa no chamador; todas passam pelas guardas gerais de premissa e pergunta viva em `reconciliar`, que são predicados diferentes. (`app/chamados/models.py:651`; `:682`; `:685`.)

Também existem estes caminhos indiretos; não devem ser confundidos automaticamente com o motivo CSF:

- **E2 → disputa → pai:** o cartório carimba perguntas, E2 fecha disputas e o signal reconcilia o chamado-pai. (`app/ponto/services/cartorio.py:993`; `app/chamados/reconciliador.py:595`; `:611`; `app/chamados/signals.py:42`.)
- **Cluster:** o cartório chama `cluster_espurio.reconciliar_colab`, que usa a mesma porta para outro motivo. (`app/ponto/services/cartorio.py:1028`; `app/ponto/services/cluster_espurio.py:189`.)
- **Pai acordado após fecho:** o signal pode agendar `reconciliar_chamados_colab`, que executa E2, órfãs e premissa morta. (`app/chamados/signals.py:350`; `:368`; `:369`; `app/chamados/reconciliador.py:1432`; `:1433`; `:1441`.)
- **Lavra → EventoCelula → consumidor:** pode retratar por absolvição da célula, premissa morta ou lastro. Nenhum desses caminhos consulta hoje a guarda do supra. (`app/ponto/portas/celula.py:796`; `:896`; `app/chamados/reconciliador.py:1368`; `:1372`; `:1383`; `app/ponto/services/lastro.py:400`.)

3. **VARREDURA, barrados e contador**

`classificar_dia` não recebe objetos de chamado nem consulta conversa. O CSF puro exige:

`tipo_dia == 'trabalho' and prev and real >= prev and chamados_vivos and todas_casadas`

`Todas_casadas` exige células não vazias e número de casadas igual ao total. (`app/ponto/supra_juiz.py:204`; `:463`; `:467`.)

O comando seleciona colaboradores ativos da empresa, pula quem não possui batidas na janela e carrega chamados dos `FURO_MODULES`, associados ao dia pelo juiz do catálogo. (`app/ponto/management/commands/supra_juiz.py:110`; `:113`; `:117`; `:186`; `:199`.)

Hoje há duas decisões distintas:

- A classificação local alimenta placar, exemplos e alarme.
- A escrita obedece à **lavra** `cobranca_indevida` e aos códigos dos protestos; sem lavra conta `CSF_SEM_LAVRA`, sem código conta `CSF_LAVRA_SEM_CODIGO`, sem retratar. (`app/ponto/management/commands/supra_juiz.py:239`; `:246`; `:248`; `:250`; `:253`.)

A guarda roda antes da cobrança órfã e também em dry-run. Chamado barrado soma `CSF_COM_CONVERSA_PRESERVADO` e recebe `continue`; não chega a `retratar`. (`app/ponto/management/commands/supra_juiz.py:261`; `:263`; `:275`.)

Com `--executar` e trava T6.1 desligada, chama o retratador. Retorno verdadeiro soma `CSF_RETRATADO`; falso soma `CSF_RETRATACAO_RECUSADA`; sem flag soma `CSF_RETRATAVEL_DRY`. (`app/ponto/management/commands/supra_juiz.py:45`; `:279`; `:286`; `:288`; `:290`; `:295`.)

| Operação sobre `CSF_RETRATADO` | Local |
|---|---|
| Nasce como chave possível de `Counter` local, por execução. | `app/ponto/management/commands/supra_juiz.py:105` |
| Única escrita do contador encontrada: incremento após retorno verdadeiro. | `app/ponto/management/commands/supra_juiz.py:288` |
| Leitura para stdout por `most_common()`. Chave não incrementada não gera linha explícita de zero. | `app/ponto/management/commands/supra_juiz.py:299` |
| Testes leem presença/ausência da chave. | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:122`; `:153`; `:167` |

O alarme A6 não lê esse contador: consulta `FURO_SEM_COBRANCA` e `COBRANCA_SEM_FURO`. O cron ainda contém `supra_juiz --executar`, dependente do cartório. (`app/ponto/management/commands/supra_juiz.py:305`; `app/config/crons.py:599`.)

4. **Porta comum e alcance das alternativas, sem escolher**

O caminho comum é:

`cartório → reconciliar`  
`supra → retratar → _retratar → reconciliar`  
`reconciliar → _decisao_do_fechamento → _gravar_estado`

(`app/ponto/services/cartorio.py:1559`; `app/ponto/management/commands/supra_juiz.py:286`; `app/chamados/reconciliador.py:544`; `:523`; `app/chamados/models.py:651`; `:654`.)

`_gravar_estado` escreve status, carimbo de resolução, via e urgência. A reconciliação usa via **`sistema`**, não uma via exclusiva de CSF ou cartório. (`app/chamados/models.py:201`; `:228`; `:242`; `:654`; `app/chamados/catalogo/motor.py:95`.)

**Dentro da decisão da porta:** `_decisao_do_fechamento` é compartilhada por `reconciliar(registrar=True)` e `reconciliaria(registrar=False)`. Uma guarda ali mantém ato e previsão juntos, mas, se incondicional, muda toda reconciliação. (`app/chamados/models.py:651`; `:661`; `:663`.)

**Nos dois caminhos:** preserva os demais usos da porta, mas cartório representa vários sítios e efeitos indiretos, não somente `:1559`; precisa impedir que o mesmo chamado protegido feche por marco, adiado, signal ou consumidor. (`app/ponto/services/cartorio.py:998`; `:1525`; `:1559`; `app/chamados/signals.py:42`; `app/chamados/reconciliador.py:1368`.)

Censo dos demais chamadores **diretos** de `reconciliar`, fora do cartório:

| Arquivo | Função e sítios |
|---|---|
| `app/chamados/signals.py` | `disputa_fechada_fecha_chamado_pai`:42 |
| `app/chamados/reconciliador.py` | `_retratar`:523 |
| `app/chamados/services/adesao.py` | `sincronizar_adesao`:115,150 |
| `app/chamados/services/feriado_reavaliar.py` | `encerrar`:47 |
| `app/chamados/services/limbo_folgas.py` | `aceitar_sugestao`:101; `agrupar_furo`:283,286 |
| `app/chamados/services/silencio_ausencia.py` | `reconciliar_por_ausencia`:101 |
| `app/chamados/services/materializacao.py` | `_materializar_ausencia_de_resposta`:587; `_reconciliar_destino_e_clones/_fecha`:776 |
| `app/chamados/management/commands/auditar_chamados_orfaos.py` | `handle`:67 |
| `app/colaboradores/services/pos_vinculo.py` | `fechar_adesao_com_batida`:42 |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py` | `_marcar`:140 |
| `app/ponto/services/triagem_batida.py` | `_autoresolve_f2_f4`:597 |
| `app/ponto/services/veredito_celula.py` | `fechar_o_fio_do_dia`:202 |
| `app/ponto/services/ausencia.py` | `fechar_chamado_da_ausencia`:694 |
| `app/ponto/services/cluster_espurio.py` | `reconciliar_colab`:189 |
| `app/ponto/services/entrada_sincronizada.py` | `resolver_cobranca_da_entrada`:31 |
| `app/ponto/services/cobertura.py` | `designar_cobertura`:122; `declarar_cobertura_passada`:317 |
| `app/ponto/services/lastro.py` | `fechar_pelo_marco_colab`:400 |
| `app/ponto/management/commands/detectar_intervalo_ausente.py` | `handle`:138 |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `_ata_sem_furo`:151 |
| `app/ponto/management/commands/detectar_ausencias.py` | `handle`:158; `_fechar_por_afastamento`:824; `_fechar_por_cobertura`:834; `_fechar_chamado`:845 |

A mudança dentro de `_decisao_do_fechamento` afeta também estes leitores de previsão: lastro, vigia de reconciliação e dry/alarme de intervalo. (`app/ponto/services/lastro.py:318`; `app/chamados/management/commands/reconciliar_chamados.py:79`; `app/ponto/management/commands/detectar_intervalo_ausente.py:138`.)

Uma guarda somente em `retratar` não alcança o cartório direto. Além do supra, esse wrapper atende justificativa, geofence, grade, fantasmas, vínculo, alertas de turno e retratação por lei; `_retratar` tem ainda entradas internas de absolvição e premissa morta. (`app/ponto/services/justificativa.py:46`; `app/ponto/services/geofence.py:441`; `app/ponto/management/commands/reconciliar_grade.py:309`; `app/ponto/management/commands/reconciliar_fantasmas.py:280`; `app/ponto/management/commands/reconciliar_vinculo.py:68`; `app/ponto/management/commands/processar_alertas_turno.py:216`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`; `app/chamados/reconciliador.py:1004`; `:1464`.)

Uma guarda indiscriminada em `_gravar_estado` alcançaria também transição humana, cancelamento, análise, reabertura, cascade e tranca. `encerrar()` é outra porta; não é a chamada direta usada pelo CSF desses dois caminhos. (`app/chamados/models.py:450`; `:788`; `:806`; `:1442`; `:1619`; `:1639`; `app/chamados/reconciliador.py:523`.)

5. **Testes e selos existentes** [nao verificado]

| Cobertura | Referências |
|---|---|
| Guarda CSF: vazio, sem timestamp, resposta na disputa e resposta direta. | `app/ponto/tests/test_supra_juiz_executor.py:38` |
| Classificação CSF: todas casadas; missing, `None`, sem vivo e realizado insuficiente. | `app/ponto/tests/test_supra_juiz_cobranca.py:19` |
| `--executar` real: lavra concorde preserva; indevida retrata; lavra prevalece sobre classificação local; ausência de lavra/código; recusa não conta como retratação. | `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:89`; `:106`; `:116`; `:124`; `:134`; `:142`; `:155` |
| T6.1: constante única, valor vigente, flag/trava, placar travado e texto delegado ao juiz. | `app/ponto/tests/test_t61_retratacao_em_seco.py:29`; `:40`; `:47`; `:57`; `:64` |
| Cartório: CSF na passada pura, fechamento aplicado e fio não ressuscitado. | `app/ponto/tests/test_o261_cobrado_le_o_fio.py:393`; `:412`; `:416`; `:420` |
| Batida real → cartório → E2 → encerramento sem pergunta viva pendurada em terminal. | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:358`; `:390` |
| Porta geral: recusa pergunta viva; permite reata; dry sem escrita. | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`; `:94`; `:135`; `:143`; `:152` |
| Consumidor/lastro: marco satisfeito fecha apesar de outro furo; marco apagado permanece vivo. | `app/ponto/tests/test_bug115_lampada_do_marco.py:185`; `:207` |
| Universo cartório/supra e exclusão de chamado sem dia. | `app/ponto/tests/test_cartorio_so_chamado_de_batida.py:53`; `:63`; `:74` |
| Texto protege contra ressuscitação; não demonstra proteção CSF. | `app/ponto/tests/test_hx_cartorio_ressuscita.py:68` |
| Custo do caminho real e da guarda, com chamados fechando ou permanecendo. | `app/ponto/tests/test_selo_chokepoint_escrita.py:387`; `:392`; `:422`; `:427`; `:432` |

Esses casos não constituem prova executada da guarda CSF no evento; execução e resultado nesta cópia [nao verificado].

6. **Lista fechada para conferência de arquivos em comum**

Sem escolher entre guarda na porta e nos caminhos, uma lista única exata de alterações futuras seria presumir a implementação. Abaixo está a **união fechada de reserva proposta** para as duas alternativas, incluindo instrumentação e testes; não é um diff realizado. Alterações futuras [nao verificado]. [nao verificado]

| Arquivo reservado | Razão e sítio atual |
|---|---|
| `app/chamados/juizes.py` | Destino possível do predicado compartilhado; juízes de leitura já residem aqui. (`:1980`; `:2021`.) |
| `app/core/juizes.py` | Registrar o endereço da autoridade existente. (`:272`.) |
| `app/ponto/supra_juiz.py` | Extrair/reexportar a guarda. (`:539`.) |
| `app/ponto/management/commands/supra_juiz.py` | Consumir autoridade e tornar a medição explícita. (`:257`; `:288`; `:299`.) |
| `app/ponto/services/cartorio.py` | Caminhos diretos e observabilidade do evento. (`:998`; `:1525`; `:1559`.) |
| `app/chamados/models.py` | Alternativa de guarda na decisão comum e contexto da trilha. (`:663`; `:292`.) |
| `app/chamados/reconciliador.py` | Retratador, E2 e consumidor pós-lavra na alternativa por caminhos. (`:503`; `:549`; `:1368`.) |
| `app/chamados/signals.py` | Fechamento indireto do pai na alternativa por caminhos. (`:42`.) |
| `app/ponto/services/lastro.py` | Fechamento pelo consumidor na alternativa por caminhos. (`:400`.) |
| `app/config/crons.py` | Segunda etapa: retirar execução somente depois da prova exigida. (`:599`; `app/docs/BACKLOG.md:1395`.) |
| `app/ponto/tests/test_supra_juiz_executor.py` | Matriz exata do predicado. (`:38`.) |
| `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py` | Integração do comando e contador. (`:89`; `:155`.) |
| `app/ponto/tests/test_o261_cobrado_le_o_fio.py` | CSF no cartório. (`:393`.) |
| `app/chamados/tests/test_o261e_f1_porta_encerramento.py` | Porta comum, dry e evento real. (`:152`; `:358`.) |
| `app/ponto/tests/test_bug115_lampada_do_marco.py` | Fechamento alternativo por marco/consumidor. (`:185`.) |
| `app/ponto/tests/test_hx_cartorio_ressuscita.py` | Diferença entre texto e timestamp no bloco do fio. (`:68`.) |
| `app/ponto/tests/test_selo_chokepoint_escrita.py` | Custo da guarda no caminho quente. (`:432`.) |
| `app/docs/JUIZES.md` | Ponteiro documental da autoridade movida. (`:10`.) |

Arquivos das outras obras — E4B c6, TRIAGEM, UI-PAINEL e BO-ROTULO — não vieram no dado; interseção e liberação para abrir construtor [nao verificado].

7. **Sonda somente leitura**

A medição histórica **exata** solicitada não é recuperável pelo esquema mostrado: a trilha grava status anterior, status novo e via, sem motivo CSF ou sítio chamador; o ator identifica processo/request, não a pilha. Além disso, relações e timestamps atuais podem ter mudado: ressuscitação limpa `respondida_em`. (`app/chamados/models.py:292`; `app/core/observ.py:22`; `:94`; `app/chamados/models.py:1563`.)

`CSF_RETRATADO` também não possui valor atual persistido: é contador local impresso pelo comando. Portanto a sonda mantém ambos os resultados exatos como **“nao medido”** e entrega separadamente o que o banco permite: um **proxy** de reconciliações atribuídas ao processo `processar_cartorio`, com a guarda barrando hoje e timestamps anteriores ao fecho nas relações atuais. Isso não identifica todos os eventos do cartório nem prova motivo CSF. (`app/ponto/management/commands/supra_juiz.py:105`; `:288`; `:299`; `app/core/observ.py:31`; `app/chamados/models.py:292`.)

O agrupamento abaixo usa **motivo da pergunta**, não motivo do fechamento; um evento com motivos diferentes entra em mais de um grupo. Os campos vêm de `PerguntaDisputa` e da trilha; a avaliação atual chama o predicado real. (`app/chamados/models.py:2432`; `app/core/models.py:298`; `:300`; `:307`; `app/ponto/supra_juiz.py:539`.)

Código para fornecer ao `bin/sonda_leitura.sh <arquivo.py>`; produção é o padrão e o wrapper executa shell Django no schema configurado. Não foi executado em produção [nao verificado]. (`bin/sonda_leitura.sh:56`; `:93`; `:235`.)

```python
import json

resultado = {
    "CSF_CARTORIO_30D_POR_EMPRESA_E_MOTIVO": "nao medido",
    "CSF_RETRATADO": "nao medido",
    "proxy": "nao medido",
}
tr

[truncado em 20000 de 25069 caracteres]

## 11/11 13:32 · sessao fadf31a99bdf · MAPA-20 · censo · origem=Code

A premissa de escritor único é contradita pelo anexo: ele relata geração pelo montador e edição externa por `PRONTO-AUSENTE-RASCUNHO`. O código continua usando essa exclusividade como fundamento do parser (`opus_final_ffa_achados.txt:6`; `bin/mapa20_valida.py:10`).

As referências aos anexos abaixo contam as linhas do texto fornecido: cada aval ocupa a linha 1; no opus, o cabeçalho é a linha 1 e os cinco bloqueantes ocupam as linhas 2–6. A identidade Git da cópia, o mapa vivo, o montador e o estado publicado estão [nao verificado]. [nao verificado]

**1. Escritores**

| Escritor/caminho | Evidência e limite |
|---|---|
| `logs/MAPA-20/montar_mapa.py` | Declarado como produtor em `bin/mapa20_valida.py:10` e `bin/mapa20_valida.py:128`. Implementação e linha que grava o mapa: [nao verificado]. |
| Edição externa `PRONTO-AUSENTE-RASCUNHO` | Segundo escritor informado pelo anexo; comando, arquivo executor e linha da gravação: [nao verificado] (`opus_final_ffa_achados.txt:6`). |
| Outro script de produção | [nao verificado]. |
| Escritores das fixtures | Regravação em `bin/tests/test_mapa20_valida.sh:154`, cópia verde em `:177` e escrita vermelha em `:178`; todos no repositório temporário criado em `:146`. |
| Publicador | Lê o mapa e escreve uma foto temporária; essa operação não regrava o mapa de origem (`bin/relato.sh:94`, `bin/relato.sh:97`). |

Portanto, “único escritor = montador” não está provado: é premissa do código, contrariada pelo dado fornecido (`bin/mapa20_valida.py:10`; `opus_final_ffa_achados.txt:6`).

A lei exige **“1 escritor canonico + tripwire”** (`CLAUDE.md:32`). Os dois avais determinam regravação por evento e julgamento no publicador, mas não nomeiam um escritor exclusivo; escolher montador, edição manual ou outro por Ronald seria [nao verificado] (`aval_mapa_ate_o_20_1111.txt:1`, item 3; `aval_mapa20_pronto_x_de_n_1111.txt:1`, item 4). [nao verificado]

**2. Matriz da porta**

Convenções de publicação:

- **A — aceita:** foto selecionada, raspada, adicionada ao Git e enviada; rc final 0 se as etapas seguintes concluírem (`bin/relato.sh:97`, `:98`, `:143`, `:153`, `:164`, `:165`).
- **R — retém:** candidato excluído; cópia anterior, se houver, é raspada novamente; resto segue; rc final 1 (`bin/relato.sh:100`, `:102`, `:105`, `:158`, `:165`).
- Os pares de rc abaixo são **validador → publicador**, supondo sucesso das etapas posteriores. Falhas posteriores também podem produzir rc 1 (`bin/mapa20_valida.py:225`; `bin/relato.sh:143`, `:163`, `:164`).

| Estado de entrada | Juiz → operação | Publicação | rc | Prova que morde / lacuna |
|---|---|---|---|---|
| Tabela sintaticamente válida | Cabeçalho, estrutura, células, X/N e universo (`bin/mapa20_valida.py:154`, `:166`, `:178`, `:190`, `:202`) | A | 0→0 | Verde em `bin/tests/test_mapa20_valida.sh:42`; igualdade X=N em `:43`. Não prova a verdade de X nem de N (`bin/mapa20_valida.py:190`, `:192`). |
| Obra ausente ou coluna vazia | Acumula problema (`bin/mapa20_valida.py:178`, `:202`) | R | 1→1 | Cada obra e SEM DONO: `bin/tests/test_mapa20_valida.sh:45`, `:46`, `:47`. |
| “encerrada” em célula, X<N | `_afirma_encerrada` e comparação de completude (`bin/mapa20_valida.py:68`, `:200`) | R | 1→1 | `bin/tests/test_mapa20_valida.sh:55`; PRONTO AUSENTE encerrada em `:56`; ênfase em `:65`, `:66`. |
| “encerrada” fora da tabela: ATX, setext, negrito ou lista, separados da tabela | **SEM juiz de estado:** só percorre células do bloco (`bin/mapa20_valida.py:147`, `:166`, `:200`) | A | 0→0 | **SEM PROVA** dessas quatro formas. Os casos `bin/tests/test_mapa20_valida.sh:62` e `:63` tratam outras tabelas, não essa prosa. |
| Invisível da categoria Cf, como U+200B | Recusa global por categoria Unicode (`bin/mapa20_valida.py:132`) | R | 1→1 | `bin/tests/test_mapa20_valida.sh:71`. |
| Combinantes U+034F/U+FE00 ou homóglifo cirílico em “encerrada” | **SEM juiz dessas classes:** filtro só Cf; reconhecimento literal do estado (`bin/mapa20_valida.py:58`, `:132`) | A | 0→0 | **SEM PROVA** dessas classes; o teste vizinho cobre somente U+200B (`bin/tests/test_mapa20_valida.sh:71`). |
| FF, FS ou U+2028 entre E3 e E4B, sem LF | `splitlines()` cria duas linhas para o juiz; **SEM juiz da divergência com GFM** (`bin/mapa20_valida.py:125`) | A | 0→0 | **SEM PROVA**; contracaso descrito em `opus_final_ffa_achados.txt:4`. |
| N com fonte alheia a aval/TICKETS/BACKLOG | `_fonte_ruim` confere existência, raiz e linha não vazia; não confere elegibilidade, vínculo à obra ou contagem (`bin/mapa20_valida.py:73`, `:78`, `:88`) | A | 0→0 | **SEM PROVA da regra de fonte.** O verde usa o comentário `bin/relato.sh:2` como fonte (`bin/tests/test_mapa20_valida.sh:27`, `:42`). |
| SEM DONO com `n/a (SEM DONO)` | Confere literal e executa `continue` (`bin/mapa20_valida.py:182`, `:185`) | A | 0→0 | Presença/vazias em `bin/tests/test_mapa20_valida.sh:45`; rejeição de X/N em `:72`. |
| SEM DONO com “encerrada” | **SEM juiz de encerramento:** sai antes da guarda (`bin/mapa20_valida.py:185`, `:200`) | A | 0→0 | **SEM PROVA** desse estado; `bin/tests/test_mapa20_valida.sh:72` testa outro conteúdo. |
| RASCUNHO como título/prosa antes de tabela válida | **SEM juiz específico:** prosa sem pipes não entra na avaliação de estado (`bin/mapa20_valida.py:137`, `:144`, `:166`) | A | 0→0 | **SEM PROVA** específica. Se substituir X/N por `RASCUNHO`, é recusado pela gramática (`bin/mapa20_valida.py:198`). Motivo do rc 1 do mapa vivo: [nao verificado]. |
| Legenda literal de `montar_mapa.py:103` | Literal e implementação: [nao verificado] | [nao verificado] | [nao verificado] | **SEM PROVA** específica; exigência de preservá-la consta de `opus_final_ffa_achados.txt:3`. |
| X declarado sem prova escrita no RELATO | **SEM juiz de lastro:** compara inteiros e verifica somente a citação de N (`bin/mapa20_valida.py:190`, `:192`) | A | 0→0 | **SEM PROVA** de correspondência item→prova; fixture usa falta genérica em `bin/tests/test_mapa20_valida.sh:27`. |
| `status/encerrada.sim` em célula | Token removido como caminho antes da busca por estado (`bin/mapa20_valida.py:52`, `:65`) | A | 0→0 | **SEM PROVA** desse literal. Há teste de `logs/encerrada.md:10` em `bin/tests/test_mapa20_valida.sh:102`; correspondência ao `test:414` do opus: [nao verificado]. |

Os 105 casos existentes de conteúdo foram avaliados em memória contra `problemas()`, sem divergências das expectativas codificadas; isso cobre as fixtures de `bin/tests/test_mapa20_valida.sh:42` até `:114`, não execução integral do shell, publicação ou render. O shell cria arquivos e repositórios, portanto não foi executado nesta leitura (`bin/tests/test_mapa20_valida.sh:13`, `:37`, `:169`).

**Transições depois do julgamento**

| Entrada/decisão | Operação → resultado previsto | rc ao chamador | Prova |
|---|---|---|---|
| Candidato aprovado | Raspa, adiciona, commita e envia (`bin/relato.sh:143`, `:153`, `:163`, `:164`) | 0 (`bin/relato.sh:165`) | `bin/tests/test_mapa20_valida.sh:196`, `:197`. |
| Candidato recusado ou ausente | Conserva conteúdo anterior após nova raspagem; restante pode publicar (`bin/relato.sh:100`, `:105`) | 1, inclusive sem mudanças (`bin/relato.sh:158`, `:165`) | `bin/tests/test_mapa20_valida.sh:198`, `:199`, `:200`. |
| Raspagem/rename do retido falha | Remove cópia local; remoção entra no commit (`bin/relato.sh:109`, `:152`, `:153`) | 1 (`bin/relato.sh:165`) | `bin/tests/test_mapa20_valida.sh:201`. |
| Remoção também falha | Interrompe antes do commit/push (`bin/relato.sh:110`) | 1 | `bin/tests/test_mapa20_valida.sh:204`. |
| Vivo muda depois da foto | Publica foto anterior à regravação (`bin/relato.sh:97`, `:98`) | 0 no cenário testado | `bin/tests/test_mapa20_valida.sh:154`, `:203`. |
| Raspagem transforma candidato aprovado | Texto alterado passa só pela conferência de privacidade; **SEM novo julgamento MAPA-20** (`bin/raspar_publico.py:31`, `:36`, `:40`; `bin/relato.sh:143`) | Pode terminar 0 (`bin/relato.sh:165`) | **SEM PROVA de conformidade após raspagem**; raspador falso apenas prefixa texto (`bin/tests/test_mapa20_valida.sh:166`, `:189`). |
| Commit falha | Interrompe antes do push (`bin/relato.sh:163`) | 1 | `bin/tests/test_mapa20_valida.sh:202`. |
| Push falha | Retorna falha; estado remoto resultante: [nao verificado] (`bin/relato.sh:164`) | 1 | **SEM PROVA específica** entre os casos de `bin/tests/test_mapa20_valida.sh:196`–`:204`. |

**3. GFM e confronto com o aval nas lacunas**

As referências literais de lei são:

- **A3:** “o publicador sai rc 1 se faltar no mapa alguma obra […] ou se uma linha tiver coluna vazia sem ‘nao medido’” (`aval_mapa_ate_o_20_1111.txt:1`, item 3). [nao verificado]
- **B1:** “N = itens numerados do PRONTO da obra”, com fonte no aval da obra, TICKETS ou BACKLOG; “X = itens com prova escrita no RELATO” (`aval_mapa20_pronto_x_de_n_1111.txt:1`, item 1).
- **B3/B4:** “‘encerrada’ so com X == N”; “rc 1 […] se aparecer ‘encerrada’ com X < N” (`aval_mapa20_pronto_x_de_n_1111.txt:1`, itens 3–4).

| Lacuna | O que chega ao leitor GFM / confronto |
|---|---|
| Encerrada fora da tabela | O anexo relata título ATX/setext e negrito exibindo a afirmação que o juiz ignora; lista também fica fora do percurso das células. Com obra X<N, contraria B3/B4 (`opus_final_ffa_achados.txt:3`; `bin/mapa20_valida.py:166`, `:200`). Render remoto: [nao verificado]. |
| Mn e homóglifo | A grafia modificada passa com 1 de 2; aparência exata depende do render/fonte [nao verificado]. Se apresenta “encerrada” ao leitor, contorna B3/B4 (`opus_final_ffa_achados.txt:2`; `bin/mapa20_valida.py:58`, `:132`). |
| FF/FS/U+2028 | Segundo o anexo, GFM não faz a mesma quebra e E4B perde sua linha visível; contraria A3, apesar da presença lógica para Python (`opus_final_ffa_achados.txt:4`; `bin/mapa20_valida.py:125`). |
| Fonte imprópria ou N inventado | A tabela pode exibir X/N e uma citação existente que não fundamenta N; contraria B1. O próprio verde legitima esse caso (`bin/tests/test_mapa20_valida.sh:27`, `:42`; `bin/mapa20_valida.py:73`). |
| X sem prova | A tabela exibe “resultados provados” sem conferência do lastro; aceitar X inventado contraria B1. Que X do mapa vivo está nessa situação: [nao verificado] (`bin/mapa20_valida.py:190`, `:192`). |
| SEM DONO encerrada | O texto “encerrada” pode coexistir com `n/a (SEM DONO)`; o código não demonstrou X=N. Os avais não fornecem autorização literal dessa exceção de encerramento (`bin/mapa20_valida.py:182`, `:185`; `aval_mapa20_pronto_x_de_n_1111.txt:1`, item 3). |
| RASCUNHO | Um prefixo separado aparece junto da tabela; o rótulo sozinho não é proibido pelos avais nem dispensa A3/B1/B4. O defeito concreto do mapa vivo não foi fornecido: [nao verificado] (`bin/mapa20_valida.py:137`, `:166`; `aval_mapa20_pronto_x_de_n_1111.txt:1`). |
| Legenda literal | Conteúdo e render: [nao verificado]. Não cabe inventar a legenda nem rejeitar toda ocorrência de “encerrada” indiscriminadamente; o anexo exige que a legenda passe (`opus_final_ffa_achados.txt:3`). |
| Caminho `status/encerrada.sim` | O texto do caminho permanece no documento, embora seja apagado da análise de estado. Um nome de arquivo isolado não prova afirmação de encerramento; uso enganoso no mapa vivo: [nao verificado] (`bin/mapa20_valida.py:52`, `:65`). |
| Transformação pela raspagem | O leitor recebe os bytes transformados, sem selo MAPA-20 posterior. Violação concreta de A3/B4 pelo raspado vivo: [nao verificado] (`bin/raspar_publico.py:31`, `:40`; `bin/relato.sh:143`). |
| Retenção e falha de push | O resultado visível depende da cópia anterior e do remoto; ambos [nao verificado]. Rc 1 não demonstra atualização nem conformidade da página anterior (`bin/relato.sh:105`, `:158`, `:164`, `:165`). |

**4. Ordem de cura por dependência e perímetro fechado**

Esta é uma proposta de desenho, sem aplicar mudanças; a cura na origem e a testemunha lendo a mesma autoridade são exigências existentes (`CLAUDE.md:17`, `:20`).

1. **Definir o escritor canônico e seu tripwire.** O juiz não pode apoiar sua abrangência numa exclusividade apenas declarada (`CLAUDE.md:32`; `bin/mapa20_valida.py:10`).
2. **Ancorar N e X nas autoridades do aval.** Existência de linha não substitui lista numerada vinculada à obra; igualdade numérica não substitui prova de X (`aval_mapa20_pronto_x_de_n_1111.txt:1`, item 1; `bin/mapa20_valida.py:73`, `:190`).
3. **Ancorar o julgamento no documento que o leitor recebe.** Cobrir conteúdo visível fora da tabela, classes Unicode e separação de linhas, preservando a legenda literal quando disponível (`bin/mapa20_valida.py:125`, `:132`, `:166`; `opus_final_ffa_achados.txt:3`).
4. **Ancorar a publicação nos bytes finais julgados.** A raspagem muda texto depois do selo; o mesmo juiz MAPA-20 precisa alcançar esse resultado antes de sua publicação (`bin/relato.sh:97`, `:143`; `bin/raspar_publico.py:31`).
5. **Ancorar o tratamento operacional no resultado da porta.** Preservar o rc 1 exigido pelo aval, distinguindo retenção, publicação parcial e falha operacional para o consumidor (`aval_mapa20_pronto_x_de_n_1111.txt:1`, item 4; `bin/relato.sh:100`, `:110`, `:164`; `bin/hasner-relato.service:15`).
6. **Fazer os testes morderem cada transição e os bytes finais.** Os atuais testes de publicação substituem o raspador real (`CLAUDE.md:28`; `bin/tests/test_mapa20_valida.sh:158`, `:166`).

Perímetro fechado proposto para revisão:

| Arquivo | Papel |
|---|---|
| `logs/MAPA-20/montar_mapa.py` | Produtor referido pelo juiz; implementação e escolha como escritor canônico [nao verificado] (`bin/mapa20_valida.py:10`). |
| `logs/MAPA-20/mapa.md` | Artefato de entrada; conteúdo vivo [nao verificado] (`bin/relato.sh:97`). |
| `bin/mapa20_valida.py` | Juiz único de conformidade (`bin/mapa20_valida.py:124`). |
| `bin/relato.sh` | Foto, raspagem, retenção, Git e rc (`bin/relato.sh:94`, `:105`, `:143`, `:153`, `:165`). |
| `bin/tests/test_mapa20_valida.sh` | Regressões do juiz e das transições (`bin/tests/test_mapa20_valida.sh:42`, `:138`, `:196`). |
| `bin/hasner-relato.service` | Consumidor operacional da execução (`bin/hasner-relato.service:15`). |

O raspador e seu juiz de privacidade são dependências lidas; a proposta acima resolve a ordem no publicador, sem propor mudança na regra de privacidade (`bin/raspar_publico.py:16`, `:31`; `bin/relato.sh:143`). Fechamento executável desse perímetro antes de conhecer o produtor e a legenda: [nao verificado].

**5. Quem lê o rc**

| Consumidor | Leitura efetiva |
|---|---|
| systemd, via `hasner-relato.service` | Recebe a saída do processo `ExecStart`, declarado `oneshot`; o arquivo não configura tratamento específico de retenção ou discriminação de resultados (`bin/hasner-relato.service:13`, `:15`, `:19`, `:21`). Unit instalada e alertas externos: [nao verificado]. |
| `hasner-relato.timer` | Agenda execuções; não contém leitura ou classificação do rc (`bin/hasner-relato.timer:5`, `:6`, `:8`). |
| Teste do publicador | Executa recortes, captura `$?`, lê e compara rc com conteúdo do remoto sintético (`bin/tests/test_mapa20_valida.sh:138`, `:182`, `:183`, `:187`, `:191`). |
| Outros chamadores produtivos, aliases ou monitoramento | [nao verificado]. |

Logo, “sem leitor” deve ser qualificado: há o consumidor do processo pelo serviço e há o teste; **tratamento que interprete a retenção do MAPA-20** não aparece na unit fornecida (`bin/hasner-relato.service:15`; `bin/tests/test_mapa20_valida.sh:191`).

Rc 1 é ambíguo: representa retenção com restante publicado, nenhuma mudança, interrupção antes de publicar ou falha de push (`bin/relato.sh:100`, `:110`, `:158`, `:164`, `:165`).

Pergunta de lei pendente: **quem Ronald designa como único escritor canônico do mapa?** A obrigação de unicidade existe; a nomeação não consta dos dois avais fornecidos (`CLAUDE.md:32`; `aval_mapa_ate_o_20_1111.txt:1`; `aval_mapa20_pronto_x_de_n_1111.txt:1`).

perguntas de lei: 1

## 11/11 13:32 · sessao c24a41a8a57f · PROPORCAO-POR-RISCO · verificador · origem=Code

A v2 fecha os exemplos literais de grafia e cabeçalho da v1, mas mantém falsos verdes por seleção dos nós do AST e pela exclusão de testes. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:106`)

Todos os rc abaixo são **deduzidos por leitura**, sem executar selo ou suíte. Execução dos exemplos, efeitos em produção, banco e logs: **[nao verificado]**. [nao verificado]

1. **Fechamento dos achados da v1**

| Achado v1 | Situação na v2 / cura | Caso que o morde |
|---|---|---|
| Oito métodos ORM com espaço depois do ponto | **Fecha o exemplo.** Compara `Attribute.attr` com os oito nomes, independentemente da grafia. (`bin/selo_proporcao.py:28`, `bin/selo_proporcao.py:55`) | Oito `orm_$m`, mais `espaco_no_ponto`. (`bin/tests/test_selo_proporcao.sh:28`, `bin/tests/test_selo_proporcao.sh:31`) |
| Quebra antes de `()` | **Fecha quando o atributo também é adicionado. Não fecha continuação isolada**, demonstrada adiante. (`bin/selo_proporcao.py:106`) | `quebra_de_linha` adiciona o arquivo inteiro; não cobre atributo antigo. (`bin/tests/test_selo_proporcao.sh:32`) |
| Alias `gravar = obj.save; gravar()` | **Fecha o exemplo:** a referência ao atributo basta. (`bin/selo_proporcao.py:55`) | `alias` contém somente a atribuição; prova reconhecimento da referência. (`bin/tests/test_selo_proporcao.sh:33`) |
| `getattr(obj, "save")()` | **Fecha a string literal exata.** (`bin/selo_proporcao.py:59`) | `getattr_string`. (`bin/tests/test_selo_proporcao.sh:34`) |
| `(obj.save)()` | **Fecha pelo atributo.** (`bin/selo_proporcao.py:55`) | Sem caso específico; os casos próximos não reproduzem essa forma. (`bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:32`) |
| `#` dentro de string | **Fecha:** analisa o fonte pelo AST, sem truncar no `#`. (`bin/selo_proporcao.py:101`) | `cerquilha_string`. (`bin/tests/test_selo_proporcao.sh:35`) |
| Signal por alias e `getattr` | **Fecha o exemplo v1 pelo `.connect`**, mesmo com nome de signal calculado. (`bin/selo_proporcao.py:30`, `bin/selo_proporcao.py:55`) | `signal_connect` isola esse gatilho; não reproduz o exemplo inteiro. (`bin/tests/test_selo_proporcao.sh:39`) |
| Command que escreve por alias | **Fecha o exemplo pela referência `.create`.** Continua sem regra específica de command. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:91`) | `command_alias`; não contém invocação do alias. (`bin/tests/test_selo_proporcao.sh:43`) |
| Migration por rename sem linhas adicionadas | **Fecha:** lê destino pelo protocolo NUL e aplica regra por caminho. (`bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:86`, `bin/selo_proporcao.py:94`) | `migration_rename`. (`bin/tests/test_selo_proporcao.sh:42`) |
| `.py` escondido como binário/`-diff` | **Fecha o exemplo:** fonte por `git show`, diff com `--text`. (`bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:100`) | `binario_attr`. (`bin/tests/test_selo_proporcao.sh:44`) |
| Caminho com espaço/tabulação ou Unicode escapado | **Fecha a dependência do cabeçalho textual:** caminhos NUL e `surrogateescape`. (`bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:89`) | `caminho_espaco` e `caminho_unicode`; falta caso específico com tabulação. (`bin/tests/test_selo_proporcao.sh:45`, `bin/tests/test_selo_proporcao.sh:46`) |
| `+++ /dev/null` dentro do conteúdo | **Fecha:** caminho não vem mais de `+++`. (`bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:100`) | Sem caso correspondente na lista. (`bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:55`) |
| Revisão interpretada como opção Git | **Fecha:** rejeita `-` inicial e resolve ambos os commits antes do diff. (`bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:80`) | `opcao_como_revisao`. (`bin/tests/test_selo_proporcao.sh:58`) |
| Produção importa módulo excluído de teste | **Parcial:** import estático adicionado é marcado; import antigo, dinâmico ou reexportado continua permitindo fuga. (`bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:106`) | `prod_importa_teste` testa apenas o import, sem criar módulo escritor; não cobre `tests.writer` separadamente. (`bin/tests/test_selo_proporcao.sh:47`) |

Os achados v1 sobre isolamento/preparo dos testes estão detalhados no item 5; os de rc, consumidores, atalho da régua e worktree, nos itens 4 e 6. (`bin/tests/test_selo_proporcao.sh:20`, `bin/tests/test_selo_proporcao.sh:38`, `bin/regua.sh:86`, `bin/tests/test_hook_nao_e_copia.sh:21`)

2. **Escapes da v2**

A expressão precisa é “nenhum **nó reconhecido como escrita** cruza a linha adicionada”: `Call`, `Expr`, `FunctionDef` e `Lambda` podem cruzá-la, mas não são candidatos em `nome_de_escrita`. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:104`)

Nos fragmentos abaixo, cada caso é independente e declarado B; os `.py` ficam fora de `tests/`, `migrations/` e `signals.py`, salvo indicação explícita. Essas condições determinam a seleção feita pelo selo. (`bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:98`)

**a) Continuação adicionada transforma referência antiga em chamada — rc 0.** O `Attribute` continua inteiramente na linha antiga; o `Call` novo não é reconhecido. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,2 +1,3 @@
 (obj.save
+()
 )
```

**b) `exec` e `eval` com escrita literal no código encapsulado — rc 0 em cada alternativa.** A string `"obj.save()"` não é exatamente `"save"` nem SQL; não existe análise recursiva desse código. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1 @@
+exec("obj.save()")
```

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1 @@
+eval("obj.save()")
```

**c) Decorador com código encapsulado — rc 0.** A expressão avaliada produz um decorador que chama `save`; para o selo, continua sendo uma constante não reconhecida. Compartilha o mecanismo de `eval`, não é uma exclusão geral de decoradores. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+@eval("lambda f: (obj.save(), f)[1]")
+def f(): pass
```

**d) F-string formando nome de método — rc 0.** Os fragmentos constantes são `"sa"` e `"ve"`; o selo não calcula o resultado da expressão. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1 @@
+getattr(obj, f"sa{'ve'}")()
```

**e) `setattr` instala um alias escritor — rc 0.** Nem `"acao"`, `"sa"`, `"ve"` nem o atributo `.acao` pertencem aos conjuntos reconhecidos. (`bin/selo_proporcao.py:28`, `bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+setattr(obj, "acao", getattr(obj, "sa" + "ve"))
+obj.acao()
```

**f) QuerySet por variável e método calculado — rc 0.** Não há resolução do método calculado nem do tipo de `qs`. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+qs = M.objects.all()
+getattr(qs, "up" + "date")(ativo=False)
```

**g) Cursor por alias e SQL fragmentado — rc 0.** `.execute` não é reconhecido e nenhuma constante isolada satisfaz a regex SQL. (`bin/selo_proporcao.py:28`, `bin/selo_proporcao.py:31`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,3 @@
+cur = connection.cursor()
+run = cur.execute
+run("DE" + "LETE FROM tabela")
```

**h) SQL em f-string — rc 0.** `"UPDATE "` e `" SET ativo=0"` ficam em constantes separadas; nenhuma contém o padrão completo exigido. (`bin/selo_proporcao.py:31`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1 @@
+cur.execute(f"UPDATE {tabela} SET ativo=0")
```

**i) Python com outro sufixo e carregamento explícito — rc 0.** O payload é descartado pelo sufixo; o carregador não contém nome reconhecido como escrita. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/payload.pyw
@@ -0,0 +1,2 @@
+from app.x.models import M
+M.objects.create()
--- /dev/null
+++ b/app/x/carregar.py
@@ -0,0 +1,2 @@
+import runpy
+runpy.run_path("app/x/payload.pyw")
```

**j) `.sql` com executor adicionado — rc 0.** O SQL fica fora dos `.py`; no executor, a constante é apenas um caminho. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/gravar.sql
@@ -0,0 +1 @@
+DELETE FROM tabela;
--- /dev/null
+++ b/app/x/executar.py
@@ -0,0 +1,3 @@
+from pathlib import Path
+with connection.cursor() as cur:
+    cur.execute(Path("app/x/gravar.sql").read_text())
```

**k) Import antigo de teste, com escritor novo no módulo excluído — rc 0.** Pressupondo que produção já importe `gravar`, alterar somente o corpo abaixo é ignorado. A escrita é adicionada agora; não se trata apenas de chamar helper que já escrevia. (`bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/test_writer.py
+++ b/app/x/test_writer.py
@@ -1,2 +1,2 @@
 def gravar(obj):
-    pass
+    obj.save()
```

**l) `import *` por reexportação — rc 0.** Se `app/x/__init__.py` já reexporta `gravar` de `.tests.fixt`, o import novo não menciona teste e o arquivo escritor é excluído. `import *` sozinho não oculta escrita adicionada em um `.py` comum; aqui a fuga combina reexportação e exclusão. (`bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:63`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/tests/fixt.py
+++ b/app/x/tests/fixt.py
@@ -1,2 +1,2 @@
 def gravar(obj):
-    pass
+    obj.save()
--- /dev/null
+++ b/app/x/usar.py
@@ -0,0 +1,2 @@
+from app.x import *
+gravar(obj)
```

**m) Template chama método escritor — rc 0 do selo.** O `.html` é ignorado; o método existente `rebaixar()` pode chegar a `self.save()`. Renderização efetiva, objeto no contexto e efeito persistido: **[nao verificado]**. Esse caso de helper já existente está no limite declarado do instrumento. (`bin/selo_proporcao.py:19`, `bin/selo_proporcao.py:98`, `app/chamados/models.py:488`, `app/chamados/models.py:498`)

```diff
--- a/app/templates/chamados/fio_colaborador.html
+++ b/app/templates/chamados/fio_colaborador.html
@@ -175,0 +176 @@
+{{ chamado.rebaixar }}
```

Não são escapes, isoladamente: `qs.update(...)`, `f"{obj.save()}"` e referência nova `run = cur.execute` acompanhada de `"DELETE FROM tabela"` dão **rc 1** pelos atributos ou pela constante SQL. Decorador que contenha diretamente atributo novo `.save` também é reconhecido. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:103`, `bin/selo_proporcao.py:117`)

3. **Falsos positivos**

| Exemplo adicionado | Rc B deduzido / motivo |
|---|---|
| Palavra `update` em template `.html` | **0**; o arquivo é ignorado. Não é falso positivo da v2. (`bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`) |
| `rotulo = "update"` ou docstring exatamente `"create"` em `.py` | **1**; constante exata, sem exigir chamada. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:117`) |
| `d.update(...)`, com `d` sendo dict | **1**; não resolve tipo. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:117`) |
| `s.update(...)`, com `s` sendo set | **1**, pelo mesmo atributo. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:117`) |
| `[].delete()` | **1**; aceita sintaxe, sem verificar existência do método. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:101`, `bin/selo_proporcao.py:117`) |
| Import de teste usado somente para leitura | **1**; o import basta. (`bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:117`) |
| Alterar só comentário em migration ou `signals.py` | **1**; a regra por caminho é incondicional para arquivo não deletado, embora o cabeçalho diga “NOVO ou RENOMEADO”. (`bin/selo_proporcao.py:10`, `bin/selo_proporcao.py:92`, `bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:96`) |

**Aceitos na direção conservadora para A**, conforme a orientação fornecida; o código declara expressamente esse custo. Portanto, esses falsos positivos não constituem bloqueante por si. (`bin/selo_proporcao.py:19`)

4. **Retornos**

- **Erros ordinários tratados dão 2:** argumentos insuficientes/classe inválida, revisão inválida, Git não zero, parse inválido e demais `Exception` dentro da fronteira de `main`. (`bin/selo_proporcao.py:34`, `bin/selo_proporcao.py:70`, `bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:124`)
- **Nem todo caminho de erro dá 2:** o wrapper faz somente `exec`; Python ausente pode devolver 127, execução recusada 126, e sinais não são normalizados. Ocorrência nesse ambiente: **[nao verificado]**. (`bin/selo_proporcao.sh:4`)
- **Argumentos excedentes não são rejeitados:** `base head B repo lixo` ignora `lixo`; com diff inócuo pode devolver 0. (`bin/selo_proporcao.py:70`, `bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:74`, `bin/selo_proporcao.py:119`)
- **Rc 1 não prova escrita real:** significa `achados && classe B`, incluindo os falsos positivos acima. (`bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:117`)
- **O defeito interno de rc 3 da v1 está fechado:** não há mais pipeline intermediário; `exec` preserva o retorno Python. Um chamador `bash -e` recebe 1 e encerra nessa chamada. O teste atual não confere esse cenário. (`bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:117`, `bin/tests/test_selo_proporcao.sh:62`)
- Encadear externamente com `| cat` ainda pode esconder o rc se o chamador não usar `pipefail`; esse uso hoje é **[nao verificado]**.

5. **Qualidade dos casos**

| Caso/achado v1 | Avaliação v2 |
|---|---|
| `signal_receiver` tinha dois gatilhos | **Corrigido nesse caso:** agora só `@receiver`. Porém `signal_nome` introduz outra duplicidade: módulo terminado em `signals` **ou** nome importado `post_save` bastam independentemente. (`bin/tests/test_selo_proporcao.sh:37`, `bin/tests/test_selo_proporcao.sh:38`, `bin/selo_proporcao.py:64`) |
| Fixture tinha duas exclusões | **Corrigido:** diretório e nome estão separados. (`bin/tests/test_selo_proporcao.sh:51`, `bin/tests/test_selo_proporcao.sh:52`) |
| Faltavam `bulk_update` e `get_or_create` | **Corrigido:** casos separados no laço dos oito métodos. (`bin/tests/test_selo_proporcao.sh:28`) |
| Command não provava tratamento próprio | **Permanece:** `command_alias` passa pela regra genérica de atributo; não prova mecanismo específico de command. (`bin/tests/test_selo_proporcao.sh:43`, `bin/selo_proporcao.py:55`) |
| Preparo podia julgar outro diff | **Melhorou, mas parcialmente:** checkout, resultado global, commit e HEAD são conferidos; scripts internos com `;` ainda podem mascarar falha anterior. O bloco especial de rc também não confere seu preparo. (`bin/tests/test_selo_proporcao.sh:20`, `bin/tests/test_selo_proporcao.sh:21`, `bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:61`) |
| `apagou_migration` | **Nome não corresponde ao preparo:** apenas cria `velho2.py`; não exercita exclusão de migration nem o ramo `D`. (`bin/tests/test_selo_proporcao.sh:54`, `bin/selo_proporcao.py:92`) |
| `bash -e` | **Não prova o anunciado:** descarta o rc dessa chamada; a comparação seguinte usa outro shell, sem `-e`. (`bin/tests/test_selo_proporcao.sh:62`, `bin/tests/test_selo_proporcao.sh:63`) |
| `prod_importa_teste` | Prova classificação conservadora de import; não cria módulo escritor nem demonstra a costura produção→teste→escrita. (`bin/tests/test_selo_proporcao.sh:47`) |
| `so_remocao` | Remove escrita e adiciona `pass`; não é diff exclusivamente de remoção. (`bin/tests/test_selo_proporcao.sh:49`) |

A afirmação do comentário de que “cada escape” virou caso excede a cobertura: faltam, entre outros, falso `+++`, parênteses envolvendo método e continuação adicionada sobre atributo antigo. (`bin/tests/test_selo_proporcao.sh:4`, `bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:32`, `bin/tests/test_selo_proporcao.sh:55`)

6. **QUEM ESCREVE, QUEM CHAMA e QUEM LÊ**

- **Quem produz o veredito:** Python imprime achados e devolve 0/1/2; o shell apenas o executa. (`bin/selo_proporcao.py:111`, `bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:129`, `bin/selo_proporcao.sh:4`)
- **Chamador concreto encontrado:** `test_selo_proporcao.sh`, via `SELO`; lê e compara rc nos casos e nos argumentos. A chamada com `bash -e` descarta o resultado. (`bin/tests/test_selo_proporcao.sh:8`, `bin/tests/test_selo_proporcao.sh:23`, `bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:56`, `bin/tests/test_selo_proporcao.sh:62`, `bin/tests/test_selo_proporcao.sh:63`)
- **Leitor indireto:** a régua executa o teste pelo glob e converte falha do teste em rc 1. Isso verifica um repositório sintético, não classifica o diff real do pouso. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:23`)
- **Pouso real:** o contrato fornecido exige chamada antes do merge, mas não identifiquei essa integração na cópia; chamada manual, leitor operacional do rc e registro no RELATO: **[nao verificado]**. (`contrato.txt:2`, `contrato.txt:3`, anexos) [nao verificado]
- **Pre-push:** chama testes específicos, e o próprio código registra que não executa o loop de host. Integração deste selo no hook efetivamente instalado: **[nao verificado]**. (`bin/pre-push.sh:14`, `bin/pre-push.sh:16`, `bin/pre-push.sh:19`, `bin/pre-push.sh:38`)
- **Atalho da régua da v1 permanece:** fingerprint só de `.py`/`.html` de `app/`; o “JA VERDE” retorna antes dos testes de host sem comparar `ARVORE_FP`. (`bin/regua.sh:33`, `bin/regua.sh:45`, `bin/regua.sh:86`, `bin/regua.sh:93`, `bin/regua.sh:138`)
- **Worktree:** o teste deste selo resolve seu caminho e passa repo sintético explicitamente; execução em worktree: **[nao verificado]**. A quebra independente da pasta inteira permanece: outro teste exige `.git/hooks/pre-push`, enquanto um vizinho já resolve o diretório comum. (`bin/tests/test_selo_proporcao.sh:7`, `bin/tests/test_selo_proporcao.sh:23`, `bin/tests/test_hook_nao_e_copia.sh:21`, `bin/tests/test_hook_nao_e_copia.sh:27`, `bin/tests/test_commit_so_o_declarado.sh:56`, `bin/tests/test_commit_so_o_declarado.sh:57`)

Este veredito trata do instrumento. O aval manda encaminhar achados de forma para fila, sem reabrir voltas das obras B abrangidas pela autorização. (`aval_proporcao_por_risco_1111.txt:1`, anexo, itens 2 e 5)

**REPROVADO — bloqueantes do instrumento: falsos verdes por continuação e código dinâmico (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:106`); exclusão de testes ainda escapável (`bin/selo_proporcao.py:98`); cobertura anunciada não demonstrada (`bin/tests/test_selo_proporcao.sh:54`, `bin/tests/test_selo_proporcao.sh:62`); normalização universal de erros incompleta (`bin/selo_proporcao.sh:4`); consumidor do pouso real [nao verificado].**

## 11/11 13:30 · sessao 23e5b22c9217 · LASTRO-FOLGA · censo · origem=Code

**O fecho separa dois casos: fora de `TIPO_CHOICES` é permitido; fora do catálogo vivo é rejeitado pela criação e pela edição.** A persistência comum, `Ausencia.save()`, não valida nenhum desses conjuntos. O dado fornecido não demonstra qual caminho gravou `folga`, nem se esse código estava cadastrado e ativo naquele momento. (`app/ponto/services/ausencia.py:193-195,474-475`; `app/ponto/models.py:868-908`; origem e cadastro históricos: [nao verificado])

**1. DELTA a0269430 → e70a54f7e**

Comparação direta com a cópia `../a026943099b8d88468240fd4b18491141b6810af`; identificação dos hashes pelos nomes dos diretórios, sem metadados Git: [nao verificado].

Abaixo estão somente alterações pertinentes aos caminhos do censo e novos leitores/escritores:

| Caminho | Delta | Citação atual |
|---|---|---|
| `editar_ausencia_registro` | Guarda o intervalo anterior e invalida o fechamento abrangendo os intervalos antigo e novo. A validação de tipo permanece pelo catálogo vivo. | `app/ponto/services/ausencia.py:474-475,497-524` |
| Materialização de disputa | Retirou minutos artificiais de dia inteiro e `checar_parcial=False`; continua criando pela porta validada. | `app/chamados/services/materializacao.py:474,490-499,526-535` |
| **Novo command** `rederivar_minutos_h06_comp11` | Confere identidade incluindo tipo e chama edição com `tipo=a.tipo`; portanto, passa pelo validador existente. Também lê tipo para snapshot. | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120-123,142-146,169-172` |
| `cobertura_ausencia_periodo` | Extraiu seleção e expansão para os **novos** `_q_cobertura` e `_cobertura_pura`; conserva o tratamento do desconhecido. | `app/ponto/turnos.py:2752-2783` |
| **Novo acesso em lote** `CargaDaJanela.cobertura` | Reutiliza seleção e expansão da cobertura. | `app/ponto/turnos.py:3068-3082` |
| **Novo carregador** `carga_pergunta_viva` | Carrega ausências para cobrança usando o mesmo filtro de cobertura; oferece essas linhas ao classificador. | `app/chamados/services/carga_pergunta_viva.py:79-108,149-150` |
| `classificar_falta` | Aceita carga previamente lida; mantém o veto à cobrança quando encontra ausência não parcial. | `app/escala/servico_jornada.py:280-293,347-353` |
| Fechamento financeiro | Substituiu “tem minutos” por `cobre_parte_do_dia`; removeu acumulador morto. Desconhecido continua suprimindo previsto. | `app/ponto/services/fechamento.py:672-673,688-701` |
| Coletor de efeitos do espelho/PDF | Nos dois ramos de parcialidade, passou a consultar `cobre_parte_do_dia`. | `app/ponto/services/efeito_ausencia.py:33-55` |
| Crédito parcial | Exige parcialidade confirmada pelo juiz, além de tipo pago. | `app/ponto/services/credito_parcial.py:20-28` |
| `dia_decidido.rotulo_de` | Deixa de apresentar minutos como abono parcial quando a ausência explica o dia inteiro. | `app/ponto/services/dia_decidido.py:487-497` |
| Lista de ausências | Acrescentou leitura de parcialidade e de fim aberto pelo tipo para apresentação. | `app/ponto/views.py:2233-2245` |
| Cartório, ramo sem adesão | Passou a considerar o julgamento de dia sem trabalho no horizonte declarado; preserva a precedência da cobertura integral de ausência. | `app/ponto/services/cartorio.py:267-285,1230-1280` |

Nenhum escritor/leitor citado foi encontrado como removido na comparação. Essa é uma constatação negativa da inspeção, sem relatório persistido: [nao verificado].

**2. Escritores: quem aceita fora de qual catálogo**

| Entrada/escritor | Validação existente e limite |
|---|---|
| Admin da aplicação | Recebe tipo do POST e chama `criar_ausencia`, que rejeita código fora do vivo. Aceita fora dos choices quando cadastrado. `app/ponto/views.py:2378,2415-2422`; `app/ponto/services/ausencia.py:193-195`. |
| PWA | Verifica o vivo diretamente e chama o produtor validado. `app/ponto/views.py:2509-2511,2536-2542`. |
| API | Traduz aliases legados de atestado/luto e chama o produtor validado; não traduz `folga`. `app/api/views.py:1765-1768,1817-1824`. |
| Edição HTTP | Delega à edição, que rejeita fora do vivo. `app/ponto/views.py:2794-2802`; `app/ponto/services/ausencia.py:474-475`. |
| Formulário | `clean_tipo` valida pelo vivo. Seu campo é `CharField`, não uma escolha congelada. A definição existe; sua utilização efetiva pelos endpoints não ficou demonstrada. `app/ponto/forms.py:41-68`; utilização: [nao verificado]. |
| Django admin | `AusenciaAdmin` usa `SoLeitura`, negando inclusão, alteração e exclusão. `app/ponto/admin.py:37-47`; `app/core/admin.py:59-75`. |
| Copiloto | Prévia consulta o vivo; escrita chama o produtor. `app/ponto/services/checar_ausencia.py:23-24,43-48`. |
| Veredito humano | Envia `falta` ou o tipo recebido ao produtor validado. `app/ponto/services/veredito_celula.py:331-340`. |
| Materialização | Envia tipo do mapa/argumento ao mesmo produtor. `app/chamados/services/materializacao.py:474,490-499,526-535`. |
| Regularização e abandono provisório | Enviam `falta` por `garantir_ausencia`, que delega à criação validada. `app/ponto/services/regularizacao.py:118-122`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76-84`; `app/ponto/services/ausencia.py:453-462`. |
| Espelhamento de férias | Envia literalmente `ferias` ao produtor validado. `app/ponto/services/ausencia.py:1202-1207`. |
| Command novo de minutos | Reenvia o tipo carregado à edição validada. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169-172`. |
| Command de rederivar fim | Seleciona registros pelo intervalo/tipo e chama `rederivar_fim`; este faz `save()` completo sem validar tipo. Pode regravar código desconhecido já carregado. `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-31,42-46`; `app/ponto/services/ausencia.py:1293-1298`. |
| Persistência `Ausencia.save()` | Valida coerência do intervalo e deriva outros campos, mas não consulta validade do tipo. É a brecha estrutural demonstrável para persistência direta. `app/ponto/models.py:868-908`. |

Não foi identificado importador próprio que introduza `Ausencia.tipo`; cobertura de execução externa, SQL manual ou importação não presente na cópia: [nao verificado]. O varredor do repositório contempla criação, alterações em lote e saves atribuíveis, mas declara limites de atribuição. (`app/core/censo_escritas.py:6-56,61-82,469-491`)

**Por que os choices não barraram:** a criação exclui deliberadamente `tipo` do `full_clean`, depois de validar pelo vivo. Há teste explícito exigindo que código cadastrado fora dos choices seja aceito. (`app/ponto/services/ausencia.py:361-387`; `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`)

**Como `folga` poderia passar pelas portas validadas:** o cadastro permite criar esse código, sem reserva nominal; se ativo, ele entra no catálogo vivo. Isso seria aceitação deliberada pelo cadastro, não ausência de validação. Existência desse cadastro na gravação fornecida: [nao verificado]. (`app/ponto/views_tipos_ausencia.py:61-94`; `app/ponto/services/ausencia.py:1301-1310`; `app/ponto/catalogo/ausencias.py:249-252,284-285`)

**3. Autoridade e listas existentes**

| Lista/fonte | Papel |
|---|---|
| `TipoAusencia` ativo → `vigente()` → `codigos_validos()` | Autoridade operacional de validade. Há cache por processo de 60 segundos; consulta vazia ou falha recai na semente. `app/ponto/catalogo/ausencias.py:240-285`. |
| `catalogo.ausencias.CATALOGO` | Semente/fallback com 17 códigos; não contém `folga`. `app/ponto/catalogo/ausencias.py:87-105`. |
| `Ausencia.TIPO_CHOICES` | Lista congelada com 17 códigos, ligada ao campo; não contém `folga`. `app/ponto/models.py:661-679,749`. |
| `forms.CATALOGO` | Outra lista, de regras de duração; ausência nela recebe máximo padrão de 30 dias. A validade continua sendo consultada no vivo. `app/ponto/forms.py:13-38,60-68`. |
| `_FAIXA_DE_TIPO` | Mapa separado de precedência; não é catálogo de validade. `app/ponto/precedencia.py:52-79`. |
| `TIPOS_PAGOS`, bloqueantes e abertos do modelo | Conjuntos derivados da autoridade, não listas independentes de validade. `app/ponto/models.py:680-689`. |
| Migration de seed e choices históricos | Listas históricas de instalação/esquema. `app/ponto/migrations/0052_seed_tipoausencia.py:9-37`; `app/ponto/migrations/0070_alter_ausencia_tipo.py:19-22`. |

Portanto, existe **uma autoridade operacional pretendida**, acompanhada de listas congeladas e mapas com outros papéis. Não existe uma única enumeração física. (`app/ponto/catalogo/ausencias.py:17-27,240-285`; `app/ponto/models.py:661-689`; `app/ponto/forms.py:13-38`)

**4. Decisores: comportamento de código ausente de `vigente()`**

| Decisor | Resultado para desconhecido |
|---|---|
| Catálogo | Efeito `None`; rótulo é o código. `app/ponto/catalogo/ausencias.py:293-300`. |
| `precedencia.faixa_de` | Retorna `atestado`, inclusive para `folga`. Só `folga_compensatoria` e `feriado_folga` têm faixa folga no mapa. `app/ponto/precedencia.py:52-79`. |
| Fatos do período | Inclui ausência não rejeitada, preserva código e aplica a precedência; pode vencer feriado, folga e trabalho. `app/ponto/precedencia.py:273-279,336-339,379-408`. |
| Cobertura | Folha exige aprovada; cobrança aceita não rejeitada. Intervalo fechado pode cobrir vários dias; desconhecido sem fim cobre somente a data inicial. `app/ponto/turnos.py:2511-2520,2554-2559,2770-2783`. |
| Parcialidade/integral | Não é parcial; sendo diferente de `falta`, explica o dia inteiro. Porém não entra no mapa integral **abonado**, que exige efeito `ABONA`. `app/ponto/turnos.py:2597-2602,2670-2692`. |
| Cartório | Copia a precedência para a célula; ausência aprovada integral dispensa lâmpadas e grava o código literal em `dispensada`. `app/ponto/services/cartorio.py:790-800,1984-1993`. |
| Motor/calculador | O desconhecido fica fora do mapa de integrais abonados que zera atraso/saída; o motor não valida novamente o catálogo nesse ramo. `app/ponto/turnos.py:2686-2692`; `app/ponto/motor_calculo_v2.py:1085-1105`; `app/ponto/calculador/regras.py:117-120`. |
| Fechamento financeiro | Não abona e **suprime previsto**, pois `None != DESCONTA`. `app/ponto/services/fechamento.py:693-701,729-732`. |
| Coletor do espelho/PDF | Não justifica nem suprime pelo efeito: exige `ABONA` ou `SUPRIME` explícitos. Conserva a divergência com o fechamento. `app/ponto/services/efeito_ausencia.py:53-65`. |
| Crédito parcial | Não recebe crédito: precisa pertencer aos tipos pagos e ser parcial. `app/ponto/services/credito_parcial.py:26-28`. |
| Export de faltas/8792 | Fica fora do filtro de tipos que descontam. `app/folha/export.py:87-91,138-140,179-181`. |
| Export de abonos/8932 | Fica fora do conjunto vivo de abonados. `app/folha/export.py:193-205`. |
| Veredito visual | Retorna `ABONADO`, salvo rejeitada; lançamento aberto aparece pendente. `app/ponto/services/dia_decidido.py:447-450,460-481`. |
| Status do dia | Faixa atestado com efeito `None` resulta em `ausencia`. `app/ponto/status_dia.py:296-319`. |

Esses resultados descrevem condições do código. Ata, competência, export e efeito financeiro efetivamente associados ao registro fornecido: [nao verificado].

**5. Arquivos da cura na origem e testes vizinhos** [nao verificado]

Para o recorte **impedir introdução ou troca para tipo fora do vivo pela persistência comum**, o pacote operacional mínimo fechado é:

- `app/ponto/models.py`: guarda no escritor `Ausencia.save`, consultando a autoridade existente; o tratamento deve distinguir introdução de tipo de preservação de histórico, já prevista pelo cadastro inativo. (`app/ponto/models.py:607-608,868-908`; `app/ponto/catalogo/ausencias.py:284-285`)
- `app/ponto/tests/test_selo_b5f1_porta_ausencia.py`: acrescentar casos de persistência direta inválida e preservar o caso concreto válido fora dos choices. (`app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`)

Esse pacote fecha a ausência de guarda demonstrada; atribuí-lo como cura da causa histórica de `folga` permanece [nao verificado]. Impor `TIPO_CHOICES` como validade contrariaria o contrato existente. (`app/ponto/services/ausencia.py:361-370`; `app/ponto/tests/test_cadastro_tipo.py:43-50`)

Os testes vizinhos que enumeram catálogo, choices, conjuntos ou diferenças entre eles são: [nao verificado]

| Arquivo | Enumeração/contrato |
|---|---|
| `app/ponto/tests/test_catalogo_ausencias.py` | Choices × semente, efeitos, partição e dropdown. `:10-20,36-42,56-60` |
| `app/ponto/tests/test_cadastro_tipo.py` | Seed, vivo, novo, inativo e fallback. `:14-34,43-50` |
| `app/ponto/tests/test_selo_b5f1_porta_ausencia.py` | Código concreto vivo fora dos choices. `:180-194` |
| `app/ponto/tests/test_aus_f4_criar_pelo_juiz.py` | Vivo menos choices e inválido; o laço pode ficar vazio. `:43-54` |
| `app/api/tests/test_api_ausencias_tipos.py` | Confere anúncio GET contra choices congelados. `:46-51` |
| `app/ponto/tests/test_fim_separado.py` | Semente e conjuntos de duração. `:34-42,55-60` |
| `app/ponto/tests/test_contract_rotulo_x_efeito.py` | Semente por efeito e tipos nomeados. `:34-37,69-75` |
| `app/ponto/tests/test_rotulo_x_efeito.py` | Rótulos por catálogo. `:45-64` |
| `app/ponto/tests/test_rotulo_do_dia_decidido.py` | Tipos da semente. `:84-92` |
| `app/ponto/tests/test_contract_espelho_sem_emoji.py` | Ícones de todos os tipos da semente. `:271-282` |
| `app/ponto/tests/test_ui3_rotulo_do_espelho.py` | Tipos da semente e desconhecido. `:25-30,46-64` |
| `app/ponto/tests/test_catalogo_vivo.py` | Tipos novos/inativos nos conjuntos. `:30-50` |
| `app/ponto/tests/test_conjuntos_vivos.py` | Conjuntos de efeito e exclusões do export. `:14-45` |
| `app/ponto/tests/test_aviso_home_office.py` | Presença do tipo nos choices e conjuntos. `:11-17` |
| `app/ponto/tests/test_o124_medico_sai_do_cadastro.py` | Tipo médico fora da semente e alterações do conjunto vivo. `:87-93,109-140` |

A correção na origem e a autoridade do cadastro já têm regra; dado gravado e cadastro seguem o destino separado da L-099. O dado disponível não sustenta criar regra nominal especial para `folga`. (`CLAUDE.md:17-27,32-44`; `app/docs/LEIS.md:122`; `app/ponto/models.py:562-569`; situação histórica de `folga`: [nao verificado])

perguntas de lei: 0

## 11/11 13:30 · sessao c168102e1e42 · RAIZ-DA-DISPUTA · censo · origem=Code

**O fecho mantém D8 aberta. O DELTA Git e a lista futura de edições não podem ser certificados nesta cópia.** (`app/chamados/services/fio.py:1275`; limitações da comparação e do perímetro futuro: [nao verificado].)

1. **DELTA entre `9ca563bb3` e `e70a54f7e`**

**[nao verificado].** A tentativa de comparação falhou porque a cópia não contém repositório Git; não há diff fornecido entre os commits. A documentação confirma que esse ambiente recebe uma árvore por `git archive`. Portanto, deslocamentos de citações não demonstram quais linhas mudaram ou sumiram entre os commits. (`CLAUDE.md:344`; ausência de `.git` e saída do comando: [nao verificado].) [nao verificado]

A lista solicitada de **somente mudanças/supressões** permanece não verificada; isso não significa delta vazio. **[nao verificado]**

2. **O aval responde à pergunta BATIDA sem episódio × BATIDA com episódio?**

**Não.** A frase “O par e LIDO dos marcos da celula do dia (saida e volta do mesmo intervalo)” decide a identidade da **pausa**, sem estabelecer equivalência entre BATIDAS com e sem episódio. **[nao verificado — frase e alcance do anexo recebido, sem arquivo local numerado.]**

O código continua declarando a questão aberta: a BATIDA do acervo “nao retrata linha de episodio nem e retratada por ela”; o ramo executável mantém essas linhas em `sem_episodio_intocada`. (`app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1275`, `app/chamados/services/fio.py:1294`.)

**Falta precisamente o critério que determine quando essas duas linhas representam o mesmo fato e permita ou vede a retratação entre elas.** O aval fornecido não contém esse critério. (`app/chamados/services/fio.py:1275`; conteúdo do aval: [nao verificado].)

3. **Já existe por AST a função “estas duas perguntas são o par da mesma pausa”?**

**Nenhuma encontrada no AST inspecionado.** A varredura foi executada em memória sobre `app` e `bin`, excluindo testes, migrations e diretórios gerados; execução sem artefato local numerado: **[nao verificado]**. [nao verificado]

Os sítios pertinentes existentes têm estes contratos:

| Sítio | O que efetivamente responde |
|---|---|
| `CelulaDia.PARES_PAUSA_DNA` | Declara `hii/hfi` e `hii2/hfi2`; não compara perguntas. (`app/escala/models.py:1652`.) |
| `EscalaColaborador.pausas_do_dia` | Enumera pausas completas do DNA; célula existente sem marcos retorna vazio. (`app/escala/models.py:1197`, `app/escala/models.py:1207`.) |
| `EscalaColaborador.intervalo_do_dia` | Retorna somente a primeira pausa. (`app/escala/models.py:1163`.) |
| `tarefa_do_dia._posicoes` / `instancias_do_turno` | Enumeram instâncias, inclusive `int_ida:2`, e expõem posição, hora, lâmpada e teto. (`app/chamados/services/tarefa_do_dia.py:58`, `app/chamados/services/tarefa_do_dia.py:92`.) |
| `fato_do_marco_no_espelho` | Para intervalo, exige acesas as lâmpadas da posição em **todas** as pausas cadastradas. (`app/chamados/juizes.py:957`, `app/chamados/juizes.py:971`.) |
| `_resposta_concorda_com_a_luz` | Retorna `None` quando há mais de uma pausa naquela posição, pois não identifica qual delas a pergunta cobra. (`app/chamados/juizes.py:1334`, `app/chamados/juizes.py:1336`.) |

A limitação está explícita: nenhum emissor carrega o índice da pausa, e o nascimento rejeita `instancia_marco` diferente da posição simples derivada do motivo. (`app/chamados/services/declaracao_texto.py:49`, `app/chamados/models.py:3441`.)

4. **Portas que fecham automaticamente hoje: autor e motivo**

Nos fechos normais abaixo, o chamador omite `motivo`; a porta grava **`veredito` quando existem perguntas ou `vazia` quando não existem**. Grava o autor recebido em `fechada_por`, a resolução e o evento `disputa_fechada`; não consulta o autor do último carimbo. (`app/chamados/models.py:1903`, `app/chamados/models.py:1933`, `app/chamados/models.py:1937`.)

| Porta | Autor passado hoje | Resolução gravada |
|---|---|---|
| Ausência declarada | `user` recebido. | `Ausencia declarada pelo colaborador; perguntas do dia resolvidas.` (`app/chamados/services/materializacao.py:591`.) |
| Cauda da materialização | `request_user`. | Primeira não vazia entre `resolucao_pedida`, `disputa.resolucao` e `Regularizacao granular: {n_mat} pergunta(s) materializada(s).` (`app/chamados/services/materializacao.py:1333`, `app/chamados/services/materializacao.py:1338`.) |
| Reconciliação sem turno aberto | Sistema, também autor do carimbo. | `Reconciliador: sem turno aberto no dia; perguntas resolvidas.` (`app/chamados/reconciliador.py:400`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:412`.) |
| Absolvição pela célula | Sistema, também autor dos carimbos. | `Celula soberana julgada absolveu o(s) dia(s); perguntas resolvidas pelo reconciliador.` (`app/chamados/reconciliador.py:587`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:612`.) |
| Órfãs pela lâmpada | Sistema, também autor de `encerrar_fantasma`. | `Perguntas orfas encerradas no evento da celula: a lampada do proprio marco acendeu (ORFAS-ENTERRA-FURO-REAL (b)).` (`app/chamados/reconciliador.py:1142`, `app/chamados/reconciliador.py:1147`, `app/chamados/reconciliador.py:1171`.) |
| Beco | Parâmetro `sistema`; a entrada do evento fornece sistema. | `Nenhuma pergunta viva; chamado-pai resolvido. Beco fechado pelo varredor (HX-BECO).` (`app/chamados/services/becos.py:18`, `app/chamados/services/becos.py:57`, `app/chamados/services/becos.py:78`.) |
| Emissão sem pergunta exigível | Sistema. | `Fechada automaticamente: nada a perguntar ao colaborador neste dia (o juiz da jornada nao aponta marco faltante). Se o fato voltar a acusar, o cartorio reabre pelo caminho normal.` (`app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1284`.) |
| Chamado resolvido / S128 | `user` recebido; o signal fornece sistema. | `Saida registrada; turno encerrado - resolvido automaticamente pelo sistema (S128).` (`app/chamados/services/disputa_emissao.py:1964`, `app/chamados/signals.py:115`.) |
| Remoção do último motivo | `ator` recebido. | `resolucao` recebida; os eventos de posto/âncora usam sistema e as resoluções `auto_revalidacao_posto_reativado` / `auto_revalidacao_ancora_calibrada`. (`app/chamados/models.py:2072`, `app/colaboradores/signals.py:77`, `app/escala/signals.py:161`.) |
| Entrada sincronizada | Sistema, embora a função receba `autor_user`. | `Entrada registrada em {hora} (sync); orfao reconciliado, disputa dissolvida automaticamente.` (`app/ponto/services/entrada_sincronizada.py:19`, `app/ponto/services/entrada_sincronizada.py:59`.) |
| Foto recebida | `user`, também passado à validação. | `Auto-fechada: foto recebida no upload separado do APK.` (`app/api/foto_recebida.py:79`, `app/api/foto_recebida.py:80`.) |
| Comando `encerrar_perguntas_com_fato`, com `--apply` | Usuário indicado por `--por`. | `BUG 117: todas as perguntas tinham o fato no espelho. Encerrada pela porta, com trilha.` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:53`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:63`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`.) |

A cauda de materialização também atende à validação e ao reparador do passivo; o comando de becos usa a mesma porta de beco. (`app/chamados/services/validacao.py:469`, `app/chamados/services/passivo_validado.py:405`, `app/chamados/management/commands/fechar_becos_disputa.py:57`.)

As exceções automáticas nomeadas são:

| Porta | Autor / motivo / resolução |
|---|---|
| Competência trancada | `autor` recebido ou sistema; motivo `competencia_trancada`; resolução identifica competência e empresa. Esse caminho não carimba perguntas. (`app/chamados/services/competencia_trancada.py:175`, `app/chamados/services/competencia_trancada.py:178`, `app/chamados/services/competencia_trancada.py:204`.) |
| Desligamento | `_ator` recebido ou sistema; motivo `desligamento`; resolução `Colab desligado em {data_demissao} - fechamento automatico.` (`app/colaboradores/services/desligamento.py:131`, `app/colaboradores/services/desligamento.py:142`.) |

`admin_encerra` é a porta administrativa explícita, com motivo textual obrigatório, não uma cauda automática. (`app/chamados/models.py:1943`, `app/chamados/models.py:1950`.)

**Restrição concreta para F3:** `materializada_por` não substitui necessariamente o autor do ato atual. A retratação pode preservar o carimbo antigo com `manter_carimbo=True`; o beco, por sua vez, agenda somente `disputa_id` e escolhe sistema depois. (`app/chamados/models.py:2061`, `app/chamados/models.py:3182`, `app/chamados/signals.py:231`, `app/chamados/services/becos.py:78`.)

5. **Arquivos separados para conferência de sobreposição**

**Lista futura FECHADA: [nao verificado].** O código permite identificar os sítios envolvidos, mas não comprova quais serão editados: isso depende de onde forem implementados o transporte do autor, a reação da raiz e a identidade da instância. A relação abaixo é um **perímetro conservador proposto para reserva**, não um diff certificado. **[nao verificado quanto às edições futuras.]**

**F3 — produção:**

- `app/chamados/models.py:1855` — fechamento, trilha e portas de carimbo (`:3103`, `:3168`).
- `app/chamados/signals.py:228` — reação da pergunta sem transporte de autor.
- `app/chamados/services/becos.py:57` — cauda do beco.
- `app/chamados/services/materializacao.py:591` — fecho por ausência e cauda geral (`:1338`).
- `app/chamados/reconciliador.py:412` — três caudas (`:612`, `:1171`).
- `app/chamados/services/disputa_emissao.py:1284` — emissão muda e S128 (`:1964`).
- `app/api/foto_recebida.py:80` — fecho posterior à validação.
- `app/ponto/services/entrada_sincronizada.py:59` — fecho com sistema.
- `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` — cauda do comando.
- `app/chamados/services/explicador.py:137` — resposta parcial concorrente com C7.
- `app/chamados/relatorios_disputa.py:20` — `PRONTA_FECHAR` derivado apenas de ausência de vivas.

**F3 — testes existentes propostos:** `app/chamados/tests/test_c7_red_minimos.py:147`; `app/chamados/tests/test_varredura_becos_evento_pergunta.py:47`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:43`. Edição futura: **[nao verificado]**.

**F4 — produção:**

- `app/chamados/models.py:3441` — identidade persistida e consultas por instância (`:2753`).
- `app/chamados/services/declaracao_texto.py:49` — perda do índice da pausa.
- `app/chamados/services/disputa_emissao.py:823` — emissão das perguntas dos marcos.
- `app/chamados/services/fabrica_da_celula.py:145` — deduplicação e emissão por motivo/dia.
- `app/chamados/services/fio.py:1460` — identidade MARCO inclui instância.
- `app/chamados/services/materializacao.py:1163` — unidade individual e lote (`:1381`).
- `app/chamados/services/validacao.py:461` — aplicação individual anterior ao lote.
- `app/chamados/services/passivo_validado.py:516` — outro consumidor da unidade individual.
- `app/chamados/juizes.py:957` — leitores existentes de lâmpada/concordância da pausa (`:1334`).

**F4 — testes existentes propostos:** `app/chamados/tests/test_e1_v5_indice.py:121`; `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:174`; `app/chamados/tests/test_validar_materializa_wiring.py:51`. Edição futura: **[nao verificado]**.

A fonte dos pares já existe em `app/escala/models.py:1652`, e o enumerador de instâncias já existe em `app/chamados/services/tarefa_do_dia.py:58`; serem dependências não demonstra necessidade de edição. Documentação, migrations e novos arquivos do futuro diff: **[nao verificado]**.

A interseção dos perímetros propostos é **`app/chamados/models.py`, `app/chamados/services/materializacao.py` e `app/chamados/services/disputa_emissao.py`**; portanto, essa reserva não permite tratar F3/F4 como obras disjuntas. (`app/chamados/models.py:3103`, `app/chamados/services/materializacao.py:1163`, `app/chamados/services/disputa_emissao.py:823`; previsão de edição: [nao verificado].)

6. **Fecho de lei:** permanece somente D8, explicitamente aberta no plano de retratação; a identidade da pausa foi decidida pelo aval recebido. (`app/chamados/services/fio.py:1275`; aval sem arquivo local numerado: [nao verificado].)

perguntas de lei: 1

## 11/11 13:30 · sessao d0eecec3bfe2 · - · - · origem=Ronald (tmux)

Escapes deduzidos, sem executar:

1. **Continuação que transforma referência em chamada: rc 0.** O `Attribute` continua na linha antiga; o `Call` que cruza a nova linha não é candidato. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:66`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,2 +1,3 @@
 (obj.save
+()
 )
```

2. **`exec` e `eval`: rc 0.** A string completa não é exatamente `"save"` nem SQL; nomes `exec`/`eval` não pertencem a `SIGNAL`. (`bin/selo_proporcao.py:57`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+exec("obj.save()")
```

```diff
+eval("obj.save()")
```

3. **F-string com nome calculado: rc 0.** Os fragmentos constantes são `"sa"` e `"ve"`, nenhum deles reconhecido. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+getattr(obj, f"{'sa'}ve")()
```

4. **`setattr` com alias novo: rc 0.** Nem `"acao"` nem os fragmentos `"sa"`/`"ve"` são reconhecidos; a chamada final usa atributo `acao`. (`bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+setattr(obj, "acao", getattr(obj, "sa" + "ve"))
+obj.acao()
```

5. **Cursor por alias com SQL fragmentado: rc 0.** O detector não acompanha o alias nem monta `BinOp`; nenhum fragmento isolado casa com SQL. Alias sozinho com `"DELETE FROM x"` daria rc 1. (`bin/selo_proporcao.py:31`, `bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+executar = cursor.execute
+executar("DELETE " + "FROM x")
```

6. **Exclusão de testes continua aberta: rc 0.** Se produção já importa `gravar` de `.test_writer`, basta mudar somente o helper excluído: o import antigo não será examinado. Isso introduz escrita nova; não é helper já escritor. (`bin/selo_proporcao.py:18`, `bin/selo_proporcao.py:42`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/test_writer.py
+++ b/app/x/test_writer.py
@@ -1,2 +1,2 @@
 def gravar(obj):
-    pass
+    obj.save()
```

**Import dinâmico novo também não fecha essa exclusão:** o detector só reconhece `ast.Import`/`ast.ImportFrom`; a string do módulo não é nome de escrita. (`bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:98`)

```diff
+__import__("app.x.test_writer", fromlist=["gravar"]).gravar(obj)
```

7. **`import *` por reexportação neutra: rc 0**, sob preparação explícita: `app/x/__init__.py` já contém `from .tests.fixt import gravar`; `fixt.py` antes tinha `pass`. A alteração do teste é excluída e `from app.x import *` não contém módulo de teste; `gravar` não é `SIGNAL`. (`bin/selo_proporcao.py:57`, `bin/selo_proporcao.py:63`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- a/app/x/tests/fixt.py
+++ b/app/x/tests/fixt.py
@@ -1 +1 @@
-def gravar(obj): pass
+def gravar(obj): obj.save()
--- /dev/null
+++ b/app/x/use.py
@@ -0,0 +1,2 @@
+from app.x import *
+gravar(obj)
```

8. **Decorador ativa escrita antiga: rc 0.** A adição cria somente `Lambda`/`Call`/nomes não reconhecidos; o `save` do corpo permanece em linha antiga. Pode ser enquadrado no limite de helper preexistente, embora a ativação no carregamento seja nova. (`bin/selo_proporcao.py:19`, `bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:119`)

```diff
+@(lambda f: f())
 def gravar():
     obj.save()
```

Para decorador sem helper escritor preexistente, mas compartilhando o mecanismo de nome calculado: (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:119`)

```diff
+@(lambda f: getattr(obj, "sa" + "ve")())
+def f(): pass
```

9. **Arquivo `.pyw` ou `.sql` carregado por novo `.py`: rc 0.** As extensões são excluídas; `exec`, `open`, `read` e `execute` não são nomes reconhecidos. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:119`)

```diff
--- /dev/null
+++ b/app/x/run.py
@@ -0,0 +1 @@
+exec(open(__file__ + "w").read())
--- /dev/null
+++ b/app/x/run.pyw
@@ -0,0 +1 @@
+obj.save()
```

```diff
--- /dev/null
+++ b/app/x/run.py
@@ -0,0 +1 @@
+cursor.execute(open(__file__ + ".sql").read())
--- /dev/null
+++ b/app/x/run.py.sql
@@ -0,0 +1 @@
+DELETE FROM x;
```

10. **Template:** `.html` é ignorado e o backend configurado é Django. `{{ obj.gravar }}` pode chamar método customizado existente que escreve; contexto, existência e execução reais são **[nao verificado]** e esse caso pode cair no limite de helper aceito. Não usar `{{ obj.save }}`/`{{ obj.delete }}` como prova automática: `alters_data` pode impedir a execução. (`bin/selo_proporcao.py:19`, `bin/selo_proporcao.py:98`, `app/config/settings/base.py:72`; comportamento concreto de `alters_data`: [nao verificado])

Precisão terminológica: na continuação há nós AST (`Call`/`Expr`) cruzando a linha adicionada; o escape é que **nenhum nó reconhecido por `nome_de_escrita`** cruza essa linha. (`bin/selo_proporcao.py:54`, `bin/selo_proporcao.py:104`, `bin/selo_proporcao.py:106`)

## 11/11 13:30 · sessao 37302d31d8ea · - · - · origem=Ronald (tmux)

Censo de chamadores por AST dos `.py` de `app/`, excluídos testes e migrations; as referências abaixo são os sítios reais de chamada. [nao verificado]

**Portas e alcance**

- O cartório chama diretamente `ChamadoColaborador.reconciliar()` em seis sítios: adiados depois da lavra/E2 (`app/ponto/services/cartorio.py:998`); marcos acesos ou dia sem trabalho (`app/ponto/services/cartorio.py:1525`); marcos inexistentes (`app/ponto/services/cartorio.py:1538`); protesto CSF ou cobrança em dia sem trabalho (`app/ponto/services/cartorio.py:1559`); FIO-DUP (`app/ponto/services/cartorio.py:1637`); FIO-DUP-C8 (`app/ponto/services/cartorio.py:1652`).
- Os quatro primeiros não consultam o predicado de conversa do supra; os dois últimos usam somente a guarda local de texto `any((q.resposta_colab or '').strip()...)`, universo `perguntas_que_resolvem`, definida em `app/ponto/services/cartorio.py:1623`, aplicada em `app/ponto/services/cartorio.py:1634` e `app/ponto/services/cartorio.py:1651`.
- A porta comum entre evento e supra é `reconciliar`, não `retratar`: o cartório chama a primeira diretamente; o supra chama `retratar`, que delega a `_retratar`, que chama `reconciliar`. (`app/ponto/services/cartorio.py:1559`; `app/ponto/management/commands/supra_juiz.py:286`; `app/chamados/reconciliador.py:544`; `app/chamados/reconciliador.py:523`.)
- `reconciliar` consulta `_decisao_do_fechamento` e escreve via `_gravar_estado`; as guardas atuais são estado permitido, premissa regularizada/dedup e pergunta viva, não o predicado histórico de conversa do supra. (`app/chamados/models.py:641`; `app/chamados/models.py:663`; `app/chamados/models.py:682`; `app/chamados/models.py:685`; `app/chamados/models.py:654`.)
- `_decisao_do_fechamento` tem exatamente dois chamadores internos: `reconciliar(registrar=True)` e `reconciliaria(registrar=False)`. (`app/chamados/models.py:651`; `app/chamados/models.py:661`.)
- Os leitores externos de `reconciliaria` são o balde do lastro, o vigia `reconciliar_chamados` e o dry/alarme de `detectar_intervalo_ausente`. Uma guarda dentro de `_decisao_do_fechamento` muda também essas leituras. (`app/ponto/services/lastro.py:318`; `app/chamados/management/commands/reconciliar_chamados.py:79`; `app/ponto/management/commands/detectar_intervalo_ausente.py:138`.)

**Todos os chamadores diretos adicionais de `reconciliar`**

| Arquivo | Função | Linha(s) |
|---|---|---|
| `app/chamados/signals.py` | `disputa_fechada_fecha_chamado_pai` | 42 |
| `app/chamados/reconciliador.py` | `_retratar` | 523 |
| `app/chamados/services/adesao.py` | `sincronizar_adesao` | 115, 150 |
| `app/chamados/services/feriado_reavaliar.py` | `encerrar` | 47 |
| `app/chamados/services/limbo_folgas.py` | `aceitar_sugestao` | 101 |
| `app/chamados/services/limbo_folgas.py` | `agrupar_furo` | 283, 286 |
| `app/chamados/services/silencio_ausencia.py` | `reconciliar_por_ausencia` | 101 |
| `app/chamados/services/materializacao.py` | `_materializar_ausencia_de_resposta` | 587 |
| `app/chamados/services/materializacao.py` | `_reconciliar_destino_e_clones` / `_fecha` | 776 |
| `app/chamados/management/commands/auditar_chamados_orfaos.py` | `handle` | 67 |
| `app/colaboradores/services/pos_vinculo.py` | `fechar_adesao_com_batida` | 42 |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py` | `_marcar` | 140 |
| `app/ponto/services/triagem_batida.py` | `_autoresolve_f2_f4` | 597 |
| `app/ponto/services/veredito_celula.py` | `fechar_o_fio_do_dia` | 202 |
| `app/ponto/services/ausencia.py` | `fechar_chamado_da_ausencia` | 694 |
| `app/ponto/services/cluster_espurio.py` | `reconciliar_colab` | 189 |
| `app/ponto/services/entrada_sincronizada.py` | `resolver_cobranca_da_entrada` | 31 |
| `app/ponto/services/cobertura.py` | `designar_cobertura` | 122 |
| `app/ponto/services/cobertura.py` | `declarar_cobertura_passada` | 317 |
| `app/ponto/services/lastro.py` | `fechar_pelo_marco_colab` | 400 |
| `app/ponto/management/commands/detectar_intervalo_ausente.py` | `handle` | 138 |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `_ata_sem_furo` | 151 |
| `app/ponto/management/commands/detectar_ausencias.py` | `handle` | 158 |
| `app/ponto/management/commands/detectar_ausencias.py` | `_fechar_por_afastamento` | 824 |
| `app/ponto/management/commands/detectar_ausencias.py` | `_fechar_por_cobertura` | 834 |
| `app/ponto/management/commands/detectar_ausencias.py` | `_fechar_chamado` | 845 |

**Se a guarda entrar em `retratar` ou `_retratar`**

`retratar` tem estes oito chamadores externos; entrar somente nesse wrapper público não alcança o cartório nem os dois chamadores internos diretos de `_retratar`. (`app/chamados/reconciliador.py:544`; `app/ponto/services/cartorio.py:1559`; `app/chamados/reconciliador.py:1004`; `app/chamados/reconciliador.py:1464`.)

| Arquivo | Função | Linha |
|---|---|---|
| `app/ponto/services/justificativa.py` | `encerrar_chamado_de_origem` | 46 |
| `app/ponto/services/geofence.py` | `reavaliar_chamados_geofence` | 441 |
| `app/ponto/management/commands/supra_juiz.py` | `handle` | 286 |
| `app/ponto/management/commands/reconciliar_grade.py` | `handle` | 309 |
| `app/ponto/management/commands/reconciliar_fantasmas.py` | `handle` | 280 |
| `app/ponto/management/commands/reconciliar_vinculo.py` | `handle` | 68 |
| `app/ponto/management/commands/processar_alertas_turno.py` | `_resolver_chamado_do_turno` | 216 |
| `app/chamados/management/commands/retratar_chamados_lei_cala.py` | `handle` | 59 |

Dentro de `_retratar`, alcançaria também `retratar_chamados_absolvidos_por_celula` e `retratar_premissa_morta_colab`, além desses oito caminhos. (`app/chamados/reconciliador.py:1004`; `app/chamados/reconciliador.py:1464`.)

**Se entrar no escritor mais baixo**

`_gravar_estado` é o escritor de `status_local`, `resolvido_em`, `via_encerramento` e urgência; o evento de auditoria conserva status anterior, status novo e via, sem indicar “cartório/CSF”. (`app/chamados/models.py:201`; `app/chamados/models.py:228`; `app/chamados/models.py:242`; `app/chamados/models.py:292`.)

Se guardado sem recorte, alcança sete entradas distintas, incluindo transições que nem são encerramento:

| Chamador interno | Referência |
|---|---|
| `_aplicar` — transição humana | `app/chamados/models.py:450` |
| `reconciliar` | `app/chamados/models.py:654` |
| `cancelar` | `app/chamados/models.py:788` |
| `analisar` | `app/chamados/models.py:806` |
| `reabrir` | `app/chamados/models.py:1442` |
| `encerrar` | `app/chamados/models.py:1619` |
| `encerrar_por_competencia_trancada` | `app/chamados/models.py:1639` |

`encerrar` é outra porta: usa evento `ENCERRAR`, vai a `fechado`, consulta pergunta viva e não usa `_decisao_do_fechamento`; o CSF do cartório e do supra não a chama diretamente. (`app/chamados/models.py:1605`; `app/chamados/models.py:1614`; `app/chamados/models.py:1617`; `app/ponto/services/cartorio.py:1559`; `app/chamados/reconciliador.py:523`.)

**Caminhos indiretos do cartório**

- E2 no cartório carimba perguntas; depois fecha disputas; o signal da disputa fecha o chamado-pai por `reconciliar`. Nenhum desses pontos consulta `pode_executar_csf`. (`app/ponto/services/cartorio.py:993`; `app/chamados/reconciliador.py:595`; `app/chamados/reconciliador.py:611`; `app/chamados/reconciliador.py:612`; `app/chamados/signals.py:42`.)
- O cartório chama também o reconciliador de cluster, que fecha chamados por outro motivo usando a mesma porta. (`app/ponto/services/cartorio.py:1028`; `app/ponto/services/cartorio.py:1029`; `app/ponto/services/cluster_espurio.py:189`.)
- Quando um chamado deixa de ser vivo, o signal `_filho_morto_acorda_o_pai` pode registrar `on_commit` e chamar `reconciliar_chamados_colab`; este executa E2, órfãs e premissa morta. Não existe chamada explícita final dessa função dentro de `julgar_colab`. (`app/chamados/signals.py:350`; `app/chamados/signals.py:368`; `app/chamados/signals.py:369`; `app/chamados/reconciliador.py:1432`; `app/chamados/reconciliador.py:1433`; `app/chamados/reconciliador.py:1441`; `app/ponto/services/cartorio.py:1085`.)
- A lavra gera `EventoCelula`; o consumidor pode fechar por absolvição da célula, premissa morta e lastro. Esse caminho também passa pela porta geral sem a guarda de conversa do supra. (`app/ponto/portas/celula.py:796`; `app/ponto/portas/celula.py:896`; `app/chamados/reconciliador.py:1368`; `app/chamados/reconciliador.py:1372`; `app/chamados/reconciliador.py:1383`; `app/ponto/services/lastro.py:400`.)

Assim, uma guarda global na porta geral muda os chamadores da tabela; uma guarda nos chamadores cartório/supra deixa esses outros usos com o comportamento atual, mas precisa cobrir os vários sítios do cartório e explicitar se o consumidor pós-lavra integra o alcance pretendido. (`app/chamados/models.py:641`; `app/ponto/services/cartorio.py:998`; `app/ponto/services/cartorio.py:1525`; `app/ponto/services/cartorio.py:1559`; `app/chamados/reconciliador.py:1368`.)

**Testes e selos relevantes** [nao verificado]

| Cobertura | Referência |
|---|---|
| Cartório: CSF aparece na sombra, chamado fecha na aplicação, fio não ressuscita | `app/ponto/tests/test_o261_cobrado_le_o_fio.py:393`; asserts em `:412`, `:416`, `:420` |
| Evento real `registrar_batida` → cartório → E2 → chamado encerrado, sem estado terminal com pergunta viva | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:358`; asserts em `:390`, `:392`, `:395` |
| Guarda geral recusa pergunta viva por destino/container | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`; `:94` |
| Guarda geral permite reata ao sobrevivente e mantém dry sem escrita | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:135`; `:143`; `:152` |
| Cobrança de marco satisfeito fecha mesmo com outro furo no dia, atualmente via consumidor | `app/ponto/tests/test_bug115_lampada_do_marco.py:185`; chamada consumidor `:192`; assert `:205` |
| Marco ainda apagado mantém cobrança viva | `app/ponto/tests/test_bug115_lampada_do_marco.py:207`; assert `:221` |
| Universo do cartório: módulo de furo, vocabulário do supra, data declarada | `app/ponto/tests/test_cartorio_so_chamado_de_batida.py:53`; `:63`; `:74` |
| Resposta escrita não ressuscita fio — não testa preservação CSF | `app/ponto/tests/test_o261_cobrado_le_o_fio.py:341` |
| Selo custo real da batida com um/três chamados encerrando | `app/ponto/tests/test_selo_chokepoint_escrita.py:387`; `:392` |
| Selo custo real da batida com um/três chamados permanecendo | `app/ponto/tests/test_selo_chokepoint_escrita.py:422`; `:427` |
| Selo custo constante da guarda no caminho real | `app/ponto/tests/test_selo_chokepoint_escrita.py:432` |
| Selo teto de `on_commit` da batida | `app/ponto/tests/test_selo_chokepoint_escrita.py:201`; cobrança `:242` |

Nenhum resultado de execução desses testes foi obtido neste censo; apenas o conteúdo foi lido. [nao verificado] [nao verificado]

## 11/11 13:29 · sessao c4906477109b · - · - · origem=Ronald (tmux)

Resultados confirmados por execução em memória com `python3 -B`, sem escrever arquivos; o rc referido abaixo é o equivalente ao retorno de `problemas()` (`bin/mapa20_valida.py:221`, `bin/mapa20_valida.py:225`).

| Entrada | Juiz e operação | Resultado | Prova existente |
|---|---|---|---|
| Tabela válida | Forma/cabeçalho/universo, células e X/N (`bin/mapa20_valida.py:135`, `bin/mapa20_valida.py:154`, `bin/mapa20_valida.py:166`, `bin/mapa20_valida.py:202`) | Fixture verde: rc 0 | `bin/tests/test_mapa20_valida.sh:42`; completude/vazias: `bin/tests/test_mapa20_valida.sh:45` |
| `encerrada` com X<N em célula de obra | `_afirma_encerrada()` e guarda de completude (`bin/mapa20_valida.py:68`, `bin/mapa20_valida.py:200`) | rc 1 | `bin/tests/test_mapa20_valida.sh:55`; AUSENTE encerrada: `bin/tests/test_mapa20_valida.sh:56` |
| `encerrada` fora da tabela: negrito, ATX, setext, lista | SEM juiz de estado para a prosa: iteração restringe-se ao bloco da tabela (`bin/mapa20_valida.py:147`, `bin/mapa20_valida.py:166`, `bin/mapa20_valida.py:200`) | Quatro contracasos em memória: rc 0 | SEM PROVA de regressão na cópia; os casos de segunda tabela tratam pipes, não prosa (`bin/tests/test_mapa20_valida.sh:62`, `bin/tests/test_mapa20_valida.sh:63`) |
| Invisível Cf | Recusa categoria Cf no texto inteiro (`bin/mapa20_valida.py:132`) | U+200B: rc 1 | `bin/tests/test_mapa20_valida.sh:71` |
| Combinantes Mn/homóglifo | SEM juiz dessas classes: só Cf é recusado; estado usa regex literal (`bin/mapa20_valida.py:58`, `bin/mapa20_valida.py:132`) | `enc`+U+034F+`errada`, U+FE00 equivalente, `еncerrada` cirílico: rc 0 com 1 de 2 | SEM PROVA de regressão na cópia (`bin/tests/test_mapa20_valida.sh:67`, `bin/tests/test_mapa20_valida.sh:71`) |
| FF, FS, U+2028 unindo E3/E4B | `splitlines()` fabrica linhas lógicas; SEM juiz da divergência com linha física (`bin/mapa20_valida.py:125`) | Três contracasos em memória: rc 0 | SEM PROVA de regressão na cópia; o teste de foto verifica mesma variável, não separadores (`bin/tests/test_mapa20_valida.sh:206`) |
| N com fonte fora de aval/TICKETS/BACKLOG | `_fonte_ruim()` só exige citação existente, contida na raiz e linha não vazia (`bin/mapa20_valida.py:73`, `bin/mapa20_valida.py:78`, `bin/mapa20_valida.py:88`) | `bin/relato.sh:2`: rc 0 | SEM PROVA de elegibilidade; teste verde legitima a fonte errada em `bin/tests/test_mapa20_valida.sh:27`, `bin/tests/test_mapa20_valida.sh:42`; testes negativos só existência/raiz em `bin/tests/test_mapa20_valida.sh:107` |
| SEM DONO | Exige `n/a (SEM DONO)` e sai antes da guarda de encerrada (`bin/mapa20_valida.py:182`, `bin/mapa20_valida.py:185`, `bin/mapa20_valida.py:200`) | SEM DONO com `encerrada`: rc 0 | Ausência/vazia: `bin/tests/test_mapa20_valida.sh:45`; X/N indevido: `bin/tests/test_mapa20_valida.sh:72`; estado encerrada: SEM PROVA |
| RASCUNHO | SEM juiz específico; prosa sem pipes pode ficar fora da tabela (`bin/mapa20_valida.py:137`, `bin/mapa20_valida.py:144`, `bin/mapa20_valida.py:166`) | Prefixar `RASCUNHO` a mapa válido: rc 0 | SEM PROVA; mapa vivo RASCUNHO do anexo: [nao verificado] na cópia |
| Legenda literal `montar_mapa.py:103` | Arquivo ausente na cópia; literal não fornecido integralmente | [nao verificado] | SEM PROVA; não inventar a legenda a partir do comentário do juiz (`bin/mapa20_valida.py:128`) |

Detalhes adicionais relevantes:

- Não há conferência semântica de X contra prova no RELATO: a implementação calcula completude comparando os dois inteiros e confere apenas a fonte de N (`bin/mapa20_valida.py:190`, `bin/mapa20_valida.py:192`).
- A fixture pública usa `a.md:1` com conteúdo literal `fonte`; portanto ela prova o circuito de publicação, não a lei de origem/contagem de N (`bin/tests/test_mapa20_valida.sh:133`, `bin/tests/test_mapa20_valida.sh:148`).
- `status/encerrada.sim` é removido como caminho e passa com X<N (`bin/mapa20_valida.py:52`, `bin/mapa20_valida.py:65`). O caso local existente é `logs/encerrada.md:10`, em `bin/tests/test_mapa20_valida.sh:102`; o `test:414` citado no opus não corresponde à numeração desta cópia [nao verificado].
- Numeração local a usar: `splitlines()` em `bin/mapa20_valida.py:125`; Cf em `bin/mapa20_valida.py:132`; fonte em `bin/mapa20_valida.py:73`; SEM DONO `continue` em `bin/mapa20_valida.py:185`; guarda encerrada em `bin/mapa20_valida.py:200`.

## 11/11 13:29 · sessao 3e14c3367581 · - · - · origem=Ronald (tmux)

Escritores comprováveis:

| Origem | Evidência |
|---|---|
| `montar_mapa.py` | Atribuído como escritor pelo próprio validador; implementação e linha de gravação ausentes da cópia: [nao verificado]. (`bin/mapa20_valida.py:10`, `bin/mapa20_valida.py:128`) |
| Edição pela sessão `PRONTO-AUSENTE-RASCUNHO` | Informada no item 5 do anexo `opus_final_ffa_achados.txt`; comando, autoria individual e linha da gravação: [nao verificado]. |
| Outro escritor de produção | [nao verificado]. |
| Fixture de regravação | Grava `REGRAVADO` no mapa do repositório temporário de teste. (`bin/tests/test_mapa20_valida.sh:146`, `bin/tests/test_mapa20_valida.sh:154`) |
| Fixtures verde/vermelha | Copiam ou escrevem mapa no repositório temporário de teste. (`bin/tests/test_mapa20_valida.sh:177`, `bin/tests/test_mapa20_valida.sh:178`) |

O publicador lê o mapa vivo e faz uma foto; essa operação não escreve no mapa vivo. (`bin/relato.sh:97`)

A premissa de escritor único é contradita pelo dado fornecido, que relata montador e edição externa; confirmação independente na cópia: [nao verificado]. O comentário que fundamenta a forma rígida continua afirmando exclusividade do montador. (`bin/mapa20_valida.py:10`)

Nos dois avais fornecidos, há obrigação de regravar por evento e de julgar na publicação, mas nenhuma escolha literal de escritor exclusivo; concluir que a lei escolheu o montador seria [nao verificado]. A pergunta de lei é: **quem fica como único escritor autorizado do mapa?** A escolha não deve ser feita pelo censo. (`bin/relato.sh:90`, `bin/mapa20_valida.py:10`; avais fornecidos, item 3 do primeiro e item 4 do complemento.)

A legenda de `montar_mapa.py:103` é mencionada no anexo, mas seu literal não foi fornecido e o arquivo está ausente; conteúdo, aceitação e teste específico: [nao verificado]. [nao verificado]

Ordem de dependência proposta, sem implementar:

1. Escolher o escritor autorizado e sua forma de saída; o juiz atual fundamenta a forma nessa escolha. (`bin/mapa20_valida.py:10`)
2. Ancorar o juiz no literal dos avais, cobrindo conteúdo visível fora da tabela, caracteres, separadores, fonte de N e estados especiais; hoje o conteúdo julgado restringe-se ao bloco da tabela e `SEM DONO` sai antes da checagem de encerramento. (`bin/mapa20_valida.py:125`, `bin/mapa20_valida.py:132`, `bin/mapa20_valida.py:166`, `bin/mapa20_valida.py:182`, `bin/mapa20_valida.py:200`)
3. Ancorar a publicação no artefato final julgado; hoje a raspagem ocorre depois da validação. (`bin/relato.sh:97`, `bin/relato.sh:127`)
4. Ancorar o chamador num resultado distinguível; o serviço apenas declara `ExecStart` e o publicador usa rc 1 tanto para retenção quanto para falhas. (`bin/hasner-relato.service:15`, `bin/relato.sh:100`, `bin/relato.sh:110`)
5. Fazer os testes morderem essas transições; os atuais testes de publicação usam raspador substituto. (`bin/tests/test_mapa20_valida.sh:158`, `bin/tests/test_mapa20_valida.sh:162`)

Lista candidata de arquivos da cura, sujeita à escolha do escritor: `logs/MAPA-20/montar_mapa.py`, `logs/MAPA-20/mapa.md`, `bin/mapa20_valida.py`, `bin/relato.sh`, `bin/tests/test_mapa20_valida.sh`, `bin/hasner-relato.service`. Os dois primeiros não estão disponíveis: [nao verificado]. Uma lista fechada incondicional antes de decidir o escritor seria [nao verificado].

## 11/11 13:29 · sessao a8335eaf93cf · - · - · origem=Ronald (tmux)

Transições verificadas na cópia:

| Juiz/operação | Resultado público previsto pelo código | rc | Prova existente |
|---|---|---|---|
| Foto e validador: `bin/relato.sh:97` | Aceito: foto entra no par de raspagem, depois no `git add`, commit e push (`bin/relato.sh:98`, `bin/relato.sh:143`, `bin/relato.sh:153`, `bin/relato.sh:163`, `bin/relato.sh:164`). | 0 se resto concluir; falha posterior também retorna 1 (`bin/relato.sh:164`, `bin/relato.sh:165`). | `verde_novo`, `verde_so_mapa`: `bin/tests/test_mapa20_valida.sh:196`, `bin/tests/test_mapa20_valida.sh:197`. |
| Mapa recusado/ausente: `bin/relato.sh:100` | Novo mapa excluído; antigo, se existir, é raspado e renomeado; resto segue (`bin/relato.sh:102`, `bin/relato.sh:105`, `bin/relato.sh:106`). | 1 mesmo com publicação do restante ou nenhuma mudança (`bin/relato.sh:158`, `bin/relato.sh:165`). | `vermelho_resto`, `vermelho_nada`, `ausente`: `bin/tests/test_mapa20_valida.sh:198`, `bin/tests/test_mapa20_valida.sh:199`, `bin/tests/test_mapa20_valida.sh:200`. |
| Raspagem/rename do retido falha: `bin/relato.sh:105` | Apaga cópia local antiga e adiciona remoção ao commit, caso consiga removê-la (`bin/relato.sh:109`, `bin/relato.sh:152`, `bin/relato.sh:153`). | 1; resto segue se remoção concluir (`bin/relato.sh:110`, `bin/relato.sh:165`). | `raspa_falha`: `bin/tests/test_mapa20_valida.sh:201`. |
| Remoção também falha: `bin/relato.sh:110` | Interrompe antes do commit/push; remoto conserva estado anterior (`bin/relato.sh:110`, `bin/relato.sh:163`, `bin/relato.sh:164`). | 1 (`bin/relato.sh:110`). | `rm_falha`: `bin/tests/test_mapa20_valida.sh:204`. |
| Escritor altera vivo depois do selo: `bin/tests/test_mapa20_valida.sh:154` | Publicação usa foto já capturada (`bin/relato.sh:97`, `bin/relato.sh:98`). | 0 no caso sintético (`bin/tests/test_mapa20_valida.sh:203`). | `regravado`: `bin/tests/test_mapa20_valida.sh:149`, `bin/tests/test_mapa20_valida.sh:203`. |
| Commit falha: `bin/relato.sh:163` | Não alcança push (`bin/relato.sh:163`, `bin/relato.sh:164`). | 1 (`bin/relato.sh:163`). | `commit_falha`: `bin/tests/test_mapa20_valida.sh:202`. |
| Push falha: `bin/relato.sh:164` | Atualização no remoto não demonstrada [nao verificado]. | 1 (`bin/relato.sh:164`). | **SEM PROVA específica**: lista dos nove casos em `bin/tests/test_mapa20_valida.sh:196` até `bin/tests/test_mapa20_valida.sh:204`. |

**Buraco entre julgamento e publicação:** o raspador transforma texto em `bin/raspar_publico.py:31`, confere somente nomes/CPF em `bin/raspar_publico.py:36`, grava em `bin/raspar_publico.py:40`; a chamada acontece depois do julgamento (`bin/relato.sh:97`, `bin/relato.sh:129`). Não há julgamento de conformidade MAPA-20 desses bytes depois da transformação antes do commit (`bin/relato.sh:143`, `bin/relato.sh:163`). O teste usa raspador falso que prefixa `RASPADO:` e compara esse texto, sem validar novamente o resultado (`bin/tests/test_mapa20_valida.sh:158`, `bin/tests/test_mapa20_valida.sh:166`, `bin/tests/test_mapa20_valida.sh:189`). A consequência concreta da raspagem real no mapa vivo depende dos nomes e do mapa, ausentes [nao verificado].

**Leitores do rc:**

- `hasner-relato.service` entrega a execução ao systemd por `ExecStart`, com `Type=oneshot` (`bin/hasner-relato.service:13`, `bin/hasner-relato.service:15`). O arquivo não configura ação específica para MAPA retido, `OnFailure`, `Restart` ou discriminação de rc; termina em comentários depois da afinidade (`bin/hasner-relato.service:19`, `bin/hasner-relato.service:21`). Estado efetivo da unit instalada/monitoramento externo [nao verificado].
- `hasner-relato.timer` fornece agendamento periódico; não lê nem discrimina o rc (`bin/hasner-relato.timer:5`, `bin/hasner-relato.timer:6`, `bin/hasner-relato.timer:8`).
- O teste executa **recortes**, captura `$?`, lê e compara o resultado (`bin/tests/test_mapa20_valida.sh:138`, `bin/tests/test_mapa20_valida.sh:182`, `bin/tests/test_mapa20_valida.sh:183`, `bin/tests/test_mapa20_valida.sh:187`, `bin/tests/test_mapa20_valida.sh:191`).
- Outros chamadores produtivos, aliases, units instaladas ou pessoas lendo o rc [nao verificado]. Os demais arquivos encontrados mencionam o publicador ou verificam sua costura; exemplo: teste apenas procura a chamada ao checador em `bin/tests/test_relato_afirma_com_prova.sh:44`; ele não lê o rc do publicador.

**Ambiguidade:** rc 1 representa tanto publicação parcial bem-sucedida com MAPA retido (`bin/relato.sh:100`, `bin/relato.sh:165`) quanto interrupções antes de publicar (`bin/relato.sh:28`, `bin/relato.sh:32`, `bin/relato.sh:110`, `bin/relato.sh:163`) ou falha de push (`bin/relato.sh:164`). O rc sozinho não distingue esses resultados (`bin/relato.sh:158`, `bin/relato.sh:165`).

## 11/11 13:29 · sessao d5f84499206c · - · - · origem=Ronald (tmux)

Não é possível certificar uma lista futura **FECHADA** apenas pelo código atual: escolher onde pôr a reação da raiz, como transportar o autor e como preservar a instância da pausa são decisões de implementação ainda não materializadas em diff. [nao verificado]

Para um brief com perímetro conservador **proposto**, os arquivos concretos são:

| F3 — produção | Razão verificável |
|---|---|
| `app/chamados/models.py` | Porta de fecho e trilha; carimbo normal e identidade. `:1855`, `:1932`, `:3103`, `:3168`. |
| `app/chamados/signals.py` | Beco agenda fecho sem transportar autor; S143 resolve com usuário sistema. `:228`, `:237`, `:115`. |
| `app/chamados/services/becos.py` | Fecha com parâmetro `sistema`; entrada de evento fornece usuário sistema. `:57`, `:78`. |
| `app/chamados/services/materializacao.py` | Caudas atuais fecham diretamente; necessidade de compatibilidade/idempotência com reação nova da raiz. `:591`, `:1307`, `:1338`. |
| `app/chamados/reconciliador.py` | Três caudas diretas de fecho. `:412`, `:612`, `:1171`. |
| `app/chamados/services/disputa_emissao.py` | Fechos diretos de disputa muda e S128. `:1284`, `:1964`. |
| `app/api/foto_recebida.py` | Valida perguntas antes de chamar `d.fechar`, que pode encontrar disputa já fechada pela reação nova. `:79`, `:80`. |
| `app/ponto/services/entrada_sincronizada.py` | Fecho direto com sistema. `:59`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py` | Cauda direta depois de carimbar perguntas. `:88`. |
| `app/chamados/services/explicador.py` | Ainda responde fechamento por `not fio_vivo`, sem a outra cláusula C7. `:137`. |
| `app/chamados/relatorios_disputa.py` | Ainda emite `PRONTA_FECHAR` apenas por ausência de vivas. `:18`, `:20`. |

F3 — testes existentes adequados para concentrar mudanças: `app/chamados/tests/test_c7_red_minimos.py:147` (`:428` idempotência, `:574` escritor único), `app/chamados/tests/test_varredura_becos_evento_pergunta.py:47` (`:70` hoje exige pai resolvido), `app/chamados/tests/test_o261e_f2_porta_carimbo.py:611`. A alteração futura desses testes é proposta, não fato executado. [nao verificado]

| F4 — produção | Razão verificável |
|---|---|
| `app/chamados/models.py` | Identidade rejeita `instancia_marco` diferente da posição simples derivada do motivo; consultas também derivam posição só pelo motivo. `:3441`, `:3446`, `:2753`. |
| `app/chamados/services/declaracao_texto.py` | Declara perda do índice das pausas; faltantes retornam posição simples e lote agrupa por motivo/dia. `:49`, `:139`, `:582`, `:590`. |
| `app/chamados/services/disputa_emissao.py` | Emissores materializam perguntas dos marcos faltantes. `:823`, `:837`, `:850`, `:875`. |
| `app/chamados/services/fabrica_da_celula.py` | Dedup e emissão atuais operam por motivo/dia. `:145`, `:154`, `:170`, `:172`, `:174`. |
| `app/chamados/services/materializacao.py` | Unidade e lote são individuais; ponto de composição do ato conjunto. `:1163`, `:1225`, `:1381`. |
| `app/chamados/services/validacao.py` | Valida uma pergunta e chama unidade individual antes do lote. `:435`, `:461`, `:463`, `:469`. |
| `app/chamados/services/passivo_validado.py` | Importa e chama unidade individual; não pode deixar caminho alternativo furar unidade do par. `:39`, `:514`, `:516`. |
| `app/chamados/juizes.py` | Leitor de fato do marco ainda verifica todas as pausas para motivos de intervalo; precisa conferir instância efetivamente cobrada. `:930`, `:949`, `:965`. |
| `app/chamados/services/fio.py` | Identidade da família MARCO inclui instância; leitura e migração devem continuar coerentes com seu significado ampliado. `:1373`, `:1460`, `:1495`. |

F4 — testes existentes onde cabe concentrar a mudança: `app/chamados/tests/test_emissao_costura_intervalo.py:50` (`:65` dedup), `app/chamados/tests/test_guarda_paridade_materializador.py:59`, `app/chamados/tests/test_plantio_intervalo_overnight.py:27`, `app/chamados/tests/test_porta_validacao_pergunta.py:224`. Alterações futuras são propostas. [nao verificado]

Dependências de leitura que **não precisam ser declaradas como edição só por serem consumidas**: `app/escala/models.py:1652` fornece pares DNA; `app/escala/services/leitor_celula.py:114` fornece grade; `app/chamados/services/tarefa_do_dia.py:58` e `:92` já enumeram instâncias, e `:109` já aceita instância persistida. Necessidade futura de editá-las: [nao verificado].

Interseção do perímetro conservador proposto F3/F4: `app/chamados/models.py`, `app/chamados/services/materializacao.py`, `app/chamados/services/disputa_emissao.py`. Portanto, nesse perímetro, F3 e F4 não são disjuntas. A propriedade de serem futuras edições continua [nao verificado].

## 11/11 13:29 · sessao 19e32ec7963c · - · - · origem=Ronald (tmux)

Base disponível em `../a026943099b8d88468240fd4b18491141b6810af`; comparação por AST realizada sem importar a aplicação nem executar testes. [nao verificado: identificação do hash sem metadados Git]

Delta pertinente confirmado:

- `editar_ausencia_registro` passou a invalidar o fechamento entre os limites antigo e novo depois do `save`; validação de tipo continua igual. `app/ponto/services/ausencia.py:473-475,496-524`.
- Materialização de ausência retirou minutos fixos e `checar_parcial=False`; as duas chamadas continuam pela criação validada. `app/chamados/services/materializacao.py:474-498,526-534`.
- `cobertura_ausencia_periodo` delega aos novos `_q_cobertura` e `_cobertura_pura`; desconhecido sem fim continua restrito à data inicial. `app/ponto/turnos.py:2752-2783`.
- `lista_ausencias` ganhou decisões pelo catálogo/parcialidade para apresentação. `app/ponto/views.py:2234-2245`.
- Nenhum escritor ou leitor citado desapareceu; exclusões encontradas (`data_saida_esperada`, `_fim_turno_encadeado`) não eram decisões citadas por `Ausencia.tipo`. [nao verificado: constatação negativa da comparação]

Testes que enumeram tipos/catálogos: [nao verificado]

| Arquivo | Evidência |
|---|---|
| `app/ponto/tests/test_catalogo_ausencias.py` | `:10-20,36-42,56-60` |
| `app/ponto/tests/test_cadastro_tipo.py` | `:14-34,40-50` |
| `app/ponto/tests/test_selo_b5f1_porta_ausencia.py` | `:180-194`: exige aceitar tipo vivo fora dos choices |
| `app/api/tests/test_api_ausencias_tipos.py` | `:46-51`: ainda confere GET contra choices |
| `app/ponto/tests/test_aus_f4_criar_pelo_juiz.py` | `:43-54`: enumera válidos fora dos choices |
| `app/ponto/tests/test_fim_separado.py` | `:34-42,55-60` |
| `app/ponto/tests/test_contract_rotulo_x_efeito.py` | `:34-37,69-75` |
| `app/ponto/tests/test_rotulo_x_efeito.py` | `:47-64` |
| `app/ponto/tests/test_rotulo_do_dia_decidido.py` | `:84-92` |
| `app/ponto/tests/test_contract_espelho_sem_emoji.py` | `:271-282` |
| `app/ponto/tests/test_ui3_rotulo_do_espelho.py` | `:25-30,46-64` |
| `app/ponto/tests/test_conjuntos_vivos.py` | `:14-45`: efeitos e exclusões da rubrica |
| `app/ponto/tests/test_catalogo_vivo.py` | `:30-50`: cadastro novo/inativo nos conjuntos |

Esses enumeradores já existiam na base; não foram encontrados novos enumeradores de catálogo entre as adições. [nao verificado: constatação negativa da comparação]

A cura não deve converter a validação para `TIPO_CHOICES`: isso quebraria contrato explícito de aceitação do cadastro vivo. `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`; `app/ponto/tests/test_cadastro_tipo.py:43-50`.

A L-099 já separa estrutura de cadastro/batida e determina destino de admin para os últimos; não exige uma nova pergunta de lei para essa separação. `app/docs/LEIS.md:122`. A regra de origem e de cadastro já existe em `CLAUDE.md:17-27,32-44`.

## 11/11 13:29 · sessao 6eda4fef642f · - · - · origem=Ronald (tmux)

Auditoria estática; não executei selo nem suíte. [nao verificado] [nao verificado]

| Achado v1 | Cura v2 e caso |
|---|---|
| Oito métodos ORM | `Attribute.attr` independe de espaçamento; os oito nomes estão no conjunto. `bin/selo_proporcao.py:28`, `bin/selo_proporcao.py:55`. Casos independentes no laço: `bin/tests/test_selo_proporcao.sh:28`. |
| Espaço depois do ponto | Curado pelo AST; caso `espaco_no_ponto`. `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:31`. |
| Quebra antes de `()` | O exemplo v1 é curado; caso `quebra_de_linha` cria ambas as linhas, portanto não prova continuação isolada adicionada. `bin/selo_proporcao.py:106`, `bin/tests/test_selo_proporcao.sh:32`. |
| Alias | Referência ao atributo agora basta; caso `alias`, sem invocação. `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:33`. |
| `getattr(obj,"save")()` | String exata reconhecida; caso `getattr_string`. `bin/selo_proporcao.py:59`, `bin/tests/test_selo_proporcao.sh:34`. |
| `(obj.save)()` | Reconhecido pelo mesmo `Attribute`; não há caso específico. `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:31`. |
| `#` em string | AST evita truncamento; `cerquilha_string`. `bin/selo_proporcao.py:101`, `bin/tests/test_selo_proporcao.sh:35`. |
| Signal por alias/getattr | O `.connect` do exemplo v1 é reconhecido; caso isolado `signal_connect`, sem reproduzir exatamente o exemplo. `bin/selo_proporcao.py:30`, `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:39`. |
| Command por alias | A referência `M.objects.create` é reconhecida; `command_alias` contém apenas a referência, sem chamada. Não existe regra específica de command. `bin/selo_proporcao.py:55`, `bin/tests/test_selo_proporcao.sh:43`. |
| Migration por rename | Lista `--name-status -z -M` lê destino e regra independe de linhas adicionadas; `migration_rename`. `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:86`, `bin/selo_proporcao.py:94`, `bin/tests/test_selo_proporcao.sh:42`. |
| Binário/`-diff` | Fonte vem de `git show`; linhas usam `--text`; `binario_attr`. `bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:100`, `bin/tests/test_selo_proporcao.sh:44`. |
| Caminho com espaço/Unicode | NUL e `surrogateescape` removem dependência dos cabeçalhos; respectivos casos. Não há caso específico com tabulação. `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:89`, `bin/tests/test_selo_proporcao.sh:45`, `bin/tests/test_selo_proporcao.sh:46`. |
| Falso cabeçalho `+++` no conteúdo | Parser não lê cabeçalhos de caminho; cura no código, sem caso correspondente. `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:100`, `bin/tests/test_selo_proporcao.sh:27`. |
| Revisão interpretada como opção | Rejeita prefixo `-` e resolve commits antes do diff; `opcao_como_revisao`. `bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:80`, `bin/tests/test_selo_proporcao.sh:58`. |
| Importação de módulo de teste em produção | Import estático reconhecido; `prod_importa_teste` cobre `test_writer`, sem caso independente para importação de `tests.writer`. Cura limitada a `Import`/`ImportFrom`; carregamento dinâmico fica fora. `bin/selo_proporcao.py:61`, `bin/selo_proporcao.py:63`, `bin/tests/test_selo_proporcao.sh:47`. |

Persistem problemas nos testes: [nao verificado]

- `signal_nome` possui dois gatilhos independentes no mesmo `ImportFrom`: módulo terminando em `signals` e alias `post_save`; remover qualquer um mantém rc 1. `bin/tests/test_selo_proporcao.sh:38`, `bin/selo_proporcao.py:64`.
- `apagou_migration` não apaga migration: cria `app/x/velho2.py`; não testa o ramo `st == 'D'`. `bin/tests/test_selo_proporcao.sh:54`, `bin/selo_proporcao.py:92`.
- O bloco de `bash -e` não verifica `$?`; em seguida, o único rc comparado vem de `bash -c` sem `-e`. Regressão no cenário anunciado pode passar. `bin/tests/test_selo_proporcao.sh:62`, `bin/tests/test_selo_proporcao.sh:63`.
- A função verifica checkout, resultado global do preparo, commit sem `--allow-empty` e HEAD diferente; melhorou a preparação v1. Porém `bash -c "$4"` executa scripts internos sem `-e`, e comandos separados por `;` podem esconder falha anterior. O preparo especial de `bash -e` também ignora falhas de checkout/add/commit. `bin/tests/test_selo_proporcao.sh:20`, `bin/tests/test_selo_proporcao.sh:21`, `bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:61`.
- `so_remocao` substitui uma chamada por `pass`; cobre remoção da escrita com adição inócua, não patch exclusivamente de remoção. `bin/tests/test_selo_proporcao.sh:49`.
- As exclusões de fixture agora estão isoladas corretamente; também foram acrescentados `bulk_update` e `get_or_create`. `bin/tests/test_selo_proporcao.sh:28`, `bin/tests/test_selo_proporcao.sh:51`, `bin/tests/test_selo_proporcao.sh:52`.

Sobre rc:

- Erros ordinários dentro de `main`, incluindo git, revisão e parse, viram 2; a fronteira adicional captura outras `Exception`. `bin/selo_proporcao.py:34`, `bin/selo_proporcao.py:70`, `bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:124`.
- Não é universal: o wrapper apenas executa Python, sem normalizar falha de inicialização/execução; Python ausente pode produzir 127, não executável 126, e sinais também escapam. Ocorrência real desses ambientes: [nao verificado]. `bin/selo_proporcao.sh:4`.
- Argumentos excedentes são aceitos e ignorados: só existe limite mínimo, e somente quatro posições são consumidas. Uma chamada `base head B repo lixo` pode produzir 0 para diff inócuo. `bin/selo_proporcao.py:70`, `bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:74`, `bin/selo_proporcao.py:119`.
- Rc 1 significa **achado sintático com classe B**, não escrita real demonstrada; referências e strings também geram achados. `bin/selo_proporcao.py:55`, `bin/selo_proporcao.py:59`, `bin/selo_proporcao.py:115`.
- O problema v1 de rc interno 3 desaparece: `exec` preserva diretamente o rc Python. Um chamador `bash -e` que encerre nessa chamada recebe 1; a suíte atual não verifica esse caminho. `bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:117`, `bin/tests/test_selo_proporcao.sh:62`.

## 11/11 13:29 · sessao c92a7eaf4320 · - · - · origem=Ronald (tmux)

Censo dos itens 1 e 5, somente leitura.

- `pode_executar_csf(ch)` retorna `False` se alguma pergunta de **qualquer** disputa em `ch.disputa_origem.all()` ou de `ch.perguntas_que_resolvem.all()` possui `respondida_em is not None`. Retorna `True` nos demais casos, inclusive relações vazias. Não filtra pergunta viva, disputa aberta, texto, validação, materialização, data nem o campo `conversa`. (`app/ponto/supra_juiz.py:539`)
- Único chamador de produção encontrado: comando supra, importação em `app/ponto/management/commands/supra_juiz.py:257` e chamada em `:261`; barrado incrementa `CSF_COM_CONVERSA_PRESERVADO` e recebe `continue`. (`app/ponto/management/commands/supra_juiz.py:261`)
- **Outra função diretamente relacionada à mesma proteção por conversa:** `_tem_resposta_do_colab`, local ao julgamento do cartório, retorna `any((q.resposta_colab or '').strip() for q in perguntas)`. Nos dois leitores, recebe somente `perguntas_que_resolvem.all()`; não examina `disputa_origem` nem exige timestamp. Protege ressuscitação/FIO-DUP depois do bloco CSF. (`app/ponto/services/cartorio.py:1623`, `:1631`, `:1634`, `:1651`)
- Não encontrei terceiro predicado geral de conversa de chamado, nem função `tem_interacao`, após buscas por conversa, diálogo, interação, resposta, Followup e ambas as relações. Ausência em todo o universo: **[não verificado]**; evidência positiva do registro atual são as perguntas declaradas em `app/core/juizes.py:107` e seguintes, que distinguem pergunta viva/fio/disputa, sem declarar a guarda CSF. Mensagem livre de `FollowupChamado` não entra no predicado CSF pelo corpo integral da função. (`app/ponto/supra_juiz.py:543`)

Predicados relacionados, **não equivalentes**:

| Função | Predicado e universo |
|---|---|
| `PerguntaDisputa.respondida` | `respondida_em is not None`; uma pergunta. (`app/chamados/models.py:3496`) |
| `fio_vivo(perguntas)` | `any(pergunta_viva(p))`; coleção inteiramente definida pelo chamador, não exige resposta. (`app/chamados/juizes.py:1488`) |
| `DisputaSupervisao.respostas_sem_veredito` | Perguntas da disputa sem validação/materialização, com timestamp **ou** texto não branco, fora de tranca salvo `ignorar_tranca`; devolve lista. (`app/chamados/models.py:1826`) |
| `DisputaSupervisao.pode_fechar` | Ausência de respostas sem veredito e de fio vivo; disputa. (`app/chamados/models.py:1848`) |
| `irmas_respondidas_sem_consumo(ch)` | Sem materialização/validação, timestamp presente e texto não branco. Universo de crédito: destino é `ch`, ou pai da disputa é `ch` e destino não está vivo. (`app/chamados/juizes.py:1980`, `:2012`) |
| `retencao_da_retratacao(ch)` | Retém pelas irmãs anteriores ou justificativa alheia pendente durante outra aprovação. Não protege toda conversa histórica. (`app/chamados/reconciliador.py:437`) |
| `disputa_viva_do_chamado(ch)` | Primeira disputa aberta diretamente ligada; retém se não tiver perguntas ou tiver fio vivo. (`app/chamados/juizes.py:2371`) |
| `Command._disputa_viva(ch)` do arquivador | Resolve uma disputa aberta por `_disputa_do_chamado`; devolve-a se alguma pergunta for viva; erro produz sentinela conservadora. (`app/chamados/management/commands/arquivar_competencia_encerrada.py:148`) |
| `_disputa_do_chamado(ch)` | Resolve uma disputa aberta, primeiro ligação direta, depois pergunta de destino; não mede conversa. (`app/chamados/services/fio.py:120`) |
| `canonica_do_grupo(perguntas)` | Prioriza pergunta com texto não branco, depois menor PK; universo de perguntas do mesmo fato, não chamado. (`app/chamados/services/fio.py:1225`) |

Testes e selos confirmados: [nao verificado]

| Arquivo | Cobertura |
|---|---|
| `app/ponto/tests/test_supra_juiz_executor.py:38` | Quatro unitários: vazio; pergunta sem timestamp permite; timestamp em disputa barra; timestamp direto barra. |
| `app/ponto/tests/test_supra_juiz_cobranca.py:19` | CSF puro: todas casadas acusa; missing/None/sem chamado vivo/real abaixo do previsto não acusam. |
| `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:89` | Chama `--executar` real. Casos: lavra concorde preserva (`:106`); indevida retrata e conta (`:116`); lavra manda mesmo puro calado (`:124`); sem lavra conta (`:134`); sem código preserva e conta (`:142`); retorno `False` do retratador não vira `CSF_RETRATADO` (`:155`). |
| `app/ponto/tests/test_t61_retratacao_em_seco.py:29` | Constante única, estado vigente `False` (`:40`), chamada de escrita textualmente depois da flag/trava (`:47`), placar travado visível (`:57`), texto do juiz (`:64`), grade da célula (`:76`). |
| `app/ponto/tests/test_cartorio_so_chamado_de_batida.py:53` | Universo do cartório: módulos de batida, paridade com supra (`:63`), chamado sem dia excluído (`:74`). Não testa guarda de conversa no CSF. |
| `app/ponto/tests/test_bug115_lampada_do_marco.py:185` | Fecho parcial pelo consumidor de evento/lastro; satisfeita fecha apesar de outro furo, marco apagado preserva (`:207`). |
| `app/ponto/tests/test_hx_cartorio_ressuscita.py:68` | Resposta textual protege contra ressuscitação, não contra o CSF anterior. |

Os testes acima não demonstram guarda CSF aplicada no cartório; cobertura específica nova será necessária **[não verificado quanto a execução futura]**. Nenhum teste foi executado nesta exploração. [nao verificado]

## 11/11 13:28 · sessao ad57caedd1f2 · - · - · origem=Ronald (tmux)

Não encontrei função que compare duas perguntas e responda se cobram os dois marcos da mesma pausa. O catálogo atual perde o índice da pausa, e o nascimento da pergunta rejeita instância diferente da derivada apenas do motivo. (`app/chamados/services/declaracao_texto.py:49`; `app/chamados/models.py:3438`; `app/chamados/models.py:3446`.)

Varredura AST em memória: `app` e `bin`, excluídos testes, migrations e diretórios gerados; 836 arquivos, 5.369 funções, nenhum erro de parse, 25 candidatos revistos. Evidência de execução sem artefato numerado: [nao verificado]. [nao verificado]

Os candidatos relevantes respondem perguntas diferentes:

| Função/dado | Predicado efetivo |
|---|---|
| `CelulaDia.PARES_PAUSA_DNA` | Declara os pares de chaves `hii/hfi` e `hii2/hfi2`; não compara perguntas. (`app/escala/models.py:1652`.) |
| `EscalaColaborador.pausas_do_dia` | Enumera pares completos do DNA da célula; célula sem marcos retorna vazio. (`app/escala/models.py:1197`; `app/escala/models.py:1207`.) |
| `EscalaColaborador.intervalo_do_dia` | Retorna somente a primeira pausa encontrada pelo método anterior. (`app/escala/models.py:1163`.) |
| `CelulaDia.chaves_do_turno` | Enumera os marcos na ordem canônica do turno. (`app/escala/models.py:1659`.) |
| `tarefa_do_dia._posicoes` | Numera repetições da posição, inclusive `int_ida:2`. (`app/chamados/services/tarefa_do_dia.py:58`; `app/chamados/services/tarefa_do_dia.py:68`.) |
| `tarefa_do_dia.instancias_do_turno` | Expõe instância, posição, tipo, hora, lâmpada e teto; não compara perguntas. (`app/chamados/services/tarefa_do_dia.py:92`; `app/chamados/services/tarefa_do_dia.py:102`.) |
| `fato_do_marco_no_espelho` | Para intervalo, exige acesas todas as lâmpadas daquela posição em todas as pausas cadastradas. (`app/chamados/juizes.py:957`; `app/chamados/juizes.py:971`.) |
| `_resposta_concorda_com_a_luz` | Com mais de uma pausa na posição, retorna `None`: não identifica qual delas a resposta cobra. (`app/chamados/juizes.py:1334`; `app/chamados/juizes.py:1336`.) |

D8 permanece aberta: o comentário declara a lei aberta, e o ramo executável conserva `sem_episodio_intocada`. (`app/chamados/services/fio.py:1274`; `app/chamados/services/fio.py:1275`; `app/chamados/services/fio.py:1294`; `app/chamados/services/fio.py:1295`.)

O trecho do aval “O par é LIDO dos marcos da célula do dia (saída e volta do mesmo intervalo)” responde à identidade da pausa; não fornece o critério de equivalência entre BATIDA sem episódio e BATIDA com episódio. Falta precisamente dizer quando essas linhas são o mesmo fato e se uma pode retratar a outra. Frase e alcance do anexo recebido: [nao verificado].

Perímetro F4 proposto para o brief — proposta técnica, garantia de diff futuro [nao verificado]:

- `app/chamados/models.py`: agrupamento DS e identidade persistida. (`app/chamados/models.py:3425`.)
- `app/chamados/services/declaracao_texto.py`: posição e índice hoje colapsados. (`app/chamados/services/declaracao_texto.py:49`.)
- `app/chamados/services/disputa_emissao.py`: emissão/complemento e seleção por motivo. (`app/chamados/services/disputa_emissao.py:226`; `app/chamados/services/disputa_emissao.py:1081`.)
- `app/chamados/services/fio.py`: consulta dos vizinhos não transmite a instância, embora a chave a contenha. (`app/chamados/services/fio.py:1447`; `app/chamados/services/fio.py:1460`.)
- `app/chamados/services/materializacao.py`: unidade individual, fonte de hora e lote. (`app/chamados/services/materializacao.py:412`; `app/chamados/services/materializacao.py:1163`; `app/chamados/services/materializacao.py:1380`.)
- `app/chamados/services/validacao.py`: materializa uma pergunta antes do lote. (`app/chamados/services/validacao.py:461`; `app/chamados/services/validacao.py:469`.)
- `app/chamados/juizes.py`: leitores existentes de lâmpada e concordância precisam preservar o intervalo cobrado. (`app/chamados/juizes.py:957`; `app/chamados/juizes.py:1334`.)
- Testes existentes apropriados: `app/chamados/tests/test_e1_v5_indice.py:121`, `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:174`, `app/chamados/tests/test_validar_materializa_wiring.py:51`.

A fonte da célula já contém os pares; a necessidade de alterá-la não apareceu nesta leitura. (`app/escala/models.py:1652`.)

## 11/11 13:28 · sessao 67154ccb23ef · - · - · origem=Ronald (tmux)

Evidências confirmadas por comparação real com a cópia `a026943099b8d88468240fd4b18491141b6810af`; nenhum arquivo foi alterado.

**Delta de escritores**

- `editar_ausencia_registro`: acrescentou captura do intervalo antigo e invalidação do fechamento dos intervalos antigo/novo; validação de tipo continua igual, pelo catálogo vivo. `app/ponto/services/ausencia.py:474-475,497,517-524`.
- Materialização de disputa: retirou os minutos fixos de dia inteiro e `checar_parcial=False`; mantém a criação pela mesma porta. `app/chamados/services/materializacao.py:474,490-499,526-535`.
- Novo command `rederivar_minutos_h06_comp11`: confere identidade incluindo `a.tipo` e chama edição com `tipo=a.tipo`, portanto passa pela validação existente. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142-146,169-172`.
- Novo leitor da lista de ausências: acrescenta parcialidade e fim aberto por tipo à apresentação. `app/ponto/views.py:2233-2243`.
- `criar_ausencia`, `Ausencia.save`, forms, catálogo, cadastro de tipos, admins, checar_ausencia, regularização e commands antigos não tiveram mudança funcional de validação de tipo; `criar` mudou apenas comentário sobre H06 e o modelo mudou help_text de minutos. Referências atuais: `app/ponto/services/ausencia.py:193-195,200-205`; `app/ponto/models.py:757-759,868-908`.

**Escritores e validação**

| Porta | Validação já existente / limite |
|---|---|
| Admin da aplicação | POST aceita o argumento cru, mas chama `criar_ausencia`; este rejeita fora do catálogo vivo. `app/ponto/views.py:2378,2415-2422`; `app/ponto/services/ausencia.py:193-195`. |
| PWA | Validação própria pelo vivo e chamada ao produtor validado. `app/ponto/views.py:2509-2511,2536-2542`. |
| API | Traduz somente aliases legados de atestado/luto e chama produtor validado. `app/api/views.py:1765-1768,1817-1824`. |
| Edição HTTP | Delega a edição validada pelo vivo. `app/ponto/views.py:2794-2802`; `app/ponto/services/ausencia.py:474-475`. |
| Form | `clean_tipo` consulta vivo; porém **nenhum código de produção instancia `AusenciaForm`**: a ocorrência de produção é sua definição. Não atribuir a ele a validação HTTP efetiva. `app/ponto/forms.py:41,60-68`. |
| Django admin | `AusenciaAdmin` usa `SoLeitura`; add/change/delete são negados. `app/ponto/admin.py:37-47`; `app/core/admin.py:59-66`. |
| Copiloto | Prévia valida vivo e escrita chama produtor. `app/ponto/services/checar_ausencia.py:23-24,43-48`. |
| Veredito humano | Passa `falta` ou tipo recebido ao produtor validado. `app/ponto/services/veredito_celula.py:331-340`. |
| Materialização | Mesmo produtor; tipo vem do mapa/argumento. `app/chamados/services/materializacao.py:474,490-499,526-535`. |
| Férias | Literal `ferias`, pelo produtor validado. `app/ponto/services/ausencia.py:1202-1207`. |
| Command novo | Edição validada, conservando o código carregado. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169-172`. |
| Command de rederivar fim | Regrava o valor carregado sem validar tipo. `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-31,42-46`; `app/ponto/services/ausencia.py:1293-1297`. |
| ORM/modelo | `save` deriva carimbo, minutos e intervalo e chama `super`; não valida catálogo nem choices. `app/ponto/models.py:868-908`. |

As portas validadas aceitam **fora dos choices**, desde que no vivo; isso é deliberado e protegido por teste. `app/ponto/services/ausencia.py:361-370,387`; `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`.

O cadastro pode criar código `folga`: valida nome, efeito, código vazio e duplicidade, sem reserva desse código; o serviço não chama `full_clean`. Se ativo, entrará no vivo. `app/ponto/views_tipos_ausencia.py:61-66,80,89-94`; `app/ponto/services/ausencia.py:1301-1310`; `app/ponto/catalogo/ausencias.py:249-252,284-285`. Existência histórica desse cadastro e caminho efetivo da gravação fornecida: **[nao verificado]**.

**Listas e autoridade**

- Autoridade de validade: `TipoAusencia` ativo → `_do_banco` → `vigente` → `codigos_validos`; cache de 60 segundos; tabela vazia/erro recai na semente. `app/ponto/catalogo/ausencias.py:240-285`.
- Choices congelados, 17 entradas: `app/ponto/models.py:661-679,749`.
- Semente runtime, 17 entradas: `app/ponto/catalogo/ausencias.py:87-105`.
- Outra lista `CATALOGO` no form, com regras de duração; desconhecido dessa lista recebe máximo de 30 dias, mas validade é conferida no catálogo vivo. `app/ponto/forms.py:13-29,37-38,60-68`.
- Semente histórica da migration: `app/ponto/migrations/0052_seed_tipoausencia.py:9-37`; choices históricos mais recentes: `app/ponto/migrations/0070_alter_ausencia_tipo.py:19-22`.
- `TIPOS_PAGOS`, bloqueantes e abertos do modelo são aliases derivados, não novas listas de validade. `app/ponto/models.py:680-689`.
- Cadastro declara explicitamente que o admin cria novos tipos e consumidor pergunta flags, sem conhecer nome. `app/ponto/models.py:562-569`.

**Pacote para brief da cura na origem**

A brecha estrutural demonstrável é `Ausencia.save` sem validação; a origem efetiva do registro de produção continua **[nao verificado]**. Para uma cura estritamente de impedir **novo tipo fora do vivo** pelo caminho ORM comum, o arquivo operacional mínimo é `app/ponto/models.py:868-908`, preservando a autoridade `app/ponto/catalogo/ausencias.py:284-285`; impor choices quebraria contrato explícito. `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`.

Se houver decisão de centralizar o validador nomeado e reutilizá-lo, os locais de validação existentes são exclusivamente `app/ponto/catalogo/ausencias.py:284-285`, `app/ponto/services/ausencia.py:193-195,474-475`, `app/ponto/forms.py:60-68` e o novo guard do modelo em `app/ponto/models.py:868`. Não há necessidade demonstrada de editar cascatas de endpoints ou leitores para a cura de validade na origem. A aplicação da guarda a saves de registros legados exige preservar o recorte separado de cura de dado; comportamento pretendido desses saves: **[nao verificado]**.

**Testes vizinhos que efetivamente enumeram catálogo/choices** [nao verificado]

- `app/ponto/tests/test_catalogo_ausencias.py:10-20,39-42,56-60`: igualdade choices↔semente, efeito e partição.
- `app/api/tests/test_api_ausencias_tipos.py:46-51`: anúncios GET limitados aos choices; este teste não cobre cadastro adicional ativo.
- `app/ponto/tests/test_cadastro_tipo.py:14-34,43-50`: seed, vivo, inativo/fallback, proibição de validar pelos choices.
- `app/ponto/tests/test_aus_f4_criar_pelo_juiz.py:43-54`: enumera vivo menos choices e garante aceitação; o laço pode ficar vazio.
- `app/ponto/tests/test_selo_b5f1_porta_ausencia.py:180-194`: caso concreto fora dos choices e dentro do vivo.
- `app/ponto/tests/test_fim_separado.py:37,58-59`: enumera semente e conjuntos.
- `app/ponto/tests/test_rotulo_do_dia_decidido.py:86`: enumera semente para rótulo.
- `app/ponto/tests/test_ui3_rotulo_do_espelho.py:26,47,63`: enumera semente e desconhecido.
- `app/ponto/tests/test_rotulo_x_efeito.py:45`: fornece semente ao teste de efeitos.
- `app/ponto/tests/test_aviso_home_office.py:11`: exige tipo nos choices.
- Contratos adjacentes de arquitetura: `app/ponto/tests/test_contract_porta_ausencia.py:30-41,68-78`; `app/ponto/tests/test_contract_catalogo_congelado.py:42-60`; caso de cadastro adicional: `app/ponto/tests/test_o137_esmeril_ausencia_le_o_cadastro.py:82-91`.

_3889 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
