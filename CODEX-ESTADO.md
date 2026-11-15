# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 01:42** · publicado em **15/11/2026 01:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 01:42 · sessao a186d9d5c966 · - · - · origem=Ronald (tmux)

Confirmado: há arredondamento intermediário do subtotal mensal, seguido de outro ao acrescentar folga sem escala certa; não há arredondamento diário nesse caminho. (`app/ponto/services/fechamento.py:902–903`, `app/ponto/services/fechamento.py:980–985`)

O exemplo solicitado reproduz **FM − soma = +0,01 h**:

| Peça | Minutos | Horas |
|---|---:|---:|
| Dia comum 1 | 482 | 8,033333… |
| Dia comum 2 | 482 | 8,033333… |
| Folga sem escala certa | 481 | 8,016666… |

As conversões correspondem aos caminhos de `DiaPago` em `app/ponto/services/dia_pago.py:178` e `app/ponto/services/dia_pago.py:264`.

- FM: `round((482+482)/60, 2) = 16,07`; depois `round(16,07 + 481/60, 2) = 24,09`. (`app/ponto/services/fechamento.py:902–903`, `app/ponto/services/fechamento.py:984–985`)
- Soma lavrada: `round(482/60 + 482/60 + 481/60, 2) = 24,08`. (`app/ponto/services/dia_pago.py:178`, `app/ponto/services/dia_pago.py:264`, `app/ponto/services/dia_pago.py:562–563`)

Verifiquei a conta com `python3 -B`, sem importar a aplicação e sem escrever arquivos; sua reprodução fica sustentada pelas expressões citadas acima.

Quanto a outros escritores: `restaurar_fechamento` aceita campos válidos do modelo, inclui `horas_trabalhadas` entre os possíveis, e os entrega diretamente a `.update(**_upd)`, sem recalcular nem acrescentar arredondamento explícito nessa função. (`app/ponto/services/fechamento.py:1373–1380`, `app/ponto/services/fechamento.py:1391–1393`)

A gravação ordinária persiste pelo `.save()` para status aberto ou pelo `.update(horas_trabalhadas=...)` no outro ramo. (`app/ponto/services/fechamento.py:1137–1144`, `app/ponto/services/fechamento.py:1160–1162`)

Não encontrei outra atribuição operacional explícita a esse campo fora dessas rotas na busca textual; exaustividade contra atribuições dinâmicas externas: [não verificado].

## 15/11 01:42 · sessao e3c7fb0de668 · - · - · origem=Ronald (tmux)

- O comparador disponível é `app/folha/porta_export.py::_medir_dia_pago`: chama `soma_da_competencia` em `:388`, lê cada campo do fechamento em `:451-452` e compara os valores convertidos em `float`, sem novo arredondamento, em `:455`. A soma já veio arredondada a duas casas (`app/ponto/services/dia_pago.py:559-563`).
- A tolerância é **0,015 h** (`app/folha/porta_export.py:239`). Portanto esse comparador **não acusa diferença de exatamente 0,01 h** (`app/folha/porta_export.py:455`). Só depois de decidir a divergência ele arredonda ambos os lados igualmente para o registro de saída (`app/folha/porta_export.py:456-458`).
- `linha_x_badge` também lê a soma (`app/folha/porta_export.py:267-268`), compara badge contra soma com a mesma tolerância (`:272-276`) e combina trabalhadas com folga trabalhada (`:245-248`).
- O selo concreto disponível chama `medir` e `lavrar` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88-92`), conta elementos das listas (`:106-107`) e imprime contagens (`:153-155`). O contador `fechamento_x_soma_dias_pagos` fica publicado, fora de `falhas` (`app/folha/porta_export.py:687-697`; rótulo explícito em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:67`). Seus detalhes ficam em `MetricaSnapshot` (`app/folha/porta_export.py:772-782`).
- O TXT consulta o carimbo desse selo (`app/folha/views.py:128-133`); `conferir` lê `MetricaSnapshot` e testa seu valor (`app/folha/porta_export.py:807-821`). Não mostra outro total de trabalhadas nesse caminho (`app/folha/porta_export.py:800-821`).
- `e6_oraculo.dono_da_divergencia` classifica cadastro/batida/estrutura (`app/ponto/management/commands/e6_oraculo.py:98-110`); isso não demonstra a fórmula da comparação da sonda q4b. A identificação do código de **m2**, **selo de silêncio** e da **sonda específica SILENCIO-VENCIDO/q4b**, ausentes como fontes identificáveis na cópia pesquisada, permanece **[não verificado]**.

## 15/11 01:42 · sessao 54e5daabd0c0 · - · - · origem=Ronald (tmux)

Leitores encontrados, distinguindo código executável dos comentários antigos:

- **Lista de fechamento:** lê os `FechamentoMensal` salvos, passa-os a `montar_resumos`, que conserva o objeto em `r.fechamento`; a coluna mostra `horas_trabalhadas|floatformat:2`. Caminho: `app/ponto/views.py:1137-1154`, `app/ponto/services/fechamento_lista.py:80`, `app/ponto/services/fechamento_lista.py:105-107`, `app/templates/ponto/_fech_linha.html:30`.
- **Relatório “Horas por colaborador” e CSV:** usa diretamente o FM gravado; HTML com duas casas e CSV com o próprio Decimal. `app/relatorios/views.py:68-73`, `app/relatorios/views.py:89-95`, `app/templates/relatorios/horas_mes.html:67`.
- **Painel individual de fechamento:** lê apenas o `pk` do gravado, mas o número vem de `fechamento_lido`; template com duas casas. `app/ponto/views.py:993-996`, `app/templates/ponto/partials/painel_fechamento.html:32`.
- **Espelho (tela/app) e resumo do PDF:** para competência identificada, passam por `folha_manda`, que primeiro chama `totais_da_folha`. Esta exige existência de FM, mas lê **`fechamento_lido` derivado agora**, não as horas do FM salvo. Mapeia `fe.horas_trabalhadas` a `total_trabalhadas`. `app/relatorios/cartao_pela_celula.py:69-76`, `app/relatorios/cartao_pela_celula.py:111`, `app/relatorios/cartao_pela_celula.py:321-346`. `fechamento_lido` chama o mesmo recálculo com `somente_leitura=True, retornar_objeto=True`: `app/ponto/services/fechamento.py:1759-1761`.
- **Fallback do espelho/PDF:** se não obtiver totais daquele fechamento, `_totais_da_lavratura` lê `soma_da_competencia` e mapeia sua `horas_trabalhadas` a `total_trabalhadas`; escolhido por `folha_manda`. `app/relatorios/cartao_pela_celula.py:142-146`, `app/relatorios/cartao_pela_celula.py:174`, `app/relatorios/cartao_pela_celula.py:340-346`.
- **Formatação desses resumos:** espelho exibe uma casa decimal; PDF converte a horas/minutos inteiros, com `round((horas-h)*60)`. Portanto nem toda diferença centesimal será visível nesses dois formatos. `app/templates/ponto/espelho.html:39`, `app/relatorios/pdf_espelho.py:766`, `app/relatorios/pdf_espelho.py:667-678`. Entradas de tela/PDF em `app/ponto/services/espelho.py:1283-1284` e `app/relatorios/pdf_espelho.py:628`.
- **Linhas diárias:** tela usa rubricas do `DiaPago` (trabalhadas com uma casa); PDF reaproveita o dict de dias da tela, e seu “Trabalhado” usa `pago_h`, que soma trabalhadas+folga+feriado pertinente, arredondado a duas casas no serviço e depois formatado por `_h`. Não confundir esse “pago” com `horas_trabalhadas` isolada. `app/ponto/services/espelho.py:805-813`, `app/ponto/services/dia_pago.py:898`, `app/templates/ponto/espelho.html:273-274`, `app/relatorios/pdf_espelho.py:596-598`, `app/relatorios/pdf_espelho.py:901-902`, `app/relatorios/pdf_espelho.py:981`.
- **Extrato parcial:** lê soma de `DiaPago` por intervalo (`soma_do_periodo`, não diretamente `soma_da_competencia`); arredonda o total de trabalhadas em duas casas e exibe duas casas, inclusive no CSV. Atenção: janela própria do mês civil expandido por 12h e limitado a agora; não presumir igualdade de universo com competência. `app/relatorios/views.py:463-471`, `app/relatorios/views.py:481-487`, `app/relatorios/views.py:523`, `app/templates/relatorios/extrato_parcial.html:73`, `app/relatorios/views.py:560-561`.
- **TXT Domínio:** o gerador atual recalcula objetos em leitura e os injeta na montagem; não confiar no comentário antigo de que todo chamador usa gravado. `app/folha/views.py:207-209`, `app/folha/views.py:265-268`. **Nenhum dos dois totais T é emitido como rubrica de trabalhadas**: a lista de eventos contém HE50, HE100, feriado, folga, noturno, intra e atraso. `app/folha/export.py:439-461`. Cada valor emitido vira centésimos com `int(round(float(valor)*100))`. `app/folha/export.py:34-45`. O classificador/prontidão lê `fech.horas_trabalhadas` para condições/retorno, mas isso não transforma T em linha TXT. `app/folha/export.py:831`, `app/folha/export.py:902`, `app/folha/export.py:1036`.

Par concreto de leitores de **fontes diferentes**: relatório/lista que lê FM salvo versus extrato parcial que soma DiaPago, desde que comparados sobre intervalo comum; já espelho e PDF compartilham o mesmo seletor de fonte, portanto atribuir a eles uma oposição fixa “FM versus soma diária” seria incorreto. Fontes acima: `app/relatorios/views.py:68`, `app/relatorios/views.py:487`, `app/relatorios/cartao_pela_celula.py:321-346`.

## 15/11 01:42 · sessao bfb6a09b5bab · BATIDA-DEDUP-RETRATADA · sonda · origem=Code

```python
# Django shell; tenant juliani ja ativo. Producao: [nao verificado].
# Guardas e UTC-03 conforme q3a fornecida: [nao verificado].
# Convencoes desta sonda: "-" = inaplicavel; "[nao verificado]" = desconhecido.
# FALHA_T4 informa a primeira condicao que falharia na q3a fornecida.
# Ausencia/divergencia diagnosticada nao incrementa ERRO; falha de leitura sim.
# Correspondencia e resultados atuais em producao: [nao verificado].

erros = 0
nivel_logging_anterior = None
pergunta_atual = None


def erro(etapa, exc):
    global erros
    erros += 1
    alvo = str(pergunta_atual) if pergunta_atual is not None else "SONDA"
    print(
        "P{}_ERRO_{} {}".format(alvo, etapa, type(exc).__name__),
        flush=True,
    )


try:
    import logging
    import re
    from contextlib import ExitStack
    from datetime import timedelta, timezone as dt_timezone
    from unittest.mock import patch

    from django.db import connection, connections
    from django.utils import timezone
    from django.utils.dateparse import parse_datetime

    nivel_logging_anterior = logging.root.manager.disable
    logging.disable(logging.CRITICAL)

    alias_leitura = connection.alias
    local = dt_timezone(timedelta(hours=-3))

    def somente_select(execute, sql, params, many, context):
        if (
            many
            or context["connection"].alias != alias_leitura
            or not re.match(r"\s*SELECT\b", sql, re.I)
            or ";" in sql
            or re.search(
                r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b",
                sql,
                re.I,
            )
        ):
            raise RuntimeError("consulta_recusada")
        return execute(sql, params, many, context)

    # app/core/observ.py:73; app/chamados/juizes.py:1473 e :1486.
    def best_effort_falhou(logger_nome, operacao, exc, **contexto):
        erro("BEST_EFFORT", exc)

    with ExitStack() as pilha:
        for alias in connections:
            pilha.enter_context(
                connections[alias].execute_wrapper(somente_select)
            )
        pilha.enter_context(timezone.override(local))
        pilha.enter_context(
            patch("core.observ.registrar_engolido", new=best_effort_falhou)
        )

        from chamados.models import PerguntaDisputa
        from chamados.juizes import tranca_da_consulta
        from ponto.models import Batida
        from ponto.janelas import janela_atual
        from folha.export import competencia_entregue

        with connection.cursor() as cursor:
            cursor.execute("SELECT current_schema()")
            if cursor.fetchone()[0] != "juliani":
                raise RuntimeError("tenant_incorreto")

        cache_exportada = {}

        def competencia_no_dia(empresa, dia):
            # app/ponto/janelas.py:60; app/folha/export.py:556 e :588.
            _, fim = janela_atual(dia, empresa)
            chave = (empresa.pk, fim.month, fim.year)
            if chave not in cache_exportada:
                cache_exportada[chave] = competencia_entregue(*chave)
            return (
                "{:04d}-{:02d}".format(fim.year, fim.month),
                int(cache_exportada[chave]),
            )

        def sondar(pk, dados):
            # Consulta direta aos tres PKs, sem excluir perguntas agora mortas.
            # Vida exata: app/chamados/models.py:3474.
            try:
                p = (
                    PerguntaDisputa.objects.using(alias_leitura)
                    .select_related(
                        "disputa__colaborador__empresa",
                        "celula",
                        "chamado_destino",
                        "batida_origem",
                    )
                    .defer(
                        "disputa__colaborador__nome",
                        "disputa__colaborador__cpf",
                    )
                    .get(pk=pk)
                )
            except PerguntaDisputa.DoesNotExist as exc:
                dados["PERGUNTA_EXISTE"] = 0
                erro("PERGUNTA", exc)
                return

            dados["PERGUNTA_EXISTE"] = 1

            # app/chamados/models.py:2494, :2511 e :1783.
            dados["COLABORADOR_DISPUTA_PK"] = p.disputa.colaborador_id
            dados["COLABORADOR_PERGUNTA_PK"] = p.colaborador_id

            # app/colaboradores/models.py:321.
            empresa = p.disputa.colaborador.empresa
            dados["EMPRESA_Q3A_PK"] = empresa.pk

            ub = None
            bid = None
            dia_contexto = None
            dia_batida = None

            try:
                # Contexto: app/chamados/models.py:1792.
                # Formato efetivo destes registros: [nao verificado].
                ctx = p.disputa.contexto
                if ctx is None:
                    ctx = {}
                if not isinstance(ctx, dict):
                    raise ValueError("contexto_invalido")

                ub = ctx.get("ultima_batida")
                if not isinstance(ub, dict):
                    raise ValueError("ultima_batida_ausente_ou_invalida")

                bruto_id = ub.get("id")
                if (
                    isinstance(bruto_id, bool)
                    or not isinstance(bruto_id, (int, str))
                    or not re.fullmatch(r"[0-9]+", str(bruto_id))
                    or int(bruto_id) <= 0
                ):
                    raise ValueError("batida_id_invalido")

                bid = int(bruto_id)
                dados["ULTIMA_BATIDA_ID"] = bid
            except BaseException as exc:
                erro("CONTEXTO_ID", exc)

            if isinstance(ub, dict):
                try:
                    bruto_ts = ub.get("timestamp")
                    if not isinstance(bruto_ts, str):
                        raise ValueError("timestamp_ausente_ou_invalido")

                    ts = parse_datetime(bruto_ts)
                    if ts is None or timezone.is_naive(ts):
                        raise ValueError("timestamp_invalido_ou_sem_fuso")

                    dia_contexto = ts.astimezone(local).date()
                    dados["DIA_CONTEXTO_LOCAL"] = dia_contexto.isoformat()
                except BaseException as exc:
                    erro("CONTEXTO_DIA", exc)

            if bid is not None:
                try:
                    # app/ponto/models.py:22, :23 e :67.
                    b = (
                        Batida.objects.using(alias_leitura)
                        .filter(pk=bid)
                        .values(
                            "pk", "colaborador_id", "timestamp", "retratada_em"
                        )
                        .first()
                    )
                    dados["BATIDA_EXISTE"] = int(b is not None)

                    if b is None:
                        dados["FALHA_T4"] = "batida_referenciada_ausente"
                        for chave in (
                            "BATIDA_RETRATADA",
                            "COLABORADOR_BATIDA_PK",
                            "DIA_BATIDA_LOCAL",
                            "COMPETENCIA_BATIDA",
                            "EXPORTADA_BATIDA",
                        ):
                            dados[chave] = "-"
                    else:
                        dados["BATIDA_RETRATADA"] = int(
                            b["retratada_em"] is not None
                        )
                        dados["COLABORADOR_BATIDA_PK"] = b["colaborador_id"]

                        cid = p.disputa.colaborador_id
                        dados["FALHA_T4"] = (
                            "colaborador_divergente"
                            if (
                                b["colaborador_id"] != cid
                                or p.colaborador_id != cid
                            )
                            else "nenhuma_das_duas"
                        )

                        if timezone.is_naive(b["timestamp"]):
                            raise ValueError("batida_timestamp_sem_fuso")

                        dia_batida = b["timestamp"].astimezone(local).date()
                        dados["DIA_BATIDA_LOCAL"] = dia_batida.isoformat()
                except BaseException as exc:
                    erro("BATIDA", exc)

            try:
                # app/chamados/juizes.py:1128;
                # app/chamados/models.py:3484.
                antes = erros
                with tranca_da_consulta([p]):
                    viva = int(p.viva)

                if erros == antes:
                    dados["VIVA"] = viva
            except BaseException as exc:
                erro("VIDA", exc)

            # Usar a empresa da disputa para ambos os dias, conforme q3a
            # fornecida. FK: app/colaboradores/models.py:321.
            # Consultar exportacao mesmo se houver divergencia ou pergunta morta.
            for rotulo, dia in (
                ("CONTEXTO", dia_contexto),
                ("BATIDA", dia_batida),
            ):
                if dia is None:
                    continue
                try:
                    comp, exportada = competencia_no_dia(empresa, dia)
                    dados["COMPETENCIA_" + rotulo] = comp
                    dados["EXPORTADA_" + rotulo] = exportada
                except BaseException as exc:
                    erro("EXPORTACAO_" + rotulo, exc)

        campos = (
            "PERGUNTA_EXISTE",
            "ULTIMA_BATIDA_ID",
            "FALHA_T4",
            "BATIDA_EXISTE",
            "BATIDA_RETRATADA",
            "COLABORADOR_DISPUTA_PK",
            "COLABORADOR_PERGUNTA_PK",
            "COLABORADOR_BATIDA_PK",
            "DIA_CONTEXTO_LOCAL",
            "DIA_BATIDA_LOCAL",
            "VIVA",
            "EMPRESA_Q3A_PK",
            "COMPETENCIA_CONTEXTO",
            "EXPORTADA_CONTEXTO",
            "COMPETENCIA_BATIDA",
            "EXPORTADA_BATIDA",
        )

        for pk in (115, 116, 1339):
            pergunta_atual = pk
            dados = dict.fromkeys(campos, "[nao verificado]")
            try:
                sondar(pk, dados)
            except BaseException as exc:
                erro("ITEM", exc)
            finally:
                for chave in campos:
                    print(
                        "P{}_{} {}".format(pk, chave, dados[chave]),
                        flush=True,
                    )

        pergunta_atual = None

except BaseException as exc:
    erro("SONDA", exc)
finally:
    try:
        if nivel_logging_anterior is not None:
            logging.disable(nivel_logging_anterior)
    except BaseException as exc:
        erro("RESTAURA_LOGGING", exc)

    print("ERRO {}".format(erros), flush=True)
    print("FIM ok={}".format(int(erros == 0)), flush=True)
```

## 15/11 01:35 · sessao 0f4c61a1ae54 · SILENCIO-VENCIDO · sonda · origem=Code

```python
"""
SONDA SILENCIO-VENCIDO. Execucao na SOMBRA: [nao verificado].

FechamentoMensal esta em app/ponto/models.py:398; o arquivo pedido,
app/folha/models.py:310, referencia esse modelo. Os campos solicitados
sao totais mensais, sem decomposicao diaria:
app/ponto/models.py:405, :408, :412, :432, :440, :445.
Por isso, DIA.gravado=null; gravado_competencia informa o total mensal.

Lavra diaria: mesma chamada da m2 colada [nao verificado por arquivo local],
linhas_do_periodo(colab, ini, fim), ds.get(dia), getattr(dp, campo).
Contrato: app/ponto/services/dia_pago.py:833 e :855.
Soma mensal: app/ponto/services/dia_pago.py:550.

InsumosDoDono.dono(data) carrega os insumos e chama dono_da_divergencia:
app/ponto/management/commands/e6_oraculo.py:182, :190, :195.
Preparo usado pelo command: mesmo arquivo:316, :328, :378.

Sem gravado diario, percorremos TODOS os dias de janela_fechamento:
app/ponto/janelas.py:54. O dono e diario; sua responsabilidade pela
diferenca mensal continua [nao verificado]. Nao elegemos dono mensal.

TOT unidade=dia_campo: empresa x campo individual x dono; conta DIA.
As incidencias de pares podem se sobrepor entre campos e donos.
TOT unidade=par: empresa x conjunto exato de campos x conjunto de donos
encontrados; cada par completo entra uma vez, fechando 59.
O conjunto de donos e um perfil, nao um novo veredito mensal.
59 pares e 65 ocorrencias de campos: [nao verificado — anexo colado].

FIM ok=1 indica coleta completa sem excecao, nao divergencia resolvida.
Bloqueios baseados no anexo m2_sonda.py [nao verificado por arquivo local]
e em app/chamados/management/commands/diff_e3_sombra.py:30.
"""
import sys

_BYTECODE_ANTERIOR = sys.dont_write_bytecode
sys.dont_write_bytecode = True

import datetime as dt
import json
import logging
import re
from collections import Counter, defaultdict
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from decimal import Decimal, localcontext
from unittest.mock import patch

# Relogio da m2 colada: [nao verificado por arquivo local].
AGORA = dt.datetime.fromisoformat("2026-11-15T02:20:00+00:00")

R = "minutos_realizados"
A = "minutos_abonados"
P = "minutos_previstos"
T = "horas_trabalhadas"

# (empresa, colab, mes, ano, #fechamento, campos divergentes).
# Lista literal do m2_extrato.txt colado: [nao verificado por arquivo local].
PARES = (
    (2, 174, 9, 2026, 4825, (R,)),
    (2, 191, 9, 2026, 4518, (R,)),
    (2, 193, 9, 2026, 4801, (R,)),
    (2, 206, 9, 2026, 4418, (R,)),
    (2, 211, 9, 2026, 4648, (R,)),
    (2, 214, 9, 2026, 4668, (A,)),
    (2, 231, 9, 2026, 4699, (R,)),
    (2, 242, 9, 2026, 4503, (T, R)),
    (2, 252, 9, 2026, 4796, (R,)),
    (2, 269, 9, 2026, 4569, (R,)),
    (2, 272, 9, 2026, 4828, (A,)),
    (2, 278, 9, 2026, 4750, (T, R)),
    (2, 292, 9, 2026, 4671, (A, R)),
    (2, 331, 9, 2026, 4737, (R,)),
    (2, 349, 9, 2026, 4666, (R,)),
    (2, 355, 9, 2026, 4782, (A,)),
    (2, 371, 9, 2026, 4564, (R,)),
    (2, 444, 9, 2026, 4571, (R,)),
    (2, 489, 9, 2026, 4552, (R,)),
    (2, 529, 9, 2026, 4698, (R,)),
    (2, 532, 9, 2026, 4508, (R,)),
    (2, 592, 9, 2026, 4512, (R,)),
    (2, 616, 9, 2026, 4703, (R,)),
    (2, 634, 9, 2026, 4442, (R,)),
    (2, 735, 9, 2026, 4496, (R,)),
    (2, 788, 9, 2026, 4475, (R,)),
    (2, 789, 9, 2026, 4551, (R,)),
    (2, 833, 9, 2026, 4434, (R,)),
    (2, 849, 9, 2026, 4640, (P,)),
    (2, 859, 9, 2026, 4820, (A,)),
    (2, 865, 9, 2026, 4694, (R,)),
    (2, 872, 9, 2026, 4744, (R,)),
    (2, 885, 9, 2026, 4702, (R,)),
    (2, 915, 9, 2026, 4759, (R,)),
    (2, 923, 9, 2026, 4642, (R,)),
    (2, 941, 9, 2026, 4436, (R,)),
    (2, 945, 9, 2026, 4608, (R,)),
    (2, 301, 10, 2026, 5308, (T,)),
    (2, 899, 10, 2026, 5220, (T,)),
    (2, 191, 11, 2026, 6074, (T,)),
    (2, 369, 11, 2026, 5970, (T,)),
    (2, 415, 11, 2026, 6054, (T,)),
    (2, 476, 11, 2026, 5855, (T,)),
    (2, 788, 11, 2026, 6139, (T,)),
    (2, 899, 11, 2026, 6004, (T,)),
    (2, 961, 11, 2026, 6018, (T,)),
    (3, 82, 9, 2026, 4856, (R,)),
    (3, 87, 9, 2026, 4861, (R,)),
    (3, 114, 9, 2026, 4892, (R,)),
    (3, 133, 9, 2026, 4915, (A,)),
    (3, 145, 9, 2026, 4926, (R,)),
    (3, 165, 9, 2026, 4946, (R,)),
    (3, 502, 9, 2026, 4887, (R,)),
    (3, 638, 9, 2026, 4839, (P, R)),
    (3, 749, 9, 2026, 4910, (P, R)),
    (3, 868, 9, 2026, 4905, (R,)),
    (3, 749, 11, 2026, 5630, (T,)),
    (3, 911, 11, 2026, 6041, (T,)),
    (4, 50, 9, 2026, 4964, (A, R)),
)


class EscritaProibida(BaseException):
    pass


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass


def bloquear(*args, **kwargs):
    raise EscritaProibida()


def so_select(execute, sql, params, many, context):
    s = str(sql).strip()
    if (
        many
        or context["connection"].alias != "default"
        or not re.match(r"^SELECT\b", s, re.I)
        or ";" in s
        or "--" in s
        or "/*" in s
        or re.search(
            r"\b(INSERT|UPDATE|DELETE|MERGE|INTO|CREATE|ALTER|DROP|"
            r"TRUNCATE|COPY|CALL|DO|SET|RESET|GRANT|REVOKE|"
            r"NEXTVAL|SETVAL|PG_ADVISORY_LOCK|PG_TRY_ADVISORY_LOCK|"
            r"PG_ADVISORY_XACT_LOCK|PG_TRY_ADVISORY_XACT_LOCK|"
            r"LO_IMPORT|LO_EXPORT|PG_TERMINATE_BACKEND|PG_CANCEL_BACKEND|"
            r"PG_RELOAD_CONF|SET_CONFIG)\b|\bFOR\s+(SHARE|KEY)\b",
            s, re.I,
        )
    ):
        raise EscritaProibida()
    return execute(sql, params, many, context)


def sonda():
    saida = sys.stdout
    erros = 0
    pars = []
    totais_dia = Counter()
    membros = defaultdict(set)
    totais_par = Counter()
    dias_par = Counter()
    linhas_dia = 0
    etapa = "preparo"
    contexto = {}
    nivel = logging.root.manager.disable
    bytecode = _BYTECODE_ANTERIOR

    def emitir(tipo, **dados):
        def numero(x):
            if isinstance(x, Decimal):
                return format(x, "f")
            raise TypeError()

        saida.write(tipo + "|" + json.dumps(
            dados, ensure_ascii=False, sort_keys=True,
            default=numero, allow_nan=False,
        ) + "\n")

    def erro(onde, exc, **chaves):
        nonlocal erros
        erros += 1
        # Sem mensagem da excecao, modelo, SQL ou traceback.
        emitir(
            "ERRO " + str(erros),
            etapa=onde,
            escrita_bloqueada=isinstance(exc, EscritaProibida),
            **chaves,
        )

    def exigir(condicao):
        if not condicao:
            raise ValueError()

    def minutos(valor, campo):
        if valor is None:
            return None
        n = Decimal(str(valor))
        exigir(n.is_finite())
        return n if campo.startswith("minutos_") else n * 60

    try:
        sys.dont_write_bytecode = True
        logging.disable(logging.CRITICAL)

        with ExitStack() as stack, localcontext() as ctx:
            ctx.prec = 50
            stack.enter_context(redirect_stdout(Descartar()))
            stack.enter_context(redirect_stderr(Descartar()))

            from django.db import connection, connections
            from django.db.models import Model
            from django.db.models.query import QuerySet

            exigir(connection.settings_dict["NAME"] == "sombra")
            exigir(connection.schema_name == "juliani")

            for alias in connections:
                stack.enter_context(
                    connections[alias].execute_wrapper(so_select)
                )

            for classe, metodos in (
                (Model, ("save", "delete")),
                (QuerySet, (
                    "create", "update", "delete", "_raw_delete",
                    "bulk_create", "bulk_update", "get_or_create",
                    "update_or_create",
                )),
            ):
                for metodo in metodos:
                    stack.enter_context(
                        patch.object(classe, metodo, bloquear)
                    )

            with connection.cursor() as cursor:
                cursor.execute(
                    "SELECT current_database(), current_schema()"
                )
                exigir(cursor.fetchone() == ("sombra", "juliani"))

            from freezegun import freeze_time
            from django.contrib.auth import get_user_model
            from django.utils import timezone
            from chamados.juizes import tranca_em_lote
            from colaboradores.models import Colaborador
            from ponto.models import FechamentoMensal
            from ponto.janelas import janela_fechamento
            from ponto.services.dia_pago import (
                CAMPOS_DIA, linhas_do_periodo, soma_da_competencia,
            )
            from ponto.services.espelho import espelho_do_colab
            from ponto.management.commands.e6_oraculo import InsumosDoDono

            stack.enter_context(freeze_time(AGORA, tick=False))
            stack.enter_context(
                timezone.override("America/Sao_Paulo")
            )
            hoje = timezone.localtime(AGORA).date()

            sistema_pk = (
                get_user_model().objects.filter(username="sistema")
                .values_list("pk", flat=True).first()
            )
            exigir(sistema_pk is not None)
            stack.enter_context(
                tranca_em_lote(sistema_pk=sistema_pk)
            )

            exigir(len(PARES) == len({p[:4] for p in PARES}) == 59)
            exigir(
                Counter(p[0] for p in PARES) == {2: 46, 3: 12, 4: 1}
            )
            exigir(sum(len(p[5]) for p in PARES) == 65)
            exigir(all(
                set(p[5]) <= set(CAMPOS_DIA) for p in PARES
            ))

            emitir(
                "META",
                unidade="minutos",
                gravado_diario=False,
                selecao="todos os dias de janela_fechamento",
                fonte_lavra="app/ponto/services/dia_pago.py:833",
                fonte_gravado="app/ponto/models.py:398",
                fonte_dono=(
                    "app/ponto/management/commands/e6_oraculo.py:182"
                ),
                execucao_nesta_resposta="[nao verificado]",
            )

            for eid, cid, mes, ano, fid, campos in PARES:
                contexto = dict(
                    empresa=f"emp{eid}",
                    colab=f"col{cid}",
                    comp=f"{mes:02d}/{ano}",
                )
                donos = Counter()
                dias = 0
                inicio_erros = erros
                etapa = "preparo_par"

                try:
                    c = (
                        Colaborador.objects
                        .select_related("empresa").get(pk=cid)
                    )
                    exigir(c.empresa_id == eid)

                    fm = FechamentoMensal.objects.filter(
                        colaborador_id=cid, mes=mes, ano=ano,
                    ).first()
                    exigir(fm is not None and fm.pk == fid)

                    ini, fim = janela_fechamento(
                        mes, ano, c.empresa
                    )
                    exigir(fim >= ini)
                    dias = (fim - ini).days + 1

                    # Mesmas chamadas da m2 colada [nao verificado].
                    # app/ponto/services/dia_pago.py:833 e :550.
                    ds = linhas_do_periodo(c, ini, fim)
                    soma_lavra = soma_da_competencia(c, mes, ano)

                    gravado_comp = {
                        k: minutos(getattr(fm, k), k)
                        for k in campos
                    }

                    emitir(
                        "COMP",
                        **contexto,
                        fechamento=f"#{fm.pk}",
                        ini=ini.isoformat(),
                        fim=fim.isoformat(),
                        campos_anexo=list(campos),
                        gravado_diario=False,
                        gravado_competencia=gravado_comp,
                        soma_lavra_competencia=(
                            None if soma_lavra is None else {
                                k: minutos(soma_lavra[k], k)
                                for k in campos
                            }
                        ),
                        campos_divergentes_agora=(
                            None if soma_lavra is None else [
                                k for k in CAMPOS_DIA
                                if getattr(fm, k) is not None
                                and round(float(getattr(fm, k)), 2)
                                    != soma_lavra[k]
                            ]
                        ),
                    )

                    # Mesmo preparo do command:
                    # app/ponto/management/commands/e6_oraculo.py:316/:328.
                    esp = espelho_do_colab(
                        c, hoje, ini, fim, agora=AGORA
                    )
                    ins = InsumosDoDono(
                        c, ini, fim, hoje,
                        espelho=esp, agora=AGORA,
                    )

                    for offset in range(dias):
                        dia = ini + dt.timedelta(days=offset)
                        etapa = "dia"

                        try:
                            dp = ds.get(dia)
                            if dp is not None:
                                exigir(
                                    (dp.mes, dp.ano) == (mes, ano)
                                )

                            valores = {
                                k: (
                                    None if dp is None
                                    else minutos(getattr(dp, k), k)
                                )
                                for k in campos
                            }

                            # Carregador E juiz real, sem reproduzir regras:
                            # e6_oraculo.py:182/:190/:195.
                            dono = ins.dono(dia)
                            exigir(dono in (
                                "ESTRUTURA", "CADASTRO", "BATIDA",
                            ))

                            registros = [
                                dict(
                                    **contexto,
                                    dia=dia.isoformat(),
                                    campo=k,
                                    lavra=valores[k],
                                    gravado=None,
                                    dono=dono,
                                    unidade="minutos",
                                    gravado_estado=(
                                        "sem valor diario no fechamento"
                                    ),
                                    gravado_competencia=gravado_comp[k],
                                    dia_pago=(
                                        None if dp is None
                                        else f"#{dp.pk}"
                                    ),
                                    lavra_estado=(
                                        "sem lavratura do dia"
                                        if dp is None else
                                        "sem valor no campo"
                                        if valores[k] is None else
                                        "lavrado"
                                    ),
                                    divergencia_diaria_comprovada=False,
                                )
                                for k in campos
                            ]

                            for registro in registros:
                                emitir("DIA", **registro)
                                chave = (
                                    eid, registro["campo"], dono,
                                )
                                totais_dia[chave] += 1
                                membros[chave].add((cid, mes, ano))
                                linhas_dia += 1

                            # Uma contagem por DIA, mesmo com dois campos.
                            donos[dono] += 1

                        except Exception as exc:
                            erro(
                                "dia", exc, **contexto,
                                dia=dia.isoformat(),
                            )

                except Exception as exc:
                    erro(etapa, exc, **contexto)

                completo = (
                    erros == inicio_erros
                    and sum(donos.values()) == dias > 0
                )
                par = dict(
                    **contexto,
                    campos=list(campos),
                    donos=dict(sorted(donos.items())),
                    dias=dias,
                    dias_julgados=sum(donos.values()),
                    completo=completo,
                )
                pars.append(par)
                emitir("PAR", **par)

                if completo:
                    perfil = (
                        eid,
                        tuple(sorted(campos)),
                        tuple(sorted(donos)),
                    )
                    totais_par[perfil] += 1
                    dias_par[perfil] += dias

            etapa, contexto = "conferencia", {}
            exigir(len(pars) == 59)

            if erros == 0:
                exigir(all(p["completo"] for p in pars))
                exigir(sum(totais_par.values()) == 59)
                exigir(
                    sum(dias_par.values())
                    == sum(p["dias"] for p in pars)
                )
                exigir(
                    sum(totais_dia.values()) == linhas_dia
                )
                exigir(
                    linhas_dia == sum(
                        p["dias"] * len(p["campos"])
                        for p in pars
                    )
                )

    except BaseException as exc:
        erro(etapa, exc, **contexto)

    finally:
        logging.disable(nivel)
        sys.dont_write_bytecode = bytecode

    for (eid, campo, dono), n in sorted(totais_dia.items()):
        emitir(
            "TOT",
            unidade="dia_campo",
            empresa=f"emp{eid}",
            campo=campo,
            dono=dono,
            dias=n,
            pares_incidentes=len(membros[(eid, campo, dono)]),
        )

    for (eid, campos, perfil), n in sorted(totais_par.items()):
        emitir(
            "TOT",
            unidade="par",
            empresa=f"emp{eid}",
            campo=list(campos),
            dono=list(perfil),
            pares=n,
            dias=dias_par[(eid, campos, perfil)],
        )

    saida.write(
        f"FIM ok={int(erros == 0)} erros={erros}"
        f" pares={len(pars)}"
        f" pares_completos={sum(totais_par.values())}"
        f" esperado=59 linhas_dia_campo={linhas_dia}\n"
    )

    if erros:
        raise SystemExit(1)


sonda()
```

## 15/11 01:35 · sessao d3bab7ce6ebf · BATIDA-DEDUP-RETRATADA · verificador · origem=Code

**REPROVADO nesta volta.** Há incoerências nos retornos que a cura passa a alcançar e uma guarda existente que não preserva a remoção do filtro cru (`app/ponto/registro_batida.py:120`; `app/ponto/services/triagem_batida.py:716`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`).

As referências abaixo usam a numeração do HEAD. O teste novo, seus RED/GREEN e mutantes permanecem **[nao verificado]**. [nao verificado]

1. **Campos, significados, ordem e ID**

   | Campo/valor | Quem escreve/define; quem lê; conclusão |
   |---|---|
   | `retratada_em` | `retratar_batida` fornece o instante **da retratação**, e `anotar` persiste; desfazer restaura `None` pela mesma porta. A autoridade e o dedup temporal da porta leem apenas NULL/não NULL. Não comparam esse carimbo com a janela do toque (`app/ponto/registro_batida.py:274`; `app/ponto/registro_batida.py:252`; `app/ponto/services/retratar_humano.py:207`; `app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:109`). |
   | `timestamp` | As APIs obtêm `ts_efetivo` da autoridade de hora; a triagem captura `agora`; `registrar_batida` grava o argumento recebido. Dedup, chave exata e respostas leem esse instante (`app/api/views.py:507`; `app/api/views_core.py:493`; `app/ponto/services/triagem_batida.py:626`; `app/ponto/registro_batida.py:158`; `app/api/views.py:540`). |
   | `tipo` | Entra pelo request, mas as APIs podem substituí-lo **depois do dedup**, antes da porta. O campo persistido também pode ser invertido por `flip_tipo`, via `anotar`. Portanto, dedup da casca e porta podem perguntar por tipos diferentes, mesmo após c1 (`app/api/views.py:420`; `app/api/views.py:529`; `app/api/views.py:578`; `app/api/views.py:601`; `app/api/views.py:655`; `app/ponto/services/flip_batida.py:30`). |
   | Janela | APIs: ±5 minutos do fato; triagem: `[agora−5min, agora]`; porta, mesmo tipo: ±60 segundos. Todos usam limites **inclusivos**, `gte/lte`; continuam sendo perguntas temporalmente diferentes (`app/api/views.py:527`; `app/api/views_core.py:513`; `app/ponto/services/triagem_batida.py:663`; `app/ponto/registro_batida.py:110`; `app/ponto/turnos.py:2878`). |
   | Fuso/dia | A autoridade não converte os limites em dia nem consulta relógio. A entrada de hora torna timestamps aware; a configuração declara São Paulo e `USE_TZ=True`; apresentação usa `localtime`. Nesse caminho, não encontrei troca de instante por dia causada pela substituição (`app/ponto/turnos.py:2875`; `app/api/hora_do_aparelho.py:116`; `app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/api/views.py:535`). |

   **A ordem efetiva da triagem não muda entre candidatas vivas.** Antes, `Meta.ordering=["colaborador","timestamp"]`; depois, a autoridade ordena por `timestamp`. Com colaborador fixo, ambos escolhem o menor instante; a unicidade colaborador/timestamp elimina empate. Antes sai o ID da primeira candidata, inclusive retratada; depois sai o ID da primeira **viva**, ou a execução continua se nenhuma existir (`app/ponto/models.py:105`; `app/ponto/models.py:106`; `app/ponto/services/triagem_batida.py:664`; `app/ponto/services/triagem_batida.py:671`; `app/ponto/turnos.py:2883`).

   Nas APIs continua saindo a candidata de **maior timestamp**, agora dentro do universo vivo proposto; `id`, horário e sequência vêm dessa linha (`app/api/views.py:532`; `app/api/views.py:538`; `app/api/views.py:541`; `app/api/views_core.py:518`; `app/api/views_core.py:524`).

   Quanto ao consumidor: o PWA online presente ignora o corpo e considera `r.ok` sucesso; o sincronizador apaga a pendência em todo `resp.ok`, sem usar o ID retornado. O upload servidor recebe `batida_id`, mas o cliente APK que ligaria esse campo ao retorno está **[nao verificado]** (`app/templates/ponto/bater_ponto_v2.html:327`; `app/static/pwa.js:158`; `app/static/pwa.js:162`; `app/api/views.py:836`; `app/api/views.py:842`).

2. **O que acontece depois que a batida chega à porta**

   | Caso | Resultado da porta |
   |---|---|
   | Retratada dentro de 60 s, **instante diferente** | A retratada não satisfaz o dedup. Sem outra viva, chave exata ou colisão impeditiva, cria uma nova viva. Não duplica a chave de banco (`app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:120`; `app/ponto/registro_batida.py:157`; `app/ponto/models.py:106`). |
   | Chave exata colaborador/timestamp | Devolve a anterior, **inclusive retratada ou de outro tipo**. Não cria nem desfaz a retratação; portanto, pode continuar sem nova batida apurável (`app/ponto/registro_batida.py:116`; `app/ponto/registro_batida.py:120`; `app/ponto/registro_batida.py:122`; `app/ponto/turnos.py:2876`). |
   | Oposta viva na janela | Usa ±300 s com escala/marcos ou ±60 s sem eles. Salvo correspondência ao marco próprio, retorna `None, conflito` para o automático: **barra a materialização**. As três cascas passam `autoridade_humana=False` (`app/ponto/registro_batida.py:21`; `app/ponto/registro_batida.py:145`; `app/ponto/registro_batida.py:151`; `app/api/views.py:656`; `app/api/views_core.py:647`; `app/ponto/services/triagem_batida.py:704`). |

   **A resposta posterior não é coerente em todos os casos.** A porta não distingue criação de reaproveitamento no retorno. Após chave exata, as APIs anunciam `status=ok`, usando `tipo` e rótulo da variável local, mas timestamp/sequência do objeto devolvido. Se ele for de outro tipo, o payload pode contradizer o registro; se retratado, não informa que continua fora da apuração. A sequência retornada é a antiga, não a recém-calculada (`app/ponto/registro_batida.py:102`; `app/ponto/registro_batida.py:122`; `app/api/views.py:555`; `app/api/views.py:799`; `app/api/views.py:812`; `app/api/views.py:814`; `app/api/views_core.py:806`).

   **Há outro erro concreto na triagem:** após a recusa, ela procura oposta em ±60 s **sem excluir retratadas**. A porta pode ter recusado por uma viva a três minutos; a resposta então pode trazer `id=None`, ou apontar uma retratada próxima que não causou a recusa. As APIs usam a janela da porta e excluem retratadas nesse retorno (`app/ponto/services/triagem_batida.py:716`; `app/ponto/services/triagem_batida.py:723`; `app/ponto/registro_batida.py:31`; `app/ponto/registro_batida.py:141`; `app/api/views.py:672`; `app/api/views_core.py:663`).

   Esses ramos já existem no HEAD; retirar o retorno antecipado por retratada permite que mais chamadas os alcancem. A troca de consulta não garante, sozinha, resposta correta depois da porta (`app/ponto/services/triagem_batida.py:670`; `app/ponto/services/triagem_batida.py:702`; `app/ponto/services/triagem_batida.py:715`; `app/api/views.py:533`; `app/api/views.py:654`).

3. **Pendente de DP, origem e canal**

   `aprovada=False`, origem e canal, isoladamente, **não explicam divergência entre esses dedups**: as consultas antigas não os filtram; a autoridade também não; a porta temporal também não. Pendente de DP continua contando. `aprovada` é crivo do DP, enquanto `retratada_em` determina exclusão da apuração (`app/api/views.py:529`; `app/api/views_core.py:515`; `app/ponto/services/triagem_batida.py:664`; `app/ponto/turnos.py:2859`; `app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:107`; `app/ponto/models.py:63`).

   Permanecem as diferenças de janela, a possível correção de tipo entre casca e porta e a exceção da chave exata, que aceita até retratada. Não atribuo essas diferenças a um estado de aprovação/canal (`app/api/views.py:527`; `app/api/views.py:578`; `app/ponto/registro_batida.py:106`; `app/ponto/registro_batida.py:120`).

4. **Censo independente e classes estruturais**

   O censo encontrou **três dedups de chão** antes dos três chamadores de origem `app`: API original, API core e triagem. Coincide com os três sítios do T3; quarto sítio estático não encontrado. Caminhos dinâmicos não identificados permanecem **[nao verificado]** (`app/api/views.py:529`; `app/api/views.py:654`; `app/api/views_core.py:515`; `app/api/views_core.py:645`; `app/ponto/services/triagem_batida.py:664`; `app/ponto/services/triagem_batida.py:702`).

   Fora desse conjunto há reaproveitamento de materialização por ±20 minutos, guardas de vizinhas e chave exata de regularização externa. São controles existentes, mas não outro toque de chão (`app/chamados/juizes.py:235`; `app/chamados/juizes.py:243`; `app/chamados/services/materializacao.py:95`; `app/chamados/services/materializacao.py:101`; `app/chamados/services/regularizacao_ext.py:74`).

   A substituição usa uma autoridade existente; ela não acrescenta escritor, relógio ou trava. A criação encontrada permanece na porta, inclusive o bulk de ensaio (`app/ponto/turnos.py:2856`; `app/ponto/turnos.py:2875`; `app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:157`; `app/ponto/registro_batida.py:335`).

   **Derivação paralela está presente no retorno da triagem:** ela tenta reconstruir a causa da recusa com universo e janela diferentes. Também permanece a divergência entre instante do fato e recepção no envelope `ultima_batida.timestamp`, escrito com `agora`; isso pertence ao item T4 distinto, não foi corrigido por c1 (`app/ponto/services/triagem_batida.py:716`; `app/ponto/registro_batida.py:139`; `app/api/views.py:761`; `app/api/views_core.py:755`; `app/chamados/juizes.py:151`).

   Um selo restrito aos `SITIOS` enumerados deixaria um quarto sítio fora da lista sem cobertura, conforme a limitação relatada; implementação e mordida do teste novo **[nao verificado]**. Suite inteira, opus, smoke e eventual pouso antes desses pré-requisitos **[nao verificado]**. [nao verificado]

5. **Import local e uso de `Batida`**

   Não encontrei ciclo no trajeto estático: o topo de `turnos` importa bibliotecas e `core.dia_do_turno`; este último só constrói o tipo com `datetime/typing`. `batidas_apuraveis` apenas monta a consulta. Execução real do novo import **[nao verificado]** (`app/ponto/turnos.py:36`; `app/ponto/turnos.py:41`; `app/core/dia_do_turno.py:10`; `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2875`).

   **`Batida` continua usado nos três módulos**, particularmente nas buscas de oposta após a porta; remover seu import seria errado (`app/api/views.py:674`; `app/api/views_core.py:665`; `app/ponto/services/triagem_batida.py:716`).

6. **Selos existentes: distinguir duas situações**

   **B5j não perde copiadores com esta cura.** Seu detector considera funções que alimentam o pareador, não qualquer dedup. A contagem em memória permanece **6 → 6**; os seis estão em `turnos` duas vezes, lastro, piso, meio período e fila por causa. O teto oito já deixa duas posições de folga no HEAD; essa folga não nasceu em c1 (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:123`; `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:191`; `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:217`; `app/ponto/turnos.py:1863`; `app/ponto/turnos.py:1901`; `app/ponto/services/lastro.py:261`; `app/ponto/services/piso_intervalo.py:39`; `app/escala/services/meio_periodo.py:158`; `app/chamados/services/fila_por_causa.py:190`).

   **O tripwire de bocais perde uma ocorrência e deveria guardar essa remoção.** No HEAD, sua regex encontra três filtros em `api/views`; com a troca, ficam dois. O teto continua sete, e a asserção usa `<=`: restaurar a consulta removida passa nesse selo. `views_core` e triagem sequer estão nessa allowlist (`app/api/views.py:296`; `app/api/views.py:529`; `app/api/views.py:674`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:13`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:20`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:25`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`).

**Veredito final: REPROVADO**, pelos retornos inconsistentes detalhados no item 2 e pela guarda incompleta do item 6. A ordem da triagem e a contagem de pendentes de DP não são defeitos da substituição (`app/ponto/models.py:105`; `app/ponto/turnos.py:2859`; `app/ponto/turnos.py:2883`).

## 15/11 01:32 · sessao 9e8e05c97134 · - · - · origem=Ronald (tmux)

1. **Não há mudança na escolha entre batidas vivas da triagem.** Antes, `Meta.ordering=["colaborador","timestamp"]`; depois, `order_by("timestamp")`. Como a query restringe um colaborador e existe unicidade `(colaborador,timestamp)`, ambas devolvem o menor instante elegível; a diferença é remover retratadas (`app/ponto/models.py:105`, `app/ponto/models.py:106`, `app/ponto/services/triagem_batida.py:664`, `app/ponto/turnos.py:2883`). A afirmação de equivalência da entrega está correta para esse cenário; confirmação SQL executada [nao verificado].

2. **B5j não cai com esta cura.** Executei apenas suas funções AST em memória, simulando as três substituições em strings: copiadores `6 → 6`; os três módulos tocados contribuem `0 → 0`. O detector considera funções que chamam `parear_turnos`/`_turnos_do_colab_puro`, não qualquer função do módulo (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:123`). Os seis copiadores atuais são:
   - `app/ponto/turnos.py:1863`;
   - `app/ponto/turnos.py:1901`;
   - `app/ponto/services/lastro.py:261`;
   - `app/ponto/services/piso_intervalo.py:39`;
   - `app/escala/services/meio_periodo.py:158`;
   - `app/chamados/services/fila_por_causa.py:190`.

3. **B5j já tem folga anterior: teto 8 para seis atuais.** O comentário promete catraca fundada no número contado, mas a implementação exige somente `total <= 8`, permitindo dois copiadores adicionais (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:184`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:191`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:215`). Não atribuir essa folga à cura: ela já existe no HEAD; a lei apenas declara “8 copiadores ... não cresce” (`CLAUDE.md:163`).

4. **O selo de bocais deveria apertar sua contagem para guardar a remoção.** A regex encontra três filtros em `api/views.py`, nas linhas `296`, `529`, `674`; a substituição proposta leva a dois. Entretanto, o teto permanece sete e a asserção é somente `<=`, aumentando a folga de quatro para cinco (`app/ponto/tests/test_tripwire_bocais_apuracao.py:20`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:25`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`). Restaurar o filtro removido continuará passando nesse selo (`app/api/views.py:529`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`). `views_core` e `triagem_batida` não constam da allowlist (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`).

5. **Sem troca de instante por dia no predicado novo.** A autoridade aplica `timestamp__gte` e `timestamp__lte`, inclusive nas duas pontas; filtra somente retratação, contando pendentes de DP e sem restrição por origem/canal (`app/ponto/turnos.py:2859`, `app/ponto/turnos.py:2876`). Ela não lê relógio, não escreve e não trava (`app/ponto/turnos.py:2875`). As APIs fornecem `ts_efetivo` da autoridade de hora; a triagem usa seu `agora` capturado (`app/api/views.py:497`, `app/api/views.py:507`, `app/ponto/services/triagem_batida.py:626`).

6. **Retratação é carimbo do ato, não o instante da batida.** `retratar_batida` anota `aprovada=False` e `retratada_em=timezone.now()`; o gravador material é `anotar`/`save(update_fields=...)` (`app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:252`). O modelo declara `aprovada` como crivo do DP e retratação como exclusão da geometria/apuração (`app/ponto/models.py:63`, `app/ponto/models.py:64`).

7. **Fuso consistente para o caminho inspecionado.** A configuração declara `America/Sao_Paulo` e `USE_TZ=True`; a regra documental exige UTC no banco e apresentação UTC−3 (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `CLAUDE.md:146`). A autoridade da hora torna timestamps ingênuos aware; aceita o aparelho com atraso inclusivo de zero a 48 horas e retorna `agora` fora disso (`app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`). Persistência/fuso efetivos de produção [nao verificado].

## 15/11 01:32 · sessao cfe31aeba724 · - · - · origem=Ronald (tmux)

1. **Retratada dentro de 60s:** a porta exclui a anterior no filtro (`app/ponto/registro_batida.py:107-113`). Sem outra viva do mesmo tipo, chave exata ou oposta impeditiva, cria linha viva (`app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:145`, `app/ponto/registro_batida.py:157`). Não grava duplicata da chave; o banco exige unicidade por colaborador+timestamp (`app/ponto/models.py:106`).

2. **Chave exata:** devolve a linha anterior, inclusive retratada ou de outro tipo, sem revivê-la e sem criar nova (`app/ponto/registro_batida.py:116-122`). A cura portanto não garante nova batida apurável nesse caso (`app/ponto/turnos.py:2876`).

3. **Oposta na janela:** a janela é ±300s com escala/marcos e ±60s sem eles (`app/ponto/registro_batida.py:21-32`). Considera só opostas vivas e recusa o automático com `(None, conflito)`, salvo correspondência ao marco próprio (`app/ponto/registro_batida.py:139-151`). A tolerância do marco próprio é inclusiva, `<=5min` (`app/ponto/registro_batida.py:41`, `app/ponto/registro_batida.py:48`). As três cascas passam `autoridade_humana=False` (`app/api/views.py:656`, `app/api/views_core.py:647`, `app/ponto/services/triagem_batida.py:704`). Esse caminho continua barrando materialização (`app/ponto/registro_batida.py:151`).

4. **Resposta rica após retorno existente:** a porta não distingue “criada” de “reaproveitada” no contrato de retorno (`app/ponto/registro_batida.py:102`, `app/ponto/registro_batida.py:115`, `app/ponto/registro_batida.py:122`, `app/ponto/registro_batida.py:161`). As APIs devolvem `status=ok`, tipo e rótulo calculados da variável `tipo`, mas timestamp e sequência da linha retornada (`app/api/views.py:799-815`, `app/api/views_core.py:793-809`). Se a chave exata devolver retratada/outro tipo, o payload não informa a retratação e pode anunciar um tipo diferente do objeto (`app/ponto/registro_batida.py:116-122`, `app/api/views.py:812-815`). A sequência devolvida é a gravada na linha antiga, não `sequencia_jornada` recém-calculada (`app/api/views.py:555`, `app/api/views.py:659`, `app/api/views.py:814`). A linha reutilizada também recebe os efeitos posteriores, como foto e geofence (`app/api/views.py:769-781`).

5. **Triagem possui divergência adicional na resposta de oposta:** após `None`, procura oposta crua em ±60s, sem excluir retratada (`app/ponto/services/triagem_batida.py:715-725`). A porta pode ter recusado por uma viva a 3min, pois usa até ±300s (`app/ponto/registro_batida.py:21-32`, `app/ponto/registro_batida.py:139-151`). Logo a resposta pode trazer `id=None`, ou apontar uma retratada que não causou a recusa (`app/ponto/services/triagem_batida.py:716-725`). As APIs consultam a janela da porta e excluem retratada (`app/api/views.py:672-687`, `app/api/views_core.py:663-678`).

6. **Pendente DP/origem/canal:** as três queries antigas não filtram esses estados (`app/api/views.py:529-532`, `app/api/views_core.py:515-518`, `app/ponto/services/triagem_batida.py:664-669`). `batidas_apuraveis` também não os filtra; exclui somente retratada (`app/ponto/turnos.py:2876-2883`). A porta usa o mesmo critério no dedup e na colisão, e nem esse filtro na chave exata (`app/ponto/registro_batida.py:107-120`, `app/ponto/registro_batida.py:139-144`). Portanto `aprovada=False`, origem ou canal, isoladamente, não são divergências entre essas consultas (`app/ponto/turnos.py:2876-2883`, `app/ponto/registro_batida.py:107-120`).

7. **Divergências remanescentes:** API usa ±5min, triagem `[agora−5min,agora]`, porta ±60s para mesmo tipo (`app/api/views.py:527-532`, `app/ponto/services/triagem_batida.py:663-669`, `app/ponto/registro_batida.py:106-113`). Na API, dedup usa o tipo solicitado antes da correção pela autoridade; a porta recebe o tipo corrigido (`app/api/views.py:529-532`, `app/api/views.py:570-601`, `app/api/views.py:654-655`). Essas diferenças já existem no HEAD; a cura informada não as altera [nao verificado].

## 15/11 01:32 · sessao 747fd334a147 · - · - · origem=Ronald (tmux)

Censo independente por busca textual e AST dos `.py` da cópia, excluindo testes/migrations: encontrei os mesmos **três sítios de dedup de chão**, antes dos únicos três chamadores com origem `'app'`: `app/api/views.py:529` → `:654`; `app/api/views_core.py:515` → `:645`; `app/ponto/services/triagem_batida.py:664` → `:702`. Quarto sítio estático de chão não encontrado; cobertura de caminhos dinâmicos [nao verificado].

Outros controles antes de criar/reaproveitar batida, sem serem toque de chão:

- Materialização: `_batida_existente_na_janela`, mesmo tipo ±20 min, primeiro timestamp, sem filtro de retratação (`app/chamados/juizes.py:235`, `:243`); guarda de vizinha imediata anterior/posterior até 12h, também crua (`app/chamados/services/materializacao.py:86`, `:95`, `:101`).
- Regularização externa: chave exata colaborador+timestamp, pula antes da porta (`app/chamados/services/regularizacao_ext.py:71`, `:74`, `:78`).
- Veredito humano: carga crua, simulação e criação somente das novas propostas pela porta (`app/ponto/services/veredito_celula.py:237`, `:323`).
- Demais chamadores são autorização humana/justificativa: `app/chamados/services/acoes_chamado.py:44`; `app/ponto/views.py:1818`; `app/ponto/services/justificativa.py:188`, `:361`; materialização com `fato_de_chao=False`: `app/chamados/services/materializacao.py:933`, `:1013`. O comando de ensaio também chama a porta (`app/ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229`).

O AST encontrou criação de Batida apenas dentro da porta: `app/ponto/registro_batida.py:152`, `:157`, `:335`. Escritas via reflexão/SQL dinâmico [nao verificado].

**ID e clientes:** APIs devolvem `dupla.pk` em `ja_registrada` (`app/api/views.py:538`; `app/api/views_core.py:524`), triagem devolve `dupla.id` (`app/ponto/services/triagem_batida.py:671`). No cliente v2 presente, todo HTTP 2xx vira sucesso sem ler corpo/ID (`app/templates/ponto/bater_ponto_v2.html:327`); sincronizador PWA lê JSON e descarta a fila em todo `resp.ok`, ignorando status/ID (`app/static/pwa.js:158`, `:162`). Leitor Android/APK desse ID [nao verificado]. Há consumidor servidor de `batida_id` no upload separado de foto (`app/api/views.py:836`, `:842`; `app/api/views_core.py:820`, `:826`), mas vincular esse consumidor ao ID retornado exige o cliente ausente [nao verificado]. O cliente web antigo envia formulário normal (`app/templates/ponto/bater_ponto.html:237`), e a view repassa a resposta JSON de triagem (`app/ponto/views.py:353`, `:356`).

**Ordem:** triagem no HEAD usa `Meta.ordering = ["colaborador", "timestamp"]` (`app/ponto/models.py:105`) com colaborador fixo (`app/ponto/services/triagem_batida.py:665`), portanto escolhe o menor timestamp. A autoridade também usa timestamp ascendente (`app/ponto/turnos.py:2883`): a troca proposta não muda a escolha entre vivas; elimina candidatas retratadas [nao verificado: diff proposto]. APIs mantêm a maior timestamp (`app/api/views.py:532`; `app/api/views_core.py:518`). A chave única impede empate de timestamp no mesmo colaborador (`app/ponto/models.py:106`).

**Import/uso:** trajeto de topo de `turnos.py` é copy/datetime/namedtuple/timezone/`core.dia_do_turno` (`app/ponto/turnos.py:36`–`:43`); `core.dia_do_turno` só importa datetime/typing e constrói NewType (`app/core/dia_do_turno.py:10`–`:13`). Não há import de retorno para as views/triagem nesse trajeto; execução real do novo import [nao verificado]. `Batida` continua necessário nas buscas de oposta posteriores à porta (`app/api/views.py:674`; `app/api/views_core.py:665`; `app/ponto/services/triagem_batida.py:716`) e na busca de entrada da triagem (`app/ponto/services/triagem_batida.py:257`).

## 15/11 01:28 · sessao d954e76d3e69 · BATIDA-DEDUP-RETRATADA · construtor · origem=Code

Cura nas três cascas e censo registrados na [entrega](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c1_entrega.txt:6).

RED anterior, 9 testes GREEN sem banco e 3 mutantes mortos, com comandos e saídas guardados na [evidência](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c1_entrega.txt:33). [nao verificado]

Certificação por Code/suite, opus e smoke: pendente [não verificado]. [nao verificado]

## 15/11 01:26 · sessao 4793ac876070 · SILENCIO-VENCIDO · censo · origem=Code

**Dono: não sei para cada um dos 59 pares e para a diferença FIM.** O anexo informa divergências mensais, mas não os dias responsáveis nem os insumos diários necessários ao juiz. Isso não permite afirmar que ESTRUTURA, CADASTRO ou BATIDA tenham contagem zero. [nao verificado — anexo colado]

O juiz encontrado na cópia está em `app/ponto/management/commands/e6_oraculo.py`. Sua entrada é diária; o campo mensal divergente não é argumento da função. [app/ponto/management/commands/e6_oraculo.py:53](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:53)

| Ordem | Predicado que decide | Dono retornado | Fonte |
|---|---|---|---|
| 1 | `folga_nao_definida or sem_vinculo_no_dia or sem_vinculo_em_dia_de_trabalho` | CADASTRO | [e6_oraculo.py:98](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:98) |
| 2 | `batida_fora_de_turno` | BATIDA | [e6_oraculo.py:104](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:104) |
| 3 | `len(horas_do_dia or ()) % 2` | BATIDA | [e6_oraculo.py:106](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:106) |
| 4 | `fase_deslocada` | CADASTRO | [e6_oraculo.py:108](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:108) |
| 5 | `em_cadastro_x_realidade` | CADASTRO se verdadeiro; ESTRUTURA se falso | [e6_oraculo.py:110](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:110) |

O carregador existente reúne esses insumos e chama o juiz para uma `data`; não fornece uma redução de vários dias para um único dono mensal. [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:182)

Para compactar as tabelas: **R**=`minutos_realizados`, **A**=`minutos_abonados`, **P**=`minutos_previstos`, **T**=`horas_trabalhadas`. Esses campos pertencem a `CAMPOS_DIA`. [app/ponto/services/dia_pago.py:34](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/services/dia_pago.py:34)

Cada comparação abaixo está em **minutos, na ordem lavra × fechamento**, exatamente no escopo de `campos_divergentes` do anexo; Q1–Q4, adiante, são a coleta faltante comum aos 59 casos. [nao verificado — anexo colado]

| Empresa | Colab | Comp. | Fechamento | Campo: lavra × fechamento | Dono | Evidência |
|---|---|---|---|---|---|---|
| emp2 | col174 | 09/2026 | #4825 | R: 7355 × 7546 | não sei | [nao verificado] |
| emp2 | col191 | 09/2026 | #4518 | R: 8817 × 8489 | não sei | [nao verificado] |
| emp2 | col193 | 09/2026 | #4801 | R: 9825 × 5074 | não sei | [nao verificado] |
| emp2 | col206 | 09/2026 | #4418 | R: 7839 × 7479 | não sei | [nao verificado] |
| emp2 | col211 | 09/2026 | #4648 | R: 9000 × 8940 | não sei | [nao verificado] |
| emp2 | col214 | 09/2026 | #4668 | A: 6500 × 0 | não sei | [nao verificado] |
| emp2 | col231 | 09/2026 | #4699 | R: 9550 × 4996 | não sei | [nao verificado] |
| emp2 | col242 | 09/2026 | #4503 | T: 8869,20 × 8868,60; R: 5027 × 4797 | não sei | [nao verificado] |
| emp2 | col252 | 09/2026 | #4796 | R: 10260 × 9797 | não sei | [nao verificado] |
| emp2 | col269 | 09/2026 | #4569 | R: 9223 × 9226 | não sei | [nao verificado] |
| emp2 | col272 | 09/2026 | #4828 | A: 1980 × 0 | não sei | [nao verificado] |
| emp2 | col278 | 09/2026 | #4750 | T: 10659 × 10658,40; R: 4984 × 4939 | não sei | [nao verificado] |
| emp2 | col292 | 09/2026 | #4671 | A: 5280 × 4800; R: 5538 × 5964 | não sei | [nao verificado] |
| emp2 | col331 | 09/2026 | #4737 | R: 7562 × 6882 | não sei | [nao verificado] |
| emp2 | col349 | 09/2026 | #4666 | R: 9259 × 9178 | não sei | [nao verificado] |
| emp2 | col355 | 09/2026 | #4782 | A: 3960 × 3300 | não sei | [nao verificado] |
| emp2 | col371 | 09/2026 | #4564 | R: 6929 × 7169 | não sei | [nao verificado] |
| emp2 | col444 | 09/2026 | #4571 | R: 9877 × 9590 | não sei | [nao verificado] |
| emp2 | col489 | 09/2026 | #4552 | R: 8999 × 8759 | não sei | [nao verificado] |
| emp2 | col529 | 09/2026 | #4698 | R: 10989 × 10964 | não sei | [nao verificado] |
| emp2 | col532 | 09/2026 | #4508 | R: 10556 × 10376 | não sei | [nao verificado] |
| emp2 | col592 | 09/2026 | #4512 | R: 10205 × 10316 | não sei | [nao verificado] |
| emp2 | col616 | 09/2026 | #4703 | R: 9225 × 8042 | não sei | [nao verificado] |
| emp2 | col634 | 09/2026 | #4442 | R: 9373 × 9321 | não sei | [nao verificado] |
| emp2 | col735 | 09/2026 | #4496 | R: 6348 × 6226 | não sei | [nao verificado] |
| emp2 | col788 | 09/2026 | #4475 | R: 3092 × 2404 | não sei | [nao verificado] |
| emp2 | col789 | 09/2026 | #4551 | R: 10807 × 11187 | não sei | [nao verificado] |
| emp2 | col833 | 09/2026 | #4434 | R: 7162 × 7403 | não sei | [nao verificado] |
| emp2 | col849 | 09/2026 | #4640 | P: 11340 × 12360 | não sei | [nao verificado] |
| emp2 | col859 | 09/2026 | #4820 | A: 0 × 420 | não sei | [nao verificado] |
| emp2 | col865 | 09/2026 | #4694 | R: 8644 × 8914 | não sei | [nao verificado] |
| emp2 | col872 | 09/2026 | #4744 | R: 9759 × 9663 | não sei | [nao verificado] |
| emp2 | col885 | 09/2026 | #4702 | R: 9594 × 8987 | não sei | [nao verificado] |
| emp2 | col915 | 09/2026 | #4759 | R: 8548 × 8499 | não sei | [nao verificado] |
| emp2 | col923 | 09/2026 | #4642 | R: 5803 × 2414 | não sei | [nao verificado] |
| emp2 | col941 | 09/2026 | #4436 | R: 2310 × 1998 | não sei | [nao verificado] |
| emp2 | col945 | 09/2026 | #4608 | R: 1437 × 1766 | não sei | [nao verificado] |
| emp2 | col301 | 10/2026 | #5308 | T: 10326 × 10325,40 | não sei | [nao verificado] |
| emp2 | col899 | 10/2026 | #5220 | T: 10192,80 × 10192,20 | não sei | [nao verificado] |
| emp2 | col191 | 11/2026 | #6074 | T: 8223 × 8223,60 | não sei | [nao verificado] |
| emp2 | col369 | 11/2026 | #5970 | T: 8800,20 × 8800,80 | não sei | [nao verificado] |
| emp2 | col415 | 11/2026 | #6054 | T: 8303,40 × 8304 | não sei | [nao verificado] |
| emp2 | col476 | 11/2026 | #5855 | T: 8959,20 × 8959,80 | não sei | [nao verificado] |
| emp2 | col788 | 11/2026 | #6139 | T: 6606 × 6606,60 | não sei | [nao verificado] |
| emp2 | col899 | 11/2026 | #6004 | T: 7587,60 × 7588,20 | não sei | [nao verificado] |
| emp2 | col961 | 11/2026 | #6018 | T: 8848,80 × 8849,40 | não sei | [nao verificado] |
| emp3 | col82 | 09/2026 | #4856 | R: 10560 × 10517 | não sei | [nao verificado] |
| emp3 | col87 | 09/2026 | #4861 | R: 8858 × 8227 | não sei | [nao verificado] |
| emp3 | col114 | 09/2026 | #4892 | R: 3051 × 2694 | não sei | [nao verificado] |
| emp3 | col133 | 09/2026 | #4915 | A: 3840 × 0 | não sei | [nao verificado] |
| emp3 | col145 | 09/2026 | #4926 | R: 1320 × 1267 | não sei | [nao verificado] |
| emp3 | col165 | 09/2026 | #4946 | R: 2265 × 2597 | não sei | [nao verificado] |
| emp3 | col502 | 09/2026 | #4887 | R: 7883 × 7409 | não sei | [nao verificado] |
| emp3 | col638 | 09/2026 | #4839 | P: 10690 × 12040; R: 9632 × 9773 | não sei | [nao verificado] |
| emp3 | col749 | 09/2026 | #4910 | P: 11440 × 11880; R: 9324 × 9702 | não sei | [nao verificado] |
| emp3 | col868 | 09/2026 | #4905 | R: 8645 × 8389 | não sei | [nao verificado] |
| emp3 | col749 | 11/2026 | #5630 | T: 7419,60 × 7420,20 | não sei | [nao verificado] |
| emp3 | col911 | 11/2026 | #6041 | T: 8576,40 × 8577 | não sei | [nao verificado] |
| emp4 | col50 | 09/2026 | #4964 | A: 660 × 1320; R: 7799 × 7724 | não sei | [nao verificado] |

O agrupamento **por conjunto exato de campos** conta cada linha uma vez e soma 59. [nao verificado — contagem do anexo colado]

| Campos divergentes | emp2 | emp3 | emp4 | Total | Evidência |
|---|---:|---:|---:|---:|---|
| somente R | 29 | 7 | 0 | 36 | [nao verificado] |
| somente A | 4 | 1 | 0 | 5 | [nao verificado] |
| somente P | 1 | 0 | 0 | 1 | [nao verificado] |
| somente T | 9 | 2 | 0 | 11 | [nao verificado] |
| T + R | 2 | 0 | 0 | 2 | [nao verificado] |
| A + R | 1 | 0 | 1 | 2 | [nao verificado] |
| P + R | 0 | 2 | 0 | 2 | [nao verificado] |
| **Total de linhas; dono não sei** | **46** | **12** | **1** | **59** | [nao verificado] |

Contando cada campo individualmente, são R=42, A=7, P=3 e T=13: **65 ocorrências**, porque seis linhas têm dois campos. [nao verificado — contagem do anexo colado]

DSR não integra esse predicado de erro: `CAMPOS_DIA` contém os campos comparados, enquanto `horas_reflexo_dsr` pertence a `CAMPOS_AJUSTE`. A soma da competência também arredonda cada agregado a duas casas antes de devolvê-lo. [app/ponto/services/dia_pago.py:34](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/services/dia_pago.py:34), [app/ponto/services/dia_pago.py:563](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/services/dia_pago.py:563)

**FIM: diferença de 19 dias; dono não sei.** O campo confrontado é a cardinalidade de `universo`/`total.dias`/`soma_partes.dias` contra `esperado`, não uma rubrica de lavra contra fechamento. [nao verificado — FIM colado]

| Conferência | Resultado | Evidência |
|---|---:|---|
| Soma das partes | 12+5+546+599+776+87+124+151+6+8+9 = 2323 | [nao verificado] |
| Estados | 2307 lavrados + 16 sem lavratura = 2323 | [nao verificado] |
| Universo contra esperado | 2323−2304 = **19** | [nao verificado] |
| Universo contra TSV | 2303 coincidentes + 20 fora = 2323 | [nao verificado] |
| TSV contra universo | 2303 coincidentes + 78 fora = 2381 | [nao verificado] |

Os 20 fora do TSV não podem ser chamados de “19 excedentes do esperado”: faltam as chaves dos conjuntos e a origem do esperado 2304. Também não há vínculo demonstrado entre esses 19 e os 16 dias sem lavratura. [nao verificado — anexo colado]

**Dado faltante e SELECTs de coleta.** Para os 59, faltam: identificação dos dias implicados na diferença mensal; horas atribuídas pelo oráculo; resposta de `batida_fora_de_turno`; flags de cadastro; fase lavrada; publicação CXR com cobertura. Essas são as fontes usadas por `InsumosDoDono`; valores mensais de R/A/P/T não as substituem. [app/ponto/management/commands/e6_oraculo.py:128](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:128), [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:182)

Os SELECTs abaixo são propostas **não executadas**; tabelas físicas e resultados não foram conferidos em banco. `fechamentos` e `colabs` são arrays dos identificadores da tabela acima, sem os prefixos; `ini/fim` devem vir de `janela_fechamento`, respeitando o corte da empresa. [nao verificado], [app/ponto/janelas.py:54](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/janelas.py:54)

**Q1 — linhas diárias, valores e carimbos.** A consulta recuperaria a decomposição atual da lavra; o fechamento mostrado pelo modelo guarda esses valores como totais mensais. [app/ponto/models.py:315](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/models.py:315), [app/ponto/models.py:398](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/models.py:398)

```sql
SELECT 'emp' || c.empresa_id AS empresa,
       'col' || f.colaborador_id AS colab,
       '#' || f.id AS fechamento,
       f.mes, f.ano, f.derivado_em,
       '#' || d.id AS dia_pago,
       d.data, d.tipo, d.versao, d.lavrado_em,
       d.versao_motor, d.versao_dna, d.evento_origem,
       d.horas_trabalhadas * 60 AS lavra_trabalhadas_min,
       d.minutos_abonados AS lavra_abonados_min,
       d.minutos_previstos AS lavra_previstos_min,
       d.minutos_realizados AS lavra_realizados_min,
       f.horas_trabalhadas * 60 AS fechamento_trabalhadas_min,
       f.minutos_abonados AS fechamento_abonados_min,
       f.minutos_previstos AS fechamento_previstos_min,
       f.minutos_realizados AS fechamento_realizados_min
FROM ponto_fechamentomensal f
JOIN colaboradores_colaborador c ON c.id = f.colaborador_id
LEFT JOIN ponto_diapago d
       ON d.colaborador_id = f.colaborador_id
      AND d.mes = f.mes AND d.ano = f.ano
      AND d.versao = 'motor'
WHERE f.id = ANY(%(fechamentos)s)
ORDER BY c.empresa_id, f.colaborador_id, f.ano, f.mes, d.data
```

**Q2 — batidas para alimentar as autoridades existentes.** A seleção do universo apurável pertence a `batidas_apuraveis`; a atribuição e a paridade usadas pelo dono vêm do carregador, e não de contar batidas por data civil em SQL. [app/ponto/turnos.py:2875](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/turnos.py:2875), [app/ponto/management/commands/e6_oraculo.py:150](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:150)

```sql
SELECT 'col' || b.colaborador_id AS colab,
       '#' || b.id AS batida,
       b.timestamp, b.tipo, b.is_intervalo,
       b.retratada_em
FROM ponto_batida b
WHERE b.colaborador_id = ANY(%(colabs)s)
ORDER BY b.colaborador_id, b.timestamp, b.id
```

**Q3 — cadastro, geradora e folgas.** Os resultados devem voltar a `cadastro_do_dia`/`cadastro_do_dia_carregado`; os SELECTs não escolhem o vínculo nem derivam os flags. [app/ponto/supra_juiz.py:155](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/supra_juiz.py:155), [app/ponto/supra_juiz.py:179](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/supra_juiz.py:179), [app/escala/models.py:1241](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/escala/models.py:1241)

```sql
SELECT 'col' || c.id AS colab,
       'emp' || c.empresa_id AS empresa,
       c.data_demissao, e.dia_inicio_competencia,
       '#' || v.id AS vinculo,
       v.data_inicio, v.data_fim, v.ativa,
       '#' || v.tipo_escala_id AS template,
       t.tipo_ciclo, t.folga_dia_semana
FROM colaboradores_colaborador c
JOIN colaboradores_empresa e ON e.id = c.empresa_id
LEFT JOIN escala_escalacolaborador v ON v.colaborador_id = c.id
LEFT JOIN escala_tipoescala t ON t.id = v.tipo_escala_id
WHERE c.id = ANY(%(colabs)s)
ORDER BY c.id, v.id
```

```sql
SELECT 'col' || colaborador_id AS colab,
       '#' || id AS celula, data, trabalha,
       '#' || escala_geradora_id AS geradora,
       dna -> 'marcos' AS marcos,
       ata -> 'tipo_dia' AS tipo_dia,
       ata -> 'lampadas' AS lampadas,
       ata -> 'orfas' AS orfas
FROM escala_celuladia
WHERE colaborador_id = ANY(%(colabs)s)
  AND data BETWEEN %(ini)s AND %(fim)s
ORDER BY colaborador_id, data
```

```sql
SELECT 'col' || v.colaborador_id AS colab,
       '#' || f.id AS folga,
       '#' || f.escala_colaborador_id AS vinculo,
       f.data
FROM escala_folgadia f
JOIN escala_escalacolaborador v ON v.id = f.escala_colaborador_id
WHERE v.colaborador_id = ANY(%(colabs)s)
ORDER BY v.colaborador_id, f.data
```

**Q4 — lavras de CXR e fase, preservando cobertura.** CXR não coberto retorna `None`, não conjunto vazio; fase é lida pela lavra mais recente que cobriu cada dia. A decisão deve continuar passando por esses leitores e pelo juiz existente. [app/ponto/services/cxr_na_lista.py:165](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/services/cxr_na_lista.py:165), [app/escala/services/fase_12x36.py:117](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/escala/services/fase_12x36.py:117)

```sql
SELECT '#' || s.id AS snapshot,
       s.chave, s.escopo, s.escopo_id,
       s.data_ref, s.calculado_em,
       s.detalhe -> 'agora' AS agora,
       s.detalhe -> 'janelas' AS janelas,
       s.detalhe -> 'cobertos' AS cobertos,
       (
         SELECT jsonb_agg(jsonb_build_object(
                  'colab', 'col' || x.item ->> 'colab',
                  'empresa', 'emp' || x.item ->> 'empresa',
                  'data', x.item ->> 'data',
                  'competencia', x.item ->> 'competencia',
                  'dono', x.item ->> 'dono'
                ))
         FROM jsonb_array_elements(
           COALESCE(s.detalhe -> 'itens', '[]'::jsonb) ||
           COALESCE(s.detalhe -> 'itens_reabertos', '[]'::jsonb)
         ) AS x(item)
         WHERE (x.item ->> 'colab')::integer = ANY(%(colabs)s)
       ) AS itens_dos_colabs
FROM inteligencia_metricasnapshot s
WHERE s.data_ref <= %(hoje)s
  AND (
    (s.chave = 'cxr_do_motor' AND s.escopo = 'frota')
    OR
    (s.chave = 'desvio_de_cadastro_12x36'
     AND s.escopo = 'tenant' AND s.escopo_id = 0)
  )
ORDER BY s.data_ref DESC, s.calculado_em DESC
```

Mesmo com essa coleta, faltaria demonstrar **quais dias explicam a diferença mensal**; aplicar o dono a todos os dias e eleger um deles como dono do mês acrescentaria uma regra ausente da função. [app/ponto/management/commands/e6_oraculo.py:53](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:53)

Para o **FIM**, faltam as chaves completas do universo 2323, do TSV 2381 e, principalmente, do conjunto esperado 2304. A sonda colada contém somente a constante `ESPERADO = 2304`; não demonstra onde obter a composição desse conjunto por SELECT. [nao verificado — sonda/anexo colados]

Este SELECT confrontaria os conjuntos **depois de fornecidos**, sem reproduzir a seleção da sonda; não recupera a referência ausente. [nao verificado — proposta não executada]

```sql
SELECT 'emp' || COALESCE(u.emp, e.emp) AS empresa,
       'col' || COALESCE(u.colab, e.colab) AS colab,
       COALESCE(u.mes, e.mes) AS mes,
       COALESCE(u.ano, e.ano) AS ano,
       COALESCE(u.dia, e.dia) AS dia,
       u.dia IS NOT NULL AS no_universo,
       e.dia IS NOT NULL AS no_esperado
FROM jsonb_to_recordset(%(universo_json)s::jsonb)
     AS u(emp integer, colab integer, mes integer, ano integer, dia date)
FULL JOIN jsonb_to_recordset(%(esperado_json)s::jsonb)
     AS e(emp integer, colab integer, mes integer, ano integer, dia date)
USING (emp, colab, mes, ano, dia)
WHERE u.dia IS NULL OR e.dia IS NULL
ORDER BY empresa, colab, ano, mes, dia
```

## 15/11 01:26 · sessao 7e9c9062581d · BATIDA-DEDUP-RETRATADA · sonda · origem=Code

```python
# Django shell; tenant juliani previamente ativo no envelope.
# Execucao e resultados de producao: [nao verificado].
# Correspondencia da copia ao HEAD Git: [nao verificado].
#
# Campos Batida: app/ponto/models.py:22, app/ponto/models.py:67.
# Empresa: app/colaboradores/models.py:40; FK do colaborador:
# app/colaboradores/models.py:321.
# Disputa/contexto: app/chamados/models.py:2494,
# app/chamados/models.py:1783, app/chamados/models.py:1792.
# Envelope ultima_batida: app/api/views.py:758.
#
# Terminal usado: PerguntaDisputa.fora_do_fato(),
# app/chamados/models.py:3276; predicado em :3284.
# Vida exata: PerguntaDisputa.viva, app/chamados/models.py:3474,
# delegacao em :3484. q_vivas e apenas pre-filtro (:3425).
# Exportada: folha.export.competencia_entregue,
# app/folha/export.py:556; consulta/retorno em :588.
# Janela por empresa: ponto.janelas.janela_atual,
# app/ponto/janelas.py:60. Nenhuma dessas regras e replicada.
#
# Convencoes desta sonda:
# 3a: 60 datas locais, incluindo hoje ate o instante da leitura.
# Recorte pelo timestamp da retratada, nao por retratada_em.
# A outra pode estar fora do recorte; janela inclusiva de +/- 5 minutos.
# OUTRAS_* conta PKs distintos; PARES_* conta retratada -> outra.
# COLABORADORES_DISTINTOS conta colaboradores com retratada que tem outra.
# T4: todo o acervo candidato, sem recorte de 60 dias.
# EXPORTADA = entregue em qualquer dos dois dias; ABERTA = em nenhum.
# ABERTA aqui significa somente "nao exportada".
# Erros tornam os resultados parciais e FIM ok=0.
# Nenhuma mensagem inclui conteudo de excecao, nomes ou documentos.

erros = 0
nivel_logging_anterior = None


def erro(etapa, exc, pergunta_pk=None):
    global erros
    erros += 1
    print(
        "ERRO_ITEM etapa={} pergunta_pk={} classe={}".format(
            etapa, pergunta_pk if pergunta_pk is not None else "-",
            type(exc).__name__,
        ),
        flush=True,
    )


try:
    import logging
    import re
    from collections import Counter
    from contextlib import ExitStack
    from datetime import datetime, time, timedelta, timezone as dt_timezone
    from itertools import islice
    from unittest.mock import patch

    from django.db import connection, connections
    from django.utils import timezone
    from django.utils.dateparse import parse_datetime

    nivel_logging_anterior = logging.root.manager.disable
    logging.disable(logging.CRITICAL)

    alias_leitura = connection.alias

    def somente_select(execute, sql, params, many, context):
        if (
            many
            or context["connection"].alias != alias_leitura
            or not re.match(r"\s*SELECT\b", sql, re.I)
            or ";" in sql
            or re.search(
                r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b",
                sql, re.I,
            )
        ):
            raise RuntimeError("consulta_recusada")
        return execute(sql, params, many, context)

    def best_effort_falhou(logger_nome, operacao, exc, **contexto):
        # Captura sem escrever log; autoridade de observabilidade:
        # app/core/observ.py:73; excepts do juiz:
        # app/chamados/juizes.py:1473, app/chamados/juizes.py:1486.
        erro("best_effort", exc)

    local = dt_timezone(timedelta(hours=-3))

    with ExitStack() as pilha:
        for alias in connections:
            pilha.enter_context(
                connections[alias].execute_wrapper(somente_select)
            )
        pilha.enter_context(timezone.override(local))
        pilha.enter_context(
            patch("core.observ.registrar_engolido", new=best_effort_falhou)
        )

        from colaboradores.models import Colaborador, Empresa
        from ponto.models import Batida
        from chamados.models import PerguntaDisputa
        from chamados.juizes import tranca_da_consulta
        from folha.export import competencia_entregue
        from ponto.janelas import janela_atual

        # Nenhum SET, atomic, savepoint ou troca de tenant nesta sonda.
        with connection.cursor() as cursor:
            cursor.execute("SELECT current_schema()")
            if cursor.fetchone()[0] != "juliani":
                raise RuntimeError("tenant_incorreto")

        empresas = {
            e.pk: e
            for e in Empresa.objects.using(alias_leitura).only(
                "pk", "dia_inicio_competencia"
            )
        }

        agora = timezone.now().astimezone(local)
        inicio = datetime.combine(
            agora.date() - timedelta(days=59), time.min, tzinfo=local
        )
        print(
            "JANELA_3A inicio_inclusivo={} fim_exclusivo={} datas_locais=60".format(
                inicio.isoformat(), agora.isoformat()
            )
        )

        # 3a: LEFT JOIN preserva tambem retratadas sem outra.
        bt = connection.ops.quote_name(Batida._meta.db_table)
        ct = connection.ops.quote_name(Colaborador._meta.db_table)
        sql = f"""
            SELECT
                c.empresa_id,
                COUNT(DISTINCT r.id),
                COUNT(DISTINCT r.id) FILTER (WHERE o.id IS NOT NULL),
                COUNT(DISTINCT r.id) FILTER (
                    WHERE o.id IS NOT NULL AND o.retratada_em IS NULL
                ),
                COUNT(DISTINCT o.id),
                COUNT(DISTINCT o.id) FILTER (
                    WHERE o.retratada_em IS NULL
                ),
                COUNT(o.id),
                COUNT(o.id) FILTER (WHERE o.retratada_em IS NULL),
                COUNT(DISTINCT r.colaborador_id) FILTER (
                    WHERE o.id IS NOT NULL
                )
            FROM {bt} r
            JOIN {ct} c ON c.id = r.colaborador_id
            LEFT JOIN {bt} o
                ON o.colaborador_id = r.colaborador_id
                AND o.tipo = r.tipo
                AND o.id <> r.id
                AND o.timestamp BETWEEN r.timestamp - %s
                                    AND r.timestamp + %s
            WHERE r.retratada_em IS NOT NULL
                AND r.timestamp >= %s
                AND r.timestamp < %s
            GROUP BY c.empresa_id
            ORDER BY c.empresa_id
        """
        with connection.cursor() as cursor:
            cursor.execute(
                sql, [timedelta(minutes=5), timedelta(minutes=5), inicio, agora]
            )
            por_empresa = {
                row[0]: tuple(int(n) for n in row[1:])
                for row in cursor.fetchall()
            }

        metricas = (
            "RETRATADAS",
            "RETRATADAS_COM_OUTRA",
            "RETRATADAS_COM_OUTRA_VIVA",
            "OUTRAS_DISTINTAS",
            "OUTRAS_VIVAS_DISTINTAS",
            "PARES",
            "PARES_OUTRA_VIVA",
            "COLABORADORES_DISTINTOS",
        )
        total = [0] * len(metricas)
        for empresa_pk in sorted(set(empresas) | set(por_empresa)):
            valores = por_empresa.get(empresa_pk, (0,) * len(metricas))
            for i, (chave, valor) in enumerate(zip(metricas, valores)):
                print(f"3A_EMPRESA_{empresa_pk}_{chave} {valor}")
                total[i] += valor
        for chave, valor in zip(metricas, total):
            print(f"3A_TOTAL_{chave} {valor}")

        # T4: cache somente em memoria; a autoridade continua sendo chamada.
        cache_exportada = {}

        def exportada_no_dia(empresa, dia):
            _, fim = janela_atual(dia, empresa)
            chave = (empresa.pk, fim.month, fim.year)
            if chave not in cache_exportada:
                cache_exportada[chave] = competencia_entregue(*chave)
            return cache_exportada[chave]

        cont = Counter()
        exportadas_pks = []
        candidatas = (
            PerguntaDisputa.objects.using(alias_leitura)
            .filter(PerguntaDisputa.q_vivas())
            .select_related(
                "disputa__colaborador__empresa",
                "celula",
                "chamado_destino",
                "batida_origem",
            )
            .defer(
                "disputa__colaborador__nome",
                "disputa__colaborador__cpf",
            )
            .order_by("pk")
            .iterator(chunk_size=200)
        )

        while True:
            bloco = list(islice(candidatas, 200))
            if not bloco:
                break

            preparados = []
            for p in bloco:
                try:
                    if p.fora_do_fato():
                        continue
                    cont["CANDIDATAS_EXAMINADAS"] += 1
                    ctx = p.disputa.contexto
                    if ctx is None:
                        ctx = {}
                    if not isinstance(ctx, dict):
                        raise ValueError("contexto_invalido")
                    ub = ctx.get("ultima_batida")
                    if ub is None:
                        cont["CANDIDATAS_SEM_ULTIMA_BATIDA"] += 1
                        continue
                    if not isinstance(ub, dict):
                        raise ValueError("ultima_batida_invalida")
                    bruto_id = ub.get("id")
                    if (
                        isinstance(bruto_id, bool)
                        or not isinstance(bruto_id, (int, str))
                        or not re.fullmatch(r"[0-9]+", str(bruto_id))
                        or int(bruto_id) <= 0
                    ):
                        raise ValueError("batida_id_invalido")
                    bruto_ts = ub.get("timestamp")
                    if not isinstance(bruto_ts, str):
                        raise ValueError("timestamp_ausente_ou_invalido")
                    ts = parse_datetime(bruto_ts)
                    if ts is None or timezone.is_naive(ts):
                        raise ValueError("timestamp_invalido_ou_sem_fuso")
                    preparados.append(
                        (p, int(bruto_id), ts.astimezone(local).date())
                    )
                except Exception as exc:
                    erro("t4_contexto", exc, p.pk)

            batidas = {
                b["pk"]: b
                for b in Batida.objects.using(alias_leitura)
                .filter(pk__in={bid for _, bid, _ in preparados})
                .values("pk", "colaborador_id", "timestamp")
            }

            diferentes = []
            for p, bid, dia_contexto in preparados:
                try:
                    b = batidas.get(bid)
                    if b is None:
                        raise ValueError("batida_referenciada_ausente")
                    cid = p.disputa.colaborador_id
                    if b["colaborador_id"] != cid or p.colaborador_id != cid:
                        raise ValueError("colaborador_divergente")
                    if timezone.is_naive(b["timestamp"]):
                        raise ValueError("batida_timestamp_sem_fuso")
                    dia_batida = b["timestamp"].astimezone(local).date()
                    if dia_contexto != dia_batida:
                        diferentes.append((p, dia_contexto, dia_batida))
                except Exception as exc:
                    erro("t4_batida", exc, p.pk)

            # Autoridade de tranca em lote, sem reconstruir seu predicado:
            # app/chamados/juizes.py:1128.
            with tranca_da_consulta([p for p, _, _ in diferentes]):
                for p, dia_contexto, dia_batida in diferentes:
                    try:
                        if not p.viva:
                            continue
                        empresa = empresas[p.disputa.colaborador.empresa_id]
                        exp_ctx = exportada_no_dia(empresa, dia_contexto)
                        exp_bat = exportada_no_dia(empresa, dia_batida)

                        cont["VIVAS_DIAS_DIFERENTES"] += 1
                        if exp_ctx or exp_bat:
                            cont["EXPORTADA"] += 1
                            detalhe = (
                                "AMBAS" if exp_ctx and exp_bat
                                else "SO_CONTEXTO" if exp_ctx
                                else "SO_BATIDA"
                            )
                            cont["EXPORTADA_" + detalhe] += 1
                            if len(exportadas_pks) < 20:
                                exportadas_pks.append(p.pk)
                        else:
                            cont["ABERTA"] += 1
                    except Exception as exc:
                        erro("t4_vida_exportacao", exc, p.pk)

        for chave in (
            "CANDIDATAS_EXAMINADAS",
            "CANDIDATAS_SEM_ULTIMA_BATIDA",
            "VIVAS_DIAS_DIFERENTES",
            "EXPORTADA",
            "ABERTA",
            "EXPORTADA_AMBAS",
            "EXPORTADA_SO_CONTEXTO",
            "EXPORTADA_SO_BATIDA",
        ):
            print(f"T4_{chave} {cont[chave]}")
        print(
            "T4_EXPORTADA_PERGUNTA_PKS "
            + (",".join(map(str, exportadas_pks)) or "-")
        )
        print(f"T4_PAREI_EXPORTADA {int(cont['EXPORTADA'] > 0)}")

except BaseException as exc:
    erro("sonda", exc)
finally:
    if nivel_logging_anterior is not None:
        logging.disable(nivel_logging_anterior)
    print(f"ERRO {erros}", flush=True)
    print(f"FIM ok={int(erros == 0)}", flush=True)
```

## 15/11 01:06 · sessao a83435cd3ae0 · CONTRATO-DE-ENTRADA-DO-FIO · construtor · origem=Code

Entrega em [cef_c1_entrega.txt](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c1_entrega.txt:1), com curas T1/T2/T3 e selo T5.

[Verificação local](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c1_verificacao.txt:1): oito mutantes mortos, controles verdes.

T4 ficou como PERGUNTA pelo significado declarado no [leitor](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/juizes.py:151). Suite com banco e certificação final: [nao verificado]. [nao verificado]

## 15/11 00:54 · sessao 7011158ac77e · - · - · origem=Ronald (tmux)

Censo estático da cópia; nenhum arquivo escrito. AGENTS.md aplicável não localizado; descoberta de instruções por mecanismos dinâmicos [nao verificado].

| Campo | Escritores atuais | Leitores atuais |
|---|---|---|
| `ultima_batida.id` | APIs: `api/views.py:758-761`, `api/views_core.py:752-755`; agregado substitui chave de topo em `chamados/services/disputa_emissao.py:548-562`; nascimento persiste em `:590` | Origem BATIDA: `chamados/models.py:3603-3610`; referência para grade: `chamados/services/disputa_emissao.py:72-76`; dia: `:387-392`; upload: `api/foto_recebida.py:67-68` |
| `ultima_batida.tipo` | `api/views.py:760`, `api/views_core.py:754` | Nenhum leitor semântico específico localizado; despacho dinâmico [nao verificado] |
| `ultima_batida.timestamp` | Recepção `agora`: `api/views.py:497`, `:761`; `api/views_core.py:483`, `:755` | Único leitor executável localizado: `chamados/juizes.py:159-166`, que extrai `.date()`; singular chega por `:138`, lote por `:226` |
| `foto_ausente.tipo_batida` | APIs: `api/views.py:453`, `api/views_core.py:439`; retro: `ponto/management/commands/marcar_foto_ausente_retro.py:99` | Nenhum leitor semântico específico localizado; dinâmico [nao verificado] |
| `foto_ausente.batida_id` | Retro: `ponto/management/commands/marcar_foto_ausente_retro.py:100` | Nenhum leitor semântico específico localizado; origem lê outro caminho em `chamados/models.py:3606` |
| `foto_ausente.batida_em` | Retro, instante local da batida: `ponto/management/commands/marcar_foto_ausente_retro.py:101` | Nenhum leitor semântico específico localizado; gerador usa episódio em `chamados/services/disputa_emissao.py:387-392` |
| `foto_ausente._por` | Retro: `ponto/management/commands/marcar_foto_ausente_retro.py:102` | Nenhum leitor semântico específico localizado; dinâmico [nao verificado] |
| `contexto[motivo].data` | Triagem: `ponto/services/triagem_batida.py:739-740`; fábrica: `chamados/services/fabrica_da_celula.py:172`; enriquecimento: `chamados/services/disputa_emissao.py:283` | Gerador lê antes de buscar episódio em `chamados/services/disputa_emissao.py:372-377` |
| `contexto[motivo].qtd` / `ultima_em` | Agregação: `chamados/services/disputa_emissao.py:552-558` | `qtd` anterior lido pelo próprio merge em `:552`; outro leitor semântico específico [nao verificado] |

**Ressalva T4:** o código do único leitor executável consome apenas o dia civil do ISO (`chamados/juizes.py:166`), mas seu docstring atribui explicitamente aos carimbos o significado “QUANDO o problema foi notado” (`:151-156`); `dia_do_fato` também chama batida/disputa/carimbos de “QUANDO foi notado” (`:93-95`). Portanto, a conclusão “nenhum consumidor exige recepção” é inferência sobre execução; conflito documental deve constar da entrega. A varredura não encontrou decisão, cálculo de latência ou seleção baseada na recepção desse envelope; completude para despacho dinâmico [nao verificado].

O contexto fraco vem do **chamado** (`chamados/juizes.py:100`, `:138`), cujo contexto nasce separadamente do da disputa (`chamados/services/disputa_emissao.py:580`, `:590`). Em disputa já aberta, o merge atualiza somente a disputa nesse ramo (`:543-566`). A fusão de recorrência do chamado preserva escalar já existente (`chamados/services/contexto.py:131-156`).

Os cinco chamadores diretos da porta são:

- APIs, potencialmente BATIDA: `api/views.py:764-766`, `api/views_core.py:758-760`.
- Retro, motivo literal BATIDA `foto_ausente`: `ponto/management/commands/marcar_foto_ausente_retro.py:96-104`.
- Triagem, motivo literal MARCO `saida_sem_entrada`: `ponto/services/triagem_batida.py:735-744`.
- Fábrica, motivo variável originado da ata: `chamados/services/fabrica_da_celula.py:139`, `:157`, `:160-174`.

Família BATIDA declarada pelo catálogo: `chamados/catalogo/motivos.py:16`, `:36-47`; `foto_ausente` em `:43`. O índice correspondente identifica `(batida_origem, motivo)` e tem condições próprias de horizonte, conversa, via e origem preenchida (`chamados/models.py:3631-3636`).

Autoridades e padrão para a linha LEI-AKITA:

- Casos pela regra antes do código: `docs/LEIS.md:136`.
- Dia do turno por PK: `ponto/turnos.py:2927-2936`; delega a `dia_das_batidas`, cuja batida não reclamada usa o dia local (`:2955`). Registro do juiz: `core/juizes.py:212`.
- Juiz da selfie: `core/juizes.py:1205`.
- Corte do upload por episódio e incluindo foto tardia: `docs/CORTES.md:441`.
- Validação pelo escritor único: `docs/JUIZES.md:38-39`, usada pelo upload em `api/foto_recebida.py:48-51`.
- `padrao=idempotencia pela chave do fato` consta de `docs/GLOSSARIO.md:29`; para o selo AST, `Architecture Conformance Test` consta de `:15`.

Homônimos `ultima_batida` da ficha/ranking não são este envelope: escritores em `colaboradores/services/ficha.py:147`, `escala/services/furos_diarios.py:281`; leitores em `colaboradores/services/ficha.py:215-216`, `relatorios/views.py:1675`. Templates também leem esses homônimos, não o JSON da disputa: `templates/colaboradores/partials/_ps_linha.html:20`, `templates/relatorios/ranking_furos.html:76`.

## 15/11 00:54 · sessao 95b31c10c245 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhum arquivo escrito.

- O juiz geral já existe: `DisputaSupervisao.pode_fechar()` consulta respostas sem veredito e `fio_vivo`; `fechar()` recusa perguntas vivas. (`chamados/models.py:1911`, `chamados/models.py:1950`, `chamados/models.py:1960`)
- Para preservar o motivo enquanto outra pergunta de foto continua viva, o upload pode consultar `fio_vivo(d.perguntas.filter(motivo='foto_ausente'))`: esse redutor delega a `pergunta_viva`, sem novo predicado. (`chamados/juizes.py:1504`)
- `aparar_motivo_respondido()` preserva perguntas, remove motivo/contexto e não fecha a disputa; deve vir somente depois da consulta das irmãs. (`chamados/models.py:2016`)
- A validação já pode fechar a disputa: `validar_pergunta()` chama o materializador, cuja cauda consulta `pode_fechar()` e executa `fechar()`. Recarregar `d` depois de `_validar_pelo_dono` evita agir sobre estado anterior. (`chamados/services/validacao.py:494`, `chamados/services/materializacao.py:1384`, `chamados/services/materializacao.py:1322`)
- Há infraestrutura pronta para teste sem banco: `funcao()` compila a função real por AST; `fonte_real()` admite mutações somente em memória. (`chamados/tests/e4_apoio.py:8`, `chamados/tests/e4_catracas.py:5`)
- Há precedente de `unittest` puro com infraestrutura instrumentada por `patch.dict(sys.modules)`. (`chamados/tests/test_e3_tarefa_admin_sem_banco.py:19`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:82`)
- O teste atual do upload só confere gravação de `Batida.foto` e `FotoBatida`; não cobre fechamento por episódio. (`chamados/tests/test_b8_smoke_portas.py:60`)
- O teste existente de duas batidas na mesma disputa cobre episódios distintos e repetição, mas usa envelope sintético e somente ordem cronológica. (`chamados/tests/test_e1_v6_horizonte.py:257`, `chamados/tests/test_e1_v6_horizonte.py:262`)
- Os testes de atos existentes prendem a chamada `aparar_motivo_respondido` no upload e sua idempotência; mantê-la evita romper esses selos. (`chamados/tests/test_porta_chamado_e3.py:56`, `chamados/tests/test_porta_chamado_e3.py:110`)

## 15/11 00:52 · sessao 8db5c2184c1a · MADRUGADA-PREPARA · sonda · origem=Code

O previsto será calculado por `minutos_previstos_do_dia`; o pagamento diário será lido de `DiaPago`, conferido contra o `FechamentoMensal` gravado que o exportador lê. As rubricas permanecem separadas: somar trabalho, extras e seus subconjuntos produziria dupla contagem. (`app/escala/utils.py:2230`; `app/ponto/services/dia_pago.py:833`; `app/folha/export.py:719`; `app/ponto/services/dia_pago.py:904`)

A remede colada parte de `UNIV` literal; sua reconstrução original não está demonstrada por ela. [nao verificado] A sonda abaixo usa `F.dias_que_seguram`, que delega à retenção real, e depois aplica a seleção da remede com `F._carga`, `porta_da_tarefa` e o prazo do **pai da disputa**. (`app/folha/fora_da_folha.py:109`; `app/folha/fora_da_folha.py:105`; `app/folha/fora_da_folha.py:58`; `app/chamados/services/tarefa_do_dia.py:638`; seleção colada: [nao verificado])

Não identifiquei função real que receba o cenário completo “tornar este dia ausência descontante/abonado/manter” e devolva os dois deltas solicitados; existência de outra API permanece [nao verificado]. As funções próximas leem ausências existentes ou grade já julgada. Portanto, os três cenários imprimem `sem funcao real`; a leitura atual continua disponível em cada dia. (`app/ponto/services/efeito_ausencia.py:28`; `app/folha/export.py:125`; `app/ponto/services/dia_pago.py:364`)

DSR perdido é decidido por semana; reflexo de DSR é lavrado em ajuste mensal sem data. A sonda mostra esse contexto uma vez por colaborador/competência, sem rateá-lo pelos dias. (`app/ponto/motor_calculo_v2.py:2600`; `app/ponto/services/dia_pago.py:348`)

| Campos/saídas consumidos | Quem escreve/produz | Quem lê |
|---|---|---|
| `Empresa.pk`, `dia_inicio_competencia` | `criar_empresa`, `atualizar_empresa`. (`app/colaboradores/services/estrutura.py:13`; `app/colaboradores/services/estrutura.py:18`) | Janela da competência. (`app/ponto/janelas.py:36`; `app/ponto/janelas.py:54`) |
| `Colaborador.pk`, `empresa`, admissão/demissão, cadastro delegado | Portas de criação e gravação do colaborador. (`app/colaboradores/services/colaborador.py:32`; `app/colaboradores/services/colaborador.py:55`; `app/colaboradores/services/colaborador.py:91`) | Universo e retenção. (`app/folha/fora_da_folha.py:138`; `app/folha/export.py:1239`) |
| Dias retornados: `data`; célula: `veredito`, `protestos`, `trabalha` | Retorno produzido pela retenção; veredito/protestos lavrados pela porta da célula. (`app/folha/export.py:1259`; `app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1092`) | `dias_que_seguram` e retenção. (`app/folha/fora_da_folha.py:144`; `app/folha/export.py:1273`) |
| Tarefa: `instancias`, `pergunta_id`, ações `acao/de/porta`, `celula_id`, `colaborador_id`, `dia_do_turno`, `fio` | Juiz da tarefa, em memória. (`app/chamados/services/tarefa_do_dia.py:127`; `app/chamados/services/tarefa_do_dia.py:197`; `app/chamados/services/tarefa_do_dia.py:557`; `app/chamados/services/tarefa_do_dia.py:635`) | Projeção da porta e folha. (`app/chamados/services/tarefa_do_dia.py:638`; `app/folha/fora_da_folha.py:70`) |
| Alimentação: `fila_ids`, `fila_cel`, `fila_dia`; perguntas por colaborador/dia | Carregadores reais. (`app/chamados/juizes.py:1559`; `app/chamados/juizes.py:1589`; `app/chamados/services/tarefa_do_dia.py:395`) | Tarefa. (`app/chamados/services/tarefa_do_dia.py:596`; `app/chamados/services/tarefa_do_dia.py:602`) |
| Pergunta: `pk`, `disputa`, destino; disputa: `chamado`; pai: `prazo_sla` | Nascimento da pergunta/disputa; prazo inicial e renovação pelas portas existentes. (`app/chamados/models.py:2854`; `app/chamados/services/disputa_emissao.py:587`; `app/chamados/services/disputa_emissao.py:581`; `app/chamados/models.py:574`) | Âncora e relógio reais. (`app/chamados/services/fio.py:97`; `app/chamados/catalogo/motor.py:54`) |
| Previsto diário | `minutos_previstos_do_dia`, a partir de vínculo, trabalho, marcos e pausas. (`app/escala/utils.py:2230`; `app/escala/utils.py:2260`; `app/escala/utils.py:2269`; `app/escala/utils.py:2291`) | Motor e fechamento. (`app/escala/utils.py:2239`) |
| `DiaPago`: chaves e **todos os campos de `CAMPOS_DIA`**, `detalhe.folga_feriado_min` | `lavrar`, incluindo mapas da grade; retrato da exportada também atualiza os três campos de minutos. (`app/ponto/services/dia_pago.py:135`; `app/ponto/services/dia_pago.py:257`; `app/ponto/services/dia_pago.py:288`; `app/ponto/services/dia_pago.py:345`; `app/ponto/services/dia_pago.py:481`) | `linhas_do_periodo`, `pago_do_dia`, soma canônica. (`app/ponto/services/dia_pago.py:833`; `app/ponto/services/dia_pago.py:867`; `app/ponto/services/dia_pago.py:550`) |
| `FechamentoMensal`: existência, chaves, campos equivalentes a `CAMPOS_DIA` | Recalculo, persistido por `save/update`. (`app/ponto/services/fechamento.py:898`; `app/ponto/services/fechamento.py:1144`; `app/ponto/services/fechamento.py:1160`) | Autoridade mensal do TXT. (`app/folha/export.py:719`; `app/folha/export.py:730`) |
| Semanas DSR; reflexo e banco no ajuste | Fechamento e lavratura do ajuste. (`app/ponto/services/fechamento.py:1101`; `app/ponto/services/fechamento.py:1104`; `app/ponto/services/dia_pago.py:350`) | Soma da competência; o exportador não emite reflexo de DSR como evento. (`app/ponto/services/dia_pago.py:562`; `app/folha/export.py:453`) |
| Competência entregue | Registro/invalidação da exportação. (`app/folha/models.py:174`; `app/folha/models.py:143`) | `competencia_entregue`. (`app/folha/export.py:556`) |
| PK do usuário reservado de sistema | Escritor desse cadastro: [nao verificado] | Preparo de leitura para evitar `get_or_create` nas dependências. (`app/chamados/management/commands/diff_e3_sombra.py:96`; `app/chamados/management/commands/diff_e3_sombra.py:100`) |

Fechamento de **todos** os escritores transitivos das funções delegadas: [nao verificado].

A entrada é `sonda(tsv_texto)`, com o conteúdo integral do TSV recebido; ele só participa da conferência. Essa entrada explícita é necessária porque o envelope monta a árvore e a sonda, sem montar automaticamente os anexos. (`bin/sonda_leitura.sh:217`; `bin/sonda_leitura.sh:238`) Sintaxe analisada em memória, sem executar Django; execução e resultados: [nao verificado]. O relógio reproduz o instante informado, mas a própria tarefa declara que parte do estado consultado continua atual. (`app/chamados/services/tarefa_do_dia.py:374`)

```python
import csv
import datetime as dt
import io
import json
import logging
import re
import sys
from collections import Counter, defaultdict
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from decimal import Decimal, localcontext
from unittest.mock import patch

# Valores do anexo colado: [nao verificado] por arquivo local.
AGORA = dt.datetime.fromisoformat("2026-11-15T02:20:00+00:00")
COMPETENCIAS = ((9, 2026), (10, 2026), (11, 2026))
ESPERADO = 2304
SEM_FUNCAO = "sem funcao real"


class EscritaProibida(BaseException):
    pass


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass


def bloquear(*args, **kwargs):
    raise EscritaProibida()


def so_select(execute, sql, params, many, context):
    # Molde: app/chamados/management/commands/diff_e3_sombra.py:30.
    # SET também é recusado; tenant já escolhido pelo envelope.
    s = str(sql).strip()
    if (
        many
        or context["connection"].alias != "default"
        or not re.match(r"^SELECT\b", s, re.I)
        or ";" in s
        or "--" in s
        or "/*" in s
        or re.search(
            r"\b(INSERT|UPDATE|DELETE|MERGE|INTO|CREATE|ALTER|DROP|"
            r"TRUNCATE|COPY|CALL|DO|SET|RESET|GRANT|REVOKE|"
            r"NEXTVAL|SETVAL|PG_ADVISORY_LOCK|PG_TRY_ADVISORY_LOCK|"
            r"PG_ADVISORY_XACT_LOCK|PG_TRY_ADVISORY_XACT_LOCK|"
            r"LO_IMPORT|LO_EXPORT|PG_TERMINATE_BACKEND|PG_CANCEL_BACKEND|"
            r"PG_RELOAD_CONF|SET_CONFIG)\b|\bFOR\s+(SHARE|KEY)\b",
            s, re.I,
        )
    ):
        raise EscritaProibida()
    return execute(sql, params, many, context)


def sonda(tsv_texto):
    """Entrada de conferência: conteúdo INTEGRAL do TSV fornecido."""
    saida = sys.stdout
    erros = Counter()
    candidatos = set()
    universo = {}
    linhas = []
    referencia = set()
    partes = []
    tsv_linhas = 0
    completo = False
    campos = ()

    def emitir(tipo, **dados):
        saida.write(tipo + "|" + json.dumps(
            dados, ensure_ascii=False, sort_keys=True,
            default=str, allow_nan=False,
        ) + "\n")

    def erro(etapa, emp=None, cid=None, dia=None, comp=None):
        erros[etapa] += 1
        emitir(
            "ERRO", etapa=etapa,
            empresa=None if emp is None else f"emp{emp}",
            colab=None if cid is None else f"col{cid}",
            dia=None if dia is None else dia.isoformat(),
            comp=comp,
        )

    def dec(valor):
        if valor is None:
            return None
        n = Decimal(str(valor))
        if not n.is_finite():
            raise ValueError()
        return n

    def somar(rs):
        out = {
            "dias": len(rs),
            "colabs_distintos": len({r["cid"] for r in rs}),
            "estados": dict(sorted(
                Counter(r["estado"] for r in rs).items()
            )),
        }
        for nome in ("previsto_min", "pago_trabalho_min", *campos):
            valores = [
                r.get(nome) for r in rs if r.get(nome) is not None
            ]
            out[nome] = {
                "minutos": (
                    sum(valores, Decimal(0)) if valores else None
                ),
                "dias_com_numero": len(valores),
                "dias_sem_numero": len(rs) - len(valores),
            }
        return out

    nivel = logging.root.manager.disable
    try:
        logging.disable(logging.CRITICAL)
        with ExitStack() as stack, localcontext() as ctx:
            ctx.prec = 50
            stack.enter_context(redirect_stdout(Descartar()))
            stack.enter_context(redirect_stderr(Descartar()))

            from freezegun import freeze_time
            from django.db import connection, connections
            from django.db.models import Model
            from django.db.models.query import QuerySet
            from django.contrib.auth import get_user_model
            from django.utils import timezone
            from chamados.juizes import tranca_em_lote
            from chamados.models import PerguntaDisputa
            from chamados.catalogo.motor import prazo_estourou
            from chamados.services.fio import chamado_que_lista
            from chamados.services.tarefa_do_dia import porta_da_tarefa
            from colaboradores.models import Empresa
            from escala.utils import minutos_previstos_do_dia
            from folha import fora_da_folha as F
            from folha.export import competencia_entregue
            from ponto.janelas import janela_fechamento
            from ponto.models import DiaPago, FechamentoMensal
            from ponto.services.dia_pago import (
                CAMPOS_DIA, linhas_do_periodo, pago_do_dia,
                soma_da_competencia,
            )

            campos = tuple(CAMPOS_DIA)

            # bin/sonda_leitura.sh:237 e :239.
            if connection.settings_dict["NAME"] != "sombra":
                raise ValueError()
            if connection.schema_name != "juliani":
                raise ValueError()

            for alias in connections:
                stack.enter_context(
                    connections[alias].execute_wrapper(so_select)
                )
            for classe, metodos in (
                (Model, ("save", "delete")),
                (QuerySet, (
                    "create", "update", "delete", "_raw_delete",
                    "bulk_create", "bulk_update", "get_or_create",
                    "update_or_create",
                )),
            ):
                for metodo in metodos:
                    stack.enter_context(
                        patch.object(classe, metodo, bloquear)
                    )

            stack.enter_context(freeze_time(AGORA, tick=False))
            stack.enter_context(
                timezone.override("America/Sao_Paulo")
            )

            # TSV somente para conferência, nunca para seleção.
            leitor = csv.DictReader(
                io.StringIO(tsv_texto), delimiter="\t"
            )
            if not {"emp", "comp", "colab", "dia"} <= set(
                leitor.fieldnames or ()
            ):
                raise ValueError()

            for item in leitor:
                tsv_linhas += 1
                e = re.fullmatch(
                    r"(?:emp)?([0-9]+)", item["emp"].strip()
                )
                c = re.fullmatch(
                    r"(?:col)?([0-9]+)", item["colab"].strip()
                )
                m, a = map(int, item["comp"].split("/"))
                if not e or not c or (m, a) not in COMPETENCIAS:
                    raise ValueError()
                referencia.add((
                    int(e[1]), m, a, int(c[1]),
                    dt.date.fromisoformat(item["dia"]),
                ))

            # Preparo real: diff_e3_sombra.py:96.
            sistema_pk = (
                get_user_model().objects.filter(username="sistema")
                .values_list("pk", flat=True).first()
            )
            if sistema_pk is None:
                raise ValueError()
            stack.enter_context(
                tranca_em_lote(sistema_pk=sistema_pk)
            )

            # Universo: fora_da_folha.py:109/:58.
            for emp in Empresa.objects.order_by("pk"):
                for mes, ano in COMPETENCIAS:
                    comp = f"{mes:02d}/{ano}"
                    try:
                        u = F.dias_que_seguram(emp, mes, ano)
                        cs, por = u["colabs"], u["dias"]
                        ini, fim = janela_fechamento(
                            mes, ano, emp
                        )
                        retidos = [
                            cs[c] for c in por if por[c][1]
                        ]
                        if not retidos:
                            continue
                        carga = F._carga(
                            retidos, ini, fim, AGORA
                        )
                    except EscritaProibida:
                        raise
                    except Exception:
                        erro(
                            "carga_universo",
                            emp=emp.pk, comp=comp,
                        )
                        continue

                    for cid, (_motivos, dias) in sorted(por.items()):
                        for registro in dias:
                            dia = registro["data"]
                            chave = (emp.pk, mes, ano, cid, dia)
                            if chave in candidatos:
                                erro(
                                    "dia_duplicado",
                                    emp.pk, cid, dia, comp,
                                )
                                continue
                            candidatos.add(chave)

                            try:
                                colab = cs[cid]
                                t = carga.tarefa(colab, dia)
                                porta = porta_da_tarefa(t)
                                if (
                                    porta is None
                                    or str(porta["fio"]) != "colab"
                                ):
                                    continue

                                alim = carga._portas
                                perg = {
                                    p.pk: p for p in
                                    carga._perg.get((cid, dia), [])
                                }
                                acoes = [
                                    a for i in t["instancias"]
                                    for a in i["acoes"]
                                ]
                                pa = {
                                    a["porta"] for a in acoes
                                    if isinstance(a["porta"], int)
                                } & alim["fila_ids"]
                                cel = set(alim["fila_cel"].get(
                                    t["celula_id"], ()
                                )) & alim["fila_ids"]
                                anc = {
                                    chamado_que_lista(p)
                                    for p in perg.values()
                                } & alim["fila_ids"]
                                fdia = set(
                                    alim.get("fila_dia", {}).get((
                                        t.get("colaborador_id"),
                                        t.get("dia_do_turno"),
                                    ), ())
                                ) & alim["fila_ids"]
                                portas = pa | cel | anc | fdia

                                # Seleção literal da remede colada:
                                # [nao verificado] por arquivo local.
                                # Sem filtro extra de porta para prazo.
                                vencido = False
                                for i in t["instancias"]:
                                    for acao in i["acoes"]:
                                        if (
                                            acao["acao"] == "responder"
                                            and str(acao["de"]).lower()
                                                .startswith("colab")
                                            and i["pergunta_id"]
                                        ):
                                            p = perg.get(
                                                i["pergunta_id"]
                                            )
                                            if p is None:
                                                p = (
                                                    PerguntaDisputa.objects
                                                    .select_related(
                                                        "disputa__chamado"
                                                    )
                                                    .get(pk=i["pergunta_id"])
                                                )
                                            pai = p.disputa.chamado
                                            if (
                                                pai is not None
                                                and pai.prazo_sla
                                                and prazo_estourou(
                                                    pai.prazo_sla, AGORA
                                                )
                                            ):
                                                vencido = True

                                admin = {
                                    a["acao"] for a in acoes
                                    if a["de"] == "admin"
                                    and (
                                        not portas
                                        or a["porta"] in pa
                                    )
                                }
                                if vencido and not admin:
                                    universo[chave] = colab
                            except EscritaProibida:
                                raise
                            except Exception:
                                erro(
                                    "selecao_dia",
                                    emp.pk, cid, dia, comp,
                                )

            completo = not erros
            emitir(
                "CONFERENCIA",
                tsv_li

[truncado em 20000 de 31156 caracteres]

## 15/11 00:50 · sessao 8fb4cbca634a · MADRUGADA-PREPARA · censo · origem=Code

## (A) MATRIZ DE TRANSIÇÃO

Referências conferidas na cópia local; correspondência material com o hash informado: [nao verificado]. O envelope não contém registros individuais de disputa/pergunta; incidência atual em produção: [nao verificado].

**Estados e campos**

| Objeto | Estado/valor | Definição |
|---|---|---|
| Disputa | Aberta: `fechada_em=None`; fechada: instante preenchido. | `app/chamados/models.py:1804` |
| Fechamento | `motivo_fechamento`: `''`, `veredito`, `vazia`, `desligamento`, `admin_encerra`, `competencia_trancada`. `''` também admite legado. | `app/chamados/models.py:1827`; `app/chamados/models.py:1840` |
| Complemento do fechamento | `fechada_por`: autor ou nulo; `resolucao`: texto ou `''`. | `app/chamados/models.py:1813`; `app/chamados/models.py:1820` |
| Pergunta | Rótulos **derivados**, sem coluna de status: `pendente`, `respondida`, `validada`, `resolvida`. | `app/chamados/models.py:2694`; `app/chamados/models.py:3117` |
| Projeção da pergunta | `resposta_colab=''`/texto; `respondida_em` e `validada_em` nulos/preenchidos; `materializada_em` nulo/preenchido. | `app/chamados/models.py:2525`; `app/chamados/models.py:3502` |
| Precedência do rótulo | Validação preenchida resulta em `validada`, ou `resolvida` com materialização; nos demais casos, consulta C6 e resposta/carimbo. | `app/chamados/models.py:3124` |
| Vitalidade | `viva` é outro resultado: delega a `pergunta_viva`; não equivale ao rótulo `pendente`. | `app/chamados/models.py:3473`; `app/chamados/juizes.py:1399` |
| Via | `''`, `declaracao`, `completude_turno`, `fantasma`, `sem_turno_aberto`, `juiz_aprovou`, `admin_decidiu`, `paridade_recusada`, `celula_absolveu`, `cadastro_folga`, `adesao`, `fato_ocupado`, `fato_materializado`, `chamado_encerrado`, `duplicata`, `ausencia_dia`, `legado_sem_lastro`, `competencia_trancada`, `motivo_retratado`. | `app/chamados/models.py:2546`; `app/chamados/models.py:2562`; `app/chamados/models.py:2571`; `app/chamados/models.py:2639` |
| Fora do fato | Conjunto `duplicata`, `motivo_retratado`; as portas impedem retorno dessas linhas ao fato. | `app/chamados/models.py:2471`; `app/chamados/models.py:3263`; `app/chamados/models.py:3289` |
| Destino/dona | `chamado_destino`: FK nula/preenchida, `SET_NULL`; `disputa`: FK obrigatória, `CASCADE`. | `app/chamados/models.py:3494`; `app/chamados/models.py:2494` |
| Identidade | Colaborador, instância de marco, conversa e episódio; dia/célula identificam o fato datado. | `app/chamados/models.py:2491`; `app/chamados/models.py:2505`; `app/chamados/models.py:2518` |
| Lavra | `veredito_lavrado`: `''`, `sim`, `nao`, `indeterminado`; acompanha `julgado_em` e `janela_expira_em`. | `app/chamados/models.py:2655`; `app/chamados/models.py:2663`; `app/chamados/models.py:2668` |
| Trilha | Oito atos: `resposta`, `validacao`, `devolucao`, `recusa`, `reabertura`, `silencio`, `retratacao`, `absorcao`. | `app/chamados/models.py:4000` |
| Auxiliares | Contadores administrativos/colaborador; conferência de data; pendência administrativa e motivo de recusa. Não constituem novos rótulos da pergunta. | `app/chamados/models.py:1805`; `app/chamados/models.py:2536`; `app/chamados/models.py:2636` |

Para identificar os leitores nas matrizes:

| Referência | Leitor real |
|---|---|
| **APP** | `perguntas_do_app` e `particao_do_app`: viva sem resposta em dona aberta entra em pendentes; demais vivas entram no histórico. `app/chamados/services/pergunta_no_app.py:45`; `app/chamados/services/pergunta_no_app.py:76` |
| **ESTADO/C6** | `PerguntaDisputa.estado`, `pergunta_viva`/`_pergunta_viva_calc`. `app/chamados/models.py:3117`; `app/chamados/juizes.py:862`; `app/chamados/juizes.py:1399` |
| **VALIDAÇÃO** | `validar_pergunta`. `app/chamados/services/validacao.py:418` |
| **MATERIALIZAÇÃO** | `materializar_perguntas_validadas_da_disputa`. `app/chamados/services/materializacao.py:1344` |
| **HISTÓRICO** | `PerguntaDisputa.respostas_anteriores`. `app/chamados/models.py:3643` |
| **LAVRA** | `esta_stale` e leitores do raio X. `app/chamados/veredito_lavrado.py:224`; `app/chamados/services/raiox.py:136` |
| **RAIZ** | `colaborador_em_disputa`, signal do chamado-pai, relatório e motor. `app/chamados/services/disputa_emissao.py:34`; `app/chamados/signals.py:29`; `app/chamados/relatorios_disputa.py:11`; `app/ponto/services/disputa.py:31` |

**Transições da disputa**

| Transição | Escritor — arquivo::função:linha | Guarda conferida | Quem lê |
|---|---|---|---|
| **N1 — inexistente → aberta, núcleo** | `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa:587` | Trava universo do colaborador; procura aberta; cria na ausência. `app/chamados/services/disputa_emissao.py:530`; `app/chamados/services/disputa_emissao.py:531` | APP, RAIZ. |
| **N1 — inexistente → aberta, repasse** | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1123` | Exclui dias da lista administrativa; filtra regularizáveis/perguntas existentes; procura aberta. `app/chamados/services/disputa_emissao.py:1047`; `app/chamados/services/disputa_emissao.py:1086`; `app/chamados/services/disputa_emissao.py:1096` | APP, RAIZ. |
| **N1 — inexistente → aberta, âncora** | `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado:1358` | Categoria `ancora_limbo`, chamado não encerrado, sem pergunta anterior não materializada; procura aberta. `app/chamados/services/disputa_emissao.py:1316`; `app/chamados/services/disputa_emissao.py:1323`; `app/chamados/services/disputa_emissao.py:1332` | APP, RAIZ. |
| **N2 — aberta A → aberta A∪B** | Núcleo `::_abrir_ou_agregar_disputa:565`; repasse `::criar_disputa_a_partir_de_chamados_orfaos:1132`, `:1161`, `:1256`; âncora `::criar_disputa_ancora_de_chamado:1367`, todos em `app/chamados/services/disputa_emissao.py`. | Núcleo compara conjuntos e salva campos alterados; repasse/âncora agregam em ramos próprios. `app/chamados/services/disputa_emissao.py:537`; `app/chamados/services/disputa_emissao.py:564`; `app/chamados/services/disputa_emissao.py:1131`; `app/chamados/services/disputa_emissao.py:1366` | Gerador e relatório. `app/chamados/services/disputa_emissao.py:300`; `app/chamados/relatorios_disputa.py:11` |
| **N3 — fechada A → continua fechada, com complemento B** | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1084` | Seleciona pergunta sem materialização **sem exigir dona aberta**; exige dia, ata e complemento criado. `app/chamados/services/disputa_emissao.py:1070`; `app/chamados/services/disputa_emissao.py:1077`; `app/chamados/services/disputa_emissao.py:1081` | APP envia viva dessa dona ao histórico; vigia mede a classe. `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/vigia_fio.py:758` |
| **F1 — aberta → fechada `veredito`/`vazia`** | `app/chamados/models.py::DisputaSupervisao.fechar:1996`; persistência `app/chamados/models.py:2000`. | Autor; nenhuma resposta sem veredito; nenhuma viva. Zero perguntas escolhe `vazia`, demais `veredito`. `app/chamados/models.py:1948`; `app/chamados/models.py:1950`; `app/chamados/models.py:1960`; `app/chamados/models.py:1967` | APP, signal do pai; contador de vazias. `app/chamados/services/vigia_fio.py:748` |
| **F2 — aberta → fechada `desligamento`** | Mesmo `fechar:1996`; origem `app/colaboradores/services/desligamento.py:167`. | Exceção nomeada, autor e recusa de resposta sem veredito. Vivas são encaminhadas a `fechar_por_admin`, com ato `silencio`, antes do fechamento. `app/chamados/models.py:1950`; `app/chamados/models.py:1982`; `app/chamados/models.py:1988` | APP, ESTADO/C6, signal do pai. |
| **F3 — aberta → fechada `admin_encerra`** | `app/chamados/models.py::DisputaSupervisao.admin_encerra:2014` → `fechar:1996`. | Motivo escrito obrigatório; mantém guardas de autor/resposta sem veredito; trata vivas no mesmo ramo de F2. `app/chamados/models.py:2011`; `app/chamados/models.py:1950`; `app/chamados/models.py:1982` | APP, ESTADO/C6, signal do pai. |
| **F4 — aberta → fechada `competencia_trancada`** | Mesmo `fechar:1996`; origem `app/chamados/services/competencia_trancada.py::encerrar:229`. | Chamador exige `pode_fechar`; porta exige autor e ausência de resposta sem veredito; esse motivo não entra no carimbo das mudas. `app/chamados/services/competencia_trancada.py:227`; `app/chamados/models.py:1950`; `app/chamados/models.py:1982` | APP, RAIZ; C6 lê a tranca. `app/chamados/juizes.py:1405` |
| **R1 — fechada → aberta, admin** | `app/chamados/models.py::DisputaSupervisao.reabrir_admin:2213`; save `app/chamados/models.py:2225`. | Fechada, contador menor que dois, autor; casca exige perfil, motivo válido e justificativa de ao menos 20 caracteres. `app/chamados/models.py:2202`; `app/chamados/models.py:2204`; `app/chamados/models.py:2206`; `app/chamados/services/acoes_disputa.py:245`; `app/chamados/services/acoes_disputa.py:253` | APP; procura de aberta pelo emissor. `app/chamados/services/disputa_emissao.py:531` |
| **R2 — fechada → aberta, sistema** | `app/chamados/models.py::DisputaSupervisao.reabrir_sistema:2237`; save `app/chamados/models.py:2241`. | Só exige fechada; aberta retorna `False`; não incrementa contador. `app/chamados/models.py:2231`; `app/chamados/models.py:2241` | APP, RAIZ. |
| **R3 — aberta/corrigível → aberta/questionário pendente** | `app/chamados/models.py::DisputaSupervisao.reabrir_por_colab:2301`; contador/save `app/chamados/models.py:2303`. | Autor, dona aberta, contador menor que dois e perguntas corrigíveis; delega limpeza à correção. `app/chamados/models.py:2282`; `app/chamados/models.py:2288`; `app/chamados/models.py:2292`; `app/chamados/models.py:2301` | APP, VALIDAÇÃO. |
| **M1 — aberta A → aberta A−motivo; último motivo encaminha F1** | `app/chamados/models.py::DisputaSupervisao.remover_motivo:2127`; save `app/chamados/models.py:2129`; fecho `app/chamados/models.py:2136`. | Autor; releitura sob trava; dona aberta; motivo presente. Conserva perguntas com retratação e via de identidade. `app/chamados/models.py:2095`; `app/chamados/models.py:2100`; `app/chamados/models.py:2120`; `app/chamados/models.py:2125` | ESTADO/C6, gerador e relatório. |
| **M2 — com motivo de foto → sem esse motivo** | `app/chamados/models.py::DisputaSupervisao.aparar_motivo_respondido:2032`. | Motivo presente; remove motivo/contexto. Fechamento é outro ato do chamador. `app/chamados/models.py:2025`; `app/chamados/models.py:2029`; `app/api/foto_recebida.py:80` | Relatório e fechamento. |
| **Aberta/resolução residual → aberta/`resolucao=''`** | `app/chamados/models.py::DisputaSupervisao.limpar_resolucao_residual:2329`; save `app/chamados/models.py:2330`. | Aberta e texto não vazio. `app/chamados/models.py:2326` | Signal do fechamento. `app/chamados/signals.py:46` |
| **Contexto anterior → contexto anotado/renovado; abertura preservada** | `app/chamados/models.py::DisputaSupervisao.anotar_contexto:2169`; `app/chamados/models.py::DisputaSupervisao.renovar_contexto:2187`. | Anotar exige chave sem valor; renovar exige valor diferente. `app/chamados/models.py:2164`; `app/chamados/models.py:2182` | Censo completo dos leitores por chave: [nao verificado]. |
| **Contexto → contexto com dia de entrada pendente** | `app/chamados/services/disputa_emissao.py::perguntar_entradas_pendentes:296`. | Classificador retornou motivo e a geração criou pergunta. `app/chamados/services/disputa_emissao.py:288`; `app/chamados/services/disputa_emissao.py:293`; `app/chamados/services/disputa_emissao.py:295` | Gerador da própria operação. `app/chamados/services/disputa_emissao.py:294` |

**Transições da pergunta**

| Transição | Escritor — arquivo::função:linha | Guarda conferida | Quem lê |
|---|---|---|---|
| **P1 — inexistente → pergunta** | `app/chamados/models.py::PerguntaDisputa._nascer:3095`; insert `app/chamados/models.py:3099`. | Contrato de nascimento, dia do turno, identidade/dedup, destino válido e par resposta/instante permitido. `app/chamados/models.py:2917`; `app/chamados/models.py:2939`; `app/chamados/models.py:2964`; `app/chamados/models.py:2983` | ESTADO/C6, APP. |
| **P2 — canônica → mesma pergunta, eventualmente outra célula/destino** | Mesma `_nascer`: destino `app/chamados/models.py:3065`; save `app/chamados/models.py:3089`. | Canônica do fato; destino válido; costura não mata fio. Conserva a dona encontrada. `app/chamados/models.py:3004`; `app/chamados/models.py:3059`; `app/chamados/models.py:3063` | ESTADO/C6, APP, LAVRA. |
| **P3 — pendente/respondida/validada → nova resposta** | `app/chamados/services/disputa_emissao.py::responder_pergunta_disputa_core:1467`; save `app/chamados/services/disputa_emissao.py:1508`. | Texto/formato/opções por tipo; limpa validação anterior e pode desfazer carimbo sem lastro; registra resposta. `app/chamados/services/disputa_emissao.py:1432`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1485`; `app/chamados/services/disputa_emissao.py:1511` | ESTADO/C6, VALIDAÇÃO, HISTÓRICO. |
| **P4 — sem texto → resposta automática** | `app/chamados/services/disputa_emissao.py::responder_automaticamente:1662`. | Não sobrescreve texto existente; recusa par resposta/instante ilegal; registra resposta. `app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1657`; `app/chamados/services/disputa_emissao.py:1663` | ESTADO/C6, VALIDAÇÃO, HISTÓRICO. |
| **P4 — texto sem instante → resposta com instante** | `app/chamados/services/disputa_emissao.py::carimbar_resposta_pela_trilha:1683`. | Exige texto e instante ausente; registra ato. `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1684` | Mesmos leitores anteriores. |
| **P5 — respondida → validada; com carimbo, resolvida** | `app/chamados/services/validacao.py::_carimbar_validacao:159`; save `app/chamados/services/validacao.py:162`. | Chamador exige resposta, ausência de validação, plausibilidade e tratamento de folga; segue materialização. `app/chamados/services/validacao.py:418`; `app/chamados/services/validacao.py:421`; `app/chamados/services/validacao.py:429`; `app/chamados/services/validacao.py:437`; `app/chamados/services/validacao.py:487` | ESTADO/C6, MATERIALIZAÇÃO, HISTÓRICO. |
| **P6 — validação recusada → respondida aguardando admin** | `app/chamados/services/validacao.py::devolver_validacao_recusada:224`; save `app/chamados/services/validacao.py:227`. | Resposta de hora, sem materialização, recusa com motivo; conserva resposta. Pode chamar R2, condicionado a `reabrir`. `app/chamados/services/validacao.py:215`; `app/chamados/services/validacao.py:217`; `app/chamados/services/validacao.py:242` | ESTADO/C6, VALIDAÇÃO, HISTÓRICO. |
| **P7 — respondida não validada → pendente, recusa humana** | Motivo: `app/chamados/services/validacao.py::recusar_pergunta:625`; ciclo delegado à correção em `app/chamados/services/validacao.py:628`. | Motivo obrigatório; recusa validada e fora do fato; pendente é no-op. `app/chamados/services/validacao.py:591`; `app/chamados/services/validacao.py:602`; `app/chamados/services/validacao.py:605`; `app/chamados/services/validacao.py:609` | APP, ESTADO/C6, HISTÓRICO. |
| **P8 — respondida/validada/carimbada → pendente, correção** | `app/chamados/services/disputa_emissao.py::reabrir_para_correcao:1634` delega a `app/chamados/models.py::PerguntaDisputa.descarimbar_em_lote:3363`. | Limpa resposta, validação **e carimbo**; exclui fora do fato; trilha só para linhas alteradas do fato. `app/chamados/services/disputa_emissao.py:1595`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/models.py:3343`; `app/chamados/models.py:3363` | APP, ESTADO/C6, HISTÓRICO. |
| **P9 — sem carimbo → carimbo de fato** | `app/chamados/models.py::PerguntaDisputa.carimbar:3235`; atribuição central `app/chamados/models.py:3180`. | Recusa via de identidade/fora do fato; aplica pós-estado e exige C6 morto; restaura memória na recusa. Pode completar resposta/validação com texto. `app/chamados/models.py:3201`; `app/chamados/models.py:3215`; `app/chamados/models.py:3222`; `app/chamados/models.py:3227` | ESTADO/C6, LAVRA. |
| **P10 — pergunta → carimbo de identidade** | `app/chamados/models.py::PerguntaDisputa.carimbar_identidade:3273`. | Via pertencente à lista de identidade; fora do fato não retorna ao fato. Não consulta C6 como guarda dessa porta. `app/chamados/models.py:3261`; `app/chamados/models.py:3263` | ESTADO/C6, identidade/dedup. |
| **P11 — carimbada → sem carimbo** | `app/chamados/models.py::PerguntaDisputa.descarimbar:3295`; lote `app/chamados/models.py::PerguntaDisputa.descarimbar_em_lote:3363`. | Individual veta fora do fato; lote também exclui essas linhas, exige campos permitidos e protege par resposta/instante. `app/chamados/models.py:3289`; `app/chamados/models.py:3338`; `app/chamados/models.py:3343`; `app/chamados/models.py:3358` | ESTADO/C6, APP. |
| **P12 — carimbo de outra via → `fato_materializado`** | `app/chamados/models.py::PerguntaDisputa.encerrar_por_fato_no_espelho:3375`. | Chamador prova marco no espelho; porta veta via já igual ou fora do fato. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; `app/chamados/models.py:3372` | ESTADO/C6, lastro. `app/chamados/juizes.py:689` |
| **P13 — via derivada antiga → via atual + lavra** | `app/chamados/services/relavra_via.py::relavrar:127`. | Via derivada; calcula `via_de_hoje`; só escreve se mudou. `app/chamados/services/relavra_via.py:118`; `app/chamados/services/relavra_via.py:120`; `app/chamados/services/relavra_via.py:121` | ESTADO/C6, LAVRA. |
| **V1 — lavra vazia/antiga → `sim`/`nao`/`indeterminado`** | `app/chamados/veredito_lavrado.py::lavrar:270`; save `app/chamados/veredito_lavrado.py:275`. | Juiz do lastro decide; exceção produz `indeterminado`; grava também relógio/janela. `app/chamados/veredito_lavrado.py:262`; `app/chamados/veredito_lavrado.py:269`; `app/chamados/veredito_lavrado.py:272` | LAVRA. |
| **V2 — lavra divergente → lavra de backfill** | Cálculo/atribuições: `app/chamados/management/commands/backfill_veredito_lavrado.py::Command.handle:138`; persistência `app/chamados/veredito_lavrado.py::gravar_lavras_em_lote:250`. | Divergência de veredito; não substitui preenchido por indeterminado decorrente de falha. `app/chamados/management/commands/backfill_veredito_lavrado.py:120`; `app/chamados/management/commands/backfill_veredito_lavrado.py:124` | LAVRA. |
| **T1 — ato inexistente → nova linha de decisão** | `app/chamados/models.py::DecisaoPergunta.registrar:4096`. | Lista vazia não escreve; linha existente não admite edição/apagamento. `app/chamados/models.py:4090`; `app/chamados/models.py:4047`; `app/chamados/models.py:4051` | HISTÓRICO. |
| **I1 — identidade histórica vazia → preenchida** | `app/chamados/migrations/0051_decisao_pergunta.py::_preencher_chave:25`; `app/chamados/migrations/0051_decisao_pergunta.py:27`. | Mapeamento congelado acionado por `RunPython`; separado das portas de produção. `app/chamados/migrations/0051_decisao_pergunta.py:22`; `app/chamados/migrations/0051_decisao_pergunta.py:101` | Identidade/dedup. `app/chamados/models.py:3568` |
| **Destino nulo/anterior → destino recebido, nascimento/reuso** | `app/chamados/models.py::PerguntaDisputa._nascer:3065`; constructor `app/chamados/models.py:3095`. | Destino válido; no reuso, costura não mata fio. `app/chamados/models.py:2964`; `app/chamados/models.py:3063` | ESTADO/C6, materializador, LAVRA. |
| **Destino anterior → novo destino, reaponte** | `app/chamados/services/fio.py::reapontar_resolvedora:964`; save `app/chamados/services/fio.py:966`. | `resolvedora_valida` e `_costura_nao_mata_o_fio`; lavra no mesmo save. `app/chamados/services/fio.py:946`; `app/chamados/services/fio.py:948`; `app/chamados/services/fio.py:965` | Mesmos leitores anteriores. |
| **Destino preenchido → nulo por exclusão do chamado** | Ação referencial `SET_NULL`; sem função escritora Python. `app/chamados/models.py:3494` | Exclusão do chamado referenciado. `app/chamados/models.py:3496` | ESTADO/C6, mater

[truncado em 20000 de 42751 caracteres]

## 15/11 00:49 · sessao 2b5f0dfe31a1 · MADRUGADA-PREPARA · leitura · origem=Code

A incompatibilidade central **SEGUE**: o retro entrega `foto_ausente.batida_id`; o gerador e o derivador de origem procuram `ultima_batida.id`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/services/disputa_emissao.py:387`; `app/chamados/models.py:3606`)

O vínculo Git desta cópia ao hash solicitado não pôde ser confirmado: `git rev-parse HEAD` não reconheceu repositório. As referências abaixo são das linhas lidas no diretório fornecido. **[nao verificado quanto à identidade Git e à comparação histórica entre commits]** [nao verificado]

## RECONFERÊNCIA DAS CITAÇÕES

As ocorrências repetidas entre cef, matriz e r.r estão reunidas nas tabelas. **MUDOU** indica referência ou enunciado que precisa de atualização; não certifica quando ocorreu a alteração. **[nao verificado quanto ao histórico]**

### Referências que MUDARAM

| Citação dos anexos | Referência atual | Conferência |
|---|---|---|
| `models.py:2437` — r.r | `app/chamados/models.py:2514` | Contrato da coluna de episódio BATIDA; campo em `app/chamados/models.py:2518`. |
| `models.py:2784` — r.r | `app/chamados/models.py:2861` | Pergunta datada recebe dia do turno; conversa tem porta própria. |
| `models.py:2795`, `:2802` — r.r | `app/chamados/models.py:2872`; `app/chamados/models.py:2879` | `abrir_conversa`; grava dia civil local do pedido. |
| `models.py:2850-2859` — matriz | `app/chamados/models.py:2872` | Nascimento da conversa; `conversa=True` e `localdate()` em `app/chamados/models.py:2879`. |
| `models.py:2867` — r.r | `app/chamados/models.py:2949` | `_nascer` recebe ou deriva episódio antes de perguntar o dia. |
| `models.py:2876` — r.r | `app/chamados/models.py:2958` | Consulta `dia_do_turno_da_linha`, passando o dia pedido como insumo. |
| `models.py:2965-2976` — matriz | `app/chamados/models.py:2992` | A recusa citada é de episódio já representado por linha recusada em outro dia; não é recusa genérica de qualquer dia pedido divergente. O nascimento toma o dia do juiz. (`app/chamados/models.py:2938`) |
| `models.py:2982` — matriz | `app/chamados/models.py:3009` | Recusa de episódio retratado antes do reuso. |
| `models.py:3450` — r.r | `app/chamados/models.py:3592` | `_chave_de_identidade` deriva origem quando não é conversa e a origem está ausente. |
| `models.py:3453` — r.r | `app/chamados/models.py:3595` | Definição de `batida_origem_do_nascimento`. |
| `models.py:3457` — cef/r.r | `app/chamados/models.py:3599` | Advertência sobre contexto fundido; ele pode apontar outra anomalia e não recompõe a origem do acervo. |
| `models.py:3462-3466`, particularmente `:3464` — cef/r.r | `app/chamados/models.py:3603`; `app/chamados/models.py:3606` | Família BATIDA e leitura de `ultima_batida.id`. |
| `models.py:3468` — r.r | `app/chamados/models.py:3610` | Exige batida pertencente ao colaborador da disputa. |
| `models.py:3490` — cef/r.r | `app/chamados/models.py:3631` | Índice por episódio e motivo; parcial, condicionado a horizonte, origem preenchida, ausência de conversa e vias admitidas. |
| `models.py:3508-3523` — matriz | `app/chamados/models.py:3595` | Derivador completo: família, envelope, existência e posse da batida. |
| `models.py:3537-3550` — matriz | `app/chamados/models.py:3624` | Índices por família; condição do episódio em `app/chamados/models.py:3633`. |
| `models.py:3604` — r.r | `app/chamados/models.py:3746` | Literal do catálogo legado; não é o envelope da disputa. |
| `models.py:4094` — r.r | `app/chamados/models.py:4236` | Reexportação da porta e do gerador. |
| `core/juizes.py:208` — r.r | `app/core/juizes.py:212` | Registro de `turnos_do_colab`; a antiga linha aponta hoje outro registro. |
| `core/juizes.py:1199` — r.r | `app/core/juizes.py:1205` | Registro de `selfie_ausente`. |
| `docs/CORTES.md:283` — r.r | `app/docs/CORTES.md:334` | Corte integral da selfie; também resumido em `app/docs/CORTES.md:94`. |
| `docs/LEIS.md:152` como L-114 — r.r | `app/docs/LEIS.md:153` | Certificado, impacto e termômetro. |
| `docs/LEIS.md:153` como L-113 — r.r | `app/docs/LEIS.md:154` | Nascimento permitido em exportada; regime de alteração do gravado continua distinto. |
| `BACKLOG.md:2109` — matriz | `app/docs/BACKLOG.md:2166` | Mesmo desencontro entre envelope emitido e consumido; texto atualizado com aval posterior. |

### Referências que SEGUEM

Em cada linha abaixo, as referências dos anexos permanecem no mesmo arquivo, linha e significado indicado. As referências relativas da matriz foram expandidas para `app/`. (`app/config/crons.py:686`; `app/chamados/services/disputa_emissao.py:300`)

| Arquivo e citações conferidas — SEGUE | Significado conservado |
|---|---|
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:45`, `:65`, `:66-69` | Janela desde agora, padrão de três dias de duração, bordas inclusivas, origem de aparelho e exclusão de retratadas. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:70-77` | Duas fontes de foto; delegação ao juiz; minutos reais desde a batida. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:81`, `:89-92` | Cabeçalho temporal e retorno anterior à emissão quando não há `--apply`. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:95-106`, especialmente `:96`, `:98-102` | Emissão com `foto_ausente`, sem `ultima_batida`; PK, tipo, instante local do fato e proveniência. |
| `app/ponto/registro_batida.py:72`, `:75-77`, `:80-91` | PWA zero; APK/NULL noventa minutos; override preserva PWA; comparação `>=`. A lista de origens começa em `app/ponto/registro_batida.py:71`. |
| `app/config/crons.py:686` | Cron `26 7 * * *`, retro com `--apply`. |
| `app/api/views.py:447-453`, `:507`, `:655` | Juiz da selfie no ato; obtenção de instante efetivo; entrega desse instante à porta da batida. |
| `app/api/views.py:758-766` | Envelope com PK, tipo e `timestamp=agora`; chamada da porta. |
| `app/api/views.py:833-859` | Upload: definição em `app/api/views.py:835`, anexação em `:847`, resolução em `:857`, resposta em `:859`. O intervalo citado continua correto. |
| `app/api/views_core.py:439`, `:752-758` | Mesmo formato do envelope na API gêmea. |
| `app/ponto/services/triagem_batida.py:735`, `:739` | Emissão de `saida_sem_entrada`; dia do juiz em `app/ponto/services/triagem_batida.py:740`. |
| `app/chamados/services/fabrica_da_celula.py:71`, `:139`, `:172`, `:174` | Motivos pelos marcos da ata; transporte de dia; chamadas da porta e do gerador. |
| `app/chamados/services/declaracao_texto.py:42` | Tabela de motivo por posição e tipo de marco. |
| `app/chamados/management/commands/smoke_chamados.py:103` | Chamada direta do gerador. |
| `app/chamados/services/disputa_emissao.py:72`, `:294`, `:300`, `:325-329` | Referência de batida para grade; chamada e definição do gerador; dedup de topo quando falta episódio. |
| `app/chamados/services/disputa_emissao.py:371-377`, `:387-392` | Primeiro lê `motivo.data`; depois procura episódio e pergunta seu dia ao juiz. |
| `app/chamados/services/disputa_emissao.py:436-441`, `:470-471` | Dedup sequencial de conversa, nascimento e retorno das perguntas. |
| `app/chamados/services/disputa_emissao.py:535-566`, especialmente `:548`, `:552-558`, `:560-565` | Agregação do contexto, contador, instante do merge, substituição das demais chaves de topo e persistência. |
| `app/chamados/services/disputa_emissao.py:568-603`, especialmente `:568`, `:587-592`, `:597` | Nascimento do chamado e da disputa; contexto inicial e trilha. |
| `app/chamados/services/disputa_emissao.py:634-688`, especialmente `:659`, `:664`, `:670`, `:682` | Porta pública, motivos sem repetição, descrição de origem e chamadas do gerador. |
| `app/api/foto_recebida.py:28-54`, `:57-85` | Resposta automática pela porta, validação pelo dono, seleção por última batida e tratamento coletivo das perguntas. |
| `app/chamados/juizes.py:138`, `:141-168`, `:226` | Último degrau temporal, compartilhado entre singular e lote. A entidade de entrada precisa da correção descrita abaixo. |
| `app/ponto/turnos.py:2927-2936`, `:2955` | Dia do turno da batida; não reclamada usa dia local; retratada fica fora do universo. |
| `app/chamados/catalogo/motivos.py:16`, `:35`, `:43`, `:53` | Identidade BATIDA por episódio; motivo da triagem MARCO; família pelo catálogo. |
| `app/chamados/catalogo/query.py:45-66`; `app/chamados/catalogo/perguntas.py:93-98` | Interpolação de data quando o texto a pede; texto de foto permanece escolha sem `{data}`. |
| `app/chamados/tests/test_e1_v5_familia.py:25`, `:98` | Selo da declaração de família e das chaves dos índices; não prende o envelope dos emissores. |
| `CLAUDE.md:142`, `:143`, `:146`, `:148`, `:178`, `:179`, `:411` | Zonas invioláveis, fuso, previsto, idempotência, separação dos donos e regime do gravado exportado. |
| `app/docs/LEIS.md:107`, `:136`, `:145` | Fuso; casos pela regra antes do código; horizonte e backfill único. |
| `app/docs/JUIZES.md:14`, `:20`, `:38` | Prevalência do vivo, juiz do turno e dono da validação. |
| `app/colaboradores/services/ficha.py:215`; `app/escala/services/furos_diarios.py:281`; `app/relatorios/views.py:1675` | Homônimos de ficha/relatório, separados do envelope da disputa; a ficha tem escritor em `app/colaboradores/services/ficha.py:147`. |

### Testes citados: símbolo SEGUE, alcance conferido [nao verificado]

Os métodos citados existem nos sítios abaixo. Execução e resultado dos testes: **[nao verificado]**. [nao verificado]

| Citação da matriz/preparo | Local atual e alcance lido |
|---|---|
| A1 — dry-run | `app/ponto/tests/test_bug126_selfie_invisivel.py:103`: verifica ausência de disputa, não ausência de toda escrita possível. |
| A3 — origem retroativa | `app/ponto/tests/test_bug126_selfie_invisivel.py:91`: exclui origem retroativa; não demonstra o caso `retratada_em`. |
| A4 — manual e origem do juiz | `app/ponto/tests/test_bug126_selfie_invisivel.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:45`. |
| A5 — FotoBatida | `app/ponto/tests/test_bug126_selfie_invisivel.py:74`; `app/ponto/tests/test_selfie_ausente_juiz.py:103`. |
| A6 — Batida.foto | `app/ponto/tests/test_selfie_ausente_juiz.py:112`. |
| A7 — prazo e canal NULL | `app/ponto/tests/test_bug126_selfie_invisivel.py:84`; `app/ponto/tests/test_selfie_ausente_juiz.py:41`. |
| A8 — retro elegível | `app/ponto/tests/test_bug126_selfie_invisivel.py:65`: confere `batida_id`/`_por` em `app/ponto/tests/test_bug126_selfie_invisivel.py:71`; não confere pergunta ou episódio. |
| A9 — agregação sobre PWA | `app/ponto/tests/test_selfie_ausente_juiz.py:119`: confere disputas; payload sintético também sem `ultima_batida` em `app/ponto/tests/test_selfie_ausente_juiz.py:122`. |
| A10 — repetição | `app/ponto/tests/test_bug126_selfie_invisivel.py:108`: asserção conta disputas em `app/ponto/tests/test_bug126_selfie_invisivel.py:113`. |
| A12 — API PWA | `app/api/tests/test_api_bater_pwa.py:64`, `:78`, `:95`: motivo, contador e foto; não identidade da pergunta. |
| A13 — prazo delegado | `app/ponto/tests/test_selfie_ausente_juiz.py:82`. |
| B1 — conversa sem envelope | `app/chamados/tests/test_a2_data_evento_batida.py:53`: motivo diferente de foto; data civil conferida em `app/chamados/tests/test_a2_data_evento_batida.py:56`. |
| B4 — corrida no índice | `app/chamados/tests/test_e1_v5_indice.py:69`: inserção crua por episódio, usando GPS. |
| B5 — episódios distintos e repetição | `app/chamados/tests/test_e1_v5_indice.py:84`; `app/chamados/tests/test_a2_data_evento_batida.py:22`. |
| B6 — segunda batida | `app/chamados/tests/test_e1_v6_horizonte.py:262`: contexto sintético; não executa o retro. |
| B9 — texto de foto sem data | `app/chamados/tests/test_cenarios_disputa.py:43`. |
| B10 — conversa fora do índice | `app/chamados/tests/test_e1_v5_indice.py:96`: permissividade do índice, não dedup sequencial do gerador. |
| C1/C4 — upload | `app/chamados/tests/test_b8_smoke_portas.py:60`: confere Batida.foto e FotoBatida em `app/chamados/tests/test_b8_smoke_portas.py:68`; não confere fecho. |
| C2 — aparar motivo | `app/chamados/tests/test_porta_chamado_e3.py:56`, `:58`, `:62`: porta direta e repetição; não upload. |
| D2 — selo da selfie | `app/ponto/tests/test_selfie_ausente_juiz.py:130`: AST nas APIs; não demonstra contrato de episódio do retro. |

“SEM PROVA” não foi convertido em inexistência universal de teste: cobertura indireta ou dinâmica de todos esses cenários permanece **[nao verificado]**. Há teste de conversa com episódio retratado usando GPS, sem demonstrar especificamente foto. (`app/chamados/tests/test_e1_v6_horizonte.py:445`)

### SUMIU / fontes ausentes

Nenhuma das funções citadas acima SUMIU; seus sítios atuais constam das tabelas, incluindo `app/chamados/models.py:3595`, `app/chamados/services/disputa_emissao.py:300` e `app/chamados/services/disputa_emissao.py:634`.

**SUMIU da cópia disponível — existência histórica [nao verificado]:** o caminho fonte `logs/PREPARACAO/censos/CONTRATO-DE-ENTRADA-DO-FIO/r.r`. Os textos cef/matriz/q.q/r.r/aval foram conferidos pelo envelope, sem arquivo local numerado. **[nao verificado quanto à proveniência material desses anexos]**

`backlog_4obras.txt` e `arquivos_em_voo.txt`, referidos em q.q/r.r, não estão disponíveis no envelope nem como arquivos desta cópia. O cruzamento histórico de raias e a conclusão “arquivos em comum = 0” ficam **[nao verificado]**.

### Correções de significado da matriz

| Trecho | Estado da afirmação |
|---|---|
| A1–A7, A12–A13 | **SEGUE** o comportamento de entrada descrito; os limites das provas estão na tabela de testes. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:66`; `app/ponto/registro_batida.py:80`; `app/api/views.py:447`) |
| A8–A9 | **SEGUE** o envelope incompatível e a preservação de chave antiga ausente na emissão nova. O resultado concreto em produção não está demonstrado pelo envelope. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:548`) |
| A10 | **MUDOU o alcance da conclusão:** não duplicar disputa não prova “mesmo estado”; o merge incrementa contador e troca instante. Os campos estão dentro de `contexto[motivo]`, não em colunas próprias da disputa. (`app/chamados/services/disputa_emissao.py:552`; `app/chamados/services/disputa_emissao.py:558`; `app/ponto/tests/test_bug126_selfie_invisivel.py:113`) |
| A11 | **MUDOU o alcance:** a porta procura disputa aberta; a garantia do índice de pergunta é parcial e não autoriza enunciado irrestrito para todo acervo. (`app/chamados/services/disputa_emissao.py:531`; `app/chamados/models.py:3633`) |
| B1–B2 | **SEGUE** a conversa quando falta dia e o dedup de topo quando falta episódio. (`app/chamados/services/disputa_emissao.py:325`; `app/chamados/services/disputa_emissao.py:431`) |
| B3 | **SEGUE** a possibilidade de consumir episódio antigo; “nasce uma pergunta errada” não é resultado inevitável, pois a porta também pode reutilizar linha. (`app/chamados/services/disputa_emissao.py:387`; `app/chamados/models.py:2990`; `app/chamados/models.py:3091`) |
| B4–B6 | **SEGUE**, respeitadas as condições do índice e o domínio do fato. (`app/chamados/models.py:2795`; `app/chamados/models.py:3631`) |
| B7 | **SEGUE com ramos separados:** gerador sem dia pode produzir conversa; porta datada com origem retratada recusa. (`app/chamados/services/disputa_emissao.py:431`; `app/chamados/services/fio.py:1685`) |
| B8 | **MUDOU a explicação:** dia pedido diferente não provoca, por si, a recusa citada; o episódio determina o dia pelo juiz. (`app/chamados/models.py:2938`; `app/chamados/models.py:2992`) |
| B9–B10 | **SEGUE** o texto sem data e o dedup sequencial de conversa; mudar o texto continua pergunta separada. (`app/chamados/catalogo/perguntas.py:93`; `app/chamados/services/disputa_emissao.py:436`) |
| C1–C4 | **SEGUE** o código de seleção por disputa/última batida e validação coletiva. Há corte escrito posterior específico para o alcance por episódio. (`app/api/foto_recebida.py:67`; `app/api/foto_recebida.py:73`; `app/api/foto_recebida.py:79`; `app/docs/CORTES.md:441`) |
| D1 | **MUDOU a entidade descrita:** o contexto fraco vem do chamado, não diretamente da disputa. O nascimento recebe cópias separadas; o merge da disputa não atualiza o chamado nesse ramo. (`app/chamados/juizes.py:100`; `app/chamados/juizes.py:138`; `app/chamados/services/disputa_emissao.py:580`; `app/chamados/services/disputa_emissao.py:590`; `app/chamados/services/disputa_emissao.py:562`) |
| D2 | **SEGUE** o alcance limitado do selo existente. (`app/chamados/tests/test_e1_v5_familia.py:25`; `app/chamados/tests/test_e1_v5_familia.py:98`) |
| D3 | Os números e a distribuição do passivo continuam apenas como dado anexado; reconstrução individual e alcance da recuperação **[nao verificado]**. A restrição do contexto fundido permanece escrita em `app/chamados/models.py:3599`. |

## BRIEF ATUALIZADO — CONTRATO-DE-ENTRADA-DO-FIO

1. **Objeto da leitura:** contrato entre emissão retro, disputa e nascimento da pergunta; origem em `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`.
2. **Desencontro confirmado:** retro grava PK em `foto_ausente.batida_id`; origem e dia procuram `ultima_batida.id`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/models.py:3606`; `app/chamados/services/disputa_emissao.py:387`)
3. **Dono:** a classificação ESTRUTURA do brief anterior era inferência; permanece PERGUNTA separada, sob a separação escrita dos donos. (`CLAUDE.md:179`)
4. **Regra de preparação:** casos vêm da regra antes do código; escolha sem resposta escrita permanece pergunta. (`app/docs/LEIS.md:136`)
5. **Identidade escrita:** `foto_ausente` pertence à família BATIDA, cuja chave é episódio e motivo. (`app/chamados/catalogo/motivos.py:16`; `app/chamados/catalogo/motivos.py:43`)
6. **Dia escrito:** o episódio consulta o juiz do turno; uma batida não reclamada usa seu dia local, não uma conversão livre do envelope. (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2955`)
7. **Repetição:** a porta de pergunta e o índice tratam identidade por episódio; o contador de emissões da disputa continua diferente dessa identidade. (`app/chamados/models.py:2795`; `app/chamados/models.py:3631`; `app/chamados/services/disputa_emissao.py:552`)
8. **Contexto acumulado:** o próprio derivador declara que contexto fundido pode apontar outra anomalia e não recupera a origem histórica do acervo. (`app/chamados/models.py:3599`)
9. **Tempo:** instante efetivo da batida, recepção e dia do turno são insumos distintos; o envelope atual da API contém recepção. (`app/api/views.py:507`; `app/api/views.py:655`; `app/api/views.py:761`; `app/ponto/turnos.py:2928`)
10. **Selos existentes:** família/índice e delegação à selfie estão cobertos pelos selos citados; a obrigação e o alcance de selo de envelope ficam como PERGUNTA. (`app/chamados/tests/test_e1_v5_familia.py:25`; `app/chamados/tests/test_e1_v5_familia.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:130`)
11. **Outros consumidores:** upload seleciona disputa por `ultima_batida.id`; dia fraco lê o contexto do chamado e extrai dia civil do ISO. (`app/api/foto_recebida.py:67`; `app/chamados/juizes.py:100`; `app/chamados/juizes.py:166`)
12. **Restrições escritas:** motor exige aval; escrita de batida tem porta; previsto tem fonte única. (`CLAUDE.md:142`; `CLAUDE.md:143`; `CLAUDE.md:148`)
13. **Exportada:** nascimento e alteração do gravado têm regimes distintos; não há autorização de alteração genérica do fechamento nesta leitura. (`app/docs/LEIS.md:154`; `CLAUDE.md:411`)
14. **Passivo:** 135/15/44/76 são números do anexo; critério de classificação, contaminação por contexto e recuperação individual **[nao verificado]**. O uso posterior do contexto fundido tem a limitação escrita em `app/chamados/models.py:3599`.
15. **E3 no nascimento:** existe recusa de resposta carimbada sem texto antes das travas e também no `save`; dia e projeção de decisão passam pelos pontos atuais de escrita. (`app/chamados/models.py:2919`; `app/ch

[truncado em 20000 de 36567 caracteres]

## 15/11 00:49 · sessao a309270b1b28 · MADRUGADA-PREPARA · sonda · origem=Code

A quantidade atual de vínculos inválidos e o efeito de um saneamento permanecem **[nao verificado]**. A cópia já contém a constraint no modelo e uma migration com `CHECK ... NOT VALID`; sua aplicação no banco é **[nao verificado]**. (`app/escala/models.py:1037`, `app/escala/migrations/0041_ec_vigencia_not_valid.py:43`.)

Os escritores operacionais das datas são:

| Escritor | Escrita |
|---|---|
| `executar_vinculo` | Fecha pela porta; reabre com fim nulo; cria com início informado. `app/colaboradores/services/vinculo.py:214`, `app/colaboradores/services/vinculo.py:241`, `app/colaboradores/services/vinculo.py:277`, `app/colaboradores/services/vinculo.py:283`, `app/colaboradores/services/vinculo.py:287`, `app/colaboradores/services/vinculo.py:291`. |
| `fechar_vigencia` | Atualiza fim e desativa, somente onde início ≤ fim informado. `app/colaboradores/services/vinculo.py:641`. |
| `reabrir_vigencia_impossivel` | Atualiza fim e reativa. `app/colaboradores/services/vinculo.py:730`. |
| `encerrar_vinculo` | Atribui fim e salva. `app/colaboradores/services/vinculo.py:844`, `app/colaboradores/services/vinculo.py:845`. |
| `criar_trecho_retroativo` | Cria com início e fim. `app/colaboradores/services/vinculo.py:970`, `app/colaboradores/services/vinculo.py:972`. |
| `corrigir_vinculo_retroativo` | Atribui fim e salva. `app/colaboradores/services/vinculo.py:982`, `app/colaboradores/services/vinculo.py:983`. |
| `reverter_para_snapshot` | Reescreve início e fim do snapshot. `app/colaboradores/services/vinculo.py:1031`, `app/colaboradores/services/vinculo.py:1032`. |
| `ajustar_vinculo_pelo_sistema` | Fecha pela porta e cria outro início. `app/colaboradores/services/vinculo.py:1087`, `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1090`. |
| `desfazer_ajuste_do_sistema` | Reabre com fim nulo ou fecha pela porta e cria outro início. `app/colaboradores/services/vinculo.py:1106`, `app/colaboradores/services/vinculo.py:1108`, `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1111`. |
| `fechar_vinculos_ativos` | Delega o fechamento dos vínculos `ativa=True`. `app/colaboradores/services/vinculo.py:862`, `app/colaboradores/services/vinculo.py:863`. |
| `gravar_regua` | Aceita `**campos` e faz `update(**campos)`, sem lista restritiva de campos nessa função. Os chamadores encontrados passam campos de régua. `app/colaboradores/services/vinculo.py:1048`, `app/colaboradores/services/vinculo.py:1070`, `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:742`, `app/escala/regua_defesa.py:765`. |

As entradas desses escritores incluem wizard, chat, régua, comando de fase, desvinculação, desligamento, correção retroativa, recusa de aceite, ajuste automático e reparo de vigência. (`app/colaboradores/views.py:423`, `app/colaboradores/services/troca_pelo_chat.py:75`, `app/escala/regua_defesa.py:321`, `app/ponto/management/commands/censo_fase_12x36.py:423`, `app/escala/views.py:269`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`, `app/colaboradores/services/desligamento.py:148`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`, `app/chamados/services/acoes_chamado.py:149`, `app/escala/services/escala_auto_executor.py:103`, `app/escala/services/escala_auto_executor.py:145`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:107`.)

O admin de `EscalaColaborador` usa `SoLeitura`, que recusa adição, alteração e exclusão. Fixtures de testes também criam vínculos; não são entradas operacionais. (`app/escala/admin.py:19`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/tenants/tests.py:44`, `app/colaboradores/tests/test_ficha_do_colab.py:88`.)

Os leitores diretos do intervalo apresentam estes comportamentos:

| Leitor | Tratamento observável de fim < início |
|---|---|
| Carregamento de vínculos do período | Filtra interseção com a janela, sem validar o par início/fim; `mais=` também carrega geradoras fora desse filtro. `app/escala/alimentacao.py:149`, `app/escala/alimentacao.py:151`, `app/escala/alimentacao.py:155`. |
| `escalas_do_periodo` | Delega ao carregador plural. `app/escala/alimentacao.py:185`. |
| `vinculo_cobre_a_data` e versão SQL | Exigem início ≤ dia e fim nulo ou fim ≥ dia; um par invertido não cobre dia algum. `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:223`. |
| `vinculo_do_dia` | Confere limites da geradora; no caminho sem geradora usa `vinculo_cobre_a_data`. O invertido não responde pelo dia. `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`. |
| `previstas_do_periodo` | Recorta a fatia e pula explicitamente fim < início. `app/escala/alimentacao.py:390`, `app/escala/alimentacao.py:392`. |
| Apuração por fatias | Recorta pelos limites do vínculo e pula explicitamente fatia invertida. `app/ponto/services/fechamento.py:206`, `app/ponto/services/fechamento.py:207`, `app/ponto/services/fechamento.py:242`. |
| Censo `vigencias_impossiveis` | Chama o validador real e recolhe as recusas. `app/colaboradores/services/vinculo.py:496`, `app/colaboradores/services/vinculo.py:499`, `app/colaboradores/services/vinculo.py:500`. |
| `vinculos_inconsistentes` | Identifica invertidos e os separa do cálculo de sobreposição. `app/escala/services/vinculos_inconsistentes.py:24`, `app/escala/services/vinculos_inconsistentes.py:30`, `app/escala/services/vinculos_inconsistentes.py:37`. |
| `vigencia_sem_trilha` | Separa o passivo usando `vigencias_impossiveis`. `app/escala/services/vigencia_sem_trilha.py:66`, `app/escala/services/vigencia_sem_trilha.py:77`. |
| Rejulgamento de furos | Usa `data__range` com início e fim recortado; não valida o par nesse trecho. `app/escala/regua_defesa.py:686`. |
| Piso da fase | Consulta cobertura pelo juiz SQL; sem cobertura, usa o menor início, sem consultar fim nessa alternativa. `app/escala/services/fase_na_tela.py:160`, `app/escala/services/fase_na_tela.py:165`. |
| Propositor | Escolhe pelo juiz do dia e depois exclui encerramentos anteriores ao piso de aplicação. `app/escala/services/propositor.py:448`, `app/escala/services/propositor.py:460`. |
| Explicação de cadastro zero no export | Alimenta o previsto por dia; também lê finais para explicar encerramento. `app/folha/export.py:938`, `app/folha/export.py:957`. |
| Signals | Capturam e comparam os pares anterior/atual e calculam janelas de regeneração; esses trechos não validam fim ≥ início. `app/escala/signals.py:25`, `app/escala/signals.py:57`, `app/escala/signals.py:60`, `app/escala/signals.py:68`, `app/escala/signals.py:121`, `app/escala/signals.py:200`. |
| Diagnóstico comparativo | Conserva a seleção antiga como referência de comparação. `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:117`. |
| Gerador de batidas smoke | Usa interseção de datas sem validar o par nesse filtro. `app/ponto/management/commands/gerar_batidas_smoke_motor.py:73`. |

Outros consumidores chegam à cobertura/seleção pelas funções reais acima:

| Família | Sítios |
|---|---|
| Folgas, calendário e jornada | `app/escala/folgas.py:451`, `app/escala/folgas.py:487`, `app/escala/folgas.py:493`, `app/escala/folgas.py:539`, `app/escala/folgas.py:624`, `app/escala/services/semana_do_posto.py:75`, `app/colaboradores/services/calendario.py:505`, `app/escala/servico_jornada.py:63`. |
| Cadastro e ficha | `app/escala/services/cadastro_realidade.py:489`, `app/escala/services/cadastro_realidade.py:509`, `app/escala/services/cadastro_realidade.py:756`, `app/escala/services/cadastro_realidade.py:760`, `app/colaboradores/queries.py:64`, `app/colaboradores/services/ficha.py:129`. |
| Células e regeneração | `app/ponto/management/commands/gerar_celulas.py:283`, `app/ponto/portas/celula.py:229`, `app/ponto/portas/celula.py:339`, `app/ponto/portas/celula.py:906`, `app/ponto/portas/regen_exportada.py:147`, `app/ponto/services/cadastro_futuro.py:70`. |
| Espelho, diagnóstico e PDF | `app/ponto/services/diagnostico_escala.py:53`, `app/ponto/services/espelho.py:286`, `app/ponto/services/espelho.py:1065`, `app/relatorios/pdf_espelho.py:307`, `app/relatorios/pdf_espelho.py:571`. |
| Calculador e turnos | `app/ponto/calculador/alimentacao.py:192`, `app/ponto/turnos.py:1960`, `app/ponto/turnos.py:2166`, `app/ponto/turnos.py:2361`, `app/ponto/turnos.py:2986`, `app/ponto/turnos.py:3058`. |
| Precedência, núcleo e suprajuiz | `app/ponto/precedencia.py:312`, `app/ponto/precedencia.py:323`, `app/ponto/precedencia.py:349`, `app/ponto/supra_juiz.py:174`, `app/ponto/supra_juiz.py:185`, `app/ponto/nucleo.py:15`, `app/ponto/nucleo.py:26`. |
| Folha, feriados e CCT | `app/folha/previa.py:62`, `app/folha/previa.py:81`, `app/folha/export.py:359`, `app/folha/export.py:1252`, `app/core/feriados.py:154`, `app/core/feriados.py:161`, `app/core/regua_cct.py:329`. |
| Chamados e API | `app/chamados/services/limbo_folgas.py:200`, `app/chamados/management/commands/auditar_invariantes_chamados.py:361`, `app/chamados/management/commands/auditar_invariantes_chamados.py:391`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1538`, `app/api/views_mensageria.py:979`. |
| Triagem, cartório e vigias | `app/ponto/services/triagem_batida.py:149`, `app/ponto/services/triagem_batida.py:402`, `app/ponto/services/cartorio.py:1768`, `app/ponto/services/cartorio.py:1903`, `app/ponto/services/vigia_cadastro_futuro.py:58`, `app/ponto/management/commands/vigiar_motor_autoridade.py:51`. |

Há ainda leituras somente do início e consultas somente de vínculos abertos; elas não demonstram uma precondição sobre a ordem das duas datas. (`app/ponto/services/espelho.py:43`, `app/ponto/management/commands/detectar_ausencias.py:177`, `app/ponto/management/commands/detectar_ausencias.py:247`, `app/ponto/management/commands/supra_juiz.py:149`, `app/ponto/management/commands/tripwire_celulas.py:67`, `app/chamados/detector_anomalias.py:58`, `app/escala/services/escala_auto.py:172`, `app/ponto/services/cluster_espurio.py:185`, `app/relatorios/furos.py:61`, `app/chamados/services/disputa_emissao.py:341`.)

Uma suposição adicional de que “fim sempre é maior ou igual ao início”, além dos predicados descritos, é **[nao verificado]**. O censo AST existente também declara limites de detecção; ele não prova ausência universal de acessos dinâmicos. (`app/core/censo_pergunta2.py:10`, `app/core/censo_pergunta2.py:17`.)

Para interpretar a sonda:

- `empN` usa `Colaborador.empresa_id`; a competência respeita o corte dessa empresa e recebe mês/ano pelo fim da janela. (`app/colaboradores/models.py:321`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:60`.)
- “Exportada” significa resposta verdadeira de `competencia_entregue`; “aberta”, neste relatório, significa resposta falsa desse juiz. (`app/folha/export.py:556`, `app/folha/export.py:588`, `app/folha/export.py:594`.)
- `desligado` vem de `esta_desligado`; `situacao` preserva também `afastado` e `ferias`. (`app/core/juizes.py:271`, `app/colaboradores/services/desligamento.py:23`, `app/colaboradores/models.py:295`.)
- `EscalaColaborador` não tem campo de criação/alteração; `regua_avaliada_em` e `regua_corroborada_em` pertencem à régua. (`app/escala/models.py:871`, `app/escala/models.py:884`, `app/escala/models.py:888`, `app/escala/models.py:1399`.)
- As batidas são contadas pelo instante local no intervalo completo, usando `batidas_apuraveis`; sua atribuição a um dia de turno é outra função. (`app/ponto/turnos.py:45`, `app/ponto/turnos.py:2875`, `app/ponto/turnos.py:2886`.)

Segue somente o texto. O envelope aceita `--sombra`, mas também escreve credencial, temporário e trilha; por isso não foi chamado nesta sessão. (`bin/sonda_leitura.sh:67`, `bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:248`.) Execução desta sonda no Django e resultados: **[nao verificado]**.

```python
# Texto para: bin/sonda_leitura.sh --sombra /caminho/sonda_o50.py
# Censo real: app/colaboradores/services/vinculo.py:479
# Cobertura: app/escala/alimentacao.py:205
# Juizes declarados: app/core/juizes.py:271 e :337

import json
import re
from collections import Counter, defaultdict
from contextlib import ExitStack
from datetime import datetime, time, timedelta

from django.conf import settings
from django.db import connections
from django.db.models import Count
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from colaboradores.services.desligamento import esta_desligado
from colaboradores.services.vinculo import vigencias_impossiveis
from escala.alimentacao import vinculo_cobre_a_data
from escala.models import CelulaDia, EscalaColaborador
from folha.export import competencia_entregue
from ponto.janelas import janela_atual
from ponto.turnos import batidas_apuraveis, data_local


def somente_select(execute, sql, params, many, context):
    # Precedente:
    # app/chamados/management/commands/diff_e3_sombra.py:30
    if many or not isinstance(sql, str):
        raise RuntimeError("SQL recusado")

    s = sql.strip()
    if not re.match(r"^SELECT\b", s, re.I):
        raise RuntimeError("SQL recusado")

    # Recusa comentarios, literais SQL e comandos multiplos.
    # Os valores destas consultas seguem nos parametros do ORM.
    if any(x in s for x in (";", "--", "/*", "*/", "'")):
        raise RuntimeError("SQL recusado")

    t = re.sub(r'"(?:[^"]|"")*"', "IDENT", s)
    if re.search(
        r"\b(WITH|INTO|INSERT|UPDATE|DELETE|MERGE|CREATE|ALTER|DROP|"
        r"TRUNCATE|COPY|CALL|DO|SET|RESET|BEGIN|COMMIT|ROLLBACK|" [nao verificado]
        r"SAVEPOINT|RELEASE|GRANT|REVOKE|FOR|LOCK)\b",
        t,
        re.I,
    ):
        raise RuntimeError("SQL recusado")

    # Recusa chamadas SQL fora do necessario para estas consultas.
    permitidos = {
        "COUNT", "IN", "AND", "OR", "NOT", "WHERE",
        "FROM", "SELECT", "EXISTS", "AS",
    }
    chamadas = re.findall(r"([A-Za-z_][A-Za-z_0-9$]*)\s*\(", t)
    if any(nome.upper() not in permitidos for nome in chamadas):
        raise RuntimeError("SQL recusado")

    return execute(sql, params, many, context)


def coletar():
    conn = connections["default"]
    if (
        settings.SETTINGS_MODULE != "config.settings.sombra"
        or conn.settings_dict.get("NAME") != "sombra"
        or conn.settings_dict.get("HOST") != "sombra_db"
        or conn.settings_dict.get("USER") != "leitor"
        or getattr(conn, "schema_name", None) != "juliani"
        or not conn.get_autocommit()
    ):
        raise RuntimeError("Envelope recusado")

    # Sem limitar empresa, ativa, periodo ou quantidade esperada.
    # app/colaboradores/services/vinculo.py:491
    censo = vigencias_impossiveis()
    esperados = {
        pk: (cid, ini, fim, ativa)
        for pk, cid, ini, fim, ativa in censo
    }
    if len(esperados) != len(censo):
        raise RuntimeError("Censo duplicado")

    cids = {cid for _, cid, _, _, _ in censo}
    campos = (
        "pk", "colaborador_id", "data_inicio", "data_fim", "ativa",
    )
    por_colab = defaultdict(list)
    por_pk = {}

    for ec in (
        EscalaColaborador.objects.filter(colaborador_id__in=cids)
        .only(*campos)
        .order_by("pk")
    ):
        por_colab[ec.colaborador_id].append(ec)
        por_pk[ec.pk] = ec

    colabs = {
        c.pk: c
        for c in Colaborador.objects.filter(pk__in=cids).only(
            "pk", "empresa_id", "situacao"
        )
    }
    empresas = {
        e.pk: e
        for e in Empresa.objects.only("pk", "dia_inicio_competencia")
    }

    totais_emp = Counter({eid: 0 for eid in empresas})
    entregue = {}
    linhas = []
    um_dia = timedelta(days=1)
    tz = timezone.get_default_timezone()

    with timezone.override(tz):
        for pk in sorted(esperados):
            ec = por_pk[pk]
            if (
                ec.colaborador_id,
                ec.data_inicio,
                ec.data_fim,
                ec.ativa,
            ) != esperados[pk]:
                raise RuntimeError("Universo mudou durante a leitura")

            c = colabs[ec.colaborador_id]
            empresa = empresas[c.empresa_id]

            # Intervalo solicitado: [data_fim, data_inicio].
            lo, hi = ec.data_fim, ec.data_inicio
            grupos = {}

            def grupo(dia):
                # app/ponto/janelas.py:60
                ji, jf = janela_atual(dia, empresa)
                k = (jf.year, jf.month)
                tripla = (empresa.pk, jf.month, jf.year)

                # app/folha/export.py:556
                if tripla not in entregue:
                    entregue[tripla] = bool(
                        competencia_entregue(*tripla)
                    )

                if k not in grupos:
                    grupos[k] = {
                        "competencia": (
                            f"{jf.year:04d}-{jf.month:02d}"
                        ),
                        "janela": [ji.isoformat(), jf.isoformat()],
                        "entregue": entregue[tripla],
                        "estado": (
                            "exportada"
                            if entregue[tripla]
                            else "aberta"
                        ),
                        "recorte_intervalo": None,
                        "dias_intervalo": 0,
                        "celulas_intervalo": 0,
                        "celulas_depois_inicio": 0,
                        "celulas_intervalo_geradas_por_este": 0,
                        "celulas_depois_inicio_geradas_por_este": 0,
                        "batidas_intervalo": 0,
                        "_dias_celulas_intervalo": set(),
                        "_dias_celulas_depois_inicio": set(),
                    }

                return grupos[k], jf

            # Inclui competencias sem celulas ou batidas.
            d = lo
            while d <= hi:
                g, jf = grupo(d)
                ate = min(hi, jf)
                g["recorte_intervalo"] = [
                    d.isoformat(), ate.isoformat(),
                ]
                g["dias_intervalo"] += (ate - d).days + 1
                if ate == hi:
                    break
                d = ate + um_dia

            # Todas as celulas do colaborador:
            # [lo, hi] e, separadamente, data > hi, sem teto.
            # Proveniencia: app/escala/models.py:1692
            celulas = (
                CelulaDia.objects
                .filter(colaborador_id=c.pk, data__gte=lo)
                .order_by()
                .values("data", "escala_geradora_id")
                .annotate(n=Count("pk"))
            )
            for cel in celulas:
                dia, n = cel["data"], cel["n"]
                g, _ = grupo(dia)
                parte = (
                    "intervalo" if dia <= hi else "depois_inicio"
                )
                g["celulas_" + parte] += n
                g["_dias_celulas_" + parte].add(dia)
                if cel["escala_geradora_id"] == ec.pk:
                    g[
                        "celulas_" + parte + "_geradas_por_este"
                    ] += n

            # Inclui todas as horas do ultimo dia.
            # app/ponto/turnos.py:2875; data_local em :45
            ti = timezone.make_aware(
                datetime.combine(lo, time.min), tz
            )
            tf = timezone.make_aware(
                datetime.combine(hi, time.max), tz
            )
            for ts in batidas_apuraveis(c, ti, tf).values_list(
                "timestamp", flat=True
            ):
                g, _ = grupo(data_local(ts))
                g["batidas_intervalo"] += 1

            # Um outro vinculo individual cobrindo ambas as pontas.
            # app/escala/alimentacao.py:214
            outros = [
                f"#{outro.pk}"
           

[truncado em 20000 de 23309 caracteres]

## 15/11 00:47 · sessao 05c12876a823 · MADRUGADA-PREPARA · sonda · origem=Code

**A18/A19 da matriz são identificáveis; sua equivalência aos “itens 18–19 da E4B” não está demonstrada.** A árvore também usa “18–19” para o fim do programa E4B. O `fatias.md` fornecido descreve leitores; atribuir a projeção legada R/V a uma fatia executora da E4c permanece **[nao verificado]**. (`app/docs/BACKLOG.md:1977`; `app/docs/GRADE-MESTRA.md:717`)

A leitura abaixo usa **A18/A19** e identifica separadamente a operação real **R/V → DecisaoPergunta**. O comando E4B chama fábrica, registro de decisões e materialização; não contém uma aplicação automática de recusa humana A19. Exportadas são recusadas pelo planejamento e novamente antes da execução. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:125`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:260`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:304`)

| Operação | Modelo, campos e escritor na cópia |
|---|---|
| **A18: recusa técnica** | `PerguntaDisputa.recusa_motivo` recebe o motivo técnico truncado em 240 caracteres. Em colisão sem carimbo, delega também `materializada_em`, `materializada_por_id` e `via_resolucao=fato_ocupado`. Escritores: `app/chamados/services/materializacao.py:1096`, `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1108`; executor do carimbo: `app/chamados/models.py:3182`, `app/chamados/models.py:3268`, `app/chamados/models.py:3273`. |
| **A19: recusa humana** | `PerguntaDisputa.recusa_motivo` recebe autor e motivo; a reabertura limpa `respondida_em`, `resposta_colab`, `validada_em`, `validada_por`, `materializada_em`, `materializada_por` e `via_resolucao`. Escritores: `app/chamados/services/validacao.py:619`, `app/chamados/services/validacao.py:628`; operação delegada: `app/chamados/services/disputa_emissao.py:1595`, `app/chamados/services/disputa_emissao.py:1634`; executor: `app/chamados/models.py:3363`. |
| **A19: decisão preservada** | Insere `DecisaoPergunta` com `pergunta_id`, `ato=recusa`, `ato_texto`, `respondida_original_em`, `validada_original_em`, `validada_original_por_id`, `ato_em`, `ato_por`, `ato_motivo`. Produtor: `app/chamados/services/disputa_emissao.py:1617`; executor: `app/chamados/models.py:4096`. |
| **Projeção legada R/V** | Insere DP `resposta`: `pergunta_id`, `ato`, `ato_em=respondida_em`, `ato_texto=resposta_colab`, `ato_motivo`; DP `validacao`: os mesmos campos, com `ato_em=validada_em` e `ato_por_id=validada_por_id`. Produtor: `app/chamados/models.py:4074`, `app/chamados/models.py:4078`; executor: `app/chamados/models.py:4096`. Identificação como E4c: **[nao verificado]**. |
| **Notas e auditoria** | A18 e A19 podem inserir `FollowupChamado`: `chamado_id`, `autor_tipo`, `autor_user`, `conteudo`, `anexo_hash`, e chave de evento quando cabível. A19 também chama os adaptadores de `LogAuditoria`. (`app/chamados/services/materializacao.py:1113`; `app/chamados/services/validacao.py:633`; `app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:101`; `app/chamados/services/validacao.py:637`; `app/chamados/services/validacao.py:643`; `app/core/observ.py:134`) |
| **Eventos decorrentes** | DP registrada e reabertura chamam `acordar_celulas`; A18 com carimbo também pode chegar por signal. A escrita é `EventoCelula(celula_id, colaborador_id, data)`, com os defaults do modelo; localizar a célula apenas a lê. (`app/chamados/models.py:4097`; `app/chamados/services/disputa_emissao.py:1635`; `app/chamados/signals.py:257`; `app/chamados/models.py:3325`; `app/ponto/portas/celula.py:954`; `app/escala/models.py:1860`) |

Isso não fecha todas as cascatas: salvar pergunta agenda um callback de beco; quando elegível, ele resolve perguntas e fecha a disputa. O consumidor da outbox é outra execução. Linhas e efeitos efetivamente atingidos por essas cascatas: **[nao verificado]**. (`app/chamados/signals.py:230`; `app/chamados/signals.py:240`; `app/chamados/services/becos.py:54`; `app/chamados/services/becos.py:57`; `app/chamados/reconciliador.py:1316`)

**Nos leitores examinados do GRAVADO/TXT, não há leitura direta dos campos R/V/C/recusa, das DP, notas ou eventos acima.** A montagem seleciona `FechamentoMensal`, lê seus valores e consulta autoridades de ausência e retenção. Cobertura de todo despacho transitivo/dinâmico: **[nao verificado]**. (`app/folha/export.py:714`; `app/folha/export.py:719`; `app/folha/export.py:730`; `app/folha/export.py:740`; `app/folha/export.py:968`)

| Leitor | O que efetivamente lê |
|---|---|
| **Valores do TXT** | FM: `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_folga_trabalhada`, `horas_noturnas`, `horas_intra_indenizada`, `horas_atraso`, `horas_saida_antecipada`. (`app/folha/export.py:420`; `app/folha/export.py:439`) |
| **Entrada/retenção** | FM e cadastro; a retenção consulta o juiz da célula. (`app/folha/export.py:785`; `app/folha/export.py:831`; `app/folha/export.py:968`; `app/folha/export.py:1306`) |
| **Faltas e rubricas** | Datas de falta vêm da autoridade de ausência; o código da rubrica vem do mapa de rubricas. (`app/folha/export.py:169`; `app/folha/export.py:184`; `app/folha/export.py:735`; `app/folha/export.py:740`; `app/folha/export.py:984`) |
| **TXT já registrado** | `ExportacaoDominio` guarda `conteudo`, `hash_sha256` e `total_linhas`; `registrar` recebe o conteúdo pronto. (`app/folha/models.py:82`; `app/folha/models.py:174`; `app/folha/models.py:182`) |

Portanto, **ausência de escrita direta em horas não certifica dinheiro zero**. `fechamento_lido()` deriva o estado atual sem persistir; `txt_vigente_x_gravado()` compara o TXT vigente com a montagem atual. Nenhuma dessas funções fornece, por si, o estado posterior hipotético de A18/A19. Delta atribuível em minutos ou rubricas: **[nao verificado]**. (`app/ponto/services/fechamento.py:1759`; `app/folha/export.py:655`)

A sonda abaixo usa dois universos distintos, sem somar suas unidades:

- **E4B: dia-colaborador**, `sem_porta` de `_alcance`, mais o complemento declarado de classe e em `[hoje−45, hoje)`. Esse complemento não representa todo o histórico exportado. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:153`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:248`)
- **Projeção R/V: novas linhas DP**, pelo alcance real `linhas_a_criar()`: R ou V presente, horizonte canônico/sem dia ou conversa; conversa e sem dia ficam separados. O comando existente declara que não aplica essa projeção. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:73`)

A18/A19 não expõem alcance planejável em leitura; a sonda conta esses **dois escritores sem alcance chamável**, deixando suas quantidades de linhas e mudanças como `null`, nunca zero. A atribuição da projeção à E4c também fica explícita como não verificada. (`app/chamados/services/materializacao.py:1089`; `app/chamados/services/validacao.py:562`)

Texto para o envelope `bin/sonda_leitura.sh --sombra <sonda.py>`; execução e resultados: **[nao verificado]**. (`bin/sonda_leitura.sh:56`)

```python
# Apenas TEXTO: nao salvar nem executar nesta leitura.
# Envelope: bin/sonda_leitura.sh --sombra <sonda.py>
# RV_DP nomeia a operacao real; atribuicao a E4c nao verificada.

import io
import json
import logging
import re
from collections import Counter
from contextlib import ExitStack, redirect_stdout, redirect_stderr


class SQLRecusado(Exception):
    pass


def so_select(execute, sql, params, many, context):
    texto = sql.strip()
    if (
        many
        or not re.match(r"^SELECT\b", texto, re.I)
        or ";" in texto
        or re.search(
            r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b",
            texto, re.I,
        )
    ):
        raise SQLRecusado()
    return execute(sql, params, many, context)


def medir():
    from django.db import connections
    from django.utils import timezone

    saida = []
    with ExitStack() as pilha:
        for conn in connections.all():
            pilha.enter_context(conn.execute_wrapper(so_select))

        from folha.export import competencia_entregue
        from folha.models import ExportacaoDominio
        from colaboradores.models import Colaborador
        from chamados.management.commands.backfill_fio_morto_e4b import (
            _alcance, _exportadas_classe_e,
        )
        from chamados.management.commands.backfill_decisoes_pergunta import (
            linhas_a_criar,
        )

        agora = timezone.localtime()
        hoje = agora.date()

        # Enumerar triplas nao decide entrega: somente K6 decide.
        # folha/export.py:556,580
        triplas = set(
            ExportacaoDominio.objects.order_by()
            .values_list("empresa_id", "mes", "ano").distinct()
        )
        entregue = {}

        def k6(chave):
            if chave not in entregue:
                entregue[chave] = competencia_entregue(*chave) is True
            return entregue[chave]

        exportadas = {k for k in triplas if k6(k)}

        # E4B: alcance declarado, unidade dia-colaborador.
        # backfill_fio_morto_e4b.py:76,153,237
        _, base = _alcance(None, hoje, agora)
        extra = _exportadas_classe_e(None, hoje)
        dias = {(x[1], x[2]): x for x in base}
        for x in extra:
            dias.setdefault((x[1], x[2]), x)

        empresas = dict(
            Colaborador.objects.filter(
                pk__in={cid for cid, dia in dias}
            ).values_list("pk", "empresa_id")
        )
        e4b_exp = Counter()
        e4b_partes = Counter(
            exportada=0, nao_exportada=0, indeterminada=0,
        )
        for (cid, dia), x in dias.items():
            emp = empresas.get(cid)
            try:
                # Competencia produzida pelo alcance real.
                mes, ano = map(int, x[3].split("/"))
                chave = (emp, mes, ano)
            except (ValueError, TypeError, AttributeError):
                e4b_partes["indeterminada"] += 1
                continue

            if emp is None:
                e4b_partes["indeterminada"] += 1
            elif k6(chave):
                e4b_partes["exportada"] += 1
                e4b_exp[chave] += 1
                exportadas.add(chave)
            else:
                e4b_partes["nao_exportada"] += 1

        assert sum(e4b_partes.values()) == len(dias)
        assert sum(e4b_exp.values()) == e4b_partes["exportada"]

        # Projecao: alcance real, unidade nova linha DP.
        # backfill_decisoes_pergunta.py:44,67,73,79,82
        rv = linhas_a_criar()
        rv_partes = Counter(
            exportada=0, nao_exportada=0,
            conversa=0, sem_competencia=0,
        )
        rv_exp = {}
        total_rv = 0

        for chave, atos in rv.items():
            r = atos.get("resposta", [])
            v = atos.get("validacao", [])
            n = len(r) + len(v)
            assert n == sum(len(itens) for itens in atos.values())
            total_rv += n
            emp, mes, ano = chave

            if mes is None or ano is None or emp is None:
                balde = (
                    "conversa" if ano == "conversa"
                    else "sem_competencia"
                )
                rv_partes[balde] += n
            elif k6(chave):
                rv_partes["exportada"] += n
                exportadas.add(chave)
                rv_exp[chave] = dict(
                    resposta=len(r),
                    validacao=len(v),
                    total=n,
                    perguntas=len({pk for cid, pk in r + v}),
                )
            else:
                rv_partes["nao_exportada"] += n

        assert sum(rv_partes.values()) == total_rv
        assert (
            sum(x["total"] for x in rv_exp.values())
            == rv_partes["exportada"]
        )

        for emp, mes, ano in sorted(exportadas):
            desconhecido = dict(
                alcance="sem alcance chamavel",
                linhas=None,
                mudariam_campo_lido_gravado_TXT=None,
            )
            proj = rv_exp.get(
                (emp, mes, ano),
                dict(resposta=0, validacao=0, total=0, perguntas=0),
            )
            saida.append(dict(
                empresa="emp%d" % emp,
                competencia="%02d/%04d" % (mes, ano),
                E4B_dias_no_alcance=e4b_exp[(emp, mes, ano)],
                A18=desconhecido,
                A19=desconhecido,
                sem_alcance_chamavel=2,
                RV_DP=dict(
                    proj, mudariam_campo_lido_gravado_TXT=None,
                ),
                atribuicao_RV_DP_a_E4c="nao_verificado",
            ))

        saida.append(dict(
            TOTAL_E4B_dias=len(dias),
            partes=dict(e4b_partes),
        ))
        saida.append(dict(
            TOTAL_RV_DP=total_rv,
            partes=dict(rv_partes),
        ))
        saida.append(dict(
            TOTAL_exportadas=len(exportadas),
            escritores_distintos_sem_alcance_chamavel=2,
            pares_escritor_competencia_sem_alcance_chamavel=(
                2 * len(exportadas)
            ),
            delta_minutos_rubricas="nao_verificado",
        ))
    return saida


ok = False
nivel_anterior = logging.root.manager.disable
try:
    logging.disable(logging.CRITICAL)
    with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
        linhas = medir()
    for linha in linhas:
        print(json.dumps(linha, sort_keys=True, ensure_ascii=True))
    ok = True
except Exception:
    # Nao imprimir excecao, parametros SQL ou dados pessoais.
    print("ERRO leitura incompleta; resultados nao verificados")
finally:
    logging.disable(nivel_anterior)
    print("FIM completo=%d" % int(ok))
```

_5465 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
