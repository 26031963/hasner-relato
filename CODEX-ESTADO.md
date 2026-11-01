# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 20:59** · publicado em **31/10/2026 21:03** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 20:59 · sessao b9fa3134066a · PADRAO-DE-CIMA · verificador · origem=Code

**Veredito: bloqueia.** O fallback ainda pode liberar uma obra usando escopo antigo; o resolvedor pode bloquear sob a trava; e o harness aceita algumas mortes anteriores à decisão como “mordida”. (`bin/disjuntor.py:1488`, `bin/disjuntor.py:1530`, `bin/disjuntor.py:1247`, `bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:260`)

A identidade Git da cópia e a sequência dos dois commits ficam **[não verificado]**: não há repositório Git acessível nesta cópia. Não executei os selos completos, pois criam, alteram e apagam arquivos temporários. (`bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:39`, `bin/tests/test_disjuntor.sh:39`, `bin/tests/_mordida.sh:152`)

**1. Quem escreve e quem lê os estados**

| Estado | Quem escreve | Quem lê; multiplicidade |
|---|---|---|
| `logs/sessao/disjuntor_estado.json` | `_com_estado`: lê, chama o callback e substitui o arquivo sob `flock`. Há escritores lógicos B1, B3, PDC, B2 e B4, todos pela mesma porta física. | `_com_estado`, `ler_estado` e seus consumidores. **Uma porta física no instrumento**, vários produtores lógicos; os selos também escrevem diretamente nas árvores de teste. (`bin/disjuntor.py:244`, `bin/disjuntor.py:275`, `bin/disjuntor.py:281`, `bin/disjuntor.py:494`, `bin/disjuntor.py:986`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1745`, `bin/disjuntor.py:2029`, `bin/tests/test_padrao_de_cima.sh:556`) |
| `pdc.eventos[sha]`: `visto_em`, `tipo`, `obra`, `entidades`, `res` | `decide_pdc` cria o evento; na reclassificação altera `entidades/res`, preservando `visto_em`. | Deduplicação/reclassificação, janela, agrupamento, nomeação, histórico do escopo e painel. **Um escritor lógico no instrumento.** (`bin/disjuntor.py:1386`, `bin/disjuntor.py:1397`, `bin/disjuntor.py:1400`, `bin/disjuntor.py:1298`, `bin/disjuntor.py:1308`, `bin/disjuntor.py:1438`, `bin/disjuntor.py:1448`, `bin/disjuntor.py:1600`) |
| `pdc.nomeados` | `decide_pdc` marca `sha\|entidade` quando consome uma liberação. | `_pdc_disparadas` verifica se resta evento não nomeado. (`bin/disjuntor.py:1440`, `bin/disjuntor.py:1306`, `bin/disjuntor.py:1312`) |
| `pdc.libera_vistas` | `decide_pdc` registra o hash consumido. | A própria decisão impede reutilização da liberação. (`bin/disjuntor.py:1435`, `bin/disjuntor.py:1430`) |
| `pdc.fotos` | `decide_pdc` cria a fotografia do topo e remove fotografias de episódios encerrados. | A própria decisão impede liberação por linha anterior ao disparo. (`bin/disjuntor.py:1403`, `bin/disjuntor.py:1412`, `bin/disjuntor.py:1445`, `bin/disjuntor.py:1433`) |
| `pdc.disparadas` | `decide_pdc` substitui pelo agrupamento recalculado. | Reconhecimento de disparo novo, painel e cópia para o escopo. (`bin/disjuntor.py:1444`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:1413`, `bin/disjuntor.py:1608`, `bin/disjuntor.py:1480`) |
| `hist`, `lido`, `duvida`, `final`, `liberadas`, `avisos` | A closure `_fn` mantém a fotografia e os resultados desta transição. | `_anuncia` grava escopo/trilha; depois, a construção de `DecisaoPDC` preserva negativas e incertezas. São estados por chamada, compartilhados pelas closures. (`bin/disjuntor.py:1366`, `bin/disjuntor.py:1378`, `bin/disjuntor.py:1392`, `bin/disjuntor.py:1447`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1462`) |
| `padrao_de_cima_escopo.json`: `em`, `disparadas`, `obras_com_evento`, `duvida` | `_pdc_grava_escopo`, chamado após gravar o estado, ainda sob sua trava. | `_pdc_escopo_lido` e fallback de `estado_pdc`. **Um escritor no instrumento**, mas gravação independente da gravação do estado. `em` é escrito e não é conferido no fallback. (`bin/disjuntor.py:293`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1480`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1512`) |
| `RELATO.md` | `relato_negacao` delega a `esteira_vigia.no_relato`, que tranca o diretório e faz substituição atômica. **Há outros escritores reais:** `gerar_cortes` e `deploy_agendado` escrevem diretamente, fora dessa trava. | PDC/B3 usam `_relato_consistente`; o Stop lê o topo; handoff, alarme de ociosidade e publicação também leem o documento. A multiplicidade de escritores já está declarada como resíduo. (`bin/disjuntor.py:236`, `app/core/esteira_vigia.py:848`, `app/core/esteira_vigia.py:884`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`, `bin/disjuntor.py:697`, `bin/hook_stop_fila1.py:429`, `bin/handoff_sessao.sh:92`, `bin/alarme_sessao_ociosa.py:123`, `bin/relato.sh:104`, `app/core/esteira_vigia.py:839`) |
| Fontes `models.py`, `models/*.py` e arquivos citados; caches do resolvedor | O resolvedor somente lê as fontes; `_prepara` e `_por_sitio` preenchem `nomes`, `modelos`, mapas e `_fontes` por decisão. | `entidades` combina censo, modelos e cadastro `PROGRAMAS`. Autoria das alterações das fontes em produção: **[não verificado]**. (`bin/disjuntor.py:1214`, `bin/disjuntor.py:1235`, `bin/disjuntor.py:1273`, `bin/disjuntor.py:1289`, `app/core/censo_escritas.py:89`) |
| `payload`, `cru`, `_PDC_LIVRE_NESTA` | `main_pretool` lê stdin, faz o parse e limpa o conjunto; `pretool` acrescenta obra julgada livre. | Classificadores do envelope e `_ramo_pdc`, que dispensa nova negativa para aquela obra. (`bin/disjuntor.py:2038`, `bin/disjuntor.py:2041`, `bin/disjuntor.py:2045`, `bin/disjuntor.py:2018`, `bin/disjuntor.py:1591`) |

O mapa do harness é distinto: seus logs têm deliberadamente vários processos escritores. (`bin/tests/_mordida.sh:142`, `bin/tests/_mordida.sh:46`, `bin/tests/_mordida.sh:106`)

| Estado | Quem escreve | Quem lê; multiplicidade |
|---|---|---|
| `sonda_exc/sitecustomize.py` | O `source` do harness gera o arquivo. | Python descendente, pelo `PYTHONPATH` fornecido ao caso. (`bin/tests/_mordida.sh:35`, `bin/tests/_mordida.sh:37`, `bin/tests/_mordida.sh:143`) |
| `<raiz>.exc` | `_mordida_sondado` trunca; callbacks `RAISE` dos Python acrescentam registros. | Controle procura cinco tipos de erro; comparador verifica exceções declaradas. **Vários escritores.** (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:106`, `bin/tests/_mordida.sh:176`, `bin/tests/_mordida.sh:222`, `bin/tests/_mordida.sh:252`) |
| `<raiz>.esc` | Truncador e `sys.excepthook` dos Python. | Comparador de multiplicidade de exceções escapadas. **Vários escritores.** (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:117`, `bin/tests/_mordida.sh:223`, `bin/tests/_mordida.sh:258`) |
| `<raiz>.proc` | Truncador; startup escreve `INI`; `atexit` escreve `FIM`; handler escreve `SINAL`. | `abertos` compara conjuntos de PIDs e quantidades. **Vários escritores.** (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:46`, `bin/tests/_mordida.sh:50`, `bin/tests/_mordida.sh:56`, `bin/tests/_mordida.sh:214`) |
| `<raiz>.marca` | Truncador e os três coletores Python. O novo trap Bash escreve suas provas em variáveis. | `_mordida_morre` consulta a última marca quando o último comando não é `return`. (`bin/tests/_mordida.sh:139`, `bin/tests/test_disjuntor.sh:776`, `bin/tests/test_disjuntor.sh:947`, `bin/tests/test_disjuntor.sh:1122`, `bin/tests/_mordida.sh:191`) |
| `base_<caso>.exc/.proc/.esc` | `_mordida_controle` copia cada arquivo somente se ainda não existir. | Mutantes posteriores reutilizam a base pelo **nome do caso**, entre diferentes raízes. (`bin/tests/_mordida.sh:179`, `bin/tests/_mordida.sh:196`, `bin/tests/_mordida.sh:225`, `bin/tests/_mordida.sh:258`, `bin/tests/test_padrao_de_cima.sh:648`) |
| `<raiz>.orig.d`, `<raiz>.novo` e fonte mutada | `_mordida_muta` remove/recria a fotografia original, aplica substituições e grava os textos novos. | Verificador da declaração de guarda. (`bin/tests/_mordida.sh:152`, `bin/tests/_mordida.sh:159`, `bin/tests/_mordida.sh:167`, `bin/tests/_mordida.sh:168`, `bin/tests/_mordida.sh:246`) |
| `_MORDIDA_CASO/PID/SEQ/ULT/VAL/MARCAS` | Inicializador e trap `DEBUG`. | Trap e julgamento do último comando. São globais dentro do selo. (`bin/tests/_mordida.sh:124`, `bin/tests/_mordida.sh:125`, `bin/tests/_mordida.sh:140`, `bin/tests/_mordida.sh:187`) |
| `SELO_EXC_LOG/DIR`, `SELO_ESC_LOG`, `SELO_PROC_LOG`, `MORDIDA_MARCAS`, `PYTHONPATH`, `PYTHONDONTWRITEBYTECODE` | `_mordida_sondado` fornece o ambiente ao caso. | Startup Python, sonda e coletores. `MORDIDA_SEQ` é exportado pelo trap e retirado ao terminar. (`bin/tests/_mordida.sh:41`, `bin/tests/_mordida.sh:126`, `bin/tests/_mordida.sh:142`, `bin/tests/_mordida.sh:145`, `bin/tests/test_disjuntor.sh:776`) |
| `HOOK_STOP_RAIZ`, `CODEX_SESSIONS_DIR`, `PADRAO_DE_CIMA_BIN` | Selos atribuem os dois primeiros aos subprocessos; o último pode selecionar o código do RED. | Inicialização das raízes, leitor de sessões e seleção de `BIN`. (`bin/tests/test_padrao_de_cima.sh:90`, `bin/tests/test_disjuntor.sh:523`, `bin/tests/test_padrao_de_cima.sh:634`, `bin/disjuntor.py:67`, `bin/hook_stop_fila1.py:30`) |
| SIGTERM, `atexit`, `sys.excepthook`, monitoramento | `sitecustomize` registra callbacks; instala SIGTERM somente se encontrar default; substitui `sys.excepthook`, guardando o anterior. O handler restaura default e reenvia SIGTERM. | Runtime Python chama esses callbacks. A sonda também escreve `_selo_visto/_selo_assin` nas exceções, lidos pelo monitor e hook. (`bin/tests/_mordida.sh:52`, `bin/tests/_mordida.sh:58`, `bin/tests/_mordida.sh:60`, `bin/tests/_mordida.sh:82`, `bin/tests/_mordida.sh:103`, `bin/tests/_mordida.sh:108`, `bin/tests/_mordida.sh:112`, `bin/tests/_mordida.sh:120`) |
| `meta.out`, `falhas` | Cada meta sobrescreve a saída; `_falha` incrementa o contador; o meta restaura o contador quando reconhece sua falha esperada. | Meta exige exatamente uma falha e o trecho esperado; conclusão do selo consulta `falhas`. (`bin/tests/_mordida.sh:273`, `bin/tests/_mordida.sh:274`, `bin/tests/test_disjuntor.sh:42`, `bin/tests/test_padrao_de_cima.sh:825`) |

**2. As sete classes**

| Classe | Resultado |
|---|---|
| **1. Segundo juiz registrado** | **Não identificado no diff.** As portas PDC continuam delegando a `estado_pdc/decide_pdc`; o vocabulário continua delegado a `padrao_vocab`. JUIZES declara que o registro vivo é `core/juizes.py`. (`bin/disjuntor.py:1509`, `bin/disjuntor.py:1552`, `bin/disjuntor.py:1565`, `bin/disjuntor.py:1344`, `app/docs/JUIZES.md:10`) |
| **2. Segundo escritor fora da porta** | **Nenhum novo identificado no instrumento.** Existe dívida anterior no RELATO: escritores diretos não compartilham a trava de `no_relato`. **Fila**, porque o diff não cria esses escritores. (`bin/disjuntor.py:236`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`) |
| **3. Relógio fora da borda** | **O diff não acrescenta leitura de relógio.** `hist` usa o `agora` da decisão. Permanecem leituras internas anteriores em `decide_pdc` e `trilha`; eventual centralização é **fila**, sem regressão temporal nova demonstrada. (`bin/disjuntor.py:1448`, `bin/disjuntor.py:1364`, `bin/disjuntor.py:140`) |
| **4. Derivação paralela da fonte única** | **Incide no efeito:** o escopo derivado pode ficar antigo e virar prova independente de liberdade quando o estado não se lê. **Bloqueia**, pelo caminho descrito abaixo. (`bin/disjuntor.py:1488`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1530`) |
| **5. Selo/RED sem mordida** | **Sim.** Há mortes indevidas aceitas como mordida e falta prova específica da migração de `res=2`. **Bloqueia.** (`bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:260`, `bin/tests/test_padrao_de_cima.sh:554`, `bin/tests/test_padrao_de_cima.sh:559`) |
| **6. Ordem de trava** | **Inversão nova não demonstrada.** O escopo continua gravado sob a trava do estado. Há, porém, nova possibilidade de leitura bloqueante enquanto essa trava está tomada: **bloqueia por espera**, não por inversão comprovada. (`bin/disjuntor.py:293`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1247`) |
| **7. Pouso antes do pré-requisito** | **[não verificado]**. O dado não prova sequência de suite, leitura final e pouso destes commits. A exigência está escrita, mas não substitui evidência de execução. (`CLAUDE.md:549`, `CLAUDE.md:553`) | [nao verificado]

**3. Como o harness decide “mordeu”, e onde aceita prova errada**

A decisão implementada exige: rc não zero; último comando aceito como assert ou marca do coletor com sequência correspondente; três arquivos de base existentes; nenhuma quantidade adicional de processos sem fechamento; nenhuma exceção escapada adicional, descontadas as declaradas; e, havendo declaração, trecho único, existente no original, ausente dos textos novos e observado em função/linha correspondentes. (`bin/tests/_mordida.sh:188`, `bin/tests/_mordida.sh:190`, `bin/tests/_mordida.sh:196`, `bin/tests/_mordida.sh:225`, `bin/tests/_mordida.sh:242`, `bin/tests/_mordida.sh:248`, `bin/tests/_mordida.sh:252`, `bin/tests/_mordida.sh:257`, `bin/tests/_mordida.sh:260`)

**Bloqueia — um mutante pode ser aceito sem o módulo sequer executar.** Caminho concreto:

1. Cadastrar mutante que troque `PDC_MINIMO = 2 ` por texto sintaticamente inválido, como `PDC_MINIMO = (`. O mutador verifica âncora, mas não compila o resultado. (`bin/disjuntor.py:1100`, `bin/tests/_mordida.sh:164`, `bin/tests/_mordida.sh:167`)
2. A compilação falha antes de executar frame da cópia. A sonda só atribui assinatura a exceções observadas em frames da cópia; o hook não registra exceção sem essa assinatura. (`bin/tests/_mordida.sh:75`, `bin/tests/_mordida.sh:79`, `bin/tests/_mordida.sh:115`)
3. `_pp` retorna erro e `i14_mesma` executa `return 1`, aceito como assert. (`bin/tests/test_padrao_de_cima.sh:90`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:190`)
4. Com `FIM`, `.esc` vazio e declaração vazia, o comparador não exige outra prova e aceita a mordida. O caso detectou falha de compilação, não a retirada da costura pretendida. (`bin/tests/_mordida.sh:49`, `bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:234`, `bin/tests/_mordida.sh:260`, `bin/tests/_mordida.sh:265`)

**Bloqueia — `SystemExit` anterior à decisão também passa.** Inserir `raise SystemExit(1)` antes de `d = decide_pdc(...)` faz o mesmo caso executar `return 1`. `atexit` pode registrar `FIM`, enquanto a classificação de escape depende exclusivamente de `sys.excepthook`; não há verificação de saída antecipada em `.exc` quando não existe declaração. (`bin/disjuntor.py:1508`, `bin/disjuntor.py:1509`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:49`, `bin/tests/_mordida.sh:114`, `bin/tests/_mordida.sh:234`)

**Bloqueia — qualquer SIGTERM vira “prazo”.** Inserir `os.kill(os.getpid(), 15)` antes da decisão produz `SINAL`; `_pp` falha e o caso retorna `1`. `abertos` exclui esse processo dos sem fechamento, e o aumento de SIGTERM produz somente uma NOTA. Não exige que tenha havido `timeout`, nem confere a origem do sinal. (`bin/disjuntor.py:1509`, `bin/tests/_mordida.sh:55`, `bin/tests/test_padrao_de_cima.sh:90`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:228`, `bin/tests/_mordida.sh:265`)

**Bloqueia — a cura do legado `res=2` não tem mordida própria.** A versão `3` existe expressamente para reparar eventos vazios classificados pelo resolvedor `2`; porém, a fixture remove `res`, e a asserção passou a aceitar qualquer versão numérica. Reverter somente a constante para `2` mantém esse teste de legado passando, enquanto eventos reais `entidades=[]/res=2` voltam a ser pulados pela igualdade da versão. (`bin/disjuntor.py:1102`, `bin/disjuntor.py:1387`, `bin/tests/test_padrao_de_cima.sh:554`, `bin/tests/test_padrao_de_cima.sh:559`)

As alterações específicas de **P17** e **`escopo-fail-open`** têm caminhos coerentes de detecção: P17 exige stdout vazio **e nenhuma FALHA nova**, chegando ao `return 35`; OBRA-Z, ausente do escopo e presente apenas no RELATO novo, chega ao `return 6` se a guarda for retirada. Isso não elimina os contraexemplos do harness acima. (`bin/tests/test_disjuntor.sh:100`, `bin/tests/test_disjuntor.sh:103`, `bin/tests/test_disjuntor.sh:583`, `bin/codex_limite_estado.py:156`, `bin/disjuntor.py:2076`, `bin/tests/test_padrao_de_cima.sh:500`, `bin/tests/test_padrao_de_cima.sh:503`, `bin/tests/test_padrao_de_cima.sh:709`)

**4. Outros achados, com classificação**

- **Bloqueia — escopo antigo libera histórico que não conhece.** Existe escopo sem OBRA-A → a decisão registra evento de OBRA-A e salva o estado → salvar o escopo falha, conservando o antigo → a linha sai do RELATO → o estado fica ilegível → fallback aceita escopo antigo mais RELATO neutro e retorna `LIVRE`. O novo histórico só protege quando sua gravação funciona; não existe vínculo de geração conferido entre os dois arquivos. (`bin/disjuntor.py:1397`, `bin/disjuntor.py:281`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:1530`)

- **Bloqueia — FIFO/dispositivo pode prender a decisão sob `flock`.** `_existe` aceita qualquer objeto existente, mas os leitores seguintes usam `open().read()` comum. FIFO sem escritor em `models.py` ou no arquivo citado não levanta imediatamente: pode esperar mantendo a trava do estado. O wrapper chama as guardas antes do `timeout` que envolve o Codex. (`bin/disjuntor.py:1230`, `bin/disjuntor.py:1247`, `bin/disjuntor.py:1273`, `bin/disjuntor.py:275`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:177`)

- **Bloqueia — payload cru ignora a chave DESLIGADO.** Com a chave presente, `["Agent", "construir a obra OBRA-C agora"]` levanta antes da consulta à chave e chega à negativa do envelope. Contraria “existindo, nenhum disjuntor age”; o contrato autoriza a chave de desligar. (`bin/disjuntor.py:40`, `bin/disjuntor.py:2044`, `bin/disjuntor.py:2048`, `bin/disjuntor.py:2067`, `bin/disjuntor.py:2074`, `app/docs/CORTES.md:407`)

- **Fila — classificação do cru continua incompleta.** `{"tool_name":"Agent","tool_input":["construir a obra OBRA-C agora"]}` passa pelo teste de objeto externo, falha nos `.get` internos e também faz `_ramo_pdc` devolver `None`, terminando liberado. Uma lista com `"\u0041gent"` igualmente escapa da busca literal por `Agent`. A ocorrência desses formatos no produtor real é **[não verificado]**. (`bin/disjuntor.py:2043`, `bin/disjuntor.py:2005`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1590`, `bin/disjuntor.py:2076`)

- **Fila — raiz do RELATO pode divergir da raiz do estado.** `decide_pdc(raiz=X)` usa X para estado/modelos/escopo, mas `_relato_consistente()` usa `hook_stop_fila1.RELATO`, fixado no import pelo ambiente ou localização do módulo. Processo reutilizado com outra raiz pode cruzar fontes; as invocações CLI dos selos evitam isso ao fornecer `HOOK_STOP_RAIZ`. (`bin/disjuntor.py:1365`, `bin/disjuntor.py:1371`, `bin/disjuntor.py:707`, `bin/disjuntor.py:1523`, `bin/hook_stop_fila1.py:30`, `bin/hook_stop_fila1.py:34`, `bin/tests/test_padrao_de_cima.sh:90`)

- **Fila — temporário órfão do escopo.** Depois de `mkstemp`, falhas de escrita ou substituição só geram trilha; falta limpeza do temporário. O escritor do estado já possui essa limpeza. (`bin/disjuntor.py:1484`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:284`)

- **Fila — vermelho por privilégio do executor.** `resolvedor_fonte` presume que `chmod 000` retire acesso. Executor com privilégio suficiente pode continuar lendo e reprovar o controle por ambiente, sem relação com mutante. (`bin/tests/test_padrao_de_cima.sh:534`, `bin/tests/test_padrao_de_cima.sh:537`, `bin/tests/test_padrao_de_cima.sh:570`, `bin/tests/test_padrao_de_cima.sh:573`)

- **Fila — instalação da sonda não tem confirmação positiva.** Depende de `sys.monitoring` e disponibilidade do identificador `4`; stderr é descartado e o controle procura ausência de erros, sem exigir confirmação de monitoramento instalado. Os metas podem denunciar a falha, portanto isso sozinho não prova verde global. (`bin/tests/_mordida.sh:64`, `bin/tests/_mordida.sh:108`, `bin/tests/_mordida.sh:143`, `bin/tests/_mordida.sh:176`, `bin/tests/test_padrao_de_cima.sh:755`)

- **Fila — bases não certific

[truncado em 20000 de 21845 caracteres]

## 31/10 20:57 · sessao b2d04db14ac0 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**A cura de 1914 procede, mas o corpo ainda não sustenta o aval geral: o piso introduz uma regressão com segundos, mesmo sem troca de offset.** O `max` aplica um minuto mínimo também quando a próxima hora já é posterior a `a`; isso pode ultrapassar a borda e transformar um trecho parcialmente noturno em inteiramente noturno. (`app/ponto/motor_calculo_v2.py:183–187`)

1. **Contraexemplo e limites**

Reproduzi em memória pelas funções reais da cópia, com `America/Sao_Paulo`, configurado em `app/config/settings/base.py:93`:

| Parâmetro | Valor |
|---|---|
| Entrada | 14/10/2026 22:00 |
| Saída | 15/10/2026 07:00 |
| Intervalo | 15/10/2026 04:59:30 → 05:01:00 |
| Prorrogação | `False` |
| Próxima hora localizada | 05:00:00 |
| Piso | **05:00:30**, escolhido como `b` |
| Desconto atual | **1 minuto** |
| Janela efetiva | **0,5 minuto** |
| Corpo anterior, representado pelo mutante | **0,5 minuto** |

O cursor escolhe `05:00:30`; entre `a` e `b`, o prefixo aumenta apenas 0,5 minuto, mas o corpo soma a duração inteira de 1 minuto. O restante, até `05:01`, não aumenta o prefixo. A borda continua sendo 05:00 na função consultada. (`app/ponto/motor_calculo_v2.py:101`, `:184–188`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:426–431`)

Também reproduzi o erro com as mesmas pontas fornecidas como **naive**, **UTC** e **Asia/Tokyo**: desconto de 1 minuto. As representações convergem pelo `localizar`; portanto, outro fuso de entrada não elimina o defeito. (`app/ponto/motor_calculo_v2.py:77–79`, `:162–164`, `:184–187`)

Há contraprova equivalente na entrada da janela: recorte começando em `21:59:30`, pausa `21:59:30–22:01`, sem prorrogação, produz **1,5 minuto**, contra **1 minuto** da janela. (`app/ponto/motor_calculo_v2.py:103`, `:184–187`)

Quanto às outras propriedades:

- **`b <= a`: NÃO procede**, considerando operações sem exceção. Dentro do laço, `_f > a`; o outro argumento do `min` é pelo menos `a + 1 minuto`. Ambos são posteriores a `a`, e a atualização faz progresso. (`app/ponto/motor_calculo_v2.py:179`, `:184`, `:188`)
- **Desconto negativo ou soma devolvida maior que `minutos_noturnos(entrada, saida)`: NÃO procede.** Só são somadas durações positivas, e o retorno aplica o teto do total noturno. (`app/ponto/motor_calculo_v2.py:176`, `:185–190`)
- **Excesso material sobre a duração da união: NÃO procede.** As peças são unidas e cada avanço é contado no máximo uma vez; isso não garante correspondência com a janela, como demonstra o contraexemplo. (`app/ponto/motor_calculo_v2.py:164–172`, `:177–188`)
- **Desigualdade literal em `float`: exige ressalva.** Reproduzi novamente `60.41666666666667` contra duração direta `60.416666666666664`, diferença de aproximadamente `7,1e-15` minuto, abaixo da tolerância dos testes. (`app/ponto/motor_calculo_v2.py:187`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119–125`, `:185`)

Nos casos sondados, o histórico de 1914 passou a **354 minutos**, conforme a janela; os dois casos novos de verão deram **270** e **450**. Também sondei o início ambíguo `17/02/2018 23:30` nas duas ocorrências: **330** e **390**, ambos iguais à diferença dos prefixos. Isso confirma esses casos, sem certificar universalmente todas as transições. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:396–405`; `app/ponto/motor_calculo_v2.py:79`, `:183–190`)

2. **O piso pode ser escolhido?**

**Sim; a alegação de que ele só impede localização sem avanço NÃO procede.** No contraexemplo, `05:00` já avança 30 segundos, mas perde para o piso `05:00:30`, que atravessa a borda. O erro é de **30 segundos**, não arredondamento residual. (`app/ponto/motor_calculo_v2.py:181–187`)

O piso também é escolhido com uma hora inexistente fornecida como naive: pausa `04/11/2018 00:00–06:00`. `localizar` aplica `localize` e retorna diretamente; nessa sonda o cursor avançou em **300 passos de um minuto**, mantendo o offset carregado por `a`. O resultado foi **240 minutos**, igual à janela nesse caso. Portanto, uso do piso não implica necessariamente erro, mas sua inocuidade geral NÃO procede. (`app/ponto/motor_calculo_v2.py:77–79`, `:183–188`)

3. **Identidade com e4ed207c e chamadores**

**Identidade em qualquer recorte normal sem troca de offset: NÃO procede.** O contraexemplo de outubro de 2026 já distingue os corpos: **1 contra 0,5 minuto**. Com offset constante e pontas alinhadas a minutos inteiros, o piso não ultrapassa a próxima hora, e o cursor preserva os cortes anteriores. (`app/ponto/motor_calculo_v2.py:183–184`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:426`)

Os chamadores continuam usando o mesmo caminho; seus resultados podem mudar pelo desconto recebido:

| Leitor | Uso do desconto |
|---|---|
| `he_noturna` | Subtrai no recorte ancorado em `inicio_extra`. (`app/ponto/motor_calculo_v2.py:434–437`) |
| `_noturno_dos_intervalos` / `calcular_noturnas` | Delega à função e subtrai do total. (`app/ponto/motor_calculo_v2.py:1909–1910`, `:965–969`) |
| `_ContextoNoturno` / noturnas do dia | Empresta o mesmo método e subtrai por par. (`app/ponto/calculador/regras.py:63`, `:85–89`) |
| HE por período | Passa pontas, intervalos e flag a `he_noturna`. (`app/ponto/calculador/regras.py:886–890`) |
| `noturno_no_par` | Encaminha um par e os parâmetros da CCT; o retorno alimenta os campos do período. (`app/ponto/calculador/periodo.py:508–511`, `:752–754`) |

Impacto produtivo por colaborador ou rubrica: **[nao verificado]**.

4. **Testes novos e mordida** [nao verificado]

**Mordem o defeito histórico: SIM.** Os três métodos novos passaram na execução isolada em memória; o caso de 1914 produziu **354** no corpo atual e **360** no mutante. O `mock.patch` troca o símbolo efetivamente consultado e chama diretamente o teste dentro de `assertRaises(AssertionError)`. Vacuidade dessa mordida NÃO procede. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:396–398`, `:436–441`)

**Valor copiado: NÃO procede.** A expectativa calcula duração absoluta até a borda local das 05h. É adequada aos três casos escolhidos, todos iniciados dentro da janela e todos com `pr=False`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:392`, `:398`, `:403`, `:405`)

**Proteção geral contra o piso: NÃO procede.** Os **24 testes puros** executados por AST passaram, incluindo os novos, enquanto o contraexemplo com segundos falhou na mesma abordagem. O teste existente de segundos cobre pausa inteiramente prorrogada; a grade usa minutos inteiros, e seu oráculo trunca frações de minuto. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119–125`, `:149`, `:153–154`, `:200–207`)

**Relógio solto: NÃO procede.** `_local` constrói datas literais e aplica o fuso, sem consultar “agora”. Suíte Django canônica e testes de integração dos chamadores: **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377–405`)

5. **Classes reprovadas e escritores/leitores**

**Segundo juiz, segundo escritor fora da porta e derivação paralela da janela: NÃO procedem neste delta.** A decisão continua consultando os prefixos da mesma função; as atribuições novas são locais. **Selo sem mordida também NÃO procede para 1914**; existe uma lacuna distinta de cobertura do piso com segundos. (`app/ponto/motor_calculo_v2.py:183–190`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:439–441`)

| Valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| Fuso | `settings.TIME_ZONE`, depois `TZ_LOCAL`. (`app/config/settings/base.py:93`; `app/ponto/motor_calculo_v2.py:28`) | Conversão, janela, cursor e fixtures. (`app/ponto/motor_calculo_v2.py:78–79`, `:100–104`, `:184`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:378–379`) |
| Pontas e intervalos | Chamadores fornecem os argumentos; o motor carimba os efetivos. (`app/ponto/motor_calculo_v2.py:436–437`, `:1822–1824`, `:1909–1910`) | Conversão/recorte e leitura por período. (`app/ponto/motor_calculo_v2.py:162–164`; `app/ponto/calculador/periodo.py:752`) |
| Flag de prorrogação | Atributo do motor, contexto e CCT. (`app/ponto/motor_calculo_v2.py:752–756`; `app/ponto/calculador/regras.py:61`; `app/ponto/calculador/nucleo.py:348`) | Prefixos, teto e delegações. (`app/ponto/motor_calculo_v2.py:185–190`, `:1910`; `app/ponto/calculador/periodo.py:509`) |
| `_ent`, `_sai`, `pecas`, `unidas` | Conversão, recorte e união. (`app/ponto/motor_calculo_v2.py:162–172`) | Laço, prefixos e teto. (`app/ponto/motor_calculo_v2.py:177–190`) |
| `a`, `hora_cheia`, `b` | Inicialização/avanço de `a`, construção da hora e escolha de `b`. (`app/ponto/motor_calculo_v2.py:178`, `:183–184`, `:188`) | Guarda, localização, prefixos, duração e próximo avanço. (`app/ponto/motor_calculo_v2.py:179–188`) |
| Prefixos e desconto | `minutos_noturnos` acumula/devolve os prefixos; o corpo acumula e limita o desconto. (`app/ponto/motor_calculo_v2.py:96`, `:116–118`, `:176–190`) | Chamadores da tabela anterior. (`app/ponto/motor_calculo_v2.py:436`, `:967`, `:1909`; `app/ponto/calculador/regras.py:86`) |
| Resultados noturnos | Escritores existentes do motor, rubricas e período. (`app/ponto/motor_calculo_v2.py:986–987`; `app/ponto/calculador/regras.py:896–898`, `:912–914`; `app/ponto/calculador/periodo.py:753–754`) | Alteração desses destinos ou impacto persistido por esta obra: **[nao verificado]**. |
| Fixtures, `desc`, `esperado` e substituição do mutante | Literais/helper, chamada real, duração absoluta e `mock.patch`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377–405`, `:439`) | Asserções e chamada direta sob o mutante. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:393–394`, `:440–441`) |

Ordem de trava, execução histórica do RED, pré-requisitos de pouso e pouso efetivo: **[nao verificado]**. [nao verificado]

## 31/10 20:56 · sessao 07231a507e95 · EMIS-LEITORES-RESTANTES · censo · origem=Code

O ponto central é: **`trabalha=None` não dispensa cobrança por si só**. O juiz também considera `ata.tipo_dia`; portanto, migrar apenas comparações de `trabalha` pode mudar tanto o limbo quanto dias com ata de outro tipo. A distinção entre cobrança e previsão está expressa no código e na tabela de autoridades. (`app/chamados/juizes.py:398`, `app/chamados/juizes.py:408`, `app/docs/JUIZES.md:55`, `app/docs/JUIZES.md:74`)

O anexo fornecido contém a tabela de autoridades, sem linhas de células, chamados ou perguntas. Quantidades afetadas, ocorrência das combinações abaixo e impacto atual em produção: **[nao verificado]**.

**1. O que o juiz efetivamente responde**

O predicado inteiro é:

```python
ata = cel.ata or {}
return cel.trabalha is False or bool(
    ata.get('tipo_dia') and ata.get('tipo_dia') != 'trabalho'
)
```

Fonte: `app/chamados/juizes.py:407`, `app/chamados/juizes.py:408`.

Na matriz, `True` significa **“dia sem trabalho”**. Todos os resultados decorrem desse predicado. (`app/chamados/juizes.py:408`)

| `cel.trabalha` | Ata inexistente, `{}`, chave ausente, `None` ou `''` | `tipo_dia='trabalho'` | `'folga'` | `'ausencia'`, `'ausente'`, `'falta'`, `'feriado'` ou qualquer outro texto preenchido |
|---|---:|---:|---:|---:|
| `True` | `False` | `False` | `True` | `True` |
| `False` | `True` | `True` | `True` | `True` |
| `None` | `False` | `False` | `True` | `True` |

O juiz não valida uma lista de tipos: qualquer valor verdadeiro diferente de `'trabalho'` satisfaz a segunda cláusula, inclusive o literal `'ausente'`. (`app/chamados/juizes.py:408`)

Feriado e ausência **não são consultados diretamente** nessa função. Só influenciam sua resposta se estiverem representados nos dois campos lidos. A supressão por feriado pertence à precedência; cobertura de ausência tem autoridade própria, chamada separadamente por `_celula_furada`. (`app/chamados/juizes.py:407`, `app/chamados/juizes.py:426`, `app/docs/JUIZES.md:47`, `app/docs/JUIZES.md:63`)

`cel=None` não é uma entrada tratada pelo juiz: a função acessa `cel.ata`. O consumidor `_celula_furada` trata a inexistência da célula antes de chamá-lo. (`app/chamados/juizes.py:407`, `app/chamados/juizes.py:437`, `app/chamados/juizes.py:443`)

Há duas alimentações distintas, relevantes para todo o censo:

| Alimentação | Comportamento |
|---|---|
| `EscalaColaborador.eh_dia_trabalho` | Havendo célula, devolve seu `trabalha` cru, sem ler ata; sem célula, usa cálculo do vínculo. (`app/escala/models.py:1066`, `app/escala/models.py:1072`, `app/escala/models.py:1073`) |
| `grade_da_celula` | Prefere `ata.tipo_dia`; sem tipo preenchido, transforma `True/False/None` em `trabalho/folga/indefinido`. (`app/escala/services/leitor_celula.py:345`) |
| Builder da grade | Pode representar previsão `None` como `tipo_dia='trabalho'` com `sem_ancora=True`; não é equivalente ao leitor lavrado. (`app/escala/utils.py:725`, `app/escala/utils.py:729`, `app/escala/utils.py:752`) |

**2. A — mesma pergunta: migrar a cláusula de dia**

A classificação abaixo alcança a cláusula que permite ou cala cobrança; os demais requisitos — vínculo, marcos, prazo, veredito, cobertura, horizonte — continuam sendo perguntas próprias. Isso é visível, por exemplo, no emissor retroativo e nos dois caminhos de emissão do cartório. (`app/ponto/management/commands/emitir_furo_retroativo.py:118`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/services/cartorio.py:1495`, `app/ponto/services/cartorio.py:1608`)

Os chamadores abaixo são referências **no código de produção**. Sua execução no ambiente real: **[nao verificado]**.

| Sítio e predicado literal | Pergunta efetiva; `None` versus juiz; mudança ao migrar | Chamadores de produção |
|---|---|---|
| `app/ponto/management/commands/emitir_furo_retroativo.py:127` — `ec.eh_dia_trabalho(data) is not True` | Cala emissão de furo. `None` é descartado; o juiz permite prosseguir sem ata de outro tipo. Também muda `True` com ata não trabalho: hoje passa, juiz cala. Passar pelo gate ainda exige horário e demais condições. (`:127`, `:130`, `:142`; `app/chamados/juizes.py:408`) | `Command.handle`; comando declarado “passe retroativo sob ordem”. (`app/config/crons.py:1339`) |
| `app/ponto/management/commands/processar_alertas_avancados.py:49` — `not ec.eh_dia_trabalho(ontem)` | Cala o caminho de alerta/cobrança do turno aberto. `None` cala; juiz não cala sem ata de outro tipo. `True` com ata não trabalho passa hoje e seria barrado. (`:49`, `:82`, `:121`; `app/chamados/juizes.py:408`) | `Command.handle:19`; cron declarado em `app/config/crons.py:344`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:76` — `filter(..., trabalha=True)` | Pré-seleciona células para emitir perguntas. Exclui `None`; juiz permitiria `None` sem ata não trabalho. Inclui `True` com ata não trabalho, que o juiz excluiria. Veredito e motivos continuam filtrando. (`:76`, `:111`, `:120`, `:123`) | `Command.handle:57`; encadeamento após apuração em `app/config/crons.py:1464`. |
| `app/ponto/services/vigia_de_hora.py:21` — `filter(..., trabalha=True)` | Pré-seleciona células que chegarão ao julgamento/emissão. `None` nunca chega; juiz permitiria sua avaliação sem ata não trabalho. `True` com ata não trabalho sairia da seleção. Isso não garante emissão: ainda há vencimento de marco e outros gates. (`:21`, `:113`, `:122`, `:130`) | `_candidatas` → `rodar:113`; comando `app/ponto/management/commands/vigia_de_hora.py:28`; cron `app/config/crons.py:348`. |
| `app/chamados/services/declaracao_texto.py:95` — `tipo_dia != 'trabalho'` | Descarta dia como “sem marco a cobrar”. Pelo leitor lavrado, `None` sem tipo vira `indefinido` e é descartado; juiz não dispensa. `False` com ata `trabalho` passa hoje, mas juiz barra. `True/None` com ata não trabalho continuam barrados. (`:90`, `:95`, `:96`; `app/escala/services/leitor_celula.py:345`) | `_complementar_marcos_faltantes`: `app/chamados/services/disputa_emissao.py:794`, emissão em `:813`; `_ata_do_dia`: `:827`. Também há consumidores de apresentação em `declaracao_texto.py:285`, `:411`, `:524`, `:563`. |
| `app/ponto/services/cartorio.py:1504` — `ec is not None and ec.eh_dia_trabalho(data) is True` | Elegibilidade de emissão do furo total. `None` cala; juiz não cala sem ata de outro tipo. `True` com ata não trabalho seria barrado. Vínculo e marco inicial permanecem requisitos separados. (`:1503`, `:1504`, `:1507`, `:1514`) | Corpo de `julgar_colab`: `:777`, `:802`; lote `app/ponto/management/commands/processar_cartorio.py:65`; evento `cartorio.py:1723`. |
| `app/ponto/services/cartorio.py:1608` — `ec is None or ec.eh_dia_trabalho(data) is not True` | Faz `_marcos_do_furo_parcial` devolver `None`, impedindo a emissão parcial. Mesmas diferenças para `None` e `True` com ata não trabalho. O restante identifica os marcos apagados e sua âncora. (`:1608`, `:1611`, `:1624`, `:1635`) | Chamado em `cartorio.py:1512`, `:1543`; emissão em `:1550`. Mesmas entradas lote/evento da linha anterior. |
| `app/ponto/marcos.py:159` — `trabalha is False` | Cláusula `'FOLGA'` de `celula_veta_cobranca`. `None` não veta, igual ao juiz sem ata não trabalho. Como não lê ata, migrar acrescenta veto para `True/None` com ata não trabalho; outros vetos de veredito permanecem. (`:155`, `:159`, `:161`) | `detectar_ausencias.py:626`; `reconciliar_grade.py:502`; `app/chamados/reconciliador.py:842`; contador `app/ponto/services/furos_sem_cobranca.py:98`. |
| `app/escala/servico_jornada.py:303` — `_cel_dia is not None and _cel_dia.trabalha is False` | Primeira cláusula de dispensa dentro de `classificar_falta`. `None` não dispensa, igual ao juiz sem ata de outro tipo. Migrar acrescenta a cláusula da ata para `True/None`; não substitui o juiz inteiro de “qual furo existe?”. (`:279`, `:303`; `app/docs/JUIZES.md:81`) | Fábrica `app/chamados/services/fabrica_da_celula.py:56`; disputa `app/chamados/services/disputa_emissao.py:739`, `:1003`, `:1036`; `motivo_da_lei` em `servico_jornada.py:557`; retratação em `app/ponto/marcos.py:170`. |
| `app/ponto/management/commands/detectar_ausencias.py:305` — `not resultado_dia`, obtido em `:143` | Cala o caminho de cobrança por dia não previsto. **`None` já foi desviado antes**, pelo tratamento de limbo em `:159` e pelo `continue` em `:304`; trocar só `:305` não o faz chegar à cobrança. Para `True` que chega ao ramo, passar a ler ata não trabalho muda o gate. (`:143`, `:159`, `:304`, `:305`, `:439`) | `Command.handle:61`; cron `app/config/crons.py:345`. |
| `app/ponto/services/faltas_de_hoje.py:40` — `not (ec.eh_dia_trabalho(hoje) is True or limbo)` | Gate do contador de falta atual sem cobrança. `None` só passa se `limbo` for verdadeiro; juiz não dispensa `None` sem ata não trabalho independentemente desse defeito específico. Migrar pode mudar a população contada; não abre chamado diretamente. (`:13`, `:39`, `:40`, `:69`) | `app/ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:376` — `c.veredito in acusam or not c.trabalha or k in com_batida` | Na contraprova do piso, a cláusula `not c.trabalha` dispensa também `None`; juiz não dispensa sem ata de outro tipo. Migrar **só essa cláusula** muda a auditoria quando os outros termos não bastam para excluir. (`:305`, `:308`, `:376`) | `contraprova_piso` chamada em `:760`; cron auditor `app/config/crons.py:523`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1443` — `not _cel4.trabalha or _cel4.veredito not in _ACUSAM_a4` | Decide se a célula sustenta cobrança/reabertura. `None` é dispensado mesmo com veredito acusador; juiz sem ata não trabalho não o dispensa. Outros gates, inclusive ausência de julgamento, permanecem. (`:1433`, `:1434`, `:1443`) | Corpo de `Command.handle:552`; cron `app/config/crons.py:523`. |

**Migrar esses gates não é uma alteração neutra.** Além do limbo, existe a entrada `ata.tipo_dia='ausencia'` com cobertura parcial: a grade conserva esse tipo e os marcos, o suprajuiz admite cobrança parcial e `classificar_falta` só dispensa ausência que não seja parcial. O juiz `dia_sem_trabalho`, por sua vez, dispensa qualquer ata preenchida diferente de `trabalho`. Essa diferença precisa aparecer no eventual diff de comportamento; sua ocorrência no dado atual é **[nao verificado]**. (`app/escala/utils.py:1180`, `app/escala/utils.py:1190`, `app/ponto/supra_juiz.py:304`, `app/escala/servico_jornada.py:344`, `app/chamados/juizes.py:408`)

No cartório, os números antigos fornecidos caem hoje em comentários; os gates efetivos correspondentes estão em `:1504` e `:1608`. (`app/ponto/services/cartorio.py:1459`, `app/ponto/services/cartorio.py:1469`, `app/ponto/services/cartorio.py:1558`, `app/ponto/services/cartorio.py:1574`)

**3. B — pergunta diferente: não migrar ao juiz de cobrança**

A tabela de autoridades distingue previsão, precedência, minutos previstos, furo e status lavrado. Uma diferença para `None` nesses domínios não autoriza substituí-los por `dia_sem_trabalho`. (`app/docs/JUIZES.md:74`, `app/docs/JUIZES.md:76`, `app/docs/JUIZES.md:80`, `app/docs/JUIZES.md:81`, `app/docs/JUIZES.md:82`)

| Sítio e predicado literal | Pergunta real; `None`; efeito de uma troca mecânica | Chamadores / autoridade pertinente |
|---|---|---|
| `app/ponto/services/flip_auto.py:208` — `cel is not None and cel.trabalha is False`; `:212` — `v is None or v.trabalha is not True` | Escolhe dia/âncora para etiquetar E/S. `None` no dia atual não entra no ramo de folga; `None` na véspera não serve de âncora. Trocar pelo complemento do juiz permitiria véspera indeterminada sem ata contrária e poderia mudar flips. (`:165`, `:171`, `:176`, `:208`, `:212`) | `avaliar_fila:310`; comando `app/ponto/management/commands/flip_automatico.py:32`; tela `app/ponto/views.py:3295`. Autoridades de turno/marcos: `app/docs/JUIZES.md:22`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1542` — `_cel is None or _cel.trabalha is not True` | C5: célula contradiz `FolgaDia`? Só `True` prova a contradição; `None` não prova trabalho. Trocar pelo complemento do juiz acusaria limbo como contradição. (`:1530`, `:1542`, `:1549`) | `Command.handle:552`; cron `app/config/crons.py:523`. Juiz específico dessa contradição em JUIZES: **[nao verificado]**. |
| `app/ponto/services/cartorio.py:1714` — `data == hoje and cel.trabalha is not False and not teto_de_hoje` | Decide **quando aguardar o fim do turno para julgar hoje**. `None` aguarda. Usar o juiz poderia antecipar julgamento de `True/None` com ata não trabalho. (`:1714`, `:1715`, `:1719`) | Sinais `app/ponto/signals.py:93`, `app/chamados/signals.py:180`; vigia `app/ponto/services/vigia_de_hora.py:130`; reconciliador `app/chamados/reconciliador.py:675`. Juiz dessa guarda específica em JUIZES: **[nao verificado]**. |
| `app/ponto/services/dia_do_colab.py:121` — `not cel.trabalha` | Produz a frase “Dia sem trabalho previsto”. `None` recebe essa frase, embora o juiz não dispense cobrança sem ata contrária. Trocar muda apresentação, não o estado da cobrança. (`:121`, `:122`) | Chamada interna `:100`; API `app/api/views_mensageria.py:1963`; ficha `app/colaboradores/services/ficha.py:280`. Previsão/status: `app/docs/JUIZES.md:74`, `:82`. |
| `app/ponto/services/dia_do_colab.py:130` — `f.get('faixa') in (...) and cel.trabalha` | Apresenta divergência entre precedência e célula. `None` não prova essa divergência; o juiz responde outra coisa. (`:130`, `:131`, `:132`) | Mesma cadeia anterior; precedência: `app/docs/JUIZES.md:76`. |
| `app/relatorios/dia_da_frota.py:47` — `not cel.trabalha` | Conta folga e exclui do relatório de acusados. `None` conta folga; o juiz o manteria cobrável sem ata contrária. Trocar muda contagem/listagem. (`:39`, `:47`, `:48`, `:55`) | `app/api/views_mensageria.py:1814`. Previsão/status: `app/docs/JUIZES.md:74`, `:82`. |
| `app/chamados/services/fila_por_causa.py:192` — `filter(..., trabalha=True, ...)` | Seleciona **vizinhos previstos** para diagnosticar causa/adesão. `None` fica fora; usar o juiz mudaria os vizinhos e a gaveta atribuída. (`:192`, `:194`, `:224`, `:244`) | `quadro:271`, `toques:308`, `bloqueios_para_fechar:353`; API `app/api/views_mensageria.py:1582`; badge `app/core/management/commands/lavrar_badge_navbar.py:63`; comando `app/chamados/management/commands/fila_por_causa.py:13`. Previsão: `app/docs/JUIZES.md:74`. |
| `app/chamados/services/disputa_emissao.py:68` — `dia.get('data') == ref and dia.get('tipo_dia') == 'trabalho'` | Deriva uma **data confiável** quando o texto da pergunta não tem dia. `None` sem tipo na ata vira `indefinido` e não fornece data; cobrar o limbo não prova essa âncora. (`:42`, `:62`, `:68`, `:322`) | `gerar_perguntas_disputa`, chamada em `:322`; sem data, pergunta não nasce em `:332`. Juiz específico da inferência de data: **[nao verificado]**. |
| `app/chamados/services/disputa_emissao.py:753` — `_ec is not None and _ec.eh_dia_trabalho(data) is not False` | `_tem_base`: existe batida real **ou obrigação prevista** para o fallback? `None` com vínculo já passa; juiz também não dispensa sem ata contrária. Trocar acrescentaria a ata e mudaria a pergunta de base prevista. (`:742`, `:750`, `:753`, `:1024`) | Fallback de perguntas em `:1034`; classificação do furo ocorre depois, em `:1036`. Previsão/furo: `app/docs/JUIZES.md:74`, `:81`. |
| `app/chamados/detector_anomalias.py:164` — `eh_dia = esc.eh_dia_trabalho(...)`; `:165` — `eh_dia is None`; `:168` — `eh_dia is False` | Compara batida com escala: indeterminado não acusa; folga conhecida pode gerar anomalia. Trocar pelo juiz faria ata de ausência participar de “batida fora da escala”. (`:124`, `:164`, `:165`, `:168`, `:182`) | `app/api/views.py:730`; `app/api/views_core.py:727`. Previsão/turno: `app/docs/JUIZES.md:22`, `:74`. |
| `app/ponto/worklist.py:19` — `dia['tipo_dia'] != 'trabalho'` | Desenha gap pela ata, devolvendo “sem classificar”. `None` sem tipo é excluído; usar o juiz mudaria apresentação/classificação. (`:8`, `:12`, `:19`, `:20`) | Chamada interna `:60`; tela `app/colaboradores/views.py:3021`; comando `app/ponto/management/commands/worklist_dp.py:13`. Juiz específico do gap: **[nao verificado]**. |
| `app/ponto/propositor.py:36` e `:210` — `not dia or dia.get('tipo_dia') != 'trabalho'` | Diagnostica degeneração e amostra deltas para suspeita de template. O resultado para `trabalha=None` depende do produtor do dict: leitor lavrado pode dar `indefinido`; builder pode dar trabalho com `sem_ancora`. Trocar muda propostas/amostra. (`:34`, `:201`; `app/escala/services/leitor_celula.py:345`; `app/escala/utils.py:752`) | `app/ponto/management/commands/propor_reorganizacao.py:67`, `:71`; `app/inteligencia/calculadores.py:325`. Juiz específico em JUIZES: **[nao verificado]**. |
| `app/ponto/services/simular_dia.py:12` — `dd.get('tipo_dia') != 'trabalho' or dd.get('orfas')` | Decide **dia perfeito para uma correção simulada**, também exigindo ausência de missing e tipos corretos. `None` indireto depende da grade, não só do campo. Trocar altera critério de perfeição. (`:5`, `:12`, `:18`, `:21`; `app/escala/utils.py:729`) | `app/ponto/services/flip_auto.py:303`, `:304`; `app/ponto/services/veredito_celula.py:219`, `:223`. Juiz de cobrança não responde perfeição. (`simular_dia.py:11`) |
| `app/ponto/services/veredito_celula.py:221` — `dd0.get('tipo_dia') != 'trabalho'` | Há trabalho previsto para materializar a decisão administrativa “trabalhou”? A recusa diz “nada a materializar”; não é silêncio de chamado. (`:206`, `:221`, `:222`) | `app/ponto/views.py:3304`; `app/ponto/services/declarar_dia.py:206`. Previsão: `app/docs/JUIZES.md:74`. |
| `app/escala/services/furos_diarios.py:39` — `not d or d.get('tipo_dia') != 'trabalho'` | Apura `% marcos furados / previstos` e a projeção `FuroDiario`. `None` via leitor sem tipo fica fora; migrar muda dias/denominador, não emite chamado diretamente. (`:2`, `:19`, `:39`, `:98`) | `apurar:89`; comando `app/escala/management/commands/apurar_furos_diarios.py:8`; pós-vínculo `app/colaboradores/services/pos_vinculo.py:52`. Juiz específico da métrica em JUIZES: **[nao verificado]**. |
| `app/relatorios/furos.py:151` — `d.get('tipo_dia') != 'trabalho'`; `:218`, `:237` — mesmo teste `or d.get('sem_ancora')` | Exibição, dias em branco e matriz do Raio-X da folha. `None` sem tipo é rotulado folga/excluído; trocar muda relatório e contagens. (`:152`, `:210`, `:218`, `:229`, `:237`) | Dataset: `app/relatorios/views.py:1541`; matriz: `app/folha/views.py:616`. Chamador de produção de `dias_100_em_branco`: **[nao verificado]**. |
| `app/ponto/services/vigia_entrada.py:79`, `:120` — `filter(..., trabalha=True)` | Mede padrões de lâmpadas para investigar entrada perdida versus adesão. `None` fica fora; migrar muda a população estatística. (`:11`, `:63`, `:79`, `:111`, `:120`) | `contador:99`; comando `app/ponto/management/commands/vigia_entrada_faltante.py:24`, `:39`; cron `app/config/crons.py:716`. |
| `app/ponto/services/esmeril_espelho.py:71` — `x.trabalha is False ...`; `:121`, `:123` — `x.trabalha and ...` | Identifica assinaturas de escala/cadastro errado. `None` não prova folga nem trabalho conhecido; o juiz ampliaria essas assinaturas indevidamente. (`:66`, `:71`, `:117`, `:121`, `:123`) | Chamada interna `:105`; `app/escala/services/escala_certa.py:27`; comando `app/ponto/management/commands/esmeril_espelho.py:16`. |
| `app/ponto/services/passivo_s84.py:96` — `TRABALHO if cel.trabalha else FOLGA` | Classifica batidas antigas; a classe pode alimentar seleção para retratação. `None` vira `FOLGA`, diferente do juiz; migrar mudaria tratamento de batidas, não apenas cobrança. (`:62`, `:96`, `:108`) | `app/ponto/management/commands/passivo_disputa_retro.py:70`; `app/ponto/management/commands/diff_passivo_s84.py:45`. Autoridade dessa classificação em JUIZES: **[nao verificado]**. |
| `app/ponto/management/commands/tripwire_celulas.py:338` — `Q(trabalha=False) \| Q(trabalha__isnull=True)`; `:342` — `ata.get('tipo_dia') 

[truncado em 20000 de 30842 caracteres]

## 31/10 20:56 · sessao b6ad06b587d6 · - · - · origem=Ronald (tmux)

**Bloqueia: o harness ainda aceita morte antes da decisão como mordida.** A prova positiva exige apenas `return N` do caso, e a prova de processo exige apenas fechamento por `atexit` ou classificação como SIGTERM; nenhuma prova exige que o código mutado tenha concluído a decisão. (`bin/tests/_mordida.sh:190`, `bin/tests/_mordida.sh:214`, `bin/tests/_mordida.sh:225`)

Três caminhos concretos:

1. **Mutante sintaticamente inválido:** trocar `PDC_MINIMO = 2 ` por `PDC_MINIMO = (` passa pelo mutador, que apenas conta âncoras e grava o texto; a compilação de `disjuntor.py` falha antes de qualquer frame da cópia executar; o Python encerra normalmente por `atexit`; `_pp` devolve 1 e `i14_mesma` executa seu `return 1`. Sem declaração, `.exc` não precisa conter nada e `.esc` vazio passa. (`bin/disjuntor.py:1100`, `bin/tests/_mordida.sh:162`, `bin/tests/_mordida.sh:167`, `bin/tests/_mordida.sh:79`, `bin/tests/_mordida.sh:49`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:234`, `bin/tests/_mordida.sh:260`)

2. **`raise SystemExit(1)` antes de `decide_pdc`:** `SystemExit` deixa executar `atexit`, mas não chama `sys.excepthook`; assim há INI/FIM e nenhuma exceção escapada registrada em `.esc`. O mesmo `return 1` do caso satisfaz a prova positiva. (`bin/disjuntor.py:1508`, `bin/tests/_mordida.sh:49`, `bin/tests/_mordida.sh:114`, `bin/tests/_mordida.sh:190`, `bin/tests/test_padrao_de_cima.sh:116`; comportamento do interpretador confirmado em experimento somente em memória, sem artefato: [não verificado])

3. **Autoenvio de SIGTERM antes da decisão:** o handler anota SINAL independentemente da origem; `abertos` exclui esse processo dos sem fechamento e emite somente uma nota de “prazo”. O comparador não exige `timeout`, rc 124 ou prova de prazo, e o caso pode terminar pelo mesmo `return 1`. (`bin/tests/_mordida.sh:55`, `bin/tests/_mordida.sh:219`, `bin/tests/_mordida.sh:228`, `bin/tests/_mordida.sh:265`, `bin/tests/test_padrao_de_cima.sh:116`)

A sonda original foi extraída e executada em memória com `open` substituído por coletores em RAM: SyntaxError produziu INI/FIM e logs de exceção vazios; SystemExit produziu INI/FIM e somente `.exc`; SIGTERM produziu INI/SINAL. Nenhum arquivo foi criado pelo experimento. [não verificado]

**Mapa dos estados do harness:**

| Estado | Quem escreve | Quem lê / multiplicidade |
|---|---|---|
| `sonda_exc/sitecustomize.py` | `source` do harness gera o arquivo. | Python descendente carrega via `PYTHONPATH`; uma geração por selo. (`bin/tests/_mordida.sh:35`, `bin/tests/_mordida.sh:37`, `bin/tests/_mordida.sh:143`) |
| `<raiz>.exc` | `_mordida_sondado` trunca; callbacks RAISE de todos os Python sondados acrescentam. | Controle procura cinco tipos; comparador verifica declarações. Há múltiplos escritores. (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:106`, `bin/tests/_mordida.sh:176`, `bin/tests/_mordida.sh:222`, `bin/tests/_mordida.sh:252`) |
| `<raiz>.esc` | Truncador e hooks dos Python descendentes. | Comparador de multiplicidade. Há múltiplos escritores. (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:117`, `bin/tests/_mordida.sh:223`, `bin/tests/_mordida.sh:258`) |
| `<raiz>.proc` | Truncador; startup, `atexit` e handler SIGTERM de cada Python. | `abertos` conta conjuntos de PIDs. Há múltiplos escritores. (`bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:46`, `bin/tests/_mordida.sh:50`, `bin/tests/_mordida.sh:56`, `bin/tests/_mordida.sh:214`) |
| `<raiz>.marca` | Truncador e três coletores Python; o trap novo mantém seus dados em variáveis. | `_mordida_morre` lê a última marca quando o último comando não é `return`. (`bin/tests/_mordida.sh:139`, `bin/tests/test_disjuntor.sh:776`, `bin/tests/test_disjuntor.sh:947`, `bin/tests/test_disjuntor.sh:1122`, `bin/tests/_mordida.sh:191`) |
| `base_<caso>.exc/.proc/.esc` | `_mordida_controle`, somente se cada arquivo ainda não existe. | Comparador de mutantes; reaproveitados entre raízes pelo nome do caso. (`bin/tests/_mordida.sh:179`, `bin/tests/_mordida.sh:196`, `bin/tests/_mordida.sh:225`, `bin/tests/_mordida.sh:258`, `bin/tests/test_disjuntor.sh:1499`, `bin/tests/test_padrao_de_cima.sh:648`) |
| `<raiz>.orig.d` / `.novo` | `_mordida_muta` remove/recria o original e grava os textos novos. | Verificador de declaração. (`bin/tests/_mordida.sh:152`, `bin/tests/_mordida.sh:159`, `bin/tests/_mordida.sh:168`, `bin/tests/_mordida.sh:230`, `bin/tests/_mordida.sh:246`) |
| `_MORDIDA_CASO/PID/SEQ/ULT/VAL/MARCAS` | Inicializador do sondado e trap DEBUG. | Trap e julgamento do último comando; estado global compartilhado dentro do selo. (`bin/tests/_mordida.sh:124`, `bin/tests/_mordida.sh:125`, `bin/tests/_mordida.sh:140`, `bin/tests/_mordida.sh:187`) |
| `MORDIDA_SEQ`, `MORDIDA_MARCAS`, `SELO_EXC_LOG/DIR`, `SELO_PROC_LOG`, `SELO_ESC_LOG`, `PYTHONPATH` | Trap exporta sequência; sondado fornece ambiente. | Sonda e coletores Python. (`bin/tests/_mordida.sh:126`, `bin/tests/_mordida.sh:142`, `bin/tests/_mordida.sh:41`, `bin/tests/test_disjuntor.sh:776`) |
| Handler SIGTERM / `sys.excepthook` | Sonda instala SIGTERM se encontrar default e substitui excepthook preservando o anterior. | Runtime Python chama os handlers; SIGTERM restaura default e reenvia sinal. (`bin/tests/_mordida.sh:58`, `bin/tests/_mordida.sh:60`, `bin/tests/_mordida.sh:112`, `bin/tests/_mordida.sh:120`) |
| `meta.out`, `falhas` | Cada meta sobrescreve `meta.out`; `_falha` incrementa; meta pode restaurar o contador. | Meta exige exatamente uma falha e trecho esperado. (`bin/tests/_mordida.sh:273`, `bin/tests/_mordida.sh:274`, `bin/tests/_mordida.sh:275`, `bin/tests/test_disjuntor.sh:42`) |

**Fila:**

- A sonda depende de `sys.monitoring` e do tool ID 4, sem confirmação de instalação bem-sucedida. Um erro no startup pode deixar logs vazios, enquanto stderr é descartado; controles usam procura negativa de exceções. Os metas podem denunciar essa ausência, portanto não afirmo verde global da suíte nessa condição. (`bin/tests/_mordida.sh:64`, `bin/tests/_mordida.sh:108`, `bin/tests/_mordida.sh:139`, `bin/tests/_mordida.sh:143`, `bin/tests/_mordida.sh:176`, `bin/tests/test_padrao_de_cima.sh:755`)

- As bases são copiadas mesmo quando o controle falha. O contador conserva a reprovação, então isso sozinho não gera verde global, mas a existência da base não prova controle válido. (`bin/tests/_mordida.sh:175`, `bin/tests/_mordida.sh:179`, `bin/tests/_mordida.sh:196`)

- `.proc` compara somente quantidades de processos abertos: um processo sem FIM no controle pode compensar outro processo diferente morto no mutante. A ocorrência em caso atual não está demonstrada. (`bin/tests/_mordida.sh:214`, `bin/tests/_mordida.sh:225`; ocorrência: [não verificado])

- Não há prazo externo no sondado: dependem dos timeouts escolhidos pelos próprios casos; uma mutação bloqueante fora desses pontos pode pendurar o selo. (`bin/tests/_mordida.sh:142`, `bin/tests/_mordida.sh:146`, `bin/tests/test_disjuntor.sh:579`, `bin/tests/test_disjuntor.sh:661`)

- Os loops atuais são sequenciais. Paralelizar casos dentro da mesma execução exigiria isolar `A/P/EST`, `_MORDIDA_*`, bases e `meta.out`; atualmente compartilham esses estados. Execuções independentes dos selos têm `mktemp -d` próprio. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:50`, `bin/tests/test_disjuntor.sh:1483`, `bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:35`, `bin/tests/test_padrao_de_cima.sh:635`, `bin/tests/_mordida.sh:140`, `bin/tests/_mordida.sh:273`)

- O rc de `_mordida_morre` não é seu veredito: caminhos de erro chamam `_falha` e depois `return`; o contador é a autoridade. Os chamadores atuais usam essa convenção, portanto não há mascaramento demonstrado pelo último `[ -z "$msg" ] || echo`. (`bin/tests/_mordida.sh:194`, `bin/tests/_mordida.sh:267`, `bin/tests/_mordida.sh:268`, `bin/tests/test_disjuntor.sh:42`, `bin/tests/test_disjuntor.sh:1502`)

- O trap avalia expressões aritméticas antes do `return`, mas filtra apenas seu texto externo; variáveis podem conter expressões com efeitos colaterais e ser avaliadas novamente pelo `return`. O caso atual `b3_limiar` usa `r1` obtido dos números de falha; efeito colateral nele não foi demonstrado. (`bin/tests/_mordida.sh:130`, `bin/tests/_mordida.sh:131`, `bin/tests/test_disjuntor.sh:824`, `bin/tests/test_disjuntor.sh:825`)

## 31/10 20:55 · sessao 90e4fc606f0d · - · - · origem=Ronald (tmux)

Os testes novos mordem o defeito histórico: executei os três métodos em memória, com AST real e `unittest`; passaram. No caso de 1914: corpo atual **354**, mutante **360**, expectativa **354 minutos**. O teste substitui o símbolo consumido e chama diretamente o método sob `assertRaises(AssertionError)`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:396`, `:408`, `:436`)

A expectativa vem da duração absoluta até a borda local das 05h, sem copiar valor numérico do motor. Os casos escolhidos começam dentro da janela e usam exclusivamente `pr=False`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:392`, `:398`, `:403`, `:405`)

**Relógio solto NÃO procede:** `_local` somente constrói a data literal e aplica `localize`; os testes têm datas fixas. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377`, `:397`, `:402`, `:404`)

**A proteção geral NÃO procede:** os 24 testes puros passaram, mas o mesmo processo reproduziu pausa `04:59:30–05:01`, sem prorrogação: corpo atual **1 minuto**, mutante anterior **0,5 minuto**. O único teste de segundos cobre pausa inteiramente prorrogada, longe da borda; não detecta essa regressão. (`app/ponto/motor_calculo_v2.py:184`, `:187`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119`)

A grade usa minutos inteiros e seu oráculo trunca segundos; não certifica o caso acima. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:89`, `:149`, `:153`)

Execução Django canônica e testes de integração dos chamadores: **[nao verificado]**. [nao verificado]

## 31/10 20:55 · sessao 061e807bef00 · - · - · origem=Ronald (tmux)

Os chamadores continuam consumindo a mesma função e a mesma flag; o delta do cursor propaga aos resultados sem nova regra nos chamadores. A âncora da HE continua sendo `inicio_extra`, calculada em `app/ponto/motor_calculo_v2.py:434`, com minuendo em `:435` e desconto em `:436-437`.

| Valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| `TZ_LOCAL` | `pytz.timezone(settings.TIME_ZONE)` em `app/ponto/motor_calculo_v2.py:28`; configuração copiada em `app/config/settings/base.py:93` | `localizar` em `app/ponto/motor_calculo_v2.py:78-79`; construção das janelas em `:100-104`; cursor novo em `:184`; `_local` dos testes em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:378-379` |
| `entrada`, `saida`, `intervalos`, `prorrogacao` | Argumentos da função em `app/ponto/motor_calculo_v2.py:121`; HE fornece `inicio_extra`, `fim`, intervalos e flag em `:436-437`; método fornece parâmetros e atributo em `:1909-1910` | Conversão e recorte em `:162-164`; prefixos em `:185-186`; teto em `:190` |
| `_ent`, `_sai` | `localizar` em `app/ponto/motor_calculo_v2.py:162` | Recorte em `:164`; `_ent` ancora os prefixos em `:185-186`; ambos delimitam o teto em `:190` |
| `pecas` | Gerador convertido em lista ordenada, `app/ponto/motor_calculo_v2.py:164` | União em `:166-172` |
| `unidas` | Lista em `app/ponto/motor_calculo_v2.py:165`; extensão em `:170`; inclusão em `:172` | Condição de união em `:169-170`; laço externo em `:177` |
| `_i`, `_f` | Desempacotamento em `app/ponto/motor_calculo_v2.py:166` e `:177` | Validação/união em `:167-172`; início de `a` em `:178`; limite do laço e de `b` em `:179,184` |
| `a` | Inicialização em `app/ponto/motor_calculo_v2.py:178`; atualização em `:188` | Guarda em `:179`; próxima hora em `:183`; piso em `:184`; prefixo em `:186`; duração em `:187` |
| `hora_cheia` | Parede naive arredondada, mais uma hora, `app/ponto/motor_calculo_v2.py:183` | Localização para candidato de `b` em `:184` |
| `b` | Mínimo entre fim da peça e máximo entre candidato localizado/piso em `app/ponto/motor_calculo_v2.py:184` | Prefixo em `:185`; duração em `:187`; atualização de `a` em `:188` |
| Valor de cada prefixo / total noturno | `minutos_noturnos` escreve `total` em `app/ponto/motor_calculo_v2.py:96,116` e devolve em `:118` | Presença noturna em `:185-186`; teto do desconto em `:190` |
| `total` do desconto | Zero em `app/ponto/motor_calculo_v2.py:176`; soma de durações em `:187` | Incremento em `:187`; retorno limitado em `:190` |

Os leitores e destinos imediatos são:

- `MotorBase._noturno_dos_intervalos` delega em `app/ponto/motor_calculo_v2.py:1909-1910`; `calcular_noturnas` subtrai em `:967`, aplica fator em `:968` e retorna em `:969`. `_escrever_noturnas` escreve os campos do período em `:986-987`.
- `_ContextoNoturno` escreve sua flag em `app/ponto/calculador/regras.py:61` e empresta **o mesmo objeto de método** em `:63`. `noturnas_do_dia` mede o total em `:85`, subtrai o desconto em `:86` e retorna relógio/legais em `:89`. `do_dia` grava rubrica e detalhes em `:912-914`.
- HE por período chama a função de módulo importada em `app/ponto/calculador/regras.py:868`, passando pontas, flag e intervalos em `:886-888`; soma em `:889-890` e grava rubricas em `:896,898`.
- `noturno_no_par` passa um único par, intervalos e flags da CCT em `app/ponto/calculador/periodo.py:508-510` e devolve `NoturnoV3` em `:511`. O leitor chama em `:752` e escreve os campos noturnos em `:753-754`.
- O motor carimba pontas efetivas e intervalos em `app/ponto/motor_calculo_v2.py:1822-1824`. O atributo de prorrogação é escrito em `:756`, com consulta da regra existente se omitido em `:752-755`; a CCT do calculador recebe esse atributo em `app/ponto/calculador/nucleo.py:348`.

**Segundo juiz / derivação paralela da janela: NÃO procede neste delta.** O cursor consulta a função existente para decidir presença e teto; não replica o predicado 22–05 nem a condição de prorrogação. Há, sim, dependência estrutural declarada de bordas em horas cheias, cuja validade cabe à análise dos casos de fuso. (`app/ponto/motor_calculo_v2.py:173-175,183-190`)

**Segundo escritor fora da porta: NÃO procede no delta.** As novas atribuições são locais (`hora_cheia`, `b`); o retorno segue pelos escritores já descritos. Não há escrita de campo ou persistência no corpo examinado. (`app/ponto/motor_calculo_v2.py:162-190`)

**Relógio lido fora da borda: NÃO procede no delta.** O cursor deriva seus valores de `a`; os novos testes constroem datas literais, sem consulta a “agora”. (`app/ponto/motor_calculo_v2.py:183-184`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377-405`)

Ordem de trava, pouso antes de pré-requisito, execução histórica do RED e impacto produtivo: `[nao verificado]`. [nao verificado]

## 31/10 20:54 · sessao 6aa0072937f2 · - · - · origem=Ronald (tmux)

Achados e caminhos, sem execução dos selos. [não verificado]

1. **Bloqueia — SIGTERM direto é aceito como “prazo”.** Um mutante que insira `os.kill(os.getpid(), 15)` antes de `ents = sorted(...)` em `estado_pdc` interrompe a decisão; o caso `i14_mesma` vê rc diferente de 3 e executa `return 1`. O handler registra `SINAL`, e o comparador exclui esse processo dos “sem FIM”, produzindo apenas uma NOTA de prazo; não exige `timeout` na origem do sinal. Com declaração vazia e sem exceção escapada, aceita a mordida. Referências: `bin/disjuntor.py:1531`; `bin/tests/test_padrao_de_cima.sh:90`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:55`; `bin/tests/_mordida.sh:219`; `bin/tests/_mordida.sh:228`; `bin/tests/_mordida.sh:265`.

2. **Bloqueia — `sys.exit(1)` também pode virar mordida sem decisão.** Plantado no mesmo ponto, faz `i14_mesma` terminar pelo `return 1`; a comprovação de encerramento verifica apenas `INI/FIM/SINAL`, enquanto a comprovação de exceção escapada depende de `sys.excepthook`. `SystemExit` executa `atexit` e dispensa o hook padrão [não verificado quanto à execução nesta cópia]; o código não tem tratamento próprio de `SystemExit`. Portanto o caminho permite `.proc` fechado, `.esc` vazio e assert positivo sem julgamento concluído. Referências: `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:49`; `bin/tests/_mordida.sh:114`; `bin/tests/_mordida.sh:214`; `bin/tests/_mordida.sh:258`.

3. **Fila, ou bloqueia se exigida mordida de cada cura — migração `res=2` não provada.** A versão mudou para 3 precisamente para reprocessar eventos vazios com `res=2`; o skip usa igualdade com a versão atual. O caso de legado remove completamente `res`, e a antiga verificação da versão foi relaxada para um glob que aceita qualquer versão iniciada por dígito. Reverter somente `PDC_RESOLVEDOR_V = 3` para 2 mantém esse caso passando e deixa a cura do legado `res=2` sem prova; não há mutante dessa constante na lista v6. Referências: `bin/disjuntor.py:1102`; `bin/disjuntor.py:1387`; `bin/tests/test_padrao_de_cima.sh:552`; `bin/tests/test_padrao_de_cima.sh:559`; `bin/tests/test_padrao_de_cima.sh:711`.

4. **Fila — controles podem ficar vermelhos por privilégio do executor.** `resolvedor_fonte` presume que `chmod 000` torne arquivo/diretório ilegível, sem verificar a capacidade efetiva de leitura antes da asserção. Um executor que preserve acesso retorna resultado diferente de NAO_SEI e reprova o controle por ambiente, não pela mutação. Referências: `bin/tests/test_padrao_de_cima.sh:534`; `bin/tests/test_padrao_de_cima.sh:537`; `bin/tests/test_padrao_de_cima.sh:570`; `bin/tests/test_padrao_de_cima.sh:573`; privilégio efetivo nesta cópia [não verificado].

5. **Fila — casos não são isolados para execução paralela no mesmo selo.** Os loops atuais são sequenciais e cada processo de selo cria seu próprio `_tmp`; portanto não há corrida entre casos nesses loops. Entretanto casos invocados paralelamente dentro da mesma instância compartilhariam `A/EST`, `_arvore` apagaria a árvore alheia, e metas compartilhariam `meta.out` e o contador `falhas`. Referências: `bin/tests/test_padrao_de_cima.sh:29`; `bin/tests/test_padrao_de_cima.sh:35`; `bin/tests/test_padrao_de_cima.sh:39`; `bin/tests/test_padrao_de_cima.sh:635`; `bin/tests/test_disjuntor.sh:39`; `bin/tests/test_disjuntor.sh:54`; `bin/tests/test_disjuntor.sh:1483`; `bin/tests/_mordida.sh:273`.

As duas mudanças de selo que investiguei especificamente têm caminho coerente de detecção:

- **P17 novo:** JSON inválido sobe de `leituras_semanais` até `main_pretool`, que registra FALHA para `CODEX_PAPEL=censo`; `_passa` exige stdout vazio **e** nenhuma FALHA nova, então o caso executa `return 35`. Referências: `bin/codex_limite_estado.py:153`; `bin/codex_limite_estado.py:156`; `bin/disjuntor.py:1899`; `bin/disjuntor.py:1918`; `bin/disjuntor.py:2076`; `bin/tests/test_disjuntor.sh:100`; `bin/tests/test_disjuntor.sh:103`; `bin/tests/test_disjuntor.sh:583`.
- **`escopo-fail-open` agora em `escopo_duvida`:** OBRA-Z aparece somente no RELATO novo, após corromper o estado; o escopo anterior não a contém. Tirar `obra in com_evento` libera OBRA-Z, e o caso executa `return 6`. Referências: `bin/tests/test_padrao_de_cima.sh:500`; `bin/tests/test_padrao_de_cima.sh:503`; `bin/tests/test_padrao_de_cima.sh:709`; `bin/disjuntor.py:1528`.

## 31/10 20:54 · sessao 38e85514b6af · - · - · origem=Ronald (tmux)

Achados prioritários:

1. **BLOQUEIA — `_existe` permite uma espera sem prazo, sob a trava do estado.** `lstat` aceita FIFO/dispositivo; o leitor seguinte usa `open(...).read()` comum. Um FIFO em `app/outra/models.py` ou na fonte citada prende o processo sem chegar a `NAO_SEI`; enquanto isso, `_com_estado` conserva o `flock`. O wrapper também chama essas guardas sem `timeout`; o prazo começa apenas no comando Codex posterior. (`bin/disjuntor.py:1225`, `bin/disjuntor.py:1247`, `bin/disjuntor.py:1273`, `bin/disjuntor.py:260`, `bin/disjuntor.py:275`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:177`.)

2. **BLOQUEIA — escopo velho ainda pode autorizar uma obra com histórico.** Caminho: existe escopo antigo sem OBRA-A; `decide_pdc` incorpora evento de OBRA-A e grava o estado; a gravação do novo escopo falha, mas a exceção é engolida e o arquivo antigo permanece; a linha depois sai do RELATO; o estado fica ilegível; `estado_pdc` aceita o escopo antigo e RELATO neutro e retorna `LIVRE`. O campo `em` é escrito, mas não é usado na validação/fallback. Isso preserva o falso livre que a alteração v6 pretende eliminar. (`bin/disjuntor.py:1396`, `bin/disjuntor.py:281`, `bin/disjuntor.py:293`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1480`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1512`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:1530`.)

3. **BLOQUEIA — o novo caminho de payload cru ignora a chave DESLIGADO.** Com a chave existente, `["Agent", "construir a obra OBRA-C agora"]` provoca o `ValueError` antes da consulta a `desligado()`; o envelope cru identifica a obra e emite `deny` sem consultar a chave. O JSON truncado equivalente produz a mesma recusa. Isso contraria a definição “existindo, nenhum disjuntor age”. Os testes de chave usam payload válido, e os novos testes crus não combinam chave ligada. (`bin/disjuntor.py:40`, `bin/disjuntor.py:151`, `bin/disjuntor.py:2042`, `bin/disjuntor.py:2044`, `bin/disjuntor.py:2048`, `bin/disjuntor.py:1583`, `bin/disjuntor.py:2067`, `bin/disjuntor.py:2074`, `bin/tests/test_padrao_de_cima.sh:309`, `bin/tests/test_padrao_de_cima.sh:624`.)

4. **FILA — parâmetro `raiz` não governa o RELATO.** `decide_pdc(raiz=X)` usa X para estado, modelos, vocabulário e escopo, mas `_relato_consistente()` lê `hook_stop_fila1.RELATO`, determinado no import por `HOOK_STOP_RAIZ` ou pela localização desse módulo. O mesmo erro existe na releitura do fallback. Uma chamada em processo reutilizado com outra raiz pode cruzar estado de X e RELATO de Y; os chamadores CLI usuais que fixam o ambiente evitam esse cruzamento. (`bin/disjuntor.py:1365`, `bin/disjuntor.py:1367`, `bin/disjuntor.py:1371`, `bin/disjuntor.py:1425`, `bin/disjuntor.py:1456`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:703`, `bin/disjuntor.py:707`, `bin/hook_stop_fila1.py:26`, `bin/hook_stop_fila1.py:30`, `bin/hook_stop_fila1.py:34`, `bin/tests/test_padrao_de_cima.sh:90`.)

5. **FILA — temporário órfão do escopo.** Se `fdopen`, `json.dump` ou `os.replace` falhar após `mkstemp`, o bloco de exceção só registra trilha; não remove o temporário. O escritor do estado e o escritor do RELATO já têm limpeza correspondente. (`bin/disjuntor.py:1484`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:284`, `app/core/esteira_vigia.py:885`.)

6. **FILA — fechamento incompleto do envelope para estruturas internas inválidas.** `{"tool_name":"Agent","tool_input":["construir a obra OBRA-A"]}` é objeto JSON, mas `pretool` falha no `.get` da lista; `_ramo_pdc` também falha e devolve `None`, deixando a ferramenta liberada. O novo fallback cru só entra quando `payload is None`. A aceitação desse formato pelo produtor real da chamada é [não verificado]. (`bin/disjuntor.py:2043`, `bin/disjuntor.py:2045`, `bin/disjuntor.py:2004`, `bin/disjuntor.py:2005`, `bin/disjuntor.py:1583`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1589`, `bin/disjuntor.py:2076`.)

Mapa de estado:

| Estado | Escritores | Leitores / multiplicidade |
|---|---|---|
| `disjuntor_estado.json` | Escritor físico `_com_estado`: lê sob `flock`, chama callback e substitui atomicamente o arquivo. Escritores lógicos incluem B1, B3, PDC, B2 e B4. (`bin/disjuntor.py:244`, `bin/disjuntor.py:260`, `bin/disjuntor.py:275`, `bin/disjuntor.py:281`, `bin/disjuntor.py:494`, `bin/disjuntor.py:986`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1745`, `bin/disjuntor.py:1771`, `bin/disjuntor.py:2029`.) | `_com_estado`, `ler_estado` e painel PDC. Há vários escritores lógicos, mas uma porta física no código do instrumento. Testes também escrevem diretamente no arquivo de mentira. (`bin/disjuntor.py:267`, `bin/disjuntor.py:299`, `bin/disjuntor.py:1599`, `bin/tests/test_padrao_de_cima.sh:551`, `bin/tests/test_padrao_de_cima.sh:556`.) |
| `pdc.eventos` | `decide_pdc` cria `visto_em/tipo/obra/entidades/res` e reclassifica eventos antigos vazios. (`bin/disjuntor.py:1396`, `bin/disjuntor.py:1400`.) | Deduplicação, cálculo de janela, disparadas, nomeação e painel. (`bin/disjuntor.py:1386`, `bin/disjuntor.py:1298`, `bin/disjuntor.py:1308`, `bin/disjuntor.py:1438`, `bin/disjuntor.py:1600`.) |
| `pdc.nomeados` | Marca pares `sha|entidade` ao consumir liberação. (`bin/disjuntor.py:1438`, `bin/disjuntor.py:1440`.) | `_pdc_disparadas` verifica se ainda existe evento não nomeado. (`bin/disjuntor.py:1306`, `bin/disjuntor.py:1312`.) |
| `pdc.libera_vistas` | Registra hash da liberação consumida. (`bin/disjuntor.py:1435`.) | Impede reutilização da linha. (`bin/disjuntor.py:1430`.) |
| `pdc.fotos` | Fotografa topo no disparo e remove foto de episódio encerrado. (`bin/disjuntor.py:1403`, `bin/disjuntor.py:1412`, `bin/disjuntor.py:1445`.) | Impede liberação por linha anterior ao disparo. (`bin/disjuntor.py:1433`.) |
| `pdc.disparadas` | Substituído pelo resultado recalculado. (`bin/disjuntor.py:1443`, `bin/disjuntor.py:1444`.) | Detecta disparo novo, informa painel e gera cópia do escopo. (`bin/disjuntor.py:1405`, `bin/disjuntor.py:1413`, `bin/disjuntor.py:1608`, `bin/disjuntor.py:1480`.) |
| Escopo | `_pdc_grava_escopo`, chamado sob a trava após salvar o estado. Testes removem, corrompem e alteram a cópia temporária. (`bin/disjuntor.py:1456`, `bin/disjuntor.py:1472`, `bin/tests/test_padrao_de_cima.sh:506`, `bin/tests/test_padrao_de_cima.sh:519`, `bin/tests/test_padrao_de_cima.sh:522`.) | `_pdc_escopo_lido` e fallback de `estado_pdc`; no instrumento há um escritor, mas estado e escopo são duas gravações independentes. (`bin/disjuntor.py:1492`, `bin/disjuntor.py:1512`, `bin/disjuntor.py:281`, `bin/disjuntor.py:1487`.) |
| RELATO | `relato_negacao` chama `esteira_vigia.no_relato`, que tranca o diretório e faz replace; `gerar_cortes` e `deploy_agendado` escrevem diretamente, fora dessa porta. Há múltiplos escritores reais, já declarados como resíduo/fila. (`bin/disjuntor.py:234`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:815`, `app/core/esteira_vigia.py:848`, `app/core/esteira_vigia.py:884`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`.) | PDC lê bytes completos duas vezes, compara inode/mtime/tamanho e conteúdo, e usa a mesma foto para corpo/topo; fallback relê fora da transição fracassada. (`bin/disjuntor.py:697`, `bin/disjuntor.py:704`, `bin/disjuntor.py:710`, `bin/disjuntor.py:714`, `bin/disjuntor.py:1379`, `bin/disjuntor.py:1523`.) |
| `_PDC_LIVRE_NESTA` | `main_pretool` limpa por chamada; `pretool` acrescenta a obra após `_nega_pdc` retornar livre. (`bin/disjuntor.py:2039`, `bin/disjuntor.py:2014`, `bin/disjuntor.py:2018`.) | `_ramo_pdc` não nega novamente a obra comprovadamente livre. (`bin/disjuntor.py:1591`.) |

Não encontrei segundo juiz de negócio criado pelas mudanças de PDC: a autoridade segue `decide_pdc`/`estado_pdc`, e as portas delegam a ela; as duplicações de RELATO são de escrita do documento, anteriores e expressamente declaradas. (`bin/disjuntor.py:1355`, `bin/disjuntor.py:1503`, `bin/disjuntor.py:1552`, `bin/disjuntor.py:1565`, `app/core/esteira_vigia.py:839`.)

A leitura de tempo continua dentro de `decide_pdc` quando não recebe `agora`; `trilha` também lê o relógio diretamente. A mudança v6 não acrescentou essas leituras, apenas usa `agora` ao calcular o histórico. (`bin/disjuntor.py:1364`, `bin/disjuntor.py:140`, `bin/disjuntor.py:1448`.)

## 31/10 20:54 · sessao db15a087b000 · - · - · origem=Ronald (tmux)

**Escritores e fontes dos campos**

- `CelulaDia.trabalha` admite `True`, `False`, `None`; significado declarado: trabalho, folga, indeterminado/limbo cobra (`app/escala/models.py:1654`).
- Escrita persistente encontrada em `app/ponto/portas/celula.py`: regeneração põe `False` fora da vigência (`:231`, salva `:233`); calcula `valor = ec.eh_dia_trabalho_calculado(d)` e cria (`:243`, `:247`) ou atualiza (`:260`, `:264`); reversão deriva de `(alvo or {}).get('marcos') is not None` (`:532`, salva `:538`); geração em lote calcula (`:590`), cria (`:595`) ou atualiza (`:612`, salva `:615`).
- Chamadores desses escritores: comando `gerar_celulas` (`app/ponto/management/commands/gerar_celulas.py:311`); sinais (`app/escala/signals.py:71`, `:117`); cadastro do tipo (`app/escala/services/cadastro_tipo.py:348`); ajuste/desfazer automático (`app/escala/services/escala_auto_executor.py:104`, `:143`); folgas (`app/escala/folgas.py:220`); correção retroativa (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`); regeneração em exportada (`app/ponto/portas/regen_exportada.py:174`); regeneração de um dia delega (`app/ponto/portas/celula.py:474`); reversão é chamada em `app/ponto/management/commands/aplicar_col900.py:77`.
- `ata.tipo_dia` é produzido por `ata_do_dia`: copia `dia.get('tipo_dia')` (`app/ponto/services/cartorio.py:560`, `:568`). As lavras recebem essa ata nos caminhos sem batida (`:1044`, `:1045`) e normal (`:1563`, `:1565`).
- Persistência da ata: método do modelo delega (`app/escala/models.py:1794`); porta copia a ata recebida (`app/ponto/portas/celula.py:730`), preserva apenas chaves de outra operação (`:732`, lista `:94`), atribui (`:741`, `:768`) e salva (`:743`, `:775`). `carimbar_precedencia` altera somente `ata['precedencia']` (`:818`, `:819`, `:820`).
- Fonte anterior de `tipo_dia`: builder monta `folga`/`trabalho` (`app/escala/utils.py:1743`, `:1749`); overlay de FolgaDia escreve `folga` (`:1118`); cobertura escreve `falta`/`ausencia` (`:1174`, `:1180`).
- Leitor central prefere `ata.get('tipo_dia')`; fallback: `True→trabalho`, `False→folga`, `None→indefinido` (`app/escala/services/leitor_celula.py:345`, `:346`). Transporta também os insumos brutos do juiz (`:370`).
- Cópias de ata em memória não são escritores persistentes: `app/ponto/portas/celula.py:680`; `app/escala/utils.py:1826`, `:1827` — a última contém apenas lâmpadas e órfãs (`:1832`, `:1835`).

**Leitor adicional importante da mesma família**

`app/ponto/marcos.py:155` lê `trabalha` por `values_list`, portanto escapa de busca apenas por `.trabalha`. O predicado literal é `if trabalha is False:` (`:159`), que retorna `'FOLGA'` (`:160`). `None` não veta (`:163`), igual ao juiz sem ata não trabalho; a ata não é consultada (`:155`). Migrar adicionaria veto para `trabalha=True/None` com ata preenchida diferente de trabalho (`app/chamados/juizes.py:408`). O próprio comentário declara que o gate atende emissores de atraso, deduplicação, reabertura e furo (`app/ponto/marcos.py:133`): classificar por chamador, não migrar o gate inteiro sem separar as perguntas.

**Outros predicados encontrados: B — perguntas diferentes**

| Sítio e literal | Pergunta efetiva / None / migração |
|---|---|
| `app/ponto/worklist.py:19`, `dia['tipo_dia'] != 'trabalho'` | Desenha gap pela ata; retorna “sem classificar”, não cala emissão (`:8`, `:12`, `:20`). `None` sem ata vira indefinido e sai, enquanto juiz permite cobrança (`app/escala/services/leitor_celula.py:346`; `app/chamados/juizes.py:408`). Chamadores `app/ponto/worklist.py:60`, `app/colaboradores/views.py:3021`, comando `app/ponto/management/commands/worklist_dp.py:13`. Não migrar. |
| `app/escala/services/furos_diarios.py:39`, `not d or d.get('tipo_dia') != 'trabalho'` | Métrica de marcos furados e projeção FuroDiario (`:2`, `:7`, `:19`). None/indefinido sai; trocar incluiria universo indeterminado sem necessariamente haver marcos (`:44`, `:45`). Chamadores `:89` e `app/colaboradores/services/pos_vinculo.py:81`. Não migrar automaticamente para juiz de chamados. |
| `app/relatorios/furos.py:151`, `d.get('tipo_dia') != 'trabalho'` | Exibição atribui `'folga'` (`:152`); None sem ata torna-se folga na tela, divergindo do juiz de cobrança. Chamador `app/relatorios/views.py:1541`. Não migrar; corrigir rótulo seria outra obra. |
| `app/relatorios/furos.py:218`, `:237`, `d.get('tipo_dia') != 'trabalho' or d.get('sem_ancora')` | Dias em branco/matriz do Raio-X de folha (`:211`, `:230`). None sai. Chamador de `furos_por_dia`: `app/folha/views.py:616`; chamador de `dias_100_em_branco` não localizado [nao verificado]. Não migrar. |
| `app/ponto/propositor.py:36`, `:210`, `not dia or dia.get('tipo_dia') != 'trabalho'` | Degeneração e suspeita de template, usadas para propor reorganização de batidas (`:34`, `:201`). None sai; permitir cobrança não prova geometria. Chamadores `app/ponto/management/commands/propor_reorganizacao.py:67`, `:71`, `app/inteligencia/calculadores.py:325`. Não migrar. |
| `app/ponto/services/simular_dia.py:12`, `dd.get('tipo_dia') != 'trabalho' or dd.get('orfas')` | “Dia perfeito” para autorizar correção de batidas (`:1`, `:11`). None retorna False. Chamadores `app/ponto/services/veredito_celula.py:216`, `:223` e `app/ponto/services/flip_auto.py:258`. Não migrar. |
| `app/ponto/services/veredito_celula.py:221`, `dd0.get('tipo_dia') != 'trabalho'` | Admin pode materializar marcos previstos? Recusa “nada a materializar” (`:222`), não emissão. None recusa. Não migrar. |
| `app/escala/orfa_classe.py:65`, `(dia.get('tipo_dia') or 'folga') != 'trabalho'` | Procedência de órfã, devolve FOLGA_DESMENTIDA (`:53`, `:66`), não cobrança. None é tratado como não trabalho. Chamador de produção não localizado [nao verificado]. Não migrar. |
| `app/ponto/services/vigia_entrada.py:79`, `:120`, filtros `trabalha=True` | Mede padrões de lâmpadas/entrada perdida (`:63`, `:111`). None não entra; juiz o cobraria sem ata contrária. Chamador comando `app/ponto/management/commands/vigia_entrada_faltante.py:24`, `:39`; cron `app/config/crons.py:716`. Não migrar. |
| `app/ponto/services/esmeril_espelho.py:71`, `x.trabalha is False ...`; `:121`, `:123`, `x.trabalha and ...` | Assinaturas de cadastro/escala errada (`:66`, `:67`, `:117`). None não caracteriza folga nem trabalho conhecido. Chamadores `:105`, `app/escala/services/escala_certa.py:27`, comando `app/ponto/management/commands/esmeril_espelho.py:16`. Não migrar. |
| `app/ponto/services/passivo_s84.py:96`, `TRABALHO if cel.trabalha else FOLGA` | Classificação de batidas antigas; FOLGA alimenta seleção para retratação (`:108`, `:109`). None vira FOLGA, diferente do juiz. Chamadores `app/ponto/management/commands/passivo_disputa_retro.py:70`, `app/ponto/management/commands/diff_passivo_s84.py:45`. Não é juiz de cobrança; não migrar. |
| `app/pautas/services.py:418`, `'folga' if cel and cel.trabalha is False else '-'` | Fallback textual quando não há marcos; None mostra `'-'`. Não decide cobrança; não migrar. |
| `app/folha/export.py:1237`, `c.veredito is None and c.trabalha is not None ...` | Retenção da folha por falta de julgamento (`:1238`). None não retém por esse ramo; não migrar ao juiz de chamados. |
| `app/ponto/management/commands/tripwire_celulas.py:338`, `Q(trabalha=False) \| Q(trabalha__isnull=True)`; `:342`, `ata.get('tipo_dia') != 'folga' or ata.get('lampadas')` | Detecta n_celulas fantasma em ata de folga (`:344`, `:345`), não dia sem cobrança. None é pré-filtro; só ata folga sem lâmpadas passa (`:342`). Não migrar. |
| `app/escala/services/semana_do_posto.py:86`, `trabalha=True` | Conjunto de dias previstos na semana (`:82`, `:83`); None fica fora. Não migrar. |
| `app/escala/services/propositor.py:474`, `:485`, `trabalha=True` | Dias discordantes e previstos para proposta de escala (`:470`, `:484`). None fica fora. Não migrar. |
| `app/escala/services/cadastro_realidade.py:208`, `trabalha=True` | Amostra da jornada medida versus cadastro (`:189`, `:215`, `:223`). None fica fora. Não migrar. |
| `app/colaboradores/services/feriado_do_posto.py:94`, `trabalha=True` | Presença em feriado versus dia normal (`:74`, `:75`). None fica fora. Não migrar. |

**Inventário direto para item 5**, além das linhas acima e dos sítios fornecidos pelo usuário:

- Leitura de `trabalha`: juiz `app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:153`; `app/escala/models.py:1072`, `:1810`; `app/escala/servico_jornada.py:303`; `app/ponto/precedencia.py:306`, `:316`, `:322`; `app/ponto/supra_juiz.py:196`; calendário `app/colaboradores/services/calendario.py:814`; correção projetada `app/colaboradores/management/commands/corrigir_escala_retroativa.py:118`; censo `app/ponto/management/commands/censo_fase_12x36.py:392`; diagnóstico `app/ponto/management/commands/dna_x_batida_real.py:59`, `:65`; contagem `app/ponto/management/commands/plano_b_no_dinheiro.py:144`; tripwire `app/ponto/management/commands/tripwire_celulas.py:30`, `:33`, `:98`, `:257`; comparação com template `app/escala/management/commands/celulas_contra_o_template.py:29`, `:46`; a própria porta lê antes de decidir geração/regeneração `app/ponto/portas/celula.py:227`, `:251`, `:604`, `:611`.
- Leitura direta de `ata.tipo_dia`: juiz `app/chamados/juizes.py:408`; projeção do leitor `app/escala/services/leitor_celula.py:345`; tripwire `app/ponto/management/commands/tripwire_celulas.py:342`. Os demais predicados de `tipo_dia` consomem o dict da grade, não diretamente a chave da ata (`app/escala/services/leitor_celula.py:360`).
- Exemplos de leitores de `tipo_dia` para dinheiro, que não devem migrar: `app/folha/export.py:1071`, `:1075`, `:1080`; `app/ponto/services/dia_pago.py:386`, `:393`, `:398`; `app/folha/causas.py:40`, `:41`, `:100`; status do dia `app/ponto/status_dia.py:96`, `:98`, `:106`, `:115`.

## 31/10 20:53 · sessao e874054837e2 · - · - · origem=Ronald (tmux)

Consolidado de leitura; nenhum dado atual de banco/log foi inferido. [nao verificado]

**A — mesma pergunta; migrar a cláusula de dia, preservando outros gates.**

| Sítio/predicado literal | Pergunta efetiva; `None` versus juiz; mudança | Chamador de produção |
|---|---|---|
| `app/ponto/management/commands/emitir_furo_retroativo.py:127` — `ec.eh_dia_trabalho(data) is not True` | Cala emissão de furo antes de `_abrir_chamado`; `None` cala, enquanto juiz com ata ausente/`trabalho` permite cobrar. Migrar muda `None`; passa também a respeitar ata não trabalho quando previsão=True. `app/ponto/management/commands/emitir_furo_retroativo.py:118`, `:127`, `:142`; juiz `app/chamados/juizes.py:408`. | `Command.handle`, entrada declarada sob ordem em `app/config/crons.py:1339`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:49` — `not ec.eh_dia_trabalho(ontem)` | Cala caminho de alerta/cobrança de turno aberto; `None` cala. Juiz sem ata de outro tipo não cala. Com previsão=True e ata de ausência/folga, migrar cala onde hoje deixa avançar. `app/ponto/management/commands/processar_alertas_avancados.py:49`, `:82`, `:121`; juiz `app/chamados/juizes.py:408`. | `Command.handle:19`; cron `app/config/crons.py:344`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:76` — `filter(..., trabalha=True)` | Seleciona células para emitir perguntas; `None` excluído. Juiz sem ata de outro tipo incluiria `None`; True com ata não trabalho seria excluído. Outros requisitos permanecem: motivos e veredito que acusa. `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `:111`, `:120`, `:123`; juiz `app/chamados/juizes.py:408`. | `Command.handle:57`; encadeado de apuração `app/config/crons.py:1464`. |
| `app/ponto/services/vigia_de_hora.py:21` — `filter(..., trabalha=True)` | Seleciona células que podem chegar ao julgamento/emissão; `None` nunca chega. Juiz sem ata de outro tipo deixaria chegar; True com ata não trabalho deixaria de chegar. `app/ponto/services/vigia_de_hora.py:21`, `:113`, `:130`; juiz `app/chamados/juizes.py:408`. | `_candidatas` chamado por `rodar:113`; comando chama `rodar` em `app/ponto/management/commands/vigia_de_hora.py:28`; cron `app/config/crons.py:348`. |
| `app/chamados/services/declaracao_texto.py:95` — `tipo_dia != 'trabalho'` | Suprime marcos faltantes que sustentam perguntas. `trabalha=None`, sem tipo na ata, vira `indefinido` e é descartado; juiz retorna False. True/None com ata preenchida diferente de trabalho continuam descartados. False com ata trabalho hoje passa; juiz barraria. Leitura/fallback em `app/escala/services/leitor_celula.py:345`; gate `app/chamados/services/declaracao_texto.py:95`; juiz `app/chamados/juizes.py:408`. | `_complementar_marcos_faltantes` chama em `app/chamados/services/disputa_emissao.py:794`, criando pergunta em `:813`; `_ata_do_dia` chama em `:827`. Também leitores de declaração/exibição em `app/chamados/services/declaracao_texto.py:285`, `:411`, `:524`, `:563`. |
| `app/ponto/marcos.py:159` — `trabalha is False` | Cláusula “FOLGA” de `celula_veta_cobranca`; None não veta, igual ao juiz quando ata ausente/trabalho. Hoje não lê ata, portanto True/None com ata preenchida não trabalho só são vetados se outro veto de veredito os alcançar. Migrar apenas esta cláusula muda esses casos. `app/ponto/marcos.py:155`, `:159`, `:161`; juiz `app/chamados/juizes.py:408`. | `app/ponto/management/commands/detectar_ausencias.py:626`; `app/ponto/management/commands/reconciliar_grade.py:502`; `app/chamados/reconciliador.py:842`; métrica `app/ponto/services/furos_sem_cobranca.py:98`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:376` — `c.veredito in acusam or not c.trabalha or k in com_batida` | Em `contraprova_piso`, a cláusula `not c.trabalha` dispensa cobrança no dia None; juiz sem ata de outro tipo não dispensa. Trocar só cláusula de dia, preservando veredito/fato. Pergunta declarada: comprovar “fecha por lei, furo vive na célula”. `app/chamados/management/commands/auditar_invariantes_chamados.py:305`, `:308`, `:376`; juiz `app/chamados/juizes.py:408`. | `contraprova_piso` chamado em `app/chamados/management/commands/auditar_invariantes_chamados.py:760`; cron auditor `app/config/crons.py:523`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1443` — `not _cel4.trabalha or _cel4.veredito not in _ACUSAM_a4` | Dispensa célula como sustentação da cobrança/reabertura; None é dispensado mesmo com veredito acusador. Juiz sem ata de outro tipo não dispensa. Trocar apenas cláusula de dia, preservando veredito. `app/chamados/management/commands/auditar_invariantes_chamados.py:1423`, `:1433`, `:1443`; juiz `app/chamados/juizes.py:408`. | Corpo de `Command.handle` em `app/chamados/management/commands/auditar_invariantes_chamados.py:552`; cron `app/config/crons.py:523`. |

**B — pergunta diferente; não migrar.**

| Sítio | Pergunta, None e efeito de uma substituição mecânica | Chamador |
|---|---|---|
| `app/ponto/services/flip_auto.py:208` — `cel is not None and cel.trabalha is False`; `:212` — `v is None or v.trabalha is not True` | Geometria: escolher dia/âncora da batida para etiquetar E/S, não emissão. Current=None não entra no ramo de folga; véspera=None é recusada como âncora. Juiz não considera None sem ata como sem trabalho; substituição permitiria essa véspera, alterando âncora/flips. Pergunta de geometria/turno: autoridades de marcos e turno em `app/docs/JUIZES.md:22`; finalidade explícita em `app/ponto/services/flip_auto.py:165`, `:171`, `:176`; gates `:208`, `:212`. | `avaliar_fila` chama em `app/ponto/services/flip_auto.py:310`; comando `app/ponto/management/commands/flip_automatico.py:32`; tela `app/ponto/views.py:3295`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1542` — `_cel is None or _cel.trabalha is not True` | C5: célula contradiz FolgaDia? Só True prova contradição; None não prova trabalho. Migrar pelo complemento do juiz acusaria None como contradição sem evidência. Não há juiz específico de contradição FolgaDia identificado em JUIZES. `app/chamados/management/commands/auditar_invariantes_chamados.py:1530`, `:1542`, `:1549`; juiz `app/chamados/juizes.py:408`. | `Command.handle:552`; cron `app/config/crons.py:523`. |
| `app/ponto/worklist.py:19` — `dia['tipo_dia'] != 'trabalho'` | Desenho de gap na worklist: retorna “sem classificar”, não absolvição de cobrança. None sem ata vira indefinido e retorna gap=None. Trocar por juiz alteraria apresentação e poderia encaminhar para “sem lâmpada”, não produzir cobrança por si. `app/ponto/worklist.py:7`, `:10`, `:19`, `:22`; fallback `app/escala/services/leitor_celula.py:345`. | `coletar_gaps_dp:60`; tela `app/colaboradores/views.py:3021`; comando `app/ponto/management/commands/worklist_dp.py:13`. |
| `app/ponto/propositor.py:36` — `not dia or dia.get('tipo_dia') != 'trabalho'` | Diagnóstico de dia degenerado para proposta de movimentação de batidas. None sem ata descartado; trocar por juiz amplia universo de propostas e muda False+ata trabalho. `app/ponto/propositor.py:34`, `:36`, `:43`, `:48`; leitor `app/escala/services/leitor_celula.py:345`. | `app/ponto/management/commands/propor_reorganizacao.py:71`. |
| `app/ponto/propositor.py:210` — mesmo predicado | Estatística de deltas em dias de trabalho para suspeita de template; não decide calar cobrança. None sem ata descartado; substituição muda amostra estatística. `app/ponto/propositor.py:201`, `:203`, `:210`, `:221`; leitor `app/escala/services/leitor_celula.py:345`. | `app/ponto/management/commands/propor_reorganizacao.py:67`; `app/inteligencia/calculadores.py:325`. |
| `app/ponto/services/simular_dia.py:12` — `dd.get('tipo_dia') != 'trabalho' or dd.get('orfas')` | Juiz de “dia perfeito” para simular flips: exige tipo trabalho, nenhuma órfã, nenhum missing, tipos corretos. None sem ata falha; substituir cláusula alteraria perfeição/flip. `app/ponto/services/simular_dia.py:1`, `:12`, `:18`, `:21`. | `app/ponto/services/flip_auto.py:303`, `:304`; `app/ponto/services/veredito_celula.py:219`. |
| `app/escala/services/furos_diarios.py:39` — `not d or d.get('tipo_dia') != 'trabalho'` | Apuração da métrica `% marcos furados/marcos previstos`, persistida em FuroDiario; não é emissão de chamado. None sem ata descartado; substituição muda denominador/dias e False+ata trabalho. `app/escala/services/furos_diarios.py:1`, `:12`, `:19`, `:39`, `:98`; leitor `app/escala/services/leitor_celula.py:345`. | `apurar:89`; `app/escala/management/commands/apurar_furos_diarios.py:8`; `app/colaboradores/services/pos_vinculo.py:52`. |
| `app/ponto/services/vigia_entrada.py:79`, `:120` — `filter(..., trabalha=True)` | Contagem/diagnóstico de entrada faltante com saída presente; distingue falha de caminho versus adesão, não decide cobrar o dia. None excluído; juiz sem ata o incluiria na população, mudando estatística. `app/ponto/services/vigia_entrada.py:11`, `:63`, `:79`, `:111`, `:120`. | `contador` chama agregação em `app/ponto/services/vigia_entrada.py:99`; demais chamadores não rastreados [nao verificado]. |

A regra comum para os leitores de dict merece aparecer explicitamente: `trabalha=None` **não significa sempre** `tipo_dia=None`; o leitor prefere a ata e, sem tipo preenchido, produz `'indefinido'`. Assim None+ata trabalho passa em predicados de tipo, enquanto None sem ata é descartado. `app/escala/services/leitor_celula.py:345`.

## 31/10 20:53 · sessao 771f5d470aba · - · - · origem=Ronald (tmux)

Censo somente de leitura. Referências abaixo usam a linha atual da cópia.

| Classe | Sítio e predicado literal | Pergunta efetiva; `None` e efeito da migração | Chamadores de produção |
|---|---|---|---|
| **A — migrar a cláusula de elegibilidade** | `app/ponto/services/cartorio.py:1504`: `ec is not None and ec.eh_dia_trabalho(data) is True` | Decide se o furo total chega a `_abrir_chamado` (`:1495-1517`). Com célula `trabalha=None`, `eh_dia_trabalho` devolve `None` (`app/escala/models.py:1071-1072`) e este gate cala; juiz não cala sem ata não trabalho (`app/chamados/juizes.py:408`). Migração muda `None` e também `True` com ata preenchida diferente de trabalho. **Existência de vínculo e marcos continua sendo requisito separado** (`cartorio.py:1503-1507`). | `_julgar_colab_corpo` via `julgar_colab` (`cartorio.py:777,802`); batch `app/ponto/management/commands/processar_cartorio.py:65`; evento `cartorio.py:1721-1723`. |
| **A — migrar a cláusula de elegibilidade** | `app/ponto/services/cartorio.py:1608`: `ec is None or ec.eh_dia_trabalho(data) is not True` | `_marcos_do_furo_parcial` retorna `None` e impede emissão parcial; mesmos desvios acima. O restante responde quais marcos apagados têm motivo e âncora (`:1624-1635`), pergunta diferente que permanece. | Helper chamado em `cartorio.py:1512,1543`; emissão parcial em `:1550-1552`; cadeia batch/evento acima. |
| **C — exceção declarada, não migrar automaticamente** | `app/ponto/services/cartorio.py:1207`: `_cel.trabalha is not False and _acesos(x, _ata) is not True` | Decide se chamado **protesta**, alimentando `chamados_vivos`, lâmpada cobrado e reconciliação (`:1208,1214,1250,1281-1287`). `None` permite protesto se marcos não acesos: concorda com juiz sem ata não trabalho. Adicionar juiz mudaria `True/None` com ata não trabalho. Esta mudança está **explicitamente pendente**, porque acrescenta segunda cláusula ao universo do protesto (`app/docs/JUIZES.md:60-62`; `app/core/juizes.py:145-148`). | Mesma cadeia cartório; usos internos `cartorio.py:1208,1250,1281`. |
| **B — não migrar** | `app/ponto/services/cartorio.py:1714`: `data == hoje and cel.trabalha is not False and not teto_de_hoje` | Responde **quando aguardar fim do turno antes de julgar hoje**, não se cobrar (`:1715-1719`). `None` aguarda; juiz sem ata não trabalho também não declara dispensa. Substituir por juiz permitiria julgamento antecipado com `True/None` e ata não trabalho. Juiz específico de horário para esta guarda em `JUIZES.md`: **[nao verificado]**; documento distingue borda da meia-noite sem juiz (`app/docs/JUIZES.md:66-67`). | `app/ponto/signals.py:93`; `app/chamados/signals.py:180`; `app/chamados/reconciliador.py:675`; `app/ponto/services/vigia_de_hora.py:130`; `app/chamados/services/cobrar_dia.py:39`; `app/ponto/services/furos_sem_cobranca.py:128`; `app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/portas/celula.py:372`; comandos `rejulgar_ausencia_em_folga.py:71`, `curar_residuo_bug115.py:143`. |
| **B — não migrar** | `app/ponto/services/dia_do_colab.py:121`: `not cel.trabalha` | Gera frase de apresentação **“Dia sem trabalho previsto: nenhuma marca era esperada”** (`:122-123`). `None` recebe a frase; juiz sem ata não trabalho retorna `False`. Migrar mudaria frase para `None`, e para `True` com ata não trabalho. Previsão tem juiz `EscalaColaborador.eh_dia_trabalho` (`app/docs/JUIZES.md:74-75`); status lavrado tem outro (`:82-83`). | `_veredito_do_dia` em `dia_do_colab.py:100`; porta `app/api/views_mensageria.py:1963`; ficha `app/colaboradores/services/ficha.py:280`. |
| **B — não migrar** | `app/ponto/services/dia_do_colab.py:130-131`: `f.get('faixa') in ('feriado', 'ferias', 'atestado', 'afastamento', 'falta', 'abono', 'folga') and cel.trabalha` | Compara **precedência e célula discordando** para frase administrativa (`:132-136`), não cala cobrança. `None` não acusa a discordância. Troca por juiz alteraria a natureza da comparação; precedência é `fatos_do_dia` (`app/docs/JUIZES.md:76-77`). | Mesma cadeia anterior. |
| **B — não migrar** | `app/relatorios/dia_da_frota.py:47`: `not cel.trabalha` | Conta **folga** e exclui da listagem de acusados (`:48-55`); não altera chamado. `None` conta folga, contra juiz `False` sem ata não trabalho. Migração alteraria contagens/listagem para `None` e `True` com ata não trabalho. Pergunta declarada é relatório “quem faltou”, pelo veredito/lâmpadas (`:1-6`); autoridades pertinentes são previsão/status (`app/docs/JUIZES.md:74,82`). | `app/api/views_mensageria.py:1814`. |
| **B — não migrar** | `app/chamados/services/fila_por_causa.py:192-193`: `CelulaDia.objects.filter(colaborador_id__in=cids, trabalha=True, data__gte=ini)` | Carrega **vizinhos previstos** (`:194`) para diagnosticar início de adesão, falta em dias vizinhos e ausência sustentada (`:224-251`). `None` não entra em `previstos`; juiz sem ata não trabalho permitiria cobrança. Migração alteraria vizinhos e gaveta, além de retirar `True` com ata não trabalho. Previsão: juiz de `app/docs/JUIZES.md:74-75`. | `particionar` via `quadro` (`fila_por_causa.py:271`), `toques` (`:308`) e `bloqueios_para_fechar` (`:353`); API `app/api/views_mensageria.py:1582`; badge `app/core/management/commands/lavrar_badge_navbar.py:63`; `validar_classe_a.py:44`; comando `app/chamados/management/commands/fila_por_causa.py:13`. |

Outros predicados semelhantes confirmados como perguntas diferentes:

| Classe | Predicado e pergunta | `None` / migração / produção |
|---|---|---|
| **B** | `app/ponto/services/simular_dia.py:12`: `dd.get('tipo_dia') != 'trabalho' or dd.get('orfas')`; responde **dia perfeito para simular correções**, verificando missing e coerência de tipos (`:14-23`). | Não lê diretamente `trabalha`; lê grade construída por `grade_dia` (`:5-8`). A grade constrói `None` como trabalho com `sem_ancora` (`app/escala/utils.py:725-752`). Trocar pelo juiz mudaria conceito de perfeição e perderia/alteraria requisito das órfãs. Prod: `app/ponto/services/flip_auto.py:303-304`; `app/ponto/services/veredito_celula.py:219,223`. |
| **B** | `app/ponto/services/veredito_celula.py:221`: `dd0.get('tipo_dia') != 'trabalho'`; responde **há trabalho previsto para materializar decisão administrativa trabalhou?**, explicitado na recusa (`:222`). | Não é silêncio de chamado. `None` indireto é grade/sem âncora acima. Prod: view `app/ponto/views.py:3304`; serviço declarativo `app/ponto/services/declarar_dia.py:206-207`. |
| **B** | `app/ponto/services/espelho.py:690`: `bool(_gd and _gd.get('tipo_dia') == 'folga')`; responde flag visual `folga` (`:781`). | `None` direto não é lido. Prod interno `espelho.py:1266`; tela `app/ponto/views.py:158`, API `app/api/views.py:1369`, ficha `app/colaboradores/services/ficha.py:199`. Juiz de status lavrado declarado em `app/docs/JUIZES.md:82-83`; não substituir por juiz de cobrança. |
| **B** | `app/ponto/worklist.py:19`: `dia['tipo_dia'] != 'trabalho'`; responde **há gap desenhável na ata?** (`:8-13`), devolve `None` para não classificar (`:20`). | `grade_da_celula` sem ata e `trabalha=None` produz `indefinido` (`app/escala/services/leitor_celula.py:345-346`), então não classifica; juiz retorna `False`. Migrar mudaria classificador/exibição, não cobrança. Chamado por `worklist.py:60`; produção `app/colaboradores/views.py:3017`; comando `app/ponto/management/commands/worklist_dp.py:12`. |
| **B** | `app/ponto/propositor.py:36` e `:210`: `not dia or dia.get('tipo_dia') != 'trabalho'`; respondem **degeneração para escalada/reorganização** e **template suspeito pela mediana dos deltas** (`:34-54,201-221`). | Não leem `trabalha` diretamente; não há resposta única para `None` sem identificar o produtor do dict. Com produtor leitor_celula resulta indefinido (`leitor_celula.py:345-346`); com builder grade, trabalho/sem_ancora (`utils.py:725-752`). Prod `app/ponto/management/commands/propor_reorganizacao.py:67,71`; `template_suspeito` também em `app/inteligencia/calculadores.py:325`. |
| **B** | `app/relatorios/furos.py:151`: `d.get('tipo_dia') != 'trabalho'`; `:218,:237`: `d.get('tipo_dia') != 'trabalho' or d.get('sem_ancora')`. | Perguntas de **relatório/calendário de furos e matriz Raio-X da folha** (`:129,210-214,229-233`). Leitor sem ata + `None` fornece indefinido e é excluído, contra juiz que cobra. Migração altera contagens, não estado de chamado. Prod dataset `app/relatorios/views.py:1541`; `furos_por_dia` em `app/folha/views.py:616`; chamador produção de `dias_100_em_branco`: **[nao verificado]**. |
| **C quanto a uso; B quanto à pergunta** | `app/escala/orfa_classe.py:65`: `(dia.get('tipo_dia') or 'folga') != 'trabalho'`; classifica **procedência de órfã**, devolvendo `FOLGA_DESMENTIDA` (`:53-66`). | Não lê `trabalha` diretamente. Busca nominal não encontrou chamada fora da própria definição; execução em produção **[nao verificado]**. Não é elegibilidade de cobrança. |

**Pontos relevantes de alimentação:** o leitor lavrado privilegia `ata.tipo_dia`, depois converte `trabalha=True/False/None` em `trabalho/folga/indefinido` (`app/escala/services/leitor_celula.py:345-346`); já `eh_dia_trabalho` devolve o `trabalha` cru da célula, sem consultar ata (`app/escala/models.py:1066-1073`). Portanto, gates com um ou outro insumo não são equivalentes ao juiz de cobrança (`app/chamados/juizes.py:407-408`).

## 31/10 20:49 · sessao 3017134f97d6 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**A volta 2 está corrigida, mas a garantia geral sem ressalva de fuso NÃO procede.** Reproduzi uma entrada histórica, sem dupla contagem e abaixo de 26 h, em que o novo corpo desconta **360 minutos**, enquanto a janela efetiva dos prefixos cobre **354**. O problema está no cursor horário que conserva o offset inicial. (`app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:183`)

1. **Inclinação, limites e casos de borda**

   Com offset constante e prefixos até 26 h, a premissa procede: as contribuições começam ou terminam em 00h, 05h ou 22h; a prorrogação continua linearmente. Entre essas bordas, a inclinação é constante, 0, 1 ou 2. Entrada fora da hora cheia e segundos apenas encurtam o primeiro ou último trecho. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:104`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:116`, `app/ponto/motor_calculo_v2.py:180`)

   **Universalmente, NÃO procede**, por dois motivos:

   - Acima de 26 h, o prefixo cai abruptamente para zero; esse corte pode ocorrer dentro de uma hora. O teto final mantém o desconto devolvido em zero, mas a afirmação sobre inclinação deixa de valer. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:186`)
   - Na mudança histórica de offset de São Paulo, a grade conservada pelo cursor deixa de coincidir com as horas locais das bordas. Reproduzi pelas funções reais extraídas por AST, somente em memória: (`app/ponto/motor_calculo_v2.py:79`, `app/ponto/motor_calculo_v2.py:101`, `app/ponto/motor_calculo_v2.py:180`)

   | Parâmetro | Contraprova |
   |---|---|
   | Entrada | 31/12/1913 22:00 −03:06 |
   | Saída | 01/01/1914 07:00 −03:00 |
   | Pausa | 31/12 23:00 −03:06 → 01/01 06:00 −03:00 |
   | Prorrogação | `False` |
   | Total noturno | 414 min |
   | Diferença dos prefixos da pausa | **354 min** |
   | Corpo novo | **360 min** |

   Esses resultados vêm da conversão e das medidas efetivamente executadas pelo corpo; o teto de 414 não elimina o excesso de seis minutos. (`app/ponto/motor_calculo_v2.py:162`, `app/ponto/motor_calculo_v2.py:181`, `app/ponto/motor_calculo_v2.py:186`)

   No trecho gerado `04:00−03:06 → 05:00−03:06`, o prefixo vale 360 no início, chega a 414 às `04:54−03:06` e permanece 414 até o fim. A inclinação muda de 1 para 0 dentro do trecho; o corpo transforma aumento de 54 em desconto de 60. A borda real `05:00−03:00` corresponde a `04:54−03:06`. (`app/ponto/motor_calculo_v2.py:101`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:183`)

   A contraprova **não viola os limites numéricos**: 360 é positivo e inferior tanto à duração da peça, 414, quanto ao total, 414. Em aritmética exata, união e avanço sem sobreposição limitam cada peça à sua duração; o `min` final garante desconto ≤ total. (`app/ponto/motor_calculo_v2.py:169`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:183`, `app/ponto/motor_calculo_v2.py:186`)

   **A desigualdade literal contra a duração em `float` merece ressalva:** no próprio caso de segundos, obtive desconto `60.41666666666667` e duração `60.416666666666664`, excesso de aproximadamente `7,1e-15` minuto. É arredondamento, abaixo da tolerância `1e-9` das propriedades. (`app/ponto/motor_calculo_v2.py:183`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:124`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:185`)

   Os casos da volta 2 agora dão **1.500, 1.470 e 1.455**, respectivamente. Exatamente 26 h continua admitido; acima disso e em recorte invertido, o resultado é zero. Naive local e UTC equivalente deram o mesmo resultado no caso de segundos. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:277`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:283`, `app/ponto/motor_calculo_v2.py:77`, `app/ponto/motor_calculo_v2.py:91`, `app/ponto/motor_calculo_v2.py:94`)

   Nas duas transições de horário de verão de 2018 sondadas, com pausa cobrindo o recorte, obtive: avanço de novembro, **360 sem prorrogação / 480 com**; retorno de fevereiro, **480 / 600**. A crítica de que conservar offset necessariamente quebra essas transições de uma hora **NÃO procede nesses casos**; isso não demonstra todos os históricos. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:180`)

2. **Recorte normal e chamadores**

   Para offset constante, prefixo sem multiplicidade, recorte ≤26 h e pausas válidas disjuntas, a medida nova coincide com a diferença de prefixos e com a janela. Identidade **bit a bit** não procede por arredondamento; somente “recorte normal” também não basta se houver pausas repetidas ou sobrepostas, porque a nova união muda deliberadamente esse resultado. A contraprova histórica acima também impede afirmar equivalência sem ressalva de fuso. (`app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:169`, `app/ponto/motor_calculo_v2.py:181`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:293`)

   Todos os caminhos indicados continuam chegando ao mesmo corpo; nenhum recebe regra nova própria:

   | Chamador | Caminho |
   |---|---|
   | `he_noturna` | Usa `[inicio_extra, fim]`, intervalos e a mesma flag. (`app/ponto/motor_calculo_v2.py:430`, `app/ponto/motor_calculo_v2.py:432`) |
   | `_noturno_dos_intervalos` / `calcular_noturnas` | Delegação comum e subtração do desconto. (`app/ponto/motor_calculo_v2.py:963`, `app/ponto/motor_calculo_v2.py:1905`) |
   | `_ContextoNoturno` / noturnas do dia | Empresta o método por identidade. (`app/ponto/calculador/regras.py:63`, `app/ponto/calculador/regras.py:86`) |
   | HE por período | Chama `he_noturna` com as pontas, pausas e flag recebidas. (`app/ponto/calculador/regras.py:886`) |
   | `noturno_no_par` | Delega um par e os parâmetros da CCT. (`app/ponto/calculador/periodo.py:508`) |

   A perda de âncora da HE parcial permanece fora desta cura: o desconto continua ancorado em `inicio_extra`. Impacto produtivo por colaborador/rubrica: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:430`, `app/ponto/motor_calculo_v2.py:432`)

3. **Premissa das bordas e mudança futura**

   As bordas inteiras estão no código executado de `minutos_noturnos`: 00h, 05h, 22h e meia-noite seguinte. O corpo novo lê uma propriedade estrutural dessa função; **segunda cópia da janela NÃO procede**, pois ele não reimplementa o predicado 22–05 ou seu gatilho. Contudo, depende de essa propriedade permanecer alinhada ao cursor — condição ausente do comentário geral. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:104`, `app/ponto/motor_calculo_v2.py:173`, `app/ponto/motor_calculo_v2.py:181`)

   **Há mordida se uma borda virar não inteira:** mutando apenas em memória e mantendo a referência atual, a grade longa apresentou 69 falhas com início às 22:01, 117 às 22:30 e 386 com término às 05:01. Essa proteção vem da janela 22–05 declarada pelo oráculo; não existe uma asserção estrutural específica sobre bordas inteiras. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:159`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:225`)

4. **Testes, vacuidade e relógio** [nao verificado]

   Executei **21 testes puros** isolados em memória, extraindo o AST real e substituindo o invólucro Django por `unittest`; passaram grades, sobrepostos, segundos, contraprovas e mutantes correspondentes. Isso não representa execução da suíte Django canônica, que permanece **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:80`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:244`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:324`)

   - **Peças longas mordem:** `_limite_por_peca` produz seis falhas; grade antiga e sobrepostos produzem zero com esse mutante. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:225`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`)
   - **Vacuidade por `subTest` NÃO procede:** o método é chamado diretamente na instância criada, e a asserção propaga nessa execução; confirmei a mordida do teste da volta 2. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:327`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:358`)
   - O teste de segundos verifica **60 min 25 s**, mas o mutante anterior também passa nele; sua função é preservar frações, enquanto a regressão anterior é detectada pelas peças longas. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:302`)
   - `_regra` trunca minutos e declara relógio de parede para a grade sem horário de verão. **Não certifica segundos ou históricos genericamente**: no caso de segundos retorna 60, contra `60+25/60`; na contraprova de 1914 retorna 360 e deixa passar o erro. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:146`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:149`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:153`)
   - **Relógio solto NÃO procede:** datas constantes; `_aw` combina data/hora e aplica o fuso, sem consultar “agora”. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:169`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:26`)

5. **Classes reprovadas, custo e término**

   **Segundo juiz, segundo escritor, derivação paralela da janela e selo sem mordida NÃO procedem neste diff.** Há uma conversão defeituosa da medida no caso histórico identificado e uma lacuna de cobertura, embora os mutantes anteriores sejam detectados. Ordem de trava, RED histórico dos commits, suíte completa e pouso: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:181`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:358`)

   Para uma única pausa de 60 min, são **3 chamadas** de `minutos_noturnos` se alinhada à hora e **5** se atravessa duas horas: duas por trecho, mais uma para o teto. Em várias peças, essa última chamada é compartilhada; o chamador ainda pode medir seu total separadamente. (`app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:181`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:961`)

   **Laço infinito NÃO procede para datetimes válidos e operações sem exceção:** a próxima hora é estritamente posterior a `a`; como `_f > a`, `b > a`, e `a = b` faz progresso. Recortes enormes ainda percorrem todas as horas das peças, mesmo quando o teto final será zero. (`app/ponto/motor_calculo_v2.py:179`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:184`, `app/ponto/motor_calculo_v2.py:186`)

## 31/10 20:47 · sessao 57c81f562b71 · - · - · origem=Ronald (tmux)

**A premissa universal da inclinação constante NÃO procede.** Há contraprova histórica em São Paulo, sem prorrogação e sem dupla contagem: o cursor mantém o offset inicial enquanto as bordas de `N` são localizadas novamente. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:101`, `app/ponto/motor_calculo_v2.py:180`)

Sonda das funções reais extraídas por AST, somente em memória:

| Parâmetro | Valor |
|---|---|
| Entrada | 31/12/1913 22:00, offset −03:06 |
| Saída | 01/01/1914 07:00, offset −03:00 |
| Pausa | 31/12/1913 23:00 −03:06 → 01/01/1914 06:00 −03:00 |
| `prorrogacao` | `False` |
| `N(entrada, saída)` | 414 |
| Diferença dos prefixos da pausa | **354** |
| Corpo novo | **360** |

Resultados reproduzidos pelo corpo e pela dependência reais; o teto 414 não elimina os seis minutos excedentes. (`app/ponto/motor_calculo_v2.py:89`, `app/ponto/motor_calculo_v2.py:116`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:183`, `app/ponto/motor_calculo_v2.py:186`)

A peça começa com offset −03:06; a aritmética horária conserva esse offset. No trecho gerado `04:00−03:06 → 05:00−03:06`, obtive:

| Cursor | `N(entrada, cursor)`, `pr=False` |
|---|---:|
| 04:00−03:06 | 360 |
| 04:30−03:06 | 390 |
| 04:54−03:06 | 414 |
| 04:55−03:06 | 414 |
| 05:00−03:06 | 414 |

Portanto a inclinação passa de 1 para 0 **dentro** do trecho; o algoritmo transforma aumento de 54 minutos em desconto de 60. A borda de 05:00−03:00 ocorre às 04:54 no cursor antigo. (`app/ponto/motor_calculo_v2.py:101`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:181`, `app/ponto/motor_calculo_v2.py:183`)

**Cuidado na consolidação:** isso prova desigualdade com a diferença de prefixos e com a presença na janela efetiva de `N`. Não prova desigualdade com `_regra` literal: esse oráculo usa relógio de parede sem timezone e declara a premissa “sem horário de verão em 2026”; deve produzir 360 nesse caso histórico. A igualdade literal nessa célula específica é **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:146`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:148`)

Nas transições de **horário de verão de ±1 hora**, sondei 2.208 células representativas em 1931, 1950 e 2018, normalizando o minuto real por UTC antes de consultar a hora local: nenhuma divergência encontrada. Isso é amostra, não prova geral. (`app/ponto/motor_calculo_v2.py:79`, `app/ponto/motor_calculo_v2.py:180`)

Casos reproduzidos de 2018 com pausa cobrindo o recorte:

| Recorte São Paulo | `pr=False`: `N`/desconto | `pr=True`: `N`/desconto |
|---|---:|---:|
| 03/11 22h → 04/11 07h, avanço | 360 / 360 | 480 / 480 |
| 17/02 22h → 18/02 07h, retorno | 480 / 480 | 600 / 600 |

A ausência de normalização explícita do cursor **não basta**, por si só, para reprovar essas mudanças de uma hora: a grade continua alinhada às bordas em tempo absoluto nesses casos. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:104`, `app/ponto/motor_calculo_v2.py:180`)

Os limites continuam protegidos na contraprova histórica: soma não negativa, desconto inferior à duração da peça e ao total. O defeito identificado é a medida de presença, não violação desses limites. (`app/ponto/motor_calculo_v2.py:167`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:183`, `app/ponto/motor_calculo_v2.py:186`)

**Laço infinito NÃO procede para datetimes válidos e operações sem exceção:** a próxima hora fica estritamente depois de `a`; com `_f > a`, `min(_f, próxima_hora)` também fica depois de `a`. O cursor avança estritamente até a ponta. (`app/ponto/motor_calculo_v2.py:179`, `app/ponto/motor_calculo_v2.py:180`, `app/ponto/motor_calculo_v2.py:184`)

## 31/10 20:46 · sessao d23198846302 · - · - · origem=Ronald (tmux)

- **Mordida confirmada:** execução isolada, somente em memória, passou **21 testes** de função, grades, sobrepostos, contraprovas e mutantes. Extraí AST real; substituí apenas `SimpleTestCase` por `unittest.TestCase` e `make_aware` por `ZoneInfo`. Django não está instalado; suíte Django canônica **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:80`, `:244`, `:272`, `:324`)

- `_limite_por_peca` produz **6 falhas** na grade de peças longas; primeira: 20:30→22:15 seguinte, pausa integral, `pr=True`, **1470 versus 1455**. Grade antiga e sobrepostos produzem zero falhas com esse mutante. Portanto a cobertura nova detecta precisamente a regressão antes invisível. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:225`, `:302`, `:347`)

- **Vacuidade por `subTest` NÃO procede:** o teste mutante instancia `t` e chama seu método diretamente; não executa `t.run()`. Assim `t._outcome` permanece vazio e `subTest` não captura a asserção. A execução confirmou a mordida de `test_MORDE_contraexemplo_da_volta2_com_o_corpo_de_bd95dff7`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:277`, `:327`, `:358`)

- **Teste dos segundos passa:** desconto **60 + 25/60**. É regressão útil, sem vacuidade, mas o corpo anterior `_limite_por_peca` também satisfaz esse caso normal; a mordida contra ele vem das peças longas. A referência `_regra` inteira usa truncamento de minutos, portanto não certifica frações genericamente. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119`, `:149`, `:153`, `:302`)

- **Mudança para borda não inteira é detectada:** mutações apenas em memória, mantendo `_regra`, deram na grade longa: **22:01: 69 falhas; 22:30: 117; 05:01: 386; 05:30: 386**. Exemplos: 22:01 resulta 929 contra 930; 05:01 sem prorrogação resulta 421 contra 420. A proteção decorre da referência que declara 22–05; não há asserção estrutural específica exigindo bordas inteiras. (`app/ponto/motor_calculo_v2.py:100`, `:103`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:116`, `:133`, `:159`, `:225`)

- **Relógio solto NÃO procede:** datas constantes e `_aw` combina data/hora, aplicando o fuso; não lê “agora”. A referência declara explicitamente a ausência de horário de verão em sua grade de 2026. Histórico DST e microssegundos não são cobertos por esses testes. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:84`, `:146`, `:169`, `:230`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:26`)

## 31/10 20:40 · sessao d87ee81bfd17 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**REPROVADO para a garantia geral da janela declarada.** Os quatro contraexemplos foram fechados e os limites numéricos agora se sustentam, mas existe outra entrada em que o desconto diverge de `_regra`, dentro das 26h. A afirmação “até 26h o resultado é o da janela minuto a minuto” **NÃO procede**. (`app/ponto/motor_calculo_v2.py:158`, `app/ponto/motor_calculo_v2.py:173`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:145`)

1. **Os quatro casos anteriores e a sobreposição estão corrigidos.** Em execução das funções reais extraídas por AST, somente em memória, obtive os valores abaixo; são sondas sintéticas, não dados produtivos. (`app/ponto/motor_calculo_v2.py:163`, `app/ponto/motor_calculo_v2.py:175`, `app/ponto/motor_calculo_v2.py:176`)

   | Recorte; intervalo; prorrogação ligada | Desconto |
   |---|---:|
   | 00h → 00h seguinte; 22h30–23h30 | 60 min |
   | 04h → 04h seguinte; 01h–02h seguinte | 60 min |
   | 22h → 00h01 dois dias depois; 01h–02h intermediário | 0 min |
   | Mesmo recorte; último intervalo 23h30–00h01 | 0 min |

   Esses são os quatro casos explicitamente exercitados pelos testes novos. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:219`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:223`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:228`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:233`)

   A união trata intervalos cruzados, contidos, repetidos e contíguos; invertidos, inteiramente externos ou apenas encostados na borda tornam-se peças vazias e são descartados. Recorte invertido também retorna zero. **NÃO procede** a crítica de que o corpo novo continua descontando sobreposição duas vezes ou produzindo desconto negativo nesses casos. (`app/ponto/motor_calculo_v2.py:163`, `app/ponto/motor_calculo_v2.py:166`, `app/ponto/motor_calculo_v2.py:168`, `app/ponto/motor_calculo_v2.py:91`)

   **A nova contraprova é esta, em São Paulo, `prorrogacao=True`:**

   | Entrada → saída | Intervalo | Corpo novo | `_regra` |
   |---|---|---:|---:|
   | 14/10/2026 21h → 15/10 23h, exatamente 26h | Recorte inteiro | **1.560** | **1.500** |
   | Mesmo recorte | 14/10 21h30 → 15/10 22h30 | **1.500** | **1.470** |
   | 14/10 20h30 → 15/10 22h15, 25h45 | Recorte inteiro | **1.470** | **1.455** |

   Reproduzi também essas divergências chamando a própria `_celula`: ela devolve falha de categoria `regra`. O prefixo duplica parte do trecho final; o limite pela duração deixa essa duplicação compensar minutos diurnos anteriores às 22h. Portanto os limites passam e a medida continua errada. (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:112`, `app/ponto/motor_calculo_v2.py:173`, `app/ponto/motor_calculo_v2.py:175`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:173`)

   Para datetimes válidos e comparáveis após localização, **não encontrei violação das desigualdades**: cada peça unida fica entre zero e sua duração, e a soma devolvida não excede `N(entrada, saida)`. Exatamente 26h ainda é admitido; acima disso o desconto fica zero. Com `prorrogacao=False`, a sobreposição das extensões não ocorre. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:175`, `app/ponto/motor_calculo_v2.py:176`)

   Naive local e representação equivalente em UTC produziram o mesmo desconto na sonda; o corpo localiza recorte e intervalos antes das comparações. `intervalos=None` funciona como vazio; **par contendo `None` lança `TypeError`**, pois `localizar(None)` preserva `None`. Outros fusos configurados e transições históricas de horário: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:75`, `app/ponto/motor_calculo_v2.py:77`, `app/ponto/motor_calculo_v2.py:79`, `app/ponto/motor_calculo_v2.py:161`, `app/ponto/motor_calculo_v2.py:163`)

2. **Os limites são invariantes legítimos, mas insuficientes como cura da medida.** O teto resolve o salto para zero acima de 26h; o limite por peça resolve os dois descontos dobrados anteriores. Entretanto, o novo contraexemplo mostra que uma diferença errada pode ficar dentro de ambos os limites. Isso é defeito deste corpo ao converter o prefixo em desconto, embora a duplicação original de `minutos_noturnos` permaneça fora do aval. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:155`, `app/ponto/motor_calculo_v2.py:173`, `app/ponto/motor_calculo_v2.py:175`)

   **Existe forma de fechar os contraexemplos sem esses `min/max` e sem alterar `minutos_noturnos`.** Para entradas alinhadas ao minuto, uma construção demonstrável é unir as pausas, retornar zero quando o total é zero e contar cada minuto cuja diferença de prefixos seja **positiva**, contando presença uma vez em vez de somar sua multiplicidade. Sondei essa construção em memória: ela distingue os 1.500 minutos da contraprova dos 1.560 acumulados pelo prefixo. A autoridade da janela continua sendo a função existente; uma implementação eficiente e exata também para segundos/microssegundos está **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:112`, `app/ponto/motor_calculo_v2.py:116`, `app/ponto/motor_calculo_v2.py:173`)

3. **Todos os caminhos continuam chegando ao mesmo corpo**, com estes recortes e parâmetros:

   | Chamador | O que chega |
   |---|---|
   | `he_noturna` | `[inicio_extra, saida]`; início obtido por recuo de minutos trabalhados, atravessando pausas; mesma lista e mesma `prorrogacao` para total e desconto. (`app/ponto/motor_calculo_v2.py:375`, `app/ponto/motor_calculo_v2.py:420`, `app/ponto/motor_calculo_v2.py:422`) |
   | Motor: `_noturno_dos_intervalos` / `calcular_noturnas` | Pontas recebidas, intervalos e `self.prorrogacao_noturna_pos5h`; nos cálculos dos motores, pontas efetivas e pausas recortadas. (`app/ponto/motor_calculo_v2.py:951`, `app/ponto/motor_calculo_v2.py:1895`, `app/ponto/motor_calculo_v2.py:2732`, `app/ponto/motor_calculo_v2.py:2760`, `app/ponto/motor_calculo_v2.py:2864`, `app/ponto/motor_calculo_v2.py:3215`) |
   | `_ContextoNoturno` / `noturnas_do_dia` | Empresta o método por identidade; flag convertido em `bool`; cada par recebe os intervalos do dia. Pontas nulas ou par invertido são ignorados. (`app/ponto/calculador/regras.py:61`, `app/ponto/calculador/regras.py:63`, `app/ponto/calculador/regras.py:83`, `app/ponto/calculador/regras.py:86`) |
   | Calculador: HE por período | Pontas efetivas e pausas carimbadas; distribui HE do último período para o primeiro, limitada ao trabalho daquele período; passa o flag do dia. (`app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/alimentacao.py:127`, `app/ponto/calculador/alimentacao.py:190`, `app/ponto/calculador/regras.py:880`, `app/ponto/calculador/regras.py:886`) |
   | `noturno_no_par` | Um par, `tuple(intervalos or ())` e `cct.prorrogacao_pos5h`. (`app/ponto/calculador/periodo.py:508`, `app/ponto/calculador/periodo.py:509`) |

   No motor, o split de HE recebe pontas **cruas** do período e pausas efetivas; portanto a contraprova anterior da HE parcial perder a âncora do turno **permanece**, fora deste corpo. O novo desconto não transporta a janela original para `inicio_extra`. (`app/ponto/motor_calculo_v2.py:983`, `app/ponto/motor_calculo_v2.py:986`, `app/ponto/motor_calculo_v2.py:420`, `app/ponto/motor_calculo_v2.py:421`)

   **“Sempre chega tupla” NÃO procede:** o adaptador fornece listas de pares de timestamps; o carimbo transforma em tupla. Coleção `None` é normalizada como vazia; as montagens examinadas não fornecem pares com pontas `None`. Saída `None` pertence ao período aberto, não a uma pausa válida. (`app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:43`, `app/ponto/motor_calculo_v2.py:695`, `app/ponto/motor_calculo_v2.py:1810`, `app/ponto/calculador/alimentacao.py:136`)

   **“O pareador garante pausas disjuntas” também NÃO procede:** `_pares_marcados` junta primeiro pares declarados e depois pares adjacentes com pontas ainda não usadas; um par interior pode sobreviver dentro de outro declarado. O filtro de intervalos apenas exige duração mínima, e o recorte não une sobreposições. Existência produtiva dessa configuração: **[nao verificado]**. (`app/ponto/turnos.py:809`, `app/ponto/turnos.py:812`, `app/ponto/turnos.py:818`, `app/ponto/turnos.py:878`, `app/ponto/turnos.py:885`, `app/ponto/turnos.py:543`, `app/ponto/motor_calculo_v2.py:1853`)

   O flag vem do motor/CCT; o motor aplica piso por ciclo quando o parâmetro não foi declarado, e o calculador recebe a CCT do dia. Cadastro efetivo produtivo: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:738`, `app/ponto/motor_calculo_v2.py:740`, `app/core/regua_cct.py:466`, `app/ponto/calculador/alimentacao.py:572`)

   Comparado à diferença crua anexada, para pausas válidas, disjuntas e prefixos crescendo no máximo um minuto por minuto, o valor permanece igual. União e limites mudam os casos identificados; nenhum chamador ganha uma regra própria. O impacto por colaborador/rubrica em produção é **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:168`, `app/ponto/motor_calculo_v2.py:173`, `app/ponto/motor_calculo_v2.py:175`, `app/ponto/motor_calculo_v2.py:176`)

   A cadeia de escrita e leitura das três rubricas é:

   | Etapa | Escrita → leitura |
   |---|---|
   | Períodos | `_escrever_noturnas` grava minutos noturnos; `_aplicar_he_noturna` grava subsets HE50/100. A releitura v3 também atribui noturnas ao período. (`app/ponto/motor_calculo_v2.py:972`, `app/ponto/motor_calculo_v2.py:973`, `app/ponto/motor_calculo_v2.py:988`, `app/ponto/motor_calculo_v2.py:990`, `app/ponto/calculador/periodo.py:753`) |
   | Mensal | `ResultadoMes` soma noturnas de períodos comuns e FT; subsets HE somente dos comuns. O fechamento atribui as três rubricas e persiste. (`app/ponto/motor_calculo_v2.py:301`, `app/ponto/motor_calculo_v2.py:320`, `app/ponto/motor_calculo_v2.py:324`, `app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:896`, `app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1112`, `app/ponto/services/fechamento.py:1128`) |
   | Diário/v3 | `lavrar` lê os minutos; `regras.do_dia` produz as três rubricas; a lavra substitui campos autorizados e grava `DiaPago`, inclusive a versão `oraculo`. (`app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:358`, `app/ponto/calculador/regras.py:896`, `app/ponto/calculador/regras.py:898`, `app/ponto/calculador/regras.py:912`, `app/ponto/services/fechamento.py:1245`) |
   | Consumo | TXT lê `horas_noturnas` para adicional; subsets HE não geram linhas próprias. Conferência, apresentação diária, ranking e simulação também consomem esses valores. (`app/folha/export.py:413`, `app/folha/export.py:436`, `app/ponto/services/dia_pago.py:560`, `app/folha/porta_export.py:267`, `app/ponto/services/dia_pago.py:922`, `app/folha/services/ranking_he.py:225`, `app/folha/management/commands/simular_folha.py:84`) |

4. **`_regra` é uma referência independente de implementação**, não uma cópia dos prefixos e limites: constrói um conjunto de minutos e aplica o predicado da janela. Compartilha `localizar` e assume explicitamente as políticas da casa para gatilho e teto de 26h; não certifica essas escolhas como lei externa. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:123`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:131`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:137`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:145`)

   A grade e os sobrepostos agora são estritos. **NÃO procede a crítica antiga** de que basta igualar `_por_peca`: há igualdade obrigatória com `_regra`. Porém, a grade usa pausas de 60 minutos, pula entrada às 21h e os sobrepostos têm uniões curtas; por isso não pegam a nova contraprova. Também não cobrem segundos fracionários. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:114`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:135`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:156`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:173`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:191`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:204`)

   Na execução isolada em memória, grade e sobrepostos reais deram zero falhas; `_por_peca` gerou 1.644 registros de falha na grade e `_diferenca_crua`, 1.256. Nos sobrepostos, foram 524 e 593. São registros de propriedades, não células distintas; ambos os mutantes caem. Os quatro contraexemplos também são submetidos individualmente à diferença crua. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:160`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:184`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:197`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:268`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:277`)

   Não identifiquei vacuidade por universo vazio: existem casos positivos e exigência explícita de falha sob mutação. **Relógio solto NÃO procede:** datas são constantes e `_aw` apenas combina data/hora e aplica o fuso. Suíte Django canônica: **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:53`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:155`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:269`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:26`)

5. **Segundo juiz, segundo escritor e derivação paralela novos NÃO procedem neste diff:** o corpo continua lendo a mesma autoridade; wrappers delegam; `_regra` existe somente no teste. As atribuições adicionais da releitura v3 já existem fora do trecho alterado. (`app/ponto/motor_calculo_v2.py:173`, `app/ponto/motor_calculo_v2.py:1895`, `app/ponto/calculador/regras.py:63`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:122`, `app/ponto/calculador/periodo.py:753`)

   **“Selo sem mordida” como ausência de detecção NÃO procede**, mas há cobertura insuficiente para a garantia anunciada, demonstrada pela contraprova longa. Ordem de trava, suíte completa, pré-requisitos e pouso efetivo: **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:268`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:271`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:191`, `CLAUDE.md:544`, `CLAUDE.md:550`)

   Há ainda uma divergência documental: o anexo cita o corte V3-P5 em `CORTES.md:417`, mas a cópia termina em `app/docs/CORTES.md:414`; esse registro na cópia é **[nao verificado]**. Isso não substitui nem amplia o escopo explicitamente autorizado no pedido. (`app/docs/CORTES.md:414`)

## 31/10 20:38 · sessao 83058262f7d5 · - · - · origem=Ronald (tmux)

A referência é independente na implementação: `_regra` usa conjunto de minutos e predicado de janela/gatilho; não chama `minutos_noturnos` nem reproduz prefixos, união ordenada ou limites do corpo. Compartilha somente `localizar`; portanto não verifica independentemente essa conversão. O guarda de 26h é política explicitamente assumida, não referência legal independente. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:122-147`)

A grade e os sobrepostos são estritos: exigem lista de falhas vazia, incluindo igualdade com a referência, além das desigualdades. **Não procede** a crítica antiga de que a grade apenas admitia igualdade com `_por_peca`. Entretanto, a grade só usa pausas de 60min; os sobrepostos cobrem uniões de até 120min. Não certificam peças longas nem a preservação da medida ao dividir/juntar intervalos. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:109-119`, `:160-208`)

Sonda em memória, sem arquivos: AST das três funções reais, `unittest.TestCase` no lugar de `SimpleTestCase` e `_aw` equivalente em São Paulo. Onze métodos diretos passaram; grade e sobrepostos reais deram zero falhas. Mutantes produziram:

| Gerador | `_por_peca` | `_diferenca_crua` |
|---|---:|---:|
| Grade | 1.644 registros de falha | 1.256 |
| Sobrepostos | 524 | 593 |

São registros de propriedades reprovadas, não células distintas; os geradores podem registrar várias falhas por célula. A suíte Django canônica é **[nao verificado]**. Os pontos executados correspondem aos geradores e verificações em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:160-208`; os corpos mutantes estão em `:68-77` e `:239-251`.

**Não procede “selo sem mordida” como ausência total de detecção**: a grade discrimina ambos os mutantes e os testes exigem que a restauração provoque falha; os quatro contraexemplos são individualmente submetidos à diferença crua. Ainda pode haver buraco de cobertura, como peças longas. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:257-283`)

Não identifiquei teste vacuamente verde por universo vazio: as grades têm dimensões literais não vazias; os testes golden exigem números positivos e os mutantes exigem falhas. A verificação de duração por peça ocorre apenas quando há um intervalo original, mas a igualdade com `_regra` também atua nos sobrepostos. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:51-65`, `:155-174`, `:184-208`, `:268-283`)

**Não procede relógio solto neste arquivo**: as datas são constantes; `_aw` apenas combina data/hora e chama `make_aware`, sem consultar a hora atual. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:37`, `:84`, `:155`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:26-27`)

`_regra` pertence somente aos testes; não nasce segundo juiz ou segundo escritor de produção. As classes reprovadas estão em `CLAUDE.md:544-552`; a exigência de mordida está em `CLAUDE.md:318-323`; L-035 e L-110 existem em `app/docs/LEIS.md:83` e `:136`.

**Atenção documental:** a cópia de `app/docs/CORTES.md` termina na linha 414; o corte V3-P5 citado no anexo como linha 417 não está nela. Sua existência na cópia é **[nao verificado]**; o corte foi fornecido pelo usuário no anexo.

## 31/10 20:37 · sessao cf137b0d25d9 · - · - · origem=Ronald (tmux)

**Chamadores e entradas**

| Caminho | Recorte, flag e intervalos |
|---|---|
| `he_noturna` | Usa `[inicio_extra, saida]`, sendo `inicio_extra` calculado recuando por tempo trabalhado e pausas. Total e desconto recebem o mesmo recorte e `prorrogacao`; a lista é repassada sem conversão. (`app/ponto/motor_calculo_v2.py:372`, `app/ponto/motor_calculo_v2.py:375`, `app/ponto/motor_calculo_v2.py:420`, `app/ponto/motor_calculo_v2.py:421`, `app/ponto/motor_calculo_v2.py:422`) |
| Motor / noturnas | `calcular_noturnas` usa entrada/saída recebidas e `self.prorrogacao_noturna_pos5h`; desconta pelo wrapper e aplica piso zero. Nos três motores, são pontas efetivas após `_janela_do_par` e intervalos recortados por `_intervalos_na_janela`. (`app/ponto/motor_calculo_v2.py:951`, `app/ponto/motor_calculo_v2.py:953`, `app/ponto/motor_calculo_v2.py:1895`, `app/ponto/motor_calculo_v2.py:2727`, `app/ponto/motor_calculo_v2.py:2732`, `app/ponto/motor_calculo_v2.py:2760`, `app/ponto/motor_calculo_v2.py:2854`, `app/ponto/motor_calculo_v2.py:2859`, `app/ponto/motor_calculo_v2.py:2864`, `app/ponto/motor_calculo_v2.py:3192`, `app/ponto/motor_calculo_v2.py:3215`) |
| Motor / HE | `_aplicar_he_noturna` entrega pontas **cruas** `periodo.entrada/saida`, flag do motor e `periodo.intervalos_do_motor`, carimbados após recorte efetivo. (`app/ponto/motor_calculo_v2.py:982`, `app/ponto/motor_calculo_v2.py:983`, `app/ponto/motor_calculo_v2.py:985`, `app/ponto/motor_calculo_v2.py:986`, `app/ponto/motor_calculo_v2.py:1808`, `app/ponto/motor_calculo_v2.py:1810`) |
| `_ContextoNoturno` / `noturnas_do_dia` | Empresta o método do motor por identidade e converte flag em `bool`; percorre cada par, ignora pontas `None` ou par invertido e entrega a mesma lista de intervalos do dia a cada recorte. (`app/ponto/calculador/regras.py:60`, `app/ponto/calculador/regras.py:61`, `app/ponto/calculador/regras.py:63`, `app/ponto/calculador/regras.py:82`, `app/ponto/calculador/regras.py:83`, `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:86`) |
| Calculador / HE por período | Recebe pontas efetivas e `tuple(sorted(intervalos_do_motor))`; distribui a HE do último período para o primeiro, limitada ao trabalho daquele período, e chama `he_noturna` com flag do dia e pausas daquele período. (`app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/alimentacao.py:127`, `app/ponto/calculador/alimentacao.py:135`, `app/ponto/calculador/alimentacao.py:190`, `app/ponto/calculador/regras.py:876`, `app/ponto/calculador/regras.py:880`, `app/ponto/calculador/regras.py:883`, `app/ponto/calculador/regras.py:886`, `app/ponto/calculador/regras.py:888`) |
| `noturno_no_par` | Um único par, `tuple(intervalos or ())`, `cct.prorrogacao_pos5h` e flag de redução; a releitura passa pontas efetivas e pausas do período. (`app/ponto/calculador/periodo.py:508`, `app/ponto/calculador/periodo.py:509`, `app/ponto/calculador/periodo.py:510`, `app/ponto/calculador/periodo.py:752`) |

**Origem e formato.** `parear_turnos` recebe batidas com timestamps aware e as ordena; o adaptador converte os turnos em pares de timestamps, permitindo **saída do período** `None`, e obtém pausas por `intervalos_do_turno`. O motor recebe a lista por entrada e depois carimba uma tupla de pares. Portanto, “intervalos já chegam sempre como tupla” **NÃO procede**: a origem fornece lista, o carimbo fornece tupla. (`app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1215`, `app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:38`, `app/ponto/adaptador_motor.py:43`, `app/ponto/adaptador_motor.py:44`, `app/ponto/motor_calculo_v2.py:2426`, `app/ponto/motor_calculo_v2.py:2427`, `app/ponto/motor_calculo_v2.py:1810`)

A rota alternativa pela ata monta pausas com pontas acesas e cronológicas, transforma cada ponta em instante real e rejeita `None`; não fornece pares com `None` pela montagem observada. `None` na coleção é tratado como vazio pelo carimbo e pelo recorte. A possibilidade produtiva de uma chamada externa entregar par com `None` é **[nao verificado]**. (`app/ponto/juiz_batida.py:279`, `app/ponto/juiz_batida.py:282`, `app/ponto/juiz_batida.py:284`, `app/ponto/motor_calculo_v2.py:693`, `app/ponto/motor_calculo_v2.py:695`, `app/ponto/motor_calculo_v2.py:699`, `app/ponto/motor_calculo_v2.py:701`, `app/ponto/motor_calculo_v2.py:1810`, `app/ponto/motor_calculo_v2.py:1849`)

**“A autoridade garante intervalos disjuntos” NÃO procede.** `_pares_marcados` primeiro junta pares declarados e depois junta pares adjacentes cujas pontas não tenham sido usadas; não exclui batidas interiores ao primeiro par. `intervalos_do_turno` apenas aplica piso de duração. Sonda AST da função real retornou `[(01:00,04:00),(02:00,03:00)]` para um par declarado contendo outro par marcado. O recorte e o carimbo preservam essa sobreposição; existência em produção é **[nao verificado]**. (`app/ponto/turnos.py:809`, `app/ponto/turnos.py:810`, `app/ponto/turnos.py:812`, `app/ponto/turnos.py:818`, `app/ponto/turnos.py:878`, `app/ponto/turnos.py:881`, `app/ponto/turnos.py:885`, `app/ponto/turnos.py:543`, `app/ponto/turnos.py:544`, `app/ponto/motor_calculo_v2.py:1853`, `app/ponto/motor_calculo_v2.py:1856`, `app/ponto/motor_calculo_v2.py:1810`)

O flag do motor é resolvido pelo cadastro/régua, com piso `tipo_ciclo != '12x36'` quando não declarado; o calculador recebe flag da CCT do dia. Cadastro efetivo e valores produtivos: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:738`, `app/ponto/motor_calculo_v2.py:740`, `app/core/regua_cct.py:182`, `app/core/regua_cct.py:454`, `app/core/regua_cct.py:466`, `app/ponto/calculador/alimentacao.py:572`, `app/ponto/calculador/alimentacao.py:573`, `app/ponto/calculador/alimentacao.py:596`)

**Escritores e leitores das rubricas**

- `_escrever_noturnas` escreve os minutos de relógio/legais; `_aplicar_he_noturna` escreve os subsets HE50/100; a releitura v3 também atribui noturnas ao período. (`app/ponto/motor_calculo_v2.py:971`, `app/ponto/motor_calculo_v2.py:972`, `app/ponto/motor_calculo_v2.py:973`, `app/ponto/motor_calculo_v2.py:988`, `app/ponto/motor_calculo_v2.py:990`, `app/ponto/calculador/periodo.py:753`, `app/ponto/calculador/periodo.py:754`)
- `ResultadoMes` soma noturnas de períodos comuns e FT; subsets HE somam somente períodos comuns. O fechamento atribui as três rubricas arredondadas e persiste por `save`/`update`. (`app/ponto/motor_calculo_v2.py:301`, `app/ponto/motor_calculo_v2.py:320`, `app/ponto/motor_calculo_v2.py:324`, `app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:896`, `app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1112`, `app/ponto/services/fechamento.py:1128`, `app/ponto/services/fechamento.py:1131`, `app/ponto/services/fechamento.py:1134`, `app/ponto/services/fechamento.py:1137`)
- `DiaPago.lavrar` lê os três valores dos períodos; FT acrescenta noturnas; valores do calculador substituem campos com dono autorizado e a persistência é `bulk_create`. (`app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:224`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:327`, `app/ponto/services/dia_pago.py:328`, `app/ponto/services/dia_pago.py:358`)
- `regras.do_dia` produz as três rubricas; o fechamento lavra essa versão como `oraculo`. (`app/ponto/calculador/regras.py:896`, `app/ponto/calculador/regras.py:898`, `app/ponto/calculador/regras.py:912`, `app/ponto/services/fechamento.py:1239`, `app/ponto/services/fechamento.py:1245`, `app/ponto/services/fechamento.py:1256`, `app/ponto/services/fechamento.py:1262`)
- TXT lê `horas_noturnas` para adicional noturno; os subsets HE não geram linhas próprias. A soma de `DiaPago` agrega todos os campos e alimenta conferência da porta de exportação; apresentação diária, ranking e simulação também leem essas rubricas. (`app/folha/export.py:413`, `app/folha/export.py:430`, `app/folha/export.py:436`, `app/ponto/services/dia_pago.py:560`, `app/folha/porta_export.py:267`, `app/folha/porta_export.py:268`, `app/ponto/services/dia_pago.py:922`, `app/folha/services/ranking_he.py:34`, `app/folha/services/ranking_he.py:225`, `app/folha/management/commands/simular_folha.py:59`, `app/folha/management/commands/simular_folha.py:84`)

## 31/10 20:07 · sessao 438619dd0837 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**REPROVADO para a garantia geral.** O caso 22–07 com intervalo 05:30–06:30 foi corrigido, mas permanecem duas contraprovas: a HE parcial perde a janela do turno e plantões longos descontam minutos em dobro. A afirmação de monotonicidade também falha acima de 26h. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:156`, `app/ponto/motor_calculo_v2.py:402`)

**1. Chamadores, recortes e prorrogação**

| Leitor | Recorte e `prorrogacao` que chegam ao desconto |
|---|---|
| `he_noturna` | `[inicio_extra, saida]`; `inicio_extra` recua por minutos trabalhados, atravessando os intervalos. Total e desconto recebem esse recorte e o mesmo parâmetro `prorrogacao`. (`app/ponto/motor_calculo_v2.py:354`, `app/ponto/motor_calculo_v2.py:402`) |
| `_noturno_dos_intervalos` / `calcular_noturnas` | O wrapper encaminha entrada, saída, intervalos e `self.prorrogacao_noturna_pos5h`. Total e desconto usam o mesmo par; depois há piso zero. (`app/ponto/motor_calculo_v2.py:933`, `app/ponto/motor_calculo_v2.py:1877`) |
| `_ContextoNoturno` / `noturnas_do_dia` | Empresta o método do motor. Cada par válido é contado e descontado com `bool(prorrogacao_pos5h)`; pares nulos/invertidos são ignorados. (`app/ponto/calculador/regras.py:60`, `app/ponto/calculador/regras.py:82`) |
| HE por período no calculador | Distribui a HE do dia pelos períodos, do último para o primeiro, limitada ao trabalho de cada período; entrega suas pontas, seus intervalos e a prorrogação do dia a `he_noturna`. (`app/ponto/calculador/regras.py:875`, `app/ponto/calculador/regras.py:883`) |
| Releitura v3 do período | `noturno_no_par` entrega um único par, seus intervalos e o flag da CCT a `noturnas_do_dia`. (`app/ponto/calculador/periodo.py:508`) |

No motor, as noturnas recebem pontas **efetivas**, após a janela de HE e o corte dos intervalos. Já `_aplicar_he_noturna` entrega as pontas **cruas** do período, acompanhadas dos intervalos efetivos. No calculador, a alimentação usa as pontas efetivas carimbadas. Portanto os envelopes dos leitores podem divergir. (`app/ponto/motor_calculo_v2.py:2701`, `app/ponto/motor_calculo_v2.py:2709`, `app/ponto/motor_calculo_v2.py:2742`, `app/ponto/motor_calculo_v2.py:964`, `app/ponto/calculador/alimentacao.py:106`)

O flag vem do motor/CCT; a implementação do piso retorna `False` para `12x36`, enquanto a CCT pode fornecer outro valor. O cadastro efetivo de produção é **[nao verificado]**. (`app/core/regua_cct.py:182`, `app/core/regua_cct.py:466`, `app/ponto/calculador/alimentacao.py:572`)

Para recorte de até 26h e intervalos válidos e disjuntos, o desconto fica entre zero e o total contado pelo mesmo leitor, inclusive quando esse total contém duplicação. A função, porém, soma intervalos sem unir sobreposições nem rejeitar intervalo invertido; portanto **“sempre ≤ total” não é uma garantia irrestrita da API**. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:112`, `app/ponto/motor_calculo_v2.py:152`)

Resultados das funções reais, em minutos:

| Recorte; intervalo | Novo desconto | Avaliação |
|---|---:|---|
| 22–07; 05:30–06:30; `True` | 60 | Corrige P5. (`app/ponto/motor_calculo_v2.py:156`) |
| 19–07; 21:30–22:30; `True` | 30 | Cruza 22:00 corretamente. (`app/ponto/motor_calculo_v2.py:103`, `app/ponto/motor_calculo_v2.py:156`) |
| 22–07; 23–00; `True` | 60 | Meia-noite preservada. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:103`, `app/ponto/motor_calculo_v2.py:156`) |
| 22–07; 04:30–05:30; `True` / `False` | 60 / 30 | Respeita o flag na fronteira 05:00. (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:156`) |
| 22–07; intervalo inteiramente fora | 0 | Fora é ignorado; interseção parcial é cortada. (`app/ponto/motor_calculo_v2.py:154`, `app/ponto/motor_calculo_v2.py:156`) |
| Recorte invertido | 0 | Os prefixos utilizados retornam zero; não apareceu desconto indevido nesse caso. (`app/ponto/motor_calculo_v2.py:91`, `app/ponto/motor_calculo_v2.py:156`) |

**Contraprova da janela do turno:** turno 22–07, intervalo 05:30–06:30, prorrogação ligada e **60 minutos de HE**. O recuo encontra 05:00; `N(05:00,07:00)=0`, porque a ativação exige início estritamente anterior às 05:00. O split devolve **zero**, embora os 60 minutos trabalhados da ponta pertençam à prorrogação do turno original. Com HE de 120 minutos, o início recua a 04:00 e o split devolve 120. É defeito preexistente do recorte do leitor; o diff não transporta a janela original. (`app/ponto/motor_calculo_v2.py:109`, `app/ponto/motor_calculo_v2.py:354`, `app/ponto/motor_calculo_v2.py:402`)

**2. Validade da diferença de prefixos**

A diferença preserva a medida acumulada de `N`; ela **não transforma uma medida duplicada em minutos físicos corretos**. O segmento 00–05 é estendido até `fim`, mas os segmentos seguintes continuam sendo somados. (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:112`, `app/ponto/motor_calculo_v2.py:156`)

| Entrada → saída; intervalo; `True` | `N(total)` | Desconto novo | Quebra |
|---|---:|---:|---|
| 00:00 → 00:00 seguinte; 22:30–23:30 | 1.560 | 120 | Um intervalo físico de 60 minutos desconta 120. |
| 04:00 → 04:00 seguinte; 01–02 do dia seguinte | 1.800 | 120 | A extensão anterior sobrepõe a janela seguinte. |
| 22:00 → 00:01 dois dias depois, 26h01; intervalo 01–02 intermediário | 0 | 60 | Desconto maior que o total. |
| Mesmo recorte de 26h01; último intervalo 23:30–00:01 | 0 | −1.620 | Desconto negativo acrescenta noturnas. |

Esses resultados decorrem da sobreposição das janelas, do retorno abrupto a zero acima de 26h e da subtração dos prefixos. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:112`, `app/ponto/motor_calculo_v2.py:156`)

Logo, a justificativa “`N` não decresce” é falsa globalmente. A duplicação já ocorre **dentro de 24h**, sem atingir o teto; turnos regulares de 12h não alcançam essas sobreposições. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:103`, `app/ponto/motor_calculo_v2.py:149`)

O plantão 24h tem caminho concreto: `MotorPlantao` admite continuidade de 26h e reutiliza `Motor12x36.calcular_periodo`. O pareador também possui envelope de 26h. Existência desses exemplos em produção: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:3437`, `app/ponto/motor_calculo_v2.py:3460`, `app/ponto/turnos.py:1431`)

**3. Quem escreve e quem lê as três rubricas**

| Etapa | Escritor → leitor |
|---|---|
| Período do motor | `_escrever_noturnas` grava minutos noturnos; `_aplicar_he_noturna` grava os dois subsets de HE. (`app/ponto/motor_calculo_v2.py:953`, `app/ponto/motor_calculo_v2.py:964`) |
| Consolidação mensal | `ResultadoMes.horas_noturnas` soma períodos comuns e FT; os subsets de HE somam somente períodos comuns. (`app/ponto/motor_calculo_v2.py:283`, `app/ponto/motor_calculo_v2.py:300`) |
| `FechamentoMensal` | O serviço atribui `horas_noturnas`, `horas_extras_50_noturna` e `horas_extras_100_noturna`, arredondando a duas casas. Persiste por `save` ou `update`. (`app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:896`, `app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1112`, `app/ponto/services/fechamento.py:1128`) |
| `DiaPago`, versão motor | `lavrar` lê os minutos do período e monta as três rubricas; persiste por `bulk_create`. (`app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:358`) |
| Calculador v3 | `regras.do_dia` escreve os subsets e as noturnas; a mesma `lavrar` substitui os campos calculados. O fechamento grava essa versão como `oraculo`, sem substituir por ela os campos de `FechamentoMensal`. (`app/ponto/calculador/regras.py:896`, `app/ponto/calculador/regras.py:898`, `app/ponto/calculador/regras.py:912`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/fechamento.py:1245`) |
| Folha/TXT | `eventos_do_fechamento` lê `horas_noturnas` para `adicional_noturno`. Os dois subsets **não geram linhas próprias**: HE50/100 saem integrais. (`app/folha/export.py:413`, `app/folha/export.py:430`, `app/folha/export.py:436`) |
| Conferência, tela e relatórios | A soma de `DiaPago` agrega os campos; a porta do export consulta essa soma. `rubricas_do_dia` apresenta noturnas; ranking e simulação também leem os valores do fechamento. (`app/ponto/services/dia_pago.py:548`, `app/folha/porta_export.py:267`, `app/ponto/services/dia_pago.py:922`, `app/folha/services/ranking_he.py:225`, `app/folha/management/commands/simular_folha.py:84`) |

Há escritas excepcionais existentes: `restaurar_fechamento` aceita valores conhecidos e faz `update`; os comandos antigos de apply passam por essa porta. `retratar_exportada` preserva dinheiro existente, mas inicializa rubricas em zero ao criar dia ausente. Nenhuma dessas portas nasce neste diff. (`app/ponto/services/fechamento.py:1326`, `app/ponto/services/fechamento.py:1360`, `app/ponto/management/commands/aplicar_09_he_intra.py:149`, `app/ponto/management/commands/aplicar_09_corte_b.py:266`, `app/ponto/services/dia_pago.py:469`)

**4. Testes e mutante** [nao verificado]

Os testes exatos **mordem a regra do caso P5**: exigem 8h de HE100, 8h de HE100 noturna e 8h de noturnas. Os quatro mutantes restauram `_por_peca` e exigem falha de asserção; não inspecionam a forma da implementação. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:51`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:138`)

A grade **passa com o código antigo**: `desc >= _por_peca` admite igualdade. Portanto não detecta essa regressão, embora não seja genericamente vazia — descontos noturnos positivos impedem um mutante constante zero. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:128`)

O golden usa um período inteiro em HE100. Faltam HE parcial após 05:00, HE50, múltiplos intervalos, sobreposição, recortes externos/invertidos e limite de 26h; a grade exclui expressamente os turnos longos. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:43`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:115`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:120`)

Execução isolada em memória: 13 métodos verdes, incluindo os quatro mutantes, com substituições das dependências Django. Suite Django canônica: **[nao verificado]**. [nao verificado]

**5. Classes já reprovadas**

- **Segundo juiz, segundo escritor, derivação paralela:** o desconto continua na mesma função e usa `minutos_noturnos`; os wrappers permanecem delegadores. Não aparece nova autoridade nesses caminhos. (`app/ponto/motor_calculo_v2.py:156`, `app/ponto/motor_calculo_v2.py:1877`, `app/ponto/calculador/regras.py:63`)
- **Relógio fora da borda:** o cálculo usa os instantes recebidos e os localiza no fuso; a falha demonstrada é a troca da âncora para a ponta extra. (`app/ponto/motor_calculo_v2.py:151`, `app/ponto/motor_calculo_v2.py:402`)
- **Selo sem mordida:** há mordida focal nos quatro mutantes, mas a grade não distingue a regressão antiga e exclui a contraprova de 24h. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:115`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:132`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:138`)
- **Ordem de trava e pouso antes do pré-requisito:** execução de esteira, deploy, impacto financeiro e estado produtivo não constam dos anexos: **[nao verificado]**. [nao verificado]

## 31/10 20:05 · sessao 9e38a74b7ef3 · - · - · origem=Ronald (tmux)

Os testes mordem o caso P5, mas não provam a propriedade universal alegada: o golden fixa 22→07, intervalo05:30→06:30, prorrogação ligada e toda a jornada em HE100; cobra 8h no total, no split e nas noturnas. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:39-65`.

Os quatro mutantes verificam falha de asserção ao restaurar `_por_peca`; usam resultados numéricos, não inspeção da forma do código. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:138-156`.

A grade **passa também sob `_por_peca`**: suas condições admitem exatamente o desconto antigo; consequentemente não detecta a regressão P5. Ela não é genericamente vazia: o mutante constante zero falha em intervalos noturnos. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:128-132`.

A grade usa somente entradas18/20/22/23, durações6/8/9/12h e um intervalo contido de1h; exclui explicitamente os turnos longos que contam em dobro. O golden não exercita HE50 nem HE parcial cujo início efetivo ocorre depois05. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:115-126`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:43-49`, `app/ponto/motor_calculo_v2.py:402-405`.

`assertRaises(AssertionError)` aceita qualquer asserção interna quebrada, mas os testes numéricos normais coexistem; portanto não há fundamento para chamar os quatro mutantes atuais de vazios. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:140-142`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:51-65`.

Execução isolada:13 métodos verdes, incluindo os4 mutantes, com motor/regras originais importados e AST original dos testes, `_aw` e `piso_intervalo_art71`; substituídas somente dependências Django por `unittest.TestCase`, settings.TIME_ZONE e `timezone.make_aware=TZ_LOCAL.localize`. Suite Django canônica: [nao verificado]. O helper original está em `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:26-27`; o piso original em `app/escala/models.py:6-21`.

## 31/10 20:05 · sessao b26384d06b53 · - · - · origem=Ronald (tmux)

**Censo dos chamadores e recortes (somente leitura).**

| Caminho | Recorte e `prorrogacao` recebidos |
|---|---|
| `he_noturna` | Calcula `inicio_extra` recuando minutos trabalhados desde `saida`; chama tanto `minutos_noturnos` quanto o desconto em `[inicio_extra, saida]`, com o mesmo parâmetro `prorrogacao`. `app/ponto/motor_calculo_v2.py:354`, `app/ponto/motor_calculo_v2.py:402` |
| `MotorBase._noturno_dos_intervalos` | Encaminha os argumentos recebidos à função e usa `self.prorrogacao_noturna_pos5h`. `app/ponto/motor_calculo_v2.py:1877` |
| `MotorBase.calcular_noturnas` | Total e desconto usam exatamente `[entrada, saida]` e o mesmo atributo de prorrogação; aplica piso zero após descontar. `app/ponto/motor_calculo_v2.py:933` |
| `_ContextoNoturno` / `noturnas_do_dia` | Empresta o método de `MotorBase`; para cada par válido usa o mesmo par e `bool(prorrogacao_pos5h)` no total e desconto. Ignora par nulo/invertido e aplica piso zero depois da soma. `app/ponto/calculador/regras.py:60`, `app/ponto/calculador/regras.py:82` |
| Calculador, HE por período | Ordena períodos por saída, distribui HE do dia em ordem reversa, limita cada parcela ao trabalho do período e chama `he_noturna` com suas pontas, seus intervalos e a prorrogação do dia. `app/ponto/calculador/regras.py:875`, `app/ponto/calculador/regras.py:883` |

Os únicos chamadores diretos produtivos de `minutos_noturnos_dos_intervalos` encontrados são `he_noturna` e o wrapper de `MotorBase`. `app/ponto/motor_calculo_v2.py:404`, `app/ponto/motor_calculo_v2.py:1877`

**Diferença importante entre janela do leitor e janela do turno.**

No motor, adicional noturno usa pontas **efetivas**, depois de `_janela_do_par` e do corte dos intervalos: 12x36 em `app/ponto/motor_calculo_v2.py:2709` e `app/ponto/motor_calculo_v2.py:2742`; partido em `app/ponto/motor_calculo_v2.py:2836` e `app/ponto/motor_calculo_v2.py:2846`; comercial em `app/ponto/motor_calculo_v2.py:3161` e `app/ponto/motor_calculo_v2.py:3197`.

Já `_aplicar_he_noturna` entrega `periodo.entrada/saida` **crus**, acompanhados dos intervalos efetivos carimbados. Assim, adicional e HE podem ter envelopes diferentes, embora usem o mesmo flag. `app/ponto/motor_calculo_v2.py:2701`, `app/ponto/motor_calculo_v2.py:1790`, `app/ponto/motor_calculo_v2.py:964`

Há contraprova simples da regra “janela do turno”: turno **22–07**, intervalo **05:30–06:30**, `prorrogacao=True`, **60 minutos de HE**. O recuo encontra **05:00**; a chamada `N(05:00,07:00)` não ativa prorrogação e `he_noturna` devolve **0**, embora os 60 minutos trabalhados da ponta estejam dentro da prorrogação do turno original. Com **120 minutos de HE**, o início fica **04:00** e a função devolve **120**. Verifiquei executando em memória as quatro funções extraídas por AST; a causa está no teste estrito `inter_s1_ini < s1_fim` e na troca da âncora para `inicio_extra`. `app/ponto/motor_calculo_v2.py:109`, `app/ponto/motor_calculo_v2.py:354`, `app/ponto/motor_calculo_v2.py:402`

Isso é limitação preexistente do chamador/minuendo; o diff faz desconto e total concordarem **no recorte da HE**, mas não transporta a janela do turno original. `app/ponto/motor_calculo_v2.py:402`, `app/ponto/motor_calculo_v2.py:156`

**Alimentação do calculador.**

`segmentos_do_motor` lê pontas efetivas e intervalos carimbados, recusa `s <= e`, entrega envelopes em `pares_por_dia`, intervalos em `intervalos_por_dia`, e uma ponta HE por período com teto calculado dos segmentos. `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/alimentacao.py:127`, `app/ponto/calculador/alimentacao.py:141`, `app/ponto/calculador/alimentacao.py:185`

A prorrogação da regra diária vem do motor CCT consultado na data `k`, e segue para `regras.do_dia`. `app/ponto/calculador/alimentacao.py:445`, `app/ponto/calculador/alimentacao.py:572`, `app/ponto/calculador/alimentacao.py:596`

A releitura v3 do período chama `noturno_no_par` com **um par e seus intervalos**; este chama `noturnas_do_dia` com o flag da CCT. Pares efetivos invertidos são contados e ignorados anteriormente. `app/ponto/calculador/periodo.py:508`, `app/ponto/calculador/periodo.py:697`, `app/ponto/calculador/periodo.py:752`

**Garantias e limites upstream.**

O motor corta intervalos à janela efetiva e mantém somente interseções positivas; a função de desconto alterada não faz essa validação internamente. `app/ponto/motor_calculo_v2.py:1833`, `app/ponto/motor_calculo_v2.py:152`

A autoridade filtra intervalos com duração mínima de 15 minutos; o adaptador entrega sua resposta ao motor. A passagem pela ata pode fornecer uma lista de pausas, posteriormente cortada à janela. `app/ponto/turnos.py:497`, `app/ponto/turnos.py:543`, `app/ponto/adaptador_motor.py:44`, `app/ponto/motor_calculo_v2.py:675`

O fechamento geométrico permite envelope de **26h**, sujeito ao teto do dia e sua exceção declarada; portanto 24h não é descartado universalmente antes do cálculo. `app/ponto/turnos.py:1431`, `app/ponto/turnos.py:1442`, `app/ponto/turnos.py:1445`

O default de prorrogação do motor consulta o cadastro do ciclo; o piso implementado devolve `False` para `12x36`, enquanto CCT pode fornecer outra resposta. Portanto “12x36 sofre o defeito com prorrogação ligada” depende do cadastro; ocorrência produtiva no material dado: **[nao verificado]**. `app/ponto/motor_calculo_v2.py:720`, `app/core/regua_cct.py:166`, `app/core/regua_cct.py:182`

_1235 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
