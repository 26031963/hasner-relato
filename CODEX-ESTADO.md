# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 16:20** · publicado em **05/11/2026 16:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 16:20 · sessao d9ecee284088 · - · - · origem=Ronald (tmux)

**Item 4 — fontes e efeitos exatos**

- `SessionStart` com matcher `compact` declara, em ordem, `cat .../HANDOFF-SESSAO.md` e `python3 .../marco_de_clear.py compact`; o matcher `clear` declara os mesmos comandos, com argumento `clear`. `.claude/settings.json:34-60`.
- `PreCompact` declara `bash .../bin/handoff_sessao.sh` tanto para `manual` como para `auto`. `.claude/settings.json:14-32`.
- O HANDOFF da cópia mede **54 linhas e 2.834 bytes** (`wc -lc`, conteúdo em `app/docs/HANDOFF-SESSAO.md:1-54`). O arquivo contém a fotografia datada de **05/11/2026 10:32**, inclusive seus próprios HEAD/origin/main registrados; são conteúdo da fotografia. `app/docs/HANDOFF-SESSAO.md:3-16`.
- O `cat` declarado despeja esse arquivo inteiro; no caso do conteúdo da cópia, 54 linhas/2.834 bytes por execução. `.claude/settings.json:40`; `app/docs/HANDOFF-SESSAO.md:1-54`.
- O teto declarado e aplicado pelo gerador é **60 linhas**, sem teto de bytes: `TETO=60`; se exceder, grava as primeiras 59 e acrescenta aviso de corte como linha 60; caso contrário move o temporário inteiro para a saída. `bin/handoff_sessao.sh:22`; `bin/handoff_sessao.sh:293-302`.

**`marco_de_clear.py compact`**

- **Não imprime nada**; o `main` não contém emissão e retorna 0, inclusive após exceções absorvidas. `bin/marco_de_clear.py:10-11`; `bin/marco_de_clear.py:23-66`.
- Lê stdin como JSON; payload ilegível ou diferente de objeto vira `{}`. O argumento fixa `origem="compact"`; `source` e `session_id` continuam vindos do payload, com string vazia como fallback. `bin/marco_de_clear.py:25-32`; `bin/marco_de_clear.py:44-45`.
- Determina a raiz por `HOOK_STOP_RAIZ` ou pela localização do script e escreve `logs/sessao/ultimo_clear.json`. `bin/marco_de_clear.py:19-20`.
- Cria o diretório e abre/cria `ultimo_clear.json.lock` com modo `0600` e `O_NOFOLLOW`; toma `flock` exclusivo e só então lê a hora atual. `bin/marco_de_clear.py:36-43`.
- O objeto gravado tem `hora` epoch, `iso` local (`%Y-%m-%d %H:%M:%S`), `origem`, `session_id`, `source` e `clear_horas`. `bin/marco_de_clear.py:43-57`.
- Lê o marco anterior via `orcamento_contexto.abre`, preserva somente valores numéricos de `clear_horas`, conserva os últimos 20 e **só acrescenta uma nova hora à lista quando `origem == "clear"`**. Portanto `compact` atualiza o marco e conserva a lista sem inserir seu próprio horário nela. `bin/marco_de_clear.py:47-57`.
- Grava JSON em temporário `.ultimo_clear.*` no mesmo diretório e substitui o marco com `os.replace`; fecha a trava em `finally`; falhas são silenciadas. `bin/marco_de_clear.py:58-66`.
- O leitor `abre` exige arquivo regular, usa `O_NOFOLLOW|O_NONBLOCK` e recusa links/FIFOs etc. `bin/orcamento_contexto.py:68-81`.

**O que `PreCompact` faz**

- Determina a raiz pelo caminho do script, saída `app/docs/HANDOFF-SESSAO.md`, temporário `${TMPDIR:-/tmp}/handoff_sessao.$$`; produz o documento por comandos e captura stdout **e stderr** do bloco nesse temporário. `bin/handoff_sessao.sh:17-27`; `bin/handoff_sessao.sh:291`.
- Coleta HEAD, executa **`git fetch -q`**, lê origin/main, conta commits a empurrar e captura até oito linhas de `git status --short`. `bin/handoff_sessao.sh:30-41`.
- Importa `hook_stop_fila1`, lê BACKLOG, pergunta ao hook a ORDEM VIVA e o próximo item da fila. `bin/handoff_sessao.sh:44-78`.
- Lê todo o bloco inicial do RELATO até a primeira seção `## `; seleciona `PAREI`, `lei:` e `!:`, imprime até quatro linhas cortadas a 220 caracteres e obtém do hook os vereditos “trava o item em curso?”/“devolve o turno?”. `bin/handoff_sessao.sh:85-125`.
- Lê PROMPTS do dia, cruza os IDs com BACKLOG e consulta `_item_fechado`; imprime até seis avais mais recentes ainda não fechados e contagem dos demais. `bin/handoff_sessao.sh:128-171`.
- Lê a frase de placar em `app/core/placar_estrutural.py`, itens abertos da raia no BACKLOG, branches `raia20-*` com commits à frente de main e o último aval `COTA-*` no PROMPTS. `bin/handoff_sessao.sh:174-211`.
- Verifica as travas declaradas `/tmp/juliani_db_test.lock` e `logs/sombra.lock` via `flock -n`; lê `logs/deploy.stamp` se existir. `bin/handoff_sessao.sh:221-240`.
- Executa **`gerar_avais.py`**, lê seu contador e até oito itens de `AVAIS.md`. `bin/handoff_sessao.sh:243-250`. Esse comando lê `app/docs/PENDENTES_RONALD.json` e **reescreve `app/docs/AVAIS.md`**; sem `--escrever`, como nesta chamada, não grava a normalização no JSON. `bin/gerar_avais.py:29-31`; `bin/gerar_avais.py:94-108`.
- Se não houver próximo item livre, consulta `travas_da_fila()` e imprime até 12 itens travados; por fim consulta novamente o próximo item e imprime o próximo passo. `bin/handoff_sessao.sh:255-290`.
- Aplica o corte de 60 linhas, substitui o HANDOFF e imprime a mensagem `handoff_sessao: <caminho> (<N> linhas, teto 60)`. O código não protege a seção final contra o corte: retém literalmente as primeiras 59 linhas mais o aviso quando necessário. `bin/handoff_sessao.sh:293-302`.

**Limite factual:** o que efetivamente será incorporado pelo runtime e se esses hooks estiveram ativos na sessão não é demonstrado por esses arquivos; o código comprova as declarações e os efeitos dos comandos quando executados. `[não verificado]`

## 05/11 16:18 · sessao fa0a1a80f2f8 · LASTRO2 · censo · origem=Code

**Fora de `TIPO_CHOICES` não significa fora do catálogo válido.** A criação valida pelo catálogo vivo e exclui deliberadamente `tipo` do `full_clean` do modelo. Portanto, um código cadastrado pode ser gravado mesmo ausente dos choices. (`app/ponto/services/ausencia.py:193-195,385-387`; `app/ponto/catalogo/ausencias.py:240-285`)

O dado fornecido sobre a ausência **5356** não informa seu `data`, nem o cadastro de `TipoAusencia` vigente na criação. Sua competência e a validade de `'folga'` naquele momento são **[nao verificado]**.

1. Os escritores e o caminho de `'folga'`

| Escritor operacional | Valida antes de gravar? | Evidência |
|---|---|---|
| `criar_ausencia`: constrói `Ausencia(**kwargs)` e chama `save()` completo | **Sim, catálogo vivo**; não exige presença em `TIPO_CHOICES` | `app/ponto/services/ausencia.py:193-195,341-345,385-387,418` |
| `editar_ausencia_registro`: atribui `ausencia.tipo = tipo` e salva | **Sim, catálogo vivo**; não exige choices | `app/ponto/services/ausencia.py:473-475,497-508` |
| `rederivar_fim`: chama `save()` completo | **Não valida tipo**; regrava o valor carregado | `app/ponto/services/ausencia.py:1277-1282`; chamador: `app/ponto/management/commands/curar_ausencia_sem_fim.py:42-46` |
| `Ausencia.save`, persistência comum aos anteriores | **Não valida catálogo nem choices**; deriva carimbo, minutos e intervalo, depois chama `super().save()` | `app/ponto/models.py:866-906` |

Os demais `save(update_fields=...)` operacionais não incluem `tipo`: prazo, estado, documento, descrição e decisão. O `QuerySet.update` existente altera somente estado/carimbo. (`app/ponto/services/ausencia.py:536,807-819,843,896-911,1114,1124,1132,1265-1267`)

Não apareceu escritor operacional de `tipo` por `update`, `bulk_update` ou `bulk_create` no censo da cópia. O varredor contempla essas operações e separa testes/migrations; essa conclusão não cobre execução externa arbitrária. (`app/core/censo_escritas.py:54-82`) Execução externa em produção: **[nao verificado]**.

Os caminhos que chegam aos escritores validados são:

| Entrada | Destino e validação |
|---|---|
| Lançamento pelo admin da aplicação | Recebe `tipo` do POST e chama `criar_ausencia`, já aprovada. `app/ponto/views.py:2368,2405-2411` |
| Submissão PWA | Chama `criar_ausencia`. `app/ponto/views.py:2526` |
| API do aplicativo | Chama `criar_ausencia`; traduz apenas aliases legados de atestado/luto. `app/api/views.py:1768-1771,1820` |
| Copiloto | Chama `criar_ausencia`, aprovada. `app/ponto/services/checar_ausencia.py:43-48` |
| Veredito humano da célula | Passa `falta` ou `tipo_ausencia` à criação. `app/ponto/services/veredito_celula.py:311-317` |
| Materialização de disputa | Passa tipo do mapa/argumento à criação. `app/chamados/services/materializacao.py:484,519` |
| Regularização | Passa `falta` por `garantir_ausencia`. `app/ponto/services/regularizacao.py:118-122` |
| Abandono provisório | Passa `falta` por `garantir_ausencia`. `app/ponto/management/commands/marcar_abandono_provisorio.py:76-84` |
| Espelhamento de férias | Passa literalmente `ferias` à criação. `app/ponto/services/ausencia.py:1186-1191` |
| Wrapper idempotente | `garantir_ausencia` delega à criação validada. `app/ponto/services/ausencia.py:453-462` |
| Edição HTTP | Chama `editar_ausencia_registro`. `app/ponto/views.py:2784-2792` |

O formulário também valida pelo catálogo vivo, e não pelos choices. O Django admin atual proíbe adicionar, alterar e excluir `Ausencia`. (`app/ponto/forms.py:60-68`; `app/ponto/admin.py:37-47`; `app/core/admin.py:59-75`)

**Nenhum dos escritores identificados fixa literalmente `'folga'` como `Ausencia.tipo`.** Eles recebem o tipo como argumento; o produtor de férias fixa `ferias`. (`app/ponto/services/ausencia.py:130-134,342,465-467,497,1187`)

Existe, porém, um caminho dinâmico para gravar `'folga'`:

- O cadastro aceita código derivado do POST ou nome, verificando vazio e duplicidade; pode, portanto, cadastrar código `folga` ativo. (`app/ponto/views_tipos_ausencia.py:80-94`; `app/ponto/services/ausencia.py:1285-1294`)
- Tipos ativos entram em `codigos_validos()`. Se `folga` estiver nesse conjunto, a criação e a edição o aceitam. (`app/ponto/catalogo/ausencias.py:249-252,267-285`; `app/ponto/services/ausencia.py:194,474`)
- Os selects de lançamento, submissão e edição recebem valores dinâmicos; o JS envia `FormData` sem traduzir o tipo. A API também anuncia o catálogo vivo. (`app/templates/ponto/lancar_ausencia.html:26-34`; `app/templates/ponto/submeter_ausencia.html:21-30`; `app/templates/ponto/partials/painel_ausencia.html:134-138`; `app/templates/ponto/ausencias.html:249`; `app/api/views.py:1867`)

Existência, efeito e estado ativo de um cadastro `folga` em produção: **[nao verificado]**.

Os literais `tipo='folga'` encontrados no aceite de disputa seguem outro destino: `declarar_dia` despacha para `lancar_folga(..., 'FD')`, sem criar `Ausencia`. (`app/chamados/services/validacao.py:280,291`; `app/ponto/services/declarar_dia.py:167,179-191`)

A porta central emite exatamente auditoria `acao='criar', modelo='Ausencia'`, mas isso não identifica seu chamador. Atribuir a ausência 5356 a um endpoint específico permanece **[nao verificado]**. (`app/ponto/services/ausencia.py:441-448`)

2. Os leitores e o tratamento do desconhecido

Nesta tabela, **desconhecido** significa ausente do catálogo retornado por `vigente()`. Esse catálogo usa `TipoAusencia` ativo, com semente como fallback quando a leitura falha ou fica vazia. (`app/ponto/catalogo/ausencias.py:240-254,267-285`)

| Leitor | O que faz com tipo desconhecido | Evidência |
|---|---|---|
| Catálogo de efeitos e `Ausencia.efeito` | Efeito `None`; rótulo é o código; duração padrão `por_lei`; fica fora dos conjuntos de abono, desconto, bloqueio, parcial, médico, documento e intervalo indeterminado | `app/ponto/catalogo/ausencias.py:192-195,293-310,326-358,385-387,424-467`; `app/ponto/models.py:837-840` |
| `precedencia.faixa_de` | Cai em **`atestado`**. Isso também acontece com literal `folga`: o mapa só associa `folga_compensatoria` e `feriado_folga` à faixa folga | `app/ponto/precedencia.py:52-79` |
| `fatos_do_periodo` | Inclui a ausência não rejeitada, preserva o código e a coloca na faixa atestado; pode vencer feriado, folga e trabalho | `app/ponto/precedencia.py:273-279,336-339,379-405` |
| Cobertura, sobreposição, expansão e vigência | Pode cobrir a data inicial ou intervalo fechado. Sem `data_fim`, desconhecido cobre somente a data inicial. Folha exige aprovada; cobrança aceita não rejeitada | `app/ponto/turnos.py:2509-2518,2529-2533,2552-2557,2753-2790,2815-2824,3087-3095` |
| Parcial/integral | Não é parcial. Sendo diferente de `falta`, explica o dia inteiro; porém não entra no mapa de integrais **abonados** enviado ao motor, que exige efeito `ABONA` | `app/ponto/turnos.py:2595-2600,2668-2690` |
| Afastamentos | Não entra no universo restrito a `afastamento_inss` | `app/ponto/turnos.py:2702-2719` |
| Grade | Aprovada vira `tipo_dia='ausencia'`, preserva código/id, marca parcial falso e limpa células/órfãs; pode sobrepor uma folga da grade | `app/escala/utils.py:1208-1212,1231-1251,1272-1274` |
| Cartório | Copia precedência para a ata; se aprovada e integral, dispensa lâmpadas usando o código literal em `dispensada` | `app/ponto/services/cartorio.py:766-778,1947-1956` |
| Suprajuiz | Usa cobertura e parcialidade sem revalidar código; pode impedir cobrança de furo ou classificar trabalho/cobrança em ausência, conforme os demais insumos | `app/ponto/supra_juiz.py:255-269,304-316,400-405,530-534` |
| **Fechamento financeiro** | **Suprime o previsto**, porque `None != DESCONTA`; não abona nem concede crédito de minutos pagos | `app/ponto/services/fechamento.py:664-669,684-692,720-723` |
| **Espelho/PDF — coletor de efeitos** | **Não justifica nem suprime**: exige comparação explícita com `ABONA` ou `SUPRIME`. Diverge do fechamento para efeito `None` | `app/ponto/services/efeito_ausencia.py:43-61`; consumidores: `app/ponto/services/espelho.py:1068`; `app/relatorios/pdf_espelho.py:332-333` |
| Motor e calculador de pontualidade | Recebem o mapa de integrais abonados; desconhecido não obtém por esse mapa o teto que zera atraso/saída. O código do tipo aparece apenas na mensagem desse ramo | `app/ponto/turnos.py:2684-2690`; `app/ponto/services/fechamento.py:742`; `app/ponto/motor_calculo_v2.py:1085-1105`; `app/ponto/calculador/regras.py:117-120` |
| Export de faltas/8792 | Não entra: seleção exige tipo do conjunto `descontam_vigentes()` | `app/folha/export.py:75-81,127-131,168-170` |
| Export de abonos/8932 | Não entra: seleção exige tipo abonado pelo catálogo | `app/folha/export.py:182-195` |
| **Veredito visual** | **Retorna `ABONADO` para efeito `None`**, salvo rejeitada; apresentação de aberta tem ramo pendente | `app/ponto/services/dia_decidido.py:447-450,460-481` |
| Status do dia | Faixa atestado com efeito `None` resulta em `ausencia`; somente `DESCONTA` vira falta nesse ramo | `app/ponto/status_dia.py:296-319` |
| Classificação/materialização de cobranças | Ausência desconhecida não parcial pode calar classificação de falta e pergunta; cobertura diferente de `falta` também suspende adesão/furos | `app/escala/servico_jornada.py:340-345`; `app/chamados/services/materializacao.py:78-82`; `app/chamados/services/adesao.py:68-70`; `app/escala/services/furos_diarios.py:223-224`; `app/ponto/management/commands/reconciliar_grade.py:402` |
| Estado, documento e decisão | Estado inicial: aguardando decisão, sem prazo documental. Desconhecido não exige documento e não é considerado contradição documental por desconto | `app/ponto/services/ausencia.py:591-618,763-781`; `app/ponto/models.py:723-729`; `app/ponto/services/atestados_lote.py:25-29` |
| Crédito parcial e aviso na batida | Não recebe crédito de tipo pago nem aviso reservado aos bloqueantes | `app/ponto/services/credito_parcial.py:23-26`; `app/ponto/services/aviso_da_batida.py:77-92` |
| Universo médico | Fica fora dos alertas e relatórios restritos aos tipos médicos | `app/ponto/services/alertas_ausencia.py:49-50`; `app/ponto/views.py:2223,2830`; `app/relatorios/views.py:282` |
| Inteligência | Resumo exclui dos contadores de abono/desconto; score inclui não rejeitada como não abonada, penalizando 10 pontos; alertas recorrentes incluem no limiar de três registros | `app/inteligencia/resumo.py:41-44`; `app/inteligencia/management/commands/calcular_scores.py:11-16`; `app/inteligencia/management/commands/gerar_alertas.py:54-61` |
| Vigias | Conta tipo fora do catálogo entre não rejeitadas; desconhecido aprovado também participa das auditorias de discordância e ausência sem efeito | `app/ponto/services/vigia_ausencia.py:102-105,180-184`; `app/ponto/services/vigia_ausencia_veredito.py:49-57`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44-60` |
| Rótulo/ícone | Código cru, rótulo curto limitado e ícone padrão de atestado | `app/ponto/catalogo/ausencias.py:175-181,298-300` |

Essas são consequências condicionais do código. Efeito financeiro, ata ou export efetivamente associados à ausência 5356: **[nao verificado]**.

3. Uma sonda somente de leitura

O recorte abaixo conta **registros pela competência que contém `Ausencia.data`**, usando a empresa atual do colaborador. Não soma dias nem todas as competências atravessadas pelo intervalo. A janela vem de `janela_atual`; entrega vem exclusivamente de `competencia_entregue`. (`app/ponto/models.py:750-754`; `app/colaboradores/models.py:321`; `app/ponto/janelas.py:60-63`; `app/folha/export.py:545,569-575`)

A saída separa `fora_choices` e `fora_catalogo`, inclui todos os status e informa registrador somente por ID, agrupado por mês de criação. Os campos correspondentes estão em `app/ponto/models.py:690-705,799-811`. Empresa histórica na data do fato: **[nao verificado]**.

Código para o Python recebido por `bin/sonda_leitura.sh`, que já abre shell Django no schema `juliani`. O wrapper também escreve credencial/trilha; por isso, sua execução não integra esta resposta sob a regra absoluta de nenhum arquivo alterado. (`bin/sonda_leitura.sh:126-143,235-240,248-250`) Números de produção: **[nao verificado]**.

```python
import json
from collections import Counter

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa
from ponto.models import Ausencia, TipoAusencia
from ponto.catalogo import ausencias as CAT
from ponto.janelas import janela_atual
from folha.export import competencia_entregue

NM = "nao medido"


def medir():
    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cur.execute("SELECT current_user, current_schema()")
            if cur.fetchone() != ("leitor", "juliani"):
                raise RuntimeError("contexto_incorreto")

        # app/ponto/catalogo/ausencias.py:240-285.
        # A leitura explícita não deixa falha de SELECT parecer catálogo vazio.
        ativos = set(
            TipoAusencia.objects.filter(ativo=True)
            .values_list("codigo", flat=True)
        )
        CAT.invalidar_cache()
        catalogo = CAT.codigos_validos()
        if ativos and catalogo != ativos:
            raise RuntimeError("catalogo_inconsistente")

        choices = {codigo for codigo, _ in Ausencia.TIPO_CHOICES}

        # app/ponto/models.py:743-811. Nenhum nome/CPF selecionado.
        campos = (
            "tipo", "status", "data", "registrado_por_id", "criado_em",
            "colaborador__empresa_id",
        )
        # União dos dois universos; sem filtro de status ou período.
        linhas = list(
            Ausencia.objects.order_by()
            .exclude(tipo__in=sorted(choices & catalogo))
            .values(*campos)
        )
        ids = {a["colaborador__empresa_id"] for a in linhas}
        empresas = {
            e.pk: e
            for e in Empresa.objects.filter(pk__in=ids)
            .only("pk", "dia_inicio_competencia")
        }

        grupos, origens = Counter(), Counter()
        totais, entregas = Counter(), {}

        for a in linhas:
            universos = []
            if a["tipo"] not in choices:
                universos.append("fora_choices")
            if a["tipo"] not in catalogo:
                universos.append("fora_catalogo")

            criado = a["criado_em"]
            if criado is None:
                raise ValueError("criacao_ausente")
            if timezone.is_aware(criado):
                criado = timezone.localtime(criado)
            mes_criacao = criado.strftime("%Y-%m")

            eid = a["colaborador__empresa_id"]
            if eid is None or eid not in empresas:
                raise ValueError("empresa_ausente")

            # app/ponto/janelas.py:60-63.
            ini, fim = janela_atual(a["data"], empresas[eid])
            chave = (eid, fim.month, fim.year)
            if chave not in entregas:
                # app/folha/export.py:545,569-575; somente SELECT.
                entregas[chave] = competencia_entregue(*chave)

            for universo in universos:
                totais[universo] += 1
                grupos[
                    (universo, a["tipo"], a["status"], eid, ini, fim)
                ] += 1
                origens[
                    (universo, a["tipo"], a["registrado_por_id"], mes_criacao)
                ] += 1

        detalhes = []
        totais_entregues = Counter()
        for (u, tipo, status, eid, ini, fim), n in sorted(grupos.items()):
            entregue = entregas[(eid, fim.month, fim.year)]
            # Zero somente após False medido pelo juiz.
            ne = n if entregue else 0
            totais_entregues[u] += ne
            detalhes.append(dict(
                universo=u,
                tipo=tipo,
                status=status,
                empresa_id=eid,
                competencia=fim.strftime("%Y-%m"),
                janela_inicio=ini.isoformat(),
                janela_fim=fim.isoformat(),
                quantidade=n,
                em_competencia_entregue=ne,
            ))

        return dict(
            fonte_catalogo=(
                "cadastro_ativo" if ativos else "semente_cadastro_vazio"
            ),
            totais={
                u: dict(
                    quantidade=totais[u],
                    em_competencia_entregue=totais_entregues[u],
                )
                for u in ("fora_choices", "fora_catalogo")
            },
            por_tipo_status_empresa_competencia=detalhes,
            por_tipo_registrador_mes=[
                dict(
                    universo=u,
                    tipo=t,
                    registrado_por_id=uid,
                    mes_criacao=mes,
                    quantidade=n,
                )
                for (u, t, uid, mes), n in sorted(origens.items())
            ],
            impacto_financeiro=NM,
            outras_competencias_atravessadas_pelo_intervalo=NM,
        )


try:
    resultado = medir()
except Exception as exc:
    # Não publica resultados parciais nem transforma falha em zero.
    # Não imprime mensagem, SQL, objetos ou traceback.
    resultado = dict(
        totais=NM,
        por_tipo_status_empresa_competencia=NM,
        por_tipo_registrador_mes=NM,
        em_competencia_entregue=NM,
        erro_classe=type(exc).__name__,
    )

print(json.dumps(resultado, ensure_ascii=False, sort_keys=True))
```

## 05/11 16:15 · sessao 462f84cc4015 · CODEX-SUFOCA-20-futurologia · leitura · origem=Code

**A falha mais previsível é em 14/11: vence a exceção que hoje permite instalar o crontab apesar de a sombra não caber no horário.** Também encontrei falhas condicionais em remarcação de férias, publicação de recibos adicionais e acompanhamento da competência após 21/11. (`app/core/tests/marca_cron15.py:24`; `app/core/management/commands/crons.py:97`; `app/ferias/services.py:393`; `app/holerite/motor.py:344`; `app/folha/management/commands/pre_fechamento.py:89`)

Recorte adotado: 05/11/2026 a 05/12/2026; quatro semanas terminam em 03/12. Natal e virada de ano entram como extrapolação solicitada. A data inicial vem do ambiente, sem comprovação documental na cópia. [nao verificado]

Os contra-exemplos abaixo são sintéticos: o anexo fornece autoridades, mas não cadastros, batidas ou estados individuais que permitam afirmar ocorrência em produção. [nao verificado]

1. **14/11 — instalação de crons perde a exceção temporária.**

   A sombra tem duração registrada de **5.188 segundos**; a derivação exige início às 03:56, anterior ao piso 04:05, e levanta `CronNaoCabe`. Executei essa função em memória e obtive essa recusa. A marca vale até 13/11 inclusive; depois disso, o render de instalação propaga a exceção mesmo recebendo a linha válida do host. (`app/config/crons_duracao.json:78`; `app/config/crons.py:266`; `app/config/crons.py:270`; `app/core/tests/marca_cron15.py:52`; `app/core/management/commands/crons.py:97`)

   **Contra-exemplo:** manter esta cópia e sua duração; em 14/11 executar `bin/crons.sh install` com crontab válido. O render falha e o script sai antes da instalação. Os 15 testes nomeados também perdem `expectedFailure`. Isso não demonstra interrupção dos crons já instalados. (`bin/crons.sh:23`; `app/core/tests/marca_cron15.py:29`; `app/core/tests/marca_cron15.py:57`)

   **Certeza alta; dono ESTRUTURA. Especulação:** uma instalação necessária após o vencimento ficará bloqueada se a duração continuar nessa condição; execução futura e duração real não foram verificadas. [nao verificado]

2. **Remarcar férias pode deixar novembro coberto por férias que foram transferidas.**

   A edição comum troca as datas e salva; o espelhamento consulta ausências somente dentro do intervalo **novo**, deixando as antigas fora da limpeza. A porta distinta de correção administrativa contém explicitamente a limpeza do intervalo anterior. (`app/ferias/views.py:667`; `app/ferias/services.py:399`; `app/ferias/models.py:434`; `app/ponto/services/ausencia.py:1165`; `app/ferias/services.py:383`)

   **Contra-exemplo:** em 05/11, agendamento futuro de 10–23/11, com 14 ausências de férias já materializadas; remarcar pela edição comum para 01–14/12, com saldo suficiente e sem colisão. As ausências de novembro permanecem e podem nascer as de dezembro. O leitor de agendamentos passa a enxergar somente dezembro, enquanto a precedência ainda recebe as ausências de novembro. (`app/ferias/services.py:401`; `app/ponto/services/ausencia.py:1182`; `app/ponto/services/ausencia.py:1195`; `app/ferias/services.py:51`; `app/ponto/precedencia.py:336`)

   **Certeza alta pela leitura estática; dono ESTRUTURA. Especulação:** remarcações para o recesso ampliarão esse estado inconsistente; existência desses agendamentos em produção é [nao verificado].

3. **13º em lote posterior pode ser descartado como recibo já publicado.** [nao verificado]

   O importador considera existente qualquer holerite do mesmo colaborador, mês e ano; não distingue mensal de 13º nessa decisão. O novo recibo recebe `ja_publicado`, e a gravação publica somente itens `ok`. (`app/holerite/motor.py:323`; `app/holerite/motor.py:344`; `app/holerite/motor.py:467`)

   **Contra-exemplo:** mensal de 11/2026 já publicado; depois, importar recibo reconhecido pelo parser da primeira parcela do 13º para a mesma pessoa e competência 11/2026. O segundo documento é pulado. A ordem inversa também bloqueia o documento posterior. Dois recibos no mesmo primeiro lote podem receber sequências diferentes; o problema é a carga posterior. (`app/holerite/motor.py:323`; `app/holerite/motor.py:345`; `app/holerite/motor.py:471`)

   **Certeza alta para esse fluxo; dono ESTRUTURA. Especulação:** o DP enviará o 13º em lote separado usando a mesma competência. Layout, procedimento real e pagamento do 13º são [nao verificado].

4. **21/11 — pré-fechamento muda para competência 12 e deixa a pauta 11 congelada.**

   O contador aceita somente chamados da janela corrente. A atualização das pautas também filtra exclusivamente a âncora corrente; prazo vencido não retira uma pauta de `vivas()`. (`app/chamados/services/fila_por_causa.py:375`; `app/folha/management/commands/pre_fechamento.py:89`; `app/pautas/models.py:57`)

   **Contra-exemplo:** corte 21; em 20/11, um chamado sobre 19/11 na gaveta `respondido_a_decidir` gera pauta 11 com “1 bloqueio”. Em 21/11, sem chamados da competência 12, o contador retorna zero, mas só alcança pautas da âncora 12. Resolver o chamado antigo em 22/11 não atualiza a pauta 11 por esse comando. (`app/chamados/services/fila_por_causa.py:343`; `app/chamados/services/fila_por_causa.py:378`; `app/folha/management/commands/pre_fechamento.py:91`)

   **Certeza alta do mecanismo, média do defeito operacional**, pois o escopo corrente é explícito. **Dono ESTRUTURA. Especulação:** o DP tomará a pauta antiga como retrato atualizado; ocorrência real é [nao verificado]. (`app/chamados/services/fila_por_causa.py:355`)

5. **21–30/11 — mapa diário continua examinando competência 11.**

   O cron passa mês e ano civis do servidor; o comando usa esses argumentos para construir as janelas examinadas. Para corte 21, a competência corrente já será 12 nesse intervalo. (`app/config/crons.py:644`; `app/ponto/management/commands/mapa_divergencia.py:44`; `app/ponto/management/commands/mapa_divergencia.py:93`; `app/ponto/janelas.py:27`)

   **Contra-exemplo:** divergência entre leitores em um dia de 23/11, pertencente à competência 12. A execução de 24/11 recebe `--mes=11`, cuja janela termina em 20/11; esse dia fica fora deste vigia até a mudança do mês civil. (`app/config/crons.py:644`; `app/ponto/janelas.py:16`; `app/ponto/management/commands/mapa_divergencia.py:104`)

   **Certeza alta; dono ESTRUTURA. Especulação:** uma divergência nova demorará a aparecer por esse instrumento; cobertura pelos demais vigias é [nao verificado].

6. **15/11 e 20/11 — feriado depende de linha cadastrada, não da tabela de importação.**

   A tabela admite essas datas, mas o juiz consulta `Feriado` pela data completa. A allowlist não cria automaticamente as linhas. (`app/core/views_config.py:118`; `app/core/views_config.py:119`; `app/core/views_config.py:141`; `app/core/feriados.py:27`)

   **Contra-exemplo:** 20/11/2026 sem linha nacional cadastrada; colaborador 5x2, célula prevista de trabalho, sem ausência nem folga. O juiz não reconhece feriado e a precedência conserva trabalho. Ausência de batidas pode então alimentar cobrança conforme o restante do fluxo. (`app/core/feriados.py:82`; `app/core/feriados.py:95`; `app/ponto/precedencia.py:341`; `app/ponto/precedencia.py:382`; `app/ponto/precedencia.py:386`)

   **Certeza alta do reconhecimento incorreto nesse cadastro; dono CADASTRO. Especulação:** faltarão linhas em novembro; o anexo não mostra a tabela efetivamente cadastrada. [nao verificado]

Não consegui verificar cadastros de feriados, aplicações CCT, agendamentos, lotes de holerites, crontab instalado, execução de comandos ou quantidade de pessoas atingidas; esses dados não constam do anexo. [nao verificado]

Sobre os demais pontos pedidos:

- **CCT até 31/01/2028:** o literal está em comentário; a execução lê vigências cadastradas, inclusivamente. Com fim efetivo nessa data, não há vencimento dentro do recorte. Vigência ausente, vencida ou aplicação empatada pode levar ao piso legal; os registros reais são [nao verificado]. (`app/core/regua_cct.py:130`; `app/core/regua_cct.py:261`; `app/core/regua_cct.py:269`; `app/core/regua_cct.py:366`)
- **Férias coletivas:** a porta de lote usa o julgamento individual; uma primeira fração de dez dias, sem outra de pelo menos 14, é recusada. Não há dado fornecido que permita concluir adequação jurídica ou ocorrência de coletivas nesse formato. (`app/ferias/views.py:569`; `app/ferias/models.py:264`; `app/ferias/models.py:361`) [nao verificado]
- **Virada de ano:** as funções puras examinadas tratam dezembro/janeiro; 21/12/2026 e 01/01/2027 pertencem à janela 21/12–20/01. Isso não certifica todos os consumidores. (`app/ponto/janelas.py:17`; `app/ponto/janelas.py:27`)
- **Datas literais:** o censo executado retornou zero `DECIDE`, mas exclui arquivos de testes — precisamente onde está o prazo CRON-15 importado pelo comando de produção. Portanto, esse zero não afasta a falha de 14/11. (`app/core/datas_cravadas.py:25`; `app/core/datas_cravadas.py:86`; `app/core/management/commands/crons.py:92`)

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| Exceção dos crons vence | `app/core/management/commands/crons.py:97` | Instalar em 14/11 mantendo duração 5.188 s | Alta | ESTRUTURA |
| Férias antigas sobrevivem à remarcação | `app/ferias/services.py:399`; `app/ponto/services/ausencia.py:1165` | Mover 10–23/11 para 01–14/12 | Alta, estática | ESTRUTURA |
| Recibo adicional é pulado | `app/holerite/motor.py:344` | Mensal 11 publicado; 13º 11 em lote posterior | Alta, condicional | ESTRUTURA |
| Pauta 11 deixa de ser atualizada | `app/folha/management/commands/pre_fechamento.py:89` | Resolver em 22/11 chamado que gerou pauta em 20/11 | Alta mecanismo; média impacto | ESTRUTURA |
| Vigia fica na competência anterior | `app/config/crons.py:644` | Divergência de 23/11 fora da janela examinada em 24/11 | Alta | ESTRUTURA |
| Feriado ausente vira trabalho | `app/core/feriados.py:27`; `app/ponto/precedencia.py:386` | 20/11 sem linha nacional, célula de trabalho | Alta, condicional | CADASTRO |

## 05/11 16:15 · sessao 242c2f748e9d · CODEX-SUFOCA-18-backlog · leitura · origem=Code

**Ainda há riscos de cálculo e exportação:** exclusão por demissão fora da janela, falta fixada em 720 minutos, crédito de atraso sobrescrito e erros no recorte noturno. As evidências e os limites estão na tabela abaixo (`app/folha/export.py:840`; `app/chamados/services/materializacao.py:519`; `app/ponto/services/dia_pago.py:328`; `app/ponto/motor_calculo_v2.py:110`).

A tabela delimita o que consegui julgar; **não constitui fechamento dos cerca de 161 itens** [nao verificado]. Nenhum arquivo foi alterado. Não executei a suíte; houve reproduções isoladas em memória de funções e trechos do motor. Commits de correção, implantação e resultados em produção: **[nao verificado]**. [nao verificado]

Todos os contraexemplos abaixo são **sintéticos**, salvo os casos explicitamente identificados como relatos documentais não reproduzidos. “JA RESOLVIDO” significa resolução no código citado; quando houve reenquadramento como conforme, isso aparece expressamente. O dono L-099 refere-se ao mecanismo demonstrado, sem atribuir dono a colaboradores reais: o juiz depende também dos insumos de cadastro e batidas (`app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:110`).

Não consegui verificar frequência, dinheiro efetivamente pago, passivo corrigido ou estado atual dos casos nominais [nao verificado]. **Especulação:** nenhuma causa de produção foi escolhida entre hipóteses concorrentes.

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| **H12-DEMISSAO-NA-JANELA — AINDA VALE** | `app/folha/export.py:834`, `app/folha/export.py:840` | Competência 10/2026, janela 21/09–20/10, fechamento existente e demissão em **10/11**: já sai como `rescisao_modulo_proprio`. O ramo testa apenas demissão não nula, sem comparar a janela. | **Alta** | **ESTRUTURA** |
| **H06-FALTA-PELA-GRADE — AINDA VALE** | `app/chamados/services/materializacao.py:519`; `app/ponto/models.py:875`; `app/ponto/services/fechamento.py:865` | Jornada cadastrada de **480 min**, resposta `FALTA_DIA`, ausência nova: escritor passa **720 min**. Como os minutos já estão preenchidos, o modelo não deriva a jornada; o fechamento soma **12 h**, excedendo as 8 h previstas. | **Alta** | **ESTRUTURA** |
| **V3-LAVRAR-ATRASO-CREDITO — AINDA VALE** | `app/ponto/services/dia_pago.py:190`, `app/ponto/services/dia_pago.py:328`; `app/ponto/calculador/regras.py:855`; `app/ponto/services/fechamento.py:1261` | **Reproduzido em memória:** atraso 60 min, crédito 30 min, rubrica de atraso bruto 1 h. Primeiro resulta em **0,5 h**; depois a rubrica sobrescreve para **1 h**, embora o detalhe continue registrando crédito de 30 min. | **Alta** | **ESTRUTURA** |
| **V3-HE-NOTURNA-POR-PECA — AINDA VALE** | `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:109` | **Reproduzido:** turno 22–07h, intervalo 05h30–06h30, HE50 de 60 min, prorrogação ligada: split retorna **zero HE noturna**. A janela é recalculada pelo trecho extra após 05h, perdendo a prorrogação do turno original. | **Alta**, pela regra interna | **ESTRUTURA** |
| **V3-NOTURNO-RECORTE-LONGO — AINDA VALE** | `app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:116` | **Reproduzido:** 00h até 00h seguinte, prorrogação ligada, retorna **1.560 min para duração de 1.440 min**: faixa estendida sobrepõe 22–24h. Com 27 h retorna zero. Entrada efetiva de turnos acima de 26 h pelo pareador: [nao verificado]. | **Alta** para a função | **ESTRUTURA** |
| **V3-MOTOR-FERIADO-HE100-NOTURNA — AINDA VALE nos ramos Comercial/TurnoPartido** | `app/ponto/motor_calculo_v2.py:3323`, `app/ponto/motor_calculo_v2.py:3337`, `app/ponto/motor_calculo_v2.py:3352`; `app/ponto/motor_calculo_v2.py:3095` | **Bloco Comercial reproduzido:** período com 480 min, dobra de feriado ligada e split noturno preenchido. O bloco zera o split, grava HE100=480 e sai por `continue`, deixando **HE100 noturna=0**. Não generalizo para todos os motores. | **Alta** | **ESTRUTURA** |
| **V3-RETOMA-FILA, item 3: CCT pela data do dia — AINDA VALE no calculador** | `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/alimentacao.py:404`, `app/ponto/calculador/alimentacao.py:528` | Janela começa 21/10; aplicação da convenção muda em 01/11, mantendo a escala. Turno de 02/11 recebe flags resolvidas em **21/10**, pois elas são carregadas uma vez e reutilizadas por dia. Impacto numérico final: [nao verificado]. | **Alta** na seleção; **média** no impacto | **ESTRUTURA** |
| **V3-P5-PRORROGACAO-POR-INTERVALO — JA RESOLVIDO no desconto isolado** | `app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:191`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:111` | **Antigo contraexemplo reproduzido:** turno 22–07h, intervalo 05h30–06h30, prorrogação ligada. Agora desconta **60 min noturnos**, usando a janela do turno. Isso não resolve os dois defeitos de recorte acima. | **Alta** | **ESTRUTURA**, corrigida nesse ponto |
| **STATUS-ENCERRADO-DA-COMPETENCIA-NAO-EXISTE — AINDA VALE** | `app/ponto/models.py:399`; `app/folha/export.py:668`, `app/folha/export.py:698`, `app/folha/export.py:764` | O modelo oferece somente `aberto/aprovado/exportado`. **Hipótese rotulada:** introduzir `encerrado` sem migrar leitores pode produzir classificador dizendo `entra` e montador excluindo pela tupla de status. Nenhuma linha real com esse estado foi demonstrada. | **Alta** na ausência; **média** no efeito hipotético | **ESTRUTURA** |
| **COBPARC-AUSENCIA-CALA-DOIS-JUIZES — AINDA VALE** | `app/chamados/services/materializacao.py:78`; `app/chamados/juizes.py:446`, `app/chamados/juizes.py:475`; `app/ponto/turnos.py:2632` | Ausência parcial vencedora de 240 min, sem período, célula trabalhando e marco apagado: materializador responde **coberto** pela ausência sem período; `_celula_furada` não aplica essa absolvição e pode responder **furado** pela ata. | **Alta** | **ESTRUTURA** na divergência; ausência sem período é **CADASTRO** |
| **COBPARC-FUROS-LEITOR-PARCIAL — JA RESOLVIDO nos dois leitores nominais** | `app/escala/services/furos_diarios.py:42`, `app/escala/services/furos_diarios.py:49`; `app/inteligencia/calculadores.py:49`; `app/chamados/juizes.py:398` | Ata de ausência parcial, marco dispensado entre 08–12h e marco apagado às 17h: os dois leitores já incluem o dia pelo predicado comum. Os outros quatro sítios citados no item não foram julgados [nao verificado]. | **Alta**, escopo limitado | **ESTRUTURA**, corrigida nos dois leitores |
| **GRADE-FOLGA-GERADOR — AINDA VALE** | `app/escala/models.py:1280`, `app/escala/models.py:1309`, `app/escala/models.py:1365`; `app/ponto/precedencia.py:235` | Folga **manual** guardada no vínculo A encerrado, em data gerada pelo B ativo: juiz aceita a folga da pessoa; gerador de B lê apenas suas próprias fotos e pode produzir trabalho. | **Alta** | **ESTRUTURA** |
| **GRADE-FOLGA-C5 — AINDA VALE** | `app/chamados/management/commands/auditar_invariantes_chamados.py:1531`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1541`; `app/ponto/precedencia.py:237` | Folga **de calendário** de A encerrado, data coberta por B que prevê trabalho: juiz rejeita a folga antiga; auditor a inclui pela existência de B e acusa a célula correta. | **Alta** | **ESTRUTURA** do auditor |
| **FASE-CONFLITANTE-VINCULO-DO-DIA — AINDA VALE** | `app/escala/services/fase_conflitante.py:71`, `app/escala/services/fase_conflitante.py:80`; `app/escala/alimentacao.py:228` | B ativo começa em novembro; janela medida alcança outubro, gerado por A. O medidor aplica B e suas fotos também aos dias de outubro, sem escolher o vínculo de cada dia. | **Alta** | **ESTRUTURA** do instrumento |
| **JUSTIFICATIVA-ADMIN-FORA-DA-PORTA — AINDA VALE** | `app/ponto/admin.py:18`; `app/ponto/models.py:159`; `app/ponto/services/justificativa.py:199`, `app/ponto/services/justificativa.py:214`; `app/ponto/signals.py:161` | Usuário autorizado no Django Admin altera pendente para aprovada: contorna a porta que encerra origem, invalida fechamento e registra log de domínio. **Ressalva:** o signal ainda julga o dia; não é correto afirmar ausência absoluta de eventos ou trilha. | **Alta** | **ESTRUTURA** |
| **ISENTO-PONTO-JUIZ-REGISTRADO — AINDA VALE parcialmente** | `app/core/constants.py:13`; `app/ponto/services/cartorio.py:1140`, `app/ponto/services/cartorio.py:1229`, `app/ponto/services/cartorio.py:1419`; `app/docs/CORTES.md:419` | Há função declarada como juiz e leituras diretas da flag. **Hipótese de manutenção:** mudar a função sem mudar a flag faria impressão e classificação divergirem. Hoje são equivalentes. A alegação antiga de ausência de corte não vale integralmente: existe ISENTO-H10. | **Alta** nas leituras; **média** no restante do item | **ESTRUTURA** |
| **ISENTO-FLAG-ATEMPORAL — AINDA VALE na falta de vigência** | `app/colaboradores/models.py:294`; `app/core/constants.py:18`; `app/ponto/services/cartorio.py:520`, `app/ponto/services/cartorio.py:1354` | Alterar a flag hoje muda o insumo ao rejulgar outubro, sem data de início da isenção. **Não sustento “histórico inteiro”**: a impressão tem guarda `regra >= 4`. Alcance efetivo em competências exportadas: [nao verificado]. | **Alta** na atemporalidade | **ESTRUTURA** da representação; vigência individual [nao verificado] |
| **V-FASE-FILA-DONO-DEPENDE-DA-JANELA / V-FASE-DONO-AS-OF — JA RESOLVIDO no mecanismo descrito** | `app/ponto/management/commands/e6_oraculo.py:160`; `app/escala/services/fase_12x36.py:134`, `app/escala/services/fase_12x36.py:144` | Dia 10/10 coberto por lavra antiga; nova lavra mede só novembro. O leitor busca a lavra mais recente **que cobriu 10/10**, incluindo `itens_reabertos`; avançar a janela sozinho não descarta a medida antiga. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-01 — JA RESOLVIDO: conta vazia consultada ao juiz** | `app/folha/export.py:809`; `app/ponto/views.py:1297`, `app/ponto/views.py:1309` | Fechamento aberto com zero horas no lote cego: recebe `conta_vazia` do classificador e é separado pelo consumidor. A decisão deixou de ser query própria no botão. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-02 — JA RESOLVIDO: âncora consultada ao juiz** | `app/folha/export.py:787`, `app/folha/export.py:813`; `app/ponto/views.py:1171` | Colaborador ativo 12x36 sem âncora: juiz emite `sem_ancora_12x36`; pilula e aprovação consomem essa resposta. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-03 — JA RESOLVIDO: aprovação individual lê as travas** | `app/ponto/views.py:1481`, `app/ponto/views.py:1507`, `app/ponto/views.py:1510` | Foto gravada sem turno aberto, leitura atual com turno aberto: aprovação individual injeta a leitura no classificador e recusa pela trava atual. Não decide somente pela foto. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-04 — JA RESOLVIDO por enquadramento como conforme** | `app/ponto/views.py:1367`; `app/core/juizes.py:510`; `app/ponto/tests/test_contract_juiz_fechamento.py:117` | Aprovação parcial, com alguém pulado, não tranca a competência. O contrato registrado deixou de tratar essa condição como defeito. Não houve remoção desse `if`. | **Alta** no contrato da cópia | **ESTRUTURA**, pendência reenquadrada |
| **FECH-05 — JA RESOLVIDO por enquadramento como conforme** | `app/folha/views.py:244`, `app/folha/views.py:246`; `app/core/juizes.py:510` | Geração do TXT pode carimbar aprovados sem trancar. Isso é comportamento conforme ao contrato registrado; o carimbo já filtra as travas aplicáveis. | **Alta** no contrato da cópia | **ESTRUTURA**, pendência reenquadrada |
| **FECH-06 — JA RESOLVIDO: botão não desaparece por existir aprovado** | `app/ponto/views.py:1169`, `app/ponto/views.py:1170` | Uma linha aprovada e outra aberta sem trava: botão continua disponível por `any` sobre os abertos elegíveis. A condição antiga `total_aprovados == 0` não decide mais. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-07 — JA RESOLVIDO por enquadramento como conforme** | `app/ponto/services/fechamento.py:1121`, `app/ponto/services/fechamento.py:1144`; `app/core/juizes.py:515` | Fechamento aprovado pode receber atualização dos campos calculados, preservando o carimbo. Esse comportamento permanece e foi declarado conforme; “aprovado” não é trava do recálculo. | **Alta** no contrato da cópia | **ESTRUTURA**, pendência reenquadrada |
| **FECH-08 — JA RESOLVIDO quanto à suposta obrigação de consultar tranca** | `app/folha/views.py:152`; `app/core/juizes.py:514`; `app/ponto/tests/test_contract_juiz_fechamento.py:117` | Período trancado com fechamento aberto/aprovado continua elegível ao caminho do TXT. O contrato explicitamente cobra que TXT e recálculo não consultem tranca. Não equivale a resolver o novo estado `encerrado`. | **Alta** no escopo declarado | **ESTRUTURA**, pendência reenquadrada |
| **FECH-09 — JA RESOLVIDO: painel lê tranca viva** | `app/ponto/views.py:1027`; `app/ponto/services/fechamento.py:1454` | Linha aprovada sem `PeriodoFechado` vivo: painel não chama o período de fechado apenas pelo aprovado. Linha aberta com tranca viva: reconhece a tranca. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-10 — JA RESOLVIDO: ranking distingue entrega de aprovação** | `app/folha/services/ranking_he.py:85`, `app/folha/services/ranking_he.py:87` | Fechamento aberto com TXT vigente entregue pode receber `lavrado`; aprovado sem entrega recebe `parcial`. A resposta consulta `competencia_entregue`. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-11 — JA RESOLVIDO por correção de enquadramento** | `app/holerite/matriz.py:39`, `app/holerite/matriz.py:51`, `app/holerite/matriz.py:89`; `app/core/juizes.py:590` | Em 21/09, corte padrão 21, seleção recente aponta setembro por `janela_anterior`. `_limites(2026,9)` continua civil, pois serve ao universo do holerite, outra pergunta. Não considero `_limites` uma função corrigida. | **Alta** na distinção | **ESTRUTURA**, item mal enquadrado |
| **FECH-12 — JA RESOLVIDO: ranking abre na competência anterior** | `app/folha/services/ranking_he.py:72`, `app/folha/services/ranking_he.py:106` | Hoje 04/10, corte 21, existem fechamentos parciais de outubro: default continua setembro, pela janela anterior, em vez do último fechamento existente. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-13 — JA RESOLVIDO no sítio nominal removido** | `app/core/juizes.py:618`; `app/folha/export.py:545` | Antigo `dia_em_competencia_exportada` deixou de ser o sítio dessa derivação; existe autoridade específica de entrega. Reconstituição do comportamento antigo e efeitos nos casos de produção: [nao verificado]. | **Média**, para baixa nominal | **ESTRUTURA**, sítio removido |
| **FECH-14 — JA RESOLVIDO: default do POST de recálculo** | `app/ponto/views.py:1237`, `app/ponto/views.py:1238`; `app/ponto/janelas.py:24` | Em 25/12, corte 21, POST sem mês/ano aponta **01/2027**, não dezembro civil. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-15 — JA RESOLVIDO nas telas de fechamento** | `app/ponto/views.py:1071`, `app/ponto/views.py:1106`; `app/core/juizes.py:635` | Em 25/09, corte 21, GET sem competência abre outubro. O `hoje.month` remanescente na lista de ausências não responde por fechamento; foi reenquadrado expressamente. | **Alta**, nesse escopo | **ESTRUTURA**, corrigida |
| **FECH-16 — JA RESOLVIDO no painel** | `app/ponto/views.py:981`, `app/ponto/views.py:982` | Em 25/09, painel de colaborador com corte 21 abre competência outubro pelo juiz da empresa. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-17 — JA RESOLVIDO: justificativas pela janela** | `app/ponto/views.py:997`, `app/ponto/views.py:1000` | Competência outubro, corte 21: justificativa de **22/09 entra**; a de **25/10 fica fora**. O filtro usa intervalo, não `data_turno__month=10`. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-18 — JA RESOLVIDO na origem do universo da tela** | `app/ponto/views.py:1114`; `app/ponto/services/fechamento.py:423` | Corte 21, competência outubro, desligado em 10/09: fica fora; não entra apenas porque cairia no antigo envelope iniciado em 02/09. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-19 — JA RESOLVIDO no universo do escritor** | `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:428`, `app/ponto/services/fechamento.py:545` | Mesmo caso anterior, inclusive sem empresa selecionada: universo combina as janelas de cada empresa. O envelope de cortes continua como pré-carga, não como critério de inclusão. | **Alta** | **ESTRUTURA**, corrigida |
| **SOMBRA-X-PROD-COL988-124639 — NAO SE SABE** | Relato em `app/docs/BACKLOG.md:744` | Caso documental col988, célula 124639, 11/10, julgamentos distintos entre ambientes. Sem as duas fotografias de insumos, não discrimino configuração, cadastro ou defeito. Reprodução: [nao verificado]. | **Baixa** sobre o estado atual | **[nao verificado]** |
| **ATA-DEFASADA-COMP09 — NAO SE SABE** | `app/docs/BACKLOG.md:764` | O próprio registro contém medidas contestadas de execução forçada e bump. Sem atas, versões e insumos correspondentes, não sustento quantidade afetada nem causa. | **Baixa** | **[nao verificado]** |
| **O327 — NAO SE SABE quanto à cobertura atual** | `app/docs/BACKLOG.md:1618` | Relato de 107/608 não julgados por campos sem dono. Não há registros fornecidos para repetir a lavratura desses colaboradores; não trato os demais como verdes nem os 107 como falhas atuais comprovadas. | **Baixa** | **[nao verificado]**; falha do medidor não prova dono do dado |
| **CARIMBO-QUE-A-CELULA-DESMENTE — NAO SE SABE quanto ao passivo e ao dono** | `app/docs/BACKLOG.md:1573` | Caso documental pergunta 38225, dia 29/09, carimbo `celula_absolveu` com célula acusando. Falta comparar o estado no ato do carimbo com o rejulgamento posterior; escolher uma causa seria **especulação**. | **Baixa** | **[nao verificado]** |
| **O261-b — NAO SE SABE quanto à execução e saneamento do lote** | `app/docs/BACKLOG.md:2098` | Universo documental de perguntas antigas com `via_resolucao='chamado_encerrado'`. Sem as linhas e trilhas, não confirmo os 3.923 casos, reavaliação posterior ou residual atual. | **Baixa** | **[nao verificado]** |

## 05/11 16:15 · sessao 5dbc26f2028d · - · - · origem=Ronald (tmux)

Escritores operacionais encontrados:

| Escritor | Validação de `tipo` |
|---|---|
| Construção `Ausencia(**kwargs)` e `a.save()` — `app/ponto/services/ausencia.py:341`, `:385`, `:418` | Catálogo vivo em `:193-195`; exclui expressamente `tipo` de `full_clean` em `:387`. Aceita código fora de `TIPO_CHOICES` se estiver no catálogo vivo. |
| `editar_ausencia_registro`: atribuição `ausencia.tipo = tipo`, save completo — mesmo arquivo `:497`, `:508` | Catálogo vivo em `:473-475`; não exige choices. |
| `rederivar_fim`: save completo — mesmo arquivo `:1280` | Não valida tipo; pode regravar valor existente fora dos choices ou catálogo. Chamado pelo comando `app/ponto/management/commands/curar_ausencia_sem_fim.py:42`, `:46`. |
| `Ausencia.save` — `app/ponto/models.py:866`, `:906` | Não chama validação de choices/catalogação; deriva carimbo, minutos e range, depois `super().save`. |

Chamadores que chegam à criação e, portanto, herdam a checagem de catálogo:

- Admin da aplicação: `app/ponto/views.py:2368`, `:2405` — recebe tipo do POST, cria aprovada em `:2410-2411`.
- PWA: `app/ponto/views.py:2499`, `:2526` — checagem adicional de catálogo.
- API app: `app/api/views.py:1820` — recebe tipo; tradução legada em `:1768-1771` só contempla `atestado_medico`, `atestado_acompanhante`, `obito`.
- Copiloto: `app/ponto/services/checar_ausencia.py:46` — aprovada.
- Veredito humano: `app/ponto/services/veredito_celula.py:311`, `:315` — `falta` ou argumento `tipo_ausencia`.
- Disputa: `app/chamados/services/materializacao.py:484`, `:519` — mapa da resposta ou argumento.
- Regularização: `app/ponto/services/regularizacao.py:118` — literal `falta` em `:119`.
- Abandono provisório: `app/ponto/management/commands/marcar_abandono_provisorio.py:76` — literal `falta` em `:79`.
- Férias: `app/ponto/services/ausencia.py:1186` — literal `ferias` em `:1187`.
- Wrapper idempotente `garantir_ausencia`: `app/ponto/services/ausencia.py:461`.
- Edição HTTP: `app/ponto/views.py:2784` → `editar_ausencia_registro`, tipo do POST em `:2786`.

O censo AST existente, executado em memória, também identificou saves com `update_fields` em `app/ponto/services/ausencia.py:536`, `:819`, `:843`, `:911`, `:1114`, `:1124`, `:1132`: nenhum inclui `tipo`; o update de `:1265-1267` só altera status/carimbo. As migrations de dados `0057_a4_backfill_pendente.py:43`, `:65`, `0058_a4_relogio_recomeca.py:42`, `:63`, `0059_a4_d0_no_primeiro_dia_do_cron.py:36` só alteram status/prazo. Não apareceu update/bulk operacional de `Ausencia.tipo` nas buscas e no censo; o censo exclui testes e migrations por projeto (`app/core/censo_escritas.py:54`, `:66`, `:70`).

**Não encontrei escritor de `Ausencia` que fixe literalmente `'folga'`.** Existe um caminho condicionado ao cadastro: `salvar_tipo_ausencia` aceita código derivado de POST/nome em `app/ponto/views_tipos_ausencia.py:89-94`, incluindo `folga`; recebe ativo em `:80`; grava `TipoAusencia` por `app/ponto/services/ausencia.py:1294`. Código ativo entra no catálogo consultado em `app/ponto/catalogo/ausencias.py:249-252`, `:278`, `:284-285`. Existência/estado desse cadastro em produção: **[nao verificado]**.

As telas enviam opções dinâmicas:

- Lançar: `app/templates/ponto/lancar_ausencia.html:26-34`.
- Submeter: `app/templates/ponto/submeter_ausencia.html:21-30`.
- Editar: `app/templates/ponto/partials/painel_ausencia.html:134-138`.
- JS envia `new FormData(f)` sem tradução de tipo: `app/templates/ponto/ausencias.html:249`.
- API anuncia choices do catálogo vivo: `app/api/views.py:1867`.

Os literais `'folga'` em `app/chamados/services/validacao.py:280`, `:291` chegam a `declarar_dia`, que despacha folga em `app/ponto/services/declarar_dia.py:167` para `_escrever_folga`: este chama `escala.folgas.lancar_folga(..., 'FD')` em `:179-191`, sem criação de Ausencia.

O Django admin atual não é escritor: `app/ponto/admin.py:37-47` usa `SoLeitura`; proíbe add/change/delete em `app/core/admin.py:59-66` e torna todos os campos somente leitura em `:68-75`.

O log `acao='criar', modelo='Ausencia'` é compatível com a porta central, que o emite em `app/ponto/services/ausencia.py:441-448`, mas não identifica qual chamador originou a linha. Atribuição do pk 5356 a endpoint específico: **[nao verificado]**.

## 05/11 16:14 · sessao bbf14dffc8ac · - · - · origem=Ronald (tmux)

Censo de leitores decisórios, somente na cópia. “Desconhecido” abaixo significa ausente de `vigente()`; estar fora de `TIPO_CHOICES` não basta, pois `vigente()` consulta `TipoAusencia` ativo e só usa a semente como fallback (`app/ponto/catalogo/ausencias.py:240-254`, `:267-285`). Logo, se `folga` existe no cadastro de prod é [nao verificado].

| Leitor | Comportamento para desconhecido | Evidência |
|---|---|---|
| Catálogo de efeitos | `efeito_vigente` retorna `None`; rótulo retorna o próprio código; não pertence a abonados/descontados/bloqueantes/parciais/médicos/documentais/indeterminados. `fim_de` retorna `por_lei`. | `app/ponto/catalogo/ausencias.py:192-195`, `:293-310`, `:326-358`, `:385-387`, `:424-435`, `:403-407` |
| Propriedade `Ausencia.efeito` | Delega ao efeito vigente, portanto `None`. | `app/ponto/models.py:837-840` |
| `precedencia.faixa_de` | Todo código fora do mapa fixo cai em `atestado`, mesmo desconhecido ao cadastro; literal `folga` também cai em `atestado`, pois só `folga_compensatoria` e `feriado_folga` estão na faixa folga. | `app/ponto/precedencia.py:52-64`, `:76-79` |
| `fatos_do_periodo` | Inclui não rejeitadas sem validar código; desconhecido participa como faixa atestado, preserva código em `tipo_do_dia`; vence feriado/folga/trabalho se não houver faixa anterior. | `app/ponto/precedencia.py:273-279`, `:336-339`, `:379-405`; ordem `:47-48` |
| Cobertura/expansão/vigência | Desconhecido pode cobrir data inicial ou range fechado; `data_fim=None` cobre só a data inicial, pois não está em `TIPOS_EM_ABERTO`. Folha só aprovada; cobrança toda não rejeitada. Desempate maior data e pk. | `app/ponto/turnos.py:2509-2518`, `:2529-2533`, `:2552-2557`, `:2753-2770`, `:2774-2790`, `:2815-2824`, `:3087-3095` |
| Parcial/integral | Não pertence aos parciais, logo `cobre_parte_do_dia=False`. Se diferente de literal `falta`, `ausencia_explica_o_dia_inteiro=True`; porém mapa de integrais que alimenta o motor exige adicionalmente efeito `ABONA`, portanto desconhecido não entra. | `app/ponto/turnos.py:2595-2600`, `:2668-2675`, `:2684-2690` |
| Afastamento | Não entra: restringe aos tipos `('afastamento_inss',)`. | `app/ponto/turnos.py:2702-2719` |
| Grade `_marcar_ausencias` | Aprovação desconhecida vira `tipo_dia='ausencia'`, preserva tipo/id, marca parcial falso, limpa células e órfãs; também sobrepõe dia originalmente folga porque código difere de `falta`. | `app/escala/utils.py:1208-1212`, `:1231-1251`, `:1272-1274` |
| Cartório, lâmpadas | Se aprovada e grade não parcial, dispensa lâmpadas e escreve código literal desconhecido em `dispensada`; nenhuma validação do catálogo. | `app/ponto/services/cartorio.py:766-778` |
| Cartório, precedência | Copia resultado do juiz, inclusive código desconhecido e faixa atestado, para a ata. | `app/ponto/services/cartorio.py:1947-1956` |
| Suprajuiz | Consome status de cobertura e flag parcial da grade, não revalida código. Aprovação desconhecida conta como cobertura integral; impede ramo de furo, pode acusar trabalho em ausência ou cobrança em dia coberto conforme outros insumos. | `app/ponto/supra_juiz.py:255-269`, `:304-316`, `:400-405`, `:530-534` |
| **Fechamento financeiro** | **`None != DESCONTA`: desconhecido aprovado adiciona dia a `_datas_suprimidas`**, sem abonar. Não recebe crédito de minutos dos tipos pagos. | `app/ponto/services/fechamento.py:664-669`, `:684-692` |
| **Espelho/PDF via `efeitos_da_ausencia`** | **Diverge do fechamento**: só suprime se efeito é exatamente `SUPRIME`; desconhecido não justifica, não suprime, não recebe crédito nem entra nos integrais abonados. | `app/ponto/services/efeito_ausencia.py:43-61`; chamadas `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:332-333` |
| Motor | Não decide pela string desconhecida: recebe mapa de integrais abonados; teto que zera atraso/saída depende de presença nesse mapa. Código aparece só na mensagem. Como produtor exige `ABONA`, desconhecido não obtém esse teto. | `app/ponto/services/fechamento.py:682-683`, `:742`; `app/ponto/turnos.py:2689-2690`; `app/ponto/motor_calculo_v2.py:1085-1105` |
| Export 8792/faltas | Catálogo restringe a `descontam_vigentes`; desconhecido não entra na falta/minutos de falta. | `app/folha/export.py:75-81`, `:127-131`, `:168-170` |
| Export 8932/abonos | Catálogo restringe a abonados menos `FORA_DO_8932`; desconhecido não entra. | `app/folha/export.py:182-195` |
| **Veredito visual `dia_decidido`** | **Efeito `None` cai no retorno `ABONADO`**, salvo rejeitada; pedido aberto tem apresentação `PENDENTE` por outro ramo. | `app/ponto/services/dia_decidido.py:447-450`, `:460-473`, `:476-481` |
| Status do dia sem lavra/batida | Faixa atestado com efeito None vira status `ausencia`, pois só efeito DESCONTA vira `falta`. | `app/ponto/status_dia.py:296-319` |
| Classificador de falta | Qualquer ausência não rejeitada não parcial cala cobrança; desconhecido não é parcial, portanto pode retornar `(None,None,None)`. | `app/escala/servico_jornada.py:340-345` |
| Materialização, cobertura | Desconhecido não falta e integral cala pergunta. | `app/chamados/services/materializacao.py:78-82`; integral `app/ponto/turnos.py:2675` |
| Adesão | Cobertura desconhecida diferente de `falta` faz pular cobrança de adesão. | `app/chamados/services/adesao.py:68-70` |
| Furos diários / reconciliar grade | Cobertura desconhecida diferente de falta entra como ausência, suspendendo cobrança. | `app/escala/services/furos_diarios.py:223-224`, `app/ponto/management/commands/reconciliar_grade.py:402` |
| Crédito parcial | Só tipos pagos: desconhecido não ganha crédito. | `app/ponto/services/credito_parcial.py:23-26` |
| Estado/documento/aprovação | `estado_inicial` desconhecido cai aguardando decisão sem prazo; contradição documento só DESCONTA (None passa); gate documental só membros de `TIPOS_EXIGEM_DOC` (desconhecido passa). | `app/ponto/services/ausencia.py:763-781`, `:591-597`, `:610-618` |
| Ausência `exige_documento` | Delega ao estado inicial, portanto desconhecido retorna falso. | `app/ponto/models.py:723-729` |
| Exportar documentos | `_exige_documento` desconhecido retorna falso. | `app/ponto/services/atestados_lote.py:25-29` |
| Aviso na batida | Restrição a tipos bloqueantes com frase cadastrada: desconhecido não gera aviso. | `app/ponto/services/aviso_da_batida.py:77-92` |
| Alertas médicos / relatórios médicos | Desconhecido não é médico, fica fora. | `app/ponto/services/alertas_ausencia.py:49-50`, `app/ponto/views.py:2223`, `:2830`, `app/relatorios/views.py:282` |
| Resumo inteligência | Fora tanto de faltas como de justificadas, pois ambos usam conjuntos do catálogo. | `app/inteligencia/resumo.py:41-44` |
| Score assiduidade | Desconhecido não rejeitado permanece após excluir abonados: penaliza 10 pontos por registro. | `app/inteligencia/management/commands/calcular_scores.py:11-16` |
| Alertas recorrentes | Desconhecido não rejeitado permanece no universo não abonado; participa do limiar de 3 registros no mês. | `app/inteligencia/management/commands/gerar_alertas.py:54-61` |
| Vigia de família | Classifica não rejeitadas com código ausente de catálogo em `tipos_fora_do_catalogo`; informa contagem e tipos, não corrige código. | `app/ponto/services/vigia_ausencia.py:102-105`, `:180-184` |
| Vigia ausência/veredito | Desconhecido aprovado não parcial é candidato a discordância com célula acusadora, agrupado por código. | `app/ponto/services/vigia_ausencia_veredito.py:49-57` |
| Comando ausências sem efeito | Desconhecido não é DESCONTA nem parcial, portanto não é descartado da auditoria de cobertura sem efeito. | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44-60` |
| Rótulo/ícone | Código cru como rótulo vigente; rótulo curto limitado a 12; ícone padrão de atestado. | `app/ponto/catalogo/ausencias.py:175`, `:178-181`, `:298-300` |

Não transformar comentários históricos do código em dado de prod atual; por exemplo o comentário sobre “3 aprovadas” em `fechamento.py:674-675` não é medição autorizada pelo anexo.

Literal `folga` como declaração do colaborador é explicitamente distinguido de tipo de ausência em `app/chamados/services/declaracao_texto.py:145-156`; o menu admite `folga`, mas isso por si não prova gravação de Ausencia (`:160-169`).

## 05/11 16:14 · sessao d31d6314ef56 · - · - · origem=Ronald (tmux)

Achados sazonais, somente leitura:

1. **14/11: expira a exceção de cron; instalação e 15 testes voltam a falhar com esta configuração.** O prazo é 13/11/2026 inclusive; depois disso o decorador devolve os testes sem `expectedFailure` (`app/core/tests/marca_cron15.py:24`, `:52`, `:57`). O render também propaga `CronNaoCabe` após o prazo (`app/core/management/commands/crons.py:93`, `:97`). O JSON contém `sombra.sh=5188` segundos (`app/config/crons_duracao.json:78`); o cálculo arredonda para 87 minutos e exige começar antes do piso 04:05, considerando vizinho 05:25 e tolerância de dois minutos (`app/config/crons.py:182`, `:183`, `:219`, `:266`). **Contraexemplo:** relógio do processo em 14/11, código e JSON iguais à cópia, render para instalação mesmo com linha válida de sombra no host. **Certeza alta; ESTRUTURA.** Não prova interrupção do cron já instalado; crontab e execução reais são **[não verificado]**.

2. **Remarcar férias deixa ausências da janela anterior.** A remarcação troca datas e salva sem limpar o intervalo antigo (`app/ferias/services.py:393`, `:400`, `:406`). O `save` espelha novamente (`app/ferias/models.py:434`), mas o espelhamento consulta apenas o intervalo novo (`app/ponto/services/ausencia.py:1165`); a limpeza posterior percorre somente essas linhas (`:1195`). A outra porta de correção conhece o problema e apaga expressamente o intervalo antigo (`app/ferias/services.py:374`, `:385`). **Contraexemplo:** em 05/11, AG agendado 10–23/11 com 14 ausências aprovadas; editar para 01–14/12, mantendo 14 dias e sem colisão. As ausências de novembro permanecem; o agendamento passa a dezembro. O leitor `em_gozo_hoje` consulta as datas do AG (`app/ferias/services.py:47`, `:63`). **Certeza alta estática; ESTRUTURA.** Existência desse estado em produção: **[não verificado]**.

3. **13º em lote posterior colide com o holerite mensal da mesma competência.** A importação considera publicado qualquer holerite do mesmo colaborador/mês/ano (`app/holerite/motor.py:323`), classifica o novo recibo como `ja_publicado` (`:344`) e publica somente `ok` (`:467`). A identidade não inclui tipo de folha; o modelo contém competência e sequência (`app/holerite/models.py:107`), e a sequência reinicia dentro de cada publicação (`app/holerite/motor.py:465`). **Contraexemplo:** mensal 11/2026 publicado; carregar depois recibo reconhecido da primeira parcela do 13º para o mesmo colaborador e 11/2026: o novo recibo será pulado. **Certeza alta do comportamento; ESTRUTURA. Especulação rotulada:** que o DP carregará o 13º dessa forma nas quatro semanas é **[não verificado]**. Dois recibos juntos no primeiro lote recebem sequências diferentes (`app/holerite/motor.py:471`, `:480`).

Limitações relevantes:

- “Férias em lote” usa a validação individual (`app/ferias/views.py:569`, `:605`). Um PA sem frações anteriores, submetido com dez dias, é recusado pela exigência de uma fração de pelo menos 14 dias (`app/ferias/models.py:264`, `:365`). Aplicabilidade de regra específica de férias coletivas e utilização operacional desse caminho: **[não verificado]**; não apresentaria como infração legal provada.
- `aviso_emitido` é calculado pela antecedência, sem prova de comunicação nesse trecho: marcar férias para 20/12 em 20/11 produz `True` (`app/ferias/services.py:313`, `:323`), embora o campo se descreva como aviso emitido (`app/ferias/models.py:305`). Entrega real do aviso: **[não verificado]**.
- Ausência de agendamento automático de férias é deliberada, portanto não constitui achado por si só (`app/ferias/situacao.py:1`).

## 05/11 16:14 · sessao 166e365375c7 · - · - · origem=Ronald (tmux)

Evidências para sonda:

- `janela_atual(data, empresa=None)` respeita o corte da empresa e retorna `(ini, fim)`; competência corresponde ao mês/ano de `fim`. `app/ponto/janelas.py:24-33,36-63`.
- `competencia_entregue(empresa_id, mes, ano, registro=False)` faz leitura de `ExportacaoDominio`, aplica `anotar_vigencia`/`so_vigentes` e retorna verdadeiro se o vigente não estiver invalidado. Não escreve. `app/folha/export.py:545,569-575`.
- Empresa atual do colaborador: `app/colaboradores/models.py:321`; corte: `app/colaboradores/models.py:46`; registrador e criação da ausência: `app/ponto/models.py:799-802,811`.
- “Fora de choices” e “fora do catálogo” são universos distintos: `app/ponto/models.py:661-679`; catálogo usa tipos ativos e fallback quando vazio/erro, `app/ponto/catalogo/ausencias.py:240-254,267-285`.
- A sonda deve verificar diretamente que a leitura de `TipoAusencia` funcionou antes de usar o catálogo; `vigente()` sozinho esconde falha de banco usando a semente. `app/ponto/catalogo/ausencias.py:253-254,278`.
- Contar registros pela competência contendo `Ausencia.data` é um recorte explícito possível. Não representa todos os meses atravessados pelo intervalo; esse impacto deve ser identificado como não medido se não houver expansão por janela. Estrutura do intervalo: `app/ponto/models.py:750-754,889-905`.
- Empresa histórica na data da ausência: `[nao verificado]`; o modelo aponta somente o colaborador, cuja empresa é o campo atual. `app/ponto/models.py:743-745`; `app/colaboradores/models.py:321`.

Implementação recomendada:

1. Verificar `SELECT current_user` e abortar se diferente de `leitor`.
2. Consultar somente `.values('tipo','status','data','registrado_por_id','criado_em','colaborador__empresa_id')`, sem carregar nomes/CPF.
3. Fazer uma dimensão `exclude(tipo__in=choices)` para o censo solicitado.
4. Fazer outra `exclude(tipo__in=catalogo_validado)` para registrador/mês dos tipos fora do catálogo; isso também captura um tipo de choices removido/inativado no catálogo.
5. Agrupar por tipo/status/empresa/mês/ano da janela; cachear `competencia_entregue` por empresa/mês/ano.
6. Falha de consulta, janela ou juiz deve render `"nao medido"`, com no máximo a classe da exceção. Nunca converter falha em lista vazia, falso ou zero.
7. Total entregue deve ser `"nao medido"` se algum grupo necessário ficou sem leitura; zero é admissível somente após resultado falso medido.

O wrapper executa o Python como `leitor` e monta a árvore somente leitura, mas ele próprio escreve envfile, role/grants e trilha. Portanto, apenas fornecer a sonda, sem executá-la, respeita a ordem absoluta de não criar/editar/apagar arquivos nesta sessão. `bin/sonda_leitura.sh:126-143,153-181,235-240,248-250`.

## 05/11 16:14 · sessao c2d203764ddc · CODEX-SUFOCA-17-app-api · leitura · origem=Code

Há divergências demonstráveis na fila offline, nas respostas de batida, nos estados de ausência e no push. Os contraexemplos abaixo são **estados construídos a partir do código**, não ocorrências comprovadas em produção. Ocorrência, frequência e pessoas atingidas: **[não verificado]**. Referências principais: `app/static/pwa.js:107`, `app/api/views_core.py:551`, `app/api/views.py:1071`, `app/static/sw.js:97`.

**Certeza** indica a força da evidência estática. **Especulação rotulada:** qualquer estimativa de impacto real seria especulação; não foi usada como prova. **[não verificado]**

1. **A fila offline não identifica o titular da batida.** O IndexedDB tem nome fixo; os itens não guardam colaborador; a sincronização exige apenas perfil “colaborador”. O servidor atribui a batida à sessão atual. **Contraexemplo:** A salva uma entrada offline, sai e B entra no mesmo navegador: a pendência de A pode ser registrada para B e apagada da fila. **Certeza alta.** `app/static/pwa.js:50`, `app/static/pwa.js:113`, `app/static/pwa.js:128`, `app/api/views_core.py:389`, `app/core/views.py:79`.

2. **O horário corrigido é salvo, mas não é enviado.** A fila guarda `timestamp` corrigido pelo gap e `timestamp_dispositivo` bruto; o POST envia somente o bruto, que a API aceita como horário efetivo dentro de 48 horas. **Contraexemplo:** hora real 08:00, aparelho 07:50, gap conhecido de +10 minutos: a fila guarda 08:00, mas o servidor grava 07:50. Acima de 48 horas, o servidor substitui o instante por “agora”, e o PWA apaga a pendência após o 2xx. **Certeza alta.** `app/static/pwa.js:109`, `app/static/pwa.js:147`, `app/static/pwa.js:158`, `app/api/views_core.py:529`, `app/api/hora_do_aparelho.py:63`, `app/api/hora_do_aparelho.py:141`.

3. **A idempotência não tem uma chave estável do evento offline.** O POST não transmite o ID da pendência; a unicidade persistida é colaborador+timestamp. Existe deduplicação por janela, mas um timestamp antigo demais vira um novo “agora” a cada tentativa. **Contraexemplo:** entrada guardada há 49 horas é gravada na chegada; a resposta se perde; reenvio seis minutos depois, sem outras batidas, pode criar outra marcação porque mudou o timestamp efetivo e expiraram as janelas. **Certeza alta do caminho; execução não verificada.** `app/static/pwa.js:143`, `app/ponto/models.py:106`, `app/api/hora_do_aparelho.py:148`, `app/api/views_core.py:549`, `app/ponto/registro_batida.py:106`.

4. **A deduplicação da API pode devolver uma batida retratada como sucesso.** Seu filtro de cinco minutos não exclui retratadas; retorna antecipadamente `ja_registrada` e `sucesso:true`. A porta de escrita exclui retratadas da janela, mas não chega a ser chamada nesse ramo. **Contraexemplo:** E08:00 retratada, nova E08:04: a API devolve a antiga e o PWA remove a nova pendência, sem criar entrada ativa. **Certeza alta.** `app/api/views_core.py:551`, `app/api/views_core.py:558`, `app/ponto/registro_batida.py:107`, `app/static/pwa.js:158`.

5. **O mesmo GPS recebe julgamentos diferentes online e offline.** A fila salva `gps_accuracy`, mas omite o campo no reenvio; a API grava ausência de precisão. O envio online v2 inclui a precisão. **Contraexemplo:** raio de 150 m, distância de 500 m, precisão de 10 m: online o juiz classifica `fora`; offline, com a precisão perdida, classifica `gps_incerto`. **Certeza alta.** `app/static/pwa.js:117`, `app/static/pwa.js:143`, `app/templates/ponto/bater_ponto_v2.html:332`, `app/api/views_core.py:422`, `app/ponto/services/geofence.py:117`, `app/ponto/services/geofence.py:121`.

6. **A tela v2 anuncia o tipo solicitado, ignorando a resposta da autoridade.** Qualquer `r.ok` chama sucesso com o tipo local, sem ler o JSON. O servidor pode corrigir E/S e devolve o tipo efetivamente registrado e um `aviso`. **Contraexemplo:** duas abas carregam “E”; uma registra entrada; mais de cinco minutos depois, a outra envia “E”, o servidor corrige para “S” e a tela anuncia “Entrada registrada”. **Certeza alta sob esse estado.** `app/templates/ponto/bater_ponto_v2.html:150`, `app/templates/ponto/bater_ponto_v2.html:340`, `app/api/views_core.py:592`, `app/api/views_core.py:823`.

7. **O cache HTML offline não é preenchido pela navegação normal.** O ramo `navigate` retorna antes do trecho que grava dashboard/ponto; a instalação só precacheia JS, manifesto e ícones. **Contraexemplo:** instalação limpa, visita normal online à tela de ponto, fechamento e reabertura sem rede: esse fluxo não armazenou o HTML, e o fallback devolve erro. Além disso, Background Sync sem página aberta apenas mostra uma notificação para abrir o app. **Certeza alta no código; navegador não exercitado.** `app/static/sw.js:3`, `app/static/sw.js:29`, `app/static/sw.js:45`, `app/static/sw.js:67`, `app/static/sw.js:116`.

8. **O relógio muda de fuso ao entrar no fallback.** Online ele mostra a hora do servidor, configurado para São Paulo; offline ou após falha usa os getters locais do aparelho. **Contraexemplo:** aba aberta, instante 12:00Z, aparelho em UTC−4: online mostra 09:00; offline mostra 08:00. Isso é divergência de apresentação; não demonstra conversão incorreta do ISO enviado. **Certeza alta.** `app/config/settings/base.py:93`, `app/core/views.py:14`, `app/templates/ponto/bater_ponto_v2.html:163`, `app/templates/ponto/bater_ponto_v2.html:168`.

9. **O contrato de anexos ainda depende de um estado aposentado.** `chamados/v2` só preenche `anexo_pendente` para `status == 'pendente'`; os estados abertos declarados são `aguardando_documento` e `aguardando_decisao`. **Contraexemplo:** atestado sem documento em `aguardando_documento`, com chamado visível: GET entrega `anexo_pendente:null`, embora o endpoint de documento aceite o envio. **Certeza alta.** Correção da minha atualização intermediária: o nome declarado é `aguardando_decisao`, não `em_analise`. `app/api/views.py:1071`, `app/api/views.py:1156`, `app/ponto/models.py:701`, `app/api/views.py:3101`.

10. **Os horários anunciados pela API podem contrariar a célula congelada.** `/api/me/` e `/api/escala/v2/` consultam o vínculo para saber se há trabalho, mas buscam os horários no template. A autoridade do vínculo lê primeiro o DNA da célula. **Contraexemplo:** célula de hoje com 07–19 e template atual com 08–20: a autoridade devolve 07–19, enquanto a API anuncia 08–20. **Certeza alta.** `app/api/views_core.py:248`, `app/api/views.py:934`, `app/api/views.py:939`, `app/escala/models.py:1124`.

11. **A regularização enviada ao app perde o significado da operação.** Cada item contém apenas data e horas; não informa `acao`, `batida_alvo` ou `intervalo`. A tela web mostra uma descrição da operação. **Contraexemplo:** adicionar saída às 18:00 e corrigir uma batida existente para saída às 18:00 produzem itens JSON indistinguíveis, mas efeitos diferentes. Uma remoção com horas nulas também chega sem informar que apagará uma marcação. **Certeza alta da perda no payload; apresentação nativa [não verificado].** `app/api/views_core.py:1298`, `app/chamados/models.py:3202`, `app/chamados/models.py:3214`, `app/chamados/services/regularizacao_ext.py:131`, `app/templates/chamados/meu_atendimento.html:15`.

12. **Falha de materialização retorna HTTP 200.** A resposta contém `ok:false` e os erros, sem status HTTP de falha. **Contraexemplo:** lote aguardando resposta e disputa de supervisão aberta: a confirmação muda o lote para `confirmada`, mas não materializa batidas; a API retorna 200 com zero criadas. **Certeza alta do comportamento HTTP; falso sucesso na tela nativa [não verificado]**, porque o corpo sinaliza a falha. `app/chamados/services/regularizacao_ext.py:352`, `app/chamados/services/regularizacao_ext.py:58`, `app/api/views_core.py:1331`.

13. **Dois GETs discordam sobre uma opção que o POST rejeita.** `chamados/v2` entrega `p.opcoes` sem filtro; `disputa/perguntas` remove `OUTRO_MOTIVO` de perguntas de hora; o POST recusa essa resposta. **Contraexemplo:** pergunta viva de hora com essa opção persistida: um GET anuncia a opção, outro a oculta, e responder gera 400. **Certeza alta condicionada ao estado; existência de linhas legadas em produção [não verificado].** `app/api/views.py:1172`, `app/api/views.py:2553`, `app/api/views.py:2642`, `app/chamados/services/disputa_emissao.py:1263`.

14. **O PWA anuncia inscrição push bem-sucedida após recusa do servidor.** `inscreverPush` não verifica status ou corpo do POST e retorna `true`; o botão mostra “Avisos ligados”. **Contraexemplo:** assinatura criada no navegador, gravação no servidor falha e retorna 500: a tela anuncia sucesso. No carregamento seguinte, a assinatura local existente impede nova inscrição automática. **Certeza alta.** `app/static/pwa.js:246`, `app/static/pwa.js:335`, `app/static/pwa.js:367`, `app/ponto/views.py:2000`.

15. **A associação WebPush também sobrevive à troca de conta.** Logout web encerra a sessão; o PWA não reinscreve uma assinatura existente. O servidor associa o endpoint ao colaborador no POST e seleciona os destinatários por essa associação. **Contraexemplo:** A inscreve o navegador, sai e B entra: não ocorre o POST que transferiria o endpoint para B; ele continua associado a A. **Certeza alta da associação; entrega efetiva após a troca [não verificado].** `app/core/views.py:79`, `app/static/pwa.js:335`, `app/ponto/views.py:1989`, `app/chamados/push_utils.py:24`.

16. **O emissor e o SW discordam sobre os campos de push.** O servidor envia `url` e `requireInteraction`; o SW usa `urgente` e ignora a URL no clique. **Contraexemplo:** payload com `url:'/ponto/bater/'` e `requireInteraction:true`, sem janelas abertas: o SW abre `/` e solicita interação persistente como `false`. **Certeza alta do consumo dos campos; comportamento visual específico do navegador [não verificado].** `app/chamados/push_utils.py:28`, `app/static/sw.js:97`, `app/static/sw.js:102`.

Não consegui verificar builds instalados, comportamento do cliente nativo, entrega FCM/APNs, tokens existentes, configurações efetivas de produção ou ocorrência dos contraexemplos. Testes e consultas a banco/log não foram executados. **[não verificado]** [nao verificado]

Sobre **FCM**, o servidor exige `fcm_token`, valida formato e transfere associação de token repetido; o PWA desta cópia usa WebPush. Isso não prova discordância do cliente FCM distribuído, cujo comportamento permanece **[não verificado]**. `app/api/views_core.py:1014`, `app/api/views_core.py:1026`, `app/static/pwa.js:239`.

Também **não comprovei mudança histórica de campo/formato sem versão**: esta cópia mostra v1 aposentada com 410 e rotas v2 explícitas; quais versões instaladas ainda consomem v1 é **[não verificado]**. Os achados demonstram leitores divergentes no estado atual. `app/api/views.py:839`, `app/api/views.py:878`, `app/api/views.py:888`, `app/api/views.py:898`, `app/api/urls.py:72`.

Os donos abaixo são **ESTRUTURA**: tratam de transporte, serialização ou leitura contraditória do mesmo dado, conforme a separação L-099; não atribuem defeito ao cadastro ou à batida do colaborador. `CLAUDE.md:176`.

| achado | arquivo:linha | contraexemplo | certeza | dono L-099 |
|---|---|---|---|---|
| 1. Fila muda de titular | `app/static/pwa.js:113`; `app/api/views_core.py:389` | A deixa pendência; B entra e a envia | Alta | ESTRUTURA |
| 2. Horário corrigido descartado | `app/static/pwa.js:147`; `app/api/hora_do_aparelho.py:141` | Real 08:00, aparelho 07:50, grava 07:50 | Alta | ESTRUTURA |
| 3. Reenvio sem chave estável | `app/ponto/models.py:106`; `app/api/hora_do_aparelho.py:148` | Item de 49h; resposta perdida; retry após 6min | Alta, estática | ESTRUTURA |
| 4. Retratada devolvida como sucesso | `app/api/views_core.py:551`; `app/static/pwa.js:158` | E08:00 retratada absorve nova E08:04 | Alta | ESTRUTURA |
| 5. Precisão GPS perdida | `app/static/pwa.js:143`; `app/ponto/services/geofence.py:117` | Mesmo fix vira `fora` online e `gps_incerto` offline | Alta | ESTRUTURA |
| 6. Sucesso mostra tipo local | `app/templates/ponto/bater_ponto_v2.html:340`; `app/api/views_core.py:592` | API corrige E→S; tela anuncia entrada | Alta, condicional | ESTRUTURA |
| 7. HTML offline ausente | `app/static/sw.js:45`; `app/static/sw.js:67` | Instalação limpa, visita online, reabertura offline | Alta, estática | ESTRUTURA |
| 8. Fuso muda no fallback | `app/templates/ponto/bater_ponto_v2.html:163`; `app/core/views.py:14` | Aparelho UTC−4: 09:00 online, 08:00 offline | Alta | ESTRUTURA |
| 9. Estado morto oculta anexo | `app/api/views.py:1071`; `app/api/views.py:3101` | `aguardando_documento` recebe `anexo_pendente:null` | Alta | ESTRUTURA |
| 10. Template suplanta célula | `app/api/views.py:939`; `app/escala/models.py:1124` | DNA 07–19; API anuncia template 08–20 | Alta | ESTRUTURA |
| 11. Regularização sem operação | `app/api/views_core.py:1298`; `app/chamados/models.py:3214` | Adicionar e corrigir ficam indistinguíveis | Alta no payload | ESTRUTURA |
| 12. Falha com HTTP 200 | `app/api/views_core.py:1332`; `app/chamados/services/regularizacao_ext.py:58` | Disputa aberta impede materialização; resposta 200 | Alta no HTTP | ESTRUTURA |
| 13. GET oferece opção recusada | `app/api/views.py:1172`; `app/chamados/services/disputa_emissao.py:1267` | Pergunta de hora com `OUTRO_MOTIVO` persistido | Alta, condicional | ESTRUTURA |
| 14. Inscrição push com sucesso falso | `app/static/pwa.js:246`; `app/ponto/views.py:2000` | POST 500 seguido de “Avisos ligados” | Alta | ESTRUTURA |
| 15. Push mantém titular anterior | `app/static/pwa.js:335`; `app/ponto/views.py:1989` | A sai; B entra com assinatura local existente | Alta na associação | ESTRUTURA |
| 16. Campos push ignorados | `app/chamados/push_utils.py:28`; `app/static/sw.js:97` | Destino específico e persistência são descartados | Alta | ESTRUTURA |

## 05/11 16:13 · sessao e072b4a485c9 · - · - · origem=Ronald (tmux)

Achado principal: pauta de pré-fechamento da competência 11 pode ficar congelada depois de 21/11. A rodada calcula somente bloqueios da competência corrente, filtra e encerra pautas somente da âncora corrente; prazo vencido não retira pauta de `vivas()`. Referências: `app/chamados/services/fila_por_causa.py:375`, `app/folha/management/commands/pre_fechamento.py:89`, `app/pautas/models.py:57`.

Contra-exemplo sintético: empresa com corte 21; em 20/11 há um chamado vivo sobre 19/11, gaveta `respondido_a_decidir`, e pauta 11 registra “1 bloqueio”. Em 21/11, competência 12 sem chamados: contador retorna zero, mas a consulta da pauta alcança somente âncora 12. Se o chamado de 19/11 for resolvido em 22/11, a pauta 11 continua viva com “1 bloqueio”, salvo intervenção por outro caminho. Referências: `app/chamados/services/fila_por_causa.py:378`, `app/folha/management/commands/pre_fechamento.py:91`, `app/folha/management/commands/pre_fechamento.py:95`.

Certeza alta sobre o mecanismo; média sobre qualificá-lo como defeito operacional, pois o escopo “competência corrente” é explícito. **Especulação rotulada:** DP interpretar a pauta antiga como retrato atualizado do fechamento 11; ocorrência em produção [não verificado]. Dono L-099: **ESTRUTURA**, pois o cadastro e o chamado podem estar corretos. Referências: `app/chamados/services/fila_por_causa.py:355`, `CLAUDE.md:192`.

Reprodução sem banco e sem arquivos: corpo real de `bloqueios_para_fechar` extraído por AST, `janela_atual` real, empresa sintética corte 21; resultado em 20/11 `n=1`, em 21/11 `n=0`. O marcador `[PRE-FECHAMENTO]` só apareceu como escritor no comando correspondente; fechamento genérico semanal exige o marcador da própria família. Referências: `app/chamados/services/fila_por_causa.py:349`, `app/folha/management/commands/pre_fechamento.py:29`, `app/pautas/semanal.py:226`.

A virada do núcleo de janelas funciona nos casos examinados: 21/11 abre 21/11–20/12; 21/12 abre 21/12/2026–20/01/2027; 01/01 permanece nessa mesma competência. Executadas somente funções puras com `python3 -B`; suite Django não executada. Referências: `app/ponto/janelas.py:17`, `app/ponto/janelas.py:27`, `app/ponto/janelas.py:66`.

Literal antigo encontrado, mas **não prever como falha automática**: `arquivar_competencia_encerrada` continua com corte padrão 20/08/2026, porém está declarado como ato manual fora do pipeline. Executá-lo em 21/11 sem `--corte` não arquivaria chamados de setembro–novembro; existência de execução futura [não verificado]. Referências: `app/chamados/management/commands/arquivar_competencia_encerrada.py:60`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:84`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:121`, `app/config/crons.py:1321`.

## 05/11 16:13 · sessao 2e8765c6f048 · CODEX-SUFOCA-16-segundo-cliente · leitura · origem=Code

**O segundo cliente encontra bloqueios estruturais antes mesmo de apurar batidas:** competência 1–31 não é representável, a configuração SaaS restringe o domínio e a agenda versionada fixa Juliani. Outra CCT também encontra parâmetros sem consumo pelo motor. (`app/ponto/janelas.py:15`; `app/config/settings/saas.py:115`; `app/config/crons.py:18`; `app/core/regua_cct.py:72`)

Os contra-exemplos abaixo são cenários construídos para demonstrar os caminhos do código. Cadastros, batidas, falhas e quantidade de pessoas efetivamente atingidas em produção: **[não verificado]**. A atribuição de dono separa falha de comportamento de lacuna cadastral, conforme L-099. (`app/docs/LEIS.md:122`)

1. **Competência 1–31 vira outro período.** O POST converte corte `1` em `2`, e o juiz repete essa restrição. Para empresa com corte `1`, competência outubro/2026 resulta em **02/09–01/10**; em 31/10, a janela corrente resulta em **02/10–01/11**. Esses dois resultados foram reproduzidos executando somente as funções de janelas em memória. O export delega ao mesmo juiz, propagando a limitação. **Certeza alta; ESTRUTURA.** (`app/colaboradores/views.py:499`; `app/ponto/janelas.py:15`; `app/ponto/janelas.py:26`; `app/folha/export.py:51`)

2. **Provisionar domínio não o libera na configuração SaaS.** O painel cria `cliente2.work-key.hasner.com.br`, e o Caddy possui o wildcard correspondente; entretanto, `ALLOWED_HOSTS` contém somente Juliani, localhost e loopback. **Contra-exemplo:** provisionar `cliente2` e acessá-lo com os settings SaaS desta cópia: o Host fica fora da lista permitida. **Certeza alta; ESTRUTURA.** Settings efetivamente carregados: **[não verificado]**. (`app/tenants/views.py:48`; `Caddyfile:25`; `app/config/settings/saas.py:115`)

3. **A agenda não incorpora o novo tenant nem descobre suas empresas.** O render dos comandos usa `SCHEMA='juliani'`; rotinas por empresa expandem somente `(2,3,4)`. Há ainda `--empresa 2,3,4` diretamente no reconciliador. **Contra-exemplo:** tenant `cliente2`, empresa `pk=1`, vínculo 5x2: a agenda declarada de geração de células continua apontando para Juliani; trocar somente o schema ainda deixa rotinas restritas às empresas antigas. **Certeza alta; ESTRUTURA.** (`app/config/crons.py:17`; `app/config/crons.py:426`; `app/config/crons.py:462`; `app/config/crons.py:1821`; `app/config/crons.py:1826`)

4. **Outra CCT não consegue alterar sete parâmetros do cálculo.** A declaração inclui adicional noturno percentual, tolerâncias, intrajornada, compensação e teto de prorrogação entre os parâmetros sem efeito. **Contra-exemplo:** CCT cadastrada com tolerância de 15 minutos por marcação e 30 por dia, escala comercial com entrada 08:00 e batida 08:12: o julgador compara os 12 minutos com a constante `10`, independentemente daquela tolerância cadastrada. **Certeza alta; ESTRUTURA.** (`app/core/regua_cct.py:72`; `app/ponto/motor_calculo_v2.py:60`; `app/ponto/motor_calculo_v2.py:2006`)

5. **Intervalo de 30 minutos é aceito na CCT, mas recusado na escala.** O sindicato aceita mínimo `30`; a validação do template aplica mínimo fixo `60` para jornada superior a seis horas. **Contra-exemplo:** empresa CCT com aplicação vigente, escala 5x2/comercial 08:00–16:30, pausa 12:00–12:30: `TipoEscala.clean()` recusa o intervalo. **Certeza alta; ESTRUTURA.** **Especulação:** a CCT real do segundo cliente prever essa redução é **[não verificado]**. (`app/core/models.py:85`; `app/escala/models.py:352`; `app/escala/models.py:17`; `app/escala/models.py:597`; `app/escala/models.py:607`)

6. **O bootstrap administrativo não entrega a configuração trabalhista completa.** Ele cria empresa, setores e superusuário; a empresa tem defaults de corte `21` e regime vazio. A régua exige ainda o switch global dentro do schema e uma aplicação de convenção válida. **Contra-exemplos independentes:**
   
   - Empresa e CCT preenchidas, mas sem `ParametroSistema(regua_cct_ativa, empresa=NULL, valor='1')`: a fábrica retorna o motor sem consultar a convenção. (`app/tenants/bootstrap.py:25`; `app/colaboradores/models.py:46`; `app/colaboradores/models.py:151`; `app/core/regua_cct.py:444`; `app/core/regua_cct.py:449`)
   - Switch ativo, empresa `cct` e sindicato vinculado à praça, mas sem `AplicacaoConvencao` vigente: recebe régua legal com motivo; duas aplicações vigentes no mesmo nível também não selecionam uma CCT. **Certeza alta; CADASTRO.** (`app/core/regua_cct.py:253`; `app/core/regua_cct.py:261`; `app/core/regua_cct.py:269`; `app/core/regua_cct.py:366`)

7. **Integração de folha e código do empregado são cadastros obrigatórios, com recusa explícita.** **Contra-exemplo:** empresa nova sem integração Domínio ativa gera `ValueError` instrutivo; colaborador sem `codigo_dominio` recebe classificação `fora/sem_codigo_dominio`. A ausência dessas informações é **CADASTRO**, sem demonstrar defeito estrutural nesses bloqueios. **Certeza alta.** (`app/folha/export.py:23`; `app/folha/export.py:29`; `app/folha/export.py:846`)

8. **Rubrica ausente tem comportamento diferente: o evento desaparece do TXT.** A montagem ignora evento com valor sem mapeamento; a homologação também ignora esse caso. **Contra-exemplo:** colaborador elegível, integração e código preenchidos, fechamento com HE diurna=2h e atraso diurno=1h; existe rubrica da HE, falta a do atraso: sai a HE, sem linha do atraso e sem retenção por essa ausência. **Certeza alta; ESTRUTURA para a omissão silenciosa; CADASTRO para o mapeamento faltante.** (`app/folha/export.py:711`; `app/folha/export.py:716`; `app/folha/export.py:948`; `app/folha/export.py:981`)

9. **IDs iguais entre tenants colidem em memória.** Há dois caminhos distintos:
   
   - Janela da folha: chave somente `(empresa_id, mês, ano)`. **Contra-exemplo:** no mesmo processo, tenant A/empresa 2/corte15 cacheia início 15/09 para outubro; depois tenant B/empresa 2/corte21, snapshot previsto zerado e demissão 18/09 reutiliza 15/09. Retorna `sem_escala`, embora sua janela de 21/09 exija `rescisao`. (`app/folha/export.py:996`; `app/folha/export.py:1019`; `app/folha/export.py:1037`; `app/folha/export.py:1042`)
   - Pautas: chave somente `user.pk`, com número e remetentes no valor. **Contra-exemplo:** dois usuários staff `pk=2`, tenants diferentes, mesmo cache em menos de 30 segundos; o primeiro tem sete pautas e o segundo zero: o segundo reutiliza o resultado do primeiro. **Certeza alta nas condições descritas; ESTRUTURA.** (`app/pautas/context_processors.py:13`; `app/pautas/context_processors.py:23`; `app/pautas/context_processors.py:29`; `app/pautas/context_processors.py:33`)

10. **O assistente escolhe um cliente global, não o tenant da requisição.** **Contra-exemplo:** admin autorizado de `cliente2`, processo com configuração padrão: a pergunta enviada à mensageria leva `cliente='juliani'`; o serviço seleciona a ativação por esse slug. **Certeza alta para o direcionamento; ESTRUTURA.** **Especulação:** exposição de contexto de Juliani depende das ativações e credenciais existentes — consequência em produção **[não verificado]**. (`app/core/views_ia.py:23`; `app/core/views_ia.py:909`; `mensageria/nucleo/views.py:136`; `mensageria/nucleo/views.py:146`)

11. **O go-live de Juliani limita o histórico de qualquer cliente.** Existe piso global em 21/05/2026, usado no início das fatias e no levantamento de dias previstos. **Contra-exemplo:** segundo tenant importa vínculo 5x2 desde abril e batidas 08/12/13/17 de 04/05/2026: esse dia fica fora desses dois caminhos por causa do piso. Isso demonstra exclusão da fatia e do previsto; valor final de folha nesse cenário **[não verificado]**. **Certeza alta; ESTRUTURA.** (`app/core/constants.py:3`; `app/ponto/services/fechamento.py:204`; `app/escala/alimentacao.py:390`)

12. **A orientação para destravar o export manda operar no schema errado.** **Contra-exemplo:** empresa de `cliente2` sem carimbo diário solicita TXT: a mensagem manda executar o selo com `--schema=juliani`. Seguir essa instrução não mede o tenant solicitante. **Certeza alta; ESTRUTURA.** (`app/folha/porta_export.py:807`; `app/folha/porta_export.py:809`; `app/folha/porta_export.py:812`)

**“Sem 12x36” não demonstra quebra geral:** existem motores comerciais, de turno partido, plantão e intermitente; a trava de âncora do export filtra especificamente 12x36. Também existem sementes transportadas por migrations: usuário `sistema`, tipos de ausência e uma rubrica global de HE100; setores novos semeiam permissões por signal. (`app/ponto/motor_calculo_v2.py:3569`; `app/folha/export.py:789`; `app/core/migrations/0003_user_sistema.py:36`; `app/ponto/migrations/0052_seed_tipoausencia.py:47`; `app/folha/migrations/0009_evento_he100_rubrica_200.py:10`; `app/colaboradores/signals.py:25`)

Não consegui verificar quais sementes existem **somente no tenant atual**, os cadastros efetivos do segundo cliente, a CCT aplicável, DNS/TLS, settings carregados, cron instalado, tráfego entre tenants, valores financeiros ou qualquer ocorrência real atribuível a BATIDA. Execução integrada e resultados de testes: **[não verificado]**. [nao verificado]

| Achado | Arquivo:linha | Contra-exemplo | Certeza | Dono L-099 |
|---|---|---|---|---|
| 1. Competência 1–31 impossível | `app/colaboradores/views.py:499`; `app/ponto/janelas.py:15` | Corte1, outubro → 02/09–01/10 | Alta | ESTRUTURA |
| 2. Novo domínio fora da configuração | `app/tenants/views.py:48`; `app/config/settings/saas.py:115` | Host `cliente2.work-key.hasner.com.br` ausente da lista | Alta | ESTRUTURA |
| 3. Agenda fixa tenant e empresas | `app/config/crons.py:17`; `app/config/crons.py:462`; `app/config/crons.py:1826` | Tenant novo/empresa1 fora do universo declarado | Alta | ESTRUTURA |
| 4. Parâmetros CCT sem consumo | `app/core/regua_cct.py:72`; `app/ponto/motor_calculo_v2.py:2006` | CCT tolera15; entrada12min atrasada supera constante10 | Alta | ESTRUTURA |
| 5. Intervalo CCT30 recusado | `app/core/models.py:85`; `app/escala/models.py:597` | 5x2, 08–16:30, pausa30min | Alta | ESTRUTURA |
| 6. Configuração trabalhista incompleta | `app/tenants/bootstrap.py:25`; `app/core/regua_cct.py:444`; `app/core/regua_cct.py:366` | Sem switch ou sem aplicação vigente, CCT não governa | Alta | CADASTRO |
| 7. Pré-requisitos de folha ausentes | `app/folha/export.py:29`; `app/folha/export.py:846` | Sem integração ou código Domínio, export recusa | Alta | CADASTRO |
| 8. Evento sem rubrica omitido | `app/folha/export.py:716`; `app/folha/export.py:981` | HE mapeada sai; atraso sem rubrica desaparece | Alta | ESTRUTURA: silêncio; CADASTRO: ausência |
| 9a. Janela em memória cruza tenants | `app/folha/export.py:1037` | Mesma empresa2/competência, cortes15 e21 | Alta, condicional | ESTRUTURA |
| 9b. Cache de pautas cruza tenants | `app/pautas/context_processors.py:23` | Staff pk2 em dois tenants reutiliza número/remetentes | Alta, condicional | ESTRUTURA |
| 10. Assistente usa cliente global | `app/core/views_ia.py:23`; `app/core/views_ia.py:909` | Pedido de cliente2 enviado com slug Juliani | Alta no direcionamento | ESTRUTURA |
| 11. Piso histórico de Juliani | `app/core/constants.py:5`; `app/ponto/services/fechamento.py:204` | Histórico de 04/05/2026 fora da fatia/previsto | Alta, condicional | ESTRUTURA |
| 12. Recuperação aponta schema errado | `app/folha/porta_export.py:810` | Export cliente2 orienta medir Juliani | Alta | ESTRUTURA |

## 05/11 16:13 · sessao 8869efe3186b · - · - · origem=Ronald (tmux)

Não encontrei falha estrutural de feriado/CCT que possa ser afirmada como inevitável nas quatro semanas; as ocorrências reais dependem de cadastros ausentes do anexo. [não verificado]

- **Risco condicional: feriados nacionais não cadastrados.** O reconhecimento lê exclusivamente linhas de `Feriado` pela data completa; a presença na tabela de importação não basta (`app/core/feriados.py:27`, `app/core/feriados.py:82`, `app/core/feriados.py:95`). A tabela admite 15/11, 20/11 e 25/12 (`app/core/views_config.py:118`).
  - **Contra-exemplo:** colaborador 5x2, `trabalha_em_feriado=False`, sexta-feira 20/11/2026 prevista, sem ausência nem folga manual, sem batidas; cadastro sem linha nacional de 20/11/2026. O conjunto não contém o feriado, e a precedência conserva trabalho (`app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:341`, `app/ponto/precedencia.py:382`, `app/ponto/precedencia.py:386`).
  - **Certeza alta sobre o mecanismo; ocorrência em produção [não verificado]. Especulação rotulada:** haverá cobrança de ausência se esse cadastro faltar; emissor específico não foi percorrido nesta investigação. **Dono CADASTRO**, pois data/abrangência são dados exigidos pelo juiz (`app/core/models.py:12`, `app/core/feriados.py:60`).
  - **Outro gatilho concreto:** feriado municipal cadastrado, posto do vínculo sem município. Esse feriado não alcança a pessoa; só nacional vale (`app/core/feriados.py:50`, `app/core/feriados.py:64`, `app/core/feriados.py:114`).

- **CCT até 31/01/2028 não é uma bomba de novembro/dezembro de 2026.** O literal está em comentário; execução compara a competência com campos de vigência do sindicato, inclusivamente (`app/core/regua_cct.py:129`, `app/core/regua_cct.py:261`).
  - **Contra-exemplo futuro, fora do horizonte:** empresa `cct`, aplicação única ativa, sindicato com fim em 31/01/2028, consulta de competência em 20/02/2028: aplicação descartada e retorno ao piso legal (`app/core/regua_cct.py:263`, `app/core/regua_cct.py:366`, `app/core/regua_cct.py:371`). **Certeza alta; dono CADASTRO se vigência não renovada.**
  - Vigência efetivamente cadastrada, aplicações ativas e eventuais empates: **[não verificado]**.

- **Datas literais:** execução somente de leitura de `core.datas_cravadas.censo(date(2026,11,5))` retornou 13 `APOIO`, 2 `MARCO`, zero `DECIDE`; função e classificação em `app/core/datas_cravadas.py:75`, `app/core/datas_cravadas.py:125`. Esse resultado **não prova ausência universal de bombas**: detector usa padrões específicos e limita anos a 2024–2030 (`app/core/datas_cravadas.py:25`, `app/core/datas_cravadas.py:123`). Busca AST adicional encontrou marco histórico `DNA_EPOCH`, que classifica acervo desde julho e não vence em novembro (`app/ponto/propositor.py:225`, `app/ponto/propositor.py:235`).

- **Virada de ano examinada, sem defeito demonstrado:** janelas tratam janeiro com dezembro do ano anterior e dezembro corrente com janeiro do próximo (`app/ponto/janelas.py:17`, `app/ponto/janelas.py:30`); extração de anomalias testa anos dos extremos da janela (`app/ponto/services/espelho.py:250`). Importação de feriados de 2027 continua dependendo de linhas completas para 2027, pelo mesmo risco de cadastro (`app/core/models.py:12`, `app/core/feriados.py:29`). Natal/ano novo são extrapolação além das quatro semanas ancoradas em 05/11. [não verificado]

## 05/11 16:13 · sessao 791f00a62eab · CODEX-SUFOCA-19-desenho-celula · leitura · origem=Code

**Eu escolheria materializar o casamento completo por marco: qual batida cumpriu o marco, em qual instante completo e com qual desvio já julgado.** Nome ilustrativo: `realizacao_por_marco`, ampliando `ata.lampadas`. **Proposta de desenho; certeza média sobre a prioridade.** Hoje o produtor calcula com instantes completos, reduz a hora a `HH:MM`, e a ata preserva essa forma reduzida; leitores depois reconstroem informação. (`app/escala/utils.py:325`, `app/ponto/services/cartorio.py:677`, `app/escala/services/leitor_celula.py:121`.)

O conteúdo proposto seria `chave_marco`, `batida_id`, `instante_real` com data/segundos/fuso e `delta_min` lavrado, preservando os atuais estados de ausência, dúvida, dispensa e isenção. **Especulação rotulada:** isso permitiria compartilhar o casamento já decidido, preservando precisão e identidade. A estrutura exata e sua implementação permanecem **[nao verificado]**; a distinção atual entre batida, dispensa e isenção aparece em `app/ponto/services/cartorio.py:556`, `app/ponto/services/cartorio.py:773` e `app/ponto/juiz_batida.py:252`.

O contra-exemplo mais direto é **sintético, não executado**: marco `07:00`, batida `07:10:59`. No ramo `_celula_do_marco`, o produtor arredonda o desvio para **11 minutos**; reconstruindo `luz=07:10`, o leitor obtém **10 minutos**. Com a tolerância de conformidade declarada de 10 minutos, isso permite produtor **“atraso”** e leitor **“ok”**. É divergência dedutível dessas funções; ocorrência nas células de produção fornecidas: **[nao verificado]**. (`app/escala/utils.py:325`, `app/escala/services/leitor_celula.py:29`, `app/escala/utils.py:307`, `app/ponto/motor_calculo_v2.py:60`.)

Não escolheria como novidade `status_dia`, `minutos_realizados` ou cobertura parcial: já existem na célula/ata. Também não trataria chamadas ao mesmo juiz como cópias independentes da lei: a folha consulta `periodos_do_dia`, e o leitor chama a autoridade comum `status_do_delta`. O problema comprovado inclui **informação descartada e posteriormente reconstruída**. (`app/escala/models.py:1774`, `app/escala/models.py:1816`, `app/ponto/services/cartorio.py:732`, `app/folha/export.py:1124`, `app/escala/services/leitor_celula.py:59`.)

**Limite da escolha:** identidade, instante e desvio eliminariam reconstruções específicas; não eliminariam sozinhos o cálculo do realizado na tela nem a decisão da folha sobre quais marcos faltam. Para alcançar também essas derivações, o passo seguinte seria materializar o quadro de períodos, pausas e estados produzido pelas autoridades existentes — **especulação de desenho**, com cobertura incompleta para turno partido já declarada no motor. (`app/ponto/services/espelho.py:675`, `app/folha/export.py:1127`, `app/ponto/juiz_batida.py:258`, `app/ponto/juiz_batida.py:305`, `app/ponto/motor_calculo_v2.py:559`.)

A escrita proposta deveria acompanhar a lavra, com escritor único e invalidação pelos insumos; esse é o contrato declarado para novos campos. Reusar cegamente o julgamento anterior merece cuidado: o produtor já documenta dependência da ata anterior durante a montagem. (`app/docs/ADR/0002-celula-e-o-dia.md:35`, `app/ponto/services/cartorio.py:570`.)

Não consegui verificar frequência, impacto financeiro, estado atual de qualquer `colNN`, sucesso dos testes ou qual alternativa produziria a maior redução quantitativa: o anexo fornecido contém autoridades, sem linhas de células/batidas **[nao verificado]**. Não executei testes nem consultei banco/log **[nao verificado]**. A correspondência criptográfica entre a cópia e o hash informado também ficou **[nao verificado]**. [nao verificado]

Na tabela, **ESTRUTURA** identifica a representação ou derivação discutida; não atribui automaticamente esse dono a um dia real. O juiz L-099 considera previamente condições de cadastro, batidas e fase. (`app/ponto/management/commands/e6_oraculo.py:98`.)

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 (ESTRUTURA/CADASTRO/BATIDA) |
|---|---|---|---|---|
| **Produtor versus leitor:** desvio completo é descartado e recalculado de `HH:MM`; pode mudar a classificação da lâmpada. | `app/escala/utils.py:325`; `app/ponto/services/cartorio.py:677`; `app/escala/services/leitor_celula.py:121` | Sintético: marco 07:00, batida 07:10:59 → produtor 11 min; leitor 10 min. | Alta estática; execução [nao verificado] | **ESTRUTURA** |
| **Juiz da batida:** recompõe data e instante usando marco e luz; no intermitente, encadeia a virada de dia. | `app/ponto/juiz_batida.py:86`; `app/ponto/juiz_batida.py:220`; `app/ponto/juiz_batida.py:241` | Sintético: marco D+1 00:05, batida D 23:58:47; `23:58` exige reconstruir a data e não preserva os 47 s. | Alta estática | **ESTRUTURA** |
| **Motor:** reencontra a batida por minuto para recuperar precisão; repete a operação nas pausas. | `app/ponto/motor_calculo_v2.py:596`; `app/ponto/motor_calculo_v2.py:639`; `app/ponto/motor_calculo_v2.py:691` | Sintético: luz 08:00 representa batida 08:00:59; a ata sozinha não entrega o instante usado no dinheiro. | Alta estática | **ESTRUTURA** |
| **Pareador:** cria outro índice por minuto e recusa papéis quando a luz não identifica uma única batida. | `app/ponto/turnos.py:1602`; `app/ponto/turnos.py:1615`; `app/ponto/turnos.py:1653` | Sintético: S 18:55:05 e E 18:55:55. O pareador conta duas; o índice do motor guarda a primeira por `setdefault`. Divergência financeira final [nao verificado]. | Alta para os caminhos | **ESTRUTURA** da representação; origem das batidas [nao verificado] |
| **Espelho:** solicita turnos e realizado por dia, reconstruindo também `sem_turno` e `aberto`; o mapa por marco seria insumo, não substituição automática desse cálculo. | `app/ponto/services/espelho.py:267`; `app/ponto/services/espelho.py:614`; `app/ponto/services/espelho.py:783` | Cenários escritos: E08/S17 com pausa declarada → 480 min; E08/S12/E13/S18 → 540 min. `app/relatorios/tests/test_pdf_realizado_do_dia.py:74`. | Alta para derivação; testes não executados | **ESTRUTURA**, se leitores divergirem com os mesmos insumos |
| **Calendário:** combina minutos da ata e ausência de turno para obter `ata_sem_turno`. | `app/colaboradores/services/calendario.py:718`; `app/ponto/services/dia_decidido.py:179` | Sintético: ata com 480 min e `sem_turno=True` → 480; mesma ata e `sem_turno=False` → 0. | Alta estática | **ESTRUTURA** |
| **Espelho, calendário e relatório da frota:** extraem separadamente os marcos faltantes. | `app/ponto/services/espelho.py:762`; `app/colaboradores/services/calendario.py:378`; `app/relatorios/dia_da_frota.py:20` | Sintético: apenas S19:00 apagada deve aparecer como faltante; ata agregada sem lâmpadas não informa qual marco faltou. | Alta estática | **ESTRUTURA** da derivação; batida realmente faltante exige classificação própria |
| **Folha/exportação:** deriva “furo só de pausa” dos papéis do juiz. Guardar somente identidade/instante não elimina essa leitura. | `app/folha/export.py:1124`; `app/folha/export.py:1127` | Testes escritos: entrada 08:02 e saída 17:03, pausa apagada → libera; entrada apagada → retém. `app/folha/tests/test_furo_so_intervalo_nao_retem.py:83`; `:94`. | Alta; testes não executados | **ESTRUTURA** para interpretação divergente; falta real pode ser **BATIDA** |
| **Limite de cadastro:** coluna nova não corrige vínculo ausente ou folga não definida. | `app/ponto/management/commands/e6_oraculo.py:98` | Sintético: `sem_vinculo_no_dia=True`, mesmo com contagem ímpar, retorna CADASTRO. | Alta estática | **CADASTRO** |
| **Limite de batida:** coluna nova não completa fato ausente. | `app/ponto/management/commands/e6_oraculo.py:104` | Sintético, sem os vetos anteriores de cadastro: `batida_fora_de_turno=True`, ou contagem ímpar, retorna BATIDA. | Alta estática | **BATIDA** |

## 05/11 16:12 · sessao de429d2875c1 · - · - · origem=Ronald (tmux)

Auditei seis itens/subitens. Somente leitura; as reproduções abaixo executaram, com `python3 -B`, funções/blocos extraídos por AST da própria cópia, em memória, sem importar Django, consultar ORM ou executar persistência. Commits e ocorrência em produção: [nao verificado]. [nao verificado]

1. **V3-LAVRAR-ATRASO-CREDITO — AINDA VALE. Certeza alta. Dono ESTRUTURA.** `lavrar` abate crédito em `app/ponto/services/dia_pago.py:190-195`, mas substitui `horas_atraso` pelo valor do calculador em `:320-328`. O calculador entrega atraso sem passar crédito em `app/ponto/calculador/regras.py:855-861`; o fechamento passa simultaneamente mapas de atraso/crédito e rubricas em `app/ponto/services/fechamento.py:1261-1272`. **Contraexemplo sintético reproduzido:** atraso de 60 min, crédito de 30 min no mesmo dia, rubrica `horas_atraso=1.0`: o trecho real de `lavrar`, interrompido antes da persistência, resulta em **1 h**, enquanto sem rubricas resulta em **0,5 h**; o detalhe continua registrando `credito_parcial_min=30` (`dia_pago.py:193-195`, `:328`). O fechamento mensal abate crédito em `app/ponto/services/fechamento.py:982-985`. Portanto a divergência entre as versões da lavra está demonstrada; impacto efetivo em TXT/produção: [nao verificado].

2. **V3-HE-NOTURNA-POR-PECA — AINDA VALE. Certeza alta. Dono ESTRUTURA.** O split calcula sua própria janela apenas em `[inicio_extra, fim]` em `app/ponto/motor_calculo_v2.py:440-443`; a prorrogação só se ativa se a própria chamada intersectar 00–05 (`:109-111`). **Contraexemplo sintético reproduzido:** entrada 22h, saída 07h seguinte, intervalo 05h30–06h30, `extra_50_min=60`, `extra_100_min=0`, `prorrogacao=True`: `he_noturna` retorna **(0,0)**. O início extra fica às 06h, de modo que a chamada isolada perde a prorrogação do turno original (`:378-405`, `:440-443`). A própria função do turno conta 540 minutos noturnos brutos, dos quais o intervalo consome 60 (`:82-118`, `:162-196`). A expectativa de 60 min no trecho extra decorre da regra de prorrogação já codificada; não constitui parecer jurídico externo. Frota com esse caso: [nao verificado].

3. **V3-NOTURNO-RECORTE-LONGO — AINDA VALE. Certeza alta. Dono ESTRUTURA.** A extensão de 00–05 até `fim` sobrepõe a faixa 22–24, somada separadamente em `app/ponto/motor_calculo_v2.py:100-116`; acima de 26h, a função retorna zero (`:93-95`). **Contraexemplos sintéticos reproduzidos:** 00h de um dia até 00h seguinte, prorrogação ligada, retorna **1.560 min para um recorte de 1.440 min**; 00h até 03h do segundo dia, 27h, retorna **0**. O primeiro já prova contagem superior à duração sem precisar interpretar legislação (`:110-116`). `calcular_noturnas` consome a função em `:983-987`; `MotorPlantao` reutiliza `Motor12x36.calcular_periodo` em `:3510-3511`. Não foi provada entrada de turnos >26h pelo pareador; a proteção do pareamento usa 26h (`:199-204`).

4. **V3-MOTOR-FERIADO-HE100-NOTURNA — AINDA VALE no ramo diário de Comercial/TurnoPartido. Certeza alta. Dono ESTRUTURA.** Comercial zera os splits em `app/ponto/motor_calculo_v2.py:3319-3324`; ao dobrar o feriado grava HE100 e executa `continue` em `:3337-3352`, antes do `_aplicar_he_noturna` de `:3399`. TurnoPartido repete o caminho em `:3095-3101`, antes de `:3147`. **Contraexemplo sintético reproduzido com o bloco real do Comercial:** um período de 480 min trabalhados, HE100 noturna previamente 480, dia com `_feriado_dobra_no_dia=True`: o bloco devolve **HE100=480, HE100_noturna=0**, com **zero chamadas ao split** (`:3324`, `:3340`, `:3352`). Cadastro ilustrativo: ciclo que dobra feriado, turno 22h–07h com 1h de intervalo; regra interna de cumulação documentada em `app/ponto/calculador/regras.py:793-798`. Não atribuir esse resultado a todos os motores: o ramo `Motor12x36` chega ao split depois da dobra (`motor_calculo_v2.py:2809-2813`, `:2855`).

5. **V3-P5-PRORROGACAO-POR-INTERVALO — JA RESOLVIDO NO CÓDIGO, para o defeito do desconto isolado. Certeza alta. Dono original ESTRUTURA.** O intervalo usa a evolução da janela do turno (`app/ponto/motor_calculo_v2.py:191-196`), após unir pausas sobrepostas (`:164-172`). **Antigo contraexemplo, agora bloqueado, reproduzido:** turno 22h–07h, intervalo 05h30–06h30, prorrogação ligada: `minutos_noturnos_dos_intervalos` retorna **60 min**, não zero (`:162-196`). O caso está explicitamente coberto por `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:111-115`; não executei a suíte. Esta resolução não elimina os itens 2 e 3: o split ainda consulta a janela do trecho extra (`motor_calculo_v2.py:441`), e o minuendo longo ainda sobreconta (`:110-116`). Commit: [nao verificado].

6. **V3-RETOMA-FILA, subitem (3), CCT por data do dia — AINDA VALE no calculador. Certeza alta quanto à seleção; média quanto ao resultado numérico final. Dono ESTRUTURA.** `rubricas_por_dia` consulta `get_motor_cct(c, ini, ...)` uma vez (`app/ponto/calculador/alimentacao.py:389-406`); depois passa as mesmas flags de dobra, prorrogação e hora reduzida para cada `dia=k` (`:514-529`). **Contraexemplo sintético por leitura:** janela iniciada em 21/10, mesma escala durante toda a janela, aplicação de convenção muda em 01/11 de prorrogação ligada para desligada; turno 02/11 22h–03/11 07h recebe a flag resolvida em 21/10, porque nenhuma nova consulta por `k` existe no bloco (`:390`, `:404`, `:528`). Atribuição temporal correta da convenção depende do cadastro efetivamente entregue ao juiz; não há cadastro de aplicação no dado raspado: [nao verificado]. Não extrapolar para toda a folha: o espelho já consulta por início da fatia de vínculo (`app/ponto/services/espelho.py:1111-1115`), embora isso também não demonstre divisão por mudança de CCT dentro da mesma fatia.

Limites: não medi dinheiro, colaboradores atingidos, frequência, deploy ou estado materializado; o anexo fornecido é uma tabela de autoridades, sem registros de jornada/cadastro suficientes para essas medições [nao verificado]. As conclusões acima são locais ao código citado; números antigos do backlog não foram tratados como dados atuais [nao verificado]. [nao verificado]

## 05/11 16:12 · sessao 24f7e3e691c5 · QUEIMA-B · sonda · origem=Code

O raspado não permite calcular as interseções: faltam as chaves [nao verificado]. Além disso, dev e vigia foram medidos em horários diferentes (`resumo_16h.txt:1`, `resumo_16h.txt:3`).

No H-12, **a chamada da medida 56/19/1 e a causa concreta da diferença +1/+1/0 permanecem [nao verificado]**. O registro encontrado aponta a exclusão de qualquer demissão não nula, sem comparar com a janela; ele não documenta aquele medidor anterior ([PROMPTS.md:1860](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/docs/PROMPTS.md:1860), [export.py:840](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/export.py:840)).

As referências das regras usadas abaixo são:

| Regra | Fonte |
|---|---|
| Dev: chamada original, captura dos dias e do fio, sem reconstruir o predicado | `hasner_fios_20261105.py:12`; [fora_da_folha.py:161](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/fora_da_folha.py:161), [fora_da_folha.py:178](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/fora_da_folha.py:178) |
| Empresas visíveis: ativas, excluindo diagnóstico | [queries.py:85](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/colaboradores/queries.py:85) |
| Competências: atual e anterior enquanto não entregue | [he_pendente_lavrado.py:220](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/he_pendente_lavrado.py:220) |
| Vigia conserva a primeira competência por `(colab_id, data)` | [vigia_fio.py:586](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/services/vigia_fio.py:586) |
| Lista completa; `sem_porta` entra independentemente do limite | [vigia_fio.py:624](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/services/vigia_fio.py:624) |
| Porta, hoje/futuro e baldes da lei, nessa ordem | [vigia_fio.py:641](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/services/vigia_fio.py:641), [vigia_fio.py:647](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/services/vigia_fio.py:647), [vigia_fio.py:653](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/services/vigia_fio.py:653) |
| Classes a..e: somente dias que chegaram a `sem_porta` | [vigia_fio.py:666](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/services/vigia_fio.py:666) |
| Dono atual pelo juiz real; dono lavrado da lista registrado separadamente | [e6_oraculo.py:168](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/management/commands/e6_oraculo.py:168), [vigia_fio.py:229](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/services/vigia_fio.py:229) |
| Congelamento também alcança `date.today()`, usado pelas autoridades | [requirements.txt:39](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/requirements.txt:39), [juizes.py:560](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/juizes.py:560) |
| H-12 do dev: IDs com fechamento e demissão estritamente posterior ao fim da janela da empresa | `hasner_auditoria_20261105.py:15`, `hasner_auditoria_20261105.py:16`, `hasner_auditoria_20261105.py:17`; [janelas.py:54](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/janelas.py:54) |

Execução e resultados da sonda em produção: [nao verificado]. A porta indicada cria temporário e acrescenta trilha; portanto sua execução também envolve escrita ([sonda_leitura.sh:216](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/bin/sonda_leitura.sh:216), [sonda_leitura.sh:248](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/bin/sonda_leitura.sh:248)).

```python
import sys
sys.dont_write_bytecode = True

import datetime as dt
import importlib
import io
import json
import logging
import os
from collections import defaultdict
from contextlib import redirect_stdout, redirect_stderr
from unittest.mock import patch

logging.disable(logging.CRITICAL)


def sem_escrita(evento, args):
    if evento == "open":
        modo, flags = args[1], args[2] or 0
        if (isinstance(modo, str) and any(x in modo for x in "wax+")) or (
            flags & (
                os.O_WRONLY | os.O_RDWR | os.O_CREAT
                | os.O_TRUNC | os.O_APPEND
            )
        ):
            raise RuntimeError("sonda_interrompida")
    if evento in {
        "os.remove", "os.rmdir", "os.rename", "os.mkdir", "os.link",
        "os.symlink", "os.chmod", "os.chown", "os.utime", "os.truncate",
        "subprocess.Popen", "os.system",
    }:
        raise RuntimeError("sonda_interrompida")


sys.addaudithook(sem_escrita)


def sql_leitura(execute, sql, params, many, context):
    if sql.lstrip().split(None, 1)[0].upper() not in {
        "SELECT", "SHOW", "WITH", "SAVEPOINT", "RELEASE", "ROLLBACK",
    }:
        raise RuntimeError("sonda_interrompida")
    return execute(sql, params, many, context)


def medir():
    from django.db import connection, transaction
    from django.utils import timezone
    from freezegun import freeze_time
    from colaboradores.models import Empresa, Colaborador
    from colaboradores.queries import empresas_visiveis
    from ponto.models import FechamentoMensal
    from ponto.janelas import janela_fechamento
    from ponto.management.commands.e6_oraculo import InsumosDoDono
    from chamados.services.vigia_fio import (
        ListaDoAdmin, censo_alcance, BALDES_FORA_DO_FIO, ROTULO_CAUSA,
    )

    fdf = importlib.import_module("folha.fora_da_folha")
    assert connection.settings_dict["USER"] == "leitor"
    assert connection.vendor == "postgresql"
    assert not connection.in_atomic_block
    assert connection.get_autocommit()

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION "
                "ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cur.execute("SELECT current_user, transaction_timestamp()")
            usuario, agora = cur.fetchone()
            assert usuario == "leitor"

        with timezone.override("America/Sao_Paulo"):
            hoje = timezone.localdate(agora)
            with freeze_time(agora), patch(
                "freezegun.api.FakeDate.today", return_value=hoje
            ), connection.execute_wrapper(sql_leitura):
                empresas = list(Empresa.objects.all())
                visiveis = {e.pk for e in empresas_visiveis()}
                dev, universo_dev, export_dev = set(), {}, {}
                totais_dev = []

                for mes in (10, 11):
                    comp = "%02d/2026" % mes
                    for emp in empresas:
                        capturas, fios = [], []
                        real_u = fdf.dias_que_seguram
                        real_f = fdf._fio_do_dia

                        def captar_u(*a, **kw):
                            u = real_u(*a, **kw)
                            capturas.append(u)
                            return u

                        def captar_f(celula_id, alim):
                            fio = real_f(celula_id, alim)
                            fios.append((celula_id, fio))
                            return fio

                        with patch.object(
                            fdf, "dias_que_seguram", captar_u
                        ), patch.object(fdf, "_fio_do_dia", captar_f):
                            r = fdf.fora_da_folha(emp, mes, 2026)

                        assert len(capturas) == 1
                        u = capturas[0]
                        sequencia = [
                            (cid, dia)
                            for cid in u["furo"] if cid in u["dias"]
                            for dia in u["dias"][cid][1]
                        ]
                        assert len(sequencia) == len(fios)
                        locais = []

                        for (cid, dia), (celula_id, fio) in zip(
                            sequencia, fios
                        ):
                            assert celula_id == dia["celula_id"]
                            k = (cid, str(dia["data"]), comp)
                            assert k not in universo_dev
                            universo_dev[k] = (emp.pk, fio)
                            if fio == "nada":
                                locais.append(k)

                        assert (
                            len(locais)
                            == len(set(locais))
                            == r["dias_por_fio"]["nada"]
                        )
                        assert not (dev & set(locais))
                        dev.update(locais)

                        for item in u["itens"]:
                            export_dev[
                                (item["colaborador_id"], comp)
                            ] = (
                                item["status"],
                                item.get("motivo") or "",
                            )

                        totais_dev.append({
                            "empresa_id": emp.pk,
                            "competencia": comp,
                            "n": len(locais),
                        })
                        del r, u, capturas, sequencia, fios

                lista = ListaDoAdmin(hoje)
                v = censo_alcance(
                    hoje=hoje, lista=lista, limite=float("inf")
                )
                assert v["alcance_dias_omitidos"] == 0
                assert (
                    len(v["alcance_dias"]) == v["alcance_universo"]
                )

                todas_v, por_dia = {}, {}
                for (
                    cel, cid, data, comp, rota, causa, detalhe, papel
                ) in v["alcance_dias"]:
                    k = (cid, data, comp)
                    assert k not in todas_v
                    assert (cid, data) not in por_dia
                    todas_v[k] = (rota, causa, detalhe, papel)
                    por_dia[(cid, data)] = comp

                vigia = {
                    k for k, row in todas_v.items()
                    if row[0] == "sem_porta"
                }
                assert len(vigia) == v["fio_morto_alcance"]
                assert (
                    sum(v["alcance_" + c] for c in "abcde")
                    == len(vigia)
                )
                comps_v = {
                    (eid, comp)
                    for eid, comp in v["alcance_competencias"]
                }
                grupos = {
                    "so_dev": dev - vigia,
                    "so_vigia": vigia - dev,
                    "em_ambos": dev & vigia,
                }
                assert len(dev) == (
                    len(grupos["so_dev"]) + len(grupos["em_ambos"])
                )
                assert len(vigia) == (
                    len(grupos["so_vigia"]) + len(grupos["em_ambos"])
                )

                colabs = (
                    Colaborador.objects.select_related("empresa")
                    .in_bulk({k[0] for k in dev | vigia})
                )
                insumos = {}

                def dono(k):
                    cid, data, comp = k
                    ck = (cid, comp)
                    try:
                        with transaction.atomic():
                            if ck not in insumos:
                                mes, ano = map(int, comp.split("/"))
                                c = colabs[cid]
                                ini, fim = janela_fechamento(
                                    mes, ano, c.empresa
                                )
                                insumos[ck] = InsumosDoDono(
                                    c, ini, fim, hoje, agora=agora
                                )
                            return insumos[ck].dono(
                                dt.date.fromisoformat(data)
                            ) or "nao medido"
                    except Exception:
                        insumos.pop(ck, None)
                        return "nao medido"

                def regra(grupo, k, eid):
                    cid, data, comp = k
                    if grupo == "em_ambos":
                        return "nada_e_sem_porta"
                    if grupo == "so_vigia":
                        if comp not in {"10/2026", "11/2026"}:
                            return "recorte_fixo_dev"
                        if k in universo_dev:
                            return "fio_dev:" + universo_dev[k][1]
                        status, motivo = export_dev.get(
                            (cid, comp),
                            ("nao medido", "nao medido"),
                        )
                        return (
                            "universo_dev:" + status + ":" + motivo
                        )
                    if eid not in visiveis:
                        return "empresas_visiveis"
                    if (eid, comp) not in comps_v:
                        return "competencias_a_lavrar"
                    if k in todas_v:
                        return "rota:" + todas_v[k][0]
                    if (cid, data) in por_dia:
                        return "competencia_retida_por_setdefault"
                    return "nao medido"

                saida_grupos = []
                campos = (
                    "empresa_id", "competencia", "regra",
                    "rota", "papel", "balde_lei", "classe",
                    "causa", "detalhe", "competencia_vigia",
                    "dono_l099", "dono_lavrado_lista",
                )

                for grupo, chaves in grupos.items():
                    partes = defaultdict(list)
                    for k in sorted(chaves):
                        cid, data, comp = k
                        eid = colabs[cid].empresa_id
                        rota, classe, detalhe, papel = todas_v.get(
                            k, ("nao medido", "", "", "nao medido")
                        )
                        lavrado = lista.dia_na_lista(cid, data)
                        dono_lista = (
                            lavrado[1]
                            if lavrado and lavrado[0] and lavrado[1]
                            else "nao medido"
                        )
                        assinatura = (
                            eid, comp, regra(grupo, k, eid),
                            rota, papel,
                            rota if rota in BALDES_FORA_DO_FIO else "",
                            classe or "nao medido",
                            ROTULO_CAUSA.get(classe, ""),
                            detalhe,
                            por_dia.get((cid, data), ""),
                            dono(k), dono_lista,
                        )
                        partes[assinatura].append([cid, data, comp])

                    quebras = [
                        dict(
                            zip(campos, assinatura),
                            n=len(ks), chaves=ks,
                        )
                        for assinatura, ks in sorted(partes.items())
                    ]
                    assert (
                        sum(q["n"] for q in quebras) == len(chaves)
                    )
                    saida_grupos.append({
                        "grupo": grupo,
                        "n": len(chaves),
                        "quebras": quebras,
                    })

                h12, h12_totais = [], []
                for mes in (9, 10, 11):
                    comp = "%02d/2026" % mes
                    for emp in empresas:
                        ini, fim = janela_fechamento(mes, 2026, emp)
                        ids = set(
                            FechamentoMensal.objects.filter(
                                colaborador__empresa=emp,
                                mes=mes, ano=2026,
                            ).values_list("colaborador_id", flat=True)
                        )
                        rows = list(
                            Colaborador.objects.filter(
                                pk__in=ids, data_demissao__gt=fim
                            ).order_by("pk").values_list(
                                "pk", "data_demissao"
                            )
                        )
                        h12_totais.append({
                            "empresa_id": emp.pk,
                            "competencia": comp,
                            "n": len(rows),
                        })
                        h12.extend({
                            "empresa_id": emp.pk,
                            "colab_id": cid,
                            "competencia": comp,
                            "data_demissao": dem.isoformat(),
                            "fim": fim.isoformat(),
                        } for cid, dem in rows)

                return {
                    "instante": agora.isoformat(),
                    "hoje": hoje.isoformat(),
                    "conciliacao": {
                        "n_dev": len(dev),
                        "n_vigia": len(vigia),
                        "n_so_dev": len(grupos["so_dev"]),
                        "n_so_vigia": len(grupos["so_vigia"]),
                        "n_em_ambos": len(grupos["em_ambos"]),
                        "dev_por_empresa_competencia": totais_dev,
                        "vigia_universo": v["alcance_universo"],
                        "vigia_por_rota_papel":
                            v["alcance_por_rota_papel"],
                        "lista_sem_resposta": lista.sem_resposta,
                        "grupos": saida_grupos,
                    },
                    "H12_dev": {
                        "por_empresa_competencia": h12_totais,
                        "por_competencia": {
                            comp: sum(
                                x["competencia"] == comp for x in h12
                            )
                            for comp in (
                                "09/2026", "10/2026", "11/2026"
                            )
                        },
                        "n": len(h12),
                        "chaves": h12,
                    },
                }


try:
    with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
        resultado = medir()
except Exception:
    print(json.dumps({"sonda_incompleta": 1}))
    raise SystemExit(1)

print(json.dumps(resultado, ensure_ascii=False))
```

## 05/11 16:12 · sessao 3eac4b4e016f · - · - · origem=Ronald (tmux)

Julgamento por leitura estática; nenhum teste executado, banco ou log consultado. Ocorrência em produção, quantidade afetada e commit de correção: [nao verificado]. [nao verificado]

1. **COBPARC-AUSENCIA-CALA-DOIS-JUIZES — AINDA VALE.** O materializador cala ausência não-falta sem período em `app/chamados/services/materializacao.py:78-82`; `_celula_furada` só cala cobertura integral em `app/chamados/juizes.py:446-448`. Contraexemplo **sintético**: atestado vencedor de 240 minutos, dia único, tipo habilitado para parcial, sem `hora_ini/hora_fim`; célula `trabalha=True`, ata `tipo_dia=ausencia`, `cobertura_parcial=True` e lâmpada apagada. O materializador responde coberto; `_celula_furada` percorre a ata e responde furado (`app/chamados/juizes.py:424-426`, `:475-479`). A ausência de período retorna `sem_periodo` (`app/ponto/turnos.py:2632-2633`) e os 240 minutos caracterizam parcial (`:2598-2600`). **Certeza alta; dono ESTRUTURA pela composição divergente; o registro sem período tem dono CADASTRO**, distinção já expressa em `app/chamados/services/materializacao.py:69-70`. Efeito financeiro específico: [nao verificado].

2. **COBPARC-FUROS-LEITOR-PARCIAL — JÁ RESOLVIDO no defeito nominal dos dois leitores.** Ambos incluem ausência parcial usando o mesmo predicado: `app/escala/services/furos_diarios.py:42-50`; `app/inteligencia/calculadores.py:29`, `:49-51`; predicado em `app/chamados/juizes.py:398-408`. Contraexemplo **sintético de regressão**: ata `tipo_dia=ausencia`, `cobertura_parcial=True`, marco dispensado dentro de 08–12h e marco apagado às 17h. Esse dia já não é excluído pelo rótulo ausência; FuroDiario conta os marcos do jogo em `app/escala/services/furos_diarios.py:17-23`. **Certeza alta; dono original ESTRUTURA.** Os quatro outros sítios citados na linha do backlog (`simular_dia`, `veredito_celula`, `disputa_emissao`, `folha/causas`) não foram julgados nesta fatia: [nao verificado].

3. **JUSTIFICATIVA-ADMIN-FORA-DA-PORTA — AINDA VALE, com ressalva.** O administrador continua `admin.ModelAdmin` editável, sem mixin `SoLeitura` ou porta própria (`app/ponto/admin.py:18-22`), e `status`, responsável e data são campos do modelo (`app/ponto/models.py:159-164`). Contraexemplo **sintético**: usuário com permissão de alteração no Django Admin muda justificativa pendente com origem viva para `aprovada`; não passa pelo trecho que invalida fechamento e encerra origem em `app/ponto/services/justificativa.py:199-203`, nem pelo log de domínio em `:214-219`. **Ressalva:** não afirmar “nenhum evento/trilha”: o `save` dispara julgamento do dia (`app/ponto/signals.py:161-165`), e trilha padrão do Django Admin não foi auditada [nao verificado]. **Certeza alta para desvio de porta; dono ESTRUTURA.**

4. **GRADE-FOLGA-GERADOR — AINDA VALE.** O gerador busca foto apenas pelo próprio vínculo, tanto no feed (`app/escala/models.py:1280-1288`) como no teste da data (`:1309-1310`). O juiz aceita folga manual de qualquer vínculo da pessoa (`app/ponto/precedencia.py:235-239`). Contraexemplo **sintético**: vínculo A encerrado guarda folga manual em 10/11; vínculo B ativo gera esse dia e o ciclo de B prevê trabalho. A folga de A é aceita pelo juiz, mas não entra no gerador de B, que retorna a previsão do ciclo (`app/escala/models.py:1365-1371`). **Certeza alta; dono ESTRUTURA.** Valor de falta/HE decorrente: [nao verificado].

5. **FASE-CONFLITANTE-VINCULO-DO-DIA — AINDA VALE.** O medidor seleciona somente `ativa=True`, carrega fotos do próprio vínculo e aplica esse vínculo a todos os dias da janela (`app/escala/services/fase_conflitante.py:71-80`), enquanto o juiz do vínculo declara a geradora da célula como autoridade (`app/escala/alimentacao.py:228-233`). Contraexemplo **sintético**: B ativo começa em 01/11, foto de folga 02/11 e âncora 02/11; medição em 05/11 com janela de 60 dias aplica B também em outubro, quando a célula pertence a A. Foto e âncora de B discordam inclusive nesses dias, contados por `:80-89`. **Certeza alta; dono ESTRUTURA do instrumento.** O módulo publica métrica, sem escrita de folha nesse caminho (`app/escala/services/fase_conflitante.py:92-102`).

6. **GRADE-FOLGA-C5 — AINDA VALE.** O auditor reúne FolgaDia de qualquer vínculo que tenha cobertura própria ou outro vínculo cobrindo a data, sem aplicar o predicado de origem da folga (`app/chamados/management/commands/auditar_invariantes_chamados.py:1531-1537`). Depois acusa toda célula `trabalha=True` em competência não exportada (`:1541-1551`). Contraexemplo **sintético**: folga de calendário de A encerrado em data agora gerada por B, que prevê trabalho. O juiz rejeita essa folga de calendário (`app/ponto/precedencia.py:237-239`), mas o C5 a inclui por existir B e acusa a célula correta. **Certeza alta; dono ESTRUTURA do auditor.**

7. **ISENTO-PONTO-JUIZ-REGISTRADO — AINDA VALE parcialmente.** A função declara “JUIZ UNICO” e lê apenas a flag (`app/core/constants.py:13-18`); permanecem leituras diretas no cartório (`app/ponto/services/cartorio.py:1229`, `:1246`, `:1419`, `:1806`), embora a impressão já use a função (`:1140-1141`, `:1354`). Busca textual não encontrou `isento_ponto` no registro nem em JUIZES; ausência demonstrada por comando, sem arquivo:linha pontual [nao verificado]. **Não repetir “sem frase no CORTES”**: há corte posterior ISENTO-H10 em `app/docs/CORTES.md:419`, `:421`. Contraexemplo **sintético de manutenção**: alterar a interpretação da função sem alterar a flag faz a impressão ler a função e a classificação continuar lendo a flag; hoje a função é equivalente à flag (`app/core/constants.py:18`). **Certeza alta para leituras paralelas; média para pendência de registro; dono ESTRUTURA.**

8. **ISENTO-FLAG-ATEMPORAL — AINDA VALE para a falta de vigência; NAO SE SABE para o alcance integral descrito.** A isenção continua um booleano sem data (`app/colaboradores/models.py:291-294`; `app/core/constants.py:13-18`). Contraexemplo **sintético**: alterar a flag hoje e rejulgar um dia de outubro muda o insumo de isenção daquele dia (`app/ponto/services/cartorio.py:1140-1141`, `:1354`). **A alegação “histórico inteiro, inclusive exportada” precisa ser restringida**: a impressão só incorpora isenção em `regra >= 4` (`app/ponto/services/cartorio.py:520-521`), com horizonte comp10+ documentado em `:514-519`. A fonte dos testes protege a impressão de comp09 (`app/ponto/tests/test_e3_isento_na_impressao.py:78-100`); execução desses testes [nao verificado]. **Certeza alta para atemporalidade; baixa para impacto histórico efetivo; dono ESTRUTURA se a exigência é representar vigência; vigência real de cada pessoa [nao verificado].**

## 05/11 16:12 · sessao 71558872d3b8 · - · - · origem=Ronald (tmux)

Auditei sete itens por leitura do código. Não executei testes, importei a aplicação nem acessei banco/logs; resultados de produção, quantidade afetada, valores pagos e commits reais permanecem [nao verificado]. [nao verificado]

| Item | Veredito e evidência | Contraexemplo sintético / comportamento atual | Certeza | Dono L-099 |
|---|---|---|---|---|
| **H12-DEMISSAO-NA-JANELA** | **AINDA VALE.** A documentação diz “demissão na janela”, mas o ramo testa somente `data_demissao is not None`, sem comparar com a competência: `app/folha/export.py:834`, `app/folha/export.py:840`. O item está em `app/docs/BACKLOG.md:1741`. | Competência 10/2026, janela 21/09–20/10, fechamento existente e demissão **10/11/2026**: `classificar_export` já devolve `fora/rescisao_modulo_proprio`, embora a demissão esteja após a janela. A exclusão ocorre antes de outros crivos: `app/folha/export.py:840`, `app/folha/export.py:845`. | Alta, por fluxo explícito; ocorrência em prod [nao verificado]. | **ESTRUTURA**, para esse cenário com cadastro válido; não é prova do dono de qualquer colab real. |
| **H06-FALTA-PELA-GRADE** | **AINDA VALE.** A materialização escreve `minutos=720`: `app/chamados/services/materializacao.py:519`. `Ausencia.save` só deriva jornada se os minutos forem zero: `app/ponto/models.py:875`. O fechamento lê o rateio, soma e grava horas_falta: `app/folha/export.py:110`, `app/ponto/services/fechamento.py:865`, `app/ponto/services/fechamento.py:987`. Item: `app/docs/BACKLOG.md:1742`. | Cadastro com jornada prevista **480 min**, ausência diária nova e resposta `FALTA_DIA`: a tabela manda criar falta aprovada (`app/chamados/services/materializacao.py:577`); o escritor passa **720**, o modelo não rederiva, e o rateio de um dia mantém **720 → 12 h**, excedendo a jornada de 8 h. | Alta para criação e soma; números “106 dias/38/441,87 h” são alegação do backlog, reprodução [nao verificado]. | **ESTRUTURA**, pois o próprio escritor inventa 720 com cadastro de 480. |
| **STATUS-ENCERRADO-DA-COMPETENCIA-NAO-EXISTE** | **AINDA VALE**, na ausência do estado solicitado. O modelo declara somente `aberto/aprovado/exportado`: `app/ponto/models.py:399`; o TXT aceita apenas `aberto/aprovado`: `app/folha/export.py:668`, `app/folha/export.py:698`. Item: `app/docs/BACKLOG.md:2113`. | Competência que precisa representar “encerrada”: não há esse valor entre as escolhas. **Cenário hipotético, sem afirmar linha existente:** se uma instância vier com `status='encerrado'` e todos os crivos de `classificar_export` a deixarem entrar, o montador ainda a elimina pela tupla de status; o classificador não filtra status (`app/folha/export.py:764`). | Alta para ausência do estado; média para impacto futuro de introduzi-lo sem migrar leitores. | **ESTRUTURA**, para o contrato ausente. |
| **FECHAMENTO-REFERENCIA-POR-POSICAO** | **JA RESOLVIDO no código.** A referência agora percorre os dias de trás para frente e chama `vinculo_do_dia`: `app/ponto/services/fechamento.py:111`, `app/ponto/services/fechamento.py:125`; o fechamento usa essa função: `app/ponto/services/fechamento.py:842`. O backlog já o declara fechado e atribui commit `10f42d4f`: `app/docs/BACKLOG.md:1709`; existência/autoria do commit [nao verificado]. | Vínculo A começa 25/09 e termina 05/10; B começa 01/09 e permanece cobrindo 20/10. A antiga posição escolheria A; o código atual pergunta por 20/10 e escolhe B. Sem vínculo em nenhum dia, retorna `None`. Esses casos estão no teste existente: `app/ponto/tests/test_referencia_pergunta_a_juiza.py:28`, `app/ponto/tests/test_referencia_pergunta_a_juiza.py:40`, `app/ponto/tests/test_referencia_pergunta_a_juiza.py:45`; teste não executado [nao verificado]. | Alta por implementação; execução dos testes [nao verificado]. | **ESTRUTURA**, defeito anterior corrigido. |
| **FECH-10 — ranking chama status de lavra** | **JA RESOLVIDO no código.** `_carimbo` pergunta `competencia_entregue` para todas as empresas das linhas: `app/folha/services/ranking_he.py:85`, `app/folha/services/ranking_he.py:87`. A entrada velha está em `app/docs/BACKLOG.md:2057`; registro vivo documenta sua retirada em `app/core/juizes.py:567`. | Fechamento **aberto**, mas TXT vigente entregue: atual retorna `lavrado`; fechamento **aprovado** sem entrega: retorna `parcial`. Testes existentes desses casos: `app/folha/tests/test_k6_competencia_entregue.py:169`, `app/folha/tests/test_k6_competencia_entregue.py:176`; execução [nao verificado]. | Alta. | **ESTRUTURA**, defeito anterior corrigido. |
| **FECH-11 — `_limites` do holerite como competência encerrada** | **JA RESOLVIDO quanto à pergunta declarada, por enquadramento corrigido; não é uma cura de `_limites`.** `_limites` recebe ano/mês e produz mês civil (`app/holerite/matriz.py:51`), usado para universo (`app/holerite/matriz.py:89`). Quem escolhe a competência recente já chama `janela_anterior`: `app/holerite/matriz.py:39`. Registro vivo declara explicitamente “MAL ARQUIVADO” e limita a conformidade: `app/core/juizes.py:590`, `app/core/juizes.py:602`. Entrada velha: `app/docs/BACKLOG.md:2058`. | Em **21/09/2026**, corte padrão 21, `competencias_recentes(n=1)` escolhe **09/2026** pela janela anterior; `_limites(2026, 9)` continua produzindo 01–30/09 para selecionar o universo civil. A distinção é visível nos dois corpos: `app/holerite/matriz.py:41`, `app/holerite/matriz.py:52`. | Alta para as duas funções; persistência do contrato externo Domínio/mês civil [nao verificado]. | **ESTRUTURA**, item documental mal enquadrado; nenhum dono individual de dado foi medido. |
| **FECH-12 — ranking escolhe último fechamento existente** | **JA RESOLVIDO no código.** `competencia_default` chama `janela_anterior` e devolve mês/ano do fim: `app/folha/services/ranking_he.py:72`; `ranking` usa esse default: `app/folha/services/ranking_he.py:106`. Entrada velha: `app/docs/BACKLOG.md:2059`. | Hoje **04/10/2026**, corte 21, fechamentos em setembro e outubro: o atual abre **09/2026**, mesmo existindo linha parcial na 10. Sem linha na 09, continua escolhendo 09 e o carimbo responde `nao_apurado` (`app/folha/services/ranking_he.py:85`). Casos existentes: `app/holerite/test_fech_encerrada_pelo_juiz.py:86`, `app/holerite/test_fech_encerrada_pelo_juiz.py:121`; execução [nao verificado]. | Alta. | **ESTRUTURA**, defeito anterior corrigido. |

A classificação estrutural acima é a do contraexemplo descrito, não uma atribuição de dono a dados de produção. O juiz real prioriza cadastro sem vínculo/folga, depois batida fora de turno ou contagem ímpar, depois fase/cadastro: `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:110`.

**Limites:** não verifiquei pagamento efetivo, frequência, frota, passivo ou aplicação em produção [nao verificado]. Os números históricos e commits presentes em comentários/documentação são evidência de registro, não prova independente de execução [nao verificado]. [nao verificado]

## 05/11 16:11 · sessao f6f7a767bf95 · - · - · origem=Ronald (tmux)

Achados estáticos, sem afirmar ocorrência em produção:

1. **Anexo exigido pelo domínio some do contrato do app — certeza alta; ESTRUTURA.** `/api/chamados/v2/` só preenche `anexo_pendente` quando `_a.status == 'pendente'`, e devolve `null` nos demais casos (`app/api/views.py:1071`, `app/api/views.py:1156`). Os estados abertos atuais são `aguardando_documento` e `aguardando_decisao` (`app/ponto/services/ausencia.py:749`); atestado sem documento nasce como `aguardando_documento` (`app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:779`). O próprio endpoint de documento aceita esses estados (`app/api/views.py:3101`). **Contra-exemplo:** colaborador com chamado visível de ausência, atestado sem arquivo e estado `aguardando_documento`: GET anuncia `anexo_pendente:null`, mas POST do documento é permitido. O teste específico ainda fabrica `status='pendente'` (`app/api/tests/test_api_chamado_anexo_pendente.py:32`). Exibição efetiva da tela nativa: [não verificado].

2. **Regularização perde o significado da operação no JSON — certeza alta; ESTRUTURA.** GET `/api/regularizacao/pendente/` entrega por item apenas `data`, `hora_entrada` e `hora_saida` nos dois kernels (`app/api/views.py:2980`, `app/api/views_core.py:1298`). O domínio também usa `acao`, `batida_alvo` e `intervalo` (`app/chamados/models.py:3202`, `app/chamados/models.py:3214`). **Contra-exemplo:** dois itens com mesma data e `hora_saida=18:00`, um `adicionar` e outro `corrigir`, ficam indistinguíveis no JSON; no serviço, corrigir altera a batida existente e adicionar cria uma (`app/chamados/services/regularizacao_ext.py:142`, `app/chamados/services/regularizacao_ext.py:184`). Um item `remover`, com horas nulas, sai como `{data, hora_entrada:null, hora_saida:null}` e sua confirmação apaga `batida_alvo` (`app/chamados/services/regularizacao_ext.py:131`). A tela web explicita a operação usando `descricao_humana` (`app/templates/chamados/meu_atendimento.html:14`, `app/chamados/models.py:3237`). UI específica do APK: [não verificado]. O estado é criável pela UI da supervisão, que aceita as três ações e exige o alvo para corrigir/remover (`app/chamados/views_regularizacao.py:102`, `app/chamados/views_regularizacao.py:128`).

3. **Horários da API podem discordar da célula congelada — certeza alta; ESTRUTURA.** `/api/me/` obtém `trabalha_hoje` pelo vínculo/célula, mas horários diretamente do template (`app/api/views.py:259`, `app/api/views_core.py:248`); `/api/escala/v2/` repete a mistura (`app/api/views.py:934`, `app/api/views.py:939`). A autoridade de marcos do vínculo lê o DNA da célula antes do template (`app/escala/models.py:1122`). **Contra-exemplo:** célula de hoje `trabalha=True`, DNA `hi=07:00,hf=19:00`; template atualmente `08:00–20:00`. `EscalaColaborador.marcos_do_dia` devolve 07–19, mas API anuncia 08–20 (`app/escala/models.py:1124`, `app/api/views.py:261`). Cadastro real com essa configuração: [não verificado].

4. **Erro de materialização responde HTTP 200 — certeza alta do comportamento; ESTRUTURA.** Confirmar regularização retorna `ok:false` e `erros`, sem status HTTP de falha (`app/api/views.py:3013`, `app/api/views_core.py:1331`). **Contra-exemplo:** lote `aguardando` e disputa de supervisão aberta para a pessoa; confirmação altera o lote para `confirmada`, mas materializador devolve `materializada=False`, zero criadas e erro (`app/chamados/services/regularizacao_ext.py:352`, `app/chamados/services/regularizacao_ext.py:58`). Resposta resultante: HTTP200, `ok:false`, `status:"confirmada"`, `batidas_criadas:0`. O cliente nativo reconhecer ou ignorar `ok:false`: [não verificado]; não afirmar falso sucesso visual como fato.

5. **Dois GETs discordam sobre opção que o POST recusa — certeza alta sob o estado descrito; ESTRUTURA.** `/api/chamados/v2/` entrega `p.opcoes` sem filtro (`app/api/views.py:1171`). `/api/disputa/perguntas/` remove `OUTRO_MOTIVO` das perguntas de hora (`app/api/views.py:2553`). POST responde 400 se o core recusar (`app/api/views.py:2642`), e o core recusa literalmente `OUTRO_MOTIVO` para `tipo_resposta='hora'` (`app/chamados/services/disputa_emissao.py:1263`). **Contra-exemplo:** pergunta viva de hora em disputa aberta com `opcoes=[{"valor":"OUTRO_MOTIVO","rotulo":"Outro motivo"}]`: chamados/v2 anuncia opção que o POST rejeita; o outro GET já a remove. O estado é fixture explícita (`app/chamados/tests/test_escape_outro_motivo.py:9`), mas o catálogo atual já não oferece essa opção (`app/chamados/tests/test_escape_outro_motivo.py:16`); existência de linhas legadas em produção: [não verificado].

Observação relacionada ao achado 1: `/api/ausencias/` ainda traduz `status_display` apenas para `pendente/aprovada/rejeitada`; estados atuais abertos saem como código bruto `aguardando_documento`/`aguardando_decisao` (`app/api/views.py:1875`). É efeito do mesmo leitor não migrado, não achado independente.

Não comprovado: mudança histórica de formato sem versão. As rotas antigas de espelho, escala, chamados e holerites devolvem 410 e apontam explicitamente para v2 (`app/api/views.py:837`, `app/api/views.py:876`); há rotas v2 distintas (`app/api/urls.py:72`). `MIN_SUPORTADO=1` em app/version não prova que algum cliente instalado continua chamando v1 (`app/api/views.py:3060`). Sem fonte/build do APK e sem dados de produção, compatibilidade do app distribuído permanece [não verificado].

## 05/11 16:11 · sessao 8e0fa962801a · - · - · origem=Ronald (tmux)

Achados da leitura estática; ocorrência e quantidade em produção [não verificado]. Todos abaixo são defeitos de **ESTRUTURA**, porque os mesmos dados atravessam caminhos com resultados diferentes; a separação L-099 está em `CLAUDE.md:176`.

1. **Fila offline pode mudar de titular. Certeza alta.** O IndexedDB usa nome fixo, sem usuário (`app/static/pwa.js:50`); itens guardam tipo, horários, GPS e foto, sem colaborador (`app/static/pwa.js:113`). A sincronização só exige perfil “colaborador” (`app/static/pwa.js:128`), envia todas as pendências (`app/static/pwa.js:132`) e o servidor toma o colaborador da sessão corrente (`app/api/views_core.py:389`). O logout apenas encerra a sessão e redireciona (`app/core/views.py:79`). **Contraexemplo construído:** A guarda E offline; sai sem sincronizar; B entra no mesmo navegador/origem; a pendência de A é enviada e registrada para B. A fila apaga o item no 200 (`app/static/pwa.js:158`). Uso compartilhado real [não verificado].

2. **Compensação do relógio é calculada e descartada no contrato. Certeza alta.** O PWA calcula gap pelo timestamp do servidor (`app/static/pwa.js:90`), guarda horário corrigido e bruto (`app/static/pwa.js:109`, `app/static/pwa.js:115`), mas envia somente o bruto (`app/static/pwa.js:147`). A API usa esse bruto como hora efetiva quando estiver nas últimas 48h (`app/api/views_core.py:529`, `app/api/hora_do_aparelho.py:141`). **Contraexemplo construído:** hora real 08:00; aparelho 07:50; gap conhecido +600000 ms; fila guarda `timestamp=08:00`, mas manda `timestamp_dispositivo=07:50`; servidor grava 07:50. Não é prova de erro de timezone: o ISO do servidor tem offset (`app/core/views.py:23`), e o PWA usa `toISOString()` (`app/static/pwa.js:115`).

3. **Fila com mais de 48h muda para a hora da chegada, perde a marcação original e enfraquece idempotência. Certeza alta do caminho estático.** A fila não impõe prazo (`app/static/pwa.js:138`); o parser substitui a hora antiga por `agora` (`app/api/hora_do_aparelho.py:148`); a resposta informa sucesso (`app/api/views_core.py:823`) e o PWA apaga o item (`app/static/pwa.js:162`). Não envia chave própria do evento (`app/static/pwa.js:143`); a chave única persistida é colaborador+timestamp efetivo (`app/ponto/models.py:106`), além dos dedups por janela (`app/api/views_core.py:549`, `app/ponto/registro_batida.py:106`). **Contraexemplo construído:** E de segunda 08:00 sincroniza quarta 09:00, grava quarta 09:00; resposta se perde; reenvio após mais de cinco minutos pode gravar novo instante, pois `agora` mudou. Para evitar dependência de marcos, usar cadastro sem marcos e intervalo superior às janelas de dedup/colisão (`app/ponto/registro_batida.py:21`). Execução/prod [não verificado].

4. **Offline perde precisão GPS e muda o julgamento do mesmo fix. Certeza alta.** A fila guarda `gps_accuracy` (`app/static/pwa.js:117`), mas não envia esse campo no FormData (`app/static/pwa.js:143`). A API lê ausente como None (`app/api/views_core.py:422`), grava None (`app/api/views_core.py:688`), e o juiz devolve `gps_incerto` quando falta precisão (`app/ponto/services/geofence.py:117`). O envio online v2 inclui precisão (`app/templates/ponto/bater_ponto_v2.html:332`). **Contraexemplo construído:** posto raio150 m, fix a500 m e accuracy10 m: online classifica `fora` (`app/ponto/services/geofence.py:121`); mesmo fix pela fila offline classifica `gps_incerto` (`app/ponto/services/geofence.py:118`).

5. **Dedup API pode devolver como sucesso uma batida retratada. Certeza alta.** O filtro ±5min da API usa colaborador/tipo/instante, sem `retratada_em__isnull=True` (`app/api/views_core.py:551`), e devolve `ja_registrada`, `sucesso=True` (`app/api/views_core.py:558`). A porta de escrita filtra retratadas na janela (`app/ponto/registro_batida.py:107`), mas fica depois desse retorno antecipado (`app/api/views_core.py:679`). **Contraexemplo construído:** E08:00 retratada; nova E08:04; API devolve a E retratada, PWA apaga a nova pendência no 200 (`app/static/pwa.js:158`). Resultado: nenhuma nova E ativa.

6. **PWA v2 anuncia o tipo pedido e ignora o tipo/aviso devolvidos pela autoridade. Certeza alta.** Há rota acessível v2 (`app/ponto/urls.py:12`, `app/ponto/views.py:356`). O JS aceita qualquer `r.ok`, sem ler JSON, e chama sucesso com o tipo local (`app/templates/ponto/bater_ponto_v2.html:340`); a mensagem diz Entrada/Saída pelo argumento (`app/templates/ponto/bater_ponto_v2.html:150`). A API pode corrigir o tipo (`app/api/views_core.py:592`) e devolve tipo real, timestamp, mensagem e aviso (`app/api/views_core.py:823`). **Contraexemplo construído:** duas abas sugerem E; primeira registra E; segunda fica aberta menos de5min desde que ficou visível, mas envia após mais de5min da primeira E para ultrapassar dedup: servidor corrige para S e tela anuncia “Entrada registrada”. O caso pode ser montado com aba já carregada sugerindo E antes da primeira marcação; prevalência real [não verificado]. Avisos de dia explicado enviados no campo `aviso` também são ignorados (`app/api/views_core.py:833`, `app/templates/ponto/bater_ponto_v2.html:340`).

7. **Service worker não preenche o cache HTML no fluxo normal que promete fallback offline. Certeza alta.** Install precacheia somente JS, manifest e ícones (`app/static/sw.js:3`); qualquer navegação entra no ramo que retorna cedo, sem `cache.put` (`app/static/sw.js:45`). O ramo que grava `/ponto/bater/` e `/` só aparece depois (`app/static/sw.js:67`). Sem cópia, fallback retorna erro de rede (`app/static/sw.js:29`). **Contraexemplo construído:** instalação nova, navegação normal para dashboard/ponto, fechar aba e abrir URL sem rede: não houve preenchimento HTML por esses caminhos. Outros agentes/cache de navegador poderiam alterar a experiência [não verificado].

8. **Background Sync sem página aberta não envia a fila. Certeza alta, comportamento explícito.** O SW envia mensagens às páginas se existirem; caso contrário apenas mostra aviso para abrir o app (`app/static/sw.js:116`). A promessa “Enviada automaticamente quando a internet voltar” está no v2 (`app/templates/ponto/bater_ponto_v2.html:126`). **Contraexemplo construído:** registra offline, fecha todas as páginas, internet retorna: SW só notifica; fila depende de abrir página colaborador (`app/static/pwa.js:128`). Entrega efetiva do aviso pelo navegador [não verificado].

Limites: não executei testes, não consultei DB/logs e não demonstrei ocorrência em produção [não verificado]. O APK nativo não está nesta árvore segundo o próprio parser (`app/api/hora_do_aparelho.py:3`); o que ele efetivamente envia permanece [não verificado]. Não há evidência de mudança histórica de formato nesta leitura estática; isso exige comparação temporal [não verificado].

## 05/11 16:11 · sessao 0741fa0a343b · - · - · origem=Ronald (tmux)

Minha escolha neste recorte é **registrar em cada lâmpada o vínculo inequívoco `marco → Batida`**, com `batida_id` e instante completo, incluindo data, segundos e fuso. É proposta de desenho, não constatação de benefício medido; o máximo ganho entre todos os leitores permanece **[não verificado]**.

A perda atual está explícita: o produtor possui `instante`, calcula o delta e reduz o casamento a `hora: '%H:%M'`; a ata persiste `tipo`, `hora`, `acesa`, `luz` e `tipo_real`, sem identidade nem instante completo. (`app/escala/utils.py:314-327`; `app/ponto/services/cartorio.py:677-690`.)

| Achado | arquivo:linha | Contra-exemplo concreto | Certeza | Dono L-099 |
|---|---|---|---|---|
| Juiz precisa reconstruir data e instante da batida usando marco + luz HH:MM, incluindo correção ±1 dia | `app/ponto/juiz_batida.py:86-103`; `app/ponto/juiz_batida.py:236-242` | **Sintético:** marco D+1 00:05, batida D 23:58:47; `luz=23:58` exige inferir dia e perde 47 s. Ocorrência em produção **[não verificado]** | Alta sobre mecanismo; média sobre prioridade | ESTRUTURA |
| Motor precisa reencontrar Batida por minuto para recuperar segundos; escolhe a primeira no minuto com `setdefault` | `app/ponto/motor_calculo_v2.py:582-609` | **Sintético:** duas batidas no mesmo minuto, 18:55:05 S e 18:55:55 E; o índice por minuto não identifica qual acendeu cada marco. Ocorrência em produção **[não verificado]** | Alta estática | ESTRUTURA |
| Pareador constrói outro índice por minuto e recusa atribuir papéis se houver mais de uma batida no minuto | `app/ponto/turnos.py:1602-1615`; `app/ponto/turnos.py:1636-1655` | Mesmo exemplo sintético: `_bats_min=2` faz a ata deixar de alimentar os papéis, enquanto motor mantém a primeira batida naquele minuto. Não afirmo divergência final de dinheiro sem execução **[não verificado]** | Alta para os caminhos; efeito final não verificado | ESTRUTURA |
| Delta/status do marco já é calculado no produtor, descartado pela ata e recalculado no leitor | `app/escala/utils.py:325-327`; `app/ponto/services/cartorio.py:677-690`; `app/escala/services/leitor_celula.py:119-132` | **Sintético:** marco 07:00, batida 07:05:59: produtor arredonda diferença completa para 6 min; leitor com HH:MM refaz 5 min. Ocorrência real **[não verificado]** | Alta sobre aritmética | ESTRUTURA |
| `classificar_falta` ainda toma decisão por tipos, contagens e turno aberto; coluna nova não elimina isso automaticamente | `app/escala/servico_jornada.py:440-477` | **Sintético:** DNA de duas pausas tem 6 marcos; o esperado desta função é montado pela API de quatro horários em `:427-435`, sem `hii2/hfi2`. Efeito integral depende dos guardas e pareamento **[não verificado]** | Alta sobre leitura de quatro horários; média sobre efeito | ESTRUTURA |
| `procedencia_fina` recalcula saldo E/S do universo de batidas | `app/escala/servico_jornada.py:521-534` | **Sintético:** batidas E/E/E/E cumprindo quatro marcos posicionais continuam produzindo saldo 4 nesse algoritmo; resultado local `falta_real`, independentemente de um casamento posicional bem-sucedido. Alcance real **[não verificado]** | Alta para algoritmo local | ESTRUTURA |

**Por que delta/status não é minha primeira escolha:** ambos já chamam a mesma autoridade `status_do_delta`; não são duas cópias da regra. Além disso, a função declara que essa gradação pinta a lâmpada e **não é** a tolerância de desconto da folha. (`app/escala/utils.py:299-311`; `app/escala/services/leitor_celula.py:58-60`.) Guardar delta preservaria precisão e evitaria rematerialização, mas não resolveria a identificação ambígua por minuto.

**Já existe; não vender como próximo fato:** cobertura parcial, período e dispensa por marco já são lavrados e transportados; a antiga derivação de cobertura no leitor foi removida, com lápide explícita. (`app/ponto/services/cartorio.py:558-562`; `app/ponto/services/cartorio.py:729-737`; `app/escala/services/leitor_celula.py:404-407`; `app/escala/services/leitor_celula.py:419-422`.)

Leitores diretos do juiz que rematerializa instantes e períodos: motor (`app/ponto/motor_calculo_v2.py:616`), pareador (`app/ponto/turnos.py:1631`), exportação (`app/folha/export.py:1124`) e cadastro versus realidade (`app/escala/services/cadastro_realidade.py:212`). São chamadas de um juiz existente; chamá-las de clones da lei seria extrapolação. O juiz também deriva pausas completas, duração, fonte cadastrada e estado aberto. (`app/ponto/juiz_batida.py:259-313`.)

Limite da proposta: identidade + instante não resolvem sozinhos batidas órfãs, precedência, tipo de ausência, decisão humana nem geometrias incompatíveis; o próprio motor exige que a ata explique as batidas antes de substituir períodos. (`app/ponto/motor_calculo_v2.py:622-645`.) Benefício de guardar toda a geometria realizada, quantidade de leitores eliminados, estado das células reais e impacto financeiro: **[não verificado]**.

_1816 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
