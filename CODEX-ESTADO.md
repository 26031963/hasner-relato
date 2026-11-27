# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 20:06** · publicado em **27/11/2026 20:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 27/11 19:58 · sessao 674d858a35ca · INVENTARIO-PELA-REGRA · leitura · origem=Code

A prévia segue a porta desta cópia. Ela recalcula em memória, classifica esses mesmos objetos e os entrega ao montador real. O FM gravado é lido em separado. Para a 8792, a prévia conta datas de falta e o FM guarda `horas_falta`. Por isso, `fm=-` em dias significa quantidade indisponível nessa fonte. [app/folha/views.py:214] [app/folha/views.py:275] [app/folha/export.py:749] [app/ponto/models.py:427]

Execução dos scripts e resultado de outubro em produção [nao verificado].

## Script A — sonda Django na Sombra

Para uma sessão Django já inicializada. O script aplica as três guardas do molde, faz a leitura em uma transação própria e imprime o DPH após o rollback. A inicialização fica fora da sonda porque a configuração herdada prepara diretório e handlers de log. [modelo_sonda_S1.py:14] [app/config/settings/sombra.py:27] [app/config/settings/saas.py:118] [app/config/settings/saas.py:160]

```python
import logging
import re
import sys
from collections import Counter, defaultdict, deque
from decimal import Decimal
from pathlib import Path
from traceback import extract_tb
from types import SimpleNamespace

from django.conf import settings
from django.db import DatabaseError, connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from colaboradores.models import Empresa, HistoricoVinculo
from core.constants import isento_ponto
from folha import export as ex
from folha.models import EVENTO_CHOICES, RubricaDominio
from ponto.models import FechamentoMensal
from ponto.services import fechamento as fc

MES, ANO = 10, 2026
EMPRESAS = (2, 3, 4)
COMP = "2026-10"
COMP_TXT = "202610"
FALTA = "falta_diurna"


def pare(motivo):
    raise SystemExit(f"PAREI: {motivo}")


def sql_estrito(execute, sql, params, many, context):
    try:
        return execute(sql, params, many, context)
    except DatabaseError:
        raise SystemExit("PAREI: erro SQL na transacao da sonda") from None


def recalcular_leitura(emp, universo):
    # app/ponto/services/fechamento.py:74, 1069, 1105, 1111, 1327.
    # Interrompe a função real antes dos handlers que degradam ou pulam o vínculo.
    alvo = fc.recalcular_fechamento_mes.__code__

    def rastrear_frame(frame, evento, arg):
        if evento == "exception" and not isinstance(arg[1], fc._GradeSemCelula):
            raise SystemExit(
                f"PAREI: recálculo em memória, empresa {emp}, "
                f"app/ponto/services/fechamento.py:{frame.f_lineno}"
            ) from None
        return rastrear_frame

    def rastrear_chamada(frame, evento, arg):
        if frame.f_code is alvo:
            return rastrear_frame
        return None

    anterior = sys.gettrace()
    sys.settrace(rastrear_chamada)
    try:
        lidos = fc.recalcular_fechamento_mes(
            MES, ANO, empresa_id=emp, somente_leitura=True,
            lavrar_dias_pagos=False, retornar_objeto=True,
        )
    finally:
        sys.settrace(anterior)
    if set(lidos) != set(universo):
        pare(f"empresa {emp}: recálculo incompleto")
    return lidos


def dph(tipo, **campos):
    return "|".join(["DPH", tipo] + [f"{k}={v}" for k, v in campos.items()])


def matricula(c):
    codigo = (c.codigo_dominio or "").strip()
    if re.fullmatch(r"[0-9]{1,10}", codigo):
        return codigo.zfill(10)
    return "-"


def unidade(rub):
    if rub.tipo == "horas":
        return "h"
    if rub.tipo == "dias":
        return "dias"
    pare("tipo de RubricaDominio desconhecido")


def cent_h(quantidade):
    # Escalar de app/folha/export.py:42, sem montagem de layout.
    return int(round(float(quantidade) * 100))


def equivalente_min(quantidade):
    return Decimal(str(quantidade)).quantize(Decimal("0.01")) * 60


def evento_fm(fm):
    if fm is None:
        return []
    # Projeta apenas os campos persistidos, antes de isenção e supressão 12x36.
    snapshot = SimpleNamespace(**{
        campo.attname: getattr(fm, campo.attname)
        for campo in FechamentoMensal._meta.concrete_fields
    })
    return ex.eventos_do_fechamento(snapshot)


def evento_lido(fm):
    return ex.eventos_do_fechamento(
        fm, tipo_ciclo=ex._ciclo_do_colab(fm.colaborador, MES, ANO),
        feriado_12x36_em_dobra=ex.feriado_12x36_em_dobra(fm.colaborador, MES, ANO),
    )


def sondar(empresa, medido_em):
    emp = empresa.pk
    ini, fim = ex.janela_competencia(empresa, MES, ANO)
    cod_empresa = ex.codigo_no_sistema_dominio(empresa)
    if not re.fullmatch(r"[0-9]{10}", cod_empresa):
        pare(f"empresa {emp}: código de integração inválido")

    mapa = ex._rubricas_map(empresa)
    globais = Counter(RubricaDominio.objects.filter(
        empresa__isnull=True, ativo=True,
    ).values_list("evento", flat=True))
    mapas = {}
    for evento in sorted({e for e, _ in EVENTO_CHOICES} | set(mapa)):
        rub = mapa.get(evento)
        if rub is None:
            mapas[evento] = ("-", "-", "-")
            continue
        if rub.empresa_id is None and globais[evento] != 1:
            pare(f"empresa {emp}: default ativo ambíguo")
        codigo = rub.codigo_fopag.zfill(4)
        if not re.fullmatch(r"[0-9]{4}", codigo):
            pare(f"empresa {emp}: código de rubrica inválido")
        fonte = "RubricaDominio" if rub.empresa_id is not None else "default"
        mapas[evento] = (codigo, unidade(rub), fonte)

    universo = {c.pk: c for c in fc.universo_da_competencia(MES, ANO, emp)}
    gravados = {
        f.colaborador_id: f for f in FechamentoMensal.objects.filter(
            colaborador__empresa=empresa, mes=MES, ano=ANO,
        ).select_related("colaborador")
    }
    colabs = {cid: f.colaborador for cid, f in gravados.items()}
    colabs.update(universo)
    codigos = {cid: matricula(c) for cid, c in colabs.items()}
    validos = [codigo for codigo in codigos.values() if codigo != "-"]
    if len(set(validos)) != len(validos):
        pare(f"empresa {emp}: matrícula duplicada após formatação do layout")

    # O marcador vem do ato de transferência. A consulta traz somente IDs.
    mudancas = set(HistoricoVinculo.objects.filter(
        colaborador_id__in=colabs, motivo="transferencia", data__gte=ini,
        obs__contains="[CNPJ mudou:",
    ).values_list("colaborador_id", flat=True))

    lidos = recalcular_leitura(emp, universo)
    lista_lida = list(lidos.values())
    itens = ex.classificar_export(empresa, MES, ANO, fechamentos=lista_lida)
    juiz = {i["colaborador_id"]: i for i in itens}
    if set(juiz) != set(lidos) or len(juiz) != len(itens):
        pare(f"empresa {emp}: classificação incompleta ou duplicada")

    linhas, entra = ex.montar_linhas_export(
        empresa, MES, ANO, itens=itens, fechamentos=lista_lida,
    )
    if ex.validar_layout_dominio(linhas):
        pare(f"empresa {emp}: prévia rejeitada pelo validador real")

    proj_lida = {cid: evento_lido(f) for cid, f in lidos.items()}
    dias = {cid: len(ex.datas_falta(c, empresa, MES, ANO))
            for cid, c in colabs.items()}

    # A quantidade TXT vem exclusivamente dos registros reais. A fila só
    # identifica evento/unidade e confere cada registro com o montador real.
    esperados = deque()
    for cid, f in lidos.items():
        if cid not in entra or f.status not in ex.STATUS_NO_TXT:
            continue
        for evento, valor in [*proj_lida[cid], (FALTA, dias[cid])]:
            rub = mapa.get(evento)
            if not valor or rub is None:
                continue
            linha = ex.montar_linha_10(
                codigos[cid], COMP_TXT, rub.codigo_fopag, rub.tipo,
                valor, cod_empresa,
            )
            esperados.append((linha, cid, evento))

    saidas_txt = defaultdict(list)
    for linha in linhas:
        parte = ex.partes_da_linha(linha)
        if parte is None:
            pare(f"empresa {emp}: registro TXT ilegível")
        if parte["reg"] == "11":
            continue
        if not esperados:
            pare(f"empresa {emp}: registro 10 sem evento correspondente")
        esperada, cid, evento = esperados.popleft()
        if linha != esperada:
            pare(f"empresa {emp}: evento e registro 10 divergentes")
        saidas_txt[cid].append((evento, parte["valor"]))
    if esperados:
        pare(f"empresa {emp}: registro 10 esperado ausente")

    fm_status = Counter(f.status for f in gravados.values())
    yield dph("META", emp=emp, comp=COMP, janela=f"{ini}..{fim}", motor="-",
              fm_status=",".join(f"{s}:{n}" for s, n in sorted(fm_status.items())) or "-",
              hash_txt=ex.hash_do_txt(ex.conteudo_do_txt(linhas)),
              codigo_dominio_empresa=cod_empresa, medido_em=medido_em)
    for evento, (rub, un, fonte) in mapas.items():
        yield dph("MAP", emp=emp, evento=evento, rubrica=rub, fonte=fonte,
                  unidade=un)

    for cid, c in sorted(colabs.items()):
        fm = gravados.get(cid)
        lido = lidos.get(cid)
        item = juiz.get(cid)
        isento = isento_ponto(c)
        txt = saidas_txt[cid]
        codigo = codigos[cid]
        fora = ((c.data_admissao is not None and c.data_admissao > fim)
                or (c.data_demissao is not None and c.data_demissao < ini))

        classe, motivo = "elegivel", "-"
        if ex.sai_pela_rescisao(c, fim):
            classe, motivo = "rescisao", "rescisao_modulo_proprio"
        elif item is None:
            classe, motivo = "retido", "fora_universo_fechamento"
        elif item["status"] != "entra":
            classe, motivo = "retido", item["motivo"]
        elif codigo == "-":
            classe, motivo = "retido", "codigo_invalido_dominio"
        elif lido.status not in ex.STATUS_NO_TXT:
            classe, motivo = "retido", "status_fora_txt"
        elif isento and not txt:
            classe, motivo = "isento", "isento_eventos_horas"

        col = f"col{cid}"
        yield dph("COLAB", emp=emp, col=col, cod=codigo,
                  fm=fm.pk if fm else "-", status=fm.status if fm else "-",
                  export=classe, motivo=motivo, juiz=item["status"] if item else "-",
                  universo=int(cid in universo), fora_janela=int(fora),
                  mudanca_empresa=int(cid in mudancas), isento=int(isento),
                  fm_falta_min=equivalente_min(fm.horas_falta) if fm else "-",
                  fm_dsr_min=Decimal(str(fm.horas_reflexo_dsr)) * 60 if fm else "-")

        # Agrupa parcelas só depois de converter cada evento. Código ausente
        # fica separado por evento. Unidade física do FM vem do campo gravado.
        grupos = defaultdict(lambda: defaultdict(dict))
        for lado, eventos in (("fm", evento_fm(fm)), ("lido", proj_lida.get(cid, []))):
            for evento, horas in eventos:
                rub, _, _ = mapas[evento]
                chave = (rub, "h", evento if rub == "-" else "")
                grupos[chave][evento][lado] = (cent_h(horas), equivalente_min(horas))
        if dias[cid]:
            rub, _, _ = mapas[FALTA]
            chave = (rub, "dias", FALTA if rub == "-" else "")
            grupos[chave][FALTA]["lido"] = (dias[cid], dias[cid])
        for evento, valor in txt:
            rub, un, _ = mapas[evento]
            grupos[(rub, un, "")][evento]["txt"] = (valor, None)

        for (rub, un, _), parcelas in sorted(grupos.items()):
            lados = {
                lado: [p[lado] for p in parcelas.values() if lado in p]
                for lado in ("fm", "lido", "txt")
            }
            if not lados["txt"] and not any(
                bruto for lado in ("fm", "lido") for _, bruto in lados[lado]
            ):
                continue
            totais = {lado: sum(q for q, _ in itens) if itens else "-"
                      for lado, itens in lados.items()}
            fonte = next((lado for lado in ("fm", "lido") if lados[lado]), None)
            bruto = sum(b for _, b in lados[fonte]) if fonte else "-"
            fonte_bruto = (
                "dias_falta_vivos" if un == "dias" and fonte else
                f"{fonte}_horas_x60" if fonte else "-"
            )
            yield dph("RUB", emp=emp, col=col, cod=codigo, rubrica=rub,
                      evento=",".join(sorted(parcelas)), unidade=un,
                      fm=totais["fm"], txt=totais["txt"], bruto_min=bruto,
                      lido=totais["lido"], fonte_bruto=fonte_bruto)


nivel_log = logging.root.manager.disable
logging.disable(logging.CRITICAL)
try:
    with schema_context("juliani"), transaction.atomic():
        with connection.execute_wrapper(sql_estrito):
            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
            medido_em = timezone.now().isoformat()
            registros = [
                linha for emp in EMPRESAS
                for linha in sondar(Empresa.objects.get(pk=emp), medido_em)
            ]
            transaction.set_rollback(True)
except Exception as erro:
    ponto = extract_tb(erro.__traceback__)[-1]
    raise SystemExit(
        f"PAREI: {type(erro).__name__} em {Path(ponto.filename).name}:{ponto.lineno}"
    ) from None
finally:
    logging.disable(nivel_log)

for registro in registros:
    print(registro)
print(dph("FIM", n=len(registros)))
```

## Script B — `join.py`

A cobertura entra inteira no universo do DP, inclusive os 151 vínculos sem linha selecionada. O join confere os totais do anexo e prova a ausência de colisões dentro de cada empresa e fonte antes de parear os códigos. As duas saídas são abertas em modo exclusivo quando o script for executado no host. [parecer.md:8–13] [formato_csv.md:3] [formato_csv.md:7]

```python
import csv
import io
import os
import re
import sys
from collections import Counter, defaultdict
from decimal import Decimal
from pathlib import Path

EMP = {"JA": 2, "JSP": 3, "RA": 4}
COB_ESPERADA = {2: 431, 3: 118, 4: 23}
RUB_ESPERADA = {2: 788, 3: 203, 4: 27}
FOCO = {"0025": "h", "0150": "h", "0200": "h", "0243": "h",
        "8069": "h", "8792": "dias"}
ESPECIAIS = {
    "DSR": set("0237 0244 0250 0854 8125 8794 9360".split()),
    "Horista": {"0204"},
    "Intermitente": {"0251", "0284", "0286"},
    "Ausência justificada": {"8932"},
}
SELECAO = set(FOCO).union(*ESPECIAIS.values())
CAB_RUB = (
    "competencia empresa cnpj codigo_empregado_dominio rubrica descricao "
    "unidade_proposta referencia_original vencimento_brl desconto_brl pagina_pdf"
).split()
CAB_COB = (
    "competencia empresa cnpj codigo_empregado_dominio paginas_pdf "
    "linhas_rubricas_ponto recibo_conferido"
).split()
CAMPOS_DPH = {
    "META": "emp comp janela motor fm_status hash_txt codigo_dominio_empresa medido_em",
    "MAP": "emp evento rubrica fonte unidade",
    "COLAB": "emp col cod fm status export motivo juiz universo fora_janela "
             "mudanca_empresa isento fm_falta_min fm_dsr_min",
    "RUB": "emp col cod rubrica evento unidade fm txt bruto_min lido fonte_bruto",
    "FIM": "n",
}
MOTIVOS = set(
    "- rescisao_modulo_proprio sem_codigo_dominio ferias_com_batida cadastro_zero "
    "furo_espelho nao_homologado fora_universo_fechamento codigo_invalido_dominio "
    "status_fora_txt isento_eventos_horas".split()
)
UNIDADES_TXT = {"h", "dias"}


def erro(local, mensagem):
    raise SystemExit(f"{local}: {mensagem}")


def exige(sim, local, mensagem):
    if not sim:
        erro(local, mensagem)


def padrao(valor, regex, local, campo):
    exige(re.fullmatch(regex, valor) is not None, local, f"{campo} inválido")
    return valor


def inteiro(valor, local, campo, opcional=False, sinal=False):
    if opcional and valor == "-":
        return None
    regex = r"-?[0-9]+" if sinal else r"[0-9]+"
    padrao(valor, regex, local, campo)
    return int(valor)


def linhas_csv(caminho, cabecalho):
    with caminho.open(encoding="utf-8-sig", newline="") as arquivo:
        leitor = csv.reader(arquivo, delimiter=";", strict=True)
        try:
            exige(next(leitor, None) == cabecalho, f"{caminho}:1", "cabeçalho inesperado")
            while True:
                primeira = leitor.line_num + 1
                campos = next(leitor, None)
                if campos is None:
                    return
                local = f"{caminho}:{primeira}"
                exige(len(campos) == len(cabecalho), local, "número de campos incorreto")
                yield local, dict(zip(cabecalho, campos))
        except (csv.Error, UnicodeError):
            erro(f"{caminho}:{leitor.line_num + 1 if leitor.line_num == 0 else leitor.line_num}",
                 "CSV ou UTF-8 inválido")


def linhas_dph(caminho):
    total = 0
    fim = False
    with caminho.open(encoding="utf-8-sig") as arquivo:
        for numero, linha in enumerate(arquivo, 1):
            local = f"{caminho}:{numero}"
            exige(not fim, local, "dados após DPH|FIM")
            marca, tipo, *campos = linha.rstrip("\n").split("|")
            exige(marca == "DPH" and tipo in CAMPOS_DPH, local, "registro DPH inválido")
            valores = {}
            for campo in campos:
                nome, separador, valor = campo.partition("=")
                exige(separador and nome not in valores, local, "campo DPH inválido ou repetido")
                valores[nome] = valor
            exige(set(valores) == set(CAMPOS_DPH[tipo].split()), local, "campos DPH inesperados")
            if tipo == "FIM":
                exige(inteiro(valores["n"], local, "n") == total, local, "DPH incompleto")
                fim = True
            else:
                total += 1
                yield local, tipo, valores
    exige(fim, f"{caminho}:{total + 1}", "DPH sem marcador FIM")


def identidade_dp(linha, local):
    exige(linha["competencia"] == "2026-10", local, "competência diferente de 2026-10")
    exige(linha["empresa"] in EMP, local, "empresa DP desconhecida")
    codigo = padrao(linha["codigo_empregado_dominio"], r"[0-9]+", local, "código DP")
    return EMP[linha["empresa"]], codigo


def candidato(codigo):
    return codigo.lstrip("0") or "0"


def prova_zeros(indice, emp, codigo, identidade, local):
    # Usado apenas para prova. O join começa depois de ler ambas as fontes.
    chave = emp, candidato(codigo)
    anterior, origem = indice.setdefault(chave, (identidade, local))
    exige(anterior == identidade, local, f"colisão de zeros à esquerda com {origem}")


def referencia(texto, unidade, local):
    padrao(texto, r"[0-9]+(?:,[0-9]+)?", local, "referência DP")
    valor = Decimal(texto.replace(",", "."))
    if unidade == "h":
        valor *= 100
    if unidade in UNIDADES_TXT:
        exige(valor == valor.to_integral_value(), local, "precisão incompatível com a unidade")
        return int(valor)
    return valor


def ler_dp(rubricas, cobertura):
    cob, rub, prova = {}, {}, {}
    n_cob, n_rub, por_vinculo = Counter(), Counter(), Counter()
    for local, linha in linhas_csv(cobertura, CAB_COB):
        emp, codigo = chave = identidade_dp(linha, local)
        prova_zeros(prova, emp, codigo, codigo, local)
        exige(chave not in cob, local, "vínculo DP duplicado na cobertura")
        exige(linha["recibo_conferido"] == "sim", local, "recibo sem conferência")
        n = inteiro(linha["linhas_rubricas_ponto"], local, "linhas_rubricas_ponto")
        cob[chave] = (n, local)
        n_cob[emp] += 1

    for local, linha in linhas_csv(rubricas, CAB_RUB):
        emp, codigo = vinculo = identidade_dp(linha, local)
        prova_zeros(prova, emp, codigo, codigo, local)
        exige(vinculo in cob, local, "rubrica DP sem vínculo na cobertura")
        r =

[truncado em 20000 de 40900 caracteres]

## 27/11 19:56 · sessao 5063728d707e · FIM-DE-SEMANA-JA · censo · origem=Code

# FIM-DE-SEMANA-JA · Triagem T0

**O aval de 0 BO Hasner fica [nao verificado].** Contei as **38 linhas marcadas de C1, em 22 colaboradores**. A transcrição de `partida_2711.out` termina no meio de C2, sem o rodapé e sem as linhas de C4 e C5. Usei os totais do enunciado como declarados. A cópia tem o código citado abaixo. A identidade Git do HEAD ficou [nao verificado], pois a cópia não tem metadados Git acessíveis. A análise foi só por leitura, com os REDs descritos como propostas. `sonda.py:98–111`.

## C1 · Dias que seguram a folha

O retentor lê o veredito da célula e os protestos. A sonda imprime o veredito, o fio e, quando existe, o primeiro PK da fila. A divisão abaixo é exata para as **38 linhas marcadas**. A causa dentro de cada veredito fica [nao verificado] quando depende dos protestos ou da razão da tarefa, campos ausentes da saída. `app/folha/export.py:1268–1271,1282–1305`, `sonda.py:98–111`.

| Motivo / fio | Linhas / colabs | Causa e classificação |
|---|---:|---|
| `indefinida / ninguem` | 15 / 6 | A célula acusa previsto em dia indefinido, fase sem declaração ou ausência parcial sem período. A tarefa ficou sem rota. **Hasner** se nenhuma saída complementar existir. **Produção** se o dia estiver encaminhado para corrigir vínculo, escala ou ausência. A divisão fica [nao verificado]. `app/escala/models.py:1626`, `app/chamados/services/vigia_fio.py:268–278`, `app/folha/retrato_folha.py:1003–1020`. |
| `furo / ninguem` | 1 / 1 | O veredito agrega cinco protestos de furo. A tarefa ficou sem rota. **Hasner** se também faltar saída no calendário, na lista do admin ou em outro destino previsto. Protesto e saída complementar [nao verificado]. `app/escala/models.py:1623`, `app/folha/fora_da_folha.py:86–91`, `app/chamados/services/vigia_fio.py:654–706`. |
| `furo / nao_sei` | 2 / 2 | **Hasner na leitura da tarefa.** Os ramos incluem célula ausente, ata sem lâmpadas e marco sem leitura. O ramo exato por dia fica [nao verificado]. `app/chamados/services/tarefa_do_dia.py:236–246,304–310`. |
| `cobrado / nao_sei` | 1 / 1 | **Hasner na leitura da tarefa.** Há veredito de cobrança lavrado e a tarefa deixou a rota indeterminada. A razão exata fica [nao verificado]. `app/ponto/services/cartorio.py:1544–1545`, `app/chamados/services/tarefa_do_dia.py:731–733,766–779`. |
| `cobranca_indevida / ninguem` | 2 / 2 | **Candidatas Hasner.** A célula acusa cobrança com todos os marcos casados ou cobrança em dia sem trabalho, e a tarefa ficou sem rota. Qual dos dois protestos, a atualidade da lavra e a presença de uma conversa a julgar ficam [nao verificado]. `app/ponto/supra_juiz.py:479–484,507–512,578–588`. |
| `cobranca_indevida / admin` | 5 / 2 | A tarefa publicou porta de admin. **Produção** quando o gestor precisa julgar uma resposta preservada. **Hasner** quando a cobrança já perdeu a premissa, está apta a fechar e segue retendo. A divisão fica [nao verificado]. `app/ponto/supra_juiz.py:578–588`, `app/ponto/services/lastro.py:207–228`, `app/templates/chamados/partials/modal_fio.html:306–339`. |
| `discordante / colab` | 4 / 4 | **Produção se a pergunta procede.** A porta publicada é responder no app. O veredito pode vir de batida órfã, realizado inflado ou realizado zero com turno. A pertinência da cobrança por linha fica [nao verificado]. `app/escala/models.py:1627`, `app/chamados/services/tarefa_do_dia.py:748–750,775–776`, `app/api/views.py:2538–2555,2634–2656`. |
| `discordante / admin` | 8 / 5 | **Produção se a ação publicada procede.** O admin pode validar ou decidir no fio, resolver o dia no calendário ou decidir a ausência. A ação específica e o protesto por linha ficam [nao verificado]. `app/escala/models.py:1627`, `app/folha/retrato_folha.py:1016–1020`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/ponto/views.py:2095`. |

Nas **18 linhas `ninguem`**, a ausência confirmada é a da rota da tarefa. O próprio vigia ainda cruza ausência aberta, folga contestada, Cadastro x Realidade e Pauta DP. Nas **3 linhas `nao_sei`**, há também uma falha de apresentação reproduzível pela leitura estática: o retrato guarda `dias_sem_leitura` e o drawer só marca `Sem porta` ao lado da data. A quantidade de casos que esse detalhe escondeu na tela T0 fica [nao verificado]. `app/chamados/services/vigia_fio.py:252–279,630–706`, `app/folha/retrato_folha.py:1244–1245`, `app/templates/folha/_retrato_o_que_fazer.html:42–46`.

**As 1.000 linhas `aval=-` declaradas, em três linhas:**

- `espelho_*` com fio `colab` ou `admin` tem rota humana publicada. **Produção** quando falta responder no app, validar no fio, resolver no calendário ou decidir a ausência. `app/chamados/services/tarefa_do_dia.py:748–790`, `app/templates/chamados/partials/_card_atendimento.html:73–78`, `app/folha/retrato_folha.py:1016–1020`.
- `sem_codigo_dominio` tem porta humana de **produção**. Abrir Cadastro do colaborador, editar **Código folha** e salvar. `app/folha/export.py:881–884`, `app/folha/retrato_folha.py:1009`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1353–1365`.
- `ferias_com_batida` pede conferência do DP. O retrato deixa esse motivo sem link próprio. A aba Férias oferece correção do agendamento quando cabível. A ação correta para a batida do anexo fica [nao verificado]. `app/folha/export.py:885–893`, `app/folha/retrato_folha.py:1010`, `app/templates/colaboradores/partials/painel_colaborador.html:43`, `app/templates/ferias/_secao_colab.html:145–171`.

## C2 · Pergunta que segura o chamado

**Número real de chamados cujo gestor não vê ao menos uma pergunta bloqueante: [nao verificado].** Os **2.527 pares em 1.018 chamados** são o total declarado para o filtro da sonda. Esse filtro compara a disputa da pergunta com a primeira disputa própria aberta do detalhe local. Para GLPI, a disputa de comparação fica vazia. A saída não traz o container da disputa, o estado aberto ou fechado, a rota efetiva, o perfil do gestor nem as perguntas renderizadas. O teste de diferença de disputa tem **falsos positivos demonstráveis pelo código**. `app/chamados/services/acoes_chamado.py:345–358`, `app/templates/chamados/detalhe_local.html:296–310`, `app/chamados/services/fio.py:97–117,251–273,305–323`.

| Subclasse a medir | Classificação pelo código | Linhas / colabs |
|---|---|---:|
| Pergunta visível e com ação pelo modal do fio | Falso positivo de invisibilidade. O modal lista por `chamado_destino`, com fallback para o container, e pode juntar perguntas de **containers diferentes** no mesmo cartão. A viva de disputa fechada também pode entrar. **Produção** se falta validar, rejeitar ou corrigir onde o botão é aceito. O POST usa o PK da pergunta e a âncora que a lista. `app/chamados/views.py:870–890,928–936`, `app/chamados/services/fio.py:291–323,728–760,776–790`, `app/templates/chamados/partials/modal_fio.html:123,157–160,306–339`. | [nao verificado] |
| Pergunta fora do cartão, com outra via | A gaveta **detalhes do caso** pode mostrar texto e dia da disputa eleita. A prévia **Validar em lote** mostra as respostas elegíveis da classe A e permite confirmar. O botão inline atua sobre a disputa aberta eleita. Para o aval, uma pergunta apenas legível segue candidata **Hasner** se nenhuma via permite resolver o bloqueio. `app/chamados/services/fio.py:335–346,421–436`, `app/templates/chamados/partials/modal_fio.html:461–476`, `app/chamados/views_cobrar.py:145–172,371–399`, `app/templates/chamados/partials/_lote_preview.html:21–24`. | [nao verificado] |
| Pergunta ausente das vias acessíveis | **Hasner** quando a omissão esconde o bloqueio do gestor autorizado. A segunda aberta do **mesmo container** pode ficar fora da lista do modal, inclusive se tiver destino. Uma disputa sem container pode alimentar a explicação quando eleita, permanecendo fora da lista de cartões. O detalhe GLPI renderiza followups e texto livre. Pode haver menção em texto, cujo conteúdo fica [nao verificado]. Um chamado GLPI também pode aparecer no modal. `app/chamados/services/fio.py:242–273,335–348`, `app/chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:217–234`, `app/chamados/views.py:243–269`, `app/templates/chamados/detalhe.html:69–103,116–143`. | [nao verificado] |

Para contar, cruzar o **PK exato da pergunta** com o detalhe, o modal incluindo a gaveta, e a prévia de lote para o gestor autorizado. Publicar separadamente `COUNT(DISTINCT chamado_id)` com alguma pergunta invisível e com algum bloqueio sem ação alcançável. Manter os dois predicados da guarda. `ancora` é C6 no pós-estado. `conversa_pai` inclui respostas sem veredito de origem e de destino, que podem aparecer em outro cartão. Para medir todo o universo de bloqueios, preservar também respostas que o CSF segura e o segundo filtro C6 da sonda descarta. `app/chamados/models.py:332–391,718–758`, `app/ponto/supra_juiz.py:561–575`, `app/chamados/utils.py:47–72`.

A sonda etiqueta C2 com `JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO`. O código liga essa obra ao carimbo de validação transitório e à releitura da disputa na devolução. A divergência de tela é demonstrável por outra causa. Atribuir alguma linha T0 ao juiz que mudou sem fato ou a uma mudança real de dia fica [nao verificado]. `app/chamados/services/validacao.py:319–327,734–742`, `app/chamados/services/fio.py:251–273`.

## C3 · Caso que o reconciliador deixou vivo

**0 emitidos é o total declarado. A causa do zero e a identidade dos 112 ficam [nao verificado].** A sonda pode omitir candidatos antes de chamar `reconciliaria()`. No ramo de premissa, exclui o colaborador inteiro se houver evento pendente. Isso segue a cobertura prevista pelo consumidor, que agrupa por colaborador e revisita seus chamados, inclusive os de outro dia. O ramo de célula usa o PK da célula. O de lastro descarta `retido_pelo_fechamento` e preserva como escape o lastro por tolerância sem lâmpada, mesmo com evento no colaborador. A fila de lastro da sonda também deixa fora `registrado` e silenciado. `app/chamados/management/commands/reconciliar_chamados.py:62–85`, `app/chamados/reconciliador.py:1389–1398,1414–1424,1493–1533`, `app/ponto/services/lastro.py:316–319,388–402,412–424`, `app/chamados/services/painel.py:188–190`.

**Consulta para procurar os 112.** Na mesma empresa e janela, partir dos `VIVOS` e fazer a união por PK de três conjuntos, antes dos descartes. Incluir premissa morta por `_veredito_premissa`, absolvição por `_absolvido_pela_celula` no universo de `_chamados_de_turno_vivos`, e `com_lastro` mais `retido_pelo_fechamento` de `julgar`. No lastro, anotar presença na fila original e se os marcos estão acesos ou se a prova veio da tolerância. Em cada chamado, conservar todos os motivos e anotar evento por colaborador e por célula, idade, tentativas, erro, `retencao_da_retratacao` e a guarda que impede fechar. A consulta recupera os candidatos que esses filtros podem ter escondido. Retornar exatamente 112 fica [nao verificado]. `app/chamados/reconciliador.py:248,440–463,1140,1344,1547–1556`, `app/ponto/services/lastro.py:196–249,299–319`, `app/chamados/models.py:677–712`.

- **Hasner** se premissa morta, célula que absolveu ou lastro suficiente deixam um chamado apto sem gatilho efetivo. Separar evento em curso, falha ou esgotamento, e tolerância que o consumidor não executa. Subtotais [nao verificado]. `app/chamados/reconciliador.py:1389–1404,1518–1533,1547–1556`, `app/ponto/services/lastro.py:412–424`.
- **Produção** se uma resposta de irmã ou do pai ainda exige veredito e **Validar/Rejeitar** está alcançável. **Hasner** se a retenção esconde o motivo ou deixa a pergunta sem saída. Subtotais [nao verificado]. `app/chamados/reconciliador.py:440–453`, `app/chamados/models.py:718–758`, `app/templates/chamados/partials/modal_fio.html:306–339`.
- `reconciliaria()` é a decisão seca da transição. O ato completo de retratação ainda passa pela cerca de competência entregue. O resultado seco fica como coluna da consulta. `app/chamados/models.py:677–681`, `app/chamados/reconciliador.py:588–607`.

## C4 · Disputas abertas há mais de 30 dias

**260 disputas é o total declarado. Hasner sem porta, produção esperando resposta, produção esperando validação e respectivos colabs ficam [nao verificado].** `listar_c4` imprime idade e recorta a competência pelas datas disponíveis. A marca `SEM-JUIZ` é literal na sonda. A contagem por idade tem equivalente no auditor da cópia, com outro limiar. A guarda real de fechamento pergunta por C6 e por respostas sem veredito. `app/chamados/management/commands/auditar_disputas.py:132–144`, `app/chamados/juizes.py:1603–1619`, `app/chamados/models.py:2000–2053`.

A consulta de C4 deve agrupar por disputa e verificar as duas guardas e as portas de cada pergunta bloqueante:

- **Hasner, sem porta.** Alguma pergunta segura e nenhuma via resolve aquela pergunta. Isso inclui cartão oculto e botão suprimido quando falta via alternativa. `app/chamados/services/fio.py:251–273`, `app/templates/chamados/partials/modal_fio.html:123,251–276`.
- **Produção, resposta pendente.** O colaborador alcança **Responder** no app ou em Meu Atendimento. **Cobrar resposta** no modal avisa o colaborador. `app/chamados/services/pergunta_no_app.py:68–92`, `app/chamados/views.py:560–570,1229–1247`, `app/templates/chamados/partials/_card_atendimento.html:73–78`, `app/templates/chamados/partials/modal_fio.html:363–380`.
- **Produção, veredito pendente.** O gestor tem **Validar/Rejeitar/Corrigir** conforme o cartão e a guarda da ação. Uma disputa pode ter espera pelos dois atores. Para totais exclusivos, contar primeiro a disputa com algum bloqueio sem porta, depois a com veredito pendente e depois a que só espera resposta. `app/chamados/models.py:2041–2053`, `app/templates/chamados/partials/modal_fio.html:306–339`.
- **Sem bloqueante C7.** Uma disputa vazia ou com perguntas já mortas pode continuar aberta e requer inspeção do caminho de fechamento. O vigia de becos tem recorte próprio pelo pai resolvido. **Regularizar** pode devolver “Nenhuma pergunta validada para materializar”. O método `admin_encerra` existe, e sua ligação a uma tela fica [nao verificado]. `app/chamados/models.py:2047–2069,2210–2217`, `app/chamados/services/becos.py:25–35,56–61`, `app/chamados/services/acoes_disputa.py:199–203`.

## C5 · Chamados de desligados

**56 linhas declaradas, 23 `registrado s89` e 33 `aberto`/`em_analise`.** Colabs distintos, divisão dos 33 por estado e causas por PK ficam [nao verificado]. A sonda usa `situacao='desligado'` e o módulo de origem para marcar S89. `app/colaboradores/services/desligamento.py:23–32,191–217`.

- **23 S89 decl. — auditoria com porta do DP.** A emissão pede resposta oficial e aponta ao detalhe local. O catálogo os cria como `registrado` e a fila padrão de cobrança os deixa fora. O DP autorizado chega pelo link emitido ou pelo filtro `status=registrado`, abre **Responder como DP / Gestão** e envia a resposta livre. É **produção** se a conferência humana ainda estiver pendente. O estado `registrado` sozinho não informa se alguém já respondeu. `app/colaboradores/services/desligamento.py:195–223`, `app/chamados/catalogo/natureza.py:18–23,36–45`, `app/chamados/models.py:1126–1130`, `app/chamados/services/painel.py:300–316`, `app/templates/chamados/detalhe_local.html:374–407`, `app/chamados/services/acoes_chamado.py:292–305`.
- **33 abertos/em análise decl. — três causas a verificar.** Resposta já recebida sem veredito pode manter disputa e chamado abertos pela cascata. É **produção** quando o DP consegue validar ou rejeitar no fio. Se a única saída ainda exige resposta do desligado, cujo login a rotina bloqueou, é **Hasner**. Se nenhuma guarda legítima segura o chamado, cruzar a data de criação com a execução do desligamento para distinguir sobra da cascata, emissão posterior ou ação de DP ainda devida. A partição fica [nao verificado]. `app/colaboradores/services/desligamento.py:128–137,164–190`, `app/core/credenciais.py:100–114`, `app/chamados/models.py:1779–1794`, `app/templates/chamados/partials/modal_fio.html:306–339`.
- **Hasner reproduzível no texto do S89.** Quando a cascata mantém um chamado ou disputa aberto pelas guardas, o S89 ainda grava a frase de que a cascata fechou chamados e disputas abertas. A incidência nos 23 fica [nao verificado]. `app/colaboradores/services/desligamento.py:164–190,195–208`.

As obras da tabela são atribuições de triagem propostas. A confirmação de titularidade para causas novas fica [nao verificado]. Os REDs descrevem a prova a obter.

| FATIA | classe/subclasse | n | obra dona | arquivos | RED | mão |
|---|---|---:|---|---|---|---|
| Registrar causa e porta por dia | C1, todas as marcadas | 38 / 22 colabs | `REGISTRA-ACHADOS` | `sonda.py:98–111`, `app/chamados/services/vigia_fio.py:252–279,654–706` | Dois dias saem `ninguem` na sonda atual. Um tem correção de cadastro alcançável, o outro fica sem saída. A nova leitura distingue os dois e traz protesto, razão, rota e ação. | Dupla |
| Mostrar falha de leitura por data | C1, `nao_sei` | 3 / 3 colabs na sonda | `REGISTRA-ACHADOS` | `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/folha/retrato_folha.py:1244` | Mesmo colaborador com um dia com porta e outro sem leitura. O segundo exibe **Sem leitura** na data. | Exceção, toca Folha |
| Medir visibilidade e ação real | C2, detalhe local, modal, GLPI e lote | 2.527 pares / 1.018 chamados decl. Subtotais [nao verificado] | `REGISTRA-ACHADOS` | `sonda.py`, `app/chamados/services/fio.py:251–348,421–436`, `app/chamados/views_cobrar.py:371–399` | O cruzamento reconhece a pergunta de outro pai visível no modal e a elegível no lote. Separa a visível só na gaveta da ausente de todas as telas. | Dupla |
| Expor a pergunta que bloqueia | C2/C4, segunda aberta do mesmo pai ou disputa fora da carga, sem alternativa efetiva | [nao verificado] | sem obra | `app/chamados/services/fio.py:251–273,335–348`, `app/templates/chamados/partials/modal_fio.html:123,157` | Duas abertas do mesmo pai e um caso sem container. O gestor autorizado encontra o PK bloqueante e a ação aponta para a disputa certa. O perfil sem permissão segue barrado. | Dupla na UI |
| Selar a mudança sem fato | C2/C4, se reproduzir a oscilação de C6 | [nao verificado] | `JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO` | `app/chamados/services/validacao.py:319–327,734–742`, `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:1` | Duas leituras com os mesmos fatos persistidos dão o mesmo veredito. Validação recusada nunca publica o carimbo intermediário ao fechador. | Exceção se alterar materialização de ponto ou célula |
| Consumir a mudança de dia na origem | C1/C2/C4, se o dia mudou e a pergunta de turno continuou cobrando | [nao verificado] | `PERGUNTA-VIVA-NAO-MORRE-NO-DIA-MUDADO` | `app/ponto/services/veredito_celula.py:160–203`, `app/chamados/juizes.py:1499–1589` | Ato que transforma trabalho em folga encerra a cobrança do turno quando a lei libera. Uma conversa do mesmo dia civil permanece disponível para decisão. | Exceção, toca ponto e célula |
| Inventariar antes dos descartes | C3, premissa, célula e lastro | 0 emitidos decl. / 112 citados [nao verificado] | `RECONCILIAR-SEM-GATILHO` | `app/chamados/management/commands/reconciliar_chamados.py:62–87`, `app/ponto/services/lastro.py:316–319,412–424` | Candidato com evento de outro dia e candidato retido aparecem com suas razões. O evento que cobre o primeiro fica fora do alarme de falta de gatilho. | Dupla, só leitor |
| Ligar o fato ao produtor de evento | C3, apto sem gatilho confirmado | [nao verificado] | `RECONCILIAR-SEM-GATILHO` | `app/ponto/services/vigia_de_hora.py:87–113`, `app/ponto/portas/celula.py:981`, `app/chamados/reconciliador.py:1518–1533` | O prazo vence sem batida nova. O evento fecha o apto, a guarda humana conserva o retido e a segunda passagem é idempotente. | Exceção, toca ponto e célula |
| Medir bloqueio por disputa | C4, espera humana, sem porta ou sem bloqueante C7 | 260 decl. Subtotais [nao verificado] | `REGISTRA-ACHADOS` | `sonda.py`, `app/cham

[truncado em 20000 de 21108 caracteres]

## 27/11 19:55 · sessao 9cc8f9239142 · - · - · origem=Ronald (tmux)

**C2. O número de chamados entre os 1.018 em que o gestor realmente deixa de ver pelo menos uma pergunta que segura ficou [nao verificado].** A sonda compara o ID da disputa com a primeira aberta própria do detalhe local. O recorte pode produzir falso positivo e o anexo termina dentro de C2, sem estado da disputa, container, destino, RBAC, resposta ou conteúdo efetivamente mostrado por linha. O detalhe local escolhe a primeira aberta própria e inclui suas perguntas C6 vivas. `app/chamados/services/acoes_chamado.py:345–358`, `app/chamados/views.py:363,550–552`, `app/templates/chamados/detalhe_local.html:296–310`.

- **Pergunta visível no modal.** O modal de gestor do painel lê disputas abertas próprias eleitas e todas as fechadas com perguntas C6 vivas. Agrupa pelo `chamado_destino`, com fallback para o container, e exibe a lista quando o cartão tem disputa no cabeçalho e autorização departamental. Disputas de containers diferentes podem alimentar o mesmo cartão. No empate de `aberta_em`, a eleição do cabeçalho desempata por `-pk`, enquanto a carga das perguntas carece desse desempate. `app/templates/chamados/painel_gestao.html:495,528–534`, `app/chamados/views.py:870–890,928–934`, `app/chamados/services/fio.py:97–117,136–156,242–273,291–322,334–362`, `app/templates/chamados/partials/modal_fio.html:123,157–160`, `app/chamados/utils.py:47–72`.
- **Pergunta fora da lista interativa.** A segunda aberta do mesmo container fica fora da carga da lista, mesmo quando seu destino difere. Disputa sem container pode ser eleita pelo destino e servir à explicação, sem entrar na lista interativa. A gaveta de detalhes pode exibir texto e dia de itens não resolvidos da disputa eleita, inclusive quando o explicador recarrega as perguntas da segunda aberta. O fio de página inteira renderiza a conversa livre e os links do GLPI. A timeline GLPI recebe os followups externos e renderiza texto livre. O modal aceita cartões GLPI. `app/chamados/services/fio.py:251–273,335–346,421–436`, `app/chamados/services/explicador.py:98–150,166–186`, `app/templates/chamados/partials/modal_fio.html:461–476`, `app/chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:217–234`, `app/templates/chamados/fio_colaborador.html:234–285`, `app/chamados/views.py:243–269`, `app/templates/chamados/detalhe.html:69–103,116–143`.
- **Portas alternativas.** A prévia da Central mostra pergunta e resposta para a classe A da fila global de respondidas vivas, com confirmação do gestor autorizado. O inline valida a disputa aberta eleita e usualmente opera sem mostrar cada pergunta. Para contar, separe texto disponível, ação alcançável e invisibilidade real, por `(chamado, pergunta)` e depois por chamados distintos com ao menos um bloqueador invisível. `app/chamados/juizes.py:1863–1868`, `app/chamados/services/validacao.py:1283–1301,1336–1357`, `app/chamados/views_cobrar.py:145–172,180–226,370–404`, `app/templates/chamados/painel_gestao.html:14`, `app/templates/chamados/partials/_lote_preview.html:20–26`.
- **Duas guardas.** `ancora` é o filtro pela mesma âncora, na competência aberta e no pós-estado. `conversa_pai` é a resposta sem veredito nas disputas próprias ou no destino, sem que a etiqueta prove `p.conversa`. O gate registra IDs em nota quando `registrar=True`. Aplicar C6 também a essa lista pode omitir um bloqueador de CSF cujo C6 dê morto. `app/chamados/models.py:332–388,718–758,2041–2045`, `app/ponto/supra_juiz.py:552–575`.

**C4. A divisão das 260 disputas e seus colabs ficou [nao verificado].** `listar_c4` em `sonda.py` mede aberta há mais de 30 dias e usa datas de todas as perguntas ou o fato forte do chamado, mantendo também casos sem data. A saída só identifica idade e disputa. No código, o relatório análogo mede idade e motivos, e o fechamento exige zero C6 vivas e zero respostas sem veredito. Portanto há pelo menos três classes a testar. `app/chamados/management/commands/auditar_disputas.py:132–144`, `app/chamados/models.py:1930–1931,2000–2053`, `app/chamados/juizes.py:1603–1606`.

1. **Produção, aguarda resposta.** Porta do colaborador `Enviar resposta` no PWA ou a pendente no nativo. O nativo consulta C6 por colaborador, sem depender do container, e separa a pergunta sem resposta e de disputa aberta. A supervisão pode cobrar pelo modal. `app/chamados/views.py:560–570,1229–1247`, `app/templates/chamados/partials/_card_atendimento.html:73–78,163–165`, `app/chamados/services/pergunta_no_app.py:45–92`, `app/api/views.py:2538–2555,2609–2640`, `app/templates/chamados/partials/modal_fio.html:363–380`.
2. **Produção, aguarda veredito.** Validar, rejeitar ou corrigir conforme a pergunta e as guardas do modal, ou validar pelas portas alternativas da Central. Pergunta renderizada com ação suprimida e sem alternativa real passa à classe Hasner. `app/templates/chamados/partials/modal_fio.html:217–250,274–276,306–340`, `app/chamados/views.py:952–955`, `app/chamados/services/fio.py:694–709,721–760,776–790`, `app/chamados/views_cobrar.py:145–226,370–404`.
3. **Hasner sem saída, ou disputa já fechável pelo C7 a desempatar.** Se a retenção carece de porta para ambos os atores, é Hasner. Se `pode_fechar()` passa, separe vazia, mortas e eventual fecho/regularização pendente. A presença do botão Regularizar pode terminar em “Nenhuma pergunta validada para materializar”; a mera presença do botão não encerra o diagnóstico. `admin_encerra` existe no modelo para silêncio, ligação a UI [nao verificado]. `app/chamados/models.py:2047–2069,2210–2217`, `app/chamados/services/becos.py:25–35,56–61`, `app/chamados/services/vigia_fio.py:736–743`, `app/templates/chamados/partials/modal_fio.html:382–389`, `app/chamados/services/fio.py:797–824,847–848`.

**Fatias propostas.** Atribuição à obra da sonda é triagem [nao verificado]. A obra `JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO` tem causa histórica específica, hoje com releitura da disputa e validação atômica. A divergência visual isolada pode ser “sem obra”; registrar o censo em `REGISTRA-ACHADOS` é proposta [nao verificado]. `app/chamados/services/validacao.py:319–327,734–746`, `app/docs/BACKLOG.md:1209`.

- **C2/C4 apresentação, dupla.** Em `acoes_chamado.py` e `detalhe_local.html`/`detalhe.html`, indicar o bloqueador pelo ID e oferecer acesso ao cartão que o lista, preservando RBAC. Se a própria carga omite o bloqueador, ajustar `services/fio.py` e `partials/modal_fio.html` para renderizá-lo sob âncora válida, inclusive quando a disputa escolhida no cabeçalho difere. RED com dois pais no mesmo destino, duas abertas no mesmo container, sem container, GLPI, e gestor sem departamento autorizado. Provar texto e ação sobre o ID certo, mantendo a eleição do cabeçalho e os acessos. `app/chamados/services/fio.py:136–156,251–273,319–348,721–760`, `app/chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:217–234`.
- **C4 dia alterado, mão única se alcançar célula/ponto.** Hipótese `PERGUNTA-VIVA-NAO-MORRE-NO-DIA-MUDADO` [nao verificado]. Só abrir com prova de antes/depois, distinguindo dia do turno de data de conversa, marca respondida de marco vizinho e efeito da ata. RED muda o fato do dia e testa C6, C7 e ausência de cobrança indevida com trilha, mantendo viva a irmã de outro marco. `app/chamados/juizes.py:339–343,1499–1589`, `app/chamados/services/relavra_via.py:187–207,236–243`, `app/ponto/services/veredito_celula.py:160–203`.

## 27/11 19:54 · sessao 098942e39778 · - · - · origem=Ronald (tmux)

- A porta oficial calcula o fechamento em memória e passa os mesmos objetos a `classificar_export` e `montar_linhas_export`. O recálculo precisa de `somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False`. Há exceções internas que continuariam após falha, inclusive com log nominal. Interceptar essas exceções e confirmar que os IDs retornados cobrem todo o universo evita uma prévia parcial. [app/folha/views.py:214] [app/folha/views.py:275] [app/ponto/services/fechamento.py:402] [app/ponto/services/fechamento.py:1105] [app/ponto/services/fechamento.py:1111] [app/ponto/services/fechamento.py:1215] [app/ponto/services/fechamento.py:1327]
- O TXT carrega a quantidade inteira e o código da rubrica. O tipo vem do mapa efetivo e vários eventos podem convergir para a mesma rubrica. Converta cada evento antes de agregar e confronte todos os registros `10`. `falta_diurna` usa datas de ausências aprovadas e o FM tem apenas `horas_falta`. A presença de `detalhe.falta_min` no `DiaPago` é evidência diária separada e inclui falta de zero minuto. [app/folha/export.py:34] [app/folha/export.py:169] [app/folha/export.py:224] [app/folha/export.py:439] [app/folha/export.py:739] [app/folha/export.py:993] [app/ponto/models.py:427] [app/ponto/services/dia_pago.py:274]
- A identidade ausente deve permanecer sem pareamento, com `col<pk>` disponível no `RUB` para localizar o vínculo. Antes da junção, valide a normalização por empresa e fonte, rejeitando duas grafias DP distintas colapsadas na mesma chave e dois colaboradores Hasner colapsados no mesmo código. Conserve a presença de cada fonte, inclusive zero explícito, e use a cobertura para distinguir ausência de lançamento, ausência de recibo e recorte não selecionado. [formato_csv.md:3] [formato_csv.md:7] [app/folha/export.py:737] [app/folha/export.py:881]
- Na agregação, use `sum(delta)` e `sum(abs(delta))` por unidade. METODO descreve a diferença DP–TXT nas referências padronizadas e a diferença FM–TXT precisa de categoria própria. Rubricas de DSR e ausência têm evidências distintas, e `datas_abonadas` não é chamada pela rotina que monta a prévia. [app/folha/export.py:201] [app/folha/export.py:453] [app/folha/export.py:739] [nao verificado] para os resultados da competência.

## 27/11 19:54 · sessao fd87f6bca18e · - · - · origem=Ronald (tmux)

**C1 tem 38 linhas marcadas, de 22 colabs.** A sonda agrupa ausência de porta e ausência de leitura na mesma marca, mas publica `fio` distinto. O PK do chamado é só o primeiro elemento de `fila`. `sonda.py:98–111`, `app/folha/fora_da_folha.py:86–91`.

| Motivo e fio | Linhas / colabs | Triagem |
|---|---:|---|
| `indefinida / ninguem` | 15 / 6 | Previsto com dia indefinido, fase não declarada ou parcial sem período. A causa exata e a eventual porta de cadastro ficam [nao verificado]. `app/escala/models.py:1626`, `app/chamados/services/vigia_fio.py:268–278`. |
| `furo / ninguem` | 1 / 1 | A tarefa ficou sem rota. O protesto específico, o estado da pergunta e a rota alternativa ficam [nao verificado]. `app/escala/models.py:1623`, `app/chamados/services/vigia_fio.py:654–706`. |
| `furo / nao_sei`; `cobrado / nao_sei` | 2 / 2; 1 / 1 | Falta leitura da tarefa. Causa interna [nao verificado]. A tela do retrato exibe por data `Sem porta` e omite a marca distinta `dias_sem_leitura`. `app/chamados/services/tarefa_do_dia.py:236–246`, `app/chamados/services/tarefa_do_dia.py:304–310`, `app/folha/retrato_folha.py:1244`, `app/templates/folha/_retrato_o_que_fazer.html:46`. |
| `discordante / colab`; `discordante / admin` | 4 / 4; 8 / 5 | Há porta publicada. Colab responde pendência do app. Admin tem ação exigível conforme a tarefa, que pode ser validar/decidir no fio, resolver dia ou decidir ausência. A pertinência da ação por linha fica [nao verificado], pois a divergência agrega órfã, inflado e realizado zero. `app/escala/models.py:1627`, `app/api/views.py:2538–2555`, `app/api/views.py:2634–2656`, `app/folha/retrato_folha.py:1016–1020`. |
| `cobranca_indevida / ninguem`; `cobranca_indevida / admin` | 2 / 2; 5 / 2 | A cobrança sobrevive ao furo ou ao dia de trabalho. As duas primeiras têm beco na tarefa; as cinco têm porta publicada. Distinguir passivo ou erro do cartório de conversa recebida que exige decisão humana é [nao verificado]. O cartório filtra o chamado que não pode protestar antes de julgar. `app/ponto/supra_juiz.py:479–484`, `app/ponto/supra_juiz.py:507–512`, `app/ponto/services/cartorio.py:1498–1508`, `app/ponto/services/lastro.py:207–228`. |

As 18 linhas `ninguem` exigem cruzamento por dia com ausência aberta, folga contestada validada, Cadastro x Realidade e Pauta DP. A ausência dessas saídas caracteriza beco Hasner. A sonda C1 omite esse cruzamento. `sonda.py:98–111`, `app/chamados/services/vigia_fio.py:252–279`, `app/chamados/services/vigia_fio.py:650–706`.

As 1.000 linhas `aval=-` cabem em três linhas:

- `espelho_*` com fio `colab` ou `admin` tem porta da tarefa. Responder no app, validar/decidir no fio, Resolver dia no calendário ou decidir ausência, conforme a ação publicada. `app/chamados/services/tarefa_do_dia.py:748–790`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/ponto/views.py:2095`.
- `sem_codigo_dominio` tem porta humana. Editar colaborador, preencher Código folha e salvar. `app/folha/export.py:881`, `app/templates/colaboradores/partials/painel_colaborador.html:155`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1353–1365`.
- `ferias_com_batida` exige conferência do DP. Aba Férias oferece corrigir datas ou lançamento quando o cadastro estiver errado. A ação correta para a batida do anexo fica [nao verificado]. `app/folha/export.py:885–893`, `app/templates/colaboradores/partials/painel_colaborador.html:43`, `app/templates/ferias/_secao_colab.html:145–171`.

Propostas de fatia com dono formal [nao verificado]:

- **REGISTRA-ACHADOS**. Na sonda, registrar por dia os protestos, a razão e as instâncias da tarefa, a ação e a rota e o resultado de `dono_fora_do_fio`. RED com dois dias `ninguem`, um alcançado por Cadastro x Realidade e um fora da lista, que hoje saem com o mesmo fio. Mão dupla. `sonda.py:98–111`, `app/chamados/services/vigia_fio.py:252–279`.
- **REGISTRA-ACHADOS**. No drawer do retrato, exibir `dias_sem_leitura` por data. RED em dia sem leitura na mesma linha de outro dia com porta, preservando a distinção de `Sem porta`. Exceção à mão dupla por tocar Folha. `app/folha/retrato_folha.py:1244`, `app/templates/folha/_retrato_o_que_fazer.html:46`.
- **RECONCILIAR-SEM-GATILHO**, se o diagnóstico reproduzir cobrança com premissa morta apta à reconciliação. RED com os dois ramos da cobrança indevida, e controle de conversa divergente ainda à espera do admin. A alteração no cartório ou na retenção fica na exceção à mão dupla. `app/ponto/services/cartorio.py:1498–1508`, `app/folha/export.py:1282–1303`, `app/ponto/services/lastro.py:207–228`.

## 27/11 19:54 · sessao d801de3a13af · - · - · origem=Ronald (tmux)

A prévia fiel à porta da cópia usa `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False)` e entrega os mesmos objetos a `classificar_export` e `montar_linhas_export`. A montagem devolve linhas em memória. O gerador oficial aprova, audita e registra a exportação. [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:253](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:253) [app/folha/export.py:699](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:699)

O FM grava horas decimais de duas casas e guarda relógios gerais separados em minutos. Falta em dias vem da consulta de ausências aprovadas usada pela montagem. A versão do motor pertence a `DiaPago`, e `FechamentoMensal` só guarda o horário da derivação. [app/ponto/models.py:374](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/models.py:374) [app/ponto/models.py:412](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/models.py:412) [app/ponto/models.py:464](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/models.py:464) [app/folha/export.py:749](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:749)

A sonda precisa impedir que o recálculo omita vínculo em falha. O serviço registra nome ao capturar erro individual e continua. Um interceptador local do método `logger.error` que levanta `SystemExit`, a conferência de igualdade do universo e a impressão após rollback cobrem esse caminho. A execução em sessão já inicializada com logging silenciado evita a emissão normal em arquivo. [app/ponto/services/fechamento.py:1010](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1010) [app/ponto/services/fechamento.py:1327](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1327) [app/config/settings/saas.py:118](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/settings/saas.py:118)

## 27/11 19:54 · sessao 84bca40901d8 · - · - · origem=Ronald (tmux)

O universo do fechamento vem de `universo_da_competencia`, enquanto o conjunto de FMs já gravados vem da consulta por empresa atual e competência do colaborador. A união preserva tanto vínculos sem FM quanto registros gravados fora do universo atual. A janela efetiva vem de `janela_competencia`, que chama `janela_fechamento`. `app/ponto/services/fechamento.py:402–465`, `app/folha/export.py:47–51`, `app/folha/export.py:791–797`, `app/ponto/janelas.py:10–57`.

A prévia fiel ao POST usa `recalcular_fechamento_mes(..., somente_leitura=True, lavrar_dias_pagos=False, retornar_objeto=True)` e injeta os FMs destacados nas funções `classificar_export` e `montar_linhas_export`. O POST também escreve aprovação, auditoria e registro da exportação. O recálculo em leitura tem um `except` que registra nome e segue, além de caminhos que registram `INFO` em arquivos de log conforme a configuração. A sonda deve interceptar o erro para abortar de forma sanitizada e suprimir a emissão de log no escopo do recálculo. `app/folha/views.py:214–219`, `app/folha/views.py:253–275`, `app/folha/views.py:295–307`, `app/ponto/services/fechamento.py:553–560`, `app/ponto/services/fechamento.py:865`, `app/ponto/services/fechamento.py:1011`, `app/ponto/services/fechamento.py:1152–1168`, `app/ponto/services/fechamento.py:1327–1334`, `app/config/settings/saas.py:160–183`.

Os FMs guardam as quantidades `horas_*` em horas decimais arredondadas. O equivalente em minutos recuperado dessas colunas já carrega o arredondamento. O campo `derivado_em` registra o instante da derivação e o carimbo de versão pertence a `DiaPago`. O dia de falta do TXT é contado da fonte de ausências no momento da prévia. `app/ponto/models.py:374`, `app/ponto/models.py:412–428`, `app/ponto/models.py:454–464`, `app/ponto/services/fechamento.py:922–945`, `app/ponto/services/fechamento.py:1025–1028`, `app/folha/export.py:169–185`, `app/folha/export.py:749–753`.

A empresa do FM é a empresa atual do colaborador, que muda na transferência. A empresa histórica do recibo, quando preenchida, fica em `Holerite.empresa_folha` e permite sinalizar divergência de atribuição. O vínculo histórico exato por código da DP segue [nao verificado]. `app/ponto/models.py:405–409`, `app/colaboradores/views.py:1408–1429`, `app/holerite/models.py:115–125`.

## 27/11 19:53 · sessao 8a3bae80d8c7 · - · - · origem=Ronald (tmux)

**C3: 0 é o total relatado no enunciado [nao verificado].** A causa do zero e a identidade dos 112 também ficam [nao verificado]. A sonda descarta candidatos por evento, retenção e fechamento, mas só emite aprovados. O vigia da premissa conserva um contador separado de recusados e retidos. `app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:76`, `app/chamados/management/commands/reconciliar_chamados.py:107`.

**Consulta de inventário proposta.** No universo `VIVOS` da empresa e janela da sonda, fazer união por PK de chamado de três leituras antes dos descartes:

- `_veredito_premissa(ch)` com veredito morto. Registrar o fato coletado, módulo, subcategoria e motivo. `app/chamados/reconciliador.py:248`.
- `_absolvido_pela_celula(ch)` no universo de `_chamados_de_turno_vivos()`. Registrar dia e célula. `absolvidos_sem_evento()` serve de comparação, pois exclui pendente pela célula, sem aplicar as guardas da sonda. `app/chamados/reconciliador.py:1140`, `app/chamados/reconciliador.py:1344`, `app/chamados/reconciliador.py:1414`.
- Uniões dos baldes `com_lastro` e `retido_pelo_fechamento` de `julgar`, anotando todos os marcos acesos versus lastro por tolerância sem lâmpada. A comparação deve expor que `qs_na_fila` deixa fora `registrado` e silenciado. `app/ponto/services/lastro.py:239`, `app/ponto/services/lastro.py:249`, `app/ponto/services/lastro.py:288`, `app/ponto/services/lastro.py:316`, `app/chamados/services/painel.py:188`.

Por linha, projetar PK/colab/dia, todos os motivos, retenção, primeira guarda que impede fechar e evento pendente por colab e por célula ou dia, com idade, tentativas e erro. Um evento de outro dia **pode cobrir o caso**, pois o consumidor agrupa por colab e chama os três leitores do colaborador inteiro. Pode também atrasar/falhar e não fecha lastro por tolerância sem lâmpada. `app/chamados/reconciliador.py:1389`, `app/chamados/reconciliador.py:1493`, `app/chamados/reconciliador.py:1518`, `app/chamados/reconciliador.py:1522`, `app/chamados/reconciliador.py:1533`, `app/chamados/reconciliador.py:1547`, `app/ponto/services/lastro.py:394`, `app/ponto/services/lastro.py:412`. `reconciliaria()` espelha a decisão seca da transição de sistema. A retratação completa também pode recusar pela cerca de competência entregue. `app/chamados/models.py:677`, `app/chamados/models.py:691`, `app/chamados/reconciliador.py:588`.

- **HASNER condicional** nos aptos por premissa morta, célula que absolveu ou lastro sem cobertura efetiva, incluindo tolerância sem lâmpada, e nas recusas que escondem o motivo ou deixam a pergunta sem ação alcançável. Linhas/colabs [nao verificado]. `app/chamados/management/commands/reconciliar_chamados.py:19`, `app/chamados/reconciliador.py:1140`, `app/ponto/services/lastro.py:412`, `app/chamados/models.py:429`.
- **PRODUÇÃO condicional** quando a resposta de irmã ou a conversa ainda exige decisão humana e o gestor alcança **Validar** ou **Rejeitar**. Contagem [nao verificado]. `app/chamados/reconciliador.py:440`, `app/chamados/models.py:718`, `app/templates/chamados/partials/_pergunta_card.html:100`, `app/templates/chamados/partials/_pergunta_card.html:117`, `app/chamados/views.py:483`, `app/chamados/views.py:506`. Evento pendente que cobre candidato é fila do sistema. Premissa de folha ainda viva exige leitura do caso pela autoridade do dia antes de chamar de BO. `app/chamados/reconciliador.py:1518`, `app/chamados/models.py:828`.

**Fatia mínima de medição** proposta para `RECONCILIAR-SEM-GATILHO`, com mão dupla, no leitor `app/chamados/management/commands/reconciliar_chamados.py:62`. RED com um candidato e pendente de outro dia e outro candidato com guarda humana exige ambos no inventário, nenhum falso alarme de falta de gatilho e nenhuma escrita. Os selos existentes verificam alarme e retenção em `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:194` e `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:212`.

**Fatia de correção condicional** pela mesma obra se o inventário confirmar ausência de produtor. Emitir pela porta existente no ato que mata a premissa. Para óbito só pelo relógio, a extensão mínima candidata é o produtor de prazo hoje limitado à decisão de HE, com exceção de mão por tocar ponto/célula. RED vence prazo sem batida nova, produz evento e fecha só o apto, com segunda passagem idempotente. `app/ponto/services/vigia_de_hora.py:87`, `app/ponto/services/vigia_de_hora.py:113`, `app/ponto/portas/celula.py:981`, `app/chamados/reconciliador.py:1522`. Para lastro por tolerância, o teste existente mostra que o evento por si já é insuficiente, então o fechamento segue condicionado à prova da célula. `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:234`, `app/ponto/services/lastro.py:394`.

## 27/11 19:47 · sessao de9bb18c8ace · FIM-DE-SEMANA-JA · leitura · origem=Code

```python
import logging
import sys
from copy import copy
from datetime import datetime, time, timedelta
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

# modelo_sonda_S1.py, guardas de topo. app/config/settings/sombra.py:47–60.
if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit(1)
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit(1)
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit(1)

from colaboradores.models import Empresa
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from escala.models import CelulaDia, TipoEscala
from escala.services.leitor_celula import grade_da_celula
from ponto import turnos
from ponto.janelas import janela_fechamento, periodo_apuracao
from ponto.motor_calculo_v2 import ResultadoMes, get_motor, turnos_via_autoridade
from ponto.precedencia import folga_lancada_do_dia, folgas_lancadas_do_periodo
from ponto.services.fechamento import apurar_por_fatia, universo_da_competencia

EMPRESA_ID, MES, ANO = 2, 11, 2026
UM_DIA = timedelta(days=1)


class HoraDePausaDoTemplate(time):
    """Identifica, apenas na segunda passada, as horas vindas do template."""


def fecha_sem_pausa_do_template(pareador, batidas, opcoes, aberto, saida):
    # _ivm continua preenchido. Só a distância à pausa do template para
    # esta batida fica indisponível. app/ponto/turnos.py:1217–1251,1349–1376.
    pausas = tuple(
        HoraDePausaDoTemplate(
            h.hour, h.minute, h.second, h.microsecond, h.tzinfo, fold=h.fold
        )
        for h in opcoes["marcos_intervalo"]
    )
    distancia_real = turnos._dist_marco

    def distancia(instante, marcos):
        if instante == saida.timestamp and any(
            isinstance(marco, HoraDePausaDoTemplate) for marco in marcos
        ):
            return None
        return distancia_real(instante, marcos)

    opcoes_sem_pausa = dict(opcoes, marcos_intervalo=pausas)
    with patch.object(turnos, "_dist_marco", distancia):
        alternativo = pareador([copy(b) for b in batidas], **opcoes_sem_pausa)

    return any(
        par.data_turno == aberto.data_turno
        and par.entrada is not None
        and par.entrada.pk == aberto.entrada.pk
        and par.saida is not None
        and par.saida.pk == saida.pk
        and not par.aberto
        for par in alternativo
    )


def medir_c6(colab, ini, fim, escalas, celulas, alvos):
    if not alvos:
        return set()

    # Os mesmos limites inclusivos do fechamento. O recorte por vigência
    # fica com apurar_por_fatia. app/ponto/services/fechamento.py:264–282,641–653.
    inicio_busca = timezone.make_aware(datetime.combine(ini - UM_DIA, time.min))
    fim_busca = timezone.make_aware(
        datetime.combine(fim + UM_DIA, time(23, 59, 59))
    )
    batidas = list(turnos.batidas_apuraveis(colab, inicio_busca, fim_busca))
    if not batidas:
        return set()

    achados = set()
    pareador = turnos.parear_turnos

    def montar_motor(escala, _ini_fatia, _fim_fatia):
        motor = get_motor(tipo_escala=escala.tipo_escala)
        motor.celulas_alimentadas = celulas

        def ler_pareamento(
            batidas_fatia, ini_fatia, fim_fatia, *,
            tipo_escala, escala_colaborador, colaborador_id
        ):
            motor._ec = escala_colaborador
            limpas = motor.remover_duplicatas(motor._copia_de_trabalho(batidas_fatia))

            def comparar(entrada_do_pareador, **opcoes):
                if not opcoes.get("marcos_intervalo"):
                    return pareador(entrada_do_pareador, **opcoes)

                # O pareador carimba as instâncias. Guarde as entradas
                # antes da primeira passada. app/ponto/turnos.py:1356–1376.
                antes = [copy(b) for b in entrada_do_pareador]
                normal = pareador(entrada_do_pareador, **opcoes)
                for par in normal:
                    dia = par.data_turno
                    if not par.aberto or par.entrada is None or not par.batidas:
                        continue
                    if dia is None or not ini_fatia <= dia <= fim_fatia:
                        continue
                    if alvos.get(dia) != escala_colaborador.pk or dia in achados:
                        continue
                    saida = par.batidas[-1]
                    papel = getattr(saida, "_papel_ata", None) or saida.tipo
                    if (
                        papel == "S"
                        and getattr(saida, "_intra_dur", False)
                        and not getattr(saida, "_intra_ata", False)
                        and not saida.is_intervalo
                        and fecha_sem_pausa_do_template(
                            pareador, antes, opcoes, par, saida
                        )
                    ):
                        achados.add(dia)
                return normal

            # A fatia chama esta leitura, que usa a geometria do motor.
            # app/ponto/services/fechamento.py:298–311.
            # app/ponto/motor_calculo_v2.py:480–533,2421–2422,2463–2467.
            with patch.object(turnos, "parear_turnos", comparar):
                turnos_via_autoridade(motor, limpas)
            return ResultadoMes(colaborador_id=colaborador_id)

        motor.calcular_mes = ler_pareamento
        return motor

    apuracao = apurar_por_fatia(colab, ini, fim, batidas, escalas, celulas, montar_motor)
    return achados & apuracao.dias_cobertos


def medir_c7(colab, ini, fim, celulas_por_dia, folgas_12x36):
    if not folgas_12x36:
        return set()

    # A mesma cobertura aprovada que alimenta _marcar_ausencias.
    # app/escala/utils.py:1229–1233,1252–1265.
    cobertura = turnos.cobertura_ausencia_periodo(colab, ini, fim, para="folha")
    atestados_em_folga = {
        dia for dia in folgas_12x36
        if dia in cobertura and cobertura[dia].tipo == "atestado"
    }
    if not atestados_em_folga:
        return set()

    grade = grade_da_celula(colab, ini, fim, celulas=celulas_por_dia)
    return {
        linha["data"] for linha in grade["dias"]
        if linha["data"] in atestados_em_folga and linha["tipo_dia"] == "ausencia"
    }


def medir_frota():
    empresa = Empresa.objects.get(pk=EMPRESA_ID)
    ini, fim = janela_fechamento(MES, ANO, empresa)
    # O mesmo envelope de células do fechamento. As datas contadas
    # continuam restritas a ini..fim. app/ponto/services/fechamento.py:603–626.
    env_ini = periodo_apuracao(MES, ANO, 2)[0]
    env_fim = periodo_apuracao(MES, ANO, 28)[1]
    chaves_pausa = {chave for par in CelulaDia.PARES_PAUSA_DNA for chave in par}
    frota = universo_da_competencia(MES, ANO, empresa_id=empresa.pk).order_by("pk")
    achados = {"C6": set(), "C7": set()}

    for colab in frota.iterator(chunk_size=200):
        celulas = celulas_do_periodo([colab.pk], env_ini, env_fim)
        por_dia = {dia: cel for (_, dia), cel in celulas.items() if ini <= dia <= fim}
        if not por_dia:
            continue

        geradoras = {cel.escala_geradora_id for cel in por_dia.values()}
        escalas = sorted(
            escalas_do_periodo(colab.pk, ini, fim, mais=geradoras),
            key=lambda escala: escala.data_inicio,
        )
        folgas_lancadas = folgas_lancadas_do_periodo(colab, ini, fim)
        alvos_c6 = {}
        folgas_12x36 = set()

        for dia, celula in por_dia.items():
            vinculo = vinculo_do_dia(colab.pk, dia, escalas, celulas=celulas)
            if vinculo is None:
                continue

            previsto = vinculo.eh_dia_trabalho(dia, celulas=celulas)
            if previsto is False and vinculo.tipo_escala.tipo_ciclo == TipoEscala.CICLO_12x36:
                folgas_12x36.add(dia)

            if not celula.dna:
                continue
            marcos = celula.dna.get("marcos") or {}
            if any(marcos.get(chave) for chave in chaves_pausa):
                continue
            if (
                dia.weekday() == 6
                or previsto is False
                or folga_lancada_do_dia(folgas_lancadas.get(dia), vinculo) is not None
            ):
                alvos_c6[dia] = vinculo.pk

        achados["C6"].update(
            (colab.pk, dia)
            for dia in medir_c6(colab, ini, fim, escalas, celulas, alvos_c6)
        )
        achados["C7"].update(
            (colab.pk, dia)
            for dia in medir_c7(colab, ini, fim, por_dia, folgas_12x36)
        )

    return achados


nivel_anterior = logging.root.manager.disable
logging.disable(sys.maxsize)
try:
    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")

        # As chamadas são de leitura. A transação própria termina em rollback.
        # modelo_sonda_S1.py. app/ponto/turnos.py:1192–1199.
        # app/escala/services/leitor_celula.py:350–364.
        achados = medir_frota()
        transaction.set_rollback(True)
except Exception:
    # Falha encerra a sonda antes de imprimir, sem expor dados da exceção.
    raise SystemExit(1) from None
finally:
    logging.disable(nivel_anterior)

# As duas obras constam como "sem obra" no anexo T2_secB.md:7–8.
detalhes = {"C6": "saida_absorvida_pelo_template", "C7": "atestado_em_folga_12x36"}
for classe, pares in achados.items():
    for pk, dia in sorted(pares):
        print(
            f"S0|classe={classe}|colab=col{pk}|chamado=-|disputa=-|"
            f"dia={dia.isoformat()}|desde=-|obra=sem obra|det={detalhes[classe]}"
        )
    print(f"S0|TOTAL|classe={classe}|n={len(pares)}|colabs={len({pk for pk, _ in pares})}")
print("S0|FIM")
```

| Classe | Função real (arquivo:linha) | Critério | Universo | Falso positivo possível |
|---|---|---|---|---|
| C6 · ESC-12 | `batidas_apuraveis` e `parear_turnos` (`app/ponto/turnos.py:2941`, `:1192`), via `apurar_por_fatia` (`app/ponto/services/fechamento.py:152`) e `turnos_via_autoridade` (`app/ponto/motor_calculo_v2.py:480`). | No retorno final, o turno está aberto e a última batida tem papel de saída e carimbo de pausa, sem marca explícita na batida ou na ata. Na segunda passada, só se retira a influência dos marcos de pausa do template naquele instante. A mesma entrada e a mesma saída, por PK, têm de fechar no mesmo `data_turno`. (`app/ponto/turnos.py:1217–1251`, `:1310–1376`, `:1487–1502`) | Frota do fechamento da empresa 2 em 11/2026. Batidas da janela da empresa com um dia de margem em cada ponta e recorte real por vigência. Conta uma vez por colaborador e dia de jornada com vínculo, célula com DNA presente e sem nenhum marco de pausa, em domingo, folga prevista ou folga lançada válida. O DSR entra por esse recorte de domingo e folga. (`app/ponto/janelas.py:54–57`, `app/ponto/services/fechamento.py:264–290`, `:459–465`, `:641–653`, `app/escala/models.py:1080–1107`, `:1663`, `app/ponto/precedencia.py:212–239`) | Uma saída real para pausa cuja volta e saída final ficaram sem registro pode fechar no contrafactual. A intenção da batida é [nao verificado]. |
| C7 · AUS-03 | `grade_da_celula` (`app/escala/services/leitor_celula.py:350`), `cobertura_ausencia_periodo` (`app/ponto/turnos.py:2816`), `vinculo_do_dia` (`app/escala/alimentacao.py:228`) e `eh_dia_trabalho` (`app/escala/models.py:1080`). | A leitura da célula devolve `tipo_dia == "ausencia"`, a cobertura aprovada vencedora tem o tipo `atestado`, e o vínculo 12x36 responde `False` para trabalho previsto. (`app/escala/services/leitor_celula.py:397–418`, `app/ponto/turnos.py:2548–2556`, `:2833–2839`, `app/ponto/catalogo/ausencias.py:92–95`, `app/escala/models.py:1098–1107`) | Mesma frota e janela. Uma ocorrência por colaborador e dia na grade lida da célula lavrada. (`app/ponto/services/fechamento.py:459–465`, `app/ponto/janelas.py:54–57`, `app/escala/services/leitor_celula.py:377–398`) | Uma ata antiga pode conservar o rótulo de outra ausência enquanto a cobertura atual aponta para o atestado. A ata transporta `tipo_dia`, e a cobertura é consultada à parte. (`app/ponto/services/cartorio.py:749–791`, `app/ponto/turnos.py:2833–2839`) |

A conferência foi feita no código da cópia fornecida. A identidade do HEAD pelo hash informado, a execução da sonda e os totais da frota ficam [nao verificado].

## 27/11 19:43 · sessao 95e2e50649cb · FIM-DE-SEMANA-JA · leitura · origem=Code

```python
import logging
from datetime import timedelta

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

from chamados.catalogo.motor import VIVOS
from chamados.juizes import (
    FIO_SEM_PORTA,
    _dia_de_turno_da_pergunta,
    competencia_do_chamado,
    dia_do_fato,
    pergunta_viva,
)
from chamados.models import ChamadoColaborador, DisputaSupervisao, PerguntaDisputa
from chamados.reconciliador import (
    _absolvido_pela_celula,
    _chamados_de_turno_vivos,
    _veredito_premissa,
    eventos_pendentes,
    retencao_da_retratacao,
)
from chamados.services.painel import qs_na_fila
from colaboradores.models import Empresa
from colaboradores.services.desligamento import esta_desligado
from folha.fora_da_folha import _carga, _fio_da_porta, _porta, dias_que_seguram
from folha.retrato_folha import _balde
from ponto.janelas import janela_fechamento
from ponto.services.lastro import com_lastro_sem_evento, julgar as julgar_lastro
from ponto.supra_juiz import perguntas_que_seguram_csf

EMPRESA_ID = 2
MES, ANO = 11, 2026
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)

OBRAS = {
    "C1": "INVENTARIO-CLASSES-ACHADOS-1720",  # Atribuicao para triagem [nao verificado].
    "C2": "JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO",  # Aval, item (2), e obras_donas.txt.
    "C3": "RECONCILIAR-SEM-GATILHO",  # obras_donas.txt.
    "C4": "JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO",  # Atribuicao para triagem [nao verificado].
    "C5": "DESLIGADO-PORTA-DO-CHAMADO",  # obras_donas.txt.
}


def data_ou_traco(data):
    return data.isoformat() if data is not None else "-"


def id_ou_traco(pk):
    return str(pk) if pk is not None else "-"


def listar_c1(empresa, inicio, fim):
    # Universo e motivo do TXT, dias e motivos do retentor, fio da tarefa.
    # app/folha/export.py:774,1185
    # app/folha/fora_da_folha.py:70,86,125,257
    # app/folha/retrato_folha.py:89,186
    universo = dias_que_seguram(empresa, MES, ANO, hoje=HOJE)
    colabs_com_dias = [
        universo["colabs"][cid]
        for cid, (_, dias) in universo["dias"].items()
        if dias
    ]
    carga = _carga(colabs_com_dias, inicio, fim, AGORA) if colabs_com_dias else None

    for item in sorted(universo["itens"], key=lambda item: item["colaborador_id"]):
        if _balde(item) != "bloqueado":
            continue
        cid = item["colaborador_id"]
        motivo = item["motivo"]
        if motivo != "furo_espelho":
            for dia in item.get("dias_ferias_com_batida") or (None,):
                if dia is None or inicio <= dia <= fim:
                    yield cid, None, None, dia, None, f"motivo={motivo} aval=-"
            continue

        _, dias = universo["dias"][cid]
        if not dias:
            yield cid, None, None, None, None, "motivo=furo_espelho sem_dia aval=-"
        for retencao in dias:
            dia = retencao["data"]
            if not inicio <= dia <= fim:
                continue
            porta = _porta(universo["colabs"][cid], dia, carga)
            fio = _fio_da_porta(porta)
            marcas = []
            if fio in (FIO_SEM_PORTA, "nao_sei"):
                marcas.append("fio_ninguem_nao_sei")
            if retencao["veredito"] in ("discordante", "cobranca_indevida"):
                marcas.append(retencao["veredito"])
            fila = (porta or {}).get("fila") or ()
            fio_exibido = "ninguem" if fio == FIO_SEM_PORTA else fio
            detalhe = (
                f"motivo={retencao['motivo']} fio={fio_exibido} "
                f"aval={','.join(marcas) or '-'}"
            )
            yield cid, next(iter(fila), None), None, dia, None, detalhe


def dia_da_pergunta(pergunta):
    # C6 separa dia do turno de conversa. O degrau de disputa usa a celula
    # para a pergunta legada sem data. app/chamados/juizes.py:58,339
    dia = _dia_de_turno_da_pergunta(pergunta)
    if dia is None and not pergunta.conversa and pergunta.celula_id:
        return pergunta.celula.data
    return dia


def listar_c2(chamados, inicio, fim):
    for ch, dia_chamado in chamados.values():
        # A guarda do fechamento humano usa C6 no pos-estado.
        # app/chamados/models.py:160,174,332,368
        candidatas = {p.pk: (p, "ancora") for p in ch._vivas_ancoradas("fechado")}
        # A conversa do pai pode barrar a reconciliacao, inclusive pelo destino.
        # app/chamados/models.py:705,718 e app/ponto/supra_juiz.py:561
        if not ch._gate_conversa_do_pai(registrar=False):
            extras = PerguntaDisputa.objects.filter(
                pk__in=perguntas_que_seguram_csf(ch)
            ).select_related(
                "disputa__colaborador", "chamado_destino", "celula"
            ).prefetch_related("decisoes")
            for p in extras:
                candidatas.setdefault(p.pk, (p, "conversa_pai"))
        if not candidatas:
            continue

        # So o recorte de exibicao da tela. O C6 ja decide a vida da pergunta.
        # app/chamados/views.py:219,262,359,550
        # app/chamados/services/acoes_chamado.py:345
        # app/templates/chamados/detalhe_local.html:296
        disputa_na_tela = None
        if ch.ticket_id_glpi is None:
            disputa_na_tela = ch.disputa_origem.filter(
                fechada_em__isnull=True
            ).values_list("pk", flat=True).first()

        for pk in sorted(candidatas):
            pergunta, guarda = candidatas[pk]
            disputa = pergunta.disputa
            if disputa.colaborador.empresa_id != EMPRESA_ID:
                continue
            if disputa.pk == disputa_na_tela or not pergunta_viva(pergunta, hoje=HOJE):
                continue
            dia = dia_da_pergunta(pergunta) or dia_chamado
            if dia is None or inicio <= dia <= fim:
                yield (
                    ch.colaborador_id, ch.pk, disputa.pk, dia,
                    timezone.localtime(disputa.aberta_em).date(),
                    f"pergunta={pk} guarda={guarda}",
                )


def listar_c3(base, chamados, inicio, fim):
    # Leitura seca da mesma decisao de reconciliar: app/chamados/models.py:661,677,683.
    # Tres caminhos do consumidor: app/chamados/reconciliador.py:1518,1522,1533.
    pendentes = list(eventos_pendentes().values_list("colaborador_id", "celula_id"))
    colabs_com_evento = {cid for cid, _ in pendentes}
    celulas_com_evento = {celula_id for _, celula_id in pendentes}
    achados = {}

    # Mesmo universo do vigia de premissa morta.
    # app/chamados/management/commands/reconciliar_chamados.py:59
    for ch, _ in chamados.values():
        if ch.colaborador_id in colabs_com_evento:
            continue
        veredito = _veredito_premissa(ch)
        if veredito is None or veredito.viva:
            continue
        if retencao_da_retratacao(ch) is None and ch.reconciliaria():
            achados[ch.pk] = veredito.motivo

    # Vigia da celula, com as guardas secas da porta de retratacao.
    # app/chamados/reconciliador.py:440,558,1083,1140,1414
    for ch in _chamados_de_turno_vivos().filter(pk__in=chamados):
        if ch.pk in achados:
            continue
        absolvido = _absolvido_pela_celula(ch)
        if absolvido is None:
            continue
        dia, celula = absolvido
        if inicio <= dia <= fim and celula.pk not in celulas_com_evento:
            if retencao_da_retratacao(ch) is None and ch.reconciliaria():
                achados[ch.pk] = "celula_absolveu_sem_evento"

    # O proprio vigia chama reconciliaria() em julgar_lastro.
    # app/ponto/services/lastro.py:196,316,412
    fila = qs_na_fila(_base=base.filter(pk__in=chamados), agora=AGORA)
    for ch, _, dia in com_lastro_sem_evento(julgar_lastro(base=fila)):
        if inicio <= dia <= fim:
            achados.setdefault(ch.pk, "lastro_sem_evento")

    for pk, motivo in sorted(achados.items()):
        ch, dia = chamados[pk]
        yield (
            ch.colaborador_id, pk, None, dia,
            timezone.localtime(ch.criado_em).date(), motivo,
        )


def listar_c4(inicio, fim):
    # SEM-JUIZ para >30 dias civis. Consulta mais proxima usa aberta_em
    # e fechada_em, com limiar de 7 dias.
    # app/chamados/management/commands/auditar_disputas.py:132
    # app/chamados/models.py:1930,1997
    disputas = DisputaSupervisao.objects.filter(
        colaborador__empresa_id=EMPRESA_ID,
        fechada_em__isnull=True,
        aberta_em__date__lt=HOJE - timedelta(days=30),
    ).select_related("chamado__celula").prefetch_related("perguntas__celula").order_by("pk")

    for disputa in disputas:
        datas = {
            dia for pergunta in disputa.perguntas.all()
            if (dia := dia_da_pergunta(pergunta)) is not None
        }
        if not datas and disputa.chamado_id:
            dia_chamado = dia_do_fato(disputa.chamado, fortes=True)
            if dia_chamado is not None:
                datas.add(dia_chamado)
        na_competencia = [dia for dia in datas if inicio <= dia <= fim]
        if datas and not na_competencia:
            continue
        desde = timezone.localtime(disputa.aberta_em).date()
        yield (
            disputa.colaborador_id, disputa.chamado_id, disputa.pk,
            min(na_competencia, default=None), desde,
            f"SEM-JUIZ idade_dias={(HOJE - desde).days}",
        )


def listar_c5(chamados):
    # app/colaboradores/services/desligamento.py:23,191
    for ch, dia in chamados.values():
        if esta_desligado(ch.colaborador):
            s89 = " s89=1" if ch.modulo_origem == "desligamento_revisao" else ""
            yield (
                ch.colaborador_id, ch.pk, None, dia,
                timezone.localtime(ch.criado_em).date(),
                f"estado={ch.status_local}{s89}",
            )


def colher():
    with schema_context("juliani"):
        # Transacao manual READ ONLY e rollback. Dentro de atomic(), ate a
        # leitura seca adquire SELECT FOR UPDATE. app/chamados/models.py:761,773
        connection.set_autocommit(False)
        try:
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")

            # O leitor da tranca chega a get_user_sistema(), que tem caminho
            # de criacao. Exigir a conta na mesma foto garante o caminho de
            # leitura. app/chamados/juizes.py:1321 e app/core/utils.py:6
            if not User.objects.filter(username="sistema").exists():
                raise SystemExit("PAREI: usuario de sistema ausente na Sombra")

            empresa = Empresa.objects.get(pk=EMPRESA_ID)
            inicio, fim = janela_fechamento(MES, ANO, empresa)  # app/ponto/janelas.py:54
            base = ChamadoColaborador.objects.filter(
                colaborador__empresa_id=EMPRESA_ID,
                status_local__in=VIVOS,
            ).select_related("colaborador__empresa", "celula").order_by("pk")

            chamados = {}
            for ch in base:
                competencia = competencia_do_chamado(ch)  # app/chamados/juizes.py:2536
                dia = competencia.dia if competencia is not None else None
                if dia is None or inicio <= dia <= fim:
                    chamados[ch.pk] = (ch, dia)

            return {
                "C1": list(listar_c1(empresa, inicio, fim)),
                "C2": list(listar_c2(chamados, inicio, fim)),
                "C3": list(listar_c3(base, chamados, inicio, fim)),
                "C4": list(listar_c4(inicio, fim)),
                "C5": list(listar_c5(chamados)),
            }
        finally:
            connection.rollback()
            connection.set_autocommit(True)


def medir():
    fabrica_anterior = logging.getLogRecordFactory()

    def log_da_sonda(*args, **kwargs):
        registro = fabrica_anterior(*args, **kwargs)
        erro = registro.exc_info[0] if registro.exc_info else None
        if erro or registro.levelno >= logging.ERROR:
            tipo = erro.__name__ if erro else "erro registrado"
            raise SystemExit(
                f"PAREI: {tipo} em {registro.filename}:{registro.lineno}"
            )
        # Impede que avisos do codigo chamado imprimam contexto livre ou
        # escrevam nos handlers de arquivo. app/config/settings/saas.py:119
        registro.levelno = logging.NOTSET - 1
        return registro

    # Alguns leitores registram a excecao e devolvem um valor conservador.
    # Aqui a excecao registrada encerra a medicao. app/chamados/juizes.py:1577
    # app/colaboradores/services/calendario.py:162 e app/core/observ.py:73
    logging.setLogRecordFactory(log_da_sonda)
    try:
        return colher()
    except Exception as erro:
        ponto = erro.__traceback__
        while ponto.tb_next is not None:
            ponto = ponto.tb_next
        arquivo = ponto.tb_frame.f_code.co_filename.rsplit("/", 1)[-1]
        raise SystemExit(
            f"PAREI: {type(erro).__name__} em {arquivo}:{ponto.tb_lineno}"
        ) from None
    finally:
        logging.setLogRecordFactory(fabrica_anterior)


print(f"S0|HOJE|{HOJE.isoformat()}")
resultado = medir()
for classe, itens in resultado.items():
    for cid, chamado, disputa, dia, desde, detalhe in itens:
        detalhe = " ".join(detalhe.replace("|", "/").split())
        print(
            f"S0|classe={classe}|colab=col{cid}|chamado={id_ou_traco(chamado)}"
            f"|disputa={id_ou_traco(disputa)}|dia={data_ou_traco(dia)}"
            f"|desde={data_ou_traco(desde)}|obra={OBRAS[classe]}|det={detalhe}"
        )
for classe, itens in resultado.items():
    print(f"S0|TOTAL|classe={classe}|n={len(itens)}|colabs={len({item[0] for item in itens})}")
print("S0|FIM")
```

| Classe | Função real | Universo e unidade de `n` | Obra dona | O que ainda pode dar falso positivo |
|---|---|---|---|---|
| C1 | `classificar_export` decide o TXT, `motivos_retencao_celula` devolve o motivo por dia e a tarefa publica o fio que `o_que_segura` lê. A aprovação usa a saída `travas_aprovacao` do mesmo juiz. `app/folha/export.py:774`, `app/folha/export.py:1185`, `app/folha/fora_da_folha.py:86`, `app/folha/fora_da_folha.py:257`, `app/ponto/views.py:1317` | Bloqueados do Retrato entre os fechamentos gravados da empresa 2 na competência. Uma linha por pessoa e dia retido, ou uma linha por pessoa quando a fonte só devolve o motivo. `aval=` identifica cada um dos três grupos pedidos. `app/folha/export.py:791`, `app/folha/retrato_folha.py:89`, `app/folha/retrato_folha.py:1197`, `app/folha/retrato_folha.py:178` | `INVENTARIO-CLASSES-ACHADOS-1720` para triagem [nao verificado] | O total inclui motivos operacionais como cadastro. Uma pessoa marcada também pode ter dias com ação humana disponível. `app/folha/export.py:881`, `app/folha/fora_da_folha.py:94` |
| C2 | `pergunta_viva`, `_vivas_ancoradas` na guarda humana e `perguntas_que_seguram_csf` na guarda de conversa do pai. `app/chamados/juizes.py:911`, `app/chamados/models.py:174`, `app/chamados/models.py:332`, `app/chamados/models.py:718`, `app/ponto/supra_juiz.py:561` | Um par chamado vivo × pergunta bloqueadora viva. Para gestor, o detalhe local mostra as vivas da primeira disputa aberta cujo `chamado` é o próprio item. O detalhe GLPI renderiza a timeline. `app/chamados/services/acoes_chamado.py:345`, `app/templates/chamados/detalhe_local.html:296`, `app/chamados/views.py:262`, `app/templates/chamados/detalhe.html:69` | `JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO`. A fatia do dia mudado está em `PERGUNTA-VIVA-NAO-MORRE-NO-DIA-MUDADO`. `obras_donas.txt:3` | A guarda humana pode barrar enquanto a porta automática consegue reatar a pergunta. O fio de atendimento também pode mostrá-la sob o chamado-destino. `app/chamados/models.py:393`, `app/chamados/models.py:429`, `app/chamados/services/fio.py:97` |
| C3 | `ChamadoColaborador.reconciliaria()` consulta a mesma decisão de `reconciliar()`. Os candidatos vêm dos vigias de premissa, célula e lastro. `app/chamados/models.py:661`, `app/chamados/models.py:677`, `app/chamados/management/commands/reconciliar_chamados.py:64`, `app/chamados/reconciliador.py:1414`, `app/ponto/services/lastro.py:412` | Um chamado vivo por PK, com premissa morta sem evento pendente do colaborador, absolvição sem evento da célula, ou lastro na fila fora do alcance do evento. `app/chamados/management/commands/reconciliar_chamados.py:59`, `app/chamados/reconciliador.py:1414`, `app/ponto/services/lastro.py:412` | `RECONCILIAR-SEM-GATILHO`. `obras_donas.txt:1` | A foto mede a fila atual. O histórico de chamadas do reconciliador está [nao verificado]. A cerca de competência entregue ainda pode vetar a retratação no ato. `app/chamados/reconciliador.py:558` |
| C4 | **SEM-JUIZ** para o limiar solicitado. A auditoria mais próxima consulta `fechada_em` e `aberta_em` com corte de sete dias. `DisputaSupervisao.pode_fechar` julga o fechamento pela vida das perguntas e por resposta sem veredito. `app/chamados/management/commands/auditar_disputas.py:132`, `app/chamados/models.py:1997`, `app/chamados/models.py:2047` | Uma disputa aberta com mais de 30 dias civis desde `aberta_em`. Em disputa com vários dias, imprime o primeiro dia da competência. Usa o dia forte do chamado vinculado quando as perguntas carecem de dia. `app/chamados/models.py:1930`, `app/chamados/juizes.py:58`, `app/chamados/juizes.py:89`, `app/chamados/services/disputa_emissao.py:174` | `JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO` para triagem [nao verificado] | A disputa pode estar aguardando ação legítima. Uma reabertura conserva `aberta_em`, então a idade impressa é a da abertura original. `app/chamados/models.py:2047`, `app/chamados/models.py:2419`, `app/chamados/models.py:2443` |
| C5 | `esta_desligado`, registrado como juiz do estado. `app/colaboradores/services/desligamento.py:23`, `app/core/juizes.py:283` | Um chamado em `VIVOS` por PK. O catálogo inclui `registrado`. A sonda identifica a revisão S89 com `s89=1`. `app/chamados/catalogo/motor.py:24`, `app/colaboradores/services/desligamento.py:191` | `DESLIGADO-PORTA-DO-CHAMADO`. `obras_donas.txt:4` | O S89 nasce para auditoria, e a guarda vigente pode conservar um chamado com pergunta viva aguardando veredito. `app/colaboradores/services/desligamento.py:168`, `app/colaboradores/services/desligamento.py:191` |

A janela vem de `janela_fechamento`. Nos chamados, `competencia_do_chamado` escolhe o fato datado mais recente. Os vivos sem dia entram no recorte da empresa. Em C3 e C5, `desde` é a criação do chamado. Em C2 e C4, é a abertura da disputa. `colabs` contabiliza os PKs distintos nas linhas da classe. `app/ponto/janelas.py:54`, `app/chamados/juizes.py:2536`, `app/chamados/models.py:79`, `app/chamados/models.py:1930`

O saldo dos 17 e dos 112, a inclusão de `col824` / chamado `23501` / disputa `5271`, a execução da sonda e a correspondência Git da cópia ao hash informado estão [nao verificado].

## 27/11 19:40 · sessao c85881a0f8bd · - · - · origem=Ronald (tmux)

C2 pode consultar `ch._vivas_ancoradas('fechado')`, a mesma guarda que recusa a transição humana, e confirmar vida atual com `pergunta_viva(p, hoje=HOJE)`. O detalhe local do gestor mostra as vivas da primeira disputa aberta cujo `chamado` é o próprio item. Uma viva de outra disputa, inclusive fechada, pode travar por `chamado_destino` e ficar fora desse detalhe. A confirmação do caso nominal depende dos vínculos e do estado no banco [nao verificado]. `app/chamados/models.py:160-183`, `app/chamados/models.py:332-391`, `app/chamados/juizes.py:911`, `app/chamados/views.py:359-363`, `app/chamados/views.py:550-552`, `app/chamados/services/acoes_chamado.py:337-358`, `app/templates/chamados/detalhe_local.html:296-310`.

Na porta automática, `_plano_pergunta_viva` identifica as vivas e decide se todas podem ser reatadas. `_reatas is None` é a recusa. O gate percorre premissa, conversa do pai e pergunta viva nessa ordem. A conversa do pai usa a lista de respostas sem veredito, que deve passar pelo C6 caso seja incluída em C2. `app/chamados/models.py:393-414`, `app/chamados/models.py:429-450`, `app/chamados/models.py:683-740`, `app/ponto/supra_juiz.py:561-575`.

`reconciliaria()` é a prévia seca de C3. Em `atomic`, a prévia solicita `SELECT FOR UPDATE`, que exige transação sem `READ ONLY`. O vigia existente mede premissa morta sem evento pendente, consulta a retenção da retratação e depois a prévia. Isso oferece um universo mais preciso que consultar só a elegibilidade, embora a correspondência com os 112 continue [nao verificado]. `app/chamados/models.py:677-681`, `app/chamados/models.py:761-778`, `app/chamados/models.py:2977-3001`, `app/chamados/management/commands/reconciliar_chamados.py:59-85`.

C4 pode usar a consulta de `auditar_disputas`, que seleciona disputa aberta por idade desde `aberta_em`. O antigo encerramento baseado em silêncio de 30 dias foi removido. A idade de abertura e a data do fato devem ser tratadas separadamente. `app/chamados/management/commands/auditar_disputas.py:132-143`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61-64`, `app/chamados/models.py:1930-1931`, `app/chamados/juizes.py:339-343`.

## 27/11 19:38 · sessao ae9d801bbcb5 · - · - · origem=Ronald (tmux)

C1 deve partir de `dias_que_seguram` (`app/folha/fora_da_folha.py:125`), que usa `classificar_export` para selecionar os retidos entre os fechamentos mensais gravados da empresa e competência (`app/folha/export.py:774`, `:791`, `:977`). A aprovação consulta o mesmo juiz pela saída `travas_aprovacao` (`app/ponto/views.py:1317`, `:1326`). Os dias e seus motivos vêm de `motivos_retencao_celula(com_dias=True)` (`app/folha/export.py:1185`, `:1268`, `:1302`), e o fio vem de `carga_da_tarefa`, `porta_da_tarefa` e `_fio_da_porta` (`app/chamados/services/tarefa_do_dia.py:809`, `:825`, `app/folha/fora_da_folha.py:86`). O juiz publica `nada`, gravado pelo Retrato como `ninguem` (`app/chamados/juizes.py:1789`, `app/folha/retrato_folha.py:178`, `:186`). Marcar o fio e os dois vereditos independentemente preserva dias mistos (`app/folha/fora_da_folha.py:94`, `:234`). O total histórico de 17 permanece [nao verificado].

C3 tem `reconciliaria()` sem escrita, mas dentro de `atomic()` a leitura adquire `SELECT FOR UPDATE` (`app/chamados/models.py:677`, `:773`, `:2977`). A transação PostgreSQL `READ ONLY` do molde conflita com essas travas. Para preservar a guarda SQL de leitura, rode C3 fora do `atomic()`. Uma foto única exige transação comum com rollback e as travas do juiz. O vigia de premissa, com o filtro de evento pendente, e o vigia de lastro fornecem recortes reais para qualificar “sem gatilho” (`app/chamados/management/commands/reconciliar_chamados.py:59`, `:64`, `:79`, `app/ponto/services/lastro.py:412`).

## 27/11 19:38 · sessao 1d50086870d4 · - · - · origem=Ronald (tmux)

C3 usa `ChamadoColaborador.reconciliaria()` (`app/chamados/models.py:677`), que chama o mesmo `_decisao_do_fechamento` de `reconciliar()` com `registrar=False` (`app/chamados/models.py:661–716`).

O vigia existente conta chamados em `VIVOS` de colaboradores sem `EventoCelula` pendente, com `_veredito_premissa(ch)` morto, `retencao_da_retratacao(ch) is None` e `ch.reconciliaria()` verdadeiro (`app/chamados/management/commands/reconciliar_chamados.py:59–85`, `app/chamados/reconciliador.py:248`, `app/chamados/reconciliador.py:440`, `app/chamados/reconciliador.py:1389`). A aplicação direta do gate a todos os vivos inclui registros e chamados sem obrigação de fecho pelo reconciliador (`app/chamados/catalogo/motor.py:24–29`, `app/chamados/catalogo/motor.py:111`, `app/chamados/models.py:793–794`).

Dentro de `atomic`, o dry adquire `SELECT FOR UPDATE`, incompatível com a transação SQL `READ ONLY`. Uma transação manual com autocommit desativado, `REPEATABLE READ, READ ONLY` e rollback em `finally` mantém a sonda em uma foto consistente, com `connection.in_atomic_block` falso para o juiz (`app/chamados/models.py:761–781`, `app/chamados/models.py:2978–3005`).

A equivalência com os 112 e o histórico de gatilhos de cada linha estão [nao verificado]. A cerca de competência entregue ainda pode vetar a retratação, mesmo com aprovação do gate seco (`app/chamados/reconciliador.py:558–599`).

## 27/11 19:30 · sessao 794e306372fc · INVENTARIO-PELA-REGRA · leitura · origem=Code

## A. Triagem dos sete casos

Os resultados observados vêm de `s.log` e estão referenciados pelas linhas das asserções. A classificação usa a L-099 e o gabarito dos testes. A íntegra do aval L4 está [nao verificado]. `app/docs/LEIS.md:122`, `app/docs/LEIS.md:136`, `app/folha/tests/test_inventario_competencia.py:336`, `:362`, `:383`.

| Caso | Subteste | Regra | Sistema | Dono | Sítio | Nota |
|---|---|---|---|---|---|---|
| AUS-05 | Asserção direta de `furo_apurado` | L-039/L-088. Com entrada às 08:00 e saída sem resposta, preservar **480 min previstos** e manter o dia **em aberto**, com **0 min de falta decidida** e sem desconto de DSR por esse evento. `app/docs/LEIS.md:87`, `:120`, `app/ponto/tests/test_inventario_ausencia.py:98–128` | A pergunta continuou sem resposta e sem validação, e os **480 min** passaram. `furo_apurado` deu **`False`**. Falta e desconto de DSR nesta rodada [nao verificado]. `app/ponto/tests/test_inventario_ausencia.py:120–128` | **INSTRUMENTO**. A asserção lê a parcela de ausência apurada, enquanto a entrada sem saída aparece na parcela de turno aberto. `app/ponto/services/dia_decidido.py:284–319` | A ETIQUETA aponta para `app/ponto/services/dia_pago.py:346`, que grava a lista filtrada pelo fechamento. O espelho publica os turnos abertos e o cartão compõe `datas_em_aberto`. `app/ponto/services/fechamento.py:1139`, `:1250`, `app/ponto/services/espelho.py:485`, `app/relatorios/cartao_pela_celula.py:419–422` | Manter a fixture da C3, que já omite `instancia_marco`. Conferir a data em `resumo['datas_em_aberto']` e a ausência de falta decidida, preservando os 480 min e as asserções da pergunta. `app/ponto/tests/test_inventario_ausencia.py:113–128`, `app/ponto/services/dia_decidido.py:330–363` |
| CMP-02 | Asserção direta de alcance municipal | L-026/L-095, Lei 605/49 art. 9º e Súmula 146. Em **10/12**, reconhecer o feriado municipal e, sem compensação, lançar **480 min de base a 200%**, com **`800`** na rubrica de teste de 12/2026 e DSR preservado. `app/folha/tests/test_inventario_competencia.py:162–183` | A janela passou. **10/12/2026** ficou fora de **`set()`**. Base, dobra, TXT e DSR [nao verificado]. `app/folha/tests/test_inventario_competencia.py:176–183` | **INSTRUMENTO**. O feriado foi cadastrado, mas faltou a FK `Posto.municipio` no posto do vínculo. A praça com cidade e UF é insuficiente para o juiz de alcance. `app/folha/tests/test_inventario_competencia.py:58–63`, `app/ponto/tests/test_inventario_escala.py:50`, `:103–117`, `app/core/feriados.py:40–66` | `autoridade_do_periodo` lê o município do posto do vínculo do dia. A ETIQUETA aponta para a rubrica posterior. `app/ponto/services/espelho.py:1063–1066`, `app/core/feriados.py:124–132`, `:160–167`, `app/folha/tests/test_inventario_competencia.py:164` | Associar `Municipio` Londrina/PR ao posto da fixture e manter a verificação de alcance antes de medir os 480 min e o TXT. `app/colaboradores/models.py:230–231`, `app/core/tests/test_contract_feriado.py:86`, `app/folha/tests/test_inventario_competencia.py:177–183` |
| CMP-03 | Asserção direta de alcance municipal | L-026, Lei 605/49 arts. 6º e 9º e Súmula 146. Reconhecer **480 min** no feriado de **10/12**, a folga compensatória integral em **11/12**, **zero na rubrica de feriado** e DSR preservado. `app/folha/tests/test_inventario_competencia.py:185–211` | A autorização e a janela passaram. **10/12/2026** ficou fora de **`set()`**. Efeito da compensação, dobra e DSR [nao verificado]. `app/folha/tests/test_inventario_competencia.py:197–211` | **INSTRUMENTO**. A fixture repete o cadastro de feriado sem `Posto.municipio`. O juiz devolve o conjunto condizente com o posto que recebeu. `app/folha/tests/test_inventario_competencia.py:58–63`, `:189–194`, `app/ponto/tests/test_inventario_escala.py:103–117`, `app/core/feriados.py:57–66`, `:124–132` | Alcance em `app/core/feriados.py:64–66` e `:160–167`. A ETIQUETA aponta para a rubrica no fechamento, depois do alcance. `app/folha/tests/test_inventario_competencia.py:187`, `app/ponto/services/fechamento.py:979` | Associar Londrina/PR ao posto e preservar a ausência aprovada, os 480 min de trabalho e a checagem de zero na rubrica. `app/core/tests/test_contract_feriado.py:86`, `app/ponto/tests/test_inventario_escala.py:150–155`, `app/folha/tests/test_inventario_competencia.py:193–211` |
| CMP-04 | Asserção direta de alcance estadual | L-023/L-026 e Lei 605/49 arts. 1º e 6º–9º. O domingo **13/12** é feriado estadual e DSR da escala. Esperados **0 min previstos e trabalhados**, **um DSR preservado**, zero perdido e zero reflexo. `app/docs/LEIS.md:74`, `:77`, `app/folha/tests/test_inventario_competencia.py:77–82`, `:213–228` | A janela passou. **13/12/2026** ficou fora de **`set()`**. A contagem de DSR [nao verificado]. `app/folha/tests/test_inventario_competencia.py:226–228` | **INSTRUMENTO**. O alcance estadual também exige a UF do `Municipio` do posto do vínculo. Esse campo ficou vazio na fixture. `app/ponto/tests/test_inventario_escala.py:103–117`, `app/core/feriados.py:50–63`, `:124–132` | Alcance estadual em `app/core/feriados.py:62–63`, lido por `app/ponto/services/espelho.py:1063–1066`. A ETIQUETA aponta para o contador posterior de DSR comercial. `app/folha/tests/test_inventario_competencia.py:215` | Associar ao posto um `Municipio` com UF `PR`, manter o feriado estadual no domingo e conservar a asserção de **um** DSR. `app/colaboradores/models.py:230–231`, `app/core/tests/test_contract_feriado.py:86`, `app/folha/tests/test_inventario_competencia.py:77–82`, `:217–228` |
| CMP-12 | 01/2027 · elegibilidade | L-026/L-095 e aval L4 citado no teste. A saída em **20/01** inclui o último dia. Os **720 min** pertencem a 01/2027 e a pendência anterior deixa de reter após o desligamento. `app/folha/tests/test_inventario_competencia.py:335–359` | A conferência de retenção aceitou `[]`, mas o ID **15** ficou fora de **`set()`** de elegíveis. `app/folha/tests/test_inventario_competencia.py:121–128`, `:345–359` | **ESTRUTURA**. O crivo de rescisão exclui demissão até o fim da janela, inclusive. `app/folha/export.py:54–60`, `:875–880` | **Mesma ETIQUETA**, `app/folha/export.py:875`. O predicado nasce em `:60` e é aplicado antes da retenção. O ensaio local reproduziu a exclusão em `app/folha/tests/_inventario_c3_mutantes.py:146–156`. | A dispensa da retenção por célula respondeu. O corte de rescisão é o que exclui este fechamento do TXT mensal. `app/folha/export.py:875–880`, `:1214–1222` |
| CMP-12 | 01/2027 · movimento | Os **720 min** até 20/01 devem aparecer no movimento da competência, como **`1200`** centésimos de hora no gabarito. `app/folha/tests/test_inventario_competencia.py:335–359`, `app/folha/export.py:38–43` | **`1200` ausente de `[]`** entre os registros que o teste procura. `app/folha/tests/test_inventario_competencia.py:130–141`, `:359` | **ESTRUTURA** na exclusão. Há também uma limitação de **INSTRUMENTO** na leitura da rubrica. `app/folha/export.py:723–746`, `:875–880`, `app/folha/tests/test_inventario_competencia.py:132–141` | **Mesma ETIQUETA** para a exclusão. O montador pula o colaborador fora de `entra_set`. A lista de eventos também carece de normais. `app/folha/export.py:875`, `:725–736`, `:439–461` | Conferir os **720 min** no fechamento lido e a elegibilidade em asserções próprias. A exigência de normais no TXT fica sem campo nesta cópia. O filtro por “qualquer outra rubrica” não identifica normais. `app/folha/tests/test_inventario_competencia.py:124–141`, `app/ponto/services/fechamento.py:922–923`, `app/folha/export.py:439–461` |
| CMP-13 | 12/2026 · movimento de 18/12 | L-026/L-095 e aval L4 citado no teste. **240 min** em 18/12 pertencem ao TXT de **12/2026**, com **`400`** no gabarito. A saída em 21/12 pertence à competência seguinte. `app/folha/tests/test_inventario_competencia.py:35–39`, `:361–380` | **`400` ausente de `[]`**. A leitura de elegibilidade de dezembro passou. `app/folha/tests/test_inventario_competencia.py:127–141`, `:377–380` | **INSTRUMENTO**, leitura de normais sem campo. O colaborador passa pelo corte de dezembro porque 21/12 é depois de 20/12. `app/folha/export.py:54–60`, `:439–461`, `app/folha/tests/test_inventario_competencia.py:52–56`, `:132–141` | **Outra nascente** neste subteste, a asserção em `app/folha/tests/test_inventario_competencia.py:132–141` e a lista de eventos em `app/folha/export.py:439–461`. A ETIQUETA `app/folha/export.py:875` responde à elegibilidade. | Conferir **240 min** no fechamento lido de dezembro e a entrada no conjunto de elegíveis. Registrar a leitura de normais no TXT como sem campo, preservando o gabarito. A fixture homologa apenas feriado e noturno. `app/folha/tests/test_inventario_competencia.py:52–56`, `:124–141`, `:377–380`, `app/ponto/services/fechamento.py:922–923` |
| CMP-13 | 01/2027 · elegibilidade em 21/12 | A saída em **21/12** abre 01/2027 e inclui nela **240 min** do último dia, além dos 240 min da competência anterior. `app/folha/tests/test_inventario_competencia.py:35–39`, `:361–380` | O ID **16** ficou fora de **`set()`**. A conferência de retenção aceitou `[]`. `app/folha/tests/test_inventario_competencia.py:121–128`, `:377–380` | **ESTRUTURA**. 21/12 está dentro da janela de janeiro e cai no ramo de rescisão. `app/folha/export.py:54–60`, `:875–880` | **Mesma ETIQUETA**, `app/folha/export.py:875`. O ensaio local reproduziu a exclusão de janeiro e a passagem por esse crivo em dezembro. `app/folha/tests/_inventario_c3_mutantes.py:146–156` | O recorte 20/21 e a exclusão de 21/12 são decisões distintas no caminho. `app/folha/tests/test_inventario_competencia.py:374–380`, `app/folha/export.py:822–824`, `:875–880` |
| CMP-13 | 01/2027 · movimento de 21/12 | Os **240 min** de 21/12 devem constar em 01/2027, com **`400`** no gabarito. `app/folha/tests/test_inventario_competencia.py:361–380` | **`400` ausente de `[]`** entre os registros filtrados. `app/folha/tests/test_inventario_competencia.py:130–141`, `:380` | **ESTRUTURA** na exclusão. Há também a limitação de **INSTRUMENTO** da leitura de normais. `app/folha/export.py:723–746`, `:875–880`, `app/folha/tests/test_inventario_competencia.py:132–141` | **Mesma ETIQUETA** para a exclusão em `app/folha/export.py:875`, propagada pelo montador em `:734–736`. Normais sem evento em `:439–461`. | Conferir os **240 min** no fechamento lido de janeiro e manter a elegibilidade em asserção própria. A leitura de normais no TXT permanece sem campo. `app/folha/tests/test_inventario_competencia.py:124–141`, `:377–380`, `app/ponto/services/fechamento.py:922–923`, `app/folha/export.py:439–461` |
| CMP-14 | DSR posterior ao desligamento | L-026/L-095 e aval L4 citado no teste. Após **10/12**, os dias ficam fora do vínculo. De **11 a 13/12**, o gabarito é **zero semanas de DSR** e **zero reflexo**. `app/folha/tests/test_inventario_competencia.py:382–405` | Os dias posteriores passaram como `fora_vinculo` com zero previsto e trabalhado. `semanas_dsr_ok` deu **`1`**. O reflexo [nao verificado]. `app/folha/tests/test_inventario_competencia.py:395–405` | **ESTRUTURA**. No recorte todo posterior ao vínculo, o motor recebe o conjunto vazio de previstos. O laço concede DSR porque a lista de faltas também fica vazia. `app/escala/alimentacao.py:388–399`, `app/ponto/motor_calculo_v2.py:2706–2718`, `:2742–2743` | **Outra nascente**. `autoridade_do_periodo` monta o motor sem escala, que usa `Motor12x36` e herda o laço de `MotorBase`. A ETIQUETA de TXT aponta para outro crivo. `app/ponto/services/espelho.py:1147–1151`, `:1214–1216`, `app/ponto/motor_calculo_v2.py:2843`, `:3757–3762`, `app/folha/tests/test_inventario_competencia.py:384` | O vermelho é o **contador** de DSR no recorte posterior. A asserção de reflexo vem depois. `app/folha/tests/test_inventario_competencia.py:402–405` |
| CMP-14 | 12/2026 · elegibilidade | Os **1.920 min** trabalhados até **10/12**, inclusive, devem entrar no gabarito do TXT de dezembro. `app/folha/tests/test_inventario_competencia.py:382–406` | O ID **17** ficou fora de **`set()`**. A conferência de retenção aceitou `[]`. `app/folha/tests/test_inventario_competencia.py:121–128`, `:406` | **ESTRUTURA**. Demissão em 10/12 é anterior ao fim da janela em 20/12 e cai no ramo de rescisão. `app/folha/tests/test_inventario_competencia.py:37`, `app/folha/export.py:54–60`, `:875–880` | **Mesma ETIQUETA**, `app/folha/export.py:875`. O ensaio local reproduziu essa exclusão. `app/folha/tests/_inventario_c3_mutantes.py:146–156` | A exclusão do TXT e o `1` no contador de DSR têm nascentes distintas. `app/folha/export.py:875–880`, `app/ponto/motor_calculo_v2.py:2708–2718`, `:2742–2743` |
| CMP-14 | 12/2026 · movimento | Os **1.920 min** até 10/12 devem resultar em **`3200`** no gabarito de movimento. `app/folha/tests/test_inventario_competencia.py:382–406` | **`3200` ausente de `[]`** entre os registros filtrados. `app/folha/tests/test_inventario_competencia.py:130–141`, `:406` | **ESTRUTURA** na exclusão. Há também a limitação de **INSTRUMENTO** da leitura de normais. `app/folha/export.py:723–746`, `:875–880`, `app/folha/tests/test_inventario_competencia.py:132–141` | **Mesma ETIQUETA** para a exclusão em `app/folha/export.py:875`, propagada em `:734–736`. Normais sem evento em `:439–461`. | Conferir os **1.920 min** no fechamento lido e a elegibilidade em asserções próprias, mantendo o gabarito de DSR e registrando o limite da leitura de normais no TXT. `app/folha/tests/test_inventario_competencia.py:124–141`, `:394–406`, `app/ponto/services/fechamento.py:922–923`, `app/folha/export.py:439–461` |

## B. Obras donas das nascentes

A numeração de `obras.txt` começa na primeira linha de obra do anexo. A atribuição considera o trecho fornecido de cada entrega.

| Caso | Nascente | Obra dona | Por que (trecho da linha da obra) |
|---|---|---|---|
| ESC-12 | **Pareamento**. Sem marcos de pausa no DNA da folga, o pareador usa os do template, absorve a saída das 12:00 como pausa e deixa aberto o par de 240 min. A ETIQUETA aponta para a lavra posterior. `app/ponto/management/commands/gerar_celulas.py:85–93`, `app/ponto/turnos.py:1224–1244`, `:1356–1376`, `:1166–1175`, `app/ponto/tests/test_inventario_escala.py:355–369` | **sem obra** | `FOLGA-TRAB-COMERCIAL-PREVISTO-0` descreve “hoje paga trab-480”, e `E1C-LIMITE-COMERCIAL` entrega “limite diario [...] = previsto da grade no lugar dos marcos”. Ambos tratam do cálculo com realizado. A linha de `PAR-CONFIRMADO-PELO-SUPERVISOR-EVIDENCIA-HJ026` descreve pares “separados pelo teto”, outro ramo do pareamento. `obras.txt:85`, `:81`, `:1` |
| AUS-03 | **Precedência na grade**. `_marcar_ausencias` troca folga por `ausencia` para o atestado, e a ata propaga o rótulo. Coincide com a ETIQUETA. `app/escala/utils.py:1248–1265`, `app/ponto/services/cartorio.py:757–759`, `app/escala/services/leitor_celula.py:397–422`, `app/ponto/tests/test_inventario_ausencia.py:67–79` | **sem obra** | `ESMERIL-SEM-DIAS-PREVISTOS` trata de “dias_previstos conta ausencia em celula de folga”, no contador do fechamento. `AUSENCIA-FALTA-SEM-JORNADA` trata de “falta em dia sem previsto” e consta como “FECHADA [...] sem codigo”. A falha aqui é o rótulo da folga com atestado. `obras.txt:73`, `:57`, `app/docs/LEIS.md:180` |
| AUS-05 | **Leitura do instrumento**. A asserção de `DiaPago.furo_apurado` mede uma das parcelas, e a regra pede o estado em aberto com entrada sem saída. `app/ponto/tests/test_inventario_ausencia.py:123–128`, `app/ponto/services/dia_decidido.py:281–319` | **`INVENTARIO-PELA-REGRA`** | “bateria pela regra em [...] ausencia/ferias” cobre a asserção do cenário. `PERGUNTA-VIVA-NAO-MORRE-NO-DIA-MUDADO` trata de chamados segurados “quando o dia ja mudou”. `obras.txt:86`, `:17` |
| CMP-02 | **Fixture**. Falta `Posto.municipio` para o feriado municipal alcançar o vínculo. `app/ponto/tests/test_inventario_escala.py:103–117`, `app/core/feriados.py:64–66`, `:124–132` | **`INVENTARIO-PELA-REGRA`** | “bateria pela regra em escala [...] folha/export” cobre o cadastro da massa. `FERIADO-UM-JUIZ` trata de “feriado em folga dobra pelo juiz do ciclo”, decisão posterior ao alcance. `obras.txt:86`, `:80` |
| CMP-03 | **Fixture**. O mesmo município ausente impede chegar à medição da compensação. `app/folha/tests/test_inventario_competencia.py:189–211`, `app/core/feriados.py:64–66`, `:124–132` | **`INVENTARIO-PELA-REGRA`** | “bateria pela regra em escala [...] folha/export” cobre a massa do cenário. A entrega visível de `FERIADO-UM-JUIZ` cobre a dobra pelo ciclo e a escolha de rubrica. Seu alcance sobre o cenário com compensação integral [nao verificado]. `obras.txt:86`, `:80` |
| CMP-04 | **Fixture**. Falta o município do posto que fornece a UF para o feriado estadual. `app/ponto/tests/test_inventario_escala.py:103–117`, `app/core/feriados.py:62–63`, `:124–132` | **`INVENTARIO-PELA-REGRA`** | “bateria pela regra em escala” cobre o cadastro de entrada. `INVENTARIO-GERADOR` “le etiquetas dos casos + ultima suite + DIFF DP” e publica a lista. `obras.txt:86–87` |
| CMP-12 | **Crivo de rescisão no TXT** em 01/2027 e **leitura de normais** no instrumento. `app/folha/export.py:54–60`, `:875–880`, `:439–461`, `app/folha/tests/test_inventario_competencia.py:132–141` | **sem obra** para o crivo. **`INVENTARIO-PELA-REGRA`** para a leitura do teste. | `DESLIGADO-PORTA-DO-CHAMADO` entrega “criar/abrir perguntam esta_desligado” e “S89 vira registro informativo”, no fluxo de chamados. A “bateria pela regra em [...] folha/export” cobre a leitura do teste. Entrega de evento de normais [nao verificado]. `obras.txt:82`, `:86` |
| CMP-13 | **Crivo de rescisão** em 01/2027 e **leitura de normais** em ambas as competências. Em 12/2026 o vermelho é só do movimento. `app/folha/export.py:54–60`, `:875–880`, `:439–461`, `app/folha/tests/test_inventario_competencia.py:127–141`, `:377–380` | **sem obra** para o crivo. **`INVENTARIO-PELA-REGRA`** para a leitura do teste. | “bateria pela regra em [...] folha/export” cobre a medição das duas competências. `DESLIGADO-PORTA-DO-CHAMADO` descreve criação, reabertura e fecho de chamados. Entrega para o crivo mensal ou para o evento de normais [nao verificado]. `obras.txt:86`, `:82` |
| CMP-14 | **DSR fora do vínculo**, **crivo de rescisão** em 12/2026 e **leitura de normais** no instrumento. `app/ponto/motor_calculo_v2.py:2708–2718`, `:2742–2743`, `app/folha/export.py:875–880`, `:439–461`, `app/folha/tests/test_inventario_competencia.py:132–141`, `:402–406` | **sem obra** para DSR e crivo. **`INVENTARIO-PELA-REGRA`** para a leitura do teste. | `DESLIGADO-PORTA-DO-CHAMADO` fecha o passivo de “chamados de [...] desligados”. `ESMERIL-SEM-BORDA` trata de “data_fim inclusiva” e janelas de instante. Nenhum trecho cobre contar DSR numa janela sem vínculo ou alterar a elegibilidade mensal. A “bateria pela regra em [...] folha/export” cobre a leitura do teste. `obras.txt:82`, `:72`, `:86` |
| FOL-14 | **Duas nascentes**. Ao zerar o atraso, a reemissão fica sem linhas e para antes de registrar novo vigente, na ETIQUETA. Em paralelo, o recálculo de exportada registra o `warning` e a consulta do teste fica sem nova Pauta DP. `app/folha/export.py:439–461`, `:739–755`, `app/folha/views.py:277–280`, `:302–307`, `app/ponto/services/fechamento.py:559–567`, `app/folha/tests/test_inventario_folha.py:358`, `:403–420` | **sem obra** para reemissão/vigência e aviso | `GUARDA-10-B` descreve a retirada da guarda de **10/2026** e o apply da 10. `INVENTARIO-PELA-REGRA` entrega “bateria pela regra [...] + DIFF TXT x fechamento DP”. Os trechos fornecidos não cobrem a emissão que fica sem eventos em **11/2026** nem a criação do aviso ao DP com os dois números. `obras.txt:46`, `:86`, `app/docs/LEIS.md:155–156` |

## 27/11 19:24 · sessao 9b527bad8734 · FERIADO-UM-JUIZ · construtor · origem=Code

Implementei a c10. A recusa do oráculo reverte o savepoint da sombra. No BUG-144, o mensal e o DiaPago motor preservaram **5,95 h**, com retorno **1**. A guarda exige procedência por dia, inclusive quando há saída antecipada e zero horas trabalhadas. [fechamento.py:1295](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/fechamento.py:1295) · [dia_pago.py:316](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/dia_pago.py:316) · [R1:340](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:340)

O vigia lê `recusas_oraculo` em `/app/logs/ponto.log` na configuração SaaS. O esperado é **0**. Cada recusa registra colaborador, motivo e total acumulado. [fechamento.py:1328](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/fechamento.py:1328) · [saas.py:160](/home/ronald/wt-codex-feriado-um-juiz-152427/app/config/settings/saas.py:160)

Os **9 testes** ficaram vermelhos contra `597fdc40d` e passaram com a cura. Os **7 comandos de mutação** reproduziram as falhas esperadas, os **41 estruturais** passaram e a contagem de `except` ficou igual. O relato traz os comandos e as asserções antiga e nova de R4. [r.md:17](/home/ronald/wt-codex-feriado-um-juiz-152427/app/r.md:17)

Também alinhei o teste ORM c5 da própria obra. Execução ORM, suíte integral e produção **[nao verificado]**. A certificação fica com Code e Opus. [teste ORM:462](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:462) [nao verificado]

## 27/11 19:02 · sessao d36c9265c821 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

# REPROVADO

1. **L-111 — a restauração do Partido deixou um segundo juiz sem chamador de produção.** Em `570e13f24`, o cálculo mensal do Partido chamava `_ganho_noturno_da_base_da_he`. No c2x, voltou a somar `legais - relógio` diretamente. A função antiga ficou na árvore e só o teste a chama. O motor ainda calcula e grava `ganho_noturno_da_base_da_he`, cujo valor perdeu o leitor de negócio. A L-111 exige tratar o sítio órfão no mesmo commit. `570e13f24:app/ponto/motor_calculo_v2.py:3383` · [motor_calculo_v2.py:293](/home/ronald/codex_ws/d63a32aec0ff01e4909cef1801f4490723736aca/app/ponto/motor_calculo_v2.py:293) · [motor_calculo_v2.py:1978](/home/ronald/codex_ws/d63a32aec0ff01e4909cef1801f4490723736aca/app/ponto/motor_calculo_v2.py:1978) · [motor_calculo_v2.py:3382](/home/ronald/codex_ws/d63a32aec0ff01e4909cef1801f4490723736aca/app/ponto/motor_calculo_v2.py:3382) · [test_ramo2b_c2_base_da_he.py:402](/home/ronald/codex_ws/d63a32aec0ff01e4909cef1801f4490723736aca/app/ponto/tests/test_ramo2b_c2_base_da_he.py:402) · [LEIS.md:137](/home/ronald/codex_ws/d63a32aec0ff01e4909cef1801f4490723736aca/app/docs/LEIS.md:137)

## Chamadores e escopo

| Chamador ou referência | Contrato conferido | Sítio |
|---|---|---|
| `Motor12x36.calcular_periodo` | Pede e desempacota **5** | `app/ponto/motor_calculo_v2.py:2987` |
| `MotorComercial.calcular_periodo` | Pede e desempacota **5** | `app/ponto/motor_calculo_v2.py:3470` |
| `MotorTurnoPartido.calcular_periodo` e `MotorTurnoPartidoNoturno` por herança | Usam o padrão e desempacotam **4** | `app/ponto/motor_calculo_v2.py:3132`, `:3823` |
| `Motor12x36ComEscala` | Delega ao pai e usa a chamada de **5** | `app/ponto/motor_calculo_v2.py:3810` |
| `MotorPlantao` e `MotorIntermitente` | Pedem **5** pelo cálculo do 12x36, por delegação e por herança, respectivamente | `app/ponto/motor_calculo_v2.py:3798`, `:3832` |
| `MotorBase._janela_do_par` | Único chamador de `MotorBase._saida_efetiva`. Encaminha a opção e desempacota **3** | `app/ponto/motor_calculo_v2.py:1941` |
| Teste c2x | Compara a tupla inteira de **4** sem opção e de **5** com opção | `app/ponto/tests/test_ramo2b_c2x_janela.py:31`, `:41` |
| Mock e selo de teste | O mock retorna **5** para 12x36 e Comercial. O selo usa `getattr` para inspecionar a fonte | `app/ponto/tests/test_ramo2b_c2_base_da_he.py:493`, `:513`, `:528`, `:541` · `app/ponto/tests/test_janela_he_regra.py:154` |

**Corpos do Partido.** O AST completo das duas classes, sem posições, coincide com a cópia de `59600163c`. O SHA-256 do texto de cada classe também bate com o selo. `59600163c:app/ponto/motor_calculo_v2.py:2975`, `:3677` · `app/ponto/motor_calculo_v2.py:3105`, `:3823` · `app/ponto/tests/test_ramo2b_c2u_escopo.py:72`

**Flag.** `MotorBase` define `True`. Plantão e Intermitente sobrescrevem com `False`. A única leitura está em `_saida_efetiva`, que exige a opção, a flag e o juiz de cruzamento. Assim, Plantão e Intermitente pedem a opção por reúso e continuam parando no marco. `app/ponto/motor_calculo_v2.py:796`, `:1919`, `:3775`, `:3798`, `:3832`, `:3841` · `app/escala/servico_jornada.py:104`

## Catracas e contraprovas

- **`LINHAS_REGISTRADAS`** — as 11 entradas de registro remapeiam 26 ocorrências, contando as repetidas. Cada entrada manteve a identidade `(função, critério/sítio)` e apontou para o mesmo código. O censo deu **9 → 9** pareadores e **80 → 80** derivações de dia, com a mesma composição e classificação. Os 35 testes do módulo passaram em memória. `app/core/tests/test_medida_turno_do_par.py:253`, `:304`, `:351`, `:512`, `:550`, `:566`
- **RED contra `570e13f24`** — reconstruí o motor anterior em memória pelo diff e conferi o hash do blob `003eeac28`. Os três testes passaram no c2x. No anterior, a asserção sem opção falhou nos cinco motores porque voltavam cinco valores. Os três subcasos com opção deram `TypeError` pela assinatura antiga. O teste comportamental do Partido passou no anterior. `570e13f24:app/ponto/motor_calculo_v2.py:1923` · `app/ponto/tests/test_ramo2b_c2x_janela.py:28`, `:34`, `:44`
- **Mutante “opção ignorada”** — retirei a opção só da condição que libera a cauda, preservando a aridade. O teste do Partido falhou nas duas classes. O trio `(trabalhado, noturno de relógio, fora)` foi de `(720, 540, 30)` para `(750, 570, 0)`. Ao forçar o sentido inverso, com a cauda sempre desativada, o teste do 12x36 com opção falhou. `app/ponto/motor_calculo_v2.py:1919`, `:1942` · `app/ponto/tests/test_ramo2b_c2x_janela.py:34`, `:44`

## Demais classes

- **Segundo juiz e derivação paralela** — o sítio órfão do bloqueio 1 é o achado. A janela consulta o dia uma vez, delega o recorte à regra da L-097 e consulta a autoridade de cruzamento. `app/ponto/motor_calculo_v2.py:1909`, `:1918`, `:1939` · `app/ponto/janela_he.py:150` · `app/escala/servico_jornada.py:104`
- **Segundo escritor** — os campos da base da HE passam pela mesma porta. O selo de escritor único passou nos oito testes em memória. `app/ponto/motor_calculo_v2.py:267`, `:1950`, `:1968` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:661`
- **Aridade variável** — é um contrato de forma com um prefixo de significado fixo. `(e, s, fe, fs)` mantém as quatro posições, e `s_he` acrescenta o limite da base da HE. A opção também habilita a cauda sob a flag, efeito explícito no contrato. Nenhum chamador auditado interpreta a mesma posição com dois significados. `app/ponto/motor_calculo_v2.py:1919`, `:1924`, `:1941`
- **Relógio, ordem de trava e `except`** — zero leituras novas do relógio, zero mudanças de aquisição de trava e **0 `except` adicionados, 0 removidos**. A contagem AST foi motor **18 → 18**, teste da catraca **1 → 1**, teste novo **0**. `app/ponto/motor_calculo_v2.py:1874`, `:1924`, `:2987`, `:3105`, `:3470` · `app/core/tests/test_medida_turno_do_par.py:253` · `app/ponto/tests/test_ramo2b_c2x_janela.py:27`

As execuções usaram o código da cópia em memória, com substitutos mínimos para as dependências de teste do Django. Suíte Django inteira, efeitos em produção e integridade das competências exportadas **[nao verificado]**. [nao verificado]

_8777 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
