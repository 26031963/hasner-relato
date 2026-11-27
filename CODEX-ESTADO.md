# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 10:03** · publicado em **27/11/2026 10:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 10:03 · sessao 123515df12cb · CASO-LITERAL-14 · leitura · origem=Code

```python
SOBRAS = [
    # Preencha com (empresa, colab_id, "AAAA-MM-DD", batida_id).
    # Exemplos da amostra_sobra.txt:1-2
    # (2, 43, "2026-11-02", 143768),
    # (2, 43, "2026-11-02", 143890),
]

# O envelope recebe um .py do chamador e o executa com
# tenant_command shell --schema=juliani e exec(open(...).read()).
# O código e a sonda são montados em leitura e o ORM usa a role leitor.
# bin/sonda_leitura.sh:63-91,153-181,235-243
#
# PORTA DA FASE 2
# O botão "Não foi trabalho (retratar)" faz POST com motivo_id e observacao.
# app/templates/colaboradores/partials/_calendario_grade.html:127
# ponto:retratar_batida -> retratar_batida_view -> retratar_humano.retratar
# -> registro_batida.retratar_batida -> registro_batida.anotar.
# O serviço humano exige a permissão regularizar_batida e motivo cadastrado
# ativo. O escritor preserva a linha, grava aprovada=False e retratada_em,
# registra o motivo na auditoria e dispara o evento de fechamento.
# app/ponto/urls.py:62; app/ponto/views.py:3453-3462
# app/ponto/services/retratar_humano.py:118-183
# app/ponto/registro_batida.py:255-302
#
# O botão "desfazer" usa desfazer_retratacao_view -> retratar_humano.desfazer
# -> anotar. A mesma permissão é exigida. A última linha de auditoria da
# batida deve ser retratar_batida_admin e o prazo é de sete dias a partir
# de retratada_em. O ato grava retratada_em=None e aprovada=True.
# app/templates/colaboradores/partials/_calendario_grade.html:127
# app/ponto/urls.py:64; app/ponto/views.py:3465-3473
# app/ponto/services/retratar_humano.py:26-49,186-206
#
# Campos de aprovação/admin em Batida: aprovada, aprovada_por (lido via
# aprovada_por_id), aprovada_em e retratada_em. O sinal de aprovação
# atribuída a um usuário é aprovada_por_id preenchido. aprovada tem
# default True. Edições de tipo e de horário têm autoria e data na trilha.
# app/ponto/models.py:22-95; app/ponto/views.py:490-494
# app/ponto/services/flip_auto.py:285-298; app/ponto/services/flip_batida.py:41-49
# app/chamados/services/regularizacao_ext.py:150-162; app/core/models.py:285-307
#
# Lista completa dos 14, dados de produção e execução com o ORM no
# envelope [nao verificado]. Sintaxe e funções puras conferidas em memória.

import json
from collections import defaultdict
from datetime import date, datetime, time, timedelta
from functools import lru_cache
from zoneinfo import ZoneInfo

from django.db import connection
from django.db.models import Q
from django.utils import timezone

from chamados.catalogo.modulos import data_do_chamado
from chamados.models import (
    ChamadoColaborador,
    DecisaoPergunta,
    FollowupChamado,
    PerguntaDisputa,
    RegularizacaoItemBatida,
)
from colaboradores.models import Colaborador
from core.models import LogAuditoria
from escala.models import CelulaDia
from escala.servico_jornada import escala_vigente
from ferias.models import AgendamentoFerias
from ponto.catalogo.ausencias import CATALOGO, INDETERMINADO
from ponto.models import Ausencia, Batida, Justificativa, TipoAusencia

TZ = ZoneInfo("America/Sao_Paulo")  # app/config/settings/base.py:93-95
UM_DIA = timedelta(days=1)
NV = "[nao verificado]"


def formatar(valor):
    if isinstance(valor, datetime):
        return timezone.localtime(valor, TZ).isoformat()
    if isinstance(valor, (date, time)):
        return valor.isoformat()
    if isinstance(valor, str) and valor and "W004" not in valor and all(
        c.isalnum() or c in "_-+.:/" for c in valor
    ):
        return valor
    # Escapes JSON preservam o valor ao passar pelo filtro do envelope.
    # bin/sonda_leitura.sh:239-243
    return json.dumps(valor, ensure_ascii=False).replace(
        "W004", r"\u0057004"
    ).replace("objects imported", r"objects\u0020imported")


def linha(rotulo, *, marca="", **campos):
    valores = " ".join(f"{k}={formatar(v)}" for k, v in campos.items())
    print(f"{rotulo} {valores}" + (f" {marca}" if marca else ""))


def meia_noite(dia):
    return timezone.make_aware(datetime.combine(dia, time.min), TZ)


def agrupar():
    dias = {}
    todas = set()
    for empresa, cid, data_iso, bid in SOBRAS:
        if any(type(pk) is not int or pk <= 0 for pk in (empresa, cid, bid)):
            raise ValueError("SOBRAS exige IDs inteiros positivos")
        dia = date.fromisoformat(data_iso)
        if dia.isoformat() != data_iso or not date(2026, 10, 21) <= dia <= date(2026, 11, 20):
            raise ValueError(f"Data fora do formato ou da comp 11 de 2026 (colab_id={cid})")
        empresa_dia, ids = dias.setdefault((cid, dia), (empresa, set()))
        if empresa != empresa_dia:
            raise ValueError(f"Empresas divergentes no mesmo dia (colab_id={cid} data={dia})")
        ids.add(bid)
        todas.add(bid)
    if not dias:
        raise ValueError("Preencha SOBRAS antes da execucao")
    return dias, todas


def batidas_do_recorte(cid, dia, sobras_dia):
    # Janela semiaberta entre as meias-noites locais de D-1 e D+2.
    # Leitura da linha bruta, incluindo retratadas e pendentes.
    # app/ponto/models.py:22-29,40-73; app/ponto/registro_batida.py:99-109
    batidas = list(Batida.objects.filter(
        colaborador_id=cid, timestamp__gte=meia_noite(dia - UM_DIA),
        timestamp__lt=meia_noite(dia + 2 * UM_DIA),
    ).order_by("timestamp", "pk").values(
        "id", "timestamp", "tipo", "origem", "canal", "tipo_registro",
        "is_intervalo", "escala_id", "pergunta_origem_id",
        "aprovada", "aprovada_por_id", "aprovada_em", "retratada_em",
    ))
    faltam = sobras_dia - {b["id"] for b in batidas}
    if faltam:
        raise ValueError(
            f"SOBRA fora da janela ou do colaborador "
            f"(colab_id={cid} data={dia} ids={sorted(faltam)})"
        )
    return batidas


def trilhas_de(batida_ids):
    # A ação csrf_batida usa o ID do colaborador no lugar do ID da batida.
    # app/core/views.py:1224-1242; app/core/models.py:285-307
    saida = defaultdict(list)
    for ato in (LogAuditoria.objects.filter(modelo="Batida", objeto_id__in=batida_ids)
                .exclude(acao="csrf_batida").order_by("criado_em", "pk")
                .values("id", "objeto_id", "acao", "usuario_id", "criado_em", "valor_depois")):
        saida[ato["objeto_id"]].append(ato)
    return saida


def retratacao_vigente(instante, atos):
    # Casa o carimbo vigente com a trilha. Motivo humano sai como ID.
    # Quando a justificativa só existe em texto livre, a trilha identifica
    # o ato e motivo_retratacao sai como [nao verificado].
    # app/ponto/registro_batida.py:231-239,270-296
    # app/ponto/services/retratar_humano.py:172-181
    # app/chamados/services/acoes_chamado.py:261-279
    # app/ponto/management/commands/passivo_disputa_retro.py:164-171
    if instante is None:
        return None, None
    for ato in reversed(atos):
        depois = ato["valor_depois"]
        if isinstance(depois, dict) and depois.get("retratada_em"):
            if datetime.fromisoformat(depois["retratada_em"]) == instante:
                motivo = depois.get("motivo_retratacao") or {}
                motivo_id = motivo.get("id")
                if type(motivo_id) is int:
                    return ato, motivo_id
                if depois.get("motivo_classe") == "passivo_s84_folga":
                    return ato, "passivo_s84_folga"
                return ato, NV
    return None, NV


def imprimir_batida(batida, atos, todas_as_sobras):
    bid = batida["id"]
    local = timezone.localtime(batida["timestamp"], TZ)
    retratacao, motivo = retratacao_vigente(batida["retratada_em"], atos)
    linha(
        "BAT", id=bid, data=local.date(), hora=local.strftime("%H:%M:%S"),
        marca="SOBRA" if bid in todas_as_sobras else "",
        **{k: v for k, v in batida.items() if k not in ("id", "timestamp")},
        motivo_retratacao=motivo,
        retratacao_trilha_id=retratacao["id"] if retratacao else None,
        retratada_por_id=retratacao["usuario_id"] if retratacao else None,
        retratacao_acao=retratacao["acao"] if retratacao else None,
    )
    for ato in atos:
        linha("BAT_AUD", batida_id=bid, trilha_id=ato["id"], acao=ato["acao"],
              usuario_id=ato["usuario_id"], quando=ato["criado_em"])


def imprimir_escala(cid, dia):
    # hi/hf são entrada/saída. hii/hfi e hii2/hfi2 são saída/volta das pausas.
    # Os instantes previstos já gravados permitem ler a virada de meia-noite.
    # app/escala/models.py:1656-1747,1843-1847
    # app/ponto/management/commands/gerar_celulas.py:36-70,88-113
    celula = (CelulaDia.objects.filter(colaborador_id=cid, data=dia)
              .select_related("escala_geradora__tipo_escala").first())
    celulas = {(cid, dia): celula} if celula is not None else {}
    if celula is None:
        linha("CEL", id=None, estado=NV)
    else:
        dna = celula.dna or {}
        linha(
            "CEL", id=celula.pk, origem=celula.origem, trabalha=celula.trabalha,
            estado={True: "trabalho", False: "folga", None: "indeterminado"}[celula.trabalha],
            marcos=dna.get("marcos"), instantes_previstos=dna.get("lampadas"),
            escala_geradora_id=celula.escala_geradora_id, tipo_escala_dna_id=dna.get("tipo_escala_id"),
            regime_dna=dna.get("tipo_ciclo"), gerada_em=celula.gerada_em,
            regenerada_em=celula.regenerada_em, regeneracoes=celula.regeneracoes,
            editada_em=celula.editada_em, editada_por_id=celula.editada_por_id,
        )
    # Leitor do vínculo do dia, com a precedência e a vigência da geradora.
    # app/escala/servico_jornada.py:15-66; app/escala/alimentacao.py:269-286
    # app/escala/models.py:117-121,801-804,871-877
    escala = escala_vigente(cid, dia, celulas=celulas)
    if escala is None:
        linha("VINCULO", id=None)
    else:
        template = escala.tipo_escala
        linha("VINCULO", id=escala.pk, posto_id=escala.posto_id, template_id=template.pk,
              template=str(template), regime=template.tipo_ciclo, inicio=escala.data_inicio,
              fim=escala.data_fim, ativa=escala.ativa)


def imprimir_ausencias(cid, dia, tipos):
    # Mostra cada lançamento e seu estado, inclusive rejeitados e cancelados.
    # efeito_tipo=abona é a regra do tipo. O estado do lançamento sai em status.
    # app/ponto/models.py:571-590,660-705,743-820
    # app/ponto/turnos.py:2548-2579; app/ferias/models.py:119-121,291-319
    em_aberto = [codigo for codigo, dados in tipos.items() if dados[1] == INDETERMINADO]
    ausencias = list(Ausencia.objects.filter(colaborador_id=cid, data__lte=dia).filter(
        Q(data=dia) | Q(data_fim__gte=dia) | Q(data_fim__isnull=True, tipo__in=em_aberto)
    ).order_by("data", "pk").values(
        "id", "tipo", "status", "data", "data_fim", "minutos", "hora_ini", "hora_fim",
        "aprovado_por_id", "aprovada_em", "agendamento_ferias_id", "chamado_origem_id",
    ))
    ferias = list(AgendamentoFerias.objects.filter(
        periodo_aquisitivo__colaborador_id=cid, data_inicio__lte=dia, data_fim__gte=dia,
    ).order_by("data_inicio", "pk").values("id", "status", "data_inicio", "data_fim"))
    linha("AUSENCIAS", ids=[a["id"] for a in ausencias], ferias_ids=[f["id"] for f in ferias])
    for ausencia in ausencias:
        efeito, _, medico = tipos.get(ausencia["tipo"], (NV, None, NV))
        linha("AUS", **ausencia, efeito_tipo=efeito, tipo_medico=medico)
    for agendamento in ferias:
        linha("FERIAS", **agendamento)
    return {a["chamado_origem_id"] for a in ausencias if a["chamado_origem_id"] is not None}


@lru_cache(maxsize=None)
def chamados_de(cid):
    return list(ChamadoColaborador.objects.filter(colaborador_id=cid).order_by("pk").values(
        "id", "status_local", "modulo_origem", "contexto_json", "celula_id", "celula__data",
        "justificativa_relacionada_id", "via_encerramento", "respondido_por_id", "resolvido_em",
    ))


def imprimir_chamados(cid, dia, batidas, chamados_ids):
    # Vínculos explícitos da pergunta: dia/célula, batida de origem e batida
    # plantada. Estado resume os atos lidos e os carimbos armazenados.
    # Visibilidade atual na fila [nao verificado].
    # app/chamados/models.py:2724-2760,3351-3370,3811-3838,4472-4515
    # app/chamados/juizes.py:1626-1628,1644-1653; app/ponto/models.py:40-44
    ids = {b["id"] for b in batidas}
    ids_texto = {str(pk) for pk in ids}
    origens = {b["pergunta_origem_id"] for b in batidas if b["pergunta_origem_id"] is not None}
    perguntas = (PerguntaDisputa.objects.filter(colaborador_id=cid).filter(
        Q(celula__data=dia) | Q(data_evento=dia, conversa=False)
        | Q(batida_origem_id__in=ids) | Q(pk__in=origens)
    ).select_related("disputa").prefetch_related("decisoes").order_by("pk"))
    for p in perguntas:
        lido = DecisaoPergunta.composta(p)
        respondida = lido.respondida_em is not None and bool((lido.resposta_colab or "").strip())
        estado = "respondida" if respondida else "pendente"
        if lido.materializada_em is not None:
            estado = "materializada"
        elif lido.validada_em is not None:
            estado = "validada"
        linha(
            "PERGUNTA", id=p.pk, estado=estado, motivo=p.motivo, data_evento=p.data_evento,
            celula_id=p.celula_id, batida_origem_id=p.batida_origem_id,
            batidas_plantadas_ids=[b["id"] for b in batidas if b["pergunta_origem_id"] == p.pk],
            disputa_id=p.disputa_id, disputa_fechada_em=p.disputa.fechada_em,
            chamado_pai_id=p.disputa.chamado_id, chamado_destino_id=p.chamado_destino_id,
            respondida=respondida, respondida_em=lido.respondida_em, validada_em=lido.validada_em,
            materializada_em=lido.materializada_em, via=lido.via_resolucao,
        )
        chamados_ids |= set(map(int, p.chamados_relacionados_ids or ()))
        chamados_ids |= {p.disputa.chamado_id, p.chamado_destino_id}

    # Ligações formais por justificativa, regularização ou ausência.
    # app/ponto/models.py:139-144,167-177,786-790
    # app/chamados/models.py:81-85,1923-1931,4658-4662,4687-4715
    justificativas = Justificativa.objects.filter(colaborador_id=cid).filter(
        Q(data_turno=dia) | Q(batida_entrada_id__in=ids) | Q(batida_gerada_id__in=ids)
    ).order_by("pk").values_list("pk", "chamado_origem_id")
    justificativas_ids = set()
    for jid, chamado_id in justificativas:
        justificativas_ids.add(jid)
        chamados_ids.add(chamado_id)
    itens = RegularizacaoItemBatida.objects.filter(regularizacao__colaborador_id=cid).filter(
        Q(data=dia) | Q(batida_entrada_id__in=ids)
        | Q(batida_saida_id__in=ids) | Q(batida_alvo_id__in=ids)
    )
    chamados_ids |= set(itens.order_by().values_list("regularizacao__chamado_id", flat=True))

    # Chamado por célula, data declarada ou batida no contexto. Seguimentos
    # mostram autoria e instante sem expor o conteúdo da conversa.
    # app/chamados/models.py:39-72,1835-1848
    # app/chamados/juizes.py:89-129; app/chamados/catalogo/modulos.py:219-225
    # app/chamados/services/acoes_chamado.py:244-248
    relacionados = []
    for ch in chamados_de(cid):
        ctx = ch["contexto_json"] or {}
        data_ctx = data_do_chamado(ch["modulo_origem"], ctx) or ctx.get("data_turno")
        batidas_ctx = {str(ctx.get("batida_id"))}
        batidas_ctx |= {str((o or {}).get("batida_id")) for o in ctx.get("ocorrencias") or []}
        if not (
            ch["id"] in chamados_ids or ch["justificativa_relacionada_id"] in justificativas_ids
            or ch["celula__data"] == dia or batidas_ctx & ids_texto
            or (ch["celula_id"] is None and str(data_ctx)[:10] == dia.isoformat())
        ):
            continue
        relacionados.append(ch["id"])
        linha("CHAMADO", id=ch["id"], estado=ch["status_local"], celula_id=ch["celula_id"],
              batidas_contexto_ids=sorted(map(int, batidas_ctx & ids_texto)),
              via_encerramento=ch["via_encerramento"], respondido_por_id=ch["respondido_por_id"],
              resolvido_em=ch["resolvido_em"])
    for resposta in (FollowupChamado.objects.filter(chamado_id__in=relacionados)
                     .order_by("criado_em", "pk")
                     .values("id", "chamado_id", "autor_tipo", "autor_user_id", "criado_em")):
        linha("CHAMADO_SEGUIMENTO", **resposta)


def main():
    if connection.schema_name != "juliani":
        raise RuntimeError("A sonda exige o schema juliani")
    dias, sobras = agrupar()

    # Leitura direta do catálogo para propagar falhas de consulta. O catálogo
    # da cópia é usado quando a consulta retorna zero tipos ativos.
    # app/ponto/catalogo/ausencias.py:86-105,240-281,430-435
    tipos = {codigo: (efeito, fim, medico) for codigo, efeito, fim, medico in
             TipoAusencia.objects.filter(ativo=True).order_by().values_list("codigo", "efeito", "fim", "medico")}
    if not tipos:
        tipos = {codigo: (dados[0], dados[5], dados[6]) for codigo, dados in CATALOGO.items()}
    empresas_cadastradas = dict(Colaborador.objects.filter(
        pk__in={cid for cid, _ in dias},
    ).order_by().values_list("pk", "empresa_id"))  # app/colaboradores/models.py:319-322

    total_batidas = 0
    for (cid, dia), (empresa, sobras_dia) in sorted(dias.items()):
        batidas = batidas_do_recorte(cid, dia, sobras_dia)
        trilha = trilhas_de([b["id"] for b in batidas])
        linha("DIA", colab_id=cid, data=dia, empresa=empresa,
              empresa_cadastro_id=empresas_cadastradas[cid], sobras_ids=sorted(sobras_dia))
        for batida in batidas:
            imprimir_batida(batida, trilha[batida["id"]], sobras)
        imprimir_escala(cid, dia)
        chamados_ids = imprimir_ausencias(cid, dia, tipos)
        imprimir_chamados(cid, dia, batidas, chamados_ids)
        total_batidas += len(batidas)

    # batidas conta linhas BAT emitidas, inclusive em janelas sobrepostas.
    print(f"FIM dias={len(dias)} batidas={total_batidas}")


main()
```

## 27/11 10:02 · sessao b89a8e9ccbee · PAR-PELO-TURNO · verificador · origem=Code

## Procedentes

1. **O mesmo PK ainda muda de dia.** Com DNA `21:30–05:42`, `E 23/10 01:30` e `S 24/10 00:00`, a casca e o lote publicam a entrada em **22/10** na consulta de 21/10 e em **23/10** na consulta de 22/10. A segunda consulta alcança a saída e reclassifica a entrada. Há divergência mesmo com todas as marcas carregadas. Com A noturno até 21/10, B diurno a partir de 22/10 e `E 21/10 21:30`, `S 22/10 08:00`, `S 22/10 20:00`, a última saída recebe **22/10** na casca e na consulta individual, mas **21/10** na mesma janela com `turnos=`. Dois segmentos reclamam o PK e a última atribuição vence. O RED de `r.txt` passou na geometria original de um vínculo, assim como o controle sintético de col297. A invariância exigida pela L-085 segue aberta. `app/docs/LEIS.md:117` · `app/ponto/turnos.py:287` · `app/ponto/turnos.py:1998` · `app/ponto/turnos.py:3037` · `app/ponto/turnos.py:3060` · `app/ponto/turnos.py:3082` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:57` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:73`

2. **A pré-passagem da pausa ainda perde o relógio da entrada.** O caso anterior da `E` interna passou com flag gravada e com `Xi`. O novo contraexemplo usa marcos de borda `19:00–07:00`, `E 20/10 19:00`, `S 19:00:30`, `S 21/10 01:00`, `E 02:00`, `S 07:00`. Sem marcos de borda, a sonda fecha `19:00→07:00` com 60 min de pausa. Com os marcos, ela produz `19:00→01:00` e `02:00→07:00`, sem pausa. A pré-passagem com bordas zera `limite` pela saída de 30 s, enquanto o laço principal conserva a entrada. O RED da saída degenerada só chama o ramo sem bordas. `app/ponto/turnos.py:435` · `app/ponto/turnos.py:447` · `app/ponto/turnos.py:508` · `app/ponto/turnos.py:1501` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:82` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:116`

3. **O item 2 do aval ainda falha por regra.** A sonda original com só `E 22/10 12:00` deu **0,00 h** nos dois braços. Em `21/09–20/10`, com previsão comercial e sem ausência, usei `E 21/10 01:30`, `S 22/10 00:00` com papel `Xi` e uma batida posterior em `23/10 08:00`. A casca dispõe do fato posterior da O95, fecha o par e data ambas as marcas em **21/10**. A projeção nova da folha passa `ultima_batida=None`, mantém o turno aberto e atribui ambas a **20/10**. Com zero períodos no motor, os trechos reais do fechamento deram **0,00 h** sem as marcas e **−193,60 h** com elas. Esse saldo alimenta o `FechamentoMensal` e o banco do `DiaPago`. `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:204` · `app/ponto/turnos.py:1172` · `app/ponto/turnos.py:1186` · `app/ponto/turnos.py:1931` · `app/ponto/services/fechamento.py:165` · `app/ponto/services/fechamento.py:679` · `app/ponto/services/fechamento.py:1123` · `app/ponto/services/fechamento.py:1199` · `app/ponto/services/fechamento.py:1227`

4. **A segunda virada continua dependente do cadastro do dia da saída.** Para `E 20/10 23:30 → S 22/10 01:30`, o juiz devolve um par fechado de 26 h. A sonda de `_turno_noturno_completo` devolveu `True` com o vínculo noturno e `False` com vínculo diurno ou sem vínculo em 22/10. A guarda rejeita o caso antes de consultar o par. O teste usa o mesmo vínculo nas duas datas e simula o lote com a própria casca. A transferência das duas pontas para a fatia de A passou no caso anterior de 26 h, mas o teste usa dois vínculos sem `tipo_escala`, o mesmo DNA e ata vazia. A fatia continua consumindo o mapa sujeito à dupla atribuição do item 1. `app/escala/servico_jornada.py:230` · `app/escala/servico_jornada.py:245` · `app/escala/servico_jornada.py:354` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:125` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:149` · `app/ponto/services/fechamento.py:284`

5. **O item 1 do aval, 4b, continua reservado e produz diferença de dinheiro.** Com `E 19/10 22:00 → S 20/10 22:00` e `E 21/10 03:00 → S 21/10 11:00`, o juiz data o segundo par em **21/10**. No mesmo `Motor12x36`, variar só a carga da antiga borda do PDF para `ini−4` fez **8,00 h virarem 0,00 h** na janela iniciada em 21/10. A primeira jornada inteira passa a chegar ao motor, que aplica a herança pelo vão de 5 h e exclui o segundo par como dia 20. A folha já recebe a carga ampliada. O PDF livre passou a recebê-la nesta volta e soma a saída desse motor. A outra pendência reservada, 3c, também permanece. Para fim em 20/10, a testemunha da folha pede julgamento até 21/10, carrega até 23/10 inclusive e só recorta a comparação depois. Uma marca em `23/10 00:00` entra nessa apuração e fica fora da carga da competência. `app/docs/par_pelo_turno_c1v6_evidencia.txt:137` · `app/core/juizes.py:216` · `app/ponto/motor_calculo_v2.py:2516` · `app/ponto/motor_calculo_v2.py:2542` · `app/ponto/services/fechamento.py:655` · `app/relatorios/pdf_espelho.py:281` · `app/relatorios/pdf_espelho.py:495` · `app/folha/porta_export.py:77` · `app/folha/porta_export.py:106` · `app/folha/porta_export.py:114`

## Escritores e leitores

| Valor | Onde nasce e onde o contrato diverge |
|---|---|
| `janela_com_pad` e `batidas_apuraveis_na_janela` | `turnos.py` escreve `ini−4..fim+2`, por dias locais completos, do primeiro ao último microssegundo inclusive. Casca e lote usam filtros de data local. Grade, carga de perguntas, fechamento, autoridade do espelho e PDF leem a janela. O fechamento usa o helper também na passada de órfãos. A carga de perguntas escreve a borda equivalente como `< 00:00` do dia seguinte, mas recorta de um lote já carregado. A divergência demonstrada está no período pedido pela testemunha da folha e no provedor da O95. `app/ponto/turnos.py:1869` · `app/ponto/turnos.py:1895` · `app/ponto/turnos.py:2990` · `app/ponto/turnos.py:3001` · `app/ponto/turnos.py:3149` · `app/escala/utils.py:1352` · `app/chamados/services/carga_pergunta_viva.py:174` · `app/ponto/services/fechamento.py:831` · `app/ponto/services/espelho.py:1031` · `app/relatorios/pdf_espelho.py:281` |
| Dia por PK e dia do período | `_dia_das_batidas_puro` escreve o dia do turno reclamante e completa as demais marcas pelo dia civil. Espelho e app filtram por PK, painel, férias e vigia consultam PKs específicos, e disputa e fio chegam pela consulta individual. Na tela, a soma de horas passou a ler o dia publicado pelo motor, mas `turnos` e `turnos_abertos` ainda filtram pela entrada civil. No próprio caso 3d, `E 21/09 00:00 → S 07:50` atribuído a 20/09 soma **7 h 50 min** no recorte até dia 20 e acrescenta **zero** a `turnos`. `app/ponto/turnos.py:3070` · `app/ponto/services/espelho.py:435` · `app/ponto/services/espelho.py:451` · `app/ponto/services/espelho.py:588` · `app/api/views.py:1444` · `app/api/views.py:1461` · `app/core/services/painel_op.py:330` · `app/ferias/management/commands/listar_ferias_com_batida.py:54` · `app/ponto/management/commands/vigia_plantio_overnight.py:87` · `app/chamados/services/disputa_emissao.py:283` · `app/chamados/services/fio.py:1827` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:103` |
| `dias_inconsistentes` | A tela incrementa uma vez por dia inconsistente. O PDF reconta as linhas. `folha_manda` pode sobrescrever ambos com `FechamentoMensal.inconsistencias`, que conta anomalias e acrescenta 1 por `sem_movimento`. Com zero anomalias e nenhuma linha inconsistente, o contador de dias vale **0** e o fechamento sem movimento publica **1**. O badge, o app e a comparação da porta leem a mesma chave com esses dois significados. `app/ponto/services/espelho.py:670` · `app/ponto/services/espelho.py:685` · `app/relatorios/pdf_espelho.py:604` · `app/ponto/services/fechamento.py:1119` · `app/relatorios/cartao_pela_celula.py:113` · `app/relatorios/cartao_pela_celula.py:346` · `app/templates/ponto/espelho.html:67` · `app/api/views.py:1434` · `app/folha/porta_export.py:502` |
| `pdf_espelho` | O coletor lê as linhas de `espelho_do_colab`, mas monta insumos e roda `apurar_por_fatia` ou `calcular_mes` em paralelo. Em período livre, publica o próprio saldo do motor. O mesmo coletor alimenta a geração do PDF, a testemunha de exportação e a pauta de dias em aberto. Com `agora=25/11` e pedido `26–27/11`, o `max` faz a apuração começar e terminar em **26/11**, enquanto a tela consultada para as linhas termina em **24/11**. É uma divergência de teto usando o mesmo `agora`. O delta fornecido tem **+0** cláusulas `except`. `app/relatorios/pdf_espelho.py:277` · `app/relatorios/pdf_espelho.py:410` · `app/relatorios/pdf_espelho.py:426` · `app/relatorios/pdf_espelho.py:524` · `app/relatorios/pdf_espelho.py:589` · `app/relatorios/pdf_espelho.py:647` · `app/relatorios/pdf_espelho.py:1041` · `app/folha/porta_export.py:502` · `app/ponto/services/dias_em_aberto.py:112` · `app/docs/par_pelo_turno_c1v6_evidencia.txt:149` |

## O que os testes deixam passar [nao verificado]

- **c1v6.** Ata vazia derrubou o teste novo, fechando aquele mutante de `r.txt`. Encurtar a carga do PDF, restaurar o recorte civil da soma e usar `not batidas` também derrubaram as respectivas sondas. Em contrapartida, mudei só `dia <= _data_fim` para `dia < _data_fim` no `sem_movimento`. Os **95 testes e sondas em memória** passaram. Uma única marca apurável atribuída ao dia 20, sem ausência, passaria a ser tratada como mês sem movimento. Os dois braços do 4c exercitam apenas o caso sem fato na competência. `app/ponto/tests/test_par_pelo_turno_c1v6.py:182` · `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:266` · `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:319` · `app/ponto/services/fechamento.py:679` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:120`
- **c1v7.** O AST distingue os dois nomes por igualdade e contou uma chamada do embrulho na linha 281. Os cinco testes estruturais passaram em memória. Uma segunda chamada via `carregar = batidas_apuraveis_na_janela; carregar(...)` deixou o censo idêntico. A chamada real a `apurar_por_fatia` também está fora de `AUTORIDADES`. O verde do selo permite a derivação paralela do coletor. `app/relatorios/tests/test_pdf_nao_calcula.py:34` · `app/relatorios/tests/test_pdf_nao_calcula.py:63` · `app/relatorios/tests/test_pdf_nao_calcula.py:96` · `app/relatorios/pdf_espelho.py:281` · `app/relatorios/pdf_espelho.py:412`
- **c1v8.** Comparei os literais via AST. A fixture copia as nove chaves e valores de `espelho.py`, incluindo `dias_inconsistentes`. Ela corrige o `KeyError` da montagem manual e preserva a asserção de horas. Como injeta o próprio resumo e chama `montar_dias`, o 3d deixa passar a remoção de `dias_inconsistentes` do inicializador de produção e a remoção do incremento. A comparação da fixture com o contrato atual é exata, mas o teste não protege esse contrato contra mudança futura. `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:100` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:113` · `app/ponto/services/espelho.py:685` · `app/ponto/services/espelho.py:1236`

A execução Django parou no import de `django.core`. O `Ran 356 OK`, a identidade Git de HEAD e das bases, o efeito no gravado e no TXT, e o laudo dos 29 casos por rubrica ficam **[nao verificado]**. Para col155/08-11, a cópia só oferece o ensaio sintético de **782 min**, teto de **732 min** e uma C9. `app/manage.py:11` · `app/docs/BACKLOG.md:2027` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:195`

**REPROVADO — PK muda de dia e de dono (`app/ponto/turnos.py:3085`), pausa perde a entrada (`app/ponto/turnos.py:508`), 4c perde a prova da O95 e muda o saldo (`app/ponto/services/fechamento.py:165`), guarda noturna rejeita par fechado (`app/escala/servico_jornada.py:230`), 3c amplia a janela (`app/folha/porta_export.py:77`), 4b perde 8 h (`app/ponto/motor_calculo_v2.py:2521`), PDF recalcula e usa outro recorte (`app/relatorios/pdf_espelho.py:412`), resumo mistura dias e ocorrências (`app/relatorios/cartao_pela_celula.py:113`) e os testes deixam passar borda exclusiva, chamada indireta e perda da chave de produção (`app/ponto/services/fechamento.py:679` · `app/relatorios/tests/test_pdf_nao_calcula.py:77` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:113`).**

## 27/11 09:52 · sessao ca7d3c808b96 · - · - · origem=Ronald (tmux)

- **R4 e R5.** O cenário de intervalo permite exercitar origem aberta e marco efetivo entregue na virada do corte. Na base, o repasse grava os IDs de ambos os itens no agregado antes de tentar as perguntas. O nascimento distingue alteração de inserção e só aplica a guarda B2 à alteração. O teste pode observar `_nascer`, o dia efetivo e a tentativa de `INSERT`, além da releitura pública de `cobrar_dia`. `chamados/tests/test_fio_intervalo.py:45`, `ponto/janelas.py:60`, `folha/tests/test_export_vigente.py:33`, `chamados/services/disputa_emissao.py:1117`, `chamados/models.py:3192`, `chamados/models.py:3333`, `chamados/models.py:3847`, `chamados/services/cobrar_dia.py:45`.
- **R3.** O callback autônomo de silêncio de isentos já tem fixture com callback real. Adicionar TXT vigente e cobrar o registro do silêncio detecta uma extensão indevida de `_K6`. O beco tem cerca B2 própria e é outro cenário. `chamados/signals.py:285`, `chamados/tests/test_varredura_isentos_evento.py:43`, `chamados/services/silencio_isentos.py:34`, `chamados/services/becos.py:63`.
- **Volta 3.** O template imprime só a hora, o PDF consulta HH:MM e o supra só imprime a lista de horas. Os pontos observáveis de transporte são a ata em curso no emissor, `grade_da_celula` durante `_marcos_faltantes` e `grade_do_fechamento` na folha. `templates/ponto/espelho.html:242`, `relatorios/pdf_espelho.py:472`, `relatorios/pdf_espelho.py:607`, `ponto/supra_juiz.py:527`, `ponto/services/cartorio.py:1505`, `chamados/services/declaracao_texto.py:101`, `folha/export.py:329`.

Execução dos cenários [nao verificado].

## 27/11 09:51 · sessao 74ba2bb386d4 · - · - · origem=Ronald (tmux)

- R3 tem um caminho autônomo observável. O `on_commit` de `retoma_regularizacoes_ao_fechar_disputa` chama a retomada fora do cartório. Um mutante que condicione essa chamada à ausência de TXT vigente deve fazer falhar o teste que observa a retomada com TXT. O caminho de beco já tem cerca própria. `chamados/signals.py:122–146`, `chamados/services/becos.py:63–71`
- R4 tem um ponto de asserção concreto. O repasse grava os IDs de todos os candidatos no contexto da disputa e do pai antes de processar as perguntas. O cenário misto deve exigir `[admitido.pk]` nos dois contextos e a trilha individual da vetada. `chamados/services/disputa_emissao.py:1100–1142`, `chamados/tests/test_e4a2_nunca_bateu.py:81–114`
- R6 pode interpor um residual depois do primeiro `carimbar` real. `vigiar_escrita_de_perguntas` é inerte fora da cerca e sinaliza o veto dentro dela. `executar_sob_cerca` desfaz o savepoint mesmo quando a exceção interna foi capturada. O cartório atual chama `julgar_colab` e aplica a precedência sem usar essa cerca. `ponto/services/cartorio.py:1905–1932`, `ponto/services/cartorio.py:2078–2134`, `chamados/services/fio.py:1536–1592`
- Desenho A. `julgar_celula` usa a data e a hora ambientes na margem do turno. Gate e tripwire convergem em `_vivas_entre`, que chama o C6 sem o instante explícito já aceito pelo juiz. O RELATO descreve o cenário C6 `0→1` após a primeira lavra. O percurso Django integral desse cenário segue [nao verificado]. `ponto/services/cartorio.py:2083–2115`, `chamados/models.py:236`, `chamados/models.py:368–402`, `chamados/juizes.py:911–943`, `docs/RELATO.md:1123`
- E2 e fábrica. Há uma fixture de pergunta absolvida e uma de carimbo sem lastro para montar dois dias do mesmo colaborador com TXT vigente em apenas um. A asserção deve observar tentativas de escrita, além da foto final, para distinguir o mutante que guarda depois do carimbo. `chamados/tests/test_e4c_f5_guarda_reconciliador.py:141–174`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:49–104`, `folha/tests/test_k6_fechada_para_celula.py:32–44`, `chamados/reconciliador.py:695–707`, `chamados/services/fabrica_da_celula.py:206–219`

Resultados de execução [nao verificado].

## 27/11 09:50 · sessao e5a6d7e4fdc0 · - · - · origem=Ronald (tmux)

- R1 pode pedir a autoridade `escala.orfa_classe.razao_da_sobra`. A base tem `classificar_orfas`, que opera por minuto. O sítio futuro consta em `docs/RELATO.md:361,2639` e `docs/BACKLOG.md:2087`. O controle de 11 s confere a sobra e o carimbo de recusa no pareador. A pausa de 8 s pode usar as bordas 08:00–17:00 e o intervalo cadastrado 12:00–13:00, com marcas 12:30:00 S e 12:30:08 E, e verificar um turno de 480 minutos com 60 minutos de janela descontada. `escala/orfa_classe.py:53`, `ponto/turnos.py:130–162,707–771,932–940,1356–1376,1482–1490`.
- R2 pode pedir a autoridade pública `sobras_da_ata` e usar `grade_da_celula` como controle independente da foto. A base usa `_orfas_da_ata`, que entrega duas entradas `12:00` quando a ata contém duas sobras no mesmo minuto. `escala/services/leitor_celula.py:180–190,350–428`.
- V1 pode alimentar a porta pública da grade com marcas sintéticas distintas no mesmo minuto, no mesmo instante e em duas datas civis. A tupla de entrada perde a identidade e a saída conserva apenas `HH:MM`. O RED de conservação pode exigir identidade, instante, linha dona e razão. `escala/utils.py:1367–1368,1483–1484,1863–1891,2138`.
- A reconciliação da grade remove uma entrada pelo minuto, o que fornece o sítio para cobrar a remoção da ocorrência exata. `escala/utils.py:2047–2060,2093`.

Execução dos casos e arquivos integrais dos anexos [nao verificado].

## 27/11 09:48 · sessao c472a9d1c2c0 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

Conferi os cinco arquivos alterados contra o diff anexo e a cópia da base `59600163c`. Os hashes dos cinco blobs conferem com o anexo. A cópia veio sem `.git`, como prevê `CLAUDE.md:375-379`. O objeto do commit, sua relação de parentesco e o estado da VM estão [nao verificado]. Fiz a conferência em modo somente leitura.

## 1. Pronto do aval

| Item | O que a cópia demonstra | O que falta |
|---|---|---|
| Régua | Consulta o juiz antes do atalho `JA VERDE`. Uma recusa grava `FALHOU` e encerra antes do Docker. A consulta ocorre antes das travas e a montagem posterior usa a árvore viva. [bin/regua.sh:57](/home/ronald/codex_ws/2c2f212fca4b8b2af80aa979ddeb0934f18e0201/bin/regua.sh:57), `bin/regua.sh:102-122,183-187`. | RED da régua com teste novo inventariado, antes do opus [nao verificado]. |
| Vigia | `_rodar_testes` ganhou a guarda. O vigia horário ainda faz a suíte do retrato, o reteste dos vermelhos e a confirmação na árvore viva sem consultar o juiz. O próprio vigia Python pode relançar esse shell. [bin/vigia_arvore.sh:77](/home/ronald/codex_ws/2c2f212fca4b8b2af80aa979ddeb0934f18e0201/bin/vigia_arvore.sh:77), `bin/vigia_arvore.sh:38-40,83-88,105-106,131-135`, `app/config/crons.py:296`, `app/core/esteira_vigia.py:750-756,1187-1195`. | A cobertura operacional do vigia falha. RED de ponta a ponta na VM [nao verificado]. |
| Vizinhos da volta | `vizinhos_da_volta.py` seleciona labels, inclusive do teste tocado no diff. `suite.sh --volta` consome essas labels e tenta a pista. Se cair na VM, consulta o juiz antes da fila e novamente na vez. [bin/vizinhos_da_volta.py:92](/home/ronald/codex_ws/2c2f212fca4b8b2af80aa979ddeb0934f18e0201/bin/vizinhos_da_volta.py:92), `bin/vizinhos_da_volta.py:25-30,128-132`, `bin/volta_intermediaria.sh:32-46`, `bin/suite.sh:139-160,214-229,514-544`. | RED do caminho `--volta` na VM [nao verificado]. |
| Mesmo teste na pista | A rota envia SHA e labels e confere recibo e log. O selo de rota usa SSH falso e imprime `Ran` simulado. `bin/suite.sh:433-459,474-509`, `bin/tests/test_suite_rota_pista.sh:27-44,61-64`. | Recibo e log identificando o mesmo teste dos três REDs, na árvore enviada à pista [nao verificado]. |
| Opus final e LIBERA-B3 | A regra da cópia exige a suíte inteira verde antes do opus final. `CLAUDE.md:421-431`. | Suíte inteira verde, opus final `APROVADO` para este diff e cumprimento do restante do LIBERA-B3 [nao verificado]. | [nao verificado]

A lista positiva preservada pelo aval também diverge da cópia. O wrapper disponível proíbe `motor_calculo_v2.py` e `models.py` e aplica uma lista de proibições aos arquivos tocados. `bin/codex_pergunta.sh:159-176,224-234,643-647`.

## 2. Quem escreve, quem lê e o que cada valor significa

| Valor | Escritor, leitores e semântica |
|---|---|
| `.inventario` | O wrapper compara a worktree com o hash-base e publica o inventário **depois** de o construtor sair. `TOCADO` cobre arquivo novo, modificação, remoção e mudança de modo ou tipo. O wrapper relê o inventário para montar `.recusa`. O juiz lê apenas `TOCADO` e caminho, e escolhe a ocorrência de maior `mtime`. Portanto, `TOCADO` prova diferença contra a base daquela chamada. A ocorrência mais recente por caminho, sozinha, não identifica quem produziu os bytes da árvore montada. `bin/codex_pergunta.sh:143-149,195-234,781-807,811-824`, `bin/codex_teste_fora.py:68-94,152-164`. |
| `.id` | O wrapper escreve `obra,papel,hash,pid,fim,sha` após `rc=0` com resposta nova. O SHA é **da resposta**. O disjuntor valida os campos e o SHA sob a trava de resposta. O juiz CCM lê `obra` e `papel` sem essa trava. `obra` vem de `CODEX_OBRA`, enquanto `id_chamada` identifica uma execução. No transcript, `obra` vem da primeira identificação reconhecida no prompt, por outra gramática. `bin/codex_pergunta.sh:117-121,421,549,725-744,797-798,854-860`, `bin/disjuntor.py:159-175,607-632,856-863,880-905`, `bin/codex_teste_fora.py:85-94`. |
| `refs/heads/main` e bytes | Git grava a ref local quando o pouso avança `main`. `git revert` também a avança se `HEAD` estiver nessa branch. O juiz lê a ponta e o histórico em dois comandos, sem fixar um SHA comum, e compara os blobs aos bytes do arquivo montado. A busca histórica pode aceitar um blob de commit lateral alcançável após um merge, mesmo que esse blob jamais tenha estado na ponta de `main`. A ref real na VM está [nao verificado]. `CLAUDE.md:347-354`, `app/core/esteira_vigia.py:1316,1332`, `bin/codex_teste_fora.py:98-129,152-166`. |
| Transcript final | O harness é identificado como escritor do JSONL. O agente entrega o texto via `SubagentHandback`. O B3 lê o diretório da sessão e exige `agentType=verificador`. O CCM reutiliza o parser, varre todas as sessões do caminho fixo, dispensa metadado de tipo e modelo e escolhe o maior timestamp **entre os transcripts com veredito**. Uma verificação posterior sem veredito deixa a anterior em vigor. Nenhum SHA do diff é conferido nessa decisão. `bin/disjuntor.py:438-442,649-671,839-853`, `bin/codex_teste_fora.py:44-45,132-149,167-173`. O escritor físico e o transcript desta volta estão [nao verificado]. |
| Instante, `mtime` e fuso | O juiz compara o timestamp da **primeira linha** do transcript com o `mtime` do inventário e o do arquivo montado. `Z` ou offset explícito vira epoch. Timestamp sem offset é interpretado no fuso local do processo. O `fim` do `.id` é hora civil sem offset e fica fora da comparação. Início do agente, entrega do veredito e modificação no filesystem são eventos distintos. O fuso e os valores reais estão [nao verificado]. `bin/codex_pergunta.sh:854-859`, `bin/codex_teste_fora.py:91-94,132-148,169-173`. |
| RC do juiz e `None` | `main` emite `0` sem pendências detectadas, `1` com pendência e `3` para argv inválido. `NaoSei` sobe como exceção sem tratamento, com traceback e rc não zero. Régua traduz qualquer rc não zero em `1`, suite local em `3` e `_rodar_testes` grava stdout/stderr e devolve `None`. `_isolar` pula o cache, mas `classificar` pode devolver `CAIU_FATIA` com “não isolado”. O congelado devolve recusa explícita, o auto-revert conserva o revert, e a autocura descarta `None` pelo mesmo filtro que descarta `False`. `bin/codex_teste_fora.py:39-49,177-188`, `bin/regua.sh:57-68`, `bin/suite.sh:139-149`, `app/core/esteira_vigia.py:140-144,740-756,1324-1334,1377-1382,1467-1475`. |
| Carimbo, estado e avisos | A régua escreve `.regua_stamp` com impressões da árvore, status, epoch, hora civil e resumo. Leem a própria régua, o pre-push, as cadeias e o rodapé. O vigia escreve `ultimo_revert`, lido pelo limitador de uma hora. O aviso passa pelas portas existentes de Pauta e RELATO. A Pauta deduplica pela chave `auto_revert`, então uma Pauta já viva pode conservar o texto anterior. `bin/regua.sh:38-45,60-67,102-109,214-221`, `bin/pre-push.sh:52-70`, `bin/molde_fatia/cadeia.sh:72,82`, `bin/tickets_rodape.sh:29-33`, `app/core/esteira_vigia.py:852-853,946-948,1072-1074,1324-1329`, `app/core/management/commands/alarme_esteira.py:34-41,54-57`. |

## 3. Classes de reprovação

| Classe | Resultado |
|---|---|
| Segundo juiz registrado | **Ausente como cópia do parser**. O CCM chama `_veredito_e_obra`. **Presente como decisão paralela de quem pode aprovar**. O B3 exige metadado de verificador, enquanto o CCM pode aceitar um agente de outro tipo pelo prompt e pelo `Handback`. A L-011 trata da entrada no registro vivo, que este diff não altera. `bin/codex_teste_fora.py:52-56,138-149`, `bin/disjuntor.py:649-671,839-853`, `app/core/juizes.py:190`, `app/docs/LEIS.md:69`. |
| Segundo escritor fora da porta | **Ausente para inventário, recibo e transcript no delta. Presente como novo ponto de gravação fora da trava da régua**, no próprio script, inclusive com `--only`. Há ainda uma sobrescrita preexistente no vigia. `passada` lê o estado antes do auto-revert e pode gravar essa cópia depois, apagando `ultimo_revert`. `bin/codex_teste_fora.py:68-174`, `bin/regua.sh:47-68,115-122,198-221`, `app/core/esteira_vigia.py:1324-1325,1523-1525,1613,1632-1634`. |
| Relógio fora da borda | **Presente.** O ramo novo da régua lê `date` duas vezes no mesmo carimbo. Epoch e hora impressa podem cair em minutos diferentes. O novo relato do auto-revert formata o `agora` recebido. `bin/regua.sh:64-65`, `app/core/esteira_vigia.py:1294-1301,1329`. |
| Derivação paralela | **Presente.** CCM repete a conversão ISO para epoch e introduz outro leitor de início de transcript. Repete também a fórmula do blob. As `LABELS` continuam vindo da régua, conforme L-075. `bin/codex_teste_fora.py:126-135`, `bin/disjuntor.py:434-435`, `bin/orcamento_contexto.py:161-174`, `bin/codex_pergunta.sh:180-182`, `bin/regua.sh:23`, `bin/suite.sh:209`, `app/docs/LEIS.md:102`. |
| Selo ou RED sem mordida | **Mordida unitária presente.** `test_vigia_ccm.py` simula rc `1`, `2` e `-15` e exige uma única chamada de subprocesso, o juiz. Tirar a guarda quebra essa asserção. Os demais casos simulam o retorno ou o Docker. O teste deixa de fora o juiz real, o cron do vigia e o percurso VM/pista. RED executado no HEAD pelo motivo certo [nao verificado]. `app/core/tests/test_vigia_ccm.py:22-57,61-137`, `.claude/agents/verificador.md:9-10`. |
| Ordem de trava | **Presente.** Régua julga e carimba antes da trava própria e julga antes da espera pela trava de testes. A suite local normal reconsulta sob a trava, mas entre a consulta e a montagem ainda usa uma árvore mutável. O vigia Python tem a trava da passada quando chamado pelo wrapper e seu reteste direto não passa pela trava compartilhada da suíte. `bin/regua.sh:57-68,115-122,183-187`, `bin/suite.sh:149-160,544,575-584`, `bin/trava_teste.sh:35-42`, `bin/vigia_esteira.sh:8-11`, `app/core/esteira_vigia.py:748-772`. |
| Pouso antes do pré-requisito | **Presente no auto-revert.** Reverte e chama deploy antes de consultar o juiz no reteste. A recusa conserva a mudança. Um pouso efetivo desta volta está [nao verificado]. `app/core/esteira_vigia.py:1316-1333`. |
| Campo com dois significados | **Presente.** `.regua_stamp` é descrito como testemunha de que a régua rodou. O ramo CCM escreve `FALHOU` e horário por recusa pré-execução. Na autocura, `None` também cai no filtro de vermelho e `passada` registra `autocura_de`, o que pode impedir nova tentativa após a liberação enquanto a lista de falhas continuar igual. `bin/artefatos_de_estado.txt:28`, `bin/regua.sh:57-68,193-221`, `app/core/esteira_vigia.py:1467-1475,1560-1563,1632-1634`. |
| `except` novo | **Ausente. Zero no diff inteiro**, inclusive `bin/`. A conferência por AST encontrou 25 → 25 handlers no vigia e zero nos dois Python novos. O parser herdado do disjuntor já descarta linhas JSON inválidas, portanto a promessa de traceback para toda leitura que falha é mais ampla que o comportamento. `app/core/esteira_vigia.py:748-772`, `app/core/tests/test_vigia_ccm.py:22-57`, `bin/codex_teste_fora.py:36-65,132-149`, `bin/disjuntor.py:445-453`. |

## 4. O executor recusa todo rc não zero?

**Sim, quando o juiz é chamado para a árvore efetivamente montada e retorna rc não zero**, inclusive traceback. A régua sai `1`, a suite local sai `3` e `_rodar_testes` volta antes de `docker exec` e `docker run`. Se a chamada ou a gravação do motivo levantar no próprio vigia, o teste também fica sem iniciar, mas o chamador recebe exceção em vez de `None`. `bin/regua.sh:57-68`, `bin/suite.sh:139-160`, `app/core/esteira_vigia.py:750-770`.

**Há caminhos locais que escapam.** O vigia horário e o relançamento via Python executam pelo shell sem juiz. Na régua e na suite normal, o bind-mount pode ver alterações posteriores à consulta. Além disso, `--na-vez` julga o app indicado em `_NV_DIR` e executa `"$@"` sem conferir se o comando monta aquele app. O chamador normal fornece o app correspondente e passa pela trava. `app/core/esteira_vigia.py:1187-1195`, `bin/vigia_arvore.sh:83-88,105-106,131-135`, `bin/regua.sh:57,183-187`, `bin/suite.sh:146-160,575-584`.

**Também há liberações com rc 0 indevido pela regra do aval.** Um teste já escrito durante uma chamada ativa ainda pode estar sem inventário, que só nasce no fim. Um teste novo na raiz de `ferias`, como `app/ferias/test_ccm.py`, pode entrar na seleção e ficar fora da guarda, que exige uma pasta `tests/`. O mesmo descompasso existe na cópia para testes na raiz, como `app/ferias/tests_disponibilidade.py`. `bin/codex_pergunta.sh:738-745,781-807`, `bin/codex_teste_fora.py:59-61,78-81,152-157`, `bin/vizinhos_da_volta.py:33-45,92-94`, `app/ferias/tests_disponibilidade.py:30-31`.

**Sim, o auto-revert faz `git revert` primeiro.** Depois invoca `deploy.sh --sem-sombra`, sem usar o rc do deploy, e só então pergunta ao juiz pelo reteste. A recusa grava o estado e chama Pauta/RELATO, deixando o revert aplicado. `app/core/esteira_vigia.py:1316-1333`.

**Há uma regressão adicional nos selos da régua.** As fixtures de `test_suite_rota_pista.sh` e `test_suite_motivo.sh` copiam `suite.sh` sem instalar o juiz. Nos casos locais que exigem chegada à trava, o novo código tenta abrir o script ausente e sai `3` antes dela. A régua roda esses selos antes do Django. É uma falha derivada do código, com execução [nao verificado]. `bin/tests/test_suite_rota_pista.sh:27-37,239-240,377-379`, `bin/tests/test_suite_motivo.sh:23-47,73-78`, `bin/suite.sh:139-145,544`, `bin/regua.sh:152-163`.

## 5. Recusa indefinida de árvore legítima

- **Inventário sem recibo.** Se o inventário mais recente lista `app/folha/tests/test_ccm.py`, o recibo está ausente e a cópia tem bytes inéditos em `main`, a obra vira `None`. Nem um transcript aprovado da obra real pode liberar esse registro. O juiz não aplica prazo de expiração. `bin/codex_teste_fora.py:82-94,158-174`.
- **Merge com bytes diferentes.** Se a cópia legítima tem versão A e o merge levou só a versão B, A continua pendente quando seus bytes nunca apareceram no histórico consultado e não há aprovação reconhecida para a cópia. Uma árvore montada com B passa pela igualdade com `main`. `bin/codex_teste_fora.py:98-129,158-173`.
- **Transcript ausente ou cópia recriada.** Com recibo válido e bytes inéditos em `main`, um transcript ausente do caminho pesquisado mantém a recusa por tempo indeterminado. Mesmo com transcript `APROVADO`, recriar a cópia com os mesmos bytes e `mtime` posterior ao início do agente bloqueia de novo. A condição compara datas e dispensa uma identidade dos bytes aprovados. `bin/codex_teste_fora.py:44-45,132-149,167-174`. Ocorrências desses casos na VM [nao verificado].

**REPROVADO** — Críticos: vigia executa sem juiz, cobertura incompleta do inventário e aprovação sem vínculo ao opus e ao diff (`bin/vigia_arvore.sh:83-88`, `bin/codex_teste_fora.py:59-61,138-173`). Altos: REDs, mesmo teste na pista e opus final [nao verificado], regressão dos selos locais, auto-revert antes do reteste e recusas indefinidas (`bin/tests/test_suite_motivo.sh:23-47,73-78`, `app/core/esteira_vigia.py:1316-1330`, `bin/codex_teste_fora.py:82-94,158-174`). Médios: carimbo fora da trava, relógio duplicado e recusa absorvida por consumidores (`bin/regua.sh:57-68,115-122`, `app/core/esteira_vigia.py:140-144,1467-1475,1560-1563`).

## 27/11 09:48 · sessao dda30177af91 · - · - · origem=Ronald (tmux)

**REPROVADO.** O caminho periódico de `vigia_arvore.sh` continua rodando na VM sem consultar o juiz. O cron está declarado em `app/config/crons.py:296`. O vigia fotografa a árvore viva por `rsync` em `bin/vigia_arvore.sh:38-40`, roda a suíte em `:83-88`, repete os vermelhos em `:105-106` e pode confirmar direto na árvore viva em `:131-135`. Atividade em produção [nao verificado].

- **Régua.** Todo rc não zero encerra antes do atalho e do Docker, com carimbo global `FALHOU`. A checagem antecede ambas as travas e o bind da árvore continua mutável. O novo ramo carimba inclusive no `--only`, cujo caminho normal promete não carimbar. `bin/regua.sh:47-68`, `:102-122`, `:183-199`. RED [nao verificado].
- **Vigia da esteira.** Todo rc não zero encerra `_rodar_testes` antes do Docker, grava o diagnóstico e devolve `None`. `_isolar` evita o cache, mas `classificar` ainda pode declarar `CAIU_FATIA` com motivo `GREEN vermelho (nao isolado)`. O congelado trata `None` separadamente e a autocura o descarta pela mesma verdade booleana de `False`. `app/core/esteira_vigia.py:140-144`, `:740-756`, `:1377-1381`, `:1467-1475`. O teste novo morde a integração local sob simulação de subprocesso e falharia contra a versão anterior, mas não executa o juiz real. `app/core/tests/test_vigia_ccm.py:22-57`, `:61-72`. RED executado [nao verificado].
- **Vizinhos.** Seleciona labels do diff `<base>..HEAD`, inclusive teste novo já presente no diff, sem lançar processo de teste. `bin/vizinhos_da_volta.py:25-30`, `:92-94`, `:116-130`. `bin/volta_intermediaria.sh:33` delega a `suite.sh`, que envia SHA e labels à pista antes do ramo local, e no fallback recusa qualquer não zero antes da fila e novamente sob a trava. `bin/suite.sh:139-160`, `:220-230`, `:454-459`, `:516-520`, `:544`, `:576-583`. RED e mesmo teste na pista [nao verificado].
- **Ordem.** O auto-revert executa `git revert` e invoca deploy antes do reteste que consulta o juiz. Recusa grava `ultimo_revert`, deixa o revert aplicado e avisa; o rc do deploy não é consultado. `app/core/esteira_vigia.py:1316-1333`. Na suíte, `--na-vez` aceita chamada direta e executa o argv recebido sem verificar que seu bind corresponda ao app julgado, embora a chamada normal passe a árvore correta pela trava. `bin/suite.sh:146-160`, `:576-583`.
- **Regressão dos selos de host.** A régua roda os scripts `test_*.sh`. Pelo código, os cenários locais de três selos agora recebem rc 3 antes do resultado esperado porque suas raízes falsas têm `suite.sh` e os dublês antigos, mas não `codex_teste_fora.py`. `bin/regua.sh:156-162`, `bin/suite.sh:139-145`, `:544`, `bin/tests/test_suite_rota_pista.sh:27-37`, `:223-240`, `bin/tests/test_suite_motivo.sh:23-45`, `:240-249`, `bin/tests/test_rm_com_guarda.sh:113-122`, `:346-347`. Execução dos selos [nao verificado].
- **Teste sem inventário.** O wrapper remove o inventário velho antes da chamada e produz o novo só ao fim. A ausência de entrada é interpretada como ausência de pendência. A forma reconhecida pelo juiz também exclui teste Django na raiz de app, que `vizinhos` reconhece. `bin/codex_pergunta.sh:738-745`, `:776-789`, `:800-807`, `bin/codex_teste_fora.py:59-61`, `:152-157`, `bin/vizinhos_da_volta.py:33-45`.

## 27/11 09:47 · sessao 21ebe8cdcd3c · - · - · origem=Ronald (tmux)

**REPROVADO.** O vigia tem caminho local sem consulta, a régua tem janela entre consulta e execução, os selos existentes de suite perdem os casos positivos de VM, e faltam as provas exigidas de RED, pista e opus. [bin/vigia_arvore.sh:65,83,131] [bin/regua.sh:57,115,156,183] [bin/tests/test_suite_rota_pista.sh:27,189] [bin/tests/test_suite_motivo.sh:25,73] [nao verificado] [nao verificado]

Os cinco blobs calculados na cópia conferem com o pós-diff anexo. A cópia veio sem `.git`, então a autenticação do commit inteiro, do pai e do histórico de execução continua [nao verificado]. [nao verificado]

- **Régua:** a guarda cobre rc não zero antes de `JA VERDE`, grava `FALHOU` e sai 1. Consulta e gravação precedem o flock e a espera da suite, sem nova consulta sob a trava. Ela também grava carimbo em `--only`, apesar do contrato do parcial. [bin/regua.sh:47,57,102,115,183,198] RED CCM executado [nao verificado]. [nao verificado]
- **Vigia:** `_rodar_testes` para antes de Docker. O cron da árvore e o próprio vigia Python também chamam `bin/vigia_arvore.sh`, que executa suite e confirmação local sem juiz. [app/core/esteira_vigia.py:748,1187] [app/config/crons.py:296] [bin/vigia_arvore.sh:83,105,131] `test_vigia_ccm.py` morde a regressão de ignorar o rc ou seguir para Docker, com rc simulados 1, 2 e -15. Cobre rc 0 e consumidores de `None` com mocks. RED do HEAD, inventário real, rc 3 e traceback real [nao verificado]. [app/core/tests/test_vigia_ccm.py:22,38,50,60] [.claude/agents/verificador.md:10]
- **Vizinhos:** o helper lê a árvore e emite labels, a suite decide a execução. O caminho VM é consultado antes da fila e sob a trava, o recorte com cópia pode sair pela pista. O critério Django do helper inclui `test*.py` na raiz do app, enquanto o CCM exige diretório `tests/`. [bin/vizinhos_da_volta.py:25,33,92,116] [bin/codex_teste_fora.py:59] [bin/suite.sh:139,149,214,514,544] RED CCM da volta e mesmo teste na pista [nao verificado]. O selo da pista usa SSH, trava e Docker simulados. [bin/tests/test_suite_rota_pista.sh:11,27,56] `opus final APROVADO`, suite inteira verde e restante do LIBERA-B3 [nao verificado]. [CLAUDE.md:421,427] [nao verificado]
- **Regressão dos selos de host:** as fixtures de `test_suite_motivo` e `test_suite_rota_pista` copiam `suite.sh` sem copiar ou simular `codex_teste_fora.py`. Os casos positivos que esperam a trava recebem 3 antes dela. A própria régua executa esses selos antes do Django. [bin/tests/test_suite_motivo.sh:25,73] [bin/tests/test_suite_rota_pista.sh:27,189,295] [bin/suite.sh:139,544] [bin/regua.sh:152] [nao verificado]
- **Classes:** o parser de veredito é compartilhado, mas a seleção da aprovação é nova, ampla e sem a validação `agentType=verificador` do B3. Projeções repetem o cálculo de blob e ISO→epoch. A régua também acrescenta duas leituras do relógio do host, e o carimbo documentado como prova de que rodou passa a registrar recusa sem execução. Há novo ponto de gravação no mesmo escritor antes do flock. Pauta/RELATO continuam pelas portas existentes. [bin/disjuntor.py:434,649,839] [bin/codex_teste_fora.py:126,132,138] [bin/codex_pergunta.sh:180] [bin/regua.sh:57,115,214] [bin/artefatos_de_estado.txt:28] [app/core/esteira_vigia.py:825,946,1328] Zero novos `except` sintáticos no diff anexo. O `auto_revert` aplica o revert e chama deploy antes da consulta; se ela recusa, mantém revert, grava o limitador e avisa. [app/core/esteira_vigia.py:1316,1320,1322,1324,1326] [nao verificado]

## 27/11 09:46 · sessao 855e05c10268 · RC-SEM-PROXIMA · censo · origem=Code

## CENSO
- leitores: A cópia fornecida contém `RC SEM COLHER`. `RC SEM PROXIMA` aparece no anexo, com presença no HEAD [nao verificado]. As linhas abaixo foram conferidas na cópia do hash informado. A identidade Git do HEAD é [nao verificado].
  - `bin/disjuntor.py:1967` executa o juiz pela string `ESPERA_UNICA` antes do laço. `bin/disjuntor.py:2166`, `bin/disjuntor.py:2170` e `bin/disjuntor.py:2173` oferecem essa string nas negativas de `espera_nega`. `bin/disjuntor.py:2192` faz o mesmo na negativa de polling. O PreToolUse chama ambos em `bin/disjuntor.py:2226`, ligado por `.claude/settings.json:68` e `bin/disjuntor_settings.json:11`. `espera_nega` verifica a forma do `find` e o teto do `timeout`. A presença da chamada ao juiz nessa validação é [nao verificado] em `bin/disjuntor.py:2163`, `bin/disjuntor.py:2169` e `bin/disjuntor.py:2172`.
  - `bin/tests/test_rc_sem_colher.sh:2` declara o selo, `bin/tests/test_rc_sem_colher.sh:18` verifica a presença do arquivo, `bin/tests/test_rc_sem_colher.sh:50` executa o CLI e `bin/tests/test_rc_sem_colher.sh:54` confere a saída. O selo usa `ESPERA_UNICA` para acordar com lista, aguardar publicação por rename e propagar falha em `bin/tests/test_rc_sem_colher.sh:75`, `bin/tests/test_rc_sem_colher.sh:84`, `bin/tests/test_rc_sem_colher.sh:86` e `bin/tests/test_rc_sem_colher.sh:95`. Confere a forma e muda cópias do juiz e do disjuntor nos mutantes de `bin/tests/test_rc_sem_colher.sh:103`, `bin/tests/test_rc_sem_colher.sh:111`, `bin/tests/test_rc_sem_colher.sh:124` e `bin/tests/test_rc_sem_colher.sh:128`. O selo de agente usa a constante para aceitação, polling e paridade com a documentação em `bin/tests/test_agente_no_modelo.sh:210`, `bin/tests/test_agente_no_modelo.sh:244`, `bin/tests/test_agente_no_modelo.sh:278`, `bin/tests/test_agente_no_modelo.sh:283`, `bin/tests/test_agente_no_modelo.sh:301`, `bin/tests/test_agente_no_modelo.sh:345` e `bin/tests/test_agente_no_modelo.sh:346`.
  - Documentação da forma e do contrato em `CLAUDE.md:335`, `CLAUDE.md:341`, `app/docs/RELATO.md:13639`, `app/docs/RELATO.md:13640`, `app/docs/RELATO.md:13648`, `app/docs/RELATO.md:13655`, `app/docs/PROMPTS.md:3091`, `app/docs/PROMPTS.md:3093` e `app/docs/BACKLOG.md:2183`. `~/vercodex.py` é citado em `app/docs/PROMPTS.md:3070` e a chamada com `--min-idade 900` em `app/docs/RELATO.md:13640` e `bin/rc_sem_colher.py:51`. Seu código, a forma de chamada, o filtro de saída e qualquer import direto do juiz são [nao verificado]. `logs/espera_unica.sh` é citado em `app/docs/RELATO.md:13598`, sob `logs/` ignorado em `.gitignore:53`. Seu conteúdo e versionamento são [nao verificado].
  - Das marcas, o parser verificado é `bin/rc_sem_colher.py:26`, `bin/rc_sem_colher.py:33` e `bin/rc_sem_colher.py:80`. O Stop lê o topo e chama o checador de prova de ato em `bin/hook_stop_fila1.py:462` e `bin/hook_stop_fila1.py:1441`. O handoff lê `PAREI`/`lei`/`!` no topo e consulta a ordem viva em `bin/handoff_sessao.sh:97`, `bin/handoff_sessao.sh:116` e `bin/handoff_sessao.sh:320`. `gerar_estado` lê raias e interrupções em `bin/gerar_estado.py:215` e `bin/gerar_estado.py:236`. `gerar_codex_estado` usa `chamadas.log` em `bin/gerar_codex_estado.py:41`. A publicação carrega o RELATO como documento em `bin/relato.sh:125` e `bin/relato.sh:130`. A leitura das marcas por esses consumidores ou pelo vercodex é [nao verificado]. O Stop como leitor de RC sem colher aparece no contrato anterior em `app/docs/RELATO.md:137` e `app/docs/PROMPTS.md:3070`.
- escritores:
  - Marcas do RELATO são atribuição da sessão pelo fluxo de `CLAUDE.md:334` e `CLAUDE.md:337`, com registros de `superado` em `app/docs/RELATO.md:13625` e `app/docs/RELATO.md:13644` e de `colhido` em `app/docs/RELATO.md:13645` e `app/docs/RELATO.md:13651`. A autoria de cada edição e outro emissor de marcas são [nao verificado]. Há escritores do documento para alarmes e topo de censo, tabela de cortes e deploys agendados, respectivamente `app/core/esteira_vigia.py:815`, `app/core/esteira_vigia.py:865`, `bin/disjuntor.py:329`, `bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136` e `bin/deploy_agendado.sh:77`. A fixture grava marcas e `.rc` em árvore temporária em `bin/tests/test_rc_sem_colher.sh:22`, `bin/tests/test_rc_sem_colher.sh:31` e `bin/tests/test_rc_sem_colher.sh:40`.
  - `bin/codex_pergunta.sh:446` confina a resposta sob `logs/`. O trap de saída publica `<resposta>.rc` por temporário e rename para o construtor com a trava, em `bin/codex_pergunta.sh:568`, `bin/codex_pergunta.sh:572`, `bin/codex_pergunta.sh:573`, `bin/codex_pergunta.sh:574` e `bin/codex_pergunta.sh:582`. O RELATO cita `lanca_c16.sh` e `F4e/lanca2.sh`, suite por `suite_c1g/roda.sh`, mutantes por `c1g/mut.sh`, RED em `c3/red.rc` e recibo de certificação em `pouso_1900/certifica.rc`. Os corpos dos lançadores citados e o inventário dos escritores avulsos são [nao verificado]. `bin/pista_certifica.sh:13` recebe o recibo do chamador. `bin/suite.sh:459` coleta o recibo e `bin/suite.sh:585` grava o rc interno em temporário. `app/docs/RELATO.md:13637`, `app/docs/RELATO.md:13638`, `app/docs/RELATO.md:13606`, `app/docs/RELATO.md:13612`, `app/docs/RELATO.md:13663`, `app/docs/RELATO.md:13635`.
- decisoes:
  - Preservar `sem_colher`, inclusive `--min-idade` e os códigos do CLI. A saída adicional cabe no mesmo juiz, com a mesma seleção de obra e rc. `ESPERA_UNICA` já acorda com rc `3` na abertura e entra no laço com rc `0`. Durante o laço, só procura publicação de `.rc` por `ctime`, até 300 s. O selo de forma precisa cobrir a chamada, além do `find` e do teto. `bin/rc_sem_colher.py:50`, `bin/rc_sem_colher.py:76`, `bin/rc_sem_colher.py:104`, `bin/rc_sem_colher.py:107`, `bin/disjuntor.py:1967`, `bin/disjuntor.py:1969`, `bin/disjuntor.py:2172`, `bin/tests/test_rc_sem_colher.sh:103`.
  - Tratar `colhido` e `superado` como marcas por rc. Hoje ambos tiram o rc de `sem_colher`. O BACKLOG tem outro predicado de fechamento, que o handoff e a lista de interrompidas reutilizam. A divergência já cabe nos registros da cópia. `VIGIA-DISCO` tem um rc `superado` e o BACKLOG ainda registra construção. `O30-LOTE-MINIMA` registra pouso e `NO AR` sem negrito no BACKLOG, enquanto a suite da c2 tem `colhido` e o `superado` da c1 pertence a outro rc. `bin/rc_sem_colher.py:26`, `bin/rc_sem_colher.py:80`, `bin/hook_stop_fila1.py:55`, `bin/hook_stop_fila1.py:164`, `bin/handoff_sessao.sh:181`, `bin/interrompidas_estado.py:125`, `app/docs/RELATO.md:13633`, `app/docs/BACKLOG.md:2150`, `app/docs/BACKLOG.md:2184`, `app/docs/RELATO.md:13674`, `app/docs/RELATO.md:13683`.
  - Manter a pergunta no juiz de rc. Os candidatos encontrados respondem a outras condições. O Stop lê item fechado, congelado ou em espera no BACKLOG. O B3 lista obras com merge retido por reprovação. O handoff mostra o próximo item da ordem viva e ramos `raia20-*` à frente do main. O ESTADO lista raias sem agente e obras interrompidas por bug. O alarme de sessão mede ociosidade da sessão. A esteira mede fatia pronta fora da fila de integração ou falta de atividade da fila. O placar de tickets lê uma parada declarada. Outro predicado com a mesma combinação de rc, marca e inatividade é [nao verificado], portanto ampliar o juiz existente não duplica os predicados encontrados. `bin/hook_stop_fila1.py:176`, `bin/disjuntor.py:965`, `bin/handoff_sessao.sh:237`, `bin/handoff_sessao.sh:320`, `bin/raias_estado.py:117`, `bin/interrompidas_estado.py:105`, `bin/alarme_sessao_ociosa.py:33`, `app/core/esteira_vigia.py:460`, `app/core/esteira_vigia.py:584`, `app/core/placar_tickets.py:114`.
  - ESMERIL-SEMANTICA. O juiz atual compara o `HH:MM` entre parênteses com o minuto local do `mtime`, sem data nem segundos na chave. O `HH:MM` do início só valida a forma. Refazer o mesmo caminho no mesmo minuto, ou em outro dia com o mesmo `HH:MM` e a marca ainda no RELATO, pode reaproveitar a marca. A varredura inclui exatamente 24 h, tolera outro arquivo até exatamente `rc + 5 s` e exige idade do rc estritamente maior que `--min-idade`. O selo usa `date` local para montar a marca, e a convenção documentada para a trilha é `America/Sao_Paulo`. A configuração efetiva dos processos e a paridade com o vercodex são [nao verificado]. `bin/rc_sem_colher.py:26`, `bin/rc_sem_colher.py:53`, `bin/rc_sem_colher.py:59`, `bin/rc_sem_colher.py:63`, `bin/rc_sem_colher.py:69`, `bin/rc_sem_colher.py:76`, `bin/rc_sem_colher.py:79`, `bin/tests/test_rc_sem_colher.sh:30`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:6`.
  - No anexo, a maior hora de `colhido` é reconstruída a partir do dia local do rc, somando `86400` se a hora cair antes do piso calculado. Com `mtime` fracionário, esse piso conserva a fração e pode mandar uma marca do mesmo minuto para o dia seguinte. A idade usa `> 900 s` a partir da marca reconstruída. O anexo também exclui o rc se houver outro arquivo escrito mais de 5 s depois dele, mesmo que esse arquivo já tenha mais de 15 min. A existência dessas regras na cópia é [nao verificado]. O juiz atual descarta a hora da linha e compara a idade do próprio rc. `bin/rc_sem_colher.py:29`, `bin/rc_sem_colher.py:76`.

perguntas de lei: 2
- Os 15 min correm desde a marca `colhido`, com qualquer escrita posterior ao rc além de 5 s já valendo como etapa seguinte, como no anexo, ou desde o último sinal de atividade da obra, permitindo que ela volte à lista após nova inatividade (`bin/rc_sem_colher.py:76`, regra adicional no HEAD [nao verificado])?
- Para `RC SEM PROXIMA`, basta o `superado` do rc mais novo para decidir encerramento, como no anexo, ou o fechamento no BACKLOG também deve excluir a obra quando as fontes divergem (`CLAUDE.md:337`, `bin/hook_stop_fila1.py:164`, `app/docs/RELATO.md:13633`, `app/docs/BACKLOG.md:2150`)?

## 27/11 09:44 · sessao d5d9b0c69d43 · PAR-PELO-TURNO · construtor · origem=Code

**c1v8. Um teste tocado:** `test_3d_resumo_da_tela_inclui_o_periodo_que_o_motor_datou_na_vespera`. O teste agora usa as nove chaves do literal de `espelho_do_colab`, com os mesmos valores iniciais. A asserção original foi preservada. [Teste:100](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:100), [literal canônico:1236](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/services/espelho.py:1236) [nao verificado]

A busca nos outros três módulos da obra não encontrou montagem manual de resumo. [test_par_pelo_turno.py:1](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno.py:1), [test_par_pelo_turno_bordas.py:1](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno_bordas.py:1), [test_par_pelo_turno_c1v6.py:1](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno_c1v6.py:1)

### Diff

```diff
--- a/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py
+++ b/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py
@@ -109,7 +109,10 @@
         periodo, = r.periodos + r.periodos_ft
         self.assertEqual(r.dia_da_jornada[id(periodo)], dia)
         self.assertGreater(periodo.horas_trabalhadas, 0)
-        resumo = {'turnos': 0, 'turnos_abertos': 0}
+        # Resumo inicial de espelho_do_colab em ponto/services/espelho.py:1236.
+        resumo = {'total_trabalhadas': 0.0, 'total_noturnas': 0.0, 'total_extras': 0.0,
+                  'total_intra_indenizada': 0.0, 'turnos': 0, 'turnos_abertos': 0, 'dias_inconsistentes': 0,
+                  'noturnas_relogio': 0.0, 'acrescimo_noturno': 0.0}
         montar_dias(self.colab, escala, r, autoridade.batidas, dia, amanha,
                     self.agora.date(), dia.year, dia, dia, resumo, agora=self.agora)
         self.assertAlmostEqual(resumo['total_trabalhadas'], periodo.horas_trabalhadas)
```

### RED e mutantes

No ensaio isolado em memória, `atual` passou. `base` e `sem-contador` reproduziram `KeyError: 'dias_inconsistentes'`. `total-adulterado` fez a asserção original falhar com `8.0 != 7.0`. [Soma:454](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/services/espelho.py:454), [contador:685](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/services/espelho.py:685), [asserção:118](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:118) [nao verificado]

```bash
ensaio() {
  PYTHONDONTWRITEBYTECODE=1 python3 -I -S - "$1" <<'PY'
import ast
import subprocess
import sys
from pathlib import Path
from types import SimpleNamespace
from unittest import TestCase

modo = sys.argv[1]
alvo = Path("relatorios/tests/test_par_pelo_turno_c1v6_pdf.py")
fonte = (subprocess.check_output(["git", "show", f"HEAD:app/{alvo}"], text=True)
         if modo == "base" else alvo.read_text())
teste = next(n for n in ast.walk(ast.parse(fonte)) [nao verificado]
             if isinstance(n, ast.FunctionDef) and n.name.startswith("test_3d_"))
literal = next(n.value for n in teste.body if isinstance(n, ast.Assign) [nao verificado]
               and ast.unparse(n.targets[0]) == "resumo")
resumo = ast.literal_eval(literal)
if modo == "sem-contador":
    del resumo["dias_inconsistentes"]
if modo == "total-adulterado":
    resumo["total_trabalhadas"] = 1.0
periodo = SimpleNamespace(horas_trabalhadas=7.0)
servico = Path("ponto/services/espelho.py")
linhas = servico.read_text().splitlines()
escopo = dict(resumo_mes=resumo, resumo=resumo, self=TestCase(), periodo=periodo,
              _k="total_trabalhadas", _v=periodo.horas_trabalhadas)
for linha in (454, 685):
    exec(compile("\n" * (linha - 1) + linhas[linha - 1].strip(), str(servico), "exec"), escopo)
exec(compile(ast.Module(body=teste.body[-1:], type_ignores=[]), str(alvo), "exec"), escopo) [nao verificado]
PY
}

ensaio atual
ensaio base
ensaio sem-contador
ensaio total-adulterado
```

### Recorte sugerido para o Code

Execução Django pendente no banco de ensaio. [não verificado], [configuração CI:6](/home/ronald/wt-codex-par-pelo-turno-010806/app/config/settings/ci.py:6)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  ponto.tests.test_par_pelo_turno \
  ponto.tests.test_par_pelo_turno_bordas \
  ponto.tests.test_par_pelo_turno_c1v6 \
  relatorios.tests.test_par_pelo_turno_c1v6_pdf
```

## 27/11 09:32 · sessao eaf2b30bba07 · SONDA-ESPURIAS-RAMO2 · leitura · origem=Code

**1. Os três colaboradores faltaram pelo corte global `detalhes[:20]`.** A s3 conta toda diferença de registro no `TETO`, exige também diferença de minutos para gerar `COLAB` e só imprime as primeiras 20 linhas `COLAB` acumuladas entre as empresas. [logs/sombra/espurias_txt11_s3.py:333–349,406–408,435–436, anexo]

As 20 linhas exibidas são de 11 colaboradores da empresa 2. Nessa empresa, o total de `0025` é −78,0 min e as linhas exibidas somam −8,4 min, deixando −69,6 min. Em `0243`, o total é +693,0 min e as linhas exibidas somam +633,0 min, deixando +60,0 min. Na empresa 3, nenhuma linha `COLAB` saiu e os deltas são +113,4 min em `0025` e +120,0 min em `0243`. São quatro pares distintos com delta de minutos, exatamente os quatro que faltam para os 24 pares do `TETO`. Portanto, o filtro `ma != mb` não eliminou nenhum par nesse resultado. Faltaram dois colaboradores da empresa 2 e um da empresa 3. Os IDs dos três ficam **[nao verificado]**. [logs/espurias_txt11_s3.out:27–59, anexo]

**2. A0 existe como leitura do gravado pelo mesmo montador.** O botão de exportar recalcula o fechamento em memória e injeta esses objetos na classificação e na montagem. Para A0, a sonda passa `fechamentos=None`: `classificar_export` consulta `FechamentoMensal` gravado e `montar_linhas_export` consulta o mesmo gravado com `STATUS_NO_TXT`. O montador lê os campos do fechamento e as datas de falta, e `conteudo_do_txt` serializa as linhas. Esse caminho dispensa a chamada ao motor de fechamento. O `original` rotulado `A0` na s3 já chamava o motor. [app/folha/views.py:214–219,269–288] [app/folha/export.py:402–452,525–539,696–755,791–797] [logs/sombra/espurias_txt11_s3.py:171–177,400–410, anexo]

Na sonda, `reg` é o SHA‑1 dos bytes UTF‑8 dos registros 10 e seus filhos 11, com os CRLF do TXT e na ordem emitida para o par. `FIM colabs` conta todos os colaboradores das três empresas. No comparador, um lado ausente vale zero minutos e tem registro ausente. `no_teto` testa o par colaborador/rubrica, e `RAMO2 muda` conta colaboradores distintos do teto com alguma mudança em R. [espurias_ramo2_s2.py:198–242,343,377] [compara_ramo2_s2.py:9,53–61,89–108] [app/folha/export.py:525–539]

A1, B e R usam as mesmas portas de relavratura da s3, com escritas temporárias em savepoints revertidos. O próprio `--rodar` tem uma etapa anterior à sonda que lê a lavra de produção e a grava na sombra. A execução no container e os resultados de R ficam **[nao verificado]**. [logs/sombra/espurias_txt11_s3.py:152–168,310–325, anexo] [bin/sombra.sh:136–167,483–493]

```python
# espurias_ramo2_s2.py
import logging
import sys
from collections import defaultdict
from contextlib import redirect_stderr, redirect_stdout
from datetime import date, datetime, time, timedelta
from decimal import Decimal
from hashlib import sha1
from io import StringIO
from traceback import walk_tb
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from chamados import fcm_utils, push_utils [nao verificado]
from colaboradores.models import Colaborador, Empresa
from escala import utils as grade
from escala.models import CelulaDia
from escala.orfa_classe import classificar_orfas
from folha import export
from ponto import turnos
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.nucleo import recompute_turnos
from ponto.services import cartorio, fechamento

LADOS = "A0A1B"
MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA = (date(2026, 10, 21), date(2026, 11, 20))
REF_S1 = "app/escala/orfa_classe.py:53-78"


class FalhaSonda(RuntimeError):
    pass


class FalhaRegistrada(BaseException):
    pass


class HoraComId(str):
    pass


class InstanteComId(datetime):
    def strftime(self, formato):
        hora = super().strftime(formato)
        if formato == "%H:%M" and hasattr(self, "batida_id"):
            hora = HoraComId(hora)
            hora.batida_id = self.batida_id
        return hora


def log_sem_dados(_logger, registro):
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        falha = FalhaRegistrada(f"{registro.pathname}:{registro.lineno} {tipo}")
        if erro is not None:
            falha = falha.with_traceback(erro.__traceback__)
        raise falha from None


def alvos_da_s1(empresa, janela, agora):
    # logs/sombra/espurias_txt11_s3.py:79-149
    ini, fim = janela
    primeiro = timezone.make_aware(datetime.combine(ini - timedelta(days=1), time.min))
    ultimo = timezone.make_aware(datetime.combine(fim + timedelta(days=3), time.min))
    ultimo -= timedelta(microseconds=1)
    ref = fim + timedelta(days=1)
    dias = (ref - ini).days + 1
    alvos = {}

    localtime_real = timezone.localtime
    pessoas = Colaborador.objects.filter(empresa=empresa).order_by("pk")
    with (
        patch.object(grade, "Exception", (), create=True),
        patch.object(turnos, "Exception", (), create=True),
    ):
        for colab in pessoas.iterator(chunk_size=200):
            batidas = list(turnos.batidas_apuraveis(colab, primeiro, ultimo))
            if not batidas:
                continue

            kwargs = dict(dias=dias, ref=ref, agora=agora)
            linhas = grade.montar_grade_prevista_periodo(colab, batidas, **kwargs)["dias"]
            if len(linhas) != dias:
                raise FalhaSonda(f"Grade fora da janela colab_id={colab.pk} {REF_S1}")
            classes = [classificar_orfas(linha) for linha in linhas[1:]]
            if not any(classes):
                continue

            # Só transporta a identidade. Os datetime e HH:MM mantêm seus valores.
            # [app/escala/utils.py:314-327,699-705,1367-1385,1766-1782,1882-1891]
            por_objeto = {id(b.timestamp): b.pk for b in batidas}

            def localtime_com_id(value=None, timezone=None):
                instante = localtime_real(value, timezone)
                pk = por_objeto.get(id(value))
                if pk is None or sys._getframe(1).f_globals is not vars(grade):
                    return instante
                marcado = InstanteComId(
                    instante.year, instante.month, instante.day,
                    instante.hour, instante.minute, instante.second,
                    instante.microsecond, tzinfo=instante.tzinfo, fold=instante.fold,
                )
                marcado.batida_id = pk
                return marcado

            with patch.object(timezone, "localtime", localtime_com_id):
                marcadas = grade.montar_grade_prevista_periodo(
                    colab, batidas, **kwargs,
                )["dias"]

            ids = set()
            for linha, marcada, classificacao in zip(
                linhas[1:], marcadas[1:], classes, strict=True,
            ):
                horas = marcada.get("orfas") or []
                if horas != (linha.get("orfas") or []):
                    raise FalhaSonda(f"Instrumentacao mudou a grade colab_id={colab.pk} {REF_S1}")
                for hora in horas:
                    if hora in classificacao:
                        pk = getattr(hora, "batida_id", None)
                        if pk is None:
                            raise FalhaSonda(f"Sobra sem identidade colab_id={colab.pk} {REF_S1}")
                        ids.add(pk)
            alvos[colab.pk] = ids

    return alvos


def reler_celulas(empresa, janela, alvo_colabs, agora):
    ini, fim = turnos.janela_com_pad(*janela)
    hoje = timezone.localdate(agora)
    emissor = Emissor(stdout=StringIO(), stderr=StringIO())
    pessoas = Colaborador.objects.filter(
        empresa=empresa, pk__in=alvo_colabs,
    ).order_by("pk")
    for colab in pessoas:
        recompute_turnos(colab, ini, fim)
        celulas = list(CelulaDia.objects.filter(
            colaborador=colab, data__range=(ini, fim), data__lt=hoje,
        ).order_by("data"))
        if celulas:
            cartorio.julgar_colab(
                colab, celulas, hoje=hoje, agora=agora, apply_=True,
                emissor=emissor, out=cartorio.contadores(), forcar=True,
            )


def medir_txt(empresa, janela, *, recalcular=False):
    # None le o gravado nas duas portas. app/folha/export.py:720-731,791-797
    objetos = None
    if recalcular:
        # Preserva a guarda da s3 e deixa os demais erros subirem.
        # logs/sombra/espurias_txt11_s3.py:171-177
        with patch.object(fechamento, "Exception", fechamento._GradeSemCelula, create=True):
            objetos = list(fechamento.recalcular_fechamento_mes(
                MES, ANO, empresa_id=empresa.pk, somente_leitura=True,
                retornar_objeto=True, lavrar_dias_pagos=False,
            ).values())
    itens = export.classificar_export(empresa, MES, ANO, fechamentos=objetos)

    eventos = export.eventos_do_fechamento
    montar = export.montar_linha_10
    atual = None
    pais = []

    def observar_eventos(fech, *args, **kwargs):
        nonlocal atual
        atual = fech.colaborador
        return eventos(fech, *args, **kwargs)

    def observar_linha_10(matricula, comp, rubrica, tipo, valor, cod_sistema):
        linha = montar(matricula, comp, rubrica, tipo, valor, cod_sistema)
        mat = str(matricula).zfill(10)
        cod = str(rubrica).zfill(4)
        inicio = len(f"10{mat}{comp}{cod}11")
        fim = -len(str(cod_sistema).zfill(10))
        if atual is None or not (cod.isascii() and cod.isdigit()):
            raise FalhaSonda("Registro 10 sem autoria ou rubrica numerica")
        pais.append((atual, cod, tipo, inicio, fim, linha))
        return linha

    # Cada bloco do montador comeca chamando eventos para o seu fechamento.
    # app/folha/export.py:734-755
    with (
        patch.object(export, "eventos_do_fechamento", observar_eventos),
        patch.object(export, "montar_linha_10", observar_linha_10),
    ):
        linhas, _ = export.montar_linhas_export(
            empresa, MES, ANO, itens=itens, fechamentos=objetos,
        )

    grupos = defaultdict(list)
    pai_txt = None
    datas = None
    fichas = iter(pais)
    for linha in export.conteudo_do_txt(linhas).splitlines():
        parte = export.partes_da_linha(linha)
        if linha.startswith("10"):
            colab, cod, tipo, inicio, fim, linha_montada = next(fichas)
            if linha != linha_montada:
                raise FalhaSonda("Registro 10 fora da ordem de montagem")
            # Mantem o valor numerico mesmo quando o layout do pai e irregular.
            # logs/sombra/espurias_txt11_s3.py:183-191,207-216
            cru = parte["valor"] if parte is not None else int(linha[inicio:fim])
            pai_txt, datas = [linha], []
            grupos[colab.pk, cod].append((colab, tipo, cru, pai_txt, datas))
        elif pai_txt is not None and parte is not None and parte["reg"] == "11":
            pai_txt.append(linha)
            datas.append(parte["data"])
        else:
            raise FalhaSonda("Registro 11 invalido ou sem pai")
    if next(fichas, None) is not None:
        raise FalhaSonda("Montador chamou registro 10 que nao saiu no TXT")

    faltas = {}
    foto = {}
    for chave, grupo in sorted(grupos.items()):
        total = Decimal(0)
        linhas_rubrica = []
        for colab, tipo, cru, bloco, datas in grupo:
            linhas_rubrica.extend(bloco)
            if tipo != "dias":
                total += Decimal(cru) * Decimal("0.6")
            else:
                if not datas:
                    raise FalhaSonda(f"Rubrica em dias sem filhos colab_id={colab.pk}")
                if colab.pk not in faltas:
                    mapa = export.minutos_de_falta_por_dia(colab, *janela)
                    faltas[colab.pk] = {
                        dia.strftime("%Y%m%d"): Decimal(str(minutos))
                        for dia, minutos in mapa.items()
                    }
                total += sum(faltas[colab.pk][dia] for dia in sorted(set(datas)))
        registro = export.conteudo_do_txt(linhas_rubrica).encode("utf-8")
        foto[chave] = (total, sha1(registro).hexdigest())
    return foto


def sequencias():
    # bin/sombra_db.sh:45
    quote = connection.ops.quote_name
    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT n.nspname, c.relname, s.seqincrement, s.seqmin, s.seqmax, s.seqcycle
            FROM pg_catalog.pg_sequence s
            JOIN pg_catalog.pg_class c ON c.oid = s.seqrelid
            JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
            WHERE n.nspname IN (%s, %s)
            ORDER BY n.nspname, c.relname
            """,
            ("juliani", "public"),
        )
        saida = {}
        for schema, nome, passo, minimo, maximo, ciclo in cursor.fetchall():
            tabela = f"{quote(schema)}.{quote(nome)}"
            cursor.execute(f"SELECT last_value, is_called FROM {tabela}")
            valor, chamado = cursor.fetchone()
            saida[tabela] = (valor, chamado, passo, minimo, maximo, ciclo)
    return saida


def reiniciar_no_savepoint(estado):
    with connection.cursor() as cursor:
        for nome, (valor, chamado, passo, minimo, maximo, ciclo) in estado.items():
            proximo = valor + passo if chamado else valor
            if ciclo and proximo > maximo:
                proximo = minimo
            if ciclo and proximo < minimo:
                proximo = maximo
            cursor.execute(f"ALTER SEQUENCE {nome} RESTART WITH {int(proximo)}")


def rodada(empresa, janela, alvos, pks, agora, seq_antes):
    # A1/B da s3 usam savepoints distintos. logs/sombra/espurias_txt11_s3.py:310-325
    with transaction.atomic():
        reiniciar_no_savepoint(seq_antes)
        if pks:
            marcas = turnos.batidas_apuraveis(None).filter(
                colaborador__empresa=empresa, pk__in=pks,
            )
            if marcas.update(retratada_em=agora) != len(pks) or marcas.exists():
                raise FalhaSonda(f"Retirada incompleta empresa={empresa.pk}")
        reler_celulas(empresa, janela, alvos, agora)
        foto = medir_txt(empresa, janela, recalcular=True)
        transaction.set_rollback(True)
    return foto


def fatos_de_sobra(empresa, alvos, pks):
    marcas = turnos.batidas_apuraveis(None).filter(
        colaborador__empresa=empresa, pk__in=pks,
    ).order_by("colaborador_id", "timestamp", "pk")
    encontrados = set()
    for pk, cid, timestamp, tipo in marcas.values_list(
        "pk", "colaborador_id", "timestamp", "tipo",
    ):
        if pk not in alvos.get(cid, ()) or tipo not in ("E", "S"):
            raise FalhaSonda(f"Batida invalida batida_id={pk} colab_id={cid}")
        encontrados.add(pk)
        local = timezone.localtime(timestamp)
        yield (
            f"SOBRA empresa={empresa.pk} colab_id={cid} data={local:%Y-%m-%d} "
            f"hora={local:%H:%M:%S} tipo={tipo} batida_id={pk}"
        )
    if encontrados != pks:
        raise FalhaSonda(f"Leitura de sobras incompleta empresa={empresa.pk}")


def medir():
    if LADOS not in ("A0A1B", "R"):
        raise FalhaSonda("LADOS invalido")
    if (
        not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict["HOST"] != "sombra_db"
        or connection.settings_dict["NAME"] != "sombra"
        or connection.schema_name != "juliani"
    ):
        raise FalhaSonda("Conexao fora da sombra")

    fatos = []
    with (
        patch.object(logging.Logger, "handle", log_sem_dados),
        redirect_stdout(StringIO()),
        redirect_stderr(StringIO()),
        transaction.atomic(durable=True),
    ):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ WRITE")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise FalhaSonda("Banco ou schema fora da sombra")

        empresas = list(Empresa.objects.filter(pk__in=EMPRESAS).order_by("pk"))
        if tuple(e.pk for e in empresas) != EMPRESAS:
            raise FalhaSonda("Escopo incompleto de empresas")
        n_colabs = Colaborador.objects.filter(empresa_id__in=EMPRESAS).count()
        seq_antes = sequencias()
        agora = timezone.now()
        with (
            patch.object(timezone, "now", new=lambda: agora),
            patch.object(push_utils, "_enviar_push_usuario", return_value={"ok": False}), [nao verificado]
            patch.object(fcm_utils, "push_fcm_colaborador", return_value=False), [nao verificado]
        ):
            for empresa in empresas:
                janela = export.janela_competencia(empresa, MES, ANO)
                if janela != JANELA:
                    raise FalhaSonda(f"Janela fora do aval empresa={empresa.pk}")
                alvos = alvos_da_s1(empresa, janela, agora)
                fotos = {}
                if LADOS == "A0A1B":
                    fotos["A0"] = medir_txt(empresa, janela)
                    fotos["A1"] = rodada(empresa, janela, alvos, set(), agora, seq_antes)
                    pks = {pk for ids in alvos.values() for pk in ids}
                    fatos.extend(fatos_de_sobra(empresa, alvos, pks))
                    fotos["B"] = rodada(empresa, janela, alvos, pks, agora, seq_antes)
                else:
                    fotos["R"] = rodada(empresa, janela, alvos, set(), agora, seq_antes)

                for lado, foto in fotos.items():
                    for (cid, cod), (minutos, reg) in foto.items():
                        fatos.append(
                            f"MIN lado={lado} empresa={empresa.pk} colab_id={cid} "
                            f"rubrica={cod} min={minutos:f} reg={reg}"
                        )
        if sequencias() != seq_antes:
            raise FalhaSonda("Estado das sequencias divergiu")
        transaction.set_rollback(True)

    ids = ",".join(map(str, EMPRESAS))
    fatos.append(f"FIM lados={LADOS} empresas={ids} colabs={n_colabs}")
    return fatos


try:
    for fato in medir():
        print(fato, flush=True)
except (Exception, FalhaRegistrada) as erro:
    # Pilha completa sem texto de excecao do aplicativo nem linhas de fonte.
    print("Traceback (most recent call last):", file=sys.stderr)
    for quadro, linha in walk_tb(erro.__traceback__):
        codigo = quadro.f_code
        print(f'  File "{codigo.co_filename}", line {linha}, in {codigo.co_name}', file=sys.stderr)
    print(type(erro).__name__, file=sys.stderr)
    if isinstance(erro, (FalhaSonda, FalhaRegistrada)):
        print(str(erro), file=sys.stderr)
    raise SystemExit(1) from None
```

```python
# compara_ramo2_s2.py
import re
import sys
from collections import defaultdict
from decimal import Decimal
from pathlib import Path

LADOS = ("A0", "A1", "B", "R")
AUSENTE = (Decimal(0), None)
MIN = re.compile(
    r"MIN lado=(A0|A1|B|R) empresa=([0-9]+) colab_id=([0-9]+) "
    r"rubrica=([0-9]+) min=(-?[0-9]+(?:\.[0-9]+)?) reg=([0-9a-f]{40})"
)
SOBRA = re.compile(
    r"SOBRA empresa=([0-9]+) colab_id=([0-9]+) data=([0-9]{4}-[0-9]{2}-[0-9]{2}) "
    r"hora=([0-9]{2}:[0-9]{2}:[0-9]{2}) tipo=[ES] batida_id=([0-9]+)"
)
FIM = re.compile(r"FIM lados=(A0A1B|R) empresas=([0-9]+(?:,[0-9]+)*) colabs=[0-9]+")


def ler_fatos(caminho):
    try:
        linhas = Path(caminho).read_text(encoding="utf-8").splitlines()
    except FileNotFoundError:
        return []
    return [linha for linha in linhas if linha.startswith(("MIN ", "SOBRA ", "FIM "))]


def carregar(fatos, lados_arquivo, dados, sobras):
    rodape = FIM.fullmatch(fatos[-1])
    if rodape is None or rodape[1] != "".join(lados_arquivo):
        raise ValueError("FIM invalido ou fora do lugar")
    empresas = set(map(int, rodape[2].split(",")))
    for linha in fatos[:-1]:
        medida = MIN.fullmatch(linha)
        if medida is not None:
            lado, empresa, cid, cod

[truncado em 20000 de 24297 caracteres]

## 27/11 09:30 · sessao c5fc6340d46c · E4C-F5-INSTANTE · verificador · origem=Code

## 1. Quem escreve e quem lê

Conferi os 13 arquivos tocados pelos identificadores de conteúdo do diff fornecido. Fiz a inspeção em modo somente leitura e reproduzi em memória o caso do par descrito no item 5. A ancestralidade Git, a execução da suíte e os efeitos em produção ficam [nao verificado]. [nao verificado]

| Campo ou valor | Quem escreve | Quem lê |
|---|---|---|
| `resposta_colab`, `respondida_em` | Resposta humana e automática gravam o par. A cura pela nota e `carimbar` preenchem o instante sobre texto existente. Nascimento assinado pode receber o par. Reabertura, ressuscitação e devolução L-130 limpam pela porta de lote. `app/chamados/services/disputa_emissao.py:1529–1567,1661–1714,1757–1763,1793–1797`, `app/chamados/models.py:1728–1731,3322–3327,3478–3485`, `app/chamados/services/validacao.py:480–488` | Foto do passivo, comparação ato × coluna, C6/C21, histórico, API, prévia de folha, vigia de texto e foto do veto. `app/chamados/models.py:4509–4518,4620–4633`, `app/chamados/juizes.py:1499–1521`, `app/chamados/services/historico.py:96–114`, `app/api/views.py:2587–2601`, `app/folha/previa.py:143`, `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/services/materializacao.py:855–858` |
| `validada_em`, `validada_por[_id]` | `_carimbar_validacao` assina a validação humana e `carimbar` pode assinar no mesmo instante do carimbo. Nova resposta, devoluções e reaberturas limpam a assinatura. `app/chamados/services/validacao.py:164–178,309–318,384–396`, `app/chamados/models.py:3486–3492`, `app/chamados/services/disputa_emissao.py:1542–1544,1661–1714` | A foto do passivo usa os dois campos. As guardas, C6, seleção de plantio e trilha K6 usam o instante. A procedência da batida também lê o autor. `app/chamados/models.py:4514–4518,4664–4684`, `app/chamados/juizes.py:1499–1504`, `app/chamados/services/materializacao.py:855–858,2246–2255,2315–2318`, `app/ponto/services/procedencia_batida.py:72` |
| `materializada_em`, `materializada_por[_id]`, `via_resolucao`, `recusa_do_carimbo` | `_escrever_carimbo` atende ao carimbo normal, de identidade e ao descarimbo individual. O lote zera o trio e o encerramento por fato no espelho pode trocar só a via. A recusa do carimbo é escrita na instância. `app/chamados/models.py:3419–3423,3452–3457,3495–3513,3520–3539,3562–3592,3668–3681` | C6, materialização, fecho S144, histórico, procedência da batida e trilha K6. `app/chamados/juizes.py:1484–1496,1514–1515,1541–1549`, `app/chamados/services/materializacao.py:855–858,1124,2117,2312–2315,2375–2383`, `app/chamados/services/historico.py:104–106`, `app/ponto/services/procedencia_batida.py:72` |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em`, `_atos_do_carimbo`, `_pv_memo` | A porta da lavra escreve os três primeiros. O carimbo monta os atos pendentes e os descarta no save ou no desfazimento. C6 escreve o memo e o desfazimento e `registrar` o invalidam. `app/chamados/veredito_lavrado.py:237–289`, `app/chamados/models.py:3477–3495,3520–3539,4778–4784`, `app/chamados/juizes.py:939–949` | A foto e a restauração leem a lavra. A tarefa verifica sua vigência e filtra os pendentes por `ato_em <= agora` antes de chamar `leitura`. `composta` passa os pendentes ao C6 e à guarda do par. `app/chamados/models.py:3507,3524–3528,4557–4569,4653–4684`, `app/chamados/services/tarefa_do_dia.py:393–443`, `app/chamados/juizes.py:1499–1502` |
| `DecisaoPergunta.pergunta[_id]`, `ato`, `ato_em`, `ato_por[_id]`, `ato_texto`, `ato_motivo`, `resposta_mantem_validacao` | As portas de resposta, validação e compensação montam os valores. `linhas_da_projecao_sem_linha` monta a foto histórica e `registrar` insere a sequência, ajustando o instante de atos compensadores. `app/chamados/models.py:3480–3492,4462–4466,4487–4518,4740–4746,4750–4772`, `app/chamados/services/disputa_emissao.py:1562–1567`, `app/chamados/services/validacao.py:173–178,309–318` | A chave de estoque compara todos esses valores, agora também o motivo. A redução lê tipo, instante, texto e preservação de validação. A foto de ressuscitação lê o autor e o rótulo BUG-PAR-103 lê o prefixo do motivo. `app/chamados/models.py:4457–4466,4530–4541,4557–4591,4815–4850`, `app/chamados/services/materializacao.py:279–292` |
| `entrada_origem`, `entrada_em`, `marco`, classes `a`/`b`, `True`/`INDETERMINADO` | Os defaults gravam `ato` e a hora do banco. A 0058 marca as linhas históricas com `estoque` e o marco. O lote explícito fornece esses valores sob conferência. O marco vem de `django_migrations.applied` da 0057. O comparador produz as classes e o terceiro valor. `app/chamados/models.py:4412–4420,4594–4633,4791–4808`, `app/chamados/migrations/0057_decisao_entrada_do_ato.py:15–32`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:9–20` | A porta do estoque lê por campo. Vigia e DRY agregam por PK e classe, com precedência de `True`. O DRY também lê a coluna e o instante do ato real para imprimir a reprojeção. O VFM publica A/B no censo e no alarme e `ato_x_coluna_indeterminado` no carimbo, cuja comparação no painel usa `censo` e `esperado`. Consumidores externos dos rótulos antigos [nao verificado]. `app/chamados/models.py:4645–4650,4851–4867`, `app/chamados/services/vigia_ato_x_coluna.py:12–35`, `app/chamados/management/commands/estoque_ato_x_coluna.py:23–61`, `app/chamados/management/commands/vigiar_fio_morto.py:213–216,238–242,288–307`, `bin/fio_estado.py:98–126` |
| `pk`, `disputa`, `colaborador_id`, `empresa_id`, `data_evento`, `conversa`, `motivo`, competência, trava e trilha | A identidade vem do nascimento da pergunta. `gravar_dia_do_fato` atribui `data_evento`. A janela usa o corte da Empresa, gravado pelo cadastro. `ExportacaoDominio.registrar` grava a entrega e `invalidar` registra a invalidação. `travar_entrega` grava o marcador em memória. O helper do veto entrega ação, PK, motivo, autor e fotos ao escritor `evento`. `app/chamados/models.py:3090–3105,3322–3327`, `app/chamados/services/fio.py:950–961`, `app/colaboradores/views.py:515–557`, `app/folha/models.py:156–187`, `app/folha/export.py:573–594`, `app/chamados/services/materializacao.py:846–858`, `app/core/observ.py:94–138` | K6 lê a exportação vigente. Suas guardas leem motivo, dia, empresa e janela. Estoque lê também `conversa` e a trava. DRY usa PK, colaborador e competência nas listas. A consulta genérica de auditoria lê a linha e a sonda de fotos lê o JSON. `app/folha/export.py:602–648`, `app/chamados/services/passivo_validado.py:91–100`, `app/chamados/services/materializacao.py:803–842`, `app/chamados/models.py:4836–4873`, `app/chamados/management/commands/estoque_ato_x_coluna.py:31–60`, `app/core/views.py:705–738`, `app/chamados/services/foto_e4b.py:310–323` |

## 2. TXT e dinheiro

**Informativo — a alteração em `export.py` acrescenta um registro da trava na transação.** O `SELECT FOR NO KEY UPDATE` continua vindo primeiro. `travar_entrega` lê o registro para evitar outro marcador e `exigir_entrega_travada` o lê para conferir o lote de estoque. Os outros chamadores da trava são a tela de exportação, o regerador em modo `--aplicar` e a revisão de batidas. `app/folha/export.py:556–607`, `app/chamados/models.py:4767–4772,4836–4839`, `app/folha/views.py:134`, `app/folha/management/commands/regerar_txt_dominio.py:64–67`, `app/ponto/services/par_relampago_na_lista.py:172–186`.

**Nenhum byte, rubrica ou valor do TXT já entregue muda por esse caminho.** A montagem, a serialização e o hash estão fora do trecho alterado. O download histórico serve `ExportacaoDominio.conteudo`. O lote de estoque recusa competência entregue antes do `INSERT` e, quando aceito, só insere decisões e pula a emissão para células. Valores e hashes de produção [nao verificado]. `app/folha/export.py:525–554,744–776`, `app/folha/models.py:174–187`, `app/folha/views.py:574–589`, `app/chamados/models.py:4769–4787,4868–4873`.

## 3. Trilha do veto K6

**Média — duas tentativas idênticas que chegam ao veto e persistem deixam duas linhas.** Cada chamada faz um `create`, sem consulta de existência nem unicidade. A foto antes/depois repete `resposta_colab`, `respondida_em`, `validada_em` e `materializada_em`. A validação tardia desfaz o `atomic` local, relê a pergunta e grava a trilha depois. A escrita é *best effort* e depende do commit externo, quando houver. O contrato estrutural pede uma linha para duas chamadas iguais. `app/chamados/services/materializacao.py:846–858,1999–2004`, `app/chamados/services/validacao.py:237–245,795–797,833–845`, `app/core/observ.py:105–139`, `app/core/models.py:295–316`, `app/docs/ESTRUTURAL.md:22–25`.

**Média — cobertura parcial da trilha.** O RED aceita qualquer evento com foto compatível e descrição esperada, inclusive se houver duplicatas. O teste sem banco injeta um `Mock` da trilha sem verificar a chamada. A devolução BUG-PAR-103 também pode sair por K6 sem gravar essa ação. `app/chamados/tests/test_e4c_f5_c4_red.py:884–895,973–991`, `app/chamados/tests/test_bug_par_sem_banco.py:153–169`, `app/chamados/services/validacao.py:369–378`.

## 4. Asserções que trocaram `False` por `INDETERMINADO`

| Local | Expectativas alteradas |
|---|---|
| `app/chamados/tests/test_e4c_f5_c4_red.py:386,399` | `a/respondida` no encaminhamento à reprojeção e `b/respondida` na resposta compensada. |
| `app/chamados/tests/test_e4c_f5_c4_red.py:607–609` | `a_mista1 a/validada`, `a_mista2 a/respondida`, `estoque b/respondida`. |
| `app/chamados/tests/test_e4c_f5_c4_red.py:620–624` | `a_mista1 a/validada`, `a_mista2 a/respondida`, `pk_misto b/respondida`, `estoque b/respondida`. |
| `app/chamados/tests/test_e4c_f5_c4_red.py:653–658` | `a_mista1 a/validada`, `a_mista2 a/respondida`, `pk_misto b/respondida`, `estoque b/respondida`. |

**As 13 trocas diretas preservam a mordida sobre classe e campo.** Também passam a rejeitar `False` como saída. M35 cobre os três casos históricos de 607–609. Em um teste que estava pulado houve cinco trocas adicionais de `True` para `INDETERMINADO`. Limpeza e retroação sem registro continuam exigidas na listagem, mas o cenário passa a esperar A=0 e B=2 no alarme, antes A=4 e B=3. O teste de carimbo em entregue, também antes pulado, agora admite carimbo e linha `ATO_NOVO`, pois só exige ausência de `ESTOQUE` e de entrada no marco. `app/chamados/tests/test_e4c_f5_c4_red.py:463–475,597–602,648–692`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:674–681`, anexo `diff.patch`.

## 5. Classes de reprovação

| Classe | Achado |
|---|---|
| Segundo juiz que duplica juiz registrado | **Ausente para C6, C21 e K6.** O código continua chamando as autoridades registradas. A divergência nova está na guarda do **par**, abaixo. `app/core/juizes.py:298–320,350`, `app/chamados/models.py:3496–3503`, `app/chamados/juizes.py:1499–1521`, `app/chamados/services/materializacao.py:803–816` |
| Segundo escritor fora da porta | **Ausente nas escritas novas.** A trilha entra por `evento`, as decisões por `registrar` e o registro da trava por `travar_entrega`. Há dois chamadores do emissor da célula no mesmo ato, conforme o item 6. `app/chamados/services/materializacao.py:851–858`, `app/core/observ.py:94–138`, `app/chamados/models.py:3520–3523,4750–4772`, `app/folha/export.py:573–585` |
| Relógio lido fora da borda | **Ausente no filtro novo.** Ele usa `agora` recebido. O caminho preexistente do carimbo ainda chama C6 sem repassar `quando`, e o degrau da janela pode ler `timezone.localdate()` quando chega sem `hoje`. `app/chamados/services/tarefa_do_dia.py:408–425`, `app/chamados/models.py:3460,3496`, `app/chamados/juizes.py:633–643,932–943,1568–1569` |
| Derivação paralela de fonte única | **Alta — presente na guarda do par.** Reproduzi em memória: ato real em T1 e coluna em T2 posterior, sem linha T2. A leitura composta sintetiza T2 como passivo, a guarda retorna `False` para divergência e o comparador de linhas reais acusa `a`. Vale para resposta e validação. A reconferência após o save usa a mesma guarda. **Média — DRY e porta também divergem:** conversa com dia civil pode entrar na lista aberta, e um `b` novo pode ocultar na lista um `b` indeterminado de outro campo do mesmo PK. A porta recusa conversa e confere cada campo do payload. `app/chamados/models.py:4508–4518,4557–4589,4620–4642,4682–4684,4851–4873`, `app/chamados/services/materializacao.py:1132–1136`, `app/chamados/models.py:3097–3105`, `app/chamados/management/commands/estoque_ato_x_coluna.py:33–55` |
| Selo ou RED sem mordida | **Presente.** O RED do carimbo põe o ato real depois da coluna e perde o caso inverso acima. O RED do veto não exige unicidade. O RED dos pendentes injeta a candidata na carga, enquanto os dois chamadores reais recarregam do ORM. Ele prova o filtro com a candidata injetada. Alcance desse ramo pela face pública sem a injeção [nao verificado]. `app/chamados/tests/test_e4c_f5_c4_red.py:98–119,225–248,884–895`, `app/chamados/services/tarefa_do_dia.py:465–478,536–549` |
| Ordem de trava | **Média — conferência incompleta.** Os chamadores mostrados tomam Empresa antes do dono. A porta só confirma que ambas estão registradas. Uma chamada que já detenha ambas em ordem inversa passa nessa conferência. Chamador de produção do `--aplicar` desse estoque [nao verificado]. `app/folha/export.py:581–607`, `app/chamados/models.py:4767–4770,4838–4839`, `app/chamados/tests/test_e4c_f5_c4_red.py:713–717,795–821`, `app/chamados/management/commands/estoque_ato_x_coluna.py:16–25` |
| Pouso antes do pré-requisito | **[nao verificado] para cura1.** A cópia documenta o pouso de PORTA-CONFERE-TRAVA antes do início da obra. A 0058 depende da 0057 e exige seu registro de aplicação. `app/docs/RELATO.md:28,34–35`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:9–14,23–30` |
| Campo com dois significados | **Presente.** `data_evento` guarda dia de turno e dia civil de conversa. `entrada_em` guarda hora efetiva para `ato` e marco convencional para `estoque`. Além disso, o rótulo `ato` também cobre a foto de passivo gravada posteriormente, embora o comparador use essa origem como evidência de novidade. A cura acrescenta `ato_motivo` à conferência de proveniência, além de seu uso na trilha. `app/chamados/models.py:3090–3105,4402–4420,4509–4518,4627–4632,4815–4850`, `app/chamados/tests/test_e4c_f5_c4_red.py:522–538` |
| `except` novo | **Presente como ampliação de cobertura.** O `except Exception` unificado agora cobre também as fotos da projeção e da lavra, restaura os campos e relança. `_pv_memo` e `_atos_do_carimbo` são descartados, portanto a restauração literal desses marcadores não faz parte do comportamento. `app/chamados/models.py:3462–3479,3507,3524–3539` |

## 6. Emissão dupla

**Alta — já existia em `8d160dbd9`, pelo diff fornecido. A cura não cria nem agrava a duplicidade nesse fluxo.** A resposta pública faz `save` com campos que acordam a célula e chama `DecisaoPergunta.registrar` no mesmo ato. O `post_save` e `registrar` chamam `acordar_celulas` separadamente. O conjunto `vistos` vale por chamada e o emissor faz um `INSERT` a cada chamada. Com uma célula resolvida, são duas linhas pelo mesmo ato. O RED inalterado exige uma, então a falha é esperada já no controle, antes do mutante M20. A execução desse RED fica [nao verificado]. `app/chamados/services/disputa_emissao.py:1529–1532,1562–1567`, `app/chamados/signals.py:253–259`, `app/chamados/models.py:3600–3629,4785–4787`, `app/ponto/portas/celula.py:981–991`, `app/chamados/tests/test_e4c_f5_c4_red.py:541–553`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:37–59,291–298,488–490`, anexo `diff.patch`.

## 27/11 09:30 · sessao b189a5326482 · - · - · origem=Ronald (tmux)

**Alta — a guarda de instante pode aceitar uma divergência que o vigia encontra.** A nova guarda compara a coluna com `composta`, cuja sequência sintetiza a própria coluna e pode colocá-la depois do ato real. Reproduzi os métodos em memória com ato real às 10h e coluna às 11h. A guarda respondeu `False` para divergência e o comparador respondeu `a/ respondida / INDETERMINADO`. A absorção repete a guarda após reler o banco. O RED direto cobre ato posterior à coluna. `app/chamados/models.py:3502,4509,4566,4580,4620,4682`; `app/chamados/services/materializacao.py:1132`; `app/chamados/tests/test_e4c_f5_c4_red.py:99`.

**Alta — emissão dupla herdada e RED vermelho pela leitura.** A resposta humana salva os campos que acionam o `post_save` e chama `registrar`. Os dois emitem para a mesma célula. A deduplicação de `acordar_celulas` vale dentro de cada chamada e a porta faz um `create` por chamada. O RED exige 1, mas a leitura prevê 2 já no controle inicial do M20. Execução Django [nao verificado]. `app/chamados/services/disputa_emissao.py:1562,1565`; `app/chamados/signals.py:253`; `app/chamados/models.py:3600,3618,4785`; `app/ponto/portas/celula.py:981`; `app/chamados/tests/test_e4c_f5_c4_red.py:545`; `app/chamados/tests/e4c_f5_c4red2_mutantes.py:37,488`.

**Média — DRY e porta de estoque discordam.** A lista anunciada como universo do estoque pode incluir conversa usando sua data civil de abertura, que a porta rejeita. Também omite um campo `b` indeterminado se o outro campo da mesma pergunta for `b` novo, embora a porta julgue o campo submetido. `app/chamados/management/commands/estoque_ato_x_coluna.py:33,45,55`; `app/chamados/models.py:3097,4853,4865`; `app/chamados/tests/test_e4c_f5_c4_red.py:588,623`.

**13 expectativas False → INDETERMINADO.** Em `app/chamados/tests/test_e4c_f5_c4_red.py` são `:386` A/resposta, `:399` B/resposta, `:607–609` A/validação, A/resposta e B/resposta, `:620–624` A/validação, A/resposta e duas B/resposta, `:653–654` A/validação e A/resposta, `:657–658` duas B/resposta. Preservam classe e campo e reforçam a distinção de `False`. Separadamente, as cinco expectativas True→INDETERMINADO de `:651–655` rebaixam alarmes A de 4 para 0 e B de 3 para 2 no teste antes pulado. Agora ele exige a listagem em `:660–662`. O teste entregue antes pulado também deixou de exigir rollback integral e aceita atos `ATO_NOVO` em `:463–475`. M35 cobre os três casos históricos de `:607–609`. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:674`.

**Demais classes.** Juiz registrado duplicado e segundo escritor operacional novo ausentes nos fluxos vistos. A nova trilha chama a porta canônica `evento`, e o comparador novo é uma derivação paralela da paridade, como demonstrado acima. O relógio novo da tarefa é recebido pela borda. A lavra mantém a leitura própria de `timezone.now()`. A nova trava verifica presença e permite ao futuro chamador passar tendo adquirido Colaborador antes de Empresa. As fixtures adquirem Empresa primeiro. `[nao verificado]` para ordem de deploy da cura. A dependência da 0057 e a anotação do pouso anterior da PORTA constam da cópia. `entrada_em` guarda ora hora da inserção, ora marco convencional. `data_evento` da conversa guarda data civil. `ato_em` de compensador pode ser deslocado para a última data da sequência. O `except Exception` unificado amplia escopo, restaura e relança. `app/core/juizes.py:165,185`; `app/chamados/services/materializacao.py:846`; `app/core/observ.py:94`; `app/chamados/services/tarefa_do_dia.py:408`; `app/chamados/veredito_lavrado.py:271`; `app/folha/export.py:581,602`; `app/chamados/tests/test_e4c_f5_c4_red.py:713`; `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:25`; `app/docs/RELATO.md:28,34`; `app/chamados/models.py:3097,3524,4402,4740`.

## 27/11 09:29 · sessao 38de0429b039 · - · - · origem=Ronald (tmux)

- **Alta. A guarda de instante pode aceitar coluna mais recente sem ato correspondente.** Em `composta`, a coluna T2 sem linha gera passivo sintético e vence a linha real T1 quando T2 > T1. O novo comparador vê T2 dos dois lados, inclusive após reler do banco. O vigia vê T1 na sequência real e classifica como A. O RED cobre a direção inversa, com instante do ato maior que o da coluna. `app/chamados/models.py:4508–4518,4563–4585,4622–4642,4682–4684` · `app/chamados/services/materializacao.py:1132–1136` · `app/chamados/tests/test_e4c_f5_c4_red.py:98–115`.
- **Média. DRY e porta discordam sobre elegibilidade.** Conversa tem data civil preenchida. O DRY usa essa data para K6 e pode mostrar conversa em `b_indeterminado_aberta`, mas a porta recusa conversa. Um PK misto B novo/B indeterminado também some da lista, embora o campo indeterminado seja julgado individualmente pela porta. `app/chamados/models.py:3097–3105,4853–4869` · `app/chamados/services/materializacao.py:811–816` · `app/chamados/management/commands/estoque_ato_x_coluna.py:31–56` · `app/chamados/tests/test_e4c_f5_c4_red.py:620–630`.
- **Média. Trilha K6 sem dedupe.** Cada veto chama `evento`, que insere em `LogAuditoria`. Duas tentativas efetivamente vetadas e confirmadas produzem duas linhas. Os REDs verificam existência e fotos compatíveis, sem fixar cardinalidade. O teste sem banco usa mock sem exigir chamada. `app/chamados/services/materializacao.py:846–858,2000–2004` · `app/chamados/services/validacao.py:237–245,794–797,834–845` · `app/core/observ.py:105,133–138` · `app/chamados/tests/test_e4c_f5_c4_red.py:884–895,908–923,961–991` · `app/chamados/tests/test_bug_par_sem_banco.py:153–169`.
- **Média. Ramo temporal sem entrada normal demonstrada.** Ambos os chamadores de `_fatos_das_perguntas` carregam novas instâncias do ORM, sem `_atos_do_carimbo`. O RED faz a substituição pela instância em trânsito via patch. O RED testa a regra local, mas o caminho público sem patch não chega a ela. `app/chamados/services/tarefa_do_dia.py:419–433,465–478,536–549` · `app/chamados/tests/test_e4c_f5_c4_red.py:225–248`.
- **Média. Ordem contratual apenas conferida por presença.** O sentinela da Empresa registra só seu PK. A conferência aceita as duas travas presentes mesmo se tomadas em ordem invertida. O concorrente verifica a ordem do chamador correto e a recusa sem Empresa. `app/folha/export.py:563–606` · `app/chamados/models.py:4767–4770,4838–4839` · `app/chamados/tests/test_e4c_f5_c4_red.py:771–821`.
- **Baixa. `ato_motivo` ganhou uso na igualdade da foto de estoque.** O gerador escreve o motivo da foto. Além de texto de histórico, o campo já identificava devolução BUG-PAR-103 pelo prefixo. `entrada_em` já era instante real para ato e marco convencional para estoque. `app/chamados/models.py:4404–4420,4511–4518,4818–4819,4840–4850` · `app/chamados/services/materializacao.py:279–292` · `app/chamados/services/validacao.py:392–396` · `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:20`.

**Escritores e leitores fora do diff**

- Resposta/texto e instante saem da porta humana, automática, cura por nota, nascimento assinado e `carimbar`. Validada e autor saem de `_carimbar_validacao` e `carimbar`. A resposta humana, reabertura, ressuscitação e devolução apagam os respectivos pares. C6, API, fila de validação e placar leem a projeção diretamente ou via composta. `app/chamados/services/disputa_emissao.py:1529–1567,1661,1714,1757–1763,1793–1797` · `app/chamados/models.py:1728–1731,3322–3327,3478–3492` · `app/chamados/services/validacao.py:164–178,309–318,384–396,483–488,768–776` · `app/chamados/juizes.py:1499–1521` · `app/api/views.py:2587,2601,2646` · `app/chamados/services/verbos.py:263–271`.
- Carimbo materializado, autor e via passam pelas portas do modelo. As escritas só da via incluem encerramento por fato e relavra. A lâmpada e seus dois instantes são escritos por `lavrar` ou gravação em lote, lidos pela tarefa e fotografados/restaurados na borda do carimbo. `app/chamados/models.py:3419–3423,3507,3520–3526,3562–3570,3668–3682` · `app/chamados/services/relavra_via.py:123–128` · `app/chamados/veredito_lavrado.py:240–250,254–289` · `app/chamados/services/tarefa_do_dia.py:439–443` · `app/chamados/reconciliador.py:1287–1298`.
- A sequência recebe `pergunta`, `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo`, `resposta_mantem_validacao` pela porta `registrar`. Consomem-na composição, comparador, SQL de validação, tarefa, histórico e modal. `ato_por_id` integra a foto da ressuscitação. `app/chamados/models.py:3723–3742,3978–4000,4460–4466,4530–4568,4637–4642,4750–4787` · `app/chamados/services/tarefa_do_dia.py:158–168,387–433` · `app/templates/chamados/partials/modal_fio.html:210–212`.
- Marco é `django_migrations.applied` da 0057. A origem é default `ato`, estoque é payload conferido ou backfill 0058, e `entrada_em` é default de banco ou marco conferido. Comparador lê origem da linha efetiva. O terceiro valor é lido por conferência de estoque, vigia e DRY. VFM publica A/B no censo e indeterminados no topo do carimbo. `fio_estado` compara `censo` e `esperado`, enquanto o cadastro do comando ainda descreve “NOVO x ESTOQUE”. Leitor externo das chaves renomeadas [nao verificado]. `app/chamados/models.py:4419–4420,4594–4600,4629,4791–4806,4853–4864` · `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:20` · `app/chamados/services/vigia_ato_x_coluna.py:26–35` · `app/chamados/management/commands/estoque_ato_x_coluna.py:33–56` · `app/chamados/management/commands/vigiar_fio_morto.py:213–216,238–242,288–302` · `bin/fio_estado.py:104–105,121–126` · `app/config/crons.py:802–805`.
- K6 lê dia, motivo e vínculo disputa/dono/empresa, deriva mês/ano pela janela e delega ao juiz de exportação vigente. A data é escrita por `gravar_dia_do_fato`. O registro TXT é escrito por `registrar` e seu estado de invalidação por `invalidar`. A trilha é escrita por `evento`, com `criado_em` automático, e lida na tela genérica de auditoria. `app/chamados/services/materializacao.py:803–840` · `app/chamados/services/fio.py:950–961` · `app/chamados/services/passivo_validado.py:91–100` · `app/folha/export.py:634–648` · `app/folha/models.py:156–162,174–187` · `app/core/observ.py:133–138` · `app/core/models.py:295–307` · `app/core/views.py:705–733`.

## 27/11 09:28 · sessao 254352022df7 · - · - · origem=Ronald (tmux)

- **Média — trilha K6 duplica em nova tentativa.** Cada chamada aceita pelo banco faz um `LogAuditoria.objects.create`. Duas tentativas idênticas que chegam ao veto e persistem deixam duas linhas. O contrato pede uma, e o teste exige apenas que exista trilha completa. O veto tardio desfaz primeiro o carimbo e a trilha interna, relê a pergunta e registra depois do `atomic` local. [materializacao.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/chamados/services/materializacao.py:846) · [observ.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/core/observ.py:133) · [ESTRUTURAL.md](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/docs/ESTRUTURAL.md:22) · [test_e4c_f5_c4_red.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/chamados/tests/test_e4c_f5_c4_red.py:887) · [validacao.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/chamados/services/validacao.py:833)

- **Informativo — o hunk de exportação registra a trava existente.** O marcador em memória é escrito depois do `SELECT FOR NO KEY UPDATE` por `travar_entrega` e lido para evitar novo marcador e conferir a precondição do lote de estoque. Os chamadores antigos são a tela de exportação, o `regerar_txt_dominio --aplicar` e a revisão de batidas. O hunk preserva montagem, formatação e hash. O arquivo entregue é baixado de `ExportacaoDominio.conteudo`, e o estoque recusa competências entregues antes de inserir. Nenhum valor muda por esse caminho. Valores em produção [nao verificado]. [export.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/folha/export.py:573) · [views.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/folha/views.py:134) · [regerar_txt_dominio.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/folha/management/commands/regerar_txt_dominio.py:64) · [par_relampago_na_lista.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/ponto/services/par_relampago_na_lista.py:172) · [views.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/folha/views.py:574) · [models.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/chamados/models.py:4868)

- **Baixa — a checagem de trava confere presença.** O registro de `Empresa` é um conjunto, sem ordem de aquisição. As fixtures tomam `Empresa` antes do `Colaborador`. A porta passaria pela mesma conferência se um chamador futuro invertesse a ordem e já detivesse ambas. [export.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/folha/export.py:588) · [models.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/chamados/models.py:4767) · [test_e4c_f5_c4_red.py](/home/ronald/codex_ws/d5b417695b2ead837b70847d1e2b515c7decbd45/app/chamados/tests/test_e4c_f5_c4_red.py:713)

## 27/11 09:25 · sessao 9c24fac33fc6 · PAR-PELO-TURNO · construtor · origem=Code

A c1v7 registra `batidas_apuraveis_na_janela` como autoridade e mantém a dívida em **1 chamada**. O censo compara nomes exatos nos imports e nas chamadas. A checagem com e sem alias confirmou que os dois nomes não casam por prefixo em nenhum sentido. (`relatorios/tests/test_pdf_nao_calcula.py:38,49,70,77`, `relatorios/pdf_espelho.py:281`)

```diff
diff --git a/app/relatorios/tests/test_pdf_nao_calcula.py b/app/relatorios/tests/test_pdf_nao_calcula.py
--- a/app/relatorios/tests/test_pdf_nao_calcula.py
+++ b/app/relatorios/tests/test_pdf_nao_calcula.py
@@ -37,0 +38 @@ AUTORIDADES = frozenset({
+    'batidas_apuraveis_na_janela',
@@ -48 +49 @@ DIVIDA = {
-    ('pdf_espelho.py', 'batidas_apuraveis'): 1,
+    ('pdf_espelho.py', 'batidas_apuraveis_na_janela'): 1,
```

**Contagem de `except` no diff** `+0 -0`. (`relatorios/tests/test_pdf_nao_calcula.py:38,49`)

**Prova local**. A execução direta parou com `ModuleNotFoundError: No module named 'django.test'`. Execução com Django [nao verificado]. Com `unittest.TestCase` em memória, o RED original acusou a entrada antiga como podre e os cinco testes passaram após a cura. (`relatorios/tests/test_pdf_nao_calcula.py:28,94,114`)

**Censo AST de `pdf_espelho.py`**, com as linhas das chamadas. (`relatorios/tests/test_pdf_nao_calcula.py:63`, `relatorios/pdf_espelho.py:213,281,347,358,427,444,450`)

```text
Antes   celulas_do_periodo [347], folgas_do_periodo [358], turnos_do_colab [213, 444],
        calcular_mes [427], minutos_previstos_do_dia [450]
Depois  celulas_do_periodo [347], folgas_do_periodo [358], turnos_do_colab [213, 444],
        batidas_apuraveis_na_janela [281], calcular_mes [427], minutos_previstos_do_dia [450]
```

| Comando de prova | Resultado |
|---|---|
| `rodar_selo antes` | RED, entrada antiga podre. `relatorios/tests/test_pdf_nao_calcula.py:114` |
| `rodar_selo depois` | Cinco testes passaram com `unittest.TestCase`. `relatorios/tests/test_pdf_nao_calcula.py:94` |
| `rodar_selo sem_autoridade` | RED, a chamada some do censo e a entrada do embrulho fica podre. `relatorios/tests/test_pdf_nao_calcula.py:77,114` |
| `rodar_selo chave_antiga` | RED, a chave antiga fica podre e o embrulho aparece como cálculo novo. `relatorios/tests/test_pdf_nao_calcula.py:101,114` |
| `rodar_selo chamada_duplicada` | RED, duas chamadas para uma declarada. `relatorios/tests/test_pdf_nao_calcula.py:106` |

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v relatorios.tests.test_pdf_nao_calcula

rodar_selo() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$1" <<'PY'
import ast
from pathlib import Path
import sys
import unittest

selo = Path("relatorios/tests/test_pdf_nao_calcula.py")
pdf = Path("relatorios/pdf_espelho.py").read_text(encoding="utf-8")
arvore = ast.parse(selo.read_text(encoding="utf-8"))
arvore.body = [
    no for no in arvore.body
    if not (isinstance(no, ast.ImportFrom) and no.module == "django.test")
]
ns = {"__file__": str(selo), "SimpleTestCase": unittest.TestCase}
exec(compile(arvore, str(selo), "exec"), ns)

modo = sys.argv[1]
antiga = "batidas_apuraveis"
nova = "batidas_apuraveis_na_janela"
chave = ("pdf_espelho.py", nova)
if modo in ("antes", "sem_autoridade"):
    ns["AUTORIDADES"] -= {nova}
if modo in ("antes", "chave_antiga"):
    ns["DIVIDA"][("pdf_espelho.py", antiga)] = ns["DIVIDA"].pop(chave)
if modo == "chamada_duplicada":
    linha = next(l for l in pdf.splitlines(keepends=True) if "list(" + nova + "(" in l)
    censo = ns["censo"]
    ns["censo"] = lambda fonte: censo(
        fonte.replace(linha, linha * 2, 1) if fonte == pdf else fonte
    )

print("Censo PDF:", ns["censo"](pdf), flush=True)
suite = unittest.defaultTestLoader.loadTestsFromTestCase(ns["PdfNaoCalculaTest"]) [nao verificado]
raise SystemExit(not unittest.TextTestRunner(verbosity=2).run(suite).wasSuccessful()) [nao verificado]
PY
}
```

## 27/11 09:22 · sessao e0dac6bc0611 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

Registrei as portas de `celula_absolveu`, `fantasma` e `competencia_trancada` para os casos **06, 07, 27 e 37**. O diff de código tem **30 inserções e zero `except` novos**. [core/portas.py](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/core/portas.py:396) · [Diff stat](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/construtor.r:37)

**25, 26 e 28 ficaram parados** pela regra do censo. Os leitores ou escritores dos campos envolvidos estão em `chamados/models.py` e `chamados/services/disputa_emissao.py`, arquivos proibidos nesta volta. [Censo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/construtor.r:20)

Na verificação isolada do selo, os quatro recortes falharam na base, passaram com as declarações e voltaram a falhar sob os mutantes. A execução completa em Django/PostgreSQL fica com o Code **[nao verificado]**. O recibo contém o diagnóstico dos sete REDs, a linha `--only` e o comando de cada mutante. [construtor.r](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/construtor.r:5) · [Comandos](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/construtor.r:48)

## 27/11 09:21 · sessao 217e2dcf82a3 · PAR-PELO-TURNO · construtor · origem=Code

**PAR-PELO-TURNO c1v6 aplicada na cópia indicada.** O recorte em memória passou **95 testes**, incluindo composição de fatias e vizinhos. O diff passou em `git diff --check` e acrescentou **zero `except`**. [Comandos e vereditos](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:29) · [Auditoria e diff](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:146) [nao verificado]

- **Na base**, 1a, 2, 3a, 3b, 3d, 4a e 4c falharam pelos motivos esperados. Em 4c, a marca de `fim+2` derrubava `sem_movimento` e mudava o saldo de `0,00 h` para `−193,60 h`. Com a cura, os dois braços deram `0,00 h` e `sem_movimento=True`. [Evidência da base e da cura](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:6)
- **1b, 5 e 6** passaram na base. Os mutantes os fizeram falhar ao trazer as marcas de col297 para a janela errada, esvaziar a ata e retirar a C9. No total, **15 mutantes falharam por asserção** e o alias que preserva a ata passou. A evidência traz o comando de cada um. [Comandos dos mutantes](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:43) [nao verificado]

**Pendências para a certificação**

- A suíte Django da obra, composição e vizinhos parou na importação de `django.core`. Os testes completos de PDF e fechamento ficam **[nao verificado]**. [Comando e erro](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:123) [nao verificado]
- Em **3a**, `test_MORDE_a_divida_so_ENCOLHE_e_nao_apodrece` falha porque o inventário ainda exige a chamada direta a `batidas_apuraveis`. O PDF passou a usar `batidas_apuraveis_na_janela`. A asserção existente segue como estava. [Selo do PDF](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/tests/test_pdf_nao_calcula.py:107) · [Carga do PDF](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/pdf_espelho.py:280) [nao verificado]
- **3c e 4b** ficaram fora da volta conforme o recorte. Em **6**, o ensaio sintético deu um par de **782 min**, teto de **732 min** e uma C9. O laudo nominal do col155 em 08/11 e o impacto de frota por rubrica ficam **[nao verificado]**. [Itens em aberto](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:137)

## 27/11 09:12 · sessao bd73245393ec · CODEX-CONSTROI-MOTOR · construtor · origem=Code

Implementei a guarda do CCM no vigia. O juiz é chamado antes dos dois comandos Docker. Qualquer `rc != 0` grava stdout, stderr e a linha de recusa em `saida`, e retorna `None`. O diff acrescenta **0 `except`**. [core/esteira_vigia.py:748](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:748)

- O isolamento retorna `None` antes de classificar ou gravar o cache. [core/esteira_vigia.py:740](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:740)
- O auto-revert mantém o revert aplicado sem prova, registra a recusa na Pauta e no RELATO e retorna `revert aplicado, reteste recusado (CCM)`. [core/esteira_vigia.py:1322](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:1322) [nao verificado]
- O congelado e a autocura já exigiam verde para abrir quarentena e criar fatia. Acrescentei ao congelado um retorno específico para a recusa, antes do alarme de vermelho. [core/esteira_vigia.py:1377](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:1377) [core/esteira_vigia.py:1467](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:1467) [nao verificado]

**Evidência da volta**

Os seis casos foram escritos com o vigia ainda no HEAD `59600163ccc9cffb8f3ae148beede7cd7866ae86`. A leitura desse HEAD mostrou que `_rodar_testes` começava pelo Docker, que o isolamento classificava `None` como `'arvore'` e que o auto-revert desfazia o revert com `None`. A sintaxe dos dois arquivos e dos sete mutantes foi conferida. O RED, o GREEN e os mutantes seguem **[nao verificado]** em execução. A pista 2 documentada exige SSH e uma cópia commitada, operações vedadas nesta volta. [HEAD core/esteira_vigia.py:747, :741, :1315] [core/tests/test_vigia_ccm.py:38](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/tests/test_vigia_ccm.py:38) [docs/BACKLOG.md:2375](/home/ronald/wt-codex-codex-constroi-motor-090747/app/docs/BACKLOG.md:2375)

Os vizinhos retornaram `rc 1` após a cura, com `FAILED (errors=2)` na importação. Os erros foram `No module named 'django.contrib'` e `No module named 'django.test'`. Nenhum caso dos dois módulos chegou a executar. [core/tests/test_esteira_sem_humano.py:10](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/tests/test_esteira_sem_humano.py:10) [core/tests/test_esteira_status_verdade.py:13](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/tests/test_esteira_status_verdade.py:13)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v \
  core.tests.test_esteira_sem_humano core.tests.test_esteira_status_verdade

git --no-optional-locks diff -- core/esteira_vigia.py
git --no-optional-locks diff --no-index /dev/null core/tests/test_vigia_ccm.py
```

**Comandos do RED, GREEN e mutantes na pista 2**

Com o checkout Git e o Django disponíveis, a função abaixo roda o teste novo com a versão escolhida do vigia em memória. Saídas de execução **[nao verificado]**. [core/tests/test_vigia_ccm.py:18](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/tests/test_vigia_ccm.py:18) [nao verificado]

```bash
ccm() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
import ast
import subprocess
import sys
import unittest
from pathlib import Path

import django
from django.conf import settings

settings.configure(SECRET_KEY="ccm-teste-isolado") [nao verificado]
django.setup()
from core import esteira_vigia as vigia

modo, *args = sys.argv[1:]
p = Path(vigia.__file__)
fonte = p.read_text(encoding="utf-8")
if modo == "head":
    fonte = subprocess.check_output([
        "git", "show",
        "59600163ccc9cffb8f3ae148beede7cd7866ae86:app/core/esteira_vigia.py",
    ], text=True)
elif modo == "mutante":
    nome, antes, depois = args
    no = next(n for n in ast.parse(fonte).body if isinstance(n, ast.FunctionDef) and n.name == nome)
    linhas = fonte.splitlines(keepends=True)
    trecho = "".join(linhas[no.lineno - 1:no.end_lineno])
    assert trecho.count(antes) == 1, (nome, antes)
    linhas[no.lineno - 1:no.end_lineno] = [trecho.replace(antes, depois, 1)]
    fonte = "".join(linhas)
else:
    assert modo == "cura", modo

exec(compile(fonte, str(p), "exec"), vars(vigia))
unittest.main(module="core.tests.test_vigia_ccm", argv=["ccm", "-v"])
PY
}

ccm head
ccm cura
ccm mutante _rodar_testes 'juiz.returncode != 0' 'juiz.returncode == 1'
ccm mutante _rodar_testes 'juiz.stdout + juiz.stderr' 'juiz.stdout'
ccm mutante _rodar_testes 'CCM-REDUZ-2711-0845' 'CCM'
ccm mutante _rodar_testes $'return None\n    pw =' $'return False\n    pw ='
ccm mutante _isolar 'if ok is None:' 'if ok is False:'
ccm mutante auto_revert 'if verde is None:' 'if verde is False:' [nao verificado]
ccm mutante curar_por_relogio 'if verde is None:' 'if verde is False:' [nao verificado]
```

## 27/11 09:05 · sessao e6e68478a2db · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**C2u parcial.** Apliquei os itens 2, 5 e 6. Os itens 1, 3 e 4 dependem de autorização explícita para editar `motor_calculo_v2.py`, que a ordem também proíbe sob pena de descarte. [Intermitente:3841](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3841) · [Comercial:3671](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3671) · [Partido:3103](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3103) [nao verificado]

| Item | RED antes da cura e resultado observado |
|---|---|
| 1. Intermitente | RED em `(750, 570, 0)` para trabalhado, noturno e fora. O teste exige `(720, 540, 30)` e segue RED. [Teste:31](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:31) | [nao verificado]
| 2. Pausa | A releitura dava `(300, 20)` para trabalhado e pausa, contra `(320, 0)` do motor e da alimentação. Com o gatilho compartilhado, os três dão `(320, 0)`. Duas fixtures existentes passaram a informar o desconto do motor, com as asserções originais preservadas. [RED:38](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:38) · [Cura:755](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:755) · [Fixtures:203](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:203) |
| 3. Comercial | RED em **HE 0 e saldo −33,83 h**, com 610 trabalhados e previsto de 660. Em memória, condicionar o previsto a `saida_da_base_da_he` deu **HE 130 e saldo −36,00 h** sem cauda. O controle com saída às 07h20 continuou com HE zero. [RED:60](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:60) · [Controle:145](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:145) |
| 4. Partido | O teste de igualdade literal deu RED para `MotorTurnoPartido`. `MotorTurnoPartidoNoturno` já coincide com `59600163c`. [Teste:72](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:72) |
| 5. Export | `AttributeError` no resultado composto vazio e `KeyError` no período sem chave. Ambos passaram com a leitura por `dia_pago._dia_de`. [REDs:15](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_export.py:15) · [Cura:346](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/porta_export.py:346) |
| 6. Catraca | O censo acusou a lista antiga. Registrei `[3586, 3716, 3734, 3759, 3763, 3764]`. A catraca passou **35/35** e a restauração da lista antiga em memória voltou a falhar. [Registro:309](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:309) · [Asserção:564](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:564) | [nao verificado]

**Conflitos medidos nos itens 3 e 4**

- **Pontualidade sem cauda.** Com os marcos do `main` em memória, o caso `test_entrada_depois_da_meia_noite_com_d1_de_folga_tem_atraso_abatido_pela_parcial` deu atraso **0** e exige **5**. `test_sem_parcial_o_atraso_e_o_bruto` deu **0** e exige **12**. Nos dois, a saída é às 07h50, no marco. [Com parcial:119](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:119) · [Sem parcial:129](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:129)
- **Partido.** Na cópia literal em memória, `test_partido_com_intervalo_entre_dois_periodos` e `test_partido_preserva_os_490_minutos_do_main` deram `ValueError`. O Partido do `main` desempacota quatro valores e a janela atual retorna cinco. O teste do construtor da tripla passou nessa simulação. Os oito casos de `EscritorUnicoDaBaseDaHeTest` passaram na cópia de trabalho. [Janela:1939](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1939) · [Intervalo:147](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:147) · [490 minutos e tripla:51](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:51) · [Escritor:661](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:661) [nao verificado]

**Execução.** Os 11 casos focados de pausa e export passaram em `unittest` em memória, com as fronteiras externas substituídas. A chamada conjunta de `ponto.tests.test_ramo2b_*`, `ponto.tests.test_o280_bateria_da_lei`, `core.tests.test_medida_turno_do_par` e `folha.tests` parou no import de `django.core`. O veredito da suíte Django e a certificação do Code e do Opus final estão pendentes. O arquivo `verif_c2t.md` ficou [nao verificado]. [Entrada da suíte:11](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/manage.py:11) [nao verificado]

Os quatro mutantes em memória produziram os REDs esperados. Os dois primeiros saíram com `rc 1`, e os do export com `rc 2`, respectivamente `AttributeError` e `KeyError`. Comandos para reproduzir com Django disponível. [Mutantes:14](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2u_mutantes.py:14)

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
python3 -B -m ponto.tests.ramo2b_c2u_mutantes releitura_ignora_desconto
python3 -B -m ponto.tests.ramo2b_c2u_mutantes alimentacao_ignora_desconto
python3 -B -m ponto.tests.ramo2b_c2u_mutantes export_exige_mapa
python3 -B -m ponto.tests.ramo2b_c2u_mutantes export_exige_chave
```

Para reproduzir o mutante da catraca sem Django, execute o trecho `unittest` documentado no teste com a lista do Comercial restaurada em memória para `[3586, 3717, 3735, 3760, 3764, 3765]`. [Comando da catraca:30](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:30) · [Entrada mutada:309](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:309) [nao verificado]

**Diff da cópia.** `git diff --check` passou. A contagem de `except` adicionados foi zero, incluindo os arquivos novos. [Pausa:35](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/alimentacao.py:35) · [Export:589](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/porta_export.py:589) [nao verificado]

```bash
git diff HEAD -- .
for arquivo in ponto/tests/*c2u*.py; do
  git diff --no-index -- /dev/null "$arquivo"
done
```

_8608 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
