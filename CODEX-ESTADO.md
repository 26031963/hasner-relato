# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 04:21** · publicado em **15/11/2026 04:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 04:21 · sessao 3453bc3dc40d · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Entrega em [c13_entrega.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_entrega.txt:1), com RED/GREEN, comandos, censo e mutantes.

[26 testes OK](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_green.txt:30); [13/13 mutantes mortos](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_mutantes.txt:341). [nao verificado]

Pendências explícitas: [copy/replace sem origem local ainda escapa](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_entrega.txt:15) e [Ruff indisponível](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_checks.txt:6). Certificação suite/Opus: [nao verificado]. [nao verificado]

## 15/11 04:05 · sessao 0c663ade5d11 · - · - · origem=Ronald (tmux)

Censo somente por leitura; nenhuma escrita.

- `chamados/models.py:1541`: cópia de pergunta do queryset de `:1530`; `:1544`: cópia de `_state`. Falsos positivos para ResultadoMes.
- `chamados/services/carga_pergunta_viva.py:177`: cópia de batidas, com filtro sobre `timestamp` em `:178`; classe herda `CargaDaJanela` em `:83`. Falso positivo.
- `escala/utils.py:1929`: cópia de célula do mapa produzido por `celulas_do_periodo` em `:1351`, iterado em `:1924`. Falso positivo.
- `ponto/portas/celula.py:1001`: cópia de célula para sobrepor ata/veredito em `:1002-1005`. Falso positivo.
- `ponto/turnos.py:1543`, `:1999`, `:3121`: cópias de batidas; origem da carga em `:3101-3105`. Falsos positivos.
- `ponto/motor_calculo_v2.py:2268`: cópias das batidas de entrada, usadas em `:2273`. Falso positivo.
- `core/management/commands/describe.py:71`: membership em `alvo.__dict__` para listar métodos, sem transportar valores. Falso positivo.

Os demais `__dict__` encontrados fazem seleção de chave (`get`, `pop`, `setdefault`, subscrito), como `chamados/juizes.py:1112` e `escala/alimentacao.py:43`. Separar seleção de chave de abertura inteira é um estreitamento estrutural útil.

Não há prova de tipo em `def f(resultado): return {**vars(resultado)}`. Capturá-lo exige tratar parâmetro desconhecido conservadoramente; distinguir esse parâmetro de `cel` em `ponto/portas/celula.py:1001` exige proveniência por chamadas/tipos. Exclusão apenas por nomes ou por acesso a atributos estranhos é insegura: ResultadoMes admite atributos dinâmicos, e o motor o constrói em `ponto/motor_calculo_v2.py:2274`.

Tipos efetivos em execução e ocorrência em produção: [nao verificado].

## 15/11 03:58 · sessao 90dd9a6ed7b7 · JUSTIFICATIVA-PAGA-O-PERIODO · verificador · origem=Code

**Há bloqueio: K1 ainda deixa passar leitores novos, inclusive mutantes que sobrevivem aos 12 testes da própria catraca.** O rastreador não acompanha cadeias de aliases e descarta módulos sem os nomes literais dos campos (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:147`).

1. **PROCEDE parcialmente — bloqueia/fila: os achados do v7 estão mencionados, mas nem todos estão curados ou têm destino verificável.** A seção 6 lista os resíduos; para alguns, apenas diz FORA (`app/ponto/tests/c12_entrega.txt:138`).

| Achado do v7 | Conferência nesta cópia | Gravidade restante |
|---|---|---|
| 1. Exportada sem proteção genérica | **FORA**, destino declarado: sessão mede 09/10. Continua existindo somente a guarda de 09/2026; exportação nas demais competências gera aviso e permite persistência (`app/ponto/tests/c12_entrega.txt:141`; `app/ponto/services/fechamento.py:36`, `:539`, `:1157`, `:1194`). | **bloqueia** |
| 2. Lavratura converte `None` em zero | **FORA**, sem destino inequívoco. A seção 6 remete a “pergunta de lei (c)”, mas a pergunta local 5(c) trata de K1. A conversão continua (`app/ponto/tests/c12_entrega.txt:138`, `:132`; `app/ponto/services/dia_pago.py:292`, `:490`). | **bloqueia** |
| 3. Template não lê o rótulo | **Fechado pelo hunk** de `espelho.html`, `@@ -92,6 +92,16 @@`: agora consulta motivo e dias desconhecidos (`app/templates/ponto/espelho.html:100`). | — |
| 4. Rótulo usa janela visual inteira | **Fechado nesse ponto pelo hunk** de `espelho.py`, `@@ -1260,8 +1260,11 @@`: conjunto recortado por competência. Permanecem as diferenças descritas no item 3 abaixo (`app/ponto/services/espelho.py:1266`). | **fila**, para os resíduos |
| 5. Escape literal de K1 | **Escape original fechado** pelo detector de módulo dinâmico e pelos casos acrescentados; o contrato completo continua furado, conforme item 4 (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:93`, `:289`). | **bloqueia** |
| 6. Mutante `any → all` | **Fechado pelo hunk novo de testes c12**: fixture exige mapa misto `{D: None, D2: 180}` e conservação dos demais campos. O mutante morre (`app/ponto/tests/test_justificativa_c12_sem_banco.py:70`, `:77`; `app/ponto/tests/c12_mutantes.txt:4`). | — |
| 7. Mutante `discard → clear` | **Fechado pelo hunk novo de testes c12**: ausência inteira em D conserva VIZ desconhecido. O mutante morre (`app/ponto/tests/test_justificativa_c12_sem_banco.py:92`; `app/ponto/tests/c12_mutantes.txt:8`). | — |
| 8. Precedência da ausência inteira diverge na grade | **FORA**, sem encaminhamento concreto na seção 6. O feed remove o desconhecido, mas a grade financeira recebe os períodos crus (`app/ponto/tests/c12_entrega.txt:139`; `app/ponto/services/efeito_ausencia.py:75`; `app/folha/export.py:339`). | **fila** |
| 9. Cartão sobrescreve parcialmente o rótulo | **FORA**, sem encaminhamento concreto. Horas podem ser numéricas com dias desconhecidos; motivo posterior `None` não limpa o anterior (`app/ponto/tests/c12_entrega.txt:139`; `app/relatorios/cartao_pela_celula.py:187`, `:208`, `:214`). | **fila** |
| 10. Causa falsa de zero | **Fechado para o caso marcado `sem_ata`**, pelo hunk de `causas.py`, `@@ -83,6 +83,15 @@`, e teste com classificador real (`app/folha/causas.py:91`; `app/ponto/tests/test_justificativa_c12_sem_banco.py:145`). | — |
| 11. Relógio fora da passada | **FORA**, sem encaminhamento concreto. A retenção continua chamada sem `hoje` e alcança `localdate()` (`app/ponto/tests/c12_entrega.txt:140`; `app/ponto/services/fechamento.py:1080`; `app/folha/export.py:1248`). | **fila** |

2. **NÃO PROCEDE a persistência de `realizado_zero` nos leitores da causa corrigida; PROCEDE a quebra mais ampla de `None` — bloqueia.** Para previsto positivo, soma conhecida zero e algum dia marcado `sem_ata`, o classificador retorna `indeterminado`. O fechamento grava essa resposta; retrato e lista leem o campo gravado. O retrato traduz `indeterminado` para “sem causa”, e o template da lista mostra “a apurar” (`app/folha/causas.py:92`; `app/ponto/services/fechamento.py:1093`; `app/folha/retrato_folha.py:249`, `:1115`; `app/ponto/services/fechamento_lista.py:122`; `app/templates/ponto/_fech_linha.html:18`).

   O censo de escritores da marca precisa incluir **também o builder**: `escala/utils.py` transporta `motivo='sem_ata'` para `motivo_justificativa`, além do overlay do leitor de célula. Ambos recebem a indicação da mesma fonte de períodos; isso, por si, não demonstra juiz novo (`app/escala/utils.py:1259`, `:1281`; `app/escala/services/leitor_celula.py:479`; `app/ponto/services/efeito_ausencia.py:165`).

   O contrato global ainda não é “`None` nunca vira zero”: lavratura e retrato de exportada fazem exatamente essa conversão (`app/ponto/services/dia_pago.py:292`, `:490`, `:495`). Há ainda consumidor que acusa zero: com `minutos_realizados=None`, previsto 480 e `n_ok=2`, `supra_juiz.classificar_dia` produz `REALIZADO_ZERO_COM_TURNO`, pois começa por `None or 0`. É contraprova do consumidor, distinta de `causa_espelho`; ocorrência desse formato em produção **[nao verificado]** (`app/ponto/supra_juiz.py:237`, `:294`). **Fila** para esse consumidor.

3. **NÃO PROCEDE a existência de autoridade nova para o rótulo; PROCEDE a identidade incompleta das janelas — fila.** Tela e PDF chamam `rotular_abono_sem_apuracao`; template e badge exigem dias `None` e motivo `sem_ata` (`app/ponto/services/espelho.py:1266`; `app/relatorios/pdf_espelho.py:483`, `:790`; `app/templates/ponto/espelho.html:100`).

   O recorte nominal por competência foi corrigido, mas os universos efetivos diferem: a tela pede autoridade até **hoje**, enquanto o PDF aplica teto em **ontem**. Um dia de hoje justificado sem ata pode entrar no conjunto da tela e ficar fora do PDF (`app/ponto/services/espelho.py:1236`, `:1267`; `app/relatorios/pdf_espelho.py:287`, `:333`).

   Além disso, somente o **rótulo** foi recortado: `dias_abono` continua copiado do resultado da janela visual. Quando o rótulo de competência anterior deixa de esconder esse número, ele pode aparecer no resumo atual; o mapeamento do fechamento não fornece `dias_abono` para substituí-lo. A própria entrega registra essa pendência (`app/ponto/services/espelho.py:1261`; `app/relatorios/cartao_pela_celula.py:93`; `app/ponto/tests/c12_entrega.txt:122`).

   **NÃO PROCEDE parâmetro novo com default neste delta:** a assinatura conserva os parâmetros existentes, com `agora` obrigatório. O hunk altera somente o argumento do helper (`app/ponto/services/espelho.py:1203`, `:1266`). Os 12 chamadores estão enumerados na entrega (`app/ponto/tests/c12_entrega.txt:112`).

4. **PROCEDE — bloqueia: K1 ainda tem escapes estáticos.** O exemplo literal do v7 agora é capturado, inclusive quando injetado na tela (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:289`, `:296`).

   Entretanto, esta variante passa: o detector reconhece `g = getattr`, mas não reconhece `h = g`; a chave do dicionário volta a ser descartada (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:125`).

   ```python
   g = getattr
   h = g
   def _escape(resultado):
       nomes = {'dias_abono': 1}
       return [h(resultado, k) for k in nomes]
   ```

   Em reprodução somente em memória, essa fonte anexada a `folha/causas.py` devolveu `[1]`, manteve a allowlist sem diferenças e sobreviveu aos **12 testes K1**. A atribuição `g, h = getattr, getattr` também escapa pelo tratamento limitado dos destinos (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:104`, `:179`).

   Outro escape é `return {**vars(resultado)}`, equivalente a `{**resultado.__dict__}`: transporta os dois campos sem nomeá-los. O filtro textual descarta o módulo antes de analisar esses acessos; a variante `vars` também sobreviveu aos 12 testes. `ResultadoMes` é dataclass com ambos os campos (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:147`; `app/ponto/motor_calculo_v2.py:268`, `:284`, `:292`).

   `vars(resultado)['dias_abono']` e `resultado.__dict__['dias_abono']` são detectados. Subscript direto é ignorado em módulo não dinâmico, mas seu funcionamento sobre o resultado real **[nao verificado]** (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:107`, `:127`).

5. **PROCEDE parcialmente — fila: nem toda ata nasce pela porta real.** As duas fixtures usam `montar_dna`, mas `_justificar` ainda calcula os números e grava `ata` por `QuerySet.update`. Ela própria declara que não prova a escrita do cartório (`app/ponto/tests/test_justificativa_paga_o_periodo.py:204`, `:210`, `:219`, `:224`). A aprovação posterior aciona o signal do cartório; isso não elimina a inicialização manual anterior (`app/ponto/tests/test_justificativa_paga_o_periodo.py:225`; `app/ponto/signals.py:165`, `:93`).

   A fixture do PDF chama explicitamente `julgar_celula`, recarrega e exige W=0/P>0 (`app/relatorios/test_espelho_falta.py:48`, `:49`, `:51`).

   **NÃO PROCEDE troca dos esperados positivos no delta fornecido:** continuam duas pausas de 85 minutos e os dois esperados **550**; continua **1 dia de abono** no PDF. O caso irmão sem ata exige `None/None/sem_ata` (`app/ponto/tests/test_justificativa_paga_o_periodo.py:282`, `:296`, `:297`; `app/relatorios/test_espelho_falta.py:97`, `:71`). Houve enfraquecimento declarado de outro teste, P3, de igualdade com 1 para `IsNotNone`; ele verifica ausência do rótulo, não a correção do total (`app/ponto/tests/c12_entrega.txt:67`; `app/ponto/tests/test_justificativa_c12_sem_banco.py:54`).

6. **PROCEDE — bloqueia/fila: permanecem selo sem mordida e campos com significados divergentes.** K1 não garante a exclusividade dos leitores; `None` continua significando desconhecido no fechamento e zero no `DiaPago`; o resumo pode conservar dias desconhecidos com horas numéricas ou motivo residual (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:147`; `app/ponto/services/fechamento.py:1073`; `app/ponto/services/dia_pago.py:292`; `app/relatorios/cartao_pela_celula.py:187`, `:214`).

   **NÃO PROCEDE fórmula ou juiz novo nos três hunks produtivos c12:** eles acrescentam diagnóstico, recorte do conjunto e leitura do resumo. A fórmula continua na fonte de períodos (`app/folha/causas.py:91`; `app/ponto/services/espelho.py:1266`; `app/templates/ponto/espelho.html:100`; `app/ponto/services/efeito_ausencia.py:170`). O relógio fora da passada permanece como resíduo declarado (`app/ponto/services/fechamento.py:1080`; `app/folha/export.py:1248`).

O verde de 6477 testes do recorte foi informado no dado; sua execução independente nesta volta e a suíte inteira são **[nao verificado]**. [nao verificado]

**REPROVADO para liberar a suíte inteira nesta pré-verificação**, pelo escape sobrevivente de K1 e pelos bloqueios de exportada/`None` ainda abertos (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:147`; `app/ponto/services/fechamento.py:36`; `app/ponto/services/dia_pago.py:292`).

## 15/11 03:56 · sessao 3e3b5a8340d3 · - · - · origem=Ronald (tmux)

Item 4 — **PROCEDE, bloqueia**: o escape literal do v7 está coberto pelos sete casos e pela injeção na tela, mas K1 ainda admite novos leitores estáticos. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:217`, `:289`, `:296`)

Reprodução em memória, anexando cada fonte a `folha/causas.py` na árvore viva:

```python
g = getattr
h = g
def _escape(resultado):
    nomes = {'dias_abono': 1}
    return [h(resultado, k) for k in nomes]
```

Também passa com `g,h=getattr,getattr`. O rastreador reconhece somente atribuição cujo valor pertence diretamente a `DINAMICOS` e cujo destino é `ast.Name`; não acompanha cadeia nem desempacotamento. Ambos devolvem `[1]`, mantêm `fora_da_allowlist={}` e sobrevivem aos **12 testes completos da K1**. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:103`, `:105`, `:125`)

Outro escape:

```python
def _escape(resultado):
    return {**vars(resultado)}
```

O equivalente `{**resultado.__dict__}` também transporta os dois abonos. Sem literal `dias_abono`/`horas_abono` no módulo, o filtro nem analisa seu AST; a versão `vars` sobrevive aos **12 testes completos da K1** quando injetada em `folha/causas.py`. A classe real permite isso: `ResultadoMes` é dataclass e declara ambos os campos. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:147`, `:148`; `app/ponto/motor_calculo_v2.py:268`, `:284`, `:292`)

`vars(resultado)['dias_abono']` e `resultado.__dict__['dias_abono']` são detectados; o subscript direto `resultado['dias_abono']` é ignorado, mas seu funcionamento no objeto real não foi demonstrado. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:107`, `:118`, `:127`; funcionamento do subscript direto: [nao verificado])

As execuções usaram `PYTHONDONTWRITEBYTECODE=1`/`-B`, fontes e mutações somente em memória. [nao verificado]

## 15/11 03:54 · sessao 3ea9d6f3a750 · - · - · origem=Ronald (tmux)

**Item 5: PROCEDE parcialmente; gravidade fila.** A primeira fixture ainda fabrica a ata por `ata.update` e `CelulaDia.objects.filter(...).update(ata=ata)`, embora use `montar_dna` real. Ela própria declara que não prova a escrita do cartório (`app/ponto/tests/test_justificativa_paga_o_periodo.py:204-224`). O selo exclui diretórios `tests`, portanto não certifica esse caminho (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:171-174`).

Há uma nuance: depois da escrita manual, a fixture cria a justificativa aprovada (`app/ponto/tests/test_justificativa_paga_o_periodo.py:225-226`); seu `post_save` chama o cartório (`app/ponto/signals.py:161-165`, `:85-93`). A execução efetiva dessa relavratura nesta verificação é **[nao verificado]**.

**A segunda fixture usa a porta real:** cria a célula com `montar_dna`, chama `julgar_celula`, recarrega e exige realizado zero e previsto positivo (`app/relatorios/test_espelho_falta.py:43-52`). O cartório monta a ata e passa-a à lavra (`app/ponto/services/cartorio.py:1850-1874`); sem realizado, grava `int(... or 0)` (`:738-740`).

**Não houve troca dos esperados positivos no delta fornecido:** continuam duas pausas de 85 minutos e duas asserções de **550**, na fonte e na grade (`app/ponto/tests/test_justificativa_paga_o_periodo.py:282-297`); o PDF conserva **um dia de abono**, uma falta e sua data (`app/relatorios/test_espelho_falta.py:94-97`). O caso irmão acrescentado exige `None/None/'sem_ata'` (`:60-73`). O produtor real inclui o segundo par de pausas no DNA (`app/ponto/management/commands/gerar_celulas.py:90-103`).

A prova de execução ORM nesta verificação é **[nao verificado]**.

## 15/11 03:51 · sessao 41ee8830bc04 · BATIDA-DEDUP-RETRATADA · verificador · origem=Code

Referências relativas a `app/`.

1. **PROCEDE o fechamento das três lacunas.** Os mutantes são os mesmos descritos no v3: acrescentar `sequencia=1`, condicionar a marca a `timestamp.second == 0` e remover a ordenação da oposta na triagem. Existem controles verdes separados. Reexecutei C1–C4: **37 testes OK**. (`ponto/tests/test_dedup_retratada_c4.py:47`, `:55`, `:75`, `:83`, `:100`, `:106`)

   Não encontrei passagem vazia: `SITIOS` contém as três cascas; os mapas alterados são lidos ao compilar a porta e os trechos reais das cascas. As mortes ocorreram nas verificações esperadas: porta indevidamente chamada, efeito `ordem` ausente e id1 apresentado em vez de id2. O executor usa ORM e emissores simulados; não certifica HTTP/SQL. (`ponto/tests/test_dedup_retratada_c1.py:20`; `ponto/tests/test_dedup_retratada_c2.py:77`, `:109`, `:132`; `ponto/tests/test_dedup_retratada_c4.py:43`, `:68`, `:97`)

2. **NÃO PROCEDE classificar esse sítio como novo juiz E/S.** A comparação escolhe somente o rótulo usado na mensagem; criação/reuso é decidido pela marca da porta. (`ponto/services/resposta_batida.py:14`, `:20`, `:27`)

   Quem lê a allowlist é `NoRawTipoJudgmentContract`: ela tolera **até uma comparação detectada no arquivo inteiro**, sem vincular a permissão à linha ou conferir se continua sendo exibição. Portanto, a exceção é mais ampla que o comentário. (`ponto/tests/test_contract_no_raw_tipo_judgment.py:24`, `:48`, `:73`)

   Há alternativa sem ampliar a allowlist: `batida.get_tipo_display()`, baseado nas escolhas do campo. Atenção à redação: o catálogo fornece **“Saida”**, enquanto o helper escreve **“Saída”**. (`ponto/models.py:9`, `:24`; `ponto/services/resposta_batida.py:20`)

3. **PROCEDE o registro das linhas atuais; equivalência histórica completa `[nao verificado]`.** O censo reexecutado encontrou precisamente `719` e `713`, ambas na expressão `data_turno=_tzc9.localdate(batida.timestamp).isoformat()`, no ramo de desvio leve. A catraca de linhas passou. (`api/views.py:719`; `api/views_core.py:713`; `core/tests/test_medida_turno_do_par.py:256`, `:549`)

   Isso confirma as ocorrências atuais, mas a catraca não compara expressões históricas nem consulta Git. A prova direta de que são as mesmas derivações de `880331a8c`, com deslocamento líquido de três linhas, permanece **`[nao verificado]`**. (`core/tests/test_medida_turno_do_par.py:17`, `:550`)

4. **NÃO PROCEDE dizer que a corrida SQL desapareceu do relato.** Continua explicitamente registrada como preexistente, “listado sem cura”; os dois `create` permanecem sem captura diante da unicidade colaborador/timestamp. Encaminhamento operacional posterior: **`[nao verificado]`**. (`ponto/tests/dedup_retratada_c3_entrega.txt:163`; `ponto/registro_batida.py:152`, `:158`; `ponto/models.py:106`)

**REPROVADO para seguir ao opus final:** falta comprovar a equivalência histórica solicitada no item 3; os testes atuais não fornecem essa prova. (`core/tests/test_medida_turno_do_par.py:17`, `:550`)

## 15/11 03:39 · sessao 0556b8c85746 · E3-RESTO-R3 · leitura · origem=Code

Os 16 casos se dividem em **2 FIXTURE, 11 EXPECTATIVA QUE CODIFICA UMA REGRA e 3 CONHECIDO**, conforme as cadeias abaixo. Os valores obtidos são os transcritos no `fails.txt` fornecido; reprodução própria da execução e comprovação pela história Git da c6 exata `a5ff3627f`: **[nao verificado]**. A entrega local declara essa base, mas também registra sua identidade Git como não verificada (`app/chamados/tests/e3_r3_c7_entrega.txt:2`).

**Cadeia compartilhada pelos 11 casos de expectativa**

Esses testes afirmam que uma resposta sob âncora **terminal** oferece modal administrativo. Isso é literal em “âncora terminal COM modal”, “inclusive terminal” e “mesmo terminal”; portanto trocar a âncora por uma viva mudaria a prova pretendida (`app/chamados/tests/test_f3_beco1_reata_limbo.py:29`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:79`; `app/chamados/tests/test_e4_passivo.py:8`).

A cadeia aberta é:

- **Identidade e estado:** as fixtures escrevem `chamado_destino`, o chamado container e `status_local`; `chamado_que_lista` lê primeiro o destino, depois o container. `resolvido` e `fechado` pertencem a `TERMINAIS` (`app/chamados/services/fio.py:116`; `app/chamados/catalogo/motor.py:30`).
- **Resposta → ação exigível:** a carga lê as decisões E1; `_pela_decisao` lê a última resposta textual e escreve a ação `validar/admin`. Se a âncora estiver fora da fila, a ação conserva `porta=None` (`app/chamados/services/tarefa_do_dia.py:419`; `app/chamados/services/tarefa_do_dia.py:150`; `app/chamados/services/tarefa_do_dia.py:160`).
- **Estado terminal → alimentação:** o construtor lê `status_local`, escreve `_ancoras_terminais`, e `tarefa::ler` transporta o conjunto para `alimentacao['ancoras_terminais']` (`app/chamados/services/tarefa_do_dia.py:448`; `app/chamados/services/tarefa_do_dia.py:449`; `app/chamados/services/tarefa_do_dia.py:589`).
- **Ação → porta publicada:** `_portas_da_tarefa` exclui a validação por âncora terminal de `admin_alcancavel`. Sem outro dono, escreve `fio='nada'`, `rota=None`, `acao=None`; `porta_da_tarefa` devolve `None` para essa ausência conhecida de porta (`app/chamados/services/tarefa_do_dia.py:652`; `app/chamados/services/tarefa_do_dia.py:656`; `app/chamados/services/tarefa_do_dia.py:667`; `app/chamados/services/tarefa_do_dia.py:696`; `app/chamados/services/tarefa_do_dia.py:708`).

**A c7 participa diretamente nos 11:** hunks de `chamados/services/tarefa_do_dia.py::_carga_da_tarefa_cls` — `__init__` e `tarefa::ler` — e de `::_portas_da_tarefa`, especialmente o filtro de terminais (`app/chamados/services/tarefa_do_dia.py:407`; `app/chamados/services/tarefa_do_dia.py:449`; `app/chamados/services/tarefa_do_dia.py:589`; `app/chamados/services/tarefa_do_dia.py:656`).

O aval LU-03 registrado diz **“acao direta do admin com tela = porta”**; LU-04 diz **“pergunta em marco dispensado, fora de marco ou porta None NAO da dono”**. Nenhuma dessas frases, isoladamente, declara modal sob terminal (`app/docs/CORTES.md:435`; `app/docs/CORTES.md:436`). Já o sítio da costura declara, citando O261-E F1, aval Ronald 30/10 17:3x: **“ancora terminal e fio morto -- o admin nao a ve, o colab a ve”** (`app/chamados/services/fio.py:786`; `app/chamados/services/fio.py:789`). O aval específico que autorizaria a extensão dos testes, “inclusive terminal”, permanece **[nao verificado]**.

1. **`test_v8_R10_ancora_fora_do_fato_sem_acao` — FIXTURE.**

   O traceback inteiro fornecido percorre `unittest/case.py:58` → `:651` → `:606` → teste `:626` → apoio `:150` → `tarefa_do_dia.py:598` → `:589`. A exceção é **`AttributeError: 'CargaDaTarefa' object has no attribute '_ancoras_terminais'`**, originada na leitura de `self._ancoras_terminais` em `app/chamados/services/tarefa_do_dia.py:589`; os fontes externos de `unittest` não foram abertos **[nao verificado]**.

   A chamada falha antes das asserções: esperava-se nenhuma ação nas instâncias e `('porta_fila', (202,), {202: 'decidir'})`; não houve valor de tarefa obtido (`app/chamados/tests/test_e3_portas_tarefa.py:626`; `app/chamados/tests/test_e3_portas_tarefa.py:627`; `app/chamados/tests/test_e3_portas_tarefa.py:628`).

   **Escritor/leitor:** o apoio cria a carga por `cls.__new__`, preenche seus atributos manualmente e chama `obj.tarefa`; assim contorna o construtor que escreve `_ancoras_terminais` (`app/chamados/tests/e3_v8_apoio.py:130`; `app/chamados/tests/e3_v8_apoio.py:132`; `app/chamados/tests/e3_v8_apoio.py:150`; `app/chamados/services/tarefa_do_dia.py:449`). Em produção, `carga_da_tarefa` chama o construtor normal (`app/chamados/services/tarefa_do_dia.py:711`).

   **C7:** participa pelo hunk `CargaDaTarefa.tarefa::ler`, que introduziu a leitura obrigatória (`app/chamados/services/tarefa_do_dia.py:589`). **Cura:** completar a fixture com o conjunto de terminais correspondente ao cenário; aqui, conjunto vazio conserva a prova sobre âncora fora do fato, sem acrescentar terminalidade que o teste não monta. A fixture sintética irmã já recebeu exatamente essa inicialização na c7 (`app/chamados/tests/test_e3_c8b_sem_banco.py:247`).

2. **`test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

   Falha em `app/chamados/tests/test_e4_passivo.py:65`: esperado conter **`-> resolvido`**, obtido **`-> segue_sem_porta`**.

   **Escritor/leitor:** a fixture monta resposta sob destino `resolvido`; o comando identifica o passivo sem linha, registra a decisão E1, mede novamente o alcance e escreve `segue_sem_porta` quando o dia permanece sem porta. A impressão do comando lê esse resultado (`app/chamados/tests/test_e4_passivo.py:59`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:352`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:368`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:378`).

   **Regra literal:** “VALIDAR do admin tem modal na ancora existente, mesmo terminal (LU-03)”. Afirma precisamente **terminal → porta do admin**; o filtro terminal da cadeia comum participa (`app/chamados/tests/test_e4_passivo.py:8`; `app/chamados/services/tarefa_do_dia.py:656`).

3. **`test_MORDE_dry_nao_escreve` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

   Falha em `app/chamados/tests/test_f3_beco1_reata_limbo.py:74`: esperado **`0 pergunta(s) em 0 pai(s)`**, obtido **`1 pergunta(s) em 1 pai(s)`**.

   **Escritor/leitor:** `_beco` monta a âncora terminal; `_pai` escreve pai `aberto`. O leitor C11 admite o dia quando `porta_da_tarefa` é `None`, escreve sua assinatura, e o comando agrupa as perguntas por pai e imprime as contagens (`app/chamados/tests/test_f3_beco1_reata_limbo.py:28`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:38`; `app/escala/services/cadastro_realidade.py:472`; `app/escala/services/cadastro_realidade.py:515`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:68`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:74`).

   **Regra:** terminal COM modal, logo nenhum candidato C11. A falha é na seleção; o comando retorna no ramo DRY antes da escrita de destinos (`app/chamados/tests/test_f3_beco1_reata_limbo.py:29`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`). Participa o filtro terminal da c7 (`app/chamados/services/tarefa_do_dia.py:656`).

4. **`test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

   Falha em `app/chamados/tests/test_f3_beco1_reata_limbo.py:160`: esperado **`com pai LIMBO vivo: 0 pergunta(s)`**, obtido **`1 pergunta(s) em 1 pai(s)`**, seguido de **`{"reatada": 1}`**.

   **Escritor/leitor:** o teste escreve `silenciado_ate` no passado; a fila volta a admitir o pai. O comando lê a assinatura C11, agrupa a pergunta, chama a reata e imprime seu resultado (`app/chamados/tests/test_f3_beco1_reata_limbo.py:158`; `app/chamados/services/painel.py:169`; `app/chamados/services/painel.py:188`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:68`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:118`).

   A reata efetiva passa por `reatar_perguntas_ao_limbo` e pela porta que atribui/salva `chamado_destino` (`app/chamados/services/limbo_folgas.py:239`; `app/chamados/services/limbo_folgas.py:245`; `app/chamados/services/fio.py:964`). Apesar do nome histórico, a asserção atual espera **nenhum candidato**, pela regra terminal COM modal; a c7 participa nessa seleção (`app/chamados/tests/test_f3_beco1_reata_limbo.py:29`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:160`; `app/chamados/services/tarefa_do_dia.py:656`).

5. **`test_MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

   Falha em `app/chamados/tests/test_f3_beco1_reata_limbo.py:84`: esperava ausência de **`SEM PAI:`**, obtida presença dessa linha com uma pergunta.

   **Escritor/leitor:** o teste monta o beco sem criar pai; C11 escreve a assinatura sem `pai_limbo`; o comando a coloca em `sem_pai` e imprime a mensagem (`app/chamados/tests/test_f3_beco1_reata_limbo.py:80`; `app/escala/services/cadastro_realidade.py:518`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:71`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:83`).

   **Regra literal:** “a ancora existente oferece o modal”; junto do wrapper terminal, afirma terminal → admin. A presença da mensagem não demonstra nascimento de pai; a asserção que conferiria a contagem de chamados vem depois da falha (`app/chamados/tests/test_f3_beco1_reata_limbo.py:29`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:85`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:87`). Participa o filtro terminal da c7 (`app/chamados/services/tarefa_do_dia.py:656`).

6. **`test_RED_apply_reata_ao_pai_e_o_dia_sai_do_beco` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

   Falha em `app/chamados/tests/test_f3_beco1_reata_limbo.py:55`: esperado **`[]`**, obtida uma assinatura C11 com pergunta `342`, âncora terminal `205` e pai `206`.

   **Escritor/leitor:** `_c11` chama o leitor de produção; este lê a ausência de porta e escreve a assinatura com `pai_limbo`, perguntas e detalhe (`app/chamados/tests/test_f3_beco1_reata_limbo.py:45`; `app/escala/services/cadastro_realidade.py:472`; `app/escala/services/cadastro_realidade.py:515`; `app/escala/services/cadastro_realidade.py:520`).

   **Regra literal:** “a ancora terminal abre o modal; nao seleciona reata”. Afirma terminal → admin. A falha ocorre **antes** da chamada `--apply`; esse traceback não demonstra resultado de reata (`app/chamados/tests/test_f3_beco1_reata_limbo.py:55`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:58`). Participa o filtro terminal da c7 (`app/chamados/services/tarefa_do_dia.py:656`).

7. **`test_RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

   Falha em `app/chamados/tests/test_f3_beco1_reata_limbo.py:146`: esperava ausência de **`pai saiu da fila antes da costura`**, obtida contagem `1` dessa recusa.

   **Escritor/leitor:** o mock escreve silêncio futuro depois de gravar a reversão; o comando consulta novamente a fila sob trava, encontra o pai ausente e escreve a recusa (`app/chamados/tests/test_f3_beco1_reata_limbo.py:139`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:141`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:115`).

   Há duas regras: a docstring exige **“A fila tem de ser perguntada de novo sob a trava do pai”**; a asserção que falhou exige **“LU-03: nenhum candidato”**, pela premissa terminal COM modal. O dado mostra a recusa prevista pela primeira; a divergência está na segunda, atingida pela c7 (`app/chamados/tests/test_f3_beco1_reata_limbo.py:127`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:146`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:29`; `app/chamados/services/tarefa_do_dia.py:656`).

8. **`test_RED_pai_silenciado_fora_da_fila_nao_recebe_reata` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

   Falha em `app/chamados/tests/test_f3_beco1_reata_limbo.py:120`: esperava ausência de **`PAI FORA DA FILA:`**, obtida uma linha com pai e uma pergunta.

   **Escritor/leitor:** o teste escreve silêncio futuro; o comando lê a fila, separa a assinatura em `fora_da_fila` e imprime a mensagem (`app/chamados/tests/test_f3_beco1_reata_limbo.py:114`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:69`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:89`).

   O aval próprio citado é **“reata SO a pai LIMBO vivo NA FILA”**, Ronald 05/11 13:0x; a falha atual exige **nenhuma seleção**, pela outra regra, terminal COM modal. A mensagem observada não prova reata ao pai silenciado (`app/chamados/tests/test_f3_beco1_reata_limbo.py:103`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:120`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:29`). Participa o filtro terminal da c7 (`app/chamados/services/tarefa_do_dia.py:656`).

9. **`test_ancora_terminal_com_modal_NAO_e_C11` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

   Falha em `app/escala/tests/test_f3_beco1_folga_na_lista.py:72`: esperado **`('admin', 'porta_acao', 'validar')`**, obtido **`('nada', None, None)`**.

   **Escritor/leitor:** a fixture escreve destino `resolvido`, resposta textual anterior à leitura, registra E1 e recarimba por último a ata conhecida. O teste chama diretamente `tarefa_do_dia`, antes da leitura da lista C11 (`app/escala/tests/test_f3_beco1_folga_na_lista.py:29`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:36`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:70`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:93`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:97`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:107`).

   **Regra literal:** “COM modal (LU-03), inclusive terminal”. Aqui a expectativa terminal → admin está isolada de qualquer pai ou reata; a c7 a altera diretamente (`app/escala/tests/test_f3_beco1_folga_na_lista.py:79`; `app/chamados/services/tarefa_do_dia.py:656`).

10. **`test_MORDE_ricardo_a_folha_le_o_juiz_e_diz_nada` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

    Falha em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:139`: esperado **`admin`**, obtido **`nada`**.

    **Escritor/leitor:** `_ricardo` escreve destino `fechado`, container `resolvido` e resposta textual com E1. `_fio_da_folha` chama a carga e o leitor reais; a Folha lê a porta publicada e devolve `FIO_SEM_PORTA='nada'` quando ela é `None` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:90`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:97`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:125`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:127`; `app/folha/fora_da_folha.py:72`; `app/folha/fora_da_folha.py:89`; `app/chamados/juizes.py:1652`).

    **Regra literal:** “LU-03: a Folha le admin, cuja tela e o modal da ancora existente”. Nessa fixture, “existente” é terminal; portanto afirma terminal → admin, atingido pela c7 (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:125`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:137`; `app/chamados/services/tarefa_do_dia.py:656`).

11. **`test_MORDE_ricardo_folha_e_vigia_dizem_a_MESMA_coisa` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

    Falha em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:143`: esperado **`('porta_acao', '')`**, obtido **`('sem_porta', 'c')`**.

    **Escritor/leitor:** usa a mesma fixture terminal de `_ricardo`; o Vigia lê a tarefa/porta e, sem rota, classifica a respondida sob terminal como causa `c`. O teste lê os índices de rota/causa de `alcance_dias` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:116`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:121`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:142`; `app/chamados/services/vigia_fio.py:587`; `app/chamados/services/vigia_fio.py:631`; `app/chamados/services/vigia_fio.py:655`).

    **Regra literal:** “LU-03: respondida ancorada tem modal”; pela fixture, terminal → admin. A asserção seguinte da Folha não foi alcançada no traceback fornecido (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:143`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:144`). Participa o filtro terminal da c7 (`app/chamados/services/tarefa_do_dia.py:656`).

12. **`test_c_respondida_com_destino_FECHADO` — EXPECTATIVA QUE CODIFICA UMA REGRA.**

    Falha em `app/chamados/tests/test_o261e_f5_alcance.py:217`: esperado **`('porta_acao', 'admin')`**, obtido **`('sem_porta', 'nenhuma porta')`**.

    **Escritor/leitor:** apesar do nome, o corpo escreve destino `resolvido`; o apoio monta resposta com E1 e nega a oferta de Resolver dia. O Vigia publica rota/papel, e `_rota_e_papel` lê os índices 4/7 (`app/chamados/tests/test_o261e_f5_alcance.py:80`; `app/chamados/tests/test_o261e_f5_alcance.py:103`; `app/chamados/tests/test_o261e_f5_alcance.py:113`; `app/chamados/tests/test_o261e_f5_alcance.py:138`; `app/chamados/tests/test_o261e_f5_alcance.py:214`; `app/chamados/services/vigia_fio.py:655`).

    **Regra:** a própria asserção exige porta/admin sob terminal e cita LU-03; participa o filtro terminal da c7 (`app/chamados/tests/test_o261e_f5_alcance.py:217`; `app/chamados/services/tarefa_do_dia.py:656`).

13. **`test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` — FIXTURE.**

    Falha em `app/folha/tests/test_haiku_fora_da_folha.py:95`: esperado grupo **`D`**, obtido **`E`**. As asserções anteriores do mesmo teste exigem e já ultrapassam `veredito='furo'`, `fio='nada'` e texto `SEM PORTA` para **01/09**; a falha é no grupo da pessoa (`app/folha/tests/test_haiku_fora_da_folha.py:91`; `app/folha/tests/test_haiku_fora_da_folha.py:92`; `app/folha/tests/test_haiku_fora_da_folha.py:94`).

    **Escritores:** a fixture cria vínculo desde janeiro e somente duas células explícitas na competência: furo em 01/09, agora com ata conhecida, e trabalhou em 02/09. A janela declarada é 21/08–20/09 (`app/folha/tests/test_haiku_fora_da_folha.py:56`; `app/folha/tests/test_haiku_fora_da_folha.py:58`; `app/folha/tests/test_haiku_fora_da_folha.py:61`; `app/folha/tests/test_haiku_fora_da_folha.py:65`).

    **Leitores e agregação:** o export percorre a janela inteira; dia passado sem célula, havendo vínculo, entra como `sem_celula`. A grade transporta essa ausência; a tarefa escreve `nao_sei`; a Folha lê os fios de **todos** os dias retentores e calcula o grupo (`app/folha/export.py:1264`; `app/folha/export.py:1270`; `app/escala/services/leitor_celula.py:394`; `app/chamados/services/tarefa_do_dia.py:228`; `app/folha/fora_da_folha.py:259`; `app/folha/fora_da_folha.py:283`).

    Pela função `_grupo`, **E** significa que não houve fio de ação e houve pelo menos um `nao_sei`; **D** exige todos `nada`. Isso não contradiz o `nada` já conferido para 01/09. A lista exata dos outros dias dessa execução não veio no dado **[nao verificado]** (`app/folha/fora_da_folha.py:99`; `app/folha/fora_da_folha.py:109`).

    **C7:** participa pelo hunk da fixture `folha/tests/test_haiku_fora_da_folha.py::_ata_conhecida_sem_porta` e `HaikuForaDaFolhaTest.setUpTestData`; a ata foi acrescentada ao furo, mas não completou a competência (`app/folha/tests/test_haiku_fora_da_folha.py:32`; `app/folha/tests/test_haiku_fora_da_folha.py:63`). O selo sem banco calcula `_grupo([fio])` de um único dia, portanto não prova esse agregado mensal (`app/chamados/tests/test_e3_r3_c7_sem_banco.py:99`; `app/chamados/tests/test_e3_r3_c7_sem_banco.py:106`).

    **Cura:** montar os demais dias cobertos da competência com estado conhecido e não retentor, mantendo a prova furo/sem porta de 01/09 e trabalhou de 02/09; não trocar o esperado para E nem retirar o desconhecimento do código (`app/folha/tests/test_haiku_fora_da_folha.py:58`; `app/folha/export.py:1270`; `app/folha/fora_da_folha.py:109`).

14. **`test_MORDE_pendente_curado_sai_da_lista` — CONHECIDO.**

    Falha em `app/core/tests/test_contract_juiz_tela.p

[truncado em 20000 de 28149 caracteres]

## 15/11 03:35 · sessao 7683d0944868 · - · - · origem=Ronald (tmux)

Há **6 FAILs** no arquivo de reata e **1 FAIL** no arquivo de escala; os sete são **EXPECTATIVA QUE CODIFICA UMA REGRA**, porque o estado terminal é premissa deliberada e os testes declaram que ele oferece modal, inclusive literalmente “âncora terminal COM modal” e “inclusive terminal”. Não são fixtures incompletas sob a prova que hoje afirmam. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:28`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:76`)

A regra literal que os sete carregam é **“validar sob âncora TERMINAL é porta do admin”**, derivada pelos testes de **LU-03**; LU-03 registrada diz “ação direta do admin com tela = porta”, sem dizer nessa linha que terminal tem tela. LU-04 registrada diz “pergunta em marco dispensado, fora de marco ou porta None NAO da dono”. Portanto o aval específico para a extensão “inclusive terminal” é **[nao verificado]**, e não cabe apagar essa expectativa como mera manutenção de fixture. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:29`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:79`, `app/docs/CORTES.md:435`, `app/docs/CORTES.md:436`)

Cadeia comum aberta integralmente:

- A fixture escreve `status_local='resolvido'` no chamado, `chamado_destino=furo`, resposta textual `17:00` com instante anterior, registra a linha E1 e, por último, escreve ata conhecida com entrada acesa e saída apagada. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:28`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:26`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:85`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:91`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:96`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:99`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:105`)
- `fio.chamado_que_lista` lê destino, senão container; `_pela_decisao` lê a última resposta E1 e produz ação `validar/admin`, com porta inteira somente se a âncora estiver na fila. (`app/chamados/services/fio.py:116`, `app/chamados/services/tarefa_do_dia.py:150`, `app/chamados/services/tarefa_do_dia.py:157`)
- **C7 participa diretamente**: `CargaDaTarefa.__init__` carrega o container adicional e escreve `_ancoras_terminais` pelos status do catálogo; `CargaDaTarefa.tarefa::ler` publica esse conjunto; `_portas_da_tarefa` exclui `validar` por âncora terminal do conjunto administrativo alcançável. Hunk **`chamados/services/tarefa_do_dia.py::_carga_da_tarefa_cls`** (`__init__`, `tarefa::ler`) e hunk **`::_portas_da_tarefa`**. (`app/chamados/services/tarefa_do_dia.py:407`, `app/chamados/services/tarefa_do_dia.py:443`, `app/chamados/services/tarefa_do_dia.py:449`, `app/chamados/services/tarefa_do_dia.py:589`, `app/chamados/services/tarefa_do_dia.py:652`)
- O publicador escreve `fio='nada'`, `rota=None`, `acao=None` quando não sobra dono; `porta_da_tarefa` devolve `None` para ausência conhecida de porta. (`app/chamados/services/tarefa_do_dia.py:667`, `app/chamados/services/tarefa_do_dia.py:674`, `app/chamados/services/tarefa_do_dia.py:692`, `app/chamados/services/tarefa_do_dia.py:696`, `app/chamados/services/tarefa_do_dia.py:708`)
- O sítio C11 lê essa porta e só prossegue se ela for `None`; lê status novamente, exige terminal ou M1 e `folga_nao_definida`, então escreve a assinatura contendo `pai_limbo` e IDs das perguntas sob terminal. (`app/escala/services/cadastro_realidade.py:466`, `app/escala/services/cadastro_realidade.py:472`, `app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:499`, `app/escala/services/cadastro_realidade.py:503`, `app/escala/services/cadastro_realidade.py:515`)
- O command lê a assinatura C11; particiona por pai na fila, pai fora da fila ou sem pai, e escreve as mensagens que os testes comparam. A fila lê `status_local ∈ COBRANCA` e exclui silêncio futuro; silêncio passado já voltou. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:58`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:67`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:74`, `app/chamados/services/painel.py:169`, `app/chamados/services/painel.py:188`)
- A escrita efetiva, quando autorizada pelo command, passa por `reatar_perguntas_ao_limbo` → `reapontar_resolvedora`, que atribui e salva `chamado_destino`; o command consulta a fila outra vez sob trava antes dessa escrita. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:118`, `app/chamados/services/limbo_folgas.py:239`, `app/chamados/services/limbo_folgas.py:245`, `app/chamados/services/fio.py:964`)
- A própria costura declara outra regra, O261-E F1, aval Ronald 30/10 17:3x: “pergunta VIVA da competência ABERTA não se costura em chamado TERMINAL”, explicando que “âncora terminal é fio morto — o admin não a vê”. Isso apoia o comportamento escolhido pela c7, mas não é prova de que os testes tenham recebido decisão para alterar sua regra específica. (`app/chamados/services/fio.py:785`)

| Teste | Falha; esperado × obtido do dado anexado | Ramo final da cadeia / regra particular | Cura | [nao verificado]
|---|---|---|---|
| `test_RED_apply_reata_ao_pai_e_o_dia_sai_do_beco` | Linha 55: `[]` × assinatura C11 com pai e pergunta; o dado mostra o detalhe “âncoras terminais: 205; reata ao LIMBO-FOLGAS #206 pendente”. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:55`; valores observados: anexo do usuário, localização física **[nao verificado]**) | `_c11` chama diretamente o leitor de produção; a asserção literal é “LU-03: a âncora terminal abre o modal; não seleciona reata”. C11 passa pela ausência de porta e escreve a assinatura. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:45`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:55`, `app/escala/services/cadastro_realidade.py:472`, `app/escala/services/cadastro_realidade.py:515`) | **Nada até decisão** da regra terminal/modal; se a regra dos testes prevalecer, código do publicador; se prevalecer O261-E F1/c7, expectativas e provas de reata precisam ser separadas. (`app/chamados/services/tarefa_do_dia.py:652`, `app/chamados/services/fio.py:786`) |
| `test_MORDE_dry_nao_escreve` | Linha 74: substring “0 pergunta(s) em 0 pai(s)” × “1 pergunta(s) em 1 pai(s)”. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:74`; observado: anexo, arquivo **[nao verificado]**) | A fixture escreve pai aberto; C11 entrega candidato; o command soma `por_pai` e imprime 1/1, antes do retorno DRY. A falha é na **seleção**, não numa escrita indevida demonstrada. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:33`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:71`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:68`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:73`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`) | Nada até decisão; não completar fixture retirando terminal, pois isso elimina a premissa declarada. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:29`) |
| `test_MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce` | Linha 84: ausência de `SEM PAI:` × presença de `SEM PAI: ... 1 pergunta(s)`. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:84`; observado: anexo, arquivo **[nao verificado]**) | Este caso não cria pai; o command lê C11, põe o dia em `sem_pai` e imprime o motivo. A regra imediata da asserção é “LU-03: sem candidato, não inventa pai”, e a asserção posterior explicita “a âncora existente oferece o modal”. A falha não demonstra pai criado. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:80`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:84`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:87`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:71`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:82`) | Nada até decisão. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:87`, `app/chamados/services/tarefa_do_dia.py:628`) |
| `test_RED_pai_silenciado_fora_da_fila_nao_recebe_reata` | Linha 120: ausência de `PAI FORA DA FILA:` × presença de uma linha com pai e 1 pergunta. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:120`; observado: anexo, arquivo **[nao verificado]**) | O teste escreve silêncio futuro no pai; o command exclui esse pai de `na_fila`, põe a assinatura em `fora_da_fila` e imprime. Além da regra terminal/modal, há aval próprio: “reata SÓ a pai LIMBO vivo NA FILA ... os 13 pais silenciados NÃO”. O comportamento observado da mensagem conserva esse segundo aval; a falha exige que nenhum C11 tivesse sido selecionado. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:103`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:114`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:120`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:69`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:88`) | Nada até decisão terminal/modal; não remover a guarda da fila para curar a expectativa. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:55`) |
| `test_RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata` | Linha 146: ausência de “pai saiu da fila antes da costura” × contador 1 dessa recusa. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:146`; observado: anexo, arquivo **[nao verificado]**) | O mock grava reversão e então escreve silêncio futuro no pai; o command rele a fila sob trava, encontra `None` e escreve essa recusa. Docstring literal: “A fila tem de ser perguntada de novo sob a trava do pai”; a falha atual é o comentário posterior “LU-03: nenhum candidato”, não evidência de reata ao pai silenciado. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:125`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:139`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:146`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`) | Nada até decisão terminal/modal; preservar prova da concorrência/segunda consulta da fila. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:126`) |
| `test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata` | Linha 160: “0 pergunta(s)” × “1 pergunta(s) em 1 pai(s)” e `{"reatada": 1}`. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:160`; observado: anexo, arquivo **[nao verificado]**) | O teste escreve silêncio passado; `qs_na_fila` inclui o pai; o command seleciona a pergunta C11, chama a porta e imprime `reatada`. A expectativa efetiva da linha 160 é **não haver candidato porque terminal teria modal**, apesar do nome histórico do teste falar em reatar. (`app/chamados/tests/test_f3_beco1_reata_limbo.py:156`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:158`, `app/chamados/tests/test_f3_beco1_reata_limbo.py:160`, `app/chamados/services/painel.py:169`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:118`) | Nada até decisão; se reconstituir prova de reata, ela precisa continuar usando candidato C11 legítimo. (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:7`) |
| `test_ancora_terminal_com_modal_NAO_e_C11` | Linha 72: `('admin','porta_acao','validar')` × `('nada',None,None)`. (`app/escala/tests/test_f3_beco1_folga_na_lista.py:72`; observado: anexo, arquivo **[nao verificado]**) | Chamada direta ao juiz, antes da lista C11; o `setUp` fixa oferta Resolver=False. A própria asserção exige modal em terminal, sem depender de pai ou reata. (`app/escala/tests/test_f3_beco1_folga_na_lista.py:58`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:67`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:74`) | Nada até decisão terminal/modal; sítio que mudaria é `_portas_da_tarefa`, não o command C11. (`app/chamados/services/tarefa_do_dia.py:652`) |

Os sete têm participação da c7 na cadeia; vermelho desses mesmos testes em c6 `a5ff3627f` é **[nao verificado]**. A cópia não expõe `.git`; a entrega c7 também declara a base como identidade via git não verificada, e não certifica suite completa. (`app/chamados/tests/e3_r3_c7_entrega.txt:2`, `app/chamados/tests/e3_r3_c7_entrega.txt:4`)

## 15/11 03:35 · sessao c0289a9b8b79 · - · - · origem=Ronald (tmux)

Os quatro casos classificam-se como **EXPECTATIVA QUE CODIFICA UMA REGRA**: esperam porta administrativa para resposta sob âncora terminal; a c7 participa por `_carga_da_tarefa_cls` (carrega/escreve/transmite terminais) e `_portas_da_tarefa` (exclui essa âncora de `admin_alcancavel`). A regra declarada hoje pelo sítio exige âncora não terminal. (`app/chamados/services/tarefa_do_dia.py:407`, `:443`, `:449`, `:589`, `:628`, `:652`.)

O aval LU-03 citado pelos testes diz literalmente **“acao direta do admin com tela = porta; baldes do vigia leem o juiz”**; não explicita que terminal tem tela. LU-04 diz **“pergunta em marco dispensado, fora de marco ou porta None NAO da dono”**. A afirmação explícita de terminal ter modal está nos testes, enquanto o contrato de produção O261-E F1 diz **“ancora terminal e fio morto -- o admin nao a ve, o colab a ve”**, citando aval Ronald de 30/10 17:3x. (`app/docs/CORTES.md:435`, `:436`; `app/chamados/tests/test_e4_passivo.py:5`; `app/chamados/services/fio.py:786`, `:789`.)

**Cadeia comum aberta**

- A identidade da âncora é escrita nas fixtures como `chamado_destino`/`disputa.chamado`; `chamado_que_lista` lê primeiro destino e depois container. `resolvido` e `fechado` pertencem a `TERMINAIS`. (`app/chamados/tests/test_e4_passivo.py:35`, `:45`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:75`, `:82`, `:86`; `app/chamados/tests/test_o261e_f5_alcance.py:94`, `:105`; `app/chamados/services/fio.py:117`; `app/chamados/catalogo/motor.py:30`.)
- A carga lê as perguntas e a sequência E1, escreve `_ancoras_terminais` pelo `status_local`, escreve `_fila` pelo juiz `qs_na_fila` e transporta terminais para `_portas`. (`app/chamados/services/tarefa_do_dia.py:404`, `:419`, `:449`, `:450`, `:589`.)
- `_pela_decisao` lê a última RESPOSTA textual e produz `validar/admin`; a porta da ação fica `None` quando a âncora não está na fila. `_tarefa_pura` copia ação/razão para a instância. (`app/chamados/services/tarefa_do_dia.py:150`, `:151`, `:157`, `:160`, `:330`, `:339`.)
- A c7 exclui a âncora terminal de `admin_alcancavel`; a projeção escreve `fio='nada'`, `rota=None` quando não há outro dono; `porta_da_tarefa` devolve `None` para ausência conhecida de porta. (`app/chamados/services/tarefa_do_dia.py:652`, `:656`, `:667`, `:674`, `:696`, `:708`.)
- A Folha lê a tarefa por `_porta`, e `_fio_da_porta(None)` devolve `FIO_SEM_PORTA`, definido como `'nada'`. O Vigia lê a mesma projeção; sem rota, pergunta respondida sob terminal recebe causa `c`, rota `sem_porta`, papel `nenhuma porta`. (`app/folha/fora_da_folha.py:65`, `:72`, `:81`, `:89`; `app/chamados/juizes.py:1652`; `app/chamados/services/vigia_fio.py:587`, `:589`, `:631`, `:655`, `:161`.)

| Teste | Falha: esperado × obtido | Escritor/leituras particulares e regra | [nao verificado]
|---|---|---|
| `test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | linha 65 exige `-> resolvido`; dado raspado obtém `-> segue_sem_porta`. (`app/chamados/tests/test_e4_passivo.py:65`; obtido: anexo `fails.txt` fornecido, [nao verificado] por execução própria.) | A fixture escreve destino `resolvido`, resposta `07:00` e carimbo, sem sequência inicial. O comando lê `passivo_sem_linha`, escolhe `registrar_decisao`; chama a porta E1 que escreve a RESPOSTA; mede o alcance depois; escreve `SEGUE` se a célula continua no conjunto sem porta. A linha impressa lê esse resultado. (`app/chamados/tests/test_e4_passivo.py:59`, `:60`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`, `:141`, `:352`, `:368`, `:371`, `:378`; `app/chamados/models.py:4074`, `:4077`, `:4096`.) Regra literal: **“VALIDAR do admin tem modal na ancora existente, mesmo terminal (LU-03) -> o dia tem porta_acao”**. É precisamente a expectativa terminal/admin. (`app/chamados/tests/test_e4_passivo.py:8`.) |
| `test_MORDE_ricardo_a_folha_le_o_juiz_e_diz_nada` | linha 139 exige `admin`; dado obtém `nada`. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:139`; obtido: anexo [nao verificado] por execução própria.) | `_ricardo` escreve destino `fechado`, container `resolvido`, resposta `07:00`; `_pergunta` registra a resposta pela E1, portanto o teste monta sequência. `_fio_da_folha` lê `_carga`/`_fio_do_dia` reais. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:125`, `:126`, `:127`, `:90`, `:97`.) Regra literal da docstring local: **“LU-03: a Folha le admin, cuja tela e o modal da ancora existente.”** A âncora dessa fixture é terminal; espera, portanto, terminal/admin. A docstring geral ainda descreve o contrário, **“porta None, fio 'nada', vigia sem_porta causa (c)”**. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:137`, `:125`, `:11`.) |
| `test_MORDE_ricardo_folha_e_vigia_dizem_a_MESMA_coisa` | linha 143 exige `('porta_acao','')`; dado obtém `('sem_porta','c')`. A asserção seguinte da Folha, linha 144, não foi alcançada pelo traceback dado. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:143`, `:144`; obtido: anexo [nao verificado] por execução própria.) | Mesma escrita `_ricardo`; `_vigia` chama `censo_alcance`, seleciona a célula e lê índices 4/5 (`rota`, `causa`) de `alcance_dias`. Regra literal da asserção: **“LU-03: respondida ancorada tem modal”**; pela fixture, ancorada em terminal. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:142`, `:123`, `:116`, `:119`, `:121`, `:143`.) |
| `test_c_respondida_com_destino_FECHADO` | linha 217 exige `('porta_acao','admin')`; dado obtém `('sem_porta','nenhuma porta')`. (`app/chamados/tests/test_o261e_f5_alcance.py:217`; obtido: anexo [nao verificado] por execução própria.) | Apesar do nome “FECHADO”, o corpo escreve `status_local='resolvido'`; `_pergunta` escreve resposta/carimbo e registra a E1. O Resolver dia está explicitamente negado na montagem. `_alc` chama o Vigia; `_rota_e_papel` lê índices 4/7. (`app/chamados/tests/test_o261e_f5_alcance.py:214`, `:215`, `:103`, `:106`, `:113`, `:80`, `:116`, `:138`, `:141`.) A própria asserção codifica porta/admin para a resposta sob terminal e cita `LU-03`; a docstring geral só declara medir “dia sem porta viva enderecavel”, não declara modal terminal. (`app/chamados/tests/test_o261e_f5_alcance.py:217`, `:6`.) |

Para os quatro, uma cura que preserve a afirmação atual do teste exige **decisão sobre a regra/tela de terminal**; alterar a âncora para viva apagaria precisamente a prova que eles montam. Se o contrato terminal atual prevalecer, mudam as expectativas; se a decisão instituir modal terminal, muda o código de alcance e a tela correspondente. A autorização literal para essa segunda alternativa não foi encontrada nos avais abertos. (`app/chamados/tests/test_e4_passivo.py:8`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:125`, `:137`; `app/chamados/tests/test_o261e_f5_alcance.py:214`, `:217`; `app/chamados/services/tarefa_do_dia.py:628`; `app/chamados/services/fio.py:789`; `app/docs/CORTES.md:435`.)

## 15/11 03:35 · sessao e954b47bcdea · - · - · origem=Ronald (tmux)

Os três casos são **CONHECIDO** no material da cópia: a entrega da c6 já os enumera como pendências da sessão, e a entrega da c7 os conserva explicitamente como “três falhas conhecidas fora da c7”. Isso é evidência documental anterior, mas **não prova execução na árvore exata c6 `a5ff3627f`**: a própria entrega da c7 declara a identidade Git da base `[nao verificado]`. [`app/chamados/tests/e3_r3_c6_entrega.txt:55`–65; `app/chamados/tests/e3_r3_c7_entrega.txt:2`,175–176]

1. **`test_MORDE_pendente_curado_sai_da_lista`**

   - Falha em `app/core/tests/test_contract_juiz_tela.py:38`: esperado `sumiram == []`; o dado fornecido mostra obtido uma lista com a impressão antiga de `cal = contexto_calendario(...)`. O teste constrói essa lista lendo cada `PENDENTES['tela']` e procurando sua impressão como substring literal do arquivo correspondente. [`app/core/tests/test_contract_juiz_tela.py:34`–38]
   - Escritor do valor divergente: `PENDENTES['tela']` registra a impressão antiga com o fechamento `..._fim.isoformat()})`; `_p` transforma os argumentos em dicionário com `arquivo`/`impressao`. [`app/core/juizes.py:367`–368,878–879]
   - Leitor: o teste lê o arquivo e usa `p['impressao'] not in texto`, sem interpretação semântica. [`app/core/tests/test_contract_juiz_tela.py:35`–37]
   - Sítio de produção: `contexto_drawer` agora chama `contexto_calendario` em duas linhas, passando `agora=agora`; portanto a antiga substring inteira deixou de existir por mudança da chamada, o que por si só **não prova cura semântica do pendente**. [`app/colaboradores/services/drawer.py:121`–122]
   - Continuação até a tela: `contexto_drawer` devolve `'cal': cal`; a view usa esse contexto ao renderizar o drawer; o template mostra `cal.data_ini`, `cal.data_fim`, `cal.dias` e `cal.resumo`. [`app/colaboradores/services/drawer.py:154`; `app/colaboradores/views.py:2945`–2947; `app/templates/colaboradores/partials/_drawer_diagnostico.html:33`–35]
   - Delta c7: nenhum hunk fornecido altera o registro, o drawer ou este teste; a entrega da c7 afirma que `core/juizes.py` e templates não foram alterados. [`app/chamados/tests/e3_r3_c7_entrega.txt:172`–175]
   - Antiguidade: já mencionado no relatório de vermelhos do recorte c5, dentro da entrega da c6; execução na base exata `a5ff3627f` `[nao verificado]`. [`app/chamados/tests/e3_r3_c6_entrega.txt:55`,64; `app/chamados/tests/e3_r3_c7_entrega.txt:2`]
   - Cura: **código do registro**, após verificar se a pendência foi efetivamente curada: reancorar a impressão se o problema continua, ou aplicar baixa se houver prova de cura. O contrato exige impressão existente e o registro explica o vínculo entre impressão e derivação; o mecanismo de baixa prevê explicitamente distinguir “outra fatia tirou” de “a impressão mudou”. [`app/core/juizes.py:10`–14; `app/core/registro_baixa.py:84`–103]

2. **`test_MORDE_o_mmd_commitado_bate_com_o_codigo`**

   - Falha em `app/core/tests/test_selo_diagrama_do_codigo.py:46`: esperado `rc == 0`; obtido `rc == 1`, com `DIVERGE: docs/MAPA.md nao bate com o codigo`, conforme dado fornecido. O nome/mensagem do teste menciona MMD, mas a função chamada confere **MMD e MAPA**. [`app/core/tests/test_selo_diagrama_do_codigo.py:44`–47; `app/core/management/commands/gerar_diagrama.py:921`–930]
   - Cadeia: `_gerar` chama `gerar(check=True)` e captura stdout; `gerar` monta `txt` e `mapa`, lê ambos os arquivos persistidos, escreve `rc = 1` quando um conteúdo diverge e devolve esse inteiro; o teste lê esse retorno. [`app/core/tests/test_selo_diagrama_do_codigo.py:29`–39; `app/core/management/commands/gerar_diagrama.py:911`–930]
   - Quem escreve os arquivos em produção/ferramenta: `gerar(check=False)` abre `ARQUITETURA.mmd` e `MAPA.md` com `'w'` e grava as projeções; `gerar_mapa` devolve o texto do MAPA e inclui `mapa_cobertura()`. [`app/core/management/commands/gerar_diagrama.py:906`–908,931–935]
   - Fontes lidas pelo MAPA: registro de juízes, classificação dos pendentes por fonte AST, selos que referenciam o registro e portas da tela; não há banco nessa declaração da camada de cobertura. [`app/core/management/commands/gerar_diagrama.py:712`–719,750–784,799–848]
   - Relação específica possível com o primeiro conhecido: se a impressão não é achada, `classificar_pendente` publica `nao_sei / impressao nao achada`, e esses contadores entram no MAPA. **Que essa seja a única ou efetiva diferença do MAPA anexado `[nao verificado]`**, pois o dado só fornece a mensagem de divergência. [`app/core/management/commands/gerar_diagrama.py:603`–605,764–780,829–838]
   - Delta c7: não altera gerador ou docs; suas mudanças de imports/leitores não entram automaticamente nos números publicados de leitores, que ficam fora do MAPA commitado. Uma contribuição indireta específica da c7 à divergência do MAPA `[nao verificado]`; documentação da c7 classifica esse vermelho como externo ao delta. [`app/core/management/commands/gerar_diagrama.py:715`–716,821–822; `app/chamados/tests/e3_r3_c7_entrega.txt:172`–176]
   - Antiguidade: “selo_diagrama x2” já consta na entrega da c6, na tabela dos vermelhos do recorte c5; execução na base exata `a5ff3627f` `[nao verificado]`. [`app/chamados/tests/e3_r3_c6_entrega.txt:55`,65; `app/chamados/tests/e3_r3_c7_entrega.txt:2`]
   - Cura: **artefato gerado/código documental**, regenerar e revisar os arquivos a partir da árvore final; não mudar expectativa para aceitar `rc=1`. Esse é precisamente o contrato de regeneração e comparação. [`app/core/tests/test_selo_diagrama_do_codigo.py:13`–18; `app/core/management/commands/gerar_diagrama.py:931`–935]

3. **`test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo`**

   - Falha em `app/core/tests/test_selo_diagrama_do_codigo.py:98`: esperado `rc == 0` **após restaurar o MMD original**; obtido `rc == 1` com a mesma mensagem de divergência do MAPA, conforme dado fornecido. [`app/core/tests/test_selo_diagrama_do_codigo.py:94`–98]
   - Cadeia própria do teste: lê e guarda o MMD original; grava uma aresta extra; verifica `rc == 1`; no `finally` regrava exatamente `original`; chama novamente `_gerar` e espera zero. O erro anexado está nessa última verificação, não na verificação de rejeição da aresta. [`app/core/tests/test_selo_diagrama_do_codigo.py:88`–98]
   - Cadeia restante igual ao caso anterior: `_gerar → gerar(check=True) → comparação dos dois arquivos → rc`; o `MAPA.md` não é modificado/restaurado por este teste. Assim a saída fornecida não prova falha da restauração do MMD nem aceitação da aresta. [`app/core/tests/test_selo_diagrama_do_codigo.py:29`–39,88–98; `app/core/management/commands/gerar_diagrama.py:921`–930]
   - Delta, antiguidade e cura: mesmos limites do caso anterior; documentação anterior já enumera dois vermelhos do selo, mas a execução na c6 exata continua `[nao verificado]`. [`app/chamados/tests/e3_r3_c6_entrega.txt:55`,65; `app/chamados/tests/e3_r3_c7_entrega.txt:2`,175–176]

| teste | classe | sítio | cura | [nao verificado]
|---|---|---|---|
| `test_MORDE_pendente_curado_sai_da_lista` | CONHECIDO | impressão em `app/core/juizes.py:878` versus chamada em `app/colaboradores/services/drawer.py:121` | Código do registro: reancorar ou baixar conforme prova semântica; baixa automática por mera ausência textual não está provada. [`app/core/registro_baixa.py:91`–103] |
| `test_MORDE_o_mmd_commitado_bate_com_o_codigo` | CONHECIDO | comparação de `MAPA.md` em `app/core/management/commands/gerar_diagrama.py:923`–927 | Regenerar/revisar artefatos após acertar a árvore final. [`app/core/management/commands/gerar_diagrama.py:931`–935] |
| `test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` | CONHECIDO | última chamada do selo em `app/core/tests/test_selo_diagrama_do_codigo.py:97`–98, mesma comparação do MAPA | Mesma cura do MAPA; nenhuma alteração da prova da aresta está sustentada pelo dado. [`app/core/management/commands/gerar_diagrama.py:923`–930] |

## 15/11 02:46 · sessao 2bd3857e8104 · E3-RESTO-R3 · verificador · origem=Code

1. **ESMERIL-SEMÂNTICA: nenhum desvio adicional identificado nos leitores da saída; há um selo antigo incompatível, descrito no item 6.** A mudança pretendida é: validar sob âncora terminal, sem outra porta/oferta, deixa de publicar `admin/porta_acao/validar`. Os leitores continuam distinguindo ausência conhecida de porta e desconhecimento. (`app/chamados/services/tarefa_do_dia.py:652`, `app/chamados/services/tarefa_do_dia.py:667`, `app/chamados/services/tarefa_do_dia.py:708`)

   | Valor | Quem escreve → quem lê |
   |---|---|
   | `status_local` | Nascimento em `app/chamados/models.py:1142`; transição canônica em `app/chamados/models.py:228`, persistida em `:296`. Há simulação temporária de pós-estado em `:356`, restaurada em `:371`. A cura somente lê em `app/chamados/services/tarefa_do_dia.py:449`; a costura lê em `app/chamados/services/fio.py:809`; fila/contadores leem em `app/chamados/services/painel.py:155`, `:189`, `:213`. |
   | `TERMINAIS` | Definido em `app/chamados/catalogo/motor.py:31`; consumido pela carga em `app/chamados/services/tarefa_do_dia.py:449`, costura em `app/chamados/services/fio.py:810` e C11 em `app/escala/services/cadastro_realidade.py:499`. |
   | FKs da âncora | Destino nasce em `app/chamados/models.py:3097` e é reapontado em `app/chamados/services/fio.py:964`; container é atribuído em `app/chamados/services/disputa_emissao.py:591`. A identidade é lida exclusivamente por `app/chamados/services/fio.py:116`. |
   | `ancoras` | Conjunto de IDs em `app/chamados/services/tarefa_do_dia.py:444`; mapas pergunta→ID em `:591` e `:617`; leitores em `:639`, `:646`, `:655`. |
   | `_ancoras_terminais` | Escrito em memória em `app/chamados/services/tarefa_do_dia.py:449`; transportado para `alimentacao['ancoras_terminais']` em `:589`; lido em `:656`. |
   | `admin_alcancavel` / `com_dono` | Locais escritos em `app/chamados/services/tarefa_do_dia.py:652`, `:658`; consumidos para donos, residuais, portas e ação em `:660`, `:663`, `:670`, `:692`. |
   | `fio/rota/acao` | Publicados em `app/chamados/services/tarefa_do_dia.py:696`; projetados em `:708`; Folha lê em `app/folha/fora_da_folha.py:84`, agrega ações/grupos/contagens em `:203`. |
   | Retrato e persistência | Copia fio como `dono` e ações em `app/folha/retrato_folha.py:1228`; persiste em `app/folha/management/commands/lavrar_retrato_folha.py:78`; lê fio/balde em `app/folha/retrato_folha.py:448`, `:485`; monta links em `:1036`. |
   | Views/templates | Consomem foto, frases e links em `app/colaboradores/views.py:982`, `app/folha/views_retrato.py:318`, `app/templates/folha/_retrato_o_que_fazer.html:33`, `:72` e `app/templates/folha/_retrato_lista.html:47`. |
   | Contadores | Folha agrega em `app/folha/retrato_folha.py:695`, `:835`; vigia dos retidos conta sem ação/sem leitura em `app/folha/vigia_retido_calado.py:53`, `:57`; alcance lê rota/fio em `app/chamados/services/vigia_fio.py:596`, `:601`; furo sem canal lê ausência de rota em `app/chamados/services/furo_sem_canal.py:98`, `:104`. |
   | Pauta / Cadastro | Pauta aceita apenas `nada`, separando `nao_sei`, em `app/ponto/services/pautas_dos_discordantes.py:90`, `:95`. Cadastro exige projeção `None`, depois relê status e aplica `TERMINAIS`, em `app/escala/services/cadastro_realidade.py:472`, `:477`, `:499`. |

   O significado permanece coerente: `nada` corresponde a ausência conhecida de porta; no retrato vira `ninguem` e depois `sem_acao_disponivel`. `nao_sei` continua chegando a `sem_leitura_sistema`. Fotos já persistidas precisam de nova lavra para refletir a mudança. (`app/chamados/services/tarefa_do_dia.py:705`, `app/folha/retrato_folha.py:185`, `:193`, `:1228`)

2. **Chamadores: nenhum caminho produtivo encontrado que alcance `_portas_da_tarefa` sem o transporte dos terminais.** (`app/chamados/services/tarefa_do_dia.py:589`, `:591`, `:616`)

   | Chamador produtivo | O que passa |
   |---|---|
   | `CargaDaTarefa.tarefa → alimentar_fila` | IDs das células da janela, `self.agora`, `colab_ids`; depois acrescenta `ancoras_terminais`. (`app/chamados/services/tarefa_do_dia.py:586`) |
   | `alimentar_portas → alimentar_fila` | `_ids`, `agora`; não passa `colab_ids` nem acrescenta terminais. Esse resultado segue para C17, sem chamar `_portas_da_tarefa`. (`app/chamados/juizes.py:1616`, `:1627`) |
   | Primeira publicação → `_portas_da_tarefa` | `out`, `self._portas`, mapa de âncoras e pendentes do app. (`app/chamados/services/tarefa_do_dia.py:591`) |
   | Segunda publicação → `_portas_da_tarefa` | A mesma alimentação enriquecida, âncoras, pendentes e oferta de Resolver. (`app/chamados/services/tarefa_do_dia.py:616`) |

   Nos testes diretos, omitir a chave equivale a conjunto terminal vazio: uma âncora existente será aceita. M2 omite; M2b fornece explicitamente `{202}`. Isso limita a prova desses testes, mas não demonstra outro chamador produtivo defeituoso. (`app/chamados/services/tarefa_do_dia.py:656`, `app/chamados/tests/e3_portas_apoio.py:34`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:541`, `:547`)

3. **Status: mesma fonte lógica, sem garantia de mesma fotografia; limitação admissível para leitura.** A carga usa `status_local` e `motor.TERMINAIS`, como a costura. Entretanto, captura os objetos no `select_related` e congela o conjunto; a costura relê o banco e, dentro de transação, adquire a trava. (`app/chamados/services/tarefa_do_dia.py:407`, `:449`; `app/chamados/services/fio.py:794`, `:801`, `:807`, `:809`)

   Janela concreta possível: a consulta inicial vê a âncora viva; ocorre fechamento; a consulta posterior da fila já a exclui. `_pela_decisao` produz `porta=None`, mas o conjunto antigo não contém o terminal e permite `admin/porta_acao`. O inverso pode negar porta após reabertura posterior às leituras. Há também desvio pelo ramo `isinstance(porta, int)`, baseado na fotografia anterior da fila. (`app/chamados/services/tarefa_do_dia.py:450`, `:151`, `:160`, `:654`, `:656`)

   Não classifico isso como corrupção de estado: a carga apenas lê, declara leitura de estado atual sem reconstrução histórica e memoriza a resposta. Não oferece a garantia transacional da costura. Ocorrência e duração dessa janela em produção: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:383`, `:449`, `:598`)

4. **KeyError: nenhum sob integridade válida das FKs.** `chamado_que_lista` retorna somente `chamado_destino_id` ou `disputa.chamado_id`; ambos os objetos entram em `chamados`, e `None` é retirado de `ancoras`. Portanto, todo PK selecionado tem correspondente no dicionário. (`app/chamados/services/fio.py:116`, `app/chamados/services/tarefa_do_dia.py:444`, `:448`, `:449`)

   `PerguntaDisputa.disputa` não admite nulo; os dois chamados admitem nulo com `SET_NULL`, sendo ignorados pelo dicionário quando ausentes. Integridade efetiva do banco: **[nao verificado]**. (`app/chamados/models.py:2494`, `:1796`, `:3494`)

5. **Achado — diagnóstico obrigatório de F2 permanece incompleto. Gravidade: média, lacuna de verificação.** A entrega reconhece que não identificou os dois fios finais originais e deixa esse requisito pendente. Os novos testes controlam Resolver como `False`; isso prova a fixture corrigida sob esse controle, sem determinar a oferta/ata originais. (`app/chamados/tests/e3_r3_c7_entrega.txt:56`, `:58`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:71`, `:246`)

   **Nenhum esperado positivo enfraquecido identificado:** permanecem `nada`/D, golden `sem_acao_disponivel=1`, transição para chamado/admin, `[D1]` e `[(D1,'BATIDA')]` com motor não chamado. (`app/folha/tests/test_haiku_fora_da_folha.py:92`, `:537`, `:595`, `:617`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:79`, `:258`, `:260`)

   Os irmãos têm asserções efetivas de desconhecimento: Folha exige `nao_sei`/E e `sem_leitura_sistema`; Pauta exige `nao_sei=True`, rota `None`, exclusão e contagem em `sem_leitura`. A execução ORM desses irmãos permanece **[nao verificado]**. (`app/folha/tests/test_haiku_fora_da_folha.py:110`, `:549`, `:555`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:91`, `:93`, `:97`)

   A ata é montada manualmente, sem chamar o produtor/lavra. Passa pelo leitor, instâncias, tarefa e porta reais no teste sem banco; os campos das lâmpadas existem no produtor real. Não encontrei estrutura comprovadamente impossível, mas a combinação completa de veredito acusador e lâmpadas acesas não foi produzida pelo escritor nesses testes **[nao verificado]**. (`app/folha/tests/test_haiku_fora_da_folha.py:32`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:27`; `app/chamados/tests/test_e3_r3_c7_sem_banco.py:91`; `app/ponto/services/cartorio.py:711`, `:738`)

6. **Achado — selo antigo conserva precisamente o comportamento proibido pela c7. Gravidade: média, regressão no contrato dos testes.** `test_ancora_terminal_com_modal_NAO_e_C11` desativa Resolver, cria âncora `resolvido` e exige `('admin','porta_acao','validar')`. O filtro novo elimina esse alcance; M2b exige ausência de porta para o mesmo princípio. (`app/escala/tests/test_f3_beco1_folga_na_lista.py:62`, `:70`, `:72`; `app/chamados/services/tarefa_do_dia.py:655`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:548`)

   O caso antigo possui resposta E1 e ata conhecida, portanto não se resolve classificando-o como passivo ou ausência de lâmpadas. A execução ORM original continua **[nao verificado]**; a incompatibilidade entre as expectativas está no código. O contrato antigo também aparece nos testes de reata e passivo. (`app/escala/tests/test_f3_beco1_folga_na_lista.py:36`, `:99`, `:105`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:29`, `:55`; `app/chamados/tests/test_e4_passivo.py:65`, `:69`)

   **Demais classes: nenhum problema novo identificado no delta.** Não nasce juiz, escritor persistente, relógio interno, trava ou lista terminal paralela: a cura importa a constante existente e acrescenta dados em memória. (`app/chamados/services/tarefa_do_dia.py:447`, `:449`, `:589`, `:652`; `app/chamados/catalogo/motor.py:31`)

   Os nove mutantes têm controle verde e morrem por asserção; o runner recusa erro de infraestrutura como morte. O mutante obrigatório remove exatamente o filtro terminal. O mutante do join verifica presença do argumento, sem medir SQL/N+1 real. (`app/chamados/tests/e3_r3_c7_mutantes.py:17`, `:46`, `:60`; `app/chamados/tests/e3_r3_c7_mutantes.txt:206`; `app/chamados/tests/test_e3_r3_c7_sem_banco.py:215`)

7. **Docstrings/comentários do diff: nenhum erro adicional demonstrado.** “Mesmo status_local/TERMINAIS” descreve corretamente campo e constante, sem prometer mesma trava ou instante. Os testes sem banco declaram seus limites de ORM, signals e calendário. A certificação dos casos ORM e C11 completos permanece ausente. (`app/chamados/services/tarefa_do_dia.py:445`; `app/chamados/tests/test_e3_r3_c7_sem_banco.py:1`, `:83`; `app/chamados/tests/e3_r3_c7_entrega.txt:167`)

**VEREDITO: REPROVA.**

## 15/11 02:44 · sessao 2c5a4a7cf2df · - · - · origem=Ronald (tmux)

Achado de gravidade média: selo antigo contradiz a cura c7. `app/escala/tests/test_f3_beco1_folga_na_lista.py:67-74` cria âncora `resolvido` e exige `('admin', 'porta_acao', 'validar')`, enquanto o filtro novo elimina esse dono (`app/chamados/services/tarefa_do_dia.py:655-657`) e M2b exige ausência de porta (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:544-550`).

Executei contraponto sem banco, sem gravar arquivos: corpo real de `_lavra` (`app/escala/tests/test_f3_beco1_folga_na_lista.py:105-111`), leitor real da ata, instâncias/tarefa/publicador reais, teto 08h/17h vencido, pergunta `orfao_14h` com resposta E1, fila vazia, oferta False. c7 produziu `('nada', None, None)` e o corpo antigo falhou na linha 72; removendo apenas o filtro c7 em memória, produziu `('admin', 'porta_acao', 'validar')` e passou. Execução ORM original [nao verificado]. A fixture controla oferta False (`:58-65`) e cria resposta E1 (`:26-36`, `:95-102`).

O mesmo contrato antigo aparece em `app/chamados/tests/test_f3_beco1_reata_limbo.py:28-30`, `:55-67`, `:87`, e em `app/chamados/tests/test_e4_passivo.py:65-69`, `:122-127`; consequências completas desses testes ORM [nao verificado].

Item 3: a fonte lógica é a mesma — campo `status_local` e constante `TERMINAIS` (`app/chamados/services/tarefa_do_dia.py:447-449`; `app/chamados/services/fio.py:794`, `:809-810`). O estado observado pode diferir: carga faz SELECT e congela o conjunto (`tarefa_do_dia.py:404-408`, `:449`), enquanto costura relê banco sob trava (`fio.py:801-808`). Interleaving concreto possível: SELECT vê vivo; fechamento ocorre; fila posterior já exclui o PK (`tarefa_do_dia.py:450-451`); `_pela_decisao` fornece `porta=None` (`:150-160`), mas conjunto antigo permite admin (`:655-657`). Considero limite admissível de leitura sem atomicidade, pois contrato declara estado atual, sem reconstrução histórica (`:383-388`), e a carga não escreve esse estado (`:448-449`). Ocorrência em produção [nao verificado].

Item 4: nenhum KeyError demonstrado sob FKs válidas. `chamado_que_lista` só retorna destino ou chamado da disputa (`app/chamados/services/fio.py:116-117`); ambos entram no dicionário e `None` sai das âncoras (`app/chamados/services/tarefa_do_dia.py:444`, `:448-449`). Pergunta exige disputa (`app/chamados/models.py:2494-2498`); os dois chamados são nullable com SET_NULL (`:1796-1802`, `:3494-3500`). Integridade real do banco [nao verificado].

Item 6: os nove mutantes morreram na execução atual, cada qual com controle verde e falha de asserção; o runner rejeita erros como morte (`app/chamados/tests/e3_r3_c7_mutantes.py:46-63`). Nenhuma lista terminal literal nova em produção: a cura importa a constante única (`app/chamados/services/tarefa_do_dia.py:447-449`; definição `app/chamados/catalogo/motor.py:30-31`). Nenhuma trava ou leitura de relógio acrescentada nesses blocos (`tarefa_do_dia.py:404-449`, `:586-589`, `:652-657`).

## 15/11 02:44 · sessao 3d95f3e4c5cb · - · - · origem=Ronald (tmux)

Não achei falha estática nova nos chamadores ou mudança de significado incompatível nos leitores do mapa publicado; os dois caminhos produtivos passam a mesma alimentação enriquecida com terminais (`app/chamados/services/tarefa_do_dia.py:589`, `app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:616`).

- `alimentar_fila`: chamado pela carga com células da janela, `agora` e `colab_ids`; chamado por `alimentar_portas` com células e `agora`, sem `colab_ids` ou terminais. Esse segundo caminho segue para C17, sem passar por `_portas_da_tarefa` (`app/chamados/services/tarefa_do_dia.py:586`, `app/chamados/juizes.py:1616`, `app/chamados/juizes.py:1627`).
- `_portas_da_tarefa`: os únicos chamadores produtivos são a primeira publicação e a segunda passagem após consultar Resolver; ambos recebem `self._portas`, âncoras por pergunta e pendentes do app, e a segunda acrescenta a oferta (`app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:616`).
- Chamadas de teste podem omitir a chave; `.get(..., ())` então considera qualquer âncora não terminal. M2 usa essa entrada sintética; M2b fornece explicitamente a âncora terminal (`app/chamados/services/tarefa_do_dia.py:656`, `app/chamados/tests/e3_portas_apoio.py:34`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:541`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:547`).
- O ramo de porta inteira antecede o filtro de terminal. A porta inteira nasce da pertinência à foto da fila; portanto, avaliar esse desvio exige avaliar as fotos tomadas em consultas distintas, e não constitui sozinho contraexemplo estático (`app/chamados/services/tarefa_do_dia.py:151`, `app/chamados/services/tarefa_do_dia.py:654`).

Censo da saída:

| Valor/caminho | Escritor → leitores |
|---|---|
| `admin_alcancavel`, `com_dono` | Locais em `app/chamados/services/tarefa_do_dia.py:652` e `app/chamados/services/tarefa_do_dia.py:658`; alimentam donos, residuais e portas vivas em `app/chamados/services/tarefa_do_dia.py:660`, `app/chamados/services/tarefa_do_dia.py:663`, `app/chamados/services/tarefa_do_dia.py:670`. |
| `fio/rota/acao` | Publicados em `app/chamados/services/tarefa_do_dia.py:696`; projetados sem rejulgar em `app/chamados/services/tarefa_do_dia.py:708`. |
| Folha | Lê envelope e fio em `app/folha/fora_da_folha.py:84`; agrega grupos, ações e contadores em `app/folha/fora_da_folha.py:203`; ferramenta monta links em `app/folha/fora_da_folha.py:267`. |
| Retrato | Grava fio como `dono` e copia ações em `app/folha/retrato_folha.py:1228`; persistência em `app/folha/management/commands/lavrar_retrato_folha.py:78`; lê fio e traduz balde em `app/folha/retrato_folha.py:448`, `app/folha/retrato_folha.py:485`; ações viram links em `app/folha/retrato_folha.py:1036`. |
| Views/templates | Foto→frase/links em `app/colaboradores/views.py:982`; quadros em `app/folha/views_retrato.py:318`; template mostra valores e links prontos em `app/templates/folha/_retrato_o_que_fazer.html:33` e `app/templates/folha/_retrato_o_que_fazer.html:72`; LED pronto em `app/templates/folha/_retrato_lista.html:47`. |
| Contadores Folha | Partição em `app/folha/retrato_folha.py:695`; pílulas em `app/folha/retrato_folha.py:835`; sem ação e sem leitura em `app/folha/vigia_retido_calado.py:53` e `app/folha/vigia_retido_calado.py:57`. |
| Pauta | Admite apenas fio `nada`; desconhecido é contado à parte em `app/ponto/services/pautas_dos_discordantes.py:87`, `app/ponto/services/pautas_dos_discordantes.py:90`, `app/ponto/services/pautas_dos_discordantes.py:95`. |
| Cadastro/C11 | Exige envelope `None`, depois relê status das âncoras e aplica `TERMINAIS` em `app/escala/services/cadastro_realidade.py:472`, `app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:499`. |
| Vigia/Furo | Alcance por rota e admin por fio em `app/chamados/services/vigia_fio.py:596` e `app/chamados/services/vigia_fio.py:601`; sem canal por ausência de rota em `app/chamados/services/furo_sem_canal.py:98` e `app/chamados/services/furo_sem_canal.py:104`. |

Para validar sob âncora terminal, sem outra ação ou oferta, a mudança publicada é `admin/porta_acao/validar` → `nada/None/None`; isso conduz Folha para grupo D/sem ação, pode admitir pauta discordante e permite continuar a avaliação C11. São efeitos coerentes com os predicados existentes desses leitores (`app/chamados/services/tarefa_do_dia.py:652`, `app/chamados/services/tarefa_do_dia.py:667`, `app/folha/fora_da_folha.py:109`, `app/folha/retrato_folha.py:193`, `app/ponto/services/pautas_dos_discordantes.py:95`, `app/escala/services/cadastro_realidade.py:472`).

## 15/11 02:43 · sessao 71f85e0dcf2f · - · - · origem=Ronald (tmux)

Itens 5/7 e mutantes:

- **Nenhum positivo enfraquecido encontrado.** Permanecem `nada`/D, golden `sem_acao_disponivel=1`, transição `nada → chamado → admin`, `[D1]` e `[(D1,'BATIDA')]` com motor não chamado: `app/folha/tests/test_haiku_fora_da_folha.py:92`, `:537`, `:584`, `:595`, `:617`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:79`, `:258`, `:260`.
- **Irmãos desconhecidos têm asserções concretas.** Folha exige `nao_sei`/E e golden exige `sem_leitura_sistema=1`, conferindo ferramenta/tela; pauta exige `nao_sei=True`, rota `None`, exclusão e contagem em `sem_leitura`: `app/folha/tests/test_haiku_fora_da_folha.py:110`, `:545`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:87`.
- **Ata montada manualmente; leitor real exercitado.** Helpers devolvem dict, sem chamar produtor/lavra: `app/folha/tests/test_haiku_fora_da_folha.py:32`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:27`. O teste extrai `_celulas_da_ata`, instâncias, tarefa e porta reais: `app/chamados/tests/test_e3_r3_c7_sem_banco.py:91`. O produtor real pode produzir esses valores de lâmpada, embora acrescente outros campos: `app/ponto/services/cartorio.py:711`, `:722`, `:738`. Não há prova de estrutura semanticamente impossível; a combinação completa `furo` + lâmpadas acesas sem batidas reais não foi produzida pela lavra nesses testes [nao verificado].
- **Mutantes mordem as alterações escolhidas.** Reexecutei os oito testes novos e nove mutantes com `PYTHONDONTWRITEBYTECODE=1`: oito verdes e 9/9 mortos por asserção, sem erros [nao verificado como arquivo de minha execução]. Evidência preservada equivalente: `app/chamados/tests/e3_r3_c7_green_carga.txt:11`; `app/chamados/tests/e3_r3_c7_mutantes.txt:206`. O mutante sem join verifica presença do argumento, sem medir SQL/N+1 real: `app/chamados/tests/test_e3_r3_c7_sem_banco.py:183`, `:215`.
- **Lacuna declarada de F2:** identificação exata dos dois fios originais continua pendente; entrega não a prova e reconhece isso: `app/chamados/tests/e3_r3_c7_entrega.txt:56`.
- **Risco posterior ao estado inicial da golden, ainda não confirmado:** criar o chamado em `app/folha/tests/test_haiku_fora_da_folha.py:590` aciona relavra pelo signal genérico em `app/chamados/signals.py:190`, `:210`, `:218`. A fixture não tem batidas; a ata manual pode ser substituída antes da asserção `:595` [nao verificado]. O teste sem banco chamado “preserva … transição” só confere o estado inicial: `app/chamados/tests/test_e3_r3_c7_sem_banco.py:111`, `:119`.
- **Comentários/docstrings:** nenhum erro concreto adicional encontrado; o alcance limitado sem ORM/paridade completa é explicitado em `app/chamados/tests/test_e3_r3_c7_sem_banco.py:1`, `:83`.

## 15/11 02:37 · sessao a9d477f71569 · E3-RESTO-R3 · construtor · origem=Code

Entrega em [e3_r3_c7_entrega.txt](chamados/tests/e3_r3_c7_entrega.txt:1), com curas, REDs, censo e comandos.

[145 testes sem banco passaram](chamados/tests/e3_r3_c7_green_final.txt:174), [9/9 mutantes morreram](chamados/tests/e3_r3_c7_mutantes.txt:206) e [ruff passou](chamados/tests/e3_r3_c7_ruff.txt:1).

Pendente: fio final das duas execuções originais de F2 `[nao verificado]`; certificação pela suite do Code também pendente ([limites](chamados/tests/e3_r3_c7_entrega.txt:40)).

## 15/11 02:37 · sessao f36d7ab685c1 · JUSTIFICATIVA-PAGA-O-PERIODO · leitura · origem=Code

**F-A é fixture incompleta; F-B é expectativa antiga de apresentação.** O código sustenta esses diagnósticos, mas o **primeiro commit responsável por cada valor permanece [não verificado]**: esta cópia não contém histórico Git acessível. Não atribuo um SHA por aproximação. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:213`; `app/ponto/services/efeito_ausencia.py:96`)

**F-A — 635 versus 550**

**(1) Origem atual e commit.** A linha que devolve o saldo é `periodo['minutos'] = max(0, previsto - trabalhado)`. Ela recebe **previsto 635 e realizado zero da ata**; o previsto é lido da ata antes de qualquer fallback. Primeiro commit da série que passou a produzir esse resultado: **[não verificado]**. (`app/ponto/services/efeito_ausencia.py:159`; `app/ponto/services/efeito_ausencia.py:165`; `app/ponto/services/efeito_ausencia.py:170`; `app/escala/services/leitor_celula.py:372`)

**(2) Quem escreve, quem lê e de onde sai 635.**

| Etapa | Escritor → leitor; significado |
|---|---|
| Cadastro | `_cadastrar_pausas` grava **10:00–11:25 e 14:00–15:25** no template. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:280`) |
| Previsto preliminar | `_justificar` consulta o previsto **antes de criar a célula**. Sem célula, o vínculo lê as duas pausas do template: **720−85−85=550 minutos**. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:211`; `app/escala/models.py:1208`; `app/escala/utils.py:2338`; `app/escala/utils.py:2352`) |
| DNA da fixture | Logo depois, o helper cria a célula copiando somente `hi,hf,hii,hfi`. **Não escreve `hii2/hfi2`**. Grava ainda o previsto preliminar na ata. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:213`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:217`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:220`) |
| Nova lavra | Criar a justificativa aprovada aciona o cartório. A grade lê os pares presentes no DNA: agora existe **uma pausa**. Soma **07–10 = 180** e **11:25–19 = 455**, totalizando **635 minutos**. (`app/ponto/signals.py:161`; `app/ponto/signals.py:165`; `app/ponto/services/cartorio.py:1175`; `app/escala/utils.py:57`; `app/escala/utils.py:1658`; `app/escala/utils.py:1678`) |
| Persistência e saldo | A grade publica 635; `ata_do_dia` copia o previsto, a porta grava a ata e a fonte lê esse valor. A única entrada não contém par fechado: realizado zero; saldo **635−0=635**. (`app/escala/utils.py:1852`; `app/ponto/services/cartorio.py:739`; `app/ponto/portas/celula.py:1091`; `app/ponto/turnos.py:733`; `app/ponto/services/efeito_ausencia.py:170`) |

A chave é o **dia da jornada/turno**, inclusive quando atravessa a meia-noite; previsto, realizado e saldo são **minutos**. Ata sem realizado responde `None`, desconhecido; realizado lavrado zero responde zero. Separadamente, `cobre=None` significa cobertura inteira, não desconhecimento. (`app/ponto/turnos.py:708`; `app/ponto/turnos.py:719`; `app/escala/utils.py:2278`; `app/escala/services/leitor_celula.py:354`; `app/escala/services/leitor_celula.py:360`; `app/ponto/services/efeito_ausencia.py:106`)

**(3) Resposta pela regra.** A célula nasce **depois** do cadastro das pausas, durante `_justificar`; a pista de “célula gerada antes” não explica este caso. Porém ela nasce **sem a segunda pausa**. A soberania é da célula congelada, e a grade é fonte única do previsto; uma chave ausente da segunda pausa significa uma pausa só. Portanto **635 corresponde à célula efetivamente montada; 550 corresponde ao caso de duas pausas que o teste pretendia montar**. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:291`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:293`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:213`; `app/docs/LEIS.md:71`; `app/docs/LEIS.md:75`; `app/escala/models.py:1173`; `app/escala/models.py:1177`)

**(4) Julgamento e montagem positiva.** O vermelho não prova perda da segunda pausa pela produção: **a fixture não a colocou no DNA**. Para conservar a prova positiva de 550, cadastrar ambas e criar a célula com **`montar_dna(self.ec, self.d, True)` antes da aprovação**, substituindo a cópia manual da quatro-tupla. Esse produtor lê todas as pausas e escreve o segundo par. O comentário sobre a v2 não comprova a montagem atual. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:217`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:289`; `app/ponto/management/commands/gerar_celulas.py:73`; `app/ponto/management/commands/gerar_celulas.py:91`; `app/ponto/management/commands/gerar_celulas.py:101`)

**F-B — `None` versus 1**

**(1) Origem atual e commit.** O escritor direto é: [nao verificado]

`resumo.update(dias_abono=None, horas_abono=None, motivo_dias_abono='sem_ata')`.

O PDF o chama **depois de copiar o número do motor**. A entrega identifica essa alteração como **c10**; o SHA introdutor exato no `git log` permanece **[não verificado]**. (`app/ponto/services/efeito_ausencia.py:96`; `app/relatorios/pdf_espelho.py:479`; `app/relatorios/pdf_espelho.py:483`; `app/ponto/tests/c10_entrega.txt:16`; `app/ponto/tests/c10_entrega.txt:21`)

**(2) Caminho dos valores.**

| Etapa | Escritor → leitor; significado |
|---|---|
| Fixture | Deixa `dia_abono` sem batidas e cria somente a justificativa aprovada, sem montar sua ata. (`app/relatorios/test_espelho_falta.py:45`; `app/relatorios/test_espelho_falta.py:50`; `app/relatorios/test_espelho_falta.py:54`) |
| Fonte | Seleciona a aprovação por `data_turno`; lê previsto da célula/grade e realizado da ata. Sem realizado lavrado, escreve `minutos=None`, `trabalhado=None`, motivo `sem_ata`. (`app/ponto/services/efeito_ausencia.py:121`; `app/ponto/services/efeito_ausencia.py:139`; `app/ponto/services/efeito_ausencia.py:159`; `app/ponto/services/efeito_ausencia.py:168`) |
| Alimentação | Mantém o dia em `justificadas`, coloca-o em `abono_sem_apuracao` e o exclui do mapa numérico `min_justificados`. (`app/ponto/services/efeito_ausencia.py:55`; `app/ponto/services/efeito_ausencia.py:56`; `app/ponto/services/efeito_ausencia.py:57`) |
| Motor | Sem trabalho e com justificativa, entra em `_abono`; fora do mapa parcial, entra em `_abono_inteiro`: soma **1 dia** e **previsto/60 horas**. (`app/ponto/motor_calculo_v2.py:2611`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/motor_calculo_v2.py:2630`; `app/ponto/motor_calculo_v2.py:2631`) |
| Resumo e PDF | O coletor copia os números; o rotulador substitui-os por `None` e escreve `motivo_dias_abono='sem_ata'`. O renderizador lê ambos e imprime **“Abonos — sem apuracao ainda (sem ata)”**. (`app/relatorios/pdf_espelho.py:479`; `app/relatorios/pdf_espelho.py:483`; `app/ponto/services/efeito_ausencia.py:96`; `app/relatorios/pdf_espelho.py:790`) |

Na rota posterior de lavratura, `dias_com_abono` conta dias com crédito e realizado conhecido; realizado ausente também devolve `None`. O cartão escreve `motivo_dias_abono='sem_ata'` e preserva esse `None` ao aplicar os totais. Portanto **dias são contagem; horas/minutos são duração; `None` não é zero**. (`app/ponto/services/dia_pago.py:702`; `app/ponto/services/dia_pago.py:723`; `app/ponto/services/dia_pago.py:737`; `app/relatorios/cartao_pela_celula.py:192`; `app/relatorios/cartao_pela_celula.py:208`)

**(3) Resposta pela regra.** O item 2 do aval fornecido foi incorporado ao contrato: **sem ata e sem batida, entrando em `_abono_inteiro`, vale o previsto por P−0; esse pagamento não é inventado**. Simultaneamente, **tela e PDF permanecem rotulados “sem apuração ainda”**. Assim, neste cadastro, o motor conta o dia inteiro, mas o resumo do PDF deve conter **`dias_abono=None`, `horas_abono=None`, `motivo_dias_abono='sem_ata'`**; o papel mostra o rótulo, não “1”, “0” ou o literal “None”. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:3`; `app/ponto/tests/test_catraca_abono_do_motor_c11.py:4`; `app/ponto/services/efeito_ausencia.py:96`; `app/relatorios/pdf_espelho.py:790`)

**(4) Julgamento e montagem positiva.** A expectativa `dias_abono=1` está velha **para a apresentação sem ata**. Para conservar o positivo “uma falta e um abono numérico”, manter as batidas e a justificativa, mas fornecer **célula/ata lavrada do `dia_abono`, com previsto correspondente e `minutos_realizados=0`**, deixando `dia_falta` como está. A prova da c10 já declara essa transição: sem ata → ata W=0 → PDF com **1 dia**, sem motivo `sem_ata`. A porta real exige célula existente e é `julgar_celula`. (`app/relatorios/test_espelho_falta.py:58`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:172`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:180`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:181`; `app/ponto/services/cartorio.py:2027`; `app/ponto/services/cartorio.py:2040`)

Execução dessas montagens ajustadas e identificação dos commits introdutores: **[não verificado]**. [nao verificado]

## 15/11 02:36 · sessao 031bbd28fb38 · - · - · origem=Ronald (tmux)

F-A: a célula da fixture é criada **depois** do cadastro das duas pausas, mas nasce com apenas a primeira; a causa é o `zip(('hi','hf','hii','hfi'), marcos)`, que não escreve `hii2/hfi2`. Portanto não é uma célula antiga que antecede o cadastro. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:291`, `:292`, `:293`, `:213`, `:215`, `:217`)

**Caminho dos escritores/leitores**

1. A fixture cria o template 07–19 sem intervalos; `_cadastrar_pausas(duas=True)` declara 10–11:25 e 14–15:25 e salva o template. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:185`, `:187`, `:280`, `:286`)

2. `_bater(07:00,E)` grava a única batida; seu signal chama o cartório, mas o cartório retorna `None` quando não existe célula, sem criá-la. O núcleo também só emite evento para células existentes. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:197`, `:201`; `app/ponto/signals.py:131`, `:135`; `app/ponto/services/cartorio.py:2027`, `:2029`; `app/ponto/nucleo.py:88`, `:93`)

3. `_justificar` mede o previsto **antes** de criar a célula; sem célula, `EscalaColaborador.pausas_do_dia` cai no template, que vê ambas as pausas. A autoridade de previsto desconta todas elas: `720−85−85=550`. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:211`, `:213`; `app/escala/models.py:1208`, `:304`, `:307`; `app/escala/utils.py:2319`, `:2338`, `:2352`, `:2360`)

4. Em seguida o helper cria a célula com somente quatro chaves do DNA: 07,19,10,11:25. A segunda pausa não entra. A ata preliminar recebe o previsto calculado anteriormente (`setdefault`). (`app/ponto/tests/test_justificativa_paga_o_periodo.py:213`, `:217`, `:220`, `:222`)

5. A aprovação aciona novo julgamento, agora com célula existente. O cartório monta a grade; a grade lê as pausas do **DNA**, não completa o DNA com pausas atuais do template. O mapa percorre `PARES_PAUSA_DNA`, mas só acrescenta pares presentes; aqui devolve apenas 10–11:25. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:223`; `app/ponto/signals.py:161`, `:165`; `app/ponto/services/cartorio.py:1175`; `app/escala/utils.py:27`, `:53`, `:61`, `:1658`, `:1661`)

6. A grade soma os segmentos E→S: 07–10 (180) + 11:25–19 (455) = **635 minutos**, publica esse previsto na linha do dia; `ata_do_dia` transporta-o para a ata; a porta `lavrar_veredito` escreve a ata, substituindo o 550 preliminar da fixture. (`app/escala/utils.py:1678`, `:1682`, `:1850`, `:1852`; `app/ponto/services/cartorio.py:739`, `:1851`, `:1853`; `app/ponto/portas/celula.py:1091`)

7. `periodos_justificados` lê primeiro `ata.minutos_previstos`, inclusive zero, e o realizado da mesma ata. A única E não fornece par fechado; o realizado soma zero, conservando `aberto=True`. O saldo final é `max(0,635−0)=635`. (`app/escala/services/leitor_celula.py:372`, `:377`, `:360`, `:362`; `app/ponto/turnos.py:733`, `:739`, `:771`; `app/ponto/services/efeito_ausencia.py:159`, `:170`)

**Regra e diagnóstico**

A lei da casa declara célula soberana, com DNA congelado/irretroativo, e grade como fonte única do previsto; chave ausente significa que aquele dia não declara o marco, não desconhecimento. O leitor de pausas explicita que DNA de uma pausa responde uma pausa, não “não sei”. Logo **635 é coerente com a célula efetivamente montada**, enquanto **550 é coerente com o caso positivo de célula com duas pausas que o teste pretendia montar**. A fixture está incompleta; completar pela leitura do template violaria a autoridade congelada do dia. (`app/docs/LEIS.md:71`, `:75`; `app/escala/models.py:1173`, `:1177`, `:1675`, `:1676`)

Para conservar o caso positivo, cadastrar as duas pausas e depois construir a célula com `montar_dna(self.ec,self.d,True)` antes da aprovação, em vez de copiar a quatro-tupla manualmente. O produtor oficial lê todas as pausas e grava o segundo par `hii2/hfi2`. A célula e a ata desse cenário responderiam 550 pela mesma grade. (`app/ponto/management/commands/gerar_celulas.py:73`, `:89`, `:103`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:213`, `:217`)

**Unidades/datas**

O previsto e o saldo são minutos; o realizado agrega turnos cujo `data_turno` é o dia da jornada, não `timestamp.date()` civil. `None` em realizado significa ata ausente/incompleta; `cobre=None` é outro contrato: cobertura do dia inteiro. (`app/escala/utils.py:2278`; `app/ponto/turnos.py:708`, `:719`; `app/escala/services/leitor_celula.py:354`, `:362`; `app/ponto/services/efeito_ausencia.py:105`, `:112`)

**Histórico**

Não foi possível atribuir o primeiro commit da série: a cópia não contém `.git`; a busca literal por `29460c1d1` nos Markdown da cópia também não retornou registro. Portanto qualquer atribuição de F-A à v2/c8/c9 sem o histórico seria **[não verificado]**.

## 15/11 02:36 · sessao b7d93176c112 · - · - · origem=Ronald (tmux)

F-B: expectativa de `dias_abono=1` no resumo está velha para uma fixture **sem ata**; o motor continua contando o abono inteiro, mas o contrato de apresentação exige `None` com rótulo. [app/ponto/motor_calculo_v2.py:2611](app/ponto/motor_calculo_v2.py:2611), [app/ponto/motor_calculo_v2.py:2628](app/ponto/motor_calculo_v2.py:2628), [app/ponto/services/efeito_ausencia.py:95](app/ponto/services/efeito_ausencia.py:95)

1. **Origem e commit.** A produção escreve os valores finais em `resumo.update(dias_abono=None, horas_abono=None, motivo_dias_abono='sem_ata')`; o PDF chama esse escritor depois de copiar o resultado do motor. O próprio código atribui esse comportamento à **c10**. SHA exato e identificação pelo `git log`: **[não verificado]**, pois a cópia não contém `.git`. [app/ponto/services/efeito_ausencia.py:86](app/ponto/services/efeito_ausencia.py:86), [app/ponto/services/efeito_ausencia.py:95](app/ponto/services/efeito_ausencia.py:95), [app/relatorios/pdf_espelho.py:479](app/relatorios/pdf_espelho.py:479)

2. **Caminho dos valores.**
   - A fixture deixa os dois últimos dias previstos sem batidas; cria somente a justificativa aprovada para o penúltimo e coleta o PDF, sem chamada de lavra. [app/relatorios/test_espelho_falta.py:43](app/relatorios/test_espelho_falta.py:43), [app/relatorios/test_espelho_falta.py:50](app/relatorios/test_espelho_falta.py:50)
   - `periodos_justificados` seleciona aprovação por **`data_turno`**, usa essa data como chave e lê previsto da célula/grade; o trabalhado vem da ata. Sem ata ou sem `minutos_realizados`, o leitor devolve `None`, significando **desconhecido**, distinto de W=0. [app/ponto/services/efeito_ausencia.py:121](app/ponto/services/efeito_ausencia.py:121), [app/ponto/services/efeito_ausencia.py:139](app/ponto/services/efeito_ausencia.py:139), [app/ponto/services/efeito_ausencia.py:158](app/ponto/services/efeito_ausencia.py:158), [app/escala/services/leitor_celula.py:354](app/escala/services/leitor_celula.py:354)
   - O previsto é lido primeiro de `ata.minutos_previstos`, inclusive zero; ausente, consulta S133 `minutos_previstos_do_dia`. São **minutos**, enquanto `dias_abono` é contagem de dias. [app/escala/services/leitor_celula.py:365](app/escala/services/leitor_celula.py:365), [app/ponto/motor_calculo_v2.py:2630](app/ponto/motor_calculo_v2.py:2630)
   - `efeitos_da_ausencia` mantém o dia em `justificadas`, põe-o em `abono_sem_apuracao` e o exclui de `min_justificados`. O PDF fornece esses mapas ao motor. [app/ponto/services/efeito_ausencia.py:54](app/ponto/services/efeito_ausencia.py:54), [app/relatorios/pdf_espelho.py:333](app/relatorios/pdf_espelho.py:333), [app/relatorios/pdf_espelho.py:394](app/relatorios/pdf_espelho.py:394), [app/relatorios/pdf_espelho.py:407](app/relatorios/pdf_espelho.py:407)
   - Sem trabalho e com aprovação, o motor inclui o dia em `_abono`; ausente de `_mj`, inclui em `_abono_inteiro`, soma **1 dia** e o previsto dividido por 60 às horas. O PDF copia esse número e depois o rotulador escreve `None` e `sem_ata`. [app/ponto/motor_calculo_v2.py:2611](app/ponto/motor_calculo_v2.py:2611), [app/ponto/motor_calculo_v2.py:2628](app/ponto/motor_calculo_v2.py:2628), [app/relatorios/pdf_espelho.py:479](app/relatorios/pdf_espelho.py:479), [app/ponto/services/efeito_ausencia.py:95](app/ponto/services/efeito_ausencia.py:95)
   - A renderização lê os dois campos e imprime **“Abonos — sem apuracao ainda (sem ata)”**, sem imprimir 1 ou zero. [app/relatorios/pdf_espelho.py:790](app/relatorios/pdf_espelho.py:790)

3. **Regra.** O item 2 do aval anexado diz expressamente que o dia **sem ata e sem batida**, entrando em `_abono_inteiro`, vale o previsto por **COMPLETA-ATÉ-O-PREVISTO, P−0**; portanto o valor do motor é correto, não “inventado”. O mesmo aval mantém tela/PDF rotulados “sem apuração ainda”. O anexo não existe como arquivo numerado na cópia: **[não verificado: localização arquivo:linha do aval]**. A implementação da apresentação coincide com essa orientação. [app/ponto/motor_calculo_v2.py:2628](app/ponto/motor_calculo_v2.py:2628), [app/ponto/services/efeito_ausencia.py:95](app/ponto/services/efeito_ausencia.py:95), [app/relatorios/pdf_espelho.py:790](app/relatorios/pdf_espelho.py:790)

4. **Montagem que conserva o positivo.** Para continuar provando **falta=1 e abono=1 no PDF**, a fixture deve fornecer **ata lavrada para `dia_abono`, com `minutos_realizados=0` e previsto correspondente**, conservando o dia sem batidas e deixando `dia_falta` como está. O teste da própria c10 demonstra a transição sem ata → ata W=0 → PDF/tela com 1 dia, sem motivo `sem_ata`; há também prova ORM positiva com essa ata. [app/ponto/tests/test_justificativa_c10_sem_banco.py:168](app/ponto/tests/test_justificativa_c10_sem_banco.py:168), [app/ponto/tests/test_justificativa_paga_o_periodo.py:622](app/ponto/tests/test_justificativa_paga_o_periodo.py:622), [app/ponto/tests/test_justificativa_paga_o_periodo.py:644](app/ponto/tests/test_justificativa_paga_o_periodo.py:644)

A porta real de lavra é `julgar_celula`; o teste ORM usa essa porta e verifica o W persistido. Execução da montagem modificada de F-B: **[não verificado]**, não rodei banco nem testes. [app/ponto/services/cartorio.py:2011](app/ponto/services/cartorio.py:2011), [app/ponto/tests/test_justificativa_paga_o_periodo.py:703](app/ponto/tests/test_justificativa_paga_o_periodo.py:703)

## 15/11 02:20 · sessao 6e535e4da7d0 · BATIDA-DEDUP-RETRATADA · verificador · origem=Code

**Não demonstrei regressão nova de produção no diff. A corrida de classificação do v2 está fechada, e o horário da triagem foi restaurado. Encontrei três lacunas concretas de testes e a corrida SQL preexistente.** (`app/ponto/registro_batida.py:155`; `app/ponto/registro_batida.py:161`; `app/ponto/services/triagem_batida.py:657`; `app/ponto/tests/dedup_retratada_c3_checks.txt:39`)

Referências abaixo relativas a `app/`, salvo indicação contrária.

| Arquivo:linha | O que está errado | Caso concreto | Gravidade |
|---|---|---|---|
| `ponto/services/triagem_batida.py:666`; `ponto/tests/test_dedup_retratada_c1.py:150` | **Cobertura insuficiente de sequência no dedup.** Mutante acrescentando `sequencia=1` ao `.filter(tipo=tipo)` passou nos **31 testes**, inclusive tripwire e paridade. As fixtures anteriores têm sequência 1; o selo verifica a origem da consulta, sem conferir esse filtro. (`ponto/tests/test_dedup_retratada_c1.py:194`) | Viva S18:00, sequência 2; pedido S18:03. Original responde `ja_registrada`; mutante chama a porta, cria segunda linha e preenche sucesso. Reproduzido em memória pelo executor existente. (`ponto/tests/test_dedup_retratada_c2.py:47`) | **fila** — lacuna da bateria; esse filtro não existe no código atual. |
| `ponto/registro_batida.py:161`; `ponto/tests/test_dedup_retratada_c1.py:26` | **Cobertura temporal concentrada no segundo zero.** Mutante `batida.criada_neste_toque = timestamp.second == 0` passou nos **31 testes**. Variar microssegundos conserva o segundo zero. (`ponto/tests/test_dedup_retratada_c3.py:79`) | Nova batida às 18:03:01 nas três cascas: mutante grava uma linha, publica SSE, responde `ja_registrada` e pula foto/ordem. Reproduzido em memória; o código atual atribui `True` incondicionalmente. (`ponto/tests/test_dedup_retratada_c2.py:64`; `ponto/services/resposta_batida.py:14`) | **fila** — defeito do mutante, não do original. |
| `ponto/services/triagem_batida.py:716`; `ponto/tests/test_dedup_retratada_c2.py:252` | **Falta controle com duas opostas vivas elegíveis.** Remover `.order_by('-timestamp')` passou nos **31 testes**. Sem essa ordenação, a autoridade entrega ordem crescente. (`ponto/turnos.py:2883`) | E18:00/id1 e E18:02/id2; pedido S18:03, janela 300 s. Original apresenta id2; mutante apresenta id1. Reproduzido em memória. (`ponto/tests/test_dedup_retratada_c2.py:47`) | **fila** — cobertura insuficiente da testemunha apresentada. |
| `ponto/registro_batida.py:152`; `ponto/registro_batida.py:158`; `ponto/models.py:106` | **`IntegrityError` concorrente continua sem captura.** É preexistente; o diff somente acrescenta as marcas após os creates. (`ponto/tests/dedup_retratada_c3_checks.txt:38`) | A e B consultam ausência; B grava antes do create de A. A colide na unicidade colaborador/timestamp e não alcança marca/helper. Resposta HTTP e ocorrência em produção: **[nao verificado]**. | **fila** — distinta da corrida do v2. |

Os três mutantes foram aplicados somente em memória, nos dois mapas `SOBRESCRITAS`; também adaptei a leitura de `open()` para conferir a paridade sobre fontes mutadas. Cada execução terminou com **31 testes, zero falhas e zero erros**. Os pontos de injeção e execução são os usados pela própria entrega. (`ponto/tests/test_dedup_retratada_c3.py:228`; `ponto/tests/test_dedup_retratada_c3.py:234`; `ponto/tests/test_dedup_retratada_c2.py:288`)

A marca e os valores consumidos pelo diff ficaram assim:

| Valor | Quem escreve e quem lê |
|---|---|
| `criada_neste_toque` | **Dois escritores de produção**, ambos dentro de `registrar_batida`, imediatamente após cada create. **Um leitor direto**, o helper; as três cascas consomem seu resultado. Não é campo do modelo nem argumento persistido pelo create. (`ponto/registro_batida.py:155`; `ponto/registro_batida.py:161`; `ponto/services/resposta_batida.py:14`; `ponto/models.py:8`; `api/views.py:693`; `api/views_core.py:684`; `ponto/services/triagem_batida.py:723`) |
| Colaborador, instante e tipo pedidos | Cascas fornecem os argumentos; dedup/porta os usam nas consultas; create persiste os argumentos. As APIs usam `ts_efetivo`; triagem usa `agora`. (`api/views.py:507`; `api/views.py:531`; `api/views.py:654`; `api/views_core.py:645`; `ponto/services/triagem_batida.py:626`; `ponto/services/triagem_batida.py:700`; `ponto/registro_batida.py:153`) |
| `retratada_em` | Dedup lê o filtro da autoridade; porta mantém seus filtros vivos e o lookup de chave exata. Helper não usa retratação para inferir criação. Nenhuma retratação é desfeita pelo delta. (`ponto/turnos.py:2876`; `ponto/registro_batida.py:109`; `ponto/registro_batida.py:120`; `ponto/services/resposta_batida.py:14`) |
| PK, tipo, timestamp e sequência devolvidos | Helper lê exclusivamente o objeto retornado. Apresenta seu timestamp local, preservando microssegundos, e sua sequência antiga. A sequência de uma nova continua vindo de `sequencia_na_jornada`. (`ponto/services/resposta_batida.py:17`; `ponto/services/resposta_batida.py:21`; `api/views.py:554`; `api/views_core.py:540`; `ponto/services/triagem_batida.py:672`) |
| `status`, mensagem, sucesso e disputa | Helper escreve `ja_registrada`, mensagem derivada do objeto, `sucesso=True`, `em_disputa=False`, motivos vazios. As duas últimas constantes não certificam a disputa atual da batida. (`ponto/services/resposta_batida.py:19`) |
| `_resposta_existente` | Helper produz; cada casca retorna antes dos efeitos posteriores de criação. (`api/views.py:693`; `api/views_core.py:684`; `ponto/services/triagem_batida.py:723`) |
| `g_op`, `op`, id/tipo da recusa | `_janela_oposto` fornece a janela; triagem consulta a autoridade viva e apresenta a candidata. Essa consulta não decide a recusa. (`ponto/registro_batida.py:21`; `ponto/registro_batida.py:145`; `ponto/services/triagem_batida.py:714`) |
| `fora_horario_info`, justificativa e aprovação | Emissor escreve informação/contexto/sessão; triagem transforma a informação em justificativa e `aprovada=False` enviados à porta. (`ponto/services/triagem_batida.py:478`; `ponto/services/triagem_batida.py:486`; `ponto/services/triagem_batida.py:685`; `ponto/services/triagem_batida.py:709`) |
| `ALLOW`, contagem e dono | Tripwire declara os totais e compara igualdade exata: APIs 2/2, triagem 1, helper 0. (`ponto/tests/test_tripwire_bocais_apuracao.py:21`; `ponto/tests/test_tripwire_bocais_apuracao.py:27`; `ponto/tests/test_tripwire_bocais_apuracao.py:37`) |

**Não encontrei caminho atual de criação chegando sem marca, nem reutilização chegando marcada.** Entre create e retorno não há cópia, refresh ou reconsulta; entre retorno e helper também não. Fechamento recebe colaborador/instante, sem receber a batida. Os receivers inspecionados não substituem a instância retornada. (`ponto/registro_batida.py:152`; `ponto/registro_batida.py:177`; `api/views.py:669`; `api/views_core.py:660`; `ponto/services/triagem_batida.py:713`; `ponto/signals.py:35`; `ponto/signals.py:132`; `ponto/signals.py:207`; `colaboradores/signals.py:87`)

O censo direto fora dos testes encontrou **16 sítios**, incluindo o comando de ensaio: [nao verificado]

| Chamador | Arquivo:linha; consumo |
|---|---|
| Três cascas | `api/views.py:654`; `api/views_core.py:645`; `ponto/services/triagem_batida.py:700`: objeto passa diretamente ao helper. |
| Justificativas e aprovação em lote | `ponto/services/justificativa.py:188`; `ponto/services/justificativa.py:361`; `ponto/views.py:1818`: leem `None`, PK, justificativa e vinculam FK. (`ponto/services/justificativa.py:195`; `ponto/services/justificativa.py:370`; `ponto/views.py:1825`) |
| Veredito | `ponto/services/veredito_celula.py:323`: verifica recusa por `None`. (`ponto/services/veredito_celula.py:325`) |
| Materialização | `chamados/services/materializacao.py:933`; `chamados/services/materializacao.py:1013`: leem vínculo/PK e anotam campos explícitos. (`chamados/services/materializacao.py:952`; `chamados/services/materializacao.py:1041`) |
| Regularização externa | `chamados/services/regularizacao_ext.py:78`: lê PK para auditoria e retorna o objeto. (`chamados/services/regularizacao_ext.py:95`; `chamados/services/regularizacao_ext.py:106`) |
| Exceção autorizada | `chamados/services/acoes_chamado.py:44`: lê justificativa e passa objeto a anotação/foto. (`chamados/services/acoes_chamado.py:54`; `chamados/services/acoes_chamado.py:67`) |
| Comando de ensaio | `ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229`: três retornos descartados; depois compara `None`/PK. (`ponto/management/commands/testar_pwa.py:230`) |

Nesses consumidores não encontrei enumeração dos atributos da batida que incorporasse a marca nova. `anotar` lê somente campos explicitamente fornecidos, sob allowlist; SSE também monta payload explícito. Chamadas dinâmicas e execução desses fluxos com ORM real: **[nao verificado]**. (`ponto/registro_batida.py:243`; `ponto/registro_batida.py:246`; `ponto/realtime.py:32`)

Para os ramos da porta, o comportamento é:

| Ramo | APIs UI/core | Triagem |
|---|---|---|
| Viva do mesmo tipo ≤60 s | Se alcançar a porta, retorna existente sem marca; helper responde `ja_registrada` com seus campos e interrompe efeitos posteriores. Normalmente o dedup de cinco minutos captura antes; correção de tipo pode permitir alcançar a porta. (`ponto/registro_batida.py:113`; `api/views.py:577`; `api/views.py:693`; `api/views_core.py:684`) | Mesmo tratamento pelo helper; pode alcançar por intercalação concorrente ou batida futura dentro dos 60 s. (`ponto/registro_batida.py:113`; `ponto/services/triagem_batida.py:666`; `ponto/services/triagem_batida.py:723`) |
| Mesma chave viva/retratada/outro tipo | Consulta temporal tem precedência; depois chave exata. Retorna objeto antigo, sem desfazer retratação, com `ja_registrada` e sem efeitos posteriores. (`ponto/registro_batida.py:113`; `ponto/registro_batida.py:120`; `ponto/services/resposta_batida.py:19`) | Igual. (`ponto/services/triagem_batida.py:723`) |
| Oposta → `None` | `ignorada_intervalo` antes do helper; sem efeitos posteriores. Reconsulta oposta para apresentar payload. (`api/views.py:669`; `api/views_core.py:660`) | `ignorada_intervalo`, com id/tipo consultados no universo vivo e janela da porta; sem efeitos posteriores. (`ponto/services/triagem_batida.py:713`) |
| Create com conflito revisado | **Inalcançável pelas três chamadas**, que passam autoridade humana falsa. Na porta, esse ramo marca True e agenda fechamento. (`api/views.py:656`; `api/views_core.py:647`; `ponto/services/triagem_batida.py:702`; `ponto/registro_batida.py:150`) | Mesma restrição. (`ponto/services/triagem_batida.py:702`) |
| Create normal, inclusive oposta admitida pelo marco próprio | Marca True; helper libera ordem, detector/chamado leve, disputa, foto, geofence, autoresolve de entrada e aviso; responde `ok`. Efeitos são condicionais aos respectivos ramos. (`ponto/registro_batida.py:145`; `ponto/registro_batida.py:158`; `api/views.py:696`; `api/views.py:704`; `api/views.py:758`; `api/views.py:772`; `api/views.py:792`; `api/views.py:812`) | Libera disputa pendente, ordem, foto, geofence, autoresolve, contexto de sucesso e aviso. (`ponto/services/triagem_batida.py:726`; `ponto/services/triagem_batida.py:745`; `ponto/services/triagem_batida.py:757`) |
| `IntegrityError` concorrente | Exceção escapa antes da marca/helper; HTTP efetivo **[nao verificado]**. (`ponto/registro_batida.py:158`) | Igual. (`ponto/services/triagem_batida.py:700`) |

Criação também dispara os receivers do ORM e fechamento da porta; reutilização não executa novo create. SSE consulta `created` do ORM, independentemente da marca transiente. (`ponto/signals.py:40`; `ponto/registro_batida.py:156`; `ponto/registro_batida.py:162`)

**A corrida específica do v2 está fechada:** se B confirma antes das consultas de A, A reutiliza objeto sem marca; somente o create executado por A recebe a marca. A colisão posterior às consultas permanece sendo o problema SQL separado da tabela. (`ponto/registro_batida.py:113`; `ponto/registro_batida.py:120`; `ponto/registro_batida.py:161`)

Quanto à ordem e ao escopo completo:

| Conferência | Resultado |
|---|---|
| `_bloqueio_horario_clt` | Está incondicionalmente antes da recuperação da confirmação e do dedup. Pode criar chamado e atualizar contexto/sessão mesmo quando depois houver reutilização ou recusa. Essa posição corresponde ao diff fornecido e à comparação histórica registrada; não reexecutei o objeto git nesta cópia. (`ponto/services/triagem_batida.py:657`; `ponto/services/triagem_batida.py:459`; `ponto/services/triagem_batida.py:486`; `ponto/tests/dedup_retratada_c3_checks.txt:39`) |
| Outros efeitos anteriores à porta | Não identifiquei outro reposicionamento no diff integral fornecido. O novo predicado permite que tentativas antes barradas pela retratada alcancem o preparo já existente. (`api/views.py:531`; `api/views.py:554`; `ponto/services/triagem_batida.py:666`; `ponto/services/triagem_batida.py:672`) |
| Janelas, dupla viva e sequência | APIs conservam ±5 min e escolha mais recente; triagem conserva `[agora−5min, agora]` e escolha crescente. Porta conserva ±60 s para mesmo tipo. Mensagens da dupla viva e autoridade da sequência permanecem. (`api/views.py:529`; `api/views.py:531`; `api/views.py:542`; `ponto/services/triagem_batida.py:665`; `ponto/turnos.py:2883`; `ponto/models.py:105`; `ponto/registro_batida.py:106`) |
| Mudanças além do filtro | O helper muda resposta/efeitos de reutilização. Na triagem, `ignorada_intervalo` passa a apresentar testemunha viva usando 60/300 s da porta, em lugar da reconsulta crua fixa em 60 s. Não muda quem a porta recusa. (`ponto/services/resposta_batida.py:14`; `ponto/services/triagem_batida.py:714`; `ponto/registro_batida.py:145`) |
| Escritor, universo e juiz | Não identifiquei escritor novo fora da porta nem derivação paralela do universo vivo. Helper apresenta o resultado causal declarado pela porta; não decide admissibilidade. (`ponto/registro_batida.py:155`; `ponto/services/resposta_batida.py:14`; `ponto/turnos.py:2876`; `core/portas.py:103`) |

Os selos **mordem os casos declarados**, mas não certificam toda a semântica: tripwire conta regex exata; C1 acompanha origem da consulta; paridade compara AST das funções de mesmo nome nas duas APIs, com whitelist vazia. O harness original da paridade lê disco, sem `SOBRESCRITAS`; essa limitação já está declarada na entrega. (`ponto/tests/test_tripwire_bocais_apuracao.py:30`; `ponto/tests/test_dedup_retratada_c1.py:175`; `api/tests/test_contract_paridade_kernels.py:12`; `api/tests/test_contract_paridade_kernels.py:19`; `api/tests/test_contract_paridade_kernels.py:31`; `ponto/tests/dedup_retratada_c3_entrega.txt:162`)

Reexecutei C1–C3: **31 testes OK**, incluindo os contratos existentes. Suite inteira, opus final, smoke real col677 nas duas cascas e medições de produção dos 60 dias: **[nao verificado]**; os testes locais não encerram esses requisitos. (`ponto/tests/dedup_retratada_c3_green.txt:38`; `ponto/tests/dedup_retratada_c3_entrega.txt:169`; `ponto/tests/dedup_retratada_c3_entrega.txt:176`)

## 15/11 02:18 · sessao 085ea1e4b857 · - · - · origem=Ronald (tmux)

Achado reproduzido, referências relativas a `app/`:

| Arquivo:linha | O que está errado | Caso concreto | Gravidade |
|---|---|---|---|
| `ponto/services/triagem_batida.py:666`; `ponto/tests/test_dedup_retratada_c1.py:261` | Falta teste com **duas duplas vivas elegíveis**. Mutante de uma linha adicionando `.order_by('-timestamp')` antes de `.first()` passa nos 31 testes C1–C3, com zero falhas/erros. A paridade API fica legitimamente intacta; o tripwire mantém a mesma contagem. | Duas S vivas: 17:59, id=1; 18:02, id=2. Tentativa às 18:03: original retorna id=1; mutante retorna id=2. O teste existente põe apenas uma viva e uma retratada. (`ponto/tests/test_dedup_retratada_c1.py:261–268`; `ponto/tests/test_tripwire_bocais_apuracao.py:30`; `api/tests/test_contract_paridade_kernels.py:28`) | **fila**: lacuna real de cobertura; a implementação atual conserva a escolha anterior. (`ponto/models.py:105`; `ponto/turnos.py:2883`) |
| `ponto/registro_batida.py:152`; `ponto/registro_batida.py:158`; `ponto/models.py:106` | `IntegrityError` concorrente continua sem captura nesses creates. É preexistente conforme o diff fornecido; não confundir com a corrida de classificação do v2. | A e B passam pelas duas consultas sem linha; B grava a chave antes do create de A. A encontra a restrição `(colaborador,timestamp)`. Resposta HTTP/ocorrência em produção: **[nao verificado]**. (`ponto/registro_batida.py:107`; `ponto/registro_batida.py:120`; `ponto/models.py:106`) | **fila** |

Ramos conferidos:

| Ramo da porta | Resposta e efeitos nas cascas |
|---|---|
| Viva mesmo tipo ≤60 s | Retorna existente sem marca, sem create/fechamento da porta; helper produz `ja_registrada`, interrompendo efeitos posteriores. Normalmente dedup de cinco minutos captura antes; APIs podem chegar após correção de tipo, triagem pode chegar por concorrência/futuro ≤60 s. (`ponto/registro_batida.py:113`; `ponto/services/resposta_batida.py:14`; `api/views.py:577`; `api/views.py:693`; `api/views_core.py:684`; `ponto/services/triagem_batida.py:723`) |
| Mesma chave viva/retratada/outro tipo | Temporal ganha primeiro; caso contrário retorna a chave exata sem marca. Payload usa os campos antigos; não cria, não desfaz retratação e não roda efeitos posteriores. (`ponto/registro_batida.py:113`; `ponto/registro_batida.py:120`; `ponto/services/resposta_batida.py:17`) |
| Oposta → `None` | As três chamadas usam `autoridade_humana=False`; retornam `ignorada_intervalo` antes do helper/efeitos posteriores. Triagem consulta agora a janela da porta e o universo vivo para apresentar id/tipo. (`api/views.py:656`; `api/views.py:669`; `api/views_core.py:647`; `api/views_core.py:660`; `ponto/services/triagem_batida.py:702`; `ponto/services/triagem_batida.py:713`) |
| Create com conflito revisado | Marca True e fechamento; inacessível pelas três chamadas, que passam autoridade False. (`ponto/registro_batida.py:150`; `ponto/registro_batida.py:152`; `ponto/registro_batida.py:155`; `api/views.py:656`; `api/views_core.py:647`; `ponto/services/triagem_batida.py:702`) |
| Create normal, inclusive oposta admitida pelo marco | Marca True, fechamento; helper libera efeitos. APIs: ordem, detector/chamado leve, disputa, foto, geofence, autoresolve de E, aviso, `ok`. Triagem: disputa quando pendente, ordem, foto, geofence, autoresolve, contexto de sucesso e aviso. (`ponto/registro_batida.py:145`; `ponto/registro_batida.py:158`; `api/views.py:696`; `api/views.py:702`; `api/views.py:758`; `api/views.py:772`; `api/views.py:780`; `api/views.py:792`; `api/views.py:810`; `ponto/services/triagem_batida.py:726`; `ponto/services/triagem_batida.py:745`; `ponto/services/triagem_batida.py:748`; `ponto/services/triagem_batida.py:757`) |
| `IntegrityError` | Exceção escapa antes da marca/helper/efeitos posteriores; HTTP efetivo **[nao verificado]**. (`ponto/registro_batida.py:152`; `ponto/registro_batida.py:155`; `ponto/registro_batida.py:158`; `ponto/registro_batida.py:161`) |

Em todos os ramos de triagem, `_bloqueio_horario_clt` roda antes da confirmação/dedup/porta e pode criar chamado e atualizar contexto/sessão. Essa posição/condição coincide com o diff integral fornecido e com a conferência histórica registrada; o objeto git `880331a8c` não está disponível nesta cópia para repetir a comparação. (`ponto/services/triagem_batida.py:657`; `ponto/services/triagem_batida.py:459`; `ponto/services/triagem_batida.py:478`; `ponto/services/triagem_batida.py:486`; `ponto/tests/dedup_retratada_c3_checks.txt:28`; `ponto/tests/dedup_retratada_c3_checks.txt:39`)

Não encontrei alteração adicional de janela, sequência ou mensagem da dupla viva no código atual: APIs conservam ±5 min e mais recente; triagem conserva `[agora−5min,agora]` e mais antiga para o colaborador fixo. A sequência continua calculada pela mesma autoridade. (`api/views.py:529`; `api/views.py:531`; `api/views.py:541`; `api/views.py:554`; `api/views_core.py:515`; `api/views_core.py:517`; `api/views_core.py:540`; `ponto/services/triagem_batida.py:665`; `ponto/services/triagem_batida.py:666`; `ponto/services/triagem_batida.py:672`; `ponto/models.py:105`; `ponto/turnos.py:2883`)

O helper acrescenta classificação do retorno para suprimir efeitos posteriores em reutilização; não consulta universo nem escreve `Batida`. A mudança de `ignorada_intervalo` na triagem é de identificação da testemunha após a recusa, com a mesma janela e universo da porta; a decisão de recusar continua exclusivamente na porta. (`ponto/services/resposta_batida.py:7`; `ponto/services/resposta_batida.py:14`; `ponto/registro_batida.py:138`; `ponto/registro_batida.py:145`; `ponto/services/triagem_batida.py:713`)

_5505 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
