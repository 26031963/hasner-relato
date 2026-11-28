# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 21:13** · publicado em **27/11/2026 21:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 21:13 · sessao 682dadce1e1b · FIM-DE-SEMANA-JA · leitura · origem=Code

A S1 informa **38 chamados distintos e 10 colabs**. A S4 reutiliza `c2` do anexo e lista todos os pares do terceiro retorno. Um par com **Conferir data** continua visível para auditoria, com `sem_acao_no_recorte=nao`, e entra no subtotal de falsos positivos. O subtotal de chamados conta os distintos com pelo menos um par marcado. Um mesmo chamado pode ter outro par ainda sem ação. [hasner.txt:53, citado no anexo S2] [app/chamados/services/fio.py:610] [app/templates/chamados/partials/modal_fio.html:173] [app/chamados/views.py:1308]

A sonda lê o anexo original de `sonda_S1.py` ou do texto em `S4_S1_FONTE`. Carrega as definições de C2 em memória, pois o corpo de topo do anexo dispara a coleta completa. Os IDs dos pares, os novos totais e a execução na Sombra ficam [nao verificado].

```python
import sys

sys.dont_write_bytecode = True

import ast
import json
import logging
import os
import tokenize
from collections import defaultdict
from functools import lru_cache
from html.parser import HTMLParser

from django.conf import settings
from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context

if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from django.contrib.auth.models import User
from django.template import Context, engines

from chamados.catalogo.motor import VIVOS, decidir
from chamados.juizes import competencia_do_chamado, pergunta_viva
from chamados.models import ChamadoColaborador, DisputaSupervisao
from chamados.services.fio import (
    chamado_que_lista,
    contexto_parcial_fio,
    data_do_fato_de,
    espera_o_texto_do_colaborador,
)
from chamados.services.materializacao import estado_aplicacao_pergunta
from chamados.services.tarefa_do_dia import exigencia_das_perguntas
from chamados.services.verbos import particionar_verbos
from chamados.utils import departamento_do_chamado, pode_agir_chamado
from colaboradores.models import Empresa
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.supra_juiz import perguntas_que_seguram_csf

EMPRESA_ID = 2
MES, ANO = 11, 2026
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
ESPERADO = 38
NV = "[nao verificado]"

FONTE_DONO = "app/ponto/management/commands/e6_oraculo.py:182,53"
OBRA_A3 = "E4C-F5-82-FALTA"
FONTE_A3 = "app/docs/BACKLOG.md:2059"
ESCRITOR_VALIDAR = (
    "app/chamados/services/validacao.py:_carimbar_validacao:164"
)
ESCRITOR_RESPOSTA = (
    "app/chamados/services/disputa_emissao.py:responder_pergunta_disputa_core:1471"
)
PORTAS = {
    "conferir_data": (
        ("app/chamados/views.py:conferir_data_pergunta:1288",),
        "app/chamados/services/validacao.py:conferir_data_do_fato:1031",
    ),
    "validar": (
        (
            "app/chamados/services/fio.py:processar_post_fio:721",
            "app/chamados/views.py:detalhe_chamado_local:483",
            "app/chamados/views_cobrar.py:validar_inline:145",
            "app/chamados/views_cobrar.py:validar_lote:250",
        ),
        ESCRITOR_VALIDAR,
    ),
    "recusar_pergunta": (
        (
            "app/chamados/services/fio.py:processar_post_fio:776",
            "app/chamados/views.py:detalhe_chamado_local:506",
        ),
        "app/chamados/services/validacao.py:recusar_pergunta:902",
    ),
    "corrigir_resposta": (
        ("app/chamados/services/acoes_disputa.py:acao_corrigir_resposta:343",),
        ESCRITOR_RESPOSTA,
    ),
    "responder": (
        (
            "app/api/views.py:api_disputa_responder:2609",
            "app/chamados/views.py:responder_pergunta_disputa:1229",
        ),
        ESCRITOR_RESPOSTA,
    ),
    "materializar": (
        ("app/chamados/services/materializacao.py:materializar_perguntas_validadas_da_disputa:2238",),
        "app/chamados/services/materializacao.py:_materializar_uma:1955",
    ),
    "rejulgar": (
        ("app/ponto/services/cartorio.py:julgar_celula:2078",),
        "app/ponto/portas/celula.py:lavrar_veredito:1069",
    ),
    "decidir_ausencia": (
        (
            "app/ponto/views.py:aprovar_ausencia:2770",
            "app/ponto/views.py:rejeitar_ausencia:2854",
        ),
        "app/ponto/services/ausencia.py:_decidir_no_ato:1084",
    ),
    "entregar_documento": (
        (
            "app/api/views.py:api_ausencia_documento:3085",
            "app/ponto/views.py:anexar_documento_ausencia:2165",
        ),
        "app/ponto/services/ausencia.py:anexar_documento:1005",
    ),
}


def carregar_s1():
    # Use apenas o fonte original do anexo. Seus imports e as definicoes
    # abaixo bastam para chamar a propria c2 sem disparar medir() no topo.
    fonte = os.environ.get("S4_S1_FONTE")
    if fonte is None:
        with tokenize.open(os.environ.get("S4_S1_ARQUIVO", "sonda_S1.py")) as arquivo:
            fonte = arquivo.read()
    nome = "sonda_S1.py"
    arvore = ast.parse(fonte, filename=nome)
    nomes = {
        "Linha", "valor", "ids", "dia_da_pergunta", "CartoesHTML", "templates",
        "chamado_de", "validar_em_leitura", "PortasGestor", "portas_colab",
        "c2", "ator",
    }
    nos = []
    for no in arvore.body:
        if isinstance(no, (ast.Import, ast.ImportFrom)):
            nos.append(no)
        elif isinstance(no, (ast.FunctionDef, ast.ClassDef)) and no.name in nomes:
            nos.append(no)
        elif isinstance(no, ast.Assign) and any(
            isinstance(alvo, ast.Name) and alvo.id == "VERBOS_DA_PERGUNTA"
            for alvo in no.targets
        ):
            nos.append(no)
    s1 = dict(__name__=__name__, EMPRESA_ID=EMPRESA_ID, AGORA=AGORA, HOJE=HOJE, NV=NV)
    exec(compile(ast.Module(body=nos, type_ignores=[]), nome, "exec"), s1)
    return s1


class FormulariosConferirData(HTMLParser):
    def __init__(self, html):
        super().__init__()
        self.cartoes_da_pergunta = defaultdict(set)
        self.feed(html)
        self.close()

    def handle_starttag(self, tag, attrs):
        a = dict(attrs)
        if tag == "form" and "data-s4-pergunta" in a:
            pergunta = int(a["data-s4-pergunta"])
            self.cartoes_da_pergunta[pergunta].add(int(a["data-s4-chamado"]))


@lru_cache(maxsize=1)
def template_conferir_data():
    # Marca em memoria apenas o formulario do POST real.
    # app/templates/chamados/partials/modal_fio.html:163-178
    engine = engines["django"].engine
    fonte = engine.get_template("chamados/partials/modal_fio.html").source
    alvo = """<form hx-post="{% url 'chamados:conferir_data_pergunta' perg.pk %}" """
    if fonte.count(alvo) != 1:
        raise SystemExit("PAREI: formulario Conferir data mudou")
    marca = 'data-s4-pergunta="{{ perg.pk }}" data-s4-chamado="{{ chamado.pk }}" '
    return engine.from_string(fonte.replace(alvo, alvo + marca, 1))


@lru_cache(maxsize=None)
def foto_do_fechamento(ch):
    # Mesma decisao seca que reconciliar, fora de atomic no molde da S3.
    # app/chamados/models.py:661-716,761-781
    fecha = ch.reconciliaria()
    motor = decidir(ch.status_local, "RECONCILIAR")
    premissa_ok = (
        ch._gate_premissa_do_fechamento(registrar=False) if motor.permitido else None
    )
    csf_ok = ch._gate_conversa_do_pai(registrar=False)
    csf = perguntas_que_seguram_csf(ch)
    vivas, reatas, pergunta_ok = [], None, None
    if motor.permitido:
        plano = ch._plano_pergunta_viva(motor.novo_estado)
        vivas, reatas = plano
        pergunta_ok = ch._gate_pergunta_viva(motor.novo_estado, registrar=False, plano=plano)

    # A ordem rotula a recusa da leitura acima. Cada guarda responde por si.
    motivo = None
    if not fecha:
        if not motor.permitido:
            motivo = "motor_RECONCILIAR"
        elif not premissa_ok:
            motivo = "dia_furado_para_a_folha"
        elif not csf_ok:
            motivo = "conversa_exige_decisao_humana"
        elif not pergunta_ok:
            motivo = "pergunta_viva_na_competencia_aberta"
        else:
            motivo = NV

    return {
        "fecha": fecha,
        "motivo": motivo,
        "vivas": [p.pk for p in vivas],
        "reatas": None if reatas is None else {p.pk: destino.pk for p, destino in reatas},
        "destino_motor": motor.novo_estado if motor.permitido else None,
        "gate_pergunta_viva": pergunta_ok,
        "gate_pai": csf_ok,
        "csf": csf,
    }


def origem_da_acao(acao):
    portas, escritor = PORTAS.get(acao, (NV, NV))
    return {"portas": portas, "escritor": escritor}


def linhas_c2(empresa, inicio, fim):
    s1 = carregar_s1()
    conta = s1["ator"]("S1_GESTOR_PK")
    observacoes = {}

    class GestorDaS1(s1["PortasGestor"]):
        def consultar(self, p, ch=None):
            leitura = super().consultar(p, ch)
            if ch is not None:
                observacoes[(ch.pk, p.pk)] = p, leitura
            return leitura

    # Mesmos insumos entregues a c2 por colher() de sonda_S1.py.
    base = ChamadoColaborador.objects.filter(
        colaborador__empresa_id=empresa.pk, status_local__in=VIVOS,
    ).select_related("colaborador__empresa", "celula").order_by("pk")
    chamados = {}
    for ch in base:
        comp = competencia_do_chamado(ch)
        dia = comp.dia if comp is not None else None
        if dia is None or inicio <= dia <= fim:
            chamados[ch.pk] = ch, dia
    gestor = GestorDaS1(conta, particionar_verbos(base, agora=AGORA)["mapa"])
    _, _, pares = s1["c2"](chamados, inicio, fim, gestor)
    exigencias = exigencia_das_perguntas(sorted({r.pergunta for r in pares}), AGORA)
    cxr_da_competencia = cxr_lavrado()

    @lru_cache(maxsize=None)
    def insumos(colab):
        return InsumosDoDono(
            colab, inicio, fim, HOJE, agora=AGORA, sem_motor=True,
            cxr=cxr_da_competencia(colab.pk, inicio),
        )

    @lru_cache(maxsize=None)
    def parcial(colab):
        # O GET da view marca lidas. O construtor do parcial so fornece contexto.
        # app/chamados/views.py:870-881; app/chamados/services/fio.py:573-610
        ctx = contexto_parcial_fio(gestor.req, colab)
        ctx["cor_disputas_map"] = DisputaSupervisao.cor_map_por_colaborador(colab)
        html = template_conferir_data().render(Context(ctx, use_l10n=False))
        return (
            FormulariosConferirData(html).cartoes_da_pergunta,
            {ch.pk: ch for ch in ctx["chamados"]},
        )

    linhas = []
    for r in sorted(pares, key=lambda r: (r.chamado, r.pergunta)):
        ch = chamados[r.chamado][0]
        p, leitura = observacoes[(r.chamado, r.pergunta)]
        local, fio, gaveta, glpi_local, remoto, lote, acoes_s1 = leitura
        d = p.disputa
        colab = d.colaborador
        ancora = chamado_que_lista(p)
        pai = s1["chamado_de"](d.chamado_id) if d.chamado_id is not None else None
        alvo = s1["chamado_de"](ancora) if ancora is not None else None
        colabs = {colab, ch.colaborador}
        colabs.update(c.colaborador for c in (pai, alvo) if c is not None)
        cartoes_conferir = sorted({
            pk for c in colabs for pk in parcial(c)[0].get(p.pk, ())
        })
        dia_evento, diverge, dia_ancora = data_do_fato_de(p)
        # A porta confere permissao de acao e pergunta por PK.
        # A justificativa e entrada humana. app/chamados/views.py:1288-1309
        # app/chamados/services/validacao.py:1057-1072
        conferir = bool(
            cartoes_conferir and gestor.pode_inline and diverge
            and p.data_conferida_em is None
        )

        foto = foto_do_fechamento(ch)
        c6 = pergunta_viva(p, hoje=HOJE, agora=AGORA)
        app, web, app_login, web_login = s1["portas_colab"](colab)
        razao, exigidas = exigencias.get(p.pk, (NV, None))
        aplicacao = estado_aplicacao_pergunta(d, p) or {}
        espera_texto = espera_o_texto_do_colaborador(p)
        pode_agir_ancora = pode_agir_chamado(conta, alvo) if alvo is not None else None

        forms_fio = {
            (card.pk, f["acao"])
            for c in colabs
            for card, _, f in gestor.modal(c)[1].get(p.pk, ())
        }
        cartao = parcial(alvo.colaborador)[1].get(ancora) if alvo is not None else None
        eleita = getattr(cartao, "disputa_obj", None)
        # Testemunha em leitura do caso A3, com a propria disputa fechada,
        # outra aberta eleita no container e falta de Validar no cartao.
        a3 = bool(
            not conferir and pode_agir_ancora and c6
            and alvo.colaborador_id == d.colaborador_id
            and d.fechada_em is not None and d.chamado_id != ancora
            and eleita is not None and eleita.chamado_id == ancora
            and (ancora, "validar_pergunta") not in forms_fio
            and s1["validar_em_leitura"](p, False)
        )

        verbos = {a["acao"] for a in exigidas or ()}
        verbos.update(
            "validar" if acao == "validar_pergunta" else acao for _, acao in forms_fio
        )
        if p.pk in foto["csf"]:
            verbos.add("validar")
            if d.aberta:
                verbos.add("recusar_pergunta")
        if conferir:
            verbos.add("conferir_data")

        linhas.append({
            "chamado": r.chamado,
            "pergunta": p.pk,
            "origem/destino": {
                "disputa": d.pk, "pai": d.chamado_id,
                "destino": p.chamado_destino_id, "ancora": ancora,
                "disputa_fechada": d.fechada_em is not None,
                "disputa_eleita_no_cartao": getattr(eleita, "pk", None),
                "dia_do_recorte_S1": r.dia, "data_evento": dia_evento,
                "dia_da_ancora": dia_ancora,
            },
            "plano_de_reata": {
                "reconciliaria": foto["fecha"], "destino_motor": foto["destino_motor"],
                "gate_pergunta_viva": foto["gate_pergunta_viva"],
                "vivas_no_pos_estado": foto["vivas"], "reatas": foto["reatas"],
                "neste_plano": p.pk in foto["vivas"],
            },
            "CSF": {
                "gate_pai_permite": foto["gate_pai"],
                "perguntas_que_seguram": foto["csf"],
                "neste_par": p.pk in foto["csf"],
            },
            "C6": c6,
            "ator/perfil": {
                "gestor_S1_pk": conta.pk, "gestor_ativo": conta.is_active,
                "gestor_staff": conta.is_staff, "gestor_superuser": conta.is_superuser,
                "gerir_chamados": gestor.pode_inline, "pode_agir_ancora": pode_agir_ancora,
                "departamento_ancora": departamento_do_chamado(alvo) if alvo else NV,
                "colab_do_fato": f"col{colab.pk}",
                "login_app_estimado": app_login, "conta_web_ativa": web_login,
            },
            "canal/acao": {
                "sub_S1": r.sub,
                "local": sorted(local), "fio": sorted(fio), "gaveta": sorted(gaveta),
                "glpi_local": sorted(glpi_local), "glpi_remoto": NV if remoto else False,
                "lote_S1": lote, "formularios_fio": sorted(forms_fio),
                "acoes_gestor_S1": sorted(acoes_s1),
                "app_S1": p.pk in app, "web_S1": p.pk in web,
                "conferir_data_no_parcial": cartoes_conferir,
                "exigidas_pelo_juiz_da_tarefa": exigidas if exigidas is not None else NV,
                "sem_acao_no_recorte": "nao" if conferir else "sim",
            },
            "motivo_da_recusa": {
                "fechamento_em_seco": foto["motivo"],
                "razao_tarefa": razao.partition(":")[0],
                "estado_aplicacao": aplicacao.get("estado", NV),
                "espera_texto_colab": espera_texto,
                "data_diverge": diverge, "data_ja_conferida": p.data_conferida_em is not None,
                "resultado_POST": NV,
            },
            "dono": (insumos(colab).dono(r.dia) or NV) if r.dia is not None else NV,
            "colab": f"col{r.colab}",
            "conferir_data": "sim" if conferir else "nao",
            "porta/escritor": {acao: origem_da_acao(acao) for acao in sorted(verbos)} or NV,
            "obra_dona": NV,
            "obra_candidata": {"obra": OBRA_A3, "fonte": FONTE_A3} if a3 else NV,
            "fontes": {
                "universo": f"sonda_S1.py:{s1['c2'].__code__.co_firstlineno}",
                "origem/destino": "app/chamados/services/fio.py:97,136",
                "gate": "app/chamados/models.py:393,429,677,683,718,783",
                "CSF": "app/ponto/supra_juiz.py:561",
                "C6": "app/chamados/juizes.py:911",
                "ator": "app/chamados/views.py:35;app/chamados/utils.py:68;app/api/login_helpers.py:110",
                "conferir_data": "app/chamados/services/fio.py:610;app/templates/chamados/partials/modal_fio.html:173;app/chamados/views.py:1306",
                "tarefa/aplicacao": "app/chamados/services/tarefa_do_dia.py:461;app/chamados/services/materializacao.py:880",
                "dono": FONTE_DONO,
            },
        })
    return linhas


def colher():
    with schema_context("juliani"):
        connection.set_autocommit(False)
        try:
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
                cursor.execute(
                    "SELECT current_schema(), current_setting('transaction_read_only'), "
                    "current_setting('transaction_isolation')"
                )
                if cursor.fetchone() != ("juliani", "on", "repeatable read"):
                    raise SystemExit("PAREI: schema ou modo de transacao incorreto")
            if not User.objects.filter(username="sistema").exists():
                raise SystemExit("PAREI: usuario de sistema ausente na Sombra")
            empresa = Empresa.objects.get(pk=EMPRESA_ID)
            inicio, fim = janela_fechamento(MES, ANO, empresa)
            linhas = linhas_c2(empresa, inicio, fim)
            with connection.cursor() as cursor:
                cursor.execute("SELECT 1")
            return linhas
        finally:
            connection.rollback()
            connection.set_autocommit(True)


class SemErroComDadosLivres:
    def __enter__(self):
        return self

    def __exit__(self, tipo, erro, quadro):
        if tipo is None or issubclass(tipo, (SystemExit, KeyboardInterrupt)):
            return False
        while quadro.tb_next is not None:
            quadro = quadro.tb_next
        arquivo = quadro.tb_frame.f_code.co_filename.rsplit("/", 1)[-1]
        raise SystemExit(f"PAREI: {tipo.__name__} em {arquivo}:{quadro.tb_lineno}") from None


def valor(x):
    if x is None:
        return "-"
    if isinstance(x, str):
        return x.replace("|", "\\u007c")
    return json.dumps(x, default=lambda v: v.isoformat(), ensure_ascii=True).replace("|", "\\u007c")


def main():
    nivel_anterior = logging.root.manager.disable
    logging.disable(sys.maxsize)
    try:
        with SemErroComDadosLivres():
            linhas = colher()
            chamados = {r["chamado"] for r in linhas}
            colabs = {r["colab"] for r in linhas}
            falsos = [r for r in linhas if r["conferir_data"] == "sim"]
            chamados_falsos = {r["chamado"] for r in falsos}
            saida = ["S4|C2|" + "|".join(f"{k}={valor(v)}" for k, v in r.items()) for r in linhas]
    finally:
        logging.disable(nivel_anterior)
    print(
        f"S4|UNIVERSO|chamados={len(chamados)}|colabs={len(colabs)}"
        f"|pares={len(linhas)}|esperado_chamados={ESPERADO}"
    )
    for linha in saida:
        print(linha)
    print(f"S4|FALSO_POSITIVO_CONFERIR_DATA|pares={len(falsos)}|chamados={len(chamado

[truncado em 20000 de 22521 caracteres]

## 27/11 21:13 · sessao 3ad2776bbb19 · FERIADO-L097-MOTOR · leitura · origem=Code

# FERIADO-L097-MOTOR · Dossiê de leitura

**Base para a cura integrada: FERIADO-UM-JUIZ em `1d2eb496c`, em cópia própria, ou a `main` depois que essa obra pousar.** O motor é idêntico nas duas cópias. A diferença que interessa está depois dele: na `main` do hash solicitado, 240 min de feriado em folga incerta entram em trabalhadas comuns. Na obra em voo, para um 6x1 elegível na competência 11, os mesmos 240 min entram em HE100-feriado. É nessa segunda base que o RED suspenso verifica a troca de rubrica mantendo 240 min. As perguntas de lei ficaram nos casos **só feriado com previsto zero** e **12x36 em folga no feriado**. `app/ponto/services/fechamento.py:948–1009` · `c11/app/ponto/calculador/regras.py:371–432` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:547–563` · `aval.txt:1` · `parei.txt:1`.

`c11/` indica a cópia `~/wt-codex-feriado-um-juiz-152427`. O checkout aponta para `1d2eb496c` e sua base comum com `main` é `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Os números de teste neste dossiê são expectativas lidas no código. Execução dos testes e resultado de produção [nao verificado]. `/home/ronald/saas-hasner/.git/packed-refs:165` · `~/wt-codex-feriado-um-juiz-152427/.git:1` · `/home/ronald/saas-hasner/.git/worktrees/wt-codex-feriado-um-juiz-c4-152427/HEAD:1`.

## 1. Mapa do bloco vivo

| Linhas | O que faz hoje |
|---|---|
| `app/ponto/motor_calculo_v2.py:2513–2522,2558–2574` | O laço resolve o dia da jornada, que pode herdar o do par anterior. Classifica a folga pelo vínculo ou pela regra semanal, guarda a resposta pela entrada localizada, calcula o par e chama `_dia_sem_previsto_fora_da_janela`. |
| `app/ponto/motor_calculo_v2.py:1554–1564` | Sai sem entrada ou sem minutos trabalhados. Continua se `_folga_do_par` ou `eh_feriado(dia)` for verdadeiro. |
| `app/ponto/motor_calculo_v2.py:1486–1509,1511–1534,1565–1571` | Exige janela HE vigente para a empresa e previsto **igual a zero**. Previsto positivo e `None` saem. Consulta a decisão vigente e sai se `autorizado`. |
| `app/ponto/motor_calculo_v2.py:1579–1584` | Só consulta `escala_certa_no_dia` se o par é folga e há identificador de colaborador. Se a escala é incerta, retorna com o par intacto. |
| `app/ponto/motor_calculo_v2.py:1585–1590` | Soma o trabalhado líquido a `minutos_fora_janela`. Zera `minutos_trabalhados`, noturno relógio e legal, extra total, extra 50, extra 100, extra 100 feriado, os dois splits noturnos de HE e intra indenizada. |
| `app/ponto/motor_calculo_v2.py:1591–1602,2833` | Colapsa `efetivo_saida` na entrada efetiva, acrescenta o alerta L-097 e publica `{dia, minutos arredondados, hora}` em `dias_sem_previsto_fora_da_janela`. A numeração viva põe o colapso e a publicação depois de `:1590`. Um retorno dentro do trecho avalizado já evita as três escritas. `aval.txt:1`. |
| `app/ponto/motor_calculo_v2.py:2581–2607` | Depois da chamada, o mesmo juízo de folga manda o par para `periodos_ft` e acrescenta o alerta genérico de FT. Os demais pares vão para `periodos`. |

| Caso | Resposta atual com janela vigente e sem autorização |
|---|---|
| **a. Feriado em folga, escala certa** | Com previsto zero, bloqueia o par inteiro. Sai em `periodos_ft` com zero, pontas efetivas iguais e pendência. `app/ponto/motor_calculo_v2.py:1562–1602,2581–2605`. |
| **b. Feriado em folga, escala incerta** | Retorna em `:1584` e preserva o par em `periodos_ft`, sem pendência deste bloco. `app/ponto/motor_calculo_v2.py:1579–1584,2581–2605`. |
| **c. Feriado sem flag de folga** | Com previsto zero, chega pelo `eh_feriado` e bloqueia sem consultar certeza. Com previsto positivo, sai em `:1568` e segue o cálculo do ciclo. Cadastro com marcos incompletos pode produzir o primeiro caso. A janela ordinária das pontas roda antes deste bloco. `app/escala/utils.py:2314–2325` · `app/ponto/motor_calculo_v2.py:1562–1586,1865–1876,2571–2574,3380–3397`. |
| **d. Folga comum, escala certa** | Com previsto zero, continua bloqueada, com alerta, par colapsado e pendência. `app/ponto/motor_calculo_v2.py:1579–1602` · `app/ponto/tests/test_he100_dia_sem_previsto.py:151–159`. |
| **e. 12x36** | Passa pela chamada comum. Plantão previsto sai pelo previsto positivo. Folga real com escala certa e previsto zero pode ser bloqueada, inclusive no feriado. O corte de **dobra** é independente: `CICLOS_FERIADO_SIMPLES` põe o 12x36 em horas simples e a CCT cadastrada pode reativar a dobra. `app/ponto/motor_calculo_v2.py:1567–1586,2574,2911–2945,3740,3810–3821` · `app/core/regua_cct.py:473–479` · `app/ponto/tests/test_he100_dia_sem_previsto.py:39–48,208–212`. |
| **f. Intermitente** | Passa pela chamada comum. A fábrica escolhe `MotorIntermitente` por ciclo ou base e o construtor injeta calendário vazio, logo `eh_feriado` é falso. O laço pula o juízo de folga do vínculo intermitente. Na fixture vigente, sai em `:1564` e apura o par. O `elif dias_folga` ainda existe para folga semanal configurada. Ocorrência desse cadastro na frota [nao verificado]. `app/ponto/motor_calculo_v2.py:1000–1011,2474–2478,2558–2574,3694–3698,3764–3767` · `app/ponto/tests/test_s6_intermitente_feriado.py:105–152` · `app/docs/PROMPTS.md:1231,2536`. |

## 2. Escritores, leitores e risco de duplicação

| Escritor ou leitor | Efeito ao preservar o par |
|---|---|
| **Motor e composição das fatias** | O cálculo por par carimba entrada, saída e intervalos e calcula minutos, noturno, intra e HE. A L-097 sobrescreve os campos e publica a pendência. O fechamento concatena as listas das fatias. Com a exceção, os valores calculados chegam aos leitores e o acréscimo de fora da janela deste bloco deixa de existir. O teto L-093 lê `trabalhado + fora`, cuja soma se conserva nessa troca. `app/ponto/motor_calculo_v2.py:1276–1278,1585–1602,1946–1955,2875–2932,3353–3389` · `app/ponto/services/fechamento.py:116–120`. |
| **`ResultadoMes` e fechamento atual** | HE50/100 do motor soma `periodos`. Noturno e intra incluem também `periodos_ft`. O fechamento consulta `_certa` sobre FT, usa `feriado_vira_folga` para separar a certa, promove o feriado pelo trabalhado preservado, põe a outra certa em FT e a incerta em trabalhadas. `app/ponto/motor_calculo_v2.py:295–370` · `app/ponto/services/fechamento.py:938–1009,1028`. |
| **Juiz de dobra e partição c11** | `_feriado_dobra_no_dia` lê política do ciclo e calendário no dia do turno. Ele já marca HE100 no cálculo por par. A partição c11 consulta esse juiz no contexto do dia, escolhe uma única rubrica para cada FT e entrega as mesmas listas ao mensal, ao `DiaPago` e ao oráculo. Preservar o par dá minutos a essa partição. `app/ponto/motor_calculo_v2.py:941–978,2925–2932,3385–3392` · `c11/app/ponto/calculador/regras.py:386–453` · `c11/app/ponto/services/fechamento.py:995–1021,1237–1241,1296–1320`. |
| **`segmentos_do_motor` e `regras.py`** | O endereço vivo é `app/ponto/calculador/alimentacao.py`. A função lê `efetivo_entrada/saida`, fora da janela, pontas HE e intervalos. Hoje descarta o par colapsado e conta `par_bloqueado_sem_previsto`. A lista de bloqueados faz `rubricas_por_dia` visitar o dia para afirmar zero. Preservado o par, o dia chega pelo segmento válido. Na base, a Regra 4 põe FT incerta em trabalhadas pela L-007. No c11, o oráculo recebe a projeção da partição de FT e usa suas rubricas. `app/ponto/calculador/alimentacao.py:35,73–123,124–159,459–471` · `app/ponto/calculador/regras.py:674–725` · `c11/app/ponto/calculador/alimentacao.py:353–389,512–518,571–591` · `c11/app/ponto/calculador/regras.py:767–818`. |
| **`DiaPago`, espelho, PDF e app** | A lavra lê minutos e rubricas da lista correspondente. Na FT promovida, registra também `folga_feriado_min`. `pago_do_dia` soma trabalhadas, FT e esse detalhe uma vez. Uma linha toda zerada devolve `None`. Com a cura, os leitores passam a mostrar as horas lavradas. O espelho/PDF também leem os alertas do par. O alerta L-097 some no caso liberado. O aviso genérico de FT, que ainda diz “Requer validacao”, nasce depois do bloco. `app/ponto/services/dia_pago.py:175–185,223–267,867–898` · `app/ponto/services/espelho.py:51–105,669–671,825–833` · `app/api/views.py:1517–1536` · `app/relatorios/pdf_espelho.py:973–977` · `app/templates/ponto/espelho.html:302–304` · `app/ponto/motor_calculo_v2.py:2598–2605`. |
| **Gestão de HE e portão** | O espelho traduz a lista do motor em `he_fora_da_janela`, com `sentido='dia'`. O retrato lavrado alimenta Gestão, lote e contador. Na próxima lavra do retrato, desaparece a pendência produzida por esse bloco. O portão deixa de contar essa ocorrência quando ela era a que travava por estar acima do limite, sem decisão e com a trava ligada. Marcações ordinárias de ponta têm origem própria. `app/ponto/services/espelho.py:735–763,790` · `app/ponto/services/he_pendente_lavrado.py:138–159,183–195` · `app/ponto/services/gestao_he.py:426–434,488–504` · `app/folha/porta_export.py:181–201,229–242,501,682–697`. |
| **TXT, 59-A e conferências** | O export separa HE100 comum pela diferença `HE100 total − HE100 feriado` e emite o evento de feriado uma vez. O split noturno de HE é subconjunto. Na base, o filtro 59-A é feito na emissão. No c11, desde 11/2026, a emissão lê o mensal já classificado por fatia e mantém a leitura histórica pelo 59-A. O cartão c11 lê o mesmo evento. O portão e os comandos de DIFF também leem minutos para conferência. `app/folha/export.py:402–448` · `c11/app/folha/export.py:383–394,434–462` · `c11/app/relatorios/cartao_pela_celula.py:90–100` · `app/folha/porta_export.py:106–111,541–554` · `app/ponto/calculador/periodo.py:747–790` · `app/ponto/management/commands/diff_calculador.py:253–268`. |

**Risco de pagar duas vezes.** O caminho normal de dobra existe e consome `periodos`. A promoção de FT consome `periodos_ft`. A separação por identidade impede que o mesmo FT promovido também entre em trabalhadas comuns. O próprio código registra a regressão anterior que fazia isso, inclusive duplicando noturno e intra. O RED deve cobrar uma parcela de 240 min no pago do dia e um único evento de pagamento da hora. Em um feriado de `periodos`, trabalhadas e adicional de feriado são parcelas da dobra do ciclo. `app/ponto/motor_calculo_v2.py:295–344,2581–2607` · `app/ponto/services/fechamento.py:975–1009` · `app/ponto/services/dia_pago.py:175–185,244–267,894–898` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:67–92`.

## 3. Os 240 min e a rubrica

| Cenário com o mesmo par líquido de 240 min | Resultado |
|---|---|
| **Hash solicitado, escala incerta** | **240 min em `horas_trabalhadas`**, sem adicional de 100% dessa FT. Retorno do motor → `_ft_sem_escala` → mensal e `DiaPago`. `app/ponto/motor_calculo_v2.py:1579–1584` · `app/ponto/services/fechamento.py:999–1009` · `app/ponto/services/dia_pago.py:259–267`. |
| **c11, competência 11, 6x1 elegível, incerta** | **240 min em HE100-feriado**, evento `hora_extra_feriado_diurna`. A fixture verifica LIMBO, A2 e A8. O controle histórico de outubro conserva trabalhadas comuns. `c11/app/ponto/calculador/regras.py:371–377,415–432` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:94–108` · `c11/app/ponto/tests/test_he100_dia_sem_previsto.py:234–268`. |
| **c11, 6x1 certa, após preservar o par** | **240 min em HE100-feriado** se o vínculo faz o feriado virar folga e o ciclo dobra. **240 min em FT** se já era uma folga da célula e o vínculo trabalha em feriados. O RED suspenso usa esta segunda configuração. Decidem `escala_certa_no_dia`, `feriado_vira_folga` e o juiz de dobra, consumidos por `particionar_ft`. `app/escala/services/escala_certa.py:19–51` · `app/escala/models.py:1384–1406` · `c11/app/ponto/calculador/regras.py:371–377,415–453` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:110–118,547–563`. |

O código efetivo do Domínio vem de `RubricaDominio` ativa, com empresa prevalecendo sobre global. **0200** aparece para feriado em cadastro explícito da fixture c11. As migrações também usam 200 para FT global e HE100 comum global. **0243** é o padrão de intrajornada. O código cadastrado para J.A., a emissão efetiva e qualquer igualdade em reais [nao verificado]. O anexo fixa a igualdade em **240 minutos pagos**. `c11/app/folha/export.py:1008–1015` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:151–176` · `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:1–11` · `app/folha/migrations/0009_evento_he100_rubrica_200.py:1–10` · `app/ponto/models.py:987` · `LEIS-2711-1610.txt:1(C)`.

## 4. Leis herdadas

| Lei | Consequência para a obra |
|---|---|
| **L-097** | A janela de HE tem cadastro, decisão e portão. O aval substitui o corte anterior para o feriado trabalhado no bloco sem previsto e preserva o bloqueio da folga comum certa. O registro da L-097 também traz a emenda RAMO2 para a ponta de saída, uma regra distinta deste bloqueio integral. `app/docs/LEIS.md:115` · `app/ponto/motor_calculo_v2.py:1536–1552,1847–1876` · `aval.txt:1`. |
| **L-007 e HE100-ALCANCE-A-PREMISSA** | Escritor canônico e resultado explícito. A folga incerta escapa do bloqueio pela autoridade `escala_certa_no_dia`. O corte anterior ainda incluía feriado no alcance, ponto que o novo aval altera ao menos para o RED de folga certa. `app/docs/LEIS.md:65` · `app/docs/PROMPTS.md:1514–1516` · `app/ponto/calculador/regras.py:689–705` · `aval.txt:1`. |
| **FERIADO-UM-JUIZ-2711-1210** | O anexo C exige o mesmo trabalhado nos dois estados de certeza, com a rubrica resolvida pela regra existente. O `!` posterior abre o motor no bloco citado e resolve a proibição de tocá-lo registrada na parada. Fechamento e oráculo já têm a partição compartilhada no c11. `LEIS-2711-1610.txt:1(C)` · `parei.txt:1` · `aval.txt:1` · `c11/app/ponto/calculador/regras.py:371–453` · `CLAUDE.md:17–22`. |
| **L-092, L-113 e L-095** | O corte desta obra exige exportada intocada, lista nominal da comp. 11 publicada antes do apply e reversão. A permissão genérica da L-113 para recalcular certas exportadas não serve como guarda da obra. L-095 põe os leitores sobre o dinheiro lavrado pela porta. `app/docs/LEIS.md:113,125,155` · `CLAUDE.md:453–465` · `aval.txt:1`. |
| **BUG-144** | O realizado vem dos pares fechados pelo juiz, descontadas as pausas julgadas, com a borda posicional prevista pela L-115. A cura preserva o par efetivo que o motor já calculou, lido também pelo oráculo. `app/docs/TICKETS.md:1006` · `app/docs/LEIS.md:153` · `app/ponto/turnos.py:707–771,774–795` · `app/ponto/calculador/alimentacao.py:114–159`. |

## 5. Casos para o RED

| Cadastro e batidas | Resposta pela regra e molde |
|---|---|
| **Feriado em folga certa e incerta.** 6x1 comercial 07–15, comp. 11, feriado 02/11, célula `trabalha=False`, E07/S11 e janela vigente. Rodar a mesma batida com LIMBO e com escala certa, sem decisão e com autorização como controle. | **240 min nos dois.** Com `trabalha_em_feriado=True`, incerta → HE100-feriado e certa → FT. Repetir com `False` para provar certa → HE100-feriado quando o feriado causa a folga. Conferir mensal, as duas lavras, `pago_do_dia` e evento. O skip já contém a primeira comparação. Para a janela por cadastro, reaproveitar os campos da fixture da base. `c11/app/ponto/tests/test_feriado_um_juiz.py:9` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:23–118,547–563` · `app/ponto/tests/test_he100_dia_sem_previsto.py:31–34`. |
| **Folga comum certa.** Janela vigente, previsto zero, E06h30/S12h, sem decisão. | **0 pago, 330 fora, par colapsado, alerta L-097, pendência `sentido='dia'`**, zero nas duas lavras. `app/ponto/tests/test_he100_dia_sem_previsto.py:55–81,120–123,151–159,203–206,294–297` · `app/ponto/motor_calculo_v2.py:1585–1602`. |
| **Feriado sem folga.** 6x1, `trabalha_em_feriado=True`, célula de trabalho com marcos completos, E07/S11 dentro da janela. Repetir com marcos incompletos e afirmar previsto zero. | Com previsto positivo, preserva **240 min** e classifica a dobra pelo ciclo. Com previsto zero, hoje bloqueia. O esperado novo dessa segunda variante depende da pergunta 0. `app/escala/utils.py:2319–2325` · `app/ponto/motor_calculo_v2.py:1562–1602,3380–3392` · `app/ponto/tests/test_feriado_sumula146.py:68–86`. |
| **12x36 no feriado.** Plantão previsto E07/S19 no molde vigente, com e sem cláusula. Em separado, folga real de escala certa com janela vigente, E06h30/S12h. | Plantão previsto conserva o corte de horas simples no piso e a dobra com cláusula. A fixture com cláusula fixa **720 trabalhados e 720 de HE100-feriado**, sem pendência. O controle da folga real hoje fixa **zero nas duas lavras**. `app/ponto/tests/test_feriado_12x36_art59a.py:156–169,226–234` · `app/ponto/tests/test_s6_intermitente_feriado.py:168–177` · `app/ponto/tests/test_he100_dia_sem_previsto.py:39–48,179–193,208–212`. |
| **Intermitente em feriado e oráculo do mesmo par.** Intermitente E07/S19 na fixture vigente. Para o 6x1 liberado, entregar o próprio `ResultadoMes` do RED a `segmentos_do_motor`. | Intermitente **720 trabalhados, zero HE100-feriado, sem pendência integral**. Para o 6x1, cobrar E07/S11 nas pontas efetivas, **240 min** em `pares_por_dia`, ausência de bloqueio e paridade das lavras. Para a folga comum, cobrar o descarte do par e `par_bloqueado_sem_previsto`. `app/ponto/tests/test_s6_intermitente_feriado.py:105–152` · `app/ponto/tests/test_he100_dia_sem_previsto.py:259–297` · `app/ponto/calculador/alimentacao.py:73–123,157–159`. |

## 6. Arquivos e pouso [nao verificado]

- A mudança de produção cabe no bloco de `app/ponto/motor_calculo_v2.py`. Os testes de integração se sobrepõem à obra em voo, que já mudou fechamento, `dia_pago`, partição, alimentação e export. O RED mais direto para abrir é `test_RED_c4_l097_certeza_so_decide_rubrica_com_ou_sem_autorizacao`. O caracterizador atual `test_RED_folga_feriado_bloqueada_oraculo_zera_como_o_motor` é **12x36 de outubro**. Para provar a inversão autorizada do 6x1, usar a variante da comp. 11. A inversão da asserção do 12x36 depende da pergunta 1. `aval.txt:1` · `app/ponto/tests/test_he100_dia_sem_previsto.py:39–48,208–212` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:547–563` · `c11/app/ponto/services/fechamento.py:995–1000,1296–1320` · `CLAUDE.md:318–320,441–442`.
- O `expectedFailure` de troca 6x1→12x36 na borda 20/21 carrega a referência ao PAREI, mas seu helper desliga a janela e força certeza falsa. Ele não demonstra a falha L-097. Para TXT ponta a ponta, há ainda uma borda: a exceção de `horas_trabalhadas == 0` no c11 reconhece feriado emitível, mas a condição não inclui FT. O `_confere` da fixture verifica a função de eventos e as lavras, sem provar a passagem pelo classificador nesse mês composto só por FT. `c11/app/ponto/tests/test_feriado_um_juiz.py:903–922,942–959` · `c11/app/folha/export.py:924–960` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:67–92`.
- **Comp. 11 J.A., `colab | dia | minutos` que passam a pagar: [nao verificado].** Os anexos trazem o alvo do RED e o requisito de publicação. A lista nominal, a situação de exportação da competência e a prova de reversão [nao verificado]. Esses itens seguem como condições do apply. `aval.txt:1` · `LEIS-2711-1610.txt:1(C)` · `CLAUDE.md:453–465`.

## 7. Padrão e perguntas de lei

**Padrão:** `Single Writer Principle` (*juiz único / escritor único*). O motor decide a elegibilidade do par. A partição existente escolhe a rubrica e as projeções leem essa decisão. No pouso, a reconstrução de projeções da competência aberta segue o padrão *projection rebuild* da L-121. `app/docs/GLOSSARIO.md:11–12,25–26` · `app/docs/PLANO-PADROES.md:28–34` · `c11/app/ponto/services/fechamento.py:995–1000,1296–1320`.

0. **Abre — feriado sem folga e com previsto zero.** A frase “feriado trabalhado [...] não entra no filtro” inclui o par que só chegou por `eh_feriado`, ou o `!` vale só para feriado em folga com escala certa, como pede o PAREI? Se também abranger feriado com marcos, a janela ordinária roda antes e está fora do bloco autorizado. `aval.txt:1` · `parei.txt:1` · `app/ponto/motor_calculo_v2.py:1562–1586,1865–1876,2571–2574`.
1. **12x36 em folga real no feriado.** “Segue pelos cortes vigentes” preserv

[truncado em 20000 de 20787 caracteres]

## 27/11 21:11 · sessao 346c9d3e4456 · - · - · origem=Ronald (tmux)

**Base integrada recomendada: c11 da FERIADO-UM-JUIZ.** `main` aponta para `59600163c` e o worktree indicado para `1d2eb496c`. A comparação Git encontrou 22 arquivos modificados e o motor idêntico. O c11 já tem o julgamento de rubrica, a prova de paridade e o RED exato suspenso. O commit declara que ainda aguardava certificação e pouso. A cura de produção cabe no bloco do motor e a prova se beneficia dos testes da obra em voo. `/home/ronald/saas-hasner/.git/packed-refs:165`, `/home/ronald/saas-hasner/.git/worktrees/wt-codex-feriado-um-juiz-c4-152427/HEAD:1`, `/home/ronald/saas-hasner/.git/worktrees/wt-codex-feriado-um-juiz-c4-152427/logs/HEAD:9`, `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:547`.

**Leis herdadas.**

- L-097 bloqueia HE de janela e publica pendência. A emenda RAMO2 preserva trabalhado e noturno após a saída e mantém a classificação da HE. O aval novo excepciona feriado no bloco nomeado. `app/docs/LEIS.md:115`, `app/ponto/motor_calculo_v2.py:1536–1602`, `aval.txt:1`.
- L-007 exige escritor canônico e trilha. A aplicação de “nada em branco” sobre folga incerta paga hora comum no caminho histórico. HE100-ALCANCE-A-PREMISSA exclui folga incerta do bloqueio. O critério posterior exige classificação nominal de cada dia como folga certa ou feriado. `app/docs/LEIS.md:65`, `app/ponto/calculador/regras.py:389–394`, `app/docs/PROMPTS.md:1514–1516`.
- FERIADO-UM-JUIZ mantém o juiz `_feriado_dobra_no_dia` e classifica FT no c11. A escala certa, o ciclo e `feriado_vira_folga` determinam o destino. A regra dos 240 minutos iguais consta do anexo C. O aval completo 2711-1210 segue [nao verificado]. `LEIS-2711-1610.txt:1`, `parei.txt:1`, `app/ponto/motor_calculo_v2.py:941–961`, `~/wt-codex-feriado-um-juiz-152427/app/ponto/calculador/regras.py:371–432`, `app/escala/models.py:1384–1406`.
- L-095 impõe uma lavra de referência do dinheiro. BUG-144 preserva pares fechados do juiz, desconta pausas julgadas e tem a exceção posicional L-115. Corrigir a origem e usar o mesmo par no oráculo preserva ambos. `app/docs/LEIS.md:113,153`, `app/docs/TICKETS.md:1006`, `app/ponto/motor_calculo_v2.py:1591–1597`.
- L-092 deve ser lida com L-113 e a guarda atual, que contém 09/2026. A restrição desta obra é exportada intocada e DIFF nominal da 11/J.A. antes do apply com reversão. O dado entregue não contém linhas colab/dia/minutos nem estado de exportação da 11/J.A. [nao verificado]. `app/docs/LEIS.md:125,155,183`, `CLAUDE.md:453–465`, `app/ponto/services/fechamento.py:36–50,559–565,578–595`, `aval.txt:1`.

**RED e controles.** O c11 já tem 6x1 de 02/11, E 07:00/S 11:00, LIMBO para incerta e `None` para certa, com/sem autorização. Espera 4h HE feriado na incerta e 4h FT na certa se `trabalha_em_feriado=True`. A variante que faz feriado virar folga espera as mesmas 4h HE feriado na certa. A fixture compara mensal, duas lavras, `pago_do_dia` e eventos TXT. `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:23–118,547–563`.

O teste existente que exige zero em feriado/certa é **12x36**, e seu conteúdo permaneceu no c11. Serve de guarda se “12x36 intocado” preservar o bloqueio em folga/certa. Para inverter com 6x1, use o RED suspenso. A mesma bateria cobre folga comum zerada, +330 fora da janela e publicação de pendência. `app/ponto/tests/test_he100_dia_sem_previsto.py:39–47,151–159,203–212`, `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_he100_dia_sem_previsto.py:208–212,250–268`.

**Perguntas de lei.**

0. Abre a obra liberar só a folga certa de ciclo com dobra ou também a folga certa 12x36? O bloco atual alcança 12x36 em folga com previsto zero. A regra 59-A e a cláusula são outro corte. `aval.txt:1`, `app/ponto/tests/test_he100_dia_sem_previsto.py:39–47,208–212`, `app/ponto/motor_calculo_v2.py:1562–1589,3733–3740`, `app/core/regua_cct.py:473–479`.
1. No feriado fora de folga com previsto zero, o par continua bloqueado como hoje ou entra na frase ampla do aval? Para provocar o caso, use 6x1, célula `trabalha=True`, janela vigente, marcos incompletos e E 07:00/S 11:00. Um irmão com previsto positivo sai do bloco em `:1567`. `parei.txt:1`, `aval.txt:1`, `app/escala/utils.py:2319–2325`, `app/ponto/motor_calculo_v2.py:1562–1589`.
2. A dispensa do filtro inclui pontas de jornada regular ou a mensagem posterior “Requer validação do responsável”? A janela regular e essa mensagem estão fora do bloco autorizado. `aval.txt:1`, `app/ponto/motor_calculo_v2.py:1865–1876,2571–2574,2598–2605`.

**Padrão:** *single source of truth / um juiz por pergunta*. O motor escreve o par efetivo, a partição chama o juiz de dobra e o oráculo lê as mesmas pontas. `app/docs/PLANO-PADROES.md:49,55–62`, `app/docs/GLOSSARIO.md:12`, `CLAUDE.md:17–22,182`, `app/ponto/motor_calculo_v2.py:1591–1597`, `~/wt-codex-feriado-um-juiz-152427/app/ponto/calculador/regras.py:410–432`.

## 27/11 21:11 · sessao 9b1e28f281d8 · - · - · origem=Ronald (tmux)

Censo concluído em leitura. As referências `wt` são da worktree `~/wt-codex-feriado-um-juiz-152427`, no commit `1d2eb496c`. O hash fornecido é ancestral direto da série e o motor ficou sem diff entre os dois pontos. [Verificação Git sem arquivo:linha — nao verificado pelo material citado.]

### Total e rubrica

- Na cópia do hash, o feriado em folga com escala incerta retorna antes do bloqueio e preserva os minutos líquidos. O mensal e o `DiaPago` os registram em `horas_trabalhadas`, sem o adicional de 100%. Um par 07h–11h vale 240 min; a cópia atual não gera evento TXT de `horas_trabalhadas`. `app/ponto/motor_calculo_v2.py:1579-1586`, `app/ponto/services/fechamento.py:999-1009`, `app/ponto/services/dia_pago.py:259-267`, `app/folha/export.py:439-461`.
- Na worktree, desde a comp. 11, o teste pulado usa 6x1 comercial, `trabalha_em_feriado=True`, célula de folga e 07h–11h. Incerta espera 240 min de HE100 feriado. Certa espera 240 min de folga trabalhada. Com `trabalha_em_feriado=False`, a certa também vai para HE100 feriado. O juiz é `particionar_ft` → `_feriado_dobra_no_dia`, `escala_certa_no_dia` e `feriado_vira_folga`. `wt/app/ponto/tests/test_feriado_um_juiz_porta.py:23-38`, `:98-118`, `:547-563`, `wt/app/ponto/calculador/regras.py:371-377`, `:386-453`, `wt/app/folha/export.py:383-394`.
- Os eventos são `hora_extra_feriado_diurna` e `folga_trabalhada_diurna`. O cadastro `RubricaDominio` resolve empresa sobre global. A fixture cadastra `200` para o evento feriado e prova `0200` no TXT. `0243` é o default legado de intrajornada. Código ativo na J.A. [nao verificado]. `app/folha/export.py:439-448`, `:739-748`, `:993-1000`, `app/ponto/models.py:980-987`, `wt/app/ponto/tests/test_feriado_um_juiz_porta.py:151-176`.

### Escritores, leitores e efeito

- O motor escreve as rubricas zeradas e soma trabalhado a `minutos_fora_janela`, colapsa a ponta efetiva, acrescenta o alerta e publica dia/minutos/hora. O carimbo original vem de `_carimbar_segmento` e a publicação sai em `ResultadoMes`. A composição do fechamento concatena a publicação. `app/ponto/motor_calculo_v2.py:1585-1602`, `:1954`, `:2833`, `app/ponto/services/fechamento.py:116-117`.
- `ResultadoMes` soma HE de `periodos`, e noturno e intra de `periodos` + `periodos_ft`. A dobra já escrita no par não gera outro pagamento para FT porque entra apenas em `periodos_ft`. A partição do mensal é disjunta por identidade e a lavra recebe as mesmas listas. O TXT desconta o subconjunto de feriado do total HE100 antes de emitir a HE100 normal. Duplicação reaparece se o mesmo FT entrar em dois destinos ou se o subconjunto for somado ao pai. `app/ponto/motor_calculo_v2.py:294-344`, `:370`, `:2581-2607`, `:2925-2932`, `:3385-3392`, `app/ponto/services/fechamento.py:978-1009`, `:1227`, `app/folha/export.py:420-446`.
- O oráculo lê pontas de **`app/ponto/calculador/alimentacao.py`**, soma `fora_por_dia`, distingue `par_bloqueado_sem_previsto` por dia/hora e descarta o colapso. Com a liberação volta a segmentar a duração efetiva, respeitando o intervalo que o motor descontou. A rubrica vem da partição compartilhada na worktree. Em paralelo, a releitura diagnóstica por período também recusa pontas colapsadas e usa `fora` para o teto. `app/ponto/calculador/alimentacao.py:73-159`, `:314`, `:459-471`, `wt/app/ponto/calculador/alimentacao.py:353-389`, `:512-523`, `app/ponto/calculador/periodo.py:608-706`, `:747-790`.
- O `DiaPago` registra HE100 feriado/FT/trabalhadas conforme o destino, junto de noturno e intra. `pago_do_dia` soma trabalhadas, FT e o detalhe `folga_feriado_min`. A linha só com rubricas zero recebe `None`. Os leitores de tela e conferência usam a lavra e os totais HE, sem outro evento de pagamento. `app/ponto/services/dia_pago.py:223-267`, `:867-928`, `app/ponto/services/espelho.py:421`, `app/folha/porta_export.py:118-121`.
- O espelho traduz a publicação em `he_fora_da_janela` com `sentido='dia'`. Dali saem PDF, retrato, listagem, lote e trava de export. Sem a publicação, o dia deixa de ser pendência integral no próximo retrato. O alerta L-097 também some, mas o motor ainda pode acrescentar `FOLGA TRABALHADA` e o espelho considera qualquer alerta para inconsistência. `app/ponto/services/espelho.py:671`, `:735-763`, `:790`, `:939-948`, `app/ponto/motor_calculo_v2.py:2598-2605`, `app/relatorios/pdf_espelho.py:931-932`, `:975`, `app/ponto/services/he_pendente_lavrado.py:68`, `:138-159`, `app/ponto/services/gestao_he.py:426-432`, `:488-504`, `app/folha/porta_export.py:181-201`, `:229-242`, `:687-697`, `:864-871`.
- A L-093 soma `minutos_trabalhados + minutos_fora_janela`. A mesma parcela mudando de coluna conserva essa soma. `app/ponto/motor_calculo_v2.py:1260-1278`.

### Bordas para levar ao dossiê

- O aviso do aval tem numeração antiga. No vivo, o bloco mutante vai até `1602`, incluindo colapso e publicação. Uma guarda colocada no começo da faixa autorizada consegue evitar todas essas escritas sem editar suas linhas; o sentido literal da faixa é questão de lei caso se exija alterar diretamente o colapso. `app/ponto/motor_calculo_v2.py:1574-1602`.
- O teste existente que bloqueia feriado em folga é **12x36 em outubro**, portanto constitui controle se o corte vigente de 12x36 for preservado. Para inverter use o RED pulado de novembro 6x1. A worktree já sela outubro 6x1 incerto comum e novembro 6x1 incerto HE100 feriado. `app/ponto/tests/test_he100_dia_sem_previsto.py:39-48`, `:175-212`, `wt/app/ponto/tests/test_he100_dia_sem_previsto.py:227-268`, `wt/app/ponto/tests/test_feriado_um_juiz_porta.py:547-563`.
- Uma exceção por `eh_feriado` puro alcançaria o 12x36 em folga sem previsto; já o 12x36 em plantão com previsto positivo sai cedo. O intermitente limpa inclusive o calendário injetado, de modo que percorre o método, mas o feriado não ativa o filtro como feriado. `app/ponto/motor_calculo_v2.py:1562-1568`, `:2558-2574`, `:3686-3698`.
- Sem FT e com previsto 0, o feriado entra no bloqueio sem consultar certeza. Essa combinação pode nascer de marcos incompletos. Com previsto positivo, a regra integral não entra. A frase ampla do aval deixa em aberto se muda também o primeiro caso e se a janela ordinária do segundo permanece aplicável às pontas quando o dia dá dobra. `app/ponto/motor_calculo_v2.py:1562-1586`, `:1634-1660`, `app/escala/utils.py:2310-2325`.
- Tem mais uma borda de emissão a testar se “paga” for exigido ponta a ponta: desde comp. 11, a exceção ao gate de `horas_trabalhadas=0` é `feriado_emitivel`, condicionada à rubrica de feriado cadastrada. Ela não cobre um mensal todo zerado em trabalhadas que só tenha a rubrica de FT, que é um dos destinos do RED pulado. O próprio RED confere mensal, enquanto outros testes conferem TXT. `wt/app/folha/export.py:924-926`, `wt/app/ponto/tests/test_feriado_um_juiz_porta.py:151-176`, `:547-563`.

## 27/11 21:10 · sessao 3cbd4a5f872b · - · - · origem=Ronald (tmux)

- O RED da obra em voo está suspenso em `test_feriado_um_juiz_porta.py:547-563`. A fixture usa 6x1 comercial, célula de folga em 02/11 e batidas 07h–11h. Para escala incerta, espera 240 min em HE100-feriado. Para certa e `trabalha_em_feriado=True`, espera 240 min em FT. Para certa e `trabalha_em_feriado=False`, o controle espera 240 min em HE100-feriado. A função `_confere` também verifica export, mensal, lavra motor e oráculo, e a soma única de quatro horas. `test_feriado_um_juiz.py:9` e `test_feriado_um_juiz_porta.py:16-118`.
- O teste que fixa o bloqueio do feriado na base é `app/ponto/tests/test_he100_dia_sem_previsto.py:208-212`, com fixture **12x36** e competência 10, `:39-48,179-193`. Se o corte preservar esse bloqueio de 12x36 em folga, conservar a asserção e acrescentar o RED comercial em competência 11. A obra em voo já fixa 330 min de HE100-feriado para 6x1 incerta em novembro, e 330 min de trabalhadas para 12x36 incerta ou 6x1 incerta em outubro. `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_he100_dia_sem_previsto.py:227-268`.
- A folga comum certa, 06h30–12h, mantém zero, 330 min fora, publicação do dia e pendência `sentido='dia'`. O oráculo descarta o par colapsado e conta `par_bloqueado_sem_previsto`. No feriado liberado, verificar o mesmo carimbo não colapsado e total 240 em `pares_por_dia`, sem esse contador e sem a publicação de pendência do bloco. `app/ponto/tests/test_he100_dia_sem_previsto.py:55-81,120-123,151-159,259-275,294-297`, `app/ponto/calculador/alimentacao.py:73-95,114-123,157-159`.
- Para feriado com célula trabalha=True, marcos completos e previsto positivo, o bloco já retorna. O ciclo 6x1 dobra pelo juiz. Com flag de folga falsa e previsto 0 por marcos incompletos ou `trabalha=None`, mesmo feriado de escala incerta hoje bloqueia. O aval não resolve inequivocamente a diferença entre a ordem de pagar feriado direto e o alcance explícito da exceção de folga certa. `app/escala/models.py:1098-1107`, `app/escala/utils.py:2319-2325`, `app/ponto/motor_calculo_v2.py:1562-1584,2560-2574,3465-3478`, `app/ponto/tests/test_feriado_sumula146.py:59-79`.
- O 12x36 com plantão previsto e janela ligada tem controle CCT com 720 min trabalhados, 720 min de HE100-feriado e sem pendência. Sem cláusula, a fixture 59-A fixa hora simples e alerta próprio. No intermitente a fixture 07h–19h com feriado oficial e janela ligada fixa 720 min trabalhados, HE100 zerada e nenhuma pendência. O corte de calendário injetado vazio neutraliza feriado, mas uma folga semanal de cadastro pode entrar pelo `elif dias_folga`; ocorrência em produção [nao verificado]. `app/ponto/tests/test_s6_intermitente_feriado.py:105-152,168-177`, `app/ponto/tests/test_feriado_12x36_art59a.py:156-169,226-234`, `app/ponto/motor_calculo_v2.py:2436-2451,2475-2478,2560-2566,3694-3698,1000-1011`.
- A borda `@expectedFailure` da obra em voo traz “PAREI ! FERIADO-L097-MOTOR”, mas o helper desliga a janela e força certeza falsa. O teste não verifica o bloqueio L-097. `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:903-922,942-959`. [nao verificado]
- O alerta genérico de FT sobre validar antes do fechamento é gerado depois do bloco e permanece visível se a cura sair apenas da L-097. A pendência da Gestão de HE vem de `dias_sem_previsto_fora_da_janela`, uma lista distinta. `app/ponto/motor_calculo_v2.py:2598-2605`, `app/ponto/services/espelho.py:669-671,757-760`, `app/templates/ponto/espelho.html:302-304`, `app/relatorios/pdf_espelho.py:973-977`.
- A obra testa 0200 para HE100-feriado com mapeamento explícito. As migrações põem FT e HE100 normal em 200 global, e a empresa pode substituir o global. 0243 é padrão de intrajornada. Código real da rubrica para cada empresa e lista de dias da competência 11 [nao verificado]. `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:151-176`, `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:1-11`, `app/folha/migrations/0009_evento_he100_rubrica_200.py:1-10`, `app/folha/export.py:993-1000`, `app/ponto/models.py:987`.

## 27/11 21:05 · sessao 96fd5c7b7b0f · FIM-DE-SEMANA-JA · leitura · origem=Code

## 1. Dono L‑099 por linha

**11 CADASTRO, 7 ESTRUTURA, 0 BATIDA**, em 18 dias. As referências a `sombra.out` contam a linha `sonda_leitura` como linha 1. O `dono_motivo` veio `-` nas 18 linhas. O motivo abaixo vem de seguir os insumos até o retorno de `dono_da_divergencia`, chamado por `InsumosDoDono.dono`. `sombra.out:1–21`, `app/docs/LEIS.md:122`, `app/ponto/management/commands/e6_oraculo.py:182–195`.

- **Vínculo ausente.** `sem_vinculo_no_dia` e `sem_vinculo_em_dia_de_trabalho` vieram `true`. Esse ramo prevalece sobre as batidas. A geradora nomeada pela célula estava fora da vigência do dia, e a leitora do vínculo devolve `None` nessa condição. `app/ponto/management/commands/e6_oraculo.py:98–99`, `app/escala/alimentacao.py:269–284`, `app/ponto/supra_juiz.py:206–210`.
- **Retorno residual.** Os três indicadores de cadastro do primeiro ramo vieram `false`, com zero ou quatro horas no oráculo, sem batida fora de turno, fase deslocada ou presença do dia na lavra CxR. O último retorno dá `ESTRUTURA`. `sombra.out:14–21`, `app/ponto/management/commands/e6_oraculo.py:98–110`.

| Colab · dia de 2026 | Dono | Evidência do dia e ramo decisivo |
|---|---|---|
| col97 · 01/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:4`, `e6_oraculo.py:98–99`. |
| col97 · 03/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:5`, `e6_oraculo.py:98–99`. |
| col97 · 05/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:6`, `e6_oraculo.py:98–99`. |
| col97 · 07/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:7`, `e6_oraculo.py:98–99`. |
| col97 · 09/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:8`, `e6_oraculo.py:98–99`. |
| col97 · 11/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:9`, `e6_oraculo.py:98–99`. |
| col97 · 13/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:10`, `e6_oraculo.py:98–99`. |
| col97 · 15/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:11`, `e6_oraculo.py:98–99`. |
| col97 · 17/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:12`, `e6_oraculo.py:98–99`. |
| col97 · 19/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:13`, `e6_oraculo.py:98–99`. |
| col296 · 10/11 | ESTRUTURA | `COBRANCA_SEM_FURO`, 660 previstos e 660 realizados. Quatro horas no oráculo e retorno residual. `sombra.out:14`, `e6_oraculo.py:110`. |
| col327 · 10/11 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 60 min sem início e fim. Quatro horas e retorno residual. `sombra.out:15`, `e6_oraculo.py:110`. |
| col344 · 07/11 | ESTRUTURA | Célula `concorde`, agendamento 103 cobre o dia e há batida civil apurável. Quatro horas e retorno residual. `sombra.out:16`, `e6_oraculo.py:110`. |
| col348 · 20/11 | CADASTRO | Célula editada, indefinida, sem vínculo no dia, com 712 min realizados. `sombra.out:17`, `e6_oraculo.py:98–99`. |
| col354 · 21/10 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 120 min sem início e fim. Quatro horas e retorno residual. `sombra.out:18`, `e6_oraculo.py:110`. |
| col407 · 29/10 | ESTRUTURA | `FURO_PARCIAL`, 720 previstos e 0 realizados. Oráculo vazio e retorno residual. `sombra.out:19`, `e6_oraculo.py:106–110`. |
| col655 · 21/10 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 180 min sem início e fim. Quatro horas e retorno residual. `sombra.out:20`, `e6_oraculo.py:110`. |
| col673 · 21/10 | ESTRUTURA | `FURO_PARCIAL`, 720 previstos e 3 realizados. Oráculo vazio e retorno residual. `sombra.out:21`, `e6_oraculo.py:106–110`. |

Nos 11 casos de vínculo, a célula traz `PREVISTO_COM_DIA_INDEFINIDO`, e `cadastro.falta_previsto` aponta um vínculo que responda pela data. O retrato lavrado inclui as datas dos 11. Para col97, a lista de portas do retrato veio vazia. Para col348, trouxe **Vínculo da escala** e duas entradas **Resolver dia no calendário**. Essas portas vêm da linha do colaborador na competência. `sombra.out:4–13`, `sombra.out:17`, `app/folha/retrato_folha.py:930–935`, `app/folha/retrato_folha.py:1024–1059`.

**Onde o S2 divergiu.** O S2 tratou o protesto `PARCIAL_SEM_PERIODO` como se já fosse a decisão L‑099. O supra-juiz identifica corretamente o campo faltante e manda cobrar o cadastro. O cartório também prevê encaminhar a parcial sem período à lista do admin. Para a **contagem pedida**, a L‑099 aponta para `dono_da_divergencia`. Esse juiz recebe vínculo, folga, batidas, fase e CxR, e os três parciais caíram no retorno `ESTRUTURA`. Os dois `FURO_PARCIAL` tinham dono do fato em aberto no S2 e agora têm `ESTRUTURA` medido. O S2 já descrevia col296 como estrutural na seção de cobrança, mas o deixou fora da soma C1 de 14 + 2 + 1. A S3 também identificou nominalmente o caso de férias e mediu seu dono. `S2_r.md:11–14`, `app/docs/LEIS.md:122`, `app/ponto/supra_juiz.py:395–402`, `app/ponto/services/cartorio.py:423–446`, `app/ponto/management/commands/e6_oraculo.py:53–54`, `app/ponto/management/commands/e6_oraculo.py:98–110`, `sombra.out:14–21`.

## 2. Lista para o Mauricio

**Vínculo da escala**, formulário **Vincular Escala**, é a tela indicada em `tela.previsto` para cada linha abaixo. URL name `colaboradores:painel_vinculo`, template `colaboradores/partials/painel_vinculo.html`. `app/folha/retrato_folha.py:1003–1006`, `app/colaboradores/urls.py:12–13`, `app/colaboradores/views.py:306`, `app/templates/colaboradores/partials/painel_vinculo.html:128–137`.

| Colab | Dia de 2026 | Motivo para o DP e cadastro faltante | Tela |
|---|---|---|---|
| col97 | 01/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:4`. | Vínculo da escala |
| col97 | 03/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:5`. | Vínculo da escala |
| col97 | 05/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:6`. | Vínculo da escala |
| col97 | 07/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:7`. | Vínculo da escala |
| col97 | 09/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:8`. | Vínculo da escala |
| col97 | 11/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:9`. | Vínculo da escala |
| col97 | 13/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:10`. | Vínculo da escala |
| col97 | 15/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:11`. | Vínculo da escala |
| col97 | 17/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:12`. | Vínculo da escala |
| col97 | 19/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:13`. | Vínculo da escala |
| col348 | 20/11 | Houve trabalho registrado e falta vínculo de escala que cubra o dia. A geradora intermitente terminou em 20/10. `sombra.out:17`. | Vínculo da escala |

## 3. ESTRUTURA, ato faltante, escritor e obra

Nas sete linhas, a S3 encontrou `colab_na_lista=true`, `dia_na_lista=false`, `dono_lavrado=null`, `mesa_admin=false` e `destino_DP=-`. A consulta à lista distingue a presença do colaborador da presença daquele dia. Em todas, o calendário da tarefa veio `false` e o retrato de 27/11 inclui a data. `sombra.out:14–16`, `sombra.out:18–21`, `app/chamados/services/vigia_fio.py:202–241`.

| Caso · célula | Linha C1 preenchida com a evidência disponível | Ato, escritor canônico e BACKLOG |
|---|---|---|
| **col296 · 10/11** · 137154 | Chamado 38343 `turno_aberto` ainda aberto, 38318 resolvido. Perguntas 56532 e 56545 ancoradas em 38343, C6 `true`, sem resposta ou ato. A 56612 tem C6 `false` e projeção `fantasma`. Tarefa sem porta e fila vazia. Retrato com **Cobrar no fio** e **Vínculo da escala**. `sombra.out:14`. | Baixa de 38343 e rejulgamento, **se o gate autorizar**. O cartório chama a reconciliação em `app/ponto/services/cartorio.py:_julgar_colab_corpo:1618–1629`. A baixa passa por `app/chamados/models.py:ChamadoColaborador.reconciliar:661–675`. **E4B-SCA-25-COBRANCA-INDEVIDA** é a obra da classe. Inclusão nominal no estoque dos 25 e motivo da permanência [nao verificado]. Faltam `vivos`/`_protestam` da lavra e resultado ou recusa da reconciliação de 38343. `app/docs/BACKLOG.md:2318`, `app/ponto/services/cartorio.py:1498–1507`, `app/chamados/models.py:683–712`. |
| **col327 · 10/11** · 122970 | Declaração 5515, 60 min, `hora_ini=null`, `hora_fim=null`. Chamado 38375 `ausencia_colab` resolvido. Sem pergunta, ato, porta ou link no retrato. `sombra.out:15`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col344 · 07/11** · 136655 | Agendamento 103, de 06/10 a 07/11, concluído, aquisitivo 639 concedido. Batida civil 148780, `E`, às 18:33:49 locais. Chamado 37604 registrado. Pergunta 55062, âncora e fila 37605, C6 `true`. `porta_pergunta` publica `validar` com `nao_sei=true`, e a instância está `passivo_sem_linha`. Retrato com **Validar no fio**. `sombra.out:16`. | No fio, falta `DecisaoPergunta` de **resposta** para 55062 em `2026-11-07T21:33:56.900522+00:00`. Escritor do passivo abaixo. Para férias, **HJ‑090, item 7**, é a obra da lista com dono L‑099. Ela referencia `CASOS-103-114-PELA-PORTA`, cuja decisão depende das datas do DP. Falta a linha `Ausencia(tipo=ferias)` de 07/11, com status e agendamento de origem, para confirmar entrada no relatório da obra. A correção do agendamento ou da batida fica [nao verificado]. `app/docs/BACKLOG.md:2137`, `app/docs/BACKLOG.md:751`, `app/ferias/management/commands/listar_ferias_com_batida.py:1–12`, `app/ferias/management/commands/listar_ferias_com_batida.py:41–65`. |
| **col354 · 21/10** · 128888 | Declaração 5125, 120 min, `hora_ini=null`, `hora_fim=null`. Sem chamado, pergunta ou ato. Tarefa sem porta. Retrato com **Resolver dia no calendário**. `sombra.out:18`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col407 · 29/10** · 133670 | Chamado e âncora 35801 fechados. Pergunta 51727, C6 `true`, sequência vazia, resposta só na projeção. Instância `passivo_sem_linha`, fio `nao_sei`, sem rota ou fila. Retrato com duas entradas **Resolver dia no calendário**. `sombra.out:19`. | Falta `DecisaoPergunta` de **resposta** para 51727 em `2026-11-01T10:02:34.925352+00:00`. **FIO-E0-V4-E4B** é a obra do passivo. Escritor abaixo. `app/docs/BACKLOG.md:2315`. |
| **col655 · 21/10** · 129497 | Declaração 5076, 180 min, `hora_ini=null`, `hora_fim=null`. Chamados 33158 e 33163 resolvidos, com 33158 no cartório. Sem pergunta ou ato. Tarefa sem porta. Retrato com **Cobrar no fio**. `sombra.out:20`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col673 · 21/10** · 135708 | Âncora 33282 fechada. 33494 resolvido, 34029 fechado. A pergunta 46935 tem C6 `true`, sequência vazia e `passivo_sem_linha`. A 46936 tem C6 `false` e projeção materializada por `declaracao`. Fio `nao_sei`, sem rota, fila ou link no retrato. `sombra.out:21`. | Falta `DecisaoPergunta` de **resposta** para 46935 em `2026-10-22T10:08:24.022007+00:00`. **FIO-E0-V4-E4B** é a obra do passivo. O BACKLOG nomeia col673 e a pergunta **46936** em **DIA-DA-BATIDA-JUIZ-UNICO**, para a atribuição da batida ao turno. `app/docs/BACKLOG.md:2315`, `app/docs/BACKLOG.md:1864`. |

**Três parciais.** O ato de cadastro é salvar o início e o fim declarados pelo DP no painel `ponto:painel_ausencia`, template `ponto/partials/painel_ausencia.html`. O POST `ponto:editar_periodo_ausencia` chama `decidir`, que grava pela função `app/ponto/services/ausencia.py:_aplicar_periodo:1051–1054` e persiste em `app/ponto/services/ausencia.py:_decidir_no_ato:1101–1104`. Para o encaminhamento ausente, **COBPARC-B-CAUSA-NO-MOTOR** descreve a classe. O escritor atual das causas CxR é `app/ponto/motor_calculo_v2.py:_acusa_cadastro_x_realidade:1766–1783`, com chamadas nas pontas da janela de HE. A ausência das três datas na lista está medida. A causa exata da omissão e o escritor que deveria publicar essas três linhas ficam [nao verificado]. Faltam a assinatura por `(colab, dia)` na origem da lista e o resultado da sua publicação. **BO2910-ESTRUTURA (05)** cobre a aprovação pela tela sem exigir período. Para atribuir essa falha histórica a uma das três declarações, faltam canal e trilha da aprovação. `app/ponto/urls.py:68–72`, `app/ponto/views.py:2836–2849`, `app/ponto/services/cartorio.py:423–446`, `app/ponto/motor_calculo_v2.py:1822–1824`, `app/ponto/motor_calculo_v2.py:1860–1862`, `app/docs/BACKLOG.md:1105`, `app/docs/BACKLOG.md:1191`, `sombra.out:15`, `sombra.out:18`, `sombra.out:20`.

**Três respostas sem linha.** O escritor canônico é `app/chamados/models.py:DecisaoPergunta.registrar:4590–4609`. O backfill despacha a recomposição em `app/chamados/management/commands/backfill_fio_morto_e4b.py:_executar_dia:161–164`. A correspondência exige a mesma pergunta, o mesmo ato e o mesmo instante. **E4C-F8** trata da migração de leitores da projeção para a leitura do ato. A inclusão nominal de 51727 ou 46935 nas nove unidades dessa obra fica [nao verificado], pois falta identificar o leitor específico na lista de escopo da F8. `app/chamados/models.py:4416–4447`, `app/docs/BACKLOG.md:2211`, `app/docs/BACKLOG.md:2315`, `sombra.out:16`, `sombra.out:19`, `sombra.out:21`.

**Férias de col344.** A Folha já publica a retenção a partir do agendamento e do dia civil da batida. A S3 incluiu o caso pelo ramo sem porta **do tipo** `ferias`. A tarefa e o retrato puderam publicar **Validar no fio** pela pergunta. O ato de férias ainda depende de confirmar o gozo real, o dia de turno, a origem da batida e a decisão do DP. Se o DP confirmar datas diferentes, o escritor das datas é `app/ferias/services.py:_trocar_janela:694–729`. A tela encontrada é `ferias:secao_ferias_colab`, template `ferias/_secao_colab.html`. `s3.py:92–97`, `sombra.out:16`, `app/folha/export.py:490–504`, `app/folha/export.py:885–893`, `app/folha/retrato_folha.py:1003–1017`, `app/ferias/views.py:851–861`.

## 4. O que segue [nao verificado]

- **Identidade Git da cópia, execução independente da S3 e equivalência com produção.** A cópia consultada está sem metadados Git. O cabeçalho do anexo identifica a sonda, e o código traz as guardas de ambiente da sombra. O resultado disponível é a saída raspada fornecida. `sombra.out:1–3`, `s3.py:16–22`. [nao verificado]
- **Valores corretos e histórico das correções.** Faltam o vínculo e a vigência corretos para col97/col348, os horários declarados das três parciais, o retorno histórico do gate de 38343 e a decisão do DP sobre a sobreposição de férias. A saída mostra o estado consultado e a sobreposição civil. `sombra.out:4–21`, `app/chamados/models.py:683–712`, `app/folha/export.py:490–504`. [nao verificado]
- **Origem e elegibilidade das três respostas sem linha.** Faltam o escritor original, a trilha do ato, a presença no plano do backfill e o estado da competência no gate. A sonda mede a falta da linha, e o backfill ainda aplica seus critérios de alcance, competência e entrega. `sombra.out:16`, `sombra.out:19`, `sombra.out:21`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:307–343`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:383–405`. [nao verificado]
- **Alcance por um ator e efeito na Folha.** O anexo traz rótulo e URL name das portas do retrato. Faltam o destino completo de cada link, o perfil e a autorização do ator, o resultado da ação e uma nova classificação da Folha. Liberação do TXT e efeito em pagamento ficam [nao verificado]. `sombra.out:14–21`, `app/folha/retrato_folha.py:1024–1065`, `app/folha/export.py:699`, `app/folha/export.py:774`.

## 27/11 20:59 · sessao 5e148d3706d5 · FERIADO-UM-JUIZ · construtor · origem=Code

A c11 está na cópia. O RED de FT passou na cura e falhou nos três subcasos do mutante com `ValueError not raised`. O teste de `do_motor` também passou na cura e falhou na c10 e no mutante quando o dia estava sem entrada em `rubricas`. [RED de FT](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:395) · [RED de `do_motor`](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:418) [nao verificado]

### Diff por arquivo

| Arquivo | Diff | Alteração |
|---|---:|---|
| [dia_pago.py:316](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/dia_pago.py:316) | +4 −6 | Uma chamada a `_DONOS_DECLARADOS` por dia, com `do_motor` nos dois casos. Saiu o `or CAMPOS_DIA`. |
| [fechamento.py:682](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/fechamento.py:682) | +1 −0 | Comentário do contador registra recusas por colaborador e competência e remete ao R1. |
| [test_feriado_um_juiz.py:395](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:395) | +45 −0 | REDs de FT isolado e de `do_motor`. O R7 ganhou a docstring de [caracterização](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:476). |
| [feriado_um_juiz_mutantes.py:167](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:167) | +15 −4 | Dois mutantes novos e ajuste do alvo textual do [M1](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:484). |

**`except` no diff dos quatro arquivos acima: 0.** O controle local rodou 52 testes, com uma falha esperada. O M1 produziu as duas falhas previstas. [Controle](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:716) · [Alvos do M1](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:122) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes controle
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes c11_foto_sem_dia_so_com_ft
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes c11_ignora_do_motor_sem_rubrica
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M1
```

### `--only` para o Code

Da pasta `app/`. Suite e savepoint real: **[nao verificado]**. [Teste do savepoint real](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:462) [nao verificado]

```bash
../bin/suite.sh --dir /home/ronald/wt-codex-feriado-um-juiz-152427 \
  --only "ponto.tests.test_feriado_um_juiz ponto.tests.test_feriado_um_juiz_porta"
```

## 27/11 20:58 · sessao 7c0cb70cf686 · FIM-DE-SEMANA-JA · leitura · origem=Code

A S1 registrou 18 ocorrências em 9 colaboradores. `col296`, em 10/11, está entre as 17 linhas nominais. A ocorrência adicional é `ferias_com_batida`, publicada apenas no total. A identidade, o dia e as portas dessa ocorrência, assim como o dono L‑099 de cada linha, ficam [nao verificado]. A identidade Git da cópia e a execução da S3 também ficam [nao verificado]. `c1_s1.txt:26`, `c1_s1.txt:2972`, `c1_s1.txt:2980`, `c1_s1.txt:3006`. [nao verificado]

## Peça 1 — Sonda S3

A seleção conserva o teste de saída da S1. A sonda lê as portas do retrato gravado depois de selecionar o universo, pois o drawer também consulta o fio e pode publicar um link genérico em férias. `dono=` é a resposta de `InsumosDoDono.dono`, que chama `dono_da_divergencia` com os insumos da casa e a lavra CxR, seguindo o caminho do relatório de férias. A S3 usa um `AGORA` e um `HOJE`. A classificação do export mantém o relógio interno já documentado no caminho da S1. `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1010`, `app/folha/retrato_folha.py:1066`, `app/colaboradores/views.py:1001`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:53`, `app/ferias/management/commands/listar_ferias_com_batida.py:53`, `app/folha/fora_da_folha.py:137`.

```python
import sys

sys.dont_write_bytecode = True

import json
import logging
from collections import defaultdict
from datetime import datetime, time
from functools import lru_cache
from urllib.parse import urlsplit

from django.conf import settings
from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context

if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from django.contrib.auth.models import User
from django.db.models import Q
from django.urls import resolve, reverse

from chamados.juizes import dia_do_fato, pergunta_viva
from chamados.models import ChamadoColaborador, DecisaoPergunta, PerguntaDisputa
from chamados.services.fio import chamado_que_lista
from chamados.services.tarefa_do_dia import porta_da_tarefa, tarefa_do_dia
from chamados.services.vigia_fio import ListaDoAdmin, dono_fora_do_fio
from colaboradores.models import Colaborador, Empresa
from colaboradores.queries import empresas_visiveis
from colaboradores.services.calendario import resolver_dia_oferecido_nas_datas
from escala.alimentacao import celulas_do_periodo, vinculo_do_dia_carregado
from ferias.services import ferias_na_janela
from folha.fora_da_folha import _carga, _fio_da_porta, dias_que_seguram
from folha.models import RetratoFolha
from folha.retrato_folha import (
    PORTA_DO_TIPO, _balde, linha_do_colab, portas_da_linha, tipo_da_linha,
)
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import InsumosDoDono, batida_fora_de_turno
from ponto.services.cartorio import chamados_que_falam_de_batida
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.supra_juiz import cadastro_do_dia_carregado
from ponto.turnos import (
    batidas_apuraveis, cobre_parte_do_dia, cobertura_ausencia_periodo,
    periodo_da_parcial_no_dia,
)

EMPRESA_ID = 2
MES, ANO = 11, 2026
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
ESPERADO = 18
NV = "[nao verificado]"
FONTE_DONO = "app/ponto/management/commands/e6_oraculo.py:182,53"
ESCRITOR_PASSIVO = (
    "app/chamados/management/commands/backfill_fio_morto_e4b.py:164",
    "app/chamados/models.py:4590",
)


def origem_s1(empresa, inicio, fim, lista):
    # Mesmo critério de pertencimento da S1 c1. As leituras extras vêm depois.
    u = dias_que_seguram(empresa, MES, ANO, hoje=HOJE)
    itens = sorted(
        (i for i in u["itens"] if _balde(i) == "bloqueado"),
        key=lambda i: i["colaborador_id"],
    )
    datas = {
        cid: [r["data"] for r in dias if inicio <= r["data"] <= fim]
        for cid, (_, dias) in u["dias"].items() if dias
    }
    carga = _carga([u["colabs"][cid] for cid in datas], inicio, fim, AGORA) if datas else None
    cel_ids = {
        r["celula_id"] for _, dias in u["dias"].values() for r in dias
        if inicio <= r["data"] <= fim and r["celula_id"] is not None
    }
    perguntas = defaultdict(list)
    for p in PerguntaDisputa.objects.filter(celula_id__in=cel_ids).select_related(
        "disputa__colaborador__empresa", "celula"
    ).prefetch_related("decisoes"):
        perguntas[p.celula_id].append(p)

    casos = []
    for item in itens:
        cid = item["colaborador_id"]
        if item["motivo"] != "furo_espelho":
            if not PORTA_DO_TIPO.get(tipo_da_linha(item)):
                for dia in item.get("dias_ferias_com_batida") or (None,):
                    if dia is None or inicio <= dia <= fim:
                        casos.append((item, dia, None, None))
            continue

        colab = u["colabs"][cid]
        _, dias = u["dias"][cid]
        if not dias:
            casos.append((item, None, None, None))
        calendario = resolver_dia_oferecido_nas_datas(colab, datas.get(cid, ()), agora=AGORA)
        for r in dias:
            dia, cel_id = r["data"], r["celula_id"]
            if not inicio <= dia <= fim:
                continue
            tarefa = tarefa_do_dia(colab, dia, AGORA, carga=carga)
            publicada = porta_da_tarefa(tarefa) [nao verificado]
            fio = _fio_da_porta(publicada) [nao verificado]
            fora, papel = dono_fora_do_fio(colab, dia, perguntas[cel_id], lista, com_papel=True)
            lista.dia_na_lista(cid, dia.isoformat())
            mesa = lista.dia_do_admin(cid, dia.isoformat())
            destino = lista.destino_fora_da_mesa(cid, dia.isoformat())
            cal = calendario[dia]
            if (
                ((publicada or {}).get("rota") and fio in ("admin", "colab")) [nao verificado]
                or cal is True or fora or mesa or destino
            ):
                continue
            casos.append((item, dia, r, (tarefa, publicada, fio, cal, fora, papel))) [nao verificado]
    return casos


def campos(obj, *nomes):
    return {nome: getattr(obj, nome) for nome in nomes} if obj is not None else None


def tela(nome, template, post=None, args=(), **dados):
    out = {"url": resolve(reverse(nome, args=args)).view_name, "template": template}
    if post is not None:
        out["post"] = resolve(reverse(post, args=args)).view_name
    out.update(dados)
    return out


def dados_vinculo(ec):
    if ec is None:
        return None
    return {
        **campos(ec, "pk", "ativa", "data_inicio", "data_fim", "data_ancora_colaborador"),
        "escala": campos(ec.tipo_escala, "pk", "tipo_ciclo", "folga_dia_semana", "editavel"),
    }


def cadastro_e_tela(colab, dia, cel, protestos, cadastro):
    cid = colab.pk
    dados, telas = {}, {}
    if "PREVISTO_COM_DIA_INDEFINIDO" in protestos:
        ec = vinculo_do_dia_carregado(cid, dia)
        geradora = cel.escala_geradora if cel is not None and cel.escala_geradora_id else None
        dados["vinculo"] = dados_vinculo(ec)
        dados["geradora"] = dados_vinculo(geradora)
        telas["previsto"] = []
        if cadastro["sem_vinculo_no_dia"]:
            dados["falta_previsto"] = "vinculo_que_responda_pelo_dia"
            telas["previsto"].append(tela(
                "colaboradores:painel_vinculo", "colaboradores/partials/painel_vinculo.html",
                "colaboradores:vincular_escala", args=(cid,),
            ))
        if cadastro["folga_nao_definida"]:
            dados["falta_previsto"] = ["folga_semanal_no_tipo", "FolgaDia_no_mes"]
            if ec.ativa:
                telas["previsto"].append(tela(
                    "escala:plano_folgas", "escala/plano_folgas.html",
                    "escala:plano_folgas_aplicar",
                    filtros={"empresa": EMPRESA_ID, "ano": dia.year, "mes": dia.month},
                ))
            if ec.tipo_escala.editavel:
                telas["previsto"].append(tela(
                    "escala:wizard_tipo_editar", "escala/wizard_tipo_escala.html",
                    "escala:wizard_salvar_editar", args=(ec.tipo_escala_id,),
                ))
        if not telas["previsto"]:
            telas["previsto"] = NV
        dados.setdefault("falta_previsto", NV)

    if "PARCIAL_SEM_PERIODO" in protestos:
        a = cobertura_ausencia_periodo(colab, dia, dia, para="folha").get(dia)
        dados["ausencia_folha"] = campos(
            a, "pk", "tipo", "data", "data_fim", "status", "minutos", "hora_ini", "hora_fim",
        )
        dados["falta_periodo"] = NV
        telas["parcial"] = NV
        if (
            a is not None and a.status == "aprovada" and cobre_parte_do_dia(a)
            and periodo_da_parcial_no_dia(a, ()) == "sem_periodo"
        ):
            dados["falta_periodo"] = [k for k in ("hora_ini", "hora_fim") if getattr(a, k) is None]
            telas["parcial"] = tela(
                "ponto:painel_ausencia", "ponto/partials/painel_ausencia.html",
                "ponto:editar_periodo_ausencia", args=(a.pk,),
            )
    return dados, telas


def perguntas_no_dia(cid, dia, cel_id, instancias):
    # Relação da célula, relação por fato usada pela tarefa e IDs que ela publicou.
    q = Q(colaborador_id=cid, conversa=False, data_evento=dia)
    if cel_id is not None:
        q |= Q(celula_id=cel_id)
    q |= Q(pk__in=[i["pergunta_id"] for i in instancias if i["pergunta_id"] is not None])
    return list(PerguntaDisputa.objects.filter(q).select_related(
        "disputa__colaborador__empresa", "disputa__chamado", "chamado_destino", "celula",
    ).prefetch_related("decisoes").order_by("pk"))


def ato_e_projecao(perguntas, instancias):
    passivo = {i["pergunta_id"] for i in instancias if i["razao"] == "passivo_sem_linha"}
    faltantes = defaultdict(list)
    for ln in DecisaoPergunta.linhas_da_projecao_sem_linha([p for p in perguntas if p.pk in passivo]):
        if ln["ato_em"] <= AGORA:
            faltantes[ln["pergunta_id"]].append({"ato": ln["ato"], "em": ln["ato_em"]})
    out = []
    for p in perguntas:
        lido = DecisaoPergunta.composta(p)
        foto = {
            "pergunta": p.pk,
            "sequencia_gravada": [campos(d, "pk", "ato", "ato_em") for d in p.decisoes.all()],
            "projecao": campos(p, "respondida_em", "validada_em", "materializada_em", "via_resolucao"),
            "leitura_composta": campos(lido, "respondida_em", "validada_em", "materializada_em", "via_resolucao"),
        }
        if p.pk in passivo:
            foto["atos_faltantes"] = faltantes.get(p.pk) or NV
            foto["escritor_passivo"] = ESCRITOR_PASSIVO
        out.append(foto)
    return out


def dados_ferias(colab, dia):
    de = timezone.make_aware(datetime.combine(dia, time.min))
    ate = timezone.make_aware(datetime.combine(dia, time.max))
    ags = ferias_na_janela(dia, dia, colaborador_ids=[colab.pk]).select_related(
        "periodo_aquisitivo"
    ).order_by("pk")
    return {
        "agendamentos": [
            {
                **campos(ag, "pk", "data_inicio", "data_fim", "status"),
                "aquisitivo": campos(ag.periodo_aquisitivo, "pk", "data_inicio", "data_fim", "status"),
            }
            for ag in ags
        ],
        "batidas_apuraveis_civis": [
            {"pk": b.pk, "hora_local": timezone.localtime(b.timestamp), "tipo_gravado": b.tipo}
            for b in batidas_apuraveis(colab, de, ate).order_by("timestamp", "pk")
        ],
        "tela_do_modulo": tela("ferias:secao_ferias_colab", "ferias/_secao_colab.html", args=(colab.pk,)),
    }


def portas_retrato(retrato, cid, dia):
    linha = linha_do_colab(retrato, cid)
    if linha is None:
        return {"portas": NV, "motivo": "sem_linha_no_retrato_visivel"}
    return {
        "lavrado_em": retrato.lavrado_em,
        "motivo_lavrado": linha.get("motivo"),
        "dia_nas_datas_lavradas": str(dia) in linha["datas"] if linha.get("datas") is not None else NV,
        "portas": [
            {"url": resolve(urlsplit(p["href"]).path).view_name, "rotulo": p["rotulo"]}
            for p in portas_da_linha(linha)
        ],
    }


def molede_linhas(empresa, inicio, fim):
    lista = ListaDoAdmin(HOJE, [empresa], agora=AGORA)
    casos = origem_s1(empresa, inicio, fim, lista)
    colabs = Colaborador.objects.select_related("empresa").in_bulk(
        {i["colaborador_id"] for i, _, _, _ in casos}
    )
    cxr_da_competencia = cxr_lavrado()
    retrato = RetratoFolha.objects.filter(
        empresa_id=empresa.pk, mes=MES, ano=ANO, empresa__in=empresas_visiveis(),
    ).first()

    @lru_cache(maxsize=None)
    def cadastro_atual(cid, dia):
        return cadastro_do_dia_carregado(cid, dia)

    @lru_cache(maxsize=None)
    def fonte_dono(cid):
        cxr = cxr_da_competencia(cid, inicio)
        ins = InsumosDoDono(
            colabs[cid], inicio, fim, HOJE, agora=AGORA, sem_motor=True, cxr=cxr,
            cadastro=lambda dia: cadastro_atual(cid, dia),
        )
        return ins, cxr

    @lru_cache(maxsize=None)
    def chamados_do_colab(cid):
        por_dia = defaultdict(list)
        for ch in ChamadoColaborador.objects.filter(colaborador_id=cid).select_related("celula").order_by("pk"):
            por_dia[dia_do_fato(ch, fortes=True)].append(ch)
        return por_dia, chamados_que_falam_de_batida(colabs[cid])

    linhas = []
    for item, dia, retencao, leitura_s1 in casos:
        cid = item["colaborador_id"]
        colab = colabs[cid]
        membro = lista.colab_na_lista(cid)
        row = dict.fromkeys((
            "celula", "chamados/ancoras/C6", "rota/acao/razao", "ato/projecao",
            "dia_na_lista/dono", "insumos_L099", "destino_DP",
        ), NV)
        row.update(
            colab=f"col{cid}", dia=dia if dia is not None else NV, motivo=item["motivo"],
            colab_na_lista=NV if membro is None else membro,
            dono=NV, dono_fonte=FONTE_DONO, dono_motivo="S1_sem_dia_publicado",
            cadastro=None, tela=None, ferias=None,
            porta_tipo_S1=PORTA_DO_TIPO.get(tipo_da_linha(item)),
            retrato=portas_retrato(retrato, cid, dia),
        )
        if dia is None:
            linhas.append(row)
            continue

        cel = celulas_do_periodo([cid], dia, dia).get((cid, dia))
        cel_id = cel.pk if cel is not None else None
        protestos = sorted(cel.protestos or ()) if cel is not None else []
        ata = (cel.ata or {}) if cel is not None else {}
        if leitura_s1 is None:
            tarefa = tarefa_do_dia(colab, dia, AGORA)
            porta = porta_da_tarefa(tarefa)
            fio = _fio_da_porta(porta)
            cal = resolver_dia_oferecido_nas_datas(colab, [dia], agora=AGORA)[dia]
            ligadas = PerguntaDisputa.objects.filter(celula_id=cel_id) if cel_id is not None else ()
            fora, papel = dono_fora_do_fio(colab, dia, ligadas, lista, com_papel=True)
        else:
            tarefa, porta, fio, cal, fora, papel = leitura_s1

        instancias = tarefa["instancias"]
        perguntas = perguntas_no_dia(cid, dia, cel_id, instancias)
        por_dia, cartorio = chamados_do_colab(cid)
        cadastro = cadastro_atual(cid, dia)
        ins, cxr = fonte_dono(cid)
        iso = dia.isoformat()
        dono = ins.dono(dia)
        na_lista = lista.dia_na_lista(cid, iso)
        destino = lista.destino_fora_da_mesa(cid, iso)
        dados, telas = cadastro_e_tela(colab, dia, cel, protestos, cadastro)
        row.update({
            "celula": None if cel is None else {
                **campos(cel, "pk", "trabalha", "origem", "escala_geradora_id", "ausencia_id",
                         "julgada_em", "veredito", "veredito_via"),
                "protestos": protestos,
                "ata": {k: ata.get(k) for k in (
                    "tipo_dia", "minutos_previstos", "minutos_realizados",
                    "sem_ancora", "cobertura_parcial", "periodo_parcial",
                )},
            },
            "motivo_dia": retencao["motivo"] if retencao is not None else item["motivo"],
            "chamados/ancoras/C6": {
                "dia_forte": [campos(ch, "pk", "status_local", "modulo_origem") for ch in por_dia.get(dia, ())],
                "cartorio": [campos(ch, "pk", "status_local", "modulo_origem") for ch in cartorio.get(iso, ())],
                "fila_publicada": (porta or {}).get("fila", ()),
                "perguntas": [{
                    **campos(p, "pk", "disputa_id", "celula_id", "chamado_destino_id"),
                    "pai": p.disputa.chamado_id,
                    "ancora": chamado_que_lista(p),
                    "C6": pergunta_viva(p, hoje=HOJE, agora=AGORA),
                } for p in perguntas],
            },
            "rota/acao/razao": {
                "porta": porta, "fio": fio, "razao": tarefa["razao"],
                "instancias": [{k: i[k] for k in ("instancia", "pergunta_id", "razao", "acoes")} for i in instancias],
                "calendario": NV if cal is None else cal, "fora_fio": fora, "papel": papel,
            },
            "ato/projecao": ato_e_projecao(perguntas, instancias),
            "dia_na_lista/dono": NV if na_lista is None else {
                "dia_na_lista": na_lista[0], "dono_lavrado": na_lista[1],
                "mesa_admin": lista.dia_do_admin(cid, iso),
            },
            "insumos_L099": {
                **cadastro,
                "horas_oraculo": ins.horas.get(iso, []),
                "batida_fora_de_turno": batida_fora_de_turno(colab, dia, ins.turnos()),
                "fase_deslocada": iso in ins.fase(),
                "em_cadastro_x_realidade": NV if cxr is None else iso in cxr,
            },
            "dono": NV if dono is None else dono,
            "dono_motivo": "lavra_CxR_sem_cobertura_e_insumo_necessario" if dono is None else None,
            "destino_DP": NV if na_lista is None else destino,
            "cadastro": dados,
            "tela": telas,
            "ferias": dados_ferias(colab, dia) if item["motivo"] == "ferias_com_batida" else None,
        })
        linhas.append(row)
    return linhas


def colher():
    with schema_context("juliani"):
        connection.set_autocommit(False)
        try:
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
                cursor.execute(
                    "SELECT current_schema(), current_setting('transaction_read_only'), "
                    "current_setting('transaction_isolation')"
                )
                if cursor.fetchone() != ("juliani", "on", "repeatable read"):
                    raise SystemExit("PAREI: schema ou modo de transacao incorreto")
            if not User.objects.filter(username="sistema").exists():
                raise SystemExit("PAREI: usuario de sistema ausente na Sombra")
            empresa = Empresa.objects.get(pk=EMPRESA_ID)
            inicio, fim = janela_fechamento(MES, ANO, empresa)
            linhas = molede_linhas(empresa, inicio, fim)
            with connection.cursor() as cursor:
                cursor.execute("SELECT 1")
            return linhas
        finally:
            connection.rollback()
            connection.set_autocommit(True)


class SemErroComDadosLivres:
    def __enter__(self):
        return self

    def __exit__(self, tipo, erro, quadro):
        if tipo is None or issubclass(tipo, (SystemExit, KeyboardInterrupt)):
            return False
        while quadro.tb_next is not None:
            quadro = quadro.tb_next
        arquivo = quadro.tb_frame.f_code.co_filename.rsplit("/", 1)[-1]
        raise SystemExit(f"PAREI: {tipo.__name__} em {arquivo}:{quadro.t

[truncado em 20000 de 32230 caracteres]

## 27/11 20:54 · sessao f4be0ffb7b4d · - · - · origem=Ronald (tmux)

- A autoridade do dono é `InsumosDoDono(..., sem_motor=True, cxr=cxr_lavrado()(cid, ini)).dono(dia)`, que chama `dono_da_divergencia`. O mesmo caminho já existe no relatório nominal de férias. `None` significa que a lavra CxR não cobre a competência do colaborador e que o juiz precisaria desse insumo para resolver o dono. Registre horas do oráculo, fase, turno aberto com batida e as três chaves de cadastro pelos mesmos leitores. O juiz pode devolver `BATIDA` ou `ESTRUTURA` para um protesto cuja origem o supra-juiz descreve como cadastro, pois `PARCIAL_SEM_PERIODO` não é um argumento do E6. `app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:198`, `app/ferias/management/commands/listar_ferias_com_batida.py:53`, `app/ponto/services/cxr_na_lista.py:165`, `app/ponto/supra_juiz.py:180`, `app/ponto/supra_juiz.py:395`.
- IDs exatos da pergunta do `passivo_sem_linha` vêm das instâncias da tarefa. `DecisaoPergunta.linhas_da_projecao_sem_linha` devolve o tipo e o instante do ato faltante. O leitor da tarefa corta em `ato_em <= AGORA`, e `composta` lê a projeção combinada. `DecisaoPergunta.registrar` é o escritor único da sequência. Resposta humana e validação são publicadas pelas respectivas portas junto à projeção, e o backfill tem a correção específica do passivo. A porta que omitiu a linha de cada caso segue [nao verificado]. `app/chamados/services/tarefa_do_dia.py:338`, `app/chamados/services/tarefa_do_dia.py:433`, `app/chamados/models.py:4416`, `app/chamados/models.py:4507`, `app/chamados/models.py:4590`, `app/chamados/services/disputa_emissao.py:1565`, `app/chamados/services/validacao.py:176`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:164`.
- A S1 usa só `PORTA_DO_TIPO['ferias']`, que vale `None`. A publicação do drawer consulta também o fio. O contrato vigente da linha de férias produz fio `admin` e permite o link genérico `chamados:modal_fio`. Leia a publicação efetivamente lavrada por `linha_do_colab` + `portas_da_linha`, chamadas pela view do painel. A existência de pergunta ou ação nesse link para o caso nominal segue [nao verificado]. `app/folha/retrato_folha.py:351`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1003`, `app/folha/retrato_folha.py:1066`, `app/colaboradores/views.py:996`, `app/folha/management/commands/lavrar_retrato_folha.py:77`.
- O cruzamento da Folha usa `ferias_na_janela` para o agendamento, e `batidas_apuraveis` pela data civil. O relatório nominal de férias usa um universo distinto, de `Ausencia` espelho. A seção administrativa de férias e a página de saldo chamam um escritor de status durante o GET. `app/folha/export.py:464`, `app/ferias/services.py:49`, `app/ponto/turnos.py:2941`, `app/ferias/management/commands/listar_ferias_com_batida.py:4`, `app/ferias/views.py:121`, `app/ferias/views.py:838`.
- Para `col296`, o leitor canônico de chamados que compõem o vocabulário do cartório é `chamados_que_falam_de_batida`. A versão de chamados vivos que protestou no julgamento é um subconjunto filtrado pela ata daquele ato. A data forte do chamado, as âncoras por pergunta (destino ou pai), o C6 e o histórico da disputa são leituras distintas. A causa da permanência da cobrança segue [nao verificado]. `app/ponto/services/cartorio.py:131`, `app/ponto/services/cartorio.py:1498`, `app/ponto/services/cartorio.py:1618`, `app/chamados/juizes.py:89`, `app/chamados/juizes.py:911`, `app/chamados/services/fio.py:97`.
- A lista administrativa lê a pertença do colaborador, a presença do dia, o dono lavrado e o destino DP por portas distintas. O encaminhamento `pauta-dp` é publicado para o C13 entregue, com fonte no serviço de pares-relâmpago. `app/chamados/services/vigia_fio.py:210`, `app/chamados/services/vigia_fio.py:228`, `app/chamados/services/vigia_fio.py:238`, `app/escala/services/cadastro_realidade.py:1274`, `app/ponto/services/par_relampago_na_lista.py:375`.

## 27/11 20:54 · sessao 416874d39030 · - · - · origem=Ronald (tmux)

- **`PREVISTO_COM_DIA_INDEFINIDO`.** Ler `cadastro_do_dia_carregado(cid, dia)`, `vinculo_do_dia_carregado(cid, dia)` e `celulas_do_periodo([cid], dia, dia)`. Os parâmetros do juiz distinguem folga 6x1 indefinida e vínculo inválido. A célula traz o vínculo gerador, `trabalha` e `ata['sem_ancora']`. Esse protesto tem vários ramos, de modo que o campo exato de cada linha depende desses insumos. `app/ponto/supra_juiz.py:179`, `app/ponto/supra_juiz.py:350`, `app/ponto/supra_juiz.py:369`, `app/ponto/supra_juiz.py:408`, `app/ponto/supra_juiz.py:467`, `app/escala/alimentacao.py:54`, `app/escala/alimentacao.py:289`, `app/ponto/services/cartorio.py:749`.

- **Folga do mês.** `reverse('escala:plano_folgas')`, com empresa, ano e mês civil do dia. A view `plano_folgas` usa `escala/plano_folgas.html`. O POST `escala:plano_folgas_aplicar` chama `aplicar_folgas_em_lote`. O POST `escala:plano_folgas_toggle` usa `lancar_folga` e `alterar_folga`. O grid e os dois POSTs exigem vínculo ativo. `defeito_folga_nao_definida` lê `TipoEscala.folga_dia_semana` e `FolgaDia` do mês. A C11 indica Plano de Folgas para o caso que publica com dono de cadastro. `app/escala/urls.py:8`, `app/escala/views_plano_folgas.py:29`, `app/escala/views_plano_folgas.py:98`, `app/escala/views_plano_folgas.py:144`, `app/escala/folgas.py:303`, `app/escala/folgas.py:369`, `app/escala/folgas.py:385`, `app/escala/models.py:1252`, `app/escala/services/cadastro_realidade.py:793`.

- **Folga semanal no template.** `reverse('escala:wizard_tipo_editar', args=[tipo.pk])`, `escala/wizard_tipo_escala.html`, POST `escala:wizard_salvar_editar`. O escritor `salvar_tipo` atribui `folga_dia_semana`. `app/escala/urls.py:25`, `app/escala/urls.py:27`, `app/escala/views_wizard.py:65`, `app/escala/views_wizard.py:98`, `app/escala/services/cadastro_tipo.py:261`.

- **Vínculo.** `reverse('colaboradores:painel_vinculo', args=[cid])`, `colaboradores/partials/painel_vinculo.html`, POST `colaboradores:vincular_escala`. A view chama `executar_vinculo` para atribuição do template, início, fase e reativação de vínculo do mesmo início. Data de fim histórica segue os escritores de vigência, e o ajuste específico de vigência inválida localizado é o comando `corrigir_vinculo_vigencia`. `app/colaboradores/urls.py:12`, `app/colaboradores/views.py:262`, `app/colaboradores/views.py:306`, `app/templates/colaboradores/partials/painel_vinculo.html:136`, `app/colaboradores/views.py:392`, `app/colaboradores/views.py:442`, `app/colaboradores/services/vinculo.py:259`, `app/colaboradores/services/vinculo.py:582`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`.

- **`PARCIAL_SEM_PERIODO`.** Usar a cobertura da grade `cobertura_ausencia_periodo(colab, dia, dia, para='folha').get(dia)`, ler `pk`, `tipo`, `data`, `data_fim`, `status`, `minutos`, `hora_ini`, `hora_fim`. `cobre_parte_do_dia` e `periodo_da_parcial_no_dia(a, ())` identificam a falta atual. A ata registra o período visto pelo cartório. O painel `reverse('ponto:painel_ausencia', args=[a.pk])` usa `ponto/partials/painel_ausencia.html` e o form POST `ponto:editar_periodo_ausencia`. A view lê as duas horas e chama `_a4_decidir` → `decidir` → `_aplicar_periodo`, que grava os campos. `app/escala/utils.py:1230`, `app/ponto/turnos.py:2619`, `app/ponto/turnos.py:2653`, `app/ponto/turnos.py:2816`, `app/ponto/services/cartorio.py:785`, `app/ponto/views.py:2095`, `app/templates/ponto/partials/painel_ausencia.html:310`, `app/ponto/views.py:2836`, `app/ponto/services/ausencia.py:1051`, `app/ponto/services/ausencia.py:1102`.

- **Férias.** `ferias_na_janela(dia, dia, colaborador_ids=[cid])` e `batidas_apuraveis(colab, de, ate)` com janela civil inclusiva em fuso local são as fontes do export. Agendamentos são escritos por `executar_agendamento`, `_trocar_janela`, canceladores e `alinhar_status_por_data`. Batidas são escritas por `registrar_batida`, com `retratar_batida` e `restaurar_batida` alterando sua participação na apuração. Para portas publicadas no drawer ler `linha_do_colab(retrato, cid)` e `portas_da_linha(linha)` quando a linha lavrada contiver o motivo e dia. A porta estática `ferias` é `None`. A Gestão de Férias tem a URL genérica `ferias:lista_ferias`. `app/ferias/services.py:49`, `app/folha/export.py:490`, `app/ponto/turnos.py:2941`, `app/ferias/services.py:581`, `app/ferias/services.py:694`, `app/ferias/services.py:940`, `app/ponto/registro_batida.py:118`, `app/ponto/registro_batida.py:287`, `app/ponto/registro_batida.py:305`, `app/folha/retrato_folha.py:930`, `app/folha/retrato_folha.py:1010`, `app/folha/retrato_folha.py:1024`, `app/colaboradores/views.py:1001`, `app/ferias/urls.py:7`.

- **Passivo e dono.** `DecisaoPergunta.linhas_da_projecao_sem_linha` devolve ato, instante e pergunta que a projeção contém e a sequência carece. A tarefa filtra pelo instante e o backfill passa esses atos ao escritor `registrar`. O juiz L‑099 usa vínculo/folga, batida/turno, fase e CxR. O pedido de cadastro da ausência parcial vem do supra-juiz e do predicado do cartório, enquanto o retorno nominal da L‑099 depende de insumos do dia [nao verificado]. `app/chamados/models.py:4416`, `app/chamados/models.py:4590`, `app/chamados/services/tarefa_do_dia.py:433`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:161`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/supra_juiz.py:395`, `app/ponto/services/cartorio.py:416`.

## 27/11 20:54 · sessao 5a41dcb3b87d · - · - · origem=Ronald (tmux)

- O retrato da Folha traduz as portas diretamente com `linha_do_colab` e `portas_da_linha`. Férias têm `PORTA_DO_TIPO['ferias'] = None`. Um retrato com fio `admin` pode publicar o link genérico `chamados:modal_fio` pelo fallback. O escritor do retrato é `lavrar_retrato_folha`, e o drawer o lê sem recalcular. `app/folha/retrato_folha.py:351`, `app/folha/retrato_folha.py:930`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1010`, `app/folha/retrato_folha.py:1024`, `app/folha/management/commands/lavrar_retrato_folha.py:77`, `app/colaboradores/views.py:996`.
- A aba `ferias:secao_ferias_colab` é uma tela independente. O GET sincroniza e pode escrever o período aquisitivo. A sonda pode resolver a URL e consultar `ferias_na_janela(dia, dia, colaborador_ids=[cid])` e `batidas_apuraveis` com os limites locais do dia. `app/ferias/urls.py:14`, `app/ferias/views.py:832`, `app/ferias/models.py:152`, `app/ferias/services.py:49`, `app/folha/export.py:490`, `app/ponto/turnos.py:2941`.
- Os chamados por FK ou por dia forte podem ser lidos com `dia_do_fato(ch, fortes=True)`, a âncora da pergunta é `chamado_que_lista(p)` e C6 é `pergunta_viva(p, hoje=HOJE, agora=AGORA)`. A tarefa inclui perguntas por colaborador e `data_evento`, inclusive sem célula, enquanto o filtro original da S1 passa só as da célula a `dono_fora_do_fio`. A fila publicada deve ser exibida à parte dos chamados ligados ao fato. `app/chamados/juizes.py:89`, `app/chamados/juizes.py:911`, `app/chamados/juizes.py:1695`, `app/chamados/services/fio.py:97`, `app/chamados/services/tarefa_do_dia.py:532`, `app/chamados/services/tarefa_do_dia.py:805`.
- As leituras `colab_na_lista`, `dia_na_lista` e `destino_fora_da_mesa` vêm de `ListaDoAdmin`. O sinal entregue do C13 publica `encaminhamento` em `pendentes`, consumido por `dias_da_pauta_dp`. Se a lista inteira ficou sem resposta, `destino=None` também fica indeterminado. `app/chamados/services/vigia_fio.py:210`, `app/chamados/services/vigia_fio.py:228`, `app/chamados/services/vigia_fio.py:238`, `app/ponto/services/par_relampago_na_lista.py:341`, `app/escala/services/cadastro_realidade.py:1274`.
- A guarda da conta `sistema` é útil sem os atores da S1, pois o C6 consulta um helper que usa `get_or_create`. A chamada real da S1 já possui resíduo de relógio interno documentado na classificação do export. `app/core/utils.py:6`, `app/chamados/juizes.py:1323`, `app/folha/fora_da_folha.py:137`, `app/folha/export.py:1247`.

## 27/11 20:36 · sessao 12c65147695d · FIM-DE-SEMANA-JA · leitura · origem=Code

Usei a cópia local para as referências de código e numerei o `hasner.txt` colado a partir do primeiro `S1|UNIVERSO`. A identidade Git da cópia com o hash informado fica [nao verificado]. Os números são do recorte J.A., empresa 2, competência 11, de 21/10 a 20/11. `hasner.txt:1`.

Pela L-099, o dono do **fato** e o dono de uma **falha de encaminhamento** podem ser diferentes. Um cadastro incompleto pode explicar a retenção do dia, enquanto uma divergência entre a porta e a tela é ESTRUTURA. Atribuo o dono e o sítio apenas até onde o código e o anexo sustentam. [app/docs/LEIS.md:122](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:122).

## C1 — 18 dias sem saída e três marcas

**Escritor e leitores comuns.** O cartório chama `classificar_dia`, entrega os protestos a `CelulaDia.lavrar_veredito` e grava pela porta da célula. `motivos_retencao_celula` lê a lavra para decidir a retenção e `dias_que_seguram` dá esse universo à S1. Para o fio, `tarefa_do_dia` decide e `porta_da_tarefa` publica rota, ator e ação. `app/ponto/services/cartorio.py:1505`, `app/ponto/services/cartorio.py:1908`, `app/ponto/portas/celula.py:1069`, `app/folha/export.py:1282`, `app/folha/export.py:1302`, `app/folha/fora_da_folha.py:125`, `app/chamados/services/tarefa_do_dia.py:805`, `app/chamados/services/tarefa_do_dia.py:809`.

**`hasner_sem_saida` — 18 dias, 9 colabs.** A S1 usa os leitores reais, mas monta o rótulo pela união de tarefa, calendário, lista e Pauta DP. Um falso positivo possível é a rota com fio `chamado`, que o vigia real aceita e a S1 exclui da sua primeira condição. A Folha também distingue `nao_sei` de `nada`; a S1 inclui dois `nao_sei` com alcance indeterminado. O `chamados=0` do total conta o primeiro PK da fila publicada pela tarefa, de modo que a existência de chamados por outras âncoras fica [nao verificado]. `hasner.txt:11`, `hasner.txt:94`, `hasner.txt:96`, `app/chamados/services/vigia_fio.py:642`, `app/folha/fora_da_folha.py:215`.

- Dos 17 marcados sem saída, 11 têm `PREVISTO_COM_DIA_INDEFINIDO` e três têm `PARCIAL_SEM_PERIODO`, ambos **CADASTRO** pelo juiz. Tirar esses dias da retenção sem acertar o cadastro ocultaria, por exemplo, uma ausência parcial aprovada sem período. Nos dois `FURO_PARCIAL`, o dono do fato fica [nao verificado]. A falta de leitura com `passivo_sem_linha` é **ESTRUTURA** na trilha de `DecisaoPergunta`; a tarefa se abstém quando a projeção carece do ato. A E4C-F8 está aberta e migra leitores da projeção. A cobertura desses dois casos pela obra fica [nao verificado]. O padrão é escritor único e tipo soma, preservando a distinção entre leitura desconhecida e ausência de porta. `hasner.txt:80–96`, `app/ponto/supra_juiz.py:350`, `app/ponto/supra_juiz.py:369`, `app/ponto/supra_juiz.py:395`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/chamados/services/tarefa_do_dia.py:340`, `app/chamados/models.py:4416`, `app/docs/BACKLOG.md:2211`, `app/docs/GLOSSARIO.md:12`, `app/docs/GLOSSARIO.md:34`.
- O 18º é `ferias_com_batida`. O escritor da classificação é `classificar_export`, que cruza férias e batidas apuráveis. Para esse ramo, a S1 usa só `PORTA_DO_TIPO`, onde `ferias` não tem link. O dono da sobreposição e a existência de saída por outra tela ficam [nao verificado]. Liberar o TXT só para zerar o caso pode pagar novamente um dia que sai pelo módulo de férias. `hasner.txt:45`, `app/folha/export.py:464`, `app/folha/export.py:885`, `app/folha/retrato_folha.py:1003`, `app/folha/retrato_folha.py:1024`.

**`cobranca_indevida` — 7 marcas.** A S1 lê o veredito real. Os sete registros exibem `COBRANCA_SEM_FURO`: cinco têm `validar`, col246 tem lista do admin e col296 saiu sem porta na S1. Para a cobrança que subsiste após o fato ter sido satisfeito, o problema é **ESTRUTURA** no encerrador do cartório, `_julgar_colab_corpo`, no ramo que chama `reconciliar`. A **E4B-SCA-25-COBRANCA-INDEVIDA**, aberta no BACKLOG, cita col246 em 01/11 e ainda pede a causa da não baixa. O padrão é a lâmpada do fato acionando a reconciliação pelo escritor único. O contraexemplo é a resposta sem veredito que precisa sobreviver mesmo com a lâmpada acesa. A causa da permanência nos sete casos fica [nao verificado]. `hasner.txt:27`, `hasner.txt:90`, `hasner.txt:158`, `hasner.txt:174`, `hasner.txt:180–183`, `app/ponto/supra_juiz.py:475`, `app/ponto/services/cartorio.py:1618`, `app/chamados/models.py:718`, `app/docs/BACKLOG.md:2318`, `app/docs/GLOSSARIO.md:12–13`.

**`discordante` — 12 marcas.** A S1 lê o veredito real, que agrega batida órfã fora da tolerância, realizado inflado e realizado zero com turno. As 12 linhas têm saída na reconstrução: quatro pelo app, sete por `resolver_dia` e uma por `decidir`. O leitor de produção da Pauta de discordantes consulta também dias sem fio e o juiz do dono. Dono, sítio e obra por caso ficam [nao verificado]. Se houver divergência entre leitores com os mesmos insumos, aplica-se o padrão de fonte única. Zerar a marca por filtro esconderia, por exemplo, o realizado zero apesar dos marcos casados. `hasner.txt:20`, `hasner.txt:22–23`, `hasner.txt:26`, `app/escala/models.py:1627`, `app/ponto/supra_juiz.py:294`, `app/ponto/management/commands/pautas_discordantes.py:72`, `app/ponto/management/commands/pautas_discordantes.py:118`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/docs/GLOSSARIO.md:11`.

**`fio_ninguem_nao_sei` — 21 marcas.** Aqui a marca é da S1, que junta dois resultados do leitor real. Quatro dias têm `fio_do_admin` fora da tarefa e 17 foram rotulados sem saída. O dono do fato segue os protestos e a L-099. O `nao_sei` é falha de leitura do sistema. Para uma omissão confirmada no ramo sem perguntas vivas, com o colab na lista e o dia fora dela, o sítio é `vigia_fio.censo_alcance`, ramo `d`, tocado pela **CLASSE-D-NA-FRENTE-DA-E4D**, aberta no BACKLOG. O enquadramento dos 17 nesse ramo fica [nao verificado]. O padrão é ler o dono e a rota da fonte única. Transformar toda abstenção em porta de cobrança esconderia a falta de ato no passivo. `hasner.txt:19`, `hasner.txt:21`, `hasner.txt:25`, `app/folha/fora_da_folha.py:57`, `app/folha/fora_da_folha.py:86`, `app/chamados/services/vigia_fio.py:202`, `app/chamados/services/vigia_fio.py:680`, `app/chamados/services/tarefa_do_dia.py:340`, `app/docs/BACKLOG.md:2239`, `app/docs/GLOSSARIO.md:11`, `app/docs/GLOSSARIO.md:34`.

**Linha faltante:** `C1|colab|dia|celula|chamados/ancoras/C6|rota/acao/razao|ato/projecao|colab_na_lista|dia_na_lista/dono|insumos_L099|destino_DP`. Para férias, falta ainda a linha nominal com agendamento, batida apurável e portas efetivamente publicadas. `app/chamados/services/vigia_fio.py:210`, `app/chamados/services/vigia_fio.py:238`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/folha/export.py:490`, `hasner.txt:45`.

## C2 — `chamado_com_bloqueio_sem_acao_gestor_ou_colab`

**38 chamados distintos, 10 colabs.** Basta um par chamado–pergunta sem ação no recorte para o chamado entrar no total. O escritor de estado é `ChamadoColaborador._gravar_estado`; o nascimento da pergunta e o vínculo com o destino passam por `_nascer`. Os leitores reais são o plano e o gate de pergunta viva e, no caminho de `reconciliar`, o gate da conversa do pai, que consulta `perguntas_que_seguram_csf`. `hasner.txt:5`, `hasner.txt:47–53`, `app/chamados/models.py:213`, `app/chamados/models.py:3137`, `app/chamados/models.py:393`, `app/chamados/models.py:429`, `app/chamados/models.py:683`, `app/chamados/models.py:718`, `app/ponto/supra_juiz.py:561`.

A S1 usa `_vivas_ancoradas("fechado")` e acrescenta o CSF, mas recompõe o bloqueio. O gate real deixa fechar se **todas** as vivas puderem ser reatadas. A sonda também exclui a disputa própria selecionada e exige C6 das perguntas trazidas pelo CSF. Para alcance, renderiza cartões, simula três verbos com uma conta gestora e estima login novo do colab. Um falso positivo concreto é **Conferir data**: a S1 retira `pode_conferir_data` do contexto, embora o parcial publique o controle e exista uma porta real. A ação disponível por outros atores e o resultado do POST ficam [nao verificado]. `app/chamados/models.py:332`, `app/chamados/models.py:393`, `app/chamados/models.py:451`, `app/ponto/supra_juiz.py:573`, `app/chamados/services/fio.py:610`, `app/templates/chamados/partials/modal_fio.html:169`, `app/chamados/views.py:1288`, `app/api/login_helpers.py:118`.

**Dono, sítio e padrão.** Uma contradição confirmada entre a ação autorizada e o cartão é **ESTRUTURA** em `fio.contexto_parcial_fio`. A **E4C-F5-82-FALTA**, aberta, toca esse sítio na divergência porta × cartão por dono. O padrão é a tela ler o mesmo juiz de âncora e autorização que a porta aplica. Os 38 vêm só agregados, então dono e causa por chamado ficam [nao verificado]. Contraexemplo: liberar o pai pelo sistema para zerar a classe pode enterrar uma resposta que ainda exige decisão humana. Falta `C2|chamado|pergunta|origem/destino|plano_de_reata|CSF|C6|ator/perfil|canal/acao|motivo_da_recusa`, por par. `hasner.txt:47–53`, `app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:573`, `app/chamados/models.py:735`, `app/docs/BACKLOG.md:2059`, `app/docs/GLOSSARIO.md:12`, `app/docs/GLOSSARIO.md:33`.

## C3 — `premissa_morta:*`

**Leitura comum.** `_veredito_premissa` coleta o fato e `catalogo.premissa.avaliar` devolve o motivo real. A S1 conta o veredito antes dos descartes. O vigia de produção exclui colab com evento pendente e separa retenção da retratação e recusa de `reconciliaria()`. O escritor `retratar_premissa_morta_colab` chama `retratar_com_motivo`, que relê sob trava, aplica a cerca da competência e chama `reconciliar` para gravar. Assim, o resultado real do juiz pode virar falso positivo **de falta de saída** na contagem bruta. Os 20 `retido_pelo_fechamento` vêm de outra leitura da união e seu cruzamento com os 54 fica [nao verificado]. `hasner.txt:54–59`, `app/chamados/catalogo/premissa.py:115`, `app/chamados/reconciliador.py:248`, `app/chamados/reconciliador.py:587`, `app/chamados/reconciliador.py:602`, `app/chamados/reconciliador.py:1600`, `app/chamados/management/commands/reconciliar_chamados.py:62`.

- **`turno_aberto ausente no nucleo` — 48 chamados, 31 colabs.** O catálogo aceita vários módulos de nascimento. O leitor usa o turno denunciado, por entrada ou dia, e cai no teste global quando falta data. O escritor do fato no núcleo é `recompute_turnos`, que emite evento quando o turno aberto some do recorte e existe célula. Se o evento devido desse recompute faltou, o dono é **ESTRUTURA** e o sítio é `recompute_turnos`. A obra que cobre esse sítio, **C20-JUIZ-3**, consta **FECHADA**. Obra aberta nesse sítio e causa dos 48 ficam [nao verificado]. O padrão é **Transactional Outbox**. Contraexemplo: a premissa do núcleo pode morrer enquanto o chamado ainda é a cobrança viva de um dia furado para a folha; o gate deve retê-lo. `hasner.txt:58`, `app/chamados/catalogo/premissa.py:45`, `app/chamados/reconciliador.py:118`, `app/chamados/reconciliador.py:287`, `app/ponto/nucleo.py:40`, `app/ponto/nucleo.py:86`, `app/chamados/models.py:783`, `app/docs/BACKLOG.md:2094`, `app/docs/PLANO-PADROES.md:10`.
- **`filhos_vivos ausente` — 6 chamados, 6 colabs.** `criar_disputa_a_partir_de_chamados_orfaos` abre o pai agregador e grava os IDs; `_filhos_vivos` lê o estado desses filhos. O receiver `_filho_morto_acorda_o_pai` chama a reconciliação no commit quando um filho sai de `VIVOS`. Se esse despertar devido falhou, o dono é **ESTRUTURA** e o sítio é esse receiver. Obra aberta nesse sítio e causa dos seis ficam [nao verificado]. O padrão é reconciliação dirigida pelo evento do filho. Contraexemplo: o último filho pode morrer deixando no pai uma resposta ainda sem consumo. `hasner.txt:57`, `app/chamados/services/disputa_emissao.py:1041`, `app/chamados/services/disputa_emissao.py:1118`, `app/chamados/reconciliador.py:239`, `app/chamados/signals.py:351`, `app/chamados/signals.py:371`, `app/chamados/reconciliador.py:440`, `app/docs/GLOSSARIO.md:16`.

**Linha faltante para as duas:** `C3|chamado|modulo/subcategoria|dia/entrada_ou_ids_e_estados_dos_filhos|mudanca_do_fato|evento/criacao/processamento/erro|retencao|reconciliaria/guarda|cerca_no_ato`. Se ela apontar a cerca do escritor, o sítio comum é `retratar_com_motivo`, tocado pela **E4C-F1-B2b** aberta. O BACKLOG registra o F0 já no ar com os instrumentos desligados. `app/chamados/reconciliador.py:558`, `app/chamados/reconciliador.py:1546`, `app/docs/BACKLOG.md:828`, `app/docs/BACKLOG.md:2210`.

## C4 — `hasner_sem_porta`

**8 disputas.** `_abrir_ou_agregar_disputa` grava a disputa aberta, `_nascer` grava a pergunta, `responder_pergunta_disputa_core` grava a resposta e `DisputaSupervisao.fechar` grava o fechamento. C6 e `respostas_sem_veredito` são leituras reais, consumidas por `pode_fechar` no C7. A S1 reconstrói o alcance com as mesmas limitações de cartões, verbos e atores de C2. Aplica ainda o corte de 30 dias e rotula a disputa inteira quando **uma** pergunta fica sem porta, mesmo com outras portas na mesma disputa. O dia impresso pode ser de uma pergunta irmã. A disputa 1398 ilustra a coexistência de sem porta, veredito e resposta. `hasner.txt:60–63`, `hasner.txt:98`, `app/chamados/services/disputa_emissao.py:582`, `app/chamados/services/disputa_emissao.py:1529`, `app/chamados/models.py:3137`, `app/chamados/models.py:2000`, `app/chamados/models.py:2047`, `app/chamados/models.py:2199`, `app/chamados/services/fio.py:610`.

**Dono, sítio e padrão.** Se o C7 já autoriza fechar e a disputa fica aberta por falha de reação aos filhos, é **ESTRUTURA** no agregado `DisputaSupervisao`, no fluxo de `fechar`. A **RAIZ-DA-DISPUTA** consta aberta e toca esse elo. O padrão é **Aggregate Root** com escritor único. Dono e mecanismo por disputa ficam [nao verificado]. Contraexemplo: fechar por idade a disputa 1362 esconderia a resposta sem veredito da pergunta 39872, que também aparece sem porta na S1. Na mesma disputa, a pergunta 53621 aparece em C5 como guarda do chamado 36799. Falta `C4|disputa|pergunta|dia_do_fato|C6|C7_sem_veredito|pode_fechar|ator/canal|acao/recusa|insumos_L099`, por pergunta e sem truncar IDs. `hasner.txt:97`, `hasner.txt:116`, `app/chamados/models.py:2122`, `app/docs/BACKLOG.md:1632`, `app/docs/BACKLOG.md:2326`, `app/docs/GLOSSARIO.md:11–12`.

## C5 — `so_saida_pelo_desligado` e `sem_guarda`

**Escritor e leitores comuns.** `executar_desligamento` grava situação e credencial, tenta fechar primeiro as disputas sem resposta pendente de veredito, chama `encerrar` nos chamados e grava S89. `encerrar` relê o estado sob trava e usa o gate de pergunta viva antes de gravar. A recusa CSF do pai pertence ao caminho de `reconciliar`, que o fechamento da disputa pode disparar antes de `encerrar`. `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:175`, `app/colaboradores/services/desligamento.py:182`, `app/colaboradores/services/desligamento.py:195`, `app/chamados/models.py:1779`.

- **`so_saida_pelo_desligado` — 31 chamados, quatro colabs.** A S1 usa o gate real em seco. O “só saída” é reconstruído: todas as perguntas devolvidas pelo plano esperam texto, aparecem em app ou web, a elegibilidade estimada falha e a conta DP medida fica sem os três verbos. Quando uma reata falha, o plano devolve **todas** as vivas. A S1 também fotografa a guarda atual antes da sequência que, no ato, tenta fechar a disputa e pode carimbar as mudas. Logo, os 31 criados antes do S89 não provam 31 recusas históricas. O contraexemplo está no teste da cascata: fecha o chamado da pergunta muda e conserva aberto o da resposta sem veredito. `hasner.txt:67`, `hasner.txt:78`, `app/chamados/models.py:393`, `app/chamados/services/fio.py:213`, `app/api/login_helpers.py:118`, `app/colaboradores/services/desligamento.py:164`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:327`.
- **`sem_guarda` — chamado 36413, col427.** A foto seca tem plano sem bloqueio de `ENCERRAR` e `csf_pai_recusa=0`. O plano deixa passar tanto lista vazia como vivas todas reatáveis. A história do chamado e a permissão da porta humana ficam [nao verificado]. Contraexemplo: a transição humana recusa justificativa pendente. Fechar à força para zerar a classe esconderia essa pendência. `hasner.txt:66`, `hasner.txt:113`, `app/chamados/models.py:393`, `app/chamados/models.py:451`, `app/chamados/models.py:147`.

**Dono, sítio e padrão para ambas.** Uma falha confirmada da cascata é **ESTRUTURA**, com um sítio, `executar_desligamento`. O BACKLOG marca **C7-DISPUTA-FECHA-POR-UM-JUIZ** `EM CURSO` e nomeia esse sítio; a chamada à porta C7 já está na cópia. A fatia específica **DESLIGAR-LIQUIDA-TUDO**, O50(d), consta com despacho de censo e sonda. O padrão é escritor único. Falta `C5|chamado|s89|antes/depois|C7/perguntas/resultado|ENCERRAR/plano/reatas/resultado|trilha_de_estado_e_recusa_ate_hoje|justificativa|ator/canal`. A frase de fechamento do S89 é fixa e a causa histórica fica [nao verificado]. `app/docs/LEIS.md:122`, `app/docs/BACKLOG.md:732`, `app/docs/BACKLOG.md:1466`, `app/docs/BACKLOG.md:1632`, `app/docs/GLOSSARIO.md:12`, `app/colaboradores/services/desligamento.py:199`.

## AVISO — `tipo_marco_divergente` — 1.077

`_tipo_pelo_marco` compara o tipo E/S do marco cobrado com o tipo do marco da ata mais próximo da hora respondida, dentro da tolerância. Havendo os dois tipos e divergência, registra o aviso e devolve o tipo cobrado. O derivador descarta o booleano `_div` e usa o tipo escolhido. A S1 conta emissões de log, portanto 1.077 é o número de ocorrências, sem deduplicação por pergunta. `hasner.txt:79`, `app/chamados/services/materializacao.py:539`, `app/chamados/services/materializacao.py:567`, `app/chamados/services/materializacao.py:595`, `app/chamados/services/materializacao.py:683`, `app/core/observ.py:73`.

O aviso em si só incrementa observabilidade. O **tipo escolhido** pode alimentar o slot do C6 e a aplicabilidade de uma resposta, alcançando a leitura de portas/guardas em C1, C2, C4 e C5. Em C3, esse caminho pode alcançar a guarda seca, enquanto os dois vereditos de premissa vêm do turno ou dos filhos. A interseção dos avisos com qualquer subclasse e uma eventual mudança de classificação ficam [nao verificado]. Falta `AVISO_CASO|classe/etapa|colab/dia|chamado/disputa/pergunta|tipo_cobrado/ata/usado|resultado_C6/aplicabilidade/porta`. Para provar efeito na classe, falta também o resultado com o tipo confirmado pela autoridade do caso. `app/chamados/juizes.py:404`, `app/chamados/juizes.py:802`, `app/chamados/juizes.py:1587`, `app/chamados/services/materializacao.py:880`, `app/chamados/reconciliador.py:287`, `app/chamados/models.py:708`, `hasner.txt:79`.

## 27/11 20:36 · sessao 0483e8190b85 · - · - · origem=Ronald (tmux)

**C2 — 38 chamados, 10 colaboradores.** A unidade é o chamado com pelo menos um par testemunha. A S1 registrou 2.203 pares visíveis sem ação para o gestor amostrado, e nenhum par ausente de todas as superfícies testadas ou só na gaveta. O anexo traz os totais, sem as linhas dos 38. `hasner.txt:47–53`

- **Escrita e leitura.** `ChamadoColaborador.abrir` cria o chamado e `_gravar_estado` escreve o status. `PerguntaDisputa._nascer` grava o vínculo, inclusive ao reaproveitar pergunta, e `fio.reapontar_resolvedora` move a âncora. Para reconciliar, o modelo consulta motor, premissa, conversa do pai e vivas ancoradas. A guarda de vivas libera o fechamento se todas puderem ser reatadas. A guarda de conversa protege a resposta sem veredito do pai ou destino. `app/chamados/models.py:213–241,683–716,718–759,899,1161,3297–3336`; `app/chamados/services/fio.py:964,1085`; `app/ponto/supra_juiz.py:561–575`
- **Divergência.** A S1 reúne leitores reais, mas reconstrói o resultado. Usa `_vivas_ancoradas("fechado")` sem o plano de reata, soma respostas da guarda do pai, exige C6 também nessas respostas e exclui uma disputa própria aberta antes de contar. `reconciliar` usa o destino `resolvido` e chama as guardas em sequência. `encerrar`, com destino `fechado`, chama a guarda de vivas, sem chamar a de conversa. A S1 renderiza contextos reais sob um gestor, testa só três verbos e infere o login do colaborador. Uma pergunta reatável, uma ação disponível a outro gestor autorizado ou uma conferência de data utilizável podem produzir falso positivo. A montagem parcial real oferece conferência de data, que a S1 remove da renderização. `app/chamados/models.py:332–347,393–450,683–716,1779–1794`; `app/chamados/catalogo/motor.py:109–115`; `app/chamados/utils.py:47–83`; `app/chamados/services/fio.py:607–610`; `app/templates/chamados/partials/modal_fio.html:162–181`; `app/chamados/views.py:1288–1310`; `app/api/login_helpers.py:107–149`
- **Dono e controle.** ESTRUTURA quando uma ação devida ao ator autorizado fica inalcançável pelo desencontro entre a âncora da tela e a da porta. O sítio desse defeito é `fio.chamado_que_lista`. `E4C-F5-82-FALTA` está aberta e toca `fio.py`, com porta × cartão por dono no escopo. Padrões **fonte única** e **Policy Enforcement Point**. Dono e causa caso a caso [nao verificado]. Forçar o fechamento esconderia uma resposta real aguardando decisão humana ou deixaria uma pergunta viva sem âncora. `app/docs/LEIS.md:122`; `app/docs/GLOSSARIO.md:11–12,33`; `app/docs/BACKLOG.md:2059`; `app/chamados/services/fio.py:97–117`; `app/chamados/models.py:429–450,735–759`
- **Linha de sonda faltante.** Uma por chamado/pergunta, com `ancora`, `dia_fato`, `estado`, `C6`, `CSF`, destino e resultado de `reconciliaria` e da reata, área do chamado e portas elegíveis por perfil, conferência de data, app/PWA, login e código de recusa. `hasner.txt:47–53`; `app/chamados/models.py:393–450,677–716`; `app/chamados/utils.py:47–83`

**C4 — oito disputas.** A S1 exige disputa aberta com mais de 30 dias e marca se uma pergunta do universo C6 ou C7 ficar sem porta medida. Uma disputa com qualquer data na janela traz todas as suas perguntas para a análise e uma disputa sem data também pode entrar. Em d1398, P60396 ficou sem porta medida, ao lado de portas de veredito e resposta de outras perguntas. `hasner.txt:60–63,98`; `app/chamados/models.py:2000–2053`

- **Escrita e leitura.** `_abrir_ou_agregar_disputa` abre, `_nascer` grava a pergunta, `responder_pergunta_disputa_core` grava a fala e `validar_pergunta` escreve o veredito. `DisputaSupervisao.pode_fechar` pede zero C6 vivas e zero C7 respostas sem veredito. `fechar` faz a guarda sob trava e grava o fechamento. O explicador do modal lê o mesmo C7. `app/chamados/services/disputa_emissao.py:483,582–606,1471,1529–1567`; `app/chamados/models.py:3330–3336,2047–2053,2122–2146,2163–2177,2199–2203`; `app/chamados/services/validacao.py:697,742–787`; `app/chamados/services/explicador.py:179–186`
- **Divergência e dono.** C6 e a lista C7 são o código real. A idade e o teste de alcançabilidade são da S1, com as mesmas lacunas do C2. O explicador também descreve correção de cadastro, batida ou pendência administrativa, saídas fora dos três verbos medidos. O C7 está barrando por fundamento em todas as marcações da S1, logo ausência de um evento de fechamento, por si, não explica a classe. ESTRUTURA só se comprovada incoerência da porta devida, no sítio de publicação da âncora em `fio.py` coberto pela F5 8.2. **Aggregate Root/fonte única** é o padrão de `RAIZ-DA-DISPUTA`, ABERTA, para defeito demonstrado no ciclo dos filhos e do agregado. A sonda não localiza esse defeito nos oito [nao verificado]. `app/chamados/models.py:2047–2053`; `app/chamados/services/explicador.py:98–150`; `app/chamados/services/fio.py:97–117,228`; `app/docs/LEIS.md:122`; `app/docs/GLOSSARIO.md:11–12,26`; `app/docs/BACKLOG.md:2059,2326`
- **Controle e linha faltante.** Em d1362, P39872 está tanto em `sem_porta` quanto em `c7_sem_veredito`. Fechar por idade ocultaria uma resposta que a porta preserva. Na mesma disputa, P53621 reaparece no C5 sem login elegível no teste, com saída em app/web antes desse filtro, evidenciando por que convém ler pergunta a pergunta. Falta linha com `disputa`, `pergunta`, `dia_fato`, C6, C7, motivo, estado aplicado, portas por ator e resultado e insumos da L-099, sem truncar. `hasner.txt:97,116`; `app/chamados/models.py:2122–2146`; `app/docs/BACKLOG.md:1632`; `app/ponto/management/commands/e6_oraculo.py:98–110`

**Aviso.** `_tipo_pelo_marco` registra a divergência entre tipo do marco cobrado e tipo do marco mais próximo da ata, e devolve o tipo cobrado primeiro. A aplicação usa esse tipo e descarta o booleano de divergência. A ocorrência de log não muda a classe. A seleção de tipo entra no slot do C6 e na aplicabilidade que a S1 usa para avaliar portas, de modo que associação e efeito em C2/C4 ficam [nao verificado]. O total 1.077 é de ocorrências, sem chaves de pergunta ou classe no anexo. Falta detalhe `etapa, pergunta, chamado, disputa, tipo_cobrado, tipo_ata, tipo_usado, C6, estado_aplicacao, classe, sub`, sem texto de resposta. `hasner.txt:79`; `app/chamados/services/materializacao.py:567–603,683–721,908–973`; `app/chamados/juizes.py:404–418,802–819,1580–1589`; `app/core/observ.py:73–82`

## 27/11 20:35 · sessao 5ce08b334a9d · - · - · origem=Ronald (tmux)

### C1

- **Escritor e leitores comuns.** O cartório usa `supra_juiz.classificar_dia`, depois chama a porta de lavra de `CelulaDia`, que grava protestos, veredito, via e instante. O retentor real lê o veredito gravado e mantém as exceções de protestos não cobráveis e furo só de pausa. A tarefa deriva o fio e publica sua porta. O vigor de um dia na lista do admin vem do dono lavrado naquela lista. `dono` no retrato da folha é o fio, distinto do dono L-099. `app/ponto/services/cartorio.py:1505`, `:1908`; `app/escala/models.py:1622`, `:1832`; `app/ponto/portas/celula.py:1018`, `:1069`, `:1129`; `app/folha/export.py:1268`, `:1282`, `:1302`; `app/chamados/services/tarefa_do_dia.py:724`, `:766`, `:805`, `:809`; `app/chamados/services/vigia_fio.py:202`; `app/folha/retrato_folha.py:1246`.
- **`hasner_sem_saida`.** Os 18 dias e 9 colabs são do anexo, 17 marcados e 1 de férias [nao verificado fora do anexo]. A S1 usa o universo real de `dias_que_seguram` e o juiz da tarefa, depois reconstrói a ausência de saída. Aceita rota só com fio admin/colab, enquanto `censo_alcance` aceita qualquer rota, inclusive `chamado`. Soma calendário, destinos do vigia, mesa e Pauta DP por fora. Na produção o calendário só entra na tarefa para dia inicialmente sem rota, e o vigia consulta o destino fora do fio depois da rota. Col407/673 trazem `alcance=[nao verificado]`: abstenção entra no total S1. Chamado `-` é ausência na fila publicada que a S1 consultou, não prova de inexistência. `app/folha/fora_da_folha.py:125`, `:215`; `app/chamados/services/tarefa_do_dia.py:661`, `:692`, `:809`; `app/chamados/services/vigia_fio.py:642`, `:650`, `:656`.
- **Donos dos 17 dias marcados sem saída.** Os 11 `PREVISTO_COM_DIA_INDEFINIDO` e 3 `PARCIAL_SEM_PERIODO` têm causa de CADASTRO segundo o classificador. O parcial sem período vai para a mesa do admin preservando o dia e sem criar cobrança. Um dia fora da lista pode ser lacuna separada de encaminhamento ESTRUTURA. A classe D do vigia exige nenhuma pergunta viva, ausência do dia na lista e presença do colab nela. A obra aberta `CLASSE-D-NA-FRENTE-DA-E4D` cobre esse ramo; pertença dos dias S1 [nao verificado]. O C11 da lista também exige suas condições de folga, perguntas e âncoras; o protesto sozinho não o aciona. `app/ponto/supra_juiz.py:38`, `:398`, `:453`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:2`, `:139`; `app/chamados/services/vigia_fio.py:680`; `app/docs/BACKLOG.md:2239`; `app/escala/services/cadastro_realidade.py:500`, `:582`.
- **Férias com batida.** A produção cruza gozo agendado e batida apurável em `dias_ferias_com_batida`, depois retém pelo risco de duplicar o pagamento no TXT mensal e no módulo de férias. Agendamento e batida têm suas próprias portas de origem. A S1 só usa `tipo_da_linha` e `PORTA_DO_TIPO`, cujo `ferias` é `None`; `portas_da_linha` da produção também consulta o fio, e a linha de férias entra no retrato com fio admin. Assim a falta de link específico para férias não prova que o DP está sem destino. Estado e causa de cada registro, dono L-099 e cobertura de obra [nao verificado]. Liberar o pagamento mensal apesar do gozo válido é o contraexemplo. `app/folha/export.py:464`, `:499`, `:885`; `app/ferias/services.py:273`; `app/ponto/registro_batida.py:118`; `app/folha/retrato_folha.py:372`, `:989`, `:1003`, `:1066`, `:1248`.
- **`cobranca_indevida`.** Sete ocorrências sobrepostas às outras marcas. No anexo, cinco têm `validar`, col246 tem mesa CxR e col296 fica sem saída [nao verificado fora do anexo]. A marca lê o veredito real, cuja fonte é `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO`. O primeiro exige todos os marcos/células casados, realizado ≥ previsto e chamado vivo protestando. A contradição de manter cobrança sem lastro é ESTRUTURA. O sítio para o resíduo é a porta de reconciliação que o cartório invoca, e `E4B-SCA-25-COBRANCA-INDEVIDA` está aberta e cita o col246 01/11. Separar a causa pela qual cada caso atual ainda acusa [nao verificado]. Contraexemplo: fechar por veredito de cobrança indevida e atropelar resposta legítima pendente de validação. `app/ponto/supra_juiz.py:475`, `:507`; `app/escala/models.py:1624`; `app/ponto/services/cartorio.py:1477`, `:1618`, `:1626`; `app/chamados/reconciliador.py:1112`; `app/docs/BACKLOG.md:2318`; `app/docs/GLOSSARIO.md:13`.
- **`discordante`.** As 12 marcas têm porta no anexo, 1 `decidir`, 7 `resolver_dia` e 4 `porta_app` [nao verificado fora do anexo]. A marca relê o veredito real da lavra pelos protestos de realizado zero com turno, inflado ou batida órfã fora da tolerância. O leitor real comparável da O320 pede `dias_sem_fio` e `InsumosDoDono.dono`; a marca sozinha não pede nenhum dos dois. Dono e sítio de cura por dia [nao verificado] sem paridade, batida fora de turno, vínculo/folga/fase e CxR lavrada ou do motor. Contraexemplo: silenciar batida real órfã com realizado zero esconderia subapuração. `app/ponto/supra_juiz.py:38`, `:294`, `:513`, `:527`; `app/ponto/management/commands/pautas_discordantes.py:56`, `:118`; `app/ponto/services/pautas_dos_discordantes.py:46`; `app/ponto/management/commands/e6_oraculo.py:98`, `:182`; `app/docs/BACKLOG.md:1759`; `app/docs/GLOSSARIO.md:11`.
- **`fio_ninguem_nao_sei`.** A S1 junta o fio real em dois estados e contabiliza 21, dos quais 4 têm saída pela mesa do admin [nao verificado fora do anexo]. O leitor da folha distingue `nada` de `nao_sei`. No segundo, a leitura falha e o dono da falha é ESTRUTURA. O `FURO_PARCIAL` subjacente tem dono [nao verificado]. A razão `passivo_sem_linha` nos dois dias aponta para a trilha dos atos de `DecisaoPergunta`, cujo escritor é `registrar`. A origem histórica do passivo e uma obra em voo que repõe esses atos [nao verificado]. E4C-F8 está aberta para migração de leitores da projeção, sem prova de que reponha atos desses casos. O corte F5 preserva `passivo_sem_linha` pendente. Os quatro encaminhados à mesa podem ser BATIDA ou CADASTRO, e a S1 descarta o papel devolvido. Contraexemplo: tratar falta de trilha como ausência de resposta e voltar a cobrar uma pergunta com ato humano já praticado. `app/folha/fora_da_folha.py:44`, `:57`, `:86`; `app/chamados/services/tarefa_do_dia.py:154`, `:340`, `:429`; `app/chamados/models.py:4416`, `:4590`; `app/chamados/services/vigia_fio.py:202`, `:218`, `:252`; `app/docs/BACKLOG.md:2211`; `app/docs/CORTES.md:606`; `app/docs/GLOSSARIO.md:12`, `:19`, `:34`.

Linha C1 faltante por dia: `celula_id|veredito_em|dono_L099|insumos_dono|chamados_vivos_ids_e_guardas|perguntas_C6_ids_e_ancoras|tarefa_acoes_completas|ato_e_projecao|colab_na_lista|dia_na_lista_dono_e_assinatura|destino_DP|porta_real`. Para o dia de férias a S1 não executa `imprimir` sobre o resto; falta a linha nominal com dia, agendamento/estado, batida/origem e destinos.

### Aviso

`_tipo_pelo_marco` compara E/S do `marcos_faltantes` do destino, selecionado pelo motivo, com E/S do marco mais próximo da hora respondida na ata dentro da tolerância. Havendo os dois com diferença, lança registro best-effort em nível ERROR sem lançar a exceção criada, preserva o tipo do chamado e entrega o booleano à `_ts_tipo_do_plantio`, que o descarta. O 1077 da fábrica S1 é contagem de registros, sem deduplicação ou atribuição por classe [nao verificado por caso]. `app/chamados/services/materializacao.py:539`, `:568`, `:573`, `:595`, `:683`, `:700`; `app/core/observ.py:73`.

O registro não altera a classificação diretamente. O **tipo eleito** alimenta estado de aplicação e a leitura de validação C2/C4/C5, com janela, paridade e espera. Também pode ser lido pela vitalidade C6 via `_slot_esperado`, alcançando C1 e guardas auxiliares C3. A causa de premissa `turno_aberto` ou `filhos_vivos` em C3 vem do juiz próprio, não do aviso. Quantas classes mudariam com uma autoridade de tipo corrigida [nao verificado]. Faltaria `S1|AVISO_CASO|classe/fase/caller|colab/dia/chamado/disputa/pergunta|tipo_chamado|tipo_ata|tipo_eleito|estado_aplicacao|C6|resultado_porta`, com o resultado da autoridade válida para o mesmo caso. `app/chamados/services/materializacao.py:927`, `:935`, `:968`; `app/chamados/services/fio.py:461`, `:582`; `app/chamados/juizes.py:408`, `:802`, `:1587`; `app/chamados/reconciliador.py:248`.

## 27/11 20:34 · sessao 24a4ce798286 · - · - · origem=Ronald (tmux)

### C5

- **Escritor e leitor.** `executar_desligamento` grava a situação e desativa a credencial, tenta fechar primeiro cada disputa sem resposta pendente de veredito e depois chama `encerrar` para os chamados. `encerrar` relê o estado sob trava, aplica o motor e o gate de pergunta viva, que aceita o fechamento se a lista estiver vazia ou se todas as perguntas puderem ser reatadas. A gravação do estado passa por `_gravar_estado`. O sinal de fechamento da disputa tenta `reconciliar` no pai antes da etapa `encerrar`, por isso `csf_pai_recusa` no anexo descreve outro caminho. [app/colaboradores/services/desligamento.py:128] [app/colaboradores/services/desligamento.py:164] [app/colaboradores/services/desligamento.py:175] [app/colaboradores/services/desligamento.py:182] [app/chamados/models.py:393] [app/chamados/models.py:429] [app/chamados/models.py:1779] [app/chamados/models.py:213] [app/chamados/signals.py:43]
- **`so_saida_pelo_desligado` — 31 chamados, quatro colabs.** A sonda usa o plano e o gate reais em modo seco. A parte “só saída” é uma composição dos cartões, três verbos de pergunta e uma conta DP, mais a elegibilidade estimada para login novo. A escrita passa pelos POSTs, com autenticação, conferência de vitalidade e validação da resposta. O chamador pode ter outras portas, como decisão na célula ou `admin_encerra` para o silêncio com motivo. A disponibilidade dessas portas nos 31 chamados é [nao verificado]. O rótulo não prova recusa histórica na cascata. `criado=antes` compara com o instante S89, cuja descrição afirma sempre que a cascata fechou. [hasner.txt:67] [hasner.txt:78] [app/chamados/services/fio.py:213] [app/api/login_helpers.py:118] [app/api/views.py:2607] [app/chamados/views.py:1228] [app/chamados/models.py:177] [app/chamados/models.py:2210] [app/colaboradores/services/desligamento.py:195]
- **`sem_guarda` — #36413, col427.** A foto seca mostra `aberto`, criado antes do S89, sem bloqueio de `ENCERRAR` e sem recusa de conversa do pai. O plano também passa quando há perguntas vivas e todas podem ser reatadas. Mesmo com o retorno vazio, a transição humana verifica justificativa pendente e disputa viva, e a execução sob trava pode ter uma foto diferente. Encerrar genericamente pode esconder a justificativa pendente ou perder a âncora de perguntas que precisavam ser reatadas. [hasner.txt:113] [app/chamados/catalogo/motor.py:114] [app/chamados/models.py:140] [app/chamados/models.py:393] [app/chamados/models.py:451] [app/chamados/models.py:761]
- **Dono e origem.** ESTRUTURA para a incoerência de ciclo de vida, com o sítio único `executar_desligamento`. A causa histórica de cada registro é [nao verificado]. `C7-DISPUTA-FECHA-POR-UM-JUIZ` figura `EM CURSO` e toca esse sítio; seu uso da porta `DisputaSupervisao.fechar` está na cópia. A obra específica é `O50-DESLIGAR-ENCERRA-TUDO` (d), `DESLIGAR-LIQUIDA-TUDO`, com despacho de censo e sonda, e a grade registra separadamente o complemento de reabertura. Padrão da fatia (d): **Single Writer Principle**. A O50 usa também o padrão bitemporal para vínculos. O contraexemplo do teste mantém aberto o chamado da resposta sem veredito e encerra o da pergunta muda, preservando a resposta do colaborador segundo a porta da cópia. [app/docs/LEIS.md:122] [app/colaboradores/services/desligamento.py:164] [app/docs/BACKLOG.md:732] [app/docs/BACKLOG.md:733] [app/docs/BACKLOG.md:1466] [app/docs/BACKLOG.md:1632] [app/docs/GLOSSARIO.md:12] [app/docs/GLOSSARIO.md:27] [app/chamados/tests/test_o261e_f1_porta_encerramento.py:327] [nao verificado]
- **Linha faltante.** `C5|chamado|s89|estado_antes/depois|disputa|C7_perguntas_e_resultado|ENCERRAR_perguntas_reatas_e_resultado|eventos_estado/via/recusa_ate_hoje|justificativa|ator/canal_e_motivo_da_recusa`. Isso permite distinguir tentativa recusada, fechamento seguido de reabertura e ausência de tentativa, hoje [nao verificado]. [app/chamados/models.py:304] [app/chamados/models.py:416] [app/chamados/models.py:1428] [app/colaboradores/services/desligamento.py:182]
- **Aviso.** `_tipo_pelo_marco` conta divergência entre o tipo do marco cobrado e o tipo do marco mais próximo na ata; o retorno escolhido pode alimentar a aplicação e o slot usado em ramos do C6. `_div` é descartado pelo chamador. A testemunha de aplicação exige resposta e o slot não usa texto sem resposta vigente, portanto não se atribui o aviso à pergunta muda do C5 pela etiqueta. A ocorrência em outras perguntas consultadas para o mesmo chamado é [nao verificado]. Falta `AVISO_CASO|classe|chamado|pergunta|chamador|tipo_cobrado|tipo_ata|tipo_usado|resultado_relevante`. [hasner.txt:79] [app/chamados/services/materializacao.py:567] [app/chamados/services/materializacao.py:595] [app/chamados/services/materializacao.py:683] [app/chamados/services/materializacao.py:897] [app/chamados/juizes.py:409] [app/chamados/juizes.py:802] [app/chamados/juizes.py:1580]

## 27/11 20:17 · sessao aff0ee44922f · INVENTARIO-PELA-REGRA · leitura · origem=Code

## Bloco 1 — `causas_diff_comp10.py`

A sonda fica no corpo da resposta. Validei a sintaxe em memória e conferi os **57 colaboradores, 60 pares e 1.800 linhas `CZ|DIA`** esperadas. Execução com Django e PostgreSQL, conteúdo de `Sindicato.obs` e hash Git da cópia [nao verificado]. (`amostra.txt:1–60`, anexo)

`CZ|RUB` mostra o FM gravado, o cálculo em memória e a projeção com as regras de evento do exportador. `CZ|BASE` abre as parcelas que ajudam a localizar a diferença. `CZ|8792` mostra quantas vezes a data entra na lista real, inclusive quando há mais de um registro. A captura diária depende do ponto em que esta cópia publica o fechamento em memória e interrompe a execução se esse ponto mudar. (`app/folha/export.py:159–185`, `app/folha/export.py:402–452`, `app/ponto/services/fechamento.py:1139–1159`)

```python
# causas_diff_comp10.py
# Executar a partir de app/, no ambiente Django da sombra:
# python3 -B - < /caminho/causas_diff_comp10.py

import logging
import os
import re
import sys
from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
from linecache import getline
from types import SimpleNamespace
from unittest.mock import patch

MES, ANO = 10, 2026
INI, FIM = date(2026, 9, 21), date(2026, 10, 20)
EMPRESAS = (2, 3, 4)

# amostra.txt:1-60 (anexo)
AMOSTRA = {
    "0025": (300, 128, 146, 788, 980, 865, 348, 85, 612, 76),
    "0150": (263, 155, 129, 297, 923, 988, 879, 899, 915, 82),
    "0200": (953, 949, 696, 608, 521, 490, 302, 274, 272, 226),
    "0243": (186, 263, 60, 922, 835, 300, 82, 252, 960, 736),
    "8069": (87, 820, 42, 735, 368, 634, 821, 926, 924, 655),
    "8792": (674, 717, 650, 607, 534, 391, 271, 210, 646, 942),
}
PARES = tuple((rub, pk) for rub, pks in AMOSTRA.items() for pk in pks)
DIAS = tuple(INI + timedelta(days=i) for i in range((FIM - INI).days + 1))
OBS = re.compile(r"feriad|12\s*[x×]\s*36|\b38\b|dobr|s[uú]mula\s+444|hora\s+noturna", re.I)


def cz(tipo, *posicionais, **campos):
    return "|".join(("CZ", tipo, *map(str, posicionais),
                     *(f"{chave}={valor}" for chave, valor in campos.items())))


def leitura_diaria(fc, emp, ids):
    # app/ponto/services/fechamento.py:468-491, 1139-1159, 1215.
    alvo = fc.recalcular_fechamento_mes.__code__
    linha = 1159
    if not getline(alvo.co_filename, linha).lstrip().startswith("leitura[colab.pk] ="):
        raise SystemExit("PAREI: ponto de coleta mudou")
    apurados = {}
    furos = {}

    def observar(frame, evento, argumento):
        if evento == "exception" and not isinstance(argumento[1], fc._GradeSemCelula):
            raise SystemExit(f"PAREI: fechamento emp={emp} linha={frame.f_lineno}")
        if evento == "line" and frame.f_lineno == linha:
            v = frame.f_locals
            pk = v["colab"].pk
            apurados[pk] = (
                v["_ap"], v["_g"], v["feriados"], v["batidas"],
                v["_ft_feriado"], v["_ft_certa"], v["_ft_sem_escala"],
                v["_atrasos_dia"], v["_creditos_dia"],
            )
        return observar

    def selecionar(frame, evento, argumento):
        return observar if frame.f_code is alvo else None

    anterior = sys.gettrace()
    sys.settrace(selecionar)
    try:
        fechamentos = fc.recalcular_fechamento_mes(
            MES, ANO, empresa_id=emp, colaborador_ids=ids,
            somente_leitura=True, lavrar_dias_pagos=False,
            retornar_objeto=True, coletar_furos=furos,
        )
    finally:
        sys.settrace(anterior)
    if set(fechamentos) != set(apurados) or set(fechamentos) != set(furos):
        raise SystemExit(f"PAREI: coleta inconsistente emp={emp}")
    return fechamentos, apurados, furos


def dias_do_motor(dados, abater, dia_de):
    ap, _, _, _, ft_feriado, ft_certa, ft_sem_escala, atrasos, creditos = dados
    res = ap.resultado
    mapa_dia = getattr(res, "dia_da_jornada", None)
    campos = defaultdict(Counter)
    periodos = defaultdict(list)

    for p in (*res.periodos, *res.periodos_ft):
        d = dia_de(p, mapa_dia)
        a = campos[d]
        # app/ponto/motor_calculo_v2.py:208-252, 303-321, 363-370.
        a["noturno"] += p.minutos_noturnos_legais or 0
        a["noturno_relogio"] += p.minutos_noturnos_relogio or 0
        a["intra"] += p.minutos_intrajornada_indenizada or 0
        a["pausa"] += p.intra_descontada or 0
        a["trabalhado"] += p.minutos_trabalhados or 0
        a["fora_janela"] += p.minutos_fora_janela or 0
        periodos[d].append(p)

    for p in res.periodos:
        d = dia_de(p, mapa_dia)
        # app/ponto/services/dia_pago.py:173-186.
        campos[d]["he50"] += p.minutos_extra_50 or 0
        campos[d]["he100"] += p.minutos_extra_100 or 0
        campos[d]["he100_feriado"] += p.minutos_extra_100_feriado or 0
        campos[d]["saida_ant"] += p.minutos_saida_antecipada or 0

    for p in ft_feriado:
        d = dia_de(p, mapa_dia)
        # app/ponto/services/fechamento.py:968-981.
        campos[d]["he100"] += p.minutos_trabalhados
        campos[d]["he100_feriado"] += p.minutos_trabalhados
    for p in ft_certa:
        # app/ponto/services/fechamento.py:989-990.
        campos[dia_de(p, mapa_dia)]["folga100"] += p.minutos_trabalhados
    for p in ft_sem_escala:
        # app/ponto/services/fechamento.py:1004-1009.
        campos[dia_de(p, mapa_dia)]["folga_sem_escala"] += p.minutos_trabalhados

    for d, bruto in atrasos.items():
        # app/ponto/services/fechamento.py:1018-1026.
        liquido, usado = abater({d: bruto}, creditos)
        campos[d]["atraso"] = liquido
        campos[d]["credito"] = usado
    return campos, periodos


def previsto(grade):
    # app/escala/services/leitor_celula.py:114-149, 418-424.
    marcos = grade.get("marcos") or ()
    if not marcos or marcos[0]["tipo"] != "E" or marcos[-1]["tipo"] != "S":
        return "-"
    inicio, fim = marcos[0]["hora"], marcos[-1]["hora"]
    return f"{inicio[:5]}-{fim[:5]}" if inicio and fim else "-"


def total_rubrica(ex, mapa, rub, fm, **regra):
    if fm is None:
        return "-"
    # app/folha/export.py:402-452, 739-748, 993-1000.
    return round(sum(valor for ev, valor in ex.eventos_do_fechamento(fm, **regra)
                     if ev in mapa and mapa[ev].codigo_fopag.zfill(4) == rub), 2)


def cru(fm, modelo):
    # Projeção usada no anexo sonda.py::evento_fm.
    if fm is None:
        return None
    return SimpleNamespace(**{f.attname: getattr(fm, f.attname)
                              for f in modelo._meta.concrete_fields})


def dados_da_sombra():
    from django.conf import settings
    from django.db import connection, transaction
    from django.utils.timezone import localtime, make_aware
    from django_tenants.utils import schema_context

    db = connection.settings_dict
    if (getattr(settings, "HASNER_SOMBRA", False) is not True
            or (db["NAME"], db["HOST"]) != ("sombra", "sombra_db")):
        raise SystemExit("PAREI: configuração fora da sombra")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise SystemExit("PAREI: a sonda exige transação própria")

    from colaboradores.models import Colaborador, Empresa
    from core.feriados import feriados_do_colab
    from core.models import Sindicato
    from core.regua_cct import get_motor_cct, regua_para
    from escala.alimentacao import vinculo_do_dia_carregado
    from escala.servico_jornada import escala_vigente
    from escala.services.leitor_celula import grade_da_celula
    from folha import export as ex
    from ponto.catalogo.ausencias import descontam_vigentes
    from ponto.models import Ausencia, FechamentoMensal
    from ponto.services import fechamento as fc
    from ponto.services.credito_parcial import abater_no_proprio_dia
    from ponto.services.dia_pago import _dia_de
    from ponto.turnos import batidas_apuraveis, dia_das_batidas

    def hora_local(instante):
        return localtime(instante).strftime("%H:%M") if instante is not None else "-"

    linhas = []
    dias_emitidos = 0
    ids = {pk for _, pk in PARES}
    # app/config/settings/sombra.py:13-14, 47-60.
    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_schema(), current_setting('transaction_read_only')")
            if cursor.fetchone() != ("juliani", "on"):
                raise SystemExit("PAREI: schema ou proteção inválida")

        empresas = Empresa.objects.in_bulk(EMPRESAS)
        if set(empresas) != set(EMPRESAS) or any(
            ex.janela_competencia(e, MES, ANO) != (INI, FIM) for e in empresas.values()
        ):
            raise SystemExit("PAREI: empresas ou janelas diferentes do anexo")
        colabs = Colaborador.objects.select_related("empresa").in_bulk(ids)
        if set(colabs) != ids or any(c.empresa_id not in empresas for c in colabs.values()):
            raise SystemExit("PAREI: amostra indisponível nas empresas da sonda")

        mapas = {emp: ex._rubricas_map(e) for emp, e in empresas.items()}
        gravados = {f.colaborador_id: f for f in FechamentoMensal.objects.filter(
            colaborador_id__in=ids, mes=MES, ano=ANO)}
        ausencias = defaultdict(list)
        # app/ponto/models.py:690-705 e app/folha/export.py:63-70.
        for a in Ausencia.objects.filter(colaborador_id__in=ids, data__lte=FIM).only(
            "colaborador_id", "data", "data_fim", "tipo", "status"
        ).order_by("pk"):
            for d in ex.datas_no_periodo(a, INI, FIM):
                ausencias[a.colaborador_id, d].append((a.tipo, a.status))
        tipos_falta = descontam_vigentes()

        lidos, apurados, furos = {}, {}, {}
        for emp in EMPRESAS:
            grupo = {pk for pk, c in colabs.items() if c.empresa_id == emp}
            esperado = set(fc.universo_da_competencia(MES, ANO, emp)
                           .filter(pk__in=grupo).values_list("pk", flat=True))
            f, a, u = leitura_diaria(fc, emp, sorted(grupo))
            if set(f) != esperado:
                raise SystemExit(f"PAREI: leitura incompleta emp={emp}")
            lidos.update(f)
            apurados.update(a)
            furos.update(u)

        for pk, c in sorted(colabs.items()):
            col = f"col{pk}"
            emp = c.empresa_id  # app/ponto/services/fechamento.py:639-641.
            ciclo = ex._ciclo_do_colab(c, MES, ANO)  # app/folha/export.py:341-360.
            dobra = ex.feriado_12x36_em_dobra(c, MES, ANO)  # app/folha/export.py:363-380.
            regua = regua_para(c, FIM)  # app/core/regua_cct.py:297-380.
            ec = vinculo_do_dia_carregado(c, FIM)
            motor = get_motor_cct(c, FIM, tipo_escala=ec.tipo_escala if ec else None)
            linhas.append(cz(
                "COLAB", col=col, emp=emp, ciclo=ciclo or "-",
                regime=c.empresa.regime_trabalhista or "-",  # app/colaboradores/models.py:146-156.
                fonte_regua=regua["fonte"],  # app/core/regua_cct.py:352-380.
                hora_reduzida_afastada=int(motor.hora_reduzida_afastada),  # app/core/regua_cct.py:441-472.
                feriado_12x36_em_dobra=int(dobra),
                # Valores cadastrados. Efeito em app/core/regua_cct.py:72-79.
                tolerancia_marcacao=regua["tolerancia_marcacao_min"],
                tolerancia_dia=regua["tolerancia_dia_min"],
                intrajornada_min=regua["intrajornada_minima_min"],
            ))

            dados = apurados.get(pk)
            if dados is None:
                grade = grade_da_celula(c, INI, FIM)
                feriados = feriados_do_colab(c, INI, FIM)
                comeco = make_aware(datetime.combine(INI - timedelta(days=1), time.min))
                termino = make_aware(datetime.combine(FIM + timedelta(days=1), time.max))
                batidas = list(batidas_apuraveis(c, comeco, termino))
                campos, periodos, cobertos = {}, {}, set()
            else:
                ap, grade, feriados, batidas, *_ = dados
                campos, periodos = dias_do_motor(dados, abater_no_proprio_dia, _dia_de)
                cobertos = ap.dias_cobertos
            grade_dia = {g["data"]: g for g in grade["dias"]}
            # app/ponto/services/fechamento.py:652-653 e app/ponto/turnos.py:2971-3004.
            dia_batida = dia_das_batidas(c, INI, FIM)
            n_batidas = Counter(dia_batida[b.pk] for b in batidas if b.pk in dia_batida)
            # app/folha/export.py:87-92, 159-185, 749-753.
            faltas = Counter(ex.datas_falta(c, empresas[emp], MES, ANO))
            furo = set(furos.get(pk, ())) if dados is not None else None
            gravado_cru = cru(gravados.get(pk), FechamentoMensal)
            lido_cru = cru(lidos.get(pk), FechamentoMensal)

            for rub, amostrado in PARES:
                if amostrado != pk:
                    continue
                if rub != "8792":
                    linhas.append(cz(
                        "RUB", rub=rub, col=col,
                        fm=total_rubrica(ex, mapas[emp], rub, gravado_cru),
                        lido=total_rubrica(ex, mapas[emp], rub, lido_cru),
                        lido_com_regra_export=total_rubrica(
                            ex, mapas[emp], rub, lidos.get(pk),
                            tipo_ciclo=ciclo, feriado_12x36_em_dobra=dobra),
                        eventos=",".join(ev for ev, r in sorted(mapas[emp].items())
                                         if r.codigo_fopag.zfill(4) == rub) or "-",
                    ))
                for d in DIAS:
                    g = grade_dia[d]
                    tipo = g["tipo_dia"]  # app/escala/services/leitor_celula.py:390-424.
                    aus = ausencias[pk, d]  # app/folha/export.py:63-70.
                    m = campos.get(d, Counter())
                    tem_motor = d in cobertos or d in campos

                    def minutos(campo):
                        return round(m[campo], 4) if tem_motor else "-"

                    # app/folha/export.py:169-185: sim = data na lista real.
                    decidida = ("sim" if faltas[d] else "nao"
                                if tipo in ("trabalho", "ausencia") or aus or (furo and d in furo)
                                else "-")
                    linhas.append(cz(
                        "DIA", rub=rub, col=col, dia=d,
                        celula=tipo,
                        previsto=previsto(g),
                        n_batidas_apuraveis=n_batidas[d],  # app/ponto/turnos.py:2941-2968, 3034-3045.
                        feriado=int(d in feriados),  # app/core/feriados.py:135-167.
                        ausencia=",".join(f"{t}/{s}" for t, s in aus) or "-",
                        noturno_min=minutos("noturno"),  # app/ponto/motor_calculo_v2.py:303-321.
                        intra_min=minutos("intra"),  # app/ponto/motor_calculo_v2.py:363-370.
                        he100_min=minutos("he100"),  # app/ponto/services/fechamento.py:942-943, 978-980.
                        atraso_min=minutos("atraso"),  # app/ponto/services/fechamento.py:1018-1025.
                        saida_ant_min=minutos("saida_ant"),  # app/ponto/services/fechamento.py:1026.
                        falta_decidida=decidida,
                    ))
                    dias_emitidos += 1
                    if rub == "8792":
                        # app/ponto/services/fechamento.py:1139-1151 e app/folha/export.py:87-92, 169-185.
                        linhas.append(cz(
                            "8792", col=col, dia=d, conta=faltas[d],
                            furo_motor="-" if furo is None else int(d in furo),
                            trabalho_sem_batida=int(tipo == "trabalho" and not n_batidas[d]),
                            ausencia_sem_aprovacao=int(any(s != "aprovada" for _, s in aus)),
                            aprovada_sem_desconto=int(any(
                                s == "aprovada" and t not in tipos_falta for t, s in aus)),
                        ))
                    else:
                        ec_fer = (escala_vigente(c, d)
                                  if rub == "0200" and d in feriados and m["folga_sem_escala"]
                                  else None)
                        # app/ponto/services/fechamento.py:300-305, 938-1009, 1018-1026.
                        linhas.append(cz(
                            "BASE", rub=rub, col=col, dia=d,
                            he50_min=minutos("he50"), he100_feriado_min=minutos("he100_feriado"),
                            folga100_min=minutos("folga100"), noturno_relogio_min=minutos("noturno_relogio"),
                            folga_sem_escala_min=minutos("folga_sem_escala"),
                            feriado_vira_folga=int(ec_fer.feriado_vira_folga(d)) if ec_fer else "-",
                            previsto_min=g["minutos_previstos"],
                            pausa_descontada_min=minutos("pausa"), credito_atraso_min=minutos("credito"),
                            trabalhado_min=minutos("trabalhado"), fora_janela_min=minutos("fora_janela"),
                            pares_motor=",".join(
                                f"{hora_local(p.entrada)}-{hora_local(p.saida)}"
                                for p in periodos.get(d, ())) or "-",
                            regua_dia=ap.regua_por_dia.get(d, "-").replace("|", ",") if dados else "-",
                        ))

        # app/core/models.py:34-64, 143-156 e app/core/regua_cct.py:253-263.
        sindicatos = Sindicato.objects.filter(
            aplicacoes__empresa_id__in=EMPRESAS, aplicacoes__ativo=True,
            cct_vigencia_inicio__lte=FIM, cct_vigencia_fim__gte=FIM,
        ).distinct().order_by("pk")
        for s in sindicatos:
            # Rótulo de exibição: Sindicato.nome (app/core/models.py:34-40).
            linhas.append(cz(
                "CCT", f"sind={s.pk}", s.nome,
                feriado_12x36_em_dobra=int(s.feriado_12x36_em_dobra),
                hora_reduzida_afastada_12x36=int(s.hora_reduzida_afastada_12x36),
            ))
            for paragrafo in re.split(r"\r?\n[ \t]*\r?\n(?:[ \t]*\r?\n)*", s.obs):
                if OBS.search(paragrafo):
                    linhas.append(cz("OBS", f"sind={s.pk}", paragrafo))

        linhas.append(cz("UNIVERSO", colabs=len(colabs), pares=len(PARES), dias=dias_emitidos))
        linhas.append(cz("FIM"))
        transaction.set_rollback(True)
    return linhas


def main():
    sys.dont_write_bytecode = True
    # app/ponto/services/fechamento.py:1327-1331.
    logging.disable(logging.CRITICAL)
    sys.excepthook = lambda *_: print("PAREI: falha da sonda", file=sys.stderr)
    if os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.sombra") != "config.settings.sombra":
        raise SystemExit("PAREI: settings fora da sombra")

    import django
    from django.conf import settings

    def sem_mkdir(caminho, *, exist_ok=False):
        if caminho != "/app/logs" or not exist_ok:
            raise SystemExit("PAREI: inicialização pediu diretório inesperado")

    # Sombra herda o mkdir e os handlers de arquivo de app/config/settings/saas.py:117-183.
    with patch.object(os, "makedirs", sem_mkdir):
        settings.LOGGING_CONFIG = None
        django.setup()
    for linha in dados_da_sombra():
        print(linha)


if __name__ == "__main__":
    main()
```

## Bloco 2 — Leitura da regra e classificação

**Feriado no 12x36 dos vigilantes de Londrina.** A leitura registrada na cópia é que **não há cláusula de dobra**. O comentário do campo é explícito sobre as convenções da casa: “nenhuma das nossas diz isso hoje, e por isso o default e False.” A cláusula 38-d transcrita trata da duração da hora noturna e do adicional. `CLAUDE.md` remete a fundamentação integral para `Sindicato.obs` e registra que o PDF da CCT está fora do sistema. O conteúdo cadastrado de `ob

[truncado em 20000 de 30325 caracteres]

## 27/11 20:06 · sessao 0144c70af86d · FIM-DE-SEMANA-JA · leitura · origem=Code

Segue a S1 em texto. A sonda mantém os recortes da S0, publica os denominadores antes dos subtotais e conta C2 por PK exato da pergunta. Para C2 e C4, `S1_GESTOR_PK` identifica a conta de gestão a medir. `S1_DP_PK` pode indicar outra conta para C5 e, se omitido, usa a mesma conta. A sonda confere as permissões dessas contas no código da aplicação. A execução, os subtotais e a correspondência do snapshot com o hash informado ficam [nao verificado]. `sonda_S0.py:61–267` `app/chamados/views.py:35–38` `app/chamados/utils.py:47–83`

```python
import logging
from collections import defaultdict
from dataclasses import dataclass
from datetime import timedelta
from functools import lru_cache
from html.parser import HTMLParser
from os import environ
from types import SimpleNamespace

from django.conf import settings
from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context

# Mesmas guardas de topo da S0.
if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from django.contrib.auth.models import User
from django.template import Context, engines
from django.utils.dateparse import parse_datetime

from api.login_helpers import is_admin_user
from chamados.catalogo.motor import COBRANCA, NAO_FECHADOS, VIVOS, decidir
from chamados.juizes import (
    FIO_SEM_PORTA,
    _dia_de_turno_da_pergunta,
    competencia_do_chamado,
    dia_do_fato,
    pergunta_viva,
    perguntas_vivas_de,
)
from chamados.models import (
    ChamadoColaborador,
    DecisaoPergunta,
    DisputaSupervisao,
    PerguntaDisputa,
    motivo_implausivel_validacao,
)
from chamados.reconciliador import (
    _absolvido_pela_celula,
    _chamados_de_turno_vivos,
    _veredito_premissa,
    eventos_pendentes,
    filtro_evento_morto,
    retencao_da_retratacao,
)
from chamados.services.acoes_chamado import montar_contexto_detalhe
from chamados.services.fio import (
    chamado_que_lista,
    contexto_parcial_fio,
    disputas_do_chamado_em_lote,
    espera_o_texto_do_colaborador,
    montar_chamados_fio,
)
from chamados.services.materializacao import (
    _data_em_disputa,
    estado_aplicacao_pergunta,
    espera_declarado_x_gravado,
)
from chamados.services.painel import qs_na_fila
from chamados.services.pergunta_no_app import consulta_do_app
from chamados.services.tarefa_do_dia import (
    aguarda_admin_pela_tarefa,
    exigencia_das_perguntas,
    porta_da_tarefa,
    tarefa_do_dia,
)
from chamados.services.validacao import (
    _e_aceite_de_folga,
    _resposta_de_hora,
    entrega_que_barra_a_validacao,
    fila_validar_classificada,
)
from chamados.services.verbos import particionar_verbos
from chamados.services.vigia_fio import ListaDoAdmin, dono_fora_do_fio
from chamados.utils import pode_agir_chamado, pode_agir_departamento
from chamados.views import _is_gestor
from chamados.views_cobrar import _pode_validar_lote
from colaboradores.mixins import tem_acao
from colaboradores.models import Empresa, HistoricoVinculo
from colaboradores.services.calendario import resolver_dia_oferecido_nas_datas
from colaboradores.services.desligamento import esta_desligado
from escala.models import CelulaDia, EventoCelula
from folha.fora_da_folha import _carga, _fio_da_porta, dias_que_seguram
from folha.retrato_folha import PORTA_DO_TIPO, _balde, tipo_da_linha
from ponto.janelas import janela_fechamento
from ponto.services.declarar_dia import checar_declaracao, o_dia_ja_e_folga
from ponto.services.lastro import julgar as julgar_lastro, marcos_acesos_do_dia
from ponto.supra_juiz import perguntas_que_seguram_csf

EMPRESA_ID = 2
MES, ANO = 11, 2026
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
NV = "[nao verificado]"
S89 = "desligamento_revisao"
AVISOS = defaultdict(int)

VERBOS_DA_PERGUNTA = {
    "validar_pergunta", "recusar_pergunta", "corrigir_resposta",
}


@dataclass
class Linha:
    sub: str
    colab: int
    chamado: int = None
    disputa: int = None
    pergunta: int = None
    dia: object = None
    det: str = ""


def valor(x):
    if x is None:
        return "-"
    return x.isoformat() if hasattr(x, "isoformat") else str(x)


def ids(pks, limite=8):
    pks = sorted({pk for pk in pks if pk is not None})
    texto = ",".join(map(str, pks[:limite])) or "-"
    return texto if len(pks) <= limite else f"{texto},+{len(pks) - limite}"


def dia_da_pergunta(p):
    # Mesmo fallback da S0. app/chamados/juizes.py:58,339
    dia = _dia_de_turno_da_pergunta(p)
    if dia is None and not p.conversa and p.celula_id:
        return p.celula.data
    return dia


def c1(empresa, inicio, fim):
    # app/folha/fora_da_folha.py:61–91,125–174
    # app/folha/export.py:1268–1305
    u = dias_que_seguram(empresa, MES, ANO, hoje=HOJE)
    itens = sorted(
        (i for i in u["itens"] if _balde(i) == "bloqueado"),
        key=lambda i: i["colaborador_id"],
    )
    datas = {
        cid: [r["data"] for r in dias if inicio <= r["data"] <= fim]
        for cid, (_, dias) in u["dias"].items() if dias
    }
    carga = _carga([u["colabs"][cid] for cid in datas], inicio, fim, AGORA) if datas else None
    cel_ids = {
        r["celula_id"] for _, dias in u["dias"].values() for r in dias
        if inicio <= r["data"] <= fim and r["celula_id"] is not None
    }
    cels = CelulaDia.objects.only("protestos", "veredito_via").in_bulk(cel_ids)
    perguntas = defaultdict(list)
    for p in PerguntaDisputa.objects.filter(celula_id__in=cel_ids).select_related(
        "disputa__colaborador__empresa", "celula"
    ).prefetch_related("decisoes"):
        perguntas[p.celula_id].append(p)
    lista = ListaDoAdmin(HOJE, [empresa], agora=AGORA)
    marcadas, resto = [], defaultdict(list)

    for item in itens:
        cid, motivo = item["colaborador_id"], item["motivo"]
        if motivo != "furo_espelho":
            tipo = tipo_da_linha(item)
            porta_tipo = PORTA_DO_TIPO.get(tipo)
            sub = f"producao_com_porta:{tipo}" if porta_tipo else "hasner_sem_saida"
            for dia in item.get("dias_ferias_com_batida") or (None,):
                if dia is None or inicio <= dia <= fim:
                    resto[motivo].append(Linha(sub, cid, dia=dia))
            continue

        colab = u["colabs"][cid]
        _, dias = u["dias"][cid]
        if not dias:
            resto["furo_espelho_sem_dia"].append(Linha("hasner_sem_saida", cid))
        calendario = resolver_dia_oferecido_nas_datas(
            colab, datas.get(cid, ()), agora=AGORA
        )
        for r in dias:
            dia, cel_id = r["data"], r["celula_id"]
            if not inicio <= dia <= fim:
                continue
            tarefa = tarefa_do_dia(colab, dia, AGORA, carga=carga)
            porta = porta_da_tarefa(tarefa)
            fio = _fio_da_porta(porta)
            porta = porta or {}
            rota = porta.get("rota")
            acao = porta.get("acao")
            fila = porta.get("fila") or ()
            fora, _ = dono_fora_do_fio(
                colab, dia, perguntas[cel_id], lista, com_papel=True
            )
            na_lista = lista.dia_na_lista(cid, dia.isoformat())
            mesa = lista.dia_do_admin(cid, dia.isoformat())
            pauta = bool(lista.destino_fora_da_mesa(cid, dia.isoformat()))
            cal = calendario[dia]

            # Prioridade de apresentação da S1. As saídas concorrentes vão em det.
            publicadas = [] [nao verificado]
            if rota and fio in ("admin", "colab"):
                publicadas.append(acao or rota) [nao verificado]
            if cal is True:
                publicadas.append("calendario") [nao verificado]
            if fora:
                publicadas.append(fora) [nao verificado]
            if mesa:
                publicadas.append("cadastro_x_realidade") [nao verificado]
            if pauta:
                publicadas.append("pauta_dp") [nao verificado]
            sub = f"producao_com_porta:{publicadas[0]}" if publicadas else "hasner_sem_saida" [nao verificado]

            peso = []
            if fio in (FIO_SEM_PORTA, "nao_sei"):
                peso.append("fio_ninguem_nao_sei")
            if r["veredito"] in ("discordante", "cobranca_indevida"):
                peso.append(r["veredito"])

            cel = cels.get(cel_id)
            protestos = ",".join(sorted(cel.protestos or ())) if cel else ""
            razoes = sorted({tarefa["razao"], *(i.get("razao") for i in tarefa["instancias"])}
                            - {None, ""})
            incerto = (
                fio == "nao_sei" or cal is None or na_lista is None
                or (rota and fio not in ("admin", "colab"))
            )
            linha = Linha(
                sub, cid, next(iter(fila), None), dia=dia,
                det=(
                    f"aval={','.join(peso) or '-'} motivo={r['motivo']} "
                    f"protestos={protestos or '-'} via={valor(cel.veredito_via if cel else None)} "
                    f"fio={'ninguem' if fio == FIO_SEM_PORTA else fio} "
                    f"razao={','.join(razoes) or '-'} rota={valor(rota)} acao={valor(acao)} "
                    f"calendario={NV if cal is None else int(cal)} "
                    f"lista_admin={int(mesa)} cadastro_x_realidade="
                    f"{NV if na_lista is None else int(na_lista[0])} "
                    f"fora_fio={valor(fora)} pauta_dp={int(pauta)} "
                    f"saidas={','.join(dict.fromkeys(publicadas)) or '-'}" [nao verificado]
                    + (f" alcance={NV}" if incerto else "")
                ),
            )
            if peso:
                marcadas.append((linha, peso))
            else:
                resto[r["motivo"]].append(linha)
    return marcadas, resto


class CartoesHTML(HTMLParser):
    """Extrai somente IDs e verbos de formulários. O HTML fica em memória."""

    def __init__(self, html):
        super().__init__()
        self.fio = defaultdict(set)
        self.gaveta = defaultdict(set)
        self.locais = set()
        self.forms = []
        self.form = None
        self.feed(html)
        self.close()

    def handle_starttag(self, tag, attrs):
        a = dict(attrs)
        if "data-s1-fio" in a:
            self.fio[int(a["data-s1-fio"])].add(int(a["data-s1-ch"]))
        if "data-s1-gaveta" in a:
            self.gaveta[int(a["data-s1-gaveta"])].add(int(a["data-s1-ch"]))
        prefixo, _, ref = (a.get("id") or "").partition("-")
        if prefixo == "pergunta" and ref.isdecimal():
            self.locais.add(int(ref))
        if tag == "form":
            self.form = {}
        elif tag == "input" and self.form is not None and a.get("type") == "hidden":
            if a.get("name") in {"acao", "chamado_id", "pergunta_id", "confirmar_implausivel"}:
                self.form[a["name"]] = a.get("value", "")

    def handle_endtag(self, tag):
        if tag == "form" and self.form is not None:
            if self.form.get("acao") in VERBOS_DA_PERGUNTA:
                self.forms.append(self.form)
            self.form = None


@lru_cache(maxsize=1)
def templates():
    e = engines["django"].engine
    fonte = e.get_template("chamados/partials/modal_fio.html").source
    # Marcadores dentro das ramificações reais. app/templates/chamados/partials/modal_fio.html:157–160,468–476
    for alvo, marcador in (
        ("{{ perg.pergunta }}</div>",
         '<span data-s1-fio="{{ perg.pk }}" data-s1-ch="{{ chamado.pk }}"></span>'),
        ("{{ it.dia }}</span>",
         '<span data-s1-gaveta="{{ it.pergunta_id }}" data-s1-ch="{{ chamado.pk }}"></span>'),
    ):
        if fonte.count(alvo) != 1:
            raise SystemExit("PAREI: marcador de template mudou")
        fonte = fonte.replace(alvo, marcador + alvo, 1)
    return e.from_string(fonte), e.get_template("chamados/partials/_pergunta_card.html")


@lru_cache(maxsize=None)
def chamado_de(pk):
    return ChamadoColaborador.objects.select_related("colaborador__empresa").filter(pk=pk).first()


def validar_em_leitura(p, confirma):
    # Testemunhas da porta. app/chamados/services/validacao.py:218–234,755–784
    if not p.respondida or DecisaoPergunta.composta(p).validada_em is not None:
        return False
    if not confirma and motivo_implausivel_validacao(p):
        return False
    if _e_aceite_de_folga(p):
        dia, colab = p.data_evento, p.disputa.colaborador
        return bool(dia and (
            o_dia_ja_e_folga(colab, dia)
            or checar_declaracao(colaborador=colab, dias=[dia], tipo="folga")["aceitos"]
        ))
    if entrega_que_barra_a_validacao(p):
        return False
    if _resposta_de_hora(p):
        if espera_declarado_x_gravado(p):
            return False
        aplicacao = estado_aplicacao_pergunta(p.disputa, p) or {}
        return aplicacao.get("estado") in ("aplicavel", "ja_satisfeita", "aplicada") [nao verificado]
    if p.tipo_resposta == "data_hora" and _data_em_disputa(p):
        return False
    return True


class PortasGestor:
    def __init__(self, user, verbos):
        self.user = user
        self.req = SimpleNamespace(user=user, GET={})
        self.pode_inline = tem_acao(user, "gerir_chamados")
        self.verbos = verbos
        self.lote = (
            set(fila_validar_classificada()["refs_lote"])
            if _pode_validar_lote(self.req) is None else set()
        )

    @lru_cache(maxsize=None)
    def modal(self, colab):
        # GET real marca lidas. Usar o construtor de leitura.
        # app/chamados/views.py:870–890,928–949
        ctx = contexto_parcial_fio(self.req, colab)
        ctx.pop("pode_conferir_data", None)
        ctx["cor_disputas_map"] = DisputaSupervisao.cor_map_por_colaborador(colab)
        tela = CartoesHTML(templates()[0].render(Context(ctx, use_l10n=False)))
        cards = {ch.pk: ch for ch in ctx["chamados"]}
        eleitas = disputas_do_chamado_em_lote(cards)
        forms = defaultdict(list)
        for f in tela.forms:
            pid, cid = int(f["pergunta_id"]), int(f["chamado_id"])
            forms[pid].append((cards[cid], eleitas.get(cid), f))
        return tela, forms

    @lru_cache(maxsize=None)
    def local(self, pk):
        # app/chamados/views.py:359–372,550–552
        ch = chamado_de(pk)
        if ch is None or ch.ticket_id_glpi is not None:
            return set(), defaultdict(list)
        ctx = montar_contexto_detalhe(self.req, ch, True, ())
        visiveis, forms = set(), defaultdict(list)
        for p in ctx["perguntas_disputa"]:
            html = templates()[1].render(Context({**ctx, "perg": p}, use_l10n=False))
            tela = CartoesHTML(html)
            visiveis.update(tela.locais)
            for f in tela.forms:
                forms[int(f["pergunta_id"])].append(f)
        return visiveis, forms

    @lru_cache(maxsize=None)
    def inline(self, pk):
        ch = chamado_de(pk)
        if not (self.pode_inline and self.verbos.get(pk) == "validar"
                and pode_agir_chamado(self.user, ch)):
            return set()
        d = disputas_do_chamado_em_lote([pk]).get(pk)
        if d is None or d.colaborador_id != ch.colaborador_id:
            return set()
        return {p.pk for p in perguntas_vivas_de(d) if p.respondida}

    def consultar(self, p, ch=None):
        pid, d = p.pk, p.disputa
        ancora = chamado_que_lista(p)
        card_ids = {d.chamado_id, ancora, ch.pk if ch else None} - {None}
        cards = [chamado_de(pk) for pk in sorted(card_ids)]
        colabs = {d.colaborador_id: d.colaborador}
        for card in cards:
            colabs[card.colaborador_id] = card.colaborador

        local, fio, gaveta, glpi_local, acoes = set(), set(), set(), set(), set()
        glpi_remoto = False
        for card in cards:
            if card.ticket_id_glpi is not None:
                # O roteador primeiro testa colisão com PK local. app/chamados/views.py:219–222
                alvo = chamado_de(card.ticket_id_glpi)
                if alvo is not None and alvo.ticket_id_glpi is None:
                    if pid in self.local(alvo.pk)[0]:
                        glpi_local.add(alvo.pk)
                    card_ids.add(alvo.pk)
                else:
                    glpi_remoto = True

        def acao_do_form(f, card, canal, eleita=None):
            acao = f["acao"]
            if card.pk != ancora or not pode_agir_chamado(self.user, card):
                return
            if canal == "local" and not d.aberta:
                return
            if acao == "validar_pergunta":
                if canal == "fio" and (
                    card.colaborador_id != d.colaborador_id
                    or (not d.aberta and not pergunta_viva(p, hoje=HOJE))
                ):
                    return
                aceito = validar_em_leitura(p, f.get("confirmar_implausivel") == "1")
            else:
                if not d.aberta or (canal == "fio" and eleita is None):
                    return
                leitura = DecisaoPergunta.composta(p)
                aceito = leitura.validada_em is None
                if acao == "recusar_pergunta":
                    aceito = aceito and p.respondida and not p.fora_do_fato()
                else:
                    # A hora corrigida é entrada humana. A validação dela fica em aberto.
                    aceito = aceito and p.tipo_resposta == "hora"
            if aceito:
                acoes.add(f"{canal}:{acao}:{card.pk}")

        for pk in sorted(card_ids):
            visiveis, forms = self.local(pk)
            if pid in visiveis:
                local.add(pk)
            for f in forms.get(pid, ()):
                acao_do_form(f, chamado_de(pk), "local")
            if pid in self.inline(pk) and validar_em_leitura(p, False):
                acoes.add(f"inline:validar_pergunta:{pk}")

        for colab in colabs.values():
            tela, forms = self.modal(colab)
            fio.update(tela.fio.get(pid, ()))
            gaveta.update(tela.gaveta.get(pid, ()))
            for card, eleita, f in forms.get(pid, ()):
                acao_do_form(f, card, "fio", eleita)
        lote = pid in self.lote
        if lote and validar_em_leitura(p, False):
            acoes.add("lote:validar_pergunta")
        return local, fio, gaveta, glpi_local, glpi_remoto, lote, acoes


@lru_cache(maxsize=None)
def portas_colab(colab):
    # app/chamados/services/pergunta_no_app.py:68–92
    # app/api/views.py:2553–2574
    pendentes = consulta_do_app(colab, agora=AGORA)["pendentes"]
    exigencias = exigencia_das_perguntas(pendentes, AGORA)
    app = set()
    for p in pendentes:
        razao, acoes = exigencias[p.pk]
        if not aguarda_admin_pela_tarefa(razao, acoes, conservado=p.estado == p.RESPONDIDA):
            app.add(p.pk)
    web = {
        p.pk for card in montar_chamados_fio(colab, so_janela=True)
        for p in card.perguntas_disputa if p.viva
    }
    user = colab.usuario
    conta_ativa = bool(user and user.is_active)
    # Elegibilidade de login novo, sem autenticar nem testar sessão.
    app_login = bool(
        conta_ativa and colab.situacao == "ativo"
        and colab.empresa.ativa and not is_admin_user(user)
    )
    return app, web, app_login, conta_ativa


def c2(chamados, inicio, fim, gestor):
    linhas, sem_gestor, sem_ator = [], [], []
    for ch, dia_ch in chamados.values():
        candidatas = {p.pk: (p, {"ancora"}) for p in ch._vivas_ancoradas("fechado")}
        if not ch._gate_conversa_do_pai(registrar=False):
            extras = PerguntaDisputa.objects.filter(
                pk__in=perguntas_que_seguram_csf(ch)
            ).select_related(
                "disputa__colaborador__empresa", "disputa__chamado", "chamado_destino", "celula"
    

[truncado em 20000 de 47222 caracteres]

## 27/11 20:01 · sessao 70043781f402 · - · - · origem=Ronald (tmux)

C2 pode cruzar o PK pela projeção real do detalhe local, pelos cartões e itens da gaveta do fio e pelas referências da classe A na prévia. O modal HTTP marca notas como lidas no GET. A sonda deve usar `montar_chamados_fio` ou seu construtor de contexto e renderizar os fragmentos somente em memória. O formulário renderizado identifica a ação oferecida, e as guardas de leitura identificam recusas previsíveis. O resultado da escrita fica [nao verificado]. `app/chamados/services/acoes_chamado.py:337–358`, `app/chamados/views.py:870–890`, `app/chamados/services/fio.py:573–618,694–760,776–790`, `app/chamados/views_cobrar.py:352–399`.

O detalhe GLPI pode redirecionar para um cartão local por colisão do ID do ticket com um PK local. O ramo GLPI só apresenta descrição e followups sem vínculo estruturado por PK de pergunta. Menções em texto remoto ficam [nao verificado]. `app/chamados/views.py:219–269`, `app/templates/chamados/detalhe.html:65–143`.

C4 usa a união por PK do C6 com `respostas_sem_veredito` sobre a disputa inteira. O app expõe os PKs pendentes em `consulta_do_app`. Meu Atendimento usa `montar_chamados_fio(so_janela=True)` e o predicado `p.viva`. O acesso do colaborador também depende da conta e da situação cadastral. `app/chamados/models.py:2000–2053`, `app/chamados/services/pergunta_no_app.py:68–92`, `app/chamados/views.py:560–570,1229–1247`, `app/templates/chamados/partials/_card_atendimento.html:73–78`, `app/api/login_helpers.py:121–148`.

C5 deve ler `_plano_pergunta_viva('fechado')` para distinguir uma guarda de um caso que seria reatado. O S89 registra a execução em `contexto_json['executado_em']`, antes dos dois laços da cascata. Abertos hoje e anteriores a esse instante são candidatos. A relação causal com aquela execução fica [nao verificado] sem trilha histórica. `app/chamados/models.py:393–464,1779–1794`, `app/colaboradores/services/desligamento.py:150–214`.

## 27/11 20:00 · sessao c7d29a82460e · - · - · origem=Ronald (tmux)

- **C4.** O universo continua sendo o filtro da S0. Para cada disputa, `respostas_sem_veredito(perguntas=ps)` e `pergunta_viva(p)` fornecem a união dos PKs bloqueantes de C7. `fio_vivo(ps)` devolve um booleano. A ordem exclusiva fica sem porta para algum bloqueante, ação de gestor pendente, resposta de colaborador pendente e, com a união vazia, `sem_bloqueante`. `app/chamados/models.py:2000–2053`, `app/chamados/juizes.py:1603–1619`.
- **Porta do colaborador.** Cruze o PK com `consulta_do_app(colab, agora=AGORA)['pendentes']` e os cartões de `montar_chamados_fio(colab, so_janela=True)` com `p.viva`. A view de Meu Atendimento grava trilha, portanto use o leitor. Confira a credencial ativa e as guardas do login nativo. A API ainda devolve `aguarda_admin` calculado pela tarefa. O drawer de arquivados leva ao detalhe local, cujo bloco tipado exige gestor. `app/chamados/services/pergunta_no_app.py:68–92`, `app/chamados/services/fio.py:228–244`, `app/chamados/views.py:560–594,1228–1247`, `app/api/login_helpers.py:121–149`, `app/api/views.py:2549–2575`, `app/templates/chamados/partials/_card_atendimento.html:73–78`, `app/templates/chamados/partials/_meus_arquivados.html:26–36`, `app/templates/chamados/detalhe_local.html:296–310`.
- **C5.** Use a guarda seca `ch._gate_pergunta_viva('fechado', registrar=False)` e, quando ela recusar, os PKs de `ch._plano_pergunta_viva('fechado')`. Cruze só esses PKs com `DisputaSupervisao.sem_veredito_entre(...)` e com as portas de DP. A conversa do pai recusa `reconciliar()`, enquanto o `encerrar()` da cascata chama a guarda da pergunta viva. Uma resposta sem veredito sem porta de DP é uma guarda real. A incidência dessa combinação fica [nao verificado] e a partição em três rótulos precisa explicitar como lida com ela. `app/chamados/models.py:393–450,705–708,1779–1794,2019–2045`, `app/colaboradores/services/desligamento.py:164–190`.
- **S89 e data.** Prefira `contexto_json.executado_em`. Para o antes/depois sem S89, `HistoricoVinculo.criado_em` da linha efetiva é um candidato quando coincide com a data civil e exclui o agendamento. Por auditoria, compare todos os chamados `NAO_FECHADOS` e disputas ainda abertas do colaborador criados antes do instante da cascata, fora inclusive da janela S0. Separe contagens de auditorias com chamado, disputa e qualquer um dos dois. O estado presente, mesmo com guarda seca, deixa causalidade histórica [nao verificado] quando o item pode ter fechado e reaberto. `app/colaboradores/services/desligamento.py:112–117,138–155,182–217,235–259`, `app/colaboradores/models.py:577–584`, `app/chamados/catalogo/motor.py:24–32`, `app/chamados/models.py:2414–2450`.

## 27/11 19:59 · sessao 05af44278a51 · - · - · origem=Ronald (tmux)

C1 deve preservar a seleção da S0 e ler `protestos` pelo `celula_id` do retentor. Use `tarefa_do_dia(..., carga=carga)` com `porta_da_tarefa` e `_fio_da_porta`. A razão aparece também em `instancias[*].razao`, portanto a saída precisa incluir as duas origens. `app/folha/export.py:1268`, `app/escala/models.py:1786`, `app/chamados/services/tarefa_do_dia.py:292,809,830`, `app/folha/fora_da_folha.py:86`.

Para complementar, use `ListaDoAdmin` e `dono_fora_do_fio` com todas as perguntas da célula. O segundo retorna o primeiro encaminhamento reconhecido. `dia_na_lista`, `dia_do_admin` e `destino_fora_da_mesa` permitem registrar o que a mesma lista publicou para o dia. A oferta do calendário vem de `resolver_dia_oferecido_nas_datas`, com permissão de tela à parte. `app/chamados/services/vigia_fio.py:166,202,210,228,252,607`, `app/colaboradores/services/calendario.py:97,125`, `app/templates/colaboradores/partials/_calendario_grade.html:134`.

Para o agregado sem marca, `PORTA_DO_TIPO` com `tipo_da_linha` distingue a porta publicada de Cadastro da ausência de link próprio de férias. `portas_da_linha` pode oferecer o legado “Validar no fio” por padrão em motivos não-espelho, inclusive férias. Uma porta oferecida e o primeiro carimbo verdadeiro do papel da tarefa sustentam a classificação da sonda. Sua eficácia para resolver o caso e a atribuição ao Hasner ficam [nao verificado]. `app/folha/retrato_folha.py:361,436,496,1003,1066`, `app/chamados/services/tarefa_do_dia.py:762`.

Enviei também ao agente principal as referências de C3 para a união anterior aos descartes, registro dos eventos e leitura das guardas secas. `app/ponto/services/lastro.py:185,238,316`, `app/chamados/reconciliador.py:440,1140,1344,1389,1550`, `app/chamados/models.py:677,683`.

_8793 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
