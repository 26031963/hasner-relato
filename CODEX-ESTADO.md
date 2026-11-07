# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 22:01** · publicado em **06/11/2026 22:02** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 22:01 · sessao 4fed66c7fd38 · V2-BUGS-I2 · sonda · origem=Code

**Sim: duplica a interseção entre 22h e a saída.** O `s1` é estendido até `fim`; depois, o laço soma também o `s2`, iniciado às 22h. Para saída às 23h, duplica 22–23; não necessariamente 22–24 inteiro. [`app/ponto/motor_calculo_v2.py:110`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:110), [`app/ponto/motor_calculo_v2.py:112`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:112)

Na `he_noturna`, essa duplicação depende de **`inicio_extra`** também cair antes das 05h: a função primeiro recua pelo total de HE e só então mede o noturno. Portanto, um período candidato pode ter excesso de noturno e diferença **zero na HE**. [`app/ponto/motor_calculo_v2.py:440`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:440)

Contraexemplo próprio, deduzido do código: **01/10/2026, 04:00–23:00**, sem intervalos, prorrogação ligada, `extra_50_min=1140`, `extra_100_min=0`. O recuo encontra 04h; `s1` conta **1.140 min**, `s2` acrescenta **60 min**: `he_noturna` devolve **(1200, 0)**. Limitando somente `s1` às 22h, devolve **(1140, 0)**: excesso de **60 min**. Dedução, sem execução. [`app/ponto/motor_calculo_v2.py:392`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:392), [`app/ponto/motor_calculo_v2.py:110`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:110), [`app/ponto/motor_calculo_v2.py:448`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:448)

O script abaixo intercepta a **chamada real** do fechamento a `apurar_por_fatia`, preserva os motores de todas as fatias e interrompe após medir `resultado.periodos`. Essa porta compõe os mesmos objetos; `_ap.motor`, isoladamente, representa apenas a última fatia. A lavratura fica posteriormente à interceptação. [`app/ponto/services/fechamento.py:754`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/services/fechamento.py:754), [`app/ponto/services/fechamento.py:294`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/services/fechamento.py:294), [`app/ponto/services/fechamento.py:317`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/services/fechamento.py:317), [`app/ponto/services/fechamento.py:1190`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/services/fechamento.py:1190)

**É medida, não cura:** mantém períodos, HE, intervalos e decisões originais; cria em memória uma variante da função existente alterando exclusivamente `s1_fim_efetivo = fim` para `min(fim, s2_ini)`. Chama a `he_noturna` real duas vezes por candidato e também compara o noturno de relógio nas pontas efetivas. Script completo proposto, sem execução; totais da frota **[não verificado]**.

```python
# v2_bugs_i2_sonda.py
# Proposta nao executada [nao verificado].
# Medida, nao cura. Nenhum arquivo e escrito por este Python.

import ast
import inspect
import io
import json
import logging
import math
import textwrap
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from datetime import date, time
from unittest.mock import patch


class Falha(Exception):
    pass


class EncerrarComRollback(Exception):
    def __init__(self, dados):
        super().__init__("rollback_sonda")
        self.dados = dados


class Silencio(io.TextIOBase):
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass


JANELAS = (
    (10, date(2026, 9, 21), date(2026, 10, 20)),
    (11, date(2026, 10, 21), date(2026, 11, 20)),
)

CAMPOS = (
    "periodos_condicao",
    "periodos_excesso_noturno",
    "periodos_excesso_he",
    "min_noturno_relogio_a_mais",
    "min_he50_noturna_a_mais",
    "min_he100_noturna_a_mais",
    "min_he_noturna_a_mais",
)
EPS = 1e-7


def vazio():
    return {campo: 0 for campo in CAMPOS}


def somar(destino, origem):
    for campo in CAMPOS:
        destino[campo] += origem[campo]


def pacote():
    return {
        "todos": vazio(),
        "fora_feriado_dobra": vazio(),
    }


def somar_pacote(destino, origem):
    for grupo in destino:
        somar(destino[grupo], origem[grupo])


def diferenca(antes, depois):
    antes, depois = float(antes), float(depois)
    if not (math.isfinite(antes) and math.isfinite(depois)):
        raise Falha("medida_nao_finita")
    valor = antes - depois
    if valor < -EPS:
        raise Falha("diferenca_negativa")
    return max(0.0, valor)


def variante_com_s1_limitado(modulo):
    # Fonte real: motor_calculo_v2.py:82-118; unica alteracao em :111.
    arvore = ast.parse(textwrap.dedent(
        inspect.getsource(modulo.minutos_noturnos)
    ))
    alvos = [
        node
        for node in ast.walk(arvore)
        if isinstance(node, ast.Assign)
        and len(node.targets) == 1
        and isinstance(node.targets[0], ast.Name)
        and node.targets[0].id == "s1_fim_efetivo"
        and isinstance(node.value, ast.Name)
        and node.value.id == "fim"
    ]
    if len(alvos) != 1:
        raise Falha("fonte_incompativel")

    alvo = alvos[0]
    alvo.value = ast.copy_location(
        ast.Call(
            func=ast.Name(id="min", ctx=ast.Load()),
            args=[
                alvo.value,
                ast.Name(id="s2_ini", ctx=ast.Load()),
            ],
            keywords=[],
        ),
        alvo.value,
    )
    ast.fix_missing_locations(arvore)
    ambiente = dict(vars(modulo))
    exec(compile(arvore, "<sonda_s1_limitado>", "exec"), ambiente)
    return ambiente["minutos_noturnos"]


def medir_resultado(ap, donos, ini, fim, mod, limitado):
    resultado = ap.resultado
    totais = pacote()
    dias = {}

    for periodo in resultado.periodos:
        motor = donos[id(periodo)]  # Ausencia do dono e erro, nunca fallback.
        if periodo.saida is None:
            continue

        entrada = mod.localizar(periodo.entrada)
        saida = mod.localizar(periodo.saida)
        prorrogacao = motor.prorrogacao_noturna_pos5h

        candidato = (
            bool(prorrogacao)
            and entrada.date() == saida.date()
            and entrada.time() < time(5)
            and saida.time() > time(22)
        )
        if not candidato:
            continue

        jornada = resultado.dia_da_jornada[id(periodo)]
        if not ini <= jornada <= fim:
            raise Falha("jornada_fora_da_janela")

        # Regra real, no motor desta fatia, com o dia julgado da jornada.
        dobra = motor._feriado_dobra_no_dia(jornada)

        argumentos_he = {
            "entrada": periodo.entrada,
            "saida": periodo.saida,
            "extra_50_min": periodo.minutos_extra_50,
            "extra_100_min": periodo.minutos_extra_100,
            "prorrogacao": prorrogacao,
            "intervalos": periodo.intervalos_do_motor,
        }
        efetivo_entrada = periodo.efetivo_entrada
        efetivo_saida = periodo.efetivo_saida
        if efetivo_entrada is None or efetivo_saida is None:
            raise Falha("segmento_efetivo_ausente")

        # Primeira chamada: he_noturna REAL, com os dados originais.
        he_antes = mod.he_noturna(**argumentos_he)
        noturno_antes = motor.calcular_noturnas(
            efetivo_entrada,
            efetivo_saida,
            periodo.intervalos_do_motor,
        )[0]

        if not math.isclose(
            noturno_antes,
            periodo.minutos_noturnos_relogio,
            rel_tol=1e-9,
            abs_tol=EPS,
        ):
            raise Falha("releitura_noturno_divergente")

        # Somente durante a medida; nao recalcula a apuracao.
        # A prorrogacao permanece a do periodo, inclusive nos intervalos.
        with patch.object(mod, "minutos_noturnos", limitado):
            # Segunda chamada: a MESMA he_noturna REAL.
            he_depois = mod.he_noturna(**argumentos_he)
            noturno_depois = motor.calcular_noturnas(
                efetivo_entrada,
                efetivo_saida,
                periodo.intervalos_do_motor,
            )[0]

        d50 = diferenca(he_antes[0], he_depois[0])
        d100 = diferenca(he_antes[1], he_depois[1])
        dnot = diferenca(noturno_antes, noturno_depois)
        dhe = d50 + d100

        medida = {
            "periodos_condicao": 1,
            "periodos_excesso_noturno": int(dnot > EPS),
            "periodos_excesso_he": int(dhe > EPS),
            "min_noturno_relogio_a_mais": dnot,
            "min_he50_noturna_a_mais": d50,
            "min_he100_noturna_a_mais": d100,
            "min_he_noturna_a_mais": dhe,
        }

        chave = (entrada.date().isoformat(), jornada.isoformat())
        por_dia = dias.setdefault(chave, pacote())
        for destino in (totais, por_dia):
            somar(destino["todos"], medida)
            if not dobra:
                somar(destino["fora_feriado_dobra"], medida)

    return {
        "periodos_produzidos": len(resultado.periodos),
        "totais": totais,
        "dias": dias,
    }


def medir_colaborador(cid, mes, ini, fim, fechamento, mod, limitado):
    from django.db import transaction

    original_apurar = fechamento.apurar_por_fatia
    donos = {}

    def observar_porta(colab, inicio, final, batidas,
                       escalas, celulas, montar_motor):
        if colab.pk != cid:
            raise Falha("colaborador_inesperado")
        if (inicio, final) != (ini, fim):
            raise Falha("corte_da_empresa_divergente")

        with ExitStack() as pilha:
            def observar_montagem(escala, fatia_ini, fatia_fim):
                motor = montar_motor(escala, fatia_ini, fatia_fim)
                calcular_real = motor.calcular_mes

                def observar_mes(*args, **kwargs):
                    parcial = calcular_real(*args, **kwargs)
                    for periodo in parcial.periodos:
                        anterior = donos.get(id(periodo))
                        if anterior is not None and anterior is not motor:
                            raise Falha("periodo_com_dois_motores")
                        donos[id(periodo)] = motor
                    return parcial

                pilha.enter_context(
                    patch.object(motor, "calcular_mes", observar_mes)
                )
                return motor

            ap = original_apurar(
                colab, inicio, final, batidas,
                escalas, celulas, observar_montagem,
            )

        dados = medir_resultado(
            ap, donos, ini, fim, mod, limitado,
        )
        # Sai antes do downstream/lavratura; rollback do atomic externo.
        raise EncerrarComRollback(dados)

    try:
        with transaction.atomic():
            with patch.object(
                fechamento, "apurar_por_fatia", observar_porta
            ):
                fechamento.recalcular_fechamento_mes(
                    mes=mes,
                    ano=2026,
                    colaborador_ids=[cid],
                    somente_leitura=True,
                    lavrar_dias_pagos=False,
                )
            raise Falha("porta_nao_alcancada")
    except EncerrarComRollback as encerramento:
        return encerramento.dados


def executar():
    from django.db import connection
    import ponto.motor_calculo_v2 as mod
    import ponto.services.fechamento as fechamento

    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        if tuple(cursor.fetchone()) != ("sombra", "juliani"):
            raise Falha("ambiente_incorreto")

    limitado = variante_com_s1_limitado(mod)

    # Primeiro materializa os dois universos pela autoridade do fechamento.
    universos = [
        (
            mes, ini, fim,
            list(
                fechamento.universo_da_competencia(mes, 2026)
                .order_by("pk")
                .values_list("pk", flat=True)
            ),
        )
        for mes, ini, fim in JANELAS
    ]

    saidas = []
    soma_global = pacote()
    tentativas_global = 0
    validos_global = 0
    erros_global = 0
    ids_tentados = set()

    for mes, ini, fim, ids in universos:
        competencia = "2026-%02d" % mes
        soma_competencia = pacote()
        validos = 0
        erros = 0

        for cid in ids:
            identificador = "col%03d" % int(cid)
            ids_tentados.add(cid)
            try:
                dados = medir_colaborador(
                    cid, mes, ini, fim, fechamento, mod, limitado,
                )
            except Exception as erro:
                erros += 1
                # Mensagens e tracebacks podem conter dados pessoais.
                motivo = (
                    str(erro) if isinstance(erro, Falha)
                    else "erro_apuracao_ou_medida"
                )
                saidas.append({
                    "tipo": "colaborador",
                    "competencia": competencia,
                    "col": identificador,
                    "status": "nao_sei",
                    "contador_nao_sei": 1,
                    "motivo": motivo,
                    "soma": None,
                })
                continue

            validos += 1
            somar_pacote(soma_competencia, dados["totais"])
            saidas.append({
                "tipo": "colaborador",
                "competencia": competencia,
                "col": identificador,
                "status": "valido",
                "periodos_produzidos": dados["periodos_produzidos"],
                "soma": dados["totais"],
            })
            for (civil, jornada), valores in sorted(dados["dias"].items()):
                saidas.append({
                    "tipo": "dia",
                    "competencia": competencia,
                    "col": identificador,
                    "dia_civil": civil,
                    "dia_jornada": jornada,
                    "soma": valores,
                })

        if validos + erros != len(ids):
            raise Falha("universo_inconsistente")

        saidas.append({
            "tipo": "universo",
            "competencia": competencia,
            "inicio": ini.isoformat(),
            "fim": fim.isoformat(),
            "colabs_tentados": len(ids),
            "colabs_validos": validos,
            "colabs_com_erro": erros,
            "contador_nao_sei": erros,
            "status": "completo" if erros == 0 else "nao_sei",
            "soma_conhecida": soma_competencia,
            "soma": soma_competencia if erros == 0 else None,
        })

        tentativas_global += len(ids)
        validos_global += validos
        erros_global += erros
        somar_pacote(soma_global, soma_competencia)

    saidas.append({
        "tipo": "soma_ambas_competencias",
        "unidade_universo": "colaborador_competencia",
        "tentados": tentativas_global,
        "validos": validos_global,
        "com_erro": erros_global,
        "colabs_distintos_tentados": len(ids_tentados),
        "contador_nao_sei": erros_global,
        "status": "completo" if erros_global == 0 else "nao_sei",
        "soma_conhecida": soma_global,
        "soma": soma_global if erros_global == 0 else None,
        "natureza": "medida_nao_cura",
    })
    return saidas


def arredondar(valor):
    if isinstance(valor, dict):
        return {k: arredondar(v) for k, v in valor.items()}
    if isinstance(valor, list):
        return [arredondar(v) for v in valor]
    if isinstance(valor, float):
        return round(valor, 9)
    return valor


nivel_anterior = logging.root.manager.disable
try:
    logging.disable(logging.CRITICAL)
    with redirect_stdout(Silencio()), redirect_stderr(Silencio()):
        try:
            registros = executar()
        except Exception:
            registros = [{
                "tipo": "fatal",
                "status": "nao_sei",
                "motivo": "preparacao_ou_universo",
                "colabs_tentados": None,
                "colabs_validos": None,
                "colabs_com_erro": None,
                "soma": None,
            }]
finally:
    logging.disable(nivel_anterior)

for registro in registros:
    print(json.dumps(
        arredondar(registro),
        ensure_ascii=True,
        sort_keys=True,
        allow_nan=False,
    ))
```

No desenho, `todos.periodos_condicao` responde (1), e `fora_feriado_dobra.periodos_condicao` responde (2); as respectivas somas mostram excesso de relógio e de HE separadamente. Os candidatos com diferença zero continuam contados. Erro invalida o colaborador inteiro; a soma integral fica `null`, acompanhada da soma conhecida. Comportamento proposto **[não verificado]**.

A identificação de feriado-dobra chama `_feriado_dobra_no_dia` com a jornada julgada e o motor da fatia. Usar apenas `minutos_extra_100_feriado` seria insuficiente: comercial/partido podem concentrar a dobra diária no último período. `app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:3092`, `app/ponto/motor_calculo_v2.py:3342`.

Os campos de aplicação acessados diretamente e as dependências delegadas aos juízes têm esta proveniência:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `Colaborador.pk` | Porta de criação: `app/colaboradores/services/colaborador.py:91`; geração automática do identificador **[não verificado]** | Universo e apuração: `app/ponto/services/fechamento.py:439`, `app/ponto/services/fechamento.py:290` |
| `situacao`, `data_demissao` — universo | `app/colaboradores/services/desligamento.py:104`; persistência: `app/colaboradores/services/colaborador.py:73` | `app/ponto/services/fechamento.py:439` |
| `empresa_id` — universo/janela | Edição: `app/colaboradores/views.py:1313`; transferência: `app/colaboradores/views.py:1386` | `app/ponto/services/fechamento.py:437`, `app/ponto/services/fechamento.py:621` |
| `Empresa.dia_inicio_competencia` — janela | `app/colaboradores/views.py:523`; persistência: `app/colaboradores/services/estrutura.py:17` | `app/ponto/janelas.py:50`; fechamento: `app/ponto/services/fechamento.py:621` |
| `_Apuracao.resultado`, `resultado.periodos` | Retorno: `app/ponto/services/fechamento.py:318`; composição: `app/ponto/services/fechamento.py:294`; motor: `app/ponto/motor_calculo_v2.py:2494` | `app/ponto/services/fechamento.py:756`, `app/ponto/services/fechamento.py:1202` |
| `periodo.entrada`, `saida` | Construtores: `app/ponto/motor_calculo_v2.py:2751`, `app/ponto/motor_calculo_v2.py:2878`, `app/ponto/motor_calculo_v2.py:3208` | HE: `app/ponto/motor_calculo_v2.py:1015` |
| `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor` | Carimbo: `app/ponto/motor_calculo_v2.py:1840` | Alimentação do calculador: `app/ponto/calculador/alimentacao.py:5`; intervalos da HE: `app/ponto/motor_calculo_v2.py:1018` |
| `minutos_extra_50`, `minutos_extra_100` | 12x36: `app/ponto/motor_calculo_v2.py:2851`; partido: `app/ponto/motor_calculo_v2.py:3147`; comercial: `app/ponto/motor_calculo_v2.py:3404`; dobra: `app/ponto/motor_calculo_v2.py:3094`, `app/ponto/motor_calculo_v2.py:3345` | `app/ponto/motor_calculo_v2.py:1016` |
| `minutos_noturnos_relogio` | Escritor único: `app/ponto/motor_calculo_v2.py:1004` | Soma do resultado: `app/ponto/motor_calculo_v2.py:269`; localização exata do leitor agregado **[não verificado]** |
| `resultado.dia_da_jornada` | Motor: `app/ponto/motor_calculo_v2.py:2460`, `app/ponto/motor_calculo_v2.py:1075`; composição: `app/ponto/services/fechamento.py:311` | `app/ponto/calculador/periodo.py:596`, `app/ponto/calculador/periodo.py:611` |
| `motor.prorrogacao_noturna_pos5h` | Fábrica CCT: `app/core/regua_cct.py:454`, `app/core/regua_cct.py:466`; armazenamento: `app/ponto/motor_calculo_v2.py:762` | Noturno: `app/ponto/motor_calculo_v2.py:984`; HE: `app/ponto/motor_calculo_v2.py:1017` |
| `feriado_em_dobra`, `feriados`, `_feriados_injetados` — juiz delegado | `app/ponto/motor_calculo_v2.py:787`, `app/ponto/motor_calculo_v2.py:790`, `app/p

[truncado em 20000 de 20761 caracteres]

## 06/11 21:57 · sessao 0b0cf1ff4167 · - · - · origem=Ronald (tmux)

A porta fiel é chamar `recalcular_fechamento_mes(..., colaborador_ids=[pk], somente_leitura=True, lavrar_dias_pagos=False)` e interceptar sua chamada real a `apurar_por_fatia`, que entrega `resultado` em `app/ponto/services/fechamento.py:754-756`; assinatura em `app/ponto/services/fechamento.py:448-451`.

Desenho recomendado: patch temporário de `fechamento.apurar_por_fatia`; wrapper chama a função original com seus argumentos originais, captura o `_Apuracao.resultado` e levanta sentinela imediatamente. Envolver a chamada ao recalculador em `transaction.atomic`, capturando a sentinela fora do `with` é proposta de script, ainda não executada. [não verificado]

Para preservar o motor de cada período, envolver também o `montar_motor` recebido: ele cria a instância efetivamente usada pela fatia em `app/ponto/services/fechamento.py:279`; envolver seu `calcular_mes` e registrar `{id(p): motor}` dos períodos do resultado retornado. A chamada efetiva é `app/ponto/services/fechamento.py:286-291`. A composição preserva os objetos dos períodos em `app/ponto/services/fechamento.py:294-297`, explicitado em `app/ponto/services/fechamento.py:306-307`.

Leituras e escritores:

| Campo | Escritor | Leitor real |
|---|---|---|
| `resultado.periodos` | motor encaminha período em `app/ponto/motor_calculo_v2.py:2494`; composição em `app/ponto/services/fechamento.py:294-297` | fechamento em `app/ponto/services/fechamento.py:1202` |
| `entrada`, `saida` | construtores dos três motores em `app/ponto/motor_calculo_v2.py:2751`, `:2878`, `:3208` | `_aplicar_he_noturna` em `app/ponto/motor_calculo_v2.py:1015` |
| `minutos_extra_50`, `minutos_extra_100` | 12x36: `app/ponto/motor_calculo_v2.py:2811`, `:2851-2854`; partido: `:3094`, `:3147-3150`, `:3179`; comercial: `:3345`, `:3404-3407` | `_aplicar_he_noturna` em `app/ponto/motor_calculo_v2.py:1016` |
| `intervalos_do_motor` | `_carimbar_segmento` em `app/ponto/motor_calculo_v2.py:1842` | `_aplicar_he_noturna` em `app/ponto/motor_calculo_v2.py:1018` |
| `prorrogacao_noturna_pos5h` | inicialização em `app/ponto/motor_calculo_v2.py:758-762` | `_aplicar_he_noturna` em `app/ponto/motor_calculo_v2.py:1017` |
| universo de colaboradores | query canônica em `app/ponto/services/fechamento.py:439-445` | recalculador em `app/ponto/services/fechamento.py:554` |

As pontas usadas por `_aplicar_he_noturna` são `periodo.entrada/saida`, conforme `app/ponto/motor_calculo_v2.py:1015`; os campos `efetivo_entrada/saida` são escritos separadamente em `app/ponto/motor_calculo_v2.py:1840-1841`.

A classificação “fora de feriado-dobra” pode chamar o motor real da fatia: `motor._feriado_dobra_no_dia(dia)`, cuja resposta é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)` em `app/ponto/motor_calculo_v2.py:934`. O contrato informa que o dia recebido é o início do plantão em `app/ponto/motor_calculo_v2.py:928-930`.

A primeira lavratura fica depois da porta interceptada e tem guarda `(not somente_leitura) or lavrar_dias_pagos`, em `app/ponto/services/fechamento.py:1190`; chamadas de `lavrar` em `app/ponto/services/fechamento.py:1200` e `app/ponto/services/fechamento.py:1276`.

## 06/11 21:57 · sessao 1157629651c1 · - · - · origem=Ronald (tmux)

Para classificar “fora de dia de feriado-dobra”, use o **motor que efetivamente calculou aquela fatia** e o **dia da jornada do período**:

```python
dia = resultado.dia_da_jornada[id(periodo)]
feriado_dobra = motor_da_fatia._feriado_dobra_no_dia(dia)
```

A regra chamada é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; o próprio método estabelece que `dia` pertence ao início do plantão, incluindo suas caudas. [app/ponto/motor_calculo_v2.py:914-934]

Não use `_ap.motor` indiscriminadamente: esse atributo representa somente a **última fatia que rodou**, enquanto `resultado` reúne todas as fatias. [app/ponto/services/fechamento.py:171-174; app/ponto/services/fechamento.py:317-320]

Não use apenas `periodo.minutos_extra_100_feriado > 0`: no comercial/partido, o valor de **todo o dia** fica em `periodos_dia[-1]`, e os outros períodos daquele mesmo dia podem carregar zero. [app/ponto/motor_calculo_v2.py:3081-3095; app/ponto/motor_calculo_v2.py:3334-3346]

Proveniência dos campos:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `resultado.dia_da_jornada[id(p)]` | Motor atribui a jornada em `data_jornada`, grava o mapa do período e o expõe no resultado; a composição mescla mapas. [app/ponto/motor_calculo_v2.py:2403-2408; app/ponto/motor_calculo_v2.py:2460; app/ponto/motor_calculo_v2.py:1073-1075; app/ponto/services/fechamento.py:311-316] | Releitura agrupa períodos pelo mapa. [app/ponto/calculador/periodo.py:596-615] |
| `motor.feriado_em_dobra` | `get_motor` resolve pelo ciclo quando não recebeu override, passa ao construtor e este armazena; fábrica CCT pode habilitar override para `12x36`. [app/ponto/motor_calculo_v2.py:3679-3690; app/ponto/motor_calculo_v2.py:787; app/core/regua_cct.py:473-479] | `_feriado_dobra_no_dia`. [app/ponto/motor_calculo_v2.py:934] |
| `motor.feriados` / `_feriados_injetados` | Fechamento obtém feriados do colaborador e os injeta na fábrica do motor; construtor guarda conjunto e sua identidade. [app/ponto/services/fechamento.py:647-648; app/ponto/services/fechamento.py:735-741; app/ponto/motor_calculo_v2.py:790-795] | `eh_feriado` consulta conjunto; quando conjunto injetado responde, ausência retorna `False`, sem fallback. [app/ponto/motor_calculo_v2.py:956-967] |
| `minutos_extra_100_feriado` | Motor escreve dobra de todo dia no último período; ramo 12x36 também escreve diretamente no período. [app/ponto/motor_calculo_v2.py:3092-3095; app/ponto/motor_calculo_v2.py:3342-3346; app/ponto/motor_calculo_v2.py:2809-2813] | Fechamento lê a propriedade agregada `resultado.horas_extra_100_feriado`. [app/ponto/services/fechamento.py:923] |
| `prorrogacao_noturna_pos5h` | Fábrica CCT define segundo piso/regra cadastrada; construtor armazena. [app/core/regua_cct.py:449-466; app/ponto/motor_calculo_v2.py:758-762] | `_aplicar_he_noturna` passa o atributo à função real. [app/ponto/motor_calculo_v2.py:1014-1018] |
| `intervalos_do_motor` | `_carimbar_segmento` copia os intervalos efetivamente considerados. [app/ponto/motor_calculo_v2.py:1833-1842] | `_aplicar_he_noturna` entrega-os à função real. [app/ponto/motor_calculo_v2.py:1014-1018] |

Uma captura por fatia pode envolver a instância retornada pelo `_montar_motor` real e interceptar seu `calcular_mes`: a porta compartilhada chama `montar_motor(...)` e, imediatamente depois, `motor.calcular_mes(...)`, antes de compor os mesmos objetos em `resultado.periodos`. [app/ponto/services/fechamento.py:279-297]

## 06/11 21:57 · sessao 78efd8081cd5 · - · - · origem=Ronald (tmux)

O universo correto é `universo_da_competencia(mes, ano)`, sem filtrar `situacao='ativo'`: inclui todo não desligado e desligado cuja demissão alcance o início da janela da própria empresa; sem `empresa_id`, une as empresas. Use `.order_by('pk')` para evitar a ordenação por nome da função. (`app/ponto/services/fechamento.py:382`, `:431-445`)

Campos envolvidos:

| Campo | Escritor | Leitor |
|---|---|---|
| `Colaborador.situacao`, `data_demissao` | `app/colaboradores/services/desligamento.py:104-108`; persistência em `app/colaboradores/services/colaborador.py:73-76` | `app/ponto/services/fechamento.py:432-442` |
| `Colaborador.empresa_id` | edição `app/colaboradores/views.py:1313`, persistência `:1341-1342`; transferência `:1385-1388` | `app/ponto/services/fechamento.py:437-438` |
| `Empresa.dia_inicio_competencia` | UI `app/colaboradores/views.py:496-499`, `:520-523`, `:531-538`; persistência `app/colaboradores/services/estrutura.py:12-20` | `app/ponto/janelas.py:36-57`; universo em `app/ponto/services/fechamento.py:436-438` |

As janelas solicitadas só coincidem com a janela real quando o corte efetivo é 21; valide `janela_fechamento` por colaborador e conte divergência como “não sei” em vez de apurar janela diferente silenciosamente. (`app/ponto/janelas.py:10-21`, `:36-57`)

`--rodar` exige o arquivo em `logs/sombra`, monta essa pasta como `/sombra` e chama `tenant_command shell --schema=juliani`. (`bin/sombra.sh:189-190`, `:483-491`)

O wrapper usa settings de sombra e proxy morto, com banco e host literais `sombra` / `sombra_db`. (`bin/sombra.sh:184-188`; `app/config/settings/sombra.py:29-50`)

Há um detalhe material: **antes do Python da sonda**, `--rodar` chama `lavra_de_prod`; ela lê o snapshot em produção e grava snapshots na sombra. Essas ações ficam fora da transação do script. Logo, o script Python pode ser apenas leitura/rollback, mas não se deve descrever o wrapper inteiro como “sem rede” ou “sem escrita”. (`bin/sombra.sh:138-166`, `:489-491`)

## 06/11 21:52 · sessao 6e064515fdcd · V2-BUGS-I2 · verificador · origem=Code

**Reprovaria a volta 1:** a correção deixa o partido com dois pares incompleto, mantém a promoção de FT partido sem HE100 noturna e transforma o resultado errado em expectativa obrigatória no F11d. (`app/ponto/motor_calculo_v2.py:3105`, `app/ponto/motor_calculo_v2.py:2897`, `app/ponto/tests/test_o280_bateria_da_lei.py:1705`)

1. **Escritores, leitores e significados**

   | Campo/função | Quem escreve | Quem lê |
   |---|---|---|
   | `minutos_extra_100_noturna` | `_aplicar_he_noturna`; resets mensais e bloqueio zeram. (`app/ponto/motor_calculo_v2.py:1020`, `:2938`, `:3329`, `:1475`) | `ResultadoMes`, lavratura normal, promoção no DiaPago e promoção no FM. (`app/ponto/motor_calculo_v2.py:344`; `app/ponto/services/dia_pago.py:184`, `:252`; `app/ponto/services/fechamento.py:963`) |
   | `minutos_extra_100_feriado` | Dobras por período e por dia; também a releitura do calculador. (`app/ponto/motor_calculo_v2.py:2812`, `:3095`, `:3263`, `:3346`; `app/ponto/calculador/periodo.py:844`) | `ResultadoMes`, DiaPago e contas que excluem a dobra do excedente semanal. (`app/ponto/motor_calculo_v2.py:332`, `:2637`, `:3474`; `app/ponto/services/dia_pago.py:183`) |
   | FM.`horas_extras_100_noturna` | `recalcular_fechamento_mes`, com persistência na mesma porta; `restaurar_fechamento` também aceita restaurar o campo. (`app/ponto/services/fechamento.py:924`, `:961`, `:1168`, `:1379`, `:1392`) | Reconciliação da `porta_export`, por nome de campo. (`app/folha/porta_export.py:451`, `:452`) |
   | DiaPago.`horas_extras_100_noturna` | `lavrar`, somando períodos normais e FT promovida; versão oráculo pode substituir pela rubrica calculada, dentro da mesma função. (`app/ponto/services/dia_pago.py:184`, `:252`, `:329`, `:345`, `:360`) | Soma da competência, reconciliação e identificação de dia com dinheiro lavrado. (`app/ponto/services/dia_pago.py:562`, `:863`; `app/folha/porta_export.py:451`) |
   | `he_noturna` | Função decisória, retorna dois valores; não persiste campos. (`app/ponto/motor_calculo_v2.py:409`, `:451`) | Casca do motor e calculador por período. (`app/ponto/motor_calculo_v2.py:1014`; `app/ponto/calculador/regras.py:832`) |

   **Unidade:** o split devolve minutos de relógio; `ResultadoMes`, FM e DiaPago convertem para horas dividindo por 60. A redução fica em `minutos_noturnos_legais = relogio × fator`; o fator vale 1 quando afastada a hora reduzida. Portanto HE100 noturna e adicional noturno legal **não são numericamente a mesma medida**. (`app/ponto/motor_calculo_v2.py:441`, `:448`, `:344`, `:986`, `:992`; `app/ponto/services/dia_pago.py:252`; `app/ponto/services/fechamento.py:963`)

   **Não há significado uniforme de dia nem de trecho:** o DiaPago recebe o mapa da jornada, mas a promoção identifica feriado pela data civil da entrada; o split lê pontas cruas, enquanto duração e noturnas usam pontas efetivas. (`app/ponto/services/fechamento.py:947`, `:949`, `:1216`; `app/ponto/services/dia_pago.py:128`; `app/ponto/motor_calculo_v2.py:1015`, `:3216`, `:3252`)

2. **Duplicação e FM versus soma dos dias**

   Não encontrei duplicação pelo caminho normal `periodos + ft_feriado`: o motor coloca cada período em um dos dois conjuntos; `ResultadoMes` soma somente os regulares; a promoção atribui essa base mais FT feriada, sem acumular novamente o FM anterior. O DiaPago recebe as mesmas partições. (`app/ponto/motor_calculo_v2.py:2492`, `:2494`, `:344`; `app/ponto/services/fechamento.py:961`, `:979`, `:1202`)

   **Existe duplicação dentro da própria função noturna:** com prorrogação habilitada, o segmento 00–05 é estendido até a saída e depois o segmento 22–24 é somado novamente. Exemplo reproduzido com as funções extraídas em memória: **03–23, HE100=1.200 minutos → HE100 noturna=1.260**, contando 22–23 duas vezes. A dívida já está documentada no código; a chamada nova do comercial passa a expô-la no mensal. (`app/ponto/motor_calculo_v2.py:110`, `:112`, `:116`, `:153`, `:3361`)

   **FM diferente da soma dos dias continua possível por caminhos preexistentes:** o FM é persistido antes da lavratura, cuja exceção é capturada; uma falha Python nessa etapa pode deixar DP antigo ou ausente. A restauração também permite alterar somente o FM. Ocorrência real: **[não verificado]**. (`app/ponto/services/fechamento.py:1143`, `:1159`, `:1198`, `:1237`, `:1392`)

3. **Efeito da chamada antes do `continue`**

   A hipótese de mudar `minutos_noturnos` ou `extra_50` **não se sustenta**: a casca só escreve os dois subconjuntos noturnos; nos ramos da dobra, HE50 e HE50 noturna já foram zeradas. Assim, nesse caminho, somente HE100 noturna muda. (`app/ponto/motor_calculo_v2.py:1019`, `:1022`, `:2934`, `:2937`, `:3325`, `:3328`)

   O contraexemplo concreto é outro: **cru 13–23, efetivo 13–22, HE100=540, noturnas=0**. A chamada escreve **60 de HE100 noturna**, porque lê a saída crua. Na reprodução em memória, esse foi o único campo alterado. O recorte da saída ao marco existe, mas o carimbo guarda os efetivos em campos separados. (`app/ponto/motor_calculo_v2.py:1015`, `:1742`, `:1750`, `:1840`, `:1841`)

4. **Dois pares: início errado para a dobra do dia**

   **O split não alcança todo o trecho extra do dia:** os 420 minutos da dobra ficam em `ultimo`; o início calculado é limitado à entrada desse período. Para **21–00 / 03–07**, ele alcança somente 03–07: **240 minutos**, deixando os 120 noturnos do primeiro par de fora. (`app/ponto/motor_calculo_v2.py:3087`, `:3094`, `:406`; `app/ponto/tests/test_o280_bateria_da_lei.py:1697`, `:1705`)

   Pelo diff fornecido, isso é **lacuna preexistente parcialmente corrigida**, não regressão numérica: o pai pulava o split e deixava zero; agora entrega 240, ainda abaixo dos 360 exigidos pelo próprio F11e. Execução no pai: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:3102`, `:3105`; `app/ponto/tests/test_o280_bateria_da_lei.py:1717`)

5. **Mordida dos REDs**

   - F11/F11b/F11c cobram valores numéricos — 120 minutos, 60 minutos e 1 hora — por `assertEqual`; não aceitam qualquer exceção. Pelo diff, a falha esperada no pai é valor zero, sem depender de símbolo introduzido pela correção. Execução efetiva no pai: **[não verificado]**. (`app/ponto/tests/test_o280_bateria_da_lei.py:1266`, `:1594`, `:1632`, `:1674`)
   - **F11c mata o mutante “somente FM”**, pois verifica separadamente `dp_he100_not=1.0`. Entretanto cobre somente COM/comercial, deixando FT partido sem essa prova. (`app/ponto/tests/test_o280_bateria_da_lei.py:1659`, `:1674`, `:1679`)
   - **F11d morde pelo critério errado:** exige 240, o defeito conhecido. Uma correção completa quebraria esse teste. (`app/ponto/tests/test_o280_bateria_da_lei.py:1704`, `:1705`)
   - **F11e aceita falha anterior ao valor legal:** `expectedFailure` envolve também a montagem do caso e a chamada ao motor, sem restringir tipo ou mensagem. Seu resultado esperado não prova que a falha ocorreu na comparação 240 versus 360. (`app/ponto/tests/test_o280_bateria_da_lei.py:1688`, `:1689`, `:1708`, `:1716`)

6. **Classes estruturais e outro buraco da correção**

   Não identifiquei novo juiz da janela nem escritor fora da porta no diff: as chamadas reutilizam `he_noturna`, FM permanece no serviço registrado e DiaPago permanece em `lavrar`. Fechamento e lavratura apenas somam minutos; o oráculo chama a mesma função decisória. (`app/ponto/motor_calculo_v2.py:1014`; `app/core/portas.py:500`; `app/ponto/services/dia_pago.py:360`; `app/ponto/calculador/regras.py:832`)

   **A alegação “o motor JÁ escreveu” é falsa para FT partido:** seu `calcular_periodo` retorna sem HE/split, e o mensal recalcula apenas períodos regulares. Logo, um FT partido **21–00**, promovido com escala certa, pode receber **180 minutos de HE100 e zero HE100 noturna**, apesar dos 120 minutos noturnos. As novas linhas apenas leem esse zero. (`app/ponto/motor_calculo_v2.py:225`, `:2893`, `:2896`, `:2897`, `:2928`, `:3083`; `app/ponto/services/fechamento.py:954`, `:963`; `app/ponto/services/dia_pago.py:252`)

   As classes problemáticas são **selo que protege o erro**, **pontas cruas versus efetivas** e **dia civil versus jornada**. Na promoção, o segundo par pós-meia-noite pode receber classificação diferente do primeiro mesmo pertencendo à mesma jornada feriada. Ocorrência em produção: **[não verificado]**. (`app/ponto/tests/test_o280_bateria_da_lei.py:1705`; `app/ponto/motor_calculo_v2.py:1015`; `app/ponto/services/fechamento.py:947`, `:949`; `app/ponto/services/dia_pago.py:245`)

7. **TXT/Domínio**

   **O subconjunto alterado não muda diretamente os eventos do TXT:** `eventos_do_fechamento` emite HE50, HE100 normal/feriado e adicional noturno, sem emitir `horas_extras_100_noturna`; os totais usados nesses eventos não são alterados pela nova casca. Igualdade byte a byte de arquivos gerados: **[não verificado]**. (`app/folha/export.py:420`, `:439`, `:447`; `app/ponto/motor_calculo_v2.py:1019`)

   A `porta_export` lê o campo indiretamente pela soma do DiaPago e compara ao FM, mas **essa divergência fica fora de `falhas`**. Portanto a reconciliação observa o problema sem, por si só, impedir o TXT. (`app/ponto/services/dia_pago.py:562`; `app/folha/porta_export.py:451`, `:455`, `:687`, `:697`)

## 06/11 21:51 · sessao eb474d436086 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **M3 continua incompleto para turno partido:** `calcular_periodo` desse motor retorna com trabalhados/noturnos, sem calcular HE nem split (`app/ponto/motor_calculo_v2.py:2877`, `:2893`, `:2896`, `:2897`). O mensal recalcula somente `resultado.periodos`, deixando FT fora (`:2927`, `:3083`). Portanto FT partido promovido pode levar 180 min de HE100 e 120 min noturnos, mas **zero HE100 noturna**, pois o default é zero (`:225`) e as novas linhas apenas o somam (`app/ponto/services/fechamento.py:963`; `app/ponto/services/dia_pago.py:252`). Caso real em produção: [não verificado].

- **Item 3: nenhum outro campo muda pela nova chamada.** A casca só atribui HE100 noturna e HE50 noturna (`motor_calculo_v2.py:1019`, `:1021`); nos ramos da dobra, HE50 e HE50 noturna já foram zerados (`:2934`, `:2937`, `:3325`, `:3328`). Logo não há contraexemplo de alteração de `minutos_noturnos` ou `extra_50` por essa chamada (`:1014`–`:1022`).

- **Mas o subconjunto pode incluir minuto bloqueado:** a casca lê `entrada/saida` crus (`motor_calculo_v2.py:1015`), enquanto o recorte só grava `efetivo_entrada/saida` (`:1840`, `:1841`), usados no cálculo de duração/noturnas (`:3216`, `:3218`, `:3252`). Contraexemplo reproduzido com AST em memória: cru 13–23, efetivo 13–22, HE100=540, noturno=0 ⇒ nova chamada altera somente HE100 noturna, **0→60**. O corte da saída ao marco é permitido em `:1733`–`:1750`. O calculador usa os efetivos (`app/ponto/calculador/alimentacao.py:83`, `:106`), portanto também pode divergir do motor nesse cenário (`app/ponto/calculador/regras.py:832`).

- **Item 4: split não alcança o primeiro par.** A dobra inteira vai em `ultimo` (`motor_calculo_v2.py:3087`, `:3094`); `_inicio_do_trecho_extra` limita o início à entrada desse período (`:406`). AST exata confirmou 03–07 com extra100=420 ⇒ HE100 noturna=240. F11d exige 240 (`app/ponto/tests/test_o280_bateria_da_lei.py:1705`); F11e exige 360, mas está `expectedFailure` (`:1708`, `:1717`). Pelo diff fornecido, o pai pulava a chamada e deixava zero: **lacuna preexistente parcialmente corrigida**, não regressão numérica nova; execução do pai [não verificado].

- **Outra duplicidade possível dentro do próprio split:** `minutos_noturnos` estende o segmento 00–05 até `fim` (`motor_calculo_v2.py:110`, `:111`) e soma também 22–24 (`:112`–`:116`). AST exata: 03–23, HE100=1200, prorrogação=True ⇒ **HE100 noturna=1260**, contando 22–23 duas vezes. A dívida consta no próprio comentário (`:151`–`:160`); a chamada nova do comercial passa a expô-la no mensal (`:3361`). Ocorrência em produção [não verificado].

- **Não há soma aditiva por chamadas repetidas da casca:** ela sobrescreve o campo (`motor_calculo_v2.py:1020`). A classificação normal coloca o período em FT **ou** em períodos regulares (`:2492`, `:2493`, `:2494`); as properties de HE somam apenas os regulares (`:344`). Assim, não identifiquei duplicação M1/M2+M3 por esse caminho (`app/ponto/services/fechamento.py:933`, `:962`, `:963`).

## 06/11 21:50 · sessao f5b13d011799 · - · - · origem=Ronald (tmux)

- **Não há dupla soma por `periodos + ft_feriado` no caminho normal.** O motor põe cada período em um lado do `if/else` (`app/ponto/motor_calculo_v2.py:2468`, `:2492`, `:2494`); `ResultadoMes.horas_extra_100_noturna` soma somente `periodos` (`:344`). A promoção substitui o FM por essa base mais `_ft_feriado`, sem `+=` (`app/ponto/services/fechamento.py:961`). A partição remove feriados de `_ft_certa` e exclui ambos de `_ft_sem_escala` (`:952`, `:953`, `:979`, `:980`). O DiaPago recebe essas mesmas listas (`:1202`) e soma o subconjunto uma vez em cada ramo (`app/ponto/services/dia_pago.py:184`, `:252`).

- **Há divergência preexistente de significado do dia na promoção.** O motor decide folga pelo `data_jornada` e registra esse dia no mapa (`app/ponto/motor_calculo_v2.py:2448`, `:2460`); a promoção decide feriado pela **data civil da entrada** (`app/ponto/services/fechamento.py:947`, `:949`). A lavratura, por sua vez, usa o mapa (`app/ponto/services/dia_pago.py:245`, `:128`, `:132`; mapa passado em `app/ponto/services/fechamento.py:1216`). Contraexemplo deduzido: dois pares de uma jornada feriada, `21–00 / 03–07`; o segundo pode continuar em folga trabalhada por começar civilmente no dia seguinte, em vez de ser promovido com o primeiro. Ocorrência em produção [não verificado].

- **FM ≠ soma DP continua possível por caminho preexistente.** O FM é persistido antes da lavratura (`app/ponto/services/fechamento.py:1143`, `:1159`); falha em `lavrar` é capturada e apenas registrada (`:1237`, `:1240`), permitindo FM novo com DP antigo/ausente. A porta de restauração também aceita somente campos de FM (`:1357`, `:1379`, `:1391`). Ocorrência real [não verificado]. Diferença de armazenamento float/decimal não prova falha de reconciliação: a soma DP arredonda ao final (`app/ponto/services/dia_pago.py:562`, `:563`).

- **Unidades e escritores do mensal/dia:** DP declara rubricas em horas e usa `FloatField` (`app/ponto/models.py:325`, `:332`); FM usa decimal de duas casas (`:419`). Ambos leem minutos do período e dividem por 60, sem recalcular janela (`app/ponto/services/dia_pago.py:184`, `:252`; `app/ponto/services/fechamento.py:961`). Persistência DP fica na mesma `lavrar`, por `bulk_create` (`app/ponto/services/dia_pago.py:345`, `:359`, `:360`); FM escreve na porta de fechamento (`app/ponto/services/fechamento.py:1143`, `:1168`), também restaurável pela porta (`:1391`).

- **TXT não lê diretamente os splits alterados.** `eventos_do_fechamento` emite HE50, HE100 normal/feriado, folga e adicional noturno, sem evento para `_100_noturna` (`app/folha/export.py:439`, `:447`); comentários identificam os splits como subconjuntos (`:424`, `:433`). Porém mudança colateral em HE50 ou `minutos_noturnos_legais` pode alterar o TXT via `horas_extras_50` e `horas_noturnas` (`:441`, `:447`).

- **`porta_export` lê o campo, mas a reconciliação não bloqueia por si.** A soma inclui o campo via `CAMPOS_DIA` (`app/ponto/services/dia_pago.py:36`, `:562`), e compara cada campo ao FM com tolerância de 0,015h (`app/folha/porta_export.py:239`, `:451`, `:455`). As divergências são publicadas em `fechamento_x_soma_dias_pagos`, fora de `_falhas_proprias` (`:687`, `:697`). Portanto é testemunha, sem mordida impeditiva para essa classe (`:687`, `:697`).

- **O oráculo possui outra derivação, mas usa a mesma função decisória:** `regras.do_dia` chama `he_noturna` por período e soma (`app/ponto/calculador/regras.py:825`, `:832`, `:835`, `:839`); a mesma `lavrar` grava a versão separada `oraculo` (`app/ponto/services/fechamento.py:1276`, `:1287`, `:1293`). O fechamento e a lavratura do motor não fazem essa conta; leem o campo do período (`app/ponto/services/fechamento.py:963`; `app/ponto/services/dia_pago.py:252`).

## 06/11 21:50 · sessao abee3abff962 · - · - · origem=Ronald (tmux)

- **F11d cobra o defeito, não a lei:** exige `he100_not=240`; F11e afirma `360`, mas fica sob `expectedFailure`. Uma implementação correta dos dois pares reprovaria F11d e produziria sucesso inesperado em F11e. (`app/ponto/tests/test_o280_bateria_da_lei.py:1704`, `:1708`, `:1717`)
- **F11/F11b/F11c/F11d cobram números**, por `assertEqual`, sem aceitar exceção genérica. F11 e F11b exigem respectivamente 120 e 60 minutos; F11c exige 1 hora no FM **e** no DiaPago, portanto mata o mutante “somar somente no FM”. (`app/ponto/tests/test_o280_bateria_da_lei.py:1263`, `:1594`, `:1632`, `:1672`, `:1679`)
- **F11e tem mordida fraca:** `expectedFailure` cobre também o fixture e a execução do motor, antes da comparação; não restringe tipo ou mensagem da falha. Assim, uma falha anterior à divergência legal também pode satisfazer o selo. (`app/ponto/tests/test_o280_bateria_da_lei.py:1688`, `:1689`, `:1708`, `:1716`)
- **RED no pai:** execução efetiva não verificada; a cópia não possui repositório Git acessível. A análise estática mostra campos existentes e expectativa numérica: `leitura` usa `getattr(..., 0)`, e os modelos já declaram os campos noturnos. (`app/ponto/tests/test_o280_bateria_da_lei.py:167`, `app/ponto/models.py:332`, `:419`; execução no pai [não verificado])
- **Não identifiquei novo juiz da janela:** motor chama `he_noturna`; os acréscimos FM/DP apenas somam o campo já preenchido. O calculador existente chama essa mesma função por período. (`app/ponto/motor_calculo_v2.py:1014`, `app/ponto/services/fechamento.py:961`, `app/ponto/services/dia_pago.py:252`, `app/ponto/calculador/regras.py:824`)
- **Não identifiquei novo escritor fora da porta no diff:** FM continua na função autorizada pelo registro e persiste por `save`/`update` no mesmo serviço; os campos novos do DiaPago continuam em `lavrar`. (`app/core/portas.py:500`, `app/ponto/services/fechamento.py:1143`, `:1159`, `:1168`, `app/ponto/services/dia_pago.py:252`)
- **Não há relógio novo no diff fornecido:** a leitura do instante da passada já fica no serviço de fechamento, e as somas acrescentadas não consultam o relógio. (`app/ponto/services/fechamento.py:616`, `:961`; `app/ponto/services/dia_pago.py:252`)

## 06/11 21:47 · sessao abecf1c1d59d · FERIAS-CANCELAR-2X · verificador · origem=Code

**Parecer: não certificaria a obra ainda.** A guarda protege duas chamadas de cancelamento entre si, mas outro escritor pode ressuscitar `cancelado` e permitir nova devolução. Há também uma inversão de acesso a linhas com a remarcação e lacunas na mordida dos seis testes. (`app/ferias/services.py:347`, `app/ferias/services.py:361`, `app/ferias/services.py:420`, `app/ferias/services.py:548`, `app/ferias/test_cancelar_2x.py:74`, `app/ferias/test_cancelar_2x.py:82`.)

Execução dos testes nesta cópia, RED real contra `19aeede1`, concorrência no PostgreSQL, estado efetivo de produção e confirmação independente do hash: **[não verificado]**.

1. **Quem escreve e quem lê**

O censo abaixo reúne os sítios de aplicação encontrados; chamadas dinâmicas, SQL externo e completude integral das fixtures/asserts de toda a suíte: **[não verificado]**. [nao verificado]

| Campo | Escritores |
|---|---|
| `AgendamentoFerias.status` | Default `agendado`: `app/ferias/models.py:309`, utilizado nas criações em `app/ferias/services.py:321` e `app/ferias/services.py:467`. Histórico deriva estado em `app/ferias/services.py:282`. Canceladores escrevem em `app/ferias/services.py:365` e `app/ferias/services.py:393`. Correção/remarcação escreve em `app/ferias/services.py:417`, persistindo em `app/ferias/services.py:425`. Cron escreve por `app/ferias/services.py:551`, chamado em `app/ferias/management/commands/sincronizar_status_ferias.py:49`. |
| `PeriodoAquisitivo.dias_concedidos` | Default em `app/ferias/models.py:125`; entradas de criação em `app/ferias/views.py:177` e `app/ferias/views.py:185`, materializadas em `app/ferias/services.py:259`. Histórico soma com teto em `app/ferias/services.py:299`; agendamento soma em `app/ferias/services.py:324`; DP subtrai em `app/ferias/services.py:362`; admin subtrai em `app/ferias/services.py:385`; remarcação aplica delta em `app/ferias/services.py:411`; aprovação de solicitação soma em `app/ferias/services.py:478`. |
| `AgendamentoFerias.obs` | Histórico em `app/ferias/services.py:284`; observação recebida no agendamento em `app/ferias/services.py:323`; lote fornece texto em `app/ferias/views.py:632`; cancelamento admin acrescenta carimbo em `app/ferias/services.py:392`; aprovação copia origem/observação da solicitação em `app/ferias/services.py:474`. |

As definições iniciais desses campos também estão na migration: `app/ferias/migrations/0001_initial.py:47`, `app/ferias/migrations/0001_initial.py:68`, `app/ferias/migrations/0001_initial.py:69`. O `save` do agendamento persiste e sincroniza ausências, sem impor uma transição terminal para `cancelado`. (`app/ferias/models.py:434`.)

Leitores de **`AgendamentoFerias.status`**:

| Sítio | Leituras |
|---|---|
| Modelo | Exclusão de período, início, permissões de edição/cancelamento/correção, fracionamento e guarda de sobreposição: `app/ferias/models.py:243`, `app/ferias/models.py:324`, `app/ferias/models.py:328`, `app/ferias/models.py:332`, `app/ferias/models.py:339`, `app/ferias/models.py:346`, `app/ferias/models.py:355`, `app/ferias/models.py:394`. |
| Serviços | Janela, divergências, próximos agendamentos, duplicata, recusa e alinhamento: `app/ferias/services.py:51`, `app/ferias/services.py:78`, `app/ferias/services.py:80`, `app/ferias/services.py:82`, `app/ferias/services.py:102`, `app/ferias/services.py:318`, `app/ferias/services.py:348`, `app/ferias/services.py:550`. |
| Views: listagem e validação | `app/ferias/views.py:26`, `app/ferias/views.py:27`, `app/ferias/views.py:32`, `app/ferias/views.py:72`, `app/ferias/views.py:76`, `app/ferias/views.py:78`, `app/ferias/views.py:80`, `app/ferias/views.py:82`, `app/ferias/views.py:235`. |
| Views: ações e auditoria | Propriedades/estado e antes/depois: `app/ferias/views.py:364`, `app/ferias/views.py:366`, `app/ferias/views.py:379`, `app/ferias/views.py:403`, `app/ferias/views.py:411`, `app/ferias/views.py:423`, `app/ferias/views.py:462`, `app/ferias/views.py:482`, `app/ferias/views.py:512`, `app/ferias/views.py:539`, `app/ferias/views.py:566`, `app/ferias/views.py:663`, `app/ferias/views.py:828`, `app/ferias/views.py:852`. |
| Situação e ausências | `app/ferias/situacao.py:119`, `app/ferias/situacao.py:166`, `app/ferias/situacao.py:271`, `app/ferias/situacao.py:346`, `app/ponto/services/ausencia.py:1180`. |
| Commands | `app/ferias/management/commands/sincronizar_status_ferias.py:34`, `app/ferias/management/commands/sincronizar_status_ferias.py:39`, `app/ferias/management/commands/sincronizar_status_ferias.py:43`, `app/ferias/management/commands/alertar_retornos.py:21`. |
| Outros módulos | API: `app/api/views.py:2262`, `app/api/views.py:2268`; mensageria: `app/api/views_mensageria.py:842`, `app/api/views_mensageria.py:943`, `app/api/views_mensageria.py:1095`; disponibilidade: `app/colaboradores/views.py:2844`. |
| Inteligência | Consulta literal `aprovado`, ausente das choices do agendamento: `app/inteligencia/views.py:94`, `app/inteligencia/views.py:278`, `app/inteligencia/views.py:286`, `app/ferias/models.py:293`. |
| Templates | `app/templates/ferias/_tabela_ferias.html:42`; `app/templates/ferias/cancelar_admin_massa.html:45`; `app/templates/ferias/saldo.html:63`, `app/templates/ferias/saldo.html:64`, `app/templates/ferias/saldo.html:65`, `app/templates/ferias/saldo.html:66`, `app/templates/ferias/saldo.html:70`; `app/templates/ferias/_secao_colab.html:120`, `app/templates/ferias/_secao_colab.html:124`, `app/templates/ferias/_secao_colab.html:140`, `app/templates/ferias/_secao_colab.html:141`, `app/templates/ferias/_secao_colab.html:143`, `app/templates/ferias/_secao_colab.html:175`, `app/templates/ferias/_secao_colab.html:176`. |

Leitores de **`dias_concedidos`**, além das operações de leitura/modificação/escrita já enumeradas:

| Sítio | Leituras |
|---|---|
| Modelo | Estado do período e saldo: `app/ferias/models.py:95`, `app/ferias/models.py:217`. O saldo alimenta remarcação, agendamento e venda: `app/ferias/models.py:222`, `app/ferias/models.py:231`, `app/ferias/models.py:239`. |
| Views | Filtros: `app/ferias/views.py:31`, `app/ferias/views.py:60`; auditoria: `app/ferias/views.py:368`, `app/ferias/views.py:380`, `app/ferias/views.py:413`, `app/ferias/views.py:424`, `app/ferias/views.py:483`, `app/ferias/views.py:513`; validação do direito: `app/ferias/views.py:804`, `app/ferias/views.py:805`. |
| Situação, relatório e telas | `app/ferias/situacao.py:182`, `app/relatorios/services.py:290`, `app/templates/ferias/saldo.html:37`, `app/templates/ferias/_secao_colab.html:49`. |

Os leitores **indiretos pelo saldo** incluem validação/projeção em `app/ferias/services.py:200`, `app/ferias/services.py:202`, `app/ferias/services.py:205`, `app/ferias/services.py:463`, `app/ferias/services.py:464`, `app/ferias/views.py:233`, `app/ferias/views.py:234`, `app/ferias/views.py:675`, `app/ferias/views.py:934`, `app/ferias/views.py:935`; avisos e totalização em `app/ferias/situacao.py:69`, `app/ferias/situacao.py:178`, `app/ferias/situacao.py:193`, `app/ferias/situacao.py:207`, `app/ferias/situacao.py:296`, `app/ferias/situacao.py:297`; commands em `app/ferias/management/commands/alertar_ferias_vencendo.py:36`, `app/ferias/management/commands/alertar_ferias_vencendo.py:41`, `app/ferias/management/commands/pautas_de_ferias.py:97`.

Também consomem saldo o PDF/CSV, mensageria e intake: `app/relatorios/services.py:316`, `app/relatorios/views.py:370`, `app/api/views_mensageria.py:838`, `app/api/views_mensageria.py:952`, `app/api/views_mensageria.py:1101`, `app/core/views_ia.py:262`, `app/core/views_ia.py:269`, `app/core/views_ia.py:492`, `app/core/views_ia.py:503`, `app/core/views_ia.py:532`.

Nos templates, o saldo aparece em `app/templates/ferias/saldo.html:41`, `app/templates/ferias/saldo.html:46`, `app/templates/ferias/_secao_colab.html:50`, `app/templates/ferias/_secao_colab.html:67`, `app/templates/ferias/_secao_colab.html:68`, `app/templates/ferias/_secao_colab.html:77`, `app/templates/ferias/_tabela_ferias.html:27`, `app/templates/ferias/agendar.html:7`, `app/templates/ferias/agendar.html:22`, `app/templates/ferias/agendar_lote.html:40`, `app/templates/relatorios/ferias_vencendo.html:48`.

A leitura explícita de **`AgendamentoFerias.obs`** na aplicação encontrada é a concatenação que preserva o texto anterior no cancelamento admin. `solic.obs` é outro campo, de outro modelo. (`app/ferias/services.py:392`, `app/ferias/services.py:474`.) Há leitores de teste em `app/ferias/test_cancelar_2x.py:48`, `app/ferias/test_agendar_lote.py:78`, `app/ferias/test_canc_admin_massa.py:87`, `app/ferias/test_remarcar_limpa_janela_f346.py:117`, e escrita de teste em `app/ferias/test_elo_ausencia.py:47`.

2. **Chamadores, exceções e falso sucesso**

| Caminho encontrado | Tratamento |
|---|---|
| DP | Chama em `app/ferias/views.py:330`; captura especificamente em `app/ferias/views.py:331`; retorna erro antes do sucesso em `app/ferias/views.py:336`, `app/ferias/views.py:338`, `app/ferias/views.py:339`. |
| Admin individual | Chama em `app/ferias/views.py:370`; captura em `app/ferias/views.py:371`; auditoria só vem depois, em `app/ferias/views.py:375`. |
| Corrigir concluída | Chama em `app/ferias/views.py:415`; captura em `app/ferias/views.py:416`; auditoria posterior em `app/ferias/views.py:420`. |
| Massa | Chama em `app/ferias/views.py:570`; recusa vira `pulado` e `continue` em `app/ferias/views.py:571`; não soma execução nem dias em `app/ferias/views.py:574`. |
| Wrapper admin | Apenas delega: `app/ferias/views.py:522`, `app/ferias/views.py:526`. |

**Há mensagem de nível sucesso mesmo quando todos os itens forem recusados/pulados.** Ela informa os números reais — inclusive zero cancelamentos e zero dias —, portanto não credita devolução ao recusado, mas mantém `messages.success`. (`app/ferias/views.py:576`.)

Nos caminhos acima não há `except Exception` engolindo `AgendamentoNaoCancelavel`; os genéricos estão nas segundas chamadas dos testes. (`app/ferias/views.py:331`, `app/ferias/views.py:371`, `app/ferias/views.py:416`, `app/ferias/views.py:571`, `app/ferias/test_cancelar_2x.py:57`, `app/ferias/test_cancelar_2x.py:66`, `app/ferias/test_cancelar_2x.py:95`.)

Nenhum chamador adicional em API, command, signal ou admin Django foi localizado; exaustividade contra chamadas dinâmicas: **[não verificado]**. O admin de férias aberto registra somente `Afastamento`. (`app/ferias/admin.py:2`, `app/ferias/admin.py:4`.)

3. **`corrigir_concluida_admin` continua funcionando?**

**Sim, pela leitura.** A view exige `concluido` e justificativa de pelo menos cinco caracteres; o escritor admin recusa somente `cancelado`, preservando a passagem de `concluido`. (`app/ferias/models.py:346`, `app/ferias/views.py:403`, `app/ferias/views.py:406`, `app/ferias/services.py:384`.)

Existe teste anterior esperando cancelamento, saldo devolvido e justificativa na auditoria; execução dele: **[não verificado]**. (`app/ferias/test_corrigir_concluida.py:76`, `app/ferias/test_corrigir_concluida.py:82`, `app/ferias/test_corrigir_concluida.py:83`, `app/ferias/test_corrigir_concluida.py:84`.)

Resta uma brecha entre checagem e escrita: admin comum/massa julgam a foto anterior; se o estado virar `concluido` antes da trava, o escritor aceita sem exigir a justificativa da porta de concluída. (`app/ferias/views.py:364`, `app/ferias/views.py:566`, `app/ferias/services.py:384`.)

4. **A checagem retirada da view DP foi preservada?**

**Sim, para a coluna `status`.** O escritor recusa `concluido`, e a view mantém a mensagem literal “Ferias ja concluidas nao podem ser canceladas.” nos caminhos HTMX e redirect. (`app/ferias/services.py:361`, `app/ferias/views.py:333`, `app/ferias/views.py:336`, `app/ferias/views.py:338`.) Outro chamador dependente da antiga checagem não foi localizado; dependências dinâmicas: **[não verificado]**.

Isso não garante conclusão **pelas datas**: o módulo reconhece atraso do cron, mas a recusa usa somente a coluna. Um registro com fim passado e `status='agendado'` ainda passa pelo DP; é uma lacuna preexistente, não uma regressão demonstrada. (`app/ferias/services.py:17`, `app/ferias/services.py:57`, `app/ferias/services.py:361`.)

5. **Atomic e ordem de trava**

**Os dois chamadores do helper estão dentro do `atomic` próprio dos serviços.** A view não precisa abrir transação para esse `select_for_update`. (`app/ferias/services.py:360`, `app/ferias/services.py:361`, `app/ferias/services.py:380`, `app/ferias/services.py:384`.)

O perfil SaaS não define `ATOMIC_REQUESTS`; WSGI/manage apontam por padrão para ele. Configuração efetivamente carregada em produção: **[não verificado]**. (`app/config/settings/saas.py:83`, `app/config/wsgi.py:14`, `app/manage.py:9`.)

A ordem explícita é **agendamento → período**. Não localizei outro `select_for_update` desses objetos na ordem inversa; ausência global de deadlock: **[não verificado]**. (`app/ferias/services.py:347`, `app/ferias/services.py:350`.)

**Existe outra inversão relevante: agendamento ↔ ausência.** Cancelamento segura agendamento/período antes de atualizar ou apagar ausências; remarcação apaga ausências antes de atualizar agendamento. (`app/ferias/services.py:347`, `app/ferias/services.py:350`, `app/ferias/services.py:388`, `app/ponto/services/ausencia.py:1281`, `app/ferias/models.py:437`, `app/ponto/services/ausencia.py:1188`, `app/ferias/services.py:420`, `app/ferias/services.py:425`.)

Interleaving possível: remarcação segura ausência antiga; cancelamento segura agendamento e espera ausência; remarcação espera agendamento. Reprodução/detecção no PostgreSQL e atribuição exclusiva dessa classe ao patch: **[não verificado]**.

6. **Contraexemplos, testes e efeito nos leitores** [nao verificado]

- **Cron permite devolver novamente.** Cron carrega `agendado` que precisa virar `em_gozo`; cancelamento faz `30→0` e grava `cancelado`; cron grava `em_gozo` usando o objeto antigo; novo cancelamento faz `0→-30`. A exclusão de cancelados ocorre somente na consulta inicial; `alinhar_status_por_data` não relê nem protege o estado terminal. Reprodução concorrente: **[não verificado]**. (`app/ferias/management/commands/sincronizar_status_ferias.py:33`, `app/ferias/management/commands/sincronizar_status_ferias.py:38`, `app/ferias/management/commands/sincronizar_status_ferias.py:49`, `app/ferias/services.py:550`, `app/ferias/services.py:552`, `app/ferias/services.py:361`, `app/ferias/services.py:362`.)
- **Remarcação também pode ressuscitar cancelado.** Usa agendamento/período carregados anteriormente, calcula saldo dessa foto e regrava estado temporal sem reler sob trava. Reprodução concorrente: **[não verificado]**. (`app/ferias/services.py:408`, `app/ferias/services.py:411`, `app/ferias/services.py:417`, `app/ferias/services.py:425`, `app/ferias/services.py:427`.)
- **Histórico pode ficar negativo já no primeiro cancelamento.** O histórico guarda os dias integrais no agendamento, mas satura o agregado em `dias_direito`; cancelar subtrai os dias integrais. Exemplo estático: direito 30, histórico 40, agregado 30; cancelar dá `-10`. Ocorrência em produção: **[não verificado]**. (`app/ferias/services.py:281`, `app/ferias/services.py:299`, `app/ferias/services.py:362`, `app/ferias/services.py:385`.)

A alegação de **seis REDs mordendo o pai pelo motivo certo não está sustentada**:

| Testes | Avaliação estática | [nao verificado]
|---|---|
| Mesmo objeto, objeto relido, admin, POST | Asserções finais confrontam a dupla devolução; execução no pai: **[não verificado]**. (`app/ferias/test_cancelar_2x.py:59`, `app/ferias/test_cancelar_2x.py:68`, `app/ferias/test_cancelar_2x.py:97`, `app/ferias/test_cancelar_2x.py:114`.) |
| Recusa explícita e concluído | No pai sem a classe nova, param em `assertIsNotNone`, antes da chamada que deveria demonstrar o comportamento errado. (`app/ferias/test_cancelar_2x.py:74`, `app/ferias/test_cancelar_2x.py:76`, `app/ferias/test_cancelar_2x.py:82`, `app/ferias/test_cancelar_2x.py:84`.) |

Três testes aceitam qualquer exceção; “sem escrita” observa apenas saldo/status/obs, sem detectar UPDATE idêntico ou efeitos em ausências/auditoria. Todos são sequenciais e herdam `TestCase`, sem provar disputa entre conexões. (`app/ferias/test_cancelar_2x.py:57`, `app/ferias/test_cancelar_2x.py:66`, `app/ferias/test_cancelar_2x.py:95`, `app/ferias/test_cancelar_2x.py:48`, `app/ferias/test_cancelar_2x.py:25`.)

O saldo inflado chega a telas, validações, relatórios e intake pelos leitores enumerados. Ressuscitar status também rematerializa ausências e recoloca o agendamento no gate de batida e no cruzamento que retém TXT da folha. (`app/ferias/models.py:217`, `app/ferias/views.py:233`, `app/ferias/models.py:437`, `app/ponto/services/ausencia.py:1180`, `app/ponto/utils.py:89`, `app/folha/export.py:491`, `app/ferias/services.py:51`.) Efeito monetário em holerite ou quantidade de pessoas afetadas: **[não verificado]**.

Quanto ao restante do checklist:

- **Juiz duplicado:** o patch continua delegando estado do período a `aplicar_status`; as autoridades registradas de saldo/vencimento/abono/fracionamento permanecem as mesmas. (`app/ferias/services.py:363`, `app/ferias/services.py:386`, `app/ferias/models.py:212`, `app/core/juizes.py:252`, `app/core/juizes.py:253`, `app/core/juizes.py:254`, `app/core/juizes.py:255`.)
- **Escritor fora da porta:** os escritores encontrados estão no módulo autorizado, mas o selo verifica localização, não coordenação entre escritores do mesmo módulo. (`app/core/portas.py:488`, `app/core/portas.py:489`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:35`.)
- **Relógio fora da borda:** permanece no carimbo admin e no cálculo implícito de estado; é código preexistente. (`app/ferias/services.py:391`, `app/ferias/services.py:363`, `app/ferias/services.py:386`, `app/ferias/models.py:94`.)
- **Semântica:** `status` reúne progressão temporal e cancelamento humano; `dias_concedidos` reúne reservado/gozado e sofre saturação no histórico; `obs` reúne texto livre, origem e carimbo administrativo. (`app/ferias/models.py:273`, `app/ferias/models.py:278`, `app/templates/ferias/_secao_colab.html:49`, `app/ferias/services.py:299`, `app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:392`, `app/ferias/services.py:474`.)
- **Pouso:** RED evidenciado, GREEN, suíte inteira e leitura final são pré-requisitos documentados. Cumprimento nesta obra: **[não verificado]**. (`CLAUDE.md:190`, `CLAUDE.md:369`, `CLAUDE.md:372`.) [nao verificado]

## 06/11 21:44 · sessao cb5df36716d8 · L-123-MEDIDA-EM-PY · censo · origem=Code

Numerei os anexos pelas linhas do texto fornecido, incluindo linhas vazias, sem contar os delimitadores `== anexo ==`.

1. **Comentário livre falsifica o handoff — (a), especificamente na variante por comentário.** O Codex identificou o leitor e o ataque pela docstring, mas não apontou o comentário como veículo remanescente. (`v3.r:28`; `opus_final.txt:2`)

   **Pergunta exata:** “Ha caminho em que um commit com n/a (medida) muda COMPORTAMENTO (calculo, dinheiro, batida, ou o que os leitores do placar decidem) e o selo novo aceita?” (`v1.q:3`). Comentários também estavam expressamente no mecanismo apresentado: “docstring do modulo, comentario/linha em branco fora de no preso”. (`v2.q:2`) [nao verificado]

   **Resposta exata:** “Inserir `contratos_estruturais: 0/999 verdes` no início da docstring do módulo”; “O handoff muda sua leitura de **19/20** para **0/999**, porque pesquisa o primeiro casamento no texto bruto inteiro.” (`v3.r:28`)

   A triagem registrou como **procedente**, não descartado: “docstring mascarada (handoff_sessao.sh:179 le o placar nela)”. Portanto, não há suporte para atribuir a sobrevivência por comentário a um descarte da triagem. (`triagem_v3.txt:2`). A leitura do arquivo inteiro está confirmada na cópia. ([bin/handoff_sessao.sh:179](/home/ronald/codex_ws/19aeede118ae6d3ec12c85eb81d44a38ab7c7cc0/bin/handoff_sessao.sh:179))

2. **Prosa falsa no comentário e no `quem-mais-le` — (e), para essas afirmações específicas.** Não aparece nas perguntas fornecidas uma solicitação de auditar a veracidade dessas duas frases do resultado final. (`v1.q:1–3`; `v2.q:1–3`; `v3.q:1–22`; `opus_final.txt:3`)

   **Pergunta exata que delimita a última revisão:** “A arvore julgada e o hash passado (copia do commit fd7fc252).” (`v3.q:2`). A triagem registra a subida da volta 4 “sem nova volta Codex”. (`triagem_v3.txt:4`)

   **Resposta exata relacionada ao fato subjacente:** “porque pesquisa o primeiro casamento no texto bruto inteiro”. (`v3.r:28`)

   Assim, o Codex conhecia a leitura integral; trecho `.q` pedindo auditoria dessas duas afirmações posteriores e trecho `.r` denunciando-as: **[não verificado]**. Atribuir o achado a desconhecimento do leitor contrariaria a resposta registrada. (`v3.r:28`)

3. **Ampliação de `DADO_MEDIDO`, inclusive retroativa — visto; descarte correspondente [não verificado]. Não há classe integral comprovada na lista fornecida.** O núcleo do mecanismo já foi explicitamente apontado. (`v2.r:34`; `v3.r:94`)

   **Pergunta exata:** “A leitura dos NOMES no ALVO (e nao no commit julgado) abre algo alem do retroativo de dado puro declarado?” (`v2.q:3`) [nao verificado]

   **Resposta exata:** “Assim, um ALVO que liste `CADENCIA_TERMOMETRO_DIAS` pode autorizar retroativamente sua troca literal.” (`v2.r:34`)

   A volta 3 também afirmou: “Não há validação de que cada chave declarada tenha significado de medição: a validação exige strings e estrutura sintática.” (`v3.r:94`)

   **O descarte documentado é mais restrito:** “(2) fonte_do_medido, fonte, nota_do_papel declaradas como medido -- o aval exige LITERAL os 3 commits R1 (36c90132, 6ed6adbe, a16637ca) verdes, e eles mudam exatamente essas chaves (medido: so valores str, num caminho unico).” (`triagem_v3.txt:3`)

   Essa linha sustenta **(b) para aquelas chaves concretas**; não registra descarte da ampliação arbitrária da declaração para autorizar decisões retroativamente. Portanto, chamar o mecanismo geral de “não visto” seria incorreto; atribuir seu descarte à sessão também excederia o registro. (`v2.r:34`; `triagem_v3.txt:3`)

   O subtema de arquivo novo também foi perguntado — “arquivo novo que se declara medida” — e respondido — “**Aceita se todas as linhas forem livres**”. (`v1.q:3`; `v1.r:17`)

4. **Itens 3–4 do contrato desatualizados — (a), considerando o critério da casa de tema presente.** Contrato e cobertura das cláusulas estavam no escopo; a obsolescência documental desses itens não foi apontada nas respostas fornecidas. (`v1.q:1–3`; `v2.q:1–3`; `v1.r`; `v2.r`; `v3.r`; `opus_final.txt:5`)

   **Perguntas exatas:** “o CONTRATO de 5 linhas e o texto do aval”; “O MORDE morde de verdade cada clausula (cite qual caso mata qual mutante) e falta caso?” (`v1.q:1`; `v1.q:3`)

   **Resposta exata sobre uma cláusula contratual:** “Recusam a concessão, mas não cumprem literalmente ‘erro Git = PADRAO_ERRO’.” (`v2.r:62`)

   **Resposta exata sobre a verificação prometida:** “Execução integral do MORDE e dos dois mutantes vermelhos prometidos: **[não verificado]**.” (`v2.r:64`)

   As respostas examinaram conformidade e cobertura, mas não registraram que o texto dos itens 3–4 precisava acompanhar a implementação. A ausência de execução indicada na resposta não transforma a revisão documental em dependência de RED, suíte ou produção. (`contrato.txt:1`; `v2.r:40–64`; `v3.r:72–88`) [nao verificado]

## 06/11 21:44 · sessao 372b548f765b · - · - · origem=Ronald (tmux)

Todos os chamadores de produção encontrados tratam a exceção nova, e os dois escritores abrem seu próprio `atomic`; a segurança contra outros escritores concorrentes, porém, continua incompleta. (`app/ferias/views.py:330`, `app/ferias/views.py:371`, `app/ferias/views.py:416`, `app/ferias/views.py:571`, `app/ferias/services.py:360`, `app/ferias/services.py:380`.)

- **DP:** único chamador de produção encontrado é `cancelar_agendamento`, com captura específica em `331`; devolve erro tanto HTMX quanto redirect, e sucesso fica depois do `except`. (`app/ferias/views.py:328`, `app/ferias/views.py:331`, `app/ferias/views.py:336`, `app/ferias/views.py:339`.)
- **Admin:** wrapper `522–526` apenas delega; chamadores são cancelamento individual, correção de concluída e massa; todos capturam `AgendamentoNaoCancelavel` antes da trilha ou contagem de sucesso. (`app/ferias/views.py:522`, `app/ferias/views.py:370`, `app/ferias/views.py:375`, `app/ferias/views.py:415`, `app/ferias/views.py:420`, `app/ferias/views.py:570`.)
- **Massa:** recusa incrementa `pulados`, segue com `continue` e não incrementa `feitos/devolvidos`; entretanto a mensagem final tem nível `success` mesmo se todos forem pulados — com números explícitos, sem atribuir dias aos recusados. (`app/ferias/views.py:571`, `app/ferias/views.py:574`, `app/ferias/views.py:576`.)
- **API/command/signal/admin Django:** nenhuma referência aos escritores de cancelamento encontrada na busca global; exaustividade contra chamadas dinâmicas é **[não verificado]**. O admin de férias aberto registra somente `Afastamento`; não existe `ferias/signals.py` nesta cópia. (`app/ferias/admin.py:1`, `app/ferias/admin.py:4`; ausência de arquivo **[não verificado]**.)
- Não há `except Exception` envolvendo as chamadas de cancelamento nas views abertas; os genéricos dos seis testes existem nas segundas chamadas, inclusive no teste admin. (`app/ferias/views.py:329`, `app/ferias/views.py:369`, `app/ferias/views.py:414`, `app/ferias/views.py:569`, `app/ferias/test_cancelar_2x.py:57`, `app/ferias/test_cancelar_2x.py:66`, `app/ferias/test_cancelar_2x.py:95`.)

**Atomic e ordem de trava:** o helper tem exatamente dois chamadores, ambos dentro do atomic próprio; não depende de a view estar em transação. (`app/ferias/services.py:360`, `app/ferias/services.py:361`, `app/ferias/services.py:380`, `app/ferias/services.py:384`.) `DATABASES` de SaaS não define `ATOMIC_REQUESTS`, e o WSGI/manage defaultam para esse perfil; configuração efetivamente carregada em produção é **[não verificado]**. (`app/config/settings/saas.py:83`, `app/config/wsgi.py:14`, `app/manage.py:9`.) O helper trava primeiro agendamento e depois período. (`app/ferias/services.py:347`, `app/ferias/services.py:350`.) Nenhum outro `select_for_update` de PA→agendamento foi localizado; garantia global de ausência de deadlock é **[não verificado]**.

Há uma inversão concreta **agendamento↔ausência** no código: cancelamento trava agendamento, depois período, e admin atualiza ausências; a correção/remarcação de datas deleta ausências antes de atualizar agendamento. (`app/ferias/services.py:347`, `app/ferias/services.py:350`, `app/ferias/services.py:388`, `app/ponto/services/ausencia.py:1281`, `app/ferias/services.py:420`, `app/ferias/services.py:425`, `app/ponto/services/ausencia.py:1266`.) Interleaving possível: remarcação segura ausência antiga; cancelamento segura agendamento e espera ausência; remarcação espera agendamento no save. Execução PostgreSQL e detecção de deadlock **[não verificado]**. Não atribuir exclusivamente ao patch: o código anterior do anexo também atualizava período antes de ausência/agendamento, enquanto a remarcação termina atualizando período. (`app/ferias/services.py:427`; versão anterior, `diff.patch` fornecido, trecho `executar_cancelamento_admin` **[não verificado: sem linhas de arquivo local]**.)

**Contraexemplo forte à proteção global:** cron carrega agendamento `agendado` com datas que exigem `em_gozo`; DP cancela e reduz `30→0`; cron escreve `em_gozo` usando o objeto antigo; DP cancela novamente e reduz `0→-30`. O cron exclui cancelados somente na consulta inicial, e `alinhar_status_por_data` não relê nem trava o status antes do save. (`app/ferias/management/commands/sincronizar_status_ferias.py:33`, `app/ferias/management/commands/sincronizar_status_ferias.py:37`, `app/ferias/management/commands/sincronizar_status_ferias.py:49`, `app/ferias/services.py:548`, `app/ferias/services.py:550`, `app/ferias/services.py:552`, `app/ferias/services.py:361`, `app/ferias/services.py:362`.) A nova guarda serializa canceladores, mas outro escritor pode ressuscitar o estado; reprodução runtime **[não verificado]**. O teste “cancelado nunca é tocado” começa com o cancelado já persistido antes do cron, não cobre objeto carregado antes de cancelamento concorrente. (`app/ferias/test_sync_status.py:41`, `app/ferias/test_sync_status.py:65`.)

**Concluídas:** a correção continua exigindo propriedade específica e justificativa de cinco caracteres, chama o escritor admin, e registra justificativa na trilha; o escritor recusa somente `cancelado`, preservando `concluido`. (`app/ferias/views.py:403`, `app/ferias/views.py:406`, `app/ferias/views.py:415`, `app/ferias/views.py:420`, `app/ferias/services.py:384`.) Há teste existente com concluída que espera saldo zero, cancelamento e justificativa no log; execução desse teste **[não verificado]**. (`app/ferias/test_corrigir_concluida.py:76`, `app/ferias/test_corrigir_concluida.py:82`, `app/ferias/test_corrigir_concluida.py:84`.)

A view DP mantém exatamente a mensagem fornecida para `concluido`, agora escolhida pelo status da exceção do escritor; cobre HTMX e redirect. (`app/ferias/services.py:361`, `app/ferias/views.py:333`, `app/ferias/views.py:336`, `app/ferias/views.py:338`.) Não foi localizado outro chamador de produção dependente da guarda antiga; dependências dinâmicas **[não verificado]**. A porta admin comum continua julgando `pode_cancelar_admin` antes de chamar; portanto um status que mude para `concluido` entre a guarda e a trava ainda entra pelo escritor que recusa somente cancelado, sem passar pela justificativa da porta de concluída. (`app/ferias/views.py:364`, `app/ferias/models.py:339`, `app/ferias/services.py:384`.)

## 06/11 21:44 · sessao 5a4e2d91d110 · - · - · origem=Ronald (tmux)

Censo estático de produção na cópia, sem executar nem escrever arquivos. A completude em código dinâmico e a ocorrência em produção ficam [não verificado].

**Escritores**

| Campo | Todos os escritores de aplicação encontrados |
|---|---|
| `AgendamentoFerias.status` | Default `agendado`: `app/ferias/models.py:309`, usado em criação por `executar_agendamento` (`app/ferias/services.py:321`) e `aprovar_solicitacao` (`app/ferias/services.py:467`). Gozo histórico deriva por `status_por_data`: `app/ferias/services.py:282`. DP cancela: `app/ferias/services.py:365`. Admin cancela: `app/ferias/services.py:393`. Remarcação/correção de datas rederiva: `app/ferias/services.py:417`, grava em `:425`, portas em `:431` e `:436`. Cron grava pela porta `alinhar_status_por_data`: `app/ferias/services.py:550`, `:551`, `:552`, chamado por `app/ferias/management/commands/sincronizar_status_ferias.py:48`. |
| `PeriodoAquisitivo.dias_concedidos` | Default zero: `app/ferias/models.py:125`; defaults de criação na view: `app/ferias/views.py:177`, `:185`, encaminhados a `garantir_periodo_aquisitivo` (`app/ferias/services.py:259`). Histórico incrementa com teto: `app/ferias/services.py:299`, `:302`. Agendamento incrementa: `:324`, `:326`. DP subtrai: `:362`, `:364`. Admin subtrai: `:385`, `:387`. Troca de datas faz delta: `:411`, `:427`. Aprovar solicitação incrementa: `:478`, `:480`. |
| `AgendamentoFerias.obs` | Campo livre: `app/ferias/models.py:313`. Histórico: `app/ferias/services.py:284`; agendamento recebe `obs`: `:323`; lote fornece carimbo de lote: `app/ferias/views.py:632`; admin acrescenta carimbo: `app/ferias/services.py:389`, `:392`, `:394`; aprovação materializa texto da solicitação: `:474`. |

O `save` do modelo persiste o que recebe e sincroniza Ausência (`app/ferias/models.py:434`), sem impor transição terminal. No admin Django deste app só há registro de `Afastamento` (`app/ferias/admin.py:2`, `:4`). Não encontrei escritor desses três campos em `bin`, `scripts`, `ops` ou `mensageria`; ocorrência por SQL/dinâmica não identificada: [não verificado].

**Leitores diretos de `AgendamentoFerias.status`**

- Modelos: `PeriodoAquisitivo.pode_excluir` (`app/ferias/models.py:243`); `ja_iniciou` (`:324`), `pode_editar` (`:328`), `pode_cancelar` (`:332`), `pode_cancelar_admin` (`:339`), `pode_corrigir_concluida` (`:346`), fracionamento (`:355`), guarda de sobreposição (`:394`).
- Serviços: janela de férias (`app/ferias/services.py:51`); divergências (`:78`, `:80`, `:82`); começam entre (`:102`); duplicata (`:318`); recusa sob trava (`:348`); cron (`:550`).
- Views férias: KPIs (`app/ferias/views.py:26`, `:27`, `:32`); lista (`:72`, `:76`, `:78`, `:80`); validação (`:235`); auditoria antes/depois (`:366`, `:379`, `:411`, `:423`, `:482`, `:512`); correção de datas (`:462`); edição (`:663`); exclusão de período (`:828`); pedir confirmação (`:852`). Cancelamento admin/lote usam as propriedades do modelo (`app/ferias/views.py:363`, `:565`).
- Situação: frações (`app/ferias/situacao.py:119`), vivos (`:166`), férias com batida (`:271`), colisão (`:346`).
- Materialização de ausência: `app/ponto/services/ausencia.py:1180`.
- Cron de status: `app/ferias/management/commands/sincronizar_status_ferias.py:34`, `:39`, `:43`; retorno: `app/ferias/management/commands/alertar_retornos.py:21`.
- API colaborador: `app/api/views.py:2262`, `:2268`; disponibilidade/voltando: `app/colaboradores/views.py:2844`.
- Inteligência tem leitor incorreto preexistente de literal `aprovado`, ausente nas choices: `app/inteligencia/views.py:94`, `:278`, `:286`; choices reais: `app/ferias/models.py:293`.
- Templates: `app/templates/ferias/_tabela_ferias.html:42`; `cancelar_admin_massa.html:45`; `saldo.html:63`, `:64`, `:65`, `:66`, `:70`; `_secao_colab.html:120`, `:124`, `:140`, `:141`, `:143`, `:175`, `:176`.

**Leitores de `dias_concedidos`**

- Conta de status: `app/ferias/models.py:95`; autoridade do saldo: `:217`. Leitores indiretos são todas as propriedades/consumidores de `dias_saldo`, incluindo remarcação (`:222`), pode agendar (`:231`), pode vender (`:239`).
- Read-modify-write nos escritores enumerados acima: `app/ferias/services.py:299`, `:324`, `:362`, `:385`, `:411`, `:478`.
- Filtros de sem agendamento: `app/ferias/views.py:31`, `:60`; auditoria: `:368`, `:380`, `:413`, `:424`, `:483`, `:513`; validação de direito: `:804`, `:805`.
- Aviso de cadastro inconsistente: `app/ferias/situacao.py:182`; relatório PDF: filtro `app/relatorios/services.py:290`, saldo `:316`.
- Templates diretos: `app/templates/ferias/saldo.html:37`; `app/templates/ferias/_secao_colab.html:49`.
- Templates de saldo derivados: `app/templates/ferias/saldo.html:41`, `:46`; `_secao_colab.html:50`, `:67`, `:68`, `:77`; `_tabela_ferias.html:27`; `agendar.html:7`, `:22`; `agendar_lote.html:40`. Serviços de validação também leem a autoridade: `app/ferias/services.py:200`, `:202`, `:205`, `:463`, `:464`; `app/ferias/views.py:233`, `:234`, `:934`, `:935`.

**Leitores de `AgendamentoFerias.obs`**

- Aplicação faz leitura explícita para preservar e acrescentar carimbo somente no cancelamento admin: `app/ferias/services.py:392`. A leitura `solic.obs` de `:474` pertence a `SolicitacaoFerias`, outro campo/modelo.
- Testes leem o campo de agendamento: `app/ferias/test_cancelar_2x.py:48`; `app/ferias/test_agendar_lote.py:78`; `app/ferias/test_canc_admin_massa.py:87`; `app/ferias/test_remarcar_limpa_janela_f346.py:117`; teste modifica observação em `app/ferias/test_elo_ausencia.py:47`.
- Inputs de observação: `app/templates/ferias/_secao_colab.html:70`, `app/templates/ferias/agendar.html:27`. Não encontrei exibição do `ag.obs` nos templates pesquisados; consumidor dinâmico não identificado: [não verificado].

**Contra-exemplos importantes para consolidar**

1. **Cron pode ressuscitar cancelado, permitindo nova devolução.** O cron lê ativos sem trava (`app/ferias/management/commands/sincronizar_status_ferias.py:33`) e passa objeto antigo à porta (`:49`). A porta compara e grava o status antigo sem reler nem preservar `cancelado` (`app/ferias/services.py:550`, `:552`). Interleaving estático: cron lê `agendado` que precisa virar `em_gozo`; cancelamento grava `cancelado` e saldo zero; cron grava `em_gozo`; próximo cancelamento aceita e chega a `-30` (`app/ferias/services.py:361`, `:362`). Execução concorrente real: [não verificado].

2. **Remarcação/correção pode ressuscitar cancelado.** A view faz guarda antes da escrita (`app/ferias/views.py:462`, `:663`), mas `_trocar_janela` usa objeto antigo, calcula saldo antigo e grava status ativo sem trava/releitura (`app/ferias/services.py:408`, `:411`, `:417`, `:425`, `:427`). Corrida real: [não verificado].

3. **Primeiro cancelamento já pode ficar negativo por semântica incompatível com histórico.** Histórico registra `ag.dias` integral e limita o agregado com `min(direito, concedidos + dias)` (`app/ferias/services.py:281`, `:299`), enquanto cancelamento subtrai os dias integrais (`:362`, `:385`). Exemplo estático: 40 dias históricos, direito 30, concedidos 30; cancelar uma vez dá concedidos `-10`. Ocorrência em produção: [não verificado].

4. **Semântica existente:** `status` mistura progressão por data e decisão humana, explicitamente separadas pelo juiz (`app/ferias/models.py:273`, `:278`). `dias_concedidos` agrega reservado e gozado, como a própria UI informa (`app/templates/ferias/_secao_colab.html:49`), enquanto o histórico aplica teto (`app/ferias/services.py:299`). `obs` reúne observação livre, origem e trilha administrativa (`app/ferias/models.py:313`; `app/ferias/services.py:284`, `:323`, `:392`, `:474`).

5. **Impacto:** saldo inflado aparece em telas e valida agendamento; status ressuscitado rematerializa Ausências (`app/ferias/models.py:437`; `app/ponto/services/ausencia.py:1180`) e reaparece no gate de batida (`app/ponto/utils.py:89`) e retenção da folha (`app/folha/export.py:491`, via `app/ferias/services.py:51`). Folha consulta janela/status, não `dias_concedidos` diretamente nesses caminhos. Efeito monetário em holerite existente: [não verificado].

As autoridades de saldo, vencimento, abono e fracionamento continuam registradas em `app/core/juizes.py:252`, `:253`, `:254`, `:255`; a guarda nova de cancelamento não recalcula saldo nem substitui esses juízes (`app/ferias/services.py:342`, `:348`).

## 06/11 21:44 · sessao aed87e5089b1 · - · - · origem=Ronald (tmux)

**RED insuficiente para certificar os seis pelo motivo certo.**

- Quatro testes têm mordida estática no defeito de dupla devolução: saldo final em `app/ferias/test_cancelar_2x.py:59`, `:68`, `:97`, `:114`. Pelo diff fornecido, no pai a segunda chamada subtrairia novamente os 30 dias; execução real contra `19aeede1` **[não verificado]**.
- Dois testes falham antes de exercer a recusa: `getattr(..., None)` seguido de `assertIsNotNone` em `app/ferias/test_cancelar_2x.py:73-74` e `:81-82`. No pai, acusam ausência da classe nova, não demonstram comportamento incorreto do escritor. A chamada que deveria morder só vem depois, em `:76` e `:84`.
- Três testes aceitam qualquer exceção na segunda chamada, inclusive falha acidental de implementação: `app/ferias/test_cancelar_2x.py:57`, `:66`, `:95`. O teste específico de recusa DP reduz essa lacuna, mas não há equivalente administrativo tipado (`:70-77`, `:89-97`).
- “Sem escrita” compara somente três valores, não proíbe UPDATE idêntico nem observa ausência/pauta/auditoria: `app/ferias/test_cancelar_2x.py:45-48`, `:77`, `:97`.
- Não há prova de concorrência: todos herdam `TestCase` (`app/ferias/test_cancelar_2x.py:14`, `:25`) e executam chamadas sequenciais (`:53-56`, `:63-65`, `:90-94`, `:107-113`). `select_for_update`, espera, ordem de travas e duas conexões PostgreSQL **[não verificado]**.

**Contraexemplos fora do golden.**

1. **Objeto de remarcação obsoleto pode ressuscitar cancelado.** `_trocar_janela` usa período/agendamento já carregados (`app/ferias/services.py:408-409`), calcula saldo dessa foto (`:411`), reescreve status temporal (`:417`), grava agendamento (`:425`) e período (`:427`), sem reler status sob trava. Interleaving possível pela leitura: remarcação carrega → cancelamento conclui → remarcação grava foto antiga → novo cancelamento aceita novamente. Reprodução executada **[não verificado]**.
2. **Estado temporal atrasado permite cancelar férias já encerradas pelas datas.** O módulo reconhece atraso do status pelo cron (`app/ferias/services.py:17-19`, `:57-61`), mas o escritor DP testa somente coluna `status` (`:361`). Com `data_fim` passada e coluna ainda `agendado`, a recusa por concluído não ocorre. É lacuna preexistente, não regressão demonstrada; ocorrência em produção **[não verificado]**.
3. O teste de concluído novo fixa datas futuras e muda só `status`, portanto não cobre esse atraso temporal (`app/ferias/test_cancelar_2x.py:22`, `:40-41`, `:80`).

**Juízes e fonte única.**

- O patch continua delegando estado do período a `aplicar_status` (`app/ferias/services.py:363`, `:386`), que chama `status_de_periodo` (`app/ferias/models.py:212`). As autoridades registradas seguem `dias_saldo`, `status_de_periodo`, `dias_vendaveis` e `clean` (`app/core/juizes.py:252-255`). Não identifiquei novo juiz concorrente dessas perguntas nesse trecho.
- A fórmula de saldo permanece também dentro do juiz de status (`app/ferias/models.py:95`) e na property canônica (`:217`): duplicação já existente, não criada pelo diff.
- A porta declarada para essas entidades é `ferias/services.py`; o selo de escritores verifica módulo permitido, não serialização entre operações do mesmo módulo (`app/ponto/tests/test_chokepoint_ausencia_gate.py:29-39`, `:53-57`, `:65-68`). Por isso a corrida de `_trocar_janela` pode escapar mesmo com esse selo verde.
- A idempotência declarada pela matriz aponta para testes de espelho/alinhar estado, não para cancelamento (`app/core/contratos_estruturais.py:132`; `app/ponto/tests/test_porta_ausencia_ferias.py:83-135`).

**Relógio e semântica.**

- O relógio continua sendo lido dentro do escritor para carimbo administrativo (`app/ferias/services.py:390-391`). O cálculo de status também busca hoje internamente porque os canceladores chamam `aplicar_status()` sem data (`:363`, `:386`; `app/ferias/models.py:94`, `:212`). Isso já existia; o diff não cura a classe “relógio fora da borda”.
- `status` mistura estado temporal e decisão humana: `status_por_data` produz três estados e exclui explicitamente cancelamento humano (`app/ferias/models.py:273-285`). A guarda nova usa a coluna sob trava, não rederiva temporalidade (`app/ferias/services.py:347-349`).
- `dias_concedidos` reserva dias já no agendamento (`app/ferias/services.py:324`) e também contabiliza gozo histórico com saturação em `dias_direito` (`:299`). Cancelar subtrai os dias integrais (`:362`, `:385`): histórico saturado oferece outro cenário adversarial para saldo negativo.
- `obs` junta texto livre recebido no agendamento (`app/ferias/services.py:323`), marcador de registro histórico (`:284`) e carimbo de cancelamento (`:390-394`); não é trilha estruturada.

**Concluído administrativo preservado na leitura.**

O escritor administrativo recusa apenas `cancelado` (`app/ferias/services.py:384`), preservando a porta de concluído com justificativa (`app/ferias/views.py:403-415`). Existe teste anterior que exige cancelamento, saldo devolvido e justificativa no log (`app/ferias/test_corrigir_concluida.py:76-87`); execução nesta cópia **[não verificado]**.

**Pré-requisitos de pouso.** [nao verificado]

A regra exige RED evidenciado → fix → GREEN → suite (`CLAUDE.md:190`), suite inteira antes do opus final e proíbe o Codex de aprovar sozinho o pouso (`CLAUDE.md:364-373`). RED real no pai, GREEN, suite, leitura final e deploy da obra **[não verificado]**. [nao verificado]

## 06/11 21:43 · sessao 9f0c4140c953 · BUG-PLANTIO-NOTURNO-MAIS-1 · leitura · origem=Code

**A cópia ainda não tem uma porta que re-chaveie uma pergunta com `data_evento` preenchida:** `fio.reapontar_resolvedora` só data perguntas sem dia; `abrir` procura outra pergunta pela chave solicitada ou cria uma nova. Portanto, o backfill precisa dessa operação na porta canônica antes de ser executável com identidade preservada. (`app/chamados/services/fio.py:835`; `app/chamados/models.py:2453`; `app/chamados/models.py:2497`)

As referências abaixo são da cópia fornecida. Código e numeração do main `19aeede1`: **[nao verificado]**. Os anexos relatam 81 vivas com D significando saída, 13 `saida_sem_entrada` materializadas e um teto de 21 SSE vivas; identificação individual, interseções, ausência de marcos e plantios efetivos em D−1: **[nao verificado]**.

1. **Universo: identificar pela fonte, não por motivo ou por “data − 1”.**

   O predicado seguro para o lote é: **fonte do episódio identificada sem ambiguidade; `data_evento` igual ao dia da saída dessa fonte; juiz existente devolvendo outro dia para o turno**. A conversão recebe a resposta do juiz, sem calcular uma véspera própria. `dia_do_turno_da_batida(colab, batida_fonte)` consulta `dia_das_batidas` por PK; esse mapa usa `Turno.data_turno`. (`app/ponto/turnos.py:2917`; `app/ponto/turnos.py:2923`; `app/ponto/turnos.py:2939`)

   | Motivo/fonte | Evidência que distingue os significados |
   |---|---|
   | `orfao_14h` | A fonte é a **entrada aberta**. O detector recupera seu PK; o resolvedor atual pergunta ao juiz dessa batida. A docstring registra que antes se gravava a saída esperada, mas isso não comprova as linhas do main. (`app/chamados/detector_anomalias.py:35`; `app/chamados/detector_anomalias.py:43`; `app/escala/servico_jornada.py:480`) |
   | `saida_sem_entrada` | A fonte é a **S órfã**. API, clone e triagem atuais obtêm seu dia depois do registro, pelo mesmo juiz. A docstring registra o recebimento antigo do dia da saída órfã. (`app/api/views.py:775`; `app/api/views_core.py:775`; `app/ponto/services/triagem_batida.py:726`; `app/escala/servico_jornada.py:482`) |
   | Fonte célula/ata | O dia é o da **célula dona do marco**; o instante do marco pode cair no dia seguinte. `dia_do_turno_da_celula` apenas declara a proveniência recebida, sem descobrir ou corrigir o dia. (`app/ponto/turnos.py:2929`; `app/escala/utils.py:915`) |

   Não fecharia o censo sem examinar também `intervalo_saida` e `intervalo_volta`: há um caminho que lê `_m['data']` do marco faltante e o entrega como dia da pergunta; o produtor consultado monta essa data pelo instante encadeado do marco. Isso pode representar dia do marco, distinto do turno, mas quais perguntas gravadas vieram desse caminho é **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1018`; `app/chamados/services/disputa_emissao.py:1023`; `app/ponto/management/commands/detectar_intervalo_ausente.py:77`)

   O juiz concreto é `_data_do_turno`, chamado por `_mk`: para S órfã, consulta o envelope da jornada da véspera. **Sem marcos da véspera, devolve o dia civil da referência**; batida que nenhum turno reclama também recebe seu dia civil. Logo, “sem marcos + resposta 19h + saída 07h” não basta para autorizar D−1 sem outra fonte demonstrada. (`app/ponto/turnos.py:285`; `app/ponto/turnos.py:293`; `app/ponto/turnos.py:312`; `app/ponto/turnos.py:2944`)

   Contraexemplos ao filtro por motivo: declarações dos mesmos marcos já nascem com o dia da célula; o complemento de marcos também conserva esse dia. O teste diurno 08–17 espera pergunta e saída no mesmo D. Exemplares reais desses contraexemplos no dado fornecido: **[nao verificado]**. (`app/chamados/services/declaracao_texto.py:325`; `app/chamados/services/disputa_emissao.py:840`; `app/chamados/tests/test_plantio_noturno_mais1.py:123`)

   Os antigos produtores são descritos pela própria raia — `data_saida_esperada`/resolvedor para `orfao_14h`, dia da S para SSE — e seus consumidores atuais estão acima; uma lista histórica fechada com `arquivo:linha` do main permanece **[nao verificado]**. (`app/escala/servico_jornada.py:480`; `app/escala/utils.py:945`)

2. **Materializada: conservar PK, célula e associação do ato.**

   Para `hora`, `_slot_esperado` chama o derivador real do plantio. `_pergunta_materializada_de_fato` procura batidas ligadas à pergunta, não retratadas, e `_plantio_na_hora_do_slot` aceita a janela de ±20 minutos nos offsets **0 e +1**. Re-chavear corretamente uma pergunta cujo plantio estava em antigo D−1 coloca esse mesmo timestamp no offset 0; conservar apenas a FK não garante reconhecimento se hora ou data continuarem incompatíveis. (`app/chamados/juizes.py:344`; `app/chamados/juizes.py:719`; `app/chamados/juizes.py:262`; `app/chamados/juizes.py:271`)

   O conjunto a tratar no mesmo ato é:

   | Objeto | Tratamento necessário |
   |---|---|
   | `PerguntaDisputa` | Preservar PK, resposta, validação e carimbo original; alterar a identidade temporal e relavrar o cache sobre o novo estado. `janela_expira_em` deriva de `data_evento`. (`app/ponto/models.py:40`; `app/chamados/veredito_lavrado.py:219`; `app/chamados/veredito_lavrado.py:227`) |
   | `p.celula` | Reatar à célula existente do colaborador/D correto. O juiz lê essa FK diretamente; `_celulas_da_pergunta` rejeita alimentação cuja célula tenha outro dia. Não deslocar a própria célula para corrigir a pergunta. (`app/chamados/juizes.py:622`; `app/chamados/juizes.py:890`; `app/chamados/juizes.py:1148`) |
   | Chamado/destino | Conferir célula e dia declarado pela autoridade do chamado. A célula vence o JSON; divergência com a pergunta, sem conferência, bloqueia materialização. Não basta trocar `contexto_json['data_turno']`. (`app/chamados/juizes.py:94`; `app/chamados/services/fio.py:77`; `app/chamados/services/materializacao.py:1167`) |
   | `Disputa` | Preservar sua identidade, especialmente nos grupos absorvidos. Contexto por motivo ainda é fonte subsidiária, e a derivação do dia do chamado pode consultar as datas das perguntas da disputa. (`app/chamados/services/materializacao.py:343`; `app/chamados/juizes.py:56`; `app/chamados/services/materializacao.py:595`) |
   | `EventoCelula` | Emitir evento da célula correta pela porta existente; não redatar eventos históricos. O evento guarda célula/colaborador/data, e o `post_save` da pergunta prefere a célula da FK ao fallback pelo dia. (`app/escala/models.py:1856`; `app/ponto/portas/celula.py:882`; `app/chamados/models.py:2693`; `app/chamados/signals.py:219`) |

   **A Batida plantada permanece com o mesmo timestamp e fato.** Seu vínculo aponta ao PK da pergunta; a materialização grava esse vínculo tanto em batida nova quanto reutilizada. Criar uma pergunta substituta não transfere automaticamente esse lastro. (`app/ponto/models.py:40`; `app/chamados/services/materializacao.py:917`; `app/chamados/models.py:2498`)

   **Absorção é um caso separado:** a recuperação exige coerência entre `(colaborador, tipo, Ausencia.data)` e o grupo `(disputa, data_evento, materializada_em)`, contendo o escape; exclui perguntas com plantio vivo. Mudar só uma irmã rompe a associação. (`app/chamados/services/materializacao.py:593`; `app/chamados/services/materializacao.py:606`; `app/chamados/services/materializacao.py:617`)

   Para conservar essa ligação, o grupo completo — inclusive a pergunta de escape — precisa continuar associado à ausência correta, mantendo disputa e instante do ato. Isso **não autoriza redatar a ausência automaticamente**: se o dia correto dela divergir do dia do turno das perguntas, essa distinção não está representada pelas chaves atuais; a solução desse caso é **[nao verificado]**. (`app/chamados/services/materializacao.py:517`; `app/chamados/services/materializacao.py:532`; `app/chamados/services/materializacao.py:595`)

3. **Descarimbo: condição exata e prova de “dispararia 0”.**

   A porta relê cada PK sob trava e só desfaz quando a linha gravada satisfaz simultaneamente:

   - `materializada_em IS NOT NULL`;
   - via fora de `VIAS_DE_IDENTIDADE`;
   - disputa aberta, quando `reabrir_fechada=False`;
   - `pergunta_viva(p) == True`. (`app/chamados/services/relavra_via.py:169`; `app/chamados/services/relavra_via.py:217`; `app/chamados/services/relavra_via.py:227`)

   As vias excluídas são `admin_decidiu`, `duplicata`, `fato_ocupado` e `paridade_recusada`. Ao disparar, zera os três campos do carimbo, registra trilha e, no comportamento padrão, reabre a disputa fechada. (`app/chamados/models.py:2290`; `app/chamados/models.py:2668`; `app/chamados/services/relavra_via.py:229`)

   **Perder reconhecimento não implica descarimbo automático:** validação preenchida e tranca encerram antes; resposta, lâmpada do marco, veredito da célula e classificação da falta também podem decidir antes do último degrau que consulta `_pergunta_materializada_de_fato`. (`app/chamados/juizes.py:1136`; `app/chamados/juizes.py:1151`; `app/chamados/juizes.py:1178`; `app/chamados/juizes.py:1198`; `app/chamados/juizes.py:1214`)

   A leitura equivalente ao contador é aplicar **o próprio** `q_carimbo_desfazivel` ao estado pós-migração e contar as linhas para as quais **a função real** `pergunta_viva` retorna verdadeiro. O vigia já faz essa composição para disputas abertas, separando aberta/exportada; exigir somente `aberta=0` não prova zero no conjunto inteiro. (`app/chamados/services/vigia_fio.py:806`; `app/chamados/services/vigia_fio.py:812`; `app/chamados/services/vigia_fio.py:847`)

   Para chamar literalmente `desfazer_carimbo_sem_lastro` sem escrever, seria necessário um ensaio que apresentasse a projeção pós-migração à **releitura ORM** e interceptasse `descarimbar`, `evento` e `reabrir_sistema`, conservando os juízes reais. Passar uma instância alterada em memória não serve; a função relê pelo PK e não tem `dry_run`. Ensaio proposto, não executado: **[nao verificado]**. (`app/chamados/services/relavra_via.py:185`; `app/chamados/services/relavra_via.py:213`; `app/chamados/services/relavra_via.py:231`)

   A prova precisa incluir **zero falhas/engolidos**, pois a exceção no último degrau de vitalidade retorna `False`; além disso, a sombra deve bloquear efeitos laterais de observação e o possível `get_or_create` do usuário sistema. “Dispararia 0” em produção: **[nao verificado]**. (`app/chamados/juizes.py:1216`; `app/chamados/juizes.py:1069`; `app/core/utils.py:18`)

4. **Escritores, porta do backfill e colisão.**

   O censo da raia pelo próprio varredor AST encontrou estes quatro sítios explícitos; apenas dois persistem a data:

   | Sítio | Papel |
   |---|---|
   | `app/chamados/models.py:2444` | Proxy em memória para o gate. |
   | `app/chamados/models.py:2498` | Nascimento por `_nascer`. |
   | `app/chamados/services/fio.py:837` | Preenchimento de data ausente. |
   | `app/chamados/services/fio.py:846` | Reversão em memória após recusa. |

   O contrato citado sanciona somente `models.py` e `fio.py`; um command escrevendo diretamente não está admitido. O selo novo restringe também as funções e cobre atribuição, `setattr`, operações ORM e `bulk_update`. Censo correspondente no main: **[nao verificado]**. (`app/core/tests/test_contract_escritores_estado.py:62`; `app/core/tests/test_contract_escritores_estado.py:200`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:28`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:133`)

   **Nenhuma porta serve intacta para re-chavear a mesma PK.** A operação cabe como extensão da porta de costura/reconciliação, com command apenas como gatilho, mantendo data antes dos juízes e lavra no mesmo save; resposta e validação não precisam ser alteradas. Essa extensão ainda não está implementada nas portas consultadas. (`app/chamados/services/fio.py:819`; `app/chamados/services/fio.py:835`; `app/chamados/services/fio.py:848`; `app/chamados/services/fio.py:863`)

   **Colisão não é necessariamente erro de unicidade do banco:** o modelo não declara essa restrição; `abrir` faz deduplicação por colaborador/motivo/data em qualquer disputa/estado e devolve a última PK encontrada. Isso não migra a pergunta antiga nem funde respostas ou plantios. (`app/chamados/models.py:2879`; `app/chamados/models.py:2396`; `app/chamados/models.py:2453`; `app/chamados/models.py:2497`)

   A política já existente escolhe quem tem resposta; no empate, menor PK; as demais recebem `VIA_DUPLICATA`, com referência à canônica e preservação do carimbo anterior. Porém, a migração existente aplica isso às **vivas no destino**. Uma fusão automática de duas materializadas, preservando os dois lastros durante re-chaveamento, não está implementada nesse caminho; não se pode presumir que `abrir` resolva esse conflito. (`app/chamados/services/fio.py:870`; `app/chamados/services/fio.py:894`; `app/chamados/services/fio.py:934`; `app/chamados/services/fio.py:940`)

5. **Leitores fora do pareador e DIFF FM/DiaPago 10 e 11.**

   | Leitor | Valor que pode mudar |
   |---|---|
   | Fio/modal/cartão | Dia exibido e aviso de divergência. (`app/chamados/services/fio.py:87`; `app/templates/chamados/partials/modal_fio.html:161`; `app/templates/chamados/partials/_card_atendimento.html:87`) |
   | Ficha do dia | Pertencimento da pergunta ao dia consultado. (`app/ponto/services/dia_do_colab.py:64`) |
   | Cobrança/app/decisão do dia | Dias pendentes, diagnóstico da pergunta e seleção administrativa. (`app/chamados/services/cobranca.py:81`; `app/chamados/services/pergunta_no_app.py:109`; `app/ponto/services/veredito_celula.py:174`) |
   | Prévia da folha | Inclusão na janela, agrupamento diário e rótulo de material disponível. (`app/folha/previa.py:64`; `app/folha/previa.py:95`; `app/folha/previa.py:139`) |
   | Validação | Data usada para agendar recálculo por evento. (`app/chamados/services/validacao.py:395`) |
   | Cache da pergunta/outbox | Expiração do veredito e célula acordada. (`app/chamados/veredito_lavrado.py:227`; `app/chamados/models.py:2682`; `app/chamados/signals.py:219`) |

   FM e DiaPago recebem a apuração de batidas/escalas/células; a lavratura diária usa o mesmo resultado do fechamento. Alterar a identidade da pergunta, conservando a Batida, **não demonstra por si alteração monetária**; efeitos via ausência, célula e processamento posterior precisam ser comparados. Deltas efetivos: **[nao verificado]**. (`app/ponto/services/fechamento.py:725`; `app/ponto/services/fechamento.py:745`; `app/ponto/services/fechamento.py:1175`)

   Para FM, existe a porta real `recalcular_fechamento_mes`, com `somente_leitura=True`, `lavrar_dias_pagos=False`, `retornar_objeto=True` e `coletar_furos={}`. Ela devolve objetos destacados e os furos do mesmo percurso. Para 10/11, o recorte deve usar `janela_fechamento(mes, ano, empresa)`, respeitando o corte da empresa. (`app/ponto/services/fechamento.py:439`; `app/ponto/services/fechamento.py:1099`; `app/ponto/services/fechamento.py:1112`; `app/ponto/janelas.py:54`)

   **`lavrar_dias_pagos=True` escreve mesmo junto de `somente_leitura=True`.** O lavrador termina em `delete` e `bulk_create`; portanto, DIFF DiaPago sem escrita exige capturar sua derivação real em memória, interceptando a persistência, inclusive a linha de ajuste. Esse ensaio e os DIFFs antes/depois de FM e DiaPago nas competências 10 e 11: **[nao verificado]**. (`app/ponto/services/fechamento.py:1175`; `app/ponto/services/dia_pago.py:343`; `app/ponto/services/dia_pago.py:348`; `app/ponto/services/dia_pago.py:357`)

## 06/11 21:43 · sessao 2eb2733b0c8b · - · - · origem=Ronald (tmux)

Minha leitura exige preservar uma lacuna da taxonomia no item 3; classificar esse item integralmente como (a) ou (b) excederia os anexos. (`v2.r:34`; `triagem_v3.txt:3`)

1. **(a), delimitado à variante por comentário.** O tema estava perguntado: “Ha caminho em que um commit com n/a (medida) muda COMPORTAMENTO […] ou o que os leitores do placar decidem” (`v1.q:3`); também “qualquer leitor” (`v3.q:11`). A resposta viu o mecanismo próximo: “O handoff muda sua leitura de **19/20** para **0/999**, porque pesquisa o primeiro casamento no texto bruto inteiro”, mas o contraexemplo apresentado inseria a frase na **docstring**, não em comentário (`v3.r:28`). A triagem considerou procedente e curada a “docstring mascarada”; não descartou a variante comentário (`triagem_v3.txt:2`). Portanto, não chamar de “Codex não viu o leitor”: viu o leitor e não apontou a variante que sobreviveu. (`v3.r:28`; `opus_final.txt:2`) [nao verificado]

2. **(e), para a falsidade das duas afirmações específicas.** Não há pergunta expressa nos `.q` sobre a verdade da prosa do comentário final ou do `quem-mais-le` do commit `7a2e3bc5`; a rodada seguinte ocorreu sem nova volta Codex (`v1.q:1–3`; `v2.q:1–3`; `v3.q:1–22`; `triagem_v3.txt:4`). Não existe trecho `.r` que denuncie essas duas frases específicas: **[não verificado]**. Há uma resposta que contradiz a redução à docstring: “pesquisa o primeiro casamento no texto bruto inteiro” (`v3.r:28`). Isso comprova conhecimento da leitura integral, não denúncia da prosa posterior. (`opus_final.txt:3`)

3. **Visto, mas descarte correspondente [não verificado]; nenhuma classe integral comprovada.** Pergunta exata: “A leitura dos NOMES no ALVO (e nao no commit julgado) abre algo alem do retroativo de dado puro declarado?” (`v2.q:3`). Resposta exata: “Não há restrição que impeça declarar uma constante de decisão como nome elegível […] um ALVO que liste `CADENCIA_TERMOMETRO_DIAS` pode autorizar retroativamente sua troca literal.” (`v2.r:34`). A v3.r reafirma: “Não há validação de que cada chave declarada tenha significado de medição” (`v3.r:94`). A triagem descarta **as chaves atuais** `fonte_do_medido`, `fonte`, `nota_do_papel`, por exigência dos três commits verdes; não registra descarte da ampliação arbitrária de `DADO_MEDIDO` (`triagem_v3.txt:3`). Também o tema arquivo novo foi perguntado e respondido: “arquivo novo que se declara medida” / “**Aceita se todas as linhas forem livres**” (`v1.q:3`; `v1.r:17`). Assim, (a) e (e) seriam falsos para o núcleo; (b) requer uma linha de triagem que não foi fornecida. (`v2.r:34`; `triagem_v3.txt:3`) [nao verificado]

4. **Cuidado: (a) é defensável pelo critério literal “tema presente”; (e) depende de estreitar o tema para “atualização documental”.** O CONTRATO aparece expressamente nas perguntas: “o CONTRATO de 5 linhas” (`v1.q:1`) e “o CONTRATO e o aval” (`v2.q:1`); o MORDE e suas cláusulas também são objeto de pergunta (`v1.q:3`; `v2.q:3`). As respostas examinam cobertura e falha Git, sem apontar que os itens 3–4 do contrato ficaram velhos (`v2.r:40–64`; `v3.r:72–88`). Se o critério da casa é presença do **tema**, eu escolheria **(a)**, deixando claro que não houve pergunta específica “o contrato continua atualizado”. Atribuir (e) só porque faltou essa formulação exata seria um estreitamento não autorizado do critério. (`contrato.txt:1`; `opus_final.txt:5`)

As linhas acima seguem a contagem lógica dos anexos fornecidos, incluindo linhas vazias, desde a primeira linha de cada conteúdo.

## 06/11 21:41 · sessao 87b166d0bc57 · - · - · origem=Ronald (tmux)

(1) **Universo: não há predicado seguro por motivo + data isoladamente.** O recorte migrável precisa demonstrar a fonte antiga e comparar `p.data_evento = dia da saída da fonte` com `dia_do_turno_da_batida(colab, batida_fonte) != p.data_evento`; o juiz faz o recorte por PK da batida sobre `dia_das_batidas`, sem o backfill criar uma regra “subtrair um dia”. (`app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2923`, `app/ponto/turnos.py:2939`)

- Os motivos diretamente envolvidos são `orfao_14h` (falta S) e `saida_sem_entrada` (falta E). A implementação atual documenta que antes o primeiro recebia a data da saída esperada e o segundo a data da saída órfã. Isso documenta a história na raia, mas não substitui abertura do main `19aeede1`, aqui **[nao verificado]**. (`app/escala/servico_jornada.py:474`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:485`)
- Fontes atuais: detector encontra a entrada pendente pelo PK e usa `resolver_data_evento`; questionário de chamados usa a batida origem; API e clone corrigem o contexto depois de registrar a S; triagem usa o mesmo juiz da batida. (`app/chamados/detector_anomalias.py:39`, `app/chamados/detector_anomalias.py:47`, `app/chamados/services/disputa_emissao.py:1068`, `app/chamados/services/disputa_emissao.py:1080`, `app/api/views.py:775`, `app/api/views_core.py:775`, `app/ponto/services/triagem_batida.py:726`)
- Outros caminhos da raia que passaram a obter o dia da batida: referência da grade por `entrada_id`, instante exato ou `ultima_batida.id`; lote de entradas pendentes por PK; fallback da última batida por ID. (`app/chamados/services/disputa_emissao.py:59`, `app/chamados/services/disputa_emissao.py:68`, `app/chamados/services/disputa_emissao.py:272`, `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:379`)
- `_T1` concreto é `_data_do_turno`, chamado por `_mk`. Para S órfã ele consulta o envelope da jornada da véspera; sem marcos da véspera, retorna o dia civil da referência. Portanto “sem marcos” não autoriza o backfill a inventar D−1: origem histórica suficiente ou conclusão **[nao verificado]**. (`app/ponto/turnos.py:248`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:293`, `app/ponto/turnos.py:312`)
- A casca `turnos_do_colab` carrega células da janela, escalas incluindo a geradora da célula e marcos do DNA. `dia_do_turno_da_batida` usa essa autoridade por `dia_das_batidas`; batida não reclamada por turno recebe o dia civil. (`app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1909`, `app/ponto/turnos.py:2902`, `app/ponto/turnos.py:2944`)
- Contraexemplos de código ao filtro por motivo: perguntas de declaração de marco da célula já recebem `dia_do_turno_da_celula(dia)`; uma pergunta `orfao_14h` ou `saida_sem_entrada` pode, portanto, já conter o significado correto. Turno diurno não atravessa a meia-noite no juiz e conserva o dia próprio. Exemplares reais nos anexos: **[nao verificado]**. (`app/chamados/services/declaracao_texto.py:325`, `app/chamados/services/declaracao_texto.py:615`, `app/ponto/turnos.py:289`, `app/ponto/turnos.py:2929`)

(4) **Censo fechado da raia, executado sem Django/DB, usando o próprio varredor AST extraído:** quatro sítios, sendo apenas dois persistentes: `_nascer` cria a pergunta; `reapontar_resolvedora` preenche a data ausente. Os outros são proxy de validação e reversão em memória. O algoritmo e as duas portas sancionadas estão no selo. (`app/chamados/models.py:2444`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:846`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:28`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:112`)

**Nenhuma porta atual re-chaveia uma pergunta com data preenchida preservando sua identidade.** `fio.reapontar_resolvedora` só escreve quando `not p.data_evento`; `abrir` consulta o fato no dia pedido e devolve outra pergunta existente ou cria nova. Logo o command deve ser gatilho de uma extensão da porta canônica, com trilha, e não atribuir diretamente o campo; a extensão ainda não existe nesta cópia. (`app/chamados/services/fio.py:835`, `app/chamados/models.py:2453`, `app/chamados/models.py:2497`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:863`)

O contrato antigo admite somente os arquivos `models.py` e `fio.py` para `data_evento`; escritor novo fora deles falha ou exige declaração justificada. A raia acrescenta selo AST mais restrito: somente `_nascer` e `reapontar_resolvedora`, incluindo `bulk_update`, `setattr`, kwargs e aliases do modelo. Um command de backfill direto viola o desenho e o selo atuais. (`app/core/tests/test_contract_escritores_estado.py:48`, `app/core/tests/test_contract_escritores_estado.py:62`, `app/core/tests/test_contract_escritores_estado.py:200`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:13`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:133`)

A data precisa entrar antes do juiz e da lavra; o save da porta inclui `veredito_lavrado`, `julgado_em` e `janela_expira_em`. Portanto uma extensão de re-chaveamento não deve simplesmente salvar `data_evento` isoladamente. (`app/chamados/services/fio.py:819`, `app/chamados/services/fio.py:848`)

**Colisão:** identidade é `(colaborador, motivo, data_evento)` em qualquer disputa/estado; `abrir` pega a última por PK e retorna a existente, sem transferir a pergunta antiga, respostas ou lastro. (`app/chamados/models.py:2385`, `app/chamados/models.py:2396`, `app/chamados/models.py:2403`, `app/chamados/models.py:2453`, `app/chamados/models.py:2497`)

A lei já existente para duplicatas escolhe quem tem resposta, depois menor PK; a outra recebe `VIA_DUPLICATA` e motivo com a canônica, sem apagar e mantendo carimbo anterior. A migração de resolvedoras aplica essa lei às vivas que conflitam no destino. Uma fusão automática de duas materializadas no re-chaveamento não está implementada por essas portas **[nao verificado quanto a outra porta não aberta]**. (`app/chamados/services/fio.py:870`, `app/chamados/services/fio.py:881`, `app/chamados/services/fio.py:894`, `app/chamados/services/fio.py:934`, `app/chamados/services/fio.py:940`)

## 06/11 21:40 · sessao 579cf276cb5a · - · - · origem=Ronald (tmux)

**(2) O lastro que a migração precisa conservar**

- Para pergunta `hora`, `_slot_esperado` chama o materializador real; o reconhecedor busca `Batida.pergunta_origem` não retratada e compara janela de ±20 minutos nos dias `data_evento+0/+1`. Portanto, um plantio em D−1 só passa a caber no offset 0 após a pergunta receber o D do turno; ter FK de origem, isoladamente, não prova a materialização. (`app/chamados/juizes.py:245`, `app/chamados/juizes.py:262`, `app/chamados/juizes.py:271`, `app/chamados/juizes.py:344`, `app/chamados/juizes.py:719`)
- O vínculo da batida é FK para o PK da pergunta; conservar PK mantém esse vínculo sem mudar timestamp nem fato. O materializador grava a origem tanto na batida nova como na reutilizada. (`app/ponto/models.py:40`, `app/chamados/services/materializacao.py:917`)
- Migrar apenas `data_evento` pode deixar a FK `p.celula` no outro dia: `_celulas_da_pergunta` só aceita a célula quando data e colaborador coincidem; o juiz de vitalidade e o juiz do marco leem diretamente a célula da pergunta. Deve-se reatar à **célula existente do dia correto**, sem deslocar `CelulaDia.data` ou seu DNA. (`app/chamados/juizes.py:622`, `app/chamados/juizes.py:1148`, `app/chamados/juizes.py:1182`, `app/chamados/juizes.py:890`, `app/escala/models.py:1685`)
- O chamado que lista a pergunta também precisa continuar falando desse mesmo fato: `data_do_fato_de` compara `data_evento` com `dia_do_fato(ch, fortes=True)`; célula do chamado vence o contexto; divergência sem conferência bloqueia a materialização. Revisar `chamado_destino`, sua célula e sua chave de data, não somente o JSON. (`app/chamados/services/fio.py:77`, `app/chamados/juizes.py:94`, `app/chamados/juizes.py:107`, `app/chamados/services/materializacao.py:1167`)
- `DisputaSupervisao` não tem coluna própria de dia: possui `contexto`, chamado e perguntas; a derivação fraca do dia do chamado pode usar o mínimo das datas das perguntas da disputa. O contexto por motivo também alimenta `_turno_tem_saida` quando a pergunta não tem data. (`app/chamados/models.py:1679`, `app/chamados/models.py:1683`, `app/chamados/juizes.py:56`, `app/chamados/juizes.py:363`)
- Não se deve reescrever historicamente `EventoCelula` como se fosse a pergunta: ele registra a lavra da célula, com `celula_id`, colaborador e data, sem FK. A porta emite um **novo evento** da célula correta. O `post_save` da pergunta acorda o consumidor quando mudam `data_evento`, célula, disputa ou destino; ele usa a célula da FK antes do fallback pelo par colaborador/data, portanto FK errada emite evento do dia errado. (`app/escala/models.py:1845`, `app/ponto/portas/celula.py:882`, `app/chamados/models.py:2682`, `app/chamados/models.py:2693`, `app/chamados/signals.py:214`)
- O consumidor relê células e chamados atuais e pode rejuntar, carimbar perguntas e retratar chamados; logo esse evento é efeito operacional da migração que precisa constar da sombra. (`app/chamados/reconciliador.py:1305`, `app/chamados/reconciliador.py:1346`)

**Absorção exige preservar o grupo, não só a pergunta.** O escritor carimba todas as irmãs `hora` daquele dia com o mesmo instante; a recuperação usa `(disputa, data_evento, materializada_em)`, exige escape `FALTA_DIA/ATESTADO`, casa com `(colaborador, tipo, Ausencia.data)` e exclui perguntas com batida plantada viva. Rechavear isoladamente uma irmã rompe essa associação. Para conservar exatamente a ligação atual, dia do grupo completo e dia da ausência têm de continuar coerentes, preservando disputa/instante; isso **não autoriza** mudar `Ausencia.data` sem provar que seu próprio fato pertence ao dia do turno. Se a ausência pertence corretamente ao dia civil e a pergunta ao dia do turno, o desenho atual não expressa essa distinção. (`app/chamados/services/materializacao.py:530`, `app/chamados/services/materializacao.py:586`, `app/chamados/services/materializacao.py:606`)

Quais perguntas do dado são absorvidas, quais têm FK de origem e quais plantios estão em D−1: **[nao verificado]**.

**(3) Condição exata e “dispararia 0”**

A função relê cada PK sob trava; a instância recebida já modificada em memória não é usada como estado pós-migração. Só dispara quando:

1. a linha gravada ainda satisfaz `materializada_em IS NOT NULL`;
2. a via está fora de `VIAS_DE_IDENTIDADE`;
3. com `reabrir_fechada=False`, a disputa permanece aberta;
4. `pergunta_viva(p)` retorna `True`. (`app/chamados/services/relavra_via.py:169`, `app/chamados/services/relavra_via.py:217`)

As vias de identidade são `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`. (`app/chamados/models.py:2290`)

Ao disparar, `descarimbar()` zera conjuntamente `materializada_em`, `materializada_por`, `via_resolucao`, emite trilha e, no comportamento padrão, reabre disputa fechada. (`app/chamados/models.py:2668`, `app/chamados/services/relavra_via.py:229`)

**Perder reconhecimento do plantio não implica automaticamente descarimbo.** `validada_em` preenchida mata a pergunta logo no primeiro degrau; tranca também a mata; resposta não vazia, célula acusando e lei do dia podem decidir antes do último degrau de reconhecimento. O último degrau devolve o inverso de `_pergunta_materializada_de_fato`; exceção nele retorna `False`, portanto “zero” com juiz indisponível seria uma falsa certificação. (`app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1151`, `app/chamados/juizes.py:1178`, `app/chamados/juizes.py:1198`, `app/chamados/juizes.py:1208`)

O gancho atual do julgamento coleta elegíveis pela mesma expressão `q_carimbo_desfazivel(so_disputa_aberta=True)` e chama a função real com `reabrir_fechada=False`. (`app/chamados/services/fabrica_da_celula.py:117`, `app/chamados/services/fabrica_da_celula.py:163`)

Para uma sombra **sem escrever**, a função não possui `dry_run`: seria necessário executar a função real sobre uma projeção pós-migração que sua releitura ORM enxergue, mantendo os juízes reais e substituindo apenas os efeitos `descarimbar`, `evento` e `reabrir_sistema` por espiões; contar o retorno real e exigir retorno 0, nenhum efeito chamado e nenhuma exceção/engolido. Proposta de instrumentação, ainda não executada: **[nao verificado]**. Não basta passar cópias com datas trocadas: a releitura por PK ignora essas cópias. (`app/chamados/services/relavra_via.py:213`)

O precedente documentado chama a função real em `atomic+raise`, mas isso executa escritas e depois as desfaz; não satisfaz “sem escrever”. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:23`)

O contador existente `materializada_viva()` compartilha elegibilidade e juiz, porém separa `aberta` de `exportada` e só inclui disputa aberta; `aberta=0` sozinho não prova que a função padrão dispararia zero para todo o lote. (`app/chamados/services/vigia_fio.py:789`, `app/chamados/services/vigia_fio.py:806`, `app/chamados/services/vigia_fio.py:847`)

## 06/11 21:40 · sessao 0f08dec0b063 · FIO-COM-ASSENTO-E0-V4 · leitura · origem=Code

**A v4 elimina o escritor de estado derivado da tarefa, mas suas entradas ainda não existem completas na cópia:** decisões são sobrescritas/apagadas, alguns efeitos precisam de consulta externa à célula e a ligação de absorção depende de `materializada_em`. Isso exige completar a alimentação; não demonstra necessidade de uma entidade de obrigação. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/models.py:1976`; `app/chamados/juizes.py:720`; `app/chamados/services/materializacao.py:588`.

Vínculo desta cópia ao HASH/HEAD, execução de testes, custos medidos e ocorrências em produção: **[nao verificado]**. Os exemplos abaixo são construções para examinar informação disponível, não relatos de produção. **[nao verificado]** [nao verificado]

**(1) Entradas existentes e lacunas**

O lookup distingue autoridades por domínio e remete ao registro vivo; ele não registra uma função que reúna toda a pergunta da v4. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:20`; `app/core/juizes.py:175`.

| Entrada | Fonte existente e retorno | Universo e unidade |
|---|---|---|
| Célula, DNA e ata atuais | `celulas_do_periodo(ids, ini, fim)` → mapa `{(colaborador_id, data): CelulaDia}`; contém a foto persistida, não uma sequência histórica. `app/escala/alimentacao.py:54` | Colaboradores explicitados, faixa inclusiva, sem exigir situação ativa; chave por data da célula. `app/escala/alimentacao.py:65` |
| Fatos concorrentes e precedência | `fatos_do_dia`/`fatos_do_periodo` → tipo vencedor, faixa, rótulo, suprimidos, previsto e fatos encontrados. Não substituem o conteúdo inteiro da célula. `app/ponto/precedencia.py:242` | Pessoa e datas consultadas; ausências, feriados e folgas usam vigência por data. `app/ponto/precedencia.py:259`; `app/ponto/precedencia.py:273` |
| Batidas consideradas na apuração | `batidas_apuraveis` → queryset sem retratadas; batida pendente de DP continua entrando. `app/ponto/turnos.py:2844` | Pessoa opcional; intervalo inclusivo de **instantes**, ou vida inteira sem limites. Não é recorte por dia do turno. `app/ponto/turnos.py:2852`; `app/ponto/turnos.py:2864` |
| Pertencimento ao turno | `turnos_do_colab`; `batidas_do_dia_turno` → turnos ou batidas cuja jornada pertence ao dia pedido. `app/core/juizes.py:196`; `app/ponto/turnos.py:3013` | Dia do **turno**, inclusive batidas civis em D+1. `app/ponto/turnos.py:3017` |
| Marcos previstos | `EscalaColaborador.marcos_do_dia`; `marcos_turno` → marcos e seus datetimes encadeados. `app/core/juizes.py:201`; `app/escala/servico_jornada.py:61` | Dia do turno; o encadeamento avança a data quando atravessa meia-noite. `app/escala/servico_jornada.py:75` |
| Teto temporal | `marcos_vencidos(c, cel, data, agora)` → `(vencidos, pendentes)`, listas `(tipo, HH:MM)`, comparando instante do marco + teto. `app/ponto/services/cartorio.py:831` | Instante aware, ancorado no turno; teto atual de 30 minutos. `app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:845` |
| Turno encerrado | `dia_encerrado`/`dias_encerrados`; `CargaDaJanela.dias_encerrados` reutiliza o corpo existente. `app/core/juizes.py:223`; `app/ponto/turnos.py:3001` | Fato encerrado do turno, não simplesmente `data < hoje`. `app/core/juizes.py:210` |
| Respostas | Campos atuais de `PerguntaDisputa`; `RespostaAnterior` guarda versões retiradas pela reabertura. `app/chamados/models.py:2227`; `app/chamados/models.py:3165` | Resposta ligada à pergunta; `data_evento` é data, resposta/validação são instantes. A versão anterior não constitui histórico completo de todas as alterações. `app/chamados/models.py:2218`; `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1406` |
| Validações e devoluções | `_carimbar_validacao` grava instante/autor; `devolver_validacao_recusada` limpa ambos. São comandos escritores, não um feed imutável. `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188` | Pergunta existente, por instante. O estado atual perde a validação desfeita se a alimentação não incluir sua trilha. `app/chamados/services/validacao.py:176` |
| Decisões sobre ausência | Linha atual de `Ausencia`; cobertura por `ausencia_cobre`/`cobertura_ausencia_periodo`; parcialidade por funções próprias. `app/ponto/turnos.py:2536`; `app/ponto/turnos.py:2733`; `app/ponto/turnos.py:2583` | Cobrança exclui rejeitadas; folha exige aprovadas; cobertura por datas não determina sozinha cobertura de cada marco. `app/ponto/turnos.py:2509`; `app/ponto/turnos.py:2542`; `app/ponto/turnos.py:2668` |
| Atos administrativos | Veredito/via da célula; fechamento/reabertura da disputa; conferência de data; silêncio; recusa. Fontes persistidas distribuídas. `app/ponto/portas/celula.py:811`; `app/chamados/models.py:1861`; `app/chamados/services/validacao.py:617`; `app/chamados/services/arquivo.py:166`; `app/chamados/services/validacao.py:545` | Alguns atos atingem o dia, outros pergunta, disputa ou chamado; seus campos atuais não são um registro imutável uniforme. `app/chamados/juizes.py:1148`; `app/chamados/models.py:2063` |
| Reescrita do previsto | `carimbos_de_reescrita` → par regeneração/edição por data; `cobranca_orfa` compara com nascimento da cobrança. `app/escala/services/regeneracao.py:87`; `app/escala/services/regeneracao.py:141` | Data da célula e instantes de reescrita/cobrança; fallback histórico usa `LogAuditoria`. `app/escala/services/regeneracao.py:37`; `app/escala/services/regeneracao.py:108` |
| Responsável e porta existentes | `porta_alcancavel_do_dia` → `None` ou `{rota, fila, verbos, fio}`; admite várias portas/verbos. `app/chamados/juizes.py:1341` | Célula, universo da fila e perguntas no app; exclui acesso manual e “Resolver dia”. Não identifica uma pessoa nominal responsável. `app/chamados/juizes.py:1350`; `app/chamados/juizes.py:1367` |

Há ainda entradas que não cabem apenas em “fatos da célula”: situação do colaborador, veto de cadastro/régua, tranca e contexto de acesso. A fábrica exige ativo; o veto responde separadamente; permissão depende do usuário e do departamento do chamado. `app/chamados/services/fabrica_da_celula.py:91`; `app/ponto/services/furos_vetados.py:27`; `app/chamados/juizes.py:1140`; `app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`.

**Entrada sem fonte completa hoje:** sequência preservada e ordenada de decisões, contendo correções, revogações, reaberturas e vínculos aos efeitos/marcos que cada decisão atingiu. A resposta ordinária sobrescreve; a reabertura preserva somente parte dos campos; a remoção de motivo apaga a pergunta e suas versões por cascata. Existência de outro feed completo fora das fontes examinadas: **[nao verificado]**. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1407`; `app/chamados/models.py:1976`; `app/chamados/models.py:3179`.

A fábrica também não supre essa lacuna: `motivos_a_perguntar` responde “o que ainda não foi perguntado”, excluindo motivos encontrados no histórico, e não “qual tarefa permanece exigível”. `app/chamados/services/fabrica_da_celula.py:74`; `app/chamados/services/fabrica_da_celula.py:118`.

**(2) Derivabilidade: separar entradas completas de snapshots atuais**

Não encontrei contraexemplo demonstrando que **entradas completas, ordenadas e imutáveis**, com os efeitos e vínculos necessários, exigiriam duas saídas diferentes pela regra vigente. Portanto, não há prova aqui de que os quatro conceitos necessitem de tabela própria de tarefa. Essa impossibilidade geral permanece **[nao verificado]**; as perdas efetivas dos snapshots estão nos escritores abaixo. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/models.py:1976`.

| Conceito | O que pode sair das entradas; onde falta informação |
|---|---|
| **Episódio** | Pode ser reduzido de uma história completa, desde que esteja definido qual acontecimento inicia outro episódio. A identidade atual usa colaborador/motivo/data, sem episódio. O significado universal de episódio pós-reabertura não está implementado nesses caminhos. `app/chamados/models.py:2392`; `app/chamados/models.py:2398` |
| **Pós-condição** | É derivável dos efeitos externos alimentados. Não sai de validação + célula apenas. C6 mata pela validação antes de exigir testemunha, enquanto o plantio de hora faz verificação adicional. `app/chamados/juizes.py:1135`; `app/chamados/services/materializacao.py:1207` |
| **Marco** | Os marcos faltantes podem ser enumerados; falta identidade suficiente para atribuir universalmente uma decisão a uma **instância** quando há repetição. O próprio juiz retorna desconhecido com duas pausas. `app/chamados/services/declaracao_texto.py:65`; `app/chamados/juizes.py:1107` |
| **Equivalência de resposta** | Igualdade do efeito pretendido pode ser calculada com tipo, marco, data e contexto completos. Não encontrei juiz geral de equivalência resposta×resposta: o existente compara resposta×luz e classifica delta. `app/chamados/juizes.py:1087`; `app/chamados/juizes.py:1548` |

Os contraexemplos que a cópia sustenta são de **projeções incompletas**:

- **Episódio perdido:** H1 responde `18:00`, é validada e depois corrigida para `19:00` em T; H2 responde pela primeira vez `19:00` em T. O caminho de resposta permite os mesmos campos atuais `19:00/T/validada=None`, sem criar `RespostaAnterior` na correção ordinária. A história distingue os episódios; os campos atuais não. **Pela regra atual, ambas aguardam admin**: exigir pendências diferentes apenas porque o número do episódio difere seria regra adicional, não demonstrada. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/juizes.py:1150`.

- **Efeito omitido:** H1 valida e planta uma batida viva no slot; H2 valida, mas o plantio é recusado. Ao projetar ambas somente em resposta/validação e numa mesma foto da célula, perde-se a diferença que a pós-condição precisa responder: “efeito existe” versus “efeito não existe”. A consulta independente às batidas separa as histórias; o carimbo de validação não separa. O fluxo possui justamente recusa, prova e devolução. `app/chamados/services/materializacao.py:1207`; `app/chamados/juizes.py:720`; `app/chamados/services/validacao.py:180`.

- **Marco sem vínculo:** dia com duas saídas de intervalo, às `12:00` e `16:00`; uma declaração sem identificador da instância pode referir-se a qualquer uma. As duas intenções não são distinguíveis por motivo/data/texto iguais. O código agrupa por motivo e explicitamente se abstém diante de duas pausas; escolher marcos diferentes como resposta obrigatória, sem vínculo registrado, seria inventar informação. `app/chamados/models.py:2392`; `app/chamados/services/fabrica_da_celula.py:62`; `app/chamados/juizes.py:1107`.

- **Equivalência contextual:** para intervalo fixo às `12:00`, respostas `12:01` e `12:03` podem gerar o mesmo plantio `12:00`, pois o materializador usa a hora do marco. Isso já permite igualdade de efeito nesse contexto; não autoriza equivalência universal entre textos. Também não autoriza tratar tolerância como equivalência transitiva: a regra de classe usa distância limitada. `app/chamados/services/materializacao.py:404`; `app/chamados/services/materializacao.py:411`; `app/chamados/juizes.py:1560`.

Consequentemente, **não seria correto fabricar quatro pares com saídas obrigatoriamente diferentes**: para episódio e marco, a cópia demonstra perda/ambiguidade, mas não uma lei que obrigue saídas diferentes apesar de entradas completas iguais. Essa obrigação permanece **[nao verificado]**.

As decisões atuais são mutáveis nestes pontos:

| Registro | Escritores que alteram ou removem |
|---|---|
| Resposta | Sobrescrita em `responder_pergunta_disputa_core`; limpeza em `reabrir_para_correcao`; exclusão em `remover_motivo`. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1976` |
| `validada_em` | Gravação explícita; limpeza por devolução, correção e reabertura; gravação automática também em `carimbar`. `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:2591` |
| `materializada_em` | Atribuição pelo núcleo do carimbo; descarimbo individual e em lote. `app/chamados/models.py:2556`; `app/chamados/models.py:2648`; `app/chamados/models.py:2699` |
| Ausência | Edição de tipo/datas/minutos; substituição de estado; reabertura da mesma linha; exclusão de férias e rejeição em lote. `app/ponto/services/ausencia.py:498`; `app/ponto/services/ausencia.py:824`; `app/ponto/services/ausencia.py:1141`; `app/ponto/services/ausencia.py:1262`; `app/ponto/services/ausencia.py:1281` |
| Batida | Alteração de tipo/aprovação/retratação/origem; retratação; exclusão de offline pendente. `app/ponto/registro_batida.py:203`; `app/ponto/registro_batida.py:252`; `app/ponto/registro_batida.py:274`; `app/ponto/registro_batida.py:285` |

**(3) Alimentação e desenho da medida**

| Caminho | Custo que a leitura estática permite afirmar |
|---|---|
| `CargaDaJanela` | Lote real para muitos colaboradores na mesma janela. Base não vazia: **5–7 consultas**, conforme geradoras adicionais e necessidade da última batida; consultas auxiliares dos corpos ficam fora desse subtotal. Logo, parcela base amortizada `5..7 / (N × D)`, não “5–7 por dia”. `app/ponto/turnos.py:2931`; `app/ponto/turnos.py:2943`; `app/ponto/turnos.py:2962`; `app/escala/alimentacao.py:148` |
| Cartório | Por colaborador: batidas da janela, primeira batida e chamados; perguntas vêm em prefetch. Cobertura de ausência é consultada por célula. Grade, veto, teto, C6 e novas passadas acrescentam custo variável. **Não há número total fixo por colab-dia demonstrado.** `app/ponto/services/cartorio.py:117`; `app/ponto/services/cartorio.py:1123`; `app/ponto/services/cartorio.py:1154`; `app/ponto/services/cartorio.py:1306` |
| Comando do cartório | Acrescenta consulta da fila e carregamento de colaborador/células por colaborador. `app/ponto/management/commands/processar_cartorio.py:49` |
| `apurar_por_fatia` | Recebe um colaborador e listas já carregadas; monta motores por callback. Não é o carregador de N colaboradores e seu custo completo depende do callback. `app/ponto/services/fechamento.py:132`; `app/ponto/services/fechamento.py:164` |
| `alimentar_portas` | Faz lote de células, mas lê `qs_vivos()` e `qs_na_fila()` globais; C6 ainda pode consultar por pergunta e app por colaborador. Seu quociente por N×D não representa custo marginal constante. `app/chamados/juizes.py:1261`; `app/chamados/juizes.py:1287`; `app/chamados/juizes.py:1312` |

O script seguinte é **texto proposto, não executado [nao verificado]**. Mede as fontes atuais e mantém a parcela global de portas separada; não mede um feed imutável inexistente nem uma implementação futura de `tarefa_do_dia`. As APIs usadas correspondem aos carregadores acima e às fontes de decisões/efeitos já identificadas. `app/ponto/turnos.py:2936`; `app/chamados/models.py:3179`; `app/chamados/juizes.py:1260`.

Cole no `manage.py shell` já configurado para a sombra e o tenant pretendido. A execução e a configuração desse ambiente permanecem **[nao verificado]**.

```python
from datetime import date, datetime, time, timedelta
from time import perf_counter
from django.db import connection, transaction
from django.db.models import Q
from django.test.utils import CaptureQueriesContext
from django.utils import timezone

from colaboradores.models import Colaborador
from ponto.models import Batida, Ausencia
from chamados.models import (
    ChamadoColaborador, DisputaSupervisao,
    PerguntaDisputa, RespostaAnterior,
)
from ponto.turnos import CargaDaJanela, cobertura_ausencia_periodo
from ponto.precedencia import fatos_do_periodo
from ponto.services.cartorio import marcos_vencidos
from chamados.juizes import alimentar_portas, porta_alcancavel_do_dia

N = 100
INI, FIM = date(2026, 10, 21), date(2026, 11, 5)
AGORA = timezone.now()
assert INI <= FIM
assert connection.vendor == "postgresql"
assert not connection.in_atomic_block

medidas, dados = [], {}

def medir(nome, fn):
    # Apenas instrumentação em memória.
    connection.queries_log.clear()
    t0 = perf_counter()
    with CaptureQueriesContext(connection) as cap:
        valor = fn()
    segundos = perf_counter() - t0
    if len(connection.queries_log) >= connection.queries_limit:
        raise RuntimeError("Captura saturada; não reportar contagem truncada")
    medidas.append((nome, segundos, len(cap)))
    return valor

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
        )

    colabs = medir("selecao", lambda: list(
        Colaborador.objects.select_related("empresa").order_by("pk")[:N]
    ))
    ids = [c.pk for c in colabs]
    assert ids, "Universo vazio"
    den = len(ids) * ((FIM - INI).days + 1)

    carga = medir("carga_lote", lambda: CargaDaJanela(colabs, INI, FIM))
    cels = [
        cel for c in colabs
        for cel in carga.celulas_de(c.pk, INI, FIM).values()
    ]

    dados["geometria"] = medir("geometria", lambda: {
        c.pk: (
            carga.turnos(c),
            carga.dias_encerrados(c, AGORA),
            carga.dia_das_batidas(c),
        ) for c in colabs
    })

    def decisoes():
        # Vida inteira, explicitamente: não cortar versões anteriores.
        ps = list(PerguntaDisputa.objects.filter(
            disputa__colaborador_id__in=ids
        ).values())
        pids = [p["id"] for p in ps]
        return {
            "perguntas": ps,
            "anteriores": list(RespostaAnterior.objects.filter(
                pergunta_id__in=pids).values()),
            "disputas": list(DisputaSupervisao.objects.filter(
                colaborador_id__in=ids).values()),
            "chamados": list(ChamadoColaborador.objects.filter(
                colaborador_id__in=ids).values()),
        }

    dados["decisoes"] = medir("decisoes_atuais_historico", decisoes)
    pids = [p["id"] for p in dados["decisoes"]["perguntas"]]
    lo = timezone.make_aware(datetime.combine(carga.pad_ini, time.min))
    hi = timezone.make_aware(datetime.combine(
        carga.pad_fim + timedelta(days=1), time.min
    ))

    dados["efeitos"] = medir("efeitos_brutos", lambda: {
        "batidas": list(Batida.objects.filter(
            Q(colaborador_id__in=ids, timestamp__gte=lo, timestamp__lt=hi)
            | Q(pergunta_origem_id__in=pids)
        ).values()),
        "ausencias": list(Ausencia.objects.filter(
            colaborador_id__in=ids).values()),
    })

    def juizos():
        out = {}
        for c in colabs:
            out[c.pk] = {
                "fatos": fatos_do_periodo(c, INI, FIM),
                "cobranca": cobertura_ausencia_periodo(
                    c, INI, FIM, para="cobranca"),
                "folha": cobertura_ausencia_periodo(
                    c, INI, FIM, para="folha"),
                "teto": {
                    cel.pk: marcos_vencidos(c, cel, cel.data, AGORA)
                    for cel in carga.celulas_de(c.pk, INI, FIM).values()
                },
            }
        return out

    dados["juizos"] = medir("precedencia_cobertura_teto", juizos)

    def portas():
        alim = alimentar_portas([cel.pk for cel in cels], colabs=colabs)
        return {
            cel.pk: porta_alcancavel_do_dia(cel.pk, alimentacao=alim)
            for cel in cels
        }

    dados["portas"] = medir("portas_com_universo_global", portas)

    print({"N": len(ids), "colab_dias": den, "celulas": len(cels)})
    for nome, segundos, queries in medidas:
        print({
            "etapa": nome, "segundos": segundos, "queries": queries,
            "ms_por_colab_dia": 1000 * segundos / den,
            "queries_por_colab_dia": queries / den,
        })
    print({
        "total_segundos": sum(s for _, s, _ in 

[truncado em 20000 de 31108 caracteres]

## 06/11 21:40 · sessao 361072d4a9c5 · - · - · origem=Ronald (tmux)

Item (5), somente leitura; números de produção/DIFF efetivo das competências 10 e 11: **[nao verificado]**.

| Leitor/efeito | O que muda ao re-chavear | Referência |
|---|---|---|
| Fio: data e aviso de divergência | `ev=p.data_evento`; compara com `dia_do_fato(ch, fortes=True)` do chamado que lista | `app/chamados/services/fio.py:77-88` |
| Modal do fio | Mostra data da pergunta e data do chamado quando divergentes; senão “Refere-se ao dia…” | `app/templates/chamados/partials/modal_fio.html:161-182` |
| Cartão atendimento | Exibe diretamente `p.data_evento` | `app/templates/chamados/partials/_card_atendimento.html:87-90` |
| Ficha do dia | Pergunta passa de um dia para outro porque consulta `data_evento=data` | `app/ponto/services/dia_do_colab.py:64-71` |
| Prévia da folha | Universo da competência e agrupamento por dia usam `data_evento`; sinal “resposta”, “pergunta viva” ou “nada” acompanha novo dia | `app/folha/previa.py:64-67`, `app/folha/previa.py:95`, `app/folha/previa.py:139-146` |
| Diagnóstico de pergunta no app | “pergunta viva em outro chamado” e pergunta encerrada do marco são procuradas no dia | `app/chamados/services/pergunta_no_app.py:92-95`, `app/chamados/services/pergunta_no_app.py:109-111` |
| Cobrança | Lista de dias pendentes usa `data_evento` | `app/chamados/services/cobranca.py:81-90` |
| Decisão administrativa do dia | Seleção de perguntas a fechar é por `data_evento=data` | `app/ponto/services/veredito_celula.py:174-178` |
| Validação e folha por evento | Competência agendada para recálculo recebe `perg.data_evento` | `app/chamados/services/validacao.py:395-408` |
| Expiração/veredito persistido | `janela_expira_em` deriva de `data_evento`; reapontamento exige relavra no mesmo save | `app/chamados/veredito_lavrado.py:95-118`, `app/chamados/veredito_lavrado.py:227-231`, `app/chamados/services/fio.py:23-25` |
| Outbox | `data_evento` é campo que acorda célula; busca prefere FK `p.celula`, só depois `(colab,data_evento)` | `app/chamados/models.py:2682-2710` |

Para FM/DiaPago, há **efeito indireto a medir**, sem prova fornecida de alteração monetária: fechamento alimenta o motor com batidas, escalas e células, e recebe `resultado`; a lavratura do DiaPago recebe esse mesmo resultado e os mapas da grade. (`app/ponto/services/fechamento.py:725-747`, `app/ponto/services/fechamento.py:1175-1211`) A migração da data da pergunta, por si só, não demonstra diferença nas horas. **[nao verificado quanto ao resultado de produção]**

DIFF FM sem escrever: porta real `recalcular_fechamento_mes(10/11, 2026, ..., somente_leitura=True, lavrar_dias_pagos=False, retornar_objeto=True, coletar_furos={})`; retorna objeto destacado e furos derivados pelo mesmo percurso. (`app/ponto/services/fechamento.py:439-474`, `app/ponto/services/fechamento.py:1112-1120`, `app/ponto/services/fechamento.py:1293-1295`) `fechamento_lido` já usa esse modo para uma pessoa. (`app/ponto/services/fechamento.py:1743-1745`)

**Não usar `lavrar_dias_pagos=True` em sonda só leitura:** ele chama o lavrador mesmo com `somente_leitura=True`, e o lavrador termina em `delete` e `bulk_create`. (`app/ponto/services/fechamento.py:1175-1185`, `app/ponto/services/dia_pago.py:357-358`) Não há retorno de projeção DiaPago no retorno FM acima; a comparação por dia pode interceptar a chamada real a `dia_pago.lavrar`, deixando executar sua derivação em memória e substituindo as operações finais de persistência — proposta de ensaio, **[nao verificado por execução]**. Os campos monetários canônicos são `CAMPOS_DIA` e `CAMPOS_AJUSTE`; ajuste inclui DSR e banco, e a soma existente arredonda os totais a duas casas. (`app/ponto/services/dia_pago.py:34-44`, `app/ponto/services/dia_pago.py:348-356`, `app/ponto/services/dia_pago.py:556-561`)

Competências 10/11 devem ser recortadas pela janela da empresa, não pelo mês civil: `folha.export.janela_competencia` delega a `janela_fechamento`, que usa `corte_da_empresa`. (`app/folha/export.py:47-51`, `app/ponto/janelas.py:36-63`) Para cada par antigo/novo, o roteamento existente é `competencias_do_fato` → `competencias_do_intervalo` → `janela_atual(cur, empresa)`, sem uma regra própria de dia. (`app/ponto/services/fechamento.py:1578-1588`, `app/ponto/services/dia_pago.py:711-716`) Janela concreta de cada empresa, deltas FM, deltas DiaPago, retenção/exportação e zeros nas competências 10/11: **[nao verificado]**.

## 06/11 21:37 · sessao 86792b3bca5e · - · - · origem=Ronald (tmux)

Censo conferido, sem executar Django nem escrever arquivos. Não há `AGENTS.md` listado por `rg --files -g AGENTS.md`; instruções eventualmente fora da árvore permanecem [nao verificado].

**Unidade recomendada: funções/portas que persistem alteração após o nascimento**, sem somar wrappers e auxiliares que só alteram memória. Nesse recorte são **16: 1 Chamado + 3 Disputa + 12 Pergunta**. A soma inclui os campos de resposta/validação que hoje decidem pendência, não apenas um campo chamado `status`. Fontes abaixo.

| Entidade | Porta persistente | Campo/efeito |
|---|---|---|
| Chamado | `_gravar_estado` | `status_local`, `resolvido_em`, `via_encerramento`; atribuições em `app/chamados/models.py:222`, `app/chamados/models.py:238`, `app/chamados/models.py:242`. |
| Disputa | `fechar` | `fechada_em`, autor, resolução, motivo: `app/chamados/models.py:1861`. |
| Disputa | `reabrir_admin` | Limpa os quatro anteriores: `app/chamados/models.py:2063`. |
| Disputa | `reabrir_sistema` | Limpa os quatro anteriores: `app/chamados/models.py:2087`. |
| Pergunta | `responder_pergunta_disputa_core` | Substitui resposta/instante; desfaz validação anterior: `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1295`, save em `app/chamados/services/disputa_emissao.py:1301`. |
| Pergunta | `reabrir_para_correcao` | Zera texto, resposta e validação: `app/chamados/services/disputa_emissao.py:1393`, persistência em `app/chamados/services/disputa_emissao.py:1414` ou `app/chamados/services/disputa_emissao.py:1416`. |
| Pergunta | `responder_automaticamente` | Escreve texto/instante: `app/chamados/services/disputa_emissao.py:1433`. |
| Pergunta | `carimbar_resposta_pela_trilha` | Reconstrói instante: `app/chamados/services/disputa_emissao.py:1450`. |
| Pergunta | `_carimbar_validacao` | Escreve validação/autor: `app/chamados/services/validacao.py:131`. |
| Pergunta | `devolver_validacao_recusada` | Limpa validação/autor: `app/chamados/services/validacao.py:188`. |
| Pergunta | `carimbar` | Carimbo materialização/via e, opcionalmente, resposta/validação: `app/chamados/models.py:2586`, `app/chamados/models.py:2589`, `app/chamados/models.py:2592`, save em `app/chamados/models.py:2609`. |
| Pergunta | `carimbar_identidade` | Carimbo materialização/via: `app/chamados/models.py:2636`, save em `app/chamados/models.py:2644`. |
| Pergunta | `descarimbar` | Limpa carimbo/via: `app/chamados/models.py:2648`, save em `app/chamados/models.py:2652`. |
| Pergunta | `descarimbar_em_lote` | Limpa carimbo/via e outros campos recebidos: `app/chamados/models.py:2699`. |
| Pergunta | `encerrar_por_fato_no_espelho` | Escreve somente `via_resolucao`: `app/chamados/models.py:2709`. |
| Pergunta | `relavrar` | **Outro escritor de `via_resolucao`**, fora da porta do carimbo: `app/chamados/services/relavra_via.py:123`, save em `app/chamados/services/relavra_via.py:125`. |

Os nascimentos explícitos de Chamado acrescentam **duas portas**, resultando em **18 no recorte com esses nascimentos**: `abrir` fornece `status_local` nos defaults em `app/chamados/models.py:899`; `criar` o grava no create em `app/chamados/models.py:1056`. Nascimentos de pergunta/disputa com valores vazios por default devem ficar em categoria separada para não comparar unidades diferentes; pergunta nasce em `app/chamados/models.py:2476`, defaults de resposta em `app/chamados/models.py:2227`, default de disputa aberta em `app/chamados/models.py:1687`.

Não somar como escritores persistentes: `_escrever_carimbo` é auxiliar em memória (`app/chamados/models.py:2556`); `_desfazer_em_memoria` restaura memória (`app/chamados/models.py:2616`); `_vivas_entre` troca/restaura status em memória (`app/chamados/models.py:339`, `app/chamados/models.py:354`); `_universo_da_ressuscitacao` altera cópia (`app/chamados/models.py:1443`, `app/chamados/models.py:1456`). O selo AST de status declara limites de resolução dinâmica em `app/chamados/tests/test_selo_status_so_pelas_portas.py:15`; exclusão absoluta de escritores indiretos permanece [nao verificado].

**Extras que influem na pendência/exibição e precisam aparecer fora desse subtotal:**

| Campos | Escritores | Leitores |
|---|---|---|
| `pendencia_admin` | `_materializar_resposta_hora`, único sítio encontrado: `app/chamados/services/materializacao.py:825`. | `explicador` dá dono admin antes de consultar efeito: `app/chamados/services/explicador.py:85`, `app/chamados/services/explicador.py:93`. |
| `recusa_motivo` | Cinco funções encontradas: `responder_pergunta_disputa_core` em `app/chamados/services/disputa_emissao.py:1340`; `_materializar_resposta_hora` em `app/chamados/services/materializacao.py:825`; `_carimbar_recusa` em `app/chamados/services/materializacao.py:1077`; `recusar_pergunta` em `app/chamados/services/validacao.py:545`; `carimbar_identidade` em `app/chamados/models.py:2642`. | Motivo da pendência no explicador: `app/chamados/services/explicador.py:90`; habilita devolução de validação em `app/chamados/services/validacao.py:184`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar` produz/grava em `app/chamados/veredito_lavrado.py:227`; `gravar_lavras_em_lote` persiste em `app/chamados/veredito_lavrado.py:207`; command também **produz** os valores em `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, mas persiste pela porta de lote em `app/chamados/management/commands/backfill_veredito_lavrado.py:143`. | `esta_stale` em `app/chamados/veredito_lavrado.py:179`; crédito admin exclui materializada + VL_SIM em `app/chamados/services/raiox.py:133`. |
| `silenciado_ate/por/motivo`, `resgatado_em` | `silenciar` em `app/chamados/services/arquivo.py:166`; `levantar` em `app/chamados/services/arquivo.py:201`. | Painel exclui silêncio futuro em `app/chamados/services/painel.py:186`; contrato declara que não muda status nem apuração em `app/chamados/services/arquivo.py:13`. |
| `data_conferida_em/por` | `conferir_data` em `app/chamados/services/validacao.py:617`. | Materializador barra divergência sem conferência em `app/chamados/services/materializacao.py:1168`. |
| `chamado_destino`, `data_evento` | `PerguntaDisputa.abrir` reaponta em `app/chamados/models.py:2450`, salva em `app/chamados/models.py:2474`, nasce em `app/chamados/models.py:2476`; `fio.reapontar_resolvedora` preenche data vazia em `app/chamados/services/fio.py:834`, reaponta/salva em `app/chamados/services/fio.py:845`. | Âncora do fio em `app/chamados/services/fio.py:116`; crédito destino/pai em `app/chamados/services/raiox.py:54`. |

**Leitores do núcleo:** `status_local` alimenta universo e fila em `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:187`; `resolvido_em/via_encerramento` sustentam veto de reabertura sem fato novo em `app/chamados/models.py:1303`; `fechada_em` define `.aberta` em `app/chamados/models.py:1765`, restringe badge em `app/chamados/models.py:1737` e universo PWA em `app/chamados/services/fio.py:213`. A API busca perguntas sem o filtro de disputa aberta em `app/chamados/services/pergunta_no_app.py:46`.

Resposta/validação/materialização/via são lidas pelo C6 em `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`; `.estado` deriva rótulo desses campos em `app/chamados/models.py:2499`; o materializador exige respondida+validada+materializada nula em `app/chamados/services/materializacao.py:1344`.

**Imutabilidade adicional importante:** `DisputaSupervisao.remover_motivo` **apaga a pergunta**, inclusive respondida: operação efetiva em `app/chamados/models.py:1976`, comportamento documentado em `app/chamados/models.py:1960`. Isso não está no subtotal de atribuições de campos; é uma porta adicional de remoção da própria decisão. `reabrir_para_correcao` preserva versão anterior em `RespostaAnterior` antes de limpar em `app/chamados/services/disputa_emissao.py:1406`; a resposta humana ordinária simplesmente sobrescreve o campo em `app/chamados/services/disputa_emissao.py:1282`.

Para v3/v4, contagem **implementada** de escritores novos permanece [nao verificado]. Pelo desenho do usuário, v3 pretende um escritor de estado da obrigação; v4 pretende zero escritores de estado derivado da tarefa. Isso não significa apagar os escritores de decisões, efeitos, arquivos ou cache; as fontes acima mostram que essas categorias hoje compartilham campos e portas.

_2603 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
