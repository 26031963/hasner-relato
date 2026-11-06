# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 10:53** · publicado em **06/11/2026 10:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 06/11 10:40 · sessao 79d40633b87e · - · - · origem=Ronald (tmux)

O “resto dos 38” tem três blocos no registro: cenários B17–B33; fechamento/export para a propriedade “todo leitor lê o mesmo número”; e dimensionamentos de intervalo/tolerância. (`app/docs/BACKLOG.md:1621`)

**Oito cenários sem execução naquela costura, segundo o registro atual da bateria complementar:**

| Caso | O que falta / destino declarado | Evidência |
|---|---|---|
| B18 | Batida 00:00 UTC−3: pergunta dos leitores e do dia da jornada; `onde='fora'`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:69` |
| B20 | Segundo vínculo vigente sobreposto ao primeiro; `onde='cadastro'`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:76` |
| B24 | Só duas batidas da pausa: falta exercício pelos leitores/gravação; `onde='fora'`. O corte posterior manda EM ABERTO, soma dos pares fechados. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:100`; `app/docs/BACKLOG.md:1521` |
| B25 | Colaborador sem vínculo: precisa exercitar a porta de fechamento, pois a costura exige `ec`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:104` |
| B29 | Retratação pertence ao universo apurável antes do motor; aponta para tripwire próprio, não para caso de período. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:122` |
| B30 | Registro ainda pede vínculo com âncora futura; corte posterior descreve extrapolação máxima cadastrada por TipoEscala, inicialmente 180 minutos. Essa diferença impede declarar o caso atualizado apenas pelo rótulo. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:126`; `app/docs/BACKLOG.md:1521` |
| B31 | Falta exercício da porta de cadastro e lista dos existentes; registro ainda usa pergunta antiga sobre início=fim. Corte posterior manda recusar na porta e listar existentes. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:129`; `app/docs/BACKLOG.md:1521` |
| B33 | Atestado aprovado em folga com par fechado; fixture de cadastro ausente e metade da lei declarada sem dono. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:136` |

O selo de cobertura só exige caso executável para entradas `onde='roda'`; para `fora`/`cadastro` exige explicação textual. Portanto os oito itens acima não são oito testes vermelhos nem oito bugs da v3. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:503`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:514`)

**Casos já presentes que não devem ser apresentados como ainda ausentes:**

- B17: virada da competência, teste e asserções 720 na competência inicial e zero na seguinte. O `660+0` do backlog está desatualizado frente à correção explicitada pela bateria. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:252`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:266`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:44`)
- B19, B22, B23 e B32: casos tabelados com respostas numéricas. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:204`; `:213`; `:220`; `:226`)
- B21: teste presente, mas com redação anterior “abono se cancela”; corte posterior diz batidas contam e abono cobre apenas previsto descoberto. Não certificar aderência ao corte novo sem conferir esse comportamento. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:276`; `app/docs/BACKLOG.md:1521`)
- B26 e B28: testes presentes. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:319`; `:478`)
- B27: presente; B27b e B27c são pinos explícitos do defeito v2 ligado à ordem/deduplicação. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:340`; `:371`; `:423`)

**Demais pendências:**

- Propriedade de quatro leitores: o backlog declara só 2/4 pontas e deixa fechamento/export de fixture pendentes. O teste de export explica por que compara a função isolada: montar entrada no TXT exigia catálogo, fechamento e célula julgada em todo dia, e as tentativas caíram em `cadastro_zero`/`furo_espelho`. Isso demonstra o obstáculo, não a cobertura integral dos quatro leitores. (`app/docs/BACKLOG.md:1522`; `app/docs/BACKLOG.md:1621`; `app/folha/tests/test_porta_do_export.py:216`; `:224`)
- Art.71 §4 pós-2017: dimensionamento suprimido versus “INTEGRAL” continua pergunta nomeada; comentário v2 contém “indeniza INTEGRAL”. (`app/docs/BACKLOG.md:1621`; `app/ponto/motor_calculo_v2.py:68`)
- Art.58 §1: dimensionamento 5 por marcação versus 10 por ponta; C13 declara explicitamente a contradição, e F9/F9b são indicados como exercícios da lei. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o280_bateria_da_lei.py:605`; `:611`)
- Degrau HE >2h→100% continua explicitamente aberto no backlog; os casos só afirmam total e conservação das parcelas, sem certificar 120/restante como lei. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o280_bateria_da_lei.py:1954`; `:5742`)

Há divergência documental de contagem: o backlog diz “15 células B17–B33” e chama B33 verde; a tabela atual contém **17 IDs**, o selo exige 17 e B33 está `cadastro`. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:64`; `:136`; `:527`)

Nenhum resultado de execução desta bateria foi produzido nesta auditoria; estado verde no ambiente atual: [nao verificado]. [nao verificado]

## 06/11 10:40 · sessao f6ffc8ffebe3 · - · - · origem=Ronald (tmux)

Sítios de escrita reais, nesta cópia:

| Ramo | Sítio |
|---|---|
| Disputa existente: agrega contexto/motivos antes de gerar perguntas | `app/chamados/services/disputa_emissao.py:501` — `disputa_aberta.save(...)` |
| Disputa nova: abre/reutiliza pai | `app/chamados/services/disputa_emissao.py:504` — `ChamadoColaborador.abrir(...)` |
| Pai novo | `app/chamados/models.py:932` — `cls.objects.get_or_create(...)` |
| Disputa nova | `app/chamados/services/disputa_emissao.py:523` — `DisputaSupervisao.objects.create(...)` |
| Pergunta existente reapontada | `app/chamados/models.py:2474` — `ex.save(...)` |
| Pergunta nova | `app/chamados/models.py:2476` — `cls.objects.create(...)` |
| Descarimbo anterior à emissão | `app/chamados/services/relavra_via.py:231` → `app/chamados/models.py:2652` — `PerguntaDisputa.save(...)` |
| Pauta do limbo exportado | `app/pautas/services.py:180` — `Pauta.objects.create(...)` |
| Usuário sistema ausente, no ramo de pauta | `app/core/utils.py:18` — `User.objects.get_or_create(...)` |

Atenções para a sonda:

- Patch apenas em `ChamadoColaborador.objects.create` **não intercepta necessariamente** o `get_or_create`: este usa o `QuerySet.create`; patch de `ChamadoColaborador.save` cobre o INSERT real. (`app/chamados/models.py:932`; funcionamento interno do Django: [nao verificado])
- A disputa existente sempre recebe `merge_contexto=contexto or {}` neste caminho, salva o contexto e só depois chama a geração da pergunta. Interromper nesse `save` prova entrada no escritor, não que a pergunta passaria pelas guardas posteriores. (`app/chamados/services/disputa_emissao.py:479`, `:501`, `:603`, `:618`)
- Uma sentinela derivada diretamente de `BaseException` evita que os `except Exception` internos a engulam. Há captura desse tipo na emissão/reatação e na leitura da ata. (`app/chamados/services/disputa_emissao.py:411`, `:828`)
- Se houver candidata de descarimbo, o emissor pode tentar essa escrita **antes** da emissão. Não deve contabilizar o descarimbo como pergunta/chamado emitível. (`app/chamados/services/fabrica_da_celula.py:164`, `:166`, `:174`)
- `SELECT FOR UPDATE` ocorre antes das escritas na disputa e no descarimbo. Permissão do papel LEITOR para executá-lo: [nao verificado]. (`app/chamados/services/disputa_emissao.py:467`; `app/chamados/services/relavra_via.py:221`)
- O ramo real da pauta chama `autor_sistema` antes do `Pauta.objects.create`; logo também existe possível escrita na obtenção do usuário sistema. (`app/chamados/services/fabrica_da_celula.py:236`; `app/pautas/services.py:87`; `app/core/utils.py:18`)

Há outro emissor do chamado no cartório: `julgar_celula` instancia `detectar_ausencias.Command` e o corpo chama seu `_abrir_chamado`. A fábrica da pergunta roda depois da lavra, apenas nas células julgadas naquele ato. (`app/ponto/services/cartorio.py:1964`, `:1990`, `:1740`, `:1778`, `:981`, `:997`)

## 06/11 10:40 · sessao 963cba183f13 · - · - · origem=Ronald (tmux)

A contagem declarada é **8 `EM_CURSO` + 1 `ESPERA_RONALD` = 9 itens não `FEITO`**. O placar efetivo rebaixa `FEITO` sem prova, mas os itens `FEITO` lidos têm provas preenchidas; a regra está em `app/core/espelho_verdade.py:199-203`.

| Item | Estado/local | Número do R1 que atualiza; limite |
|---|---|---|
| E1-passivo-53 | `EM_CURSO`, `app/core/espelho_verdade.py:36-47` | Não mede. `escala/services/vigencia_impossivel.py:23-24` oferece `medir()` somente leitura e devolve vínculos/colabs/ativos/lavrados em `:48-53`. |
| E1-sem-previsao | `EM_CURSO`, `app/core/espelho_verdade.py:48-49` | Classe `esp_zero_e6_trabalho` (`app/ponto/management/commands/e6_oraculo.py:358-361`), proxy de zero no espelho com trabalho no oráculo. Não certifica previsão válida; a medição específica é `escala/services/previsto_cego.py:114-115`, que usa `janela_atual` em `:138` e cobra ativo com par e nenhum previsto (`:142-174`). |
| E3-turno-aberto | `EM_CURSO`, `app/core/espelho_verdade.py:81-82` | `dia_batida_impar`, batem/divergem e `impar_FORA_da_faixa` (`e6_oraculo.py:347-349`, `:402-412`). São paridade/faixa do oráculo, não censo exato de turnos abertos. |
| E4-rotulo-do-desenho | `EM_CURSO`, `app/core/espelho_verdade.py:130-131` | R1 não mede; verificar leitor da pílula/selo de UI. **Prova do placar envelhecida:** `app/templates/escala/tipos_lista.html:55` já usa `{{ t\|desenho_do_turno }}` e cita O74 e seu selo. |
| E5-le-da-celula | `EM_CURSO`, `app/core/espelho_verdade.py:138-139` | R1 não mede proveniência do fechamento nem conclusão da troca de leitores; a pendência declarada é “falta status encerrado” (`:139`). |
| E6-sistema-bate | `EM_CURSO`, `app/core/espelho_verdade.py:168-169` | `BATE`/dias comparados (`e6_oraculo.py:398-399`) e ESTRUTURA dias/horas/colabs (`:422-423`). |
| E6-exportavel | `EM_CURSO`, `app/core/espelho_verdade.py:172-173` | `colabs com divergencia` (`e6_oraculo.py:429`) e colabs por dono (`:423`). R1 10/11 não mede elegibilidade real do export nem a 09 citada no título do item. |
| E6-piso-185 | `EM_CURSO`, `app/core/espelho_verdade.py:178-179` | `SOMA` donos/classes (`e6_oraculo.py:424-427`) e ESTRUTURA dias/horas/colabs. **Não fecha causa:** o próprio placar exige “falta causa nomeada nos ... ESTRUTURA” (`espelho_verdade.py:179`). |
| E3-separados-30 | `ESPERA_RONALD`, `app/core/espelho_verdade.py:83-84` | Não mede: é universo separado da 09 e situação de migração/aplicação; o placar declara a 09 sob responsabilidade externa (`:84`). |

O backlog pede explicitamente R1 para **cinco itens**: E1-sem-previsao, E3-turno-aberto, E6-sistema-bate, E6-exportavel e E6-piso-185, e pede confirmar E6-batida-impar; este último já é `FEITO` (`app/docs/BACKLOG.md:1478`; `app/core/espelho_verdade.py:176-177`). A confirmação usa as linhas de ímpares e paridade (`app/ponto/management/commands/e6_oraculo.py:402-412`).

`app/docs/ESTRUTURAL.md:34-42` estabelece o dono único, o juiz `dono_da_divergencia`, o destino CADASTRO/BATIDA no admin e a separação do placar principal R1–R6 e ESPELHO-VERDADE. `app/core/placar_estrutural.py:115-138` declara a fonte `logs/e6_cauda2c/r1_dono_*.txt`, cadência, dias/horas/colabs e tabela impressa pelo próprio comando. `:105-113` exige a soma obtida no comando, sem sonda paralela.

## 06/11 10:40 · sessao 27a8fa81a908 · - · - · origem=Ronald (tmux)

- **V3-MOTOR-FERIADO-HE100-NOTURNA** está na fila e exige `!`; o registro relata HE100 noturna zero em feriado com HE100 e noturnas positivas. Isso é relato do backlog, sem reexecução nesta auditoria. (`app/docs/BACKLOG.md:937`)
  - No `MotorTurnoPartido`, as HE noturnas são zeradas antes do cálculo mensal; o ramo de dobra preenche HE100 e sai por `continue`, enquanto `_aplicar_he_noturna` aparece depois. (`app/ponto/motor_calculo_v2.py:2937`, `:3092`, `:3101`, `:3147`)
  - No `MotorComercial`, ocorre a mesma estrutura: zera os splits, preenche a dobra e sai antes da chamada de HE noturna. (`app/ponto/motor_calculo_v2.py:3323`, `:3337`, `:3352`, `:3399`)
  - Qual desses ramos explica cada um dos dez colaboradores registrados: **[nao verificado]**.

- **V3-NOTURNO-RECORTE-LONGO** espera `!` e está explicitamente fora do escopo V3-P5. (`app/docs/BACKLOG.md:939`)
  - `minutos_noturnos` devolve zero acima de 26 horas. (`app/ponto/motor_calculo_v2.py:93`)
  - Com prorrogação, estende `s1` até `fim` e soma separadamente `s2` de 22–24; portanto pode contar a sobreposição duas vezes. (`app/ponto/motor_calculo_v2.py:110`, `:112`, `:116`)
  - `MotorPlantao.calcular_periodo` delega a `Motor12x36`, que escreve noturnas pela autoridade que chama `minutos_noturnos`. (`app/ponto/motor_calculo_v2.py:3510`, `:2792`, `:983`)

- **Rubricas diagnósticas para a lista:** `horas_extras_100_noturna`, contextualizada por `horas_extras_100`, `horas_extras_100_feriado` e `horas_noturnas`, pode localizar a assinatura conhecida do feriado. (`app/docs/BACKLOG.md:937`; nomes dos campos: `app/ponto/services/dia_pago.py:35`)
  - `horas_noturnas`, `horas_extras_50_noturna` e `horas_extras_100_noturna` podem expor efeitos do recorte longo; HE50/100 também podem mudar quando a régua legal inclui o ganho noturno na base do excedente. (`app/ponto/motor_calculo_v2.py:983`, `:441`, `:3123`)
  - Há **cegueira compartilhada**: a v3 também chama `minutos_noturnos`; igualdade no diff não elimina o defeito comum. (`app/ponto/calculador/regras.py:85`; `app/ponto/calculador/periodo.py:510`)
  - Divergências fora dessas assinaturas são candidatos a investigação, não comprovação de bug novo. L-110 define a comparação com o código velho como **impacto**, nunca validação. (`app/docs/LEIS.md:136`)

## 06/11 10:40 · sessao fb0ce3409b50 · - · - · origem=Ronald (tmux)

Lei geral: `CLAUDE.md:166–170` faz célula/ata a lâmpada, evento o emissor, consumidor leitor. `app/docs/CORTES.md:263` explicita “lampada acesa no marco = fio vivo obrigatorio”; não transforma toda recusa em violação, porque há exceções escritas posteriores.

Guardas de `motivos_a_perguntar`, em ordem:

1. `dia_sem_trabalho(cel)` e colaborador inativo: `app/chamados/services/fabrica_da_celula.py:91`, retorno `:92`. O juiz é `app/chamados/juizes.py:413`, predicado `:426–428`. Dia sem trabalho tem cobertura expressa em `app/docs/CORTES.md:416`: `trabalha=False` ou tipo lavrado diferente de trabalho; `None` cobra. Não viola 4a quando a própria lâmpada diz ausência de trabalho. Inativo tem piso por domínio RH documentado/testado em `app/chamados/tests/test_selo_f3_celula_soberana.py:19–25`, `:86–103`; não encontrei corte nomeado nesta leitura que autorize especificamente **todo** status diferente de `ativo`, então ampliação além de desligado [nao verificado].

2. Dia futuro: fábrica `:93–94`. Teto temporal escrito em `CLAUDE.md:185`; `CORTES.md:263` exclui hoje por teto na formulação histórica. A fábrica atual trabalha por instante e inclui hoje quando marco venceu: docstring fábrica `:8–11` e autoridade `vencidos_no_instante :15–31`. Recusa de marco ainda não vencido é coberta; não há violação demonstrada.

3. Veredito preenchido fora de `VEREDITOS_ACUSAM`: fábrica `:99–100`. Obedece à lâmpada/consumidor em `CLAUDE.md:167–170`; não há obrigação de perguntar célula absolvida.

4. `parcial_so_lista`: fábrica `:101–104`; juiz real `app/ponto/services/cartorio.py:370–400`. Duas hipóteses documentadas `:378–387`: parcial sem período sempre lista do admin; parcial com período sem fato novo desde julgamento também lista. Corte explícito `app/docs/CORTES.md:319`, complemento da resposta `nao_sei :334`; aval literal “parcial sem periodo ... nunca chamado” está no código `cartorio.py:381–382`. Portanto não imputar quebra geral da 4a a esta recusa: é exceção escrita. Causa dos 200 por ela [nao verificado].

5. Construção dos motivos e dedup: fábrica `:118–126`. Dedup elimina todo motivo com pergunta histórica que não seja `VIA_DUPLICATA`, sem testar aqui se viva. Se essa for a causa medida de célula acusadora sem fio vivo, a lei que confronta é `CLAUDE.md:168–170` + `CORTES.md:263`; não há nesta leitura lei que autorize substituir **fio vivo** por mera existência histórica.

Guardas internas de `motivos_pela_ata`:

- Vencidos vazio: fábrica `:45–46`, coberto pelo teto `CLAUDE.md:185`.
- `_ata_do_dia` nega: fábrica `:49–51`. Autoridade real `disputa_emissao.py:818–844`; nega somente sem célula com escala vigente (`:833–843`). Docstring `:819–823` exige lavra antes de questionário. Se lâmpada acusadora existente e emissor negar por leitura inconsistente, confronto com `CLAUDE.md:167–170`; caso concreto [nao verificado].
- Sem ata e teto por marcos: fábrica `:52–54`; aguarda porque não há marco para comparar; lei específica além de teto/lâmpada [nao verificado].
- Sem ata e sem marcos passados: chama `classificar_falta` e pode obter nenhum motivo, fábrica `:55–57`. Se isso cala célula/ata que acusa, é consumidor rejulgando contra `CLAUDE.md:170`; ocorrência [nao verificado].
- Filtra faltantes por vencimento, data, mapeamento e unicidade: fábrica `:59–65`. Vencimento tem teto; identidade/data preservam marco do dia. Mapeamento ausente que silencie marco acusador confronta `CORTES.md:263`, ocorrência [nao verificado].

Depois dos motivos, `perguntar_pela_celula`:

- Limbo exportado vai à pauta em vez de pergunta: fábrica `:171–173`; juiz `limbo_vai_a_pauta :180–188`; pauta `:225–242`. Exceção expressa comentada fábrica `:167–170` e aval em `app/docs/PROMPTS.md:1719`. Não confundir com fronteira geral exportada, abolida por `app/docs/CORTES.md:219` e `app/docs/LEIS.md:91`.
- Pauta já existente impede duplicata: fábrica `:208–222`, `:234–236`; idempotência escrita em `CLAUDE.md:175` e fábrica `:228–230`.

Atenção ao contrato de erros: `_ata_do_dia` **engole** erro real de `_marcos_faltantes` em `app/chamados/services/disputa_emissao.py:828–832`, registrando por `core.observ.registrar_engolido` e retornando `(True, None)`. Sonda deve observar essa chamada/registrador por mock, contar o erro e marcar incompleto; retorno normal do emissor não garante ausência de erro.

## 06/11 10:35 · sessao 63c4f7796b6e · PEP-REENTRADA · verificador · origem=Code

Parecer estático: execução dos testes, dos mutantes e comportamento em produção `[nao verificado]`. [nao verificado]

**1. P1–P4: cura e mutantes sobreviventes**

| Item | Parecer |
|---|---|
| **P1** | **Cura implementada para observar entrada no serviço durante o voo:** o espia registra `pergunta_em_voo(p.pk)` e exige pelo menos um `True`. Ele não identifica exclusivamente o receiver que originou a chamada. `app/chamados/tests/test_pep_reentrada.py:90`, `app/chamados/tests/test_pep_reentrada.py:97`, `app/chamados/tests/test_pep_reentrada.py:99`. |
| **P2** | **Curado quanto ao estado persistido do trio nos dois alvos**, mas **parcial quanto à exigência “dentro do voo”**: sem credor verifica depois de sair do `with`; S144 verifica ainda dentro. `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/tests/test_pep_reentrada.py:256`, `app/chamados/tests/test_pep_reentrada.py:259`, `app/chamados/tests/test_pep_reentrada.py:274`, `app/chamados/tests/test_pep_reentrada.py:278`. |
| **P3** | **O mutante original passa a ser distinguido:** ambas sem destino atravessariam `if q.chamado_destino_id: continue`, inclusive a P protegida; a exigência de P sem carimbo o reprovaria. A alteração, porém, muda o ingresso da disputa em `_cands`. `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/tests/test_pep_reentrada.py:259`, `app/chamados/services/disputa_emissao.py:1652`. |
| **P4** | **Curado:** C7 e C9 agora coincidem com registro e mapa. `app/docs/JUIZES.md:54`, `app/docs/JUIZES.md:55`, `app/core/juizes.py:267`, `app/core/juizes.py:269`, `app/docs/MAPA-JUIZES.md:97`, `app/docs/MAPA-JUIZES.md:105`. |

Mutantes de produção que os testes curativos ainda não distinguem — sobrevivência inferida pela leitura; execução `[nao verificado]`: [nao verificado]

- **P1:** substituir o corpo do resolver por retorno de contadores zerados. O espia continuaria registrando entrada em voo; o próprio materializador fecharia o destino e produziria seu carimbo. O teste exige entrada, destino fechado, batida e um `carimbar` bem-sucedido pela via da declaração, sem exigir efeito exclusivo do serviço observado. `app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:101`, `app/chamados/tests/test_pep_reentrada.py:103`, `app/chamados/tests/test_pep_reentrada.py:105`, `app/chamados/services/materializacao.py:949`, `app/chamados/services/materializacao.py:1251`.
- **P2:** nas guardas de voo dos dois alvos, executar `descarimbar()` antes de `continue`. Haveria escrita indevida do trio, mas os valores continuariam exatamente os vazios esperados. Portanto, conferir estado intacto não prova ausência de escritor. `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`, `app/chamados/models.py:2646`, `app/chamados/models.py:2652`, `app/chamados/tests/test_pep_reentrada.py:260`, `app/chamados/tests/test_pep_reentrada.py:279`.
- **P3:** trocar a guarda por `if pergunta_em_voo(q.pk) and not q.chamado_destino_id: continue`. O novo caso permanece protegido, mas perde-se a proteção da P em voo **com destino** no laço sem credor. O laço admite destinos encerrados; só um destino vivo barra a disputa. `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/disputa_emissao.py:1671`.

**2. O espia de P1 está no nome correto?**

**Sim.** O receiver faz import local de `chamados.models.resolver_perguntas_por_chamado_resolvido` e chama esse nome; é precisamente o atributo substituído pelo teste. Não há outro ramo do próprio receiver que chame o resolver por um nome diferente. `app/chamados/signals.py:89`, `app/chamados/signals.py:92`, `app/chamados/tests/test_pep_reentrada.py:97`.

Há outros **chamadores do serviço**, também pelo nome observado: `_retratar`, reatamento de órfã e fechamento de beco. Eles não escapam do espia; podem, contudo, tornar ambígua a atribuição “foi o receiver S143”. Quais efetivamente chamam durante esse fixture, inclusive com S143 desligado, `[nao verificado]`. `app/chamados/reconciliador.py:508`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1528`, `app/chamados/reconciliador.py:1631`, `app/chamados/services/becos.py:40`, `app/chamados/services/becos.py:49`.

O registro ocorre **antes** de executar o serviço, e o receiver engole exceções: o `True` comprova entrada, não conclusão bem-sucedida. `app/chamados/tests/test_pep_reentrada.py:91`, `app/chamados/tests/test_pep_reentrada.py:92`, `app/chamados/signals.py:93`.

**3. P3 mantém o caminho anterior?**

**Mantém o laço, mas muda o caminho de entrada.** Antes, a P destinada também colocava a disputa no conjunto derivado de `perguntas`; agora ambas estão sem destino, o filtro inicial fica vazio para elas e a disputa entra pela consulta das abertas do colaborador. `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/services/disputa_emissao.py:1572`, `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1653`.

Pai resolvido e validações nulas permitem alcançar `_abertas`; o controle da irmã exige que alguma escrita realmente aconteça nesse laço. Portanto, a nova configuração não torna a guarda inatingível. `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:251`, `app/chamados/tests/test_pep_reentrada.py:258`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1665`.

O limite de realismo continua relevante: o fixture mantém texto, apaga resposta/validação, declara voo manualmente e força C6 a “morta”. A porta normal seleciona perguntas com resposta e validação gravadas; com C6 real, o próprio docstring declara que a irmã não receberia o carimbo. Isso prova a guarda isolada e perde cobertura do ingresso anterior por destino; não demonstra a cadeia real nesse estado. `app/chamados/tests/test_pep_reentrada.py:238`, `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/services/materializacao.py:1343`.

**4. P4: correspondência documental**

| Autoridade | JUIZES | Registro | MAPA |
|---|---|---|---|
| C7: `DisputaSupervisao.pode_fechar` | `app/docs/JUIZES.md:54` | `app/core/juizes.py:267` | `app/docs/MAPA-JUIZES.md:97` |
| C9: `catalogo/motor.py::prazo_estourou` | `app/docs/JUIZES.md:55` | `app/core/juizes.py:269` | `app/docs/MAPA-JUIZES.md:105` |

**5. Classes de reprovação no delta**

| Classe | Resultado da leitura |
|---|---|
| **Segundo juiz** | O voo é pertencimento por PK condicionado a atomic aberto; não recalcula vitalidade nem fechamento. C6 continua na porta do carimbo e C7 na cauda. `app/chamados/services/materializacao.py:1129`, `app/chamados/models.py:2596`, `app/chamados/services/disputa_emissao.py:1712`. |
| **Segundo escritor fora da porta** | As guardas novas apenas pulam os candidatos. Entretanto, o espia principal conta somente chamadas de `carimbar` que retornam `True`: uma escrita direta seguida de sobrescrita pelo materializador pode escapar dessa contagem. `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/materializacao.py:1399`, `app/chamados/tests/test_pep_reentrada.py:69`, `app/chamados/tests/test_pep_reentrada.py:105`. |
| **Relógio fora da borda** | O sinal novo não usa relógio; o fixture congela o instante. `app/chamados/services/materializacao.py:1115`, `app/chamados/services/materializacao.py:1129`, `app/chamados/tests/test_pep_reentrada.py:35`. |
| **Derivação paralela** | O sinal guarda PKs; os carimbos continuam delegando aos juízes existentes. `app/chamados/services/materializacao.py:1120`, `app/chamados/services/materializacao.py:1207`, `app/chamados/models.py:2596`. |
| **Selo sem mordida** | Há controles positivos, mas persistem os limites de origem do espia, estado versus escrita e mudança de caminho de P3 descritos acima. `app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:258`, `app/chamados/tests/test_pep_reentrada.py:281`. |
| **Ordem de trava** | A mudança do bloco acrescenta o contexto depois do atomic; o contexto só manipula `ContextVar`. Validação concorrente da ordem de travas `[nao verificado]`. `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1120`. |
| **Pouso antes do pré-requisito** | O atomic entra antes do voo; declarar voo fora de atomic levanta erro. O carimbo normal permanece depois do plantio e do juiz do lastro. `app/chamados/services/materializacao.py:1118`, `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1251`. |
| **Estado de módulo que sobrevive** | Existe `ContextVar` de módulo, mas o valor anterior é restaurado por token em `finally`; há testes de rollback, exceção e aninhamento. Passagem desses testes `[nao verificado]`. `app/chamados/services/materializacao.py:1106`, `app/chamados/services/materializacao.py:1124`, `app/chamados/tests/test_pep_reentrada.py:150`, `app/chamados/tests/test_pep_reentrada.py:167`, `app/chamados/tests/test_pep_reentrada.py:179`. |

**6. Quem escreve e quem lê o trio dentro do voo?**

**Nos caminhos atuais examinados, não identifiquei escritor da P além do materializador. Essa garantia está nos caminhos, não nas portas genéricas.** `app/chamados/services/materializacao.py:1251`, `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`.

| Papel | Caminho |
|---|---|
| **Escritor normal** | `_materializar_uma → carimbar → _escrever_carimbo → save`, atribuindo o trio junto. `app/chamados/services/materializacao.py:1251`, `app/chamados/models.py:2556`, `app/chamados/models.py:2609`. |
| **Escritor alternativo do próprio materializador** | A recusa por paridade usa `carimbar_identidade` e retorna `None`, impedindo o carimbo normal posterior. `app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:1201`. |
| **S143/S144** | Os dois laços de S143 pulam a P; sua cauda não fecha disputa com P em voo; S144 também pula antes de carimbar. `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`, `app/chamados/services/materializacao.py:1399`. |
| **Reconciliador** | A batida o aciona sincronamente. A absolvição por célula pode carimbar, mas exige validação nula, excluindo a P selecionada pelos chamadores atuais; a lavra sem turno exige ausência de texto. `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1345`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/reconciliador.py:364`. |
| **Dedup/descarimbo** | Migração considera perguntas vivas, enquanto C6 elimina imediatamente a validada; ressuscitação exige carimbo existente e texto vazio. `app/chamados/services/fio.py:935`, `app/chamados/juizes.py:1136`, `app/chamados/models.py:1434`. |
| **Leitores** | A porta fotografa o trio anterior; C6 e lastro leem materialização/via; S143, S144 e irmãs sem consumo usam materialização nos filtros. O autor é preservado no snapshot. `app/chamados/models.py:2583`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:647`, `app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/materializacao.py:1393`, `app/chamados/juizes.py:1748`. |

A recusa por colisão no `finally` pode escrever identidade **depois da saída do voo**; ela não constitui outro escritor dentro desse bloco. Já `carimbar_identidade` e `descarimbar` continuam sem consultar o voo: ampliar os chamadores para estados como o fixture de P3 exigiria nova análise de alcançabilidade. `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1271`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`, `app/chamados/tests/test_pep_reentrada.py:234`.

## 06/11 10:33 · sessao 083c8ea8b2b0 · - · - · origem=Ronald (tmux)

No bloco atual, não encontrei escritor do trio da P além do materializador; a garantia depende dos caminhos atuais, pois as portas genéricas não consultam o voo (`app/chamados/services/materializacao.py:1194`, `app/chamados/models.py:2562`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`).

**Quem escreve:**

- O carimbo normal sai de `_materializar_uma → p.carimbar`, após o juiz do lastro; a porta atribui os três campos e grava (`app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1251`, `app/chamados/models.py:2556`, `app/chamados/models.py:2609`).
- O próprio ramo hora pode escrever identidade por paridade e retornar imediatamente, impedindo o carimbo normal; continua sendo o materializador (`app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:1201`).
- A recusa `COLISAO` pode escrever identidade no `finally`, mas esse `finally` roda após a saída de `materializacao_em_voo`; portanto não é outro escritor **dentro** do voo (`app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1271`).

**Outros candidatos e por que não escrevem a P atual:**

- S143 pula a P nos dois laços e bloqueia o fecho da disputa com qualquer P em voo (`app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`).
- S144 pula antes do juiz e do carimbo (`app/chamados/services/materializacao.py:1399`, `app/chamados/services/materializacao.py:1405`).
- A batida chama sincronamente `reconciliar_chamados_colab`; sua absolvição por célula carimba sem consultar voo, mas exige `validada_em IS NULL`. A porta normal seleciona validação não nula; a validação individual grava antes de materializar; o passivo também seleciona validação não nula (`app/ponto/signals.py:206`, `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1343`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:424`, `app/chamados/services/vigia_carimbo.py:98`).
- `lavrar_sem_turno_aberto` exige ausência de texto, incompatível com hora que chega ao plantio e com `data_hora` parseável (`app/chamados/reconciliador.py:364`, `app/chamados/reconciliador.py:405`, `app/chamados/services/materializacao.py:804`, `app/chamados/services/materializacao.py:970`).
- Dedup só processa perguntas vivas; o C6 dá por morta imediatamente a P validada (`app/chamados/services/fio.py:931`, `app/chamados/services/fio.py:935`, `app/chamados/juizes.py:1136`).
- Ressuscitação/descarimbo filtra pergunta já materializada e sem texto; a P hora em voo tem texto (`app/chamados/models.py:1434`, `app/chamados/models.py:1437`, `app/chamados/models.py:1504`, `app/chamados/services/materializacao.py:804`).
- Re-lavra de via não está ligada ao sinal da batida; `_gravar_estado` re-lavra somente o veredito, não o trio (`app/ponto/signals.py:94`, `app/chamados/models.py:291`, `app/chamados/veredito_lavrado.py:227`).

**Quem lê:**

- `carimbar` fotografa o trio anterior e escreve pela porta única; C6 e juiz do lastro leem materialização/via; a lavra delega ao juiz do lastro (`app/chamados/models.py:2583`, `app/chamados/models.py:2586`, `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1207`, `app/chamados/juizes.py:647`, `app/chamados/juizes.py:673`, `app/chamados/veredito_lavrado.py:219`).
- Os universos de S143, S144, irmãs sem consumo e reconciliador filtram `materializada_em`; o autor é preservado no snapshot e atribuído pela porta, sem participar desses predicados (`app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/disputa_emissao.py:1666`, `app/chamados/services/materializacao.py:1393`, `app/chamados/juizes.py:1748`, `app/chamados/reconciliador.py:619`, `app/chamados/models.py:2583`, `app/chamados/models.py:2559`).

O limite estrutural merece constar: uma chamada nova de `_materializar_uma` com validação nula poderia entrar no universo do reconciliador, porque ele não consulta voo; os chamadores atuais evitam isso pelos filtros acima, e o próprio teste P3 declara esse limite (`app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1132`, `app/chamados/tests/test_pep_reentrada.py:234`).

## 06/11 10:33 · sessao d90ff57f7b2e · - · - · origem=Ronald (tmux)

- **Item 2:** o patch de P1 entra no nome correto: o teste troca `chamados.models.resolver_perguntas_por_chamado_resolvido` em `app/chamados/tests/test_pep_reentrada.py:97`; o receiver faz import local desse mesmo nome e o chama em `app/chamados/signals.py:89` e `app/chamados/signals.py:92`; o reexport vem de `app/chamados/models.py:3281`. Não há outro ramo do próprio receiver que execute o resolver sem passar por esse alias (`app/chamados/signals.py:81`, `app/chamados/signals.py:88`).

- **Limite de P1:** o espia comprova entrada no serviço durante o voo, mas não identifica qual chamador entrou nem comprova conclusão do serviço: só registra `pergunta_em_voo(p.pk)` antes de chamar o original (`app/chamados/tests/test_pep_reentrada.py:90`); o receiver engole exceções (`app/chamados/signals.py:93`). Há chamadas diretas pelo mesmo alias, também vistas pelo espia, em `app/chamados/reconciliador.py:508`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1528`, `app/chamados/reconciliador.py:1631`, `app/chamados/services/becos.py:40` e `app/chamados/services/becos.py:49`. Quais efetivamente acontecem no fixture: `[nao verificado]`.

- **Mutante sobrevivente de P1, no teste que o cura:** substituir o serviço por retorno `{'perguntas_resolvidas': 0, 'disputas_fechadas': 0}` preservaria a chamada observada em voo e deixaria o materializador produzir seu carimbo único; o método exige apenas ocorrência de `True`, destino fechado, batida presente e uma gravação com `VIA_DECLARACAO` (`app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:101`, `app/chamados/tests/test_pep_reentrada.py:103`, `app/chamados/tests/test_pep_reentrada.py:105`; carimbo do materializador em `app/chamados/services/materializacao.py:1251`). Resultado executado do mutante: `[nao verificado]`.

- **Item 3:** P3 continua atingindo o mesmo laço-alvo, mas **não pelo mesmo ingresso em `_cands`**: antes P apontava para `destino`; agora remove a FK em `app/chamados/tests/test_pep_reentrada.py:250`. Como a irmã também nasce sem destino (`app/chamados/tests/test_pep_reentrada.py:244`), o filtro `chamado_destino=chamado` fica sem essas perguntas (`app/chamados/services/disputa_emissao.py:1572`). A disputa entra pela busca de todas as abertas do colaborador (`app/chamados/services/disputa_emissao.py:1653`), enquanto antes também entrava pelo conjunto derivado de `perguntas` (`app/chamados/services/disputa_emissao.py:1652`). Pai resolvido e validações nulas satisfazem os filtros do laço (`app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:251`; filtros em `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1665`).

- **P3 mata o mutante original:** trocar a guarda por `if q.chamado_destino_id: continue` deixaria ambas passarem, inclusive P em voo, contrariando a exigência de P sem carimbo (`app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/tests/test_pep_reentrada.py:259`; ponto de mutação em `app/chamados/services/disputa_emissao.py:1671`). Execução: `[nao verificado]`.

- **Mutante sobrevivente de P3:** `if pergunta_em_voo(q.pk) or q.chamado_destino_id: continue` mantém o teste verde porque ambas estão sem destino, mas passa a pular indevidamente irmãs fora de voo com destino morto; esse universo é aceito pelo laço de produção, que só exclui a disputa quando existe destino **vivo** (`app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/disputa_emissao.py:1671`; fixture em `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`). Execução: `[nao verificado]`.

- **Realismo de P3:** o limite de C6 mockado está agora declarado explicitamente (`app/chamados/tests/test_pep_reentrada.py:238`); a modificação adicional perde cobertura do ingresso por pergunta com destino, mas não impede atingir a guarda do laço sem credor (`app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1671`). O caso continua sintético: remove carimbos de resposta/validação mantendo o texto e declara voo manualmente (`app/chamados/tests/test_pep_reentrada.py:245`, `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:254`); portanto sua evidência é da guarda, não da cadeia real de materialização (`app/chamados/tests/test_pep_reentrada.py:239`).

_2224 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
