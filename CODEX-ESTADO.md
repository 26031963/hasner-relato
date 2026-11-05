# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 16:41** · publicado em **05/11/2026 16:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 16:41 · sessao ff619ce5765a · - · - · origem=Ronald (tmux)

**O censo separa classes prováveis; os anexos não permitem declarar a causa estrutural de cada ocorrência.** Faltam a tela/requisição original, o status D4, as perguntas, suas âncoras e os atos de validação/materialização. Esses pontos ficam como **[nao verificado]**.

Adotei uma unidade por **pessoa+dia explícito**; pessoa sem dia vale uma unidade `SD`. Repetições entre A/C não somam novamente. Isso produz **72 unidades**, incluindo duas fora do escopo. A contagem deriva do relatório transcrito, sem arquivo local com linhas para citar. **[nao verificado]**

Há duas identidades distintas: **col253 do [nome] = ID 407**; **col253 = ID 253**. **col751 de [nome] = ID 923**. col465 aparece somente no JSONL e não entra na contagem do relatório. **[nao verificado]**

**G1 — tela × cálculo**

O dado comprova a fotografia transcrita de células/exportação, mas não contém a resposta da tela acusadora. Portanto, os 19 casos abaixo são **candidatos de apresentação**, não 19 contradições reproduzidas. `concorde`, status D4, chamado vivo, aprovação e entrada no TXT são respostas diferentes. O juiz do status é D4; o juiz do TXT é `classificar_export`. (`app/docs/JUIZES.md:74`; `app/docs/JUIZES.md:88`; resultado das telas: **[nao verificado]**)

| Tela possível | Função/template que efetivamente responde | Lê o juiz ou deriva? |
|---|---|---|
| Calendário do perfil | `calendario_ponto` → `contexto_calendario`; amarelo conforme `dia.status` | Lê `status_dos_dias_lavrados`; o template pinta o status recebido. (`app/colaboradores/views.py:1935`; `app/colaboradores/services/calendario.py:602`; `app/templates/colaboradores/partials/_calendario_grade.html:70`) |
| Espelho do gestor | `espelho_ponto` → `espelho_do_colab` → `montar_semanas` | A faixa lê D4; `espelho_do_colab` sozinho não monta esse status. (`app/ponto/views.py:158`; `app/ponto/services/espelho.py:156`; `app/ponto/services/espelho.py:170`) |
| App, espelho v2 | `api_espelho_v2` | Lê D4 e traduz por `status_do_app`; não escolhe novamente o status. (`app/api/views.py:1335`; `app/api/views.py:1495`; `app/ponto/status_dia.py:89`) |
| Retrato da folha e bloco do colaborador | `retrato_folha`/drawer → `linhas_do_retrato`/`linha_do_colab` → `desenho_do_led` | Lê fotografia gravada. `_balde` traduz `entra` para `entram`; demais retidos para `bloqueado`, ressalvada rescisão. Frescor tem juiz próprio. (`app/folha/views_retrato.py:190`; `app/folha/retrato_folha.py:89`; `app/folha/retrato_folha.py:915`; `app/folha/views_retrato.py:202`) |
| Fechamento mensal, tique “entra” | `fechamento_mensal` → `montar_resumos` → `_folha_do_juiz` | Lê `classificar_export`. (`app/ponto/views.py:1147`; `app/ponto/services/fechamento_lista.py:28`; `app/ponto/services/fechamento_lista.py:70`) |
| Fechamento mensal, coluna **Status** | Template `_fech_linha.html`: aprovado → “Aprovado”; turno aberto → “T. Aberto”; restante com fechamento → **“Pendente”** | **Deriva a legenda do estado administrativo**, independentemente do tique do TXT. A mesma linha pode mostrar “Pendente” e “entra”. (`app/templates/ponto/_fech_linha.html:40`; `app/templates/ponto/_fech_linha.html:50`; `app/templates/ponto/_fech_linha.html:53`; `app/templates/ponto/_fech_linha.html:11`) |

A última linha é uma derivação própria demonstrável no código. Ela não prova que foi essa a tela vista por Maurício, nem que “Pendente” significava retenção do TXT naquele relato. A identificação da tela permanece **[nao verificado]**. A exigência da LEI-AKITA 2 está em `CLAUDE.md:20`.

Também não basta comparar `entra` com bloqueio de aprovação: **o mesmo juiz responde duas perguntas**, por `status/motivo` e `travas_aprovacao`. Os JSONL fornecidos não imprimem `travas_aprovacao`. (`app/folha/export.py:751`; `app/folha/export.py:777`; valor nos casos: **[nao verificado]**)

Nos dois casos exigidos, **col253/ID253 e col283/ID283**, o export fornecido diz `entra` em 10 e 11; a tela “folha bloqueada” não foi capturada. **[nao verificado]**

A sonda abaixo chama os **construtores reais**, renderiza o template real do fechamento e chama a API real. Não replica o julgamento. Para casos `SD`, imprime somente as comparações por competência, sem inventar dia. O relógio de referência não restaura o estado histórico do banco. **Execução e resultados: [nao verificado]**.

Fontes das chamadas: `app/colaboradores/services/calendario.py:159`; `app/ponto/services/espelho.py:1194`; `app/ponto/services/espelho.py:156`; `app/api/views.py:1335`; `app/ponto/services/fechamento_lista.py:40`; `app/folha/retrato_folha.py:915`; `app/ponto/status_dia.py:201`; `app/folha/export.py:746`.

```bash
PYTHONDONTWRITEBYTECODE=1 \
PGOPTIONS='-c default_transaction_read_only=on' \
python -B manage.py shell <<'PY'
import calendar
import datetime as dt
import json
from functools import lru_cache
from html.parser import HTMLParser
from unittest.mock import patch

from django.db import connection, transaction
from django.template.loader import render_to_string
from django.utils import timezone
from rest_framework.test import APIRequestFactory, force_authenticate

from colaboradores.models import Colaborador
from colaboradores.services.calendario import contexto_calendario
from folha.export import classificar_export, prontidao
from folha.models import RetratoFolha
from folha.retrato_folha import (
    linha_do_colab, desenho_do_led, frescor_dos_retratos,
)
from ponto.janelas import janela_fechamento
from ponto.models import FechamentoMensal
from ponto.services.espelho import espelho_do_colab, montar_semanas
from ponto.services.fechamento import universo_da_competencia
from ponto.services.fechamento_lista import montar_resumos
from ponto.status_dia import status_do_dia_lavrado, status_dos_dias_lavrados
from ponto.turnos import CargaDaJanela
from api.views import api_espelho_v2

AGORA = dt.datetime(2026, 11, 5, 19, 12, 47, tzinfo=dt.timezone.utc)
HOJE = timezone.localdate(AGORA)
ANO = 2026
CASOS = [
    (504, "2026-10-21"), (226, "2026-10-21"),
    (709, "2026-10-22"), (255, "2026-10-03"),
    (204, "2026-09-21"), (43, "2026-10-26"),
    (956, "2026-10-16"), (956, "2026-10-22"),
    (253, None), (283, None), (502, "2026-10-09"),
    (55, "2026-10-03"), (92, None), (235, None),
    (219, None), (288, "2026-10-04"), (523, "2026-10-25"),
    (130, "2026-10-05"), (194, None),
]
CHAVES_DIA = (
    "data", "status", "status_rotulo", "status_dia",
    "veredito_dia", "palavra_dia", "inconsistente",
    "turno_aberto", "dia_pago_lavrada", "rubricas_dia",
    "resolver_dia_oferecido",
)

def pegar(obj, chaves):
    return {k: obj[k] for k in chaves if k in obj}

def dia_da_lista(dias, data):
    return [
        pegar(x, CHAVES_DIA)
        for x in dias if str(x.get("data")) == str(data)
    ]

def tentar(fn):
    try:
        with transaction.atomic():
            return fn()
    except Exception as exc:
        # Não imprime texto de exceção que possa conter dados pessoais.
        return {"estado": "[nao verificado]", "erro": type(exc).__name__}

class CelulasHTML(HTMLParser):
    def __init__(self):
        super().__init__()
        self.coluna = None
        self.textos = {}
    def handle_starttag(self, tag, attrs):
        if tag == "td":
            coluna = dict(attrs).get("data-col")
            self.coluna = coluna if coluna in ("status", "pront") else None
    def handle_data(self, texto):
        if self.coluna:
            self.textos.setdefault(self.coluna, []).append(texto)
    def handle_endtag(self, tag):
        if tag == "td":
            self.coluna = None
    def resultado(self):
        return {k: " ".join(" ".join(v).split())
                for k, v in self.textos.items()}

@lru_cache(None)
def export_da_empresa(empresa_id, mes):
    empresa = Colaborador.objects.select_related("empresa").filter(
        empresa_id=empresa_id
    ).first().empresa
    return {x["colaborador_id"]: pegar(x, (
        "status", "motivo", "travas_aprovacao", "motivos_espelho",
    )) for x in classificar_export(empresa, mes, ANO)}

@lru_cache(None)
def fechamento_tela(cid, mes):
    c = Colaborador.objects.select_related("empresa").get(pk=cid)
    qs = universo_da_competencia(mes, ANO, c.empresa_id).filter(pk=cid)
    fechs = {f.colaborador_id: f for f in FechamentoMensal.objects.filter(
        colaborador_id=cid, mes=mes, ano=ANO,
    ).select_related("colaborador__empresa")}
    linhas, _ = montar_resumos(qs, fechs, mes, ANO,
                              empresa_id=c.empresa_id)
    saida = []
    for r in linhas:
        html = render_to_string("ponto/_fech_linha.html", {
            "r": r, "empresa_id": c.empresa_id, "periodo_fechado": None,
        })
        parser = CelulasHTML()
        parser.feed(html)
        f = r["fechamento"]
        saida.append({
            "template_real": parser.resultado(),
            "folha": r["folha"], "travas": r["travas"],
            "pront": r["pront"],
            "status_administrativo": f.status if f else None,
            "juiz_prontidao": prontidao(f) if f else None,
        })
    return saida

def retrato_tela(c, mes):
    r = RetratoFolha.objects.filter(
        empresa_id=c.empresa_id, mes=mes, ano=ANO,
    ).first()
    linha = linha_do_colab(r, c.pk)
    return {
        "lavrado_em": timezone.localtime(r.lavrado_em) if r else None,
        "linha": pegar(linha, (
            "colab", "status", "motivo", "dias", "datas",
            "dono", "tipo", "saldo",
        )) if linha else None,
        "led": pegar(desenho_do_led(linha), ("cor", "rotulo"))
               if linha else None,
        "frescor": [
            x for x in frescor_dos_retratos(agora=AGORA)
            if x["empresa"] == c.empresa_id
            and x["mes"] == mes and x["ano"] == ANO
        ],
    }

def diario(c, data):
    mes = data.month
    params = {"modo": "mes", "mes": str(mes), "ano": str(ANO)}
    cal = contexto_calendario(c, params)
    inicio = dt.date(ANO, mes, 1)
    fim = dt.date(ANO, mes, calendar.monthrange(ANO, mes)[1])
    juiz_mes = status_dos_dias_lavrados(
        [(c, data)], agora=AGORA,
        carga=CargaDaJanela([c], inicio, fim),
    )[(c.pk, data)]

    ini_comp, fim_comp = janela_fechamento(mes, ANO, c.empresa)
    esp = espelho_do_colab(
        c, HOJE, ini_comp, fim_comp,
        ano=ANO, modo_gestor=True, agora=AGORA,
    )
    montar_semanas(esp["dias"], HOJE, colaborador=c, agora=AGORA)

    app = {"estado": "sem usuario associado"}
    if c.usuario_id:
        req = APIRequestFactory().get("/api/espelho/v2/", {
            "mes": mes, "ano": ANO,
        })
        force_authenticate(req, user=c.usuario)
        resposta = api_espelho_v2(req)
        app = {
            "http": resposta.status_code,
            "dia": dia_da_lista(resposta.data.get("dias", []), data),
        }

    return {
        "parametros_calendario": params,
        "calendario_real": dia_da_lista(cal["dias"], data),
        "espelho_real": dia_da_lista(esp["dias"], data),
        "api_real": app,
        "juiz_D4_janela_mes": juiz_mes,
        "juiz_D4_unitario": status_do_dia_lavrado(c, data, agora=AGORA),
    }

assert connection.vendor == "postgresql"
with connection.cursor() as cursor:
    cursor.execute("SHOW transaction_read_only")
    assert cursor.fetchone()[0] == "on"

with patch("django.utils.timezone.now", return_value=AGORA):
    for cid, texto_data in CASOS:
        c = Colaborador.objects.select_related(
            "empresa", "usuario"
        ).get(pk=cid)
        data = dt.date.fromisoformat(texto_data) if texto_data else None
        registro = {
            "colaborador_id": cid, "dia": texto_data,
            "relogio_referencia": timezone.localtime(AGORA),
            "diario": tentar(lambda: diario(c, data)) if data else {
                "estado": "[nao verificado]", "motivo": "dia nao informado"
            },
            "competencias": {},
        }
        for mes in (10, 11):
            registro["competencias"][str(mes)] = {
                "juiz_export": tentar(
                    lambda: export_da_empresa(c.empresa_id, mes).get(cid)
                ),
                "fechamento_tela_real": tentar(
                    lambda: fechamento_tela(cid, mes)
                ),
                "retrato_tela_real": tentar(lambda: retrato_tela(c, mes)),
            }
        print(json.dumps(registro, default=str, ensure_ascii=False))
PY
```

O script não reproduz permissões de um supervisor identificado, filtros desconhecidos, HTML do app compilado ou a requisição histórica. Essas partes da reprodução continuam **[nao verificado]**.

**G2 — dia sem porta ou zero hora**

Quem responde **alcance** é `censo_alcance`, perguntando a `porta_alcancavel_do_dia`. Seu universo são **dias que retêm competências abertas**, não todas as reclamações nem todos os dias com zero. (`app/chamados/services/vigia_fio.py:541`; `app/chamados/services/vigia_fio.py:576`; `app/chamados/services/vigia_fio.py:634`)

| Classe do vigia | Condição |
|---|---|
| `a` | Chamado/âncora vivo fora da fila, formando véu. (`app/chamados/services/vigia_fio.py:663`) |
| `b` | Pergunta viva respondida sem texto. (`app/chamados/services/vigia_fio.py:672`) |
| `c` | Respondida com destino terminal. (`app/chamados/services/vigia_fio.py:674`) |
| `d` | Colaborador na Cadastro × Realidade, mas dia fora da lista. (`app/chamados/services/vigia_fio.py:676`) |
| `e` | Detalhe nomeado: sem célula, sem pergunta viva, âncora terminal/ausente, fora do app etc. (`app/chamados/services/vigia_fio.py:666`; `app/chamados/services/vigia_fio.py:679`) |

Antes dessas classes, o vigia separa porta alcançável, hoje/futuro e os quatro baldes com responsável: decisão do dia, ausência aguardando decisão, escala que não descreve o dia e fio do admin. (`app/chamados/services/vigia_fio.py:641`; `app/chamados/services/vigia_fio.py:647`; `app/chamados/services/vigia_fio.py:651`)

**L-099 responde em `e6_oraculo.dono_da_divergencia`**: cadastro sem sustentação vem primeiro; batida solta ou conjunto ímpar → BATIDA; fase deslocada/CxR → CADASTRO; resíduo → ESTRUTURA. O carregador usa batidas da jornada com margens e turnos; `batidas_qtd` civil do anexo não substitui esse insumo. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:128`; `app/ponto/management/commands/e6_oraculo.py:168`)

| Caso | Classe do vigia por dia | Dono L-099 provável e cobertura |
|---|---|---|
| col253 **ID407**, 21/10 | Furo/zero no dado; `a–e` **[nao verificado]** | BATIDA é hipótese pela batida física única; cadastro tem precedência. O261/JUIZ-3 somente se seus predicados forem confirmados. **[nao verificado]** (`app/ponto/management/commands/e6_oraculo.py:98`) |
| col239, 07/10 | Jornada parcial/QRF; ausência de porta **[nao verificado]** | BATIDA/CADASTRO/ESTRUTURA ainda não distinguíveis. **[nao verificado]** (`app/ponto/management/commands/e6_oraculo.py:104`) |
| col909, 16/10 | Furo com horas e chamado terminal/vivo no dado; `a–e` **[nao verificado]** | BATIDA é hipótese; JUIZ-3 é cobertura condicional para turno aberto. **[nao verificado]** (`app/docs/RELATO.md:63`) |
| col292, 21/09 | Sem porta relatada; classe atual **[nao verificado]** | Caso histórico explicitamente citado em O261; dono atual **[nao verificado]**. (`app/docs/BACKLOG.md:782`) |
| col207, SD | Falta mapear dia/pergunta/porta; classe **[nao verificado]** | O261 é família candidata; dono **[nao verificado]**. (`app/docs/BACKLOG.md:859`) |
| col332, SD | Chamados de batida/turno; alcance **[nao verificado]** | JUIZ-3, FIO MUDO ou O168 dependem de fatos distintos ainda ausentes. **[nao verificado]** (`app/docs/RELATO.md:63`; `app/docs/RELATO.md:89`; `app/ponto/supra_juiz.py:539`) |
| col446, SD | Canal inexistente é condicional no relatório; classe **[nao verificado]** | O261/FIO MUDO somente após verificar pergunta e âncora; dono **[nao verificado]**. (`app/chamados/services/vigia_fio.py:684`) |
| col1023, SD | Chamado `sem_previsao_escala`; dia/alcance **[nao verificado]** | CADASTRO é hipótese; F3-BECO-1 exige a classe específica de folga não definida. **[nao verificado]** (`app/docs/BACKLOG.md:1762`) |

A cobertura existente não é intercambiável:

| Obra | Classe que cobre |
|---|---|
| F3-BECO-1 | Beco de cadastro/folga não definida; não todo zero hora. (`app/docs/BACKLOG.md:1762`) |
| O261-E | Pergunta viva sob âncora terminal/passivo. (`app/docs/BACKLOG.md:859`) |
| CONTRATO-20 / O169 | Cluster com ≥2 órfãs em rajada ≤15 min e marco apagado. (`app/docs/BACKLOG.md:1380`; `app/ponto/services/cluster_espurio.py:15`) |
| CONTRATO-20 / JUIZ-3 | Emissão de turno aberto/24h/órfão14h por recompute/outbox, conforme opção documentada. (`app/docs/RELATO.md:63`) |
| CONTRATO-20 / FIO MUDO | Batida ausente sem pergunta viva; emissão na abertura, conforme opção documentada. (`app/docs/RELATO.md:89`; `app/docs/RELATO.md:39`) |
| CONTRATO-20 / O168 | Guarda de cobrança sem furo: conversa respondida impede retratação automática. (`app/docs/BACKLOG.md:1379`; `app/ponto/supra_juiz.py:539`) |

Nenhum caso fornecido comprova a assinatura O169, FIO MUDO ou violação de O168. **[nao verificado]**

**G3 — transnoite e troca**

| Decisão | Código responsável |
|---|---|
| Vínculo do dia, inclusive após troca | `vinculo_do_dia`; a geradora/célula e a vigência alimentam a resposta. (`app/docs/JUIZES.md:98`; `app/escala/servico_jornada.py:38`) |
| Marcos | `EscalaColaborador.marcos_do_dia`, lendo DNA congelado quando existe. (`app/escala/models.py:1098`; `app/escala/models.py:1124`) |
| Encadeamento após meia-noite | `marcos_turno`, avançando a data quando o próximo marco é ≤ anterior. (`app/escala/servico_jornada.py:61`; `app/escala/servico_jornada.py:80`) |
| Dia da jornada e pareamento | `_data_do_turno` examina a véspera; `parear_turnos` recebe DNA e papel da ata. (`app/ponto/turnos.py:246`; `app/ponto/turnos.py:1190`; `app/ponto/motor_calculo_v2.py:503`) |
| Troca dentro da competência | `apurar_por_fatia`, com vínculo por dia. (`app/ponto/services/fechamento.py:132`; `app/ponto/services/fechamento.py:188`) |
| Jornada → DiaPago | `resultado.dia_da_jornada` segue para `lavrar`; `_dia_de` usa esse mapa. (`app/ponto/services/fechamento.py:1271`; `app/ponto/services/dia_pago.py:109`; `app/ponto/services/dia_pago.py:135`) |
| Ato de troca de vínculo | `julgar_vinculo`/`executar_vinculo`; o chat usa a mesma porta. (`app/colaboradores/services/vinculo.py:50`; `app/colaboradores/services/vinculo.py:167`; `app/colaboradores/services/troca_pelo_chat.py:64`) |

Uma batida física na data civil não prova par ausente no transnoite: a atribuição é por jornada. (`app/ponto/turnos.py:246`)

**H-02** aparece como referência à triagem de col129, sem definição suficiente de escopo para declarar cobertura de toda a classe. (`app/docs/RELATO.md:24`; `app/docs/BACKLOG.md:1742`) A definição de **H-04** não foi localizada nesta cópia: **[nao verificado]**.

Plantão efetivamente realizado, ciclo aprovado, vigência da troca e autorização de HE dependem da decisão/documentação do DP; os anexos não os demonstram. **[nao verificado]**

**G4 — ato administrativo sem efeito**

Há quatro unidades candidatas: col836 13–14/10, col39 15/10 e col250 SD. A identificação do ato e do elo quebrado não está nos anexos. Chamado `resolvido/fechado` não comprova resposta validada e materializada. **[nao verificado]**

| Ato/porta | Escritor e caminho esperado | Leitor/efeito |
|---|---|---|
| Responder pergunta | `responder_pergunta_disputa_core` escreve resposta e `respondida_em`. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/disputa_emissao.py:1282`) | Validação/materialização leem a pergunta. (`app/chamados/services/validacao.py:422`) |
| Admin validar | `acao_validar_pergunta` → `validar_pergunta`; `_carimbar_validacao` escreve a validação. (`app/chamados/services/acoes_disputa.py:13`; `app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:343`) | Materializa a pergunta real e agenda recálculo pela data do fato. (`app/cham

[truncado em 20000 de 28756 caracteres]

## 05/11 16:38 · sessao 0bddf690b83d · PD-MODO-COMUM-PASSE · censo · origem=Code

## 1. QUEM ESCREVE E QUEM LÊ

**O juiz atual responde pela flag carregada, sem receber D e sem consultar a auditoria.** Portanto, sua chamada durante uma consulta histórica continua respondendo pelo cadastro atual. (`app/core/constants.py:13`, `app/core/constants.py:18`.)

**Campos, escritores e trilha:**

| Campo/estado | Quem escreve hoje | Quem lê |
|---|---|---|
| `Colaborador.isento_ponto` | Passe Livre: carrega o colaborador, compara a decisão, exige motivo de dez caracteres quando muda e chama `gravar(..., 'cadastro', ...)`. Repetição sequencial do mesmo valor não grava nova transição. (`app/colaboradores/views.py:156`, `app/colaboradores/views.py:164`, `app/colaboradores/views.py:173`, `app/colaboradores/views.py:176`.) | Juiz central e leitores históricos/atuais enumerados abaixo. (`app/core/constants.py:18`.) |
| Persistência da flag | A porta `gravar` atribui e salva os campos; não acrescenta auditoria. As portas genéricas também permitem salvar campos, objeto inteiro, atualização em lote e criação. Isso demonstra capacidade de escrita, sem demonstrar outro ato específico de Passe Livre em produção. (`app/colaboradores/services/colaborador.py:34`, `app/colaboradores/services/colaborador.py:55`, `app/colaboradores/services/colaborador.py:65`, `app/colaboradores/services/colaborador.py:73`, `app/colaboradores/services/colaborador.py:80`, `app/colaboradores/services/colaborador.py:91`.) | O admin de Colaborador herda somente leitura. (`app/colaboradores/admin.py:47`.) |
| Flag inicial | O modelo tem default `False`; a migration 0043 semeou `True` por substring de cargo, sem registrar Passe Livre na auditoria. O juiz executável atual não usa cargo. (`app/colaboradores/models.py:294`, `app/colaboradores/migrations/0043_isento_ponto.py:6`, `app/colaboradores/migrations/0043_isento_ponto.py:13`, `app/core/constants.py:16`.) | Consultas posteriores recebem esse booleano como qualquer outro valor da flag. (`app/core/constants.py:18`.) |
| Identificação da transição | Passe Livre grava `acao='editar'`, `modelo='Colaborador'`, `objeto_id=colab.pk`; descrição `Passe livre (isencao Art.62): {antes} -> {novo_valor}. Motivo: ...`. (`app/colaboradores/views.py:183`.) | A tela de auditoria consulta, filtra e limita a listagem a 500 registros; não julga vigência em D. (`app/core/views.py:705`, `app/core/views.py:713`, `app/core/views.py:726`.) |
| `valor_antes`, `valor_depois`, motivo | O escritor informa `{'isento_ponto': antes}` e `{'isento_ponto': novo_valor, 'motivo': motivo}`. `registrar_log` encaminha a `core.observ.evento`, que persiste `LogAuditoria`. (`app/colaboradores/views.py:185`, `app/colaboradores/views.py:186`, `app/core/auditoria.py:22`, `app/core/observ.py:134`.) | Os testes da trilha conferem os JSONs e ordenam transições por `criado_em`. O próprio código documenta linhas antigas cuja transição estava somente na descrição. (`app/colaboradores/tests/test_passe_livre_trilha.py:71`, `app/colaboradores/tests/test_passe_livre_trilha.py:100`, `app/colaboradores/views.py:178`.) |
| Usuário, IP, ator e instante | `registrar_log` fornece usuário/IP; `evento` completa o contexto. `criado_em` é automático. O modelo não contém início/fim separado de vigência da isenção. (`app/core/auditoria.py:18`, `app/core/auditoria.py:24`, `app/core/observ.py:108`, `app/core/models.py:295`, `app/core/models.py:307`.) | Auditoria ordena por instante descendente; o modelo não declara desempate por PK nessa ordenação. (`app/core/models.py:312`.) |
| `data_admissao` | É campo permitido pela operação cadastral. (`app/colaboradores/services/colaborador.py:34`.) | Recorta a leitura de células e a retenção; o corte de quarentena também a usa como início quando falta trilha. (`app/escala/services/leitor_celula.py:351`, `app/folha/export.py:1180`, `app/docs/CORTES.md:352`.) |

**A trilha permite identificar transições registradas, mas sua completude não é garantida:** a flag é salva antes da auditoria; falhar a auditoria não desfaz a decisão, e o escritor da trilha também trabalha com captura de falha. (`app/colaboradores/views.py:176`, `app/colaboradores/views.py:187`, `app/core/auditoria.py:25`, `app/core/observ.py:105`.)

**Leitores que perguntam por D ou por uma janela histórica:**

| Leitor | O que faz hoje |
|---|---|
| `eventos_do_fechamento` | Retorna lista vazia de eventos pela isenção atual, inclusive sobre snapshot descrito como anterior à isenção. (`app/folha/export.py:402`, `app/folha/export.py:407`.) |
| `prontidao` — sítio de partida `:1008` | Retorna `cego=True`, `previsto=0`, `motivo='isento'` antes de examinar a previsão da competência. (`app/folha/export.py:1008`, `app/folha/export.py:1011`.) |
| `motivos_retencao_celula` — sítio de partida `:1173` | Dispensa a janela inteira antes do recorte e do laço por dia; preserva a forma `([], [])` quando `com_dias=True`. (`app/folha/export.py:1173`, `app/folha/export.py:1174`, `app/folha/export.py:1178`.) |
| Prévia | Exclui o colaborador atualmente isento antes de examinar as células da competência. (`app/folha/previa.py:42`, `app/folha/previa.py:74`.) |
| Builder da grade | `_marcar_isencao` pergunta uma vez pela pessoa e aplica a resposta a todos os dias recebidos. (`app/escala/utils.py:904`, `app/escala/utils.py:909`.) |
| Leitor da célula | `grade_da_celula` fixa a isenção antes do laço, aplica o overlay por dia e devolve uma flag única no topo da grade. (`app/escala/services/leitor_celula.py:349`, `app/escala/services/leitor_celula.py:412`, `app/escala/services/leitor_celula.py:416`.) |
| Cartório: impressão | Lê o juiz uma vez e entrega a resposta às impressões dos dois ramos. (`app/ponto/services/cartorio.py:1141`, `app/ponto/services/cartorio.py:1198`, `app/ponto/services/cartorio.py:1354`.) |
| Cartório: julgamento/status | Passa diretamente `c.isento_ponto` ao julgamento e aos fatos do status de D. (`app/ponto/services/cartorio.py:890`, `app/ponto/services/cartorio.py:1229`, `app/ponto/services/cartorio.py:1246`, `app/ponto/services/cartorio.py:1419`, `app/ponto/services/cartorio.py:1806`.) |
| Espelho e fechamento | O espelho fixa `_isento` fora do laço e o usa para afastar inconsistência; o fechamento retorna lista de faltas vazia antes de examinar os dias. (`app/ponto/services/espelho.py:600`, `app/ponto/services/espelho.py:669`, `app/ponto/services/fechamento.py:357`.) |
| Cartão/resumo | `folha_manda` entrega a isenção atual a `datas_em_aberto_de` para o período solicitado. (`app/relatorios/cartao_pela_celula.py:418`.) |
| Furos | Exclui toda `[ini,fim]` pela isenção atual e repete a exclusão no quadro da janela. (`app/escala/services/furos_diarios.py:33`, `app/escala/services/furos_diarios.py:298`.) |
| Diagnósticos de escala | A flag direta exclui diagnóstico da régua, turnos longos e entradas fora do início; propositor e previsto cego também filtram pela flag atual. (`app/escala/regua_defesa.py:136`, `app/escala/regua_defesa.py:355`, `app/escala/regua_defesa.py:513`, `app/escala/services/propositor.py:456`, `app/escala/services/previsto_cego.py:143`.) |
| Termômetro/esmeril | Excluem isentos atuais de diagnósticos sobre janelas anteriores. (`app/escala/management/commands/termometro_regua.py:34`, `app/ponto/services/esmeril_espelho.py:216`.) |
| Auditoria e emissão retroativa | Passam a flag atual diretamente a `classificar_dia`. (`app/ponto/management/commands/emitir_furo_retroativo.py:106`, `app/ponto/management/commands/supra_juiz.py:228`.) |
| Outros comandos históricos | Mapa de divergência, proposta de reorganização e vigia do motor filtram o universo pela flag atual. Reconciliar grade usa o alias sem D `isento_art62`. (`app/ponto/management/commands/mapa_divergencia.py:65`, `app/ponto/management/commands/mapa_divergencia.py:99`, `app/ponto/management/commands/propor_reorganizacao.py:35`, `app/ponto/management/commands/vigiar_motor_autoridade.py:46`, `app/ponto/management/commands/reconciliar_grade.py:230`, `app/ponto/marcos.py:116`.) |
| Detectores/alertas | Ausências pergunta hoje/ontem após um gate único; alertas avançados examina ontem; alertas de turno salta inclusive a resolução de turno fechado. (`app/ponto/management/commands/detectar_ausencias.py:71`, `app/ponto/management/commands/detectar_ausencias.py:104`, `app/ponto/management/commands/processar_alertas_avancados.py:32`, `app/ponto/management/commands/processar_alertas_avancados.py:34`, `app/ponto/management/commands/processar_alertas_turno.py:85`, `app/ponto/management/commands/processar_alertas_turno.py:88`.) |
| Vigia de hora | Filtra pela isenção atual células de hoje/ontem que serão julgadas pela própria data. (`app/ponto/services/vigia_de_hora.py:109`, `app/ponto/services/vigia_de_hora.py:117`, `app/ponto/services/vigia_de_hora.py:131`.) |
| Chamados/contraprova | A reemissão de chamado de D e a auditoria do piso de perguntas consultam a flag atual. (`app/chamados/reconciliador.py:819`, `app/chamados/management/commands/auditar_invariantes_chamados.py:400`.) |
| Inteligência | Diagnóstico de desvio usa dias anteriores, mas seleciona não isentos atuais. (`app/inteligencia/calculadores.py:320`, `app/inteligencia/calculadores.py:325`, `app/inteligencia/calculadores.py:332`.) |
| Censo auxiliar | Acrescenta `ART62` pela flag atual em relatório de competência; é rótulo diagnóstico. (`bin/censo_folha.py:14`, `bin/censo_folha.py:34`.) |

**Leitores cuja pergunta é hoje ou o ato atual:**

| Leitor | Distinção |
|---|---|
| Tela Passe Livre | Lista isentos atuais e candidatos ativos não isentos. (`app/colaboradores/views.py:204`, `app/colaboradores/views.py:206`.) |
| Faltas de hoje e deslogados | Filtram quem responde agora; deslogados usa histórico para medir a corrida, mas declara a pergunta como HOJE. (`app/ponto/services/faltas_de_hoje.py:29`, `app/ponto/services/faltas_de_hoje.py:33`, `app/ponto/services/deslogados.py:45`, `app/ponto/services/deslogados.py:157`, `app/ponto/services/deslogados.py:163`.) |
| Mensageria cadastral/adesão | Filtra pendências cadastrais atuais e adesão corrente; não percorre vigências pessoais da flag em cada D. (`app/api/views_mensageria.py:1221`, `app/api/views_mensageria.py:1236`, `app/api/views_mensageria.py:1273`, `app/chamados/services/adesao.py:56`, `app/chamados/services/adesao.py:64`.) |
| Escala automática | Seleciona propostas para vínculos atuais; esse filtro isolado não demonstra julgamento histórico de isenção. (`app/escala/services/escala_auto.py:171`, `app/escala/services/escala_auto.py:182`.) |
| Detector de intervalo | Reconcilia o acervo antes do gate de isento; a emissão desse detector está aposentada. (`app/ponto/management/commands/detectar_intervalo_ausente.py:127`, `app/ponto/management/commands/detectar_intervalo_ausente.py:147`, `app/ponto/management/commands/detectar_intervalo_ausente.py:149`.) |
| Situação no cartório | Os gates de desligado são intencionais para emissão agora; não afirmam se havia trabalho em D. (`app/ponto/services/cartorio.py:1651`, `app/ponto/services/cartorio.py:1697`, `app/ponto/services/cartorio.py:1762`.) |

**Funções e campos que propagam a decisão:**

| Escritor/função | Leitores e efeito |
|---|---|
| `aplicar_isencao_no_dia` | Modifica a representação em memória: crédito sintético com `missing=False`, `status='ok'`, `delta=0`, `isencao=True`; conserva hora real existente. `_overlay_isencao` delega à mesma função. (`app/escala/utils.py:850`, `app/escala/utils.py:858`, `app/escala/services/leitor_celula.py:212`.) |
| `dia.isencao` e células marcadas | Espelho transporta os marcadores; PDF imprime `(i)` e a observação Art.62. O rótulo do dia exige trabalho com forma conhecida. (`app/escala/utils.py:878`, `app/escala/utils.py:888`, `app/ponto/services/espelho.py:856`, `app/ponto/services/espelho.py:861`, `app/relatorios/pdf_espelho.py:889`, `app/relatorios/pdf_espelho.py:925`.) |
| Ata/lâmpadas/status | Cartório propaga o carimbo de isenção para a lâmpada; a porta da célula chama `status_da_lavra`. Este preserva a precedência de falta, ausência, indefinida e folga antes da isenção. (`app/ponto/services/cartorio.py:556`, `app/ponto/portas/celula.py:701`, `app/ponto/status_dia.py:139`, `app/ponto/status_dia.py:150`.) |
| `classificar_dia` | Recebendo `isento_ponto=True`, retorna lista vazia de acusações. (`app/ponto/supra_juiz.py:220`.) |
| `impressao_insumos` | Inclui `isento:1` somente com versão de regra ≥4; o horizonte é de regra, não início individual do passe. (`app/ponto/services/cartorio.py:514`, `app/ponto/services/cartorio.py:520`.) |
| Flag agregada da grade | `classificar_causa` responde `sem_furo`; declaração de marcos retorna antes do teste de ata ausente; bordas do realizado aplica a flag a todos os dias da grade. (`app/folha/causas.py:35`, `app/chamados/services/declaracao_texto.py:83`, `app/ponto/services/bordas_realizado.py:71`.) |
| Fechamento materializado | Grava motivos do espelho, causa e previsão; os leitores posteriores recebem a projeção já influenciada pelos gates anteriores. (`app/ponto/services/fechamento.py:1051`, `app/ponto/services/fechamento.py:1064`, `app/ponto/services/fechamento.py:1067`.) |
| Eventos da folha | São consumidos pelo TXT, conferência de rubricas e totais do cartão. (`app/folha/export.py:711`, `app/folha/export.py:976`, `app/relatorios/cartao_pela_celula.py:90`.) |
| Retenção/prontidão | Retenção alimenta prévia, fora da folha e vigia de pausa; prontidão alimenta lista de fechamentos e simulação. (`app/folha/previa.py:170`, `app/folha/fora_da_folha.py:98`, `app/folha/services/vigia_pausa.py:125`, `app/ponto/services/fechamento_lista.py:82`, `app/folha/management/commands/simular_folha.py:87`.) |

A grade também chega ao calendário, espelho, relatório de furos, materialização e emissão de disputas; `folha_manda` propaga o resumo para calendário, espelho e PDF. (`app/colaboradores/services/calendario.py:334`, `app/ponto/services/espelho.py:519`, `app/relatorios/furos.py:132`, `app/relatorios/furos.py:194`, `app/chamados/services/materializacao.py:233`, `app/chamados/services/materializacao.py:282`, `app/chamados/services/disputa_emissao.py:66`, `app/colaboradores/services/calendario.py:583`, `app/ponto/services/espelho.py:1284`, `app/relatorios/pdf_espelho.py:628`.)

O motor é documentado como não conhecendo o Passe Livre; há filtros externos de seus resultados. Completude absoluta dos caminhos indiretos não enumerados: **[nao verificado]**. (`app/ponto/services/dia_decidido.py:264`, `app/relatorios/cartao_pela_celula.py:413`.)

## 2. JUIZ OU LEI QUE JÁ RESPONDE

| Pergunta | Autoridade existente e limite |
|---|---|
| “É isento agora?” | `core.constants.isento_ponto`: booleano atual, sem D. (`app/core/constants.py:13`, `app/core/constants.py:18`.) |
| “Era isento em D?” | O aval fornecido manda ler a vigência na trilha, mantendo esse juiz; arquivo local do complemento e implementação dessa resposta histórica: **[nao verificado]**. O juiz executável conferido continua sem data. (`app/core/constants.py:13`.) |
| “Sem trilha, desde quando?” | **QUARENTENA-ISENTO-DESDE-QUANDO**, literalmente em `CORTES.md:352`: desde a admissão, com rótulo de flag sem trilha; troca abre/fecha janela; item sem dia entra somente em janela aberta. A linha está marcada em construção. (`app/docs/CORTES.md:352`.) |
| “Quarentena altera cálculo?” | O corte anterior distingue cobrança de cálculo/espelho/TXT e proíbe apagar, fechar ou silenciar como efeito da quarentena. Não constitui, sozinho, resposta histórica implementada para a folha. (`app/docs/CORTES.md:348`.) |
| “Qual vínculo e quais marcos respondem por D?” | **Os dois**: `vinculo_do_dia` responde o vínculo; `EscalaColaborador.marcos_do_dia` responde os marcos. `vinculo_cobre_a_data` responde cobertura, outra pergunta. (`app/core/juizes.py:1285`, `app/core/juizes.py:201`.) |
| “A saída pertence a qual dia?” | `turnos_do_colab` responde pertença da batida; `marcos_turno` encadeia os marcos a partir de D. Nenhum define a vigência pessoal do passe. (`app/core/juizes.py:196`, `app/escala/servico_jornada.py:61`, `app/escala/servico_jornada.py:81`.) |
| “Qual competência?” | `janela_fechamento`/`janela_atual`, usando o corte da empresa; com corte 21, a janela é 21 do mês anterior até 20. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`.) |
| “Entra no TXT, qual prontidão e está entregue?” | `classificar_export`, `prontidao` e `competencia_entregue` são perguntas distintas já registradas. (`app/core/juizes.py:298`, `app/core/juizes.py:300`, `app/core/juizes.py:312`.) |
| “Qual status fica lavrado?” | `status_da_lavra` julga a palavra com os fatos fornecidos; não procura a vigência da flag. (`app/ponto/status_dia.py:113`, `app/ponto/status_dia.py:150`.) |

`JUIZES.md` é consulta; declara que o registro vivo prevalece. Não foi demonstrado um segundo juiz implementado para a isenção histórica nem implementação de `inicio_da_isencao`/`colab_em_quarentena` nesta cópia: **[nao verificado]**. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`.)

A implementação identificada de silêncio ainda seleciona isentos atuais e chamados vivos sem comparar o dia do fato com início individual; chama a porta de silêncio com prazo. Isso é diferente do corte de quarentena por período. (`app/chamados/services/silencio_isentos.py:24`, `app/chamados/services/silencio_isentos.py:27`, `app/chamados/services/silencio_isentos.py:39`, `app/chamados/services/silencio_isentos.py:43`, `app/docs/CORTES.md:348`.)

L-007 exige escritor/trilha; L-110 exige referência na regra; L-124 trata horizonte da obra, sem fornecer vigência individual. A proibição específica de tocar exportada está no aval fornecido, sem arquivo local conferível: **[nao verificado]**. (`app/docs/LEIS.md:65`, `CLAUDE.md:45`, `app/docs/LEIS.md:145`.)

## 3. CONTRA-EXEMPLOS PELA REGRA DO AVAL

O raspado informa col427 com remoção em 05/10 às 11:32, 23 dias até 05/10 em `nunca_bateu/irregular`, duas remoções no conjunto da trilha e 16 isentos atuais. Não fornece a sequência integral por pessoa nem as atas desses 23 dias; classificar todos eles como divergência comprovada seria **[nao verificado]**.

| Leitura óbvia que erra | Contra-exemplo e limite |
|---|---|
| “Retirou o passe; todo passado volta a ser cobrado.” | Um D comprovadamente dentro do passe passa pelos leitores com flag atual falsa e perde a dispensa. A flag atual não contém a data da retirada. (`app/core/constants.py:18`, `app/escala/utils.py:904`, `app/folha/export.py:1174`.) |
| “Ligou hoje; a competência antiga toda é isenta.” | Uma entrada posterior a D faz os gates atuais excluírem dias anteriores e até eventos do snapshot pré-isencão. (`app/folha/export.py:402`, `app/folha/export.py:407`, `app/folha/previa.py:74`.) |
| “Remover e religar no mesmo dia é uma decisão só.” | São dois atos distintos quando registrados. O booleano final elimina essa diferença; o juiz não recebe instante. Inclusividade e tratamento do intervalo intradia pelo aval: **[nao verificado]**. (`app/colaboradores/views.py:173`, `app/colaboradores/views.py:183`, `app/core/models.py:307`, `app/core/constants.py:13`.) |
| “Duas remoções provam dois períodos completos.” | Duas linhas `True→False` não demonstram, sozinhas, duas entradas: pode existir início sem log pela semente, falha de auditoria ou concorrência. (`app/colaboradores/migrations/0043_isento_ponto.py:13`, `app/core/observ.py:105`, `app/colaboradores/views.py:156`, `app/colaboradores/views.py:183`.) |
| “Sem JSON, sem trilha.” | A transição antiga pode estar na descrição. Ler somente `valor_depois` perde esse formato. (`app/colaboradores/views.py:178`.) |
| “Sem trilha, não há passe anterior.” | Para isento sem trilha, o corte manda desde a admissão. Isso não prova que qualquer não isento atual jamais tenha sido isento. (`app/docs/CORTES.md:352`; última inferência: **[nao verificado]**.) |
| “A saída após a remoção torna o turno anterior não isento.” | Um turno iniciado em D pode terminar em D+1; seus marcos continuam ancorados em D. Política de divisão d

[truncado em 20000 de 30956 caracteres]

## 05/11 16:35 · sessao 493adf4cd9cc · - · - · origem=Ronald (tmux)

G3 — cadeia provada no código:

| Pergunta | Autoridade/cadeia | Prova |
|---|---|---|
| Qual vínculo responde pelo dia, inclusive após troca? | `escala_vigente` carrega e chama `escala.alimentacao.vinculo_do_dia`; célula/geradora e vigência alimentam o juiz. | `app/escala/servico_jornada.py:15`, `:38`, `:55` |
| Quais marcos? | `EscalaColaborador.marcos_do_dia` lê `CelulaDia.dna.marcos`; sem marcos congelados, lê template. | `app/escala/models.py:1098`, `:1124`, `:1133` |
| Qual dia é trabalho? | `eh_dia_trabalho` lê `cel.trabalha`; sem célula usa aritmética do vínculo. | `app/escala/models.py:1069`, `:1094` |
| Como cruza meia-noite? | `marcos_turno` ancora no dia e avança um dia quando marco seguinte `<=` anterior. | `app/escala/servico_jornada.py:61`, `:80` |
| A qual jornada pertence a batida? | `_data_do_turno` examina envelope da véspera; `_mk` usa essa resposta também para saída órfã. | `app/ponto/turnos.py:246`, `:283`, `:310` |
| Quem pareia turno? | `parear_turnos`, chamado pelo motor através de `turnos_via_autoridade` com DNA e papel da ata. | `app/ponto/turnos.py:1190`; `app/ponto/motor_calculo_v2.py:453`, `:472`, `:497`, `:503` |
| Como turno chega ao DiaPago? | `_periodos_via_autoridade` preserva jornada; `lavrar` usa `dia_da_jornada` em `_dia_de` e agrega rubricas. | `app/ponto/motor_calculo_v2.py:509`, `:530`; `app/ponto/services/dia_pago.py:109`, `:135`, `:173` |
| Troca no meio da competência? | `apurar_por_fatia` resolve vínculo por dia e fatias; lavra recebe `resultado.dia_da_jornada`. | `app/ponto/services/fechamento.py:132`, `:188`, `:203`, `:1271` |
| Porta de troca de vínculo? | `julgar_vinculo` julga, `executar_vinculo` escreve; chat chama mesma porta da tela. | `app/colaboradores/services/vinculo.py:50`, `:167`; `app/colaboradores/services/troca_pelo_chat.py:3`, `:64` |

H-02: encontrei somente referência à “triagem H-02 col129 11/10”; não há definição de escopo comprovada. `app/docs/PROMPTS.md:1861`, `app/docs/RELATO.md:24`, `app/docs/BACKLOG.md:1742`. H-04 e aval BO-[nome]-0511-CENSO não foram localizados nos Markdown desta cópia; significado e cobertura [não verificado].

Casos candidatos G3 pelo relatório fornecido: col29 15/10; col789 s/d; col129 s/d; **col751 com id 923**, s/d; col174 25–26/10; col33 03→04/10; col94 16/10; col163 troca 10↔17/10; col873 21/09 e 26/09; col949 troca de ciclo s/d; col214 mudança manhã/tarde s/d. Datas sem detalhe de batidas/turnos não permitem determinar o defeito concreto [não verificado].

Uma batida física na data civil não prova falta do par transnoite: o código atribui batidas por jornada. `app/ponto/turnos.py:246`, `:2874`. Autorização de plantão, escala real, troca e HE continua dependente dos fatos/documentos do DP no dado fornecido [não verificado].

G5 — separação:

| Subclasse | Casos candidatos do relatório | Prova e limite |
|---|---|---|
| Exibição de rubricas | col297 06/13/20/10 e autorização 27/10; col960 s/d; col235 s/d; col194 atraso s/d; col219 s/d | `rubricas_do_dia` lê campos do DiaPago; `pago_do_dia` soma normais, folga e folga-feriado. `app/ponto/services/dia_pago.py:865`, `:896`, `:899`. A tela específica e defeito desses casos [não verificado]. |
| Decisão DP | col189 21/09 e 19/10; col43 10/10 e 12/10; col222/col256 feriado; col123 troca/reciclagem; col247 QRF 24/10; janta col960 | Dado não contém autorização/documentos; defeito medido [não verificado]. Precedência de feriado/folga vem de `app/ponto/precedencia.py:242`, `:369`. |
| Cadastro temporal / defeito de classe já nomeado | col270 competência 10/11, data desligamento ausente | `classificar_export` exclui qualquer `data_demissao is not None`, sem comparar janela, embora comentário diga “demissão na janela”. `app/folha/export.py:835`, `:840`. H-12/MODO-COMUM já consta em `app/docs/RELATO.md:9`; impacto retroativo específico no col270 [não verificado]. |
| Integridade ao alterar batidas livres | supervisor não identificado | Transição e registros antes/depois ausentes; defeito [não verificado]. Usuário excluiu passe livre fechado. |
| Identificação insuficiente | Weverton reciclagem | Cadastro/período não identificado univocamente no anexo; classe de persistência [não verificado]. |

A gestão de HE já separa pendência lavrada, cadastro e dinheiro do DiaPago; escrita ocorre em `ponto/portas/he.py`, leitura em gestão. `app/ponto/services/gestao_he.py:2`, `:6`, `:9`, `:14`. Isso permite separar reclamação de exibição de autorização e efeito financeiro; não prova comportamento de produção desses casos [não verificado].

G6 — só reprodução, sem cura:

A tela do relato não está identificada [não verificado]. Candidata rastreável: Central `/chamados/painel/`, view `painel_gestao`, que usa `request.GET.get('pagina',1)` e retorna partial em HTMX. `app/chamados/views.py:721`, `:734`, `:742`.

No template, busca envia filtros sem parâmetro `pagina`; partial guarda `data-pagina`, links paginam HTMX; já há restauração de `scrollTop` e, ao filtrar colaborador, guarda/restaura página e rolagem. `app/templates/chamados/painel_gestao.html:127`, `:131`, `:278`, `:648`, `:665`; `app/templates/chamados/partials/_lista_resumo.html:8`, `:12`, `:17`.

Roteiro somente leitura: abrir listagem; ir a página N>1; rolar container `lista-scroller`; registrar URL, filtros, `data-pagina`, `scrollTop`; usar atualização GET da tela; capturar requisição e verificar presença de `pagina`, partial retornado, filtros e rolagem; repetir em resumo/lista e filtro por colaborador/limpar. Presença do defeito e operação concreta do relato [não verificado]. Não executar validação, resposta ou alteração de chamado para reproduzir nesta auditoria.

## 05/11 16:35 · sessao 94d8d9e9b483 · - · - · origem=Ronald (tmux)

G2 — **classe exata e dono por dia não são demonstráveis nos anexos**: faltam PerguntaDisputa, âncora, participação em `qs_na_fila`/app, lista Cadastro x Realidade e timestamps apuráveis. `batidas_qtd` por data não substitui o conjunto da jornada usado pelo oráculo. [não verificado]

- `censo_alcance`: universo = dias que retêm competências abertas; porta = `porta_alcancavel_do_dia`. Fontes em `app/chamados/services/vigia_fio.py:541`, `:576`, `:634`.
- Causas sem porta: **a** chamado vivo com véu; **b** respondida sem resposta; **c** respondida com destino terminal; **d** colaborador na CxR, dia fora; **e** detalhe nomeado, incluindo sem célula/pergunta/âncora ou pergunta fora do app. Definição `app/chamados/services/vigia_fio.py:518`; execução `:660`.
- Antes dessas causas, separa porta acessível, hoje/futuro e quatro situações com responsável: decisão do dia pendente, ausência aberta, folga contestada validada, fio do admin. `app/chamados/services/vigia_fio.py:641`, `:273`.
- L-099 responde em `dono_da_divergencia`: cadastro inválido primeiro; batida fora de turno ou conjunto ímpar → BATIDA; fase deslocada/CxR → CADASTRO; resto → ESTRUTURA. `app/ponto/management/commands/e6_oraculo.py:98`. Carregamento correto em `InsumosDoDono`, `:128`.
- col407+21/10 e col909+16/10 sugerem BATIDA somente pela contagem física; col836+13–14/10 admite hipótese ESTRUTURA condicionada à ausência de defeitos cadastrais; todos esses donos permanecem [não verificado].
- col292+21/09 está explicitamente no complemento O261; o documento descreve a classe histórica, sem provar a classe atual. `app/docs/BACKLOG.md:782`.

Cobertura existente:

| Família | Cobertura comprovada no código/docs |
|---|---|
| Folga não definida, beco de cadastro | F3-BECO-1; não é cobertura genérica de zero horas. `app/docs/BACKLOG.md:1762`, `:1745` |
| Pergunta viva sob âncora terminal/passivo | O261-E-CELULA-20-UM-DONO. `app/docs/BACKLOG.md:859` |
| Cluster | O169; assinatura própria: ≥2 órfãs em rajada ≤15 minutos e marco apagado. `app/docs/BACKLOG.md:1380`; `app/ponto/services/cluster_espurio.py:15` |
| Turno aberto/24h/órfão14h | JUIZ-3; emissão por recompute/outbox é a opção aprovada documentada. `app/docs/RELATO.md:63`, `:39` |
| Batida ausente viva sem pergunta viva | FIO MUDO/JUIZ-4; opção aprovada: evento da abertura de `batida_ausente`. `app/docs/RELATO.md:89`, `:39` |
| Cobrança sem furo com conversa | O168, guarda `pode_executar_csf`; resposta existente impede retratação automática. `app/docs/BACKLOG.md:1379`; `app/ponto/supra_juiz.py:539` |

Nenhum anexo permite atribuir cluster, FIO MUDO ou violação da guarda de conversa a um caso específico. [não verificado]

G4 — cadeia do ato até célula/DiaPago:

1. Resposta entra por `responder_pergunta_disputa_core`; escreve texto e `respondida_em`. `app/chamados/services/disputa_emissao.py:1247`, `:1282`.
2. Ação de tela `acao_validar_pergunta` chama `validar_pergunta`; `_carimbar_validacao` escreve a validação. `app/chamados/services/acoes_disputa.py:13`; `app/chamados/services/validacao.py:127`, `:343`.
3. Validação agenda recálculo pela **data do fato** e chama a materialização real. `app/chamados/services/validacao.py:395`, `:405`, `:422`.
4. `_materializar_uma` exige batida e lastro; reprovação faz rollback. `app/chamados/services/materializacao.py:1142`.
5. Materialização usa `registrar_batida`, escritor único; carimba a pergunta somente após comprovação. `app/chamados/services/materializacao.py:893`, `:1193`; `app/ponto/registro_batida.py:94`, `:157`.
6. Evento da batida recompõe turno e julga dia local/anterior. `app/ponto/signals.py:19`, `:85`, `:108`, `:132`.
7. Cartório lavra célula; porta única escreve veredito, ata e status. `app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:717`, `:775`.
8. Recálculo `on_commit` escolhe competência do fato e chama fechamento; este chama `dia_pago.lavrar`. `app/ponto/services/fechamento.py:1782`, `:1793`, `:1183`; `app/ponto/services/dia_pago.py:135`.
9. Calendário lê DiaPago por `linhas_do_periodo`. `app/colaboradores/services/calendario.py:274`, `:290`.

`veredito_lavrado` da **pergunta** é outra coisa: certifica materialização da pergunta, não é o veredito da célula. `app/chamados/veredito_lavrado.py:211`.

Para col407+21/10, col836+13–14/10 e col909+16/10, os anexos provam estados administrativos e célula acusando, mas não identificam o ATO admin nem o elo que falhou; resposta/validação/materialização e recusa específicas permanecem [não verificado].

## 05/11 16:35 · sessao 0afbc76a1b6f · - · - · origem=Ronald (tmux)

Censo fechado em **72 unidades**, contando cada pessoa+dia explícito; sem dia, uma unidade `ND`; duplicatas A/C eliminadas. Isso vem dos anexos inline, sem arquivo local para citar. [não verificado]

| Parte | Unidades |
|---|---:|
| A | 23 |
| B | 32 |
| C, apenas unidades novas | 17 |
| Total | 72 |

| Grupo | Unidades | Casos |
|---|---:|---|
| G1, candidatos | 19 | col504 21/10; col226 21/10; col709 22/10; col255 03/10; col204 21/09; col43 26/10; col956 16 e 22/10; col253 ND; col283 ND; col502 09/10; col55 03/10; col92 ND; col235 ND; col219 ND; col288 04/10; col523 25/10; col130 05/10; col194 ND |
| G2 | 8 | col407 21/10; col239 07/10; col909 16/10; col292 21/09; col207 ND; col332 ND; col446 ND; col1023 ND |
| G3 | 16 | col29 15/10; col789 ND; col129 ND; col923 ND; col174 25 e 26/10; col33 03 e 04/10; col94 16/10; col163 10 e 17/10; col873 21 e 26/09; col949 ND; col214 ND; col673 ND |
| G4 | 7 | col836 13 e 14/10; col39 15/10; col292 09/10; col42 ND; col960 ND; col250 ND |
| G5 — decisão DP | 15 | col239 12/10; col189 21/09 e 19/10; col43 10 e 12/10; col222 12/10; col256 12/10; col297 06, 13, 20 e 27/10; col247 24/10; col231 ND; col123 ND; col270 ND |
| G5 — exibição | 1 | col246 ND |
| G5 — mistos, defeito não medido | 2 | col89 ND; col78 ND |
| G6 | 1 | UX, sem pessoa/dia |
| Não classificado | 1 | Weverton: ID e período indefinidos |
| Fora | 2 | col523 07/11, futuro; batidas livres, excluído pelo usuário |

**19 + 8 + 16 + 7 + 18 + 1 + 1 + 2 = 72.** Classificação provável pelo relato, sem reprodução de produção. [não verificado]

Cuidados de identidade: `col253 do [nome]` tem **ID 407**, diferente de `col253`, ID 253; `col751 de [nome]` tem **ID 923**. `col465 21/09` aparece somente no JSONL, sem caso no relatório, portanto não soma às 72 unidades. [não verificado]

A contagem inclui as datas existentes no corpo das linhas: col297 06/13/20/27; col292 21/09 e 09/10; col43 10/12/26. Em G4, col836 13–14 fica somente uma vez por dia, priorizando o ato resolvido sem efeito descrito. [não verificado]

Não encontrei `BO-[nome]-0511` ou `BO-[nome]-0511-CENSO` na busca limitada aos Markdown de `app/docs`; a existência do aval em outro local permanece [não verificado].

## 05/11 16:34 · sessao 6cc051563d19 · - · - · origem=Ronald (tmux)

Censo somente estático; execução e incidência além do dado raspado: [nao verificado].

**Leitura histórica direta/por juiz em `ponto/` e `escala/`:**

| Sítio | Comportamento atual |
|---|---|
| `app/escala/utils.py:904` | `_marcar_isencao` consulta a isenção uma vez por colaborador e aplica o resultado a todos os dias (`:909`); chamada pelos builders em `:808` e `:1871`. |
| `app/escala/services/leitor_celula.py:349` | `grade_da_celula` consulta uma vez antes do laço e aplica overlay por dia (`:412`), devolvendo também um único `isento_ponto` no topo (`:416`). |
| `app/ponto/services/cartorio.py:1141` | Consulta uma vez; entrega à impressão dos dois ramos (`:1198`, `:1354`). |
| `app/ponto/services/cartorio.py:890`, `:1229`, `:1246`, `:1419`, `:1806` | Lê diretamente a flag atual ao classificar/lavrar status de células históricas. |
| `app/ponto/services/espelho.py:600`, `:669` | `_isento` único fora do laço elimina inconsistência de todos os dias. |
| `app/ponto/services/fechamento.py:357` | Retorna lista de faltas vazia antes de examinar as datas (`:360`). |
| `app/escala/services/furos_diarios.py:33` | Retorna vazio para toda `[ini,fim]`; `:298` classifica isento no quadro da janela. |
| `app/escala/regua_defesa.py:136`, `:355`, `:513` | Flag direta exclui os diagnósticos de janelas históricas; funções `diagnostico_regua` (`:118`), `turnos_longos_suspeitos` (`:345`), `entradas_fora_do_inicio` (`:503`). |
| `app/escala/services/propositor.py:456` | Exclui colaborador atualmente isento depois de distribuir dias históricos por `vinculo_do_dia` (`:448`); usa janela até ontem (`:436`). |
| `app/escala/services/previsto_cego.py:143` | Filtra não isentos atuais no universo da competência corrente (`:138`). |
| `app/escala/management/commands/termometro_regua.py:34` | Exclui isentos atuais antes do diagnóstico com `--dias` (`:43`, `:47`). |
| `app/ponto/services/esmeril_espelho.py:216` | Exclui isentos da apuração de anomalias da janela e proposta dos últimos sessenta dias (`:219`, `:223`). |
| `app/ponto/management/commands/emitir_furo_retroativo.py:106` | Passa flag atual diretamente para julgamento retroativo. |
| `app/ponto/management/commands/supra_juiz.py:228` | Passa flag atual diretamente para auditoria histórica. |
| `app/ponto/management/commands/mapa_divergencia.py:65`, `:99` | Exclui pelo cadastro atual e pelo topo da grade ao comparar competência. |
| `app/ponto/management/commands/propor_reorganizacao.py:35` | Universo não isento atual; lê janela referenciada em `:65`. |
| `app/ponto/management/commands/vigiar_motor_autoridade.py:46` | Universo não isento atual; motor calcula `[hoje-dias,hoje]` em `:58`. |
| `app/ponto/management/commands/reconciliar_grade.py:230` | Usa alias `isento_art62`; alias apenas delega sem D a `isento_ponto` (`app/ponto/marcos.py:111`, `:115`, `:116`). |
| `app/ponto/management/commands/detectar_ausencias.py:104` | Gate único antes do laço hoje/ontem (`:71`, `:72`, `:112`). |
| `app/ponto/management/commands/processar_alertas_turno.py:85` | Salta turnos materializados de janela de 36h (`:51`, `:76`), inclusive resolução de fechado (`:88`, `:89`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:32` | Gate atual, embora restante do laço seja explicitamente de ontem (`:34`, `:37`). |
| `app/ponto/services/vigia_de_hora.py:117` | Gate atual sobre células de hoje/ontem (`:109`, `:115`); julga pela data da célula (`:131`). |
| `app/ponto/services/bordas_realizado.py:71` | Usa flag única no topo da grade para todos os dias da janela (`:72`); para isentos só órfã prova batida, `n_ok` não (`:49`, `:51`). |

**Leituras sobre hoje/ato atual e distinções:**

- `faltas_de_hoje_sem_cobranca` pergunta hoje e exclui isentos atuais: `app/ponto/services/faltas_de_hoje.py:29`, `:33`.
- `deslogados.apurar` declara pergunta de hoje (`app/ponto/services/deslogados.py:45`) e filtra destinatário atual em `:157`, embora use histórico para medir corrida (`:163`, `:168`); só esse filtro não demonstra que esteja respondendo isenção histórica.
- `escala_auto.dry_da_frota` seleciona propostas para vínculo atual (`app/escala/services/escala_auto.py:171`, `:172`, `:182`); decisão histórica de isenção nesse ponto: [nao verificado].
- `detectar_intervalo_ausente` reconcilia acervo antes do gate de isento (`app/ponto/management/commands/detectar_intervalo_ausente.py:127`, `:138`, `:147`); nascimento já está aposentado (`:149`, `:156`).
- Gates de desligado no cartório são explicitamente de emissão para quem responde agora (`app/ponto/services/cartorio.py:1651`, `:1697`, `:1762`).

**Funções e campos derivados que transportam o efeito:**

- `aplicar_isencao_no_dia` muda célula de memória para `status='ok'`, `missing=False`, `delta=0`, `isencao=True`, com hora prevista; hora real existente permanece (`app/escala/utils.py:850`, `:852`, `:858`, `:860`). O rótulo do dia só entra para trabalho com forma conhecida (`:878`, `:888`, `:889`).
- `_overlay_isencao` só delega à mesma função (`app/escala/services/leitor_celula.py:212`, `:213`); seu docstring antigo diz que sem hora não cobre (`:206`), mas a lei chamada explicita crédito também sem marco (`app/escala/utils.py:823`, `:825`).
- `classificar_dia` retorna lista vazia quando recebe `isento_ponto=True` (`app/ponto/supra_juiz.py:220`, `:221`).
- `fatos_do_status` transporta `isento` como bool (`app/ponto/status_dia.py:190`); porta da célula chama `status_da_lavra` com esses fatos (`app/ponto/portas/celula.py:701`, `:706`).
- `status_da_lavra` precede isenção por falta/ausência/indefinida/folga; com previsto e sem batida, isento resulta `ok` e rótulo próprio (`app/ponto/status_dia.py:139`, `:141`, `:143`, `:148`, `:150`, `:151`).
- `_carimbar_isencao` propaga marcador da célula para lâmpada da ata (`app/ponto/services/cartorio.py:556`, `:557`); atas antigas permanecem ambíguas segundo contrato local (`:547`, `:549`).
- `impressao_insumos` acrescenta `isento:1` somente com regra ≥4 (`app/ponto/services/cartorio.py:520`, `:521`); horizonte da regra é competência 10+, não início individual do passe (`:514`, `:516`).
- `datas_em_aberto_de` recebe `isento` obrigatório e o usa ao calcular ausência (`app/ponto/services/dia_decidido.py:238`, `:263`, `:270`, `:315`); leitor identificado: `app/relatorios/cartao_pela_celula.py:418`.
- PDF herda grade e lê `isencao`/`celulas_isencao`: `app/relatorios/pdf_espelho.py:543`, `:553`, `:556`, `:889`, `:925`.
- A documentação local declara motor sem conhecimento do Art.62 (`app/ponto/services/dia_decidido.py:264`); busca textual não encontrou leitura direta em `motor_calculo_v2.py`; ausência absoluta de caminho indireto: [nao verificado].

**Casos de borda sustentados pelo código:**

- Célula inexistente gera `sem_celula` antes do overlay; não é automaticamente jornada cumprida (`app/escala/services/leitor_celula.py:371`, `:372`, `:412`).
- Célula sem ata lê tipo da célula e pode calcular previsto pelo juiz (`app/escala/services/leitor_celula.py:356`, `:374`, `:376`, `:382`, `:383`); sem lâmpadas não ganha rótulo de isenção (`app/escala/utils.py:878`, `:888`).
- Marcos noturnos são ancorados em D e avançam dia quando o próximo horário não sucede o anterior (`app/escala/servico_jornada.py:62`, `:81`, `:82`, `:83`); saída pode estar em D+1 sem mudar o dia-turno. `marco_vencido` expressa o mesmo caso (`app/ponto/marcos.py:73`, `:100`, `:103`).
- Vínculo do dia é decidido pela geradora dentro da vigência, sem substituir pelo atual; geradora fora da vigência não autoriza escolher outra (`app/escala/alimentacao.py:228`, `:269`, `:272`, `:277`, `:279`).
- Competência com corte 21 vai do dia 21 do mês anterior até dia 20; virada do ano é explícita (`app/ponto/janelas.py:15`, `:16`, `:17`, `:18`, `:20`). A janela usa corte atual da empresa (`:50`, `:57`).
- E3 testa troca da flag provocando rejulgamento em competência 10 (`app/ponto/tests/test_e3_isento_na_impressao.py:189`, `:195`, `:197`, `:198`), e permanência da impressão anterior na 09 (`:215`, `:230`, `:232`, `:233`); não apresenta períodos sucessivos da pessoa nesses trechos.
- O crédito não deve ser confundido com batida; há carimbo próprio exatamente porque hora igual ao marco não prova batida (`app/ponto/services/cartorio.py:527`, `:531`, `:539`, `:556`).

**Leitores indiretos adicionais da mesma grade (para consolidação com outros exploradores):**

`app/ponto/worklist.py:18`; `app/chamados/services/disputa_emissao.py:66`; `app/chamados/services/declaracao_texto.py:82`, `:83`; `app/chamados/services/materializacao.py:233`, `:282`; `app/relatorios/furos.py:132`, `:194`; `app/inteligencia/calculadores.py:42`, `:143`, `:332`; `app/folha/export.py:327`; `app/colaboradores/services/calendario.py:334`; `app/ponto/services/espelho.py:519`; `app/ponto/management/commands/supra_juiz.py:129`; `app/ponto/management/commands/emitir_furo_retroativo.py:60`.

## 05/11 16:33 · sessao 6afa9ba448ed · - · - · origem=Ronald (tmux)

**Escritor e trilha**

- `Colaborador.isento_ponto` é booleano, default `False`; o comentário declara flag como fonte única e `core.constants.isento_ponto` como juiz. (`app/colaboradores/models.py:291`, `app/colaboradores/models.py:294`.)
- Passe Livre exige autenticação e ação `editar_cadastro`; lê o colaborador no POST; `acao == 'add'` significa `True`, qualquer outro valor significa `False`; se muda, exige motivo com ao menos 10 caracteres. (`app/colaboradores/views.py:144`, `app/colaboradores/views.py:151`, `app/colaboradores/views.py:156`, `app/colaboradores/views.py:157`, `app/colaboradores/views.py:164`.)
- O ato compara o valor carregado, chama `gravar(..., 'cadastro', {'isento_ponto': novo_valor})` e só depois chama a auditoria. Repetir o mesmo valor não escreve a linha. (`app/colaboradores/views.py:173`, `app/colaboradores/views.py:176`, `app/colaboradores/views.py:183`; selo estático em `app/colaboradores/tests/test_passe_livre_trilha.py:103`.)
- A porta permite a flag na operação `cadastro`; `gravar` atribui e salva com `update_fields`, sem acrescentar trilha. `salvar_campos` também grava campos permitidos; `salvar_inteiro` salva o objeto completo; `atualizar_em_lote` faz `update`; `criar_colaborador` aceita `**campos`. São capacidades de escrita, não evidência de outros atos específicos da flag em runtime. (`app/colaboradores/services/colaborador.py:34`, `app/colaboradores/services/colaborador.py:55`, `app/colaboradores/services/colaborador.py:65`, `app/colaboradores/services/colaborador.py:73`, `app/colaboradores/services/colaborador.py:80`, `app/colaboradores/services/colaborador.py:91`.)
- A migration 0043 é escritor histórico: semeia `True` por substring de cargo `SUPERVISOR`, `DIRETOR`, `GERENTE`, sem chamar auditoria. O juiz atual não consulta cargo. (`app/colaboradores/migrations/0043_isento_ponto.py:6`, `app/colaboradores/migrations/0043_isento_ponto.py:10`, `app/colaboradores/migrations/0043_isento_ponto.py:13`; `app/core/constants.py:13`.)
- O admin de Colaborador herda `SoLeitura`; não é outro editor manual da flag. (`app/colaboradores/admin.py:47`.)

**Formato e alcance da auditoria**

- Passe Livre grava `acao='editar'`, `modelo='Colaborador'`, `objeto_id=colab.pk`, descrição `Passe livre (isencao Art.62): {antes} -> {novo_valor}. Motivo: {_motivo}`, JSON anterior `{'isento_ponto': antes}` e posterior `{'isento_ponto': novo_valor, 'motivo': _motivo}`. (`app/colaboradores/views.py:183`, `app/colaboradores/views.py:185`.)
- A trilha antiga existia somente na descrição; procurar exclusivamente `valor_antes`/`valor_depois` exclui esse formato antigo. O comentário e o selo narram explicitamente essa diferença. (`app/colaboradores/views.py:178`, `app/colaboradores/tests/test_passe_livre_trilha.py:6`.)
- `registrar_log` leva usuário autenticado e IP para `core.observ.evento`; `evento` é o escritor de `LogAuditoria`, que persiste identificação, descrição, antes/depois e IP. (`app/core/auditoria.py:8`, `app/core/auditoria.py:18`, `app/core/auditoria.py:22`; `app/core/observ.py:94`, `app/core/observ.py:133`.)
- `LogAuditoria` dispõe de usuário, ação, modelo, objeto, descrição, dois JSONs, IP, ator e `criado_em=auto_now_add`; ordenação default apenas `-criado_em`; não há campo separado de vigência da isenção nesse modelo. (`app/core/models.py:295`, `app/core/models.py:300`, `app/core/models.py:306`, `app/core/models.py:307`, `app/core/models.py:312`.)
- A troca da flag pode sobreviver à falha de trilha: o escritor explicita isso, `registrar_log` captura exceção, e `evento` usa best effort com savepoint próprio. Completude da trilha por colaborador não decorre da existência da tela. (`app/colaboradores/views.py:187`, `app/colaboradores/views.py:189`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`.)
- O leitor de auditoria da tela lista `LogAuditoria` por data descendente, filtra usuário/ação/modelo/texto/período e limita a 500. É apresentação da trilha, não julgamento da isenção em D. (`app/core/views.py:705`, `app/core/views.py:707`, `app/core/views.py:713`, `app/core/views.py:715`, `app/core/views.py:726`.)

**Autoridades e quarentena**

- `isento_ponto(colab)` responde exclusivamente pelo booleano carregado, sem argumento de data e sem ler auditoria. (`app/core/constants.py:13`, `app/core/constants.py:18`.)
- `JUIZES.md` declara ser lookup e manda que o registro vivo `core/juizes.py` prevaleça. Não encontrei registro específico de isenção histórica ou implementação de `inicio_da_isencao`/`colab_em_quarentena` na cópia: implementação **[nao verificado]**. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`.)
- A regra pedida mora literalmente em `CORTES.md:352`: isento sem trilha = quarentena desde admissão, rótulo “desde a admissao -- flag sem trilha”; toda troca abre/fecha janela pela porta; item sem dia entra somente em janela aberta. A linha consta como “em construcao”. (`app/docs/CORTES.md:352`.)
- O corte anterior distingue quarentena de cobrança e cálculo: nada apagar/fechar/silenciar; espelho, folha e TXT não mudam pela quarentena; item cujo dia pertence à janela não cobra. Isso não equivale a um juiz histórico implementado para a folha. (`app/docs/CORTES.md:348`.)
- Na cópia, o serviço ativo `silencio_isentos.alvos` seleciona todos os chamados vivos, ainda sem véu, dos colaboradores cuja flag atual é `True`, sem data do chamado nem início da isenção. `silenciar` passa pela porta do arquivo e cria prazo. (`app/chamados/services/silencio_isentos.py:20`, `app/chamados/services/silencio_isentos.py:24`, `app/chamados/services/silencio_isentos.py:27`, `app/chamados/services/silencio_isentos.py:39`, `app/chamados/services/silencio_isentos.py:43`.)
- Receivers chamam esse serviço após commit: save de colaborador com flag ligada e nascimento/retorno de chamado vivo. O receiver de colaborador não testa transição antes/depois: qualquer save de instância com flag verdadeira agenda o serviço. (`app/chamados/signals.py:213`, `app/chamados/signals.py:215`, `app/chamados/signals.py:224`, `app/chamados/signals.py:227`, `app/chamados/signals.py:238`, `app/chamados/signals.py:251`.)
- L-007 exige um escritor e trilha de ação crítica; L-124 trata horizonte da obra, não início/fim individual da flag. (`app/docs/LEIS.md:65`, `app/docs/LEIS.md:145`.)

**Limites para contraexemplos/RED**

- **Duas remoções:** quantidade de linhas `True -> False` não prova dois períodos completos nem datas de entrada; a semente pode iniciar `True` sem trilha, e a auditoria pode falhar. Os JSONs e o texto podem informar transições quando presentes. (`app/colaboradores/migrations/0043_isento_ponto.py:13`, `app/colaboradores/views.py:183`, `app/core/observ.py:105`.)
- **Remover e religar no mesmo dia:** as duas mudanças deixam linhas distintas e timestamp de registro, quando a trilha funciona; o código não tem argumento de dia/instante no juiz. Política de arredondamento do dia da mudança, inclusividade e tratamento do intervalo intradiário pelo aval: **[nao verificado]**. (`app/colaboradores/views.py:173`, `app/colaboradores/views.py:183`, `app/core/models.py:307`, `app/core/constants.py:13`.)
- **Concorrência:** duas requisições podem carregar `True` antes de qualquer salvar e ambas registrar `True -> False`: leitura/comparação/save/log são separados e `gravar` não faz atualização condicional. É contrafactual estático; ocorrência em produção **[nao verificado]**. (`app/colaboradores/views.py:156`, `app/colaboradores/views.py:173`, `app/colaboradores/views.py:176`, `app/colaboradores/views.py:183`, `app/colaboradores/services/colaborador.py:55`.)
- **Sem trilha:** o corte manda desde admissão para o isento sem trilha; a origem sem log pode ser a semente, mas atribuir essa origem a um `colNN` específico do raspado é **[nao verificado]**. (`app/docs/CORTES.md:352`, `app/colaboradores/migrations/0043_isento_ponto.py:6`.)
- **Fuso/borda:** Django usa `America/Sao_Paulo`, `USE_TZ=True`; `criado_em` registra datetime. Converter o timestamp em dia sem considerar esse fuso não é uma equivalência garantida. (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `app/core/models.py:307`.)
- **Turno cross-meia-noite:** as autoridades existentes distinguem pertença ao turno (`turnos_do_colab`), marcos congelados e cruzamento de meia-noite; nenhuma dessas declara a política intradiária da alteração da flag. (`app/core/juizes.py:196`, `app/core/juizes.py:201`, `app/core/juizes.py:204`.)
- **Vínculo trocado:** `vinculo_do_dia` e `vinculo_cobre_a_data` estão registrados; são autoridades de vínculo, não vigência da isenção pessoal. (`app/core/juizes.py:1285`.)
- **Competência exportada:** L-124 preserva L-092/L-113 e remete correção comprovada em exportada à porta existente; o aval dado proíbe tocar exportada, portanto não autoriza reconstrução nela. (`app/docs/LEIS.md:145`; proibição específica do anexo **[nao verificado]** por não haver arquivo local do anexo.)

## 05/11 16:33 · sessao 956be7cc0f3c · - · - · origem=Ronald (tmux)

Censo somente leitura fora de `ponto/`, `escala/`, `folha/` e `relatorios/`; completude de caminhos indiretos não enumerados: [nao verificado].

| Classe | Leitor e efeito atual |
|---|---|
| **D passado** | `reemitir_chamado_do_dia(p,colab,dia)` suprime a reemissão pela flag atual, sem passar `dia` ao juiz: `app/chamados/reconciliador.py:730`, `app/chamados/reconciliador.py:819`. |
| **D passado** | A contraprova do piso de perguntas absolve por `getattr(_col,'isento_ponto',False)`; a condição é posterior aos testes da célula, mas não compara a data da flag com D: `app/chamados/management/commands/auditar_invariantes_chamados.py:400`, `app/chamados/management/commands/auditar_invariantes_chamados.py:410`. |
| **D passado / diagnóstico** | `_calc_escalas_desvio` usa janela de 31 dias terminada ontem, filtrando o universo pela flag atual e pelo vínculo ativo atual: `app/inteligencia/calculadores.py:320`, `app/inteligencia/calculadores.py:325`, `app/inteligencia/calculadores.py:327`, `app/inteligencia/calculadores.py:332`. |
| **D passado / leitura indireta** | `_marcos_faltantes` consulta grade de véspera até D; a flag agregada da grade retorna `ESTADO_FORA_VINCULO,[]` antes do teste de ata ausente: `app/chamados/services/declaracao_texto.py:80`, `app/chamados/services/declaracao_texto.py:83`, `app/chamados/services/declaracao_texto.py:91`, `app/chamados/services/declaracao_texto.py:103`. |
| **D passado / propagação** | O calendário lê `grade_da_celula` para o período; materialização lê os marcos daquela mesma autoridade: `app/colaboradores/services/calendario.py:334`, `app/chamados/services/materializacao.py:233`, `app/chamados/services/materializacao.py:282`. |
| **Hoje / intencional pelo objeto consultado** | API de pendências cadastrais exclui isentos dos cadastros correntes sem plano de folgas, sem adesão e incompletos; cobertura de ausência é consultada para hoje: `app/api/views_mensageria.py:1216`, `app/api/views_mensageria.py:1221`, `app/api/views_mensageria.py:1236`, `app/api/views_mensageria.py:1243`, `app/api/views_mensageria.py:1273`. |
| **Hoje / adesão corrente** | `sincronizar_adesao` calcula `janela_atual(hoje,empresa)`, exclui isentos atuais e testa ausência de hoje para emitir pai de adesão; isso não constitui consulta de vigência individual da flag em cada D: `app/chamados/services/adesao.py:44`, `app/chamados/services/adesao.py:56`, `app/chamados/services/adesao.py:64`, `app/chamados/services/adesao.py:68`, `app/chamados/services/adesao.py:74`. |
| **Hoje / lista de cadastro** | Passe Livre lista isentos atuais e candidatos ativos não isentos: `app/colaboradores/views.py:204`, `app/colaboradores/views.py:206`. |
| **Competência / rótulo diagnóstico** | `bin/censo_folha.py` consulta `classificar_export` para a competência e acrescenta rótulo `ART62` pela flag atual; o rótulo não é cálculo de isenção em D: `bin/censo_folha.py:14`, `bin/censo_folha.py:17`, `bin/censo_folha.py:34`. |

**Silêncio de chamados, escritor e leitores derivados.**

- `silencio_isentos.alvos` seleciona isentos atuais e todos os chamados vivos sem véu, sem cláusula de data do fato ou início individual da isenção: `app/chamados/services/silencio_isentos.py:24`, `app/chamados/services/silencio_isentos.py:27`.
- `silenciar` usa `arquivo.prazo_padrao`, aplica `arquivo.silenciar` chamado a chamado com motivo e `extra={'lote':'isentos'}`; é escrita de véu, não de batidas: `app/chamados/services/silencio_isentos.py:39`, `app/chamados/services/silencio_isentos.py:43`, `app/chamados/services/silencio_isentos.py:47`.
- O receiver do colaborador dispara em qualquer `post_save` cujo objeto esteja isento, sem exigir mudança da flag; agenda `silenciar_do_colab` por `on_commit`: `app/chamados/signals.py:213`, `app/chamados/signals.py:215`, `app/chamados/signals.py:224`.
- O receiver do chamado agenda o mesmo serviço quando chamado nasce/volta vivo ou tem véu levantado, desde que esteja sem véu: `app/chamados/signals.py:228`, `app/chamados/signals.py:238`, `app/chamados/signals.py:242`, `app/chamados/signals.py:251`.
- O cron compartilha `alvos`; `--alarme` impede aplicar, conta e sai com código 2 se houver alvo; `--apply` sem alarme chama a escrita: `app/chamados/management/commands/silenciar_chamados_isentos.py:33`, `app/chamados/management/commands/silenciar_chamados_isentos.py:36`, `app/chamados/management/commands/silenciar_chamados_isentos.py:44`, `app/chamados/management/commands/silenciar_chamados_isentos.py:49`. O agendamento é diário às 07:05 com `--alarme`: `app/config/crons.py:609`.
- Um teste existente exige que desligar a flag **não levante** o véu já escrito; outro exige que resgatar chamado de isento volte ao véu pelo evento: `app/chamados/tests/test_varredura_isentos_evento.py:77`, `app/chamados/tests/test_varredura_isentos_evento.py:85`, `app/chamados/tests/test_varredura_isentos_evento.py:57`, `app/chamados/tests/test_varredura_isentos_evento.py:68`.
- Frente ao corte posterior por período, esse silêncio sem data individual é sítio irmão da mesma classe; não oferece resposta “era isento em D”: `app/chamados/services/silencio_isentos.py:24`, `app/docs/CORTES.md:348`, `app/docs/CORTES.md:352`.

**Limites úteis para a consolidação.**

- `cala_para_isento` responde apenas se o módulo pertence ao conjunto `CALADOS_POR_ISENCAO`; não calcula a condição pessoal nem sua vigência: `app/chamados/catalogo/modulos.py:235`, `app/chamados/catalogo/modulos.py:275`.
- O admin de colaboradores herda `SoLeitura`; não é outra tela editável da flag: `app/colaboradores/admin.py:48`.
- `core/configuracao_efeito.py` declara metadado `('Posto','isento_ponto')`, enquanto o juiz executável lê a propriedade do colaborador; o metadado não demonstra outro leitor operacional: `app/core/configuracao_efeito.py:117`, `app/core/constants.py:18`.
- `bin/censo_folha.py` não foi executado; se executado, escreve CSV apesar do comentário inicial “Zero escrita”: `bin/censo_folha.py:3`, `bin/censo_folha.py:45`.

## 05/11 16:31 · sessao f758b9bcb9ae · PD-JUIZ-3 · censo · origem=Code

## 1 QUEM ESCREVE e QUEM LÊ

O nascimento do chamado, suas transições e a geometria do turno são escritas distintas: `abrir/criar` persistem o nascimento; `_gravar_estado` persiste transições; `recompute_turnos` reconstrói o materializado. (`app/chamados/models.py:932`; `app/chamados/models.py:1048`; `app/chamados/models.py:222`; `app/ponto/nucleo.py:48`)

| Campo ou função | Quem escreve ou chama | Quem lê ou consome |
|---|---|---|
| `TurnoMaterializado`: `colaborador`, `data_turno`, `entrada`, `saida`, `aberto`, `cross_meianoite`, `dt_fim_previsto` | `recompute_turnos` monta linhas, apaga `[ini,fim]` e recria em `atomic`; `atualizado_em` é automático. (`app/ponto/nucleo.py:45`; `app/ponto/nucleo.py:57`; `app/ponto/models.py:1211`) | Emissores de alertas; coletor de premissas; juiz de idade; seleção de turnos atuais; escolha da testemunha de resolução; flip; auditor e verificador. (`app/ponto/management/commands/processar_alertas_turno.py:76`; `app/ponto/management/commands/processar_alertas_avancados.py:58`; `app/chamados/reconciliador.py:95`; `app/ponto/turnos.py:3122`; `app/ponto/selecao_periodo.py:270`; `app/chamados/juizes.py:1420`; `app/ponto/services/flip_batida.py:37`; `app/chamados/management/commands/auditar_invariantes_chamados.py:602`; `app/ponto/management/commands/verificar_nucleo.py:26`) |
| `escalas_no_periodo`, `te_vigente_em`, `dt_fim_previsto_de` | Carregam vínculos, ordenam por `(ativa,data_inicio)`, perguntam cobertura da data e calculam o fim pelo tipo vigente naquele dia. (`app/ponto/nucleo.py:7`; `app/ponto/nucleo.py:18`; `app/ponto/nucleo.py:31`) | `recompute_turnos` usa essas respostas para gravar `dt_fim_previsto`. (`app/ponto/nucleo.py:45`; `app/ponto/nucleo.py:55`) |
| Chamado: `modulo_origem`, `subcategoria`, categoria, título, descrição, área, urgência inicial, SLA e contexto inicial | `turno_aberto` nasce em `_abrir_chamado_critico → abrir`, com `saida_pendente`, emergência, dia e entrada; `_24h` nasce no N5 por `criar`; o emissor retroativo usa `abrir`. (`app/ponto/management/commands/processar_alertas_turno.py:335`; `app/ponto/management/commands/processar_alertas_turno.py:345`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`) | Catálogo escolhe a premissa pela classe; N5 procura chamados existentes; disputa usa entrada e estágio; fila ordena e filtra urgência. (`app/chamados/catalogo/premissa.py:45`; `app/ponto/management/commands/processar_alertas_avancados.py:100`; `app/chamados/services/disputa_emissao.py:116`; `app/chamados/services/painel.py:101`) |
| `status_local`, `resolvido_em`, `via_encerramento`, restauração/rebaixamento da urgência e usuário da transição | `_gravar_estado`; entradas incluem `reconciliar`, cancelar, analisar, reabrir e encerrar. A subida da urgência tem `escalar_urgencia`. (`app/chamados/models.py:222`; `app/chamados/models.py:482`; `app/chamados/models.py:620`; `app/chamados/models.py:751`; `app/chamados/models.py:769`; `app/chamados/models.py:1382`; `app/chamados/models.py:1538`) | Reconciliador, emissores, gates, signals, fila, verbos e marcador de atualização do painel. (`app/chamados/reconciliador.py:1384`; `app/ponto/management/commands/processar_alertas_avancados.py:103`; `app/chamados/models.py:638`; `app/chamados/signals.py:70`; `app/chamados/services/painel.py:155`; `app/chamados/services/verbos.py:230`; `app/chamados/services/painel.py:65`) |
| `contexto_json`, especialmente `data_turno` e `entrada_timestamp` | Nasce pelos emissores; alterações posteriores passam pelas operações `anotar_contexto`, `renovar_contexto`, `retirar_contexto` e `fundir_contexto`. (`app/ponto/management/commands/processar_alertas_turno.py:348`; `app/ponto/management/commands/processar_alertas_avancados.py:126`; `app/chamados/services/contexto.py:62`; `app/chamados/services/contexto.py:81`; `app/chamados/services/contexto.py:106`; `app/chamados/services/contexto.py:128`) | `dia_do_fato`, recorte do turno específico, desempate de destino e fechador específico. (`app/chamados/juizes.py:78`; `app/chamados/reconciliador.py:145`; `app/chamados/services/disputa_emissao.py:128`; `app/ponto/management/commands/processar_alertas_turno.py:166`) |
| `ChamadoColaborador.celula` | No nascimento, `_celula_de` resolve a célula; depois, `apontar_celula` recebe chamadas da rejunção, backfill e carimbo por coincidência. (`app/chamados/models.py:919`; `app/chamados/models.py:1028`; `app/chamados/models.py:1184`; `app/chamados/services/vinculos.py:92`; `app/chamados/rejuncao.py:80`; `app/chamados/services/celula_carimbada.py:109`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:100`) | Dia do fato, guarda de nascimento e condição para emissão do evento em `abrir`. (`app/chamados/juizes.py:89`; `app/chamados/models.py:1063`; `app/chamados/models.py:939`) |
| Disputa: pai, `motivos`, `contexto`, subcategoria inicial | API detecta antes da batida e agrega depois; fábrica também agrega. Disputa existente recebe motivos/contexto; pai novo nasce por `abrir`, com a subcategoria escolhida na emissão. (`app/api/views_core.py:432`; `app/api/views_core.py:777`; `app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/disputa_emissao.py:471`; `app/chamados/services/disputa_emissao.py:498`; `app/chamados/services/disputa_emissao.py:504`; `app/chamados/services/disputa_emissao.py:601`) | Catálogo distingue o par exato `disputa_supervisao/orfao_14h`; fechamento da disputa tenta reconciliar o pai pelo signal. (`app/chamados/catalogo/premissa.py:49`; `app/chamados/signals.py:28`; `app/chamados/signals.py:42`) |
| Célula: `veredito`, `ata`, lâmpadas e `pendente` | Porta da célula recebe lavra/carimbo; `salvar_e_emitir` salva e emite quando os campos lavrados incluem `veredito` ou `ata`. (`app/ponto/portas/celula.py:630`; `app/ponto/portas/celula.py:642`; `app/ponto/portas/celula.py:751`; `app/ponto/portas/celula.py:815`) | `_absolvido_pela_celula`, `marcos_pendentes`, juiz da pergunta e encerramento das órfãs. (`app/chamados/reconciliador.py:986`; `app/ponto/services/lastro.py:138`; `app/chamados/reconciliador.py:610`; `app/chamados/reconciliador.py:1052`) |
| `EventoCelula`: `celula_id`, `colaborador_id`, `data`, `criado_em` | `emitir_evento_celula` insere os identificadores; criação fornece o relógio. Chamadores atuais: `salvar_e_emitir`, nascimento por `abrir` e semeadura. (`app/ponto/portas/celula.py:639`; `app/escala/models.py:1854`; `app/chamados/models.py:941`; `app/chamados/reconciliador.py:1265`; `app/chamados/reconciliador.py:1278`) | Consumidor agrupa por colaborador; ordenação/idade leem criação; semeadura/vigia consultam célula. O consumo não restringe seu trabalho ao dia ou à célula do evento. (`app/chamados/reconciliador.py:1229`; `app/chamados/reconciliador.py:1241`; `app/chamados/reconciliador.py:1309`; `app/chamados/reconciliador.py:1315`; `app/chamados/reconciliador.py:1326`) |
| Evento: `processado_em`, `tentativas`, `erro` | Consumidor marca sucesso, incrementa tentativas/escreve erro em falha e apaga processados antigos. (`app/chamados/reconciliador.py:1337`; `app/chamados/reconciliador.py:1340`; `app/chamados/reconciliador.py:1346`; `app/chamados/reconciliador.py:1353`) | Filtros de pendentes/mortos e retenção leem processamento/tentativas; leitor produtivo específico de `erro`: **[nao verificado]**. (`app/chamados/reconciliador.py:1217`; `app/chamados/reconciliador.py:1223`; `app/chamados/reconciliador.py:1353`) |
| Alerta e justificativa ligados ao `pos_20` | `_marco_critico` consulta alerta existente, abre chamado, registra alerta e então abre justificativa automática; o apontamento da justificativa tem porta própria. (`app/ponto/management/commands/processar_alertas_turno.py:314`; `app/ponto/management/commands/processar_alertas_turno.py:319`; `app/ponto/management/commands/processar_alertas_turno.py:321`; `app/chamados/services/vinculos.py:81`) | Alerta existente impede nova execução do marco; retratação lê a justificativa ligada e pode rejeitá-la se pendente. (`app/ponto/management/commands/processar_alertas_turno.py:314`; `app/chamados/reconciliador.py:529`) |
| Pergunta: carimbo/materialização e disputa fechada | Retratação resolve perguntas; absolvição usa `carimbar`; órfã usa `encerrar_fantasma`; disputa esvaziada passa por `pode_fechar/fechar`. (`app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:620`; `app/chamados/reconciliador.py:1115`; `app/chamados/reconciliador.py:1138`) | Retenção por resposta sem consumo, juiz de vitalidade, gates do chamado e signal do pai. (`app/chamados/reconciliador.py:447`; `app/chamados/reconciliador.py:1054`; `app/chamados/models.py:651`; `app/chamados/signals.py:32`) |

Os chamadores diretos de `recompute_turnos` incluem save/delete de batida, save de vínculo, ambos os emissores de alertas, veredito humano, flip e materialização explícita; suas janelas são diferentes. (`app/ponto/signals.py:26`; `app/ponto/signals.py:48`; `app/ponto/signals.py:64`; `app/ponto/management/commands/processar_alertas_turno.py:67`; `app/ponto/management/commands/processar_alertas_avancados.py:57`; `app/ponto/services/veredito_celula.py:308`; `app/ponto/services/flip_batida.py:39`; `app/ponto/management/commands/materializar_turnos.py:24`)

`criar` também é compartilhado com APIs, telas de colaboradores/ponto, abertura local/DP, regularização externa, ausência de posto/férias, triagem, justificativa, geofence, desligamento, vínculo e smoke. Portanto, seu alcance excede `_24h`. (`app/api/views.py:746`; `app/api/views_core.py:746`; `app/api/views.py:2185`; `app/colaboradores/views.py:1978`; `app/colaboradores/views.py:2363`; `app/ponto/views.py:291`; `app/chamados/services/abertura.py:47`; `app/chamados/services/abertura.py:172`; `app/chamados/services/regularizacao_ext.py:246`; `app/ponto/utils.py:40`; `app/ponto/utils.py:103`; `app/ponto/services/triagem_batida.py:107`; `app/ponto/services/triagem_batida.py:336`; `app/ponto/services/triagem_batida.py:459`; `app/ponto/services/justificativa.py:276`; `app/ponto/services/geofence.py:339`; `app/colaboradores/services/desligamento.py:163`; `app/colaboradores/services/vinculo.py:393`; `app/chamados/management/commands/smoke_chamados.py:123`)

`abrir` também atende views, API de justificativa, ausência, cluster, reemissão, adesão, outros pais de disputa, detecção de ausências, reconciliação de grade, par relâmpago e régua de defesa. (`app/chamados/views.py:973`; `app/api/views.py:1981`; `app/ponto/views.py:3002`; `app/ponto/services/ausencia.py:643`; `app/ponto/services/cluster_espurio.py:104`; `app/chamados/reconciliador.py:879`; `app/chamados/services/adesao.py:74`; `app/chamados/services/disputa_emissao.py:943`; `app/chamados/services/disputa_emissao.py:1163`; `app/ponto/management/commands/detectar_ausencias.py:190`; `app/ponto/management/commands/detectar_ausencias.py:271`; `app/ponto/management/commands/detectar_ausencias.py:510`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/detectar_par_relampago.py:368`; `app/escala/regua_defesa.py:51`)

O consumidor O-OUTBOX existente é `consumir_eventos_celula`: chama a retratação de chamados absolvidos pela célula e o encerramento de órfãs. `reconciliar_chamados_colab` é outra função, chamada pelo signal da batida e pelo cron geral; a rejunção permanece separada nesse cron. (`app/chamados/reconciliador.py:1283`; `app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`; `app/ponto/signals.py:214`; `app/chamados/management/commands/reconciliar_chamados.py:52`; `app/chamados/management/commands/reconciliar_chamados.py:56`)

## 2 JUIZ ou LEI QUE JÁ RESPONDE

- **Aval desta obra:** o anexo determina JUIZ-3 `(a)+(b)`, evento em `criar/recompute_turnos`, consumidor O-OUTBOX e selo estrutural; não concede lei nova. (`avais.txt:1`, anexo fornecido)
- **Fonte de autoridade:** `JUIZES.md` é consulta; aponta `core/juizes.py` como registro vivo. O contrato exige juiz único, escritor e idempotência. (`app/docs/JUIZES.md:7`; `CLAUDE.md:175`)
- **Primeiro respondedor da premissa:** o registro aponta `catalogo/premissa.py::premissa_morta`. O catálogo associa `turno_aberto`, `_24h`, `entrada_bloqueada_turno_aberto` e o par exato `disputa_supervisao/orfao_14h` ao fato turno aberto; o coletor consulta materializado e entrada/dia do chamado. (`app/core/juizes.py:263`; `app/chamados/catalogo/premissa.py:46`; `app/chamados/reconciliador.py:264`; `app/chamados/reconciliador.py:286`)
- **Segundo respondedor da premissa:** o corte nomeia a **célula**, por `_absolvido_pela_celula`. Esse predicado exige dia, célula, veredito não acusador e nenhum marco pendente; o ramo direto limita-se a `MODULOS_DE_TURNO`. Os dois respondedores continuam presentes em caminhos distintos. (`app/docs/CORTES.md:357`; `app/chamados/reconciliador.py:336`; `app/chamados/reconciliador.py:964`; `app/chamados/reconciliador.py:991`)
- **Nascimento, duplicidade e transição:** `abrir` é autoridade declarada de duplicidade; `decidir` governa transição; `motivo_do_veto` governa veto automático; `pergunta_viva` governa vitalidade da pergunta. (`app/core/juizes.py:262`; `app/core/juizes.py:266`; `app/core/juizes.py:268`; `app/core/juizes.py:276`)
- **Prazo:** `turno_aberto_ha` mede idade do materializado; `nivel_turno_aberto` e `urgencia_turno_aberto` aplicam 8/24/72 horas; `lembrete_fim_turno` responde pelos lembretes. Reutilização de juiz de prazo exige a mesma pergunta. (`app/ponto/turnos.py:3110`; `app/ponto/turnos.py:3115`; `app/ponto/turnos.py:3128`; `app/ponto/turnos.py:3141`; `app/ponto/turnos.py:3158`; `CLAUDE.md:427`)
- **Data, competência e vínculo:** `dia_do_fato` prioriza célula e contexto; competência usa corte da empresa e data do fato; `vinculo_do_dia` lê geradora/vigência. Mês civil e vínculo ativo hoje não substituem essas respostas. (`app/chamados/juizes.py:78`; `app/docs/CORTES.md:216`; `app/ponto/janelas.py:24`; `app/escala/alimentacao.py:228`)
- **Outbox e rebaixamento:** o corte exige evento no mesmo `atomic`, consumo idempotente e consumidor de um minuto; o cron só rebaixa quando existe evento produtor da classe. O corte cita `FOR UPDATE SKIP LOCKED`; a implementação atual usa advisory lock nos commands e leitura comum do lote. (`app/docs/CORTES.md:351`; `app/docs/CORTES.md:358`; `app/chamados/reconciliador.py:1203`; `app/chamados/reconciliador.py:1309`; `app/chamados/management/commands/consumir_eventos_celula.py:19`)

**Selo estrutural referido como “17”:** a matriz identifica células por `(família, contrato)`, sem identificador numérico explícito; a correspondência ordinal do rótulo 17 é **[nao verificado]**. A entrada que cobra esta obra é `('chamado','um juiz por pergunta')`: nomeia `JuizChamadoContratoTest`, acompanha `JUIZES_POR_VARREDURA` e `PENDENTES_CHAMADO`, permanece sem verde e registra a condição O168/O169/LASTRO, vedando reclassificação apenas para obter verde. (`app/core/contratos_estruturais.py:62`; `app/core/contratos_estruturais.py:261`; `app/core/contratos_estruturais.py:269`; `app/core/contratos_estruturais.py:1048`)

Hoje, o teste nomeado mede existência das autoridades, ausência de perguntas sem juiz, pendentes e respostas fora da autoridade por censo de código, além de anticópias de data/SLA/COBRANCA. O selo da matriz confere existência da classe e exceções vazias para aceitar verde; essas verificações não demonstram entrega ou cobertura dos eventos. (`app/chamados/tests/test_contract_juiz_chamado.py:24`; `app/chamados/tests/test_contract_juiz_chamado.py:29`; `app/chamados/tests/test_contract_juiz_chamado.py:43`; `app/chamados/tests/test_contract_juiz_chamado.py:59`; `app/chamados/tests/test_contract_juiz_chamado.py:98`; `app/core/tests/test_selo_contratos_estruturais.py:71`; `app/core/tests/test_selo_contratos_estruturais.py:81`)

O teto SQL de valor 17 pertence a outro instrumento: captura `ch.reconciliar()` e compara quantidade de queries. (`app/ponto/tests/test_selo_chokepoint_escrita.py:112`; `app/ponto/tests/test_selo_chokepoint_escrita.py:286`)

## 3 CONTRA-EXEMPLOS PELA REGRA DO AVAL

- **“Chamou `abrir`, logo houve evento.”** O evento exige nascimento novo, módulo de turno e célula. Reutilização com renascimento retorna antes desse trecho; pai `disputa_supervisao/orfao_14h` não integra a lista de módulos do ramo. (`app/chamados/models.py:882`; `app/chamados/models.py:935`; `app/chamados/models.py:939`; `app/chamados/reconciliador.py:336`)
- **“Recomputou, logo o consumidor perceberá.”** O corpo de `recompute_turnos` apenas reconstrói o materializado; consumidor com fila vazia termina sem visitar colaborador. Lavra idêntica também pode retornar sem save/evento. (`app/ponto/nucleo.py:57`; `app/chamados/reconciliador.py:1309`; `app/ponto/portas/celula.py:761`)
- **Borda de data e competência 21→20.** Com corte 21, os fatos de 20/10 e 21/10 pertencem a competências diferentes apesar do mesmo mês civil. No chamado agregador, `competencia_do_chamado` considera também o fato mais recente das perguntas vivas. (`app/ponto/janelas.py:24`; `app/ponto/janelas.py:54`; `app/docs/CORTES.md:216`; `app/chamados/juizes.py:1978`)
- **Cross-meia-noite e outro turno aberto.** A janela inclui véspera/dia, mas entrada declarada já fechada não pode ser sustentada por outro turno dessa janela. Sem dia apurável, o coletor volta à existência global, portanto outro aberto pode sustentá-lo nesse caminho. (`app/chamados/reconciliador.py:98`; `app/chamados/reconciliador.py:145`; `app/chamados/reconciliador.py:152`; `app/chamados/reconciliador.py:286`)
- **Vínculo trocado e dia fora da janela.** O vínculo do dia pode ser antigo e inativo hoje. O receiver de save do vínculo recompõe somente `hoje−2…hoje+1`; uma data histórica fora disso permanece fora dessa recomposição. (`app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:282`; `app/ponto/signals.py:63`; `app/ponto/nucleo.py:58`)
- **Célula sem ata.** `trabalha=None` e ata vazia não significam folga. Porém, com veredito não acusador preenchido, `_absolvido_pela_celula` não exige ata presente: `marcos_pendentes` devolve vazio sobre ata ausente. “Nenhum pendente encontrado” não demonstra, sozinho, existência de ata completa. (`app/chamados/juizes.py:416`; `app/chamados/juizes.py:426`; `app/chamados/reconciliador.py:995`; `app/ponto/services/lastro.py:148`)
- **Janela perdida do `pos_20`.** A tolerância é inclusiva em 240 segundos; 241 segundos fora do alvo não corresponde ao lembrete. Uma execução ausente durante toda a janela pode perder esse nascimento; replay desse ramo não aparece no caminho. (`app/ponto/turnos.py:3155`; `app/ponto/turnos.py:3164`; `app/ponto/management/commands/processar_alertas_turno.py:118`; `app/ponto/management/commands/processar_alertas_turno.py:140`)
- **“Chegou a 24/72 horas, logo extingue.”** Esses limiares governam nível/urgência. O emissor N5 examina ontem; entrada às 23h pode sair desse universo na virada seguinte antes de uma execução elegível. Seu fallback `_24h` também pode reutilizar chamado de outro dia porque não filtra data. (`app/ponto/turnos.py:3133`; `app/ponto/turnos.py:3143`; `app/ponto/management/commands/processar_alertas_avancados.py:22`; `app/ponto/management/commands/processar_alertas_avancados.py:58`; `app/ponto/management/commands/processar_alertas_avancados.py:105`)
- **“Premissa morta, logo fecha.”** Retenção por irmã respondida sem validação ou justificativa alheia pode impedir retratação; depois ainda existem gates de premissa e pergunta viva. Retenção sem exceção não integra, por si, o tratamento de falha que mantém evento pendente. (`app/chamados/reconciliador.py:437`; `app/chamados/reconciliador.py:518`; `app/chamados/models.py:648`; `app/chamados/models.py:651`; `app/chamados/reconciliador.py:1339`)
- **Concorrência.** O fechamento recarrega/trava a linha do chamado dentro do `atomic`; `abrir` serializa pelo colaborador. `criar` não

[truncado em 20000 de 28094 caracteres]

## 05/11 16:31 · sessao 77a097dcbdde · H12H06 · verificador · origem=Code

A transferência de empresa foi corrigida no código; a prova da ordem global de travas continua incompleta, e o teste de status do aplicar aceita um erro posterior como se comprovasse a guarda anterior à escrita. (`app/ponto/management/commands/rederivar_falta_declarada.py:189`, `app/ponto/management/commands/rederivar_falta_declarada.py:193`, `app/ponto/tests/test_h06_reversao_guardas.py:298`, `app/ponto/management/commands/rederivar_falta_declarada.py:202`.)

Revisão estática. Execução independente de testes/mutantes, GREEN, identidade Git da cópia, merge-base, sombra, ocorrência em produção e ordem efetiva dos pousos: **[nao verificado]**. O RED com três falhas é informação do anexo fornecido, não execução desta revisão. (`triagem_v3.txt:9`, anexo.) [nao verificado]

**1. Achados da volta 3**

| Achado | Situação na v4 |
|---|---|
| Reversão sobre outra identidade/empresa | **Fechado no código para os contraexemplos apresentados.** Compara identidade, empresa atual protegida, minutos deixados pelo ato e competências entregues antes da reposição. (`app/ponto/management/commands/rederivar_falta_declarada.py:248`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`, `app/ponto/management/commands/rederivar_falta_declarada.py:258`, `app/ponto/management/commands/rederivar_falta_declarada.py:261`, `app/ponto/management/commands/rederivar_falta_declarada.py:263`.) |
| Arquivo de reversão sobrescrevível | **Fechado para colisão do nome existente.** Nome contém PID/UUID, abertura continua exclusiva, e o teste exige preservar a sentinela. (`app/ponto/management/commands/rederivar_falta_declarada.py:169`, `app/ponto/management/commands/rederivar_falta_declarada.py:172`, `app/ponto/tests/test_h06_reversao_guardas.py:230`.) |
| Aplicar usando empresa antiga depois de transferência | **Fechado no código.** Agora lê Colaborador com trava e recusa empresa diferente antes da edição. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/management/commands/rederivar_falta_declarada.py:189`, `app/ponto/management/commands/rederivar_falta_declarada.py:193`, `app/ponto/management/commands/rederivar_falta_declarada.py:199`.) |
| Concorrência/ranges/atomicidade, como achado agregado | **Parcial.** Permanecem transação, reconferência e guarda de todas as competências; a ordem física das travas e sua compatibilidade global não estão demonstradas pelos testes — item 3 abaixo. (`app/ponto/management/commands/rederivar_falta_declarada.py:180`, `app/ponto/management/commands/rederivar_falta_declarada.py:183`, `app/ponto/management/commands/rederivar_falta_declarada.py:210`, `app/ponto/models.py:819`.) |
| Leitor H12 divergente | **Fechado no código.** TXT e lista chamam o predicado compartilhado; contador e HTML leem `rescisao`. Agora existe caso pela rota para o contador. (`app/folha/export.py:870`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:2`, `app/ponto/tests/test_h06_reversao_guardas.py:385`.) |
| Rollback após falha na segunda reposição | **Fechado como caso de teste estático.** A primeira chamada usa a porta real; a segunda falha dentro do `atomic`. Resultado executado: **[nao verificado]**. (`app/ponto/tests/test_h06_reversao_guardas.py:212`, `app/ponto/tests/test_h06_reversao_guardas.py:219`, `app/ponto/management/commands/rederivar_falta_declarada.py:244`.) |
| Parcialidade 480 versus financeiro | **Aberto, preexistente à v4.** Cobertura usa o limite 480; os leitores financeiros continuam tratando minutos por outros critérios. (`app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2598`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.) |
| Rateio uniforme | **Aberto, preexistente à v4.** Continua no leitor financeiro. (`app/folha/export.py:121`.) |
| Trilha engolida/prova posterior | **Parcial.** Foto divergente aborta a transação, mas `evento` continua best-effort: sua falha pode ser engolida sem impedir a escrita de negócio. (`app/ponto/management/commands/rederivar_falta_declarada.py:218`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`.) |
| Cadastro atual decidindo passado | **Aberto, com investigação separada no aval.** A janela continua lendo o corte atual da empresa. (`app/ponto/janelas.py:50`, `app/ponto/services/fechamento_lista.py:104`, `app/docs/PROMPTS.md:1861`.) |
| Arquivos antigos sem as novas chaves | **Aberto como compatibilidade.** São recusados antes da transação. Existência de arquivos afetados em produção: **[nao verificado]**. (`app/ponto/management/commands/rederivar_falta_declarada.py:239`, `app/ponto/management/commands/rederivar_falta_declarada.py:242`.) |
| Ausência de casos de status/data_fim/contador/trava da volta | **Parcial.** Os casos foram acrescentados; aplicar/status não isola a guarda, e os casos de ordem não comprovam as linhas realmente travadas. (`app/ponto/tests/test_h06_reversao_guardas.py:309`, `app/ponto/tests/test_h06_reversao_guardas.py:350`, `app/ponto/tests/test_h06_reversao_guardas.py:387`.) |

**2. Descartes da triagem**

| Descarte | Conferência |
|---|---|
| **D1 — `invalidar` sem trava só pode recusar a mais** | **Compatível com o código para essa corrida.** `invalidar` preenche a invalidação; o juiz exige vigente com `invalidada_em is None`. Não há nessa operação criação ou desinvalidação de uma entrega. Isso não torna verdadeira a afirmação mais ampla de que todo escritor de ED toma a trava Empresa. (`app/folha/models.py:154`, `app/folha/models.py:158`, `app/folha/models.py:162`, `app/folha/export.py:595`.) |
| **D2 — leitores externos sem trava, sem deadlock mostrado** | A ausência de trava desses leitores não prova, sozinha, deadlock. Tampouco permite dispensar os escritores e sinais alcançados pelo novo caminho: há aquisição condicional de Colaborador durante `Ausencia.save`. Ciclo concreto executado: **[nao verificado]**. (`app/ponto/signals.py:147`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:866`.) |
| **D3 — 480/rateio/arquivos antigos “preexistentes, apartados pelo aval”** | **Preexistentes à v4**, mas “apartados pelo aval” não está demonstrado para todos esses assuntos. O aval separa explicitamente MODO-COMUM; não apresenta dispensa explícita para compatibilidade dos arquivos. Os problemas continuam visíveis no código. (`app/docs/PROMPTS.md:1861`, `app/ponto/turnos.py:2580`, `app/folha/export.py:121`, `app/ponto/management/commands/rederivar_falta_declarada.py:239`.) |
| **D4 — TOCTOU não tem caso determinístico** | **Motivo falso.** O teste já fixa selo/PID/UUID. Um wrapper de `open` pode inserir a sentinela imediatamente antes da abertura real: `x` recusaria; `exists()` seguido de `w` truncaria após o `exists` já ter respondido falso. Não exige acertar um nanossegundo. Caso proposto executado: **[nao verificado]**; implementação atual com `x` permanece correta para essa disputa. (`app/ponto/tests/test_h06_reversao_guardas.py:233`, `app/ponto/tests/test_h06_reversao_guardas.py:236`, `app/ponto/management/commands/rederivar_falta_declarada.py:172`.) |

**3. Ordem de trava**

A sequência **explícita no código** é esta; não equivale a uma prova da ordem física de todas as linhas do SQL. (`app/ponto/management/commands/rederivar_falta_declarada.py:181`, `app/ponto/management/commands/rederivar_falta_declarada.py:182`, `app/ponto/models.py:819`.)

| Caminho | Ordem |
|---|---|
| Aplicar | `atomic` → **Empresa**, por `travar_entrega` → **Ausencia**, por `select_for_update` → **Colaborador**, por `_empresa_de_hoje`, com PK crescente → UPDATE **Ausencia** pela edição → consultas de entrega/foto → commit. (`app/ponto/management/commands/rederivar_falta_declarada.py:180`, `app/ponto/management/commands/rederivar_falta_declarada.py:181`, `app/ponto/management/commands/rederivar_falta_declarada.py:182`, `app/ponto/management/commands/rederivar_falta_declarada.py:189`, `app/ponto/management/commands/rederivar_falta_declarada.py:199`, `app/ponto/management/commands/rederivar_falta_declarada.py:210`.) |
| Reverter | `atomic` → **Empresas**, em PK crescente → **Ausencia**, por `select_for_update` → **Colaborador**, em PK crescente → consultas de entrega → UPDATE **Ausencia** pela reposição → commit. (`app/ponto/management/commands/rederivar_falta_declarada.py:244`, `app/ponto/management/commands/rederivar_falta_declarada.py:245`, `app/ponto/management/commands/rederivar_falta_declarada.py:247`, `app/ponto/management/commands/rederivar_falta_declarada.py:248`, `app/ponto/management/commands/rederivar_falta_declarada.py:263`, `app/ponto/management/commands/rederivar_falta_declarada.py:269`.) |

`travar_entrega` trava **Empresa**, não ExportacaoDominio. `competencia_entregue` consulta ED sem `FOR UPDATE`; `foto` consulta FechamentoMensal sem `FOR UPDATE`. Portanto essas leituras não acrescentam as travas ED/FM à sequência explícita do comando. (`app/folha/export.py:565`, `app/folha/export.py:593`, `app/ponto/management/commands/carimbo_gravado.py:53`.)

Há duas ressalvas à descrição “Empresa → Ausencia → Colaborador”:

- As consultas de Ausencia não usam `of=('self',)` nem substituem a ordenação padrão, que referencia `colaborador__nome`. Não se pode atribuir a aquisição de Colaborador exclusivamente à chamada posterior de `_empresa_de_hoje`; SQL efetivo, conjunto e ordem física das linhas travadas: **[nao verificado]**. (`app/ponto/management/commands/rederivar_falta_declarada.py:182`, `app/ponto/management/commands/rederivar_falta_declarada.py:247`, `app/ponto/models.py:819`.)
- O save dispara julgamento síncrono. **Se houver emissão**, ele alcança outra aquisição de Colaborador: `post_save Ausencia` → cartório → emissor → `ChamadoColaborador.abrir` → `Colaborador.select_for_update`. No comando, seria uma aquisição de uma trava já tomada; em outros escritores, pode ser a primeira. (`app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:1740`, `app/ponto/services/cartorio.py:1778`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:866`.)

Outros escritores/caminhos encontrados que podem acumular duas dessas tabelas:

| Escritor/caminho | Ordem comprovada ou condicional |
|---|---|
| `ExportacaoDominio.registrar` | **Empresa → INSERT ExportacaoDominio**, no mesmo `atomic`. (`app/folha/models.py:179`, `app/folha/models.py:181`, `app/folha/models.py:186`.) |
| `criar_ausencia`, com emissão síncrona pelo signal | **Ausencia → Colaborador**, condicional aos gates do cartório. O `atomic` da criação envolve o save e seus sinais. (`app/ponto/services/ausencia.py:417`, `app/ponto/services/ausencia.py:418`, `app/ponto/signals.py:147`, `app/chamados/models.py:866`.) |
| Aprovação de irmãos | **Ausencia → [Colaborador, se emitir] → FechamentoMensal**; o laço mantém as travas anteriores e pode repetir a sequência para o próximo irmão. (`app/ponto/views.py:2638`, `app/ponto/views.py:2640`, `app/ponto/services/ausencia.py:839`, `app/ponto/services/ausencia.py:950`, `app/ponto/services/fechamento.py:1635`, `app/ponto/services/fechamento.py:1640`.) |
| Regularização de turno, criando ausências intermediárias | O `atomic` externo envolve várias chamadas à criação; pode acumular **Ausencia → [Colaborador, se emitir] → próxima Ausencia**. (`app/ponto/services/regularizacao.py:89`, `app/ponto/services/regularizacao.py:111`, `app/ponto/services/regularizacao.py:118`, `app/ponto/services/ausencia.py:461`, `app/ponto/services/ausencia.py:418`.) |
| Materialização transacional que declara ausência | **Ausencia → [Colaborador, se emitir]**, pelo mesmo save e signal. (`app/chamados/services/materializacao.py:1145`, `app/chamados/services/materializacao.py:1151`, `app/chamados/services/materializacao.py:1048`, `app/chamados/services/materializacao.py:484`, `app/ponto/signals.py:147`.) |

Não contei como transação única a aprovação de fechamento seguida do registro do TXT: a aprovação antecede o `atomic` local de `registrar`. Também não atribuí FechamentoMensal ao desligamento por um signal inexistente: o fechamento dos vínculos usa `QuerySet.update`. (`app/folha/views.py:252`, `app/folha/views.py:297`, `app/folha/models.py:179`, `app/colaboradores/services/desligamento.py:123`, `app/colaboradores/services/vinculo.py:615`.)

**Há aquisição posterior de Ausencia depois de Colaborador em caminhos com laço, mas isso não basta para afirmar deadlock com o comando: é necessário demonstrar disputa pelas mesmas linhas. Não demonstrei esse ciclo.** A aprovação seleciona abertas e o censo H06 seleciona aprovadas; mudanças de estado entre os dois momentos exigem um interleaving adicional. Deadlock efetivo e exaustividade de todas as combinações transitivas de sinais/FKs: **[nao verificado]**. (`app/ponto/views.py:2613`, `app/ponto/views.py:2615`, `app/ponto/management/commands/rederivar_falta_declarada.py:87`, `app/ponto/services/regularizacao.py:111`.)

**4. Os testes novos mordem?** [nao verificado]

Os mutantes abaixo são contrafactuais **por teste isolado**; execução deles: **[nao verificado]**. [nao verificado]

| Teste | Mordida e mutante que permanece verde | [nao verificado]
|---|---|
| Aplicar/empresa | Morde retirar a reconferência da empresa. **Sobrevive retirar apenas a trava de Colaborador**, mantendo leitura e comparação: a transferência é serial, injetada depois do censo. (`app/ponto/tests/test_h06_reversao_guardas.py:293`, `app/ponto/tests/test_h06_reversao_guardas.py:301`, `app/ponto/management/commands/rederivar_falta_declarada.py:56`.) |
| Aplicar/status | **Não morde especificamente a guarda de status.** Sobrevive ignorar `status` na reconferência do aplicar: a edição zera minutos, o modelo não deriva com `pendente`, a pós-checagem encontra `0 != 420`, lança outro `CommandError` e o rollback preserva o resultado esperado. (`app/ponto/tests/test_h06_reversao_guardas.py:309`, `app/ponto/services/ausencia.py:507`, `app/ponto/models.py:877`, `app/ponto/management/commands/rederivar_falta_declarada.py:202`.) |
| Aplicar/data_fim | Morde retirar `data_fim` da identidade: a edição reconstrói o fim pelo `dias_corridos` antigo e pode concluir sem a exceção exigida. **Sobrevive retirar a trava de Ausencia**, mantendo a leitura serial. (`app/ponto/tests/test_h06_reversao_guardas.py:315`, `app/ponto/services/ausencia.py:477`, `app/ponto/services/ausencia.py:499`, `app/ponto/management/commands/rederivar_falta_declarada.py:182`.) |
| Reverter/status | Morde retirar a comparação de status: as demais guardas permitem repor720. **Sobrevive retirar a trava de Colaborador**, mantendo a consulta. (`app/ponto/tests/test_h06_reversao_guardas.py:322`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`, `app/ponto/management/commands/rederivar_falta_declarada.py:269`, `app/ponto/management/commands/rederivar_falta_declarada.py:56`.) |
| Reverter/data_fim | Morde retirar a comparação de fim; o novo fim continua numa competência aberta. **Sobrevive retirar a trava de Colaborador**, mantendo a consulta. (`app/ponto/tests/test_h06_reversao_guardas.py:331`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`, `app/ponto/management/commands/rederivar_falta_declarada.py:263`, `app/ponto/management/commands/rederivar_falta_declarada.py:56`.) |
| Ordem do aplicar | Morde remover/inverter os SQLs reconhecidos. **Sobrevive travar `Colaborador.pk=-1` e depois ler os colaboradores reais sem trava**: o detector exige tabela/`FOR UPDATE`, não alvo ou linhas protegidas. (`app/ponto/tests/test_h06_reversao_guardas.py:350`, `app/ponto/tests/test_h06_reversao_guardas.py:352`, `app/ponto/tests/test_h06_reversao_guardas.py:365`, `app/ponto/management/commands/rederivar_falta_declarada.py:56`.) |
| Ordem do reverter | Mesma mordida e mesmo sobrevivente de trava vazia seguida de leitura real sem trava. (`app/ponto/tests/test_h06_reversao_guardas.py:352`, `app/ponto/tests/test_h06_reversao_guardas.py:369`.) |
| Contador pela rota | Morde restaurar a contagem pela presença de data ou zerar o contador. **Sobrevivem `n_rescisao=1` e uma cópia do predicado na view/produtor**: o caso verifica o número desse cenário, não a proveniência da decisão. (`app/ponto/tests/test_h06_reversao_guardas.py:169`, `app/ponto/tests/test_h06_reversao_guardas.py:170`, `app/ponto/tests/test_h06_reversao_guardas.py:387`, `app/ponto/views.py:1215`, `app/ponto/services/fechamento_lista.py:109`.) |

O RED com apenas três falhas é compatível com guardas e contador já corrigidos na versão anterior; estar verde antes da v4 não significa automaticamente “não morde”. O falso positivo específico é o aplicar/status explicado acima. (`triagem_v3.txt:9`, anexo; `app/ponto/management/commands/rederivar_falta_declarada.py:184`, `app/ponto/management/commands/rederivar_falta_declarada.py:202`.)

**5. Quem escreve e quem lê**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `Colaborador.empresa_id` | Porta de Colaborador; admissão, edição geral e transferência alimentam o valor. A edição geral também escreve empresa, porque `salvar_inteiro` salva o objeto completo. (`app/colaboradores/services/colaborador.py:61`, `app/colaboradores/services/colaborador.py:76`, `app/colaboradores/services/colaborador.py:93`, `app/colaboradores/views.py:1182`, `app/colaboradores/views.py:1313`, `app/colaboradores/views.py:1342`, `app/colaboradores/views.py:1386`.) | Censo, reconferência dos dois caminhos, janela da lista e filtro da foto. (`app/ponto/management/commands/rederivar_falta_declarada.py:87`, `app/ponto/management/commands/rederivar_falta_declarada.py:57`, `app/ponto/management/commands/rederivar_falta_declarada.py:191`, `app/ponto/management/commands/rederivar_falta_declarada.py:258`, `app/ponto/services/fechamento_lista.py:104`, `app/ponto/management/commands/carimbo_gravado.py:53`.) |
| `Colaborador.situacao` | Admissão, desligamento, reativação, lançamento INSS e reversor de afastados, pela porta. (`app/colaboradores/views.py:1205`, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`, `app/ponto/views.py:2432`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`.) | Universo do fechamento e crivo de âncora no export. O campo persistido chama-se `situacao`; o `status` reconferido pelo comando pertence a Ausencia. (`app/colaboradores/models.py:323`, `app/ponto/services/fechamento.py:423`, `app/folha/export.py:814`, `app/ponto/management/commands/rederivar_falta_declarada.py:44`.) |
| `Colaborador.data_demissao` | Agendamento/efetivação do desligamento; reativação limpa o campo. (`app/colaboradores/services/desligamento.py:85`, `app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:211`.) | Predicado compartilhado, universo histórico e recorte do cálculo; contador/HTML recebem o booleano derivado, usando a data no texto. (`app/folha/export.py:60`, `app/ponto/services/fechamento.py:423`, `app/folha/export.py:1091`, `app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:4`.) |
| `Ausencia.colaborador_id` | Criação pela porta. (`app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:418`.) | Identidade, consulta da empresa atual e cobertura. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/management/commands/rederivar_falta_declarada.py:189`, `app/ponto/management/commands/rederivar_falta_declarada.py:248`, `app/ponto/turnos.py:2529`.) |
| `Ausencia.tipo` | Criação e edição; a troca também aciona rejulgamento do status. (`app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:513`.) | Censo/identidade, derivação do modelo, cobertura, parcialidade e financeiro. (`app/ponto/management/commands/rederivar_falta_declarada.py:87`, `app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/models.py:877`, `app/ponto/turnos

[truncado em 20000 de 28613 caracteres]

## 05/11 16:30 · sessao 769cfff6115c · PD-O169 · censo · origem=Code

## 1 QUEM ESCREVE e QUEM LÊ

**O fechamento por janela permanece na cópia:** a coleta consulta 14 dias, enquanto a seleção de fechamento percorre todos os clusters vivos e transforma ausência no conjunto coletado em `False`. O ato chama `reconciliar()` para esses candidatos. (`app/ponto/services/cluster_espurio.py:12`, `app/ponto/services/cluster_espurio.py:23`, `app/ponto/services/cluster_espurio.py:45`, `app/ponto/services/cluster_espurio.py:50`, `app/ponto/services/cluster_espurio.py:98`)

| Campo ou conjunto | Quem escreve/produz | Quem lê e para quê |
|---|---|---|
| `JANELA_DIAS`, `dias`, `hoje/ref` | Serviço declara `14`; comando declara outro padrão `14` e obtém a data local; cartório passa `hoje`, deixando o padrão do serviço. | `dias_com_cluster` calcula início inclusivo `ref-(dias-1)` e consulta até `ref`. (`app/ponto/services/cluster_espurio.py:12`, `app/ponto/services/cluster_espurio.py:21`, `app/ponto/management/commands/detectar_cluster_espurio.py:29`, `app/ponto/management/commands/detectar_cluster_espurio.py:35`, `app/ponto/services/cartorio.py:994`) |
| `Colaborador.situacao`, identidade e `nome` | Cadastro nasce pela porta `criar_colaborador`; desligamento e reativação atribuem a situação; a porta também admite gravação completa. | Serviço exige situação `ativo`; comando filtra ativos; nome entra em assunto/descrição de saída. (`app/colaboradores/services/colaborador.py:73`, `app/colaboradores/services/colaborador.py:91`, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`, `app/ponto/services/cluster_espurio.py:91`, `app/ponto/services/cluster_espurio.py:111`, `app/ponto/management/commands/detectar_cluster_espurio.py:37`) |
| Vínculo: `ativa`, `data_fim`, colaborador | Portas de vínculo reativam, encerram, corrigem e desfazem ajuste; há gravações desses campos fora do cluster. | Serviço exige existir vínculo **atualmente ativo e sem fim**; comando usa o mesmo recorte. O argumento `ec` de `dias_com_cluster` não participa do corpo; `pendencias` passa `None`. (`app/colaboradores/services/vinculo.py:267`, `app/colaboradores/services/vinculo.py:615`, `app/colaboradores/services/vinculo.py:691`, `app/colaboradores/services/vinculo.py:753`, `app/colaboradores/services/vinculo.py:797`, `app/colaboradores/services/vinculo.py:924`, `app/colaboradores/services/vinculo.py:1021`, `app/ponto/services/cluster_espurio.py:15`, `app/ponto/services/cluster_espurio.py:73`, `app/ponto/services/cluster_espurio.py:93`) |
| Célula: colaborador, `data`, `trabalha`, `escala_geradora`, `dna` | Gerador cria e atualiza; regeneração também escreve; reversão restaura DNA, trabalho e geradora. | Cluster seleciona por pessoa/data e pergunta `dia_sem_trabalho`; não escolhe vínculo histórico. A autoridade de vínculo lê a geradora e sua vigência. (`app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:538`, `app/ponto/portas/celula.py:601`, `app/ponto/services/cluster_espurio.py:23`, `app/ponto/services/cluster_espurio.py:28`, `app/escala/alimentacao.py:269`) |
| `ata.tipo_dia`, `ata.orfas`, `ata.lampadas[].acesa`, cobertura parcial | `ata_do_dia` monta o conteúdo; `lavrar_veredito` grava a ata; `carimbar_precedencia` escreve sua chave própria. | Cluster lê tipo pelo juiz, filtra órfãs textuais e **reconta** lâmpadas com `acesa is False`; não usa `ata.n_missing` como contador. Fora dele, `_celula_furada`, cartão pela célula e vigia do pareador também leem esses insumos. (`app/ponto/services/cartorio.py:696`, `app/ponto/services/cartorio.py:704`, `app/ponto/services/cartorio.py:711`, `app/ponto/portas/celula.py:775`, `app/ponto/portas/celula.py:819`, `app/ponto/services/cluster_espurio.py:28`, `app/ponto/services/cluster_espurio.py:30`, `app/chamados/juizes.py:478`, `app/relatorios/cartao_pela_celula.py:288`, `app/ponto/services/vigia_pareador_celula.py:74`) |
| `julgada_em` e instante do julgamento | `carimbar` grava `julgada_em`; `julgar_colab` conserva o instante anterior e o `agora` do ato. | `pendencias` lê ambos por `julgada_antes_deste_julgamento` e `agora_deste_julgamento`, passando-os a `parcial_so_lista`. (`app/ponto/portas/celula.py:808`, `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:946`, `app/ponto/services/cluster_espurio.py:75`) |
| `achados`, `datas`, `na_lista`, `a_abrir`, `a_fechar` | `dias_com_cluster`, compreensão de datas, `parcial_so_lista`, `_sem_chamado` e `_vivos_que_a_premissa_matou` produzem esses valores em memória. | Comando seco/vigia lê as listas; ato lê `a_fechar`, mas abre percorrendo **todos os `achados`**, ressalvados os dias `na_lista`. (`app/ponto/services/cluster_espurio.py:73`, `app/ponto/services/cluster_espurio.py:79`, `app/ponto/services/cluster_espurio.py:97`, `app/ponto/services/cluster_espurio.py:101`, `app/ponto/management/commands/detectar_cluster_espurio.py:50`) |
| `FATO_CLUSTER_ESPURIO`; `Veredito.escopo/viva/motivo` | Catálogo declara a chave e associação à classe; coletor escreve o booleano `_d.isoformat() in datas`; `avaliar` produz o veredito. | `premissa_morta` reduz o veredito a booleano; reconciliador genérico lê o veredito completo. (`app/chamados/catalogo/premissa.py:23`, `app/chamados/catalogo/premissa.py:37`, `app/chamados/catalogo/premissa.py:52`, `app/ponto/services/cluster_espurio.py:50`, `app/chamados/catalogo/premissa.py:149`, `app/chamados/reconciliador.py:1389`) |
| Chamado: `modulo_origem`, categoria, área, assunto, título, descrição e `contexto_json.{data_turno,orfas,n_missing}` | Serviço fornece os valores a `ChamadoColaborador.abrir`. | Módulo/data servem ao dedup e à seleção; catálogo e `dia_do_fato` leem a identidade temporal; painel genérico apresenta os itens do contexto. (`app/ponto/services/cluster_espurio.py:104`, `app/ponto/services/cluster_espurio.py:119`, `app/ponto/services/cluster_espurio.py:58`, `app/chamados/catalogo/modulos.py:115`, `app/chamados/catalogo/modulos.py:219`, `app/chamados/juizes.py:89`, `app/templates/chamados/partials/painel_chamado_turno.html:33`) |
| `status_local`, `resolvido_em`, `via_encerramento`, urgência e atualização | `_gravar_estado` escreve esses campos; `reconciliar` fornece a via de sistema. | Coletor seleciona `VIVOS`; motor decide a transição; portas de fechamento/reabertura releem estado; pergunta/disputa também dependem da passagem para encerrado. (`app/chamados/models.py:222`, `app/chamados/models.py:237`, `app/chamados/models.py:242`, `app/chamados/models.py:250`, `app/chamados/models.py:620`, `app/ponto/services/cluster_espurio.py:45`, `app/chamados/catalogo/motor.py:111`) |

| Função ou porta | Chamadores e leitores do resultado |
|---|---|
| `dias_com_cluster` | `pendencias`; comando a reexporta; testes de detecção, dia sem trabalho e cobertura parcial a chamam diretamente. (`app/ponto/services/cluster_espurio.py:73`, `app/ponto/management/commands/detectar_cluster_espurio.py:19`, `app/ponto/tests/test_detectar_cluster_espurio.py:56`, `app/chamados/tests/test_f1c4_3_emissores.py:116`, `app/ponto/tests/test_cobparc7_cluster_le_a_lista.py:40`) |
| `_vivos_que_a_premissa_matou`, `_sem_chamado` | Ambas alimentam `pendencias`; a primeira consulta todos os vivos, a segunda considera chamado em qualquer estado. (`app/ponto/services/cluster_espurio.py:45`, `app/ponto/services/cluster_espurio.py:55`, `app/ponto/services/cluster_espurio.py:81`) |
| `pendencias` | Ato `reconciliar_colab`, comando seco/vigia e testes do evento/cobertura parcial. (`app/ponto/services/cluster_espurio.py:95`, `app/ponto/management/commands/detectar_cluster_espurio.py:50`, `app/ponto/tests/test_varredura_cluster_evento.py:84`, `app/ponto/tests/test_cobparc7_cluster_le_a_lista.py:43`) |
| `reconciliar_colab` | Cartório após julgamento aplicado com datas julgadas; comando manual com `--apply`; teste do evento. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:990`, `app/ponto/services/cartorio.py:994`, `app/ponto/management/commands/detectar_cluster_espurio.py:34`, `app/ponto/management/commands/detectar_cluster_espurio.py:46`, `app/ponto/tests/test_varredura_cluster_evento.py:82`) |
| `_normalizar`, `_resolver_chave`, `fonte_do_fato`, `avaliar`, `premissa_morta`, `fato_da_classe` | Helpers internos normalizam classe e compõem o veredito; reconciliador genérico chama `avaliar`/`fato_da_classe`; cluster chama `premissa_morta`. (`app/chamados/catalogo/premissa.py:99`, `app/chamados/catalogo/premissa.py:106`, `app/chamados/catalogo/premissa.py:121`, `app/chamados/catalogo/premissa.py:131`, `app/chamados/catalogo/premissa.py:149`, `app/chamados/reconciliador.py:257`, `app/chamados/reconciliador.py:262`, `app/ponto/services/cluster_espurio.py:50`) |
| `premissa_morta`: consumidores fora do cluster | Intervalo, alertas de turno, reconciliar grade, detectar ausências, reconciliar vínculo, triagem, geofence, cobertura, silêncio por ausência e reavaliação de feriado também consomem a função compartilhada. (`app/ponto/management/commands/detectar_intervalo_ausente.py:132`, `app/ponto/management/commands/processar_alertas_turno.py:207`, `app/ponto/management/commands/reconciliar_grade.py:298`, `app/ponto/management/commands/detectar_ausencias.py:809`, `app/ponto/management/commands/reconciliar_vinculo.py:49`, `app/ponto/services/triagem_batida.py:593`, `app/ponto/services/geofence.py:411`, `app/ponto/services/cobertura.py:310`, `app/chamados/services/silencio_ausencia.py:36`, `app/chamados/services/feriado_reavaliar.py:38`) |

O cron do cluster está configurado com `--alarme`, que impede aplicação. `supra_juiz --executar` não dispara diretamente esse serviço: seleciona `FURO_MODULES`, conjunto que não inclui cluster, e retrata por outra cadeia. (`app/config/crons.py:598`, `app/ponto/management/commands/detectar_cluster_espurio.py:34`, `app/ponto/management/commands/supra_juiz.py:186`, `app/ponto/management/commands/supra_juiz.py:286`, `app/chamados/catalogo/furo_modulos.py:15`)

## 2 JUIZ ou LEI QUE JÁ RESPONDE

| Pergunta | Autoridades existentes |
|---|---|
| Cluster pode morrer por idade? | **Lei específica já escolheu (b): não fecha por idade; só morre por lastro.** Aval exige medir 30 dias antes, RED de 15 dias e padrão `level-triggered`. (`app/docs/BACKLOG.md:424`; `avais.txt:1`, anexo fornecido) |
| Premissa desconhecida equivale a morta? | Registro aponta `premissa_morta`; implementação conserva tanto chave ausente quanto valor `None`. Valor falso declara premissa morta. (`app/core/juizes.py:263`, `app/chamados/catalogo/premissa.py:127`, `app/chamados/catalogo/premissa.py:134`) |
| Quem decide estado e dedup? | `VIVOS`, `decidir` e `ChamadoColaborador.abrir` estão registrados. `abrir` pode renascer encerrado quando o emissor volta a afirmar a premissa, sujeito às guardas existentes. (`app/core/juizes.py:261`, `app/core/juizes.py:262`, `app/core/juizes.py:268`, `app/chamados/models.py:879`, `app/chamados/models.py:965`) |
| Quem julga a assinatura de cluster? | A pergunta “a batida é espúria (cluster)?” permanece em `SEM_JUIZ`, mencionando detector e cluster-guard como sítios diferentes. O juiz da **premissa** existe; o registro ainda não declara juiz único da **assinatura**. (`app/core/juizes.py:1184`, `app/core/juizes.py:1185`, `app/core/juizes.py:263`) |
| O dia cala a cobrança? | `dia_sem_trabalho`: `trabalha=False` cala; `None` isolado não cala; tipo de dia preenchido diferente de trabalho cala, com ressalva da ausência parcial que cobra o resto. (`app/core/juizes.py:278`, `app/chamados/juizes.py:411`, `app/chamados/juizes.py:424`, `app/docs/CORTES.md:416`) |
| Qual dia e competência pertencem ao chamado? | `data_do_chamado` e `dia_do_fato`; competência pelo fato mais recente em `competencia_do_chamado`, usando a janela da empresa. Corte K8 e leis L-026/L-090 vedam confundir competência com mês civil. (`app/core/juizes.py:265`, `app/core/juizes.py:274`, `app/chamados/juizes.py:89`, `app/chamados/juizes.py:1978`, `app/docs/CORTES.md:216`, `app/docs/LEIS.md:77`, `app/docs/LEIS.md:123`) |
| Tranca também pode encerrar cluster? | Sim: há via própria por competência efetivamente trancada, com preservação de resgatados posteriores e perguntas vivas preservadas. Esse lastro é distinto de envelhecimento da janela. (`app/chamados/services/competencia_trancada.py:82`, `app/chamados/services/competencia_trancada.py:104`, `app/chamados/services/competencia_trancada.py:135`, `app/chamados/models.py:1541`) |
| Quem responde vínculo e madrugada? | `vinculo_do_dia` considera geradora/vigência; `marcos_turno` encadeia horários atravessando meia-noite. (`app/core/juizes.py:1285`, `app/escala/alimentacao.py:269`, `app/core/juizes.py:196`, `app/escala/servico_jornada.py:61`) |

O lookup determina que o código vivo prevalece; a matriz estrutural continua declarada parcial por O168, O169 e LASTRO, proibindo torná-la verde por reclassificação do papel de um sítio. (`app/docs/JUIZES.md:14`, `app/core/contratos_estruturais.py:269`)

## 3 CONTRA-EXEMPLOS PELA REGRA DO AVAL

- **Borda inclusiva:** com referência em 05/11/2026, padrão 14 consulta 23/10–05/11. Um cluster de 22/10 sai da coleta já com 14 dias decorridos; isso não demonstra morte da assinatura. A regra proíbe o fechamento por idade. (`app/ponto/services/cluster_espurio.py:12`, `app/ponto/services/cluster_espurio.py:21`, `app/ponto/services/cluster_espurio.py:23`, `app/ponto/services/cluster_espurio.py:50`, `app/docs/BACKLOG.md:424`)

- **Competência 21→20:** no cadastro com corte 21, 21/10 pertence à competência encerrada em 20/11. Virar mês ou trancar a competência terminada em 20/10 não constitui lastro para esse chamado; a seleção da tranca compara a competência efetiva. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/chamados/services/competencia_trancada.py:82`, `app/chamados/services/competencia_trancada.py:92`)

- **Cross-meia-noite:** cenário sintético com duas órfãs do mesmo turno, `23:58` e `00:03`, tem distância cronológica de cinco minutos; o coletor ordena minutos do relógio e compara `1438-3`, perdendo a assinatura desse par. A ocorrência desse formato em produção é [não verificado]. (`app/ponto/services/cluster_espurio.py:30`, `app/ponto/services/cluster_espurio.py:33`, `app/escala/servico_jornada.py:80`)

- **Vínculo trocado:** encerrar o vínculo gerador e cadastrar outro não demonstra desaparecimento do fato antigo. O coletor lê pessoa/data; a autorização de executar depende de haver algum vínculo atual ativo sem fim. Sem esse vínculo, retorna antes da leitura dos chamados antigos. (`app/ponto/services/cluster_espurio.py:23`, `app/ponto/services/cluster_espurio.py:93`, `app/escala/alimentacao.py:269`, `app/docs/BACKLOG.md:424`)

- **Célula ausente ou sem ata:** célula inexistente não aparece; `ata=None` vira `{}` e produz nenhuma assinatura. Ambos chegam como `False` ao catálogo especializado, embora o catálogo preserve desconhecimento e o leitor genérico devolva `None` sem célula/ata de lâmpadas. (`app/ponto/services/cluster_espurio.py:23`, `app/ponto/services/cluster_espurio.py:24`, `app/ponto/services/cluster_espurio.py:50`, `app/chamados/catalogo/premissa.py:127`, `app/chamados/juizes.py:457`, `app/chamados/juizes.py:471`)

- **Dia fora da janela:** julgar uma célula recente pode selecionar para fechamento um cluster antigo cuja célula não foi consultada; o disparo do cartório chama o serviço por colaborador, com a janela padrão. (`app/ponto/services/cartorio.py:990`, `app/ponto/services/cartorio.py:994`, `app/ponto/services/cluster_espurio.py:45`, `app/ponto/services/cluster_espurio.py:50`)

- **Assinatura presente não obriga nascimento em toda situação:** parcial destinada apenas à lista do administrador fica fora da abertura, mas continua no conjunto que sustenta a premissa do chamado já vivo. (`app/ponto/services/cluster_espurio.py:65`, `app/ponto/services/cluster_espurio.py:74`, `app/ponto/services/cluster_espurio.py:81`, `app/ponto/services/cluster_espurio.py:102`)

- **Candidato não equivale a fechamento:** pergunta viva sem destino admissível pode recusar o ato. O cluster canônico não passa pela proteção de premissa de furo, mas passa pela guarda da pergunta viva. (`app/chamados/models.py:405`, `app/chamados/models.py:648`, `app/chamados/models.py:651`, `app/chamados/models.py:690`, `app/chamados/models.py:1194`, `app/ponto/services/cluster_espurio.py:109`)

- **Concorrência:** estado do chamado é relido e travado dentro do fechamento; isso não demonstra atualização da assinatura coletada anteriormente. Para cluster canônico, a guarda de furo autoriza sem recolher essa assinatura. Corrida concreta em produção é [não verificado]. (`app/ponto/services/cluster_espurio.py:95`, `app/ponto/services/cluster_espurio.py:98`, `app/chamados/models.py:616`, `app/chamados/models.py:638`, `app/chamados/models.py:668`, `app/chamados/models.py:690`)

- **Evento e vigia podem discordar sobre renascimento:** `_sem_chamado` considera qualquer estado, enquanto o ato chama `abrir` para todo achado; um encerrado pode renascer com `criado=False`, sem aumentar `abertos`. (`app/ponto/services/cluster_espurio.py:55`, `app/ponto/services/cluster_espurio.py:101`, `app/ponto/services/cluster_espurio.py:122`, `app/chamados/models.py:882`, `app/chamados/models.py:885`)

## 4 LISTA DE CASOS RED

Os casos abaixo são **cenários sintéticos de aceitação pela regra**, não resultados de execução nem registros individuais de produção [não verificado]. A referência normativa é não fechar por idade, exigir lastro e observar o padrão `level-triggered`. (`app/docs/BACKLOG.md:424`; `avais.txt:1`, anexo fornecido)

| Caso | Cadastro + estado + ato | O que a regra exige |
|---|---|---|
| R1 — **15 dias** | Pessoa ativa, vínculo ativo sem fim; cluster vivo de `hoje-15`, ata ainda com assinatura; julgar célula recente ou executar reconciliação padrão. | Preservar o chamado antigo: quinze dias não são lastro de morte. (`app/docs/BACKLOG.md:424`; `avais.txt:1`, anexo) |
| R2 — borda | Mesmo cadastro; assinatura preservada em `hoje-13`; avançar referência para o dia seguinte. | Não haver mudança de vida apenas porque o fato atravessou a borda da coleta. (`app/docs/BACKLOG.md:424`, `app/ponto/services/cluster_espurio.py:21`) |
| R3 — fora de janela ampliada | Mesmo cadastro; chamado com assinatura preservada anterior ao período observado; executar leitura com outra quantidade de dias. | A quantidade de dias observados não autoriza encerrar o que não foi medido. (`app/docs/BACKLOG.md:424`, `app/chamados/catalogo/premissa.py:127`) |
| R4 — célula inexistente | Mesmo cadastro; chamado vivo com dia forte, sem célula correspondente; reconciliar. | Conservar por desconhecimento; ausência de célula não prova regularização. (`app/chamados/catalogo/premissa.py:127`, `app/chamados/juizes.py:457`, `app/docs/BACKLOG.md:424`) |
| R5 — célula sem ata | Mesmo cadastro; chamado vivo, célula de trabalho com ata ausente/sem lâmpadas; reconciliar. | Não tomar ata não lavrada como lastro de morte. (`app/chamados/juizes.py:471`, `app/chamados/catalogo/premissa.py:127`) |
| R6 — lastro positivo de morte | Mesmo cadastro; chamado vivo; retratação humana das espúrias seguida de julgamento comprova desaparecimento da assinatura, sem pergunta viva impeditiva. | Admitir encerramento por desaparecimento comprovado da premissa. (`app/docs/BACKLOG.md:424`, `app/ponto/tests/test_varredura_cluster_evento.py:50`, `app/chamados/models.py:393`) |
| R7 — madrugada | Escala noturna; chamado vivo, órfãs `23:58/00:03` comprovadamente do mesmo turno e marco apagado; reavaliar o fato. | Não declarar morto o cluster por perda da cronologia na representação das horas. (`app/ponto/services/cluster_espurio.py:16`, `app/escala/servico_jornada.py:61`, `app/docs/BACKLOG.md:424`) |
| R8 — competência | Empresa com corte 21; cluster vivo de 21/10, competência 11 aberta e competência 10 trancada; atravessar mês/reconciliar. | Preservar: a tranca de outra competência e a idade não encerram esse fato. (`app/ponto/janelas.py:10`, `app/c

[truncado em 20000 de 25571 caracteres]

## 05/11 16:28 · sessao 57e81fcf8eff · SOMBRA-NAO-CABE-2 · verificador · origem=Code

A cura de empresa/tranca entre colaboradores está presente, mas permanecem divergências temporais: a célula da pergunta é lida mais cedo, e a grade conserva células e vínculos que o caminho anterior relia. Isso permite respostas diferentes com o mesmo juiz. (app/chamados/juizes.py:940–954; app/chamados/services/pergunta_no_app.py:46–53; app/escala/services/leitor_celula.py:350–395; app/escala/utils.py:2158–2160,2200–2215)

Identidade Git da cópia, execução de testes/mutantes, ocorrência dos cenários em produção, resultados independentes de sombra e pouso: [nao verificado]. Os contraexemplos abaixo são deduções do código, com execução e ocorrência explicitamente não verificadas. [nao verificado] [nao verificado]

**1. Quem escreve, quem lê e qual é o dono.**

| Estado do memo | Quem escreve | Quem lê e onde fica |
|---|---|---|
| `_tranca_memo` | `_memo_da_tranca` cria por `setdefault`; um teste o remove. | Fora do lote: requisição, quando disponível; senão, instância do colaborador. (app/chamados/juizes.py:975–979; app/ponto/tests/test_selo_chokepoint_escrita.py:417) |
| `('empresa', empresa_id)` | `dia_da_pergunta_trancado` guarda `colab.empresa`. | A mesma função entrega a empresa a `janela_atual`. Usa `lote=False`, inclusive dentro do comando. (app/chamados/juizes.py:940–944) |
| `('tranca', empresa_id, mês, ano)` | A mesma função guarda `periodo_trancado`, inclusive `None`. | A mesma função lê presença e `fechado_em`. Também fica na requisição/colaborador, inclusive dentro do lote. (app/chamados/juizes.py:940,945–954) |
| `('toque', colab.pk, data, desde)` | `_admin_tocou_o_dia_depois` grava somente `True`. | A mesma função reutiliza o SIM; após NÃO, consulta novamente. Dentro do lote pertence ao lote; fora, à requisição/colaborador. (app/chamados/juizes.py:972–979,1041–1050) |
| `('sistema',)` | `_toque_humano_no_dia` guarda o PK de `get_user_sistema`. | A mesma função exclui esse PK dos autores humanos; recebe o mesmo memo do toque. (app/chamados/juizes.py:1047,1068–1072) |
| `LOTE_DA_TRANCA` | Declaração com `None`; `tranca_em_lote` instala `{}` e restaura o token em `finally`. | `_memo_da_tranca`, guarda de reentrância e teste do comando. (app/chamados/juizes.py:972–974,990–1003; app/chamados/tests/test_snc2_toque_em_lote.py:208–213) |
| `REQUISICAO_DA_TRILHA` | Middleware instala/restaura; teste também instala/restaura. | `_memo_da_tranca` escolhe o dono; `evento` obtém usuário/IP. O lote não substitui esse contexto. (app/core/middleware.py:76–82; app/chamados/tests/test_snc2_toque_em_lote.py:235–243; app/chamados/juizes.py:975–979; app/core/observ.py:108–118) |

O comando abre o contexto para `censo`, `censo_alcance`, `viva_em_disputa_fechada`, `respondida_sem_resposta` e `materializada_viva`. (app/chamados/management/commands/vigiar_fio_morto.py:171–176)

As fontes consultadas pelo memo têm estes escritores e leitores fora do diff:

| Fonte | Escritores | Leitores relevantes |
|---|---|---|
| Empresa do colaborador | Edição atribui `empresa_id`; transferência atribui a empresa do posto e salva pela porta do colaborador. | C6 lê `empresa_id`/`empresa` para identificar a competência. (app/colaboradores/views.py:1313,1383–1388; app/colaboradores/services/colaborador.py:73–76; app/chamados/juizes.py:932–945) |
| Corte da empresa | Formulário recebe o corte e chama `atualizar_empresa`, que salva. | `janela_atual` → `corte_da_empresa` → classificação da competência. (app/colaboradores/views.py:496–499,521–523; app/colaboradores/services/estrutura.py:17–20; app/ponto/janelas.py:24–33,50–63) |
| Tranca e reabertura | `trancar_periodo` cria/retranca e atualiza `fechado_em`; `reabrir_periodo` grava `reaberto_em`. | `periodo_trancado` seleciona somente linha com `reaberto_em` nulo; C6 usa seu `fechado_em`. (app/ponto/services/fechamento.py:1392–1403,1421–1425,1448–1454; app/chamados/juizes.py:947–954) |
| Usuário sistema | `get_user_sistema` usa `get_or_create`; o comando garantidor cria/normaliza o usuário. | Exclusão do autor sistema na trilha humana. (app/core/utils.py:18; app/core/management/commands/garantir_user_sistema.py:13–41; app/chamados/juizes.py:1068–1072) |
| Trilha: usuário, ação, modelo, objeto, descrição, tempo | `registrar_log` adapta para `evento`; `evento` cria a linha; `criado_em` é automático. | O helper filtra tempo estritamente posterior, usuário não nulo/não sistema e os três atos. (app/core/auditoria.py:22–24; app/core/observ.py:134–138; app/core/models.py:295–307; app/chamados/juizes.py:1071–1084) |
| Cobrar | Serviço grava ação, PK do colaborador e `dia=ISO`. | Primeiro `exists()` do helper. (app/chamados/services/cobrar_dia.py:29–39; app/chamados/juizes.py:1073–1075) |
| Resolver | Serviço e porta da célula escrevem `veredito_celula`. | Helper consulta células atuais do colaborador/dia, sem filtro de `via`. (app/ponto/services/veredito_celula.py:329–336; app/ponto/portas/celula.py:770–789; app/chamados/juizes.py:1076–1080) |
| Desarquivar | Porta altera o arquivo e chama `_trilha` → `evento`. | Helper reconstrói os chamados atuais ligados às células daquele colaborador/dia. (app/chamados/services/arquivo.py:130–138,193–209; app/chamados/juizes.py:1081–1084) |
| Identidades de célula e chamado | Porta cria célula com colaborador/data; nascimento do chamado grava colaborador/célula; `apontar_celula` preenche vínculo ausente. | Helper busca essas identidades após cada resultado negativo. (app/ponto/portas/celula.py:247–248,601–604; app/chamados/models.py:913–932,1028–1048; app/chamados/services/vinculos.py:92–100; app/chamados/juizes.py:1076–1084) |
| Pergunta: disputa, data, célula e carimbos | `abrir` recebe identidade e associa célula; métodos canônicos escrevem carimbos; Resolver fecha perguntas obtidas por consulta própria. | `_chave_memo_pv` e `_pergunta_viva_calc` leem esses campos. (app/chamados/models.py:2387–2419,2465–2466,2497–2512,2545–2549; app/ponto/services/veredito_celula.py:174–178; app/chamados/juizes.py:801–818,1135–1196) |

Fora do comando, os leitores alcançam emissão, reconciliação, fechamento/ressuscitação e carimbo de perguntas; `perguntas_do_app` também atende API, ficha, medição e consulta do dia. (app/chamados/services/disputa_emissao.py:1585–1589,1663; app/chamados/reconciliador.py:610–616,716–727; app/chamados/models.py:1447,1776,2585; app/api/views.py:2552; app/colaboradores/services/ficha.py:166; app/chamados/services/pergunta_no_app.py:139,162)

Na grade, os argumentos efetivamente passados são estes:

| Argumento | Quem o fornece/escreve | O que o juiz faz |
|---|---|---|
| `colab` | É o argumento da grade; seu PK identifica as cargas e as chaves. | `_celula_do_dia` e `_esc_vigente_do_dia` restringem ao colaborador. (app/escala/services/leitor_celula.py:345,350,391–394; app/escala/utils.py:2158–2160,2201–2204) |
| `data=_cur` | A grade inicia em `fim` e decrementa até `ini`. | Seleciona a chave do dia, verifica vigência e obtém trabalho/marcos/pausas. (app/escala/services/leitor_celula.py:359–360,394,427; app/escala/alimentacao.py:269–286; app/escala/utils.py:2281–2290,2312) |
| `celulas=_cels_alim` | A grade transforma sua carga completa da janela em `{(colab.pk, data): CelulaDia}`. | Alimenta seleção da geradora, `eh_dia_trabalho`, `marcos_do_dia` e `pausas_do_dia`. (app/escala/services/leitor_celula.py:350,393–395; app/escala/utils.py:2281–2290,2312; app/escala/models.py:1087–1096,1117–1133,1182–1208) |
| `escalas=_escalas_alim` | Primeira necessidade carrega todos os vínculos do colaborador, com `tipo_escala`, sem filtro de janela/atividade. | `_esc_vigente_do_dia` entrega a lista a `vinculo_do_dia`; `[]` significa nenhum vínculo. (app/escala/services/leitor_celula.py:389–394; app/escala/utils.py:2175–2187,2200–2215,2277–2283) |
| `folgas` | Não é passado; permanece `None`. | O juiz transmite somente alimentações presentes. Com célula presente, `eh_dia_trabalho` devolve `cel.trabalha`. (app/escala/services/leitor_celula.py:394–395; app/escala/utils.py:2251,2286–2288; app/escala/models.py:1089–1096) |

Os objetos dessa alimentação têm escritores próprios:

| Campos | Escritores | Leitores |
|---|---|---|
| Célula: colaborador/data, `trabalha`, geradora, DNA | Porta cria, regenera, restaura e atualiza células. | Grade, juiz do vínculo e métodos de trabalho/marcos/pausas. (app/ponto/portas/celula.py:247–265,538–546,601–623; app/escala/services/leitor_celula.py:350–395; app/escala/alimentacao.py:269–283; app/escala/models.py:1087–1208) |
| DNA: horários e pausas | `montar_dna` congela o template. | Métodos da EC entregam marcos/pausas; previsto calcula duração líquida. (app/ponto/management/commands/gerar_celulas.py:73–103; app/escala/models.py:1124–1133,1189–1208; app/escala/utils.py:2290–2341) |
| Vínculo: colaborador, template, início, fim, atividade | Porta de vínculo cria/atualiza; fechamento de vigência; correção e trecho retroativo. | Juiz escolhe geradora ou vigência; métodos da EC entregam os marcos. (app/colaboradores/services/vinculo.py:265–288,612–625,909–925; app/escala/alimentacao.py:205–216,269–286; app/escala/models.py:1098–1133) |
| Template: horários, intervalos e horários por dia | Cadastro escreve/salva e pode propagar regeneração. | Fallback dos marcos/pausas da EC. (app/escala/services/cadastro_tipo.py:223–235,264–266,306,334–350; app/escala/models.py:326–345,1133,1208) |
| `ata.minutos_previstos` | Cartório produz; porta persiste a ata. | Grade prefere o valor lavrado e só consulta previsto se ausente. (app/ponto/services/cartorio.py:696–705; app/ponto/portas/celula.py:735–751,775–783; app/escala/services/leitor_celula.py:382–395) |

O juiz continua calculando pelos marcos, descontando pausas, com tratamento da virada; não usa `minutos_jornada` como jornada prevista. (app/escala/utils.py:2251–2255,2290–2295,2312,2326–2341)

A mudança da grade alcança folha, espelho, calendário, relatórios, calculadores, furos, suprajuiz, worklist, declaração, emissão e materialização. (app/folha/export.py:327; app/ponto/services/espelho.py:519; app/colaboradores/services/calendario.py:334; app/relatorios/furos.py:132,194; app/inteligencia/calculadores.py:42,143,332; app/escala/services/furos_diarios.py:43; app/ponto/management/commands/supra_juiz.py:129; app/ponto/worklist.py:18; app/chamados/services/declaracao_texto.py:82; app/chamados/services/disputa_emissao.py:66; app/chamados/services/materializacao.py:233,282)

**2. Contraexemplos concretos e formas dos vínculos/células.**

**Vigia: novo=viva; anterior=falsa após Resolver confirmado.** Considere competência aberta, P2 ainda não julgada, sem validação/resposta/materialização, vinculada a célula `furo`. `perguntas_do_app` carrega a lista; durante o julgamento de outra pergunta, o admin confirma Resolver de P2 como `trabalhou`, sem marcos pendentes. O serviço fecha perguntas por instâncias próprias e grava a célula. (app/chamados/services/pergunta_no_app.py:46–53; app/ponto/services/veredito_celula.py:174–178,324–336)

P2 da lista conserva seus carimbos anteriores nos dois caminhos. No novo, conserva também a célula `furo` e C6 retorna `True`; no caminho anterior fornecido, a célula seria carregada por `_chave_memo_pv` depois do ato, veria `trabalhou` e retornaria `False`. Portanto, carimbos da pergunta já serem antigos não neutraliza a diferença introduzida na célula. (app/chamados/juizes.py:801,1135–1172,1181–1196; app/chamados/services/pergunta_no_app.py:47–53)

Execução dessa intercalação e ocorrência em produção: [nao verificado].

**Grade: novo=480; anterior/avulso=0 após encerrar vínculo.** Considere col01 com EC iniciada em 01/07/2026, ativa e sem fim; células de 04 e 05/08, ambas com essa geradora, trabalhando, DNA 08:00–17:00 e pausa 12:00–13:00, sem previsto lavrado. A grade começa em 05/08 e conserva a lista de vínculos. Durante a chamada, `fechar_vigencia(EC, 03/08/2026, autor=admin)` confirma `ativa=False` e `data_fim=03/08`. (app/escala/services/leitor_celula.py:359–360,382–395; app/colaboradores/services/vinculo.py:612–625)

Ao julgar 04/08, o novo caminho ainda entrega a EC sem fim e produz **480**. O anterior recarregaria a EC, encontraria a geradora fora da vigência e produziria **0**, assim como o juiz avulso consultado depois da escrita. Não é necessário alterar célula ou ata para produzir essa diferença. (app/escala/utils.py:2201–2204,2282–2295,2326–2341; app/escala/alimentacao.py:279–283; app/escala/services/leitor_celula.py:391–395)

Execução dessa intercalação e ocorrência em produção: [nao verificado].

**Grade: novo=480; anterior/avulso=540 após regenerar DNA.** Considere célula gerada de competência aberta, carregada pela grade com DNA 08:00–17:00/12:00–13:00 e ata sem previsto. Antes do cálculo daquele dia, a porta confirma regeneração para 08:00–18:00/12:00–13:00. O novo juiz recebe a célula antiga da grade e produz **480**; o anterior releria a célula e produziria **540**. A ata já carregada pela grade pode permanecer igual nos dois caminhos. (app/escala/services/leitor_celula.py:350,382–395; app/ponto/portas/celula.py:243–265; app/escala/utils.py:2158–2160,2279–2280,2290–2295,2312,2326–2341)

Execução dessa intercalação e ocorrência em produção: [nao verificado].

Nos casos estáticos, a alimentação preserva estes contratos:

| Forma | Resultado da inspeção |
|---|---|
| Geradora fora da janela carregada | Continua na lista, pois a consulta restringe somente colaborador. (app/escala/services/leitor_celula.py:391–392) |
| Geradora fora da própria vigência | O juiz devolve `None`; previsto vira zero. (app/escala/alimentacao.py:279–283; app/escala/utils.py:2282–2283) |
| Múltiplos vínculos | Havendo geradora, seu PK manda; sem geradora, vence o máximo por `(ativa, data_inicio)` entre os que cobrem a data. (app/escala/alimentacao.py:269–286) |
| Célula sem geradora | Usa o fallback de vigência, mantendo a célula como fonte de trabalho/DNA. (app/escala/alimentacao.py:285–286; app/escala/models.py:1089–1096,1119–1133) |
| Nenhum vínculo | Lista vazia produz zero. (app/escala/utils.py:2277–2278) |
| Dia sem célula | A grade devolve `sem_celula/0` antes de chamar o juiz. Comparação direta com eventual previsto aritmético do avulso não demonstra regressão deste diff. (app/escala/services/leitor_celula.py:373–378; app/escala/models.py:1094–1096) |
| Folga ou previsto lavrado | A grade mantém zero para não trabalho sem previsto; havendo previsto lavrado, prefere a ata. (app/escala/services/leitor_celula.py:382–395) |

Os contraexemplos v2 de compartilhar empresa/tranca entre colaboradores frescos foram retirados por `lote=False`; o NÃO do toque continua sendo relido, inclusive com vínculo que aparece durante o lote. Isso não torna frescos os memos herdados da mesma requisição/instância. (app/chamados/juizes.py:940–948,975–979,1043–1084; app/chamados/tests/test_snc2_toque_em_lote.py:137–198)

**3. Regra alterada sem aval demonstrado.**

Não apareceu segundo corpo de `pergunta_viva` ou de `minutos_previstos_do_dia`: permanecem os endereços registrados, e os leitores chamam essas autoridades. (app/core/juizes.py:173,266; app/chamados/juizes.py:839; app/escala/services/leitor_celula.py:394)

A alteração observável está na atualidade dos insumos: C6 pode decidir pela célula anterior a Resolver confirmado; previsto pode decidir pela célula/vigência anterior à escrita confirmada. Os corpos dos juízes permanecem, mas o valor entregue deixa de acompanhar o caminho avulso nos cenários acima. (app/chamados/services/pergunta_no_app.py:47–53; app/chamados/juizes.py:1181–1196; app/escala/services/leitor_celula.py:350,391–395; app/escala/utils.py:2159,2201–2204)

Aval específico para aceitar esses vereditos/valores diferentes: [nao verificado].

O desvio da tranca compartilhada foi curado no trecho alterado; `_pv_memo` por pergunta e empresa/tranca por requisição/colaborador continuam com as respectivas durações herdadas. (app/chamados/juizes.py:835–847,940–948,975–979)

**4. Selos que mordem e selos que passam com a cura revertida.**

A tabela considera reversões **isoladas**, mantendo os demais trechos atuais: “toque 144” restaura o NÃO/conjunto antigo descrito no anexo; “empresa/tranca 766” restaura essas chaves no lote. São previsões por inspeção; execução dos mutantes: [nao verificado].

| Teste atual | Toque 144 | Empresa/tranca 766 | Comando sem lote | [nao verificado]
|---|---|---|---|
| Custo usuário/trilha | Falha pelo crescimento de trilha exigido | Passa | Passa: abre lote diretamente. (app/chamados/tests/test_snc2_toque_em_lote.py:87–111) |
| Paridade estática | Passa | Passa | Passa. (app/chamados/tests/test_snc2_toque_em_lote.py:82–85,113–135) |
| Cobrar durante lote | Falha | Passa | Passa. (app/chamados/tests/test_snc2_toque_em_lote.py:137–149) |
| Chamado ganha célula | Falha | Passa | Passa; fixture escreve FK diretamente. (app/chamados/tests/test_snc2_toque_em_lote.py:151–164) |
| Reabrir durante lote | Passa com empresa/tranca atuais | Falha | Passa: lote direto. (app/chamados/tests/test_snc2_toque_em_lote.py:171–184) |
| Trancar durante lote | Passa com empresa/tranca atuais | Falha | Passa: lote direto. (app/chamados/tests/test_snc2_toque_em_lote.py:186–198) |
| Comando dentro do lote | Passa | Passa | Falha. (app/chamados/tests/test_snc2_toque_em_lote.py:200–213) |
| Guarda por instância | Passa | Passa | Passa. (app/chamados/tests/test_snc2_toque_em_lote.py:215–223) |
| Request posterior | Passa | Passa | Passa. (app/chamados/tests/test_snc2_toque_em_lote.py:225–243) |

Para a introdução original do lote, a guarda de custo distingue o caminho avulso: exige economia de usuário sistema e total menor; a paridade estática, isoladamente, não distingue otimização de avulso. (app/chamados/tests/test_snc2_toque_em_lote.py:102–135)

O selo do comando observa exclusivamente `censo`. Os outros quatro consumidores poderiam sair do contexto sem mudar essa observação. (app/chamados/tests/test_snc2_toque_em_lote.py:205–212; app/chamados/management/commands/vigiar_fio_morto.py:171–176)

No passo 2, custo morde a reversão da alimentação; paridade de valor passaria com a otimização revertida. Essa paridade é guarda estática, não testemunha das intercalações demonstradas. (app/escala/tests/test_snc2_grade_previsto_em_lote.py:83–109)

A fixture da grade cria célula e geradora em todos os dias, usa vínculos adjacentes e não intercala escritores: não cobre célula sem geradora, dia sem célula, zero vínculos, sobreposição simultânea ou alteração durante a grade. Os testes de perguntas usam `_frescas` com célula antecipada, sem exercitar a diferença de carregamento de `perguntas_do_app`. (app/escala/tests/test_snc2_grade_previsto_em_lote.py:53–80,96–109; app/chamados/tests/test_snc2_toque_em_lote.py:75–85)

**5. Triagem v2, linha por linha.**

| Linha fornecida | Conferência |
|---|---|
| Cabeçalho: conferido no 76626871 | Identidade histórica e conferência Git independente: [nao verificado]. |
| Item 1: tranca entre colaboradores, “curado” | **Sustentado para essa causa.** `lote=False` retira empresa/tranca do lote; os dois REDs usam outro colaborador e portas reais. Execução de `red_p1c.log`: [nao verificado]. (app/chamados/juizes.py:940–948; app/chamados/tests/test_snc2_toque_em_lote.py:171–198) |
| Item 2: corte da empresa, mesma cura, sem RED próprio | **Sustentado para o compartilhamento entre colaboradores frescos.** A empresa sai do dono lote. O arquivo novo não exercita mudança de corte durante a rodada. (app/chamados/juizes.py:940–944; app/chamados/tests/test_snc2_toque_em_lote.py:102–243) |
| Item 3: célula antecipada, descartado por compartilhar fotografia dos carimbos | **Descarte não se sustenta.** Os carimbos antigos podem ser iguais nos dois caminhos enquanto a célula difere; `furo` retorna viva e `trabalhou` sem pendências retorna morta. (app/chamados/services/pergunta_no_app.py:47–53; app/ponto/services/veredito_celula.py:174–178,325–330; app/chamados/juizes.py:801,1181–1196) |
| Item 4: reabrir/trancar são REDs; paridade/request são guardas | **Sustentado nessa distinção.

[truncado em 20000 de 24570 caracteres]

## 05/11 16:28 · sessao beed8fd989aa · - · - · origem=Ronald (tmux)

Rastreamento consolidado — somente leitura.

- `JANELA_DIAS=14` é lida como default por `pendencias` e `reconciliar_colab`; o comando possui outro default literal `14` em `--dias`, passado aos dois caminhos. (`app/ponto/services/cluster_espurio.py:12,62,85`; `app/ponto/management/commands/detectar_cluster_espurio.py:29,46,50`)
- `dias_com_cluster` lê `CelulaDia.colaborador/data/ata/trabalha`, aplica `dia_sem_trabalho`, extrai `ata.orfas`, conta `ata.lampadas[*].acesa is False` e testa pares de horários ordenados com diferença ≤15. O parâmetro `ec` não participa do corpo. (`app/ponto/services/cluster_espurio.py:15-36`)
- `_vivos_que_a_premissa_matou` lê todo chamado VIVO do módulo para a pessoa, pergunta `dia_do_fato(fortes=True)` e entrega `False` quando a data não está no conjunto encontrado pela consulta recortada. (`app/ponto/services/cluster_espurio.py:39-52`)
- `pendencias` monta `achados`, `na_lista`, `a_abrir`, `a_fechar`; usa julgamento anterior/instante do cartório e `parcial_so_lista`. A lista parcial restringe abertura, mas mantém assinatura sustentando chamado vivo. (`app/ponto/services/cluster_espurio.py:62-82`)
- `reconciliar_colab` exige pessoa ativa e alguma escala ativa sem fim; fecha `a_fechar` por `ch.reconciliar()` e percorre **todos os achados**, não `a_abrir`, chamando `abrir()` em cada dia fora da lista parcial. (`app/ponto/services/cluster_espurio.py:85-124`)
- O escritor do payload cluster fornece `modulo_origem`, `contexto_json.data_turno/orfas/n_missing`, `categoria='batida_espuria'`, `area='supervisao'`, assunto/título/descrição; o gate efetivamente persiste pelo `get_or_create`. (`app/ponto/services/cluster_espurio.py:104-120`; `app/chamados/models.py:932-934`)
- Cartório chama o serviço com default somente sob `apply_` e `_julgadas` não vazio; comando manual `--apply` chama o mesmo serviço; o cron cadastrado executa `--alarme`. (`app/ponto/services/cartorio.py:981-994`; `app/ponto/management/commands/detectar_cluster_espurio.py:34-50`; `app/config/crons.py:598`)
- O cron alarme lê `achados/a_abrir/a_fechar` e lança `SystemExit(2)` quando existem pendências; não chama `reconciliaria()`. (`app/ponto/management/commands/detectar_cluster_espurio.py:50-64`)
- `supra_juiz` não aciona diretamente o serviço cluster: seleciona chamados somente em `FURO_MODULES`, que não inclui cluster; sua escrita delega `retratar` quando a lavra marca `cobranca_indevida`, após guardas. (`app/ponto/management/commands/supra_juiz.py:186-192,246-287`; `app/chamados/catalogo/furo_modulos.py:15-16`)

**Premissa e leitores externos**

- `FATO_CLUSTER_ESPURIO` tem escritor explícito de valor no coletor especializado e leitor em `premissa.avaliar`; o catálogo liga o módulo cluster ao fato e à fonte textual “nas batidas do dia”. (`app/ponto/services/cluster_espurio.py:50`; `app/chamados/catalogo/premissa.py:23,52,82,126-134`)
- O reconciliador genérico também lê a classe cluster: percorre todos VIVOS, chama `_veredito_premissa`, faz probe e resolve fato da classe. Contudo seus ramos coletam turno/âncora/filhos/par/vínculo, sem ramo cluster; termina avaliando com o fato ausente. Assim esse caminho conserva cluster por `None`, ao contrário do coletor especializado. (`app/chamados/reconciliador.py:247-301,1384-1395`; `app/chamados/catalogo/premissa.py:127-130`)
- Leitores externos diretos de `premissa_morta`: `feriado_reavaliar`, `silencio_ausencia`, `triagem_batida`, `cobertura`, `geofence`, detector intervalo, `reconciliar_grade`, alertas turno, detector ausências e `reconciliar_vinculo`. (`app/chamados/services/feriado_reavaliar.py:38`; `app/chamados/services/silencio_ausencia.py:36`; `app/ponto/services/triagem_batida.py:593`; `app/ponto/services/cobertura.py:117,310`; `app/ponto/services/geofence.py:411`; `app/ponto/management/commands/detectar_intervalo_ausente.py:132`; `app/ponto/management/commands/reconciliar_grade.py:298`; `app/ponto/management/commands/processar_alertas_turno.py:207`; `app/ponto/management/commands/detectar_ausencias.py:809`; `app/ponto/management/commands/reconciliar_vinculo.py:49`)
- Outros leitores de `avaliar`: `ChamadoColaborador.premissa_regularizada`, `reconciliar_fantasmas` e o reconciliador genérico; `fato_da_classe` é lido pelo genérico; `fonte_do_fato` é lida por `avaliar`. (`app/chamados/models.py:1257-1266`; `app/ponto/management/commands/reconciliar_fantasmas.py:255-260`; `app/chamados/reconciliador.py:257,262,301`; `app/chamados/catalogo/premissa.py:131`)
- Testes que leem funções cluster: detector original, varredura/evento, parcial na lista, dia sem trabalho e retorno real do gate. (`app/ponto/tests/test_detectar_cluster_espurio.py:8,56,73`; `app/ponto/tests/test_varredura_cluster_evento.py:16,82-84`; `app/ponto/tests/test_cobparc7_cluster_le_a_lista.py:21,40-43`; `app/chamados/tests/test_f1c4_3_emissores.py:114-116`; `app/chamados/tests/test_fechar3_retorno_do_gate.py:205-216`)

**Datas, payload e telas**

- `contexto_json.data_turno` é lido pelo catálogo via chave declarada; `dia_do_fato` prioriza célula ligada, depois catálogo/data_turno, devolvendo `None` no modo forte quando não há data válida. (`app/chamados/catalogo/modulos.py:115-116,219-225`; `app/chamados/juizes.py:89-118`)
- A junção `chamado.celula` é escrita no nascimento a partir do mesmo juiz de data. (`app/chamados/models.py:913-919,1174-1190`)
- O payload `orfas/n_missing` possui leitor genérico de tela que itera todos os itens de `contexto_json`; leitor de negócio externo específico desses dois subcampos do chamado não foi localizado [não verificado]. (`app/templates/chamados/partials/painel_chamado_turno.html:33-37`)
- O painel lateral lê módulo, assunto/título, pessoa, urgência e estado; o endpoint carrega chamado por PK e seus followups. (`app/templates/chamados/partials/painel_chamado_turno.html:8-26`; `app/chamados/views.py:347-355`)
- Outro leitor direto de `data_turno` apresenta a data no cartão. (`app/templates/chamados/partials/_card_atendimento.html:232-235`)
- A fila e seus leitores genéricos respondem ao estado/silenciamento: `qs_na_fila` alimenta painel, contadores e navbar; consumidores incluem `views`, `core/context_processors`, painel operacional, fila por causa, SLA, lastro e propositor. (`app/chamados/services/painel.py:158-183`; `app/chamados/views.py:901`; `app/core/context_processors.py:93,150,187`; `app/core/services/painel_op.py:119`; `app/chamados/services/fila_por_causa.py:160`; `app/chamados/services/sla.py:62`; `app/ponto/services/lastro.py:239`; `app/escala/services/propositor.py:632`)

**Level-triggered e concorrência**

- `_sem_chamado` considera qualquer estado; portanto `a_abrir` pode estar vazio enquanto o ato reapresenta o achado a `abrir`, que tenta renascer um encerrado. O retorno permanece `criado=False`; renascimento não incrementa `abertos`. (`app/ponto/services/cluster_espurio.py:55-59,81,101-123`; `app/chamados/models.py:867-885`)
- `abrir` serializa dedup/create travando a pessoa; existente encerrado passa a `_renascer_por_premissa_viva`. (`app/chamados/models.py:863-885`)
- Renascimento recusa pessoa inativa, via cascade, competência trancada; fechado pelo admin exige fato novo; motor só reabre resolvido/fechado, não cancelado/superado; sucesso escreve ocorrência `renascido`. (`app/chamados/models.py:965-985,1292-1309`; `app/chamados/catalogo/motor.py:113`)
- Cluster não pertence ao veto do dia nem à absolvição de nascimento reservada a módulos de turno. (`app/chamados/models.py:1062-1064,1086-1088`; `app/ponto/services/furos_vetados.py:79-81`; `app/chamados/catalogo/furo_modulos.py:15-16`; `app/chamados/reconciliador.py:336-337`)
- `resgatado_em` posterior à tranca preserva contra o coletor da competência; o coletor cluster não testa esse campo. (`app/chamados/services/competencia_trancada.py:104-105`; `app/ponto/services/cluster_espurio.py:45-50`)
- `reconciliar` entra em atomic, recarrega status e trava a linha; isso impede decidir pelo estado antigo da instância. O gate de premissa retorna imediatamente para chamado fora de furo; o cluster canônico cai nesse retorno e não recoleta sua assinatura após `pendencias`. (`app/chamados/models.py:616-620,637-639,668-691`; `app/ponto/services/cluster_espurio.py:95-99,109`; `app/chamados/catalogo/modulos.py:235-241`; `app/chamados/models.py:1194-1200,2955-2972`)
- Assim, mutação concorrente de ata entre coleta e fechamento não tem nova leitura cluster nesse gate; ocorrência concreta em produção é [não verificado]. (`app/ponto/services/cluster_espurio.py:95-99`; `app/chamados/models.py:680-691`)
- `_gravar_estado` escreve estado, `resolvido_em`, via, urgência e atualização; grava auditoria antes do save no mesmo atomic. O evento RECONCILIAR usa via sistema. (`app/chamados/models.py:222-247,274-279`; `app/chamados/catalogo/motor.py:95`)
- Cluster fecha diretamente por `reconciliar`, enquanto `retratar` possui unidade adicional de justificativa/perguntas/followup; não há followup próprio de retratação no corpo cluster. (`app/ponto/services/cluster_espurio.py:97-99`; `app/chamados/reconciliador.py:503-540`)
- Pergunta viva sem destino impede o fechamento; todas reatáveis podem ser reapontadas. (`app/chamados/models.py:393-425,648-655`)

## 05/11 16:28 · sessao 4d5958956d51 · - · - · origem=Ronald (tmux)

Complemento confirmado: **o save de Ausencia pode adquirir novamente a trava de Colaborador dentro da mesma transação**, via julgamento síncrono e emissão. (`app/ponto/models.py:908`; `app/ponto/signals.py:143`, `:147`, `:93`; `app/ponto/services/cartorio.py:1989`, `:1740`, `:1778`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:863`, `:866`.)

Esse caminho é condicional à existência da célula, ao teto temporal e aos gates de emissão; portanto deve aparecer na ordem completa como **Ausencia.save→[se emitir] Colaborador.select_for_update**, sem afirmar que ocorre em toda gravação. (`app/ponto/services/cartorio.py:1974`, `:1977`, `:1982`, `:1725`, `:1739`, `:1777`.)

Na aprovação atômica de irmãos, a sequência possível é **A1→C→F1→A2→C→F2**; o C vem do signal e o F de `invalidar_fechamento`. (`app/ponto/views.py:2638`, `:2640`; `app/ponto/services/ausencia.py:839`, `:950`; `app/ponto/services/fechamento.py:1640`.)

Contudo, **essa inversão por tabela não prova deadlock deste comando**: aprovação seleciona ausências abertas, enquanto o censo H06 seleciona aprovadas; férias manipula outro tipo. Mesmas linhas concorrentes e ciclo executado continuam **[nao verificado]**. (`app/ponto/views.py:2613`, `:2615`; `app/ponto/management/commands/rederivar_falta_declarada.py:87`; `app/ponto/services/ausencia.py:1187`.)

A rejeição de irmãos não possui o atomic externo da aprovação; não acumular suas iterações como se fossem uma transação única. (`app/ponto/views.py:2697`, `:2698`.)

## 05/11 16:27 · sessao de3a064d2e93 · AUT-H12H06 · censo · origem=Code

**A causa dominante é DESENHO:** a proteção da escrita reaparece incompleta na reversão, na entrega concorrente e, depois, no aplicar após transferência de empresa. Essa classificação é uma inferência causal da sequência documentada, sustentada pelos bloqueantes de `codex_v1.r` itens 1/3, `codex_v2.r` §2 e `codex_v3.r` bloqueante 1; na cópia atual, essas responsabilidades aparecem em `app/ponto/management/commands/rederivar_falta_declarada.py:181`, `:189`, `:246` e `:258`.

Conto **12 unidades procedentes**, preservando o agrupamento das triagens: quatro na v1, cinco na v2 — incluindo o grupo “x→w / tela” — e três na v3. Os descartes ficam fora da contagem (`triagem_v1.txt`, trecho “PROCEDENTES 1…4”; `triagem_v2.txt`, bloco “PROCEDENTES”; `triagem_v3.txt`, “PROCEDENTE 1”, “2” e “3”). As referências históricas abaixo vêm dos anexos fornecidos; somente a cópia v4 foi inspecionada diretamente. Execução dos testes e ocorrência em produção: **[nao verificado]**. [nao verificado]

| Procedente | Classe | Causa e sustentação |
|---|---|---|
| **v1/1 — reversão sem guarda de entrega/estado** | **DESENHO** | A reversão foi constituída como reposição de número por PK, sem carregar as condições que tornavam aquela escrita permitida. `codex_v1.r`, bloqueante 1: “busca por PK e repõe minutos sem conferir competência, identidade complementar ou estado esperado”, referindo `app/ponto/management/commands/rederivar_falta_declarada.py:161` e `app/ponto/services/ausencia.py:528`. A recorrência na v2 confirma que o problema excedia acrescentar uma comparação. |
| **v1/2 — arquivo sobrescrevível** | **IMPLEMENTACAO** | Nome por segundo e abertura destrutiva contrariavam a finalidade já estabelecida do arquivo de reversão. `triagem_v1.txt`: “arquivo 'w' por segundo”; referências históricas `app/ponto/management/commands/rederivar_falta_declarada.py:122` e `:124`. O detalhe corrigido aparece na v4 no nome em `:169` e na abertura exclusiva em `:172`. |
| **v1/3 — ausência de atomicidade; concorrência e ranges** | **DESENHO** | A consulta da entrega antecedia uma escrita desprotegida e abrangia menos competências que o registro alterado. `triagem_v1.txt`: “sem atomic e range que cruza comp”; referências históricas `app/ponto/management/commands/rederivar_falta_declarada.py:52`, `:114`, `:132`. A forma da proteção precisou mudar novamente na v2 para incluir o escritor da entrega. |
| **v1/4 — leitor H12 não migrado** | **IMPLEMENTACAO** | O requisito já exigia censo dos leitores; contador, filtro e badge ficaram com a condição antiga. `codex_v1.q`: “censo dos leitores do motivo”; `triagem_v1.txt`: “leitor H12 nao migrado”, em `app/ponto/views.py:1215` e `app/templates/ponto/_fech_linha.html:2`. O requisito também está em `app/docs/PROMPTS.md:1860`. |
| **v2/1a — reversão sobre tipo/status posteriormente alterado** | **DESENHO** | A prova por igualdade de minutos não identificava suficientemente o estado alterado pelo ato. `codex_v2.r`, contraexemplo do item 1: falta420 convertida em atestado420 recebia720; referências históricas `app/ponto/management/commands/rederivar_falta_declarada.py:224`, `:226` e `app/ponto/services/ausencia.py:531`. `triagem_v2.txt`: “identidade do ato no arquivo e nas duas pontas”. |
| **v2/1b — transferência; guarda consulta empresa do JSON** | **DESENHO** | A guarda usava empresa congelada no arquivo, enquanto o pertencimento do colaborador podia mudar. `triagem_v2.txt`: “guarda usava a empresa do JSON”; `codex_v2.r`, contraexemplo A→B, em `app/ponto/management/commands/rederivar_falta_declarada.py:228` e `:234`. A possibilidade de alterar empresa permanece explícita em `app/colaboradores/services/colaborador.py:33` e `:55`. |
| **v2/3 — entrega sem trava comum com o ato** | **DESENHO** | `atomic` e trava de ausência não ordenavam a operação perante o escritor da entrega. `codex_v2.r`, contraexemplo do item 3: “Não há trava compartilhada”; referências históricas `app/ponto/management/commands/rederivar_falta_declarada.py:185` e `app/folha/models.py:182`. `triagem_v2.txt`: “entrega sem trava comum com o ato”. |
| **v2 — tudo-ou-nada sem prova de rollback** | **FERRAMENTA** | O teste provocava recusa antes da primeira escrita; podia passar sem transação. `codex_v2.r`, tabela de mordida: “Sim, retirando somente atomic”; `triagem_v2.txt`: “tudo-ou-nada sem prova por rollback real”. O caso antigo está em `app/ponto/tests/test_h06_reversao_guardas.py:120`; a falha na segunda gravação aparece em `:219`. |
| **v2 — grupo x→w / tela não renderizada** | **FERRAMENTA** | O teste não isolava a abertura exclusiva mantendo o nome único; o teste H12 inspecionava o dict sem executar os consumidores HTML. `triagem_v2.txt`: “caso x->w sem mordida / tela nao renderizada”; `codex_v2.r`, tabela de mordida. Sítios em `app/ponto/tests/test_h06_reversao_guardas.py:130` e `:173`; casos posteriores em `:230` e `:267`. |
| **v3/1 — aplicar protege empresa antiga** | **DESENHO** | A proteção fora acrescentada à reversão, mas o aplicar continuava escolhendo empresa/trava pelo censo anterior. `triagem_v3.txt`: “aplicar nao reconfere a empresa de HOJE (…) _reverter faz”; referências históricas `app/ponto/management/commands/rederivar_falta_declarada.py:175`, `:194`, `:244`. É a mesma responsabilidade distribuída de maneira assimétrica entre as duas operações. |
| **v3/2 — D3 falso; contador sem caso** | **FERRAMENTA** | A triagem tomou prova do atributo do partial como prova da soma executada pela view. `codex_v3.r`, D3: “Alterar n_rescisao para0 mantém esse teste verde”; `triagem_v3.txt`: “o contador (…) nao tinha caso”. Produtor em `app/ponto/views.py:1215`; teste limitado ao partial em `app/ponto/tests/test_h06_reversao_guardas.py:267` e `:272`. |
| **v3/3 — mutantes de identidade/trava/ordem sobreviventes** | **FERRAMENTA** | Faltavam casos discriminantes para status/data_fim, trava da reversão e reconferência posterior à trava. `triagem_v3.txt`, procedente 3; seu último parágrafo registra que “status/data_fim (…) ja verdes no RED”, portanto nem todos eram defeitos produtivos novos. Guardas em `app/ponto/management/commands/rederivar_falta_declarada.py:44`, `:184`, `:255`; provas adicionais em `app/ponto/tests/test_h06_reversao_guardas.py:309`, `:331`, `:365`, `:369`. |

**(a)** Há uma recorrência principal de **DESENHO**, acompanhada de ocorrências de implementação e ferramenta:

- **v1:** condições de reversão e entrega fora da escrita protegida — referências históricas `app/ponto/management/commands/rederivar_falta_declarada.py:114`, `:132`, `:161` (`codex_v1.r`, bloqueantes 1/3).
- **v2:** proteção acrescentada ainda insuficiente quanto à identidade, empresa e serialização da entrega — referências históricas do mesmo arquivo `:224`, `:228`, `:185`, mais `app/folha/models.py:182` (`codex_v2.r`, §2).
- **v3:** empresa protegida na reversão, mas não reconferida no aplicar — referências históricas do mesmo arquivo `:175`, `:194`, `:244` (`codex_v3.r`, bloqueante 1).

O arquivo sobrescrevível e o leitor omitido são falhas de implementação distintas dessa cadeia; as provas insuficientes têm recorrência própria de ferramenta, conforme as linhas correspondentes da tabela (`app/ponto/management/commands/rederivar_falta_declarada.py:172`; `app/ponto/services/fechamento_lista.py:109`; `app/ponto/tests/test_h06_reversao_guardas.py:120`, `:267`).

**(b) Sim, os três procedentes da v3 já estavam em temas presentes anteriormente.**

- **Transferência no aplicar:** a v1 perguntava por concorrência e cadastro atual; na v2, transferência A→B já era contraexemplo explícito, mas a cura ficou concentrada na reversão (`codex_v1.q`, perguntas 4/7; `codex_v2.r`, contraexemplo de empresa; referências históricas `app/ponto/management/commands/rederivar_falta_declarada.py:167`, `:228`).
- **Contador:** a lacuna já fora vista na v2, mas descartada com justificativa que a v3 demonstrou falsa (`triagem_v2.txt`, D3; `codex_v3.r`, §2; `app/ponto/views.py:1215`; `app/ponto/tests/test_h06_reversao_guardas.py:272`).
- **Identidade/travas/mordida:** já eram perguntas expressas da v2; a v3 encontrou dimensões ainda não discriminadas pelos testes (`codex_v2.q`, perguntas 2/3e/3f; `codex_v3.r`, §4; `app/ponto/tests/test_h06_reversao_guardas.py:193`, `:202`, `:249`).

**(c) Sim, ainda há sítios abertos, embora os contraexemplos produtivos aceitos tenham recebido guardas na v4.**

O aplicar agora confere identidade e empresa atual sob trava antes de escrever; a reversão também confere identidade, empresa, minutos e entrega. O registro do TXT compartilha a trava da Empresa (`app/ponto/management/commands/rederivar_falta_declarada.py:183`, `:189`, `:199`, `:255`, `:263`; `app/folha/models.py:181`).

Permanece um sítio de **FERRAMENTA**: os testes reconhecem tabela, `FOR UPDATE` e ordem, sem comprovar qual Empresa foi efetivamente travada. O sobrevivente “Empresa errada ou inexistente” já constava de `codex_v3.r`, §4, e essa limitação permanece nas verificações atuais. Sobrevivência deduzida estaticamente; execução do mutante: **[nao verificado]** (`app/folha/export.py:565`; `app/ponto/tests/test_h06_reversao_guardas.py:249`, `:350`, `:357`).

Também permanece um limite de **DESENHO** na comprovação exclusiva do ato: aplicar R1→reverter R1→aplicar R2 pode devolver exatamente a mesma identidade/empresa/grade, permitindo usar novamente R1. O arquivo é lido, sem consumo, e a aceitação depende do estado corrente; esse limite já aparecia em `codex_v2.r`, §5, “não há estado de consumo do arquivo”. Não o acrescento à contagem histórica. Ocorrência e prejuízo em produção: **[nao verificado]** (`app/ponto/management/commands/rederivar_falta_declarada.py:87`, `:236`, `:255`, `:261`, `:269`).

CLASSE DOMINANTE = desenho — implementacao: 2; desenho: 6; ferramenta: 4.

## 05/11 16:27 · sessao c20309f6322a · - · - · origem=Ronald (tmux)

**Recompute e geometria**

- `recompute_turnos(colab,ini,fim)` lê `turnos_do_colab`, `escalas_no_periodo` e `dt_fim_previsto_de`; escreve `TurnoMaterializado.colaborador`, `data_turno`, `entrada`, `saida`, `aberto`, `cross_meianoite`, `dt_fim_previsto`; apaga e recria somente `[ini,fim]` em `atomic`, devolve quantidade; seu corpo não emite evento. A preparação das linhas ocorre antes do `atomic`. `app/ponto/nucleo.py:40`, `:45`, `:48`, `:57`, `:61`.
- `escalas_no_periodo` delega a `escalas_do_periodo` e ordena por `(ativa,data_inicio)`; `te_vigente_em` delega candidatura a `vinculo_cobre_a_data`, mantém o último candidato; `dt_fim_previsto_de` pergunta à janela do tipo vigente na data do turno. `app/ponto/nucleo.py:7`, `:15`, `:18`, `:23`, `:31`, `:37`.
- Chamadores produtivos diretos: save/delete de batida, janela `data_local(timestamp)-2..+1`; save de vínculo, `hoje-2..+1`; alertas turno, `agora-2..+1` de quem tem batida nas últimas 36h; alertas avançados, `ontem-1..hoje`; veredito humano, `data-1..+1`; flip, dia do materializado da batida ou hoje, `-2..+2`; backfill explícito, `hoje-dias..hoje`. `app/ponto/signals.py:19`, `:26`, `:34`, `:48`, `:53`, `:64`; `app/ponto/management/commands/processar_alertas_turno.py:59`, `:67`; `app/ponto/management/commands/processar_alertas_avancados.py:57`; `app/ponto/services/veredito_celula.py:308`; `app/ponto/services/flip_batida.py:37`, `:39`; `app/ponto/management/commands/materializar_turnos.py:19`, `:24`.
- Leitores diretos além dos emissores: `turnos_abertos_de` lê entrada/saída e dias `(dia-1,dia)` e aplica veto posicional; `turno_do_chamado_aberto` recorta pela entrada normalizada do contexto, senão data; `_tem_turno_aberto` usa janela com dia, mas sem dia usa existência global. `app/chamados/reconciliador.py:95`, `:111`, `:117`, `:139`, `:145`, `:161`, `:173`.
- Outros leitores: `turno_aberto_ha` lê aberto/entrada e calcula idade; `turnos_em_turno_stored` lê aberto, entrada, data e fim previsto, com margem e jornada ativa; `core.views` usa seu wrapper `colabs_em_turno_stored`; via de resolução de pergunta lê saída do materializado do dia; flip lê o turno que contém a batida; verificador compara os seis campos geométricos com rederivação; auditor de invariantes lê existência global de `aberto=True`. `app/ponto/turnos.py:3115`, `:3122`; `app/ponto/selecao_periodo.py:256`, `:270`, `:278`, `:287`; `app/core/views.py:1159`; `app/chamados/juizes.py:1420`; `app/ponto/services/flip_batida.py:35`; `app/ponto/management/commands/verificar_nucleo.py:26`; `app/chamados/management/commands/auditar_invariantes_chamados.py:602`.
- Definição do modelo: `atualizado_em` é `auto_now`; não há `unique_together` nem constraint de unicidade em seu `Meta`, somente índices. O corpo de recompute não toma `select_for_update`. Ocorrência produtiva de corrida/duplicata: [nao verificado]. `app/ponto/models.py:1196`, `:1211`, `:1213`; `app/ponto/nucleo.py:40`.

**Evento e consumidor existente**

- `EventoCelula` guarda `celula_id`, `colaborador_id`, `data`, `criado_em`, `processado_em`, `tentativas`, `erro`. `celula_id` é inteiro sem FK; não há payload nem índice único de pendente; o comentário explica sobrevivência à regeneração e preservação de fato novo concorrente. `app/escala/models.py:1845`, `:1848`, `:1853`, `:1854`, `:1862`.
- Escritor do nascimento do evento: `emitir_evento_celula`, `objects.create` dos três identificadores; `criado_em` e defaults completam a linha. Chamadores: `salvar_e_emitir`, `abrir` e `semear_eventos_celula`. `app/ponto/portas/celula.py:634`, `:639`; `app/chamados/models.py:939`; `app/chamados/reconciliador.py:1254`, `:1265`, `:1278`.
- `salvar_e_emitir` salva célula e evento no mesmo `atomic`, somente quando os campos lavrados incluem `veredito` ou `ata`. É chamado por lavra, carimbo e precedência; carimbo com só metadados ou status sozinho não acorda o consumidor. `app/ponto/portas/celula.py:630`, `:642`, `:646`, `:648`, `:751`, `:768`, `:783`, `:815`, `:830`.
- `abrir` emite quando criou chamado cujo módulo está em `MODULOS_DE_TURNO` e tem célula; `criar` cria, trilha e retorna sem emitir; reabrir não aparece entre chamadores da função emissora. `app/chamados/models.py:932`, `:939`, `:1048`; `app/ponto/portas/celula.py:634`.
- Consumidor: `chamados.reconciliador.consumir_eventos_celula`, chamado pelo command homônimo a cada minuto e pelo dreno do reconciliador de 15 minutos. Ambos envolvem chamada com `consumidor_exclusivo`. `app/chamados/reconciliador.py:1283`; `app/chamados/management/commands/consumir_eventos_celula.py:17`, `:31`; `app/chamados/management/commands/reconciliar_chamados.py:60`; `app/config/crons.py:394`, `:395`.
- Exclusão usa `pg_try_advisory_lock`, chave `7190000+crc32(schema)%1000`; a função de consumo não adquire essa trava internamente. Colisão de chaves entre schemas é matematicamente possível; ocorrência produtiva: [nao verificado]. `app/chamados/reconciliador.py:1180`, `:1183`, `:1189`, `:1192`, `:1203`, `:1283`.
- Pendentes = não processados e tentativas `<5`; mortos = não processados e `>=5`; leitura ordena por criação/PK, lote default 500, agrupa por colaborador, encerra com fila vazia ou após limite temporal de 45s checado depois do lote. `app/chamados/reconciliador.py:1177`, `:1179`, `:1214`, `:1220`, `:1283`, `:1309`, `:1315`, `:1351`.
- Para cada colaborador chama **somente** `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`; não chama `reconciliar_chamados_colab`. Usa colaborador do evento; não filtra trabalho pelo dia nem pelo ID da célula do evento. `app/chamados/reconciliador.py:1315`, `:1322`, `:1326`, `:1329`, `:1357`.
- Falha mantém pendentes os eventos lidos daquele colaborador, incrementa `tentativas` com `F` e escreve erro truncado; adiamento mantém pendente sem tentativa; sucesso marca `processado_em`; exclusão remove processados com mais de sete dias. Evento inserido depois da leitura do lote não está nos IDs atualizados por esse lote. `app/chamados/reconciliador.py:1318`, `:1337`, `:1340`, `:1341`, `:1346`, `:1353`.
- `criado_em` é lido pela ordenação/idade; `celula_id` pelo vigia e semeadura; `processado_em`/`tentativas` pelos filtros; `erro` tem escrita demonstrada e nenhum leitor produtivo explícito localizado por referência ao modelo [nao verificado quanto a leitura dinâmica/admin]. `app/chamados/reconciliador.py:1217`, `:1229`, `:1241`, `:1263`, `:1309`, `:1340`.

**Gates e contraexemplos**

- Célula sem veredito, célula ausente ou célula acusadora não absolve; mesmo veredito que absolve não basta se há `marcos_pendentes`. Esse ramo não testa explicitamente existência de ata: quem lê a ata é `marcos_pendentes`. `app/chamados/reconciliador.py:986`, `:995`, `:997`.
- `_retratar` pode reter por irmã respondida sem consumo e por justificativa alheia pendente durante outra aprovação; só depois entra em `atomic` e chama `reconciliar`. O gate de fechamento ainda pergunta premissa e pergunta viva. `app/chamados/reconciliador.py:437`, `:446`, `:451`, `:518`, `:522`; `app/chamados/models.py:648`, `:651`.
- Concorrência de fechamento: `reconciliar` abre `atomic`; recarregamento dentro de transação usa `select_for_update` da linha do chamado. Portanto uma instância antiga não decide fechamento só pelo estado que carregou. `app/chamados/models.py:616`, `:638`, `:661`, `:673`.
- O consumidor não retrata diretamente `disputa_supervisao/orfao_14h`: a função de chamados restringe a módulos de turno; pode encerrar órfãs `orfao_14h`/`saida_sem_entrada`, fechar disputa esvaziada e chegar ao pai por signal. `app/chamados/reconciliador.py:964`, `:1008`, `:1138`; `app/chamados/signals.py:42`.
- Órfã com marco aceso não morre por isso sozinho: resposta do colaborador, `pergunta_viva`, dia de hoje, fim previsto e dia encerrado participam. Se só relógio segura, evento fica pendente; ausência de escala/previsto não é adiada indefinidamente: pergunta fica viva e evento pode ser marcado processado. `app/chamados/reconciliador.py:1050`, `:1052`, `:1054`, `:1056`, `:1058`, `:1065`, `:1071`, `:1341`.
- Fila vazia não causa visita ao colaborador; recompute sem evento não é alcançado pelo consumidor por si mesmo. Lavra idêntica retorna sem save/evento; status sozinho também não o cria. `app/chamados/reconciliador.py:1309`; `app/ponto/nucleo.py:57`; `app/ponto/portas/celula.py:761`, `:764`, `:768`.
- Dia fora da janela do recompute conserva o materializado anterior; save de vínculo usa janela ancorada em hoje, portanto não recompõe por esse receiver toda a competência histórica. `app/ponto/nucleo.py:58`; `app/ponto/signals.py:63`.
- Cross meia-noite exige recorte pelo fato: `turnos_abertos_de(d)` olha véspera e dia, mas `turno_do_chamado_aberto` exige entrada denunciada se disponível; outro aberto da véspera não basta. Sem data, `_tem_turno_aberto` volta ao global. `app/chamados/reconciliador.py:98`, `:145`, `:152`, `:173`.

**Selo estrutural e teto de queries**

- Chave relevante ao rebaixamento é `('chamado','um juiz por pergunta')`, teste `JuizChamadoContratoTest`, exceções `JUIZES_POR_VARREDURA` e `PENDENTES_CHAMADO`; `verde` usa default False; nota exige O168/O169/LASTRO e proíbe reclassificação de papel para torná-la verde. O módulo não declara IDs numéricos por célula: equivalência do rótulo “17” vem do dado do usuário, não de enumeração explícita da cópia. `app/core/contratos_estruturais.py:62`, `:90`, `:261`, `:269`, `:1048`.
- O selo da matriz verifica existência da classe declarada e listas de exceções vazias para verde; não executa nessa verificação a classe nomeada. O tabuleiro exige que a célula chamado/juiz declare `JUIZES_POR_VARREDURA` e não esteja verde com lista cheia. `app/core/tests/test_selo_contratos_estruturais.py:71`, `:81`; `app/core/tests/test_contract_tabuleiro.py:41`.
- `JuizChamadoContratoTest` mede autoridade declarada existente, SEM_JUIZ vazio, impressões pendentes presentes, nenhuma nova resposta fora da autoridade por censo, anticópia de data crua/SLA/COBRANCA e teto zero de `PENDENTES_CHAMADO`. Não prova cobertura de produtores nem consumo do outbox. `app/chamados/tests/test_contract_juiz_chamado.py:24`, `:29`, `:37`, `:43`, `:49`, `:59`, `:71`, `:85`, `:98`.
- O **outro 17** é teto SQL de `_gravar_estado (sem pergunta)`; fixture nasce via `criar` sem contexto/dia/célula explícitos, captura `ch.reconciliar`, só compara quantidade de queries `<=17`; não verifica retorno nem estado final. Contagem de registros `on_commit` só se aplica às chaves da batida, não a esta. `app/ponto/tests/test_selo_chokepoint_escrita.py:112`, `:177`, `:212`, `:225`, `:286`. O aval antigo 17→18 está documentalmente em `app/docs/PROMPTS.md:1853`, mas o executável permanece 17 em `app/ponto/tests/test_selo_chokepoint_escrita.py:112`.

**Irmãos concretos**

- Todos os chamadores de recompute acima repetem a mesma saída sem evento da porta; destaque save de vínculo sem batida, veredito humano, flip e backfill. `app/ponto/signals.py:64`; `app/ponto/services/veredito_celula.py:308`; `app/ponto/services/flip_batida.py:39`; `app/ponto/management/commands/materializar_turnos.py:24`; `app/ponto/nucleo.py:57`.
- Rejuntar célula de chamado continua trabalho separado no cron, não na função reativa por batida. `app/chamados/management/commands/reconciliar_chamados.py:50`, `:56`; `app/chamados/reconciliador.py:1378`.
- Auditor I3 usa `aberto=True` global por colaborador, enquanto o julgamento específico recorta entrada/dia: pode não denunciar chamado antigo sustentado por outro turno aberto. É diferença de leitor demonstrada, ocorrência produtiva [nao verificado]. `app/chamados/management/commands/auditar_invariantes_chamados.py:602`; `app/chamados/reconciliador.py:145`.
- `turnos_em_turno_stored` usa jornada de vínculo `ativa=True` atual para liveness; recompute usa tipo vigente na data para fim previsto. Troca de vínculo pode alterar um lado sem coincidir com o outro, ocorrência produtiva [nao verificado]. `app/ponto/selecao_periodo.py:278`, `:287`; `app/ponto/nucleo.py:36`.

## 05/11 16:27 · sessao 361ab4d25c20 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura estática; incidência e números atuais de produção [não verificado].

**Irmão exato do defeito**

- `detectar_intervalo_ausente` consulta células apenas em `ref-(dias-1)..ref`, com padrão de dois dias; transforma a falta de um dia no conjunto recortado em `FATO_FURO_INTERVALO=False`, embora percorra todos os chamados vivos do módulo, e tenta reconciliar. (`app/ponto/management/commands/detectar_intervalo_ausente.py:44`, `:46`, `:95`, `:119`, `:123`, `:132`, `:138`)
- Seu emissor está aposentado, mas a reconciliação do legado permanece; `--alarme` força leitura seca, enquanto execução normal pode escrever. (`app/ponto/management/commands/detectar_intervalo_ausente.py:107`, `:138`, `:149`, `:154`)

**Contraexemplos e RED sintéticos**

- **Borda exata:** para `ref=05/11/2026` e padrão 14, a consulta inclui 23/10 a 05/11; um cluster vivo de 22/10, com assinatura preservada, sai do conjunto sem sua célula ser consultada e vira candidato ao fechamento. A regra exige preservá-lo: idade não constitui lastro. (`app/ponto/services/cluster_espurio.py:12`, `:21`, `:23`, `:45`, `:50`; `app/docs/BACKLOG.md:424`)
- **RED de quinze dias:** cadastro ativo com vínculo ativo sem fim; chamado vivo e célula com assinatura em `hoje-15`; ato de julgar outra célula ou executar comando com padrão 14; pela regra, o chamado antigo deve continuar vivo. (`app/ponto/services/cluster_espurio.py:91`, `:93`, `:95`, `:98`; `app/ponto/services/cartorio.py:990`; `app/docs/BACKLOG.md:424`)
- **Célula inexistente ou sem ata:** ambas produzem ausência de assinatura no coletor atual; isso não demonstra morte por lastro. O catálogo conserva quando o fato é `None`, e o leitor genérico do dia distingue explicitamente desconhecimento de regularização. (`app/ponto/services/cluster_espurio.py:23`, `:24`, `:30`, `:31`, `:50`; `app/chamados/catalogo/premissa.py:127`; `app/chamados/juizes.py:457`, `:471`)
- **Competência 21→20:** cadastro com corte 21, cluster em 21/10 e competência 11 aberta; avançar o calendário ou ter competência anterior trancada não demonstra morte desse cluster. O calendário manda 21/10 para 21/10–20/11; a via de tranca verifica competência efetiva e existência da tranca. (`app/ponto/janelas.py:10`, `:24`, `:27`; `app/chamados/services/competencia_trancada.py:82`, `:86`, `:92`, `:96`; `app/docs/BACKLOG.md:424`)
- **Cross-meia-noite:** ata sintética do mesmo turno com órfãs `23:58` e `00:03`, mais uma lâmpada apagada: são cinco minutos reais, mas o código ordena minutos do relógio e compara `1438-3`, perdendo a assinatura. O encadeamento cronológico já existe nos marcos do turno; ocorrência real desse formato na ata [não verificado]. (`app/ponto/services/cluster_espurio.py:30`, `:33`, `:34`; `app/escala/servico_jornada.py:61`, `:80`)
- **Vínculo trocado:** cadastro cujo vínculo antigo gerou a célula e foi encerrado, com novo vínculo ativo; o coletor lê a célula por pessoa/data e ignora o parâmetro `ec`. Se não existir nenhum vínculo atualmente ativo sem fim, a reconciliação inteira retorna antes de examinar o cluster antigo. Pela regra, troca de vínculo isoladamente não constitui lastro de morte. (`app/ponto/services/cluster_espurio.py:15`, `:23`, `:73`, `:93`; `app/escala/servico_jornada.py:29`, `:55`; `app/docs/BACKLOG.md:424`)
- **Concorrência de fato:** `pendencias` decide candidatos antes de `reconciliar`; o fecho trava/recarrega o estado do chamado, mas a guarda genérica retorna autorização para classes que não sejam de furo. Assim, um cenário RED deve distinguir atualização concorrente da assinatura na ata de atualização concorrente do estado; ocorrência real [não verificado]. (`app/ponto/services/cluster_espurio.py:95`, `:97`; `app/chamados/models.py:616`, `:638`, `:668`, `:690`)
- **Pergunta viva:** candidato não equivale a encerramento consumado: a guarda da pergunta viva ainda pode recusar o fecho. (`app/chamados/models.py:651`, `:654`)

**Cobertura que já existe na cópia**

- O teste do evento cobre nascimento, segunda execução sem duplicação e fechamento após retratação das espúrias. Não apresenta envelhecimento de quinze dias nesses casos. (`app/ponto/tests/test_varredura_cluster_evento.py:36`, `:44`, `:50`)
- A base usa cadastro 12×36 noturno 19–07, dia do cluster em `hoje-4`, e cobre rajada e turno normal. (`app/ponto/tests/test_detectar_cluster_espurio.py:21`, `:24`, `:35`, `:47`, `:59`)
- O teste de parcial sem período demonstra que assinatura viva não obriga nascimento: a assinatura continua presente, mas a regra manda lista do admin, sem chamado novo nem pendência de abertura. (`app/ponto/tests/test_cobparc7_cluster_le_a_lista.py:3`, `:31`, `:39`, `:40`, `:41`, `:43`)
- O RED de concorrência existente cobre instância velha de chamado já resolvido: segundo fecho retorna falso e deixa uma única trilha; isso não testa mudança concorrente da ata. (`app/chamados/tests/test_reconciliar_le_o_banco.py:29`, `:32`, `:33`, `:34`)
- Medição atual dos trinta dias e resultado de execução do RED de quinze dias: [não verificado].

## 05/11 16:27 · sessao 02389f37b179 · CODEX-PRE-DESENHO-PILOTO · leitura · origem=Code

**Cobre parcialmente.** Há omissões e um desvio declarado no O169; as proteções de fila, pouso aprovado e verificador estão escritas, mas a espera do wrapper pode atrasar chamadas. (contrato.txt:2–3; contrato.txt:5; avais.txt:2; bin/codex_pergunta.sh:137–175)

Referências aos anexos seguem suas cinco e duas linhas, respectivamente.

1. **Item do aval sem linha correspondente no contrato**

   O papel `censo` aparece, mas faltam seus conteúdos exigidos: **escreve/lê, juiz/lei, contraexemplo, RED e irmãos**. A peça 2 tem o gatilho temporal, porém não declara a autópsia nem sua classificação em **implementação/desenho/ferramenta**. (contrato.txt:1; avais.txt:2)

   Faltam as restrições expressas **“não regra”, “nenhuma lei nova” e “CLAUDE.md intocado”**. O contrato também não repete **teto 2 e proteção nominal de F3/H12+H06**; “fila não se reordena” cobre a ordem, não explicita o teto. Essas ausências não demonstram descumprimento efetivo. (contrato.txt:1–5; avais.txt:1–2)

2. **Linha do contrato que vai além do aval**

   **Linha 5: exceção O169.** Entregar o PD por SendMessage depois de aberto o construtor contraria a precedência “antes do construtor”. Declarar o desvio não equivale a autorizá-lo; reaval dessa exceção: **[nao verificado]**. (contrato.txt:5; avais.txt:2)

   A linha 3 acrescenta **“sem código”** e estende **“sem desenho”** indistintamente às duas peças; o aval explicita “sem desenho” na peça 1. São restrições adicionais, sem prova de que impeçam a autópsia pedida. (contrato.txt:3; avais.txt:2)

3. **Onde pode travar ou atrasar fila, pouso ou verificador** [nao verificado]

   **Espera anterior ao timeout:** o wrapper aguarda o gate sem prazo e procura slot em laço. O timeout padrão de 900 segundos só limita separadamente a trava da resposta e a execução do Codex. Assim, “falha → segue” não limita toda a espera; PD/AUT disputam os mesmos slots usados pelo verificador. (contrato.txt:1–3; bin/codex_pergunta.sh:46; bin/codex_pergunta.sh:137–175)

   **Negativa antes da execução:** o disjuntor pode negar consultas por orçamento, inclusive `censo`. O contrato não explicita essa negativa anterior ao wrapper, situação em que não chega seu rc 75. (contrato.txt:3; bin/disjuntor.py:1373–1388)

   **Forma da espera:** “o construtor espera” não esclarece como isso ocorre; a regra vigente exige declarar como, sem agente ocioso ou turno vazio. Ociosidade efetiva: **[nao verificado]**. (contrato.txt:2; app/docs/PISTA.md:171–172)

   **Pouso aprovado está expressamente dispensado da espera.** Contudo, “medida por obra no pouso” não esclarece o tratamento de medida ainda indisponível. Transformá-la em condição de pouso contrariaria aquela dispensa; bloqueio efetivo: **[nao verificado]**. (contrato.txt:2; contrato.txt:4) [nao verificado]

   A autópsia na segunda reprovação não libera a parada vigente na primeira reprovação final; resposta `censo` também não entra na triagem formal de verificador. O contrato não concede essa liberação. (contrato.txt:1; contrato.txt:3; bin/disjuntor.py:76; bin/disjuntor.py:798–799)

4. **Medida sem origem identificada em arquivo/log**

   A linha 4 não nomeia fontes nem a ligação entre registros para **voltas Codex/opus até aprovação, tokens do construtor, tokens do PD e classificação útil/prevista/nova**. Também não aponta o registro que sustentaria a classificação dos achados. (contrato.txt:4)

   Há fontes possíveis no código: o wrapper registra chamadas, obra, papel, sessão e rc; o leitor de tokens cruza `chamadas.log` com sessões, mas agrega por papel. O contrato não declara esse uso nem a associação entre `PD-<obra>`, `AUT-<obra>` e a obra principal. (contrato.txt:1; contrato.txt:4; bin/codex_pergunta.sh:53–60; bin/codex_limite_estado.py:185–212)

   **“Obras de 05/11 já no RELATO”** identifica apenas uma referência geral, sem selecionar obras, linhas e valores comparáveis. RELATO é também o destino da tabela, não a origem demonstrada de todos os números; disponibilidade desses dados: **[nao verificado]**. (contrato.txt:4–5)

## 05/11 16:27 · sessao 48628233a7df · - · - · origem=Ronald (tmux)

Censo dos produtores e leitores, somente código; ocorrência e frequência em produção permanecem [nao verificado].

- `abrir` escreve nascimento após consultar motor, resolver célula, absolvição e veto; serializa pelo colaborador, cria, registra trilha e só emite `EventoCelula` quando o módulo integra `MODULOS_DE_TURNO` e existe `celula_id`. `app/chamados/models.py:863`, `app/chamados/models.py:894`, `app/chamados/models.py:919`, `app/chamados/models.py:923`, `app/chamados/models.py:928`, `app/chamados/models.py:932`, `app/chamados/models.py:939`.
- `criar` consulta os mesmos juízes do nascimento e grava `status_local` pelo motor; seu corpo termina em `create`, trilha e retorno, sem emissão de `EventoCelula`, sem dedupe e sem `atomic` próprio. `app/chamados/models.py:1015`, `app/chamados/models.py:1021`, `app/chamados/models.py:1028`, `app/chamados/models.py:1032`, `app/chamados/models.py:1036`, `app/chamados/models.py:1048`.
- `_gravar_estado` escreve `status_local`, `resolvido_em`, `via_encerramento`, urgência/restauração e, opcionalmente, usuário; recusa terminal com perguntas vivas ancoradas, grava trilha e estado dentro de `atomic`. Seus chamadores internos são `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, encerramento e competência trancada. `app/chamados/models.py:215`, `app/chamados/models.py:222`, `app/chamados/models.py:274`, `app/chamados/models.py:433`, `app/chamados/models.py:620`, `app/chamados/models.py:751`, `app/chamados/models.py:769`, `app/chamados/models.py:1382`, `app/chamados/models.py:1538`, `app/chamados/models.py:1558`.

Produtores concretos:

- `processar_alertas_turno` recompõe quem tem batida nas últimas 36 horas; lê `TurnoMaterializado.aberto`, entrada, data e fim previsto; o juiz temporal devolve lembretes, e somente `pos_20` chama `_marco_critico`. `app/ponto/management/commands/processar_alertas_turno.py:51`, `app/ponto/management/commands/processar_alertas_turno.py:59`, `app/ponto/management/commands/processar_alertas_turno.py:67`, `app/ponto/management/commands/processar_alertas_turno.py:88`, `app/ponto/management/commands/processar_alertas_turno.py:100`, `app/ponto/management/commands/processar_alertas_turno.py:118`, `app/ponto/management/commands/processar_alertas_turno.py:140`.
- `_marco_critico` verifica `AlertaTurno/pos_20`, tenta abrir antes de notificar e criar justificativa; `_abrir_chamado_critico` usa `abrir`, grava módulo `turno_aberto`, subcategoria `saida_pendente`, urgência `emergencia`, `data_turno`, `entrada_timestamp`, posto e SLA. `app/ponto/management/commands/processar_alertas_turno.py:314`, `app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:319`, `app/ponto/management/commands/processar_alertas_turno.py:335`, `app/ponto/management/commands/processar_alertas_turno.py:345`, `app/ponto/management/commands/processar_alertas_turno.py:347`, `app/ponto/management/commands/processar_alertas_turno.py:348`.
- `processar_alertas_avancados` examina ontem, exige vínculo e dia de trabalho, recompõe e pega o primeiro materializado desse dia; pergunta idade/nível ao juiz. `app/ponto/management/commands/processar_alertas_avancados.py:22`, `app/ponto/management/commands/processar_alertas_avancados.py:37`, `app/ponto/management/commands/processar_alertas_avancados.py:49`, `app/ponto/management/commands/processar_alertas_avancados.py:57`, `app/ponto/management/commands/processar_alertas_avancados.py:58`, `app/ponto/management/commands/processar_alertas_avancados.py:63`, `app/ponto/management/commands/processar_alertas_avancados.py:72`.
- No N5, reaproveita `turno_aberto` vivo daquele dia; o fallback `_24h` filtra colaborador/módulo/`NAO_FECHADOS`, **sem filtro de dia**. Se não houver, `criar` grava `_24h` com dia e entrada; se houver, escala urgência e acrescenta ocorrência diária. `app/ponto/management/commands/processar_alertas_avancados.py:100`, `app/ponto/management/commands/processar_alertas_avancados.py:105`, `app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/ponto/management/commands/processar_alertas_avancados.py:126`, `app/ponto/management/commands/processar_alertas_avancados.py:173`, `app/ponto/management/commands/processar_alertas_avancados.py:177`, `app/ponto/management/commands/processar_alertas_avancados.py:185`.
- `_24h` também nasce por `abrir` no emissor retroativo, portanto o mesmo módulo atravessa duas portas com emissão de evento diferente. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`, `app/chamados/models.py:939`, `app/chamados/models.py:1048`.
- A API detecta anomalias antes da batida e agrega disputa depois; `orfao_14h` depende de bloqueio e ausência de regularização pendente. O predicado de bloqueio recente é `14 ≤ horas < 96`. `app/api/views_core.py:432`, `app/api/views_core.py:777`, `app/chamados/detector_anomalias.py:31`, `app/chamados/detector_anomalias.py:33`, `app/ponto/services/triagem_batida.py:17`, `app/ponto/services/triagem_batida.py:28`.
- A fábrica também chama `criar_ou_atualizar_disputa`; o nascimento do pai usa `abrir`, módulo `disputa_supervisao`, subcategoria recebida. `app/chamados/services/fabrica_da_celula.py:129`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:514`.
- Disputa existente recebe `motivos/contexto`, sem reclassificação do pai; disputa nova recebe o primeiro motivo ordenado como subcategoria. `app/chamados/services/disputa_emissao.py:471`, `app/chamados/services/disputa_emissao.py:477`, `app/chamados/services/disputa_emissao.py:498`, `app/chamados/services/disputa_emissao.py:595`, `app/chamados/services/disputa_emissao.py:601`.

Leitores externos relevantes:

- Reconciliador geral lê todos os chamados vivos do colaborador e julga a premissa antes de retratar. `app/chamados/reconciliador.py:1384`, `app/chamados/reconciliador.py:1389`, `app/chamados/reconciliador.py:1395`.
- Fechador específico lê apenas `turno_aberto/entrada_bloqueada_turno_aberto`, dia do contexto ou janela legada; verifica outros turnos abertos antes de retratar. `_24h` fica fora desse filtro. `app/ponto/management/commands/processar_alertas_turno.py:162`, `app/ponto/management/commands/processar_alertas_turno.py:178`, `app/ponto/management/commands/processar_alertas_turno.py:203`, `app/ponto/management/commands/processar_alertas_turno.py:207`.
- API de justificativa procura `turno_aberto/batida_ausente` vivos por dia ou data de criação legada; `_24h` não participa desse filtro. `app/api/views.py:1970`, `app/api/views.py:1974`.
- Derivação do destino da pergunta lê `entrada_timestamp`, turnos abertos e estágio; para entrada idêntica em `turno_aberto/_24h`, escolhe o estágio avançado. `app/chamados/services/disputa_emissao.py:116`, `app/chamados/services/disputa_emissao.py:128`, `app/chamados/services/disputa_emissao.py:140`.
- Regularização de órfão verifica fechamento cross-meia-noite antes de admitir a pergunta. `app/chamados/services/disputa_emissao.py:709`, `app/chamados/services/disputa_emissao.py:713`, `app/chamados/services/disputa_emissao.py:716`.
- Signal de disputa fechada lê estado do pai e chama `reconciliar`, cuja decisão consulta premissa e pergunta viva. `app/chamados/signals.py:28`, `app/chamados/signals.py:32`, `app/chamados/signals.py:42`, `app/chamados/models.py:648`, `app/chamados/models.py:651`.

Contraexemplos e irmãos:

- Reutilizar encerrado em `abrir` passa pelo renascimento e retorna antes do trecho emissor do evento; `reabrir` grava estado e ressuscita perguntas sem emissão explícita de `EventoCelula` nesse corpo. `app/chamados/models.py:882`, `app/chamados/models.py:885`, `app/chamados/models.py:935`, `app/chamados/models.py:1382`, `app/chamados/models.py:1391`.
- Nascer por `abrir` não implica evento: célula ausente faz o guard pular; resolução da célula retorna `None` sem dia forte, registro ou em exceção. `app/chamados/models.py:939`, `app/chamados/models.py:1184`, `app/chamados/models.py:1186`, `app/chamados/models.py:1190`, `app/chamados/models.py:1191`.
- `disputa_supervisao/orfao_14h` nasce por `abrir`, mas seu módulo não integra `MODULOS_DE_TURNO`, logo não ganha o evento desse ramo. `app/chamados/services/disputa_emissao.py:507`, `app/chamados/reconciliador.py:336`, `app/chamados/models.py:939`.
- `disputa_supervisao_manual/orfao_14h` é outra classe, nasce com IDs de órfãos no contexto e premissa `FATO_FILHOS_VIVOS`. `app/chamados/services/disputa_emissao.py:943`, `app/chamados/services/disputa_emissao.py:946`, `app/chamados/services/disputa_emissao.py:955`, `app/chamados/catalogo/premissa.py:50`.
- A fábrica distingue sem ata com janela de marcos, que espera, de sem ata/sem marcos, que pode recorrer a `classificar_falta`; ausência de ata não possui uma única consequência universal. `app/chamados/services/fabrica_da_celula.py:45`, `app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/fabrica_da_celula.py:52`, `app/chamados/services/fabrica_da_celula.py:55`.
- Outros pontos repetem `filter().first()` seguido de `criar` sem trava local: sem posto e férias; corrida produtiva não foi demonstrada. `app/ponto/utils.py:31`, `app/ponto/utils.py:40`, `app/ponto/utils.py:94`, `app/ponto/utils.py:103`; ocorrência [nao verificado].
- A abertura humana do DP oferece contraste existente: `atomic`, trava da pessoa, dedupe e só então `criar`; a abertura local genérica chama `criar` diretamente. `app/chamados/services/abertura.py:157`, `app/chamados/services/abertura.py:164`, `app/chamados/services/abertura.py:165`, `app/chamados/services/abertura.py:172`, `app/chamados/services/abertura.py:47`.

## 05/11 16:26 · sessao f99505840c46 · - · - · origem=Ronald (tmux)

Autoridades e limites confirmados:

- A regra já escrita determina evento na transação, consumidor idempotente fora da pilha e vigia sem julgamento; o contrato do O-OUTBOX explicita consumidor a cada minuto e `FOR UPDATE SKIP LOCKED`. `CLAUDE.md:167`; `CLAUDE.md:169`; `app/docs/CORTES.md:288`; `app/docs/CORTES.md:351`.
- O rebaixamento depende de existir evento produtor da classe; mudar o rótulo não satisfaz o contrato. A célula estrutural de chamado continua condicionada a O168, O169 e LASTRO. `app/docs/CORTES.md:358`; `app/core/contratos_estruturais.py:261`; `app/core/contratos_estruturais.py:269`.
- Há dois respondedores concretos: o registro C3 aponta `premissa_morta`, alimentada pelo turno materializado específico; o corte nomeia a célula, por `_absolvido_pela_celula`, para a morte da premissa do chamado de turno. Devem aparecer ambos no censo. `app/core/juizes.py:263`; `app/chamados/reconciliador.py:264`; `app/chamados/reconciliador.py:286`; `app/docs/CORTES.md:357`; `app/chamados/reconciliador.py:986`.
- `_absolvido_pela_celula` exige dia identificável, célula existente, veredito preenchido que não acusa e ausência de marcos pendentes. Não exige explicitamente ata preenchida nesse corpo. `app/chamados/reconciliador.py:991`; `app/chamados/reconciliador.py:995`; `app/chamados/reconciliador.py:997`.
- `turno_aberto_ha` mede idade do materializado; `turno_aberto_de` responde existência de aberta viva agora e expira após fim previsto mais margem. A escada 8/24/72 governa nível e urgência, sem extinção por idade nesse juiz. `app/core/juizes.py:229`; `app/ponto/turnos.py:3110`; `app/ponto/turnos.py:3123`; `app/ponto/turnos.py:3133`; `app/ponto/turnos.py:3143`.
- `lembrete_fim_turno` aceita tolerância inclusiva de 240 segundos e pode retornar dois lembretes nas janelas sobrepostas. `app/ponto/turnos.py:3153`; `app/ponto/turnos.py:3158`; `app/ponto/turnos.py:3164`.
- A regra temporal exige instante ou fato encerrado; `dia_encerrado` conjuga turno vivo e fim previsto, evitando turno partido encerrado pelo primeiro bloco e `all([])` verdadeiro no dia sem batida. Sem previsto, não afirma encerramento. `CLAUDE.md:185`; `app/ponto/turnos.py:2043`; `app/ponto/turnos.py:2050`; `app/ponto/turnos.py:2064`.
- Vínculo do dia é a geradora da célula dentro da vigência, inclusive inativa hoje; sem geradora, usa candidatura por cobertura da data. Geradora ausente da alimentação ou fora da vigência devolve `None`, sem escolher outra silenciosamente. `app/escala/alimentacao.py:228`; `app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:279`; `app/escala/alimentacao.py:285`.
- `motivo_do_veto` veta ausência de vínculo/template; o corte exclui nascimento/reabertura automáticos e mantém passagem do ato humano com informação e trilha. `app/ponto/services/furos_vetados.py:27`; `app/ponto/services/furos_vetados.py:40`; `app/docs/CORTES.md:403`.
- `trabalha=None` com ata vazia não é folga: o limbo cobra; ausência parcial não cala o restante. `app/docs/CORTES.md:416`; `app/chamados/juizes.py:411`; `app/chamados/juizes.py:424`.
- Competência segue o dia do fato e corte da empresa, padrão 21; no chamado, o fato mais recente inclui perguntas vivas do container. Tranca empresarial e FechamentoMensal individual são fontes diferentes. `app/docs/CORTES.md:216`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:36`; `app/chamados/juizes.py:1978`; `app/chamados/juizes.py:1996`.

Casos RED propostos pela regra, sem alegação de ocorrência produtiva:

| Cadastro + estado + ato | Resultado exigido pela regra |
|---|---|
| Empresa corte 21; fatos em 20/10 e 21/10 com trancas distintas; consumo do evento | Julgar cada fato na respectiva competência, sem usar mês civil ou competência de hoje. `app/docs/CORTES.md:216`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:60`. |
| Escala 19–07; entrada ontem sem saída; evento às 00:01 | A virada não demonstra encerramento; observar instante e turno vivo. `CLAUDE.md:185`; `app/ponto/turnos.py:2043`; `app/ponto/turnos.py:2050`. |
| Célula nomeia vínculo antigo ainda vigente no dia; troca de vínculo hoje; recomposição antiga | O dono do dia permanece a geradora válida naquele dia. `app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:282`. |
| `trabalha=None`, ata vazia; fabricação/consumo | Não absolver como folga somente por indeterminação. `app/docs/CORTES.md:416`; `app/chamados/juizes.py:426`. |
| Materializado aberto há 72 horas; consumo | Idade sustenta emergência, sem demonstrar morte da premissa. `app/ponto/turnos.py:3141`; `app/chamados/reconciliador.py:286`. |
| Mesmo fato entregue duas vezes ou dois consumidores concorrentes | Mesmo estado, uma trilha e nenhuma duplicata; consumidor respeita exclusão das linhas travadas. `CLAUDE.md:175`; `app/core/contratos_estruturais.py:33`; `app/docs/CORTES.md:351`. |
| Chamado com entrada declarada já fechada e outro turno aberto na janela | O outro turno não sustenta o fato denunciado por aquele chamado. `app/chamados/reconciliador.py:130`; `app/chamados/reconciliador.py:145`; `app/chamados/reconciliador.py:152`. |
| Ausência de vínculo; tentativa automática e depois ato humano | Automático vetado; humano segue a exceção do corte, com aviso e trilha. `app/docs/CORTES.md:403`. |

Concorrência já observável na implementação: o fechamento recarrega o estado e trava a linha dentro do `atomic`, evitando decisão apenas pela instância velha; ainda aplica gates de premissa e pergunta viva. Isso demonstra o mecanismo presente, sem provar resultado produtivo. `app/chamados/models.py:616`; `app/chamados/models.py:638`; `app/chamados/models.py:648`; `app/chamados/models.py:651`; `app/chamados/models.py:668`.

_1850 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
