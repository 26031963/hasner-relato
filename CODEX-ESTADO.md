# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 11:26** · publicado em **06/11/2026 11:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 11:26 · sessao 5710202ccc04 · R1-CAUSAD-V3 · leitura · origem=Code

```python
import ast
import datetime as dt
import gc
import io
import logging
import re
import sys
import time
import tokenize
from collections import Counter
from contextlib import redirect_stderr, redirect_stdout
from decimal import Decimal, localcontext
from pathlib import Path
from unittest.mock import patch

PARTE = 1
PARTES = 6

sys.dont_write_bytecode = True


def sonda_r1():
    # Proposta sem execução; desempenho e compatibilidade: [nao verificado].
    saida = sys.stdout
    inicio = time.monotonic()
    limite = inicio + 15 * 60
    rastreador_anterior = sys.gettrace()
    competencia = "-"
    raiz = None
    prefixo_app = None
    codigo_handle = None
    bruto = None
    chamadas = 0
    universo_parte_total = 0
    erros_total = 0
    erros_por_tipo = Counter()
    somas = {}
    quantidades = {}
    selecionados = Counter()
    arquivos = {}
    codigos = {}
    pendentes = {}
    pulsos = 0
    marca = object()
    resetar_queries = None
    invalidar_catalogo = None

    DONOS = ("ESTRUTURA", "CADASTRO", "BATIDA")
    CHAVES = (
        "dias_comparados", "bate_ate_10min",
        "esp_zero_e6_trabalho", "e6_zero_esp_trabalho",
        "diverge_10_60", "diverge_acima_60",
        "dia_batida_impar", "impar_que_BATE", "impar_que_DIVERGE",
        "impar_dentro_da_faixa", "impar_FORA_da_faixa",
        "impar_no_conjunto_do_dia", "impar_SO_POR_CORTE_do_oraculo",
        "dia_sem_linha_no_espelho", "dia_sem_lavratura_no_espelho",
        "dia_isento_ou_ausencia", "dia_sem_trabalho_ambos",
        "esp_desconta_janela_declarada", "esp_lido_pelo_pago_h",
    )

    class TempoEsgotado(BaseException):
        pass

    class Descartar:
        # Descarte imediato: não acumula stdout/stderr livres em StringIO.
        def write(self, texto):
            return len(texto)

        def flush(self):
            return None

    def imprimir(*partes):
        print(*partes, file=saida, flush=True)

    def data_segura(valor):
        if isinstance(valor, dt.datetime):
            return valor.date().isoformat()
        if isinstance(valor, dt.date):
            return valor.isoformat()
        if isinstance(valor, str) and re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
            return dt.date.fromisoformat(valor).isoformat()
        return None

    def contexto(frame):
        col, dia = None, None
        while frame is not None:
            loc = frame.f_locals
            if col is None:
                objetos = [loc.get("colaborador"), loc.get("colab"), loc.get("c")]
                obj_self = loc.get("self")
                if obj_self is not None:
                    objetos.append(getattr(obj_self, "__dict__", {}).get("c"))
                for obj in objetos:
                    meta = getattr(type(obj), "_meta", None)
                    if getattr(meta, "model_name", None) == "colaborador":
                        pk = obj.pk
                        if isinstance(pk, int) and not isinstance(pk, bool):
                            col = "col%d" % pk
                            break
                if col is None:
                    for chave in ("colaborador_id", "colab_id"):
                        pk = loc.get(chave)
                        if isinstance(pk, int) and not isinstance(pk, bool):
                            col = "col%d" % pk
                            break
            if dia is None:
                # k é o dia julgado; dia já foi incrementado.
                # app/ponto/management/commands/e6_oraculo.py:321
                # app/ponto/management/commands/e6_oraculo.py:323
                for chave in ("k", "data", "dia", "data_turno"):
                    valor = data_segura(loc.get(chave))
                    if valor is not None:
                        dia = valor
                        break
            frame = frame.f_back
        return col or "-", dia or "-"

    def registrar(exc, frame=None):
        nonlocal erros_total

        # Marca na própria exceção: não conserva exc nem traceback num
        # dicionário global. Evita também deduplicação incorreta por id reusado.
        if exc.__dict__.get("_sonda_r1_registrada") is marca:
            return
        exc.__dict__["_sonda_r1_registrada"] = marca

        tb = exc.__traceback__
        ultimo = None
        while tb is not None:
            ultimo = tb
            tb = tb.tb_next

        col, dia = contexto(frame or (ultimo.tb_frame if ultimo else None))
        tipo = type(exc).__name__
        if not re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]*", tipo):
            tipo = "Excecao"

        origem = "-"
        if ultimo is not None:
            caminho = Path(ultimo.tb_frame.f_code.co_filename)
            if raiz is not None and caminho.is_relative_to(raiz):
                origem = "app/%s:%d" % (
                    caminho.relative_to(raiz).as_posix(), ultimo.tb_lineno
                )
            else:
                origem = "[fora-do-app]:%d" % ultimo.tb_lineno

        # Mesmo contrato de sanitização do anexo: mensagem livre suprimida.
        ultima_linha = (tipo + ": [mensagem suprimida]")[:200]
        erros_total += 1
        erros_por_tipo[tipo] += 1

        # Emite imediatamente; não guarda uma lista crescente de erros.
        imprimir("ERRO", competencia, (col, dia, tipo, origem, ultima_linha))

    def linhas_de_handlers(codigo):
        nome = codigo.co_filename
        if prefixo_app is None or not nome.startswith(prefixo_app):
            # Não memoiza todos os códigos externos vistos pelo tracer.
            return frozenset()
        if codigo in codigos:
            return codigos[codigo]
        if nome not in arquivos:
            with tokenize.open(nome) as fonte:
                arvore = ast.parse(fonte.read(), filename=nome)
            arquivos[nome] = {
                item.body[0].lineno
                for item in ast.walk(arvore)
                if isinstance(item, ast.ExceptHandler) and item.body
            }
        linhas_codigo = {linha for _, _, linha in codigo.co_lines() if linha}
        codigos[codigo] = frozenset(arquivos[nome] & linhas_codigo)
        return codigos[codigo]

    def copiar_acumuladores(frame):
        nonlocal bruto
        loc = frame.f_locals
        necessarios = ("T", "DONO_H", "DONO_C", "div", "_div_tot")
        if not all(chave in loc for chave in necessarios):
            return

        # Só lê o que a autoridade já acumulou; não refaz julgamento.
        # app/ponto/management/commands/e6_oraculo.py:367
        # app/ponto/management/commands/e6_oraculo.py:368
        # app/ponto/management/commands/e6_oraculo.py:369
        t, h, c = loc["T"], loc["DONO_H"], loc["DONO_C"]
        uniao = set()
        for dono in DONOS:
            uniao.update(c.get(dono, ()))
        bruto = {
            "dias": {dono: int(t.get("dono_" + dono, 0)) for dono in DONOS},
            # Mesma divisão da autoridade, sem a formatação %.1f.
            # app/ponto/management/commands/e6_oraculo.py:423
            "horas": {dono: repr(h.get(dono, 0) / 60.0) for dono in DONOS},
            "colabs": {dono: len(c.get(dono, ())) for dono in DONOS},
            "total_dias": int(loc["_div_tot"]),
            # Mesma união usada na saída nativa.
            # app/ponto/management/commands/e6_oraculo.py:429
            "total_colabs": len({x[1] for x in loc["div"]}),
            "uniao_colabs": len(uniao),
        }

    def rastrear(frame, evento, arg):
        nonlocal pulsos
        if evento == "call":
            pulsos += 1
            if pulsos % 256 == 0 and time.monotonic() >= limite:
                raise TempoEsgotado()
            if frame.f_code is codigo_handle:
                linhas_de_handlers(frame.f_code)
                return rastrear
            return rastrear if linhas_de_handlers(frame.f_code) else None
        if evento == "exception":
            pendentes[id(frame)] = arg[1]
        elif evento == "line":
            if frame.f_lineno in codigos.get(frame.f_code, ()):
                exc = sys.exc_info()[1] or pendentes.get(id(frame))
                if exc is not None:
                    registrar(exc, frame)
        elif evento == "return":
            try:
                if frame.f_code is codigo_handle:
                    copiar_acumuladores(frame)
            finally:
                pendentes.pop(id(frame), None)
        return rastrear

    def receber_log(logger, registro):
        if registro.exc_info and registro.exc_info[1] is not None:
            registrar(registro.exc_info[1], sys._getframe(1))

    def limpar():
        # Pendentes podem conservar exceções/tracebacks enquanto o frame vive.
        pendentes.clear()
        if resetar_queries is not None:
            resetar_queries()
        if invalidar_catalogo is not None:
            # Cache único de catálogo; não é cache crescente por colaborador.
            # app/ponto/catalogo/ausencias.py:257
            # app/ponto/catalogo/ausencias.py:261
            invalidar_catalogo()

        # Não limpa memos de motores vivos nem os acumuladores do comando.
        # Os memos pertencem à instância/cálculo:
        # app/ponto/motor_calculo_v2.py:848
        # app/ponto/motor_calculo_v2.py:2285
        # O registro de fontes é fraco e morre com os motores:
        # app/core/regua_cct.py:487
        # app/core/regua_cct.py:494
        gc.collect()

    def ler_relatorio(texto, erros_antes):
        linhas = texto.splitlines()
        padrao_counter = (
            r"\{(?:'[A-Za-z0-9_]+'\s*:\s*\d+"
            r"(?:,\s*'[A-Za-z0-9_]+'\s*:\s*\d+)*)?\}"
        )
        contadores = [
            ast.literal_eval(linha)
            for linha in linhas if re.fullmatch(padrao_counter, linha)
        ]
        donos = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*(ESTRUTURA|CADASTRO|BATIDA)\s+"
                r"(\d+) dia-colab\s+(\d+\.\d+) h\s+(\d+) colabs",
                linha
            ))
        ]
        soma = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*SOMA\s+(\d+)\s+"
                r"<- tem de ser igual a soma das classes \((\d+)\)", linha
            ))
        ]
        colabs = [
            m for linha in linhas
            if (m := re.fullmatch(r"colabs com divergencia: (\d+)", linha))
        ]
        falhas = [
            m for linha in linhas
            if (m := re.fullmatch(r"erros no espelho: (\d+)", linha))
        ]
        if not (
            len(contadores) == len(soma) == len(colabs) == len(falhas) == 1
            and len(donos) == 3
            and {m[1] for m in donos} == set(DONOS)
            and bruto is not None
        ):
            raise RuntimeError("FormatoDaAutoridadeIncompleto")

        soma_dias = sum(bruto["dias"].values())
        confere_dias = (
            soma_dias == bruto["total_dias"]
            == int(soma[0][1]) == int(soma[0][2])
        )
        confere_colabs = (
            bruto["uniao_colabs"] == bruto["total_colabs"] == int(colabs[0][1])
        )
        for m in donos:
            dono = m[1]
            if (
                int(m[2]) != bruto["dias"][dono]
                or int(m[4]) != bruto["colabs"][dono]
                or m[3] != format(float(bruto["horas"][dono]), ".1f")
            ):
                raise RuntimeError("AcumuladoresDaAutoridadeDivergentes")

        if not confere_dias or not confere_colabs:
            raise RuntimeError("SomaDaAutoridadeDivergente")
        if selecionados[competencia] != quantidades[competencia]:
            raise RuntimeError("UniversoDaParteDivergente")
        if int(falhas[0][1]) and erros_total == erros_antes:
            raise RuntimeError("ErroDoEspelhoSemDetalhamento")

        imprimir("COMPETENCIA", competencia)
        t = contadores[0]
        imprimir("[1]", {chave: t.get(chave, 0) for chave in CHAVES})
        for linha in linhas:
            if re.fullmatch(
                r"BATE \(ate 10 min\): \d+\.\d+% de \d+ dias comparados", linha
            ):
                imprimir("[2]", linha)
        for m in donos:
            imprimir("[3]", m[0].strip())
        imprimir("[4] TOTAL divergentes", soma[0][2])
        imprimir("[5]", colabs[0][0])
        imprimir("erros_no_espelho", falhas[0][1])

        # Decimal soma exatamente as representações decimais emitidas.
        # Não usa as horas arredondadas da tabela nativa.
        with localcontext() as ctx:
            ctx.prec = 80
            horas = {dono: Decimal(bruto["horas"][dono]) for dono in DONOS}
            total_horas = sum(horas.values(), Decimal(0))
            texto_total_horas = format(total_horas, "f")
            confere_horas = (
                sum((Decimal(bruto["horas"][d]) for d in DONOS), Decimal(0))
                == Decimal(texto_total_horas)
            )

        for dono in DONOS:
            imprimir(
                "PARCIAL|%s|%s|dias=%d|horas=%s|colabs=%d" % (
                    competencia, dono, bruto["dias"][dono],
                    bruto["horas"][dono], bruto["colabs"][dono],
                )
            )
        imprimir(
            "PARCIAL_TOTAL|%s|dias=%d|horas=%s|colabs=%d" % (
                competencia, bruto["total_dias"],
                texto_total_horas, bruto["total_colabs"],
            )
        )
        somas[competencia] = (
            soma[0][0].strip(), confere_dias,
            texto_total_horas, confere_horas,
            bruto["total_colabs"], confere_colabs,
        )

    try:
        with (
            patch.object(logging.Logger, "handle", receber_log),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.core.management import call_command
            from django.db import connection, reset_queries, transaction
            from ponto import janelas
            from ponto.catalogo import ausencias
            from ponto.management.commands import e6_oraculo
            from ponto.services import espelho as modulo_espelho

            raiz = Path(e6_oraculo.__file__).resolve().parents[3]
            prefixo_app = str(raiz) + "/"
            codigo_handle = e6_oraculo.Command.handle.__code__
            resetar_queries = reset_queries
            invalidar_catalogo = ausencias.invalidar_cache

            if (
                type(PARTE) is not int
                or type(PARTES) is not int
                or PARTES != 6
                or not 1 <= PARTE <= PARTES
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
                or rastreador_anterior is not None
            ):
                raise RuntimeError("AmbienteIncompativel")

            Empresa = apps.get_model("colaboradores", "Empresa")
            FM = apps.get_model("ponto", "FechamentoMensal")
            Colaborador = apps.get_model("colaboradores", "Colaborador")
            gerente = Colaborador.objects
            filtro_real = gerente.filter
            real_espelho = modulo_espelho.espelho_do_colab

            with transaction.atomic():
                try:
                    with connection.cursor() as cursor:
                        cursor.execute("SELECT current_database(), current_user")
                        if cursor.fetchone() != ("sombra", "leitor"):
                            raise RuntimeError("BancoOuPapelIncompativel")

                    empresas = list(
                        Empresa.objects.filter(ativa=True).order_by("pk")
                    )
                    imprimir("PARTE", PARTE, "PARTES", PARTES)
                    for mes in (10, 11):
                        competencia = "%02d/2026" % mes
                        total = quantidade = 0
                        ids = (
                            FM.objects.filter(
                                mes=mes, ano=2026,
                                colaborador__empresa__ativa=True,
                            )
                            .values_list("colaborador_id", flat=True)
                            .distinct()
                            .iterator(chunk_size=128)
                        )
                        try:
                            for pk in ids:
                                if type(pk) is not int:
                                    raise RuntimeError("IdentificadorIncompativel")
                                total += 1
                                quantidade += int(pk % PARTES == PARTE - 1)
                        finally:
                            ids.close()
                        quantidades[competencia] = quantidade
                        universo_parte_total += quantidade
                        imprimir("[0] UNIVERSO", competencia, total)
                        imprimir("UNIVERSO_PARTE", competencia, quantidade)

                        janelas_vistas = {
                            janelas.janela_fechamento(mes, 2026, empresa)
                            for empresa in empresas
                        }
                        for ini, fim in sorted(janelas_vistas):
                            imprimir(
                                "JANELA", competencia,
                                ini.isoformat(), fim.isoformat(),
                            )
                    del empresas
                finally:
                    transaction.set_rollback(True)
            limpar()

            def iterar_colaboradores(queryset):
                it = queryset.iterator(chunk_size=1)
                try:
                    for c in it:
                        if time.monotonic() >= limite:
                            raise TempoEsgotado()
                        if c.pk % PARTES != PARTE - 1:
                            raise RuntimeError("ColaboradorForaDaParte")
                        selecionados[competencia] += 1
                        try:
                            yield c
                        finally:
                            # O próximo passo do iterador acontece depois de
                            # toda a comparação/juiz deste colaborador.
                            # app/ponto/management/commands/e6_oraculo.py:302
                            # app/ponto/management/commands/e6_oraculo.py:392
                            limpar()
                finally:
                    it.close()

            def filtrar_colaboradores(*args, **kwargs):
                # Intercepta só a seleção de colaboradores do handle real.
                # app/ponto/management/commands/e6_oraculo.py:302
                if sys._getframe(1).f_code is not codigo_handle:
                    return filtro_real(*args, **kwargs)
                if args or set(kwargs) != {"pk__in"}:
                    raise RuntimeError("SelecaoDaAutoridadeIncompativel")
                ids = kwargs["pk__in"]
                if any(type(pk) is not int for pk in ids):
                    raise RuntimeError("IdentificadorIncompativel")
                ids_parte = [pk for pk in ids if pk % PARTES == PARTE - 1]
                return iterar_colaboradores(filtro_real(pk__in=ids_parte))

            def observar_espelho(*args, **kwargs):
                nonlocal chamadas
                if time.monotonic() >= limite:
                    raise TempoEsgotado()
                if chamadas and chamadas % 25 == 0:
                    decorrido = time.monotonic() - inicio
                    estimativa = (
                        decorrido * universo_parte_total / chamadas / 60
                    )
                    imprimir(
                        "ESTIMATIVA", "processados", chamadas,
                        "universo", universo_parte_total,
                        "minutos_total", round(estimativa, 1),
                        "ate_40_min", estimativa 

[truncado em 20000 de 27052 caracteres]

## 06/11 11:23 · sessao e60937eb81bd · - · - · origem=Ronald (tmux)

Contagem manual do **primeiro anexo `script.py`**, considerando `def sonda_r1():` como linha 1:

- `vistos = {}` — `script.py:19`.
- `vistos[chave] = exc` — `script.py:86`.
- `erros.append(...)` — `script.py:106`.
- `pendentes[id(frame)] = arg[1]` — `script.py:136`.
- `redirect_stdout(io.StringIO())` — `script.py:232`.
- `redirect_stderr(io.StringIO())` — `script.py:233`.

Âncoras: `def rastrear` — `script.py:128`; `def receber_log` — `script.py:146`; `def ler_relatorio` — `script.py:151`; `try:` externo — `script.py:229`.

## 06/11 11:21 · sessao 28b6e5352e01 · - · - · origem=Ronald (tmux)

Ponto de partição seguro: interceptar exclusivamente a chamada `Colaborador.objects.filter(pk__in=ids)` feita pelo `Command.handle`, filtrar a lista de IDs com `pk % PARTES == PARTE - 1` e chamar o filtro original. Identificar o chamador por `frame.f_code is e6_oraculo.Command.handle.__code__` evita alterar consultas internas da autoridade. A seleção nativa percorre empresas ativas e IDs de `FechamentoMensal` da competência (`app/ponto/management/commands/e6_oraculo.py:291`, `:293`, `:302`).

O wrapper pode devolver um gerador sobre `queryset.iterator(chunk_size=1)` e executar limpeza ao retomar após cada `yield`: nesse momento o comando já terminou a comparação daquele colaborador, inclusive suas chamadas ao juiz. O loop completo do colaborador ocorre entre as linhas 302 e 392; o próximo passo do iterador é solicitado após esse trabalho (`app/ponto/management/commands/e6_oraculo.py:302`, `:316`, `:366`, `:392`). Disponibilidade e implementação do `iterator()` Django instalado: [nao verificado].

Para as linhas cruas, aproveitar os acumuladores produzidos pelo próprio `handle`, observando seu frame no retorno:

- Dias por dono: `T['dono_%s' % dono]`, incrementado pelo comando (`app/ponto/management/commands/e6_oraculo.py:367`).
- Minutos por dono: `DONO_H[dono]`, acumulado como `abs(df)`; horas são esse acumulador dividido por 60 (`app/ponto/management/commands/e6_oraculo.py:368`, `:423`).
- Colaboradores por dono: `len(DONO_C[dono])`, conjunto alimentado pelo comando (`app/ponto/management/commands/e6_oraculo.py:369`, `:423`).
- Dias totais: `_div_tot`, a soma nativa das quatro classes divergentes (`app/ponto/management/commands/e6_oraculo.py:424`).
- Colaboradores totais: união de `x[1]` nas divergências, como faz a autoridade (`app/ponto/management/commands/e6_oraculo.py:429`).

Não derivar horas cruas do texto da tabela: ela usa somente uma casa decimal. Somar as seis tabelas arredondadas pode produzir diferença em relação ao acumulador original (`app/ponto/management/commands/e6_oraculo.py:422`, `:423`).

Não exigir que a soma de `colabs` dos três donos coincida com o total: o código permite que o mesmo colaborador entre nos conjuntos de vários donos e calcula o total por união (`app/ponto/management/commands/e6_oraculo.py:369`, `:429`). Dias e horas são particionados por dono; a conferência nativa de dias compara a soma dos donos com a soma das classes (`app/ponto/management/commands/e6_oraculo.py:366`, `:368`, `:415`, `:424`, `:427`).

Entre as seis partes, os colaboradores são disjuntos na mesma competência pela condição de módulo proposta [nao verificado: implementação proposta]. Portanto, somar dias, horas e colaboradores de cada dono entre partes; somar também os três campos de `PARCIAL_TOTAL` entre partes. Não somar colaboradores entre competências para obter pessoas distintas: a seleção é refeita por competência (`app/ponto/management/commands/e6_oraculo.py:284`, `:293`).

Há estado legítimo da autoridade que cresce até terminar a parte: `div` recebe uma linha por dia divergente e `DONO_C` guarda os conjuntos usados na saída. Não limpar esses objetos durante o comando: isso alteraria as classes ou o total de colaboradores (`app/ponto/management/commands/e6_oraculo.py:286`, `:289`, `:369`, `:389`, `:428`, `:429`). Duração aproximada de 15 minutos e consumo abaixo de 2 GiB após as mudanças: [nao verificado].

## 06/11 11:21 · sessao e60937eb81bd · - · - · origem=Ronald (tmux)

- O comando acumula `div` com uma lista por dia divergente e `DONO_C` com identificadores por dono; mantém tudo até publicar o relatório. `app/ponto/management/commands/e6_oraculo.py:286`, `:289`, `:369`, `:389`, `:428`, `:429`.
- Os memos do motor são de instância: `_previsto_min` nasce no construtor e `_escala_certa_cache` nasce por cálculo. Não há `lru_cache`/`cache_clear` nesses módulos examinados; descarte de referências e `gc.collect()` preservam a autoridade. `app/ponto/motor_calculo_v2.py:848`, `:2283`, `:2285`; suficiência para 2 GB: [nao verificado].
- Existe um registro de módulo do motor: `_FONTE_DO_MOTOR`, uma `WeakKeyDictionary`. Ela perde entradas quando os motores deixam de ter referências fortes; não limpar enquanto o motor estiver sendo consultado, pois guarda a fonte usada no carimbo. `app/core/regua_cct.py:480`, `:487`, `:488`, `:494`.
- O oráculo cria mapas locais por chamada, sem retenção global nessa função. `app/ponto/calculador/nucleo.py:109`, `:112`, `:116`, `:118`.
- `InsumosDoDono` retém espelho, turnos, cadastro × realidade e fase por instância; a limpeza deve ocorrer depois de concluir a comparação do colaborador. `app/ponto/management/commands/e6_oraculo.py:131`, `:132`, `:151`, `:160`, `:169`, `:316`, `:366`.
- O catálogo de ausências tem cache de módulo, mas guarda um único catálogo por 60 segundos; não cresce por colaborador. Há `invalidar_cache()`, que redefine as duas entradas. `app/ponto/catalogo/ausencias.py:257`, `:258`, `:261`, `:263`, `:264`, `:276`, `:279`.
- As horas nativas disponíveis em `DONO_H` estão em minutos; a tabela arredonda para uma casa após dividir por 60. Para linhas cruas agregáveis, capture os contadores reais antes do arredondamento. `app/ponto/management/commands/e6_oraculo.py:368`, `:422`, `:423`.
- Colaboradores por dono podem sobrepor-se; o total exige união, como já faz a autoridade com `div`. `app/ponto/management/commands/e6_oraculo.py:369`, `:429`.
- Os valores declarados padrão são quatro CPUs e memória de 2 GB; aplicação efetiva nessa execução: [nao verificado]. `bin/recursos.sh:61`, `:69`.

## 06/11 11:17 · sessao db14731f5c6f · R1-CAUSAD-V3 · sonda · origem=Code

Abaixo está a saída integral recebida, sem nova execução. O wrapper executa `--sonda` antes desta sessão e inclui o resultado raspado no prompt (`bin/codex_pergunta.sh:103`, `bin/codex_pergunta.sh:133`).

```text
sonda_leitura: script.py [a7c2bdd710f71626] no cpuset 4-7, lendo sombra como leitor (LEITURA)

col144 21/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
  ENTRA app/chamados/services/fabrica_da_celula.py:74:motivos_a_perguntar
  ENTRA app/chamados/juizes.py:413:dia_sem_trabalho
  RETURN app/chamados/juizes.py:428:dia_sem_trabalho True
  DEVOLVE dia_sem_trabalho True
  RETURN app/chamados/services/fabrica_da_celula.py:92:motivos_a_perguntar list_n=0
  DEVOLVE motivos_a_perguntar list_n=0
  ENTRA app/chamados/services/fabrica_da_celula.py:180:limbo_vai_a_pauta
  RETURN app/chamados/services/fabrica_da_celula.py:186:limbo_vai_a_pauta False
  DEVOLVE limbo_vai_a_pauta False
  RETURN app/chamados/services/fabrica_da_celula.py:177:perguntar_pela_celula list_n=0
  CAMINHO_EXECUTADO app/chamados/services/fabrica_da_celula.py:150:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:152:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:153:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:154:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:163:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:164:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:86:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:87:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:88:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:89:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:91:motivos_a_perguntar > app/chamados/juizes.py:422:dia_sem_trabalho > app/chamados/juizes.py:426:dia_sem_trabalho > app/chamados/juizes.py:428:dia_sem_trabalho > app/chamados/services/fabrica_da_celula.py:92:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:165:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:171:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:185:limbo_vai_a_pauta > app/chamados/services/fabrica_da_celula.py:186:limbo_vai_a_pauta > app/chamados/services/fabrica_da_celula.py:174:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:177:perguntar_pela_celula
  ATA_PASSAGENS|descartes_L61=0|avaliacoes_L63=0|adicoes_L64=0|app/chamados/services/fabrica_da_celula.py:59
col137 03/11/2026|G2|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
  ENTRA app/chamados/services/fabrica_da_celula.py:74:motivos_a_perguntar
  ENTRA app/chamados/juizes.py:413:dia_sem_trabalho
  RETURN app/chamados/juizes.py:428:dia_sem_trabalho True
  DEVOLVE dia_sem_trabalho True
  RETURN app/chamados/services/fabrica_da_celula.py:92:motivos_a_perguntar list_n=0
  DEVOLVE motivos_a_perguntar list_n=0
  ENTRA app/chamados/services/fabrica_da_celula.py:180:limbo_vai_a_pauta
  RETURN app/chamados/services/fabrica_da_celula.py:186:limbo_vai_a_pauta False
  DEVOLVE limbo_vai_a_pauta False
  RETURN app/chamados/services/fabrica_da_celula.py:177:perguntar_pela_celula list_n=0
  CAMINHO_EXECUTADO app/chamados/services/fabrica_da_celula.py:150:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:152:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:153:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:154:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:163:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:164:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:86:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:87:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:88:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:89:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:91:motivos_a_perguntar > app/chamados/juizes.py:422:dia_sem_trabalho > app/chamados/juizes.py:426:dia_sem_trabalho > app/chamados/juizes.py:428:dia_sem_trabalho > app/chamados/services/fabrica_da_celula.py:92:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:165:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:171:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:185:limbo_vai_a_pauta > app/chamados/services/fabrica_da_celula.py:186:limbo_vai_a_pauta > app/chamados/services/fabrica_da_celula.py:174:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:177:perguntar_pela_celula
  ATA_PASSAGENS|descartes_L61=0|avaliacoes_L63=0|adicoes_L64=0|app/chamados/services/fabrica_da_celula.py:59
col222 28/10/2026|G3|PARCIAL_SO_LISTA_RETORNO_399|app/chamados/services/fabrica_da_celula.py:102;app/ponto/services/cartorio.py:399
  ENTRA app/chamados/services/fabrica_da_celula.py:74:motivos_a_perguntar
  ENTRA app/chamados/juizes.py:413:dia_sem_trabalho
  RETURN app/chamados/juizes.py:427:dia_sem_trabalho False
  DEVOLVE dia_sem_trabalho False
  ENTRA app/chamados/services/fabrica_da_celula.py:15:vencidos_no_instante
  ENTRA app/ponto/services/cartorio.py:831:marcos_vencidos
  ENTRA app/escala/servico_jornada.py:15:escala_vigente
  RETURN app/escala/servico_jornada.py:57:escala_vigente EscalaColaborador
  DEVOLVE escala_vigente EscalaColaborador
  RETURN app/ponto/services/cartorio.py:848:marcos_vencidos tuple_n=2
  DEVOLVE marcos_vencidos tuple_n=2
  RETURN app/chamados/services/fabrica_da_celula.py:31:vencidos_no_instante set_n=4
  DEVOLVE vencidos_no_instante set_n=4
  ENTRA app/ponto/services/cartorio.py:370:parcial_so_lista
  RETURN app/ponto/services/cartorio.py:399:parcial_so_lista True
  DEVOLVE parcial_so_lista True
  RETURN app/chamados/services/fabrica_da_celula.py:104:motivos_a_perguntar list_n=0
  DEVOLVE motivos_a_perguntar list_n=0
  ENTRA app/chamados/services/fabrica_da_celula.py:180:limbo_vai_a_pauta
  RETURN app/chamados/services/fabrica_da_celula.py:186:limbo_vai_a_pauta False
  DEVOLVE limbo_vai_a_pauta False
  RETURN app/chamados/services/fabrica_da_celula.py:177:perguntar_pela_celula list_n=0
  CAMINHO_EXECUTADO app/chamados/services/fabrica_da_celula.py:150:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:152:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:153:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:154:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:163:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:164:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:86:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:87:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:88:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:89:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:91:motivos_a_perguntar > app/chamados/juizes.py:422:dia_sem_trabalho > app/chamados/juizes.py:426:dia_sem_trabalho > app/chamados/juizes.py:427:dia_sem_trabalho > app/chamados/services/fabrica_da_celula.py:93:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:95:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:98:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:27:vencidos_no_instante > app/chamados/services/fabrica_da_celula.py:28:vencidos_no_instante > app/ponto/services/cartorio.py:840:marcos_vencidos > app/ponto/services/cartorio.py:841:marcos_vencidos > app/escala/servico_jornada.py:38:escala_vigente > app/escala/servico_jornada.py:39:escala_vigente > app/escala/servico_jornada.py:40:escala_vigente > app/escala/servico_jornada.py:41:escala_vigente > app/escala/servico_jornada.py:49:escala_vigente > app/escala/servico_jornada.py:55:escala_vigente > app/escala/servico_jornada.py:56:escala_vigente > app/escala/servico_jornada.py:57:escala_vigente > app/ponto/services/cartorio.py:842:marcos_vencidos > app/ponto/services/cartorio.py:844:marcos_vencidos > app/ponto/services/cartorio.py:845:marcos_vencidos > app/ponto/services/cartorio.py:846:marcos_vencidos > app/ponto/services/cartorio.py:847:marcos_vencidos > app/ponto/services/cartorio.py:846:marcos_vencidos > app/ponto/services/cartorio.py:845:marcos_vencidos > app/ponto/services/cartorio.py:846:marcos_vencidos > app/ponto/services/cartorio.py:847:marcos_vencidos > app/ponto/services/cartorio.py:846:marcos_vencidos > app/ponto/services/cartorio.py:845:marcos_vencidos > app/ponto/services/cartorio.py:846:marcos_vencidos > app/ponto/services/cartorio.py:847:marcos_vencidos > app/ponto/services/cartorio.py:846:marcos_vencidos > app/ponto/services/cartorio.py:845:marcos_vencidos > app/ponto/services/cartorio.py:846:marcos_vencidos > app/ponto/services/cartorio.py:847:marcos_vencidos > app/ponto/services/cartorio.py:846:marcos_vencidos > app/ponto/services/cartorio.py:845:marcos_vencidos > app/ponto/services/cartorio.py:848:marcos_vencidos > app/chamados/services/fabrica_da_celula.py:29:vencidos_no_instante > app/chamados/services/fabrica_da_celula.py:31:vencidos_no_instante > app/chamados/services/fabrica_da_celula.py:99:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:101:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:102:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:103:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:102:motivos_a_perguntar > app/ponto/services/cartorio.py:395:parcial_so_lista > app/ponto/services/cartorio.py:396:parcial_so_lista > app/ponto/services/cartorio.py:398:parcial_so_lista > app/ponto/services/cartorio.py:399:parcial_so_lista > app/chamados/services/fabrica_da_celula.py:104:motivos_a_perguntar > app/chamados/services/fabrica_da_celula.py:165:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:171:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:185:limbo_vai_a_pauta > app/chamados/services/fabrica_da_celula.py:186:limbo_vai_a_pauta > app/chamados/services/fabrica_da_celula.py:174:perguntar_pela_celula > app/chamados/services/fabrica_da_celula.py:177:perguntar_pela_celula
  ATA_PASSAGENS|descartes_L61=0|avaliacoes_L63=0|adicoes_L64=0|app/chamados/services/fabrica_da_celula.py:59
QUADRO|grupo|guarda_ou_destino|n|exemplo1|exemplo2|fonte
QUADRO|G1|ATA_NAO_AUTORIZA|8|col969/22/10/2026|col969/24/10/2026|app/chamados/services/fabrica_da_celula.py:51
QUADRO|G1|DIA_SEM_TRABALHO|165|col144/21/09/2026|col144/23/09/2026|app/chamados/services/fabrica_da_celula.py:91
QUADRO|G1|ESCRITA_INTERCEPTADA_ChamadoColaborador.save|2|col1027/04/11/2026|col1028/04/11/2026|app/chamados/models.py:932:abrir
QUADRO|G1|ESCRITA_INTERCEPTADA_DisputaSupervisao.save|7|col144/05/11/2026|col209/05/11/2026|app/chamados/services/disputa_emissao.py:501:_abrir_ou_agregar_disputa
QUADRO|G2|DIA_SEM_TRABALHO|5|col137/03/11/2026|col137/05/11/2026|app/chamados/services/fabrica_da_celula.py:91
QUADRO|G2|PARCIAL_SO_LISTA_RETORNO_399|8|col258/28/09/2026|col258/19/10/2026|app/chamados/services/fabrica_da_celula.py:102;app/ponto/services/cartorio.py:399
QUADRO|G3|DIA_SEM_TRABALHO|3|col292/22/10/2026|col348/24/10/2026|app/chamados/services/fabrica_da_celula.py:91
QUADRO|G3|PARCIAL_SO_LISTA_RETORNO_399|2|col222/28/10/2026|col892/28/09/2026|app/chamados/services/fabrica_da_celula.py:102;app/ponto/services/cartorio.py:399
DIAS|fonte=app/chamados/services/vigia_fio.py:542;app/ponto/management/commands/e6_oraculo.py:172
col137 05/11/2026|G2|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 23/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 25/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 27/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 29/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 01/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 03/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 05/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 07/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 09/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 11/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 13/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 15/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 17/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 19/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 21/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 23/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 25/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 27/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 29/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 31/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 02/11/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 04/11/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col144 05/11/2026|G1|ESCRITA_INTERCEPTADA_DisputaSupervisao.save|app/chamados/services/disputa_emissao.py:501:_abrir_ou_agregar_disputa
col209 21/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 23/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 25/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 27/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 29/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 01/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 03/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 05/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 07/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 09/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 11/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 13/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 15/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 17/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 19/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 21/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 23/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 25/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 27/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 29/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 31/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 02/11/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 04/11/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col209 05/11/2026|G1|ESCRITA_INTERCEPTADA_DisputaSupervisao.save|app/chamados/services/disputa_emissao.py:501:_abrir_ou_agregar_disputa
col258 28/09/2026|G2|PARCIAL_SO_LISTA_RETORNO_399|app/chamados/services/fabrica_da_celula.py:102;app/ponto/services/cartorio.py:399
col258 19/10/2026|G2|PARCIAL_SO_LISTA_RETORNO_399|app/chamados/services/fabrica_da_celula.py:102;app/ponto/services/cartorio.py:399
col259 14/10/2026|G2|PARCIAL_SO_LISTA_RETORNO_399|app/chamados/services/fabrica_da_celula.py:102;app/ponto/services/cartorio.py:399
col259 15/10/2026|G2|PARCIAL_SO_LISTA_RETORNO_399|app/chamados/services/fabrica_da_celula.py:102;app/ponto/services/cartorio.py:399
col268 21/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 23/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 25/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 27/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 29/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 01/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 03/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 05/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 07/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 09/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 11/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 13/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 15/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 17/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 19/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 21/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 23/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 25/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 27/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 29/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 31/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 02/11/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 04/11/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col268 05/11/2026|G1|ESCRITA_INTERCEPTADA_DisputaSupervisao.save|app/chamados/services/disputa_emissao.py:501:_abrir_ou_agregar_disputa
col292 22/10/2026|G3|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col292 28/10/2026|G2|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 21/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 23/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 25/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 27/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 29/09/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 01/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 03/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 05/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 07/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 09/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 11/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 13/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 15/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 17/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 19/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 21/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 23/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 25/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celula.py:91
col312 27/10/2026|G1|DIA_SEM_TRABALHO|app/chamados/services/fabrica_da_celu

[truncado em 20000 de 31156 caracteres]

## 06/11 11:11 · sessao 391a1b785573 · R1-CAUSAD-V3 · sonda · origem=Code

Não reexecutei: o caminho `--sonda` cria e grava arquivos, contrariando sua regra de só leitura (`bin/codex_pergunta.sh:51`, `bin/codex_pergunta.sh:68`, `bin/codex_pergunta.sh:106`). Segue a saída inteira fornecida no prompt, sem resumir; execução original [nao verificado].

```text
sonda_leitura: script.py [67a53834ff5b72a8] no cpuset 4-7, lendo sombra como leitor (LEITURA)

[0] UNIVERSO 10/2026 624
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 578
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 1202 minutos_total 44.1 ate_40_min False
ESTIMATIVA processados 50 universo 1202 minutos_total 50.8 ate_40_min False
ESTIMATIVA processados 75 universo 1202 minutos_total 53.5 ate_40_min False
ESTIMATIVA processados 100 universo 1202 minutos_total 52.1 ate_40_min False
ESTIMATIVA processados 125 universo 1202 minutos_total 51.2 ate_40_min False
ESTIMATIVA processados 150 universo 1202 minutos_total 52.2 ate_40_min False
ESTIMATIVA processados 175 universo 1202 minutos_total 51.6 ate_40_min False
ESTIMATIVA processados 200 universo 1202 minutos_total 60.1 ate_40_min False
ESTIMATIVA processados 225 universo 1202 minutos_total 59.1 ate_40_min False
ESTIMATIVA processados 250 universo 1202 minutos_total 74.6 ate_40_min False
ESTIMATIVA processados 275 universo 1202 minutos_total 73.2 ate_40_min False
rc_sombra=137
```

- **SOMA: [nao verificado]** — a linha não aparece na saída fornecida; não há resultado final para validar.
- **INCOMPLETO: [nao verificado]** — a linha não aparece na saída fornecida; não é possível recuperar seu conteúdo.
- **Fechamento da execução: [nao verificado]** — o texto termina em `rc_sombra=137`; a causa desse retorno não foi demonstrada.

## 06/11 10:54 · sessao 8d2c4dc0d074 · CODEX-CONSTRUTOR-NEGOCIO · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 06/11 10:53 · sessao 0caa3c46d7af · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Construtor não existe neste hash.** Parser só aceita `--sonda`/`--anexo`, exige três posicionais; papel só admite leitura/censo/verificador/sonda; execução efetiva usa `--sandbox read-only -C "$WS"`. A chamada contratual seria recusada com rc 2. `bin/codex_pergunta.sh:32-48`, `bin/codex_pergunta.sh:174-175`.
- **Regra documentada também é anterior ao contrato:** só leitura; exceção para teste RED em cópia própria; proíbe outro sandbox, produção e configuração do Code. O selo atual reprova qualquer `workspace-write`. `CLAUDE.md:341-348`, `bin/tests/test_codex_pergunta.sh:13-21`.
- **Cópia não é autenticada ao reutilizar:** `WS` aceita `CODEX_WS_BASE`; basta `-d "$WS"` para pular criação/verificação, sem canonicalização ou atestar conteúdo/hash. Isso permite selecionar diretório/symlink preexistente, mas não demonstra escrita fora do sandbox, que continua read-only. `bin/codex_pergunta.sh:87-101`, `bin/codex_pergunta.sh:174`.
- **Saídas do host escapam da política do sandbox do filho:** `RESPOSTA` vem diretamente do argumento e controla lock, remoção do sidecar, destino da resposta e escrita do `.id`; `LOGD` é substituível; `OBRA` só precisa não estar vazia e participa do caminho JSONL. O wrapper não valida confinamento desses destinos. `bin/codex_pergunta.sh:40-51`, `bin/codex_pergunta.sh:162-178`, `bin/codex_pergunta.sh:199-201`.
- **Chamador/concorrência não autenticados:** obra/papel são variáveis; não há prova de sessão principal. `CODEX_TETO` é livre e locks dependem de `LOGD`, logo o seis é default, não teto imutável. `bin/codex_pergunta.sh:44-51`, `bin/codex_pergunta.sh:138-153`.
- **Execução intocada contorna lista de arquivos modificados:** `bin/hooks.sh install` usa raiz fixa na viva e escreve nela; recusar alteração em `bin/` não impede chamar script existente. O sandbox precisa bloquear esses efeitos. `bin/hooks.sh:9-11`, `bin/hooks.sh:42-58`.
- **`.claude/` merece proteção explícita:** configuração contém comandos para a viva no Stop, PreCompact e PreToolUse; sua execução nesta chamada é [nao verificado]. ` .claude/settings.json:9`, `.claude/settings.json:20`, `.claude/settings.json:29`, `.claude/settings.json:68`.
- **Hook substituível fora do filho:** `CODEX_SONDA_ENVELOPE` escolhe executável rodado pelo wrapper antes do Codex; `_nomes` também chama script no host. Não são escapes demonstrados do filho, mas superfícies do chamador confiável. `bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:108-109`.
- **Selo insuficiente:** testa substrings; sua única adulteração troca `read-only` por `danger-full-access`. Não exercita symlinks, caminhos reais, worktrees, chamada construtor, recusa posterior ou destinos do host. `bin/tests/test_codex_pergunta.sh:11-33`.
- **Contraexemplos propostos ainda não executados:** worktree válida com `app` symlink; caminho com `..`; worktree de outro repositório/obra; `HEAD` mudado durante execução; arquivo proibido alterado e restaurado; ignorados; metadados Git compartilhados; execução de deploy/hook existente; configuração `.claude`; mutação fora de `app` mas dentro da worktree. [nao verificado] [nao verificado]

A lista contratual precisa casar caminhos relativos à raiz da worktree, incluindo prefixo `app/` e `migrations` em qualquer aplicação; a implementação desse matcher não existe neste wrapper. `bin/codex_pergunta.sh:174-204`, `app/escala/migrations/0001_initial.py:1`, `app/pautas/migrations/0001_initial.py:1`.

## 06/11 10:53 · sessao acad92a1a511 · - · - · origem=Ronald (tmux)

O selo atual certifica apenas o formato de leitura: exige `--sandbox read-only`, rejeita `workspace-write` e exige `~/codex_ws/<hash>`; portanto não certifica o contrato construtor proposto. Sua única adulteração troca o sandbox por `danger-full-access`. (`bin/tests/test_codex_pergunta.sh:13-18`, `bin/tests/test_codex_pergunta.sh:30-33`)

Leitores e escritores:

- `chamadas.log`: wrapper escreve `ini` em `bin/codex_pergunta.sh:88-89`; trap de saída escreve `fim` com rc em `bin/codex_pergunta.sh:56-65`. Leitor central é `bin/codex_limite_estado.py:49-68`; ele associa thread a obra/papel em `bin/codex_limite_estado.py:92-96`. `construtor` ainda cairia em `outro`, pois o vocabulário contém quatro papéis e a contagem encaminha os demais para `outro`. (`bin/codex_limite_estado.py:23`, `bin/codex_limite_estado.py:74-78`)
- `.rc`: o wrapper atual termina com `exit $RC`, sem criar `logs/codex/<obra>.rc`; suas saídas são resposta, sidecar `.id`, JSONL e logs. (`bin/codex_pergunta.sh:168-204`) Um eventual escritor externo desse `.rc` não está estabelecido pelo dado. [nao verificado]
- Trava atual do wrapper é `<resposta>.lock`, aberta por redirecionamento shell e tomada com `flock` exclusivo no fd 7. (`bin/codex_pergunta.sh:162-163`) A triagem abre esse mesmo caminho com `O_NOFOLLOW` e tenta `LOCK_SH|LOCK_NB`; se ocupado, recusa ler meia resposta. (`bin/disjuntor.py:779-789`)
- Gate/slots são outros locks, `slot.gate` fd 9 e `slot.N` fd 8, responsáveis pela concorrência. (`bin/codex_pergunta.sh:138-153`)

**Incompatibilidade adicional importante do contrato com o hook de hoje:** `ESPERANDO: codex_pergunta.sh | pid=N | arquivo=...rc` exige **as duas provas**, pois o hook verifica PID e arquivo e concatena todas as falhas. (`bin/hook_stop_fila1.py:675-680`) Arquivo ausente ou que só nasce ao terminar não prova progresso durante a espera; primeira observação ou mtime parado bloqueia. (`bin/hook_stop_fila1.py:640-641`; comentário explicativo em `bin/hook_stop_fila1.py:455-459`) O selo já exige bloqueio para PID vivo+arquivo parado e PID morto+arquivo andando. (`bin/tests/test_hook_esperando_com_prova.sh:141-142`, `bin/tests/test_hook_esperando_com_prova.sh:160-167`) Esse leitor usa `stat`/mtime, sem interpretar conteúdo rc nem adquirir um lock `.rc`; a trava que ele adquire é da memória `hook_stop_espera.json.lock`. (`bin/hook_stop_fila1.py:600-641`)

Contra-exemplos a acrescentar ao selo, como requisitos de validação estática futura — resultados de execução não foram medidos. [nao verificado]

1. Chamada literal `--construtor wt hash q r` e `CODEX_PAPEL=construtor`: demonstrar ramo construtor separado e que a chamada de leitura continua intacta; hoje a aridade aceita três argumentos e o papel exclui construtor. (`bin/codex_pergunta.sh:39-48`)
2. WT com nome permitido que seja symlink para viva/outra WT, caminho relativo com `..`, `wt/app` symlink, `gitdir` apontando para outro worktree, WT registrada mas nascida de commit antigo: exigir recusa antes de chamar Codex; tais validações não integram o fluxo atual de criação/reuso por `-d`+archive. (`bin/codex_pergunta.sh:87-101`)
3. `RESPOSTA.lock` symlink para sentinela e diretório pai symlink: sentinela deve permanecer intacta; o wrapper abre com `>` antes de obter flock, enquanto o leitor Python explicitamente usa `O_NOFOLLOW`. (`bin/codex_pergunta.sh:162-163`; `bin/disjuntor.py:779`)
4. Mudança de proibido com prefixo `app/`, migration em `app/<app>/migrations/`, renomeação, exclusão, arquivo ignorado, index flags ou alteração seguida de commit na WT: testar detector completo e recusa; detector de diff do contrato ainda não existe no wrapper. (`bin/codex_pergunta.sh:174-204`)
5. Tentativa via `app/link` de editar fora de `app/`, arquivo na raiz da WT, `.claude/`, socket Docker, git hook/filter/helper configurado e processo que continue após timeout: exigir bloqueio ou recusa com efeito externo sentinela intacto; sandbox efetivo desses cenários é [nao verificado]. O RED real existente só pede três criações diretas e compara hashes/status, sem esses cenários. (`bin/tests/prova_codex_pergunta_real.sh:15-27`)
6. Saídas 0/3/75/124: `.rc` deve trazer rc e hora da chamada certa, sob trava, sem aceitar sentinela de chamada anterior; atualmente só há rc de processo e linha `fim`, e timeout/limite seguem caminhos diferentes. (`bin/codex_pergunta.sh:56-65`, `bin/codex_pergunta.sh:174-191`, `bin/codex_pergunta.sh:203-204`)
7. Testar o formato literal de `ESPERANDO` do contrato contra o hook real: PID vivo + `.rc` ausente/parado; PID morto + `.rc` novo; chamadas paralelas da mesma obra. Os dois primeiros já têm expectativas de bloqueio nos testes do hook. (`bin/tests/test_hook_esperando_com_prova.sh:141-142`, `bin/tests/test_hook_esperando_com_prova.sh:160-167`)

Observação relacionada ao desligamento: `bin/pausar.sh codex-construtor` isolado não basta; o script exige também dono, motivo e condição de saída, e escreve um arquivo no caminho dado. Não há neste script um cadastro automático de papéis suspensos. (`bin/pausar.sh:31-47`)

## 06/11 10:51 · sessao 607803feb237 · R1-CAUSAD-V3 · leitura · origem=Code

A porta anterior foi `diff_calculador --pares-da-autoridade`; o comando também oferece captura dos períodos do fechamento. (`app/docs/RELATO.md:663`; `app/ponto/management/commands/diff_calculador.py:49`)

Neste hash, sua tabela ainda omite HE100 de feriado e os splits noturnos no lado v2, além de somar toda folga sem a separação do fechamento. Por isso a sonda abaixo chama o comando real, captura sua tabela para confronto e mede o impacto pelas duas emissões reais do fechamento, interceptando somente a persistência. (`app/ponto/management/commands/diff_calculador.py:253`, `:263`; `app/ponto/services/fechamento.py:1185`, `:1261`; `app/ponto/services/dia_pago.py:357`)

Não executei a sonda; resultados, universo atual e duração estão **[nao verificado]**. Estimativa operacional: 20–40 minutos para aproximadamente 600 colaboradores por competência; há interrupção cooperativa aos 38 minutos, com saída incompleta explícita. **[nao verificado]** [nao verificado]

```python
# R1-CAUSAD-V3 — IMPACTO, nunca validacao.
# Referencias: app/docs/LEIS.md:136;
# app/ponto/management/commands/diff_calculador.py:49,61,137,140,538;
# app/ponto/services/fechamento.py:1119,1185,1261;
# app/ponto/services/dia_pago.py:135,310,357;
# app/ponto/management/commands/e6_oraculo.py:128,172.
#
# Para exec() no shell Django JA configurado.
# Sem arquivo de saida, sem CSV, sem chamada de rede.
# Script nao executado nesta auditoria: [nao verificado].

import sys
sys.dont_write_bytecode = True

import collections
import datetime as dt
import io
import logging
import math
import time
from contextlib import redirect_stdout, redirect_stderr
from decimal import Decimal
from unittest.mock import Mock, patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from ponto.calculador import regras
from ponto.management.commands import diff_calculador as DC
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.models import DiaPago
from ponto.services import dia_pago as DP
from ponto.services import fechamento as F


COMPETENCIAS = ((2026, 10), (2026, 11))
RUBRICAS = tuple(r for r in regras.RUBRICAS if r not in regras.NAO_DECIDE)
DONOS = {"BATIDA", "CADASTRO", "ESTRUTURA"}
LIMIAR_H = 0.02  # mesmo limiar do comando: diff_calculador.py:538
INICIO = time.monotonic()
PRAZO = INICIO + 38 * 60
HOJE, AGORA = timezone.localdate(), timezone.now()

ERROS = []
VISTOS = {}
SOMAS = []
COMP_ATUAL = None
REAL_RECALC = F.recalcular_fechamento_mes
REAL_LAVRAR = DP.lavrar
COD_COMANDO = DC.Command.handle.__code__
RAIZ_APP = DC.__file__.split("/ponto/management/commands/")[0] + "/"


class LacunaDeMedicao(Exception):
    pass


class ConservacaoNaoFecha(Exception):
    pass


class PrazoEsgotado(BaseException):
    pass


def conferir_prazo():
    if time.monotonic() >= PRAZO:
        raise PrazoEsgotado()


def contexto(frame):
    cid = dia = None
    while frame is not None:
        loc = frame.f_locals
        if cid is None:
            for nome in ("colab", "colaborador", "c", "_c"):
                obj = loc.get(nome)
                if obj is not None:
                    pk = getattr(obj, "pk", None)
                    if isinstance(pk, int):
                        cid = pk
                        break
        if dia is None:
            for nome in ("data", "dia", "_dia", "k"):
                valor = loc.get(nome)
                if isinstance(valor, dt.datetime):
                    dia = valor.date()
                    break
                if isinstance(valor, dt.date):
                    dia = valor
                    break
        frame = frame.f_back
    return cid, dia


def falha(exc, cid=None, dia=None, tb=None):
    # Retem a identidade para nao contar novamente a mesma propagacao.
    if id(exc) in VISTOS:
        return
    VISTOS[id(exc)] = exc
    tb = tb if tb is not None else exc.__traceback__
    sitio = "sonda"
    if tb is not None:
        while tb.tb_next is not None:
            tb = tb.tb_next
        sitio = "%s:%d" % (
            tb.tb_frame.f_code.co_filename.rsplit("/", 1)[-1], tb.tb_lineno)
    tipo = type(exc).__name__
    # A mensagem original pode conter nome/CPF/descricao.
    # Preserva o tipo da ultima linha; suprime seu texto e limita a 200 chars.
    ultima = ("%s: [texto suprimido pelo contrato]" % tipo)[:200]
    ERROS.append((COMP_ATUAL, cid, dia, tipo, sitio, ultima))


def numero(valor):
    valor = float(valor)
    if not math.isfinite(valor):
        raise LacunaDeMedicao()
    return valor


def soma(xs):
    return math.fsum(xs)


def igual(a, b):
    return math.isclose(a, b, rel_tol=1e-12, abs_tol=1e-8)


def centavos(valor):
    return Decimal(str(round(float(valor), 2)))


def executar(ano, mes):
    c = {
        "ids": set(), "janelas": {}, "colabs": {}, "fechos": {},
        "emitido": {"motor": {}, "oraculo": {}},
        "porta_v3": {}, "cegos": {},
        "historico": None, "comando_completo": False, "ok": True,
        "donos": {},
    }

    def observar(frame, evento, arg):
        arquivo = frame.f_code.co_filename
        if not arquivo.startswith(RAIZ_APP):
            return None
        frame.f_trace_lines = frame.f_code is COD_COMANDO
        if evento == "exception":
            tipo, exc, tb = arg
            # Encerramento normal de iteradores nao e erro.
            if not issubclass(tipo, (StopIteration, StopAsyncIteration, GeneratorExit)):
                cid, dia = contexto(frame)
                falha(exc, cid, dia, tb)
        if frame.f_code is COD_COMANDO:
            if evento == "line":
                conferir_prazo()
                # Primeira linha depois do laco de medicao neste hash.
                if frame.f_lineno == 550:
                    c["comando_completo"] = True
            elif evento == "return" and "S" in frame.f_locals:
                loc = frame.f_locals
                c["historico"] = {
                    "S": {r: tuple(v) for r, v in loc["S"].items()},
                    "T": dict(loc["T"]),
                    "div": dict(loc["div"]),
                }
        return observar

    def fechamento_capturado(m, a, **kw):
        conferir_prazo()
        chamador = sys._getframe(1)
        if chamador.f_code is not COD_COMANDO:
            raise LacunaDeMedicao()

        # O universo e a janela sao LIDOS do comando real.
        loc = chamador.f_locals
        ids = set(loc["ids"])
        ini, fim = loc["ini"], loc["fim"]
        c["ids"].update(ids)
        for cid in ids:
            c["janelas"][cid] = (ini, fim)

        def emissor_capturado(colab, mm, aa, **dados):
            conferir_prazo()
            versao = dados.get("versao", "motor")
            if versao not in ("motor", "oraculo"):
                raise LacunaDeMedicao()
            capturadas = []

            def capturar_bulk(objetos, *args, **kwargs):
                objetos = list(objetos)
                capturadas.extend(objetos)
                return objetos

            # Estes sao os DOIS sitios de escrita de lavrar:
            # dia_pago.py:357-358. O emissor original executa inteiro.
            apagar = Mock()
            apagar.delete.return_value = (0, {})
            manager = DiaPago.objects
            with transaction.atomic():
                try:
                    with patch.object(manager, "filter", return_value=apagar), \
                         patch.object(manager, "bulk_create", new=capturar_bulk):
                        n = REAL_LAVRAR(colab, mm, aa, **dados)
                finally:
                    transaction.set_rollback(True)

            if n != len(capturadas):
                raise ConservacaoNaoFecha()
            if colab.pk not in ids:
                return n

            cid = colab.pk
            c["colabs"][cid] = colab
            for obj in capturadas:
                if obj.tipo != DiaPago.TIPO_DIA:
                    continue
                chave = (cid, obj.data)
                if chave in c["emitido"][versao]:
                    raise LacunaDeMedicao()
                c["emitido"][versao][chave] = {
                    r: numero(getattr(obj, r)) for r in RUBRICAS
                }

            if versao == "oraculo":
                rubricas = dados.get("rubricas")
                if rubricas is None:
                    raise LacunaDeMedicao()
                for dia, rub in rubricas.items():
                    chave = (cid, dia)
                    c["porta_v3"][chave] = {
                        r: numero(v) for r, v in rub.valores.items()
                        if r in RUBRICAS
                    }
                    c["cegos"][chave] = set(rub.nao_decide or ())
            return n

        # O comando instala um emissor nulo em :137.
        # Dentro da chamada REAL recolocamos o emissor original instrumentado.
        # retornar_objeto so amplia o retorno; nao altera a conta.
        kw["retornar_objeto"] = True
        with patch.object(DP, "lavrar", new=emissor_capturado):
            retorno = REAL_RECALC(m, a, **kw)
        for cid, objeto in retorno.items():
            if cid in ids:
                c["fechos"][cid] = {
                    r: numero(getattr(objeto, r)) for r in RUBRICAS
                }
        return retorno

    trace_anterior = sys.gettrace()
    with transaction.atomic():
        try:
            with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()), \
                 patch.object(logging.Logger, "handle", return_value=None):
                sys.settrace(observar)
                try:
                    conferir_prazo()
                    with patch.object(F, "recalcular_fechamento_mes",
                                      new=fechamento_capturado):
                        DC.Command(stdout=io.StringIO(), stderr=io.StringIO(),
                                   no_color=True).handle(
                            mes=mes, ano=ano, csv=None, limite_colabs=0,
                            colab="", abrir_he50=False, contra_gravado=False,
                            periodos_do_fechamento=True, pares_da_autoridade=True,
                        )

                    if not c["comando_completo"]:
                        raise LacunaDeMedicao()

                    # Detecta tambem falhas que o comando sinaliza sem levantar.
                    for nome in (
                        "erro_dia_da_jornada",
                        "sem_resultado_do_fechamento",
                        "troca_de_periodos_FALHOU",
                        "dia_sem_rubrica_da_porta",
                    ):
                        if c["historico"]["T"].get(nome, 0):
                            falha(LacunaDeMedicao())

                    for cid in sorted(c["ids"]):
                        for versao in ("motor", "oraculo"):
                            if not any(k[0] == cid for k in c["emitido"][versao]):
                                falha(LacunaDeMedicao(), cid)
                        if cid not in c["fechos"]:
                            falha(LacunaDeMedicao(), cid)

                    # Uma montagem por colab; um julgamento por dia divergente.
                    insumos = {}
                    chaves = sorted(
                        set(c["emitido"]["motor"]) |
                        set(c["emitido"]["oraculo"])
                    )
                    for cid, dia in chaves:
                        chave = (cid, dia)
                        v2 = c["emitido"]["motor"].get(chave)
                        v3 = c["emitido"]["oraculo"].get(chave)
                        if v2 is None or v3 is None:
                            falha(LacunaDeMedicao(), cid, dia)
                            continue
                        valores = c["porta_v3"].get(chave, {})
                        cegos = c["cegos"].get(chave, set())
                        diverge = any(
                            r in valores and r not in cegos
                            and abs(v3[r] - v2[r]) > LIMIAR_H
                            for r in RUBRICAS
                        )
                        if not diverge:
                            continue
                        conferir_prazo()
                        try:
                            ini, fim = c["janelas"][cid]
                            if not ini <= dia <= fim:
                                raise LacunaDeMedicao()
                            if cid not in insumos:
                                insumos[cid] = InsumosDoDono(
                                    c["colabs"][cid], ini, fim, HOJE, agora=AGORA)
                            with transaction.atomic():
                                try:
                                    dono = insumos[cid].dono(dia)
                                finally:
                                    transaction.set_rollback(True)
                            if dono not in DONOS:
                                raise LacunaDeMedicao()
                            c["donos"][chave] = dono
                        except Exception as exc:
                            falha(exc, cid, dia)
                            c["donos"][chave] = "SEM_DONO"
                finally:
                    sys.settrace(trace_anterior)
        except (Exception, PrazoEsgotado) as exc:
            c["ok"] = False
            falha(exc)
        finally:
            transaction.set_rollback(True)
    return c


def publicar(ano, mes, c):
    comp = "%04d-%02d" % (ano, mes)
    e2, e3 = c["emitido"]["motor"], c["emitido"]["oraculo"]
    chaves = sorted(set(e2) | set(e3))
    print("UNIVERSO|%s|colabs_porta=%d|segundos=%.1f" %
          (comp, len(c["ids"]), time.monotonic() - INICIO))
    print("RUBRICA|competencia|rubrica|v2_h|v3_h|delta_h|colabs_div|dias_colab_div")
    print("DONO|competencia|rubrica|dono|v2_h|v3_h|delta_h|colabs|dias_colab")

    for r in RUBRICAS:
        grupos = collections.defaultdict(list)
        residuos = collections.defaultdict(list)
        comparados = []
        divergentes = []
        ref_v3 = []
        emitido_v3_decidido = []

        for chave in chaves:
            cid, dia = chave
            a = e2.get(chave)
            b = e3.get(chave)
            if a is None or b is None:
                # Zero aqui e somente elemento neutro da soma do lado AUSENTE;
                # o caso NAO entra na comparacao e foi registrado como erro.
                residuos["SEM_PAR"].append(
                    (chave, a[r] if a else 0.0, b[r] if b else 0.0))
                continue

            valores = c["porta_v3"].get(chave, {})
            cegos = c["cegos"].get(chave, set())
            if r in cegos or r not in valores:
                # Valor herdado pelo emissor nao e resposta independente da v3.
                residuos["SEM_DECISAO"].append((chave, a[r], b[r]))
                continue

            ref_v3.append(valores[r])
            emitido_v3_decidido.append(b[r])
            comparados.append((chave, a[r], b[r]))
            if abs(b[r] - a[r]) <= LIMIAR_H:
                residuos["ATE_LIMIAR"].append((chave, a[r], b[r]))
                continue

            dono = c["donos"].get(chave, "SEM_DONO")
            grupos[dono].append((chave, a[r], b[r]))
            divergentes.append((chave, a[r], b[r]))
            print("DIA|%s|%s|col%d|%s|%s|%.9f|%.9f|%+.9f" %
                  (comp, r, cid, dia, dono, a[r], b[r], b[r] - a[r]))

        h2 = soma(x[1] for x in comparados)
        h3 = soma(x[2] for x in comparados)
        cols_div = {x[0][0] for x in divergentes}
        print("RUBRICA|%s|%s|%.9f|%.9f|%+.9f|%d|%d" %
              (comp, r, h2, h3, h3 - h2, len(cols_div), len(divergentes)))

        for dono, xs in sorted(grupos.items()):
            a = soma(x[1] for x in xs)
            b = soma(x[2] for x in xs)
            print("DONO|%s|%s|%s|%.9f|%.9f|%+.9f|%d|%d" %
                  (comp, r, dono, a, b, b - a,
                   len({x[0][0] for x in xs}), len(xs)))

        for classe, xs in sorted(residuos.items()):
            print("RESIDUO|%s|%s|%s|%.9f|%.9f|%+.9f|%d|%d" %
                  (comp, r, classe, soma(x[1] for x in xs),
                   soma(x[2] for x in xs),
                   soma(x[2] - x[1] for x in xs),
                   len({x[0][0] for x in xs}), len(xs)))

        # Totais independentes das particoes: instancias do emissor REAL.
        autoridade2 = soma(v[r] for v in e2.values())
        autoridade3 = soma(v[r] for v in e3.values())
        partes = [x for xs in grupos.values() for x in xs]
        partes += [x for xs in residuos.values() for x in xs]
        fecha2 = igual(soma(x[1] for x in partes), autoridade2)
        fecha3 = igual(soma(x[2] for x in partes), autoridade3)

        # Divergencia = total da autoridade MENOS parcelas explicitamente
        # excluidas e diferencas ate o limiar; nao perde o sub-limiar.
        residual_delta = soma(
            x[2] - x[1] for xs in residuos.values() for x in xs)
        total_div = autoridade3 - autoridade2 - residual_delta
        partes_div = soma(
            x[2] - x[1] for xs in grupos.values() for x in xs)
        cols_partes = set().union(
            *({x[0][0] for x in xs} for xs in grupos.values()))
        dias_partes = sum(len(xs) for xs in grupos.values())
        dono_completo = "SEM_DONO" not in grupos
        cobertura = c["ok"] and c["comando_completo"]

        SOMAS.append(
            "SOMA PARTES_EMISSOR|%s|%s|v2=%s|v3=%s" %
            (comp, r, fecha2 and cobertura, fecha3 and cobertura))
        SOMAS.append(
            "SOMA DIVERGENCIA|%s|%s|partes_h=%.9f|autoridade_menos_residuo_h=%.9f|%s" %
            (comp, r, partes_div, total_div,
             igual(partes_div, total_div) and dono_completo and cobertura))
        SOMAS.append(
            "SOMA COLABS_UNIAO_DIAS|%s|%s|colabs=%s|dias=%s" %
            (comp, r, cols_partes == cols_div and dono_completo and cobertura,
             dias_partes == len(divergentes) and dono_completo and cobertura))

        # A resposta da porta v3 e o dict Rubricas.valores devolvido pelo sistema,
        # recebido pelo emissor; nao e reconstruida pela sonda.
        porta_ok = igual(soma(emitido_v3_decidido), soma(ref_v3))
        SOMAS.append("SOMA V3_PORTA_EMISSOR|%s|%s|%s" %
                     (comp, r, porta_ok and cobertura))
        if not porta_ok:
            falha(ConservacaoNaoFecha())

        # FM arredonda por colaborador; nao comparar arredondamento global
        # com soma de totais individuais ja arredondados.
        por_col = collections.defaultdict(list)
        for (cid, dia), valores in e2.items():
            por_col[cid].append(valores[r])
        total_dias = sum(
            (centavos(soma(por_col[cid])) for cid in c["ids"]), Decimal("0"))
        total_fm = sum(
            (centavos(c["fechos"][cid][r])
             for cid in c["ids"] if cid in c["fechos"]), Decimal("0"))
        fm_ok = (
            c["ids"] <= set(c["fechos"])
            and c["ids"] <= set(por_col)
            and total_dias == total_fm
        )
        SOMAS.append("SOMA MOTOR_FECHAMENTO|%s|%s|dias=%s|retorno=%s|%s" %
                     (comp, r, total_dias, total_fm, fm_ok and cobertura))
        if not fm_ok:
            falha(ConservacaoNaoFecha())

        # Confronto com a tabela HISTORICA: divergencia aqui pode ser do
        # instrumento, inclusive as omissoes documentadas. Nao e bug v2 provado.
        if c["historico"] is not None and r in c["historico"]["S"]:
            hist2, hist3 = c["historico"]["S"][r]
            print("INSTRUMENTO_HISTORICO|%s|%s|v2_h=%.9f|v3_h=%.9f|delta_h=%+.9f" %
                  (comp, r, hist2, hist3, hist3 - hist2))


try:
    if settings.SETTINGS_MODULE != "config.settings.sombra":
        raise LacunaDeMedicao()
    if connection.schema_name != "juliani":
        raise LacunaDeMedicao()

    print("ATO|hoje=%s|agora=%s" % (HOJE, timezone.localtime(AGORA).isoformat()))
    for r in sorted(regras.NAO_DECIDE):
      

[truncado em 20000 de 27106 caracteres]

## 06/11 10:48 · sessao 8fe97f68d838 · R1-CAUSAD-V3 · leitura · origem=Code

```python
# R1-CAUSAD-V3 — proposta para exec() no shell Django já configurado.
# Não executado; resultados e duração: [nao verificado].
# Cópia indicada: b6d695411d80cebb1deb0613ba93ef603922e7d4.
#
# Mede a resposta ATUAL de perguntar_pela_celula sobre a célula lavrada.
# Não reconstitui o ingresso histórico pelo cartório.
# Autoridade: app/chamados/services/fabrica_da_celula.py:145.
#
# Expectativas do raspado, não resultados impostos: 200; G1=182/G2=13/G3=5.
# [nao verificado]
#
# Uma escrita interceptada termina aquele caminho ANTES de gravar.
# "ESCRITA_INTERCEPTADA" não significa pergunta criada nem guarda recusante.
# O último frame estrutural do traceback é impresso, limitado a 200 caracteres;
# mensagens livres de exceção não são impressas, para preservar o contrato.

import collections
import contextlib
import datetime as d
import io
import logging
import os
import sys
import time
import traceback
from unittest.mock import patch

INICIO = time.monotonic()
BYTECODE_ANTES = sys.dont_write_bytecode
sys.dont_write_bytecode = True

PRIMEIROS = [
    (144, d.date(2026, 9, 21)),
    (137, d.date(2026, 11, 3)),
    (222, d.date(2026, 10, 28)),
]
ESPERADOS = {"G1": 182, "G2": 13, "G3": 5}
FONTE_UNIVERSO = (
    "app/chamados/services/vigia_fio.py:542;"
    "app/ponto/management/commands/e6_oraculo.py:172"
)

erros = collections.Counter()
lista_erros = []
excecoes_retidas = []
vistos = set()
contexto = [None, None]
estado = None
r = None
sem_d = []
linhas = []
fora_dono = collections.Counter()
dono_sem_resposta = 0
tempos = collections.Counter()


class FonteDivergente(RuntimeError):
    pass


class CaminhoNaoReconhecido(RuntimeError):
    pass


class RegistroErroSemExcecao(RuntimeError):
    pass


class EscritaInesperada(RuntimeError):
    pass


class EscritaInterceptada(BaseException):
    def __init__(self, sitio, caminho):
        self.sitio = sitio
        self.caminho = caminho


class Descarte:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass


def fonte(arquivo, numero, funcao=""):
    arquivo = str(arquivo).replace("\\", "/")
    if "/app/" in arquivo:
        arquivo = "app/" + arquivo.split("/app/", 1)[1]
    else:
        arquivo = arquivo.rsplit("/", 1)[-1]
    return ("%s:%s%s" % (
        arquivo, numero, (":" + funcao) if funcao else ""
    ))[:200]


def falha(exc, onde, cid=None, dia=None, local=None):
    if cid is None:
        cid = contexto[0]
    if dia is None:
        dia = contexto[1]
    chave = (id(exc), cid, dia)
    if chave in vistos:
        return
    vistos.add(chave)
    excecoes_retidas.append(exc)
    erros[type(exc).__name__] += 1
    tb = traceback.extract_tb(exc.__traceback__)
    ultimo = (
        fonte(tb[-1].filename, tb[-1].lineno, tb[-1].name)
        if tb else (local or "[nao verificado]")
    )
    lista_erros.append((cid, dia, type(exc).__name__, ultimo[:200], onde))


def observar_log(logger, registro):
    # Não encaminha mensagens livres nem escreve em handlers de arquivo.
    if registro.exc_info and registro.exc_info[1] is not None:
        falha(registro.exc_info[1], "log")
    elif registro.levelno >= logging.ERROR:
        falha(
            RegistroErroSemExcecao(), "log_sem_exc_info",
            local=fonte(registro.pathname, registro.lineno, registro.funcName)
        )


abrir_real = open
io_abrir_real = io.open
os_abrir_real = os.open


def abertura_so_leitura(real):
    def abrir(arquivo, mode="r", *args, **kwargs):
        if any(x in mode for x in ("w", "a", "x", "+")):
            exc = EscritaInesperada()
            falha(exc, "arquivo")
            raise exc
        return real(arquivo, mode, *args, **kwargs)
    return abrir


def os_abrir_so_leitura(arquivo, flags, *args, **kwargs):
    proibidos = os.O_WRONLY | os.O_RDWR | os.O_CREAT | os.O_TRUNC | os.O_APPEND
    if flags & proibidos:
        exc = EscritaInesperada()
        falha(exc, "arquivo")
        raise exc
    return os_abrir_real(arquivo, flags, *args, **kwargs)


def negar_mutacao_arquivo(*args, **kwargs):
    exc = EscritaInesperada()
    falha(exc, "arquivo")
    raise exc


def _dt(x):
    # Mesma conversão do anexo.
    return x if isinstance(x, d.date) else d.date.fromisoformat(str(x)[:10])


def resumo(valor):
    # Nunca repr() de modelo, descrição, observação ou mensagem livre.
    if valor is None or isinstance(valor, (bool, int)):
        return str(valor)
    if isinstance(valor, (list, tuple, set, frozenset, dict)):
        return "%s_n=%d" % (type(valor).__name__, len(valor))
    return type(valor).__name__


def caminho_python():
    f = sys._getframe(1)
    partes = []
    while f is not None:
        arquivo = f.f_code.co_filename.replace("\\", "/")
        if "/app/" in arquivo:
            partes.append(fonte(arquivo, f.f_lineno, f.f_code.co_name))
        f = f.f_back
    return list(reversed(partes))


def interceptar(sitio):
    def parar(*args, **kwargs):
        raise EscritaInterceptada(sitio, caminho_python())
    return parar


def save_nao_mapeado(*args, **kwargs):
    # Rede de detecção; não substitui os mocks dos sítios específicos abaixo.
    exc = EscritaInesperada()
    falha(exc, "save_nao_mapeado")
    raise exc


try:
    with contextlib.ExitStack() as pilha:
        pilha.enter_context(contextlib.redirect_stdout(Descarte()))
        pilha.enter_context(contextlib.redirect_stderr(Descarte()))
        pilha.enter_context(patch.object(logging.Logger, "handle", new=observar_log))
        pilha.enter_context(patch("builtins.open", new=abertura_so_leitura(abrir_real)))
        pilha.enter_context(patch.object(io, "open", new=abertura_so_leitura(io_abrir_real)))
        pilha.enter_context(patch.object(os, "open", new=os_abrir_so_leitura))
        for nome in (
            "remove", "unlink", "rename", "replace",
            "mkdir", "makedirs", "rmdir", "removedirs",
        ):
            pilha.enter_context(patch.object(os, nome, new=negar_mutacao_arquivo))

        from django.contrib.auth import get_user_model
        from django.db import transaction
        from django.db.models import Model
        from django.db.models.query import QuerySet
        from django.utils import timezone

        from chamados import juizes as J
        from chamados.models import (
            ChamadoColaborador, DisputaSupervisao, PerguntaDisputa,
        )
        from chamados.services import fabrica_da_celula as F
        from chamados.services import disputa_emissao as DE
        from chamados.services import declaracao_texto as DT
        from chamados.services import vigia_fio as V
        from colaboradores.models import Colaborador
        from core import observ as O
        from escala.models import CelulaDia
        from escala import servico_jornada as SJ
        from pautas.models import Pauta
        from ponto.management.commands.e6_oraculo import (
            InsumosDoDono, batida_fora_de_turno,
        )
        from ponto.services import cartorio as C
        from ponto.supra_juiz import cadastro_do_dia_carregado
        from ponto.turnos import realizado_do_dia

        # Sítios específicos de persistência alcançáveis pela fábrica:
        # disputa_emissao.py:501/:523; chamados/models.py:932/:2474/:2476/:2652;
        # pautas/services.py:180; core/utils.py:18.
        pilha.enter_context(patch.object(Model, "save", new=save_nao_mapeado))
        for modelo, sitio in (
            (ChamadoColaborador, "ChamadoColaborador.save"),
            (DisputaSupervisao, "DisputaSupervisao.save"),
            (PerguntaDisputa, "PerguntaDisputa.save"),
            (Pauta, "Pauta.save"),
            (get_user_model(), "User.save"),
        ):
            pilha.enter_context(patch.object(modelo, "save", new=interceptar(sitio)))
        for modelo, sitio in (
            (DisputaSupervisao, "DisputaSupervisao.objects.create"),
            (PerguntaDisputa, "PerguntaDisputa.objects.create"),
            (Pauta, "Pauta.objects.create"),
        ):
            pilha.enter_context(
                patch.object(modelo.objects, "create", new=interceptar(sitio))
            )

        # O papel é LEITOR. Conserva a construção REAL da consulta e seus filtros,
        # retirando somente o bloqueio SQL. Não ensaia concorrência.
        # Sítios: disputa_emissao.py:467; relavra_via.py:221.
        sfu_real = QuerySet.select_for_update

        def sem_lock(self, *args, **kwargs):
            qs = sfu_real(self, *args, **kwargs)
            qs.query.select_for_update = False
            return qs

        pilha.enter_context(
            patch.object(QuerySet, "select_for_update", new=sem_lock)
        )

        engolido_real = O.registrar_engolido

        def engolido(logger_nome, operacao, exc, **kwargs):
            falha(exc, "registrar_engolido")
            return engolido_real(logger_nome, operacao, exc, **kwargs)

        pilha.enter_context(
            patch.object(O, "registrar_engolido", side_effect=engolido)
        )

        # Mocks observadores: todos devolvem o resultado da função REAL.
        guardas = [
            (J, "dia_sem_trabalho"),
            (F, "vencidos_no_instante"),
            (C, "marcos_vencidos"),
            (C, "parcial_so_lista"),
            (C, "houve_fato_novo"),
            (DE, "_ata_do_dia"),
            (DT, "_marcos_faltantes"),
            (SJ, "escala_vigente"),
            (SJ, "classificar_falta"),
            (F, "motivos_pela_ata"),
            (F, "motivos_a_perguntar"),
            (F, "limbo_vai_a_pauta"),
            (V, "competencia_do_dia_entregue"),
            (F, "motivos_sem_pauta_do_limbo"),
            (F, "pauta_dp_do_limbo_exportado"),
        ]
        reais = {nome: getattr(modulo, nome) for modulo, nome in guardas}
        reais["perguntar_pela_celula"] = F.perguntar_pela_celula

        # Detecta deslocamento das linhas usadas na atribuição, em vez de
        # classificar silenciosamente outra versão.
        for nome, primeira in (
            ("perguntar_pela_celula", 145),
            ("motivos_a_perguntar", 74),
            ("motivos_pela_ata", 34),
            ("parcial_so_lista", 370),
        ):
            if reais[nome].__code__.co_firstlineno != primeira:
                raise FonteDivergente()

        codigos = {fn.__code__: nome for nome, fn in reais.items()}

        def observar_funcao(nome, real):
            def chamar(*args, **kwargs):
                st = estado
                if st is not None:
                    st["eventos"].append(
                        "ENTRA " + fonte(
                            real.__code__.co_filename,
                            real.__code__.co_firstlineno, nome
                        )
                    )
                try:
                    retorno = real(*args, **kwargs)
                except Exception as exc:
                    falha(exc, nome)
                    raise
                if st is not None:
                    st["valores"][nome] = retorno
                    st["eventos"].append("DEVOLVE %s %s" % (nome, resumo(retorno)))
                return retorno
            return chamar

        for modulo, nome in guardas:
            pilha.enter_context(
                patch.object(
                    modulo, nome,
                    side_effect=observar_funcao(nome, reais[nome])
                )
            )

        def trace(frame, evento, argumento):
            st = estado
            nome = codigos.get(frame.f_code)
            if st is None or nome is None:
                return None
            local = fonte(frame.f_code.co_filename, frame.f_lineno, nome)
            if evento == "line":
                st["passagens"][(nome, frame.f_lineno)] += 1
                if not st["caminho"] or st["caminho"][-1] != local:
                    st["caminho"].append(local)
            elif evento == "return":
                st["retornos"][nome] = frame.f_lineno
                st["eventos"].append("RETURN %s %s" % (local, resumo(argumento)))
            return trace

        def leitura(fn, *args, **kwargs):
            # Cada unidade tem savepoint próprio, inclusive quando o banco recusa
            # uma operação. O atomic externo também termina com rollback.
            with transaction.atomic():
                try:
                    return fn(*args, **kwargs)
                finally:
                    transaction.set_rollback(True)

        def assinatura(ins, c, data):
            # Mesmas cinco leituras e mesma assinatura do anexo.
            p1 = cadastro_do_dia_carregado(c.pk, data)
            p1s = "cad[folga_nd=%s,sem_vinc=%s,sem_vinc_trab=%s]" % (
                p1["folga_nao_definida"],
                p1["sem_vinculo_no_dia"],
                p1["sem_vinculo_em_dia_de_trabalho"],
            )
            aberto = bool(realizado_do_dia(
                c, data, livre=False, intervalo=(), turnos=ins.turnos()
            ).aberto)
            bf = bool(batida_fora_de_turno(c, data, ins.turnos()))
            if bf:
                p2s = "fora_de_turno=True"
            elif aberto:
                p2s = "turno_aberto_sem_orfa_real"
            else:
                p2s = "turno_nao_aberto"
            nb = len(ins.horas.get(str(data), []))
            return (
                p1s,
                p2s,
                "batidas=%s(%s)" % (
                    nb if nb < 4 else "4+", "par" if nb % 2 == 0 else "IMPAR"
                ),
                "fase_lavrada=%s" % (str(data) in ins.fase()),
                "em_cxr_do_motor=%s" % (str(data) in ins.cxr()),
            )

        base = (
            "cad[folga_nd=False,sem_vinc=False,sem_vinc_trab=False]",
            "turno_nao_aberto",
        )
        grupos_do_raspado = {
            base + ("batidas=0(par)", "fase_lavrada=False",
                    "em_cxr_do_motor=False"): "G1",
            base + ("batidas=4+(par)", "fase_lavrada=False",
                    "em_cxr_do_motor=False"): "G2",
            base + ("batidas=2(par)", "fase_lavrada=False",
                    "em_cxr_do_motor=False"): "G3",
        }

        def atribuir(st, ms):
            # Atribui pela linha REAL de retorno e pelas respostas observadas.
            # Não reexecuta nenhum predicado de negócio.
            rr, vv = st["retornos"], st["valores"]
            linha = rr.get("motivos_a_perguntar")
            caminho_f = "app/chamados/services/fabrica_da_celula.py:"
            if linha == 92:
                nome = (
                    "DIA_SEM_TRABALHO"
                    if vv["dia_sem_trabalho"] else "COLAB_NAO_ATIVO"
                )
                return nome, caminho_f + "91", True
            if linha == 94:
                return "DIA_FUTURO", caminho_f + "93", True
            if linha == 100:
                return "VEREDITO_NAO_ACUSADOR", caminho_f + "99", True
            if linha == 104:
                p = rr.get("parcial_so_lista")
                return (
                    "PARCIAL_SO_LISTA_RETORNO_%s" % p,
                    caminho_f + "102;app/ponto/services/cartorio.py:%s" % p,
                    True,
                )
            if linha != 126:
                raise CaminhoNaoReconhecido()

            devidos = vv["motivos_a_perguntar"]
            if not devidos:
                if vv["motivos_pela_ata"]:
                    return "DEDUP_TOTAL", caminho_f + "118", True
                interna = rr.get("motivos_pela_ata")
                nomes = {
                    46: "NENHUM_MARCO_VENCIDO",
                    51: "ATA_NAO_AUTORIZA",
                    54: "SEM_ATA_COM_TETO_DE_MARCOS",
                    57: "CLASSIFICAR_FALTA_SEM_MOTIVO",
                    65: "ATA_SEM_MOTIVO_APROVEITADO",
                }
                if interna not in nomes:
                    raise CaminhoNaoReconhecido()
                return nomes[interna], caminho_f + str(interna), True

            if rr.get("perguntar_pela_celula") == 173:
                return "DESTINO_PAUTA_SEM_NOVA_ESCRITA", caminho_f + "171", False
            return "RETORNO_COM_MOTIVOS_SEM_SAVE", caminho_f + "177", False

        def sondar(cel, hoje, agora):
            global estado
            st = {
                "valores": {}, "retornos": {}, "eventos": [],
                "caminho": [], "passagens": collections.Counter(),
            }
            estado = st
            trace_antes = sys.gettrace()
            erros_antes = len(lista_erros)
            try:
                sys.settrace(trace)
                ms = leitura(
                    F.perguntar_pela_celula, cel, hoje=hoje, agora=agora
                )
                resultado = atribuir(st, ms)
            except EscritaInterceptada as exc:
                st["eventos"].append(
                    "ESCRITA %s %s" % (
                        exc.sitio, " > ".join(exc.caminho)
                    )
                )
                resultado = (
                    "ESCRITA_INTERCEPTADA_" + exc.sitio,
                    exc.caminho[-1] if exc.caminho else "[nao verificado]",
                    False,
                )
            except Exception as exc:
                falha(exc, "emissor")
                resultado = ("ERRO_NAO_CLASSIFICADO", "[nao verificado]", False)
            finally:
                sys.settrace(trace_antes)
                estado = None
            if len(lista_erros) != erros_antes:
                resultado = ("ERRO_NAO_CLASSIFICADO", "[nao verificado]", False)
            return resultado, st

        hoje = timezone.localdate()
        agora = timezone.now()

        with transaction.atomic():
            try:
                t0 = time.monotonic()

                # MONTAGEM EXATA DO UNIVERSO DO ANEXO.
                r = leitura(V.censo_alcance, limite=10 ** 7)
                tempos["censo"] = time.monotonic() - t0
                sem_d = [
                    x for x in r["alcance_dias"]
                    if x[4] == "sem_porta" and x[5] == "d"
                ]
                por_colab = collections.defaultdict(list)
                for x in sem_d:
                    por_colab[x[1]].append(x)

                for cid, xs in sorted(por_colab.items()):
                    datas = sorted(_dt(x[2]) for x in xs)
                    contexto[:] = [cid, datas[0]]
                    t0 = time.monotonic()
                    try:
                        c = leitura(Colaborador.objects.get, pk=cid)
                        ins = leitura(
                            InsumosDoDono, c, datas[0], datas[-1], hoje,
                            agora=agora
                        )
                    except (Exception, EscritaInterceptada) as exc:
                        for data in datas:
                            falha(exc, "insumos", cid, data)
                        dono_sem_resposta += len(datas)
                        continue

                    for data in datas:
                        contexto[:] = [cid, data]
                        try:
                            dono = leitura(ins.dono, data)
                        except (Exception, EscritaInterceptada) as exc:
                            falha(exc, "dono", cid, data)
                            dono_sem_resposta += 1
                            continue
                        if dono != "ESTRUTURA":
                            fora_dono[dono] += 1
                            continue

                        registro = {
                            "cid": cid, "dia": data, "grupo": "G_NAO_MEDIDO",
                            "causa": "ERRO_NAO_CLASSIFICADO",
                            "fonte": "[nao verificado]", "recusa": False,
                            "detalhe": None,
                        }
                        linhas.append(registro)
                        try:
                            sig = leitura(assinatura, ins, c, data)
                            registro["grupo"] = grupos_do_raspado.get(
                                sig, "G_FORA_RASPADO

[truncado em 20000 de 34106 caracteres]

## 06/11 10:48 · sessao 5a15255be959 · E-SEM-VIVA-B · verificador · origem=Code

1. **Procede — há derivação paralela da retenção, e a igualdade prometida não vale em geral.** A C5 declara acompanhar a testemunha, mas aplica seu próprio filtro de demissão, sem chamar o retentor. Este já oferece `com_dias=True` precisamente para que os leitores recebam os dias sem copiar a regra. As divergências estão discriminadas abaixo. (`app/escala/services/cadastro_realidade.py:140–144,159–164`; `app/folha/export.py:1178–1185`; `CLAUDE.md:20–22`.)

2. **Procede — comparação das janelas.** Comparando somente os dias cujo motivo na folha seria `espelho_nunca_bateu`, como faz o teste, os contratos são estes: C5 consulta da competência anterior até ontem; folha recebe `ini/fim` e aplica seus próprios cortes e isenções. (`app/escala/services/cadastro_realidade.py:154–164`; `app/folha/export.py:1192–1206`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:40–46`.)

   | Caso | C5 × retentor | Grau e evidência |
   |---|---|---|
   | Admissão | C5 pode incluir célula anterior à admissão; folha corta no início da admissão. | **Procede:** `app/escala/services/cadastro_realidade.py:159–164`; `app/folha/export.py:1198–1199`. |
   | Desligado com demissão | Ambos excluem os dias posteriores à demissão. | **Não procede divergência:** `app/escala/services/cadastro_realidade.py:162–163`; `app/folha/export.py:1200–1201`. |
   | Dia da demissão | Ambos incluem a célula acusadora. Na folha, `_dia_dem` dispensa os ramos de `sem_celula`, não o veredito acusador. | **Não procede divergência:** `app/escala/services/cadastro_realidade.py:163`; `app/folha/export.py:1225–1256`. |
   | Demissão nula | Nenhum dos dois corta por demissão. Permanecem os demais limites de cada função. | **Não procede divergência nesse corte:** `app/escala/services/cadastro_realidade.py:162–163`; `app/folha/export.py:1200–1201`. |
   | Ativo com demissão preenchida | C5 mantém dias posteriores; folha corta independentemente de `situacao`. Vale também para outras situações diferentes de desligado. | **Procede:** `app/escala/services/cadastro_realidade.py:162–163`; `app/folha/export.py:1200–1201`. O teste exige expressamente essa diferença para o ativo: `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:81–84`. |
   | Hoje/futuro | C5 exclui. Folha pode reter célula acusadora hoje ou futura dentro de `ini/fim`; o teste `d < hoje` protege outros ramos. | **Procede:** `app/escala/services/cadastro_realidade.py:155–161`; `app/folha/export.py:1230–1256`. |
   | Ontem | Entra nos dois se satisfizer os demais limites e condições. | **Não procede divergência por essa borda:** `app/escala/services/cadastro_realidade.py:155–161`; `app/folha/export.py:1206,1225–1254`. |
   | Competência anterior × corrente | C5 agrega anterior e corrente transcorrida. Folha trabalha o período recebido: anterior isolada, corrente isolada ou outra competência. Não são janelas intercambiáveis. | **Procede:** `app/escala/services/cadastro_realidade.py:159–161`; `app/ponto/janelas.py:60–72`; `app/folha/export.py:1168,1206`. |

   **Procede também diferença de seleção, além da janela:** folha dispensa isento e aplica as exceções de protestos não cobráveis/pausa; C5 lê qualquer célula `nunca_bateu` que passe pelo SQL. A lavra usual desse ramo passa `codigos=[]`; ocorrência de combinações excepcionais em produção é **[não verificado]**. (`app/folha/export.py:1191–1196,1245–1254`; `app/escala/services/cadastro_realidade.py:160–164`; `app/ponto/services/cartorio.py:1241–1243`.)

3. **Dúvida — “segundo juiz registrado”, no sentido literal; procede a duplicação de uma autoridade existente.** `JUIZES.md` é lookup e manda consultar o registro vivo. O lookup aponta `classificar_export` para entrada no TXT; o vivo já lhe atribui também bloqueio/aprovação. Não há entrada nominal de `motivos_retencao_celula` para a pergunta específica “quais dias retêm”, mas há a cadeia registrada `classificar_export → motivos_retencao_espelho → motivos_retencao_celula`. Portanto existe autoridade que devolve esses dias; a ausência de registro nominal não justifica recalculá-los na C5. (`app/docs/JUIZES.md:7–15,74–77`; `app/core/juizes.py:288–299`; `app/folha/export.py:960,1261–1267,1185`.)

4. **Procede — retenção da célula não prova bloqueio do desligado no fluxo real da folha.** Havendo demissão até o fim da competência, `classificar_export` sai por `rescisao_modulo_proprio` **antes** de consultar a retenção do espelho. O Retrato coloca essa pessoa em `modulo_proprio`, fora de `bloqueado`. O teste direto do retentor prova sua resposta isolada, não esse percurso completo. (`app/folha/export.py:54–60,858–863,960`; `app/folha/retrato_folha.py:29–33,89–95`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:40–46`.)

   O teste alterado do vigia cria previamente um Retrato bloqueado na fixture e depois muda situação/demissão, sem reconstruí-lo pela classificação real. A ocorrência de Retrato assim em produção é **[não verificado]**. (`app/folha/tests/test_vigia_retido_calado.py:83–84,104–110`.)

5. **Procede — mapa dos quatro campos: escritores e leitores identificados.**

   | Campo | Quem decide/escreve | Quem lê |
   |---|---|---|
   | `CelulaDia.veredito` | Porta `lavrar_veredito` atribui e persiste; método do modelo delega. Cartório fornece o julgamento automático, inclusive `nunca_bateu`; serviço de decisão humana também chama a porta. (`app/ponto/portas/celula.py:753,814–819`; `app/escala/models.py:1819–1828`; `app/ponto/services/cartorio.py:1241–1247`; `app/ponto/services/veredito_celula.py:330`.) | Consulta C5, retentor, universo de adesão e leitura da decisão humana. (`app/escala/services/cadastro_realidade.py:160`; `app/folha/export.py:1233–1255`; `app/chamados/services/adesao.py:25–34`; `app/ponto/services/veredito_celula.py:107–111`.) |
   | `CelulaDia.data` | Nasce na geração/regeneração pela porta. (`app/ponto/portas/celula.py:253–254,633–636`.) | C5 filtra, ordena e devolve; alimentação indexa por colaborador/dia; retentor recorta e indexa as células. (`app/escala/services/cadastro_realidade.py:161–165`; `app/escala/alimentacao.py:73–77`; `app/folha/export.py:1206,1224–1229`.) |
   | `Colaborador.situacao` | Operação `situacao` da porta. Desligamento grava desligado; reativação grava ativo; lançamento de INSS grava afastado; reversor grava ativo; admissão cria pela porta. (`app/colaboradores/services/colaborador.py:32,55–76,91–93`; `app/colaboradores/services/desligamento.py:104–108,209–214`; `app/ponto/views.py:2433–2435`; `app/ponto/management/commands/reverter_situacao_afastado.py:65–80`; `app/colaboradores/views.py:1181–1205`.) | Novo corte C5; portões de emissão; adesão; outras famílias da lista; cron dos desligamentos vencidos. (`app/escala/services/cadastro_realidade.py:162,221,420,541`; `app/ponto/services/cartorio.py:1696–1697,1762`; `app/chamados/services/adesao.py:33,66–67`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38–40`.) |
   | `Colaborador.data_demissao` | Mesma operação da porta. Agendamento preenche data sem desligar; desligamento grava data/situação; reativação limpa data. (`app/colaboradores/services/colaborador.py:32,55–76`; `app/colaboradores/services/desligamento.py:82–86,104–108,209–214`.) | Novo corte C5; retentor; saída por rescisão; cadastro do dia; efetivação dos agendados. (`app/escala/services/cadastro_realidade.py:162–163`; `app/folha/export.py:60,1200–1201,1229`; `app/ponto/supra_juiz.py:142–162,194`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38–40`.) |

   A ausência absoluta de outros acessos dinâmicos a esses campos é **[não verificado]**.

6. **Procede — mapa das duas funções; não são escritores persistentes.**

   - `colabs_nunca_bateu` constrói `{colab_id: [datas]}` por consulta. Seus leitores produtivos diretos são `_nunca_bateu` e `vigia_retido_calado.censo`. (`app/escala/services/cadastro_realidade.py:157–172`; `app/folha/vigia_retido_calado.py:40–41`.)
   - `_nunca_bateu` constrói uma assinatura C5 por pessoa, com quantidade e até três datas **no texto**. Seu leitor produtivo é `lista()`, que agrega as assinaturas ao cadastro. (`app/escala/services/cadastro_realidade.py:169–175,802–805`.)
   - As escritas diretas novas em modelos são fixtures dos testes, não um novo escritor produtivo. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:29–34,69–84`.)

7. **Procede — C5 não transporta os dias para `dias_na_lista` nem resolve, sozinha, o fio do admin.** `_nunca_bateu` devolve somente `codigo/detalhe`; `dias_na_lista()` exige a chave `data`. O vigia carrega separadamente dias e pessoas: o desligado pode entrar no conjunto de pessoas, permanecendo fora do mapa de dias. Assim, apenas C5 não satisfaz `dia_do_admin()` nem o balde `fio_do_admin`. (`app/escala/services/cadastro_realidade.py:172–174,974–980`; `app/chamados/services/vigia_fio.py:216–228,313–314`.)

   Na ausência das condições anteriores do classificador, isso pode trocar a causa de alcance de `e` para `d`: colaborador na lista, dia fora. A ocorrência concreta em produção é **[não verificado]**. (`app/chamados/services/vigia_fio.py:654–680`.)

8. **Procede — `vigia_retido_calado` muda por presença da pessoa, sem conferir dia ou competência.** Ele transforma a resposta de `colabs_nunca_bateu` em um `set` de IDs. Toda linha bloqueada com fio `ninguem` desse colaborador passa de calada a roteada; as datas devolvidas pela C5 são descartadas. Isso permite classificar como roteada uma linha de outra competência ou outro motivo, bastando existir algum dia C5 na janela. Ocorrência em produção: **[não verificado]**. (`app/folha/vigia_retido_calado.py:35–53`.)

9. **Procede — efeitos nos demais consumidores, com estas limitações.**

   | Consumidor | Efeito da entrada somente por C5 |
   |---|---|
   | `lista()` | Acrescenta assinatura/pessoa e atualiza contadores. A busca da pessoa não exclui desligado; sem vínculo vigente, agrupa em “sem vinculo vigente”. (`app/escala/services/cadastro_realidade.py:701–704,802–805,823–826,854–867,882–898`.) |
   | Tela/PDF | Ambos recebem a mesma lista. HTML mostra detalhe/link; PDF percorre as assinaturas recebidas. A ação C5 aponta ao cadastro individual. (`app/escala/views.py:484–492`; `app/templates/escala/cadastro_x_realidade.html:25–28`; `app/escala/services/cadastro_realidade.py:628–630,1013–1024`.) |
   | `fechar_chamados_vinculo_na_lista` | **Não procede fechamento por C5:** seleciona exclusivamente C8; cancela chamados somente com `--apply`. (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35–44,52–59`.) |
   | `fechar_avisos_regua_na_lista` | **Não procede fechamento por C5:** seleciona exclusivamente C7; cancela somente com `--apply`. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27–36,42–49`.) |
   | `placar_estrutural` | **Não procede atualização automática:** a referência à lista é uma string de fonte. `placar()` processa `RESULTADOS`, sem executar a consulta. (`app/core/placar_estrutural.py:140–149,634–660`.) |

   **Dúvida quanto à porta ser utilizável por todo leitor:** lista exige `ver_escala`; detalhe exige `ver_cadastro`. O detalhe aceita desligado, mas a distribuição dessas permissões aos usuários reais é **[não verificado]**. (`app/escala/views.py:485–487`; `app/colaboradores/views.py:459–467`.)

10. **Não procede — novo push, chamado ou pergunta causado pela inclusão C5. Procede — há consumidores capazes de escrever.** A consulta/formatação C5 apenas lê, e sua ação é um hyperlink. Os formulários POST dessa tela são condicionados à C7. Os portões do cartório continuam excluindo desligado da emissão, e a adesão também o veta. (`app/escala/services/cadastro_realidade.py:160–175,628–630`; `app/templates/escala/cadastro_x_realidade.html:28`; `app/ponto/services/cartorio.py:1696–1697,1762`; `app/chamados/services/adesao.py:66–75`.)

   Além dos cancelamentos condicionais do achado 9, o comando `vigiar_fio_morto` grava um carimbo JSON em rodada de frota, sem depender de `--apply`; com `--empresa`, não carimba. A nova presença C5 pode alterar os contadores gravados, sem criar um fio de negócio. O comando do retido calado imprime resultado e pode encerrar com erro de alarme. (`app/chamados/management/commands/vigiar_fio_morto.py:127–134,210–230`; `app/folha/management/commands/vigiar_retido_calado.py:28–39`.)

11. **Procede — existem outras leituras que ainda excluem desligado; não procede que sejam filtros posteriores anulando a C5.**

   | Sítio | Exclusão e finalidade |
   |---|---|
   | Adesão | Lê `nunca_bateu` apenas de ativos e veta desligado antes de abrir/reabrir chamado. É emissão, não leitura da mesa C5. (`app/chamados/services/adesao.py:25–34,66–92`.) |
   | “Quem cobrar” | Universo declarado de ativos; elimina desligado e deriva seu próprio indicador `nunca_bateu` pelas batidas. (`app/escala/services/furos_diarios.py:247–259,272–283`.) |
   | Dia da Frota | Lê células/vereditos, inclusive nunca bateu, somente de ativos. (`app/relatorios/dia_da_frota.py:13,34–35,54–60`.) |
   | C4 da própria lista | Exclui desligado para procurar afastado com furo sem cobertura; não filtra C5. (`app/escala/services/cadastro_realidade.py:541–556`.) |

   Esses universos continuam diferentes por finalidade. No percurso efetivo C5 → lista, a busca por IDs não filtra situação e o descarte final verifica existência/empresa visível. Não identifiquei outra exclusão direta de desligado nessa cadeia; exclusões dinâmicas não identificadas são **[não verificado]**. (`app/escala/services/cadastro_realidade.py:701–704,823–826`.)

12. **Não procede — “RED sem mordida” contra os cinco mutantes pedidos, pela leitura das asserções. A execução permanece [não verificado].**

   | Mutante | Asserção que o distingue |
   |---|---|
   | Restaurar `exclude(situacao='desligado')` | Exige presença, C5, porta e dias do desligado. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:49–62`.) |
   | Trocar `__gt` por `__gte` | A fixture contém a demissão e a igualdade exige esse dia. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:23–24,60–62`.) |
   | Tirar o corte | Há dias posteriores na fixture; a igualdade os exclui. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:24,60–62`.) |
   | Cortar ativo também | Ativo com demissão preenchida deve conservar todos os dias. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:81–84`.) |
   | Excluir desligado sem data | Exige todos os dias e igualdade com a testemunha. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:69–73`; `app/folha/tests/test_vigia_retido_calado.py:104–106`.) |

   **Procede, porém, cobertura insuficiente da alegação geral:** a comparação com a testemunha usa desligado sem admissão informada, dias de 05–12/10 e hoje 25/10. Com corte padrão 21, esses dias são todos da competência anterior. Ativo com demissão e hoje/ontem são testados apenas contra a lista, sem comparação com o retentor. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:21–34,65–84`; `app/colaboradores/models.py:46`; `app/ponto/janelas.py:10–33`.)

   As contagens RED/GREEN/mutantes do corpo do commit, seus logs e a execução da suíte inteira são **[não verificado]**. [nao verificado]

13. **Procede — relógio fora da borda no sítio tocado, preexistente à alteração.** `colabs_nunca_bateu` mantém `timezone.localdate()` interno quando não recebe `hoje`; o vigia de retidos chama a função sem injetá-lo. A lei registrada limita o relógio à borda. No caminho principal da lista, `hoje` é repassado, mas o fallback continua executável pelo outro consumidor. (`app/escala/services/cadastro_realidade.py:154,172,804`; `app/folha/vigia_retido_calado.py:40–41`; `app/docs/CORTES.md:370`.)

14. **Não procede — segundo escritor fora da porta ou inversão de travas introduzidos pelo diff.** O trecho produtivo alterado é uma consulta, sem nova persistência ou `select_for_update`; os estados continuam nas portas descritas no achado 5. As gravações das fixtures não constituem escritor de produção. (`app/escala/services/cadastro_realidade.py:160–165`; `app/ponto/portas/celula.py:814–819`; `app/colaboradores/services/colaborador.py:55–76`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:29–34`.)

15. **Dúvida — pouso antes do pré-requisito.** A regra exige suíte inteira verde, depois opus final, e impede certificação de pouso apenas pelo Codex. O anexo relata testes direcionados/vizinhos; não comprova a sequência completa. Suíte inteira → opus → pouso deste hash: **[não verificado]**. (`CLAUDE.md:364–373`.) [nao verificado]

16. **Procede — a “linha corrigida” do anexo está incorreta nesta cópia.** O corte efetivo por admissão/demissão está em `app/folha/export.py:1198–1201`; `:1224–1229` contém o laço e `_dia_dem`. O teste novo cita a segunda faixa como corte, mas importa e chama a função correta, portanto o erro de referência não invalida essa chamada. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:3–5,40–46`.)

## 06/11 10:47 · sessao 0bad524b2ece · R1-CAUSAD-V3 · leitura · origem=Code

O script usa o universo do comando: empresas ativas e colaboradores com `FechamentoMensal` na competência, sem `--so-txt`. Horas e colaboradores distintos vêm da tabela original; a conferência compara os dois números da SOMA impressa, sem refazer a apuração. Com corte 21, as janelas são **21/09–20/10** e **21/10–20/11**; prevalece o corte cadastrado por empresa. (`app/ponto/management/commands/e6_oraculo.py:291`, `:368`, `:422`, `:424`; `app/ponto/janelas.py:10`, `:54`.)

A fonte dos logs declara esse comando; o argv histórico completo dos arquivos não pôde ser confirmado. (`app/core/placar_estrutural.py:115`, `:138`; [não verificado].)

Não executei a sonda. Estimativa inicial: 1.200 pares colaborador/competência a 1–2 segundos cada significariam aproximadamente 20–40 minutos; o script imprime estimativas durante a execução e interrompe cooperativamente após 39 minutos. Tempo real e conclusão dentro desse prazo: [não verificado]. [nao verificado]

```python
def sonda_r1():
    import sys
    sys.dont_write_bytecode = True

    import ast
    import datetime as dt
    import io
    import logging
    import re
    import time
    import tokenize
    from collections import Counter
    from contextlib import redirect_stdout, redirect_stderr
    from pathlib import Path
    from unittest.mock import patch

    saida = sys.stdout
    erros = []
    vistos = {}
    somas = {}
    competencia = "-"
    raiz = None
    chamadas = 0
    universo_total = 0
    inicio = time.monotonic()
    limite = inicio + 39 * 60
    rastreador_anterior = sys.gettrace()
    arquivos = {}
    codigos = {}
    pendentes = {}
    pulsos = 0

    class TempoEsgotado(BaseException):
        pass

    def imprimir(*partes):
        print(*partes, file=saida, flush=True)

    def data_segura(valor):
        if isinstance(valor, dt.datetime):
            return valor.date().isoformat()
        if isinstance(valor, dt.date):
            return valor.isoformat()
        if isinstance(valor, str) and re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
            return dt.date.fromisoformat(valor).isoformat()
        return None

    def contexto(frame):
        col, dia = None, None
        while frame is not None:
            loc = frame.f_locals
            if col is None:
                objetos = [loc.get("colaborador"), loc.get("colab"), loc.get("c")]
                obj_self = loc.get("self")
                if obj_self is not None:
                    objetos.append(getattr(obj_self, "__dict__", {}).get("c"))
                for obj in objetos:
                    meta = getattr(type(obj), "_meta", None)
                    if getattr(meta, "model_name", None) == "colaborador":
                        pk = obj.pk
                        if isinstance(pk, int):
                            col = "col%d" % pk
                            break
                if col is None:
                    for chave in ("colaborador_id", "colab_id"):
                        pk = loc.get(chave)
                        if isinstance(pk, int) and not isinstance(pk, bool):
                            col = "col%d" % pk
                            break
            if dia is None:
                # handle incrementa "dia" antes de comparar; "k" é o dia julgado.
                # app/ponto/management/commands/e6_oraculo.py:319-323
                chaves = ("k", "data", "dia", "data_turno")
                for chave in chaves:
                    valor = data_segura(loc.get(chave))
                    if valor is not None:
                        dia = valor
                        break
            frame = frame.f_back
        return col or "-", dia or "-"

    def registrar(exc, frame=None):
        chave = id(exc)
        if chave in vistos:
            return
        vistos[chave] = exc
        tb = exc.__traceback__
        ultimo = None
        while tb is not None:
            ultimo = tb
            tb = tb.tb_next
        contexto_frame = frame or (ultimo.tb_frame if ultimo else None)
        col, dia = contexto(contexto_frame)
        tipo = type(exc).__name__
        origem = "-"
        if ultimo is not None:
            caminho = Path(ultimo.tb_frame.f_code.co_filename)
            if raiz is not None and caminho.is_relative_to(raiz):
                origem = "app/%s:%d" % (
                    caminho.relative_to(raiz).as_posix(), ultimo.tb_lineno
                )
            else:
                origem = "[fora-do-app]:%d" % ultimo.tb_lineno
        # Última linha sanitizada: nenhum conteúdo da mensagem é publicado. [nao verificado]
        ultima_linha = (tipo + ": [mensagem suprimida]")[:200]
        erros.append((competencia, col, dia, tipo, origem, ultima_linha))

    def linhas_de_handlers(codigo):
        if codigo in codigos:
            return codigos[codigo]
        nome = codigo.co_filename
        caminho = Path(nome)
        if raiz is None or not caminho.is_relative_to(raiz):
            codigos[codigo] = frozenset()
            return codigos[codigo]
        if nome not in arquivos:
            with tokenize.open(nome) as fonte:
                arvore = ast.parse(fonte.read(), filename=nome)
            arquivos[nome] = {
                item.body[0].lineno
                for item in ast.walk(arvore)
                if isinstance(item, ast.ExceptHandler) and item.body
            }
        linhas_codigo = {linha for _, _, linha in codigo.co_lines() if linha}
        codigos[codigo] = frozenset(arquivos[nome] & linhas_codigo)
        return codigos[codigo]

    def rastrear(frame, evento, arg):
        nonlocal pulsos
        if evento == "call":
            pulsos += 1
            if pulsos % 256 == 0 and time.monotonic() >= limite:
                raise TempoEsgotado()
            return rastrear if linhas_de_handlers(frame.f_code) else None
        if evento == "exception":
            pendentes[id(frame)] = arg[1]
        elif evento == "line":
            if frame.f_lineno in codigos.get(frame.f_code, ()):
                exc = sys.exc_info()[1] or pendentes.get(id(frame))
                if exc is not None:
                    registrar(exc, frame)
        elif evento == "return":
            pendentes.pop(id(frame), None)
        return rastrear

    def receber_log(logger, registro):
        # Não repassa mensagens, handlers de arquivo ou dados livres.
        if registro.exc_info and registro.exc_info[1] is not None:
            registrar(registro.exc_info[1], sys._getframe(1))

    def ler_relatorio(texto):
        linhas = texto.splitlines()
        padrao_counter = (
            r"\{(?:'[A-Za-z0-9_]+'\s*:\s*\d+"
            r"(?:,\s*'[A-Za-z0-9_]+'\s*:\s*\d+)*)?\}"
        )
        contadores = [
            ast.literal_eval(linha)
            for linha in linhas if re.fullmatch(padrao_counter, linha)
        ]
        donos = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*(ESTRUTURA|CADASTRO|BATIDA)\s+"
                r"(\d+) dia-colab\s+(\d+\.\d+) h\s+(\d+) colabs",
                linha
            ))
        ]
        soma = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*SOMA\s+(\d+)\s+"
                r"<- tem de ser igual a soma das classes \((\d+)\)", linha
            ))
        ]
        colabs = [
            m for linha in linhas
            if (m := re.fullmatch(r"colabs com divergencia: (\d+)", linha))
        ]
        falhas_espelho = [
            m for linha in linhas
            if (m := re.fullmatch(r"erros no espelho: (\d+)", linha))
        ]
        if not (
            len(contadores) == len(soma) == len(colabs)
            == len(falhas_espelho) == 1
            and len(donos) == 3
            and {m[1] for m in donos} == {"ESTRUTURA", "CADASTRO", "BATIDA"}
        ):
            raise RuntimeError("FormatoDaAutoridadeIncompleto")

        t = contadores[0]
        imprimir("COMPETENCIA", competencia)
        imprimir("[1]", {
            chave: t.get(chave, 0)
            for chave in (
                "dias_comparados", "bate_ate_10min",
                "esp_zero_e6_trabalho", "e6_zero_esp_trabalho",
                "diverge_10_60", "diverge_acima_60",
                "dia_batida_impar", "impar_que_BATE", "impar_que_DIVERGE",
                "impar_dentro_da_faixa", "impar_FORA_da_faixa",
                "impar_no_conjunto_do_dia", "impar_SO_POR_CORTE_do_oraculo",
                "dia_sem_linha_no_espelho", "dia_sem_lavratura_no_espelho",
                "dia_isento_ou_ausencia", "dia_sem_trabalho_ambos",
                "esp_desconta_janela_declarada", "esp_lido_pelo_pago_h",
            )
        })
        for linha in linhas:
            if re.fullmatch(
                r"BATE \(ate 10 min\): \d+\.\d+% de \d+ dias comparados", linha
            ):
                imprimir("[2]", linha)
        for m in donos:
            imprimir("[3]", m[0].strip())
        imprimir("[4] TOTAL divergentes", soma[0][2])
        imprimir("[5]", colabs[0][0])
        imprimir("erros_no_espelho", falhas_espelho[0][1])

        # Só compara os valores da SOMA nativa; não soma novamente os donos.
        # app/ponto/management/commands/e6_oraculo.py:415-427
        somas[competencia] = (
            soma[0][0].strip(), int(soma[0][1]) == int(soma[0][2])
        )
        if not somas[competencia][1]:
            raise RuntimeError("SomaDaAutoridadeDivergente")
        if int(falhas_espelho[0][1]) and not erros:
            raise RuntimeError("ErroDoEspelhoSemDetalhamento")

    try:
        with (
            patch.object(logging.Logger, "handle", receber_log),
            redirect_stdout(io.StringIO()),
            redirect_stderr(io.StringIO()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.core.management import call_command
            from django.db import connection, transaction
            from ponto import janelas
            from ponto.management.commands import e6_oraculo
            from ponto.services import espelho as modulo_espelho

            raiz = Path(e6_oraculo.__file__).resolve().parents[3]
            if (
                settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
                or rastreador_anterior is not None
            ):
                raise RuntimeError("AmbienteIncompativel")

            Empresa = apps.get_model("colaboradores", "Empresa")
            FM = apps.get_model("ponto", "FechamentoMensal")
            real_espelho = modulo_espelho.espelho_do_colab

            # Contagem para ETA, seguindo apenas a seleção do comando.
            # app/ponto/management/commands/e6_oraculo.py:291-302
            with transaction.atomic():
                try:
                    empresas = list(Empresa.objects.filter(ativa=True).order_by("pk"))
                    for mes in (10, 11):
                        competencia = "%02d/2026" % mes
                        quantidade = (
                            FM.objects.filter(
                                mes=mes, ano=2026,
                                colaborador__empresa__ativa=True,
                            ).values("colaborador_id").distinct().count()
                        )
                        universo_total += quantidade
                        imprimir("[0] UNIVERSO", competencia, quantidade)
                        janelas_vistas = {
                            janelas.janela_fechamento(mes, 2026, empresa)
                            for empresa in empresas
                        }
                        for ini, fim in sorted(janelas_vistas):
                            imprimir("JANELA", competencia, ini.isoformat(), fim.isoformat())
                finally:
                    transaction.set_rollback(True)

            def observar_espelho(*args, **kwargs):
                nonlocal chamadas
                if time.monotonic() >= limite:
                    raise TempoEsgotado()
                if chamadas and chamadas % 25 == 0:
                    decorrido = time.monotonic() - inicio
                    estimativa = decorrido * universo_total / chamadas / 60
                    imprimir(
                        "ESTIMATIVA", "processados", chamadas,
                        "universo", universo_total,
                        "minutos_total", round(estimativa, 1),
                        "ate_40_min", estimativa <= 40,
                    )
                chamadas += 1
                try:
                    with transaction.atomic():
                        try:
                            resultado = real_espelho(*args, **kwargs)
                            if connection.needs_rollback:
                                raise RuntimeError("TransacaoDoEspelhoAbortada")
                            return resultado
                        finally:
                            transaction.set_rollback(True)
                except BaseException as exc:
                    registrar(exc, sys._getframe())
                    raise

            for mes in (10, 11):
                competencia = "%02d/2026" % mes
                captura = io.StringIO()
                try:
                    with transaction.atomic():
                        try:
                            with connection.cursor() as cursor:
                                cursor.execute("SET LOCAL statement_timeout = '60s'")
                                cursor.execute("SET LOCAL lock_timeout = '5s'")
                            with patch.object(
                                modulo_espelho, "espelho_do_colab", observar_espelho
                            ):
                                sys.settrace(rastrear)
                                try:
                                    # Mesma entrada declarada pelo placar; sem CSV.
                                    # app/core/placar_estrutural.py:138
                                    call_command(
                                        "e6_oraculo", mes=mes, ano=2026,
                                        stdout=captura, stderr=captura,
                                        no_color=True,
                                    )
                                finally:
                                    sys.settrace(rastreador_anterior)
                        finally:
                            transaction.set_rollback(True)
                    ler_relatorio(captura.getvalue())
                except BaseException as exc:
                    sys.settrace(rastreador_anterior)
                    registrar(exc)
                    if isinstance(exc, TempoEsgotado):
                        break
    except BaseException as exc:
        sys.settrace(rastreador_anterior)
        registrar(exc)
    finally:
        sys.settrace(rastreador_anterior)

    imprimir("TEMPO_SEGUNDOS", round(time.monotonic() - inicio, 1))
    for tipo, quantidade in sorted(Counter(e[3] for e in erros).items()):
        imprimir("ERROS_POR_TIPO", tipo, quantidade)
    for comp, col, dia, tipo, origem, ultima in erros:
        imprimir("ERRO", comp, (col, dia, tipo, origem, ultima))
    for comp in ("10/2026", "11/2026"):
        if comp in somas:
            linha, confere = somas[comp]
            imprimir("SOMA", comp, linha.removeprefix("SOMA").strip(), confere)
        else:
            imprimir("SOMA", comp, "NAO_DISPONIVEL", False)
    if erros:
        imprimir("INCOMPLETO:", len(erros))


sonda_r1()
```

As exceções tratadas no código do aplicativo também são observadas: o comando captura falhas do espelho, e o espelho possui fallbacks que esconderiam detalhes numa captura apenas externa. A mensagem da última linha do traceback é suprimida integralmente; permanecem tipo, origem e contexto disponível, sem inventar colaborador ou dia ausente. (`app/ponto/management/commands/e6_oraculo.py:305`; `app/ponto/services/espelho.py:411`, `:569`, `:650`; implementação proposta [não verificado].)

Funções reais chamadas, diretamente ou pelo comando, e suas autoridades na cópia fornecida:

| Função | Autoridade / referência |
|---|---|
| `Command.handle`, via `call_command` | Seleciona o universo, compara os dias e publica a tabela/SOMA. `app/ponto/management/commands/e6_oraculo.py:272`, `:291`, `:413`. |
| `janela_fechamento` | Resolve a competência pelo corte da empresa. `app/ponto/janelas.py:54`. |
| `espelho_do_colab` | Fonte compartilhada do espelho; o wrapper chama a implementação original. `app/ponto/services/espelho.py:1194`. |
| `autoridade_do_periodo` e `montar_dias` | Alimentam o resultado e as linhas lidas pelo comando. `app/ponto/services/espelho.py:971`, `:365`, `:1227`, `:1262`. |
| `folha_manda` | Alimenta os totais do espelho pela folha. `app/relatorios/cartao_pela_celula.py:296`; `app/ponto/services/espelho.py:1284`. |
| `InsumosDoDono.__init__`, `.dono`, `.fase` | Montagem compartilhada dos insumos do dono, incluindo a lavra de fase. `app/ponto/management/commands/e6_oraculo.py:128`, `:164`, `:172`. |
| `dono_da_divergencia` | Juiz efetivo de ESTRUTURA/CADASTRO/BATIDA, com a precedência vigente. `app/ponto/management/commands/e6_oraculo.py:53`, `:98`. |
| `cadastro_do_dia_carregado` | Fornece os defeitos cadastrais do dia ao juiz do dono. `app/ponto/supra_juiz.py:166`. |
| `minutos_do_oraculo` | Derivação central dos minutos comparados. `app/ponto/calculador/nucleo.py:42`. |
| `batidas_apuraveis`, `turnos_do_colab`, `realizado_do_dia` | Universo de batidas e autoridade do realizado usados pelos insumos. `app/ponto/turnos.py:2844`, `:1848`, `:772`; `app/ponto/management/commands/e6_oraculo.py:144`, `:151`, `:195`. |
| `dias_de_fase_as_of` | Lê a fase pela lavra mais recente que cobriu cada dia. `app/escala/services/fase_12x36.py:117`; `app/ponto/management/commands/e6_oraculo.py:166`. |

O R1 não chama o emissor de chamados; esse caminho pertence ao cartório com `apply_=True`. Portanto, a sonda não fornece contagem de emissão. (`app/ponto/services/cartorio.py:1725`, `:1777`.)

A contagem correta é **8 EM_CURSO + 1 ESPERA_RONALD = 9 não concluídos**. O BACKLOG pede este termômetro especificamente para cinco itens e a confirmação de um sexto já FEITO; ESTRUTURAL delega o dono ao comando e mantém os placares separados. (`app/core/espelho_verdade.py:36`, `:48`, `:81`, `:83`, `:130`, `:138`, `:168`, `:172`, `:178`; `app/docs/BACKLOG.md:1478`; `app/docs/ESTRUTURAL.md:34`.)

| Item / estado declarado | Número do script que atualiza; alcance |
|---|---|
| **E1-passivo-53 — EM_CURSO** (`app/core/espelho_verdade.py:36`) | Não mede. O contador específico é `vigencia_impossivel.medir()`: vínculos, colaboradores, ativos e competências lavradas. (`app/escala/services/vigencia_impossivel.py:23`, `:48`.) |
| **E1-sem-previsao — EM_CURSO** (`app/core/espelho_verdade.py:48`) | **[1] `esp_zero_e6_trabalho`** atualiza o indicador citado, mas não comprova previsão válida. A medição específica é `previsto_cego.medir()`, que usa a janela corrente. (`app/ponto/management/commands/e6_oraculo.py:358`; `app/escala/services/previsto_cego.py:114`, `:138`.) |
| **E3-turno-aberto — EM_CURSO** (`app/core/espelho_verdade.py:81`) | **[1] ímpares, batem/divergem e fora da faixa** atualizam o indicador citado. Não são um censo exato de turnos abertos; isso exigiria contar o realizado pela autoridade dos turnos. (`app/ponto/management/commands/e6_oraculo.py:347`, `:386`; `app/ponto/turnos.py:772`.) |
| **E4-rotulo-do-desenho — EM_CURSO** (`app/core/espelho_verdade.py:130`) | Não mede. A prova declarada envelheceu: a pílula já usa `desenho_do_turno`; falta conferir o selo indicado pelo próprio template. (`app/templates/escala/tipos_lista.html:55`.) |
| **E5-le-da-celula — EM_CURSO** (`app/core/espelho_verdade.py:138`) | Não mede a origem do fechamento nem sua conclusão; a pendência declarada é “falta o status encer

[truncado em 20000 de 21491 caracteres]

## 06/11 10:43 · sessao a6f3c423f042 · - · - · origem=Ronald (tmux)

1. **Procede — fluxo da C5 até lista/tela/PDF, sem emissão automática.** `_nunca_bateu` lê `colabs_nunca_bateu` e produz uma assinatura por colaborador, com contagem e primeiras três datas no texto (`app/escala/services/cadastro_realidade.py:169-175`). `lista` agrega C5 ao cadastro (`:802-805`), busca pessoa sem filtrar situação (`:701-704`), mantém a linha desde que pessoa exista e empresa seja visível (`:823-826`), contabiliza cadastro/C5 (`:854-857`, `:882-898`) e usa ação/link de cadastro individual (`:628-630`). Desligado sem vínculo vigente fica no grupo “sem vinculo vigente” (`:867`). A view usa essa lista para HTML ou PDF (`app/escala/views.py:487-492`); HTML renderiza detalhe e hyperlink (`app/templates/escala/cadastro_x_realidade.html:25-28`); PDF percorre exatamente os postos/assinaturas recebidos (`app/escala/services/cadastro_realidade.py:1013-1024`). C5 é só hyperlink; os formulários POST da tela são exclusivos da C7 (`app/templates/escala/cadastro_x_realidade.html:28`). Link C5 existe e detalhe não exclui desligado (`app/colaboradores/urls.py:23`, `app/colaboradores/views.py:461-467`); exige `ver_cadastro`, enquanto lista exige `ver_escala` (`app/colaboradores/views.py:459`, `app/escala/views.py:485`). Ocorrência concreta de falta dessa permissão: **[não verificado]**.

2. **Procede — C5 não fornece dias ao `dias_na_lista`; não cura automaticamente o fio do admin.** A assinatura C5 tem apenas `codigo` e `detalhe`, nenhuma chave `data` (`app/escala/services/cadastro_realidade.py:172-174`); `dias_na_lista` só inclui assinatura que tem `data` (`:974-980`). O vigia carrega tanto mapa de dias quanto conjunto de pessoas (`app/chamados/services/vigia_fio.py:216-218`). Assim, desligado com somente C5 passa a estar em `_colabs`, permanece fora de `_dias`, e C5 sozinha não satisfaz `dia_do_admin`/balde `fio_do_admin` (`:222-228`, `:313-314`). Na ausência das condições anteriores, `censo_alcance` pode mudar a causa de “e/outra” para “d/colab na lista, dia fora”, mantendo fio morto (`:654-680`). Isso é limitação comprovada do transporte da C5 para leitura por dia; ocorrência em produção **[não verificado]**.

3. **Procede — `vigia_retido_calado` passa desligado de calado a roteado por presença da pessoa, sem validar dia nem competência.** O consumidor usa `set(colabs_nunca_bateu(empresas))`, descartando as listas de datas (`app/folha/vigia_retido_calado.py:40-41`). Para cada retrato da última lavra diária por empresa (`:35-39`), toda linha bloqueada de fio `ninguem` cujo `colab` consta no conjunto vira roteada, reduzindo calados (`:44-53`). Presença C5 noutra competência já basta para essa classificação; coincidência exata dos dias do retrato com os da C5 não é testada ali (`:41`, `:46`). O comando imprime e pode retornar erro com alarme; não tem aplicação de reparo (`app/folha/management/commands/vigiar_retido_calado.py:28-39`). Não gera chamado/pergunta/push (`app/folha/vigia_retido_calado.py:27-55`).

4. **Não procede — presença C5 não faz comandos fecharem chamados C8/C7.** `fechar_chamados_vinculo_na_lista` lê `lista`, seleciona exclusivamente assinatura `C8` e chamados `vinculo_divergente` (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35-45`); só `--apply` chama `ch.cancelar` (`:52-59`). `fechar_avisos_regua_na_lista` seleciona exclusivamente `C7` e módulo `regua_suspeita` (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27-37`); só `--apply` cancela (`:42-49`). Entrada exclusivamente por C5 não muda elegibilidade de nenhum fechamento nesses comandos (`respectivamente :40`, `:32`).

5. **Não procede — `placar_estrutural` não executa essa leitura.** Há referência declarativa em string `fonte` (`app/core/placar_estrutural.py:149`), mas o módulo declara atualização por quem mede (`:28`) e `placar()` apenas copia/processa `RESULTADOS`, sem I/O (`:635-660`). A inclusão do desligado não remede automaticamente o número R2 (`:140-149`).

6. **Procede — há escrita em disco num consumidor indireto; não confundir com escrita de negócio.** Serviço do vigia censo lê e devolve contadores (`app/chamados/services/vigia_fio.py:318-323`); comando monta lista e chama censos (`app/chamados/management/commands/vigiar_fio_morto.py:163-173`). Em rodada de frota, o comando grava carimbo com resultados, sem `--apply` (`:210-226`), e `_carimbar` cria pasta, arquivo temporário e faz `os.replace` (`:127-134`). Recorte `--empresa` não carimba (`:228-230`). C5 pode alterar o conteúdo dos contadores carimbados conforme achado 2; essa escrita preexistente não cria chamado/pergunta/push (`:210-226`).

7. **Não procede — exclusões remanescentes de desligado não são outra exclusão da C5/lista; respondem outros universos.**
   - `universo_adesao` lê `nunca_bateu` só de ativos (`app/chamados/services/adesao.py:25-34`), e sincronizador ainda veta desligado (`:66-67`); ele abre/reabre chamados de adesão (`:74-92`), portanto é emissão, distinta da mesa administrativa C5.
   - Cartório continua vetando emissão de furo e parcial para desligado (`app/ponto/services/cartorio.py:1696-1697`, `:1762`).
   - “Quem cobrar” mantém universo declarado de ativos (`app/escala/services/furos_diarios.py:247-259`) e elimina desligado antes de marcar `nunca_bateu` por padrão de batidas (`:272-283`); sua view usa esse universo (`app/relatorios/views.py:1637-1642`). É tela de cobrança, não C5.
   - Dia da Frota lê vereditos incluindo rótulo nunca bateu, mas universo só ativo (`app/relatorios/dia_da_frota.py:13`, `:34`, `:54-60`).
   - A exclusão remanescente em `_afastado_sem_registro` é da C4/furo (`app/escala/services/cadastro_realidade.py:541-556`), não da C5.
   - Nenhum filtro posterior por desligado aparece no fluxo efetivo da lista: `_vinculos` lê IDs sem situação e o filtro final é existência/empresa (`app/escala/services/cadastro_realidade.py:701`, `:825-826`). Por isso esses universos divergem por finalidade, mas não anulam a nova inclusão C5.

## 06/11 10:43 · sessao d7f337b57793 · - · - · origem=Ronald (tmux)

1. **Não procede — segundo escritor de produção introduzido.** O diff apenas consulta (`app/escala/services/cadastro_realidade.py:160-165`). `CelulaDia.veredito` passa por `lavrar_veredito`, delegado pelo modelo (`app/escala/models.py:1819-1828`) à porta, que atribui e salva (`app/ponto/portas/celula.py:814-819`). Chamadores automáticos: `app/ponto/services/cartorio.py:889`, `:1242`, `:1802`; decisão humana: `app/ponto/services/veredito_celula.py:330`. As gravações diretas novas são fixtures de teste (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:29-34`).

2. **Mapa de escritores dos campos.**
   - `CelulaDia.data`: campo em `app/escala/models.py:1685`; criado pela porta em `app/ponto/portas/celula.py:253-254` e `:633-636`. Não localizei reatribuição de data de célula em produção; ausência exaustiva de escrita dinâmica: **[não verificado]**.
   - `Colaborador.situacao`/`data_demissao`: definições em `app/colaboradores/models.py:323-325`; operação dona em `app/colaboradores/services/colaborador.py:32`; persistência em `:55-76`, criação em `:91-93`. Desligamento grava ambos pela porta (`app/colaboradores/services/desligamento.py:104-108`); agendamento grava somente demissão/motivo (`:82-86`); reativação põe ativo e limpa demissão (`:209-214`). Admissão cria ativo pela porta (`app/colaboradores/views.py:1181-1205`).
   - Outros escritores de `situacao`: lançamento de INSS escreve afastado pela porta (`app/ponto/views.py:2433-2435`); reversor escreve ativo pela mesma porta e deixa trilha (`app/ponto/management/commands/reverter_situacao_afastado.py:65-80`). Django admin de Colaborador é somente leitura (`app/colaboradores/admin.py:48`, `:65-68`).

3. **Leitores dos campos, além da consulta alterada.** `CelulaDia.data` alimenta o mapa por colaborador/dia (`app/escala/alimentacao.py:73-77`), e o retentor lê data/veredito em `app/folha/export.py:1206`, `:1224-1255`; a auditoria humana lê os mesmos campos (`app/ponto/services/veredito_celula.py:107-109`). `data_demissao` é lida pelo retentor sem consultar situação (`app/folha/export.py:1198-1201`), pelo cadastro do dia (`app/ponto/supra_juiz.py:142-162`, `:194`) e pelo cron de efetivação (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38-40`). `situacao` também filtra outras famílias da lista (`app/escala/services/cadastro_realidade.py:221`, `:420`, `:541`) e a evidência de vínculo (`app/ponto/services/vinculo_na_lista.py:70`, `:151-153`). Um inventário integral dos leitores dinâmicos de todos os campos continua **[não verificado]**.

4. **Não procede — RED sem mordida contra os cinco mutantes especificados, pela leitura das asserções.**

   | Mutante | Asserção que o distingue |
   |---|---|
   | M1, restaurar exclusão inteira | Desligado precisa existir e gerar C5/porta: `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:49-57`; dias explícitos: `:60-62` |
   | M2, `__gt` → `__gte` | Demissão é um dos dias da fixture (`:23-24`) e deve entrar (`:62`) |
   | M3, tirar corte | Dias posteriores existem (`:24`) e não podem entrar (`:62`) |
   | M4, cortar ativo | Ativo com demissão preenchida conserva todos os dias (`:81-84`) |
   | M5, excluir desligado sem data | Lista inteira de dias é exigida (`:69-73`); teste vizinho também exige presença (`app/folha/tests/test_vigia_retido_calado.py:104-106`) |

   A testemunha realmente é chamada, sem copiar a regra (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:40-46`, `:65-66`). A execução e as contagens RED/GREEN/mutantes informadas no commit são **[não verificado]**: os arquivos `logs/esemvivab/*` mencionados no anexo não estão disponíveis nesta cópia.

5. **Procede — cobertura da igualdade é mais estreita que a alegação.** A fixture não fornece admissão (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:30-34`), enquanto a folha corta por admissão (`app/folha/export.py:1198-1199`). Só o teste de ativo cobre hoje/ontem (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:78-80`); a comparação contra folha usa os dias de desligado entre 5 e 12 de outubro (`:23-24`, `:65-66`), sem testar desligado hoje, futura célula acusadora, pré-admissão ou isento. A folha tem guarda própria de isento (`app/folha/export.py:1191-1196`).

6. **Procede — referência documental incorreta nesta cópia.** O novo docstring diz que a janela de retenção vive em `1224-1229` (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:4`), mas o corte por admissão/demissão real está em `app/folha/export.py:1198-1201`; `:1224-1229` percorre os dias e define `_dia_dem`. O teste importa a função pelo nome e continua chamando a testemunha correta (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:40-46`).

7. **Dúvida — ordem de trava e pouso antes do pré-requisito.** O trecho produtivo alterado contém somente uma consulta, sem nova trava/transação (`app/escala/services/cadastro_realidade.py:160-165`); portanto não demonstra inversão de locks. O contrato exige suíte inteira verde antes do opus final e impede certificação de pouso apenas pelo Codex (`CLAUDE.md:364-373`). A sequência efetiva de suíte inteira → opus → pouso deste commit é **[não verificado]**; o anexo relata somente testes direcionados/vizinhos. `app/docs/RELATO.md:3` ainda registra A/B como candidatas e descreve a exclusão antiga, sem comprovar o histórico desta obra.

## 06/11 10:43 · sessao cbe5c800c718 · - · - · origem=Ronald (tmux)

1. **Procede — a igualdade de janelas prometida não é geral.** A C5 usa `[início da competência anterior, ontem]`, sem admissão, com corte de demissão apenas quando `situacao='desligado'` (`app/escala/services/cadastro_realidade.py:154-164`). O retentor usa `[ini,fim]` fornecido, recortado por admissão e demissão independentemente da situação (`app/folha/export.py:1198-1206`). Divergências:

   - **Admissão:** C5 inclui célula `nunca_bateu` anterior à admissão dentro da janela; retentor exclui (`cadastro_realidade.py:160-164`; `export.py:1198-1199`).
   - **Ativo com demissão preenchida:** C5 mantém dias posteriores; retentor corta (`cadastro_realidade.py:162-163`; `export.py:1200-1201`). O teste exige esse comportamento da C5, mas não compara esse caso com a testemunha (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:81-84`).
   - **Hoje e futuro:** C5 exclui por `data__lte=ontem`; retentor pode reter uma célula acusadora hoje/futura até `fim`, pois `d < hoje` guarda apenas os ramos de célula ausente/sem veredito (`cadastro_realidade.py:155-161`; `export.py:1230-1256`).
   - **Competências:** C5 sempre agrega anterior e corrente transcorrida; retentor pode receber qualquer período. Com anterior isolada, C5 acrescenta corrente; com corrente isolada, acrescenta anterior; com competência mais antiga, C5 não a cobre (`cadastro_realidade.py:159-161`; `export.py:1168,1198-1206`; `app/ponto/janelas.py:60-72`).
   - **Isenção:** C5 não verifica Art.62; retentor retorna vazio (`cadastro_realidade.py:160-164`; `export.py:1191-1196`; `app/core/constants.py:13-18`).
   - **Protestos:** C5 aceita qualquer célula `nunca_bateu`; retentor ainda aplica não cobráveis e pausa (`cadastro_realidade.py:160-164`; `export.py:1245-1254`). A ocorrência dessas combinações em produção é **[não verificado]**.

2. **Não procede — divergência no dia da demissão ou na demissão nula, para desligado `nunca_bateu`.** C5 só exclui `data > data_demissao`; o retentor percorre `d <= fim`, e `_dia_dem` só isenta `sem_celula`. Sem demissão ambos deixam de cortar por esse campo (`cadastro_realidade.py:162-163`; `export.py:1200-1201,1225-1256`).

3. **Não procede, em sentido literal — duplicação de juiz nominalmente registrado para “quais dias do desligado a folha retém”.** JUIZES.md é lookup e dá precedência ao registro vivo (`app/docs/JUIZES.md:7-15`). O registro declara `classificar_export` para aprovação/TXT e entrada na folha, não declara nominalmente `motivos_retencao_celula` para os dias (`app/core/juizes.py:160-168,288-323,699-712`). Existe cadeia registrada `classificar_export → motivos_retencao_espelho → motivos_retencao_celula` (`export.py:960,1261-1267`).

4. **Procede — derivação paralela de fonte única.** O leitor declara cobrir a janela da testemunha, porém escreve SQL próprio com as diferenças acima (`cadastro_realidade.py:140-144,159-163`). O retentor já ganhou `com_dias=True` precisamente para impedir cópia dessa pergunta (`export.py:1178-1183`). Há precedente explícito de remoção de uma cópia que divergia em admissão/demissão e protestos (`app/folha/previa.py:149-170`). A lei exige que leitores usem a mesma autoridade (`CLAUDE.md:20-22`).

5. **Procede — a premissa “desligado retém a folha” precisa distinguir retenção da célula de bloqueio no pipeline real.** Se há demissão até o fim da competência, `classificar_export` sai por rescisão antes de consultar o retentor (`export.py:54-60,858-863,960`). O Retrato coloca rescisão em `modulo_proprio`, fora de `bloqueado` (`app/folha/retrato_folha.py:29-33,89-95`). Portanto o teste direto de `motivos_retencao_celula` não prova que aquele desligado estaria bloqueado no Retrato real (`test_esemvivab_desligado_nunca_bateu.py:40-46`).

6. **Procede — os testes de equivalência não cobrem as divergências.** A fixture do desligado tem somente dias 05–12/10, hoje 25/10, admissão nula e sem isenção/protestos informados (`test_esemvivab_desligado_nunca_bateu.py:21-34`; `app/colaboradores/models.py:324`). Empresa usa corte padrão 21; assim esses dias são todos da competência anterior (`app/colaboradores/models.py:46`; `app/ponto/janelas.py:10-21,24-33`). As comparações com o retentor estão apenas nos dois casos de desligado (`test_esemvivab_desligado_nunca_bateu.py:65-73`).

7. **Não procede — competência entregue como diferença direta entre os dois corpos.** Nenhum deles consulta `competencia_entregue`; a C5 inclui a anterior incondicionalmente e o retentor trabalha o período recebido (`cadastro_realidade.py:154-165`; `export.py:1192-1258`). O juiz específico de entrega existe em outro lugar (`app/core/juizes.py:312`; `export.py:556-584`). Se a anterior está entregue em produção é **[não verificado]**.

## 06/11 10:42 · sessao 481eb41296f1 · - · - · origem=Ronald (tmux)

Recomendo manter `F.perguntar_pela_celula` como autoridade da sonda e declarar que a medição descreve a recusa atual da fábrica sobre a célula lavrada; o ingresso histórico pelo cartório continua [nao verificado]. A fábrica efetivamente recebe a célula em `julgar_colab`; o comando é outra camada e não é obrigatório para chamar essa autoridade real. (`app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:145`)

`julgar_colab(apply_=False, forcar=True)` não cobre esse ingresso: a fábrica está dentro de `if apply_`, e o ramo `nunca_bateu` executa classificação/lavra somente dentro de outro `if apply_`. Portanto, adicioná-lo não elimina a limitação e cria uma segunda medição incompleta. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:1207`, `app/ponto/services/cartorio.py:1226`)

O emissor separado de chamado é `detectar_ausencias.Command._abrir_chamado`; seus parâmetros reais vêm de dois ramos específicos do cartório. Chamá-lo com parâmetros montados à mão mudaria a pergunta da sonda. Não encontrei modo somente leitura que atravesse integralmente esse emissor e depois a fábrica. (`app/ponto/management/commands/detectar_ausencias.py:570`, `app/ponto/services/cartorio.py:1717`, `app/ponto/services/cartorio.py:1740`, `app/ponto/services/cartorio.py:1768`, `app/ponto/services/cartorio.py:1778`)

Se o contrato for interpretado como exigência de medir também o chamado, seria necessário executar `apply_=True` com interceptação de todas as portas reais de escrita e lidar com as releituras posteriores; interromper na primeira escrita não chega à fábrica e não satisfaz essa interpretação. (`app/ponto/services/cartorio.py:1802`, `app/ponto/services/cartorio.py:997`, `app/ponto/services/cartorio.py:1038`)

## 06/11 10:41 · sessao 829ee648074e · - · - · origem=Ronald (tmux)

- A porta histórica é `diff_calculador.Command.handle`, com `--pares-da-autoridade`; ela chama `insumos_do_calculador` e `rubricas_por_dia` reais. (`app/ponto/management/commands/diff_calculador.py:287`, `:309`, `:312`, `:417`)
- Neste hash o instrumento ainda omite `minutos_extra_100_feriado` e soma indiscriminadamente `periodos_ft` como folga. Não serve para atribuir essas duas diferenças ao v2 sem confrontar o emissor real. (`app/ponto/management/commands/diff_calculador.py:253`, `:263`, `:268`; diagnóstico histórico: `app/docs/RELATO.md:560`)
- É possível observar o comando sem reconstruir cálculos: trace restrito ao `Command.handle.__code__`; na entrada do ramo divergente, linha 539, recolher `c.pk`, `k`, `r`, `a`, `b`; no retorno recolher `S`, `div`, `colabs_div`, `T`, `linhas`. Isso mantém valores sem arredondamento e respeita o limiar escolhido pelo comando. (`app/ponto/management/commands/diff_calculador.py:535`, `:538`, `:539`, `:542`)
- Soma dos deltas divergentes não equivale necessariamente ao delta de `S`: `S` recebe também diferenças inferiores ou iguais a 0,02 h. Para reconciliação completa, conservar uma parcela explícita residual/sub-limiar. (`app/ponto/management/commands/diff_calculador.py:535`, `:536`, `:537`, `:538`)
- Os erros capturados silenciosamente pelo comando podem ser registrados por eventos `exception` no frame do comando e nas funções locais do mesmo arquivo, sem alterar suas decisões. Sítios de captura: `app/ponto/management/commands/diff_calculador.py:148`, `:226`, `:314`, `:350`, `:368`, `:405`.
- O emissor real `dia_pago.lavrar` distribui HE100 de feriado e as parcelas de folga; o fechamento chama esse mesmo emissor nas versões `motor` e `oraculo`. (`app/ponto/services/dia_pago.py:135`, `:183`, `:223`, `:250`; `app/ponto/services/fechamento.py:1185`, `:1261`)
- Escritas exatas do emissor: `DiaPago.objects.filter(...).delete()` e `DiaPago.objects.bulk_create(novas)`. Dentro de um wrapper que chama exclusivamente o `lavrar` original, os patches podem limitar-se ao manager usado por essas duas operações: `filter` devolve objeto com `delete` simulado; `bulk_create` recolhe as instâncias em memória. O retorno original continua sendo `len(novas)`. (`app/ponto/services/dia_pago.py:163`, `:357`, `:358`, `:359`)
- Para alcançar esse emissor por `--periodos-do-fechamento`, o wrapper de `fechamento.recalcular_fechamento_mes` precisa restaurar temporariamente o emissor real instrumentado: o comando substitui `dia_pago.lavrar` por `_nada` antes de chamar o fechamento. (`app/ponto/management/commands/diff_calculador.py:123`, `:137`, `:140`, `:142`)
- Os totais v2 podem vir do retorno real de `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)`, que contém o próprio objeto destacado do fechamento. (`app/ponto/services/fechamento.py:439`, `:1112`, `:1119`, `:1293`)
- O fechamento também engole falhas do emissor e por colaborador; é necessário observá-las para cumprir `INCOMPLETO`. Seu log por colaborador contém nome, portanto não deve ir diretamente ao stdout. (`app/ponto/services/fechamento.py:1222`, `:1281`, `:1287`, `:1290`)
- Tempo de execução e universo atual não podem ser certificados sem execução/dados adicionais. [nao verificado]

## 06/11 10:41 · sessao 481eb41296f1 · - · - · origem=Ronald (tmux)

Montagem e reconciliação:

- Use exatamente `V.censo_alcance(limite=10**7)`; filtre `x[4] == 'sem_porta' and x[5] == 'd'`; agrupe por `x[1]`; monte `InsumosDoDono(c, min(datas), max(datas), hoje, agora=agora)` e retenha somente `ins.dono(data) == 'ESTRUTURA'`. Esta é a montagem do anexo; a autoridade do primeiro total é `r['alcance_d']`, e a do subconjunto é o retorno de `ins.dono`. (`app/chamados/services/vigia_fio.py:542`, `app/ponto/management/commands/e6_oraculo.py:128`, `app/ponto/management/commands/e6_oraculo.py:172`)
- A lista contém `[celula_id, colab_id, data, competencia, rota, causa, detalhe, papel]`; carregue a célula por `x[0]`, conferindo colaborador/data. Há unicidade colaborador/data na montagem do censo. (`app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:593`, `app/chamados/services/vigia_fio.py:629`)
- `200` e `182/13/5` devem ser expectativas históricas, nunca totais impostos ao processamento. Valores atuais na sombra: [nao verificado].

Caminho verdadeiro do evento:

`_cartorio_post_save_batida` → `_julgar_dias` → `cartorio.julgar_celula` → `cartorio.julgar_colab` → `fabrica_da_celula.perguntar_pela_celula`. (`app/ponto/signals.py:131`, `app/ponto/signals.py:85`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1960`, `app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:997`)

O comando `processar_cartorio` usa o mesmo `julgar_colab`, com `detectar_ausencias.Command` como emissor do chamado do dia. (`app/ponto/management/commands/processar_cartorio.py:36`, `app/ponto/management/commands/processar_cartorio.py:42`, `app/ponto/management/commands/processar_cartorio.py:65`)

Não confundir isso com `post_save` de `CelulaDia`: a porta documenta expressamente que o gatilho é o ato de corrigir cadastro, e recusa adicionar sinal à entidade. (`app/ponto/portas/celula.py:362`)

Instrumentação sem copiar predicados:

- `patch(..., wraps=real)` nas funções `dia_sem_trabalho`, `vencidos_no_instante`, `parcial_so_lista`, `_ata_do_dia`, `motivos_pela_ata`, `limbo_vai_a_pauta`.
- Para os retornos inline, `sys.settrace` limitado aos `__code__` reais de `motivos_a_perguntar`, `motivos_pela_ata`, `perguntar_pela_celula`; registrar `frame.f_lineno` no evento `return`. Assim a classificação decorre da linha executada, sem reproduzir `situacao`, `data`, `veredito` ou os filtros.
- Guardas de `motivos_a_perguntar`, na ordem: dia sem trabalho/inativo (`91–92`); futuro (`93–94`); vencimento calculado (`98`); veredito absolvente (`99–100`); parcial só lista (`102–104`); deduplicação após motivos da ata (`118–126`). (`app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:93`, `app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:126`)
- Guardas internas dos motivos: vencidos vazio (`45–46`), ata não autoriza (`50–51`), faltantes `None` com vencidos não `None` (`52–54`), classificador sem motivo (`56–57`), exclusão de marcos não vencidos (`60–61`), ausência de correspondência de motivo (`62–64`). (`app/chamados/services/fabrica_da_celula.py:45`, `app/chamados/services/fabrica_da_celula.py:50`, `app/chamados/services/fabrica_da_celula.py:52`, `app/chamados/services/fabrica_da_celula.py:56`, `app/chamados/services/fabrica_da_celula.py:60`, `app/chamados/services/fabrica_da_celula.py:62`)
- `parcial_so_lista` deve ter a própria linha de retorno registrada: `399` distingue parcial sem período; `400` distingue a regra de julgamento anterior/ausência de fato novo. A instrumentação não precisa copiar esses testes. (`app/ponto/services/cartorio.py:398`, `app/ponto/services/cartorio.py:400`)
- Depois dos motivos, limbo entregue segue para Pauta DP e retorna `[]`; isso é destino alternativo, não ausência de emissão. (`app/chamados/services/fabrica_da_celula.py:171`, `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:173`)

Armadilhas:

- `_ata_do_dia` captura `Exception` e devolve `(True, None)`. O contrato exige observar essa captura; envolver `core.observ.registrar_engolido` para contar a exceção recebida preservando/repropagando conforme desenho da sonda. Contar só erros que escapam do emissor perderia estes casos. (`app/chamados/services/disputa_emissao.py:828`, `app/chamados/services/disputa_emissao.py:830`, `app/chamados/services/disputa_emissao.py:832`)
- Chamar diretamente `perguntar_pela_celula` prova a resposta atual do emissor para o estado lido. Não demonstra que o evento histórico efetivamente o chamou: `julgar_colab` só chega ali com `apply_` e célula presente no `cods` do ato. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:986`, `app/ponto/services/cartorio.py:995`)
- Para não vazar conteúdo, represente o último frame como `arquivo:linha:funcao`, com até 200 caracteres; não imprima `str(exc)`, SQL nem linha de código contendo dados. A existência de dados pessoais em mensagens concretas permanece [nao verificado].
- Tempo de execução na sombra: [nao verificado]. O custo amplo é do censo e do motor solicitado uma vez por colaborador pelo carregador; as 200 chamadas diretas da fábrica evitam rejulgar a frota. (`app/chamados/services/vigia_fio.py:582`, `app/ponto/management/commands/e6_oraculo.py:154`, `app/ponto/management/commands/e6_oraculo.py:159`)

## 06/11 10:41 · sessao faa68ab99b0c · - · - · origem=Ronald (tmux)

R1 chama apenas o juiz do dono; cartório e emissor são caminhos separados:

- `app/ponto/management/commands/e6_oraculo.py:304`: chama `espelho_do_colab`; `:316` cria `InsumosDoDono`; `:366` chama seu `dono`; `:183` delega a `dono_da_divergencia`.
- `app/ponto/services/espelho.py:1227`: `autoridade_do_periodo`; `:1262` chama `montar_dias`; `:1284` chama `folha_manda`. Não identifiquei escrita nesse caminho. [não verificado quanto à totalidade transitiva dos métodos de modelos]
- `app/ponto/services/cartorio.py:895`: `julgar_colab` é outra entrada. Seu corpo chama `classificar_dia` em `:1413`; emissor `_abrir_chamado` é chamado somente com `apply_=True`, em `:1725`–`:1740` e `:1777`–`:1778`. Portanto `apply_=False` não mede o emissor.
- `app/ponto/management/commands/detectar_ausencias.py:693`: emissor chama a porta `ChamadoColaborador.abrir`; depois pode escrever marcos (`:744`), contexto/nota (`:765`, `:768`), enviar push (`:778`) e criar fio (`:801`). Não basta interceptar só a criação para fazer uma sondagem desse caminho.

O problema mais importante do contrato é erro engolido:

- `app/ponto/management/commands/e6_oraculo.py:305`: exceção do espelho vira apenas contador e `continue`.
- `app/ponto/services/espelho.py:411`: falha lendo DiaPago vira `{}`.
- `app/ponto/services/espelho.py:569`: falha da grade vira grade vazia.
- `app/ponto/services/espelho.py:650`: falha de fatos/palavra vira ausência da palavra.
- `app/ponto/motor_calculo_v2.py:1387`: falha de carregar empresa vira `None`; `:1419`: falha das decisões HE vira `{}`.
- `app/escala/services/leitor_celula.py:27`: erro de conversão vira `None`.

Capturar somente exceções que escapam do `call_command` ou somente registros de logging não atende “erro nunca engolido”, porque os dois últimos fallbacks do motor não registram logger. A alternativa instrumental sem reproduzir lógica é ler os módulos com `ast`, mapear a primeira linha executável de cada `ExceptHandler` e usar `sys.settrace` nesses frames; quando essa linha executar, registrar `sys.exc_info()` com contexto col/data da pilha. Deduplicar a mesma instância de exceção preservando referências evita contá-la novamente quando for tratada em mais de um nível. A execução usa Python 3.13 segundo `app/Dockerfile:1`.

_2232 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
