# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 10:28** · publicado em **18/11/2026 10:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 10:28 · sessao a2164c7661b8 · RESPOSTAS-DE-LEI-1811-1010 · leitura · origem=Code

## (a) O que a F3 propõe e o que existe hoje

O brief propõe que **os dois marcadores incluam um marco das linhas de `DecisaoPergunta` por `Max(pk)` ou `Count`**, além do marco de efeito; rejeita `max(ato_em)` porque uma linha inserida com `ato_em` antigo pode deixar esse máximo inalterado. T3 exige mudança com ato sem alteração da projeção; T3b exige mudança com a conservação retroativa. (`brief.md:64`, `brief.md:65`, `brief.md:79`, `brief.md:82`, `matriz.md:84`, `matriz.md:111`, `matriz.md:132`; linhas dos anexos fornecidos, correspondência com arquivos externos **[nao verificado]**)

Para `marcos_do_lote`, a F3 acrescenta que o marco deve respeitar `ate`, mas **não especifica o filtro que acompanha `Max(pk)` ou `Count`**. Se filtrados por `ato_em <= ate`, ambos mudam quando entra uma linha retroativa elegível; `max(ato_em)` pode não mudar. Sem filtro, `Max(pk)` e `Count` também veem linhas cujo ato é posterior a `ate`. Nenhuma dessas formas, isoladamente, identifica quais linhas já estavam inseridas em `ate`. (`brief.md:65`, `brief.md:82`, `matriz.md:111`, `app/chamados/models.py:4051`, `app/chamados/models.py:4077`)

Hoje, `marcos_do_lote` **não lê `DecisaoPergunta`**: agrega tempos de Batida, CelulaDia e os campos projetados `materializada_em` e `respondida_em` de PerguntaDisputa, aplicando `ate` aos tempos. Seu retorno é um `datetime` por colaborador; `esta_stale` compara esse tempo com `julgado_em`. O marcador do placar agrega campos de ChamadoColaborador e os três tempos projetados de PerguntaDisputa. (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:163`, `app/chamados/veredito_lavrado.py:177`, `app/chamados/veredito_lavrado.py:191`, `app/chamados/veredito_lavrado.py:202`, `app/chamados/veredito_lavrado.py:230`, `app/chamados/services/painel.py:62`)

`Max(pk)` e `Count` são inteiros, portanto não podem simplesmente ser acrescentados ao máximo temporal atual e comparados com `julgado_em`; precisariam de outro contrato de comparação e de um valor correspondente conservado na lavra. Essa mudança não está especificada nos anexos. (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:230`, `matriz.md:132`)

## (b) Censo de escritores e limite do instante de inserção

**Há escritor retroativo fora do backfill:** `carimbar_resposta_pela_trilha` usa o instante da nota anterior, recebido como argumento; o comando de manutenção o chama para curar perguntas em limbo. Portanto, a condição “só backfill” do corte retirado não está demonstrada e é contrariada por esse caminho de código. (`app/chamados/services/disputa_emissao.py:1685`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `corte_retirado.txt:1`)

“Anterior à inserção”, literalmente, também inclui o intervalo entre colher `timezone.now()` e executar o INSERT. A distinção útil abaixo é entre **instante histórico recebido/conservado**, que pode ser arbitrariamente anterior, e **agora capturado durante a operação**, que necessariamente antecede ou coincide com a execução posterior, sem medir sua duração. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/models.py:4162`)

| Escritor ou fonte de linhas | Origem de `ato_em` e retroatividade |
|---|---|
| `linhas_da_projecao_sem_linha` | Copia `respondida_em` ou `validada_em`; pode ser histórico. É chamada pelos pontos de ressuscitação e nascimento, resposta humana/automática, validação, devoluções, cura L-130 e backfill E4b. (`app/chamados/models.py:4122`, `app/chamados/models.py:4141`, `app/chamados/models.py:4146`, `app/chamados/models.py:1619`, `app/chamados/models.py:3137`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/validacao.py:167`, `app/chamados/services/validacao.py:272`, `app/chamados/services/validacao.py:345`, `app/chamados/services/validacao.py:436`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`) |
| Resposta humana | `_agora = timezone.now()` antes da transação e inserção. A mesma chamada também insere as linhas históricas de conservação. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1525`) |
| Resposta automática | Recebe `instante`; o chamador de produção indicado passa o agora do processamento. A assinatura aceita instante anterior; também conserva projeção histórica. (`app/chamados/services/disputa_emissao.py:1655`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1679`, `app/api/foto_recebida.py:48`) |
| Resposta pela trilha | Recebe instante da nota da trilha, possivelmente histórico; chamada pelo comando de cura. (`app/chamados/services/disputa_emissao.py:1685`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`) |
| Reabertura e recusa em lote | `timezone.now()` no início da transação, antes da inserção das linhas. (`app/chamados/services/disputa_emissao.py:1622`, `app/chamados/services/disputa_emissao.py:1632`) |
| Validação | `perg.validada_em`, atribuído a `timezone.now()` na operação; acompanha eventuais linhas históricas da projeção. (`app/chamados/services/validacao.py:167`, `app/chamados/services/validacao.py:168`, `app/chamados/services/validacao.py:173`) |
| Devolução normal e BUG-PAR | `timezone.now()` na montagem da linha; ambas podem inserir também conservação histórica. (`app/chamados/services/validacao.py:272`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:345`, `app/chamados/services/validacao.py:354`) |
| Silêncio no fechamento de disputa e no dia decidido | `materializada_em` do carimbo recém-feito pela porta; capturado antes da inserção. (`app/chamados/models.py:2010`, `app/chamados/models.py:2013`, `app/ponto/services/veredito_celula.py:190`, `app/ponto/services/veredito_celula.py:195`) |
| Retratação | `_agora_ret` da operação; inserção usa esse valor. (`app/chamados/models.py:2131`, `app/chamados/models.py:2145`) |
| Absorção | `agora` recebido pela operação e usado também no carimbo; a assinatura não garante que seja contemporâneo à inserção. (`app/chamados/services/materializacao.py:1046`, `app/chamados/services/materializacao.py:1048`) |
| Backfill de silêncio | Copia `p.materializada_em` histórico. O mesmo comando usa `linhas_da_projecao_sem_linha` para **medir** linhas candidatas; essa leitura não é, por si, inserção. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`) |
| Porta física | `registrar` recebe dicionários e faz `bulk_create`; não gera timestamp de inserção. O `save` do modelo também permite inserir uma instância sem pk, embora a busca estática por chamadas identificadas tenha encontrado a porta acima. Chamadas dinâmicas ou SQL externo: **[nao verificado]**. (`app/chamados/models.py:4111`, `app/chamados/models.py:4152`, `app/chamados/models.py:4162`) |
| Migração | A 0051 renomeia `RespostaAnterior` e seu instante para `DecisaoPergunta`/`ato_em`, preservando linhas antigas; sua função de dados mostrada atualiza PerguntaDisputa. Não é uma inserção nova de DP. O histórico completo de escritores anteriores à cópia: **[nao verificado]**. (`app/chamados/migrations/0051_decisao_pergunta.py:22`, `app/chamados/migrations/0051_decisao_pergunta.py:41`, `app/chamados/migrations/0051_decisao_pergunta.py:49`) |

O modelo declara `ato_em`, mas não declara coluna de instante de inserção; `registrar` também não registra uma trilha por DP que forneça esse instante. Eventos de operações sobre PerguntaDisputa existem, mas não constituem, pelo código citado, um registro universal e inequívoco da inserção de cada DP. Assim, **não é possível contar exatamente DP com `ato_em` anterior à inserção nem atribuí-las todas ao escritor**. (`app/chamados/models.py:4051`, `app/chamados/models.py:4077`, `app/chamados/models.py:4152`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/services/disputa_emissao.py:1704`)

O substituto disponível é **ordem por pk**, como aproximação da ordem de alocação; comparar `ato_em` de uma linha com o maior `ato_em` de pks menores detecta inversões na sequência, não a idade no INSERT. Concorrência, transações e importações podem separar ordem de pk, execução e commit; mesmo sem inversão, uma linha pode ter instante histórico. (`app/chamados/models.py:4081`, `app/chamados/models.py:4162`; garantias efetivas da sequência e do banco em produção **[nao verificado]**)

## (c) Candidatas e script de leitura

Para um colaborador \(c\), conjunto atual de linhas \(D_c\) e corte \(t\):

- **A — por instante do ato:** \(A(D_c,t)=\max\{d.ato\_em : d\in D_c,\ d.ato\_em\le t\}\), ou vazio. É a leitura por instante descrita no corte retirado. Ela **inclui hoje** uma inserção posterior com ato anterior ao corte: o esquema não permite recuperar a foto das linhas efetivamente presentes naquele instante. (`corte_retirado.txt:1`, `app/chamados/models.py:4053`)
- **B — sensível à inserção posterior:** conceitualmente, exige comparar o conjunto atual de IDs com o conjunto de IDs presentes na lavra, ou com um marco de inserção conservado nela. O material pergunta por essa alternativa, mas **não define sua função exata**; o esquema mostrado não guarda esse conjunto nem um instante de inserção por DP. Assim, uma função B executável e a contagem pedida para ela são **[nao verificado]**. (`F3.r`, seção 4, pergunta aberta; `app/chamados/models.py:4051`, `app/chamados/veredito_lavrado.py:237`)

Há também uma consequência matemática para a medida solicitada: com `ate=julgado_em`, o valor de A nunca supera `julgado_em`. Logo, acrescentado como `datetime` aos marcos reais, ele **não pode acionar o terceiro ramo de `esta_stale`**. Comparar o *valor do marco* com e sem linhas históricas ainda pode revelar diferenças; isso não é o mesmo que contar lavras tornadas stale. O relógio ou os marcos reais podem tornar uma lavra stale independentemente de A. (`app/chamados/veredito_lavrado.py:213`, `app/chamados/veredito_lavrado.py:224`, `app/chamados/veredito_lavrado.py:228`, `app/chamados/veredito_lavrado.py:230`)

O script abaixo é uma **sonda parcial honesta**, pois as contagens exatas por escritor e B não são recuperáveis desses campos. Execute seu conteúdo em `manage.py shell` na sombra; ele só consulta e imprime tuplas numéricas. Não foi executado aqui. (`app/chamados/models.py:4051`, `app/chamados/veredito_lavrado.py:151`)

Rótulos numéricos das saídas:

- `(1, ato, classe, n)`: universo completo de DP por ato e classe de motivo; a soma de `n` é o total de DP. Ato: 1 resposta, 2 validação, 3 devolução, 4 recusa, 5 reabertura, 6 silêncio, 7 retratação, 8 absorção, 9 outro. Classe: 1 projeção sem linha, 2 pela trilha, 3 resposta automática, 4 backfill E1 de silêncio, 5 demais. Os prefixos vêm dos escritores citados. (`app/chamados/models.py:4037`, `app/chamados/models.py:4119`, `app/chamados/services/disputa_emissao.py:1680`, `app/chamados/services/disputa_emissao.py:1702`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:327`)
- `(2, classe, n, inversoes)`: total de DP da classe e quantas têm `ato_em` menor que o maior `ato_em` observado em **qualquer DP de pk menor**. A soma de `n` é o total de DP; inversão é somente aproximação, não contagem de inserções retroativas por escritor. (`app/chamados/models.py:4053`, `app/chamados/models.py:4081`)
- `(3, universo, marcos_diferentes, stale_com, stale_sem, mudou_stale)`: lavras com veredito não vazio e `julgado_em`, comparando A com todas as DP contra A sem classes 1 e 4; ambos combinados aos marcos reais calculados em `ate=julgado_em`. `esta_stale` é avaliada no mesmo `agora` capturado para os dois lados. `marcos_diferentes` conta diferença de **marco combinado**, não necessariamente de veredito. (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:213`)
- `(4, total_dp, total_lavras)`: totais de controle. B e retroatividade real por escritor não recebem número substituto. (`app/chamados/models.py:4051`, `app/chamados/veredito_lavrado.py:237`)

```python
from collections import Counter
from django.db.models import Max
from django.utils import timezone

from chamados.models import DecisaoPergunta as DP, PerguntaDisputa as PD
from chamados.veredito_lavrado import marcos_do_lote, esta_stale

atos = {
    "resposta": 1,
    "validacao": 2,
    "devolucao": 3,
    "recusa": 4,
    "reabertura": 5,
    "silencio": 6,
    "retratacao": 7,
    "absorcao": 8,
}

def classe(motivo):
    motivo = motivo or ""
    if motivo.startswith("projecao sem linha"):
        return 1
    if motivo.startswith("pela trilha"):
        return 2
    if motivo.startswith("resposta automatica"):
        return 3
    if motivo.startswith("backfill E1 v5: admin_decidiu sem ato"):
        return 4
    return 5

universo = Counter()
classes = Counter()
inversoes = Counter()
maior_ato_anterior = None
total_dp = 0

linhas = (
    DP.objects.order_by("pk")
    .values_list("pk", "ato", "ato_em", "ato_motivo")
    .iterator(chunk_size=2000)
)
for pk, ato, instante, motivo in linhas:
    c = classe(motivo)
    universo[(atos.get(ato, 9), c)] += 1
    classes[c] += 1
    total_dp += 1
    if maior_ato_anterior is not None and instante < maior_ato_anterior:
        inversoes[c] += 1
    if maior_ato_anterior is None or instante > maior_ato_anterior:
        maior_ato_anterior = instante

for (ato, c), n in sorted(universo.items()):
    print((1, ato, c, n))

for c in range(1, 6):
    print((2, c, classes[c], inversoes[c]))

agora = timezone.now()
total_lavras = 0
marcos_diferentes = 0
stale_com = 0
stale_sem = 0
mudou_stale = 0

lavras = (
    PD.objects.exclude(veredito_lavrado="")
    .filter(veredito_lavrado__isnull=False, julgado_em__isnull=False)
    .select_related("disputa")
    .order_by("pk")
    .iterator(chunk_size=500)
)

for p in lavras:
    total_lavras += 1
    cid = p.disputa.colaborador_id
    corte = p.julgado_em

    reais = marcos_do_lote([cid], ate=corte)
    base = reais.get(cid)

    decisoes = DP.objects.filter(
        pergunta__disputa__colaborador_id=cid,
        ato_em__lte=corte,
    )
    com = decisoes.aggregate(m=Max("ato_em"))["m"]
    sem = (
        decisoes
        .exclude(ato_motivo__startswith="projecao sem linha")
        .exclude(ato_motivo__startswith="backfill E1 v5: admin_decidiu sem ato")
        .aggregate(m=Max("ato_em"))["m"]
    )

    def combinar(a, b):
        valores = [v for v in (a, b) if v is not None]
        return max(valores) if valores else None

    marco_com = combinar(base, com)
    marco_sem = combinar(base, sem)
    marcos_diferentes += int(marco_com != marco_sem)

    mapa_com = {} if marco_com is None else {cid: marco_com}
    mapa_sem = {} if marco_sem is None else {cid: marco_sem}
    s_com = esta_stale(p, mapa_com, agora)
    s_sem = esta_stale(p, mapa_sem, agora)

    stale_com += int(s_com)
    stale_sem += int(s_sem)
    mudou_stale += int(s_com != s_sem)

print((3, total_lavras, marcos_diferentes, stale_com, stale_sem, mudou_stale))
print((4, total_dp, total_lavras))
```

Pontos aproximados: as classes de motivo não identificam univocamente o escritor nem todas as possíveis linhas de backfill; exclusão das classes 1 e 4 é um recorte observável, não prova do conjunto completo de backfills. A ordem de pk é apenas proxy; a comparação dos marcos usa o estado atual das projeções e demais tabelas, não uma cópia histórica do banco. A consulta por lavra pode ser custosa na sombra. (`app/chamados/models.py:4119`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `app/chamados/veredito_lavrado.py:177`, `app/chamados/veredito_lavrado.py:202`)

**Não verificado**

- Identidade Git independente da cópia com `dd489c627`; os anexos físicos `logs/ESPINHA-BRIEFS/F3/` não estavam disponíveis, e as linhas dos anexos citadas são as do texto fornecido. **[nao verificado]**
- Dados e contagens atuais da sombra, execução do script, ordem efetiva de alocação/commit e existência de trilhas externas ou SQL fora da cópia. **[nao verificado]** [nao verificado]
- Definição operacional de B, snapshot de IDs na lavra e contagem exata de retroatividade por escritor: os dados fornecidos e os campos citados não bastam para calculá-los. **[nao verificado]**

## 18/11 10:28 · sessao c12f5ee63016 · E4C-F4 · verificador · origem=Code

**Veredito:** o delta corrige o filtro de carimbo cru no segundo laço e acrescenta à pauta as U60 não carimbadas que o contador classificou em L1/L5. A pauta ainda inclui toda U103 entregue ou sem dia, inclusive uma U103 já resolvida; isso não satisfaz literalmente “as 163 que a L1 barrar”. A comparação é uma análise do código por classe: os pks e estados efetivos de produção são **[não verificado]**. [app/chamados/management/commands/bug_par_103.py:54](app/chamados/management/commands/bug_par_103.py:54), [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328), [app/chamados/tests/test_bug_par_volta.py:622](app/chamados/tests/test_bug_par_volta.py:622)

A cópia não contém `.git`; a revisão anterior foi reconstruída pelo diff fornecido, e as linhas da revisão atual vêm dos arquivos abertos. A identidade efetiva da cópia com o hash informado é **[não verificado]**.

### 1. Escritores, leitores e significado

O delta altera a seleção e a montagem da pauta; não altera os escritores persistentes dos campos de pergunta. Abaixo, “escreve” identifica os produtores relevantes abertos, sem afirmar que sejam todos os escritores existentes.

| Dado ou função | Quem escreve ou produz | Quem lê no caminho examinado |
|---|---|---|
| `U60`, `U103` | O JSON contém listas de pks; `_carregar_universo_bug_par` valida hash, chaves, cardinalidade, inteiros, ausência de repetição e interseção, e cria `frozenset`s. [app/chamados/data/bug_par_163_2026_11_13.json:1](app/chamados/data/bug_par_163_2026_11_13.json:1), [app/chamados/services/universo_bug_par.py:12](app/chamados/services/universo_bug_par.py:12) | O comando seleciona `U103`; o contador seleciona `U163`. [app/chamados/management/commands/bug_par_103.py:50](app/chamados/management/commands/bug_par_103.py:50), [app/chamados/services/materializacao.py:323](app/chamados/services/materializacao.py:323) |
| `U163` | O carregador produz a união `U60 | U103`; não a lê como terceira chave do JSON. [app/chamados/services/universo_bug_par.py:25](app/chamados/services/universo_bug_par.py:25) | O comando exclui seus pks do segundo laço; o contador os inclui. Ambos usam pks, não objetos de pergunta. [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/services/materializacao.py:324](app/chamados/services/materializacao.py:324) |
| `bug_par_103_pauta_dp` | O contador produz lista de tuplas `(pk, 'MM/AAAA')`, tanto para U103 elegível carimbada entregue quanto para não carimbada com testemunha L1. [app/chamados/services/materializacao.py:329](app/chamados/services/materializacao.py:329), [app/chamados/services/materializacao.py:341](app/chamados/services/materializacao.py:341) | `_pauta_das_163` converte a lista em dict e usa o valor como rótulo. O formato coincide com `_competencia_falante`, mas o **significado da inclusão** não coincide sempre com o primeiro laço U103, que não consulta o estado da testemunha. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/management/commands/bug_par_103.py:143](app/chamados/management/commands/bug_par_103.py:143) |
| `bug_par_103_sem_dia` | O contador produz lista de pks, não pares nem objetos. [app/chamados/services/materializacao.py:333](app/chamados/services/materializacao.py:333), [app/chamados/services/materializacao.py:346](app/chamados/services/materializacao.py:346) | `_pauta_das_163` associa a cada pk a string literal `sem dia do fato`; se um pk aparecesse nas duas chaves, `update` daria precedência a esse rótulo. A partição do contador percorre cada pergunta uma vez e usa `continue`. [app/chamados/management/commands/bug_par_103.py:140](app/chamados/management/commands/bug_par_103.py:140), [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328) |
| `disputa__colaborador_id` | A pergunta tem FK `disputa`; o campo acessado é o id do colaborador da disputa. A identidade da pergunta também tem `colaborador`, cuja igualdade com o da disputa é descrita no modelo; a prova desse selo não foi aberta **[não verificado]**. [app/chamados/models.py:2529](app/chamados/models.py:2529), [app/chamados/models.py:2540](app/chamados/models.py:2540) | Os laços usam `p.disputa.colaborador_id`; o auxiliar obtém o mesmo id por `values_list`, não o objeto, e ambos formatam `col%s`. [app/chamados/management/commands/bug_par_103.py:64](app/chamados/management/commands/bug_par_103.py:64), [app/chamados/management/commands/bug_par_103.py:149](app/chamados/management/commands/bug_par_103.py:149) |
| `data_evento`, `motivo`, `tipo_resposta`, `respondida_em`, `resposta_colab` | O modelo declara data anulável, motivo e tipo textuais, texto e instante de resposta; os locais exatos de todos os escritores desses campos são **[não verificado]**. [app/chamados/models.py:2534](app/chamados/models.py:2534), [app/chamados/models.py:2555](app/chamados/models.py:2555) | O segundo laço usa a face SQL do juiz de resposta, motivos de intervalo e tipo `hora`; `competencia_do_fato` usa a data. Respondida requer instante **e** texto após trim. [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544), [app/chamados/services/materializacao.py:788](app/chamados/services/materializacao.py:788) |
| `materializada_em`, `materializada_por_id`, `via_resolucao`, `validada_em`, `validada_por_id`, `recusa_motivo` | `_escrever_carimbo` atribui os três campos do carimbo; as portas que o chamam persistem. `carimbar` também pode atribuir validação. Outros escritores de validação e recusa, exaustivamente: **[não verificado]**. [app/chamados/models.py:3216](app/chamados/models.py:3216), [app/chamados/models.py:3245](app/chamados/models.py:3245) | O contador distingue carimbadas e usa o predicado da devolução; o comando fotografa esses campos na reversão, sem usá-los para filtrar o segundo laço atual. [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328), [app/chamados/management/commands/bug_par_103.py:65](app/chamados/management/commands/bug_par_103.py:65) |
| `competencia_do_fato`, `estado_aplicacao_pergunta`, contador | A primeira compõe janela e juiz K6 e retorna `None` ou `(mês, ano, bool)`; a testemunha retorna dict com `estado`/`detalhe` ou `None`; o contador produz os baldes. [app/chamados/services/materializacao.py:788](app/chamados/services/materializacao.py:788), [app/chamados/services/materializacao.py:865](app/chamados/services/materializacao.py:865), [app/chamados/services/materializacao.py:364](app/chamados/services/materializacao.py:364) | Comando e contador leem `competencia_do_fato`; contador lê a testemunha; comando lê os baldes. K6 recebe **pk da empresa**, enquanto `competencia_do_fato` recebe o **objeto empresa**. [app/chamados/services/materializacao.py:798](app/chamados/services/materializacao.py:798), [app/chamados/management/commands/bug_par_103.py:81](app/chamados/management/commands/bug_par_103.py:81) |
| `pauta`, `ja`, `rot`, `col` e `_pauta_das_163` | Os laços e o auxiliar produzem tuplas `(pk, 'colNNN', rótulo)`; `ja` é conjunto de pks; `rot` mapeia pk a rótulo; `col` mapeia pk a id do colaborador. [app/chamados/management/commands/bug_par_103.py:57](app/chamados/management/commands/bug_par_103.py:57), [app/chamados/management/commands/bug_par_103.py:143](app/chamados/management/commands/bug_par_103.py:143) | O comando imprime `pauta` em JSON e volta a alterá-la após as portas em `--apply`. [app/chamados/management/commands/bug_par_103.py:90](app/chamados/management/commands/bug_par_103.py:90), [app/chamados/management/commands/bug_par_103.py:119](app/chamados/management/commands/bug_par_103.py:119) |

Os tipos e formatos dos dois produtores de rótulo são compatíveis; **a semântica da seleção não é única**: o primeiro laço pergunta K6 diretamente para toda U103, enquanto o auxiliar aceita a partição do contador, que distingue resolvidas, respondidas e estado de aplicação. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328)

### 2. Comparação dos conjuntos de pks

Defina **E** como competência própria entregue e **D** como ausência de `data_evento`. A coluna “antes” usa o segundo laço removido no diff fornecido: excluía U103 e excluía U163 carimbada. O primeiro laço U103 permaneceu. [app/chamados/management/commands/bug_par_103.py:54](app/chamados/management/commands/bug_par_103.py:54), [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73)

| Classe | `35039881d` | `5d6dd907f` e avaliação |
|---|---|---|
| **U103 carimbada**, E ou D | Lista pelo primeiro laço, qualquer que seja a via ou a resposta. | Igual. Para a candidata à devolução barrada, é coerente com a pauta específica dessa devolução; para uma já resolvida, é inclusão excedente frente à regra das bloqueadas. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:329](app/chamados/services/materializacao.py:329) |
| **U103 não carimbada**, E ou D | Lista pelo primeiro laço, mesmo sem resposta. | Igual. Se respondida e de tipo hora de intervalo, a testemunha alcança L1/L5; se não respondida, a testemunha retorna `None`, mas o primeiro laço ainda lista. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:882](app/chamados/services/materializacao.py:882) |
| **U60 carimbada**, E ou D | Não lista: o segundo laço a excluía pelo carimbo. | Não lista: contador a classifica como resolvida, pois o predicado da devolução exige U103. Para uma já gravada, as duas versões seguem a interpretação do teste de que ela não é barrada. [app/chamados/services/materializacao.py:250](app/chamados/services/materializacao.py:250), [app/chamados/services/materializacao.py:329](app/chamados/services/materializacao.py:329), [app/chamados/tests/test_bug_par_volta.py:622](app/chamados/tests/test_bug_par_volta.py:622) |
| **U60 não carimbada**, respondida, hora de intervalo, E ou D | Lista pelo segundo laço. | Lista pelo balde L1/L5 do contador. Mesmo conjunto nessas condições, agora sem filtro próprio de carimbo no comando. [app/chamados/services/materializacao.py:341](app/chamados/services/materializacao.py:341), [app/chamados/management/commands/bug_par_103.py:86](app/chamados/management/commands/bug_par_103.py:86) |
| **Não respondida**, não carimbada | U103 entra em E/D pelo primeiro laço; U60 e fora163 não entram no segundo. | Igual para esses casos: a testemunha não classifica a U60 não respondida como L1/L5. A inclusão incondicional da U103 é o excesso indicado acima. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:882](app/chamados/services/materializacao.py:882) |
| **Respondida e validada sem carimbo**, hora de intervalo | U103 entra em E/D; U60 entra pelo segundo laço em E/D. | Também entra: a testemunha consulta a guarda antes da derivação, independentemente de `validada_em`. A gravação é barrada em L1/L5. [app/chamados/services/materializacao.py:892](app/chamados/services/materializacao.py:892), [app/chamados/tests/test_bug_par_volta.py:578](app/chamados/tests/test_bug_par_volta.py:578) |
| **“Irmã” com competência diferente** | Cada linha do comando usa sua própria data. | Igual. A irmã que a porta reconhece exige mesma disputa **e mesma data**; portanto uma pergunta de outra competência não é essa irmã. A guarda normal dispensa a busca com base nessa identidade. [app/chamados/services/materializacao.py:139](app/chamados/services/materializacao.py:139), [app/chamados/services/validacao.py:219](app/chamados/services/validacao.py:219) |
| **Fora163**, respondida, hora de intervalo, E ou D | Lista, com ou sem carimbo. | Igual: o novo laço exclui U163 e consulta K6 da própria pergunta; D recebe rótulo explícito. [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/management/commands/bug_par_103.py:154](app/chamados/management/commands/bug_par_103.py:154) |

Para uma competência própria aberta e com data, nenhum dos laços diretos a lista; o auxiliar só a acrescenta se o contador a classificou L1/L5. A resposta sobre quais pks concretos caem em cada classe, e se há casos reais de excesso ou omissão, é **[não verificado]**. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:341](app/chamados/services/materializacao.py:341)

Na **primeira lista atual**, os conjuntos dos dois laços são disjuntos por U163, e o auxiliar exclui pks já presentes. Logo não há duplicata por construção sob leitura estável. O auxiliar, porém, faz nova consulta por pk e só retorna os encontrados: se uma pergunta desaparecer entre o contador e essa consulta, pode sumir sem rótulo; ocorrência real é **[não verificado]**. [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/management/commands/bug_par_103.py:143](app/chamados/management/commands/bug_par_103.py:143)

### 3. Recorte e `--apply`

Com `--empresa`, o queryset do comando filtra perguntas pela empresa da disputa; o contador recebe disputas filtradas pela mesma empresa e restringe suas perguntas a U163. Sem `--empresa`, o comando considera todas as perguntas e o contador todas as perguntas U163. Assim, **o recorte de empresa coincide para U163**, mas os universos completos não são iguais: a lista do comando também contempla fora163, enquanto o contador só conta U163. [app/chamados/management/commands/bug_par_103.py:45](app/chamados/management/commands/bug_par_103.py:45), [app/chamados/management/commands/bug_par_103.py:73](app/chamados/management/commands/bug_par_103.py:73), [app/chamados/services/materializacao.py:323](app/chamados/services/materializacao.py:323)

Em `--apply`, a segunda lista **não é necessariamente coerente por pk com a primeira**. Após uma recusa da porta, o comando relê K6 e acrescenta a tupla se a **tupla inteira** não estiver na pauta. Se o mesmo pk já estiver com outro rótulo e a leitura posterior produzir um rótulo diferente, pode aparecer duas vezes; a lista também preserva rótulos anteriores, sem reclassificação geral. Mudanças concorrentes e duplicatas efetivas em produção: **[não verificado]**. [app/chamados/management/commands/bug_par_103.py:110](app/chamados/management/commands/bug_par_103.py:110), [app/chamados/management/commands/bug_par_103.py:119](app/chamados/management/commands/bug_par_103.py:119)

### 4. Contratos e mutante

O predicado `materializada_em__isnull` removido do segundo laço deixa de ser um ofensor AST naquele ponto. O comando continua fazendo uma classificação própria de **inclusão na pauta U103** por `exp or k6 is None`; consulta o juiz de competência via composição, mas não consulta a testemunha de aplicação para decidir se a U103 foi barrada. O contador ainda possui um ramo próprio por carimbo; esse ramo e a testemunha estão na allowlist do contrato de vitalidade. Isso não demonstra, por si só, que julguem *vitalidade*, mas impede afirmar que todos os leitores delegam qualquer decisão envolvendo carimbo. [app/chamados/management/commands/bug_par_103.py:58](app/chamados/management/commands/bug_par_103.py:58), [app/chamados/services/materializacao.py:328](app/chamados/services/materializacao.py:328), [app/chamados/tests/test_contract_leitores_vitalidade.py:107](app/chamados/tests/test_contract_leitores_vitalidade.py:107)

O contrato aberto procura formas AST específicas de comparação com `None`, `__isnull` e teste direto dos dois campos, com allowlist por função; passar nele não prova ausência de todo predicado equivalente ou de decisão própria de competência. Resultado da suíte nesta cópia: **[não verificado]**. [app/chamados/tests/test_contract_leitores_vitalidade.py:313](app/chamados/tests/test_contract_leitores_vitalidade.py:313)

O alvo textual novo do mutante aparece uma vez na fonte aberta. Por raciocínio, ele **morde o teste alvo**: a fixture põe as duas perguntas somente em U60/U163; com a chamada ao auxiliar substituída por `pauta.extend([])`, o primeiro laço U103 e o segundo laço fora163 ficam vazios, mas o teste exige os dois pks. O executor também exige ocorrência textual única e classifica o mutante como integração; execução e resultado real: **[não verificado]**. [app/chamados/tests/bug_par_mutantes.py:116](app/chamados/tests/bug_par_mutantes.py:116), [app/chamados/tests/test_bug_par_volta.py:501](app/chamados/tests/test_bug_par_volta.py:501), [app/chamados/tests/test_bug_par_volta.py:611](app/chamados/tests/test_bug_par_volta.py:611), [app/chamados/tests/bug_par_mutantes.py:213](app/chamados/tests/bug_par_mutantes.py:213)

Há uma lacuna no teste de “já gravada não entra”: ele usa a mesma fixture U60 e, por isso, não exercita a U103 já resolvida que o primeiro laço ainda lista. Além disso, o teste alvo verifica L5 no contador após remover a data, mas não chama novamente o comando para conferir a pauta L5. [app/chamados/tests/test_bug_par_volta.py:501](app/chamados/tests/test_bug_par_volta.py:501), [app/chamados/tests/test_bug_par_volta.py:616](app/chamados/tests/test_bug_par_volta.py:616), [app/chamados/tests/test_bug_par_volta.py:622](app/chamados/tests/test_bug_par_volta.py:622)

## 18/11 10:27 · sessao eeb00102cb29 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
---- DADO RASPADO (sonda/anexos) ----
=== SOMBRA (sombra_db, leitor) 18/11 10:25:11 ===
sonda_leitura: sonda_i8.py [4cdf8da477f4472e] no cpuset 4-7, lendo sombra como leitor (LEITURA)

=== col198 dia 09/11
 celula vespera 08/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     09/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': None, 'hii': None}
 batidas apuraveis D-2..D+1: [('07/11 18:57E', 'app'), ('08/11 06:57S', 'app'), ('08/11 18:57E', 'app'), ('09/11 06:58S', 'app')]
  turno data_turno=2026-11-08 E=08/11 18:57E S=09/11 06:58S aberto=False cross=True nbat=2
=== col366 dia 09/11
 celula vespera 08/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     09/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('08/11 19:00E', 'app'), ('09/11 02:00S', 'app'), ('09/11 03:00E', 'app'), ('09/11 07:00S', 'app')]
  turno data_turno=2026-11-08 E=08/11 19:00E S=09/11 07:00S aberto=False cross=True nbat=4
=== col366 dia 05/11
 celula vespera 04/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     05/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('03/11 02:00S', 'app'), ('03/11 03:00E', 'app'), ('03/11 07:00S', 'app'), ('04/11 19:00E', 'app'), ('05/11 02:00S', 'app'), ('05/11 03:00E', 'app'), ('05/11 07:00S', 'app')]
  turno data_turno=2026-11-04 E=04/11 19:00E S=05/11 07:00S aberto=False cross=True nbat=4
=== col366 dia 03/11
 celula vespera 02/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     03/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('02/11 19:00E', 'app'), ('03/11 02:00S', 'app'), ('03/11 03:00E', 'app'), ('03/11 07:00S', 'app'), ('04/11 19:00E', 'app')]
  turno data_turno=2026-11-02 E=02/11 19:00E S=03/11 07:00S aberto=False cross=True nbat=4
  turno data_turno=2026-11-04 E=04/11 19:00E S=05/11 07:00S aberto=False cross=True nbat=4
=== col366 dia 30/10
 celula vespera 29/10: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     30/10: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('28/10 02:00S', 'app'), ('28/10 03:00E', 'app'), ('28/10 07:00S', 'app'), ('29/10 19:00E', 'app'), ('30/10 02:00S', 'app'), ('30/10 03:00E', 'app'), ('30/10 07:00S', 'app')]
  turno data_turno=2026-10-29 E=29/10 19:00E S=30/10 07:00S aberto=False cross=True nbat=4
=== col366 dia 28/10
 celula vespera 27/10: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     28/10: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
 batidas apuraveis D-2..D+1: [('27/10 19:00E', 'app'), ('28/10 02:00S', 'app'), ('28/10 03:00E', 'app'), ('28/10 07:00S', 'app'), ('29/10 19:00E', 'app')]
  turno data_turno=2026-10-27 E=27/10 19:00E S=28/10 07:00S aberto=False cross=True nbat=4
  turno data_turno=2026-10-29 E=29/10 19:00E S=30/10 07:00S aberto=False cross=True nbat=4
=== col489 dia 10/11
 celula vespera 09/11: trabalha=False origem=gerada regen=2 marcos=None
 celula dia     10/11: trabalha=True origem=gerada regen=2 marcos={'hf': '00:00', 'hi': '16:00', 'hfi': '21:00', 'hii': '20:00'}
 batidas apuraveis D-2..D+1: [('08/11 14:59E', 'app'), ('08/11 19:00S', 'app'), ('08/11 20:00E', 'app'), ('08/11 23:00S', 'app'), ('09/11 16:01E', 'app'), ('09/11 20:00S', 'app'), ('09/11 21:00E', 'app'), ('10/11 00:00S', 'app'), ('11/11 19:00E', 'app'), ('11/11 23:00S', 'app')]
  turno data_turno=2026-11-09 E=09/11 16:01E S=10/11 00:00S aberto=False cross=True nbat=4
  turno data_turno=2026-11-11 E=11/11 19:00E S=12/11 07:00S aberto=False cross=True nbat=4
=== col566 dia 17/11
 celula vespera 16/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     17/11: trabalha=True origem=gerada regen=0 marcos={'hf': '06:00', 'hi': '18:00', 'hfi': None, 'hii': None}
 batidas apuraveis D-2..D+1: [('15/11 06:13S', 'app'), ('16/11 18:09E', 'app'), ('17/11 06:32S', 'app')]
  turno data_turno=2026-11-16 E=16/11 18:09E S=17/11 06:32S aberto=False cross=True nbat=2
=== col868 dia 17/11
 celula vespera 16/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     17/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '02:00', 'hii': '01:00'}
 batidas apuraveis D-2..D+1: [('15/11 07:03S', 'app'), ('16/11 19:16E', 'app'), ('17/11 03:39S', 'app'), ('17/11 04:35E', 'app'), ('17/11 06:49S', 'app')]
  turno data_turno=2026-11-16 E=16/11 19:16E S=17/11 06:49S aberto=False cross=True nbat=4
=== col1017 dia 17/11
 celula vespera 16/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     17/11: trabalha=True origem=gerada regen=0 marcos={'hf': '06:00', 'hi': '18:00', 'hfi': None, 'hii': None}
 batidas apuraveis D-2..D+1: [('15/11 05:58S', 'app'), ('16/11 17:59E', 'app'), ('17/11 05:57S', 'app')]
  turno data_turno=2026-11-16 E=16/11 17:59E S=17/11 05:57S aberto=False cross=True nbat=2
=== col1031 dia 17/11
 celula vespera 16/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     17/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '00:00', 'hii': '23:00'}
 batidas apuraveis D-2..D+1: [('15/11 00:01E', 'app'), ('15/11 06:55S', 'app'), ('16/11 18:56E', 'app'), ('17/11 06:55S', 'app')]
  turno data_turno=2026-11-16 E=16/11 18:56E S=17/11 06:55S aberto=False cross=True nbat=2
=== col1031 dia 15/11
 celula vespera 14/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     15/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '00:00', 'hii': '23:00'}
 batidas apuraveis D-2..D+1: [('13/11 06:52S', 'app'), ('13/11 19:02E', 'app'), ('13/11 23:00S', 'app'), ('14/11 00:03E', 'app'), ('14/11 18:54E', 'app'), ('14/11 23:00S', 'app'), ('15/11 00:01E', 'app'), ('15/11 06:55S', 'app'), ('16/11 18:56E', 'app')]
  turno data_turno=2026-11-14 E=14/11 18:54E S=15/11 06:55S aberto=False cross=True nbat=4
  turno data_turno=2026-11-16 E=16/11 18:56E S=17/11 06:55S aberto=False cross=True nbat=2
=== col1031 dia 11/11
 celula vespera 10/11: trabalha=False origem=gerada regen=0 marcos=None
 celula dia     11/11: trabalha=True origem=gerada regen=0 marcos={'hf': '07:00', 'hi': '19:00', 'hfi': '00:00', 'hii': '23:00'}
 batidas apuraveis D-2..D+1: [('10/11 18:55E', 'app'), ('10/11 23:01S', 'app'), ('11/11 00:07E', 'app'), ('11/11 06:52S', 'app'), ('12/11 18:56E', 'app')]
  turno data_turno=2026-11-10 E=10/11 18:55E S=11/11 06:52S aberto=False cross=True nbat=4
  turno data_turno=2026-11-12 E=12/11 18:56E S=13/11 06:52S aberto=False cross=True nbat=2
rc_sombra=0
---- FIM DO DADO ----
```

linhas copiadas: 71.

Na tabela, as células e os turnos são transcritos do **DADO RASPADO** acima [não verificado em arquivo]. `T` e `F` representam `trabalha=True` e `False`; os marcos estão na ordem `hi/hii/hfi/hf`. As datas dos turnos são de 2026. O parecer “não” compara a célula que contém o marco `hf` com a `data_turno` impressa: não afirma que o cadastro esteja correto nem que um consumidor tenha julgado a saída de determinada forma. O código associa as batidas contidas em um turno à sua `data_turno` em [app/ponto/turnos.py:2957](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/turnos.py:2957).

A sonda imprime as pontas `E` e `S` e `nbat`, mas não imprime a lista `turno.batidas`. Portanto, para batidas intermediárias, a pertença individual está marcada **[não verificado]**: falta essa lista por turno. A saída impressa como `S` está identificada diretamente.

| ID | Dia | Célula da véspera | Célula do dia | Turnos montados: data, E, S, aberto | Batidas da madrugada do dia → data_turno | Concordam sobre a saída? |
|---|---|---|---|---|---|---|
| col198 | 09/11 | F; None | T; 19:00/None/None/07:00 | 08/11, 08/11 18:57, 09/11 06:58, False | 06:58S → 08/11 | Não: `hf` na célula 09/11; `data_turno=08/11`. |
| col366 | 09/11 | F; None | T; 19:00/02:00/03:00/07:00 | 08/11, 08/11 19:00, 09/11 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 08/11 | Não: `hf` na célula 09/11; `data_turno=08/11`. |
| col366 | 05/11 | F; None | T; 19:00/02:00/03:00/07:00 | 04/11, 04/11 19:00, 05/11 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 04/11 | Não: `hf` na célula 05/11; `data_turno=04/11`. |
| col366 | 03/11 | F; None | T; 19:00/02:00/03:00/07:00 | 02/11, 02/11 19:00, 03/11 07:00, False; **04/11**, 04/11 19:00, 05/11 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 02/11 | Não: `hf` na célula 03/11; `data_turno=02/11`. |
| col366 | 30/10 | F; None | T; 19:00/02:00/03:00/07:00 | 29/10, 29/10 19:00, 30/10 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 29/10 | Não: `hf` na célula 30/10; `data_turno=29/10`. |
| col366 | 28/10 | F; None | T; 19:00/02:00/03:00/07:00 | 27/10, 27/10 19:00, 28/10 07:00, False; **29/10**, 29/10 19:00, 30/10 07:00, False | 02:00S e 03:00E → [não verificado]; 07:00S → 27/10 | Não: `hf` na célula 28/10; `data_turno=27/10`. |
| col489 | 10/11 | F; None | T; 16:00/20:00/21:00/00:00 | 09/11, 09/11 16:01, 10/11 00:00, False; **11/11**, 11/11 19:00, 12/11 07:00, False | 00:00S → 09/11 | Não: `hf` na célula 10/11; `data_turno=09/11`. |
| col566 | 17/11 | F; None | T; 18:00/None/None/06:00 | 16/11, 16/11 18:09, 17/11 06:32, False | 06:32S → 16/11 | Não: `hf` na célula 17/11; `data_turno=16/11`. |
| col868 | 17/11 | F; None | T; 19:00/01:00/02:00/07:00 | 16/11, 16/11 19:16, 17/11 06:49, False | 03:39S e 04:35E → [não verificado]; 06:49S → 16/11 | Não: `hf` na célula 17/11; `data_turno=16/11`. |
| col1017 | 17/11 | F; None | T; 18:00/None/None/06:00 | 16/11, 16/11 17:59, 17/11 05:57, False | 05:57S → 16/11 | Não: `hf` na célula 17/11; `data_turno=16/11`. |
| col1031 | 17/11 | F; None | T; 19:00/23:00/00:00/07:00 | 16/11, 16/11 18:56, 17/11 06:55, False | 06:55S → 16/11 | Não: `hf` na célula 17/11; `data_turno=16/11`. |
| col1031 | 15/11 | F; None | T; 19:00/23:00/00:00/07:00 | 14/11, 14/11 18:54, 15/11 06:55, False; **16/11**, 16/11 18:56, 17/11 06:55, False | 00:01E → [não verificado]; 06:55S → 14/11 | Não: `hf` na célula 15/11; `data_turno=14/11`. |
| col1031 | 11/11 | F; None | T; 19:00/23:00/00:00/07:00 | 10/11, 10/11 18:55, 11/11 06:52, False; **12/11**, 12/11 18:56, 13/11 06:52, False | 00:07E → [não verificado]; 06:52S → 10/11 | Não: `hf` na célula 11/11; `data_turno=10/11`. |

O mesmo limite vale para atribuir uma **obra causal** a cada um dos 13 dias: [não verificado]. O dado mostra a disposição das células e o resultado do pareador; faltam as leituras ou decisões dos demais componentes, e, quando pertinente, o histórico de lavratura e rejulgamento. As linhas abaixo distinguem o que cada obra registrada cobre do que a sonda permite concluir.

| Obra registrada | Trecho do backlog que cobre seu mecanismo | Aplicação aos 13 dias |
|---|---|---|
| MOTOR-LE-O-TURNO | “motor e DiaPago leem par e dia SO de core/juizes.py::turnos_do_colab.” [app/docs/BACKLOG.md:2043](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2043) | [não verificado] em todos: faltam o par e o dia efetivamente usados pelo motor e pelo DiaPago. |
| DIA-DA-BATIDA-JUIZ-UNICO | “JUIZO PARALELO (migra para data_turno do juiz do par)” e “RED turno 19-07 + DIFF da 10 por migracao”. [app/docs/BACKLOG.md:1846](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1846) | [não verificado] em todos: falta identificar um juízo consumidor que use o dia civil da batida e sua decisão. A divergência de campos, por si só, não demonstra esse juízo. |
| DIA-SEM-TRABALHO-PORTA | “batida em folga do calendario e em feriado sem trabalha_em_feriado -> porta de decisao do admin”. [app/docs/BACKLOG.md:1968](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1968) | A véspera aparece com `trabalha=False` em todos [não verificado em arquivo]. A aplicação da porta é [não verificado]: faltam a classificação completa do dia e a decisão ou passagem pela porta. |
| CELULA-CONCORDE-NO-MEIO-DO-TURNO | “celula concorde no meio do turno e rejulgada no fim”. [app/docs/BACKLOG.md:1821](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1821) | [não verificado] em todos: faltam estado da célula, horários de lavratura e eventual rejulgamento. |

Nos totais seguintes, “mecanismo” significa **padrão observado de célula e turno**, sem atribuição causal de obra [não verificado em arquivo]:

| Padrão observado | Dias | Contagem |
|---|---|---:|
| Véspera F/None; dia T sem marcos de intervalo; turno da véspera fechado com 2 batidas | col198 09/11; col566 17/11; col1017 17/11 | 3 |
| Véspera F/None; dia T com marcos de intervalo; turno da véspera fechado com 4 batidas | col366 09/11, 05/11, 03/11, 30/10 e 28/10; col489 10/11; col868 17/11; col1031 15/11 e 11/11 | 9 |
| Véspera F/None; dia T com marcos de intervalo; turno da véspera fechado com 2 batidas | col1031 17/11 | 1 |
| **Total** | | **13** |

Por obra causal comprovada: **0 dias atribuíveis com o dado fornecido; 13 com atribuição [não verificado]**. Isso não é contagem de dias que as obras necessariamente corrigiriam: faltam as evidências indicadas na tabela por obra. [app/docs/BACKLOG.md:1846](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1846), [app/docs/BACKLOG.md:1968](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1968), [app/docs/BACKLOG.md:2043](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2043), [app/docs/BACKLOG.md:1821](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1821).

## 18/11 10:26 · sessao d9f6a5ad5bea · DST-L2 · sonda · origem=Code

```text
furo_vivos 3002
com_dia 3002 com_celula 2998 trabalha_false(ja_fechava_no_base) 11
dia_sem_trabalho_so_pela_ata_gravada(DST-L2_alarga) 35
  destes_com_justif_pendente_chamado_origem 0 com_ponteiro_pendente 0
```

## 18/11 10:25 · sessao cd955b9d3c25 · RESPOSTAS-DE-LEI-1811-1010 · verificador · origem=Code

**Resultado: item 1 devolvido; item 3 não devolvido pelos achados verificáveis.** No item 1, falta informação na saída do casador para executar literalmente o recorte pedido por órfã e comprovar o RED do dia vizinho. No item 3, a solução é possível na porta existente, mas ainda exige alteração do universo e da escrita. [app/escala/utils.py:621](app/escala/utils.py:621) · [app/chamados/models.py:1546](app/chamados/models.py:1546) · [app/chamados/models.py:1613](app/chamados/models.py:1613)

Os anexos raspados foram fornecidos sem numeração própria de linhas; afirmações que dependem exclusivamente deles estão marcadas **[não verificado]**. A identidade Git do HEAD não pôde ser conferida: esta cópia não apresentou metadados Git. **[não verificado]**

| Item | Pergunta | Veredito e prova |
|---|---|---|
| **1** | **1. Executável no literal e na ordem?** | **PROCEDE.** `_match_marcos` devolve células e órfãs como listas separadas; cada órfã é apenas `HH:MM`, sem data civil, identidade da batida ou associação ao marco `missing`. A grade reúne as listas por dia, mas também acrescenta sobras pela data civil e pode acrescentar órfãs por falta de coluna. Isso permite testar se **o dia** tem algum `missing`, mas não prova, por órfã, a relação com um marco nem distingue todos os casos necessários ao RED “dia vizinho não entra” sem informação adicional. [app/escala/utils.py:615](app/escala/utils.py:615) · [app/escala/utils.py:662](app/escala/utils.py:662) · [app/escala/utils.py:1715](app/escala/utils.py:1715) · [app/escala/utils.py:1807](app/escala/utils.py:1807) · [app/escala/utils.py:1813](app/escala/utils.py:1813) |
| **1** | **2. Falta “senão”?** | **PROCEDE.** Para a situação identificada acima — a saída não distinguir a órfã necessária ao recorte — o complemento não determina o que o vigia deve fazer; contá-la ou descartá-la escolheria uma regra que o dado não fornece. **[não verificado]** A classificação CADASTRO, por sua vez, é expressável pelo juiz existente e pode alimentar o RED de não alarmar. [app/ponto/management/commands/e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98) |
| **1** | **3. Colisão com lei, aval ou obra?** | **NÃO PROCEDE**, quanto às colisões demonstráveis nos materiais disponíveis: abrir por dono e reservar o alarme a ESTRUTURA acompanha a L-099; o complemento posterior declara substituir o anterior onde colidir. [app/docs/LEIS.md:122](app/docs/LEIS.md:122) · **[não verificado: precedência entre anexos raspados]** Compatibilidade com toda obra em voo fora do material fornecido: **[não verificado]**. |
| **1** | **4. Referências, unidade e universo corretos?** | **PROCEDE.** `utils.py:631` monta tipos das batidas candidatas, e `:673` é documentação; as emissões efetivas de horários estão em `:621` e `:662`. A unidade do complemento é **órfã/batida**, enquanto os 106 fornecidos são **dias com órfã**: não se pode comparar as contagens como se fossem a mesma unidade. [app/escala/utils.py:631](app/escala/utils.py:631) · [app/escala/utils.py:673](app/escala/utils.py:673) · [app/escala/utils.py:621](app/escala/utils.py:621) · **[não verificado: números e unidades dos anexos raspados]** A justificativa sobre `perto_do_marco` está correta: a distância usa minuto do dia e é circular, descartando a data. [app/escala/regua_defesa.py:412](app/escala/regua_defesa.py:412) · [app/escala/regua_defesa.py:442](app/escala/regua_defesa.py:442) |
| **3** | **1. Executável no literal e na ordem?** | **NÃO PROCEDE**, como impossibilidade: é possível alterar a porta existente para incluir o estado hoje excluído e registrar a foto marcada antes do lote zerar o par, sob a transação e a trava existentes. Isso **não está implementado hoje**: o filtro pula texto sem instante e sem validação, e `linhas_da_projecao_sem_linha` não o fotografa. A restrição do modelo permite a marca apenas em `REABERTURA`. [app/chamados/models.py:1541](app/chamados/models.py:1541) · [app/chamados/models.py:1613](app/chamados/models.py:1613) · [app/chamados/models.py:1636](app/chamados/models.py:1636) · [app/chamados/models.py:4094](app/chamados/models.py:4094) · [app/chamados/models.py:4132](app/chamados/models.py:4132) |
| **3** | **2. Falta “senão”?** | **NÃO PROCEDE**, para a ordem de gravação: a porta retorna sem escrever quando não há IDs e o registro e a limpeza estão no mesmo `atomic`; falha da operação não autoriza concluir que o par foi limpo com sucesso. [app/chamados/models.py:1599](app/chamados/models.py:1599) · [app/chamados/models.py:1613](app/chamados/models.py:1613) · [app/chamados/models.py:1637](app/chamados/models.py:1637) Tratamento operacional específico de uma falha futura: **[não verificado]**. |
| **3** | **3. Colisão com lei, aval ou obra?** | **NÃO PROCEDE**, nas fontes disponíveis: a L-130 exige limpar instante e texto e conservar o texto na trilha; a marca já foi prevista no expand, e a carga já a ignora antes de escolher o último ato. [app/docs/LEIS.md:151](app/docs/LEIS.md:151) · [app/chamados/models.py:4065](app/chamados/models.py:4065) · [app/chamados/services/tarefa_do_dia.py:421](app/chamados/services/tarefa_do_dia.py:421) O estado e a coordenação efetiva da obra P2 em outra cópia são **[não verificado]**. |
| **3** | **4. Referências, unidade e universo corretos?** | **NÃO PROCEDE**, com a ressalva de implementação: `models.py:1541`, `:1619`, `:4069` e `:4132` sustentam, respectivamente, a exclusão, a fotografia atual antes da limpeza, a marca prevista e a limitação da fotografia. A unidade é pergunta. A porta atual **ainda pode criar** linhas `RESPOSTA`/`VALIDACAO` de projeção; cumprir “sem RESPOSTA sintética” exige tratar essa chamada ao construir, e não presumir que ela já satisfaz o aval. [app/chamados/models.py:1541](app/chamados/models.py:1541) · [app/chamados/models.py:1619](app/chamados/models.py:1619) · [app/chamados/models.py:4069](app/chamados/models.py:4069) · [app/chamados/models.py:4139](app/chamados/models.py:4139) |

**Quem escreve e quem lê — item 1.** O casador escreve em memória as células `missing` e as listas de horários; o builder agrupa essas listas pelo dia de entrada e acrescenta sobras pelo dia civil. O classificador lê `orfas` e verifica se **alguma célula do dia** está `missing`; não recebe uma ligação órfã→marco. O juiz de dono lê insumos do dia e devolve ESTRUTURA, CADASTRO ou BATIDA. O vigia proposto seria leitor dessas saídas; seu código e seu placar nesta obra são **[não verificado]**. [app/escala/utils.py:615](app/escala/utils.py:615) · [app/escala/utils.py:1434](app/escala/utils.py:1434) · [app/escala/utils.py:1813](app/escala/utils.py:1813) · [app/escala/orfa_classe.py:35](app/escala/orfa_classe.py:35) · [app/ponto/management/commands/e6_oraculo.py:53](app/ponto/management/commands/e6_oraculo.py:53) · [app/docs/JUIZES.md:27](app/docs/JUIZES.md:27)

**Quem escreve e quem lê — item 3.**

- **`ato` e `ato_texto`:** `DecisaoPergunta.registrar` é a porta de escrita; entre os chamadores atuais, `reabrir_para_correcao` fornece a reabertura e o texto anterior, e a fotografia da projeção fornece respostas ou validações. O escritor proposto seria a ressuscitação dentro da porta existente. [app/chamados/models.py:4151](app/chamados/models.py:4151) · [app/chamados/services/disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632) · [app/chamados/models.py:4139](app/chamados/models.py:4139)
- **Leitores:** a tarefa lê `ato` e lê `ato_texto` para distinguir resposta vazia; o histórico seleciona atos com foto e o modal mostra `ato_texto`. Assim, a marca protege a seleção de tarefa, mas o histórico também verá a reabertura marcada como ato com foto. [app/chamados/services/tarefa_do_dia.py:158](app/chamados/services/tarefa_do_dia.py:158) · [app/chamados/models.py:3680](app/chamados/models.py:3680) · [app/templates/chamados/partials/modal_fio.html:210](app/templates/chamados/partials/modal_fio.html:210)
- **`da_ressuscitacao`:** o modelo declara o escritor P2 futuro e o predicado que lê a marca; a carga da tarefa usa esse predicado para excluir o ato **antes** da escolha do último. Uma reabertura comum passa e é lida como devolução ao colaborador. [app/chamados/models.py:4065](app/chamados/models.py:4065) · [app/chamados/models.py:4102](app/chamados/models.py:4102) · [app/chamados/services/tarefa_do_dia.py:421](app/chamados/services/tarefa_do_dia.py:421) · [app/chamados/services/tarefa_do_dia.py:163](app/chamados/services/tarefa_do_dia.py:163) O juiz de resposta exige instante e texto não vazio. [app/docs/JUIZES.md:102](app/docs/JUIZES.md:102)

**Item 1 — AVAL-DEVOLVIDO: sim**: perguntas 1, 2 e 4 procedem.

**Item 3 — AVAL-DEVOLVIDO: não**: nenhuma das quatro perguntas procedeu nos limites verificados; a implementação ainda precisa cumprir as ressalvas acima.

## 18/11 10:24 · sessao 8841839b2e3b · DST-L2 · sonda · origem=Code

Transcrição literal das linhas numéricas fornecidas [não verificado]:

```text
pendentes_90d 68
pendente_com_ponteiro_de_chamado_encerrado 38
pendente_com_chamado_origem_encerrado 38
chamados_encerrados_universo 38 sem_followup_sistema 3
justif_orfas_com_celula 38 dia_sem_trabalho_pelo_juiz 4
```

Não há traceback no trecho fornecido [não verificado].

**Limite da análise:** o conteúdo de `sonda_justif_orfa.py` não foi disponibilizado no texto e não o localizei na cópia; portanto, não é possível verificar os filtros, o universo, as exclusões ou a forma da consulta de cada contador [não verificado]. A cópia também não contém metadados Git acessíveis para confirmar o hash informado [não verificado].

Pelos **nomes impressos**, as linhas parecem tratar dos seguintes conjuntos; a definição exata de cada um continua [não verificada]:

- `pendentes_90d`: pendentes em uma janela de 90 dias. Qual data define a janela e seus limites: [não verificado].
- `pendente_com_ponteiro_de_chamado_encerrado` e `pendente_com_chamado_origem_encerrado`: pendentes associadas a chamado encerrado por dois critérios. O modelo possui tanto `Justificativa.chamado_origem` quanto `ChamadoColaborador.justificativa_relacionada`, que são relações distintas; não é possível confirmar qual consulta produz o primeiro contador nem que os dois grupos de 38 sejam os mesmos. ([ponto/models.py:173](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), [chamados/models.py:69](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:69)) [não verificado].
- `chamados_encerrados_universo` e `sem_followup_sistema`: chamados do universo da sonda e os classificados sem follow-up de sistema. Como esse universo é formado e o que a consulta considera ausência de follow-up: [não verificado]. O modelo permite follow-up com `autor_tipo='sistema'`. ([chamados/models.py:1733](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1733))
- `justif_orfas_com_celula` e `dia_sem_trabalho_pelo_juiz`: justificativas classificadas pela sonda como órfãs e com célula, e casos classificados pelo juiz como dia sem trabalho. Como a sonda define “órfã”, encontra a célula e trata célula ausente: [não verificado]. O juiz retorna verdadeiro para `trabalha is False` ou para `ata.tipo_dia` presente e diferente de `trabalho`; numa ausência parcial, retorna apenas `trabalha is False`. ([chamados/juizes.py:432](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:432))

Quanto à possível distorção, `Justificativa`, `ChamadoColaborador` e `FollowupChamado` têm ordenações padrão, mas **não é possível dizer se elas inflam algum contador sem ver as consultas**. ([ponto/models.py:182](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:182), [chamados/models.py:103](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:103), [chamados/models.py:1765](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1765)) O uso e o efeito de `Q` com subconsulta, de `distinct`, e de células ausentes também permanecem [não verificados]. O código admite chamado sem célula, e o próprio supra-juiz distingue `sem_celula` de um juízo de folga. ([chamados/models.py:48](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:48), [ponto/supra_juiz.py:51](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/supra_juiz.py:51))

Para concluir a análise dos seis contadores e de suas possíveis distorções, preciso do conteúdo de `sonda_justif_orfa.py` mencionado no envelope [não verificado].

## 18/11 10:24 · sessao 443a793a78f8 · DST-L2 · leitura · origem=Code

O fechamento antecipado acontece durante o `create()` da justificativa, **antes de `_justif` gravar o ponteiro**. A alteração DST-L2 faz o cartório considerar a ata de ausência ao decidir se o chamado protesta; o fechamento direto pelo cartório passa ao largo da unidade S128, que é onde estão o Fix A e o followup de retratação ([teste](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:41), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1457), [S128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563)). Os resultados do diagnóstico informados na pergunta não foram reproduzidos nesta leitura: **[não verificado]**.

### 1. Sequência e diferença para a base

1. `_justif` cria uma `Justificativa` pendente; somente depois atribui `ch.justificativa_relacionada` e salva o chamado ([teste](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:41)).
2. O `post_save` da criação chama `_julgar_dias`, que chama `cartorio.julgar_celula(colaborador, data_turno)` ([signals de ponto](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161), [signals de ponto](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:85)). `julgar_celula` chama `julgar_colab` para a célula encontrada ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:2019)).
3. A ata do julgamento é construída antes de `_protesta`; no commit analisado, este pergunta a `dia_sem_trabalho` usando `trabalha` **e a ata atual** ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1435), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1462)). O juiz considera dia sem trabalho uma ata com `tipo_dia` preenchido diferente de `trabalho`, ressalvada a ausência parcial ([juiz](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:445)).
4. Para os chamados que não protestam, o cartório chama `transicionar_com_pauta(..., ato=x.reconciliar)` imediatamente em **`_porm`, linha 1546**, ou os adia quando há fio vivo e os fecha na **linha 1011**, depois da lavra e do E2 ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1000)). A fixture afirma que a célula absolve a pergunta, mas o dado fornecido não identifica qual das duas chamadas executou no diagnóstico; **linha efetivamente executada: [não verificado]** ([fixture](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/tests/_celula_absolve.py:2), [juiz do fio](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:1510)).
5. `reconciliar` passa pelo gate e chama `_gravar_estado`; é este último que atribui `self.status_local = novo` ([modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:642), [modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:228)). O `post_save` do status tenta resolver perguntas pelo receiver S143, em modo *best effort* ([signals de chamados](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:91)).

Na base `dd489c627`, `_protesta` só exclui o dia quando `cel.trabalha is False` ou os marcos estão acesos; não consulta o tipo de dia da ata ([cartório da base](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1458)). No cenário indicado, a mudança relevante é que o dia previsto de trabalho coberto por ausência passa a não protestar pela segunda cláusula do juiz ([teste](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:96), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1457)). A execução exata dos gates na base é **[não verificado]**.

Depois do fechamento, o `save` do ponteiro não muda `status_local`; o receiver do cartório retorna quando o status não mudou, e o S143 também retorna quando o save não escreveu o status ([signals de chamados](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:189), [signals de chamados](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:91)). A grade coleta apenas chamados em `OPEN`, por isso um já resolvido deixa de ser candidato à retratação por essa coleta ([reconciliar_grade](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/reconciliar_grade.py:199)).

### 2. Todos os sítios diretos do cartório

Todos os seis sítios abaixo chamam `reconciliar` pelo mesmo wrapper; se a transição efetivamente grava o status, o S143 **tenta** resolver perguntas, mas pode falhar sem desfazer o fechamento. Nenhum deles chama diretamente a unidade S128 ou seu Fix A ([modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:642), [S143](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/signals.py:91), [S128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563)).

| Sítio e condição | Followup nesse ramo | Fix A | Resolver de perguntas |
|---|---|---|---|
| Adiados após lavra e E2 ([linha 1011](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011)) | Não | Não | S143; E2 também é chamado antes ([linha 1006](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1006)) |
| `_porm`, não protestam e não adiados ([linha 1546](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1546)) | Não | Não | S143 |
| Marcos inexistentes, `_velhos` ([linha 1559](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1559)) | **Sim**, `marco_inexistente` ([linha 1563](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1563)) | Não | S143 |
| Cobrança em dia sem trabalho ou sem furo, `_rec` ([linha 1580](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1580)) | Não | Não | S143 |
| Fio composto de perguntas duplicatas, `x` ([linha 1659](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1659)) | Não | Não | S143 |
| Deduplicação dos outros chamados, `y` ([linha 1674](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1674)) | Não | Não | S143 |

“Não” na coluna de followup refere-se à chamada de `anexar_ocorrencia` no ramo do cartório; efeitos indiretos específicos de outras rotinas ou dados do chamado são **[não verificado]**.

### 3. Escritores, leitores e ordem do ponteiro

O escritor de produção do ponteiro é `vinculos.apontar_justificativa`: ele só escreve se o ponteiro estiver vazio e salva os campos do vínculo ([vínculos](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:81)). Os três chamadores encontrados são:

- API: busca e salva uma pendente existente ou cria uma nova, **antes** de apontar ([API](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:1988)).
- View de ponto: também salva a existente ou cria a nova, **antes** de apontar ([view](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1621)).
- `processar_alertas_turno`: faz `get_or_create` e aponta quando `created` é verdadeiro, portanto depois da criação ([comando](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:392)).

Em todos, o `save()` ou a criação da justificativa pode disparar o julgamento do dia **entre** a escrita da justificativa e a do ponteiro, sujeito às guardas do cartório ([signal](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:2019)).

Leitores de produção encontrados para o ponteiro são: a guarda de transição humana e o coletor de premissa no modelo ([modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:135), [modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1313)); a retenção e o Fix A do reconciliador ([reconciliador](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:440), [reconciliador](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573)); e os dois fluxos de batida que podem rejeitar a pendente ([triagem](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:598), [entrada sincronizada](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:29)).

Por retratação, o Fix A escreve `rejeitada`, acrescenta a descrição e salva ([reconciliador](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:569)). Os dois fluxos de batida também escrevem esse status após uma transição bem-sucedida ([triagem](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:600), [entrada sincronizada](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:32)). Há rejeição humana individual e em lote ([serviço](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:269), [view](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1863)); a fila lê o status para filtrar rejeitadas ([view](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1707)) e a API o apresenta ([API](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:2106)).

### 4. Ponteiro escrito depois do fechamento

**Não encontrei um caminho automático de reparo desse estado no código examinado.** A porta do ponteiro só grava o vínculo; a S128 executa o Fix A apenas depois de uma transição aceita, enquanto `reconciliar` declara no-op para chamado já encerrado; os fluxos de batida também exigem fechamento bem-sucedido antes de rejeitar ([vínculos](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:81), [S128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563), [modelo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:642), [triagem](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:598), [entrada sincronizada](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:29)). Uma rejeição posterior por decisão humana é possível ([serviço](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:269)). Existência de outro caminho não localizado ou execução em produção: **[não verificado]**.

### 5. Contratos e conflito

A S128 declara autoridade única de retratação: transição, Fix A, resolução de perguntas e ocorrência compõem o ato cercado; o teste JZ1 exige chamado encerrado, perguntas resolvidas, disputa fechada, followup de sistema e justificativa rejeitada ([reconciliador](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:522), [ato](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563), [selo](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:50)).

O cartório declara que orquestra o juiz e a lavra, e que a ata lavrada prevalece para seus leitores ([docstring](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:7)). A L-061 registra que o consumidor lê a lâmpada ([LEIS.md](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/LEIS.md:95)). O O261 descreve a ordem **lavrar → E2 → reconciliar** para o lote adiado com fio, enquanto o código fecha imediatamente o sem fio ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1528), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540)). O corte DST-L2 determina que o cartório pergunte a `dia_sem_trabalho` ([CORTES.md](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/CORTES.md:533)).

**Há conflito na implementação observada, não uma impossibilidade lógica entre as ordens:** o cartório usa `reconciliar` diretamente para quem não protesta, inclusive após E2, e assim não executa os demais componentes obrigatórios da S128; além disso, quando a justificativa nasce depois do fechamento, o Fix A não tinha ponteiro para ler no instante do fechamento ([cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1546), [ato S128](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563), [teste](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:41)). O registro antigo que diz que `_protesta` ainda usa só a primeira cláusula está desatualizado perante o corte DST-L2 e o código desta cópia ([CORTES.md](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/CORTES.md:448), [CORTES.md](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/CORTES.md:533), [cartório](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1464)).

## 18/11 10:16 · sessao fec0b6249760 · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

**O censo ainda está ABERTO nos quatro itens.** A saída recebida permitiu confirmar escritores, leitores e caminhos que faltavam, mas não sustenta uma lista exaustiva nem a classificação individual de todos os acessos dinâmicos. **[nao verificado: completude do censo]**

Há duas correções relevantes para o construtor:

- A escrita de `validada_em` não se limita ao ato de validar: há limpeza pela resposta humana, devoluções e preenchimentos associados ao carimbo. A distinção é entre o dono do **ato de validar** e todos os escritores do campo. [app/chamados/services/validacao.py:1] [app/chamados/models.py:3249] [app/chamados/services/disputa_emissao.py:1494]
- A carga remove as decisões marcadas como ressuscitação antes de escolher o último ato, mas `respostas_anteriores` percorre atos com fotografia sem esse filtro. Um novo ato marcado precisa ter os dois leitores avaliados. [app/chamados/services/tarefa_do_dia.py:421] [app/chamados/models.py:3688]

As listas abaixo registram os sítios confirmados nesta volta; **não são a lista completa pedida**. **[nao verificado: sítios restantes]**

### 1. Escritores e leitores R/V/C e porta `registrar` — STATUS ABERTO

**Escrita da projeção:**

- Resposta humana e invalidação da validação anterior: [app/chamados/services/disputa_emissao.py:1481].
- Resposta automática e carimbo pela trilha: [app/chamados/services/disputa_emissao.py:1675] [app/chamados/services/disputa_emissao.py:1698].
- Validação e devoluções: [app/chamados/services/validacao.py:168] [app/chamados/services/validacao.py:273] [app/chamados/services/validacao.py:349].
- Carimbo com preenchimento condicional de resposta e validação: [app/chamados/models.py:3249].
- Escrita em lote dinâmica por `**outros`: [app/chamados/models.py:3371] [app/chamados/models.py:3404]. Os chamadores confirmados limpam R/V/C na ressuscitação, R/V/C na reabertura e o par de resposta na devolução L-130: [app/chamados/models.py:1637] [app/chamados/services/disputa_emissao.py:1610] [app/chamados/services/disputa_emissao.py:1649] [app/chamados/services/validacao.py:440].
- A entrada por foto delega à resposta automática e à validação; o cabeçalho antigo do módulo que a descreve como escrita local não corresponde ao corpo atual. [app/api/foto_recebida.py:2] [app/api/foto_recebida.py:39]

**Escrita da sequência pela porta `DecisaoPergunta.registrar`:**

- Modelo e portas de resposta/reabertura: [app/chamados/models.py:1619] [app/chamados/models.py:2013] [app/chamados/models.py:2145] [app/chamados/models.py:3137] [app/chamados/services/disputa_emissao.py:1525] [app/chamados/services/disputa_emissao.py:1632] [app/chamados/services/disputa_emissao.py:1679] [app/chamados/services/disputa_emissao.py:1701].
- Validação, devoluções e absorção: [app/chamados/services/validacao.py:173] [app/chamados/services/validacao.py:279] [app/chamados/services/validacao.py:354] [app/chamados/services/validacao.py:439] [app/chamados/services/materializacao.py:1048].
- Cartório e comandos de acervo: [app/ponto/services/veredito_celula.py:195] [app/chamados/management/commands/backfill_decisoes_pergunta.py:324] [app/chamados/management/commands/backfill_fio_morto_e4b.py:141].
- Falso positivo: `registrar` em [app/chamados/views.py:91] é da recusa crítica, não a porta de `DecisaoPergunta`. [app/chamados/views.py:90]

**Leituras confirmadas que o desenho precisa considerar:**

- Juízes, faces SQL e ordenação da fila: [app/chamados/juizes.py:352] [app/chamados/juizes.py:856] [app/chamados/juizes.py:1389] [app/chamados/juizes.py:1534] [app/chamados/juizes.py:1552] [app/chamados/juizes.py:1572] [app/chamados/juizes.py:1580] [app/chamados/juizes.py:1761].
- Estado e apresentação da pergunta; histórico de decisões: [app/chamados/models.py:3159] [app/chamados/models.py:3681] [app/chamados/models.py:3701].
- Fio e materialização: [app/chamados/services/fio.py:188] [app/chamados/services/fio.py:263] [app/chamados/services/materializacao.py:546] [app/chamados/services/materializacao.py:1153] [app/chamados/services/materializacao.py:2091].
- API e apresentação: [app/api/views.py:1163] [app/api/views.py:2561] [app/api/views.py:2639] [app/api/views_mensageria.py:567] [app/chamados/templatetags/chamados_tags.py:139] [app/chamados/templatetags/chamados_tags.py:166].
- Histórico cronológico lê dinamicamente os três instantes e o texto; a reversão do passivo também fotografa campos da pergunta: [app/chamados/services/historico.py:96] [app/chamados/services/passivo_validado.py:64] [app/chamados/services/passivo_validado.py:83].
- Auditoria, placares e procedência têm leituras próprias: [app/chamados/management/commands/auditar_invariantes_chamados.py:1077] [app/chamados/management/commands/auditar_invariantes_chamados.py:1107] [app/chamados/services/painel.py:261] [app/chamados/services/verbos.py:263] [app/ponto/services/procedencia_batida.py:71].

**Por que permanece aberto:** faltam a classificação de todos os leitores listados no inventário, o rastreamento completo dos chamadores e a resolução individual dos acessos genéricos que podem receber objetos ou nomes em execução. **[nao verificado]**

### 2. Importadores, selo e medida da sombra — STATUS ABERTO

Importadores confirmados: `relavrar_via_i6` importa `universo_i6`; `dedup_perguntas_fato` importa `aplicar` e `CONTADORES_DO_APPLY`; `worklist` importa `_parse_pendentes` e `MODULOS_ALVO`, e tem consumidores na view e no comando. [app/chamados/management/commands/relavrar_via_i6.py:46] [app/chamados/management/commands/dedup_perguntas_fato.py:105] [app/ponto/worklist.py:37] [app/colaboradores/views.py:3061] [app/ponto/management/commands/worklist_dp.py:12]

O selo global de vitalidade declarado examina `materializada_em` e `validada_em` e formas sintáticas específicas; ele não demonstra, sozinho, cobertura de todos os campos ou acessos dinâmicos. [app/chamados/tests/test_contract_leitores_vitalidade.py:14] [app/chamados/tests/test_contract_leitores_vitalidade.py:329] O selo específico lista os quatro escritores esperados de `resposta_mantem_validacao`. [app/chamados/tests/test_f2_resposta_mantem_validacao.py:19]

A medida raspada da **sombra** informa:

| Projeção atual nula com ato histórico correspondente | Último físico / efetivo | Total |
|---|---|---:|
| Resposta | reabertura / reabertura | 52 |
| Resposta | recusa / recusa | 7 |
| Validação | devolução / devolução | 60 |
| Validação | reabertura / reabertura | 9 |
| Validação | resposta / resposta | 3 |

Esses valores são os fornecidos no anexo da conversa; produção e interseção dos grupos são **[nao verificado]**. A função confirmada mede o sentido inverso, projeção sem linha correspondente por ato e instante; os números acima não demonstram erros nem uma correspondência geral ato → projeção. [app/chamados/models.py:4122] [app/chamados/services/disputa_emissao.py:1632]

A exclusividade dos importadores e a classificação completa dos selos permanecem **[nao verificado]**.

### 3. Campos novos e leituras de REABERTURA — STATUS ABERTO

- `da_ressuscitacao` e `resposta_mantem_validacao` têm defaults e restrições por tipo de ato. [app/chamados/models.py:4071] [app/chamados/models.py:4077] [app/chamados/models.py:4095] [app/chamados/models.py:4098]
- A marca de ressuscitação é lida pelo predicado em [app/chamados/models.py:4103] e pela carga em [app/chamados/services/tarefa_do_dia.py:421]. A porta de ressuscitação nesta cópia registra a projeção faltante e limpa os campos; não se pode presumir que já grave a nova marca. [app/chamados/models.py:1619] [app/chamados/models.py:1637]
- A reabertura fotografa resposta, instantes e autor antes de limpar a projeção; `respostas_anteriores` é leitor da sequência e a carga do fio busca também o autor original. [app/chamados/services/disputa_emissao.py:1632] [app/chamados/models.py:3688] [app/chamados/services/fio.py:263]
- Os valores declarados para `resposta_mantem_validacao` são `True` na fotografia da projeção, resposta automática e carimbo pela trilha; `False` na resposta humana. [app/chamados/models.py:4141] [app/chamados/services/disputa_emissao.py:1525] [app/chamados/services/disputa_emissao.py:1679] [app/chamados/services/disputa_emissao.py:1701]

**Contradição a resolver no desenho:** o comentário do modelo associa a regra atual ao `ato_motivo`, mas `_pela_decisao` aqui trata toda última `RESPOSTA` com texto como `aguarda_validacao`, sem consultar esse motivo ou `resposta_mantem_validacao`. [app/chamados/models.py:4072] [app/chamados/services/tarefa_do_dia.py:158] Não está demonstrado que trocar esse leitor pela nova marca preserve o comportamento atual. **[nao verificado]**

A lista completa de leitores indiretos e o efeito de todos os caminhos de `registrar` seguem **[nao verificado]**.

### 4. Cascas, templates e PDF — STATUS ABERTO

- `base_tpl` é escolhido pelo contexto entre `base_app.html` e `base.html`; `meu_atendimento.html` usa essa escolha e inclui o cartão nas duas abas. [app/core/context_processors.py:194] [app/core/context_processors.py:205] [app/templates/chamados/meu_atendimento.html:1] [app/templates/chamados/meu_atendimento.html:127] [app/templates/chamados/meu_atendimento.html:144]
- O cartão do colaborador lê resposta e instante. [app/templates/chamados/partials/_card_atendimento.html:106] [app/templates/chamados/partials/_card_atendimento.html:143] [app/templates/chamados/partials/_card_atendimento.html:183] [app/templates/chamados/partials/_card_atendimento.html:391]
- A casca administrativa `detalhe_local` estende `base.html` e inclui `_pergunta_card`, que também pode ser renderizado isoladamente por HTMX. [app/templates/chamados/detalhe_local.html:1] [app/templates/chamados/detalhe_local.html:310] [app/chamados/views.py:398]
- `modal_fio` lê histórico, resposta e validação e também é renderizado isoladamente; portanto o grafo que parte apenas das bases não o enumera como alcançado. [app/templates/chamados/partials/modal_fio.html:205] [app/templates/chamados/partials/modal_fio.html:219] [app/templates/chamados/partials/modal_fio.html:228] [app/chamados/views.py:927] [app/chamados/views.py:1399]
- A prévia de lote lê a resposta e é renderizada pela view de cobrança. [app/templates/chamados/partials/_lote_preview.html:24] [app/chamados/views_cobrar.py:360]
- No caminho de PDF inspecionado, a view alimenta o gerador de histórico e o bloco examinado lê followups. Relações indiretas adicionais entre PDF e R/V/C permanecem **[nao verificado]**. [app/relatorios/views.py:871] [app/relatorios/pdf_chamados.py:135]

### Classificação dos dinâmicos examinados

- **Escrita real de R/V/C:** `models.py:3404`, por `**outros`, com os três chamadores identificados no item 1. [app/chamados/models.py:3379] [app/chamados/models.py:3404]
- **Leitura real:** `historico.py:105`; `passivo_validado.py:83`; e `_PD` em `veredito_lavrado.py:202` e `:206` é alias de `PerguntaDisputa`, com filtros sobre materialização e resposta. [app/chamados/services/historico.py:96] [app/chamados/services/passivo_validado.py:64] [app/chamados/veredito_lavrado.py:163] [app/chamados/veredito_lavrado.py:202]
- **Não é acesso R/V/C nesses sítios:** os dinâmicos de `ChamadoColaborador` em `models.py:637`, `:953`, `:976`, `:1018` e `:1142`; os filtros de chave de família em `fio.py:2110`, `:2112`, `:2122`, `:2134`, `:2164`, `:2166` e `:2187`; e os filtros dinâmicos por identidade nas duas portas de validação. [app/chamados/models.py:637] [app/chamados/models.py:953] [app/chamados/models.py:976] [app/chamados/models.py:1018] [app/chamados/models.py:1142] [app/chamados/services/fio.py:1515] [app/chamados/services/fio.py:2110] [app/chamados/services/fio.py:2112] [app/chamados/services/fio.py:2122] [app/chamados/services/fio.py:2134] [app/chamados/services/fio.py:2164] [app/chamados/services/fio.py:2166] [app/chamados/services/fio.py:2187] [app/chamados/services/validacao.py:673] [app/chamados/services/validacao.py:890]
- **Ainda indeterminados:** os demais candidatos genéricos do inventário, inclusive o alcance completo dos wrappers de `carga_pergunta_viva` e dos resolvedores genéricos de `core`, não foram classificados um a um. [app/chamados/services/carga_pergunta_viva.py:30] [app/chamados/services/carga_pergunta_viva.py:59] [app/core/intake.py:487] [app/core/reexibir.py:49] **[nao verificado]**

Para terminar a classificação dos **selos** sem acesso a logs, seria necessária a parte da saída bruta filtrada referente a `tests/`; para o censo de produção ainda falta completar a leitura dos candidatos e caminhos indiretos acima. **[nao verificado]**

alcance nao verificado restante: 4

## 18/11 10:15 · sessao 6e3da0f0e6be · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

O censo **ainda não autoriza abrir o construtor**: os itens 1, 3 e 5 permanecem abertos. O inventário amplia os sítios conhecidos, mas não demonstra que todos os acessos dinâmicos, aliases indiretos, carregamentos de templates e leitores foram classificados. [não verificado]

Há uma contradição concreta com a formulação da volta 1 sobre Q4: o card de pergunta exibe “validada” diretamente quando `validada_em` existe, mas o “Validada” do modal está dentro do ramo `perg.respondida` e depois do ramo prioritário `pendencia_admin`. Assim, validação projetada sem linha de decisão, por si só, **não demonstra** que o modal mostrará “Validada”; isso também depende do par de resposta e da ausência de pendência prioritária. [app/templates/chamados/partials/_pergunta_card.html:23](app/templates/chamados/partials/_pergunta_card.html:23) [app/templates/chamados/partials/modal_fio.html:217](app/templates/chamados/partials/modal_fio.html:217) [app/templates/chamados/partials/modal_fio.html:222](app/templates/chamados/partials/modal_fio.html:222) [app/templates/chamados/partials/modal_fio.html:228](app/templates/chamados/partials/modal_fio.html:228)

### 1. Cascas do card — STATUS: ABERTO

Os caminhos de produção identificados para a F7 são:

- **Card do colaborador, leitura:** `meu_atendimento` inclui `_card_atendimento` nos dois loops; a casca pode ser `base.html` ou `base_app.html`, conforme `base_tpl`. O card lê o par de resposta e a fase. [app/templates/chamados/meu_atendimento.html:1](app/templates/chamados/meu_atendimento.html:1) [app/templates/chamados/meu_atendimento.html:127](app/templates/chamados/meu_atendimento.html:127) [app/templates/chamados/meu_atendimento.html:144](app/templates/chamados/meu_atendimento.html:144) [app/core/context_processors.py:210](app/core/context_processors.py:210) [app/templates/chamados/partials/_card_atendimento.html:106](app/templates/chamados/partials/_card_atendimento.html:106) [app/templates/chamados/partials/_card_atendimento.html:189](app/templates/chamados/partials/_card_atendimento.html:189)
- **Card da pergunta, leitura:** a página de detalhe usa `base.html` e inclui `_pergunta_card`; o rerender parcial também chega ao mesmo template por `_render_card`, alias de `render`. [app/templates/chamados/detalhe_local.html:1](app/templates/chamados/detalhe_local.html:1) [app/templates/chamados/detalhe_local.html:310](app/templates/chamados/detalhe_local.html:310) [app/chamados/views.py:397](app/chamados/views.py:397) [app/chamados/views.py:404](app/chamados/views.py:404)
- **Modal do fio, leitura:** é um fragmento renderizado diretamente, sem casca própria; uma hospedeira comprovada é o painel em `base.html`, que o busca pela rota. Há também chamada da fila de benefícios com `modo=beneficio`; a lista integral de hospedeiras e suas cascas permanece [não verificado]. [app/chamados/views.py:927](app/chamados/views.py:927) [app/chamados/views.py:1400](app/chamados/views.py:1400) [app/chamados/services/fio.py:543](app/chamados/services/fio.py:543) [app/templates/chamados/painel_gestao.html:1](app/templates/chamados/painel_gestao.html:1) [app/templates/chamados/painel_gestao.html:528](app/templates/chamados/painel_gestao.html:528) [app/templates/colaboradores/fila_solicitacoes_beneficio.html:119](app/templates/colaboradores/fila_solicitacoes_beneficio.html:119)
- **Prévia de lote, leitura:** é fragmento renderizado diretamente; o painel em `base.html` abre-o no drawer. Outras hospedeiras não foram excluídas. [app/templates/chamados/painel_gestao.html:14](app/templates/chamados/painel_gestao.html:14) [app/chamados/views_cobrar.py:360](app/chamados/views_cobrar.py:360) [app/chamados/views_cobrar.py:364](app/chamados/views_cobrar.py:364) [app/chamados/views_cobrar.py:380](app/chamados/views_cobrar.py:380) [app/chamados/views_cobrar.py:388](app/chamados/views_cobrar.py:388) [app/templates/chamados/partials/_lote_preview.html:24](app/templates/chamados/partials/_lote_preview.html:24)

O grafo fornecido identifica includes compartilhados pelas duas bases, mas não prova, sozinho, todas as hospedeiras dos fragmentos nem renderização e cliques em execução. Essa revisão transitiva continua aberta. [app/templates/base.html:93](app/templates/base.html:93) [app/templates/base_app.html:85](app/templates/base_app.html:85) [app/templates/base.html:399](app/templates/base.html:399) [app/templates/base_app.html:154](app/templates/base_app.html:154) [não verificado]

### 2. Escritor de reabertura — STATUS: FECHADO para a dúvida específica

`reabrir_para_correcao` prepara a limpeza do par e da validação, registra uma decisão com a fotografia anterior e chama `descarimbar_em_lote`; o corpo dessa função não chama `linhas_da_projecao_sem_linha`. Isso fecha a dúvida sobre **essa chamada**, sem fechar o censo de escritores. [app/chamados/services/disputa_emissao.py:1577](app/chamados/services/disputa_emissao.py:1577) [app/chamados/services/disputa_emissao.py:1610](app/chamados/services/disputa_emissao.py:1610) [app/chamados/services/disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632) [app/chamados/services/disputa_emissao.py:1649](app/chamados/services/disputa_emissao.py:1649)

### 3. Leitores cruzados e escritores — STATUS: ABERTO

A lista seguinte é de **sítios comprovados a considerar**, não uma lista integral nem uma determinação de que todos devam ser alterados pela F7. O brief completo e o efeito exato pretendido em cada consumidor não estão demonstrados no material fornecido. [não verificado]

- **Escrita persistida:** resposta e validação em `disputa_emissao`; validações em `validacao`; carimbo com possível preenchimento de resposta e validação em `models`; e limpeza em lote, inclusive campos passados por `**outros`. O último ponto é relevante: a porta verifica expressamente se `outros` contém campos do par e os passa ao `update`. [app/chamados/services/disputa_emissao.py:1481](app/chamados/services/disputa_emissao.py:1481) [app/chamados/services/disputa_emissao.py:1494](app/chamados/services/disputa_emissao.py:1494) [app/chamados/services/disputa_emissao.py:1675](app/chamados/services/disputa_emissao.py:1675) [app/chamados/services/disputa_emissao.py:1698](app/chamados/services/disputa_emissao.py:1698) [app/chamados/services/validacao.py:168](app/chamados/services/validacao.py:168) [app/chamados/services/validacao.py:273](app/chamados/services/validacao.py:273) [app/chamados/services/validacao.py:349](app/chamados/services/validacao.py:349) [app/chamados/models.py:3247](app/chamados/models.py:3247) [app/chamados/models.py:3251](app/chamados/models.py:3251) [app/chamados/models.py:3254](app/chamados/models.py:3254) [app/chamados/models.py:3371](app/chamados/models.py:3371) [app/chamados/models.py:3404](app/chamados/models.py:3404)
- **Atribuições que não são, isoladamente, escrita persistida:** as de `models.py:1561–1563` alteram uma cópia usada no julgamento; as de `models.py:3282–3284` restauram a instância em memória após recusa. Classificá-las simplesmente como escritoras de banco seria incorreto. [app/chamados/models.py:1548](app/chamados/models.py:1548) [app/chamados/models.py:1561](app/chamados/models.py:1561) [app/chamados/models.py:3278](app/chamados/models.py:3278)
- **Juízes e fase, leitura:** projeção temporal, juiz do par, propriedade `respondida` e `estado` são pontos centrais; `estado` dá precedência à validação projetada. [app/chamados/juizes.py:1382](app/chamados/juizes.py:1382) [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544) [app/chamados/models.py:3152](app/chamados/models.py:3152) [app/chamados/models.py:3674](app/chamados/models.py:3674)
- **Interfaces e API, leitura:** além dos quatro templates do item 1, há apresentação de resposta por filtros e saídas da API e mensageria. [app/chamados/templatetags/chamados_tags.py:139](app/chamados/templatetags/chamados_tags.py:139) [app/chamados/templatetags/chamados_tags.py:166](app/chamados/templatetags/chamados_tags.py:166) [app/api/views.py:1163](app/api/views.py:1163) [app/api/views.py:2561](app/api/views.py:2561) [app/api/views_mensageria.py:567](app/api/views_mensageria.py:567)
- **Outros consumidores de produção, leitura:** materialização, reconciliação, histórico, painel, folha e cartório têm leituras diretas ou via juiz que precisam ser avaliadas conforme a mudança final. [app/chamados/services/materializacao.py:191](app/chamados/services/materializacao.py:191) [app/chamados/services/materializacao.py:2091](app/chamados/services/materializacao.py:2091) [app/chamados/reconciliador.py:365](app/chamados/reconciliador.py:365) [app/chamados/services/historico.py:98](app/chamados/services/historico.py:98) [app/chamados/services/painel.py:270](app/chamados/services/painel.py:270) [app/folha/previa.py:141](app/folha/previa.py:141) [app/ponto/services/cartorio.py:1641](app/ponto/services/cartorio.py:1641)
- **Auditoria e reconstrução, leitura e eventual escrita de decisões:** os comandos consultam as projeções, e o backfill chama a construção de linhas ausentes. Seu comportamento completo e a necessidade de mudança pela F7 permanecem [não verificado]. [app/chamados/management/commands/auditar_invariantes_chamados.py:1077](app/chamados/management/commands/auditar_invariantes_chamados.py:1077) [app/chamados/management/commands/backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py:56) [app/chamados/management/commands/backfill_decisoes_pergunta.py:67](app/chamados/management/commands/backfill_decisoes_pergunta.py:67)

**Indeterminados examinados:**

- `models.py:637–638`: acesso dinâmico aos campos de urgência de `ChamadoColaborador`, não uma escrita da projeção de `PerguntaDisputa`. [app/chamados/models.py:620](app/chamados/models.py:620) [app/chamados/models.py:637](app/chamados/models.py:637)
- `models.py:953`, `:976`, `:1018`, `:1142`: filtros e criação da porta de `ChamadoColaborador`; não são, nesses sítios, escritas de `PerguntaDisputa`. [app/chamados/models.py:953](app/chamados/models.py:953) [app/chamados/models.py:976](app/chamados/models.py:976) [app/chamados/models.py:1018](app/chamados/models.py:1018) [app/chamados/models.py:1142](app/chamados/models.py:1142)
- `models.py:3404`: **escrita relevante**, inclusive dos campos dinâmicos `outros`, além dos carimbos explícitos. [app/chamados/models.py:3371](app/chamados/models.py:3371) [app/chamados/models.py:3404](app/chamados/models.py:3404)
- `historico.py:105`: **leitura relevante**; o laço percorre `respondida_em`, `validada_em` e `materializada_em`, e também lê o texto. [app/chamados/services/historico.py:98](app/chamados/services/historico.py:98) [app/chamados/services/historico.py:105](app/chamados/services/historico.py:105) [app/chamados/services/historico.py:109](app/chamados/services/historico.py:109)
- `passivo_validado.py:83`: **leitura relevante** para fotografia de reversão; a lista inclui materialização, resposta, validação e autor da validação. [app/chamados/services/passivo_validado.py:64](app/chamados/services/passivo_validado.py:64) [app/chamados/services/passivo_validado.py:83](app/chamados/services/passivo_validado.py:83)
- `validacao.py:677` e `:894`: filtros dinâmicos por `pk` e, opcionalmente, disputa; selecionam a pergunta, mas essas chaves não são campos da projeção. Os corpos seguintes ainda são consumidores relevantes. [app/chamados/services/validacao.py:673](app/chamados/services/validacao.py:673) [app/chamados/services/validacao.py:677](app/chamados/services/validacao.py:677) [app/chamados/services/validacao.py:890](app/chamados/services/validacao.py:890) [app/chamados/services/validacao.py:894](app/chamados/services/validacao.py:894)
- `veredito_lavrado.py:202` e `:206`: **leituras relevantes** dos instantes de materialização e resposta, com limite temporal dinâmico; `:177` e `:180` consultam `Batida`. [app/chamados/veredito_lavrado.py:163](app/chamados/veredito_lavrado.py:163) [app/chamados/veredito_lavrado.py:177](app/chamados/veredito_lavrado.py:177) [app/chamados/veredito_lavrado.py:180](app/chamados/veredito_lavrado.py:180) [app/chamados/veredito_lavrado.py:202](app/chamados/veredito_lavrado.py:202) [app/chamados/veredito_lavrado.py:206](app/chamados/veredito_lavrado.py:206)
- `fio.py:2112`, `:2122`, `:2134`, `:2164`, `:2166`, `:2187`: consultas dinâmicas às chaves de família e aos campos de exclusão do índice; os nomes são definidos nas tabelas do próprio arquivo. A classificação de seu impacto indireto completo continua [não verificado]. [app/chamados/services/fio.py:1515](app/chamados/services/fio.py:1515) [app/chamados/services/fio.py:1521](app/chamados/services/fio.py:1521) [app/chamados/services/fio.py:2109](app/chamados/services/fio.py:2109) [app/chamados/services/fio.py:2164](app/chamados/services/fio.py:2164)
- `explicador.py:43`: `dict.update` em resultado de apresentação, não `QuerySet.update` nem escrita de modelo. [app/chamados/services/explicador.py:41](app/chamados/services/explicador.py:41)
- `pdf_espelho.py:856`: o `getattr` apontado lê campos de rubricas dos turnos; não é, nesse ponto, leitura direta da projeção da pergunta. Outros caminhos indiretos até PDF permanecem [não verificado]. [app/relatorios/pdf_espelho.py:844](app/relatorios/pdf_espelho.py:844) [app/relatorios/pdf_espelho.py:855](app/relatorios/pdf_espelho.py:855)
- `core/reexibir.py:45` e `:49`: atribuições genéricas em memória; os chamadores encontrados são em `colaboradores/views.py`. Não demonstram, por si, escrita persistida da pergunta. [app/core/reexibir.py:21](app/core/reexibir.py:21) [app/core/reexibir.py:45](app/core/reexibir.py:45) [app/core/reexibir.py:49](app/core/reexibir.py:49) [app/colaboradores/views.py:1116](app/colaboradores/views.py:1116) [app/colaboradores/views.py:1281](app/colaboradores/views.py:1281)

**Não concluído:** não classifiquei individualmente todos os demais indeterminados e aliases da saída, nem demonstrei a lista integral de consumidores indiretos, SQL bruto ou caminhos de PDF. O item 3, portanto, não está fechado. [não verificado]

### 4. Contrato F1 antigo — STATUS: FECHADO quanto à implicação para a F7

A F7 deve censar os leitores efetivos desta cópia; não há demonstração de equivalência geral entre eles e o fold histórico descrito no anexo. Aqui, `respondida` delega ao juiz do par projetado, enquanto a projeção temporal e `estado` consideram outros campos. A ancestralidade e os bytes do objeto histórico informados no anexo permanecem [não verificado]. [app/chamados/models.py:3674](app/chamados/models.py:3674) [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544) [app/chamados/juizes.py:1382](app/chamados/juizes.py:1382) [app/chamados/models.py:3152](app/chamados/models.py:3152)

### 5. Frota U1/U2 — STATUS: ABERTO

O anexo declara resultado de sombra com **U1 global = 29.931** e **15.118 validações projetadas sem linha**, com `rc_sombra=0`; não há acesso independente ao banco ou log nesta sessão. [não verificado]

O segundo contador usa correspondência de projeção por pergunta, ato e instante; ele não mede o último ato exigido para U2. Portanto, a sonda relatada fecha o resultado declarado para U1 e para o contador auxiliar, mas **U2 continua não medida**. [app/chamados/models.py:4122](app/chamados/models.py:4122) [app/chamados/models.py:4135](app/chamados/models.py:4135) [não verificado]

alcance nao verificado restante: 3

## 18/11 10:15 · sessao a2b0196b566b · - · - · origem=Ronald (tmux)

Evidências e limitações enviadas ao agente principal, incluindo os escritores de decisão, os leitores de REABERTURA, os importadores, os limites dos selos e a divergência entre o comentário sobre `ato_motivo` e o comportamento de `_pela_decisao` ([models.py:4075](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4075), [tarefa_do_dia.py:158](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:158)).

## 18/11 10:15 · sessao 7bb0d685aa36 · - · - · origem=Ronald (tmux)

Classificação dos candidatos dinâmicos relevantes enviada ao agente principal, com os sítios de leitura e escrita de R/V/C, os falsos positivos identificados e as lacunas que permanecem [nao verificado].

## 18/11 10:14 · sessao e888750e899c · - · - · origem=Ronald (tmux)

Rastreamento de templates, cascas dinâmicas e PDF enviado ao agente principal, com os respectivos arquivos, linhas e limites de verificação.

## 18/11 10:13 · sessao b997298c28f7 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:12:49 ===
sonda_leitura: cen_F9_s1.py [3b68d969da5dc8d8] no cpuset 4-7, lendo sombra como leitor (LEITURA)

R_projecao_nula_com_ato_resposta	ultimo_fisico=reabertura	ultimo_efetivo=reabertura	total=52
R_projecao_nula_com_ato_resposta	ultimo_fisico=recusa	ultimo_efetivo=recusa	total=7
V_projecao_nula_com_ato_validacao	ultimo_fisico=devolucao	ultimo_efetivo=devolucao	total=60
V_projecao_nula_com_ato_validacao	ultimo_fisico=reabertura	ultimo_efetivo=reabertura	total=9
V_projecao_nula_com_ato_validacao	ultimo_fisico=resposta	ultimo_efetivo=resposta	total=3
rc_sombra=0
```
linhas copiadas: 9

## 18/11 10:12 · sessao a775ebdff421 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:12:49 ===
sonda_leitura: cen_F7_s1.py [c5b054230a562d09] no cpuset 4-7, lendo sombra como leitor (LEITURA)

U1_carimbo_fora_de_identidade_global: 29931
validada_projetada_sem_linha: 15118
U2: NAO MEDIDA — porta real F1 indisponivel nesta copia
rc_sombra=0
```
linhas copiadas: 7

## 18/11 10:11 · sessao 514b43461e1f · RESPOSTAS-DE-LEI-1811-1010 · leitura · origem=Code

**Hoje, a partição já lê o juiz da L-130:** entre as perguntas vivas, fica pendente aquela cuja disputa está aberta e que não tem **instante e texto não vazio**; as demais ficam no histórico. Portanto, instante sem texto já é pendente hoje, desde que a disputa esteja aberta. [pergunta_no_app.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:76), [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544)

Depois da F5, o brief propõe ler a sequência de atos, conservando a projeção quando não houver decisão; o aval também determina que a ressuscitada, com o par zerado, apareça pendente. A definição completa para silêncio e absorção ainda é pergunta de lei do item 7; o número total que passará do histórico para pendente, portanto, está **[não verificado]**. [não verificado: anexos `brief.md` §3–4 e `respostas_de_lei_1811_1010.txt` itens 5 e 7]

A ressuscitação atual zera instante e texto sem gravar ato de devolução. A marca `da_ressuscitacao` existe, mas o código a descreve como inerte até o P2; assim, a sonda não consegue identificar todas as ressuscitadas pela sequência de atos. Uma ressuscitada cujo par já está zerado e cuja disputa está aberta **já é pendente hoje**, não uma passagem do histórico no pouso. [models.py:1567](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1567), [models.py:1619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1619), [models.py:4065](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4065), [pergunta_no_app.py:81](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:81)

A sonda abaixo chama as funções reais pedidas. Para a previsão, replica a aproximação do anexo: último ato por `ato_em, pk`, com RESPOSTA de texto vazio, RECUSA e REABERTURA como pendentes; mostra absorção rejeitada separadamente, pois sua aplicação à partição está por decidir. Ela não identifica o estoque inteiro de ressuscitadas nem substitui a medição pela função real da F5. [models.py:4081](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4081), [tarefa_do_dia.py:158](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:158), [não verificado: anexo `brief.md` §3 e §6]

```python
# Executar no shell Django da copia, com acesso de leitura ao banco.
# Nao grava nem registra atos.
from collections import Counter

from django.db.models import BooleanField, Case, OuterRef, Q, Subquery, Value, When
from django.utils import timezone

from chamados.juizes import pergunta_viva, q_nao_respondida, texto_vazio
from chamados.models import DecisaoPergunta as D, PerguntaDisputa as P

agora = timezone.now()
ultimo = D.objects.filter(pergunta_id=OuterRef("pk")).order_by("-ato_em", "-pk")
pendente_hoje = Q(disputa__fechada_em__isnull=True) & q_nao_respondida()

base = (
    P.objects.filter(P.q_vivas())
    .annotate(
        hoje_pendente=Case(
            When(pendente_hoje, then=Value(True)),
            default=Value(False),
            output_field=BooleanField(),
        ),
        ultimo_ato=Subquery(ultimo.values("ato")[:1]),
        ultimo_texto=Subquery(ultimo.values("ato_texto")[:1]),
        ultima_ausencia_status=Subquery(
            ultimo.values("absorcao_ausencia__status")[:1]
        ),
        ultima_marca_ressuscitacao=Subquery(
            ultimo.values("da_ressuscitacao")[:1]
        ),
    )
    .select_related("disputa", "celula")
    .order_by("pk")
)

cont = Counter()
prev = Counter()
indefinidos = Counter()

for p in base.iterator():
    if not pergunta_viva(p, agora=agora):
        continue

    cont["vivas"] += 1
    if p.hoje_pendente:
        cont["pendentes_hoje"] += 1
        continue

    cont["historico_hoje"] += 1

    # A disputa fechada permanece fora dos pendentes pela regra do app.
    if p.disputa.fechada_em is not None:
        cont["historico_disputa_fechada"] += 1
        continue

    cont["historico_disputa_aberta"] += 1
    ato = p.ultimo_ato

    # Categorias mutuamente exclusivas, apenas entre vivas que estao
    # hoje no historico e cuja disputa esta aberta.
    if ato == D.RESPOSTA and texto_vazio(p.ultimo_texto):
        prev["ultimo_ato_resposta_texto_vazio"] += 1
    elif ato == D.RECUSA:
        prev["ultimo_ato_recusa"] += 1
    elif ato == D.REABERTURA:
        if p.ultima_marca_ressuscitacao:
            prev["reabertura_marcada_ressuscitacao"] += 1
        else:
            prev["reabertura_sem_marca"] += 1
    elif ato == D.ABSORCAO and p.ultima_ausencia_status == "rejeitada":
        prev["absorcao_rejeitada_a_decidir"] += 1
    elif ato is None:
        # O brief conserva a projecao quando nao ha ato.
        cont["historico_sem_ato_conservado"] += 1
    elif ato == D.RESPOSTA:
        cont["historico_ultimo_ato_resposta_com_texto"] += 1
    else:
        # Nao atribuir uma regra de particao aos demais atos.
        chave = str(ato)
        if ato == D.ABSORCAO:
            chave += ":" + str(p.ultima_ausencia_status)
        indefinidos[chave] += 1

print("agora", timezone.localtime(agora).isoformat())
print("vivas", cont["vivas"])
print("pendentes hoje", cont["pendentes_hoje"])
print("historico hoje", cont["historico_hoje"])
print("historico hoje com disputa aberta", cont["historico_disputa_aberta"])

print("PREVISAO pela regra do brief, nao funcao do sistema:")
for motivo in (
    "ultimo_ato_resposta_texto_vazio",
    "ultimo_ato_recusa",
    "reabertura_marcada_ressuscitacao",
    "reabertura_sem_marca",
):
    print(" ", motivo, prev[motivo])

print(
    " subtotal dos motivos acima, sujeito a identificacao incompleta "
    "da ressuscitacao:",
    sum(prev[k] for k in (
        "ultimo_ato_resposta_texto_vazio",
        "ultimo_ato_recusa",
        "reabertura_marcada_ressuscitacao",
        "reabertura_sem_marca",
    )),
)
print(
    " absorcao rejeitada: PREVISAO condicional, particao a decidir:",
    prev["absorcao_rejeitada_a_decidir"],
)
print(" demais atos no historico aberto, sem previsao:", dict(indefinidos))
print(
    " ressuscitadas totais: NAO IDENTIFICAVEIS pela marca atual; "
    "nao somar uma categoria estimada ao subtotal"
)
```

`P.q_vivas()` é um pré-filtro; o script passa cada candidata pelo juiz real `pergunta_viva`. A face SQL `q_nao_respondida()` corresponde à negação do juiz usado pela partição atual. [models.py:3458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3458), [juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583)

Os números de produção e a execução da sonda estão **[não verificados]**: os dados fornecidos não incluem o resultado desta medição, e não há acesso ao banco. O subtotal impresso é uma aproximação sujeita à identificação incompleta da ressuscitação; não deve ser apresentado como o número definitivo de passagem no pouso. [não verificado: anexo `brief.md` §6] [nao verificado]

## 18/11 10:11 · sessao a70628e212cf · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

**Item 1 — ABERTO.** O inventário de escritores e leitores R/V/C e de `registrar` depende da execução e classificação da saída abaixo. A porta `registrar` aparece, por exemplo, em [models.py:1619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1619). [não verificado: completude e classificação do inventário]

**Item 2 — ABERTO; medida registrada para E4d.** A sonda identifica perguntas cuja projeção atual de resposta ou validação está nula e que possuem, respectivamente, **algum ato histórico** de resposta ou validação. Esse é o rótulo da medida registrada para o universo solicitado “ato sem projeção”: **projeção atual nula com existência de ato histórico correspondente; não há função real verificada nesta cópia que estabeleça a correspondência ato → projeção**. A função identificada opera no sentido projeção → sequência: [models.py:4122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4122).

| Medida da sombra | Último físico | Último efetivo | Total |
|---|---|---|---:|
| Projeção de resposta nula com ato histórico de resposta | reabertura | reabertura | 52 |
| Projeção de resposta nula com ato histórico de resposta | recusa | recusa | 7 |
| Projeção de validação nula com ato histórico de validação | devolução | devolução | 60 |
| Projeção de validação nula com ato histórico de validação | reabertura | reabertura | 9 |
| Projeção de validação nula com ato histórico de validação | resposta | resposta | 3 |

Os números são a saída raspada informada para `sombra_db`, com `rc_sombra=0`; não são uma contagem de produção verificada aqui. [não verificado: produção e eventual interseção entre os dois grupos] A reabertura pode limpar a projeção após registrar a fotografia, portanto a medida não classifica essas ocorrências como erros: [disputa_emissao.py:1577](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1577). Os imports e o selo global permanecem abertos até a análise do inventário; há exemplos em [relavrar_via_i6.py:47](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/relavrar_via_i6.py:47) e [test_contract_leitores_vitalidade.py:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:14). [não verificado: exclusividade e classificação completa]

**Item 3 — ABERTO.** Os campos novos e a interpretação de REABERTURA têm pontos conhecidos em [models.py:4071](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4071) e [tarefa_do_dia.py:163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:163); os demais caminhos dependem da saída e de sua análise. [não verificado: completude]

**Item 4 — ABERTO.** O grafo das duas cascas depende da saída abaixo; existem includes nas duas bases em [base.html:393](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/base.html:393) e [base_app.html:145](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/base_app.html:145). [não verificado: caminhos transitivos e consumidores completos]

Execute o script abaixo **de dentro de `app/`**. Ele imprime ocorrências estáticas e candidatos com arquivo, linha e papel; também sinaliza limites que exigem inspeção. Os papéis são classificações sintáticas, e a saída precisa ser examinada na volta 3: nomes montados em tempo de execução, atribuição de tipos e includes dinâmicos não são resolvidos integralmente por AST. [não verificado: saída e identidade do HEAD]

```python
import ast
import os
import re

CAMPOS = {
    "respondida_em", "resposta_colab", "validada_em", "validada_por",
    "materializada_em", "resposta_mantem_validacao", "REABERTURA",
}
SIMBOLOS = CAMPOS | {"registrar", "DecisaoPergunta"}
ALVOS_IMPORT = {
    "relavrar_via_i6", "dedup_perguntas_fato", "worklist",
    "curar_perguntas_em_limbo",
    "auditar_invariantes_chamados", "backfill_decisoes_pergunta",
    "reconciliar_fantasmas",
    "universo_i6", "aplicar", "CONTADORES_DO_APPLY",
    "_parse_pendentes", "MODULOS_ALVO",
}
ARQUIVOS_IMPORT = {
    "relavrar_via_i6.py", "dedup_perguntas_fato.py",
    "worklist.py", "curar_perguntas_em_limbo.py",
}
BASES = ("base.html", "base_app.html")
SKIP = {".git", "__pycache__", ".venv", "venv", "node_modules"}
encontrados = []


def emitir(path, linha, papel, detalhe):
    detalhe = re.sub(r"\s+", " ", str(detalhe)).strip()
    encontrados.append((path.replace(os.sep, "/"), linha, papel, detalhe))


def arquivos():
    for raiz, dirs, nomes in os.walk("."):
        dirs[:] = sorted(d for d in dirs if d not in SKIP)
        for nome in sorted(nomes):
            if nome.endswith((".py", ".html")):
                yield os.path.normpath(os.path.join(raiz, nome))


def fonte_no(no):
    try:
        return ast.unparse(no)
    except Exception:
        return type(no).__name__


def nomes_campo(texto):
    # Aceita também chaves ORM como validada_em__isnull.
    return sorted(
        c for c in SIMBOLOS
        if re.search(r"(?<![A-Za-z0-9_])" + re.escape(c)
                     + r"(?=$|[^A-Za-z0-9_]|__)", texto)
    )


def modulo_atual(path):
    partes = path[:-3].split(os.sep)
    if partes[-1] == "__init__":
        partes.pop()
    return ".".join(partes)


def resolver_import(path, no):
    modulo = no.module or ""
    if not no.level:
        return modulo
    partes = modulo_atual(path).split(".")
    if not path.endswith(os.sep + "__init__.py"):
        partes = partes[:-1]
    sobe = no.level - 1
    if sobe:
        partes = partes[:-sobe] if sobe <= len(partes) else []
    return ".".join(partes + ([modulo] if modulo else []))


class Inventario(ast.NodeVisitor):
    def __init__(self, path):
        self.path = path
        self.escopo = []
        self.pais = []

    def contexto(self):
        return ".".join(self.escopo) or "<modulo>"

    def visit(self, no):
        self.pais.append(no)
        try:
            return super().visit(no)
        finally:
            self.pais.pop()

    def em_escopo(self, no):
        self.escopo.append(no.name)
        self.generic_visit(no)
        self.escopo.pop()

    visit_FunctionDef = em_escopo
    visit_AsyncFunctionDef = em_escopo
    visit_ClassDef = em_escopo

    def visit_Import(self, no):
        for alias in no.names:
            relevante = (
                os.path.basename(self.path) in ARQUIVOS_IMPORT
                or any(x in alias.name.split(".") for x in ALVOS_IMPORT)
            )
            if relevante:
                emitir(self.path, no.lineno, "IMPORT",
                       "%s; import %s; alias=%s" %
                       (self.contexto(), alias.name, alias.asname or "-"))
        self.generic_visit(no)

    def visit_ImportFrom(self, no):
        resolvido = resolver_import(self.path, no)
        for alias in no.names:
            relevante = (
                os.path.basename(self.path) in ARQUIVOS_IMPORT
                or any(x in resolvido.split(".") for x in ALVOS_IMPORT)
                or alias.name in ALVOS_IMPORT
                or alias.name == "*"
            )
            if relevante:
                emitir(self.path, no.lineno, "IMPORT_FROM",
                       "%s; escrito=%s%s; resolvido=%s; nome=%s; alias=%s" %
                       (self.contexto(), "." * no.level, no.module or "",
                        resolvido, alias.name, alias.asname or "-"))
        self.generic_visit(no)

    def visit_Attribute(self, no):
        if no.attr in SIMBOLOS:
            papel = ("ESCRITA_ATTRIBUTE" if isinstance(no.ctx, ast.Store)
                     else "EXCLUSAO_ATTRIBUTE" if isinstance(no.ctx, ast.Del)
                     else "LEITURA_ATTRIBUTE")
            if no.attr == "registrar":
                papel = "REGISTRAR_CANDIDATO"
            emitir(self.path, no.lineno, papel,
                   "%s; %s" % (self.contexto(), fonte_no(no)))
        self.generic_visit(no)

    def visit_Name(self, no):
        if no.id in SIMBOLOS:
            emitir(self.path, no.lineno, "NOME_CANDIDATO",
                   "%s; %s; contexto=%s" %
                   (self.contexto(), no.id, type(no.ctx).__name__))
        self.generic_visit(no)

    def visit_Constant(self, no):
        if isinstance(no.value, str):
            campos = nomes_campo(no.value)
            if campos:
                emitir(self.path, no.lineno, "STRING_CANDIDATA",
                       "%s; campos=%s; valor=%r" %
                       (self.contexto(), ",".join(campos), no.value))
        self.generic_visit(no)

    def visit_Call(self, no):
        func = no.func
        nome = func.attr if isinstance(func, ast.Attribute) else (
            func.id if isinstance(func, ast.Name) else "")
        if nome in {"getattr", "setattr", "delattr", "hasattr"}:
            if len(no.args) >= 2:
                chave = no.args[1]
                if isinstance(chave, ast.Constant) and isinstance(chave.value, str):
                    if nomes_campo(chave.value):
                        emitir(self.path, no.lineno, nome.upper(),
                               "%s; %s" % (self.contexto(), fonte_no(no)))
                else:
                    emitir(self.path, no.lineno, "ACESSO_DINAMICO_NAO_RESOLVIDO",
                           "%s; %s" % (self.contexto(), fonte_no(no)))
        if nome in {"update", "filter", "exclude", "get", "values",
                    "values_list", "create", "get_or_create",
                    "update_or_create", "order_by", "only", "defer"}:
            for kw in no.keywords:
                if kw.arg is None:
                    emitir(self.path, no.lineno, "KWARGS_DINAMICOS_NAO_RESOLVIDOS",
                           "%s; %s" % (self.contexto(), fonte_no(no)))
                elif nomes_campo(kw.arg):
                    papel = ("ORM_ESCRITA_CANDIDATA" if nome in
                             {"update", "create", "get_or_create",
                              "update_or_create"} else "ORM_LEITURA_CANDIDATA")
                    emitir(self.path, no.lineno, papel,
                           "%s; chamada=%s; chave=%s" %
                           (self.contexto(), nome, kw.arg))
            for arg in no.args:
                if isinstance(arg, ast.Starred):
                    emitir(self.path, no.lineno, "ARGS_DINAMICOS_NAO_RESOLVIDOS",
                           "%s; %s" % (self.contexto(), fonte_no(no)))
        if nome == "registrar":
            emitir(self.path, no.lineno, "CHAMADA_REGISTRAR_CANDIDATA",
                   "%s; %s" % (self.contexto(), fonte_no(no)))
        self.generic_visit(no)


templates = {}
arestas = []
exibem = set()
tag_re = re.compile(r"({{.*?}}|{%.*?%})", re.S)
diretiva_re = re.compile(r"^{%\s*(include|extends)\s+(.+?)\s*%}$", re.S)
literal_re = re.compile(r"""^\s*(['"])(.*?)\1(?:\s|$)""", re.S)

for path in arquivos():
    path = path.removeprefix("./")
    try:
        with open(path, encoding="utf-8") as f:
            conteudo = f.read()
    except (OSError, UnicodeError) as exc:
        emitir(path, 1, "ERRO_LEITURA", exc)
        continue

    if path.endswith(".py"):
        try:
            arvore = ast.parse(conteudo, filename=path)
        except (SyntaxError, ValueError) as exc:
            emitir(path, getattr(exc, "lineno", 1) or 1, "ERRO_AST", exc)
            continue
        Inventario(path).visit(arvore)
        continue

    # Indexa todos os diretórios chamados templates, inclusive os de apps.
    partes = path.split(os.sep)
    if "templates" in partes:
        i = len(partes) - 1 - partes[::-1].index("templates")
        nome_template = "/".join(partes[i + 1:])
        templates.setdefault(nome_template, []).append(path)
    else:
        nome_template = path.replace(os.sep, "/")

    for m in tag_re.finditer(conteudo):
        tag = m.group(0)
        linha = conteudo.count("\n", 0, m.start()) + 1
        campos = nomes_campo(tag)
        if campos:
            emitir(path, linha, "TEMPLATE_LEITURA_CANDIDATA",
                   "campos=%s; %s" % (",".join(campos), tag))
        if re.search(r"(?i)pergunta", tag):
            exibem.add(nome_template)
            emitir(path, linha, "TEMPLATE_PERGUNTA_CANDIDATO", tag)
        d = diretiva_re.match(tag)
        if d:
            tipo, expressao = d.groups()
            literal = literal_re.match(expressao)
            if literal:
                destino = literal.group(2)
                arestas.append((nome_template, destino, path, linha, tipo))
            else:
                emitir(path, linha, "TEMPLATE_DESTINO_DINAMICO_NAO_RESOLVIDO",
                       "%s; %s" % (tipo, expressao))

print("=== OCORRENCIAS PYTHON, IMPORTS E TEMPLATES ===")
for path, linha, papel, detalhe in sorted(encontrados):
    print("%s:%s | %s | %s" % (path, linha, papel, detalhe))

print("=== GRAFO DE INCLUDES E EXTENDS A PARTIR DAS BASES ===")
# Inclui tanto includes saindo da base quanto filhos que herdam dela.
alcancados = set(BASES)
mudou = True
while mudou:
    mudou = False
    for origem, destino, path, linha, tipo in arestas:
        novo = None
        if tipo == "include" and origem in alcancados:
            novo = destino
        elif tipo == "extends" and destino in alcancados:
            novo = origem
        if novo is not None and novo not in alcancados:
            alcancados.add(novo)
            mudou = True

for origem, destino, path, linha, tipo in sorted(arestas):
    if (tipo == "include" and origem in alcancados) or (
        tipo == "extends" and destino in alcancados
    ):
        status = "resolvido" if destino in templates else "DESTINO_NAO_ENCONTRADO"
        if len(templates.get(destino, [])) > 1:
            status = "DESTINO_AMBIGUO"
        print("%s:%s | %s | %s -> %s | %s | arquivos=%s" %
              (path, linha, tipo, origem, destino, status,
               ",".join(templates.get(destino, [])) or "-"))

print("=== TEMPLATES ALCANCADOS COM PERGUNTA CANDIDATA ===")
for nome in sorted(alcancados & exibem):
    print("%s | arquivos=%s" %
          (nome, ",".join(templates.get(nome, [])) or "NAO_ENCONTRADO"))

print("=== LIMITES ===")
print("Classificacoes sao sintaticas e incluem candidatos e falsos positivos.")
print("Inspecionar erros, aliases, chamadas indiretas e acessos dinamicos.")
print("Destinos dinamicos, templates gerados e exibicao sem nome 'pergunta' "
      "exigem analise adicional.")
```

alcance nao verificado restante: 4

## 18/11 10:11 · sessao d7a04e67cf1b · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

**4. Contrato F1 antigo — STATUS: FECHADO quanto à implicação para a F7.** O anexo apresenta uma leitura histórica que ordenava decisões por `(ato_em, pk)` e fazia o fold dos atos; o dado fornecido informa que `be2b8d733` não é ancestral do HEAD. A F7 deve, portanto, censar os leitores do HEAD, sem presumir que esse contrato histórico esteja presente ou seja executado hoje. [não verificado]

No HEAD, a propriedade `respondida` delega a `pergunta_respondida`, que lê o par projetado `respondida_em` e `resposta_colab`; a projeção temporal lê também validação e materialização, e `estado` dá precedência a `validada_em`. A leitura histórica do último ato, do motivo de recusa e dos demais resultados do fold não tem equivalência geral demonstrada por esses leitores: o inventário do item 3 continua necessário. [models.py:3674](app/chamados/models.py:3674) [juizes.py:1382](app/chamados/juizes.py:1382) [juizes.py:1544](app/chamados/juizes.py:1544) [models.py:3152](app/chamados/models.py:3152)

**5. Frota U1/U2 — STATUS: FECHADO para o resultado da sonda na sombra; U2 não medida.** A saída fornecida informa **U1 global = 29.931** e **validação projetada sem linha = 15.118**, com `rc_sombra=0`. São resultados declarados da sombra, não uma leitura independente nesta sessão. [não verificado]

O segundo número conta ausência de correspondência da validação projetada por pergunta, ato e instante; não determina qual é o último ato. **U2 permanece não medida**, pois a saída declara indisponível a porta real F1. [models.py:4122](app/chamados/models.py:4122) [não verificado]

**Itens 1 e 3 — STATUS: ABERTOS até a execução e revisão da saída.** O script abaixo inventaria as formas estáticas pedidas, incluindo testes e migrações, e imprime incertezas. Atribuição de casca, papel de escrita e resolução de nomes dinâmicos exigem revisão quando a sintaxe não os determina; a execução e a completude semântica permanecem **[não verificado]**. [nao verificado]

Execute a partir de `app/`, sem Django:

```python
import ast
import os
import re

CAMPOS = {
    "validada_em", "validada_por", "respondida_em",
    "resposta_colab", "materializada_em",
}
FUNCOES = {
    "pergunta_respondida", "respondida_sem_texto", "texto_sem_carimbo",
    "projecao_temporal_da_pergunta", "linhas_da_projecao_sem_linha",
    "pergunta_viva", "_pergunta_viva_calc",
}
METODOS = {"get", "values", "values_list", "filter"}
ESCRITORES = {"update", "create", "get_or_create", "update_or_create"}
BASES = {"base.html", "base_app.html"}
CARD = "chamados/partials/_pergunta_card.html"

def arquivos():
    for raiz, dirs, nomes in os.walk("."):
        dirs[:] = sorted(d for d in dirs if d not in {
            ".git", "__pycache__", ".venv", "venv", "node_modules",
        })
        for nome in sorted(nomes):
            yield os.path.join(raiz, nome)

def local(caminho, linha):
    return "app/%s:%d" % (caminho.removeprefix("./"), linha)

def campo_em_nome(nome):
    # Inclui relações e sufixos ORM: pergunta__validada_em__isnull.
    partes = nome.split("__")
    return sorted(
        c for c in CAMPOS
        if any(p == c or p == c + "_id" for p in partes)
    )

def nome_chamada(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return ""

def trecho(texto, no):
    s = ast.get_source_segment(texto, no) or "?"
    return " ".join(s.split())[:240]

todos = list(arquivos())
templates = {}
for caminho in todos:
    if not caminho.endswith((".html", ".htm", ".txt")):
        continue
    # Permite templates em app/templates e em diretórios templates de apps.
    normal = caminho.removeprefix("./").replace(os.sep, "/")
    marcador = "templates/"
    if marcador not in normal:
        continue
    nome = normal.split(marcador, 1)[1]
    try:
        with open(caminho, encoding="utf-8") as f:
            texto = f.read()
    except (OSError, UnicodeError) as exc:
        print("ERRO template", caminho, repr(exc))
        continue
    templates.setdefault(nome, []).append((caminho, texto))

# Não elimina arquivos com o mesmo nome de template: a precedência do loader
# é uma questão de configuração, e ambas as definições são inventariadas.
arestas = []
tags = re.compile(r"{%\s*(include|extends)\s+(.*?)%}", re.S)
literal = re.compile(r"""^\s*(['"])(.*?)\1""", re.S)

for nome, versoes in sorted(templates.items()):
    for caminho, texto in versoes:
        for m in tags.finditer(texto):
            linha = texto.count("\n", 0, m.start()) + 1
            tipo, argumento = m.group(1), m.group(2).strip()
            achou = literal.match(argumento)
            destino = achou.group(2) if achou else None
            arestas.append((nome, destino, tipo, caminho, linha, argumento))

# Propaga cascas por herança e por inclusão. A atribuição é conservadora:
# um fragmento reutilizado pode receber várias cascas.
cascas = {nome: set() for nome in templates}
for base in BASES:
    cascas.setdefault(base, set()).add(base)

for origem, destino, tipo, caminho, linha, argumento in arestas:
    if tipo == "extends" and destino is None and re.search(
        r"\bbase_tpl\b", argumento
    ):
        cascas.setdefault(origem, set()).update(BASES)

mudou = True
while mudou:
    mudou = False
    for origem, destino, tipo, caminho, linha, argumento in arestas:
        if destino is None:
            continue
        cascas.setdefault(origem, set())
        cascas.setdefault(destino, set())
        antes = len(cascas[origem] if tipo == "extends" else cascas[destino])
        if tipo == "extends":
            cascas[origem].update(cascas[destino])
            depois = len(cascas[origem])
        else:
            cascas[destino].update(cascas[origem])
            depois = len(cascas[destino])
        mudou |= antes != depois

# Inclui recursivamente todos os que levam ao card; depois segue todas as
# arestas de include/extends a partir deles e das duas bases.
sementes = set(BASES) | {CARD}
mudou = True
while mudou:
    mudou = False
    for origem, destino, tipo, caminho, linha, argumento in arestas:
        if tipo == "include" and destino in sementes and origem not in sementes:
            sementes.add(origem)
            mudou = True

alcance = set(sementes)
mudou = True
while mudou:
    mudou = False
    for origem, destino, tipo, caminho, linha, argumento in arestas:
        if origem in alcance and destino and destino not in alcance:
            alcance.add(destino)
            mudou = True

print("=== GRAFO TRANSITIVO: includes, extends e cascas possíveis ===")
for nome in sorted(alcance):
    print("TEMPLATE", nome, "casca=",
          ",".join(sorted(cascas.get(nome, set()))) or "indeterminada")
    if nome not in templates:
        print("  AUSENTE OU CARREGADOR EXTERNO [nao verificado]")
    elif len(templates[nome]) > 1:
        print("  MULTIPLAS DEFINICOES: precedencia [nao verificado]")
    for origem, destino, tipo, caminho, linha, argumento in arestas:
        if origem != nome:
            continue
        print(" ", local(caminho, linha), tipo,
              destino if destino else "DINAMICO: " + argumento,
              "" if destino else "[nao verificado]")

print("=== ACESSOS EM TEMPLATES: leitura sintatica ===")
expressao = re.compile(r"{{(.*?)}}|{%(.*?)%}", re.S)
acesso = re.compile(
    r"\b[A-Za-z_]\w*(?:\.[A-Za-z_]\w*)*"
    r"\.(validada_em|validada_por|respondida_em|resposta_colab|materializada_em)"
    r"(?:_id)?\b"
)
for nome, versoes in sorted(templates.items()):
    for caminho, texto in versoes:
        for m in expressao.finditer(texto):
            conteudo = m.group(1) if m.group(1) is not None else m.group(2)
            for achou in acesso.finditer(conteudo):
                pos = m.start() + 2 + achou.start()
                linha = texto.count("\n", 0, pos) + 1
                print(local(caminho, linha), "leitura", nome,
                      achou.group(0),
                      "casca=" + (",".join(sorted(cascas.get(nome, set())))
                                  or "indeterminada"))

print("=== PYTHON: acessos e referencias estaticas ===")
for caminho in sorted(p for p in todos if p.endswith(".py")):
    try:
        with open(caminho, encoding="utf-8") as f:
            texto = f.read()
        arvore = ast.parse(texto, filename=caminho)
    except (OSError, UnicodeError, SyntaxError) as exc:
        print("ERRO Python", caminho, repr(exc), "[nao verificado]")
        continue

    pais = {}
    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    # Registra aliases explícitos; referências a nomes coincidentes são
    # candidatas, pois AST isolada não resolve tipos nem importações indiretas.
    aliases = {nome: nome for nome in FUNCOES}
    for no in ast.walk(arvore):
        if isinstance(no, ast.ImportFrom):
            for item in no.names:
                if item.name in FUNCOES:
                    aliases[item.asname or item.name] = item.name

    saidas = set()

    def emitir(no, papel, categoria, detalhe):
        saidas.add((no.lineno, no.col_offset, papel, categoria, detalhe))

    for no in ast.walk(arvore):
        if isinstance(no, ast.Attribute):
            campos = campo_em_nome(no.attr)
            if campos:
                papel = ("escrita" if isinstance(no.ctx, (ast.Store, ast.Del))
                         else "leitura")
                emitir(no, papel, "atributo " + ",".join(campos),
                       trecho(texto, no))
            if no.attr in FUNCOES:
                emitir(no, "referencia", "funcao " + no.attr,
                       trecho(texto, no))

        if isinstance(no, ast.Name) and no.id in aliases:
            emitir(no, "referencia", "funcao " + aliases[no.id],
                   trecho(texto, no))

        if isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if no.name in FUNCOES:
                emitir(no, "definicao", "funcao " + no.name, no.name)

        if isinstance(no, ast.ImportFrom):
            for item in no.names:
                if item.name in FUNCOES:
                    emitir(no, "importacao", "funcao " + item.name,
                           trecho(texto, no))

        if not isinstance(no, ast.Call):
            continue
        metodo = nome_chamada(no.func)

        if metodo in {"getattr", "setattr", "hasattr", "delattr"}:
            if len(no.args) >= 2:
                chave = no.args[1]
                if isinstance(chave, ast.Constant) and isinstance(chave.value, str):
                    campos = campo_em_nome(chave.value)
                    if campos:
                        papel = ("escrita" if metodo in {"setattr", "delattr"}
                                 else "leitura")
                        emitir(no, papel, metodo + " " + ",".join(campos),
                               trecho(texto, no))
                else:
                    emitir(no, "indeterminado", metodo + " nome dinamico",
                           trecho(texto, no))

        if metodo in METODOS | ESCRITORES | {"Q"}:
            papel = ("escrita possivel" if metodo in ESCRITORES
                     else "leitura ou consulta")
            for argumento in no.args:
                if isinstance(argumento, ast.Constant) and isinstance(argumento.value, str):
                    campos = campo_em_nome(argumento.value)
                    if campos:
                        emitir(no, papel, metodo + " " + ",".join(campos),
                               trecho(texto, no))
                elif isinstance(argumento, ast.Starred):
                    emitir(no, "indeterminado", metodo + " argumentos dinamicos",
                           trecho(texto, no))
            for kw in no.keywords:
                if kw.arg is None:
                    emitir(no, "indeterminado", metodo + " keywords dinamicos",
                           trecho(texto, no))
                else:
                    campos = campo_em_nome(kw.arg)
                    if campos:
                        emitir(no, papel, metodo + " " + ",".join(campos),
                               trecho(texto, no))

    for linha, coluna, papel, categoria, detalhe in sorted(saidas):
        print(local(caminho, linha), papel, categoria, "|", detalhe)

print("=== LIMITES ===")
print("Nomes dinamicos, aliases indiretos, SQL bruto, dicionarios,")
print("templates carregados fora da arvore e fluxo em tempo de execucao:")
print("[nao verificado]; referencias homonimas e cascas sao candidatas.")
```

alcance nao verificado restante: 2

## 18/11 10:08 · sessao fced45f6860a · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:06:59 ===
sonda_leitura: i7_sonda.py [a39a32993596c041] no cpuset 4-7, lendo sombra como leitor (LEITURA)

agora 2026-11-18T10:07:00.928492-03:00 prefiltro 14975 vivas 11381
vivas por ultimo_ato {None: 10626, 'resposta': 399, 'silencio': 15, 'reabertura': 275, 'recusa': 6, 'devolucao': 60}
vivas pendentes_hoje 10092 historico_hoje 1289
Q1 RESPOSTA texto_nao_vazio pendente_hoje->historico 0
Q2 RESPOSTA texto_vazio historico_hoje->pendente 0
Q3 RECUSA historico_hoje->pendente 0
Q4 REABERTURA historico_hoje->pendente 29
Q5 SILENCIO pendente_hoje->historico 11
Q6 ABSORCAO rejeitada historico_hoje->pendente 0
SEM_ATO vivas sem DP 10626
rc_sombra=0
```
linhas copiadas: 14

## 18/11 10:06 · sessao a72dc2fb3002 · [nome]-NAO-DESFAZ-O-PAPEL · verificador · origem=Code

1. **[ACHADO] A função genérica aceita uma grade cuja régua não está alinhada às células.** `contar_espurias` usa `grade['marcos']` por índice para interpretar `dia['celulas']` ([orfa_classe.py:165](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:165), [orfa_classe.py:194](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:194)). Já `grade_da_celula` devolve régua de união no topo e marcos próprios por dia; o remapeamento é uma operação separada ([leitor_celula.py:281](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/services/leitor_celula.py:281), [leitor_celula.py:413](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/services/leitor_celula.py:413), [leitor_celula.py:445](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/services/leitor_celula.py:445)). Nessa entrada, um `missing` pode ser associado à hora de outro marco e alterar o recorte. A entrada `contar_espurias_do_colab` usa o builder, não esse leitor ([orfa_classe.py:233](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:233)).

2. **[ACHADO] O campo `orfas` perde o produtor ao passar pela ata.** O builder emite `Orfa`, uma string com atributo `produtor` ([orfa_classe.py:125](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:125), [utils.py:631](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:631)); o cartório grava apenas `str(h)` e o leitor devolve strings ([cartorio.py:742](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/services/cartorio.py:742), [leitor_celula.py:180](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/services/leitor_celula.py:180)). Se essa grade for passada ao contador genérico, as órfãs com hora passam a `sem_produtor`, nunca P1, e não provocam pergunta ao dono ([orfa_classe.py:175](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:175), [orfa_classe.py:202](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:202)). O caminho da entrada por colaborador recompõe a grade e preserva o atributo em memória; uso do contador sobre ata em produção: **[não verificado]** ([orfa_classe.py:233](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:233)).

3. **Insumos do juiz: equivalentes sob os mesmos dados, com diferença de execução.** A entrada passa `ini`, `fim`, `hoje` e `agora` ao mesmo `InsumosDoDono`; o `handle` também passa um `espelho` já calculado ([orfa_classe.py:229](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:229), [e6_oraculo.py:296](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:296), [e6_oraculo.py:328](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:328)). Sem espelho, `dono` primeiro pergunta com CXR falso e só calcula o espelho se a resposta preliminar for ESTRUTURA; CADASTRO e BATIDA independem dessa lista pela ordem do juiz ([e6_oraculo.py:98](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:98), [e6_oraculo.py:162](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:162), [e6_oraculo.py:182](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:182)). Portanto, com a mesma fotografia dos dados e espelho bem-sucedido, a omissão não muda a resposta; o `handle` pula o colaborador se o espelho falha, enquanto a entrada pode responder CADASTRO/BATIDA sem pedir o espelho e pode propagar falha quando o pede ([e6_oraculo.py:315](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:315), [orfa_classe.py:235](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:235)). Igualdade das fotografias em produção: **[não verificado]**.

4. **Data perguntada: nos caminhos do builder examinados, corresponde à linha.** `grade_espelho_janela` passa `ref=fim` e o clip esvazia linhas sem removê-las ([utils.py:2346](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:2346), [utils.py:968](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:968)). Tanto o builder por turno quanto o legacy montam cada linha com `ref - i`; o legacy remapeia os segmentos por data antes de emitir a lista ([utils.py:1784](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1784), [utils.py:1027](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1027), [utils.py:1040](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1040)). Não encontrei nesses caminhos uma linha em que a data seja diferente de `fim - i`; o contador genérico, porém, confia no `ref_fim` fornecido e não valida a grade ([orfa_classe.py:179](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:179)).

5. **Significados e unidades exigem leitura cuidadosa.** P1 nasce no casador; P2 é sobra atribuída à data civil; no builder por turno, órfãs do casamento ficam na data de entrada do turno e sobras P3 são atribuídas à data civil ([utils.py:631](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:631), [utils.py:801](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:801), [utils.py:1445](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1445), [utils.py:1824](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/utils.py:1824)). O contador lê `missing` e a hora da coluna, lê a classe de `classificar_orfas` e o produtor do item; grava dono **por dia P1**, mas repete esse dono também nos itens P2/P3 do mesmo dia ([orfa_classe.py:165](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:165), [orfa_classe.py:200](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:200)). Assim, `por_dono` e `dias_p1` contam dias, enquanto `p1` e `por_produtor` contam ocorrências; o dono em `itens` é do **dia**, não uma decisão sobre cada órfã ([orfa_classe.py:203](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:203)). Leitor externo desses novos campos: **[não verificado]**.

6. **Raio: a constante deste módulo seleciona o universo e, por isso, afeta o alarma.** Ela é lida em `espurias_do_dia`; a seleção determina quais dias chegam ao juiz e, consequentemente, pode mudar `por_dono` e `alarma`, sem mudar a resposta do juiz para um mesmo dia nem o corpo de `classificar_orfas` ([orfa_classe.py:120](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:120), [orfa_classe.py:171](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:171), [orfa_classe.py:202](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:202)). Há outra constante **homônima**, independente, em `regua_defesa`, usada por decisões fora do contador; ela não é a constante de `orfa_classe` ([regua_defesa.py:392](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/regua_defesa.py:392), [motor_calculo_v2.py:2076](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/motor_calculo_v2.py:2076)).

7. **Testes: os quatro mutantes perguntados são cobertos por asserções, com limite de integração.** Os testes exigem CADASTRO sem alarma, ESTRUTURA com alarma e BATIDA contado sem alarma ([test_contador_espurias.py:133](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/tests/test_contador_espurias.py:133)). O teste da entrada substitui `InsumosDoDono` por mock e exige sua construção e chamada de `.dono(fim)`, portanto morde uma entrada que abandone essa chamada ([test_contador_espurias.py:275](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/tests/test_contador_espurias.py:275)). Ele não exercita o juiz real, a construção real da grade ou a igualdade entre espelhos; a data é testada numa grade fabricada ([test_contador_espurias.py:152](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/tests/test_contador_espurias.py:152), [test_contador_espurias.py:278](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/tests/test_contador_espurias.py:278)). Execução dos testes: **[não verificado]**.

8. **Checklist do delta fornecido:** a entrada delega ao juiz existente; não encontrei segundo juiz de dono no trecho examinado ([orfa_classe.py:227](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:227), [e6_oraculo.py:195](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/ponto/management/commands/e6_oraculo.py:195)). A entrada recebe `agora` e deriva `hoje` desse instante, sem ler outro relógio nela própria ([orfa_classe.py:218](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:218)). O retorno é montado em memória; segundo escritor persistente pelo trecho examinado: não identificado ([orfa_classe.py:208](/home/ronald/codex_ws/f22632e387f56e41b88c386e4eedc40ad568b1d1/app/escala/orfa_classe.py:208)). Permanecem os riscos de representação e perda de proveniência dos achados 1–2 e a mistura de dia do turno/data civil do achado 5. Comparação independente dos commits, integridade integral do delta e resultado em produção: **[não verificado]**.

## 18/11 10:06 · sessao af4df093d524 · RESPOSTAS-DE-LEI-1811-1010 · censo · origem=Code

A seção B contém **cinco itens numerados**, dos quais quatro estavam pendentes: 1, 3, 4 e 5. O item 2 já estava fechado no anexo e sua conclusão sobre a chamada específica foi confirmada no corpo de `reabrir_para_correcao`. [app/chamados/services/disputa_emissao.py:1577](app/chamados/services/disputa_emissao.py:1577)

Não consegui fechar os quatro pendentes com a evidência disponível. Em particular, a cópia não tem metadados Git acessíveis para conferir o HEAD ou ler `be2b8d733` **[não verificado]**, e a busca nesta árvore não encontrou a porta histórica `atos_pergunta.leitura` **[não verificado]**. Abaixo separo o que foi comprovado do que ainda falta.

**Método.** Usei busca literal com `rg` para campos, nomes de funções, templates e includes; examinei os corpos relevantes; e percorri com AST os arquivos Python de `app`, excluindo testes e migrações **[não verificado: classificação integral]**. A AST procurou atributos, imports de `atos_pergunta`, inclusive aliases, e nomes literais em `getattr`/`setattr`/`hasattr`; encontrou também **97 chamadas com nome dinâmico ainda não resolvido**, portanto não sustenta uma lista completa de leitores e escritores **[não verificado]**. O alias de renderização do fragmento foi identificado em [app/chamados/views.py:397](app/chamados/views.py:397). O inventário de 407 ocorrências literais Python inclui comentários e não equivale a 407 leitores **[não verificado]**.

### 1. Cascas do card — STATUS: ABERTO

O caminho estático confirmado é:

- `meu_atendimento` escolhe a casca por `base_tpl` e inclui o card nos dois loops: [meu_atendimento.html:1](app/templates/chamados/meu_atendimento.html:1), [meu_atendimento.html:127](app/templates/chamados/meu_atendimento.html:127), [meu_atendimento.html:144](app/templates/chamados/meu_atendimento.html:144); a escolha entre `base_app.html` e `base.html` está em [context_processors.py:210](app/core/context_processors.py:210).
- A página de detalhe usa `base.html` e inclui o card de pergunta em [detalhe_local.html:1](app/templates/chamados/detalhe_local.html:1) e [detalhe_local.html:310](app/templates/chamados/detalhe_local.html:310). O fragmento é renderizado pelo alias `_render_card` em [views.py:397](app/chamados/views.py:397) e [views.py:404](app/chamados/views.py:404).
- O modal é renderizado em [views.py:927](app/chamados/views.py:927), [views.py:1400](app/chamados/views.py:1400) e [fio.py:543](app/chamados/services/fio.py:543). O painel abre a prévia de lote pelo drawer em [painel_gestao.html:14](app/templates/chamados/painel_gestao.html:14); os renders da prévia estão em [views_cobrar.py:360](app/chamados/views_cobrar.py:360), [views_cobrar.py:364](app/chamados/views_cobrar.py:364), [views_cobrar.py:380](app/chamados/views_cobrar.py:380) e [views_cobrar.py:388](app/chamados/views_cobrar.py:388).
- Includes diretos identificados em `base.html`: [base.html:92](app/templates/base.html:92), [base.html:93](app/templates/base.html:93), [base.html:290](app/templates/base.html:290), [base.html:388](app/templates/base.html:388), [base.html:393](app/templates/base.html:393), [base.html:399](app/templates/base.html:399), [base.html:432](app/templates/base.html:432). Em `base_app.html`: [base_app.html:85](app/templates/base_app.html:85), [base_app.html:145](app/templates/base_app.html:145), [base_app.html:154](app/templates/base_app.html:154). O copiloto inclui o ícone em [core/_copiloto.html:34](app/templates/core/_copiloto.html:34).

**Falta:** completar a inspeção transitiva de includes e comprovar renderização e cliques nas duas cascas e nos fragmentos **[não verificado]**. A matriz antiga também não deve ser tratada como retrato literal deste HEAD: os gates de resposta do modal e do card de pergunta agora usam `respondida`, em [modal_fio.html:185](app/templates/chamados/partials/modal_fio.html:185), [modal_fio.html:217](app/templates/chamados/partials/modal_fio.html:217), [\_pergunta_card.html:24](app/templates/chamados/partials/_pergunta_card.html:24) e [\_pergunta_card.html:29](app/templates/chamados/partials/_pergunta_card.html:29).

### 2. Escritor de reabertura — STATUS: FECHADO para a chamada questionada

`reabrir_para_correcao` prepara a limpeza do par e da validação, registra diretamente a decisão com a fotografia anterior e chama `descarimbar_em_lote`; seu corpo não chama `linhas_da_projecao_sem_linha`. Isso fecha a dúvida específica do item, não o censo de todos os escritores. [disputa_emissao.py:1610](app/chamados/services/disputa_emissao.py:1610), [disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632), [disputa_emissao.py:1649](app/chamados/services/disputa_emissao.py:1649).

### 3. Leitores cruzados — STATUS: ABERTO

A lista abaixo é **parcial**, não o censo integral pedido:

- Juiz do par: [juizes.py:1544](app/chamados/juizes.py:1544); fase que prioriza `validada_em`: [models.py:3152](app/chamados/models.py:3152); flag do card: [fio.py:178](app/chamados/services/fio.py:178).
- Leitores diretos da validação na interface: [\_pergunta_card.html:20](app/templates/chamados/partials/_pergunta_card.html:20), [\_pergunta_card.html:23](app/templates/chamados/partials/_pergunta_card.html:23), [\_pergunta_card.html:42](app/templates/chamados/partials/_pergunta_card.html:42), [modal_fio.html:228](app/templates/chamados/partials/modal_fio.html:228). O card do colaborador consome a fase em [\_card_atendimento.html:189](app/templates/chamados/partials/_card_atendimento.html:189).
- Texto e instante na API: [api/views.py:1163](app/api/views.py:1163), [api/views.py:2561](app/api/views.py:2561); filtro de apresentação que lê texto, inclusive por `getattr`: [chamados_tags.py:139](app/chamados/templatetags/chamados_tags.py:139), [chamados_tags.py:166](app/chamados/templatetags/chamados_tags.py:166).
- Comandos que consultam ou classificam projeções incluem [backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [auditar_invariantes_chamados.py:206](app/chamados/management/commands/auditar_invariantes_chamados.py:206) e [auditar_invariantes_chamados.py:1077](app/chamados/management/commands/auditar_invariantes_chamados.py:1077). A função que detecta projeção sem linha é usada por escritores e pelo backfill, por exemplo [validacao.py:167](app/chamados/services/validacao.py:167), [disputa_emissao.py:1480](app/chamados/services/disputa_emissao.py:1480), [models.py:1619](app/chamados/models.py:1619) e [backfill_fio_morto_e4b.py:141](app/chamados/management/commands/backfill_fio_morto_e4b.py:141).

**Q4 é CONSERVADA**, conforme decisão fornecida no pedido: validação projetada sem linha deve aparecer como “Validada” **[não verificado no código como decisão histórica]**. Os leitores que precisam preservar esse caso são, no mínimo, os que exibem ou decidem a fase/validação nos pontos citados de `pergunta_card`, `modal_fio`, `estado` e card do colaborador; a lista integral de leitores indiretos ainda depende do censo aberto acima. [models.py:3159](app/chamados/models.py:3159), [\_pergunta_card.html:23](app/templates/chamados/partials/_pergunta_card.html:23), [modal_fio.html:228](app/templates/chamados/partials/modal_fio.html:228), [\_card_atendimento.html:189](app/templates/chamados/partials/_card_atendimento.html:189).

**Falta:** classificar todas as ocorrências ORM, escritores, leitores indiretos, nomes dinâmicos e caminhos PDF; não tenho evidência para afirmar uma lista completa arquivo:linha **[não verificado]**.

### 4. Contrato F1 antigo — STATUS: ABERTO

A assinatura e o fold foram fornecidos no texto da matriz, mas não foi possível conferir o conteúdo histórico nem seus chamadores nesta cópia **[não verificado]**. A propriedade efetiva desta cópia delega ao juiz do par projetado: [models.py:3674](app/chamados/models.py:3674), [juizes.py:1544](app/chamados/juizes.py:1544).

**Falta:** acesso ao objeto Git histórico `be2b8d733`, ou ao conteúdo da implementação e seus chamadores **[não verificado]**. Não é possível afirmar que a descrição fornecida da F1 corresponde byte a byte à implementação histórica **[não verificado]**.

### 5. Frota U1/U2 — STATUS: ABERTO; fecha só por sonda

As contagens de produção não foram fornecidas **[não verificado]**. Para U1, existe uma porta real cujo predicado é carimbo presente e via fora de `VIAS_DE_IDENTIDADE`: [relavra_via.py:171](app/chamados/services/relavra_via.py:171). A sonda abaixo usa essa porta. Para U2, a matriz exige o último ato pela leitura F1, mas essa porta não foi localizada nesta cópia **[não verificado]**. Assim, não posso entregar honestamente uma sonda U2 pela função real sem reproduzir o fold; falta a implementação F1 e sua porta executável **[não verificado]**.

A mesma execução mede o contador pedido de **validação projetada sem linha**, chamando a função real: ela compara pergunta, ato e instante, e devolve a linha virtual de validação quando não há correspondência. [models.py:4122](app/chamados/models.py:4122).

Executar no ambiente Django de produção, a partir de `app`, com `python manage.py shell`, colando o bloco abaixo. Ele apenas consulta e imprime contagens; não chama `registrar` **[não verificado em produção]**.

```python
from chamados.models import DecisaoPergunta, PerguntaDisputa
from chamados.services.relavra_via import q_carimbo_desfazivel

# U1: universo global da porta; sem restringir a disputa aberta.
u1 = PerguntaDisputa.objects.filter(
    q_carimbo_desfazivel(so_disputa_aberta=False)
).count()

# Mesmo predicado e mesma função usados pelo sistema: correspondência
# por (pergunta_id, ato=VALIDACAO, ato_em=validada_em).
qs = (
    PerguntaDisputa.objects
    .filter(validada_em__isnull=False)
    .only(
        "pk", "respondida_em", "resposta_colab",
        "validada_em", "validada_por_id",
    )
    .order_by("pk")
)

sem_linha = 0
lote = []

def contar_lote(perguntas):
    return sum(
        1
        for linha in DecisaoPergunta.linhas_da_projecao_sem_linha(perguntas)
        if linha["ato"] == DecisaoPergunta.VALIDACAO
    )

for pergunta in qs.iterator(chunk_size=1000):
    lote.append(pergunta)
    if len(lote) == 1000:
        sem_linha += contar_lote(lote)
        lote = []

if lote:
    sem_linha += contar_lote(lote)

print("U1_carimbo_fora_de_identidade_global:", u1)
print("validada_projetada_sem_linha:", sem_linha)
print("U2: NAO MEDIDA — porta real F1 indisponivel nesta copia")
```

O contador de validação sem linha **não é U2**: ele mede a ausência de uma linha de validação no mesmo instante; U2 depende da sequência e do último ato **[não verificado para a implementação F1 histórica]**. [models.py:4135](app/chamados/models.py:4135)

alcance nao verificado restante: 4

_6893 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
