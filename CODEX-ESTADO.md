# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 01:25** · publicado em **28/11/2026 01:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 01:25 · sessao 5f422d0683f6 · RETRATO-PDF-CORTA-POR-QUE · construtor · origem=Code

Implementei os itens 1, 3, 4 e 5 em três arquivos de produção. O item 2 e os arquivos de teste ficaram fora do diff. `folha/views_retrato.py:350` · `relatorios/utils_audit.py:11` · `config/settings/base.py:5`

### Cura por item

- **1 — Download.** A rota responde `attachment; filename="retrato-folha-emp<pk>-<MM>-<AAAA>.pdf"`. O link continua no partial único, sem `target` nem atributos `hx-*`. `folha/views_retrato.py:473` · `templates/folha/_retrato_barra.html:50` · `templates/core/_btn_pdf.html:34`
- **3 — Escala.** A descrição inteira entra em `Paragraph(escape(...))` na coluna de 38 mm. `folha/views_retrato.py:424` · `folha/views_retrato.py:441` · `folha/views_retrato.py:450`
- **4 — Ordem e contadores.** Só o papel ordena por Sem ação → Sem leitura → Admin → Colab → Hoje → Entram → Rescisão, com mais dias primeiro e ID crescente no empate. Dentro de cada dono, coloquei `dias=None` depois das linhas com número. O topo imprime `Rótulo: N` com o `numero` de cada pílula de `Depende de` retornada por `quadros_barra_retrato`. A ordem da tela, o recorte, a contagem e o cabeçalho da tabela foram preservados. `folha/views_retrato.py:232` · `folha/views_retrato.py:324` · `folha/views_retrato.py:399` · `folha/views_retrato.py:418` · `folha/views_retrato.py:432` · `folha/views_retrato.py:467`
- **5 — Carimbo.** `COMMIT_GIT` preenchida tem precedência. Em seguida, o leitor único usa `ler_deploy_stamp` pelo caminho `settings.HASNER_COMMIT_STAMP`. Arquivo ausente retorna `''`. O padrão resolve para `/app/logs/deploy.stamp` no container. O escritor grava no `logs/` da raiz do host e o bind documentado monta `app/` em `/app`. A exposição do carimbo nesse caminho em produção está [nao verificado]. `relatorios/utils_audit.py:11` · `core/esteira_vigia.py:1032` · `config/settings/base.py:4` · `bin/deploy.sh:436` · `CLAUDE.md:88`

### Leitores

- `_recorte_da_barra` é chamado pela tela, pelo PDF e pelo Raio‑X. `folha/views_retrato.py:260` · `folha/views_retrato.py:391` · `folha/views_retrato.py:507`
- `get_commit_git` é chamado diretamente por `gerar_pdf_auditavel` e pelo espelho informacional. Pela porta auditável, a mudança alcança o retrato, o espelho individual, o lote de espelhos, o histórico de chamados e o PDF de gestão de HE. `relatorios/pdf_base.py:34` · `relatorios/pdf_base.py:135` · `relatorios/pdf_espelho.py:1070` · `folha/views_retrato.py:461` · `relatorios/pdf_espelho.py:1106` · `relatorios/services.py:338` · `relatorios/pdf_chamados.py:168` · `ponto/views.py:887`

### REDs e mutantes para o opus

| RED | Caso e mutantes que deve pegar |
|---|---|
| **1. Download** | Conferir o `attachment` e o nome exato pela rota, além do link no HTML cheio e no parcial filtrado. Mutantes `inline`, nome alterado ou `target`/`hx-*` no link. `folha/tests/test_retrato_pdf_pacote.py:187` |
| **3. Escala** | Usar descrição longa com `<` e `&`, conferir o texto integral no `Paragraph` e a quebra dentro da coluna. Mutantes de string crua, truncamento ou retirada de `escape`. `folha/tests/test_retrato_pdf_pacote.py:210` · `folha/views_retrato.py:441` |
| **4a. Ordem** | Intercalar os sete estados, incluir dias distintos na mesma faixa e um empate com nomes em ordem inversa à dos IDs. Conferir a ordem da tela e a igualdade do conjunto de IDs. Mutantes de ordem compartilhada, dias crescentes, ordenação por faixa ou desempate por nome. `folha/tests/test_retrato_pdf_pacote.py:228` · `folha/tests/test_retrato_pdf_pacote.py:245` |
| **4b. Contadores** | Comparar todos os `Rótulo: N` com a tela, usando filtro universal, janela e Admin ativo. Mutantes que contam a partir das linhas visíveis, usam o universo inteiro ou imprimem só a pílula ativa. `folha/tests/test_retrato_pdf_pacote.py:251` · `folha/views_retrato.py:411` |
| **5a. Fonte do SHA** | Ler um carimbo válido sem `COMMIT_GIT`, repetir com a variável preenchida e com o arquivo ausente. Mutantes que ignoram o caminho configurado, preferem o carimbo ao ambiente ou voltam a consultar Git. `folha/tests/test_retrato_pdf_pacote.py:292` · `folha/tests/test_retrato_pdf_pacote.py:317` · `relatorios/utils_audit.py:11` |
| **5b. Trilha** | Conferir o SHA inteiro no registro e nas duas chamadas do rodapé, e os oito primeiros caracteres no texto impresso. Mutantes que gravam o SHA só no registro, omitem uma passagem ou truncam o registro. `folha/tests/test_retrato_pdf_pacote.py:300` · `relatorios/pdf_base.py:72` · `relatorios/pdf_base.py:139` · `relatorios/pdf_base.py:164` · `relatorios/pdf_base.py:170` |

**Diff** com **0 `except` adicionados e 1 removido**. A sintaxe dos três arquivos e `git diff --check` foram conferidos. Resultado da suíte e renderização real do PDF [nao verificado]. `relatorios/utils_audit.py:11` · `folha/views_retrato.py:350` · `config/settings/base.py:5`

```bash
bash bin/suite.sh --dir /home/ronald/wt-codex-retrato-pdf-corta-por-que-012113 --only "folha.tests.test_retrato_pdf_pacote folha.tests.test_retrato_pdf relatorios core.tests.test_selo_teste_sem_relogio"
```

## 28/11 01:24 · sessao d2ce5ce5364d · INVENTARIO-PELA-REGRA · leitura · origem=Code

```python
import sys

sys.dont_write_bytecode = True

import json
import logging
from collections import defaultdict
from datetime import date
from functools import lru_cache
from html.parser import HTMLParser
from types import SimpleNamespace
from typing import NamedTuple


def parar(fonte, motivo):
    print(f"BECO|ABORTOU|{fonte}|{motivo}", flush=True)
    raise SystemExit(1) from None


def fonte_segura(caminho, linha):
    _, separador, relativo = caminho.rpartition("/app/")
    return f"{'app/' + relativo if separador else caminho.rsplit('/', 1)[-1]}:{linha}"


class FalhasSemDados:
    def __enter__(self):
        self.anterior = logging.getLogRecordFactory()

        def fabrica(*args, **kwargs):
            registro = self.anterior(*args, **kwargs)
            if registro.levelno >= logging.ERROR or (
                registro.exc_info and registro.exc_info[0]
            ):
                parar(fonte_segura(registro.pathname, registro.lineno), "erro_do_leitor")
            registro.msg, registro.args = "", ()
            registro.levelno = logging.NOTSET - 1
            return registro

        logging.setLogRecordFactory(fabrica)

    def __exit__(self, tipo, erro, quadro):
        logging.setLogRecordFactory(self.anterior)
        if tipo is None or (tipo is SystemExit and erro.code == 1):
            return False
        while quadro.tb_next is not None:
            quadro = quadro.tb_next
        parar(fonte_segura(quadro.tb_frame.f_code.co_filename, quadro.tb_lineno), tipo.__name__)


with FalhasSemDados():
    from django.conf import settings
    from django.db import connection
    from django.utils import timezone
    from django_tenants.utils import schema_context

    if getattr(settings, "HASNER_SOMBRA", False) is not True:
        parar("bin/sonda_leitura.sh:94", "HASNER_SOMBRA_ausente")
    db = connection.settings_dict
    if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
        parar("bin/sonda_leitura.sh:94", "conexao_fora_da_sombra")
    if connection.in_atomic_block or not connection.get_autocommit():
        parar("app/chamados/models.py:761", "sonda_exige_transacao_propria")

    from django.contrib.auth.models import User
    from django.db.models import Q
    from django.template import Context, engines

    from chamados.catalogo.motor import TERMINAIS, VIVOS, decidir
    from chamados.juizes import (
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
        _veredito_premissa,
        eventos_pendentes,
        retencao_da_retratacao,
    )
    from chamados.services.acoes_chamado import montar_contexto_detalhe
    from chamados.services.fio import (
        chamado_que_lista,
        contexto_parcial_fio,
        data_do_fato_de,
        disputas_do_chamado_em_lote,
    )
    from chamados.services.materializacao import (
        _data_em_disputa,
        estado_aplicacao_pergunta,
        espera_declarado_x_gravado,
    )
    from chamados.services.tarefa_do_dia import (
        carga_da_tarefa,
        exigencia_das_perguntas,
        porta_da_tarefa,
    )
    from chamados.services.validacao import (
        _e_aceite_de_folga,
        _resposta_de_hora,
        entrega_que_barra_a_validacao,
        fila_validar_classificada,
    )
    from chamados.services.verbos import particionar_verbos
    from chamados.utils import pode_agir_chamado
    from chamados.views import _is_gestor
    from chamados.views_cobrar import _pode_validar_lote
    from colaboradores.mixins import tem_acao
    from colaboradores.models import Empresa
    from folha.fora_da_folha import dias_que_seguram
    from folha.models import RetratoFolha
    from folha.retrato_folha import TIPO_SEM_CAUSA, TIPOS, dono_da_linha
    from ponto.janelas import janela_fechamento
    from ponto.services.declarar_dia import checar_declaracao, o_dia_ja_e_folga
    from ponto.supra_juiz import perguntas_que_seguram_csf


MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
GESTOR_S1_PK = 909
NV = "[nao verificado]"
FAMILIAS = ("a", "b", "c", "d", "e", "e_retido_pelo_fechamento",
            "e_retido_na_retratacao", "f")
OBRAS = {
    "a": ("O261-FIO-MORTO-PELO-RETRATO", 1132),
    "b": ("ORFAS-VOLTOU", 2102),
    "c": ("FDS-S1-C2-SEM-ACAO", 2197),
    "d": ("O261-E-CELULA-20-UM-DONO", 1012),
    "e": ("FDS-S1-C3-PREMISSA-MORTA", 2198),
    "f": ("FDS-S1-C1-SEM-SAIDA", 2196),
}
ACOES = {"validar_pergunta", "recusar_pergunta", "corrigir_resposta", "conferir_data"}


class Linha(NamedTuple):
    colab: int
    dia: date | None
    referencia: str
    motivo: str
    fonte: str
    obra: tuple


def ref(pk):
    return str(pk) if pk is not None else "-"


def dia_pergunta(p):
    # O fallback pela celula e a costura da S1. app/chamados/juizes.py:58,339
    dia = _dia_de_turno_da_pergunta(p)
    if dia is None and not p.conversa and p.celula_id:
        return p.celula.data, "celula"
    return dia, "dia_do_turno"


def consulta_perguntas(emp):
    return PerguntaDisputa.objects.filter(disputa__colaborador__empresa_id=emp).select_related(
        "disputa__colaborador__empresa", "disputa__chamado",
        "chamado_destino__colaborador", "chamado_destino__celula", "celula",
    ).prefetch_related("decisoes")


class Cartoes(HTMLParser):
    def __init__(self, html, chamado_local=None):
        super().__init__()
        self.perguntas = defaultdict(set)
        self.pares = defaultdict(set)
        self.formularios = defaultdict(list)
        self.chamado_local = chamado_local
        self.formulario = None
        self.feed(html)
        self.close()

    def handle_starttag(self, tag, atributos):
        a = dict(atributos)
        if "data-beco-pergunta" in a:
            self.perguntas[int(a["data-beco-pergunta"])].add(int(a["data-beco-chamado"]))
        if "data-beco-par" in a:
            self.pares[int(a["data-beco-par"])].add(int(a["data-beco-chamado"]))
        if tag == "form":
            self.formulario = {}
            if "data-beco-conferir" in a:
                self.formulario.update(
                    acao="conferir_data",
                    pergunta_id=a["data-beco-conferir"],
                    chamado_id=a["data-beco-chamado"],
                )
        elif tag == "input" and self.formulario is not None and a.get("type") == "hidden":
            nome = a.get("name")
            if nome in {"acao", "pergunta_id", "chamado_id", "confirmar_implausivel"}:
                self.formulario[nome] = a.get("value", "")

    def handle_endtag(self, tag):
        if tag != "form" or self.formulario is None:
            return
        f, self.formulario = self.formulario, None
        if f.get("acao") in ACOES:
            cid = int(f.get("chamado_id", self.chamado_local))
            self.formularios[int(f["pergunta_id"])].append(
                (cid, f["acao"], f.get("confirmar_implausivel") == "1")
            )


@lru_cache(maxsize=2)
def template_marcado(modal):
    # Marcadores em memoria nas ramificacoes reais. O HTML fica em memoria.
    # app/templates/chamados/partials/modal_fio.html:160,173,219,470
    # app/templates/chamados/partials/_pergunta_card.html:28,31
    engine = engines["django"].engine
    nome = "modal_fio.html" if modal else "_pergunta_card.html"
    caminho = f"chamados/partials/{nome}"
    fonte = engine.get_template(caminho).source
    pk = "perg.pk"
    card = "chamado.pk"

    def marca(tipo, pergunta=pk):
        return f'<span data-beco-{tipo}="{{{{ {pergunta} }}}}" data-beco-chamado="{{{{ {card} }}}}"></span>'

    resposta = (
        "<strong>Resposta:</strong> {{ perg.resposta_colab }}" if modal
        else "<strong>Resposta do colab:</strong> {{ perg.resposta_display }}"
    )
    pergunta = "{{ perg.pergunta }}</div>"
    trocas = [(pergunta, marca("pergunta") + pergunta), (resposta, marca("par") + resposta)]
    if modal:
        gaveta = "{{ it.dia }}</span>"
        conferir = """<form hx-post="{% url 'chamados:conferir_data_pergunta' perg.pk %}" """
        trocas.extend((
            (gaveta, marca("pergunta", "it.pergunta_id") + gaveta),
            (conferir, conferir + 'data-beco-conferir="{{ perg.pk }}" data-beco-chamado="{{ chamado.pk }}" '),
        ))
    for original, marcado in trocas:
        if fonte.count(original) != 1:
            parar(f"app/templates/{caminho}:1", "marcador_do_template_mudou")
        fonte = fonte.replace(original, marcado, 1)
    return engine.from_string(fonte)


@lru_cache(maxsize=None)
def chamado_de(pk):
    return ChamadoColaborador.objects.select_related(
        "colaborador__empresa", "celula"
    ).get(pk=pk)


def validar_em_leitura(p, confirma):
    # RECONSTRUIDO: testemunhas da S1, sem executar o ato sob trava.
    # app/chamados/services/validacao.py:218,755,779,782
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
        aplicacao = estado_aplicacao_pergunta(p.disputa, p)
        if aplicacao is None:
            parar("app/chamados/services/materializacao.py:880", "sem_testemunha_da_resposta_de_hora")
        return aplicacao["estado"] in ("aplicavel", "ja_satisfeita", "aplicada") [nao verificado]
    return not (p.tipo_resposta == "data_hora" and _data_em_disputa(p))


class TelaGestor:
    def __init__(self, conta, verbos, lote):
        self.conta = conta
        self.req = SimpleNamespace(user=conta, GET={})
        self.verbos = verbos
        self.lote = lote
        self.pode_inline = tem_acao(conta, "gerir_chamados")

    @lru_cache(maxsize=None)
    def modal(self, colab):
        # O GET marca lidas. O construtor do parcial le o mesmo fio.
        # app/chamados/views.py:870,881,890; app/chamados/services/fio.py:573,610
        ctx = contexto_parcial_fio(self.req, colab)
        ctx["cor_disputas_map"] = DisputaSupervisao.cor_map_por_colaborador(colab)
        for ch in ctx["chamados"]:
            if ch.disputa_obj is not None and ch.explicacao is None:
                parar("app/chamados/services/fio.py:437", "explicador_sem_resposta")
            for p in ch.perguntas_disputa:
                estado = p.estado_aplicacao or {}
                if estado.get("detalhe") == "estado de aplicacao indisponivel -- sem validar":
                    parar("app/chamados/services/fio.py:470", "erro_na_testemunha_da_aplicacao")
        return Cartoes(template_marcado(True).render(Context(ctx, use_l10n=False)))

    @lru_cache(maxsize=None)
    def local(self, pk):
        ch = chamado_de(pk)
        # Colisao GLPI -> detalhe local, igual a casca. app/chamados/views.py:219
        if ch.ticket_id_glpi is not None:
            alvo = ChamadoColaborador.objects.filter(
                pk=ch.ticket_id_glpi, ticket_id_glpi__isnull=True
            ).values_list("pk", flat=True).first()
            return self.local(alvo) if alvo is not None else Cartoes("")
        ctx = montar_contexto_detalhe(self.req, ch, True, ())
        html = "".join(
            template_marcado(False).render(Context({**ctx, "perg": p}, use_l10n=False))
            for p in ctx["perguntas_disputa"]
        )
        return Cartoes(html, chamado_local=pk)

    @lru_cache(maxsize=None)
    def eleita(self, pk):
        return disputas_do_chamado_em_lote([pk]).get(pk)

    @lru_cache(maxsize=None)
    def inline(self, pk):
        ch = chamado_de(pk)
        if not (self.pode_inline and self.verbos.get(pk) == "validar"
                and pode_agir_chamado(self.conta, ch)):
            return set()
        d = self.eleita(pk)
        if d is None or d.colaborador_id != ch.colaborador_id:
            return set()
        return {p.pk for p in perguntas_vivas_de(d) if p.respondida}

    def formularios(self, p, cartoes, canal):
        ancora = chamado_que_lista(p)
        for cid, acao, confirma in cartoes.formularios.get(p.pk, ()):
            if acao == "conferir_data":
                # O POST e por pergunta, com a permissao de tela. app/chamados/views.py:1288
                if self.pode_inline and data_do_fato_de(p)[1] and p.data_conferida_em is None:
                    yield acao
                continue
            if cid != ancora or not pode_agir_chamado(self.conta, chamado_de(cid)):
                continue
            if canal == "local" and not p.disputa.aberta:
                continue
            if acao == "validar_pergunta":
                if canal == "fio" and chamado_de(cid).colaborador_id != p.disputa.colaborador_id:
                    continue
                if validar_em_leitura(p, confirma):
                    yield acao
                continue
            # app/chamados/services/fio.py:694,709,776
            if not p.disputa.aberta or (canal == "fio" and self.eleita(cid) is None):
                continue
            if DecisaoPergunta.composta(p).validada_em is not None:
                continue
            if acao == "recusar_pergunta" and p.respondida and not p.fora_do_fato():
                yield acao
            if acao == "corrigir_resposta" and p.tipo_resposta == "hora":
                yield acao

    def consultar(self, p, ch=None, *, medir_acoes=False):
        ids = {chamado_que_lista(p), p.disputa.chamado_id, ch.pk if ch else None} - {None}
        colabs = {p.disputa.colaborador} | {chamado_de(pk).colaborador for pk in ids}
        canais = [("fio", self.modal(colab)) for colab in sorted(colabs, key=lambda c: c.pk)]
        canais += [("local", self.local(pk)) for pk in sorted(ids)]
        no_lote = p.pk in self.lote  # app/chamados/views_cobrar.py:371
        visivel = no_lote or any(p.pk in c.perguntas for _, c in canais)
        par = no_lote or any(p.pk in c.pares for _, c in canais)
        acoes = set()
        if medir_acoes and par:
            acoes = {acao for canal, c in canais for acao in self.formularios(p, c, canal)}
            if (no_lote or any(p.pk in self.inline(pk) for pk in ids)) and validar_em_leitura(p, False):
                acoes.add("validar_pergunta")
        return visivel, par, acoes


def medir_a(empresa, janela, agora, hoje, vivos, perguntas_dia, tela):
    universo = dias_que_seguram(empresa, MES, ANO, hoje=hoje)
    ancoras = defaultdict(set)
    for ch in vivos.values():
        dia = dia_do_fato(ch, fortes=True)
        if dia is not None:
            ancoras[ch.colaborador_id, dia].add(ch.pk)
    for chave, perguntas in perguntas_dia.items():
        for p, _ in perguntas:
            pk = chamado_que_lista(p)
            if pk in vivos and vivos[pk].colaborador_id == chave[0]:
                ancoras[chave].add(pk)

    candidatos = {}
    for cid, (_, dias) in universo["dias"].items():
        for r in dias:
            chave = (cid, r["data"])
            if chave not in ancoras:
                candidatos[chave] = r
    if not candidatos:
        return []

    colabs = universo["colabs"]
    carga = carga_da_tarefa(
        [colabs[cid] for cid in sorted({cid for cid, _ in candidatos})],
        *janela, agora,
    )
    linhas = []
    for (cid, dia), r in sorted(candidatos.items()):
        tarefa = carga.tarefa(colabs[cid], dia)
        porta = porta_da_tarefa(tarefa)
        fila = (porta or {}).get("fila") or ()
        if any(pk in vivos and vivos[pk].colaborador_id == cid for pk in fila):
            continue
        ps = [p for p, viva in perguntas_dia.get((cid, dia), ()) if viva]
        if any(tela.consultar(p)[0] for p in ps):
            continue
        linhas.append(Linha(
            cid, dia, "-",
            f"{r['motivo']} celula={ref(r['celula_id'])} "
            f"rota_tarefa={(porta or {}).get('rota') or '-'} "
            f"fio_tarefa={(porta or {}).get('fio') or '-'} nao_sei={int(tarefa['nao_sei'])} "
            "RECONSTRUIDO: juncao_colab_dia_ancora_e_superficies_admin",
            "app/folha/export.py:1240", OBRAS["a"],
        ))
    return linhas


def medir_c(emp, janela, agora, hoje, vivos, tela, sem_dia):
    pares = defaultdict(set)
    for ch in vivos.values():
        motor = decidir(ch.status_local, "RECONCILIAR")
        if motor.permitido and motor.novo_estado in TERMINAIS:
            plano = ch._plano_pergunta_viva(motor.novo_estado)
            if not ch._gate_pergunta_viva(motor.novo_estado, registrar=False, plano=plano):
                for p in plano[0]:
                    pares[ch.pk, p.pk].add("pergunta_viva_na_competencia_aberta")
        if not ch._gate_conversa_do_pai(registrar=False):
            for pk in perguntas_que_seguram_csf(ch):
                pares[ch.pk, pk].add("conversa_exige_decisao_humana")

    # A pergunta e relida no estado corrente, pois o plano julgou o pos-estado.
    ps = consulta_perguntas(emp).in_bulk({pk for _, pk in pares})
    encontrados = []
    for (ch_pk, p_pk), guardas in sorted(pares.items()):
        p = ps.get(p_pk)
        if p is None or not p.respondida or not pergunta_viva(p, hoje=hoje, agora=agora):
            continue
        ch = vivos[ch_pk]
        dia, fonte_dia = dia_pergunta(p)
        if dia is None:
            comp = competencia_do_chamado(ch)
            dia = comp.dia if comp is not None else None
            fonte_dia = "competencia_do_chamado"
        if dia is not None and not janela[0] <= dia <= janela[1]:
            continue
        _, par, acoes = tela.consultar(p, ch, medir_acoes=True)
        if not par or acoes:
            continue
        if dia is None:
            sem_dia["c"] += 1
            continue
        encontrados.append((ch, p, dia, fonte_dia, guardas))

    razoes = exigencia_das_perguntas({p.pk for _, p, _, _, _ in encontrados}, agora)
    linhas = []
    for ch, p, dia, fonte_dia, guardas in encontrados:
        razao, _ = razoes[p.pk]
        nascente = (
            "app/chamados/models.py:429" if "pergunta_viva_na_competencia_aberta" in guardas
            else "app/ponto/supra_juiz.py:561"
        )
        linhas.append(Linha(
            p.disputa.colaborador_id, dia, str(ch.pk),
            f"pergunta={p.pk} disputa={p.disputa_id} ancora={ref(chamado_que_lista(p))} "
            f"guardas={','.join(sorted(guardas))} tarefa={razao.partition(':')[0]} "
            f"RECONSTRUIDO: alcance_do_par_gestor_S1_sem_POST dia_por={fonte_dia}",
            nascente, OBRAS["c"],
        ))
    return linhas


def medir_e(janela, vivos, com_evento, grupos, sem_dia):
    # Ordem de producao. app/chamados/management/commands/reconciliar_chamados.py:62,73,79
    for ch in vivos.values():
        if ch.colaborador_id in com_evento:
            continue
        comp = competencia_do_chamado(ch)
        dia = comp.dia if comp is not None else None
        if dia is not None and not janela[0] <= dia <= janela[1]:
            continue
        veredito = _veredito_premissa(ch)
        if veredito is None or veredito.viva:
            continue
        ret = retencao_da_retratacao(ch)
        if ret is not None:
            fam, det = "e_retido_na_retratacao", f"retencao={ret[0]}"
        elif not ch.reconciliaria():
            fam, det = "e_retido_pelo_fechamento", "reconciliaria=False"
        else:
            fam, det = "e", "reconciliaria=True"
        if dia is None:
            sem_dia[fam] += 1
            continue
        grupos[fam].append(Linha(
            ch.colaborador_id, dia, str(ch.pk), f"{ver

[truncado em 20000 de 32189 caracteres]

## 28/11 01:19 · sessao 49d483221237 · CODIGO-DOMINIO-VALIDO · leitura · origem=Code

# Dossiê — CODIGO-DOMINIO-VALIDO

**O ponto de mudança é `classificar_export`, na resposta de quem entra no TXT.** Hoje o juiz só verifica se o código do colaborador está vazio depois de `strip()`. Um valor preenchido como `VIGIA` pode passar por essa etapa e, se houver rubrica a emitir, fazer a validação recusar o TXT inteiro. O Retrato já grava o código na linha e já atribui `Admin` aos bloqueios de cadastro. [app/folha/export.py:849](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/export.py:849) [app/folha/export.py:881](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/export.py:881) [app/folha/views.py:282](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/views.py:282) [app/folha/retrato_folha.py:1239](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/retrato_folha.py:1239) [app/folha/retrato_folha.py:374](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/retrato_folha.py:374)

Leitura da cópia e prova em memória do reg10, sem alteração de arquivos. Hash do conteúdo `[nao verificado]`, pois a cópia veio sem metadados Git. O anexo entregue contém a tabela `JUIZES.md`, cujo verbete de fechamento consta na cópia. Ocorrências de `VIGIA` em produção, quantidade de pessoas afetadas, empresa, competência e frequência de uso real `[nao verificado]`. `app/docs/JUIZES.md:119`, `CLAUDE.md:35`.

## A. Onde o juiz decide e onde o layout acusa

- **Precedência atual:** `rescisao_modulo_proprio` → `sem_codigo_dominio` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → `entra`. A rescisão vence quando a data de demissão existe e é até o fim da janela. O ramo seguinte usa `if not item['codigo_dominio']`, com o item já preenchido por `(colab.codigo_dominio or '').strip()`. Cada retenção encerra a classificação daquela pessoa. `app/folha/export.py:54–60`, `app/folha/export.py:774–782`, `app/folha/export.py:846–893`, `app/folha/export.py:968–990`.
- **Matrícula do reg10:** `montar_linhas_export` pega os IDs com `status == 'entra'` e lê `(fech.colaborador.codigo_dominio or '').strip().zfill(10)`. O reg10 põe a matrícula em `[2:12]`. `validar_layout_dominio` exige 43 caracteres na linha e aplica `.isdigit()` a esse campo. `VIGIA` vira `00000VIGIA` e dá `reg10 campo matric nao-numerico`. A view devolve `Falha de layout no export` antes de entregar ou registrar o TXT. O command de regeração também recusa erro de layout. `app/folha/export.py:34–45`, `app/folha/export.py:272–279`, `app/folha/export.py:723–753`, `app/folha/views.py:275–304`, `app/folha/management/commands/regerar_txt_dominio.py:94–96`.
- **Predicados existentes:** a checagem numérica da matrícula está no validador de layout e também em `partes_da_linha`, que lê um reg10 já montado. `codigo_no_sistema_dominio` procura a integração ativa **da empresa**, acusa ausência de configuração e preenche com zeros o código da integração. `nao_homologado` consulta códigos **de rubrica**. `app/folha/export.py:23–33`, `app/folha/export.py:245–254`, `app/folha/export.py:277–279`, `app/folha/export.py:983–987`, `app/folha/export.py:1003`.
- **Cadastro hoje:** admissão e edição tratam o campo como texto, aplicam `strip()` e atribuem o valor se preenchido. A validação de obrigatórios só trata nome e CPF, e a de pendências verifica presença. O campo é `CharField(max_length=20, blank=True, default='')`, sem validador numérico, e o `save()` do colaborador não valida esse campo. O `clean()` em `models.py:515` pertence a `HistoricoSalario`. Na admissão o código fica fora dos dados enviados à guarda de tamanho. Na edição o POST usa `codigo_folha` e a guarda compara nomes exatos do modelo. `app/colaboradores/views.py:1131–1138`, `app/colaboradores/views.py:1169–1185`, `app/colaboradores/views.py:1231–1237`, `app/colaboradores/views.py:1297–1306`, `app/colaboradores/views.py:1353–1365`, `app/colaboradores/services/cadastro.py:22–30`, `app/colaboradores/services/cadastro.py:49–87`, `app/core/validacao.py:46–59`, `app/colaboradores/models.py:311–314`, `app/colaboradores/models.py:415–439`, `app/colaboradores/models.py:478`, `app/colaboradores/models.py:515`.

## B. Censo do campo `Colaborador.codigo_dominio`

| Sítio | Leitura ou escrita encontrada |
|---|---|
| **Admissão** | A rota `novo/` recebe `codigo_folha` no input de texto. A view cria o colaborador e depois atribui o código preenchido por `salvar_campos(..., 'cadastro', ...)`. `app/colaboradores/urls.py:49`, `app/templates/colaboradores/admitir.html:58`, `app/colaboradores/views.py:1131`, `app/colaboradores/views.py:1204–1237`. |
| **Edição** | A rota `editar/` exibe o código atual no input `codigo_folha`. A view atribui o valor preenchido e chama `salvar_inteiro(..., 'cadastro')`. POST vazio mantém o código anterior. Os aliases usados para reexibir a admissão e a edição preenchem rascunhos em memória. `app/colaboradores/urls.py:50`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1113–1118`, `app/colaboradores/views.py:1267–1285`, `app/colaboradores/views.py:1353–1365`. |
| **Persistência genérica** | `cadastro` inclui o campo, e `gravar`, `salvar_campos`, `atualizar_em_lote` e `criar_colaborador` podem transportá-lo via argumentos. `salvar_inteiro` e `salvar_reativacao` fazem `save()` completo e podem regravar o valor que já estava carregado. A migration declara o campo e o modelo fornece o vazio inicial. `app/colaboradores/services/colaborador.py:46`, `app/colaboradores/services/colaborador.py:126–173`, `app/colaboradores/services/colaborador.py:184–206`, `app/colaboradores/migrations/0026_colaborador_codigo_dominio.py:15`, `app/colaboradores/models.py:311–314`. |
| **Admin, importadores, commands, API e signals** | Admin Django lê o campo por `SoLeitura`. O importador Nexti encontrado grava telefone e e-mail. O signal de `Colaborador` localizado trata isenção de ponto. A API de cadastro incompleto consulta o campo e o upload de holerite usa o código para casar páginas. Outros escritores explícitos do campo nessas categorias não apareceram na busca na cópia. Escritas por fora da cópia `[nao verificado]`. `app/colaboradores/admin.py:47–48`, `app/core/admin.py:53–75`, `app/colaboradores/management/commands/importar_telefones_nexti.py:113`, `app/chamados/signals.py:270–281`, `app/api/views_mensageria.py:1275–1288`, `app/holerite/motor.py:216–230`. |
| **Export, prévias e auditoria do TXT** | O juiz lê o código para classificar, e o montador lê para preencher a matrícula. A prévia nominal, a proposta em tela/CSV e o PDF de retidos leem o campo do item. O PDF de validação e o vigia indexam pela matrícula preenchida com zeros. O PDF do TXT gravado e `haiku_export` também consultam o cadastro para cruzar matrículas. `app/folha/export.py:737`, `app/folha/export.py:849–884`, `app/folha/views.py:427–429`, `app/folha/previa.py:97–105`, `app/folha/views.py:762–768`, `app/folha/views.py:813–837`, `app/folha/retidos_pdf.py:88–95`, `app/folha/validacao_pdf.py:65–72`, `app/folha/services/vigia_txt.py:98–116`, `app/folha/pdf_export.py:147–148`, `app/folha/haiku_export.py:111–116`. |
| **Retrato, fechamento e relatórios** | O Retrato grava `codigo` do item. Fechamento, Pendências de Cadastro e Quem Cobrar leem o veredito do juiz. O contador de cadastro lê o valor pelo alias `codigo_folha`, e a API de cadastro incompleto tem filtro próprio de `NULL`/`''`, sem `strip()`. `app/folha/retrato_folha.py:1212–1243`, `app/ponto/services/fechamento_lista.py:25–37`, `app/ponto/services/fechamento_lista.py:65–73`, `app/relatorios/views_pend.py:47–69`, `app/relatorios/views.py:1723–1737`, `app/colaboradores/services/cadastro.py:74–87`, `app/colaboradores/services/cadastro.py:99–120`, `app/api/views_mensageria.py:1275–1288`. |
| **Holerite** | O parser Domínio extrai um identificador numérico do PDF. A configuração padrão Domínio casa por nome completo, com CPF quando fornecido pela página. O modo configurável oferece `codigo_dominio` e pode filtrar o colaborador por esse campo e pela empresa. `app/holerite/motor.py:96–118`, `app/holerite/motor.py:161–163`, `app/holerite/motor.py:192–230`, `app/holerite/motor.py:298–313`, `app/holerite/views.py:1741`, `app/templates/holerite/upload_lote_v2.html:46`. |

O corte anterior `EXPORT-UM-JUIZ-DO-CODIGO` registra um censo de **8 escritores, 20 leitores e 4 predicados vivos**, sem enumerá-los naquela linha. Equivalência entre esse levantamento e o censo da cópia `[nao verificado]`. `app/docs/CORTES.md:587`.

## C. Quem lê o vocabulário do motivo

| Sítio | Efeito de emitir `codigo_dominio_invalido` |
|---|---|
| **`MOTIVO_RH` e `rotulo_do_motivo`** | A entrada no mapa dá o rótulo humano à prévia do export, ao PDF de retidos e, via `rotulo_do_motivo`, à lista, ao PDF, ao Raio-X e ao drawer do Retrato. Chave desconhecida aparece crua. `app/folha/views.py:12–24`, `app/folha/views.py:373`, `app/folha/views.py:415–429`, `app/folha/views.py:884–885`, `app/folha/retidos_pdf.py:83–95`, `app/folha/retrato_folha.py:76–86`, `app/folha/retrato_folha.py:1119`, `app/folha/views_retrato.py:291`, `app/folha/views_retrato.py:424`, `app/folha/views_retrato.py:503`. |
| **Tipo e faixa** | A nova chave precisa de destino `cadastro` em `TIPO_POR_MOTIVO`, junto de `sem_codigo_dominio`. `_tipo_cru` usa esse mapa para retenções de cadastro e `TIPO_POR_CAUSA` para `furo_espelho`. `MOTIVOS_DE_CADASTRO` deriva do primeiro mapa e `_saldo_cru` já devolve `cadastro` mesmo com `dias=None`. `app/folha/retrato_folha.py:210–230`, `app/folha/retrato_folha.py:379–390`, `app/folha/retrato_folha.py:422–433`. |
| **Dono e fio** | `_fio_cru` já devolve `admin` para bloqueio diferente de `furo_espelho`, excetuado o módulo próprio. `montar_linhas` grava esse fio em `dono`, `DONO_DO_FIO` o leva a `admin` e o rótulo é `Admin`. `models.py:321` documenta a regra e dá `sem_codigo_dominio` como exemplo. `app/folha/retrato_folha.py:33`, `app/folha/retrato_folha.py:194–201`, `app/folha/retrato_folha.py:372–376`, `app/folha/retrato_folha.py:486–493`, `app/folha/retrato_folha.py:1248–1251`, `app/folha/models.py:315–323`. |
| **Listas e rótulos locais** | `_MOTIVO_NA_TELA` do fechamento cai na pílula `espelho` para chaves novas. `_PORTAS_CADASTRO` filtra apenas as chaves declaradas, então Pendências de Cadastro omite o novo motivo. Quem Cobrar e `EM_PORTUGUES` da mensageria caem na chave crua. Esses sítios precisam conhecer a chave para mostrá-la com o mesmo sentido. A API que conta apenas código faltante tem outro universo e outro predicado. `app/ponto/services/fechamento_lista.py:21–37`, `app/templates/ponto/_fech_linha.html:11`, `app/relatorios/views_pend.py:47–69`, `app/relatorios/views.py:1727–1737`, `app/relatorios/views.py:1762`, `mensageria/nucleo/ferramentas.py:1196–1203`, `mensageria/nucleo/ferramentas.py:1246–1253`, `app/api/views_mensageria.py:1275–1288`. |
| **Agrupadores e vigias** | O Raio-X do export põe chaves fora de `_COM_PILULA` em **Outras pendências**. `fora_da_folha`, a API de prontidão, as métricas e o simulador agrupam pela chave recebida. O vigia do TXT transporta o motivo. A enumeração em `vigia_pausa.py:29` é documentação da precedência. O contador daquele vigia usa `motivos_retencao_celula`. `app/folha/views.py:615–627`, `app/folha/views.py:661–667`, `app/folha/fora_da_folha.py:147–173`, `app/api/views_mensageria.py:283–302`, `app/inteligencia/calculadores.py:210–220`, `app/folha/management/commands/simular_folha.py:133–146`, `app/folha/services/vigia_txt.py:114–116`, `app/folha/services/vigia_pausa.py:26–32`, `app/folha/services/vigia_pausa.py:123–132`. |

## D. Como mostrar o valor gravado

`montar_linhas` já grava `codigo` ao lado de `motivo`. O valor vem do item do juiz depois de `strip()`, antes de `zfill(10)`. `detalhe` também existe no item, nasce vazio e é preenchido pelos ramos, inclusive pelo de espelho a partir de `:977`. A linha JSON do Retrato usa as chaves estruturadas e deixa `detalhe` no item do export. `app/folha/export.py:846–858`, `app/folha/export.py:881–884`, `app/folha/export.py:977–987`, `app/folha/retrato_folha.py:1237–1256`, `app/folha/models.py:350–354`.

**Já há forma para tela e PDF.** A projeção compartilhada do *Por quê* pode entregar o rótulo literal de `MOTIVO_RH` e, para a nova chave, o rótulo fixo `codigo Dominio =` com o dado `linha['codigo']`. A lista, o PDF e o Raio-X recebem os mesmos componentes. No drawer, `r.valor` continua literal do mapa, com o código exibido como dado ao lado. O próprio drawer já exibe datas gravadas como dados e o PDF de retidos já tem coluna estruturada de código. Isso resolve `Código Domínio inválido · codigo Dominio = VIGIA` sem prosa livre nem consulta ao cadastro atual. Interpolar o código dentro de `r.valor` quebraria o selo literal assim que a nova linha entrasse na fixture. `app/folha/views_retrato.py:278–293`, `app/folha/views_retrato.py:414–425`, `app/folha/views_retrato.py:500–506`, `app/folha/retrato_folha.py:1079–1093`, `app/templates/folha/_retrato_o_que_fazer.html:33–50`, `app/folha/retidos_pdf.py:88–95`, `app/folha/tests/test_retrato_folha_drawer.py:213–231`, `app/docs/CORTES.md:405`.

## E. Leis e juiz registrado

- **L-001, L-002, L-011 e L-095.** A cura da classificação fica na fonte, as testemunhas leem a resposta, autoridade nova exigiria corte e o dinheiro lavrado segue sendo lido. O registro vivo põe `_K2` **quem entra no TXT** e `_K1` **o que bloqueia aprovação/tranca** na mesma função. O motivo pertence a `_K2`. `_K1` sai em `travas_aprovacao`, montada antes dos retornos antecipados. `app/docs/LEIS.md:59–60`, `app/docs/LEIS.md:69`, `app/docs/LEIS.md:113`, `app/core/juizes.py:180–181`, `app/core/juizes.py:326–337`, `app/folha/export.py:779–782`, `app/folha/export.py:839–858`, `app/docs/DOSSIES.md:85–89`, `app/docs/DOSSIES.md:118–122`.
- **L-099, O256 e Saldo Cadastro.** L-099 separa a inconsistência estrutural do valor de cadastro, cuja correção fica com o Admin. A O256 manda o drawer ler os dados lavrados. O complemento põe cadastro sem forma de dia na faixa **Cadastro** e sela a igualdade entre a soma do Saldo e Bloqueados. `app/docs/LEIS.md:122`, `app/docs/CORTES.md:392`, `app/docs/CORTES.md:405`, `app/folha/tests/test_retrato_folha_quadros.py:1133–1160`.
- **L-026/L-090, L-092/L-113 e L-121.** A janela vem do corte da empresa. Para exportadas, a L-113 e sua emenda qualificam a regra histórica da L-092, preservam o TXT anterior e tratam DIFF e aviso quando aplicáveis. L-121 liga cura de juiz à reconstrução das projeções pela porta. `app/docs/LEIS.md:77`, `app/docs/LEIS.md:123–125`, `app/docs/LEIS.md:142`, `app/docs/LEIS.md:155–156`, `app/folha/export.py:47–51`.
- **Diferença entre corte e cópia.** `RESPOSTAS-COERENCIA-1.6` manda levar `STATUS_NO_TXT` para dentro do juiz. Nesta cópia o juiz consulta todos os fechamentos da competência e o montador aplica `STATUS_NO_TXT = ('aberto', 'aprovado')`. O dossiê usa esse comportamento observado como linha de base. `app/docs/CORTES.md:523`, `app/folha/export.py:696`, `app/folha/export.py:723–731`, `app/folha/export.py:791–797`.

## F. Selos atuais e REDs descritos

**Selos atuais.** `MotivoExaustivoTest` colhe por AST as seis chaves literais atuais e fica vermelho se a nova chave vier sem destino no eixo Tipo. Os testes de dono e faixa têm listas literais de exemplos, e a asserção que deriva Cadastro do mapa já cobre entradas novas. O selo Saldo = Bloqueados só observa a nova linha se ela entrar na fixture. O selo do drawer cobra o valor literal em mapa e cinco linhas do bloco. `app/folha/tests/test_retrato_folha_quadros.py:255–280`, `app/folha/tests/test_retrato_folha_quadros.py:131–136`, `app/folha/tests/test_retrato_folha_quadros.py:314–335`, `app/folha/tests/test_retrato_folha_quadros.py:375–398`, `app/folha/tests/test_retrato_folha_quadros.py:1133–1160`, `app/folha/tests/test_retrato_folha_drawer.py:208–231`.

**Precedência já selada em partes.** Há casos de rescisão, código vazio, vazio simultâneo a turno aberto com preservação de `travas_aprovacao`, férias com batida, cadastro zero antes de furo e rubrica não homologada. Há também selo de motivo desconhecido que continua visível e selo de PDF que lê o mesmo recorte sem chamar o juiz. Esses testes protegem regressões, mas nenhum desses casos exige o novo motivo. `app/folha/tests/test_rescisao_modulo_proprio.py:24–38`, `app/folha/tests/test_classificar_export.py:34–66`, `app/folha/tests/test_classificar_export.py:113–130`, `app/folha/tests/test_contrato_export_um_juiz.py:132–150`, `app/folha/tests/test_ferias_com_batida.py:74–94`, `app/folha/tests/test_cadastro_zero_com_batida.py:161–168`, `app/folha/tests/test_integracao.py:143–152`, `app/folha/tests/test_retrato_pdf.py:138–150`, `app/folha/tests/test_retrato_pdf.py:330–345`.

| Entrada de teste | Saída esperada | [nao verificado]
|---|---|
| **`VIGIA`, fechamento apto nos demais critérios e rubrica que geraria reg10** | Um item `fora/codigo_dominio_invalido`, com `codigo_dominio='VIGIA'`. O ID sai do conjunto de aptos e nenhuma linha leva `00000VIGIA`. A rubrica positiva faz o caso detectar a falha atual de layout. `app/folha/export.py:723–753`, `app/folha/export.py:849–884`, `app/folha/export.py:277–279`. |
| **Controle numérico `00123`, mesma janela e demais insumos** | Continua `entra`, matrícula `0000000123`, reg10 idêntico ao controle anterior. No lote misto com esse controle o TXT tem linha, `validar_layout_dominio(linhas) == []` e a rota entrega o arquivo. `app/folha/export.py:34–45`, `app/folha/export.py:725–755`, `app/folha/views.py:275–292`, `app/folha/tests/test_integracao.py:211`. |
| **`''` e `'   '`** | Ambos continuam `fora/sem_codigo_dominio`, sem linha no TXT. `app/folha/export.py:849`, `app/folha/export.py:881–884`, `app/folha/tests/test_classificar_export.py:40–45`. |
| **Retrato lavrado com `VIGIA`, depois cadastro alterado para `00123`** | Na lista, no PDF do mesmo recorte e no drawer, verificar o rótulo do motivo, o valor `VIGIA` da foto e `Depende de = Admin`. A linha está em **Cadastro**, com `dias`/`datas` vazios. No drawer, verificar também que o valor de mapa segue literal. `app/folha/retrato_folha.py:1212–1251`, `app/folha/views_retrato.py:194–201`, `app/folha/views_retrato.py:278–293`, `app/folha/views_retrato.py:414–425`, `app/folha/tests/test_retrato_folha_drawer.py:213–231`. |
| **POST real de admissão e edição com os outros campos válidos** | `VIGIA` retorna mensagem visível, por exemplo `Código Domínio deve conter apenas números.` Na admissão, não nasce colaborador nem usuário. Na edição, o registro anterior fica intacto e o valor digitado reaparece. `00123` grava exatamente `00123` nas duas portas. Casos de regressão para manter a admissão sem código e a edição com campo vazio conforme o comportamento atual. `app/colaboradores/views.py:1131`, `app/colaboradores/views.py:1165–1168`, `app/colaboradores/views.py:1190–1237`, `app/colaboradores/views.py:1267–1285`, `app/colaboradores/views.py:1353–1365`, `app/core/respostas.py:58–65`, `app/core/respostas.py:74–85`. |

**Bordas verificadas em memória com o código da cópia.** `' 00123 '` e `'00123'` saem como `0000000123`. `'00000'` sai como `0000000000`. Espaço interno e sinal são rejeitados pelo `.isdigit()` do layout. Onze dígitos produzem reg10 de 44 caracteres e falham no comprimento. O `.isdigit()` local aceita dígitos Unicode, inclusive `１２３` e `²`. Aceitação desses caracteres pelo importador externo `[nao verificado]`. Os REDs de integração e a suíte ficaram `[nao verificado]`. `app/folha/export.py:34–45`, `app/folha/export.py:272–279`, `app/folha/export.py:737`, `app/folha/export.py:849–884`.

## G. Contrato de entrada e perguntas de lei

- **Fonte e unidade.** Para o TXT, `FechamentoMensal` e `Colaborador.codigo_dominio`, uma decisão por colaborador × empresa × competência. O caminho normal consulta o fechamento gravado e o caminho de export pode injetar os fechamentos que já tem em mãos. Para o cadastro, uma submissão de `codigo_folha` na admissão ou edição. 

[truncado em 20000 de 22700 caracteres]

## 28/11 01:10 · sessao d4649f8dfc9b · RETRATO-PDF-CORTA-POR-QUE · leitura · origem=Code

## RETRATO-PDF-CORTA-POR-QUE

A linha lavrada tem **um `tipo` por pessoa e uma lista plana de `datas`**. O vínculo entre motivo e data existe na fonte anterior à lavra, mas fica de fora do JSON. O anexo `JUIZES.md` localiza a autoridade de entrada no TXT. Linhas reais, contadores de produção, PDF renderizado em produção e ambiente da casca estão `[nao verificado]`. `app/folha/export.py:1202` · `app/folha/fora_da_folha.py:234` · `app/folha/retrato_folha.py:1237` · `app/docs/JUIZES.md:119`

### A. Mapa das colunas e do vocabulário

Tela e PDF passam por `_recorte_da_barra`. Ele lê `RetratoFolha.linhas` via `linhas_do_retrato`, junta o cadastro, aplica o filtro universal, passa por `no_periodo` e entrega `ordenar(recortar(...))`. A tela projeta `_tabela` e usa `_retrato_lista.html`, tanto na página cheia quanto no parcial. O PDF monta a tabela em `_story`, dentro de `retrato_pdf`. `app/folha/retrato_folha.py:98` · `app/folha/views_retrato.py:194` · `app/folha/views_retrato.py:225` · `app/folha/views_retrato.py:278` · `app/folha/views_retrato.py:345` · `app/templates/folha/retrato_folha.html:53` · `app/templates/folha/_retrato_parcial.html:7` · `app/folha/views_retrato.py:406`

| Coluna | Origem e saída |
|---|---|
| LED na tela / **Depende de** no PDF | `desenho_do_led` lê `led_da_linha`, que usa `dono_da_linha` para bloqueados e o próprio balde para `Entram` e `Rescisão`. A tela desenha o ponto com cor, rótulo e link de filtro. O PDF imprime o rótulo na cor do LED. `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:531` · `app/folha/retrato_folha.py:559` · `app/templates/folha/_retrato_lista.html:47` · `app/folha/views_retrato.py:418` |
| **Colaborador** | `_cadastro` junta `Colaborador.nome` pelo ID `colab`. As duas saídas usam `(cadastro ausente)` se o nome estiver vazio. Na tela, o nome abre o drawer com mês e ano. `app/folha/views_retrato.py:99` · `app/folha/views_retrato.py:118` · `app/folha/views_retrato.py:287` · `app/folha/views_retrato.py:421` · `app/templates/folha/_retrato_lista.html:60` |
| **Posto** | `_cadastro` prefere o posto do vínculo ativo mais recente e, na falta dele, usa o posto do colaborador. As duas saídas exibem `posto.nome`. `app/folha/views_retrato.py:107` · `app/folha/views_retrato.py:115` · `app/templates/folha/_retrato_lista.html:63` · `app/folha/views_retrato.py:422` |
| **Escala** | `_cadastro` pega `tipo_escala` daquele vínculo ativo. As duas saídas exibem `descricao`. `app/folha/views_retrato.py:107` · `app/folha/views_retrato.py:122` · `app/templates/folha/_retrato_lista.html:64` · `app/folha/views_retrato.py:423` |
| **Por quê** | Ambas aplicam `rotulo_do_motivo(linha['motivo'])`. O HTML imprime `l.motivo`. O rótulo visível **“Espelho com pendencia”** vem de `MOTIVO_RH['furo_espelho']`. O export também monta um `detalhe` com esse prefixo e os códigos, mas essa chave fica fora da linha lavrada. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/templates/folha/_retrato_lista.html:65` · `app/folha/retrato_folha.py:76` · `app/folha/views.py:18` · `app/folha/export.py:977` · `app/folha/retrato_folha.py:1237` |
| **Dias que seguram** | `dias` vem da lavra. Com janela, `no_periodo` conta as datas no intervalo inclusivo e recalcula a faixa `saldo`. `None` sai em branco nas duas saídas. `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1334` · `app/templates/folha/_retrato_lista.html:66` · `app/folha/views_retrato.py:425` |

- **Pílulas e Tipo.** `ROTULO_DO_DONO` alimenta o quadro `Depende de` e os LEDs. `ROTULO_DO_TIPO` contém `Escala`, `Folga`, `Sem escala`, `Não bate`, `Falta`, `Cadastro`, `Férias` e `Sem causa`. O eixo `tipo` continua no recorte, no Raio-X e no drawer. A barra atual desenha `dono` e, com Bloqueados ativo, `saldo`. O **Por quê** consulta o mapa `MOTIVO_RH`. As expressões do exemplo “Batida faltando” e “pergunta aberta” não são entradas desses mapas. `app/folha/retrato_folha.py:200` · `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:587` · `app/folha/retrato_folha.py:906` · `app/folha/retrato_folha.py:1118` · `app/folha/views_retrato.py:500` · `app/folha/views.py:15`
- **Forma exata da linha construída.** As chaves são `colab`, `codigo`, `status`, `motivo`, `dias`, `datas`, `dias_sem_leitura`, `dias_sem_porta`, `dono`, `acoes`, `tipo`, `saldo` e `informativo`. `datas` é lista plana ISO `AAAA-MM-DD` ou `None` quando a fonte não dá datas, com possibilidade de a chave faltar em retratos legados. As duas listas `dias_sem_*` registram sem leitura e sem porta. `acoes` guarda as portas publicadas, algumas com `dia_do_turno`. `informativo` é uma lista ISO separada para rescisão. Nenhuma dessas chaves guarda a associação geral `data → motivo/tipo de retenção`. `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1237` · `app/folha/retrato_folha.py:1288` · `app/folha/fora_da_folha.py:215` · `app/folha/models.py:334`
- **Onde havia o par.** `motivos_retencao_celula(com_dias=True)` devolve motivos agregados e registros `{data, motivo, veredito, celula_id}`. `fora_da_folha` tem esses registros em `_dias`, mas publica no resumo por pessoa `motivos` agregados e `datas` planas. O lavrador leva as datas ao retrato e usa `dias_ferias_com_batida` como segunda fonte. A função avulsa `o_que_segura` expõe `data`, `motivo`, `veredito` e `fio` por dia e consulta o export em sua própria execução. `app/folha/export.py:1195` · `app/folha/export.py:1268` · `app/folha/fora_da_folha.py:214` · `app/folha/fora_da_folha.py:234` · `app/folha/fora_da_folha.py:257` · `app/folha/fora_da_folha.py:273` · `app/folha/fora_da_folha.py:292` · `app/folha/retrato_folha.py:1231`

### B. Censo de escritores e leitores

| Campo | Escrita e leitura na cópia |
|---|---|
| `motivo` | `classificar_export` produz a chave no item. `montar_linhas` copia o valor para a linha. `_balde` usa o motivo para reconhecer rescisão. `fio_da_linha`, `tipo_da_linha`, `saldo_da_linha` e `no_periodo` também o consultam. Tela, PDF, Raio-X e drawer passam o valor a `rotulo_do_motivo`. `app/folha/fora_da_folha.py:147` · `app/folha/export.py:977` · `app/folha/retrato_folha.py:1225` · `app/folha/retrato_folha.py:89` · `app/folha/retrato_folha.py:444` · `app/folha/retrato_folha.py:496` · `app/folha/retrato_folha.py:1338` · `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/views_retrato.py:503` · `app/folha/retrato_folha.py:1116` |
| `dono` | `_fio_cru` grava o **fio** (`admin`, `colab`, `chamado`, `ninguem`, `nao_sei` ou vazio). `fio_da_linha` lê a chave, com tratamento de legado. `dono_da_linha` traduz o fio para o balde da UI e trata `Hoje` usando `datas`. Esses leitores alimentam o recorte, os contadores, o LED, o drawer e o vigia. `app/folha/retrato_folha.py:178` · `app/folha/retrato_folha.py:351` · `app/folha/retrato_folha.py:436` · `app/folha/retrato_folha.py:486` · `app/folha/retrato_folha.py:588` · `app/folha/retrato_folha.py:1112` · `app/folha/retrato_folha.py:1248` · `app/folha/vigia_retido_calado.py:77` |
| `tipo` | `_tipo_cru` grava um tipo por pessoa a partir do motivo ou de `FechamentoMensal.causa_espelho['causa']`. `_causas_gravadas` lê essa causa. `tipo_da_linha` é o leitor do recorte, do agrupamento do Raio-X e dos rótulos e portas do drawer. `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:496` · `app/folha/retrato_folha.py:588` · `app/folha/retrato_folha.py:1066` · `app/folha/retrato_folha.py:1124` · `app/folha/retrato_folha.py:1167` · `app/folha/retrato_folha.py:1250` · `app/folha/views_retrato.py:506` |
| `datas` | `montar_linhas` guarda a lista. `no_periodo` lê a lista e devolve cópias com `dias` e `saldo` ajustados, mantendo `datas` integral. `dono_da_linha` lê a lista para `Hoje`. O drawer localiza a linha via `linha_do_colab` e imprime as datas em ISO. `app/folha/retrato_folha.py:1231` · `app/folha/retrato_folha.py:489` · `app/folha/retrato_folha.py:1334` · `app/colaboradores/views.py:1001` · `app/templates/folha/_retrato_o_que_fazer.html:42` |
| `motivos_espelho` | Chave do **item do export**, fora do JSON do retrato. `classificar_export` a preenche com a lista de códigos agregados de `motivos_retencao_espelho`, que delega ao retentor. Leitores diretos do item incluem a prévia da folha, o PDF legado de retidos e `fechamento_lista._folha_do_juiz`. Há um campo homônimo em `FechamentoMensal`, preenchido por `recalcular_fechamento_mes` a partir do mesmo juiz. Tela e PDF do retrato leem `motivo` para a coluna. `app/folha/export.py:977` · `app/folha/export.py:1310` · `app/folha/views.py:267` · `app/folha/views.py:424` · `app/folha/retidos_pdf.py:90` · `app/ponto/services/fechamento_lista.py:28` · `app/ponto/models.py:451` · `app/ponto/services/fechamento.py:1091` · `app/ponto/services/fechamento.py:1210` · `app/folha/retrato_folha.py:1237` · `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` |

O escritor persistente do retrato é `lavrar_retrato_folha --apply`, com `update_or_create` por empresa, mês e ano. `montar_linhas` constrói o JSON. O cron chama a mesma porta. O selo AST exige um único chamador de produção de `montar_linhas`. `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/management/commands/lavrar_retrato_folha.py:72` · `app/config/crons.py:1659` · `app/folha/tests/test_retrato_folha_um_derivador.py:68`

**Contadores de dono.** A tela chama `quadros_barra_retrato`, e `_pilula` calcula `numero = len(recortar(...))` sobre o universo após filtro universal e janela. A pílula ativa conta o recorte atual. Cada pílula inativa conta o recorte que seu clique abriria, substituindo o dono e mantendo os outros eixos. O PDF chama a mesma função, com o mesmo universo e grupos, mas consome apenas título e rótulo dos filtros ativos. Hoje imprime `n de total`, sem os `numero` por dono. `contadores(retrato)` é outra função e conta os baldes gerais `entram`, `bloqueado`, `modulo_proprio` e `total`. `app/folha/views_retrato.py:324` · `app/folha/retrato_folha.py:808` · `app/folha/retrato_folha.py:838` · `app/templates/core/_barra_gestao.html:67` · `app/folha/views_retrato.py:399` · `app/folha/views_retrato.py:409` · `app/folha/retrato_folha.py:114`

### C. Como abre e precedente de `attachment`

O botão da barra é `<a href="{{ pdf_href }}">` do partial `core/_btn_pdf.html`, sem `target="_blank"`, sem `download` e sem `onclick` nesse ramo. A view monta o `href` da rota `/folha/exportar/retrato.pdf` com `qs_do_recorte`; o mesmo botão volta no parcial. A resposta atual é `application/pdf` com `Content-Disposition: inline; filename="retrato-folha-emp…-MM-AAAA.pdf"`. Resultado do clique no navegador de produção `[nao verificado]`. `app/templates/folha/_retrato_barra.html:50` · `app/templates/core/_btn_pdf.html:34` · `app/templates/folha/_retrato_parcial.html:8` · `app/folha/views_retrato.py:337` · `app/config/urls.py:34` · `app/folha/urls.py:25` · `app/folha/views_retrato.py:450`

Na mesma casa, o PDF do histórico do TXT, `retidos_pdf` e `validar_export_pdf` usam `application/pdf` com `Content-Disposition: attachment; filename="…"`. O `retidos_pdf` legado chama `classificar_export` ao vivo. O precedente para este item é o cabeçalho HTTP. `app/folha/views.py:860` · `app/folha/views.py:884` · `app/folha/views.py:911`

### D. Rodapé e fontes de commit [nao verificado]

`gerar_pdf_auditavel` lê `get_commit_git()` uma vez, passa o resultado aos rodapés das duas passagens e grava `RegistroPDFAuditavel.commit_git`. `RodapeAuditavel.__call__` imprime os oito primeiros caracteres ou `n/d` quando recebe vazio. O helper prioriza `COMMIT_GIT` e, na falta dela, tenta `git -C /app rev-parse HEAD` se `/app/.git` existir. Se nenhum caminho entregar valor, retorna `''`. `app/relatorios/pdf_base.py:72` · `app/relatorios/pdf_base.py:133` · `app/relatorios/pdf_base.py:152` · `app/relatorios/pdf_base.py:170` · `app/relatorios/utils_audit.py:7`

A montagem documentada leva `app/` a `/app`, enquanto o repositório fica no diretório pai no host. O Dockerfile usa `python:3.13-slim`, copia o contexto e não declara `COMMIT_GIT` nem instala `git` explicitamente. Isso explica o caminho de `n/d` quando o ambiente efetivo também não fornece a variável. O compose de produção é excluído da cópia versionada. A variável, o executável, `/app/.git` e o SHA efetivo da casca de produção estão `[nao verificado]`. `CLAUDE.md:88` · `app/.dockerignore:8` · `app/Dockerfile:1` · `app/Dockerfile:30` · `app/Dockerfile:40` · `.gitignore:40` · `app/relatorios/utils_audit.py:12`

| Fonte existente | O que identifica |
|---|---|
| `AMBIENTE sha=` | `HASNER_SHA` injetado pela suite para a árvore de teste medida. O teste imprime a variável e `pista_certifica.sh` compara o valor com o SHA do pouso. A disponibilidade na casca web de produção está `[nao verificado]`. `bin/suite.sh:109` · `bin/suite.sh:566` · `app/core/tests/test_ambiente_do_run.py:52` · `bin/pista_certifica.sh:48` |
| Carimbo de deploy | `bin/deploy.sh` escreve o `COMMIT` lido do Git do **host** em `logs/deploy.stamp`. O vigia já lê esse carimbo. A correspondência com o código carregado pelo worker que gera um PDF está `[nao verificado]`. `bin/deploy.sh:430` · `app/core/esteira_vigia.py:1032` · `app/core/esteira_vigia.py:1202` |
| `/health/` e versão do app | O health do core devolve `ok`, `schema` e `host`. O de mensageria devolve `ok` e `servico`. A versão da API é a versão do aplicativo Android/iOS. Esses retornos não incluem SHA do servidor. `app/config/urls_core.py:5` · `app/core/views.py:1306` · `mensageria/config/urls.py:7` · `app/api/views.py:3068` |

### E. Escala no PDF

A célula recebe `getattr(_l.get('_escala'), 'descricao', '') or ''` como **string crua**. As larguras são `26 / 68 / 48 / 38 / 60 / 24 mm`, com **38 mm para Escala** e **60 mm para Por quê**. A tabela repete o cabeçalho, usa fonte 7 e alinhamento vertical no topo. Os `Paragraph` dessa função estão no título e no resumo, fora da tabela. O PDF legado de retidos mostra o uso de `Paragraph(escape(...))` dentro de célula. O vazamento em um PDF real está `[nao verificado]`. `app/folha/views_retrato.py:406` · `app/folha/views_retrato.py:423` · `app/folha/views_retrato.py:431` · `app/folha/retidos_pdf.py:91`

### F. Ordem atual

- **Linhas da tela e do PDF.** Ambos iteram o mesmo `_vis`, ordenado por **Bloqueados → Entram → Rescisão**, depois pelas faixas **1 → 2 a 5 → 6 ou mais → Cadastro → sem faixa**, depois pelo ID do colaborador. Dois bloqueados na mesma faixa são ordenados pelo ID, mesmo com quantidades de dias diferentes. `app/folha/views_retrato.py:232` · `app/folha/views_retrato.py:293` · `app/templates/folha/_retrato_lista.html:35` · `app/folha/views_retrato.py:417` · `app/folha/retrato_folha.py:754`
- **Donos na barra.** **Admin → Colab → Hoje → Sem ação disponível → Sem leitura (sistema)**. A ordem vem de `DONOS` via `VALORES_DO_GRUPO` e `quadros_barra_retrato`. `Entram` e `Rescisão` são baldes sem dono, com rótulos próprios no PDF. `app/folha/retrato_folha.py:191` · `app/folha/retrato_folha.py:200` · `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:590` · `app/folha/retrato_folha.py:908`

### G. Leis e cortes

- **L-002, L-007 e L-011.** “TESTEMUNHA LE, NAO RECALCULA” exige uma autoridade comum para tela, PDF, TXT, badge e contador. L-007 exige um escritor canônico por estado. L-011 exige a frase de corte para registrar um juiz novo. `classificar_export` está registrado em K1 e K2. O corte `O256-LEGENDA-E-PDF-ANTES-DO-RAIO-X` manda o PDF respeitar os filtros da barra e fixa “Juizes novos = 0”. `app/docs/LEIS.md:60` · `app/docs/LEIS.md:65` · `app/docs/LEIS.md:69` · `app/core/juizes.py:336` · `app/docs/CORTES.md:407`
- **L-026/L-090 e L-038.** As duas primeiras protegem a janela da empresa em `ponto/janelas.py`, usada na entrada do recorte. L-038 diz “o PDF nao tem regra de dia; le o MESMO juiz do espelho” e nomeia `relatorios/pdf_espelho.py::_coletar_dados_espelho`. A paridade específica do PDF do retrato está em L-002 e no corte O256 acima. `app/docs/LEIS.md:77` · `app/docs/LEIS.md:86` · `app/docs/LEIS.md:123` · `app/folha/views_retrato.py:177` · `app/docs/CORTES.md:407`
- **Leis de UI L1, L2, L6, L7 e L9.** Contador e lista vêm da mesma fonte, tela e ação consultam a mesma função, o filtro é cumulativo e único, a ajuda sai do mesmo mapa, e o botão PDF usa um partial único com rótulo `PDF` e ajuda no `title`. `RETRATO-DONO-SAO-TRES` registrou a soma dos donos com os bloqueados, e LU-05 deu balde próprio ao dia sem porta. A cópia também declara o balde `Sem leitura (sistema)`. `app/docs/LEIS-UI.md:12` · `app/docs/LEIS-UI.md:23` · `app/docs/LEIS-UI.md:68` · `app/docs/LEIS-UI.md:77` · `app/docs/LEIS-UI.md:95` · `app/docs/CORTES.md:408` · `app/docs/CORTES.md:539` · `app/folha/retrato_folha.py:191`
- **“NENHUM TEXTO SE GERA”.** O literal de `O256-ORDEM-DRAWER-PRIMEIRO` se refere ao bloco **O que fazer** do drawer, “montado SO das chaves gravadas na linha do retrato (motivo, dono, tipo, datas), sem texto gerado”. A docstring diz “os valores sao ROTULOS de mapa [...] nunca frase montada”. O selo do drawer exige que o valor inteiro pertença literalmente a um mapa existente. O exemplo do item 2 tem a forma de **rótulos concatenados com datas**. A permissão para essa composição na coluna da tela e no PDF, e sua eventual extensão ao drawer, fica em aberto. `app/docs/CORTES.md:405` · `app/folha/retrato_folha.py:1089` · `app/folha/tests/test_retrato_folha_drawer.py:213`

### H. Testes atuais e o que fica vermelho [nao verificado]

- **PDF.** `test_retrato_pdf.py` cobre bytes `%PDF`, tipo, nome canônico no header, hash e registro, igualdade de tamanho do recorte com a tela, filtros na trilha, zero chamada à fonte, papel vazio, permissão, link no parcial e recorte comum por AST. O teste de header procura o **nome** e aceita `attachment` com o mesmo nome. O teste de clique segue o `href` com `client.get`. Ele não mede abas ou navegação de navegador. `app/folha/tests/test_retrato_pdf.py:93` · `app/folha/tests/test_retrato_pdf.py:99` · `app/folha/tests/test_retrato_pdf.py:108` · `app/folha/tests/test_retrato_pdf.py:127` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_pdf.py:152` · `app/folha/tests/test_retrato_pdf.py:163` · `app/folha/tests/test_retrato_pdf.py:221` · `app/folha/tests/test_retrato_pdf.py:252` · `app/folha/tests/test_retrato_pdf.py:330`
- **Por quê.** Há assert do cabeçalho e do vínculo com `MOTIVO_RH`. O teste da listagem verifica a presença do rótulo de cadastro, sem exigir igualdade da célula inteira. Se a composição for aplicada ao valor do **drawer**, o selo de literal de mapa fica vermelho. Se a janela passar a mudar o texto de **Por quê** na linha comparada pelo teste de período, o assert que preserva todas as colunas salvo Dias fica vermelho. `app/folha/tests/test_retrato_folha_tela2.py:253` · `app/folha/tests/test_retrato_folha_contador.py:190` · `app/folha/tests/test_retrato_folha_drawer.py:181` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/folha/tests/test_retrato_folha_periodo.py:371`
- **Ordem, contagem e botão.** Alterar `DONOS` para obter a ordem do PDF quebra o assert da ordem da barra. Mudar a ordem compartilhada de forma que a tela deixe de seguir as faixas quebra as expectativas de ordenação e período. Escrever um botão PDF fora do partial quebra L9. Os REDs abaixo são previsões por leitura. Execução da suíte `[nao verificado]`. `app/folha/tests/test_retrato_folha_dono_sao_tres.py:47` · `app/folha/tests/test_retrato_folha_dono_sao_tres.py:105` · `app/folha/tests/test_retrato_folha_quadros.py:569` · `app/folha/tests/test_retrato_folha_periodo.py:354` · `app/core/tests/test_contract_btn_pdf.py:87`

### I. Matriz de REDs descritos

| Item | Entrada de teste | Saída esperada | [nao verificado]
|---|---|---|
| **1. Download** | Admin com empresa, competência e pílula ativos. Clicar no PDF após atualizar o filtro. | Mesmo `href` do recorte vigente, PDF válido, `attachment` com o nome canônico, mesma URL da tela e mesma quant

[truncado em 20000 de 24044 caracteres]

## 28/11 00:31 · sessao 48b9f6bb0a8f · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**APROVADO para a cura c12 no recorte `base.html` a 1366 px, página e drawer. Nenhum bloqueante de código encontrado.** O BLOQ 1 está curado pela ordem de pintura. A execução do GREEN e do RED da c12, a suíte inteira e o clique real pré-pouso seguem [nao verificado]. O clique real continua como pré-requisito do deploy. `aval_desktop.txt:1` · `ver3_triagem.md:3` · `app/templates/core/_drawer_generico.html:3` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `CLAUDE.md:206` · `CLAUDE.md:427`

### 1. Ordem de pintura e chamadores

O cabeçalho continua `sticky; top:0` e passa de `auto` para `z-index:1` **dentro** do drawer `10999`. Toda caixa de célula passa atrás do cabeçalho quando há sobreposição. A célula mensal isolada fica na camada `0` do drawer. O `1` do gatilho e o `0` da região rolável são internos à célula. O título e o × pertencem ao mesmo cabeçalho. No Retrato, o calendário entra direto no corpo rolável. No Espelho, entra pela aba com rolagem própria. `app/templates/core/_drawer_generico.html:3` · `app/templates/core/_drawer_generico.html:8` · `app/templates/core/_drawer_generico.html:134` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/templates/colaboradores/partials/_calendario_grade.html:95` · `app/templates/colaboradores/partials/painel_colaborador.html:182` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/folha/retrato_folha.py:1019`

**A célula continua abaixo do cabeçalho com o popover aberto. O painel destacado fica acima.** O JS move esse painel para o `body` em `11000`. A confirmação em `100000` cobre a viewport e recebe o clique sobre o × enquanto estiver visível. O posicionador limita o popover ao retângulo visível do drawer sem reservar a faixa do cabeçalho. Se o painel cruzar o ×, ele ganha o hit-test. O cruzamento efetivo a 1366 px [nao verificado]. O detalhe geo hospedado no corpo também já fica acima do cabeçalho pelo `9999` local. `app/static/js/popover-acao.js:36` · `app/static/js/popover-acao.js:67` · `app/static/js/popover-acao.js:127` · `app/templates/core/_dialogo.html:112` · `app/templates/core/_dialogo.html:119` · `app/templates/colaboradores/calendario_partial.html:70` · `app/templates/colaboradores/partials/_geo_modal.html:2`

**A promoção também alcança outros conteúdos na camada `0`.** A barra inferior `sticky` de Ausências e os controles do painel do colaborador enquanto recebem `transform` podiam pintar acima do cabeçalho por ordem de árvore. Agora pintam abaixo dele se houver interseção. O cabeçalho da tabela da seção de Férias (`2`), as sugestões de Posto (`5`) e as de Vínculo (`50`) continuam acima em caso de interseção. Com o cabeçalho subindo de `0` para `1`, nenhum conteúdo passa de baixo para cima. Sobreposição efetiva nesses caminhos e as camadas geradas pelas bibliotecas externas de mapa [nao verificado]. `app/templates/core/_drawer_generico.html:4` · `app/templates/ponto/partials/painel_ausencia.html:286` · `app/templates/colaboradores/partials/painel_colaborador.html:147` · `app/templates/ferias/_secao_colab.html:112` · `app/static/css/hasner-ponto.css:433` · `app/templates/colaboradores/partials/painel_posto.html:57` · `app/templates/colaboradores/partials/painel_vinculo.html:153` · `app/templates/colaboradores/postos.html:74`

| Caminho | Chamadores na cópia |
|---|---|
| Inclusão do genérico | As duas cascas incluem o drawer no corpo. A inclusão textual da Central está dentro do `<title>`. A de Folha está fora dos blocos do template filho. O drawer ativo nesses dois caminhos vem de `base.html`. `app/templates/base.html:7` · `app/templates/base.html:399` · `app/templates/base_app.html:154` · `app/templates/chamados/painel_gestao.html:1` · `app/templates/folha/index.html:1` |
| Colaboradores | Lista via `abrirPainelColab`, Benefícios, Postos e diagnóstico pelas duas entradas. `app/templates/colaboradores/partials/_lista_tabela.html:16` · `app/templates/core/_drawer_generico.html:91` · `app/templates/core/_drawer_generico.html:106` · `app/templates/colaboradores/beneficios_lista.html:86` · `app/templates/colaboradores/postos.html:65` · `app/templates/colaboradores/partials/_ps_linha.html:4` · `app/templates/colaboradores/partials/pendencias_disputa.html:9` |
| Folha e Retrato | Raio-X da exportação, Raio-X do Retrato, ficha pela lista ou pelo Raio-X, e links de ação para Fio, Vínculo, Cadastro, Calendário e Ausência. `app/templates/folha/index.html:26` · `app/templates/folha/_retrato_barra.html:73` · `app/templates/folha/_retrato_lista.html:61` · `app/templates/folha/_retrato_raiox.html:70` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/folha/retrato_folha.py:989` · `app/folha/retrato_folha.py:1003` · `app/folha/retrato_folha.py:1016` |
| Chamados e mapas | Arquivados do atendimento, lote, busca de arquivo e Raio-X da Central, troca de empresa na lente e três entradas que escrevem direto no drawer para mostrar mapa. `app/templates/chamados/meu_atendimento.html:45` · `app/templates/chamados/painel_gestao.html:14` · `app/templates/chamados/painel_gestao.html:416` · `app/templates/chamados/painel_gestao.html:629` · `app/templates/chamados/partials/_raiox_lente.html:5` · `app/templates/chamados/painel_gestao.html:574` · `app/templates/chamados/fio_colaborador.html:304` · `app/static/js/geo-colab-map.js:33` |
| Ausências, Férias e Holerite | Painel de Ausências, seção de Férias pela lista e pela aba da ficha, upload de lote pela lista DP e pelo histórico, e detalhe da matriz por modo. `app/templates/ponto/ausencias.html:217` · `app/templates/ferias/lista.html:81` · `app/templates/core/_drawer_generico.html:147` · `app/templates/holerite/lista_dp.html:12` · `app/templates/holerite/historico_lotes.html:7` · `app/templates/holerite/_quadrinho_celula.html:17` |

### 2. Quem escreve e quem lê

| Valor | Escrita e leitura |
|---|---|
| Estilo do cabeçalho e camadas | O template escreve `sticky`, `top:0` e o novo `z-index:1`. O navegador consome essas declarações para pintar e resolver o hit-test. O mutante lê o retorno de `removeProperty` e remove o valor inline apenas no DOM do ensaio. Drawer, painel destacado e confirmação têm seus próprios escritores de camada. `app/templates/core/_drawer_generico.html:3` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:214` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/static/js/popover-acao.js:133` · `app/templates/core/_dialogo.html:112` |
| Seletores do cabeçalho e do drawer | O genérico fornece `#drawer-geral`, `#drawer-geral-backdrop`, `#drawer-geral-titulo` e o botão que chama `fecharDrawer`. A sonda lê o drawer e o fundo e acha o botão no pai do título. A abertura padrão e os três caminhos de mapa escrevem texto no mesmo título. O botão continua chamando a função que recolhe o drawer e emite `drawer:fechou`. `app/templates/core/_drawer_generico.html:2` · `app/templates/core/_drawer_generico.html:11` · `app/templates/core/_drawer_generico.html:71` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:210` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:246` · `app/templates/chamados/painel_gestao.html:576` · `app/templates/chamados/fio_colaborador.html:306` · `app/static/js/geo-colab-map.js:35` |
| Seletores e dia do feriado | O parcial do feriado escreve `data-feriado-vira-trabalho` e `value` no campo oculto `dia`, com a data ISO. A grade escreve `data-celula-dia` no ancestral. Python escreve `DIA.isoformat()` no JSON. As duas sondas usam os mesmos seletores para achar ação e célula. O formulário envia a mesma data à view. O delta reaproveita os atributos existentes. `app/templates/ponto/partials/_feriado_vira_trabalho.html:9` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:212` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:298` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:756` · `app/ponto/views.py:3584` · `app/ponto/views.py:3615` |
| Entrada e saída novas do smoke | Python escreve `cabecalhoDrawer` no JSON para selecionar o cenário do cabeçalho. A sonda o lê para buscar `urlCalendario` direto e entrar na medição. `urlPainel` continua indicando que o cenário usa drawer e `respostas` serve os HTMLs do Client. A sonda escreve `gridArea`, solicita a rolagem, relê `scrollTop` e mede os centros após um quadro. Ela escreve `fecharNoCabecalho` e `tituloNoCabecalho` como resultados de hit-test. O driver CDP transmite os valores e Python os compara. Os booleanos medem quem recebe o ponto. O fechamento do drawer e o texto do título são observações distintas. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:124` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:218` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:244` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:579` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:756` · `app/core/tests/smoke_feriado_chromium.js:52` |

### 3. Mordida e cobertura

- **Tirar o `z-index`** é o RED explícito. A sonda tira a declaração inline, exige que a célula esteja sob os dois pontos e o `MORDE` exige `False` nos dois hit-tests. Uma falha de montagem ou a ausência prévia da declaração produz outra mensagem e reprova o controle negativo. A execução do par GREEN/RED [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:214` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:612` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:765`
- **Pôr `z-index` abaixo de `1`** deve derrubar o positivo. Em `0`, a célula posterior volta a ganhar por ordem de árvore. Com valor negativo, o cabeçalho desce ainda mais. **Tirar o `sticky`** também deve derrubá-lo, pela guarda de sobreposição. Cabeçalho e célula passam a rolar juntos e a célula deixa de cruzar os centros do título e do ×. São consequências da geometria e das asserções. A execução desses dois mutantes [nao verificado]. `app/templates/core/_drawer_generico.html:3` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:219` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:716`
- **Lacuna no desktop.** O novo teste carrega o parcial direto numa casca de ensaio, alarga uma célula e mede os dois centros com o painel fechado. O smoke anterior cobre o popover na ficha e no Espelho, e fecha o drawer chamando `window.fecharDrawer()`. Falta o percurso integrado pelo link real do Retrato, com a grade intacta, rolagem e clique no ×, inclusive a disputa com o popover e a confirmação. O submit do feriado tem hit-test no navegador e teste do POST com motivo no Client Django. O percurso inteiro pelo navegador segue [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:208` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:260` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:421` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:488` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:731` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/ponto/tests/test_kaua_feriado_pela_tela.py:149`

### 4. Classes que já reprovaram obra

- **Segundo juiz, segundo escritor, relógio fora da borda e derivação paralela** — zero introduzidos no delta fornecido. A grade e o POST leem a oferta de `contexto_calendario`. O POST captura `agora` uma vez e passa esse instante à consulta e à porta. A porta grava a célula e a trilha. `app/templates/colaboradores/partials/_calendario_grade.html:96` · `app/colaboradores/services/calendario.py:791` · `app/ponto/views.py:3622` · `app/ponto/portas/celula.py:650` · `app/ponto/portas/celula.py:665`
- **Campo com dois significados, selo sem mordida e exceções** — zero campos ambíguos encontrados. O novo flag escolhe o cenário, e os dois campos de saída têm uma única leitura de hit-test. O RED exige a falha específica nos dois alvos. O delta introduz **0 `except` Python e 0 `catch` JavaScript**. O tratamento existente propaga a falha da sonda como `motivo`, que o positivo exige nulo. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:260` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:502` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:579` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:612`
- **Ordem de trava e pré-requisitos do pouso** — a porta entra em `atomic`, lê com `select_for_update`, aplica `so_se_folga` e então grava. A ordem operacional exigida é suíte inteira verde, revisão final e pouso, com o smoke humano antes do push. O cumprimento dessa ordem e a identidade Git do hash nesta cópia seguem [nao verificado]. A leitura usa o delta fornecido e as linhas correspondentes da cópia. `app/ponto/portas/celula.py:642` · `app/ponto/portas/celula.py:655` · `CLAUDE.md:206` · `CLAUDE.md:421` · `CLAUDE.md:427`

### 5. Lista do pouso [nao verificado]

- **360 px e `base_app` real.** Conferir página e drawer a 360, incluindo rolagem e acionabilidade. A ficha tem coluna inicial de `420px` e o drawer é limitado a `92vw`. Conferir também a entrada e o bootstrap do WebView. O ensaio existente de `base_app` usa desktop a 1366 e desativa o PWA. Resultados reais [nao verificado]. `aval_desktop.txt:1` · `app/templates/colaboradores/detalhe.html:87` · `app/templates/core/_drawer_generico.html:3` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:699` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:745`
- **Smoke humano pré-pouso.** Clique do Ronald no 20/11 real nas duas cascas, com as verificações pendentes do drawer em rolagem. Resultado [nao verificado]. `ver3_triagem.md:3` · `app/docs/LEIS.md:99` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231`
- **Lacunas da ver3.** Tab, POST com motivo pelo Chromium, paginação real da lista com drawer aberto e reabertura após restaurar histórico. Snapshot antigo com painel órfão também fica na lista. O fechamento pré-snapshot atua sobre o popover corrente. Ocorrência de snapshot antigo afetado [nao verificado]. `ver3_triagem.md:4` · `ver3_triagem.md:5` · `app/templates/colaboradores/partials/_lista_tabela.html:50` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:742` · `app/static/js/popover-acao.js:144` · `app/static/js/popover-acao.js:176`

## 28/11 00:07 · sessao 85f79d3ba089 · FERIADO-UM-JUIZ · verificador · origem=Code

**APROVADO para a c14, sem bloqueantes no delta.** O achado específico da ver9 foi curado. Fixar o total em `1` e retirar o prefixo do `ERROR` agora derrubam os testes. O terceiro mutante confirma a propagação da falha de alimentação. Os três deram RC 0 no harness, que exige controle verde, falha de todos os alvos previstos e zero erros de execução. `app/ponto/tests/feriado_um_juiz_mutantes.py:189`, `app/ponto/tests/feriado_um_juiz_mutantes.py:781`, `app/ponto/tests/feriado_um_juiz_mutantes.py:802`

### 1. Mordida dos mutantes

| Mutante | Teste e asserção que falhou | [nao verificado]
|---|---|
| `M_c14_total_fixo_1` | `test_RED_c14_resumo_publica_zero_falhas_tecnicas` e `test_RED_c14_resumo_soma_duas_falhas_tecnicas_na_mesma_passada`. Ambos caíram no `assertIn` do token inteiro em `resumos[0].split()`. Esperavam `falhas_tecnicas_oraculo=0` e `=2`, respectivamente. `app/ponto/tests/test_feriado_um_juiz.py:339`, `app/ponto/tests/test_feriado_um_juiz.py:467`, `app/ponto/tests/test_feriado_um_juiz.py:479` |
| `M_c14_sem_prefixo` | `test_RED_c10_falha_sql_da_alimentacao_reverte_o_savepoint` e `test_RED_c13_value_error_generico_na_alimentacao_e_falha_tecnica`. Ambos caíram no `assertRegex` ancorado em `^dia_pago oraculo: falha tecnica colab=1\b`. `app/ponto/tests/test_feriado_um_juiz.py:481`, `app/ponto/tests/test_feriado_um_juiz.py:484`, `app/ponto/tests/test_feriado_um_juiz.py:505` |
| `M_c14_engole_alimentacao` | `test_RED_c13_falha_na_alimentacao_propaga_quando_solicitado`. Caiu no `assertRaises(RuntimeError)` com `propagar_falha=True`. A asserção de identidade da causa fica depois e não foi alcançada. `app/ponto/tests/test_feriado_um_juiz.py:507`, `app/ponto/tests/test_feriado_um_juiz.py:510`, `app/ponto/tests/test_feriado_um_juiz.py:512` |

**Ainda há lacunas.** O incremento tem mutante c13 e o resumo agora tem prova para `0`, `1` e `2`. Em sondas adicionais em memória, o controle de 58 testes continuou verde ao fixar o ID do `ERROR` em `1`, trocar a competência do `ERROR`, silenciar o `ERROR` só na falha de alimentação com `propagar_falha=True` e publicar o resumo mesmo sem tentativa. O teste de dois colaboradores confere duas chamadas de `error`, e o teste de propagação confere a exceção e a causa. Essas lacunas ficam fora dos três defeitos reproduzidos pelos mutantes da c14. `app/ponto/tests/feriado_um_juiz_mutantes.py:226`, `app/ponto/tests/feriado_um_juiz_mutantes.py:776`, `app/ponto/tests/test_feriado_um_juiz.py:477`, `app/ponto/tests/test_feriado_um_juiz.py:500`, `app/ponto/tests/test_feriado_um_juiz.py:507`, `app/ponto/services/fechamento.py:1327`, `app/ponto/services/fechamento.py:1346`

### 2. Quem escreve e quem lê

- **Contador.** `recalcular_fechamento_mes` zera uma vez por passada, incrementa no ramo de exceção técnica da sombra e lê o acumulado como argumento do `INFO`. O resumo sai se houve tentativa e a execução chegou ao fim. A saída por exceção propagada ocorre antes do resumo. No teste, `_confere_vigia` localiza um único resumo e lê o token exato. O vigia do harness também lê o token `=1`. `app/ponto/services/fechamento.py:683`, `app/ponto/services/fechamento.py:1320`, `app/ponto/services/fechamento.py:1327`, `app/ponto/services/fechamento.py:1338`, `app/ponto/services/fechamento.py:1346`, `app/ponto/tests/test_feriado_um_juiz.py:333`, `app/ponto/tests/feriado_um_juiz_mutantes.py:173`
- **`ERROR` técnico.** O mesmo serviço emite `dia_pago oraculo: falha tecnica colab=%s %02d/%s: %s` pelo logger `ponto.services.fechamento`. A regex nova casa o início da mensagem interpolada e distingue esse formato dos formatos atuais de recusa, falha do motor e resumo. Ela aceita outros textos com o mesmo início, inclusive `colab=1-extra`, e deixa competência e resto da mensagem fora do padrão. A asserção anterior confere o motivo separadamente. Os testes de falha de escrita capturam o logger e conferem o motivo injetado. `app/ponto/services/fechamento.py:18`, `app/ponto/services/fechamento.py:1275`, `app/ponto/services/fechamento.py:1323`, `app/ponto/services/fechamento.py:1328`, `app/ponto/services/fechamento.py:1347`, `app/ponto/tests/test_feriado_um_juiz.py:504`, `app/ponto/tests/test_feriado_um_juiz.py:117`, `app/ponto/tests/test_feriado_um_juiz.py:146`, `app/ponto/tests/test_feriado_um_juiz_porta.py:497`
- **Comandos, crons e vigias.** A tela e `recalcular_fechamento` leem `processados`. O backfill conta colaboradores do dicionário retornado. `diff_calculador` lê rubricas diretamente. O cron `pre_fechamento` lê bloqueios e marcos. No SaaS, `pontofile` recebe `INFO` e `ERROR`, e o console recebe `ERROR`. O formatador põe data, nível e nome do logger antes da mensagem, então a regex nova, com `^`, atua sobre o corpo capturado no teste. ESTADO e HAIKU leem `ponto.log` por um parser que exige `ponto.recusa_critica RECUSA-ACAO-CRITICA`. Leitor automatizado da telemetria técnica em produção **[nao verificado]**. `app/ponto/views.py:1280`, `app/ponto/management/commands/recalcular_fechamento.py:130`, `app/ponto/management/commands/lavrar_dias_pagos.py:106`, `app/ponto/management/commands/diff_calculador.py:406`, `app/config/crons.py:406`, `app/folha/management/commands/pre_fechamento.py:46`, `app/folha/management/commands/pre_fechamento.py:74`, `app/config/settings/saas.py:122`, `app/config/settings/saas.py:160`, `app/config/settings/saas.py:181`, `bin/gerar_estado.py:265`, `bin/recusa_estado.py:45`, `bin/recusa_estado.py:68`, `app/api/views_mensageria.py:1872`, `app/core/recusa_critica.py:46`

### 3. Isolamento e execução

Os dois testes passaram isolados e nas duas ordens. Cada cenário cria seus próprios mocks e batidas, fixa `timezone.now()` em 05/11/2026 às 12h e captura diretamente o logger do fechamento. O teste de duas falhas exige a ordem da lista que ele mesmo injeta. O delta acrescenta **0 `try` e 0 `except`**. `app/ponto/tests/test_feriado_um_juiz.py:161`, `app/ponto/tests/test_feriado_um_juiz.py:244`, `app/ponto/tests/test_feriado_um_juiz.py:256`, `app/ponto/tests/test_feriado_um_juiz.py:286`, `app/ponto/tests/test_feriado_um_juiz.py:309`, `app/ponto/tests/test_feriado_um_juiz.py:462`, `app/ponto/tests/feriado_um_juiz_mutantes.py:781`

Rodei os três módulos localmente com o harness em memória e substitutos de `SimpleTestCase`, `DiaPago` e `timezone.make_aware`. Resultado **102 testes, OK, uma falha esperada**, na borda 20/21 com troca de 6x1 para 12x36. Os dois blobs de teste conferem com o anexo. Identidade do commit inteiro, execução da pista, ORM com banco e logs de produção **[nao verificado]**. `app/ponto/tests/feriado_um_juiz_mutantes.py:502`, `app/ponto/tests/feriado_um_juiz_mutantes.py:519`, `app/ponto/tests/test_feriado_um_juiz.py:986`

### 4. Demais achados da ver9

- **Saída antecipada.** Permanece o cenário de `06:00 E / 06:30 S`: mensal e motor preservam zero trabalhado e 420 min de saída antecipada, e a sombra conserva a lavra anterior por recusa de procedência. O descarte do bloqueio 2 é o informado no pedido. A L-095 atribui ao motor a lavra do dinheiro do dia. `app/ponto/tests/test_feriado_um_juiz.py:322`, `app/ponto/tests/test_feriado_um_juiz.py:386`, `app/ponto/services/dia_pago.py:321`, `app/docs/LEIS.md:113`
- **C13, BUG-144 e O116.** A classificação por `RecusaDeProcedencia`, os 357 min preservados, a guarda por dia e os três tipos de FT continuam cobertos. A falha técnica que chega ao fechamento continua propagando na revisão, cujo `atomic` externo desfaz o ato. A captura interna da fábrica de motor continua podendo absorver uma exceção antes desse tratador. `app/ponto/tests/test_feriado_um_juiz.py:341`, `app/ponto/tests/test_feriado_um_juiz.py:396`, `app/ponto/tests/test_feriado_um_juiz.py:419`, `app/ponto/services/fechamento.py:1320`, `app/ponto/services/fechamento.py:1893`, `app/ponto/services/par_relampago_na_lista.py:479`, `app/ponto/calculador/alimentacao.py:429`
- **C12.** A catraca de linhas passou. Duas ocorrências na mesma linha continuam virando uma entrada, pelo `set` do censo. `app/core/tests/test_medida_turno_do_par.py:565`, `app/core/medida_turno_do_par.py:224`, `app/core/medida_turno_do_par.py:228`

### 5. Classes de reprovação

- **Segundo juiz e segundo escritor.** As duas versões continuam pela mesma `lavrar`, com a partição existente. O retrato da exportada é uma porta preexistente. `app/ponto/services/fechamento.py:997`, `app/ponto/services/fechamento.py:1236`, `app/ponto/services/fechamento.py:1296`, `app/ponto/services/fechamento.py:1300`, `app/ponto/services/dia_pago.py:370`, `app/ponto/services/dia_pago.py:417`
- **Relógio e ordem de trava.** O teto do furo recebe o instante da borda da passada e a lista resultante vai às duas lavras. A revisão mantém Empresa por PK → Colaborador → Batida por PK, antes do recálculo. `app/ponto/services/fechamento.py:680`, `app/ponto/services/fechamento.py:1154`, `app/ponto/services/fechamento.py:1262`, `app/ponto/services/fechamento.py:1316`, `app/ponto/services/par_relampago_na_lista.py:172`, `app/ponto/services/par_relampago_na_lista.py:483`
- **Derivação paralela e campo com dois significados.** Persistem as divergências anteriores. Períodos e FT usam o mapa da jornada quando fornecido, enquanto o atraso de ligação usa a data civil de entrada. `nao_decide` sinaliza rubrica sem afirmação no DIFF e também satisfaz a declaração de dono na guarda. Efeito em produção **[nao verificado]**. `app/ponto/services/dia_pago.py:62`, `app/ponto/services/dia_pago.py:109`, `app/ponto/services/dia_pago.py:228`, `app/ponto/services/credito_parcial.py:73`, `app/ponto/management/commands/diff_calculador.py:508`
- **Selo sem mordida.** Os três mutantes novos morreram nas asserções indicadas, com as lacunas remanescentes registradas no item 1. `app/ponto/tests/feriado_um_juiz_mutantes.py:189`, `app/ponto/tests/feriado_um_juiz_mutantes.py:802`

## 28/11 00:00 · sessao 6e7123508798 · FIM-DE-SEMANA-JA · leitura · origem=Code

**FIM-DE-SEMANA-JA.** O recorte traz sete pares em cinco disputas fechadas e uma cobrança indevida no chamado 38343. A cópia local não tem metadados Git para confirmar o hash `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Identidade com o HEAD informado, estado atual dos casos e resultado de qualquer operação em produção [nao verificado]. `lista_por_par.md:3–8` `S2_r.md:14`

## 1. Quem decide

`ChamadoColaborador._decisao_do_fechamento` é a autoridade das duas recusas. `reconciliar()` executa a decisão e `reconciliaria()` consulta a mesma decisão em seco. A ordem é motor de estados, premissa do furo, conversa do pai e pergunta viva. Assim, `dia_furado_para_a_folha` pode ser a primeira recusa mesmo quando também há pergunta viva. `app/chamados/models.py:661–715`

- **`pergunta_viva_na_competencia_aberta`.** O gate busca perguntas sem validação ancoradas no chamado. A âncora é `chamado_destino`, o filho, ou `disputa.chamado` quando falta destino. Ele filtra pela competência do dia e consulta o C6 com o **estado que o filho teria após fechar**. O C6 lê a sequência de decisões, a resposta, a tranca, a célula e o lastro. Em um ramo do lastro, lê também o estado do filho. `fechada_em` da disputa, por si só, não encerra a pergunta. Se todas as vivas podem ser reatadas a outra cobrança legítima do mesmo colab-dia, o próprio gate reaponta e grava a trilha. Se alguma não pode, recusa. `app/chamados/models.py:349–414` `app/chamados/models.py:429–464` `app/chamados/juizes.py:835` `app/chamados/juizes.py:1499–1589`
- **`dia_furado_para_a_folha`.** O gate lê o chamado de furo e seu dia forte, depois `premissa_regularizada(para='folha')`. Entram justificativa aprovada, célula, veredito do admin, ata e lâmpadas dos marcos, cobertura de ausência e os fatos de jornada quando a célula não responde. Também aceita agrupamento sob pai vivo ou deduplicação se outra cobrança for a sobrevivente do C8. O gate de conversa do pai é separado e protege respostas sem veredito. `app/chamados/models.py:718–739` `app/chamados/models.py:783–837` `app/chamados/models.py:1318–1382` `app/chamados/juizes.py:508–569`
- **Retenção da folha.** `classificar_export` consulta `motivos_retencao_celula` pelo espelho. O leitor usa a lavra e os protestos da célula, com as exceções previstas. A S4 mede pares do C2 no universo de chamados vivos. A prova de saída da retenção exige conferir o dia na Folha além de conferir o par na S4. `app/folha/export.py:977–982` `app/folha/export.py:1282–1316` `app/folha/fora_da_folha.py:125–165` `S4_r.md:1`

## 2. Portas existentes e seus efeitos

| Porta | Quem aciona, trilha e repetição | Batida, célula, escala/vínculo e dinheiro gravado |
|---|---|---|
| **Responder à pergunta**, no app ou PWA | O próprio colaborador autenticado. O POST confere dono e C6 mesmo com a disputa fechada. Grava `RESPOSTA` com usuário, texto e instante, preserva os atos anteriores e registra o motivo da pergunta no log. O app também registra o resultado do POST. Reenvio pode gravar outro ato. Uma resposta de hora pode seguir viva até o veredito. `app/api/views.py:2607–2664` `app/chamados/views.py:1228–1279` `app/chamados/services/disputa_emissao.py:1529–1611` `app/chamados/juizes.py:1521–1533` | **Batida não. Célula sim**, se a resposta gerar ausência ou disparar a reconciliação do dia. **Escala/vínculo não. Dinheiro gravado não diretamente**. Falta ou atestado pode alterar o insumo da folha e invalidar sua projeção. `app/chamados/services/disputa_emissao.py:1591–1596` `app/chamados/services/materializacao.py:1010–1046` `app/ponto/signals.py:148–152` `app/ponto/services/ausencia.py:581–597` |
| **Validar** no Fio do admin, no cartão do filho | Gestor autenticado com permissão contextual no chamado. O Fio checa a pergunta exata, a âncora e o C6 sob trava, e passa `sem_reabrir=True` para a fechada. Exige resposta com texto e instante. Grava `VALIDACAO` com usuário, instante e texto, além de `val_perg_disp` e `validar_viva_em_disputa_fechada` quando o ato fica. A sequência preserva os atos anteriores. Já validada é recusada sem regravar. `app/chamados/views.py:24–38` `app/chamados/views.py:869–875` `app/chamados/utils.py:47–72` `app/chamados/services/fio.py:694–768` `app/chamados/services/validacao.py:164–178` `app/chamados/services/validacao.py:755–765` `app/chamados/services/validacao.py:825–843` | **Batida sim, pode plantar. Célula sim. Escala/vínculo não** no sentido de editar o vínculo ou o tipo de escala, mas o aceite de folga pode gravar `FolgaDia`. **Dinheiro gravado sim**, pode recalcular e persistir `FechamentoMensal`. O materializador pode alcançar irmãs já validadas. `app/chamados/services/validacao.py:587–602` `app/chamados/services/validacao.py:777–830` `app/chamados/services/materializacao.py:2040–2061` `app/chamados/services/materializacao.py:2240–2301` `app/ponto/services/fechamento.py:1184–1192` |
| **Resolver dia**, no calendário, para um colab-dia | Admin com `regularizar_batida`. O botão exige dia passado, estado elegível e condição de último recurso. O serviço seleciona as perguntas de fato vivas do dia, inclusive de disputa fechada. Grava `admin_decidiu`, `SILENCIO` com autor, instante e motivo, e a lavra com antes/depois. O carimbo e a lavra iguais são idempotentes. Um novo POST aceito pode repetir o log do despachante. `app/ponto/views.py:3517–3556` `app/colaboradores/services/calendario.py:94–122` `app/colaboradores/services/calendario.py:493–508` `app/templates/colaboradores/partials/_calendario_grade.html:134` `app/ponto/services/veredito_celula.py:176–212` `app/ponto/services/veredito_celula.py:351–363` `app/chamados/models.py:3372–3386` `app/ponto/portas/celula.py:1113–1141` | **Batida sim** em `trabalhou`, se houver correção ou plantio. **Batida não** em `faltou` ou `justificado`, que podem criar ausência aprovada. **Célula sim. Escala/vínculo não. Dinheiro gravado sim**, se houver plantio com recálculo. A ausência pode invalidar a projeção, com efeito nos valores quando rederivados. `app/ponto/services/veredito_celula.py:317–357` `app/ponto/registro_batida.py:176–198` `app/ponto/services/ausencia.py:154–195` `app/ponto/services/ausencia.py:597` `app/ponto/services/fechamento.py:1822–1848` |
| **Encerrar** (`resolvido`) no chamado local | Gestor autenticado com permissão contextual. No detalhe local, o chamado precisa estar sem GLPI. A mensagem com motivo fica no fio com o usuário. `estado_chamado` grava antes/depois e via `admin`. A transição exige premissa regularizada para chamado de furo e recusa pergunta viva ancorada ou disputa própria aberta que segure. Repetir o status é recusado, mas a mensagem já terá sido gravada e pode duplicar. `app/chamados/views.py:358–370` `app/chamados/views.py:418–421` `app/chamados/services/verbos.py:132–136` `app/chamados/services/acoes_chamado.py:292–331` `app/chamados/models.py:140–211` `app/chamados/models.py:304–308` `app/chamados/catalogo/motor.py:148–149` | **Batida não diretamente. Célula sim**, pelo rejulgamento disparado pela mudança de estado. **Escala/vínculo não. Dinheiro gravado não diretamente.** Se a cascata fechar outra disputa aberta, pode retomar regularização já confirmada e alcançar batida e recálculo. Esse efeito em 38343 [nao verificado]. `app/chamados/signals.py:91–118` `app/chamados/signals.py:123–146` `app/chamados/signals.py:189–218` `app/chamados/services/disputa_emissao.py:2221–2225` `app/chamados/services/regularizacao_ext.py:77–78` `app/ponto/registro_batida.py:176–198` |
| **`supra_juiz --executar`**, retratação por lavra | Processo autorizado a rodar `manage.py` no tenant. Exige lavra `cobranca_indevida` com código, preserva conversa sem veredito, exclui a órfã de regeneração e passa pela retratação canônica e pelos gates de fechamento. Grava `estado_chamado` antes/depois com ator de processo `sistema:supra_juiz` e nota `retratado` com o motivo. Já encerrado sai da seleção de vivos. A unidade pode rejeitar justificativa vinculada. O menor recorte disponível é empresa e data, sem filtro de chamado ou colab. `app/ponto/management/commands/supra_juiz.py:61–74` `app/ponto/management/commands/supra_juiz.py:110` `app/ponto/management/commands/supra_juiz.py:246–295` `app/chamados/reconciliador.py:558–620` `app/core/observ.py:22–32` | **Batida não diretamente. Célula sim**, pelo cartório. **Escala/vínculo não. Dinheiro gravado não diretamente.** Vale a mesma ressalva de cascata do fechamento do chamado. `app/chamados/reconciliador.py:602–620` `app/chamados/signals.py:123–146` `app/chamados/signals.py:189–218` |

Em **Resolver dia**, a tentativa de reconciliar os chamados vem antes da lavra `admin`. A premissa pode recusar nessa primeira tentativa. A lavra posterior faz o coletor da premissa aceitar a decisão humana, mas o fechamento do filho ainda precisa ser comprovado. A fila de eventos da célula volta a tentar os casos elegíveis. `app/ponto/services/veredito_celula.py:207–212` `app/ponto/services/veredito_celula.py:351–357` `app/chamados/models.py:1345–1349` `app/ponto/portas/celula.py:1130–1135` `app/chamados/reconciliador.py:1513–1533`

## 3. Pergunta viva em disputa fechada

**Há portas sem reabrir.** O colaborador pode responder pelo POST autenticado, o gestor pode validar uma resposta no Fio, e **Resolver dia** pode encerrar uma pergunta de fato com decisão comprovada do dia. O app põe as vivas de disputas fechadas no histórico, e o Fio web corrente do colaborador carrega as abertas. O teste da API comprova o aceite do POST de uma pergunta do histórico. A existência de um botão alcançável e de sessão utilizável para estes casos [nao verificado]. `app/api/tests/test_hx_api_estado.py:54–65` `app/chamados/services/pergunta_no_app.py:80–92` `app/chamados/services/fio.py:287–291` `app/chamados/services/fio.py:721–768` `app/ponto/services/veredito_celula.py:176–202`

**SEM PORTA de expiração avulsa da muda.** A expiração por silêncio saiu do comando de arquivamento. `reconciliar_perguntas_orfas` é vigia, e o escritor por lâmpada seleciona disputas abertas. `admin_encerra` é método sem ação de tela localizada e, para disputa já fechada, encontra a guarda de fechamento antes de carimbar a pergunta. `app/chamados/management/commands/arquivar_competencia_encerrada.py:61–64` `app/ponto/management/commands/reconciliar_perguntas_orfas.py:1–8` `app/chamados/reconciliador.py:1166–1173` `app/chamados/models.py:2094–2102` `app/chamados/models.py:2210–2217` `app/docs/BACKLOG.md:1635`

A **Reabertura administrativa** existe no cartão do pai. Staff ou `admin_ti`, com permissão contextual, escolhe motivo do catálogo e escreve justificativa de pelo menos 20 caracteres. A porta escolhe a fechada mais recente daquele pai, consome uma das duas reaberturas, cria nova pergunta, pode reabrir o chamado pai e grava nota assinada e log com motivo e contador. A nota guarda a justificativa. O log de reabertura não traz campos estruturados de antes/depois. Repetir o POST pode selecionar outra fechada do mesmo pai. O ato direto tem **batida não, célula possível pelo sinal do chamado, escala/vínculo não, dinheiro gravado não**. O refechamento comum continua sujeito ao C7, que recusa viva ou resposta sem veredito. **Reabrir e refechar não é uma saída para dar baixa na muda.** `app/templates/chamados/partials/modal_fio.html:418–443` `app/chamados/services/fio.py:694–719` `app/chamados/services/acoes_disputa.py:274–339` `app/chamados/models.py:2396–2431` `app/chamados/models.py:2122–2145` `app/chamados/signals.py:189–218`

## 4. Roteiro para as oito ocorrências

| Ocorrência | Porta recomendada |
|---|---|
| **col296 · 10/11 · 38343** | **Encerrar**, no chamado exato, pelo gestor autorizado, com o motivo factual da cobrança indevida. Confirmar primeiro que o seletor está disponível e que as guardas permitem `resolvido`. **Validar** trata uma resposta de pergunta. **Cancelar** é evento de sistema e o seletor humano o recusa. Se **Encerrar** recusar, a baixa desse PK fica **SEM PORTA comprovada no recorte**. O `supra_juiz --executar` tem alcance sobre outros chamados da empresa no dia. `S2_r.md:14` `app/templates/chamados/detalhe_local.html:374–405` `app/chamados/models.py:160–211` `app/chamados/views_cobrar.py:160–172` `app/chamados/catalogo/motor.py:139–149` `app/ponto/management/commands/supra_juiz.py:61–74` |
| **col719 · 01/11 · 7751 → 36403** | **Responder**, pelo próprio col719, se a pergunta exata estiver alcançável no canal existente. A S4 indica que falta a resposta. **SEM PORTA** de baixa administrativa avulsa com a evidência fornecida. Se a resposta continuar viva aguardando veredito, o gestor usa **Validar** no mesmo filho. `lista_por_par.md:4` `app/api/tests/test_hx_api_estado.py:54–65` `app/chamados/services/fio.py:721–768` `app/chamados/services/validacao.py:755–758` |
| **col719 · 03/11 · 7817 → 36796** | **Responder**, nas mesmas condições, para a pergunta deste filho. Alcance real [nao verificado]. `lista_por_par.md:5` `app/chamados/views.py:1228–1279` |
| **col719 · 05/11 · 7888 → 37210** | **Responder**, nas mesmas condições, para a pergunta deste filho. Alcance real [nao verificado]. `lista_por_par.md:6` `app/chamados/views.py:1228–1279` |
| **col935 · 18/11 · 8272 → 40002** | **Resolver dia**, individual, pelo admin com `regularizar_batida`, se a UI oferecer o dia e houver evidência para o veredito. **SEM PORTA** para baixar só a pergunta e liberar um dia cuja premissa ainda acusa. Oferta e veredito cabível [nao verificado]. `lista_por_par.md:7` `app/colaboradores/services/calendario.py:94–122` `app/ponto/views.py:3517–3546` `app/chamados/models.py:793–837` |
| **col976 · 22/10 · 7385 → 34061 · três pares** | **Um Resolver dia** para 22/10, sob as mesmas condições. Conferir o conjunto de perguntas de fato vivas desse dia antes do ato, pois a porta opera no colab-dia. **SEM PORTA** para baixar os três pares mantendo uma premissa de furo que ainda acusa. IDs das três perguntas, oferta e veredito cabível [nao verificado]. `lista_por_par.md:8` `app/ponto/services/veredito_celula.py:176–212` `app/chamados/models.py:793–837` |

1. **Antes dos atos**, obter a foto nominal em Sombra atualizada, empresa 2, competência 11. Registrar PK da pergunta, destino, C6, resposta válida ou não, `reconciliaria()` e lavra. Em seguida, tratar 38343 pelo chamado exato, os três dias de col719 pela resposta e os dois dias com recusa de premissa pelo calendário, conforme as condições da tabela. Essa ordem é operacional. A decisão do gate é por chamado e dia. `S4_r.md:1–3` `app/chamados/models.py:677–715` `app/chamados/models.py:793–837`
2. **Depois de 38343**, conferir a transição com motivo no fio e a célula de 10/11. A mudança de status dispara o cartório em melhor esforço. **O col296 sai da marca `cobranca_indevida` da S2 quando a lavra deixar de trazer essa marca.** `COBRANCA_SEM_FURO` exige cobrança viva que ainda protesta, dia de trabalho, realizado suficiente e todas as células casadas. O encerramento de 38343 pode retirar esse insumo. Outra cobrança, outra retenção ou falha de rejulgamento no caso concreto [nao verificado]. `S2_r.md:14` `app/chamados/signals.py:189–218` `app/ponto/services/cartorio.py:1498–1508` `app/ponto/services/cartorio.py:1905–1913` `app/ponto/supra_juiz.py:476–484` `app/escala/models.py:1624` `app/folha/export.py:1282–1305`
3. **Repetir S4 e S2 na foto posterior.** Para os sete pares, usar o coletor **`S4|C2` embutido em `S4_r.md`**, que requer o fonte original de `sonda_S1.py`. O bloco colado com o nome `s4_sonda.py` imprime **`S3|C1`** e serve para detalhar dias. Comparar por `(chamado, pergunta)` e conferir o ato gravado, o C6 e o estado do filho. Para a S2, repetir a coleta S1 que sustenta seu C1 e conferir a lavra de 10/11 do col296. Por fim, confirmar o dia em `dias_que_seguram` e a linha do colaborador em `classificar_export`. Disponibilidade do fonte original, nova coleta e efeito final no TXT [nao verificado]. `S4_r.md:1–3` `S2_r.md:7–14` `app/chamados/models.py:677–681` `app/folha/fora_da_folha.py:125–165` `app/folha/export.py:977–989`

## 27/11 23:59 · sessao f5b9cdfcd0a7 · - · - · origem=Ronald (tmux)

Para o **col296, chamado 38343**, a menor porta nominativa é o detalhe do chamado local. Um gestor autenticado da área registra o motivo no campo de resposta e seleciona **Encerrar** (`resolvido`). A ação exige chamado local sem GLPI e, para chamado de furo, exige premissa regularizada. Recusa se houver disputa própria aberta com guarda ou pergunta viva ancorada. Status exato, vínculo com GLPI, permissões efetivas e resultado para o #38343 ficam [nao verificado]. `app/chamados/views.py:24,35–38,358–370,418–421,547–552`, `app/chamados/utils.py:47–72`, `app/templates/chamados/detalhe_local.html:374–405`, `app/chamados/services/verbos.py:132–136`, `app/chamados/models.py:140–211`.

A trilha é a nota com motivo e usuário, os logs `responder`/`status` e `estado_chamado` com antes/depois, via `admin` e autor. Repetir o status é recusado, mas a nota precede a transição e pode duplicar. Efeitos diretos do encerramento e do rejulgamento usual: batida **não**, célula **sim** pelo cartório, escala/vínculo **não**, dinheiro gravado **não**. Existe cascata condicional se o S143 fechar outra disputa e ela destravar regularização externa confirmada, com possível alteração em batida e recálculo. Incidência em #38343 [nao verificado]. `app/chamados/services/acoes_chamado.py:292–331`, `app/chamados/models.py:304–308,466–469`, `app/chamados/catalogo/motor.py:148–149`, `app/chamados/signals.py:112–146,189–218`, `app/chamados/services/disputa_emissao.py:2206–2225`, `app/chamados/services/regularizacao_ext.py:170–226`, `app/ponto/registro_batida.py:176–198`.

`validar_inline` valida perguntas vivas respondidas em disputa aberta. `cancelado` é rejeitado na transição humana, mesmo para `admin_ti`; o `cancelar()` interno tem chamadores de comandos de revisão de vínculo/régua. O command `supra_juiz --executar` é a alternativa específica de sistema para lavra `cobranca_indevida`, mas o menor recorte por flags é empresa × dia. Ele preserva conversa sem veredito, cobrança órfã de regeneração e as guardas da retratação/reconciliação. Assina como `sistema:supra_juiz`, com `estado_chamado` antes/depois e nota `retratado` com motivo. O fechamento idempotente só atua em vivos e a retratação pode rejeitar justificativa pendente vinculada. `app/chamados/views_cobrar.py:145–172`, `app/chamados/catalogo/motor.py:139–156`, `app/chamados/models.py:839–858`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:48`, `app/ponto/management/commands/supra_juiz.py:61–67,110–118,190–212,246–295`, `app/chamados/reconciliador.py:558–620`, `app/core/observ.py:22–32,94–137`.

**S2:** a baixa faz diferença quando o cartório relavra a célula. O status do chamado integra a impressão e `COBRANCA_SEM_FURO` exige chamado vivo entre os que ainda protestam. Após o ato, conferir o #38343 fora de `VIVOS`, a lavra de 10/11 sem essa marca e o dia ausente de `dias_que_seguram`. Outras retenções do col296 e efeito no TXT [nao verificado]. `app/ponto/services/cartorio.py:490–504,1432–1449,1498–1508,1905–1932`, `app/ponto/supra_juiz.py:476–484`, `app/escala/models.py:1624,1780`, `app/folha/export.py:1282–1303`, `app/folha/fora_da_folha.py:115–165`.

Para órfãs de disputa fechada, a **validação no fio** admite uma viva fechada sem reabertura, desde que respondida. Os endpoints de resposta aceitam dona/dono + C6 viva sem exigir disputa aberta, mas a UI web exclui as fechadas e a consulta do app coloca a muda de disputa fechada no histórico. `Resolver dia`, quando oferecido e respaldado pelos fatos, pega perguntas não conversacionais de colab/dia sem filtrar disputa aberta e as carimba com autor e `SILENCIO`; pode mexer nas batidas e decide o dia inteiro. O `reconciliar` dos filhos vem antes da lavra administrativa, então verificar depois o chamado e a retenção independentemente do C6. `app/chamados/services/fio.py:287–291,721–769`, `app/chamados/services/validacao.py:755–758`, `app/chamados/views.py:1228–1279`, `app/api/views.py:2607–2658`, `app/chamados/services/pergunta_no_app.py:68–92`, `app/ponto/services/veredito_celula.py:180–212,317–363`, `app/colaboradores/services/calendario.py:94–104`.

O método `admin_encerra` exige motivo e só atende silêncio, com trava para resposta sem veredito. A ausência de tela para ele está documentada. Reabrir pela UI acrescenta pergunta de conversa, e o fechamento ordinário volta a recusar C6 viva. `app/chamados/models.py:2122–2146,2184–2217`, `app/docs/BACKLOG.md:1635`, `app/chamados/services/acoes_disputa.py:274–339`.

## 27/11 23:59 · sessao ff179defc550 · - · - · origem=Ronald (tmux)

Para col935, disputa 8272, filho 40002 em 18/11, e col976, disputa 7385, filho 34061 em 22/10, os quatro pares têm `dia_furado_para_a_folha`. As perguntas individuais, o fato do dia e o módulo dos filhos estão [nao verificado]. `lista_por_par.md:9–10` do anexo.

- **Quem decide.** `ChamadoColaborador._decisao_do_fechamento` pergunta ao motor `RECONCILIAR`, depois à premissa, ao CSF e à pergunta viva. `_gate_premissa_do_fechamento` consulta o dia forte do filho, `premissa_regularizada(para='folha')`, pai agrupador vivo e o sobrevivente C8. O coletor lê justificativa aprovada, lavra administrativa, dia sem trabalho, lâmpadas e aba de faltas da célula. O TXT tem leitor próprio da lavra e dos protestos. `app/chamados/models.py:683–712,783–837,1332–1382`, `app/chamados/juizes.py:508–567`, `app/folha/export.py:1185–1222,1282–1305`.
- **Porta por dia, quando houver fato confirmado.** Admin com `colaboradores.rbac_regularizar_batida` ou superusuário usa **Resolver dia** no calendário individual. O botão exige dia passado de competência anterior e status elegível, e a tela considera se existe pergunta C6-viva com resposta. O serviço seleciona perguntas de fato C6-vivas do mesmo colaborador e dia, inclusive em disputa fechada, grava via `admin_decidiu` e ato `SILENCIO` com autor, instante, texto anterior e razão fixa. Registra ações na trilha do despachante e antes/depois na lavra. `app/colaboradores/mixins.py:322–331`, `app/colaboradores/services/calendario.py:94–104,117–124,493–508,774`, `app/templates/colaboradores/partials/_calendario_grade.html:134–135`, `app/ponto/views.py:3517–3556`, `app/ponto/services/veredito_celula.py:180–213,351–364`, `app/ponto/portas/celula.py:1082–1083,1130–1141`.
- **Efeitos e reaplicação.** `Trabalhou` pode inverter ou materializar batida e recalcular o dinheiro gravado após commit. `Faltou` e `Justificado` criam ausência, com batida **não**, dinheiro gravado **não** no ato e possível invalidação da projeção. Célula **sim**, se existente, e escala/vínculo **não** nesses três ramos. Carimbo individual e lavra repetida são idempotentes. O despachante registra outra trilha quando repetido com sucesso e a sobreposição de ausência pode recusar. `app/ponto/services/veredito_celula.py:216–218,317–364`, `app/ponto/registro_batida.py:174–198`, `app/ponto/services/ausencia.py:465–471,581–599`, `app/ponto/services/fechamento.py:1640–1684,1822–1848`, `app/chamados/models.py:3372–3386`, `app/ponto/portas/celula.py:1113–1116`.
- **Resultado garantido pelo código sob condição.** A lavra via `admin` libera a cláusula de premissa e os três vereditos administrativos ficam fora de `VEREDITOS_ACUSAM`. O despachante tenta reconciliar antes da lavra, então o filho ainda pode recusar nessa tentativa. A lavra emite evento e o consumidor reconcilia os módulos elegíveis, com outras guardas ainda aplicáveis. A disponibilidade do botão, a escolha do veredito e a baixa desses filhos estão [nao verificado]. Sem veredito factual confirmado, **SEM PORTA** para baixar só a pergunta e eliminar `dia_furado_para_a_folha`. `app/chamados/models.py:1345–1349`, `app/escala/models.py:1772–1781`, `app/ponto/services/veredito_celula.py:204–212,351–357`, `app/ponto/portas/celula.py:977–1002,1130–1135`, `app/chamados/reconciliador.py:1118–1131,1513–1533`.
- **Resposta sem reabrir.** POST web/API confere dono autenticado e C6, inclusive para disputa fechada. O Meu Atendimento web exclui fechadas da montagem, e o app envia fechadas ao histórico, fora de pendentes. O admin pode validar uma resposta de fechada no fio sob a âncora correta, com `sem_reabrir=True`. `Regularizar disputa` exige disputa aberta e recusa pergunta muda. `admin_encerra` exige motivo mas carece de chamador na UI, e `fechar` retorna cedo ou levanta quando a disputa já está fechada. `app/chamados/views.py:466–475,560–570,1228–1247,1275–1279`, `app/api/views.py:2607–2644,2652–2664`, `app/chamados/services/pergunta_no_app.py:80–92`, `app/chamados/services/fio.py:287–309,694–709,721–768`, `app/chamados/services/validacao.py:755–765`, `app/chamados/services/acoes_disputa.py:156–178`, `app/chamados/models.py:2094–2100,2210–2217`, `app/docs/BACKLOG.md:1635`.
- **Comandos.** `reconciliar_perguntas_orfas` só mede e seu escritor real seleciona disputa aberta e dois motivos. `processar_cartorio --apply` rejulga por empresa em todas as datas anteriores, sem filtro dos IDs. `consumir_eventos_celula` é consumidor de eventos no tenant inteiro. `app/ponto/management/commands/reconciliar_perguntas_orfas.py:20–36`, `app/chamados/reconciliador.py:1162–1175`, `app/ponto/management/commands/processar_cartorio.py:25–29,45–65`, `app/chamados/management/commands/consumir_eventos_celula.py:13–35`.

## 27/11 23:58 · sessao 3f74ace4fec9 · FERIADO-UM-JUIZ · construtor · origem=Code

**c14 entregue** com alterações apenas nos dois arquivos de teste autorizados. [nao verificado]

- `test_feriado_um_juiz.py` (`+27 / −6`) exige totais exatos de `0` e `2`. O segundo caso injeta a falha de alimentação em dois colaboradores na mesma passada, com `propagar_falha=False`. O ERROR técnico exige o prefixo literal `dia_pago oraculo:` e o ID do colaborador. [Testes do resumo](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:462) · [Asserção do ERROR](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:505) [nao verificado]
- `feriado_um_juiz_mutantes.py` (`+40 / −9`) registra os três mutantes e exige que falhem exatamente os testes listados. **Diff com `0` `except` e `0` `try` adicionados ou removidos.** [Registro dos mutantes](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:189) · [Verificação dos alvos](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:802) [nao verificado]

| Mutante | Testes que falharam | [nao verificado]
|---|---|
| `M_c14_total_fixo_1` | `test_RED_c14_resumo_publica_zero_falhas_tecnicas` e `test_RED_c14_resumo_soma_duas_falhas_tecnicas_na_mesma_passada`. Ambos receberam `1`. [Alvos](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:462) |
| `M_c14_sem_prefixo` | `test_RED_c10_falha_sql_da_alimentacao_reverte_o_savepoint` e `test_RED_c13_value_error_generico_na_alimentacao_e_falha_tecnica`. Ambos falharam na asserção do prefixo. [Alvos](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:481) |
| `M_c14_engole_alimentacao` | `test_RED_c13_falha_na_alimentacao_propaga_quando_solicitado`. O `RuntimeError` esperado não foi lançado. [Alvo](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:507) |

Os três comandos retornaram `0` após as falhas previstas. O controle local sem banco rodou 58 testes, com 57 aprovados e uma falha esperada. [Executor](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:776) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M_c14_total_fixo_1
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M_c14_sem_prefixo
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M_c14_engole_alimentacao
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes controle
```

Certificação pelo Code e pelo Opus final [nao verificado].

## 27/11 23:58 · sessao b0324e92ec8c · - · - · origem=Ronald (tmux)

A pergunta viva de disputa fechada tem duas saídas sem reabrir. O colaborador autenticado consegue responder pelo POST próprio do PWA ou do app, desde que o C6 ainda a considere viva. O app a coloca no histórico, e o fio web do colaborador exclui as fechadas. O teste confirma que o POST aceita a pergunta do histórico. O alcance por uma tela e uma credencial reais nos cinco casos fica [nao verificado]. O registro `DecisaoPergunta.RESPOSTA` guarda autor, texto e instante, preservando os atos anteriores. Novo envio pode gravar outra resposta. Uma resposta de hora normalmente passa à validação e pode seguir viva. `FALTA_DIA` e `ATESTADO` podem criar ausência e repercutir em célula e fechamento. A resposta por si não planta batida nem altera vínculo. `app/chamados/views.py:1228`, `app/chamados/views.py:1275`, `app/api/views.py:2607`, `app/api/tests/test_hx_api_estado.py:54`, `app/chamados/services/pergunta_no_app.py:80`, `app/chamados/services/fio.py:287`, `app/chamados/services/disputa_emissao.py:1530`, `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1591`, `app/chamados/services/materializacao.py:1010`, `app/chamados/juizes.py:1521`.

O Fio do gestor aceita **Validar** por pergunta respondida, no cartão do filho que a lista, com verificação de permissão contextual e do C6 sob trava. Passa `sem_reabrir=True`; sem resposta íntegra retorna `sem_resposta`. A UI condiciona o formulário à resposta e ao estado de aplicação. A validação persistida é idempotente por estado, grava autor, instante e texto em `DecisaoPergunta.VALIDACAO`, `val_perg_disp` e o log específico `validar_viva_em_disputa_fechada`. O motivo da pergunta aparece no log e a eventual devolução tem antes/depois. Pode plantar batida, repercutir na célula, lançar FolgaDia no aceite de folga e recalcular valores gravados em `FechamentoMensal`. O laço pode alcançar irmãs validadas. `app/chamados/views.py:869`, `app/chamados/views.py:952`, `app/chamados/utils.py:47`, `app/chamados/services/fio.py:694`, `app/chamados/services/fio.py:721`, `app/templates/chamados/partials/modal_fio.html:217`, `app/templates/chamados/partials/modal_fio.html:273`, `app/chamados/services/validacao.py:164`, `app/chamados/services/validacao.py:332`, `app/chamados/services/validacao.py:755`, `app/chamados/services/validacao.py:777`, `app/chamados/services/validacao.py:796`, `app/chamados/services/validacao.py:839`, `app/chamados/services/validacao.py:587`, `app/chamados/services/materializacao.py:2240`, `app/chamados/services/materializacao.py:2273`, `app/ponto/services/fechamento.py:1161`, `app/ponto/services/fechamento.py:1822`.

**Resolver dia** pode encerrar a pergunta muda sem abrir a disputa quando há um veredito factual para o dia. Seleciona perguntas de fato, pelo mesmo colaborador e `data_evento`, sem filtro de disputa aberta, reavalia o C6 e grava `admin_decidiu` e `SILENCIO` com ator e motivo. Em seguida tenta reconciliar os chamados. A célula recebe lavra com antes/depois e autor; `trabalhou` pode retipar ou plantar batidas, `faltou` ou `justificado` podem criar ausência. Escala/vínculo é insumo; valores derivados podem mudar por batida/ausência. O carimbo e a lavra têm guardas idempotentes, mas o log do despachante volta a ser gravado quando um novo POST `trabalhou` passa. A UI exige `regularizar_batida` e só mostra o controle para dia passado, último recurso e status elegível. Veredito, oferta do botão e toda condição de efeito nos casos exatos ficam [nao verificado]. `app/ponto/views.py:3517`, `app/ponto/services/veredito_celula.py:176`, `app/ponto/services/veredito_celula.py:207`, `app/ponto/services/veredito_celula.py:318`, `app/ponto/services/veredito_celula.py:336`, `app/ponto/services/veredito_celula.py:351`, `app/ponto/portas/celula.py:1113`, `app/ponto/portas/celula.py:1130`, `app/chamados/models.py:3372`, `app/chamados/juizes.py:1516`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/colaboradores/services/calendario.py:94`.

A reabertura administrativa existente é no cartão **pai**, para staff ou `admin_ti` com permissão contextual. Escolhe a fechada mais recente daquele pai, exige motivo do catálogo, justificativa de pelo menos 20 caracteres e teto de duas. Limpa fechamento, cria pergunta nova de confirmação, tenta reabrir o pai terminal, grava nota com justificativa e log do autor/motivo/contador. A seleção por pai impede tratar o POST como idempotente por ID de disputa. Refechar continua sujeito ao C7, que preserva vivas e respostas sem veredito. A exceção de modelo `admin_encerra` encontra a guarda de já fechada antes do carimbo e ficou sem caller de produção localizado. A expiração por silêncio saiu do comando de arquivamento. `app/templates/chamados/partials/modal_fio.html:411`, `app/chamados/services/fio.py:160`, `app/chamados/services/fio.py:694`, `app/chamados/services/acoes_disputa.py:274`, `app/chamados/services/acoes_disputa.py:303`, `app/chamados/services/acoes_disputa.py:338`, `app/chamados/models.py:2047`, `app/chamados/models.py:2094`, `app/chamados/models.py:2184`, `app/chamados/models.py:2210`, `app/chamados/models.py:2396`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`.

Para os sete pares, **SEM PORTA administrativa avulsa para expirar pelo silêncio**. A recomendação condicional é a UI **Resolver dia** por dia individual com fato comprovado, ou confirmação do alcance real da resposta do próprio colaborador quando o responsável ainda precisa dizer o fato. Nos quatro pares `dia_furado_para_a_folha`, é indispensável medir novamente o veto da premissa e a retenção real da célula, além do C6. O desaparecimento de um par na S4 pode decorrer da exclusão por status de chamado vivo; a folha lê `classificar_export` e `motivos_retencao_celula`. `app/ponto/services/veredito_celula.py:176`, `app/chamados/views.py:1228`, `app/chamados/models.py:783`, `app/folha/fora_da_folha.py:115`, `app/folha/fora_da_folha.py:125`, `app/folha/export.py:1282`.

## 27/11 23:54 · sessao 2133c5a2fd15 · FERIADO-UM-JUIZ · verificador · origem=Code

**REPROVADO.** A c13 cura o bloqueio 1 da ver8 no tratador do fechamento. O bloqueio 2 persiste: um dia válido com 420 min de saída antecipada deixa a sombra inteira na lavra anterior. A prova da nova telemetria também tem uma lacuna demonstrada: fixar o total de falhas técnicas em `1` no resumo passa pelos seis alvos c13 e pelo controle completo. `app/ponto/services/fechamento.py:1320` `app/ponto/tests/test_feriado_um_juiz.py:384` `app/ponto/tests/feriado_um_juiz_mutantes.py:168` `app/ponto/tests/feriado_um_juiz_mutantes.py:753`

## 1. Bloqueio 1 e demais achados da ver8

**Bloqueio 1 curado.** A guarda lança `RecusaDeProcedencia` para campos sem dono declarado. Só esse tipo incrementa `recusas_oraculo`. As demais exceções que chegam ao tratador incrementam `falhas_tecnicas_oraculo` e emitem erro próprio. Com a flag ligada, a falha técnica é encadeada como causa de `RuntimeError`. A recusa de procedência ainda permite retornar `1` quando o motor gravou. `app/ponto/services/dia_pago.py:135` `app/ponto/services/dia_pago.py:321` `app/ponto/services/fechamento.py:1320` `app/ponto/services/fechamento.py:1339` `app/ponto/tests/test_feriado_um_juiz.py:493`

- **Bloqueio 2 aberto.** Reproduzi em memória `06:00 E / 06:30 S`, escala `07:00–15:00`. O mensal e o `motor` ficam com zero trabalhado e 420 min de antecipada. O oráculo registra uma recusa e conserva a versão anterior. O dia fechado de 7 h, isolado, grava as duas versões. Junto ao BUG-144, o `motor` grava 5,95 h e 7 h, o mensal fica em 12,95 h e a sombra inteira conserva a lavra anterior. `app/ponto/tests/test_feriado_um_juiz.py:255` `app/ponto/tests/test_feriado_um_juiz.py:350` `app/ponto/tests/test_feriado_um_juiz.py:384` `app/ponto/tests/test_feriado_um_juiz.py:498` `app/ponto/services/dia_pago.py:329`
- **Ver7 e Opus.** Os 357 min do BUG-144 continuam preservados no mensal e no `motor`. A exigência por dia segue baseada em `CAMPOS_DIA`, com acesso direto à foto dos períodos quando falta a chave em `rubricas`. `do_motor` chega à guarda nos dois casos, e a foto inclui os três tipos de FT. Reexecutei os mutantes c10 e os dois c11 desses pontos, todos com a falha esperada. `app/ponto/tests/test_feriado_um_juiz.py:339` `app/ponto/services/dia_pago.py:228` `app/ponto/services/dia_pago.py:238` `app/ponto/services/dia_pago.py:252` `app/ponto/services/dia_pago.py:262` `app/ponto/services/dia_pago.py:325` `app/ponto/tests/feriado_um_juiz_mutantes.py:123` `app/ponto/tests/feriado_um_juiz_mutantes.py:206`
- **C12.** O registro de linhas B1 passou na catraca. Persiste o ponto cego da ver8: duas ocorrências na mesma linha viram uma entrada no registro. `app/core/tests/test_medida_turno_do_par.py:332` `app/core/tests/test_medida_turno_do_par.py:565` `app/core/medida_turno_do_par.py:217` `app/core/medida_turno_do_par.py:228`

## 2. Quem escreve e quem lê

| Item | Escritores, leitores e significado |
|---|---|
| `RecusaDeProcedencia` | A classe herda de `ValueError`, e a guarda de `lavrar` é o único `raise` desse tipo na cópia. O fechamento importa e testa o tipo no tratador da sombra. Os leitores legados localizados são testes que capturam `ValueError` e conferem a mensagem ou os campos. O116 também captura `ValueError` no caso com todos os donos para excluir a mensagem da guarda. `app/ponto/services/dia_pago.py:135` `app/ponto/services/dia_pago.py:329` `app/ponto/services/fechamento.py:1236` `app/ponto/services/fechamento.py:1321` `app/ponto/tests/test_feriado_um_juiz.py:380` `app/ponto/tests/test_feriado_um_juiz.py:410` `app/ponto/tests/test_feriado_um_juiz.py:431` `app/ponto/tests/test_o116_lavratura_le_o_dia_do_motor.py:223` `app/ponto/tests/test_o116_lavratura_le_o_dia_do_motor.py:246` |
| `recusas_oraculo` | O fechamento zera por chamada e incrementa uma vez por colaborador cuja tentativa termina em `RecusaDeProcedencia`. Lê o acumulado no `ERROR` como `total_recusas` e no resumo `INFO`. Os testes leem o erro individual e extraem o total do resumo por regex. O vigia do harness também usa a chave para localizar o resumo. `app/ponto/services/fechamento.py:683` `app/ponto/services/fechamento.py:1321` `app/ponto/services/fechamento.py:1347` `app/ponto/tests/test_feriado_um_juiz.py:321` `app/ponto/tests/test_feriado_um_juiz.py:337` `app/ponto/tests/feriado_um_juiz_mutantes.py:176` |
| `falhas_tecnicas_oraculo` | O fechamento zera por chamada, incrementa no outro ramo do tratador e lê o total no resumo. O vigia do harness lê `falhas_tecnicas_oraculo=1`. Na chamada que relança o erro, há `ERROR` individual e a execução sai antes do resumo. `app/ponto/services/fechamento.py:683` `app/ponto/services/fechamento.py:1327` `app/ponto/services/fechamento.py:1338` `app/ponto/services/fechamento.py:1347` `app/ponto/tests/feriado_um_juiz_mutantes.py:168` |
| `falha_lavra` | É local ao colaborador. Recebe `None` no início, recebe o erro do `motor` depois de marcar rollback, ou recebe o erro técnico da sombra com a flag ligada. O tratador da sombra lê `is None` antes de atribuir. Ao sair do `atomic`, o fechamento usa a variável para contar `processados` ou relançar. Portanto `None` ainda admite falha técnica da sombra no modo padrão. `app/ponto/services/fechamento.py:928` `app/ponto/services/fechamento.py:1274` `app/ponto/services/fechamento.py:1331` |
| `propagar_falha` | A assinatura fornece `False`. O único chamador de produção localizado que fornece `True` é `recalcular_na_revisao`. As leituras são a atribuição da falha técnica da sombra, o relançamento após o `atomic` e o encadeamento no `except` externo. O teste da revisão verifica a flag nas duas competências. Os testes c13 passam `True` diretamente para falha de alimentação e recusa. `app/ponto/services/fechamento.py:513` `app/ponto/services/fechamento.py:1331` `app/ponto/services/fechamento.py:1338` `app/ponto/services/fechamento.py:1893` `app/ponto/tests/test_revisar_par_relampago.py:163` `app/ponto/tests/test_feriado_um_juiz.py:486` |
| `dia_pago oraculo` | O fechamento escreve três formatos sob o prefixo. `lavratura falhou` agora significa recusa de procedência. `falha tecnica` é o erro técnico individual. O `INFO` publica os dois totais. O teste da recusa lê trechos do erro e usa regex no resumo. O teste técnico usa regex para a expressão “falha técnica”. Os testes de escrita, incluindo o ORM, capturam o logger e conferem o motivo injetado. A configuração SaaS encaminha `INFO` e `ERROR` a `ponto.log`, e `ERROR` também ao console. `app/ponto/services/fechamento.py:18` `app/ponto/services/fechamento.py:1323` `app/ponto/services/fechamento.py:1328` `app/ponto/services/fechamento.py:1347` `app/ponto/tests/test_feriado_um_juiz.py:117` `app/ponto/tests/test_feriado_um_juiz.py:321` `app/ponto/tests/test_feriado_um_juiz.py:480` `app/ponto/tests/test_feriado_um_juiz_porta.py:496` `app/config/settings/saas.py:130` `app/config/settings/saas.py:160` `app/config/settings/saas.py:181` |

A tela e o comando de recálculo leem `processados`. O backfill lê o dicionário retornado e conta seus colaboradores. `diff_calculador` lê `Rubricas` diretamente. O cron `pre_fechamento` lê bloqueios e marcos. O vigia do `ESTADO` e a API do HAIKU leem `ponto.log` pelo parser de `ponto.recusa_critica`, cujo regex exige `RECUSA-ACAO-CRITICA`. Leitor automatizado dos três formatos `dia_pago oraculo` em produção **[nao verificado]**. `app/ponto/views.py:1280` `app/ponto/management/commands/recalcular_fechamento.py:130` `app/ponto/management/commands/lavrar_dias_pagos.py:104` `app/ponto/management/commands/diff_calculador.py:406` `app/config/crons.py:406` `app/config/crons.py:1153` `app/folha/management/commands/pre_fechamento.py:41` `app/folha/management/commands/pre_fechamento.py:69` `bin/gerar_estado.py:265` `bin/recusa_estado.py:45` `bin/recusa_estado.py:68` `app/api/views_mensageria.py:1872` `app/core/recusa_critica.py:46`

## 3. Outros `ValueError`

Na cadeia de `lavrar`, **nenhum outro `raise ValueError` explícito de procedência** foi localizado. A guarda é o único `raise` explícito no corpo e nos auxiliares locais. O `raise ValueError` de `apurar_furo_da_exportada` exige que o chamador forneça a apuração do furo e pertence a outra porta. `app/ponto/services/dia_pago.py:50` `app/ponto/services/dia_pago.py:109` `app/ponto/services/dia_pago.py:139` `app/ponto/services/dia_pago.py:329` `app/ponto/services/dia_pago.py:653`

- **Na lavra**, os pontos de falha de valor incluem conversões numéricas de grade, rubricas e ajustes, desempacotamento de FT e a chamada de `timezone.localtime` no fallback de dia. São operações de entrada ou escrita. Se a exceção escapar da execução da sombra, cai como técnica. Tipos exatos de exceções de Django e do banco fora da cópia **[nao verificado]**. `app/ponto/services/dia_pago.py:132` `app/ponto/services/dia_pago.py:238` `app/ponto/services/dia_pago.py:300` `app/ponto/services/dia_pago.py:339` `app/ponto/services/dia_pago.py:360` `app/ponto/services/dia_pago.py:370` `app/ponto/services/fechamento.py:1326`
- **Na alimentação**, datas ISO, conversões numéricas e dados de pontualidade podem falhar antes de `lavrar`. A fábrica de motor tem dois casos explícitos, régua inválida e `CicloSemMotor`, subclasse de `ValueError`. Ambos são problemas de configuração. O `except` preexistente em `rubricas_por_dia` captura erros da fábrica e segue com `_mcct=None`. Reproduzi uma injeção de `ValueError` ali em um dia fechado: gravaram as duas versões, com `recusas_oraculo=0` e `falhas_tecnicas_oraculo=0`. A c13 classifica as exceções que **chegam** ao tratador, e a alimentação já pode absorvê-las antes. `app/ponto/calculador/alimentacao.py:429` `app/ponto/calculador/alimentacao.py:512` `app/ponto/calculador/alimentacao.py:524` `app/ponto/calculador/alimentacao.py:568` `app/ponto/calculador/regras.py:707` `app/core/regua_cct.py:457` `app/core/regua_cct.py:479` `app/ponto/motor_calculo_v2.py:769` `app/ponto/motor_calculo_v2.py:3701` `app/ponto/motor_calculo_v2.py:3769`

## 4. Onde muda o que fica gravado

**Muda na restauração administrativa de batida retratada pelo cron.** O POST `decidir_par_relampago` seleciona `restaurar_retratacao_cron`, que abre o `atomic` do ato. `restaurar_batida` anota a batida e a trilha e chama `recalcular_na_revisao`. Este roda uma vez por competência elegível do dia civil e do dia de turno, pula as entregues por K6 e passa `propagar_falha=True`. A sombra é tentada a partir de 10/2026, após sucesso do `motor`. `app/ponto/urls.py:63` `app/ponto/views.py:3486` `app/ponto/views.py:3497` `app/ponto/services/par_relampago_na_lista.py:479` `app/ponto/services/par_relampago_na_lista.py:492` `app/ponto/registro_batida.py:283` `app/ponto/registro_batida.py:317` `app/ponto/services/fechamento.py:1888` `app/ponto/services/fechamento.py:1284`

O savepoint da sombra desfaz a sombra. O `atomic` interno do colaborador já saiu quando o `raise` é executado. **Quem desfaz `FechamentoMensal`, `DiaPago motor`, batida e trilha é o `atomic` externo da restauração**, inclusive se outra competência já tiver sido recalculada no mesmo ato. Antes da c13, a falha técnica da sombra deixava a revisão gravar. Em chamada direta com `True` e sem transação externa, reproduzi o `RuntimeError` com o mensal e o `motor` novos ainda gravados na simulação. `app/ponto/services/fechamento.py:930` `app/ponto/services/fechamento.py:1181` `app/ponto/services/fechamento.py:1197` `app/ponto/services/fechamento.py:1287` `app/ponto/services/fechamento.py:1331` `app/ponto/services/par_relampago_na_lista.py:479` `app/ponto/tests/test_feriado_um_juiz.py:129` `app/ponto/tests/test_feriado_um_juiz.py:149`

Tela, comando e recálculo por evento usam o padrão `False`, que preserva mensal e `motor` diante de falha técnica da sombra. O backfill usa o mesmo padrão e grava `DiaPago` em modo de leitura do mensal. Incidência, valores afetados, DIFF de frota e aval para o novo efeito na revisão **[nao verificado]**. `app/ponto/views.py:1280` `app/ponto/management/commands/recalcular_fechamento.py:130` `app/ponto/services/fechamento.py:1850` `app/ponto/management/commands/lavrar_dias_pagos.py:106` `app/ponto/tests/test_feriado_um_juiz_porta.py:498` `app/docs/LEIS.md:112`

## 5. Mordida dos REDs

**Os seis mutantes c13 morreram na execução local.** Eles cobrem alargar a recusa para `ValueError`, suprimir a propagação na escrita, lançar o tipo genérico na guarda, retirar a herança de `ValueError`, trocar o rótulo do erro técnico e retirar seu incremento. M4 cobre propagar indevidamente uma recusa, M5 cobre tirar a contagem de recusas e M2_savepoint cobre retirar o savepoint da alimentação. Reconstituí em memória os dois blobs de produção da base do anexo e confirmei que nela falham os testes de classificação técnica, propagação e resumo. `app/ponto/tests/feriado_um_juiz_mutantes.py:123` `app/ponto/tests/feriado_um_juiz_mutantes.py:184` `app/ponto/tests/feriado_um_juiz_mutantes.py:535` `app/ponto/tests/feriado_um_juiz_mutantes.py:758` `app/ponto/tests/test_feriado_um_juiz.py:460`

**Há lacunas.** Propagar só erros de escrita e engolir erros de alimentação passa pelos seis alvos c13. O RED de alimentação com `True` derruba essa alteração no controle, mas ficou sem mutante dedicado. Fixar o resumo técnico em `1` ou tirar `dia_pago oraculo:` do `ERROR` técnico passa também pelo controle de 56 testes. O controle só exige o total técnico positivo em um cenário, e a regex do erro técnico só exige a expressão “falha técnica”. Já o ramo falso de `falha_lavra is None` é inalcançável ali: a sombra está no `else` da lavra `motor`. `app/ponto/tests/test_feriado_um_juiz.py:480` `app/ponto/tests/test_feriado_um_juiz.py:486` `app/ponto/tests/feriado_um_juiz_mutantes.py:168` `app/ponto/tests/feriado_um_juiz_mutantes.py:189` `app/ponto/tests/feriado_um_juiz_mutantes.py:753` `app/ponto/services/fechamento.py:1274` `app/ponto/services/fechamento.py:1331`

## 6. Classes de reprovação e limites da prova

- **Juiz, escritor e derivação.** O delta reutiliza a partição existente e a mesma `lavrar` para as duas versões. O retrato da exportada é uma porta preexistente. Persiste a divergência de chave da ver8: períodos, FT e alimentação usam o dia da jornada quando há mapa, enquanto o atraso de ligação e o O85 usam data civil da entrada. Efeito dessa divergência em produção **[nao verificado]**. `app/ponto/services/fechamento.py:905` `app/ponto/services/fechamento.py:997` `app/ponto/services/fechamento.py:1237` `app/ponto/services/fechamento.py:1296` `app/ponto/services/fechamento.py:1300` `app/ponto/services/dia_pago.py:109` `app/ponto/services/dia_pago.py:370` `app/ponto/services/dia_pago.py:417` `app/ponto/services/credito_parcial.py:73` `app/ponto/calculador/alimentacao.py:73`
- **Relógio e trava.** O teto do furo usa um instante lido na borda da passada e a mesma lista nas duas lavras. A revisão mantém Empresa por PK → Colaborador → Batida por PK, antes do recálculo e do savepoint. `app/ponto/services/fechamento.py:680` `app/ponto/services/fechamento.py:1152` `app/ponto/services/fechamento.py:1262` `app/ponto/services/fechamento.py:1316` `app/ponto/services/par_relampago_na_lista.py:172` `app/ponto/services/par_relampago_na_lista.py:402` `app/ponto/services/par_relampago_na_lista.py:483`
- **Campo com dois significados.** `nao_decide` continua significando rubrica sem afirmação no DIFF e suficiência para declarar dono na guarda. Reproduzi a chamada direta com `horas_falta` só em `NAO_DECIDE` e sem `faltas_por_dia`: gravou `0.0` com a chave em `calculador_nao_decide`. O fechamento fornece o mapa de faltas. `app/ponto/calculador/regras.py:532` `app/ponto/management/commands/diff_calculador.py:508` `app/ponto/services/dia_pago.py:59` `app/ponto/services/dia_pago.py:172` `app/ponto/services/dia_pago.py:340` `app/ponto/services/fechamento.py:946` `app/ponto/services/fechamento.py:1304`
- **Selo e `except`.** O delta adiciona **0** cláusulas `except` e remove **0**. Na AST dos blobs de produção, `dia_pago.py` ficou em 1 e `fechamento.py` em 9. Rodei 56 controles com uma falha esperada e 41 estruturais verdes. A falha esperada local é a borda 20/21 com troca de 6x1 para 12x36. Os quatro blobs de saída coincidem com o anexo. A identidade do commit inteiro, os 100 testes da pista e a execução ORM com banco ficam **[nao verificado]** nesta cópia. `app/ponto/services/dia_pago.py:469` `app/ponto/services/fechamento.py:1320` `app/ponto/tests/feriado_um_juiz_mutantes.py:535` `app/ponto/tests/feriado_um_juiz_mutantes.py:584` `app/ponto/tests/feriado_um_juiz_mutantes.py:753` `app/ponto/tests/test_feriado_um_juiz.py:965`

## 27/11 23:52 · sessao 1e4ec689109c · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A cura da c12 está na cópia. O cabeçalho `sticky` ganhou `z-index:1` dentro do drawer. A checagem estática confirmou as camadas `10999` do drawer, `11000` do popover e `100000` da confirmação, além do `isolation:isolate` da célula. [Cabeçalho](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:4) · [Drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:3) · [Popover](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:133) · [Confirmação](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_dialogo.html:113) · [Célula](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68)

### RED, GREEN e m12

Antes da cura, a checagem estática retornou `sintaxe Python OK, caso RED presente` com o cabeçalho ainda em `position:sticky;top:0`. A sintaxe da sonda também passou no V8. A saída do RED e do GREEN no Chromium está [nao verificado]. `node`, `chromium` e `chromium-browser` estavam ausentes do `PATH` desta sessão. [Caso RED](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:594) · [Driver](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:562) [nao verificado]

O caso abre o calendário diretamente no drawer de `base.html` a 1366 px. A sonda posiciona a célula de 20/11 na primeira linha e a estende à largura da grade para passar sob os dois centros na mesma rolagem. Exige rolagem efetiva e a presença da célula sob ambos os pontos antes de cobrar que `elementFromPoint` acerte o × e o título. O m12 remove somente o `z-index` inline do cabeçalho e exige os dois resultados `False`. [Sonda](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:208) · [Cenário desktop](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:716) · [MORDE](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:612)

Comandos para a sessão, a partir de `app/`. O primeiro deve falhar com o template da base e passar com a cura. O segundo aplica o m12 e deve falhar pelos dois hit-tests. O terceiro exige essa falha específica. O quarto inclui o smoke existente de página e drawer. [Asserção comum](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:579) · [Smoke de clique](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:588)

```sh
classe=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest
caso="$classe.test_cabecalho_do_drawer_fica_acima_do_calendario_ao_rolar"

PYTHONDONTWRITEBYTECODE=1 python3 -m django test "$caso" --settings=config.settings.ci --noinput

SMOKE_FERIADO_MUTANTE=drawer-sem-z-index-no-cabecalho PYTHONDONTWRITEBYTECODE=1 \
  python3 -m django test "$caso" --settings=config.settings.ci --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -m django test \
  "$classe.test_MORDE_drawer_sem_z_index_no_cabecalho" --settings=config.settings.ci --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -m django test "$classe" --settings=config.settings.ci --noinput
```

### Censo do drawer genérico

- **Colaboradores e Retrato.** Cadastro e Espelho pela lista, Benefícios, lista e Raio-X do Retrato. As ações do Retrato também abrem vínculo, fio, calendário e decisão de ausência. [Lista](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_lista_tabela.html:16) · [Benefícios](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/beneficios_lista.html:86) · [Lista do Retrato](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/folha/_retrato_lista.html:61) · [Raio-X do Retrato](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/folha/_retrato_raiox.html:70) · [Portas das ações](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/folha/retrato_folha.py:989)
- **Fita, postos e mapas.** A fita entra pelo Situacional e pelas pendências de disputa. Postos reinjeta o painel após operações da lista. GPS do colaborador e os mapas de localização do fio e da gestão escrevem diretamente no corpo. [Situacional](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_ps_linha.html:4) · [Disputa](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/pendencias_disputa.html:9) · [Postos](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/postos.html:96) · [GPS](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/geo-colab-map.js:38) · [Mapa do fio](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/fio_colaborador.html:310) · [Mapa da gestão](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/painel_gestao.html:580)
- **Chamados.** Prévia de validação em lote, Raio-X com filtro por empresa, busca no Arquivo e Arquivados do atendimento, com reinjeção nas duas buscas. [Lote](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/painel_gestao.html:14) · [Raio-X](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/painel_gestao.html:629) · [Filtro](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/partials/_raiox_lente.html:5) · [Arquivo](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/painel_gestao.html:416) · [Atendimento](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/meu_atendimento.html:45) · [Reinjeção do Arquivo](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/partials/_busca_arquivados.html:68) · [Reinjeção do atendimento](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/chamados/partials/_meus_arquivados.html:48)
- **Férias, ausências, folha e holerites.** Seção de férias, painel de ausência, Raio-X da folha e do Retrato, publicação em lote e modos do quadrinho. O formulário do lote tem o corpo do drawer como alvo HTMX. [Férias](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/ferias/lista.html:81) · [Ausências](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/ponto/ausencias.html:217) · [Folha](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/folha/index.html:26) · [Retrato](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/folha/_retrato_barra.html:73) · [Lista DP](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/holerite/lista_dp.html:12) · [Histórico de lotes](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/holerite/historico_lotes.html:7) · [Quadrinho](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/holerite/_quadrinho_celula.html:17) · [Alvo HTMX](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/holerite/_lote_drawer.html:19)

Na leitura da cópia, nenhum consumidor identificado dependia de conteúdo com camada `auto` ou `0` sobre o cabeçalho. O rodapé de ausência se ancora em `bottom:0`. As listas de posto e escala têm camadas positivas próprias, assim como o modal geográfico. O lightbox é anexado ao `body`. O comportamento desses consumidores em navegador está [nao verificado]. [Rodapé](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/ponto/partials/painel_ausencia.html:286) · [Posto](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/painel_posto.html:57) · [Escala](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/painel_vinculo.html:153) · [Modal geográfico](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_geo_modal.html:2) · [Lightbox](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:60)

O commit fica com a sessão pela proibição expressa de `git commit` no despacho. A certificação cabe ao Code e ao Opus final. O clique do Ronald no calendário real de 20/11 e o recorte de 360 px no WebView seguem [nao verificado]. [L-126](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/docs/LEIS.md:147) · [Recorte do smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:699) [nao verificado]

```text
OBRA: FERIADO-BOTAO-CORTADO
LEI-AKITA: origem=app/templates/core/_drawer_generico.html:4, testemunha=hit-test do título e do × em app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235, RED=test_cabecalho_do_drawer_fica_acima_do_calendario_ao_rolar, quem-mais-le=app/templates/base.html:399 e app/templates/base_app.html:154, juizes novos=0, padrao=n/a (instrumento)
```

## 27/11 23:44 · sessao 62872894f163 · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**REPROVADO para pouso.** B1 está curado para snapshots gravados com c11, e B2 está curado no desenho do smoke de página. Há dois bloqueantes no recorte acordado de `base.html` a 1366 px: o c11 põe células do calendário acima do cabeçalho fixo do drawer quando há sobreposição na rolagem, e o clique pré-pouso do Ronald no calendário real de 20/11 segue [nao verificado]. A sobreposição é um achado de leitura do CSS. Sua ocorrência em uma sessão real segue [nao verificado]. `app/static/js/popover-acao.js:28` `app/static/js/popover-acao.js:176` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:242` `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/core/_drawer_generico.html:3` `app/docs/LEIS.md:99`

### 1. B1 e B2 da ver2

- **B1 — curado para gravações novas e para os listeners do componente.** O HTMX 1.9.10 emite `htmx:beforeHistorySave` antes de clonar o `body`. Nesse evento, `fecha(false)` fecha o `details`, devolve o painel ao pai, restaura CSS e `tabindex` e desliga o observador, sem devolver foco ao gatilho. Na restauração, o HTMX volta a executar os scripts do fragmento, e a guarda em `window` impede uma segunda instalação dos oito listeners do popover. Um snapshot antigo que já contenha um órfão continua fora do alcance de `fecha()`, que opera sobre `atual`. Existência de snapshots antigos afetados em uso real [nao verificado]. `app/core/tests/vendor/htmx-1.9.10.min.js:1` `app/static/js/popover-acao.js:28` `app/static/js/popover-acao.js:144` `app/static/js/popover-acao.js:160`
- **B2 — curado.** O Client Django abre `colaboradores:detalhe`, exige `detalhe.html` e entrega seu HTML ao Chromium. O próprio script da ficha carrega o calendário por HTMX. A sonda espera `afterSettle` e exige a grade com primeira coluna de `420px`, a coluna do calendário com `overflow:hidden` e o fragmento dentro dela. A resposta HTMX vem do Client por CDP na origem virtual. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:134` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:242` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:677` `app/templates/colaboradores/detalhe.html:87` `app/templates/colaboradores/detalhe.html:202` `app/templates/colaboradores/detalhe.html:215` `app/core/tests/smoke_feriado_chromium.js:68`

### 2. Pintura, rolagem, foco e consumidores

- **c10.** A região interna vira grid e, nas células de 132 px que oferecem a ação, as batidas vivas e retratadas, inclusive **Inverter/Retratar** e **Desfazer**, passam visualmente para depois dos badges e, quando houver, da fileira de lote, **Resolver dia** e **Pauta DP**. Ícone, ausência, chamado e selos continuam no começo da região. A mudança é por `order:1`, com os controles das marcações ainda antes dos controles da fileira no DOM. O `tabindex="0"` da região permanece. A sequência efetiva com Tab e a rolagem produzida pelo foco, quando os controles coexistem, estão [nao verificado]. Como `calendario_partial.html` sempre passa `layout='mes'`, isso alcança também Semana, Quinzena e Competência pela mesma rota. `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:99` `app/templates/colaboradores/partials/_calendario_grade.html:102` `app/templates/colaboradores/partials/_calendario_grade.html:132` `app/templates/colaboradores/partials/_calendario_grade.html:138` `app/templates/colaboradores/calendario_partial.html:50` `app/templates/colaboradores/calendario_partial.html:67`
- **c11.** A célula vira contexto de empilhamento. Dentro dela, o gatilho fica em `z-index:1` e toda a região rolável em `0`. Aberto, o painel vai para o `body` em `11000`, acima do drawer em `10999` e abaixo da confirmação em `100000`. O efeito colateral está no **cabeçalho do drawer genérico**: ele é `sticky` com `z-index:auto`, e o corpo posterior está no mesmo contexto do drawer. Quando o conteúdo rola por baixo do cabeçalho, a célula isolada é pintada depois dele. A faixa da célula pode cobrir o título e interceptar o **×** na área de interseção. O caminho direto do Retrato injeta o calendário no corpo rolável do drawer e expõe essa combinação. O hit-test do **×** durante a rolagem está [nao verificado]. `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:97` `app/templates/core/_drawer_generico.html:3` `app/templates/core/_drawer_generico.html:8` `app/templates/core/_drawer_generico.html:25` `app/templates/folha/_retrato_o_que_fazer.html:73` `app/folha/retrato_folha.py:1019` `app/static/js/popover-acao.js:127` `app/templates/core/_dialogo.html:113`
- **Efeito compartilhado do JS.** **Tornar dia de trabalho**, **Inverter/Retratar** e **Resolver dia/Cobrar este dia** passam a fechar antes de um snapshot HTMX, em qualquer superfície que os renderize. A fita de diagnóstico recebe esse efeito do JS, enquanto o CSS de c10/c11 exige `layout='mes'`. Furos passa só `dias` e `resumo` com `only`, portanto fica fora do novo ramo e das ações condicionadas às permissões. `app/templates/ponto/partials/_feriado_vira_trabalho.html:5` `app/templates/ponto/partials/_veredito_dia.html:5` `app/templates/ponto/partials/_veredito_dia.html:25` `app/templates/colaboradores/partials/_calendario_grade.html:134` `app/static/js/popover-acao.js:176` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/relatorios/furos.html:42`

| Inclusão | Caminho na cópia |
|---|---|
| `popover-acao.js` | Inclusão direta em `app/templates/base.html:440` e `app/templates/base_app.html:194`. Ambas também incluem o drawer genérico em `app/templates/base.html:399` e `app/templates/base_app.html:154`. |
| `_calendario_grade.html` | Três inclusões diretas. Calendário mensal em `app/templates/colaboradores/calendario_partial.html:67`, fita em `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`, Furos em `app/templates/relatorios/furos.html:42`. A view entrega o parcial mensal em `app/colaboradores/views.py:2010` e o diagnóstico em `app/colaboradores/views.py:2991`. |
| Caminhos em `base.html` | Ficha em `app/templates/colaboradores/detalhe.html:1` e `:215`. Espelho do drawer em `app/templates/colaboradores/partials/painel_colaborador.html:182` e `app/templates/core/_drawer_generico.html:134`. Entradas do Espelho pela lista, Benefícios, lista e Raio-X do Retrato em `app/templates/colaboradores/partials/_lista_tabela.html:16`, `app/templates/colaboradores/beneficios_lista.html:84`, `app/templates/folha/_retrato_lista.html:61` e `app/templates/folha/_retrato_raiox.html:68`. Entrada direta no calendário pelo Retrato em `app/templates/folha/_retrato_o_que_fazer.html:73`. Entradas da fita em `app/templates/colaboradores/partials/_ps_linha.html:4` e `app/templates/colaboradores/partials/pendencias_disputa.html:9`. Furos estende `base.html` em `app/templates/relatorios/furos.html:1`. |
| Caminhos em `base_app.html` | O drawer e o JS estão disponíveis pela casca e o parcial mensal é reaproveitável, condicionado a `ver_cadastro`. A ficha de gestor estende `base.html` literalmente. O Espelho pessoal usa `base_tpl` e tem template próprio. O smoke de `base_app.html` monta a página artificial e abre o drawer por JS com respostas do Client. Uma entrada real do WebView para a grade de gestor está [nao verificado]. `app/templates/base_app.html:154` `app/templates/base_app.html:194` `app/colaboradores/views.py:1975` `app/templates/colaboradores/detalhe.html:1` `app/templates/ponto/espelho.html:1` `app/core/context_processors.py:194` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:209` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:690` |

### 3. Quem escreve e quem lê

| Valor | Escrita → leitura |
|---|---|
| Oferta e permissão | `contexto_calendario` escreve `feriado_vira_trabalho_oferecido` a partir de feriado aplicável e célula existente com `trabalha is False`. A grade e o POST leem o mesmo booleano. `tem_acao` fornece `pode_editar_dia` e `pode_veredito` à view mensal e ao serviço da fita. A grade lê as flags, e o POST consulta `editar_escalas` de novo. `app/colaboradores/services/calendario.py:343` `app/colaboradores/services/calendario.py:791` `app/colaboradores/mixins.py:322` `app/colaboradores/views.py:1992` `app/colaboradores/views.py:2007` `app/colaboradores/services/drawer.py:157` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/ponto/views.py:3607` `app/ponto/views.py:3624` |
| Dados da grade | Os chamadores passam `layout`. O calendário produz `dias`, `cal_offset`, status, palavra, badges, batidas, retratadas e oferta de Resolver dia. A grade lê essas projeções. As batidas vivas são do **dia do turno** e as retratadas são agrupadas por **dia civil** em campos distintos. A ordem visual do bloco conserva a ordem das marcações dentro dele. `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/colaboradores/services/calendario.py:639` `app/colaboradores/services/calendario.py:673` `app/colaboradores/services/calendario.py:754` `app/colaboradores/services/calendario.py:812` `app/colaboradores/services/calendario.py:842` `app/templates/colaboradores/partials/_calendario_grade.html:124` |
| `data-*` da grade e dos painéis | A grade escreve `data-celula-dia`, `data-mes` e `data-calendario-marcacoes`. A sonda e os selos usam a célula como âncora. `fita-mes.js` lê o mês formatado. O novo marcador de marcações é lido pelo seletor do mutante, e a ordem em produção vem do estilo inline. O parcial escreve `data-calendario-raiz`, que o HTMX lê para os swaps e a sonda lê para localizar o fragmento. Os três produtores de popover escrevem `data-popover-acao` e `data-popover-painel`. O componente e as sondas os leem como marcadores de `details` e do painel associado. `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:132` `app/static/js/fita-mes.js:11` `app/templates/colaboradores/calendario_partial.html:39` `app/templates/ponto/partials/_feriado_vira_trabalho.html:5` `app/templates/ponto/partials/_veredito_dia.html:5` `app/static/js/popover-acao.js:112` `app/static/js/popover-acao.js:162` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:77` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:259` |
| Campos do POST | O parcial escreve `colab`, `dia` e `volta`, emite o CSRF e recebe `motivo` do administrador. `data-feriado-vira-trabalho` localiza na sonda o mesmo campo oculto `dia` que o navegador envia. O middleware lê o token, e a view valida os valores e entrega a edição à porta. `app/templates/ponto/partials/_feriado_vira_trabalho.html:7` `app/config/settings/base.py:58` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:259` `app/ponto/views.py:3584` `app/ponto/views.py:3610` `app/ponto/views.py:3632` |
| Estado, foco e listeners | O navegador atualiza `details.open` e emite `toggle`. `abre` lê as medidas e guarda em `atual` os nós, o pai, o irmão, o CSS e o `tabindex`. `posiciona` escreve a geometria. `fecha` limpa `atual`, restaura o nó e seus atributos, e devolve foco quando cabível. O drawer emite `drawer:fechou`, o HTMX emite `beforeHistorySave`, e o componente escuta ambos. O mesmo componente escreve e lê `window.popoverAcaoInicializado`. O observador lê `isConnected` após mutações, e os demais listeners leem clique, Escape, resize, orientação e scroll. `app/static/js/popover-acao.js:28` `app/static/js/popover-acao.js:49` `app/static/js/popover-acao.js:105` `app/static/js/popover-acao.js:119` `app/static/js/popover-acao.js:144` `app/static/js/popover-acao.js:160` `app/templates/core/_drawer_generico.html:71` |
| Snapshot HTMX | O HTMX escreve `{url, content, title, scroll}` no `htmx-history-cache`, lê a entrada pela URL, repõe o HTML e restaura a rolagem da janela. O popover altera o DOM antes da captura. A sonda lê `historyRestore.detail.item.content` e o DOM restaurado para contar órfãos. A chave de cache é do HTMX. `app/core/tests/vendor/htmx-1.9.10.min.js:1` `app/static/js/popover-acao.js:176` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:176` |
| Instrumentação do smoke | Python escreve os dados da sonda, a marca `data-smoke-popover-script` e a resposta com `data-smoke-pagina`. A sonda lê os seletores, conta registros em `window.smokeListenersPopover` e emite as medidas. `fichaReal` na entrada escolhe o caminho, e no resultado registra o layout observado. O driver lê o mapa de GETs e transmite as medidas à asserção Python. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:59` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:124` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:147` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:249` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:521` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:541` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:689` `app/core/tests/smoke_feriado_chromium.js:52` `app/core/tests/smoke_feriado_chromium.js:68` |

### 4. Mordida dos smokes e selos

- **Há asserções capazes de pegar as duas falhas da ver2.** Sem a guarda, a repetição do script aumenta os três contadores instrumentados. Sem o fechamento pré-snapshot, a sonda parte de um painel no `body` e cobra zero órfãos no HTML salvo e no documento após Voltar e Avançar. `pagina-sem-ficha` contraria a exigência de `fichaReal=True`. O MORDE de recorte exige o painel dentro da viewport com hit-test falhando. Os demais MORDEs automáticos exigem a falha de viewport, gatilho ausente, drawer fechado, exceção ou sobreposição ao gatilho. O `7/7` é o resultado informado. Identidade dos sete, saídas individuais e execução da suíte inteira nesta cópia [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:72` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:144` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:556` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:640` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:677`
- **Os selos vizinhos têm sinal próprio.** A varredura distingue o exemplo estrutural ruim do bom. O selo de Resolver dia exige que o controle negativo estoure de fato. O de Retratar exige uma rota `/retratar/` entre os painéis medidos. O teste de quatro batidas exige os quatro horários em ordem, o gatilho antes da região e a célula mensal de 132 px. `app/core/tests/test_selo_popover_acao.py:88` `app/core/tests/test_selo_popover_acao.py:185` `app/core/tests/test_smoke_retratar_1366.py:167` `app/colaboradores/tests/test_feriado_botao_cortado.py:71`
- **Lacunas que sobrevivem ao 7/7.** O mutante de empilhamento retira `isolation` e os dois `z-index` juntos, só no drawer. Nenhuma asserção mede o cabeçalho ou o **×** durante o scroll, nem a sequência com Tab. A sonda do feriado faz hit-test no submit e confere método e rota, mas o POST com motivo é coberto separadamente pelo Client Django, sem percorrer o formulário pelo Chromium. O histórico usa paginação sintética na ficha e conta três dos oito listeners. A paginação real da lista com drawer aberto, reabrir o popover depois de restaurar e o clique do Ronald em 20/11 seguem [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:59` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:78` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:158` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:195` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:383` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:439` `app/templates/colaboradores/partials/_lista_tabela.html:54` `app/ponto/tests/test_kaua_feriado_pela_tela.py:149` `app/docs/LEIS.md:99`

### 5. Classes e travas

- **Segundo juiz, segundo escritor, relógio fora da borda, derivação paralela e campo com dois significados.** Zero introduzidos no delta fornecido. O template consome os predicados existentes. O POST passa o mesmo `agora` à consulta de oferta e à edição. A escrita de `CelulaDia` e sua trilha continuam na porta. Os marcadores novos identificam elementos ou instrumentam o ensaio. `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/templates/colaboradores/partials/_calendario_grade.html:132` `app/ponto/views.py:3622` `app/ponto/portas/celula.py:642` `app/ponto/portas/celula.py:665` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:521`
- **Selo/RED e ordem de trava.** Há mordida para B1 e B2, com a lacuna de empilhamento acima. A porta lê `trabalha` com `select_for_update` dentro de `atomic`, aplica `so_se_folga` e só então grava. A ordem operacional suíte inteira → revisão final → pouso, e o smoke humano pré-pouso, seguem [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:556` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:640` `app/ponto/portas/celula.py:642` `CLAUDE.md:421` `app/docs/LEIS.md:99`
- **Cláusulas novas no delta:** **0 `except` Python e 0 `catch` JavaScript**. A rejeição da sonda e do driver vai para `motivo`, que precisa ser nulo na asserção. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:464` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:541` `app/core/tests/smoke_feriado_chromium.js:101` `app/core/tests/smoke_feriado_chromium.js:112`

### 6. Lista do pouso [nao verificado]

- **360 px.** A coluna inicial fixa de 420 px já excede a viewport da ficha. No drawer, a largura é limitada a `92vw` e o calendário conserva sete colunas e 132 px por célula. Visibilidade do gatilho, rolagem das marcações e acionabilidade do painel nessa largura [nao verificado]. `app/templates/colaboradores/detalhe.html:87` `app/templates/colaboradores/detalhe.html:202` `app/templates/core/_drawer_generico.html:3` `app/templates/colaboradores/partials/_calendario_grade.html:64` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:649`
- **`base_app`.** Ficam a superfície real do WebView, a passagem pelo bootstrap e os cliques reais de página e drawer. A cobertura disponível é em Chromium desktop, a 1366, com a casca de ensaio e o bootstrap PWA desativado. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:651` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:690` `app/core/tests/smoke_feriado_chromium.js:92`
- **Comparação com `main 59600163c` [nao verificado].** A cópia veio sem `.git`, e o delta fornecido começa em `c44771b4a`. Esse delta permite atribuir o novo empilhamento ao c11 e constatar que os 132 px já estavam no lado anterior do diff. A equivalência com `59600163c` e a identidade Git da cópia ficam [nao verificado]. `app/templates/colaboradores/partials/_calendario_grade.html:68`

## 27/11 23:43 · sessao 0195b4b4a14c · FERIADO-UM-JUIZ · construtor · origem=Code

Cura c13 aplicada nos três arquivos autorizados. Na base `9ed2051aa`, o controle sem banco reproduziu as quatro falhas de R-a, R-a2, R-b e R6 no subcaso `oraculo`. Com a cura, os quatro passaram, assim como R-c e R3. O controle final rodou **56 testes**, com uma falha esperada preexistente, e os **41 estruturais** passaram. [REDs](ponto/tests/test_feriado_um_juiz.py:460) · [R6](ponto/tests/test_feriado_um_juiz.py:49) · [R3](ponto/tests/test_feriado_um_juiz.py:383) · [Controle](ponto/tests/feriado_um_juiz_mutantes.py:753) · [Estruturais](ponto/tests/feriado_um_juiz_mutantes.py:582)

### Diff por arquivo

- **`dia_pago.py` (+7/−2)** — `RecusaDeProcedencia` herda de `ValueError`. Só a guarda BUG-144 passa a usar a classe nova, com a mesma mensagem. [Classe](ponto/services/dia_pago.py:135) · [Guarda](ponto/services/dia_pago.py:329)
- **`fechamento.py` (+19/−19)** — A captura continua sendo `except Exception`, com zero cláusulas acrescentadas. A recusa declarada conserva o contador e o log originais. A falha técnica tem log próprio, entra no resumo e segue para o `raise` existente quando `propagar_falha=True` e `falha_lavra` está vazia. [Captura](ponto/services/fechamento.py:1320) · [Propagação](ponto/services/fechamento.py:1332) · [Resumo](ponto/services/fechamento.py:1347)
- **`feriado_um_juiz_mutantes.py` (+62/−23)** — Atualizei o nome de R6, usei R-c para detectar M4 e alinhei a expectativa histórica de R5 à asserção vigente. Acrescentei ao harness uma verificação do contador técnico. As asserções dos REDs existentes foram preservadas. [Mapa e expectativas](ponto/tests/feriado_um_juiz_mutantes.py:100) · [Vigia](ponto/tests/feriado_um_juiz_mutantes.py:168)

### Comandos e mutantes

Executei em `app/`. `controle`, `estruturais`, `base_c10`, `reds_c10` e todos os comandos de mutação abaixo retornaram `rc=0`. Para mutação, `rc=0` indica que o controle passou e o teste detectou a alteração. [Mutantes c10](ponto/tests/feriado_um_juiz_mutantes.py:123) · [Mutantes c13](ponto/tests/feriado_um_juiz_mutantes.py:184) · [Executor](ponto/tests/feriado_um_juiz_mutantes.py:758)

```bash
fuj() {
  PYTHONDONTWRITEBYTECODE=1 PYTHONPATH=/usr/lib/python3/dist-packages \
    python3 -S -m ponto.tests.feriado_um_juiz_mutantes "$1"
}

fuj controle
fuj estruturais
fuj base_c10
fuj reds_c10
fuj M1
fuj M2
fuj M2_savepoint
fuj M3
fuj M3_literal
fuj M4
fuj M5
fuj c13_value_error_e_recusa
fuj c13_oraculo_nao_propaga
fuj c13_guarda_levanta_value_error
fuj c13_recusa_sem_heranca_value_error
fuj c13_falha_tecnica_sem_log_proprio
fuj c13_falha_tecnica_sem_contagem
```

`M2_savepoint` fez R-a falhar com `5.95 != 2`. Classificar por `ValueError` fez R-a2 acusar uma recusa indevida. Suprimir a propagação fez R6 falhar em `oraculo` com `RuntimeError not raised`. M4 fez a recusa real escapar em R-c. Os demais mutantes c13 foram detectados pela troca do tipo da guarda, pela perda da herança de `ValueError`, pela perda do log próprio ou pelo contador técnico zerado. [R-a e R-a2](ponto/tests/test_feriado_um_juiz.py:460) · [R6](ponto/tests/test_feriado_um_juiz.py:49) · [R-c](ponto/tests/test_feriado_um_juiz.py:493) · [Mutantes c13](ponto/tests/feriado_um_juiz_mutantes.py:184)

### Censo de leitores e capturas

- **`recusas_oraculo`** — O log individual lê o acumulado e o resumo publica o total. Os testes leem `total_recusas` e `recusas_oraculo`. O harness confere o resumo em M5 e localiza a mesma mensagem no teste do contador técnico. [Log individual](ponto/services/fechamento.py:1324) · [Resumo](ponto/services/fechamento.py:1347) · [Testes](ponto/tests/test_feriado_um_juiz.py:330) · [M5](ponto/tests/feriado_um_juiz_mutantes.py:130) · [Vigia](ponto/tests/feriado_um_juiz_mutantes.py:176)
- **Quem pega o `ValueError` da guarda** — O grep encontrou um `except ValueError` direto, no teste O116. As asserções de R2 e c11 também capturam `ValueError`. Todos continuam compatíveis pela herança. No fechamento, a guarda chega ao `except Exception` existente. [O116](ponto/tests/test_o116_lavratura_le_o_dia_do_motor.py:246) · [R2](ponto/tests/test_feriado_um_juiz.py:380) · [c11 FT](ponto/tests/test_feriado_um_juiz.py:410) · [c11 `do_motor`](ponto/tests/test_feriado_um_juiz.py:431) · [Fechamento](ponto/services/fechamento.py:1320)

Leitor automatizado em produção, execução com banco e certificação pelo Code e pelo opus final **[nao verificado]**.

## 27/11 23:36 · sessao 68d9d2d80e89 · FERIADO-UM-JUIZ · construtor · origem=Code

Passo 1 da c13 entregue em [app/ponto/tests/test_feriado_um_juiz.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:460). A execução local sem banco rodou 55 testes, com quatro falhas nos casos abaixo e uma falha esperada preexistente. R-c e R3 passaram. A certificação da cura fica para o Code e o opus final. [Executor sem banco](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:440) · [R3](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:384) [nao verificado]

| Caso | Teste | O que verifica | [nao verificado]
|---|---|---|
| R-a, reescrito | `test_RED_c10_falha_sql_da_alimentacao_reverte_o_savepoint` | Um `RuntimeError` na alimentação preserva o retorno `1`, o mensal `5.95`, o `motor` `[5.95]` e o oráculo anterior. O vigia exige zero recusas e um único log de erro que identifique a falha técnica. Na base, o resumo registra uma recusa. [app/ponto/tests/test_feriado_um_juiz.py:460](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:460) |
| R-a2, novo | `test_RED_c13_value_error_generico_na_alimentacao_e_falha_tecnica` | Aplica as mesmas verificações a um `ValueError` genérico. Na base e no mutante que classifica por `ValueError`, o resumo registra uma recusa. [app/ponto/tests/test_feriado_um_juiz.py:463](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:463) |
| R-b, novo | `test_RED_c13_falha_na_alimentacao_propaga_quando_solicitado` | Com `propagar_falha=True`, exige `RuntimeError` e confere a identidade do erro injetado na exceção ou na sua causa. Na base, a chamada retorna. [app/ponto/tests/test_feriado_um_juiz.py:486](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:486) |
| R-c, novo | `test_RED_c13_recusa_de_procedencia_nao_propaga_na_revisao` | Com `propagar_falha=True`, a recusa real do BUG-144 devolve `1`, registra uma recusa e preserva o oráculo anterior. Passou na base e detectou o mutante que propaga a recusa. [app/ponto/tests/test_feriado_um_juiz.py:493](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:493) | [nao verificado]
| R6, reescrito | `test_RED_c13_revisao_propaga_falha_de_escrita_nas_duas_lavras` | A revisão propaga falhas de escrita do `motor` e do oráculo e reverte a revisão e as gravações simuladas. Na base, o subcaso do oráculo falha por ausência de `RuntimeError`. O mapa histórico `REDS_C10` ainda referencia o nome anterior. [Teste](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:49) · [Mapa histórico](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:106) | [nao verificado]

### Comandos executados em `app/`

O comando do RED saiu com `rc=1`. [Entrada `controle`](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:716)

```bash
PYTHONDONTWRITEBYTECODE=1 PYTHONPATH=/usr/lib/python3/dist-packages \
  python3 -S -m ponto.tests.feriado_um_juiz_mutantes controle
```

Cada iteração abaixo executa um mutante em memória. `M2_savepoint` fez R-a falhar no mensal (`5.95 != 2`). `M4` fez R-b passar e R-c levantar a recusa. `ValueError` fez R-a passar e R-a2 falhar no contador. [Mutantes c10](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:482) · [Asserções c13](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:473)

```bash
for mutante in M2_savepoint M4 ValueError; do
  PYTHONDONTWRITEBYTECODE=1 PYTHONPATH=/usr/lib/python3/dist-packages \
    python3 -S - "$mutante" <<'PY' || exit $?
import inspect
import sys
from textwrap import dedent, indent
from unittest.mock import patch

from ponto.tests.feriado_um_juiz_mutantes import (
    TESTES, _fonte_mutada_c10, _preparar_sem_banco, _rodar, [nao verificado]
)

_preparar_sem_banco()
from ponto.services import fechamento

a = 'test_RED_c10_falha_sql_da_alimentacao_reverte_o_savepoint'
a2 = 'test_RED_c13_value_error_generico_na_alimentacao_e_falha_tecnica'
b = 'test_RED_c13_falha_na_alimentacao_propaga_quando_solicitado'
c = 'test_RED_c13_recusa_de_procedencia_nao_propaga_na_revisao'
caso = sys.argv[1]
fonte = dedent(inspect.getsource(fechamento.recalcular_fechamento_mes))

if caso == 'ValueError':
    espaco = ' ' * 32
    inicio = fonte.index(espaco + 'recusas_oraculo += 1\n')
    fim = fonte.index('            if falha_lavra is None:', inicio)
    bloco = fonte[inicio:fim]
    fonte = (
        fonte[:inicio] + espaco + 'if isinstance(_e_or, ValueError):\n'
        + indent(bloco, '    ') + espaco + 'else:\n'
        + espaco + "    logger.error('dia_pago oraculo: falha tecnica: %s', _e_or)\n"
        + fonte[fim:]
    )
    nomes, trecho, tipo = (a, a2), 'recusas_oraculo=1', 'failures'
else:
    fonte = _fonte_mutada_c10(caso, fonte)
    if caso == 'M2_savepoint':
        nomes, trecho, tipo = (a,), '5.95 != 2', 'failures'
    else:
        nomes, trecho, tipo = (b, c), 'sem dono declarado', 'errors'

funcoes = {}
exec(compile(fonte, f'<mutante {caso}>', 'exec'), vars(fechamento), funcoes)
classe = f'{TESTES}.FeriadoUmJuizLavraturaSemBancoTest' [nao verificado]
with patch.object(fechamento, 'recalcular_fechamento_mes', funcoes['recalcular_fechamento_mes']):
    resultado = _rodar(*(f'{classe}.{nome}' for nome in nomes))

[(teste, trilha)] = getattr(resultado, tipo) [nao verificado]
assert teste._testMethodName == nomes[-1] and trecho in trilha, trilha [nao verificado]
assert not getattr(resultado, 'errors' if tipo == 'failures' else 'failures')
PY
done
```

## 27/11 23:36 · sessao cca906a30392 · FERIADO-L097-MOTOR · verificador · origem=Code

## 1. Achados do ver1

| Achado | Situação em `dc876f5d1` |
|---|---|
| **1. Guarda e partição julgam dias e vínculos diferentes** | **Parcial.** Com escala certa e sem decisão, c2 impede que o feriado em D libere os 240 min da folga comum de D+1. Ainda zera os 240 min do feriado em D+1 quando o par herda D. A pendência fica em D e o oráculo pode chamar o par colapsado de invertido. `app/ponto/motor_calculo_v2.py:1599–1619,2530–2539` · `app/ponto/calculador/regras.py:410–432` · `app/ponto/calculador/alimentacao.py:114–123` |
| **2. Exportada intocada** | **Não curado.** O TXT registrado preserva conteúdo e hash. O recálculo por evento ainda pode regravar `FechamentoMensal` e `DiaPago` de uma competência entregue fora da guarda, que contém somente 09/2026. A exceção do motor também alcança a partição histórica. Em memória, um par de folga certa em feriado de 12/10, com janela ativa, passou de 0 para 240 min nessa partição. O teste de outubro entregue usa `LIMBO`, que já escapava do bloqueio. Efeito em exportada real, lista nominal da comp. 11 J.A. no RELATO e reversão [nao verificado]. `aval.txt:1` · `app/folha/models.py:174–186` · `app/ponto/services/fechamento.py:37–51,603–629,1197–1240,1847–1853` · `app/ponto/calculador/regras.py:393–409` · `app/ponto/tests/test_feriado_um_juiz_porta.py:239–289` |
| **3. REDs deixam passar falhas da testemunha** | **Parcial.** Os novos testes exigem a pendência no motor e no espelho e conferem um segmento de 240 min, além do contador de bloqueio. Essas asserções usam feriado e folga comum separados por dois dias. Nos dois REDs da virada em D, a pendência é conferida por hora e minutos, sem conferir sua data. O rótulo do oráculo na virada continua sem asserção. O caso anterior 20/21 com troca continua `expectedFailure`. `app/ponto/tests/test_feriado_um_juiz_porta.py:837–862,895–936` · `app/ponto/tests/test_feriado_um_juiz.py:942–959` |

## 2. Dia e vínculo do mesmo par

**Sim, a guarda e a partição chamam `_dia_do_turno_do_par`.** O auxiliar lê o mapa do juiz pela entrada crua localizada e usa a data civil local se a chave faltar. A guarda usa a resposta só para exigir igualdade com o `dia` recebido. Antes disso, já consultou a janela, o previsto, a autorização e a certeza nesse `dia`. A partição pode trocar a data do par e usar motor e vínculo de outra fatia. Essa troca é condicional. Na partição histórica, o auxiliar sequer é chamado. `app/ponto/motor_calculo_v2.py:963–978,1554–1601` · `app/ponto/calculador/regras.py:393–432` · `app/ponto/services/fechamento.py:330–351`

Reproduzi os casos abaixo em memória com o pareador, o motor, a composição por fatia, a partição e o leitor de segmentos da cópia. Cadastro, vigência da janela e decisões foram alimentados em memória. Execução da suíte Django e frequência em produção [nao verificado]. `app/ponto/turnos.py:1517–1535` · `app/ponto/services/fechamento.py:297–376` · `app/ponto/calculador/alimentacao.py:73–123`

| Caso | Resultado em c2 |
|---|---|
| **Um par `D 22:00 → D+1 02:00`, 6x1** | Com feriado em D, preserva 240 min em D. Com feriado só em D+1, o par continua em D e fica pendente como folga comum. No teste `21:00–00:00 / 01:00–05:00`, a sonda obteve 420 min em D com **um período com pausa**. O teste exige o total, sem exigir dois períodos. `app/ponto/motor_calculo_v2.py:963–978,1599–1619,3370–3377` · `app/ponto/turnos.py:430–459` · `app/ponto/tests/test_feriado_um_juiz_porta.py:886–893` |
| **Dois turnos `D 18:00–22:00` e `D+1 03:00–07:00`, feriado em D** | Em 6x1 → 6x1 e 6x1 → 12x36, D paga 240 min de HE100-feriado. A guarda bloqueia o segundo par e publica `(D, 03:00, 240)`. A partição põe esse par zerado em D+1, usando o vínculo de D+1 na troca. O oráculo conta `par_invertido_pela_janela`. `app/ponto/motor_calculo_v2.py:1599–1619,2533–2539` · `app/ponto/calculador/regras.py:410–432` · `app/ponto/calculador/alimentacao.py:114–123` |
| **Mesmos turnos, feriado em D+1** | Em 6x1 → 6x1 e 12x36 → 6x1, ambos ficam em zero e pendentes em D. A partição move o segundo, já zerado, para o feriado de D+1. Em 6x1 → 12x36, o pagamento zero é o controle esperado, mas a pendência de `03:00` também fica em D. Com feriado em D e troca 12x36 → 6x1, a partição pode conservar os dois pares em D pela condição de remapeamento. `app/ponto/motor_calculo_v2.py:1599–1619,2533–2539` · `app/ponto/calculador/regras.py:410–432` · `app/ponto/tests/test_feriado_um_juiz_porta.py:843–884` |
| **Limite de 6 h e autorização** | Com saída em D às `21:00`, entrada às `03:00` ainda herda D. Às `03:01`, a segunda entrada conserva D+1 e, sendo feriado 6x1, paga os 240 min. No caso de 5 h com feriado em D, autorizar somente D libera também a folga comum de D+1. Autorizar somente D+1 deixa o par bloqueado em D. Escala incerta só em D também pode liberar o par antes do novo `if`, mesmo com escala certa em D+1. `app/ponto/motor_calculo_v2.py:1565–1601,2533–2539` · `app/ponto/calculador/regras.py:410–432` |

## 3. Alcance do diff

| Arquivo | Hunks | Linhas no destino | Saldo |
|---|---:|---|---:|
| `app/ponto/motor_calculo_v2.py` | 1 | 1585–1601 | +17 / −0 |
| `app/ponto/tests/test_feriado_um_juiz_porta.py` | 3 | 547–548, 566–650, 773–936 | +251 / −1 |
| **Total** | **4** | | **+268 / −1** |

O único hunk do motor insere após a linha-base 1584, dentro do bloco de escala certa autorizado. Pela leitura literal da numeração **no destino**, 11 linhas novas ficam após a 1590, incluindo o `if` e o `return` em 1599–1601. Nenhum outro bloco do motor aparece no diff. Os blobs dos dois arquivos conferiram antes e depois da reversão em memória. A cópia não tem metadados Git para comprovar ancestralidade ou ordem de pouso [nao verificado]. `aval.txt:1` · `obra.diff:1–28` · `app/ponto/motor_calculo_v2.py:1579–1602`

## 4. Escritores, leitores e significado

| Valor | Quem escreve, quem lê e qual dia usa |
|---|---|
| **Dia, vínculo, ciclo e feriado** | O motor monta o mapa do turno por instante de entrada e o laço atribui `data_jornada`, com herança de até 6 h. O fechamento alimenta a escala do motor e o calendário por dia e vínculo, e guarda os contextos por dia e por par. A guarda lê o ciclo da própria fatia e o feriado em `dia`. A partição lê esses contextos e pode reescrever `resultado.dia_da_jornada[id(p)]`. `app/ponto/motor_calculo_v2.py:815–819,894–922,1000–1011,1599–1600,2530–2539,2587–2591` · `app/core/feriados.py:159–167` · `app/ponto/services/fechamento.py:330–351,713–714,800–807` · `app/ponto/calculador/regras.py:410–432` |
| **Folga, previsto, certeza e janela** | O laço carimba `_folga_do_par` pelo vínculo e pelo dia herdado. A célula responde `trabalha`; `minutos_previstos_do_dia` fornece o previsto; `escala_certa_no_dia` lê vínculo, `LIMBO` e assinaturas da competência do dia. A guarda consulta esses valores em D no caso herdado. A partição pode consultar certeza e `feriado_vira_folga` em D+1. A vigência da janela vem do cadastro da empresa. `app/escala/models.py:1098–1107,1400–1406` · `app/escala/utils.py:2310–2325` · `app/escala/services/escala_certa.py:19–51` · `app/ponto/motor_calculo_v2.py:1486–1509,1562–1584,2575–2587` · `app/ponto/calculador/regras.py:428–432` |
| **Pendência e autorização** | O motor publica `{dia, hora, minutos}` com o **dia herdado**, a **hora local da entrada crua** e os minutos trabalhados arredondados. O espelho projeta `sentido='dia'`; retrato da Gestão de HE, PDF e portão do TXT leem a projeção. A view passa a data da tela, e `portas/he.py::_gravar` grava `DecisaoHE` por colaborador e data. O motor carrega `data → estado` das vigentes e libera pelo estado `autorizado`, sem usar os minutos da trilha como limite. `app/ponto/motor_calculo_v2.py:1511–1534,1569–1571,1602–1619,2850` · `app/ponto/services/espelho.py:757–763` · `app/ponto/services/he_pendente_lavrado.py:138–158,183–211` · `app/relatorios/pdf_espelho.py:934–935` · `app/folha/porta_export.py:183–201,687–697` · `app/ponto/views.py:542–550` · `app/ponto/portas/he.py:65–86,195–208` |
| **Segmentos e horas pagas** | `calcular_periodo` e `_carimbar_segmento` escrevem minutos, pontas efetivas e intervalos. O bloqueio zera trabalhado, noturno, extras e intra indenizada, soma os minutos a `fora_janela` e colapsa a saída. `segmentos_do_motor` lê esse mesmo objeto e o mapa da jornada, que pode já estar remapeado, e compara dia e hora efetiva com a pendência. O fechamento soma a partição em `FechamentoMensal`; `dia_pago.lavrar` grava as duas versões pela mesma porta. O oráculo lê os segmentos e recebe as rubricas-base de FT da própria partição. O TXT lê o mensal; espelho, calendário e PDF leem a lavra diária. `app/ponto/motor_calculo_v2.py:1602–1619,1964–1972,3370–3405` · `app/ponto/calculador/alimentacao.py:73–138,353–377,494–518` · `app/ponto/services/fechamento.py:995–1021,1197–1207,1235–1240,1296–1328` · `app/ponto/services/dia_pago.py:224–247,344–367,904–905` · `app/folha/export.py:434–462,734–745` · `app/ponto/services/espelho.py:819–831` · `app/colaboradores/services/calendario.py:684–689` · `app/relatorios/pdf_espelho.py:904–913` |

## 5. Classes de reprovação

- **2º juiz e derivação paralela — presentes na atribuição do dia.** O mapa do juiz e a herança de 6 h podem divergir. A partição ainda remapeia condicionalmente. A guarda reutiliza o calendário, o juiz de certeza e a constante registrada de ciclo. `app/ponto/motor_calculo_v2.py:1562–1601,2530–2539` · `app/ponto/calculador/regras.py:314–322,410–432` · `app/core/juizes.py:318–323`
- **2º escritor persistente, relógio fora da borda e `except` novo — ausentes nas adições de produção.** A única adição executável é o retorno condicional no motor. Mensal e lavras seguem pelas portas existentes. `app/ponto/motor_calculo_v2.py:1585–1601` · `app/ponto/services/fechamento.py:1197–1240,1296–1328`
- **Campo com dois significados — presente.** Para o mesmo `id(p)`, `dia_da_jornada` pode começar como o dia herdado usado para bloquear e terminar como o dia do turno usado para lavrar. A pendência conserva a primeira data. `app/ponto/motor_calculo_v2.py:1618–1619,2587–2591` · `app/ponto/calculador/regras.py:421–428`
- **RED sem mordida — parcial.** Repondo em memória a condição de c1 descrita no ver1, os dois REDs de feriado em D mordem: aparecem 240 min indevidos de FT e some a pendência de `03:00`, com ou sem troca 6x1 → 12x36. Os REDs do feriado isolado mordem a base sem a exceção, de 240 para 0 min, mas já passam nesse comportamento de c1. A cobertura do dia da pendência e do contador na virada continua aberta. Execução dos testes diretamente no commit c1 [nao verificado]. `ver1_r.md:1` · `app/ponto/tests/test_feriado_um_juiz_porta.py:549–605,837–862,903–936`

## 6. Os dois `expectedFailure`

**A expectativa de 4 h de HE100-feriado está correta pela regra fornecida.** Nos dois casos, o segundo turno começa em D+1, em feriado, com vínculo 6x1 que folga por causa do feriado. A falha de pagamento é real na sonda. Os testes só exigem totais mensais e o decorador também aceita uma falha anterior à asserção. Assim, podem encobrir a pendência lançada em D, o rótulo de par invertido e a falta de prova da lavra em D+1. `aval.txt:1` · `leis.txt:117` · `app/ponto/tests/test_feriado_um_juiz_porta.py:810–835,849–857,880–884` · `app/ponto/calculador/regras.py:410–432` · `app/ponto/calculador/alimentacao.py:114–123`

**REPROVADO** — **Bloqueantes** · feriado 6x1 ainda perde 240 min na virada e o recálculo de exportada fora da guarda pode regravar o pago. **Altos** · pendência e oráculo divergem na data do mesmo par, a autorização pode liberar pelo dia errado e a lista J.A. com reversão segue [nao verificado]. `app/ponto/motor_calculo_v2.py:1565–1619,2533–2539` · `app/ponto/calculador/regras.py:410–432` · `app/ponto/calculador/alimentacao.py:114–123` · `app/ponto/services/fechamento.py:603–629,1847–1853` · `aval.txt:1`

## 27/11 23:33 · sessao 2738f074ad57 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

Conferi a cadeia Git `73464f2fd` → `23c7758e4` → `59600163c` e comparei os dez arquivos do diff com os blobs da c2. Rodei o inventário e o disjuntor em memória, com entradas controladas. O resultado dos selos completos e da suite da VM para a c2 continua [nao verificado]. `diff.patch` `bin/codex_pergunta.sh:193–239` `bin/disjuntor.py:2082–2130`

## 1. Achados do opus, item a item

| Item | Situação na c2 |
|---|---|
| **1 · P4, inteira na pista** | **Parcial.** O aval aceita a inteira da VM com `cmp` entre a árvore testada e a que pousa. O anexo relata `Ran 18090`, `OK`, rc 0, mas a identidade de `opa/g` com a c2 é [nao verificado]. `aval.txt:3` `bin/suite.sh:118–144` |
| **2 · P2, GREEN sem rc** | **Não curado na evidência.** O selo prevê rc 0 para GREEN e rc 2 para falha. Os casos focais passaram em memória na c2. O rc 0 do selo completo, vinculado à c2, é [nao verificado]. `opus_c1.md:3` `bin/tests/test_agente_no_modelo.sh:326–329` `bin/tests/test_agente_no_modelo.sh:386–391` |
| **3 · Caminhos de teste e quarentena** | **Curado para os sete caminhos citados.** Os seis módulos e `app/core/testes_em_quarentena.txt` casam `p_teste`. Nos três casos novos do selo, o inventário mudou de rc 0 na c1 para rc 3 na c2. `bin/codex_pergunta.sh:176–180` `bin/tests/test_codex_pergunta.sh:423–425` |
| **4 · Três predicados** | **Parcial.** Wrapper e disjuntor leem a mesma regex. A guarda da suite e a classe B conservam critérios próprios. O texto da c2 ainda descreve a regra do wrapper como aquilo que o runner descobre, apesar dos contraexemplos abaixo. `bin/codex_pergunta.sh:176–180` `bin/disjuntor.py:2025–2046` `bin/suite.sh:261–281` `bin/selo_proporcao.py:110–128` |
| **5 · Um teste abre o opus para a linha inteira** | **Curado no caso do laudo.** `app/ponto/views.py` junto de teste passa na c1 e é recusado na c2. Se `bin/disjuntor.py` também estiver declarado, a outra tag continua abrindo a linha inteira, com teste explícito dessa conduta. Caminhos relativos a `app/`, como `ponto/views.py`, continuam sendo classificados como fora de `app/` se enviados assim no brief. `bin/disjuntor.py:2049–2064` `bin/disjuntor.py:2121–2130` `bin/codex_pergunta.sh:175` `bin/tests/test_agente_no_modelo.sh:140–150` |
| **6 · Leitores da regra antiga** | **Parcial.** L-126, PISTA, cabeçalho e mensagem do disjuntor receberam a emenda. O BACKLOG ainda registra `CODEX-ESCREVE-RED-WRAPPER` como trabalho a fazer. RAIAS 3 ainda enumera as permissões do opus sem a entrada de teste, em conflito com CODEX 2. `app/docs/LEIS.md:147` `app/docs/PISTA.md:16–18` `bin/codex_pergunta.sh:30–31` `bin/codex_pergunta.sh:651–652` `bin/disjuntor.py:2116–2129` `app/docs/BACKLOG.md:1137` `CLAUDE.md:325–330` `CLAUDE.md:402–405` |
| **7 · Referência numérica ao wrapper** | **Curado.** Os pontos citados passaram a usar `_INVENTARIO_PY` ou `PROIBIDOS`. A verificação textual acusa a troca da âncora para `:647`. `CLAUDE.md:404–405` `bin/disjuntor.py:1959–1963` `bin/tests/test_agente_no_modelo.sh:298–304` `bin/tests/test_agente_no_modelo.sh:371–373` `bin/tests/test_censo_antes_da_vaga.sh:201–203` |
| **8 · Hunk estranho em `CLAUDE.md`** | **Curado.** A c1 traz a inserção `OPUS-VERIFICA-EM-PARALELO` em `CLAUDE.md:397–398` da c1. A c2 restaura o texto da base nesse ponto. `CLAUDE.md:395–399` |
| **9 · Código operacional importa de `core.tests`** | **Não curado.** O comando `crons` continua lendo `marca_cron15`, e `tblib_ausente_na_imagem` continua importando `tblib_instalado` de um módulo de teste. Ambos os módulos importados continuam sob `p_teste`. O `crons.py:92` do laudo é o comando em `core/management/commands/`. `app/core/management/commands/crons.py:92–97` `app/chamados/management/commands/tblib_ausente_na_imagem.py:17–22` `bin/codex_pergunta.sh:168–180` |
| **10 · `test_vigia_ccm.py` e item 6 de mão dupla** | **Não curado no commit.** A árvore da c2 segue sem `test_vigia_ccm.py`. O segundo `git revert HEAD` segue no vigia e a linha 2180 do BACKLOG da c2 é de `CLEAR-NAO-PARA-A-NOITE`. O registro externo de mão dupla é [nao verificado]. `opus_c1.md:11` `app/core/esteira_vigia.py:1311–1317` `app/docs/BACKLOG.md:2180` |

## 2. `p_teste` e descoberta

- **Wrapper.** Classifica pelo caminho: componente `tests`, basename `test*.py` ou o arquivo de quarentena. O inventário aplica a regra a caminhos novos, alterados e removidos. `bin/codex_pergunta.sh:180` `bin/codex_pergunta.sh:193–239`
- **Guarda da suite.** `_suite_inteira` procura `test*.py` com `def test...` nos apps de `LABELS`, exclui quatro diretórios e usa o resultado para medir cobertura da seleção. Na c2 contei **2.463** basenames `test*.py` em `app/` e **2.456** arquivos pelo `grep` da guarda com as `LABELS` reais. **Zero** caminhos desses dois conjuntos escapam de `p_teste`. `bin/regua.sh:23` `bin/suite.sh:195` `bin/suite.sh:261–290` `bin/codex_pergunta.sh:180`
- **Django.** A suite passa as labels ao `manage.py test`, e `RunnerComQuarentena` delega a descoberta ao `DiscoverRunner` antes de podar por ID de teste ou de classe. As labels explícitas aceitam módulos pontuados sem exigir prefixo `test`. Um módulo fora da convenção com casos selecionados por label pode ficar fora de `p_teste`. Um arquivo existente da c2 que o Django efetivamente descubra como teste e que passe pelo wrapper é [nao verificado]. `bin/suite.sh:219–234` `bin/suite.sh:569–570` `app/config/settings/ci.py:40` `app/core/runner_quarentena.py:36–38` `app/core/runner_quarentena.py:72–91`
- **Falsos positivos concretos.** `app/ponto/management/commands/testar_pwa.py` é um comando de ensaio e fica fora do `grep` da suite. `app/config/settings/teste_motor.py` é configuração do ensaio do control-plane. Ambos recebem rc 3 pelo nome. Já `app/tenants/tests.py` contém casos reais e roda no pre-push do control-plane, fora das `LABELS` da suite principal. `app/ponto/management/commands/testar_pwa.py:31–40` `app/config/settings/teste_motor.py:1–18` `app/tenants/tests.py:5–8` `bin/pre-push.sh:198–199` `bin/codex_pergunta.sh:176–180`
- **Controle de teste ainda editável.** O inventário permite `app/config/settings/ci.py`, `app/core/runner_quarentena.py` e `app/core/quarentena_lista.py`. Eles escolhem o runner, a poda e a interpretação da lista. A c2 protege o arquivo de dados da quarentena, enquanto esses três caminhos continuam podendo mudar quais casos rodam. `bin/codex_pergunta.sh:162–181` `app/config/settings/ci.py:38–40` `app/core/runner_quarentena.py:75–96` `app/core/quarentena_lista.py:9–28`

## 3. Escritores, leitores e significado

| Valor | Quem escreve, quem lê e significado |
|---|---|
| **Caminho, regex, tag e motivo de recusa** | O executor produz mudanças na cópia. O inventário deriva os caminhos do Git e do FS e aplica `PROIBIDOS`. O disjuntor extrai `{tag: regex}` do wrapper e escolhe as sete tags de admissão do opus. A regex é compartilhada, mas o inventário recebe caminhos reais relativos à raiz e o disjuntor recebe texto declarado, com normalização parcial. `bin/codex_pergunta.sh:162–239` `bin/codex_pergunta.sh:781–790` `bin/disjuntor.py:1963` `bin/disjuntor.py:2025–2064` |
| **`PAPEL`, `model`, `ARQUIVOS:` e decisão** | Quem despacha declara o papel, o modelo e os arquivos. `agente_nega` lê a declaração, confere o modelo com a sessão e retorna motivo ou `None`. `PreToolUse` consome a decisão. O selo de agente e o de censo montam entradas sintéticas para a mesma porta. `CLAUDE.md:325–331` `bin/disjuntor.py:2004–2009` `bin/disjuntor.py:2082–2130` `bin/disjuntor.py:2280–2290` `bin/tests/test_agente_no_modelo.sh:59–76` `bin/tests/test_censo_antes_da_vaga.sh:201–210` |
| **Cabeçalho, inventário e rc** | O wrapper escreve o cabeçalho lido pelo Codex. O inventário escreve `TOCADO`/`PROIBIDO` e o wrapper grava `.inventario`, publica `.recusa` e o rc final. O selo lê o motivo, `.rc` e a ausência de `.id`. Aqui rc 3 é recusa do construtor. Na suite, rc 3 é reservado a run que não nasceu. `bin/codex_pergunta.sh:644–652` `bin/codex_pergunta.sh:785–790` `bin/codex_pergunta.sh:573–585` `bin/codex_pergunta.sh:812–836` `bin/tests/test_codex_pergunta.sh:153–162` `bin/suite.sh:146–149` |
| **IDs, `ate` e motivo da quarentena** | O vigia formata e grava entradas e retira as que voltam a verde. O integrador chama essas portas e lê a lista. O parser é compartilhado com o runner e o placar. Para a poda, vale a presença do ID ou da classe. `ate` é o prazo de cobrança do vigia, embora o docstring do escritor descreva o prazo como limite para o teste ficar fora. `app/core/esteira_vigia.py:1096–1103` `app/core/esteira_vigia.py:1234–1275` `app/core/integrador_lote.py:665–668` `app/core/integrador_lote.py:775–786` `app/core/quarentena_lista.py:9–28` `app/core/runner_quarentena.py:36–38` `app/core/runner_quarentena.py:75–95` `bin/esteira_status.sh:90–92` |
| **Seleção e classe B** | `regua.sh` declara `LABELS`. A suite lê a lista, recebe `ONLY` do chamador ou de `vizinhos_da_volta.py` e usa a cobertura para exigir motivo de inteira. O selo de proporção lê o diff Git e as listas de `CORTES.md` da base. Sua categoria `teste` exige pasta `tests/` **e** basename `test_*.py`. Cada saída tem uma decisão própria e a c2 só comenta as duas regras. `bin/regua.sh:23` `bin/suite.sh:195–216` `bin/suite.sh:261–299` `bin/selo_proporcao.py:110–128` `bin/selo_proporcao.py:151–174` |

## 4. Classes que já reprovaram obra

- **Segundo juiz registrado e segundo escritor fora da porta — ausentes no diff.** A recusa continua no inventário, a admissão continua em `agente_nega` e os escritores da quarentena são preexistentes. `app/core/juizes.py:190` `bin/codex_pergunta.sh:229–239` `bin/disjuntor.py:2280–2290` `app/core/esteira_vigia.py:1234–1264`
- **Relógio fora da borda e ordem de trava — ausentes no diff.** A suite ainda confere a árvore sob a trava antes de executar. O relógio lido no import de `marca_cron15.py` é preexistente. `bin/suite.sh:133–146` `bin/suite.sh:559–570` `bin/trava_teste.sh:35–42` `app/core/tests/marca_cron15.py:52–57`
- **Derivação paralela e dois significados — presente na equivalência declarada.** A admissão lê a regex da fonte. Já a documentação chama de descoberta a regra de propriedade por caminho. `ARQUIVOS:` também assume caminhos da raiz, enquanto o inventário recebe essa forma por construção. `CLAUDE.md:380–382` `bin/disjuntor.py:2025–2064` `bin/codex_pergunta.sh:208–230` `bin/suite.sh:261–281`
- **RED sem mordida — ausente nos casos focais, presente na checagem textual.** Em memória, c1 → c2 deu rc **0 → 3** para `ferias/tests.py`, `comunicados/tests_x.py` e a quarentena, e **aceita → recusa** para `views.py` junto de teste. As 11 linhas de mutação de `p_teste` inverteram o inventário de 3 para 0 nos casos correspondentes. Os dois mutantes da linha mista inverteram a decisão do disjuntor. Já retirar a menção a arquivos de teste de CODEX 2 deixou as asserções textuais verdes. Execução integral dos selos [nao verificado]. `bin/tests/test_codex_pergunta.sh:417–425` `bin/tests/test_codex_pergunta.sh:669–679` `bin/tests/test_agente_no_modelo.sh:298–310` `bin/tests/test_agente_no_modelo.sh:374–375`
- **Pouso antes do pré-requisito — ausente no diff, ordem operacional [nao verificado].** O contrato exige inteira da árvore antes do opus final. O aval da VM exige identidade com o run. A exceção de selos da base depende de falha demonstrada na base e registro no BACKLOG. A comparação dos selos e o registro exigido para eventuais falhas, inclusive `n_vagas`, são [nao verificado]. `CLAUDE.md:422–432` `aval.txt:3` `aval.txt:8` `app/docs/BACKLOG.md:2156`
- **`except` novo — 0** no diff inteiro, tanto base → c2 quanto c1 → c2. `diff.patch` `bin/codex_pergunta.sh:223–225` `bin/disjuntor.py:2025–2046`

## 5. Sítios tocados

O diff altera `CLAUDE.md`, `app/docs/LEIS.md`, `app/docs/PISTA.md`, `bin/codex_pergunta.sh`, `bin/disjuntor.py`, `bin/selo_proporcao.py`, `bin/suite.sh` e os três selos `test_agente_no_modelo.sh`, `test_censo_antes_da_vaga.sh` e `test_codex_pergunta.sh`. **Zero alterações** em `app/ponto`, folha, porta da célula, export ou `bin/gerar_estado.py`. `diff.patch` `CLAUDE.md:380` `app/docs/LEIS.md:147` `app/docs/PISTA.md:16` `bin/codex_pergunta.sh:180` `bin/disjuntor.py:2121` `bin/selo_proporcao.py:125` `bin/suite.sh:272` `bin/tests/test_agente_no_modelo.sh:138` `bin/tests/test_censo_antes_da_vaga.sh:203` `bin/tests/test_codex_pergunta.sh:417`

**REPROVADO — bloqueantes** GREEN integral e identidade `opa/g` ↔ c2 [nao verificado] · **altos** equivalência com a descoberta não cumprida e controle do runner editável (`bin/codex_pergunta.sh:180`, `bin/suite.sh:280–281`, `app/core/runner_quarentena.py:75–91`) · **médios** leitores desatualizados, diferença de caminhos no brief e selo textual sem mordida para a cláusula nova (`CLAUDE.md:329`, `app/docs/BACKLOG.md:1137`, `bin/disjuntor.py:2049–2064`, `bin/tests/test_agente_no_modelo.sh:298–310`) · **baixos** dependências operacionais em `core.tests` e pendência do vigia (`app/core/management/commands/crons.py:92`, `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`, `app/core/esteira_vigia.py:1316`).

## 27/11 23:27 · sessao 739aeb1477ca · PISTA3-NA-ESTEIRA · leitura · origem=Code

## (a) Onde o repo assume um host só

| Sítio | Efeito com `pista` e `pista3` |
|---|---|
| **`bin/suite.sh` · sondagem e fila** | `ping`, `estado` e a coleta dos recibos históricos miram `pista`. Se esse host falha ou devolve estado ilegível, a rota vai direto à VM. O parser do `ping` também exige o prefixo literal `pista ok`. A admissão, a estimativa e a linha de diagnóstico fixam duas vagas. `bin/suite.sh:378,379,381,382,388,396,397,403,413,414,415` |
| **`bin/suite.sh` · pedido e coleta** | `pede`, `espera` e `log` miram `pista`. Se só o envio mudar, a coleta consulta o host errado. A linha de aceite, a de fim e os erros identificam o job sem host. A rota tem chamadas de espera e log, e o cancelamento documentado é da sessão, via SSH. `bin/suite.sh:445,452,459,471,477,481,485,497` `app/docs/RELATO.md:306,3625` |
| **`bin/disjuntor.py`** | Há três pontos que fixam `pista`: a regex de laço sobre `estado`, a regex de `espera` direto e a extração da chave de polling. A mensagem de recusa também imprime `ssh pista espera` literalmente. `bin/disjuntor.py:1974,1975,2132,2133,2134,2136,2137,2165,2166` |
| **`bin/tests/test_suite_rota_pista.sh`** | O SSH falso procura o argumento `pista`, descarta o host ao registrar a chamada e usa um modo de falha único. Uma chamada a `pista3` cai no caso de comando desconhecido, e as asserções de `espera` e `log` conferem só o ID. Recorte, inteira pre-opus e foto suja também exigem o formato antigo da linha `ROTA PISTA`. `bin/tests/test_suite_rota_pista.sh:40,42,43,44,47,126,146,160,162,163,165,180,183,264,299,300,392` |
| **`bin/pista_certifica.sh` e seu selo** | Os exemplos de coleta fixam `ssh pista`, e a mensagem de OOM nomeia `srv-glpi`. O certificador recebe dois arquivos locais e compara ID e SHA, sem conferir a origem por host. O selo fabrica os arquivos, sem SSH, e cobre ID diferente. Se o mesmo ID e SHA existirem nos dois hosts, misturar os arquivos pode passar pela checagem de procedência. `bin/pista_certifica.sh:13,14,15,16,22,29,48,54,55,60` `bin/tests/test_pista_certifica.sh:41,48,50,57,120` |

A instrução operacional em `CLAUDE.md` descreve a rota e o formato antigos, nomeia `ssh pista` para certificar e limita as regras de espera e polling a esse alias. O contrato anterior no RELATO ainda mede IDs por um único `estado`. Seguir esses comandos para um job da `pista3` consulta o host errado ou deixa esse job fora da contagem. `CLAUDE.md:100,101,129,337,339,340,346` `app/docs/RELATO.md:197,199`

As demais ocorrências literais de `ssh pista` estão nos registros e exemplos abaixo. Servem para os jobs ali registrados. Reusá-las como instruções para todo o pool consultaria apenas `pista`. `app/docs/BACKLOG.md:2375,2376` `app/docs/CORTES.md:567` `app/docs/CORTES.json:1823` `app/docs/PROMPTS.md:2308,2975,2988,3055,3057` `app/docs/RELATO.md:159,201,306,367,517,773,953,961,1190,1267,1754,1843,1929,2081,2269,2276,2531,2636,2872,3188,3545,3624,3697`

## (b) O que falta fechar no contrato

- **Queda após aceite com ID recebido:** o item 3 cobre o destino, `ERRO-INFRA` rc 71. O código atual já faz isso quando perde `espera` ou `log`, mas ainda cai na VM em duas variantes de rc 71 classificado como recusa sem execução, depois de imprimir o aceite. O “como hoje” deixa essa exceção ambígua. Também falta um prazo para o caso em que o SSH continua respondendo e o job fica preso. Os keepalives verificam a conexão, e `--espera` limita a estimativa anterior ao envio. `bin/suite.sh:391,415,452,458,459,468,470,471,476,481,485` `bin/tests/test_suite_rota_pista.sh:206,209,210,302,314,375`
- **Queda no `pede` antes de o ID chegar:** hoje um ID válido já conta como aceite, mesmo com erro do SSH. Sem ID, a rota cai na VM. Se o servidor admitiu o job e a resposta se perdeu, o contrato deixa indefinido como distinguir esse caso de recusa certa antes de reenviar ao outro host ou à VM. `bin/suite.sh:445,446,447,449,450` `bin/tests/test_suite_rota_pista.sh:57,59,60,368,369,370`
- **ID igual e cancelamento errado:** “usar esse host” cobre o caminho normal. Falta explicitar que **`(host, id)` identifica o job** também na retomada, na coleta para certificação, na contagem e no cancelamento. O ID aceito localmente tem dez dígitos e oito caracteres do SHA. Com o mesmo ID e SHA nos dois hosts, o certificador não distingue arquivos cruzados. O contrato histórico de `cancela` prevê rc 2 para ID inexistente. Se o ID existe no host errado, há risco de cancelar outro job. `bin/suite.sh:446,452,460,497` `bin/pista_certifica.sh:48,54,55` `app/docs/RELATO.md:199,3625`
- **Fila por host:** falta definir o que ocorre quando nenhum host elegível tem vaga, inclusive com um fora e o outro cheio, ou quando duas chamadas enxergam a mesma última vaga entre `estado` e `pede`. A regra antiga ainda admite enfileirar abaixo do limite estimado. Se a estimativa continuar valendo, ela precisa ter relógio, recibos e capacidade do próprio host. Hoje esses dados vêm de `pista` e o divisor é fixo em dois. `bin/suite.sh:378,382,384,396,403,409,413,415,445`
- **Prova de pronto:** falta atribuir quem pede a inteira de pouso na `pista3` e registra a origem dos dois arquivos. A rota atual exclui `pouso`, e `pista_certifica.sh` rc 0 prova o conteúdo dos arquivos sem provar o host que os produziu. `bin/suite.sh:500,502,504` `bin/pista_certifica.sh:22,48,54,55,94,95`

## (c) O disjuntor reconhece `pista3`?

**Não.** Na sessão principal, `polling_nega` não conta `ssh pista3 estado`, porque o extrator exige a palavra exata `pista`. `espera_nega` não classifica `ssh pista3 espera` como espera por alvo único, nem um laço com `sleep` que só consulta `ssh pista3 estado` como espera de obra. A coleta redirecionada da `pista3` passa por ausência de reconhecimento. Para `pista`, ela passa pela exceção explícita de recibo. `bin/disjuntor.py:1971,1974,1975,1977,2132,2137,2165,2167,2180,2182,2225,2226` `bin/tests/test_agente_no_modelo.sh:191,204,206,228,229`

Hash Git da cópia, comportamento dos lançadores remotos e disponibilidade real dos dois hosts [nao verificado].

## 27/11 23:19 · sessao 9b5089c5b761 · FERIADO-UM-JUIZ · verificador · origem=Code

# REPROVADO

1. **Uma falha técnica do oráculo vira recusa contada, inclusive na revisão com `propagar_falha=True`.** O mesmo `except Exception` recebe a falta de dono, um erro de alimentação e uma falha de escrita. Todos incrementam `recusas_oraculo`, ficam no log e deixam o colaborador contar como processado se a lavra `motor` passou. O teste de falha SQL injeta esse caminho e exige retorno `1` e contador `1`. `app/ponto/services/fechamento.py:1296` `app/ponto/services/fechamento.py:1329` `app/ponto/services/fechamento.py:1334` `app/ponto/tests/test_feriado_um_juiz.py:461`
2. **A c10 passou a recusar a sombra no caso válido de saída antecipada do ver7.** Para `06:00 E / 06:30 S`, a escala `07:00–15:00` produz zero trabalhado e **420 min de saída antecipada**. A base esperava as duas lavras. O R3 atual exige recusa da sombra e preserva os 420 min no mensal e no `motor`. Reexecutei também o dia fechado de **7 h** isolado e junto ao BUG-144. Isolado, atualizou as duas versões. Junto ao dia aberto, atualizou o `motor`, e a sombra inteira ficou na versão anterior, inclusive para o dia fechado. `597fdc40d:app/ponto/tests/test_feriado_um_juiz.py:248` `app/ponto/tests/test_feriado_um_juiz.py:351` `app/ponto/tests/test_feriado_um_juiz.py:383` `app/ponto/tests/test_feriado_um_juiz.py:475` `app/ponto/services/dia_pago.py:317`

## Ver7 e fila do Opus

| Item | Resultado |
|---|---|
| **Ver7-1 — rollback do fechamento válido e dependência de outro dia** | **Curado para o mensal e o `motor`.** O R1 mantém **357 min / 5,95 h** e retorna `1`. O R2 mantém o mesmo valor com outro dia no mapa e acusa a falta de dono na data certa. O savepoint próprio começa antes da alimentação do oráculo. `app/ponto/services/fechamento.py:1296` `app/ponto/services/dia_pago.py:317` `app/ponto/tests/test_feriado_um_juiz.py:340` `app/ponto/tests/test_feriado_um_juiz.py:351` |
| **Ver7-2 — literal de pontualidade e RED sem mordida** | **Curado na guarda e no RED.** A guarda deriva os campos preenchidos de `CAMPOS_DIA`. Reexecutei M3 e M3_literal. O R3 falhou tanto ao limitar a guarda a `horas_trabalhadas` quanto ao recolocar a exclusão literal. O caso funcional de saída antecipada é o bloqueio 2. `app/ponto/services/dia_pago.py:258` `app/ponto/tests/test_feriado_um_juiz.py:383` `app/ponto/tests/feriado_um_juiz_mutantes.py:506` |
| **Opus (a) — default morto** | **Curado.** A presença da chave determina a exigência de todos os campos. Sem a chave, o laço já garante uma entrada em `campos_dos_periodos`, acessada diretamente. `app/ponto/services/dia_pago.py:318` |
| **Opus (b) — `do_motor` nos dois ramos** | **Curado.** A mesma chamada recebe `do_motor` com ou sem entrada em `rubricas`. Reexecutei o mutante que o retira no segundo caso, e o RED falhou nesse subcaso. `app/ponto/services/dia_pago.py:321` `app/ponto/tests/test_feriado_um_juiz.py:418` `app/ponto/tests/feriado_um_juiz_mutantes.py:172` |
| **Opus (c) — RED de dia só com FT** | **Curado.** A coleta ocorre depois dos três laços de FT. Reexecutei o mutante que deixa esses dias fora, e o RED falhou nos três subcasos, FT certa, FT feriado e FT sem escala. `app/ponto/services/dia_pago.py:224` `app/ponto/services/dia_pago.py:234` `app/ponto/services/dia_pago.py:248` `app/ponto/services/dia_pago.py:258` `app/ponto/tests/test_feriado_um_juiz.py:395` `app/ponto/tests/feriado_um_juiz_mutantes.py:167` |

## Quem escreve e quem lê

| Campo ou valor | Escritor, leitor e significado |
|---|---|
| `tentativas_oraculo`, `recusas_oraculo`, `processados` | `recalcular_fechamento_mes` inicializa os três por chamada. Conta uma tentativa por colaborador que chega à sombra, a partir de 10/2026, e uma recusa por exceção capturada nesse bloco. O log lê o acumulado no erro individual e publica o resumo se houve tentativa. Os testes conferem o resumo. O SaaS encaminha o logger para `/app/logs/ponto.log`. Leitor automatizado do contador em produção **[nao verificado]**. Tela e comando de recálculo leem `processados`, que mede o sucesso da lavra paga nesse caminho. `app/ponto/services/fechamento.py:680` `app/ponto/services/fechamento.py:1293` `app/ponto/services/fechamento.py:1329` `app/ponto/services/fechamento.py:1347` `app/ponto/tests/test_feriado_um_juiz.py:334` `app/config/settings/saas.py:160` `app/config/settings/saas.py:181` `app/ponto/views.py:1280` `app/ponto/management/commands/recalcular_fechamento.py:130` |
| `propagar_falha` | O padrão é `False`. `recalcular_na_revisao` passa `True` para cada competência elegível. O serviço lê a flag na falha da lavra `motor` e no `except` externo. A falha da sombra segue o bloqueio 1. O teste da revisão confere a flag em julho e agosto. `app/ponto/services/fechamento.py:513` `app/ponto/services/fechamento.py:1274` `app/ponto/services/fechamento.py:1336` `app/ponto/services/fechamento.py:1888` `app/ponto/tests/test_revisar_par_relampago.py:163` |
| Donos declarados por dia | `_DONOS_DECLARADOS` lê as chaves de `valores` e `nao_decide` da data e acrescenta os mapas fornecidos de falta e grade e os campos de `do_motor`. `lavrar` usa o conjunto antes de qualquer escrita. **Há uma diferença de contrato.** O DIFF lê `nao_decide` como rubrica sem afirmação do calculador. A guarda aceita a chave como declaração de dono. Em chamada direta em memória, confirmei que `horas_falta` em `NAO_DECIDE` passa e grava `0.0` mesmo sem `faltas_por_dia`. O fechamento fornece esse mapa explicitamente. `app/ponto/services/dia_pago.py:50` `app/ponto/services/dia_pago.py:166` `app/ponto/services/dia_pago.py:317` `app/ponto/calculador/regras.py:532` `app/ponto/management/commands/diff_calculador.py:508` `app/ponto/services/fechamento.py:946` `app/ponto/services/fechamento.py:1313` |
| `do_motor` | O chamador nomeia os campos que devem conservar o valor já montado na linha. A guarda aceita esses campos e a sobreposição os preserva. As duas chamadas do fechamento usam o padrão `()`. O RED declara `horas_extras_100` explicitamente. `app/ponto/services/dia_pago.py:69` `app/ponto/services/dia_pago.py:140` `app/ponto/services/dia_pago.py:322` `app/ponto/services/dia_pago.py:328` `app/ponto/services/fechamento.py:1237` `app/ponto/services/fechamento.py:1309` `app/ponto/tests/test_feriado_um_juiz.py:424` |
| `CAMPOS_DIA` e valores lavrados | `dia_pago.py` declara os 15 campos. `lavrar` monta horas de períodos e FT dividindo os minutos por 60, incorpora atraso líquido, órfãos e faltas por seus mapas e mantém a grade em minutos. DSR e banco vão no ajuste da competência. O agregador do intervalo, o relatório e os leitores do calendário e da folha consultam o gravado. A HE100 de feriado e os splits noturnos são parcelas dos totais correspondentes. `app/ponto/services/dia_pago.py:34` `app/ponto/services/dia_pago.py:172` `app/ponto/services/dia_pago.py:224` `app/ponto/services/dia_pago.py:266` `app/ponto/services/dia_pago.py:292` `app/ponto/services/dia_pago.py:354` `app/ponto/services/dia_pago.py:791` `app/relatorios/views.py:547` `app/colaboradores/services/calendario.py:319` `app/folha/porta_export.py:290` `app/folha/export.py:434` |
| `rubricas` do dia | `rubricas_por_dia` monta `{date: Rubricas}` e `do_dia` produz `valores` em horas, `nao_decide` com causas e `detalhe` para o DIFF. O fechamento passa o mapa à lavra da sombra. `lavrar` substitui os campos decididos e grava as chaves de `nao_decide` no detalhe. `diff_calculador` usa a mesma alimentação e lê os valores e as causas para comparação. `app/ponto/calculador/alimentacao.py:571` `app/ponto/calculador/regras.py:663` `app/ponto/calculador/regras.py:962` `app/ponto/services/fechamento.py:1320` `app/ponto/services/dia_pago.py:326` `app/ponto/management/commands/diff_calculador.py:406` `app/ponto/management/commands/diff_calculador.py:460` `app/ponto/management/commands/diff_calculador.py:508` |

Os períodos, a FT e as rubricas usam o dia da jornada do motor quando há mapa. **O atraso de ligação ainda agrupa pela data civil da entrada do período**, e o O85 também usa data civil. A pontualidade do calculador lê o mapa da jornada. Portanto as chaves podem divergir após a meia-noite. Efeito em produção **[nao verificado]**. As janelas de datas usadas pela alimentação e pela soma lavrada incluem as duas pontas. No corte 21, a competência vai de 21 do mês anterior a 20 do mês de referência. `app/ponto/services/dia_pago.py:109` `app/ponto/calculador/alimentacao.py:73` `app/ponto/calculador/alimentacao.py:259` `app/ponto/services/credito_parcial.py:73` `app/ponto/services/fechamento.py:905` `app/ponto/calculador/alimentacao.py:524` `app/ponto/services/dia_pago.py:791` `app/ponto/janelas.py:10`

## Savepoint, catraca e classes de reprovação

- **Atomicidade.** Mensal e `motor` ficam na transação externa. A guarda da sombra roda antes do `delete`, e `delete` e `bulk_create` são atômicos por versão. A recusa preserva o mensal e o `motor` completos e deixa a sombra anterior inteira, conforme o R7. O teste ORM cobre falha no `bulk_create`, depois do `delete`. O mutante que remove só o savepoint da alimentação é detectado pelo teste com transação simulada. A execução individual do teste ORM na VM está **[nao verificado]** pelo resumo agregado fornecido. `app/ponto/services/fechamento.py:927` `app/ponto/services/fechamento.py:1174` `app/ponto/services/fechamento.py:1197` `app/ponto/services/fechamento.py:1296` `app/ponto/services/dia_pago.py:317` `app/ponto/services/dia_pago.py:365` `app/ponto/tests/test_feriado_um_juiz.py:199` `app/ponto/tests/test_feriado_um_juiz.py:475` `app/ponto/tests/test_feriado_um_juiz_porta.py:486` `app/ponto/tests/feriado_um_juiz_mutantes.py:494`
- **C12.** Conferi as chamadas reais por AST. O registro de `recalcular_fechamento_mes` bate em **721, 723, 729, 749, 820, 859, 905 e 917**. `_so_a_data` bate em **1609** e `apurar_por_fatia` em **257**. Um B1 em função nova faz subir o contador. Um B1 em linha nova de função já contada muda o registro de linhas. Há um limite concreto. Em memória, aumentei de duas para quatro as chamadas `.date()` na linha 723 e o registro B1 ficou idêntico, pois o detector deduplica por linha. A c12 acertou o registro e herdou esse ponto cego. `app/core/tests/test_medida_turno_do_par.py:332` `app/core/tests/test_medida_turno_do_par.py:565` `app/core/tests/test_medida_turno_do_par.py:848` `app/core/medida_turno_do_par.py:217` `app/core/medida_turno_do_par.py:228` `app/ponto/services/fechamento.py:257` `app/ponto/services/fechamento.py:723` `app/ponto/services/fechamento.py:1609`
- **Juiz, escritor, relógio e travas.** O delta acrescenta a verificação de procedência. A classificação segue pela partição existente, e as duas versões escrevem pela mesma `lavrar`. O relógio do furo é lido uma vez na borda da passada, e as duas lavras recebem a mesma lista. A revisão mantém Empresa por PK → Colaborador → Batida por PK. O savepoint entra depois dessas travas. O degrau da sombra exige sucesso da lavra `motor` e competência a partir de 10/2026. A classificação de feriado do mensal tem seu degrau em 11/2026. `app/ponto/services/dia_pago.py:317` `app/ponto/services/dia_pago.py:365` `app/ponto/services/fechamento.py:680` `app/ponto/services/fechamento.py:997` `app/ponto/services/fechamento.py:1152` `app/ponto/services/fechamento.py:1262` `app/ponto/services/fechamento.py:1279` `app/ponto/services/fechamento.py:1293` `app/ponto/services/fechamento.py:1325` `app/ponto/services/par_relampago_na_lista.py:172` `app/ponto/services/par_relampago_na_lista.py:479` `app/folha/export.py:385`
- **Contagem e prova.** O delta acrescenta **0 cláusulas `except`** e remove **0**. O `except` da sombra já existia e mudou de efeito. A ocorrência textual acrescentada no mutante é uma string. Rodei **52 testes de controle**, com uma falha esperada, **41 estruturais** e oito comandos de mutação, todos com o resultado previsto. Os seis arquivos alterados batem com os hashes de saída do diff fornecido. A identidade da árvore inteira pelo Git, a execução individual dos testes na VM, o DIFF de frota e os efeitos em produção ficam **[nao verificado]**. `app/ponto/services/fechamento.py:1329` `app/ponto/tests/feriado_um_juiz_mutantes.py:496` `app/ponto/tests/feriado_um_juiz_mutantes.py:545` `app/ponto/tests/feriado_um_juiz_mutantes.py:716` `app/ponto/tests/test_feriado_um_juiz.py:942`

_8822 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
