# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 12:55** · publicado em **19/11/2026 12:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 12:55 · sessao 0972cf0af452 · CADEIA-20-PASSO-1 · construtor · origem=Code

Implementei a sincronização dos instantes com as órfãs, a ligação por ocorrência no cartório, os dois testes RED, os mutantes m13 e m14 e o ajuste da medição de consultas. A execução dos testes e dos mutantes está **[não verificada]**; foi verificada apenas a sintaxe dos cinco arquivos alterados. [nao verificado]

A causa exata do `-14 != -28` também permanece **[não verificada]**. A medição anterior reutilizava o colaborador e executava primeiro o caminho sem lavra; o código possui caches de instância e de processo que podem sobreviver ao rollback. A nova medição usa colaborador novo e aquece os dois caminhos antes das capturas, mantendo a igualdade dos acréscimos e o limite de uma chamada a CXR. A relação específica desses caches com os `-14` e `-28` não foi demonstrada. ([test_cadeia20_lavra.py:205](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:205), [colaboradores/models.py:455](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/colaboradores/models.py:455), [ausencias.py:257](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/catalogo/ausencias.py:257))

**Inventário**

- A grade ordena os pares `(hora, instante)` juntos e, na reconciliação, remove ambos pelo índice do instante identificado. Se houver mais de um par com o mesmo instante exato, a borda não é reconciliada: o transporte não distingue qual ocorrência pertence a ela. Esse caso fica sem adivinhação e sua ocorrência em produção é **[não verificada]**. ([utils.py:1867](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1867), [utils.py:2043](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2043), [utils.py:2084](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2084))
- No cartório, a k-ésima ocorrência recebe o k-ésimo instante da mesma hora; contagens divergentes geram `BATIDA_AMBIGUA`. ([cartorio.py:975](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:975))
- O RED de duas batidas reais no mesmo minuto está em [test_cadeia20_costura.py:66](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:66); o RED da reconciliação está em [test_cadeia20_costura.py:81](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:81).
- No catálogo, `m13_ambigua_por_hora` deve ser morto por `test_RED_duas_sobras_mesmo_minuto`, e `m14_instante_nao_removido` por `test_RED_reconciliacao_retira_instante_da_sobra`. Resultado de ambos: **[não verificado]**. ([test_cadeia20_mutantes.py:52](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_mutantes.py:52))

**Censo das alterações de órfãs na grade**

- Inicialização e montagem: listas vazias no intermitente e na folga; lista do turno e instantes nascidos do mesmo coletor, com acréscimos pareados nas duas pontas e nos excedentes internos. ([utils.py:1589](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1589), [utils.py:1724](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1724), [utils.py:1746](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1746), [utils.py:1758](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1758), [utils.py:1834](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1834))
- Conservação acrescenta hora e instante nos dois ramos; a ordenação posterior opera sobre os pares. ([utils.py:1859](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1859), [utils.py:1863](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1863), [utils.py:1867](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1867))
- Folga, falta, ausência integral e justificativa limpam a lista inteira; a grade limpa os instantes correspondentes após os overlays. ([utils.py:1174](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1174), [utils.py:1239](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1239), [utils.py:1274](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1274), [utils.py:1279](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1279), [utils.py:1884](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1884))
- A reconciliação remove a ocorrência identificada; o recorte posterior da grade também limpa os instantes dos dias cujas órfãs apaga. ([utils.py:2084](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2084), [utils.py:979](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:979), [utils.py:983](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:983))
- Ficaram fora os demais itens do laudo, por estarem fora do escopo solicitado. A identificação de ocorrências com instante exatamente igual permanece limitada como descrito acima. ([utils.py:2046](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2046))

Comandos para a sessão executar, **não executados aqui**:

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_costura.CosturaTest.test_RED_duas_sobras_mesmo_minuto
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_costura.CosturaTest.test_RED_reconciliacao_retira_instante_da_sobra
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_lavra.TetoConsultaLavraTest.test_mesmo_acrescimo_em_n_e_2n_dias
CADEIA20_MUTANTE=m13_ambigua_por_hora PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
CADEIA20_MUTANTE=m14_instante_nao_removido PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
```

## 19/11 12:51 · sessao 93aecad35d67 · CADEIA-20-PASSO-1 · verificador · origem=Code

**BLOQUEIA.** A cópia transporta instantes, mas a lavra volta a ligá-los às sobras por `HH:MM`: se há dois instantes na mesma hora e no mesmo dia da grade, atribui `BATIDA_AMBIGUA` a cada ocorrência sem usar o instante para distingui-las. Além disso, a grade pode retirar uma sobra depois de registrar seu instante, sem retirar esse instante da lista transportada. Isso deixa a ligação pedida sem garantia de correspondência um a um. ([app/ponto/services/cartorio.py:975](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:975); [app/escala/utils.py:1855](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1855); [app/escala/utils.py:2067](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2067))

A árvore disponível não é um repositório Git: a confirmação independente do `HEAD` e dos dois diffs por Git ficou **[não verificado]**. A análise abaixo usa os diffs fornecidos e os arquivos presentes na cópia. Não executei testes que criariam banco ou arquivos; resultados reais dos mutantes ficam **[não verificado]**. ([app/escala/tests/test_cadeia20_mutantes.py:1](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:1))

### 1. Itens procedentes da triagem

| Item | Veredito | Evidência e falta |
|---|---|---|
| **1a** | **NÃO BLOQUEIA quanto à medida; alcance da relavra [não verificado]** | A medida lê `cel.ata`; a impressão igual ainda pula células sem `forcar`, e o comando oferece `--forcar`. O dado não prova que a relavra da competência 11 ocorreu. ([cartorio.py:994](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:994); [cartorio.py:1511](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1511); [processar_cartorio.py:29](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:29)) |
| **2** | **NÃO BLOQUEIA** | A marca é calculada ao lavrar, entra em cada resposta e o teste muda a fonte e verifica a ata antiga e uma nova lavra. A costura exige ata idêntica em duas lavras forçadas; execução **[não verificado]**. ([cartorio.py:980](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:980); [test_cadeia20_lavra.py:160](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:160); [test_cadeia20_costura.py:54](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:54)) |
| **3** | **NÃO BLOQUEIA, com limite da igualdade no item 4** | O contador transitório saiu; a função única soma sobre as atas recebidas e separa legado sem chave. ([cartorio.py:994](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:994); [cartorio.py:2072](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:2072)) |
| **4** | **BLOQUEIA** | O transporte por instante existe, mas a etapa `hora → lista de instantes` e a reconciliação posterior descritas acima impedem afirmar que cada sobra recebe seu próprio instante. Falta preservar e reconciliar a associação por ocorrência até a ata. ([utils.py:1719](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1719); [utils.py:2067](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2067); [cartorio.py:976](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:976)) |
| **5** | **NÃO BLOQUEIA** | `cxr_lavrado` declara os dias da lavra e devolve `None` quando a cobertura não é conhecida; a marca preserva esse `None`. ([cxr_na_lista.py:165](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cxr_na_lista.py:165); [cartorio.py:980](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:980)) |
| **6** | **NÃO BLOQUEIA quanto à carga por colaborador** | `_cxr` inicializa na primeira consulta e memoriza; sem sobra a lavra retorna antes da consulta. Há teste de consultas para 2 e 4 dias; execução **[não verificado]**. O comando ainda busca a última lavra uma vez quando há fila. ([cartorio.py:964](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:964); [cartorio.py:1052](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1052); [test_cadeia20_lavra.py:188](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:188); [processar_cartorio.py:59](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:59)) |
| **7** | **BLOQUEIA como prova completa** | Há costura e catálogo de 12 mutantes, mas a costura força uma célula e não reproduz todas as condições do comando; tampouco prova o caso de duas ocorrências da mesma hora descrito no item 4. Resultado de execução dos mutantes **[não verificado]**. ([test_cadeia20_costura.py:31](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:31); [test_cadeia20_mutantes.py:25](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:25)) |
| **8** | **NÃO BLOQUEIA** | O diff fornecido remove o executor que alterava `sys.modules`; o novo catálogo é um `TestCase` Django. Sua execução **[não verificado]**. ([test_cadeia20_mutantes.py:51](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:51); [não verificado] quanto à conferência Git independente) |

### 2. Escritores, leitores e efeitos

- **`ata['razoes_orfas']` e `falha` — NÃO BLOQUEIA quanto à porta de escrita.** O juiz monta `hora`, `razao`, `falha` e `escala_nao_descreve`; `_lavrar_razoes_sobras` pode substituir a resposta por `BATIDA_AMBIGUA` e escreve a lista na ata. Ele é chamado nos ramos `nunca_bateu` e principal antes de `lavrar_veredito`. A medida lê `razao` e `falha`; os demais leitores diretos encontrados são testes. ([orfa_classe.py:78](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:78); [cartorio.py:987](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:987); [cartorio.py:1389](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1389); [cartorio.py:1970](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1970); [cartorio.py:1007](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1007))

- **`insumos_razao_sobras` — BLOQUEIA pela associação de instantes.** A grade escreve `turnos`, `avulsas` e `instantes`; o cartório lê os três. O dispatcher legado pode devolver grade sem a chave, caso em que a lavra retorna sem escrever razões. `grade_espelho_janela` devolve o dicionário inteiro, portanto seus chamadores recebem também a chave adicional quando o builder por turno é usado; serialização integral por algum deles permanece **[não verificado]**. ([utils.py:1925](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1925); [utils.py:2099](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2099); [utils.py:2113](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2113); [utils.py:2366](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2366); [cartorio.py:967](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:967))

- **Coletor de `_match_marcos` — NÃO BLOQUEIA quanto ao significado dos índices.** Nos dois ramos, `marcos` mapeia índice do marco para índice em `locs`; `orfas` contém índices em `locs`, não em `B` nem na lista final de órfãs. A grade lê ambos para transportar instantes. ([utils.py:623](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:623); [utils.py:642](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:642); [utils.py:676](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:676); [utils.py:1719](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1719))

- **Retorno da medida e contador removido — NÃO BLOQUEIA quanto a leitores diretos encontrados.** `medir_razoes_sobras` é consumida nos testes encontrados; não encontrei leitor de produção direto de `sobras_sem_razao` na cópia. O comando imprime outros contadores, sem esse campo. A ausência de leitores dinâmicos ou externos à cópia é **[não verificado]**. ([cartorio.py:994](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:994); [test_cadeia20_costura.py:52](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:52); [processar_cartorio.py:68](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:68))

- **Impressão, ata e consumidores indiretos — NÃO BLOQUEIA a impressão; censo completo [não verificado].** A impressão de insumos não inclui a nova marca ou a ata; por isso a chave nova não força relavra, e células com impressão igual podem ser puladas. A ata é `JSONField`, e uma ata relavrada passa a ter conteúdo adicional; comparação ou serialização da ata inteira pode, portanto, observar conteúdo diferente, mas os efeitos em todos os leitores de tela, comando, ESTADO e vigia não foram integralmente estabelecidos. ([cartorio.py:490](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:490); [cartorio.py:601](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:601); [cartorio.py:1511](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1511); [models.py:1778](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/models.py:1778))

### 3. Instante, identidade e meia-noite

**BLOQUEIA a garantia de ligação correta.** `_identidade_batida` usa `pk`, ou `id` quando falta `pk`; candidatos com o mesmo `pk` não viram ambíguos apenas por serem cópias em turno e pausa. Porém as avulsas são deduplicadas entre si, não removidas dos candidatos de turno; a classificação consulta os carimbos de todos os encontrados e escolhe o primeiro turno encontrado. Os testes cobrem cópias com o mesmo `pk` e duas batidas com `pk` distintos, mas não demonstram todos os papéis possíveis das cópias. ([orfa_classe.py:25](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:25); [orfa_classe.py:42](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:42); [orfa_classe.py:63](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:63); [test_cadeia20_lavra.py:111](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:111))

O `locs` guarda `localtime(b.timestamp)`, preservando o valor temporal e microssegundos na conversão; o pareador recebe cópias rasas das batidas carregadas, com o timestamp do objeto copiado. Portanto a comparação pretende igualdade de `datetime`, não identidade do mesmo objeto `datetime`; igualdade efetiva para todos os registros de produção é **[não verificado]**. ([utils.py:1339](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1339); [turnos.py:1984](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/turnos.py:1984); [orfa_classe.py:58](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:58))

Na fronteira, a grade pode atribuir pelo dia do turno no alinhamento e pelo dia civil na conservação S133; a reconciliação da saída cruzada procura a lista do dia seguinte. `data_turno` é recebido pelo juiz de razão, mas não filtra seus candidatos: o instante é o discriminador ali. Correção de todos os casos de fronteira é **[não verificado]**, além do defeito de associação por hora já demonstrado no código. ([utils.py:1778](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1778); [utils.py:1839](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1839); [utils.py:1997](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1997); [orfa_classe.py:30](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:30))

### 4. Universo da medida

**NÃO BLOQUEIA para o tratamento de ata antiga; BLOQUEIA se `contador_igual_universo` for tomado como prova de correspondência por célula.** `sobras_total` soma `ata['orfas']` de todas as células recebidas. Ata com órfãs e sem a chave incrementa células e órfãs sem chave e não entra em `com_razao` nem `sem_razao`. ([cartorio.py:998](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:998))

A igualdade, porém, compara **totais agregados**: uma razão excedente numa célula e uma faltante em outra podem compensar-se. Ela verifica igualdade de cardinalidades globais, não identidade, ordem ou correspondência por célula. O teste cobre uma entrada removida, não essa compensação. ([cartorio.py:1007](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1007); [cartorio.py:1015](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1015); [test_cadeia20_lavra.py:145](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:145))

### 5. Costura e mutantes

**BLOQUEIA como equivalência completa ao comando.** A costura chama `julgar_colab` com uma célula, `apply_=True`, `forcar=True` e lavra explícita. O comando passa todas as células passadas do colaborador, usa as opções de `--apply` e `--forcar`, e também passa `prefix` e `write`. Assim, a costura exercita a porta real, mas não o mesmo universo nem todos os modos do comando. ([test_cadeia20_costura.py:27](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:27); [processar_cartorio.py:38](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:38); [processar_cartorio.py:61](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:61))

**Mutantes: resultado [não verificado].** O executor exige uma ocorrência textual, executa a fonte mutada, aplica patches e exige falha com regex; isso é uma condição do teste, não prova de que os 12 passaram sem executá-los. O patch do builder altera o atributo do módulo usado pelo dispatcher do cartório, mas a referência importada diretamente em `test_cadeia20_costura` continua original: a premissa da grade usa essa referência original, enquanto a chamada do cartório passa pelo módulo alterado. Não concluo que todos os chamadores possíveis estejam cobertos. ([test_cadeia20_mutantes.py:73](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:73); [test_cadeia20_mutantes.py:84](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:84); [test_cadeia20_costura.py:10](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:10); [cartorio.py:1272](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1272))

**NÃO BLOQUEIA por duplicação:** `MutantesTest` herda os dois testes públicos de `CosturaTest`, de modo que a descoberta normal os executa também na subclasse; isso repete trabalho, mas não demonstra por si só resultado incorreto. Os testes ORM citados congelam o relógio; não encontrei `timezone.now()` direto nos arquivos novos pesquisados, mas todas as chamadas transitivas sem relógio solto ficam **[não verificado]**. ([test_cadeia20_mutantes.py:51](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:51); [test_cadeia20_costura.py:20](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:20); [test_cadeia20_lavra.py:200](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:200))

### 6. Classes estruturais

- **Segundo juiz — NÃO BLOQUEIA por duplicação demonstrada; registro [não verificado].** O juiz de razão consulta o juiz de dia sem trabalho e o papel dos turnos; a grade continua usando `_match_marcos` para o alinhamento. Não estabeleci pela leitura realizada que a nova pergunta duplique um juiz registrado, nem confirmei seu registro vivo como autoridade. ([cartorio.py:971](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:971); [orfa_classe.py:38](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:38); [utils.py:595](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:595))
- **Segundo escritor — NÃO BLOQUEIA quanto aos escritores encontrados:** a lista da ata é escrita na função de lavra, inclusive o vazio; a alteração de `falha` ocorre nessa mesma função antes da atribuição. ([cartorio.py:959](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:959); [cartorio.py:987](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:987))
- **Relógio — NÃO BLOQUEIA por leitura nova de instante demonstrada:** o juiz recebe fuso e o cartório obtém o fuso corrente; a grade recebe `agora`. Uma prova de todas as chamadas transitivas fica **[não verificado]**. ([orfa_classe.py:30](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:30); [cartorio.py:983](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:983); [utils.py:1307](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1307))
- **Fonte única e semântica — BLOQUEIA na costura das ocorrências:** os instantes são derivados de `locs` da grade, mas sua lista deixa de acompanhar a remoção de órfãs e é reagrupada por `HH:MM`. Os significados de `hora` local, instante e data da lista precisam permanecer associados até a gravação. ([utils.py:1339](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1339); [utils.py:2067](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2067); [cartorio.py:976](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:976))
- **Selo com mordida — BLOQUEIA como prova suficiente:** o catálogo pretende matar mutações nas duas chamadas, no instante, identidade, rótulos e medida; sua execução e cobertura do defeito de associação apontado acima não estão demonstradas. ([test_cadeia20_mutantes.py:25](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:25); [test_cadeia20_mutantes.py:68](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:68))

### 7. Órfãos após remoções

**NÃO BLOQUEIA para os símbolos pesquisados; censo completo [não verificado].** Não encontrei chamador de código da constante antiga `RAZOES_SEM_EXPLICACAO`, de `classificar_orfas` ou do campo removido `sobras_sem_razao`; há referências históricas em documentação. `JANELA_CLUSTER

[truncado em 20000 de 20521 caracteres]

## 19/11 12:46 · sessao e767fb2a700e · CADEIA-20-PASSO-1 · construtor · origem=Code

Implementei os testes dos três itens na cópia. **A execução dos testes, o RED e a morte dos mutantes permanecem [não verificados]**, conforme a instrução de não rodar a suíte nesta volta. [nao verificado]

- A costura usa dados ORM, chama o builder por turno e verifica a ata relida do banco e a idempotência. Há um caso separado para o ramo `nunca_bateu` ([test_cadeia20_costura.py:22](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:22)). A fixture foi adaptada de [test_juiz_dia_encerrado.py:48](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/tests/test_juiz_dia_encerrado.py:48) e [test_saida_valida_vira_orfa.py:188](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_saida_valida_vira_orfa.py:188).
- O teste de teto compara o acréscimo de consultas com a lavra ativa em 2 e 4 dias e cobra uma carga de CXR por julgamento ([test_cadeia20_lavra.py:190](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:190)). Seu resultado permanece [não verificado]. [nao verificado]
- O catálogo e o executor em memória estão em [test_cadeia20_mutantes.py:25](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_mutantes.py:25). Verifiquei estaticamente que cada alvo ocorre uma vez e que as 12 fontes mutadas compilam; a execução permanece [não verificada].

Para a sessão executar os testes: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_costura escala.tests.test_cadeia20_lavra.TetoConsultaLavraTest --noinput
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido --noinput
```

Para executar cada mutante individualmente, defina esta função e execute as chamadas desejadas:

```bash
mutante() {
  CADEIA20_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido --noinput
}
mutante m1_sem_instantes
mutante m2_sem_lavra_nunca_bateu
mutante m3_sem_lavra_principal
mutante m4_hora_no_lugar_do_instante
mutante m5_identidade_objeto
mutante m6_trabalho_em_dia_sem_trabalho
mutante m7_batida_repetida
mutante m8_intervalo_fora_da_escala
mutante m9_periodo_adicional
mutante m10_turno_aberto
mutante m11_saida_sem_entrada
mutante m12_falha_conta_com_razao
```

Inventário dos métodos que devem matar os mutantes — todos [não verificados]:

| Mutantes | Método |
|---|---|
| m1, m3 | `test_RED_sobra_gravada_e_idempotente` ([linha 37](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:37)) |
| m2 | `test_RED_nunca_bateu_grava_razoes_vazias` ([linha 60](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:60)) |
| m4 | `test_instante_distingue_turno_e_pausa_do_dia_anterior` ([linha 74](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:74)) |
| m5 | `test_RED_copias_da_mesma_batida_em_segmentos_e_turno` ([linha 111](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:111)) |
| m6–m11 | `test_razoes_e_idempotencia` ([linha 35](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:35)) |
| m12 | `test_RED_medida_usa_ata_e_prova_contagem` ([linha 134](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:134)) |

Ficou fora a demonstração executada do RED, do teto e dos mutantes: a instrução desta volta reservou a execução à sessão. Não afirmei certificação do comportamento.

## 19/11 12:43 · sessao 6448171c5edb · E4C-F1-B2b · censo · origem=Code

**AVAL-DEVOLVIDO B2B-F0:** a reconferência não fecha a abertura. A cópia não tem metadados Git, de modo que o hash, os commits posteriores e os arquivos das raias não puderam ser verificados; a tabela, o brief e `r2.txt` foram recebidos como texto, mas não estão disponíveis como arquivos com numeração de linhas. O censo AST executado localiza chamadas diretas por nome, mas não prova todas as leituras, escritas e chamadas indiretas exigidas. **Não há base para declarar a F0 pronta.** [não verificado] [nao verificado]

## 1. F0 na cópia disponível

A referência “tabela F6/F8/F9/F4” abaixo identifica a seção do anexo fornecido; a linha física da tabela no HEAD está [não verificado]. A comparação de conteúdo com 18/11 usa os trechos literais e números antigos fornecidos, não um diff Git. [não verificado]

| Instrumento | Tabela volta 8 e `r2.txt` | Cópia disponível e classificação |
|---|---|---|
| **Dias, F6** | A tabela situa a extração do bloco de dias em `fio.py:1403–1413` da cópia de 18/11; exige dias da instância **e** da linha gravada, com dois discriminadores de conversa. `r2.txt` diz que a leitura ainda estava dentro da guarda. [não verificado: linha física dos anexos] | **MUDOU DE LINHA:** a query está em [fio.py:1409](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1409), `values_list('pk', 'conversa', 'data_evento')`; os dois discriminadores e o conjunto vazio estão em [fio.py:1413](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1413). A função `_dia_de_turno_da_pergunta` já existe, mas responde apenas o dia da **instância**, não o par instância/gravado: “`return None if ... conversa ... else ... data_evento`” em [juizes.py:338](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/juizes.py:338). Não identifiquei a função completa sob outro nome; exaustividade [não verificado]. |
| **Coletor, F8** | A tabela diz “contextvar NOVO”, ao lado de `_CERCA`, usando `_ESPERAS_163` como molde; o conjunto é de pares `(pk, alvo)` e conta PKs distintos. `r2.txt` observa que o estado da cerca contém `vetadas`, `memo` e `empresa`, sem sítio. [não verificado: linha física dos anexos] | **MUDOU DE LINHA:** `_CERCA` continua com `default=None` em [fio.py:1381](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1381), e o estado citado em [fio.py:1445](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1445). O molde está em [materializacao.py:1818](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/materializacao.py:1818), antes `:1728` no anexo. Não identifiquei coletor equivalente; busca semântica exaustiva e commit [não verificado]. |
| **Retorno `RETIDA`, F9/F9b** | A tabela situa F9 depois das duas saídas sem escrita em `models.py:3239–3264` e F9b depois de `:3299–3300`; exige sentinela falsy distinguível por identidade. `r2.txt` não identifica implementação do instrumento. [não verificado: linha física dos anexos] | **MUDOU DE LINHA:** `carimbar` começa em [models.py:3299](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3299); a saída fora do fato está em [models.py:3316](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3316), e a saída C6 e a lavra em [models.py:3339](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3339). F9b está em [models.py:3376](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3376), antes da escrita `:3378`. Ambos retornam `False` ou `True` nos trechos lidos; sentinela equivalente sob outro nome [não verificado]. |
| **Trilha por pergunta, F4** | A tabela situa `registrar_retencao` em `reconciliador.py:459`, dedup em `:489` e evento em `:493`; pede estender a entidade e comparar motivo mais PKs ordenados. `r2.txt` diz que o censo de leitores e escritores era parcial. [não verificado: linha física dos anexos] | **MUDOU DE LINHA:** a função está em [reconciliador.py:465](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:465); consulta e evento continuam fixos em `'ChamadoColaborador'` em [reconciliador.py:495](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:495). O escritor genérico `evento(acao, modelo, objeto_id, ...)` já existe em [observ.py:94](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/observ.py:94), mas isso não demonstra a trilha F4 com o comparador pedido. Equivalente completo e commit [não verificado]. |

Há também uma segunda composição de dias já existente no reuso: `veto_de_competencia(..., [p.data_evento, novo_dia], ...)`; ela não é a função F6 pedida, pois usa esse par específico. [fio.py:1117](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1117).

## 2. Censo AST

O script abaixo **foi executado com `python3`**, diretamente pela entrada padrão, sem arquivo. Ele procura chamadas sintáticas pelos nomes listados e exclui testes e migrations. Não resolve aliases, despacho dinâmico, SQL, campos ORM, nem instrumentos com nomes desconhecidos; portanto **não é prova exaustiva**. As chamadas homônimas de outros domínios também aparecem e precisam ser filtradas pelo contexto. [não verificado: cobertura semântica] [nao verificado]

```python
import ast
import pathlib

names = {
    "vigiar_escrita_de_perguntas", "veto_de_competencia",
    "registrar_retencao", "carimbar", "carimbar_identidade",
    "reapontar_resolvedora", "_dia_de_turno_da_pergunta", "RETIDA",
}

for p in pathlib.Path("app").rglob("*.py"):
    if "tests" in p.parts or "migrations" in p.parts:
        continue
    try:
        tree = ast.parse(p.read_bytes(), filename=str(p))
    except (SyntaxError, UnicodeError):
        continue
    for node in ast.walk(tree):
        if isinstance(node, ast.Call):
            f = node.func
            name = (
                f.id if isinstance(f, ast.Name)
                else f.attr if isinstance(f, ast.Attribute)
                else ""
            )
            if name in names:
                print(f"{p}:{node.lineno}: {ast.unparse(node)[:160]}")
```

O censo confirmou estas **interfaces atuais**, que delimitam parte dos futuros chamadores; a atribuição de todos os sítios F1–F7 a cada instrumento e a assinatura definitiva continuam [não verificado]:

- **Dias:** a guarda tem as perguntas, consulta `pk`, `conversa` e `data_evento` gravados e passa `empresa`, dias e `memo` ao veto; o reuso tem `p` e `novo_dia`. [fio.py:1408](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1408), [fio.py:1424](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1424), [fio.py:1117](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1117).
- **Coletor e retorno:** B2a tem `p` e `_user` e trata retorno falsy com `return False`; B1a tem `p` e `_user` e faz `continue` no falsy; B3a faz o mesmo com `encerrar_fantasma`. [reconciliador.py:407](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:407), [reconciliador.py:642](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:642), [reconciliador.py:1195](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1195).
- **Retorno que precisa ser distinguido:** `retratar_duplicata` tem `p`, `canonica`, `agora`, `user`, ignora hoje o retorno de `carimbar_identidade` e devolve `True`; a tabela fornecida determina distinguir `RETIDA` nesse sítio. [fio.py:1488](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1488), [fio.py:1502](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1502); linha física da tabela [não verificado].
- **Reaponte compartilhado:** as chamadas AST incluem B2b em [reconciliador.py:1001](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1001), B2d em [reconciliador.py:1715](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1715), X8 em [models.py:449](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:449), X7 em [fio.py:2338](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:2338), X9/X10 em [limbo_folgas.py:245](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/limbo_folgas.py:245) e [limbo_folgas.py:283](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/limbo_folgas.py:283), e X12 em [reatar_beco_folga_ao_limbo.py:135](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:135). A entrada humana também chama a mesma porta em [declaracao_texto.py:371](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/declaracao_texto.py:371).
- **Trilha:** o contrato atual é `(ch, ret) -> bool`, deduplica por ação, modelo fixo e ID, e chama `evento`; os futuros alvos e tratamento por cada chamador estão [não verificado]. [reconciliador.py:465](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:465), [reconciliador.py:495](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:495).

**Prova de que ninguém liga na F0: incompleta.** Os instrumentos pedidos não foram construídos nesta leitura; não existe diff F0 para censar. O AST encontrou chamadas atuais de guarda em [models.py:3479](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3479) e [models.py:3678](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3678), e a guarda retorna fora da cerca em [fio.py:1402](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1402). Isso **não** prova ausência de chamadas a um instrumento futuro nem fornece todos os escritores e leitores dos campos. O censo completo por campo, inclusive `data_evento`, `conversa`, carimbos e `LogAuditoria`, permanece [não verificado].

## 3. Casos e mutantes para a regra da F0

A regra dada é instrumento disponível sem alteração observável enquanto desligado; os casos abaixo são especificações de teste, não testes executados. [não verificado: resultado] [nao verificado]

1. **Ligação acidental:** exercer um sítio real de produção com pergunta que o veto atingiria e outra controle; mutante liga o coletor nesse sítio. O selo precisa falhar por diferença de retorno, exceção, escrita ou trilha. A guarda atual sai sem consulta quando `_CERCA` é `None`. [fio.py:1402](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1402).
2. **Efeito colateral:** chamar cada instrumento isoladamente e comparar estado antes/depois, incluindo banco, estado de contexto e relógio; mutantes gravam, alteram estado compartilhado ou consultam relógio sem insumo explícito. O alcance de “puro” para cada instrumento ainda depende de contrato escrito [não verificado].
3. **`RETIDA` engolida:** nos chamadores que distinguem a sentinela, injetar `RETIDA` e conferir o resultado específico; mutante a converte em sucesso ou `None`. Em X6, por exemplo, o retorno atual de identidade é ignorado e a função devolve `True`. [fio.py:1502](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1502). Os chamadores que ainda não recebem `RETIDA` dependem de implementação futura [não verificado].

Para **morder**, o selo deve comparar o mesmo universo de entradas e estado inicial entre base e versão F0, com coletor desligado: retornos, exceções, consultas observáveis, alterações persistidas, trilha e estado transitório. Precisa alcançar os sítios reais da tabela e demonstrar que o mutante de ligação altera ao menos uma observação; uma comparação apenas de contagens ou de chamadas isoladas não estabelece isso. Universo fechado e resultado [não verificado].

## 4. Arquivos e cruzamentos da F0

A tabela fornecida situa os instrumentos em `fio.py` e `reconciliador.py`; os pontos F9/F9b ficam em `models.py`. Como a F0 ainda não foi construída, a lista final e os testes necessários são [não verificado]. [fio.py:1389](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1389), [models.py:3299](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3299), [reconciliador.py:465](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:465).

| Arquivo candidato | Cruzamento apurado |
|---|---|
| `app/chamados/services/fio.py` | **F5: possível; não verificado por diff da raia.** O relato registra F5 em construção e menciona `fio.py` entre leitores de texto. [RELATO.md:9](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/RELATO.md:9), [RELATO.md:20](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/RELATO.md:20). CADEIA-20 passo 1: não consta da lista fornecida; diff [não verificado]. C7 e LASTRO: [não verificado]. |
| `app/chamados/models.py` | **F5, CADEIA, C7 e LASTRO:** interseção por diff [não verificado]. O sítio F9 existe aqui em [models.py:3299](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3299). |
| `app/chamados/reconciliador.py` | **C7: sim, conforme o escopo C7 fornecido pelo usuário**, se F0 estender `registrar_retencao` como na tabela: a função está neste arquivo em [reconciliador.py:465](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:465). O caminho `app/chamados/services/reconciliador.py` citado no pedido é divergente do arquivo lido; sua existência e o diff C7 [não verificado]. F5, CADEIA e LASTRO por diff [não verificado]. |

O backlog registra o passo 3 como `LASTRO-FAIXA-SEM-DEFAULT`, com base refeita e 41 arquivos, mas o conjunto completo desses arquivos não foi disponibilizado. [BACKLOG.md:1960](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1960). O `git log` solicitado falhou porque a cópia não é um repositório Git; logo não se pode concluir que as demais interseções sejam vazias. [não verificado]

## 5. Premissas antigas e perguntas de lei

- **F7 sem dia:** o anexo do brief dizia que conferir data sem dia passaria; o registro posterior diz literalmente “`conferir data sem dia RECUSA (L5)`”. A pergunta de lei sugerida em `r2.txt`, fundada naquela passagem, não permanece aberta com essa formulação. [RELATO.md:110](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/RELATO.md:110). A porta atual recebe ID, autor e justificativa e exige divergência: [validacao.py:957](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:957), [validacao.py:990](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:990).
- **A5:** a tabela antiga descreve filtro `validada_em__isnull=True`; a cópia usa `q_nao_validada_pelo_ato()`. É mudança de conteúdo, também apontada no `r2.txt` fornecido. [models.py:315](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:315); linha física do anexo [não verificado].
- **Fila:** o backlog ainda escreve “depois de F6F7, F9, F5, F3”, enquanto o relato registra F5 em volta 3 e prova em andamento; não há Git para afirmar seu pouso posterior. [BACKLOG.md:827](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:827), [RELATO.md:9](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/RELATO.md:9).

**Perguntas de lei: 0 identificadas nesta leitura parcial; total [não verificado].** A incompletude do censo e dos diffs é lacuna de evidência, não uma pergunta de lei. [não verificado]

## 6. E4C-F8

A linha `E4C-F8 PENDENTE SEM_DIFF` do anexo indica uma entrada própria no pré-mapa, mas não demonstra identidade, absorção ou relação com o coletor F8 da F0; o relato menciona separadamente “ULTIMO-ATO-TEXTO-F8”, insuficiente para estabelecer a equivalência. [RELATO.md:20](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/RELATO.md:20); linha física do pré-mapa e conclusão [não verificado].

## 7. F1 adiantada: sítios e arquivos

A numeração física das linhas da tabela não está disponível; os endereços antigos abaixo são literais do anexo fornecido. **MUDOU** significa deslocamento verificado do trecho citado, sem afirmar equivalência integral de contexto; onde os números coincidem, a comparação integral de conteúdo está [não verificado].

| Sítio | 18/11, segundo a tabela | Cópia disponível |
|---|---|---|
| X1 / A9c | `materializacao.py:2167` | **MUDOU:** `if p.carimbar(...)` em [materializacao.py:2180](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/materializacao.py:2180). |
| A9d | `competencia_trancada.py:229` | **IGUAL em linha e chamada:** `d.fechar(...)` em [competencia_trancada.py:229](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/competencia_trancada.py:229); conteúdo integral [não verificado]. |
| B1a / B1b | `reconciliador.py:639 / :656` | **MUDOU:** `p.carimbar` em [reconciliador.py:642](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:642), `_d.fechar` em [reconciliador.py:659](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:659). |
| B2a | `reconciliador.py:407 / :414` | **IGUAL em linha e chamada:** `if not p.carimbar` e `p.disputa.fechar` em [reconciliador.py:407](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:407); conteúdo integral [não verificado]. |
| B3a / B3b | `reconciliador.py:1192 / :1216` | **MUDOU:** `encerrar_fantasma` em [reconciliador.py:1195](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1195), `d.fechar` em [reconciliador.py:1219](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1219). |
| X13 | `materializacao.py:2030` | **MUDOU:** `disputa.fechar` em [materializacao.py:2043](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/materializacao.py:2043). |
| X14 | `disputa_emissao.py:1298` | **IGUAL em linha e chamada:** `disputa.fechar` em [disputa_emissao.py:1298](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/disputa_emissao.py:1298); conteúdo integral [não verificado]. |

Os arquivos que contêm esses sítios são `materializacao.py`, `competencia_trancada.py`, `reconciliador.py` e `disputa_emissao.py`; a lista de arquivos que a implementação F1 efetivamente tocará, inclusive testes, é [não verificado]. **C7: há cruzamento de arquivo em `app/chamados/reconciliador.py`**, tomando o escopo C7 informado no pedido; os diffs de C7, F5 e LASTRO e a comparação integral com CADEIA-20 permanecem [não verificado]. [reconciliador.py:642](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:642), [BACKLOG.md:1960](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1960).

**Registro proposto, não gravado:** `B2B F0 REBRIEF: N sitios conferidos [nao verificado], M mudaram [nao verificado], arquivos K [nao verificado] (cruza C7: sim, conforme escopo informado), perguntas de lei 0 identificadas; total [nao verificado]`.

## 19/11 12:42 · sessao 04527cddf101 · CADEIA-20-PASSO-1 · construtor · origem=Code

Implementei os quatro itens na cópia. A execução dos testes e o comportamento em execução permanecem **[não verificados]**. [nao verificado]

- Os testes antigos passam pares de hora e instante ao juiz e verificam as falhas em `falha`, com `razao=None` nos casos ajustados ([test_razao_da_sobra.py:25](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:25), [test_razao_da_sobra.py:104](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:104), [test_cadeia20_coletor.py:12](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_coletor.py:12)). [nao verificado]
- A identidade usa `pk` quando existe e identidade do objeto nos demais casos ([orfa_classe.py:25](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:25), [orfa_classe.py:59](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:59)). Os testes RED cobrem cópias em segmentos e turno, e batidas distintas no mesmo instante ([test_cadeia20_lavra.py:111](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:111), [test_cadeia20_lavra.py:125](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:125)). [nao verificado]
- A medida lê `cel.ata` e devolve as contagens e a comparação com o universo; removi o contador da lavra e de `contadores()` ([cartorio.py:994](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:994), [cartorio.py:959](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:959), [cartorio.py:2072](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:2072)). O teste inclui uma ata sem a chave e um caso em que a soma não bate ([test_cadeia20_lavra.py:134](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:134)). [nao verificado]
- Apaguei `escala/tests/cadeia20_mutantes.py`; `git grep` não encontrou referência ao módulo **[verificado na cópia]**.

A análise sintática dos cinco arquivos Python alterados e `git diff --check` passaram. Não rodei a suíte, conforme solicitado. Para a sessão executar os testes escritos: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_razao_da_sobra escala.tests.test_cadeia20_coletor escala.tests.test_cadeia20_lavra.LavraDasSobrasTest --noinput
```

**Ficou fora:** a demonstração RED e a execução de mutantes **[não verificados]**. Não criei o instrumento de mutação: o item 4 reserva os mutantes para a volta seguinte no padrão indicado ([test_e4b_classe_e_memoria.py:1034](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/chamados/tests/test_e4b_classe_e_memoria.py:1034)). Assim, a entrega ainda não contém os mutantes nem seus comandos individuais.

## 19/11 12:38 · sessao c705a044ca3b · CADEIA-20-PASSO-1 · construtor · origem=Code

Implementei os três itens em código, mas **a entrega ainda não está completa nem certificada**: não consegui executar o teste unitário porque o `python3` disponível não encontra Django, e não produzi nem executei os mutantes pedidos. O comportamento em execução permanece [não verificado]. [nao verificado]

Inventário:

- O builder transporta os instantes por dia e `_match_marcos` oferece um coletor opcional de índices ([utils.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:595), [utils.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2100)).
- A lavra associa cada hora aos instantes do dia e o juiz procura por igualdade de timestamp em turnos e pausas ([cartorio.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:959), [orfa_classe.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:25)).
- As falhas passam para `falha`, com `razao=None`; acrescentei testes para identidade, ambiguidade, ausência de instante, vocabulário e marca congelada ([orfa_classe.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:75), [test_cadeia20_lavra.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:74)). [nao verificado]
- Os testes antigos chamam o juiz com strings e esperam o vocabulário anterior; precisam ser adaptados à nova interface antes da certificação ([test_razao_da_sobra.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:25), [test_cadeia20_coletor.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_coletor.py:15)). [nao verificado]

A análise sintática AST dos quatro arquivos alterados e `git diff --check` passaram. O comando abaixo falhou na importação de Django; não rodei a suíte: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra.LavraDasSobrasTest --noinput
```

Faltam o RED demonstrado, os mutantes com seus comandos e a verificação da costura completa do builder até a lavra.

## 19/11 12:35 · sessao d4d9f65fbb61 · CADEIA-20-PASSO-1 · construtor · origem=Code

O censo abaixo é da cópia indicada. **A entrega está incompleta:** implementei apenas parte da carga preguiçosa; a correção da identidade, a medida persistida, a costura e os mutantes pedidos continuam pendentes. Os defeitos que motivaram o bloqueio permanecem no código ([orfa_classe.py:36](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:36), [cartorio.py:975](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:975)).

**Censo de `orfas`:**

- `_match_marcos` monta listas de horas nos dois caminhos de alinhamento ([utils.py:631](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:631), [utils.py:673](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:673)). O builder simples também anexa e ordena sobras ([utils.py:810](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:810), [utils.py:814](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:814)).
- No builder por turno, pontas e intervalos sem coluna são anexados ([utils.py:1733](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1733), [utils.py:1744](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1744)); a lista é guardada por data ([utils.py:1769](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1769)); batidas não consumidas são anexadas e as horas ordenadas ([utils.py:1843](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1843), [utils.py:1845](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1845), [utils.py:1849](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1849)).
- O recorte de janela zera as listas futuras ou anteriores ao piso ([utils.py:973](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:973), [utils.py:977](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:977)). Os overlays zeram por folga, falta, ausência integral ou justificativa ([utils.py:1164](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1164), [utils.py:1229](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1229), [utils.py:1264](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1264), [utils.py:1269](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1269)).
- `ata_do_dia` copia as horas da grade ([cartorio.py:749](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:749), [cartorio.py:764](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:764)). `dispensar_por_ausencia` altera lâmpadas ([cartorio.py:800](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:800)); `_ata_ate_o_teto` altera lâmpadas e `n_missing` ([cartorio.py:926](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:926)).

**Censo 1b:** os chamadores diretos examinados são o cartório ([cartorio.py:1240](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1240)), `reconciliar_grade` ([reconciliar_grade.py:118](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/management/commands/reconciliar_grade.py:118)), `detectar_par_relampago` ([detectar_par_relampago.py:136](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/management/commands/detectar_par_relampago.py:136)) e o adaptador `grade_espelho_janela` ([utils.py:2350](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2350)). Este último é consumido por simulação, espelho, PDF e mapa de divergência ([simular_dia.py:7](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/simular_dia.py:7), [espelho.py:506](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/espelho.py:506), [pdf_espelho.py:543](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/relatorios/pdf_espelho.py:543), [mapa_divergencia.py:104](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/management/commands/mapa_divergencia.py:104)). Não identifiquei serialização, cache ou hash do dicionário inteiro nesses caminhos; completude do censo para chamadas indiretas é **[não verificado]**.

**Inventário:**

- Alterei `julgar_colab` para construir o leitor CXR na primeira consulta e reutilizá-lo na chamada; uma ata sem sobras já recebe lista vazia sem consultar CXR ([cartorio.py:963](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:963), [cartorio.py:1020](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1020)).
- Os dois comandos passam uma lavra carregada antes do laço; `julgar_celula` a repassa ([processar_cartorio.py:60](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/management/commands/processar_cartorio.py:60), [rejulgar_ausencia_em_folga.py:78](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78), [cartorio.py:2097](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:2097)).
- Acrescentei teste para a ata vazia não consultar CXR e ajustei a expectativa do teste de consultas existente ([test_cadeia20_lavra.py:71](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:71), [test_cadeia20_lavra.py:107](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:107)). Comando alvo: `PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra`. Resultado: **[não verificado]**; não executei a suíte nem testes com banco. [nao verificado]
- Fiz somente análise sintática AST dos quatro arquivos alterados, que passou. Os testes RED de laço e de ponta a ponta, os mutantes e seus comandos ainda não foram produzidos; `cadeia20_mutantes.py` ainda não saiu. **[não verificado]** [nao verificado]

## 19/11 12:30 · sessao 0c51c19d16d8 · CADEIA-20-PASSO-1 · verificador · origem=Code

**Veredito geral: BLOQUEIA.** Há defeitos demonstráveis na ligação da sobra à batida e na contagem por universo; a integração também não tem teste que prove que a grade entrega os insumos até a ata. O alcance em produção é **[não verificado]**. [nao verificado]

1. **BLOQUEIA — campos, leitores e compatibilidade.**

   - `ata['orfas']` é produzido por `ata_do_dia` a partir das horas da grade e lido pela nova lavra; o leitor existente de cluster também o lê junto das lâmpadas. [cartorio.py:749](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:749), [cartorio.py:963](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:963), [cluster_espurio.py:27](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cluster_espurio.py:27).
   - `razoes_orfas` é escrito no dicionário temporário pela nova função e gravado pela porta de veredito; a comparação da porta vê a chave nova. Não identifiquei leitor de produção que interprete essa chave isoladamente. [cartorio.py:982](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:982), [celula.py:1049](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/portas/celula.py:1049). A primeira relavra de uma ata antiga pode, portanto, ser reconhecida como mudança da ata e atualizar o veredito. [celula.py:1058](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/portas/celula.py:1058), [celula.py:1090](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/portas/celula.py:1090).
   - `sobras_sem_razao` nasce em `contadores` e é incrementado pela lavra; o uso de produção encontrado é essa própria acumulação, sem leitor posterior identificado. [cartorio.py:2033](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:2033), [cartorio.py:964](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:964).
   - A grade escreve `insumos_razao_sobras`; os dois ramos do cartório a leem. A chave contém turnos e batidas, não apenas valores JSON. O parâmetro `avulsas` é uma lista coletora preenchida pelo pareador por segmento e consumida pela lavra. [utils.py:1909](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/utils.py:1909), [utils.py:2083](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/utils.py:2083), [turnos.py:2000](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/turnos.py:2000), [cartorio.py:972](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:972).
   - `escala_nao_descreve` é calculado pela pertinência da data à resposta `cxr`, passado ao juiz e copiado em cada razão; leitor posterior específico **[não verificado]**. [cartorio.py:973](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:973), [orfa_classe.py:78](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:78).

   A **impressão dos insumos** não inclui a ata nem a marca CXR: atas antigas podem continuar puladas, e uma mudança apenas da marca não força atualização. [cartorio.py:490](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:490), [cartorio.py:1456](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1456), [cartorio.py:1471](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1471). A ata recebe somente horas, razões e `bool`/`None`, não os objetos transportados, e seu campo é JSON. [orfa_classe.py:78](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:78), [models.py:1778](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/models.py:1778). Um consumidor que serialize **a grade inteira** passa a receber objetos não serializáveis pelo JSON padrão; existência de consumidor que o faça e efeito em impressão ou hash de ata integral: **[não verificado]**. [utils.py:2083](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/utils.py:2083), [turnos.py:1868](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/turnos.py:1868).

2. **BLOQUEIA — idempotência não demonstrada de ponta a ponta.** Para entradas idênticas, o juiz percorre as sobras na ordem recebida; a grade inicialmente ordena as horas, e o teste compara duas chamadas diretas com os mesmos insumos. [orfa_classe.py:54](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:54), [utils.py:1847](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/utils.py:1847), [test_cadeia20_lavra.py:32](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/tests/test_cadeia20_lavra.py:32). Isso não prova duas relavras completas com o mesmo estado: a marca depende da última lavra CXR, que pode mudar independentemente da impressão, e a comparação da porta inclui o conteúdo novo. [cartorio.py:1016](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1016), [celula.py:1058](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/portas/celula.py:1058). Ordem final estável em todos os caminhos da grade: **[não verificado]**.

3. **BLOQUEIA — cobertura e contador.** Os dois pontos novos antecedem as duas lavras do cartório que constroem e passam uma ata nova com `orfas`; `ata_do_dia` também é chamada para uma ata temporária usada no julgamento. [cartorio.py:1308](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1308), [cartorio.py:1349](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1349), [cartorio.py:1495](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1495), [cartorio.py:1928](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1928). A conclusão de que **nenhum outro chamador**, em toda a cópia, grava ata com `orfas` sem razões é **[não verificado]**.

   O contador não equivale ao universo final: uma segunda passagem forçada usa o mesmo `out` e pode contar a mesma sobra duas vezes; a releitura pós-emissão usa outro contador e não incorpora seu `sobras_sem_razao` ao original. [cartorio.py:1073](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1073), [cartorio.py:1140](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1140). Além disso, `apply_=False` e células puladas não passam pela lavra; no caminho legado sem insumos contam-se as sobras, mas não se grava a chave. [cartorio.py:964](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:964), [cartorio.py:1471](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1471), [cartorio.py:1927](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1927).

4. **BLOQUEIA — dia e bordas.** A grade atribui sobras não consumidas pela **data civil** da batida; o juiz restringe turnos por `data_turno`, mas compara candidatos somente por `HH:MM`. Assim, uma cauda civil de D+1 pertencente ao turno D pode ser considerada para uma sobra de D com a mesma hora, enquanto a sobra civil de D+1 pode não encontrar esse turno. [utils.py:1824](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/utils.py:1824), [orfa_classe.py:38](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:38).

   Nas pausas há um erro adicional: cada segmento recebe batidas com margem, mas a lavra seleciona o **segmento** que contém D, sem filtrar a data da batida; o juiz novamente usa só a hora. Uma pausa de D−1 ou D+1 pode casar indevidamente com uma sobra de D, ou torná-la ambígua. [turnos.py:1977](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/turnos.py:1977), [turnos.py:2000](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/turnos.py:2000), [cartorio.py:972](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:972), [orfa_classe.py:44](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:44). O pad carrega fatos além da borda, mas o retorno de turnos é limitado ao período pedido; a propriedade em todas as bordas **[não verificado]**. [turnos.py:1847](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/turnos.py:1847), [turnos.py:2004](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/turnos.py:2004).

   O juiz novo não recebe `agora` nem exige turno encerrado e pode rotular `TURNO_ABERTO`; se esse rótulo for tratado como conclusão sobre fato encerrado, o teto exigido precisa ser resolvido. A violação prática de um emissor ou juiz de marco por esse novo campo é **[não verificado]**. [orfa_classe.py:26](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:26), [orfa_classe.py:70](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:70), [CLAUDE.md:188](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/CLAUDE.md:188).

5. **BLOQUEIA — equivalência CXR incompleta.** A chave usa corretamente o início de `janela_atual`, e `None` é preservado até a razão. [cartorio.py:973](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:973), [cxr_na_lista.py:182](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cxr_na_lista.py:182). Porém, a lavra cobre somente competências medidas e descarta dias publicados fora da competência pedida; `InsumosDoDono.cxr()` lê os dias publicados pelo espelho. Logo não se pode afirmar igualdade de universo nem de instante da resposta. [cxr_na_lista.py:24](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cxr_na_lista.py:24), [cxr_na_lista.py:90](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cxr_na_lista.py:90), [e6_oraculo.py:162](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/management/commands/e6_oraculo.py:162). Neste caminho, a marca é derivada da lista lavrada; não encontrei chamada ao motor para recalculá-la. [cartorio.py:1016](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1016).

6. **BLOQUEIA — custo por chamada, repetido em laços.** `ultimo_lavrado()` faz uma consulta e `cxr_lavrado` constrói seus mapas uma vez por chamada de `julgar_colab`, inclusive antes de saber se haverá lavra; as passagens internas reutilizam `_cxr`. [cartorio.py:1016](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1016), [cxr_na_lista.py:152](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cxr_na_lista.py:152), [cartorio.py:1031](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:1031). O lote recarrega por colaborador, e há chamador que percorre dias chamando `julgar_celula`, que por sua vez chama `julgar_colab`: nesse caminho recarrega por dia-colaborador. [processar_cartorio.py:59](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/management/commands/processar_cartorio.py:59), [rejulgar_ausencia_em_folga.py:77](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/management/commands/rejulgar_ausencia_em_folga.py:77), [cartorio.py:2117](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:2117). O teste N/2N cobre dias dentro de **uma** chamada, em dry-run; não mede esses laços. [test_cadeia20_lavra.py:92](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/tests/test_cadeia20_lavra.py:92).

7. **BLOQUEIA — semântica e mordida da integração.** A razão reutiliza a ideia de rajada de 15 minutos, mas a pergunta existente de cluster também exige lâmpada apagada e exclui dia sem trabalho; não são predicados idênticos, portanto clonagem exata do juiz registrado é **[não verificado]**. [orfa_classe.py:13](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:13), [cluster_espurio.py:15](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cluster_espurio.py:15), [juizes.py:1224](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/core/juizes.py:1224). A persistência continua pela porta; `get_current_timezone()` obtém configuração de fuso, não o instante do relógio. [cartorio.py:979](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:979), [celula.py:1032](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/portas/celula.py:1032).

   O bloqueio de fonte e semântica está na ligação por hora e dia descrita no item 4: a razão usa a lista da grade, mas procura novamente sua batida em outro conjunto sem identidade nem data civil suficiente. [cartorio.py:963](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/ponto/services/cartorio.py:963), [orfa_classe.py:36](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/orfa_classe.py:36). Os testes da lavra passam insumos fabricados diretamente, e o teste do coletor chama o corpo diretamente; remover a ligação da grade ou uma chamada de lavra não é coberto por esses testes. [test_cadeia20_lavra.py:23](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/tests/test_cadeia20_lavra.py:23), [test_cadeia20_coletor.py:17](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/tests/test_cadeia20_coletor.py:17). Os mutantes alteram rótulos do juiz, não a costura. [cadeia20_mutantes.py:26](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/tests/cadeia20_mutantes.py:26).

8. **BLOQUEIA — importação do auxiliar é perigosa; quebra efetiva de contrato é [não verificado].** O arquivo executa código no topo, substitui `django`, `django.test` e `ponto.turnos` em `sys.modules`, usa caminhos relativos ao diretório corrente e interpreta `sys.argv`; importá-lo num processo de testes pode contaminar o processo ou falhar. [cadeia20_mutantes.py:10](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/tests/cadeia20_mutantes.py:10), [cadeia20_mutantes.py:34](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/escala/tests/cadeia20_mutantes.py:34). Encontrei contrato que varre todos os `.py` de `tests`, mas ele os analisa por AST e só importa os nomes marcados, não esse auxiliar; outro varredor seleciona nomes iniciados por `test`. Portanto não atribuo a eles uma quebra comprovada. [test_marca_cron15.py:25](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/core/tests/test_marca_cron15.py:25), [test_marca_cron15.py:60](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/core/tests/test_marca_cron15.py:60), [test_selo_teste_sem_relogio.py:48](/home/ronald/codex_ws/ab61d58e51fafd04f6a51faa58589fd060742eb2/app/core/tests/test_selo_teste_sem_relogio.py:48).

Não executei a suíte: seus resultados são **[não verificado]**. A cópia disponível não contém metadados Git, então a comparação com a base foi feita pelo diff fornecido; integridade do hash da cópia frente ao commit é **[não verificado]**. [nao verificado]

## 19/11 12:25 · sessao 38b21ec9d394 · CADEIA-20-PASSO-1 · construtor · origem=Code

**A volta 2 permanece incompleta e não certificada.** A implementação e os testes foram escritos, mas o ambiente disponível não tem `django.core`, então não foi possível executar os REDs de lavra nem o teste de teto de consultas [não verificado]. A relavra e as medidas de sombra também não foram executadas [não verificado]. [nao verificado]

A grade agora transporta os turnos e as pausas avulsas por segmento ([utils.py:1909](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1909), [utils.py:2084](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2084)). A lavra seleciona as pausas do segmento que contém o dia atribuído pela grade, grava `razoes_orfas` ao lado de `orfas` e conta sobras sem razão; na grade legada, não grava a chave de razões ([cartorio.py:959](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:959)). A lavra de CXR é carregada uma vez por chamada quando não é fornecida ([cartorio.py:1019](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1019)).

Os testes novos cobrem as seis razões no encaixe, idempotência da função de lavra, lista vazia, legado, marca desconhecida, seleção do segmento dono e teto para duas janelas ([test_cadeia20_lavra.py:32](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:32), [test_cadeia20_lavra.py:82](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:82)). **Esses testes não foram executados**, nem foi demonstrado que ficam vermelhos na base anterior [não verificado]. O comando tentou executá-los, mas falhou na importação do Django: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra escala.tests.test_cadeia20_coletor escala.tests.test_razao_da_sobra --settings=config.settings.base --noinput
```

Executei isoladamente os seis mutantes das razões do **juiz existente**, com substituições em memória e dependências mínimas: cada teste indicado no executor matou seu mutante ([cadeia20_mutantes.py:26](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/cadeia20_mutantes.py:26)). Isso não valida mutantes do transporte ou da lavra recém-alterados [não verificado]. Para reproduzir todos ou um ramo: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/cadeia20_mutantes.py
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/cadeia20_mutantes.py NOME_DA_RAZAO
```

Inventário desta volta: [utils.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1909), [cartorio.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:959), [test_cadeia20_lavra.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:1) e [cadeia20_mutantes.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/cadeia20_mutantes.py:1). A sintaxe desses arquivos e `git diff --check` passaram; não houve commit [não verificado por certificador]. [nao verificado]

## 19/11 12:23 · sessao 9cc17ec5e76e · E4D · censo · origem=Code

**Perguntas de lei: 0**, conforme o aval V2 fornecido, que declara aplicar leis existentes e fornece inclusive a frase para o caso de se exigir corte literal para o novo ato [não verificado]. A condição que não fecha hoje é técnica: a porta limpa a projeção, mas a leitura da sequência não reconhece um ato de saneamento; também não achei uma função de replay que grave novamente a projeção a partir do log de atos. [validacao.py:428](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:428), [models.py:4295](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4295).

O aval V2 fornecido substitui integralmente o brief anterior: determina um ato genérico de saneamento com a lei em campo, criado na D3 antes da limpeza das duas elegíveis; as 109 entregues, 8.709 validadas e 804 materializadas ficam intactas [não verificado]. As contagens, os dois PKs e a ausência de limpezas são **dados da sonda fornecida**, não medições que eu tenha repetido [não verificado]. O código contém a lista fechada `(38251, 38252)`. [validacao.py:378](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:378).

### 1. Censo dos leitores de tipo

O script abaixo faz uma **varredura AST de candidatos**, não uma prova automática de tipos Python: ele procura atributos, nomes de constantes, palavras-chave ORM, subscritos, `getattr`, `match` e literais de tipos em todo `app/`, inclusive quando a classe foi importada com apelido. Ele pode trazer falsos positivos de outros modelos e não consegue provar a origem de valores propagados dinamicamente entre funções; por isso a atribuição e as consequências da tabela foram examinadas no código. O script é entregue como texto; não foi criado arquivo nem executado [não verificado].

```python
#!/usr/bin/env python3
"""Candidatos AST a leitores/escritores do tipo de DecisaoPergunta.

Executar da raiz da copia. Somente abre arquivos para leitura e escreve stdout.
Saida ampla de proposito: exige revisao humana da origem de cada valor.
"""
import ast
from pathlib import Path

RAIZ = Path("app")
EXCLUIR = {"tests", "migrations", "__pycache__"}
CAMPOS = {"ato", "ato__in"}
CONSTANTES = {
    "RESPOSTA", "VALIDACAO", "DEVOLUCAO", "RECUSA", "REABERTURA",
    "SILENCIO", "RETRATACAO", "ABSORCAO", "ATOS", "ATOS_COM_FOTO",
    "_LIMPAM_VALIDACAO", "_LIMPAM_RESPOSTA",
}
LITERAIS = {
    "resposta", "validacao", "devolucao", "recusa", "reabertura",
    "silencio", "retratacao", "absorcao",
}


def partes(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return partes(no.value) + "." + no.attr
    return ""


class Censo(ast.NodeVisitor):
    def __init__(self, caminho):
        self.caminho = caminho
        self.escopo = []
        self.achados = set()

    def registrar(self, no, motivo):
        self.achados.add((
            no.lineno,
            ".".join(self.escopo) or "<modulo>",
            motivo,
            ast.unparse(no).replace("\n", " "),
        ))

    def visit_ClassDef(self, no):
        self.escopo.append(no.name)
        self.generic_visit(no)
        self.escopo.pop()

    def visit_FunctionDef(self, no):
        self.escopo.append(no.name)
        self.generic_visit(no)
        self.escopo.pop()

    visit_AsyncFunctionDef = visit_FunctionDef

    def visit_ImportFrom(self, no):
        for alias in no.names:
            if alias.name == "DecisaoPergunta":
                self.registrar(no, "import da classe / apelido")
        self.generic_visit(no)

    def visit_Attribute(self, no):
        if no.attr == "ato" or no.attr in CONSTANTES:
            self.registrar(no, "atributo ato ou constante")
        self.generic_visit(no)

    def visit_Name(self, no):
        if no.id in CONSTANTES:
            self.registrar(no, "nome de constante")
        self.generic_visit(no)

    def visit_keyword(self, no):
        if no.arg in CAMPOS:
            self.registrar(no.value, "keyword ORM ou construtor: " + no.arg)
        self.generic_visit(no)

    def visit_Subscript(self, no):
        if isinstance(no.slice, ast.Constant) and no.slice.value == "ato":
            self.registrar(no, "subscrito ato")
        self.generic_visit(no)

    def visit_Call(self, no):
        if (partes(no.func) == "getattr" and len(no.args) >= 2
                and isinstance(no.args[1], ast.Constant)
                and no.args[1].value == "ato"):
            self.registrar(no, "getattr ato")
        self.generic_visit(no)

    def visit_Match(self, no):
        self.registrar(no, "match: revisar sujeito e padroes")
        self.generic_visit(no)

    def visit_Constant(self, no):
        if isinstance(no.value, str) and no.value in LITERAIS | CAMPOS:
            self.registrar(no, "literal: revisar contexto")
        self.generic_visit(no)


for caminho in sorted(RAIZ.rglob("*.py")):
    if any(parte in EXCLUIR for parte in caminho.parts):
        continue
    try:
        fonte = caminho.read_text(encoding="utf-8")
        arvore = ast.parse(fonte, filename=str(caminho))
    except (OSError, UnicodeError, SyntaxError) as exc:
        print(f"ERRO {caminho}: {exc}")
        continue
    visitante = Censo(caminho)
    visitante.visit(arvore)
    for linha, escopo, motivo, expressao in sorted(visitante.achados):
        print(f"{caminho}:{linha}\t{escopo}\t{motivo}\t{expressao}")
```

Na coluna “conhecer”, **sim** significa que o comportamento exigido depende de tratamento; **ignorar** significa que não deve atribuir ao saneamento o significado dos tipos que seleciona. A decisão de desenho sobre o novo ato vem do aval fornecido [não verificado].

| Local e função | Leitura e comportamento atual se chegar o tipo novo | Conhecer? |
|---|---|---|
| [models.py:1692](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:1692), `ressuscitar_perguntas` | Procura a linha `VALIDACAO` correspondente ao instante vigente para recuperar o autor; o novo tipo não casa. | Ignorar nessa busca. |
| [models.py:3551](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3551), `q_validada_pelo_ato` | Alias `D`; seleciona `VALIDACAO` e atos que a limpam. O novo tipo não é considerado limpador nem validação. | Ignorar **para os casos elegíveis desta limpeza**, que já não têm validação; efeito para outros saneamentos genéricos [não verificado]. |
| [models.py:3802](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3802), `respostas_anteriores` | Trata `RESPOSTA` e `ATOS_COM_FOTO`; o novo tipo passa sem alterar `vigente` nem a lista. | Precisa ser avaliado como efeito de limpeza: deixar `vigente` anterior pode alterar quais respostas posteriores aparecem no histórico. A apresentação desejada não está especificada [não verificado]. |
| [models.py:4203](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4203), `DecisaoPergunta.Meta` | Restrições distinguem absorção, foto, marca de ressuscitação e resposta que mantém validação. Para outro tipo, exigem respectivamente vínculos de absorção e foto nulos e as duas marcas falsas. | Conhecer no desenho do esquema: um campo de lei deve ser definido; não se pode reutilizar os campos protegidos com valores proibidos. |
| [models.py:4221](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4221), `es_da_ressuscitacao` | Só reconhece `REABERTURA` marcada; saneamento retorna falso. | Ignorar como ressuscitação. |
| [models.py:4255](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4255), `linhas_da_projecao_sem_linha` | Busca correspondências somente de resposta e validação; o novo ato não impede a fotografia dessas linhas. | Ignorar como correspondência de resposta/validação. A ordem da fotografia antes de limpar continua relevante. |
| [models.py:4292](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4292), `_resposta_desfaz_validacao` | Só uma `RESPOSTA` com a marca falsa satisfaz o predicado; saneamento retorna falso. | Ignorar como resposta. |
| [models.py:4309](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4309), `leitura` | Monta sequência real mais passivo da projeção; só resposta, validação e os conjuntos de limpeza mudam os três valores. O novo ato passa sem efeito, deixando a resposta anterior vigente na leitura. | **Sim:** para L-130, precisa interpretar o ato como par zerado. Essa função lê, mas não grava a projeção. |
| [tarefa_do_dia.py:142](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/tarefa_do_dia.py:142), `_pela_decisao` | Alias de import `D`. Se saneamento for o último ato recebido, cai em `ato_desconhecido:<tipo>, None`, não em `devolvida_ao_colaborador`. | **Deve ignorá-lo na escolha da decisão para a fila**, conforme o aval; hoje não o ignora. O resultado exato após filtragem depende dos atos anteriores. |
| [tarefa_do_dia.py:262](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/tarefa_do_dia.py:262), `_tarefa_pura` | Se o último ato for literalmente `retratacao`, exclui a pergunta; saneamento posterior faz esse teste ser falso. | Precisa que a sequência fornecida respeite a decisão de ignorar saneamento na fila; caso contrário pode encobrir uma retratação anterior. |
| [tarefa_do_dia.py:390](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/tarefa_do_dia.py:390), carga dos fatos | Remove apenas ressuscitação marcada e identifica `RETRATACAO` posterior ao horizonte. O saneamento entra na lista quando está no horizonte; depois dele não conta como retratação. | **Conhecer para excluí-lo da sequência usada pela fila**, preservando a leitura apropriada da sequência para os demais fins. |
| [backfill_decisoes_pergunta.py:63](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/backfill_decisoes_pergunta.py:63), `linhas_a_criar` | Dicionário indexado pelos tipos gerados por `linhas_da_projecao_sem_linha`, resposta e validação; não indexa diretamente todos os atos reais. | Ignorar enquanto aquela função continuar gerando apenas esses dois tipos; se passar a gerar outro, a chave não declarada pode falhar. |
| [backfill_decisoes_pergunta.py:109](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/backfill_decisoes_pergunta.py:109), `contador_validada_projetada_sem_linha` | Conta somente dicionários de validação gerados pela fotografia. | Ignorar como validação. |
| [backfill_decisoes_pergunta.py:121](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/backfill_decisoes_pergunta.py:121) e [linha 342](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/backfill_decisoes_pergunta.py:342), `admin_decidiu_sem_ato` e `aplicar` | Consultam especificamente existência de `SILENCIO` ou `VALIDACAO`; saneamento sozinho não satisfaz o filtro. | Ignorar como esses atos; consequência operacional em outros estados de backfill [não verificada]. |

O catálogo atual tem oito tipos, `choices=ATOS`, e não tem campo de lei declarado; `choices` e as restrições existentes precisam ser considerados na implementação, sem presumir que já representem o novo ato. [models.py:4152](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4152).

**Limite do censo:** a tabela cobre os pontos identificados pela leitura e buscas textuais de `ato`, constantes e usos de `DecisaoPergunta`; o script AST acima não foi executado, portanto não posso afirmar que sua saída foi reconciliada integralmente com a tabela [não verificado].

### 2. Escritores de `DecisaoPergunta`

O ponto central observado é `registrar`, que faz `bulk_create` dentro de `transaction.atomic()`; o `save` do modelo permite inserção sem PK e recusa atualização de linha existente. [models.py:4227](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4227), [models.py:4329](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4329).

Chamadas de produção encontradas à porta de escrita:

- Modelo: [models.py:1654](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:1654), [1700](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:1700), [2087](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:2087), [2219](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:2219) e [3211](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3211).
- Validação: [validacao.py:173](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:173), [279](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:279), [354](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:354) e [439](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:439).
- Emissão e respostas: [disputa_emissao.py:1532](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/disputa_emissao.py:1532), [1639](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/disputa_emissao.py:1639), [1686](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/disputa_emissao.py:1686) e [1708](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/disputa_emissao.py:1708).
- Outros: [materializacao.py:1061](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/materializacao.py:1061), [veredito_celula.py:195](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/veredito_celula.py:195), [backfill_fio_morto_e4b.py:141](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/backfill_fio_morto_e4b.py:141) e [backfill_decisoes_pergunta.py:351](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/backfill_decisoes_pergunta.py:351).

Não encontrei outra chamada direta de `.objects.create` ou `bulk_create` para essa classe na busca textual; escritas indiretas ou dinâmicas não são descartadas por essa busca [não verificado].

### 3. Porta e comando atuais

`devolver_instante_sem_texto_l130` trava e relê a pergunta dentro de um `atomic`, consulta `impedimento_estoque_l130`, fotografa eventual resposta/validação sem linha e chama `registrar` para essas linhas. Depois zera `respondida_em` e `resposta_colab` pela porta de lote; essa porta também grava `materializada_em=None`, `materializada_por=None` e `via_resolucao=''`. A elegibilidade já recusa uma pergunta com `materializada_em` ou `validada_em` preenchido. [validacao.py:381](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:381), [validacao.py:428](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:428), [models.py:3484](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:3484).

A porta registra o evento `l130_instante_sem_texto_devolvida`, com antes/depois do par e da via, ator e IP; `evento` escreve `LogAuditoria`, e retorno `None` causa exceção dentro do atomic. Hoje a linha de decisão ali é apenas a fotografia eventualmente faltante, não um ato de limpeza. [validacao.py:434](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:434), [validacao.py:449](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:449), [observ.py:94](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/observ.py:94).

O lugar concreto para o ato compensador é a chamada a `DecisaoPergunta.registrar` **depois da fotografia e dentro do atomic da porta**, junto da mudança da pergunta e antes de concluir a transação; `registrar` tem atomic próprio aninhado. A ordem e o instante do novo ato precisam preservar o replay ordenado por `(ato_em, pk)`. [validacao.py:436](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:436), [models.py:4308](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4308), [models.py:4337](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4337).

O comando é DRY por padrão. Com `--apply`, exige autor staff e conjunto elegível exatamente igual à lista, grava antes um JSON de reversão e envolve **as duas chamadas** em atomic externo, com travas e releitura; divergência ou contagem incompleta desfaz as escritas do banco. O arquivo de reversão é criado antes desse atomic e, portanto, não é revertido por rollback do banco. [estoque_l130_instante_sem_texto.py:35](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:35), [linha 54](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:54), [linha 57](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:57), [linha 69](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:69).

### 4. Replay

Não achei função de produção que **reconstitua e grave** a projeção de `PerguntaDisputa` apenas a partir do log de atos [não verificado]. A função mais próxima, `DecisaoPergunta.leitura`, calcula valores em memória e ainda incorpora o passivo da própria projeção; ela não é, portanto, um replay puro do log. Hoje ela ignora um tipo novo e conserva o efeito da resposta anterior. [models.py:4295](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4295), [models.py:4317](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:4317).

Logo, o caso “replay reproduz pendente” precisa de uma função de replay/projetor identificada ou construída na obra e de tratamento do novo ato; atribuí-lo hoje a uma função existente que grave a projeção seria [não verificado]. O aval fornecido determina replay do log sem saneamento embutido e reprodução das entregues como estão [não verificado].

### 5. Casos RED pela regra

| Caso | Função real a exercitar e resultado exigido |
|---|---|
| Limpar uma vez | `devolver_instante_sem_texto_l130`: elegível retorna `True`, par fica `(None, '')`, evento de auditoria existe e deve existir o novo ato com lei L-130. A última parte ainda não existe hoje. [validacao.py:408](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:408) |
| Limpar duas vezes | A mesma porta: segunda chamada retorna `None`, pois `impedimento_estoque_l130` deixa de reconhecer instante sem texto; exigir uma única linha do **novo ato** e uma única linha do evento de limpeza, distinguindo-as da possível fotografia `RESPOSTA`. [validacao.py:390](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:390), [validacao.py:432](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/validacao.py:432) |
| Replay reproduz pendente | Exercitar o futuro projetor/replay real após resposta antiga e saneamento; exigir par zerado. Hoje `DecisaoPergunta.leitura` é a função real disponível para testar leitura, mas não grava projeção e ignora o novo ti

[truncado em 20000 de 22339 caracteres]

## 19/11 12:21 · sessao ca4b76e89fd7 · CADEIA-20-PASSO-1 · construtor · origem=Code

**A volta 2 não está concluída.** A razão ainda não é gravada na ata; faltam o transporte pela grade, a leitura da lavra em `julgar_colab`, os REDs i–vi completos, o teto de consultas e os mutantes por razão. Não houve certificação nem medidas de sombra [não verificado].

O encaixe iniciado passa o coletor opcional pela casca e pelo corpo de turnos, com cópias de pausas por segmento ([turnos.py:1850](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1850), [turnos.py:1997](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1997)). A marca desconhecida agora preserva `None` ([orfa_classe.py:76](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:76)). Acrescentei testes para esses dois pontos, mas o teste do coletor ainda não prova a seleção da cópia pelo dia dono ([test_cadeia20_coletor.py:12](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_coletor.py:12)). [nao verificado]

A implementação restante precisa conservar o instante da sobra também nos caminhos em que a grade guarda apenas a hora, inclusive no alinhamento de marcos ([utils.py:673](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:673), [utils.py:1733](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1733)). Não concluí esse transporte.

A sintaxe dos três arquivos passou na análise com `ast.parse`; `git diff --check` também passou [não verificado por certificador]. O comando Django abaixo não executou os testes: `python3` disponível não tem Django instalado. Nenhum mutante foi executado. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_coletor escala.tests.test_razao_da_sobra --settings=config.settings.base --noinput
```

Arquivos tocados: [orfa_classe.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:76), [turnos.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1850) e [test_cadeia20_coletor.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_coletor.py:12). Não fiz commit, conforme a proibição expressa no pedido. Perguntas de lei: **[não verificado]**. [nao verificado]

## 19/11 12:19 · sessao 8d62a2ce206d · CADEIA-20-PASSO-1 · construtor · origem=Code

**PAREI: dois encaixes precisam de decisão antes da cura.** A base da cópia confere com o hash informado: `8ba222d1bb9d1b886ce79ddd9d630008cdcb76a4` [verificado pelo Git].

1. **Coletor de pausas.** A autorização limita a alteração à casca `turnos_do_colab`, mas ela delega ao corpo `_turnos_do_colab_puro` ([turnos.py:1920](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1920)). É nesse corpo que existem os segmentos, as cópias carimbadas e o resultado do pareador ([turnos.py:1962](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1962), [turnos.py:1988](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1988), [turnos.py:1997](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1997)). A casca recebe só os turnos retornados. **Autoriza passar o coletor também por `_turnos_do_colab_puro`, preservando seu comportamento padrão e mantendo `parear_turnos` intocado?**

2. **Marca CXR e teto de consultas.** `cxr_lavrado` sem uma lavra fornecida chama `ultimo_lavrado`, que consulta `MetricaSnapshot` ([cxr_na_lista.py:152](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:152), [cxr_na_lista.py:174](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:174)). Não encontrei essa lavra no fluxo de `julgar_colab` [não verificado além da busca no arquivo; entrada em [cartorio.py:959](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:959)]. A instrução pede carregá-la uma vez e também que o teto de consultas não suba. **De onde deve vir a lavra já carregada, ou a consulta adicional única por execução está autorizada com o teto ajustado?**

Os três usos de produção de `ata_do_dia` no cartório estão no ramo de quem nunca bateu, na ata deste julgamento e na lavra ([cartorio.py:1280](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1280), [cartorio.py:1466](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1466), [cartorio.py:1899](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1899)). A grade já obtém os turnos pareados ([utils.py:1909](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1909)); o juiz do tipo do dia lê a ata que lhe é passada ([juizes.py:445](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/chamados/juizes.py:445)).

Arquivos tocados: **nenhum** [verificado por `git status --short`]. Cura, REDs, mutantes, prova dos consumidores, medidas na sombra e impacto em dinheiro: **[não verificado]**. Perguntas de lei: **[não verificado]**; as duas perguntas acima tratam de autorização de encaixe e alimentação.

## 19/11 12:16 · sessao 729f50e6638a · CADEIA-20-PASSO-1 · construtor · origem=Code

**PAREI: a pausa avulsa carimbada e atribuída ao dia ainda não chega à grade.** Preciso de uma decisão sobre como transportá-la e atribuí-la antes de escrever a cura.

**Aval devolvido, revisão por item:**

1. A base confere com o hash informado: `8ba222d1bb9d1b886ce79ddd9d630008cdcb76a4`. [git HEAD; verificado na cópia]
2. A decisão recebida resolve a origem da marca pela porta `cxr_lavrado`, que distingue falta de cobertura de lista vazia ([cxr_na_lista.py:165](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:165)). O juiz atual, porém, converte a marca com `bool`, portanto precisará preservar o valor desconhecido ([orfa_classe.py:75](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:75)).
3. O encaixe literal das pausas não fecha: `parear_turnos` carimba a instância, mas, sem turno corrente, não a acrescenta a um turno ([turnos.py:1365](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1365)). O wrapper cria cópias por segmento e retorna apenas turnos, descartando as cópias avulsas ([turnos.py:1984](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1984), [turnos.py:1997](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1997)). O juiz exige pausas **já atribuídas ao dia** ([orfa_classe.py:23](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:23)). A grade usa também a data civil em um caminho de sobras ([utils.py:1825](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1825)); isso, por si só, não demonstra a atribuição pedida para todas as pausas.
4. Colisão com outros avais, impacto em dinheiro, exportação ou `!`: **[não verificado]**.

Os três chamados de produção de `ata_do_dia` continuam nos ramos de quem nunca bateu, de leitura da ata atual e de lavra, respectivamente ([cartorio.py:1280](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1280), [cartorio.py:1464](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1464), [cartorio.py:1899](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1899)). A grade já recebe os turnos pareados com a ata desta montagem ([utils.py:1909](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1909)); o tipo do dia pode usar a ata atual conforme a decisão recebida ([cartorio.py:1460](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1460)).

**Pergunta:** qual autoridade deve atribuir ao dia uma pausa avulsa sem turno corrente, inclusive quando as cópias da mesma batida aparecem em segmentos sobrepostos? É autorizado alterar a casca/corpo de `turnos_do_colab` para expor as cópias carimbadas, mantendo `parear_turnos` intocado, e de onde deve vir essa atribuição?

Arquivos tocados: nenhum. Cura, REDs, mutantes e teto de consultas: **não realizados**. Prova de todos os consumidores do dispatcher: **[não verificado]**. Perguntas de lei: **[não verificado]**; há a pergunta de alimentação acima.

## 19/11 12:15 · sessao 73c2244e8ef0 · FERIADO-POR-POSTO · censo · origem=Code

**Resultado:** há duas fontes com efeitos diferentes no código: a marca do posto alimenta o padrão de vínculo novo e o veto de cobrança; a chave gravada no vínculo alimenta `feriado_vira_folga` e, por ele, a geração e a precedência do dia ([escala/models.py:905](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:905), [escala/models.py:1373](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:1373), [detectar_ausencias.py:617](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/detectar_ausencias.py:617)).

A cópia não contém `.git`; portanto, o hash `ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba` como HEAD é **[não verificado]**. Os anexos foram fornecidos no texto do pedido, não como arquivos com linhas verificáveis. Nenhuma sonda de produção foi executada; seus resultados são **[não verificado]**.

## 1. Censo AST

Escopo da contagem: código Python de `app`, sem testes e migrações; **20 sítios leitores**, contando uma vez cada linha que lê diretamente os campos ou chama os métodos pedidos. Escritores aparecem separadamente. O script imprime os aliases de import e os candidatos encontrados por sintaxe, inclusive `getattr`, argumentos ORM e gravações; a classificação da tabela exige leitura humana. A busca sintática não prova ausência de acesso dinâmico, SQL cru, templates ou leitores indiretos; completude fora desse escopo é **[não verificado]**. [nao verificado]

Execute a partir da raiz da cópia; o script somente lê arquivos:

```python
import ast
from pathlib import Path

ALVOS = {
    "opera_em_feriado",
    "trabalha_em_feriado",
    "padrao_trabalha_em_feriado",
    "feriado_vira_folga",
}
RAIZ = Path("app")

for caminho in sorted(RAIZ.rglob("*.py")):
    if {"tests", "migrations"} & set(caminho.parts):
        continue
    try:
        arvore = ast.parse(caminho.read_text(encoding="utf-8"), filename=str(caminho))
    except (SyntaxError, UnicodeError) as exc:
        print(f"ERRO_PARSE {caminho} {type(exc).__name__}")
        continue

    aliases = {}
    for no in ast.walk(arvore):
        if isinstance(no, ast.Import):
            for item in no.names:
                aliases[item.asname or item.name.split(".")[0]] = item.name
        elif isinstance(no, ast.ImportFrom):
            modulo = "." * no.level + (no.module or "")
            for item in no.names:
                aliases[item.asname or item.name] = f"{modulo}.{item.name}"

    pais = {}
    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    achados = []
    for no in ast.walk(arvore):
        nome = None
        papel = None
        if isinstance(no, ast.Attribute) and no.attr in ALVOS:
            nome = no.attr
            papel = type(no.ctx).__name__
        elif isinstance(no, ast.Call) and isinstance(no.func, ast.Name):
            if (no.func.id in {"getattr", "setattr", "hasattr"}
                    and len(no.args) >= 2
                    and isinstance(no.args[1], ast.Constant)
                    and no.args[1].value in ALVOS):
                nome = no.args[1].value
                papel = no.func.id
        elif isinstance(no, ast.keyword) and no.arg:
            if any(no.arg == x or no.arg.endswith("__" + x) for x in ALVOS):
                nome = no.arg
                papel = "keyword: classificar ORM/leitura ou construtor/escrita"
        elif isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if no.name in ALVOS:
                nome = no.name
                papel = "definicao"
        elif isinstance(no, ast.Name) and no.id in ALVOS:
            nome = no.id
            papel = type(no.ctx).__name__
        if nome is None:
            continue
        atual = no
        contexto = "<modulo>"
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef)):
                contexto = atual.name
                break
        achados.append((no.lineno, nome, papel, contexto))

    if achados:
        print(f"\n{caminho}")
        for alias, origem in sorted(aliases.items()):
            print(f"  IMPORT {alias} -> {origem}")
        for linha, nome, papel, contexto in sorted(set(achados)):
            print(f"  {linha}: {contexto} {nome} {papel}")
```

| Sítio | Papel e fonte |
|---|---|
| [escala/models.py:911](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:911) | **Decide padrão novo:** lê marca do posto; se nula, lê tipo e base da escala. |
| [colaboradores/services/vinculo.py:296](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:296) | **Grava vínculo novo:** chama o padrão acima e grava sua resposta na chave do vínculo. |
| [escala/models.py:929](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:929), [932](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:932) | **Conta/exibe:** filtra chave True do vínculo e separa os que têm posto declarado True. |
| [escala/models.py:1390](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:1390) | **Decide:** `feriado_vira_folga` lê chave do vínculo, ciclo e feriado no lugar do vínculo; não lê a marca de operação. |
| [escala/models.py:1369](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:1369) | **Decide geração/fallback:** chama o predicado do vínculo para transformar dia-base de trabalho em folga. |
| [ponto/precedencia.py:377](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/precedencia.py:377) | **Decide fatos:** chama o predicado do vínculo para determinar `feriado_so_no_pagamento`. |
| [ponto/services/fechamento.py:949](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/fechamento.py:949), [ponto/calculador/alimentacao.py:521](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/alimentacao.py:521) | **Classificam pagamento:** chamam o predicado do vínculo; há diferença de dia-chave registrada em [BACKLOG.md:1654](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1654). |
| [colaboradores/services/feriado_do_posto.py:69](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:69) | **Decide veto:** `cobra_no_dia` lê marca do posto e feriado de seu município. |
| [colaboradores/services/feriado_do_posto.py:127](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:127) | **Exibe/classifica:** lê marca do posto para excluir marcados das sugestões. |
| [colaboradores/management/commands/marcar_feriado_do_posto.py:85](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/management/commands/marcar_feriado_do_posto.py:85), [129](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/management/commands/marcar_feriado_do_posto.py:129) | **Exibe e lê antes para trilha:** marca do posto. |
| [colaboradores/services/estrutura.py:84](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/estrutura.py:84) | **Grava:** marca do posto. |
| [colaboradores/services/vinculo.py:890](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:890), [892–894](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:892) | **Decide e grava:** compara e altera chave do vínculo existente. São dois sítios leitores e um escritor. |
| [colaboradores/services/vinculo.py:974](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:974), [1092](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:1092), [1113](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:1113) | **Copia/grava:** lê a chave de vínculo anterior e a passa ao novo objeto. |
| [sincronizar_feriado_admin.py:48](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/management/commands/sincronizar_feriado_admin.py:48), [52](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/management/commands/sincronizar_feriado_admin.py:52) | **Compara/exibe:** lê chave do vínculo; delega eventual gravação à porta na linha 56. |

Leitores indiretos relevantes: `detectar_ausencias` lê o resultado de `fatos_do_dia` e de `cobra_no_dia` ([detectar_ausencias.py:616](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/detectar_ausencias.py:616)); `sugestoes` usa presença registrada na ata e somente postos ainda sem marca ([feriado_do_posto.py:74](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:74), [136](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:136)).

## 2. Matriz do feriado em dia de escala

Hipóteses para a tabela: vínculo weekday, feriado que alcança seu posto, **célula gerada normalmente**, dia-base previsto, sem ausência ou outro fato de precedência superior. Célula editada ou antiga pode divergir do gerador; nesse caso, `fatos_do_dia` lê primeiro o `trabalha` da célula ([precedencia.py:347](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/precedencia.py:347)). A batida não altera a marca nem a chave.

`F/T/F` abaixo significa `(faixa=feriado, previsto=True, só pagamento=False)`; na tabela gerada com chave False, o previsto é False, portanto aparece `F/F/F`. `T/T/T` significa `(faixa=trabalho, previsto=True, só pagamento=True)`.

| Posto | Chave | Batida | Célula; fatos | Emissor de falta, se alcançado | Dobra no motor |
|---|---:|---|---|---|---|
| Opera | False | Sem | False; F/F/F | `VETO_FERIADO` | Sem duração apurada: não há dobra por essa batida. |
| **Opera** | **False** | **Com** | **False; F/F/F — contradição declarada** | `VETO_FERIADO` se o funil for alcançado | Depende de duração apurada e `feriado_em_dobra` do ciclo. |
| Fecha | False | Sem | False; F/F/F | `VETO_FERIADO` chega antes do veto do posto. | Sem duração apurada: não há dobra por essa batida. |
| Fecha | False | Com | False; F/F/F | `VETO_FERIADO` se alcançado. | Depende de duração e configuração do ciclo. |
| Sem marca | False | Sem | False; F/F/F | `VETO_FERIADO`; a ausência de marca não veta por posto. | Sem duração apurada: não há dobra por essa batida. |
| Sem marca | False | Com | False; F/F/F | `VETO_FERIADO` se alcançado. | Depende de duração e configuração do ciclo. |
| Opera | True | Sem | True; T/T/T | Passa os dois vetos de feriado; emissão ainda depende das demais guardas. | Sem duração apurada: não há dobra por essa batida. |
| Opera | True | Com | True; T/T/T | Passa os dois vetos de feriado; demais guardas seguem aplicáveis. | Depende de duração e configuração do ciclo. |
| **Fecha** | **True** | **Sem** | **True; T/T/T — contradição declarada** | **`VETO_FERIADO_DO_POSTO`**, se nenhuma guarda anterior interromper. | Sem duração apurada: não há dobra por essa batida. |
| **Fecha** | **True** | **Com** | **True; T/T/T — contradição declarada** | `VETO_FERIADO_DO_POSTO` se o funil for alcançado. | Depende de duração e configuração do ciclo. |
| Sem marca | True | Sem | True; T/T/T | Passa os dois vetos de feriado; demais guardas seguem aplicáveis. | Sem duração apurada: não há dobra por essa batida. |
| Sem marca | True | Com | True; T/T/T | Passa os dois vetos de feriado; demais guardas seguem aplicáveis. | Depende de duração e configuração do ciclo. |

A contradição **opera/False** também está presente na primeira linha, com ou sem batida: o vínculo faz o dia virar feriado/folga apesar da marca de operação. A tabela identifica **dois tipos** de contradição, correspondentes a quatro combinações com batida; o número de vínculos contraditórios na frota é **[não verificado]**. Posto sem marca não é tratado aqui como declaração contraditória.

A cadeia da matriz está em [escala/models.py:1367](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:1367), [precedencia.py:376](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/precedencia.py:376), [detectar_ausencias.py:617](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/detectar_ausencias.py:617) e [feriado_do_posto.py:60](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:60). O motor pergunta `feriado_em_dobra` e `eh_feriado`, e aplica a dobra a duração positiva; ele não pergunta essas duas marcas nesse predicado ([motor_calculo_v2.py:914](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/motor_calculo_v2.py:914), [2809](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/motor_calculo_v2.py:2809)). O efeito monetário completo de cada linha é **[não verificado]**.

## 3. Sonda somente leitura

Universo: cada `EscalaColaborador` cujo ciclo esteja em `TipoEscala.CICLOS_FERIADO_VIRA_FOLGA` e que satisfaça **`q_vinculo_cobre_a_data(hoje)`**, a função SQL da casa para vigência ([escala/models.py:32](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:32), [escala/alimentacao.py:219](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/alimentacao.py:219)). É contagem de **vínculos**, inclusive sobrepostos, sem filtro adicional de situação do colaborador; não significa que todos sejam o vínculo escolhido para um dia.

A sonda usa `sugestoes()` real, que só sugere para postos não marcados ([feriado_do_posto.py:136](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:136)). Saída identificadora: apenas PK de posto e vínculo. Executar no envelope Django da sombra; **não executada aqui**.

```python
from collections import Counter
from django.utils import timezone

from colaboradores.models import Posto
from colaboradores.services.feriado_do_posto import sugestoes
from escala.alimentacao import q_vinculo_cobre_a_data
from escala.models import EscalaColaborador, TipoEscala

ERROS = 0

try:
    hoje = timezone.localdate()
    postos = list(Posto.objects.values_list("pk", "opera_em_feriado"))
    marca_por_posto = dict(postos)
    postos_por_marca = Counter(
        "OPERA" if marca is True else "FECHA" if marca is False else "SEM_MARCA"
        for _, marca in postos
    )

    qs = EscalaColaborador.objects.filter(
        q_vinculo_cobre_a_data(hoje),
        tipo_escala__tipo_ciclo__in=TipoEscala.CICLOS_FERIADO_VIRA_FOLGA,
    )
    universo = qs.count()
    linhas = list(qs.values_list("pk", "posto_id", "trabalha_em_feriado"))
    contagem = Counter()
    for vinculo_pk, posto_pk, chave in linhas:
        if posto_pk is None:
            classe = "SEM_POSTO"
        else:
            marca = marca_por_posto[posto_pk]
            classe = "OPERA" if marca is True else "FECHA" if marca is False else "SEM_MARCA"
        contagem[(classe, chave)] += 1

    print("UNIVERSO=%d" % universo)
    for classe in ("OPERA", "FECHA", "SEM_MARCA"):
        print("POSTOS_%s=%d" % (classe, postos_por_marca[classe]))
    for classe in ("OPERA", "FECHA", "SEM_MARCA", "SEM_POSTO"):
        for chave in (True, False):
            print("VINCULOS_%s_%s=%d" % (classe, chave, contagem[(classe, chave)]))

    contador = sum(contagem.values())
    print("CONTADOR=%d UNIVERSO=%d IGUAL=%s" %
          (contador, universo, contador == universo))
    if contador != universo:
        ERROS += 1

    s = sugestoes()
    sug_opera = {x["posto_id"] for x in s["sugere_OPERA"]}
    sug_fecha = {x["posto_id"] for x in s["sugere_FECHA"]}
    bo = sorted((p, v) for v, p, chave in linhas if p in sug_opera and chave is False)
    inverso = sorted((p, v) for v, p, chave in linhas if p in sug_fecha and chave is True)

    for rotulo, pares in (("BO_FALSE_SUGERE_OPERA", bo),
                          ("INVERSO_TRUE_SUGERE_FECHA", inverso)):
        print("%s_VINCULOS=%d POSTOS=%d" %
              (rotulo, len(pares), len({p for p, _ in pares})))
        for posto_pk, vinculo_pk in pares:
            print("%s POSTO=%d VINCULO=%d" % (rotulo, posto_pk, vinculo_pk))

    if 596 in marca_por_posto:
        print("POSTO=596 MARCA=%s SUGERE_OPERA=%s SUGERE_FECHA=%s" %
              (marca_por_posto[596], 596 in sug_opera, 596 in sug_fecha))
    else:
        print("POSTO=596 AUSENTE")

    focal = EscalaColaborador.objects.filter(pk=1255).values_list(
        "pk", "posto_id", "trabalha_em_feriado"
    ).first()
    if focal is None:
        print("VINCULO=1255 AUSENTE")
    else:
        v, p, chave = focal
        print("VINCULO=%d POSTO=%s CHAVE=%s NO_UNIVERSO=%s" %
              (v, p, chave, any(linha[0] == v for linha in linhas)))

except Exception:
    ERROS += 1
    print("ERRO_SONDA")

print("ERROS=%d" % ERROS)
print("FIM_SONDA")
```

`ERROS=0`, igualdade e números de classe só podem ser afirmados após execução. A quantidade de postos fechados e vínculos mencionada no anexo de contexto tem outro universo — vínculos de qualquer vigência — e não substitui esta medição **[não verificado]**.

## 4. Pergunta aberta de lei

**Não achei uma regra geral que diga qual fonte deve governar todos os efeitos quando posto e vínculo existente divergem.** A busca nas referências disponíveis tem estes limites e achados:

- Para **vínculo novo**, a regra é explícita: “O posto que DECLAROU … vence”; o mesmo texto diz que vínculo existente não passa pelo padrão ([escala/models.py:905](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:905); também [TICKETS.md:1168](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/TICKETS.md:1168)).
- Para **cobrança**, o código registra que posto marcado fechado veta mesmo vínculo que trabalha em feriado ([detectar_ausencias.py:619](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/detectar_ausencias.py:619)). Isso resolve esse veto específico.
- O backlog registra expressamente as duas fontes e encaminha a cópia por vínculo como segunda fonte para dossiê ([BACKLOG.md:1617](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1617)).
- Não achei, nas buscas pelos termos de posto e feriado em `CORTES.md`, `LEIS.md`, `LAPIDES.md` e diretório ADR, uma regra adicional para a divergência geral. O conteúdo das **Pautas DP 140–149** não foi localizado na cópia; sua decisão é **[não verificado]**.

**Pergunta de lei L=1:** `lei: FERIADO-POR-POSTO fonte unica` — a marca declarada do posto deve mandar também nos efeitos hoje decididos pela chave do vínculo existente, ou a chave do vínculo permanece autoridade própria nesses efeitos? Outra decisão é **[não verificado]**. Números da classe do BO, inverso e contradições: **[não verificado]**, pendentes da sonda.

## 5. Tela e porta existente

O cadastro está em `form_posto`, protegido por login e pela ação `editar_postos`; a lista exige `ver_postos` ([views.py:699](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/views.py:699), [733](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/views.py:733)). As rotas de criar e editar estão em [urls.py:38](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/urls.py:38); o template é [posto_form.html:10](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/templates/colaboradores/posto_form.html:10). O dicionário salvo pela view não inclui `opera_em_feriado` ([views.py:85

[truncado em 20000 de 21131 caracteres]

## 19/11 12:12 · sessao 97052842952f · CADEIA-DO-CONTRATO-20 · censo · origem=Code

A regra pedida para o passo 2 é preservar o **instante completo de cada sobra**, mantendo sua associação ao dia do turno informada no brief do passo 1. A ata atual recebe uma lista que já foi reduzida a `HH:MM`; portanto, trocar somente a serialização em `ata_do_dia` não recuperaria data nem segundos. ([utils.py:631](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:631), [utils.py:673](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:673), [cartorio.py:764](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:764); brief do passo 1 fornecido: linha [não verificado])

**Limite da cópia:** os arquivos estão disponíveis, mas `git rev-parse HEAD` falhou porque o diretório não contém um repositório Git acessível. Assim, a identidade da cópia com o HEAD informado é **[não verificado]**. O censo abaixo foi executado nos arquivos presentes, com `python3 -B`, sem criar arquivos. [não verificado: identidade do HEAD]

## Casos pela regra, antes de qualquer código

A L-110 pede o caso e a resposta da regra antes do código; a comparação com o comportamento anterior serve para medir impacto, não para certificar a resposta. ([CLAUDE.md:46](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/CLAUDE.md:46))

Os exemplos são casos especificados, **não resultados de testes executados**. A representação ISO com offset abaixo concretiza o instante proposto no pré-desenho; o formato efetivamente adotado em implementação futura é [não verificado]. [não verificado: linha local do anexo `predesenho.r`] [nao verificado]

| Caso e entrada | Resposta esperada pela regra |
|---|---|
| Uma sobra em 19/11/2026 às `14:17:23-03:00`, pertencente ao turno de 19/11 | A ata conserva `2026-11-19T14:17:23-03:00`; a apresentação pode mostrar `14:17`, sem usar esse texto como identidade. ([cartorio.py:764](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:764)) |
| Turno de 19/11 cruza meia-noite; sua sobra ocorre em `2026-11-20T04:00:12-03:00` | O instante conserva a data civil de 20/11 e a sobra pertence ao **dia do turno 19/11**, conforme o brief fornecido. Não se deve substituir a data do instante pela data do turno. [não verificado: linha local do brief do passo 1] |
| Duas sobras do mesmo turno em `14:17:05-03:00` e `14:17:48-03:00` | São duas ocorrências distintas; ambas sobrevivem à lavra e à leitura, embora as duas sejam exibidas como `14:17`. A reconciliação de uma não remove a outra. O código atual compara e remove por `HH:MM`. ([utils.py:2025](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:2025), [utils.py:2052](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:2052)) |
| O mesmo instante expresso com offsets diferentes | A comparação temporal reconhece o mesmo instante; a apresentação não decide identidade por texto. Esse é um caso de representação proposto no pré-desenho. [não verificado: linha local do anexo `predesenho.r`] |
| Ata antiga com `orfas=["14:17"]` ou apenas `n_orfas=1` | A contagem pode demonstrar presença; o instante não é conhecido e não deve ser fabricado a partir da data da célula. O leitor atual fabrica `?` quando só dispõe da contagem. ([leitor_celula.py:187](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/leitor_celula.py:187)) |
| Lista lavrada explicitamente vazia, sem contagem contraditória | Zero sobras conhecido; é diferente de uma lista ausente ou incompleta. A gravação atual registra lista e contagem separadamente. ([cartorio.py:761](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:761)) |
| Sobra existe, mas não há marcos para desenhar | A existência e o instante da sobra continuam disponíveis ao leitor e à apresentação. Hoje o adaptador pode devolver duas listas vazias quando não há marcos. ([utils.py:1095](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1095), [utils.py:1121](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1121)) |

**Ressalva no noturno:** o brief fornecido diz que a sobra pertence ao dia do turno, enquanto a reconciliação atual procura a **ponta de saída** na lista de `data_turno + 1` quando o juiz informa cruzamento de meia-noite. O caso acima registra a resposta solicitada; não prova que o código atual já a cumpra, nem autoriza remover as guardas existentes da reconciliação. ([utils.py:1965](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1965), [utils.py:1981](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1981); brief fornecido, linha [não verificado])

## Censo dos escritores e leitores

**Papel** indica o efeito comprovado no sítio: **MOSTRA** apresenta ou transporta fatos; **DECIDE** usa o dado em classificação, seleção ou chamada; **PAGA** altera cálculo monetário. Uma chamada à grade, por si só, não prova que o chamador consuma órfãs nem define seu papel; o script a inclui como candidato para inspeção. ([leitor_celula.py:420](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/leitor_celula.py:420))

| Sítio | Papel e efeito constatado | Dependência do passo 1 |
|---|---|---|
| Grade: formação e promoções | **DECIDE/transporta:** forma sobras em `HH:MM`, acrescenta e ordena; também promove células para a lista. O instante precisa sobreviver desde a fonte. ([utils.py:631](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:631), [utils.py:810](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:810), [utils.py:1733](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1733)) | Preservar instante **não depende** da razão; atribuir razão a cada ocorrência **depende** da lavra do passo 1. [não verificado: linha local do brief] |
| Grade: ata sintética e reconciliação | **DECIDE/transporta:** constrói ata em memória e procura/remove bordas por hora. A migração deve preservar as guardas existentes e operar sobre a ocorrência correta. ([utils.py:1898](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1898), [utils.py:1979](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1979), [utils.py:2039](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:2039)) | Comparação por instante **não depende** da razão; transportar uma razão já associada **depende** do contrato entregue pelo passo 1. [não verificado: formato futuro] |
| Cartório: `lampadas_do_dia` e `ata_do_dia` | **DECIDE/escreve:** o primeiro usa horas de órfãs na geometria; o segundo grava `n_orfas` e `orfas`. ([cartorio.py:647](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:647), [cartorio.py:755](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:755), [cartorio.py:764](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:764)) | Gravar e comparar instantes **não depende** da razão; gravar a razão na mesma lavra **depende** do passo 1. [não verificado: linha local do brief] |
| Porta da célula | **Escreve/transporta:** substitui a ata na lavra; outra operação copia a ata para carimbar precedência. A projeção `out.ata` é cópia em memória. ([celula.py:1003](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/portas/celula.py:1003), [celula.py:1064](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/portas/celula.py:1064), [celula.py:1214](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/portas/celula.py:1214)) | Transportar o novo valor **não depende** da razão; preservar o campo de razão quando presente **depende** do formato que o passo 1 lavrar. [não verificado: formato futuro] |
| `leitor_celula` | **MOSTRA/transporta:** lê a lista crua ou cria marcadores pela contagem e publica `dia.orfas`. É a fronteira natural para distinguir instante conhecido de dado legado incompleto. ([leitor_celula.py:180](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/leitor_celula.py:180), [leitor_celula.py:420](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/leitor_celula.py:420)) | Ler instantes e contagem **não depende** da razão; publicar a razão **depende** de ela estar lavrada. [não verificado: contrato futuro] |
| Espelho e template | **MOSTRA:** a grade passa por adaptador e chega a `orfas_grade`, impresso no template. ([espelho.py:703](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:703), [espelho.py:858](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:858), [espelho.html:242](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/templates/ponto/espelho.html:242)) | Mostrar hora **não depende** da razão; mostrar o motivo **depende** do passo 1. [não verificado: apresentação futura] |
| Calendário | **MOSTRA, com seleção:** usa `orfas` da grade para marcar batidas; a associação atual usa a lista e a hora. ([calendario.py:409](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:409), [calendario.py:655](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:655)) | Marcação por instante **não depende** da razão; eventual rótulo de motivo **depende** dela. [não verificado: rótulo futuro] |
| PDF | **MOSTRA/DECIDE apresentação:** coleta órfãs e usa `HH:MM` para reconhecer entrada espúria e marcas. ([pdf_espelho.py:174](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/relatorios/pdf_espelho.py:174), [pdf_espelho.py:477](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/relatorios/pdf_espelho.py:477), [pdf_espelho.py:552](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/relatorios/pdf_espelho.py:552)) | Reconhecer a ocorrência e formatar **não depende** da razão; imprimir razão **depende** dela. [não verificado: impressão futura] |
| `periodos_do_dia` | **MOSTRA/transporta:** copia as strings da ata no resultado; a própria documentação afirma que a órfã aparece e não entra em período. ([juiz_batida.py:18](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/juiz_batida.py:18), [juiz_batida.py:161](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/juiz_batida.py:161)) | Transporte de instante **não depende** da razão. Efeito PAGA de uma eventual mudança nesse caminho: **[não verificado]**. |
| Chamados: juiz de furo e materialização | **DECIDE:** o juiz testa lista ou contagem; a materialização filtra horas e usa as marcas na pergunta de preenchimento. ([juizes.py:514](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/juizes.py:514), [materializacao.py:449](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/materializacao.py:449)) | Presença por contagem e identidade por instante **não dependem** da razão. Fazer a razão influenciar uma decisão existente seria outra mudança; autorização e efeito **[não verificado]**. |
| Cluster espúrio | **DECIDE:** lê a ata, calcula proximidade por minutos e pode produzir contexto de chamado. ([cluster_espurio.py:29](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cluster_espurio.py:29), [cluster_espurio.py:34](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cluster_espurio.py:34), [cluster_espurio.py:211](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cluster_espurio.py:211)) | Diferença entre instantes **não depende** da razão; exibir ou usar razão dependeria do passo 1 e do contrato do chamado **[não verificado]**. |
| Esmeril e vigias | **DECIDE medida/alarme:** esmeril conta e agrupa por faixa; vigia do pareador compara horas; tripwire recebe a lista crua. ([esmeril_espelho.py:130](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/esmeril_espelho.py:130), [vigia_pareador_celula.py:74](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/vigia_pareador_celula.py:74), [tripwire_celulas.py:264](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/tripwire_celulas.py:264)) | Contar, medir e reconhecer instante **não dependem** da razão; medir por motivo **depende** dela. [não verificado: medida futura] |
| Propositor, ausência parcial e suprajuiz | **DECIDE/MOSTRA:** o propositor analisa horas; turnos converte sobras em minutos do dia; suprajuiz testa presença e descreve. ([propositor.py:39](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/propositor.py:39), [turnos.py:2679](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/turnos.py:2679), [supra_juiz.py:527](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/supra_juiz.py:527)) | Adaptar a representação **não depende** da razão; classificar ou descrever por razão **depende** dela. Consequência PAGA da cadeia de ausência parcial: **[não verificado]**. |
| Agregados e classificações | **DECIDE/conta:** causas da folha, status, furos, bordas e simulação usam presença ou quantidade. Não se deve converter identidade desconhecida em zero. ([causas.py:43](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/folha/causas.py:43), [status_dia.py:188](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/status_dia.py:188), [furos_diarios.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/furos_diarios.py:22), [bordas_realizado.py:49](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/bordas_realizado.py:49), [simular_dia.py:12](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/simular_dia.py:12)) | Presença e contagem **não dependem** da razão. Classificação por motivo depende do passo 1 e de decisão autorizada **[não verificado]**. O nome `folha` não prova, sozinho, efeito **PAGA**. |
| Classificador atual | **DECIDE:** `classificar_orfas` lê `dia.orfas` e devolve classes chaveadas por hora. O brief fornecido determina que seu corpo vire o juiz da razão ou morra no marco. ([orfa_classe.py:53](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/orfa_classe.py:53), [orfa_classe.py:58](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/orfa_classe.py:58); brief fornecido, linha [não verificado]) | A **razão** depende do passo 1; a chave de identidade por instante é necessidade do passo 2. A forma integrada final é **[não verificado]**. |

O AST também encontrou leitura direta no cartão e no snapshot, além do comando do propositor; sua existência ou chamada por nome não comprova, por si, uso produtivo atual. ([cartao_pela_celula.py:288](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/relatorios/cartao_pela_celula.py:288), [snapshot_o118_col221.py:32](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/bin/snapshot_o118_col221.py:32), [propor_reorganizacao.py:104](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/propor_reorganizacao.py:104))

O script abaixo é o executado na sessão, com `python3 -B`. Lista ocorrências sintáticas e chamadas às funções-alvo, inclusive `from ... import nome as apelido`; a classificação de papel na tabela exige leitura do contexto. Ele também lista homônimos, não resolve fluxo de dados, aliases atribuídos em tempo de execução, acesso dinâmico ou templates; completude para esses casos é **[não verificado]**. Os templates foram conferidos por busca textual. ([calendario.py:362](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:362), [espelho.html:242](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/templates/ponto/espelho.html:242))

```python
import ast
from pathlib import Path

roots = (Path('app'), Path('bin'))
targets = {
    '_orfas_da_ata', 'grade_da_celula', 'montar_realizado_grade',
    'ata_do_dia', 'periodos_do_dia', 'lavrar_veredito',
}
keys = {'orfas', 'n_orfas', 'orfas_grade'}

for root in roots:
    for path in sorted(root.rglob('*.py')):
        if 'tests' in path.parts or path.name.startswith('test_'):
            continue
        try:
            tree = ast.parse(path.read_text(encoding='utf-8'), filename=str(path))
        except (SyntaxError, UnicodeError):
            print(f'NAO_ANALISADO {path}')
            continue

        aliases = {}
        for node in ast.walk(tree):
            if isinstance(node, ast.ImportFrom):
                for item in node.names:
                    if item.name in targets:
                        aliases[item.asname or item.name] = item.name

        hits = set()
        for node in ast.walk(tree):
            if isinstance(node, ast.Dict):
                for key in node.keys:
                    if isinstance(key, ast.Constant) and key.value in keys:
                        hits.add((key.lineno, 'CHAVE ' + key.value))
            elif isinstance(node, ast.Subscript):
                k = node.slice
                if isinstance(k, ast.Constant) and k.value in keys:
                    kind = 'ESCREVE' if isinstance(node.ctx, (ast.Store, ast.Del)) else 'LE'
                    hits.add((node.lineno, kind + ' ' + k.value))
            elif isinstance(node, ast.Call):
                f = node.func
                if isinstance(f, ast.Attribute) and f.attr in {'get', 'pop', 'setdefault'} and node.args:
                    k = node.args[0]
                    if isinstance(k, ast.Constant) and k.value in keys:
                        hits.add((node.lineno, f.attr.upper() + ' ' + k.value))
                name = f.id if isinstance(f, ast.Name) else f.attr if isinstance(f, ast.Attribute) else None
                original = aliases.get(name, name)
                if original in targets:
                    suffix = f' COMO {name}' if original != name else ''
                    hits.add((node.lineno, 'CHAMA ' + original + suffix))
                for kw in node.keywords:
                    if kw.arg in keys:
                        hits.add((kw.value.lineno, 'ARG ' + kw.arg))
            elif isinstance(node, ast.Attribute) and node.attr == 'ata' and isinstance(node.ctx, ast.Store):
                hits.add((node.lineno, 'ESCREVE .ata'))

        for line, role in sorted(hits):
            print(f'{path}:{line} {role}')
```

**Perguntas de lei: 0**, para os casos de preservação de instante e transporte aqui descritos, mantendo as autoridades e guardas existentes. A execução dos casos, o impacto em dados reais e a comprovação de ausência de efeito monetário permanecem **[não verificado]**. ([CLAUDE.md:46](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/CLAUDE.md:46))

## 19/11 12:11 · sessao 859ef8c33903 · CADEIA-20-PASSO-1 · construtor · origem=Code

**PAREI: a marca `ESCALA_NAO_DESCREVE` não chega ao fluxo da lavra sem nova consulta identificada.** A pergunta está ao fim; não escrevi código nem testes. [nao verificado]

`ata_do_dia` tem três chamadas diretas de produção, todas em `_julgar_colab_corpo`:

- Ramo de quem nunca bateu: monta `_ata_nb` em [cartorio.py:1280](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1280), depois a passa à lavra em [cartorio.py:1321](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1321).
- Ramo normal, ata deste julgamento para leitura durante o julgamento: [cartorio.py:1464](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1464).
- Ramo normal, ata passada a `lavrar_veredito`: [cartorio.py:1898](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1898).

Nos três casos, a grade vem da chamada em [cartorio.py:1204](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1204). Os insumos têm estas origens e limites:

- **Turnos:** a grade por turno já chama `turnos_do_colab` com a ata desta montagem em [escala/utils.py:1890](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1890) e [escala/utils.py:1909](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1909), mas retorna somente `marcos` e `dias` em [escala/utils.py:2082](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2082). O dispatcher também admite o caminho legado em [escala/utils.py:2095](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2095). Portanto, reutilizar os turnos exige transportá-los até a lavra; disponibilidade equivalente no legado: **[não verificado]**.
- **Tipo do dia:** `dia_sem_trabalho` lê `cel.trabalha` e `cel.ata`, inclusive `cobertura_parcial`, em [chamados/juizes.py:445](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/chamados/juizes.py:445). Para este julgamento, a testemunha deve ser a ata montada agora com `cel.trabalha`, não `cel.ata` antiga: o próprio cartório distingue a ata atual da anterior em [cartorio.py:1460](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1460).
- **Pausas:** o juiz recebe batidas de pausa avulsas já atribuídas ao dia em [orfa_classe.py:23](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:23). O pareador carimba a batida e, se não houver turno corrente, não a acrescenta a um turno em [ponto/turnos.py:1365](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1365). `turnos_do_colab` pareia cópias e devolve turnos em [ponto/turnos.py:1984](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1984). Uma lista dessas pausas avulsas, com atribuição ao dia, disponível na lavra sem nova consulta: **[não verificado]**.
- **Marca:** a lista publicada vem de `espelho_do_colab(...).resultado.dias_cadastro_x_realidade` em [cxr_na_lista.py:77](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:77). Existe leitura da lavra por consulta a `MetricaSnapshot` em [cxr_na_lista.py:152](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:152); sua ausência de cobertura retorna `None`, não lista vazia, em [cxr_na_lista.py:165](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:165). Não identifiquei essa lista já carregada no fluxo de `cartorio.py` **[não verificado quanto a acesso indireto em tempo de execução]**. [nao verificado]

**Aval devolvido, revisão por item:**

1. As chamadas e a ata atual conferem nas referências acima; a disponibilidade dos turnos e pausas até a lavra não está demonstrada, e a marca não foi identificada carregada no fluxo.
2. Compatibilidade integral com outros avais vigentes: **[não verificado]**; foi fornecido apenas o aval raspado.
3. A condição literal de não acrescentar consulta não está demonstrada para a marca; a alimentação das pausas avulsas também precisa ser esclarecida ([cxr_na_lista.py:152](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:152), [orfa_classe.py:27](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:27)).
4. Alteração de exportação, dinheiro ou `!`: **[não verificado]**; nenhuma alteração foi feita.

**Pergunta:** qual fonte já carregada deve alimentar a marca `ESCALA_NAO_DESCREVE` na lavra e como devem chegar as pausas avulsas atribuídas ao dia, sem consulta nova nem novo pareamento? Em particular, se a única fonte da marca for o snapshot, como proceder quando `cxr_lavrado` retorna `None` por falta de cobertura ([cxr_na_lista.py:170](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:170))?

Arquivos tocados: nenhum. Testes RED, mutantes, teto de consultas e comando Django executado: **[não verificado; não realizados]**. Perguntas de lei: **[não verificado]**; há uma pergunta de alimentação acima. [nao verificado]

## 19/11 12:11 · sessao 8aba5c841c41 · CADEIA-DO-CONTRATO-20 · censo · origem=Code

O pré-mapa passa de **34 para 38 arquivos necessários, provisoriamente**, com **3 não decididos** e **4 conflitos de conteúdo**. Os 38 incluem os 34 classificados como necessários no anexo de 18/11, cuja classificação integral não foi refeita arquivo a arquivo nesta resposta; os quatro acréscimos são `calendario.py`, `dia_decidido.py`, `ausencia.py` e `export.py`. A necessidade do registro final de `test_medida_turno_do_par.py`, de `situacao.py` e de `test_a2_precedencia.py` permanece não decidida. [não verificado]

A medição fornecida aponta **5 dos 41 arquivos alterados no main** e **nenhum arquivo em comum** com as listas fornecidas da F5 e do passo 1; isso mede interseção por arquivo, não elimina dependências indiretas. [não verificado]

| Arquivo alterado | Pouso e efeito observado | Mudança LASTRO | [nao verificado]
|---|---|---|
| `calendario.py` | **ESPURIA-TELA c11**, pela linhagem `585dbb8bf` informada: transporta quem decidiu, quando e o ID da ausência a partir de `_dd`. [HEAD: calendario.py:757](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:757) | **Necessária**: LASTRO alimenta o status e o compositor com recusas por data; isso não aparece no trecho novo do HEAD. [cópia: calendario.py:499](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:499), [cópia: calendario.py:710](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:710) |
| `test_medida_turno_do_par.py` | **ESCALA c7, ESPURIA-TELA c11 e DST c5**, conforme os commits de catraca informados; o HEAD registra posições medidas. A atribuição individual de cada linha a cada pouso está **[não verificado]**. [HEAD: test_medida_turno_do_par.py:247](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_medida_turno_do_par.py:247) | **Não decidida**: a cópia registra deslocamentos LASTRO, mas os valores finais dependem de nova medição da árvore integrada. [cópia: test_medida_turno_do_par.py:247](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/core/tests/test_medida_turno_do_par.py:247), [HEAD: teste:559](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_medida_turno_do_par.py:559) |
| `cartorio.py` | **DST c5** e sua linhagem DST: o fecho passa a chamar a rejeição em lote dentro da transação; o predicado de protesto também passa pelo juiz de dia sem trabalho. [HEAD: cartorio.py:1028](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:1028), [HEAD: cartorio.py:1480](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:1480) | **Necessária**: LASTRO transporta campos da cobertura e amplia a leitura de cobertura inteira; o HEAD ainda usa o predicado anterior nesse ponto. [cópia: cartorio.py:773](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/cartorio.py:773), [cópia: cartorio.py:1253](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/cartorio.py:1253), [HEAD: cartorio.py:1273](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:1273) |
| `dia_decidido.py` | **ESPURIA-TELA c11**, pela linhagem informada: acrescenta ícone de classe e trilha da ausência que deu a palavra. [HEAD: dia_decidido.py:196](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/dia_decidido.py:196), [HEAD: dia_decidido.py:438](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/dia_decidido.py:438) | **Necessária** para a saída LASTRO: a cópia reconhece também `folga_da_escala`; o HEAD reconhece apenas `folga` nesse ramo. [cópia: dia_decidido.py:371](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_decidido.py:371), [HEAD: dia_decidido.py:400](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/dia_decidido.py:400) |
| `espelho.py` | **ESPURIA-TELA c11**, pela linhagem informada: produz e limpa trilha da decisão e `classe_dia`. [HEAD: espelho.py:344](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:344) | **Necessária**: LASTRO propaga e apresenta a recusa por dia. [cópia: espelho.py:339](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:339), [cópia: espelho.py:649](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:649) |

A lista de commits fornecida não demonstra alteração adicional desses cinco atribuível a **F6F7, F9, F3 ou DST c6 v4**; atribuição por esses pousos: **[não verificado]**. [nao verificado]

**Conflitos novos.**

- **`calendario.py`: exige decisão de integração e verificação, não apenas escolher um lado.** O HEAD lê `veredito`, `palavra`, `cor` e a trilha de decisão do retorno de `do_dia`; LASTRO substitui esse retorno por um dicionário processado por `aplicar_palavra_do_dia` e lê as chaves terminadas em `_dia`. [HEAD: calendario.py:757](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:757), [cópia: calendario.py:710](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:710) A dificuldade concreta é que LASTRO chama o compositor com `limpar=True`, enquanto o HEAD limpa nessa opção justamente as três chaves da trilha que o calendário precisa entregar; a forma de preservar a trilha na tela e a limpeza no documento precisa ser acertada e verificada. [cópia: calendario.py:719](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:719), [HEAD: espelho.py:355](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:355)
- **`espelho.py`: união textual orientada pelo contrato, com verificação funcional.** No trecho conflitado, o HEAD acrescenta à limpeza a trilha e `classe_dia`; LASTRO acrescenta `_recusa_do_dia`. O texto da docstring também diverge na contagem das chaves. A união das chaves é identificável, mas o ramo de recusa e a possibilidade de deixar dados anteriores no dicionário integrado precisam ser verificados; não classifico a integração inteira como puramente mecânica. [HEAD: espelho.py:317](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:317), [cópia: espelho.py:334](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:334), [cópia: espelho.py:350](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:350)

**Conflitos antigos.** A natureza do teste continua sendo a catraca de posições, agora com mais deslocamentos a medir após a integração; não se deve escolher os números de um lado por simples precedência. [HEAD: test_medida_turno_do_par.py:247](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_medida_turno_do_par.py:247), [cópia: teste:247](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/core/tests/test_medida_turno_do_par.py:247) Em `ausencia.py`, agora é possível identificar o trecho: o HEAD renomeou o parâmetro para `tipo_ausencia`, enquanto LASTRO insere validação de código e faixa usando o nome antigo `tipo`; integrar requer adaptar a validação ao parâmetro atual e preservar os dois comportamentos. O pré-mapa não identificava o trecho, portanto não permite concluir que sua natureza **mudou** desde então. [HEAD: ausencia.py:1761](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/ausencia.py:1761), [cópia: ausencia.py:1303](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/ausencia.py:1303)

**Ordem sugerida para o rebase futuro, por dependência de arquivo:** `precedencia.py` e catálogo → `ausencia.py` e produtores de cobertura/grade → `dia_pago.py`, `cartorio.py` e `dia_decidido.py` → `espelho.py` → `calendario.py`, `export.py` e demais leitores → testes e fixtures → remedição final de `test_medida_turno_do_par.py`. É uma ordem de trabalho sugerida, não uma prova de ordenação obrigatória de todos os commits. A precedência fornece faixa e fatos; o espelho compõe os dados usados pelo calendário; o teste congela posições da árvore resultante. [cópia: precedencia.py:105](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/precedencia.py:105), [cópia: espelho.py:297](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:297), [cópia: calendario.py:710](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:710), [HEAD: teste:559](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_medida_turno_do_par.py:559)

**Folha, dia pago e retenção.** O toque de LASTRO em `export.py` é agora identificável: na prontidão sem snapshot, troca a derivação local pela leitura de `por_dia_da_grade`; é necessário para acompanhar a mudança de cobertura da cópia. [HEAD: export.py:1093](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/folha/export.py:1093), [cópia: export.py:1093](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/folha/export.py:1093) Em `dia_pago.py`, LASTRO amplia o reconhecimento de ausência com `ausencia_tipo` e declara preservar a fórmula legada; não há evidência, entre os cinco arquivos medidos como alterados hoje, de mudança direta nesse arquivo. [cópia: dia_pago.py:385](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_pago.py:385), [não verificado]

A comparação da cópia com sua base mostra o toque de `export.py` na prontidão, sem alteração direta em `motivos_retencao_celula`; já DST c5 usa um **portão de retenção de retratação** na rejeição de justificativas, assunto relacionado ao fecho e distinto da função de retenção da folha. Consequência indireta conjunta após integração: **[não verificado]**. [HEAD: export.py:1176](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/folha/export.py:1176), [HEAD: justificativa.py:292](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:292)

**38 arquivos necessários [provisório; não verificado], 4 conflitos, ordem: precedência e catálogo → ausência e cobertura → dia pago, cartório e dia decidido → espelho → calendário, exportação e leitores → testes e medição, perguntas de lei: [não verificado].** [nao verificado]

## 19/11 12:11 · sessao f8349cca706f · CADEIA-DO-CONTRATO-20 · leitura · origem=Code

**NÃO PROCEDE literalmente — AVAL-DEVOLVIDO.** As duas listas estão vazias e o aval mantém a célula parcial, mas **o texto da nota não é interpretado para decidir a cor**. A célula está não verde porque sua declaração omite `verde`, cujo padrão é `False`. A nota documenta a razão dessa declaração. [app/config/crons.py:2061](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/config/crons.py:2061), [app/core/juizes.py:481](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/juizes.py:481), [app/core/contratos_estruturais.py:62](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:62), [app/core/contratos_estruturais.py:262](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:262).

- A primeira tupla começa em `JUIZES_POR_VARREDURA: tuple[str, ...] = (` e termina sem elementos ativos; a segunda é literalmente `PENDENTES['chamado'] = ( )`. A célula aponta para ela por `PENDENTES_CHAMADO = PENDENTES['chamado']`. [app/config/crons.py:2061](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/config/crons.py:2061), [app/config/crons.py:2126](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/config/crons.py:2126), [app/core/juizes.py:481](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/juizes.py:481), [app/core/juizes.py:1382](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/juizes.py:1382).
- A nota mora na própria célula: **“esta celula fica PARCIAL ate a O168, a O169 e o LASTRO”**. O aval que a originou está identificado como **CELULA-CHAMADO-PARCIAL-ATE-O168-O169-LASTRO**, de 08/10; o texto também proíbe reclassificar papel para virar verde. [app/core/contratos_estruturais.py:270](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:270), [app/docs/PROMPTS.md:1083](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/PROMPTS.md:1083).
- Quem lê a nota: o placar a inclui para células não verdes; o selo da matriz exige que uma célula não verde tenha nota não vazia. Nenhum desses trechos interpreta O168, O169 ou LASTRO como condição executável. [app/core/placar_tickets.py:346](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/placar_tickets.py:346), [app/core/tests/test_selo_contratos_estruturais.py:98](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_selo_contratos_estruturais.py:98).
- `total()` calcula o **teto**, sem ler nota nem cor; `verdes()` conta as declarações com `verde=True`. A leitura do placar usa ambos e identifica as células com `verde=False`. [app/core/contratos_estruturais.py:1038](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:1038), [app/core/contratos_estruturais.py:1053](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:1053), [app/core/arquitetura_leitura.py:49](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/arquitetura_leitura.py:49).

A condição literal, com os estados documentados, é a seguinte:

| Item | Texto do aval ou registro literal | Estado documentado | O que o selo da célula mede |
|---|---|---|---|
| Aval 1 | “celula chamado x juiz fica PARCIAL ate O168, O169 e o LASTRO; proibido reclassificar papel para virar verde.” [app/docs/PROMPTS.md:1083](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/PROMPTS.md:1083) | A condição permanece registrada na nota. O corte posterior de chegada lista O168 e LASTRO, com O169 já pousada. [app/core/contratos_estruturais.py:270](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:270), [app/docs/CORTES.md:469](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/CORTES.md:469) | Não interpreta essa condição. [app/chamados/tests/test_contract_juiz_chamado.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:22) |
| O168 | “a guarda de conversa do supra_juiz (pode_executar_csf) vale tambem no evento”; o `--executar` sai depois e com `CSF_RETRATADO` medido em zero. [app/docs/BACKLOG.md:1544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1544) | BACKLOG registra pouso em `89a5b0d85`; o complemento do gate registra **NO AR** com `d33c0a899`. Valor de `CSF_RETRATADO` em produção hoje: **[não verificado]**. [app/docs/BACKLOG.md:1544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1544), [app/docs/BACKLOG.md:2176](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:2176) | A classe da célula não mede o contador de produção nem comprova essa sequência. [app/chamados/tests/test_contract_juiz_chamado.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:22) |
| O169 | A decisão registrada é “cluster nao fecha por idade”, com RED de 15 dias. [app/docs/BACKLOG.md:1944](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1944) | C20-O169 consta **POUSADA** em 05/11; commit do pouso **[não verificado]**. A linha O169-FECHADA registra separadamente contador e mutante como delta, em B3 desenho; conclusão desse delta hoje **[não verificado]**. [app/docs/BACKLOG.md:1944](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1944), [app/docs/BACKLOG.md:2254](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:2254) | A classe da célula não executa o caso de cluster com 15 dias nem mede `fechados_por_idade`. [app/chamados/tests/test_contract_juiz_chamado.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:22) |
| LASTRO | “todo tipo ATIVO no mapa ... com faixa nomeada; faixa_de fora do mapa = recusa nomeada; contador tipos_ativos_sem_faixa esperado 0; RED pk 5356 + tipo fantasma”. [app/docs/BACKLOG.md:1965](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1965) | **ABERTA**; depende do acessor TIPO-NA-FONTE. `f3ebd821b` é a **base da cópia**, não um commit de pouso. A cadeia exige base refeita, DIFF de frota e proíbe virar a célula antes do passo 3. [app/docs/BACKLOG.md:1965](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1965), [app/docs/BACKLOG.md:1960](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1960) | A classe da célula não mede cobertura de faixas, recusa, contador ou RED do LASTRO. [app/chamados/tests/test_contract_juiz_chamado.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:22) |

O selo nomeado em `teste=` é `JuizChamadoContratoTest`: confere existência das autoridades declaradas, ausência de perguntas sem juiz, teto zero dos pendentes e padrões de resposta fora da autoridade, com casos que exercitam sua detecção e verificações específicas de SLA e COBRANÇA. Ele **não muda a declaração `verde`**. [app/core/contratos_estruturais.py:262](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:262), [app/chamados/tests/test_contract_juiz_chamado.py:24](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:24), [app/chamados/tests/test_contract_juiz_chamado.py:85](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:85).

**Sem a nota, a célula continuaria não verde**, pois `verde` continuaria no padrão `False`; ademais, o selo da matriz rejeitaria a célula não verde sem nota. Declarada verde, a matriz verifica a existência da classe e as exceções vazias, mas esses testes não transformam as condições textuais dos avais em verificações executáveis. Resultado atual de execução da classe: **[não verificado]**. [app/core/contratos_estruturais.py:62](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:62), [app/core/tests/test_selo_contratos_estruturais.py:71](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_selo_contratos_estruturais.py:71), [app/core/tests/test_selo_contratos_estruturais.py:98](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_selo_contratos_estruturais.py:98).

**falta:** LASTRO no ar após os passos anteriores da cadeia; prova atual de `CSF_RETRATADO = 0` **[não verificado]**; estado final do delta de contador e mutante da O169 **[não verificado]**. A cadeia proíbe o verde antes do passo 3. [app/docs/BACKLOG.md:1960](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1960), [app/docs/BACKLOG.md:1544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1544), [app/docs/BACKLOG.md:2254](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:2254).

**perguntas de lei: 0** identificadas nesta leitura do passo 4. [não verificado para os passos 2 e 3]

## 19/11 12:02 · sessao 689240b7551f · CADEIA-20-PASSO-1 · construtor · origem=Code

A cura está em [orfa_classe.py:31](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:31): `razao_da_sobra` consulta `_carimbada_como_miolo`, que reconhece os três carimbos ([turnos.py:932](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:932)). Escolhi import local para evitar dependência no carregamento entre os módulos; `ponto.turnos` também importa `escala.orfa_classe` localmente ([turnos.py:1581](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1581)). Os papéis de entrada, saída e aberto continuam lidos dos campos do `Turno` ([orfa_classe.py:65](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:65)).

A fixture agora explicita os três atributos ([teste:13](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:13)). O novo caso usa somente `_intra_ata=True` ([teste:112](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:112)): [nao verificado]

- **RED → verde:** antes da cura, retornou `PERIODO_ADICIONAL` e falhou; depois, passou. No executor isolado, os 13 testes passaram. [nao verificado]
- **Mutante:** substituir a chamada por `getattr(b, 'is_intervalo', False)` fez o novo caso falhar com `PERIODO_ADICIONAL`; mutante morto.
- A execução Django real e a certificação pela suíte permanecem **[não verificado]**: nesta cópia, `python` não está disponível e `python3` não encontra `django.core`. [nao verificado]

Para reproduzir os resultados isolados, execute o bloco abaixo no diretório `app`. Ele usa o teste da cópia e extrai a função de autoridade do arquivo real; a mutação ocorre somente em memória. [nao verificado]

```bash
verificar() {
  PYTHONDONTWRITEBYTECODE=1 MUTANTE="$1" python3 - <<'PY'
import ast, collections, os, sys, types, unittest

source = open('ponto/turnos.py').read()
node = next(n for n in ast.parse(source).body
            if isinstance(n, ast.FunctionDef) and n.name == '_carimbada_como_miolo')
ponto = types.ModuleType('ponto.turnos')
exec(compile(ast.Module(body=[node], type_ignores=[]), 'ponto/turnos.py', 'exec'), ponto.__dict__)
ponto.Turno = collections.namedtuple(
    'Turno', 'data_turno entrada saida batidas aberto cross_meianoite')
sys.modules['ponto.turnos'] = ponto

django = types.ModuleType('django')
django.__path__ = []
test = types.ModuleType('django.test')
test.SimpleTestCase = unittest.TestCase
sys.modules.update({'django': django, 'django.test': test})

if os.environ['MUTANTE'] == '1':
    source = open('escala/orfa_classe.py').read()
    old = '_carimbada_como_miolo(b) for b, _ in encontrados'
    assert source.count(old) == 1
    source = source.replace(old, "getattr(b, 'is_intervalo', False) for b, _ in encontrados")
    module = types.ModuleType('escala.orfa_classe')
    exec(compile(source, 'escala/orfa_classe.py', 'exec'), module.__dict__)
    sys.modules['escala.orfa_classe'] = module
    nome = 'escala.tests.test_razao_da_sobra.RazaoDaSobraTest.test_carimbo_intra_ata_no_turno'
else:
    nome = 'escala.tests.test_razao_da_sobra'

suite = unittest.defaultTestLoader.loadTestsFromName(nome) [nao verificado]
sys.exit(not unittest.TextTestRunner(verbosity=2).run(suite).wasSuccessful()) [nao verificado]
PY
}
verificar 0  # verde: 13 testes [nao verificado]
verificar 1  # mutante: falha esperada no novo caso
```

Com Django disponível, o comando do teste real é: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_razao_da_sobra --noinput
```

## 19/11 12:01 · sessao 848b07639c50 · ESCALA-NAO-DESCREVE · sonda · origem=Code

**Resultado:** a sonda informa 19 casos `nenhuma`, 9 `nova==cadastro` e 2 `velha==cadastro`; no corte A3, informa 15 chaves com mais de um bloco, em 9 colaboradores. A saída informa `ERROS=0` e `CONTADOS=34 (esperado 34)`. [não verificado: saída fornecida no pedido, sem arquivo e linhas locais]

Os valores abaixo são transcrições do envelope fornecido, não uma nova leitura da sombra. Como o envelope não tem arquivo:linha disponível na cópia, suas afirmações estão marcadas `[não verificado]`. O diretório também não tem metadados Git disponíveis para confirmar o hash do HEAD. [não verificado]

## 1. As 30 linhas A1

Em **cadastro**, **velha** e **nova**, a notação é `pk: entrada–saída / início–fim do intervalo (cruza)`. `—` significa sem intervalo; `∅`, sem proposta. Em **civil** e **núcleo**, a notação é `entrada/saída; marcos; dias`. As classes comparam os marcos informados, não a igualdade dos PKs. [não verificado: linhas A1 fornecidas]

| Vínculo:colab | Cadastro | Velha | Nova | Civil | Núcleo | Classe |
|---|---|---|---|---|---|---|
| 1140:85 | 483: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 378: 21:00–07:00/— (1) | 06:57/06:57; 1; 24 | 20:50/06:57; 2; 12 | nenhuma |
| 821:110 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:55/07:00; 1; 25 | 18:55/07:00; 4; 11 | nenhuma |
| 829:111 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:55/07:00; 1; 44 | 18:55/07:00; 4; 22 | nenhuma |
| 832:121 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:59/07:01; 1; 60 | 19:00/07:01; 4; 26 | nenhuma |
| 1027:134 | 179: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 06:58/06:58; 1; 44 | 18:55/06:58; 2; 22 | nova==cadastro |
| 187:219 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 19:04/07:04; 1; 60 | 19:04/07:03; 4; 28 | nenhuma |
| 219:253 | 378: 21:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 06:55/06:55; 1; 60 | 18:56/06:55; 2; 30 | nenhuma |
| 238:272 | 235: 18:00–06:00/01:00–02:00 (1) | 182: 18:00–06:00/— (1) | 235: 18:00–06:00/01:00–02:00 (1) | 17:52/05:55; 1; 34 | 17:52/05:55; 4; 16 | nova==cadastro |
| 247:282 | 172: 19:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:59/07:00; 3; 58 | 18:59/07:00; 4; 30 | velha==cadastro |
| 255:290 | 179: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:00/07:00; 1; 57 | 19:00/07:00; 2; 28 | nova==cadastro |
| 1576:340 | 172: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 209: 18:30–06:30/— (1) | 06:30/06:30; 1; 45 | 18:33/06:30; 2; 21 | nenhuma |
| 302:344 | 186: 19:00–07:00/23:00–00:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:43/06:59; 1; 28 | 18:43/06:59; 4; 14 | nenhuma |
| 1336:416 | 196: 19:00–07:00/03:00–04:00 (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:00/07:00; 1; 42 | 18:55/06:56; 2; 19 | nenhuma |
| 1334:418 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 19:00/07:00; 3; 45 | 19:00/07:00; 4; 22 | nenhuma |
| 401:467 | 183: 19:00–07:00/00:00–01:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:51/06:51; 3; 51 | 18:51/06:51; 4; 24 | nenhuma |
| 425:496 | 179: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:00/07:00; 1; 52 | 18:58/07:00; 2; 23 | nova==cadastro |
| 1150:502 | 278: 10:00–18:20/14:00–15:00 (0) | 197: 07:00–19:00/14:00–15:00 (0) | 204: 10:20–19:29/14:40–15:40 (0) | 06:59/19:00; 4; 50 | 09:54/19:00; 4; 46 | nenhuma |
| 1164:511 | 186: 19:00–07:00/23:00–00:00 (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:01/07:01; 1; 57 | 18:58/07:01; 2; 27 | nenhuma |
| 950:612 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:50/06:50; 1; 34 | 18:50/06:50; 4; 15 | nenhuma |
| 994:841 | 380: 19:00–07:00/01:00–02:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:56/06:54; 1; 58 | 18:56/06:54; 4; 29 | nova==cadastro |
| 1006:854 | 34: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 06:51/06:51; 1; 60 | 18:55/06:51; 2; 30 | nova==cadastro |
| 1337:877 | 203: 10:00–22:00/14:00–15:00 (0) | 193: 10:30–22:30/15:30–16:30 (0) | 185: 10:00–22:00/13:00–14:00 (0) | 10:56/22:09; 4; 28 | 09:59/22:09; 4; 25 | nenhuma |
| 1478:888 | 172: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:05/07:05; 1; 58 | 19:01/07:05; 2; 28 | nova==cadastro |
| 1388:898 | 253: 12:30–00:30/19:00–20:00 (1) | 314: 00:00–00:00/— (1) | 253: 12:30–00:30/19:00–20:00 (1) | 00:30/00:30; 3; 45 | 12:28/00:30; 4; 22 | nova==cadastro |
| 1594:900 | 35: 07:00–19:00/— (0) | 212: 07:00–16:00/— (0) | 365: 07:00–15:50/12:00–13:00 (0) | 06:55/15:54; 1; 39 | 06:54/15:54; 4; 28 | nenhuma |
| 1181:926 | 496: 11:00–20:00/16:00–17:00 (0) | 541: 11:00–20:00/15:30–17:00 (0) | 291: 08:00–20:00/12:00–13:00 (0) | 11:00/20:00; 4; 43 | 08:00/20:00; 4; 30 | nenhuma |
| 1592:932 | 35: 07:00–19:00/— (0) | 274: 10:00–22:00/— (0) | 177: 07:00–19:00/— (0) | 10:00/22:00; 2; 30 | 06:52/18:54; 2; 18 | nova==cadastro |
| 1186:935 | 172: 19:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:50/06:50; 3; 58 | 18:50/06:50; 4; 29 | velha==cadastro |
| 1511:993 | 178: 07:00–19:00/12:00–13:00 (0) | 197: 07:00–19:00/14:00–15:00 (0) | 177: 07:00–19:00/— (0) | 07:00/19:00; 4; 18 | 07:00/19:00; 2; 14 | nenhuma |
| 1123:915 | 279: 14:40–23:00/18:00–19:00 (0) | 449: 12:00–20:20/16:00–17:00 (0) | ∅; cruza — | 11:48/20:24; 4; 19 | 11:48/17:27; 4; 16 | nenhuma |

**CLASSES:** `nenhuma=19 nova==cadastro=9 velha==cadastro=2`. [não verificado: linha CLASSES fornecida]

### Por que a proposta muda

O detector civil agrupa batidas pela data local e toma, para cada dia, a primeira e a última; calcula separadamente as modas dessas bordas e do número de batidas. O núcleo, no ramo sequencial, corta blocos por gaps e vão, atribui cada bloco à data da primeira batida e acumula suas horas nessa chave. Isso permite que madrugada e noite sejam separadas no civil e reunidas no núcleo. [app/escala/detector_proposta.py:53](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:53), [app/ponto/calculador/nucleo.py:186](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:186)

O casamento exige entrada e saída a até 30 minutos do template e escolhe a menor soma das distâncias. Para intervalo, testa apenas se `marcos_moda >= 4` e se o template **tem** os dois marcos de intervalo; não compara seus horários com batidas ou cadastro. O código tampouco usa `cruza` nesse casamento. [app/escala/detector_proposta.py:34](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:34), [app/escala/detector_proposta.py:99](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:99)

- **Entrada muda de madrugada para noite:** 1140:85, 1027:134, 219:253, 255:290, 1576:340, 1336:416, 425:496, 1164:511 e 1006:854 e 1478:888 passam de entrada civil próxima da saída para entrada de bloco noturna; 1388:898 também muda a entrada e a moda de marcos. A troca observada acompanha essa diferença de chave e de bordas. [não verificado: linhas A1 fornecidas; mecanismo em [detector_proposta.py:60](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:60) e [nucleo.py:236](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:236)]
- **Mesmas bordas aproximadas, intervalo muda:** 821:110, 829:111, 832:121, 187:219, 238:272, 247:282, 302:344, 1334:418, 401:467, 950:612, 994:841, 1594:900 e 1186:935 têm moda civil de 1 ou 3 e nuclear de 4; 1511:993 faz o inverso, de 4 para 2. Isso muda o filtro de presença de intervalo. O horário específico do intervalo escolhido continua dependendo do catálogo e da ordem em empate, não de comparação dos marcos internos. [não verificado: linhas A1 fornecidas; regra em [detector_proposta.py:105](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:105)]
- **Bordas modais diferentes com mesma categoria de marcos:** em 1150:502, 1337:877, 1181:926 e 1592:932 a entrada medida muda e, com ela, o candidato mais próximo. O envelope não contém as batidas completas nem todos os candidatos para atribuir a diferença modal a um evento individual. [não verificado: linhas A1 fornecidas; regra em [detector_proposta.py:113](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:113)]
- **1123:915:** a saída modal muda de 20:24 para 17:27; a referência informa proposta nova `NONE` e confiança insuficiente. Com 16 dias, o limite de cinco dias não explica a ausência; o código também retorna insuficiente quando nenhum template casa as bordas e a categoria de intervalo. O catálogo completo não foi fornecido para verificar os candidatos individualmente. [não verificado: A1 e referência fornecidos; regra em [detector_proposta.py:125](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:125)]

## 2. A2: batidas e medições por dia

Datas são de novembro de 2026. Em cada linha, **núcleo** mostra `chave: horas; ímpar`; `∅` reproduz `horas=None`. A chave impressa é a data da linha mesmo quando não há horas nessa chave. As tabelas reproduzem somente as linhas A2 fornecidas, sem inventar dias ausentes. [não verificado: linhas A2 fornecidas]

**845:146** — cadastro pk 383, `00:00–23:59/—`, cruza 0; velha pk 39; `nbatidas=6`. [não verificado]

| Dia | Batidas locais | Civil E/S; n | Núcleo: chave, horas; ímpar |
|---|---|---|---|
| 09 | 06:30 | 06:30/06:30; 1 | 09: ∅; falso |
| 10 | 21:29 | 21:29/21:29; 1 | 10: 21:29, 06:31; falso |
| 11 | 06:31 | 06:31/06:31; 1 | 11: ∅; falso |
| 16 | 21:31 | 21:31/21:31; 1 | 16: 21:31, 06:35; falso |
| 17 | 06:35 | 06:35/06:35; 1 | 17: ∅; falso |

O civil vê uma batida em cada data exibida, portanto primeira e última coincidem. O núcleo reúne as duas noites exibidas com suas saídas da manhã seguinte. Nenhuma dessas bordas reproduz o cadastro `00:00–23:59`; a referência de outra janela informa nova pk 500 e E/S nuclear modal `21:29/06:30`. A sexta batida contada no cabeçalho não aparece nas linhas A2 transcritas. [não verificado: A2 e sonda34d_ref.out fornecidos; agrupamento em [detector_proposta.py:60](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:60) e [nucleo.py:236](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:236)]

**1547:220** — cadastro pk 591, `22:00–06:00/02:00–03:00`, cruza 1; velha pk 252; `nbatidas=24`. [não verificado]

| Dia | Batidas locais | Civil E/S; n | Núcleo: chave, horas; ímpar |
|---|---|---|---|
| 10 | 21:54 | 21:54/21:54; 1 | 10: 21:54, 03:35, 04:31, 06:00; falso |
| 11 | 03:35, 04:31, 06:00 | 03:35/06:00; 3 | 11: ∅; falso |
| 12 | 21:59 | 21:59/21:59; 1 | 12: 21:59, 04:01, 05:00, 06:00; falso |
| 13 | 04:01, 05:00, 06:00, 22:00 | 04:01/22:00; 4 | 13: 22:00, 02:00, 03:02, 06:00; falso |
| 14 | 02:00, 03:02, 06:00, 21:58 | 02:00/21:58; 4 | 14: 21:58, 02:02, 03:00, 06:00; falso |
| 15 | 02:02, 03:00, 06:00, 21:57 | 02:02/21:57; 4 | 15: 21:57, 02:09, 03:00, 06:00; falso |
| 16 | 02:09, 03:00, 06:00 | 02:09/06:00; 3 | 16: ∅; falso |
| 17 | 21:54 | 21:54/21:54; 1 | 17: 21:54, 02:00, 03:00, 06:01; falso |
| 18 | 02:00, 03:00, 06:01 | 02:00/06:01; 3 | 18: ∅; falso |

Nesses dias, o civil separa ou mistura bordas de jornadas em datas civis; o núcleo mostra jornadas noturnas próximas ao cadastro, embora alguns intervalos exibidos sejam diferentes. A referência de janela mais ampla informa, porém, nova e velha pk 252 e E/S nuclear modal `15:00/23:00`, divergente do cadastro. **A2 não mostra por que essa moda da janela completa prevalece.** [não verificado: A2 e sonda34d_ref.out fornecidos; regras em [detector_proposta.py:70](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:70) e [nucleo.py:236](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:236)]

**1597:489** — cadastro pk 172, `19:00–07:00/—`, cruza 1; velha pk 252; `nbatidas=23`. [não verificado]

| Dia | Batidas locais | Civil E/S; n | Núcleo: chave, horas; ímpar |
|---|---|---|---|
| 09 | 16:01, 20:00, 21:00 | 16:01/21:00; 3 | 09: 16:01, 20:00, 21:00, 00:00; falso |
| 10 | 00:00 | 00:00/00:00; 1 | 10: ∅; falso |
| 11 | 19:00, 23:00 | 19:00/23:00; 2 | 11: 19:00, 23:00, 00:00, 07:00; falso |
| 12 | 00:00, 07:00 | 00:00/07:00; 2 | 12: ∅; falso |
| 13 | 19:00, 23:00 | 19:00/23:00; 2 | 13: 19:00, 23:00, 00:00, 07:00; falso |
| 14 | 00:00, 07:00 | 00:00/07:00; 2 | 14: ∅; falso |
| 15 | 19:00, 23:00 | 19:00/23:00; 2 | 15: 19:00, 23:00, 00:00, 07:00; falso |
| 16 | 00:00, 07:00 | 00:00/07:00; 2 | 16: ∅; falso |
| 18 | 19:00, 23:00 | 19:00/23:00; 2 | 18: 19:00, 23:00, 00:00; verdadeiro |

O civil divide as jornadas de 19:00–07:00 entre datas. O núcleo as reúne, mas as linhas completas exibidas têm quatro marcos, enquanto o cadastro não tem intervalo; a última é ímpar. A referência da janela mais ampla informa nova e velha pk 252 e moda nuclear `15:00/23:00`; os dez dias exibidos não explicam a prevalência dessa moda. [não verificado: A2 e sonda34d_ref.out fornecidos; regra de intervalo em [detector_proposta.py:105](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:105)]

**1604:934** — cadastro pk 590, `23:00–11:00/05:00–06:00`, cruza 1; velha pk 177; `nbatidas=10`. [não verificado]

| Dia | Batidas locais | Civil E/S; n | Núcleo: chave, horas; ímpar |
|---|---|---|---|
| 09 | 22:57 | 22:57/22:57; 1 | 09: 22:57, 11:09; falso |
| 10 | 11:09 | 11:09/11:09; 1 | 10: ∅; falso |
| 11 | 23:01 | 23:01/23:01; 1 | 11: 23:01, 11:00; falso |
| 12 | 11:00 | 11:00/11:00; 1 | 12: ∅; falso |
| 13 | 22:57 | 22:57/22:57; 1 | 13: 22:57, 11:00; falso |
| 14 | 11:00 | 11:00/11:00; 1 | 14: ∅; falso |
| 15 | 22:58 | 22:58/22:58; 1 | 15: 22:58, 11:04; falso |
| 16 | 11:04 | 11:04/11:04; 1 | 16: ∅; falso |
| 17 | 23:27 | 23:27/23:27; 1 | 17: 23:27, 11:40; falso |
| 18 | 11:40 | 11:40/11:40; 1 | 18: ∅; falso |

O civil vê uma batida por data; o núcleo reúne cada entrada com a saída da manhã seguinte, mas nessas jornadas mostra dois marcos, sem os dois marcos de intervalo do cadastro. A referência da janela mais ampla informa nova pk 197 e E/S nuclear modal `07:01/19:00`; os dez dias exibidos não explicam essa moda. [não verificado: A2 e sonda34d_ref.out fornecidos; regras em [detector_proposta.py:70](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:70) e [detector_proposta.py:105](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:105)]

## 3. A3: contagem de chaves multibloco

O corte referenciado da autópsia deduplica batidas a menos de dez minutos e separa blocos por gap; essa contagem replicada não deve ser confundida automaticamente com o corte atual do núcleo, que não faz esse dedup e também tem limite de vão. [app/ponto/management/commands/e6_piso_autopsia.py:65](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/e6_piso_autopsia.py:65), [app/ponto/calculador/nucleo.py:45](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:45), [app/ponto/calculador/nucleo.py:215](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:215)

| Colab pk | Chaves | Chaves com >1 bloco |
|---|---:|---:|
| 109 | 27 | 1 |
| 328 | 34 | 1 |
| 451 | 54 | 1 |
| 466 | 30 | 2 |
| 467 | 25 | 1 |
| 638 | 23 | 1 |
| 877 | 28 | 1 |
| 911 | 40 | 4 |
| 932 | 29 | 3 |

**A3_TOTAL:** `universo_colabs=233 chaves=7524 chaves_multibloco=15 colabs_com_multibloco=9`. [não verificado: A3 fornecido]

**Controle da sonda principal:** `ERROS=0 UNIVERSO=233 CONTADOS=34 (esperado 34)`. **Controle do anexo de referência:** `ERROS=0 UNIVERSO=233 CONTADOS=233`. [não verificado: envelopes fornecidos]

_7349 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
