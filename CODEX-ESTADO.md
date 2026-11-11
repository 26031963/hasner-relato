# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 07:51** · publicado em **11/11/2026 07:52** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 07:51 · sessao 926be1d0ec43 · TRAVA-SELO · verificador · origem=Code

**NÃO LIBERA.** As correções pontuais funcionam, mas a descoberta ainda confunde arquivos candidatos com os testes efetivos, e o selo documental continua aceitando comandos que a porta recusaria. Isso independe da pergunta de desenho do item 6. (`bin/suite.sh:155`, `bin/suite.sh:157`; `bin/tests/test_suite_sh.sh:37`; `bin/suite.sh:60`)

**1. NÃO PROCEDE o fechamento da classe; PROCEDEM curas pontuais.**

Há um contraexemplo compacto nesta cópia:

```text
--only 'api chamados colaboradores comunicados core escala ferias folha holerite inteligencia pautas ponto.tests relatorios'
```

São as LABELS com apenas `ponto` substituído por `ponto.tests`. Executando somente `_suite_inteira` em memória, obtive retorno **1**, portanto “não inteira”. A diferença é `ponto.management.commands.testar_pwa`: o `find` exige sua cobertura porque seu nome começa com `test`, embora ele defina `Command(BaseCommand)` e execute métodos `t01_...` por `handle`. Pela análise estática, deixá-lo fora não retira casos de teste; igualdade da suíte no Django instalado é **[nao verificado]**. (`bin/regua.sh:23`; `bin/suite.sh:155`, `bin/suite.sh:157`; `app/ponto/management/commands/testar_pwa.py:31`, `app/ponto/management/commands/testar_pwa.py:70`, `app/ponto/management/commands/testar_pwa.py:100`)

Logo, “todos os módulos encontrados” e “todos menos um” não discriminam necessariamente inteira e recorte: o módulo omitido pode não fornecer teste algum. Enumerar classes ou métodos também fica fora da comparação implementada, que exige selecionar o módulo ou um ancestral dele; uma enumeração integral concreta por classes nesta cópia é **[nao verificado]**. (`bin/suite.sh:156`; `app/ponto/management/commands/testar_pwa.py:31`)

Quanto às formas perguntadas:

| Forma | Resultado |
|---|---|
| Pasta com nome fora de `test*` | **Não é escape isoladamente:** o padrão restringe arquivos; o `find` atravessa as demais pastas. (`bin/suite.sh:158`, `bin/suite.sh:159`) |
| Pasta sem `__init__.py` | A guarda não verifica importabilidade nem `__init__`; pode exigir arquivos que a descoberta efetiva ignore. Efeito no Django instalado: **[nao verificado]**. (`bin/suite.sh:158`; `app/core/runner_quarentena.py:76`) |
| Pasta prunada | Os quatro nomes são excluídos pelo script, sem regra equivalente no override do runner. Se houver testes efetivos ali, um recorte que cubra apenas o restante será recusado como inteira. Ocorrência atual: **[nao verificado]**. (`bin/suite.sh:158`; `app/core/runner_quarentena.py:75`) |
| Symlink | A busca não usa `-L`, e exige `-type f`; não representa integralmente arquivos e diretórios alcançáveis por links. Divergência concreta da suíte desta cópia: **[nao verificado]**. (`bin/suite.sh:158`, `bin/suite.sh:159`) |
| Espaço/acento no caminho externo | O caminho está citado na busca. Nomes selecionáveis de app/módulo, porém, continuam limitados à forma ASCII. (`bin/suite.sh:104`, `bin/suite.sh:149`, `bin/suite.sh:158`) |
| `--dir` buscando outra árvore | Não encontrei troca ordinária: a busca usa `<DIR>/app`, e o bind principal usa esse caminho canonicalizado. As sobreposições adicionais são staticfiles, caches, logs e media. (`bin/suite.sh:149`, `bin/suite.sh:194`, `bin/suite.sh:220`; `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:80`) |

Há custo adicional até proporcional a **módulos × palavras selecionadas**, inclusive com motivo válido e em `--seco`: a classificação vem antes dessas decisões. Duração em produção: **[nao verificado]**. (`bin/suite.sh:154`, `bin/suite.sh:156`, `bin/suite.sh:163`, `bin/suite.sh:173`)

As demais curas:

- **PROCEDE:** `_valor` agora recusa um próximo flag como valor; EXTRA continua preservado como array; SECO ganhou representação `%q`. (`bin/suite.sh:51`, `bin/suite.sh:178`, `bin/suite.sh:223`)
- **PROCEDE:** `RecursionError` no parse cai no regex, e `AST-FALHOU` passa ao stderr da chamada direta. (`bin/vizinhos_da_volta.py:68`, `bin/vizinhos_da_volta.py:120`, `bin/vizinhos_da_volta.py:123`; `bin/suite.sh:89`)
- **NÃO PROCEDE fechar a classe documental:** os dois exemplos citados foram barrados, mas o juiz ainda aceita `bash bin/suite.sh --motivo "pre-opus X" --motivo "quero ver"`. Conferi o juiz em memória; o parser usa o último motivo, que a guarda recusaria. Também passa uma linha `echo` contendo o texto do comando. (`bin/tests/test_suite_sh.sh:37`; `bin/suite.sh:60`, `bin/suite.sh:163`)

**2. PROCEDE a ordem antes de env-file/Docker; NÃO PROCEDE tratamento confiável da falha da busca.**

A classificação ocorre antes de `teste_envfile`, da montagem e do executor Docker. O `source recursos.sh` e, com `--volta`, o helper e o arquivo de diagnóstico ocorrem antes dela. (`bin/suite.sh:39`, `bin/suite.sh:86`, `bin/suite.sh:163`, `bin/suite.sh:184`, `bin/suite.sh:197`, `bin/suite.sh:216`)

O status de `find` é perdido na substituição de processo. Diretório ausente é ignorado por `continue`; busca sem resultados termina como **inteira**, pedindo motivo — não vira automaticamente “não inteira”. (`bin/suite.sh:153`, `bin/suite.sh:158`, `bin/suite.sh:161`)

O retorno “não inteira” acontece quando algum resultado emitido não está coberto. Isso também pode ocorrer com uma busca incompleta ou com erro, pois não existe validação posterior de sua conclusão. Uma falha real por permissão nesta cópia é **[nao verificado]**. (`bin/suite.sh:157`, `bin/suite.sh:158`)

**3. PROCEDE a razão dos três novos; NÃO PROCEDE suficiência da cobertura.**

| Mutante | Discriminante correto |
|---|---|
| `executor_extra_junta` | `%q` distingue `--tag x --failfast` de `--tag\ x\ --failfast`; o grep exige argumentos separados. (`bin/tests/test_suite_motivo.sh:23`, `bin/tests/test_suite_motivo.sh:103`, `bin/tests/test_suite_motivo.sh:177`) |
| `valor_aceita_flag` | Os quatro casos `flag --seco` exigem rc 2 **e** “precisa de valor”; eventual recusa posterior não satisfaz o diagnóstico. (`bin/tests/test_suite_motivo.sh:111`, `bin/tests/test_suite_motivo.sh:112`, `bin/tests/test_suite_motivo.sh:178`) |
| `descoberta_por_nome_de_app` | A enumeração completa dos três módulos deve ser inteira; o mutante não reconhece esses nomes e a libera. (`bin/tests/test_suite_motivo.sh:18`, `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:106`, `bin/tests/test_suite_motivo.sh:179`) |

**Sobrevivente:** trocar `"$s".*` por `"$s"*` mantém o resultado das seleções da fixture, que não contém irmãos como `test_x` e `test_x_extra`. A mutação passaria a cobrir indevidamente o segundo pelo primeiro, podendo recusar um recorte legítimo. A comparação foi conferida em memória. (`bin/suite.sh:156`; `bin/tests/test_suite_motivo.sh:18`, `bin/tests/test_suite_motivo.sh:109`)

Remover todo o prune **morre**: inclui `migrations/test_0001.py`, ausente da enumeração esperada como inteira. Trocar o `return 1` por `continue` também **morre**: os recortes passam a ser classificados como inteiros e quebram os positivos. (`bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:106`, `bin/tests/test_suite_motivo.sh:109`; `bin/suite.sh:157`, `bin/suite.sh:161`)

Execução integral do selo e confirmação dos “22 mortos”: **[nao verificado]**; ele cria, escreve e remove arquivos. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:31`)

**4. PROCEDE a compatibilidade dos leitores examinados.**

| Testemunho | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `_porq` / `VOLTA_PORQUE` | Wrapper fornece o caminho; `suite.sh` redireciona para ele o stderr produzido pelo helper. (`bin/volta_intermediaria.sh:32`; `bin/suite.sh:86`, `bin/suite.sh:87`; `bin/vizinhos_da_volta.py:130`) | `suite.sh` reproduz falhas/`AST-FALHOU`; wrapper conta linhas com ` <- `. Esse formato permanece. (`bin/suite.sh:88`, `bin/suite.sh:89`; `bin/volta_intermediaria.sh:34`) |
| `trava.chamada` | Trava falsa grava `%q` por argumento. (`bin/tests/test_suite_motivo.sh:23`) | `_args` extrai labels; greps verificam sufixos. As labels admitidas não precisam de escapes, portanto esses leitores continuam compatíveis. (`bin/tests/test_suite_motivo.sh:28`, `bin/tests/test_suite_motivo.sh:99`, `bin/tests/test_suite_motivo.sh:103`; `bin/suite.sh:104`) |
| `SECO argv` | `suite.sh` imprime as mesmas expansões de seleção, paralelismo e EXTRA do executor. (`bin/suite.sh:178`, `bin/suite.sh:222`, `bin/suite.sh:223`) | O selo ainda verifica apenas o resumo anterior; não protege a nova linha `argv`. (`bin/tests/test_suite_motivo.sh:121`, `bin/tests/test_suite_motivo.sh:122`) |

O wrapper passa a receber `AST-FALHOU` também em seu log principal, pois captura stderr. A régua executa os selos e lê seu status, sem interpretar esses três formatos. (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:42`; `bin/regua.sh:138`, `bin/regua.sh:140`)

Há um resíduo novo: se o helper falhar, o ramo sai antes de remover o temporário automático `_porq`. (`bin/suite.sh:86`, `bin/suite.sh:88`, `bin/suite.sh:90`)

**5. PROCEDEM os descartes sintáticos; NÃO PROCEDEM as generalizações de fechamento.**

- **Último `--only` vence:** descarte correto como fuga isolada; seleção julgada e executada usam o mesmo valor final. (`bin/suite.sh:55`, `bin/suite.sh:150`, `bin/suite.sh:222`)
- **Assimetria de `--tag`:** existe, mas não demonstra ampliação; o valor separado é consumido e EXTRA chega citado. Efeito no runner instalado: **[nao verificado]**. (`bin/suite.sh:121`, `bin/suite.sh:124`, `bin/suite.sh:125`, `bin/suite.sh:223`)
- **Mapa de campos:** as novas representações têm leitores compatíveis nos caminhos examinados. (`bin/suite.sh:87`, `bin/suite.sh:178`; `bin/tests/test_suite_motivo.sh:23`, `bin/tests/test_suite_motivo.sh:28`)
- **“Descoberta curou a enumeração” e frase documental agora verdadeira:** não fecham a classe, pelo contraexemplo de `testar_pwa` e pela comparação somente entre prefixos de módulos. (`bin/suite.sh:156`; `app/ponto/management/commands/testar_pwa.py:31`; `CLAUDE.md:101`)
- **Item 6:** não reaberto. Os defeitos acima podem ser julgados sem decidir a unidade de régua/pre-push; o delta mantém a guarda em `suite.sh`. (`bin/suite.sh:163`; `bin/regua.sh:165`, `bin/regua.sh:168`)

**Fecho: NÃO LIBERA**, pela classe de descoberta ainda aberta e pelo selo documental ainda permissivo, independentemente do item 6. (`bin/suite.sh:155`, `bin/suite.sh:157`; `bin/tests/test_suite_sh.sh:37`)

## 11/11 07:50 · sessao 1971b03a273e · - · - · origem=Ronald (tmux)

A cura por descoberta **não fecha a classe**, pois compara nomes de arquivos, sem conferir se esses arquivos contêm casos da suíte. (`bin/suite.sh:155`, `bin/suite.sh:156`, `bin/suite.sh:157`)

Contraexemplo compacto nesta cópia:

```text
--only 'api chamados colaboradores comunicados core escala ferias folha holerite inteligencia pautas ponto.tests relatorios'
```

São as LABELS, trocando apenas `ponto` por `ponto.tests`. A guarda retorna **1**, confirmado executando somente `_suite_inteira` extraída em memória; com as LABELS originais retorna **0**. A diferença é `ponto.management.commands.testar_pwa`, incluído pelo `find test*.py`, mas fora do prefixo `ponto.tests`. (`bin/regua.sh:23`, `bin/suite.sh:148`, `bin/suite.sh:155`, `bin/suite.sh:157`, `app/ponto/management/commands/testar_pwa.py:31`)

Esse arquivo é comando de produção: importa `BaseCommand` e define `Command(BaseCommand)`; os seus métodos são `t01_...`, etc., chamados por `handle`, sem classes `TestCase` ou métodos `test_*`. Assim, sua exclusão não retira casos unittest pela análise estática; igualdade de casos no Django efetivamente instalado **[nao verificado]**. (`app/ponto/management/commands/testar_pwa.py:12`, `app/ponto/management/commands/testar_pwa.py:31`, `app/ponto/management/commands/testar_pwa.py:70`, `app/ponto/management/commands/testar_pwa.py:100`, `app/ponto/management/commands/testar_pwa.py:134`)

Outros limites:

- Enumerar todas as classes/métodos pode somar a mesma suíte, mas nunca cobre o nome mais curto do módulo na comparação implementada. Seleção integral concreta por classes nesta cópia **[nao verificado]**. (`bin/suite.sh:156`)
- Nome da **pasta** fora de `test*.py` não é problema isolado: `find` percorre pastas sem exigir esse padrão; aplica-o somente ao arquivo. (`bin/suite.sh:158`, `bin/suite.sh:159`)
- A guarda não verifica `__init__.py` e pode contar módulos que a descoberta real não percorre; efeito exato no Django instalado **[nao verificado]**. (`bin/suite.sh:158`, `app/requirements.txt:13`, `app/core/runner_quarentena.py:76`)
- `migrations`, `__pycache__`, `node_modules`, `staticfiles` são excluídos explicitamente, embora isso não esteja espelhado em override do runner da cópia. Pode haver recorte falso positivo quando testes reais estejam nesses pacotes; ocorrência atual **[nao verificado]**. (`bin/suite.sh:158`, `app/core/runner_quarentena.py:72`, `app/core/runner_quarentena.py:76`)
- `find` sem `-L` e com `-type f` não segue symlinks de arquivos/pastas; pode subcontar descoberta e chamar recorte de inteira. Ocorrência atual **[nao verificado]**. (`bin/suite.sh:158`, `bin/suite.sh:159`)
- Caminho externo com espaço/acento está citado no cálculo da árvore e no argumento do `find`; não há quebra de palavras ali. Já os nomes selecionáveis permanecem ASCII. (`bin/suite.sh:104`, `bin/suite.sh:149`, `bin/suite.sh:155`, `bin/suite.sh:158`)
- `--dir` escolhe `<DIR>/app` tanto para a guarda quanto para o bind principal, que canonicaliza `DIR`; não achei troca ordinária de árvore nesses caminhos. A montagem adicional sobrepõe `staticfiles` e caches/logs/media; `staticfiles` já é prunado pela guarda. (`bin/suite.sh:149`, `bin/suite.sh:194`, `bin/suite.sh:197`, `bin/suite.sh:220`, `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:80`, `bin/arvore_do_push.sh:81`)
- Custo da classificação é até número de módulos × número de palavras selecionadas, inclusive com motivo válido e em `--seco`, pois `_suite_inteira` é avaliada primeiro. (`bin/suite.sh:154`, `bin/suite.sh:156`, `bin/suite.sh:163`, `bin/suite.sh:173`)

O erro de `find` é ignorado pela substituição de processo; ausência de árvore é ignorada por `continue`. Erro sem resultados conduz a **inteira**, não a “não inteira”; o único retorno 1 explícito é um módulo emitido sem cobertura. Assim, falha de busca pode causar recusa falsa de recorte, mas não há conversão direta `find falhou → não inteira`. (`bin/suite.sh:153`, `bin/suite.sh:157`, `bin/suite.sh:158`, `bin/suite.sh:161`)

O host tem Python 3.12 e nenhum módulo Django instalado; a cópia declara imagem Python 3.13 e Django 5.2.14. Logo não usei a stdlib do host como prova da descoberta instalada, nem executei Django ou scripts que escrevem. (`app/Dockerfile:1`, `app/requirements.txt:13`; ambiente instalado **[nao verificado]**)

## 11/11 07:49 · sessao 103424a4cba2 · - · - · origem=Ronald (tmux)

Os três mutantes novos têm discriminantes pela causa pretendida; a cobertura da guarda continua incompleta. (`bin/tests/test_suite_motivo.sh:102`, `bin/tests/test_suite_motivo.sh:106`, `bin/tests/test_suite_motivo.sh:112`, `bin/tests/test_suite_motivo.sh:177`)

- **executor_extra_junta:** `%q` registra o vivo como `--tag x --failfast` e o mutante como `--tag\ x\ --failfast`; o grep exige três palavras. Conferi essa diferença em Bash, em memória. (`bin/tests/test_suite_motivo.sh:23`, `bin/tests/test_suite_motivo.sh:102`, `bin/tests/test_suite_motivo.sh:103`, `bin/tests/test_suite_motivo.sh:177`)
- **valor_aceita_flag:** a função viva recusa os quatro `f --seco` com rc 2 e “precisa de valor”; a mutada aceita o argumento. Mesmo quando a guarda posterior der rc 2, faltará o diagnóstico exigido: não é morte colateral apenas por rc. Conferi a função em memória. (`bin/suite.sh:51`, `bin/tests/test_suite_motivo.sh:111`, `bin/tests/test_suite_motivo.sh:112`, `bin/tests/test_suite_motivo.sh:178`)
- **descoberta_por_nome_de_app:** a enumeração completa dos três módulos é inteira no vivo, recorte no mutante, porque os módulos não são `appA`/`appA.tests` ou `appB`/`appB.tests`; portanto o mutante deixa passar precisamente a classe que devia barrar. Conferi a função em memória com o `find` da fixture simulado. (`bin/suite.sh:156`, `bin/tests/test_suite_motivo.sh:18`, `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:106`, `bin/tests/test_suite_motivo.sh:179`)

**Sobrevivente plausível demonstrado:** trocar `"$s".*` por `"$s"*`. Todas as seleções da fixture mantêm o retorno do vivo, pois não há módulos irmãos com prefixo coincidente. Com um hipotético `test_x_extra.py`, selecionar somente `test_x` passa a cobrir indevidamente o irmão e pode recusar um recorte legítimo. A comparação das seleções existentes e desse discriminante adicional foi executada somente em memória. (`bin/suite.sh:156`, `bin/tests/test_suite_motivo.sh:18`, `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:109`)

Os outros dois exemplos **não sobrevivem** à cobertura atual:

- Remover todo o `prune` inclui `appA/tests/migrations/test_0001.py`; a enumeração esperada como completa deixa esse arquivo descoberto fora e passa, contrariando o teste negativo. (`bin/suite.sh:158`, `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:106`)
- Trocar `[ $cob = 0 ] || return 1` por `continue` ignora módulos descobertos sem cobertura e acaba retornando inteira para todos os recortes, quebrando os positivos. (`bin/suite.sh:157`, `bin/suite.sh:161`, `bin/tests/test_suite_motivo.sh:56`, `bin/tests/test_suite_motivo.sh:109`)

Execução integral do selo e dos mutantes: **[nao verificado]**, pois o selo cria, escreve e remove arquivos. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:11`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:31`)

## 11/11 07:48 · sessao 40220f0ab37e · FIO-E0-V4-E3 · verificador · origem=Code

**1. PROCEDE o fechamento do sobrevivente parcial; NÃO PROCEDE considerar a matriz inteira coberta.** O marcador continua dependendo de `not _coberto_inteiro_nb and sem_trabalho`; ausência parcial não é integral, e `trabalha=False` prevalece no juiz. Os testes novos exigem o marcador justamente nessa combinação. `app/ponto/services/cartorio.py:1211`, `:1223`; `app/chamados/juizes.py:454`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:309`; `app/ponto/tests/test_x1_folga_sem_adesao.py:126`, `:141`.

| Recorte | Comportamento do código atual |
|---|---|
| Folga + ausência parcial aprovada | Mantém marcador. `app/ponto/services/cartorio.py:1223`; `app/chamados/juizes.py:454`. |
| Feriado + ausência parcial | Com `trabalha=False`, mantém; com `True`/`None`, a ata parcial não dispensa o trabalho pelo juiz existente. Ocorrência concreta desse segundo estado: **[nao verificado]**. `app/chamados/juizes.py:454`; `app/ponto/services/cartorio.py:1253`. |
| Folga dentro de afastamento aprovado integral | Fica sem marcador deliberadamente, pela exceção de integralidade; não reproduz a perda indevida da parcial. `app/ponto/services/cartorio.py:1211`, `:1223`. |
| Folga + ausência ainda não aprovada | Mantém marcador: cobrança aceita não rejeitadas, mas a grade só reconhece aprovadas para folha; `trabalha=False` mantém a dispensa. `app/ponto/turnos.py:2517`, `:2519`; `app/escala/utils.py:1188`; `app/chamados/juizes.py:456`. |

Não encontrei nesses recortes outra perda do marcador **no código atual**. Entretanto, ausência ainda não aprovada permanece uma lacuna dos testes examinados, demonstrada pelo mutante da pergunta 3. `app/ponto/tests/test_x1_folga_sem_adesao.py:99`, `:116`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:131`.

**2. PROCEDE a fragilidade da primeira testemunha, com uma distinção.**

- Se o veredito anterior estiver vazio e o julgamento forçado escrever `concorde`, a igualdade **morde**. Se ambos permanecerem `None`, ou ambos `''`, igualdade e exclusão dos acusadores **passam**. `app/ponto/tests/test_x1_folga_sem_adesao.py:132`, `:135`, `:136`; `app/escala/models.py:1767`.
- O C2b inteiro continua verificando ausência, hash, rejulgamento e veredito final derivado. Porém, esse último veredito é o **posterior à adesão**: não certifica sozinho que o NB inicial estava preenchido. O teste de memória exige positivamente `concorde` nessa etapa. `app/ponto/tests/test_x1_folga_sem_adesao.py:137`, `:141`, `:151`, `:160`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:316`.

O `forcar=True` contorna a guarda NB. A criação da ausência anteriormente aciona o signal sem força, mas o C2b não observa diretamente sua execução nem exige um veredito inicial positivo. A segunda metade cobre explicitamente o julgamento sem força **após a adesão**. `app/ponto/services/cartorio.py:1228`; `app/ponto/signals.py:93`, `:143`, `:147`; `app/ponto/tests/test_x1_folga_sem_adesao.py:116`, `:133`, `:151`.

Os caminhos ordinários sem força existem no signal, no cron e no reconciliador. Há também uma segunda passada produtiva que força condicionalmente; portanto, produção não é exclusivamente sem força. `app/ponto/signals.py:93`; `app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:29`, `:65`; `app/chamados/reconciliador.py:702`; `app/ponto/services/cartorio.py:985`, `:989`.

**3. PROCEDE uma lacuna com sobrevivente demonstrado sem banco; sobrevivência nos 116 ORM: NÃO SEI.**

Mutante plausível na chamada do marcador: acrescentar a condição abaixo, confundindo aprovação com necessidade de registrar a fonte sem adesão. `app/ponto/services/cartorio.py:1223`.

```python
and getattr(_cob_nb, 'status', None) in (None, 'aprovada')
```

Na execução efêmera desta volta, esse mutante passou **19 testes de memória + sete pós-suite**, com zero falhas/erros. Uma testemunha adicional de folga com `aguardando_decisao` produziu `concorde` nos dois códigos, mas somente o original manteve o marcador. Resultado sem transcrição em arquivo: **[nao verificado por arquivo:linha]**. [nao verificado]

A não equivalência tem fundamento no código: esse status entra em cobrança, não constitui cobertura integral aprovada na grade e não deve retirar o marcador da folga. As coberturas dos testes citados são `None` ou aprovadas. `app/ponto/turnos.py:2519`; `app/escala/utils.py:1188`; `app/ponto/services/cartorio.py:1211`, `:1223`; `app/ponto/tests/test_x1_folga_sem_adesao.py:101`, `:120`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:131`.

Os exemplos mais amplos de marcador apenas na integral ou hash completamente cego ao marcador já encontram testemunhas: folga simples exige hash diferente, e C3 exige rejulgamento após batida distante. `app/ponto/tests/test_e3_nb_impressao_memoria.py:264`; `app/ponto/tests/test_x1_folga_sem_adesao.py:181`, `:185`.

Dois limites adicionais: o executor fixa `BASE=7313a031…`, não `52a0022e9`; e `so_acusador` segue sempre pelo executor sem banco, mesmo sem a flag. Logo, “base RED” e essa morte não certificam, sozinhos, comparação ORM contra a base produtiva indicada. `app/ponto/tests/x1_mutantes.py:20`, `:94`, `:105`.

**4. NÃO PROCEDE segundo juiz novo no adaptador; NÃO SEI certificar exaustão do diff total sem a fonte histórica da base. [nao verificado]**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.veredito` | NB deriva, aplica o rename e solicita lavra; a porta grava fisicamente. O ramo normal entrega códigos à mesma porta. `app/ponto/services/cartorio.py:1238`, `:1253`, `:1256`, `:1817`; `app/ponto/portas/celula.py:923`, `:1035`. | Guardas do cartório; retenção; universo de adesão/C5; fábrica; disputa; veto; reconciliador; grade leitora/status/relatório. `app/ponto/services/cartorio.py:1224`, `:1373`; `app/folha/export.py:1241`; `app/chamados/services/adesao.py:32`; `app/escala/services/cadastro_realidade.py:181`; `app/chamados/services/fabrica_da_celula.py:89`, `:120`; `app/chamados/services/disputa_emissao.py:758`; `app/ponto/marcos.py:155`; `app/chamados/reconciliador.py:637`; `app/escala/services/leitor_celula.py:424`; `app/ponto/status_dia.py:68`; `app/relatorios/dia_da_frota.py:51`. |
| `impressao` / `sem_adesao` | NB fornece o booleano; a função acrescenta **`sem_adesao:1` ao material do SHA256**. Não é chave persistida separadamente. A porta grava o hash; o ramo normal omite o argumento, cujo default é falso. `app/ponto/services/cartorio.py:444`, `:525`, `:527`, `:1223`, `:1368`; `app/ponto/portas/celula.py:1066`. | Guardas NB e normal e idempotência da porta. `app/ponto/services/cartorio.py:1224`, `:1373`; `app/ponto/portas/celula.py:1062`. |
| `out['cods']` | Ramo normal escreve; releitura agrega; costura devolve ao lote. NB retorna sem escrever. `app/ponto/services/cartorio.py:1441`, `:1055`, `:1084`, `:1282`. | Fábrica e cluster usam a presença da data julgada **neste ato**; não representa todo veredito gravado. `app/ponto/services/cartorio.py:1002`, `:1006`, `:1012`. |
| `data_civil` sintética | Ambas as espécies recebem `None`; marcos reais recebem a data local do instante. `app/chamados/services/tarefa_do_dia.py:103`, `:316`, `:331`. | Carga entrega o resultado; enriquecimento consulta ações/portas; sombra projeta rota/fila/verbos/fio. Outro consumidor direto: **[nao verificado]**. `app/chamados/services/tarefa_do_dia.py:523`, `:533`, `:545`; `app/chamados/management/commands/diff_e3_sombra.py:27`, `:44`. |
| Retorno de `dia_sem_trabalho` | Autoridade responde pelas duas cláusulas, com exceção explícita da parcial. `app/chamados/juizes.py:454`, `:456`. | Marcador/rename, suprajuiz, tarefa, fábrica, fechamento de chamado, materialização, cluster e vigia de intervalo. `app/ponto/services/cartorio.py:1220`, `:1223`, `:1253`; `app/ponto/supra_juiz.py:496`; `app/chamados/services/tarefa_do_dia.py:207`; `app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/models.py:1284`; `app/chamados/services/materializacao.py:174`; `app/ponto/services/cluster_espurio.py:25`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`. |

Os significados são **distintos e compatíveis**: julgamento usa dia do turno; `data_civil` pertence ao instante do marco. A janela civil `[data−1,data+2)` corresponde às datas inclusivas de `data−1` a `data+1`; ausência usa faixa fechada; período parcial inclui ambas as horas e ancora a madrugada no início do turno. `app/ponto/services/cartorio.py:445`, `:1146`, `:1322`, `:1383`; `app/chamados/services/tarefa_do_dia.py:103`; `app/ponto/turnos.py:2540`, `:2623`, `:2630`.

O adaptador apenas transporta insumos e chama a autoridade. **Não são todos os leitores que usam predicado idêntico:** `_protesta` continua parcialmente migrado, como declarado em JUIZES; trabalho previsto e dispensa de cobrança são perguntas diferentes. `app/ponto/supra_juiz.py:60`, `:71`; `app/ponto/services/cartorio.py:1423`; `app/docs/JUIZES.md:64`, `:66`, `:75`.

**5. PROCEDEM os descartes somente com seus limites; PROCEDE a causa técnica do e3fix6.**

- **Base versus d6:** o HEAD conserva NB e hash sem marcador para trabalho exigível. A história de pouso e o conteúdo exato de `52a0022e9` são dados fornecidos, sem comprovação independente nesta cópia. `app/ponto/services/cartorio.py:1223`, `:1253`; **[nao verificado: histórico/base]**.
- **Relógio:** o residual permanece; anterioridade não está demonstrada pelo HEAD isolado. `app/escala/servico_jornada.py:387`, `:566`; **[nao verificado: anterioridade]**.
- **Reconciliador:** zero candidatos no recorte informado não fecha a classe. O escritor continua carimbando perguntas e podendo fechar disputas; o teste atual exige esse carimbo. `app/chamados/reconciliador.py:595`, `:611`; `app/ponto/tests/test_x1_folga_sem_adesao.py:315`.
- **TXT:** estoque armazenado e regeneração são provas diferentes; C4 verifica registros/contagens, sem gerar outro TXT. `app/folha/views.py:561`; `app/folha/export.py:726`, `:968`; `app/ponto/tests/test_x1_folga_sem_adesao.py:213`, `:219`.
- **e3fix6:** forçar vence a guarda, mas NB retorna antes de escrever `cods`; a objeção ao acesso antigo procede. Diff historicamente vazio: **[nao verificado]**. `app/ponto/services/cartorio.py:1228`, `:1282`, `:1441`.

O relato também ficou desatualizado ao descrever rename por conjunto acusatório e marcador em todo o ramo; o código vivo usa `sem_trabalho` e exclui integralidade. `app/docs/relato_fio_e0_v4_e3_x1.txt:11`, `:22`, `:28`; `app/ponto/services/cartorio.py:1223`, `:1253`.

**Fecho: NÃO LIBERA como pré-verificação final.** Falta tornar positiva a testemunha NB inicial do C2b e cobrir o marcador com ausência ainda não aprovada. São lacunas de verificação; não demonstrei perda desse marcador no HEAD atual. `app/ponto/tests/test_x1_folga_sem_adesao.py:135`, `:136`, `:120`; `app/ponto/services/cartorio.py:1223`.

## 11/11 07:46 · sessao 44f945fbb965 · - · - · origem=Ronald (tmux)

Q4 — **NÃO PROCEDE duplicação nova de juiz; NÃO SEI certificar exaustão do diff total**, pois a cópia não fornece histórico Git verificável. [nao verificado]

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.veredito` | Cartório NB calcula `derivar_veredito`, preserva-o quando `sem_trabalho` e renomeia trabalho para NB (`app/ponto/services/cartorio.py:1238`, `:1253`, `:1256`). Escrita física pela porta única (`app/ponto/portas/celula.py:1035`); método do modelo delega (`app/escala/models.py:1819`). | Guardas do cartório (`cartorio.py:1224`, `:1373`); retenção (`app/folha/export.py:1241`, `:1263`); universo adesão/C5 (`app/chamados/services/adesao.py:32`, `app/escala/services/cadastro_realidade.py:181`); fábrica (`app/chamados/services/fabrica_da_celula.py:89`, `:105`, `:120`, `:183`); disputa (`app/chamados/services/disputa_emissao.py:758`, `:772`); veto de cobrança (`app/ponto/marcos.py:119`, `:155`, `:161`); reconciliador (`app/chamados/reconciliador.py:637`); grade leitora (`app/escala/services/leitor_celula.py:424`), depois tarefa (`app/chamados/services/tarefa_do_dia.py:327`); status (`app/ponto/status_dia.py:68`); relatório (`app/relatorios/dia_da_frota.py:51`). |
| `impressao`, `sem_adesao` | NB passa o booleano produzido pelo juiz (`cartorio.py:1218`–`:1223`); função acrescenta fragmento **`sem_adesao:1`**, e produz SHA256 (`cartorio.py:525`–`:527`). Não é chave persistida separada. Porta grava hash (`app/ponto/portas/celula.py:1066`). | Guarda NB (`cartorio.py:1224`, `:1228`), guarda normal (`:1373`, `:1377`) e idempotência da porta (`app/ponto/portas/celula.py:1062`). Normal omite argumento e usa default falso (`cartorio.py:444`, `:1362`–`:1368`). |
| `out['cods']` | Ramo normal (`cartorio.py:1441`), releitura agrega (`:1055`), costura devolve ao lote (`:1084`–`:1089`). NB retorna sem escrever (`:1282`). | Fábrica e cluster, usando presença da **data julgada neste ato** (`cartorio.py:1002`–`:1013`). Não representa todos os vereditos gravados: NB fica fora (`:1282`). |
| `data_civil` sintética | Ambas as espécies recebem `None` (`app/chamados/services/tarefa_do_dia.py:316`, `:331`). Marcos reais usam data local do instante do marco (`:103`). | Carga entrega dicionário (`:523`, `:533`); enriquecedor lê ações/portas (`:545`–`:554`). Sombra projeta somente rota/fila/verbos/fio (`app/chamados/management/commands/diff_e3_sombra.py:27`, `:44`). Outro consumidor direto na cópia: [nao verificado]. |
| retorno de `dia_sem_trabalho` | Autoridade única: parcial em ata exige `trabalha is False`; demais casos aceitam `trabalha=False` ou tipo preenchido diferente de trabalho (`app/chamados/juizes.py:454`–`:456`). | Cartório marcador e renomeação (`cartorio.py:1220`, `:1223`, `:1253`), suprajuiz (`app/ponto/supra_juiz.py:496`), tarefa (`app/chamados/services/tarefa_do_dia.py:207`), fábrica (`app/chamados/services/fabrica_da_celula.py:112`, `:201`), fechamento de chamado (`app/chamados/models.py:1284`), materialização (`app/chamados/services/materializacao.py:174`), cluster (`app/ponto/services/cluster_espurio.py:25`), vigia de intervalo (`app/ponto/management/commands/detectar_intervalo_ausente.py:51`), `_celula_furada` (`app/chamados/juizes.py:494`). |

Os significados são diferentes e explicitados: julgamento é por **dia do turno**, indexado por `cel.data`; `data_civil` pertence ao instante de cada marco e pode cair no dia seguinte. A janela de batidas `[data-1,data+2)` equivale ao filtro de datas civis inclusivas de `data-1` a `data+1`; a conversão entre `lt` e `lte` desconta um microssegundo. Cobertura de ausência fecha ambas as pontas; carga da tarefa também usa `[ini,fim]`. (`app/ponto/services/cartorio.py:445`, `:1135`, `:1140`, `:1322`, `:1383`; `app/ponto/turnos.py:2540`; `app/chamados/services/tarefa_do_dia.py:103`, `:517`, `:562`.)

**Não se deve afirmar que todo leitor usa predicado idêntico.** `_protesta` continua lendo somente `trabalha is not False` e marcos acesos; essa migração parcial já está declarada. Também há dois efeitos deliberados de cobertura: folha exige aprovada; cobrança admite não rejeitada. (`app/ponto/services/cartorio.py:1422`–`:1423`; `app/docs/JUIZES.md:75`–`:77`; `app/ponto/turnos.py:2511`–`:2519`.)

O adaptador **não é segundo juiz**: transporta `trabalha` e `ata`, exige o insumo e chama `chamados.juizes.dia_sem_trabalho`. A pergunta de trabalho **previsto** pertence a `EscalaColaborador.eh_dia_trabalho`; a de cobrança que pode calar pertence a `dia_sem_trabalho`. (`app/ponto/supra_juiz.py:60`–`:71`; `app/docs/JUIZES.md:64`–`:69`, `:103`.)

A mudança de NB para palavra não acusadora pode alcançar o reconciliador: seu seletor exige palavra preenchida fora dos acusadores, depois carimba perguntas e pode fechar disputa. Isso é leitura existente da nova palavra, sem recalcular a folga; zero candidatos na classe inteira não está demonstrado. (`app/chamados/reconciliador.py:595`, `:611`–`:614`, `:637`–`:641`; [nao verificado].)

## 11/11 07:46 · sessao 65b60786f4c1 · - · - · origem=Ronald (tmux)

1. **PROCEDE o fechamento do mutante parcial original.** O marcador real depende de `not _coberto_inteiro_nb and sem_trabalho`, sem excluir parcial; `trabalha=False` prevalece na ausência parcial. O novo teste exige hash com marcador e a morte está no executor. `app/ponto/services/cartorio.py:1211`, `:1223`; `app/chamados/juizes.py:454`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:309`; `app/ponto/tests/x1_mutantes.py:36`.

   Folga com ausência ainda aguardando conserva marcador: a grade usa apenas aprovadas, a cobertura de cobrança aceita não rejeitadas, e a célula `False` mantém `sem_trabalho=True`. `app/escala/utils.py:1188`; `app/ponto/turnos.py:2516`; `app/chamados/juizes.py:456`; `app/ponto/services/cartorio.py:1223`.

   Folga sob afastamento aprovado integral fica sem marcador deliberadamente, pelo filtro de integralidade. `app/ponto/services/cartorio.py:1211`, `:1223`.

   Feriado com ausência parcial depende de `cel.trabalha`: `False` conserva marcador; `True`/`None` com ata parcial resulta em `sem_trabalho=False` pelo juiz existente e recebe NB. Não é diferença criada pela guarda do marcador. Ocorrência em produção e autorização específica desse recorte: **[nao verificado]**. `app/chamados/juizes.py:454`; `app/ponto/services/cartorio.py:1253`.

2. **PROCEDE a fragilidade da primeira testemunha.** As afirmações `antes == depois` e `NotIn(VEREDITOS_ACUSAM)` aceitam ambos `None` ou vazios. O teste não exige `'concorde'` nessa etapa. `app/ponto/tests/test_x1_folga_sem_adesao.py:132`, `:135`, `:136`.

   Isso não torna o C2b inteiro vazio: ainda exige ausência, hash com marcador, rejulgamento sem força depois da adesão e veredito final derivado dos códigos. `app/ponto/tests/test_x1_folga_sem_adesao.py:137`, `:141`, `:151`, `:160`.

   `forcar=True` evita a guarda de impressão na primeira etapa. Assim, não prova o caminho ordinário imediatamente após o signal da ausência; signal e reconciliador chamam sem forçar. A segunda etapa prova explicitamente a transição depois da adesão, sem forçar. `app/ponto/tests/test_x1_folga_sem_adesao.py:133`, `:151`; `app/ponto/services/cartorio.py:1228`; `app/ponto/signals.py:93`, `:143`; `app/chamados/reconciliador.py:702`.

3. **PROCEDE um sobrevivente plausível; execução integral ORM é [nao verificado].** Mutação apenas em memória na linha 1223:

   ```python
   sem_adesao=(
       not _coberto_inteiro_nb and sem_trabalho
       and getattr(_cob_nb, 'status', None) in (None, 'aprovada')
   )
   ```

   Executei os 19 testes de memória e os sete pós-suite com essa fonte: **26 passaram, zero FAIL/ERROR**. Execução efêmera, sem arquivo de transcrição: **[nao verificado por arquivo:linha]**. Os conjuntos existentes usam cobertura `None` ou aprovada; o C2b acrescenta somente aprovada. `app/ponto/tests/test_e3_nb_impressao_memoria.py:91`, `:290`, `:301`, `:311`, `:330`, `:338`, `:353`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:131`; `app/ponto/tests/test_x1_folga_sem_adesao.py:99`, `:116`.

   Testemunha extra em memória, folga + `aguardando_decisao`: original deu `concorde` com marcador; mutante deu `concorde` sem marcador, hash legado. Execução efêmera: **[nao verificado por arquivo:linha]**. A não equivalência decorre do contrato real: cobrança aceita esse status e a grade não o considera cobertura integral aprovada. `app/ponto/turnos.py:2516`; `app/escala/utils.py:1188`; `app/ponto/services/cartorio.py:1195`, `:1211`, `:1223`.

   Não afirmar que esse mutante passou os 116 ORM: **[nao verificado]**. [nao verificado]

   Dois limites do executor também precisam aparecer: seu `BASE` é `7313a031...`, não a base produtiva informada `52a0022e9`; e `so_acusador` sempre segue a execução sem banco mesmo sem `--sem-banco`. `app/ponto/tests/x1_mutantes.py:20`, `:97`.

## 11/11 07:46 · sessao 453485b0cb78 · - · - · origem=Ronald (tmux)

Q2 — **PROCEDE a fragilidade da primeira testemunha do C2b.** `(None, None)` e `('', '')` passam tanto na igualdade quanto no `assertNotIn`, porque o conjunto acusatório não inclui esses valores; não há afirmação positiva de `concorde` nesse ponto. A comparação posterior com `derivar_veredito(out['cods'][d])` verifica o veredito depois da adesão, não o NB inicial. `app/ponto/tests/test_x1_folga_sem_adesao.py:132`, `:135`, `:136`, `:160`; `app/escala/models.py:1767`.

O `forcar=True` vence a guarda NB e deixa de testar a chamada explícita inicial sem força. Entretanto, a criação da ausência anteriormente já aciona o signal sem força, e o teste compara o resultado produzido por ele com o rejulgamento forçado; a lacuna é não exigir resultado inicial preenchido nem observar diretamente a execução do signal. `app/ponto/tests/test_x1_folga_sem_adesao.py:116`, `:133`; `app/ponto/signals.py:93`, `:143`, `:147`; `app/ponto/services/cartorio.py:1224`, `:1228`.

Caminhos produtivos sem força:

- Signal de ausência: chama `julgar_celula(colab, d)`; default `forcar=False`. `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1975`.
- Cron: configuração passa somente `--apply`, command repassa a flag `store_true`. `app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:29`, `:65`.
- Reconciliador: quando faltam lâmpadas na ata, chama `julgar_celula(colab, dia)` sem força. `app/chamados/reconciliador.py:700`, `:702`.
- Há também força em produção: a segunda passada condicional de `out['rejulgar']`. Portanto, “produção nunca força” seria incorreto. `app/ponto/services/cartorio.py:985`, `:989`.

Q5 — **PROCEDE a triagem e3fix6 quanto à causa técnica.** A força altera a guarda, mas o ramo NB retorna antes da escrita de `out['cods']`, feita no ramo normal. O primeiro acesso antigo continuaria falhando. `app/ponto/services/cartorio.py:1228`, `:1282`, `:1441`. O receiver julgador é o de ausência em `:143`; `:275` é consumidor posterior. `app/ponto/signals.py:143`, `:147`, `:275`, `:279`.

**Triagem v2: procede com os limites declarados.** O HEAD conserva NB em trabalho sem cobertura integral e deixa o hash sem marcador quando `sem_trabalho=False`. `app/ponto/services/cartorio.py:1223`, `:1253`. A reconstrução histórica da base 52a0022e9 e a afirmação de que d6 nunca pousou constam somente do dado fornecido; nesta cópia não há repositório Git para comprovação independente. **[nao verificado]**

Relógio: o residual existe, pois ainda há default `_tz.now()` e chamador sem `agora`; anterioridade em relação à obra não é demonstrável pelo HEAD isolado. `app/escala/servico_jornada.py:387`, `:566`. **Anterioridade: [nao verificado].**

Reconciliador: o descarte vale para o recorte informado, não para a classe inteira: continua havendo escritor de carimbo e fechamento de disputa. O teste atual inclusive exige que a pergunta receba `VIA_CELULA_ABSOLVEU`. `app/chamados/reconciliador.py:595`, `:611`, `:1365`; `app/ponto/tests/test_x1_folga_sem_adesao.py:311`, `:315`.

TXT: download usa conteúdo armazenado; geração nova consulta elegibilidade corrente. C4 compara somente registros e contagens após julgamento. Portanto, preservação do estoque procede; identidade numa regeneração não foi provada. `app/folha/views.py:561`; `app/folha/export.py:714`, `:716`, `:726`, `:968`; `app/ponto/tests/test_x1_folga_sem_adesao.py:213`, `:216`, `:219`.

Achado documental: o relato da obra está desatualizado: descreve rename condicionado a `VEREDITOS_ACUSAM` e marcador em todo o ramo, inclusive trabalho/integral; o código atual usa `sem_trabalho` e exclui cobertura integral do marcador. `app/docs/relato_fio_e0_v4_e3_x1.txt:11`, `:16`, `:22`, `:28`; `app/ponto/services/cartorio.py:1223`, `:1253`.

## 11/11 07:42 · sessao 826204932097 · TRAVA-SELO · verificador · origem=Code

**NÃO PROCEDE o fechamento integral.** As sete correções pontuais têm efeito, mas permanecem a enumeração de módulos fora da classificação, uma lacuna na preservação de argumentos pelo selo e chamadas inteiras sem motivo em régua/pre-push. (`bin/suite.sh:137`, `bin/tests/test_suite_motivo.sh:19`, `bin/regua.sh:165`, `bin/pre-push.sh:186`)

A execução integral dos selos nesta auditoria é **[não verificado]**: eles criam e removem temporários. Conferi os caminhos relevantes em memória, incluindo os sete mutantes novos. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:11`, `bin/tests/test_suite_motivo.sh:15`)

**1) NÃO PROCEDE como fechamento de todas as classes; PROCEDE para as correções sintáticas abaixo.**

| Forma de argumento | Resultado da conferência |
|---|---|
| `--only ''`, apenas espaços; `--dir`, `--motivo`, `--volta` vazios ou sem valor no fim | Agora recusam com rc 2 e “precisa de valor”. (`bin/suite.sh:50`, `bin/suite.sh:53`, `bin/suite.sh:54`, `bin/suite.sh:59`, `bin/suite.sh:60`) |
| Caminho/glob em `ONLY`, inclusive depois de label válida | Continuam recusados pela forma e pertinência; o executor permanece protegido contra glob. (`bin/suite.sh:70`, `bin/suite.sh:98`, `bin/suite.sh:100`, `bin/suite.sh:101`) |
| Token cru vazio ou `'--failfast --reverse'` depois de `--` | Rc 2 antes do custo. (`bin/suite.sh:115`, `bin/suite.sh:116`, `bin/suite.sh:124`, `bin/suite.sh:168`) |
| `--failfast=x`, `--reverse=1`, `--settings=x` | Recusados: `=valor` só admite nomes de `_FLAG_VALOR`. (`bin/suite.sh:109`, `bin/suite.sh:119`, `bin/suite.sh:120`) |
| `-k`/`--tag` sem valor no fim de EXTRA | Rc 2; o ramo exige uma palavra seguinte. (`bin/suite.sh:118`, `bin/suite.sh:120`) |
| Palavras repetidas em `ONLY` | Não substituem pertinência: todas as LABELS continuam necessárias para classificar inteira. (`bin/suite.sh:142`, `bin/suite.sh:145`) |
| Opção superior repetida, como `--only api --only core` | Última atribuição vence; o recorte efetivo é `core`. (`bin/suite.sh:54`, `bin/suite.sh:206`) |
| `--only=api`, `-v2`, `-kpadrao` | Continuam recusados pelas formas implementadas. Obrigatoriedade dessas formas no runner instalado: **[não verificado]**. (`bin/suite.sh:64`, `bin/suite.sh:117`, `bin/suite.sh:119`) |

**A classe “enumeração de módulos que esgota a inteira” continua aberta**, inclusive quando produzida por `--volta`: a guarda reconhece somente cada app nu ou `<app>.tests`. Uma enumeração concreta que esgote a descoberta desta cópia: **[não verificado]**. (`bin/suite.sh:83`, `bin/suite.sh:137`, `bin/suite.sh:145`, `bin/suite.sh:206`)

Há uma assimetria restante: `-- --tag 'dois termos'` passa, mas `-- '--tag=dois termos'` é recusado por conter espaço antes de chegar ao ramo `=valor`. Vazio também passa como valor consumido (`--tag ''`) ou embutido (`--tag=`). Isso não demonstra ampliação: EXTRA chega citado ao executor; o efeito no `DiscoverRunner` instalado é **[não verificado]**. (`bin/suite.sh:115`, `bin/suite.sh:118`, `bin/suite.sh:119`, `bin/suite.sh:207`; `app/core/runner_quarentena.py:15`, `app/core/runner_quarentena.py:76`)

**Novo problema concreto de consumo:** `--only api --motivo --seco` aceita `--seco` como texto de MOTIVO, deixa `SECO=0` e ultrapassa o desvio seco. Isso perde a intenção de simulação; não é fuga demonstrada para inteira. (`bin/suite.sh:47`, `bin/suite.sh:50`, `bin/suite.sh:59`, `bin/suite.sh:149`, `bin/suite.sh:159`)

O `set -f` continua antes das expansões não citadas da seleção; não antecede literalmente todas as expansões do script, pois parser e cálculo de RAIZ vêm antes. (`bin/suite.sh:35`, `bin/suite.sh:53`, `bin/suite.sh:70`, `bin/suite.sh:87`, `bin/suite.sh:206`)

**2) PROCEDE o mapa abaixo; NÃO PROCEDE uma inversão dos retornos de `_valor`/`_extra_ok`.**

| Campo/retorno | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `ONLY` | Inicialização, parser, helper e filtro: `bin/suite.sh:47`, `bin/suite.sh:54`, `bin/suite.sh:83`, `bin/suite.sh:90` | Conflito, filtro, vazio, forma, classificação, seco e executor: `bin/suite.sh:81`, `bin/suite.sh:87`, `bin/suite.sh:91`, `bin/suite.sh:100`, `bin/suite.sh:142`, `bin/suite.sh:161`, `bin/suite.sh:206` |
| `EXTRA` | Array inicial e captura citada: `bin/suite.sh:47`, `bin/suite.sh:62` | Validação, seco e executor: `bin/suite.sh:112`, `bin/suite.sh:113`, `bin/suite.sh:118`, `bin/suite.sh:162`, `bin/suite.sh:207` |
| `MOTIVO` | `bin/suite.sh:47`, `bin/suite.sh:59` | Guarda, diagnóstico e seco: `bin/suite.sh:149`, `bin/suite.sh:151`, `bin/suite.sh:160` |
| `VOLTA` | `bin/suite.sh:47`, `bin/suite.sh:60` | Ativação, helper, erro e seco: `bin/suite.sh:80`, `bin/suite.sh:83`, `bin/suite.sh:84`, `bin/suite.sh:160` |
| `PARALELO` | Default e parser, após validação: `bin/suite.sh:47`, `bin/suite.sh:55`, `bin/suite.sh:56` | Seco e executor: `bin/suite.sh:162`, `bin/suite.sh:207` |
| `ESPERA` | Array inicial e parser: `bin/suite.sh:47`, `bin/suite.sh:57`, `bin/suite.sh:58`; escalar na trava: `bin/trava_teste.sh:22`, `bin/trava_teste.sh:27` | Seco, chamada da trava e `flock`: `bin/suite.sh:162`, `bin/suite.sh:200`, `bin/trava_teste.sh:36` |
| Retorno `_valor` | Sucesso 0; falha **encerra o processo** com 2: `bin/suite.sh:50` | Chamadores prosseguem após sucesso, sem capturar status: `bin/suite.sh:53`, `bin/suite.sh:54`, `bin/suite.sh:59`, `bin/suite.sh:60` |
| Retorno `_extra_ok` | Erro retorna 1; conclusão normal retorna 0: `bin/suite.sh:116`, `bin/suite.sh:120`, `bin/suite.sh:123` | Chamador converte falha em saída 2: `bin/suite.sh:124` |
| `DIR` | `bin/suite.sh:47`, `bin/suite.sh:53` | Helper, seco e montagem: `bin/suite.sh:82`, `bin/suite.sh:160`, `bin/suite.sh:176`, `bin/suite.sh:178`, `bin/suite.sh:181` |
| `SECO` | `bin/suite.sh:47`, `bin/suite.sh:61` | `bin/suite.sh:159` |
| `LABELS` | Literal em `bin/regua.sh:23`; derivação em `bin/suite.sh:75` | Filtro, pertinência, classificação e fallback: `bin/suite.sh:88`, `bin/suite.sh:99`, `bin/suite.sh:144`, `bin/suite.sh:206` |
| Listas de flags | `bin/suite.sh:108`, `bin/suite.sh:109` | `_extra_ok`: `bin/suite.sh:117`, `bin/suite.sh:118`, `bin/suite.sh:119`, `bin/suite.sh:120` |

As mudanças de representação são explícitas: PARALELO é texto expandido em palavras; ESPERA é array convertido em escalar pela trava; EXTRA permanece array citado, e a trava executa `"$@"`. Não encontrei nesses caminhos uma inversão de significado comprovada. (`bin/suite.sh:56`, `bin/suite.sh:58`, `bin/suite.sh:200`, `bin/suite.sh:207`; `bin/trava_teste.sh:27`, `bin/trava_teste.sh:42`)

A diferença semântica relevante está em **ONLY**: a guarda reconhece inteira por nomes exatos, enquanto o executor entrega módulos ao runner. O `--seco` agora mostra os campos antes omitidos, mas fornece um resumo textual, não o comando Docker completo nem uma representação inequívoca de argv. (`bin/suite.sh:145`, `bin/suite.sh:160`, `bin/suite.sh:162`, `bin/suite.sh:175`, `bin/suite.sh:206`)

**3) PROCEDE para a morte dos sete mutantes pela causa pretendida; NÃO PROCEDE como cobertura suficiente.**

A conferência isolada em memória encontrou estes discriminantes, correspondentes às mutações efetivamente escritas no selo. (`bin/tests/test_suite_motivo.sh:147`, `bin/tests/test_suite_motivo.sh:148`, `bin/tests/test_suite_motivo.sh:149`, `bin/tests/test_suite_motivo.sh:150`, `bin/tests/test_suite_motivo.sh:151`, `bin/tests/test_suite_motivo.sh:152`, `bin/tests/test_suite_motivo.sh:153`)

| Mutante | Causa da reprovação |
|---|---|
| `pattern_na_lista` | `--pattern` sozinho passa no mutante; o vivo recusa rc 2. O novo caso elimina a recusa colateral de `y`. (`bin/tests/test_suite_motivo.sh:86`, `bin/tests/test_suite_motivo.sh:87`) |
| `k_sem_valor` | `-k padrao` passa no vivo; o mutante recusa `padrao` como palavra solta, rc 2. (`bin/tests/test_suite_motivo.sh:92`; `bin/suite.sh:120`) |
| `igual_aceita_qualquer` | Formas proibidas com `=`, agora sozinhas, passam no mutante. (`bin/tests/test_suite_motivo.sh:86`, `bin/tests/test_suite_motivo.sh:87`) |
| `palavra_com_espaco` | O token único `'--failfast --reverse'` volta a casar a lista e passa. (`bin/tests/test_suite_motivo.sh:90`; `bin/suite.sh:117`) |
| `valor_vazio_cego` | Pode continuar dando rc 2 pela guarda de inteira, mas perde “precisa de valor”; o teste cobra esse stderr. (`bin/tests/test_suite_motivo.sh:82`; `bin/suite.sh:151`) |
| `executor_sem_extra` | Continua chegando à trava, mas falta o sufixo EXTRA exigido no registro. (`bin/tests/test_suite_motivo.sh:94`, `bin/tests/test_suite_motivo.sh:95`) |
| `executor_sem_paralelo` | Chega com `--parallel 4`, contrariando `auto` no sufixo exigido. (`bin/tests/test_suite_motivo.sh:94`, `bin/tests/test_suite_motivo.sh:95`) |

**Ressalva de razão:** os novos negativos de flags e da palavra com espaço cobram rc/chegada, sem conferir stderr específico. A morte atual tem a causa certa; a mensagem da recusa não fica protegida nesses casos. (`bin/tests/test_suite_motivo.sh:87`, `bin/tests/test_suite_motivo.sh:90`)

**Sobrevivente plausível concreto:** trocar somente no executor `"${EXTRA[@]}"` por `"${EXTRA[*]}"`. Vários argumentos viram um único argumento, mas a trava falsa registra `$*`: o registro continua idêntico e passa no novo grep. Confirmei essa indistinguibilidade em memória; execução integral desse mutante: **[não verificado]**. (`bin/suite.sh:207`; `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:24`, `bin/tests/test_suite_motivo.sh:95`)

QUEM ESCREVE/QUEM LÊ do testemunho: `_roda` escreve stderr, a trava falsa escreve `trava.chamada`; os greps e `_args` leem esses arquivos. O escritor de `trava.chamada` já perde as fronteiras que o leitor precisaria conferir. (`bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:24`, `bin/tests/test_suite_motivo.sh:28`, `bin/tests/test_suite_motivo.sh:82`, `bin/tests/test_suite_motivo.sh:95`)

O selo documental também continua permissivo: aceita motivo `"pre-opus X;rm"` e até uma linha comentada contendo o comando; a guarda real rejeita aquele motivo. Os selos próprios do wrapper/helper real continuam fora deste teste, que substitui o helper. (`bin/tests/test_suite_sh.sh:35`; `bin/suite.sh:139`; `bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:28`)

**4) NÃO PROCEDE redução numa execução bem-sucedida; PROCEDE a falta de visibilidade na chamada direta.**

A composição é união: `poe()` só acrescenta, a AST acrescenta suas referências e o regex continua rodando integralmente. `SyntaxError` e `ValueError` retornam `None` e preservam o caminho regex. (`bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:68`, `bin/vizinhos_da_volta.py:121`, `bin/vizinhos_da_volta.py:123`)

Há uma ressalva ao fallback universal: `RecursionError` não é capturado. Um texto sintético com `import core.sub.foo` seguido de expressão muito longa reproduziu essa exceção em memória; o helper aborta antes do regex e da impressão das labels. Isso causa falha integral, rc 1 em `suite.sh`, sem `AST-FALHOU`; ocorrência na obra: **[não verificado]**. (`bin/vizinhos_da_volta.py:67`, `bin/vizinhos_da_volta.py:68`, `bin/vizinhos_da_volta.py:118`, `bin/vizinhos_da_volta.py:128`; `bin/suite.sh:83`, `bin/suite.sh:84`)

`AST-FALHOU` é escrito em **stderr**. A chamada direta por `suite.sh --volta` o descarta em `/dev/null` por padrão; o wrapper preserva em `<saida>.porque`, mas não o reproduz no log principal. (`bin/vizinhos_da_volta.py:120`; `bin/suite.sh:83`; `bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:34`)

QUEM ESCREVE/QUEM LÊ: `cita_por_ast` escreve retorno conjunto/`None`; o laço lê esse retorno para diagnóstico e inclusão; `poe` escreve `saida/porque`; o laço final lê ambos para stdout/stderr. (`bin/vizinhos_da_volta.py:69`, `bin/vizinhos_da_volta.py:87`, `bin/vizinhos_da_volta.py:118`, `bin/vizinhos_da_volta.py:119`, `bin/vizinhos_da_volta.py:121`, `bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:128`, `bin/vizinhos_da_volta.py:130`)

**5) PROCEDE para as correções documentais da v8; NÃO PROCEDE fidelidade integral.**

Foram corrigidos paralelismo `N|auto`, espera, seleção de todos os apps e indicação do wrapper com ruff/disjuntor. Isso corresponde aos respectivos caminhos do código. (`CLAUDE.md:100`, `CLAUDE.md:101`, `CLAUDE.md:102`, `CLAUDE.md:103`; `bin/suite.sh:55`, `bin/suite.sh:57`; `bin/volta_intermediaria.sh:23`, `bin/volta_intermediaria.sh:29`, `bin/volta_intermediaria.sh:32`)

A frase “recorte que cubra todo app das LABELS” é mais abrangente que o reconhecimento implementado, que só aceita app nu/`app.tests`; o limite de enumeração de módulos está no script, não nessa descrição. (`CLAUDE.md:101`, `CLAUDE.md:102`; `bin/suite.sh:137`, `bin/suite.sh:145`)

Os exemplos incompletos da ajuda foram corrigidos. Não identifiquei outra orientação operacional atual ensinando `suite.sh` nu; ausência absoluta fora do universo pesquisado: **[não verificado]**. A chamada nua do selo é fixture negativa; o comando sem motivo no RELATO é registro histórico, não demonstração de orientação vigente. (`bin/suite.sh:29`, `bin/suite.sh:30`, `bin/suite.sh:31`; `bin/tests/test_suite_sh.sh:82`, `bin/tests/test_suite_sh.sh:98`; `app/docs/RELATO.md:681`)

**6) PROCEDE que há triagem incompatível com o aval.**

- **Régua/pre-push:** reconhecer o fato e colocá-lo “fora desta volta” não fecha o aval. A unidade fornecida é **chamada de suíte inteira pela trava**, e os dois fazem essa chamada sem motivo; a trava não impõe essa guarda. Evitar aninhamento não exige dispensar motivo. (`triagem_v6.txt:7`; `aval_trava_menos_fila_com_selo_1111.txt:1`; `bin/regua.sh:165`, `bin/regua.sh:168`; `bin/pre-push.sh:142`, `bin/pre-push.sh:186`; `bin/trava_teste.sh:42`; `CLAUDE.md:104`)

- **Módulos e vizinhos:** declarar limite não demonstra autorização para dispensá-lo. O aval exige vizinhos e transformação de escapes em regra de classe; a defesa “inteira pre-opus” continua sendo uma limitação declarada pelo instrumento. Fechamento dessas classes: **NÃO PROCEDE**; uma enumeração concreta equivalente à inteira: **[não verificado]**. (`triagem_v6.txt:2`; `aval_trava_menos_fila_com_selo_1111.txt:1`; `bin/suite.sh:137`; `bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:16`)

- **Flags consumindo valores:** procede a triagem de que isso, sozinho, não provou ampliação; o executor preserva EXTRA e a descoberta é delegada. (`triagem_v6.txt:4`; `bin/suite.sh:118`, `bin/suite.sh:207`; `app/core/runner_quarentena.py:76`)

- **“19 mortos, rc 0” e “AST-FALHOU visível”:** o primeiro resultado integral é **[não verificado]** nesta auditoria e não elimina o sobrevivente de fronteiras de argv; o segundo só é verdadeiro com stderr preservado. (`triagem_v6.txt:5`, `triagem_v6.txt:8`; `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:95`; `bin/suite.sh:83`)

- **Selo específico do wrapper:** **NÃO SEI** que esse formato particular seja obrigação literal do aval; a cobertura do fluxo continua ausente do selo examinado. (`aval_trava_menos_fila_com_selo_1111.txt:1`; `bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:28`)

## 11/11 07:38 · sessao a79c577f1b80 · - · - · origem=Ronald (tmux)

Não achei escape sintático concreto adicional além da enumeração de módulos já declarada como limite; a guarda só reconhece os nomes exatos `app`/`app.tests`, enquanto o executor aceita qualquer label pontuada válida. (`bin/suite.sh:98`, `bin/suite.sh:136`, `bin/suite.sh:145`, `bin/suite.sh:206`)

Conferência em memória de trechos literais do parser, guardas e seco, sem executar helper, recursos, envfile ou trava:

| Entrada | Resultado | Fonte |
|---|---|---|
| `--only ''`, `--only ' \t\n'`, flag sem valor no fim | rc 2, “precisa de valor” para os quatro campos de `_valor` | `bin/suite.sh:50`, `bin/suite.sh:53`, `bin/suite.sh:54`, `bin/suite.sh:59`, `bin/suite.sh:60` |
| `--only=api` | rc 2, argumento desconhecido; o parser superior só implementa valor separado | `bin/suite.sh:52`, `bin/suite.sh:64` |
| `--only api --only core --seco` | rc 0, seleção final `core`: última atribuição vence | `bin/suite.sh:54`, `bin/suite.sh:161` |
| `--only 'api api' --seco` | rc 0; a repetição não satisfaz os demais apps na classificação | `bin/suite.sh:142`, `bin/suite.sh:145` |
| `--only api --seco -- -k` | rc 2, “não é flag permitida” | `bin/suite.sh:118`, `bin/suite.sh:120` |
| `--only api --seco -- --failfast --failfast` | rc 0 | `bin/suite.sh:117` |
| `--only api --seco -- --tag ''` | rc 0, valor vazio preservado | `bin/suite.sh:118`, `bin/suite.sh:162` |
| `--only api --seco -- --tag=` | rc 0 | `bin/suite.sh:119` |
| `--only api --seco -- -k 'dois termos'` | rc 0: a palavra consumida como valor não é inspecionada | `bin/suite.sh:118` |
| `--only api --seco -- '--tag=dois termos'` | rc 2, “tem espaço”: a inspeção acontece antes da identificação da forma `=valor` | `bin/suite.sh:115`, `bin/suite.sh:119` |

Há assimetria comprovada entre valor separado e `=valor` quando contém espaço; se essa recusa é indevida no runner instalado: **[não verificado]**. A seleção efetivamente descoberta pelo Django também permanece externa à cópia: a classe local delega `build_suite` ao `DiscoverRunner`. (`bin/suite.sh:115`, `bin/suite.sh:118`, `bin/suite.sh:119`; `app/core/runner_quarentena.py:15`, `app/core/runner_quarentena.py:75`, `app/core/runner_quarentena.py:76`)

`_valor` só garante existência e conteúdo não composto exclusivamente de espaços: uma palavra começando por `--` pode ser consumida como valor. Por exemplo, `--dir --seco` atribui literalmente `--seco` a `DIR`; isso não constitui escape demonstrado para suíte inteira. (`bin/suite.sh:50`, `bin/suite.sh:53`)

| Campo | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `ONLY` | inicialização `:47`; parser `:54`; helper `:83`; filtro `:90` | conflito `:81`; filtro `:87`; vazio `:91`; guarda de forma `:100`; classificação `:142`; seco `:161`; executor `:206`, todos de `bin/suite.sh` |
| `EXTRA` | `bin/suite.sh:47`, `bin/suite.sh:62` | `_extra_ok` em `bin/suite.sh:112`, `bin/suite.sh:113`, `bin/suite.sh:118`; seco `bin/suite.sh:162`; executor citado `bin/suite.sh:207` |
| `MOTIVO` | `bin/suite.sh:47`, `bin/suite.sh:59` | guarda `bin/suite.sh:149`; diagnóstico `bin/suite.sh:151`; seco `bin/suite.sh:160` |
| `VOLTA` | `bin/suite.sh:47`, `bin/suite.sh:60` | ativação `bin/suite.sh:80`; helper/redireção `bin/suite.sh:83`; erro `bin/suite.sh:84`; seco `bin/suite.sh:160` |
| `PARALELO` | `bin/suite.sh:47`, `bin/suite.sh:56`; validado em `bin/suite.sh:55` | seco `bin/suite.sh:162`; executor expande em duas palavras `bin/suite.sh:207` |
| `ESPERA` | array `bin/suite.sh:47`, `bin/suite.sh:58`; validado em `bin/suite.sh:57` | seco achata para texto `bin/suite.sh:162`; trava recebe array `bin/suite.sh:200`; trava reescreve escalar em `bin/trava_teste.sh:22`, `bin/trava_teste.sh:27`; `flock` lê em `bin/trava_teste.sh:36` |
| retorno `_valor` | sucesso 0 dos testes; falha termina o processo com 2 | chamadores superiores prosseguem após sucesso; não capturam retorno explicitamente. (`bin/suite.sh:50`, `bin/suite.sh:53`, `bin/suite.sh:54`, `bin/suite.sh:59`, `bin/suite.sh:60`) |
| retorno `_extra_ok` | retorno 1 explícito nos erros; conclusão normal retorna 0 | `bin/suite.sh:116`, `bin/suite.sh:120`, `bin/suite.sh:123`; o leitor converte 1 para saída 2 em `bin/suite.sh:124` |

Não há inversão comprovada de significado nesses retornos: `_valor` aborta; `_extra_ok` devolve falha booleana que o chamador transforma em rc 2. (`bin/suite.sh:50`, `bin/suite.sh:116`, `bin/suite.sh:120`, `bin/suite.sh:124`)

O seco agora mostra paralelo, espera e extra, mas não representa integralmente o comando Docker: monta um resumo textual e sai antes da montagem; além disso, `printf '[%s]' "${EXTRA[@]}"` imprime `[]` até para array sem elementos. (`bin/suite.sh:159`, `bin/suite.sh:162`, `bin/suite.sh:163`, `bin/suite.sh:175`, `bin/suite.sh:200`)

## 11/11 07:37 · sessao 4a09f27e1e0a · - · - · origem=Ronald (tmux)

Q4 — **NÃO PROCEDE a hipótese de redução de uma execução bem-sucedida.** A saída é união: `poe()` apenas acrescenta; AST acrescenta antes do regex, que continua sendo executado integralmente. `SyntaxError`/`ValueError` caem no regex. (`bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:68`, `bin/vizinhos_da_volta.py:121`, `bin/vizinhos_da_volta.py:123`)

**PROCEDE uma ressalva ao fallback universal:** `RecursionError` não é capturado. Reproduzi em memória com `ast.parse('import core.sub.foo\nx=' + '+'.join(['1'] * 10000))`: a exceção aborta antes do regex e antes da impressão das labels. Isso causa falha integral, não uma lista menor com sucesso; `suite.sh` devolve rc 1. (`bin/vizinhos_da_volta.py:67`, `bin/vizinhos_da_volta.py:68`, `bin/vizinhos_da_volta.py:118`, `bin/vizinhos_da_volta.py:128`, `bin/suite.sh:83`, `bin/suite.sh:84`)

**AST-FALHOU tem visibilidade condicionada:** o helper escreve o marcador em stderr; `suite.sh --volta` descarta stderr em `/dev/null` por padrão. O wrapper preserva em `<saida>.porque`, mas seu log apenas conta linhas de justificativa. (`bin/vizinhos_da_volta.py:120`, `bin/suite.sh:83`, `bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:34`)

**QUEM ESCREVE / QUEM LÊ:** `cita_por_ast` escreve retorno `set`/`None`; o laço lê `None` para emitir o marcador e lê o conjunto para acrescentar labels; `poe` escreve `saida/porque`; o laço final lê ambos. (`bin/vizinhos_da_volta.py:69`, `bin/vizinhos_da_volta.py:87`, `bin/vizinhos_da_volta.py:118`, `bin/vizinhos_da_volta.py:119`, `bin/vizinhos_da_volta.py:121`, `bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:128`)

A varredura em memória dos 2.220 candidatos desta cópia não apresentou exceções de parse; o contraexemplo de recursão é sintético, sem ocorrência observada nesses candidatos. O universo segue o filtro do helper. (`bin/vizinhos_da_volta.py:43`, `bin/vizinhos_da_volta.py:44`; contagem da sonda sem artefato: **[não verificado]**)

## 11/11 07:37 · sessao 458d41f06c57 · FIO-E0-V4-E3 · construtor · origem=Code

Aplicada a troca no c2b: testemunha persistida, julgamento forçado e comparação do veredito antes/depois. A afirmação final com `derivar_veredito(out['cods'][self.d])` permanece (`ponto/tests/test_x1_folga_sem_adesao.py:131`, `:160`).

```diff
-        out = self._julgar()
-        self.assertEqual(self.cel.veredito, derivar_veredito(out['cods'][self.d]))
-        self.assertNotIn(self.cel.veredito, CelulaDia.VEREDITOS_ACUSAM)
+        self.cel.refresh_from_db()
+        veredito_antes = self.cel.veredito
+        self._julgar(forcar=True)
+        veredito_nb = self.cel.veredito
+        self.assertEqual(veredito_nb, veredito_antes)
+        self.assertNotIn(veredito_nb, CelulaDia.VEREDITOS_ACUSAM)
```

Por leitura, `parcial_tira_marcador` remove o marcador quando há cobertura parcial (`ponto/tests/x1_mutantes.py:36`). O juiz mantém folga como dia sem trabalho nessa situação (`chamados/juizes.py:454`). Assim, a comparação da impressão deve falhar por **AssertionError em `ponto/tests/test_x1_folga_sem_adesao.py:141`**, antes do primeiro acesso restante a `cods`, em `:153`. Execução ORM: **[nao verificado]**.

Comandos executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria -v
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo green
```

Ambos: **rc=0, 19 testes** (`ponto/tests/test_e3_nb_impressao_memoria.py:88`; `ponto/tests/x1_mutantes.py:65`).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante literal_incondicional
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante sem_fonte_no_gancho
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante so_acusador
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante parcial_tira_marcador
```

Cada um: **rc=1, FAIL=1, ERROR=0**, por assertion, respectivamente em `ponto/tests/test_e3_nb_impressao_memoria.py:262`, `:264`, `:278`, `:318`. Esses resultados são somente em memória (`ponto/tests/x1_mutantes.py:49`).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ruff check --no-cache ponto/tests/test_x1_folga_sem_adesao.py
```

Ruff: **rc=1**, `No module named ruff`; lint limpo: **[nao verificado]**.

## 11/11 07:36 · sessao 0c75a3ff382c · FIO-E0-V4-E4B · verificador · origem=Code

**NAO LIBERA.** Há defeitos na cerca do backfill, além de condições de certificação ainda não demonstradas. O GREEN informado de 130 testes não comprova a suíte inteira nem resolve os caminhos abaixo. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:297`, `app/chamados/tests/e4b_c4_verificar.py:74`; suíte inteira: [nao verificado])

1. **Achados por severidade**

   - **Alta — efeitos de APPLY escapam da foto, da cerca e do DIFF.** O backfill materializa a disputa inteira; quitada, ela fecha e agenda `retomar_regularizacoes_confirmadas` em `on_commit`. Esse callback procura todas as regularizações confirmadas do colaborador, sem competência alvo, podendo criar/corrigir batidas nas datas dos itens. A foto final precede o commit; DRY descarta esses callbacks. Portanto, o estado medido no DRY não cobre necessariamente o estado final de APPLY. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:145`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:314`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/materializacao.py:1333`, `app/chamados/signals.py:137`, `app/chamados/signals.py:144`, `app/chamados/services/regularizacao_ext.py:218`, `app/chamados/services/regularizacao_ext.py:181`)

   - **Alta — relógio de leitura vira falsa mudança e pode impedir a cura.** `fechamento_lido` devolve objeto calculado com `previsto_em=timezone.now()`. A foto captura esse campo e `mudancas` compara o dicionário inteiro. Duas fotos com dinheiro idêntico podem diferir apenas nesse instante; havendo competência histórica fora do alvo, a cerca desfaz o ato por `fora_da_competencia_alvo`. Isso acontece por leitura, sem escrita monetária correspondente. (`app/ponto/services/fechamento.py:1086`, `app/ponto/services/fechamento.py:1135`, `app/ponto/services/fechamento.py:1759`, `app/chamados/services/foto_e4b.py:194`, `app/chamados/services/foto_e4b.py:15`, `app/chamados/services/foto_e4b.py:32`)

   - **Alta — comp 10 sem `!` não possui impedimento próprio.** O comando admite qualquer competência formatada e seleciona executáveis quando K6 retorna `False`; a cerca aceita `comp == alvo` e `entregue=False`. Logo, `--competencia 10/2026 --aplicar` não é recusado pela condição literal de mês pago fora do sistema. O recibo reconhece que deixou o `!` como condição operacional externa. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:210`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:244`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:246`, `app/chamados/services/foto_e4b.py:32`, `app/docs/FIO-E0-V4-E4B-c4.txt:65`)

   - **Alta — fotografia/reversão não abrange todas as escritas alcançadas.** Não inclui `EventoCelula`, embora ressuscitação e registro de decisão emitam eventos. Também não inclui regularizações/itens do callback acima; a fábrica pode escrever Pauta, cuja porta atualiza `ContadorPauta`, igualmente ausente. A auditoria fotografada restringe-se a seis modelos. (`app/chamados/services/foto_e4b.py:84`, `app/chamados/services/foto_e4b.py:175`, `app/chamados/models.py:1568`, `app/chamados/models.py:3954`, `app/ponto/portas/celula.py:896`, `app/chamados/services/fabrica_da_celula.py:369`, `app/pautas/services.py:188`, `app/pautas/services.py:354`)

   - **Média — campo com dois significados na nova foto.** `abrir_conversa` grava dia civil em `data_evento`; o juiz de dia do turno devolve `None` para conversa. A foto ignora esse discriminador e atribui competência usando `p.data_evento`, propagando-a à disputa e aos chamados. Impacto em registros de produção: [nao verificado]. (`app/chamados/models.py:2794`, `app/chamados/models.py:2801`, `app/chamados/juizes.py:332`, `app/chamados/services/foto_e4b.py:128`, `app/chamados/services/foto_e4b.py:131`, `app/chamados/services/foto_e4b.py:134`)

   - **Média — rechecagem não serializa entrega.** A trava é C/D/Ch/P; o escritor de `ExportacaoDominio` faz `exists/create` sem participar desse protocolo. Continua possível uma entrega após a última leitura K6 e antes do commit. Reprodução concorrente: [nao verificado]. (`app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`, `app/folha/models.py:174`, `app/folha/models.py:182`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:297`)

2. **Mudanças de produção e aval literal**

   | Arquivo | Comportamento/evento | Aval |
   |---|---|---|
   | `chamados/juizes.py` | `tranca_da_consulta` aceita memo inicial e copia suas entradas para a consulta, mantendo cálculo por `dias_trancados`. (`app/chamados/juizes.py:1086`, `app/chamados/juizes.py:1102`, `app/chamados/juizes.py:1107`) | Não cria regra nova; frescor depende da fonte copiada. (`app/chamados/juizes.py:1026`, `app/chamados/juizes.py:1060`) |
   | `disputa_emissao.py` | Silêncio passa a coletar tentativas de push; cargas passam por `_ancoradas`; S143 compartilha mapa de colaboradores e consulta da tranca até C7. (`app/chamados/services/disputa_emissao.py:1748`, `app/chamados/services/disputa_emissao.py:1762`, `app/chamados/services/disputa_emissao.py:1799`, `app/chamados/services/disputa_emissao.py:1937`, `app/chamados/services/disputa_emissao.py:2080`) | A viva não respondida recupera o credor pela porta `reabrir`, com nota, após C6 e âncora canônica. Respondida continua fora dessa cura. (`app/chamados/services/disputa_emissao.py:1821`, `app/chamados/services/disputa_emissao.py:1823`, `app/chamados/services/disputa_emissao.py:1849`) |
   | `foto_e4b.py` | Fotografa campos concretos, calcula delta, cerca ambos os lados e produz relato financeiro. (`app/chamados/services/foto_e4b.py:124`, `app/chamados/services/foto_e4b.py:22`, `app/chamados/services/foto_e4b.py:60`) | Dinheiro vem da autoridade; porém cobertura e comparação temporal têm os defeitos do item 1. (`app/chamados/services/foto_e4b.py:192`, `app/ponto/services/fechamento.py:1086`) |
   | `backfill_fio_morto_e4b.py` | Trava colaboradores, fotografa sob transação, grava/relê reversão, reconfere plano e desfaz cada tentativa que viola a cerca. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:264`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:270`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:288`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:306`) | Implementa BANG e DIFF, mas não garante comp 10 protegida nem efeitos posteriores cercados. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:244`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:355`, `app/chamados/signals.py:144`) |
   | `cartorio.py` | Publica a instância do colaborador durante `julgar_colab` e restaura a anterior no `finally`. (`app/ponto/services/cartorio.py:949`, `app/ponto/services/cartorio.py:1097`) | É transporte de contexto; não decide vitalidade ou tranca. (`app/ponto/services/cartorio.py:90`, `app/ponto/services/cartorio.py:97`) |

   **“Não entregue” não demonstra, sozinho, “aberta”.** K6 responde existência de TXT vigente; o comando/cerca não faz uma verificação própria de tranca ou do pagamento externo da 10. Não achei essa proteção adicional. (`app/folha/export.py:556`, `app/folha/export.py:564`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:283`, `app/chamados/services/foto_e4b.py:30`)

   **Não achei redução nova do contador por exclusão de colaboradores, CADASTRO ou fallback.** O comando continua lendo o universo do vigia, soma a lista complementar de exportadas e remede pelo mesmo vigia; há teste preservando o universo. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:223`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:318`, `app/chamados/tests/test_e4_passivo.py:107`)

3. **Quem escreve e quem lê — contexto, memo, foto e reversão**

   | Campo/valor | Escritor | Leitor |
   |---|---|---|
   | `_tl.colaborador` | Publicação/restauração: `app/ponto/services/cartorio.py:949`, `app/ponto/services/cartorio.py:1097` | `colaborador_deste_julgamento`: `app/ponto/services/cartorio.py:96`; S143: `app/chamados/services/disputa_emissao.py:1917` |
   | `_tl.ativo` | Julgamento, limpeza e `calado`: `app/ponto/services/cartorio.py:948`, `app/ponto/services/cartorio.py:1098`, `app/ponto/services/cartorio.py:105` | Guarda do colaborador e anti-reentrada: `app/ponto/services/cartorio.py:97`, `app/chamados/signals.py:210` |
   | `_tranca_memo` | Dono escolhido por `_memo_da_tranca`: `app/chamados/juizes.py:1060`, `app/chamados/juizes.py:1070` | `dias_trancados`, toque e S143: `app/chamados/juizes.py:994`, `app/chamados/juizes.py:1173`, `app/chamados/services/disputa_emissao.py:1921` |
   | `('empresa', empresa_id)` | `app/chamados/juizes.py:1018` | Janela da competência: `app/chamados/juizes.py:1024` |
   | `('tranca', empresa, mês, ano)` → período/`None` | `app/chamados/juizes.py:1028`, `app/chamados/juizes.py:1029` | `app/chamados/juizes.py:1030` |
   | `('sistema',)` e `('toque', colab, data, desde)` | `app/chamados/juizes.py:1186`, `app/chamados/juizes.py:1190` | `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1175` |
   | `partial(...memo_inicial=...)` | S143 guarda **referência**: `app/chamados/services/disputa_emissao.py:1920` | Cópia efetiva ocorre depois, na entrada do contexto: `app/chamados/juizes.py:1102` |
   | `TRANCA_DA_CONSULTA`, `consulta['dias']` | `app/chamados/juizes.py:1104`, `app/chamados/juizes.py:1107` | C20: `app/chamados/juizes.py:948`; restauração: `app/chamados/juizes.py:1110` |
   | `_PUSH_PREVISTOS` e itens `{colaborador_id, disputa_id, motivos, nova}` | Contexto/coleta: `app/chamados/services/disputa_emissao.py:1748`, `app/chamados/services/disputa_emissao.py:1762` | Corte após rollback e relato por colaborador: `app/chamados/management/commands/backfill_fio_morto_e4b.py:305`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:317` |
   | Foto `.campos` | Cópia de **todos os campos concretos**: `app/chamados/services/foto_e4b.py:124` | Delta, nascimentos e dinheiro: `app/chamados/services/foto_e4b.py:15`, `app/chamados/services/foto_e4b.py:44`, `app/chamados/services/foto_e4b.py:69` |
   | Foto `.alcance[{dia,comp,entregue}]` | `app/chamados/services/foto_e4b.py:102`, `app/chamados/services/foto_e4b.py:115` | Cerca, relato e BANG-LINHA: `app/chamados/services/foto_e4b.py:26`, `app/chamados/services/foto_e4b.py:78`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:345` |
   | `delta`, `tentativa`, `cerca`, `diffs`, `atos` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:299`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:301`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:317`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:352` | Rollback/BANG/DIFF: `app/chamados/management/commands/backfill_fio_morto_e4b.py:303`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:345`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:355` |
   | JSON de reversão | `app/chamados/management/commands/backfill_fio_morto_e4b.py:270`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:60` | Releitura e igualdade antes do ato: `app/chamados/management/commands/backfill_fio_morto_e4b.py:62`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:63` |

   **Não achei executor que restaure esse JSON.** Achei fotografia, releitura e declaração de compensação para decisões imutáveis; capacidade operacional de reversão completa: [nao verificado]. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:56`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:272`)

   O censo dos campos persistidos alcançados fica assim, agrupado pelos donos:

   | Campos/família | Escritores encontrados | Leitores encontrados |
   |---|---|---|
   | Pergunta: `materializada_em/por`, `via_resolucao`, resposta/validação e autores | Carimbo/restauração/ressuscitação; validação: `app/chamados/models.py:3098`, `app/chamados/models.py:3130`, `app/chamados/models.py:3161`, `app/chamados/models.py:1566`, `app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:199` | C6, histórico, foto: `app/chamados/juizes.py:1331`, `app/chamados/juizes.py:1347`, `app/chamados/services/historico.py:98`, `app/chamados/services/foto_e4b.py:143` |
   | Pergunta: identidade, destino, célula, disputa, `data_evento`, `conversa` | Portas de nascimento e identidade: `app/chamados/models.py:2789`, `app/chamados/models.py:2801`, `app/chamados/models.py:3430` | Âncora, C20 e foto: `app/chamados/services/fio.py:117`, `app/chamados/juizes.py:336`, `app/chamados/services/foto_e4b.py:128` |
   | Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | Lavra/carimbo: `app/chamados/veredito_lavrado.py:254`, `app/chamados/models.py:3150` | Tarefa/foto: `app/chamados/services/tarefa_do_dia.py:349`, `app/chamados/services/foto_e4b.py:143` |
   | Chamado: `status_local`, `resolvido_em`, `via_encerramento`, urgência/original, respondente | Núcleo do estado: `app/chamados/models.py:228`, `app/chamados/models.py:244`, `app/chamados/models.py:248`, `app/chamados/models.py:251` | S143, reabertura e foto: `app/chamados/signals.py:96`, `app/chamados/models.py:1342`, `app/chamados/services/foto_e4b.py:147` |
   | Disputa: fechamento, autor, resolução/motivo | Portas fechar/reabrir: `app/chamados/models.py:1855`, `app/chamados/models.py:2163` | S143/C7/foto: `app/chamados/services/disputa_emissao.py:2024`, `app/chamados/services/disputa_emissao.py:2080`, `app/chamados/services/foto_e4b.py:145` |
   | Decisões: ato, instante, autor, texto, motivo e referências | `app/chamados/models.py:3943` | Tarefa/foto: `app/chamados/services/tarefa_do_dia.py:337`, `app/chamados/services/foto_e4b.py:151` |
   | Nota: conteúdo, autoria e `evento_chave` | `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:100`, `app/chamados/services/nota.py:103` | Dedup/histórico/foto: `app/chamados/services/nota.py:109`, `app/chamados/services/historico.py:80`, `app/chamados/services/foto_e4b.py:149` |
   | Célula: veredito, ata e carimbos | Porta da célula: `app/ponto/portas/celula.py:899`, `app/ponto/portas/celula.py:912` | C6/foto: `app/chamados/juizes.py:1378`, `app/chamados/services/foto_e4b.py:142` |
   | DiaPago: identificação, versões, rubricas, faltas, DSR e detalhe | `app/ponto/services/dia_pago.py:335`, `app/ponto/services/dia_pago.py:345`, `app/ponto/services/dia_pago.py:357`, `app/ponto/services/dia_pago.py:360` | Foto/relato: `app/chamados/services/foto_e4b.py:160`, `app/chamados/services/foto_e4b.py:68` |
   | Fechamento: campos monetários e carimbos; objeto online | `app/ponto/services/fechamento.py:1086`, `app/ponto/services/fechamento.py:1100`, `app/ponto/services/fechamento.py:1160` | `fechamento_lido` e foto: `app/ponto/services/fechamento.py:1735`, `app/chamados/services/foto_e4b.py:161`, `app/chamados/services/foto_e4b.py:192` |
   | Batida, Ausência, Pauta e Auditoria: demais campos concretos | Portas identificadas no recibo; callback adicional demonstrado: `app/chamados/services/regularizacao_ext.py:77`, `app/chamados/services/regularizacao_ext.py:155`, `app/pautas/services.py:180` | Captura genérica e alcance: `app/chamados/services/foto_e4b.py:124`, `app/chamados/services/foto_e4b.py:153`, `app/chamados/services/foto_e4b.py:159`, `app/chamados/services/foto_e4b.py:173`, `app/chamados/services/foto_e4b.py:186` |

   **Não certifico esse mapa como censo exaustivo de todos os escritores/leitores dos 249 campos.** O próprio recibo limita o levantamento às portas alcançadas e marca demais escritores/leitores como não verificados; as omissões concretas do item 1 impedem aceitar “integral”. (`app/docs/FIO-E0-V4-E4B-c4.txt:143`, `app/docs/FIO-E0-V4-E4B-c4.txt:176`, `app/chamados/services/foto_e4b.py:175`)

4. **Memo: frescor, outro colaborador, exceção e aninhamento**

   - O colaborador é **do ato**, conferido pela PK do chamado; as cargas continuam usando colaboradores **das perguntas**. Não achei troca indevida entre colaboradores nesse caminho. (`app/ponto/services/cartorio.py:97`, `app/chamados/services/disputa_emissao.py:1917`, `app/chamados/services/disputa_emissao.py:1937`)
   - “Memo do colaborador” é uma descrição incompleta: `_memo_da_tranca(...lote=False)` ainda prefere consulta ativa e depois request; pode devolver memo desses donos. As chaves de toque incluem colaborador/data/instante, e a tranca é por empresa/competência. (`app/chamados/juizes.py:1060`, `app/chamados/juizes.py:1067`, `app/chamados/juizes.py:1025`, `app/chamados/juizes.py:1174`)
   - Existe caminho condicional de memo velho: uma entrada de tranca já presente evita nova consulta, mesmo que a tranca tenha mudado entre E2 e S143. Trancar/reabrir grava `PeriodoFechado` sem invalidar esse memo; não achei invalidação produtiva. Ocorrência em produção: [nao verificado]. (`app/chamados/juizes.py:1026`, `app/chamados/juizes.py:1070`, `app/chamados/juizes.py:1102`, `app/ponto/services/fechamento.py:1408`, `app/ponto/services/fechamento.py:1437`)
   - Não achei escritor de tranca/toque dentro da nova cura S143; ela reabre chamado e ressuscita perguntas. O contexto da consulta restaura o anterior mesmo com exceção. (`app/chamados/services/disputa_emissao.py:1849`, `app/chamados/models.py:1442`, `app/chamados/models.py:1451`, `app/chamados/juizes.py:1109`)
   - Julgamento aninhado restaura colaborador, mas força `_tl.ativo=False`, deixando o julgamento exterior sem fonte após o interior. A atribuição do ativo já existia no trecho anterior do diff; **não achei caminho produtivo aninhado**, portanto não atribuo regressão nova comprovada. (`app/ponto/services/cartorio.py:1097`, `app/ponto/services/cartorio.py:1098`, `app/ponto/services/cartorio.py:97`; alcance produtivo: [nao verificado])
   - S143 roda fora de `julgar_colab`: receiver e chamadores avulsos passam pelo wrapper. Fora do julgamento, a fonte retorna `None` e não fornece colaborador errado; abre consulta sem memo inicial. (`app/chamados/signals.py:115`, `app/chamados/services/disputa_emissao.py:1863`, `app/ponto/services/cartorio.py:97`, `app/chamados/services/disputa_emissao.py:1918`)

5. **Juiz e escritor**

   **Não achei segundo juiz nessas duas funções.** `tranca_da_consulta` alimenta o C20 por `dias_trancados`; `colaborador_deste_julgamento` apenas fornece contexto. C6/C20 permanecem nas autoridades registradas. (`app/chamados/juizes.py:1107`, `app/ponto/services/cartorio.py:90`, `app/core/juizes.py:278`, `app/core/juizes.py:292`)

   **Não achei novo escritor direto de Pergunta/Chamado/Célula nos hunks de produção.** A cura chama `reabrir`, carimbo continua em `PerguntaDisputa.carimbar`, decisões em `registrar`, e fábrica/materialização permanecem portas. O problema encontrado é alcance não observado dessas portas e callbacks. (`app/chamados/services/disputa_emissao.py:1849`, `app/chamados/services/disputa_emissao.py:1980`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:143`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:147`)

6. **Travas, ordem, reconferência e reversão**

   A ordem inicial é C de todo o lote por PK, depois disputas abertas, pais e chamados vivos por colaborador; portas posteriores adquirem suas perguntas/disputas. Há reconferência de K6 e do plano sob trava, foto antes/depois e rollback antes da confirmação do savepoint. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:186`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:189`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:1562`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:283`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:288`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:306`)

   **Universo inicialmente travado não equivale literalmen

[truncado em 20000 de 25121 caracteres]

## 11/11 07:35 · sessao 9f734a1d238b · FIO-E0-V4-E3 · construtor · origem=Code

Discordo da solução proposta e **não apliquei alteração**, conforme sua instrução: `forcar=True` vence a guarda, mas o ramo NB retorna sem preencher `cods`; portanto, o primeiro acesso continuaria sujeito a `KeyError`. A escrita de `cods` fica no ramo normal (`ponto/services/cartorio.py:1228`, `:1282`, `:1441`; `ponto/tests/test_x1_folga_sem_adesao.py:132`).

O signal realmente chama o julgamento na criação da ausência. O receiver de `:275` é consumidor posterior; quem julga é o de `:143`, via `_julgar_dias` (`ponto/signals.py:93`, `:143`, `:275`).

**Diff vazio**; C2b e suas afirmações permanecem intactos (`ponto/tests/test_x1_folga_sem_adesao.py:131`).

Comandos executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria -v
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo green
```

Ambos: **rc=0, 19 testes** (`ponto/tests/test_e3_nb_impressao_memoria.py:88`; `ponto/tests/x1_mutantes.py:65`).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante literal_incondicional
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante sem_fonte_no_gancho
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante so_acusador
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante parcial_tira_marcador
```

Cada um: **rc=1, FAIL=1, ERROR=0**. Assertions atingidas, respectivamente: `ponto/tests/test_e3_nb_impressao_memoria.py:262`, `:264`, `:278`, `:318`. Isso comprova o mutante parcial somente em memória; resultado ORM: **[nao verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ruff check --no-cache ponto/tests/test_x1_folga_sem_adesao.py ponto/tests/test_e3_nb_impressao_memoria.py ponto/tests/x1_mutantes.py
```

Ruff: rc=1, `No module named ruff`; Ruff limpo: **[nao verificado]**.

A cura exclusivamente no teste exige rever a primeira afirmação que depende de `out['cods']`, preservando a verificação do veredito por outra testemunha; isso ultrapassa a troca autorizada e a exigência de manter essa afirmação (`ponto/tests/test_x1_folga_sem_adesao.py:132`).

## 11/11 07:33 · sessao 010796d81078 · - · - · origem=Ronald (tmux)

Achados do backfill/foto, por severidade:

1. **Alta — relógio da leitura vira falsa escrita e bloqueia cura.** `fechamento_lido` usa `recalcular_fechamento_mes(... somente_leitura=True, retornar_objeto=True)`; a conta atribui `previsto_em = timezone.now()` antes do ramo de leitura devolver o objeto. A foto inclui esse campo transitório em `campos`, e `mudancas` compara todos os campos. Logo duas leituras sem alteração monetária produzem delta. Como a foto percorre competências de todo o histórico, um `dinheiro_online:10/2026` pode cancelar um ato da 11 por `fora_da_competencia_alvo`; se for exportada, também entra na cerca. Fontes: `app/ponto/services/fechamento.py:1086`, `:1128`, `:1135`, `:1759`; `app/chamados/services/foto_e4b.py:15`, `:108`, `:142`, `:160`, `:190`, `:194`, `:32`.
   
   Prova determinística sem Django/arquivos realizada com corpos reais extraídos por AST: antes/depois com horas_falta=8 e DSR=1 idênticos, só `previsto_em` t versus t+1μs, retornou `campos_alterados=['previsto_em']` e `cerca(...,'11/2026')=[('dinheiro_online:10/2026','fora_da_competencia_alvo')]`. Integração ORM com relógio solto: [nao verificado]. Os novos ensaios ORM congelam o relógio: `app/chamados/tests/test_e4b_c4_orm.py:24`, `:125`.

2. **Alta — o estado final de APPLY pode nascer depois da foto/cerca/DIFF.** O backfill materializa a disputa inteira; ao quitá-la fecha a disputa. O signal agenda `retomar_regularizacoes_confirmadas` em `on_commit`, que percorre todas as regularizações confirmadas do colaborador, sem filtro pela competência alvo, podendo criar/corrigir batidas nas datas dos itens e marcar regularizações materializadas. Isso roda depois de foto, cerca e DIFF final; DRY descarta os callbacks. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:145`, `:147`, `:297`, `:308`, `:314`, `:319`; `app/chamados/services/materializacao.py:1375`, `:1378`, `:1333`; `app/chamados/signals.py:129`, `:132`, `:137`, `:144`; `app/chamados/services/regularizacao_ext.py:218`, `:221`, `:224`, `:166`, `:188`, `:197`.
   
   Há ainda fechamento monetário agendado pela criação de batida, depois da foto do gravado: `app/ponto/registro_batida.py:157`, `:160`, `:175`; `app/ponto/services/fechamento.py:1807`, `:1810`, `:1824`. O dinheiro online tenta antecipar o cálculo, mas não observa essas futuras escritas reais nem seu resultado: `app/chamados/services/foto_e4b.py:188`, `:192`.

   A ordem `with transaction.atomic(), push_disputa_silenciado(...)` também restaura o silêncio antes de sair do atomic e disparar seus callbacks; previsão/silêncio desses efeitos posteriores não ficam cobertos pelo bloco. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:263`; `app/chamados/services/disputa_emissao.py:1749`, `:1753`. Push concreto nessa cascata: [nao verificado].

3. **Alta — “não entregue” substitui “aberta”, e comp10 não tem proteção própria.** Executáveis dependem de `competencia_do_dia_entregue(...) is False`; a cerca aceita a mesma competência alvo com `entregue=False`. Não achei guarda especial de 10/2026 nem exigência de `!`, nem pergunta a `periodo_trancado`/`competencia_fechada_para_celula` no comando/cerca. Assim `--competencia 10/2026 --aplicar` é aceito se K6 falso, exatamente a hipótese que o aval diz não bastar. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:210`, `:213`, `:240`, `:244`, `:246`, `:283`; `app/chamados/services/foto_e4b.py:30`, `:32`, `:34`; K6 só responde existência de TXT vigente: `app/folha/export.py:556`, `:580`, `:586`. O recibo admite que `!` ficou condição operacional externa: `app/docs/FIO-E0-V4-E4B-c4.txt:60`, `:65`, `:190`.

4. **Média/alta — foto/reversão “integral” omite escritas alcançadas.** Não captura `EventoCelula`, embora ressuscitar perguntas chame `acordar_celulas` e o save de pergunta também emita evento; portanto novas linhas da outbox não aparecem no delta/nascimentos/reversão. Fontes: enumeração da foto `app/chamados/services/foto_e4b.py:142` a `:187`; emissão `app/chamados/models.py:1566`, `:1568`, `:3413`; `app/chamados/signals.py:251`, `:257`; escritor `app/ponto/portas/celula.py:887`, `:896`; campos e consumidor `app/escala/models.py:1845`, `:1848`, `:1857`.
   
   Fábrica pode escrever pauta, cuja porta também cria/atualiza `ContadorPauta`; foto captura Pauta mas não contador. Auditoria da foto só inclui seis modelos, excluindo inclusive modelo Pauta. Fontes: `app/chamados/services/fabrica_da_celula.py:204`, `:214`, `:215`, `:354`, `:369`; `app/pautas/services.py:180`, `:186`, `:188`, `:351`, `:352`, `:354`; `app/chamados/services/foto_e4b.py:163`, `:173`, `:175`, `:186`. Regularizações e itens do achado 2 também não estão na enumeração da foto: `app/chamados/services/foto_e4b.py:84`, `:90`, `:142`.

5. **Média — rechecagem K6 não serializa a entrega.** Trava cobre C/D/Ch/P, mas o escritor `ExportacaoDominio.registrar` usa `exists/create` sem essa trava; `invalidar` também não trava C. A entrega pode ocorrer depois da última leitura K6 da foto posterior e antes do commit. Fontes: `app/chamados/models.py:2665`, `:2675`, `:2688`, `:2692`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:283`, `:297`, `:308`; `app/folha/export.py:580`; `app/folha/models.py:151`, `:162`, `:174`, `:178`, `:182`. Corrida efetivamente reproduzida: [nao verificado]; recibo reconhece a pendência em `app/docs/FIO-E0-V4-E4B-c4.txt:190`.

Aspectos corretos demonstráveis por leitura:

- Ordem inicial C por PK de todo lote, depois universo aberto por colaborador; protocolo trava disputas abertas, pais delas e chamados vivos. Portas adicionais travam perguntas e disputas fechadas ao ressuscitar. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:183`, `:186`, `:187`, `:189`; `app/chamados/models.py:2640`, `:2675`, `:2688`, `:1562`. Universo inicial não é literalmente todas as linhas da foto nem todos os terminais/fechados que a porta pode escrever: mesmas fontes.
- Foto nasce sob atomic/C, após refresh de colaborador; reversão grava e relê JSON antes do primeiro ato. Reversão é fotografia para compensação, não achei executor de reversão neste comando. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:56`, `:59`, `:61`, `:63`, `:263`, `:264`, `:266`, `:267`, `:270`, `:275`, `:277`, `:295`.
- Reconfere entrega e plano sob trava; limbo/exportada abortam savepoint; cerca usa ambos lados do delta, recusando fora do alvo/indeterminado/exportado. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:283`, `:288`, `:290`, `:292`, `:302`, `:306`; `app/chamados/services/foto_e4b.py:22`, `:30`, `:32`, `:34`.
- DIFF por colaborador contém nascimentos por identidade, tentativas de push e campos numéricos de DiaPago, fechamento gravado e online; não refaz soma monetária. Fontes: `app/chamados/services/foto_e4b.py:39`, `:50`, `:60`, `:68`, `:71`, `:72`, `:192`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:317`, `:351`, `:355`.
- Não achei mecanismo de comparar esse DIFF com expectativa de dinheiro, publicar previamente no RELATO ou exigir essa publicação antes de APPLY; execução operacional dessas condições: [nao verificado]. O DIFF impresso no mesmo APPLY vem depois da saída do atomic: `app/chamados/management/commands/backfill_fio_morto_e4b.py:263`, `:320`, `:351`, `:355`.
- Não introduziu exclusão por col950/col969/CADASTRO no comando; universo vem do vigia e lista complementar, e antes/depois continuam medidos pelo mesmo vigia. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `:78`, `:153`, `:222`, `:223`, `:318`, `:365`.
- Provas de idempotência cobrem fixture específica de fábrica e classe e com mesma nota/âncora; não certificam todos os efeitos posteriores ou tabelas omitidas. Fontes: `app/chamados/tests/test_e4_backfill.py:141`, `:145`, `:154`, `:157`; `app/chamados/tests/test_e4_passivo.py:61`, `:75`, `:78`, `:80`.

Mapa do formato novo:

- `foto[chave].campos`: escritor é leitura dos campos concretos do ORM (`foto_e4b.py:124`), leitor `mudancas` (`:15`), `nascimentos` (`:44`), `relato` (`:69`), JSON reversão (`backfill:273`).
- `foto[chave].alcance[{dia,comp,entregue}]`: escritor `alcance/mensal` (`foto_e4b.py:102`, `:108`, `:111`, `:115`, `:118`); leitores cerca (`:26`, `:29`), relato (`:78`), BANG-LINHA (`backfill:345`, `:346`).
- `delta`: escritor `mudancas` (`foto_e4b.py:12`), leitores cerca (`:21`), nascimentos (`:43`), relato (`:73`) e backfill (`:300`, `:345`).
- `previstos`: alimentado por `_enviar_push_disputa_colab` sob contexto (`disputa_emissao.py:1760`, `:1763`); backfill corta tentativa desfeita (`backfill:305`, `:311`), filtra por colaborador (`:317`) e publica via relato (`foto_e4b.py:72`).
- Datas mantêm autoridades: batida pelo dia do turno (`foto_e4b.py:154`), ausência por `dias_da_ausencia` (`:158`), competência pelo fim da janela (`:108`, `:109`), instante de reversão pelo `agora` da borda (`backfill:215`, `:271`). O campo com dois sentidos encontrado é `previsto_em` do objeto online tratado como mudança persistida, achado 1.

## 11/11 07:33 · sessao 64dd4b872ec3 · - · - · origem=Ronald (tmux)

Mapa do memo/thread-local e conclusões:

- `_tl.colaborador` tem escritor em `app/ponto/services/cartorio.py:949`, restauração em `:1097` e leitor único produtivo `colaborador_deste_julgamento` em `:96-97`; S143 consome esse leitor em `app/chamados/services/disputa_emissao.py:1917`. O significado é instância do colaborador do ato; a igualdade de PK impede usar outro colaborador (`cartorio.py:97`).
- `_tl.ativo` também é escrito por `calado` (`cartorio.py:105-110`). Portanto “em julgamento” abrange supressão de signals; não significa necessariamente execução de `julgar_colab` (`cartorio.py:101-104`). Em execução normal, `julgar_colab` restaura o colaborador anterior, geralmente `None` (`cartorio.py:946-949`, `:1097`).
- `_memo_da_tranca` escolhe, nesta ordem: consulta ativa, lote quando permitido, request ou instância (`app/chamados/juizes.py:1060-1070`). Assim `lote=False` não ignora consulta ativa, e “memo do colaborador” pode efetivamente ser memo do request/consulta (`:1060-1069`).
- Empresa é escrita no memo por `dias_trancados` (`juizes.py:1015-1023`); tranca por empresa/competência é escrita e lida em `:1025-1032`; usuário sistema e toque positivo são escritos em `:1181-1190`, lidos em `:1173-1187`. Toque usa PK do colaborador/data/instante na chave; não achei colisão entre colaboradores (`:1174`).
- O `partial` recebe referência ao memo (`disputa_emissao.py:1919-1921`); a cópia efetiva acontece ao entrar no contexto (`juizes.py:1101-1104`). É cópia rasa: valores Empresa/PeriodoFechado continuam sendo referências aos objetos (`juizes.py:1018-1029`, `:1102`).
- `tranca_da_consulta` calcula pelo juiz existente `dias_trancados`, guarda resultado por identidade da pergunta e restaura o contexto com token no `finally` (`juizes.py:1106-1110`, `:948-954`). Não achei segundo juiz: C6/C20 estão registrados em `app/core/juizes.py:278`, `:292`; a função apenas alimenta C20 (`juizes.py:1107`). `colaborador_deste_julgamento` fornece metadado, não julgamento de domínio (`cartorio.py:90-97`).
- Fora de julgamento ou para outra PK, S143 recebe `None` e abre consulta fria; não lê colaborador errado por esse caminho (`cartorio.py:97`, `disputa_emissao.py:1917-1921`, `juizes.py:1097`). S143 também roda por receiver e por chamadores avulsos (`app/chamados/signals.py:111-115`, `disputa_emissao.py:1863-1879`).
- Não achei escritor de tranca/toque dentro da nova cura S143: ela chama `Chamado.reabrir`, que altera estado e ressuscita perguntas (`disputa_emissao.py:1849`, `app/chamados/models.py:1442`, `:1451`). Mudança concorrente da tranca continua possível pelo código: os escritores salvam `PeriodoFechado` antes de eventual trabalho de fila, sem trava C nessa escrita (`app/ponto/services/fechamento.py:1408-1428`, `:1437-1448`).
- Risco verificável de foto velha: uma entrada `('tranca', empresa, mês, ano): None` copiada evita consultar novamente `periodo_trancado`, mesmo se este já mudou (`juizes.py:1025-1030`, `:1102`). Probe somente em memória confirmou snapshot `False/0 consultas` contra consulta fresca `True/1 consulta`; ocorrência em produção [nao verificado]. Não achei invalidação produtiva de `_tranca_memo`; seu armazenamento é `juizes.py:1070`.
- Aninhamento de `julgar_colab`: restaura colaborador, mas força `_tl.ativo=False`; o julgamento exterior deixa de fornecer colaborador após o interior (`cartorio.py:946-949`, `:1097-1098`, `:97`). Esse defeito do ativo já aparece no trecho anterior do diff; caminho produtivo aninhado não achei. O teste novo exercita exceção, não julgamento aninhado (`app/chamados/tests/test_e4b_c4_tranca_memoria.py:119-144`).
- Semântica suspeita fora do memo: conversa escreve dia civil (`models.py:2794-2802`), e o juiz de dia do turno retorna `None` para ela (`juizes.py:332-336`); a nova foto usa `p.data_evento` indistintamente (`app/chamados/services/foto_e4b.py:128`). `_ancoradas` não exclui conversa (`models.py:344-346`), e a cerca da cura também usa data crua (`disputa_emissao.py:1835-1836`). Há atribuição de competência de fato a campo que também representa dia civil; impacto produtivo específico [nao verificado].
- Executei os cinco testes de memória: OK; os seis mutantes catalogados morreram por AssertionError, sem errors. Fontes dos alvos e enxertos: `test_e4b_c4_tranca_memoria.py:59-144`, `:147-166`. Mutação adicional `memo.update(memo_inicial) → pass` também morreu no assert `:81`; portanto “memo_inicial ignorado”, “memo não copiado” e “colaborador não restaurado” têm mordida de memória (`:81`, `:91-101`, `:143`, `:154-159`). Integração ORM dessas propriedades [nao verificado]. [nao verificado]

## 11/11 07:32 · sessao a5f07e75a644 · - · - · origem=Ronald (tmux)

- **Não dá para afirmar “cada RED morde a base”.** `--red-base` carrega somente `disputa_emissao.py` de b20 e executa somente D3/evento; RED da tranca usa outra origem (`ca42...`); a sonda de queries usa MAIN `fb926...`, conservando os demais arquivos HEAD. (`app/chamados/tests/e4b_c4_verificar.py:141`, `app/chamados/tests/test_e4b_c4_tranca_memoria.py:22`, `app/chamados/tests/test_e4b_c4_tranca_memoria.py:175`, `app/chamados/tests/e4b_c4_base.py:9`, `app/chamados/tests/e4b_c4_medir_queries.py:3`).

- **Os três mutantes óbvios citados são pegos.** Executei os seis mutantes de tranca e um extra `memo.update(memo_inicial) -> pass`: todos morreram com uma assertion, zero errors. `sem_copia` acusa aquecimento da fonte; `sem_restaurar_colab` acusa falta de restauração; ignorar `memo_inicial` acusa leitura avulsa extra. As assertions estão em `app/chamados/tests/test_e4b_c4_tranca_memoria.py:59`, `:83`, `:120`; catálogo em `:148`. Resultado da execução desta sessão, sem artefato persistido: [nao verificado].

- **Lacuna de aninhamento:** o teste de `julgar_colab` começa com `_tl.ativo=False`; verifica colaborador externo restaurado e leitura fora do ato, mas não entra em julgamento externo ativo nem prova que o retorno de julgamento interno preserva esse estado ativo. (`app/chamados/tests/test_e4b_c4_tranca_memoria.py:32`, `:120`, `:145`). Também não achei teste de alteração da fonte do memo no meio do ato; [nao verificado].

- **Teto global mede caminho real:** registra batida, exige 1/3 chamados fechados e captura SQL do registro real; não é proxy. O teste da tranca compara o mesmo caminho real HEAD × avulso, com rollback, métrica de `periodo_trancado`, SQL total e `on_commit`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:406`, `:428`, `:435`, `app/chamados/tests/test_e4b_c4_orm.py:61`, `:105`).

- **RED do mapa é isolamento de componente:** explicitamente substitui `tranca_da_consulta` por `nullcontext`; prova economia do mapa nesse cenário modificado, não economia adicional do mapa com a tranca HEAD ativa. (`app/chamados/tests/test_e4b_c4_orm.py:259`, `:265`, `:297`, `:307`).

- **O anexo informado não prova morte de `tranca_avulsa`.** O executor atual a exige depois dos cinco mutantes ORM e do mapa; a ausência desse resultado não pode ser preenchida por `rc=0` de protocolo simulado. (`app/chamados/tests/e4b_c4_verificar.py:106`, `app/chamados/tests/test_e4b_c4_orm_protocolo.py:73`; anexo `c4v6_orm_resumo.txt`: [nao verificado]).

- **Foto/cerca ORM é adversarial declarado:** o hook chama a porta real de ressuscitação depois da fábrica; não certifica cascata natural completa. O teste exige que o hook realmente executou e que P90/D90 ressuscitaram antes do rollback. (`app/chamados/tests/test_e4b_c4_orm.py:3`, `:226`, `:237`).

- **Relógio:** não achei teste novo solto no escopo verificado. Classes ORM congeladas e B2d agora congelado inteiro; selo estático distingue relógio sem argumento e conversão `localtime(x)`. (`app/chamados/tests/test_e4b_c4_orm.py:24`, `:125`, `app/chamados/tests/test_b2d_resposta_visivel.py:25`, `app/core/tests/test_selo_teste_sem_relogio.py:32`, `:76`).

- Executei **71 testes de memória OK** e **40 selos OK**, com `PYTHONDONTWRITEBYTECODE=1`; nenhum ORM/banco foi executado. Resultado sem arquivo de saída persistido: [nao verificado]. O executor ORM é subconjunto explícito, portanto nem ele nem os 130 informados equivalem à suíte inteira. (`app/chamados/tests/e4b_c4_verificar.py:74`).

- Recibos não são prova atual: o primeiro declara ausência de certificação ORM; o v2 ainda fala de números/tetos e fixtures de voltas anteriores. (`app/docs/FIO-E0-V4-E4B-c4.txt:3`, `app/docs/FIO-E0-V4-E4B-c4-v2.txt:90`, `:183`).

## 11/11 07:27 · sessao 3b06430316b1 · FIO-E0-V4-E3 · construtor · origem=Code

Diff entregue:

- [Teste ORM C2b](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_x1_folga_sem_adesao.py:115): exige parcial real na grade, precedência, marcador e rejulgamento sem forçar após batida distante. [nao verificado]
- [Caso sem banco](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:309): assertion da impressão mata o novo mutante.
- [Executor](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:36): mutante registrado; contagens green ajustadas para 19 em memória e 12 ORM.

Comandos executados e RC ([critério do executor](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:62)):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo green
# rc=0; 19 testes, FAIL=0, ERROR=0 [nao verificado]

PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante parcial_tira_marcador
# rc=1; FAIL=1, ERROR=0; assertion da impressão

PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante literal_incondicional
# rc=1; FAIL=1, ERROR=0

PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante sem_fonte_no_gancho
# rc=1; FAIL=1, ERROR=0

PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante so_acusador
# rc=1; FAIL=1, ERROR=0
```

Ruff nos três arquivos: rc=0, `All checks passed!`:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache ponto/tests/test_x1_folga_sem_adesao.py ponto/tests/test_e3_nb_impressao_memoria.py ponto/tests/x1_mutantes.py
```

[Catraca das linhas registradas](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/core/tests/test_medida_turno_do_par.py:539): rc=0 pelo executor em memória documentado no arquivo; registro mantido.

ORM green e morte do mutante pelo teste ORM: **[nao verificado]**, reservados à sessão com banco isolado: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --modo green
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --modo mutante --mutante parcial_tira_marcador
```

## 11/11 07:25 · sessao f637d95c917c · TRAVA-SELO · verificador · origem=Code

**O fechamento integral NÃO PROCEDE.** Os escapes anteriores por caminhos/globs e por `--parallel '1 .'` foram tratados, mas permanecem lacunas nos selos, na documentação e na cobertura dos chamadores. A própria guarda declara não reconhecer uma enumeração de módulos equivalente à suíte inteira. (`bin/suite.sh:52`, `bin/suite.sh:67`, `bin/suite.sh:95`, `bin/suite.sh:131`; `bin/tests/test_suite_motivo.sh:66`; `CLAUDE.md:100`; `bin/regua.sh:165`)

1. **NÃO PROCEDE como fechamento de todas as classes; PROCEDE para os escapes sintáticos anteriores.**

   | Classe/entrada | Conferência na cópia |
   |---|---|
   | `--only '.'`, `/app`, pastas, glob, segmento vazio, app fora de LABELS | A forma ancorada e a pertinência a LABELS recusam essas palavras antes do custo. (`bin/suite.sh:95`, `bin/suite.sh:96`, `bin/suite.sh:98`) |
   | Inválido após label válida: `--only 'api .'` | Todas as palavras passam pela guarda; não valida apenas a primeira. (`bin/suite.sh:97`, `bin/suite.sh:98`) |
   | Glob produzido por `--volta` | Não expande no cwd: `set -f` antecede o filtro. Uma saída sem nenhuma label aproveitável termina com rc 1. (`bin/suite.sh:67`, `bin/suite.sh:84`, `bin/suite.sh:88`) |
   | `--only api -- .`, `/app`, outro app | A lista fechada recusa posicionais depois de `--`. (`bin/suite.sh:105`, `bin/suite.sh:111`, `bin/suite.sh:114`, `bin/suite.sh:118`) |
   | `--parallel '1 .'`; `--espera x` | Recusa rc 2 no parser. Paralelismo aceita inteiro positivo ou `auto`; espera aceita dígitos, incluindo zero. (`bin/suite.sh:52`, `bin/suite.sh:54`) |
   | `-- -v 2`, `--tag x`, `--tag core`, `--exclude-tag=x`, `-k padrao`; `--parallel auto` | Passam pelas guardas com recorte parcial; a coincidência do valor com um app deixou de causar recusa. (`bin/suite.sh:52`, `bin/suite.sh:112`, `bin/suite.sh:113`) |
   | `-- --failfast=x` | Recusado corretamente: a forma `--x=v` consulta apenas `_FLAG_VALOR`. (`bin/suite.sh:113`, `bin/suite.sh:114`) |
   | Todas as LABELS, inclusive repetidas ou como `<app>.tests`, em `--only` ou saída de `--volta` | A guarda exige motivo pela seleção efetiva. (`bin/suite.sh:80`, `bin/suite.sh:136`, `bin/suite.sh:139`, `bin/suite.sh:143`) |

   **Limites remanescentes:**

   - Uma lista de módulos que esgote a inteira continua fora da classificação. Isso também vale para módulos provenientes de `--volta`, pois ambos alimentam `ONLY`. Qual lista concreta esgota os testes desta cópia: **[não verificado]**. (`bin/suite.sh:80`, `bin/suite.sh:131`, `bin/suite.sh:139`)
   - `--only ''` ainda termina pelo `${2:?}` — observei rc 1 na chamada seca — sem a recusa falante rc 2. (`bin/suite.sh:51`)
   - A lista de flags usa busca por substring: um único argumento `'--failfast --reverse'` pode casar com `_FLAG_SO`. Ele permanece **um argumento** no executor; ampliação efetiva por essa forma: **[não verificado]**. (`bin/suite.sh:105`, `bin/suite.sh:111`, `bin/suite.sh:200`)
   - `-v2` e `-kpadrao` são recusados pela lista. Que sejam formas exigíveis no runner instalado, para classificá-las como recusas indevidas: **[não verificado]**. (`bin/suite.sh:111`, `bin/suite.sh:113`, `bin/suite.sh:114`)

   Quanto aos demais achados anteriores: `-z --no-renames`, import entre parênteses e relativo em subpacote foram implementados; URL, signal, transitividade e reexportação foram **declarados como limites**, sem implementação de seguimento. (`bin/vizinhos_da_volta.py:13`, `bin/vizinhos_da_volta.py:14`, `bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:16`, `bin/vizinhos_da_volta.py:27`, `bin/vizinhos_da_volta.py:55`, `bin/vizinhos_da_volta.py:70`)

   **Ainda há dependência direta omitida fora do limite declarado:** para `core/sub/foo.py`, as formas Python válidas `import core . sub . foo` e `from core . sub import foo` não casam com os padrões atuais. Conferi a sintaxe e o não casamento em memória; ocorrência desses imports nesta obra: **[não verificado]**. (`bin/vizinhos_da_volta.py:64`, `bin/vizinhos_da_volta.py:66`, `bin/vizinhos_da_volta.py:70`)

2. **PROCEDE para anteceder splitting/glob de seleção; NÃO PROCEDE literalmente para “toda expansão”.**

   Mapa dos campos de `suite.sh`, incluindo os demais controles tocados pelo delta: [nao verificado]

   | Campo | QUEM ESCREVE | QUEM LÊ |
   |---|---|---|
   | `ONLY` | Inicialização, parser, helper e seleção filtrada: `bin/suite.sh:47`, `:51`, `:80`, `:87` | Conflito, filtro, vazio, validação, classificação, seco e executor: `bin/suite.sh:78`, `:84`, `:88`, `:97`, `:136`, `:155`, `:199` |
   | `EXTRA` | Inicialização e captura citada após `--`: `bin/suite.sh:47`, `:59` | `_extra_ok` e executor: `bin/suite.sh:109`, `:110`, `:112`, `:113`, `:200` |
   | `MOTIVO` | `bin/suite.sh:47`, `:56` | Guarda, diagnóstico e seco: `bin/suite.sh:143`, `:145`, `:154` |
   | `VOLTA` | `bin/suite.sh:47`, `:57` | Ativação, helper, diagnóstico e seco: `bin/suite.sh:77`, `:80`, `:81`, `:154` |
   | `LABELS` | Fonte literal: `bin/regua.sh:23`; derivação: `bin/suite.sh:72` | Filtro, pertinência, classificação e fallback: `bin/suite.sh:85`, `:96`, `:138`, `:139`, `:199` |
   | `_FORMA_LABEL` | `bin/suite.sh:95` | `_eh_label`, chamado na guarda: `bin/suite.sh:96`, `:98` |
   | `_FLAG_SO` | `bin/suite.sh:105` | Pertinência e diagnóstico: `bin/suite.sh:111`, `:114` |
   | `_FLAG_VALOR` | `bin/suite.sh:106` | Consumo, forma com `=` e diagnóstico: `bin/suite.sh:112`, `:113`, `:114` |
   | `PARALELO` | Inicialização e parser: `bin/suite.sh:47`, `:53`; validação em `:52` | Executor: `bin/suite.sh:200` |
   | `ESPERA` | Inicialização e parser: `bin/suite.sh:47`, `:55`; validação em `:54` | Chamada da trava: `bin/suite.sh:193`; parser/flock: `bin/trava_teste.sh:27`, `:36` |
   | `DIR` | `bin/suite.sh:47`, `:50` | Helper, seco e montagem: `bin/suite.sh:79`, `:154`, `:169`, `:170`, `:171`, `:174` |
   | `SECO` | `bin/suite.sh:47`, `:58` | Desvio seco: `bin/suite.sh:153` |

   Antes de `set -f; LC_ALL=C`, há expansões no parser e no subshell que calcula `RAIZ`; elas usam aspas, atribuições ou `[[ ]]`, sem splitting/glob das palavras de seleção. (`bin/suite.sh:35`, `bin/suite.sh:49`, `bin/suite.sh:51`, `bin/suite.sh:52`, `bin/suite.sh:59`, `bin/suite.sh:67`)

   O `source recursos.sh` também antecede a proteção, mas nesta cópia só faz atribuições/exports e define funções; o guard de arquivo executado impede chamar as funções operacionais durante o source. (`bin/suite.sh:39`; `bin/recursos.sh:40`, `bin/recursos.sh:81`, `bin/recursos.sh:105`, `bin/recursos.sh:140`, `bin/recursos.sh:149`, `bin/recursos.sh:178`, `bin/recursos.sh:191`)

   As expansões não citadas de `ONLY` começam depois da proteção. No executor, `${ONLY:-$LABELS}` e `$PARALELO` continuam sem aspas; `EXTRA` permanece array citado. A trava recebe o argv já expandido e executa `"$@"`. (`bin/suite.sh:67`, `bin/suite.sh:84`, `bin/suite.sh:97`, `bin/suite.sh:199`, `bin/suite.sh:200`; `bin/trava_teste.sh:42`)

   **O seco não mostra o comando completo:** passa pelas guardas e imprime motivo, volta, diretório e labels; omite EXTRA, paralelismo e espera. (`bin/suite.sh:118`, `bin/suite.sh:143`, `bin/suite.sh:153`, `bin/suite.sh:154`, `bin/suite.sh:155`, `bin/suite.sh:156`)

3. **PROCEDE que a flag consome label/caminho; NÃO SEI sobre ampliação efetiva no runner instalado.**

   `-- -k .` e `-- --tag /app` são aceitos porque o ramo exige apenas existir a próxima palavra. Esses valores não passam pela guarda de posicionais. (`bin/suite.sh:112`)

   Isso, sozinho, **não demonstra fuga**: o shell entrega `-k` e `.` como elementos distintos, preservados, e não reapresenta `.` como label por expansão. A descoberta efetiva é delegada ao `DiscoverRunner` externo; quantidade executada nesses exemplos: **[não verificado]**. (`bin/suite.sh:200`; `app/core/runner_quarentena.py:15`, `app/core/runner_quarentena.py:72`, `app/core/runner_quarentena.py:76`)

4. **NÃO PROCEDE que os selos mordam cada classe pela razão certa.**

   Há melhoria: caminhos/globs de `ONLY` exigem o diagnóstico específico; existem casos mistos, cwd preparado, âncora de motivo e rc 75. (`bin/tests/test_suite_motivo.sh:57`, `bin/tests/test_suite_motivo.sh:58`, `bin/tests/test_suite_motivo.sh:62`, `bin/tests/test_suite_motivo.sh:64`, `bin/tests/test_suite_motivo.sh:75`, `bin/tests/test_suite_motivo.sh:76`)

   Persistem estes sobreviventes plausíveis:

   | Mutante/lacuna | Por que o selo pode continuar verde | [nao verificado]
   |---|---|
   | Acrescentar `--pattern` a `_FLAG_SO` | O negativo testa `--pattern y`: passa a primeira palavra e recusa `y`, mantendo rc 2 e o diagnóstico genérico procurado. (`bin/tests/test_suite_motivo.sh:66`, `:67`; `bin/suite.sh:111`, `:114`) |
   | Tratar `-k` como flag simples, acrescentando-o a `_FLAG_SO` | O positivo não usa `-k`; cobre `-v`, `--tag`, `--exclude-tag=x` e `--failfast`. (`bin/suite.sh:111`, `:112`; `bin/tests/test_suite_motivo.sh:70`) |
   | Aceitar `--x=v` também para `_FLAG_SO` | Falta negativo como `--failfast=x`; `--exclude-tag=x` continua passando. (`bin/suite.sh:113`; `bin/tests/test_suite_motivo.sh:66`, `:70`, `:74`) |
   | Executor descartar EXTRA ou ignorar `--parallel auto` | Os positivos cobram chegada à trava, sem conferir esses argumentos; `_args` extrai apenas labels, e a trava falsa registra `$*`, perdendo fronteiras. (`bin/tests/test_suite_motivo.sh:19`, `:24`, `:70`, `:73`) |

   Os ramos dos três primeiros mutantes foram conferidos em memória; sobrevivência à execução **integral** do selo: **[não verificado]**. Os selos completos escrevem e removem arquivos temporários. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:27`)

   O caso “todas as LABELS depois de `--`” também recusa primeiro em EXTRA; não prova a classificação de inteira pela razão anunciada. (`bin/tests/test_suite_motivo.sh:43`; `bin/suite.sh:118`, `bin/suite.sh:143`)

   Em `test_suite_sh.sh`, a fixture nua agora é negativa, mas o juiz continua sendo dois greps. Ele aceita, por exemplo, `bash …/bin/suite.sh # --motivo "pre-opus X"` ou motivo sem obra; a guarda real recusaria ambos. (`bin/tests/test_suite_sh.sh:34`, `bin/tests/test_suite_sh.sh:35`, `bin/tests/test_suite_sh.sh:80`; `bin/suite.sh:133`, `bin/suite.sh:143`)

   Os selos próprios do wrapper e da descoberta real de vizinhos continuam ausentes deste teste: ele executa a suite copiada e substitui o helper. (`bin/tests/test_suite_motivo.sh:20`, `bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:28`)

5. **NÃO PROCEDE como fidelidade integral da documentação.**

   O comando canônico ganhou motivo e a descrição de EXTRA deixou de ensinar repasse cru. Porém:

   - “Só número” contradiz `--parallel auto`. (`CLAUDE.md:96`, `CLAUDE.md:100`; `bin/suite.sh:52`)
   - A redação deve explicitar que `--only`/`--volta` cobrindo todas as LABELS também exigem motivo. (`CLAUDE.md:101`, `CLAUDE.md:102`; `bin/suite.sh:139`, `bin/suite.sh:143`)
   - “Volta do meio = `--volta`” omite ruff e disjuntor: quem os executa é `volta_intermediaria.sh`. (`CLAUDE.md:102`; `bin/volta_intermediaria.sh:23`, `bin/volta_intermediaria.sh:29`, `bin/volta_intermediaria.sh:32`)
   - A descrição de rc 75 corresponde à trava; a recusa de inteira é rc 2, mas não se pode generalizar esse rc a todo erro de argumento, como `--only ''`. (`CLAUDE.md:102`, `CLAUDE.md:103`; `bin/trava_teste.sh:36`, `bin/trava_teste.sh:38`; `bin/suite.sh:51`, `bin/suite.sh:150`)

   A busca em `app/docs`, `bin/` e `.claude/` encontrou **orientações vivas incompletas na própria ajuda**: `suite.sh --dir …`, `--parallel 2` e `--espera 1`, sem recorte/motivo, são recusadas se copiadas literalmente. A chamada nua do selo é fixture negativa; há também comandos antigos em relatos, cujo uso atual é **[não verificado]**. (`bin/suite.sh:29`, `bin/suite.sh:30`, `bin/suite.sh:31`, `bin/suite.sh:143`; `bin/tests/test_suite_sh.sh:80`, `bin/tests/test_suite_sh.sh:91`; `app/docs/RELATO.md:681`)

   Não encontrei outra orientação viva de EXTRA cru nessa busca; ausência fora dos arquivos pesquisados: **[não verificado]**.

6. **PROCEDE o erro de descarte de régua/pre-push; NÃO SEI quanto à obrigação de um selo próprio específico.** [nao verificado]

   A triagem restringe o aval a `bin/suite.sh`, mas o aval define a unidade como **“chamada de suite inteira pela trava”**. Régua e pre-push fazem precisamente essa chamada sem guarda de motivo; a trava apenas executa o comando recebido. Ser preexistente não constitui exceção expressa no aval fornecido. (`triagem_v5.txt:5`; `aval_trava_menos_fila_com_selo_1111.txt:1`; `bin/regua.sh:165`, `bin/regua.sh:168`; `bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/pre-push.sh:187`; `bin/trava_teste.sh:42`)

   Para o **selo próprio de `volta_intermediaria.sh`**, o aval não impõe literalmente esse formato de teste. O descarte para fila não está demonstrado errado apenas por essa exigência específica; a cobertura do fluxo continua pendente. (`aval_trava_menos_fila_com_selo_1111.txt:1`; `triagem_v5.txt:5`; `bin/tests/test_suite_motivo.sh:28`; `bin/volta_intermediaria.sh:37`, `bin/volta_intermediaria.sh:43`, `bin/volta_intermediaria.sh:44`)

   Para **vizinhos**, declarar limites e invocar a inteira pre-opus não demonstra fechamento das classes já conhecidas: o aval exige vizinhos e transformação de escapes em regras de classe. A defesa pre-opus está escrita no helper/triagem, sem exceção correspondente no aval dado. (`aval_trava_menos_fila_com_selo_1111.txt:1`; `triagem_v5.txt:5`; `bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:16`, `bin/vizinhos_da_volta.py:18`)

## 11/11 07:22 · sessao 559aedadd68b · - · - · origem=Ronald (tmux)

**Q1 — NÃO PROCEDE como fechamento completo.** As formas caminho/glob e a injeção por `--parallel '1 .'` estão barradas; continua declarado que a enumeração de módulos cobrindo a inteira não é reconhecida, inclusive quando vem de `--volta`. (`bin/suite.sh:52`, `bin/suite.sh:67`, `bin/suite.sh:84`, `bin/suite.sh:95`, `bin/suite.sh:98`, `bin/suite.sh:130`, `bin/suite.sh:131`)

A whitelist usa pertinência por substring: um único argumento `'--failfast --reverse'` é aceito por conter trecho contíguo da lista. Isso não demonstra ampliação: o executor preserva esse elemento inteiro. (`bin/suite.sh:105`, `bin/suite.sh:111`, `bin/suite.sh:200`) O comportamento efetivo desse argumento no Django é **[não verificado]**.

**Q2 — PROCEDE para antecedência às expansões sujeitas a splitting/glob; não literalmente a toda expansão.** O parser já expande argumentos antes de `set -f`, mas usa aspas, atribuições e `[[ ]]`. A primeira expansão não citada da seleção acontece depois da proteção. (`bin/suite.sh:49`, `bin/suite.sh:67`, `bin/suite.sh:84`, `bin/suite.sh:97`)

O `source` anterior não chama as funções que contêm expansões não citadas: o guard de execução distingue arquivo executado de arquivo sourceado. O subshell anterior calcula a raiz usando caminho citado. (`bin/suite.sh:35`, `bin/suite.sh:39`; `bin/recursos.sh:140`, `bin/recursos.sh:149`, `bin/recursos.sh:178`, `bin/recursos.sh:191`)

O comentário “LC_ALL sem export […] não para os filhos” não é universal: se `LC_ALL` já foi importada com atributo export, a atribuição preserva esse atributo; efeito sobre esta seleção: **[não verificado]**. (`bin/suite.sh:66`, `bin/suite.sh:67`)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| ONLY | `suite.sh:47,51,80,87` | `suite.sh:78,84,88,97,136,155,199` | [nao verificado]
| EXTRA | `suite.sh:47,59` | `suite.sh:109–113,200` | [nao verificado]
| MOTIVO | `suite.sh:47,56` | `suite.sh:143,145,154` | [nao verificado]
| VOLTA | `suite.sh:47,57` | `suite.sh:77,80–81,154` | [nao verificado]
| LABELS | Fonte `regua.sh:23`; derivação `suite.sh:72` | `suite.sh:85,96,138–139,199` | [nao verificado]
| _FORMA_LABEL | `suite.sh:95` | `_eh_label`, `suite.sh:96`, chamado em `suite.sh:98` | [nao verificado]
| _FLAG_SO | `suite.sh:105` | `suite.sh:111,114` | [nao verificado]
| _FLAG_VALOR | `suite.sh:106` | `suite.sh:112–114` | [nao verificado]
| PARALELO | `suite.sh:47,53`, validado em `suite.sh:52` | `suite.sh:200` | [nao verificado]
| ESPERA | `suite.sh:47,55`, validado em `suite.sh:54` | `suite.sh:193`; `trava_teste.sh:27,36` | [nao verificado]

O executor expande `${ONLY:-$LABELS}` e `$PARALELO` sem aspas; preserva `EXTRA` citado. A trava recebe esse argv já formado e executa `"$@"`. (`bin/suite.sh:199`, `bin/suite.sh:200`; `bin/trava_teste.sh:42`)

O modo seco passa pelas guardas, imprime motivo/volta/diretório e labels, mas não imprime `EXTRA`, `PARALELO`, `ESPERA` ou o argv completo. (`bin/suite.sh:143`, `bin/suite.sh:153`, `bin/suite.sh:154`, `bin/suite.sh:155`, `bin/suite.sh:156`)

**Q3 — PROCEDE que consome; NÃO SEI sobre ampliação efetiva no runner instalado.** `-- -k .` e `-- --tag /app` passam porque a guarda de flag de valor exige somente a existência da próxima palavra. Esses valores continuam elementos do array citado; não ganham condição de posicional por expansão do shell. (`bin/suite.sh:112`, `bin/suite.sh:200`)

A cópia delega a descoberta ao `DiscoverRunner` externo; não contém nesse trecho a implementação que permitiria medir a seleção efetiva. Portanto, os exemplos não sustentam afirmar fuga para a inteira. (`app/core/runner_quarentena.py:15`, `app/core/runner_quarentena.py:72`, `app/core/runner_quarentena.py:76`) Quantidade de testes executada: **[não verificado]**.

## 11/11 07:22 · sessao bec9acda611b · - · - · origem=Ronald (tmux)

**Q4 — NÃO PROCEDE como cobertura completa.** Há melhoria real, mas permanecem recusas pela razão errada e mutantes plausíveis sobreviventes. (`bin/tests/test_suite_motivo.sh:57–79`; `bin/tests/test_suite_sh.sh:31–35`)

- Caminhos/globs em `ONLY` agora exigem diagnóstico `nao e label`; há seleção mista inválida e cwd preparado para detectar ausência de `set -f`. (`bin/tests/test_suite_motivo.sh:57–64`)
- O caso `--only ' ' -- --failfast appA appB` recusa na guarda de EXTRA antes de alcançar a classificação da inteira; portanto não prova essa classificação pela razão anunciada. (`bin/tests/test_suite_motivo.sh:43`; `bin/suite.sh:118`; `bin/suite.sh:143`)
- Negativos de EXTRA sempre acrescentam `y` e cobram apenas diagnóstico genérico. Mutante que acrescenta `--pattern` a `_FLAG_SO` aceita a primeira palavra, recusa `y`, mantém rc 2 e o mesmo texto procurado: o caso continua verde pela palavra errada. (`bin/tests/test_suite_motivo.sh:66–68`; `bin/suite.sh:105`; `bin/suite.sh:111–114`)
- **Flag de valor tratada como simples:** acrescentar `-k` a `_FLAG_SO`, preservando `_FLAG_VALOR`, muda `-- -k .` de aceito para recusado; nenhum teste usa `-k` como flag válida. O positivo combinado cobre apenas `-v`, `--tag`, `--exclude-tag=x` e `--failfast`. Sobrevivência integral: **[não verificado]**; esses ramos foram conferidos em memória. (`bin/suite.sh:105–113`; `bin/tests/test_suite_motivo.sh:70`)
- **`--x=v` para flag simples:** ampliar a pertinência do ramo de `=` para `_FLAG_SO` permite `--failfast=x`. Falta negativo dessa forma; o positivo `--exclude-tag=x` continua passando. Sobrevivência integral: **[não verificado]**; ramo confirmado em memória. (`bin/suite.sh:113`; `bin/tests/test_suite_motivo.sh:66–74`)
- O positivo de flags só exige chegada à trava falsa; não confere que EXTRA e PARALELO chegaram intactos. A trava falsa registra `$*`, perdendo fronteiras dos argumentos, e `_args` extrai somente labels anteriores a `--settings`. (`bin/tests/test_suite_motivo.sh:19`; `bin/tests/test_suite_motivo.sh:24`; `bin/tests/test_suite_motivo.sh:70–73`)
- Âncora do motivo, rc 75, executor ignorando recorte, primeira palavra e falta de `set -f` têm casos/mutantes dedicados; o helper de vizinhos permanece substituído. (`bin/tests/test_suite_motivo.sh:20–21`; `bin/tests/test_suite_motivo.sh:75–79`; `bin/tests/test_suite_motivo.sh:96–128`)

**Selo da documentação:** a fixture nua agora é coberta, mas o juiz procura duas substrings, sem provar que o motivo é argumento efetivo nem validar a obra. Ele aceita `bash .../bin/suite.sh # --motivo "pre-opus X"` e `bash .../bin/suite.sh --motivo "pre-opus "`; a guarda real recusa ambos. (`bin/tests/test_suite_sh.sh:34–35`; `bin/tests/test_suite_sh.sh:80–92`; `bin/suite.sh:133`; `bin/suite.sh:143`)

Os selos integrais não foram executados: criam, escrevem e removem arquivos. (`bin/tests/test_suite_motivo.sh:10–21`; `bin/tests/test_suite_motivo.sh:27`; `bin/tests/test_suite_sh.sh:66–83`)

_3714 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
