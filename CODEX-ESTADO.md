# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 16:52** · publicado em **05/11/2026 16:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 16:52 · sessao 10093ac31db0 · BO-[nome]-0511 · censo · origem=Code

O censo fecha em **72 unidades deduplicadas: 68 agrupadas, 3 não classificadas e 1 excluída por data futura**. A unidade é pessoa+dia explicitado; sem data, conta uma ocorrência; pares de datas contam duas. Isso é contagem do texto fornecido, não resultado de nova consulta. [nao verificado]

**G1 reúne candidatos a divergência de apresentação; os anexos não comprovam qual tela produziu cada alerta.** Eles trazem vereditos, DiaPago e classificação do exportador, mas não trazem URL, resposta HTTP, `status_dia`, retrato gravado ou perfil usado na tela. [nao verificado]

Identidades preservadas: `col253` é **id 253**; `col253 do [nome]` é **id 407**; `col751 de [nome]` é **id 923**. O id 465 aparece nas sondas, mas não constitui ocorrência do relatório. A equivalência da cópia a `main` não pôde ser confirmada pelo Git neste diretório. [nao verificado]

**G1 — tela × cálculo**

Estas são as telas e funções reais que podem responder às alegações; atribuir uma delas a cada relato continua pendente de reprodução. [nao verificado]

| Tela | Onde nasce a indicação | Autoridade lida / LEI-AKITA 2 |
|---|---|---|
| Calendário do perfil | `calendario_ponto` chama `contexto_calendario`; o template pinta `dia.status`. `app/colaboradores/views.py:1935`; `app/colaboradores/services/calendario.py:663`; `app/templates/colaboradores/partials/_calendario_grade.html:69` | Lê `status_dos_dias_lavrados`; o antigo classificador próprio foi removido. `app/colaboradores/services/calendario.py:602`; `app/colaboradores/services/calendario.py:78` |
| Espelho web | `espelho_ponto` chama `espelho_do_colab` e depois `montar_semanas`, que preenche status e rótulo. `app/ponto/views.py:158`; `app/ponto/views.py:184` | `montar_semanas` lê D4 em lote; não escolhe o status. `app/ponto/services/espelho.py:167` |
| API do espelho | `api_espelho_v2`, rota `espelho/v2/`, entrega `status`, `status_dia`, `inconsistente` e `turno_aberto`. `app/api/urls.py:81`; `app/api/views.py:1559` | Lê D4 e traduz por `status_do_app`; `inconsistente` também inclui turno geometricamente aberto. `app/api/views.py:1495`; `app/api/views.py:1514`; `app/ponto/status_dia.py:89` |
| Retrato da folha | `retrato_folha` lê o retrato gravado; `desenho_do_led` fornece cor e rótulo. `app/folha/views_retrato.py:246`; `app/folha/views_retrato.py:280` | Não consulta o exportador ao abrir. `_balde` classifica a **linha gravada**; o lavrador monta a origem pelo exportador. `app/folha/retrato_folha.py:89`; `app/folha/retrato_folha.py:1120`; `app/folha/models.py:293` |
| Lista do fechamento | `montar_resumos` fornece `folha`, `pront` e `travas`; o template também exibe o estado administrativo. `app/ponto/services/fechamento_lista.py:40`; `app/templates/ponto/_fech_linha.html:39` | Entrada no TXT e travas leem `classificar_export`; prontidão lê `prontidao`. `app/ponto/services/fechamento_lista.py:70`; `app/ponto/services/fechamento_lista.py:82` |
| Painel lateral do fechamento | `painel_fechamento` lê `fechamento_lido`; o template mostra “Aprovado”, “T. Aberto” ou “Pendente”. `app/ponto/views.py:993`; `app/templates/ponto/partials/painel_fechamento.html:10` | “Pendente” é estado administrativo; não responde, sozinho, se entra no TXT. O exportador expressamente não usa esse estado como porta. `app/folha/export.py:829` |

O juiz de **status do dia** é `status_do_dia_lavrado`; o de **entrada no TXT** é `classificar_export`; prontidão é outra pergunta. São os endereços de `JUIZES.md`, respectivamente em `app/docs/JUIZES.md:88` e `app/docs/JUIZES.md:74`. Um `veredito=concorde` anexado não substitui a chamada atual de D4, que também considera andamento e disponibilidade da lavra. `app/ponto/status_dia.py:201`

Há uma distinção demonstrável no código: a mesma linha pode mostrar o tique de entrada na folha e o badge **“Pendente”**. O primeiro lê o juiz do TXT; o segundo lê o estado administrativo. Isso não basta para declarar violação da LEI-AKITA 2. `app/templates/ponto/_fech_linha.html:11`; `app/templates/ponto/_fech_linha.html:39`; `app/folha/export.py:777`

Para **col253/id253 e col283/id283**, os anexos mostram entrada nos dois meses, mas não mostram a indicação concreta de “folha bloqueada”. Não há prova de qual tela divergiu, de qual competência ela mostrava ou de seu carimbo. [nao verificado] O retrato possui leitor de frescor, usado pela tela, que permite confrontar essa hipótese sem alterar jornadas. `app/folha/views_retrato.py:202`; `app/folha/retrato_folha.py:1268`

A sonda abaixo fica **somente como texto**, para `manage.py shell` com conta de leitura. Ela chama views reais do calendário, espelho e painel; o endpoint real da API; os montadores reais da lista e do retrato; D4 e o exportador. Não foi executada aqui nem validada contra banco. [nao verificado]

O retrato é consultado pelas funções reais de leitura, pois abrir sua view com `empresa=` chama o escritor de preferência da UI. A sonda não chama esse caminho. `app/folha/views_retrato.py:150`; `app/folha/retrato_folha.py:915`

```bash
# Em app/, com conexão de leitura já configurada.
# Informar o ID de um usuário supervisor existente, sem alterar permissões.
SONDA_SUPERVISOR_ID=123 PYTHONDONTWRITEBYTECODE=1 python -B manage.py shell <<'PY'
import datetime as dt
import json
import os
from html.parser import HTMLParser
from unittest.mock import patch

from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.template.loader import render_to_string
from django.test import RequestFactory
from django.utils import timezone
from rest_framework.test import APIRequestFactory, force_authenticate

from colaboradores.models import Colaborador
from colaboradores import views as cv
from ponto import views as pv
from api import views as av
from folha.models import RetratoFolha
from folha.export import classificar_export
from folha.retrato_folha import linha_do_colab, desenho_do_led
from ponto.janelas import janela_fechamento
from ponto.status_dia import (
    status_do_dia_lavrado, status_dos_dias_lavrados,
)
from ponto.turnos import CargaDaJanela
from ponto.models import FechamentoMensal
from ponto.services.fechamento import (
    universo_da_competencia, periodo_trancado,
)
from ponto.services.fechamento_lista import montar_resumos

# IDs do JSONL. None significa data não fornecida: não inventar uma.
G1 = [
    (504, "2026-10-21"), (226, "2026-10-21"),
    (709, "2026-10-22"), (255, "2026-10-03"),
    (204, "2026-09-21"), (43, "2026-10-10"),
    (43, "2026-10-26"), (956, "2026-10-16"),
    (956, "2026-10-22"), (502, "2026-10-09"),
    (55, "2026-10-03"), (288, "2026-10-04"),
    (523, "2026-10-25"),
    (253, None), (283, None), (92, None),
    (235, None), (219, None),
]
ANO = 2026
# Fixa somente o relógio; não reconstrói um banco histórico.
AGORA = dt.datetime.fromisoformat("2026-11-05T19:12:47.272817+00:00")
rf, arf = RequestFactory(), APIRequestFactory()
supervisor = get_user_model().objects.get(
    pk=int(os.environ["SONDA_SUPERVISOR_ID"])
)

def selecionar(obj, campos):
    return {k: obj[k] for k in campos if k in obj}

def guarda_sql(execute, sql, params, many, context):
    # Recusa inclusive SQL desconhecido: erro não vira resultado regular.
    if not sql.lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL_nao_permitido_na_sonda")
    return execute(sql, params, many, context)

def emitir(cid, dia, mes, fonte, fn):
    out = {
        "colaborador_id": cid, "dia": dia,
        "mes": mes, "ano": ANO, "fonte": fonte,
    }
    try:
        if connection.vendor != "postgresql":
            raise RuntimeError("sonda_exige_postgresql")
        with transaction.atomic():
            with connection.cursor() as cur:
                cur.execute("SET TRANSACTION READ ONLY")
            with connection.execute_wrapper(guarda_sql):
                out["resultado"] = fn()
    except Exception as exc:
        # Não imprimir exceção com possível nome/documento.
        out["erro"] = type(exc).__name__
    print(json.dumps(out, ensure_ascii=False, default=str))

def contexto_view(modulo, funcao, query, **kwargs):
    req = rf.get("/", query)
    req.user = supervisor
    req.session = {}
    # Executa a view; intercepta somente a renderização.
    with patch.object(modulo, "render",
                      side_effect=lambda request, template, context: context):
        result = getattr(modulo, funcao)(req, **kwargs)
    if not isinstance(result, dict):
        raise RuntimeError("view_sem_contexto_autorizado")
    return result

CAMPOS_DIA = (
    "data", "status", "status_rotulo", "status_dia",
    "veredito_dia", "horas_trabalhadas", "dia_pago_lavrada",
    "inconsistente", "turno_aberto", "resolver_dia_oferecido",
)

def dia_da_tela(ctx, dia):
    if dia is None:
        return {"dia_nao_informado": True}
    linhas = [
        selecionar(x, CAMPOS_DIA)
        for x in ctx.get("dias", [])
        if str(x.get("data")) == dia
    ]
    return {"linhas": linhas, "presente": bool(linhas)}

class TextoStatus(HTMLParser):
    def __init__(self):
        super().__init__()
        self.dentro = False
        self.partes = []

    def handle_starttag(self, tag, attrs):
        if tag == "td" and dict(attrs).get("data-col") == "status":
            self.dentro = True

    def handle_endtag(self, tag):
        if tag == "td":
            self.dentro = False

    def handle_data(self, data):
        if self.dentro:
            self.partes.append(data)

def lista_fechamento(c, mes):
    qs = universo_da_competencia(mes, ANO, c.empresa_id).filter(pk=c.pk)
    fechs = {
        f.colaborador_id: f
        for f in FechamentoMensal.objects.filter(
            mes=mes, ano=ANO, colaborador__in=qs,
        ).select_related("colaborador", "colaborador__empresa")
    }
    linhas, _ = montar_resumos(qs, fechs, mes, ANO, c.empresa_id)
    result = []
    for r in linhas:
        html = render_to_string("ponto/_fech_linha.html", {
            "r": r,
            "empresa_id": c.empresa_id,
            "periodo_fechado": periodo_trancado(c.empresa_id, mes, ANO),
            "csrf_token": "NOTPROVIDED",
        })
        texto = TextoStatus()
        texto.feed(html)
        f = r["fechamento"]
        result.append({
            "badge_template": " ".join(" ".join(texto.partes).split()),
            "estado_administrativo": f.status if f else None,
            "turnos_abertos": f.turnos_abertos if f else None,
            "folha": selecionar(r["folha"] or {}, (
                "entra", "motivo", "motivo_juiz",
            )),
            "prontidao": r["pront"],
            "travas": r["travas"],
        })
    return result

def retrato(c, mes):
    r = RetratoFolha.objects.filter(
        empresa_id=c.empresa_id, mes=mes, ano=ANO,
    ).first()
    linha = linha_do_colab(r, c.pk)
    return {
        "retrato_id": r.pk if r else None,
        "lavrado_em": r.lavrado_em if r else None,
        "linha": selecionar(linha or {}, (
            "colab", "status", "motivo", "dias", "datas", "dono", "tipo",
        )),
        "led": desenho_do_led(linha) if linha is not None else None,
    }

def api(c, mes, dia):
    if c.usuario is None:
        return {"sem_usuario_do_colaborador": True}
    req = arf.get("/", {"mes": mes, "ano": ANO})
    force_authenticate(req, user=c.usuario)
    resp = av.api_espelho_v2(req)
    return {
        "http_status": resp.status_code,
        "dia": dia_da_tela(resp.data, dia),
    }

with patch.object(timezone, "now", return_value=AGORA):
    for cid, dia in G1:
        # Busca por PK exclusivamente; nunca por nome.
        c = Colaborador.objects.select_related("empresa", "usuario").get(pk=cid)
        d = dt.date.fromisoformat(dia) if dia else None

        if d:
            def calendario():
                ctx = contexto_view(cv, "calendario_ponto", {
                    "modo": "mes", "mes": d.month, "ano": ANO,
                }, cid=cid)
                carga = CargaDaJanela([c], ctx["data_ini"], ctx["data_fim"])
                juiz = status_dos_dias_lavrados(
                    [(c, d)], agora=AGORA, carga=carga,
                )[(cid, d)]
                return {
                    "janela": [ctx["data_ini"], ctx["data_fim"]],
                    "tela": dia_da_tela(ctx, dia),
                    "juiz_D4_mesma_janela": juiz,
                }

            emitir(cid, dia, d.month, "calendario_ponto", calendario)
            emitir(cid, dia, d.month, "status_do_dia_lavrado",
                   lambda: status_do_dia_lavrado(c, d, agora=AGORA))

        for mes in (10, 11):
            def export():
                ini, fim = janela_fechamento(mes, ANO, c.empresa)
                itens = classificar_export(c.empresa, mes, ANO)
                return {
                    "janela": [ini, fim],
                    "dia_na_competencia": ini <= d <= fim if d else None,
                    "itens": [
                        selecionar(x, (
                            "colaborador_id", "status", "motivo",
                            "travas_aprovacao",
                        ))
                        for x in itens if x["colaborador_id"] == cid
                    ],
                }

            emitir(cid, dia, mes, "classificar_export", export)
            emitir(cid, dia, mes, "montar_resumos+template",
                   lambda: lista_fechamento(c, mes))
            emitir(cid, dia, mes, "linha_do_colab+desenho_do_led",
                   lambda: retrato(c, mes))

            emitir(cid, dia, mes, "espelho_ponto", lambda: dia_da_tela(
                contexto_view(pv, "espelho_ponto", {
                    "colab": cid, "mes": mes, "ano": ANO,
                }), dia,
            ))

            def painel():
                ctx = contexto_view(pv, "painel_fechamento", {
                    "colab": cid, "mes": mes, "ano": ANO,
                })
                f = ctx.get("fechamento")
                return {
                    "estado_administrativo": f.status if f else None,
                    "turnos_abertos": f.turnos_abertos if f else None,
                    "periodo_fechado": ctx.get("periodo_fechado"),
                }

            emitir(cid, dia, mes, "painel_fechamento", painel)

        # API navega por mês civil; a janela de apuração é decidida dentro dela.
        for mes_api in ([d.month] if d else [10, 11]):
            emitir(cid, dia, mes_api, "api_espelho_v2",
                   lambda: api(c, mes_api, dia))
PY
```

A sonda distingue D4 na janela do calendário da chamada unitária. Essa distinção é necessária porque o próprio juiz documenta que o pareamento pode depender da janela carregada. `app/ponto/status_dia.py:225`; `app/ponto/status_dia.py:238` Para os cinco casos G1 sem data, ela consulta as competências, mas não inventa um dia regular. [nao verificado]

**G2 — dia sem porta ou zero hora**

As funções que respondem são:

| Pergunta | Função real e prova |
|---|---|
| O dia efetivamente segura a folha? | `dias_que_seguram`, alimentando `censo_alcance`. `app/chamados/services/vigia_fio.py:576` |
| Há porta operacional alcançável? | `porta_alcancavel_do_dia`: fila, pergunta viva no app, texto e âncora. `app/chamados/juizes.py:1283` |
| Há outro encaminhamento previsto, fora do fio? | `dono_fora_do_fio`: Resolver dia, ausência aberta e demais destinos nomeados. `app/chamados/services/vigia_fio.py:273` |
| Qual classe do alcance? | `censo_alcance`. `app/chamados/services/vigia_fio.py:666` |
| Qual dono L-099? | `dono_da_divergencia`, com insumos reais montados por `InsumosDoDono.dono`. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:168` |

O vocabulário do vigia é: **a** chamado vivo com véu; **b** respondida sem texto; **c** respondida sob destino terminal; **d** colaborador na lista Cadastro×Realidade, mas dia fora; **e** demais detalhes, incluindo sem célula, sem pergunta viva, pergunta sob terminal, sem âncora ou fora do app. Há precedência entre esses ramos; um chamado terminal sozinho não determina a classe. `app/chamados/services/vigia_fio.py:666`

| Caso | Classe do vigia e dono provável | Cobertura existente |
|---|---|---|
| col253 do [nome], **id407**, 21/10 | Zero hora/furo com terminais: classe ainda indeterminada. BATIDA é provável **se** a batida continuar solta no dia atribuído e não houver prioridade cadastral. [nao verificado] | O261 para fio/pergunta sob terminal; enquadramento individual não demonstrado. `app/docs/BACKLOG.md:859`; `app/ponto/management/commands/e6_oraculo.py:98` |
| col836, 13/10 | `indefinida`, zero hora e chamado resolvido não mostram pergunta/âncora. CADASTRO apenas se confirmado o insumo cadastral prioritário. [nao verificado] | F3-BECO-1 condicional; O261. `app/docs/RELATO.md:93`; `app/docs/RELATO.md:101` |
| col836, 14/10 | Mesma insuficiência de insumos, julgada separadamente do dia 13. [nao verificado] | F3-BECO-1 condicional; O261. `app/docs/RELATO.md:101` |
| col909, 16/10 | Furo com horas parciais; BATIDA provável apenas após atribuição real das batidas. `em_analise` não prova alcance. [nao verificado] | O261; JUIZ-3 se o problema for a premissa daquele turno denunciado. `app/chamados/reconciliador.py:117`; `app/chamados/reconciliador.py:264` |
| col292, 21/09 | Relato sem porta; classe e dono não determináveis sem célula/perguntas/fila. [nao verificado] | O261; F3 depende de condições não fornecidas. `app/docs/BACKLOG.md:859` |
| col207, col332 e col446, sem dia | Candidatos a problema de alcance/encerramento; não há dia nem insumos suficientes para emitir a classe. [nao verificado] | O261/FIO MUDO condicionais. `app/chamados/services/vigia_fio.py:576`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141` |

L-099 prioriza folga não definida/ausência de vínculo → CADASTRO; depois batida fora de turno ou quantidade ímpar atribuída → BATIDA; fase deslocada/Cadastro×Realidade → CADASTRO; o restante → ESTRUTURA. `app/ponto/management/commands/e6_oraculo.py:98` **Zero batidas físicas na data não autoriza chamar BATIDA**, e a quantidade física do JSONL não substitui o carregamento ampliado e a atribuição do oráculo. `app/ponto/management/commands/e6_oraculo.py:72`; `app/ponto/management/commands/e6_oraculo.py:138`

As coberturas não são intercambiáveis:

- **F3-BECO-1:** cobre a combinação de passado indefinido, pergunta viva sob terminal e problema de folga; reatar ao LIMBO fornece âncora, não encerramento. Os anexos não demonstram a combinação completa em col836. `app/docs/RELATO.md:101`; `app/docs/RELATO.md:34` [nao verificado quanto ao caso]
- **O261:** cobre alcançabilidade e carimbo com lastro; não equivale a reabrir todo chamado terminal. `app/docs/BACKLOG.md:859`; `app/chamados/reconciliador.py:340`
- **CONTRATO-20 / JUIZ-3:** verifica o turno denunciado, identificado por entrada/contexto ou data; não qualquer turno aberto da janela. `app/chamados/reconciliador.py:117`; `app/chamados/reconciliador.py:145`
- **FIO MUDO — `batida_ausente`:** exige chamado vivo com célula, motivo emitível e ausência de pergunta viva; as guardas também barram fato já perguntado/decidido. `app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:211`
- **O168 / `pode_executar_csf`:** protege a conversa respondida antes de executar cobrança-sem-furo. Não há trilha anexada que atribua algum encerramento deste lote àquela porta. `app/ponto/supra_juiz.py:539`; `app/ponto/management/commands/supra_juiz.py:257` [nao verificado quanto aos casos]
- **O169 / cluster:** exige ao menos duas órfãs em rajada de até 15 minutos e marco faltante. Quantidade de batidas por data não prova essa classe em nenhum caso fornecido. `app/ponto/services/cluster_espurio.py:15` [nao verificado quanto aos casos]

**G3 — transnoite, troca e marco**

O caminho decisório é:

| Decisão | Código |
|---|---|
| Marcos previstos do dia | `EscalaColaborador.marcos_do_dia` lê o DNA congelado

[truncado em 20000 de 35526 caracteres]

## 05/11 16:49 · sessao c8b6f3f80328 · PD-FIO-MUDO · censo · origem=Code

HASH LIDO = a026943099b8d88468240fd4b18491141b6810af

## 1 QUEM ESCREVE e QUEM LÊ

O hash acima identifica a cópia informada; sua correspondência com um commit Git é **[não verificado]**. O aval fornecido declara universo **494**, mas não fornece seus registros, distribuição por causa nem estado individual: **[não verificado]**. (`avais.txt:1`, anexo, contando a primeira data como linha 1) [nao verificado]

**Campos e portas de escrita**

| Campo ou conjunto | Quem escreve hoje | Quem lê hoje |
|---|---|---|
| Chamado: `colaborador`, `categoria`, `subcategoria`, `modulo_origem`, `contexto_json`, `celula` | `ChamadoColaborador.abrir/criar`; para `batida_ausente`, detector, grade e C2 fornecem o contexto. Alterações posteriores de contexto passam por `anotar_contexto`, `renovar_contexto`, `retirar_contexto`, `fundir_contexto`. (`app/chamados/models.py:913`; `app/chamados/models.py:1048`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/chamados/reconciliador.py:879`; `app/chamados/services/contexto.py:62`; `app/chamados/services/contexto.py:128`) | Dia pelo juiz `dia_do_fato`; enquadramento pela fábrica; seleção pelo disparador; agrupamento por célula nos vigias. (`app/chamados/signals.py:178`; `app/chamados/services/disputa_emissao.py:683`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/services/vigia_fio.py:337`) |
| Chamado: `status_local`, `resolvido_em`, `via_encerramento` | Nascimento nas portas acima; transições por `_gravar_estado`, que escreve estado, carimbo e via. A entrada em terminal tem guarda de perguntas vivas ancoradas. (`app/chamados/models.py:195`; `app/chamados/models.py:215`; `app/chamados/models.py:222`; `app/chamados/models.py:238`) | Dedup, reabertura, sinais, fábrica, painel, cartório e vigias; `VIVOS`, `COBRANCA` e `TERMINAIS` são conjuntos diferentes. (`app/chamados/models.py:867`; `app/chamados/signals.py:169`; `app/chamados/services/disputa_emissao.py:691`; `app/chamados/services/painel.py:155`; `app/chamados/services/vigia_fio.py:334`; `app/chamados/catalogo/motor.py:24`) |
| Chamado: `silenciado_ate` e motivo do silêncio | Portas `arquivo.silenciar/levantar`; o C11 de ausência chama essas portas. Silenciar conserva o estado vivo. (`app/chamados/services/arquivo.py:141`; `app/chamados/services/arquivo.py:176`; `app/chamados/services/silencio_ausencia.py:86`; `app/chamados/models.py:81`) | `qs_na_fila` compara o prazo ao relógio; a alimentação de alcance usa essa fila. (`app/chamados/services/painel.py:186`; `app/chamados/juizes.py:1232`) |
| Pergunta: `disputa`, `motivo`, `data_evento`, `celula`, `chamado_destino`, texto, tipo e opções | `PerguntaDisputa.abrir` cria; na identidade existente também atualiza célula/destino. `reapontar_resolvedora` altera destino e pode preencher data ausente, relavrando no mesmo `save`. (`app/chamados/models.py:2387`; `app/chamados/models.py:2424`; `app/chamados/models.py:2439`; `app/chamados/models.py:2465`; `app/chamados/services/fio.py:832`; `app/chamados/services/fio.py:845`) | C6, identidade do fato, `resolvedora_valida`, fábrica, materialização, `chamado_que_lista`, app e alcance. A âncora é destino, com fallback para o chamado da disputa. (`app/chamados/models.py:2381`; `app/chamados/models.py:2408`; `app/chamados/juizes.py:1077`; `app/chamados/services/fio.py:116`; `app/chamados/juizes.py:1320`) |
| Pergunta: `resposta_colab`, `respondida_em` | Resposta humana por `responder_pergunta_disputa_core`; resposta automática por `responder_automaticamente`; recomposição de instante por `carimbar_resposta_pela_trilha`. Correção limpa resposta e preserva versão anterior; `carimbar(resposta=...)` também pode preencher instante quando já existe texto. (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1403`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/models.py:2576`) | C6 distingue resposta com texto; validação exige resposta; crédito do painel separa respondidas; alcance recusa resposta vazia como porta da pergunta. (`app/chamados/juizes.py:1092`; `app/chamados/services/validacao.py:363`; `app/chamados/services/raiox.py:137`; `app/chamados/juizes.py:1322`) |
| Pergunta: `validada_em`, `validada_por` | `_carimbar_validacao`; devolução de validação recusada; resposta nova desfaz assinatura; correção e ressuscitação limpam; `carimbar(resposta='validar')` também preenche assinatura vazia. (`app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`; `app/chamados/models.py:2580`) | Primeiro degrau do C6, propriedade `estado`, seleção de validação e materialização. (`app/chamados/juizes.py:1078`; `app/chamados/models.py:2488`; `app/chamados/services/validacao.py:366`; `app/chamados/services/validacao.py:422`) |
| Pergunta: `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo`, pelas portas `carimbar`, `carimbar_identidade`, `descarimbar`; volta em lote por `descarimbar_em_lote`. Existe troca isolada de via em `encerrar_por_fato_no_espelho`. (`app/chamados/models.py:2545`; `app/chamados/models.py:2551`; `app/chamados/models.py:2614`; `app/chamados/models.py:2635`; `app/chamados/models.py:2644`; `app/chamados/models.py:2662`) | C6, `q_vivas`, fábrica, ressuscitação, materialização, raio-X e vigia de carimbos. Carimbo e morte pelo juiz não são equivalentes. (`app/chamados/juizes.py:1087`; `app/chamados/juizes.py:1113`; `app/chamados/models.py:2714`; `app/chamados/services/disputa_emissao.py:695`; `app/chamados/models.py:1423`; `app/chamados/services/raiox.py:173`; `app/chamados/services/vigia_fio.py:462`) |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar`; persistência em lote por `gravar_lavras_em_lote`; costura relavra junto com destino/data. (`app/chamados/veredito_lavrado.py:197`; `app/chamados/veredito_lavrado.py:211`; `app/chamados/veredito_lavrado.py:227`; `app/chamados/services/fio.py:846`) | Crédito do painel e `pergunta_sem_acao_possivel` leem a lavra; sem lavra, esta última consulta `_pergunta_materializada_de_fato`. (`app/chamados/services/raiox.py:133`; `app/chamados/services/raiox.py:182`) |
| Célula: `data`, `trabalha`, `escala_geradora`, `dna` | Portas de geração, regeneração e reversão da célula; a geradora é insumo do juiz do vínculo do dia. (`app/ponto/portas/celula.py:100`; `app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:488`; `app/ponto/portas/celula.py:563`; `app/escala/alimentacao.py:269`) | `marcos_vencidos`, prova do marco, juiz de dia sem trabalho, fábrica e disparador. (`app/ponto/services/cartorio.py:841`; `app/chamados/juizes.py:891`; `app/chamados/juizes.py:411`; `app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/management/commands/disparar_perguntas_competencia.py:150`) |
| Célula: `ata`, `veredito`, `veredito_via`, `veredito_em`, `protestos`, `julgada_em` | `CelulaDia.lavrar_veredito` delega à porta; `lavrar_veredito`, `carimbar` e `carimbar_precedencia` escrevem os respectivos conjuntos. (`app/escala/models.py:1819`; `app/ponto/portas/celula.py:717`; `app/ponto/portas/celula.py:795`; `app/ponto/portas/celula.py:819`) | C6, fábrica pela ata, parcial na lista, ressuscitação e ambos os censos do vigia. (`app/chamados/juizes.py:1089`; `app/chamados/juizes.py:1123`; `app/chamados/services/fabrica_da_celula.py:34`; `app/chamados/services/disputa_emissao.py:868`; `app/chamados/models.py:1441`; `app/chamados/services/vigia_fio.py:377`; `app/chamados/services/vigia_fio.py:595`) |

**Abertura e emissão atuais**

- `abrir()` trava o colaborador dentro de `atomic`, consulta dedup e devolve existente vivo ou tenta renascer encerrado; ambos podem retornar `criado=False`. Seu outbox explícito só cobre `MODULOS_DE_TURNO`, conjunto sem `batida_ausente`. (`app/chamados/models.py:863`; `app/chamados/models.py:882`; `app/chamados/models.py:938`; `app/chamados/reconciliador.py:336`)
- Existe evento indireto: `post_save` do chamado criado ou com mudança de estado chama `julgar_celula`, salvo quando já está em julgamento. A tentativa fica em `engolir`; portanto não equivale a uma entrega incondicional de pergunta. (`app/chamados/signals.py:164`; `app/chamados/signals.py:172`; `app/chamados/signals.py:174`; `app/chamados/signals.py:180`)
- O cartório chama `perguntar_pela_celula` somente para células julgadas naquela passada; a fábrica consulta motivos, teto, parcial, identidade histórica e destino. (`app/ponto/services/cartorio.py:981`; `app/ponto/services/cartorio.py:995`; `app/chamados/services/fabrica_da_celula.py:74`; `app/chamados/services/fabrica_da_celula.py:145`)
- Grade abre e retorna sem emissão direta de pergunta. Detector abre primeiro e tenta construir o fio depois, em bloco que absorve exceção. C2 abre e reaponta pela porta. (`app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/reconciliar_grade.py:531`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/detectar_ausencias.py:787`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/chamados/reconciliador.py:879`; `app/chamados/reconciliador.py:922`)
- O ramo FIO MUDO consulta **qualquer módulo** vivo com célula na janela, não somente `batida_ausente`; verifica resolvedoras vivas, parcial, motivo vencido, situação ativa e identidade histórica. Com `--apply`, escreve pela função `perguntar`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `app/chamados/management/commands/disparar_perguntas_competencia.py:169`; `app/chamados/management/commands/disparar_perguntas_competencia.py:211`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`)

**Cinco mecanismos pós-abertura localizáveis a partir de Q2**

A correspondência literal desta enumeração com os “5 atos de perda” do aval é **[não verificado]**; os avais da cópia localizados nomeiam abertura/evento, mas não enumeram esses cinco. (`app/docs/PROMPTS.md:1519`; `app/docs/PROMPTS.md:1853`)

| Mecanismo | Escritor ou leitura que altera a condição do fio |
|---|---|
| Reabertura sem pergunta restaurável | `reabrir` grava vivo e chama ressuscitação; esta só considera materializadas sem texto, consulta marco/C6 e pode devolver zero. Não cria pergunta inexistente. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1423`; `app/chamados/models.py:1447`; `app/chamados/models.py:1481`) |
| Identidade/duplicata | `retratar_duplicata` carimba identidade; não encerra diretamente o chamado. Resposta pendente pode manter a duplicata viva porque precede a identidade no C6. (`app/chamados/services/fio.py:878`; `app/chamados/services/fio.py:891`; `app/chamados/juizes.py:1092`; `app/chamados/juizes.py:1113`) |
| Validação/materialização com furo residual | Validação mata no primeiro degrau; materialização tenta reconciliar destino/clones, mas o sobrevivente pode permanecer vivo por premissa residual. Recusa de materialização também pode devolver a validação. (`app/chamados/juizes.py:1078`; `app/chamados/services/materializacao.py:728`; `app/chamados/services/materializacao.py:748`; `app/chamados/models.py:730`; `app/chamados/services/validacao.py:425`) |
| Morte por leitura | Tranca, marco comprovado ou célula não acusadora podem produzir morte sem novo carimbo. Célula acusadora, resposta discordante e pendência do marco alteram a resposta. (`app/chamados/juizes.py:1082`; `app/chamados/juizes.py:1104`; `app/chamados/juizes.py:1119`; `app/chamados/juizes.py:1125`; `app/chamados/juizes.py:1137`) |
| Transferência da última resolvedora | `reapontar_resolvedora` salva novo destino e lavra; seu corpo não encerra o anterior. A guarda examina o novo destino terminal, não a perda da última resolvedora no antigo. Incidência nos 494: **[não verificado]**. (`app/chamados/services/fio.py:764`; `app/chamados/services/fio.py:770`; `app/chamados/services/fio.py:829`; `app/chamados/services/fio.py:845`) |

**Leitores e acionadores fora dos sítios iniciais**

- `fio_vivo`: disputa pode fechar, encerramento da disputa, dedup da emissão, limbo, encerramento por fato, prévia da folha, reconciliação da grade, supra e emissão retroativa. (`app/chamados/models.py:1783`; `app/chamados/juizes.py:2026`; `app/chamados/services/disputa_emissao.py:1244`; `app/chamados/services/limbo_folgas.py:201`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:83`; `app/folha/previa.py:144`; `app/ponto/management/commands/reconciliar_grade.py:460`; `app/ponto/management/commands/supra_juiz.py:215`; `app/ponto/management/commands/emitir_furo_retroativo.py:98`)
- C6 também alimenta app, portas de resposta web/API, relatórios, aviso de pendência, detalhe do dia, calendário e as propriedades `estado/viva`. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/views.py:1238`; `app/api/views.py:2627`; `app/chamados/relatorios_disputa.py:18`; `app/chamados/services/aviso_pendencia.py:13`; `app/ponto/services/dia_do_colab.py:70`; `app/colaboradores/services/calendario.py:468`; `app/chamados/models.py:2491`; `app/chamados/models.py:2762`)
- `porta_alcancavel_do_dia` é lido pela folha e por `censo_alcance`; `vigiar_fio_morto` chama os dois censos com o mesmo dia e a mesma lista administrativa. (`app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:634`; `app/chamados/management/commands/vigiar_fio_morto.py:162`)
- Chamadores de perda/costura incluem dedup por comando, migração de resolvedoras, reconciliador, declaração textual e limbo. Carimbos também vêm de ausência, materialização, decisão administrativa, paridade e colisão de plantio. (`app/chamados/management/commands/dedup_perguntas_fato.py:88`; `app/chamados/services/fio.py:943`; `app/chamados/reconciliador.py:1570`; `app/chamados/services/declaracao_texto.py:320`; `app/chamados/services/limbo_folgas.py:242`; `app/chamados/services/materializacao.py:546`; `app/chamados/models.py:2511`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:1066`)

## 2 JUIZ ou LEI que já responde

| Pergunta | Autoridades existentes e limite |
|---|---|
| O que significa C11? | No registro vivo, `_C11` é “a ausência suspende, fecha ou levanta a cobrança?”, autoridade `silencio_ausencia.reconciliar_por_ausencia`. O rótulo operacional “FIO-MUDO = C11” aparece no aval; equivalência técnica entre os dois: **[não verificado]**. (`app/core/juizes.py:117`; `app/core/juizes.py:271`; `app/docs/PROMPTS.md:1519`) |
| A pergunta está viva? Há alguma viva? | **C6** é `pergunta_viva`; **C15** é `fio_vivo`, redução `any` sobre a coleção recebida. Ambos participam, mas respondem perguntas diferentes. (`app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/chamados/juizes.py:1184`) |
| Alguém consegue chegar ao dia? | **C17**, `porta_alcancavel_do_dia`: chamado da célula na fila **ou** pergunta viva no app com âncora na fila. C6/C15 não provam sozinhos essa alcançabilidade. (`app/core/juizes.py:277`; `app/chamados/juizes.py:1286`; `app/chamados/juizes.py:1322`) |
| Há ação possível sobre a pergunta? | O raio-X mantém `pergunta_sem_acao_possivel`, baseado em materialização e lavra; C17 compõe fila, app e âncora. O corte manda raio-X, folha e vigia lerem a porta, mas o corpo atual do raio-X ainda responde por seu próprio caminho de lavra. São os dois sítios a registrar, sem tratá-los como predicados equivalentes. (`app/docs/CORTES.md:404`; `app/chamados/services/raiox.py:173`; `app/chamados/juizes.py:1283`) |
| Chamado vivo significa cobrança? | **C1/VIVOS** responde existência não terminal; `COBRANCA` exclui `registrado`; fila também exclui silêncio vigente. O registro já separou “vivo” de “tem fio”. (`app/core/juizes.py:100`; `app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/services/painel.py:186`) |
| Onde nasce a pergunta? | Aval escolheu evento de abertura de `batida_ausente`; lei existente manda emissão/pergunta/chamado nascerem do sinal da célula, consumidor ler a lâmpada e varredura apenas vigiar. (`app/docs/PROMPTS.md:1853`; `CLAUDE.md:166`; `CLAUDE.md:168`; `CLAUDE.md:169`; `CLAUDE.md:170`) |
| Pode fechar ou deixar morrer? | Corte FIO-MORTO-E-ESTRUTURAL exige fio vivo ou ação administrativa nomeada quando persiste a dívida; chamado nasce da célula, morre por lastro, nunca sem dono. (`app/docs/CORTES.md:253`; `app/docs/ESTRUTURAL.md:46`) |
| O marco venceu? Qual vínculo e dia? | `marcos_vencidos` usa instante e marcos encadeados; `vinculo_do_dia` responde pela geradora dentro da vigência; `dia_do_fato` e `resolvedora_valida` respondem dia/costura. (`app/ponto/services/cartorio.py:831`; `app/ponto/services/cartorio.py:845`; `app/escala/alimentacao.py:228`; `app/escala/alimentacao.py:272`; `app/chamados/models.py:2402`) |
| A competência cala a pergunta? | `dia_da_pergunta_trancado` compõe competência da data e `periodo_trancado`, considerando toque humano posterior. Exportação e tranca não são a mesma pergunta. (`app/chamados/juizes.py:917`; `app/chamados/juizes.py:939`; `app/chamados/juizes.py:949`; `app/docs/CORTES.md:216`) |
| Ausência cala tudo? | C11 suspende ausência aberta; aprovada fecha somente quando a premissa morre; rejeitada/apagada levanta seu silêncio. Parcial sem período vai à lista administrativa; não autoriza julgar dentro/fora nem fabricar falta. (`app/chamados/services/silencio_ausencia.py:77`; `app/chamados/services/silencio_ausencia.py:93`; `app/chamados/services/silencio_ausencia.py:112`; `app/docs/CORTES.md:319`) |
| Qual referência dos RED e alcance do passivo? | L-110 exige regra aplicada ao cadastro/batidas, não código antigo como gabarito; L-092 conserva o gravado exportado; L-124 mantém passado como foto até fato novo e reserva backfill único. (`app/docs/LEIS.md:136`; `app/docs/LEIS.md:125`; `app/docs/CORTES.md:421`) |
| O cron já deixou de julgar FIO MUDO? | Não nesta cópia: o ramo permanece escritor e em `JUIZES_POR_VARREDURA`; a exceção tem saída condicionada à medida e ao pouso do C11 operacional. (`app/config/crons.py:1995`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`; `app/docs/CORTES.md:305`) |

## 3 CONTRA-EXEMPLOS pela regra do aval

| Leitura aparentemente óbvia | Contra-exemplo demonstrável |
|---|---|
| “Toda abertura já tem pergunta porque existe sinal.” | O sinal pode sair por `em_julgamento`; a fábrica só roda nas células julgadas naquela passada. Existência do sinal não demonstra cobertura universal da abertura. (`app/chamados/signals.py:174`; `app/ponto/services/cartorio.py:995`) |
| “Vivo sem pergunta é sempre defeito.” | `registrado` é vivo sem ação devida; parcial pode estar legitimamente na lista do admin; ausência aberta tem decisão pendente própria. (`app/chamados/catalogo/motor.py:24`; `app/chamados/services/disputa_emissao.py:892`; `app/chamados/services/vigia_fio.py:435`) |
| “Criou há 30 dias, então ficou mudo no nascimento.” | `criado_em` registra nascimento; a vitalidade é leitura atual, afetada por validação, tranca, resposta, identidade e célula. Estado no nascimento dos 494: **[não verificado]**. (`app/chamados/models.py:61`; `app/chamados/juizes.py:1077`) |
| “Virou meia-noite: o turno acabou.” | Turno 22h–06h ainda não venceu a saída às 06h10; o juiz compara marco encadeado mais margem ao instante. Saída de intervalo às 00h30 também não prova saída final. (`CLAUDE.md:185`; `app/chamados/services/fabrica_da_celula.py:18`; `app/ponto/services/cartorio.py:845`; `app/chamados/services/disputa_emissao.py:638`) |
| “No mês civil de outubro vale a mesma tranca.” | Com

[truncado em 20000 de 32727 caracteres]

## 05/11 16:48 · sessao 9b2946a29690 · PD-O168 · censo · origem=Code

HASH LIDO = a026943099b8d88468240fd4b18491141b6810af

## 1 QUEM ESCREVE e QUEM LÊ

O identificador acima é o da cópia indicada no enunciado; sua correspondência com um commit Git é [não verificado]. As referências a `avais.txt` e `backlog_obras.txt` abaixo numeram as linhas de conteúdo dos anexos fornecidos. Medições atuais de produção, DIFF sombra, percentual da fila viva e `CSF_RETRATADO=0`: [não verificado]. [nao verificado]

**A separação atual:** `pode_executar_csf` responde sobre conversa; `classificar_dia` classifica contradições; as portas do chamado decidem e gravam o fechamento com outras guardas. A única chamada de produção encontrada da guarda CSF está no executor do supra (`app/ponto/supra_juiz.py:539`; `app/ponto/supra_juiz.py:204`; `app/chamados/models.py:629`; `app/ponto/management/commands/supra_juiz.py:261`).

| Campo ou relação | Quem escreve | Quem lê, inclusive fora dos sítios iniciais |
|---|---|---|
| `PerguntaDisputa.resposta_colab`, `respondida_em` | Resposta humana; resposta automática; recuperação do carimbo pela trilha; reabertura para correção; ressuscitação; `carimbar` quando existe texto. A reabertura limpa texto/carimbos e preserva resposta anterior (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1407`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/models.py:1493`; `app/chamados/models.py:2576`). | CSF; C6; respostas sem veredito; validação; materializador; retenção de irmãs; fio; estado da pergunta; verbos e lavra persistida (`app/ponto/supra_juiz.py:545`; `app/chamados/juizes.py:1092`; `app/chamados/models.py:1772`; `app/chamados/services/validacao.py:363`; `app/chamados/services/materializacao.py:1292`; `app/chamados/juizes.py:1691`; `app/chamados/services/fio.py:336`; `app/chamados/models.py:2492`; `app/chamados/services/verbos.py:311`; `app/chamados/veredito_lavrado.py:167`). |
| `validada_em`, `validada_por` | Validação escreve pelo carimbo próprio; `PerguntaDisputa.carimbar` também pode preencher validação; reabertura e ressuscitação limpam esses campos (`app/chamados/services/validacao.py:127`; `app/chamados/models.py:2581`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`). | C6, guarda de perguntas ancoradas e respostas sem veredito. CSF não consulta esses campos (`app/chamados/juizes.py:1078`; `app/chamados/models.py:327`; `app/chamados/models.py:1773`; `app/ponto/supra_juiz.py:539`). |
| `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição central em `_escrever_carimbo`; `carimbar` verifica C6 e desfaz a alteração quando a pergunta continua viva; `carimbar_identidade` possui protocolo distinto, sem essa consulta (`app/chamados/models.py:2545`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`; `app/chamados/models.py:2614`). | C6, disputa, materialização, emissão e raio X. CSF continua lendo apenas `respondida_em` (`app/chamados/juizes.py:1087`; `app/chamados/models.py:1773`; `app/chamados/services/materializacao.py:1292`; `app/chamados/services/disputa_emissao.py:909`; `app/chamados/services/raiox.py:173`; `app/ponto/supra_juiz.py:545`). |
| `DisputaSupervisao.chamado` / reversa `disputa_origem` | Nascimentos de disputa atribuem o pai; a relação está declarada no modelo (`app/chamados/services/disputa_emissao.py:523`; `app/chamados/services/disputa_emissao.py:962`; `app/chamados/services/disputa_emissao.py:1182`; `app/chamados/models.py:1668`). | CSF percorre **todas** as perguntas das disputas de origem; fio, retenção, guarda de encerramento, entrada sincronizada e receiver disputa→pai também leem a relação (`app/ponto/supra_juiz.py:543`; `app/chamados/services/fio.py:131`; `app/chamados/juizes.py:1693`; `app/chamados/models.py:328`; `app/ponto/services/entrada_sincronizada.py:56`; `app/chamados/signals.py:28`). |
| `PerguntaDisputa.chamado_destino`, `data_evento` / reversa `perguntas_que_resolvem` | Nascimento/reuso por `PerguntaDisputa.abrir`; reapontamento por `fio.reapontar_resolvedora`. A porta recebe a data antes de julgar e relavrar (`app/chamados/models.py:2387`; `app/chamados/models.py:2439`; `app/chamados/models.py:2466`; `app/chamados/services/fio.py:784`; `app/chamados/services/fio.py:816`). | CSF; âncora do fio; gate de fechamento; retenção; emissão; tranca; materialização (`app/ponto/supra_juiz.py:547`; `app/chamados/services/fio.py:97`; `app/chamados/models.py:328`; `app/chamados/juizes.py:1692`; `app/chamados/services/disputa_emissao.py:929`; `app/chamados/services/competencia_trancada.py:140`; `app/chamados/services/materializacao.py:561`). |
| `FollowupChamado`: conteúdo, autor e instante | `nota.anexar_ocorrencia`; resposta livre só cria mensagem quando há conteúdo; resposta rápida exige conteúdo e cria `mensagem_dp` (`app/chamados/services/nota.py:75`; `app/chamados/services/nota.py:103`; `app/chamados/services/acoes_chamado.py:289`; `app/chamados/services/fio.py:705`; `app/chamados/services/fio.py:714`). | Detalhe, fio, contexto das ações e verbos. **CSF não lê followups** (`app/chamados/views.py:352`; `app/chamados/services/fio.py:172`; `app/chamados/services/acoes_chamado.py:402`; `app/chamados/services/verbos.py:266`; `app/ponto/supra_juiz.py:539`). |
| `ChamadoColaborador.status_local`, `resolvido_em`, `via_encerramento`, urgência e `respondido_por` | `_gravar_estado` concentra a escrita das portas: terminal carimba resolução/via e rebaixa urgência; reabertura limpa resolução/via. A auditoria antecede o save, no mesmo atomic (`app/chamados/models.py:195`; `app/chamados/models.py:222`; `app/chamados/models.py:238`; `app/chamados/models.py:242`; `app/chamados/models.py:274`). | Motor de transição; cartório; supra; gates; receivers; C6 para destino encerrado; fio e filas (`app/chamados/models.py:639`; `app/ponto/services/cartorio.py:1334`; `app/ponto/management/commands/supra_juiz.py:212`; `app/chamados/models.py:215`; `app/chamados/signals.py:81`; `app/chamados/models.py:255`; `app/chamados/services/fio.py:117`). |
| `DisputaSupervisao.fechada_em`, resolução e autor | `DisputaSupervisao.fechar`; portas de reabertura limpam o fechamento (`app/chamados/models.py:1785`; `app/chamados/models.py:1850`; `app/chamados/models.py:2052`; `app/chamados/models.py:2076`). | Receiver pede reconciliação do pai; fio e validação também leem o fechamento (`app/chamados/signals.py:28`; `app/chamados/signals.py:42`; `app/chamados/services/fio.py:135`; `app/chamados/services/validacao.py:139`). |
| Célula: `trabalha`, DNA, vínculo gerador e regeneração | Portas de geração/regeneração escrevem a previsão e o DNA (`app/ponto/portas/celula.py:100`; `app/ponto/portas/celula.py:247`; `app/ponto/portas/celula.py:451`). | `vinculo_do_dia`, grade, cartório, juiz de dia sem trabalho e guarda de cobrança órfã (`app/escala/alimentacao.py:269`; `app/ponto/services/cartorio.py:1126`; `app/ponto/services/cartorio.py:1408`; `app/chamados/juizes.py:420`; `app/ponto/management/commands/supra_juiz.py:275`). |
| Célula: `veredito`, via, instante, `protestos`, `ata`, status do dia | `lavrar_veredito`; cartório e decisão administrativa solicitam a lavra. `carimbar_precedencia` escreve a chave de precedência na mesma ata (`app/ponto/portas/celula.py:717`; `app/ponto/services/cartorio.py:1802`; `app/ponto/services/veredito_celula.py:330`; `app/ponto/portas/celula.py:819`). | Executor supra; `marcos_acesos`/`marcos_inexistentes`; C6; reconciliação/materialização; leitores de porta da folha (`app/ponto/management/commands/supra_juiz.py:246`; `app/ponto/services/lastro.py:98`; `app/ponto/services/lastro.py:153`; `app/chamados/juizes.py:1089`; `app/chamados/reconciliador.py:972`; `app/folha/fora_da_folha.py:65`). |
| Célula: `julgada_em`, `impressao`, ausência, entrada/saída e `insumos_em` | `carimbar`, solicitado pelos dois ramos do cartório (`app/ponto/portas/celula.py:795`; `app/ponto/services/cartorio.py:1264`; `app/ponto/services/cartorio.py:1822`). | Cartório decide se pula o julgamento; fábrica lê o instante anterior do ato. A impressão inclui batidas, status dos chamados, DNA, veto, teto, regra, fio, parcial e isenção (`app/ponto/services/cartorio.py:1358`; `app/ponto/services/cartorio.py:943`; `app/ponto/services/cartorio.py:443`). |
| Contagens `chamados_vivos`, `chamados_total`, `cobrancas_vivas` e saída `cods` | Cartório, supra e retroativo montam os insumos; `classificar_dia` produz contradições sem I/O (`app/ponto/services/cartorio.py:1413`; `app/ponto/management/commands/supra_juiz.py:223`; `app/ponto/management/commands/emitir_furo_retroativo.py:101`; `app/ponto/supra_juiz.py:204`). | Cartório usa códigos para lavra/emissão/fechamento; supra usa classificação para placar e alarme, mas usa a **lavra persistida** para retratar (`app/ponto/services/cartorio.py:1514`; `app/ponto/services/cartorio.py:1802`; `app/ponto/management/commands/supra_juiz.py:239`). |
| `CSF_RETRATADO` | Contador em memória incrementado somente quando `retratar` retorna verdadeiro (`app/ponto/management/commands/supra_juiz.py:286`). | Impressão do placar; não é um motivo persistido no evento `estado_chamado` (`app/ponto/management/commands/supra_juiz.py:299`; `app/chamados/models.py:275`). |

**Funções e encadeamentos atuais:**

- `pode_executar_csf` retorna falso ao encontrar qualquer `respondida_em` na origem ou no destino; não restringe por pergunta viva, validação, materialização ou disputa aberta. Seus testes cobrem origem, destino, ausência de resposta e coleção vazia (`app/ponto/supra_juiz.py:543`; `app/ponto/supra_juiz.py:547`; `app/ponto/tests/test_supra_juiz_executor.py:38`).
- `classificar_dia` possui quatro chamadas de produção: cartório sem batidas, cartório normal, supra e emissão retroativa. Separa chamado vivo de cobrança com fio vivo (`app/ponto/services/cartorio.py:1226`; `app/ponto/services/cartorio.py:1413`; `app/ponto/management/commands/supra_juiz.py:223`; `app/ponto/management/commands/emitir_furo_retroativo.py:101`; `app/ponto/supra_juiz.py:213`).
- Eventos de batida/ausência chamam `julgar_celula`; o lote chama `julgar_colab`. Dentro do cartório, os pedidos de reconciliação estão nos adiados, marcos satisfeitos, marcos inexistentes, CSF/dia sem trabalho e duplicatas, sem chamada CSF nesses pontos (`app/ponto/signals.py:131`; `app/ponto/signals.py:143`; `app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:963`; `app/ponto/services/cartorio.py:1488`; `app/ponto/services/cartorio.py:1501`; `app/ponto/services/cartorio.py:1522`; `app/ponto/services/cartorio.py:1600`; `app/ponto/services/cartorio.py:1615`).
- As portas são `transicionar/fechar`, `reconciliar`, `encerrar`, `cancelar` e `encerrar_por_competencia_trancada`; convergem na escrita de estado. A reconciliação consulta premissa e pergunta viva; as demais possuem seus protocolos próprios, sem consultar CSF (`app/chamados/models.py:122`; `app/chamados/models.py:430`; `app/chamados/models.py:616`; `app/chamados/models.py:648`; `app/chamados/models.py:651`; `app/chamados/models.py:749`; `app/chamados/models.py:1536`; `app/chamados/models.py:1556`).
- `retratar→_retratar` acrescenta retenção, tratamento de justificativa, resolução das perguntas e nota; `encerrar_com_retencao` consulta retenção antes de encerrar. Materialização também consulta irmãs sem consumo antes de reconciliar destino/clones (`app/chamados/reconciliador.py:492`; `app/chamados/reconciliador.py:518`; `app/chamados/reconciliador.py:523`; `app/chamados/reconciliador.py:532`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:540`; `app/chamados/services/materializacao.py:730`; `app/chamados/services/materializacao.py:738`).
- Disputa fechada solicita reconciliação do pai; fechamento do chamado solicita resolução das perguntas em savepoint próprio. Falha nessa resolução é registrada e não necessariamente desfaz o fechamento do chamado (`app/chamados/signals.py:42`; `app/chamados/signals.py:45`; `app/chamados/signals.py:83`; `app/chamados/signals.py:91`).

A exaustão semântica de **todo leitor** desses campos em todo o repositório, além dos leitores nomeados acima e dos fechadores da seção 5, é [não verificado].

## 2 JUIZ ou LEI QUE JÁ RESPONDE

| Pergunta | Autoridade ou lei existente |
|---|---|
| Cobrança sem furo com conversa pode morrer automaticamente? | A guarda nominal é `pode_executar_csf`; a O168 exige sua validade também no evento e condiciona a retirada de `--executar` à guarda no evento e `CSF_RETRATADO` medido em zero (`app/ponto/supra_juiz.py:539`; `app/docs/BACKLOG.md:1379`). |
| Qual é o complemento vigente da obra? | Núcleo, DIFF sombra, `>10% fila viva = PAREI`, padrão juiz único; o anexo repete isso em C20-O168 (`avais.txt:1`, anexo; `backlog_obras.txt:8`, anexo). Denominador operacional de “fila viva” e DIFF medido: [não verificado]. |
| O chamado está vivo? Ele cobra alguém? | `VIVOS` responde ao estado; `fio_vivo` responde ao fio. O registro distingue essas perguntas (`app/core/juizes.py:100`; `app/core/juizes.py:261`; `app/core/juizes.py:275`). |
| A transição é permitida? A premissa morreu? | `decidir` e `premissa_morta` são autoridades distintas; a porta de reconciliação também consulta pergunta viva (`app/core/juizes.py:262`; `app/core/juizes.py:263`; `app/chamados/models.py:648`; `app/chamados/models.py:651`). |
| A pergunta está viva? A disputa pode fechar? | C6=`pergunta_viva`; C7=`DisputaSupervisao.pode_fechar`. C7 exige ausência de resposta sem veredito e de fio vivo (`app/core/juizes.py:266`; `app/core/juizes.py:267`; `app/chamados/models.py:1778`). |
| O dia não tem trabalho? Os marcos cobrados já acenderam? | `dia_sem_trabalho` responde à primeira; `marcos_acesos` responde à segunda. O cartório possui ainda `_protesta`, lendo `trabalha is not False` e marcos acesos (`app/core/juizes.py:278`; `app/ponto/services/lastro.py:153`; `app/ponto/services/cartorio.py:1407`). |
| Quem governa dia, vínculo e janela? | `data_do_chamado`; `vinculo_do_dia`; `janela_atual/janela_fechamento`; `dia_encerrado` para encerramento do turno (`app/core/juizes.py:265`; `app/escala/alimentacao.py:269`; `app/ponto/janelas.py:54`; `app/ponto/janelas.py:60`; `app/ponto/turnos.py:2105`). |
| Evento e varredura podem manter decisões independentes? | A seção 4a manda evento julgar/carimbar e consumidor ler a lâmpada; contrato 4b exige um juiz por pergunta e um escritor por entidade. L-060/L-061 registram cron vigia e consumidor leitor (`CLAUDE.md:166`; `CLAUDE.md:175`; `app/docs/LEIS.md:94`; `app/docs/LEIS.md:95`). |

**Há respostas sobrepostas, mas não equivalentes:** CSF protege qualquer pergunta respondida; C6 decide vida da pergunta; a retenção protege resposta sem consumo; a premissa decide se o fato ainda sustenta cobrança. Hoje um fechamento pode passar por algumas dessas autoridades sem consultar CSF (`app/ponto/supra_juiz.py:545`; `app/chamados/juizes.py:1077`; `app/chamados/reconciliador.py:437`; `app/chamados/models.py:648`).

A tabela JUIZES é consulta, e o registro vivo é `core/juizes.py`; o bloco de autoridades de chamado não registra CSF entre suas entradas (`app/docs/JUIZES.md:9`; `app/docs/JUIZES.md:14`; `app/core/juizes.py:261`).

## 3 CONTRA-EXEMPLOS PELA REGRA DO AVAL

| Leitura que erra | Contraexemplo demonstrado |
|---|---|
| “Pergunta morta significa sem conversa.” | Pergunta validada deixa de estar viva pelo C6, mas `respondida_em` continua impedindo CSF. Materialização também não é consultada pela guarda CSF (`app/chamados/juizes.py:1078`; `app/ponto/supra_juiz.py:545`). |
| “Conversa significa qualquer mensagem que CSF encontra.” | Uma mensagem apenas em `FollowupChamado`, sem pergunta respondida, não é percorrida por CSF. A extensão normativa da palavra “conversa” a todo followup não está explicitada além da guarda nomeada: [não verificado] (`app/chamados/services/nota.py:103`; `app/ponto/supra_juiz.py:543`). |
| “Sem CSF, o caminho necessariamente enterra uma resposta.” | R pode recusar por premissa/pergunta viva; `_retratar` pode reter resposta sem consumo; duplicata pode transferir a pergunta ao sobrevivente; FIO-DUP tem guarda própria por texto (`app/chamados/models.py:648`; `app/chamados/models.py:651`; `app/chamados/reconciliador.py:518`; `app/chamados/models.py:420`; `app/ponto/services/cartorio.py:1597`). |
| “Minutos realizados suficientes provam ausência de furo.” | Pontas casadas podem englobar intervalo sem batida. CSF exige todas as células casadas, além de realizado≥previsto (`app/ponto/supra_juiz.py:458`; `app/ponto/supra_juiz.py:466`; `app/ponto/tests/test_supra_juiz_cobranca.py:32`). |
| “Virou o dia civil, terminou o fato.” | Turno da véspera pode continuar aberto após meia-noite; o encerramento consulta abertura viva e fim previsto. A contagem do cartório atribui madrugada ao dia-turno (`CLAUDE.md:185`; `app/ponto/turnos.py:2105`; `app/ponto/services/cartorio.py:1368`). |
| “Outubro civil é competência outubro.” | Com corte 21, 20/10 encerra uma janela e 21/10 inicia a seguinte; o corte efetivo vem da empresa (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:36`). |
| “Vínculo ativo atual explica o passado.” | A geradora da célula governa o dia dentro da vigência; geradora fora da vigência não autoriza substituir silenciosamente pelo vínculo novo. DNA refeito depois da cobrança também não prova que o marco nunca existiu (`app/escala/alimentacao.py:269`; `app/ponto/management/commands/supra_juiz.py:264`). |
| “Sem célula, ata ou lavra significa sem furo.” | `sem_celula` é ausência de jurisdição; `trabalha=None` sem ata não declara folga; executor sem lavra conta `CSF_SEM_LAVRA`, sem retratar (`app/ponto/supra_juiz.py:38`; `app/chamados/juizes.py:420`; `app/ponto/management/commands/supra_juiz.py:245`). |
| “Ausência aprovada cala o dia inteiro.” | Ausência parcial com período pode deixar resto missing cobrável (`app/chamados/juizes.py:398`; `app/chamados/juizes.py:424`; `app/ponto/supra_juiz.py:530`). |
| “Dia ausente do carregamento significa fato ausente.” | A alimentação exige janela completa; chave ausente não distingue célula inexistente de célula não carregada. O lote do cartório alcança todas as células passadas; `julgar_colab` não impõe sozinho piso de competência (`app/escala/alimentacao.py:14`; `app/escala/alimentacao.py:240`; `app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/services/cartorio.py:912`). |
| “Consultar CSF antes garante decisão concorrente atual.” | CSF percorre relações sem trava própria; as portas recarregam/travam o chamado e a costura usa a mesma linha. Resposta concorrente entre a consulta CSF e o fecho, sua serialização e dano efetivo: [não verificado] (`app/ponto/supra_juiz.py:543`; `app/chamados/models.py:668`; `app/chamados/services/fio.py:752`). |
| “`CSF_RETRATADO=0` prova cobertura do evento.” | O contador só sobe no executor, após retorno verdadeiro de `retratar`; fechamentos do cartório usam outros contadores. Zero isolado não demonstra passagem pela guarda no evento (`app/ponto/management/commands/supra_juiz.py:286`; `app/ponto/services/cartorio.py:1524`; `app/chamados/models.py:275`). |

## 4 LISTA DE CASOS RED

Os casos abaixo são casos de contrato; sua ocorrência atual em produção é [não verificado]. As expectativas vêm da O168, do complemento fornecido e das leis indicadas, não de comparação com a implementação antiga (`app/docs/BACKLOG.md:1379`; `avais.txt:1`, anexo).

| Caso | Cadastro + estado + ato | O que a regra exige |
|---|---|---|
| RED-01 — resposta na origem | Dia de trabalho completo; cobrança viva; pergunta de `disputa_origem` respondida; evento tenta fechar como CSF. | Preservar da retratação automática: conversa exige decisão humana (`app/docs/BACKLOG.md:1379`; `app/ponto/supra_juiz.py:540`). |
| RED-02 — resposta no destino | Mesmo cadastro, pergunta respondida ligada por `chamado_destino`; fecho automático pelo cartório. | A mesma proteção do RED-

[truncado em 20000 de 30468 caracteres]

## 05/11 16:48 · sessao a44cf8dd5ae4 · PD-O261-ITEM3-PEP · censo · origem=Code

HASH LIDO = a026943099b8d88468240fd4b18491141b6810af — correspondência com objeto Git [nao verificado].

## 0. Localização de “item 3” e “PEP”

Há três numerações possíveis, sem escolher entre elas:

| Localização | O que “item 3” designa |
|---|---|
| Fila recente **O261-E-F2-FILA** | “carimbar sem select_for_update, carimbo em memoria apos erro pos-C6”. O RELATO separa memória, registrada como curada, de trava, registrada como parada. `app/docs/BACKLOG.md:1154`; `app/docs/RELATO.md:66`. |
| Corte **FIO-MORTO-E-ESTRUTURAL** | Os três leitores: fábrica de perguntas, supra_juiz e registro `_C1`. `app/docs/CORTES.md:64`; `app/docs/PROMPTS.md:1184`; `app/core/juizes.py:100`. |
| Ordem histórica de 18/10 | Contador de fio morto, esperado zero fora do dia de hoje. `app/docs/RELATO-ARQUIVO.md:368`. |

O aval MANHA-0511 escreve literalmente **“O261 item 3 PEP no carimbar”**; BACKLOG e RELATO repetem a ordem. O pouso documentado da F2-FILA informa a restauração da memória, mas não declara a metade trava resolvida. `app/docs/PROMPTS.md:1853`; `app/docs/BACKLOG.md:1738`; `app/docs/RELATO.md:39`; `app/docs/RELATO.md:53`; `app/docs/RELATO.md:66`.

**Policy Enforcement Point** aparece por extenso no aval **O261-VETO-NA-PORTA**, associado à aplicação do veto dentro da porta única. Sua equivalência com “PEP” no item MANHA é uma leitura possível, sem definição literal nesse item **[nao verificado]**. As ocorrências “PEP 562” do arquivo histórico tratam de módulos Python. `app/docs/PROMPTS.md:1656`; `app/docs/PROMPTS.md:1853`; `app/docs/RELATO-ARQUIVO.md:17996`; `app/docs/RELATO-ARQUIVO.md:28267`.

## 1. Quem escreve e quem lê

**Comportamento atual de `carimbar`:** recebe uma instância, aplica o carimbo nela, consulta C6 e devolve `False` restaurando a memória se a pergunta continuar viva. Se C6 aceitar, pode lavrar o veredito e salva os campos. Exceções do juiz, da lavra ou do salvamento restauram a memória. O corpo não relê nem trava a linha recebida. `app/chamados/models.py:2551`; `app/chamados/models.py:2572`; `app/chamados/models.py:2584`; `app/chamados/models.py:2592`; `app/chamados/models.py:2605`.

| Campo ou conjunto | Escritores atuais | Leitores e finalidade |
|---|---|---|
| `materializada_em`, `materializada_por` | `_escrever_carimbo`; limpeza persistida em `descarimbar_em_lote`. As demais portas delegam a esses escritores. `app/chamados/models.py:2545`; `app/chamados/models.py:2635`; `app/chamados/models.py:2644`. | C6 lê o instante; `estado` combina validação, carimbo e C6; procedência lê autor e instante. `app/chamados/juizes.py:809`; `app/chamados/juizes.py:1077`; `app/chamados/models.py:2481`; `app/ponto/services/procedencia_batida.py:69`. |
| `via_resolucao` | Escritor da tripla, limpeza em lote, `encerrar_por_fato_no_espelho` e `relavra_via.relavrar`. Portanto, “porta única da tripla” não significa um único escritor da via isoladamente. `app/chamados/models.py:2549`; `app/chamados/models.py:2652`; `app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`. | C6, juiz do lastro, SQL de vivas, relavra, cartório e apresentação. `app/chamados/juizes.py:645`; `app/chamados/juizes.py:1112`; `app/chamados/models.py:2734`; `app/chamados/services/relavra_via.py:116`; `app/ponto/services/cartorio.py:1599`; `app/templates/chamados/partials/_card_atendimento.html:186`. |
| `respondida_em` | `carimbar`, quando solicitado e há texto; resposta humana; resposta automática; recuperação pela trilha; reabertura e ressuscitação. `app/chamados/models.py:2576`; `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/models.py:1493`. | C6, estado, validação, filas e API. `app/chamados/juizes.py:1092`; `app/chamados/models.py:2491`; `app/chamados/services/validacao.py:363`; `app/chamados/juizes.py:1339`; `app/api/views.py:2636`. |
| `validada_em`, `validada_por` | `carimbar` pode preencher ambos com `resposta='validar'`; validação os preenche ou devolve; resposta nova, correção e ressuscitação os limpam. `app/chamados/models.py:2580`; `app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:187`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`. | C6 encerra pela validação antes dos demais degraus; estado e materialização também a leem; procedência prefere seu autor ao autor da materialização. `app/chamados/juizes.py:1078`; `app/chamados/models.py:2488`; `app/chamados/services/materializacao.py:1294`; `app/ponto/services/procedencia_batida.py:72`. |
| `resposta_colab` | `carimbar` apenas lê o texto. Escritores: resposta humana, automática e limpeza para correção. `app/chamados/models.py:2576`; `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1428`. | C6, derivação do slot, materialização, API, calendário, ficha e prévia da folha. `app/chamados/juizes.py:324`; `app/chamados/juizes.py:1092`; `app/chamados/services/materializacao.py:1183`; `app/api/views.py:2580`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | A opção `lavrar_veredito` chama `veredito_lavrado.lavrar`; a restauração passa por `desfazer_lavra_em_memoria`; persistência em lote tem porta própria. `app/chamados/models.py:2593`; `app/chamados/veredito_lavrado.py:211`; `app/chamados/veredito_lavrado.py:240`; `app/chamados/veredito_lavrado.py:197`. | `esta_stale` verifica ausência, expiração e mudança de insumos; raio-X consome a lavra para determinar ausência de ação. `app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:173`. |
| `recusa_motivo` | A porta de identidade pode escrevê-lo; materialização, resposta de escape e recusa humana também escrevem. `app/chamados/models.py:2630`; `app/chamados/services/materializacao.py:817`; `app/chamados/services/materializacao.py:1069`; `app/chamados/services/disputa_emissao.py:1340`; `app/chamados/services/validacao.py:545`. | Validação fotografa e relê a recusa; passivo a conserva no conjunto de reversão. `app/chamados/services/validacao.py:136`; `app/chamados/services/validacao.py:180`; `app/chamados/services/passivo_validado.py:64`. |
| `_pv_memo` | `pergunta_viva` grava o memo; descarimbo e restauração o removem. `app/chamados/juizes.py:833`; `app/chamados/models.py:2612`; `app/chamados/models.py:2638`. | C6 reutiliza o resultado quando a chave da instância coincide; a chave não contém `materializada_por`. `app/chamados/juizes.py:809`; `app/chamados/juizes.py:835`. |

**Chamadores do carimbo comum — oito pontos atuais:**

| Função chamadora | Ponto de chamada |
|---|---|
| `encerrar_fantasma` | `app/chamados/models.py:2674`. |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:405`. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:620`. |
| `resolver_perguntas_por_chamado_resolvido`, dois ramos | `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1675`. |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:546`. |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1199`. |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1346`. |

`_materializar_uma` já envolve plantio e carimbo em transação; a recusa do carimbo levanta `_Reprovada` dentro dela. Isso não equivale a uma releitura travada na própria `carimbar`. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1197`; `app/chamados/models.py:2572`.

**Identidade:** `carimbar_identidade` aceita quatro vias, salva sem consultar C6 e pode preservar instante/autor. Seus chamadores são decisão administrativa, duplicata, paridade e colisão. Uma resposta por validar pode continuar viva porque sua precedência vem antes da identidade. `app/chamados/models.py:2275`; `app/chamados/models.py:2614`; `app/chamados/models.py:2511`; `app/chamados/services/fio.py:891`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:1066`; `app/chamados/juizes.py:1092`.

**Caminhos inversos:** descarimbar atende julgamento de cópia, reabertura de fantasma, resposta nova, competência e desfazimento sem lastro; a versão em lote atende ressuscitação e correção. A ressuscitação também é chamada por desarquivamento e cartório. `app/chamados/models.py:1444`; `app/chamados/models.py:2685`; `app/chamados/models.py:2702`; `app/chamados/models.py:2709`; `app/chamados/services/relavra_via.py:231`; `app/chamados/models.py:1493`; `app/chamados/services/disputa_emissao.py:1412`; `app/chamados/views_arquivar.py:86`; `app/ponto/services/cartorio.py:1622`.

**Leitores adicionais do estado produzido:**

- App e tela usam C6 depois de seleção ampla; `q_vivas` declara continência, não igualdade com C6. `app/chamados/services/pergunta_no_app.py:42`; `app/chamados/views.py:1257`; `app/chamados/models.py:2723`.
- Materializador, validação e reconciliador usam presença/ausência do carimbo para selecionar trabalho ou impedir repetição. `app/chamados/services/materializacao.py:433`; `app/chamados/services/materializacao.py:529`; `app/chamados/services/validacao.py:422`; `app/chamados/reconciliador.py:366`; `app/chamados/reconciliador.py:583`.
- Fábrica lê carimbo e célula; seu corpo atual já consulta `VEREDITOS_ACUSAM`, diferentemente do corpo histórico descrito no anexo. `app/chamados/services/disputa_emissao.py:695`; `app/docs/CORTES.md:64`.
- Histórico gera evento do carimbo; painel agrega seu máximo; lavra usa o instante como marco de atualização; passivo guarda campos para reversão. `app/chamados/services/historico.py:103`; `app/chamados/services/painel.py:67`; `app/chamados/veredito_lavrado.py:163`; `app/chamados/services/passivo_validado.py:64`.
- Fora de chamados: fantasmas selecionam sem carimbo; supra_juiz lê resposta; API e mensageria expõem texto/instante; foto delega resposta e validação às portas. `app/ponto/management/commands/reconciliar_fantasmas.py:212`; `app/ponto/supra_juiz.py:545`; `app/api/views.py:1173`; `app/api/views_mensageria.py:567`; `app/api/foto_recebida.py:48`.
- Auditorias leem carimbo sem testemunha, carimbada ainda viva, ausência de via e validação sem materialização. `app/chamados/management/commands/auditar_invariantes_chamados.py:538`; `app/chamados/management/commands/auditar_invariantes_chamados.py:905`; `app/chamados/management/commands/auditar_invariantes_chamados.py:954`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`.

## 2. Juiz ou lei que já responde

| Pergunta | Autoridade existente |
|---|---|
| Esta pergunta está viva? | **C6 `pergunta_viva`**; é quem `carimbar` consulta sobre o candidato aplicado. `app/core/juizes.py:266`; `app/chamados/models.py:2585`. |
| Existe testemunha da materialização? | **`_pergunta_materializada_de_fato`**; também alimenta a lavra. É pergunta distinta da vitalidade, embora C6 a consulte no último degrau. `app/chamados/juizes.py:630`; `app/chamados/juizes.py:1155`; `app/chamados/veredito_lavrado.py:219`. |
| Alguma pergunta da coleção vive? O dia tem porta alcançável? | **C15 `fio_vivo`** e **C17 `porta_alcancavel_do_dia`**, respectivamente. `app/core/juizes.py:275`; `app/core/juizes.py:277`. |
| Chamado vivo significa cobrança viva? | O registro atual separa: **C1 `VIVOS`** responde pelo chamado; C6/C15 respondem pela pergunta/coleção. `app/core/juizes.py:100`; `app/core/juizes.py:261`; `app/core/juizes.py:266`; `app/core/juizes.py:275`. |
| O dia dispensa cobrança? | **C18 `dia_sem_trabalho`**; ausência parcial tem exceção explícita para o resto descoberto. `app/core/juizes.py:278`; `app/chamados/juizes.py:398`; `app/chamados/juizes.py:411`. |
| Qual competência e qual vínculo pertencem ao fato? | **`janela_atual`**, respeitando corte empresarial; **`vinculo_do_dia`**, com precedência da geradora da célula. `app/core/juizes.py:283`; `app/ponto/janelas.py:60`; `app/escala/alimentacao.py:228`. |

O próprio JUIZES.md se declara consulta de endereços e manda prevalecer o registro vivo. Ele também nomeia `validar_pergunta` como autoridade de validação; o código atual mostra que `carimbar` pode preencher os campos de validação em seu modo `validar`. Ambas as referências precisam constar no censo de escrita. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`; `app/docs/JUIZES.md:38`; `app/chamados/models.py:2580`.

As leis/cortes aplicáveis já escritos são:

- **L-061 e corte estrutural O261:** consumidor lê célula/ata; vida do fio deriva da lâmpada; carimbo não substitui o fato. `app/docs/LEIS.md:95`; `app/docs/PROMPTS.md:1184`.
- **L-007:** escritor canônico e trilha de mudança; **ESMERIL-V2-PROPRIEDADES:** invariantes também nas intercalações responder/validar/recusar/fechar/reabrir/lote. `app/docs/LEIS.md:65`; `app/docs/CORTES.md:333`.
- **L-110:** resultado esperado nasce da regra e cadastro, antes do código. `app/docs/LEIS.md:136`.
- **RELOGIO-INJETADO:** juiz e montador recebem o instante do chamador; a fila registra essa parte ainda separada da metade trava. `app/docs/CORTES.md:370`; `app/docs/RELATO.md:66`.
- **Teto e chegada da batida:** célula provisória não absolve o marco ainda pendente; batida que responde ao marco absolve na passada seguinte. `app/chamados/juizes.py:1127`; `app/docs/CORTES.md:413`.

O mecanismo exato de concorrência autorizado pela abreviação “PEP no carimbar” não está explicitado no aval; o RELATO registra uma pergunta de desenho com duas candidatas **[nao verificado]**. `app/docs/PROMPTS.md:1853`; `app/docs/RELATO.md:66`.

## 3. Contra-exemplos pela regra do aval

| Leitura óbvia que erra | Contra-exemplo e comportamento atual |
|---|---|
| “C6 aprovou; a decisão necessariamente descreve a linha que será gravada.” | A porta julga a instância recebida e salva depois; não relê a linha no corpo. Uma alteração entre coleta e gravação não entra automaticamente nessa fotografia. Ocorrência efetiva em produção **[nao verificado]**. `app/chamados/models.py:2572`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`. |
| “Carimbada é morta.” | Resposta por validar precede identidade; célula acusadora também pode manter carimbo comum vivo. `app/chamados/juizes.py:1092`; `app/chamados/juizes.py:1113`; `app/chamados/juizes.py:1125`. |
| “O dia acusa, então toda pergunta desse dia vive.” | O marco perguntado pode estar provado aceso enquanto outro marco mantém o dia acusador; o marco é consultado antes do veredito diário. `app/chamados/juizes.py:1115`. |
| “Concorde autoriza carimbar.” | Veredito provisório com marco pendente não absolve sem prova do marco perguntado. A chegada da batida altera essa condição. `app/chamados/juizes.py:1127`; `app/chamados/juizes.py:1136`; `app/docs/CORTES.md:413`. |
| “Célula sem ata equivale a dia resolvido.” | `_celula_furada` devolve desconhecido quando não há lâmpadas lavradas. Entretanto, C6 pode absolver célula com veredito não acusador e ata vazia; existe teste esperando essa morte. A expectativa específica do aval para essa combinação é **[nao verificado]**. `app/chamados/juizes.py:471`; `app/chamados/juizes.py:1137`; `app/chamados/tests/test_o261_pergunta_viva_teto.py:80`. |
| “Virou o dia civil; o fato encerrou.” | O corte exige fato encerrado, não `data < hoje`; marcos pendentes continuam relevantes. `app/docs/PROMPTS.md:1180`; `app/chamados/juizes.py:1127`. |
| “20 e 21 pertencem ao mesmo mês de folha.” | Com corte 21, 20/10 encerra competência 10 e 21/10 inicia competência 11. A tranca consulta a competência de `data_evento` com a empresa. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/chamados/juizes.py:933`. |
| “Resposta 00:00 pertence ao dia da pergunta.” | No 12x36 19–07 com intervalo 23–00, a volta 00:00 pertence a D+1. O plantio usa cadeia de marcos; a prova histórica não pode perder sua data por troca posterior de escala. `app/chamados/juizes.py:283`; `app/chamados/juizes.py:692`. |
| “O vínculo ativo hoje julga o passado.” | A geradora da célula tem precedência; célula lavrada continua julgável após encerramento do vínculo. Alimentação de célula de outro dia/pessoa é recusada. `app/escala/alimentacao.py:228`; `app/chamados/juizes.py:458`; `app/chamados/juizes.py:619`. |
| “Fora da janela, toda pergunta morreu” ou “todo carimbo automático ressuscita.” | C6 consulta célula acusadora antes da janela; já o lastro pode aceitar via sem testemunha fora dela. A janela desse ramo inclui anterior, corrente e véspera, calculadas sem empresa explícita. `app/chamados/juizes.py:1125`; `app/chamados/juizes.py:554`; `app/chamados/juizes.py:645`. |
| “C6 morto pela tranca permite carimbo definitivo.” | A tranca mata por leitura e pode ser reversível; os consumidores E2 e resolução de chamado têm guarda própria para não converter isso em carimbo. `app/chamados/juizes.py:917`; `app/chamados/reconciliador.py:612`; `app/chamados/services/disputa_emissao.py:1587`. |

## 4. Lista de casos RED

Os cadastros abaixo são especificações de ensaio pela regra; ocorrência e execução na produção **[nao verificado]**. A fonte exigida para o resultado esperado é regra e cadastro. `app/docs/LEIS.md:136`.

| Caso | Cadastro + estado | Ato | O que a regra exige |
|---|---|---|---|
| **R1 — decisão humana intercalada** | Mesma pergunta; consumidor conserva instância anterior; admin decide o dia entre coleta e escrita. | Consumidor tenta carimbo automático antigo. | A decisão humana não pode desaparecer sob escrita baseada no estado anterior; mudança conserva trilha. A mesma perda está descrita no caminho inverso. `app/docs/LEIS.md:65`; `app/chamados/services/relavra_via.py:214`. |
| **R2 — resposta nova intercalada** | Pergunta previamente validada; colaborador entrega resposta nova enquanto outro ato conserva a versão anterior. | Ato antigo tenta concluir/carimbar. | Validação anterior não vale para resposta nova; sua voz e a mudança de estado permanecem registradas. `app/chamados/services/disputa_emissao.py:1287`; `app/chamados/services/disputa_emissao.py:1303`; `app/docs/CORTES.md:333`. |
| **R3 — erro depois de C6** | Pergunta sem carimbo, com fotografia anterior dos campos e da lavra. | Lavra ou salvamento lança erro após C6. | Instância não fica declarando um ato que não foi gravado. Existem casos para erro de save, lavra e save após lavra. `app/chamados/tests/test_o261e_f2_fila.py:179`; `app/chamados/tests/test_o261e_f2_fila.py:190`; `app/chamados/tests/test_o261e_f2_fila.py:200`. |
| **R4 — identidade com resposta discordante** | Pergunta respondida, não validada; resposta não concorda com a luz; via duplicata. | Carimbar identidade. | Identidade não apaga a resposta que ainda exige decisão administrativa; a precedência da resposta permanece. `app/chamados/juizes.py:1092`; `app/chamados/models.py:2614`. |
| **R5 — marco respondido, outro furo** | Dia acusador; marco desta pergunta aceso; outro marco apagado. | Encerrar a pergunta do marco aceso. | Não continuar perguntando o fato já testemunhado; o outro furo permanece separado. `app/chamados/models.py:2257`; `app/chamados/juizes.py:1115`. |
| **R6 — borda de instante** | Célula provisória não acusadora, com marco da pergunta pendente e sem batida correspondente. | Tentar carimbar antes do fato encerrado; depois entregar a batida e rejulgar. | Antes, não absolver por veredito provisório; depois, reconhecer a batida na passada seguinte. `app/chamados/juizes.py:1127`; `app/docs/CORTES.md:413`. |
| **R7 — competência 21→20** | Empresa corte 21; fatos em 20/10 e 21/10; competência 10 trancada e 11 aberta. | Consultar tranca/carimbar cada pergunta. | Aplicar competências diferentes; tranca não fabrica quitação do fato nem batida. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/chamados/juizes.py:1080`. 

[truncado em 20000 de 24716 caracteres]

## 05/11 16:47 · sessao 56b0d86bad52 · PD-PWA-OFFLINE-APP-SHELL · censo · origem=Code

HASH LIDO = a026943099b8d88468240fd4b184911b6810af

## 1 QUEM ESCREVE e QUEM LÊ

As referências `avais.txt (anexo):N` e `backlog_obras.txt (anexo):N` numeram as entradas na ordem fornecida; sua existência como arquivos locais é [nao verificado]. Correspondência do hash informado com um commit Git: [nao verificado]. [nao verificado]

**Cache, registro do SW e cascas**

| Campo, chave ou função | Quem escreve | Quem lê e comportamento atual |
|---|---|---|
| `CACHE_NAME = juliani-<build>` | `build_id_do_sw()` calcula SHA1 truncado do conteúdo de SW, PWA, manifest e dois ícones; `service_worker()` substitui `__BUILD_ID__`. | Instalação e gravações usam esse nome; ativação apaga **todos** os outros caches, sem restringir pelo prefixo `juliani-`. (`app/core/views.py:524–556`; `app/core/views.py:560–576`; `app/static/sw.js:1–20`, `app/static/sw.js:59–74`.) |
| `_CACHE_DO_BUILD['v']` | `build_id_do_sw()` grava `(agora, build)`. | A mesma função reutiliza o valor durante 30 segundos; templates não integram `GOVERNADOS_PELO_SW`. (`app/core/views.py:520–556`.) |
| `/static/pwa.js`, `/static/manifest.json`, `/static/icons/icon-192.png`, `/static/icons/icon-512.png` | Evento `install`, por `cache.addAll(STATIC_CACHE)`. | Ramo `/static/`, por `caches.match(event.request)`, antes da rede. (`app/static/sw.js:3–12`; `app/static/sw.js:53–63`.) |
| Demais chaves `/static/...` | Primeira busca sem cópia: clona a resposta e chama `cache.put(Request, clone)`, sem condição explícita de status. | Mesmo ramo estático; a busca por `caches.match` consulta os caches globalmente. (`app/static/sw.js:54–60`.) |
| Chaves `/` e `/ponto/bater/...` | Ramo `comCache` grava a resposta inteira quando alcançado. | Na navegação, o ramo anterior retorna após rede/fallback: **não alcança o escritor**. Requisições que não sejam navegação podem alcançar a gravação. (`app/static/sw.js:45–79`.) |
| `doCacheOuErroDeRede(Request)` | Não escreve. | Navegação, `comCache` e ramo restante chamam a função; ela devolve cópia existente ou `Response.error()`. Login, logout, admin e onboarding usam somente rede; métodos diferentes de GET ficam fora da interceptação. (`app/static/sw.js:29–48`; `app/static/sw.js:77–84`.) |
| Registro e atualização | `registrarSW()` registra `/sw.js`, escopo `/`; atualiza imediatamente, a cada 15 minutos, no foco e no evento `online`. | As duas bases carregam `pwa.js`; `install` usa `skipWaiting`, `activate` usa `clients.claim`; `controllerchange` recarrega uma vez se nenhum formulário recebeu `input`. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/static/pwa.js:11–45`; `app/static/sw.js:10–23`.) |
| Escolha `base_tpl` | `app_shell()` consulta `modo_app()` e fornece `base.html` ou `base_app.html`. | Templates que estendem `base_tpl` consomem a escolha; V2 e base da triagem fixam `base.html`. (`app/core/context_processors.py:194–210`; `app/core/utils.py:45–75`; `app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`.) |
| Meta `user-perfil` | Ambas as bases escrevem `colaborador` quando `request.user.is_staff` é falso. | Sincronização, inscrição push e ping geográfico leem essa meta; ela não contém identificador de colaborador. (`app/templates/base.html:16`; `app/templates/base_app.html:16`; `app/static/pwa.js:128`; `app/static/pwa.js:332–340`; `app/static/pwa.js:396`.) |

O HTML atual contém dados de usuário: `base.html` mostra username; triagem mostra nome/matrícula; dashboard mostra nome, matrícula e empresa; V2 injeta tipo sugerido e coordenadas/raio do posto. Cache pessoal efetivamente encontrado em aparelho real: [nao verificado]. (`app/templates/base.html:316`; `app/templates/ponto/triagem/base_triagem.html:10–11`; `app/templates/core/dashboard_colaborador_v2.html:14–16`; `app/templates/ponto/bater_ponto_v2.html:104`; `app/templates/ponto/bater_ponto_v2.html:139–140`.)

**IndexedDB, itens da fila e armazenamento auxiliar**

| Campo, chave ou função | Quem escreve | Quem lê |
|---|---|---|
| Banco `juliani-offline`, versão `2`; stores `batidas_pendentes` e `config` | `abrirDB()` abre o banco; no upgrade cria stores ausentes. A fila tem `id` autoincremental; configuração usa `chave`. | Gravação, sincronização, contador e funções de configuração abrem o mesmo banco, sem separação por colaborador ou versão do SW. (`app/static/pwa.js:48–79`; `app/static/pwa.js:107–132`; `app/static/pwa.js:184–190`.) |
| `config: {chave, valor}` | `salvarConfig()`, por `put`. | `lerConfig()`, por `get(chave).valor`. (`app/static/pwa.js:63–79`.) |
| `ntp_gap` | `calcularESalvarGap()` calcula diferença usando `/hora/` e metade da latência. | Fallback dessa função e `obterGap()`; o gap alimenta a gravação offline e `JulianiPWA.agora()`. (`app/static/pwa.js:83–109`; `app/static/pwa.js:305–308`.) |
| `ntp_calculado_em` | `calcularESalvarGap()`, com `Date.now()`. | `obterGap()` decide recalcular após 30 minutos. (`app/static/pwa.js:93–104`.) |
| Item: `id` | IndexedDB, automaticamente. | Sincronizador usa apenas para excluir o item local; não envia esse identificador à API. (`app/static/pwa.js:54`; `app/static/pwa.js:143–175`.) |
| Item: `tipo` | `registrarBatidaOffline()` recebe do chamador. | Sincronizador envia no multipart; API valida E/S e pode alterar o tipo segundo a autoridade de jornada. (`app/static/pwa.js:107–119`; `app/static/pwa.js:144`; `app/api/views.py:432–439`; `app/api/views.py:596–631`.) |
| Item: `timestamp` corrigido | Gravação calcula `Date.now() + gap`, depois de aguardar `obterGap()`. | Sincronizador ordena por esse campo; não o envia. (`app/static/pwa.js:107–119`; `app/static/pwa.js:136–157`.) |
| Item: `timestamp_dispositivo` | Gravação usa `new Date().toISOString()`. | Sincronizador envia; API entrega a `hora_do_aparelho()` e grava a hora lida em `Batida.timestamp_dispositivo`. (`app/static/pwa.js:115`; `app/static/pwa.js:147`; `app/api/views.py:536–539`; `app/api/views.py:682–695`.) |
| Item: `timestamp_gap_ms` | Gravação conserva o gap utilizado. | Não entra no multipart atual; tampouco é preenchido pela chamada atual da API ao chokepoint. (`app/static/pwa.js:116`; `app/static/pwa.js:143–157`; `app/api/views.py:682–697`.) |
| Item: `latitude`, `longitude` | Gravação recebe dos chamadores. | Sincronizador envia quando não nulos; API exige coordenadas e as entrega ao chokepoint. (`app/static/pwa.js:107–118`; `app/static/pwa.js:145–146`; `app/api/views.py:432–466`; `app/api/views.py:682–695`.) |
| Item: `gps_accuracy`, `gps_captured_at` | Gravação recebe dos chamadores. | São guardados, mas não entram no multipart do sincronizador. A API aceita accuracy quando recebida por outros caminhos. (`app/static/pwa.js:117`; `app/static/pwa.js:143–157`; `app/api/views.py:435`.) |
| Item: `foto` | Gravação recebe o blob ou guarda `null`. | Sincronizador envia como `selfie.jpg`; API chama a porta de foto, que escreve `FotoBatida` e `Batida.foto`. (`app/static/pwa.js:118`; `app/static/pwa.js:152`; `app/api/views.py:783–789`; `app/ponto/registro_batida.py:296–311`.) |
| Identidade do colaborador no item | O objeto gravado não contém colaborador, usuário, vínculo ou sessão. | Sincronização usa meta de perfil e CSRF atual; **a identidade efetiva vem de `request.user.colaborador`**, nos dois kernels. (`app/static/pwa.js:113–119`; `app/static/pwa.js:128–157`; `app/api/views.py:398–404`; `app/api/views_core.py:385–391`.) |
| Exclusão e contador da fila | Sincronizador exclui após resposta `ok` com JSON válido ou HTTP 400. | Outros status e exceções conservam o item; `atualizarIndicadorOffline()` conta a store inteira, sem distinguir colaborador. (`app/static/pwa.js:158–201`.) |
| `avisos_convite` | Botões adiar/ligar escrevem `visto` em sessionStorage. | `oferecerAvisos()` consulta para evitar outro convite na sessão. (`app/static/pwa.js:351–366`.) |
| `gps_aviso_visto` | Confirmação offline da triagem escreve `1` em sessionStorage. | O mesmo fluxo consulta antes do aviso. (`app/templates/ponto/triagem/apto.html:278–286`.) |
| `scrollpos:<pathname>` | Submit POST clássico nas duas bases escreve posição em sessionStorage. | Load lê, remove e restaura o scroll. A fila de batidas usa IndexedDB. (`app/templates/base.html:406–416`; `app/templates/base_app.html:161–171`; `app/static/pwa.js:107–132`.) |

Os escritores que chamam `registrarBatidaOffline()` estão nos dois caminhos da V2, nos dois caminhos do template antigo e na triagem apta. (`app/templates/ponto/bater_ponto_v2.html:356`; `app/templates/ponto/bater_ponto_v2.html:374`; `app/templates/ponto/bater_ponto.html:543`; `app/templates/ponto/bater_ponto.html:597`; `app/templates/ponto/triagem/apto.html:289`.)

`sincronizarBatidas()` é acionada por inicialização online, evento `online` e mensagem `SYNC_BATIDAS`; o SW envia essa mensagem a todos os clientes quando recebe `sync-batidas`. Sem cliente aberto, mostra notificação; não executa os POSTs da fila nesse ramo. (`app/static/pwa.js:15–16`; `app/static/pwa.js:320–328`; `app/static/sw.js:112–129`.)

**Entrada web, API, chokepoint e leitores externos**

- `/ponto/bater/` e `/ponto/bater-v2/` chamam `bater_ponto()`: identifica o colaborador pela sessão, monta triagem e encaminha POST E/S a `ramo_registrar_es()`. A primeira rota termina em template de triagem; a segunda seleciona V2. A fila envia a `/api/ponto/bater/`, presente nos dois kernels; `/ponto/batida-offline/` retorna 410. (`app/ponto/urls.py:11–13`; `app/ponto/views.py:241–260`; `app/ponto/views.py:346–358`; `app/ponto/views.py:417–428`; `app/api/urls.py:67`; `app/api/urls_core.py:14`.)
- A API lê tipo, coordenadas, accuracy, foto e timestamp do dispositivo; obtém hora efetiva pelo juiz de horário; deduplica mesmo tipo em ±5 minutos; calcula sequência por jornada; consulta ordem de substituição com `agora`; julga tipo ao vivo ou retroativo; chama `registrar_batida()`. (`app/api/views.py:432–439`; `app/api/views.py:527–631`; `app/api/views.py:682–697`.)
- `registrar_batida()` sanitiza geografia; deduplica mesmo tipo em ±60 segundos e mesma chave colaborador/instante; distingue fato de chão de escrita institucional na guarda de competência; verifica colisão oposta e exceção de marco próprio no dia/véspera; cria `Batida` e aciona fechamento por evento. (`app/ponto/registro_batida.py:5–50`; `app/ponto/registro_batida.py:94–161`; `app/ponto/registro_batida.py:175–183`.)
- Fora das entradas iniciais, também chamam essa criação: ações de chamado, materialização, regularização externa, saída administrativa, justificativas e veredito de célula. (`app/chamados/services/acoes_chamado.py:44`; `app/chamados/services/materializacao.py:894`; `app/chamados/services/materializacao.py:974`; `app/chamados/services/regularizacao_ext.py:77`; `app/ponto/views.py:1816`; `app/ponto/services/justificativa.py:173`; `app/ponto/services/justificativa.py:346`; `app/ponto/services/veredito_celula.py:301`.)
- A família de escrita também contém `anotar()`, `retratar_batida()`, `apagar()` e `anexar_foto()`. A tela de pendentes usa essas portas para aprovar ou rejeitar; essa tela lê `origem='offline_pendente'`, enquanto a API atual grava `origem='app'`. (`app/ponto/registro_batida.py:235–311`; `app/ponto/views.py:434–479`; `app/api/views.py:683–695`.)
- Leitores externos de horário/gap: contador `hora_do_aparelho.descartadas()`, tela de pendentes e resposta `gap_ms` de `core/views.py`. Leitores de accuracy: geofence e levantamento de vínculo; leitor do canal gravado: cobrança retroativa de selfie. (`app/api/hora_do_aparelho.py:156–185`; `app/templates/ponto/batidas_offline_pendentes.html:71–90`; `app/core/views.py:1022`; `app/ponto/services/geofence.py:265`; `app/ponto/services/geofence.py:406`; `app/ponto/services/vinculo_na_lista.py:49`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:74`.)
- O `post_save` da batida lê colaborador/timestamp para recomputar turnos entre D−2 e D+1 e publica evento de tempo real quando criada; o julgamento de células consulta D−1 e D. (`app/ponto/signals.py:19–45`; `app/ponto/signals.py:85–112`.)
- A recarga de “5 min” usa `_vistoEm`: ao voltar a `visible`, recarrega se transcorreram **mais de** 300.000 ms; não é timer contínuo. V2 e PWA global também recarregam em `pageshow.persisted`, sem consulta à conectividade nesses trechos. (`app/templates/ponto/bater_ponto_v2.html:305–310`; `app/static/pwa.js:430–431`.)
- No ciclo compartilhado de `pwa.js`, instalação usa `_installPrompt`; push usa assinatura/VAPID e CSRF; ping envia coordenadas/accuracy apenas online; persistência solicita `navigator.storage.persist()` sem bloquear o fluxo. (`app/static/pwa.js:3–8`; `app/static/pwa.js:225–286`; `app/static/pwa.js:380–442`.)

## 2 JUIZ ou LEI que já responde

| Pergunta | Autoridade existente e alcance |
|---|---|
| O que esta obra exige? | O aval fornecido exige casca offline sem dado pessoal, identidade na fila, ausência de recarga offline, envio único com hora do aparelho, foto/GPS intactos e Safari iOS medido. O corte define App Shell, vizinho Offline Fallback, e nomeia a armadilha do HTML de usuário em cache. (`avais.txt (anexo):7–8`.) |
| O fallback pode devolver ausência de resposta? | **Offline Fallback** já está no glossário e em `doCacheOuErroDeRede()`: cópia ou `Response.error()`. Isso responde à forma da resposta, não à existência de uma casca guardada. (`app/docs/GLOSSARIO.md:29`; `app/static/sw.js:26–30`.) |
| As duas cascas entram na prova? | **L-071 e L-072** respondem conjuntamente: duas cascas e smoke de alteração de SW/template base nas duas. (`app/docs/LEIS.md:98–99`; `CLAUDE.md:199–200`.) |
| Quem responde por hora, tipo, sequência e meia-noite? | `hora_do_aparelho()` responde pelo instante; `sequencia_na_jornada()`, `proximo_tipo_de()` e os juízes de marcos/turno respondem por jornada e tipo. A API consome essas autoridades; `core/juizes.py` declara a família turno/marcos. (`app/api/hora_do_aparelho.py:124–153`; `app/api/views.py:584–631`; `app/core/juizes.py:195–204`.) |
| Quem responde por vínculo e competência? | `vinculo_do_dia()` responde pelo vínculo histórico; `escala_vigente()` carrega seus insumos. `ponto/janelas.py` responde pela competência conforme cadastro da empresa; L-026/L-090 afastam mês civil e corte fixo universal. (`app/escala/alimentacao.py:228–286`; `app/escala/servico_jornada.py:15–58`; `app/ponto/janelas.py:10–63`; `app/docs/LEIS.md:77`; `app/docs/LEIS.md:123`.) |
| Quem responde por GPS e selfie? | `classificar_posicao()` julga posição; `canal_da_batida()` identifica PWA/APK; `selfie_ausente()` julga ausência com prazo por canal. O registro vivo e os cortes nomeiam essas autoridades. (`app/ponto/services/geofence.py:95–123`; `app/ponto/registro_batida.py:53–91`; `app/core/juizes.py:1173–1183`; `app/docs/CORTES.md:282–283`.) |
| Pode barrar fato de chão por competência fechada? | **L-030** nomeia o chokepoint e a proteção da batida de chão; a guarda de competência em `registrar_batida()` só se aplica quando `fato_de_chao=False`. (`app/docs/LEIS.md:78`; `app/ponto/registro_batida.py:123–137`.) |
| Ausência de ata significa folga? | `dia_sem_trabalho()` responde: `trabalha=None` não equivale a folga; ata vazia não declara tipo de dia. O corte correspondente está registrado. (`app/chamados/juizes.py:411–426`; `app/docs/CORTES.md:416`.) |
| Código atual pode ser gabarito dos RED? | **L-110** exige casos respondidos pela regra e cadastro; `JUIZES.md` declara ser lookup e remete ao registro vivo. (`app/docs/LEIS.md:136`; `app/docs/JUIZES.md:7–15`.) |

O aval fornecido homologa a batida 146850, col389, PWA 14:40 para o smoke anterior e manda não reabrir BUG-SW-RESPOND-UNDEFINED; isso é dado do anexo, não consulta realizada a banco/log. (`avais.txt (anexo):7`.)

## 3 CONTRA-EXEMPLOS pela regra do aval

| Leitura óbvia que erra | Contra-exemplo e comportamento atual |
|---|---|
| “Abriu online uma vez, portanto o HTML ficou disponível offline.” | Navegação retorna antes de `cache.put`; instalação guarda somente quatro recursos estáticos. O teste existente fornece cache pronto, com `put` vazio: não prova nascimento da cópia. (`app/static/sw.js:3–12`; `app/static/sw.js:45–79`; `app/core/tests/test_sw_offline_fallback.py:33–45`.) |
| “Guardar o HTML atual cumpre a casca sem dados pessoais.” | Bases, triagem e dashboard incluem dados de usuário; o ramo `comCache` clona a resposta inteira. O corte distingue casca de HTML pessoal. (`app/templates/base.html:316`; `app/templates/ponto/triagem/base_triagem.html:10–11`; `app/templates/core/dashboard_colaborador_v2.html:14–16`; `app/static/sw.js:68–75`; `avais.txt (anexo):8`.) |
| “A fila pertence a quem bateu.” | A deixa fila, B entra no mesmo aparelho: o item não identifica A e a API usa B autenticado. O cadastro de identidade exigido pelo aval não está no objeto atual; ocorrência real dessa troca é [nao verificado]. (`app/static/pwa.js:113–119`; `app/static/pwa.js:128–157`; `app/api/views.py:398–404`; `avais.txt (anexo):7–8`.) |
| “Tela aberta prova sessão válida.” | Offline, o perfil permanece no HTML; a autenticação é exigida somente no POST à API. Respostas não `ok` diferentes de 400 conservam o item; a meta não verifica validade da sessão. (`app/templates/base.html:16`; `app/static/pwa.js:128–177`; `app/api/views.py:398–404`.) |
| “SW novo implica fila nova.” | Ativação remove caches; IndexedDB permanece com nome fixo/versão 2. Upgrade só cria stores ausentes; sincronização lê itens antigos sem campo de versão ou identidade. (`app/static/sw.js:17–20`; `app/static/pwa.js:48–58`; `app/static/pwa.js:126–157`.) |
| “As duas cascas percorrem o mesmo caminho por usar PWA.” | Ambas carregam PWA, mas V2 e triagem fixam `base.html`; `/ponto/bater-v2/` também não corresponde ao prefixo `/ponto/bater/` do ramo `comCache`. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`; `app/static/sw.js:68–69`; `app/ponto/urls.py:12`.) |
| “Cinco minutos sempre provocam recarga.” | O disparo exige retorno à visibilidade e diferença estritamente maior que 300.000 ms; não verifica rede. Retorno por bfcache tem disparos adicionais. (`app/templates/ponto/bater_ponto_v2.html:305–310`; `app/static/pwa.js:431`.) |
| “A data de sincronização decide dia/competência.” | Uma batida local de 20/11 enviada em 21/11 atravessa o corte padrão; meia-noite UTC também não equivale à local. Hoje o instante efetivo e a janela do cadastro são autoridades separadas. (`app/api/hora_do_aparelho.py:141–153`; `app/ponto/janelas.py:10–63`; `app/ponto/janelas.py:84–88`; `app/ponto/signals.py:111–112`.) |
| “Saída depois da meia-noite pertence ao novo turno.” | A API calcula sequência pela jornada; o chokepoint reconhece marco próprio também na véspera. Há caso de teste em que uma entrada retida por 24 horas não transforma a nova entrada em eco. (`app/api/views.py:578–585`; `app/ponto/registro_batida.py:35–49`; `app/ponto/tests/test_eco_flush_mesmo_turno.py:4–10`, `app/ponto/tests/test_eco_flush_mesmo_turno.py:28–45`.) |
| “O vínculo atual resolve a batida antiga.” | A geradora histórica da célula prevalece dentro da vigência; ausente na alimentação ou fora da vigência, a juíza devolve `None`. Além disso, a API consulta substituição por `agora`, embora use o dia retroativo para os marcos. (`app/escala/alimentacao.py:269–286`; `app/api/views.py:587–595`; `app/api/views.py:615–625`.) |
| “Célula sem ata é dia livre.” | Com `trabalha=None` e ata vazia, `dia_sem_trabalho()` não declara folga; ausência parcial também não dispensa automaticamente o restante. (`app/chamados/juizes.py:398–426`.) |
| “Hora do aparelho sempre é preservada como hora efetiva.” | Hoje atraso de 0 até 48 horas, inclusive, usa a hora do aparelho; futuro ou atraso maior usa `agora`, conservando a hora lida. A exceção normativa do novo aval para essas bordas é [nao verificado]. (`app/api/hora_do_aparelho.py:141–153`; `avais.txt (anexo):7`.) |
| “Guardar GPS intacto garante julgamento equivalente.” | A fila guarda accuracy, mas não

[truncado em 20000 de 27978 caracteres]

## 05/11 16:47 · sessao 54d681772cf7 · PD-OTIMIZA-3 · censo · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 05/11 16:46 · sessao c02328d36a55 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 05/11 16:45 · sessao 6a9567ba7e2d · - · - · origem=Ronald (tmux)

Censo de escritores atuais, sem alterar arquivos:

| Campo/porta | Escritores e chamadores atuais |
|---|---|
| Tripla `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo`, `app/chamados/models.py:2545`; reversão em lote, `app/chamados/models.py:2652`. |
| `carimbar` | Aplica estado em memória, consulta `pergunta_viva`, desfaz se viva/exceção, salva se morta; opcionalmente escreve resposta, validação e lavra. `app/chamados/models.py:2572`, `app/chamados/models.py:2585`, `app/chamados/models.py:2595`, `app/chamados/models.py:2605`. |
| Chamadores não identidade | `encerrar_fantasma`: `app/chamados/models.py:2674`; `lavrar_sem_turno_aberto`: `app/chamados/reconciliador.py:405`; absolvição pela célula: `app/chamados/reconciliador.py:620`; resolução de chamado: `app/chamados/services/disputa_emissao.py:1603`, `app/chamados/services/disputa_emissao.py:1675`; ausência declarada: `app/chamados/services/materializacao.py:546`; materialização unitária: `app/chamados/services/materializacao.py:1199`; pendentes aprovadas: `app/chamados/services/materializacao.py:1346`. |
| `carimbar_identidade` | Grava sem C6; valida via, pode conservar instante/autor e atualizar recusa. `app/chamados/models.py:2623`, `app/chamados/models.py:2625`, `app/chamados/models.py:2630`. |
| Chamadores identidade | Admin: `app/chamados/models.py:2511`; duplicata: `app/chamados/services/fio.py:891`; paridade: `app/chamados/services/materializacao.py:891`; colisão: `app/chamados/services/materializacao.py:1066`. Cadastro das quatro vias: `app/chamados/models.py:2275`; selo exato chamador/via: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:373`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:398`. |
| Escritores só da via | `encerrar_por_fato_no_espelho`: `app/chamados/models.py:2662`; `relavrar`: `app/chamados/services/relavra_via.py:123`. |
| `respondida_em`, `resposta_colab` fora `carimbar` | Resposta humana: `app/chamados/services/disputa_emissao.py:1282`; reabertura: `app/chamados/services/disputa_emissao.py:1393`; resposta automática: `app/chamados/services/disputa_emissao.py:1428`; recuperação somente do instante: `app/chamados/services/disputa_emissao.py:1445`; ressuscitação zera instante: `app/chamados/models.py:1493`. |
| `validada_em`, `validada_por` fora `carimbar` | `_carimbar_validacao`: `app/chamados/services/validacao.py:131`; devolução: `app/chamados/services/validacao.py:187`; resposta nova desfaz: `app/chamados/services/disputa_emissao.py:1295`; reabertura: `app/chamados/services/disputa_emissao.py:1393`; ressuscitação: `app/chamados/models.py:1493`. |
| Lavra opcional | `veredito_lavrado`, `julgado_em`, `janela_expira_em`: `app/chamados/veredito_lavrado.py:227`; reversão em memória: `app/chamados/veredito_lavrado.py:240`. |
| `recusa_motivo` | Identidade: `app/chamados/models.py:2631`; pendência de plantio: `app/chamados/services/materializacao.py:817`; recusa: `app/chamados/services/materializacao.py:1069`; escape respondido: `app/chamados/services/disputa_emissao.py:1340`; recusa humana: `app/chamados/services/validacao.py:545`. |

Reversões e chamadores:

- `descarimbar`: cópia para julgamento em `app/chamados/models.py:1444`; fantasma em `app/chamados/models.py:2685`; resposta nova em `app/chamados/models.py:2702`; competência em `app/chamados/models.py:2709`; desfazer sem lastro em `app/chamados/services/relavra_via.py:231`.
- `descarimbar_em_lote`: ressuscitação em `app/chamados/models.py:1493`; correção em `app/chamados/services/disputa_emissao.py:1412`.
- `ressuscitar_perguntas` chamado por reabrir (`app/chamados/models.py:1391`), desarquivar (`app/chamados/views_arquivar.py:86`) e cartório (`app/ponto/services/cartorio.py:1622`).
- Reabertura de fantasma chamada em `app/chamados/management/commands/reabrir_orfas_enterradas.py:147`; competência em `app/chamados/services/competencia_trancada.py:254`; resposta nova em `app/chamados/services/disputa_emissao.py:1300`.

Leitores externos relevantes:

- Autor/procedência: `validada_por or materializada_por`, instante `validada_em or materializada_em`, em `app/ponto/services/procedencia_batida.py:69`; frase apresentada em `app/ponto/services/procedencia_batida.py:87`.
- API: resposta e instante em `app/api/views.py:1173`, `app/api/views.py:2580`, `app/api/views_mensageria.py:567`; estado “já respondida” em `app/api/views.py:2636`.
- Outros consumidores de resposta: `app/folha/previa.py:142`, `app/colaboradores/services/ficha.py:171`, `app/colaboradores/services/calendario.py:474`, `app/ponto/services/cartorio.py:1590`, `app/ponto/services/dia_do_colab.py:69`; instante em `app/ponto/supra_juiz.py:545`.
- Tela: `app/templates/chamados/partials/modal_fio.html:212`, `app/templates/chamados/partials/_card_atendimento.html:94`.
- Entrada externa que delega escrita: foto chama resposta automática e validação em `app/api/foto_recebida.py:48`.
- Auditoria/reversão lê autor/carimbo: `app/chamados/reconciliador.py:1111`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:130`, `app/chamados/services/passivo_validado.py:64`.

Sítios irmãos observáveis:

- `ressuscitar_perguntas` julga cópias, coleta IDs e escreve depois em lote; a sequência está em `app/chamados/models.py:1431`, `app/chamados/models.py:1447`, `app/chamados/models.py:1488`. Resultado efetivo de corrida nesse caminho: [nao verificado].
- `carimbar_identidade` escreve e salva diretamente, sem o mecanismo de restauração usado por `carimbar`: `app/chamados/models.py:2625`, `app/chamados/models.py:2633`, `app/chamados/models.py:2599`. Resultado de concorrência: [nao verificado].
- `encerrar_por_fato_no_espelho` confia no julgamento do chamador e altera somente a via: `app/chamados/models.py:2655`.
- Contraponto já existente: desfazer sem lastro relê a linha e disputa travadas dentro de transação antes de julgar/escrever, em `app/chamados/services/relavra_via.py:217`; o comentário registra sobrescrita concorrente de decisão humana como motivação em `app/chamados/services/relavra_via.py:214`.

## 05/11 16:45 · sessao 43ffc42e6b07 · - · - · origem=Ronald (tmux)

**Guarda e chamadores atuais.** `pode_executar_csf` está em `app/ponto/supra_juiz.py:539`; percorre `disputa_origem → perguntas` e `perguntas_que_resolvem`, recusando ao primeiro `respondida_em != None`, sem consultar validação, materialização, texto ou `FollowupChamado` (`app/ponto/supra_juiz.py:543`, `app/ponto/supra_juiz.py:547`). Única chamada de produção encontrada: executor supra (`app/ponto/management/commands/supra_juiz.py:261`); testes da guarda em `app/ponto/tests/test_supra_juiz_executor.py:40`, `:43`, `:46`, `:49`.

`classificar_dia` está agora em `app/ponto/supra_juiz.py:204` — a linha antiga `:117` do anexo não é o endereço da definição nesta cópia (`app/ponto/supra_juiz.py:204`). Chamadas de produção: cartório ramo nunca bateu (`app/ponto/services/cartorio.py:1226`), cartório normal (`app/ponto/services/cartorio.py:1413`), supra (`app/ponto/management/commands/supra_juiz.py:223`), retroativo (`app/ponto/management/commands/emitir_furo_retroativo.py:101`). O classificador distingue chamado vivo de cobrança com fio vivo (`app/ponto/supra_juiz.py:213`); ramo furo lê `cobrancas_vivas`, não `chamados_vivos` (`app/ponto/supra_juiz.py:315`).

**Escritores e leitores dos insumos da CSF.**

| Campo/relação | Escritores | Leitores fora CSF |
|---|---|---|
| `PerguntaDisputa.respondida_em` | Resposta do colab grava texto e instante (`app/chamados/services/disputa_emissao.py:1282`); `responder_automaticamente` (`:1428`); carimbo pela trilha (`:1445`); `reabrir_para_correcao` limpa texto/carimbos (`:1393`, `:1413`); `PerguntaDisputa.carimbar` preenche se há texto e instante ausente (`app/chamados/models.py:2576`); restauração em memória em recusa (`:2609`); ressuscitação limpa em lote (`:1493`). Os dois arquivos escritores são também os declarados no contrato AST (`app/core/tests/test_contract_escritores_estado.py:163`). | C6 pergunta viva exige carimbo **e** texto, depois de validação/tranca (`app/chamados/juizes.py:1078`, `:1092`); corrigíveis (`:1334`); fila respondidas a validar (`:1341`); irmãs sem consumo (`:1691`); fila materializador (`app/chamados/services/materializacao.py:1292`); fio completude resposta (`app/chamados/services/fio.py:336`, `:656`); lavra calcula máximo de respostas (`app/chamados/veredito_lavrado.py:167`); modelo status da pergunta (`app/chamados/models.py:2492`) e propriedade respondida (`:2807`); verbos/contadores (`app/chamados/services/verbos.py:311`). |
| `DisputaSupervisao.chamado` / reversa `disputa_origem` | FK declarada em `app/chamados/models.py:1668`; nascimentos em `app/chamados/services/disputa_emissao.py:523`, `:962`, `:1182`. | CSF (`app/ponto/supra_juiz.py:543`), `_disputa_do_chamado` (`app/chamados/services/fio.py:131`), retenção de irmãs (`app/chamados/juizes.py:1693`), perguntas ancoradas no fecho (`app/chamados/models.py:328`), entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:56`). |
| `PerguntaDisputa.chamado_destino` / reversa `perguntas_que_resolvem` | FK em `app/chamados/models.py:2772`; nascimento/reuso por `PerguntaDisputa.abrir` (`:2387`, `:2439`, `:2466`); reapontamento pela porta `fio.reapontar_resolvedora` (`app/chamados/services/fio.py:784`, `:845`). Dois arquivos declarados no AST (`app/core/tests/test_contract_escritores_estado.py:61`). | CSF (`app/ponto/supra_juiz.py:547`), fio por destino (`app/chamados/services/fio.py:135`), gate fecho (`app/chamados/models.py:328`), retenção irmãs (`app/chamados/juizes.py:1692`), emissão/reuso (`app/chamados/services/disputa_emissao.py:909`, `:929`), competência trancada (`app/chamados/services/competencia_trancada.py:140`), materialização fecha destino (`app/chamados/services/materializacao.py:561`). Chamadores de reapontamento: modelo fecho/dedup (`app/chamados/models.py:420`), reconciliador (`app/chamados/reconciliador.py:922`, `:1570`, `:1582`), declaração (`app/chamados/services/declaracao_texto.py:320`), limbo (`app/chamados/services/limbo_folgas.py:242`), migração resolvedoras (`app/chamados/services/fio.py:947`). |
| `FollowupChamado` (`autor_tipo`, conteúdo, criação, leitura) | Modelo (`app/chamados/models.py:1587`); porta criação `nota.anexar_ocorrencia` (`app/chamados/services/nota.py:75`, `:103`, `:107`), leitura marcada por `marcar_lidas` (`:126`, `:132`), apagar ensaio (`:136`, `:141`). | Detalhe/fio lê ordenado (`app/chamados/views.py:352`, `:372`), fio prefetch (`app/chamados/services/fio.py:172`), contexto detalhe (`app/chamados/services/acoes_chamado.py:402`), contagem competência (`app/chamados/views.py:903`), verbos cobrança (`app/chamados/services/verbos.py:266`). A CSF não lê essa entidade (`app/ponto/supra_juiz.py:539`). |

A tabela de leitores acima cobre leitores nomeados e verificados, **não prova exaustão de cada leitura de `respondida_em` no repositório** [não verificado].

**Inventário de pedidos de fecho fora supra/cartório, agrupado por porta; ausência de CSF não demonstra perda da conversa, pois há guardas nas portas (`app/chamados/reconciliador.py:518`, `app/chamados/models.py:651`).**

| Porta | Pedidos de fecho |
|---|---|
| `reconciliar()` | Disputa→pai (`app/chamados/signals.py:42`); auditoria órfãos (`app/chamados/management/commands/auditar_chamados_orfaos.py:67`); pós vínculo (`app/colaboradores/services/pos_vinculo.py:42`); feriado posto (`app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`); adesão (`app/chamados/services/adesao.py:115`, `:150`); materialização (`app/chamados/services/materializacao.py:561`, `:738`); silêncio ausência (`app/chamados/services/silencio_ausencia.py:101`); feriado (`app/chamados/services/feriado_reavaliar.py:47`); limbo (`app/chamados/services/limbo_folgas.py:101`, `:243`, `:246`); triagem (`app/ponto/services/triagem_batida.py:597`); decisão célula (`app/ponto/services/veredito_celula.py:183`); intervalo (`app/ponto/management/commands/detectar_intervalo_ausente.py:138`); ausência (`app/ponto/services/ausencia.py:678`); detector ausência (`app/ponto/management/commands/detectar_ausencias.py:158`, `:821`, `:831`, `:842`); cura BUG115 (`app/ponto/management/commands/curar_residuo_bug115.py:151`); lastro (`app/ponto/services/lastro.py:345`); cobertura (`app/ponto/services/cobertura.py:122`, `:317`); cluster espúrio (`app/ponto/services/cluster_espurio.py:98`); entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:31`). |
| `retratar()` → `_retratar()` → `reconciliar()` | Unidade (`app/chamados/reconciliador.py:503`, `:523`, `:544`); reconciliador célula/premissa (`:972`, `:1395`); lei cala (`app/chamados/management/commands/retratar_chamados_lei_cala.py:59`); geofence (`app/ponto/services/geofence.py:419`); justificativa (`app/ponto/services/justificativa.py:42`); grade (`app/ponto/management/commands/reconciliar_grade.py:309`); alertas turno (`app/ponto/management/commands/processar_alertas_turno.py:216`); vínculo (`app/ponto/management/commands/reconciliar_vinculo.py:68`); fantasmas (`app/ponto/management/commands/reconciliar_fantasmas.py:279`). |
| `encerrar()` | Unidade com retenção (`app/chamados/reconciliador.py:496`), chamada por justificativa/bloqueio (`app/ponto/services/justificativa.py:79`, `:385`); regularização externa (`app/chamados/services/regularizacao_ext.py:205`); troca vínculo (`app/colaboradores/services/vinculo.py:387`); desligamento (`app/colaboradores/services/desligamento.py:155`). |
| `encerrar_por_competencia_trancada()` | `app/chamados/services/competencia_trancada.py:184`. |
| `cancelar()` | Revisão vínculo (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`); avisos régua (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`). |
| `transicionar()` / `ChamadoColaborador.fechar()` | `fechar` delega (`app/chamados/models.py:437`); meio período (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`); ações humanas (`app/chamados/services/acoes_chamado.py:70`, `:108`, `:149`, `:222`, `:277`, `:315`); fio resposta rápida (`app/chamados/services/fio.py:717`); benefício (`app/colaboradores/views.py:2068`); instrumentação smoke (`app/chamados/management/commands/smoke_chamados.py:134`, `:147`, `:163`). |

**Pedidos indiretos via `DisputaSupervisao.fechar()`, que chegam a `signals.py:42`:** foto recebida (`app/api/foto_recebida.py:80`), encerrar perguntas com fato (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`), modelo admin/remover último motivo (`app/chamados/models.py:1868`, `:1975`), reconciliador (`app/chamados/reconciliador.py:412`, `:636`, `:1139`), materialização (`app/chamados/services/materializacao.py:565`, `:1255`), becos (`app/chamados/services/becos.py:52`), competência (`app/chamados/services/competencia_trancada.py:204`), emissão/saída (`app/chamados/services/disputa_emissao.py:1118`, `:1696`), desligamento (`app/colaboradores/services/desligamento.py:142`), entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:59`).

**Contraexemplos úteis ao censo e casos RED.**

- Pergunta validada ou materializada mas com `respondida_em` continua proibindo CSF; “pergunta morta pelo C6” não equivale a “sem conversa” (`app/ponto/supra_juiz.py:545`, `app/chamados/juizes.py:1078`). Caso RED: cadastro com cobrança indevida, pergunta respondida e depois validada/materializada; ato fecho automático CSF; lei literal conserva conversa, pois a guarda já responde False (`app/ponto/tests/test_supra_juiz_executor.py:43`, `app/docs/BACKLOG.md:1379`).
- Mensagem livre apenas em followup não muda CSF; a guarda só percorre perguntas respondidas (`app/chamados/services/nota.py:103`, `app/ponto/supra_juiz.py:543`). Regra do aval cobre “conversa”, mas extensão a todo followup livre não está definida além da guarda nomeada [não verificado].
- Fecho humano por resposta livre aceita `conteudo=''` e `novo_status` válido: a nota depende de conteúdo, a transição não (`app/chamados/services/acoes_chamado.py:289`, `:313`). Resposta rápida do fio exige conteúdo e grava `mensagem_dp` antes de transicionar; portanto “sem CSF” não implica “sem fala do DP” (`app/chamados/services/fio.py:705`, `:714`, `:717`).
- Sem lavra não é “sem furo”: supra conta `CSF_SEM_LAVRA`, sem retratar (`app/ponto/management/commands/supra_juiz.py:245`); sem código da lavra conta `CSF_LAVRA_SEM_CODIGO` (`:250`).
- DNA refeito depois do nascimento não prova cobrança originalmente falsa: supra segura e conta `COBRANCA_ORFA_REGENERACAO` (`app/ponto/management/commands/supra_juiz.py:264`, `:275`).
- FIO-DUP do cartório tem guarda própria por texto, mesmo sem CSF (`app/ponto/services/cartorio.py:1586`, `:1597`, `:1614`).
- Concorrência já possui outra guarda: costura lê linha destino fresca, usa `select_for_update` se estiver em atomic e preserva trava até save (`app/chamados/services/fio.py:752`, `:761`, `:837`); CSF em si não possui atomic/trava (`app/ponto/supra_juiz.py:539`). Corrida específica resposta × CSF e seu dano em produção: [não verificado].
- O executor mantém `--executar` (`app/ponto/management/commands/supra_juiz.py:62`) e `T6_1_RETRATACAO_EM_SECO=False` (`:45`); só incrementa `CSF_RETRATADO` se a porta retorna True (`:286`, `:288`), depois imprime o contador (`:299`). Valor atual/histórico zero não demonstrado [não verificado].

## 05/11 16:45 · sessao 121f53e2c421 · - · - · origem=Ronald (tmux)

Evidências de leitura e contraexemplos, somente código/documentação lidos; nenhum teste executado [nao verificado]. [nao verificado]

**Leitores centrais**

- `pergunta_viva` memoriza por instância; a chave lê `validada_em`, `respondida_em`, `resposta_colab`, `via_resolucao`, `materializada_em`, `data_evento`, `tipo_resposta`, `celula_id`, veredito/via da célula e destino/status; não inclui `materializada_por` (`app/chamados/juizes.py:809`, `app/chamados/juizes.py:820`).
- `_pergunta_viva_calc` decide nesta precedência: validação; tranca atual/histórica; resposta por validar salvo decisão administrativa/concordância com luz; identidade; marco provado; célula; falta pela lei dentro da janela; ausência de carimbo; testemunha externa (`app/chamados/juizes.py:1077`).
- `estado` lê validação/carimbo e C6, mas pode retornar PENDENTE/RESPONDIDA mesmo com C6 morto sem carimbo; `.viva` delega diretamente a C6 (`app/chamados/models.py:2481`, `app/chamados/models.py:2752`).
- `q_vivas` é prefiltro generoso, contrato de continência, explicitamente sem igualdade com C6; lê validação, resposta, identidade/tranca, célula/veredito/ata (`app/chamados/models.py:2713`, `app/chamados/models.py:2734`).
- App: `perguntas_do_app` aplica `q_vivas` e depois C6; o destinatário é o colaborador da disputa (`app/chamados/services/pergunta_no_app.py:42`).
- Cobrança coletiva: `fio_vivo` é `any(pergunta_viva(p))`, universo fornecido pelo chamador (`app/chamados/juizes.py:1175`).
- Tela de resposta barra apenas materializada E não viva, preservando a resposta a carimbo contradito por C6 (`app/chamados/views.py:1257`).
- `raiox.pergunta_sem_acao_possivel`: sem carimbo retorna falso; com lavra lê `VL_SIM`; sem lavra chama juiz do lastro; exceção mantém na mesa (`app/chamados/services/raiox.py:173`).
- Fábrica já consulta célula: carimbo existente exclui fábrica apenas se célula ausente ou veredito fora de `VEREDITOS_ACUSAM`; portanto o texto histórico da O261 sobre leitor que nunca consulta célula não descreve mais este corpo (`app/chamados/services/disputa_emissao.py:695`).
- Materialização usa presença de carimbo como elegibilidade/antirreplantio; há guarda `if p.materializada_em`, seleção de irmãs sem carimbo e fila de pendentes sem carimbo (`app/chamados/services/materializacao.py:433`, `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:1294`).
- Validação relê carimbo/validação; não roda unidade de plantio se já carimbada; relê ambos após materializar (`app/chamados/services/validacao.py:180`, `app/chamados/services/validacao.py:422`, `app/chamados/services/validacao.py:433`).
- Reconciliação lê carimbo para impedir repetição e selecionar pendentes (`app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:583`, `app/chamados/reconciliador.py:1017`, `app/chamados/reconciliador.py:1433`).
- Outros leitores fora de `chamados`: procedência exibe autor (`validada_por or materializada_por`) e instante; reconciliador de fantasmas seleciona sem carimbo; cartório reconhece coleção toda duplicata (`app/ponto/services/procedencia_batida.py:71`, `app/ponto/management/commands/reconciliar_fantasmas.py:212`, `app/ponto/services/cartorio.py:1599`).
- Histórico transforma `materializada_em` em evento; painel agrega máximo desse instante; template traduz `via_resolucao` em frase (`app/chamados/services/historico.py:103`, `app/chamados/services/painel.py:67`, `app/templates/chamados/partials/_card_atendimento.html:186`).
- Lavra calcula staleness por máximo de materialização por colaborador (`app/chamados/veredito_lavrado.py:163`).
- Passivo validado guarda carimbo/autor/via em reversão, lê via após recusa e agrupa reparo por competência (`app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:140`, `app/chamados/services/passivo_validado.py:289`).
- Auditorias também leem: sem carimbo (`app/chamados/management/commands/auditar_disputas.py:71`); I6 carimbo/testemunha (`app/chamados/management/commands/auditar_invariantes_chamados.py:538`); I8b carimbada e viva (`app/chamados/management/commands/auditar_invariantes_chamados.py:905`); sem via (`app/chamados/management/commands/auditar_invariantes_chamados.py:954`); validada sem materializada (`app/chamados/management/commands/auditar_invariantes_chamados.py:1073`).

**Contraexemplos e limites**

- Concorrência é o “item 3 metade-trava” ainda parado no RELATO; a metade memória já está curada. O documento registra duas candidatas e oito chamadores sem `select_for_update` (`app/docs/RELATO.md:66`). `carimbar` fotografa a instância recebida, pergunta C6 nela e faz `save(update_fields=...)`; não relê linha nem trava no corpo (`app/chamados/models.py:2572`, `app/chamados/models.py:2585`, `app/chamados/models.py:2598`).
- A porta irmã **já protegida** `desfazer_carimbo_sem_lastro` relê pergunta e disputa travadas antes de C6; não apaga decisão humana que surgiu após coleta nem reabre disputa que fechou depois dela (`app/chamados/services/relavra_via.py:214`, `app/chamados/services/relavra_via.py:221`). REDs desses dois entrelaçamentos: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:148`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:161`.
- “Carimbo escrito ⇒ pergunta morreu” erra: identidade pode continuar viva quando resposta por validar discorda da luz, pois degrau da resposta vem antes da identidade (`app/chamados/juizes.py:1092`, `app/chamados/juizes.py:1113`; advertência explícita `app/chamados/models.py:2614`).
- “Célula concorde ⇒ pode carimbar” erra com marco pendente; célula provisória não absolve pergunta sem prova do marco. RED de saída 14:20 ainda pendente com relógio congelado 21/10 14:45:08 (`app/chamados/juizes.py:1127`, `app/chamados/tests/test_o261_pergunta_viva_teto.py:27`, `app/chamados/tests/test_o261_pergunta_viva_teto.py:52`).
- **Célula sem ata:** juiz de marco devolve `None` sem prova; mas célula com veredito não acusador e ata vazia atualmente absolve, pois não há marcos pendentes. O teste explicitamente espera morto em SQL e C6 com ata `{}` (`app/chamados/juizes.py:873`, `app/chamados/juizes.py:1137`, `app/chamados/tests/test_o261_pergunta_viva_teto.py:80`). Expectativa específica do aval para esse cadastro faltante: [nao verificado].
- “Carimbo de via automática sempre ressuscita se falta” erra fora da janela: `_pergunta_materializada_de_fato` aceita via sem testemunha fora dela (`app/chamados/juizes.py:645`, `app/chamados/juizes.py:658`). Janela calculada com `date.today`, anterior+atual e véspera, chamando `janela_atual/anterior` **sem empresa** (`app/chamados/juizes.py:554`). Empresa efetiva diversa nesse caminho: [nao verificado].
- Competência não é mês civil: corte 21 contém 21 anterior→20 corrente; `dia_da_pergunta_trancado` usa empresa e competência da `data_evento`, prevalece sobre resposta e é reversível por toque humano posterior (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/chamados/juizes.py:933`, `app/chamados/juizes.py:946`, `app/chamados/juizes.py:1080`).
- Overnight/vínculo trocado: data derivada pela escala atual pode divergir da data do plantio histórico; código preserva prova por batida ligada, compara H:M no caso `hora` e instante inteiro em `data_hora` (`app/chamados/juizes.py:692`, `app/chamados/juizes.py:707`, `app/chamados/juizes.py:712`, `app/chamados/juizes.py:716`). `_celulas_da_pergunta` rejeita célula de outro dia/colaborador, não declara ausência (`app/chamados/juizes.py:619`).
- “Vizinha do mesmo tipo prova o marco” erra: recusa por paridade exige `fato_do_marco_no_espelho(p) is True`; desconhecido não conta como prova (`app/chamados/services/materializacao.py:856`, `app/chamados/services/materializacao.py:867`).
- Erro após C6 não deixa carimbo/lavra em memória: já tratado na cópia e coberto por REDs save falho, lavra falha, save após lavra (`app/chamados/models.py:2593`, `app/chamados/tests/test_o261e_f2_fila.py:179`, `app/chamados/tests/test_o261e_f2_fila.py:190`, `app/chamados/tests/test_o261e_f2_fila.py:200`).

**Juízes registrados**

- C1 é hoje só “chamado vivo”; C6 pergunta viva; C15 coleção viva; C17 porta alcançável. Registro separa explicitamente status de cobrança (`app/core/juizes.py:100`, `app/core/juizes.py:107`, `app/core/juizes.py:112`, `app/core/juizes.py:125`, `app/core/juizes.py:135`).
- A própria `carimbar` documenta que C6 já é o juiz e aplica candidato antes de perguntar; identidade julga pelo chamador (`app/chamados/models.py:2525`, `app/chamados/models.py:2532`, `app/chamados/models.py:2614`).

**REDs adicionais existentes**

- Fábrica pode ter célula sem acusação mas resposta ainda por validar: fechar não deve carimbar; vigia não conta dia regularizado mas I20 conta pergunta viva em disputa fechada (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:199`, `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:257`).
- Após validar resposta recusada por paridade, disputa não pode fechar por validação transitória; teste cobre segunda tentativa e intercalações (`app/chamados/tests/test_pvmuda_carimbo_em_voo.py:80`, `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:95`, `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:178`).
- Fantasma não carimba viva; pendente com resposta por validar não sai pelo fecho; ressuscitar limpa autor junto; recusa restaura memória (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:663`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:685`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:696`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:711`).

## 05/11 16:45 · sessao 915f35b204d8 · - · - · origem=Ronald (tmux)

Censo de abertura, reabertura e concorrência, somente pelo código:

- **Campos do chamado:** `colaborador`, categoria/subcategoria, `modulo_origem`, `contexto_json`, `celula`, `status_local`, carimbos de resolução e criação estão declarados em `app/chamados/models.py:32`, `:38`, `:41`, `:44`, `:46`, `:48`, `:54`, `:61`. O detector escreve o nascimento com identificação do módulo, categoria, contexto do dia, marcos faltantes e SLA em `app/ponto/management/commands/detectar_ausencias.py:693`; a grade escreve contexto `origem=grade` e marcos em `app/ponto/management/commands/reconciliar_grade.py:507`; o C2 escreve contexto por `_ctx_c2` em `app/chamados/reconciliador.py:879`.
- **`abrir`:** roda em `atomic`, trava a linha de Colaborador antes de consultar dedup; existente terminal tenta renascer, existente vivo retorna intacto e ambos retornam `criado=False`; agrupado sob pai vivo também satisfaz dedup. (`app/chamados/models.py:863`, `:867`, `:882`, `:885`, `:888`)
- **Criação:** `abrir` resolve célula, pergunta absolvição/veto, insere e registra nascimento. Seu evento explícito só contempla `MODULOS_DE_TURNO` com célula; esse conjunto é `turno_aberto`, `turno_aberto_24h`, `entrada_bloqueada_turno_aberto`, sem `batida_ausente`. (`app/chamados/models.py:913`, `:923`, `:928`, `:932`, `:938`; `app/chamados/reconciliador.py:336`)
- **`criar`:** consulta motor, resolve célula e veto, cria e registra nascimento; não contém emissão direta de pergunta nem o bloco de outbox de `abrir`. (`app/chamados/models.py:1015`, `:1020`, `:1027`, `:1036`, `:1048`)
- **Nuance decisiva:** existe evento indireto de nascimento: `post_save` de chamado criado ou com mudança de estado chama `cartorio.julgar_celula` pelo `dia_do_fato(..., fortes=True)`, salvo quando já está em julgamento; erro fica no contexto `engolir`. Portanto “abrir não tem evento algum” seria incorreto. (`app/chamados/signals.py:164`, `:168`, `:171`, `:174`, `:178`, `:180`)
- **Perguntas pelo cartório:** somente as células julgadas nesta passada chegam a `perguntar_pela_celula`; o comentário explicita teto por instante para hoje, passado e noturno anterior. O evento explícito consumido pelo reconciliador só retrata chamados absolvidos e encerra órfãs; não chama fábrica de perguntas nesse corpo. (`app/ponto/services/cartorio.py:981`, `:985`, `:995`, `:997`; `app/chamados/reconciliador.py:1322`, `:1326`, `:1329`)
- **Grade:** `_emitir_furo` passa pelos vetos, chama `abrir`, eventualmente `reabrir` e retorna; o caminho pergunta por marco está removido após o retorno. (`app/ponto/management/commands/reconciliar_grade.py:488`, `:494`, `:501`, `:507`, `:527`, `:529`, `:531`, `:532`)
- **Detector:** abre chamado primeiro; recusa `None`/encerrado que não renasceu antes de push e fio; para LIMBO pode agrupar e retornar sem pergunta; por fim tenta construir disputa dos órfãos cobráveis em `try/except` que absorve a falha. (`app/ponto/management/commands/detectar_ausencias.py:693`, `:737`, `:739`, `:745`, `:761`, `:787`, `:795`, `:801`, `:802`)
- **Outro emissor:** C2 usa chave `batida_ausente` + `contexto_json__data_turno`; costurar em vivo passa pelos gates de nascimento; nascer/renascer exige horizonte, protesto elegível, cobertura/veto; depois abre e reaponta a resolvedora. Não costura no chamado devolvido terminal. (`app/chamados/reconciliador.py:804`, `:807`, `:809`, `:824`, `:838`, `:844`, `:869`, `:879`, `:905`, `:922`)
- **Leitores que acionam o detector fora dele:** cartório chama `_abrir_chamado` em duas portas de emissão; emissor retroativo também a chama. Vigia de hora consulta vivos de hoje/ontem e chama julgamento com `teto_de_hoje=True`. (`app/ponto/services/cartorio.py:1740`, `:1778`; `app/ponto/management/commands/emitir_furo_retroativo.py:154`; `app/ponto/services/vigia_de_hora.py:109`, `:123`, `:131`)

Reabertura e conservação do silêncio:

- `_renascer_por_premissa_viva` recusa colaborador não ativo, cascata, competência trancada e veto; `reabrir` recarrega estado do banco, pergunta `recusa_da_reabertura`, migra resolvedoras para irmão quando indicado, grava estado e ressuscita perguntas. (`app/chamados/models.py:965`, `:970`, `:976`, `:1322`, `:1340`, `:1361`, `:1382`, `:1391`)
- Ressuscitação só considera resolvedoras materializadas sem texto; pergunta ao marco e ao C6 sobre uma cópia descarimbada; pode devolver zero; quando escreve, descarimba em lote e também reabre disputa. Não fabrica pergunta inexistente. (`app/chamados/models.py:1423`, `:1441`, `:1444`, `:1447`, `:1481`, `:1487`, `:1493`, `:1494`)
- Cartório pula chamados sem pergunta histórica, com fio vivo ou com resposta textual; trata duplicatas por reconciliação e demais candidatas pela porta de ressuscitação. Portanto a ressuscitação não cobre ausência absoluta de pergunta. (`app/ponto/services/cartorio.py:1591`, `:1594`, `:1595`, `:1597`, `:1599`, `:1622`)
- `_cala_emissao` usa existência de chamado vivo, separada de existência de fio, e também piso de emissão; o código documenta que estreitar `vivos` para fio apenas voltaria ao mesmo dedup e redispararia push. (`app/ponto/services/cartorio.py:1676`, `:1677`, `:1684`)
- Contexto pós-nascimento tem portas `anotar`, `renovar`, `retirar`, `fundir`; elas salvam `contexto_json` + `atualizado_em`, sem lock de leitura, e não reconciliam. (`app/chamados/services/contexto.py:7`, `:25`, `:35`, `:47`, `:52`, `:62`, `:81`, `:106`, `:128`)

Contraexemplos e RED úteis:

- **Concorrência:** cadastro de mesma pessoa e mesma chave, duas transações abrindo simultaneamente → contrato existente exige um chamado, um `criado=True`, um `False`; o gate atual serializa a leitura/criação pela pessoa. O selo existente afirma esses resultados, mas não foi executado nesta leitura. (`app/chamados/models.py:863`; `app/chamados/tests/test_c2_corrida_do_abrir.py:127`, `:140`, `:147`, `:149`)
- **Meia-noite:** entrada 22h + saída de intervalo 00h30, sem saída final → intervalo não prova turno resolvido; o código usa `turnos_do_colab`, ignora turno aberto, exige entrada/saída e duração ≤26h antes de concluir fechamento na virada. (`app/chamados/services/disputa_emissao.py:638`, `:643`, `:647`, `:649`, `:653`)
- **Sem ata:** `_ata_do_dia` com estado `sem_celula` e escala vigente abstém e sinaliza; sem escala vigente retorna permitido para a classificação por batidas; falha de leitura também retorna permitido. Assim “qualquer sem ata é sempre veto” erra. (`app/chamados/services/disputa_emissao.py:818`, `:828`, `:832`, `:833`, `:839`, `:842`)
- **Parcial na lista:** fábrica recebe chamado vivo, mas `dia_na_lista_do_admin` aplica `parcial_so_lista` com ata e julgamento anterior; filtra antes das passadas e retorna sem emitir pergunta, conservando chamado vivo. “Vivo sem pergunta é sempre defeito” erra aqui. (`app/chamados/services/disputa_emissao.py:847`, `:855`, `:868`, `:871`, `:892`, `:894`, `:896`)
- **Marco aceso e outro marco apagado:** ressuscitar todas as materializadas sem texto erra; a porta pula marco comprovadamente aceso e consulta C6 após descarimbo para os demais. (`app/chamados/models.py:1441`, `:1444`, `:1447`; exemplo documentado em `:1458`)
- **Silêncio de exibição:** `silenciado_ate` é véu, conserva estado vivo e leitura pelo emissor/juiz/folha; portanto retirar silêncio pode apenas revelar cobrança que já era viva. (`app/chamados/models.py:81`)
- **Existente terminal recusado:** `abrir` retorna `False` tanto para renascido quanto recusado, exigindo leitura de `status_local` pelo chamador; detector já distingue, enquanto grade retorna `None` depois da tentativa. (`app/chamados/models.py:882`, `:885`; `app/ponto/management/commands/detectar_ausencias.py:739`; `app/ponto/management/commands/reconciliar_grade.py:529`, `:531`)

Irmãos concretos:

- Fábrica de órfãos usa consulta `materializada_em IS NULL` para complemento/filtro, enquanto o leitor canônico de vitalidade está em C6: são filtros concretos a examinar como irmãos, sem afirmar incidência produtiva. (`app/chamados/services/disputa_emissao.py:909`, `:928`)
- Falhas best-effort conservam a escrita anterior também nos signals que resolvem perguntas ao encerrar chamado e materializam ao fechar disputa; ambos capturam exceção. (`app/chamados/signals.py:75`, `:88`, `:93`, `:124`, `:139`, `:141`)
- `criar` é irmã da abertura quanto à ausência de emissão direta/outbox próprio; `reabrir` é irmã quanto à possibilidade de retorno vivo sem pergunta fabricada; grade é irmã quanto a nascer somente chamado. (`app/chamados/models.py:1048`, `:1382`, `:1391`; `app/ponto/management/commands/reconciliar_grade.py:507`, `:531`)

## 05/11 16:44 · sessao 4d9193f4d03d · - · - · origem=Ronald (tmux)

C11 e autoridades existentes:

- `_C11` registrado pergunta se a ausência suspende, fecha ou levanta a cobrança viva e aponta para `reconciliar_por_ausencia`; o rótulo operacional “FIO-MUDO = C11 antes” aparece em outra referência, sem equivalência técnica demonstrada. (`app/core/juizes.py:117`, `app/core/juizes.py:271`, `app/docs/PROMPTS.md:1519`; equivalência: [não verificado])
- C6 responde pela vida individual da pergunta, C15 pela existência de alguma viva na coleção e C17 pela porta alcançável do dia; são perguntas distintas com autoridades distintas. (`app/core/juizes.py:112`, `app/core/juizes.py:125`, `app/core/juizes.py:135`, `app/core/juizes.py:266`, `app/core/juizes.py:275`, `app/core/juizes.py:277`)
- `fio_vivo` reduz exclusivamente as perguntas recebidas com `any(pergunta_viva(...))`; coleção vazia devolve falso, e o universo pertence ao chamador. (`app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1184`)
- `porta_alcancavel_do_dia` aceita chamado da célula na fila **ou** pergunta viva no app com âncora na fila, sem resposta vazia; não inclui o acesso manual por URL nem o botão Resolver dia. (`app/chamados/juizes.py:1283`, `app/chamados/juizes.py:1286`, `app/chamados/juizes.py:1309`, `app/chamados/juizes.py:1322`)
- O juiz registrado C11, `reconciliar_por_ausencia`, mantém ausência aberta como suspensão; aprovada só fecha se a premissa morrer e a célula deixar de acusar; rejeitada/apagada levanta seu marcador de silêncio. (`app/chamados/services/silencio_ausencia.py:1`, `app/chamados/services/silencio_ausencia.py:28`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/silencio_ausencia.py:112`)

Leitores fora dos sítios iniciais:

- `fio_vivo`: `DisputaSupervisao.pode_fechar`, encerramento de disputa, emissor de disputa, limbo de folgas, encerramento de perguntas com fato, prévia da folha, reconciliação de grade, supra e emissor retroativo. (`app/chamados/models.py:1783`, `app/chamados/juizes.py:2026`, `app/chamados/services/disputa_emissao.py:1244`, `app/chamados/services/limbo_folgas.py:201`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:83`, `app/folha/previa.py:144`, `app/ponto/management/commands/reconciliar_grade.py:460`, `app/ponto/management/commands/supra_juiz.py:215`, `app/ponto/management/commands/emitir_furo_retroativo.py:98`)
- A redução `vigia_fio.fio_do_dia` só delega ao juiz; seus leitores internos incluem as contagens do vocabulário externo e do universo de furo. (`app/chamados/services/vigia_fio.py:156`, `app/chamados/services/vigia_fio.py:165`, `app/chamados/services/vigia_fio.py:413`, `app/chamados/services/vigia_fio.py:416`)
- `porta_alcancavel_do_dia` é lido pela folha e por `censo_alcance`; o comando `vigiar_fio_morto` chama `censo_alcance`. (`app/folha/fora_da_folha.py:67`, `app/chamados/services/vigia_fio.py:634`, `app/chamados/management/commands/vigiar_fio_morto.py:168`)
- `reconciliar_por_ausencia` é chamado pelo evento da ausência e pela varredura que mede/aplica. (`app/chamados/services/silencio_ausencia.py:119`, `app/chamados/services/silencio_ausencia.py:127`, `app/ponto/management/commands/reavaliar_ausencias_lancadas.py:35`, `app/ponto/management/commands/reavaliar_ausencias_lancadas.py:54`)
- `dia_da_pergunta_trancado` também é lido por respostas sem veredito, emissão e reconciliação; o predicado lê `PeriodoFechado` pela competência do dia e admite toque humano posterior à tranca. (`app/chamados/models.py:1776`, `app/chamados/services/disputa_emissao.py:1589`, `app/chamados/reconciliador.py:616`, `app/chamados/reconciliador.py:716`, `app/chamados/juizes.py:933`, `app/chamados/juizes.py:939`, `app/chamados/juizes.py:949`)

Universos dos vigias:

- `censo` começa em chamados VIVOS com célula; portanto, não alcança o dia sem chamado nesse ponto de entrada. (`app/chamados/services/vigia_fio.py:333`, `app/chamados/services/vigia_fio.py:334`)
- Seus dias precisam ter veredito acusador; os módulos fora do vocabulário do cartório têm contagem separada, e hoje/futuro sem fio saem do contador alarmado. (`app/chamados/services/vigia_fio.py:338`, `app/chamados/services/vigia_fio.py:377`, `app/chamados/services/vigia_fio.py:411`, `app/chamados/services/vigia_fio.py:419`)
- `censo_alcance` parte dos dias que seguram a folha nas empresas visíveis e competências a lavrar; monta a alimentação do juiz uma vez e consulta a porta por dia. (`app/chamados/services/vigia_fio.py:565`, `app/chamados/services/vigia_fio.py:576`, `app/chamados/services/vigia_fio.py:582`, `app/chamados/services/vigia_fio.py:603`, `app/chamados/services/vigia_fio.py:634`)
- O limite de lista não limita contadores; dia sem porta entra sempre na lista, e dias com porta/bola com alguém podem ter linhas omitidas declaradamente. (`app/chamados/services/vigia_fio.py:550`, `app/chamados/services/vigia_fio.py:624`)

Contraexemplos e casos RED pela lei:

- **Competência 21→20:** cadastro com corte 21; pergunta de 21/10 pertence à competência 11, e 20/10 pertence à 10; trancar a 10 não equivale a trancar todo outubro civil. O código usa `janela_atual(data, empresa)` na tranca da pergunta; corte vem da empresa e pode diferir de 21. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/chamados/juizes.py:939`)
- **Tranca reversível por toque:** pergunta de dia em competência trancada, admin executa Cobrar este dia/Resolver dia/desarquivar depois da tranca; a leitura da tranca considera esse toque humano, mas não resposta do colaborador nem cron. (`app/chamados/juizes.py:946`, `app/chamados/juizes.py:949`, `app/chamados/juizes.py:952`)
- **Cross-meia-noite:** turno 19h→07h ainda aberto às 00h; virar a data não prova fato encerrado. A lei exige instante ou fato encerrado, e `marcos_turno` encadeia marcos avançando dia quando necessário. (`CLAUDE.md:185`, `app/escala/servico_jornada.py:61`, `app/escala/servico_jornada.py:83`)
- **Hoje fora do alarme:** célula de hoje acusa e ainda não tem pergunta; `censo` conta isso separadamente, não como fio morto alarmado. `censo_alcance` também separa hoje/futuro sem porta. Não se deve converter ausência imediata de fio em defeito consumado sem teto temporal. (`app/chamados/services/vigia_fio.py:419`, `app/chamados/services/vigia_fio.py:647`, `CLAUDE.md:185`)
- **Vínculo trocado:** vínculo antigo inativo cobre o dia e é a geradora da célula; o atual ativo não substitui essa autoridade. Geradora ausente da alimentação ou fora da própria vigência devolve `None`, sem escolher outro vínculo. (`app/escala/alimentacao.py:228`, `app/escala/alimentacao.py:232`, `app/escala/alimentacao.py:264`, `app/escala/alimentacao.py:269`)
- **Célula sem ata:** a prova do marco retorna `None`, que não vira verdadeiro; entretanto, o leitor `marcos_pendentes` devolve lista vazia sem ata, e no degrau da célula não acusadora essa lista vazia faz a pergunta morta. São respostas diferentes: não provar o marco aceso não garante vida por todos os degraus. (`app/chamados/juizes.py:873`, `app/chamados/juizes.py:876`, `app/ponto/services/lastro.py:138`, `app/ponto/services/lastro.py:148`, `app/chamados/juizes.py:1123`, `app/chamados/juizes.py:1137`)
- **Sem ata de trabalho + `trabalha=None`:** não equivale a folga; `dia_sem_trabalho` só cala por `trabalha is False` ou tipo preenchido diverso de trabalho, com exceção da ausência parcial. (`app/chamados/juizes.py:411`, `app/chamados/juizes.py:416`, `app/chamados/juizes.py:424`)
- **Dia fora da janela:** o fallback de `pergunta_viva` só chama `classificar_falta` dentro da janela; fora dela ainda podem valer os degraus de célula, ausência de carimbo e testemunha de materialização. Não há equivalência “fora da janela = morta”. (`app/chamados/juizes.py:1123`, `app/chamados/juizes.py:1139`, `app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1156`)
- **Janela versus empresa:** `_dentro_da_janela_viva` usa `date.today()` e `janela_atual/anterior` sem empresa; a tranca usa a empresa da pergunta. O alcance exato da diferença em produção é [não verificado]. (`app/chamados/juizes.py:559`, `app/chamados/juizes.py:560`, `app/chamados/juizes.py:561`, `app/chamados/juizes.py:580`, `app/chamados/juizes.py:939`)
- **Resposta viva antes da identidade:** pergunta respondida com texto, sem decisão explícita do admin, continua viva se a resposta não concordar comprovadamente com a luz, mesmo antes do degrau de duplicata/identidade. Não basta ler `via_resolucao` isoladamente. (`app/chamados/juizes.py:1092`, `app/chamados/juizes.py:1100`, `app/chamados/juizes.py:1104`, `app/chamados/juizes.py:1112`)
- **Materializada ≠ morta:** materializada por via não identitária com célula ainda acusadora pode voltar viva na leitura; já validada é morta no primeiro degrau. (`app/chamados/juizes.py:1078`, `app/chamados/juizes.py:1113`, `app/chamados/juizes.py:1125`)
- **Concorrência da porta:** a alimentação lê verbos e fila separadamente; chamado que aparece na fila mas não no mapa de verbos é pulado explicitamente como corrida entre leituras. Esse mecanismo não prova snapshot consistente da rodada. (`app/chamados/juizes.py:1229`, `app/chamados/juizes.py:1232`, `app/chamados/juizes.py:1233`; consistência transacional do conjunto: [não verificado])
- **Ausência parcial:** cadastro de ausência aprovada parcial e célula com resto acusador; aprovação não autoriza fechar por ausência, e marcador pendente é levantado quando a premissa segue viva. (`app/chamados/services/silencio_ausencia.py:28`, `app/chamados/services/silencio_ausencia.py:40`, `app/chamados/services/silencio_ausencia.py:93`)

Normas existentes:

- Evento carimba e julga; emissão/pergunta/chamado nascem do sinal da célula; varredura é vigia, consumidor lê a lâmpada. (`CLAUDE.md:166`, `CLAUDE.md:168`, `CLAUDE.md:169`, `CLAUDE.md:170`)
- Um juiz por pergunta e um escritor por entidade, com idempotência “2x = mesmo estado, 1 trilha, 0 duplicata”; censo de escritores vem antes de filtro/regra nova. (`CLAUDE.md:175`, `CLAUDE.md:193`)
- O aval operacional existente escolhe emissão no evento de abertura de `batida_ausente`; a exceção do cron permanece condicionada a medir fio mudo e pousar C11, mantendo-o contado como juiz por varredura. (`app/docs/PROMPTS.md:1853`, `app/docs/CORTES.md:305`)
- A própria cópia ainda descreve no relato a pergunta do evento como pendente, e distingue a parte A do comando que virou vigia do ramo B fio mudo. Isso é relato da cópia, não comprovação do estado de produção. (`app/docs/RELATO.md:65`, `app/docs/RELATO.md:89`)

## 05/11 16:44 · sessao 892f2f4c2ece · - · - · origem=Ronald (tmux)

Censo de fechadores e campos, somente leitura:

- `COBRANCA=('aberto','em_analise')`; `VIVOS` inclui também `registrado`; terminais incluem encerrados, cancelado e superado. “Fila viva” não é um denominador automaticamente idêntico a nenhum dos dois conjuntos (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:31`); definição quantitativa do aval [não verificado].
- `ChamadoColaborador._gravar_estado` escreve `status_local`, `resolvido_em`, `via_encerramento`, urgência e, quando requerido, `respondido_por`; grava auditoria antes do save e relavra perguntas de destino depois, no mesmo atomic (`app/chamados/models.py:195`, `app/chamados/models.py:222`, `app/chamados/models.py:238`, `app/chamados/models.py:274`, `app/chamados/models.py:292`).
- A guarda universal existente é de **pergunta viva ancorada**, não CSF: tripwire vivo→terminal recusa quando `_vivas_entre` retorna perguntas (`app/chamados/models.py:215`). O universo exclui perguntas validadas, considera destino ou pai sem destino e exclui competência entregue ou indecidível (`app/chamados/models.py:327`, `app/chamados/models.py:347`).
- Portas: humana `transicionar→_aplicar→_gravar_estado`, com justificativa pendente, disputa viva, pergunta viva e premissa para `resolvido` (`app/chamados/models.py:130`, `app/chamados/models.py:147`, `app/chamados/models.py:160`, `app/chamados/models.py:175`, `app/chamados/models.py:193`, `app/chamados/models.py:433`); R=`reconciliar` pergunta premissa e pergunta viva (`app/chamados/models.py:616`, `app/chamados/models.py:648`, `app/chamados/models.py:651`); K/E/CT perguntam pergunta viva (`app/chamados/models.py:749`, `app/chamados/models.py:1536`, `app/chamados/models.py:1556`). Nenhuma dessas funções chama `pode_executar_csf` nos corpos lidos (respectivos corpos acima).
- `_gate_pergunta_viva` pode reatar todas ao sobrevivente do mesmo colab-dia pela porta `reapontar_resolvedora`; se alguma não tiver destino válido, recusa e registra motivo (`app/chamados/models.py:373`, `app/chamados/models.py:405`, `app/chamados/models.py:420`). Portanto sem CSF não equivale a fechamento incondicional (`app/chamados/models.py:648`, `app/chamados/models.py:651`).

Campos de conversa/pergunta:

- Texto e `respondida_em`: escritor de resposta humana `responder_pergunta_disputa_core` (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1282`); resposta automática de foto escreve par se ainda não há texto (`app/chamados/services/disputa_emissao.py:1417`, `app/chamados/services/disputa_emissao.py:1426`); recuperação pela trilha preenche só carimbo existente no texto (`app/chamados/services/disputa_emissao.py:1434`, `app/chamados/services/disputa_emissao.py:1443`); reabertura apaga texto/carimbo/validação depois de preservar `RespostaAnterior` no mesmo atomic (`app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1401`, `app/chamados/services/disputa_emissao.py:1413`); ressurreição também zera `respondida_em` (`app/chamados/models.py:1493`).
- `validar_pergunta` lê `respondida_em` e recusa pergunta já validada (`app/chamados/services/validacao.py:343`, `app/chamados/services/validacao.py:363`).
- Carimbo (`materializada_em`,`materializada_por`,`via_resolucao`) tem atribuição central `_escrever_carimbo`; `carimbar` aplica em memória, consulta C6, desfaz se viva ou falha; pode preencher resposta/validação só com texto (`app/chamados/models.py:2545`, `app/chamados/models.py:2576`, `app/chamados/models.py:2585`, `app/chamados/models.py:2589`, `app/chamados/models.py:2598`).
- `carimbar_identidade` grava por via sem perguntar C6 e explicitamente pode deixar pergunta com resposta viva pela precedência anterior à via (`app/chamados/models.py:2614`, `app/chamados/models.py:2618`, `app/chamados/models.py:2633`).
- Followup é escrito pela porta `nota.anexar_ocorrencia`, com `autor_tipo`, usuário e conteúdo; chave opcional deduplica por evento (`app/chamados/services/nota.py:75`, `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:103`). CSF não lê Followup: conferir no núcleo com agente principal.

Reconciliação/materialização/indiretos:

- `retencao_da_retratacao` protege irmãs respondidas sem consumo e justificativa alheia pendente no ato de aprovar outra (`app/chamados/reconciliador.py:437`, `app/chamados/reconciliador.py:447`, `app/chamados/reconciliador.py:451`).
- `_retratar` consulta retenção; dentro de atomic chama R, rejeita justificativa pendente vinculada, resolve perguntas e acrescenta nota `retratado`; `retratar` é entrada pública (`app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:522`, `app/chamados/reconciliador.py:532`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:540`, `app/chamados/reconciliador.py:544`).
- `encerrar_com_retencao` consulta mesma retenção, chama E e acrescenta nota (`app/chamados/reconciliador.py:492`, `app/chamados/reconciliador.py:496`, `app/chamados/reconciliador.py:499`).
- Disputa `.fechar` recusa respostas sem veredito e perguntas vivas fora das exceções nomeadas; seu post_save solicita R no pai e só acrescenta nota se R fechou (`app/chamados/models.py:1816`, `app/chamados/models.py:1827`, `app/chamados/models.py:1801`, `app/chamados/signals.py:41`, `app/chamados/signals.py:42`, `app/chamados/signals.py:45`).
- Fechamento do chamado dispara resolver perguntas em savepoint próprio; falha desse resolver não desfaz fechamento, com logger (`app/chamados/signals.py:81`, `app/chamados/signals.py:91`, `app/chamados/signals.py:94`).
- Resolver consulta C6, tranca e resposta com testemunha antes de carimbar pela porta, poupando respostas sem materialização factual (`app/chamados/services/disputa_emissao.py:1585`, `app/chamados/services/disputa_emissao.py:1589`, `app/chamados/services/disputa_emissao.py:1596`, `app/chamados/services/disputa_emissao.py:1603`).
- Materialização ausência: destino só reconcilia quando ausência não é ABERTA, disputa só fecha por `pode_fechar` (`app/chamados/services/materializacao.py:560`, `app/chamados/services/materializacao.py:564`); destino/clones de hora consultam irmãs sem consumo antes de R (`app/chamados/services/materializacao.py:730`, `app/chamados/services/materializacao.py:738`, `app/chamados/services/materializacao.py:744`).

Concorrência / RED derivados:

- Gates R/K/E/CT recarregam estado dentro do atomic e usam select_for_update da linha; costura usa a mesma linha se atomic, então o código descreve serialização fechamento↔costura (`app/chamados/models.py:616`, `app/chamados/models.py:668`, `app/chamados/models.py:674`, `app/chamados/services/fio.py:752`, `app/chamados/services/fio.py:761`). Resultado de disputa/CSF simultâneo em produção [não verificado].
- `transicionar` não abre atomic nem chama `_recarregar_estado_se_mudou` no corpo de sua avaliação; `_gravar_estado` abre atomic depois do tripwire (`app/chamados/models.py:122`, `app/chamados/models.py:193`, `app/chamados/models.py:215`, `app/chamados/models.py:274`). Isto é limite de leitura, não prova de corrida ocorrida [não verificado].
- RED de conversa respondida **validada/materializada**, premissa morta: guarda C6 pode deixá-la fora do universo `validada_em IS NULL`; O168 ainda exige perguntar guarda de conversa no evento (`app/chamados/models.py:327`; literal do aval anexo, referência local a levantar).
- RED de mensagem livre sem PerguntaDisputa respondida: CSF pode autorizar, mas lei literal diz conversa; separar sem criar interpretação nova (CSF principal levantará referência).
- RED de irmã respondida sem consumo: `_retratar` já retém mesmo sem CSF (`app/chamados/reconciliador.py:447`, `app/chamados/reconciliador.py:518`).
- RED de disputa→pai: disputa que já fechou não garante R permitido, porque premissa e pergunta ancorada ainda julgam (`app/chamados/signals.py:42`, `app/chamados/models.py:648`, `app/chamados/models.py:651`).

Sítios irmãos fora cartório/supra (todos chamadores reais encontrados, sem CSF no ponto indicado):

- R: adesão filhos/pai `app/chamados/services/adesao.py:115`, `:150`; feriado `app/chamados/services/feriado_reavaliar.py:47`; limbo `app/chamados/services/limbo_folgas.py:101`, `:243`, `:246`; silêncio ausência `app/chamados/services/silencio_ausencia.py:101`; materialização `app/chamados/services/materializacao.py:561`, `:738`; órfãos `app/chamados/management/commands/auditar_chamados_orfaos.py:67`; triagem `app/ponto/services/triagem_batida.py:597`; veredito `app/ponto/services/veredito_celula.py:183`; ausência `app/ponto/services/ausencia.py:678`; cluster `app/ponto/services/cluster_espurio.py:98`; entrada sincronizada `app/ponto/services/entrada_sincronizada.py:31`; cobertura `app/ponto/services/cobertura.py:122`, `:317`; lastro `app/ponto/services/lastro.py:345`; detectar intervalo `app/ponto/management/commands/detectar_intervalo_ausente.py:138`; BUG115 `app/ponto/management/commands/curar_residuo_bug115.py:151`; detectar ausência `app/ponto/management/commands/detectar_ausencias.py:158`, `:821`, `:831`, `:842`; feriado posto `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`; pós vínculo `app/colaboradores/services/pos_vinculo.py:42`.
- T: lei cala `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`; geofence `app/ponto/services/geofence.py:419`; alertas turno `app/ponto/management/commands/processar_alertas_turno.py:216`; justificativa origem `app/ponto/services/justificativa.py:42`; fantasmas `app/ponto/management/commands/reconciliar_fantasmas.py:279`; grade `app/ponto/management/commands/reconciliar_grade.py:309`; vínculo `app/ponto/management/commands/reconciliar_vinculo.py:68`; absolvição por célula/premissa `app/chamados/reconciliador.py:972`, `:1395`.
- ER: justificativa bloqueio/cascata `app/ponto/services/justificativa.py:79`, `:385`.
- E: regularização externa `app/chamados/services/regularizacao_ext.py:205`; desligamento `app/colaboradores/services/desligamento.py:155`; troca escala/vínculo `app/colaboradores/services/vinculo.py:387`.
- CT: `app/chamados/services/competencia_trancada.py:184`.
- H: ações `app/chamados/services/acoes_chamado.py:70`, `:108`, `:149`, `:222`, `:277`, `:315`; fio `app/chamados/services/fio.py:717`; benefício `app/colaboradores/views.py:2068`; meio período `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`.
- K: revisão vínculo `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`; aviso régua `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`.
- Indiretos `.Disputa.fechar→signal R`: admin/remoção motivo `app/chamados/models.py:1868`, `:1975`; becos `app/chamados/services/becos.py:52`; tranca `app/chamados/services/competencia_trancada.py:204`; emissor/ciclo `app/chamados/services/disputa_emissao.py:1118`, `:1696`; reconciliador `app/chamados/reconciliador.py:412`, `:636`, `:1139`; materialização `app/chamados/services/materializacao.py:565`, `:1255`; encerrar fato `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`; foto `app/api/foto_recebida.py:80`; entrada sincronizada `app/ponto/services/entrada_sincronizada.py:59`; desligamento `app/colaboradores/services/desligamento.py:142`.

## 05/11 16:44 · sessao 9dbce1eedae8 · - · - · origem=Ronald (tmux)

Censo somente leitura de SW/cascas, com limites:

- **Cache nomeado** `juliani-__BUILD_ID__`: escritor do ID é `build_id_do_sw()`, hash SHA1 truncado dos conteúdos `sw.js`, `pwa.js`, manifest e dois ícones; `_CACHE_DO_BUILD['v']=(agora,build)` memoriza por 30 s; `service_worker()` injeta o ID, publica ETag/no-cache e retorna 304 quando coincide. Leitores do nome são instalação, gravação estática e gravação de `/`/batida; ativação compara o nome para apagar todos os demais caches, sem filtro de prefixo. (`app/core/views.py:520`, `:524`, `:533`, `:543`, `:556`, `:560`, `:564`, `:572`; `app/static/sw.js:1`, `:12`, `:20`, `:59`, `:74`.)
- **Chaves instaladas**: `/static/pwa.js`, `/static/manifest.json`, `/static/icons/icon-192.png`, `/static/icons/icon-512.png`; escritor `install → cache.addAll`; leitor `fetch /static/ → caches.match(event.request)`; demais URLs estáticas entram sob a chave Request na primeira busca sem cópia. Leituras `caches.match` pesquisam globalmente, sem abrir primeiro `CACHE_NAME`; gravações clonam a resposta sem condição explícita de status. (`app/static/sw.js:3`, `:10`, `:12`, `:54`, `:56`, `:57`, `:59`.)
- **Chaves HTML**: `/` e prefixo `/ponto/bater/` têm escritor no ramo `comCache`, mas `request.mode==='navigate'` retorna antes dele, buscando rede e somente lendo cache em falha; navegação bem sucedida não grava sua resposta. Bater-v2 `/ponto/bater-v2/` tampouco corresponde ao prefixo `/ponto/bater/`. (`app/static/sw.js:45`, `:48`, `:50`, `:68`, `:69`, `:74`; `app/ponto/urls.py:12`.)
- **Fallback** `doCacheOuErroDeRede` lê a chave Request e devolve cópia ou `Response.error()`. É chamado pela navegação, ramo comCache e resto. Login/logout/admin/onboarding tentam somente rede; métodos diferentes de GET não são interceptados. (`app/static/sw.js:29`, `:30`, `:36`, `:40`, `:42`, `:48`, `:77`, `:84`.)
- **Duas cascas** carregam `pwa.js`, manifest e meta `user-perfil=colaborador` quando `!request.user.is_staff`; meta não contém ID nem exige explicitamente usuário autenticado. A escolha normal `base_tpl` é `app_shell → modo_app`, usando carimbo de sessão/cookie ou `app=1` GET/POST; bater-v2 e base da triagem fixam `base.html`. (`app/templates/base.html:8`, `:16`, `:20`; `app/templates/base_app.html:8`, `:16`, `:20`; `app/core/context_processors.py:194`, `:205`, `:210`; `app/core/utils.py:45`, `:67`; `app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`.)
- **HTML contém usuário**: base mostra `user.username`; triagem mostra nome/matrícula; dashboard mostra nome, matrícula e empresa. V2 injeta coordenadas/raio do posto e tipo sugerido. Portanto esses documentos atuais contêm dados associados ao usuário; cache efetivamente encontrado em aparelho real é [não verificado]. (`app/templates/base.html:316`; `app/templates/ponto/triagem/base_triagem.html:10`, `:11`; `app/templates/core/dashboard_colaborador_v2.html:14`, `:16`; `app/templates/ponto/bater_ponto_v2.html:104`, `:139`, `:140`.)
- **Registro e atualização**: `registrarSW()` registra `/sw.js` escopo `/`, chama update imediatamente, a cada 15 min, no foco e `online`; mensagem `SYNC_BATIDAS` dispara sincronização. `install` chama skipWaiting; activate chama clients.claim; controllerchange recarrega uma vez se não houve input em form. (`app/static/pwa.js:11`, `:14`, `:15`, `:18`, `:24`, `:25`, `:26`, `:34`, `:41`, `:44`; `app/static/sw.js:14`, `:23`; `app/core/urls.py:24`.)
- **Versão SW versus fila**: SW novo apaga caches antigos; banco IndexedDB continua nome fixo `juliani-offline`, versão 2; upgrade apenas cria stores ausentes. Não há migração dos itens no corpo de upgrade nem ligação explícita ao ID do SW. (`app/static/sw.js:17`, `:20`; `app/static/pwa.js:48`, `:50`, `:51`, `:53`, `:55`.)
- **Config IndexedDB**: store `config`, keyPath `chave`; `salvarConfig` grava `{chave,valor}`, `lerConfig` lê `.valor`. `calcularESalvarGap` grava `ntp_gap` e `ntp_calculado_em` após `/hora/`; fallback lê gap. `obterGap` lê data, atualiza após 30 min e lê gap; `registrarBatidaOffline` e `JulianiPWA.agora` consomem `obterGap`; inicialização também calcula gap. (`app/static/pwa.js:55`, `:63`, `:67`, `:73`, `:78`, `:83`, `:86`, `:92`, `:93`, `:96`, `:100`, `:107`, `:108`, `:305`, `:307`, `:326`; `app/core/urls.py:23`.)
- **SessionStorage pertinente**: `avisos_convite` é lido por `oferecerAvisos`, escrito como `visto` pelos botões adiar/ligar; `gps_aviso_visto` é lido/escrito por confirmação offline da triagem; `scrollpos:<pathname>` é escrito no submit POST clássico e lido/removido no load em ambas as bases. A fila usa IndexedDB, sem chamadas localStorage no caminho de gravação/sincronização. (`app/static/pwa.js:351`, `:353`, `:362`, `:366`; `app/templates/ponto/triagem/apto.html:278`, `:286`; `app/templates/base.html:406`, `:410`, `:415`, `:416`; `app/templates/base_app.html:161`, `:165`, `:170`, `:171`; `app/static/pwa.js:107`, `:126`.)
- **Recarga “5 min”** ocorre quando a V2 volta a visible após diferença `>300000`, não por timer contínuo; `_vistoEm` é atualizado ao ocultar. Não há verificação onLine. Dois listeners pageshow persisted também recarregam, V2 e pwa global. (`app/templates/ponto/bater_ponto_v2.html:305`, `:306`, `:307`, `:308`, `:309`; `app/static/pwa.js:431`.)
- **Outros deslocamentos offline**: triagem, após salvar fila, redireciona ao dashboard em 3 s; V2 online usa `v2Sucesso` que redireciona em 3 s; offline V2 mostra feedback sem chamar `v2Sucesso`. (`app/templates/ponto/triagem/apto.html:289`, `:299`, `:305`; `app/templates/ponto/bater_ponto_v2.html:150`, `:157`, `:340`, `:355`, `:367`, `:374`.)
- **Sync SW** escuta tag `sync-batidas`, manda mensagem para TODOS os clientes inclusive não controlados; sem cliente mostra notificação. Cliques em notificações focam a primeira janela ou abrem `/`, sem consultar `data.url`. (`app/static/sw.js:102`, `:105`, `:106`, `:107`, `:112`, `:113`, `:116`, `:119`, `:122`, `:125`.)
- **Concorrência**: inicialização, evento online e mensagem SW chamam `sincronizarBatidas`; a função lê todos os itens antes do laço e só deleta após resposta, sem trava explícita no corpo. SW manda para todos os clientes. Guardas `_enviando` V2 e `_confirmandoOffline` triagem pertencem à página. (`app/static/pwa.js:16`, `:126`, `:132`, `:138`, `:153`, `:162`, `:320`, `:328`; `app/static/sw.js:119`; `app/templates/ponto/bater_ponto_v2.html:321`; `app/templates/ponto/triagem/apto.html:260`, `:262`.)
- **Consumidores fora da V2** da mesma gravação sem identidade: `bater_ponto.html` possui duas chamadas; triagem apto possui uma. A view ordinária `/ponto/bater/` termina na triagem; `/ponto/bater-v2/` ativa ui_v2, e os dois dashboards ligam à V2. (`app/templates/ponto/bater_ponto.html:543`, `:597`; `app/templates/ponto/triagem/apto.html:289`; `app/ponto/views.py:225`, `:235`, `:356`, `:358`; `app/ponto/urls.py:12`; `app/templates/core/dashboard_colaborador.html:39`; `app/templates/core/dashboard_colaborador_v2.html:31`.)
- **Teste existente** executa sw real em Node com mocks, recebe cache pronto do cenário e `put` vazio; verifica Response.error sem cópia, cópia quando existe, rede prevalecendo. Não mede a criação do cache de navegação nem ciclo entre usuários. A rota batida no cenário é `mode='cors'`, não navegação. (`app/core/tests/test_sw_offline_fallback.py:23`, `:33`, `:37`, `:42`, `:62`, `:64`, `:79`, `:89`, `:96`.)
- **Teste de versão** compara lista governada, altera conteúdo simulado, verifica ETag/304/no-cache/build injetado/escopo. Seu teste de template chama `os.utime`; não executei. (`app/core/tests/test_sw_casca_versao.py:35`, `:47`, `:63`, `:79`, `:102`, `:108`, `:114`, `:120`, `:126`.)
- **Lei já nomeada** Offline Fallback no glossário; duas cascas são L-071, smoke de clique nas duas é L-072. App Shell no glossário lido: [não verificado]; o corte fornecido pelo usuário é a evidência do novo padrão. (`app/docs/GLOSSARIO.md:29`; `app/docs/LEIS.md:98`, `:99`.)

Funções finais de pwa que compartilham ciclo/casca: `ehInstalado` lê navigator.standalone/media query; `inscreverPush` usa VAPID/subscription e POST com cookie CSRF; `avisarNotificacaoNegada` POST keepalive; `oferecerAvisos` usa sessionStorage; `agendarPingGeo` agenda timer 15–90 min e `executarPingGeo` envia latitude/longitude/accuracy ao `/ponto/ping-geo/` apenas online e com meta perfil; persistência pede `navigator.storage.persist` best effort. (`app/static/pwa.js:225`, `:239`, `:246`, `:259`, `:273`, `:351`, `:380`, `:392`, `:404`, `:419`, `:436`.)

## 05/11 16:44 · sessao e0d43004ad27 · - · - · origem=Ronald (tmux)

Contagem conferida no texto fornecido: **A = 23; B = 32; C = 17 novas; total = 72 unidades**. Deduplicadas as repetições de col253, col283 e col270 em C. Sem 07/11: **71 unidades no escopo**. [não verificado por arquivo]

- Base: 54 pessoas distintas + 3 temas sem identificação (Weverton, batidas livres, UX) = 57.
- Expansões de datas: A +5; B +9; C +1 = 15.
- Fechamento: **57 + 15 = 72; 72 − 1 futuro = 71**. [não verificado por arquivo]

Expansões:

| Seção | Pessoas e acréscimos |
|---|---|
| A | col239 +1; col189 +1; col43 +2; col956 +1 |
| B | col836 +1; col292 +1; col297 +3; col174 +1; col33 +1; col163 +1; col873 +1 |
| C | col523 +1 |

col297 conta **06, 13, 20 e 27/10**, quatro datas explícitas. col222 e col256 ficam **sem data individual verificada**: há referência conjunta a 12/10 na ação do relatório, mas a sonda não informa célula/data desses dois. col465 aparece nas sondas, porém não constitui caso do relatório. [não verificado por arquivo]

## 05/11 16:44 · sessao c6dcf7b81ff8 · - · - · origem=Ronald (tmux)

**Cinco classes de perda identificáveis no Q2; vínculo literal da expressão “5 atos” a essa enumeração: [não verificado].** Na cópia, os avais encontrados nomeiam evento na abertura e distinguem “sem furo: morrer por lastro / com furo: evento”, sem enumerar cinco atos. (`app/docs/PROMPTS.md:1519`; `app/docs/PROMPTS.md:1853`)

| Classe | Comportamento atual e limites |
|---|---|
| Reabertura sem pergunta restaurável | `reabrir()` grava estado vivo e chama `ressuscitar_perguntas()`, sem emitir nova pergunta. A ressuscitação seleciona somente materializadas sem resposta, pula marco aceso e pergunta ao C6 sobre cópia descarimbada; universo vazio retorna zero. (`app/chamados/models.py:1379`; `app/chamados/models.py:1391`; `app/chamados/models.py:1423`; `app/chamados/models.py:1441`; `app/chamados/models.py:1481`) |
| Identidade/duplicata | `retratar_duplicata()` carimba `VIA_DUPLICATA` pela porta de identidade; não encerra chamado. C6 mata identidade materializada somente depois de conferir resposta por decidir; portanto duplicata respondida pode permanecer viva. (`app/chamados/services/fio.py:878`; `app/chamados/services/fio.py:891`; `app/chamados/juizes.py:1092`; `app/chamados/juizes.py:1112`) |
| Validação/materialização e outro furo residual | Validação grava assinatura dentro de `atomic`; pergunta validada é morta no primeiro degrau do C6. Materialização reconcilia destino e clones; o gate recusa fechar sobrevivente cujo dia segue furado para folha. A recusa de plantio HORA com motivo pode devolver assinatura, preservando resposta. (`app/chamados/services/validacao.py:395`; `app/chamados/juizes.py:1078`; `app/chamados/services/materializacao.py:728`; `app/chamados/models.py:701`; `app/chamados/models.py:730`; `app/chamados/services/validacao.py:180`) |
| Morte por leitura | C6 declara morta por tranca, marco já aceso com carimbo, ou célula não acusando sem pendência aplicável, sem precisar novo carimbo. Célula acusando ou resposta discordante por decidir dão contraexemplo à leitura “carimbo = morte”. (`app/chamados/juizes.py:1082`; `app/chamados/juizes.py:1092`; `app/chamados/juizes.py:1119`; `app/chamados/juizes.py:1125`; `app/chamados/juizes.py:1137`) |
| Transferência da última resolvedora | `reapontar_resolvedora()` salva destino e lavra, sem encerrar destino anterior. Sua guarda protege o **novo** destino terminal; não verifica se o anterior perdeu a última resolvedora. Incidência concreta: [não verificado]. (`app/chamados/services/fio.py:742`; `app/chamados/services/fio.py:764`; `app/chamados/services/fio.py:829`; `app/chamados/services/fio.py:845`) |

**Escritores atuais importantes para o censo:**

- Nascimento, identidade do fato e reapontamento durante dedup: `PerguntaDisputa.abrir`; reapontamento posterior explícito: `reapontar_resolvedora`. **Q2 não deve ser repetido como exclusividade absoluta do segundo**, pois o primeiro também atribui `chamado_destino`. (`app/chamados/models.py:2387`; `app/chamados/models.py:2439`; `app/chamados/models.py:2465`; `app/chamados/services/fio.py:845`)
- Trio `materializada_em/materializada_por/via_resolucao`: `_escrever_carimbo`, chamado por `carimbar`, `carimbar_identidade`, `descarimbar`; lote por `descarimbar_em_lote`. Há ainda troca isolada de via em `encerrar_por_fato_no_espelho`. (`app/chamados/models.py:2545`; `app/chamados/models.py:2551`; `app/chamados/models.py:2614`; `app/chamados/models.py:2635`; `app/chamados/models.py:2644`; `app/chamados/models.py:2662`)
- `validada_em/validada_por`: `_carimbar_validacao` escreve, `devolver_validacao_recusada` zera; resposta nova zera assinatura; ressuscitação e correção zeram em lote; `carimbar(resposta='validar')` também preenche quando há texto. (`app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:1493`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:2580`)
- `resposta_colab/respondida_em`: `responder_pergunta_disputa_core` escreve resposta e instante, desfaz validação anterior e chama `reabrir_por_resposta`; correção apaga resposta e carimbos, guardando versão anterior dentro do mesmo atomic. (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1300`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1401`)

**Chamadores de perda/carimbo, inclusive fora dos sítios iniciais:**

- Dedup: comando `dedup_perguntas_fato` e migração de resolvedoras usam `retratar_duplicata`. (`app/chamados/management/commands/dedup_perguntas_fato.py:88`; `app/chamados/services/fio.py:943`)
- Reapontamento: reconciliador, declaração textual, limbo e gate do chamado usam porta de fio. (`app/chamados/reconciliador.py:922`; `app/chamados/reconciliador.py:1570`; `app/chamados/reconciliador.py:1582`; `app/chamados/services/declaracao_texto.py:320`; `app/chamados/services/limbo_folgas.py:242`; `app/chamados/models.py:420`)
- Carimbos não identidade: ausência declarada, declaração/materialização, aprovação pelo lastro, sem turno aberto e célula absolvida passam por `carimbar`, que pergunta ao C6 no pós-estado e desfaz em memória se viva. (`app/chamados/services/materializacao.py:546`; `app/chamados/services/materializacao.py:1199`; `app/chamados/services/materializacao.py:1346`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:620`; `app/chamados/models.py:2585`)
- Identidade também nasce em decisão admin, paridade recusada e colisão de plantio. (`app/chamados/models.py:2511`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:1066`)

**Leitores fora dos sítios iniciais:**

- App filtra pelo juiz; propriedade `.viva` delega ao juiz; `estado` traduz assinatura e C6; disputa usa ausência de respostas sem veredito e ausência de fio vivo para permitir fecho. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/models.py:2762`; `app/chamados/models.py:2488`; `app/chamados/models.py:1783`)
- Cartório, supra e emissão retroativa distinguem chamados com fio vivo; prévia de folha e explicador também leem `fio_vivo`. (`app/ponto/services/cartorio.py:1339`; `app/ponto/management/commands/supra_juiz.py:215`; `app/ponto/management/commands/emitir_furo_retroativo.py:98`; `app/folha/previa.py:144`; `app/chamados/services/explicador.py:137`)
- Reconciliar grade lê resolvedoras vivas; avisos, relatórios, detalhe do dia e limbo leem C6/fio. (`app/ponto/management/commands/reconciliar_grade.py:460`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/relatorios_disputa.py:18`; `app/ponto/services/dia_do_colab.py:70`; `app/chamados/services/limbo_folgas.py:201`)

**Contraexemplos/concurrency precisos:**

- Novo destino terminal é protegido por estado relido do banco e `select_for_update` dentro de atomic; o gate `PerguntaDisputa.abrir` também usa a guarda, mas a própria guarda declara que essa chamada pode vir fora de atomic e nesse caso só lê banco. (`app/chamados/services/fio.py:752`; `app/chamados/services/fio.py:761`; `app/chamados/services/fio.py:837`; `app/chamados/models.py:2436`)
- Desfazer carimbo de instância antiga não é decisão atual: `relavra_via` relê pergunta e disputa travadas e só descarimba se C6 da linha atual disser viva. (`app/chamados/services/relavra_via.py:214`; `app/chamados/services/relavra_via.py:221`; `app/chamados/services/relavra_via.py:227`)
- O SQL `q_vivas` declara contenção e não igualdade com C6; não implementa tranca por leitura, admin decidiu, lei acusa e juiz do lastro. Usá-lo como substituto exato muda o universo contado. (`app/chamados/models.py:2723`)

**Irmãos comprovados por corpo:**

1. `PerguntaDisputa.abrir` reaponta pergunta histórica existente e retorna a mesma, sem gerar nova para o destino anterior. (`app/chamados/models.py:2420`; `app/chamados/models.py:2439`; `app/chamados/models.py:2464`)
2. `migrar_resolvedoras_vivas` combina dedup e transferência e termina em relatório, sem fechamento do antigo no corpo; chamado anterior sobreviver e ficar mudo depende de seus outros atos/leitores [não verificado]. (`app/chamados/services/fio.py:931`; `app/chamados/services/fio.py:943`; `app/chamados/services/fio.py:947`; `app/chamados/services/fio.py:960`)
3. `_reconciliar_destino_e_clones` repete mesma decisão para destino e clones; residual mantém vivo pelo gate da premissa. (`app/chamados/services/materializacao.py:728`; `app/chamados/services/materializacao.py:740`; `app/chamados/services/materializacao.py:748`; `app/chamados/models.py:730`)

## 05/11 16:44 · sessao 0645a9ea289e · - · - · origem=Ronald (tmux)

- **Lei específica:** O168 exige guarda de conversa também no evento, cartório não fecha cobrança sem furo com conversa e `--executar` só sai depois dessa guarda e `CSF_RETRATADO` medido em zero (`app/docs/BACKLOG.md:1379`). O complemento de 05/11 sobre núcleo, DIFF sombra e >10% da fila viva consta apenas do anexo do usuário; localização na cópia [não verificado].

- **Leis estruturais:** testemunhas leem a mesma autoridade; evento julga e carimba; consumidores leem a lâmpada; cron de varredura é vigia; contrato exige um juiz por pergunta e um escritor por entidade (`CLAUDE.md:20`, `CLAUDE.md:166`, `CLAUDE.md:175`; `app/docs/LEIS.md:94`, `app/docs/LEIS.md:95`). O teto temporal exige instante ou fato encerrado, e o censo de escritores precede filtro/regra nova (`CLAUDE.md:185`, `CLAUDE.md:193`).

- **Autoridades distintas já existentes:** chamado vivo = `VIVOS`; transição = `decidir`; premissa morta = `premissa_morta`; pergunta viva = `pergunta_viva`; disputa pode fechar = `DisputaSupervisao.pode_fechar`; conjunto com fio vivo = `fio_vivo`; dia sem trabalho = `dia_sem_trabalho` (`app/core/juizes.py:261`, `app/core/juizes.py:262`, `app/core/juizes.py:263`, `app/core/juizes.py:266`, `app/core/juizes.py:267`, `app/core/juizes.py:275`, `app/core/juizes.py:278`). O parentesco “vivo implica cobrança” foi expressamente separado no próprio registro (`app/core/juizes.py:100`).

- **CSF hoje:** `pode_executar_csf` recusa qualquer pergunta com `respondida_em` preenchido nas disputas de origem ou em `perguntas_que_resolvem`; não verifica `validada_em`, `materializada_em`, status da disputa ou conteúdo de followup (`app/ponto/supra_juiz.py:539`). Os testes existentes cobrem ausência de conversa, resposta na origem, resposta direta e coleção vazia (`app/ponto/tests/test_supra_juiz_executor.py:38`).

- **Supra hoje:** `classificar_dia` alimenta placar/alarme; o executor lê `veredito='cobranca_indevida'` e um código de protesto da lavra, consulta CSF, depois cobrança órfã de regeneração e finalmente chama `retratar`; incrementa `CSF_RETRATADO` somente se a porta devolver verdadeiro (`app/ponto/management/commands/supra_juiz.py:239`, `app/ponto/management/commands/supra_juiz.py:261`, `app/ponto/management/commands/supra_juiz.py:275`, `app/ponto/management/commands/supra_juiz.py:286`).

Contraexemplos e ingredientes para os casos RED:

| Cadastro + estado + ato | Distinção demonstrada e expectativa lastreada |
|---|---|
| Trabalho, todas as marcas casadas, pergunta respondida na origem ou destino; fecho automático CSF. | A conversa exige decisão humana; CSF recusa (`app/ponto/supra_juiz.py:540`; `app/ponto/tests/test_supra_juiz_executor.py:42`). |
| Trabalho, 731 minutos para previsto 720, dois marcos intermediários missing; julgamento CSF. | Minutos suficientes não provam ausência de furo; não deve ser classificado CSF (`app/ponto/tests/test_supra_juiz_cobranca.py:32`). |
| `trabalha=None`, sem ata/tipo; julgamento de “dia sem trabalho”. | Indeterminação não é folga: cobrança não cala (`app/chamados/juizes.py:411`, `app/chamados/juizes.py:420`; `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:56`). |
| Célula com chamado vivo, mas sem veredito; `supra --executar`. | Ausência de lavra é “não sei”: não retrata e conta `CSF_SEM_LAVRA` (`app/ponto/management/commands/supra_juiz.py:245`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:134`). |
| Ausência parcial com período e resto missing; tentativa de calar toda cobrança. | A parcial cobra o resto; não equivale a dia inteiro coberto (`app/chamados/juizes.py:398`, `app/chamados/juizes.py:424`; `app/ponto/supra_juiz.py:525`). |
| Corte 21, ato sobre 20/10 e 21/10. | São competências distintas: período termina em 20/10; 21/10 inicia o período seguinte, respeitando corte da empresa (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`). |
| Turno noturno da véspera ainda aberto após meia-noite; julgamento de furo. | Data anterior não encerra o fato: juiz verifica aberta viva e fim previsto; sem previsto não afirma encerramento (`CLAUDE.md:185`; `app/ponto/turnos.py:2105`). |
| Saída 06:30 de turno 21:30 da véspera; contagem por calendário. | A saída pertence ao dia-turno anterior; não deve produzir órfã fantasma no seguinte (`app/ponto/tests/test_supra_juiz_orfa_builder.py:1`, `app/ponto/tests/test_supra_juiz_orfa_builder.py:16`). |
| Célula gerada por vínculo anterior, vínculo novo ativo; julgamento histórico. | A geradora governa dentro da vigência; geradora ausente ou fora da vigência devolve `None`, sem escolher outro vínculo (`app/escala/alimentacao.py:269`). |
| Dia fora da janela carregada; consumidor interpreta chave ausente como ausência real. | Alimentação exige janela completa; chave ausente significa “não existe”, não “não carreguei” (`app/escala/alimentacao.py:14`, `app/escala/alimentacao.py:240`). |
| Resposta concorrente entre leitura de CSF e fechamento. | A guarda CSF apenas percorre relações; a porta possui recarga e trava da linha do chamado dentro de atomic; execução concreta desse entrelaçamento e proteção da pergunta pela mesma trava [não verificado] (`app/ponto/supra_juiz.py:543`; `app/chamados/models.py:668`). |

O histórico de cinco linhas `CSF_RETRATADO=1` é texto de backlog, não série disponível de produção atual; medição atual zero, DIFF e proporção da fila viva [não verificado] (`app/docs/BACKLOG.md:1381`).

## 05/11 16:44 · sessao de8020ea9668 · - · - · origem=Ronald (tmux)

Referências para consolidar, sem usar registros de produção dos documentos como medição desta obra:

- **Lei do aval:** App Shell e identidade da fila vêm dos anexos fornecidos; a cópia documenta somente o padrão vizinho Offline Fallback, que devolve cache ou `Response.error()`. (`app/docs/GLOSSARIO.md:29`; `app/static/sw.js:26`; anexos `avais.txt`:7–8 conforme numeração adotada pelo pai.)
- **Duas cascas:** L-071 exige renderização de `base.html` e `base_app.html`; L-072 alcança service worker e template base, com smoke nas duas cascas. (`app/docs/LEIS.md:98–99`; `CLAUDE.md:199–200`.)
- **Juízes existentes:** `JUIZES.md` é lookup e aponta o registro vivo; a batida tem juízes de geofence e selfie; turno/marcos tem dia do turno, próximo tipo, marcos e cruzamento da meia-noite; escala tem vínculo do dia e cobertura da data. (`app/docs/JUIZES.md:7–15`; `app/core/juizes.py:195–204`, `:1173–1183`, `:1285–1286`.)
- **Cortes correspondentes:** geofence e selfie aparecem em duas linhas cada; vínculo do dia, cobertura da data e dia sem trabalho têm cortes próprios. (`app/docs/CORTES.md:93–94`, `:282–284`, `:287`, `:416`.)

Contraexemplos úteis:

1. **Vínculo trocado:** o vínculo atual não substitui automaticamente a geradora histórica; a juíza procura a geradora da célula, recusa quando falta na alimentação ou quando o dia fica fora de sua vigência; sem geradora, usa os vínculos que cobrem a data. `escala_vigente` carrega e consulta essa juíza. (`app/escala/alimentacao.py:269–286`; `app/escala/servico_jornada.py:38–58`.)
2. **Célula sem ata:** `trabalha=None` e ata vazia não significam folga; `dia_sem_trabalho` só cala por `trabalha=False` ou `ata.tipo_dia` preenchido e diferente de trabalho, com ressalva para cobertura parcial. (`app/chamados/juizes.py:411–426`; `app/docs/CORTES.md:416`.)
3. **Competência 21→20:** janela vem do cadastro da empresa; o período atual muda quando `data.day >= corte`, inclusive na virada de ano. A lei proíbe decidir período pelo mês civil. (`app/ponto/janelas.py:10–33`, `:36–63`; `app/docs/LEIS.md:123`.)
4. **Cross-meia-noite:** marcos avançam de dia quando o seguinte não supera o anterior; início igual ao fim também cruza. O dia do turno tem autoridade declarada em `turnos_do_colab`. (`app/escala/servico_jornada.py:61–86`, `:96–103`; `app/core/juizes.py:196`.)
5. **Hora antiga/futura:** hoje `hora_do_aparelho` aceita atraso entre zero e 48 horas, inclusive; fora disso grava `agora` e conserva a hora lida do aparelho. A resposta normativa do novo aval para `>48h`/futuro não está explicitada no anexo; não transformar esse código em gabarito. (`app/api/hora_do_aparelho.py:62–67`, `:124–153`; `app/docs/LEIS.md:136`; resultado do aval nessa borda `[não verificado]`.)
6. **Fato de chão em competência fechada:** a porta aplica o bloqueio de competência apenas quando `fato_de_chao=False`; a lei nomeia o chokepoint e proíbe barrar batida de chão em runtime. (`app/ponto/registro_batida.py:123–137`; `app/docs/LEIS.md:78`; `CLAUDE.md:140–144`.)
7. **Flush antigo junto de nova jornada:** existe caso escrito pela regra no qual uma entrada retida por 24 horas não transforma a entrada da jornada seguinte em eco; o teste cobre também 13 e 15 horas entre entradas. Isso é cenário de teste da cópia, não medição atual de produção. (`app/ponto/tests/test_eco_flush_mesmo_turno.py:4–10`, `:28–45`.)

Limite dos testes atuais: o harness de SW testa três ramos com/sem rede e cache previamente simulado; seus casos exigem Response válida, preferência pela cópia sem rede e preferência pela rede disponível. Não verifica gravação real da navegação nem identidade de fila. (`app/core/tests/test_sw_offline_fallback.py:33–45`, `:60–66`, `:77–101`.)

## 05/11 16:44 · sessao 79c89fae86fe · - · - · origem=Ronald (tmux)

Censo da fila e batida, somente leitura:

- **Banco e configuração.** `abrirDB()` abre IndexedDB `juliani-offline`, versão 2; cria `batidas_pendentes` com `id` autoincremental e `config` com chave `chave`. `salvarConfig()` escreve `{chave, valor}`; `lerConfig()` lê `valor`. `calcularESalvarGap()` consulta `/hora/`, escreve `ntp_gap` e `ntp_calculado_em`; no erro lê gap existente. `obterGap()` lê as duas chaves, recalcula após 30 minutos; `registrarBatidaOffline()` e a função exportada `JulianiPWA.agora()` consomem o gap. (`app/static/pwa.js:48-104`, `app/static/pwa.js:107-109`, `app/static/pwa.js:305-318`.)

- **Escritor dos itens.** `registrarBatidaOffline(tipo, lat, lng, accuracy, capturedAt, fotoBlob)` grava `tipo`, `timestamp` corrigido, `timestamp_dispositivo` bruto, `timestamp_gap_ms`, `latitude`, `longitude`, `gps_accuracy`, `gps_captured_at`, `foto`; nenhum identificador do colaborador integra o objeto. Os chamadores são os dois ramos de V2, os dois caminhos do template antigo e a confirmação offline da triagem. (`app/static/pwa.js:107-122`; `app/templates/ponto/bater_ponto_v2.html:356`, `app/templates/ponto/bater_ponto_v2.html:374`; `app/templates/ponto/bater_ponto.html:543`, `app/templates/ponto/bater_ponto.html:597`; `app/templates/ponto/triagem/apto.html:289`.)

- **Leitores por campo.** `sincronizarBatidas()` lê todos os itens: `timestamp` ordena; `id` identifica a exclusão; `tipo`, `latitude`, `longitude`, `timestamp_dispositivo` e `foto` alimentam multipart. `timestamp_gap_ms`, `gps_accuracy`, `gps_captured_at` são gravados no item e não entram no multipart. O indicador lê apenas a contagem global da store, sem distinguir colaborador. (`app/static/pwa.js:126-175`, `app/static/pwa.js:184-201`.)

- **Identidade do envio.** A sincronização exige somente meta `user-perfil=colaborador`, usa o cookie CSRF atual e não envia colaborador nem `id` da fila. Ambas as APIs obtêm o colaborador de `request.user.colaborador`; a configuração admite JWT e sessão Django. Assim, no cenário A deixa fila e B entra no mesmo navegador/origem, o código não conserva vínculo do item com A e a API recebe a identidade autenticada de B. (`app/static/pwa.js:128-157`; `app/api/views.py:398-404`; `app/api/views_core.py:385-391`; `app/config/settings/base.py:32-43`.)

- **Remoção e repetição.** Resposta `ok` remove o item após JSON válido; HTTP 400 também remove. Outros status, erro de rede e falha ao interpretar JSON deixam o item. Os gatilhos são mensagem `SYNC_BATIDAS`, evento `online` e inicialização online. Não há trava no corpo de `sincronizarBatidas()` entre `getAll()` e os POSTs: chamadas simultâneas podem ler o mesmo item antes da exclusão; resultado efetivo em navegador não foi medido. (`app/static/pwa.js:15-16`, `app/static/pwa.js:126-183`, `app/static/pwa.js:320-328`; medição `[não verificado]`.)

- **Hora da batida.** A fila cria os horários depois de aguardar `obterGap()`; envia o bruto e usa o corrigido somente na ordenação. Ambas as APIs chamam `hora_do_aparelho()`: intervalo inclusivo de 0 a 48 horas de atraso usa hora do aparelho; futuro, mais de 48 horas ou valor ilegível usam `agora`, conservando o bruto quando legível e emitindo trilha conforme tolerância. (`app/static/pwa.js:107-118`, `app/static/pwa.js:137-147`; `app/api/views.py:527-539`; `app/api/views_core.py:529-531`; `app/api/hora_do_aparelho.py:62-67`, `app/api/hora_do_aparelho.py:124-153`.)

- **Leitores externos dos campos de horário.** `hora_do_aparelho.descartadas()` compara `Batida.timestamp` e `Batida.timestamp_dispositivo`; a tela de pendentes exibe timestamp bruto, gap e horário de sincronização; `core/views.py` expõe `timestamp_gap_ms` como `gap_ms`. O POST atual da fila não envia gap e a criação da batida na API não preenche gap nem sincronizado_em. (`app/api/hora_do_aparelho.py:156-185`; `app/templates/ponto/batidas_offline_pendentes.html:71-90`; `app/core/views.py:1022`; `app/static/pwa.js:143-157`; `app/api/views.py:682-697`.)

- **GPS e foto.** A API lê `gps_accuracy`/`accuracy`, coordenadas e foto; recusa ausência de coordenadas com 403. O item guarda accuracy, porém o sincronizador não a transmite. `classificar_posicao()` devolve `gps_incerto` quando fora do raio sem accuracy, e somente `fora` sustenta acusação; portanto a omissão alcança o julgamento do geofence. Foto é enviada no multipart e anexada pela porta `anexar_foto(... registro='criar')`, que escreve `FotoBatida` e `Batida.foto`. (`app/api/views.py:432-466`, `app/api/views.py:783-795`; `app/static/pwa.js:113-118`, `app/static/pwa.js:143-157`; `app/ponto/services/geofence.py:95-123`; `app/ponto/registro_batida.py:296-311`.)

- **Canal e selfie.** `canal_da_batida()` deriva APK/PWA da presença de `device_id`; ambas as APIs e a porta web gravam o canal. `selfie_ausente()` usa origem, canal, foto e tempo decorrido: PWA tem prazo 0, APK 90 minutos. As APIs e o comando retroativo leem esse juiz. (`app/ponto/registro_batida.py:53-91`; `app/api/views.py:477-483`, `app/api/views.py:682-695`; `app/api/views_core.py:471`, `app/api/views_core.py:680-692`; `app/ponto/services/triagem_batida.py:707-719`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:74`.)

- **View web e API são entradas distintas.** `/ponto/bater/` e `/ponto/bater-v2/` roteiam para `bater_ponto()`. Ela lê usuário da sessão, monta contexto, chama triagem, encaminha POST E/S a `ramo_registrar_es()` e escolhe template V2 ou triagem. A fila envia a `/api/ponto/bater/`, roteado separadamente nos dois kernels. O endpoint antigo `/ponto/batida-offline/` devolve 410. (`app/ponto/urls.py:11-13`; `app/ponto/views.py:241-260`, `app/ponto/views.py:346-358`, `app/ponto/views.py:417-428`; `app/api/urls.py:67`; `app/api/urls_core.py:14`.)

- **Chokepoint.** `registrar_batida()` sanitiza geografia, retorna mesma batida de mesmo tipo até ±60 segundos ou mesma chave colaborador/instante; fato de chão não passa pela recusa de competência fechada; colisão oposta usa 300 segundos com escala/marcos ou 60 sem eles, com exceção de marco próprio no dia ou véspera; criação dispara `_fechamento_por_evento()`. Existe constraint única colaborador/timestamp; o trecho de consulta/criação não contém captura de `IntegrityError` nem bloqueio transacional explícito. (`app/ponto/registro_batida.py:5-50`, `app/ponto/registro_batida.py:94-161`, `app/ponto/registro_batida.py:175-183`; `app/ponto/models.py:102-106`.)

- **Consumidores adicionais da criação.** Além dos dois kernels e da triagem, chamam `registrar_batida()` ações de chamado, materialização, regularização externa, saída administrativa, justificativas e veredito de célula; comandos de ensaio também chamam a função. (`app/chamados/services/acoes_chamado.py:44`; `app/chamados/services/materializacao.py:894`, `app/chamados/services/materializacao.py:974`; `app/chamados/services/regularizacao_ext.py:77`; `app/ponto/views.py:1816`; `app/ponto/services/justificativa.py:173`, `app/ponto/services/justificativa.py:346`; `app/ponto/services/veredito_celula.py:301`; `app/ponto/management/commands/testar_pwa.py:184`, `app/ponto/management/commands/testar_pwa.py:228`.)

- **Dedup da API e jornada.** Antes do chokepoint, a API devolve batida existente do mesmo tipo em ±5 minutos pelo `ts_efetivo`; calcula sequência por `sequencia_na_jornada()`. Tipo ao vivo até 5 minutos é respondido por `proximo_tipo_de()`; retroativo usa vínculo do dia e marcos. A ordem de substituição, entretanto, é consultada com `agora`, não com `ts_efetivo`. (`app/api/views.py:557-585`, `app/api/views.py:587-631`; gêmeo `app/api/views_core.py:680-697`.)

Irmãos e contraexemplos úteis:

- V2 recarrega em `pageshow` persistido e na volta à visibilidade depois de cinco minutos, sem consulta à rede nesse trecho. (`app/templates/ponto/bater_ponto_v2.html:305-310`.)
- A triagem confirma offline mesmo sem GPS, anuncia futura confirmação da supervisão e grava o item; API recusa sem GPS com 403 e sincronizador conserva 403, repetindo o item em futuros gatilhos. (`app/templates/ponto/triagem/apto.html:275-289`; `app/api/views.py:448-466`; `app/static/pwa.js:158-177`.)
- Após gravar offline, a triagem navega automaticamente ao dashboard em três segundos. (`app/templates/ponto/triagem/apto.html:299-305`.)
- O template antigo põe fila em qualquer HTTP não-ok; V2 põe fila somente na exceção do `fetch`, devolvendo erros HTTP ao usuário. (`app/templates/ponto/bater_ponto.html:533-543`; `app/templates/ponto/bater_ponto_v2.html:335-356`.)
- A troca de SW não migra o esquema dos itens por si: a abertura IndexedDB continua versão 2 e `onupgradeneeded` apenas cria stores ausentes; itens antigos são lidos sem validação de identidade ou versão. (`app/static/pwa.js:48-58`, `app/static/pwa.js:126-157`.)
- Duas abas, evento online junto de init ou mensagem do SW podem iniciar sincronizações independentes; a idempotência atual da API/chokepoint depende de colaborador, tipo e tempo, enquanto o `id` IndexedDB não é enviado. (`app/static/pwa.js:15-16`, `app/static/pwa.js:126-183`, `app/static/pwa.js:320-328`; `app/api/views.py:557-576`; `app/ponto/registro_batida.py:107-122`.)
- Campo de banco `Batida.canal` tem leitores além do ato: juiz de selfie no comando retroativo; latitude/longitude/accuracy são consumidas por geofence e latitude/longitude por contagem de divergência de vínculo. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:74`; `app/ponto/services/geofence.py:40-61`, `app/ponto/services/geofence.py:95-123`.)

## 05/11 16:44 · sessao 584e2fbf280a · - · - · origem=Ronald (tmux)

Localização documental, sem escolher entre numerações homônimas:

- “Item 3” da **O261-E-F2-FILA** é literalmente “carimbar sem select_for_update, carimbo em memoria apos erro pos-C6”. A documentação separa depois a metade memória, registrada como curada, da metade trava, registrada como parada com duas candidatas; a ordem MANHA nomeia “O261 item 3 PEP no carimbar”. Referências: `app/docs/BACKLOG.md:1154`, `app/docs/RELATO.md:66`, `app/docs/PROMPTS.md:1853`.
- O registro do pouso F2-FILA informa alterações de identidade, vias e memória; não declara nesse registro que resolveu a metade trava. Referências: `app/docs/RELATO.md:53`, `app/docs/RELATO.md:66`.
- “Item 3” do corte **FIO-MORTO-E-ESTRUTURAL** designa três leitores: fábrica de perguntas, supra_juiz e registro `_C1`. O próprio registro vivo ainda usa “O261 item 3” para explicar a separação entre chamado vivo e fio vivo. Referências: `app/docs/CORTES.md:64`, `app/docs/CORTES.md:253`, `app/core/juizes.py:100`.
- Na ordem histórica de 18/10, “item (3)” é o contador fio morto, esperado zero fora do dia de hoje. Referência: `app/docs/RELATO-ARQUIVO.md:368`.
- Existe a expressão **Policy Enforcement Point** em aval O261-VETO-NA-PORTA; ali o documento exige veto dentro da porta única. A identidade dessa expressão com a abreviação “PEP” do item MANHA é inferência, não definição literal do item. Referências: `app/docs/PROMPTS.md:1656`, `app/docs/PROMPTS.md:1853`; identidade exata `[nao verificado]`.
- As outras ocorrências de PEP no RELATO-ARQUIVO são PEP 562, relativas a `__getattr__` de módulo Python. Referências: `app/docs/RELATO-ARQUIVO.md:17996`, `app/docs/RELATO-ARQUIVO.md:28267`.

Autoridades e leis:

- C6: “a pergunta da disputa esta viva?” → `chamados/juizes.py::pergunta_viva`; C15 pergunta pela coleção → `fio_vivo`; C17 porta alcançável → `porta_alcancavel_do_dia`; C18 dia sem trabalho → `dia_sem_trabalho`. Referências: `app/core/juizes.py:112`, `app/core/juizes.py:125`, `app/core/juizes.py:266`, `app/core/juizes.py:275`, `app/core/juizes.py:277`, `app/core/juizes.py:278`.
- JUIZES.md é lookup; o registro vivo prevalece em caso de divergência. Referências: `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`.
- L-061: consumidores leem célula/ata, nunca rejulgam; L-007: estado tem escritor canônico, tripwire e trilha verdadeira; L-110: casos e resultado esperado nascem da regra e do cadastro, antes do código. Referências: `app/docs/LEIS.md:95`, `app/docs/LEIS.md:65`, `app/docs/LEIS.md:136`.
- Há regra explícita para intercalaciones responder/validar/recusar/fechar/reabrir/lote: mesmas entradas produzem mesmo resultado de `pergunta_viva`, e nenhum estado muda sem trilha. A documentação nomeia #44764 como defeito de determinismo. Referências: `app/docs/CORTES.md:333`, `app/docs/CORTES.md:326`.
- Relógio injetado: juiz e montador recebem instante do chamador; a regra não manda chamar relógio interno. Referência: `app/docs/CORTES.md:370`.
- Batida que chega absolve na passada seguinte; caiu a parte anterior que segurava cobrança já respondida pela batida. Referência: `app/docs/CORTES.md:413`.
- L-113 altera L-092/L-052 quanto a exportada; portanto a frase geral de exportada intocada requer ler o corte específico aplicável. Referência: `app/docs/LEIS.md:149`.

_1874 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
