# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 19:11** · publicado em **12/11/2026 19:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 19:11 · sessao d9121af81e22 · TIPO-NA-FONTE · construtor · origem=Code

Entrega registrada no [relato v2](docs/tipo_na_fonte_orfas_relato_v2.txt:1), com os oito itens do censo, REDs, comandos e INCOMPLETO.

- **47 testes dirigidos passaram, rc=0.** [Registro](docs/tipo_na_fonte_orfas_execucao_v2.txt:251)
- **Sete mutantes foram rejeitados.** [Comandos e resultados](docs/tipo_na_fonte_orfas_execucao_v2.txt:307)
- Suite Django e integração completa do PDF não executadas; certificação Code/opus pendente. [Limites](docs/tipo_na_fonte_orfas_relato_v2.txt:101)

## 12/11 19:00 · sessao 16931418c3a8 · - · - · origem=Ronald (tmux)

Achados para corrigir ou declarar:

- **Alta:** legado direto `orfas=['08:00']` alcança `h.strftime` e quebra; RED atual cobre somente projeção `None` com `n_orfas`. `ponto/supra_juiz.py:520`.
- **Alta:** um único dia desconhecido transforma a janela PDF inteira em `None`, apagando identificações conhecidas no contador. `relatorios/pdf_espelho.py:492`.
- **Alta:** período sem entrada correspondente nas batidas passa a suprimir todos os alertas, ampliando o comportamento anterior. `relatorios/pdf_espelho.py:195`, `relatorios/pdf_espelho.py:990`.
- **Média:** `_furos_do_dia` mudou de três para quatro posições; três expectativas vizinhas precisam acompanhar. `escala/tests/test_cobparc_rotulo_furos_diarios.py:34`, `:46`, `:50`.
- **Limite comprovado:** `copy().get`, `dict(ata).get` e `pop` escapam da contagem de `AtaVigiada`; executei os três sem Django: `leituras_acessor=1`, `acessos_crus_ata=0`. Interceptadores atuais: `escala/services/ata.py:68`.
- **Limite de cobertura:** a catraca dirigida executa tripwire, esmeril e duas portas; não sustenta afirmação de unicidade global. `escala/tests/test_selo_tipo_na_fonte_orfas.py:25`.

Não escrevi arquivos nem executei Django/banco/rede. [nao verificado]

## 12/11 18:55 · sessao 81eaddfdbeb2 · TRANCA-VIVAS-08 · sonda · origem=Code

Transcrição integral das linhas de ids e contagens fornecidas; valores de produção [nao verificado]:

```text
comp emp2 04/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp2 05/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp2 06/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp2 07/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp2 08/2026 perguntas=0 chamados=207 disputas=10 com_o_admin=4 vivas_apesar_da_tranca=0
comp emp3 04/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp3 05/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp3 06/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp3 07/2026 perguntas=0 chamados=1 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp3 08/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 04/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 05/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 06/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 07/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
comp emp4 08/2026 perguntas=0 chamados=0 disputas=0 com_o_admin=0 vivas_apesar_da_tranca=0
TOTAL perguntas=0 chamados=208 disputas=10 com_o_admin=4 vivas_apesar_da_tranca=0
emp2 08/2026 perguntas n=0 ids=[]
emp2 08/2026 vivas_apesar_da_tranca n=0 ids=[]
emp2 08/2026 vivas_resgatadas n=0 ids=[]
emp2 08/2026 vivas_tocadas_pelo_admin n=0 ids=[]
emp2 08/2026 fora_conversa n=0 ids=[]
emp2 08/2026 decidir_pelo_dp n=4 ids=[20599, 47879, 47881, 47884]
alvo_ainda_vivas_apesar=[]
total_erros=0
```

Montagem das seis listas explicitadas para emp2 08/2026, na cópia:

| Lista | Linha e condição |
|---|---|
| `perguntas` | Inicializada vazia em [competencia_trancada.py:90](app/chamados/services/competencia_trancada.py:90); permanece vazia por construção, conforme [competencia_trancada.py:135](app/chamados/services/competencia_trancada.py:135). |
| `vivas_apesar_da_tranca` | Recebe `q.pk` quando `tocou` é falso, em [competencia_trancada.py:158](app/chamados/services/competencia_trancada.py:158). |
| `vivas_resgatadas` | Recebe `q.pk` quando a pergunta é resgatada e viva, em [competencia_trancada.py:149](app/chamados/services/competencia_trancada.py:149). |
| `vivas_tocadas_pelo_admin` | Recebe `q.pk` quando `tocou` é verdadeiro, em [competencia_trancada.py:158](app/chamados/services/competencia_trancada.py:158); consulta do toque em [competencia_trancada.py:155](app/chamados/services/competencia_trancada.py:155). |
| `fora_conversa` | Recebe `q.pk` quando `_dia_de_turno_da_pergunta(q)` retorna `None`, em [competencia_trancada.py:142](app/chamados/services/competencia_trancada.py:142). |
| `decidir_pelo_dp` | Recebe `q.pk` quando não é resgatada e pertence a `_sem_veredito`, em [competencia_trancada.py:139](app/chamados/services/competencia_trancada.py:139); origem desse conjunto em [competencia_trancada.py:126](app/chamados/services/competencia_trancada.py:126). |

As listas representadas pelas outras contagens são montadas em [competencia_trancada.py:171](app/chamados/services/competencia_trancada.py:171) (`chamados`) e [competencia_trancada.py:183](app/chamados/services/competencia_trancada.py:183) (`disputas`). `com_o_admin` corresponde à contagem da união, sem repetição, de `decidir_pelo_dp`, `vivas_resgatadas` e `vivas_tocadas_pelo_admin`, formada em [competencia_trancada.py:343](app/chamados/services/competencia_trancada.py:343).

**O degrau efetivamente decisivo por id não está demonstrado pelo dado fornecido [nao verificado].**

| ID pedido | O que consta no dado | Degrau efetivamente percorrido |
|---|---|---|
| `20798` | Não aparece na saída transcrita [nao verificado]. | [nao verificado] |
| `47879` | Consta em `decidir_pelo_dp` [nao verificado]. | [nao verificado] |
| `47881` | Consta em `decidir_pelo_dp` [nao verificado]. | [nao verificado] |
| `47884` | Consta em `decidir_pelo_dp` [nao verificado]. | [nao verificado] |

O código permite localizar o degrau da **leitura da tranca**: retorna `False` em [juizes.py:1405](app/chamados/juizes.py:1405). Entretanto, a validação já retorna `False` antes dele, em [juizes.py:1401](app/chamados/juizes.py:1401), e existem outros retornos posteriores; portanto, resultado agregado não identifica sozinho o ramo executado. Atribuir especificamente o degrau da tranca a cada id permanece [nao verificado].

Além disso, `decidir_pelo_dp` é preenchida **antes** de consultar `pergunta_viva`, em [competencia_trancada.py:139](app/chamados/services/competencia_trancada.py:139) e [competencia_trancada.py:144](app/chamados/services/competencia_trancada.py:144). Sua presença nessa lista não registra qual degrau do juiz decidiu a pergunta.

## 12/11 18:52 · sessao 02c4b526a44e · TIPO-ITEM1-DIMENSAO · verificador · origem=Code

**1. PROCEDENTE — cura parcial; o “PRONTO” do aval ainda não está demonstrado.** Os contraexemplos exatos de acesso anterior à reatribuição e de contaminação dos dois sítios reais foram corrigidos, mas permanecem perdas, proveniências falsas e mutantes sobreviventes. (`bin/leitores_de_chave.py:318`, `bin/leitores_de_chave.py:351`, `bin/tests/test_leitores_de_chave.sh:204`)

Executei reproduções com ASTs e mutantes somente em memória, usando `python3 -B`. O selo integral, que cria e remove temporários, **não foi executado**. Identidade Git do commit e comportamento em produção: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:33`, `bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:41`)

**2. PROCEDENTE — destino de cada achado do preverif v2.**

| Achado | Resultado |
|---|---|
| Semente de laço com prioridade e escopo inteiro | **Curado nos exemplos exatos; parcial no geral.** A semente de laço agora soma ao alias e exige pertencimento ao corpo/comprehension. Na cópia, desapareceram as contaminações em materialização e diagnóstico, preservando `lampadas.[].luz`. Persistem reatribuição interna, sombreamento e laços aninhados, detalhados abaixo. (`bin/leitores_de_chave.py:239`, `bin/leitores_de_chave.py:351`; `app/chamados/services/materializacao.py:177`; `app/ponto/services/diagnostico_escala.py:154`) |
| Reatribuição posterior apaga acesso válido anterior | **Curado no caso linear exato:** `a=c.ata; a["k"]; a={}` preserva `k` e registra lacuna. Não resolve fluxo de ramos nem todos os ciclos de aliases. (`bin/leitores_de_chave.py:331`, `bin/leitores_de_chave.py:335`, `bin/leitores_de_chave.py:337`) |
| RETURNS aceita `return 42` / `return None` | **Curado quando são os únicos retornos reconhecíveis.** Ambos retiram a função de `RET_EF` e geram lacuna. Retornos mistos, definições duplicadas e ciclos ainda permitem proveniência indevida. (`bin/leitores_de_chave.py:228`, `bin/leitores_de_chave.py:708`, `bin/leitores_de_chave.py:712`) |
| Falta mutante de RETURNS | **Curada a ausência; cobertura parcial.** k existe e exige a lacuna após trocar os retornos por `42`, mas não exige que chamadas deixem de receber proveniência. (`bin/tests/test_leitores_de_chave.sh:91`, `bin/tests/test_leitores_de_chave.sh:101`) |
| “Catraca” no selo | **Curado literalmente:** o selo apresenta dimensão e cota inferior. Ainda imprime `acessos_crus_ata = N` em a, preservando o nome associado à antiga catraca. (`bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:247`) |

**3. PROCEDENTE — posição do valor não representa execução nem conclusão da atribuição.**

Os dois exemplos abaixo foram reproduzidos: o primeiro perde `k` apesar de o ramo não executar; o segundo atribui `ys.k`, sem lacuna, embora o ramo escolhido use `xs`. A lacuna depende de misturar proveniência vazia e não vazia; dois caminhos JSON diferentes não a acionam. (`bin/leitores_de_chave.py:331`, `bin/leitores_de_chave.py:335`, `bin/leitores_de_chave.py:338`)

```python
def perda(c):
    a = c.ata
    if False:
        a = {}
    a["k"]

def caminho_errado(c):
    if True:
        a = c.ata["xs"]
    else:
        a = c.ata["ys"]
    a["k"]
```

Há também erro sem ramo: cada componente da tupla recebe a posição final do respectivo valor, antes de Python concluir toda a avaliação do lado direito. Neste exemplo, a ferramenta conta a leitura como de `c.ata`, mas Python ainda lê o dicionário anterior. (`bin/leitores_de_chave.py:274`, `bin/leitores_de_chave.py:331`, `bin/leitores_de_chave.py:337`)

```python
def atribuicao(c):
    a = {"k": 0}
    a, b = c.ata, a["k"]
```

**4. PROCEDENTE — `try`, `while`, `with`, `global/nonlocal` e `AugAssign` continuam fora da resolução correta.** Nem todos são defeitos introduzidos nesta obra: o novo resolvedor continua indexando somente atribuições específicas e alvos simples de `for`; não passou a interpretar os demais bindings. (`bin/leitores_de_chave.py:159`, `bin/leitores_de_chave.py:174`, `bin/leitores_de_chave.py:318`)

- **`try`:** uma atribuição textualmente anterior pode ser inalcançável. A reprodução abaixo perde `k` e emite apenas lacunas de alias. (`bin/leitores_de_chave.py:337`)
- **`while`:** `a=c.ata; while False: a={}` seguido de `a["k"]` perde `k`. Na direção inversa, a atribuição ao final do corpo não determina o binding da próxima volta. (`bin/leitores_de_chave.py:331`, `bin/leitores_de_chave.py:338`)
- **`with`:** `optional_vars` não entra no ambiente; o exemplo abaixo atribui indevidamente `k` à ata, sem lacuna. (`bin/leitores_de_chave.py:159`, `bin/leitores_de_chave.py:174`)
- **`global/nonlocal`:** a consulta usa somente `env[s]`, sem seguir essas declarações. Um alias da ata no módulo ou na função externa pode desaparecer no consumidor, sem lacuna. (`bin/leitores_de_chave.py:254`, `bin/leitores_de_chave.py:332`)
- **`AugAssign`:** há tratamento de papéis para subscripts, mas `a |= {"k":1}` não é indexado nem emitido como escrita; com `a=c.ata`, essa é mutação real do dicionário. (`bin/leitores_de_chave.py:160`, `bin/leitores_de_chave.py:511`, `bin/leitores_de_chave.py:641`)

```python
def tentativa(c):
    a = c.ata
    try:
        raise ValueError()
        a = {}
    except ValueError:
        pass
    a["k"]

def contexto(c, cm):
    a = c.ata
    with cm() as a:  # suponha __enter__ devolvendo {"k": 0}
        a["k"]
```

**5. PROCEDENTE — a sentinela de `For` corrige um caso e cria perdas; a semente corporal ainda contamina.**

A sentinela é instalada somente para alvo `Name`, na posição do iterável, independentemente de haver iteração. A semente, por sua vez, vale em qualquer descendente do corpo, mesmo após outro binding do mesmo nome. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:180`, `bin/leitores_de_chave.py:252`, `bin/leitores_de_chave.py:351`)

| Contraexemplo reproduzido | Erro observado |
|---|---|
| `a=c.ata; for a in []: pass`, seguido de `a["k"]` | Perde `k`, embora o laço vazio preserve `a` em Python; aparece lacuna de alias. (`bin/leitores_de_chave.py:183`, `bin/leitores_de_chave.py:338`) |
| `for a in c.ata["xs"]: pass`, com `a["k"]` no `else` | Perde `xs.[].k` **sem lacuna**, inclusive quando `xs` contém um dicionário e o laço termina normalmente. O `else` está fora da região semeada. (`bin/leitores_de_chave.py:252`, `bin/leitores_de_chave.py:335`) |
| Laço externo sobre `xs`, laço interno vazio reutilizando `a`, depois `a["k"]` | A semente externa sobrevive à sentinela interna. Se o laço interno receber outro dicionário, também permanece a atribuição indevida a `xs.[].k`. (`bin/leitores_de_chave.py:252`, `bin/leitores_de_chave.py:354`) |
| Alvo `(a,b)` após `a=c.ata` | Não recebe a sentinela; o alias anterior pode continuar valendo dentro e depois do laço, embora Python tenha religado `a`. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:281`) |

Este contraexemplo ainda conta `xs.[].k` sem lacuna: a reatribuição não elimina a semente corporal. (`bin/leitores_de_chave.py:351`)

```python
def reatribuicao_no_corpo(c):
    for a in c.ata["xs"]:
        a = {}
        a["k"]
```

Lambda dentro do corpo também continua contaminável: `for x in c.ata["xs"]:` seguido de `f=lambda x: x["k"]` atribui `xs.[].k`, apesar do parâmetro próprio da lambda. O indexador não cria escopo para lambda. (`bin/leitores_de_chave.py:260`, `bin/leitores_de_chave.py:252`)

**6. PROCEDENTE — a regra de escrita em alvo `Name` perde escrita real e ainda conta rebinding como escrita.**

O exemplo abaixo termina atribuindo `{"k":1}` a `c.ata`, mas a ferramenta não emite o escritor de `k`. Remover somente o novo filtro de `Name`, em memória, recupera esse evento. A perda combina o filtro com o limite de um salto de `write_dict`. (`bin/leitores_de_chave.py:445`, `bin/leitores_de_chave.py:652`)

```python
def escrita_perdida(c):
    a = c.ata
    a = {"k": 1}
    b = a
    d = b
    c.ata = d
```

Na direção contrária, `a=c.ata; a=dict(a,k=1)` emite `k` como ESCRITOR, embora apenas construa uma cópia e religue `a`, sem modificar `c.ata`. Interseção de caminhos não prova identidade do objeto nem destino da cópia. (`bin/leitores_de_chave.py:393`, `bin/leitores_de_chave.py:475`, `bin/leitores_de_chave.py:653`)

Sementes declaradas também mantêm o problema: numa função declarada com parâmetro `ata`, `ata={"k":1}` é contado como escrita, e acessos posteriores seguem recebendo raiz JSON, sem lacuna de reatribuição. (`bin/leitores_de_chave.py:156`, `bin/leitores_de_chave.py:349`, `bin/leitores_de_chave.py:652`)

**Limite da evidência:** a comparação da árvore desta cópia com o mutante que remove somente esse filtro não encontrou eventos adicionais. Portanto, a regressão está demonstrada pelo contraexemplo sintético; ocorrência concreta dessa perda na aplicação atual: **[não verificado]**. (`bin/leitores_de_chave.py:652`)

**7. PROCEDENTE — RETURNS: encadeamento direto funciona; duplicação, ciclos e retornos mistos não.**

Nas reproduções abaixo, `f`/`g` foram declaradas no mapa RETURNS do módulo sintético. (`bin/leitores_de_chave.py:83`, `bin/leitores_de_chave.py:693`)

| Situação | Resultado |
|---|---|
| `f(): return g()`; `g(): return 42` | **Curado:** caem `g` e depois `f`, com duas lacunas; chamadas deixam de semear. (`bin/leitores_de_chave.py:700`, `bin/leitores_de_chave.py:712`) |
| `def f(): return {}` e depois `def f(): return 42` | **Furo:** `defs` guarda a última definição, mas os nós das duas são acumulados no mesmo escopo. O retorno antigo mantém `f` efetiva, sem lacuna. (`bin/leitores_de_chave.py:256`, `bin/leitores_de_chave.py:268`, `bin/leitores_de_chave.py:231`) |
| `f(): return g()`; `g(): return f()` | **Furo:** ambas permanecem efetivas sem origem JSON independente. O ponto fixo começa assumindo todas válidas e o ciclo se sustenta. (`bin/leitores_de_chave.py:397`, `bin/leitores_de_chave.py:697`) |
| Um ramo retorna `{}`, outro retorna `42`/`None` | **Furo:** basta **algum** retorno reconhecível; a chamada que retorna escalar ainda recebe proveniência JSON. (`bin/leitores_de_chave.py:229`, `bin/leitores_de_chave.py:236`) |
| Função declarada em arquivo de teste, retornando `None` | **Separação correta no caso reproduzido:** cai com `secao="testes"`; não aumenta as lacunas de produção da dimensão. (`bin/leitores_de_chave.py:715`, `bin/leitores_de_chave.py:726`) |

Há ainda distinção de saída: `return None` produz lacuna, mas `return` sem expressão também reprova a guarda de declaração e chega a rc 2. (`bin/leitores_de_chave.py:225`, `bin/leitores_de_chave.py:688`)

**8. DUVIDA — não confirmei dependência de ordem entre ata e dna.**

`RET_EF` é global, mas tem entradas separadas por campo, e `_verifica_returns` reinicializa a entrada consultada. A execução em memória `ata → dna → ata` produziu os mesmos eventos e lacunas nas duas passagens de ata. Não há contraprova de contaminação entre esses campos. (`bin/leitores_de_chave.py:96`, `bin/leitores_de_chave.py:100`, `bin/leitores_de_chave.py:697`)

Existe outra persistência global: `run(--sem-declaracoes)` altera permanentemente `PARAMS[field]` e `RETURNS[field]` naquele processo. Isso afeta chamadas posteriores do mesmo campo; não é defeito novo de `RET_EF`. O selo inicia um processo por chamada. (`bin/leitores_de_chave.py:799`; `bin/tests/test_leitores_de_chave.sh:41`)

**9. PROCEDENTE — os três modos comunicam dimensão, mas resta promessa indevida no docstring.**

| Saída normal de medição | Rótulo |
|---|---|
| stderr de `--contar-crus` | Contém “cota inferior”, “nem prova de completude” e o total de lacunas por tipo. (`bin/leitores_de_chave.py:46`, `bin/leitores_de_chave.py:819`) |
| Bloco `dimensao` do JSON | `nota` contém a ressalva; `rotulo` contém os números e tipos. O valor isolado de `rotulo` não contém “cota inferior”. (`bin/leitores_de_chave.py:728`, `bin/leitores_de_chave.py:834`) |
| Linha textual | Contém literalmente “cota inferior, nao e completude”, mais números e tipos. (`bin/leitores_de_chave.py:855`) |

**Permanece falsa** a frase “O que o AST nao reconhece vai a `lacunas`, que saem SEMPRE”: `with`, `nonlocal`, `AugAssign` e `for...else` demonstram omissões sem lacuna. Além disso, erros anteriores à medição retornam sem bloco de dimensão. (`bin/leitores_de_chave.py:5`, `bin/leitores_de_chave.py:160`, `bin/leitores_de_chave.py:252`, `bin/leitores_de_chave.py:803`)

`NAO_COBERTOS` não promete completude; o selo não chama mais a medida de catraca. Entretanto, os falsos positivos demonstrados impedem interpretar o número como limite inferior garantido dos **acessos reais ao JSON**: ele dimensiona eventos classificados pelo reconhecedor, sujeito também a classificação errada. (`bin/leitores_de_chave.py:49`, `bin/leitores_de_chave.py:351`, `bin/leitores_de_chave.py:724`; `bin/tests/test_leitores_de_chave.sh:4`)

**10. PROCEDENTE — mordida dos casos novos é parcial.**

| Caso | Mordida e sobreviventes |
|---|---|
| **j, materialização real** | Cobra ausência da contaminação, mas não exige testemunha positiva nesse sítio. Suprimir toda a análise desse arquivo satisfaria essa asserção: há vacuidade possível do teste negativo isolado. (`bin/tests/test_leitores_de_chave.sh:66`, `bin/tests/test_leitores_de_chave.sh:70`) |
| **j, diagnóstico real** | Mais forte: exige ausência de `lampadas.[].entrada` **e presença** de `lampadas.[].luz`. (`bin/tests/test_leitores_de_chave.sh:67`, `bin/tests/test_leitores_de_chave.sh:71`) |
| **j sintético** | Morde as regressões exatas da ferramenta velha. Porém o mutante novo que devolve a união de todos os bindings passou nas três asserções reproduzidas: TEM, NAO e GAP. Falta proibir, por exemplo, `k_reatrib` de um alias religado depois. **O g existente captura esse mutante global**, pela comparação exata; a sobrevivência é de j isoladamente. (`bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:204`) |
| **k RETURNS** | Não é vazio quanto à troca de retorno: exige ausência prévia da lacuna e pelo menos um `return 42`. Morde a ferramenta velha e o mutante “não emitir lacuna RETURNS”. (`bin/tests/test_leitores_de_chave.sh:97`, `bin/tests/test_leitores_de_chave.sh:102`) |
| **k, retirada da proveniência** | **Não morde.** Substituir somente `RET_EF[field] -= set(caem)` por `pass` mantém exatamente K1, mas chamadas continuam semeadas. Reproduzido: a chamada mutada ainda emite uma chave fictícia. (`bin/leitores_de_chave.py:712`; `bin/tests/test_leitores_de_chave.sh:101`) |
| **Rótulo a/h** | Mordem a ausência do novo prefixo na ferramenta velha. Não cobram a ressalva de completude; h cobra apenas “NAO exclui o acessor” e os números. (`bin/tests/test_leitores_de_chave.sh:61`, `bin/tests/test_leitores_de_chave.sh:220`) |
| **JSON/texto** | j verifica somente as chaves do bloco, não os valores nem a ressalva; não há asserção nova do conteúdo da linha textual. (`bin/tests/test_leitores_de_chave.sh:69`, `bin/tests/test_leitores_de_chave.sh:72`, `bin/leitores_de_chave.py:855`) |

Logo, “todos os casos mordem” não demonstra todas as propriedades anunciadas, especialmente “RETURNS inválido nunca semeia”. (`bin/tests/test_leitores_de_chave.sh:22`, `bin/tests/test_leitores_de_chave.sh:247`)

**11. PROCEDENTE — QUEM ESCREVE e QUEM LÊ: consumidor comprovado é o selo; disjuntor lê outra coisa.**

| Valor | Escritor | Leitor comprovado |
|---|---|---|
| stdout numérico | `run()`, `len(top)`. (`bin/leitores_de_chave.py:815`, `bin/leitores_de_chave.py:821`) | Selo: positividade real, igualdade sintética 10, zero e DNA positivo. (`bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:178`, `bin/tests/test_leitores_de_chave.sh:220`, `bin/tests/test_leitores_de_chave.sh:228`) |
| JSON | `run()`: eventos, resumo global, dimensão e lacunas. (`bin/leitores_de_chave.py:831`) | `_jq()` e asserções do selo. (`bin/tests/test_leitores_de_chave.sh:43`, `bin/tests/test_leitores_de_chave.sh:163`) |
| Texto | `run()`: listagens e linha de dimensão. (`bin/leitores_de_chave.py:849`) | Selo usa sobretudo rc e diagnósticos; não confere a dimensão textual. (`bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:106`) |
| CENSO de brief | Autor do brief: **[não verificado]**. | `disjuntor.py` exige referências `arquivo:linha` em leitores/escritores; não consulta a ferramenta nem confronta seus números. (`bin/disjuntor.py:1543`, `bin/disjuntor.py:1565`) |

A busca na cópia, inclusive arquivos ocultos, não encontrou outro consumidor nominal da ferramenta ou de `CONTA_REATRIBUIDO`; briefs efetivos, logs e leitores externos: **[não verificado]**. O consumo demonstrável está nas chamadas e extrações do selo. (`bin/tests/test_leitores_de_chave.sh:32`, `bin/tests/test_leitores_de_chave.sh:41`, `bin/tests/test_leitores_de_chave.sh:43`)

O JSON conserva as chaves anteriores e acrescenta `dimensao`; não há remoção/renomeação de chave no diff fornecido. A remoção de `CONTA_REATRIBUIDO` acompanha a troca da regra de resolução, sem consumidor externo encontrado na cópia. Dependência externa dessa constante: **[não verificado]**. (`bin/leitores_de_chave.py:48`, `bin/leitores_de_chave.py:318`, `bin/leitores_de_chave.py:833`)

**12. PROCEDENTE — classes de reprovação presentes, com limites.**

- **Selo/RED sem mordida: presente parcialmente.** k não distingue “emitir lacuna” de “retirar proveniência”; j sintético não distingue resolução temporal de união indiscriminada, embora g capture esta última globalmente. (`bin/tests/test_leitores_de_chave.sh:101`, `bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:204`)
- **ESMERIL-SEMÂNTICA: presente.** `ESCRITOR` agrega mutação do JSON, construção de cópia e rebinding declarado; o filtro por interseção não separa esses significados. Já o total da dimensão e `operacoes_topo` derivam do mesmo `top`, portanto não achei duas contas diferentes para esse total. (`bin/leitores_de_chave.py:475`, `bin/leitores_de_chave.py:652`, `bin/leitores_de_chave.py:815`, `bin/leitores_de_chave.py:836`)
- **Derivação paralela: permanece na proveniência manual de PARAMS/RETURNS**, agora filtrada por uma verificação sintática parcial. Não encontrei nova derivação paralela da quantidade entre as três saídas: compartilham `dimensao()`. Implementação da catraca no acessor e seu consumo em produção: **[não verificado]**. (`bin/leitores_de_chave.py:56`, `bin/leitores_de_chave.py:83`, `bin/leitores_de_chave.py:228`, `bin/leitores_de_chave.py:817`)

## 12/11 18:52 · sessao 9c408b3bfeef · - · - · origem=Ronald (tmux)

Recomendações concretas, sem alterações:

- A projeção precisa transportar a contagem pelo novo acessor: hoje lê `n_orfas` diretamente e perde a quantidade do legado HH:MM sem esse campo. [escala/services/leitor_celula.py:414](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/services/leitor_celula.py:414)
- `_celula_furada` precisa decidir pela contagem conhecida, inclusive lista HH:MM sem `n_orfas`; quantidade desconhecida continua `None`. Hoje mistura instantes com leitura direta da contagem. [chamados/juizes.py:510](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/juizes.py:510)
- `furos_diarios._furos_do_dia` transforma desconhecimento em zero; deve usar a contagem e publicar `nao_medidos`. Atenção: a porta exige tuplas de quatro valores e está proibida nesta obra; preservar esse contrato e proteger dias desconhecidos contra exclusão demanda adaptação nos chamadores. [escala/services/furos_diarios.py:22](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/services/furos_diarios.py:22), [ponto/portas/celula.py:1125](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/portas/celula.py:1125), [colaboradores/services/pos_vinculo.py:82](/home/ronald/wt-codex-tipo-na-fonte-184856/app/colaboradores/services/pos_vinculo.py:82)
- `fatos_do_status` deve conservar quantidade conhecida, publicar `nao_medidos` e produzir `tem_batida=None` quando nenhuma outra prova permite decidir presença. Qualquer campo aditivo exige parâmetro correspondente em `status_da_lavra`, pois a porta expande o dicionário com `**fatos`. [ponto/status_dia.py:184](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/status_dia.py:184), [ponto/status_dia.py:113](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/status_dia.py:113), [ponto/portas/celula.py:963](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/portas/celula.py:963)
- `bordas_realizado` deve usar presença trivalente e contar separadamente os desconhecidos. Acrescentar `nao_medidos` em `NOMES` também o publica no command existente. [ponto/services/bordas_realizado.py:49](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/services/bordas_realizado.py:49), [ponto/services/bordas_realizado.py:80](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/services/bordas_realizado.py:80), [ponto/management/commands/bordas_realizado.py:22](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/bordas_realizado.py:22)
- `_score_geometria` deve carregar `nao_medidos` ao lado das quatro dimensões; score desconhecido não pode provar melhora nem geometria perfeita. Seus consumidores são `_melhora_estrita`, `escolher_retratavel` e `_par_embutido_retratavel_geometria`. [ponto/management/commands/detectar_par_relampago.py:140](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/detectar_par_relampago.py:140), [ponto/management/commands/detectar_par_relampago.py:154](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/detectar_par_relampago.py:154), [ponto/management/commands/detectar_par_relampago.py:244](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/detectar_par_relampago.py:244), [ponto/management/commands/detectar_par_relampago.py:267](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/management/commands/detectar_par_relampago.py:267)
- `classificar_causa` deve selecionar dias com órfãs pela quantidade, incluir `nao_medidos` no resultado e impedir conclusões baseadas em ausência quando essa ausência é desconhecida. O resultado é transportado integralmente ao fechamento, permitindo campo aditivo sem mudar modelo. [folha/causas.py:43](/home/ronald/wt-codex-tipo-na-fonte-184856/app/folha/causas.py:43), [folha/causas.py:93](/home/ronald/wt-codex-tipo-na-fonte-184856/app/folha/causas.py:93), [folha/causas.py:100](/home/ronald/wt-codex-tipo-na-fonte-184856/app/folha/causas.py:100), [ponto/services/fechamento.py:1080](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/services/fechamento.py:1080)

## 12/11 18:34 · sessao e230e00fb7f6 · TIPO-NA-FONTE · construtor · origem=Code

**Entrega parcial; não pronta para pouso.** A fonte preserva instantes, mas restam leitores sem contrato de “não sei”. Resultado dirigido: **24 testes, 22 passaram e 2 selos globais falharam**. [Evidência](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_testes.txt:68) [nao verificado]

O [relato completo](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:15) registra arquivos::funções alterados, comandos individuais dos mutantes e limitações.

Foram alterados:

- `ata.py::ler_orfas/gravar_orfas` e `leitor_celula.py::orfas_da_ata/grade_da_celula`: ISO aware; `None` como único desconhecimento; leitor privado apagado. [ata.py:13](/home/ronald/wt-codex-tipo-na-fonte-181049/app/escala/services/ata.py:13), [leitor_celula.py:180](/home/ronald/wt-codex-tipo-na-fonte-181049/app/escala/services/leitor_celula.py:180)
- `utils.py::_celula_do_marco/_match_marcos/montar_grade_prevista/montar_realizado_grade/montar_grade_prevista_periodo_por_turno`: conservação, promoções, dry-run, reconciliação e ata sintética por instante. [Inventário](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:18)
- `cartorio.py::lampadas_do_dia/ata_do_dia/_julgar_colab_corpo`: comparação absoluta e escrita pelo acessor. [cartorio.py:599](/home/ronald/wt-codex-tipo-na-fonte-181049/app/ponto/services/cartorio.py:599)
- Calendário, classificador, propositor, ausência parcial, suprajuiz, espelho, simulação e render do detector receberam adaptações; as limitações restantes estão discriminadas no relato. [Funções e referências](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:26)

Destino de cada leitor do censo:

| Leitor | Resultado e “não sei” |
|---|---|
| `_celula_furada` | Migrado; `None` sem prova escalar de presença. `chamados/juizes.py:510` |
| `_plantio_preenche_furo_na_data` | Migrado; dado ausente não absolve, conforme contrato existente. `chamados/services/materializacao.py:143` |
| `_orfas_da_ata` / `grade_da_celula` | Privado apagado; grade transporta `None`. `escala/services/leitor_celula.py:180`, `:414` |
| `periodos_do_dia` | Migrado; transporta `None` e registra pergunta. `ponto/juiz_batida.py:161`, `:254` |
| Cluster espúrio | Migrado; `None` permanece não medido. `ponto/services/cluster_espurio.py:29` |
| Vigia pareador | Migrado; desconhecimento publica `cego=1`. `ponto/services/vigia_pareador_celula.py:120` |
| Cartão pela célula | Função sem chamador produtivo encontrado apagada; chamadas dinâmicas [não verificado]. `relatorios/cartao_pela_celula.py:277` |
| Tripwire | **Pendente:** `(achados, universo)` não representa não medidos. `ponto/management/commands/tripwire_celulas.py:237` |
| Esmeril A5 | **Pendente:** saída só contém assinaturas positivas. `ponto/services/esmeril_espelho.py:79` |

RED antes/depois:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest escala.tests.test_tipo_na_fonte_orfas -v
```

Antes: seis erros por ausência da API/acessor, observados nesta sessão; transcrição anterior em arquivo [não verificado]. Depois: os mesmos seis passaram. **O RED anterior é de contrato/API**; execução da travessia completa no HEAD original [não verificado]. [Registro](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:39)

Os mutantes de acesso cru/alias, comparação por HH:MM, perda da data na grade, fabricação de instante e escritor persistindo HH:MM foram mordidos. Cada comando está no [relato dos mutantes](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:52); resultados em [testes:7](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_testes.txt:7) e [testes:19](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_testes.txt:19). [nao verificado]

Catraca:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 tools/censo_orfas.py CelulaDia.ata orfas
```

Antes pelo mesmo scanner: **[não verificado]**. O anexo informa 21 entradas incluindo cópias; não afirmo redução comparável `21→2`. Depois: **2 acessos crus**, teto declarado **2**, meta zero pendente. [Censo](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_censo.txt:1)

Quem escreve agora: `ata.py::gravar_orfas`, chamado pelo cartório e pela ata sintética. Quem lê o JSON: `ata.py::ler_orfas`, pela porta pública; **tripwire e esmeril ainda leem cru**. [Escrita e leitura](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:87)

**INCOMPLETO:**

- Perguntas de desenho pendentes para tripwire e esmeril A5. `ponto/management/commands/tripwire_celulas.py:237`; `ponto/services/esmeril_espelho.py:79`.
- PDF: contagem desconhecida e supressão de anomalias sem identidade temporal. `relatorios/pdf_espelho.py:513`, `:716`.
- Contadores de projeção, cadeia completa do propositor e fixtures dos pontos interrompidos. [Lista detalhada](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:96)
- Suite Django, integração/render e certificação Code/opus **[não verificado]**; `django.test` indisponível nesta cópia. [Registro](/home/ronald/wt-codex-tipo-na-fonte-181049/app/docs/tipo_na_fonte_orfas_relato.txt:49) [nao verificado]

LEI-AKITA: origem=cartorio.ata_do_dia/escala.utils (grade), testemunha=leitor_celula.orfas_da_ata, RED=test_tipo_na_fonte_orfas+test_tipo_na_fonte_orfas_fluxo, quem-mais-le=tripwire/esmeril (docs/tipo_na_fonte_orfas_censo.txt:3), juizes novos=0, padrao=Single Source of Truth

## 12/11 18:18 · sessao 0d22f717f4c0 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · verificador · origem=Code

**P1 foi corrigido; P2 continua PROCEDENTE.** A trilha tornou-se obrigatória nos dois ramos, mas o relógio ainda é lido no interior e em vários instantes do mesmo Salvar. (`app/ponto/portas/celula.py:1145`, `app/ponto/portas/celula.py:1163`; `app/colaboradores/services/vinculo.py:646`, `app/colaboradores/services/vinculo.py:314`; `app/docs/PLANO-PADROES.md:92`)

1. **PROCEDENTE 1 — trilha e atomicidade**

- **Sim, ambos levantam se `evento()` devolver `None`:** conservado em `app/ponto/portas/celula.py:1151`; desfeito em `app/ponto/portas/celula.py:1163`. O retorno `None` após falha engolida está em `app/core/observ.py:105` e `app/core/observ.py:139`.
- **DNA, limpeza e evento desfazem juntos no banco nos três pontos de chamada direta:** chamadas `:262` e `:309` sob o `atomic` iniciado em `:220`; chamada `:834` sob o `atomic` iniciado em `:818`. A limpeza possui ainda seu próprio `atomic`, incluindo `salvar_e_emitir` e a trilha obrigatória. (`app/ponto/portas/celula.py:220`, `app/ponto/portas/celula.py:262`, `app/ponto/portas/celula.py:309`, `app/ponto/portas/celula.py:818`, `app/ponto/portas/celula.py:834`, `app/ponto/portas/celula.py:1145`; `app/ponto/portas/celula.py:953`)
- **Ressalva:** o objeto Python já recebeu os `None`; o código não restaura seus atributos quando o banco faz rollback. (`app/ponto/portas/celula.py:1156`)
- **Existe ancestral que captura e segue:** `censo_fase_12x36` captura `Exception`, imprime `RECUSADO` e continua o lote. A captura ocorre depois da saída de `executar_vinculo`, que é `@transaction.atomic`; portanto não confirma aquele ato como sucesso. (`app/ponto/management/commands/censo_fase_12x36.py:421`, `app/ponto/management/commands/censo_fase_12x36.py:427`; `app/colaboradores/services/vinculo.py:169`)
- Os catches históricos de `salvar_tipo` e dos receivers de vínculo cobrem a parte até hoje; o suplemento futuro está fora deles. A view encaminha a exceção para código que relança `RuntimeError`; o chamador do snapshot captura apenas `ValueError`. (`app/escala/services/cadastro_tipo.py:323`, `app/escala/services/cadastro_tipo.py:328`; `app/escala/signals.py:73`, `app/escala/signals.py:81`, `app/escala/signals.py:125`, `app/escala/signals.py:132`; `app/colaboradores/views.py:438`; `app/core/erros.py:75`; `app/chamados/services/acoes_chamado.py:148`)

**O teste de banco tem mordida por construção:** exige `RuntimeError`, faz `refresh_from_db()` e confere ata, julgamento e veredito anteriores. Um mutante que ignore novamente o retorno falha já no `assertRaises`; o ramo conservado também exige a exceção. **Execução efetiva desse teste no banco: [nao verificado].** (`app/ponto/tests/test_cadastro_futuro_c5d_banco.py:38`, `app/ponto/tests/test_cadastro_futuro_c5d_banco.py:43`, `app/ponto/tests/test_cadastro_futuro_c5d_banco.py:56`)

2. **PROCEDENTE 2 — censo do relógio e chamadores**

O serviço `cadastro_futuro` ficou sem relógio, com `hoje` obrigatório e recusa explícita de `None`; isso corrige aquele arquivo, mas não o caminho inteiro. (`app/ponto/services/cadastro_futuro.py:9`, `app/ponto/services/cadastro_futuro.py:24`, `app/ponto/services/cadastro_futuro.py:49`, `app/ponto/services/cadastro_futuro.py:55`)

Censo do caminho de cadastro/materialização, incluindo geração, reversão e caudas imediatas; cada posição abaixo identifica a leitura na cópia:

| Arquivo:função | Leitura(s) | Classe |
|---|---|---|
| `app/core/signals.py::_feriado_regenerar_futuras` | `localdate`, `:36` | BORDA, receiver `:28` |
| `app/core/signals.py::_feriado_excluido_regenerar_futuras` | `localdate`, `:43` | BORDA, receiver `:39` |
| `app/escala/signals.py::_ec_vigencia_regenera_celulas` | `localdate`, `:67`, `:68` | BORDA, receiver `:34`; duas leituras |
| `app/escala/signals.py::_ec_ancora_regenera_celulas` | `localdate`, `:119` | BORDA, receiver `:86` |
| `app/escala/admin.py::TipoEscalaAdmin.save_model` | `localdate`, `:15` | BORDA admin |
| `app/colaboradores/views.py::vincular_folga` | `date.today`, `:242`, `:243` | BORDA view |
| `app/colaboradores/views.py::painel_vinculo` | `date.today`, `:280` | BORDA view |
| `app/colaboradores/views.py::vincular_posto` | `localdate`, `:331` | BORDA view |
| `app/escala/views.py::escala_remove_colab` | `date.today`, `:269` | BORDA view |
| `app/escala/views.py::aplicar_folga_calendario` | `date.today`, `:393` | BORDA view |
| `app/escala/views_plano_folgas.py::plano_folgas` | `date.today`, `:31` | BORDA view |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py::Command._rodar / _aplicar` | `localdate`, `:67`, `:227` | BORDA command |
| `app/ponto/management/commands/gerar_celulas.py::Command.handle` | `date.today`, `:339` | BORDA command |
| `app/colaboradores/services/vinculo.py::julgar_vinculo` | `date.today`, `:71` | INTERIOR |
| `app/escala/models.py::EscalaColaborador.recusa_ancora_futura` | `localdate`, `:965` | INTERIOR |
| `app/colaboradores/services/vinculo.py::_copiar_folga_padrao` | `date.today`, `:163` | INTERIOR |
| `app/colaboradores/services/vinculo.py::executar_vinculo` | `localdate`, `:314` | INTERIOR |
| `app/colaboradores/services/vinculo.py::trocar_calendario_de_folga` | `localdate`, `:460` | INTERIOR |
| `app/colaboradores/services/vinculo.py::fechar_vigencia` | `localdate`, `:646` | INTERIOR |
| `app/colaboradores/services/vinculo.py::reabrir_vigencia_impossivel` | `date.today`, `:728`; `localdate`, `:740` | INTERIOR |
| `app/colaboradores/services/vinculo.py::marcar_trabalha_em_feriado` | `localdate`, `:885` | INTERIOR |
| `app/colaboradores/services/vinculo.py::reverter_para_snapshot` | `localdate`, `:1021` | INTERIOR |
| `app/colaboradores/services/estrutura.py::_evento_do_municipio` | `localdate`, `:62` | INTERIOR |
| `app/escala/services/cadastro_tipo.py::salvar_tipo` | `localdate`, `:310` | INTERIOR |
| `app/escala/folgas.py::salvar_calendario_de_folga` | `localdate`, `:599` | INTERIOR |
| `app/escala/folgas.py::relavrar_celulas_da_foto` | `localdate`, `:280` | INTERIOR |
| `app/escala/folgas.py::aplicar_folgas_em_lote` | `_tz.localdate`, `:401` | INTERIOR |
| `app/escala/folgas.py::reparentar_folga` | `_tz.localdate`, `:512` | INTERIOR |
| `app/escala/services/escala_auto_executor.py::inicio_do_efeito / aplicar_no_vinculo / desfazer` | `localdate`, `:29`, `:98`, `:127` | INTERIOR |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | `date.today`, `:188`; `now`, `:258`, `:303`; `localdate`, `:329` | INTERIOR |
| `app/ponto/management/commands/gerar_celulas.py::_marcar_cobrancas_orfas` | `now`, `:264` | INTERIOR, apesar do arquivo de command |
| `app/ponto/portas/celula.py::_o_chao_do_dia_mudou` | `localdate`, `:459` | INTERIOR |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `_tz.localdate`, `:56` | INTERIOR |
| `app/ponto/portas/celula.py::reverter_regeneracao_dia` | `now`, `:826`; `localdate`, `:831` | INTERIOR |
| `app/ponto/portas/celula.py::gerar_celulas_periodo` | `date.today`, `:860` | INTERIOR |

Na cauda de julgamento/apuração também permanecem `cartorio.julgar_celula`: `localdate` em `:2016`, `now` em `:2037` e `:2040`; `lavrar_veredito`: `now` em `:1086`; e `gravar_furos`: `now` em `:1252`, todos INTERIOR. Os `now` de regeneração/furo são carimbos; os `localdate` citados decidem alcance ou elegibilidade temporal. (`app/ponto/services/cartorio.py:2016`, `app/ponto/services/cartorio.py:2037`, `app/ponto/services/cartorio.py:2040`; `app/ponto/portas/celula.py:1086`, `app/ponto/portas/celula.py:1252`, `app/ponto/portas/celula.py:333`, `app/ponto/portas/celula.py:464`)

**Restaram defaults/fallbacks no caminho:**

| Função | Prova |
|---|---|
| `regenerar_celulas_vinculo` | `hoje=None`: `app/ponto/portas/celula.py:113`; `hoje or date.today()`: `:188` |
| `regenerar_celulas_dia` | `hoje=None`: `app/ponto/portas/celula.py:736` |
| `reverter_regeneracao_dia` | `hoje=None`: `app/ponto/portas/celula.py:773`; `hoje or`: `:831` |
| `gerar_celulas_periodo` | `hoje=None`: `app/ponto/portas/celula.py:855`; `hoje or`: `:860` |
| `aplicar_folgas_em_lote` | `hoje=None`: `app/escala/folgas.py:385`; `hoje or`: `:401` |
| `reparentar_folga` / wrapper do colaborador | `app/escala/folgas.py:471`, `app/escala/folgas.py:512`, `app/escala/folgas.py:529` |
| `recusa_ancora_futura` | `app/escala/models.py:938`, `app/escala/models.py:965` |
| executor automático: início/aplicar/desfazer | `app/escala/services/escala_auto_executor.py:24`, `:29`, `:87`, `:98`, `:118`, `:127` |
| `apurar_colab`, na cauda pós-vínculo | `app/escala/services/furos_diarios.py:25`, `app/escala/services/furos_diarios.py:30` |

**Não identifiquei chamada da cópia que passe a quebrar por omitir `hoje` nas sete assinaturas alteradas.** Os receivers passam explicitamente; os demais chamadores encontrados também. Não apareceu importação com apelido nem `getattr` literal dessas sete funções; resolução dinâmica externa permanece [nao verificado]. (`app/core/signals.py:35`, `app/core/signals.py:43`; `app/escala/signals.py:81`, `app/escala/signals.py:132`; `app/escala/admin.py:15`; `app/escala/folgas.py:298`, `:601`, `:640`; `app/colaboradores/services/estrutura.py:62`; `app/colaboradores/services/vinculo.py:333`, `:460`, `:650`, `:738`, `:898`, `:949`, `:1023`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:243`; `app/escala/services/cadastro_tipo.py:323`, `:328`, `:366`, `:376`, `:388`; `app/ponto/services/cadastro_futuro.py:77`)

Há chamadas sem `hoje` a funções **que continuam opcionais**, portanto ainda não quebram: `julgar_vinculo → recusa_ancora_futura` e o receiver que chama `reparentar_folgas_do_colaborador`. São pendências da classe, não novos `TypeError` deste delta. (`app/colaboradores/services/vinculo.py:133`; `app/escala/signals.py:204`)

3. **As sete “portas do ato” são INTERIOR; meia-noite continua relevante**

| Porta | Chamador que demonstra composição interna |
|---|---|
| `fechar_vigencia` | `executar_vinculo`: `app/colaboradores/services/vinculo.py:208`, `:235` |
| `reabrir_vigencia_impossivel` | command: `app/escala/management/commands/corrigir_vinculo_vigencia.py:84` |
| `reverter_para_snapshot` | serviço: `app/chamados/services/acoes_chamado.py:149` |
| `trocar_calendario_de_folga` | view: `app/colaboradores/views.py:239` |
| `salvar_calendario_de_folga` | view: `app/escala/views.py:356` |
| `_evento_do_municipio` | outras portas: `app/colaboradores/services/estrutura.py:72`, `:97` |
| `salvar_tipo` | inclusive outro serviço: `app/chamados/services/limbo_folgas.py:92` |

**Sim, podem discordar.** O executor chama `fechar_vigencia` duas vezes antes de capturar seu próprio `_hoje`; cada fechamento lê seu relógio. O comentário “UM relógio no ato” não descreve esse caminho. (`app/colaboradores/services/vinculo.py:208`, `:235`, `:314`, `:646`)

Consequência possível: fechamento antes da meia-noite de D trata D+1 como futura e limpa seu julgamento; executor depois da meia-noite captura D+1, inclui o mesmo dia na regeneração até hoje e pode executar o pós-vínculo. Outra passagem permite selecionar uma célula como futura pelo `_hoje` recebido e depois elegê-la para julgamento pela nova leitura da cauda. São decisões temporais diferentes no mesmo Salvar; ocorrência ou dano em produção: [nao verificado]. (`app/ponto/services/cadastro_futuro.py:27`; `app/ponto/portas/celula.py:246`, `:262`, `:459`, `:464`; `app/colaboradores/services/vinculo.py:319`, `:340`)

4. **QUEM ESCREVE / QUEM LÊ**

**`hoje` é dia civil de referência**, usado para competência e futuro estrito. O dia da célula/turno permanece separado em `dia` ou `cel.data`; não identifiquei dois significados nesse parâmetro alterado. `date.today()` usa a data do processo; equivalência efetiva com a data local do Django em produção: [nao verificado]. (`app/ponto/services/cadastro_futuro.py:26`, `:27`, `:43`, `:45`; `app/ponto/portas/celula.py:1137`; `app/ponto/management/commands/gerar_celulas.py:339`; `app/config/settings/base.py:93`)

| Parâmetro/estado | Escritor ou produtor | Leitor/efeito |
|---|---|---|
| `hoje` | bordas e serviços enumerados no item 2 | janela, filtro `> hoje`, classificação da limpeza: `app/ponto/services/cadastro_futuro.py:26`, `:27`, `:60`; `app/escala/folgas.py:623`; `app/ponto/portas/celula.py:1137` |
| `desde`, `datas`, `substitui_dono` | chamador do evento | seleção e proteção do dono: `app/ponto/services/cadastro_futuro.py:27`, `:33`, `:37` |
| `autor/ator`, `motivo` | chamador; request/processo complementa autoria | descrição, usuário e antes/depois da trilha: `app/ponto/portas/celula.py:1147`, `:1159`; `app/core/observ.py:108`, `:118`, `:134` |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`, `status_dia`, `status_rotulo` | `lavrar_veredito`; D1 limpa pela mesma porta: `app/ponto/portas/celula.py:1082`, `:1103`, `:1156` | grade, precedência admin, stale e status: `app/escala/services/leitor_celula.py:379`, `:424`; `app/ponto/portas/celula.py:970`; `app/chamados/veredito_lavrado.py:191`; `app/ponto/services/cartorio.py:1084`; `app/ponto/status_dia.py:287` |
| `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | `carimbar` e operação de insumos; D1 limpa: `app/ponto/portas/celula.py:1185`, `:1223`, `:1156` | cache/idempotência, stale e vigia B5: `app/ponto/services/cartorio.py:1260`, `:1409`; `app/chamados/veredito_lavrado.py:191`; `app/ponto/portas/celula.py:1181`; `app/ponto/management/commands/vigia_b5_ausencia.py:71` |
| ações `julgamento_da_futura_conservado/desfeito` | D1 solicita; `evento` grava `LogAuditoria`: `app/ponto/portas/celula.py:1147`, `:1159`; `app/core/observ.py:134` | leitores nominais encontrados nos testes; leitura genérica na auditoria: `app/ponto/tests/test_cadastro_futuro_c5.py:157`, `:204`; `app/ponto/tests/test_cadastro_futuro_c5d.py:78`, `:87`; `app/core/views.py:705` |

O vigia futuro **lê os campos da célula**, não a trilha nominal: `julgada_em`, `ata`, `veredito` e a distinção administrativa por `veredito_via`. (`app/ponto/services/vigia_cadastro_futuro.py:21`, `:45`, `:47`)

A autoridade permanece a indicada em `JUIZES.md`: `vinculo_do_dia` e `vinculo_cobre_a_data`; o suplemento usa o carregador que chama essa juíza, e a janela vem de `janela_atual`. (`app/docs/JUIZES.md:120`; `app/ponto/services/cadastro_futuro.py:38`; `app/escala/alimentacao.py:358`; `app/ponto/management/commands/gerar_celulas.py:299`)

5. **Classes que já reprovaram obra**

| Classe | Resultado nesta volta |
|---|---|
| Segundo juiz | **Ausente no delta examinado:** permanece chamada à autoridade existente. `app/ponto/services/cadastro_futuro.py:38`; `app/escala/alimentacao.py:358` |
| Segundo escritor fora da porta | **Ausente no delta examinado:** limpeza é operação declarada da porta; auditoria continua em `evento`. `app/ponto/portas/celula.py:80`, `:1156`; `app/core/observ.py:134` |
| Relógio fora da borda | **Presente:** exemplos novos `app/colaboradores/services/vinculo.py:646`, `:1021`; exemplo remanescente `app/ponto/portas/celula.py:188` |
| Derivação paralela de fonte única | **Não introduzida pelo delta:** dono, janela e calendário continuam nas derivações existentes. `app/ponto/services/cadastro_futuro.py:38`; `app/ponto/management/commands/gerar_celulas.py:299`; `app/escala/folgas.py:629` |
| Selo/RED sem mordida | **Problema presente:** censo do relógio cobre somente sete funções e compara nomes finais; não resolve apelidos/`getattr`. `app/ponto/tests/test_cadastro_futuro_c5d.py:25`, `:185`, `:195` |
| Ordem de trava | **Nenhuma inversão nova demonstrada:** snapshot mantém vínculo→célula; fechamento escreve vínculo antes do suplemento. Ausência global de deadlock: [nao verificado]. `app/colaboradores/services/vinculo.py:1008`, `:1013`, `:629`, `:650`; `app/ponto/services/cadastro_futuro.py:31` |
| Pouso antes do pré-requisito | **[nao verificado]**: cópia/delta não demonstram a ordem efetiva dos pousos. | [nao verificado]
| Campo com dois significados | **Não identificado em `hoje` no delta:** referência civil separada do dia-alvo. `app/ponto/services/cadastro_futuro.py:27`, `:43`; `app/ponto/portas/celula.py:1137` |

**Há regressão concreta no instrumento anterior:** `desfazer_apaga_admin` perdeu a âncora. Ele procura o corpo antigo, enquanto o novo possui `atomic` e `_t = evento`; falha na preparação com “mutante sem alvo unico”, antes de testar a preservação administrativa. Esse vermelho não prova mais aquela propriedade. (`app/ponto/tests/test_cadastro_futuro_c5.py:37`, `:78`, `:103`, `:199`; `app/ponto/portas/celula.py:1145`, `:1147`)

6. **LEIS dos sítios**

- **L-007:** o ponto específico da triagem foi atendido: recusa de trilha ausente e rollback conjunto. (`app/docs/LEIS.md:65`; `app/ponto/portas/celula.py:1145`, `:1151`, `:1163`)
- **Clock Injection permanece descumprido no caminho:** a exigência é leitura uma vez na borda; serviços compostos continuam lendo seu próprio relógio. (`app/docs/PLANO-PADROES.md:91`, `:92`, `:94`; `app/colaboradores/services/vinculo.py:646`, `:314`)
- **L-112 preservada pelo delta:** o executor continua consumindo início e âncora declarados, gravando a âncora explícita e fechando a inativa sobreposta pela porta. O delta não converte a sugestão por batida em limite da declaração. (`app/docs/LEIS.md:154`; `app/colaboradores/services/vinculo.py:179`, `:235`, `:274`, `:285`)
- **L-021 e L-020:** o delta mantém o filtro de vigência, a escrita pela porta e a proteção de célula editada; não demonstra ampliação nova do passado. (`app/docs/LEIS.md:71`, `:72`; `app/colaboradores/services/vinculo.py:629`; `app/ponto/portas/celula.py:292`; `app/ponto/services/cadastro_futuro.py:27`, `:32`)

7. **DESCARTADO 3**

**Continua DESCARTADO.** O cron conserva o alcance de hoje consultando a juíza, passa `hoje` explicitamente e não usa `ativa=True` como resposta sobre o dono daquele dia. Vínculo inativo com fim que cobre o dia continua admitido pela autoridade; o sem vigência declarada continua recusado pela porta. O delta não altera esses critérios. (`app/ponto/management/commands/gerar_celulas.py:303`, `:321`, `:324`; `app/escala/alimentacao.py:272`, `:282`; `app/ponto/portas/celula.py:148`)

## 12/11 18:16 · sessao 452d55645847 · - · - · origem=Ronald (tmux)

Mapa de projeções, somente leitura:

- `calendario`: grava sets por `(dia,mês)` e compara `strftime('%H:%M')`; trocar por data completa e timestamps aware. Para legado, `b.orfa=None` preserva classificação desconhecida; o render usa timestamp da batida. (`colaboradores/services/calendario.py:380`, `colaboradores/services/calendario.py:622`, `templates/colaboradores/partials/_calendario_grade.html:121`)
- PDF: `_entrada_espuria`, associação de batidas e supressão de alertas devem comparar `periodo.entrada`/`b.timestamp` diretamente com instantes. As agregações `or []` e `or set()` apagam desconhecimento. (`relatorios/pdf_espelho.py:174`, `relatorios/pdf_espelho.py:477`, `relatorios/pdf_espelho.py:552`, `relatorios/pdf_espelho.py:607`, `relatorios/pdf_espelho.py:960`)
- **PERGUNTA DE DESENHO:** filtro de anomalias do PDF recebe mensagens textuais e as identifica por substring HH:MM. Não há identidade temporal disponível nesse caminho; preservar comportamento por instante exige mudança do contrato de anomalias. (`relatorios/pdf_espelho.py:716`, `relatorios/pdf_espelho.py:719`)
- `orfa_classe`: parser por fatia e diferença de minutos devem virar diferenças entre datetimes; `classificar_orfas` precisa propagar desconhecido em vez de `{}`. (`escala/orfa_classe.py:31`, `escala/orfa_classe.py:44`, `escala/orfa_classe.py:58`)
- `horas_reais_do_dia`: já possui saída `[int|None]`; pode calcular deslocamentos dos instantes reais desde a origem do dia e publicar `None` para legado. O consumidor já responde `'nao_sei'`. (`ponto/turnos.py:2616`, `ponto/turnos.py:2643`, `ponto/turnos.py:2646`, `ponto/turnos.py:2667`)
- `supra_juiz`: `join(_orfas)` quebra com datetime; formatar no texto final. O fallback por batidas deve ficar restrito à chave ausente, sem executar quando `orfas=None`. (`ponto/supra_juiz.py:514`, `ponto/supra_juiz.py:520`, `ponto/supra_juiz.py:521`)
- Espelho: transportar `None`; templates devem formatar `o|date:"H:i"` e renderizar órfãs independentemente de `realizado_grade`. (`ponto/services/espelho.py:689`, `ponto/services/espelho.py:844`, `templates/ponto/espelho.html:151`, `templates/ponto/espelho.html:240`)
- Contadores: `furos_diarios`, `status_dia`, bordas e causas devem usar `n_orfas` conhecido quando identidades forem desconhecidas. (`escala/services/furos_diarios.py:22`, `ponto/status_dia.py:188`, `ponto/services/bordas_realizado.py:49`, `folha/causas.py:43`)
- **PERGUNTA DE DESENHO:** sem lista e sem contagem, `_furos_do_dia` retorna tupla numérica e `_teve_batida_na_ata` retorna bool consumido por `int`; não possuem saída atual para desconhecido. (`escala/services/furos_diarios.py:12`, `escala/services/furos_diarios.py:23`, `ponto/services/bordas_realizado.py:21`, `ponto/services/bordas_realizado.py:80`)
- `classificar_causa` já tem resultado `indeterminado`; `dia_perfeito` pode negar prova de perfeição quando órfãs forem desconhecidas. (`folha/causas.py:34`, `folha/causas.py:80`, `ponto/services/simular_dia.py:12`)
- Render do detector de cluster precisa formatar datetime antes de `join`; detector de par relâmpago também apaga desconhecimento na contagem. (`ponto/management/commands/detectar_cluster_espurio.py:87`, `ponto/management/commands/detectar_par_relampago.py:140`)

## 12/11 18:16 · sessao 4960b88c2174 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhum arquivo alterado.

| Leitor | Caminho existente para “não sei” / adaptação |
|---|---|
| `_celula_furada` | Já retorna `None` para célula/ata ausente. Presença pode continuar provada por `n_orfas>0`; identidade desconhecida sem contagem positiva precisa retornar `None`, evitando o `False` final. `chamados/juizes.py:488`, `:505`, `:510`, `:518`. |
| `_plantio_preenche_furo_na_data` | Contrato explícito: leitura ausente/ambígua → `False`, segunda opinião não absolve plantio. Migrar sem HH:MM exige adaptar DP para coordenadas temporais: slots, luzes e plantio continuam strings hoje. `chamados/services/materializacao.py:143`, `:177`, `:182`; `ponto/propositor.py:112`, `:122`. |
| `periodos_do_dia` | Pode transportar `orfas=None` no resultado; já dispõe de `nao_sei`, `perguntas` e `desalinhado`. Atenção: `nao_sei` é sobrescrito ao final. `ponto/juiz_batida.py:152`, `:161`, `:164`, `:252`. |
| `_leitura_da_celula` | Migração direta: `None` do leitor único → `None`; rajada por diferença entre datetimes. Consumidor já conserva `None` no catálogo. Descrição formata HH:MM; contexto persiste ISO. `ponto/services/cluster_espurio.py:15`, `:29`, `:34`, `:74`, `:210`, `:211`. |
| `assinaturas_do_colab` — A5 | Não dispõe de estado de medição: saída é `{codigo: [ocorrencias]}`, posteriormente contada por `len`. Categoria recorrente pode derivar de hora/minuto do instante localizado; legado não pode simplesmente desaparecer. **Pergunta de desenho necessária para publicação de A5 desconhecida.** `ponto/services/esmeril_espelho.py:79`, `:125`, `:139`, `:198`. |
| `medir_sobra_com_tipo_trocado` | Saída somente `(achados, universo)`; não há contador de não medidos. Peneira atual é circular por HH:MM e confirmação ainda identifica órfã por HH:MM. **Pergunta de desenho necessária para legado não medido.** `ponto/management/commands/tripwire_celulas.py:135`, `:165`, `:183`, `:195`, `:237`, `:264`, `:297`. |
| `vigia_pareador_celula.censo` | Já publica `cego`, porém global por universo vazio; não há representação explícita por célula desconhecida. Comparação migra para `b.timestamp in orfas_civil`. **Pergunta de desenho: legado marca todo censo cego ou recebe contador próprio?** `ponto/services/vigia_pareador_celula.py:43`, `:60`, `:74`, `:109`, `:113`. |
| `orfas_e_isencao_da_ata` | `rg` nos Python permitidos encontrou somente definição e teste; nenhum chamador produtivo. L-111 permite apagar função e teste específico. Chamadas dinâmicas são [não verificado]. `relatorios/cartao_pela_celula.py:277`; `relatorios/tests/test_cartao_pela_celula.py:159`, `:166`. |

Propositor possui saída explícita `MOTIVO_NAO_AVALIAVEL`, utilizável para órfãs desconhecidas; a guarda precisa ocorrer antes de `or []`. A DP atual inteira trabalha com minutos extraídos de strings, portanto converter somente a órfã para HH:MM perderia novamente sua identidade. `ponto/propositor.py:18`, `:39`, `:44`, `:55`, `:112`, `:122`.

## 12/11 18:16 · sessao c305d899aace · - · - · origem=Ronald (tmux)

P2 continua PROCEDENTE no caminho completo: o serviço `cadastro_futuro` deixou de ler relógio e exige `hoje`, mas o delta deslocou leituras para serviços internos, enquanto a regra exige uma leitura na borda e passagem até o julgamento. (`app/ponto/services/cadastro_futuro.py:17`; `app/docs/PLANO-PADROES.md:92`)

Censo do caminho direto de cadastro/materialização, incluindo as caudas imediatas:

| Arquivo::função | Leitura | Classe |
|---|---|---|
| `app/core/signals.py::_feriado_regenerar_futuras` | `localdate`, :36 | BORDA receiver (:28) |
| `app/core/signals.py::_feriado_excluido_regenerar_futuras` | `localdate`, :43 | BORDA receiver (:39) |
| `app/escala/signals.py::_ec_vigencia_regenera_celulas` | `localdate`, :67 e :68 | BORDA receiver (:34); já são duas leituras |
| `app/escala/signals.py::_ec_ancora_regenera_celulas` | `localdate`, :119 | BORDA receiver (:86) |
| `app/escala/admin.py::TipoEscalaAdmin.save_model` | `localdate`, :15 | BORDA admin (:11) |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py::Command._rodar` | `localdate`, :67 e :227 | BORDA command, chamado por `handle` (:48-51) |
| `app/ponto/management/commands/gerar_celulas.py::Command.handle` | `date.today`, :339 | BORDA command (:337) |
| `app/colaboradores/services/vinculo.py::julgar_vinculo` | `date.today`, :71 | INTERIOR (:52) |
| `app/escala/models.py::EscalaColaborador.recusa_ancora_futura` | `hoje or timezone.localdate`, :965 | INTERIOR; chamado sem `hoje` pelo juiz (:133) |
| `app/colaboradores/services/vinculo.py::_copiar_folga_padrao` | `date.today`, :163 | INTERIOR; executor chama :278/:292; foto chega à re-lavra em `app/escala/folgas.py:215` |
| `app/colaboradores/services/vinculo.py::executar_vinculo` | `localdate`, :314 | INTERIOR executor (:170) |
| `app/colaboradores/services/vinculo.py::trocar_calendario_de_folga` | `localdate`, :460 | INTERIOR; view chama `app/colaboradores/views.py:239` |
| `app/colaboradores/services/vinculo.py::fechar_vigencia` | `localdate`, :646 | INTERIOR; executor chama :208/:235 |
| `app/colaboradores/services/vinculo.py::reabrir_vigencia_impossivel` | `date.today`, :728; `localdate`, :740 | INTERIOR; command chama `app/escala/management/commands/corrigir_vinculo_vigencia.py:84` |
| `app/colaboradores/services/vinculo.py::marcar_trabalha_em_feriado` | `localdate`, :885 | INTERIOR (:857) |
| `app/colaboradores/services/vinculo.py::reverter_para_snapshot` | `localdate`, :1021 | INTERIOR; serviço chama `app/chamados/services/acoes_chamado.py:149` |
| `app/colaboradores/services/estrutura.py::_evento_do_municipio` | `localdate`, :62 | INTERIOR; outras portas chamam :72/:97 |
| `app/escala/services/cadastro_tipo.py::salvar_tipo` | `localdate`, :310 | INTERIOR; chamado inclusive pelo serviço `app/chamados/services/limbo_folgas.py:92` |
| `app/escala/folgas.py::salvar_calendario_de_folga` | `localdate`, :599 | INTERIOR; view chama `app/escala/views.py:356` |
| `app/escala/folgas.py::relavrar_celulas_da_foto` | `localdate`, :280 | INTERIOR; portas chamam :337/:380/:418/:523 |
| `app/escala/folgas.py::aplicar_folgas_em_lote` | `hoje or _tz.localdate`, :401 | INTERIOR; view chama `app/escala/views_plano_folgas.py:185` |
| `app/escala/folgas.py::reparentar_folga` | `hoje or _tz.localdate`, :512 | INTERIOR; command chama `app/escala/management/commands/reparentar_folgas.py:34` |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | fallback `date.today`, :188; `timezone.now`, :258/:303; `localdate`, :329 | INTERIOR; serviço futuro chama `app/ponto/services/cadastro_futuro.py:44` |
| `app/ponto/management/commands/gerar_celulas.py::_marcar_cobrancas_orfas` | `timezone.now`, :264 | INTERIOR; porta chama `app/ponto/portas/celula.py:322` |
| `app/ponto/portas/celula.py::_o_chao_do_dia_mudou` | `localdate`, :459 | INTERIOR; porta chama :387 |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `_tz.localdate`, :56 | INTERIOR; executor chama `app/colaboradores/services/vinculo.py:342` |
| `app/ponto/portas/celula.py::gerar_celulas_periodo` | fallback `date.today`, :860 | INTERIOR; command chama `app/ponto/management/commands/gerar_celulas.py:349` |

As leituras `now` de `regenerada_em` e da trilha de cobrança são carimbos de escrita, enquanto :188/:329/:459 decidem classificação temporal; todos continuam dentro da porta/serviço. (`app/ponto/portas/celula.py:258`; `app/ponto/portas/celula.py:303`; `app/ponto/management/commands/gerar_celulas.py:264`; `app/ponto/portas/celula.py:333`; `app/ponto/portas/celula.py:464`)

Também resta o caminho de reversão, fora do suplemento principal: `reverter_regeneracao_dia` tem `hoje=None`, `timezone.now` :826 e `hoje or timezone.localdate` :831; chama a mesma limpeza D1 :834. (`app/ponto/portas/celula.py:773`)

Defaults restantes comprovados:

- `regenerar_celulas_vinculo(hoje=None)` e fallback :188. (`app/ponto/portas/celula.py:113`)
- `regenerar_celulas_dia(hoje=None)`, repassa à porta :768. (`app/ponto/portas/celula.py:736`)
- `gerar_celulas_periodo(hoje=None)` e fallback :860. (`app/ponto/portas/celula.py:855`)
- `aplicar_folgas_em_lote(hoje=None)` e fallback :401. (`app/escala/folgas.py:385`)
- `reparentar_folga(hoje=None)` e fallback :512; wrapper `reparentar_folgas_do_colaborador(hoje=None)` :529. O signal chama wrapper sem `hoje`. (`app/escala/signals.py:204`)
- `recusa_ancora_futura(hoje=None)` e fallback :965, efetivamente chamado sem `hoje`. (`app/escala/models.py:938`; `app/colaboradores/services/vinculo.py:133`)

Nas sete funções que o delta tornou obrigatórias, o censo AST de produção encontrou 23 chamadas, todas com `hoje=`; nenhuma ocorrência de alias de import nem literal exato para `getattr` foi encontrada. Os receivers efetivos também passam o parâmetro. (`app/core/signals.py:35`; `app/core/signals.py:43`; `app/escala/signals.py:81`; `app/escala/signals.py:132`; demais chamadas em `app/escala/folgas.py:298/:601/:640`, `app/escala/admin.py:15`, `app/colaboradores/services/estrutura.py:62`, `app/colaboradores/services/vinculo.py:333/:460/:650/:738/:898/:949/:1023`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:243`, `app/ponto/services/cadastro_futuro.py:77`, `app/escala/services/cadastro_tipo.py:323/:328/:366/:376/:388`). Chamada dinâmica construída externamente ou ocorrência real em produção: [não verificado].

O selo cobre apenas as sete funções de `SEM_DEFAULT`; deixa fora as portas internas que ganharam relógio e a própria porta da célula. Seu detector de chamadas compara o nome final, sem resolver aliases/getattr; o anti-vacuidade usa só chamada direta. (`app/ponto/tests/test_cadastro_futuro_c5d.py:25`; `app/ponto/tests/test_cadastro_futuro_c5d.py:185`; `app/ponto/tests/test_cadastro_futuro_c5d.py:195`)

Sobre meia-noite: `executar_vinculo` chama `fechar_vigencia` duas vezes antes de capturar `_hoje`; cada chamada captura seu próprio dia, e signals/fotos também podem ler relógio antes disso. Portanto o comentário “UM relógio no ato” não corresponde ao caminho. (`app/colaboradores/services/vinculo.py:208`; `app/colaboradores/services/vinculo.py:235`; `app/colaboradores/services/vinculo.py:646`; `app/colaboradores/services/vinculo.py:314`)

Consequência concreta de execução possível: encerramento às 23:59 de D classifica D+1 como futura e apaga seu julgamento; executor após 00:00 captura D+1, inclui esse mesmo dia no ramo até hoje e aciona pós-vínculo para um início em D+1. Além disso a cauda :459 pode julgar uma célula selecionada como futura pelo `_hoje` anterior. Isso demonstra decisões temporais diferentes no mesmo Salvar; ocorrência ou dano em produção não está no dado. (`app/ponto/services/cadastro_futuro.py:27`; `app/ponto/portas/celula.py:246`; `app/ponto/portas/celula.py:262`; `app/colaboradores/services/vinculo.py:319`; `app/colaboradores/services/vinculo.py:340`; `app/ponto/portas/celula.py:459`; `app/ponto/portas/celula.py:464`)

## 12/11 18:16 · sessao 55c1b880a856 · - · - · origem=Ronald (tmux)

P1 está curado no corpo da porta: ambos os ramos exigem retorno diferente de `None`, levantam `RuntimeError` dentro do `atomic` e propagam a falha. `evento` é best-effort e retorna `None` depois de engolir a falha. (app/ponto/portas/celula.py:1145; app/ponto/portas/celula.py:1151; app/ponto/portas/celula.py:1163; app/core/observ.py:105; app/core/observ.py:139)

As três chamadas diretas estão protegidas pelo `atomic` que também escreveu o DNA: as duas da regeneração ficam sob o bloco iniciado em `:220`; a reversão fica sob `:818`. Nenhuma dessas chamadas está dentro de `try/except` local. Portanto a falha desfaz no banco DNA e limpeza conjuntamente; o objeto Python já alterado não é explicitamente restaurado. (app/ponto/portas/celula.py:220; app/ponto/portas/celula.py:260; app/ponto/portas/celula.py:262; app/ponto/portas/celula.py:305; app/ponto/portas/celula.py:309; app/ponto/portas/celula.py:818; app/ponto/portas/celula.py:828; app/ponto/portas/celula.py:834; app/ponto/portas/celula.py:1156)

Há um ancestral que captura e segue: `censo_fase_12x36._curar` captura `Exception`, imprime `RECUSADO` e continua o laço. A captura acontece depois que `executar_vinculo`, decorado com `@transaction.atomic`, já saiu com erro e desfez seu ato. (app/ponto/management/commands/censo_fase_12x36.py:400; app/ponto/management/commands/censo_fase_12x36.py:421; app/ponto/management/commands/censo_fase_12x36.py:427; app/colaboradores/services/vinculo.py:169)

A view também captura `Exception`, mas encaminha para `so_recusa_de_negocio`, que relança esse `RuntimeError`; o chamador do snapshot captura somente `ValueError`. Os catches históricos dos signals e de `salvar_tipo` cobrem a parte até hoje; o suplemento futuro está fora deles. (app/colaboradores/views.py:431; app/colaboradores/views.py:438; app/core/erros.py:42; app/core/erros.py:75; app/chamados/services/acoes_chamado.py:148; app/escala/signals.py:73; app/escala/signals.py:80; app/escala/signals.py:125; app/escala/signals.py:131; app/escala/services/cadastro_tipo.py:323; app/escala/services/cadastro_tipo.py:328)

O teste de banco tem mordida estrutural: com `evento=None`, exige `RuntimeError`, recarrega a célula e confere ata, julgamento e veredito anteriores. Ignorar novamente o retorno faria o `assertRaises` falhar; executar esse teste no banco permanece [não verificado]. Há também teste do ramo conservado. (app/ponto/tests/test_cadastro_futuro_c5d_banco.py:38; app/ponto/tests/test_cadastro_futuro_c5d_banco.py:43; app/ponto/tests/test_cadastro_futuro_c5d_banco.py:56)

Executei os 13 testes c5d somente em memória: `OK`, rc=0. Executei ainda mutante somente em memória, trocando os dois guards `_t is None` por `False`: os três testes de recusa falharam com `RuntimeError not raised`, confirmando a mordida desses testes. (app/ponto/tests/test_cadastro_futuro_c5d.py:73; app/ponto/tests/test_cadastro_futuro_c5d.py:82; app/ponto/tests/test_cadastro_futuro_c5d.py:91)

Novo problema de instrumento: o mutante antigo `desfazer_apaga_admin` perdeu sua âncora porque c5d introduziu `atomic` e `_t = evento`. Confirmei executando o teste: ele falha em `setUp` com `mutante sem alvo unico`, antes de exercer o comportamento. Esse RED não comprova mais a preservação do julgamento administrativo. (app/ponto/tests/test_cadastro_futuro_c5.py:38; app/ponto/tests/test_cadastro_futuro_c5.py:78; app/ponto/tests/test_cadastro_futuro_c5.py:103; app/ponto/tests/test_cadastro_futuro_c5.py:199; app/ponto/portas/celula.py:1145; app/ponto/portas/celula.py:1147)

## 12/11 18:15 · sessao 6b14b64296ac · - · - · origem=Ronald (tmux)

- **Quem escreve os 13 campos:** `lavrar_veredito` escreve `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`, `status_dia` e `status_rotulo`; `carimbar` escreve `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida` e `insumos_em`; D1 limpa esses mesmos campos pela operação `lavrar_veredito` da mesma porta, sem escritor externo novo. `app/ponto/portas/celula.py:74`, `:1082`, `:1103`, `:1156`, `:1185`.

- **Leitores concretos:** `ata` e `veredito` alimentam o leitor da grade (`app/escala/services/leitor_celula.py:379`, `:424`); `veredito_via` protege decisão admin (`app/ponto/portas/celula.py:970`, `:1052`); `protestos` entra na contagem do cartório (`app/ponto/services/cartorio.py:1084`); `veredito_em` e `julgada_em` compõem o marco de invalidação das perguntas (`app/chamados/veredito_lavrado.py:191`); `status_dia/status_rotulo` alimentam o leitor do status (`app/ponto/status_dia.py:287`); `julgada_em/impressao` permitem pular julgamento já atualizado (`app/ponto/services/cartorio.py:1260`, `:1409`); `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` são comparados pelo vigia B5 e pela idempotência da própria porta (`app/ponto/management/commands/vigia_b5_ausencia.py:71`, `:75`, `:80`; `app/ponto/portas/celula.py:1181`, `:1220`).

- **Trilha:** a função D1 produz `acao`, `modelo='CelulaDia'`, `objeto_id=cel.pk`, descrição com motivo, antes/depois e ator; `core.observ.evento` escreve `LogAuditoria`, associa usuário/IP da request ou ator de processo. `app/ponto/portas/celula.py:1147`, `:1159`; `app/core/observ.py:108`, `:118`, `:134`. Leitura nominal de `julgamento_da_futura_*` encontrada somente nos testes; a tela faz leitura genérica dos logs. `app/ponto/tests/test_cadastro_futuro_c5.py:157`, `:204`; `app/ponto/tests/test_cadastro_futuro_c5d.py:78`, `:87`; `app/core/views.py:705`, `:709`. O vigia futuro **não lê essa trilha**: lê `julgada_em/ata/veredito` e `veredito_via`. `app/ponto/services/vigia_cadastro_futuro.py:21`, `:45`.

- **Semântica de `hoje`:** no suplemento é o dia civil usado para escolher competência e futuro estrito; o dia da célula/turno continua separado em `dia`/`cel.data`. Não identifiquei significado duplo no parâmetro alterado. `app/ponto/services/cadastro_futuro.py:26`, `:27`, `:43`, `:45`; `app/ponto/portas/celula.py:1137`. `timezone.localdate()` produz o valor nas portas, enquanto o cron usa `datetime.date.today()`; equivalência de fuso efetiva em produção [não verificado]. `app/colaboradores/services/vinculo.py:314`; `app/ponto/management/commands/gerar_celulas.py:339`.

- **Juiz novo/derivação paralela: ausentes no delta examinado.** O suplemento usa o carregador `vinculos_dos_dias`; este chama `vinculo_do_dia`, autoridades registradas no anexo vivo. A sincronização da foto usa a derivação existente do calendário. `app/ponto/services/cadastro_futuro.py:38`; `app/escala/alimentacao.py:334`, `:358`; `app/docs/JUIZES.md:120`; `app/escala/folgas.py:629`.

- **Ordem de travas:** o snapshot mantém E→C: vínculos por PK, depois células; `fechar_vigencia` escreve E antes de chamar suplemento que trava C. O delta acrescenta `atomic` na limpeza, sem nova trava explícita. Nenhuma inversão nova demonstrada; ausência global de deadlock [não verificado]. `app/colaboradores/services/vinculo.py:1008`, `:1013`, `:629`, `:650`; `app/ponto/services/cadastro_futuro.py:31`; `app/ponto/portas/celula.py:1145`.

- **Selo com problema novo:** o mutante `desfazer_apaga_admin` ainda procura o corpo anterior, sem o novo `atomic` e sem `_t =`; seu alvo desapareceu. Isso exige reconferir a execução do mutante, sem declarar selo completo por inspeção. `app/ponto/tests/test_cadastro_futuro_c5.py:37`; `app/ponto/portas/celula.py:1146`.

- **L-112 preservada no delta:** executor continua lendo `v['data_inicio']` e `v['data_ancora']`, gravando a declaração explícita e fechando a vigência inativa sobreposta pela porta. O delta só unifica parte das leituras posteriores de relógio; não transforma batida em limite da declaração. `app/colaboradores/services/vinculo.py:179`, `:235`, `:274`, `:285`; `app/docs/LEIS.md:154`.

- **Pré-requisito/pouso:** atendimento à condição de pousar após E4B/E3 não demonstrável pela cópia e pelo delta anexado [não verificado]. [nao verificado]

- **DESCARTADO 3 continua descartado:** o cron ainda pergunta ao carregador do juiz, passando `hoje` explicitamente; vínculo inativo com fim que cobre o dia continua aceito pela autoridade. O delta não altera esse caminho. `app/ponto/management/commands/gerar_celulas.py:303`, `:321`, `:324`; `app/escala/alimentacao.py:272`, `:282`.

## 12/11 18:10 · sessao 2f0114aa1e5a · TIPO-NA-FONTE · verificador · origem=Code

**Parcial; mantenho a não aprovação como instrumento completo de “todo leitor e escritor”.** O delta cura defeitos concretos, mas introduz perdas silenciosas por sementes de laço e atribuições indevidas de proveniência. A medida também continua sem excluir acessor ou aplicar teto. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`, `bin/leitores_de_chave.py:695`)

Execução integral do selo, publicação do censo, identidade Git do pouso e comportamento em produção: **[não verificado]**. O selo contém gravações e remoção de temporários; seus casos foram examinados sem executá-lo. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:29`, `bin/tests/test_leitores_de_chave.sh:82`)

**1. Destino dos achados da volta 1.**

| Achado anterior | Veredito nesta volta e lugar no delta |
|---|---|
| Briefs chamarem obrigatoriamente a ferramenta; entrega ao construtor/preverificador | **Não curado demonstravelmente.** O selo chama a ferramenta; o disjuntor continua exigindo referências textuais, sem conferir sua saída. Entrega efetiva: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`; `bin/disjuntor.py:1565`) |
| Universo Python, separação produção/testes, parse inválido, chave ausente, uso errado | **Garantias anteriores mantidas.** Parse e declaração falham antes da saída; consulta sem resultado continua rc 1 nomeado. Isso não prova completude da proveniência. (`bin/leitores_de_chave.py:623`, `bin/leitores_de_chave.py:635`, `bin/leitores_de_chave.py:683`, `bin/leitores_de_chave.py:736`) |
| Apelido de dois ou mais saltos | **Curado para cadeias acíclicas simples; parcial no geral.** `alias()` substitui o corte de um salto, mas ciclos de atribuições válidas podem perder proveniência. (`bin/leitores_de_chave.py:257`, `bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:270`, `bin/leitores_de_chave.py:288`) |
| Laço perdia `lamp` e as escritas de subchaves | **Curado no sítio apontado.** Agora o laço transmite `lampadas.[]`, recuperando as escritas de `acesa`, `luz` e `dispensada`. A solução geral é **parcial**, pelos defeitos de escopo abaixo. (`bin/leitores_de_chave.py:168`; `app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`) |
| Renomear somente o parâmetro declarado encolhia o censo silenciosamente | **Curado para o mutante exato; parcial como guarda de declaração.** Agora confere o nome, mas aceita também qualquer variável local atribuída com aquele nome. (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`; `bin/tests/test_leitores_de_chave.sh:82`) |
| Reatribuição mantinha proveniência indevida | **Parcial.** Apelidos comuns ambíguos viram lacuna e deixam de contar; isso também apaga acessos válidos anteriores. Nomes semeados continuam ignorando a reatribuição. (`bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:286`) |
| Dependência manual de `PARAMS/RETURNS` | **Não curada como fonte paralela de proveniência.** Os mapas continuam alimentando sementes e retornos presumidos; a guarda ficou mais forte, mas não verifica que o valor seja o JSON declarado. (`bin/leitores_de_chave.py:48`, `bin/leitores_de_chave.py:75`, `bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:330`, `bin/leitores_de_chave.py:608`) |
| `.ata/.dna`, `getattr` e argumentos nomeados aceitos sem provar classe | **Não curado; agora mais claramente declarado.** Permanecem candidatos sintáticos. (`bin/leitores_de_chave.py:44`, `bin/leitores_de_chave.py:282`, `bin/leitores_de_chave.py:309`, `bin/leitores_de_chave.py:554`) |
| Comprehension sobre lista literal/chamada, chamadas indiretas e consultas ORM | **Não curado.** Lista literal/chamada e ORM continuam explicitamente fora; a resolução de chamadas segue reconhecendo formas específicas. (`bin/leitores_de_chave.py:43`, `bin/leitores_de_chave.py:44`, `bin/leitores_de_chave.py:307`) |
| Contador confundido com persistência/catraca; sem exclusão do acessor ou teto | **Parcial na comunicação; não curado na medida contratada.** O delta explica a unidade e a escrita em memória, mas a seleção continua sendo eventos reconhecidos de topo. (`bin/leitores_de_chave.py:26`, `bin/leitores_de_chave.py:38`, `bin/leitores_de_chave.py:695`) |
| Meta zero retornava erro | **Curado.** Quando consegue medir, imprime inclusive zero e retorna rc 0. Parse, raiz e declaração ainda podem produzir rc 2 antes disso. (`bin/leitores_de_chave.py:675`, `bin/leitores_de_chave.py:683`, `bin/leitores_de_chave.py:687`, `bin/leitores_de_chave.py:697`) |
| Selo ignorava papéis, DNA e rc do contador sintético | **Curados nos casos exercitados.** Agora compara papéis, confere rc e exercita DNA com controle, mutante e vizinha. (`bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:164`) |
| Falha de `mktemp` permitia caminhos fora do temporário | **Curado para a falha apontada.** Aborta por falha, vazio ou ausência do diretório antes dos redirecionamentos. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:35`) |
| Árvore real só exigia presença mínima; raiz só provava presença | **Não curados.** Continuam mínimo de leitores/escritores, número positivo e existência de três arquivos; não há conferência de completude ou identidade do hash. (`bin/tests/test_leitores_de_chave.sh:23`, `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:54`) |

As constatações anteriores sobre ausência de escrita nos dados da aplicação e relógio apenas de duração permanecem: o instrumento lê fontes; o selo escreve cópias e usa `date` para duração. Acessor, migração e lei dos instantes não são implementados por esses caminhos. (`bin/leitores_de_chave.py:629`, `bin/leitores_de_chave.py:710`; `bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:183`; `aval.txt:5`, anexo)

**2. Novos riscos do delta.**

| Mudança | Falso verde / falso vermelho | [nao verificado]
|---|---|
| Guarda `PARAMS` | **Pode aceitar declaração semanticamente falsa:** renomear o parâmetro e deixar um local `ata={}` satisfaz `nomes`; esse local recebe a semente declarada. Presença do nome não prova sua origem. (`bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`) |
| Guarda `RETURNS` | **Pode aceitar retorno sem JSON:** `return None` e `return 42` satisfazem `retorna`, porque ambos têm expressão AST. Chamadas continuam recebendo raiz presumida pelo mapa. A guarda detecta ausência de função ou de expressão de retorno. (`bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:330`, `bin/leitores_de_chave.py:613`) |
| Semeadura de laço | **Produz falsos positivos e negativos.** A semente vale para todo o escopo; até uma semente vazia tem prioridade sobre aliases. Também contorna a verificação de reatribuição. (`bin/leitores_de_chave.py:165`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`) |
| Alias memoizado | **Resolve a cadeia simples, mas pode perder ciclos válidos:** `a=c.ata; b=a; a=b; a["k"]; b["j"]` perde ambos na reprodução em memória. O vazio devolvido por `busy` participa do resultado memoizado e da classificação como reatribuído. (`bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:270`) |
| Reatribuição excluída | **Pode dar falso verde na medida:** `a=c.ata; a["k"]; a={}` apaga o acesso válido anterior. A consulta de `k` pode dar falso negativo; o contador pode diminuir com rc 0. A lacuna não acompanha a saída do contador. (`bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:274`, `bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:736`) |
| rc 0 quando mede | **Correto para permitir a meta zero; insuficiente como certificado de completude.** Lacunas não impedem rc 0 e o modo não informa sua quantidade. (`bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:721`; `aval.txt:5`, anexo) |

O sítio real pedido é **`app/chamados/services/materializacao.py:177` e `:178`**. Os três cortes de string da lambda `_h_ok` passam a aparecer como `orfas.[].<dinamica>` por causa do `for h` **posterior**, em `:181`. O indexador não separa o escopo de lambda/comprehension, e a semeadura aplica o nome à função inteira. Isso é atribuição indevida de proveniência, não mudança física da linha do arquivo. (`app/chamados/services/materializacao.py:177`, `app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:181`; `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:199`, `bin/leitores_de_chave.py:286`)

Há outro exemplo real: o `x["entrada"]` que percorre `turnos` passa a ser atribuído a `lampadas.[].entrada`, contaminado pelo `x` da comprehension anterior de lâmpadas. (`app/ponto/services/diagnostico_escala.py:154`, `app/ponto/services/diagnostico_escala.py:156`, `app/ponto/services/diagnostico_escala.py:171`; `bin/leitores_de_chave.py:286`)

Dois contraexemplos adicionais reproduzidos em memória delimitam a falha:

- `a=c.ata; a["k"]; for a in []: pass` perde `k` **sem lacuna**: o laço instala semente vazia e impede a resolução do alias. (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`)
- `for a in c.ata["xs"]: pass; a={}; a["k"]` emite indevidamente `xs.[].k`, **sem lacuna**: a semente prevalece sobre os bindings. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:286`)

Esses falsos positivos podem tornar vermelha uma checagem por presença de acesso; as perdas podem tornar verde uma checagem por diminuição/zero. Neste contador específico, eventos aninhados falsos ficam fora de `top`; portanto não se deve atribuir automaticamente aumento numérico aos exemplos aninhados. (`bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:699`, `bin/leitores_de_chave.py:736`)

**3. Mordida e vacuidade do selo.**

| Caso | Avaliação |
|---|---|
| **f2** | **Morde o defeito exato e não é vazio:** exige âncora antes da mutação, renomeia o parâmetro e cobra rc 2, nome declarado e stdout vazio. Não demonstra verdade da proveniência nem resistência a um local homônimo. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:84`; `bin/leitores_de_chave.py:188`) |
| **h** | **Morde a regressão zero→rc 1.** Universo sem acesso é o objeto do teste, não vacuidade: exige exatamente zero, rc 0 e uma ressalva do rótulo. Os outros casos exigem contagens positivas/exatas. (`bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:54`, `bin/tests/test_leitores_de_chave.sh:145`) |
| **i** | **Morde DNA ausente e desaparecimento da chave:** exige medida positiva, controle conhecido, mutação, rc 1 nomeado e vizinha preservada. Não prova cobertura completa de DNA. (`bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:168`, `bin/tests/test_leitores_de_chave.sh:171`, `bin/tests/test_leitores_de_chave.sh:175`) |
| **g ampliado** | **Morde dois saltos simples, laço simples e papéis.** A reatribuição testada contém somente acesso posterior ao valor não JSON; não testa acesso válido anterior, ciclos ou sementes vazias. Lacunas são comparadas por conjunto caminho/forma, sem localização ou multiplicidade. (`bin/tests/test_leitores_de_chave.sh:113`, `bin/tests/test_leitores_de_chave.sh:119`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:134`) |

**Falta mutante de `RETURNS`.** f/f2 alteram uma função de `PARAMS`; os sintéticos desligam declarações. Retirar somente a verificação de `RETURNS` não contraria nenhuma dessas asserções. Logo, “todos os casos mordem” não demonstra mordida desse ramo. (`bin/leitores_de_chave.py:61`, `bin/leitores_de_chave.py:610`; `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:91`, `bin/tests/test_leitores_de_chave.sh:129`, `bin/tests/test_leitores_de_chave.sh:184`)

**É furo de cobertura da garantia adicionada**, mas o contrato fornecido exige explicitamente o mutante de chave conhecida sem resultado, não um mutante para cada ramo de declaração. O furo semântico de aceitar qualquer expressão retornada existe independentemente dessa ausência de teste. (`contrato.txt:6`, anexo; `bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:613`)

**4. ESMERIL-SEMÂNTICA: as saídas compartilham eventos, mas seus números não são intercambiáveis.**

| Saída | Quem escreve; quem lê; significado |
|---|---|
| **Texto** | `run()` imprime eventos selecionados por seção, incluindo descendentes e `*`. O selo consulta principalmente rc/stderr dessas chamadas; não valida o conteúdo da listagem textual. (`bin/leitores_de_chave.py:702`, `bin/leitores_de_chave.py:727`, `bin/leitores_de_chave.py:730`; `bin/tests/test_leitores_de_chave.sh:62`, `bin/tests/test_leitores_de_chave.sh:67`) |
| **`--json`** | `run()` publica listas filtradas, lacunas globais e resumo global. O selo lê sítios, papéis, formas, separação e lacunas. `operacoes_topo` é o mesmo total usado pelo contador; tamanho da lista `producao` não é esse total. (`bin/leitores_de_chave.py:713`, `bin/leitores_de_chave.py:719`, `bin/leitores_de_chave.py:721`; `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:134`) |
| **`--contar-crus`** | Imprime `len(top)`: eventos reconhecidos de produção cujo caminho é exatamente uma chave literal de topo. O selo lê o número e rc; h também lê parte do rótulo. Descendentes, `*`, caminhos dinâmicos e aliases descartados não compõem esse total. (`bin/leitores_de_chave.py:694`, `bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:699`; `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`) |

A unidade continua sendo evento deduplicado por posição/caminho/papéis/forma. Uma operação com LEITOR e ESCRITOR vale um evento no contador, mas participa das duas contagens por papel no JSON. (`bin/leitores_de_chave.py:361`, `bin/leitores_de_chave.py:715`, `bin/leitores_de_chave.py:716`)

**O rótulo stderr melhorou e descreve o recorte principal**, inclusive escrita em memória e ausência de exclusão do acessor. Entretanto, não explicita “reconhecidos”, exclusão de reatribuídos ou lacunas. O selo ainda chama a medida de “catraca”, enquanto o instrumento declara que teto/catraca não mora nele; o contrato exige acessos fora do acessor. Portanto a concordância de significado continua parcial. (`bin/leitores_de_chave.py:29`, `bin/leitores_de_chave.py:38`, `bin/leitores_de_chave.py:274`; `bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`; `contrato.txt:5`, anexo)

Há ainda rótulo textual desatualizado: “lacunas ... chave dinamica/copia inteira” agora também totaliza `apelido`. (`bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:732`)

Consumidores efetivos em briefs, construtor ou HAIKU, além do selo demonstrado: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:38`)

**5. QUEM ESCREVE e QUEM LÊ os valores alterados pelo diff.**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `ROTULO_CRUS` | Literal. (`bin/leitores_de_chave.py:38`) | `run()`→stderr; h confere substring. (`bin/leitores_de_chave.py:698`; `bin/tests/test_leitores_de_chave.sh:157`) |
| `CONTA_REATRIBUIDO` | Constante `False`. (`bin/leitores_de_chave.py:40`) | Retorno de `alias()`. (`bin/leitores_de_chave.py:274`) |
| `NAO_COBERTOS` | Enumeração de limites. (`bin/leitores_de_chave.py:41`) | JSON e texto. (`bin/leitores_de_chave.py:722`, `bin/leitores_de_chave.py:734`) |
| Bindings em `env` | Indexação de atribuições→`bind()`. (`bin/leitores_de_chave.py:142`, `bin/leitores_de_chave.py:212`) | Novo resolvedor de aliases. (`bin/leitores_de_chave.py:267`) |
| `busy` | Inicialização; inclusão/remoção durante resolução. (`bin/leitores_de_chave.py:117`, `bin/leitores_de_chave.py:266`, `bin/leitores_de_chave.py:268`) | Guarda de ciclo. (`bin/leitores_de_chave.py:263`) |
| `memo[(escopo,nome)] = (out, reatribuido)` | Resolução dos bindings; limpeza por rodada e ao terminar semeadura. (`bin/leitores_de_chave.py:163`, `bin/leitores_de_chave.py:175`, `bin/leitores_de_chave.py:270`) | Consulta/cache de `alias()`. (`bin/leitores_de_chave.py:265`, `bin/leitores_de_chave.py:271`) |
| `ps`, `atual`, `novo`; sementes `<chave>.[]` | Laço de até quatro rodadas; união em `seeds`. (`bin/leitores_de_chave.py:162`, `bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:171`) | Controle de nova rodada e `paths(Name)`; depois emissão de eventos. (`bin/leitores_de_chave.py:173`, `bin/leitores_de_chave.py:286`, `bin/leitores_de_chave.py:447`, `bin/leitores_de_chave.py:482`) |
| Lacuna `reatribuido:<nome>` / `apelido` | `alias()`; lacunas preliminares são apagadas após semeadura. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:273`) | `censo()`→JSON/total textual; g lê caminho/forma. (`bin/leitores_de_chave.py:639`, `bin/leitores_de_chave.py:721`, `bin/leitores_de_chave.py:732`; `bin/tests/test_leitores_de_chave.sh:134`) |
| Assinatura `nomes` | Parâmetros e nomes locais `Store`. (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`) | Guarda `PARAMS`. (`bin/leitores_de_chave.py:608`) |
| Assinatura `retorna` | Qualquer `Return` com expressão. (`bin/leitores_de_chave.py:190`) | Guarda `RETURNS`. (`bin/leitores_de_chave.py:613`) |
| `defs[f.rel]`, `faltam` | `censo()` guarda assinaturas; guard acrescenta faltantes. (`bin/leitores_de_chave.py:634`, `bin/leitores_de_chave.py:605`, `bin/leitores_de_chave.py:609`, `bin/leitores_de_chave.py:614`) | `run()` imprime diagnóstico e retorna rc 2; f/f2 conferem. (`bin/leitores_de_chave.py:687`, `bin/leitores_de_chave.py:690`; `bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:93`) |
| `len(top)` / rc 0 | Seleção produção→topo; impressão/retorno. (`bin/leitores_de_chave.py:693`, `bin/leitores_de_chave.py:699`) | JSON e selo real/e/h/i. (`bin/leitores_de_chave.py:713`; `bin/tests/test_leitores_de_chave.sh:52`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:165`) |
| `_tmp` | `mktemp`, agora validado. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:28`) | Redirecionamentos, cópias, fixtures e limpeza. (`bin/tests/test_leitores_de_chave.sh:29`, `bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:57`, `bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:161`) |
| `LC` / parâmetro f2 | Caminho e `sed` de renomeação/restauração. (`bin/tests/test_leitores_de_chave.sh:79`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:87`) | Âncora, ferramenta e asserção. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:83`, `bin/tests/test_leitores_de_chave.sh:84`) |
| Fixture g; `G`, `ESP`, `A` | Heredoc; extração JSON e expectativa literal. (`bin/tests/test_leitores_de_chave.sh:113`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:134`) | Comparações de eventos/lacunas e contador esperado 10. (`bin/tests/test_leitores_de_chave.sh:132`, `bin/tests/test_leitores_de_chave.sh:135`, `bin/tests/test_leitores_de_chave.sh:145`) |
| `Z` / fonte sem acesso | Fixture zero. (`bin/tests/test_leitores_de_chave.sh:155`) | Ferramenta e asserção h. (`bin/tests/test_leitores_de_chave.sh:156`, `bin/tests/test_leitores_de_chave.sh:157`) |
| `D`, `N_DNA`, chave mutada | Cópia, captura do contador e substituição de literal. (`bin/tests/test_leitores_de_chave.sh:161`, `bin/test

[truncado em 20000 de 21407 caracteres]

## 12/11 18:08 · sessao f61fb656431c · - · - · origem=Ronald (tmux)

Achados de proveniência, com reprodução somente em memória:

- **Apelido de 2+ saltos: curado para cadeia acíclica simples.** `a=c.ata; b=a; b["k"]` agora aparece; resolução passa por `alias()` com memo e guarda de ciclo. (`bin/leitores_de_chave.py:257`, `bin/leitores_de_chave.py:288`)
- **Perda de `lamp` em `dispensar_por_ausencia`: curada no sítio real.** O laço transmite `lampadas.[]` e agora aparecem leitura em `:807`, escritas em `:808–810` e leitura em `:812`. (`bin/leitores_de_chave.py:167`, `app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:807`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`, `app/ponto/services/cartorio.py:812`)
- **Guarda de nome declarado: parcial.** Renomear exclusivamente o parâmetro de `_orfas_da_ata` agora falha; porém um local `ata={}` mantém a assinatura válida e recebe proveniência declarada, mesmo com parâmetro renomeado. O guard aceita parâmetros **ou qualquer `Name(Store)`**. (`bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`, `app/escala/services/leitor_celula.py:180`)
- **Guarda de `RETURNS`: parcial.** Detecta função ausente e `return` sem expressão; aceita `return None` e `return 42`, porque apenas testa `n.value is not None`. Logo prova presença sintática de retorno, sem provar que retorna JSON. (`bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:613`, `bin/leitores_de_chave.py:330`)

Falsos resultados novos confirmados:

1. **Semeadura retroativa real:** `h[2]`, `h[:2]`, `h[3:]` da lambda `_h_ok`, definidos em `app/chamados/services/materializacao.py:177` e `app/chamados/services/materializacao.py:178`, passam a sair como `orfas.[].<dinamica>`, devido ao `for h in ...orfas` **posterior**, em `app/chamados/services/materializacao.py:181`. Lambda e comprehension não criam escopo no indexador; sementes valem para a função inteira. (`bin/leitores_de_chave.py:167`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:199`, `bin/leitores_de_chave.py:285`)

2. **Outro falso positivo real:** `x['entrada']`, cujo `x` percorre `turnos`, vira `lampadas.[].entrada`; o `x` usado antes na comprehension de lâmpadas contamina o restante da função. O mesmo atinge `x` percorrendo `dias` em `:209–235`. (`app/ponto/services/diagnostico_escala.py:154`, `app/ponto/services/diagnostico_escala.py:156`, `app/ponto/services/diagnostico_escala.py:171`, `app/ponto/services/diagnostico_escala.py:208`, `app/ponto/services/diagnostico_escala.py:209`)

3. **Seed vazio apaga alias silenciosamente:** sintético `a=c.ata; a["k"]; for a in []: pass; a["j"]` perde os dois acessos, **sem lacuna**. O bloco sempre cria entrada em `seeds`, mesmo com `ps=set()`, e `paths(Name)` retorna essa entrada antes de tentar o alias. (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`)

4. **Seed também burla reatribuição:** sintético `for a in c.ata["xs"]: pass; a={}; a["k"]` emite `xs.[].k` sem lacuna. A proveniência semeada ignora os bindings e não consulta `CONTA_REATRIBUIDO`. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:274`, `bin/leitores_de_chave.py:286`)

5. **Reatribuição reduz medida apesar de acesso válido anterior:** sintético `a=c.ata; a["k"]; a={}` remove `k`, deixando lacunas. A lacuna é visível no JSON e contada no texto, porém `--contar-crus` retorna número menor com rc 0 sem informar a lacuna. Isso permite falso verde se o número for usado como catraca/completude. (`bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:721`, `bin/leitores_de_chave.py:732`)

6. **Ciclo válido de atribuições perde proveniência:** sintético `a=c.ata; b=a; a=b; a["k"]; b["j"]` termina, mas perde ambos. `busy` devolve conjunto vazio; esse resultado parcial participa do memo e da detecção de reatribuição. (`bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:270`)

Comparação v1 reconstruída em memória versus atual: eventos **de topo** permanecem 22→22 em `cartorio.py`, 20→20 em `leitor_celula.py`, 10→10 em `juizes.py`; nenhum topo entra/sai nesses três arquivos. Unidade/filtro: `bin/leitores_de_chave.py:695`; emissão e deduplicação: `bin/leitores_de_chave.py:347`, `bin/leitores_de_chave.py:361`.

No trecho `_ata_ate_o_teto`, entram eventos aninhados nas linhas `914`, `915` e `917`: leitura de `acesa/tipo/hora`, cópia de `lampadas.[].*`, escritas `lampadas.[].acesa`/`pendente` e leitura final de `acesa`. (`app/ponto/services/cartorio.py:913`, `app/ponto/services/cartorio.py:914`, `app/ponto/services/cartorio.py:915`, `app/ponto/services/cartorio.py:917`)

## 12/11 18:07 · sessao 0f18aeb186a9 · - · - · origem=Ronald (tmux)

| Valor do delta | Quem escreve | Quem lê / efeito |
|---|---|---|
| `ROTULO_CRUS` | Literal em `bin/leitores_de_chave.py:38` | `run` imprime no stderr (`bin/leitores_de_chave.py:698`); h confere apenas substring (`bin/tests/test_leitores_de_chave.sh:157`). |
| `CONTA_REATRIBUIDO=False` | `bin/leitores_de_chave.py:40` | `alias` descarta proveniência ambígua (`bin/leitores_de_chave.py:274`). |
| `NAO_COBERTOS` | `bin/leitores_de_chave.py:41` | JSON (`bin/leitores_de_chave.py:722`) e texto (`bin/leitores_de_chave.py:734`); g confere aviso de `--sem-declaracoes`, não essa enumeração (`bin/tests/test_leitores_de_chave.sh:132`). |
| `env` / bindings usados pelo novo alias | Atribuições indexadas e `bind` (`bin/leitores_de_chave.py:142`, `bin/leitores_de_chave.py:212`) | `alias` resolve cada binding (`bin/leitores_de_chave.py:267`). |
| `memo` | Inicialização (`bin/leitores_de_chave.py:117`), limpeza por rodada/final (`bin/leitores_de_chave.py:163`, `bin/leitores_de_chave.py:175`), preenchimento `(out, reatribuido)` (`bin/leitores_de_chave.py:270`) | Teste de presença e recuperação (`bin/leitores_de_chave.py:265`, `bin/leitores_de_chave.py:271`). |
| `busy` | Inicialização (`bin/leitores_de_chave.py:117`), inclusão/remoção (`bin/leitores_de_chave.py:266`, `bin/leitores_de_chave.py:268`) | Guarda de ciclo retorna conjunto vazio (`bin/leitores_de_chave.py:263`). |
| `seeds` / caminho `<chave>.[]` | PARAMS (`bin/leitores_de_chave.py:138`), atribuição a atributo (`bin/leitores_de_chave.py:155`), `values_list` (`bin/leitores_de_chave.py:246`), novo laço (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:171`) | `paths(Name)` dá precedência à seed (`bin/leitores_de_chave.py:286`), alimentando novos eventos de `scan` (`bin/leitores_de_chave.py:447`, `bin/leitores_de_chave.py:482`). |
| `ps`, `atual`, `novo` | `bin/leitores_de_chave.py:164`, `bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:172` | União de caminhos e encerramento das até quatro rodadas (`bin/leitores_de_chave.py:170`, `bin/leitores_de_chave.py:173`, `bin/leitores_de_chave.py:162`). |
| Lacuna `reatribuido:<nome>`, forma `apelido` | `alias` (`bin/leitores_de_chave.py:273`); lacunas preliminares apagadas ao fim da semeadura (`bin/leitores_de_chave.py:176`) | `censo` serializa (`bin/leitores_de_chave.py:639`); JSON publica (`bin/leitores_de_chave.py:721`); texto só totaliza (`bin/leitores_de_chave.py:732`); g lê conjunto caminho/forma (`bin/tests/test_leitores_de_chave.sh:134`). |
| Assinatura `nomes` | Parâmetros e nomes `Store` (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`) | Guard PARAMS compara existência do nome (`bin/leitores_de_chave.py:608`). |
| Assinatura `retorna` | Existência de qualquer `Return` com expressão AST (`bin/leitores_de_chave.py:190`) | Guard RETURNS (`bin/leitores_de_chave.py:613`). |
| `defs[f.rel]` | `censo` recebe `f.assinaturas()` (`bin/leitores_de_chave.py:634`) | `run` entrega ao guard (`bin/leitores_de_chave.py:687`), que consulta arquivo/função (`bin/leitores_de_chave.py:603`, `bin/leitores_de_chave.py:612`). |
| `faltam` | Guard acrescenta função/nome/retorno ausente (`bin/leitores_de_chave.py:605`, `bin/leitores_de_chave.py:609`, `bin/leitores_de_chave.py:614`) | `run` emite diagnóstico e rc 2 (`bin/leitores_de_chave.py:688`); f/f2 conferem rc, diagnóstico e stdout vazio (`bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:93`). |
| Número `len(top)` / rc 0 | Seleção produção→chaves literais de topo (`bin/leitores_de_chave.py:693`); impressão e retorno (`bin/leitores_de_chave.py:699`) | Selo real (`bin/tests/test_leitores_de_chave.sh:52`), e (`bin/tests/test_leitores_de_chave.sh:145`), h (`bin/tests/test_leitores_de_chave.sh:157`), DNA (`bin/tests/test_leitores_de_chave.sh:165`). JSON publica o mesmo total (`bin/leitores_de_chave.py:713`). |
| `_tmp` | `mktemp`, agora conferido (`bin/tests/test_leitores_de_chave.sh:27`) | Guarda de diretório (`bin/tests/test_leitores_de_chave.sh:28`), redirecionamentos (`bin/tests/test_leitores_de_chave.sh:35`), cópias/fixtures (`bin/tests/test_leitores_de_chave.sh:57`, `bin/tests/test_leitores_de_chave.sh:97`, `bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:161`), limpeza (`bin/tests/test_leitores_de_chave.sh:29`). |
| `LC` / parâmetro mutado f2 | Caminho (`bin/tests/test_leitores_de_chave.sh:79`), `sed` de renomeação (`bin/tests/test_leitores_de_chave.sh:82`) | Âncora (`bin/tests/test_leitores_de_chave.sh:80`), ferramenta (`bin/tests/test_leitores_de_chave.sh:83`), asserção (`bin/tests/test_leitores_de_chave.sh:84`); restauração (`bin/tests/test_leitores_de_chave.sh:87`). |
| Fixture g: laço, dois saltos, reatribuição | Heredoc (`bin/tests/test_leitores_de_chave.sh:113`) | AST da ferramenta (`bin/tests/test_leitores_de_chave.sh:129`); `G/ESP` com papéis (`bin/tests/test_leitores_de_chave.sh:130`); `A` para lacunas (`bin/tests/test_leitores_de_chave.sh:134`); contador esperado 10 (`bin/tests/test_leitores_de_chave.sh:145`). |
| `Z` / universo zero | Caminho e fonte sem acesso (`bin/tests/test_leitores_de_chave.sh:155`) | Ferramenta (`bin/tests/test_leitores_de_chave.sh:156`); h exige zero, rc 0 e substring (`bin/tests/test_leitores_de_chave.sh:157`). |
| `D`, `N_DNA`, chave mutada | Cópia (`bin/tests/test_leitores_de_chave.sh:161`), captura (`bin/tests/test_leitores_de_chave.sh:165`), mutação literal (`bin/tests/test_leitores_de_chave.sh:171`) | Contagem positiva (`bin/tests/test_leitores_de_chave.sh:167`), controle (`bin/tests/test_leitores_de_chave.sh:168`), mutante (`bin/tests/test_leitores_de_chave.sh:172`), vizinha (`bin/tests/test_leitores_de_chave.sh:175`), mensagem final (`bin/tests/test_leitores_de_chave.sh:184`). |
| stdout, stderr, `rc` entre chamadas | `_lc` sobrescreve os três (`bin/tests/test_leitores_de_chave.sh:35`) | `_jq` lê stdout JSON (`bin/tests/test_leitores_de_chave.sh:38`); asserções leem número, diagnóstico e status (`bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:173`). |

ESMERIL: o número do contador e `resumo_producao.operacoes_topo` compartilham `len(top)`, mas o texto e as listas JSON incluem eventos selecionados com descendentes e `*`; portanto seus tamanhos não são intercambiáveis. (`bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:703`, `bin/leitores_de_chave.py:713`, `bin/leitores_de_chave.py:719`, `bin/leitores_de_chave.py:727`)

A linguagem do selo ainda chama essa medida de “catraca”, enquanto o instrumento declara que teto/catraca não mora ali. O texto também continua descrevendo lacunas apenas como dinâmica/cópia inteira, embora agora inclua `apelido`. (`bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`; `bin/leitores_de_chave.py:29`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:732`)

Consumidor executável demonstrado dessas saídas: o selo; entrega efetiva a briefs, construtor, preverificador ou HAIKU: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:38`)

## 12/11 18:06 · sessao 9e5e15709a4a · - · - · origem=Ronald (tmux)

- **f2 morde e não é vazio:** exige âncora real antes de renomear; cobra rc 2, nome declarado e stdout vazio. O guard anterior passaria indevidamente esse caso. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:84`; `app/escala/services/leitor_celula.py:180`)
- **h morde a regressão zero→rc 1:** cobra exatamente `0`, rc 0 e a ressalva sobre acessor. Seu universo é vazio por construção; isso não torna o conjunto vacuamente verde, porque a/e/i exigem medidas positivas/exatas. (`bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:54`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:167`)
- **i morde DNA quebrado e desaparecimento:** medida positiva, controle conhecido rc 0, mutação, rc 1 nomeado e vizinha preservada. Não demonstra completude de DNA. (`bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:168`, `bin/tests/test_leitores_de_chave.sh:171`, `bin/tests/test_leitores_de_chave.sh:173`, `bin/tests/test_leitores_de_chave.sh:175`)
- **RETURNS continua sem mutante próprio:** f/f2 mexem exclusivamente em `_orfas_da_ata`, declarada em PARAMS; os sintéticos seguintes desligam declarações. Retirar só a verificação RETURNS pode preservar todas as asserções do selo atual. É lacuna de cobertura da promessa “todos os casos mordem”, sem estabelecer obrigação contratual explícita de um mutante para cada ramo. (`bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:91`, `bin/tests/test_leitores_de_chave.sh:129`, `bin/leitores_de_chave.py:61`, `bin/leitores_de_chave.py:610`, `bin/tests/test_leitores_de_chave.sh:184`)
- **Guarda curada parcialmente:** agora verifica nome declarado e existência de `return <expressão>`, mas nomes locais `Store` satisfazem PARAMS, e `return None`/`return 0` satisfazem RETURNS. Reprodução em memória confirmou essas três situações; a guarda prova presença sintática, sem provar JSON retornado/recebido. (`bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:608`, `bin/leitores_de_chave.py:613`)
- **Achados v1 curados no selo:** papéis de setdefault/pop agora conferidos; rc do contador sintético conferido; DNA exercitado; falha de mktemp aborta antes das gravações. (`bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:27`)
- **Achados v1 persistentes:** árvore real exige somente presenças mínimas e total positivo; raiz comprova presença, não identidade do hash; não há asserção de teto anterior ou exclusão de acessor. (`bin/tests/test_leitores_de_chave.sh:23`, `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:54`; `bin/leitores_de_chave.py:695`)
- **Semântica ainda divergente:** instrumento declara que teto/catraca não mora nele, mas selo continua nomeando medida “catraca”; texto rotula lacunas apenas como “chave dinamica/copia inteira”, omitindo novo `apelido`. h verifica apenas uma substring do rótulo. (`bin/leitores_de_chave.py:29`; `bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`; `bin/leitores_de_chave.py:732`)
- **Limite novo de g:** lacunas são verificadas por conjunto de caminho/forma, sem posição, função ou multiplicidade. (`bin/tests/test_leitores_de_chave.sh:134`, `bin/tests/test_leitores_de_chave.sh:135`)

## 12/11 17:46 · sessao d118260d8f2a · TRANCA-VIVAS-08 · sonda · origem=Code

O envelope executa a sonda antes do Codex e entrega a saída raspada no prompt ([bin/codex_pergunta.sh:326](bin/codex_pergunta.sh:326), [bin/codex_pergunta.sh:357](bin/codex_pergunta.sh:357)). Segue a transcrição integral recebida; reexecução independente: [nao verificado].

```text
sonda_leitura: sonda_v4.py [b406e34784086e43] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

log#747607 acao=veredito_celula modelo=CelulaDia obj=10153 usuario=657 ator=None em=2026-10-22T15:35:25.946330-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#747612 acao=veredito_celula modelo=CelulaDia obj=10153 usuario=657 ator=None em=2026-10-22T15:35:27.552330-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=discordante chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> discordante via cartorio'
log#747617 acao=veredito_celula modelo=CelulaDia obj=10152 usuario=657 ator=None em=2026-10-22T15:35:29.134502-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#747622 acao=veredito_celula modelo=CelulaDia obj=10152 usuario=657 ator=None em=2026-10-22T15:35:30.735412-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=discordante chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> discordante via cartorio'
log#747627 acao=veredito_celula modelo=CelulaDia obj=10151 usuario=657 ator=None em=2026-10-22T15:35:32.359774-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#769000 acao=veredito_celula modelo=CelulaDia obj=4538 usuario=653 ator=None em=2026-10-26T11:05:49.557493-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=concorde veredito_depois=furo chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito concorde -> furo via cartorio'
log#933557 acao=veredito_celula modelo=CelulaDia obj=10151 usuario=657 ator=None em=2026-11-11T11:48:54.050593-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=furo chaves_depois=['protestos', 'status_dia', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> furo via cartorio'
cel#4538 colab367 data=2026-08-01 veredito=furo via=cartorio veredito_em=2026-10-26T11:05:49.511359-03:00
   logs veredito_celula com usuario: 1; com acoes (servico Resolver dia): 0
cel#10151 colab444 data=2026-08-03 veredito=furo via=cartorio veredito_em=2026-11-11T11:48:53.970330-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
cel#10152 colab444 data=2026-08-04 veredito=furo via=cartorio veredito_em=2026-11-12T06:27:03.004867-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
cel#10153 colab444 data=2026-08-05 veredito=discordante via=cartorio veredito_em=2026-10-22T15:35:27.512146-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
total_erros=0
== anexo: retira_acervo_tranca_2.txt ==
aval Ronald: RETIRA ACERVO-TRANCA-2 (publicado 16:3x, contraria CLAUDE.md 7b DINHEIRO item 3: nenhuma trava por exportada/trancada/paga; correcao provada entra em qualquer competencia). Nao executar os itens 1, 2, 3 e 5; nenhuma lei nova. Fica SO o item 4, que e lei existente (TRANCA-DE-VERDADE): as 4 perguntas vivas na comp 08 emp2 trancada -> listar pk e o degrau de chamados/juizes.py::pergunta_viva que as deixou vivas; curar na origem se for defeito; contador vivas_apesar_da_tranca na frota, esperado 0. Segue a fila; PAREI so em lei ou !
```

1. Pela assinatura recebida, **747607, 747612, 747617, 747622 e 747627** correspondem à trilha da lavra em **`app/ponto/portas/celula.py:1042`**, todos via **`cartorio`**; a descrição coincide com `app/ponto/portas/celula.py:1043`.
2. **769000** corresponde à mesma lavra, via **`cartorio`** (`app/ponto/portas/celula.py:1042`, `app/ponto/portas/celula.py:1043`).
3. **933557** corresponde à mesma lavra, via **`cartorio`**, incluindo `status_dia` no payload (`app/ponto/portas/celula.py:1044`).
4. Nenhum tem a assinatura de “Resolver dia”, que grava `acoes` (`app/ponto/services/veredito_celula.py:356`, `app/ponto/services/veredito_celula.py:358`), nem a descrição exclusiva de alteração de status do ramo `app/ponto/portas/celula.py:1028`.
5. **`_toque_humano_nos_dias` confunde as assinaturas:** usa a ação definida em `app/chamados/juizes.py:1088`, exige usuário não nulo/diferente do sistema (`app/chamados/juizes.py:1253`) e aceita ação+célula+horário, sem conferir `via`, `acoes` ou `ator` (`app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1292`).
6. Esse toque passa por `_admin_tocou_os_dias_depois` (`app/chamados/juizes.py:1225`) e afasta a tranca em `dias_trancados` (`app/chamados/juizes.py:1079`), influenciando o degrau de `pergunta_viva` em `app/chamados/juizes.py:1395`.
7. Outro leitor direto é **`decisao_do_dia`**: restringe previamente a célula à via humana `admin` (`app/ponto/services/veredito_celula.py:76`, `app/ponto/services/veredito_celula.py:110`) e consulta a ação para completar autoria (`app/ponto/services/veredito_celula.py:115`, `app/ponto/services/veredito_celula.py:120`).
8. A lavra pode herdar o usuário autenticado da requisição (`app/core/observ.py:113`); portanto, usuário preenchido não distingue “Resolver dia” de cartório. Identidade do usuário de sistema, horário da tranca, PKs das perguntas e efeito efetivo desses logs na produção: [nao verificado].

## 12/11 17:45 · sessao a73b5ea5ea07 · - · - · origem=Ronald (tmux)

Encontrei dois leitores diretos de `LogAuditoria.acao='veredito_celula'`:

- `_toque_humano_nos_dias`: constante em `app/chamados/juizes.py:1088`; exige usuário não nulo/diferente do sistema em `app/chamados/juizes.py:1253`; consulta ação, modelo e célula em `app/chamados/juizes.py:1287`; marca toque pelo horário em `app/chamados/juizes.py:1292`, sem conferir `acoes`, `via` ou `ator`. Esse toque afasta a tranca em `app/chamados/juizes.py:1079`.
- `decisao_do_dia`: primeiro restringe as células a `veredito_via='admin'` em `app/ponto/services/veredito_celula.py:110`; depois consulta essa ação em `app/ponto/services/veredito_celula.py:115` para obter o autor da última trilha com nome em `app/ponto/services/veredito_celula.py:118`. Consumidores: calendário em `app/colaboradores/services/calendario.py:416` e API em `app/api/views.py:1411`.

Não encontrei outro leitor direto na busca da cópia; exaustividade fora dos arquivos pesquisados: [não verificado].

## 12/11 17:43 · sessao 3d62cbc2dfb9 · TRANCA-VIVAS-08 · sonda · origem=Code

Não houve nova execução: foi fornecida a saída raspada, sem o código de `sonda_v3.py`; execução independente [nao verificado]. Transcrição integral da saída fornecida [nao verificado]:

```text
sonda_leitura: sonda_v3.py [a35d1b44e7abf2fc] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T17:42:11.101502-03:00
PF#3 emp2 08/2026 fechado_em=2026-09-16T09:11:43.807307-03:00 reaberto_em=None janela_fechamento=(datetime.date(2026, 7, 21), datetime.date(2026, 8, 20))
#20798 col367 emp2 data_evento=2026-08-01 dia_turno=2026-08-01 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=False resposta_nao_vazia=False materializada=True via=sem_turno_aberto | celula=4538 veredito=furo via_cel=cartorio admin_decidiu=False concorda=n/a | viva=True
   toque cobrar: n=0 []
   toque resolver: n=1 [(769000, 653, '2026-10-26T11:05:49.557493-03:00')]
   toque desarquivar: n=0 []
#47879 col444 emp2 data_evento=2026-08-03 dia_turno=2026-08-03 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10151 veredito=furo via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747627, 657, '2026-10-22T15:35:32.359774-03:00'), (933557, 657, '2026-11-11T11:48:54.050593-03:00')]
   toque desarquivar: n=0 []
#47881 col444 emp2 data_evento=2026-08-04 dia_turno=2026-08-04 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10152 veredito=furo via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747617, 657, '2026-10-22T15:35:29.134502-03:00'), (747622, 657, '2026-10-22T15:35:30.735412-03:00')]
   toque desarquivar: n=0 []
#47884 col444 emp2 data_evento=2026-08-05 dia_turno=2026-08-05 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10153 veredito=discordante via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747607, 657, '2026-10-22T15:35:25.946330-03:00'), (747612, 657, '2026-10-22T15:35:27.552330-03:00')]
   toque desarquivar: n=0 []
total_erros=0
```

- Pelo dado fornecido, nas quatro perguntas a chave `(2,8,2026)`, derivada do fim da janela em `20/08/2026`, encontrou PF#3; portanto a causa não é período ausente. O juiz deriva e consulta essa chave em `app/chamados/juizes.py:1062`–`1068`. Dados de produção [nao verificado].
- **#20798:** chega ao degrau **célula acusadora**, `furo`, e retorna `True` em `app/chamados/juizes.py:1440`–`1441`, apesar do carimbo `sem_turno_aberto`. C20=False por **Resolver dia**, log 769000, usuário 653, em 26/10, depois de `fechado_em` de 16/09; comparação em `app/chamados/juizes.py:1292`, inversão em `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47879:** retorna no degrau **resposta preenchida, sem decisão administrativa atual, concordância não comprovada**: `concorda=None` faz `is not True` retornar `True` em `app/chamados/juizes.py:1407`–`1419`. C20=False por **Resolver dia**, logs 747627 e 933557, usuário 657, em 22/10 e 11/11, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47881:** mesmo degrau da resposta: `admin_decidiu=False`, `concorda=None`, retorno `True` em `app/chamados/juizes.py:1415`–`1419`. C20=False por **Resolver dia**, logs 747617 e 747622, usuário 657, em 22/10, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47884:** mesmo degrau da resposta, antes de examinar o veredito `discordante`: `admin_decidiu=False`, `concorda=None`, retorno `True` em `app/chamados/juizes.py:1415`–`1419`. C20=False por **Resolver dia**, logs 747607 e 747612, usuário 657, em 22/10, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **Resolver dia** corresponde a `veredito_celula` em `app/chamados/juizes.py:1088`; o filtro exige usuário preenchido e diferente do sistema em `app/chamados/juizes.py:1253`–`1254`. O dado fornecido mostra zero ocorrências de cobrar e desarquivar nas quatro perguntas [nao verificado].

## 12/11 17:37 · sessao d45010bd76f3 · E3-LEITORES · censo · origem=Code

**PERGUNTA DE DESENHO — o código tem várias operações que persistem a resposta.** O core humano, a resposta automática, o carimbo pela trilha e `PerguntaDisputa.carimbar` escrevem independentemente. Portanto, o censo não certifica **UMA porta de escrita**. (`app/chamados/services/disputa_emissao.py:1467`, `:1657`, `:1679`; `app/chamados/models.py:3157`)

A função denominada “porta do carimbo de resposta”, `recusa_respondida_sem_texto`, é uma **guarda**, sem persistência: constrói um `SimpleNamespace`, consulta o predicado e pode levantar exceção. (`app/chamados/services/fio.py:1578`, `:1585`, `:1587`)

**HEAD**

Critério: sítio de acesso ao campo, incluindo payloads encaminhados, projeções ORM e acessos dinâmicos; chamadas transitivas não são contadas novamente como acesso direto. A identificação Git do snapshot, execução dos testes e estado efetivo de produção ficam **[nao verificado]**. [nao verificado]

Nas tabelas, **E = `respondida_em`**, **T = `resposta_colab`**. Caminhos sem prefixo são relativos a **`app/chamados/`**. Linhas agrupadas pertencem à mesma função.

**Escritores e enquadramento nas portas**

A porta declarada para resposta web/API é `responder_pergunta_disputa_core`; ela recusa texto vazio e persiste o par. A guarda comum é `recusa_respondida_sem_texto`. São funções diferentes. (`services/disputa_emissao.py:1425`, `:1432`, `:1508`; `services/fio.py:1578`)

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo; porta |
|---|---|---|---|---|
| `models.py:1509:ChamadoColaborador._universo_da_ressuscitacao` | ESCRITOR — memória | `_q.respondida_em = None` | E | Cópia de `p` em `:1495`, vindo de `self.perguntas_que_resolvem` em `:1484`; relação de PD em `:3415`. **Fora do core**; simulação sem salvar, usada pelo juiz em `:1510`. |
| `models.py:1567:ChamadoColaborador.ressuscitar_perguntas` | ESCRITOR — delegação | `descarimbar_em_lote(..., respondida_em=None)` | E | `alvo` da mesma relação, `:1551`. **Fora do core**; passa pelo descarimbo em lote. |
| `models.py:2809:PerguntaDisputa.abrir` | ESCRITOR — encaminhamento possível | `**campos` para `_sob_contrato` | E/T recebidos | Método da classe PD. Caminho de nascimento; **fora do core**, chega à guarda em `:2864`. |
| `models.py:2821:PerguntaDisputa.abrir_conversa` | ESCRITOR — encaminhamento possível | `**campos` para `_sob_contrato` | E/T recebidos | Classe PD; mesmo enquadramento. |
| `models.py:2840:PerguntaDisputa._sob_contrato` | ESCRITOR — encaminhamento possível | `_nascer(**kw)` | E/T recebidos | `cls` da classe PD; chamadas em `:2809`, `:2821`. |
| `models.py:3039,3042:PerguntaDisputa._nascer` | ESCRITOR | `cls(..., **campos)`; `save(force_insert=True)` | E/T recebidos | `cls` de PD. **Fora do core; chama a guarda** em `:2864`, `:2865`. |
| `models.py:3157,3158,3177:PerguntaDisputa.carimbar` | ESCRITOR | atribuição; acrescenta E a `campos`; `save(update_fields=campos)` | E | `self` de PD. **Fora do core e da guarda comum**; exige texto em `:3155`. |
| `models.py:3188:PerguntaDisputa._desfazer_em_memoria` | ESCRITOR — memória | restaura `self.respondida_em` | E | `self` de PD; desfaz `carimbar`, chamadas em `:3166`, `:3169`, `:3180`. **Fora do core**, sem persistência própria. |
| `models.py:3289,3291:PerguntaDisputa.descarimbar_em_lote` | ESCRITOR | `PerguntaDisputa.objects...update(..., **outros)` | E/T recebidos | Modelo explícito. **Fora do core; HEAD sem guarda do par**. Payloads comprovados em `models.py:1567` e `services/disputa_emissao.py:1630`. |
| `models.py:3448:PerguntaDisputa.save` | ESCRITOR — persistência genérica | `super().save(*args, **kwargs)` | E/T conforme save | `self` de PD. Infraestrutura comum dos saves; **não concentra os `QuerySet.update`**, existentes em `:3291` e `services/disputa_emissao.py:1632`. |
| `services/disputa_emissao.py:1467,1468,1469,1508:responder_pergunta_disputa_core` | ESCRITOR | atribuições T/E; `save(update_fields=_campos)` | E/T | PD consultada em `views.py:1239`, passada ao core em `:1283`. **É o core**; exige texto em `:1432`, `:1434`; não chama a guarda comum. |
| `services/disputa_emissao.py:1593,1630,1632:reabrir_para_correcao` | ESCRITOR | `dict(E=None,T='')`; `descarimbar_em_lote(**campos)` ou `qs.update(**campos)` | E/T | Querysets PD fornecidos em `services/validacao.py:593`, `services/materializacao.py:688`. **Fora do core**; porta de reabertura. |
| `services/disputa_emissao.py:1657,1658,1660:responder_automaticamente` | ESCRITOR | atribuições T/E; `save(update_fields=[T,E])` | E/T | Perguntas do upload, chamada em `app/api/foto_recebida.py:48`. **Fora do core; chama a guarda** em `:1655`. |
| `services/disputa_emissao.py:1679,1681:carimbar_resposta_pela_trilha` | ESCRITOR | atribuição E; `save(update_fields=[E])` | E | PD consultadas pelo comando em `management/commands/curar_perguntas_em_limbo.py:55`, chamada em `:67`, `:68`. **Fora do core e da guarda comum**; guarda própria em `:1675`. |
| `bin/sombra_mascara_pessoal.py:57:<módulo>` | ESCRITOR — sombra | `setattr(o,c,novo)` | T | `apps.get_model(app,mod)` em `:46`; configuração identifica PD/T em `bin/sombra_regras_pessoais.py:85`. **Fora das portas**. |
| `bin/sombra_mascara_pessoal.py:62,64:<módulo>` | ESCRITOR — sombra | `bulk_update(lote,list(campos))` | T | Mesma resolução de modelo/campo. **Fora das portas**; o script exige banco `sombra` em `:10`. |

O nascimento aceita o par por `**campos`, independentemente de haver chamada atual passando essas chaves; sua aceitação e persistência estão no construtor e no save. (`models.py:2864`, `:3039`, `:3042`) Chamador de produção passando explicitamente o par ao nascimento: **[nao verificado]**.

**Leitores no modelo e nos juízes**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `models.py:1487:ChamadoColaborador._universo_da_ressuscitacao` | LEITOR | `Q(T__isnull=True)`, `Q(T='')` | T | Relação `perguntas_que_resolvem`, `:1484`; FK PD em `:3415`. |
| `models.py:1565:ChamadoColaborador.ressuscitar_perguntas` | LEITOR | `only(E,T)` | E/T | Mesmo queryset relacionado. |
| `models.py:1849,1850:DisputaSupervisao.respostas_sem_veredito` | LEITOR | atributos | E/T | `self.perguntas` em `:1847`; relação PD em `:2436`, `:2439`. |
| `models.py:1933:DisputaSupervisao.fechar` | LEITOR | atributo T para trilha | T | `self.perguntas` → `vivas`, `:1902`, `:1903`. |
| `models.py:2065:DisputaSupervisao.remover_motivo` | LEITOR | atributo T para trilha | T | Iteração de `self.perguntas.filter`, `:2061`. |
| `models.py:2864,2865:PerguntaDisputa._nascer` | LEITOR — payload | `campos.get(E/T)` | E/T | Método PD; valores destinados ao construtor em `:3039`. |
| `models.py:3070,3073:PerguntaDisputa.estado` | LEITOR | comparação de atributo | E | `self` da classe PD. |
| `models.py:3152,3155,3156:PerguntaDisputa.carimbar` | LEITOR | foto anterior; teste de texto; comparação | E/T | `self` da classe PD. | [nao verificado]
| `models.py:3373,3374:PerguntaDisputa.q_vivas` | LEITOR | `Q(E__isnull=False)`, exclusão de T vazio/nulo | E/T | Método da classe PD. |
| `models.py:3523:PerguntaDisputa.respondida` | LEITOR | atributo `is not None` | E | `self` da classe PD. |
| `models.py:3546,3551,3555,3559,3563,3566,3567,3568:PerguntaDisputa.resposta_display` | LEITOR | atributos; conversão; comparação; retorno | T | `self` da classe PD. |
| `models.py:3949,3956,3957,3958,3959,3963:DecisaoPergunta.linhas_da_projecao_sem_linha` | LEITOR | atributos para projeção da trilha | E/T | Recebe PD em chamadas como `models.py:1564` e `services/disputa_emissao.py:1466`. |
| `juizes.py:348,360:_slot_esperado` | LEITOR | atributos | E/T | Pergunta da cadeia do C6/materialização; chamada em `:752`. |
| `juizes.py:852,853:_chave_memo_pv` | LEITOR | `getattr` literal | E/T | Chamado por `pergunta_viva`, `:890`, `:902`; admite também sondadores, conforme `:844`. |
| `juizes.py:1346:_resposta_concorda_com_a_luz` | LEITOR | atributo | T | Recebe a pergunta do C6 em `:1419`. |
| `juizes.py:1375:projecao_temporal_da_pergunta` | LEITOR | atributos | E/T | Recebe a pergunta do C6 em `:1390`. |
| `juizes.py:1518:respondida_sem_texto` | LEITOR | atributos; `texto_vazio(T)` | E/T | Predicado aplicado às perguntas, inclusive em `models.py:1851`; também recebe sonda em `services/fio.py:1585`. |
| `juizes.py:1665:perguntas_corrigiveis` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` explícito. |
| `juizes.py:1672,1673,1675:perguntas_respondidas_a_validar` | LEITOR | filtro, exclusão, `order_by(E)` | E/T | `PerguntaDisputa.objects` explícito. |
| `juizes.py:1701:declarou_hora` | LEITOR | atributo | T | Perguntas da materialização, chamada em `services/materializacao.py:555`. |
| `juizes.py:1848:delta_da_resposta` | LEITOR | atributo | T | Pergunta da validação, chamada em `services/validacao.py:864`. |
| `juizes.py:1952,1957:classificar_resposta_validacao` | LEITOR | atributo | T | Fila PD em `app/api/views_mensageria.py:540`, `:544`. |
| `juizes.py:1974:motivo_implausivel_validacao` | LEITOR | atributo | T | Pergunta carregada em `services/validacao.py:389`, chamada em `:404`. |
| `juizes.py:2022,2026:irmas_respondidas_sem_consumo` | LEITOR | filtro ORM; atributo | E/T | `PerguntaDisputa.objects`, `:2020`. |

**Demais leitores de `app/chamados/`**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `relatorios_disputa.py:21:classificar` | LEITOR | atributo `is None` | E | `disputa.perguntas.all()` em `:12`. |
| `views.py:1280:responder_pergunta_disputa` | LEITOR | atributo `is not None` | E | Consulta PD em `:1239`. |
| `views.py:1416:reabrir_disputa_supervisao` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` explícito. |
| `views_cobrar.py:169,176:validar_inline` | LEITOR | atributo; filtro ORM | E | `_vivas_de(d)` e consulta PD em `:175`. |
| `views_cobrar.py:291:proposta_lote` | LEITOR | `x['pergunta'].T` | T | Itens classificados pela fila, `services/validacao.py:921`, `:924`. |
| `canal.py:65:perguntas_mudas_com_resposta` | LEITOR | filtro ORM | T | Consulta PD em `:58`, `:64`. |
| `reconciliador.py:365:lavrar_sem_turno_aberto` | LEITOR | atributo | T | Pergunta recebida pela cadeia de reatar, chamada em `:1620`. |
| `reconciliador.py:1083:veredito_orfa` | LEITOR | atributo | T | Universo PD em `:1048`, `:1049`; chamada em `:1136`. |
| `veredito_lavrado.py:207,208:marcos_do_lote` | LEITOR | filtro E; `**_ate(E)`; `Max(E)` | E | Alias `_PD` em `:201`; `_ate` gera limite em `:163`. |
| `services/acoes_disputa.py:95:acao_reabrir_disputa` | LEITOR | filtro ORM | E | Consulta PD explícita. |
| `services/acoes_disputa.py:130:acao_regularizar_disputa` | LEITOR | atributo | E | Perguntas de `_vivas_de(d)`. |
| `services/acoes_disputa.py:353:acao_corrigir_resposta` | LEITOR | atributo | T | Consulta PD em `:341`. |
| `services/becos.py:30:classificar` | LEITOR | atributo | E | `d.perguntas.all()` em `:29`. |
| `services/cobranca.py:229:_calados` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` em `:228`. |
| `services/declaracao_texto.py:417,419:declarar_hora_colab` | LEITOR | atributos; `bool(E)` | E/T | Retorno de `PerguntaDisputa.abrir`, `:376`. |
| `services/declaracao_texto.py:687:aplicar_declaracao` | LEITOR | atributo | E | Retorno de `PerguntaDisputa.abrir`, `:666`. |
| `services/disputa_emissao.py:1619,1627:reabrir_para_correcao` | LEITOR | `values_list(T,E)`; nomes `_txt`, `_resp` | E/T | Queryset de perguntas; usa portas PD em `:1609`, `:1630`. |
| `services/disputa_emissao.py:1647:responder_automaticamente` | LEITOR | atributo | T | Perguntas do upload, `app/api/foto_recebida.py:48`. |
| `services/disputa_emissao.py:1675,1683:carimbar_resposta_pela_trilha` | LEITOR | atributos | E/T | Queryset PD do comando, `management/commands/curar_perguntas_em_limbo.py:55`, `:68`. |
| `services/disputa_emissao.py:1718:recobrar_questionario` | LEITOR | filtro ORM | E | Consulta PD em `:1717`. |
| `services/disputa_emissao.py:1826,1844:reabrir_credor_de_pergunta_viva` | LEITOR | atributos | E/T | Relação `perguntas_que_resolvem`, `:1803`, `:1805`. |
| `services/disputa_emissao.py:1984,2057,2062,2074:_resolver_perguntas_por_chamado_resolvido` | LEITOR | atributos; serialização | E/T | Perguntas do chamado tratadas pela função, `:1984`. |
| `services/explicador.py:32:_explicar_validada` | LEITOR | atributo | T | Chamado por `explicar_pergunta`, `:97`; consulta PD em `:122`. |
| `services/explicador.py:79,80:explicar_pergunta` | LEITOR | atributos em textos | T | Consulta PD em `:122`, aplicação em `:133`. |
| `services/fio.py:174:aguarda_o_colaborador` | LEITOR | atributo; predicado do par | E | Coleção PD do fio, carregada em `:242`, encaminhada em `:361`. |
| `services/fio.py:681:processar_post_fio` | LEITOR | atributo | E | `perguntas_vivas_de`, importada em `:680`. |
| `services/fio.py:1254:canonica_do_grupo` | LEITOR | atributo | T | Grupo PD fornecido, por exemplo, por `management/commands/dedup_perguntas_fato.py:74`, `:76`. |
| `services/fio.py:1266:merito_perdedor` | LEITOR | atributo | T | Perguntas do mesmo grupo; constantes PD em `:1265`. |
| `services/foto_e4b.py:180:nascimentos` | LEITOR | `a[c]`, `d[c]`, com `c=E` | E | Ramo `perguntas:` em `:179`; foto PD em `:272`. |
| `services/foto_e4b.py:251:capturar.guardar` | LEITOR | `f.value_from_object(obj)` para todos os campos concretos | E/T | `guardar('perguntas', PerguntaDisputa.objects...)` em `:272`. |
| `services/historico.py:99,105,109:_eventos_dos_chamados` | LEITOR | `getattr(p,campo)` variável; atributo T | E/T | Queryset PD em `:96`; E integra a tupla em `:99`. |
| `services/materializacao.py:280,308:_tipo_pelo_marco` | LEITOR | atributos | T | Cadeia da materialização PD; queryset em `:1375` e carregamento em `services/validacao.py:389`. |
| `services/materializacao.py:359,405,412:_ts_tipo_do_plantio` | LEITOR | atributos | T | Mesma cadeia da materialização. |
| `services/materializacao.py:438:estado_aplicacao_pergunta` | LEITOR | atributo | T | Perguntas carregadas pelo fio em `services/fio.py:242`, chamada em `:357`. |
| `services/materializacao.py:474:_declarar_ausencia_de_pergunta` | LEITOR | atributo | T | Cadeia da materialização PD. |
| `services/materializacao.py:571:_materializar_ausencia_de_resposta` | LEITOR | atributo de `q` | T | `disputa.perguntas.filter`, `:538`. |
| `services/materializacao.py:633:perguntas_absorvidas_por` | LEITOR | filtro `T__in` | T | Base PD em `:629`. |
| `services/materializacao.py:674,675:devolver_absorvidas_da_ausencia_rejeitada` | LEITOR | atributos; foto em dict | E/T | `perguntas_absorvidas_por`, `:671`. |
| `services/materializacao.py:835,876,977:_materializar_resposta_hora` | LEITOR | atributos | T | Cadeia da materialização PD. |
| `services/materializacao.py:1001,1010,1062:_materializar_resposta_data_hora` | LEITOR | atributos | T | Cadeia da materialização PD. |
| `services/materializacao.py:1376:materializar_perguntas_validadas_da_disputa` | LEITOR | filtro ORM | E | `disputa.perguntas.filter`, `:1375`. |
| `services/painel.py:67:marcador_do_placar` | LEITOR | `Max(E)` | E | Agregação PD em `:66`. |
| `services/passivo_validado.py:83:_reversao_de` | LEITOR | `getattr(p,c,None)` | E | `CAMPOS_REVERSAO` em `:64`, `:66`; pergunta recebida em `:367`. |
| `services/pergunta_no_app.py:71:consulta_do_app` | LEITOR | atributo | E | `perguntas_do_app`, `:70`; consulta PD em `:49`. |
| `services/raiox.py:138,143,144:creditos_perguntas` | LEITOR | `values_list(E,T)`; índices `p[3]`, `p[4]` | E/T | `_pq=PerguntaDisputa.objects`, `:56`; seleção em `:135`, `:138`. |
| `services/validacao.py:141:_carimbar_validacao` | LEITOR | atributo | T | Pergunta carregada em `:389`. |
| `services/validacao.py:189,207:devolver_validacao_recusada` | LEITOR | `refresh_from_db(fields=[T,...])`; atributo | T | Instância carregada em `:389`, encaminhada em `:464`. |
| `services/validacao.py:236:_e_aceite_de_folga` | LEITOR | atributo | T | Helper da validação PD. |
| `services/validacao.py:393:validar_pergunta` | LEITOR | atributo | E | Consulta PD em `:389`. |
| `services/validacao.py:576,580,581,600:recusar_pergunta` | LEITOR | atributos; leitura `antes[T]` | E/T | Consulta PD em `:569`, `:570`. |
| `services/validacao.py:851,853,855,860,861,862,863,866:classe_do_lote` | LEITOR | atributos | T | Perguntas da fila, chamadas em `:921`, `:922`. |
| `services/validacao.py:924:fila_validar_classificada` | LEITOR | atributo; idade da resposta | E | Fila padrão em `:918`, `:921`; argumento alternativo `qs` sem validação de tipo. |
| `services/verbos.py:335,336,342,345:placar_aceite` | LEITOR | filtro; `values(E)`; `pg[E]` | E | Consulta PD em `:333`. |
| `services/vigia_fio.py:681:censo_alcance` | LEITOR | atributo | E | Universo PD em `:615`. |
| `services/vigia_fio.py:789,792:respondida_sem_resposta` | LEITOR | filtro E; `values_list(T)` | E/T | Consulta PD explícita em `:789`. |
| `templatetags/chamados_tags.py:142:leitura_da_resposta` | LEITOR | `getattr(perg,T,'')` | T | Recebe `perg` dos templates: `modal_fio.html:250`, `_pergunta_card.html:43`. |

**Leitores nos management commands**

Todos os caminhos desta tabela ficam em `app/chamados/management/commands/`.

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `curar_perguntas_em_limbo.py:32:nota_da_resposta` | LEITOR | atributo | T | Perguntas PD selecionadas em `:46`. |
| `curar_perguntas_em_limbo.py:46,69:Command.handle` | LEITOR | filtros ORM | E/T | `PerguntaDisputa.objects` explícito. |
| `validar_classe_a.py:80:Command.handle` | LEITOR | atributo | T | `d.respostas_sem_veredito()` em `:54`. |
| `auditar_invariantes_chamados.py:177:_contraprova_i21a` | LEITOR | atributo | E | Subconjunto PD de `:1572`, encaminhado em `:1585`. |
| `auditar_invariantes_chamados.py:592,1105,1106,1572,1575,1603,1655:Command.handle` | LEITOR | atributos; filtros ORM | E/T | Alias `PD.objects`; em `:1655`, relação de perguntas carregada em `:1652`. |
| `backfill_decisoes_pergunta.py:56,60,61:linhas_a_criar` | LEITOR | filtro; `only(E,T)` | E/T | Consulta PD explícita. |
| `backfill_decisoes_pergunta.py:326:aplicar` | LEITOR | atributo para trilha | T | Perguntas do backfill. |
| `backfill_decisoes_pergunta.py:504:Command.handle` | LEITOR | `only(T)` | T | Consulta PD explícita. |
| `dedup_perguntas_fato.py:75:Command.handle` | LEITOR | atributo | T | Queryset PD em `:74`. |

**Leitores fora de `app/chamados/`**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `bin/censo_folha.py:28,29:<módulo>` | LEITOR | filtros ORM | E | `PerguntaDisputa.objects`; import em `:12`. |
| `app/folha/previa.py:142:_material` | LEITOR | atributo | T | Mapa PD em `:64`, `:67`, encaminhado em `:95`. |
| `app/ponto/supra_juiz.py:545,548:pode_executar_csf` | LEITOR | atributos | E | Relações `d.perguntas` e `ch.perguntas_que_resolvem`, `:543`, `:548`; FKs em `app/chamados/models.py:2439`, `:3415`. |
| `app/api/views.py:1159,1161:api_chamados_v2` | LEITOR | atributos; guarda e serialização E | E/T | Perguntas de `consulta`, mapa em `:1029`, `:1035`; fonte PD em `services/pergunta_no_app.py:49`. |
| `app/api/views.py:2557,2559:api_disputa_perguntas._item` | LEITOR | atributos; guarda e serialização E | E/T | `consulta_do_app` em `:2532`, `:2535`; chamadas em `:2562`, `:2572`. |
| `app/api/views.py:2573:api_disputa_perguntas` | LEITOR | atributo | E | `consulta['historico']`; mesma origem. |
| `app/api/views.py:2616,2633:api_disputa_responder` | LEITOR | comparação; serialização | E | Consulta PD em `:2607`, `:2609`. |
| `app/api/views_mensageria.py:567,569,570:_item_validar` | LEITOR | atributos; serialização; diferença de datas | E/T | Fila em `:540`, `:546`; consulta PD em `app/chamados/juizes.py:1672`. |
| `app/colaboradores/services/calendario.py:474:contexto_calendario` | LEITOR | `exclude(T='')` | T | Alias PD em `:469`; queryset em `:473`. |
| `app/colaboradores/services/ficha.

[truncado em 20000 de 31100 caracteres]

_4388 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
