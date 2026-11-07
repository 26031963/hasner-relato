# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 21:13** · publicado em **06/11/2026 21:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 21:13 · sessao b38bc38e9798 · V2-BUGS-I2 · sonda · origem=Code

```python
# Sonda não executada; resultados por colaborador: [nao verificado].
# Caminho real, sem remontar motores:
# app/ponto/services/fechamento.py:724,754
# Leitura sem lavratura:
# app/ponto/services/fechamento.py:539,876,1121,1184
# Proteção adicional: transação PostgreSQL READ ONLY, sempre revertida.
# Comportamento dessa proteção no envelope: [nao verificado].

import sys
sys.dont_write_bytecode = True

import ast
import inspect
import io
import json
import logging
import math
import os
import re
from contextlib import redirect_stdout, redirect_stderr
from datetime import date

from django.db import connection, transaction
from django.utils import timezone
from ponto.services import fechamento as F
from ponto import motor_calculo_v2 as V
from core import regua_cct as R


PKS = [191, 270, 292, 301, 390, 468, 469, 825, 849, 865]
DIA = date(2026, 10, 12)
JANELA = (date(2026, 9, 21), date(2026, 10, 20))

REF_F = "app/ponto/services/fechamento.py:754"
REF_HE = "app/ponto/motor_calculo_v2.py:1014"
REF_M = {
    "M1": "app/ponto/motor_calculo_v2.py:3093",
    "M2": "app/ponto/motor_calculo_v2.py:3339",
    "M3": (
        "app/ponto/services/fechamento.py:955; "
        "app/ponto/services/dia_pago.py:244"
    ),
    "OUTRO": REF_HE,
}

COD_AP = F.apurar_por_fatia.__code__
COD_REC = F.recalcular_fechamento_mes.__code__
[nome] = {
    V.MotorTurnoPartido.calcular_mes.__code__: (3093, "M1"),
    V.MotorComercial.calcular_mes.__code__: (3339, "M2"),
}

# Captura também fallbacks com except Exception nos módulos observados.
# Exemplos: fechamento.py:1074,1080,1296; regua_cct.py:447.
HANDLERS = {}
for modulo in (F, V, R):
    caminho = os.path.abspath(inspect.getsourcefile(modulo))
    arvore = ast.parse(inspect.getsource(modulo))
    linhas = set()
    for node in ast.walk(arvore):
        if isinstance(node, ast.ExceptHandler):
            amplo = (
                node.type is None
                or isinstance(node.type, ast.Name)
                and node.type.id in {"Exception", "BaseException"}
            )
            if amplo:
                linhas.add(node.body[0].lineno)
    HANDLERS[caminho] = linhas


def numero(valor):
    n = float(valor)
    if not math.isfinite(n):
        raise ValueError("valor numerico nao finito")
    return n


def local(dt):
    return None if dt is None else timezone.localtime(dt).strftime(
        "%Y-%m-%d %H:%M:%S"
    )


def registrar_erro(st, exc, fonte):
    if not any(exc is anterior for anterior, _ in st["erros"]):
        st["erros"].append((exc, fonte))


def ocultar(st, texto):
    for nome in sorted(st["nomes"], key=len, reverse=True):
        texto = re.sub(re.escape(nome), st["colab"], texto, flags=re.I)
    return re.sub(
        r"(?<!\d)\d{3}\.?\d{3}\.?\d{3}-?\d{2}(?!\d)",
        "[CPF_REMOVIDO]",
        texto,
    )


def construir_trace(st):
    def trace(frame, evento, arg):
        caminho = os.path.abspath(frame.f_code.co_filename)
        if caminho not in HANDLERS:
            return None

        try:
            codigo = frame.f_code
            L = frame.f_locals
            linha = frame.f_lineno

            if codigo is COD_REC:
                colab = L.get("colab")
                if colab is not None:
                    nome = colab.__dict__.get("nome")
                    if isinstance(nome, str) and nome:
                        st["nomes"].add(nome)

            if evento == "line":
                if linha in HANDLERS[caminho]:
                    exc = sys.exc_info()[1]
                    # Ausência declarada, não erro: fechamento.py:54,1082.
                    if exc is not None and not isinstance(exc, F._GradeSemCelula):
                        registrar_erro(
                            st, exc, "%s:%d" % (caminho, linha)
                        )

                # Depois do calcular_mes real, antes da composição.
                # app/ponto/services/fechamento.py:286,294
                if codigo is COD_AP and linha == 294:
                    r = L["r_fatia"]
                    if id(r) not in st["fatias_vistas"]:
                        st["fatias_vistas"].add(id(r))
                        st["fatias"].append((
                            L["fatia_ini"], L["fatia_fim"], L["motor"], r
                        ))

                # Dentro do ramo verdadeiro: não reavalia feriado/dobra.
                ramo = [nome].get(codigo)
                if ramo is not None and linha == ramo[0]:
                    ultimo = L["ultimo"]
                    for p in L["periodos_dia"]:
                        st["ramos"][id(p)] = (
                            ramo[1], id(p) == id(ultimo)
                        )

                # Partição efetiva do fechamento, inclusive quando vazia.
                # app/ponto/services/fechamento.py:945,958
                if codigo is COD_REC and linha == 958:
                    st["ft_feriado"] = {
                        id(p) for p in L["_ft_feriado"]
                    }
                    st["janela"] = (L["_data_ini"], L["_data_fim"])

            if evento == "return" and codigo is COD_AP and arg is not None:
                st["apuracao"] = arg

        except Exception as exc:
            # Uma falha na instrumentação também invalida o colaborador.
            registrar_erro(st, exc, "[nao verificado]:instrumentacao")

        return trace

    return trace


def analisar(st, leitura, pk):
    if pk not in leitura:
        raise RuntimeError("colaborador ausente do retorno do recalculo")
    if st["apuracao"] is None:
        raise RuntimeError("apuracao real nao capturada")
    if st["ft_feriado"] is None:
        raise RuntimeError("particao ft_feriado nao capturada")
    if st["janela"] != JANELA:
        raise RuntimeError("janela real divergente: %r" % (st["janela"],))

    donos = {}
    linhas = []
    for ini, fim, motor, resultado_fatia in st["fatias"]:
        linhas.append({
            "tipo": "motor",
            "colab": st["colab"],
            "fatia": [ini.isoformat(), fim.isoformat()],
            "classe": type(motor).__name__,
            "feriado_em_dobra": bool(motor.feriado_em_dobra),
            "fonte": "app/ponto/services/fechamento.py:279",
        })
        for lista in ("periodos", "periodos_ft"):
            for p in getattr(resultado_fatia, lista):
                if id(p) in donos and donos[id(p)] is not motor:
                    raise RuntimeError("periodo atribuido a motores distintos")
                donos[id(p)] = motor

    resultado = st["apuracao"].resultado
    selecionados = {}
    for lista in ("periodos", "periodos_ft"):
        for p in getattr(resultado, lista):
            if timezone.localtime(p.entrada).date() == DIA:
                item = selecionados.setdefault(id(p), [p, []])
                item[1].append("resultado." + lista)

    parcelas = []
    mecanismos = set()

    for p, listas in sorted(
        selecionados.values(), key=lambda item: item[0].entrada
    ):
        motor = donos[id(p)]
        promovido = id(p) in st["ft_feriado"]
        extra100 = numero(
            p.minutos_trabalhados if promovido else p.minutos_extra_100
        )

        # Mesmas pontas, extra50, intervalos e prorrogação da casca real.
        # app/ponto/motor_calculo_v2.py:1014
        n50, n100 = V.he_noturna(
            entrada=p.entrada,
            saida=p.saida,
            extra_50_min=p.minutos_extra_50,
            extra_100_min=extra100,
            prorrogacao=motor.prorrogacao_noturna_pos5h,
            intervalos=p.intervalos_do_motor,
        )
        n50, n100 = numero(n50), numero(n100)
        subset = numero(p.minutos_extra_100_noturna)

        if promovido:
            mecanismo = "M3"
            motivo = "promocao_ft_soma_HE100_sem_somar_subset_noturno"
        elif subset != 0:
            mecanismo = "OUTRO"
            motivo = "subset_do_periodo_ja_e_diferente_de_zero"
        elif id(p) in st["ramos"]:
            mecanismo, ultimo = st["ramos"][id(p)]
            motivo = (
                "dobra_no_ultimo_periodo_com_continue_sem_split"
                if ultimo else
                "HE100_da_jornada_concentrada_no_ultimo_periodo"
            )
        else:
            mecanismo = "OUTRO"
            if "resultado.periodos_ft" in listas:
                motivo = "FT_nao_promovido_a_ft_feriado"
            elif extra100 == 0:
                motivo = "base_HE100_zero_no_periodo"
            elif n100 == 0:
                motivo = "he_noturna_tambem_devolve_zero"
            else:
                motivo = "zero_sem_M1_M2_M3_observado_[nao verificado]"

        mecanismos.add(mecanismo)
        parcelas.append((mecanismo, n50, n100))
        linhas.append({
            "tipo": "periodo",
            "colab": st["colab"],
            "classe": type(motor).__name__,
            "feriado_em_dobra": bool(motor.feriado_em_dobra),
            "entrada_local": local(p.entrada),
            "saida_local": local(p.saida),
            "listas": listas,
            "ft_feriado": promovido,
            "dia_da_jornada": str(
                getattr(resultado, "dia_da_jornada", {}).get(
                    id(p), "[nao verificado]"
                )
            ),
            "minutos_trabalhados": numero(p.minutos_trabalhados),
            "minutos_noturnos_relogio": numero(p.minutos_noturnos_relogio),
            "minutos_noturnos_legais": numero(p.minutos_noturnos_legais),
            "minutos_extra_100": numero(p.minutos_extra_100),
            "minutos_extra_100_noturna": subset,
            "extra_100_usado_na_sonda": extra100,
            "extra_50_usado_na_sonda": numero(p.minutos_extra_50),
            "prorrogacao": bool(motor.prorrogacao_noturna_pos5h),
            "intervalos_locais": [
                [local(a), local(b)] for a, b in p.intervalos_do_motor
            ],
            "he_noturna_retorno_min_50_100": [n50, n100],
            "he100_noturna_h": n100 / 60,
            "mecanismo": mecanismo,
            "explicacao": motivo,
            "fonte": REF_M[mecanismo] + "; " + REF_HE,
        })

    if not selecionados:
        mecanismos.add("OUTRO")
        linhas.append({
            "tipo": "sem_periodo",
            "colab": st["colab"],
            "explicacao": "nenhuma_entrada_local_em_2026-10-12_nas_duas_listas",
            "fonte": "app/ponto/services/fechamento.py:294",
        })

    return linhas, parcelas, mecanismos


def emitir(item):
    print(json.dumps(item, ensure_ascii=True, sort_keys=True))


grupos = {
    m: {"colabs": set(), "n50": [], "n100": []}
    for m in ("M1", "M2", "M3", "OUTRO")
}
falhas = []
sucessos = 0
logging_anterior = logging.root.manager.disable

try:
    logging.disable(sys.maxsize)

    for pk in PKS:
        st = {
            "colab": "col%d" % pk,
            "nomes": set(),
            "erros": [],
            "fatias": [],
            "fatias_vistas": set(),
            "ramos": {},
            "apuracao": None,
            "ft_feriado": None,
            "janela": None,
        }
        linhas = parcelas = mecanismos = None
        trace_anterior = sys.gettrace()

        # Não encaminha logs/prints internos, inclusive o nome no handler.
        # app/ponto/services/fechamento.py:1299
        with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
            try:
                if connection.vendor != "postgresql":
                    raise RuntimeError("protecao READ ONLY exige PostgreSQL")
                if connection.in_atomic_block:
                    raise RuntimeError("sonda exige transacao propria")

                with transaction.atomic():
                    with connection.cursor() as cursor:
                        cursor.execute("SET TRANSACTION READ ONLY")
                    try:
                        sys.settrace(construir_trace(st))
                        leitura = F.recalcular_fechamento_mes(
                            10, 2026,
                            colaborador_ids=[pk],
                            somente_leitura=True,
                            lavrar_dias_pagos=False,
                            retornar_objeto=True,
                        )
                    finally:
                        sys.settrace(trace_anterior)
                        transaction.set_rollback(True)

                if not st["erros"]:
                    linhas, parcelas, mecanismos = analisar(st, leitura, pk)

            except Exception as exc:
                registrar_erro(st, exc, "[nao verificado]:sonda")
            finally:
                sys.settrace(trace_anterior)

        if st["erros"]:
            exc, fonte = st["erros"][0]
            falha = {
                "tipo": "erro",
                "colab": st["colab"],
                "n_excecoes_capturadas": len(st["erros"]),
                "primeiro_repr": ocultar(st, repr(exc)),
                "fonte": fonte,
            }
            falhas.append(falha)
            emitir(falha)
            continue

        sucessos += 1
        for linha in linhas:
            emitir(linha)
        for mecanismo in mecanismos:
            grupos[mecanismo]["colabs"].add(pk)
        for mecanismo, n50, n100 in parcelas:
            grupos[mecanismo]["n50"].append(n50)
            grupos[mecanismo]["n100"].append(n100)

finally:
    logging.disable(logging_anterior)

# Contagem distinta por mecanismo; um colaborador pode aparecer em mais de um.
# Totais incluem somente colaboradores sem erro; minutos / 60:
# app/ponto/motor_calculo_v2.py:409,451
for mecanismo, grupo in grupos.items():
    emitir({
        "tipo": "total_mecanismo",
        "mecanismo": mecanismo,
        "n_colabs": len(grupo["colabs"]),
        "he50_noturna_h": math.fsum(grupo["n50"]) / 60,
        "he100_noturna_h": math.fsum(grupo["n100"]) / 60,
        "fonte": REF_M[mecanismo] + "; " + REF_HE,
    })

emitir({
    "tipo": "total_geral",
    "colabs_solicitados": len(PKS),
    "colabs_validos": sucessos,
    "colabs_com_erro_excluidos_das_somas": len(falhas),
    "he50_noturna_h": math.fsum(
        n for g in grupos.values() for n in g["n50"]
    ) / 60,
    "he100_noturna_h": math.fsum(
        n for g in grupos.values() for n in g["n100"]
    ) / 60,
    "contagens_por_mecanismo_podem_se_sobrepor": True,
    "total_parcial": bool(falhas),
    "fonte": REF_HE,
})
print("ERROS %d | %s | %s" % (
    len(falhas),
    "INVALIDO" if falhas else "VALIDO",
    "app/ponto/services/fechamento.py:1296",
))
if falhas:
    print("PRIMEIRO_REPR %s | %s" % (
        falhas[0]["primeiro_repr"], falhas[0]["fonte"]
    ))
```

Funções reais da cópia chamadas, conforme o motor de cada fatia:

- `recalcular_fechamento_mes` — `app/ponto/services/fechamento.py:448`
- `apurar_por_fatia` — `app/ponto/services/fechamento.py:132`
- `_montar_motor` — `app/ponto/services/fechamento.py:724`
- `get_motor_cct` — `app/core/regua_cct.py:428`
- `get_motor` — `app/ponto/motor_calculo_v2.py:3602`
- `exige_memos` — `app/ponto/calculador/nucleo.py:474`
- `MotorBase.calcular_mes` — `app/ponto/motor_calculo_v2.py:2265`
- `MotorTurnoPartido.calcular_mes` — `app/ponto/motor_calculo_v2.py:2899`
- `MotorComercial.calcular_mes` — `app/ponto/motor_calculo_v2.py:3295`
- `he_noturna` — `app/ponto/motor_calculo_v2.py:409`
- Auxiliares de `he_noturna`: `_inicio_do_trecho_extra` — `app/ponto/motor_calculo_v2.py:378`; `minutos_noturnos` — `app/ponto/motor_calculo_v2.py:82`; `minutos_noturnos_dos_intervalos` — `app/ponto/motor_calculo_v2.py:121`.

## 06/11 21:10 · sessao a403f60ac888 · - · - · origem=Ronald (tmux)

A sonda pode chamar `recalcular_fechamento_mes(10, 2026, colaborador_ids=[pk], somente_leitura=True, lavrar_dias_pagos=False, retornar_objeto=True)` e observar as instâncias efetivamente usadas, preservando `_montar_motor` e `apurar_por_fatia` reais. (`app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:724`, `app/ponto/services/fechamento.py:754`)

Pontos seguros para `sys.settrace`, filtrando os `__code__` das funções:

- No frame `apurar_por_fatia`, evento `line` na linha **294**: `motor`, `r_fatia`, `esc`, `fatia_ini` e `fatia_fim` estão disponíveis após o cálculo. Deduplicar por `id(r_fatia)`, pois essa linha é cabeça de laço. (`app/ponto/services/fechamento.py:286`, `app/ponto/services/fechamento.py:294`)
- No mesmo frame, evento `return`: `arg` é `_Apuracao`, contendo resultado composto. As listas compostas conservam os mesmos objetos dos resultados individuais. (`app/ponto/services/fechamento.py:295`, `app/ponto/services/fechamento.py:306`, `app/ponto/services/fechamento.py:318`)
- No frame `recalcular_fechamento_mes`, evento `line` na linha **958**: `_ft_feriado` já foi classificado e existe mesmo quando vazio. Capturar identidades dessa lista. (`app/ponto/services/fechamento.py:945`, `app/ponto/services/fechamento.py:949`, `app/ponto/services/fechamento.py:958`)
- Erro fatal por colaborador é engolido em `except`; na linha **1297**, ler `frame.f_locals["_e_colab"]`. O log normal desse ramo contém `colab.nome`; desabilitar logging durante a sonda evita vazamento. (`app/ponto/services/fechamento.py:1296`, `app/ponto/services/fechamento.py:1299`)
- Existem erros internos engolidos na grade nas linhas **1074** e **1080**; observar esses handlers se a regra `ERROS` deve abranger falhas além do erro final do colaborador. `_GradeSemCelula` é uma ausência declarada tratada no mesmo ramo. (`app/ponto/services/fechamento.py:54`, `app/ponto/services/fechamento.py:1074`, `app/ponto/services/fechamento.py:1080`)

Garantias do modo leitura:

- Não consulta/trilha exportada para escrita quando `somente_leitura=True` e `lavrar_dias_pagos=False`. (`app/ponto/services/fechamento.py:539`)
- Cria `FechamentoMensal` destacado em memória, sem `get_or_create`. (`app/ponto/services/fechamento.py:876`, `app/ponto/services/fechamento.py:891`)
- Devolve o objeto destacado no dicionário. (`app/ponto/services/fechamento.py:1121`, `app/ponto/services/fechamento.py:1128`)
- Não entra na lavratura `DiaPago`. (`app/ponto/services/fechamento.py:1184`)

Funções reais relevantes e localização:

- `recalcular_fechamento_mes`: `app/ponto/services/fechamento.py:448`
- `_montar_motor`, função local real: `app/ponto/services/fechamento.py:724`
- `apurar_por_fatia`: `app/ponto/services/fechamento.py:132`
- `get_motor_cct`: `app/core/regua_cct.py:428`
- `get_motor`: `app/ponto/motor_calculo_v2.py:3602`
- `exige_memos`: `app/ponto/calculador/nucleo.py:474`

A escolha por classe deve ser lida do objeto capturado: `get_motor` considera geometria além do rótulo do template e pode converter `turno_partido` em `MotorComercial` ou `Motor12x36ComEscala`. (`app/ponto/motor_calculo_v2.py:3635`, `app/ponto/motor_calculo_v2.py:3663`)

## 06/11 21:09 · sessao 42f90021c573 · - · - · origem=Ronald (tmux)

- `app/ponto/motor_calculo_v2.py:409`: `he_noturna(*, entrada, saida, extra_50_min, extra_100_min, prorrogacao=True, intervalos=())` retorna `(minutos_50_noturnos, minutos_100_noturnos)`; a distribuição proporcional fica em `:446-451`.
- `app/ponto/motor_calculo_v2.py:1007`: `_aplicar_he_noturna` passa **entrada e saída cruas**, `minutos_extra_50`, `minutos_extra_100`, `motor.prorrogacao_noturna_pos5h` e `periodo.intervalos_do_motor` (`:1014-1018`). Não usar `efetivo_entrada/efetivo_saida` na sonda.
- Campos reais: `minutos_trabalhados` (`:213`), `minutos_noturnos_relogio` (`:214`), `minutos_noturnos_legais` (`:215`), `minutos_extra_100` (`:218`), `minutos_extra_100_feriado` (`:219`), `minutos_extra_100_noturna` (`:225`), `intervalos_do_motor` (`:247`).
- M1: `MotorTurnoPartido.calcular_mes` (`:2899`) zera extras/subsets (`:2933-2938`), atribui dobra (`:3092-3096`) e sai com `continue` (`:3101`); a chamada `_aplicar_he_noturna` fica no caminho normal (`:3147`).
- M2: `MotorComercial.calcular_mes` (`:3295`) zera extras/subsets (`:3319-3324`), atribui dobra (`:3337-3342`) e sai com `continue` (`:3352`); a chamada `_aplicar_he_noturna` fica no caminho normal (`:3399`).
- `_feriado_dobra_no_dia` retorna `bool(self.feriado_em_dobra) and self.eh_feriado(dia)` (`app/ponto/motor_calculo_v2.py:934`).
- Para comprovar o ramo efetivamente tomado sem reavaliar regras, um `sys.settrace` restrito aos code objects dos dois overrides pode registrar `frame.f_locals["ultimo"]` em eventos `line` nas linhas `3093` e `3339`: ambas já estão dentro da condição verdadeira da dobra (`app/ponto/motor_calculo_v2.py:3092-3093`, `:3337-3339`).
- O mapa `resultado.dia_da_jornada` inclui os períodos normais e FT no TurnoPartido (`app/ponto/motor_calculo_v2.py:2940-2959`).

## 06/11 21:09 · sessao ca281ecd97ff · - · - · origem=Ronald (tmux)

**Proposta de escala IA × modal: convergência confirmada.** IA chega por `_copiloto.html:180` → `ia_perguntar` → `_aplicar_escala`; modal chega por `modal_fio.html:48` → `fio` → `aplicar_do_fio`; ambos chamam `escala.services.propositor.aplicar`. (`app/core/views_ia.py:874`, `app/core/views_ia.py:660`, `app/chamados/services/fio.py:511`, `app/chamados/services/proposta_no_fio.py:151`, `app/core/intake.py:51`, `app/escala/services/propositor.py:730`)

A IA veta `grade_por_dia` no preview antes de guardar proposta, e sua confirmação revalida assinatura que inclui `tipo`; o modal veta na aplicação. Portanto, não provei caminho normal pelo qual a IA tente aplicar esse tipo. (`app/core/views_ia.py:598`, `app/core/views_ia.py:620`, `app/core/views_ia.py:650`, `app/escala/services/propositor.py:663`, `app/chamados/services/proposta_no_fio.py:141`)

A IA ignora retorno enquanto modal verifica `r['ok']`; porém o escritor real retorna sempre dicionário com `decisao_id/saida`, sem ramo `ok=False`. Essa assimetria **não demonstra resultado divergente nesta cópia**. (`app/core/views_ia.py:660`, `app/chamados/services/proposta_no_fio.py:152`, `app/escala/services/propositor.py:788`)

**Folga Plano atual × IA: divergência formal com duas telas comprovadas.**

- Plano: `escala/plano_folgas.html` → POST `/escala/plano-folgas/aplicar/` → `plano_folgas_aplicar` → `aplicar_folgas_em_lote` → `_foto_do_dia` + relavra. (`app/templates/escala/plano_folgas.html:171`, `app/escala/views_plano_folgas.py:144`, `app/escala/views_plano_folgas.py:185`, `app/escala/folgas.py:389`, `app/escala/folgas.py:395`)
- IA: `_copiloto.html` → POST `/ia/perguntar/` → `_aplicar_declaracao` → `declarar_dia` → `lancar_folga` + `fechar_o_fio_do_dia`. (`app/templates/core/_copiloto.html:180`, `app/core/views_ia.py:775`, `app/core/intake.py:78`, `app/ponto/services/declarar_dia.py:189`, `app/ponto/services/declarar_dia.py:204`)
- Plano cria/altera/remove tipos; IA só lança `FD`, recusa folga existente, usa vínculo do dia e aceita até 120 dias passados; Plano exige vínculo ativo e recusa competência anterior à corrente. (`app/escala/views_plano_folgas.py:169`, `app/escala/views_plano_folgas.py:176`, `app/escala/folgas.py:330`, `app/escala/folgas.py:385`, `app/ponto/services/declarar_dia.py:56`, `app/ponto/services/declarar_dia.py:101`, `app/ponto/services/declarar_dia.py:117`, `app/ponto/services/declarar_dia.py:189`)
- IA fecha fio explicitamente e grava `ia_declarou_dias`; Plano só chama foto/relavra. **Dinheiro/dia: sim potencial**, porque o conjunto de datas aceitas e o estado de folga podem diferir; ocorrência concreta em produção **[não verificado]**. (`app/ponto/services/declarar_dia.py:204`, `app/core/views_ia.py:778`, `app/escala/folgas.py:389`, `app/escala/folgas.py:395`)

**Falta Resolver dia × IA:** mesmo escritor `veredito_celula`, mas a IA tem teto de lote/antiguidade, veto de futuro e log próprio; endpoint Resolver dia acrescenta `reconciliar_apos_vinculo` após sucesso. Fechar fio é comum ao escritor, portanto não é diferença. (`app/ponto/services/declarar_dia.py:45`, `app/ponto/services/declarar_dia.py:101`, `app/ponto/services/declarar_dia.py:104`, `app/ponto/services/declarar_dia.py:213`, `app/core/views_ia.py:778`, `app/ponto/views.py:3340`, `app/ponto/views.py:3351`, `app/ponto/services/veredito_celula.py:325`)

## 06/11 21:08 · sessao aed10b5c350f · V2-BUGS-I2 · leitura · origem=Code

**A cópia contém três mecanismos que deixam HE100 positiva com o subset noturno zerado:** dois `continue` no recálculo diário e uma promoção de folga-feriado que não transporta o subset. O ramo `Motor12x36`, por sua vez, chama o split depois da dobra. `app/ponto/motor_calculo_v2.py:3101`, `app/ponto/motor_calculo_v2.py:3352`, `app/ponto/services/dia_pago.py:244`, `app/ponto/motor_calculo_v2.py:2855`.

Qual desses mecanismos atingiu os dez colaboradores, seus ciclos, motores selecionados, batidas e CCT efetivamente aplicada: **[não verificado]**. [nao verificado]

**1. Caminho de cálculo até DiaPago e FechamentoMensal**

| Etapa | Quem chama quem |
|---|---|
| Entrada do fechamento | `recalcular_fechamento_mes` define `_montar_motor` e chama `apurar_por_fatia`. `app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:724`, `app/ponto/services/fechamento.py:754`. |
| Escolha do motor | `_montar_motor` chama `get_motor_cct`; esta consulta `regua_para` quando habilitada e chama `get_motor`. `app/ponto/services/fechamento.py:735`, `app/core/regua_cct.py:458`, `app/core/regua_cct.py:479`. |
| Classe e política de feriado | `get_motor` escolhe a classe pela geometria/base e resolve `feriado_em_dobra` pelo ciclo, salvo parâmetro explícito. `app/ponto/motor_calculo_v2.py:3623`, `app/ponto/motor_calculo_v2.py:3633`, `app/ponto/motor_calculo_v2.py:3663`, `app/ponto/motor_calculo_v2.py:3669`. |
| Apuração | `apurar_por_fatia` chama `motor.calcular_mes`; compõe os períodos das fatias. `app/ponto/services/fechamento.py:286`, `app/ponto/services/fechamento.py:294`. |
| Montagem e cálculo dos pares | `MotorBase.calcular_mes` chama `montar_periodos` e, para cada par, `self.calcular_periodo`. `app/ponto/motor_calculo_v2.py:2354`, `app/ponto/motor_calculo_v2.py:2458`. |
| Geometria do 12x36 | `Motor12x36.montar_periodos → _periodos_via_autoridade → turnos_via_autoridade → parear_turnos`; o resultado passa por `periodos_de_turnos` e `_periodos_pelo_marco`. `app/ponto/motor_calculo_v2.py:2748`, `app/ponto/motor_calculo_v2.py:530`, `app/ponto/motor_calculo_v2.py:503`, `app/ponto/motor_calculo_v2.py:531`. |
| Cálculo do período 12x36 | `calcular_periodo` calcula duração/intervalo, chama `_escrever_noturnas`, decide a dobra e chama `_aplicar_he_noturna`. `app/ponto/motor_calculo_v2.py:2761`, `app/ponto/motor_calculo_v2.py:2774`, `app/ponto/motor_calculo_v2.py:2792`, `app/ponto/motor_calculo_v2.py:2809`, `app/ponto/motor_calculo_v2.py:2855`. |
| Split | `_aplicar_he_noturna → he_noturna → _inicio_do_trecho_extra`, `minutos_noturnos` e `minutos_noturnos_dos_intervalos`; o retorno é escrito em `periodo.minutos_extra_100_noturna`. `app/ponto/motor_calculo_v2.py:1014`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:442`, `app/ponto/motor_calculo_v2.py:1020`. |
| Separação trabalho/folga | O período vai para `resultado.periodos_ft` ou `resultado.periodos`. `app/ponto/motor_calculo_v2.py:2468`, `app/ponto/motor_calculo_v2.py:2492`. |
| Mensal | `ResultadoMes.horas_extra_100_noturna` soma os minutos de **`periodos`** e divide por 60; o fechamento copia e arredonda, persistindo por `save` ou `update`. `app/ponto/motor_calculo_v2.py:342`, `app/ponto/services/fechamento.py:924`, `app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1162`. |
| Diário | O fechamento chama `dia_pago.lavrar`; esta acumula `p.minutos_extra_100_noturna / 60`, instancia `DiaPago` e persiste por `bulk_create`. `app/ponto/services/fechamento.py:1194`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:343`, `app/ponto/services/dia_pago.py:358`. |

Há duas variações relevantes: `MotorComercial.calcular_mes` e `MotorTurnoPartido.calcular_mes` chamam o cálculo base e depois **refazem as extras por dia**, nos ramos detalhados abaixo. `app/ponto/motor_calculo_v2.py:2908`, `app/ponto/motor_calculo_v2.py:2933`, `app/ponto/motor_calculo_v2.py:3301`, `app/ponto/motor_calculo_v2.py:3318`.

`Motor12x36ComEscala.calcular_periodo` chama o cálculo 12x36; `MotorPlantao.calcular_periodo` também o reutiliza diretamente. `app/ponto/motor_calculo_v2.py:3523`, `app/ponto/motor_calculo_v2.py:3511`.

**2. Produção da HE100, chamada do split e origem do zero**

| Caminho | Produção da HE100 | Destino do split |
|---|---|---|
| **Motor12x36** | A dobra atribui `duracao` a `minutos_extra_100` e `minutos_extra_100_feriado`. `app/ponto/motor_calculo_v2.py:2809`. | **Chama o split**, depois do ramo de feriado. O wrapper entrega esse `minutos_extra_100` como `extra_100_min`. `app/ponto/motor_calculo_v2.py:2855`, `app/ponto/motor_calculo_v2.py:1016`. |
| **MotorTurnoPartido, recálculo mensal** | Atribui `trab_dia` à HE100 do último período. `app/ponto/motor_calculo_v2.py:3092`. | Zera previamente o subset em `:2938`; o **`continue` em `:3101`** pula a chamada em `:3147`. Nesse ramo, a HE100 do feriado **não chega ao split**. `app/ponto/motor_calculo_v2.py:2938`, `app/ponto/motor_calculo_v2.py:3101`, `app/ponto/motor_calculo_v2.py:3147`. |
| **MotorComercial, recálculo mensal** | Atribui `trab_dia` à HE100 do último período. `app/ponto/motor_calculo_v2.py:3337`. | O cálculo individual chamou o split, mas o mensal apaga o subset e faz **`continue` antes da nova chamada**. `app/ponto/motor_calculo_v2.py:3281`, `app/ponto/motor_calculo_v2.py:3324`, `app/ponto/motor_calculo_v2.py:3352`, `app/ponto/motor_calculo_v2.py:3399`. |
| **Promoção `periodos_ft → ft_feriado`** | O fechamento acrescenta trabalhado a HE100/HE100-feriado. `app/ponto/services/fechamento.py:954`. | O subset mensal já foi copiado somente de `resultado.periodos` e não recebe acréscimo nessa promoção. `app/ponto/services/fechamento.py:924`, `app/ponto/motor_calculo_v2.py:344`, `app/ponto/services/fechamento.py:955`. |

No **DiaPago**, esse último caminho é explícito: `ft_feriado` acrescenta HE100 e HE100-feriado; `_rubricas_da_folga` acrescenta somente noturno e intra. Como a linha nasce com zeros, HE100-noturna permanece zero, mesmo podendo existir no período original. `app/ponto/services/dia_pago.py:169`, `app/ponto/services/dia_pago.py:223`, `app/ponto/services/dia_pago.py:244`.

Portanto, **“o feriado nunca chama o split” é falso para esta cópia**; existem ramos que o chamam e ramos que o pulam ou descartam seu resultado. `app/ponto/motor_calculo_v2.py:2855`, `app/ponto/motor_calculo_v2.py:3101`, `app/ponto/motor_calculo_v2.py:3352`, `app/ponto/services/dia_pago.py:244`.

**3. Regra jurídica documentada e unidade do campo**

Pela regra **registrada na cópia**, havendo direito à dobra, a parcela de HE100 realizada na janela noturna é HE100 noturna, cumulando a condição noturna; o comentário fundamenta isso em art.73 e Súmula264. A Súmula60 participa da extensão da janela após 05h. `app/ponto/calculador/regras.py:793`, `app/ponto/motor_calculo_v2.py:412`, `app/core/regua_cct.py:153`.

Isso **não torna todo feriado do 12x36 automaticamente dobrado**: o piso local considera esse ciclo simples; `get_motor_cct` habilita a dobra pela cláusula cadastrada `feriado_12x36_em_dobra`. `app/ponto/motor_calculo_v2.py:3599`, `app/ponto/motor_calculo_v2.py:3670`, `app/core/regua_cct.py:473`.

A cl.38-d de Vigilantes documentada ali mantém o adicional noturno e afasta a hora reduzida no 12x36: hora de 60 minutos. Sua aplicação depende do cadastro e do ciclo. Ela, isoladamente, não é a cláusula que habilita a dobra do feriado. `app/core/regua_cct.py:127`, `app/core/regua_cct.py:136`, `app/core/regua_cct.py:470`, `app/core/regua_cct.py:477`.

Também não cabe presumir prorrogação após 05h para todo 12x36: o piso local é janela estrita; a CCT cadastrada pode conceder prorrogação. `app/core/regua_cct.py:159`, `app/core/regua_cct.py:166`.

**A unidade persistida de `horas_extras_100_noturna` é hora de relógio.** `he_noturna` mede a interseção temporal, desconta intervalos e reparte proporcionalmente entre HE50/HE100, sem multiplicar pelo fator; mensal, diário e calculador dividem esses minutos por 60. `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:448`, `app/ponto/motor_calculo_v2.py:344`, `app/ponto/services/dia_pago.py:184`, `app/ponto/calculador/regras.py:839`.

Já `horas_noturnas` usa minutos legais: relógio × fator, sendo o fator `60/52,5` ou `1` quando afastada a redução. Assim, **quando todo o trabalho noturno integra a HE100**, vale a relação `HE100_noturna = horas_noturnas / fator`; não é identidade geral para dias com apenas parte da jornada em HE100. `app/ponto/motor_calculo_v2.py:986`, `app/ponto/motor_calculo_v2.py:990`, `app/ponto/motor_calculo_v2.py:447`.

Vigência jurídica externa e aplicação concreta da convenção aos dez casos: **[não verificado]**.

**4. Leitores: TXT, espelho, PDF, oráculo e placar**

| Leitor | O que efetivamente faz |
|---|---|
| **TXT/Domínio — `eventos_do_fechamento`** | **Não usa o subset como evento próprio.** Emite HE100 total repartida em normal/feriado e adicional noturno total; o comentário proíbe duplicar os subsets. `app/folha/export.py:420`, `app/folha/export.py:424`, `app/folha/export.py:439`. |
| **Espelho diário** | Usa `rubricas_do_dia`, que entrega HE100 total e noturnas totais, sem expor HE100-noturna separada. `app/ponto/services/espelho.py:813`, `app/ponto/services/dia_pago.py:912`, `app/ponto/services/dia_pago.py:922`. |
| **PDF** | Desenha `dia['rubricas']`; não refaz esse split. `app/relatorios/pdf_espelho.py:910`. |
| **`pago_do_dia`** | Consulta o campo no teste de existência de dinheiro (`any`); o valor retornado soma trabalhadas, folga e folga-feriado, sem somar o subset. `app/ponto/services/dia_pago.py:861`, `app/ponto/services/dia_pago.py:890`, `app/ponto/services/dia_pago.py:895`. |
| **Somas de DiaPago** | `CAMPOS` inclui o campo; `soma_da_competencia` e `soma_do_periodo` agregam os valores gravados. `app/ponto/services/dia_pago.py:36`, `app/ponto/services/dia_pago.py:560`, `app/ponto/services/dia_pago.py:785`. |
| **Conferência DiaPago × fechamento** | Compara cada campo agregado com o fechamento; não recalcula a regra noturna. Dois zeros iguais passam nessa identidade. `app/folha/porta_export.py:388`, `app/folha/porta_export.py:451`. |
| **Placar R4** | Registra a identidade fechamento × soma DiaPago e remete ao contador de `porta_export`; não tem split próprio. `app/core/placar_estrutural.py:199`, `app/core/placar_estrutural.py:207`. |
| **Calculador/oráculo de rubricas** | **Recalcula o subset** usando `he_noturna`, alimentado pelas extras e pontas dos períodos do motor; não lê simplesmente o subset persistido. `app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/regras.py:832`. |
| **Oráculo E6 do espelho** | Sua função de leitura compara minutos realizados ou `pago_h`; não confere numericamente o subset HE100-noturna. `app/ponto/management/commands/e6_oraculo.py:251`. |

O serviço de espelho também executa o motor para construir sua autoridade, embora as rubricas diárias exibidas venham de DiaPago; portanto “o espelho só lê e nunca executa cálculo” seria amplo demais. `app/ponto/services/espelho.py:1164`, `app/ponto/services/espelho.py:1179`, `app/ponto/services/espelho.py:813`.

Outras leituras instrumentais incluem fotos do comando de recálculo, simulação, sonda `desvio_o68b` e script O263. Já a ocorrência no detector de leitores é vocabulário AST, não leitura numérica da rubrica. `app/ponto/management/commands/recalcular_fechamento.py:64`, `app/folha/management/commands/simular_folha.py:59`, `app/ponto/management/commands/desvio_o68b.py:117`, `bin/aplicar_o263_comp10.py:46`, `app/core/medida_leitor_do_dia.py:47`.

**5. Escritores e diferenças de significado**

| Escritor/caminho | Escrita |
|---|---|
| **Motor** | Inicializa zero e escreve resultado positivo do split em minutos; Comercial/Partido zeram novamente antes do recálculo diário. `app/ponto/motor_calculo_v2.py:225`, `app/ponto/motor_calculo_v2.py:1020`, `app/ponto/motor_calculo_v2.py:2938`, `app/ponto/motor_calculo_v2.py:3324`. |
| **Bloqueio sem previsto/autorização** | Zera o subset junto com trabalhado, noturno e demais minutos; sozinho não explica HE100 e noturno positivos. `app/ponto/motor_calculo_v2.py:1453`, `app/ponto/motor_calculo_v2.py:1473`. |
| **Fechamento mensal** | Copia `/60`, arredonda duas casas e persiste. `app/ponto/services/fechamento.py:924`, `app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1162`. |
| **DiaPago — versão motor** | Acumula `/60` dos períodos e persiste as linhas. `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:358`. |
| **Calculador/oráculo** | Calcula `/60`; a mesma `lavrar` substitui os campos recebidos em `rubricas=`, salvo os declarados `do_motor`. `app/ponto/calculador/regras.py:839`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:327`. |
| **Lavratura da versão oráculo** | O fechamento chama `rubricas_por_dia` e depois `lavrar(... rubricas=_rub_o, versao='oraculo')`. `app/ponto/services/fechamento.py:1264`, `app/ponto/services/fechamento.py:1270`, `app/ponto/services/fechamento.py:1287`. |
| **`recalcular_fechamento` / MotorPlantao** | São delegadores: comando → serviço canônico; Plantao → cálculo 12x36. `app/ponto/management/commands/recalcular_fechamento.py:118`, `app/ponto/motor_calculo_v2.py:3511`. |
| **`retratar_exportada`** | Pode criar dia ausente e ajuste com rubricas zero; nas linhas diárias existentes atualiza os minutos da grade. `app/ponto/services/dia_pago.py:469`, `app/ponto/services/dia_pago.py:476`, `app/ponto/services/dia_pago.py:479`, `app/ponto/services/dia_pago.py:513`. |
| **`restaurar_fechamento`** | Aceita campos válidos e grava valores recebidos, sem recalcular. O apply histórico pode entregar `gravado + delta` incluindo este campo. `app/ponto/services/fechamento.py:1366`, `app/ponto/services/fechamento.py:1385`, `app/ponto/management/commands/aplicar_09_he_intra.py:40`, `app/ponto/management/commands/aplicar_09_he_intra.py:146`. |

**A unidade dos derivadores principais coincide, mas os recortes não são universalmente iguais:**

- DiaPago escolhe o dia pelo mapa `dia_da_jornada`, com fallback para a data local da entrada; Comercial/Partido agrupam pelo dia do turno. A seleção de `ft_feriado` no fechamento consulta a data local da entrada. `app/ponto/services/dia_pago.py:128`, `app/ponto/services/dia_pago.py:132`, `app/ponto/motor_calculo_v2.py:2942`, `app/ponto/motor_calculo_v2.py:3313`, `app/ponto/services/fechamento.py:947`.
- O wrapper v2 passa entrada/saída do período; a alimentação do calculador usa pontas efetivas. `app/ponto/motor_calculo_v2.py:1015`, `app/ponto/calculador/alimentacao.py:83`, `app/ponto/calculador/alimentacao.py:106`.
- No ramo de folga, o calculador declara os splits como `nao_decide`, inclusive quando promove os totais para feriado; a exceção poupa apenas HE100 e HE100-feriado. Portanto o oráculo não afirma esse subset para todos os perfis. `app/ponto/calculador/regras.py:708`, `app/ponto/calculador/regras.py:711`, `app/ponto/calculador/regras.py:739`.

**6. Testes existentes** [nao verificado]

**Sim, há cobertura explícita da combinação**, com estes escopos:

| Teste | Afirmação | [nao verificado]
|---|---|
| `test_v3_dobra_sem_noturna` | Feriado 22–06 com pausa 02–03: HE100-noturna **7h**, noturnas reduzidas **8h**, HE50-noturna zero. Chama `regras.do_dia` com pontas fornecidas pela fixture. `app/ponto/tests/test_v3_dobra_sem_noturna.py:40`, `app/ponto/tests/test_v3_dobra_sem_noturna.py:68`, `app/ponto/tests/test_v3_dobra_sem_noturna.py:71`. |
| `test_v3_dobra_split_noturno_porta` | Pela porta de alimentação: HE100 **9h**, HE100-noturna **8h**, igual ao subset do período do motor; exige intervalo julgado. A fixture é **ciclo 6x1/base 12x36**. `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:43`, `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:73`. |
| Gêmeos com/sem pausa | A pausa noturna retira exatamente **1h** do subset. `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:83`. |
| Split em dia não feriado | Compara o calculador com a soma dos retornos de `he_noturna / 60`. `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:292`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:320`. |

Esses testes positivos não demonstram cobertura dos dois `continue` mensais nem da promoção `ft_feriado`; cobertura específica desses três mecanismos: **[não verificado]**. Execução e resultado atual dos testes: **[não verificado]**. [nao verificado]

**7. Contraexemplos e alcance além do vigilante 12x36**

| Perfil | Passa pelo mesmo ramo que perde o subset? |
|---|---|
| **Outros ciclos com dobra e motor Comercial/Partido** | Sim, estando em `resultado.periodos`: as condições exigem trabalho positivo e `_feriado_dobra_no_dia`, não profissão nem ciclo 12x36. `app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:3083`, `app/ponto/motor_calculo_v2.py:3092`, `app/ponto/motor_calculo_v2.py:3337`. |
| **Folga trabalhada comum** | Segue outro caminho: `periodos_ft`, rubrica própria de folga e adicional noturno; não entra nos laços diários de `resultado.periodos`. `app/ponto/motor_calculo_v2.py:2492`, `app/ponto/motor_calculo_v2.py:3083`, `app/ponto/motor_calculo_v2.py:3316`, `app/ponto/services/dia_pago.py:234`. |
| **Folga causada pelo feriado** | Segue a promoção `ft_feriado`, que perde o subset. O predicado admite **6x1, 5x2 e semanal**, com `trabalha_em_feriado=False`; não admite ciclo 12x36. `app/escala/models.py:32`, `app/escala/models.py:1390`, `app/ponto/services/fechamento.py:949`, `app/ponto/services/dia_pago.py:244`. |
| **DSR/domingo trabalhado** | Quando classificado como folga, segue `periodos_ft`; domingo previsto não vira folga pelo predicado semanal. Não é automaticamente o ramo de feriado. `app/ponto/motor_calculo_v2.py:950`, `app/ponto/motor_calculo_v2.py:2453`, `app/ponto/motor_calculo_v2.py:2492`. |
| **Turno começa no feriado e termina no seguinte** | O contrato local considera o dia inicial. Comercial/Partido podem perder o subset pelos `continue`; **MotorPlantao** reutiliza o caminho 12x36 que chama o split. `app/ponto/motor_calculo_v2.py:928`, `app/ponto/motor_calculo_v2.py:3312`, `app/ponto/motor_calculo_v2.py:3511`, `app/ponto/motor_calculo_v2.py:2855`. |
| **Feriado com intervalo noturno** | O intervalo não evita os `continue`. Quando chamado, o split desconta o intervalo; a lavratura de `ft_feriado` não chama esse split. `app/ponto/motor_calculo_v2.py:3101`, `app/ponto/motor_calculo_v2.py:3352`, `app/ponto/motor_calculo_v2.py:442`, `app/ponto/services/dia_pago.py:244`. |

## 06/11 21:06 · sessao 3fc1893735dc · - · - · origem=Ronald (tmux)

Universo próprio: **46 views POST administrativas escritoras: 33 colaboradores + 13 core**; autenticação e onboarding do próprio usuário, DELETE e redirects não entram. Base das rotas: `app/colaboradores/urls.py:9`, `app/core/urls.py:14`; métodos escritores listados abaixo.

**33 colaboradores** — todas em `app/colaboradores/views.py`:

`passe_livre:145; vincular_folga:212; vincular_posto:311; vincular_escala:373; form_empresa:485; form_praca:558; posto_add_colab:631; posto_remove_colab:658; form_posto:714; toggle_posto:876; admitir_colaborador:1099; editar_colaborador:1237; transferir_posto:1366; reativar_colaborador:1408; desligar_colaborador:1423; alterar_perfil:1457; resetar_senha:1474; resetar_onboarding:1491; disparar_onboarding:1514; disparar_onboarding_massa:1534; definir_senha_manual:1556; toggle_acesso:1583; conta_bancaria:1601; lancar_beneficio:1767; lancar_beneficio_lote:1818; cancelar_beneficio:1875; avaliar_solicitacao_beneficio:2046; linhas_transporte:2108; remover_linha_transporte:2140; configuracao_va:2149; agenda_bloco_criar:2261; agenda_enviar_aceite:2340; colab_regularizar:2948`.

**13 core**:

- `app/core/views_config.py`: `form_feriado:39`, `importar_feriados_nacionais:153`, `excluir_feriado:182`, `form_sindicato:258`, `form_aplicacao:387`.
- `app/core/views_quadro.py:13`: `quadro_setores_api`.
- `app/core/views_usuarios.py`: `alternar_super_admin:70`, `vincular_setor_usuario:191`, `criar_setor_ad:239`, `criar_usuario_ad:309`, `ficha_usuario_acao:324`, `ficha_setor_acao:356`.
- `app/core/views_ia.py:859`: `ia_perguntar`.

`parametros_sistema` recusa o POST sem gravar; setores e usuários clássicos apenas redirecionam. `app/core/views_config.py:486`, `app/core/views_setores.py:11`, `app/core/views_usuarios.py:21`.

### Grupos divergentes próprios

**1. Vincular/transferir colaborador para posto — grupo de três portas.**

- A: `/colaboradores/<cid>/vincular/posto/`, `app/colaboradores/views.py:311`, template `app/templates/colaboradores/partials/painel_vinculo.html:112` → `salvar_inteiro(...,'posto')` em `app/colaboradores/views.py:329` → escritor `app/colaboradores/services/colaborador.py:73`.
- B: `/colaboradores/postos/<pid>/colabs/add/`, `app/colaboradores/views.py:631`, template `app/templates/colaboradores/postos.html:98`, botão em `app/templates/colaboradores/partials/painel_posto.html:56` → `gravar(...,'posto',{'posto':posto})` em `app/colaboradores/views.py:645` → escritor `app/colaboradores/services/colaborador.py:55`.
- C: `/colaboradores/<cid>/transferir/`, `app/colaboradores/views.py:1366`, template `app/templates/colaboradores/editar.html:215` → `salvar_inteiro` em `app/colaboradores/views.py:1388`.

Só C atualiza também `empresa=novo_posto.empresa`; só C tem `gate_disputa`. A grava histórico de mudança que pula operações sem diferença; B/C gravam evento mesmo sem comparar antes/depois. B/C envolvem gravação+histórico em transação; A não envolve esse conjunto em `atomic`. `app/colaboradores/views.py:1386`, `app/colaboradores/views.py:1365`, `app/colaboradores/views.py:331`, `app/colaboradores/views.py:647`, `app/colaboradores/views.py:1393`, `app/colaboradores/services/historico_vinculo.py:10`, `app/colaboradores/services/historico_vinculo.py:20`, `app/colaboradores/views.py:643`, `app/colaboradores/views.py:1384`.

**Dinheiro/dia: sim, potencial financeiro**: o TXT seleciona fechamentos pela empresa atual do colaborador; mudar só posto ou posto+empresa pode mudar em qual CNPJ entra. Trocar esse cadastro não reescreve automaticamente o lugar das células. `app/folha/export.py:712`, `app/colaboradores/services/colaborador.py:47`.

**2. Ativar/desativar posto — duas portas.**

- A: `/colaboradores/postos/<pid>/editar/`, view `app/colaboradores/views.py:714`, checkbox `app/templates/colaboradores/posto_form.html:119` → `atualizar_posto` em `app/colaboradores/views.py:862` → escritor `app/colaboradores/services/estrutura.py:64`.
- B: `/colaboradores/postos/<pid>/toggle/`, view `app/colaboradores/views.py:876`, formulário `app/templates/colaboradores/partials/painel_posto.html:67` → `gravar_posto_ativo` em `app/colaboradores/views.py:893` → escritor `app/colaboradores/services/estrutura.py:73`.

Só B impede desativar posto com colaboradores ativos. A aceita o checkbox `ativo` e grava sem esse veto. Ambos passam pelo mesmo receiver que revalida disputas na reativação. `app/colaboradores/views.py:884`, `app/colaboradores/views.py:846`, `app/colaboradores/services/estrutura.py:64`, `app/colaboradores/signals.py:49`.

**Dinheiro/dia: não diretamente provado**; o resultado operacional difere: o painel continua podendo listar colaboradores ativos de um posto desativado, enquanto os seletores de cadastro filtram postos ativos. Efeito monetário específico `[não verificado]`. `app/colaboradores/views.py:602`, `app/colaboradores/views.py:1242`.

**3. Lançar benefício — três origens; mesmo escritor, políticas diferentes.**

- A: `/colaboradores/beneficios/novo/`, view `app/colaboradores/views.py:1767`, template `app/templates/colaboradores/beneficio_form.html:12` → `criar_beneficio` em `app/colaboradores/views.py:1794`.
- B: `/colaboradores/beneficios/lote/`, view `app/colaboradores/views.py:1818`, template `app/templates/colaboradores/beneficio_lote.html:17` → `criar_beneficio` em `app/colaboradores/views.py:1847`.
- C: `/colaboradores/beneficios/solicitacoes/<sid>/avaliar/`, view `app/colaboradores/views.py:2046`, template `app/templates/colaboradores/painel_solicitacao_beneficio.html:15` → `criar_beneficio` em `app/colaboradores/views.py:2075`.

Convergem em `app/colaboradores/services/beneficios.py:9`, que apenas cria. Só B ignora benefício ativo do mesmo tipo/competência; A exige campos e converte mês/ano com `int`, B usa saneadores; C pode criar VA/VT por aprovação com valor zero e mês/ano civil corrente, além de transicionar chamado, anexar nota, concluir solicitação e enviar push. Não há deduplicação no escritor comum. `app/colaboradores/views.py:1839`, `app/colaboradores/views.py:1780`, `app/colaboradores/views.py:1798`, `app/colaboradores/views.py:1825`, `app/colaboradores/views.py:2066`, `app/colaboradores/views.py:2079`, `app/colaboradores/views.py:2086`, `app/colaboradores/views.py:2089`, `app/colaboradores/services/beneficios.py:11`.

**Dinheiro: sim**, os leitores somam e exportam cada benefício ativo; duplicidade e valor zero chegam ao consolidado/CSV. Chegada ao DiaPago ou TXT da folha `[não verificado]`. `app/relatorios/views.py:616`, `app/relatorios/views.py:631`, `app/relatorios/views.py:642`, `app/colaboradores/views.py:1897`, `app/colaboradores/views.py:1906`.

**4. Cadastrar feriado — manual/importação.**

- A: `/configuracoes/feriados/novo/`, `app/core/views_config.py:39`, template indicado em `app/core/views_config.py:97` → escritor direto `Feriado.objects.create`, `app/core/views_config.py:81`.
- B: `/configuracoes/feriados/importar/`, `app/core/views_config.py:153`, formulário `app/templates/core/config/feriados.html:16` → `_importar_do_payload`, `app/core/views_config.py:124` → `get_or_create`, `app/core/views_config.py:141`.

Manual aceita abrangência informada e exige fonte; importação restringe dias pela allowlist, fixa abrangência nacional, nome/fonte da lei e deduplica. Manual registra auditoria por feriado; helper/view da importação não chamam `registrar_log`. Ambos têm o mesmo evento `post_save` de reavaliação, portanto esse efeito não é exclusivo do manual. `app/core/views_config.py:48`, `app/core/views_config.py:53`, `app/core/views_config.py:85`, `app/core/views_config.py:137`, `app/core/views_config.py:141`, `app/core/views_config.py:153`, `app/chamados/signals.py:288`.

**Dinheiro/dia: sim, potencial**: o alcance cadastrado alimenta o juiz de feriados e a dobra do motor. Impacto ocorrido em produção `[não verificado]`. `app/core/feriados.py:57`, `app/core/feriados.py:70`, `app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:2809`.

**5. Redefinir senha de outro usuário — CPF/manual/sistema.**

- A: `/colaboradores/<cid>/resetar-senha/`, view `app/colaboradores/views.py:1474`, templates `app/templates/colaboradores/editar.html:230` e `app/templates/colaboradores/gestao_usuarios.html:60` → `core.credenciais.resetar_para_cpf`, `app/core/credenciais.py:47`.
- B: `/colaboradores/<cid>/definir-senha-manual/`, view `app/colaboradores/views.py:1556`, template `app/templates/colaboradores/editar.html:236` → `definir_senha_temporaria`, `app/core/credenciais.py:67`.
- C: `/configuracoes/usuarios-ad/ficha/<uid>/acao/`, view `app/core/views_usuarios.py:324`, `acao=reset_senha`, template `app/templates/core/config/_ficha_usuario.html:25` → `resetar_senha_de_sistema`, `app/core/credenciais.py:185`.

A/B gravam `Colaborador.deve_trocar_senha=True`; C apenas zera `User.last_login`. Todos registram `reset_senha_adm`, pelo mesmo módulo, mas funções diferentes. A exige CPF, B exige seis caracteres, C gera doze. `app/core/credenciais.py:57`, `app/core/credenciais.py:73`, `app/core/credenciais.py:188`, `app/core/credenciais.py:58`, `app/core/credenciais.py:74`, `app/core/credenciais.py:190`, `app/colaboradores/views.py:1481`, `app/colaboradores/views.py:1573`, `app/core/views_usuarios.py:343`.

O endpoint C aceita qualquer `User.is_staff`; para staff que também tenha Colaborador com flag falsa, o leitor de troca obrigatória ignora `last_login=None` e lê a flag. Logo, nesse caso, o resultado pode diferir. `app/core/views_usuarios.py:331`, `app/colaboradores/mixins.py:283`, `app/core/views.py:55`.

**Dinheiro/dia: não diretamente**; escrita de credencial e controle do próximo login. `app/core/credenciais.py:47`, `app/core/credenciais.py:185`.

**6. Bloquear/desbloquear acesso — cadastro/sistema.**

- A: `/colaboradores/<cid>/toggle-acesso/`, `app/colaboradores/views.py:1583`, template `app/templates/colaboradores/gestao_usuarios.html:67` → `alternar_acesso`, `app/core/credenciais.py:84`.
- B: `/configuracoes/usuarios-ad/ficha/<uid>/acao/`, `app/core/views_usuarios.py:324`, `acao=toggle_ativo`, template `app/templates/core/config/_ficha_usuario.html:28` → `alternar_ativo_de_sistema`, `app/core/credenciais.py:196`.

Mesma coluna `User.is_active`, mesmo verbo auditado, funções diferentes. Só B impede auto-desativação. A exige `toggle_acesso`; B exige `gerir_usuarios`. `app/core/credenciais.py:88`, `app/core/credenciais.py:199`, `app/core/credenciais.py:90`, `app/core/credenciais.py:201`, `app/core/views_usuarios.py:348`, `app/colaboradores/views.py:1584`, `app/core/views_usuarios.py:327`, `app/core/views_usuarios.py:16`.

**Dinheiro/dia: não diretamente**; leitores de autenticação recusam usuário inativo. `app/api/views.py:96`, `app/api/views_core.py:153`.

**7. Trocar escala pela tela/copiloto — converge com diferença de modo/trilha.**

- A: `/colaboradores/<cid>/vincular/escala/`, `app/colaboradores/views.py:373`, template `app/templates/colaboradores/partials/painel_vinculo.html:136` → `julgar_vinculo` em `app/colaboradores/views.py:411` → `executar_vinculo` em `app/colaboradores/views.py:423`.
- B: `/ia/perguntar/`, `app/core/views_ia.py:859`, template `app/templates/core/_copiloto.html:180` → `_aplicar_troca`, `app/core/views_ia.py:997` → `trocar_escala_porta`, `app/colaboradores/services/troca_pelo_chat.py:64` → mesmo juiz/escritor em `app/colaboradores/services/troca_pelo_chat.py:72`.

Converge em `app/colaboradores/services/vinculo.py:167`. IA força `modo=direto`; tela aceita também aceite. IA acrescenta `ia_trocou_escala`. Mesmos dados em modo direto não revelaram outro escritor financeiro. `app/colaboradores/services/troca_pelo_chat.py:12`, `app/colaboradores/views.py:425`, `app/core/views_ia.py:1013`.

### Convergências puras para separar no fim

- **Reset para CPF em duas telas**: editar colaborador e gestão de usuários → mesma rota/view `resetar_senha` → `core.credenciais.resetar_para_cpf`. `app/templates/colaboradores/editar.html:230`, `app/templates/colaboradores/gestao_usuarios.html:60`, `app/colaboradores/views.py:1474`, `app/core/credenciais.py:47`.
- **Vincular/desvincular setor**: ficha do usuário `/configuracoes/usuarios-ad/ficha/<uid>/` e ficha do setor `/configuracoes/usuarios-ad/setor/<sid>/` → POST `/configuracoes/usuarios-ad/ficha/<uid>/setor/` → mesma view e escrita `groups.add/remove`, mesma trilha. `app/core/urls.py:66`, `app/core/urls.py:68`, `app/templates/core/config/_ficha_usuario.html:39`, `app/templates/core/config/_ficha_usuario.html:52`, `app/templates/core/config/_ficha_setor.html:50`, `app/templates/core/config/_ficha_setor.html:61`, `app/core/views_usuarios.py:191`, `app/core/views_usuarios.py:204`, `app/core/views_usuarios.py:210`. Leitor: `tem_acao`, `app/colaboradores/mixins.py:322`.
- **Permissões do setor**: ficha do setor e drawer Quadro geral → POST `/configuracoes/setores/quadro/api/` → mesma view, `group.permissions.add/remove` e auditoria antes/depois. Componentes distintos da mesma tela raiz; incluir se “templates distintos” for aplicado literalmente. `app/templates/core/config/_ficha_setor.html:36`, `app/templates/core/config/usuarios_ad.html:168`, `app/templates/core/config/usuarios_ad.html:364`, `app/templates/core/config/usuarios_ad.html:381`, `app/core/urls.py:57`, `app/core/views_quadro.py:13`, `app/core/views_quadro.py:43`, `app/core/views_quadro.py:48`.

### Admin Django: separado do universo explícito

Empresa e Praça continuam registrados em `ModelAdmin` editável, com somente alguns campos bloqueados. Posto e Colaborador usam `SoLeitura`. `app/colaboradores/admin.py:8`, `app/colaboradores/admin.py:19`, `app/colaboradores/admin.py:23`, `app/colaboradores/admin.py:28`, `app/colaboradores/admin.py:33`, `app/colaboradores/admin.py:48`, `app/core/admin.py:59`.

- **Editar Empresa**: tela própria `/colaboradores/empresas/<eid>/editar/`, view `app/colaboradores/views.py:485`, template em `app/colaboradores/views.py:542` → `atualizar_empresa`, `app/colaboradores/services/estrutura.py:17`; outra porta é `EmpresaAdmin`, montada sob `/admin/`, `app/config/urls.py:24`, `app/colaboradores/admin.py:8`. Escritor/view/template framework `[não verificado]`. A própria normaliza corte para `[2,28]` e escreve conjunto explícito de campos; a classe admin só declara `regime_trabalhista` readonly. Diferenças efetivas das validações framework `[não verificado]`. `app/colaboradores/views.py:496`, `app/colaboradores/views.py:521`, `app/colaboradores/admin.py:19`. Leitor do corte: `app/ponto/janelas.py:36`.
- **Editar Praça**: tela própria `/colaboradores/pracas/<pid>/editar/`, view `app/colaboradores/views.py:558`, template em `app/colaboradores/views.py:587` → `atualizar_praca`, `app/colaboradores/services/estrutura.py:35`; outra porta `PracaAdmin`, `app/colaboradores/admin.py:23`. Escritor/view/template framework `[não verificado]`. Própria exige nome/cidade/UF e monta somente nome/cidade/estado/sindicato/obs; framework apenas tem três readonly declarados. Impacto monetário desse cruzamento `[não verificado]`. `app/colaboradores/views.py:572`, `app/colaboradores/views.py:574`, `app/colaboradores/admin.py:28`.

### IA e fronteiras já entregues aos outros exploradores

O dispatcher possui oito intenções que escrevem: validar classe A; corrigir proposta de escala; agendar férias; declarar falta; declarar folga; cobrar colaborador; lançar ausência; trocar escala. São sete handlers, porque falta/folga compartilham handler. Não há escrita de pautas nesse dispatcher. `app/core/views_ia.py:142`, `app/core/views_ia.py:160`, `app/core/views_ia.py:640`, `app/core/views_ia.py:668`, `app/core/views_ia.py:754`, `app/core/views_ia.py:834`, `app/core/views_ia.py:971`, `app/core/views_ia.py:997`.

As intenções, salvo troca já detalhada, foram repassadas aos donos de chamados/férias e ponto/escala para evitar duplicação de grupos. Regularização de colaboradores também ficou com chamados: rota `/colaboradores/colab/<cid>/regularizar/`, view `app/colaboradores/views.py:2948`, template em `app/colaboradores/views.py:2987`, escritor chamado em `app/colaboradores/views.py:2983`; exige categoria válida, `app/colaboradores/views.py:2961`.

Para consolidação numérica: **7 grupos próprios com diferenças**, **3 convergências puras** e **2 candidatos separados do Django admin**; os grupos compartilhados IA/ausência/férias/chamados devem ser contados somente uma vez. As três portas de posto produzem três pares; benefícios avulso/lote/aprovação também produzem três, se a contagem usar pares combinatórios. Demais critérios de expansão combinatória precisam ser explicitados para não confundir grupo com par. `[não verificado]`

## 06/11 21:05 · sessao ca281ecd97ff · - · - · origem=Ronald (tmux)

Universo sob minha responsabilidade: **41 views distintas que aceitam POST de admin e persistem**, excluído o bloco de ausências delegado ao outro explorador; aliases de rota contam uma vez. São 11 de escala, 22 de ponto, 5 de folha e 3 de relatórios. As referências nominais estão na mensagem anterior; os registros abaixo fecham os grupos encontrados. (`app/escala/urls.py:8`, `app/ponto/urls.py:19`, `app/folha/urls.py:8`, `app/relatorios/urls.py:24`)

**Divergências confirmadas**

1. **Aprovar justificativa — singular × lote.**

   - A: `/ponto/justificativa/<jid>/painel/`, `painel_justificativa` → `ponto/partials/painel_justificativa.html`; formulário singular → `/ponto/justificativa/<jid>/aprovar/`, `aprovar_justificativa` → escritor `aprovar_justificativa_dp`. (`app/ponto/views.py:1662`, `app/ponto/views.py:1685`, `app/templates/ponto/partials/painel_justificativa.html:51`, `app/ponto/views.py:1911`, `app/ponto/services/justificativa.py:117`)
   - B: `/ponto/justificativa/fila/`, `fila_justificativas` → `ponto/fila_justificativas.html`; POST `/ponto/justificativa/aprovar-lote/`, `aprovar_lote_justificativas` → **escrita inline** de `status/aprovada_por/aprovada_em`. (`app/ponto/views.py:1695`, `app/ponto/views.py:1749`, `app/templates/ponto/fila_justificativas.html:176`, `app/ponto/views.py:1763`, `app/ponto/views.py:1796`)
   - Diferenças: singular exige `saida_informada` **e entrada vinculada** para materializar, recusa saída ≤ entrada, avisa outras aprovações/turno ≥12h; lote materializa com saída informada mesmo sem testar a entrada e não contém essas validações. (`app/ponto/services/justificativa.py:137`, `app/ponto/services/justificativa.py:144`, `app/ponto/services/justificativa.py:147`, `app/ponto/services/justificativa.py:157`, `app/ponto/views.py:1808`)
   - Singular registra `LogAuditoria`, followup e chama `_encerrar_bloqueio_de_entrada`; lote encerra a origem, mas não chama esses três efeitos. Ambos invalidam fechamento. (`app/ponto/services/justificativa.py:199`, `app/ponto/services/justificativa.py:202`, `app/ponto/services/justificativa.py:203`, `app/ponto/services/justificativa.py:214`, `app/ponto/services/justificativa.py:218`, `app/ponto/views.py:1804`, `app/ponto/views.py:1837`)
   - **Dinheiro/dia: sim, potencial estático**: caminhos podem produzir conjuntos diferentes de batidas; fechamento também transforma justificativas aprovadas em `datas_justificadas`. Ocorrência em produção: **[não verificado]**. (`app/ponto/services/justificativa.py:137`, `app/ponto/views.py:1808`, `app/ponto/services/fechamento.py:660`)

2. **Rejeitar justificativa — singular × lote.**

   - A: mesmo painel, POST `/ponto/justificativa/<jid>/rejeitar/` → `rejeitar_justificativa` → `rejeitar_justificativa_dp`. (`app/templates/ponto/partials/painel_justificativa.html:58`, `app/ponto/views.py:1923`, `app/ponto/services/justificativa.py:247`)
   - B: fila, POST `/ponto/justificativa/rejeitar-lote/` → `rejeitar_lote_justificativas` → **escrita inline**. (`app/templates/ponto/fila_justificativas.html:188`, `app/ponto/views.py:1859`, `app/ponto/views.py:1874`)
   - Singular usa atomic/lock e retorna “já processada”; lote faz get/save sem lock e trata item já decidido como erro. Singular abre chamado `justificativa_rejeitada` e followup; lote só tenta push. (`app/ponto/services/justificativa.py:258`, `app/ponto/services/justificativa.py:262`, `app/ponto/services/justificativa.py:265`, `app/ponto/services/justificativa.py:275`, `app/ponto/services/justificativa.py:309`, `app/ponto/views.py:1874`, `app/ponto/views.py:1880`, `app/ponto/views.py:1898`)
   - Leitores: painel lê chamado de origem/followups e fila lê status; fechamento só inclui as aprovadas em `datas_justificadas`. **Dinheiro/dia: não na execução sequencial normal**, pois ambos gravam rejeitada; efeito concorrente sobre decisão final **[não verificado]**. (`app/ponto/views.py:1683`, `app/ponto/views.py:1709`, `app/ponto/services/fechamento.py:660`)

3. **Aprovar fechamento — grupo de três caminhos, três pares.**

   - A: `/ponto/fechamento/`, template `ponto/fechamento.html` → POST `/ponto/fechamento/aprovar/`, `aprovar_fechamento` → `fechamento.aprovar_lote`. (`app/templates/ponto/fechamento.html:209`, `app/ponto/views.py:1257`, `app/ponto/views.py:1362`, `app/ponto/services/fechamento.py:1316`)
   - B: `/ponto/fechamento/painel/`, `painel_fechamento` → `ponto/partials/painel_fechamento.html` → POST `/ponto/fechamento/aprovar-colaborador/`, `aprovar_colaborador` → `fechamento.aprovar`. (`app/ponto/views.py:966`, `app/ponto/views.py:1006`, `app/templates/ponto/partials/painel_fechamento.html:86`, `app/ponto/views.py:1461`, `app/ponto/views.py:1515`, `app/ponto/services/fechamento.py:1328`)
   - C: `/folha/exportar/` → POST `/folha/exportar/gerar/`, `gerar_dominio_txt` → `fechamento.aprovar_lote`. (`app/folha/urls.py:13`, `app/folha/urls.py:14`, `app/folha/views.py:102`, `app/folha/views.py:252`)
   - A verifica travas sobre **objetos gravados**; B sobre `fechamento_lido`; C sobre recalcular em somente leitura. Portanto a autoridade temporal das validações difere. (`app/ponto/views.py:1290`, `app/ponto/views.py:1296`, `app/ponto/views.py:1480`, `app/ponto/views.py:1506`, `app/folha/views.py:208`, `app/folha/views.py:212`)
   - A grava observação e tranca período quando há empresa e nenhum pulado; B só carimba a linha; C carimba apenas `entra` sem travas selecionadas, preserva observação e deixa log `aprovacao_por_geracao`. (`app/ponto/views.py:1362`, `app/ponto/views.py:1366`, `app/ponto/services/fechamento.py:1322`, `app/ponto/services/fechamento.py:1328`, `app/folha/views.py:243`, `app/folha/views.py:246`, `app/folha/views.py:254`)
   - A/C **convergem em `aprovar_lote`**, porém divergem antes/depois; B usa outro método da mesma casa. Trancar também dispara encerramento de chamados/perguntas. (`app/ponto/services/fechamento.py:1316`, `app/ponto/services/fechamento.py:1328`, `app/ponto/services/fechamento.py:1418`)
   - **Dinheiro/dia: sim, pela elegibilidade de aprovação e pelos efeitos da tranca; os métodos de carimbo não recalculam valores**. O TXT usa o resultado lido para montar suas linhas. (`app/ponto/services/fechamento.py:1322`, `app/ponto/services/fechamento.py:1330`, `app/folha/views.py:268`)

4. **Reabrir fechamento — três caminhos, três pares, mesma família de carimbo.**

   - A: POST `/ponto/fechamento/reabrir-colaborador/`, `reabrir_colaborador` → `fechamento.reabrir`; só aceita aprovado e registra log por linha. (`app/ponto/urls.py:34`, `app/ponto/views.py:1434`, `app/ponto/views.py:1447`, `app/ponto/views.py:1452`, `app/ponto/views.py:1453`, `app/ponto/services/fechamento.py:1342`)
   - B: POST `/ponto/fechamento/reavaliar-vazios/`, `reavaliar_vazios` → `fechamento.reabrir_lote`; seleciona aprovados com trava `conta_vazia` pelo juiz sobre gravado e registra log por lote. (`app/ponto/urls.py:35`, `app/ponto/views.py:1383`, `app/ponto/views.py:1409`, `app/ponto/views.py:1411`, `app/ponto/views.py:1423`, `app/ponto/views.py:1424`, `app/ponto/services/fechamento.py:1337`)
   - C: POST `/ponto/fechamento/reabrir/`, `reabrir_periodo` → `fechamento.reabrir_periodo`; exige permissão própria, motivo e período trancado; altera `PeriodoFechado`, reabre fila de chamados/perguntas e todos aprovados da empresa/competência. (`app/ponto/urls.py:36`, `app/ponto/views.py:1520`, `app/ponto/views.py:1524`, `app/ponto/views.py:1535`, `app/ponto/views.py:1540`, `app/ponto/services/fechamento.py:1425`, `app/ponto/services/fechamento.py:1430`, `app/ponto/services/fechamento.py:1440`, `app/ponto/services/fechamento.py:1442`)
   - A/B/C limpam os mesmos três campos da aprovação por escritores distintos; C também toca `atualizado_em`. **Dinheiro/dia: não diretamente no ato**, que apenas reabre; efeitos posteriores por recalcular **[não verificado]**. (`app/ponto/services/fechamento.py:1339`, `app/ponto/services/fechamento.py:1344`, `app/ponto/services/fechamento.py:1445`)

5. **Autorizar HE — dia × período: mesmo escritor, validação diferente.**

   - A: `ponto/gestao_he.html` → POST `/ponto/gestao-he/decidir/`, `decidir_he` → `ponto.portas.he.decidir_he`. (`app/templates/ponto/gestao_he.html:256`, `app/ponto/views.py:496`, `app/ponto/views.py:529`, `app/ponto/portas/he.py:138`)
   - B: `/ponto/gestao-he/periodo/previa/`, `previa_he_periodo` → `ponto/gestao_he_periodo_previa.html` → POST `/ponto/gestao-he/periodo/autorizar/`, `autorizar_he_periodo` → `confirmar` → `autorizar_em_lote` → **mesmo `decidir_he`**. (`app/ponto/views.py:677`, `app/ponto/views.py:712`, `app/templates/ponto/gestao_he_periodo_previa.html:79`, `app/ponto/views.py:725`, `app/ponto/views.py:751`, `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/portas/he.py:562`)
   - B recusa dia abaixo do limite e decisão existente diferente de autorizado; A pode sobrescrever a decisão e não aplica esse piso. B exige igualdade do DIFF e desfaz tudo se divergir, recorta competência e cria pauta DP quando exportada; A não faz esses passos. (`app/ponto/portas/he.py:175`, `app/ponto/portas/he.py:181`, `app/ponto/portas/he.py:549`, `app/ponto/portas/he.py:557`, `app/ponto/services/autorizacao_he_periodo.py:269`, `app/ponto/services/autorizacao_he_periodo.py:299`, `app/ponto/services/autorizacao_he_periodo.py:343`)
   - **Dinheiro/dia: sim**: escritor dispara `recalcular_por_evento` para o dia; B aceita universo menor e acrescenta pauta em exportada. (`app/ponto/portas/he.py:201`, `app/ponto/services/autorizacao_he_periodo.py:299`)

6. **Criar calendário de folga — editor × miniwizard: escritores diferentes.**

   - A: `/escala/folgas/criar/`, `editar_folga_calendario` → `escala/folga_calendario_editar.html` → `salvar_calendario_de_folga`. (`app/escala/urls.py:35`, `app/escala/views.py:316`, `app/escala/views.py:356`, `app/escala/views.py:369`, `app/escala/folgas.py:548`)
   - B: wizard → POST `/escala/tipos/wizard/calendario/criar/`, `wizard_calendario_criar` → `criar_calendario_de_folga`; responde `escala/_wizard_cal_select.html`. (`app/templates/escala/wizard_tipo_escala.html:160`, `app/escala/urls.py:20`, `app/escala/views_wizard.py:140`, `app/escala/views_wizard.py:171`, `app/escala/views_wizard.py:192`, `app/escala/folgas.py:577`)
   - A recebe dias independentes por tamanho e ignora células inválidas; B recebe uma lista e aplica clamp aos três tamanhos e rejeita célula inválida. Ambos usam veto de nome repetido. (`app/escala/views.py:332`, `app/escala/views.py:340`, `app/escala/views_wizard.py:163`, `app/escala/views_wizard.py:165`, `app/escala/folgas.py:554`, `app/escala/folgas.py:570`, `app/escala/folgas.py:584`, `app/escala/folgas.py:590`)
   - Leitor `datas_que_o_calendario_manda` escolhe o template correspondente ao mês e `gerar_foto_folgas` materializa a escolha. **Dinheiro/dia: sim, quando aplicado**, por produzir folgas em datas diferentes; simples criação não aplica. (`app/escala/folgas.py:6`, `app/escala/folgas.py:43`, `app/escala/folgas.py:150`)

7. **Confirmar cadastro errado — lista × chamado: converge, mas só chamado recebe nota.**

   - A: `/escala/cadastro-x-realidade/regua/<ec_pk>/corroborar/`, `corroborar_regua_da_lista`, template `escala/cadastro_x_realidade.html` → `corroborar_regua(ec, request)`. (`app/escala/urls.py:15`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/views.py:456`, `app/escala/views.py:459`)
   - B: modal do chamado, `acao=corroborar_regua` → `chamados.views` → `corroborar_regua(ec, request, chamado=chamado)`. (`app/templates/chamados/partials/modal_fio.html:561`, `app/chamados/views.py:438`, `app/chamados/views.py:443`)
   - **Converge em `escala/regua_defesa.py:647`**, que chama `gravar_regua`; só B adiciona ocorrência ao chamado. A endereça EC por pk; B procura ativo pelo tipo no contexto do chamado. (`app/escala/views.py:449`, `app/chamados/views.py:441`, `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:670`)
   - **Dinheiro/dia: não diretamente**: os dois gravam os mesmos campos de confiança e ligam veto à cobrança; divergência é o alvo selecionado e a nota. Igualdade dos ECs em produção **[não verificado]**. (`app/escala/regua_defesa.py:641`, `app/escala/regua_defesa.py:666`)

8. **Realocar régua — lista × chamado: converge, insumo e nota diferentes.**

   - A: `/escala/cadastro-x-realidade/regua/<ec_pk>/realocar/`, `realocar_regua_da_lista`, mesmo template da lista → destino de `ultimo_lavrado` → `realocar_vinculo_para_destino`. (`app/escala/urls.py:17`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/views.py:463`, `app/escala/views.py:469`, `app/escala/views.py:471`)
   - B: modal `acao=realocar_regua` → `realocar_para_destino` → destino de `contexto_json` → mesmo núcleo. Só B anexa ocorrência. (`app/templates/chamados/partials/modal_fio.html:570`, `app/chamados/views.py:449`, `app/escala/regua_defesa.py:326`, `app/escala/regua_defesa.py:328`, `app/escala/regua_defesa.py:329`, `app/escala/regua_defesa.py:333`)
   - **Converge em `realocar_vinculo_para_destino` → `julgar_vinculo/executar_vinculo`**; destino cíclico é recusado e início é hoje. **Dinheiro/dia: sim, potencial**, pois o cadastro de escala pode receber destinos diferentes por fonte; coincidência dos destinos reais **[não verificado]**. (`app/escala/regua_defesa.py:291`, `app/escala/regua_defesa.py:310`, `app/escala/regua_defesa.py:312`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:321`)

9. **Emitir espelho auditável — avulso × anexo × lote: três pares.**

   - A: `/relatorios/espelho/avulso/`, `espelho_pdf_avulso`, `relatorios/espelho_pdf_avulso.html` → `gerar_pdf_espelho(... chamado=None)`. (`app/relatorios/urls.py:28`, `app/relatorios/views.py:1050`, `app/relatorios/views.py:1127`, `app/relatorios/views.py:1146`)
   - B: `/relatorios/espelho/chamado/<id>/anexar/`, `anexar_espelho_chamado` → mesmo serviço com `chamado`; tela disparadora específica **[não verificado]**. (`app/relatorios/urls.py:24`, `app/relatorios/views.py:971`, `app/relatorios/views.py:1003`)
   - C: `/relatorios/espelho/lote/`, `espelho_lote`, `relatorios/espelho_lote.html` → `lote_espelho.pedir` → job → `gerar_pdf_espelho_lote_bytes` → mesmo `gerar_pdf_espelho`. (`app/relatorios/views.py:1152`, `app/relatorios/views.py:1170`, `app/relatorios/views.py:1227`, `app/relatorios/lote_espelho.py:205`, `app/relatorios/services.py:237`)
   - Convergência auditável: `pdf_base` persiste `RegistroPDFAuditavel`; B acrescenta followup/PDF no chamado; C grava job e log de pedido, perde IP no registro porque passa `request=None`, e diante de falha do auditável cai para PDF informacional, ao passo que A/B retornam erro para `ValueError`. (`app/relatorios/pdf_base.py:152`, `app/relatorios/services.py:75`, `app/relatorios/services.py:88`, `app/relatorios/views.py:1238`, `app/relatorios/lote_espelho.py:206`, `app/relatorios/services.py:244`, `app/relatorios/services.py:250`, `app/relatorios/views.py:1007`, `app/relatorios/views.py:1131`)
   - **Dinheiro/dia: não** no ato de emissão; muda evidência, anexo, IP e possibilidade de sair informacional. (`app/relatorios/pdf_base.py:152`, `app/relatorios/services.py:88`, `app/relatorios/services.py:250`)

**Folgas: grupo adicional com telas legadas não comprovadas**

- Toggle exposto em `/escala/plano-folgas/toggle/` → `plano_folgas_toggle` → `lancar_folga`/`alterar_folga`; lote atual `/escala/plano-folgas/aplicar/` → `plano_folgas_aplicar` → `aplicar_folgas_em_lote`. O template atual chama o lote; template disparador do toggle **[não verificado]**. Lote recusa dias anteriores à competência corrente; toggle não faz essa recusa. (`app/escala/urls.py:9`, `app/escala/views_plano_folgas.py:98`, `app/escala/views_plano_folgas.py:131`, `app/escala/views_plano_folgas.py:136`, `app/escala/views_plano_folgas.py:144`, `app/templates/escala/plano_folgas.html:171`, `app/escala/folgas.py:385`)
- IA declarar folga passa por `declarar_dia._escrever_folga` → mesmo `lancar_folga`, mas **fecha explicitamente o fio do dia** dentro do mesmo atomic; Plano não chama esse fechamento explícito. Lote usa `_foto_do_dia`. Os três relavram células. (`app/ponto/services/declarar_dia.py:179`, `app/ponto/services/declarar_dia.py:189`, `app/ponto/services/declarar_dia.py:203`, `app/escala/folgas.py:389`, `app/escala/folgas.py:395`, `app/escala/folgas.py:314`)
- Vincular calendário singular `/colaboradores/<cid>/vincular/folga/` → `vincular_folga` → `trocar_calendario_de_folga` + `HistoricoVinculo` + fotos; aplicar calendário por tipo `/escala/folgas/<cid>/aplicar/` → `aplicar_folga_calendario` → mesmo escritor + fotos, sem histórico e com relavra adicional explícita. Template disparador da segunda rota **[não verificado]**. (`app/colaboradores/urls.py:17`, `app/templates/colaboradores/partials/painel_vinculo.html:191`, `app/colaboradores/views.py:239`, `app/colaboradores/views.py:241`, `app/colaboradores/views.py:245`, `app/escala/urls.py:37`, `app/escala/views.py:409`, `app/escala/views.py:411`, `app/escala/views.py:416`)
- O escritor comum do calendário registra `LogAuditoria` em ambos; leitor do histórico é painel de vínculo. (`app/colaboradores/services/vinculo.py:433`, `app/colaboradores/services/vinculo.py:448`, `app/colaboradores/views.py:306`)

**Convergências a separar no fim**

- **Recusar HE singular/lote:** ambas chegam a `ponto.portas.he.decidir_he`; lote chama explicitamente esse escritor por item. (`app/ponto/views.py:529`, `app/ponto/views.py:614`, `app/ponto/portas/he.py:237`)
- **Flip manual fila/calendário:** `ponto/flip_fila.html` e `_calendario_grade.html` POSTam no mesmo endpoint/view/escritor `flip_tipo`, que anota batida e recomputa turno. (`app/templates/ponto/flip_fila.html:16`, `app/templates/colaboradores/partials/_calendario_grade.html:121`, `app/ponto/views.py:3260`, `app/ponto/services/flip_batida.py:10`, `app/ponto/services/flip_batida.py:30`, `app/ponto/services/flip_batida.py:39`)
- **Resolver dia individual/lote/calendário/drawer:** os templates convergem em `veredito_celula_view` → `veredito_celula`; declaração de falta pela IA também chama essa autoridade. (`app/templates/colaboradores/partials/_calendario_grade.html:126`, `app/templates/colaboradores/calendario_partial.html:63`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:36`, `app/ponto/views.py:3340`, `app/ponto/services/declarar_dia.py:211`)
- **Retratar/desfazer marcação nos calendários que reutilizam a grade:** endpoint/escritor comum por ação, `retratar_humano.retratar` e `.desfazer`. (`app/templates/colaboradores/partials/_calendario_grade.html:121`, `app/ponto/views.py:3274`, `app/ponto/views.py:3286`, `app/ponto/services/retratar_humano.py:150`, `app/ponto/services/retratar_humano.py:190`)

**Django admin, separado das 41 views explícitas**

- `TipoEscalaAdmin` e `JustificativaAdmin` continuam `ModelAdmin` editáveis; `EscalaColaborador`, Batida, ImportacaoAEJ e Ausencia usam `SoLeitura`. Escritor e template internos do framework: **[não verificado]**. (`app/escala/admin.py:7`, `app/escala/admin.py:12`, `app/ponto/admin.py:11`, `app/ponto/admin.py:18`, `app/ponto/admin.py:25`, `app/ponto/admin.py:37`)
- Diferença material a registrar como fronteira: wizard usa `salvar_tipo`, que veta template não editável e propaga regeneração/invalidação; admin não declara override para esse serviço. `TipoEscala.save` executa `full_clean`, mas a propagação está na porta. (`app/escala/views_wizard.py:108`, `app/escala/services/cadastro_tipo.py:140`, `app/escala/services/cadastro_tipo.py:144`, `app/escala/services/cadastro_tipo.py:320`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:357`, `app/escala/models.py:798`, `app/escala/admin.py:8`)
- Alterar status de justificativa pelo admin × aprovar/rejeitar pelas telas é uma exposição de segunda entrada; execução específica do admin e efeitos de formulário são **[não verificado]**, porque o framework não foi lido. A aprovação singular e o lote acima são provados integralmente no código da c

[truncado em 20000 de 20101 caracteres]

## 06/11 21:05 · sessao 4ba6741b8ad8 · - · - · origem=Ronald (tmux)

Achados para perguntas 4–5, todos da leitura local:

**Leitores efetivos e leitores que não consomem o subset**

| Consumidor | Comportamento comprovado |
|---|---|
| TXT/Domínio | `eventos_do_fechamento` **não lê** `_100_noturna`: emite HE100 normal por `total100 − feriado100`, feriado100 e adicional noturno total. Comentário declara subset sem linha própria, para evitar duplicidade. `app/folha/export.py:420`, `app/folha/export.py:424`, `app/folha/export.py:439`. |
| Espelho diário | Carrega `DiaPago` e usa `rubricas_do_dia`; helper mostra total100 e total noturno, sem expor subset100 noturno. `app/ponto/services/espelho.py:406`, `app/ponto/services/espelho.py:813`, `app/ponto/services/dia_pago.py:912`, `app/ponto/services/dia_pago.py:922`. |
| PDF | Usa `dia['rubricas']` para desenhar rubricas; badge usa total100 e noturnas. Não recalcula subset100 noturno. `app/relatorios/pdf_espelho.py:910`, `app/relatorios/pdf_espelho.py:771`, `app/relatorios/pdf_espelho.py:782`. |
| `pago_do_dia` | **Lê o subset indiretamente** no `any` de rubricas de dinheiro, apenas para decidir se existe valor sustentado; retorno soma trabalhadas + folga + folga-feriado, sem adicionar subset. `app/ponto/services/dia_pago.py:859`, `app/ponto/services/dia_pago.py:890`, `app/ponto/services/dia_pago.py:895`. |
| Soma da competência | `CAMPOS` inclui subset; agrega `Sum(k)`, arredonda soma mensal. `app/ponto/services/dia_pago.py:34`, `app/ponto/services/dia_pago.py:44`, `app/ponto/services/dia_pago.py:560`. |
| Soma do período | Agrega todos `CAMPOS_DIA`, incluindo subset, por colaborador/data. `app/ponto/services/dia_pago.py:782`, `app/ponto/services/dia_pago.py:785`. |
| Porta/oráculo de identidade DiaPago × fechamento | Compara cada campo da soma com `getattr(fech, campo)`. Logo valores errados iguais dos dois lados passam por essa identidade; a comparação não estabelece a lei da rubrica. `app/folha/porta_export.py:388`, `app/folha/porta_export.py:451`. |
| Placar R4 | Declara identidade fechamento × soma DiaPago, remete ao contador `porta_export.medir`; não calcula subset próprio. `app/core/placar_estrutural.py:199`, `app/core/placar_estrutural.py:204`, `app/core/placar_estrutural.py:207`. |
| Placar de derivação de leitores | Nome `_100_noturna` aparece como **vocabulário AST**, não leitura de horas: lista `DINHEIRO` é usada pelo detector de aritmética sobre períodos. `app/core/medida_leitor_do_dia.py:47`, `app/core/medida_leitor_do_dia.py:51`, `app/core/medida_leitor_do_dia.py:70`. |
| Ranking HE | `CAMPOS` declara subset, mas ranking e apuração individual entregam só HE50/HE100/HE100feriado. Declaração não é leitura efetiva do subset. `app/folha/services/ranking_he.py:33`, `app/folha/services/ranking_he.py:139`, `app/folha/services/ranking_he.py:224`. |
| Simulação da folha | Inclui subset no inventário de `CAMPOS_HORAS` das fotografias. `app/folha/management/commands/simular_folha.py:59`. |
| Sonda `desvio_o68b` | Lê `minutos_extra_100_noturna`, imprime `/60`, agrupa visualmente pela data local da entrada; não pelo mapa de jornada. `app/ponto/management/commands/desvio_o68b.py:117`, `app/ponto/management/commands/desvio_o68b.py:120`, `app/ponto/management/commands/desvio_o68b.py:127`. |
| Fotos/recalcular e ferramentas de diff | Censo canônico inclui o campo; foto lê cada nome por `getattr`. `app/ponto/management/commands/aplicar_09_corte_b.py:67`, `app/ponto/management/commands/aplicar_09_corte_b.py:70`, `app/ponto/management/commands/recalcular_fechamento.py:54`, `app/ponto/management/commands/recalcular_fechamento.py:64`, `app/ponto/management/commands/recalcular_fechamento.py:67`. |

**Escritores e semântica**

- Valor inicial do período é zero; agregador `ResultadoMes.horas_extra_100_noturna` soma **somente `resultado.periodos`**, divide minutos por 60. O campo é declarado subset de HE100. `app/ponto/motor_calculo_v2.py:225`, `app/ponto/motor_calculo_v2.py:342`.
- Escrita positiva do motor: `MotorBase._aplicar_he_noturna` chama `he_noturna` com os extras do próprio período e seus intervalos; grava `not_100` se positivo. `app/ponto/motor_calculo_v2.py:1014`, `app/ponto/motor_calculo_v2.py:1020`.
- Unidade do split é **minuto de relógio noturno**, sem fator: interseção por `minutos_noturnos`, dedução dos intervalos, proporcionalização; retorno não multiplica fator. `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:448`, `app/ponto/motor_calculo_v2.py:451`. Horas persistidas são `/60`, não horas reduzidas. `app/ponto/motor_calculo_v2.py:344`, `app/ponto/services/dia_pago.py:184`.
- Zeros posteriores: TurnoPartido e Comercial limpam os extras e subset antes de recalcular por dia. `app/ponto/motor_calculo_v2.py:2933`, `app/ponto/motor_calculo_v2.py:2938`, `app/ponto/motor_calculo_v2.py:3318`, `app/ponto/motor_calculo_v2.py:3324`.
- Bloqueio sem previsto zera todo o conjunto de minutos, incluindo subset100 noturno, por laço genérico. `app/ponto/motor_calculo_v2.py:1473`, `app/ponto/motor_calculo_v2.py:1476`.
- **MotorPlantao não tem escritor próprio da rubrica:** `calcular_periodo` delega a `Motor12x36.calcular_periodo`. `app/ponto/motor_calculo_v2.py:3510`.
- Fechamento mensal copia a property e arredonda para duas casas; persiste por `save()` no aberto e `update()` no outro ramo. `app/ponto/services/fechamento.py:924`, `app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1162`.
- `recalcular_fechamento` é **chamador**, não regra/escritor alternativo: chama `recalcular_fechamento_mes` em `--apply`. `app/ponto/management/commands/recalcular_fechamento.py:110`, `app/ponto/management/commands/recalcular_fechamento.py:118`.
- `DiaPago.lavrar` acumula minutos do período `/60`, escolhe dia pelo mapa `dia_da_jornada` (fallback data local da entrada), instancia e persiste por delete/bulk_create. `app/ponto/services/dia_pago.py:128`, `app/ponto/services/dia_pago.py:132`, `app/ponto/services/dia_pago.py:174`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:343`, `app/ponto/services/dia_pago.py:357`.
- **Oráculo/calculador** também chama a mesma `he_noturna`, alimentada por `he_pontas` de cada período, soma e grava `/60`; não deriva esse subset diretamente da HE100 diária recém-calculada. `app/ponto/calculador/regras.py:824`, `app/ponto/calculador/regras.py:827`, `app/ponto/calculador/regras.py:832`, `app/ponto/calculador/regras.py:839`.
- No fechamento, a versão motor é lavrada sem `rubricas=`; versão oráculo, desde competência 10/2026, usa o mesmo escritor `lavrar` com `rubricas=_rub_o`, `versao='oraculo'`. `app/ponto/services/fechamento.py:1194`, `app/ponto/services/fechamento.py:1258`, `app/ponto/services/fechamento.py:1270`, `app/ponto/services/fechamento.py:1281`, `app/ponto/services/fechamento.py:1287`. O override escreve valores recebidos, salvo `do_motor`. `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:327`.
- `retratar_exportada` pode **criar** linha da grade sem anterior com todas rubricas monetárias em zero, incluindo subset; em linha existente só atualiza três minutos da grade. Cria ajuste também com rubricas zero. `app/ponto/services/dia_pago.py:469`, `app/ponto/services/dia_pago.py:476`, `app/ponto/services/dia_pago.py:479`, `app/ponto/services/dia_pago.py:508`, `app/ponto/services/dia_pago.py:513`. Comando `lavrar_dias_pagos` roteia exportada para esse retrato; aberta para recalcular em leitura com lavratura explicitamente habilitada. `app/ponto/management/commands/lavrar_dias_pagos.py:104`, `app/ponto/management/commands/lavrar_dias_pagos.py:123`.
- Porta genérica `restaurar_fechamento` escreve qualquer campo válido recebido, incluindo este; não recalcula nem verifica unidade semântica. `app/ponto/services/fechamento.py:1351`, `app/ponto/services/fechamento.py:1366`, `app/ponto/services/fechamento.py:1373`, `app/ponto/services/fechamento.py:1385`. `aplicar_09_he_intra` a chama com `gravado + delta`; seu alvo inclui subset. `app/ponto/management/commands/aplicar_09_he_intra.py:40`, `app/ponto/management/commands/aplicar_09_he_intra.py:146`, `app/ponto/management/commands/aplicar_09_he_intra.py:149`.

**Atenção ao caminho FT-feriado encontrado pelo pai:** no fechamento a promoção de FT-feriado soma horas em total100 e feriado100, **não atualiza subset100 noturno**, que já foi copiado só de `resultado.periodos`. `app/ponto/services/fechamento.py:924`, `app/ponto/services/fechamento.py:934`, `app/ponto/services/fechamento.py:955`. A versão DiaPago tem a mesma assimetria: promoção de FT-feriado mantém só noturnas/intra da folga e soma HE100/feriado100. `app/ponto/services/dia_pago.py:223`, `app/ponto/services/dia_pago.py:244`.

**Dia do turno × civil:** caminho canônico DiaPago usa mapa da jornada e fallback de entrada; modelo documenta entrada do turno. TurnoPartido e Comercial agrupam pelo juiz de turno. Não há base para afirmar que todos os instrumentos usam isso: `desvio_o68b` usa entrada civil, e `somar_periodos` também filtra `localtime(p.entrada).date()`. `app/ponto/services/dia_pago.py:128`, `app/ponto/models.py:320`, `app/ponto/motor_calculo_v2.py:2942`, `app/ponto/motor_calculo_v2.py:3313`, `app/ponto/management/commands/desvio_o68b.py:120`, `app/ponto/services/espelho.py:84`.

## 06/11 21:05 · sessao c4f36b40795d · - · - · origem=Ronald (tmux)

Contraprova dos ramos, somente pela cópia:

**Q2 — Há três caminhos diferentes para perder o subset noturno.**

| Caminho | Produção da HE100 | Por que o subset fica zero |
|---|---|---|
| `MotorTurnoPartido.calcular_mes` | Atribui todo `trab_dia` a `minutos_extra_100` no feriado. `app/ponto/motor_calculo_v2.py:3092` | Zera previamente `minutos_extra_100_noturna` em `:2938`; o `continue` em `:3101` pula `_aplicar_he_noturna` em `:3147`. |
| `MotorComercial.calcular_mes` | Atribui todo `trab_dia` a `minutos_extra_100`. `app/ponto/motor_calculo_v2.py:3337` | Zera previamente o subset em `:3324`; o `continue` em `:3352` pula `_aplicar_he_noturna` em `:3399`. O cálculo individual havia chamado o split em `:3281`, mas o mensal apaga seu resultado. |
| Reclassificação de `periodos_ft` para `ft_feriado` | O fechamento acrescenta `minutos_trabalhados / 60` a HE100 e HE100-feriado. `app/ponto/services/fechamento.py:954` | O subset mensal já foi copiado em `:924` e não é acrescido nesse bloco; a property de origem soma somente `resultado.periodos`, excluindo `periodos_ft`. `app/ponto/motor_calculo_v2.py:344`. Na lavratura diária, `ft_feriado` acrescenta HE100 e HE100-feriado, mas `_rubricas_da_folga` transporta somente noturno e intra. `app/ponto/services/dia_pago.py:244`, `:249`, `:223`. |

**O ramo `Motor12x36` não pula o split:** produz a dobra em `app/ponto/motor_calculo_v2.py:2809` e chama `_aplicar_he_noturna` depois dos ramos em `:2855`. O wrapper entrega `periodo.minutos_extra_100` como `extra_100_min` e grava o retorno em `minutos_extra_100_noturna`. `app/ponto/motor_calculo_v2.py:1014`, `:1016`, `:1020`.

**`MotorPlantao` também não constitui um escritor independente nesse ponto:** delega `calcular_periodo` diretamente a `Motor12x36.calcular_periodo`. `app/ponto/motor_calculo_v2.py:3510`.

Atribuir qualquer desses três caminhos aos dez colaboradores depende de cadastro, batidas e partição dos períodos, ausentes no dado fornecido: **[não verificado]**.

**Q7 — Alcance e contraexemplos.**

- **Outros ciclos:** os dois `continue` dependem de trabalho positivo e `_feriado_dobra_no_dia`, não de ser vigilante ou 12x36. Esse predicado exige `feriado_em_dobra` e feriado na data informada. `app/ponto/motor_calculo_v2.py:3092`, `:3337`, `:934`.
- **Folga trabalhada comum:** é separada para `resultado.periodos_ft`; portanto não passa pelos laços diários que percorrem somente `resultado.periodos`. Ela recebe a rubrica própria e adicional noturno, excluindo HE e seus subsets conforme a regra registrada. `app/ponto/motor_calculo_v2.py:2468`, `:2492`, `:3083`, `:3316`, `:307`; `app/ponto/services/dia_pago.py:234`.
- **Folga provocada pelo feriado:** segue o terceiro caminho da tabela. `feriado_vira_folga` exige `trabalha_em_feriado=False`, ciclo permitido e feriado no lugar do vínculo; os ciclos permitidos são `6x1`, `5x2` e `semanal`. Logo esse caminho específico não abrange ciclo 12x36. `app/escala/models.py:32`, `:1390`, `:1392`, `:1395`.
- **DSR/domingo trabalhado:** quando reconhecido como folga, segue `periodos_ft`; domingo previsto não é classificado como folga pelo predicado semanal. Portanto não se pode afirmar que todo DSR trabalhado passa pelo mesmo `continue` de feriado. `app/ponto/motor_calculo_v2.py:2453`, `:2492`, `:950`.
- **Turno iniciado no feriado e terminado no dia seguinte:** o contrato local atribui as horas ao dia de início; Comercial e Partido agrupam pelo dia do turno. A perda pelos `continue` depende de qual motor foi selecionado; `MotorPlantao` reutiliza o caminho que chama o split. `app/ponto/motor_calculo_v2.py:928`, `:2922`, `:2942`, `:3312`, `:3511`.
- **Feriado com intervalo noturno:** o intervalo não evita os dois `continue`, pois essas condições não o consultam. Quando o split é efetivamente chamado, desconta o intervalo da janela noturna; no caminho `ft_feriado`, esse split não é chamado pela lavratura. `app/ponto/motor_calculo_v2.py:3092`, `:3337`, `:440`, `:442`; `app/ponto/services/dia_pago.py:244`.

Há ainda um zerador geral por falta de autorização em dia sem previsto, mas ele zera também trabalhado e noturno; isoladamente não explica o fato fornecido de HE100 e noturno positivos. `app/ponto/motor_calculo_v2.py:1453`, `:1456`, `:1473`, `:1476`.

## 06/11 21:05 · sessao b3333e543a6f · - · - · origem=Ronald (tmux)

Censo da minha faixa: **47 views POST de admin com escrita**; 22 em chamados, 14 em férias e 11 em ausências/ponto. IA/core e colaboradores pertencem ao universo dos outros explorers; os cruzamentos abaixo devem ser deduplicados. As rotas estão em `app/chamados/urls.py:8`, `app/ferias/urls.py:6`, `app/ponto/urls.py:53`.

**Universo nominal**

- Chamados, 22: `abrir_chamado` (`app/chamados/views.py:147`), `adicionar_followup` (:274), `detalhe_chamado_local` (:359), `fio_colaborador` (:659), `modal_fio_colaborador` (:869), `abrir_chamado_rapido` (:961), `atribuir_chamado` (:1008), `conferir_data_pergunta` (:1292), `reabrir_disputa_supervisao` (:1372); cobrar/validar: `app/chamados/views_cobrar.py:24`, :61, :145, :232, :306, :372; arquivar/desarquivar/lote: `app/chamados/views_arquivar.py:24`, :52, :93; regularização criar/add/del/enviar: `app/chamados/views_regularizacao.py:42`, :74, :137, :154.
- Férias, 14: criar período, agendar, cancelar DP, cancelar admin, corrigir concluída, editar datas admin, cancelar massa, agendar lote, editar agendamento, avaliar solicitação, editar período, excluir período, enviar confirmação, vender: `app/ferias/views.py:123`, :269, :321, :341, :378, :421, :519, :569, :633, :711, :777, :808, :830, :906.
- Ausências, 11: anexar, aprovar lote, rejeitar lote, cancelar lote, lançar, reverter rejeição, aprovar, editar período, rejeitar, cancelar, editar: `app/ponto/views.py:2133`, :2270, :2304, :2333, :2368, :2569, :2599, :2662, :2680, :2720, :2751.

**Pares/grupos com diferença concreta**

| Ação; quantidade local de pares | Telas → escritores | Diferença e leitor; gravidade |
|---|---|---|
| Aprovar ausência; **1 par** | A: `/ponto/ausencias/<aid>/aprovar/`, `app/ponto/views.py:2599`, formulário `app/templates/ponto/partials/painel_ausencia.html:309`. B: `/ponto/ausencias/lote/aprovar/`, view :2270, JS `app/templates/ponto/ausencias.html:308`. **Convergem** `decidir`, `app/ponto/services/ausencia.py:897` → `mudar_estado`:808. | A propaga todos os irmãos do documento (:2615), permite confirmar desconto (:2608) e declarar `hora_ini/hora_fim` (:2629), atomic sobre irmãos (:2641), notifica (:2646). B opera somente IDs (:2280), recusa confirmação por construção (:2284), não recebe período e não notifica (:2290). **Sim**, cobertura parcial e quantidade de dias aprovados podem diferir; fechamento lê cobertura em `app/ponto/services/fechamento.py:680`, export lê ausência aprovada em `app/folha/export.py:147`. **Não alegar timestamp ausente:** `Ausencia.save` auto-carimba ambos, `app/ponto/models.py:868`. |
| Rejeitar ausência; **1 par** | A: `/ponto/ausencias/<aid>/rejeitar/`, `app/ponto/views.py:2680`, template painel :330. B: `/ponto/ausencias/lote/rejeitar/`, view :2304, template lista :321. Mesmo `decidir`:897. | A rejeita irmãos (:2688), notifica (:2704), não recebe motivo; B só IDs (:2314), acrescenta motivo em `descricao` (:2319), não notifica. **Sim**, irmãos remanescentes aprovados continuam explicando outros dias; leitores fechamento :680 e export :147. |
| Lançar ausência aprovada; **1 par cruzado IA** | A: `/ponto/ausencias/lancar/`, `app/ponto/views.py:2368`, `ponto/lancar_ausencia.html` (:2424) → `criar_ausencia`, `app/ponto/services/ausencia.py:130` (:2415). B: IA `/ia/perguntar/` via `app/core/views_ia.py:971` → `lancar_ausencia_porta`, `app/ponto/services/checar_ausencia.py:43` → mesmo `criar_ausencia`:46. | A também grava `Colaborador.situacao='afastado'` para INSS, `app/ponto/views.py:2433`, e calcula aviso médico :2444; B não realiza esses efeitos no wrapper, `app/ponto/services/checar_ausencia.py:43`. **Dia: sim**, ambas criam ausência; **diferença monetária direta desse efeito lateral: não demonstrada**. Leitores de situação diferentes podem divergir; `app/colaboradores/services/situacao_exibida.py:37`. IA tem log extra (`app/core/views_ia.py:991`). |
| Validar pergunta; **grupo de 7 endpoints** | Detalhe `/chamados/local/<id>/`, view `app/chamados/views.py:359` → `acao_validar_pergunta`, `app/chamados/services/acoes_disputa.py:13` → `validar_pergunta`, `app/chamados/services/validacao.py:343`; fio `/chamados/colaborador/<id>/` view :659 e modal `/chamados/modal/<id>/` view :869 → `processar_post_fio`, `app/chamados/services/fio.py:481`; inline `/chamados/validar-inline/<id>/`, `app/chamados/views_cobrar.py:145`; lote `/chamados/validar-lote/` :232; lote tela `/chamados/validar-lote/tela/` :372; IA `app/core/views_ia.py:160`. | **Mesmo escritor final**, carimbo em `app/chamados/services/validacao.py:127`. Detalhe exige disputa aberta (`app/chamados/views.py:493`); fio/modal aceitam pergunta viva em disputa fechada, com `sem_reabrir=True` e log adicional (`app/chamados/services/fio.py:591`–:613). Inline só resolve disputa aberta (`app/chamados/views_cobrar.py:160`); lotes restringem classe A (`app/chamados/services/validacao.py:688`), manual pode confirmar implausível (:374). Lote tela registra log agregado extra (`app/chamados/views_cobrar.py:383`); IA também (`app/core/views_ia.py:171`). Templates: `app/templates/chamados/partials/_pergunta_card.html:70`, `app/templates/chamados/partials/modal_fio.html:258`, `app/templates/chamados/partials/_lista_chamados.html:82`, `app/templates/chamados/painel_gestao.html:14`. **Sim**, validação materializa batida e dispara fechamento (`app/chamados/services/validacao.py:395`, :406; `app/chamados/services/materializacao.py:981`). |
| Responder chamado / transicionar para resolvido ou fechado; **2 pares divergentes**, detalhe×fio e detalhe×modal; fio×modal converge | Detalhe `/chamados/local/<id>/` → `acao_resposta_livre`, `app/chamados/services/acoes_chamado.py:283`. Fio/modal → `processar_post_fio`, `app/chamados/services/fio.py:704`. Templates `app/templates/chamados/detalhe_local.html:387`, `app/templates/chamados/fio_colaborador.html:257`, `app/templates/chamados/partials/modal_fio.html:649`. | Detalhe anexa nota tipo `resposta_livre` (:292), logs explícitos `responder`/:295 e `status`/:318; fio/modal anexam `mensagem_dp` (:714), exigem texto para entrar no bloco (:705), sem os logs explícitos desse bloco. Todos chamam `chamado.transicionar` (:315 versus :717). **Dinheiro/dia direto: não demonstrado**; leitores do fio distinguem nota e trilha, e estados alimentam a fila (`app/chamados/services/fio.py:174`, `app/chamados/services/painel.py` [linha não verificada]). |
| Abrir mensagem DP; **1 par** | Form `/chamados/abrir/`, view `app/chamados/views.py:147` → `_abrir_para_colaborador`:77 → `abrir_chamado_para_colaborador`, `app/chamados/services/abertura.py:122`. Template `chamados/form_abertura.html` (:131). Rápida `/chamados/colaborador/<cid>/mensagem-rapida/`, view :961 → `ChamadoColaborador.abrir` :973 + nota :986; template `app/templates/colaboradores/partials/painel_colaborador.html:188`. | Form deduplica entrada+autor+prazo (:165), escreve descrição/contexto/prazo (:172), LogAuditoria (:194). Rápida reaproveita qualquer `mensagem_dp` vivo da pessoa (:973), assunto fixo, texto só na nota (:986), sem LogAuditoria explícito; só push web (:988). **Não muda dia/dinheiro diretamente**; altera chamado e conversa que fio lê (`app/chamados/services/fio.py:174`). |
| Cobrar colaborador; **grupo de 3 caminhos, 3 pares** | Individual `/chamados/cobrar/<cid>/`, `app/chamados/views_cobrar.py:24`; massa `/chamados/cobrar/massa/`, :61; IA → `app/core/views_ia.py:834` → `cobrar_colaborador`, `app/chamados/services/cobranca.py:140`. Templates individual `app/templates/chamados/partials/_lista_chamados.html:84`, modal :345; massa em painel [linha não verificada]. | Individual não restringe VIVOS/folha antes de enviar (:29); massa restringe VIVOS/verbo cobrar e `muda_a_folha` (:76–:96). Ambos throttle por nota `[COBRANCA]` no dia e só anotam depois do push entregue (:35–:56; :117–:133). IA exige perguntas pendentes/prazo não vigente (:155), grava prazo SLA e nota **antes** do toque (:176), LogAuditoria :188. **Dinheiro/dia direto: não**, mas o SLA que leva à supervisão difere (:170–:185); leitura operacional `checar_cobranca`:155. |
| Arquivar chamado; **1 par**, mesmo escritor com diferentes efeitos | Individual `/chamados/arquivar/<id>/`, `app/chamados/views_arquivar.py:24`; lote `/chamados/arquivar/lote/`, :93 → ambos `arquivo.silenciar`, `app/chamados/services/arquivo.py:141`. | Individual aceita objeto sem filtro de idade/status (:30), anexa ocorrência (:44); lote exige corte idade, VIVOS, verbo permitido e sem silêncio (:105, :121, :125, :135), não anexa ocorrência (:140–:147). Ambos têm trilha via escritor. **Não muda dinheiro/dia**, escritor só altera véu, sem status/apuração (`app/chamados/services/arquivo.py:10`–:17). |
| Agendar férias; **grupo 4 caminhos: manual, lote, solicitação, IA** | Manual `/ferias/periodo/<id>/agendar/`, `app/ferias/views.py:269`, `ferias/agendar.html`:312; lote `/ferias/agendar-lote/`, :569, template :626 → `executar_agendamento`, `app/ferias/services.py:306`; IA `app/core/views_ia.py:668` → `agendar_ferias_porta`, services :128 → mesmo executar :157. Solicitações `/ferias/solicitacoes/<sid>/avaliar/`, view :711, template `app/templates/ferias/solicitacoes.html:26` → **outro escritor** `aprovar_solicitacao`, services :410. | Manual/lote usam `_validar_agendamento`, views :226; ambos executam mesma unidade atômica services :306. Solicitação é marcada aprovada :416 **antes** de buscar PA/criar agendamento; escolhe PA adquirido :424, usa `full_clean` :443, captura falha :448 e pode retornar aprovação sem agendamento. IA compõe gozo+abono em atomic :156 e log extra core :693. **Sim**, Agendamento.save espelha Ausencia (`app/ferias/models.py:434`), que folha lê (`app/folha/export.py:147`). Manual/lote convergem; três pares contra solicitação têm escritor distinto. |
| Cancelar férias; **grupo 4 caminhos: DP, admin, admin massa, concluída** | DP `/ferias/agendamento/<id>/cancelar/`, `app/ferias/views.py:321` → `cancelar_agendamento_dp`, `app/ferias/services.py:330`. Admin `/cancelar-admin/`, view :341; massa `/ferias/cancelar-admin-massa/`, :519; concluída `/corrigir-concluida/`, :378 → `executar_cancelamento_admin`, services :341. Templates `app/templates/ferias/saldo.html:71`, `app/templates/ferias/_secao_colab.html:160`, :162, :166; massa template view :565. | DP só barra `concluido` (:325), não barra já cancelado; escritor decrementa saldo toda vez (services :333). Admin filtra `pode_cancelar_admin` (:360), usa atomic, rejeita ausências, obs com autor/data/lote (services :352–:362). Admin individual LogAuditoria (:368); lote não registra LogAuditoria de agendamento (:551–:564); concluída exige justificativa (:399) e log (:410). **Sim**, saldo pode decrementar repetidamente no DP; `_sync_ausencia` existe para ambos (`app/ferias/models.py:434`), portanto **não afirmar ausência órfã no DP**. Leitor saldo `app/ferias/models.py:95`; folha export :147. |
| Editar datas férias; **1 par**, convergente com portas/trilha diferentes | Normal `/ferias/agendamento/<id>/editar/`, `app/ferias/views.py:633` → `remarcar_agendamento`, services :403. Admin `/editar-datas-admin/`, view :421 → `corrigir_datas_agendamento`, services :398. **Convergem `_trocar_janela`, services :366.** Templates `app/templates/ferias/_secao_colab.html:130`, :146. | Normal exige agendado (:649); admin exclui só cancelado (:452), exige justificativa (:454), dá avisos e log antes/depois (:497). Mesmo escritor limpa janela antiga, muda datas/saldo/status, espelha ausências (:378–:394). **Sim**, datas/dias alteram saldo e cobertura; diferença de autorização é explícita, não necessariamente defeito. |
| Vender dias férias; **1 par cruzado IA**, dois escritores | `/ferias/periodo/<id>/vender/`, `app/ferias/views.py:906` → `vender_dias_do_periodo`, `app/ferias/services.py:507`; template `app/templates/ferias/_secao_colab.html:84`. IA agendar com abono → `agendar_ferias_porta`, services :128. | Tela escreve `dias_vendidos` em :509; IA escreve mesmo campo diretamente em :163. Ambos usam limite `dias_vendaveis`; tela valida saldo individual (view :918–:921); IA valida soma gozo+venda (services :194–:201) e atomic conjunto (:156). **Sim**, saldo/dias vendidos; leitor `app/ferias/models.py:95`. |
| Criar regularização externa; **1 par cruzado colaboradores**, convergente com validações distintas | `/chamados/regularizacao/colab/<id>/criar/`, `app/chamados/views_regularizacao.py:42` → `abrir_lote`, `app/chamados/services/regularizacao_ext.py:309` → `criar_regularizacao_externa`:230. Template painel :34. Colaboradores `colab_regularizar`, `app/colaboradores/views.py:2948` → mesmo criar :2983, template :2987; rota exata com explorer core. | Chamados exige gestor + departamento supervisão (`views_regularizacao.py:19`), motivo obrigatório :48, cria vazio; colaboradores usa `regularizar_batida` :2957, categoria obrigatória :2961 e itens :2981, não usa gate contextual nessa view. Criador grava categoria recebida em services :238; painel deixa vazia por default :309/:318. Itens do painel passam checagem zero jornada :120 e `adicionar_item`:322; colaboradores cria itens diretamente no laço da porta :240. **Sim posteriormente**, confirmação materializa batidas (`regularizacao_ext.py:36`, :347); criação só prepara lote. Leitor métrica categoria :277. |

**Convergentes para separar no fim**

1. **Cancelar ausência:** individual `/ponto/ausencias/<id>/cancelar/`, view `app/ponto/views.py:2720`, versus lote `/ponto/ausencias/lote/cancelar/`, :2333. Mesmo `decidir` :2734/:2351, mesma descrição/carimbos, ambos log antes/depois :2739/:2357. Lista dispara lote em `app/templates/ponto/ausencias.html:278`; formulário individual disponível em view/template [não verificado]. Não demonstrada diferença de estado; texto de trilha e envelope diferem.  
2. **Recusar pergunta:** detalhe `app/chamados/views.py:505` e fio/modal `app/chamados/services/fio.py:621` → `acao_recusar_pergunta`, `app/chamados/services/acoes_disputa.py:65` → `recusar_pergunta`, `app/chamados/services/validacao.py:494`. Mesmo gate de pergunta listada/disputa aberta. Templates `app/templates/chamados/partials/_pergunta_card.html:112`, modal :287.  
3. **Corrigir resposta:** detalhe view :523 e fio/modal `services/fio.py:621` → `acao_corrigir_resposta`, `app/chamados/services/acoes_disputa.py:298` → `validar_pergunta`. Detalhe escreve, mas botão de correção no template detalhe não confirmado; modal tem formulário `app/templates/chamados/partials/modal_fio.html:298`. Marcar exposição do segundo botão [não verificado].  
4. **Autorizar geofence; autorizar com advertência; recusar geofence:** três ações distintas, detalhe `app/chamados/views.py:540`/544 e fio/modal `services/fio.py:529` → `acoes_chamado.acao_autorizar_geofence`:165 / `acao_recusar_geofence`:243. Templates detalhe :285/:286 e modal :498/:509/:520. Mesmos escritores e gate contextual; fio/modal também têm marcação de lidas somente no modal (`app/chamados/views.py:877`).  
5. **Pedir confirmação de âncora:** detalhe view :455 → `nucleo_pedir_confirmacao_ancora`, `app/chamados/services/acoes_disputa.py:178`; fio/modal `services/fio.py:517` → wrapper :197 → mesmo núcleo. Modal formulário :636; botão detalhe [não verificado].  
6. **Reabrir questionário:** **detalhe e modal atuais convergem na mesma view** `reabrir_disputa_supervisao`, `app/chamados/views.py:1372`, rota `/chamados/local/<id>/reabrir-disputa/`; formulários `app/templates/chamados/detalhe_local.html:9` e modal :363. **Não contar falso par divergente** com ramo `acao=reabrir_disputa` em `services/fio.py:637`: não achei template atual que o dispare [não verificado].  
7. **Regularizar disputa:** detalhe view :467 → `acao_regularizar_disputa`, `app/chamados/services/acoes_disputa.py:118`; modal/fio `services/fio.py:642` tem casca copiada → **mesmo escritor** `materializar_perguntas_validadas_da_disputa`, `app/chamados/services/materializacao.py:1313`, chamado em acoes :139 e fio :666. Templates detalhe :317/modal :350. Mesmo gate de perguntas respondidas e mesma trilha; só exceção de push difere: `pass` em acoes :152/:158 versus warning no fio :679/:686. **Sem diferença financeira demonstrada pelo código desses caminhos.**
8. **Agendar manual×lote** e **editar normal×admin** já descritos acima: escritores comuns `executar_agendamento`:306 e `_trocar_janela`:366, respectivamente; preservar indicação de convergência.
9. **Fio×modal**, para ações que ambos efetivamente mostram, convergem em `processar_post_fio`, `app/chamados/services/fio.py:481`, chamado por `app/chamados/views.py:674` e :954. Modal marca notas como lidas antes do POST em :880; fio não faz esse efeito. Não afirmar que ambos exibem todos os verbos: template `fio_colaborador.html:257` comprova resposta/status; modal exibe os outros verbos citados acima.

**Admin framework:** `AfastamentoAdmin` é editável em `app/ferias/admin.py:4`–:14; `Afastamento` não sincroniza Ausencia em `app/ferias/models.py:488`–:535. A cópia só contém registro/configuração desse admin, não implementação de suas views POST [não verificado]. Não incluí na contagem de 47 nem declarei automaticamente equivalência com lançar `Ausencia(tipo='afastamento_inss')`.

## 06/11 21:04 · sessao bd2417475ff3 · - · - · origem=Ronald (tmux)

Q3 — Regra documentada na cópia:

- O fundamento local para a HE100 de feriado na janela noturna ser HE100 noturna está expresso em `app/ponto/calculador/regras.py:793-798`: a dobra decide HE50/100; adicional noturno vale em qualquer dia (art.73); HE100 na janela é HE100 noturna (Súmula264). O caso normativo dos testes também invoca art.73, Súmula60II, Súmula146 e Súmula264 em `app/ponto/tests/test_v3_dobra_sem_noturna.py:4-17`. Validade jurídica externa desses enunciados: [não verificado].
- Isso depende de o feriado efetivamente ter direito à dobra: o piso local do 12x36 é simples; a CCT pode habilitar dobra pelo cadastro `feriado_12x36_em_dobra` em `app/core/regua_cct.py:473-478`. Não decorre de “ser vigilante/12x36” automaticamente.
- CCT Vigilantes Londrina, cl.38-d, registrada nos comentários: 12x36 usa hora de 60min, mantendo adicional noturno; afasta redução, não adicional — `app/core/regua_cct.py:127-149`. Aplicação operacional apenas quando `r['hora_reduzida_afastada_12x36']`, ciclo12x36 e trava desligada — `app/core/regua_cct.py:467-472`.
- Prorrogação: piso CLT12x36 é 22–05 estrito; jornada comum aplica art.73§5/Súmula60II; CCT decide pelo cadastro — `app/core/regua_cct.py:153-168`. Logo, “após05 sempre noturno para qualquer12x36” contrariaria a regra local.
- Unidade HE100noturna = **horas de relógio**, subconjunto da HE100. `he_noturna` mede minutos pela diferença temporal em `minutos_noturnos` (`app/ponto/motor_calculo_v2.py:112-116`), desconta intervalo e reparte por proporção sem fator (`:440-451`); `ResultadoCalculo.horas_extra_100_noturna` soma minutos e divide60 (`:342-344`); calculador faz `/60` também (`app/ponto/calculador/regras.py:832-839`).
- Unidade `horas_noturnas` é diferente: relógio × fator; fator padrão60/52,5 (`app/ponto/motor_calculo_v2.py:46`), ou1 quando CCT afasta (`:983-992`). Assim, no caso de toda a HE100 abranger todo o trabalho noturno, HE100noturna = horas_noturnas/fator. Não generalizar essa igualdade para dia em que apenas parte do trabalho é HE100 — a proporção em `:447-450` limita a rubrica.

Q6 — **Existem testes explícitos**, com distinção de escopo: [nao verificado]

1. `app/ponto/tests/test_v3_dobra_sem_noturna.py:71-76`: chama `regras.do_dia`, exige HE100noturna7h e HE50noturna0. Fixture6x1, feriado, turno22–06, pausa02–03, he_pontas com toda jornada em100 em `:40-57`; exige noturnas8h em `:62-69`. O gêmeo sem dobra exige HE100noturna0 em `:78-84`; caso com excedente exige HE1009h, HE100noturna8h e noturnas reduzidas em `:86-99`.
2. `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:73-81`: teste pela porta real (`:58-69`), **também lê a saída do motor**: HE1009h, HE100noturna8h, e HE100noturna×60 igual a `periodos[0].minutos_extra_100_noturna`. Fixture é ciclo6x1 com base12x36, turno21–07 e intervalo02–03 em `:43-49`; não é ciclo12x36vigilante com CCT.
3. Gêmeos sem pausa exigem8h e9h de HE100noturna e diferença de1h pela pausa em `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:83-92`. Propriedade de noturnas/intervalo, inclusive feriado, em `:94-103`.
4. `app/ponto/tests/test_o280_bateria_da_lei.py:1354-1373` cobre12x36noturnoferiado no **piso simples**: HE100feriado0, extra0, noturnas relógio360min/legais411,43. Não afirma HE100noturna positiva.
5. `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:292-321` cobre HE100noturna em **não feriado**, com duas pontas; exige retorno somado `he_noturna/60` (`:315-320`).
6. `app/ponto/tests/test_motor_comercial_dia_do_turno.py:72-78` cobre turno cruzando meia-noite: feriado só no dia seguinte não dobra cauda; feriado no dia do turno dobra todo. Não verifica HE100noturna.

Execução/passagem dos testes nesta cópia: [não verificado]; fiz apenas leitura. [nao verificado]

## 06/11 21:03 · sessao 63fd0ec05f72 · PLACAR-LIVENESS · leitura · origem=Code

**O 400 explícito desta rota recusa o anexo da foto antes de consultar a batida. Ele não demonstra que a marcação de ponto foi recusada.** A view busca uma batida existente pelo ID e pelo colaborador autenticado (`app/api/views_core.py:840–851`).

**1. View, entrada, gravação e leitores**

O caminho é: Caddy encaminha `/api/ponto/*` ao core; a URLconf inclui `api.urls_core` sob `/api/`; esta importa `views_core` e aponta `ponto/foto/` para **`api_upload_foto`** (`Caddyfile:8–9`; `app/config/urls_core.py:6`; `app/api/urls_core.py:5–8`; `app/api/views_core.py:839`).

A view aceita POST autenticado, recebe `batida_id` em `request.data` e o arquivo `foto` em `request.FILES`; a batida precisa pertencer a `request.user.colaborador` (`app/api/views_core.py:837–846`).

O escritor chamado é `ponto.registro_batida.anexar_foto(..., registro='atualizar')`. Ele grava:

- **`Batida.foto`**, via `anotar` e `batida.save(update_fields=...)`; o campo é um `ImageField` em `batidas/fotos/` (`app/api/views_core.py:850–851`; `app/ponto/registro_batida.py:307`; `app/ponto/registro_batida.py:252–254`; `app/ponto/models.py:78`).
- **`FotoBatida.foto`**, via `update_or_create(batida=...)`; o modelo tem relação um-para-um com `Batida` e armazena em `fotos_batidas/%Y/%m/` (`app/ponto/registro_batida.py:308–311`; `app/ponto/models.py:1026–1030`).

Depois de anexar, a view chama o tratamento das disputas de `foto_ausente` associadas àquela batida e retorna `status: ok` com `disputas_fechadas` (`app/api/views_core.py:860–863`; `app/api/foto_recebida.py:61–80`).

Leitores encontrados desses campos:

| Leitor | Uso e prova |
|---|---|
| Feed do dashboard | Mostra `b.foto.url` (`app/templates/core/_d2_feed.html:6`). |
| Detalhe do colaborador | Mostra a selfie da batida (`app/templates/colaboradores/detalhe.html:186`). |
| Detalhe da batida | Extrai a URL (`app/core/views.py:1005`). |
| Mapa do colaborador | Serializa a URL (`app/colaboradores/services/detalhe.py:87`). |
| Drawer do colaborador | Calcula `tem_selfie` (`app/colaboradores/services/drawer.py:64`). |
| Upload web de selfie | Consulta se `Batida.foto` já existe antes de anexar (`app/ponto/views.py:2901–2903`). |
| Mídia protegida | Consulta os dois modelos para autorizar acesso (`app/core/media_protegida.py:25–33`). |
| Passe retroativo de foto ausente | Considera tanto `FotoBatida` quanto `Batida.foto` (`app/ponto/management/commands/marcar_foto_ausente_retro.py:70–77`). |

**2. Ramos 400, estado da batida/foto e trilha**

Há **um único ramo 400 explícito na view do core**, com condição composta:

| Condição | Retorno | Efeito desta chamada |
|---|---|---|
| `batida_id` ausente/falso **ou** `foto` ausente/falsa | `{'erro': 'batida_id e foto obrigatorios.'}`, HTTP 400 (`app/api/views_core.py:840–843`). | Retorna antes da consulta e do escritor: não cria, altera nem remove a batida ou sua foto nesse ramo (`app/api/views_core.py:843–851`). |

O contrato exige batida preexistente para anexar a foto, mas **o ramo 400 nem consulta sua existência**. Portanto, a existência da batida de cada pedido observado não pode ser comprovada pelo status 400 (`app/api/views_core.py:842–846`); nos dois casos de produção, isso está [nao verificado].

Batida inexistente para aquele colaborador produz **404**, em ramo separado (`app/api/views_core.py:845–848`). A cópia da view na casca UI tem a mesma guarda 400, em `app/api/views.py:850–853`.

**Não há chamada específica de `LogAuditoria`/`observ` nesse ramo 400**: ele retorna diretamente. A trilha de anexação fica depois, no caminho de escrita, por `anotar → _trilha → registrar_log → evento` (`app/api/views_core.py:842–851`; `app/ponto/registro_batida.py:263`; `app/ponto/registro_batida.py:225–232`; `app/core/auditoria.py:22–24`).

Existe configuração de **access log** com método, caminho, status e duração, inclusive persistência em `access-core.log`; isso é distinto de uma auditoria específica da recusa (`app/gunicorn.core.conf.py:34–41`; `app/gunicorn.core.conf.py:89–96`). A presença efetiva dessas trilhas para os pedidos informados está [nao verificado].

Outros 400 anteriores à execução da view, gerados por bibliotecas/parser/servidor, e qual ramo originou os dois 400 de produção: [nao verificado].

**3. Comportamento dos clientes**

Os dois clientes web indicados enviam a foto **inline para `/api/ponto/bater/`**, não para `/api/ponto/foto/` (`app/static/pwa.js:141–157`; `app/templates/ponto/bater_ponto_v2.html:331–338`).

| Cliente | Tratamento comprovado |
|---|---|
| `pwa.js`, sincronização offline | Para **400 de `/bater/`**, escreve `console.warn` e apaga o registro pendente, que contém também a foto; não reenvia esse registro. Nesse ramo não há aviso visual específico ao colaborador (`app/static/pwa.js:113–118`; `app/static/pwa.js:166–175`). |
| Tela `bater_ponto_v2.html` | Para erro HTTP de **`/bater/`**, mostra toast com `erro` ou mensagem contendo o status e reabilita o botão; permite nova tentativa manual. O armazenamento offline está no tratamento de exceção de rede, depois desse retorno (`app/templates/ponto/bater_ponto_v2.html:341–356`). |
| Service worker | Não intercepta POST (`app/static/sw.js:33–36`). |
| App nativo | Reenvio, descarte e aviso após 400 de **`/foto/`**: [nao verificado]. O backend descreve o envio separado pelo APK, mas isso não comprova o tratamento da resposta pelo aplicativo (`app/api/views_core.py:852–854`). |

Assim, **não se pode atribuir aos dois 400 de `/foto/` o descarte observado no código de `/bater/`** (`app/static/pwa.js:153–175`).

**4. Tempo, validade e limites**

- **Na view de foto:** não há leitura de `timestamp_dispositivo`, comparação com “agora”, validação de assinatura do aparelho, idade máxima da batida ou teste de duração. A guarda explícita de 400 verifica apenas ID/foto (`app/api/views_core.py:839–863`).
- **Autenticação:** há JWT e sessão; o access token está configurado para 12 horas e o refresh para 30 dias. O tratamento de expiração relança a exceção de autenticação, acrescentando `token_expirado`; não contém conversão da demora em 400 (`app/config/settings/base.py:32–50`; `app/api/credencial.py:221–256`).
- **Timestamp do aparelho:** sua comparação ocorre em `/bater/`, que chama `hora_do_aparelho`. Fora da janela, essa função devolve `agora`, o timestamp lido e o motivo; não devolve resposta HTTP 400 (`app/api/views_core.py:528–531`; `app/api/hora_do_aparelho.py:135–153`).
- **Prazo da selfie:** APK tem prazo de 90 minutos para caracterizar foto ausente; o passe retroativo pode abrir disputa. Esse mecanismo não é uma guarda de rejeição do upload (`app/ponto/registro_batida.py:72–91`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:95–104`; `app/api/views_core.py:839–863`).
- **Casca:** o Gunicorn do core configura `gthread`, `timeout=30` e `graceful_timeout=30`. O proxy para o core não declara limite de corpo ou timeout nesse bloco. Esses trechos não estabelecem uma regra “passou de 30 s → 400” (`app/gunicorn.core.conf.py:1–6`; `Caddyfile:8–9`).
- **Upload:** a view não verifica tamanho explicitamente. O limite efetivo das bibliotecas/infraestrutura e eventual atuação nos dois pedidos estão [nao verificado] (`app/api/views_core.py:839–863`).

**A causa das durações de 168,6 s e 200,2 s e sua relação causal com os 400 permanecem [nao verificado].**

**5. PLACAR-LIVENESS e dono L-099**

Aplicando o limiar informado no enunciado aos tempos fornecidos, sem confirmação independente dos eventos [nao verificado]:

| Caso fornecido | (2) POST >30 s | (4) Batida recusada sem trilha ou saída | Dono L-099 |
|---|---|---|---|
| 168,6 s → 400 | **Sim**, pelo dado informado [nao verificado]. | **Não demonstrado**: o 400 explícito é anterior à busca da batida e recusa o anexo (`app/api/views_core.py:842–851`); ramo real e efeito no cliente [nao verificado]. | **Não sei** [nao verificado]. |
| 200,2 s → 400 | **Sim**, pelo dado informado [nao verificado]. | **Não demonstrado**, pela mesma distinção entre anexação e marcação (`app/api/views_core.py:842–851`); ramo real e efeito no cliente [nao verificado]. | **Não sei** [nao verificado]. |
| 36,4 s → 200 | **Sim**, pelo dado informado [nao verificado]. | **Recusa não demonstrada**; o retorno normal desta view ocorre depois da anexação e do tratamento das disputas (`app/api/views_core.py:850–863`). | **Não sei** [nao verificado]. |

A L-099 exige dono por divergência; o juiz implementado recebe fatos do dia, cadastro e batidas, e **não classifica por duração/status HTTP**. Esses insumos não foram fornecidos para os três casos (`CLAUDE.md:176`; `app/ponto/management/commands/e6_oraculo.py:53–54`; `app/ponto/management/commands/e6_oraculo.py:98–110`) [nao verificado].

## 06/11 20:54 · sessao 418dcb5a30a1 · PLACAR-LIVENESS · leitura · origem=Code

**Não há dados de produção suficientes para publicar nenhum dos cinco números. Todos permanecem [nao verificado].** Há autoridades aproveitáveis para medir (1); para (4) e (5), o enunciado integral excede o que as fontes existentes conseguem demonstrar. Os limites concretos estão abaixo. (`app/folha/fora_da_folha.py:203`; `app/api/views_core.py:402`; `app/chamados/services/vigia_fio.py:334`, `:577`)

**1. Dia que segura a folha sem porta**

**Autoridade real:** `folha.fora_da_folha.o_que_segura`. Ela pede os dias retentores a `motivos_retencao_celula` e consulta `porta_alcancavel_do_dia`; `fio == FIO_SEM_PORTA` é a resposta existente para “retém sem porta”. Não é necessário reproduzir a regra. (`app/folha/fora_da_folha.py:95`, `:203`, `:217`, `:221`; `app/chamados/juizes.py:1329`)

**Universo:** enumerar `universo_da_competencia`, por empresa, e chamar `o_que_segura` para cada colaborador. Usar somente `fora_da_folha`, `dias_que_seguram` ou `censo_alcance` reduz indevidamente a frota: a classificação padrão começa nos `FechamentoMensal` existentes e o segundo filtro aceita somente `furo_espelho`. (`app/ponto/services/fechamento.py:382`, `:439`; `app/folha/export.py:777`; `app/folha/fora_da_folha.py:119`, `:128`)

A composição existente de competências abertas é `competencias_a_lavrar`: corrente incondicionalmente, mais anterior enquanto não entregue. **Ela não enumera todas as competências históricas abertas.** A sonda abaixo declara esse escopo. (`app/ponto/services/he_pendente_lavrado.py:209`, `:220–225`)

**Exclusões da autoridade:** isentos; datas fora de admissão/demissão; dia sem célula e sem vínculo; protestos integralmente não cobráveis; pausa isolada absolvida pelo juiz correspondente. Cegueira passada com vínculo pode reter; não deve ser eliminada pela sonda. (`app/folha/export.py:1192–1214`, `:1227–1256`)

**Porta** significa chamado da célula na fila, ou pergunta viva no app com âncora na fila e resposta não vazia. Acesso manual por URL e “Resolver dia” ficam expressamente fora dessa definição; ela também não recebe um usuário admin concreto para testar sua permissão. (`app/chamados/juizes.py:1341–1370`, `:1372–1387`)

**2. POST de tela acima de 30 segundos, últimas 24 horas**

**Não existe medidor pronto identificado para esse contrato.** Existe a fonte: Gunicorn escreve método, rota, status e duração em segundos, em `/app/logs/access-ui.log*` e `/app/logs/access-core.log*`, além do console. O `timeout=30` é configuração, não contagem de requisições lentas. (`app/gunicorn.conf.py:21`, `:42–49`, `:83–104`; `app/gunicorn.core.conf.py:34–41`, `:75–96`)

Quem escreve os campos é o access logger; `colab` e `cli` vêm dos cabeçalhos produzidos por `LogContextoMiddleware`. `colab=u123` identifica **User**, não `Colaborador` 123: precisa resolver a relação antes de emitir `colNNN`. O leitor existente `_uso_no_log` conta uso de portas, mas não aplica duração nem janela de 24 horas. (`app/core/middleware.py:114–119`, `:139–162`; `app/core/portas_da_tela.py:82–100`)

Padrão proposto, sobre a mensagem original do access log:

```python
ACCESS = re.compile(
    r"^(?P<ts>\d{4}-\d\d-\d\d \d\d:\d\d:\d\d) "
    r"\S+ ip=(?P<ip>.*?) colab=(?P<ator>-|u?\d+) "
    r"cli=(?P<cli>.*?) (?P<metodo>[A-Z]+) (?P<rota>/\S*) -> "
    r"(?P<status>\d{3}) (?P<segundos>\d+(?:\.\d+)?)s ua=.*$"
)
```

O padrão exige a linha completa emitida pelo escritor; uma frase mencionando “POST”, “30s” ou “timeout” não basta. Depois do casamento: janela fixa `[agora−24h, agora)`, método `POST`, duração **estritamente maior que 30**, uma ocorrência por requisição. A forma dos campos vem do escritor; a disponibilidade de 24 horas completas e o fuso efetivo dos arquivos são [nao verificado]. (`app/gunicorn.conf.py:48–49`, `:83–84`)

“De tela” ainda precisa de um universo explícito de rotas/canais: a V2 envia ao endpoint API no core; excluir `/api/` perderia essa tela. O censo estático de portas exclui API e, portanto, sozinho não resolve essa seleção. (`app/templates/ponto/bater_ponto_v2.html:335–338`; `app/core/portas_da_tela.py:20`, `:112–117`)

**3. HTTP 500 com Traceback para admin ou colaborador, 24 horas**

**Existe um medidor próximo, mas insuficiente:** `selo_paginas_500.sh` procura `50[0-9]`, usa cinco minutos por padrão, não exige Traceback e permite falha do pipeline produzir zero. Não atende ao contrato. (`bin/selo_paginas_500.sh:13–19`)

**Fontes:** access logs para status, rota e ator; logger `django.request` para a exceção, escrito em `/app/logs/errors.log*` e console. O arquivo de erros não contém casca, usuário ou request ID no formato configurado. Para preservar a casca, ler separadamente o console de `saas_ui` e `saas_core`, com limites explícitos de tempo; não somar arquivo e console do mesmo erro. (`app/config/settings/saas.py:122`, `:131–142`, `:173`; `app/gunicorn.conf.py:48–49`; `bin/selo_paginas_500.sh:15–16`)

Padrões propostos para o registro de erro:

```python
ERROR500 = re.compile(
    r"^(?P<ts>\d{4}-\d\d-\d\d \d\d:\d\d:\d\d,\d{3}) "
    r"ERROR django\.request Internal Server Error: (?P<rota>/\S*)$"
)
TRACEBACK = re.compile(r"^Traceback \(most recent call last\):$")
```

Exigir o cabeçalho e o Traceback **no mesmo registro multilinha**, antes do próximo cabeçalho; casar também um access record com `status == 500`. Isso exclui prosa solta e tracebacks de boot ou de outro logger. O formatter é verificável; uma ocorrência concreta desse cabeçalho Django não foi fornecida, portanto o padrão precisa ser confrontado com a linha real antes de valer como instrumento. (`app/config/settings/saas.py:122`, `:173–174`; `app/gunicorn.conf.py:89–106`; [nao verificado])

**Limite decisivo:** correlação por horário e rota não identifica inequivocamente uma requisição sob concorrência. Sem vínculo seguro entre erro, access record e ator admin/colaborador, esse caso é `NAO SEI`; não cabe eliminá-lo. Os formatos existentes não incluem request ID compartilhado. (`app/config/settings/saas.py:122`; `app/gunicorn.conf.py:48–49`; `app/core/middleware.py:150–162`)

**4. Batida recusada sem trilha ou sem saída para o colaborador**

**Não existe fonte completa nem função de medida equivalente.** A ausência da própria tentativa no banco impede encontrar todas as recusas sem trilha; também falta comprovação persistida de apresentação/entrega da saída. Os contraexemplos são concretos:

| Caminho | O que a cópia demonstra |
|---|---|
| API | Recusas por situação e tipo inválido retornam antes da auditoria GPS. (`app/api/views_core.py:402–426`) |
| V2 antes do POST | Câmera/GPS podem interromper o envio apenas com aviso e `return false`. (`app/templates/ponto/bater_ponto_v2.html:285–301`, `:325`) |
| Colisão | `registrar_batida` pode devolver `None, conflito`; a API responde HTTP 200, `sucesso=True`, `ignorada_intervalo`. (`app/ponto/registro_batida.py:138–151`; `app/api/views_core.py:695–718`) |
| V2 depois do POST | O ramo `r.ok` não verifica esse status antes de apresentar sucesso. (`app/templates/ponto/bater_ponto_v2.html:340`) |
| Tela antiga | Há retorno `ignorada_intervalo` sem registro da tentativa nesse ramo. (`app/ponto/services/triagem_batida.py:707–732`) |
| Fila offline | A tentativa reside no IndexedDB; respostas 400 podem removê-la com `console.warn`. (`app/static/pwa.js:107–175`) |

A parte consultável é `LogAuditoria.acao == "batida_recusada_sem_gps"`, escrita pela API. Isso conta recusas GPS **com trilha**, não as recusas sem trilha. Até essa escrita é best effort. O middleware de mensagem melhora o JSON devolvido, mas não comprova recebimento pelo colaborador. (`app/api/views_core.py:440–458`; `app/core/observ.py:105`, `:133–139`; `app/core/middleware.py:19–46`)

Logo, a sonda devolve **`C4 NAO SEI`**, acompanhada do inventário GPS disponível. Transformar esse inventário em contador integral seria afirmar o que a fonte não mostra. (`app/api/views_core.py:402–458`)

**5. Pergunta ou chamado vivo que o admin não alcança**

**Não existe uma autoridade única para o enunciado global.** Existem três recortes distintos:

| Instrumento real | Universo e limite |
|---|---|
| `vigia_fio.censo` | Chamado vivo com célula, módulo do vocabulário de furo e célula acusando. Mede ausência de fio vivo, não permissão de acesso do admin. (`app/chamados/services/vigia_fio.py:334–379`, `:417`) |
| `vigia_fio.censo_alcance` | Dias retidos pela folha nas competências de `competencias_a_lavrar`; usa a porta real. Exclui hoje/futuro e quatro baldes de encaminhamento. (`app/chamados/services/vigia_fio.py:542`, `:577–589`, `:642–660`) |
| Fio mudo | Está dentro do emissor, com filtros próprios; não há função pública pura equivalente ao contador global. Não executar o comando como sonda de leitura. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141–159`, `:251`, `:304`) |

Os quatro baldes são decisão do dia pendente, ausência em decisão, escala que não descreve o dia e fio do admin. Este último exige presença **do dia** na lista e dono lavrado BATIDA/CADASTRO. Não equivalem a provar que um admin concreto consegue alcançar todo objeto vivo. (`app/chamados/services/vigia_fio.py:274–315`; `app/chamados/juizes.py:1341`)

A sonda publica os dois censos como diagnósticos completos, mas mantém **`C5 NAO SEI`** para o enunciado literal. Falta definir e implementar o universo global, a identidade/permissões do admin e a composição sem duplicação entre pergunta, chamado e dia. (`app/chamados/services/vigia_fio.py:334`, `:577`; `app/chamados/juizes.py:1341`)

**Fontes de banco, escritores e leitores usados**

Os campos abaixo são fontes das autoridades e dos diagnósticos; valores derivados em memória, como `fio` e `dono`, não são uma nova tabela de fatos. (`app/folha/fora_da_folha.py:221–231`; `app/ponto/management/commands/e6_oraculo.py:172–183`)

| Modelo/campos | Quem escreve | Quem lê |
|---|---|---|
| `Empresa.dia_inicio_competencia` | Formulário → porta de estrutura. (`app/colaboradores/views.py:523`, `:538`; `app/colaboradores/services/estrutura.py:17`) | Janelas. (`app/ponto/janelas.py:36–57`) |
| `Colaborador.empresa`, `situacao`, admissão/demissão, `isento_ponto` | Cadastro/desligamento → portas de colaborador. (`app/colaboradores/views.py:176`, `:1313–1342`; `app/colaboradores/services/desligamento.py:103–108`; `app/colaboradores/services/colaborador.py:55–76`) | Universo e retenção. (`app/ponto/services/fechamento.py:439–445`; `app/folha/export.py:1192–1201`) |
| `CelulaDia.colaborador`, `data`, `trabalha` | Geração/edição pela porta de célula. (`app/ponto/portas/celula.py:600`, `:849–868`) | Retenção/censos. (`app/folha/export.py:1206`, `:1255`; `app/chamados/services/vigia_fio.py:378`) |
| `CelulaDia.veredito`, `protestos`, `ata` | `lavrar_veredito`. (`app/ponto/portas/celula.py:969–1035`) | Retenção e juiz da pausa. (`app/folha/export.py:1142`, `:1233–1254`) |
| `FechamentoMensal.colaborador`, mês/ano, horas, turnos abertos | Recalcular fechamento. (`app/ponto/services/fechamento.py:1121–1155`) | `classificar_export`. (`app/folha/export.py:777–780`, `:823–828`) |
| `ChamadoColaborador.colaborador`, `celula`, `modulo_origem`, `status_local` | `abrir`/`criar`; mudanças pelo núcleo de estado. (`app/chamados/models.py:222`, `:796`, `:1015`) | Painel e censos. (`app/chamados/services/painel.py:155`, `:187`; `app/chamados/services/vigia_fio.py:338`, `:607`) |
| `ChamadoColaborador.silenciado_ate` | Silenciar/levantar. (`app/chamados/services/arquivo.py:166`, `:201`) | Fila. (`app/chamados/services/painel.py:188`) |
| `PerguntaDisputa.celula`, `data_evento`, `chamado_destino` | Porta de abertura. (`app/chamados/models.py:2436–2476`) | Alimentação e porta. (`app/chamados/juizes.py:1300`, `:1378–1381`) |
| `resposta_colab`, `respondida_em`, `validada_em` | Resposta, validação e portas de carimbo. (`app/chamados/services/disputa_emissao.py:1282–1295`, `:1433–1450`; `app/chamados/services/validacao.py:131`; `app/chamados/models.py:2589–2592`) | Vitalidade e resposta vazia. (`app/chamados/juizes.py:1136`, `:1150`, `:1257`) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, pelas portas de carimbo. (`app/chamados/models.py:2556–2562`, `:2625`) | Vitalidade. (`app/chamados/juizes.py:1171`, `:1207`) |
| `LogAuditoria.pk`, `acao`, `modelo`, `objeto_id`, `criado_em`, payload GPS | Campos do modelo; `evento` persiste; API monta a recusa GPS. (`app/core/models.py:285–307`; `app/core/observ.py:134–137`; `app/api/views_core.py:440–451`) | Tela genérica de auditoria e inventário da sonda. (`app/core/views.py:686–738`) |
| `Batida.timestamp`, `retratada_em` | Registro e retratação. (`app/ponto/registro_batida.py:157–160`, `:267–281`) | `batidas_apuraveis` e insumos do dono. (`app/ponto/turnos.py:2844–2864`; `app/ponto/management/commands/e6_oraculo.py:144`) |
| Resultado `dias_cadastro_x_realidade` | Motor, em memória. (`app/ponto/motor_calculo_v2.py:2719`) | Insumos do dono. (`app/ponto/management/commands/e6_oraculo.py:154–161`) |
| `MetricaSnapshot` da fase: chave, escopo, `data_ref`, `calculado_em`, `detalhe.janelas/itens/itens_reabertos` | Lavra do censo de fase. (`app/escala/services/fase_12x36.py:85–93`) | Cobertura e fase as-of. (`app/escala/services/fase_12x36.py:97–148`) |

**Dono L-099 por caso**

Para divergência de **dia**, chamar `InsumosDoDono(...).dono(data)`, que monta os insumos e chama o juiz real. A ordem é: folga indefinida/sem vínculo → CADASTRO; batida solta ou paridade ímpar → BATIDA; fase deslocada → CADASTRO; Cadastro x Realidade → CADASTRO; restante → ESTRUTURA. Não inferir dono pela classe do vigia. (`app/ponto/management/commands/e6_oraculo.py:98–110`, `:113–183`; `app/chamados/services/vigia_fio.py:99–101`)

Para timeout, 500 ou falta de instrumentação, esse juiz diário não é autoridade de causa. Os defeitos demonstrados de código são estruturais; atribuir um evento de produção específico a CADASTRO/BATIDA/ESTRUTURA sem evidência é [nao verificado]. (`app/ponto/management/commands/e6_oraculo.py:113–125`)

Há um furo adicional: fase nunca medida volta como conjunto vazio. A sonda conserva `dono_e6`, mas marca `dono=NAO SEI` quando o residual ESTRUTURA depende dessa ausência de cobertura; usa o próprio predicado de cobertura existente. (`app/escala/services/fase_12x36.py:113–148`)

**Sonda devolvida como texto; execução e resultados [nao verificado].** Ela usa as funções citadas, impede escrita por transação PostgreSQL `READ ONLY`, não chama comandos lavradores e só publica campos selecionados. Qualquer exceção ou incremento de engolidos produz `NAO SEI`. Essa proteção é necessária porque a vitalidade engole falhas e pode alcançar `get_or_create` do usuário de sistema. (`app/chamados/juizes.py:1204–1218`, `:1069–1070`; `app/core/observ.py:60–81`; `app/core/utils.py:18`)

```python
# Colar no Django shell já inicializado.
# Não grava script, carimbo, auditoria ou arquivo de saída.
import contextlib
import datetime as dt
import io
import json
import logging
import sys
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

agora = timezone.now()
hoje = timezone.localdate(agora)


def medir(rotulo, funcao):
    anterior = logging.root.manager.disable
    try:
        logging.disable(logging.CRITICAL)
        with contextlib.redirect_stdout(io.StringIO()), \
                contextlib.redirect_stderr(io.StringIO()):
            from core.observ import engolidos
            with schema_context("juliani"), transaction.atomic():
                if connection.vendor != "postgresql":
                    raise RuntimeError()
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL "
                        "REPEATABLE READ READ ONLY"
                    )
                antes = engolidos()["total"]
                with patch("django.utils.timezone.now", return_value=agora):
                    resultado = funcao()
                if engolidos()["total"] != antes:
                    raise RuntimeError()
                transaction.set_rollback(True)
    except Exception:
        resultado = [(rotulo, "NAO SEI")]
    finally:
        logging.disable(anterior)

    for etiqueta, valor in resultado:
        print(etiqueta, json.dumps(valor, ensure_ascii=True))


def anotador():
    from ponto.janelas import janela_atual
    from ponto.management.commands.e6_oraculo import InsumosDoDono
    from escala.services.fase_12x36 import _indice_de_cobertura, _cobre

    insumos = {}
    indice = None

    def anotar(colab, dia, rota):
        nonlocal indice
        ini, fim = janela_atual(dia, colab.empresa)
        chave = (colab.pk, ini, fim)
        if chave not in insumos:
            insumos[chave] = InsumosDoDono(
                colab, ini, fim, hoje, agora=agora
            )
        dono_e6 = insumos[chave].dono(dia)
        dono = dono_e6

        if dono_e6 == "ESTRUTURA":
            if indice is None:
                indice = list(_indice_de_cobertura(hoje))
            coberto = any(
                _cobre(janelas, colab.empresa_id, dia.isoformat())
                for _pk, janelas, _instante in indice
            )
            if not coberto:
                dono = "NAO SEI"

        return {
            "chave": "col%03d/%s" % (colab.pk, dia.isoformat()),
            "rota": rota,
            "dono": dono,
            "dono_e6": dono_e6,
        }
    return anotar


def separar(casos):
    estrutura = [x for x in casos if x["dono"] == "ESTRUTURA"]
    admin = [x for x in casos if x["dono"] in ("CADASTRO", "BATIDA")]
    incertos = [x for x in casos if x["dono"] == "NAO SEI"]
    if len(estrutura) + len(admin) + len(incertos) != len(casos):
        raise RuntimeError()
    return {
        "contagem_estrutura": "NAO SEI" if incertos else len(estrutura),
        "total_casos": len(casos),
        "casos_estrutura": estrutura,
        "lista_admin": admin,
        "dono_nao_sei": incertos,
    }


def sonda1():
    from colaboradores.models import Empresa
    from chamados.juizes import FIO_SEM_PORTA, tranca_em_lote
    from folha.fora_da_folha import o_que_segura
    from ponto.services.fechamento import universo_da_competencia
    from ponto.services.he_pendente_lavrado import competencias_a_lavrar

    empresas = list(Empresa.objects.all().order_by("pk"))
    if not empresas:
        raise RuntimeError()
    anotar = anotador()
    casos, vistos, escopo = {}, 0, []
    dias_retentores = 0

    with tranca_em_lote():
        for empresa in empresas:
            for mes, ano in competencias_a_lavrar(empresa, hoje):
                escopo.append([empresa.pk, mes, ano])
                frota = universo_da_competencia(
                    mes, ano, empresa.pk
                ).select_related("empresa").order_by("pk")
                for colab in frota:
                    vistos += 1
                    resposta = o_que_segura(colab, mes, ano)
                    # Nunca imprimir o dict: ele contém nome.
                    for dia in resposta["dias"]:
                        dias_retentores += 1
                        if dia["fio"] != FIO_SEM_PORTA:
                            continue
                        data = dt.date.fromisoformat(dia["data"])
                        casos[(colab.pk, data)] = anotar(
                            colab, data, "sem_porta"
                        )
    if not vistos:
        raise RuntimeError()

    resultado = separar([casos[k] for k in sorted(casos)])
    resultado.update({
        "fonte": "o_que_segura",
        "escopo": "competencias_a_lavrar",
        "empresa_mes_ano": escopo,
        "colaborador_competencias_examinados": vistos,
        "dias_retentores": dias_retentores,
        "medido_em": agora.isoformat(),
    })
    return [("C1", resultado)]


def sonda4():
    from core.models import LogAuditoria

    casos = []
    registros = LogAuditoria.objects.filter(
        acao="batida

[truncado em 20000 de 24804 caracteres]

## 06/11 20:52 · sessao 1cb72ae84c20 · BO-[nome]-0611-V2 · censo · origem=Code

**Classificação principal: (e) nos itens 02, 05, 07 e 08; (b) nos itens 09, 11 e 12.** Nos casos (b), o descarte aparece na própria consolidação de t1.r: ela registra a pendência e atribui “ja OK”. Não há despacho separado que permita atribuir esse descarte a outro responsável. [nao verificado]

A distinção entre tema não visto, achado descartado e requisito não perguntado segue `CLAUDE.md:377`. t1.q não definiu “ja OK hoje” como ação executada na tela, resultado posterior no espelho/DiaPago e validação operacional; cobrar retrospectivamente esse conjunto seria **(e)**. [nao verificado]

**02 — col993 → col78: (e), identificação humana não exigida.** A correção do ID vem de v2.r; a fonte humana que confirma o vínculo não foi anexada. [nao verificado]

**t1.q — trechos exatos:** [nao verificado]

> diga qual candidato colNNN e o caso (o de maior atividade na janela, salvo evidencia contraria) e o que os dados mostram;

> 02 correcao de horario do 02a nao chega (a do 02b chegou): compare os dois caminhos (Justificativa / RegularizacaoExterna / PerguntaDisputa) e onde o do 02a parou.

**t1.r — nascimento da escolha:** [nao verificado]

> **02 — col993 comparado com col1023: falta informacao.** Mantive col993, atividade 102 contra 91 e 19:

A resposta seguiu a heurística expressamente pedida. Sem os CANDIDATOS/FICHAS originais, não se demonstra que havia identificação contrária disponível e ignorada. A regra de correspondência por nome completo existe em `CLAUDE.md:142`; atividade não fornece essa confirmação. [nao verificado]

**Regra preventiva proposta:** “Só vincule o item após confirmação humana de colNNN + nome completo, preservando apenas colNNN na saída. Sem confirmação, falta informacao; maior atividade não decide identidade.” [nao verificado]

**05 — col155 → col456: (e), mesma lacuna de identificação.** A correção do ID vem de v2.r. [nao verificado]

**t1.q — trechos exatos:** [nao verificado]

> diga qual candidato colNNN e o caso (o de maior atividade na janela, salvo evidencia contraria) e o que os dados mostram;

> 05 colab diz que o GPS nao aciona e nao consegue bater:

> Veja as batidas e devices da FICHA 05.

**t1.r — nascimento da escolha:** [nao verificado]

> **05 — col155: nosso com obra, dono ESTRUTURA; sem obra específica localizada.** É o maior candidato, atividade 23 contra 15;

Encontrar a barreira de GPS no código não confirma qual candidato fez a tentativa relatada; a própria re-triagem mantém a causalidade individual sem demonstração. [nao verificado]

**Regra preventiva proposta:** exigir a mesma confirmação humana do item 02 e registrar separadamente identidade, bloqueio genérico e tentativa individual comprovada. [nao verificado]

**07 — col836: (e), “visibilidade explicada” sem critério de encerramento do caso.** [nao verificado]

**t1.q — trecho exato:** [nao verificado]

> 07 chamado de 14/10 existe e nao aparece "na lista": qual lista de chamados do admin, qual filtro dela exclui o chamado da FICHA 07 (status, superado_por, silenciado, celula, empresa visivel)?

**t1.r — nascimento do “OK” limitado, depois consolidado como classe do item:** [nao verificado]

> **07 — col836, 14/10: ja OK hoje quanto à exclusão da lista inicial.**

> A pendência de escala indicada na célula é separada da visibilidade desse chamado resolvido.

A pergunta pediu a explicação da lista, e o código confirma o filtro entre cobrança e histórico (`app/chamados/services/painel.py:298`, `:314`). Faltou definir que explicar a exclusão não encerrava o caso operacional. Não se pode imputar à primeira leitura a omissão da cronologia específica revelada em v2 sem a FICHA original completa. [nao verificado]

**Regra preventiva proposta:** “Explique a visibilidade separadamente. Só classifique o item inteiro como ja OK após conferir a pendência da célula e o critério operacional explícito; ressalvas devem permanecer na tabela.” [nao verificado]

**08 — col617: (e), refutação parcial promovida a encerramento.** [nao verificado]

**t1.q — trechos exatos:** [nao verificado]

> 08 2o atestado com data errada aparece no espelho SEM aceite:

> status, quem registrou/aprovou (proprio colab?), e qual regra deixa uma declarada pelo colab valer sem passo do admin

**t1.r — nascimento do “OK” e sua própria limitação:** [nao verificado]

> **08 — col617, 05–06/11: ja OK hoje quanto à alegação de autoaceite.**

> o dado não sustenta aprovação pelo próprio colaborador; tampouco mostra a data correta do documento ou a identidade do decisor.

A resposta viu as lacunas. A cópia confirma que documento anexo nasce aguardando decisão (`app/ponto/services/ausencia.py:791`), mas isso não comprova o percurso histórico desse registro. v2.r também não demonstra autoaceite indevido nem atestado atualmente pendente. O falso fechamento amplo depende do critério que t1.q deixou indefinido. [nao verificado]

**Regra preventiva proposta:** “Não comprovar autoaceite não encerra a queixa. Exija período correto, decisão autorizada e resultado conferido; sem essas evidências, falta informacao.” [nao verificado]

**09 — col1000: (b), pendências vistas e descartadas na classificação.** [nao verificado]

**t1.q — trecho exato:** [nao verificado]

> 09 batidas faltando, sem chamado, e a tela mostra "resolvido": que tela/status diz resolvido nos dias da FICHA 09 sem batida, e quem decidiu.

**t1.r — nascimento do “OK”:** [nao verificado]

> **09 — col1000: ja OK hoje para os dias sem batida apresentados.**

**t1.r — o que já havia visto:** [nao verificado]

> **27/10 tem perguntas pendentes; 29 e 30/10 têm chamados próprios em análise**, criados hoje. A tela exata originalmente chamada de “resolvido” permanece sem identificação.

A resposta usou 26 e 31/10 para sustentar o fechamento, embora registrasse outros dias pendentes e não identificasse a tela solicitada. Esse descarte já é demonstrável em t1.r, sem exigir o critério adicional de DiaPago de v2. [nao verificado]

**Regra preventiva proposta:** “Confronte todos os dias da queixa. Dias explicados não encerram outros dias pendentes; sem identificar tela/status e decisor, não classifique o item como ja OK.” [nao verificado]

**11 — col921: (b), explicação do botão sobreposta às irregularidades já vistas.** [nao verificado]

**t1.q — trecho exato:** [nao verificado]

> 11 "Resolver dia" some depois de 20/10: resolver_dia na FICHA 11+12 por data; e regra da janela/competencia (21-20) ou defeito?

**t1.r — nascimento do “OK”:** [nao verificado]

> **11 — col921: ja OK hoje; regra de competência.**

**t1.r — pendência expressamente reconhecida:** [nao verificado]

> e também nos dias **21–23 e 25–28/10**, ainda irregulares.

A regra do botão está sustentada pelo código: exige último recurso, passado e status permitido (`app/colaboradores/services/calendario.py:103`, `:738`). A re-triagem não refuta essa regra; o descarte foi transformar sua explicação em fechamento do item apesar dos dias “ainda irregulares”. [nao verificado]

**Regra preventiva proposta:** “Responda separadamente funcionamento do botão e situação dos dias. Regra de competência correta não autoriza fechar pendências já identificadas.” [nao verificado]

**12 — col921: (b), segunda pergunta reconhecida como sem resposta comprovada.** [nao verificado]

**t1.q — trecho exato:** [nao verificado]

> 12 falta em folga recusada: confirme a regra no codigo; qual lancamento cobre abandono de posto em dia de folga/escala?

**t1.r — nascimento do “OK”:** [nao verificado]

> **12 — col921: ja OK hoje quanto à recusa de falta em folga.**

**t1.r — lacuna que já havia visto:** [nao verificado]

> **Não há lançamento específico de “abandono de posto em folga” demonstrado neste caso.**

> isso não comprova uma porta de tela para o lançamento pretendido

O veto de falta sem jornada prevista existe (`app/ponto/precedencia.py:536`, `:538`). Confirmá-lo respondeu à primeira metade; a própria resposta reconheceu que não comprovou o lançamento solicitado na segunda, mas consolidou o item como “ja OK”. [nao verificado]

**Regra preventiva proposta:** “Item com duas perguntas só recebe ja OK quando ambas estiverem respondidas com evidência. Sem demonstrar o lançamento adequado, mantenha falta informacao.” [nao verificado]

Não há suporte suficiente neste recorte para classificar algum dos sete como **(a)** ou **(d)**. Dados históricos ausentes e resultados operacionais não anexados constituem limites **(c)**; eles não demonstram, por si, uma falha de leitura nem MODO-COMUM. [nao verificado]

## 06/11 20:51 · sessao 7a8c9a2ef04b · - · - · origem=Ronald (tmux)

Contador 1 tem autoridades reais, mas não há função única com o universo integral e o contador L-099 solicitado: `o_que_segura` cruza retenção e porta, porém classifica o export apenas entre snapshots existentes; `dias_que_seguram` restringe ainda a `motivo='furo_espelho'`. Para a frota inteira, enumerar `universo_da_competencia` e chamar `o_que_segura` para cada colaborador. Fontes: `app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:221`, `app/folha/fora_da_folha.py:128`, `app/ponto/services/fechamento.py:382`, `app/folha/export.py:777`.

- Dia retentor: `motivos_retencao_celula(com_dias=True)` devolve `data`, `motivo`, `celula_id`, `veredito`; exclui isento, corta admissão/demissão, dispensa dia sem célula e sem vínculo, não acusa cegueira futura, dispensa protestos integralmente não cobráveis e pausa isolada pela autoridade existente. `app/folha/export.py:1168`, `app/folha/export.py:1192`, `app/folha/export.py:1198`, `app/folha/export.py:1227`, `app/folha/export.py:1231`, `app/folha/export.py:1253`.
- Porta: `porta_alcancavel_do_dia` aceita chamado da célula na fila ou pergunta viva no app com âncora na fila; acesso manual e “Resolver dia” ficam fora por decisão expressa. `app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1367`, `app/chamados/juizes.py:1375`.
- `o_que_segura` já chama essa porta e expõe `fio`; comparar `fio == FIO_SEM_PORTA` usa a resposta existente, sem copiar regra. `app/folha/fora_da_folha.py:70`, `app/folha/fora_da_folha.py:221`, `app/chamados/juizes.py:1329`.

Fontes e escritores/leitores:

| Campo | Escreve | Lê |
|---|---|---|
| `Empresa.dia_inicio_competencia` | `app/colaboradores/views.py:523` → `app/colaboradores/services/estrutura.py:17` | `app/ponto/janelas.py:36` |
| `Colaborador.situacao/data_demissao` | `app/colaboradores/services/desligamento.py:103` → `app/colaboradores/services/colaborador.py:73` | universo `app/ponto/services/fechamento.py:439`; retenção `app/folha/export.py:1200` |
| `Colaborador.empresa/data_admissao` | `app/colaboradores/views.py:1313` → `app/colaboradores/views.py:1342` | universo `app/ponto/services/fechamento.py:443`; retenção `app/folha/export.py:1198` |
| `Colaborador.isento_ponto` | `app/colaboradores/views.py:176` → `app/colaboradores/services/colaborador.py:55` | `app/core/constants.py:13` → `app/folha/export.py:1192` |
| `CelulaDia.colaborador/data/trabalha/dna/escala_geradora` | geração `app/ponto/portas/celula.py:849`; atualização `app/ponto/portas/celula.py:868`; edição `app/ponto/portas/celula.py:600` | retenção `app/folha/export.py:1206`; pausa `app/folha/export.py:1142`; vínculo `app/escala/alimentacao.py:228` |
| `CelulaDia.veredito/protestos/ata` | `lavrar_veredito`, `app/ponto/portas/celula.py:969`, atribuições `:1028` | `app/folha/export.py:1233`, `app/folha/export.py:1246`, `app/folha/export.py:1142` |
| `FechamentoMensal.colaborador/mes/ano/horas_trabalhadas/turnos_abertos` | `recalcular_fechamento_mes`, `app/ponto/services/fechamento.py:448`, persistência `:1137` e `:1153` | `classificar_export`, `app/folha/export.py:777`, `:823` |
| `Colaborador.codigo_dominio` | `app/colaboradores/views.py:1334` → `app/colaboradores/views.py:1342` | `app/folha/export.py:832`, `:864` |

Dono: usar `InsumosDoDono(c, ini, fim, hoje, agora=agora).dono(data)`, sem remontar seus insumos. Ele reúne batidas apuráveis, realizado/turnos, Cadastro x Realidade, cadastro do vínculo e fase lavrada, chamando `dono_da_divergencia`. `app/ponto/management/commands/e6_oraculo.py:113`, `:128`, `:172`.

A ordem real é: folga indefinida ou sem vínculo → CADASTRO; batida fora de turno ou contagem ímpar → BATIDA; fase deslocada → CADASTRO; Cadastro x Realidade → CADASTRO; resto → ESTRUTURA. `app/ponto/management/commands/e6_oraculo.py:98`.

Furo relevante: ausência de lavra de fase devolve conjunto vazio, embora o próprio código diga que não houve medida; isso pode produzir dono residual ESTRUTURA sem fase perguntada. `app/escala/services/fase_12x36.py:124`, `:132`, `:148`.

Guard somente leitura sem copiar regra:

```python
from escala.services.fase_12x36 import _indice_de_cobertura, _cobre

indice = list(_indice_de_cobertura(hoje))
fase_medida = any(
    _cobre(janelas, c.empresa_id, dia.isoformat())
    for _pk, janelas, _agora in indice
)
dono_e6 = insumos.dono(dia)
dono = "NAO SEI" if dono_e6 == "ESTRUTURA" and not fase_medida else dono_e6
```

Esse guard reutiliza a mesma seleção de lavras e o mesmo predicado de cobertura do leitor real. `app/escala/services/fase_12x36.py:97`, `:113`, `:134`. Se ocorrer, o contador estrutural deve ser `NAO SEI`, preservando a lista diagnóstica. [nao verificado]

Outra limitação: `competencias_a_lavrar` não enumera qualquer competência histórica aberta; inclui corrente incondicionalmente e anterior enquanto não entregue. `app/ponto/services/he_pendente_lavrado.py:209`, `:221`, `:223`.

## 06/11 20:51 · sessao 3c3d98446fcc · - · - · origem=Ronald (tmux)

O contador (5), literalmente “qualquer pergunta/chamado vivo que o admin não alcança”, **não tem uma função única equivalente identificada**: os instrumentos existentes têm universos diferentes e não cobrem essa formulação inteira. O `censo` parte de chamados vivos com célula e lâmpada acusando; `censo_alcance` parte exclusivamente dos dias que retêm a folha nas competências abertas; o fio mudo do emissor parte dos chamados vivos com célula na janela pedida. (`app/chamados/services/vigia_fio.py:334`, `app/chamados/services/vigia_fio.py:577`, `app/chamados/management/commands/disparar_perguntas_competencia.py:141`)

- **Autoridade de alcance:** `porta_alcancavel_do_dia` oferece porta se houver chamado da célula na fila ou pergunta viva no app com âncora na fila. O acesso manual ao chamado fechado e “Resolver dia” ficam fora dessa definição; o retorno não verifica as permissões de um usuário admin concreto. (`app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1367`, `app/chamados/juizes.py:1372`)
- **Fio morto antigo:** `censo(..., limite=sys.maxsize)` é leitura e entrega lista completa `celulas_do_fio_morto`; o contador ignora o limite da lista. Seu próprio comentário avisa que ele pode zerar enquanto persistem dias retidos cujo chamado morreu. (`app/chamados/services/vigia_fio.py:318`, `app/chamados/services/vigia_fio.py:454`, `app/chamados/services/vigia_fio.py:493`)
- **Alcance da folha:** `censo_alcance(..., limite=sys.maxsize)` fornece `[celula_id,colab_id,data,competencia,rota,causa,detalhe,papel]`; os casos `sem_porta` nunca são truncados. Exclui do contador alarmado hoje/futuro e os quatro baldes: decisão pendente, ausência aberta, escala não descreve o dia e fio do admin. (`app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:628`, `app/chamados/services/vigia_fio.py:648`, `app/chamados/services/vigia_fio.py:654`)
- **Fio mudo:** existe dentro de `Command.handle`, sem função pura pública de medida: seleciona vivos com célula na janela, elimina aqueles com pergunta viva, parcial destinada à lista e casos sem motivo atual da ata. `--alarme` retorna antes das escritas; o modo dry comum entra em tentativa de escrita com rollback e não serve como sonda estritamente somente leitura. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:148`, `app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `app/chamados/management/commands/disparar_perguntas_competencia.py:251`, `app/chamados/management/commands/disparar_perguntas_competencia.py:304`)
- **Outra autoridade de fio mudo:** invariante `F1` dentro de `auditar_invariantes_chamados`: elimina pergunta viva, qualquer resposta preenchida, colaborador não ativo, sem dia apurável, dia absolvido pela lei e fato já decidido. Não mede diretamente alcance do admin. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1624`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1648`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1715`)

Fontes e escrita/leitura:

| Campo | Escrita | Leitura |
|---|---|---|
| `ChamadoColaborador.status_local` | núcleo `_gravar_estado`, atribuição em `models.py:222`; nascimento por `criar` em `models.py:1015` | `painel.qs_vivos` em `painel.py:155`; `vigia_fio.censo` em `vigia_fio.py:335` |
| `ChamadoColaborador.celula`, `modulo_origem`, `colaborador` | campos do modelo em `models.py:32`, `models.py:40`, `models.py:44`; nascimento por `abrir`/`criar` em `models.py:796`, `models.py:1015` | censo lê célula/status/módulo em `vigia_fio.py:338`; alcance lê célula/status em `vigia_fio.py:607` |
| `silenciado_ate` | `arquivo.silenciar`, `arquivo.py:166`; `levantar`, `arquivo.py:201` | `painel.qs_na_fila`, `painel.py:188` |
| `PerguntaDisputa.celula`, `data_evento`, `chamado_destino` | `PerguntaDisputa.abrir`: `models.py:2436`, `models.py:2450`, `models.py:2476`; reapontamento também passa por serviço do fio | `alimentar_portas`, `juizes.py:1300`; `porta_alcancavel_do_dia`, `juizes.py:1378` |
| `resposta_colab`, `respondida_em` | resposta do colaborador: `disputa_emissao.py:1282`; texto recebido: `disputa_emissao.py:1433`; correção de carimbo pela trilha: `disputa_emissao.py:1450`; `carimbar` também escreve `respondida_em` em `models.py:2589` | vitalidade em `juizes.py:1150`; resposta vazia em `juizes.py:1257` |
| `validada_em` | `_carimbar_validacao`, `validacao.py:131`; `PerguntaDisputa.carimbar`, `models.py:2592`; nova resposta limpa em `disputa_emissao.py:1295` | primeiro degrau do juiz, `juizes.py:1136` |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, `models.py:2558`; portas `carimbar`/`carimbar_identidade`, `models.py:2562`, `models.py:2625` | juiz em `juizes.py:1145`, `juizes.py:1171`, `juizes.py:1207` |
| presença/dono do dia na lista admin | `ListaDoAdmin` lê `cadastro_realidade.lista` e `dias_na_lista`, `vigia_fio.py:212`; não escreve fonte | `dias_na_lista` lê assinatura com `data` e `dono_do_dia`, `cadastro_realidade.py:1003`; `dia_do_admin` lê dono em `vigia_fio.py:228` |

Todos os paths da tabela são relativos a `app/chamados/`, exceto `cadastro_realidade.py`, que está em `app/escala/services/`. (`app/chamados/services/vigia_fio.py:212`)

O dono do fio morto não se deduz da classe da porta: a fonte declara explicitamente que isso criaria outro juiz L-099. Os dias que já aparecem na lista admin saem somente se o dono lavrado for BATIDA ou CADASTRO; ESTRUTURA continua no alarme. `dias_na_lista` usa apenas assinaturas que nomeiam o dia e resolve donos concorrentes dando precedência aos lavrados pelo juiz; conflito não resolvido retorna `None`. (`app/chamados/services/vigia_fio.py:99`, `app/chamados/services/vigia_fio.py:292`, `app/escala/services/cadastro_realidade.py:978`, `app/escala/services/cadastro_realidade.py:989`)

Furos relevantes:

1. `pergunta_viva` engole erro em autoridade de falta e em materialização; no último degrau devolve `False`. Para a sonda, verificar delta de `core.observ.engolidos()['total']`; qualquer incremento exige `NAO SEI`. (`app/chamados/juizes.py:1204`, `app/chamados/juizes.py:1215`, `app/core/observ.py:60`, `app/core/observ.py:73`)
2. `ListaDoAdmin` engole erro de leitura; o delta também detecta esse caso, e `lista.sem_resposta` deve impedir resultado 0. (`app/chamados/services/vigia_fio.py:219`, `app/chamados/services/vigia_fio.py:225`, `app/chamados/services/vigia_fio.py:253`)
3. O comando `vigiar_fio_morto` escreve carimbo `logs/fio_morto.json`, portanto chamar o comando viola somente leitura; usar funções, não `call_command`. (`app/chamados/management/commands/vigiar_fio_morto.py:99`, `app/chamados/management/commands/vigiar_fio_morto.py:210`)
4. A leitura de vitalidade pode alcançar `get_user_sistema`, que usa `get_or_create`; transação PostgreSQL `READ ONLY` é necessária para impedir criação acidental. (`app/chamados/juizes.py:1069`, `app/core/utils.py:18`)
5. O alarme existente não inclui `fio_morto_alcance`, apesar do esperado 0 publicado. (`app/chamados/management/commands/vigiar_fio_morto.py:82`, `app/chamados/management/commands/vigiar_fio_morto.py:93`)

Bloco de diagnóstico para incorporar ao wrapper da sonda com PostgreSQL READ ONLY, snapshot, captura de engolidos e saída/log suprimidos durante a medida:

```python
import sys
from chamados.juizes import tranca_em_lote
from chamados.services.vigia_fio import ListaDoAdmin, censo, censo_alcance
from escala.models import CelulaDia

lista = ListaDoAdmin(hoje, agora=agora)
with tranca_em_lote():
    d = censo(hoje=hoje, limite=sys.maxsize, lista=lista)
    a = censo_alcance(hoje=hoje, limite=sys.maxsize, lista=lista)

if lista.sem_resposta:
    raise RuntimeError()
rows = list(CelulaDia.objects.filter(
    pk__in=d['celulas_do_fio_morto']
).values_list('colaborador_id', 'data', 'pk'))
if len(rows) != d['fio_morto']:
    raise RuntimeError()

# O contador literal 5 permanece desconhecido; não confundir os diagnósticos.
saida = [
    ('5_LITERAL', 'NAO SEI'),
    ('5_DIAG_FIO_MORTO', {
        'contagem': d['fio_morto'],
        'chamados_vivos_com_celula': d['chamados_vivos_com_celula'],
        'lampadas_acesas': d['lampadas_acesas'],
        'cego': d['cego'],
        'casos': [
            {'chave': 'col%d/%s' % (cid, dia), 'celula': pk}
            for cid, dia, pk in sorted(rows)
        ],
    }),
    ('5_DIAG_ALCANCE', {
        'contagem': a['fio_morto_alcance'],
        'universo': a['alcance_universo'],
        'cego': a['alcance_cego'],
        'casos': [
            {'chave': 'col%d/%s' % (cid, dia), 'celula': cel,
             'competencia': comp, 'rota': rota, 'causa': causa}
            for cel, cid, dia, comp, rota, causa, detalhe, papel
            in a['alcance_dias'] if rota == 'sem_porta'
        ],
    }),
]
```

Os limites e chaves do bloco vêm dos retornos existentes; não replica a regra de vitalidade nem de alcance. (`app/chamados/services/vigia_fio.py:322`, `app/chamados/services/vigia_fio.py:368`, `app/chamados/services/vigia_fio.py:548`)

## 06/11 20:51 · sessao ef62f149478b · - · - · origem=Ronald (tmux)

Contador 2: não encontrei função de medida pronta para “POST de tela >30 s em 24h” [nao verificado]. Fonte existente: access logs Gunicorn, com método `%m`, caminho `%U`, status `%s` e duração `%L` segundos; arquivos distintos `/app/logs/access-ui.log` e `/app/logs/access-core.log`, mais stdout (`app/gunicorn.conf.py:42,48-49,83-100`; `app/gunicorn.core.conf.py:34,40-41,75-96`). A rotação enumera ambos e retém sete dias (`app/core/management/commands/rotacionar_access_log.py:31-32,50-82`). Timeout configurado 30 s não constitui medida de POST >30 s (`app/gunicorn.conf.py:21`; `app/gunicorn.core.conf.py:5`).

Padrão proposto, ancorado à linha inteira, baseado no formato do escritor:

```python
ACCESS = re.compile(
    r"^(?P<ts>\d{4}-\d\d-\d\d \d\d:\d\d:\d\d) "
    r"(?P<peer>\S+) ip=(?P<ip>.*?) colab=(?P<colab>-|u?\d+) "
    r"cli=(?P<cli>.*?) (?P<verb>[A-Z]+) (?P<route>/\S*) -> "
    r"(?P<status>\d{3}) (?P<seconds>\d+(?:\.\d+)?)s ua=(?P<ua>.*)$"
)
```

Filtrar timestamp por `[agora−24h, agora)`, `verb=="POST"`, `float(seconds)>30`, enumerar cada linha por casca/colNNN/instante/rota; contar eventos, não rotas distintas. O padrão exige sequência completa emitida pelo Gunicorn, não apenas palavra POST ou “30s”, portanto prosa solta não casa (`app/gunicorn.conf.py:48-49,83-84`). UI versus API não se deduz da casca: `/api/ponto/*` vai ao core, demais rotas ao UI, enquanto a tela V2 envia diretamente a `/api/ponto/bater/` (`Caddyfile:8-10`; `app/templates/ponto/bater_ponto_v2.html:335-338`). A definição exata de “POST de tela” precisa universo de rotas/canais; `cli` pode estar ausente (`app/core/middleware.py:117-119`). Acesso terminado abruptamente sem access record não está demonstrado [nao verificado].

Contador 3: existe `bin/selo_paginas_500.sh:13-19`, porém mede `50[0-9]`, janela default cinco minutos, via grep não ancorado; não exige Traceback e `|| true` permite falha de leitura virar zero. Não é autoridade suficiente para o contrato. Existe ainda `bin/importerror_na_janela.sh:27-49`, mas só ImportError; linha 27 devolve explicitamente zero se log não existir, violando contrato fornecido.

Fonte de traceback: `django.request` escreve `/app/logs/errors.log`, backups `.1..3`, e console, com formato `%(asctime)s %(levelname)s %(name)s %(message)s` (`app/config/settings/saas.py:122,131-142,173-174`). `propagate=False` evita duplicação nessa configuração (`app/config/settings/saas.py:170-174`). O arquivo é compartilhado e seu formato não contém casca, usuário ou request ID (`app/config/settings/saas.py:122,139`); ler `docker logs --since ... --until ...` de **cada casca**, mantendo origem, é a alternativa existente para preservar casca (`bin/selo_paginas_500.sh:15-16`). Para não contar boot ou WARNING com traceback, usar registro iniciado por:

```python
ERROR500 = re.compile(
    r"^(?P<ts>\d{4}-\d\d-\d\d \d\d:\d\d:\d\d,\d{3}) "
    r"ERROR django\.request Internal Server Error: (?P<route>/\S*)$"
)
TRACEBACK = re.compile(r"^Traceback \(most recent call last\):$")
```

Exigir `TRACEBACK` no mesmo registro multilinha, antes do próximo cabeçalho de log. Esse formato se apoia no formatter (`app/config/settings/saas.py:122`); ocorrência concreta do texto Django emitido não foi fornecida [nao verificado]. Também casar access record com `status=="500"` usando `ACCESS`; não somar `.errors.log` e console, pois são dois destinos do mesmo logger (`app/config/settings/saas.py:173`). Não há correlação inequívoca de traceback com colaborador/administrador: erro não tem ID e access tem somente colab ou `u<ID>`, sem request ID (`app/core/middleware.py:114-119,150-162`; `app/gunicorn.conf.py:48-49`). Associação por hora/rota pode ser ambígua sob concorrência [nao verificado]. Cabe declarar NAO SEI para vínculo ambíguo, não excluir silenciosamente.

Contador 4 integral: **não existe fonte DB completa de tentativas recusadas e entrega de saída**. Provas concretas:

- API (ambos kernels): `views_core.api_bater_ponto` recusa colaborador ausente 404, situação 403 e tipo inválido 400, sem trilha nessas ramificações (`app/api/views_core.py:387-426`). Só GPS ausente grava `LogAuditoria`, ação `batida_recusada_sem_gps`, `modelo="Colaborador"`, `objeto_id=colab.pk`, `valor_depois` com tipo, latitude/longitude recebida, device_id, from_mock, timestamp do dispositivo, depois devolve 403 com instrução (`app/api/views_core.py:440-458`; gêmeo `app/api/views.py:449-467`).
- V2 pode recusar **antes do POST**, por câmera ou GPS, somente mostrando aviso e retornando false (`app/templates/ponto/bater_ponto_v2.html:285-301,325`). Não há correspondente banco nesse caminho.
- Escritor real `registrar_batida` pode devolver `(None, conflito)` sem criar Batida quando oposta colide (`app/ponto/registro_batida.py:138-151`). API converte em HTTP 200 `status=ignorada_intervalo`, `sucesso=True`, mensagem; sem trilha de tentativa (`app/api/views_core.py:695-718`). A V2 trata todo `r.ok` como sucesso sem ler esse status (`app/templates/ponto/bater_ponto_v2.html:340`).
- Tela antiga e V2 usam mesma view com parâmetro `ui_v2` (`app/ponto/urls.py:11-12`; `app/ponto/views.py:242,346-358`). Ramificação antiga devolve 200 `ignorada_intervalo`, mensagem “aguarde”, sem trilha (`app/ponto/services/triagem_batida.py:707-732`).
- Fila offline persiste no IndexedDB do navegador, fora do banco Django; remove definitivamente 400 com somente `console.warn`, sem feedback durável para colab no trecho (`app/static/pwa.js:107-124,126-175`). Portanto banco não cobre também esse universo.
- Middleware acrescenta `mensagem` às recusas JSON com `erro/detail/message/msg`, mas não persiste entrega/leitura (`app/core/middleware.py:19-46`).
- Auditoria é best effort, logo até GPS recusado pode existir sem linha: `registrar_log` delega a `evento`; `evento` engole falha e retorna None (`app/core/auditoria.py:17-29`; `app/core/observ.py:94-105,133-139`).

Modelo e leitores da parte observável: `core.LogAuditoria` define `acao`, `modelo`, `objeto_id`, `valor_depois`, `criado_em` (`app/core/models.py:285-307`); escritor único `evento` mapeia argumentos para esses campos (`app/core/observ.py:134-137`); escritor de payload GPS é view citada acima. Leitor genérico da tela auditoria filtra ação/modelo/data e enumera logs, limitado a 500 linhas (`app/core/views.py:686-738`). Não encontrei leitor específico da ação GPS fora dos testes [nao verificado]. Nunca imprimir `descricao` ou JSON inteiro: escritor GPS inclui nome na descrição e identificador de aparelho no JSON (`app/api/views_core.py:444-451`).

Sonda parcial C4, para incorporar na sonda principal já protegida por tenant/transação readonly:

```python
def sonda_4_parcial(agora):
    from datetime import timedelta
    from django.utils import timezone
    from core.models import LogAuditoria
    try:
        linhas = []
        qs = LogAuditoria.objects.filter(
            acao="batida_recusada_sem_gps",
            modelo="Colaborador",
            criado_em__gte=agora-timedelta(hours=24),
            criado_em__lt=agora,
        ).values_list("pk", "objeto_id", "criado_em")
        for pk, col, instante in qs:
            if col is None:
                raise ValueError("chave ausente")
            linhas.append((f"col{int(col):03d}",
                           timezone.localdate(instante).isoformat(),
                           pk))
        print("C4 NAO SEI")  # universo integral não gravado
        print("C4_GPS_COM_TRILHA", len(linhas))
        for col, dia, pk in sorted(linhas):
            print(col, dia, "auditoria", pk)
    except Exception:
        print("C4 NAO SEI")
```

Isso é inventário de recusas GPS **com** trilha, não contagem de recusas sem trilha/saída; zero desse inventário não autoriza zero do contador (`app/api/views_core.py:402-426,440-458`; `app/core/observ.py:105,139`).

Dono L-099: `InsumosDoDono(...).dono(data)` é montagem real para divergência de dia (`app/ponto/management/commands/e6_oraculo.py:113-125,128-183`). Ela classifica ausência de vínculo/folga indefinida como CADASTRO; batida solta ou paridade ímpar como BATIDA; fase deslocada como CADASTRO; resto depende Cadastro x Realidade (`app/ponto/management/commands/e6_oraculo.py:98-110`). **Não aplicar à falha de instrumentação/timeout/500/recusa sem trilha** para decidir causa: o juiz responde divergência diária, não esse domínio (`app/ponto/management/commands/e6_oraculo.py:113-125`). Defeitos demonstrados de software acima são ESTRUTURA; causa específica de eventual evento não fornecido [nao verificado]. Status férias/desligado ou GPS recusado sozinho não prova CADASTRO/BATIDA incorreto; sem caso concreto, dono [nao verificado].

## 06/11 20:50 · sessao af2d47c29f73 · - · - · origem=Ronald (tmux)

**07 — col836: classe principal (e) para “caso resolvido”; (b) na promoção da ressalva à tabela.** A pergunta tratava da visibilidade, e a resposta limitou expressamente seu “OK” a esse aspecto. A falta de critério de resolução operacional impede atribuir a t1 uma obrigação de comprovar ação posterior, DiaPago e validação que só aparece em v2.r. A resposta, contudo, viu a pendência e a separou da classificação consolidada. [nao verificado]

- **t1.q, exato:** `07 chamado de 14/10 existe e nao aparece "na lista": qual lista de chamados do admin, qual filtro dela exclui o chamado da FICHA 07 (status, superado_por, silenciado, celula, empresa visivel)?` [nao verificado]
- **t1.r, nascimento do rótulo, exato:** `**07 — col836, 14/10: ja OK hoje quanto à exclusão da lista inicial.**` [nao verificado]
- **t1.r, ressalva vista, exato:** `A pendência de escala indicada na célula é separada da visibilidade desse chamado resolvido.` [nao verificado]
- **Regra preventiva:** “Separe resposta da subpergunta de estado do caso: explicar por que um chamado saiu da lista não autoriza ‘ja OK hoje’. Havendo pendência da célula, mantenha o item aberto e descreva-a; só declare resolução operacional com o critério explícito requerido.” [nao verificado]
- **O código confirma a explicação estreita de t1**, pois o padrão da lista é cobrança e o histórico inclui resolvidos/fechados; isso não comprova que o dia foi saneado. `app/chamados/services/painel.py:298`, `app/chamados/services/painel.py:300`, `app/chamados/services/painel.py:314`.
- **O mecanismo específico revelado em v2**, perguntas já carimbadas que não replantam, é comprovável na cópia; sua presença histórica nesses registros é somente o anexo. `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:422`; estado histórico [nao verificado]. **Não atribuir a t1 “não viu essa cronologia”**: o conteúdo integral da FICHA t1 não foi fornecido. [nao verificado]

**08 — col617: classe principal (e); (c) para provar a história ou resultado ausentes.** A pergunta pediu apuração do suposto aceite sem admin; t1 respondeu a isso e explicitou que faltavam decisor e data correta. Nem v2 demonstra autoaceite indevido ou atestado atualmente pendente; o falso “resolvido” nasce da inexistência de um critério amplo no comando e da promoção de uma refutação parcial ao rótulo global. [nao verificado]

- **t1.q, exato:** `08 2o atestado com data errada aparece no espelho SEM aceite: as Ausencias da FICHA 08 -- status, quem registrou/aprovou (proprio colab?), e qual regra deixa uma declarada pelo colab valer sem passo do admin (corte 18:07 do ESPELHO-DIZ-QUEM-DECIDIU: falta declarada pelo colab segue aprovada sem passo do admin; atestado tambem?).` [nao verificado]
- **t1.r, exato:** `**08 — col617, 05–06/11: ja OK hoje quanto à alegação de autoaceite.**` [nao verificado]
- **t1.r, ressalva exata:** `o dado não sustenta aprovação pelo próprio colaborador; tampouco mostra a data correta do documento ou a identidade do decisor.` [nao verificado]
- **Regra preventiva:** “Não sustentar a alegação de autoaceite não significa caso resolvido. Só atribua ‘ja OK’ após confirmar período correto, decisão autorizada e efeitos requeridos; sem esses dados, ‘falta informacao’.” [nao verificado]
- **Na cópia, documento anexo nasce aguardando decisão**, coerente com a explicação estreita de t1; isso não atesta o percurso histórico da ausência. `app/ponto/services/ausencia.py:779`, `app/ponto/services/ausencia.py:791`; histórico [nao verificado].

**09 — col1000: classe principal (b).** Aqui a própria resposta identificou os dias pendentes, mas consolidou “ja OK” escolhendo outros dias. Não é “nenhum viu”: t1 viu expressamente o contrário do fechamento amplo. A tela original e seu decisor continuaram sem identificação, portanto essa parte não foi respondida. [nao verificado]

- **t1.q, exato:** `09 batidas faltando, sem chamado, e a tela mostra "resolvido": que tela/status diz resolvido nos dias da FICHA 09 sem batida, e quem decidiu.` [nao verificado]
- **t1.r, nascimento do rótulo, exato:** `**09 — col1000: ja OK hoje para os dias sem batida apresentados.**` [nao verificado]
- **t1.r, evidência já vista e descartada no rótulo, exato:** `A alegação não descreve integralmente o estado atual: **27/10 tem perguntas pendentes; 29 e 30/10 têm chamados próprios em análise**, criados hoje. A tela exata originalmente chamada de “resolvido” permanece sem identificação.` [nao verificado]
- **Regra preventiva:** “Avalie todos os dias da queixa/FICHA; exemplos de dias justificadamente sem batida não encerram dias com furos/perguntas pendentes. Sem identificar a tela/status e o decisor pedidos, não use ‘ja OK’.” [nao verificado]
- **Na cópia, validação sem resposta retorna `sem_resposta`**, e fechar chamado de furo exige premissa regularizada; status do chamado não substitui essa regularização. `app/chamados/services/validacao.py:363`, `app/chamados/models.py:175`, `app/chamados/models.py:185`.

**Ressalva de consolidação:** se for exigida uma única letra por linha, sugiro **07=(e), 08=(e), 09=(b)**, explicando em 07 que t1 viu a pendência e não pode promover o “OK da visibilidade” a resolução completa. Não há base para cravar (d) nesses três casos: t1 explicitou pendências ou lacunas, e falta a íntegra das FICHAS antigas para demonstrar que nenhuma das etapas viu o mecanismo novo. [nao verificado]

## 06/11 20:50 · sessao cfdd0b3e4aa5 · - · - · origem=Ronald (tmux)

**11 — col921. Classe principal recomendada: (b), no salto de diagnóstico local para “já OK”; lacuna adicional (e) para o critério operacional de resolução.** A resposta já registra dias irregulares, mas a tabela os absorve em “ja OK hoje”; a exigência posterior de ação na tela + espelho + DiaPago + validação operacional não constava de t1.q. [nao verificado]

- **t1.q, trecho exato:** `11 "Resolver dia" some depois de 20/10: resolver_dia na FICHA 11+12 por data; e regra da janela/competencia (21-20) ou defeito?` [nao verificado]
- **t1.r, onde nasce o fechamento:** `**11 — col921: ja OK hoje; regra de competência.**` [nao verificado]
- **t1.r, evidência que ela própria viu:** `` `resolver_dia=true` em **14, 15, 16, 18, 19 e 20/10**; falso nas folgas **17 e 24/10**, e também nos dias **21–23 e 25–28/10**, ainda irregulares. `` [nao verificado]
- **t1.r, conclusão local:** `A mudança em 21/10 é compatível com a competência 21–20 apresentada na FICHA, não demonstra defeito.` [nao verificado]
- **Limite:** v2.r não refuta essa explicação do botão: o predicado exige último recurso, passado e status permitido; último recurso depende de data anterior à competência atual. A regra de competência correta não prova que os dias irregulares tenham sido saneados. (`app/colaboradores/services/calendario.py:94`, `:103`, `:455`, `:738`; saneamento individual [nao verificado].)
- **Regra preventiva a acrescentar:** “Responder separadamente: funcionamento do botão e pendência do caso. Regra de competência correta não autoriza ‘ja OK’ enquanto houver dias irregulares sem encaminhamento comprovado; declarar qual evidência falta para fechar.” [nao verificado]

**12 — col921. Classe principal recomendada: (b).** A segunda metade da pergunta pediu qual lançamento atende à situação; a resposta reconheceu que não demonstrou essa porta e, mesmo assim, classificou o item inteiro como “ja OK”. [nao verificado]

- **t1.q, trecho exato:** `12 falta em folga recusada: confirme a regra no codigo; qual lancamento cobre abandono de posto em dia de folga/escala?` [nao verificado]
- **t1.r, onde nasce o fechamento:** `**12 — col921: ja OK hoje quanto à recusa de falta em folga.**` [nao verificado]
- **t1.r, lacuna expressamente reconhecida:** `**Não há lançamento específico de “abandono de posto em folga” demonstrado neste caso.**` [nao verificado]
- **t1.r, complemento exato:** `O comando de abandono provisório usa o próprio tipo `falta`; o modelo de Advertência tem “Ausência/Outro”, mas isso não comprova uma porta de tela para o lançamento pretendido nem autoriza tratar folga como falta.` [nao verificado]
- **Limite:** o veto existe quando a autoridade da escala responde que não há jornada prevista, e sua mensagem manda corrigir a escala se havia trabalho devido. O comando usa `tipo='falta'`; a existência de opções no modelo Advertência não demonstra uma porta operacional adequada. (`app/ponto/precedencia.py:536`, `:538`, `:541`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`, `:79`; `app/ponto/models.py:1164`; adequação ao caso [nao verificado].)
- **Regra preventiva a acrescentar:** “Item com duas perguntas só pode receber ‘ja OK’ se ambas forem respondidas com evidência. Confirmar o veto não fecha a demanda de lançamento: identificar a porta compatível com o fato e a escala, ou manter ‘falta informacao’.” [nao verificado]

**Ressalva para a consolidação:** se “erro” for definido exclusivamente como não satisfazer o critério operacional introduzido em v2, 11 e 12 são também (e), pois esse critério não estava formulado em t1.q. Evitar classificar como (a) uma obrigação retrospectiva de conferir DiaPago/validação operacional que não foi pedida; o erro já demonstrável em t1 é o fechamento amplo apesar das ressalvas que a própria resposta registrou. [nao verificado]

## 06/11 20:50 · sessao 97eeece680d1 · - · - · origem=Ronald (tmux)

**02 — ID col993 → col78: classe (e), NAO PERGUNTADO.** A troca é informada por v2.r; a comprovação humana do vínculo não foi anexada. [nao verificado]

- **t1.q, regra que autorizou a escolha:** “diga qual candidato colNNN e o caso (o de maior atividade na janela, salvo evidencia contraria) e o que os dados mostram;” [nao verificado]
- **t1.q, item:** “02 correcao de horario do 02a nao chega (a do 02b chegou): compare os dois caminhos (Justificativa / RegularizacaoExterna / PerguntaDisputa) e onde o do 02a parou.” [nao verificado]
- **t1.r, nascimento do ID errado:** “**02 — col993 comparado com col1023: falta informacao.** Mantive col993, atividade 102 contra 91 e 19: a FICHA mostra várias correções materializadas em 03/11, com origem `disputa_s84_retro`.” [nao verificado]
- **Regra preventiva proposta:** “Antes de analisar, exija vínculo do item com colNNN confirmado por fonte humana mediante col+nome completo; atividade serve somente à investigação, sem decidir identidade. Sem confirmação, marque falta informacao.” [nao verificado]

**05 — ID col155 → col456: classe (e), NAO PERGUNTADO.** A troca é informada por v2.r; o diagnóstico genérico da barreira de GPS não confirma a identidade do caso. [nao verificado]

- **t1.q, regra que autorizou a escolha:** “diga qual candidato colNNN e o caso (o de maior atividade na janela, salvo evidencia contraria) e o que os dados mostram;” [nao verificado]
- **t1.q, item:** “05 colab diz que o GPS nao aciona e nao consegue bater: o app (static/js do PWA e o fluxo da API /api/ponto) RECUSA ou bloqueia a batida sem geolocalizacao? Se sim, isso fere A Lei (CLAUDE.md secao 4: batida de chao nunca e barrada). Veja as batidas e devices da FICHA 05.” [nao verificado]
- **t1.r, nascimento do ID errado:** “**05 — col155: nosso com obra, dono ESTRUTURA; sem obra específica localizada.** É o maior candidato, atividade 23 contra 15; tem dois devices ativos e as marcações apresentadas chegaram com geolocalização.” [nao verificado]
- **Regra preventiva proposta:** a mesma confirmação humana de identidade do item 02, acrescida de: “Um bloqueio encontrado no código não identifica qual candidato fez a tentativa relatada.” [nao verificado]

**Limite da imputação:** t1.r segue expressamente a heurística pedida. Como as linhas originais de CANDIDATOS/FICHA não foram reproduzidas, não se pode afirmar que existia evidência contrária disponível e ignorada; atribuir (a) aos erros de identidade excederia o dado. [nao verificado]

_2556 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
