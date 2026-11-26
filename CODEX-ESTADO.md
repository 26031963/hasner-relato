# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 18:23** · publicado em **26/11/2026 18:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 18:23 · sessao 99bdbf42b5a2 · PAUSA-INTERNA-NO-PARTIDO · leitura · origem=Code

**1. A fábrica usada pelo fechamento é `core.regua_cct.get_motor_cct`.** `recalcular_fechamento_mes` chama a fábrica em `_montar_motor`, uma vez por fatia, com o colaborador, o primeiro dia da fatia e `esc.tipo_escala`. A fábrica delega a escolha da classe a `ponto.motor_calculo_v2.get_motor`. [app/ponto/services/fechamento.py:299](app/ponto/services/fechamento.py:299) [app/ponto/services/fechamento.py:744](app/ponto/services/fechamento.py:744) [app/ponto/services/fechamento.py:754](app/ponto/services/fechamento.py:754) [app/core/regua_cct.py:428](app/core/regua_cct.py:428) [app/core/regua_cct.py:457](app/core/regua_cct.py:457) [app/core/regua_cct.py:479](app/core/regua_cct.py:479)

**Sim, pode devolver `MotorTurnoPartido`.** O cadastro é o `TipoEscala` do `EscalaColaborador` da fatia. A base `turno_partido` aponta para `MotorTurnoPartido`. A chave legada `turno_partido_noturno` aponta para a subclasse `MotorTurnoPartidoNoturno`. O ciclo `intermitente` tem prioridade. Quando o template Partido tem início e fim, a fábrica exige um vão cadastrado superior a 120 minutos. Se o vão for menor, igual a 120 ou faltar um dos marcos do intervalo, a fábrica escolhe `Motor12x36ComEscala` se o turno cruzar a meia-noite, ou `MotorComercial` nos demais casos. Se faltar o início ou o fim do turno, vale o mapeamento da base. [app/escala/models.py:50](app/escala/models.py:50) [app/escala/models.py:874](app/escala/models.py:874) [app/ponto/motor_calculo_v2.py:3677](app/ponto/motor_calculo_v2.py:3677) [app/ponto/motor_calculo_v2.py:3710](app/ponto/motor_calculo_v2.py:3710) [app/ponto/motor_calculo_v2.py:3764](app/ponto/motor_calculo_v2.py:3764) [app/ponto/motor_calculo_v2.py:3804](app/ponto/motor_calculo_v2.py:3804) [app/escala/servico_jornada.py:104](app/escala/servico_jornada.py:104) [app/escala/servico_jornada.py:152](app/escala/servico_jornada.py:152)

**2. A sonda conta cada colaborador uma vez por classe.** Quem troca de classe na janela pode aparecer em mais de uma linha `MOTOR`. A contagem prévia usa o recorte de fatias do fechamento e inclui a chamada para dias com batida fora das fatias quando o fechamento a faria. Essa chamada passa `tipo_escala=None` e recebe `Motor12x36`. A sonda conta o Partido noturno na própria classe e o inclui no universo Partido. [app/ponto/services/fechamento.py:225](app/ponto/services/fechamento.py:225) [app/ponto/services/fechamento.py:283](app/ponto/services/fechamento.py:283) [app/ponto/services/fechamento.py:812](app/ponto/services/fechamento.py:812) [app/ponto/services/fechamento.py:837](app/ponto/services/fechamento.py:837) [app/ponto/motor_calculo_v2.py:3757](app/ponto/motor_calculo_v2.py:3757) [app/ponto/motor_calculo_v2.py:3677](app/ponto/motor_calculo_v2.py:3677)

No recálculo, a sonda intercepta a fábrica e observa `calcular_periodo` na instância Partido que ela devolveu. `UNIVERSO dias` conta pares únicos de colaborador e dia. `TOTAL` soma os intervalos recebidos por `calcular_periodo`, antes do recorte que o motor faz na janela efetiva. A saída anterior trouxe `UNIVERSO VAZIO`. A causa daquela saída e os totais da competência 11/2026 seguem [nao verificado]. `sombra.out:4` [app/ponto/services/fechamento.py:287](app/ponto/services/fechamento.py:287) [app/ponto/motor_calculo_v2.py:2571](app/ponto/motor_calculo_v2.py:2571) [app/ponto/motor_calculo_v2.py:2666](app/ponto/motor_calculo_v2.py:2666) [app/ponto/motor_calculo_v2.py:3007](app/ponto/motor_calculo_v2.py:3007)

Para propagar o código de falha até o shell chamador, use `bash -e bin/sombra.sh --rodar pausa_interna_partido_c11_escolha.py`. A chamada direta pode devolver o status do `echo` final. A transação `READ ONLY` é da sonda. O wrapper repõe a lavra no banco da sombra antes de iniciar o Python. [bin/sombra.sh:45](bin/sombra.sh:45) [bin/sombra.sh:157](bin/sombra.sh:157) [bin/sombra.sh:483](bin/sombra.sh:483)

### SCRIPT

```python
import logging
import sys
from collections import defaultdict
from datetime import date, datetime, time, timedelta
from traceback import extract_tb
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone

from core import regua_cct
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo
from ponto.janelas import janela_fechamento, periodo_apuracao
from ponto.motor_calculo_v2 import MOTOR_POR_BASE, MotorBase, MotorTurnoPartido, ResultadoMes
from ponto.services import fechamento
from ponto.turnos import batidas_apuraveis, data_local

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
INI, FIM = date(2026, 10, 21), date(2026, 11, 20)
UM_DIA = timedelta(days=1)
UM_MINUTO = timedelta(minutes=1)


class ReverterSonda(Exception):
    pass


class FalhaSonda(BaseException):
    pass


def interceptar_log(_logger, registro):
    # app/ponto/services/fechamento.py:1327-1332 e app/core/regua_cct.py:442-448.
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaSonda(f"{registro.pathname}:{registro.lineno} {tipo}") from None


def contar_escolhas():
    previstas = defaultdict(set)
    env_ini = periodo_apuracao(MES, ANO, 2)[0]
    env_fim = periodo_apuracao(MES, ANO, 28)[1]
    busca_ini = timezone.make_aware(datetime.combine(INI, time.min)) - UM_DIA
    busca_fim = timezone.make_aware(datetime.combine(FIM, time(23, 59, 59))) + UM_DIA
    for empresa_id in EMPRESAS:
        if janela_fechamento(MES, ANO, empresa_id) != (INI, FIM):
            raise FalhaSonda("app/ponto/janelas.py:54 janela divergente")

        # app/ponto/services/fechamento.py:574,603-626,639-653.
        colabs = list(fechamento.universo_da_competencia(MES, ANO, empresa_id)
                      .select_related("posto__praca", "empresa"))
        celulas_por_colab = defaultdict(dict)
        for (cid, dia), celula in celulas_do_periodo(
            [colab.pk for colab in colabs], env_ini, env_fim
        ).items():
            celulas_por_colab[cid][(cid, dia)] = celula

        for colab in colabs:
            cid = colab.pk
            celulas = celulas_por_colab[cid]

            # app/ponto/services/fechamento.py:673-678.
            geradoras = {
                celula.escala_geradora_id
                for (_, dia), celula in celulas.items()
                if INI <= dia <= FIM and celula.escala_geradora_id
            }
            escalas = sorted(
                escalas_do_periodo(cid, INI, FIM, mais=geradoras),
                key=lambda esc: esc.data_inicio,
            )

            def montar(esc, fatia_ini, _fatia_fim, *, colab=colab):
                # A classe depende de tipo_escala. app/ponto/motor_calculo_v2.py:3754-3812.
                motor = regua_cct.get_motor_cct(colab, fatia_ini, tipo_escala=esc.tipo_escala)
                previstas[type(motor)].add(colab.pk)

                # O resultado vazio permite percorrer as fatias sem calcular a folha.
                # app/ponto/services/fechamento.py:299-335.
                motor.calcular_mes = lambda *args, **kwargs: ResultadoMes(
                    colaborador_id=kwargs["colaborador_id"]
                )
                return motor

            # As fatias são selecionadas por vínculo e célula.
            # app/ponto/services/fechamento.py:208-289.
            apuracao = fechamento.apurar_por_fatia(colab, INI, FIM, (), escalas, celulas, montar)

            # app/ponto/services/fechamento.py:812-840.
            if escalas and len(apuracao.dias_cobertos) < (FIM - INI).days + 1:
                dias_orfaos = set()
                batidas = batidas_apuraveis(colab, busca_ini, busca_fim)
                for ts in batidas.values_list("timestamp", flat=True):
                    dia = data_local(ts)
                    if dia is not None and INI <= dia <= FIM and dia not in apuracao.dias_cobertos:
                        dias_orfaos.add(dia)
                if dias_orfaos:
                    motor = regua_cct.get_motor_cct(colab, min(dias_orfaos), tipo_escala=None)
                    previstas[type(motor)].add(cid)
    return previstas


def recalcular(previstas):
    fabrica = regua_cct.get_motor_cct
    codigo_fatia = fechamento.apurar_por_fatia.__code__
    codigo_recalculo = fechamento.recalcular_fechamento_mes.__code__
    codigo_mes_base = MotorBase.calcular_mes.__code__
    observadas = defaultdict(set)
    universo = set()
    pausas = defaultdict(timedelta)

    def observar_fabrica(colab, competencia, **kwargs):
        motor = fabrica(colab, competencia, **kwargs)
        origem = sys._getframe(1)
        fatia = origem.f_back

        # Imports locais em app/ponto/services/fechamento.py:755-761 e :837-840.
        e_fatia = (
            origem.f_code.co_name == "_montar_motor"
            and fatia is not None
            and fatia.f_code is codigo_fatia
            and fatia.f_back is not None
            and fatia.f_back.f_code is codigo_recalculo
        )
        if not (e_fatia or origem.f_code is codigo_recalculo):
            return motor

        cid = colab.pk
        observadas[type(motor)].add(cid)
        if isinstance(motor, MotorTurnoPartido):
            if not e_fatia:
                raise FalhaSonda("app/ponto/services/fechamento.py:838 Partido fora da fatia")
            universo.update((cid, dia) for dia in fatia.f_locals["_dias_da_fatia"])
            calcular_periodo = motor.calcular_periodo

            def observar_periodo(entrada, saida, intervalos=()):
                origem_periodo = sys._getframe(1)
                dados = origem_periodo.f_locals
                if origem_periodo.f_code is not codigo_mes_base or dados["colaborador_id"] != cid:
                    raise FalhaSonda(
                        "app/ponto/motor_calculo_v2.py:2378 origem de periodo divergente"
                    )

                # Dia da jornada regular ou do crédito de aberto.
                # app/ponto/motor_calculo_v2.py:2571-2573 e :2632-2667.
                dia = dados["_d_a"] if "_pares_a" in dados else dados["data_jornada"]
                if not INI <= dia <= FIM:
                    raise FalhaSonda("app/ponto/motor_calculo_v2.py:2542 dia fora da janela")
                chave = (cid, dia)
                universo.add(chave)
                if intervalos:
                    pausas[chave] += sum((fim - ini for ini, fim in intervalos), timedelta())
                return calcular_periodo(entrada, saida, intervalos)

            motor.calcular_periodo = observar_periodo
        return motor

    with patch.object(regua_cct, "get_motor_cct", observar_fabrica):
        for empresa_id in EMPRESAS:
            # app/ponto/services/fechamento.py:468-471,559-560,1215.
            fechamento.recalcular_fechamento_mes(
                MES, ANO, empresa_id=empresa_id,
                somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False,
            )

    if observadas != previstas or not universo:
        raise FalhaSonda("app/ponto/services/fechamento.py:756 escolhas ou cobertura divergentes")
    return universo, pausas


def em_minutos(duracao):
    return f"{duracao / UM_MINUTO:.9f}".rstrip("0").rstrip(".")


def main():
    try:
        with patch.object(logging.Logger, "handle", interceptar_log), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            previstas = contar_escolhas()
            classes = set(MOTOR_POR_BASE.values()) | previstas.keys()
            for classe in sorted(classes, key=lambda cls: cls.__name__):
                print(
                    f"MOTOR classe={classe.__name__} colabs={len(previstas.get(classe, ()))}",
                    flush=True,
                )

            tem_partido = any(issubclass(classe, MotorTurnoPartido) for classe in previstas)
            if tem_partido:
                universo, pausas = recalcular(previstas)
            raise ReverterSonda()
    except ReverterSonda:
        pass
    except Exception as erro:
        local = extract_tb(erro.__traceback__)[-1]
        raise FalhaSonda(f"{local.filename}:{local.lineno} {type(erro).__name__}") from None

    if not tem_partido:
        print("PARTIDO_ZERO_PELA_ESCOLHA", flush=True)
        return

    for (cid, dia), duracao in sorted(pausas.items()):
        print(f"col{cid:03d} {dia.isoformat()} minutos={em_minutos(duracao)}")
    print(f"UNIVERSO colabs_partido={len({cid for cid, _ in universo})} dias={len(universo)}")
    total = em_minutos(sum(pausas.values(), timedelta()))
    print(f"TOTAL dia_colab={len(pausas)} colabs={len({cid for cid, _ in pausas})} minutos={total}")


main()
```

### FIM

## 26/11 18:21 · sessao b8838c37c92a · ABONO-PARCIAL-TETO · leitura · origem=Code

### SCRIPT

```python
# Sonda ABONO-PARCIAL-TETO para logs/sombra/<script.py>. [bin/sombra.sh:477-492]
# Execução na sombra e valores dos oito alvos [nao verificado].
# O Python sai com rc 1 em universo vazio. Para propagar esse rc ao shell,
# use bash -e bin/sombra.sh --rodar <script.py>. A porta termina com echo
# após a pipeline e ativa pipefail. [bin/sombra.sh:45,483-493]

import logging
import sys
from collections import defaultdict
from datetime import datetime, time
from traceback import extract_tb
from types import SimpleNamespace
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa
from escala.servico_jornada import escala_vigente
from escala.utils import cadeia_do_dia
from folha import export
from ponto.catalogo.ausencias import ABONA, parciais_vigentes
from ponto.models import Ausencia
from ponto.motor_calculo_v2 import _minutos_da_ponta_cobertos
from ponto.services import dia_pago, fechamento
from ponto.services.credito_parcial import abater_no_proprio_dia
from ponto.turnos import cobre_parte_do_dia

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
ALVOS = {327, 242, 298, 288, 222, 885, 827, 258}  # Anexo fornecido [nao verificado].
NV = "[nao verificado]"
REF_TXT = "[app/folha/export.py:215-255,699-755]"
REF_ABONO = "[app/ponto/services/dia_pago.py:285-292,364-402]"
REF_DIA = (
    "[app/escala/services/leitor_celula.py:390-436] "
    "[app/ponto/services/fechamento.py:893-895,1018-1027,1139-1151] "
    "[app/ponto/services/dia_pago.py:173-195,274-292] "
    "[app/folha/export.py:169-185,383-399,449-452,749-755]"
)


class ReverterSonda(Exception):
    pass


class FalhaSonda(RuntimeError):
    pass


def col(cid):
    return f"col{cid:03d}"


def log_sem_dados(_logger, registro):
    # A mensagem original do fechamento pode conter identificação pessoal.
    # [app/ponto/services/fechamento.py:1327-1332]
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaSonda(f"{registro.pathname}:{registro.lineno} {tipo}") from None


def teto_no_previsto(colab, dia, ausencia, previsto, celulas):
    # A cadeia usa o vínculo da célula, todos os blocos líquidos e o dia
    # de início do turno. [app/escala/servico_jornada.py:25-66]
    # [app/escala/models.py:1109-1144,1177-1213] [app/escala/utils.py:65-81,330-363,2345-2374]
    if previsto == 0:
        return 0
    escala = escala_vigente(colab, dia, celulas=celulas)
    if escala is None or escala.tipo_escala is None:
        raise FalhaSonda(f"{col(colab.pk)} {dia} vínculo indeterminado [app/escala/servico_jornada.py:49-66]")
    marcos = escala.marcos_do_dia(dia, celulas=celulas)
    pausas = escala.pausas_do_dia(dia, celulas=celulas)
    if not pausas and marcos[2] and marcos[3]:
        pausas = (marcos[2:],)
    zero = datetime.combine(dia, time.min)
    instantes = [
        (instante.replace(tzinfo=None) - zero).total_seconds() / 60
        for _, instante in cadeia_do_dia(dia, marcos, pausas)
    ]
    blocos = list(zip(instantes[::2], instantes[1::2]))
    if not blocos or sum(fim - ini for ini, fim in blocos) != previsto:
        raise FalhaSonda(
            f"{col(colab.pk)} {dia} geometria diverge do previsto da grade "
            "[app/escala/utils.py:353-363] [app/escala/services/leitor_celula.py:397-423]"
        )
    # O juiz de cobertura é batida_dentro_da_parcial, chamado por
    # _minutos_da_ponta_cobertos. [app/ponto/motor_calculo_v2.py:453-477]
    # [app/ponto/turnos.py:2653-2711]
    return sum(_minutos_da_ponta_cobertos((ausencia,), ini, fim) for ini, fim in blocos)


def ler_txt(conteudo, itens, fechs, entra):
    # Matrícula usada apenas dentro da empresa para associar o registro ao ID.
    # Preserva multiplicidade, valor cru e datas filhas. [app/folha/export.py:215-255,725-753]
    por_matricula = defaultdict(list)
    for item in itens:
        cid = item["colaborador_id"]
        if cid in entra and fechs[cid].status in export.STATUS_NO_TXT:
            por_matricula[item["codigo_dominio"].zfill(10)].append(cid)

    registros = defaultdict(list)
    pai = None
    for linha in conteudo.splitlines():
        parte = export.partes_da_linha(linha)
        if parte is None or (parte["reg"] == "11" and pai is None):
            raise FalhaSonda("Registro inválido [app/folha/export.py:215-255]")
        if parte["reg"] == "10":
            ids = por_matricula[parte["matricula"]]
            if len(ids) != 1:
                raise FalhaSonda("Autoria ambígua do registro 10 [app/folha/export.py:725-748]")
            pai = (parte["valor"], [])
            registros[(ids[0], parte["rubrica"])].append(pai)
        else:
            pai[1].append(parte["data"])
    return {
        chave: tuple(sorted((valor, tuple(sorted(datas))) for valor, datas in linhas))
        for chave, linhas in registros.items()
    }


def medir(empresa, aplicar_teto, cadastradas):
    foto = SimpleNamespace(abonos={}, tetos={}, dias={}, furos={})
    recalcular = fechamento.recalcular_fechamento_mes
    grade_original = export.grade_do_fechamento
    por_dia_original = dia_pago.por_dia_da_grade

    def observar_grade(fech):
        grade = grade_original(fech)
        origem = sys._getframe(1)
        cid = fech.colaborador_id
        if origem.f_code is not recalcular.__code__ or cid not in cadastradas:
            return grade

        # Retém os insumos da mesma passada. Saída usa o dia da jornada da
        # lavratura e atraso usa o crédito do próprio dia.
        # [app/ponto/services/fechamento.py:893-895,1018-1038,1225-1241]
        # [app/ponto/services/dia_pago.py:109-132,173-195,274-277]
        local = origem.f_locals
        resultado = local["resultado"]
        saidas = defaultdict(float)
        for p in resultado.periodos:
            dia = dia_pago._dia_de(p, resultado.dia_da_jornada)
            saidas[dia] += p.minutos_saida_antecipada or 0

        for linha in grade["dias"]:
            dia = linha["data"]
            if dia not in cadastradas[cid]:
                continue
            esperado = linha["tipo_dia"] not in {"sem_celula", "indefinido"}
            atraso, _ = abater_no_proprio_dia(
                {dia: local["_atrasos_dia"].get(dia, 0)}, local["_creditos_dia"]
            )
            foto.dias[cid, dia] = (
                linha["minutos_previstos"] if esperado else None,
                linha["minutos_realizados"] if esperado else None,
                atraso / 60,
                saidas[dia] / 60,
                local["_faltas_por_dia"].get(dia, 0) / 60,
                getattr(local["_cob_folha"].get(dia), "pk", None),
            )
        return grade

    def envolver_por_dia(grade):
        abono, previsto, realizado = por_dia_original(grade)
        origem = sys._getframe(1)
        if origem.f_code is not recalcular.__code__:
            return abono, previsto, realizado

        # B aplica min(abono de A, cobertura do período dentro do previsto)
        # em toda a frota elegível. A cobertura vencedora já veio do fechamento.
        # [app/ponto/services/fechamento.py:700-718,1084-1088]
        # [app/ponto/turnos.py:2622-2639,2816-2839]
        local = origem.f_locals
        colab = local["colab"]
        if aplicar_teto:
            abono = dict(abono)
            for linha in grade["dias"]:
                dia = linha["data"]
                if dia not in abono or not linha.get("cobertura_parcial"):
                    continue
                a = local["_cob_folha"].get(dia)
                if (
                    a is None or a.efeito != ABONA
                    or not cobre_parte_do_dia(a, local["_parciais"])
                    or a.hora_ini is None or a.hora_fim is None or a.hora_ini == a.hora_fim
                ):
                    continue
                teto = teto_no_previsto(colab, dia, a, previsto[dia], local["_cels_lote"])
                foto.tetos[colab.pk, dia] = teto
                abono[dia] = min(abono[dia], teto)

        foto.abonos.update({(colab.pk, dia): valor for dia, valor in abono.items()})
        return abono, previsto, realizado

    # Preserva a resposta esperada _GradeSemCelula e deixa subir as demais
    # exceções capturadas pelo fechamento. [app/ponto/services/fechamento.py:1067-1069,1105-1123,1327-1332]
    with (
        patch.object(fechamento, "Exception", fechamento._GradeSemCelula, create=True),
        patch.object(export, "grade_do_fechamento", observar_grade),
        patch.object(dia_pago, "por_dia_da_grade", envolver_por_dia),
    ):
        fechs = recalcular(
            MES, ANO, empresa_id=empresa.pk, somente_leitura=True,
            retornar_objeto=True, lavrar_dias_pagos=False, coletar_furos=foto.furos,
        )

    # Caminho lido de simular_folha e serialização real da porta do TXT.
    # [app/folha/management/commands/simular_folha.py:124-134] [app/folha/views.py:272-288]
    objetos = list(fechs.values())
    itens = export.classificar_export(empresa, MES, ANO, fechamentos=objetos)
    linhas, foto.entra = export.montar_linhas_export(
        empresa, MES, ANO, itens=itens, fechamentos=objetos,
    )
    if export.validar_layout_dominio(linhas):
        raise FalhaSonda("Layout inválido [app/folha/export.py:266-307]")
    foto.txt = export.conteudo_do_txt(linhas)
    foto.registros = ler_txt(foto.txt, itens, fechs, foto.entra)
    return foto


def campos_do_dia(foto, cid, dia, lado):
    chave = cid, dia
    previsto, realizado, atraso, saida, falta, _ = foto.dias.get(chave, (None,) * 6)
    abono = foto.abonos.get(chave, 0) if previsto is not None else None
    r8069 = foto.registros.get((cid, "8069"), ())
    r8792 = foto.registros.get((cid, "8792"), ())
    campos = {
        "previsto_min": previsto,
        "realizado_min": realizado,
        "abono_min": abono,
        "horas_atraso": atraso,
        "horas_saida_antecipada": saida,
        "horas_falta": falta,
        "horas_base_8069_dia": export.rubrica_atraso_parcial(atraso, saida)
        if atraso is not None else None,
        "8069_txt_comp_valor_cru": tuple(valor for valor, _ in r8069),
        "8792_txt_data": int(any(dia.strftime("%Y%m%d") in datas for _, datas in r8792)),
        "furo_motor": int(dia in foto.furos[cid]) if cid in foto.furos else None,
        "entra_juiz_txt": int(cid in foto.entra),
    }
    return " ".join(f"{campo}_{lado}={NV if valor is None else valor}" for campo, valor in campos.items())


def medir_e_imprimir(agora):
    empresas = list(Empresa.objects.filter(ativa=True, pk__in=EMPRESAS).order_by("pk"))
    if empresas and {e.pk for e in empresas} != set(EMPRESAS):
        raise FalhaSonda("Escopo incompleto de empresas ativas [app/folha/management/commands/simular_folha.py:112-119]")

    medidas = []
    tipos = parciais_vigentes()
    for empresa in empresas:
        janela = export.janela_competencia(empresa, MES, ANO)
        entregue = export.competencia_entregue(empresa.pk, MES, ANO)
        cadastradas = {}
        faltas = {}
        # O cadastro dos oito alimenta o relatório. O wrapper usa a cobertura
        # do fechamento para todos os colaboradores. [app/ponto/turnos.py:2622-2639,2816-2839]
        for a in Ausencia.objects.filter(
            colaborador__empresa=empresa, colaborador_id__in=ALVOS,
            status="aprovada", tipo__in=tipos, data__range=janela,
        ).select_related("colaborador").order_by("colaborador_id", "data", "pk"):
            if cobre_parte_do_dia(a, tipos):
                cid = a.colaborador_id
                cadastradas.setdefault(cid, {}).setdefault(a.data, []).append(a)
                if cid not in faltas:
                    faltas[cid] = set(export.datas_falta(a.colaborador, empresa, MES, ANO))

        a = medir(empresa, False, cadastradas)
        b = medir(empresa, not entregue, cadastradas)
        medidas.append((empresa.pk, janela, entregue, cadastradas, faltas, a, b))

    # Universo = dias com parcial elegível na grade de competência em aberto.
    # [app/folha/export.py:565-603] [app/ponto/services/dia_pago.py:386-402]
    if not any(b.tetos for _, _, _, _, _, _, b in medidas):
        print("UNIVERSO VAZIO", flush=True)
        raise SystemExit(1)

    print(f"REFERENCIA={timezone.localtime(agora).isoformat()} COMP=11/2026",
          "[app/ponto/services/fechamento.py:634-642] [app/folha/export.py:1240-1247]")
    n_rubricas = 0
    colabs_txt = set()
    dias_abono = {}
    alvos_com_cadastro = set()

    for empresa_id, janela, entregue, cadastradas, faltas, a, b in medidas:
        print(f"EMPRESA={empresa_id} JANELA={janela[0]}/{janela[1]} ENTREGUE={int(entregue)} "
              f"DIAS_ELEGIVEIS={len(b.tetos)} TXT_IGUAL={int(a.txt == b.txt)}",
              "[app/folha/export.py:47-51,525-539,565-603]", REF_ABONO)
        ids_a = {cid for cid, _ in a.registros}
        ids_b = {cid for cid, _ in b.registros}
        for regra, antes, depois in (("JUIZ_TXT", a.entra, b.entra), ("TXT", ids_a, ids_b)):
            for cid in sorted(depois - antes):
                print(f"ENTRA_{regra} {col(cid)}", REF_TXT)
            for cid in sorted(antes - depois):
                print(f"SAI_{regra} {col(cid)}", REF_TXT)
        colabs_txt.update(ids_a ^ ids_b)

        # Uma mudança por (colaborador, código). Os valores são os do campo
        # cru do reg. 10 e cada tupla carrega as datas dos reg. 11. [app/folha/export.py:34-45,224-255]
        for chave in sorted(a.registros.keys() | b.registros.keys()):
            va, vb = a.registros.get(chave, ()), b.registros.get(chave, ())
            if va != vb:
                cid, codigo = chave
                n_rubricas += 1
                colabs_txt.add(cid)
                print(f"RUBRICA {codigo} {col(cid)} VALOR_A={va} VALOR_B={vb}", REF_TXT)

        if a.abonos.keys() != b.abonos.keys():
            raise FalhaSonda(f"Dias do mapa A/B divergentes na empresa {empresa_id} {REF_ABONO}")
        for cid, dia in sorted(a.abonos):
            va, vb = a.abonos[cid, dia], b.abonos[cid, dia]
            if va != vb:
                dias_abono[cid, dia] = va - vb
                print(f"DIA_PAGO_ABONO {col(cid)} {dia} ABONO_A={va:g} ABONO_B={vb:g}", REF_ABONO)

        for cid, dias in sorted(cadastradas.items()):
            alvos_com_cadastro.add(cid)
            for dia, registros in sorted(dias.items()):
                chave = cid, dia
                vencedora = a.dias.get(chave, (None,) * 6)[-1]
                periodos = [
                    (
                        r.hora_ini.strftime("%H:%M") if r.hora_ini is not None else "-",
                        r.hora_fim.strftime("%H:%M") if r.hora_fim is not None else "-",
                        "*" if r.pk == vencedora else "",
                    )
                    for r in registros
                ]
                print(f"ALVO {col(cid)} {dia} PERIODOS={periodos} "
                      f"TETO_MIN={b.tetos.get(chave, '-')} FALTA_DECIDIDA={int(dia in faltas[cid])}",
                      campos_do_dia(a, cid, dia, "A"), campos_do_dia(b, cid, dia, "B"),
                      REF_DIA, "[app/ponto/models.py:745-771] [app/ponto/turnos.py:2816-2839]")

    for cid in sorted(ALVOS - alvos_com_cadastro):
        print(f"ALVO {col(cid)} DIA_PARCIAL={NV} [app/ponto/turnos.py:2622-2639]")

    # MINUTOS_ABONO soma A - B nos dias alterados. [app/ponto/services/dia_pago.py:285-292,364-402]
    print(f"TXT_RUBRICAS_MUDADAS={n_rubricas} COLABS_TXT={len(colabs_txt)} "
          f"DIAS_ABONO_MUDADOS={len(dias_abono)} COLABS_ABONO={len({cid for cid, _ in dias_abono})} "
          f"MINUTOS_ABONO={sum(dias_abono.values()):g}", flush=True)


def main():
    try:
        with patch.object(logging.Logger, "handle", log_sem_dados), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            if connection.schema_name != "juliani":
                raise FalhaSonda("Schema esperado juliani [bin/sombra.sh:490-491]")
            agora = timezone.now()
            with patch.object(timezone, "now", new=lambda: agora):
                medir_e_imprimir(agora)
            raise ReverterSonda()
    except ReverterSonda:
        return
    except FalhaSonda:
        raise
    except Exception as erro:
        # Mantém a falha fatal e publica apenas tipo e origem.
        # [app/ponto/services/fechamento.py:1327-1332]
        origem = extract_tb(erro.__traceback__)[-1]
        raise FalhaSonda(f"{origem.filename}:{origem.lineno} {type(erro).__name__}") from None


main()
```

### FIM

## 26/11 18:14 · sessao 9b9e10f2cc46 · ABONO-PARCIAL-TETO · leitura · origem=Code

# Dossiê ABONO-PARCIAL-TETO

**Na cópia, reduzir apenas o abono de `por_dia_da_grade` muda `FechamentoMensal.minutos_abonados` e `DiaPago.minutos_abonados`. O efeito chega ao espelho, ao cartão e à prontidão. Nenhuma rubrica do TXT lê esse campo, e a classificação de entrada no TXT também prescinde dele.** A 8069 lê atraso e saída antecipada, calculados antes e por outra origem. O DIFF da sombra ainda é necessário para decidir a posição da fatia na fila pelo critério do aval. [app/ponto/services/fechamento.py:1018–1027,1084–1092,1236] [app/relatorios/cartao_pela_celula.py:118,186–190] [app/folha/export.py:439–461,739–755,977–991,1084–1089] [nao verificado]

## 1. Mapa do dia parcial

- **Hoje**, `por_dia_da_grade` lê `data`, `tipo_dia`, `minutos_previstos` e `minutos_realizados` da grade. Para `tipo_dia='ausencia'`, abona **previsto − min(realizado, previsto)**. Sem realizado, abona todo o previsto. A função devolve três mapas em minutos, na ordem abono, previsto e realizado. O abatimento residual é dedutível desses mapas. A função deixa `Ausencia.minutos`, `hora_ini`, `hora_fim`, `cobertura_parcial` e `periodo_parcial` fora da conta. [app/ponto/services/dia_pago.py:364–402]
- **Quem decide se é parcial** é `cobre_parte_do_dia`. Ele consulta os tipos que aceitam horas no catálogo vigente e exige registro de um só dia com `0 < minutos < 480`. O `480` classifica o cadastro. O previsto efetivo do dia vem da grade. `ausencia_explica_o_dia_inteiro` compõe esse juiz com a exclusão de `falta`. Para escolher qual ausência cobre o dia para folha, `cobertura_ausencia_periodo(para='folha')` considera aprovadas e, na sobreposição, vence a mais recente por `(data, pk)`. [app/ponto/turnos.py:2548–2557,2614–2639,2751–2758,2816–2839,2850–2868] [app/escala/services/leitor_celula.py:397–423]
- **Onde já se aplica o período.** A grade pergunta ao juiz da parcial e carimba `cobertura_parcial`. Para dispensar um **marco previsto**, pergunta a `periodo_da_parcial_no_dia`, com bordas inclusivas. Para uma **batida real**, pergunta a `batida_dentro_da_parcial`: S no início e E no fim ficam fora, E no início e S no fim ficam dentro. O cartório lavra os carimbos e `dispensar_por_ausencia` os consome para a dispensa integral. Para a parcial, a grade já dispensou os marcos cobertos. `ausencia_parcial_cobra_o_resto` lê o carimbo e mantém a cobrança de marcos fora do período, ressalvada a folga. [app/escala/utils.py:1229–1233,1272–1295] [app/ponto/turnos.py:2653–2711] [app/ponto/services/cartorio.py:785–790,816–831] [app/chamados/juizes.py:477–505]
- **Onde já se limita dinheiro pelo período.** `_minutos_da_ponta_cobertos` consulta o juiz da batida para contar a interseção com a ponta de atraso ou de saída antecipada. O teto do motor aplica esse abatimento e depois a L-093. O calculador reutiliza o mesmo teto. A parcial paga sem período segue pelo crédito de atraso no próprio dia. Esse percurso e o abono diário da grade têm saídas diferentes. [app/ponto/motor_calculo_v2.py:453–477,1214–1265] [app/ponto/calculador/alimentacao.py:511–539] [app/ponto/calculador/regras.py:115–126,276–282] [app/ponto/services/credito_parcial.py:35–70,91–102]

## 2. Censo dos escritores e leitores até o TXT

| Papel | Caminho na cópia |
|---|---|
| **Apuração e escrita mensal** | `grade_do_fechamento` → `grade_da_celula` → `recalcular_fechamento_mes` → `por_dia_da_grade` → soma em `FechamentoMensal.minutos_abonados`. O recálculo devolve o objeto em leitura ou persiste por `save`/`update`. [app/folha/export.py:329–338] [app/ponto/services/fechamento.py:1037–1038,1084–1088,1152–1168,1184–1209] |
| **Escrita diária** | O recálculo entrega `abono_por_dia` a `dia_pago.lavrar`, nas versões `motor` e `oraculo`. `lavrar` escreve `DiaPago.minutos_abonados` pelo mapa e substitui as linhas da versão. A falta diária chega por outro mapa, `faltas_por_dia`. [app/ponto/services/fechamento.py:1229–1237,1250–1261,1301–1312] [app/ponto/services/dia_pago.py:274–292,345–360] |
| **Escrita no retrato de exportada** | `retratar_exportada` chama a mesma função e atualiza ou cria linhas de `DiaPago` com os campos de grade. As entradas são `lavrar_dias_pagos` e `regen_exportada`. Uma alteração aplicada indiscriminadamente à função comum alcançaria esse percurso. [app/ponto/services/dia_pago.py:443–464,469–497,533–545] [app/ponto/management/commands/lavrar_dias_pagos.py:123] [app/ponto/portas/regen_exportada.py:184–193] | [nao verificado]
| **Espelho e cartão** | `totais_da_folha` lê o fechamento em memória e converte o abono em horas. No ramo de lavratura, lê `soma_da_competencia` em minutos e `dias_com_abono` pelo número de linhas positivas. `folha_manda` aplica os totais ao resumo usado pelo espelho e pelo PDF. [app/relatorios/cartao_pela_celula.py:69–76,118,141–143,186–190,340–346] [app/ponto/services/dia_pago.py:550–563,687–699] [app/ponto/services/espelho.py:1317–1332] [app/relatorios/pdf_espelho.py:628] |
| **Prontidão e conferência** | `prontidao` usa `(realizado + abono) / previsto` no snapshot. O fallback tem uma conta própria que soma todo o previsto de `ausencia`. A lista de fechamento e `simular_folha` leem a prontidão. O vigia `porta_export` compara a soma de `DiaPago` com o fechamento e, para o caso exportado, chama diretamente `por_dia_da_grade` em `_gravado_x_grade`. Essa conferência de abono entra no diagnóstico publicado da porta. [app/folha/export.py:1054–1089,1104–1121] [app/ponto/services/fechamento_lista.py:79–91] [app/folha/management/commands/simular_folha.py:72–88] [app/folha/porta_export.py:128–151,457–473,693–703,785–799] | [nao verificado]
| **Entrada e montagem do TXT** | `gerar_dominio_txt` chama o recálculo em leitura, passa os mesmos objetos a `classificar_export` e `montar_linhas_export`, formata o conteúdo e registra a exportação. A classificação consulta rescisão, código, férias com batida, cadastro, pendências de espelho e homologação. A montagem lê os eventos do fechamento e as datas de falta decidida. [app/folha/views.py:214–222,272–288,302–307] [app/folha/export.py:723–755,839–855,875–891,911–991] |

| Rubrica ou efeito | De onde sai o número |
|---|---|
| **8069 — horas de faltas parciais** | `resultado.horas_atraso` com o crédito aplicável do próprio dia, mais `resultado.horas_saida_antecipada` → campos do fechamento → `rubrica_atraso_parcial` → `atraso_diurno` → registro 10. [app/ponto/services/fechamento.py:1018–1026] [app/folha/export.py:383–399,449–452,739–748] |
| **8792 — falta em dias** | `datas_falta` → catálogo `descontam_vigentes` → ausências aprovadas → total de datas no registro 10 e uma data por registro 11. `FechamentoMensal.horas_falta` também tem origem própria, em `minutos_de_falta_por_dia`. [app/folha/export.py:71–92,125–143,169–185,215–220,749–753] [app/ponto/services/fechamento.py:893–895,1027] |
| **DSR** | O motor julga a semana com dias obrigatórios, trabalho e justificativas. O fechamento recebe semanas e reflexo do resultado. O exportador omite o reflexo de DSR da lista de eventos. [app/ponto/motor_calculo_v2.py:2706–2746] [app/ponto/services/fechamento.py:713–721,1124–1127] [app/folha/export.py:453–461] |
| **Abono da grade** | A lista de eventos e o bloco de faltas esgotam a montagem. `minutos_abonados` aparece na prontidão e nos leitores acima, sem rubrica correspondente no TXT. [app/folha/export.py:439–461,739–755,1084–1089] |

O DIFF de aceite deve comparar **quem entra**, **linhas por rubrica** e **dias alterados**, na mesma sombra e com a mesma data de referência. A simulação em modo `lido` já usa o recálculo e os montadores reais. O resultado dos oito da 11, qualquer resultado da 12 e o zero fora do alvo estão **[nao verificado]**. [app/folha/management/commands/simular_folha.py:112–134] [app/folha/export.py:699–755]

## 3. Leis herdadas e referências corrigidas

| Lei ou corte | Aplicação à fatia |
|---|---|
| **“Abono parcial = cobra o resto”**, L-061 e L-088 | Nesta cópia, a frase está em `RELATO.md:10494`, e o código aplica o corte aos marcos fora do período. L-061 manda ler a lâmpada lavrada. L-088 distingue furo em aberto de falta. A referência `RELATO:10512` do anexo aponta, nesta cópia, para a descrição do ensaio. [app/docs/RELATO.md:10493–10513] [app/docs/LEIS.md:95,120] [app/chamados/juizes.py:477–505] |
| **ABONADO de 27/10**, L-093 e L-095 | Ausência aprovada que explica o dia inteiro e tem efeito `ABONA` zera a pontualidade. Para parcial, o aval remete aos minutos de juiz existente. O registro do corte e do pouso está em `RELATO.md:7233` e `:7650–7654`. `:7251` trata de saída órfã. [app/docs/RELATO.md:7233,7251,7650–7654] [app/docs/LEIS.md:111,113] [app/ponto/turnos.py:2751–2773] | [nao verificado]
| **L-006, L-024, L-085, L-089 e L-090** | Período vem do cadastro do DP. Previsto vem da grade. Jornada pertence ao dia de início. Sem dia previsto, o atestado não gera valor. Janela de competência sai do corte por empresa. [app/docs/LEIS.md:64,75,117,121,123] [app/docs/CORTES.md:462,466,473,477] |
| **L-039, L-092, L-110, L-113 e L-121** | Falta exige decisão. O RED usa a regra aplicada ao cadastro e às batidas. O pouso reconstrói as projeções da competência aberta. Para esta fatia, o aval exclui a exportada, inclusive diante da regra geral posterior sobre recálculo fora da guarda. [app/docs/LEIS.md:87,125,136,142,155–156] [nao verificado] | [nao verificado]
| **Resultado do grep documental** | Para as cinco expressões pedidas, `LEIS.md` aponta `dia_pago` na L-095, `CORTES.md` aponta o corte de 27/10 e `RELATO.md` traz a regra, o pouso e a conta da grade. `DOSSIES.md` deu zero ocorrências literais e registra, por termos correlatos, L-095, um juiz por pergunta e o crivo do TXT. [app/docs/LEIS.md:113,177] [app/docs/CORTES.md:455,462–477,648] [app/docs/RELATO.md:3582,7233,7650–7654,10493–10494] [app/docs/DOSSIES.md:67–71,118–122] | [nao verificado]

**Padrão da cura: `juiz unico / escritor unico` e `celula soberana / fonte unica`.** A grade e a ata entregam a classificação do dia. O período é consultado pelo juiz existente. O mesmo mapa de abono alimenta o fechamento e a lavratura diária. A grade consumida pelo fechamento hoje transporta a classificação `cobertura_parcial` e a resposta categórica `periodo_parcial`. Um valor de cobertura em minutos precisa de insumo além desse rótulo. [app/docs/GLOSSARIO.md:11–12,26] [app/docs/PLANO-PADROES.md:11,55–62] [app/escala/services/leitor_celula.py:418–436] [app/ponto/services/fechamento.py:1084–1088,1236–1237]

## 4. Contrato de entrada

- **Fonte e unidade.** `Ausencia.data` identifica o dia do cadastro. `hora_ini`/`hora_fim` declaram o período aprovado ou editado pela porta do DP. `minutos` participa da classificação de parcial e do crédito legado sem período. A duração válida para o teto vem das horas cadastradas, em minutos, intersectadas com a jornada líquida prevista. [app/ponto/models.py:745–771] [app/ponto/services/ausencia.py:1039–1054,1068–1081] [app/ponto/turnos.py:2622–2639] [app/escala/utils.py:2322–2374]
- **Universo.** O censo lista parciais aprovadas de **21/10 a 20/11/2026** e de **21/11 a 20/12/2026**. Para atribuir a diferença do mapa ao caso, usar a vencedora `para='folha'`, efeito `ABONA`, dia parcial na ata e período declarado. `SUPRIME`, `DESCONTA`, ausência integral, justificativa de outra origem e parcial sem período ficam fora da mudança desta fatia. Competência entregue fica fora do efeito da cura. A sonda identifica esses limites sem alterar o cadastro. [app/ponto/janelas.py:10–21,36–57] [app/ponto/turnos.py:2548–2557,2622–2639,2816–2839] [app/ponto/services/credito_parcial.py:15–59] [app/ponto/services/fechamento.py:680–721] [app/folha/export.py:565–603] [app/docs/BACKLOG.md:2146] [nao verificado]
- **Escala.** Usar o dia da célula e o vínculo que a gerou, com marcos e pausas do DNA. A cauda de um turno iniciado em D continua em D. Em parcial `23:00–02:00` cadastrada em D, o fim fica em `1440 + 120`. Um período inteiro na madrugada, como `01:00–03:00`, é o caso distinto registrado para a obra de pontualidade. O relato registra a resposta de data civil para o cadastro. A cópia consultada resolve a vigência do crédito de ponta por `a.data`. [app/escala/servico_jornada.py:25–66] [app/escala/models.py:1109–1144,1177–1212] [app/escala/utils.py:330–363] [app/ponto/turnos.py:2664–2676] [app/ponto/services/credito_parcial.py:23–32] [app/docs/RELATO.md:50] [app/docs/LEIS.md:117]

## 5. Matriz RED

Valores sintéticos em minutos. Nos casos diurnos, a jornada padrão é **08:00–12:00 e 13:00–17:00**, com 480 min previstos. **Saldo descoberto** é o previsto que ficou sem trabalho e sem abono. Os casos de furo seguem a decisão existente de cobrança. A emissão de desconto no TXT tem o contrato da seção 2. [app/docs/LEIS.md:75,87,111,120,136] [app/docs/CORTES.md:455] [app/ponto/services/dia_pago.py:393–401] [app/escala/utils.py:1280–1295] [app/folha/export.py:439–461,749–753]

| Cadastro + batidas | Abono hoje → abono esperado | Saldo descoberto pela regra |
|---|---:|---|
| Parcial **09:00–11:00**, 120 cadastrados, sem batidas | **480 → 120** | **360**. [app/ponto/services/dia_pago.py:393–396] [app/escala/utils.py:1280–1295] |
| Mesma parcial, par **13:00–17:00** | **240 → 120** | **120**. Com todos os blocos do resto trabalhados, realizado **360**, abono **120**, saldo **0**. [app/ponto/services/dia_pago.py:393–401] [app/docs/CORTES.md:455] |
| Mesma parcial, par **08:00–10:00**, sendo 60 min trabalhados dentro da parcial | **360 → 60** | **300**. O trecho de **09:00–10:00** já foi realizado. Esse caso distingue cobertura do trecho ainda faltante de um simples `min(abono_atual, duração)`. A batida no período também passa pelo juiz de fato em ausência. [app/ponto/services/dia_pago.py:375–384,393–401] [app/docs/CORTES.md:455] [app/ponto/turnos.py:2683–2711] |
| Jornada curta **08:00–12:00**, parcial **08:00–12:00**, `minutos=240`, sem batidas | **240 → 240** | **0**. O registro continua parcial pelo juiz, com cobertura de toda a jornada efetiva. [app/ponto/turnos.py:2619–2639] [app/ponto/services/dia_pago.py:393–396] |
| Parcial **18:00–19:00**, sem batidas | **480 → 0** | **480**. Período inteiramente fora da jornada. [app/ponto/services/dia_pago.py:393–396] [app/escala/utils.py:1283–1295] [app/ponto/motor_calculo_v2.py:453–477] |
| Turno **20/11 19:00–21/11 07:00**, pausa **00:00–01:00**, parcial **23:00–02:00**, sem batidas | **660 → 120** | **540**, todos no dia 20/11 da comp 11. O período tem duração de 180, com 60 de pausa. [app/docs/CORTES.md:473] [app/docs/LEIS.md:117] [app/ponto/turnos.py:2664–2676] [app/escala/utils.py:330–363,2359–2374] |
| Dia com previsto **0**, parcial válida, sem batidas | **0 → 0** | **0**. Uma célula ausente ou indeterminada é um caso de leitura distinto. [app/docs/LEIS.md:121] [app/escala/services/leitor_celula.py:390–412] [app/ponto/services/dia_pago.py:387–401] |
| Ausência integral `ABONA`, 480 previstos, sem batidas | **480 → 480** | **0**. Preservar também o tratamento integral da pontualidade. [app/ponto/services/dia_pago.py:393–396] [app/ponto/turnos.py:2751–2773] [app/docs/RELATO.md:7233] |
| Saída bruta **120 min** às **15:00**, parcial aprovada **15:00–16:00**, 360 min de realizado | **120 → 60** | **60**. `_minutos_da_ponta_cobertos` já abate **60** da ponta no motor. A S das 15:00 encosta no início da parcial e fica fora para efeito de batida real. O RED deve conferir que a 8069 continua com a ponta residual que o motor apurou, sem segundo crédito. [app/ponto/turnos.py:2683–2711] [app/ponto/motor_calculo_v2.py:453–477,1214–1247] [app/ponto/services/credito_parcial.py:35–50] [app/folha/export.py:449–452] |

## 6. Arquivos e obras em voo

| Arquivo | Alcance e conflito |
|---|---|
| `app/ponto/services/dia_pago.py` | Alvo direto de `por_dia_da_grade`. **Conflito de arquivo com c1f pelo escopo informado**. O diff de c1f e o conflito de linhas estão **[nao verificado]**. A mesma função atende ao retrato de exportada, que precisa conservar o percurso vigente para esta fatia. [app/ponto/services/dia_pago.py:364–402,405–464] |
| `app/ponto/services/fechamento.py` e, conforme o insumo escolhido, `app/escala/services/leitor_celula.py` | Pontos de ligação se o cálculo precisar receber a cobertura temporal. O fechamento já tem a ausência vencedora e o leitor transporta os carimbos. O desenho de passagem desse insumo cabe ao construtor. [app/ponto/services/fechamento.py:700–721,1037–1088] [app/escala/services/leitor_celula.py:429–436] |
| `app/ponto/turnos.py` | Dependência de leitura dos juízes. Consumir as autoridades existentes evita conflito direto de arquivo com **PAR-PELO-TURNO** e **DIA-DA-BATIDA** nesse desenho. Há dependência comportamental na atribuição do realizado e da cauda do turno. Os diffs das raias estão **[nao verificado]**. [app/ponto/turnos.py:2622–2711,2971–3028] [app/docs/BACKLOG.md:1864,2027] [app/docs/RELATO.md:187] |
| `app/folha/export.py` e CxR | `export.py` é ponto de conferência do TXT. O fallback de prontidão é um consumidor paralelo já identificado na obra-mãe. O escopo informado de CxR em c1f está **[nao verificado]**. [app/folha/export.py:739–755,1104–1121] [app/docs/BACKLOG.md:2146] [app/docs/RELATO.md:3582] |

## 7. Sonda de leitura

O texto abaixo chama a função real sobre a grade da célula e lista cada cadastro parcial aprovado. `min_periodo` mede a duração declarada e `min_no_previsto` mede a interseção com os blocos líquidos da escala. `None` sinaliza medida indeterminada, inclusive se a geometria disponível divergir do previsto lavrado. `min_excesso_geometrico` é quanto o abono de hoje excede **até** o teto geométrico, útil para localizar os casos. O RED com trabalho dentro do período exige ainda localizar os minutos realizados. [app/escala/services/leitor_celula.py:350–364,390–436] [app/escala/utils.py:330–363,2322–2374] [app/ponto/services/dia_pago.py:364–402] [app/ponto/motor_calculo_v2.py:453–477]

**Alvo informado da 11: 8 colabs, 1.555 min**, `col327`, `col242`, `col298`, `col288`, `col222`, `col885`, `col827` e `col258`. O anexo dá o alvo agregado. A natureza exata dos 1.555 min, os valores por dia e o resultado da 12 estão **[nao verificado]**.

O payload usa consultas de leitura e imprime identificadores `colNNN`, datas, horários, valores e indicadores. Validei a sintaxe em memória e conferi três bordas noturnas e a fórmula atual com entrada sintética. A execução Django na sombra está **[nao verificado]**. O envelope é para execução posterior autorizada. Ele próprio grava credencial, estado técnico e trilha, e ficou sem execução nesta leitura. [app/ponto/turnos.py:2653–2711] [app/ponto/services/dia_pago.py:386–402] [bin/sonda_leitura.sh:121–143,147–193,216–240,245–251]

```python
from datetime import datetime, time
from itertools import groupby

from escala.services.leitor_celula import grade_da_celula
from escala.servico_jornada import escala_vigente
from escala.utils import cadeia_do_dia
from folha.export import competencia_entregue
from ponto.catalogo.ausencias import ABONA, parciais_vigentes
from ponto.janelas import janela_fechamento, periodo_apuracao
from ponto.models import Ausencia
from ponto.motor_calculo_v2 import _minutos_da_ponta_cobertos
from ponto.services.dia_pago import por_dia_da_grade
from ponto.turnos import cobre_parte_do_dia, cobertura_ausencia_periodo


def minutos_no_previsto(ausencia, colab, previsto):
    if previsto in (None, 0):
        return previsto
    dia = ausencia.data
    escala = escala_vigente(colab, dia)
    if escala is None or escala.tipo_escala is None:
        return None
    marcos = escala.marcos_do_dia(dia)
    pausas = escala.pausas_do_dia(dia)
    if not pausas and marcos[2] and marcos[3]:
        pausas = (marcos[2:],)
    zero = datetime.combine(dia, time.min)
    instantes = [
        (instante.replace(tzinfo=None) - zero).total_seconds() / 60
        for _, instante in cadeia_do_dia(dia, marcos, pausas)
    ]
    blocos = list(zip(instantes[::2], instantes[1::2]))
    if sum(fim - ini for ini, fim in blocos) != previsto:
     

[truncado em 20000 de 24470 caracteres]

## 26/11 18:13 · sessao 6fdedc20fe78 · PAUSA-INTERNA-NO-PARTIDO · leitura · origem=Code

**Totais da competência 11/2026 para as empresas 2, 3 e 4: [nao verificado].** A sonda chama `recalcular_fechamento_mes` com os mesmos argumentos de leitura usados pela geração do TXT. O fechamento carrega as células em lote, atribui o mapa a `motor.celulas_alimentadas` e chama `calcular_mes` por fatia. `app/folha/views.py:214-219` · `app/ponto/services/fechamento.py:624-626,744-775,297-311`

A interceptação fica na entrada de `MotorTurnoPartido.calcular_periodo`, antes do recorte pela janela. Ela soma os minutos **por chamada com `intervalos`**, incluindo a chamada de crédito de turno aberto, e conta cada par colaborador–dia uma vez no `TOTAL`. `UNIVERSO dias` conta os pares colaborador–dia das fatias efetivamente apuradas pelo Partido, incluindo dias sem batida e os dias atribuídos aos períodos pelo motor. `app/ponto/motor_calculo_v2.py:2570-2574,2627-2667,2993-3013,3677-3684` · `app/ponto/services/fechamento.py:283-311`

Com universo vazio, o Python imprime `UNIVERSO VAZIO` e sai com código 1. Nesta cópia, o `echo` depois do pipeline mascara o código de saída no comando externo `bin/sombra.sh --rodar`. `bin/sombra.sh:45,490-493`

```python
### SCRIPT
import logging
import sys
from collections import defaultdict
from datetime import date, timedelta
from traceback import extract_tb
from unittest.mock import patch

from django.db import connection, transaction

from ponto.janelas import janela_fechamento
from ponto.motor_calculo_v2 import MotorBase, MotorTurnoPartido
from ponto.services import fechamento

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA = (date(2026, 10, 21), date(2026, 11, 20))


class ReverterSonda(Exception):
    pass


class FalhaRegistrada(BaseException):
    pass


def interceptar_log(_logger, registro):
    # O fechamento registra o nome e segue em app/ponto/services/fechamento.py:1327-1332.
    # Interrompe antes de os handlers formatarem ou gravarem o registro.
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaRegistrada(
            f"{registro.pathname}:{registro.lineno} {tipo}"
        ) from None


def em_minutos(duracao):
    return f"{duracao / timedelta(minutes=1):.9f}".rstrip("0").rstrip(".")


def main():
    universo = set()
    pausas_por_dia = defaultdict(timedelta)
    calcular_mes = MotorTurnoPartido.calcular_mes
    calcular_periodo = MotorTurnoPartido.calcular_periodo
    codigo_fatia = fechamento.apurar_por_fatia.__code__
    codigo_mes_base = MotorBase.calcular_mes.__code__

    def observar_mes(motor, *args, **kwargs):
        # Cobertura e colaborador do fechamento em app/ponto/services/fechamento.py:283-311.
        origem = sys._getframe(1)
        if origem.f_code is not codigo_fatia or motor.celulas_alimentadas is None:
            raise RuntimeError("Partido fora da fatia alimentada do fechamento")
        dados = origem.f_locals
        colaborador_id = kwargs["colaborador_id"]
        universo.update((colaborador_id, dia) for dia in dados["_dias_da_fatia"])
        return calcular_mes(motor, *args, **kwargs)

    def observar_periodo(motor, entrada, saida, intervalos=()):
        # Dia da jornada em app/ponto/motor_calculo_v2.py:2570-2574.
        # No crédito de aberto, o dia é _d_a em :2627-2667 do mesmo arquivo.
        origem = sys._getframe(1)
        if origem.f_code is not codigo_mes_base:
            raise RuntimeError("Chamada inesperada de calcular_periodo do Partido")
        dados = origem.f_locals
        dia = dados["_d_a"] if "_pares_a" in dados else dados["data_jornada"]
        if not JANELA[0] <= dia <= JANELA[1]:
            raise RuntimeError("Dia de jornada fora da janela da sonda")
        chave = (dados["colaborador_id"], dia)
        universo.add(chave)
        if intervalos:
            pausas_por_dia[chave] += sum(
                (fim - inicio for inicio, fim in intervalos), timedelta()
            )
        return calcular_periodo(motor, entrada, saida, intervalos)

    try:
        with (
            patch.object(logging.Logger, "handle", interceptar_log),
            patch.object(MotorTurnoPartido, "calcular_mes", observar_mes),
            patch.object(MotorTurnoPartido, "calcular_periodo", observar_periodo),
            transaction.atomic(durable=True),
        ):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            for empresa_id in EMPRESAS:
                if janela_fechamento(MES, ANO, empresa_id) != JANELA:
                    raise RuntimeError(f"Janela divergente na empresa {empresa_id}")
                fechamento.recalcular_fechamento_mes(
                    MES,
                    ANO,
                    empresa_id=empresa_id,
                    somente_leitura=True,
                    retornar_objeto=True,
                    lavrar_dias_pagos=False,
                )
            raise ReverterSonda()
    except ReverterSonda:
        pass
    except Exception as erro:
        # Preserva o local da falha sem publicar a mensagem original.
        local = extract_tb(erro.__traceback__)[-1]
        raise RuntimeError(
            f"{local.filename}:{local.lineno} {type(erro).__name__}"
        ) from None

    if not universo:
        print("UNIVERSO VAZIO", flush=True)
        raise SystemExit(1)

    for (colaborador_id, dia), duracao in sorted(pausas_por_dia.items()):
        print(f"col{colaborador_id:03d} {dia.isoformat()} minutos={em_minutos(duracao)}")

    colabs_partido = len({cid for cid, _ in universo})
    colabs_afetados = len({cid for cid, _ in pausas_por_dia})
    total = em_minutos(sum(pausas_por_dia.values(), timedelta()))
    print(f"UNIVERSO colabs_partido={colabs_partido} dias={len(universo)}")
    print(f"TOTAL dia_colab={len(pausas_por_dia)} colabs={colabs_afetados} minutos={total}")


main()
### FIM
```

## 26/11 18:09 · sessao 56e47cebcda0 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

Entreguei 45 testes novos, um por caso de 01 a 46, com a exclusão do 33. A leitura estática indica **24 REDs de asserção, 10 REDs de interface e 11 guardas**. Os dois arquivos compilaram em memória, as 11 mutações encontraram âncoras únicas e a contagem literal de `except` nos arquivos novos foi **0**. Execução no main, estado da produção e certificação pelo Code e pelo opus final: [nao verificado]. [Matriz de testes](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:213) · [Runner de mutantes](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:10) [nao verificado]

Usei os nomes de negócio do brief em `snake_case` onde a assinatura ainda precisa ser definida. Os encaixes de apply e reversão, do selo com lápide e da carga do estoque na migração são propostas de teste. A integração com as interfaces finais está [nao verificado]. [Selo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:167) · [Reversão](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:261) · [Estoque](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:461) [nao verificado]

### Matriz por leitura estática

**A** é falha esperada por asserção. **I** é falha esperada por interface ausente. **Guarda** indica passagem esperada na base e falha esperada com o mutante da docstring. A coluna de teste indica a linha de `chamados/tests/test_e4d_matriz_reds.py`. Execução dos casos [nao verificado].

| Caso | Teste | Classe | Motivo na cópia | [nao verificado]
|---|---|---|---|
| 01 | `test_caso_01_backfill_resposta_sem_evento` :213 | A | `registrar` acorda a célula após inserir a resposta histórica. `chamados/models.py:4562` · `chamados/models.py:4575` |
| 02 | `test_caso_02_backfill_legado_sem_inferir_absorcao` :230 | A | A validação histórica entra pela porta que acorda a célula. `chamados/models.py:4404` · `chamados/models.py:4575` |
| 03 | `test_caso_03_absorcao_historica_sem_validacao_retroativa` :245 | A | O seletor oferece `VALIDACAO` ao encontrar V sem linha correspondente, inclusive com `ABSORCAO` física. `chamados/models.py:4393` · `chamados/models.py:4404` |
| 04 | `test_caso_04_reversao_separavel_por_competencia` :261 | I | Falta a operação de apply de R/V com manifesto e reversão por competência. `chamados/management/commands/backfill_decisoes_pergunta.py:193` · `chamados/models.py:4262` |
| 05 | `test_caso_05_absorcao_e_validacao_antes_de_v_m_no_mesmo_atomic` :931 | A | O carimbo vem primeiro e o ramo grava somente `ABSORCAO`. `chamados/services/materializacao.py:1104` · `chamados/services/materializacao.py:1125` |
| 06 | `test_caso_06_celula_absolve_pela_via_listada` :544 | I | A via carimba M e falta a lista que declara seu escritor e sua lápide. `chamados/reconciliador.py:699` · `chamados/tests/test_e4d_matriz_reds.py:167` |
| 07 | `test_caso_07_backfill_celula_absolveu_fica_como_estava` :301 | I | O caso cobra o selo para o M histórico preservado. `chamados/models.py:4390` · `chamados/tests/test_e4d_matriz_reds.py:167` |
| 08 | `test_caso_08_exportada_so_trilha_txt_e_gravado_iguais` :312 | I | Falta o apply autorizado de trilha com prova dos dois hashes. O comando atual registra a parada. `chamados/management/commands/backfill_decisoes_pergunta.py:459` · `chamados/management/commands/backfill_decisoes_pergunta.py:514` |
| 09 | `test_caso_09_resolvedor_validar_registra_antes_de_v` :564 | A | O resolvedor carimba V sem `VALIDACAO` física. `chamados/services/disputa_emissao.py:2118` |
| 10 | `test_caso_10_resolvedor_responder_registra_so_com_texto` :584 | A | O ramo carimba R e grava auditoria lateral sem `RESPOSTA` física. `chamados/services/disputa_emissao.py:2192` |
| 11 | `test_caso_11_reabertura_vazia_com_retry_do_mesmo_pedido` :346 | I | A porta carece de `id_do_pedido` e o filtro atual pula a pendente vazia. `chamados/services/disputa_emissao.py:1628` · `chamados/services/disputa_emissao.py:1709` |
| 12 | `test_caso_12_proteger_autoria_e_marcar_autoria_historica_nula` :369 | A | As FKs de V, M e das autorias da DP usam `SET_NULL`. `chamados/models.py:3765` · `chamados/models.py:3785` · `chamados/models.py:4307` |
| 13 | `test_caso_13_autor_inativo_preserva_validacao_e_refresh_recusa` :605 | Guarda | A porta desativa o acesso e o refresh verifica `is_active`. `core/credenciais.py:110` · `api/views.py:96` |
| 14 | `test_caso_14_saneamento_alveja_so_resposta_historica` :389 | I | `SANEAMENTO` está ausente dos choices. A porta guarda a resposta legada e limpa pelo lote. `chamados/models.py:4289` · `chamados/services/validacao.py:470` |
| 15 | `test_caso_15_saneamento_rele_competencia_entregue_sob_trava` :630 | Guarda | A porta relê a elegibilidade sob trava e a entrega barra a escrita. `chamados/services/validacao.py:436` · `chamados/services/validacao.py:464` |
| 16 | `test_caso_16_registrar_recusa_resposta_futura_sem_efeito` :414 | A | `registrar` insere a resposta sem comparar `ato_em` com o relógio atual. `chamados/models.py:4522` · `chamados/models.py:4562` |
| 17 | `test_caso_17_save_sem_pk_recusa_futuro_e_preserva_selo_ast` :425 | A | `save` veta PK preenchida e permite a inserção direta da instância nova. `chamados/models.py:4366` |
| 18 | `test_caso_18_estoque_futuro_compensa_no_horizonte_comum` :437 | A | A tarefa corta a sequência no horizonte, enquanto a composta reduz a compensação futura. `chamados/services/tarefa_do_dia.py:389` · `chamados/models.py:4447` |
| 19 | `test_caso_19_resposta_humana_vence_automatica_no_empate` :654 | Guarda | O automático relê o texto sob a trava e preserva a resposta humana. `chamados/services/disputa_emissao.py:1741` |
| 20 | `test_caso_20_recusa_para_so_q_e_preserva_irma_ja_absorvida` :954 | A | A passagem pode absorver Q depois que a recusa humana já limpou sua resposta. `chamados/services/materializacao.py:1087` · `chamados/services/materializacao.py:1125` |
| 21 | `test_caso_21_recusa_rele_validacao_sob_trava` :671 | Guarda | A segunda guarda retorna `ja_validada` antes do motivo e da recusa. `chamados/services/validacao.py:951` · `chamados/services/validacao.py:974` |
| 22 | `test_caso_22_validacao_rele_reabertura_sob_trava` :682 | Guarda | A releitura depois da trava detecta a falta da resposta. `chamados/services/validacao.py:742` · `chamados/services/validacao.py:755` |
| 23 | `test_caso_23_correcao_rele_validacao_sob_trava` :697 | Guarda | A validação fresca barra a correção antes da nota. `chamados/services/acoes_disputa.py:418` · `chamados/services/acoes_disputa.py:428` |
| 24 | `test_caso_24_ancora_fora_deixa_dependente_sem_cobranca` :992 | Guarda | O veto `sem_grupo_recuperavel` desfaz o carimbo tentado e a passagem grava a ocorrência. `chamados/services/materializacao.py:1115` · `chamados/services/materializacao.py:1139` |
| 25 | `test_caso_25_ausencia_rejeitada_acha_irma_pelo_ato` :1019 | A | O seletor exige âncora `hora` pela projeção e perde o vínculo físico com o escape `escolha`. `chamados/services/materializacao.py:1195` |
| 26 | `test_caso_26_sem_absorcao_preserva_legado_e_encaminha_admin` :1046 | A | A coincidência de colunas inclui Q na devolução. O leitor da lista do admin também está pendente. `chamados/services/materializacao.py:1195` · `chamados/services/materializacao.py:1263` · `chamados/tests/test_e4d_matriz_reds.py:1062` |
| 27 | `test_caso_27_fantasma_trava_rele_julga_e_escreve` :712 | Guarda | A trava do dono precede a releitura, o juízo e o carimbo. `chamados/reconciliador.py:1264` |
| 28 | `test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4` :742 | A | A seleção atual exclui `declaracao`. O ramo admitido abre `atomic` e grava a via sem a trava do dono e sem `REABERTURA`. `chamados/management/commands/encerrar_perguntas_com_fato.py:41` · `chamados/management/commands/encerrar_perguntas_com_fato.py:64` |
| 29 | `test_caso_29_perda_de_lastro_reabre_por_ato` :775 | A | A porta descarimba e grava auditoria lateral sem `REABERTURA`. `chamados/services/relavra_via.py:244` |
| 30 | `test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta` :1069 | A | SQL, API, modal e raio-x ainda leem o par cru em seus sítios de consumo. `chamados/juizes.py:1671` · `api/views.py:1163` · `templates/chamados/partials/modal_fio.html:219` · `chamados/services/raiox.py:135` |
| 31 | `test_caso_31_nascimento_da_raiz_listado_com_lapide` :1120 | I | A raiz é criada diretamente e falta o selo dos escritores declarados. `chamados/services/disputa_emissao.py:601` · `chamados/tests/test_e4d_matriz_reds.py:167` |
| 32 | `test_caso_32_nascimento_da_filha_listado_com_rvm_vazios` :1136 | I | A filha nasce pela porta existente e falta a declaração dos campos de identidade e do escritor. `chamados/services/disputa_emissao.py:851` · `chamados/models.py:3291` |
| 34 | `test_caso_34_silencio_antes_de_m_e_fecho_pela_raiz` :1152 | A | M é gravado antes de `SILENCIO`. `chamados/models.py:2148` |
| 35 | `test_caso_35_reabre_a_mais_recente_e_desempata_por_pk` :1167 | A | A seleção procura disputa aberta e cria outra quando a busca fica vazia. `chamados/services/disputa_emissao.py:539` · `chamados/services/disputa_emissao.py:601` |
| 36 | `test_caso_36_tranca_retirada_da_fila_por_leitura` :1199 | Guarda | C6 lê a tranca e o encerrador retira o caso da fila sem carimbar a filha. `chamados/juizes.py:1494` · `chamados/services/competencia_trancada.py:202` |
| 37 | `test_caso_37_destranca_passivo_preserva_resposta_pela_via_listada` :1218 | I | O descarimbo preserva R e falta a declaração do escritor e da lápide. `chamados/models.py:3664` · `chamados/tests/test_e4d_matriz_reds.py:167` |
| 38 | `test_caso_38_desligamento_registra_silencio_antes_de_m` :1237 | A | A cascata usa o fecho que grava M antes de `SILENCIO`. `colaboradores/services/desligamento.py:167` · `chamados/models.py:2148` |
| 39 | `test_caso_39_validacao_antes_da_coluna_e_sustenta_plantio` :794 | A | A porta salva V antes da `VALIDACAO` física. `chamados/services/validacao.py:170` |
| 40 | `test_caso_40_recusa_antes_do_motivo_e_da_limpeza` :815 | A | `recusa_motivo` é salvo antes da `RECUSA` física. `chamados/services/validacao.py:995` |
| 41 | `test_caso_41_correcao_subsiste_apos_recusa_do_plantio` :833 | A | A correção salva R antes da `RESPOSTA`. O caso também cobra `RESULTADO` com o motivo do plantio. `chamados/services/disputa_emissao.py:1562` · `chamados/services/acoes_disputa.py:454` |
| 42 | `test_caso_42_entrada_historica_preserva_lavra_e_marcos` :461 | I | Faltam o instante de entrada e a carga do estoque. Os marcos atuais agregam R/M pelas colunas. `chamados/models.py:4305` · `chamados/veredito_lavrado.py:201` |
| 43 | `test_caso_43_fecho_repetido_mantem_ato_e_trilha_unicos` :1264 | Guarda | O mesmo fecho retorna antes de repetir o ato ou a trilha. `chamados/models.py:2081` |
| 44 | `test_caso_44_retry_do_mesmo_payload_de_backfill` :493 | A | Reapresentar a lista preparada passa novamente pelo `bulk_create`. `chamados/models.py:4551` · `chamados/models.py:4562` |
| 45 | `test_caso_45_vigia_compara_valor_e_sustentacao_fisica` :510 | A | O vigia compara presença e a leitura pode sintetizar uma validação da coluna. `chamados/services/vigia_ato_x_coluna.py:8` · `chamados/models.py:4404` |
| 46 | `test_caso_46_prontidao_mantem_480_previstos_com_novo_realizado` :1281 | Guarda | A prontidão soma os previstos da grade e usa o realizado no numerador. `folha/export.py:1095` · `folha/export.py:1111` |

**Caso 20.** A caracterização que cai com a cura é `RespostaAposAbsorcaoTest.test_RED_recusa_datada_depois_mas_inserida_antes_da_absorcao_recusa`. Ela exige absorção física de Q após a recusa e nova resposta barrada pelo H4. O novo teste exige Q pendente, preserva a irmã já absorvida e verifica que a passagem alcança a irmã seguinte. [Teste antigo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:364) · [Teste novo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:954) [nao verificado]

### Comandos para a sessão no main

O primeiro comando roda a matriz. Para isolar um caso, acrescente `-k test_caso_NN_`. O laço executa uma invocação por mutante e seleciona sua guarda correspondente. Resultado dessas execuções [nao verificado]. [Entrada do Django](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/manage.py:7) · [Seleção dos mutantes](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:61)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4d_matriz_reds --parallel 1

for caso in 13 15 19 21 22 23 24 27 36 43 46; do
  E4D_MUTANTE="$caso" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    chamados.tests.test_e4d_matriz_reds --parallel 1 -k "test_caso_${caso}_" \
    --testrunner=chamados.tests.e4d_matriz_mutantes.RunnerMutanteE4D
done
```

## 26/11 18:00 · sessao 062718fdff71 · CLEAR-NAO-PARA-A-NOITE · verificador · origem=Code

**REPROVA na pré-verificação.** Um `/clear` seguido de compact antes do próximo Stop pode fazer o hook pedir outro `/clear`, mesmo depois de registrar `ATENDIDO`. No orçamento quebrado, dois Stops podem emitir o mesmo aviso porque a consulta e a gravação da marca ocorrem depois de soltar a trava. Confirmei o primeiro caso e a perda da marca após 4 KiB de `FALHA` com as funções da cópia e dados em memória. `bin/hook_stop_fila1.py:1074`, `bin/hook_stop_fila1.py:1082`, `bin/hook_stop_fila1.py:1090`, `bin/hook_stop_fila1.py:1152`, `bin/hook_stop_fila1.py:940`, `bin/hook_stop_fila1.py:1011`, `bin/hook_stop_fila1.py:1295`.

Os três arquivos alterados conferem com os blobs do diff anexo. Identidade do commit e do pai via Git, execução dos selos, `red.log`, entrega do construtor em anexo separado e ocorrência em produção **[nao verificado]**. Fiz a revisão em leitura, com contagem literal das âncoras e verificações em memória. `bin/hook_stop_fila1.py:866`, `bin/disjuntor.py:1378`, `bin/tests/test_orcamento_contexto.sh:507`.

## 1. Diff e censo de estado

| Mudança | O que faz e quem escreve ou lê |
|---|---|
| Forma do `AVISO` e texto noturno | `_AVISO_FORMA` alimenta o texto emitido por `_recado_sem_operador`. O texto manda o destinatário publicar o `AVISO` e aponta para `ORDEM-VIVA-TOPO`, sem consultar ou interpolar o BACKLOG. `_aviso_do_topo` lê a forma no texto que recebeu de `_linha_do_topo`. O hook emite a instrução pelo JSON do Stop. Publicação efetiva da linha em produção **[nao verificado]**. `bin/hook_stop_fila1.py:867`, `bin/hook_stop_fila1.py:877`, `bin/hook_stop_fila1.py:1024`, `bin/hook_stop_fila1.py:1108`, `bin/hook_stop_fila1.py:1485`. |
| `aviso_desde` | `_orcamento_travado` grava `agora` nos três caminhos normais, troca de `PAREI`, disparo já ativo e armamento noturno. No reset, lê o valor antigo e o copia se a origem do último marco for diferente de `clear`. Na volta, lê apenas a presença do valor. A hora exibida no aviso pode vir de `disparo.hora` e o código não compara o `HH:MM` do topo com `aviso_desde`. `bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1091`, `bin/hook_stop_fila1.py:1126`, `bin/hook_stop_fila1.py:1133`, `bin/hook_stop_fila1.py:1142`, `bin/hook_stop_fila1.py:1152`, `bin/hook_stop_fila1.py:1261`. |
| `noite_troca_visto` e `disparo.noite_visto` | O ramo de troca lê a primeira marca, formata a saída antes de gravá-la e agora também marca `noite_visto` se houver disparo ativo. O ramo de disparo ativo lê `noite_visto` e a grava depois de formatar a saída. O armamento noturno também formata antes de gravá-la. O hook retira a marca de troca quando deixa de ver `PAREI: ! CLEAR`, retira a do disparo ativo ao sair da ausência e descarta ambas ao reconstruir o estado. `bin/hook_stop_fila1.py:1088`, `bin/hook_stop_fila1.py:1113`, `bin/hook_stop_fila1.py:1119`, `bin/hook_stop_fila1.py:1128`, `bin/hook_stop_fila1.py:1132`, `bin/hook_stop_fila1.py:1135`, `bin/hook_stop_fila1.py:1140`, `bin/hook_stop_fila1.py:1260`. |
| `aviso_dia_visto` | O hook apaga a marca se houver ausência ou se o topo deixar de casar com `AVISO`. Na volta, exige `AVISO`, `aviso_desde`, ausência da marca e guardas livres. Grava a marca, registra `CLEAR-VOLTA` e pede handoff mais a linha de CLEAR. O reset de marco também descarta a marca. A garantia é de uma emissão enquanto a marca persistir. `bin/hook_stop_fila1.py:1088`, `bin/hook_stop_fila1.py:1115`, `bin/hook_stop_fila1.py:1151`, `bin/hook_stop_fila1.py:1161`. |
| `disparo`, `pendente` e `fim_visto` | O armamento por tokens já gravava `disparo.hora` e `disparo.motivo`. A volta agora também grava esses campos se não houver disparo ativo, inclusive se o anterior tiver `desfecho`, e limpa `pendente`. Com disparo ativo, a volta grava `fim_visto`. O hook lê `disparo` na transição de marco e na máquina do Stop. Lê `hora` e `handoff_min` para provar o par, grava `desfecho='seguiu'` ao expirar e lê `fim_visto` para suprimir novo pedido de fim de marco. `pendente` é lido e gravado na arbitragem do gatilho. `bin/hook_stop_fila1.py:1067`, `bin/hook_stop_fila1.py:1109`, `bin/hook_stop_fila1.py:1121`, `bin/hook_stop_fila1.py:1155`, `bin/hook_stop_fila1.py:1160`, `bin/hook_stop_fila1.py:1169`, `bin/hook_stop_fila1.py:1208`, `bin/hook_stop_fila1.py:1219`, `bin/hook_stop_fila1.py:1234`, `bin/hook_stop_fila1.py:1249`, `bin/hook_stop_fila1.py:1256`. |
| Marca do aviso com orçamento quebrado | `_orc_recado_quebrado_ja_dado` lê a cauda de 4 KiB de `orcamento.log` e procura a marca textual na primeira linha encontrada de trás para frente que não contenha ` FALHA `. O chamador formata a troca, apenda `CLEAR-NOITE orcamento quebrado…` diretamente e devolve `siga`. Esse ramo não grava `aviso_desde` nem as marcas do JSON. `bin/hook_stop_fila1.py:932`, `bin/hook_stop_fila1.py:938`, `bin/hook_stop_fila1.py:942`, `bin/hook_stop_fila1.py:1294`, `bin/hook_stop_fila1.py:1299`. |
| `ultima_fala` | O produtor da tupla é `ultima_fala`. JSON ilegível é pulado. Se o JSON foi lido e `_iso` levanta `ValueError`, a linha ainda passa por `_fala_humana` com `t=None`. A tupla leva o texto da última fala na ordem do arquivo, seu epoch opcional e o menor epoch legível da cauda. `ultima_humana` lê o texto para a ESCADA, consultada pelo B4 de Agent, pelo B4 de Stop e pela decisão Codex. `janela_ausente` lê a tupla para julgar ausência. O hook recebe o caminho do transcript pelo payload. Escritor do JSONL fora da cópia **[nao verificado]**. `bin/disjuntor.py:1342`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1382`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1404`, `bin/disjuntor.py:1414`, `bin/disjuntor.py:1425`, `bin/disjuntor.py:1467`, `bin/disjuntor.py:1641`, `bin/orcamento_contexto.py:104`, `bin/hook_stop_fila1.py:1453`. |

**Persistência e leitores fora do diff**

- **PROCEDE — há dois canais de estado.** O hook lê o JSON em `bin/hook_stop_fila1.py:1043` e o substitui em `bin/hook_stop_fila1.py:919`. No caminho normal, os eventos enfileirados só vão para a trilha depois da substituição. O caminho quebrado usa diretamente o mesmo gravador físico da trilha. `bin/hook_stop_fila1.py:895`, `bin/hook_stop_fila1.py:908`, `bin/hook_stop_fila1.py:927`, `bin/hook_stop_fila1.py:1297`.
- **NAO PROCEDE — `disjuntor.ler` ou `gerar_estado.py` contam o novo `DISPARO` do orçamento.** `disjuntor.ler` lê `ultima_medida` do JSON e conta `ATENDIDO` e `CLEAR-NOITE` de `orcamento.log`. A contagem de disparos usa o evento `DISJUNTOR` de `disjuntor.log`. `orcamento_contexto.ler` lê `ultima_medida` e `desligado` do JSON e conta `ATENDIDO` e `SEGUIU` da trilha. `gerar_estado.py` chama os dois e publica as frases. O novo `DISPARO` fica na trilha e é observável por quem procura esse literal, como os selos. `bin/disjuntor.py:123`, `bin/disjuntor.py:2117`, `bin/disjuntor.py:2142`, `bin/disjuntor.py:2160`, `bin/disjuntor.py:2416`, `bin/orcamento_contexto.py:382`, `bin/orcamento_contexto.py:393`, `bin/orcamento_contexto.py:397`, `bin/gerar_estado.py:291`, `bin/gerar_estado.py:297`, `bin/gerar_estado.py:394`, `bin/tests/test_orcamento_contexto.sh:176`.
- **PROCEDE — o handoff é testemunha por `mtime`.** `handoff_sessao.sh` grava `HANDOFF-SESSAO.md` e consulta o topo para exibição e os juízes do hook para os vereditos. Sua listagem de `PAREI`, `lei:` e `!` não inclui `AVISO`. O Stop lê o `mtime` para CLEAR e também o passa ao B4. `SessionStart` imprime o handoff e chama `marco_de_clear.py`, que é o escritor de `origem`, `hora` e `clear_horas`. `orcamento_contexto.ler_marco` entrega esses dados ao Stop. `bin/handoff_sessao.sh:87`, `bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`, `bin/handoff_sessao.sh:129`, `bin/handoff_sessao.sh:323`, `bin/handoff_sessao.sh:327`, `bin/hook_stop_fila1.py:956`, `bin/hook_stop_fila1.py:1169`, `bin/hook_stop_fila1.py:1317`, `.claude/settings.json:40`, `.claude/settings.json:44`, `.claude/settings.json:53`, `.claude/settings.json:57`, `bin/marco_de_clear.py:44`, `bin/marco_de_clear.py:52`, `bin/marco_de_clear.py:61`, `bin/orcamento_contexto.py:177`.

## 2. Âncoras dos selos

**NAO PROCEDE — âncora integral duplicada ou ausente.** Contei **202 de 202** âncoras integrais de chamadas declaradas em `test_disjuntor.sh` com uma ocorrência cada. A âncora da religação do B4 também tem **1** ocorrência. Os dois fragmentos da pergunta têm **2** ocorrências cada, mas nenhum dos mutantes usa esses fragmentos sozinhos. `bin/tests/test_disjuntor.sh:1643`, `bin/tests/test_disjuntor.sh:1668`, `bin/tests/test_disjuntor.sh:1680`, `bin/tests/test_disjuntor.sh:1983`, `bin/disjuntor.py:146`, `bin/hook_stop_fila1.py:1156`, `bin/hook_stop_fila1.py:1256`, `bin/hook_stop_fila1.py:1158`, `bin/hook_stop_fila1.py:1258`, `bin/tests/test_orcamento_contexto.sh:858`.

| Selo e declaração | Âncora no alvo | Ocorrências por âncora |
|---|---|---:|
| `bin/tests/test_disjuntor.sh:1694`, `:1699`, `:1706`, `:1937`, `:1983` | `bin/hook_stop_fila1.py:1169`, `:1096`, `:1099`, `:1096`, `:1286`, respectivamente | 1 |
| `bin/tests/test_disjuntor.sh:1733`, `:1792`, `:1882`, `:1887`, `:1889`, `:1943` | `bin/hook_stop_fila1.py:1312`, `:1321`, `:470`, `:483`, `:480`, `:1322`, respectivamente | 1 |
| `bin/tests/test_disjuntor.sh:1721`, `:1722`, `:1723`, `:1724` | `bin/disjuntor.py:1415`, `:152`, `:1360`, `:1355`, respectivamente | 1 |
| `bin/tests/test_hook_mensagem_em_toda_copia.sh:105` | `bin/commit_msg.sh:12`, `^set -uo pipefail$` | 1 |
| `bin/tests/test_orcamento_contexto.sh:827`, `:830`, `:833`, `:834`, `:839`, `:842`, `:846`, `:847`, `:848` | `bin/hook_stop_fila1.py:1127`, `:1127`, `:1114`, `:1259`, `:1142`, `:1146`, `:1295`, `:1295`, `:942`, respectivamente | 1 |
| `bin/tests/test_orcamento_contexto.sh:852`, `:853`, `:856`, `:857`, `:858`, `:859`, `:865`, `:866`, `:871`, `:872` | `bin/hook_stop_fila1.py:1032`, `:1132`, `:1152`, `:1090`, `:1156`, `:1090`, `:1135`, `:1121`, `:1032`, `:1287`, respectivamente | 1 |
| `bin/tests/test_orcamento_contexto.sh:860`, `:874` | `bin/disjuntor.py:1386`, `:2144`, respectivamente | 1 |
| `bin/tests/test_orcamento_contexto.sh:869`, `:870`, `:877` | `bin/orcamento_contexto.py:105`, `:100`, `:104`, respectivamente | 1 |

Nos demais `test_hook_*.sh`, a substituição literal identificada altera a fixture do BACKLOG, não o código do hook. Os ganchos falsos do selo de commit são construídos pelo próprio teste. `bin/tests/test_hook_parei_trava_o_item_em_curso.sh:62`, `bin/tests/test_hook_mensagem_em_toda_copia.sh:110`, `bin/tests/test_hook_mensagem_em_toda_copia.sh:114`, `bin/tests/test_hook_mensagem_em_toda_copia.sh:121`.

## 3. Classes de reprovação

| Classe | Veredito |
|---|---|
| Segundo juiz da fala | **NAO PROCEDE.** A classificação continua em `_fala_humana` e os consumidores delegam a `ultima_fala`. No contido pode haver duas consultas ao mesmo juiz no mesmo Stop. `bin/disjuntor.py:1342`, `bin/disjuntor.py:1393`, `bin/disjuntor.py:1404`, `bin/orcamento_contexto.py:104`, `bin/hook_stop_fila1.py:1287`, `bin/hook_stop_fila1.py:1294`. |
| Segundo juiz do topo | **NAO PROCEDE** para duplicação do recorte. `_aviso_do_topo` classifica a linha recebida e B3 delega ao mesmo recorte. **PROCEDE** para duas observações do topo no contido, uma para liberar e outra para trocar pelo `AVISO`. Uma escrita entre as duas pode mudar a linha examinada. `bin/hook_stop_fila1.py:408`, `bin/hook_stop_fila1.py:462`, `bin/hook_stop_fila1.py:877`, `bin/disjuntor.py:789`, `bin/hook_stop_fila1.py:1286`, `bin/hook_stop_fila1.py:1295`. |
| Segundo escritor fora da porta | **NAO PROCEDE** para o JSON, que mantém `_orc_salva`. **PROCEDE** para a marca `CLEAR-NOITE`: o ramo novo apenda diretamente depois que `_orcamento` soltou a trava. L-007 é a referência para a exigência de escritor canônico. `app/docs/LEIS.md:65`, `bin/hook_stop_fila1.py:912`, `bin/hook_stop_fila1.py:927`, `bin/hook_stop_fila1.py:1011`, `bin/hook_stop_fila1.py:1274`, `bin/hook_stop_fila1.py:1297`. |
| Relógio fora da borda | **PROCEDE.** O fluxo normal injeta um `agora`. O contido já colhia até dois instantes no P14 e agora pode colher mais dois, para julgar ausência outra vez e para formatar a hora do aviso. O desvio pode cruzar 08h, 23h ou 1.800 s, além da janela de frescor do handoff. `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1112`, `bin/hook_stop_fila1.py:1286`, `bin/hook_stop_fila1.py:1294`, `bin/hook_stop_fila1.py:1296`, `bin/orcamento_contexto.py:99`, `bin/orcamento_contexto.py:105`. |
| Derivação paralela | **PROCEDE.** A decisão sobre a dívida usa a origem do último marco, embora `clear_horas` preserve o fato que interessa: se houve `/clear` depois do aviso. A deduplicação de `CLEAR-NOITE` do ramo quebrado também cria uma regra própria sobre texto livre da trilha. O contador existente identifica o evento pelo terceiro campo e interpreta o carimbo. `app/docs/LEIS.md:60`, `bin/marco_de_clear.py:52`, `bin/marco_de_clear.py:57`, `bin/orcamento_contexto.py:184`, `bin/hook_stop_fila1.py:1090`, `bin/hook_stop_fila1.py:942`, `bin/disjuntor.py:2123`. |
| Selo ou RED sem mordida | **PROCEDE** para as lacunas dos casos novos. R14b cobre compact isolado e R14d cobre `/clear` isolado. Nenhum encadeia os dois antes do Stop. R9 exige `block` real, mas só cobre quatro Stops em série e a prova de `CLEAR-NOITE` pode usar a linha produzida no preparo do caso. R13 dá mordida ao M13 declarado pela AST, mas aceita a saída no segundo Stop e o AST aceita uma chamada em um ramo anterior sem provar que foi executada antes da gravação. Os RCs e o verde integral declarados ficam **[nao verificado]**. `bin/tests/test_orcamento_contexto.sh:651`, `:655`, `:660`, `:717`, `:737`, `:745`, `:763`, `:782`, `:823`, `:853`. |
| Ordem de trava | **PROCEDE** para a sequência ler–decidir–apendar no fallback. Dois Stops podem ler a mesma ausência da marca e ambos apendá-la. **NAO PROCEDE** para inversão de travas no caminho normal. A consulta de `sombra.lock` usa `LOCK_NB` e libera imediatamente a trava compartilhada quando consegue tomá-la. `bin/hook_stop_fila1.py:1002`, `:1009`, `:1011`, `:1295`, `:1297`, `bin/disjuntor.py:421`, `:422`. |
| Campo com dois significados | **PROCEDE** para `fim_visto` como registro do fato que ocorreu. A escrita antiga segue um handoff comprovado. A nova pode ocorrer antes do handoff, ao pedir o par. O leitor usa ambas para suprimir o próximo pedido, e a liberação continua comprovando o handoff separadamente. `aviso_desde` guarda o instante da emissão, enquanto o `desde HH:MM` apresentado pode ser o instante do disparo. A volta trata o primeiro como booleano. `bin/hook_stop_fila1.py:1126`, `:1133`, `:1152`, `:1160`, `:1169`, `:1221`, `:1224`. |

**PROCEDE — a forma dita única aceita hora inválida e prefixo truncado.** Com `aviso_desde` pendente, `AVISO: CLEAR devido desde 29:009` casa como `29:00` e aciona o pedido diurno. Reproduzi com a função da cópia em memória. O mesmo ramo anuncia “operador presente” quando o juiz retorna `None`, que abrange presença e “não sei”. `bin/hook_stop_fila1.py:868`, `:877`, `:1152`, `:1163`, `bin/orcamento_contexto.py:95`, `:104`, `:111`.

## 4. `except` e vocabulário

- **NAO PROCEDE — `except` novo.** O `except ValueError` de `ultima_fala` permanece e passa a zerar só `t`. O trecho novo do orçamento quebrado fica dentro do `except Exception as e2` já existente. O leitor novo da trilha deixa a falha subir para esse tratamento. `bin/disjuntor.py:1383`, `bin/disjuntor.py:1386`, `bin/hook_stop_fila1.py:932`, `bin/hook_stop_fila1.py:938`, `bin/hook_stop_fila1.py:1282`, `bin/hook_stop_fila1.py:1300`.
- **NAO PROCEDE — termo proibido encaminhado a `app/docs` ou ao RELATO pelo diff.** A lista literal é `post-it`, `postit`, `post it`, `bilhete`, `mural`, `recado`. A varredura usa limites de palavra e, pelo `RAIZ` calculado, começa em `app/`. Nenhum termo casou no texto novo destinado ao topo, nem na varredura de `app/docs` com a expressão do selo. `recado` aparece em comentários de `bin/` e no anexo TRIAGEM, cujo destino editorial é **[nao verificado]**. `app/pautas/tests/test_contract_vocabulario.py:17`, `:19`, `:29`, `:44`, `bin/hook_stop_fila1.py:866`, `:1029`, `:1163`, `bin/tests/test_orcamento_contexto.sh:511`.

## 5. Descartados da TRIAGEM

O veredito de cada linha responde se o diff cria ou piora o problema.

| Item | Veredito |
|---|---|
| Perna `prim` de R7 | **NAO PROCEDE** como regressão. A lacuna do selo existe: são dois `>` independentes e M7 troca só o de `ult`. R11 prova ausência com a cauda velha, sem testar a igualdade da perna `prim`. `bin/orcamento_contexto.py:105`, `:108`, `bin/tests/test_orcamento_contexto.sh:694`, `:869`. |
| Bordas do parser em R10 | **NAO PROCEDE** para o contador existente. R10 cobre janela e trilha ausente. O leitor novo dos 4 KiB tem outra regra de leitura e defeitos próprios. `bin/disjuntor.py:2123`, `:2127`, `bin/tests/test_orcamento_contexto.sh:666`, `bin/hook_stop_fila1.py:940`. |
| Delegação em R11 | **NAO PROCEDE.** `ultima_humana` lê a posição zero da própria tupla. R11 exige a delegação da janela a `DJ.ultima_fala` e M11 tenta substituí-la por leitura direta. `bin/disjuntor.py:1404`, `bin/orcamento_contexto.py:104`, `bin/tests/test_orcamento_contexto.sh:680`, `:877`. |
| Topo e concorrência com `gerar_cortes` e `deploy_agendado` | **PROCEDE** para o risco acrescido. Os escritores concorrentes já eram conhecidos, mas o diff passa a depender de o `AVISO` permanecer no topo para recuperar a dívida e adiciona outra leitura do topo no contido. A regravação integral a partir de uma leitura anterior pode perder a linha nova. Interleaving em produção **[nao verificado]**. `app/core/esteira_vigia.py:839`, `bin/gerar_cortes.py:117`, `:136`, `bin/deploy_agendado.sh:37`, `:44`, `bin/hook_stop_fila1.py:1152`, `:1295`. |
| Dois limites de 4 MiB | **NAO PROCEDE.** A cauda de fala classifica mensagens humanas e a de contexto procura uso do assistente. `bin/disjuntor.py:1365`, `:1393`, `bin/orcamento_contexto.py:65`, `:114`. |
| Relógio duplo do P14 | **PROCEDE.** A versão nova adiciona amostras e outra decisão na borda de hora ou de ausência. `bin/hook_stop_fila1.py:1286`, `:1294`, `:1296`, `bin/orcamento_contexto.py:99`, `:105`. |
| Rótulo `CLEAR-RAPIDO armado` sem disparo | **NAO PROCEDE** como regressão. A nota de ausência já podia alimentar `_LEMBRETE` sem disparo, e a saída final da fila usa o rótulo para a trilha. `bin/hook_stop_fila1.py:1125`, `:1236`, `:1524`. |
| Keepalive como fala humana | **NAO PROCEDE** como regressão. O texto que o keepalive digita pode passar pelo juiz existente se chegar ao transcript em forma aceita. A classificação desse texto antecede o diff. A gravação no transcript em produção é **[nao verificado]**. `bin/keepalive.sh:8`, `bin/disjuntor.py:1348`, `:1350`, `:1360`. |
| Loop no ramo quebrado | **PROCEDE.** Com a marca seguida de mais de 4 KiB de `FALHA`, o leitor volta a autorizar o aviso. Um evento intercalado como `OCUPADO` também faz isso. Se a trilha puder ser lida e o append falhar, a falha de gravação é engolida e a marca pode nunca chegar. `bin/hook_stop_fila1.py:895`, `:898`, `:940`, `:944`, `:1007`, `:1295`. |
| `AVISO` do ramo quebrado sem `aviso_desde` | **PROCEDE.** O diff passa a mandar publicar esse `AVISO`, mas a volta exige `aviso_desde`. Recuperar a leitura do estado, com o aviso já no topo e sem a marca, por si só não ativa o pedido. `bin/hook_stop_fila1.py:1152`, `:1295`, `:1299`. |
| Rearme após desfecho no mesmo ciclo | **PROCEDE.** O ramo de volta antecede a guarda de “sem redisparo” e pode substituir o disparo com `desfecho='seguiu'` ainda no mesmo `marco_ref`. Gera outro `DISPARO` na trilha. A atribuição desse incremento a `disjuntor.ler` **NAO PROCEDE**. `bin/hook_stop_fila1.py:859`, `:1066`, `:1155`, `:1158`, `:1208`, `:1219`, `bin/disjuntor.py:2117`, `:2142`. |
| `_iso` com carimbo não string | **NAO PROCEDE** como regressão. Os exemplos do anexo que levantam `ValueError` seguem como fala sem instante. A generalização para todo não string **NAO PROCEDE**: `_iso(20261126)` foi aceito em memória porque `_iso` converte a entrada em string, com

[truncado em 20000 de 20597 caracteres]

## 26/11 18:00 · sessao 77d3d0e10f4b · VIGIA-DISCO · verificador · origem=Code

## VIGIA-DISCO v3 — pré-verificação

Usei o diff fornecido e a cópia local, em modo somente leitura. Conferi a sintaxe dos dois scripts Bash e executei contraexemplos em memória. O selo completo cria e remove arquivos temporários, por isso ficou sem execução nesta leitura. Identidade Git da cópia, RED de 12 falhas informado na TRIAGEM, verde da suíte, instalação do timer e ocorrências em produção: [nao verificado]. `bin/tests/test_disco_estado.sh:28–47,613–627`

### 1. O que muda e quem escreve e lê

| Mudança | Escritores e leitores |
|---|---|
| **Retenção de `~/codex_ws/<sha>`** | O wrapper grava a cópia de `git archive` e tira a permissão de escrita. O Codex usa a cópia como diretório de trabalho. A retenção lê nome, idade, árvore e blobs, cadastro de worktrees e sinais de processo em `/proc`. Ela cria o nome `.rm.<sha>` com `rename`, altera permissões e remove a árvore. O v3 passa a recusar arquivo com mais de um hardlink, reconfere a árvore após o `chmod` e tenta devolver o nome original se a reconferência falhar. A árvore de referência vem de `git ls-tree` e os blobs da cópia são conferidos com `git hash-object`. `bin/codex_pergunta.sh:604–621,794`; `bin/retencao_disco.py:111–135,196–237,240–325` |
| **`logs/codex/chamadas.log`** | `codex_pergunta.sh` acrescenta `hora local`, `obra`, `papel`, `hash12`, `pid` e `ini`/`fim`. `ini` leva a pergunta raspada no 7º campo. `fim` leva sessão, rc e duração, com motivo opcional no 10º campo do construtor. A retenção lê hora, obra, hash, pid e evento para derivar último uso, obras por hash, último uso por obra e chamadas abertas. Guarda também o tamanho lido em bytes. `hash12_depois` relê o 4º campo a partir desse byte e agora recusa tamanho menor que o do plano. `codex_limite_estado.py` é o outro leitor do esquema. Seus resultados vão ao ESTADO, ao CODEX-ESTADO e ao disjuntor, que também cruza os `ini` de construtor com sua trilha de censo. `bin/codex_pergunta.sh:561–571,608–609`; `bin/retencao_disco.py:140–193,225–235,531–538`; `bin/codex_limite_estado.py:49–96,187–189`; `bin/gerar_estado.py:275–278`; `bin/gerar_codex_estado.py:176`; `bin/disjuntor.py:2125–2139,2224–2244` |
| **Logs de suíte e citações em `app/docs`** | O selo de ambiente da suíte imprime `AMBIENTE sha=` a partir de `HASNER_SHA`. A retenção lê a assinatura nos primeiros 256 KiB, além de idade, caminho e FDs. `pista_certifica.sh` também lê o log original e exige que o caminho informado exista. As citações são texto dos próprios documentos. A cópia tem, por exemplo, citação em `RELATO.md:30`. Entre os regravadores do RELATO estão `esteira_vigia`, `gerar_cortes` e `deploy_agendado`. A retenção varre `app/docs` recursivamente, no texto cru e decodificado de `%`, e usa o mesmo teste de caminho relativo à raiz, relativo a `logs/` ou basename no plano e no ato. Ela escreve o temporário gzip e o publica com `link`, confere seu conteúdo descomprimido e retira o nome original. Outros produtores de documentos e consumidores de logs vivos: [nao verificado]. `app/core/tests/test_ambiente_do_run.py:50–56`; `app/docs/RELATO.md:30`; `app/core/esteira_vigia.py:915–927`; `bin/gerar_cortes.py:114–136`; `bin/deploy_agendado.sh:31–45`; `bin/pista_certifica.sh:22–30,41–55`; `bin/retencao_disco.py:360–413,426–481,542–546` |
| **`logs/disco/retencao.tsv`** | `retencao_disco.py --aplicar` grava `epoch, produtor, acao, n, bytes`. Nas linhas `df antes/depois`, `n` é o percentual de `/` e `bytes` é o uso medido. `resumo()` lê o último grupo contíguo de mesmo epoch. O `--carimbo` usa o epoch para espaçar passadas. `disco_estado` usa os itens para o resumo na foto e lê o par de percentuais para o topo do RELATO. `bin/retencao_disco.py:486–491,506–517,548–610,628–642`; `bin/disco_estado.py:264–270,284–294,381–406` |
| **Fotografia, tendência e consumidores** | `disco_estado.py --gravar` escreve `df.tsv` com epoch, usado, livre e percentual de `/`. Seu leitor de tendência usa epoch e usado. O mesmo processo escreve e lê `consumidores.tsv`, com epoch, flag de leitura parcial e pares categoria/KiB. Os dados vêm de `df` de `/` e do volume do Postgres, `du` e Docker. `linha.md` recebe epoch, nível e corpo renderizado. `gerar_estado.py` lê a foto, calcula sua idade e escreve a seção Disco. `relato.sh` encaminha ESTADO e RELATO para publicação pelo raspador. `bin/disco_estado.py:63–102,147–217,222–265,297–375,468–482`; `bin/gerar_estado.py:320–321,336–338,393–395,414`; `bin/relato.sh:44–49,125–131`; `bin/raspar_publico.py:28–40` |
| **Linha `DISCO-VIGIA` no topo do RELATO** | `linha_topo()` compõe nível, percentuais medidos, indicação de dispositivo comum, limiares e par `df` da retenção. `OK` retira a linha. `NAO MEDIDO` usa a forma `AVISO`. O v3 troca a frase quando falta o par `df`. `no_topo_disco()` decide a posição e chama a trava e a gravação de `esteira_vigia`. O Stop lê a primeira linha útil para o painel e as duas primeiras para sua janela. B3 usa a mesma janela e também lê o `mtime` do arquivo. O disjuntor escreve a faixa de censo e a seção de alarmes pelas portas de `esteira_vigia`. O checador de afirmações lê o RELATO no Stop e na publicação. `bin/disco_estado.py:378–453`; `app/core/esteira_vigia.py:846–927`; `bin/hook_stop_fila1.py:408–416,446–484,1249–1269,1313–1325`; `bin/disjuntor.py:288–332,765–793,1030–1042`; `bin/relato.sh:65–69` |
| **Alarme de ociosidade e handoff** | O alarme lê as primeiras 40 linhas físicas do RELATO, exclui a linha do disco por `LINHA_TOPO` e usa a prova do Stop para `PAREI` de censo. O resultado `parei` entra na decisão de disparo, na linha lida pelo placar e no push do cron. O handoff usa `LINHA_TOPO` e escreve `- disco: …` fora dos cortes de quatro itens e 220 caracteres, quando encontra a linha dentro de seu próprio recorte. `handoff_sessao.sh` escreve `HANDOFF-SESSAO.md`, o `SessionStart` lê seu conteúdo e o Stop consulta seu `mtime` para CLEAR. `bin/alarme_sessao_ociosa.py:33–45,119–169`; `bin/placar_code.sh:368–369`; `app/config/crons.py:311`; `bin/handoff_sessao.sh:85–146,328–339`; `.claude/settings.json:25–54`; `bin/hook_stop_fila1.py:929–933,1063–1070` |
| **Selo** | A régua chama os selos de host. Este cria fixtures em diretório temporário, executa módulos reais e agora também o handoff real, grava saídas e cópias mutadas, e exige falha de caso sem queda de seção para cada mutante. O shell valida o `mktemp` principal. Dois lançamentos Python ganharam `-B`. O selo usa `retencao.tsv`, `chamadas.log`, documentos e logs sintéticos nos casos correspondentes. `bin/regua.sh:134–145`; `bin/tests/test_disco_estado.sh:28–47,66–69,134–161,298–326,519–575,613–650` |

### 2. Reconferência entre plano e ato

- **PROCEDE — gzip pode retirar uma fonte que mudou durante a nova releitura.** O último `lstat` e a amostra de inodes ficam antes da varredura recursiva de `app/docs`. Se o produtor acrescentar bytes à fonte ou trocar o arquivo durante a varredura, `unlink(a)` ainda pode rodar sobre o nome. O gzip contém a versão conferida antes da mudança. Uma citação criada depois de o respectivo documento ter sido lido também pode escapar. A releitura cobre a citação que chegou durante a compressão e aumenta o intervalo entre o último exame da fonte e seu `unlink`. `bin/retencao_disco.py:360–373,442–473,542–546`; `bin/tests/test_disco_estado.sh:136–161`
- **PROCEDE — a prova de ausência de citação aceita leitura vazia ou parcial.** Confirmei em memória que um único arquivo de zero bytes gera `'\n'`, e `citado_no_texto()` libera um nome ausente desse texto. Arquivos ignorados por falta de acesso também somem da decisão quando outro arquivo fornece texto. Se um arquivo desaparecer entre o teste e `open()` na releitura, a exceção pode deixar o `.gz` já publicado ao lado da fonte. A próxima passada recusa o destino existente. `bin/retencao_disco.py:360–388,430–434,461–481`
- **PROCEDE — `rm` tem reconferência de árvore e ainda aceita entrada tardia.** A segunda caminhada vem após `chmod`, seguida por `rm -rf`. Um arquivo criado depois de sua pasta ter sido enumerada pode ficar fora da lista de hashes e entrar no `rm`. A trilha e os processos também foram consultados antes do `chmod` e da segunda caminhada. Um hardlink que apareça após o primeiro `lstat` pode sofrer `chmod` antes da segunda recusa. Quando a reconferência falha e o nome volta, o código devolve o nome sem restaurar as permissões. O wrapper reutiliza a pasta existente. `bin/retencao_disco.py:263–290,301–325`; `bin/codex_pergunta.sh:611–621`
- **PROCEDE — tamanho menor cobre só uma forma de rotação.** O teste de tamanho usa o caminho antes de abrir o arquivo. Uma substituição de tamanho igual ou maior, ou truncamento entre `getsize` e `open`, pode fazer a releitura começar num arquivo diferente ou devolver conjunto vazio. O ramo `.rm.` não faz essa releitura. `bin/retencao_disco.py:152–193,301–324`
- **PROCEDE — há regravações com garantias diferentes.** O RELATO é lido e substituído sob a trava do escritor cooperante. Um escritor direto pode atuar no intervalo. A foto e o carimbo usam `os.replace` sob a trava do disco. No handoff, o ramo acima de 60 linhas trunca o destino e depois acrescenta o aviso. STOP/CLEAR pode consultar o `mtime` desse arquivo durante a publicação. O selo apaga seu diretório temporário validado. `bin/disco_estado.py:210–217,345–352,422–453,475–481`; `app/core/esteira_vigia.py:846–862,915–927`; `bin/gerar_cortes.py:117–136`; `bin/handoff_sessao.sh:328–339`; `bin/hook_stop_fila1.py:929–933,1063–1070`; `bin/tests/test_disco_estado.sh:34–35`

### 3. Classes de reprovação

| Classe | Achado |
|---|---|
| **Segundo juiz de citação ou da forma da linha do disco** | **NAO PROCEDE.** Plano e ato chamam `citado_no_texto`. Escritor, alarme e handoff usam `LINHA_TOPO`. Os limites de leitura continuam próprios de cada chamador. `bin/retencao_disco.py:381–410,542–546`; `bin/disco_estado.py:378,426–428`; `bin/alarme_sessao_ociosa.py:119–140`; `bin/handoff_sessao.sh:87–111` |
| **Segundo juiz da janela do Stop** | **PROCEDE, presente na base do diff.** O escritor usa `_util_do_topo` e `_eh_faixa_censo`, mas refaz a contagem e impõe o primeiro `## ` cru como fim. O `_topo()` do Stop aceita `## MARCO` como segunda linha útil quando vem logo após um painel. Confirmei esse caso em memória. O escritor, diante do mesmo texto, retorna `sem-janela`. `bin/disco_estado.py:422–447`; `bin/hook_stop_fila1.py:462–499` |
| **Segundo recorte do topo no handoff e no alarme** | **PROCEDE no v3.** Com 38 linhas de censo, painel e pergunta, o escritor insere o disco na 41ª linha contada pelo handoff. Confirmei em memória que `AVISO` some e `PAREI` volta ao filtro genérico, onde pode sumir. O handoff também encerra em `## ` indentado, pois faz `strip()`. No alarme, confirmei que inserir o disco antes de uma trava que estava na linha física 40 a empurra para 41. O disco é ignorado como parada, mas continua consumindo uma das 40 linhas e a trava legítima deixa de ser vista. `bin/disco_estado.py:426–447`; `bin/handoff_sessao.sh:97–123`; `bin/alarme_sessao_ociosa.py:119–145` |
| **Segundo escritor e ordem de trava** | **NAO PROCEDE como nova porta de escrita do RELATO.** A linha do disco usa o mesmo `flock` no diretório e o mesmo `_grava_relato` do censo e dos alarmes. **PROCEDE como corrida herdada** com `gerar_cortes` e `deploy_agendado`, que regravam o arquivo inteiro fora desse `flock`. A ordem visível é `relato.lock` → `disco.lock` → trava do diretório para o topo. O atalho por `VIGIA_DISCO_TRAVA` confere o valor do ambiente, sem conferir posse. `bin/relato.sh:27,37–49`; `bin/retencao_disco.py:613–625`; `bin/disco_estado.py:451–453,475–481`; `app/core/esteira_vigia.py:839–862,890–927`; `bin/gerar_cortes.py:117–136`; `bin/deploy_agendado.sh:31–45` |
| **Relógio fora da borda** | **NAO PROCEDE como acréscimo do v3.** A passada recebe a hora usada para idade e para carimbar o grupo. O orçamento usa `monotonic` injetável, verificado entre operações. **PROCEDE a divergência de interpretação já existente** entre hora civil do wrapper, hora local usada no censo e a conversão conservadora UTC+12 da retenção. A hora do `df` final e o instante do início da passada também são coisas distintas, embora as linhas recebam o mesmo carimbo do grupo. Fuso vivo: [nao verificado]. `bin/codex_pergunta.sh:570–571,608–609`; `bin/retencao_disco.py:140–149,520–527,548–587,633–642`; `bin/codex_limite_estado.py:55–67`; `bin/disjuntor.py:2232–2237` |
| **Derivação paralela de `chamadas.log`** | **PROCEDE.** A retenção aceita seis campos e liga `ini`/`fim` por `(hash12, pid)`. O leitor do ESTADO exige pelo menos sete no `ini` e nove no `fim`, com chave `(obra, hash12, pid)`. Confirmei em memória que um `fim` de seis campos tira o hash de `abertos` mesmo com PID vivo, enquanto o outro leitor rejeita esse `fim`. O selo de retenção fabrica `fim` de sete campos. A releitura no ato tem ainda outro recorte e extrai o 4º campo de qualquer linha com quatro ou mais campos. `bin/retencao_disco.py:159–193,225–229`; `bin/codex_limite_estado.py:49–68`; `bin/codex_pergunta.sh:561–571`; `bin/tests/test_disco_estado.sh:302–326` |
| **Selo sem mordida** | **NAO PROCEDE para o caso AVISO com/sem passada.** Confirmei em memória que o `**FEITO**` acrescentado às duas variantes é pego pelo juiz real. **PROCEDE na cobertura do handoff.** O teste novo executa o script real, mas seu `PAREI` tem 218 caracteres, a linha é inserida antes das quatro `TRAVA-*` e o teste só exige `TRAVA-0`. Ele não pega uma regressão que recorte a saída do disco em 220 caracteres ou que erre na fronteira de 40. O teste local que replica o filtro antigo ainda existe. **PROCEDE no isolamento do selo.** Limpar `GIT_*` não impede a descoberta de um repositório pai se `TMPDIR` estiver dentro dele. Nesse caso, a chamada nova ao handoff pode executar `git fetch` no pai. `bin/relato_afirma_com_prova.py:30–50`; `bin/disco_estado.py:389–410`; `bin/tests/test_disco_estado.sh:34,506–508,519–528,538–575,665–668`; `bin/handoff_sessao.sh:17,30–35,97–121` |
| **Campo com dois significados** | **PROCEDE, herdado.** `n` representa quantidade em produtores comuns, quantidade de volumes em `volumes_orfaos` e percentual em `df`. `bytes` representa espaço ocupado da fonte para logs, uso do filesystem para `df` e recuperação reportada para o prune. `produtor`/`acao` já permitem discriminar, mas o leitor genérico do ESTADO renderiza `df` no mesmo formato numérico de uma ação comum e o topo exibe `-1` como `-1%`. A frase nova também diz que nenhuma passada com `df` foi registrada quando o último grupo contém só um lado do par. O leitor consulta apenas o último grupo. `bin/retencao_disco.py:512–517,554–583,594–610`; `bin/disco_estado.py:284–294,381–407` |

### 4. `except` e vocabulário

- **NAO PROCEDE — `except` novo.** Os trechos acrescentados não introduzem captura. Há novos pontos de falha sem captura: a importação de `disco_estado` no alarme, a importação no handoff quando o módulo é encontrado mas falha ao carregar, e a leitura de documentos após publicar o `.gz`. `bin/alarme_sessao_ociosa.py:129–140`; `bin/handoff_sessao.sh:87–91`; `bin/retencao_disco.py:360–373,464–481,542–546`
- **NAO PROCEDE — vocabulário proibido nos textos novos.** Busquei os termos com a mesma fronteira de palavra do teste nos cinco arquivos tocados. A frase nova do RELATO e o prefixo `- disco: ` do handoff passam. A raiz efetiva da varredura do contrato é `app/`, inclui `app/docs/*.md` e fica abaixo do `bin/` da raiz do repositório. Conteúdo gerado em produção: [nao verificado]. `app/pautas/tests/test_contract_vocabulario.py:17–46`; `bin/disco_estado.py:395–410`; `bin/handoff_sessao.sh:118–123`

### 5. Descartados da TRIAGEM

| Descartado | Julgamento |
|---|---|
| Stop/B3 não usam a parada do disco | **NAO PROCEDE como defeito da exclusão.** As decisões tratam item em curso e liberação de obra. **PROCEDE como efeito indireto herdado:** B3 usa o `mtime` do RELATO inteiro para datar uma linha de liberação. Uma regravação só do disco pode atualizar esse `mtime` sem mudar a pergunta que B3 examina. `bin/hook_stop_fila1.py:1237–1302`; `bin/disjuntor.py:765–793,816–833,1030–1042`; `bin/disco_estado.py:448–453`; `app/core/esteira_vigia.py:922–927` |
| Unidades, relógio ajustado, teto cooperativo, ENOSPC | **PROCEDE como limitação herdada.** O v3 acrescenta uma varredura de documentos por gzip e uma segunda conferência de árvore sem consulta ao orçamento dentro desses atos. Uma falha no meio pode deixar atos efetuados fora de `tot`, pois o total do produtor entra após o laço. Duração e falhas no vivo: [nao verificado]. `bin/retencao_disco.py:263–290,319–324,360–373,542–587` |
| `logs/entrega/ensaio.log`, FDs e `pista_certifica` | **NAO PROCEDE como regressão do filtro lexical ou do certificador.** O padrão de caminho e a exigência do arquivo original já estão no código fora do delta. **PROCEDE a piora da janela de FD/fonte no v3**, pela varredura posterior à amostra. A reversibilidade do gzip cobre seus próprios bytes. A afirmação de que todo recibo necessário está citado: [nao verificado]. `bin/retencao_disco.py:78–89,117–123,405–412,442–473`; `bin/pista_certifica.sh:22–30` |
| `.rm.` pula idade/obra/trilha e nome original reaparece | **PROCEDE como lacuna herdada.** Qualquer diretório compatível com o nome e com a prova de subconjunto pode entrar no ramo `.rm.`. O código não registra a origem daquele nome. **PROCEDE como novo caminho de falha** o `rename` de volta depois do `chmod`: se o wrapper já recriou o destino com conteúdo, a devolução pode falhar e o resto ficar com permissões alteradas. `bin/retencao_disco.py:207–234,301–325`; `bin/codex_pergunta.sh:611–621` |
| Foto nova com topo velho, `sem-janela`, epoch futuro, `²`, cabeçalho divergente, erro de abertura e `-1%` | **PROCEDE como defeitos ou diagnósticos herdados.** A foto é publicada antes do topo e a CLI retorna 0 em `sem-janela`. `isdigit()` aceita `²`, que falha em `int()`. Epoch futuro mostra idade zero e o nível do cabeçalho não é confrontado com o corpo. O `open()` pode propagar erro e o par pode renderizar `-1%`. **PROCEDE como regressão do v3** a perda da linha no handoff e da trava legítima no alarme por fronteira de recorte. `bin/disco_estado.py:358–375,381–407,422–453,478–482`; `bin/gerar_estado.py:320–321`; `bin/handoff_sessao.sh:97–123`; `bin/alarme_sessao_ociosa.py:119–145` |
| Regravadores do RELATO, `TRAVA_ENV`, cobertura parcial e L-105 | **PROCEDE como dívida herdada** nos regravadores diretos e na prova de posse da trava. **PROCEDE na cobertura:** `ws_dentro` remove duas guardas de uma vez, `ws_48h` testa a borda do `mtime`, e `finally_reg` testa queda com o par de `df`, sem exercitar uma remoção seguida por ENOSPC no mesmo produtor. Sobre L-105, o código do timer chama `--aplicar` pelo serviço, e esse caminho não consulta certificado de suíte. Pouso efetivo e cumprimento do pré-requisito: [nao verificado]. `bin/gerar_cortes.py:117–136`; `bin/deploy_agendado.sh:31–45`; `bin/retencao_disco.py:554–587,613–625`; `bin/tests/test_disco_estado.sh:599–605,701–715`; `app/docs/LEIS.md:131`; `bin/hasner-relato.timer:4–8`; `bin/hasner-relato.service:12–15`; `bin/relato.sh:44–46` |

**Veredito final — REPROVA.** A releitura de citações alonga a janela em que a fonte pode mudar antes do `unlink`. A reconferência da cópia ainda deixa o `chmod` produzir efeitos num caminho recusado. O v3 também introduz perdas de informação nos limites do handoff e do alarme. A divergência de esquema de `chamadas.log` afeta diretamente a prova de que uma cópia está livre para remoção. `bin/retencao_disco.py:159–193,301–325,464–473`; `bin/codex_limite_estado.py:49–68`; `bin/handoff_sessao.sh:97–123`; `bin/alarme_sessao_ociosa.py:119–145`

## 26/11 17:58 · sessao 3146f4769443 · DIA-DA-BATIDA-JUIZ-UNICO · leitura · origem=Code

## Triagem F4d

**B1 — PROCEDE.** A F4d colocou a regularização e `apagar` na mesma ordem, **Colaborador antes de Batida**. A regularização trava o Colaborador no início do `atomic`; `apagar` faz o mesmo antes de reler e travar a Batida. O teste atual confirma a chamada local em `corrigir` e `remover`, com escritas simuladas e sem chamado de lote. A reprodução do ciclo na F4c fica [nao verificado]. `app/chamados/services/regularizacao_ext.py:122–157` · `app/ponto/registro_batida.py:328–336` · `app/ponto/tests/test_juiz_unico_eventos.py:409–432`

| Caminho | Ordem conferida |
|---|---|
| **Flip automático × regularização** | **T1** abre o `atomic` em `flip_automatico --apply`, entra em `flip_tipo` → `anotar` e segura a Batida **B** pelo `UPDATE`. O `post_save` chama o reconciliador, que pede o Colaborador **C** se o seletor encontrar pergunta elegível. **T2** segura **C** no materializador e pede **B** ao corrigir a mesma linha. Fecha **T1: B → C / T2: C → B**. Uma pergunta elegível de disputa fechada pode passar pelo seletor e também pela guarda do lote. `app/ponto/management/commands/flip_automatico.py:62–68` · `app/ponto/services/flip_batida.py:25–33` · `app/ponto/registro_batida.py:281–287` · `app/ponto/signals.py:216–228` · `app/chamados/reconciliador.py:673–694,740–762,1580` · `app/chamados/services/regularizacao_ext.py:59–66,122–123,153–157` |
| **Pergunta → Chamado no mesmo lote** | **T1** segura **C**, corrige ou adiciona uma Batida e, no `post_save`, pode travar Disputas, Chamados e a Pergunta **P**. No final, `encerrar` pede o Chamado **Ch** do lote explicitamente. Se **Ch** for terminal, estiver fora dos pais e destinos selecionados e ainda estiver livre, a aquisição nova é **P → Ch**. A ordem contrária existe na porta canônica. Dois atos que adquirem primeiro o mesmo **C** ficam serializados, portanto esta evidência confirma a **inversão**. Um deadlock de duas sessões para esse par fica [nao verificado]. `app/chamados/services/regularizacao_ext.py:73–83,153–157,203–207` · `app/chamados/reconciliador.py:686–694` · `app/chamados/services/fecho_runtime.py:157–168` · `app/chamados/models.py:761–777,1779–1793,2942–2966` · `app/chamados/catalogo/motor.py:24–31` |
| **Batida de chão × regularização** | **T1** segura **C** em `FOR UPDATE`. **T2**, pelo registro do app, pode reservar a chave única **K = (C, timestamp)** no `INSERT` e esperar `KEY SHARE` em **C** na FK. Se **T1** corrige outra Batida para **K** nesse intervalo, espera a chave de **T2**. Fecha **T1: C → K / T2: K → C**. Os dois kernels passam o instante efetivo à mesma porta, e a hora aceita do aparelho pode coincidir com a hora corrigida. É um ciclo inferido do código e do modo de trava. A execução desse interleaving no PostgreSQL fica [nao verificado]. `app/api/views_core.py:492–495,643–659` · `app/api/views.py:652–668` · `app/api/hora_do_aparelho.py:135–146` · `app/ponto/registro_batida.py:146–148,178–182` · `app/ponto/models.py:22,102–106` · `app/chamados/services/regularizacao_ext.py:122–123,144–159` · `app/chamados/models.py:2938–2941` · `app/colaboradores/tests/test_feriado_matriz_p3_trava.py:5–15` |

**B1, guarda do lote — PROCEDE.** O teste de disputa aberta roda antes da trava de **C**. A porta de abertura pode adquirir **C**, criar a disputa e confirmar a transação nesse intervalo. O materializador adquire **C** depois e segue com o resultado antigo da guarda. `app/chamados/services/regularizacao_ext.py:59–66,122–124` · `app/chamados/services/disputa_emissao.py:530–542,601–606`

**Escolha — (a) ordem global, Colaborador → Batida.** Em `flip_batida.py`, adquirir **C** antes de `anotar` quando o flip estiver em transação. Em `regularizacao_ext.py`, ler a guarda sob **C** e antecipar as Disputas e os Chamados que o lote e suas cascatas podem tocar, incluindo o Chamado terminal do lote, antes da primeira Pergunta. Em `chamados/models.py`, usar `FOR NO KEY UPDATE` só para a linha do Colaborador na porta existente. Esse modo mantém a exclusão entre os atos que travam **C** e permite o `KEY SHARE` da FK do chão. O custo é uma aquisição antecipada no flip transacional, mais linhas retidas pelo lote e revisão dos selos que reconhecem o SQL da trava. O `INSERT` do chão ganha zero consultas por essa troca de modo. Contenção e latência [nao verificado]. `app/ponto/services/flip_batida.py:21–33` · `app/chamados/services/regularizacao_ext.py:59–66,122–124,203–207` · `app/chamados/models.py:2901–2922,2938–2966` · `app/colaboradores/services/feriado_do_posto.py:319–324` · `app/ponto/tests/test_selo_chokepoint_escrita.py:379–402`

**Custo de (b), ponto único de serialização para todos os atos.** `trava_na_ordem` já é o ponto único de aquisição explícita para Colaborador, Disputa, Chamado e Pergunta. Fazer todos os atos de Batida passarem por uma mesma seção crítica exigiria migrar o `UPDATE` e o `DELETE` diretos da regularização, além de levar o `INSERT` do app para dentro dessa seção se ela também cobrisse o chão. Isso amplia a duração da transação do app e o alcance da mudança sobre receivers síncronos. A ordem inversa, Batida → Colaborador, também exigiria rever `apagar` e as cascatas que já começam pelo Colaborador, além de lidar com o `INSERT` antes de existir uma linha de Batida. `app/chamados/models.py:2901–2922` · `app/chamados/services/regularizacao_ext.py:133–157` · `app/ponto/registro_batida.py:172–182,328–341` · `app/ponto/signals.py:38–41,141–150,216–228` · `app/docs/LEIS.md:78`

**B2 — PROCEDE.** A leitura da marca viva está no `atomic` externo, antes do `delete` e da trilha. A proteção restante começa depois do `delete`. A view do DP captura `DoesNotExist` na consulta inicial e só responde sucesso depois de `apagar`. O laudo e a entrega registram falha de importação antes das asserções. O teste da cópia usa ORM em memória e conexão dublada, mesmo quando importa o `atomic` real. O RED por asserção, o GREEN com banco real e a resposta HTTP em produção ficam [nao verificado]. `app/ponto/registro_batida.py:318–343` · `app/ponto/views.py:467–500` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134,225–232`

**B3, K3 e E5 — PROCEDEM como verificações do laudo.** O INFO registra o contador devolvido pelo fechamento, inclusive `0`. K3 cobre uma exceção que sobe ao callback e permite a tentativa pelo outro dia da mesma competência. E5 retirou a promessa de recálculo pelo cron, e a configuração confirma que o comando fica sem cron. A execução dos mutantes consta nos anexos e fica [nao verificado] nesta leitura. `app/ponto/services/fechamento.py:637,1326–1335,1817,1830–1850` · `app/ponto/tests/test_juiz_unico_eventos.py:462–489` · `app/ponto/registro_batida.py:187–194` · `app/config/crons.py:1148–1155`

**AST — PROCEDE na fonte atual.** Conferi `4/2/9/0/0` handlers e zero chamadas a `suppress` nos cinco arquivos do delta citados pelo laudo, na mesma ordem. A cópia está sem metadados Git acessíveis. A identidade de `HEAD` e a comparação dos totais contra a base ficam [nao verificado]. `app/chamados/services/regularizacao_ext.py:104,112,158,191` · `app/ponto/registro_batida.py:18,200` · `app/ponto/services/fechamento.py:594,1105,1111,1262,1321,1327,1685,1846,1854` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:189–236` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360`

## Texto pronto de `brief_f4e.md`

```markdown
# DIA-DA-BATIDA-JUIZ-UNICO F4e -- brief do construtor (base 01f4a81a2)

## CENSO

leitores:
- `app/chamados/services/regularizacao_ext.py:37::materializar_regularizacao_externa` — estado do lote, disputa aberta, itens, Batidas e Chamado do lote.
- `app/ponto/management/commands/flip_automatico.py:21::Command.handle` e `app/ponto/services/flip_batida.py:10::flip_tipo` — fila, marca, competência, retratação e tipo. O outro chamador transacional já trava o dono em `app/ponto/services/veredito_celula.py:229::veredito_celula`.
- `app/api/views_core.py:374::api_bater_ponto` e `app/api/views.py:388::api_bater_ponto` — instante efetivo e registro do app. `app/ponto/registro_batida.py:120::registrar_batida` lê dedup e colisão antes do INSERT.
- `app/ponto/signals.py:19::_recompute_around`, `app/ponto/signals.py:113::_dias_batida`, `app/ponto/signals.py:217::_reconciliador_post_save_batida`, `app/chamados/reconciliador.py:740::qs_perguntas_absolviveis` e `app/chamados/services/trava_do_colaborador.py:37::colaboradores_travados`.
- `app/ponto/views.py:453::batidas_offline_pendentes`, `app/ponto/turnos.py:3037::julgar_evento_da_batida` e `app/ponto/services/fechamento.py:1821::recalcular_por_evento` — rejeição do DP, dias julgados e competência do fato.

escritores:
- `app/chamados/models.py:2901::PerguntaDisputa.trava_na_ordem` — trava Colaborador, Disputa, Chamado e Pergunta. `app/chamados/services/trava_do_colaborador.py:54::_registrar_travados` registra a aquisição na conexão.
- `app/chamados/services/regularizacao_ext.py:37::materializar_regularizacao_externa` — trava o dono, delega INSERT, faz UPDATE/DELETE direto e grava itens e lote. `app/chamados/services/disputa_emissao.py:510::criar_ou_atualizar_disputa` — trava o mesmo dono antes de abrir ou atualizar disputa.
- `app/chamados/reconciliador.py:629::retratar_perguntas_absolvidas_por_celula` — trava o dono, relê e trava Disputas, Chamados e Perguntas antes de carimbar. `app/chamados/models.py:1779::ChamadoColaborador.encerrar` — pede a trava do Chamado do lote por `app/chamados/models.py:761::ChamadoColaborador._recarregar_estado_se_mudou`.
- `app/ponto/registro_batida.py:120::registrar_batida`, `app/ponto/registro_batida.py:264::anotar` e `app/ponto/registro_batida.py:318::apagar` — INSERT, UPDATE e trava seguida de DELETE. `app/ponto/services/flip_batida.py:10::flip_tipo` escreve por `anotar` dentro do `atomic` de `app/ponto/management/commands/flip_automatico.py:65::Command.handle`.
- `app/ponto/services/fechamento.py:468::recalcular_fechamento_mes` — escritor mensal chamado pelo callback em `app/ponto/services/fechamento.py:1841`.

decisoes:
- Colaborador antes de Batida nos atos transacionais deste recorte. Disputa → Chamado → Pergunta nas novas aquisições da regularização. A porta existente continua sendo o ponto de aquisição das quatro entidades. Só Colaborador passa a `FOR NO KEY UPDATE`, inclusive nas chamadas reentrantes, para admitir o `KEY SHARE` da FK. `app/chamados/models.py:2901–2922,2938–2966` · `app/colaboradores/services/feriado_do_posto.py:319–324`.
- Fila herdada: B2-chão, sobre a proteção do julgamento em `registrar`/`anotar`; corrida aprovar × rejeitar; migração da correção e remoção para a porta da Batida; tratamento interno de falhas do escritor mensal. `app/ponto/registro_batida.py:172–182,197–212,264–297` · `app/ponto/views.py:474–500` · `app/chamados/services/regularizacao_ext.py:133–157` · `app/ponto/services/fechamento.py:1321–1335`.

perguntas de lei: 0

## TAREFA

1. **RED primeiro em PostgreSQL.** Criar `TransactionTestCase` com dois participantes, conexões independentes e PIDs conferidos. Sincronizar por eventos antes/depois do SQL real. Usar prazo de segurança e encerrar ambas as sessões. Observar SQLSTATE e aquisição efetiva, inclusive se o receiver capturar a exceção. O banco, as travas, o `save` e os signals rodam de verdade. Timeout, import quebrado e chamada de mock não contam como RED. `app/config/settings/ci.py:6–18` · `app/colaboradores/tests/test_feriado_matriz_p3_trava.py:54–102` · `app/ponto/signals.py:216–228`.

   - **Flip × correção.** Executar `flip_automatico --apply` e `materializar_regularizacao_externa` sobre a mesma Batida, com pergunta elegível de disputa fechada e competência aberta. Na base, pausar o flip após o `UPDATE` e deixar a regularização segurar o Colaborador antes de ambos avançarem. Exigir o ciclo Batida ↔ Colaborador e registrar `40P01`. No GREEN, o segundo ato espera pelo Colaborador antes de tocar a Batida. Conferir estado final e trilha dos atos. Mutante: retirar a aquisição antecipada do flip. `app/ponto/management/commands/flip_automatico.py:62–81` · `app/ponto/registro_batida.py:284–297` · `app/chamados/reconciliador.py:673–694,740–762` · `app/chamados/services/regularizacao_ext.py:122–166`.
   - **Pergunta × Chamado terminal do lote.** Usar um Chamado terminal real fora dos pais e destinos travados pela pergunta da fixture. Pausar depois da trava real da Pergunta. A segunda conexão sonda aquele Chamado com `FOR UPDATE NOWAIT`. Na base o Chamado ainda pode ser adquirido e a primeira transação só o pede no fechamento. No GREEN, o Chamado já está travado quando a Pergunta é adquirida. Mutante: omitir o Chamado terminal da aquisição antecipada. `app/chamados/reconciliador.py:686–694` · `app/chamados/services/regularizacao_ext.py:203–207` · `app/chamados/models.py:773–777,2952–2966` · `app/chamados/tests/test_e1_v5_ordem_trava.py:450–487`.
   - **INSERT do chão × correção para a mesma chave.** Manter o registro do app em autocommit, com instante livre nas leituras iniciais. Pausar a regularização após travar o Colaborador e antes de atualizar outra Batida para aquele instante. Na base, observar a espera da FK do `INSERT` pelo Colaborador e liberar a correção para fechar o ciclo. No GREEN, usar a conclusão do `INSERT` como marco, antes dos receivers seguintes. Conferir a Batida do app e o tratamento de duplicata já existente no materializador. Mutante: restaurar `FOR UPDATE` do Colaborador. `app/api/views_core.py:643–659` · `app/ponto/registro_batida.py:133–148,178–182` · `app/ponto/models.py:22,102–106` · `app/chamados/services/regularizacao_ext.py:144–160` · `app/chamados/models.py:2938–2941`.
   - **Guarda da disputa.** Abrir a disputa pela porta real enquanto o materializador tenta adquirir o mesmo Colaborador. No GREEN, depois de adquirir a trava, o materializador vê a disputa confirmada e veta o lote antes de escrever Batida. Mutante: voltar a leitura da guarda para antes da trava. `app/chamados/services/disputa_emissao.py:530–542,601–606` · `app/chamados/services/regularizacao_ext.py:59–66,122–124`.

2. **Aplicar a ordem.** Em `flip_tipo`, sob transação, adquirir Colaborador pela porta existente antes de ler o estado que decide a escrita e antes de `anotar`. Usar o estado vivo sob a trava nas guardas e na trilha do flip. Preservar o caminho em autocommit e a transação por item do comando. No materializador, conservar Colaborador como primeira trava, mover para baixo dela a guarda existente e adquirir Disputas e Chamados alcançáveis pelo lote antes da primeira Pergunta. Incluir o Chamado do lote mesmo terminal e as linhas alcançáveis pelas cascatas após o `save`, incluindo disputas fechadas elegíveis. Fazer a aquisição por grupo, em ordem de PK, pela porta existente. Usar `select_for_update(no_key=True)` apenas na consulta do Colaborador. Adaptar os seletores de SQL dos selos para reconhecer o novo modo e continuar cobrando ordem e exclusão. `app/ponto/services/flip_batida.py:10–40` · `app/ponto/management/commands/flip_automatico.py:62–81` · `app/chamados/services/regularizacao_ext.py:59–66,122–207` · `app/chamados/reconciliador.py:673–694,740–762,1704–1740` · `app/chamados/models.py:2926–2966`.

3. **Fechar a evidência de B2 administrativo.** Em `TransactionTestCase`, fazer a chamada viva ao juiz executar SQL que falha na conexão real, por exemplo `SELECT 1/0`, apenas nessa chamada. Chamar `apagar` real. Exigir propagação do `DatabaseError`, Batida e PK preservados, zero trilha e zero recálculo, com a conexão utilizável após rollback. O GREEN é a F4d. Para o RED, usar o bloco anterior ou restaurar proteção + savepoint só na leitura viva. Exigir falha na asserção do teste. Manter o teste de memória e a proteção que segue depois do DELETE. `app/ponto/registro_batida.py:328–343` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134`.

4. Entregar saídas da base, do GREEN e dos mutantes por caso, os testes dos dois kernels do app, a medição do teto da porta e a contagem AST contra a base. Usar `PYTHONDONTWRITEBYTECODE=1`. Evidência executável desta F4e [nao verificado]. `CLAUDE.md:28–29,392–402` · `app/api/tests/test_l030_batida_pela_porta.py:1–5` · `app/ponto/tests/test_selo_chokepoint_escrita.py:322–354,379–402`.

## ARQUIVOS

Lista fechada:
- `app/chamados/services/regularizacao_ext.py` — guarda e aquisição antecipada. `app/chamados/services/regularizacao_ext.py:59–66,122–124,203–207`.
- `app/ponto/services/flip_batida.py` — trava no flip transacional. `app/ponto/services/flip_batida.py:21–33`.
- `app/chamados/models.py` — modo da trava de Colaborador em `trava_na_ordem`. **Vai ao Opus.** `app/chamados/models.py:2938–2941`.
- `app/ponto/tests/test_f4e_ordem_batida.py` — arquivo novo para os REDs reais, seguindo o molde de `app/colaboradores/tests/test_feriado_matriz_p3_trava.py:34–102`.
- `app/ponto/tests/test_juiz_unico_eventos.py` — adaptar o selo de ordem do materializador. `app/ponto/tests/test_juiz_unico_eventos.py:409–432`.
- Selos de captura de SQL: `app/chamados/tests/test_bug_par_projetado.py:437,602`; `app/chamados/tests/test_e1_v5_ordem_trava.py:41–49`; `app/chamados/tests/test_e1_v9.py:41,68–79`; `app/chamados/tests/test_e1_v13_cadeia.py:45`; `app/chamados/tests/test_e1_v13_ordem.py:68–73`; `app/chamados/tests/test_e1_volta2.py:155–174`; `app/chamados/tests/test_e2_tarefa_do_dia.py:887–890`; `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:305`; `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:50–51,113–116`; `app/chamados/tests/test_f5_contrato_v1_guarda_sob_trava.py:121`; `app/chamados/tests/test_f5_v3_guarda_sob_trava.py:48–59`; `app/colaboradores/tests/test_lote3_c5_recusa_e_trava.py:131–145`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:93–103`; `app/ponto/tests/test_o_outbox_celula.py:418–424`.

`core/juizes.py`, `motor_calculo_v2.py`, `crons.py` e `bin/` estão fora da lista. Submeter a mudança de concorrência ao Opus conforme `CLAUDE.md:392–402`.

## PROIBIDO

Nenhum `except` novo e nenhum `with suppress(...)` novo, inclusive nos testes. Sem fallback, juiz novo, allowlist, regra nova de negócio, apagamento de teste ou decisão sobre o dono da PK. Preservar as zonas invioláveis, o motor, o domínio de competência e o caminho de chão, sem trava ou transação nova no `INSERT` do app. Sem mudança nos receivers, no escritor mensal ou na proteção do recálculo após o DELETE. `CLAUDE.md:25–38,142–168` · `app/docs/LEIS.md:77–78` · `app/ponto/registro_batida.py:172–182,328–341` · `app/ponto/services/fechamento.py:1819–1856`.

## LEI ABERTA

Nenhuma pergunta nova nesta volta. B2-chão segue na fila herdada, para a linha `lei:` no topo do RELATO. `app/docs/LEIS.md:78` · `app/ponto/registro_batida.py:197–212,264–297`.
```

## 26/11 17:52 · sessao 7d57a27180df · PAR-PELO-TURNO · leitura · origem=Code

# PAR-PELO-TURNO c1 v2 -- brief do construtor (base de65d64f4)

## CENSO

leitores:

- Teto e dia no juiz. `app/ponto/turnos.py:63::_teto_s_da_jornada` e `app/ponto/turnos.py:1434::parear_turnos` leem o cálculo do teto. `app/ponto/turnos.py:230::_cabe_na_jornada_da_vespera`, `app/ponto/turnos.py:1086::_fechar_aberto_com_saida_seguinte` e `app/ponto/turnos.py:1177::_fechar_aberto_na_pausa_sem_volta` leem a saída numérica. `app/ponto/turnos.py:298::_data_do_turno` e `app/ponto/turnos.py:302::_data_do_turno` leem a guarda da véspera. `app/ponto/turnos.py:318::_mk` lê o dia. `app/ponto/turnos.py:1501::parear_turnos` e `app/ponto/turnos.py:1502::parear_turnos` leem os pós-fechos.
- `parear_turnos`, 13 chamadas diretas de execução. `app/ponto/turnos.py:1535::turnos_de_batidas`, `app/ponto/turnos.py:2005::_turnos_do_colab_puro`, `app/ponto/turnos.py:2315::_turno_aberto_puro`, `app/ponto/motor_calculo_v2.py:530::turnos_via_autoridade`, `app/chamados/services/fila_por_causa.py:197::particionar`, `app/escala/services/meio_periodo.py:168::observado_meio_periodo`, `app/escala/services/semana_do_posto.py:120::_bateram`, `app/ponto/management/commands/reconciliar_fantasmas.py:99::_completude`, `app/ponto/services/diagnostico_escala.py:142::diagnostico_escala`, `app/ponto/services/lastro.py:268::julgar`, `app/ponto/services/piso_intervalo.py:48::pares_abaixo_do_piso`, `app/ponto/services/triagem_batida.py:178::triagem_bater_ponto`, `app/ponto/utils_turno_orfao.py:31::detectar_bloqueio_turno_orfao`.
- `turnos_de_batidas`, quatro chamadas. `app/ponto/motor_calculo_v2.py:909::_turnos_do_juiz`, `app/escala/services/propositor.py:117::_dias_batidos`, `app/escala/services/propositor.py:152::_horario_mediano`, `app/escala/services/propositor.py:522::propostas`.
- `_turnos_do_colab_puro`, três chamadas. `app/ponto/turnos.py:1928::turnos_do_colab`, `app/ponto/turnos.py:3125::CargaDaJanela.turnos`, `app/chamados/services/carga_pergunta_viva.py:182::turnos`.
- `turnos_do_colab`, 51 chamadas nos seis itens a seguir. API e chamados. `app/api/views.py:1447::api_espelho_v2`, `app/chamados/detector_anomalias.py:157::detectar_batida_fora_escala`, `app/chamados/juizes.py:447::_turno_tem_saida`, `app/chamados/juizes.py:2303::_turno_cruza_meia_noite`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:145::classificar`, `app/chamados/models.py:1378::premissa_regularizada`, `app/chamados/services/disputa_emissao.py:722::_orfao_resolvido_cross_meianoite`, `app/chamados/services/materializacao.py:399::_achar_batida_origem`, `app/chamados/services/materializacao.py:513::turnos_sem_par`.
- Colaboradores e escala. `app/colaboradores/services/drawer.py:47::contexto_drawer`, `app/colaboradores/services/feriado_do_posto.py:473::_classificar_previa`, `app/escala/detector_proposta.py:65::medir_padrao`, `app/escala/regua_defesa.py:361::turnos_longos_suspeitos`, `app/escala/services/escala_auto.py:87::propostas_do_vinculo`, `app/escala/services/impacto_troca.py:18::_trabalhados`, `app/escala/servico_jornada.py:249::_turno_noturno_completo`, `app/escala/servico_jornada.py:403::classificar_falta`, `app/escala/utils.py:1627::montar_grade_prevista_periodo_por_turno`, `app/escala/utils.py:1952::montar_grade_prevista_periodo_por_turno`.
- Comandos de ponto. `app/ponto/management/commands/censo_fase_12x36.py:180::handle`, `app/ponto/management/commands/detectar_ausencias.py:337::handle`, `app/ponto/management/commands/detectar_par_relampago.py:56::pares_relampago`, `app/ponto/management/commands/dna_x_batida_real.py:69::handle`, `app/ponto/management/commands/e6_oraculo.py:159::InsumosDoDono.turnos`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88::handle`, `app/ponto/management/commands/marcar_abandono_provisorio.py:46::handle`, `app/ponto/management/commands/verificar_nucleo.py:29::handle`, `app/ponto/management/commands/vigia_plantio_overnight.py:38::batidas_plantadas_no_dia_errado`.
- Núcleo e serviços de ponto. `app/ponto/nucleo.py:47::recompute_turnos`, `app/ponto/services/bordas_realizado.py:66::bordas_do_realizado`, `app/ponto/services/esmeril_espelho.py:102::assinaturas_do_colab`, `app/ponto/services/espelho.py:628::montar_dias`, `app/ponto/services/passivo_s84.py:82::classificar`, `app/ponto/services/regularizacao.py:61::regularizar_turno_orfao`, `app/ponto/services/teto_na_lista.py:68::medir`, `app/ponto/services/triagem_batida.py:553::_autoresolve_f2_f4`, `app/ponto/services/vigia_pareador_celula.py:97::censo`.
- Leitura do turno. `app/ponto/turno_leitura.py:89::turno_invariante`, `app/ponto/turno_leitura.py:95::turno_invariante`, `app/ponto/turnos.py:780::realizado_do_dia`, `app/ponto/turnos.py:2112::dia_encerrado_motivo`, `app/ponto/turnos.py:2170::dias_encerrados`, `app/ponto/turnos.py:2201::par_completo_da_janela`, `app/ponto/turnos.py:2998::dia_das_batidas`, `app/ponto/turnos.py:3193::batidas_do_dia_turno`.
- Telas e relatórios. `app/ponto/views.py:396::turnos_abertos`, `app/ponto/views.py:1613::_abrir_justificativa_fluxo`, `app/ponto/views.py:3356::regularizar_retorno`, `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124::handle`, `app/relatorios/pdf_espelho.py:213::_coletar_dados_espelho`, `app/relatorios/pdf_espelho.py:451::_coletar_dados_espelho`.
- Lote de turnos. `app/colaboradores/services/calendario.py:252::contexto_calendario`, `app/ponto/status_dia.py:273::status_dos_dias_lavrados`, `app/ponto/turnos.py:3135::CargaDaJanela.dias_encerrados`, `app/ponto/turnos.py:3139::CargaDaJanela.dia_das_batidas`. As cargas de turno dos chamados e do juiz do dono também são lidas em `app/chamados/juizes.py:448::_turno_tem_saida`, `app/escala/servico_jornada.py:252::_turno_noturno_completo`, `app/escala/servico_jornada.py:404::classificar_falta` e `app/ponto/management/commands/e6_oraculo.py:185::InsumosDoDono.dono`.
- Dia por PK. `app/ponto/turnos.py:3004::dia_das_batidas` e `app/ponto/turnos.py:3140::CargaDaJanela.dia_das_batidas` leem o mesmo corpo. Chamadas do mapa em `app/api/views.py:1444::api_espelho_v2`, `app/api/views.py:2042::api_justificativas`, `app/core/services/painel_op.py:330::_d4_em_lote`, `app/ferias/management/commands/listar_ferias_com_batida.py:54::handle`, `app/ponto/management/commands/vigia_plantio_overnight.py:87::batidas_fora_do_turno_da_pergunta`, `app/ponto/services/espelho.py:465::montar_dias` e `app/ponto/turnos.py:3024::dia_do_turno_da_batida`.
- Dia de uma batida. `app/api/views.py:750::api_bater_ponto`, `app/api/views_core.py:744::api_bater_ponto`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160::classificar`, `app/chamados/services/disputa_emissao.py:76::_data_furo_da_grade`, `app/chamados/services/disputa_emissao.py:283::perguntar_entradas_pendentes`, `app/chamados/services/disputa_emissao.py:400::gerar_perguntas_disputa`, `app/chamados/services/fio.py:1822::_dia_do_fato`, `app/chamados/services/foto_e4b.py:283::foto_do_colaborador`, `app/escala/servico_jornada.py:509::resolver_data_evento`, `app/ponto/management/commands/lavrar_estoque_par_relampago.py:58::_classificar`, `app/ponto/services/par_relampago_na_lista.py:101::_origem_pelo_vinculo_na_data`, `app/ponto/services/par_relampago_na_lista.py:326::verbos_do_sinal`, `app/ponto/services/triagem_batida.py:729::ramo_registrar_es`.
- Batidas do dia e sobras. `app/chamados/models.py:1377::premissa_regularizada`, `app/chamados/services/disputa_emissao.py:831::_tem_base`, `app/escala/servico_jornada.py:531::procedencia_fina`, `app/ponto/services/flip_auto.py:104::classificar_pares` leem `batidas_do_dia_turno`. `app/colaboradores/services/calendario.py:612::contexto_calendario`, `app/ponto/services/espelho.py:631::montar_dias` e `app/relatorios/pdf_espelho.py:622::_coletar_dados_espelho` leem `datas_saida_orfa`.
- Apuração. `app/ponto/adaptador_motor.py:29::periodos_de_turnos` lê o turno, as pontas e o intervalo julgado. `app/ponto/motor_calculo_v2.py:557::_periodos_via_autoridade` lê a geometria e `app/ponto/motor_calculo_v2.py:558::_periodos_via_autoridade` entrega o trio a `_periodos_pelo_marco`. Os quatro leitores são `app/ponto/motor_calculo_v2.py:2861::montar_periodos`, `app/ponto/motor_calculo_v2.py:2991::montar_periodos`, `app/ponto/motor_calculo_v2.py:3329::montar_periodos` e `app/ponto/motor_calculo_v2.py:3649::montar_periodos`. O mapa por entrada passa por `app/ponto/motor_calculo_v2.py:916::_dia_do_turno` e é lido em `app/ponto/motor_calculo_v2.py:2461::calcular_mes`, `app/ponto/motor_calculo_v2.py:3038::calcular_mes` e `app/ponto/motor_calculo_v2.py:3442::calcular_mes`. A janela de HE já usa o dia do juiz para ancorar os marcos. `app/ponto/motor_calculo_v2.py:1664::_janela_do_dia`.
- C9. `app/ponto/turnos.py:178::_declara_separado_pelo_teto` lê o primeiro carimbo e `app/ponto/turnos.py:198::pares_separados_pelo_teto` e `app/ponto/turnos.py:203::pares_separados_pelo_teto` leem geometria e carimbo. `app/ponto/services/teto_na_lista.py:68::medir` lê os eventos. `app/ponto/management/commands/lavrar_teto_do_dia.py:28::handle` lê a medição e `app/ponto/services/teto_na_lista.py:123::ultimo_lavrado` lê a foto. A projeção passa por `app/escala/services/cadastro_realidade.py:434::_teto_do_dia`, `app/escala/services/cadastro_realidade.py:772::acao`, `app/escala/services/cadastro_realidade.py:1016::lista`, `app/escala/services/cadastro_realidade.py:1086::lista`, `app/escala/services/cadastro_realidade.py:1202::um_dono_por_dia` e `app/escala/services/cadastro_realidade.py:1249::dias_na_lista`. Tela e PDF passam por `app/escala/views.py:525::cadastro_x_realidade` e `app/escala/views.py:527::cadastro_x_realidade`. O HTML lê assinatura e detalhe em `app/templates/escala/cadastro_x_realidade.html:31`.
- HE. `app/ponto/services/espelho.py:734::montar_dias` lê as pontas e o relato do motor e `app/ponto/services/espelho.py:939::montar_dias` e `app/ponto/services/espelho.py:946::montar_dias` leem a projeção. Outros leitores de `he_fora_da_janela` são `app/ponto/services/he_pendente_lavrado.py:68::minutos_fora_do_dia`, `app/ponto/services/he_pendente_lavrado.py:138::apurar`, `app/folha/porta_export.py:190::pendencias_he_da_janela`, `app/ponto/views.py:180::espelho_ponto`, `app/relatorios/pdf_espelho.py:932::_build_story` e `app/templates/ponto/espelho.html:189`. Leem o retrato `app/ponto/views.py:820::gestao_he_pdf`, `app/ponto/views.py:950::gestao_he`, `app/ponto/services/gestao_he.py:428::itens_sem_decisao`, `app/ponto/services/gestao_he.py:493::pendentes_na_central`, `app/ponto/services/he_pendente_lavrado.py:249::fotos_paradas`, `app/ponto/services/he_pendente_lavrado.py:254::fotos_paradas` e `app/ponto/management/commands/recusar_ponta_pequena.py:66::handle`. Leem a decisão vigente `app/ponto/services/gestao_he.py:93::estado_por_dia`, `app/ponto/services/gestao_he.py:123::_decisoes`, `app/ponto/management/commands/lavrar_he_pendente.py:48::handle`, `app/ponto/portas/he.py:148::_vigente` e `app/ponto/motor_calculo_v2.py:1521::_decisoes_he`.
- Censo de execução por chamada direta e leitores das projeções identificadas, incluindo comandos. Referências conferidas na cópia indicada. Identidade Git de HEAD, despacho por reflexão, consumidores externos e execução em produção [nao verificado].

escritores:

- `app/ponto/turnos.py:82::_teto_da_jornada` produz `previsto + 4 h` e a indicação de teto do dia. `app/ponto/turnos.py:1433::parear_turnos` fornece o envelope de 26 h ao fecho principal. `app/ponto/turnos.py:180::_declara_separado_pelo_teto` escreve o carimbo transitório de C9, hoje chamado por `app/ponto/turnos.py:1452::parear_turnos`.
- `app/ponto/turnos.py:285::_data_do_turno` determina o dia e `app/ponto/turnos.py:323::_mk` escreve o `Turno`. `app/ponto/turnos.py:1096::_fechar_aberto_com_saida_seguinte` e `app/ponto/turnos.py:1186::_fechar_aberto_na_pausa_sem_volta` completam a geometria por `_replace` e carregam o `data_turno` recebido. `app/ponto/turnos.py:3042::_dia_das_batidas_puro` publica o mapa por PK e `app/ponto/turnos.py:3044::_dia_das_batidas_puro` preenche o dia das marcas sem turno que as reivindique.
- `app/ponto/turnos.py:210::pares_separados_pelo_teto` produz dia, pontas, marcos, teto e vão da C9. `app/ponto/services/teto_na_lista.py:78::medir` escreve o item com a competência daquele dia e o dono de `app/ponto/management/commands/e6_oraculo.py:182::InsumosDoDono.dono`. `app/ponto/services/teto_na_lista.py:96::medir` produz totais e rótulo. `app/ponto/services/teto_na_lista.py:113::lavrar` escreve `MetricaSnapshot`. `app/escala/services/cadastro_realidade.py:105` declara a descrição C9, `app/escala/services/cadastro_realidade.py:435::_teto_do_dia` produz a assinatura e `app/escala/services/cadastro_realidade.py:1233::um_dono_por_dia` propaga o dono julgado quando as assinaturas divergem.
- Fronteiras de entrada. `app/ponto/registro_batida.py:118::registrar_batida` e `app/ponto/registro_batida.py:255::anotar` escrevem a batida e suas anotações. `app/escala/alimentacao.py:228::vinculo_do_dia` e `app/escala/utils.py:84::marcos_dna_periodo` fornecem vínculo e DNA. `app/ponto/turnos.py:1565::papel_por_minuto_da_ata` produz E/S/X/Xi. `app/ponto/turnos.py:344::_marca_ecos_flush`, `app/ponto/turnos.py:415::_marca_intra_duracao` e `app/ponto/turnos.py:1315::parear_turnos` escrevem carimbos de trabalho em memória. A apuração seleciona batidas vivas em `app/ponto/turnos.py:1868::turnos_do_colab` e `app/ponto/turnos.py:2941::batidas_apuraveis`.
- Fronteiras de saída. `app/ponto/services/espelho.py:790::montar_dias` escreve `he_fora_da_janela`. `app/ponto/services/he_pendente_lavrado.py:138::apurar` monta a pendência e `app/ponto/services/he_pendente_lavrado.py:183::lavrar` grava o retrato. `app/ponto/portas/he.py:44::_gravar` e `app/ponto/portas/he.py:394::_marcar_revogada` escrevem decisão e revogação. `app/ponto/services/dia_pago.py:360::lavrar` e `app/ponto/services/fechamento.py:1168::recalcular_fechamento_mes` são escritores do gravado financeiro.

decisoes:

- **Lei 1:** a órfã de madrugada absorvida pela jornada da véspera herda o **dia da véspera**, inclusive na borda de competência. A jornada pertence ao dia de início, L-085. Fecho e dia saem do **mesmo juiz**. `app/docs/PROMPTS.md:3025`, `app/docs/RELATO.md:170`, `app/docs/LEIS.md:117`.
- **V6, padrão juiz único.** Antecipação e prorrogação da mesma jornada preservam o intervalo efetivo e seguem para a apuração vigente. `previsto + 4 h` vira motivo de C9 quando seria o único corte de um par válido. O limite absoluto de fecho é 26 h. A classificação de pausa continua pela classe, Base 3 h, Partido 0 e Plantão 8 h, com as duas luzes `Xi` da ata fora do teto da pausa inferida. A janela de HE e a Gestão existentes classificam e publicam a pendência. `app/docs/PROMPTS.md:2852`, `app/ponto/motor_calculo_v2.py:755`, `app/ponto/motor_calculo_v2.py:2976`, `app/ponto/motor_calculo_v2.py:3629`, `app/ponto/turnos.py:1572`, `app/docs/LEIS.md:115`.
- **Fora desta obra.** Cadastro de duração da V2, alerta de interjornada, confirmação por ocorrência, porta nova, alterações nas batidas originais ou retratadas, reclassificação geral do dia civil, caso col673, desempate de PK entre dois turnos na troca de vínculo, autorização automática de HE, crédito duplicado e qualquer escrita financeira de competência exportada. A guarda de absorção de pausa continua em seu contrato próprio. `app/docs/PROMPTS.md:2823`, `app/docs/PROMPTS.md:2852`, `app/docs/BACKLOG.md:1864`, `app/docs/BACKLOG.md:2029`, `app/docs/RELATO.md:18`, `app/ponto/turnos.py:1349`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:116`.

perguntas de lei: 0

## ARQUIVOS

Lista fechada para o construtor. A escolha inclui a descrição pública do comando C9 e autoriza, nos testes legados nomeados, somente a substituição explícita de contratos superados pela V6 ou a preservação do controle com uma fixture que realmente o exerça. `app/docs/PROMPTS.md:2852`, `app/docs/LEIS.md:136`, `app/ponto/management/commands/lavrar_teto_do_dia.py:20`.

- `app/ponto/turnos.py` — juiz do fecho e do dia, guarda de 26 h, carimbo do par válido e leitura de C9. Os três pontos de fecho e a atribuição de véspera entram na prova. `app/ponto/turnos.py:216`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1084`, `app/ponto/turnos.py:1170`, `app/ponto/turnos.py:1430`.
- `app/ponto/services/teto_na_lista.py` — consumir o evento do juiz e corrigir a descrição da lavra. Preservar chave, dono, horários, vão, teto e competência derivada de `data_turno`. `app/ponto/services/teto_na_lista.py:25`, `app/ponto/services/teto_na_lista.py:68`, `app/ponto/services/teto_na_lista.py:100`.
- `app/escala/services/cadastro_realidade.py` — adequar texto e detalhe C9 para fotos antigas e novas. O texto comum é que o vão real ultrapassou o teto do dia e coube no envelope de 26 h. `app/escala/services/cadastro_realidade.py:105`, `app/escala/services/cadastro_realidade.py:430`.
- `app/ponto/management/commands/lavrar_teto_do_dia.py` — adequar docstring e `help` da C9. A execução segue a medição e a lavra existentes. `app/ponto/management/commands/lavrar_teto_do_dia.py:2`, `app/ponto/management/commands/lavrar_teto_do_dia.py:20`, `app/ponto/management/commands/lavrar_teto_do_dia.py:26`.
- `app/ponto/tests/test_par_pelo_turno.py` — arquivo novo proposto para os REDs, os controles e a integração da matriz. `app/docs/LEIS.md:136`, `app/docs/BACKLOG.md:2027`.
- `app/ponto/tests/test_col129_par_separado_pelo_teto.py` — a asserção de dois turnos de col129 contradiz literalmente a V6. Preservar o gabarito de horário, dia, vão, teto, contagem e identidade por PK. No caso F, uma cópia condicionada a `t.aberto` perde a mordida quando o turno fecha. Preservar a contraprova da absorção do caso G. `app/ponto/tests/test_col129_par_separado_pelo_teto.py:61`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:65`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:106`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:116`.
- `app/ponto/tests/test_h_portao_teto_do_dia.py`, `app/ponto/tests/test_o280_bateria_da_lei.py` e `app/ponto/tests/test_saida_pela_posicao_entre_turnos.py` — revisar somente os contratos em que a recusa dependia do teto diário antigo. Col879 requer prova do papel das batidas da própria fixture. O pino A6 exige zero por `1.020 > 960`. O selo de posição recusa a candidata pela guarda antiga. Conservar as recusas por jornada distinta, falta de prova, pausa em curso, degenerado e envelope absoluto. `app/ponto/tests/test_h_portao_teto_do_dia.py:32`, `app/ponto/tests/test_o280_bateria_da_lei.py:2226`, `app/ponto/tests/test_saida_pela_posicao_entre_turnos.py:177`, `app/ponto/tests/test_saida_pela_posicao_entre_turnos.py:286`.
- `app/ponto/tests/test_col129_teto_vira_lista.py` — preservar contagem, idempotência, minutos e porta pelo dono. O legado que exige `BATIDA` por causa do furo precisa de um furo legítimo para continuar provando essa porta. No par recuperado, a referência é o resultado do juiz do dono. `app/ponto/tests/test_col129_teto_vira_lista.py:72`, `app/ponto/tests/test_col129_teto_vira_lista.py:155`, `app/ponto/tests/test_col129_teto_vira_lista.py:209`, `app/ponto/services/teto_na_lista.py:86`.

Obras e correções do mapa da base.

- **DIA-DA-BATIDA-JUIZ-UNICO F4c/F4d** compartilha `app/ponto/turnos.py` conforme o escopo informado. O ponto de encontro é a alimentação de `turnos_do_colab`, a resposta na casca e em lote e o dia por PK em janelas sobrepostas. A F4b está registrada e a F4c consta aguardando o portão de carga. Diff exato da F4c/F4d e estado da F4d [nao verificado]. Conferir a composição na árvore candidata com o mesmo PK no dia 20 pela casca, pelo lote e pela consulta individual. `app/docs/RELATO.md:18`, `app/docs/RELATO.md:34`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:2998`, `app/ponto/turnos.py:3024`, `app/ponto/turnos.py:3140`.
- **E1-PREVISAO-24H** cruza `app/escala/services/cadastro_realidade.py` no agregador da lista. A cópia E1a acrescenta a assinatura de previsão inválida e disputa o código C13 já

[truncado em 20000 de 38663 caracteres]

## 26/11 17:50 · sessao 227df873e3ce · O30-F3-CONSTRUIR · construtor · origem=Code

**O30-F3-CONSTRUIR c12.** O RED novo passou na cópia da c11 (`rc 0`) e falhou com o mutante (`rc 1`). A falha ocorreu na asserção que exige que só o arquivo anterior permaneça. O mutante deixou o arquivo novo no storage simulado. `app/ponto/tests/test_atestado_f3_contrato.py:459,485` · `app/ponto/tests/mutantes_atestado_f3.py:421,443`

### Testes [nao verificado]

- `test_RED_reabrir_compensa_apos_sair_do_atomic_local` simula `False → True → False`, provoca um `RuntimeError` após o upload e confere a propagação da mesma exceção, a exclusão do arquivo novo, a preservação do anterior e o registro de `anexo_apagado=True` como único aviso. `app/ponto/tests/test_atestado_f3_contrato.py:459–490`
- `green_c11` passou nos 12 testes do seletor local. A certificação do Code e do Opus final está [nao verificado]. `app/ponto/tests/mutantes_atestado_f3.py:419–420`

O diff contém só os dois arquivos de teste, com **zero `except` novos**. O mutante lê `in_atomic_block` na entrada do `with` e reutiliza o valor nos dois ramos. A substituição de `ausencia.py` ocorre em memória. `app/ponto/tests/test_atestado_f3_contrato.py:459` · `app/ponto/tests/mutantes_atestado_f3.py:309,378,443`

No censo, `lote_atestados` chama `pdf_bytes` no ramo `pdf`. A rota de PDF aponta para essa view. `app/ponto/views_atestado.py:99,135–137` · `app/relatorios/urls.py:17`

### Comandos do par green e mutante

A partir de `app/`, o primeiro comando deve sair com `0` e o segundo com `1`. `app/ponto/tests/mutantes_atestado_f3.py:421,443,456`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 green_c12
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 c12_reabrir_le_atomic_dentro_do_with
```

### Diff

```diff
diff --git a/app/ponto/tests/mutantes_atestado_f3.py b/app/ponto/tests/mutantes_atestado_f3.py
--- a/app/ponto/tests/mutantes_atestado_f3.py
+++ b/app/ponto/tests/mutantes_atestado_f3.py
@@ -62,0 +63 @@
+SAIDA_ATOMIC_LOCAL = PORTA + 'test_RED_reabrir_compensa_apos_sair_do_atomic_local'
@@ -307,0 +309,14 @@
+def mutar_leitura_do_atomic(fonte):
+    entrada = '        with transaction.atomic(using=banco) if dados_atestado else nullcontext():\n'
+    compensacao = 'if anexo_gravado and not conexao.in_atomic_block:'
+    for antes, depois in (
+        (entrada, entrada + '            em_atomic = conexao.in_atomic_block\n'),
+        (compensacao, compensacao.replace('conexao.in_atomic_block', 'em_atomic')),
+        (CONDICAO_AVISO_ORFAO, CONDICAO_AVISO_ORFAO.replace('conexao.in_atomic_block', 'em_atomic')),
+    ):
+        if fonte.count(antes) != 1:
+            raise SystemExit(f'Seletor deve aparecer uma vez em {AUSENCIA}: {antes.strip()}')
+        fonte = fonte.replace(antes, depois, 1)
+    return fonte
+
+
@@ -389 +404 @@
-             'green_c11',
+             'green_c11', 'green_c12',
@@ -391 +406 @@
-             'c8_try_dentro_do_atomic')
+             'c8_try_dentro_do_atomic', 'c12_reabrir_le_atomic_dentro_do_with')
@@ -405,0 +421,2 @@
+    elif nome == 'green_c12':
+        fontes, testes = {}, (SAIDA_ATOMIC_LOCAL,) [nao verificado]
@@ -425,0 +443,3 @@
+    elif nome == 'c12_reabrir_le_atomic_dentro_do_with':
+        caminho = APP / AUSENCIA
+        fontes, testes = {caminho: mutar_leitura_do_atomic(caminho.read_text(encoding='utf-8'))}, (SAIDA_ATOMIC_LOCAL,) [nao verificado]
diff --git a/app/ponto/tests/test_atestado_f3_contrato.py b/app/ponto/tests/test_atestado_f3_contrato.py
--- a/app/ponto/tests/test_atestado_f3_contrato.py
+++ b/app/ponto/tests/test_atestado_f3_contrato.py
@@ -458,0 +459,33 @@
+    def test_RED_reabrir_compensa_apos_sair_do_atomic_local(self):
+        self._preparar_storage_da_reabertura()
+        falha = RuntimeError('falha após gravar o arquivo no atomic local')
+
+        @contextmanager
+        def atomic(**kwargs):
+            self.conexao.in_atomic_block = True
+            try:
+                yield
+            finally:
+                self.referencias[self.registro.pk] = self.nome_antigo
+                self.conexao.in_atomic_block = False
+
+        def falhar_apos_upload(*args, **kwargs):
+            self.assertTrue(self.conexao.in_atomic_block)
+            self.assertIn(self.nome_novo, self.arquivos)
+            raise falha
+
+        with patch.object(self.transacao, 'atomic', atomic), \
+                patch.object(ausencia, 'mudar_estado', side_effect=falhar_apos_upload), \
+                self.assertLogs(ausencia.logger, level='WARNING') as logs, \
+                self.assertRaises(RuntimeError) as erro:
+            ausencia._reabrir(self.registro, self.documento, dados_atestado=CAMPOS)
+
+        self.assertIs(erro.exception, falha)
+        self.assertFalse(self.conexao.in_atomic_block)
+        self.assertEqual(self.arquivos, {self.nome_antigo})
+        self.documento.storage.delete.assert_called_once_with(name=self.nome_novo)
+        self.assertEqual(logs.output, [
+            f'WARNING:ponto:falha na reabertura: ausencia={self.registro.pk} '
+            f'nome={self.nome_novo} anexo_apagado=True',
+        ])
+
```

## 26/11 17:49 · sessao 6385f2c074c3 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

A c2h ficou na cópia com a bateria de saída, os mutantes em memória e as duas entradas da catraca atualizadas. O GREEN completo ficou bloqueado por dois pontos da base: o cálculo direto do Partido devolve minutos brutos com pausa interna, e a catraca integral ainda tem **13 registros de linha divergentes**, após corrigir os dois pedidos. [Partido](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/motor_calculo_v2.py:2997) · [Catraca](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/core/tests/test_medida_turno_do_par.py:567)

| Item | Entrega | RED | GREEN local |
|---|---|---|---|
| 1. Catraca | `[530]` → `[531]` nas duas entradas pedidas. [Pareadores](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/core/tests/test_medida_turno_do_par.py:261) · [Derivações](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/core/tests/test_medida_turno_do_par.py:351) | O teste integral falhou por asserção na base. | As duas entradas conferem com o AST. A catraca integral ainda acusa 2 registros em `pareadores` e 11 em `derivacoes_de_dia`. | [nao verificado]
| 2. Bateria | Oito casos com valores fixados pela regra para 12x36 e Comercial, mais dois controles do Partido com intervalo entre períodos. [Grade](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_ramo2b_c2h_saida.py:32) · [Controles](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_ramo2b_c2h_saida.py:123) | `deed2fe72` deu 8 falhas por asserção. | 18 subcasos passaram em memória. O diagnóstico da grade com pausa interna no Partido deu 8 falhas. |
| 3. Selos P2/P3 | Docstrings registram os limites de `vars()`, `copy` com `setattr` e apelidos de `max`, e apontam para a bateria. [P2](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_ramo2b_c2_base_da_he.py:627) · [P3](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_contract_juiz_batida.py:377) | Os mutantes P2/P3 falharam na bateria. | 13 testes dos selos passaram em memória. | [nao verificado]
| 4. Commit | Corpo abaixo. A proibição de `git commit` prevaleceu. | — | SHA de commit novo [nao verificado]. | [nao verificado]

### Comandos e saídas

Medição da linha pedida, a partir de `app/`. Resultado `pareadores [531]` e `derivacoes_de_dia [531]`. [Censo AST](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/core/medida_turno_do_par.py:448) · [Chamada medida](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/motor_calculo_v2.py:531)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from core.medida_turno_do_par import censo
medida = censo('.')
for grupo in ('pareadores', 'derivacoes_de_dia'):
    print(grupo, next(i['linhas'] for i in medida[grupo]['itens'] if
                      i['chave'] == 'ponto/motor_calculo_v2.py::turnos_via_autoridade'
                      and i.get('sitio') == 'porta_parear_turnos'))
PY
```

Para cada linha abaixo, execute o comando indicado após `export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.base`. As saídas foram medidas localmente pela função `rodar`, com `unittest` e dublês de infraestrutura em memória porque `django.test` estava indisponível. A suíte Django do Code e o Opus final ficam [nao verificado]. [Executor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/ramo2b_c2h_mutantes.py:57) [nao verificado]

| Comando | Saída local |
|---|---|
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes red_deed2fe72` | rc 1, 8 asserções. No cruzamento, o oráculo devolveu `(700, 0, 60, 760)` e a regra esperava `(700, 20, 40, 760)`. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes green` | rc 0, 2 testes, 18 subcasos. | [nao verificado]
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes sem_ponta` | rc 1, 8 asserções. Reproduziu a perda dos 20 min de gozo. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes copy_setattr` | rc 1, 8 asserções. Base **1**, esperada **700** no cruzamento. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes teto_max` | rc 1, 16 asserções. Trabalhado **800**, esperado **760** no cruzamento. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes inclui_cauda` | rc 1, 5 asserções. Base **760**, esperada **680** na pausa anterior ao marco. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes perde_fim_no_marco` | rc 1, 2 asserções. Gozo **0**, esperado **20** na pausa que termina no marco. |
| `python3 -B -m ponto.tests.ramo2b_c2h_mutantes partido_interno` | rc 1, 8 asserções. No cruzamento, o Partido devolveu `(720, 0, 0, 800)` e a regra esperava `(700, 20, 40, 760)`. |

Todas as variantes registraram zero erros de execução. A tupla segue a ordem `(base, gozo, intra, trabalhado)`. A contagem AST deu **0 `ExceptHandler` novos** nos cinco arquivos do delta. [Asserções da bateria](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/test_ramo2b_c2h_saida.py:94) · [Injeções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/tests/ramo2b_c2h_mutantes.py:23)

```text
$ git -C .. rev-parse HEAD
c193310ebb5ccd111ec99d59af06e144461224ca
$ git -C .. diff --exit-code deed2fe72 -- app/ponto/motor_calculo_v2.py
```

O diff do motor saiu vazio, rc 0. [Porta da tripla](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-163306/app/ponto/motor_calculo_v2.py:1830)

### Corpo preparado para o commit [nao verificado]

```text
RAMO2-B c2h: mede a saida da base da HE e registra a linha da catraca

OBRA: RAMO2-B-EM-TURNO-QUE-CRUZA
BASE: c193310ebb5ccd111ec99d59af06e144461224ca

Regra: RAMO2-B-C2-RESPOSTA-PAUSA-2611 (L-032, L-031, L-097).
A bateria fixa base, gozo, intra e trabalhado pela geometria do turno
e confere a tripla None nos periodos sem cauda.
Catraca de linhas: porta_parear_turnos na linha 531, em dois registros.
Selos AST P2/P3: limites documentados e mutantes medidos na saida.

LEI-AKITA:
origem=ponto/calculador/alimentacao.py:173-178
testemunha=ponto/calculador/nucleo.py:146-153, segmentos e batidas
RED=ponto/tests/test_ramo2b_c2h_saida.py:115, com alimentacao de deed2fe72
quem-mais-le=ponto/calculador/alimentacao.py:106-117,168-173,469-475,539-554
  (rubricas_por_dia -> ponto/calculador/regras.py::do_dia:584-616,767-789);
  ponto/services/fechamento.py:1291-1300;
  ponto/management/commands/diff_calculador.py:414-424;
  ponto/calculador/periodo.py:745-759; folha/porta_export.py:339-358
juizes novos=0
padrao=juiz unico

fora do marco: PAUSA-NA-BORDA-DO-PAR (fatia propria)

Pendencias medidas: 13 registros de linha herdados na catraca integral
e 8 contraexemplos da grade de pausa interna no calcular_periodo do Partido.
```

## 26/11 17:47 · sessao 6845351dc60f · - · - · origem=Ronald (tmux)

Conferi a matriz na cópia e reproduzi em memória os caminhos puros do pareador com as dependências de fronteira simuladas. Encaminhei os gabaritos e as citações ao agente principal.

- A borda sintética 20/10 16:00 → 21/10 05:00, DNA 18:00–00:30, dá 780 min contra 630. Na base há dois turnos, a saída cai dia 21 e existe um C9 no dia 20. Pela Lei 1, o esperado é um par e um C9 no dia 20, ambos os PKs na competência 10 e zero na 11. `app/ponto/turnos.py:1295`, `app/ponto/turnos.py:1447`, `app/ponto/turnos.py:3037`, `app/ponto/janelas.py:24`, `app/docs/PROMPTS.md:3025`.
- As duas rotas de pós-fecho têm fixtures independentes que ficam abertas na base. A de posição leva pausa de 60 min e vão de 780 min contra 732. A de pausa sem volta comprovada leva 795 contra 780, com controle de pausa ainda em curso. `app/ponto/turnos.py:991`, `app/ponto/turnos.py:1084`, `app/ponto/turnos.py:1170`.
- O teto absoluto precisa de RED próprio. A base fecha 27 h com previsão de 23 h 50 e teto calculado de 27 h 50. A V6 exige 0 pares e 0 C9 nesse caso, com 26 h exatas permitidas. `app/ponto/turnos.py:78`, `app/ponto/turnos.py:1433`, `app/ponto/turnos.py:1447`, `app/docs/PROMPTS.md:2852`.
- O teste legado de col129 exige dois turnos onde a V6 exige um. O de col879 também cobra separação por teto e precisa de exame do papel da nova entrada antes de qualquer alteração. A contraprova de absorção tem mecanismo distinto e deve continuar sem C9. `app/ponto/tests/test_col129_par_separado_pelo_teto.py:61`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:116`, `app/ponto/tests/test_h_portao_teto_do_dia.py:32`, `app/ponto/turnos.py:1349`.
- A série HJ-026 de col155 inclui 08/11, dia omitido na matriz do dossiê. A sequência nominal de 08/11, a composição dos oito domingos auditados e os gabaritos de produção por rubrica seguem [nao verificado]. `app/docs/BACKLOG.md:745`, `app/docs/RELATO.md:542`, `app/docs/RELATO.md:544`.
- A Lei 1 resolve o dia quando a absorção é demonstrada. Substituir globalmente a guarda da véspera por 26 h derrubaria os controles da jornada diurna e da órfã sem entrada. A duplicidade de PK entre dois turnos na troca de vínculo segue registrada na obra DIA-DA-BATIDA. `app/ponto/tests/test_o195_dia_do_turno_por_envelope.py:77`, `app/ponto/tests/test_o195_dia_do_turno_por_envelope.py:141`, `app/docs/RELATO.md:18`, `app/docs/RELATO.md:139`.

## 26/11 17:47 · sessao 80756353ca60 · - · - · origem=Ronald (tmux)

- Lei 1 fechada em `app/docs/PROMPTS.md:3025`, `app/docs/RELATO.md:170` e `app/docs/LEIS.md:117`. V6 em `app/docs/PROMPTS.md:2852`, fila em `app/docs/BACKLOG.md:2027` e `app/docs/RELATO.md:141,161`. Interjornada em obra própria em `app/docs/BACKLOG.md:2029`.
- O dossiê trata PAR-RELÂMPAGO c7 como obra em voo. A cópia já traz o pouso `e216b756a` registrado em `app/docs/RELATO.md:111–112`. Preserve reapresentação e identidade do par em `app/ponto/turnos.py:1850,1921–1928,2193–2240,2971,3001–3024` e C13 em `app/escala/services/cadastro_realidade.py:135,406–427,1012–1013`.
- DIA-DA-BATIDA F4b altera a alimentação, a casca e o lote no mesmo `turnos.py`, confirmado na árvore `0d081f585` pelas linhas `1850,2901,2934,2952,2964,3126`. F4c aparece em `app/docs/RELATO.md:18`. Escopo comum F4c/F4d conforme enunciado. Delta exato F4c/F4d [nao verificado]. E1a `8d81e64c5` compartilha o agregador CxR e traz C13 de previsão inválida em `../8d81e64c58a57603b52de46bc6fcc8ab4251c949/app/escala/services/cadastro_realidade.py:135–141,719,980,989–995,1045`. O motor do E1a voltou à base antiga conforme `app/docs/RELATO.md:14,26`. FERIADO compartilha a integração de cálculo e a fila, com seu escopo registrado em `app/docs/BACKLOG.md:2005,2027`.
- Produção recomendada para o brief em `app/ponto/turnos.py:216–324,1084–1102,1170–1189,1430–1502`, `app/ponto/services/teto_na_lista.py:39–106` e `app/escala/services/cadastro_realidade.py:100–107,430–443`. Inclua `app/ponto/management/commands/lavrar_teto_do_dia.py:2,20` estritamente se corrigir a descrição pública do comando. Ela ainda diz “pares separados”. O dono continua pela mesma função em `app/ponto/services/teto_na_lista.py:86` e `app/ponto/management/commands/e6_oraculo.py:98–110,182`.
- Execução em memória da função real com stubs de infraestrutura confirmou dois turnos na borda sintética `20/10 17:30 E → 21/10 06:30 S` com DNA dia 20 `21:30–05:42` e dia 21 `08:00–17:00`. A base dá 1 aberto no dia 20, 1 órfã no dia 21 e C9 `780/732` atribuída ao dia 20. A regra exige 1 fechado, 0 órfãs, PK da saída pertencente ao dia 20 e 1 C9 na competência 10, 0 na 11. `app/ponto/turnos.py:1430–1457,1491–1502`, `app/ponto/janelas.py:50–63`, `app/colaboradores/models.py:46`.
- O fecho principal usa DNA pela data civil da entrada (`app/ponto/turnos.py:1430`), enquanto pós-fechos usam `data_turno` (`:1086,:1177`). A borda anterior pode passar com o juiz de dia intacto. O discriminante adicional usa caso com madrugada que tenha pertencimento comprovado ao turno da véspera e saída posterior a `hi + previsto +4h`. Exemplo em memória: E 21 00:30, S 21 07:10, DNA do dia 20 `23:50–03:00` e do 21 `08:00–17:00`. Base fecha 400 minutos no dia 21. Com pertencimento provado ao 20, resultado exigido no 20. `app/ponto/turnos.py:216–231,285–309`.
- `27 h` com previsto `23h50` reproduz fecho na base. `26 h` é o máximo, com testes na igualdade e em `26 h +1 s`. O 12x36 literal `19–01/02–07` já fecha em memória, então funciona como controle junto de um RED de extensão. `app/ponto/turnos.py:78–82,1447–1457,1491–1502`. A matriz mantém a guarda própria de absorção em `app/ponto/turnos.py:1327–1355`.
- O gabarito antigo contém efeitos incompatíveis com a V6 em `app/ponto/tests/test_col129_par_separado_pelo_teto.py:61–63`, `app/ponto/tests/test_h_portao_teto_do_dia.py:32–36`, além de BATIDA decorrente do furo em `app/ponto/tests/test_col129_teto_vira_lista.py:209–225`. Os novos REDs devem morder antes de qualquer adaptação das caracterizações superadas. Contadores, idempotência, dono pela função real e casos de furo legítimo continuam cobertos.
- `app/docs/RELATO.md:6225–6232` registra comp. 11 em 21/10–20/11, empresas 2/3/4 e 960 colaboradores. PAR antigo tinha 14 colab-dias em 8 colaboradores. O B1 do INTERVALO V2 tinha 760 colab-dias/199 colaboradores e B2 tinha 1584/275. A `mesa/TABELA.md` documentada em `app/docs/RELATO.md:6234–6247` contém quatro chamados/26 opções. Universo e predicado de 142 [nao verificado]. Os oito domingos ZERO de col129/155 são o recorte fornecido pelo anexo e as oito chaves individuais permanecem [nao verificado].
- As linhas de triagem corretas são `app/docs/RELATO.md:542–550` para os grupos do laudo e `:554,:566,:568` para pontas diurnas e números do teto. Para 15/11, cite a matriz raspada para instantes individuais e use `app/docs/RELATO.md:566,:568` para 780/783 e âncoras; pausa completa e rubricas [nao verificado]. Para 11/10 há horas sintéticas em `app/ponto/tests/test_col129_teto_vira_lista.py:52–56`; sequência de produção [nao verificado]. Nenhuma nova pergunta de lei confirmada.

## 26/11 17:47 · sessao c9070bf5d1ec · - · - · origem=Ronald (tmux)

Censo AST enviado com 13 chamadas diretas de `parear_turnos`, 51 de `turnos_do_colab`, quatro de `turnos_de_batidas`, três de `_turnos_do_colab_puro` e as fronteiras de atribuição por PK, C9 e HE. Os pontos de origem permanecem em `app/ponto/turnos.py:1192`, `app/ponto/turnos.py:1513`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1934` e `app/ponto/turnos.py:3034`.

A cópia registra 760 dia-colaborador no B1 e 1.584 no B2 de INTERVALO V2, e 14 dia-colaborador em oito colaboradores no recorte PAR da competência 11. O universo do 142 permanece [nao verificado]. `app/docs/RELATO.md:6225–6234`.

A asserção legada de col129 exige o par separado e conflita com o fecho da V6 para a mesma fixture. Preserve o gabarito de C9 e explicite a substituição da obrigação geométrica pela regra aprovada. `app/ponto/tests/test_col129_par_separado_pelo_teto.py:57–71`, `app/docs/PROMPTS.md:2852`.

PAR-RELÂMPAGO consta como pousada. DIA F4c consta no relatório da cópia, e o delta de F4d está [nao verificado]. `app/docs/RELATO.md:18`, `app/docs/RELATO.md:112`.

## 26/11 17:46 · sessao 9da75a87c3a9 · O30-F3-CONSTRUIR · verificador · origem=Code

**O30-F3-CONSTRUIR c11 — bloqueio 2 curado.** O RED de `KeyboardInterrupt` falhou por falta do aviso com o blob c10 reconstruído em memória e passou com a c11. Os três blobs alterados e suas reconstruções c10 conferem com o delta fornecido. A cópia veio sem `.git`. A identidade dos commits e o comportamento em produção ficam [nao verificado]. `app/ponto/services/ausencia.py:1435,1442,1444` · `app/ponto/tests/test_atestado_f3_contrato.py:524,539` · `app/ponto/tests/mutantes_atestado_f3.py:96,408`

### 1. Saídas de `_reabrir`

| Saída | Resultado na c11 |
|---|---|
| Sucesso | **nada**. `concluiu` vira `True` depois da saída normal do `with`. A função marca `_criada=False` e `_reaberta=True` e devolve a instância. `app/ponto/services/ausencia.py:1435,1443,1446` |
| `Exception` ou `BaseException` com upload detectado e atomic externo ainda ativo | Emite um aviso de possível órfão e deixa a exceção subir. A compensação fica fora desse caminho. Os testes de `RuntimeError` e `KeyboardInterrupt` conferem a identidade da exceção, o aviso exato e a ausência de consulta ou exclusão. `app/ponto/services/ausencia.py:1436,1441,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:521,524,538,542` |
| Retorno antecipado | **nada** na árvore. O único `return` fica depois do `finally`. Injetei em memória um `return` logo após o upload e ele emitiu o aviso sob atomic externo, pois ainda pulava `concluiu = True`. `app/ponto/services/ausencia.py:1422,1435,1443,1448` |
| Falha antes do upload, ausência de upload ou documento já `_committed` | **nada**. `anexo_gravado` só recebe valor se o documento passar de pendente a `_committed` durante o `save`. `app/ponto/services/ausencia.py:1408,1412,1416,1421` |
| Falha no `save` do `FileField` | Se a falha vier **depois** da transição para `_committed`, o `finally` interno captura `(storage, nome)`, inclusive para `BaseException`. Se o storage deixar um arquivo e falhar **antes** dessa confirmação, a porta fica sem sinal e o aviso deixa de sair. Reproduzi os dois caminhos em memória. A ocorrência com o storage de produção fica [nao verificado]. `app/ponto/services/ausencia.py:1416,1418,1421,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:551,555` |
| Falha ao sair do `with` | `concluiu` ainda está `False`. Com upload detectado, o estado da conexão **após** a saída do `with` decide o caminho. Simulei falhas na saída com e sem atomic externo. `app/ponto/services/ausencia.py:1411,1435,1437,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:439,444` |

**Sem atomic externo, a compensação continua igual à c10.** Com `Exception` e upload detectado, a porta tenta apagar o nome novo, preserva o nome anterior e qualquer nome ainda referenciado, registra `anexo_apagado` e relança. Uma `BaseException` fora de `Exception` continua subindo sem compensação nem aviso quando a conexão está fora de atomic. `app/ponto/services/ausencia.py:249,256,262,266,1436,1443`

O aviso sinaliza **risco**. Sob atomic externo, ele pode sair mesmo que o arquivo acabe referenciado, pois a porta não consulta referências nesse caminho. Se o chamador fizer rollback **depois** de `_reabrir` devolver, a porta já encerrou sem aviso. A view de INSS tenta compensar esse segundo caso quando recebe a instância e a falha posterior é uma `Exception`. `app/ponto/services/ausencia.py:1437,1443,1448` · `app/ponto/views.py:2569,2572,2588,2590,2596`

### 2. Quem escreve e quem lê

| Valor | Escritor, leitores e significado |
|---|---|
| `concluiu` | `_reabrir` escreve `False` antes do `try` e `True` depois do `with`. Só o `finally` lê. Significa que o fluxo local chegou ao fim do bloco protegido. O estado final da transação do chamador fica fora desse sinal. `app/ponto/services/ausencia.py:1409,1435,1443,1448` |
| `anexo_gravado` | `_reabrir` inicializa com `None` e o `finally` interno registra `(storage, nome)` pela transição de `_committed`. O `except`, o `finally` externo, os dois avisos e o compensador consomem esse mesmo fato. O compensador consulta separadamente se o nome ainda tem referência. `app/ponto/services/ausencia.py:1408,1416,1421,1437,1443,249,262` |
| `conexao.in_atomic_block` | Em produção, a porta obtém a conexão do alias escolhido pelo router e lê o atributo do Django depois do `with`. O escritor interno do atributo no Django fica [nao verificado] nesta cópia. O teste escreve `False` na fixture e `True` nos casos com atomic externo. Os leitores da porta tratam o valor como “a conexão ainda está dentro de um atomic neste ponto”. `app/ponto/services/ausencia.py:1406,1411,1437,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:368,511,529` |
| Aviso | `_reabrir` escreve no logger `ponto`. A configuração SaaS encaminha o `WARNING` ao `pontofile`. O contrato lê o texto exato e o teste de integração procura a mensagem com ID e nome. Ambos esperam um aviso de possível órfão. Consumo operacional em produção [nao verificado]. `app/ponto/services/ausencia.py:49,1444` · `app/config/settings/saas.py:131,160,181` · `app/ponto/tests/test_atestado_f3_contrato.py:546` · `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:164,183` |

### 3. Selo do `Image.open`

As cinco trocas falharam em memória. O selo exige um único tratador, do tipo AST `Tuple`, e compara a tupla inteira com `(UnidentifiedImageError, OSError)`. `app/ponto/tests/test_atestado_f3_contrato.py:228,238,241,244,246`

| Troca | Efeito e ponto da queda |
|---|---|
| `except Exception` | Também converte erros como `ValueError` em `None`. Cai em `assertIsInstance`, pois é `ast.Name`. `app/ponto/services/atestados_lote.py:147,151` · `app/ponto/tests/test_atestado_f3_contrato.py:244` |
| `except BaseException` | Também converte `KeyboardInterrupt` e `SystemExit` em `None`. Cai no mesmo `assertIsInstance`. `app/ponto/services/atestados_lote.py:147,151` · `app/ponto/tests/test_atestado_f3_contrato.py:244,525` |
| `except OSError` | Captura `OSError` e suas subclasses. No mock, isso inclui `UnidentifiedImageError`. A hierarquia no Pillow real fica [nao verificado]. Cai em `assertIsInstance`. `app/ponto/tests/test_atestado_f3_contrato.py:244,792` |
| Tupla reordenada | Captura os mesmos tipos. Cai na igualdade AST pela ordem dos elementos. `app/ponto/services/atestados_lote.py:151` · `app/ponto/tests/test_atestado_f3_contrato.py:245,246` |
| Tupla com tipo a mais | Com `ValueError` a mais, também converte esse erro em `None`. Cai na igualdade AST pelo elemento adicional. `app/ponto/services/atestados_lote.py:151` · `app/ponto/tests/test_atestado_f3_contrato.py:245,246` |

### 4. REDs, mutantes e mocks

- **Mordem.** `green_c11` passou nos 12 testes. O RED histórico e `c11_reabrir_sem_finally` caíram por falta de aviso. `c11_reabrir_sem_marca_de_sucesso` caiu por aviso indevido no sucesso. `c11_pdf_handler_amplo` caiu no selo AST. O mutante adaptado `c8_try_dentro_do_atomic` também caiu, pois deixou o arquivo novo após a falha na saída do `with`. `app/ponto/tests/mutantes_atestado_f3.py:96,124,291,404,423` · `app/ponto/tests/test_atestado_f3_contrato.py:244,456,512,539`
- **O mock de transação deixa passar uma regressão concreta.** Mover a leitura de `in_atomic_block` para dentro do `with` e reutilizar o valor nos dois ramos passou nos 12 testes. Com um contexto simulado que muda a flag de `False` para `True` na entrada e volta a `False` ao sair, o mutante emitiu o aviso de atomic externo e deixou o arquivo. A c11 compensou. A fixture padrão e o teste de falha na saída mantêm a flag fixa. `app/ponto/services/ausencia.py:1411,1437,1443` · `app/ponto/tests/test_atestado_f3_contrato.py:368,444,529`
- **O mock de decode ainda encobre a remoção isolada de `original.load()`.** A transposição simulada chama `load()`. Confirmei o teste comportamental verde e o selo AST vermelho para essa mutação. O verde e o mutante com PDF real pararam no `import PIL`, então o bloqueio 1 do laudo continua [nao verificado]. `app/ponto/tests/test_atestado_f3_contrato.py:235,239,774,812,832` · `app/ponto/tests/mutantes_atestado_f3.py:138,406`

### 5. Contagem AST c10 → c11

| Arquivo | `ExceptHandler` | Chamadas a `suppress` |
|---|---:|---:|
| `app/ponto/services/ausencia.py:266,1436,1821` | 13 → 13 | 0 → 0 |
| `app/ponto/services/atestados_lote.py:151,263,271` | 3 → 3 | 0 → 0 |
| `app/ponto/tests/test_atestado_f3_contrato.py:51,62,64,241,527` | 0 → 0 | 0 → 0 |
| `app/ponto/tests/mutantes_atestado_f3.py:124,207,291` | 0 → 0 | 0 → 0 |

O `suppress` do arquivo de mutantes está numa string usada para gerar uma variante. A contagem acima é do AST do arquivo. `app/ponto/tests/mutantes_atestado_f3.py:207,209`

### 6. Classes já reprovadas

- **2º juiz, 2º escritor fora da porta, relógio fora da borda, derivação paralela, ordem de trava e campo com dois significados:** **nada** novo no delta. Os pontos de decisão, gravação, relógio e trava permanecem nos mesmos caminhos. `app/ponto/services/ausencia.py:301,306,317,1403,1412,1418,1427,1432,1435` · `app/ponto/services/atestados_lote.py:172`
- **Selo/RED sem mordida:** **nada** entre os casos novos registrados. A lacuna da flag fixa permanece na cobertura simulada, conforme o contraexemplo do item 4. `app/ponto/tests/mutantes_atestado_f3.py:96,124,404` · `app/ponto/tests/test_atestado_f3_contrato.py:368,444,529`
- **`except` ou fallback novo:** **nada**. O `except Exception` continua no ramo de compensação. O `finally` externo passa a emitir o aviso existente também quando uma `BaseException` escapa. `app/ponto/services/ausencia.py:1436,1441,1442,1444`

## 26/11 17:41 · sessao 2871c389c64e · DIA-DA-BATIDA-JUIZ-UNICO · verificador · origem=Code

## Veredito — REPROVADO

1. **B1 corrige o ciclo apontado na F4c entre regularização e `apagar`, mas a ordem ainda falha em caminhos alcançáveis.** A regularização pode travar uma Pergunta no `post_save` e depois adquirir a trava de um Chamado no encerramento do lote. A nova trava do Colaborador também fecha um ciclo com o flip automático sobre a mesma Batida. `app/chamados/services/regularizacao_ext.py:122–123,153–157,203–207` · `app/chamados/reconciliador.py:673–694` · `app/chamados/models.py:773–777` · `app/ponto/management/commands/flip_automatico.py:62–68`
2. **A cura de B2 está no código. Falta confirmar o RED executável.** Nesta cópia, tanto o teste da versão atual quanto o mutante pararam no import de `django.db`. O mutante retornou `RC=2`, `FAIL=0`, `ERROR=1`. Sua queda por asserção fica **[nao verificado]**. `app/ponto/registro_batida.py:328–341` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:225–232`

## Curas e mutantes

| Item | Sítio e teste que morde | Resultado na cópia | [nao verificado]
|---|---|---|
| **B1** | Trava no topo de `app/chamados/services/regularizacao_ext.py:122–124`. O teste espiona a ordem de entrada no `atomic`, trava, leitura de itens e escrita em `app/ponto/tests/test_juiz_unico_eventos.py:409–432`. | Verde na fonte atual. `f4d-b1-sem-trava` cai com **RC 1**, duas falhas por asserção, em `corrigir` e `remover`. `app/ponto/tests/juiz_unico_eventos_mutantes.py:123–125` · `app/ponto/tests/test_juiz_unico_eventos.py:431` |
| **B2** | O juiz está antes do `delete`, fora da proteção e do savepoint interno. O teste exige o mesmo `DatabaseError`, linha e PK preservados, zero trilha, agendamento, savepoint e commit, e um rollback. `app/ponto/registro_batida.py:333–341` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360` | `f4d-b2-protecao-e-savepoint` restaura o bloco que engole a exceção exigida pelo teste. A execução parou antes dessa asserção. **[nao verificado]** `app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134` · `app/ponto/tests/test_juiz_unico_eventos.py:333,351` |
| **B3** | INFO passou a carregar `processados=%d`. O teste exige `processados=0` e uma só chamada para dois dias da mesma competência. `app/ponto/services/fechamento.py:1841–1845` · `app/ponto/tests/test_juiz_unico_eventos.py:474–483` | Verde na fonte atual. `f4d-b3-info-antigo` cai com **RC 1** na mensagem. O RED de K3, que exige nova tentativa quando uma exceção sobe ao callback, também cai com **RC 1**. `app/ponto/tests/juiz_unico_eventos_mutantes.py:135–141,171–175` · `app/ponto/tests/test_juiz_unico_eventos.py:462–483` |
| **E5** | A promessa do cron saiu do comentário. A configuração da cópia declara que `recalcular_fechamento` fica sem cron. `app/ponto/registro_batida.py:192–194` · `app/config/crons.py:1148–1155` | Verde na fonte atual. `f4d-e5-comentario-antigo` cai com **RC 1** na asserção sobre o comentário. `app/ponto/tests/juiz_unico_eventos_mutantes.py:142–146,206–210` · `app/ponto/tests/test_juiz_unico_eventos.py:485–489` |

## B1 — Ordem dentro do `atomic`

- **Sim, é a primeira aquisição de trava de linha daquele `atomic`.** Com os argumentos usados, `trava_na_ordem` pede só o Colaborador. No par original, regularização e `apagar` agora pedem o mesmo Colaborador antes de escrever ou travar a Batida. A consulta que veta disputa aberta ainda roda antes da trava. Uma abertura concorrente pode passar entre a consulta e a aquisição, e o materializador segue sem reler. `app/chamados/services/regularizacao_ext.py:59–66,122–124` · `app/chamados/models.py:2901–2902,2933–2967` · `app/ponto/registro_batida.py:328–336` · `app/chamados/services/disputa_emissao.py:530–542,601–606`
- **Sequência interna.** `corrigir` faz `UPDATE` direto. `adicionar` delega o `INSERT` a `registrar_batida`. Os dois passam pelo `post_save`, que pode travar Disputas, Chamados e Perguntas via reconciliador. `remover` faz `delete` direto e aciona núcleo e cartório pelo `post_delete`. No fim, `transicionar_com_pauta(..., ato=reg.chamado.encerrar)` tenta travar explicitamente o Chamado do lote. Se o `post_save` já travou uma Pergunta e o Chamado do lote era terminal e estava fora do conjunto travado, a aquisição nova é **Pergunta → Chamado**. O teste de B1 usa escritas simuladas e `chamado_id=None`. `app/chamados/services/regularizacao_ext.py:73–83,133–157,203–207` · `app/ponto/registro_batida.py:178–182` · `app/ponto/signals.py:53–55,148–150,216–225` · `app/chamados/reconciliador.py:673–694` · `app/chamados/services/fecho_runtime.py:157–168` · `app/chamados/models.py:773–777,1785–1793,2952–2966` · `app/ponto/tests/test_juiz_unico_eventos.py:417–432`
- **Contraexemplo com dois atos.** T1, `flip_automatico --apply`, abre o `atomic` e atualiza a Batida B via `flip_tipo` → `anotar`. T2, a regularização, trava o Colaborador C e espera B ao tentar corrigir a mesma linha. T1 entra no reconciliador pelo `post_save` e espera C se houver pergunta elegível. Uma pergunta de disputa já fechada satisfaz o seletor, portanto esse cenário é compatível com a guarda da regularização. O ciclo é **T1 segura B e espera C, T2 segura C e espera B**. Execução em PostgreSQL e ocorrência em produção **[nao verificado]**. `app/ponto/management/commands/flip_automatico.py:62–68` · `app/ponto/services/flip_batida.py:25–33` · `app/ponto/registro_batida.py:281–287` · `app/ponto/signals.py:216–225` · `app/chamados/reconciliador.py:673–679,740–762,1580` · `app/chamados/services/regularizacao_ext.py:59–66,122–123,153–157`
- **Há também um ciclo possível com batida de chão.** A regularização segura C em `FOR UPDATE`. O app pode reservar no `INSERT` a chave única `(C, timestamp)` e esperar o `KEY SHARE` do FK em C. Se a regularização tenta corrigir outra Batida para esse mesmo instante, espera a chave ocupada pelo app. A hora válida do aparelho pode coincidir com a hora corrigida. Execução desse ciclo e efeito em produção **[nao verificado]**. `app/chamados/models.py:2938–2941` · `app/chamados/services/regularizacao_ext.py:122–123,144–157` · `app/api/views_core.py:492–495,643–659` · `app/api/hora_do_aparelho.py:135–146` · `app/ponto/registro_batida.py:178–180` · `app/ponto/models.py:22,102–106` · `app/colaboradores/tests/test_feriado_matriz_p3_trava.py:5–10`

## B2 — Chamadores e exceção

- **Produção.** O único chamador encontrado é o POST administrativo `rejeitar` de `batidas_offline_pendentes`, protegido por `ver_painel_situacional`. O `except Batida.DoesNotExist` cobre a consulta inicial. O erro do juiz sai da view antes da resposta de sucesso. Esse ato é a rejeição administrativa de marca offline, e nenhum chamador de chão foi encontrado para `apagar`. A resposta HTTP observada em produção é **[nao verificado]**. `app/ponto/urls.py:78` · `app/ponto/views.py:452–456,467–482,496–500` · `app/docs/LEIS.md:78`
- **Testes.** O chamador em `test_juiz_unico_eventos.py:352` captura `DatabaseError` com `assertRaises`. Os demais deixam uma exceção inesperada falhar o teste. `app/ponto/tests/test_juiz_unico_eventos.py:273,305,314,351–352,368,401` · `app/ponto/tests/test_porta_batida.py:80–81`
- **Limite da proteção.** Se a leitura viva falha, o erro sai do `atomic` antes do `delete`, da trilha e da limpeza do PK. Depois do `delete`, a união dos dias e o agendamento continuam sob `_proteger_fechamento_por_evento`. O callback roda em `on_commit` e conserva seus `except`. O `delete` e seus receivers ficam fora desse guard. `app/ponto/registro_batida.py:197–204,328–343` · `app/ponto/services/fechamento.py:1830–1856`

## B3 — Retorno e log

Com os dois defaults `False`, **todo retorno normal é `int`**. O contador começa em `0`, é incrementado com `1` e é o único retorno desse modo. Universo vazio, guarda que exclui o colaborador ou exceção por colaborador antes do incremento podem deixar `0`. O dicionário pertence ao modo leitura. O `%d` recebe o contador em argumentos separados do formato, com a competência já marcada. Falhas internas de `DiaPago` ainda podem coexistir com `processados=1`, e o `except` mensal registra PK e nome quando falha no nível do colaborador. `app/ponto/services/fechamento.py:468–471,574–584,636–639,1152–1160,1262–1265,1321–1335,1841–1845`

## AST — `except` e `suppress`

Nenhum novo. Contagem de `ast.ExceptHandler` e chamadas a `suppress`, da base do delta para a cópia. Em B2 saiu um **uso** de `_proteger_fechamento_por_evento`, cujo `except` segue atendendo os outros usos. `app/ponto/registro_batida.py:197–212,333–340` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134`

| Arquivo | `except` | `suppress` |
|---|---:|---:|
| `app/chamados/services/regularizacao_ext.py:104,112,158,191` | 4 → 4 | 0 → 0 |
| `app/ponto/registro_batida.py:18,200` | 2 → 2 | 0 → 0 |
| `app/ponto/services/fechamento.py:594,1105,1111,1262,1321,1327,1685,1846,1854` | 9 → 9 | 0 → 0 |
| `app/ponto/tests/juiz_unico_eventos_mutantes.py:82,189` | 0 → 0 | 0 → 0 |
| `app/ponto/tests/test_juiz_unico_eventos.py:318–360` | 0 → 0 | 1 → 0 |

## Classes e escritores/leitores do delta

**Ordem de trava** é a classe que reprova. O selo de B1 morde a remoção da chamada local, com alcance limitado aos dois ramos simulados. A escrita direta de correção e remoção fora da porta já existia. A correção de tipo e horário está registrada como classe 9. O delta reutiliza o juiz de evento, a competência do fato e o escritor mensal, sem introduzir relógio ou derivação paralela. `processados` tem um significado de contagem, mesmo com várias causas possíveis para zero. `app/ponto/tests/test_juiz_unico_eventos.py:409–432` · `app/chamados/services/regularizacao_ext.py:133–157` · `app/core/portas.py:98–105` · `app/docs/CORTES.md:441` · `app/ponto/registro_batida.py:333–340` · `app/ponto/services/fechamento.py:637,1326–1335,1830–1845`

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `RegularizacaoExterna.colaborador_id` e sentinela da trava | `criar_regularizacao_externa` grava o FK. Guarda, criação de Batida, trilha e nova trava leem a relação ou seu ID. `trava_na_ordem` registra a aquisição na conexão, lida por `colaboradores_travados` e `estampa_das_travas`. `app/chamados/services/regularizacao_ext.py:59–65,73–82,98,123,238–241` · `app/chamados/models.py:2938–2941,4592–4595` · `app/chamados/services/trava_do_colaborador.py:26–28,37–64,72–79` |
| `Batida.pk`, `colaborador_id`, `timestamp`, `tipo` | O `create` da porta gera a linha e o PK. A correção externa também grava `timestamp` e `tipo`. A view lê a marca para a trilha. `apagar` guarda o PK, relê a linha e entrega a marca viva ao juiz. Após sucesso, limpa o PK da instância recebida. `app/ponto/registro_batida.py:172–182,318–343` · `app/chamados/services/regularizacao_ext.py:151–157` · `app/ponto/views.py:474–489` · `app/ponto/turnos.py:3037–3047` |
| `.dias`, `dias_vivos`, `_dias_exclusao`, `dias_da_jornada` | `julgar_evento_da_batida` produz `.dias`. `apagar` guarda os dias vivos. O núcleo escreve `_dias_exclusao` no `post_delete`, e o cartório lê ou preenche pelo mesmo juiz. A porta une os dias e o callback consome a união, inclusive vazia. `app/ponto/turnos.py:3034–3057` · `app/ponto/signals.py:19–25,53–55,113–122,148–150` · `app/ponto/registro_batida.py:333–340` · `app/ponto/services/fechamento.py:1830–1841` |
| `motivo`, competência e `processados` | `apagar` fornece `motivo='exclusao'`. O resolvedor fornece mês e ano para a deduplicação, a chamada mensal e o log. O escritor mensal produz a contagem, lida agora pelo INFO e já lida pela tela e pelo comando. `app/ponto/registro_batida.py:340` · `app/ponto/services/fechamento.py:1618–1628,637,1326–1335,1830–1850` · `app/ponto/views.py:1280–1281` · `app/ponto/management/commands/recalcular_fechamento.py:130–134` |
| Texto de E5 | O comentário da porta é lido pelo teste via `Path.read_text`. O mutante injeta a versão antiga nessa leitura. `app/ponto/registro_batida.py:187–196` · `app/ponto/tests/test_juiz_unico_eventos.py:485–489` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:142–146,206–210` |

## 26/11 17:40 · sessao 85c92057ec44 · O30-F3-CONSTRUIR · construtor · origem=Code

A c11 está aplicada na cópia. `_reabrir` agora avisa sobre possível anexo órfão se a execução falhar após gravar o upload sob `atomic` externo, inclusive por `KeyboardInterrupt`. A marca de sucesso é definida após a saída do `with`, e o teste de sucesso exige silêncio. O selo AST de `_foto_no_pdf` exige um único tratador com `(UnidentifiedImageError, OSError)`. [ausencia.py:1435](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1435) · [test_atestado_f3_contrato.py:241](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:241) · [test_atestado_f3_contrato.py:509](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:509) [nao verificado]

Da pasta `app/`, os comandos da tabela usam o executor de mutantes em memória. [mutantes_atestado_f3.py:365](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:365)

```bash
f3() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 "$@"
}
```

| Item | RED | GREEN | Mutantes |
|---|---|---|---|
| 1. Aviso em `_reabrir` | `f3 red_c10_reabrir_keyboard_interrupt` falhou na base com `assertLogs`, rc 1. [Teste:524](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:524) | `f3 green_c11` passou os 12 testes, incluindo `KeyboardInterrupt`, `RuntimeError` e sucesso sob `atomic` externo, rc 0. [Executor:404](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:404) | `f3 c11_reabrir_sem_marca_de_sucesso` caiu em `assertNoLogs`. `f3 c11_reabrir_sem_finally` caiu em `assertLogs`. Ambos rc 1. [Mutantes:125](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:125) | [nao verificado]
| 2. Tipo do tratador do PDF | `f3 c11_pdf_handler_amplo` passou com o selo c10 antes do reforço, rc 0. [Mutante:131](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:131) | `f3 green_c11` passou com o par de exceções especificado, rc 0. [Teste:241](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:241) | `f3 c11_pdf_handler_amplo` caiu na asserção que exige a tupla, rc 1. [Teste:244](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:244) | [nao verificado]

**Contagem AST c10 → c11**

| Arquivo | `ExceptHandler` | `suppress` |
|---|---:|---:|
| [ponto/services/ausencia.py:1436](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1436) | 13 → 13 | 0 → 0 |
| [ponto/services/atestados_lote.py:151](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/atestados_lote.py:151) | 3 → 3 | 0 → 0 |
| [ponto/tests/test_atestado_f3_contrato.py:528](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:528) | 0 → 0 | 0 → 0 |
| [ponto/tests/mutantes_atestado_f3.py:117](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:117) | 0 → 0 | 0 → 0 |

O chamador do PDF de lote é `lote_atestados`, em `ponto/views_atestado.py`. Pillow e PDF reais ficam para a suíte da obra [nao verificado]. [views_atestado.py:136](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views_atestado.py:136) · [test_atestado_f3_contrato.py:832](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:832)

**SHA.** O `HEAD` continua em `119aaa8bf8870c7eef8bbe3ecd76f703992b994e`. A entrega ficou no diff da cópia porque as regras vedam `git commit` e a operação escreveria fora de `app/`. SHA de commit c11 [nao verificado]. [mutantes_atestado_f3.py:25](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:25)

## 26/11 17:39 · sessao 65c5ec36379d · - · - · origem=Ronald (tmux)

**B1 corrige o ciclo original entre regularização e `apagar`, mas ainda há inversão de trava.** A nova chamada é a primeira instrução do atomic da materialização e trava apenas o Colaborador. A exclusão do DP também começa pelo Colaborador antes da Batida. O teste passou na cópia e o mutante caiu com RC 1, dois `FAIL` e zero `ERROR`. O teste usa mocks de trava e escrita, cobre corrigir e remover e passa `chamado_id=None`. `app/chamados/services/regularizacao_ext.py:122-124`, `app/chamados/models.py:2901-2902,2938-2967`, `app/ponto/registro_batida.py:328-336`, `app/ponto/tests/test_juiz_unico_eventos.py:409-432`, `app/ponto/tests/juiz_unico_eventos_mutantes.py:123-125`.

**Contraexemplo com dois atos.** O `flip_automatico --apply` entra no atomic e `flip_tipo` chama `anotar`, cujo UPDATE segura a Batida B. A regularização concorrente do mesmo Colaborador C segura C pelo delta e espera B ao corrigir. O `post_save` do flip chama o reconciliador, que tenta C quando há pergunta elegível. Uma pergunta de disputa já fechada satisfaz essa condição sem acionar a guarda de disputa aberta. A agenda da cópia contém esse comando. Ocorrência em produção [nao verificado]. `app/config/crons.py:707`, `app/ponto/management/commands/flip_automatico.py:51-68`, `app/ponto/services/flip_batida.py:21-32`, `app/ponto/registro_batida.py:264-287`, `app/chamados/services/regularizacao_ext.py:59-66,122-123,153-157`, `app/ponto/signals.py:216-225`, `app/chamados/reconciliador.py:651-653,673-694,740-762,1580`.

**Dentro do atomic.** Corrigir e adicionar executam `post_save` e podem travar o universo Disputa/Chamado/Pergunta no reconciliador. Remover executa os `post_delete` do núcleo e do cartório. O `transicionar_com_pauta` executa o `encerrar` do chamado do lote, que pede C, D abertas e o próprio Ch antes de decidir. Se o `post_save` já travou uma P e o chamado do lote é terminal e está fora dos destinos previamente travados, `encerrar` pode adquirir esse Ch pela primeira vez depois de P. A posse de C serializa esse exemplo com atos que também respeitam C primeiro. `app/chamados/services/regularizacao_ext.py:78-83,138,157,207`, `app/ponto/registro_batida.py:172-182`, `app/ponto/signals.py:53-55,141-150,216-225`, `app/chamados/reconciliador.py:686-694`, `app/chamados/services/fecho_runtime.py:157-168`, `app/chamados/models.py:773-777,1785-1793,2943-2966`, `app/chamados/catalogo/motor.py:24-31`, `app/chamados/services/acoes_chamado.py:322-329`.

A guarda de disputa ocorre antes da nova trava. Outro ato pode abrir D sob C entre a leitura da guarda e a aquisição de C pela regularização, que prossegue sem reler. Isso já existia no trecho do anexo. `app/chamados/services/regularizacao_ext.py:59-66,122-124`, `app/chamados/services/disputa_emissao.py:530-542,601-606`.

O `colaborador_id` novo é o FK gravado na criação do lote. A chamada também registra um sentinela de trava na fila da conexão, lido pelas portas que exigem ou identificam a trava corrente. `app/chamados/models.py:4592-4595,2939-2941`, `app/chamados/services/regularizacao_ext.py:238-241`, `app/chamados/services/trava_do_colaborador.py:26-28,37-64,72-80`.

O teste e o mutante B2 pararam no import de `django.db` no ambiente somente leitura. O mutante saiu RC 2 com `FAIL=0 ERROR=1`. Queda por asserção [nao verificado]. `app/ponto/tests/test_juiz_unico_eventos.py:333`, `app/ponto/tests/juiz_unico_eventos_mutantes.py:228-232`.

## 26/11 17:39 · sessao 715c4520f925 · - · - · origem=Ronald (tmux)

**B2 curado no código.** `apagar` trava e relê a batida, chama o juiz antes do `delete` e deixa o `DatabaseError` sair do `atomic`. A falha interrompe o ato antes do delete, agendamento, trilha e limpeza do PK. O agendamento pós-delete mantém `_proteger_fechamento_por_evento`; o recálculo roda no `on_commit` e conserva seus `except`. [app/ponto/registro_batida.py:318–343] [app/ponto/registro_batida.py:197–204] [app/ponto/services/fechamento.py:1830–1856]

- O único caller de produção encontrado é o POST `rejeitar` da tela offline. O `except Batida.DoesNotExist` cobre a consulta inicial. A exceção de `apagar` sai da view e impede a resposta `ok=True`. É um ato administrativo protegido por `ver_painel_situacional`, permissão que também pode alcançar supervisão e TI. Resposta HTTP observada em produção [nao verificado]. [app/ponto/urls.py:78] [app/ponto/views.py:452–456,467–482,496–500] [app/colaboradores/mixins.py:322–331,355–377] [app/docs/LEIS.md:78]
- Callers de teste em `test_porta_batida.py:80,81` deixam exceções subir. Em `test_juiz_unico_eventos.py:273,305,314,368,401` também. O caller de `:352` captura com `assertRaises(DatabaseError)`, afirma identidade, preservação das linhas e PK, zero trilha/savepoint/commit/on_commit e um rollback. O teste e o mutante `f4d-b2-protecao-e-savepoint` pararam no import de `django.db` nesta cópia. O mutante anunciou `FAIL=0 ERROR=1`, RC 2. A mordida em execução permanece [nao verificado]. Por inspeção, o mutante engole a exceção que o `assertRaises` exige. [app/ponto/tests/test_porta_batida.py:77–83] [app/ponto/tests/test_juiz_unico_eventos.py:318–360] [app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134,225–232] [nao verificado]

**Contraexemplo adicional para B1.** T1 regulariza e mantém `FOR UPDATE` no colaborador C. T2 bate pelo app no mesmo instante que T1 vai registrar ou usar como novo horário de uma correção. T2 chega primeiro ao INSERT de Batida e reserva a chave única `(C, timestamp)`, depois pede `FOR KEY SHARE` de C pelo FK. T1 chega ao INSERT ou UPDATE sobre a mesma chave e aguarda T2. O código aceita a mesma chave temporal nas duas portas. O conflito `FOR UPDATE` × `KEY SHARE` está documentado no teste equivalente de Posto/FK. Execução do ciclo específico de Batida e ocorrência em produção [nao verificado]. [app/chamados/services/regularizacao_ext.py:73–83,122–123,144–157] [app/chamados/models.py:2939] [app/api/views_core.py:483–495,643–659] [app/api/hora_do_aparelho.py:135–146] [app/ponto/registro_batida.py:133–148,178–180] [app/ponto/models.py:22,102–108] [app/colaboradores/tests/test_feriado_matriz_p3_trava.py:5–10] [nao verificado]

## 26/11 17:36 · sessao 2d4c89ae68b2 · O30-F3-CONSTRUIR · verificador · origem=Code

**REPROVADO no pré-verificador c10.**

1. **Falta a prova com Pillow e PDF reais do item 1.** O verde e o mutante real pararam no `import PIL`. A queda do mutante com mock foi confirmada. O resultado com as dependências reais fica [nao verificado]. `test_atestado_f3_contrato.py:821-850` · `mutantes_atestado_f3.py:120-125,387-390` [nao verificado]
2. **A c10 perdeu o aviso em uma falha ocorrida dentro da própria reabertura.** Com upload já gravado, `KeyboardInterrupt` em `mudar_estado` sob `atomic` externo gerou o aviso na c9 reconstruída e saiu em silêncio na c10. Reproduzi em memória. Considerei essa saída excepcional parte de “falha loga”, porque ela ocorre antes do retorno e também pode deixar o anexo órfão. `ausencia.py:1413-1428,1434-1445` · `mutantes_atestado_f3.py:134-136`

Os quatro blobs da cópia conferem com o `delta.patch` fornecido. Os dois arquivos de serviço reconstruídos em memória também conferem com os blobs c9 do patch, e os dois REDs correspondentes falharam nas asserções esperadas. A confirmação dos hashes de **commit** e a ocorrência dos contraexemplos em produção ficam [nao verificado]. `mutantes_atestado_f3.py:92-93,391-395` · `test_atestado_f3_contrato.py:228-240,503-507` [nao verificado]

### 1. Os quatro itens do Opus

| Item | Sítio e teste que morde | Resultado | [nao verificado]
|---|---|---|
| 1. `load()` explícito e JPEG truncado | `atestados_lote.py:147-152` · `test_atestado_f3_contrato.py:801-815,821-850` | Código e teste real presentes. O mutante composto sem `load()` e com transposição fora do `try` caiu no mock com `OSError: truncado`. Retirar **só** o `load()` deixou o teste comportamental verde e derrubou o selo AST. A transposição do mock também chama `load()`. Mutante real [nao verificado]. `mutantes_atestado_f3.py:108-125` · `test_atestado_f3_contrato.py:239-240,763-765` | [nao verificado]
| 2. Corpo exato do `try` | `atestados_lote.py:147-158` · `test_atestado_f3_contrato.py:228-240` | Curado. `thumbnail` dentro do `try` derrubou a igualdade AST. `mutantes_atestado_f3.py:126-130` |
| 3. Uma marca e rodapé igual | `atestados_lote.py:201-214,225-232` · `test_atestado_f3_contrato.py:809-815,844-850` | Curado. A marca duplicada caiu com `2 != 1`. O mutante que zera o incremento também caiu, com rodapé `0` para uma marca. `mutantes_atestado_f3.py:131-133,140-142` |
| 4. Aviso na falha sob `atomic` externo | `ausencia.py:1434-1445` · `test_atestado_f3_contrato.py:503-538` | Curado para `Exception`. O mutante que repõe o `finally` caiu em `assertNoLogs`, e o que remove o aviso caiu em `assertLogs`. A saída por `BaseException` ficou sem aviso, conforme o bloqueio 2. `mutantes_atestado_f3.py:134-139` |

### 2. `_foto_no_pdf` e o rodapé

- O tratador cobre `Image.open`, a entrada e a saída do `with`, `original.load()` e `exif_transpose`, para `UnidentifiedImageError` e `OSError`. `thumbnail`, `save`, a construção da imagem do PDF e a abertura do documento no storage ficam fora. `atestados_lote.py:147-158,168-175`
- **Um anexo inacessível ainda pode derrubar o lote.** Contraexemplo reproduzido com mock: `exists()` devolve `True` e `documento.open('rb')` levanta `FileNotFoundError`, como na janela em que o arquivo some entre as chamadas. A exceção chega à montagem do PDF. Se `exists()` já devolve `False`, sai `Anexo indisponível`, com incremento zero. `atestados_lote.py:168-178,210-216` · `views_atestado.py:134-138`
- **Uma imagem decodificada pode virar `ILEGIVEL`.** No contraexemplo em memória, `load()` e a transposição completam, mas a saída do `with` levanta `OSError`. O resultado foi uma marca, rodapé `1` e nenhuma imagem embutida. Um anexo de tipo desconhecido segue como link, inclusive quando a leitura do cabeçalho falha e devolve `(None, None)`. `atestados_lote.py:147-152,171-178,211-214` · `ausencia.py:796-817`
- **O rodapé conta exatamente as marcas geradas pelo ramo `anexo is None`**, uma por passagem, e repete o total em cada página. Uma busca textual tem outro universo: `atestado_medico='ANEXO ILEGIVEL'` imprime a expressão no texto clínico mesmo com uma foto boa e rodapé `0`. `atestados_lote.py:201-214,225-232` · `forms.py:13-15`

### 3. `_reabrir` e o rollback do chamador

O aviso específico exige uma `Exception` que chegue ao tratador, upload detectado e `conexao.in_atomic_block` ainda verdadeiro. Sem atomic externo, o ramo tenta compensar e emite outro aviso. Em sucesso, `_reabrir` devolve a instância com `_criada=False` e `_reaberta=True`, sem emitir o aviso de órfão. `ausencia.py:1406-1421,1434-1445`

**Contraexemplo reproduzido em memória:** o chamador abre um atomic, recebe o retorno de `_reabrir` e só então levanta `RuntimeError`. O rollback restaura a referência anterior no banco simulado. A instância em memória ainda aponta para o nome novo, e o arquivo novo fica no storage. Sem outra referência ou compensação do chamador, sobra um órfão sem aviso de `_reabrir`. O teste de sucesso c10 para antes desse rollback. `ausencia.py:1413-1421,1434-1445` · `test_atestado_f3_contrato.py:409-431,503-513` [nao verificado]

**INSS tem os dois caminhos.** Se a falha vem depois do retorno, a view lê o nome restaurado e tenta compensar. Se `_reabrir` levanta antes de devolver, a atribuição de `ausencia` nunca completa, e a view pula essa limpeza. Para `Exception` com upload detectado, fica o aviso da porta. `views.py:2563-2579,2588-2601` · `ausencia.py:249-264,1434-1442`

### 4. Contagem AST c9 → c10

| Arquivo | `ExceptHandler` | `suppress` |
|---|---:|---:|
| `atestados_lote.py:151,263,271` | 3 → 3 | 0 → 0 |
| `ausencia.py:266,605,611,643,807,845,1132,1307,1345,1434,1511,1560,1818` | 13 → 13 | 0 → 0 |
| `mutantes_atestado_f3.py:101-139,192-196,350-370` | 0 → 0 | 0 → 0 |
| `test_atestado_f3_contrato.py:51-72,228-240,503-538,801-850` | 0 → 0 | 0 → 0 |

O `finally` externo saiu. O `finally` interno que registra a gravação do upload continua. Em `mutantes_atestado_f3.py`, `suppress` aparece como texto de mutante. `ausencia.py:1416-1421,1434-1445` · `mutantes_atestado_f3.py:192-196`

### 5. Classes já reprovadas

- **Sem ocorrência nova no delta** de segundo juiz, segundo escritor fora da porta, relógio fora da borda, derivação paralela, ordem de trava ou campo persistido com dois significados. A classificação segue em `tipo_real_documento`, a reabertura segue usando `estado_inicial` e `mudar_estado`, e a trava da gravação dos seis campos segue na mesma função. `atestados_lote.py:147-175` · `ausencia.py:301-324,796-817,1403-1433`
- **Selo/RED:** os mutantes pedidos que puderam rodar morderam. O selo novo tem uma lacuna verificável: trocar o tipo do handler por `except Exception` deixou verdes o selo do corpo, o censo de tratadores e o teste de decode com mock. O selo compara o corpo, e o censo conta tratadores. A c10 efetivamente entregue conserva o par de exceções. `atestados_lote.py:151` · `test_atestado_f3_contrato.py:51-72,222-240,801-815` [nao verificado]

### 6. Quem escreve e quem lê

| Campo ou valor | Quem escreve ou produz | Quem lê ou consome |
|---|---|---|
| `Ausencia.documento`, nome e bytes | Upload entra pela view. `criar_ausencia`, `anexar_documento` e `_reabrir` passam o documento ao `save` do `FileField`. `views.py:2525-2538` · `models.py:777-781` · `ausencia.py:571-572,638-642,1118-1120,1413-1421` | Histórico lê a presença. PDF lê nome, disponibilidade e bytes. ZIP lê o original. A rota de anexo abre o documento da raiz. Reabertura e compensação leem o nome e suas referências. `dia_decidido.py:679-687` · `atestados_lote.py:168-175,249-261` · `views.py:2146-2161` · `ausencia.py:249-264,1405` |
| `linha['ausencia']`, `linha['documento']`, `registro.pk`, `pode_ver_anexo` | O ORM fornece a ausência e o ID. `linha_do_tempo` monta a linha e a flag booleana. A view calcula a permissão. `atestados_lote.py:36-49,58-63` · `dia_decidido.py:679-687` · `views_atestado.py:99-105` | PDF usa o ID no resumo, link e marca, e as duas flags para decidir se lê o anexo. A reabertura usa o mesmo ID no aviso. `atestados_lote.py:176-178,190-216` · `ausencia.py:1437-1441` |
| `arquivo`, `tipo`, `original`, `foto`, retorno `None` | `_anexo_no_pdf` abre o stream. `tipo_real_documento` fornece o tipo pelos bytes. Pillow abre, carrega e transpõe. `_foto_no_pdf` produz a imagem ou `None`. `atestados_lote.py:147-158,168-175` · `ausencia.py:789-817` | `_foto_no_pdf` usa o tipo para escolher JPEG ou PNG. `_anexo_no_pdf` propaga `None`. `pdf_bytes` lê esse valor para gerar a marca e incrementar o contador. `atestados_lote.py:153-158,172-175,210-216` |
| Marca, `anexos_ilegiveis` e bytes do PDF | `pdf_bytes` produz a marca e incrementa no mesmo ramo. O rodapé imprime o total. `atestados_lote.py:201-214,225-234` | ReportLab consome os blocos e o callback. A view envia os bytes. Os testes leem a marca, a imagem e o rodapé. `atestados_lote.py:231-234` · `views_atestado.py:134-150` · `test_atestado_f3_contrato.py:809-815,844-850` | [nao verificado]
| `banco`, `in_atomic_block`, `_committed`, `_nome_antes`, `anexo_gravado` | O router fornece o alias. A conexão acompanha o atomic. O `save` do `FileField` grava o upload, e `_reabrir` captura o nome anterior e a dupla `(storage, nome gravado)`. `ausencia.py:1405-1421` · `views.py:2569-2579` | `_reabrir` lê a dupla e o estado da conexão para escolher compensação ou aviso. A compensação lê o nome anterior e consulta as referências ao nome gravado. `ausencia.py:249-264,1434-1442` |
| Aviso e flags de retorno | `_reabrir` emite o aviso pelo logger `ponto` e marca a instância no caminho de sucesso. `ausencia.py:49,1439-1445` | A configuração SaaS encaminha o logger ao handler. Os testes capturam o aviso e leem `_reaberta`. A API e `garantir_ausencia` leem `_criada`. `saas.py:160-166,181` · `test_atestado_f3_contrato.py:503-538` · `api/views.py:1852-1858` · `ausencia.py:690-691` |
| Seis campos `atestado_*` e `documento_referencia_id` | O formulário fornece os seis valores. A criação grava no INSERT. Edição, anexação e reabertura passam pelo gravador comum. O vínculo é declarado no modelo. `forms.py:13-22` · `views_atestado.py:31-50` · `ausencia.py:301-324,567,640-642,744-746,1114-1115,1409-1411` · `models.py:815-819` | O gravador lê os valores atuais e a referência para recusar a gravação na filha. PDF, CSV e painel leem os seis campos. O vínculo também é lido pela rota que serve o anexo. `ausencia.py:292-324` · `atestados_lote.py:104-108,122-138,205-209` · `painel_ausencia.html:100-131` · `views.py:2146-2149` |

_8462 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
