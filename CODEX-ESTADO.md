# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 12:02** · publicado em **06/11/2026 12:03** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 12:02 · sessao d715f789926a · - · - · origem=Ronald (tmux)

Conteúdo dos arquivos de prova. O lançamento retornou `rc=0`; a sobrevivência do processo permanece [nao verificado].

[prova/1_viva.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/1_viva.out:1)
```text
/bin/bash: line 3: /home/ronald/saas-hasner/INTRUSO_VIVA_C.txt: Read-only file system
rc=1
```

[prova/2_tmp.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/2_tmp.out:1)
```text
/bin/bash: line 4: /tmp/INTRUSO_TMP_C.txt: Read-only file system
rc=1
```

[prova/2b_tmpdir.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/2b_tmpdir.out:1)
```text
/bin/bash: line 5: /tmp/INTRUSO_TMPDIR_C.txt: Read-only file system
rc=1 TMPDIR=
```

[prova/3_git.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/3_git.out:1)
```text
/bin/bash: line 6: /home/ronald/saas-hasner/.git/INTRUSO_GIT_C.txt: Read-only file system
rc=1
```

[prova/3b_commit.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/3b_commit.out:1)
```text
fatal: Unable to create '/home/ronald/saas-hasner/.git/worktrees/wt-codex-provaefeito-115741/index.lock': Read-only file system
rc=128
```

[prova/4_raiz.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/4_raiz.out:1)
```text
/bin/bash: line 8: /home/ronald/wt-codex-provaefeito-115741/INTRUSO_RAIZ_WT.txt: Read-only file system
rc=1
```

[prova/5_home.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/5_home.out:1)
```text
/bin/bash: line 9: /home/ronald/INTRUSO_HOME_C.txt: Read-only file system
rc=1
```

[prova/6_docker.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/6_docker.out:1)
```text
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
rc=1
```

[prova/6b_sock.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/6b_sock.out:1)
```text
rc=7
```

[prova/7_tcp_local.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/7_tcp_local.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/127.0.0.1/22: Operation not permitted
rc=1
```

[prova/7b_tcp_ext.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/7b_tcp_ext.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/1.1.1.1/443: Operation not permitted
rc=1
```

[prova/8_sobrevive.out:1](/home/ronald/wt-codex-provaefeito-115741/app/prova/8_sobrevive.out:1)
```text
lancado
rc=0
```

## 06/11 11:56 · sessao f377d384580c · R1-CAUSAD-V3 · leitura · origem=Code

A premissa precisa de uma ressalva: **nesta cópia, a linha 75 é o desempacotamento em `_hhmm`; `datetime.time(...)` está na linha 83.** O primeiro pode levantar uma exceção tratada para entrada ausente, sem falhar o julgamento. A correspondência entre o fonte executado pela sonda e esta cópia é **[não verificado]**. (`app/ponto/juiz_batida.py:75`, `:77`, `:78`, `:83`)

Nas referências ao anexo `script2.py`, contei `import ast` como linha 1.

1. **A cadeia e quem fornece `s`**

   A sonda chama `call_command("e6_oraculo", ...)`; o comando chama `espelho_do_colab(c, HOJE, ini, fim, agora=...)` para cada colaborador. (`script2.py:545`, anexo; `app/ponto/management/commands/e6_oraculo.py:302`, `:304`)

   O espelho chama `autoridade_do_periodo`, que alimenta as células no motor e chama `apurar_por_fatia`; esta chama `motor.calcular_mes`. (`app/ponto/services/espelho.py:1227`, `:1131`, `:1164`; `app/ponto/services/fechamento.py:277`)

   Há pelo menos estes dois caminhos até o juiz:

   - Motor → `montar_periodos` → `_periodos_via_autoridade` → `_periodos_pelo_marco` → `periodos_do_dia`. (`app/ponto/motor_calculo_v2.py:2354`, `:2748`, `:531`, `:616`)
   - `turnos_do_colab` → `_turnos_do_colab_puro` → `papel_por_minuto_da_ata` → `periodos_do_dia`. O espelho também pede esses turnos diretamente. (`app/ponto/turnos.py:1913`, `:1937`, `:1631`; `app/ponto/services/espelho.py:614`)

   **Quem passa `s` é `periodos_do_dia`**, usando `celula.dna['marcos']`: os pares `hii/hfi`, `hii2/hfi2`, além de `hi/hf`. Nesse ponto, `s` vem do marco previsto congelado, não da luz da batida. (`app/ponto/juiz_batida.py:158`, `:159`, `:172`, `:175`)

   O produtor regular é `montar_dna`: recebe horários de `TipoEscala.marcos_do_dia`/`pausas_do_dia`, serializa com `strftime('%H:%M')` ou `None` e monta `dna['marcos']`; a porta persiste esse DNA em `CelulaDia`. (`app/ponto/management/commands/gerar_celulas.py:32`, `:33`, `:81`, `:83`, `:93`, `:100`; `app/ponto/portas/celula.py:250`, `:253`, `:254`)

2. **Entrada fora da faixa e particularidade do último dia**

   Pela expressão implementada, o construtor falha por hora fora da faixa quando `m = 60*int(h)+int(minuto)` resulta em **`m < 0` ou `m >= 1440`**. Exemplos deduzidos por leitura: `"24:00"` e `"23:60"` dão 1440; `"-1:00"` dá −60. `_hhmm` não valida essas faixas. (`app/ponto/juiz_batida.py:75`, `:76`, `:83`)

   **Os horários válidos do produtor regular não geram esses valores:** são campos `TimeField` ou overrides convertidos em `datetime.time`, depois formatados como `%H:%M`. O valor efetivamente recebido nas ocorrências é **[não verificado]**. (`app/escala/models.py:50`, `:56`, `:221`, `:338`; `app/ponto/management/commands/gerar_celulas.py:33`)

   Há, porém, uma entrada **normal** que gera `ValueError` justamente na **linha 75 desta cópia**: `s=None`. O desempacotamento falha, `_hhmm` captura e devolve `None`, e `_time` retorna `None` sem chamar o construtor. (`app/ponto/juiz_batida.py:75`, `:77`, `:78`, `:83`)

   Isso ocorre estaticamente para uma segunda pausa ausente: o produtor omite suas chaves, mas o leitor consulta sempre os dois pares. Uma chamada com apenas uma pausa pode, portanto, produzir duas dessas exceções internas tratadas. **Quantas ocorrências observadas vieram disso: [não verificado].** (`app/ponto/management/commands/gerar_celulas.py:92`, `:93`; `app/escala/models.py:1652`; `app/ponto/juiz_batida.py:172`)

   Quanto ao último dia: corte 21 determina fim no dia 20, mas `_time` não recebe a janela nem aplica regra especial nessa data. (`app/ponto/janelas.py:15`, `:16`, `:20`; `app/ponto/juiz_batida.py:81`, `:83`)

   **A data registrada pode ser contexto residual da sonda.** `contexto` procura `k/data/dia/data_turno`, ignorando `_d` e `celula.data`; o laço de células usa `_d`. No comando, `k` conserva `fim` após a comparação anterior, e o próximo espelho é chamado antes de reiniciar `dia`. Se não encontrar uma data nos frames intermediários, a sonda pode herdar aquele `k`. A data real das células das ocorrências permanece **[não verificado]**. (`script2.py:110`, `:115`, anexo; `app/ponto/turnos.py:1629`; `app/ponto/management/commands/e6_oraculo.py:304`, `:319`, `:321`, `:323`)

3. **Alcance em produção**

   O caminho compartilhado existe no código de produção:

   | Leitor | Chamada comprovada |
   |---|---|
   | Tela administrativa | `espelho_do_colab` — `app/ponto/views.py:158` |
   | App do colaborador | `espelho_do_colab` — `app/api/views.py:1363` |
   | PDF do espelho | `espelho_do_colab` — `app/relatorios/pdf_espelho.py:602` |
   | Folha/recalculo | Alimenta células e chama `apurar_por_fatia` — `app/ponto/services/fechamento.py:715`, `:745` |
   | Exportação, leitor direto | `_furo_so_da_pausa` chama `periodos_do_dia` — `app/folha/export.py:1142` |
   | Cadastro × realidade, leitor direto | Chama `periodos_do_dia` — `app/escala/services/cadastro_realidade.py:221` |

   O cron configura `processar_cartorio --apply`; o comando chama `julgar_colab`. O cartório alcança `turnos_do_colab` por `dias_encerrados`, chegando ao leitor da ata citado acima. (`app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:1102`; `app/ponto/turnos.py:2147`, `:2148`, `:1937`)

   A sonda não monta manualmente o DNA nem força uma fatia diária: chama o comando real, que passa a janela completa ao espelho. O espelho usa também sua janela visual `vis_ini..hoje`. **Execução em produção com os mesmos valores de `s`: [não verificado].** (`script2.py:505`, `:545`, anexo; `app/ponto/management/commands/e6_oraculo.py:304`; `app/ponto/services/espelho.py:1225`, `:1227`)

4. **Por que milhares de repetições por colaborador-dia**

   **O registro principal acontece no tracer, ao entrar no corpo de um `except`**, inclusive o tratamento normal de `_hhmm`. Ele guarda o evento `exception` e chama `registrar` no evento `line` do handler. Não exige que a exceção escape da função. (`script2.py:213`, `:214`, `:215`, `:219`, anexo)

   Os laços externos da sonda são competências e colaboradores; há uma chamada ao comando por competência, sem retry explícito por dia. Dentro da aplicação, o leitor consulta o juiz em laço por célula, e o motor também o consulta em laço por dia. (`script2.py:515`, `:545`, `:455`, `:462`, anexo; `app/ponto/turnos.py:1629`, `:1631`; `app/ponto/motor_calculo_v2.py:611`, `:616`)

   A deduplicação elimina novos registros **da mesma exceção**, mas cada nova conversão que levanta outra exceção pode ser contada. O multiplicador exato dos milhares apresentados é **[não verificado]**; faltam as pilhas e os valores das ocorrências. (`script2.py:123`, `:125`, anexo)

5. **Efeito nos `PARCIAL`**

   O efeito depende de onde a exceção foi tratada:

   - **Capturada dentro de `_hhmm`, na linha 75:** retorna `None`, o julgamento continua e não há exclusão nem divergência automática por essa exceção. (`app/ponto/juiz_batida.py:78`, `:83`, `:173`)
   - **Escapa do juiz, mas é capturada pelo leitor de turnos:** aquela consulta é ignorada na construção do mapa da ata. No caminho do motor, há log e `continue`; isso não exclui diretamente o dia da comparação do E6. (`app/ponto/turnos.py:1632`, `:1633`; `app/ponto/motor_calculo_v2.py:617`, `:619`, `:621`)
   - **Escapa de `espelho_do_colab`:** o E6 incrementa `erros no espelho` e pula **o colaborador inteiro**. (`app/ponto/management/commands/e6_oraculo.py:305`, `:306`, `:307`)

   O E6 decide separadamente quais dias compara: falta de linha, isenção/ausência, falta de lavratura, ambos zero e janela declarada têm seus próprios desvios; só depois entram `dias_comparados` e os acumuladores de divergência. O `PARCIAL` copia esses acumuladores, sem descontar o contador de exceções da sonda. (`app/ponto/management/commands/e6_oraculo.py:324`, `:327`, `:331`, `:339`, `:342`, `:352`, `:367`, `:368`; `script2.py:186`, `:190`, `:342`, anexo)

   **O impacto numérico efetivo nas ocorrências fornecidas é [não verificado].** Para concluir, faltam o fonte efetivamente carregado, traceback completo, `repr(s)`, chave do marco e identificação/data real da célula. Somatórios consistentes verificam a soma dos acumuladores; não certificam a ausência de efeitos anteriores sobre o cálculo. (`script2.py:292`, `:309`, anexo)

## 06/11 11:54 · sessao 456448b2f2e7 · - · - · origem=Ronald (tmux)

Principais achados, com numeração do anexo contada manualmente a partir de `import ast` = linha 1:

- Há divergência decisiva na premissa: na cópia, `juiz_batida.py:75` é `h, m = str(s).split(':')[:2]`; o construtor `datetime.time(...)` está na linha 83. A linha 75 pode levantar `ValueError` esperado para `None`, e o próprio `_hhmm` captura isso e retorna `None` em 77–78. Não foi demonstrado que os registros apresentados vieram do construtor. (`app/ponto/juiz_batida.py:75`, `app/ponto/juiz_batida.py:77`, `app/ponto/juiz_batida.py:83`)

- O caminho benigno é alcançável sem dado malformado: `PARES_PAUSA_DNA` contém os dois pares, inclusive `hii2/hfi2`; `periodos_do_dia` chama `_time(marcos.get(...))` para todos e depois filtra pares ausentes. Um marco ausente produz `None`, erro de unpack capturado em 75–78 e retorno `None` de `_time`. A sonda registra a entrada nesse `except`, embora a função continue normalmente. (`app/escala/models.py:1652`, `app/ponto/juiz_batida.py:170`, `app/ponto/juiz_batida.py:172`, `app/ponto/juiz_batida.py:173`; anexo `script2.py:215`, `script2.py:219`)

- A sonda registra exceções capturadas, não apenas falhas escapadas: `rastrear` memoriza evento `exception` e chama `registrar` quando o frame entra no corpo de um handler; `registrar` deduplica pela própria exceção, sem colapsar novas exceções equivalentes. Isso permite milhares de eventos de parsing normal repetido. Quantas das 438266 ocorrências são desse tipo: **não verificado**, faltam `s`, o frame exato da captura e a versão que executou. (anexo `script2.py:123`, `script2.py:125`, `script2.py:213`, `script2.py:219`)

- Não existe retry explícito da chamada por dia no script: há laço de competências, seleção/iteração de colaboradores e chamada única ao comando por competência. O comando pede um espelho por colaborador com a janela completa. (anexo `script2.py:515`, `script2.py:545`, `script2.py:455`, `script2.py:462`; `app/ponto/management/commands/e6_oraculo.py:302`, `app/ponto/management/commands/e6_oraculo.py:304`)

- Um laço que multiplica chamadas ao juiz é `for _d, cel in celulas.items()`: chama `periodos_do_dia` para cada célula, captura exceção que escape e continua. Esse corpo é alcançado por `turnos_do_colab` → `_turnos_do_colab_puro` → `papel_por_minuto_da_ata`. O motor também consulta o juiz em outro laço por `dia`. Multiplicador exato das contagens apresentadas: **não verificado**. (`app/ponto/turnos.py:1629`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1632`, `app/ponto/turnos.py:1913`, `app/ponto/turnos.py:1937`, `app/ponto/motor_calculo_v2.py:611`, `app/ponto/motor_calculo_v2.py:616`)

- “Último dia” pode ser atribuição incorreta da sonda: ela procura locais chamados `k`, `data`, `dia`, `data_turno`, mas o laço de células usa `_d` e o juiz recebe `celula`. No `handle`, `k` permanece igual a `fim` após terminar o colaborador anterior, enquanto `dia` já virou `fim+1`; o próximo espelho é chamado antes de reiniciar `dia`. Assim, uma exceção sem uma dessas datas nos frames mais próximos pode herdar `k` do colaborador anterior. A data real das células das ocorrências **não está verificada**. (anexo `script2.py:86`, `script2.py:110`, `script2.py:115`; `app/ponto/turnos.py:1629`, `app/ponto/juiz_batida.py:149`, `app/ponto/management/commands/e6_oraculo.py:304`, `app/ponto/management/commands/e6_oraculo.py:319`, `app/ponto/management/commands/e6_oraculo.py:321`, `app/ponto/management/commands/e6_oraculo.py:323`)

Quanto aos `PARCIAL`:

- Se for o `ValueError` capturado dentro de `_hhmm`, não há exclusão automática de dia nem divergência automática: o parser retorna `None` e o julgamento continua. Impacto numérico das ocorrências reais: **não verificado**. (`app/ponto/juiz_batida.py:77`, `app/ponto/juiz_batida.py:83`, `app/ponto/juiz_batida.py:173`)

- Se o juiz levantar externamente no caminho de `papel_por_minuto_da_ata`, somente aquela consulta de célula é ignorada nesse mapa; o restante do pareamento continua. O código não transforma a exceção diretamente em `dia_sem_lavratura`, divergência ou exclusão do universo do E6. (`app/ponto/turnos.py:1632`, `app/ponto/turnos.py:1633`, `app/ponto/turnos.py:1937`, `app/ponto/turnos.py:1990`)

- Só uma exceção que escape de `espelho_do_colab` incrementa `erros no espelho` e pula **o colaborador inteiro**. Os erros internos capturados podem coexistir com `erros_no_espelho 0`. (`app/ponto/management/commands/e6_oraculo.py:304`, `app/ponto/management/commands/e6_oraculo.py:305`, `app/ponto/management/commands/e6_oraculo.py:306`, `app/ponto/management/commands/e6_oraculo.py:307`, `app/ponto/management/commands/e6_oraculo.py:396`)

- O E6 percorre `ini..fim`, classifica exclusões por falta de linha, isenção/ausência, falta de lavratura, ambos zero ou janela declarada; só depois incrementa `dias_comparados`, calcula divergência e soma donos/horas/colaboradores. O `PARCIAL` copia esses acumuladores, sem recomputar nem descontar os erros registrados pela sonda. (`app/ponto/management/commands/e6_oraculo.py:320`, `app/ponto/management/commands/e6_oraculo.py:324`, `app/ponto/management/commands/e6_oraculo.py:327`, `app/ponto/management/commands/e6_oraculo.py:330`, `app/ponto/management/commands/e6_oraculo.py:339`, `app/ponto/management/commands/e6_oraculo.py:342`, `app/ponto/management/commands/e6_oraculo.py:352`, `app/ponto/management/commands/e6_oraculo.py:367`; anexo `script2.py:175`, `script2.py:186`, `script2.py:190`, `script2.py:342`)

A soma fechar prova consistência dos acumuladores copiados; não prova ausência de efeitos das exceções sobre os valores calculados. (`app/ponto/management/commands/e6_oraculo.py:424`, `app/ponto/management/commands/e6_oraculo.py:426`; anexo `script2.py:292`, `script2.py:309`)

## 06/11 11:54 · sessao ae67676bb6c8 · O10-B · verificador · origem=Code

**Sugiro bloquear** pela guarda alimentada com empresa potencialmente desatualizada e pela proteção de cobrança órfã que não recebe a edição humana; há também risco de inversão de locks (`app/ponto/portas/celula.py:564`, `app/escala/services/regeneracao.py:145`, `app/ponto/portas/celula.py:649`).

Comparação com o pai limitada ao diff fornecido. Identidade Git da cópia, execução da suíte, REDs observados, concorrência real e efeitos em produção: **[nao verificado]**. [nao verificado]

| Item 1 — campo | Quem escreve e quem lê | procedencia sugerida |
|---|---|---|
| `CelulaDia.trabalha` | Escrevem geração, regeneração, reversão e edição, dentro da porta (`app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:276`, `app/ponto/portas/celula.py:781`, `app/ponto/portas/celula.py:842`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:600`). Leem o juiz do previsto, o juiz de dia sem trabalho, o leitor da ata, o classificador de jornada e o diagnóstico do colaborador (`app/escala/models.py:1094`, `app/chamados/juizes.py:427`, `app/escala/services/leitor_celula.py:381`, `app/escala/servico_jornada.py:303`, `app/ponto/services/dia_do_colab.py:90`). | nada |
| `CelulaDia.dna` | Escrevem geração, regeneração, enriquecimento, reversão e edição (`app/ponto/portas/celula.py:248`, `app/ponto/portas/celula.py:264`, `app/ponto/portas/celula.py:779`, `app/ponto/portas/celula.py:845`, `app/ponto/portas/celula.py:853`, `app/ponto/portas/celula.py:863`, `app/ponto/portas/celula.py:601`). Leem marcos, pausas, extra declarada, cargas de DNA e impressão/ata do cartório (`app/escala/models.py:1124`, `app/escala/models.py:1189`, `app/escala/models.py:1235`, `app/escala/utils.py:46`, `app/escala/utils.py:97`, `app/escala/utils.py:130`, `app/ponto/services/cartorio.py:1348`, `app/ponto/services/cartorio.py:1386`). | nada |
| `CelulaDia.escala_geradora` | Escrevem geração, regeneração, reversão e edição (`app/ponto/portas/celula.py:264`, `app/ponto/portas/celula.py:276`, `app/ponto/portas/celula.py:784`, `app/ponto/portas/celula.py:844`, `app/ponto/portas/celula.py:862`, `app/ponto/portas/celula.py:602`). O juiz do vínculo lê a geradora e verifica sua vigência; carregadores também a usam para incluir o vínculo nomeado (`app/escala/alimentacao.py:270`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/ponto/turnos.py:2343`, `app/ponto/turnos.py:2944`). | nada |
| `CelulaDia.origem` — escritores | Geração/regeneração criam `gerada`; `editar_dia` cria ou grava `editada` (`app/ponto/portas/celula.py:264`, `app/ponto/portas/celula.py:844`, `app/ponto/portas/celula.py:597`, `app/ponto/portas/celula.py:603`). | nada |
| `origem='editada'` — regeneração | Os dois ramos de alteração exigem `gerada`. A escrita nova exclui a célula da regeneração, inclusive da desativação de dias fora da vigência (`app/ponto/portas/celula.py:242`, `app/ponto/portas/celula.py:267`). | nada |
| `origem='editada'` — reversão e geração periódica | A reversão recusa origem diferente de `gerada`; o gerador periódico pula explicitamente `editada`. A edição passa a impedir ambos os caminhos (`app/ponto/portas/celula.py:767`, `app/ponto/portas/celula.py:847`). | nada |
| `origem='editada'` — edição e diagnósticos | A edição exige essa origem para devolver `igual`; os diagnósticos de divergência contra o template selecionam somente `gerada`, portanto deixam de contar o dia editado (`app/ponto/portas/celula.py:591`, `app/escala/management/commands/celulas_contra_o_template.py:29`, `app/ponto/management/commands/tripwire_celulas.py:21`). Origem também é exposta na fotografia da trilha, no diagnóstico e em `__str__` (`app/ponto/portas/celula.py:527`, `app/ponto/services/dia_do_colab.py:90`, `app/escala/models.py:1842`). | nada |
| `editada_em` — escritor | `editar_dia` obtém o instante e grava na criação/atualização (`app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:604`). | nada |
| `editada_em` — leitor de perguntas | Entra no máximo de fatos **por colaborador**; se esse máximo ultrapassa `julgado_em`, a pergunta fica stale. Uma edição pode invalidar perguntas daquele colaborador; repetição idêntica não avança o carimbo (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:187`, `app/ponto/portas/celula.py:591`). | nada |
| `editada_em` — leitor do cartório | `houve_fato_novo` responde verdadeiro quando `desde < editada_em <= ate`; a escrita nova pode autorizar reabertura que antes não teria esse fato (`app/ponto/services/cartorio.py:365`). | nada |
| `editada_em` — leitor de fase | A janela antiga reabre quando o maior carimbo dos colaboradores/dias examinados supera a lavra anterior. A edição pode mudar essa decisão (`app/escala/services/fase_12x36.py:188`, `app/escala/services/fase_12x36.py:191`). | nada |
| `editada_por` e fotografia | A edição grava o autor; a fotografia lê autor e instante para antes/depois (`app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:605`, `app/ponto/portas/celula.py:529`). A FK também admite anulação por exclusão do usuário (`app/escala/models.py:1733`). Outro leitor decisório específico: nenhum localizado **[nao verificado quanto a acessos dinâmicos]**. | nada |
| `LogAuditoria`: duas ações novas | A porta chama `evento_kw` para `editar_dia` e `trocar_plantao`; o adaptador delega ao escritor efetivo de auditoria (`app/ponto/portas/celula.py:608`, `app/ponto/portas/celula.py:675`, `app/core/observ.py:146`, `app/core/observ.py:134`). A tela genérica consulta e filtra essas linhas (`app/core/views.py:705`, `app/core/views.py:709`). Leitor de negócio específico das duas ações: nenhum localizado **[nao verificado quanto a acessos dinâmicos]**. | nada |
| `Ausencia.documento/.data/.colaborador` | A troca **lê**, sem alterar: relê cartas travadas, compara colaboradores, exige documento, usa cada data e fotografa o nome do arquivo (`app/ponto/portas/celula.py:649`, `app/ponto/portas/celula.py:654`, `app/ponto/portas/celula.py:656`, `app/ponto/portas/celula.py:662`, `app/ponto/portas/celula.py:672`). A porta existente monta os três campos na criação, altera data e substitui documento (`app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:499`, `app/ponto/services/ausencia.py:857`, `app/ponto/services/ausencia.py:1128`). Cobertura e fato novo leem colaborador/data (`app/ponto/turnos.py:2753`, `app/ponto/turnos.py:2766`, `app/ponto/services/cartorio.py:355`). | nada |
| Limite das cartas | O requisito implementado é `documento` não vazio; não há consulta à existência do arquivo no storage nessa guarda (`app/ponto/portas/celula.py:656`). Existência/conteúdo das cartas em produção: **[nao verificado]**. | nada |

| Item 2 — hoist | Comparação de comportamento | procedencia sugerida |
|---|---|---|
| Ordem e condições | Pelo diff, preserva julgar → chamar dinheiro → registrar evento posterior. Conserva exclusão de futuro/veredito humano, `recalculo`, `julgados` e deduplicação por competência (`app/ponto/portas/celula.py:428`, `app/ponto/portas/celula.py:437`, `app/ponto/portas/celula.py:440`, `app/ponto/portas/celula.py:451`, `app/ponto/portas/celula.py:462`, `app/ponto/portas/celula.py:508`). Diferença funcional nesse fluxo normal: **nenhum**. | nada |
| Logs | Para regeneração, rótulo, contexto e motivo mantêm o texto final anterior; os formatos passam a receber parâmetros (`app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:446`, `app/ponto/portas/celula.py:450`, `app/ponto/portas/celula.py:468`). A localização/função emissora agora é o helper, portanto não é movimento literalmente “byte a byte” (`app/ponto/portas/celula.py:372`, `app/ponto/portas/celula.py:379`). | fila |
| Callback e `engolir` | Mantém `_reavaliar_o_dia` e os contextos `ec`/`colab` da regeneração; os testes filtram por `__name__`, que permanece igual (`app/ponto/portas/celula.py:361`, `app/ponto/portas/celula.py:499`, `app/ponto/portas/celula.py:501`, `app/ponto/portas/celula.py:505`, `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:149`, `app/chamados/tests/test_e2_celula_regenerada_reavalia.py:228`). | nada |
| Diferença no no-op | Agora `ec.colaborador` é resolvido incondicionalmente ao chamar o helper; no pai fornecido, os acessos do rabo estavam dentro dos blocos condicionais. Com relação não carregada e `_tocadas` vazio, introduz dependência de carregamento desnecessária; eventual consulta/falha real **[nao verificado]** (`app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:428`, `app/ponto/portas/celula.py:496`). | fila |

| Item 3 — `editar_dia` | Resultado | procedencia sugerida |
|---|---|---|
| a — universo da guarda | Usa o mesmo enumerador e, por ele, o mesmo juiz: TXT vigente e holerite de lote publicado, com janela da autoridade (`app/ponto/portas/celula.py:566`, `app/ponto/management/commands/gerar_celulas.py:161`, `app/ponto/management/commands/gerar_celulas.py:169`, `app/ponto/management/commands/gerar_celulas.py:173`, `app/folha/export.py:610`, `app/folha/export.py:614`). **Mas não garante a mesma alimentação:** regeneração consulta `empresa_id` atual no banco; edição usa a instância recebida (`app/ponto/portas/celula.py:215`, `app/ponto/portas/celula.py:564`). Contraexemplo estático: instância ainda na empresa A, cadastro já na B, TXT apenas em B; a edição pode não enumerar a exportação de B e escrever pelo mesmo colaborador (`app/ponto/management/commands/gerar_celulas.py:161`, `app/ponto/portas/celula.py:590`). Ocorrência: **[nao verificado]**. | **bloqueia** |
| a — abertura | Só o argumento `apesar_da_lavra` desvia dessa guarda; qualquer valor verdadeiro é aceito, sem validação textual adicional, como na regeneração (`app/ponto/portas/celula.py:565`, `app/ponto/portas/celula.py:212`). A abertura é registrada na trilha (`app/ponto/portas/celula.py:611`, `app/ponto/portas/celula.py:615`). | nada |
| b — idempotência sequencial | **Nenhum** caminho localizado que mova `editada_em` numa repetição com origem, trabalho, DNA e geradora iguais: retorna antes do relógio, save, trilha e rabo (`app/ponto/portas/celula.py:591`, `app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:619`). Autor/motivo diferentes também são ignorados nesse no-op; template alterado pode produzir DNA diferente e justificar nova escrita (`app/ponto/portas/celula.py:588`, `app/ponto/portas/celula.py:591`). | nada |
| b — célula ainda inexistente | O lock procura uma linha existente; não há trava de uma chave ausente nem tratamento de conflito no `create`. Duas edições isoladas concorrentes podem disputar a criação; a unicidade impede duas linhas, mas o resultado da segunda não é garantido como `igual` (`app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:597`, `app/escala/models.py:1833`). Execução concorrente: **[nao verificado]**. | fila |
| c — falha da trilha | `evento` usa savepoint próprio e devolve `None` após falha engolida; a edição transforma `None` em `RuntimeError`, ainda dentro do atomic, desfazendo a escrita (`app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`, `app/ponto/portas/celula.py:589`, `app/ponto/portas/celula.py:616`). Exceção propagada por `evento_kw` também sai do atomic (`app/ponto/portas/celula.py:608`). | nada |
| d — DNA/vínculo do dia | **Nenhum clone localizado:** chama `vinculo_do_dia_carregado` e `montar_dna`; o carregador delega ao juiz, que privilegia a geradora dentro da vigência, sem escolher outra silenciosamente (`app/ponto/portas/celula.py:581`, `app/ponto/portas/celula.py:588`, `app/escala/alimentacao.py:289`, `app/escala/alimentacao.py:270`, `app/escala/alimentacao.py:282`). Marcos vêm do template desse vínculo (`app/ponto/management/commands/gerar_celulas.py:76`, `app/ponto/management/commands/gerar_celulas.py:81`). | nada |
| e — futuro / sem célula | Futuro é editável: grava e agenda o rabo, mas o julgamento imediato é pulado; dinheiro/eventos continuam sendo solicitados (`app/ponto/portas/celula.py:597`, `app/ponto/portas/celula.py:619`, `app/ponto/portas/celula.py:437`, `app/ponto/portas/celula.py:464`, `app/ponto/portas/celula.py:508`). Sem célula, cria se houver vínculo; sem vínculo, retorna `sem_vinculo` e informa `barrados` quando fornecido (`app/ponto/portas/celula.py:582`, `app/ponto/portas/celula.py:597`). | nada |
| e — veredito humano | A edição muda trabalho/DNA, conserva veredito/ata e o helper pula os vereditos humanos (`app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:435`, `app/ponto/portas/celula.py:437`; humanos em `app/colaboradores/services/pos_vinculo.py:14`). O leitor prefere `tipo_dia` e minutos da ata, podendo continuar exibindo o julgamento anterior ao chão novo (`app/escala/services/leitor_celula.py:380`, `app/escala/services/leitor_celula.py:382`). Resultado financeiro concreto: **[nao verificado]**. | fila |
| e — já editada | Igual devolve `igual`; diferente sobrescreve, avança autoria/instante e fotografa o antes real. Não altera os campos de regeneração (`app/ponto/portas/celula.py:591`, `app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:613`). | nada |

| Item 4 — `trocar_plantao` | Resultado | procedencia sugerida |
|---|---|---|
| a — atomicidade do par | **Nenhum caminho localizado de persistência de apenas uma edição:** atomic externo contém ambas e a trilha final; recusa sai dele antes de ser capturada, e falha inesperada propaga (`app/ponto/portas/celula.py:644`, `app/ponto/portas/celula.py:662`, `app/ponto/portas/celula.py:665`, `app/ponto/portas/celula.py:675`, `app/ponto/portas/celula.py:683`). | nada |
| b — locks/deadlock | Cartas são ordenadas por PK; células seguem cedente → substituto, sem ordem global (`app/ponto/portas/celula.py:650`, `app/ponto/portas/celula.py:662`, `app/ponto/portas/celula.py:665`, `app/ponto/portas/celula.py:590`). O lock também inclui `select_related('colaborador')`, sem `of`, com PostgreSQL configurado (`app/ponto/portas/celula.py:649`, `app/config/settings/prod.py:11`). **Inferência:** cartas distintas podem produzir ordem A→B versus B→A nos colaboradores associados; SQL efetivo/deadlock observado **[nao verificado]**. As mesmas duas cartas já se serializam pela ordenação dos PKs (`app/ponto/portas/celula.py:650`). | **bloqueia** |
| c — recusas estruturais | Par incompleto, carta removida, mesmo colaborador ou documento ausente geram uma entrada com `dias=0`, `de=None`, `ate=None` e texto explicativo (`app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:654`, `app/ponto/portas/celula.py:656`, `app/ponto/portas/celula.py:685`). | nada |
| c — recusa de um lado | Exportada/sem vínculo geram `dias=1`, datas daquele lado e motivo; a primeira recusa interrompe o fluxo, portanto não enumera necessariamente todos os lados inválidos (`app/ponto/portas/celula.py:577`, `app/ponto/portas/celula.py:584`, `app/ponto/portas/celula.py:663`, `app/ponto/portas/celula.py:666`, `app/ponto/portas/celula.py:687`). Sem autor/motivo ou falha inesperada levantam exceção, sem conversão para `barrados` (`app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:640`, `app/ponto/portas/celula.py:683`). | nada |
| d — antes do commit | `julgar_celula` roda sincronamente depois de cada edição, ainda no atomic externo; lavra e carimbo pertencem à transação e são revertidos com ela. Exceções de julgamento são logadas/engolidas (`app/ponto/portas/celula.py:619`, `app/ponto/portas/celula.py:440`, `app/ponto/portas/celula.py:443`, `app/ponto/services/cartorio.py:1802`, `app/ponto/services/cartorio.py:1822`). | nada |
| d — depois do commit/rollback | `recalcular_por_evento` registra `_rodar`; o helper registra depois rejunção/falta. Ordem direta: dinheiro cedente → evento cedente → dinheiro substituto → evento substituto (`app/ponto/services/fechamento.py:1791`, `app/ponto/services/fechamento.py:1808`, `app/ponto/portas/celula.py:464`, `app/ponto/portas/celula.py:508`, `app/ponto/portas/celula.py:662`, `app/ponto/portas/celula.py:665`). Rollback do atomic descarta esses callbacks; comportamento observado em execução **[nao verificado]**. Falhas posteriores são tratadas como best-effort, sem desfazer o par já confirmado (`app/ponto/services/fechamento.py:1802`, `app/core/observ.py:86`). | nada |

| Item 5 — classes de reprovação | Achado no diff/cadeia | procedencia sugerida |
|---|---|---|
| Segundo juiz / derivação paralela | **nenhum** novo localizado para vínculo, competência ou DNA: reutiliza carregador, enumerador, produtor e autoridades de julgamento/janela (`app/ponto/portas/celula.py:555`, `app/ponto/portas/celula.py:566`, `app/ponto/portas/celula.py:588`, `app/ponto/portas/celula.py:440`, `app/ponto/portas/celula.py:459`). A diferença de alimentação da empresa permanece sendo o achado 3a (`app/ponto/portas/celula.py:564`). | nada adicional |
| Segundo escritor fora da porta | **nenhum** no diff: novas escritas estão em `editar_dia`, declarada como quarta operação; troca chama essa operação (`app/ponto/portas/celula.py:82`, `app/ponto/portas/celula.py:597`, `app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:662`). | nada |
| Relógio fora da borda | Novo `now()` interno carimba auditoria; o `localdate()` decisório do helper já existia no pai. A entrada nova não permite injetar esse tempo (`app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:432`, `app/ponto/portas/celula.py:533`). O corte sobre juiz/grade pede instante recebido da borda; não há base para classificar automaticamente todo carimbo de auditoria como novo juiz (`app/docs/CORTES.md:370`). | fila |
| Testes com resultado | Há asserções de estado para cartas, cedente/substituto, idempotência, vínculo, rollback, exportada e trilhas (`app/ponto/tests/test_o10b_editar_dia_troca.py:147`, `app/ponto/tests/test_o10b_editar_dia_troca.py:178`, `app/ponto/tests/test_o10b_editar_dia_troca.py:206`, `app/ponto/tests/test_o10b_editar_dia_troca.py:238`, `app/ponto/tests/test_o10b_editar_dia_troca.py:287`, `app/ponto/tests/test_o10b_editar_dia_troca.py:325`, `app/ponto/tests/test_o10b_editar_dia_troca.py:380`, `app/ponto/tests/test_o10b_editar_dia_troca.py:414`). RED/verde executado: **[nao verificado]**. | nada |
| Selo de lock sem mordida suficiente | A01 procura somente os nomes `atomic`/`select_for_update`; uma chamada morta preservaria a aprovação desse selo. Não verifica avaliação, posição nem ordem das travas (`app/ponto/tests/test_o10b_editar_dia_troca.py:318`, `app/ponto/tests/test_o10b_editar_dia_troca.py:322`). | fila |
| Rollback de callbacks sem prova | `_trocar` substitui `on_commit` por execução imediata dentro do atomic; A02 verifica células/trilhas revertidas, mas não prova descarte real dos callbacks (`app/ponto/tests/test_o10b_editar_dia_troca.py:39`, `app/ponto/tests/test_o10b_editar_dia_troca.py:115`, `app/ponto/tests/test_o10b_editar_dia_troca.py:325`). | fila |
| Mutantes sobreviventes por inspeção | Remover as guardas `_t is None` não altera os caminhos normais testados; a falha injetada é no segundo `editar_dia`, não na auditoria (`app/ponto/portas/celula.py:616`, `app/ponto/portas/celula.py:681`, `app/ponto/tests/test_o10b_editar_dia_troca.py:330`). Remover a atualização de `editada_por` também não é distinguido pelas reedições com o mesmo administrador (`app/ponto/portas/celula.py:605`, `app/ponto/tests/test_o10b_editar_dia_troca.py:365`). Aprovação efetiva desses mutantes: **[nao verificado]**. | fila |
| Ordem de trava | Achado do item 4b; o selo atual não o distingue (`app/ponto/portas/celula.py:649`, `app/ponto/portas/celula.py:662`, `app/ponto/tests/test_o10b_editar_dia_troca.py:322`). | **bloqueia** |
| Fallback disfarçado | **nenhum funcional novo localizado:** falta de vínculo recusa; falha da identificação da lavra muda somente a mensagem, conservando a recusa (`app/ponto/portas/celula.py:569`, `app/ponto/portas/celula.py:573`, `app/ponto/portas/celula.py:580`, `app/ponto/po

[truncado em 20000 de 24062 caracteres]

## 06/11 11:54 · sessao afde0bb962be · - · - · origem=Ronald (tmux)

Na cópia, **`ValueError` em `:75` pode ser tratamento normal de entrada ausente**, sem falhar `_time`:

- `_hhmm(None)` tenta desempacotar a lista de um elemento produzida por `str(None).split(':')[:2]`, captura o `ValueError` e retorna `None`; `_time` então retorna `None` sem construir `datetime.time`. Referência: `app/ponto/juiz_batida.py:72-83`.
- O produtor retorna `None` para horário ausente e pode gravar `hii/hfi=None`; também omite `hii2/hfi2` quando não há segunda pausa. Referências: `app/ponto/management/commands/gerar_celulas.py:32-33`, `:83-84`, `:92-93`.
- O leitor percorre sempre os dois pares declarados em `PARES_PAUSA_DNA`, obtendo `None` para chaves ausentes. Portanto um dia com apenas uma pausa provoca **duas exceções internas capturadas** nesse ponto, pela avaliação dos dois marcos do segundo par. Referências: `app/escala/models.py:1652`, `app/ponto/juiz_batida.py:170-173`.
- Se `dna.marcos` não existe ou não é mapa, o leitor usa `{}` e avalia os quatro marcos de pausa mais `hi/hf`, todos ausentes: **seis exceções internas capturadas**, antes de prosseguir para resultado sem previsão. Referências: `app/ponto/juiz_batida.py:159`, `:170-176`, `:195-204`.
- Há ainda formato posicional normal `·I1/·I2/...`, passado a `_hhmm` pela conferência de marcos: cada string sem `:` provoca o mesmo tratamento interno, retornando `None`. Referências: `app/ponto/juiz_batida.py:190-194`, `:230`, `:75-78`.

A sonda registra a entrada de handlers e, portanto, inclui esses `ValueError` capturados; o pai deve citar a numeração do anexo em `linhas_de_handlers`/`rastrear`. Atribuir os **438266** eventos a esses casos específicos permanece **[não verificado]** sem `s`, stack e fonte executado. A origem relatada **`:75` corresponde ao desempacotamento na cópia**, enquanto o construtor de hora está em **`:83`**. Referência: `app/ponto/juiz_batida.py:75-83`.

## 06/11 11:53 · sessao 098861121871 · - · - · origem=Ronald (tmux)

Leitores de produção e alcançabilidade estática:

- **Tela admin:** `ponto/views.py:154-158` calcula a janela e chama `espelho_do_colab`. Este chama `autoridade_do_periodo(colaborador, vis_ini, hoje)` (`app/ponto/services/espelho.py:1225-1227`), alimenta células no motor (`:1131`) e chama `apurar_por_fatia` (`:1163-1165`). A fatia chama `motor.calcular_mes` (`app/ponto/services/fechamento.py:277-282`); `_periodos_via_autoridade` chama `_periodos_pelo_marco` (`app/ponto/motor_calculo_v2.py:530-531`), que chama `periodos_do_dia` e captura exceção com log+continue (`:611-621`).
- **App colaborador:** chama a mesma `espelho_do_colab` (`app/api/views.py:1363-1369`).
- **PDF espelho:** chama a mesma `espelho_do_colab`, com teto visual de ontem (`app/relatorios/pdf_espelho.py:596-602`).
- **Folha/recalculo:** `recalcular_fechamento_mes` alimenta `motor.celulas_alimentadas` e chama o mesmo `apurar_por_fatia` (`app/ponto/services/fechamento.py:715-746`). Leitura online `fechamento_lido` chama esse recalculo com `somente_leitura=True` (`:1743-1745`). Porta de export também chama `espelho_do_colab` (`app/folha/porta_export.py:469-476`) e usa `autoridade_do_periodo` em outro comparador (`:72-78`).
- **Folha, leitor direto adicional:** `_furo_so_da_pausa` chama `periodos_do_dia(c, None)` (`app/folha/export.py:1140-1147`); `nao_retem_pela_pausa` o consulta quando protestos são exatamente `FURO_PARCIAL` (`:1165`).
- **Turnos, leitor direto:** `papel_por_minuto_da_ata` carrega `CelulaDia` da faixa quando não alimentada (`app/ponto/turnos.py:1583-1585`) e chama `periodos_do_dia` em laço por célula; exceção apenas dá `continue` (`:1629-1633`). `_turnos_do_colab_puro` chama esse leitor (`:1937`).
- **Cartório/cron:** configuração agenda `processar_cartorio --apply` (`app/config/crons.py:616`); comando carrega células anteriores a hoje e chama `julgar_colab` (`app/ponto/management/commands/processar_cartorio.py:49-65`). Corpo do cartório chama grade com `ref=d1+1`, `n_dias=span+2` (`app/ponto/services/cartorio.py:1124-1126`). Despacho da grade depende de `GRADE_POR_TURNO` (`app/escala/utils.py:2105-2111`; padrão True em `app/config/settings/base.py:165`). Ramo por turno copia células/ata em memória e chama `turnos_do_colab` (`app/escala/utils.py:1901-1919`), chegando ao leitor acima. Outro elo: cartório chama `dias_encerrados` (`app/ponto/services/cartorio.py:1102-1105`), que chama `turnos_do_colab` se não recebeu lista (`app/ponto/turnos.py:2147-2148`).
- **Cadastro x realidade, leitor direto adicional:** consulta células da competência corrente até hoje e chama `periodos_do_dia` (`app/escala/services/cadastro_realidade.py:207-221`).

A sonda fornecida chama a função real e o oráculo usa `espelho_do_colab(c, HOJE, ini, fim)` (`app/ponto/management/commands/e6_oraculo.py:285,302-304`); também usa `InsumosDoDono.turnos()` com janela ampliada em um dia nas duas pontas (`:148-151`). Portanto o juiz não é exclusivo de insumo manual ou fatia artificial da sonda; a execução real em produção **com os mesmos valores de `s`** permanece **[não verificado]**, pois o dado não contém `s`, DNA nem stack completo.

Divergência de fonte a salientar: nesta cópia `_time` está em **`app/ponto/juiz_batida.py:81-83`**; a linha **75** é o `split` de `_hhmm`. O dado raspado atribui o construtor `datetime.time` à linha 75. Identidade exata do arquivo efetivamente carregado na sonda versus esta cópia **[não verificado]**.

## 06/11 11:53 · sessao afde0bb962be · - · - · origem=Ronald (tmux)

- Na cópia, `_time` está em **`app/ponto/juiz_batida.py:81-83`**; a linha **75** é `h, m = str(s).split(':')[:2]`. A identidade entre o fonte executado pela sonda e esta cópia é **[não verificado]**.

- Quem fornece `s` é `periodos_do_dia`: lê `celula.dna`, extrai `dna['marcos']` e chama `_time` para os pares de pausa e para `hi`/`hf`. Não passa a luz da batida nem `dna['lampadas'][...]['instante']`. Referências: `app/ponto/juiz_batida.py:157-175`.

- Produtor regular: `montar_dna` chama `TipoEscala.marcos_do_dia(data)` e `pausas_do_dia(data)`, serializa cada hora com `_hhmm(t) = t.strftime('%H:%M')` e põe o mapa em `dna['marcos']`. Referências: `app/ponto/management/commands/gerar_celulas.py:32-33`, `:73-100`. Os horários padrão são `TimeField`; os overrides passam por `datetime.time`. Referências: `app/escala/models.py:50-56`, `:214-222`, `:326-345`. Persistência normal da célula com esse DNA: `app/ponto/portas/celula.py:249-254`, `:630-655`.

- A condição matemática é `60*int(h)+int(minuto) < 0` ou `>= 1440`; `_hhmm` não valida faixas. Exemplos deduzidos, **não executados**: `"24:00"` → 1440; `"23:60"` → 1440; `"-1:00"` → −60. Referências: `app/ponto/juiz_batida.py:72-83`. Esses formatos não são produzidos pelo `strftime('%H:%M')` sobre os `datetime.time` do produtor regular. Referências: `app/ponto/management/commands/gerar_celulas.py:32-33`, `app/escala/models.py:214-222`. Valor efetivamente causador, chave e produtor histórico: **[não verificado]**, faltam `repr(s)`, chave de `marcos`, identificação da célula e traceback integral.

- Um instante ISO como `"2026-10-20T07:00"` ou `"2026-10-20 07:00"` não explica hora acima de 23 por este parser: o primeiro pedaço não é inteiro, gerando `ValueError` capturado e retorno `None`. Referência: `app/ponto/juiz_batida.py:75-78`. O produtor guarda ISO em `lampadas[*]['instante']`, chave distinta de `marcos`. Referências: `app/ponto/management/commands/gerar_celulas.py:68-69`, `:83-103`.

- Não há condição especial de “último dia da competência” em `_time` ou no fornecimento de `s`: `_time` recebe somente horário, e `periodos_do_dia` lê os mesmos campos sem consultar limites da janela. Referências: `app/ponto/juiz_batida.py:81-83`, `:149-176`. O fechamento com corte 21 termina no dia 20: `app/ponto/janelas.py:10-21`, `:54-57`.

- **Risco concreto de rotulagem enganosa na sonda:** o handler de `_papeis_da_ata` está dentro de `for _d, cel in celulas.items()`, e não possui uma variável de dia chamada `k/data/dia/data_turno`. Referência: `app/ponto/turnos.py:1629-1633`. O `contexto` anexado não lê `_d` nem `cel.data`; pode subir até o frame de `e6_oraculo`, onde `k` conserva o último dia após terminar a comparação anterior e antes de começar o espelho do próximo colaborador. Referências: anexo `script2.py`, função `contexto` (**numeração do anexo a cargo do pai**); `app/ponto/management/commands/e6_oraculo.py:302-304`, `:319-323`. Portanto o resumo não prova que a célula que falhou seja do último dia; a ocorrência efetiva desse mascaramento é **[não verificado]** sem stack/frame registrado.

- No caminho do motor, a variável local é realmente `dia`, portanto há um contexto capturável mais próximo: `app/ponto/motor_calculo_v2.py:611-621`. Isso impede afirmar que *todos* os rótulos sejam falsos; é necessário separar os caminhos.

## 06/11 11:52 · sessao c29238ef76c4 · E-SEM-VIVA-B · verificador · origem=Code

**Alta — procedente: a volta 2 corrige o veto por rescisão, mas não demonstra a equivalência pedida com a exportação efetiva.** A C5 consulta a autoridade da rescisão; continua sem consultar os demais portões de `classificar_export` e os filtros do retentor. (`app/escala/services/cadastro_realidade.py:163–171`; `app/folha/export.py:858–876,960,1192–1206,1245–1254`.)

Execução dos testes, execução dos mutantes, contagens de produção e sequência suíte → opus → pouso: **[não verificado]**. [nao verificado]

1. **Informativa — procedente: escritores e leitores.**

   | Campo/função | Quem escreve ou produz | Quem lê |
   |---|---|---|
   | `CelulaDia.veredito` | Porta `lavrar_veredito` atribui/persiste; método do modelo delega. Cartório fornece `nunca_bateu`; decisão humana também usa a porta. (`app/ponto/portas/celula.py:814–819`; `app/escala/models.py:1819–1828`; `app/ponto/services/cartorio.py:1241–1247`; `app/ponto/services/veredito_celula.py:330`.) | C5, retenção, adesão, vigia do fio, Dia da Frota e apresentação da decisão. (`app/escala/services/cadastro_realidade.py:163`; `app/folha/export.py:1233–1255`; `app/chamados/services/adesao.py:32–34`; `app/chamados/services/vigia_fio.py:379`; `app/relatorios/dia_da_frota.py:51–60`; `app/ponto/services/veredito_celula.py:107–109`.) |
   | `Colaborador.situacao` | Operação `situacao` da porta, inclusive escrita em lote/criação; desligamento, reativação, INSS e reversor são chamadores. (`app/colaboradores/services/colaborador.py:32,55–93`; `app/colaboradores/services/desligamento.py:104–108,210–214`; `app/ponto/views.py:2433–2435`; `app/ponto/management/commands/reverter_situacao_afastado.py:65–80`.) | Nova seleção dos desligados da C5; portões de emissão; adesão; efetivação dos agendados; C4 e “quem cobrar”. (`app/escala/services/cadastro_realidade.py:166–169,547–551`; `app/ponto/services/cartorio.py:1696–1697,1762`; `app/chamados/services/adesao.py:33,66–67`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38–40`; `app/escala/services/furos_diarios.py:259`.) |
   | `Colaborador.data_demissao` | Mesma operação da porta. Agendamento preenche sem desligar; desligamento preenche; reativação limpa. (`app/colaboradores/services/colaborador.py:32,55–76`; `app/colaboradores/services/desligamento.py:82–86,104–108,210–214`.) | C5 carrega; `sai_pela_rescisao` compara; retentor corta e reconhece o dia da demissão; cadastro do dia e cron dos agendados também leem. (`app/escala/services/cadastro_realidade.py:166–167,460`; `app/folha/export.py:60,1200–1201,1229`; `app/ponto/services/cartorio.py:1096`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38–45`.) |
   | `colabs_nunca_bateu` | Produz `{id: [datas]}` em memória, sem persistência. (`app/escala/services/cadastro_realidade.py:160–172`.) | Leitores produtivos diretos localizados: `_nunca_bateu` e `vigia_retido_calado.censo`. (`app/escala/services/cadastro_realidade.py:178`; `app/folha/vigia_retido_calado.py:42–48`.) |
   | `sai_pela_rescisao` | Função pura: `data_demissao != None` e `data_demissao <= fim`. **Não lê situação.** (`app/folha/export.py:54–60`.) | Classificador, resumo do fechamento e nova C5. A restrição a desligados pertence à C5. (`app/folha/export.py:858`; `app/ponto/services/fechamento_lista.py:51,103–109`; `app/escala/services/cadastro_realidade.py:166–169`.) |
   | `janela_atual/anterior` | Funções puras do motor; corte vem da empresa, com clamp. (`app/ponto/janelas.py:10–72`.) | Nesta cadeia: C5 consulta anterior e competência de cada dia; lavrador e montador de Retratos consultam ambas. Exportação usa `janela_fechamento` do mesmo motor. (`app/escala/services/cadastro_realidade.py:162,169`; `app/folha/management/commands/lavrar_retrato_folha.py:46,60`; `app/folha/retrato_folha.py:1284,1291`; `app/folha/export.py:47–51`.) |
   | `vigia_retido_calado.censo` | Lê Retratos e C5, produz contadores em memória. (`app/folha/vigia_retido_calado.py:31–57`.) | Command chama; `linhas()` formata o resultado. (`app/folha/management/commands/vigiar_retido_calado.py:14,31–36`; `app/folha/vigia_retido_calado.py:60–66`.) |
   | Command `vigiar_retido_calado` | Lê a data, chama censo, imprime e eventualmente encerra com código 2; não persiste. (`app/folha/management/commands/vigiar_retido_calado.py:28–40`.) | Cron registrado, dependente de `lavrar_retrato_folha`. Execução efetiva: **[não verificado]**. (`app/config/crons.py:1641–1643`.) |

   Outros consumidores das janelas incluem cadastro de tipo, furos diários, cartório e pré-fechamento; essas funções não foram alteradas pelo diff fornecido. (`app/escala/services/cadastro_tipo.py:336`; `app/escala/services/furos_diarios.py:68,76`; `app/ponto/services/cartorio.py:264,325`; `app/folha/management/commands/pre_fechamento.py:73,89`.) Exaustividade absoluta de acessos dinâmicos aos campos: **[não verificado]**.

2. **Alta — procedente: a “testemunha” do teste ainda não é `classificar_export`.** [nao verificado]

   O helper chama somente `sai_pela_rescisao` e `motivos_retencao_celula`; o classificador real exige fechamento e pode sair por `sem_codigo_dominio`, `ferias_com_batida` ou `cadastro_zero` antes de consultar o espelho. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:49–60`; `app/folha/export.py:775–780,858–876,951–960`.)

   **Contraexemplo das próprias fixtures:** A tem demissão em 22/10, e a C5 exige 05/10 e 20/10. Sua criação não informa `codigo_dominio`, cujo default é vazio. Sem fechamento, não existe item na classificação padrão; acrescentando um fechamento, ele sairia por `sem_codigo_dominio`, antes de `espelho_nunca_bateu`. Portanto a igualdade testada não prova a igualdade perguntada. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:31–43,73–84`; `app/colaboradores/models.py:311–313`; `app/folha/export.py:775–780,864–867`.)

   Mesmo reduzindo a comparação a **rescisão + retentor**, permanecem estes contraexemplos sintéticos:

   | Caso | Divergência e lado responsável |
   |---|---|
   | Demissão 22/10; admissão 10/10; célula NB em 05/10 | C5 inclui 05/10; retentor corta por admissão. (`app/escala/services/cadastro_realidade.py:163–171`; `app/folha/export.py:1198–1206`.) |
   | Desligado isento, demissão posterior ao fim da competência | C5 inclui NB; retentor devolve vazio. (`app/escala/services/cadastro_realidade.py:163–171`; `app/folha/export.py:1192–1196`.) |
   | NB com protestos exclusivamente `FURO_PRE_ADOCAO` | C5 inclui; retentor dispensa. A ocorrência dessa combinação em produção é **[não verificado]**. (`app/escala/services/cadastro_realidade.py:163`; `app/ponto/supra_juiz.py:34–36`; `app/folha/export.py:1245–1254`.) |
   | Demissão nula ou após o fim; célula NB hoje/futura | C5 exclui pelo limite ontem; retentor pode incluir célula acusadora até o fim recebido. O teste esconde essa diferença mediante `< HOJE`. (`app/escala/services/cadastro_realidade.py:158,164`; `app/folha/export.py:1225–1255`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:59`.) |

   Essas divergências não autorizam alterar o ativo: o teste exige expressamente que ativo com demissão preenchida continue sem corte na C5. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:93–98`.)

3. **Informativa — procedente: o portão de rescisão coincide nas bordas; isso não elimina o achado anterior.**

   Nos exemplos abaixo, hoje = 25/10/2026 e corte = 21: anterior 21/09–20/10; corrente 21/10–20/11. “Entra” significa célula NB existente até 24/10, antes dos demais filtros da exportação. (`app/ponto/janelas.py:10–33,60–72`; `app/escala/services/cadastro_realidade.py:162–171`.)

   | Demissão do desligado | C5 | Exportação pelo portão de rescisão |
   |---|---|---|
   | 21/09: primeiro dia da anterior | Nenhuma das duas | Ambas saem por rescisão. |
   | 20/10: último dia da anterior | Nenhuma das duas | Ambas saem por rescisão. |
   | 21/10: primeiro dia da corrente | Só anterior | Corrente sai por rescisão. |
   | 20/11: último dia da corrente | Só anterior | Corrente sai por rescisão. |
   | 30/10: futura, dentro da corrente | Só anterior | Corrente já sai por rescisão, mesmo antes da demissão. |
   | 21/11: futura, depois da corrente | Ambas | Nenhuma sai por rescisão. |
   | Nula | Ambas | Nenhuma sai por rescisão. |

   Todas as linhas decorrem da mesma comparação `demissao <= fim`, chamada pela C5 e pelo classificador. (`app/folha/export.py:60,858–863`; `app/escala/services/cadastro_realidade.py:169`.)

   **Corte diferente de 21:** com corte 10, hoje 25/10 e demissão 15/10, anterior = 10/09–09/10 e corrente = 10/10–09/11. Só a anterior passa; célula de 20/10 é excluída. Não há divergência de rescisão entre C5 e exportação nesse estado. (`app/ponto/janelas.py:10–33,36–72`; `app/escala/services/cadastro_realidade.py:162,169`; `app/folha/export.py:47–60,807,858`.)

   **Troca de empresa — média, dúvida quanto a defeito novo:** A/corte21 → B/corte10, demissão 15/10, célula 05/10: antes da troca a C5 exclui; depois inclui. O classificador padrão também usa a empresa atual, logo isso não prova divergência entre ambos no mesmo estado. Já o cruzamento com Retrato histórico mistura fotografia gravada e C5 atual. Ocorrência real: **[não verificado]**. (`app/escala/services/cadastro_realidade.py:159–169`; `app/folha/export.py:775–780,807`; `app/folha/vigia_retido_calado.py:33–48`; `app/colaboradores/views.py:1313,1340–1342`.)

4. **Informativa — procedente a remoção de `data__gt=demissao`, nas condições efetivas da função.**

   Para desligado com demissão preenchida, seja `F` o fim de `janela_atual(d, emp)`. O motor garante `d ≤ F`; sobreviver ao veto exige `demissao > F`; logo **`d < demissao`**. Assim, nenhum dia posterior — nem o próprio dia da demissão — sobrevive. Com demissão nula, o corte antigo também não operava. Não há contraexemplo lógico nesse domínio, considerando o mesmo estado cadastral. (`app/ponto/janelas.py:24–33,60–63`; `app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:166–171`.)

5. **Média — procedente: os achados de roteamento da v1 permanecem.**

   C5 transporta dias apenas no texto, sem chave `data`; `dias_na_lista()` exige essa chave. Portanto aparecer por C5 não torna o dia visível a `dia_do_admin()` nem cria o balde `fio_do_admin`. (`app/escala/services/cadastro_realidade.py:178–180,980–986`; `app/chamados/services/vigia_fio.py:222–228,313–314`.)

   O vigia de retidos faz o inverso: descarta todas as datas e usa somente IDs. Basta qualquer dia C5 na janela para rotear **cada linha bloqueada/`ninguem`** dessa pessoa, independentemente de competência ou motivo. A fixture usa inclusive `espelho_furo`, não `espelho_nunca_bateu`, e exige roteamento. (`app/folha/vigia_retido_calado.py:43–55`; `app/folha/tests/test_vigia_retido_calado.py:18,79–89`.) Ocorrência e quantidade em produção: **[não verificado]**.

6. **Média — procedente em parte: situação dos “curados” e descartes da triagem.**

   | Item da v1/triagem | Veredito desta volta |
   |---|---|
   | **1, 2 e 4 — curados** | **Cura parcial.** Rescisão agora vem da autoridade correta; equivalência geral de dias continua falsa pelos filtros e portões ausentes. (`app/escala/services/cadastro_realidade.py:163–171`; `app/folha/export.py:864–876,1192–1206,1245–1254`.) |
   | **3 — divergências seriam preexistentes do ativo** | Preservar ativo procede; isso não demonstra a condição exigida para o desligado. A declaração de equivalência continua sem esses filtros. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:80–98`; `app/escala/services/cadastro_realidade.py:163–171`.) |
   | **7 e 8 — reconhecidos e enviados à fila** | Permanecem tecnicamente procedentes. A aceitação de deixá-los pendentes nesta obra é **[não verificado]**; a regra documental manda encaminhar descartes ao opus. (`app/escala/services/cadastro_realidade.py:178–180,983`; `app/folha/vigia_retido_calado.py:43–48`; `CLAUDE.md:367–369`.) |
   | **9 — hoje/futuro fora do aval** | O limite até ontem está declarado e testado; procede como limitação explícita, não como prova de igualdade sem esse recorte. O item 9 original também tratava dos consumidores, cujos efeitos permanecem abaixo. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:9,59`; `app/escala/services/cadastro_realidade.py:164,808–814`.) |
   | **10 — descartado** | Procede descartar novo push pela C5. Não procede concluir ausência de consumidores escritores: o vigia do fio pode persistir contadores. (`app/escala/services/cadastro_realidade.py:163–180`; `app/chamados/management/commands/vigiar_fio_morto.py:210–226`.) |
   | **11 — descartado** | Procede descartar um filtro posterior que anulasse C5. Continuam existindo universos independentes que excluem desligado. (`app/escala/services/cadastro_realidade.py:707,829–832`; `app/chamados/services/adesao.py:32–34,66–67`; `app/relatorios/dia_da_frota.py:34–35`.) |
   | **12 — RED novo** | Tem mordida contra remover o veto de rescisão, mas permanece cobertura incompleta; mutantes abaixo. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:73–90`.) |
   | **13 — relógio curado** | Curado no caminho command → censo → C5. O fallback interno continua executável quando `hoje=None`. (`app/folha/management/commands/vigiar_retido_calado.py:31`; `app/folha/vigia_retido_calado.py:27,43`; `app/escala/services/cadastro_realidade.py:157`.) |
   | **14 — descartado** | Procede para novo escritor/trava: os trechos produtivos alterados só consultam e montam resultados. (`app/escala/services/cadastro_realidade.py:159–172`; `app/folha/vigia_retido_calado.py:33–57`.) |
   | **15 — respondido como preservação do ativo** | Essa preservação está testada, mas não responde à dúvida original sobre os pré-requisitos de pouso. Sequência efetiva: **[não verificado]**. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:93–98`; `CLAUDE.md:369–373`.) |
   | **16 — citação corrigida** | **Curado:** o teste agora aponta ao corte efetivo `1198–1201`. Correção no corpo do commit: **[não verificado]**. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:5–6`; `app/folha/export.py:1198–1201`.) |

7. **Média — procedente: classes de reprovação e mordida dos testes.** [nao verificado]

   | Classe | Presente ou ausente |
   |---|---|
   | Segundo juiz duplicando registrado | **Ausente para a pergunta de rescisão:** delega corretamente. **Dúvida quanto ao rótulo “segundo juiz”**, mas permanece decisão parcial própria de “folha segura”, cuja autoridade registrada é `classificar_export`. (`app/escala/services/cadastro_realidade.py:169`; `app/core/juizes.py:288–299`; `app/folha/export.py:858–960`.) |
   | Segundo escritor fora da porta | **Ausente no diff produtivo.** Escritas novas são fixtures. (`app/escala/services/cadastro_realidade.py:159–172`; `app/folha/vigia_retido_calado.py:33–57`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:31–34,88,97`.) |
   | Relógio fora da borda | **Ausente no caminho produtivo corrigido; fallback presente.** (`app/folha/management/commands/vigiar_retido_calado.py:31`; `app/folha/vigia_retido_calado.py:43`; `app/escala/services/cadastro_realidade.py:157`.) |
   | Derivação paralela de fonte única | **Presente residualmente** para a alegação de equivalência: C5 seleciona os dias sem ler os dias do retentor nem toda a classificação. (`app/escala/services/cadastro_realidade.py:163–171`; `app/folha/export.py:1178–1185,1192–1254`; `CLAUDE.md:20–22`.) |
   | Selo/RED sem mordida | **Presente parcialmente**, pelos sobreviventes abaixo; não é ausência total de mordida. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:41–43,63–104`; `app/folha/tests/test_vigia_retido_calado.py:58–66,115–120`.) |
   | Ordem de trava | **Ausente nova aquisição/inversão de trava de banco.** A incompletude da precedência de classificação permanece no helper testemunha. (`app/escala/services/cadastro_realidade.py:159–172`; `app/folha/vigia_retido_calado.py:33–57`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:55–59`; `app/folha/export.py:858–960`.) |
   | Pouso antes do pré-requisito | **Dúvida; [não verificado].** O requisito existe, mas os anexos não comprovam sua execução. (`CLAUDE.md:369–373`.) | [nao verificado]

   Pela inspeção das asserções, restaurar a exclusão total dos desligados, retirar `sai_pela_rescisao`, cortar ativo ou excluir desligado sem data seria distinguido pelos testes alterados. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:63–98`.)

   Estes mutantes relevantes **não seriam distinguidos pelos testes alterados**, por inspeção; sobrevivência executada e na suíte inteira: **[não verificado]**: [nao verificado]

   - **Ignorar a empresa:** `janela_atual(d, emp)` → `janela_atual(d)`. Todas as empresas dessas fixtures usam corte padrão 21. (`app/escala/services/cadastro_realidade.py:169`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:41`; `app/folha/tests/test_vigia_retido_calado.py:76`; `app/colaboradores/models.py:46`.)
   - **Não excluir demissão exatamente no fim:** substituir o veto por comparação estrita `< fim`. As demissões testadas são internas, 10/10 e 22/10. (`app/folha/export.py:60`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:24–28,41–43`.)
   - **Retirar a injeção somente do command:** voltar a `censo(emps)`. O teste novo chama censo diretamente; os testes do command não possuem pessoa C5. (`app/folha/management/commands/vigiar_retido_calado.py:31`; `app/folha/tests/test_vigia_retido_calado.py:58–66,115–120`.)
   - **Copiar inline o predicado da rescisão:** resultado semanticamente igual continuaria passando; os testes novos não cobram que a autoridade seja chamada. (`app/escala/services/cadastro_realidade.py:169`; `app/folha/export.py:60`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:73–90`.)

8. **Média — procedente: outros leitores mudam, com estes limites quantitativos.**

   Para o mesmo `hoje` e estado cadastral, o novo filtro remove os dias até a demissão que a versão anterior mantinha em competências cujo fim é `>= demissao`; ativos e demissão nula permanecem iguais. A regra atual está em `app/escala/services/cadastro_realidade.py:163–171` e `app/folha/export.py:60`; o corte anterior consta apenas do anexo fornecido, sem arquivo numerado nesta cópia: **[não verificado]**.

   | Consumidor | Quanto muda |
   |---|---|
   | `_nunca_bateu` / `lista()` | Reduz quantidade/datas no detalhe; perde uma C5 se perder o último dia. Pode perder uma pessoa se C5 for sua única assinatura. C5 conta uma ocorrência por assinatura, não por dia. (`app/escala/services/cadastro_realidade.py:178–180,808–814,860–863,872–904`.) |
   | Tela e PDF | Recebem a lista alterada e exibem suas assinaturas. (`app/escala/views.py:484–492`; `app/templates/escala/cadastro_x_realidade.html:28`; `app/escala/services/cadastro_realidade.py:1019–1030`.) |
   | `vigia_retido_calado` | Para cada ID que perde o último dia C5: `roteados −k`, `calados +k`, sendo `k` suas linhas bloqueadas/`ninguem` nos Retratos lidos. `bloqueados` e `cego` não mudam para os mesmos Retratos. (`app/folha/vigia_retido_calado.py:43–56`.) |
   | Vigia do fio | C5 não altera o mapa de dias; pode alterar a presença da pessoa. Perdendo a última assinatura, pode trocar causa **d → e**, quando satisfeitas as demais condições. (`app/escala/services/cadastro_realidade.py:178–180,980–986`; `app/chamados/services/vigia_fio.py:216–218,677–686`.) |
   | `vigiar_fio_morto` | Os contadores alterados podem chegar ao carimbo JSON da rodada de frota; com `--empresa`, não carimba. (`app/chamados/management/commands/vigiar_fio_morto.py:127–134,210–230`.) |
   | Fechadores de avisos/vínculos | **Variação de seleção por C5 = zero:** selecionam exclusivamente C7 e C8. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27–37`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35–45`.) |
   | `placar_estrutural` | **Variação automática = zero:** referência à lista é texto; `placar

[truncado em 20000 de 20974 caracteres]

## 06/11 11:50 · sessao b00132262b39 · R1-CAUSAD-V3 · leitura · origem=Code

```python
import collections
import datetime as dt
import gc
import logging
import math
import sys
import time
from contextlib import redirect_stdout, redirect_stderr
from decimal import Decimal
from unittest.mock import Mock, patch

from django.conf import settings
from django.db import connection, reset_queries, transaction
from django.utils import timezone

from ponto.calculador import regras
from ponto.catalogo import ausencias as CAT
from ponto.management.commands import diff_calculador as DC
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.models import DiaPago
from ponto.services import dia_pago as DP
from ponto.services import fechamento as F
PARTE = 1
PARTES = 6

# Proposta nao executada; duracao, memoria e equivalencia: [nao verificado].
# Para exec() no shell ja configurado pelo envelope de leitura.
# Envelope sem bytecode: bin/sonda_leitura.sh:237.
sys.dont_write_bytecode = True

COMPETENCIAS = ((2026, 10), (2026, 11))
RUBRICAS = tuple(r for r in regras.RUBRICAS if r not in regras.NAO_DECIDE)
DONOS = {"BATIDA", "CADASTRO", "ESTRUTURA"}
LIMIAR_H = 0.02  # app/ponto/management/commands/diff_calculador.py:538
INICIO = time.monotonic()
PRAZO = INICIO + 15 * 60
HOJE, AGORA = timezone.localdate(), timezone.now()

ERROS = []
SOMAS = []
COMP_ATUAL = None
MARCA_ERRO = object()
REAL_RECALC = F.recalcular_fechamento_mes
REAL_LAVRAR = DP.lavrar
COD_COMANDO = DC.Command.handle.__code__
RAIZ_APP = DC.__file__.split("/ponto/management/commands/")[0] + "/"


class LacunaDeMedicao(Exception):
    pass


class ConservacaoNaoFecha(Exception):
    pass


class PrazoEsgotado(BaseException):
    pass


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass

    def isatty(self):
        return False


DESCARTAR = Descartar()


def conferir_prazo():
    # Prazo cooperativo; nao interrompe uma consulta bloqueada [nao verificado].
    if time.monotonic() >= PRAZO:
        raise PrazoEsgotado()


def contexto(frame):
    cid = dia = None
    while frame is not None:
        loc = frame.f_locals
        if cid is None:
            for nome in ("colab", "colaborador", "c", "_c"):
                obj = loc.get(nome)
                pk = getattr(obj, "pk", None)
                if isinstance(pk, int):
                    cid = pk
                    break
        if dia is None:
            for nome in ("data", "dia", "_dia", "k"):
                valor = loc.get(nome)
                if isinstance(valor, dt.datetime):
                    dia = valor.date()
                    break
                if isinstance(valor, dt.date):
                    dia = valor
                    break
        frame = frame.f_back
    return cid, dia


def falha(exc, cid=None, dia=None, tb=None):
    # Deduplicacao sem guardar exc/tb/frame numa colecao global.
    # Especificacao desta reescrita, sem execucao [nao verificado].
    if getattr(exc, "_r1_causad_marca", None) is MARCA_ERRO:
        return
    exc._r1_causad_marca = MARCA_ERRO
    tb = tb if tb is not None else exc.__traceback__
    sitio = "sonda"
    if tb is not None:
        while tb.tb_next is not None:
            tb = tb.tb_next
        sitio = "%s:%d" % (
            tb.tb_frame.f_code.co_filename.rsplit("/", 1)[-1],
            tb.tb_lineno,
        )
    tipo = type(exc).__name__
    ultima = ("%s: [texto suprimido pelo contrato]" % tipo)[:200]
    ERROS.append((COMP_ATUAL, cid, dia, tipo, sitio, ultima))


def limpar(cid=None):
    # Cache de modulo de catalogo, nao cache por colaborador:
    # app/ponto/catalogo/ausencias.py:257,261.
    # Os caches locais morrem com a chamada por colaborador:
    # diff_calculador.py:95,116; fechamento.py:923;
    # motor_calculo_v2.py:2285.
    for funcao in (CAT.invalidar_cache, reset_queries, gc.collect):
        try:
            funcao()
        except Exception as exc:
            falha(exc, cid)


def numero(valor):
    valor = float(valor)
    if not math.isfinite(valor):
        raise LacunaDeMedicao()
    return valor


def soma(xs):
    return math.fsum(xs)


def igual(a, b):
    return math.isclose(a, b, rel_tol=1e-12, abs_tol=1e-8)


def centavos(valor):
    return Decimal(str(round(float(valor), 2)))


def executar(ano, mes, alvo=None):
    # alvo=None: descobre o universo pelo comando real, sem calcular colabs.
    # alvo inteiro: uma chamada real, limitada a esse colaborador.
    # Instrumentacao proposta, sem execucao [nao verificado].
    c = {
        "ids": set(), "janelas": {}, "colabs": {}, "fechos": {},
        "emitido": {"motor": {}, "oraculo": {}},
        "porta_v3": {}, "cegos": {}, "donos": {},
        "historico": None, "comando_completo": False,
        "cortes": 0, "ok": True,
    }
    erros_antes = len(ERROS)

    def observar(frame, evento, arg):
        if not frame.f_code.co_filename.startswith(RAIZ_APP):
            return None
        frame.f_trace_lines = frame.f_code is COD_COMANDO

        if evento == "exception":
            tipo, exc, tb = arg
            if not issubclass(
                tipo, (StopIteration, StopAsyncIteration, GeneratorExit)
            ):
                cid, dia = contexto(frame)
                falha(exc, cid, dia, tb)

        if frame.f_code is COD_COMANDO:
            if evento == "line":
                conferir_prazo()
                # Lista real ja selecionada e antes do fechamento/laco:
                # app/ponto/management/commands/diff_calculador.py:102,116,140,145.
                if frame.f_lineno == 116:
                    lista = frame.f_locals["ids"]
                    if not isinstance(lista, list):
                        raise LacunaDeMedicao()
                    c["cortes"] += 1
                    if alvo is None:
                        if any(not isinstance(cid, int) for cid in lista):
                            raise LacunaDeMedicao()
                        if len(set(lista)) != len(lista):
                            raise LacunaDeMedicao()
                        if c["ids"].intersection(lista):
                            raise LacunaDeMedicao()
                        c["ids"].update(lista)
                        lista[:] = []
                    else:
                        lista[:] = [
                            cid for cid in lista
                            if cid == alvo and cid % PARTES == PARTE - 1
                        ]
                        if len(lista) > 1 or c["ids"].intersection(lista):
                            raise LacunaDeMedicao()
                        c["ids"].update(lista)

                if frame.f_lineno == 550:
                    c["comando_completo"] = True

            elif evento == "return" and "S" in frame.f_locals:
                loc = frame.f_locals
                c["historico"] = {
                    "S": {r: tuple(v) for r, v in loc["S"].items()},
                    "T": dict(loc["T"]),
                }
        return observar

    def fechamento_capturado(m, a, **kw):
        conferir_prazo()
        chamador = sys._getframe(1)
        if chamador.f_code is not COD_COMANDO:
            raise LacunaDeMedicao()
        loc = chamador.f_locals
        ids = set(loc["ids"])

        # Vazio nunca cai na chamada original por empresa inteira.
        # Chamada original: diff_calculador.py:140.
        if not ids:
            return {}
        if alvo is None or ids != {alvo}:
            raise LacunaDeMedicao()
        if not kw.get("somente_leitura") or not kw.get("lavrar_dias_pagos"):
            raise LacunaDeMedicao()

        ini, fim = loc["ini"], loc["fim"]
        for cid in ids:
            c["janelas"][cid] = (ini, fim)

        def emissor_capturado(colab, mm, aa, **dados):
            conferir_prazo()
            cid = colab.pk
            versao = dados.get("versao", "motor")
            if cid not in ids or versao not in ("motor", "oraculo"):
                raise LacunaDeMedicao()
            capturadas = []

            def capturar_bulk(objetos, *args, **kwargs):
                objetos = list(objetos)
                capturadas.extend(objetos)
                return objetos

            apagar = Mock()
            apagar.delete.return_value = (0, {})
            manager = DiaPago.objects

            # Dois sitios de persistencia do emissor real:
            # app/ponto/services/dia_pago.py:357,358.
            with transaction.atomic():
                try:
                    with patch.object(manager, "filter", return_value=apagar), \
                         patch.object(manager, "bulk_create", new=capturar_bulk):
                        n = REAL_LAVRAR(colab, mm, aa, **dados)
                finally:
                    transaction.set_rollback(True)

            if n != len(capturadas):
                raise ConservacaoNaoFecha()

            c["colabs"][cid] = colab
            for obj in capturadas:
                if obj.tipo != DiaPago.TIPO_DIA:
                    continue
                chave = (cid, obj.data)
                if chave in c["emitido"][versao]:
                    raise LacunaDeMedicao()
                c["emitido"][versao][chave] = {
                    r: numero(getattr(obj, r)) for r in RUBRICAS
                }

            if versao == "oraculo":
                rubricas = dados.get("rubricas")
                if rubricas is None:
                    raise LacunaDeMedicao()
                for dia, rub in rubricas.items():
                    chave = (cid, dia)
                    if chave in c["porta_v3"]:
                        raise LacunaDeMedicao()
                    c["porta_v3"][chave] = {
                        r: numero(v) for r, v in rub.valores.items()
                        if r in RUBRICAS
                    }
                    c["cegos"][chave] = set(rub.nao_decide or ())
            return n

        # Restricao oficial antes dos lotes; retorno do proprio fechamento:
        # app/ponto/services/fechamento.py:548,596,1119.
        kw["colaborador_ids"] = sorted(ids)
        kw["retornar_objeto"] = True
        with patch.object(DP, "lavrar", new=emissor_capturado):
            retorno = REAL_RECALC(m, a, **kw)

        if set(retorno) - ids:
            raise LacunaDeMedicao()
        for cid, objeto in retorno.items():
            c["fechos"][cid] = {
                r: numero(getattr(objeto, r)) for r in RUBRICAS
            }
        return retorno

    trace_anterior = sys.gettrace()
    with transaction.atomic():
        try:
            with redirect_stdout(DESCARTAR), redirect_stderr(DESCARTAR), \
                 patch.object(logging.Logger, "handle", return_value=None):
                sys.settrace(observar)
                try:
                    conferir_prazo()
                    with patch.object(
                        F, "recalcular_fechamento_mes", new=fechamento_capturado
                    ):
                        DC.Command(
                            stdout=DESCARTAR, stderr=DESCARTAR, no_color=True
                        ).handle(
                            mes=mes, ano=ano, csv=None, limite_colabs=0,
                            colab="", abrir_he50=False, contra_gravado=False,
                            periodos_do_fechamento=True,
                            pares_da_autoridade=True,
                        )

                    if (
                        not c["comando_completo"]
                        or c["historico"] is None
                        or (alvo is not None and c["ids"] != {alvo})
                    ):
                        raise LacunaDeMedicao()

                    for nome, quantidade in c["historico"]["T"].items():
                        if quantidade and (
                            nome.startswith("erro_")
                            or nome in {
                                "sem_resultado_do_fechamento",
                                "troca_de_periodos_FALHOU",
                                "dia_sem_rubrica_da_porta",
                            }
                        ):
                            falha(LacunaDeMedicao(), alvo)

                    if alvo is not None:
                        for versao in ("motor", "oraculo"):
                            if not c["emitido"][versao]:
                                falha(LacunaDeMedicao(), alvo)
                        if alvo not in c["fechos"]:
                            falha(LacunaDeMedicao(), alvo)

                        chaves = sorted(
                            set(c["emitido"]["motor"])
                            | set(c["emitido"]["oraculo"])
                        )
                        insumo = None
                        for cid, dia in chaves:
                            chave = (cid, dia)
                            v2 = c["emitido"]["motor"].get(chave)
                            v3 = c["emitido"]["oraculo"].get(chave)
                            if v2 is None or v3 is None:
                                falha(LacunaDeMedicao(), cid, dia)
                                continue
                            valores = c["porta_v3"].get(chave, {})
                            cegos = c["cegos"].get(chave, set())
                            if not any(
                                r in valores and r not in cegos
                                and abs(v3[r] - v2[r]) > LIMIAR_H
                                for r in RUBRICAS
                            ):
                                continue
                            conferir_prazo()
                            try:
                                ini, fim = c["janelas"][cid]
                                if not ini <= dia <= fim:
                                    raise LacunaDeMedicao()
                                with transaction.atomic():
                                    try:
                                        if insumo is None:
                                            insumo = InsumosDoDono(
                                                c["colabs"][cid], ini, fim,
                                                HOJE, agora=AGORA,
                                            )
                                        dono = insumo.dono(dia)
                                    finally:
                                        transaction.set_rollback(True)
                                if dono not in DONOS:
                                    raise LacunaDeMedicao()
                                c["donos"][chave] = dono
                            except Exception as exc:
                                falha(exc, cid, dia)
                                c["donos"][chave] = "SEM_DONO"
                        insumo = None
                finally:
                    sys.settrace(trace_anterior)
        except (Exception, PrazoEsgotado) as exc:
            c["ok"] = False
            falha(exc, alvo)
        finally:
            transaction.set_rollback(True)

    c["ok"] = c["ok"] and len(ERROS) == erros_antes
    return c


def balde():
    # Guarda escalares por colaborador e IDs, sem modelos/dias/tracebacks.
    # Especificacao desta reescrita [nao verificado].
    return {"a": [], "b": [], "d": [], "cols": set(), "dias": 0}


def guardar(destino, linhas):
    if not linhas:
        return
    destino["a"].append(soma(x[1] for x in linhas))
    destino["b"].append(soma(x[2] for x in linhas))
    destino["d"].append(soma(x[2] - x[1] for x in linhas))
    destino["cols"].update(x[0][0] for x in linhas)
    destino["dias"] += len(linhas)


def novo_estado():
    return {
        r: {
            "comparados": balde(),
            "grupos": collections.defaultdict(balde),
            "residuos": collections.defaultdict(balde),
            "autoridade2": [], "autoridade3": [],
            "porta": [], "emitido_decidido": [],
            "historico2": [], "historico3": [],
            "fm_dias": Decimal("0"), "fm_retorno": Decimal("0"),
            "fm_ok": True,
        }
        for r in RUBRICAS
    }


def acumular(comp, cid, c, estado):
    e2, e3 = c["emitido"]["motor"], c["emitido"]["oraculo"]
    chaves = sorted(set(e2) | set(e3))

    for r in RUBRICAS:
        s = estado[r]
        grupos = collections.defaultdict(list)
        residuos = collections.defaultdict(list)
        comparados = []
        porta = []
        emitido_decidido = []

        for chave in chaves:
            col, dia = chave
            if col != cid:
                raise LacunaDeMedicao()
            a, b = e2.get(chave), e3.get(chave)
            if a is None or b is None:
                residuos["SEM_PAR"].append(
                    (chave, a[r] if a else 0.0, b[r] if b else 0.0)
                )
                continue
            valores = c["porta_v3"].get(chave, {})
            cegos = c["cegos"].get(chave, set())
            if r in cegos or r not in valores:
                residuos["SEM_DECISAO"].append((chave, a[r], b[r]))
                continue

            porta.append(valores[r])
            emitido_decidido.append(b[r])
            linha = (chave, a[r], b[r])
            comparados.append(linha)
            if abs(b[r] - a[r]) <= LIMIAR_H:
                residuos["ATE_LIMIAR"].append(linha)
                continue

            dono = c["donos"].get(chave, "SEM_DONO")
            grupos[dono].append(linha)
            print("DIA|%s|%s|col%d|%s|%s|%.9f|%.9f|%+.9f" % (
                comp, r, cid, dia, dono, a[r], b[r], b[r] - a[r]
            ))

        guardar(s["comparados"], comparados)
        for dono, linhas in grupos.items():
            guardar(s["grupos"][dono], linhas)
        for classe, linhas in residuos.items():
            guardar(s["residuos"][classe], linhas)

        s["autoridade2"].append(soma(v[r] for v in e2.values()))
        s["autoridade3"].append(soma(v[r] for v in e3.values()))
        s["porta"].append(soma(porta))
        s["emitido_decidido"].append(soma(emitido_decidido))

        # Arredondamento por colaborador, preservando o contrato do anexo.
        # Fechamento real devolvido em fechamento.py:1119.
        dias = centavos(soma(v[r] for v in e2.values()))
        fecho = c["fechos"].get(cid)
        retorno = centavos(fecho[r]) if fecho is not None else Decimal("0")
        s["fm_dias"] += dias
        s["fm_retorno"] += retorno
        fm_ok = bool(e2) and fecho is not None and dias == retorno
        s["fm_ok"] = s["fm_ok"] and fm_ok
        if not fm_ok:
            falha(ConservacaoNaoFecha(), cid)

        hist = c["historico"]
        if hist is not None and r in hist["S"]:
            h2, h3 = hist["S"][r]
            s["historico2"].append(numero(h2))
            s["historico3"].append(numero(h3))


def publicar(comp, estado, cobertura):
    for r in RUBRICAS:
        s = estado[r]
        grupos, residuos = s["grupos"], s["residuos"]
        cmp = s["comparados"]
        h2, h3 = soma(cmp["a"]), soma(cmp["b"])
        cols_div = set().union(*(x["cols"] for x in grupos.values()))
        dias_div = sum(x["dias"] for x in grupos.values())

        print(
            "PARCIAL|%s|%s|v2_h=%.17g|v3_h=%.17g"
            "|colabs_div=%d|dias_colab_div=%d"
            % (comp, r, h2, h3, len(cols_div), dias_div)
        )
        print("RUBRICA|%s|%s|%.9f|%.9f|%+.9f|%d|%d" % (
            comp, r, h2, h3, h3 - h2, len(cols_div), dias_div
        ))

        for dono, x in sorted(grupos.items()):
            a, b = soma(x["a"]), soma(x["b"])
            print("DONO|%s|%s|%s|%.9f|%.9f|%+.9f|%d|%d" % (
                comp, r, dono, a, b, b - a, len(x["cols"]), x["dias"]
            ))

        for classe, x in sorted(residuos.items()):
            print("RESIDUO|%s|%s|%s|%.9f|%.9f|%+.9f|%d|%d" % (
                comp, r, classe, soma(x["a"]), soma(x["b"]),
                soma(x["d"]), len(x["cols"]), x["dias"]
            ))

        autoridade2 = soma(s["autoridade2"])
        autoridade3 = soma(s["autoridade3"])
        parcelas = list(grupos.values()) + list(residuos.values

[truncado em 20000 de 28296 caracteres]

## 06/11 11:49 · sessao ce1b9b2e3cab · - · - · origem=Ronald (tmux)

| Escopo | Resultado com referências |
|---|---|
| `trabalha` escritores | Regeneração: `app/ponto/portas/celula.py:247`, `:263`, `:276`; reversão: `:781`; geração periódica: `:842`, `:861`; edição nova: `:597`, `:600`. |
| `trabalha` leitores centrais | Resposta de dia previsto em `app/escala/models.py:1094`; juiz de dia sem trabalho em `app/chamados/juizes.py:427`; protesto do cartório em `app/ponto/services/cartorio.py:1408`; no-op e fotografia em `app/ponto/portas/celula.py:527`, `:591`. |
| `dna` escritores | Regeneração `app/ponto/portas/celula.py:248`, `:264`, `:276`; reversão `:779`; geração/enriquecimento `:845`, `:853`, `:863`; edição nova `:598`, `:601`. |
| `dna` leitores centrais | Marcos `app/escala/models.py:1124`; pausas `:1189`; extra declarada `:1235`; hash/ata do cartório `app/ponto/services/cartorio.py:1348`, `:1386`; fotografia/no-op `app/ponto/portas/celula.py:527`, `:591`. |
| `escala_geradora` escritores/leitores | Escrevem regeneração `app/ponto/portas/celula.py:264`, `:276`; reversão `:784`; geração `:844`, `:862`; edição `:598`, `:602`. O juiz do vínculo lê a geradora e resolve sua vigência em `app/escala/alimentacao.py:270`, `:279`, `:282`; a fotografia/no-op lê em `app/ponto/portas/celula.py:528`, `:592`. |
| `origem` escritores | Geração/regeneração criam `gerada` em `app/ponto/portas/celula.py:264`, `:844`; edição cria/grava `editada` em `:597`, `:603`. |
| Todos os leitores funcionais de `origem='editada'` | Regeneração aceita somente `gerada`: `app/ponto/portas/celula.py:242`, `:267`; reversão recusa origem diferente de `gerada`: `:767`; gerador periódico pula `editada`: `:847`; edição idêntica exige `editada`: `:591`. Diagnósticos excluem humanas ao selecionar só `gerada`: `app/escala/management/commands/celulas_contra_o_template.py:29`, `app/ponto/management/commands/tripwire_celulas.py:21`. A escrita nova torna a célula imune aos três caminhos de geração/reversão, exclui-a desses dois diagnósticos e habilita o no-op na repetição, pelas mesmas guardas citadas. |
| Leitores expositivos de `origem` | Fotografia da trilha em `app/ponto/portas/celula.py:527`; resposta de diagnóstico em `app/ponto/services/dia_do_colab.py:90`; representação textual em `app/escala/models.py:1842`. |
| `editada_em` escritores | Somente `editar_dia`, criação/atualização em `app/ponto/portas/celula.py:598`, `:604`; o instante vem de `:595`. |
| Todos os leitores decisórios de `editada_em` | **Pergunta stale:** incorpora o carimbo ao máximo por colaborador em `app/chamados/veredito_lavrado.py:151`; compara esse máximo com `julgado_em` em `:187`. **Fato novo:** devolve True quando o carimbo está no intervalo em `app/ponto/services/cartorio.py:365`. **Janela antiga reaberta:** agrega carimbos em `app/escala/services/fase_12x36.py:188` e reabre se forem posteriores à lavra em `:191`. Uma edição nova pode tornar cada decisão verdadeira; repetição idêntica retorna antes de mover o carimbo em `app/ponto/portas/celula.py:591`. |
| Leitor expositivo de `editada_em` / `editada_por` | Ambos entram na fotografia antes/depois em `app/ponto/portas/celula.py:529`. `editada_por` é gravado em `:598`, `:605`; outro leitor de produção encontrado: nenhum [nao verificado quanto a acesso dinâmico]. |
| `LogAuditoria` | `editar_dia` chama `evento_kw` em `app/ponto/portas/celula.py:608`; `trocar_plantao` em `:675`. Adaptador em `app/core/observ.py:142`; escritor efetivo único em `:134`. Leitor genérico da tela consulta todas as ações em `app/core/views.py:705`, filtra ação em `:709`, apresenta em `:733`. Leitor de negócio específico das duas ações novas: nenhum [nao verificado quanto a acesso dinâmico]. `ACOES` não lista os novos verbos, mas o campo admite verbos livres sem validação de choices no create: `app/core/models.py:286`, `:296`; a tela usa essa lista no seletor em `app/core/views.py:738`. |
| `Ausencia.documento/data/colaborador` | A troca não escreve esses campos: relê cartas travadas em `app/ponto/portas/celula.py:649`, compara colaboradores em `:654`, exige documento em `:656`, passa colaborador/data em `:662`, `:665`, fotografa nome do documento em `:672`. Escritor existente da criação compõe os três em `app/ponto/services/ausencia.py:341`, `:350`; alteração da data em `:499`; anexo em `:857`; reabertura pode substituir documento em `:1128`. Modelos em `app/ponto/models.py:743`, `:750`, `:777`. Cobertura lê colaborador/data/range em `app/ponto/turnos.py:2753`, `:2760`, `:2766`; fato novo consulta colaborador/data em `app/ponto/services/cartorio.py:355`. Documento não vazio é o requisito implementado da troca; existência do arquivo no storage não é comprovada pela verificação booleana de `app/ponto/portas/celula.py:656`. |

Para item 7, há uma lacuna funcional: os alimentadores da proteção de cobrança órfã continuam vendo somente regeneração. A fila lê apenas `regenerada_em` em `app/escala/services/regeneracao.py:144`; o command de suprajuiz carrega apenas esse carimbo em `app/ponto/management/commands/supra_juiz.py:167` e só segura a retratação se `cobranca_orfa` responder em `:275`. A fonte retroativa de trilha também filtra apenas a ação `regenerar_celulas_vinculo`, no modelo `EscalaColaborador`, em `app/escala/services/regeneracao.py:57`. A edição nova não atualiza `regenerada_em` nem marca órfãs em `app/ponto/portas/celula.py:597`. Assim, um DNA alterado por edição humana não alimenta essa proteção; ocorrência real de retratação falsa em produção: [nao verificado]. Procedência sugerida: **bloqueia**, se o contrato exige preservar também a história das cobranças atingidas pela edição.

Outros alimentadores limitados à regeneração: marcador de órfãs em `app/ponto/management/commands/gerar_celulas.py:241`; diagnóstico `dna_reescrito_em` em `app/ponto/services/dia_do_colab.py:91`; medição específica de regeneração no ensaio em `app/chamados/management/commands/materializar_passivo_validado.py:220`. O último tem escopo explicitamente de regeneração em `:210`; sua exclusão da edição, isoladamente, não prova defeito.

Prosa/contrato desatualizados, procedência **fila**:

- `app/docs/TICKETS.md:924`: ainda declara três operações.
- `app/docs/ADR/0003-celula-tres-naturezas.md:44`: escritor de `editada` não localizado; `:51`, `:52`: nenhum escritor dos campos humanos; `:45`, `:46`, `:47`: mapa de escritores omite edição.
- `app/docs/RELATO.md:3`: afirma ausência do escritor humano e fatia B parada.
- `app/chamados/veredito_lavrado.py:146`: comentário ainda diz `editada_em` “zero hoje”; população real atual: [nao verificado].
- `app/escala/models.py:1641` e `app/ponto/management/commands/gerar_celulas.py:2` dizem que regeneração só toca `data >= hoje`, embora o caminho real reescreva passadas em `app/ponto/portas/celula.py:267`; divergência anterior ao novo diff conforme anexo fornecido [nao verificado por git].

## 06/11 11:49 · sessao 5113c8392c5f · - · - · origem=Ronald (tmux)

**Mapa de escritores/leitores e efeitos, somente por leitura estática.** Execução e contagens reais de produção: `[não verificado]`.

| Objeto | Quem escreve/produz | Quem lê |
|---|---|---|
| `CelulaDia.veredito` | Porta `lavrar_veredito`: atribui em `app/ponto/portas/celula.py:814`, persiste por `salvar_e_emitir` em `:817-819`; método do modelo delega em `app/escala/models.py:1819-1828`. Cartório produz `nunca_bateu` e chama porta em `app/ponto/services/cartorio.py:1241-1247`; decisão humana chama porta em `app/ponto/services/veredito_celula.py:330`. | C5 `app/escala/services/cadastro_realidade.py:163`; retenção `app/folha/export.py:1233-1255`; adesão `app/chamados/services/adesao.py:32-34`; vigia do fio `app/chamados/services/vigia_fio.py:379`; Dia da Frota `app/relatorios/dia_da_frota.py:51-60`; apresentação do veredito `app/ponto/services/veredito_celula.py:107-109`; leitor de célula `app/escala/services/leitor_celula.py:407`. Inventário absolutamente exaustivo dos acessos dinâmicos: `[não verificado]`. |
| `Colaborador.situacao` | Operação `situacao` declara propriedade em `app/colaboradores/services/colaborador.py:32`; portas gravam em `:55-76`, lote `:80-88`, criação `:91-93`. Desligamento atribui e salva pela porta em `app/colaboradores/services/desligamento.py:104-108`; reativação em `:210-214`; INSS em `app/ponto/views.py:2433-2435`; reversor em `app/ponto/management/commands/reverter_situacao_afastado.py:65-80`; admissão cria ativo em `app/colaboradores/views.py:1181-1205`. | Nova guarda C5 em `app/escala/services/cadastro_realidade.py:166-169`; portões de emissão `app/ponto/services/cartorio.py:1696-1697,1762`; adesão `app/chamados/services/adesao.py:33,66-67`; cron dos agendados `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38-40`; C4 `app/escala/services/cadastro_realidade.py:547-551`; universo “quem cobrar” `app/escala/services/furos_diarios.py:259`. Há muitos outros leitores de situação no código; ausência de outros acessos `[não verificado]`. |
| `Colaborador.data_demissao` | Mesma operação da porta `app/colaboradores/services/colaborador.py:32,55-76`; agendamento escreve data sem situação em `app/colaboradores/services/desligamento.py:82-86`; desligamento escreve em `:105-108`; reativação limpa em `:211-214`. | C5 carrega `app/escala/services/cadastro_realidade.py:166-167`; `sai_pela_rescisao` lê `app/folha/export.py:60`; retentor corta `:1200-1201` e compara dia em `:1229`; cadastro do dia recebe em `app/escala/services/cadastro_realidade.py:460` e `app/ponto/services/cartorio.py:1096`; cron dos agendados lê em `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38-45,51`. |
| `colabs_nunca_bateu` | Produz dicionário em memória, sem persistência, `app/escala/services/cadastro_realidade.py:160-172`. | Leitores produtivos diretos localizados: `_nunca_bateu`, `app/escala/services/cadastro_realidade.py:178`; `vigia_retido_calado.censo`, `app/folha/vigia_retido_calado.py:42-48`. |
| `sai_pela_rescisao` | Função pura responde por data de demissão e fim; não escreve modelo, `app/folha/export.py:54-60`. | `classificar_export`, `app/folha/export.py:858`; resumo de fechamento, `app/ponto/services/fechamento_lista.py:51,103-109`; nova C5, `app/escala/services/cadastro_realidade.py:155,169`. **A função não lê situação; a guarda de desligado é acrescentada pela C5.** (`app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:166-169`.) |
| `janela_atual/anterior` | Funções puras: `app/ponto/janelas.py:60-72`; dependem do corte por empresa em `:36-51`, aritmética em `:10-33`. | Nesta cadeia: C5 anterior `app/escala/services/cadastro_realidade.py:162`, competência de cada dia `:169`; lavrador dos Retratos importa/usa ambas em `app/folha/management/commands/lavrar_retrato_folha.py:46,60`; Retrato lê ambas em `app/folha/retrato_folha.py:1284,1291`; folha delega janela de fechamento em `app/folha/export.py:48-51`. Outros consumidores incluem `app/escala/services/cadastro_tipo.py:336`, `app/escala/services/furos_diarios.py:68,76,132,177`, `app/ponto/services/cartorio.py:264,325`, `app/folha/management/commands/pre_fechamento.py:73,89`; não são funções novas/escritas pelo diff. |
| `vigia_retido_calado.censo` | Lê Retratos, monta contadores em memória, `app/folha/vigia_retido_calado.py:31-57`. | Leitor produtivo direto localizado: command importa em `app/folha/management/commands/vigiar_retido_calado.py:14`, chama em `:31`; `linhas()` lê resultado em `app/folha/vigia_retido_calado.py:60-66`. |
| command `vigiar_retido_calado` | `handle` lê uma data, chama censo, imprime e eventualmente encerra com código 2; não persiste, `app/folha/management/commands/vigiar_retido_calado.py:28-40`. | Cron registrado em `app/config/crons.py:1641-1643`, dependente de `lavrar_retrato_folha`; execução efetiva desse cron `[não verificado]`. |

**Efeito quantitativo relativo ao V1 anexado — procedente, severidade média.** Com o mesmo `hoje` e estado estável, V2 remove dias somente de desligados com demissão preenchida, nas competências cujo fim seja `>= data_demissao`; V1 ainda mantinha os dias até a demissão nessas competências. Ativos e demissão nula não mudam. A fórmula está em `app/escala/services/cadastro_realidade.py:163-171`, com predicado `app/folha/export.py:60`; o filtro anterior está no `v2.diff` fornecido, sem numeração de arquivo: `[não verificado]`.

| Consumidor | Quanto pode mudar |
|---|---|
| `_nunca_bateu` / `lista()` | Uma assinatura C5 por ID que ainda tenha ao menos um dia. Redução no número mostrado em `detalhe`; perde C5 se perder todos os dias (`app/escala/services/cadastro_realidade.py:178-180`). `lista` integra essa assinatura em `:808-814`, conta uma ocorrência por assinatura em `:860-863`; pode perder uma pessoa se C5 for sua única assinatura em `:872-873`. Contadores de cadastro/listados mudam segundo conjuntos restantes em `:888-904`. |
| Tela e PDF | Recebem a mesma lista em `app/escala/views.py:484-492`; HTML exibe detalhe/link em `app/templates/escala/cadastro_x_realidade.html:28`; PDF percorre assinaturas em `app/escala/services/cadastro_realidade.py:1019-1030`. |
| `vigia_retido_calado` | Se o ID perde seu último dia C5, **cada** linha bloqueada com fio `ninguem` daquele ID deixa de roteada e passa a calada; `bloqueados` e `cego` permanecem iguais para os mesmos Retratos. O consumidor descarta dias/competências ao construir `set` e conta só por ID: `app/folha/vigia_retido_calado.py:43-56`. |
| `ListaDoAdmin` / vigia do fio | C5 não contém chave `data` (`app/escala/services/cadastro_realidade.py:179-180`), portanto não altera `dias_na_lista`, que exige essa chave (`:980-986`); não muda `dia_do_admin` por C5 (`app/chamados/services/vigia_fio.py:222-228,313-314`). Entretanto muda `_colabs`, carregado de todas as pessoas em `:216-218`; se perder sua única assinatura, pode inverter causa `d → e`, quando não houver perguntas nem dia na lista (`:677-686`). Em sentido inverso, comparando ao pré-inclusão do desligado, seria `e → d`. |
| `vigiar_fio_morto` | Mudanças em contadores de alcance podem chegar ao JSON gravado por rodada de frota em `app/chamados/management/commands/vigiar_fio_morto.py:210-226`; gravação usa arquivo novo/replace em `:127-134`; com recorte `--empresa` não carimba (`:228-230`). |
| Fechadores de C7/C8 | **Variação de seleção por C5 = zero**, pois filtram códigos específicos: `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27-37`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35-45`. |
| `placar_estrutural` | **Variação automática = zero:** a menção à lista é string em `app/core/placar_estrutural.py:149`; `placar` percorre `RESULTADOS`, sem consultar lista, em `:650-660`. |

**Porta C5 — dúvida de permissão, severidade baixa/preexistente.** C5 entrega link ao detalhe em `app/escala/services/cadastro_realidade.py:634-636`; a tela da lista exige `ver_escala` (`app/escala/views.py:485-487`), detalhe exige `ver_cadastro` (`app/colaboradores/views.py:459-464`). Distribuição real das permissões e utilidade efetiva da porta para cada admin: `[não verificado]`.

**Sem novo push — ausência procedente no trecho alterado.** A C5 apenas consulta/formata (`app/escala/services/cadastro_realidade.py:163-180`) e a tela apresenta link; POSTs são exclusivos de C7 (`app/templates/escala/cadastro_x_realidade.html:28`). A adesão continua exigindo ativo no universo e vetando desligado (`app/chamados/services/adesao.py:32-34,66-67`); portões do cartório continuam vedando desligado (`app/ponto/services/cartorio.py:1696-1697,1762`). HX e ocorrência real de push em produção: `[não verificado]`.

## 06/11 11:49 · sessao 5f6a7d45f21b · - · - · origem=Ronald (tmux)

| Item | Conclusão | Procedência sugerida |
|---|---|---|
| 5 — segundo juiz / fonte única | **nenhum novo encontrado**: edição usa `vinculo_do_dia_carregado`, `_competencias_exportadas`, `montar_dna`; efeitos usam `julgar_celula`, `janela_atual` e `recalcular_por_evento` (`app/ponto/portas/celula.py:555`, `:566`, `:581`, `:588`, `:440`, `:459`, `:464`). | nada |
| 5 — segundo escritor | **nenhum no diff**: novas escritas de CelulaDia ficam na porta, dentro de `editar_dia`, e a quarta operação declara edição e troca (`app/ponto/portas/celula.py:82`, `:597`, `:600`, `:606`). | nada |
| 5 — relógio | `editar_dia` nasce sem parâmetro de instante e lê `timezone.now()` internamente; o helper conserva leitura de `localdate()` já existente no pai (`app/ponto/portas/celula.py:533`, `:595`, `:432`; comparação pelo anexo). O corte local pede relógio recebido da borda (`app/docs/CORTES.md:370`). | fila |
| 5 — ordem das travas | Ordena cartas por PK, mas escreve células por papel cedente/substituto; cartas distintas para o mesmo par podem inverter a ordem dos colaboradores/células. `select_related('colaborador')` acompanha o `select_for_update` (`app/ponto/portas/celula.py:649`, `:650`, `:662`, `:665`, `:590`). PostgreSQL declarado (`app/config/settings/prod.py:11`); deadlock observado [nao verificado]. | bloqueia |
| 5 — mordida real | Cartas ausentes, estado cedente/substituto, idempotência, rollback no segundo lado, DNA/vínculo, exportada e trilhas têm asserções de resultado (`app/ponto/tests/test_o10b_editar_dia_troca.py:147`, `:178`, `:206`, `:238`, `:252`, `:287`, `:325`, `:380`, `:414`, `:436`). Execução/RED observado [nao verificado]. | nada |
| 5 — selo de trava fraco | A01 só procura os nomes `atomic` e `select_for_update` no AST: chamada morta conservaria a aprovação; não verifica avaliação da consulta, ordem ou concorrência (`app/ponto/tests/test_o10b_editar_dia_troca.py:314`, `:319`, `:322`, `:323`). | fila |
| 5 — rollback de callbacks sem prova | `_trocar` substitui `on_commit` por execução imediata; assim A02 verifica estado no banco, mas não prova descarte dos callbacks no rollback (`app/ponto/tests/test_o10b_editar_dia_troca.py:39`, `:115`, `:325`, `:338`). | fila |
| 5 — mutante de trilha | Remover as duas guardas `_t is None` preserva os caminhos normais exercitados; não há teste novo injetando falha de `evento_kw`, enquanto a falha adversarial existente ocorre no segundo `editar_dia` (`app/ponto/portas/celula.py:615`, `:681`; `app/ponto/tests/test_o10b_editar_dia_troca.py:330`, `:337`). Aprovação efetiva desse mutante [nao verificado]. | fila |
| 5 — mutante de autoria | Remover apenas `cel.editada_por = autor` deixaria o autor antigo nas reedições, mantendo o `save(update_fields=...)`; os testes de reedição usam o mesmo admin, sem segundo autor (`app/ponto/portas/celula.py:605`, `:606`; `app/ponto/tests/test_o10b_editar_dia_troca.py:365`, `:371`, `:377`). Aprovação efetiva desse mutante [nao verificado]. | fila |
| 5 — fallback disfarçado | **nenhum funcional novo encontrado**: falta de vínculo retorna recusa; falha ao identificar autoridade conserva a guarda e altera apenas o texto (`app/ponto/portas/celula.py:569`, `:573`, `:580`, `:582`, `:587`). | nada |
| 5 — pouso antes de pré-requisito | Pouso/ambiente de produção [nao verificado]. | nada | [nao verificado]
| 6 — d3 | **nenhum afrouxamento antigo identificado no anexo**: catraca passa a exigir conjunto vazio; o novo selo exige exatamente `{porta: {editar_dia}}`, com exemplo negativo (`app/escala/tests/test_d3_guarda_da_linha_celula.py:195`, `:197`, `:199`, `:204`, `:206`). Detector conserva limitações de AST: reconhece atribuições e certos kwargs/literais, sem interpretar `setattr` ou kwargs dinâmicos (`:86`, `:95`, `:100`). | nada para regressão; fila para limites |
| 6 — gate | **nenhum afrouxamento identificado no anexo**: allowlist/pendentes continuam vazios; varredura externa e pertença a operação continuam; conjunto ganha apenas quarta operação autorizada (`app/ponto/tests/test_chokepoint_celula_gate.py:22`, `:23`, `:50`, `:63`, `:70`). | nada |
| 6 — idempotência | **nenhum afrouxamento identificado no anexo**: alteração acrescenta teste de edição, comparando retorno, linha inteira e uma trilha (`app/ponto/tests/test_porta_celula.py:63`, `:74`, `:75`, `:77`). | nada |

Observação útil ao item 7: a mensagem do gate ainda enumera só gerar/lavrar/carimbar (`app/ponto/tests/test_chokepoint_celula_gate.py:53`).

## 06/11 11:49 · sessao 39c97379be88 · - · - · origem=Ronald (tmux)

1. **Alta — procedente: a cura da rescisão existe, mas “mesmos dias que a folha segura” continua sem prova pelo juiz efetivo.** A C5 passou a chamar `sai_pela_rescisao` por competência do dia; o juiz usa a mesma função antes da retenção. Porém o teste compara apenas `sai_pela_rescisao + motivos_retencao_celula`, sem chamar `classificar_export`, que também sai antecipadamente por falta de código, férias com batida e cadastro zero. A fixture nem preenche `codigo_dominio`. A triagem dos itens 1/2/4 só procede quanto ao núcleo da rescisão, não como cura geral da equivalência. (`app/escala/services/cadastro_realidade.py:155,169`; `app/folha/export.py:858,864–876,951–960`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:31–34,41–43,49–60`.)

2. **Média — procedente: derivação paralela residual; segundo juiz nominal é dúvida.** O registro vivo atribui a `classificar_export` as perguntas de entrada no TXT e bloqueio da aprovação. A C5 continua decidindo seus dias diretamente a partir de `veredito`, sem consultar o retentor, que já oferece `com_dias=True` expressamente para evitar cópias. Admissão, isenção e protestos continuam podendo produzir respostas diferentes; a chamada à autoridade da rescisão não elimina essas diferenças. Não há novo registro nominal de juiz no diff. (`app/core/juizes.py:288–299`; `app/escala/services/cadastro_realidade.py:163–171`; `app/folha/export.py:1178–1185,1192–1201,1245–1254`; `CLAUDE.md:20–22`.)

3. **Média — procedente: RED tem mordida parcial, com mutantes relevantes sobreviventes aos testes alterados por inspeção.** [nao verificado]

   | Mutante | Resultado pela leitura |
   |---|---|
   | Restaurar exclusão de todos os desligados | Morre pela presença C5 e pelos dias anteriores exigidos. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:63–74`.) |
   | Remover pergunta `sai_pela_rescisao` | Morre: corrente precisa desaparecer e desligado da anterior precisa retornar `None`. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:73–78`.) |
   | Aplicar o corte também ao ativo | Morre pelo ativo com demissão preenchida. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:93–98`.) |
   | Excluir desligado sem demissão | Morre pela igualdade integral de `DIAS_A`. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:87–90`.) |
   | `janela_atual(d, emp)` → `janela_atual(d)` | Sobrevive às fixtures alteradas: todas usam o corte padrão 21; nenhuma cadastra outro corte. (`app/escala/services/cadastro_realidade.py:169`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:41`; `app/folha/tests/test_vigia_retido_calado.py:76`; `app/colaboradores/models.py:46`; `app/ponto/janelas.py:36–51`.) |
   | Reverter somente command para `censo(emps)` | Sobrevive: testes do command não têm pessoa C5 e teste de injeção chama `censo` diretamente. (`app/folha/management/commands/vigiar_retido_calado.py:31`; `app/folha/tests/test_vigia_retido_calado.py:58–66,115–120`.) |
   | Substituir a chamada à autoridade por comparação inline equivalente | Novos testes semânticos não distinguem a cópia da autoridade; testemunha e resultado continuam iguais. (`app/escala/services/cadastro_realidade.py:169`; `app/folha/export.py:60`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:73–90`.) |

   Execução dos testes e mutantes: **[não verificado]**. [nao verificado]

4. **Média — parcialmente procedente: relógio curado no caminho produtivo do command; fallback permanece.** O command injeta a data e `censo` a encaminha à C5, corrigindo o caminho apontado na v1. Entretanto `censo(hoje=None)` e C5 ainda permitem leitura interna do relógio; o teste continua exercendo esse caminho diretamente. Portanto “curado” é correto para `command → censo → C5`, não para a ausência absoluta da classe no sítio. (`app/folha/management/commands/vigiar_retido_calado.py:30–31`; `app/folha/vigia_retido_calado.py:27,43`; `app/escala/services/cadastro_realidade.py:157`; `app/folha/tests/test_vigia_retido_calado.py:119–120`; `app/docs/CORTES.md:370`.)

5. **Média — procedente: achados 7/8 da v1 permanecem, como a triagem reconhece.** C5 só formata texto, sem `data`; o vigia descarta as datas ao transformar a leitura em conjunto de IDs e roteia qualquer linha bloqueada dessa pessoa cujo fio seja `ninguem`. Não há comparação de competência nem de motivo da linha. A fixture inclusive usa `espelho_furo`, não `espelho_nunca_bateu`, e isso não impede o roteamento. (`app/escala/services/cadastro_realidade.py:175–180`; `app/folha/vigia_retido_calado.py:43–48`; `app/folha/tests/test_vigia_retido_calado.py:18,79–89`.) Ocorrência real: **[não verificado]**.

6. **Baixa — procedente: a citação do achado 16 foi corrigida.** O teste agora aponta para `export.py:1198–1201`, que contém efetivamente os cortes de admissão/demissão. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:5–6`; `app/folha/export.py:1198–1201`.)

7. **Sem defeito identificado — ausência de segundo escritor e alteração de ordem de trava no trecho produtivo.** A alteração C5 consulta e monta estruturas locais; `censo` consulta Retratos e conta; command imprime e pode sair com erro. Não introduzem persistência nem aquisição de trava. As gravações adicionadas são fixtures dos testes. (`app/escala/services/cadastro_realidade.py:159–172`; `app/folha/vigia_retido_calado.py:33–57`; `app/folha/management/commands/vigiar_retido_calado.py:28–40`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:31–34,88,97`.)

8. **Média — dúvida não respondida pela triagem: pouso antes do pré-requisito.** O item 15 da v1 tratava de suíte inteira → opus final → pouso; a triagem 15 responde sobre ativo com demissão, assunto diferente. A obrigação permanece no código documental. A sequência efetivamente executada para este hash é **[não verificado]**. (`CLAUDE.md:369–373`.) [nao verificado]

9. **Sem defeito identificado no diff — sem novo push; HX preservado no anexo.** C5 é consulta/formatação, e os portões existentes continuam vetando emissão ao desligado. O ramo HX continua lavrando o veredito pela porta, com `codigos=[]`; o anexo v2 não altera esse arquivo. A preservação histórica fora do diff fornecido é **[não verificado]**. (`app/escala/services/cadastro_realidade.py:163–180`; `app/ponto/services/cartorio.py:1157–1163,1241–1247,1696–1697,1762`.)

## 06/11 11:49 · sessao ad2b77050f38 · - · - · origem=Ronald (tmux)

1. **Alta — procedente: a igualdade geral com `classificar_export` continua falsa.** A C5 pergunta apenas a saída por rescisão e consulta células `nunca_bateu`; não pergunta ao classificador completo. Este exige fechamento e ainda pode sair por falta de código, férias com batida ou cadastro zero antes da retenção. (`app/escala/services/cadastro_realidade.py:163–171`; `app/folha/export.py:775–780,858–876,894–960`.)

   **Contraexemplo concreto das próprias fixtures:** o colaborador A tem demissão em 22/10 e a C5 exige 05/10 e 20/10; sua criação não informa `codigo_dominio`, cujo default é vazio. Com um fechamento da anterior, a classificação real sairia por `sem_codigo_dominio`, não por `espelho_nunca_bateu`. Sem fechamento, nem há item no classificador. O teste chamado de “testemunha” chama somente `sai_pela_rescisao` e `motivos_retencao_celula`, pulando os demais portões. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:31–43,49–60,73–74`; `app/colaboradores/models.py:311–313`; `app/folha/export.py:775–780,864–867`.)

2. **Média — procedente: nem a igualdade com “rescisão + retentor” vale universalmente.** Considere desligado em 22/10 e célula `nunca_bateu` em 05/10, anterior à admissão em 10/10: a C5 inclui; o retentor corta por admissão. Isento produz outra divergência: C5 inclui, retentor devolve vazio. Célula com protestos somente `FURO_PRE_ADOCAO` também entra na C5 e é dispensada pelo retentor. São contraexemplos deduzidos do código; ocorrência em produção **[não verificado]**. (`app/escala/services/cadastro_realidade.py:163–171`; `app/folha/export.py:1192–1206,1245–1254`; `app/ponto/supra_juiz.py:34–36`.)

3. **Informativa — procedente: a saída por rescisão agora coincide nas bordas solicitadas, condicionada aos outros portões.** Com hoje 25/10, corte 21, anterior 21/09–20/10 e corrente 21/10–20/11:

   | Demissão | C5 após filtro de rescisão | Classificador após esse mesmo portão |
   |---|---|---|
   | 21/09, primeiro dia da anterior | Nenhuma competência | Ambas saem por rescisão |
   | 20/10, último da anterior | Nenhuma competência | Ambas saem por rescisão |
   | 21/10, primeiro da corrente | Somente anterior | Corrente sai por rescisão |
   | 20/11, último da corrente e futuro | Somente anterior | Corrente sai por rescisão |
   | 30/10, futura dentro da corrente | Somente anterior | Corrente sai por rescisão mesmo antes da data |
   | 21/11, futura após a corrente | Ambas, até ontem | Nenhuma sai por rescisão |
   | Nula | Ambas, até ontem | Nenhuma sai por rescisão |

   Os resultados decorrem de `data_demissao <= fim_janela`; não demonstram igualdade dos demais filtros. (`app/ponto/janelas.py:10–33,60–72`; `app/folha/export.py:54–60,858–863`; `app/escala/services/cadastro_realidade.py:162–171`.)

4. **Informativa — procedente: corte diferente de 21 é respeitado.** Com corte 10 e hoje 25/10, anterior = 10/09–09/10, corrente = 10/10–09/11. Demissão em 15/10 preserva somente a anterior; célula de 20/10 sai. Ambos os caminhos consultam o motor da empresa. Não há divergência de rescisão nesse exemplo. (`app/ponto/janelas.py:10–33,36–72`; `app/folha/export.py:47–60,807,858`; `app/escala/services/cadastro_realidade.py:162,169`.)

5. **Média — procedente quanto à historicidade; dúvida quanto a defeito novo: troca de empresa usa o cadastro atual.** Exemplo: empresa A com corte 21 → empresa B com corte 10, demissão 15/10, célula 05/10, hoje 25/10. Antes da troca, a C5 exclui 05/10, pois termina a competência em 20/10; depois, inclui, pois termina em 09/10. O classificador padrão também seleciona pela empresa atual, portanto isso não demonstra divergência entre ambos no mesmo estado. A tela permite alterar `empresa_id`. (`app/escala/services/cadastro_realidade.py:159–169`; `app/folha/export.py:775–780,807,858`; `app/colaboradores/views.py:1313,1340–1342`.)

   Já a comparação com Retrato histórico não está assegurada: o vigia lê Retratos gravados e cruza com a C5 atual. A ocorrência concreta em produção é **[não verificado]**. (`app/folha/vigia_retido_calado.py:33–48`.)

6. **Informativa — procedente o descarte do corte `data__gt=demissao`, no domínio declarado.** Não encontrei contraexemplo lógico para desligado, data de demissão não nula e estado coerente: `d` pertence à janela consultada, logo `d <= fim`; passar pelo portão implica `demissao > fim`; portanto `d < demissao`. Com demissão nula, o corte antigo tampouco operava. Essa prova depende do mesmo estado cadastral e da janela da empresa consultada, não de histórico preservado. (`app/ponto/janelas.py:24–33,60–63`; `app/folha/export.py:60`; `app/escala/services/cadastro_realidade.py:166–171`.)

7. **Média — procedente: faltam mordidas específicas das bordas e do classificador real.** Os testes alterados cobrem demissões internas em 10/10 e 22/10, nula e preservação do ativo; não chamam `classificar_export`. Um mutante da C5 que excluísse apenas quando `data_demissao < fim` — divergindo da autoridade em demissão exatamente no último dia da competência — não é distinguido por essas fixtures. Um mutante que usasse corte 21 no cálculo diário também não seria distinguido, pois a empresa criada usa o default 21. Sobrevivência na suíte inteira **[não verificado]**. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:24–28,41–43,49–60,73–104`; `app/colaboradores/models.py:46`; `app/folha/export.py:60`.)

## 06/11 11:49 · sessao 73e8696ef210 · - · - · origem=Ronald (tmux)

| Subitem | Resultado | Procedencia sugerida |
|---|---|---|
| 4a — par de células e trilhas | **Nenhum caminho normal deixa só uma célula editada:** atomic externo envolve as duas chamadas e a trilha final; recusa sai desse atomic antes de ser capturada. Falha inesperada propaga e também desfaz o par. Falha posterior ao commit pode deixar efeitos derivados incompletos, mas ambas as edições já foram gravadas (`app/ponto/portas/celula.py:644`, `:662`, `:665`, `:675`, `:681`, `:683`). | nada |
| 4b — ordem de locks | Cartas são travadas por PK crescente; células são travadas na ordem cedente→substituto, sem chave global (`app/ponto/portas/celula.py:649`, `:650`, `:662`, `:665`, `:590`). Além disso, o SELECT inclui `select_related('colaborador')` e não limita `of`, em configuração PostgreSQL (`app/ponto/portas/celula.py:649`; `app/config/settings/prod.py:11`). **Risco por inferência:** cartas diferentes dos mesmos colaboradores podem impor ordem A→B e B→A nos colaboradores associados; deadlock observado em execução: **[nao verificado]**. Mesmas duas cartas já se serializam pela ordem dos PKs (`app/ponto/portas/celula.py:650`). | bloqueia |
| 4c — barrados estrutural | Par incompleto, carta removida, mesmo colaborador e documento ausente produzem uma entrada `{dias:0,de:None,ate:None,texto:...}` (`app/ponto/portas/celula.py:645`, `:652`, `:654`, `:656`, `:685`). | nada |
| 4c — barrados de um lado | Exportada ou sem vínculo produzem entrada do lado recusado, `{dias:1,de:dia,ate:dia,texto:...}`; a primeira recusa interrompe o par, portanto não lista necessariamente os dois lados inválidos (`app/ponto/portas/celula.py:577`, `:584`, `:663`, `:666`, `:687`). Ausência de autor/motivo e falhas inesperadas levantam exceção, sem preencher `barrados` (`app/ponto/portas/celula.py:638`, `:640`, `:683`). | nada |
| 4d — antes do commit | `julgar_celula` roda sincronicamente dentro do atomic externo, após cada edição, com exceções engolidas e logadas; a lavra/carimbo escreve pelo mesmo atomic externo (`app/ponto/portas/celula.py:619`, `:440`, `:443`; `app/ponto/services/cartorio.py:1802`, `:1822`; `app/ponto/portas/celula.py:892`). | nada |
| 4d — depois do commit | `recalcular_por_evento` apenas registra `_rodar` em `on_commit`; depois o helper registra rejunção/falta espúria. No sucesso, ordem de registro: dinheiro cedente→rejunção/falta cedente→dinheiro substituto→rejunção/falta substituto (`app/ponto/services/fechamento.py:1791`, `:1808`; `app/ponto/portas/celula.py:464`, `:508`, `:662`, `:665`). Rollback descarta callbacks; efeitos SQL síncronos anteriores são revertidos pelo atomic externo (`app/ponto/portas/celula.py:644`, `:683`). | nada |
| Trilha falhando | `evento_kw` delega ao escritor único, cujo savepoint próprio devolve `None` após falha; `editar_dia` e `trocar_plantao` convertem isso em `RuntimeError`, desfazendo edição/par (`app/core/observ.py:133`, `:139`, `:146`; `app/ponto/portas/celula.py:616`, `:681`). | nada |
| Selo de concorrência | A01 verifica somente presença AST dos nomes `select_for_update` e `atomic`; chamada morta, lock depois da leitura ou ordem invertida passam (`app/ponto/tests/test_o10b_editar_dia_troca.py:318`, `:322`). | bloqueia |
| Selo de callbacks/rollback | `_trocar` substitui `on_commit` por execução imediata dentro do atomic; A02 verifica células/trilhas revertidas, mas não prova descarte real nem ausência de execução dos callbacks em rollback (`app/ponto/tests/test_o10b_editar_dia_troca.py:115`, `:35`, `:325`, `:338`). | fila |

Nenhum AGENTS aplicável foi encontrado na árvore pesquisada; cobertura fora dessa árvore: **[nao verificado]**.

## 06/11 11:47 · sessao 598e3a39227a · - · - · origem=Ronald (tmux)

O corte em `116` é adequado à cópia: `ids` nasce como lista (`app/ponto/management/commands/diff_calculador.py:102`), os filtros opcionais terminam antes (`:108`) e os usos seguintes são a chamada ao fechamento (`:140`) e a seleção dos colaboradores (`:145`). Mutar `loc["ids"][:]` preserva o objeto usado pelo comando; atribuir apenas em `frame.f_locals["ids"]` seria inadequado [nao verificado: comportamento em execução].

O wrapper deve retornar `{}` imediatamente quando `ids` estiver vazio. Sem isso, a chamada original varre a empresa, porque não recebe `colaborador_ids` (`app/ponto/management/commands/diff_calculador.py:140`). No caso selecionado, passar `colaborador_ids=[cid]` restringe o queryset antes do lote de células (`app/ponto/services/fechamento.py:545`, `:548`, `:596`).

Não encontrei dependência de resultados entre colaboradores: o índice de células inclui colaborador e dia (`app/ponto/services/fechamento.py:596`); as geradoras são explicitamente filtradas pelo colaborador atual (`:645`); os feriados são carregados independentemente do conjunto de colaboradores (`:575`, `:587`). Equivalência completa permanece [nao verificado: revisão estática, sem executar].

A captura original do comando permanece funcional: registra o primeiro `ResultadoMes` de cada colaborador (`app/ponto/management/commands/diff_calculador.py:131`, `:135`) e restaura o construtor/emissor em `finally` (`:141`). Descartar cada chamada elimina seu `_res_fech` e `_certa_cache` (`:116`, `:95`).

Ressalva concreta: uma chamada por colaborador também obtém um novo instante real `_agora_da_passada` por chamada (`app/ponto/services/fechamento.py:607`), enquanto a chamada por empresa obtém um por empresa. Efeito numérico durante fronteira temporal permanece [nao verificado].

## 06/11 11:44 · sessao 598e3a39227a · - · - · origem=Ronald (tmux)

- O anexo retém exceções em `VISTOS[id(exc)] = exc`; cada exceção pode manter traceback, frames e locais de colaboradores. A linha exata do anexo precisa ser contada pelo agente principal; arquivo local ausente [nao verificado]. Deduplicar com um atributo booleano/token na própria exceção evita uma raiz global que retenha traceback. Um conjunto de `id(exc)` sem referência é inadequado, porque IDs podem ser reutilizados [nao verificado].
- O comando retém resultados completos em `_res_fech` (`app/ponto/management/commands/diff_calculador.py:116`, `:135`) e mantém `_certa_cache` entre colaboradores (`app/ponto/management/commands/diff_calculador.py:95`, `:418`). Uma invocação real por colaborador, com `colaborador_ids=[cid]` no fechamento, limita essas estruturas; o parâmetro restringe o universo (`app/ponto/services/fechamento.py:502`).
- O fechamento também mantém lote de células e seu índice por colaborador (`app/ponto/services/fechamento.py:596`, `:602`), além dos objetos retornados (`app/ponto/services/fechamento.py:1119`). A chamada por colaborador limita esses lotes.
- O cache do motor é por instância/cálculo (`app/ponto/motor_calculo_v2.py:2285`); o cache do fechamento nasce dentro da passagem do colaborador (`app/ponto/services/fechamento.py:923`). Não apagar memos durante o cálculo.
- Há cache de módulo de catálogo, que guarda um catálogo e um instante, sem chave por colaborador (`app/ponto/catalogo/ausencias.py:257`, `:276`, `:279`). A API real de limpeza é `invalidar_cache`, que zera instante e valor (`app/ponto/catalogo/ausencias.py:261`). Não identifiquei caches de módulo por colaborador nos arquivos motor/calculador examinados [nao verificado: resultado de busca estática, sem execução].
- Após concluir o colaborador e agregar seus números: soltar capturas, `InsumosDoDono`, retorno e contexto; depois `reset_queries()` e `gc.collect()`. `InsumosDoDono` retém colaborador, oráculo, turnos e dados de cadastro/realidade (`app/ponto/management/commands/e6_oraculo.py:131`, `:145`, `:151`, `:160`). `reset_queries`/`gc.collect` pertencem às bibliotecas, cujas linhas locais não foram verificadas.
- Sombra herda `saas`, cujo `DEBUG=False` está em `app/config/settings/saas.py:14`; a herança está em `app/config/settings/sombra.py:27`. Acúmulo efetivo de queries nesta execução permanece [nao verificado].

## 06/11 11:43 · sessao c2c817f7858b · R1-CAUSAD-V3 · sonda · origem=Code

Não executei nova sonda: o `script.py` anexo não foi localizado na cópia. [nao verificado] O fluxo `--sonda` executa o envelope antes de entregar o dado raspado ao agente (`bin/codex_pergunta.sh:103`, `bin/codex_pergunta.sh:133`).

Vereditos:
- **SOMA: [nao verificado].** Não há linha `SOMA` no dado recebido; não há veredito de fechamento emitido pelo script. [nao verificado]
- **INCOMPLETO: sim, pelo retorno informado `rc_sombra=137`; não há linha literal `INCOMPLETO`.** A causa da interrupção e o conteúdo restante são [nao verificado]. O envelope propaga o retorno da execução (`bin/sonda_leitura.sh:241`, `bin/sonda_leitura.sh:251`).

Transcrição integral do dado recebido, sem acrescentar linhas à saída. [nao verificado]

```text
sonda_leitura: script.py [432e14f15d01f891] no cpuset 4-7, lendo sombra como leitor (LEITURA)

ATO|hoje=2026-11-06|agora=2026-11-06T11:17:26.613842-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624|segundos=844.3
RUBRICA|competencia|rubrica|v2_h|v3_h|delta_h|colabs_div|dias_colab_div
DONO|competencia|rubrica|dono|v2_h|v3_h|delta_h|colabs|dias_colab
DIA|2026-10|horas_trabalhadas|col204|2026-09-29|CADASTRO|7.316473056|4.773582500|-2.542890556
DIA|2026-10|horas_trabalhadas|col502|2026-10-10|CADASTRO|7.569179979|2.843692404|-4.725487575
DIA|2026-10|horas_trabalhadas|col512|2026-10-11|ESTRUTURA|6.478212222|11.442890556|+4.964678333
DIA|2026-10|horas_trabalhadas|col577|2026-10-15|CADASTRO|7.290805556|4.448500278|-2.842305278
DIA|2026-10|horas_trabalhadas|col592|2026-10-20|CADASTRO|7.314860278|5.421271667|-1.893588611
DIA|2026-10|horas_trabalhadas|col599|2026-10-20|CADASTRO|10.100982500|5.572917222|-4.528065278
DIA|2026-10|horas_trabalhadas|col788|2026-09-24|BATIDA|8.964272778|0.964272778|-8.000000000
DIA|2026-10|horas_trabalhadas|col988|2026-10-17|CADASTRO|6.190200326|3.268974456|-2.921225870
RUBRICA|2026-10|horas_trabalhadas|70627.257635674|70604.768750840|-22.488884834|8|8
DONO|2026-10|horas_trabalhadas|BATIDA|8.964272778|0.964272778|-8.000000000|1|1
DONO|2026-10|horas_trabalhadas|CADASTRO|45.782501694|26.328938527|-19.453563168|6|6
DONO|2026-10|horas_trabalhadas|ESTRUTURA|6.478212222|11.442890556|+4.964678333|1|1
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|70566.032648980|70566.032648980|-0.000000000|511|7676
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|343.609469184|343.609469184|+0.000000000|331|1805
RESIDUO|2026-10|horas_trabalhadas|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=70224.761664336|v3_h=70604.768750840|delta_h=+380.007086504
DIA|2026-10|horas_intra_indenizada|col82|2026-10-06|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-08|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-12|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-14|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-15|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-16|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-20|CADASTRO|1.166666667|0.000000000|-1.166666667
DIA|2026-10|horas_intra_indenizada|col450|2026-09-27|BATIDA|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col450|2026-10-11|BATIDA|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col512|2026-10-11|ESTRUTURA|0.000000000|1.000000000|+1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-22|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-24|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-26|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-28|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-09-30|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-10-02|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col599|2026-10-06|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col638|2026-10-01|CADASTRO|1.500000000|0.000000000|-1.500000000
DIA|2026-10|horas_intra_indenizada|col638|2026-10-04|CADASTRO|0.000000000|0.497353333|+0.497353333
DIA|2026-10|horas_intra_indenizada|col638|2026-10-11|CADASTRO|0.000000000|0.496789722|+0.496789722
DIA|2026-10|horas_intra_indenizada|col788|2026-09-25|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col788|2026-10-01|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col788|2026-10-05|BATIDA|1.000000000|0.449190000|-0.550810000
DIA|2026-10|horas_intra_indenizada|col824|2026-10-04|BATIDA|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col824|2026-10-18|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col849|2026-10-09|CADASTRO|1.000000000|0.000000000|-1.000000000
DIA|2026-10|horas_intra_indenizada|col915|2026-09-27|CADASTRO|0.000000000|0.181985833|+0.181985833
DIA|2026-10|horas_intra_indenizada|col915|2026-10-01|CADASTRO|0.000000000|0.182740833|+0.182740833
DIA|2026-10|horas_intra_indenizada|col941|2026-10-01|CADASTRO|1.000000000|0.000000000|-1.000000000
RUBRICA|2026-10|horas_intra_indenizada|2194.339455993|2171.480849048|-22.858606944|10|29
DONO|2026-10|horas_intra_indenizada|BATIDA|4.000000000|0.449190000|-3.550810000|3|4
DONO|2026-10|horas_intra_indenizada|CADASTRO|21.666666667|1.358869722|-20.307796944|8|24
DONO|2026-10|horas_intra_indenizada|ESTRUTURA|0.000000000|1.000000000|+1.000000000|1|1
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|2168.672789326|2168.672789326|+0.000000000|511|7655
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|331|1805
RESIDUO|2026-10|horas_intra_indenizada|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=2178.394776548|v3_h=2171.480849048|delta_h=-6.913927500
DIA|2026-10|horas_extras_50|col82|2026-10-06|CADASTRO|0.000000000|0.500647778|+0.500647778
DIA|2026-10|horas_extras_50|col82|2026-10-08|CADASTRO|0.000000000|0.471973333|+0.471973333
DIA|2026-10|horas_extras_50|col82|2026-10-14|CADASTRO|0.000000000|0.179223056|+0.179223056
DIA|2026-10|horas_extras_50|col82|2026-10-15|CADASTRO|0.000000000|0.557518333|+0.557518333
DIA|2026-10|horas_extras_50|col502|2026-10-10|CADASTRO|0.235846646|0.000000000|-0.235846646
DIA|2026-10|horas_extras_50|col599|2026-10-16|CADASTRO|0.000000000|0.224154212|+0.224154212
DIA|2026-10|horas_extras_50|col915|2026-09-27|CADASTRO|0.320207500|0.000000000|-0.320207500
DIA|2026-10|horas_extras_50|col915|2026-10-01|CADASTRO|0.516049722|0.333308889|-0.182740833
RUBRICA|2026-10|horas_extras_50|66.971818349|68.166540082|+1.194721733|4|8
DONO|2026-10|horas_extras_50|CADASTRO|1.072103868|2.266825601|+1.194721733|4|8
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|65.899714481|65.899714481|+0.000000000|509|7524
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|2.430862778|2.430862778|+0.000000000|341|1957
RESIDUO|2026-10|horas_extras_50|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=69.402681127|v3_h=68.166540082|delta_h=-1.236141045
DIA|2026-10|horas_extras_100|col727|2026-10-12|ESTRUTURA|0.000000000|10.985971389|+10.985971389
DIA|2026-10|horas_extras_100|col830|2026-10-12|ESTRUTURA|0.000000000|12.149131944|+12.149131944
RUBRICA|2026-10|horas_extras_100|422.982309410|446.117412743|+23.135103333|2|2
DONO|2026-10|horas_extras_100|ESTRUTURA|0.000000000|23.135103333|+23.135103333|2|2
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|422.982309410|422.982309410|-0.000000000|509|7531
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|6.572320665|6.572320665|+0.000000000|341|1956
RESIDUO|2026-10|horas_extras_100|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=425.616685630|v3_h=456.966780521|delta_h=+31.350094891
DIA|2026-10|horas_extras_100_feriado|col727|2026-10-12|ESTRUTURA|0.000000000|10.985971389|+10.985971389
DIA|2026-10|horas_extras_100_feriado|col830|2026-10-12|ESTRUTURA|0.000000000|12.149131944|+12.149131944
RUBRICA|2026-10|horas_extras_100_feriado|419.479367232|442.614470565|+23.135103333|2|2
DONO|2026-10|horas_extras_100_feriado|ESTRUTURA|0.000000000|23.135103333|+23.135103333|2|2
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|419.479367232|419.479367232|-0.000000000|509|7531
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|3.430878889|3.430878889|+0.000000000|341|1956
RESIDUO|2026-10|horas_extras_100_feriado|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0.000000000|v3_h=453.463838343|delta_h=+453.463838343
RUBRICA|2026-10|horas_extras_50_noturna|0.828852396|0.828852396|+0.000000000|0|0
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0.828852396|0.828852396|+0.000000000|509|7532
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|341|1957
RESIDUO|2026-10|horas_extras_50_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0.000000000|v3_h=0.828852396|delta_h=+0.828852396
DIA|2026-10|horas_extras_100_noturna|col191|2026-10-12|CADASTRO|0.000000000|1.000000000|+1.000000000
DIA|2026-10|horas_extras_100_noturna|col270|2026-10-12|ESTRUTURA|0.000000000|0.959735944|+0.959735944
DIA|2026-10|horas_extras_100_noturna|col292|2026-10-12|ESTRUTURA|0.000000000|0.909870556|+0.909870556
DIA|2026-10|horas_extras_100_noturna|col301|2026-10-12|ESTRUTURA|0.000000000|0.856398333|+0.856398333
DIA|2026-10|horas_extras_100_noturna|col390|2026-10-12|ESTRUTURA|0.000000000|0.553309638|+0.553309638
DIA|2026-10|horas_extras_100_noturna|col468|2026-10-12|ESTRUTURA|0.000000000|0.550908919|+0.550908919
DIA|2026-10|horas_extras_100_noturna|col469|2026-10-12|CADASTRO|0.000000000|1.000000000|+1.000000000
DIA|2026-10|horas_extras_100_noturna|col825|2026-10-12|ESTRUTURA|0.000000000|0.562620556|+0.562620556
DIA|2026-10|horas_extras_100_noturna|col849|2026-10-12|ESTRUTURA|0.000000000|0.841615783|+0.841615783
DIA|2026-10|horas_extras_100_noturna|col865|2026-10-12|CADASTRO|0.000000000|5.164677786|+5.164677786
RUBRICA|2026-10|horas_extras_100_noturna|60.714638630|73.127033645|+12.412395015|10|10
DONO|2026-10|horas_extras_100_noturna|CADASTRO|0.000000000|7.164677786|+7.164677786|3|3
DONO|2026-10|horas_extras_100_noturna|ESTRUTURA|0.000000000|5.234459729|+5.234459729|7|7
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|60.714638630|60.727896130|+0.013257500|509|7522
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|341|1957
RESIDUO|2026-10|horas_extras_100_noturna|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0.000000000|v3_h=73.127033645|delta_h=+73.127033645
RUBRICA|2026-10|horas_folga_trabalhada|0.000000000|0.000000000|+0.000000000|0|0
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0.000000000|0.000000000|+0.000000000|511|7684
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|331|1805
RESIDUO|2026-10|horas_folga_trabalhada|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=742.930323022|v3_h=0.000000000|delta_h=-742.930323022
DIA|2026-10|horas_noturnas|col245|2026-09-21|CADASTRO|1.867681376|2.134493001|+0.266811625
DIA|2026-10|horas_noturnas|col245|2026-09-23|CADASTRO|1.869965836|2.137103813|+0.267137977
DIA|2026-10|horas_noturnas|col245|2026-09-24|ESTRUTURA|1.851872500|2.116425714|+0.264553214
DIA|2026-10|horas_noturnas|col245|2026-09-25|CADASTRO|1.863168056|2.129334921|+0.266166865
DIA|2026-10|horas_noturnas|col245|2026-09-26|ESTRUTURA|1.915446667|2.189081905|+0.273635238
DIA|2026-10|horas_noturnas|col245|2026-09-27|CADASTRO|1.885175556|2.154486349|+0.269310794
DIA|2026-10|horas_noturnas|col245|2026-09-28|ESTRUTURA|1.942230278|2.219691746|+0.277461468
DIA|2026-10|horas_noturnas|col245|2026-09-30|ESTRUTURA|1.956297778|2.235768889|+0.279471111
DIA|2026-10|horas_noturnas|col373|2026-09-30|ESTRUTURA|0.572047302|0.500541389|-0.071505913
DIA|2026-10|horas_noturnas|col373|2026-10-01|ESTRUTURA|0.646286032|0.565500278|-0.080785754
DIA|2026-10|horas_noturnas|col489|2026-09-23|ESTRUTURA|6.000774068|6.858027506|+0.857253438
DIA|2026-10|horas_noturnas|col489|2026-09-25|ESTRUTURA|6.000494001|6.857707430|+0.857213429
DIA|2026-10|horas_noturnas|col489|2026-09-27|ESTRUTURA|6.000421816|6.857624933|+0.857203117
DIA|2026-10|horas_noturnas|col727|2026-10-12|ESTRUTURA|5.987302500|6.842631429|+0.855328929
DIA|2026-10|horas_noturnas|col727|2026-10-14|ESTRUTURA|6.003224444|6.860827937|+0.857603492
DIA|2026-10|horas_noturnas|col727|2026-10-16|ESTRUTURA|6.068942500|6.935934286|+0.866991786
DIA|2026-10|horas_noturnas|col859|2026-09-21|ESTRUTURA|6.857064127|5.999931111|-0.857133016
DIA|2026-10|horas_noturnas|col859|2026-09-22|ESTRUTURA|6.858000000|6.000750000|-0.857250000
DIA|2026-10|horas_noturnas|col859|2026-09-24|ESTRUTURA|6.844368889|5.988822778|-0.855546111
DIA|2026-10|horas_noturnas|col859|2026-09-25|ESTRUTURA|6.856735873|5.999643889|-0.857091984
DIA|2026-10|horas_noturnas|col859|2026-09-26|ESTRUTURA|6.867731746|6.009265278|-0.858466468
DIA|2026-10|horas_noturnas|col859|2026-09-28|ESTRUTURA|6.854342222|5.997549444|-0.856792778
DIA|2026-10|horas_noturnas|col964|2026-09-25|ESTRUTURA|1.000000000|1.142857143|+0.142857143
DIA|2026-10|horas_noturnas|col964|2026-09-26|BATIDA|1.015868889|1.160993016|+0.145124127
DIA|2026-10|horas_noturnas|col964|2026-09-27|ESTRUTURA|0.976877500|1.116431429|+0.139553929
DIA|2026-10|horas_noturnas|col964|2026-09-28|CADASTRO|0.939963611|1.074244127|+0.134280516
RUBRICA|2026-10|horas_noturnas|15948.373066655|15950.946552788|+2.573486133|6|26
DONO|2026-10|horas_noturnas|BATIDA|1.015868889|1.160993016|+0.145124127|1|1
DONO|2026-10|horas_noturnas|CADASTRO|8.425954434|9.629662210|+1.203707776|2|5
DONO|2026-10|horas_noturnas|ESTRUTURA|88.060460243|89.295014512|+1.234554269|6|20
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|15850.870783089|15850.860883049|-0.009900040|510|7590
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|342|1873
RESIDUO|2026-10|horas_noturnas|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=15728.549148812|v3_h=15964.082576240|delta_h=+235.533427428
DIA|2026-10|horas_atraso|col87|2026-09-22|CADASTRO|0.000000000|0.186000000|+0.186000000
DIA|2026-10|horas_atraso|col205|2026-09-21|CADASTRO|0.178166667|1.178166667|+1.000000000
DIA|2026-10|horas_atraso|col258|2026-10-19|ESTRUTURA|0.000000000|2.368500000|+2.368500000
DIA|2026-10|horas_atraso|col259|2026-10-14|ESTRUTURA|0.000000000|1.169655833|+1.169655833
DIA|2026-10|horas_atraso|col259|2026-10-15|ESTRUTURA|0.000000000|1.585000000|+1.585000000
DIA|2026-10|horas_atraso|col274|2026-10-04|ESTRUTURA|0.000000000|2.330333333|+2.330333333
DIA|2026-10|horas_atraso|col368|2026-09-21|ESTRUTURA|0.000000000|1.106833333|+1.106833333
DIA|2026-10|horas_atraso|col655|2026-09-30|ESTRUTURA|0.000000000|0.824060000|+0.824060000
RUBRICA|2026-10|horas_atraso|355.091333333|365.661421482|+10.570088149|7|8
DONO|2026-10|horas_atraso|CADASTRO|0.178166667|1.364166667|+1.186000000|2|2
DONO|2026-10|horas_atraso|ESTRUTURA|0.000000000|9.384382500|+9.384382500|5|6
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|354.913166667|354.912872316|-0.000294351|502|7477
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|10.822000000|10.822000000|+0.000000000|347|2004
RESIDUO|2026-10|horas_atraso|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=356.664166667|v3_h=346.027980551|delta_h=-10.636186116
DIA|2026-10|horas_saida_antecipada|col87|2026-09-22|CADASTRO|1.074666667|0.888500000|-0.186166667
DIA|2026-10|horas_saida_antecipada|col512|2026-10-11|ESTRUTURA|0.557166667|0.000000000|-0.557166667
RUBRICA|2026-10|horas_saida_antecipada|390.477500000|389.735126896|-0.742373104|2|2
DONO|2026-10|horas_saida_antecipada|CADASTRO|1.074666667|0.888500000|-0.186166667|1|1
DONO|2026-10|horas_saida_antecipada|ESTRUTURA|0.557166667|0.000000000|-0.557166667|1|1
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|388.845666667|388.846626896|+0.000960230|502|7483
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0.000000000|0.000000000|+0.000000000|347|2004
RESIDUO|2026-10|horas_saida_antecipada|SEM_PAR|0.000000000|0.000000000|+0.000000000|64|1196
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=354.632166667|v3_h=353.889766749|delta_h=-0.742399917
rc_sombra=137
```

## 06/11 11:26 · sessao 5710202ccc04 · R1-CAUSAD-V3 · leitura · origem=Code

```python
import ast
import datetime as dt
import gc
import io
import logging
import re
import sys
import time
import tokenize
from collections import Counter
from contextlib import redirect_stderr, redirect_stdout
from decimal import Decimal, localcontext
from pathlib import Path
from unittest.mock import patch

PARTE = 1
PARTES = 6

sys.dont_write_bytecode = True


def sonda_r1():
    # Proposta sem execução; desempenho e compatibilidade: [nao verificado].
    saida = sys.stdout
    inicio = time.monotonic()
    limite = inicio + 15 * 60
    rastreador_anterior = sys.gettrace()
    competencia = "-"
    raiz = None
    prefixo_app = None
    codigo_handle = None
    bruto = None
    chamadas = 0
    universo_parte_total = 0
    erros_total = 0
    erros_por_tipo = Counter()
    somas = {}
    quantidades = {}
    selecionados = Counter()
    arquivos = {}
    codigos = {}
    pendentes = {}
    pulsos = 0
    marca = object()
    resetar_queries = None
    invalidar_catalogo = None

    DONOS = ("ESTRUTURA", "CADASTRO", "BATIDA")
    CHAVES = (
        "dias_comparados", "bate_ate_10min",
        "esp_zero_e6_trabalho", "e6_zero_esp_trabalho",
        "diverge_10_60", "diverge_acima_60",
        "dia_batida_impar", "impar_que_BATE", "impar_que_DIVERGE",
        "impar_dentro_da_faixa", "impar_FORA_da_faixa",
        "impar_no_conjunto_do_dia", "impar_SO_POR_CORTE_do_oraculo",
        "dia_sem_linha_no_espelho", "dia_sem_lavratura_no_espelho",
        "dia_isento_ou_ausencia", "dia_sem_trabalho_ambos",
        "esp_desconta_janela_declarada", "esp_lido_pelo_pago_h",
    )

    class TempoEsgotado(BaseException):
        pass

    class Descartar:
        # Descarte imediato: não acumula stdout/stderr livres em StringIO.
        def write(self, texto):
            return len(texto)

        def flush(self):
            return None

    def imprimir(*partes):
        print(*partes, file=saida, flush=True)

    def data_segura(valor):
        if isinstance(valor, dt.datetime):
            return valor.date().isoformat()
        if isinstance(valor, dt.date):
            return valor.isoformat()
        if isinstance(valor, str) and re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
            return dt.date.fromisoformat(valor).isoformat()
        return None

    def contexto(frame):
        col, dia = None, None
        while frame is not None:
            loc = frame.f_locals
            if col is None:
                objetos = [loc.get("colaborador"), loc.get("colab"), loc.get("c")]
                obj_self = loc.get("self")
                if obj_self is not None:
                    objetos.append(getattr(obj_self, "__dict__", {}).get("c"))
                for obj in objetos:
                    meta = getattr(type(obj), "_meta", None)
                    if getattr(meta, "model_name", None) == "colaborador":
                        pk = obj.pk
                        if isinstance(pk, int) and not isinstance(pk, bool):
                            col = "col%d" % pk
                            break
                if col is None:
                    for chave in ("colaborador_id", "colab_id"):
                        pk = loc.get(chave)
                        if isinstance(pk, int) and not isinstance(pk, bool):
                            col = "col%d" % pk
                            break
            if dia is None:
                # k é o dia julgado; dia já foi incrementado.
                # app/ponto/management/commands/e6_oraculo.py:321
                # app/ponto/management/commands/e6_oraculo.py:323
                for chave in ("k", "data", "dia", "data_turno"):
                    valor = data_segura(loc.get(chave))
                    if valor is not None:
                        dia = valor
                        break
            frame = frame.f_back
        return col or "-", dia or "-"

    def registrar(exc, frame=None):
        nonlocal erros_total

        # Marca na própria exceção: não conserva exc nem traceback num
        # dicionário global. Evita também deduplicação incorreta por id reusado.
        if exc.__dict__.get("_sonda_r1_registrada") is marca:
            return
        exc.__dict__["_sonda_r1_registrada"] = marca

        tb = exc.__traceback__
        ultimo = None
        while tb is not None:
            ultimo = tb
            tb = tb.tb_next

        col, dia = contexto(frame or (ultimo.tb_frame if ultimo else None))
        tipo = type(exc).__name__
        if not re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]*", tipo):
            tipo = "Excecao"

        origem = "-"
        if ultimo is not None:
            caminho = Path(ultimo.tb_frame.f_code.co_filename)
            if raiz is not None and caminho.is_relative_to(raiz):
                origem = "app/%s:%d" % (
                    caminho.relative_to(raiz).as_posix(), ultimo.tb_lineno
                )
            else:
                origem = "[fora-do-app]:%d" % ultimo.tb_lineno

        # Mesmo contrato de sanitização do anexo: mensagem livre suprimida.
        ultima_linha = (tipo + ": [mensagem suprimida]")[:200]
        erros_total += 1
        erros_por_tipo[tipo] += 1

        # Emite imediatamente; não guarda uma lista crescente de erros.
        imprimir("ERRO", competencia, (col, dia, tipo, origem, ultima_linha))

    def linhas_de_handlers(codigo):
        nome = codigo.co_filename
        if prefixo_app is None or not nome.startswith(prefixo_app):
            # Não memoiza todos os códigos externos vistos pelo tracer.
            return frozenset()
        if codigo in codigos:
            return codigos[codigo]
        if nome not in arquivos:
            with tokenize.open(nome) as fonte:
                arvore = ast.parse(fonte.read(), filename=nome)
            arquivos[nome] = {
                item.body[0].lineno
                for item in ast.walk(arvore)
                if isinstance(item, ast.ExceptHandler) and item.body
            }
        linhas_codigo = {linha for _, _, linha in codigo.co_lines() if linha}
        codigos[codigo] = frozenset(arquivos[nome] & linhas_codigo)
        return codigos[codigo]

    def copiar_acumuladores(frame):
        nonlocal bruto
        loc = frame.f_locals
        necessarios = ("T", "DONO_H", "DONO_C", "div", "_div_tot")
        if not all(chave in loc for chave in necessarios):
            return

        # Só lê o que a autoridade já acumulou; não refaz julgamento.
        # app/ponto/management/commands/e6_oraculo.py:367
        # app/ponto/management/commands/e6_oraculo.py:368
        # app/ponto/management/commands/e6_oraculo.py:369
        t, h, c = loc["T"], loc["DONO_H"], loc["DONO_C"]
        uniao = set()
        for dono in DONOS:
            uniao.update(c.get(dono, ()))
        bruto = {
            "dias": {dono: int(t.get("dono_" + dono, 0)) for dono in DONOS},
            # Mesma divisão da autoridade, sem a formatação %.1f.
            # app/ponto/management/commands/e6_oraculo.py:423
            "horas": {dono: repr(h.get(dono, 0) / 60.0) for dono in DONOS},
            "colabs": {dono: len(c.get(dono, ())) for dono in DONOS},
            "total_dias": int(loc["_div_tot"]),
            # Mesma união usada na saída nativa.
            # app/ponto/management/commands/e6_oraculo.py:429
            "total_colabs": len({x[1] for x in loc["div"]}),
            "uniao_colabs": len(uniao),
        }

    def rastrear(frame, evento, arg):
        nonlocal pulsos
        if evento == "call":
            pulsos += 1
            if pulsos % 256 == 0 and time.monotonic() >= limite:
                raise TempoEsgotado()
            if frame.f_code is codigo_handle:
                linhas_de_handlers(frame.f_code)
                return rastrear
            return rastrear if linhas_de_handlers(frame.f_code) else None
        if evento == "exception":
            pendentes[id(frame)] = arg[1]
        elif evento == "line":
            if frame.f_lineno in codigos.get(frame.f_code, ()):
                exc = sys.exc_info()[1] or pendentes.get(id(frame))
                if exc is not None:
                    registrar(exc, frame)
        elif evento == "return":
            try:
                if frame.f_code is codigo_handle:
                    copiar_acumuladores(frame)
            finally:
                pendentes.pop(id(frame), None)
        return rastrear

    def receber_log(logger, registro):
        if registro.exc_info and registro.exc_info[1] is not None:
            registrar(registro.exc_info[1], sys._getframe(1))

    def limpar():
        # Pendentes podem conservar exceções/tracebacks enquanto o frame vive.
        pendentes.clear()
        if resetar_queries is not None:
            resetar_queries()
        if invalidar_catalogo is not None:
            # Cache único de catálogo; não é cache crescente por colaborador.
            # app/ponto/catalogo/ausencias.py:257
            # app/ponto/catalogo/ausencias.py:261
            invalidar_catalogo()

        # Não limpa memos de motores vivos nem os acumuladores do comando.
        # Os memos pertencem à instância/cálculo:
        # app/ponto/motor_calculo_v2.py:848
        # app/ponto/motor_calculo_v2.py:2285
        # O registro de fontes é fraco e morre com os motores:
        # app/core/regua_cct.py:487
        # app/core/regua_cct.py:494
        gc.collect()

    def ler_relatorio(texto, erros_antes):
        linhas = texto.splitlines()
        padrao_counter = (
            r"\{(?:'[A-Za-z0-9_]+'\s*:\s*\d+"
            r"(?:,\s*'[A-Za-z0-9_]+'\s*:\s*\d+)*)?\}"
        )
        contadores = [
            ast.literal_eval(linha)
            for linha in linhas if re.fullmatch(padrao_counter, linha)
        ]
        donos = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*(ESTRUTURA|CADASTRO|BATIDA)\s+"
                r"(\d+) dia-colab\s+(\d+\.\d+) h\s+(\d+) colabs",
                linha
            ))
        ]
        soma = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*SOMA\s+(\d+)\s+"
                r"<- tem de ser igual a soma das classes \((\d+)\)", linha
            ))
        ]
        colabs = [
            m for linha in linhas
            if (m := re.fullmatch(r"colabs com divergencia: (\d+)", linha))
        ]
        falhas = [
            m for linha in linhas
            if (m := re.fullmatch(r"erros no espelho: (\d+)", linha))
        ]
        if not (
            len(contadores) == len(soma) == len(colabs) == len(falhas) == 1
            and len(donos) == 3
            and {m[1] for m in donos} == set(DONOS)
            and bruto is not None
        ):
            raise RuntimeError("FormatoDaAutoridadeIncompleto")

        soma_dias = sum(bruto["dias"].values())
        confere_dias = (
            soma_dias == bruto["total_dias"]
            == int(soma[0][1]) == int(soma[0][2])
        )
        confere_colabs = (
            bruto["uniao_colabs"] == bruto["total_colabs"] == int(colabs[0][1])
        )
        for m in donos:
            dono = m[1]
            if (
                int(m[2]) != bruto["dias"][dono]
                or int(m[4]) != bruto["colabs"][dono]
                or m[3] != format(float(bruto["horas"][dono]), ".1f")
            ):
                raise RuntimeError("AcumuladoresDaAutoridadeDivergentes")

        if not confere_dias or not confere_colabs:
            raise RuntimeError("SomaDaAutoridadeDivergente")
        if selecionados[competencia] != quantidades[competencia]:
            raise RuntimeError("UniversoDaParteDivergente")
        if int(falhas[0][1]) and erros_total == erros_antes:
            raise RuntimeError("ErroDoEspelhoSemDetalhamento")

        imprimir("COMPETENCIA", competencia)
        t = contadores[0]
        imprimir("[1]", {chave: t.get(chave, 0) for chave in CHAVES})
        for linha in linhas:
            if re.fullmatch(
                r"BATE \(ate 10 min\): \d+\.\d+% de \d+ dias comparados", linha
            ):
                imprimir("[2]", linha)
        for m in donos:
            imprimir("[3]", m[0].strip())
        imprimir("[4] TOTAL divergentes", soma[0][2])
        imprimir("[5]", colabs[0][0])
        imprimir("erros_no_espelho", falhas[0][1])

        # Decimal soma exatamente as representações decimais emitidas.
        # Não usa as horas arredondadas da tabela nativa.
        with localcontext() as ctx:
            ctx.prec = 80
            horas = {dono: Decimal(bruto["horas"][dono]) for dono in DONOS}
            total_horas = sum(horas.values(), Decimal(0))
            texto_total_horas = format(total_horas, "f")
            confere_horas = (
                sum((Decimal(bruto["horas"][d]) for d in DONOS), Decimal(0))
                == Decimal(texto_total_horas)
            )

        for dono in DONOS:
            imprimir(
                "PARCIAL|%s|%s|dias=%d|horas=%s|colabs=%d" % (
                    competencia, dono, bruto["dias"][dono],
                    bruto["horas"][dono], bruto["colabs"][dono],
                )
            )
        imprimir(
            "PARCIAL_TOTAL|%s|dias=%d|horas=%s|colabs=%d" % (
                competencia, bruto["total_dias"],
                texto_total_horas, bruto["total_colabs"],
            )
        )
        somas[competencia] = (
            soma[0][0].strip(), confere_dias,
            texto_total_horas, confere_horas,
            bruto["total_colabs"], confere_colabs,
        )

    try:
        with (
            patch.object(logging.Logger, "handle", receber_log),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.core.management import call_command
            from django.db import connection, reset_queries, transaction
            from ponto import janelas
            from ponto.catalogo import ausencias
            from ponto.management.commands import e6_oraculo
            from ponto.services import espelho as modulo_espelho

            raiz = Path(e6_oraculo.__file__).resolve().parents[3]
            prefixo_app = str(raiz) + "/"
            codigo_handle = e6_oraculo.Command.handle.__code__
            resetar_queries = reset_queries
            invalidar_catalogo = ausencias.invalidar_cache

            if (
                type(PARTE) is not int
                or type(PARTES) is not int
                or PARTES != 6
                or not 1 <= PARTE <= PARTES
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
                or rastreador_anterior is not None
            ):
                raise RuntimeError("AmbienteIncompativel")

            Empresa = apps.get_model("colaboradores", "Empresa")
            FM = apps.get_model("ponto", "FechamentoMensal")
            Colaborador = apps.get_model("colaboradores", "Colaborador")
            gerente = Colaborador.objects
            filtro_real = gerente.filter
            real_espelho = modulo_espelho.espelho_do_colab

            with transaction.atomic():
                try:
                    with connection.cursor() as cursor:
                        cursor.execute("SELECT current_database(), current_user")
                        if cursor.fetchone() != ("sombra", "leitor"):
                            raise RuntimeError("BancoOuPapelIncompativel")

                    empresas = list(
                        Empresa.objects.filter(ativa=True).order_by("pk")
                    )
                    imprimir("PARTE", PARTE, "PARTES", PARTES)
                    for mes in (10, 11):
                        competencia = "%02d/2026" % mes
                        total = quantidade = 0
                        ids = (
                            FM.objects.filter(
                                mes=mes, ano=2026,
                                colaborador__empresa__ativa=True,
                            )
                            .values_list("colaborador_id", flat=True)
                            .distinct()
                            .iterator(chunk_size=128)
                        )
                        try:
                            for pk in ids:
                                if type(pk) is not int:
                                    raise RuntimeError("IdentificadorIncompativel")
                                total += 1
                                quantidade += int(pk % PARTES == PARTE - 1)
                        finally:
                            ids.close()
                        quantidades[competencia] = quantidade
                        universo_parte_total += quantidade
                        imprimir("[0] UNIVERSO", competencia, total)
                        imprimir("UNIVERSO_PARTE", competencia, quantidade)

                        janelas_vistas = {
                            janelas.janela_fechamento(mes, 2026, empresa)
                            for empresa in empresas
                        }
                        for ini, fim in sorted(janelas_vistas):
                            imprimir(
                                "JANELA", competencia,
                                ini.isoformat(), fim.isoformat(),
                            )
                    del empresas
                finally:
                    transaction.set_rollback(True)
            limpar()

            def iterar_colaboradores(queryset):
                it = queryset.iterator(chunk_size=1)
                try:
                    for c in it:
                        if time.monotonic() >= limite:
                            raise TempoEsgotado()
                        if c.pk % PARTES != PARTE - 1:
                            raise RuntimeError("ColaboradorForaDaParte")
                        selecionados[competencia] += 1
                        try:
                            yield c
                        finally:
                            # O próximo passo do iterador acontece depois de
                            # toda a comparação/juiz deste colaborador.
                            # app/ponto/management/commands/e6_oraculo.py:302
                            # app/ponto/management/commands/e6_oraculo.py:392
                            limpar()
                finally:
                    it.close()

            def filtrar_colaboradores(*args, **kwargs):
                # Intercepta só a seleção de colaboradores do handle real.
                # app/ponto/management/commands/e6_oraculo.py:302
                if sys._getframe(1).f_code is not codigo_handle:
                    return filtro_real(*args, **kwargs)
                if args or set(kwargs) != {"pk__in"}:
                    raise RuntimeError("SelecaoDaAutoridadeIncompativel")
                ids = kwargs["pk__in"]
                if any(type(pk) is not int for pk in ids):
                    raise RuntimeError("IdentificadorIncompativel")
                ids_parte = [pk for pk in ids if pk % PARTES == PARTE - 1]
                return iterar_colaboradores(filtro_real(pk__in=ids_parte))

            def observar_espelho(*args, **kwargs):
                nonlocal chamadas
                if time.monotonic() >= limite:
                    raise TempoEsgotado()
                if chamadas and chamadas % 25 == 0:
                    decorrido = time.monotonic() - inicio
                    estimativa = (
                        decorrido * universo_parte_total / chamadas / 60
                    )
                    imprimir(
                        "ESTIMATIVA", "processados", chamadas,
                        "universo", universo_parte_total,
                        "minutos_total", round(estimativa, 1),
                        "ate_40_min", estimativa 

[truncado em 20000 de 27052 caracteres]

## 06/11 11:23 · sessao e60937eb81bd · - · - · origem=Ronald (tmux)

Contagem manual do **primeiro anexo `script.py`**, considerando `def sonda_r1():` como linha 1:

- `vistos = {}` — `script.py:19`.
- `vistos[chave] = exc` — `script.py:86`.
- `erros.append(...)` — `script.py:106`.
- `pendentes[id(frame)] = arg[1]` — `script.py:136`.
- `redirect_stdout(io.StringIO())` — `script.py:232`.
- `redirect_stderr(io.StringIO())` — `script.py:233`.

Âncoras: `def rastrear` — `script.py:128`; `def receber_log` — `script.py:146`; `def ler_relatorio` — `script.py:151`; `try:` externo — `script.py:229`.

_2250 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
