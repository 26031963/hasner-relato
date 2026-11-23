# CODEX-ESTADO

> Ultima resposta processada: **23/11/2026 15:13** · publicado em **23/11/2026 15:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 23/11 15:13 · sessao 5917bcb4ab53 · PAR-PELO-TURNO · sonda · origem=Code

Na sonda, os lotes 1 (`turnos_do_colab`) e 2 (`turnos_via_autoridade`) deram o mesmo destino às batidas de referência dos quatro casos. A comparação abaixo usa os `batida_id` da saída. `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`

### col129

| Batida na saída | Ramo do pareador | Teto aplicado | Resultado nos dois lotes | [nao verificado]
|---|---|---|---|
| E **155814**, 15/11 17:30 | Teto da jornada. O vão de **779,991 min** deixou a E aberta. A tentativa de fechar com a S seguinte também excedeu o teto. `app/ponto/turnos.py:1447`, `app/ponto/turnos.py:1453`, `app/ponto/turnos.py:1093` | **732 min** = 492 min entre `hi` e `hf` do DNA de **15/11** + 240 min, por `_teto_da_jornada`. `cont_max_s` entrou com 840 min. As distâncias E→`hi` e S→`hf` foram **239,749 / 48,241 min**, e a L-084 exige ambas acima de 180 min para trocar o teto. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1430`–`1447` | **E aberta** em 15/11. `17:30` aparece em `dia['orfas']` nas duas grades. `app/ponto/turnos.py:1453`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` |
| S **156153**, 16/11 06:30 | A mesma comparação emitiu a **S sem entrada** e gravou `_separada_pelo_teto`. `app/ponto/turnos.py:1451`–`1454` | **732 min**, calculados com o DNA de **15/11**, data da E candidata. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1430`–`1434` | **S órfã** em 16/11 no pareador. `06:30` ficou fora de `dia['orfas']` nas duas grades consultadas. `app/ponto/turnos.py:1454`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` |
| Escala/célula | Vínculo **853**, template **198** (6×1). DNA recebido pelos dois lotes de 14 a 17/11, na ordem `(hi,hf,hii,hfi)` = **(21:30, 05:42, ∅, ∅)**. Células na grade lavrada **140135** em 15/11 e **140136** em 16/11, ambas de trabalho. `app/escala/utils.py:84`, `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424` | | |

### col155

| Batida na saída | Ramo do pareador | Teto aplicado | Resultado nos dois lotes | [nao verificado]
|---|---|---|---|
| E **155813**, 15/11 17:29 | Teto da jornada. O vão de **782,822 min** deixou a E aberta. A tentativa de fechar com a S seguinte também excedeu o teto. `app/ponto/turnos.py:1447`, `app/ponto/turnos.py:1453`, `app/ponto/turnos.py:1093` | **732 min** = 492 min entre `hi` e `hf` do DNA de **15/11** + 240 min, por `_teto_da_jornada`. `cont_max_s` entrou com 840 min. Distâncias E→`hi` e S→`hf` de **240,640 / 50,183 min** mantiveram o teto do dia pela L-084. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1430`–`1447` | **E aberta** em 15/11. `17:29` aparece em `dia['orfas']` nas duas grades. `app/ponto/turnos.py:1453`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` |
| S **156154**, 16/11 06:32 | A mesma comparação emitiu a **S sem entrada** e gravou `_separada_pelo_teto`. `app/ponto/turnos.py:1451`–`1454` | **732 min**, calculados com o DNA de **15/11**, data da E candidata. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1430`–`1434` | **S órfã** em 16/11 no pareador. `06:32` ficou fora de `dia['orfas']` nas duas grades consultadas. `app/ponto/turnos.py:1454`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` |
| Escala/célula | Vínculo **1437**, template **198** (6×1). DNA recebido pelos dois lotes de 14 a 17/11 **(21:30, 05:42, ∅, ∅)**. Células na grade lavrada **131754** em 15/11 e **131755** em 16/11, ambas de trabalho. `app/escala/utils.py:84`, `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424` | | |

### col214

| Batida na saída | Ramo do pareador | Teto aplicado | Resultado nos dois lotes | [nao verificado]
|---|---|---|---|
| E **155565**, 15/11 07:00 | **Fecho normal** com a S **155668** às 12:06. `07:00` aparece em `dia['orfas']` nas duas grades. **Não responde** qual passo a colocou na grade viva, porque a sonda traz a lista final. A grade da célula lê a lista da ata. `app/ponto/turnos.py:1491`–`1493`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:420` | **1.560 min**, envelope de 26 h do fecho. A L-084 substituiu os **740 min** de `_teto_da_jornada` (500 do DNA de 15/11 + 240), pois as distâncias **339,654 / 533,327 min** superaram 180 min. Vão de **306,327 min**. `cont_max_s` entrou com 840 min. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1430`–`1447` | **Mesmo par fechado 155565→155668** em 15/11. `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503` |
| Escala/célula | Vínculo **182**, template **220** (6×1). DNA recebido pelos dois lotes de 14 a 17/11 **(12:40, 21:00, ∅, ∅)**. Célula de trabalho **122012** em 15/11 na grade lavrada. `app/escala/utils.py:84`, `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424` | | |

### col107

| Batida na saída | Ramo do pareador | Teto aplicado | Resultado nos dois lotes | [nao verificado]
|---|---|---|---|
| E **155585**, 15/11 07:05 | **Fecho normal** com a S **155607**. `07:05` aparece em `dia['orfas']` nas duas grades. **Não responde** qual passo a colocou na grade viva, porque a sonda traz a lista final. `app/ponto/turnos.py:1491`–`1493`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` | **1.560 min**, envelope de 26 h do fecho. A L-084 substituiu os **750 min** de `_teto_da_jornada` (510 do DNA de 15/11 + 240), pois as distâncias **344,668 / 715,161 min** superaram 180 min. Vão de **129,829 min**. `cont_max_s` entrou com 840 min. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1193`, `app/ponto/turnos.py:1430`–`1447` | **E do par fechado 155585→155607** em 15/11. `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503` |
| S **155607**, 15/11 09:15 | **Fecho normal**, como saída de **155585**. `09:15` aparece em `dia['orfas']` nas duas grades. **Não responde** qual passo a colocou na grade viva, porque a sonda traz a lista final. `app/ponto/turnos.py:1491`–`1493`, `app/escala/utils.py:2334`, `app/escala/services/leitor_celula.py:420` | **1.560 min**, na mesma comparação. A S estava a **324,839 min** do marco de pausa mais próximo, fora dos **90 min** do ramo de absorção. `app/ponto/turnos.py:1356`–`1365`, `app/ponto/turnos.py:1444`–`1447` | **S do mesmo par fechado** em 15/11. `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503` |
| Escala/célula | Vínculo **1203**, template **181** (6×1). DNA recebido pelos dois lotes de 15 a 17/11 **(12:50, 21:20, 14:40, 15:50)**. Em 14/11 o mapa veio sem DNA e o EC retornou esses mesmos marcos. Célula de trabalho **123743** em 15/11 na grade lavrada. `app/escala/utils.py:84`, `app/escala/models.py:1135`–`1144`, `app/ponto/turnos.py:1997`, `app/ponto/motor_calculo_v2.py:503`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424` | | |

## 23/11 15:10 · sessao 48945df890d9 · PAR-PELO-TURNO · leitura · origem=Code

```python
# PAR-PELO-TURNO | eac7a67419eb54c3af01f061dde311b5bae06717
# Na rotina de ensaio da sombra, envie este texto pela entrada padrão de
# `python -B manage.py shell`, com DJANGO_SETTINGS_MODULE=config.settings.sombra.
# A rotina monta a cópia em somente leitura e usa tmpfs para os logs. bin/sombra.sh:182
# Execução e conteúdo do banco de ensaio [nao verificado].

import linecache
import logging
import sys
from datetime import date, datetime, time, timedelta

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from escala import utils as grade
from escala.services.leitor_celula import grade_da_celula
from escala.servico_jornada import escala_vigente
from ponto import motor_calculo_v2 as motor
from ponto import turnos
from ponto.services.espelho import autoridade_do_periodo

T = "app/ponto/turnos.py"
M = "app/ponto/motor_calculo_v2.py"
G = "app/escala/utils.py"
INI = date(2026, 11, 14)
FIM = date(2026, 11, 17)
DIA_15 = date(2026, 11, 15)
DIA_16 = date(2026, 11, 16)
UM_DIA = timedelta(days=1)
DIAS = tuple(INI + i * UM_DIA for i in range((FIM - INI).days + 1))

# Horas de referência dadas no enunciado e em aval_v2_anexo.txt:1.
ALVOS = {
    129: ((DIA_15, "17:30"), (DIA_16, "06:30")),
    155: ((DIA_15, "17:29"), (DIA_16, "06:32")),
    214: ((DIA_15, "07:00"),),
    107: ((DIA_15, "07:05"), (DIA_15, "09:15")),
}


def emitir(cid, fato, fonte, **campos):
    dados = " ".join(f"{k}={v!r}" for k, v in campos.items())
    print(f"col{cid} {fato} | {dados} | {fonte}", flush=True)


def local(ts):
    return timezone.localtime(ts).isoformat() if ts is not None else None


def batida(b):
    if b is None:
        return None
    return (b.pk, local(b.timestamp), b.tipo, b.is_intervalo)


def horas(marcos):
    if marcos is None:
        return None
    return tuple(h.isoformat() if h is not None else None for h in marcos)


def vao(entrada, saida):
    if entrada is None or saida is None:
        return None
    return (saida.timestamp - entrada.timestamp).total_seconds() / 60


def teto(marcos, envelope_s):
    segundos, do_dia = turnos._teto_da_jornada(marcos, envelope_s)
    return {"min": segundos / 60, "do_dia": do_dia}


def papel_e_flags(b):
    return {
        "papel_ata": getattr(b, "_papel_ata", None),
        "intra_dur": bool(getattr(b, "_intra_dur", False)),
        "eco_flush": bool(getattr(b, "_eco_flush", False)),
    }


def carimbo(b):
    valor = getattr(b, "_separada_pelo_teto", None)
    if valor is None:
        return None
    entrada, marcos, segundos = valor
    return {"entrada": batida(entrada), "marcos": horas(marcos), "teto_min": segundos / 60}


def imprimir_turnos(cid, leitor, resultado, fonte):
    if not resultado:
        emitir(cid, "Turno", fonte, leitor=leitor, quantidade=0)
    for t in resultado:
        bordas = {b.pk for b in (t.entrada, t.saida) if b is not None}
        emitir(
            cid, "Turno", fonte, leitor=leitor, data_turno=t.data_turno.isoformat(),
            entrada=batida(t.entrada), saida=batida(t.saida), aberto=t.aberto,
            internas=[(batida(b), papel_e_flags(b)) for b in t.batidas if b.pk not in bordas],
            vao_E_S_min=vao(t.entrada, t.saida),
            _separada_pelo_teto=carimbo(t.saida),
        )
    pares = turnos.pares_separados_pelo_teto(resultado)
    if not pares:
        emitir(cid, "pares_separados_pelo_teto", f"{T}:183", leitor=leitor, quantidade=0)
    for p in pares:
        emitir(
            cid, "pares_separados_pelo_teto", f"{T}:183", leitor=leitor,
            data_turno=p["data_turno"].isoformat(), entrada=batida(p["entrada"]),
            saida=batida(p["saida"]), hi_hf=horas((p["hi"], p["hf"])),
            teto_min=p["teto_min"], vao_E_S_min=p["vao_min"],
        )


# Os números são os pontos de execução da cópia indicada. A checagem evita
# atribuir o nome de um ramo a uma linha deslocada em outra revisão.
PONTOS = {
    1353: "turnos.append(_mk(cur, None",
    1376: "continue",
    1401: "continue",
    1416: "continue",
    1418: "turnos.append(_mk(cur, None",
    1444: "if _de_f >",
    1447: "> _teto_fecho:",
    1453: "turnos.append(_mk(cur, None",
    1477: "b._intra_dur = True",
    1488: "_declara_degenerado",
    1496: 'turnos.append(_mk({"entrada": None',
    1498: "turnos.append(_mk(cur, None",
    1094: "i += 1",
    1184: "out.append(t)",
}


class Rastro:
    def __init__(self, cid, batidas_alvo):
        self.cid = cid
        self.alvos = {b.pk for b in batidas_alvo}
        self.lotes = []
        self.ativos = {}

    def __call__(self, frame, evento, arg):
        if evento != "call":
            return None
        codigo = frame.f_code
        pai = frame.f_back
        if codigo is turnos.parear_turnos.__code__:
            if pai.f_code is turnos._turnos_do_colab_puro.__code__:
                ctx = pai.f_locals
                leitor = "turnos_do_colab"
                ini, fim, ec = ctx["seg_ini"], ctx["seg_fim"], ctx["esc"]
                classe = None
            elif pai.f_code is motor.turnos_via_autoridade.__code__:
                instancia = pai.f_locals["motor"]
                calculo = pai.f_back
                while calculo is not None and calculo.f_code is not motor.MotorBase.calcular_mes.__code__:
                    calculo = calculo.f_back
                ctx = calculo.f_locals
                leitor = "turnos_via_autoridade"
                ini, fim, ec = ctx["data_ini"], ctx["data_fim"], ctx["escala_colaborador"]
                classe = type(instancia).__name__
            else:
                return None
            lote = {
                "leitor": leitor, "ini": ini, "fim": fim, "ec_id": getattr(ec, "pk", None),
                "classe": classe, "numero": len(self.lotes) + 1, "ramos": set(),
            }
            self.lotes.append(lote)
            self.ativos[id(frame)] = lote
            return self.pareador
        if codigo in (
            turnos._fechar_aberto_com_saida_seguinte.__code__,
            turnos._fechar_aberto_na_pausa_sem_volta.__code__,
        ) and id(pai) in self.ativos:
            return self.pos_fechamento
        return None

    def fato(self, lote, linha, nome, envolvidos, *, ramo=True, **dados):
        ids = sorted(self.alvos.intersection(b.pk for b in envolvidos if b is not None))
        if ids:
            if ramo:
                lote["ramos"].update(ids)
            emitir(self.cid, nome, f"{T}:{linha}", leitor=lote["leitor"],
                   lote=lote["numero"], batidas_alvo=ids, **dados)

    def pareador(self, frame, evento, arg):
        lote = self.ativos[id(frame)]
        l = frame.f_locals
        if evento == "return":
            self.ativos.pop(id(frame))
            if arg is not None:
                lote["resultado"] = arg
                lote["batidas"] = l["bs"]
                lote["mapa"] = l["marcos_por_dia"] or {}
                lote["borda_template"] = l["marcos_borda"]
                lote["intervalo_template"] = l["marcos_intervalo"]
                lote["papeis"] = l["papel_por_minuto"] or {}
                lote["cont_max_s"] = l["cont_max_s"]
                lote["intra_max_s"] = l["intra_max_s"]
                lote["distancias"] = {
                    b.pk: (
                        horas(l["_bm_de"](b)),
                        turnos._dist_marco(b.timestamp, l["_bm_de"](b)),
                        horas(l["_ivm_de"](b)),
                        turnos._dist_marco(b.timestamp, l["_ivm_de"](b)),
                    )
                    for b in l["bs"] if b.pk in self.alvos
                }
            return self.pareador
        if evento != "line" or frame.f_lineno not in PONTOS:
            return self.pareador

        linha = frame.f_lineno
        b, cur = l.get("b"), l.get("cur")
        e = cur["entrada"] if cur else None
        if linha in (1444, 1447):
            if linha == 1444:
                self.fato(lote, linha, "distâncias L-084", (e, b), ramo=False,
                          entrada=batida(e), saida=batida(b),
                          dist_E_hi_min=l["_de_f"], dist_S_hf_min=l["_ds_f"],
                          corte_min=motor.MotorBase.CADASTRO_NAO_DESCREVE_MIN)
            else:
                self.fato(lote, linha, "comparação efetiva E→S", (e, b), ramo=False,
                          entrada=batida(e), saida=batida(b), vao_E_S_min=vao(e, b),
                          data_do_mapa=turnos.data_local(e.timestamp).isoformat(),
                          marcos=horas(l["_m_fecho"]),
                          teto_da_jornada=teto(l["_m_fecho"], l["_envelope_fecho"]),
                          teto_efetivo_min=l["_teto_fecho"] / 60,
                          do_dia_efetivo=l["_do_dia_fecho"])
        elif linha == 1453:
            c = getattr(b, "_separada_pelo_teto", None)
            if c is not None and c[0].pk == e.pk:
                motivo = "ramo teto do dia decisivo"
            elif not l["_do_dia_fecho"]:
                motivo = "ramo envelope do fechamento"
            else:
                motivo = "ramo fechamento acima do teto sem carimbo decisivo"
            self.fato(lote, linha, motivo, (e, b), entrada=batida(e), saida=batida(b),
                      _separada_pelo_teto=carimbo(b))
        elif linha == 1353:
            self.fato(lote, linha, "ramo absorção cont_max_s encerra a tentativa", (e, b),
                      entrada=batida(e), marca=batida(b), vao_desde_E_min=vao(e, b),
                      cont_max_min=l["cont_max_s"] / 60, dist_interna_min=l["_di_fora"])
        elif linha == 1376:
            motivo = "papel da ata" if l["_pa_b"] in ("X", "Xi") else "intervalo"
            self.fato(lote, linha, f"ramo {motivo} sem borda", (b,), marca=batida(b),
                      entrada_aberta=batida(e), **papel_e_flags(b))
        elif linha == 1401:
            self.fato(lote, linha, "ramo eco de flush", (b,), marca=batida(b), entrada_aberta=batida(e))
        elif linha in (1416, 1477):
            self.fato(lote, linha, "ramo pausa deslocada", (b,), marca=batida(b),
                      entrada_aberta=batida(e))
        elif linha == 1418:
            self.fato(lote, linha, "ramo nova E deixa a E anterior aberta", (e,),
                      entrada_aberta=batida(e), nova_entrada=batida(b), papel_nova_E=l["_pa_b"])
        elif linha == 1488:
            self.fato(lote, linha, "ramo par degenerado", (e, b),
                      entrada=batida(e), saida=batida(b), vao_E_S_min=vao(e, b))
        elif linha == 1496:
            self.fato(lote, linha, "ramo S sem E aberta", (b,),
                      saida=batida(b), **papel_e_flags(b))
        elif linha == 1498:
            self.fato(lote, linha, "ramo fim da sequência com E aberta", (e,), entrada=batida(e))
        return self.pareador

    def pos_fechamento(self, frame, evento, arg):
        if evento == "line" and frame.f_lineno in (1094, 1184):
            linha = frame.f_lineno
            l = frame.f_locals
            t = l["t"]
            s = l["_sai"] if linha == 1094 else l["_last"]
            lote = self.ativos[id(frame.f_back)]
            self.fato(
                lote, linha, f"ramo teto em {frame.f_code.co_name}", (t.entrada, s),
                data_turno=t.data_turno.isoformat(), entrada=batida(t.entrada), saida=batida(s),
                vao_E_S_min=l["dur_s"] / 60,
                teto_da_jornada=teto((l["marcos_por_dia"] or {}).get(t.data_turno), l["cont_max_s"]),
            )
        return self.pos_fechamento


def relatar_lote(cid, lote, batidas_alvo):
    leitor = f'{lote["leitor"]} lote {lote["numero"]}'
    fonte = f"{M}:503" if lote["classe"] else f"{T}:1997"
    emitir(cid, "alimentação do pareador", fonte, leitor=leitor,
           janela=(lote["ini"].isoformat(), lote["fim"].isoformat()),
           vinculo_id=lote["ec_id"], motor=lote["classe"],
           cont_max_min=lote["cont_max_s"] / 60, intra_max_min=lote["intra_max_s"] / 60,
           borda_template=horas(lote["borda_template"]),
           intervalo_template=horas(lote["intervalo_template"]),
           batidas_entrada=[batida(b) for b in lote["batidas"]])

    for d in DIAS:
        marcos = lote["mapa"].get(d)
        emitir(cid, "DNA passado ao pareador e tetos por data de referência",
               f"{fonte} {T}:66 {T}:1086 {T}:1177 {T}:1434", leitor=leitor,
               data_turno_referencia=d.isoformat(), hi_hf_hii_hfi=horas(marcos),
               teto_base_fecho=teto(marcos, 26 * 3600),
               teto_pos_fecho=teto(marcos, lote["cont_max_s"]))

    papeis = lote["papeis"]
    if not papeis:
        emitir(cid, "papel_por_minuto_da_ata", f"{T}:1565 {fonte}", leitor=leitor, papel=None)
    for minuto, papel in sorted(papeis.items()):
        ts = timezone.make_aware(minuto) if timezone.is_naive(minuto) else minuto
        emitir(cid, "papel_por_minuto_da_ata", f"{T}:1565 {fonte}",
               leitor=leitor, minuto=local(ts), papel=papel)

    resultado = lote["resultado"]
    if lote["classe"]:
        imprimir_turnos(cid, leitor, resultado, f"{M}:503 {T}:43 {T}:180")
    bs = lote["batidas"]
    indices = {b.pk: i for i, b in enumerate(bs)}
    for alvo in batidas_alvo:
        i = indices.get(alvo.pk)
        if i is None:
            emitir(cid, "alvo ausente da entrada deste lote; ramo [nao verificado]",
                   fonte, leitor=leitor, alvo=batida(alvo))
            continue
        b = bs[i]
        bordas, dist_borda, marcos_intra, dist_intra = lote["distancias"][b.pk]
        e_ref = next((x for x in reversed(bs[:i]) if x.tipo == "E"), None)
        s_ref = next((x for x in bs[i + 1:] if x.tipo == "S"), None)
        destinos = []
        for t in resultado:
            if not any(x.pk == b.pk for x in t.batidas):
                continue
            if t.entrada is not None and t.entrada.pk == b.pk:
                papel = "E aberta" if t.aberto else "E de par"
            elif t.saida is not None and t.saida.pk == b.pk:
                papel = "S órfã" if t.entrada is None else "S de par"
            else:
                papel = "interna"
            destinos.append((t.data_turno.isoformat(), papel, batida(t.entrada), batida(t.saida),
                             bool(lote["ini"] <= t.data_turno <= lote["fim"])))
        emitir(
            cid, "alvo no resultado do pareador", f"{T}:326 {T}:1237 {T}:1382 {T}:1997",
            leitor=leitor, alvo=batida(b), destinos_e_data_na_fatia=destinos,
            hi_hf_efetivos=bordas, dist_borda_mais_proxima_min=dist_borda,
            marcos_intra_efetivos=marcos_intra, dist_intra_mais_proxima_min=dist_intra,
            E_gravada_anterior=batida(e_ref), S_gravada_posterior=batida(s_ref),
            vao_referencia_E_anterior_ate_alvo_min=vao(e_ref, b),
            vao_referencia_alvo_ate_S_posterior_min=vao(b, s_ref),
            **papel_e_flags(b),
        )
        if b.pk not in lote["ramos"] and (
            not destinos or any(d[1] in ("E aberta", "S órfã", "interna") for d in destinos)
        ):
            emitir(cid, "ramo [nao verificado]", fonte, leitor=leitor, alvo=batida(b))


def imprimir_grade(cid, fonte, nome, por_data, dados_grade):
    for d in (DIA_15, DIA_16):
        dia = por_data.get(d)
        if dia is None:
            emitir(cid, "dia da grade [nao verificado]", fonte, leitor=nome, data=d.isoformat())
            continue
        orfas = dia["orfas"]
        _, orfas_realizado = grade.montar_realizado_grade(dia, dados_grade["marcos"])
        emitir(cid, "dia['orfas']", fonte, leitor=nome, data=d.isoformat(),
               tipo_dia=dia.get("tipo_dia"), celula_id=dia.get("celula_id"), orfas=orfas)
        for civil, hhmm in ALVOS[cid]:
            emitir(cid, "presença na lista por HH:MM", f"{fonte} {G}:1089", leitor=nome,
                   data_grade=d.isoformat(), data_civil_alvo=civil.isoformat(), hora_alvo=hhmm,
                   em_dia_orfas=hhmm in orfas, em_montar_realizado_grade=hhmm in orfas_realizado,
                   orfa_sem_hora="?" in orfas)


def sondar(cid, agora):
    colab = Colaborador.objects.select_related("empresa").get(pk=cid)
    emitir(cid, "empresa do colaborador", "app/colaboradores/models.py:150 app/colaboradores/models.py:321",
           empresa_id=colab.empresa_id, regime_trabalhista=colab.empresa.regime_trabalhista)

    inicio = timezone.make_aware(datetime.combine(INI, time.min))
    fim = timezone.make_aware(datetime.combine(FIM, time.max))
    bs = list(turnos.batidas_apuraveis(colab, inicio, fim))
    for b in bs:
        emitir(cid, "batida apurável", f"{T}:2877 app/ponto/models.py:23",
               batida_id=b.pk, timestamp=local(b.timestamp), tipo=b.tipo,
               is_intervalo=b.is_intervalo, retratada_em=local(b.retratada_em))

    por_minuto = {}
    for b in bs:
        ts = timezone.localtime(b.timestamp)
        por_minuto.setdefault((ts.date(), ts.strftime("%H:%M")), []).append(b)
    batidas_alvo = []
    for d, hhmm in ALVOS[cid]:
        correspondentes = por_minuto.get((d, hhmm), [])
        batidas_alvo.extend(correspondentes)
        emitir(cid, "referência do caso", f"aval_v2_anexo.txt:1 {T}:2877",
               data_civil=d.isoformat(), hora=hhmm,
               apuraveis=[batida(b) for b in correspondentes])
        if not correspondentes:
            emitir(cid, "ramo [nao verificado]", f"{T}:2877",
                   data_civil=d.isoformat(), hora=hhmm, apuravel_no_minuto=False)

    dna = grade.marcos_dna_periodo(cid, INI, FIM)
    for d in DIAS:
        ec = escala_vigente(colab, d)
        marcos_ec = ec.marcos_do_dia(d) if ec else None
        emitir(cid, "marcos do dia",
               f"{G}:84 app/escala/servico_jornada.py:15 app/escala/models.py:1109",
               data=d.isoformat(), vinculo_id=getattr(ec, "pk", None),
               EC_hi_hf_hii_hfi=horas(marcos_ec), DNA_hi_hf_hii_hfi=horas(dna.get(d)))
        if d == DIA_15:
            te = ec.tipo_escala if ec else None
            emitir(cid, "template vigente em 15/11",
                   "app/escala/servico_jornada.py:15 app/escala/models.py:46 app/escala/models.py:117 app/escala/models.py:130",
                   vinculo_id=getattr(ec, "pk", None), template_id=getattr(te, "pk", None),
                   nome=(te.template_nome or te.nome_canonico()) if te else None,
                   hi_hf=horas((te.hora_inicio, te.hora_fim)) if te else None,
                   ciclo=te.tipo_ciclo if te else None, ciclo_dias=te.ciclo_dias if te else None)

    emitir(cid, "caminho de produção do motor",
           "app/ponto/services/espelho.py:1263 app/ponto/services/espelho.py:1147 "
           "app/ponto/services/espelho.py:1200 app/ponto/services/fechamento.py:286 "
           "app/ponto/services/fechamento.py:735 app/ponto/services/fechamento.py:754 "
           f"{M}:530",
           caminho="autoridade_do_periodo → get_motor_cct por fatia → calcular_mes → turnos_via_autoridade")

    rastro = Rastro(cid, batidas_alvo)
    anterior = sys.gettrace()
    sys.settrace(rastro)
    try:
        juiz = turnos.turnos_do_colab(colab, INI, FIM)
        autoridade = autoridade_do_periodo(colab, INI, FIM)
    finally:
        sys.settrace(anterior)

    imprimir_turnos(cid, "turnos_do_colab", juiz, f"{T}:1850 {T}:1997 {T}:43 {T}:180")
    for lote in rastro.lotes:
        relatar_lote(cid, lote, batidas_alvo)
    if not any(lote["classe"] for lote in rastro.lotes):
        emitir(cid, "motor sem chamada observada a turnos_via_autoridade; ramo [nao verificado]",
               f"app/ponto/services/espelho.py:1198 {M}:530")

    viva = grade.grade_espelho_janela(colab, autoridade.batidas, INI, FIM, agora=agora)
    por_data_viva = {FIM - i * UM_DIA: dia for i, dia in enumerate(viva["dias"])}
    imprimir_grade(cid, f"{G}:2334 {G}:962", "grade viva", por_data_viva, viva)

    lavrada = grade_da_celula(colab, INI, FIM)
    por_data_lavrada = {dia["dat

[truncado em 20000 de 21358 caracteres]

## 23/11 15:10 · sessao e661bebbc12f · VIGIA-DO-TMUX · verificador · origem=Code

Revisão em leitura, pelo diff anexado e pelo código da cópia. Hash Git, crontab efetivo, configuração do tmux e ocorrências em produção [nao verificado]. O selo completo [nao verificado] em execução, pois cria e apaga arquivos temporários. `bin/tests/test_vigia_tmux.sh:19-26,95-110`

### 1. VEREDITO **ok**

O bloco antigo do anexo e o novo fazem as mesmas operações, na mesma ordem. Removem as crases finais, dividem por `|`, aparam os campos e exigem uma palavra para o produtor. Aplicam os mesmos `fullmatch` para `pid=` e `arquivo=`, convertem o PID com `int`, rejeitam campo desconhecido ou repetido com o mesmo trecho de 30 caracteres e rejeitam a ausência dos dois campos. O chamador reconstrói a mesma tupla `(True, motivo, linha)`, conserva a prova de PID antes da de arquivo e contém as mesmas exceções. Os dois textos de `pid morto` também permanecem iguais. Sem contraexemplo no diff fornecido. `bin/hook_stop_fila1.py:599-628,788-849` · anexo `v.diff`, hunks `-596`, `-614`, `-780` e `-806`

O B3 usa o recorte compartilhado sobre uma leitura consistente do RELATO e reconhece `DESENHO` e `ERRO-BOBO` pelo próprio reconhecedor. Esse caminho não chama o parser de `ESPERANDO`. `bin/disjuntor.py:722-750,758-785,978,1018-1025`

### 2. VEREDITO **ACHADO**

| Estado | Quem escreve e quem lê |
|---|---|
| Topo do RELATO | A edição do painel é atribuída ao Code pelas instruções da cópia. O Disjuntor grava a faixa do censo por `no_topo`. Leem o vigia, o Stop, o B3, o handoff e o alarme de sessão, com janelas próprias para os dois últimos. Autor de cada edição em produção [nao verificado]. `CLAUDE.md:263-282` · `bin/hook_stop_fila1.py:408-484,823-839,1108-1125,1265` · `bin/vigia_tmux.py:266-274` · `bin/disjuntor.py:299-326,758-785` · `bin/handoff_sessao.sh:85-109` · `bin/alarme_sessao_ociosa.py:119-141` |
| `logs/vigia_tmux/feitos.json` | O vigia lê por `_le_feitos`, consulta pelo SHA e grava por `_marca` e `os.replace`. `quando` é o instante da última transição. `hora_clear` é o instante anterior à tentativa, embora seja usado como referência do envio. Ambos são epoch em segundos. Outro escritor efetivo [nao verificado]. `bin/vigia_tmux.py:98-119,187-199,221-255,270-274` |
| `.trava` | O vigia abre ou cria e toma `flock` exclusivo não bloqueante. O caminho usa a trava sem ler ou escrever conteúdo. Outro usuário da trava [nao verificado]. `bin/vigia_tmux.py:62,259-266` |
| `PAUSADO` | `pausar.sh` grava por renomeação, lê com `--ler` e remove com `--soltar`. O vigia só consulta existência. O selo cria seu próprio arquivo na fixture. `bin/pausar.sh:19-47` · `bin/vigia_tmux.py:63,259-261` · `bin/tests/test_vigia_tmux.sh:167` |
| `logs/sessao/ultimo_clear.json` | `marco_de_clear.py` lê o anterior e grava sob trava, chamado pelos hooks de `clear` e `compact`. O vigia lê diretamente. Stop e gerador de ESTADO passam pelo leitor do orçamento. `hora` é o epoch do último evento, inclusive `compact`. `clear_horas` guarda os últimos 20 clears sem sessão por entrada. `session_id` pertence só ao último evento. `iso` é hora local sem offset explícito. `bin/marco_de_clear.py:19-20,32-63` · `.claude/settings.json:34-59` · `bin/vigia_tmux.py:207-214` · `bin/orcamento_contexto.py:152-164,352-361` · `bin/hook_stop_fila1.py:1028-1044` · `bin/gerar_estado.py:279-284` |
| `vigia.log` | O vigia apenda por `_log`, inclusive no `excepthook`. O selo lê e cobra mensagens. Leitor operacional [nao verificado]. O log usa data completa e hora local sem offset. A entrada no RELATO usa dia/mês e hora, sem ano nem offset. Fuso efetivo do host [nao verificado]. `bin/vigia_tmux.py:80-95,170-173,187-203` · `bin/tests/test_vigia_tmux.sh:111-116,126,148,160` |
| `ALARMES DA ESTEIRA` | `esteira_vigia.no_relato` lê a seção para inserir e regrava o arquivo. Chamam a porta o vigia, a esteira, o Disjuntor, o integrador, o fabricante e o vigia da árvore. O verificador de prova do Stop e a publicação leem o RELATO inteiro. `app/core/esteira_vigia.py:815-843,895-927,1215` · `bin/vigia_tmux.py:170-184` · `bin/disjuntor.py:290-293` · `app/core/integrador_lote.py:649,743,757,768` · `bin/fabricante.sh:44,113` · `bin/esteira.sh:96` · `bin/vigia_arvore.sh:166-170` · `bin/hook_stop_fila1.py:1329-1336` · `bin/relato.sh:54-57,119` |

**Contraexemplos.** `gerar_cortes.py` e `deploy_agendado.sh` regravam o RELATO inteiro fora da trava da porta. Se um deles lê a versão anterior, o vigia insere um alarme e depois o outro gravador salva sua versão, o alarme some. Além disso, uma caixa ocupada gera `adiado` só em `vigia.log`. O retorno ocorre antes da chamada que escreveria no RELATO, contrariando o item 4 do contrato. `bin/gerar_cortes.py:114-136` · `bin/deploy_agendado.sh:31-44` · `app/core/esteira_vigia.py:839-860` · `bin/vigia_tmux.py:170-173,187-203`

### 3. VEREDITO **ACHADO**

- **Sessão.** Todos os envios deste vigia passam `=code:` e a checagem passa `=code`. O código não monta um alvo `codex-hasner`. O painel, porém, é resolvido novamente a cada chamada. Se o painel ativo de `code` mudar entre a captura, o literal e o Enter, a checagem pode observar um painel e a tecla atingir outro. A topologia real [nao verificado]. `bin/vigia_tmux.py:66-67,126-128,149-167`
- **Repetição.** Com a mesma linha normalizada, o JSON preservado e a mesma trava, `ESPERANDO` bloqueia qualquer SHA já gravado. No CLEAR, os estados pendentes permitem só a retomada. Se `feitos.json` desaparecer entre ticks, `_le_feitos` devolve `{}` e a mesma linha pode causar outro envio. Também basta tirar a crase **final** de um `ESPERANDO` já atendido para mudar o SHA e produzir a mesma frase. O leitor conserva essa crase para o hash e o parser a descarta. `bin/vigia_tmux.py:74,98-105,221-255,264-274` · `bin/hook_stop_fila1.py:490-499,793-810`
- **Turno ou caixa ocupada.** Depois da segunda captura, o vigia ainda serializa e substitui `feitos.json`. Se Ronald digitar nesse intervalo, o literal entra na caixa alterada. Depois há mais `0,3 s` antes do Enter, sem recaptura. O Enter pode submeter o texto misturado. A janela declarada como “milissegundos” omite a gravação, o segundo intervalo e a possibilidade de troca de painel. Há também falso ocioso sem corrida. Uma captura com `❯ texto da caixa`, seguida por uma linha de continuação `❯` e pela borda `────`, passa porque o leitor escolhe o último `❯`. `bin/vigia_tmux.py:20-25,108-119,126-167,187-199`

### 4. VEREDITO **ACHADO**

- **Tecla parcial.** O literal `/clear` pode entrar na caixa e o subprocesso do Enter falhar ou expirar. Com exceção, fica `enviando-clear`. Com retorno não zero, fica `falhou-clear`. Nenhum dos dois estados reabre o envio. Um erro posterior também pode ocorrer com `/clear` já submetido, por exemplo ao ler um JSON inválido do marco. Portanto, o efeito pode ser texto deixado na caixa ou `/clear` aplicado sem retomada. `bin/vigia_tmux.py:74,91-95,122-123,161-167,187-214,239-250`
- **Marca sem tecla.** Um `arquivo=` com byte NUL embutido passa pelo `\S+`. Com PID aceito e tela ociosa, `_marca` grava `enviando-esperando`. A criação do primeiro subprocesso falha com o argumento inválido e o tick seguinte abandona o mesmo SHA. O mesmo estado ocorre se o executável `tmux` desaparecer depois das capturas e antes do primeiro envio. `bin/hook_stop_fila1.py:793-810` · `bin/vigia_tmux.py:115-123,161-167,187-194,221-232`
- **Saída 0 também falha.** `VIGIA_TMUX_TETO_CLEAR=x` falha na inicialização, antes de instalar o handler. Se `vigia.log` for um diretório, o próprio handler pode levantar ao tentar registrar o erro, antes de `_exit(0)`. JSON de feitos corrompido interrompe todos os ticks que encontrarem uma linha reconhecida enquanto o arquivo permanecer assim. `bin/vigia_tmux.py:68,84-105,270-280`

### 5. VEREDITO **ACHADO**

O vigia chama a mesma `_prova_pid` do Stop. Ela lê estado e `starttime` de `/proc/<pid>/stat` antes e depois de ler `exe` e `cmdline`. Dois `starttime` diferentes dão “pid trocou”; `PermissionError` e outros erros de leitura tratados dão “pid ilegível”. O mecanismo é o de `/proc`, embora o contrato diga `kill -0`. `bin/hook_stop_fila1.py:570-574,604-631` · `bin/vigia_tmux.py:221-232`

**Contraexemplo.** O primeiro `stat` devolve `('S', 123)`, mas `readlink('/proc/4321/exe')` levanta `FileNotFoundError`. A função retorna `pid morto (4321)` sem reler o `stat`, e o vigia aceita. A exceção vale para todo o bloco de leitura. Reciclagem **antes** do primeiro `stat` também é invisível, pois o painel só fornece o PID. Se o substituto já estiver zumbi ou cair no mesmo erro de leitura, pode ser aceito como morto sem casar com o produtor. Ocorrências reais [nao verificado]. `bin/hook_stop_fila1.py:615-631,793-810` · `bin/vigia_tmux.py:221-232`

### 6. VEREDITO **ACHADO**

- **Vacuidade parcial.** `c1` aceita qualquer `morte nao provada`, inclusive `cmdline nao casa`, então pode passar sem reconhecer o produtor vivo. O caso `e` põe o comando velho na linha física 9, já atrás de uma primeira linha útil. Assim, repete a rejeição exercitada por `c10` sem medir a borda da 15ª linha. Os casos positivos exigem envios concretos. `bin/tests/test_vigia_tmux.sh:82-89,95-101,121-155,158,171-177,189-193` · `bin/vigia_tmux.py:228-231`
- **Mutante que escapa.** Trocar só o alvo de `capture-pane` para `codex-hasner` preserva as saídas esperadas. O falso não verifica o alvo da captura, as igualdades cobram os envios e o AST só confere as constantes e certas chamadas `.get`. Também escapam remover o `flock` ou a marca anterior à tecla, pois o selo roda em série e o `send-keys` falso sempre devolve zero. `bin/vigia_tmux.py:128,193,264-265` · `bin/tests/test_vigia_tmux.sh:30-51,103-113,122-155,179-187`
- **Isolamento.** A versão da cópia usa o `tmux` do `PATH` falso e repassa a raiz da fixture ao Stop e à porta do RELATO. Com `VIGIA_TMUX_BIN`, porém, o selo executa um candidato. Remover dele apenas `ev.RELATO = H.RELATO` faz a porta tentar gravar no caminho absoluto do RELATO real antes de o caso `a` detectar a falta do alarme na fixture. Um candidato que invoque `tmux` por caminho absoluto também contorna a checagem de `PATH`. Efeito no host [nao verificado]. `bin/tests/test_vigia_tmux.sh:13-17,103-108,122-129` · `bin/vigia_tmux.py:55-58,122-123,177-184` · `app/core/esteira_vigia.py:38-43,895-927`

### 7. VEREDITO **ACHADO**

- **Juízo e fonte única.** O vigia reutiliza o leitor do topo e o reconhecedor de CLEAR. O recorte compartilhado, porém, pode aceitar um `ESPERANDO` na linha 17, depois do cabeçalho e 15 linhas em branco. Também pode devolver a primeira linha após um `OSError` posterior. A autorização do CLEAR é paralela à guarda do Stop. Uma linha com o literal exigido e um painel ocioso pode disparar `/clear` enquanto o Stop recusaria a liberação por handoff velho ou DIFF/apply em curso. A leitura do marco também é paralela. Um symlink para JSON com `clear_horas` posterior é aceito pelo vigia e recusado pelo leitor do orçamento. `bin/hook_stop_fila1.py:408-416,462-484,886-887,945-962,1074-1129` · `bin/vigia_tmux.py:207-214,235-250,266-274` · `bin/orcamento_contexto.py:68-81,152-164`
- **Relógio fora da borda.** `hora_clear` é lida antes das capturas e do Enter. Se outra sessão der `/clear` entre esse instante e o envio em `code`, o marco global pode autorizar a retomada mesmo sem confirmar o clear de `code`. A comparação usa só o número, e o histórico não guarda a sessão de cada clear. `bin/vigia_tmux.py:149-167,187-194,207-214,239-250` · `bin/marco_de_clear.py:43-61`
- **Escritores e ordem de trava.** O vigia passa pela porta de ALARMES. As regravações externas do RELATO estão no item 2. Há ainda um segundo emissor de teclas na cópia, `keepalive.sh`, que usa `-t code`, flag própria e nenhuma `.trava` compartilhada. Operação simultânea em produção [nao verificado]. No vigia, a ordem observada é `.trava` → diretório do RELATO, sem caminho inverso localizado. O defeito de ordenação é consultar `PAUSADO` uma vez antes da trava. Uma pausa criada enquanto o tick espera o marco ainda permite a retomada. `bin/keepalive.sh:4-9` · `bin/pausar.sh:39-46` · `bin/vigia_tmux.py:170-204,244-274` · `app/core/esteira_vigia.py:843-862`
- **Catch-all e selo.** O teste de AST passa com zero `ExceptHandler`, mesmo com o `sys.excepthook` que contém falhas depois de efeitos. O mutante de alvo da captura do item 6 também passa pelas verificações presentes. `bin/tests/test_vigia_tmux.sh:30-51,179-187,199-202` · `bin/vigia_tmux.py:91-95,187-203,277-280`
- **Agendamento.** A fonte da cópia não contém a entrada `*/3` do vigia. O `install` usa o render de `CRONS`, portanto instalar a partir desta cópia não instala esse agendamento. Timer efetivamente instalado no host [nao verificado]. `app/config/crons.py:281-755,764-767,1895-1925,1975-1976` · `bin/crons.sh:23-27,35-40`

## 23/11 15:09 · sessao 542ac8c9a8f9 · - · - · origem=Ronald (tmux)

**6. ACHADO.** O selo tem casos positivos que comparam as teclas esperadas. O c1 aceita qualquer `morte nao provada`, inclusive `cmdline nao casa` e `ancestrais ilegiveis`, então pode passar sem confirmar a identidade do produtor vivo. O caso e põe o comando antigo na linha 9 e depois de uma linha útil. Ele cobre a precedência da primeira linha útil, como c10, e deixa a borda física de 15 linhas sem teste. `bin/tests/test_vigia_tmux.sh:82–89,95–101,121–155,158,171–177,189–193` · `bin/vigia_tmux.py:228–231` · `bin/hook_stop_fila1.py:610–630`.

- Mutantes que escapam por inspeção: retirar o `flock` ou a marcação anterior à tecla, pois o selo roda sequencialmente e o falso sempre devolve sucesso em `send-keys`. Aceitar qualquer timestamp numérico em `clear_horas` também passa, pois cada fixture fornece um marco ausente ou recente. Trocar o único argumento `texto` por `*texto.split()` deixa as mesmas strings no log falso, que recompõe argumentos com `"$*"`. `bin/vigia_tmux.py:163,193,207–214,264–265` · `bin/tests/test_vigia_tmux.sh:41–51,103–113,121–155`.
- O caminho atual usa o falso no `PATH` e redireciona tanto o leitor quanto a porta de escrita do RELATO para a fixture. O selo aceita outro `bin/`, e sua barreira não contém um mutante que invoque tmux por caminho absoluto. Remover `ev.RELATO = H.RELATO` basta para a porta tentar usar `/home/ronald/saas-hasner/app/docs/RELATO.md` antes de a asserção na fixture falhar. Efeito em produção [nao verificado]. `bin/tests/test_vigia_tmux.sh:14–15,105–108,123–129` · `bin/vigia_tmux.py:55–58,122–123,177–184` · `bin/hook_stop_fila1.py:24–32` · `app/core/esteira_vigia.py:38–43,815–843,895–927`.

**Achados complementares para 3, 4 e 7.** A janela inclui gravação em disco após a segunda captura e 0,3 s entre literal e Enter. Cada comando resolve o painel de `=code:` novamente. Uma continuação da caixa começada por `─` passa como borda. Em `CLEAR`, `hora_clear` é capturada antes da sondagem e o histórico compartilhado aceita clear de outra sessão sem comparar identidade. `bin/vigia_tmux.py:66–67,126–167,187–194,207–214,239–250` · `.claude/settings.json:34–59` · `bin/marco_de_clear.py:43–57`.

Uma falha ao iniciar o primeiro `send-keys` persiste `enviando-*` sem tecla. Um timeout no segundo pode deixar o literal sem Enter. Uma falha do próprio log impede o `_exit(0)`. `VIGIA_TMUX_TETO_CLEAR=x` levanta antes da instalação do handler. `bin/vigia_tmux.py:68,84–95,122–123,161–167,187–198,221–245,277–280`.

A fixture ociosa contém `◯ construtor-x Rodando suite`, o que o `keepalive` bloqueia e o vigia aceita. O alarme usa a captura inteira para a mesma expressão `esc to interrupt`, enquanto o vigia só considera 30 linhas. `bin/tests/test_vigia_tmux.sh:59–67,122–125` · `bin/keepalive.sh:5–8` · `bin/alarme_sessao_ociosa.py:81–86` · `bin/vigia_tmux.py:131–143`.

## 23/11 15:08 · sessao 71793dd299ef · - · - · origem=Ronald (tmux)

**1. VEREDITO ok.** O trecho antigo de `v.diff` (`@@ -806`) e `bin/hook_stop_fila1.py:793–810` executam a mesma sequência. Ambos retiram crases finais, dividem por `|`, exigem um produtor de uma palavra, casam integralmente `pid=(\d+)` e `arquivo=(\S+)`, convertem o PID em `int`, recusam campos desconhecidos ou repetidos com a mesma mensagem e recusam a ausência dos dois campos. O Stop reconstrói a mesma tupla de recusa e mantém a ordem da prova PID → arquivo e o mesmo tratamento de exceções. Os dois textos `pid morto` também são idênticos após a extração da constante. A comparação em memória com 21.960 linhas construídas a partir dos dois parsers produziu resultados iguais. O B3 usa o recorte compartilhado do topo e julga `DESENHO` e `ERRO-BOBO` em outro caminho. `bin/hook_stop_fila1.py:601,621–628,793–810,837–849` · `bin/disjuntor.py:722–750,758–785`

**5. VEREDITO ACHADO.** O vigia chama a mesma `_prova_pid` do Stop e aceita o prefixo de morte. A função extrai o `starttime` de `/proc/<pid>/stat`, compara as duas amostras e recusa mudança de processo. `PermissionError`, outros `OSError` e erros de parse tornam a prova ilegível. Já qualquer `FileNotFoundError` na leitura de `stat`, `exe` ou `cmdline` vira morte. Contraexemplo reproduzido em memória com a função da cópia: primeiro `stat` de 4321 retorna `('S', 123)`, `readlink('/proc/4321/exe')` levanta `FileNotFoundError` e o resultado é `pid morto (4321)`, antes de reler `stat` ou conferir o produtor. A reciclagem anterior à primeira leitura é indistinguível porque a linha só traz o PID. Uma troca detectada entre duas leituras completas é recusada, mas um substituto com `exe` ausente ou em `Z`/`X` pode cair no motivo de morte antes de conferir executável e argumentos. Ocorrência em produção [nao verificado]. `bin/hook_stop_fila1.py:570–574,615–631,793–809` · `bin/vigia_tmux.py:221–232`

## 23/11 15:08 · sessao 3047917a4145 · - · - · origem=Ronald (tmux)

**2. ACHADO.** O vigia grava `feitos.json` por `_marca` e `os.replace`, lê o arquivo em `_le_feitos` e serializa os ticks com `.trava`. `quando` é o instante da transição, enquanto `hora_clear` é amostrada antes das capturas e do envio. `PAUSADO` é escrito/removido por `pausar.sh` e consultado uma vez antes da trava. `vigia.log` recebe append do vigia, inclusive pelo `excepthook`, e é lido pelo selo. Os horários textuais usam hora local sem fuso escrito. [bin/vigia_tmux.py:59-64,80-119,187-204,235-265; bin/pausar.sh:19-47; bin/tests/test_vigia_tmux.sh:111-116]

- Topo. O contrato de edição pelo Code aparece nas instruções do Stop. O Disjuntor grava a faixa do censo via `no_topo`. Stop, vigia, B3, handoff e alarme de sessão consultam o topo com as respectivas janelas. [bin/hook_stop_fila1.py:408-484,1108-1109,1123-1125; bin/vigia_tmux.py:266-274; bin/disjuntor.py:299-326,759-786; bin/handoff_sessao.sh:85-108; bin/alarme_sessao_ociosa.py:119-141]
- Marco. `marco_de_clear.py` lê e escreve sob trava, chamado em `clear` e `compact`. `hora` é o último evento, `clear_horas` conserva os últimos 20 clears globais e `session_id` pertence somente ao último evento. O vigia lê `clear_horas` diretamente. Stop e gerador de estado passam pelo leitor de orçamento. [bin/marco_de_clear.py:32-63; .claude/settings.json:34-59; bin/vigia_tmux.py:207-214; bin/orcamento_contexto.py:152-164,352-361; bin/hook_stop_fila1.py:1028-1044; bin/gerar_estado.py:279-284]
- ALARMES. O vigia e os demais emissores passam pela mesma porta de escrita. Ela lê a seção para inserir a linha e regrava o arquivo. `gerar_cortes.py` e `deploy_agendado.sh` regravam o arquivo inteiro fora da porta. Se um deles lê a versão anterior e grava depois do vigia, perde o alarme que acabou de entrar. A trilha contratual de adiado/recusado no RELATO também falta, pois só o envio bem-sucedido chama `_no_relato`. [bin/vigia_tmux.py:170-204,223-230,235-237; app/core/esteira_vigia.py:815-843,895-927; bin/disjuntor.py:290-293; app/core/integrador_lote.py:649,743,757,768; bin/gerar_cortes.py:114-136; bin/deploy_agendado.sh:31-44]

**7. ACHADO.** O vigia reutiliza o leitor das linhas do topo, os campos de `ESPERANDO` e o reconhecedor de CLEAR. O leitor compartilhado não limita a 15 linhas físicas. Quinze linhas em branco e um `ESPERANDO` válido na 16ª permitem ação. O novo leitor do marco é paralelo e mais permissivo que o canônico. Um symlink para um JSON com timestamp futuro é aceito pelo vigia e recusado pelo orçamento. [bin/vigia_tmux.py:207-214,221-232,266-274; bin/hook_stop_fila1.py:408-416,462-484,788-810,880-887; bin/orcamento_contexto.py:68-81,152-164]

- A autorização do CLEAR diverge da guarda registrada. Com handoff velho ou DIFF/apply ativo e a linha estipulada no topo, o Stop recusa a liberação e o vigia pode mandar `/clear`. O segundo emissor de teclas `keepalive.sh` usa a mesma sessão com alvo menos estrito e flag de desligamento própria. A operação simultânea no host é [nao verificado]. [bin/hook_stop_fila1.py:945-962,1074-1129; bin/vigia_tmux.py:149-167,235-250; bin/keepalive.sh:4-9]
- `hora_clear` é lida antes das capturas e do envio. Um clear de outra sessão nessa janela satisfaz a retomada, pois a comparação guarda somente o epoch. A ordem normal é `.trava` antes do diretório do RELATO. Foi localizada essa ordem, e `PAUSADO` consultado antes de ambas não cancela um tick que já passou pela checagem. [bin/vigia_tmux.py:149-167,184,193-214,244-265; bin/marco_de_clear.py:43-61; app/core/esteira_vigia.py:846-862]
- `sys.excepthook` implementa a captura geral fora do AST de `except`. Se `vigia.log` for diretório, seu `_log` também levanta antes de `_exit(0)`. Um mutante que troca somente o alvo de `capture-pane` para `codex-hasner` passa as verificações estáticas e dinâmicas do selo, que validam o alvo de `has-session` e a digitação, mas não o alvo da captura. [bin/vigia_tmux.py:84-95,128,277-279; bin/tests/test_vigia_tmux.sh:30-43,124-130,179-187,199-202]
- O cron `*/3` do vigia está ausente da fonte consultada. O instalador usa o render de `CRONS`. Assim, um `install` dessa cópia não instala o vigia. O crontab efetivo do host permanece [nao verificado]. [app/config/crons.py:281-318,1895-1925,1975-1976; bin/crons.sh:23-27,35-40]

## 23/11 15:08 · sessao 354b8a87b524 · E4C-F5 · verificador · origem=Code

**E4C-F5, volta 6, parte B.** Os 20 arquivos alterados da cópia batem com os hashes de arquivo do `v6.diff`. A cópia veio sem metadados Git, então a identidade do commit completo e o estado do pouso ficam **[nao verificado]**. Executei o selo AST e reproduzi o comportamento do C20 em memória, em modo somente leitura. Execução dos REDs com ORM, mutação da suíte e efeitos em produção **[nao verificado]**. `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:47` `app/chamados/juizes.py:1054` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:1`

## 6. ACHADO — o CONTROLE-VERDE morde a chamada direta e deixa passar empréstimos indiretos [nao verificado]

O selo retornou **1** para `Fix.setUp(self)` com import local, **1** para `_f.Fix.tearDown(self)` com módulo e decorador apelidados, e **0** para herança com `super().setUp()`. A varredura encontrou **0 em 2.460 arquivos** sob `app/*/tests/**/*.py`. A fixture de órfãs da volta 5 agora usa herança. `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:21` `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:97` `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:118` `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:388`

Estes três casos também deram **0** em memória e podem iniciar o relógio decorado sem executar o `tearDown` correspondente:

- `getattr(Fix, "setUp")(self)` em uma classe alheia. O AST externo chama o retorno de `getattr`. `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:73`
- `class T(M, Fix)` com `super(M, self).setUp()` e `T.tearDown()` vazio. O `super` começa depois de `M` na MRO e chega a `Fix`. O receptor da chamada é uma expressão `Call`, que cai fora da análise. `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:76`
- `class M(Fix): pass` seguido de `M.setUp(self)` numa classe alheia. `M` fornece o método decorado por herança, mas o selo só registra classes com decorador escrito na própria declaração. `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:52` `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:92`

## 7. ACHADO — censo dos valores alterados e dos campos que os alimentam

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `pergunta_id`, `hora_correta`, motivos, `cartao_esperado` | Detalhe e fio leem o POST, conferem a listagem e passam o PK do cartão. O inline preenche `pergunta_id` a partir da pergunta selecionada. Correção lê hora e motivo e compara o cartão antes e sob C. Recusa lê o motivo e compara o cartão sob C. A âncora vem de `chamado_destino_id`, com fallback para `D.chamado_id`. `app/chamados/views.py:505` `app/chamados/services/fio.py:689` `app/chamados/services/fio.py:116` `app/chamados/views_cobrar.py:215` `app/chamados/services/acoes_disputa.py:93` `app/chamados/services/acoes_disputa.py:376` `app/chamados/services/acoes_disputa.py:421` `app/chamados/services/validacao.py:967` |
| P → D, D → pai/dono, P → destino/clones, `D.fechada_em` e status de Ch | P → D nasce em `_nascer`. A emissão grava D → pai/dono e fornece os IDs dos clones no nascimento. A costura grava o destino, `fechar` grava `fechada_em`, as reaberturas a zeram e `_gravar_estado` muda o status de Ch. Leem esses vínculos o fio, o inline, a pré-trava do backfill, o protocolo de travas e o plantio. `app/chamados/models.py:3292` `app/chamados/services/disputa_emissao.py:601` `app/chamados/services/disputa_emissao.py:848` `app/chamados/services/fio.py:993` `app/chamados/models.py:2160` `app/chamados/models.py:2380` `app/chamados/models.py:2404` `app/chamados/models.py:240` `app/chamados/views_cobrar.py:144` `app/chamados/management/commands/backfill_fio_morto_e4b.py:192` `app/chamados/models.py:2944` `app/chamados/services/materializacao.py:2165` |
| `pids`, `atos`, `adquirido`, `res` | `ato_do_dia` produz `atos` e `pids` por C19 e pela porta do credor em modo de consulta. O comando grava o plano e substitui os dois na releitura. `_travar_colaboradores` produz `adquirido` e `_fora_da_trava` o consome. `_executar_dia` despacha pelos `atos` e seus IDs. A saída e o resumo leem `res`. **Dois usos de `pids`**: `materializar` contém alvos de ação, `limbo` e `bang` contêm IDs de diagnóstico. A nova união trata todos como carga de trava. Exemplo: uma P de limbo de um dia já vetado pode levar sua D fechada ao `FOR UPDATE` do lote, pois a chamada recebe `plano` inteiro. `app/chamados/management/commands/backfill_fio_morto_e4b.py:82` `app/chamados/management/commands/backfill_fio_morto_e4b.py:126` `app/chamados/management/commands/backfill_fio_morto_e4b.py:200` `app/chamados/management/commands/backfill_fio_morto_e4b.py:233` `app/chamados/management/commands/backfill_fio_morto_e4b.py:314` `app/chamados/management/commands/backfill_fio_morto_e4b.py:338` `app/chamados/management/commands/backfill_fio_morto_e4b.py:366` `app/chamados/management/commands/backfill_fio_morto_e4b.py:439` |
| Dia, contexto, célula, cadastro e proposta de hora | `gravar_dia_do_fato` atribui `P.data_evento`. `vinculos` e `contexto` gravam a ligação de Ch à célula e seu JSON. `dia_do_fato` lê célula e contexto. A edição cadastral grava `empresa_id` e `data_admissao`, e `grade_da_celula` lê admissão e demissão para montar o dia. `_juizo` lê a grade e elege o marco. `nota.anexar_ocorrencia` grava o conteúdo do followup, que `propostas_do_chamado` lê e converte em `hora`, `dia`, `motivo` e `followup_id` para `aplicar_declaracao`. **Dois sentidos existentes de `data_evento`**: dia do turno para fato e dia civil para conversa. `app/chamados/services/fio.py:858` `app/chamados/services/vinculos.py:93` `app/chamados/services/contexto.py:82` `app/chamados/juizes.py:89` `app/colaboradores/views.py:1335` `app/colaboradores/views.py:1365` `app/escala/services/leitor_celula.py:365` `app/chamados/services/declaracao_texto.py:339` `app/chamados/services/nota.py:87` `app/chamados/services/declaracao_texto.py:639` `app/chamados/services/declaracao_texto.py:663` `app/chamados/models.py:3056` `app/chamados/models.py:3067` |
| Resposta, validação, recusa e respectivas mensagens | O core e a resposta automática gravam texto e instante. A via pela trilha carimba o instante de texto existente. Reabertura e ressuscitação limpam o par. `_carimbar_validacao` grava a validação. Correção e aplicação passam `resposta_esperada`; `validar_pergunta` lê o texto relido e usa a leitura composta para a vigência. A coluna `recusa_motivo` já recebe tanto recusa do admin quanto encaminhamento ao admin por `FOLGA_CONTESTA`, além de impedimento do plantio. As cascas leem `mudou_de_cartao`, `ok` e `msg` para responder. `app/chamados/services/disputa_emissao.py:1529` `app/chamados/services/disputa_emissao.py:1558` `app/chamados/services/disputa_emissao.py:1661` `app/chamados/services/disputa_emissao.py:1758` `app/chamados/services/disputa_emissao.py:1793` `app/chamados/models.py:1740` `app/chamados/services/validacao.py:164` `app/chamados/services/validacao.py:743` `app/chamados/services/validacao.py:979` `app/chamados/services/materializacao.py:1795` `app/chamados/services/acoes_disputa.py:98` `app/chamados/services/acoes_disputa.py:452` `app/chamados/services/declaracao_texto.py:729` `app/chamados/views.py:480` |
| Escape, ausência e carimbo de absorção | O core grava o código no mesmo campo que também guarda horas ou texto e lê a tabela de escapes. `criar_ausencia` é a porta de escrita da ausência. A passagem relê dia, texto e decisões do escape, usa C21, carimba a irmã pela porta do modelo e registra `ABSORCAO` com os dois vínculos. `escape_mudou` vai ao evento e protege o destino nessa passagem. **Dois usos existentes de `materializada_em`**: instante do carimbo e parte da chave `(disputa_id, materializada_em)` usada para reencontrar o grupo na rejeição da ausência. `app/chamados/services/disputa_emissao.py:1593` `app/ponto/services/ausencia.py:262` `app/chamados/models.py:3393` `app/chamados/services/materializacao.py:1085` `app/chamados/services/materializacao.py:1103` `app/chamados/services/materializacao.py:1121` `app/chamados/services/materializacao.py:1132` `app/chamados/services/materializacao.py:1169` `app/chamados/services/materializacao.py:1199` |
| Corte, tranca e entrega | O formulário de empresa passa o corte para a porta de cadastro. Fechamento tranca e reabre o período. C20 lê corte, tranca e a trilha do toque. O backfill consulta entrega pela composição de janela e K6. `app/colaboradores/views.py:538` `app/colaboradores/services/estrutura.py:18` `app/ponto/services/fechamento.py:1400` `app/ponto/services/fechamento.py:1432` `app/chamados/juizes.py:1121` `app/chamados/juizes.py:1330` `app/chamados/services/vigia_fio.py:789` |

**`DecisaoPergunta`.** Encontrei 17 sítios de chamada a `registrar`, com dois em `ressuscitar_perguntas`. O `bulk_create` fica em `registrar`. `leitura` ordena a sequência por instante e PK, `composta` entrega os instantes do ato e o texto da projeção, e `absorcao_vigente` lê os vínculos para o core. Chamadas fora dos sítios enumerados ou gravadores externos **[nao verificado]**. `app/chamados/models.py:4320` `app/chamados/models.py:4420` `app/chamados/models.py:4449` `app/chamados/models.py:4469` `app/chamados/models.py:4532`

- Modelo e célula. `app/chamados/models.py:1681` `app/chamados/models.py:1737` `app/chamados/models.py:2152` `app/chamados/models.py:2287` `app/chamados/models.py:3297` `app/ponto/services/veredito_celula.py:200`
- Emissão, validação e materialização. `app/chamados/services/disputa_emissao.py:1565` `app/chamados/services/disputa_emissao.py:1695` `app/chamados/services/disputa_emissao.py:1761` `app/chamados/services/disputa_emissao.py:1795` `app/chamados/services/validacao.py:176` `app/chamados/services/validacao.py:313` `app/chamados/services/validacao.py:390` `app/chamados/services/validacao.py:475` `app/chamados/services/materializacao.py:1125`
- Lotes. `app/chamados/management/commands/backfill_decisoes_pergunta.py:358` `app/chamados/management/commands/backfill_fio_morto_e4b.py:150`

## 8. ACHADO — ordem de trava, guarda de borda e C20 seguem abertos

- **Ordem de trava e levantamento paralelo do universo.** Exemplo concreto: P1 materializável está no plano; sua metade irmã P2 tem destino em Ch9, terminal, ou P1 tem Ch9 em `chamados_relacionados_ids`. A pré-trava só inclui pai e destino das P enumeradas, e o replan repete esse mesmo inventário. O plantio pede os destinos das candidatas à irmã e os clones, inclusive terminais. Portanto Ch9 pode ser adquirido após o primeiro Ch. A sequência pode chegar a **D fechada após Ch** com `pids={'perguntar': []}`: a fábrica pode chamar `perguntar` para um motivo ainda exigível e encontrar uma P já carimbada em D fechada para relavra. Essa porta trava a D mesmo sem um ID no plano. `app/chamados/management/commands/backfill_fio_morto_e4b.py:103` `app/chamados/management/commands/backfill_fio_morto_e4b.py:200` `app/chamados/management/commands/backfill_fio_morto_e4b.py:238` `app/chamados/services/materializacao.py:149` `app/chamados/services/materializacao.py:2168` `app/chamados/services/materializacao.py:2190` `app/chamados/services/fabrica_da_celula.py:168` `app/chamados/services/fabrica_da_celula.py:217` `app/chamados/services/relavra_via.py:230`
- **Segundo seletor da disputa do cartão.** `JUIZES.md` aponta para `fio._disputa_do_chamado`. Na ausência de D-pai, o fio elege pela P de maior PK. O inline elege pela D de maior `aberta_em`, com filtro pelo colaborador. Exemplo: D1 foi aberta antes de D2, mas a P de maior PK apontando para A nasceu em D1. O fio escolhe D1 e o inline escolhe D2. A divergência já existia no inline e a volta 6 a conserva ao extrair o helper. O inline continua delegando C6 e C21 aos juízes registrados. `app/docs/JUIZES.md:43` `app/chamados/services/fio.py:120` `app/chamados/models.py:1975` `app/chamados/views_cobrar.py:144` `app/chamados/views_cobrar.py:227` `app/core/juizes.py:290` `app/core/juizes.py:305`
- **Validação no detalhe ainda pode usar a autorização do cartão antigo.** O detalhe confere P em A e passa só D. Se a porta de costura mover P para B antes de C, a validação ainda encontra o mesmo PK em D, relê a resposta e pode validá-la. Ao contrário dessa entrada, a validação pelo fio relê a listagem sob C. O furo do detalhe já existia e permanece na superfície auditada. `app/chamados/views.py:483` `app/chamados/services/fio.py:993` `app/chamados/services/acoes_disputa.py:21` `app/chamados/services/validacao.py:722` `app/chamados/services/validacao.py:738` `app/chamados/services/validacao.py:775` `app/chamados/services/fio.py:659`
- **Relógio e C20.** A nova enumeração do backfill chama a carga da reabertura sem repassar o `hoje` da borda. A carga chega a `pergunta_viva(_q)` e pode colher `localdate()` no C6. Se o lote cruza a virada da janela viva, essa consulta pode usar outro dia que o do plano. No C20, reproduzi com dublês o caso de 14/10, corte antigo 21 em cache e corte novo 10, com novembro trancado. A mesma instância deu `False` antes da trava e `True` sob ela. Com dois colaboradores da mesma empresa e memo compartilhado, destravado primeiro deu ambos `False`, travado primeiro deu ambos `True`. Além disso, o ramo relê a Empresa pelo `empresa_id` recebido. Nas órfãs, o mapa é semeado com `colab` da borda, de modo que uma troca de empresa antes de C ainda pode levar o juiz à empresa anterior. O pré-requisito de pureza segue aberto. Pouso ou execução da suíte **[nao verificado]**. `app/chamados/management/commands/backfill_fio_morto_e4b.py:206` `app/chamados/management/commands/backfill_fio_morto_e4b.py:275` `app/chamados/services/disputa_emissao.py:1917` `app/chamados/models.py:1599` `app/chamados/juizes.py:642` `app/chamados/juizes.py:1564` `app/chamados/juizes.py:1091` `app/chamados/juizes.py:1112` `app/chamados/reconciliador.py:1238` `app/colaboradores/views.py:1335`
- **Escritor, exceção e corte 7.a.** Nenhum novo escritor direto de `DecisaoPergunta` nos trechos alterados. `escape_mudou` usa `_Reprovada` e o `except` que já desfazia a tentativa da irmã. A criação da ausência e a leitura do relógio antes de C continuam no caminho preexistente. O C6 continua sendo consultado antes da lavra e do registro de `TOQUE_RESOLVER_DIA`, conforme 7.a. `app/chamados/models.py:4532` `app/chamados/services/materializacao.py:1037` `app/chamados/services/materializacao.py:1046` `app/chamados/services/materializacao.py:1123` `app/chamados/services/materializacao.py:1132` `app/ponto/services/veredito_celula.py:193` `app/ponto/services/veredito_celula.py:352`

**Mordida dos REDs, por inspeção.** O I1 de correção intermedeia **antes** da leitura da porta, então remover só a comparação sob C deixa esse teste passar pela recusa barata. O teste da volta 5 cobre a troca na tomada da trava. O I1 de recusa tem oráculo de zero `RECUSA`. O I2 cobre texto trocado na correção, com controle positivo; retirar só a passagem de `resposta_esperada` de `aplicar_declaracao` fica fora desses casos. O I3 cobre seletor virando `None`. O I4 isola o cadastro entregue ao juízo por mock. O I5 com `FALTA_DIA` cobre a comparação de tipo, mas deixa sobreviver a retirada isolada de C21, pois a primeira condição já recusa. O I6 só testa a releitura positiva. O I7 detecta aquisições novas do par da fixture, admite repetir `FOR UPDATE` de D depois de Ch e sequer despacha a materialização real no teste. O I8 chama o core com só C adquirido e mede se a trava de Ch precede o `UPDATE` de P. Resultados dos mutantes com ORM **[nao verificado]**. `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:67` `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:161` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:93` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:149` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:170` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:195` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:230` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:263` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:308` `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:344`

## 9. NADA — o mutante de I3 é equivalente no universo de escritores encontrado

O censo estático encontrou a atribuição da FK P → D no nascimento. Reuso pode devolver a linha existente, costura persiste o destino e lavra em lote atualiza só os campos da lavra. A migração de identidade lê `disputa_id` para preencher `colaborador_id`. Não encontrei reatribuição persistida dessa FK em P existente nos caminhos da cópia examinados. `app/chamados/models.py:2688` `app/chamados/models.py:3286` `app/chamados/models.py:3292` `app/chamados/services/fio.py:993` `app/chamados/veredito_lavrado.py:237` `app/chamados/veredito_lavrado.py:250` `app/chamados/migrations/0051_decisao_pergunta.py:25`

Se o seletor passa de D1 para D2, `_vivas_de(D2)` consulta o reverso da FK de D2, de modo que o PK de P1, que veio de D1, falha na condição seguinte mesmo sem `_d.pk != d.pk`. O `save` genérico aceita `update_fields` e o modelo não impõe imutabilidade dessa FK. A conclusão vale para o censo da cópia. Escritas externas **[nao verificado]**. `app/chamados/views_cobrar.py:177` `app/chamados/views_cobrar.py:228` `app/chamados/juizes.py:1598` `app/chamados/models.py:3782`

## 10. NADA — regra nova de negócio, dinheiro, célula ou folha

As mudanças de desfecho identificadas no diff são recusas de guarda ou de conjunto de travas. A validação segue chamando o recálculo existente quando passa, o backfill segue pelas portas já existentes e a absorção usa a tabela de escapes vigente. A regra do instante compensatório e o fallback da janela continuam nos mesmos sítios. Há uma consequência operacional verificável: um replan recusado pode deixar o lote com `FIM E4B rc=0`, pois essa recusa preenche `res` e o código de saída usa `erros`. Impacto efetivo em valores, células, folha ou produção **[nao verificado]**. `app/chamados/services/validacao.py:747` `app/chamados/services/validacao.py:784` `app/chamados/management/commands/backfill_fio_morto_e4b.py:126` `app/chamados/management/commands/backfill_fio_morto_e4b.py:373` `app/chamados/management/commands/backfill_fio_morto_e4b.py:482` `app/chamados/services/materializacao.py:1169` `app/chamados/models.py:4503` `app/chamados/juizes.py:828`

## 23/11 15:06 · sessao e39cfdfa3251 · E4C-F5 · verificador · origem=Code

## 1. ACHADO — a volta 6 fecha parte da triagem

Usei a cópia local e conferi os 20 blobs pelos cabeçalhos `index` do `v6.diff` anexo. Hash do commit via Git, suíte ORM e ocorrência em produção [nao verificado]. Os contraexemplos abaixo são leituras do código. [nao verificado]

| Item e porta | Trava e guardas | Veredito |
|---|---|---|
| **1 — corrigir** | Detalhe e fio passam o PK do cartão (`chamados/views.py:523–536`, `chamados/services/fio.py:689–703`). A porta trava C/D/Ch, relê P e confere cartão, validação vigente, D aberta e tipo `hora` (`chamados/services/acoes_disputa.py:388–402,416–428`). Formato e motivo vêm do POST. O core reconfere formato e opções na P relida (`chamados/services/acoes_disputa.py:376–381`, `chamados/services/disputa_emissao.py:1490–1505`). | **NADA** para a `RESPOSTA` inicial. **ACHADO** na `VALIDACAO` seguinte: a correção pode gravar `08:41` no cartão A, P pode mudar para B dentro da mesma D, e o segundo ato ainda valida `08:41`. Ele recebe D e texto esperado, sem o cartão (`chamados/services/acoes_disputa.py:440–453`, `chamados/services/validacao.py:722–750`, `chamados/services/fio.py:993–996`). |
| **1 — recusar** | Detalhe e fio exigem D aberta e passam o cartão (`chamados/views.py:505–518`, `chamados/services/fio.py:696–702`). Sob C, a porta relê cartão, validação vigente, resposta completa e se P saiu do fato. O motivo é do POST (`chamados/services/validacao.py:922–925,936–954,962–984`). | **ACHADO.** O reapontamento passa a ser barrado. A abertura de D continua só na borda. Exemplo: D de uma P respondida e sem validação pode fechar porque o dia está trancado, mantendo a resposta. Se fechar entre a borda e C, o cartão continua igual e a porta grava `RECUSA` (`chamados/models.py:2041–2045,2138–2164`, `chamados/juizes.py:1494–1503`, `chamados/services/validacao.py:967–984`). |
| **2 — validar após corrigir ou aplicar declaração** | Corrigir passa a hora gravada. Aplicar relê sob a trava a proposta com o mesmo followup, motivo, dia e hora. Passa a hora gravada ou o texto da resposta reaproveitada (`chamados/services/acoes_disputa.py:452–453`, `chamados/services/declaracao_texto.py:685–704,723–739`). O plantio trava C/D/Ch/P e reconfere o escopo da principal e das irmãs. Após `refresh`, o validador verifica resposta completa, texto esperado, validação vigente, espera do par, implausibilidade, ramo de folga e entrega (`chamados/services/materializacao.py:2179–2209`, `chamados/services/validacao.py:738–775,808–812`). | **NADA** para a troca de texto descrita. `08:41` → `07:41` recusa antes de `VALIDACAO`. Uma resposta apagada cai primeiro em `sem_resposta` (`chamados/services/validacao.py:744–750`). Há outras portas de `VALIDACAO` abaixo. |
| **3 — validar inline** | C em `chamados/views_cobrar.py:223`. Sob C, o seletor busca D aberta do próprio cartão ou D aberta com P destinada a ele, exige a mesma D e reconfere P viva e respondida (`chamados/views_cobrar.py:144–153,227–231`). | **NADA** para a única P que ligava D a A ser reapontada a B. **ACHADO** na leitura literal de todas as guardas: a consulta de área e `tem_acao` ficam antes de C, e o seletor recebe o `ch` carregado na entrada (`chamados/views_cobrar.py:161–172,223–231`). |
| **4 — declarar hora do colaborador** | `_trava_do_ato` trava C e depois o universo D/Ch/P. Sob a trava, a porta relê Ch com C/Empresa, relê o dia e compara o casamento dos marcos. Também verifica a candidata reutilizável, validação e resposta idêntica antes de chamar o core (`chamados/services/declaracao_texto.py:290–305,363–406,445–447`). | **NADA** para a admissão corrigida para D+1. O leitor da grade recebe o colaborador fresco (`escala/services/leitor_celula.py:365,385–395`). A guarda de módulo de furo continua só na entrada (`chamados/services/declaracao_texto.py:322–323,369–380`). Uma troca desse módulo por porta normal durante o ato [nao verificado]. |
| **5 — absorver irmãs** | C por irmã. Sob C, relê irmã, grupo, dia do escape, resposta posterior à passagem e carimbo prévio. O carimbo passa pelo C6 e pela paridade. Antes de `ABSORCAO`, verifica grupo recuperável, tipo do escape e resposta completa (`chamados/services/materializacao.py:1074–1128`, `chamados/models.py:3416–3455`). | **ACHADO.** Trocar `ATESTADO` por `08:00` barra a absorção. A mesma proteção deixa passar uma `Ausencia` já rejeitada, como em **3** abaixo (`chamados/services/materializacao.py:1115–1128`). |
| **7 — backfill** | C no topo, seguido de aquisição em lote de D/Ch. Sob a trava, relê entrega, plano, limbo e o conjunto D/Ch calculado (`chamados/management/commands/backfill_fio_morto_e4b.py:217–246,338,351–379`). | **ACHADO.** A execução pede Ch que o conjunto do replan omite. Contraexemplo em **2** abaixo (`chamados/services/materializacao.py:2165–2195`). |
| **8 — core sem a casca** | O core relê P sob C e trava Ch destino/pai antes do `UPDATE` de P. Depois confere tipo/opções, veto de absorção e C6 quando exigidos (`chamados/services/disputa_emissao.py:1487–1520,1562–1567`). | **NADA** para a lacuna do selo. O RED chama o core direto num `atomic` com só C e exige resgate efetivo, `FOR UPDATE` de Ch e `UPDATE` de P nessa ordem (`chamados/tests/test_f5_v6_guarda_sob_trava.py:344–368`). Execução do RED e do mutante [nao verificado]. |

**Portas de `VALIDACAO` que ficaram fora.**

- **Detalhe individual.** A borda verifica D aberta e cartão A. Se P muda para B na mesma D antes da leitura do validador, a validação ainda passa pelo cartão A. A casca transmite só D, e o validador relê P sem reconferir a listagem. O fio individual já reconfere cartão, dono e estado de D sob C (`chamados/views.py:483–501`, `chamados/services/acoes_disputa.py:20–24`, `chamados/services/validacao.py:722–775`, `chamados/services/fio.py:663–673,993–996`).
- **Upload de foto.** Relê o episódio nos dois atos. Se gravou a resposta automática e, entre os atos, o colaborador gravou `Não estava`, a segunda porta pode validar esse texto. A associação à foto continua válida e a chamada omite o texto esperado (`api/foto_recebida.py:64–95`, `chamados/catalogo/perguntas.py:93–98`, `chamados/services/validacao.py:741–775`).
- **CLI `validar_classe_a --apply`.** Confere classe e par ato/coluna antes de C. Uma resposta `Confirma` pode mudar para `Não estava` depois da seleção. A classe passa de A para B, e a validação canônica admite a opção sem teste de implausibilidade. O lote web reclassifica sob C (`chamados/management/commands/validar_classe_a.py:60–70,100–104`, `chamados/juizes.py:2131–2159`, `chamados/services/validacao.py:1140–1149`).

**Corte 7.a e controle verde.** No caminho do veredito, C6 continua julgando antes da lavra e da trilha do toque, conforme **MANTER** (`ponto/services/veredito_celula.py:192–202,351–363`). O ramo de C20 permanece (`chamados/juizes.py:1082–1118`). A fixture das órfãs agora herda o `setUp`. Executei a varredura AST do selo em memória: zero ocorrências em 2.460 fontes de teste (`chamados/tests/test_f5_v5_guarda_sob_trava.py:385–399`, `core/tests/test_selo_freeze_setup_nao_emprestado.py:47–116`).

## 2. ACHADO — o backfill ainda pode adquirir Ch depois de P

As novas travas explícitas passam por `PerguntaDisputa.trava_na_ordem`. O topo recebe o plano inteiro. Primeiro trava C por PK, depois relê a carga e faz uma chamada para D explícitas, inclusive fechadas, mais D abertas, seguida de Ch explícitos, pais das abertas e Ch vivos. A carga explícita inclui credor e perguntas que ele relavra ou ressuscita. A segunda chamada repete C e ainda não trava P (`chamados/management/commands/backfill_fio_morto_e4b.py:192–235,319–338`, `chamados/services/disputa_emissao.py:1911–1921`, `chamados/models.py:2938–2967`).

**Contraexemplo.** Um dia tem P1 em `registrar_decisao` e P2 em `materializar`. P2 tem em `chamados_relacionados_ids` um Ch terminal que não é pai, destino nem credor da carga. `_fora_da_trava` aceita, pois repete a enumeração que omite esse campo. A execução trava P1 e, no plantio de P2, adquire o Ch terminal. A ordem efetiva é C→D→Ch→P→Ch. O plantio também inclui candidatas a irmã, e a materialização percorre todas as P prontas da D. O RED I7 faz uma chamada simulada de D/Ch e só acusa PKs novas, portanto permite `FOR UPDATE` reentrante de D após o primeiro Ch (`chamados/management/commands/backfill_fio_morto_e4b.py:98–114,147–156,200–214,238–246`, `chamados/services/materializacao.py:2165–2195,2248–2262`, `chamados/tests/test_f5_v6_guarda_sob_trava.py:274–285,308–321`).

**Recusa e gravação parcial.** `replan_fora_da_trava` sai antes de `_executar_dia`, desfaz o savepoint e retira os callbacks daquele dia. Em `--aplicar`, outros dias aceitos podem persistir e alcançar P do mesmo fio, porque a materialização percorre a D e a cerca delimita a competência. Já uma `recusa:` devolvida pelo credor durante a execução entra em `nao_executado` e, por si só, permite os atos seguintes e o commit do dia (`chamados/management/commands/backfill_fio_morto_e4b.py:139–156,351–408,417–436`, `chamados/services/foto_e4b.py:145–163`).

## 3. ACHADO — o status efetivo da ausência pode mudar

**Status declarado pelo escape.** Cada tipo tem um único código na tabela: `FALTA_DIA` dá `('falta', 'aprovada')` e `ATESTADO` dá `('atestado', '')`. O chamador de produção passa o par dessa tabela. Comparar o tipo detecta qualquer troca entre esses escapes. O vazio de `ATESTADO` delega o estado inicial ao juiz (`chamados/services/materializacao.py:1166–1172`, `chamados/services/disputa_emissao.py:1593–1596`, `ponto/services/ausencia.py:475–486,926–944`).

**Contraexemplo com o estado efetivo.** Uma P de hora responde `ATESTADO`. A ausência nasce aguardando documento. O DP a rejeita entre `_criar_ausencia` e a primeira trava C da passagem. O `on_commit` da rejeição pode rodar antes de existir qualquer carimbo do grupo. A passagem ainda tem a instância antiga da ausência. A própria P de hora continua candidata e âncora, com resposta completa. O carimbo pode validá-la, a busca do grupo a encontra pelo texto e instante, e `ABSORCAO` fica ligada à ausência rejeitada. Isso contraria a regra de devolver as absorvidas quando a ausência é rejeitada (`chamados/catalogo/perguntas.py:47–61`, `ponto/views.py:2854–2876`, `ponto/signals.py:250–272`, `chamados/services/materializacao.py:1037–1045,1048–1054,1074–1128,1192–1208,1211–1239`, `chamados/models.py:3428–3455,4385–4389`).

**Precedência preservada.** `sem_grupo_recuperavel` é decidido antes de `escape_mudou`. Se ambas as condições falham, sai o motivo antigo. `_Reprovada` desfaz o carimbo no `atomic` da irmã e a trilha registra o motivo escolhido (`chamados/services/materializacao.py:1074,1114–1145`, `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:251–264`).

## 4. ACHADO — o detalhe HTMX reapresenta a pergunta que mudou de cartão

| Leitor | Efeito dos novos valores |
|---|---|
| **Recusa no detalhe e no fio/modal** | A casca tem listas próprias e inclui `mudou_de_cartao` no ramo `info`. O fallback dela é `sucesso`. O fio/modal remonta a listagem pelo cartão atual. No detalhe HTMX, A recebe um fragmento buscado só pelo PK de P, com `pode_agir=True`. Se P foi para B, A pode voltar a mostrar o texto, a resposta e os controles de P. O próximo POST volta a passar pela guarda de listagem (`chamados/services/acoes_disputa.py:95–102`, `chamados/services/fio.py:260–280,486–516,543–547`, `chamados/views.py:394–408,499–518`, `templates/chamados/partials/_pergunta_card.html:20–42,100–125`). |
| **Correção e aplicar declaração** | `resposta_mudou` vira `erro` com a mensagem do serviço. Na correção, a resposta inicial e a trilha já foram gravadas. Aplicar repassa `estado`, `ok` e `msg`, e o detalhe lê `ok` e `msg`. O botão de aplicar no modal posta uma ação sem ramo no roteador e cai em `Nada a fazer` (`chamados/services/acoes_disputa.py:440–463`, `chamados/services/declaracao_texto.py:738–740`, `chamados/views.py:477–481,951–954`, `templates/chamados/partials/modal_fio.html:137–143`, `chamados/services/fio.py:607–608,775–810`). |
| **Casca de validar, inline, lote, CLI e foto** | A casca tem traduções específicas e fallback por `ok`. Inline agrega `sucesso`/`aviso`, lote web conta a validação composta, CLI conta chaves dinamicamente e foto lê `ok`. Essas chamadas não pedem `resposta_esperada`, então não recebem `resposta_mudou` (`chamados/services/acoes_disputa.py:20–66`, `chamados/views_cobrar.py:212–237`, `chamados/services/validacao.py:1149–1167`, `chamados/management/commands/validar_classe_a.py:100–107`, `api/foto_recebida.py:89–96`). |
| **Estado exibido, contadores, placar, filtros e export** | Os códigos são retornos das portas. O estado exibido vem da leitura composta e do C6. Painel, filtros e placares usam fila, status de Ch, verbos, carimbos e atores. O TXT lê o veredito de exportação e os fechamentos mensais. Nenhum dos dois códigos vira categoria nesses leitores (`chamados/models.py:3312–3331`, `chamados/services/painel.py:198–232,256–279,300–323`, `chamados/services/verbos.py:253–271,325–352`, `folha/export.py:711–731,765–788`). |

O leitor localizado da tupla `validacao.ESTADOS` é a catraca AST de teste. Ela exige que os estados devolvidos estejam na tupla, sem testar a tradução nas telas (`chamados/services/validacao.py:57–79`, `chamados/tests/test_porta_validacao_pergunta.py:140–165`).

## 5. NADA — o censo AST não encontrou chamada sem `cartao_esperado`

As duas assinaturas exigem o argumento por nome. Na varredura de 3.802 fontes Python, encontrei 13 chamadas explícitas e duas chamadas indiretas de teste. Os dois wrappers `real(*a, **kw)` recebem o argumento encaminhado pelo fio (`chamados/services/acoes_disputa.py:77,341`, `chamados/services/fio.py:702–703`, `chamados/tests/test_f5_v6_guarda_sob_trava.py:72–85,98–111`).

| Chamadores | Sítios |
|---|---|
| **Produção — corrigir e recusar** | Detalhe com import direto em `chamados/views.py:518,536`. Fio com alias de módulo `_ad` em `chamados/services/fio.py:702,703`. |
| **Testes — corrigir** | `chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:580`, `chamados/tests/test_e4c_f5_validacao_plantio.py:220`, `chamados/tests/test_e4c_f5_voltaD_portas.py:91`, `chamados/tests/test_f5_v4_guarda_sob_trava.py:106`, `chamados/tests/test_f5_v5_guarda_sob_trava.py:147`, `chamados/tests/test_f5_v6_guarda_sob_trava.py:78,140`. |
| **Testes — recusar** | `chamados/tests/test_e3_porta_A_reabrir.py:93`, `chamados/tests/test_f5_v6_guarda_sob_trava.py:104`, `chamados/tests/test_selo_porta_recusa.py:82,113`. |

## 23/11 15:06 · sessao 6f7b36b52867 · - · - · origem=Ronald (tmux)

- **Item 1 — ACHADO.** Corrigir confirma cartão, disputa aberta, não validada e tipo hora após a trava. Recusar confirma cartão, resposta composta e situação do fato, mas deixa a disputa aberta na borda. Uma pergunta respondida sem validação em competência trancada permite fechar a disputa entre a borda e C, mantendo o cartão e deixando a recusa gravar. `app/chamados/services/acoes_disputa.py:388-398,421-440`, `app/chamados/views.py:510-518`, `app/chamados/services/validacao.py:936-954,962-984`, `app/chamados/models.py:2041-2045,2138-2164`, `app/chamados/juizes.py:1494-1503`.
- **Item 2 — NADA nas duas portas alteradas, ACHADO nas paralelas.** Correção passa a hora. Aplicação relê a proposta sob C e passa a hora gravada ou o texto reaproveitado. O validador confirma esse texto depois da trava do plantio. O detalhe comum ainda autoriza pelo cartão e pela disputa aberta só na borda. Reapontar A→B sem mudar D antes da primeira leitura do validador escapa. O comando de classe A seleciona `Confirma`, mas pode assinar `Não estava` se a resposta mudar antes da trava, porque não reclassifica nem passa texto esperado. `app/chamados/services/acoes_disputa.py:452-453`, `app/chamados/services/declaracao_texto.py:685-692,723-740`, `app/chamados/services/validacao.py:722-750`, `app/chamados/views.py:483-501`, `app/chamados/services/fio.py:993-996`, `app/chamados/management/commands/validar_classe_a.py:63-70,100-104`, `app/chamados/models.py:4014-4018`, `app/chamados/juizes.py:2138-2144`.
- **Item 3 — NADA no seletor D/cartão.** Sob C relê a D aberta pelo pai ou destino, exige a mesma PK e verifica P viva e respondida. Permissão contextual continua antes de C e o fallback usa o colaborador do cartão carregado na borda. `app/chamados/views_cobrar.py:144-153,161-172,223-231`.
- **Item 4 — NADA para a admissão.** C em `declaracao_texto.py:363` via `:301-305`. Dia, colaborador/empresa e casamento após C em `:369-380`. Canônica e validação/resposta verificadas em `:388-406,445-447`. A admissão D+1 tira D do vínculo quando não houve fato. `app/escala/services/leitor_celula.py:365,385-395`. A condição de módulo aparece só em `app/chamados/services/declaracao_texto.py:322-323`. Escritor de mudança desse campo em chamado existente [nao verificado].
- **Item 5 — ACHADO no status efetivo.** A tabela não contém dois escapes do mesmo tipo com status diferente. Um atestado pode passar a `rejeitada` entre criação e C. Seu callback ainda não acha absorção a devolver. Uma pergunta de hora com `ATESTADO` é a própria âncora. O carimbo aplica validação e o C6 a dá por morta. O grupo recuperável e a nova guarda aceitam o texto mesmo com a Ausência rejeitada. A prioridade de `sem_grupo_recuperavel` foi preservada. `app/chamados/services/materializacao.py:1037-1048,1075-1085,1103-1128,1169-1172,1192-1208,1228-1239`, `app/ponto/services/ausencia.py:914-917,1057-1088`, `app/ponto/signals.py:250-272`, `app/chamados/models.py:3428-3439,4385-4389`, `app/chamados/juizes.py:1496-1499`.
- **Item 7 — ACHADO.** Um clone terminal em `chamados_relacionados_ids` fica fora de ambas as fotos do backfill e entra no `FOR UPDATE` posterior do plantio. Com `registrar_decisao` antes de `materializar` no mesmo dia, é Ch novo depois de P, além de depois da primeira aquisição de Ch. A recusa de `_fora_da_trava` é anterior a `_executar_dia` e reverte o savepoint daquele dia. `app/chamados/management/commands/backfill_fio_morto_e4b.py:147-156,200-214,217-246,351-379`, `app/chamados/services/materializacao.py:2165-2195,2248-2262`, `app/chamados/services/disputa_emissao.py:837-856`.
- **Item 8 — NADA na cobertura estática pedida.** Teste entra pelo core reexportado dentro de atomic com só C, verifica resgate e espiona Ch antes de `UPDATE P`. A trava que deve morder é `disputa_emissao.py:1500`. Sem ela, o resgate chega depois do `save` e sua própria trava vem tarde. `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:344-368`, `app/chamados/services/disputa_emissao.py:1500-1503,1562,1592,1624-1625`, `app/chamados/services/materializacao.py:1301-1312`, `app/chamados/services/arquivo.py:209-210`. Mutante no ORM e suíte [nao verificado].
- **CONTROLE-VERDE.** AST do próprio selo executado em memória sobre 2.460 arquivos retornou vazio. Um teste sintético com `A.setUp(self)` por meio de uma subclasse `A` que herda `setUp` de classe congelada também retornou vazio. A correção específica herda o `tearDown`. `app/core/tests/test_selo_freeze_setup_nao_emprestado.py:49-100`, `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:385-400`.

## 23/11 15:04 · sessao 05627d3ff906 · - · - · origem=Ronald (tmux)

**Q2 / item 7 — ACHADO.** A aquisição de topo cobre as D fechadas dos PIDs diretos e da carga do credor, o credor, os pais e destinos dessas perguntas. A chamada recebe o `plano` inteiro, inclusive dias excluídos de `executaveis`. Primeiro trava C em ordem de PK e lê a carga. A segunda chamada repete C e adquire numa consulta por classe D explícitas mais abertas e Ch explícitos mais pais das abertas e chamados vivos. Não há P nessa aquisição. Todas as travas explícitas adicionadas passam por `trava_na_ordem`. [backfill_fio_morto_e4b.py:192](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:192), [backfill_fio_morto_e4b.py:338](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:338), [models.py:2938](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/models.py:2938)

O conjunto do replan deixa passar Ch fora da trava inicial. Exemplo estático: o dia contém um P de `registrar_decisao` e outro de `materializar`. Este tem em `chamados_relacionados_ids` um Ch terminal que não é pai, destino ou credor de qualquer P da carga. O replan aceita, o primeiro ato trava P e o plantio pede então o Ch terminal, invertendo P→Ch. O plantio também inclui as candidatas à irmã e a materialização itera todas as P prontas da D, inclusive as de outros dias que ficaram fora dos PIDs. [backfill_fio_morto_e4b.py:98](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:98), [backfill_fio_morto_e4b.py:147](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:147), [backfill_fio_morto_e4b.py:200](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:200), [materializacao.py:2165](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/services/materializacao.py:2165), [materializacao.py:2248](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/services/materializacao.py:2248)

O RED I7 chama a trava diretamente para D e Ch já incluídos e rejeita somente PKs novas reconhecidas pelo regex. O próprio teste emite SQL reentrante de D após o primeiro Ch e não percorre a execução real. [test_f5_v6_guarda_sob_trava.py:274](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/tests/test_f5_v6_guarda_sob_trava.py:274), [test_f5_v6_guarda_sob_trava.py:308](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/tests/test_f5_v6_guarda_sob_trava.py:308), [models.py:2944](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/models.py:2944)

`replan_fora_da_trava` recusa antes de `_executar_dia`, desfaz o savepoint do dia e retira callbacks da tentativa. No apply, outros dias aceitos podem persistir. A cerca testa a competência e a materialização da D abrange outras datas, portanto efeitos de cascata de dia aceito não são revertidos pela recusa do próximo. A recusa devolvida pela porta do credor dentro da execução é registrada em `nao_executado` e sozinha não impede os atos seguintes nem faz rollback do dia. [backfill_fio_morto_e4b.py:139](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:139), [backfill_fio_morto_e4b.py:351](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:351), [backfill_fio_morto_e4b.py:373](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:373), [backfill_fio_morto_e4b.py:398](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:398), [backfill_fio_morto_e4b.py:417](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/management/commands/backfill_fio_morto_e4b.py:417), [foto_e4b.py:145](/home/ronald/codex_ws/7bd3677008acd93a118c20b458b11cc9af2f1e1a/app/chamados/services/foto_e4b.py:145)

## 23/11 15:04 · sessao 363b72af1eae · - · - · origem=Ronald (tmux)

**4. ACHADO.** O detalhe HTMX mostra `mudou_de_cartao` como `info`, mas busca novamente a pergunta só pelo PK e renderiza o fragmento com `pode_agir=True`. Se a pergunta sai de A para B, A continua exibindo a pergunta já reapontada e pode mostrar os botões. A borda verifica a listagem novamente no próximo POST. O fio/modal recompõe a listagem corrente e o mapa de permissões por cartão. `app/chamados/services/acoes_disputa.py:95`, `app/chamados/views.py:394`, `app/chamados/views.py:499`, `app/chamados/views.py:516`, `app/templates/chamados/partials/_pergunta_card.html:20`, `app/templates/chamados/partials/_pergunta_card.html:42`, `app/chamados/services/fio.py:276`, `app/chamados/services/fio.py:486`, `app/chamados/services/fio.py:543`.

- `resposta_mudou` vira erro com a mensagem do serviço na correção, depois da `RESPOSTA` inicial e de sua trilha. `aplicar_declaracao` repassa estado, `ok` e mensagem. O detalhe lê `ok` e mensagem. O botão de aplicar no modal tem uma lacuna anterior ao diff. O formulário envia a ação ao modal, que a encaminha a um roteador sem esse ramo e recebe `Nada a fazer`. `app/chamados/services/acoes_disputa.py:440`, `app/chamados/services/acoes_disputa.py:452`, `app/chamados/services/acoes_disputa.py:461`, `app/chamados/services/declaracao_texto.py:738`, `app/chamados/views.py:477`, `app/templates/chamados/partials/modal_fio.html:137`, `app/chamados/views.py:951`, `app/chamados/services/fio.py:607`, `app/chamados/services/fio.py:809`.
- A casca de recusa tem lista própria, atualizada para `mudou_de_cartao`. Seu fallback é sucesso. A casca de validação tem traduções específicas e fallback por `ok`. A CLI conta chaves dinamicamente e a foto lê `ok`. Inline agrega `tipo` e lote web consulta a validação composta. Esses caminhos de validação não enviam `resposta_esperada`. O leitor localizado da tupla `ESTADOS` é a catraca AST de teste. `app/chamados/services/acoes_disputa.py:21`, `app/chamados/services/acoes_disputa.py:33`, `app/chamados/services/acoes_disputa.py:60`, `app/chamados/services/acoes_disputa.py:95`, `app/chamados/management/commands/validar_classe_a.py:100`, `app/api/foto_recebida.py:89`, `app/chamados/views_cobrar.py:212`, `app/chamados/services/validacao.py:1149`, `app/chamados/tests/test_porta_validacao_pergunta.py:140`.
- O estado exibido da pergunta vem da leitura composta e do C6. As listas fechadas `ap.estado` nos templates vêm da testemunha de aplicação. Contadores, filtros e placares usam fila, status de chamados, carimbos/atores e o par da resposta. O TXT usa o veredito de exportação e os fechamentos mensais. Os novos valores de retorno não entram nessas dimensões. `app/chamados/models.py:3312`, `app/chamados/templatetags/chamados_tags.py:131`, `app/templates/chamados/partials/_pergunta_card.html:43`, `app/chamados/services/painel.py:198`, `app/chamados/services/painel.py:256`, `app/chamados/services/painel.py:300`, `app/chamados/services/verbos.py:253`, `app/chamados/services/verbos.py:325`, `app/folha/export.py:711`.

## 23/11 14:25 · sessao c62153d77d08 · PAR-PELO-TURNO · censo · origem=Code

# PAR-PELO-TURNO · Censo da cópia

O censo usa o código da cópia e o recorte raspado do enunciado. A reprodução foi isolada, com as duas batidas informadas por noite e a escala fornecida. A correspondência da cópia com o SHA, os insumos completos de produção e os valores lavrados ficam **[nao verificado]**.

## 1. O que o pareador devolve hoje

O fechamento principal lê `hi/hf` do mapa pela **data local da entrada**. Com duas bordas distintas, calcula `(hf − hi) módulo 24 h + 4 h`. Sem essas bordas, usa 26 h. A L-084 também faz voltar a 26 h se **as duas** pontas estiverem a **mais de 180 min** dos respectivos marcos. A separação ocorre quando a duração real **excede** o teto escolhido. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:326`, `app/ponto/turnos.py:1430`, `app/ponto/motor_calculo_v2.py:1931`.

| Caso | Resultado e ramo |
|---|---|
| **col129 · 15→16/11** | **Dois objetos `Turno`, zero pares fechados.** Em 15/11, `E 17:30` fica aberta e sem saída. Em 16/11, `S 06:30` fica órfã e sem entrada. A escala 21:30–05:42 dá **492 + 240 = 732 min**. O vão real é **780 min** e as distâncias são **240 min na E e 48 min na S**. A S ancora a jornada, portanto a exceção das duas pontas distantes fica inativa. O teto de 732 min separa, carimba a S e impede o pós-fechamento. O teste de outubro usa as mesmas horas e fixa esse resultado. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:1084`, `app/ponto/turnos.py:1440`, `app/ponto/turnos.py:1451`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:52`. |
| **col155 · 15→16/11** | Com a mesma escala no mapa, **dois objetos `Turno`, zero pares fechados**. `E 17:29` fica aberta em 15/11 e `S 06:32` fica órfã em 16/11. São **783 min** contra **732 min**, com distâncias **241 e 50 min**. A reprodução isolada retornou o mesmo ramo e o mesmo carimbo. Ata, marcas adicionais e eventual segmentação real por vínculo **[nao verificado]**. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1451`, `app/ponto/turnos.py:1962`. |
| **col214 · 15/11** | O recorte dá a **órfã 07:00**. Tipo efetivo, par candidato, bordas previstas, quantidade de turnos e ramo exato **[nao verificado]**. Uma S sem abertura já vira `Turno` sem entrada. A grade também pode publicar sobra por falta de atribuição a um marco. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:1495`, `app/escala/utils.py:635`, `app/escala/utils.py:1822`. |
| **col107 · 15/11** | O recorte dá **um par conhecido, 10:13–14:25**, e as órfãs **07:05 e 09:15**. Elas antecedem a entrada real em **188 e 58 min**. A distância ao marco **previsto**, os tipos e papéis das órfãs, o total de objetos `Turno` e o ramo que as deixou de fora ficam **[nao verificado]**. A reconciliação existente busca as próprias bordas de um par fechado quando elas aparecem como órfãs da grade. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:1417`, `app/escala/utils.py:1956`, `app/escala/utils.py:1986`, `app/escala/utils.py:2010`. |

Na reprodução das duas noites, passar só os marcos do template, sem `marcos_por_dia`, produziu **um par fechado em cada caso** pelo envelope de 26 h. Essa diferença de alimentação importa para o censo de leitores. `app/ponto/turnos.py:1192`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1492`.

## 2. Adicional noturno e prorrogação após 05:00

**A conta está em `motor_calculo_v2.py`, na zona que exige aval explícito. O par fechado já chega à conta pelo caminho existente.** O encadeamento de produção é o seguinte. `CLAUDE.md:143`, `app/ponto/services/fechamento.py:286`, `app/ponto/services/fechamento.py:735`.

1. `calcular_mes` → `montar_periodos` da classe do motor → `_periodos_via_autoridade` → `turnos_via_autoridade` → `parear_turnos`. O motor alimenta o pareador com os marcos disponíveis, os parâmetros da classe e o papel da ata quando carregado. `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:494`, `app/ponto/motor_calculo_v2.py:509`, `app/ponto/motor_calculo_v2.py:2354`, `app/ponto/motor_calculo_v2.py:2748`, `app/ponto/motor_calculo_v2.py:2875`, `app/ponto/motor_calculo_v2.py:3213`, `app/ponto/motor_calculo_v2.py:3532`.
2. `adaptador_motor.periodos_de_turnos` devolve os timestamps e as pausas julgadas por `intervalos_do_turno`. A **S órfã só gera anomalia**. A **E aberta vira `(entrada, None)`**. Em seguida, `_periodos_pelo_marco` pode substituir os períodos do dia pelo par de `juiz_batida.periodos_do_dia`, se a ata estiver alinhada, com as duas bordas, correspondência com as batidas e cobertura do miolo. Ata aberta ou desalinhada deixa o resultado geométrico seguir. `app/ponto/adaptador_motor.py:24`, `app/ponto/adaptador_motor.py:29`, `app/ponto/motor_calculo_v2.py:530`, `app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:632`, `app/ponto/motor_calculo_v2.py:669`, `app/ponto/motor_calculo_v2.py:685`.
3. `calcular_periodo` retorna antes da conta quando falta S. Para um par fechado, apura sobre as **bordas efetivas após a janela de HE**, com as pausas recortadas. `_escrever_noturnas` chama `calcular_noturnas`, que usa `minutos_noturnos`, desconta o noturno das pausas e aplica o fator legal ou convencional. `minutos_noturnos` cruza o período com **22:00–05:00** e, com a flag ativa e interseção no segmento **00:00–05:00**, estende esse segmento até a saída efetiva. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:969`, `app/ponto/motor_calculo_v2.py:994`, `app/ponto/motor_calculo_v2.py:1914`, `app/ponto/motor_calculo_v2.py:2458`, `app/ponto/motor_calculo_v2.py:2750`, `app/ponto/motor_calculo_v2.py:2792`, `app/ponto/motor_calculo_v2.py:2877`, `app/ponto/motor_calculo_v2.py:3215`, `app/ponto/motor_calculo_v2.py:3260`.
4. Fora da zona, `calculador.regras.noturnas_do_dia` também soma os pares fechados do dia, usando a mesma função de minutos e o mesmo desconto de pausas do motor. A alimentação vem dos segmentos efetivos do `ResultadoMes`. O adicional mensal soma os minutos legais dos períodos normais e de folga trabalhada. O subconjunto de **HE noturna** tem a função própria `he_noturna`. `app/ponto/calculador/alimentacao.py:35`, `app/ponto/calculador/alimentacao.py:525`, `app/ponto/calculador/regras.py:66`, `app/ponto/calculador/regras.py:844`, `app/ponto/motor_calculo_v2.py:303`, `app/ponto/motor_calculo_v2.py:409`, `app/ponto/services/fechamento.py:918`.

**Para col129, sim, se o par fechado chegar à chamada do motor e sobreviver à seleção pela ata.** Nesse caso a conta de noturno já é chamada, sem alteração em `motor_calculo_v2.py`. A prorrogação paga depende ainda da saída efetiva e da régua. O piso CLT do código desliga a prorrogação no 12x36 e a CCT usa o flag cadastrado. Na reprodução aritmética com `17:30–06:30` integral e sem pausa, a função contou **510 min de relógio com prorrogação e 420 sem ela**. Ciclo, regime, CCT, pausa, clipe e rubrica efetiva de col129 **[nao verificado]**. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:1752`, `app/ponto/motor_calculo_v2.py:2750`, `app/core/regua_cct.py:153`, `app/core/regua_cct.py:450`, `app/core/regua_cct.py:466`.

## 3. Teto de fechamento e leitores que podem divergir

| Sítio | Quem calcula, quem lê e o efeito |
|---|---|
| **Teto da jornada** | `_teto_da_jornada` calcula duração prevista bruta + 4 h ou devolve o envelope recebido. Tem duas chamadas AST, a do fechamento principal e a de `_teto_s_da_jornada`. Esta última tem três chamadas, nos dois pós-fechamentos e em `_cabe_na_jornada_da_vespera`. `app/ponto/turnos.py:63`, `app/ponto/turnos.py:66`, `app/ponto/turnos.py:230`, `app/ponto/turnos.py:1086`, `app/ponto/turnos.py:1177`, `app/ponto/turnos.py:1434`. |
| **Fechamento principal** | `parear_turnos` escolhe o mapa da E, passa **26 h** como envelope, mede a L-084 com `_dist_marco` e compara a duração. `_dist_marco` devolve a menor distância em minutos ao marco nas três datas em torno da batida. O corte de **180 min** é declarado por `MotorBase`. As 26 h são fallback do ramo e critério de carimbo, sem `min(teto_do_dia, 26 h)` no cálculo. `app/ponto/turnos.py:78`, `app/ponto/turnos.py:326`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1451`, `app/ponto/motor_calculo_v2.py:1931`. |
| **Pós-fechamentos** | `_fechar_aberto_com_saida_seguinte` tenta a S órfã seguinte ou a candidata posicional da L-115. `_fechar_aberto_na_pausa_sem_volta` tenta a última ida à pausa nas condições de fato encerrado. Ambos medem da E real e usam o teto do `data_turno`, com fallback `cont_max_s`. A exceção das duas pontas distantes está no fechamento principal. `app/ponto/turnos.py:943`, `app/ponto/turnos.py:1064`, `app/ponto/turnos.py:1084`, `app/ponto/turnos.py:1165`, `app/ponto/turnos.py:1177`, `app/ponto/turnos.py:1440`, `app/ponto/turnos.py:1501`. |
| **Saída pertence à véspera?** | `_mk` chama `_data_do_turno`, que chama `_cabe_na_jornada_da_vespera` para a S órfã e para a entrada de madrugada com S em condição de herdar o dia. Usa o mesmo número, ancorado no **`hi` previsto da véspera**, inclusive quando a E real falta. Decide atribuição de dia. `app/ponto/turnos.py:216`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:298`, `app/ponto/turnos.py:302`, `app/ponto/turnos.py:318`. |
| **`cont_max_s` e absorção** | O padrão do pareador e `MotorBase.AUT_CONT_MAX_S` são **14 h**. `MotorPlantao` fornece **26 h** pelo adaptador do motor. `_marca_intra_duracao` lê esse envelope nas guardas de continuidade, inclusive na sequência examinada antes de classificar pausa deslocada. A guarda de absorção pode deixar o turno aberto antes do ramo de fechamento se a marca ultrapassa o envelope e está a até 90 min de marco interno. `_marca_ecos_flush` tem 14 h no próprio default e os seus três chamadores o deixam no default. `app/ponto/turnos.py:344`, `app/ponto/turnos.py:370`, `app/ponto/turnos.py:414`, `app/ponto/turnos.py:451`, `app/ponto/turnos.py:482`, `app/ponto/turnos.py:1349`, `app/ponto/turnos.py:1475`, `app/ponto/turnos.py:1533`, `app/ponto/turnos.py:1941`, `app/ponto/turnos.py:2284`, `app/ponto/motor_calculo_v2.py:505`, `app/ponto/motor_calculo_v2.py:729`, `app/ponto/motor_calculo_v2.py:3513`. |
| **L-084 em ata, grade e dinheiro** | `papel_por_minuto_da_ata` e a reconciliação da grade leem os mesmos 180 min para saber se os marcos descrevem o turno. O motor lê `_cadastro_nao_descreve` para janela de HE e pontualidade. A duração total admissível e a distância a um marco são perguntas distintas. `app/ponto/turnos.py:1695`, `app/escala/utils.py:2001`, `app/ponto/motor_calculo_v2.py:1562`, `app/ponto/motor_calculo_v2.py:1606`, `app/ponto/motor_calculo_v2.py:1933`, `app/ponto/motor_calculo_v2.py:1988`. |
| **Carimbo, lista C9 e cron** | `_declara_separado_pelo_teto` escreve na instância da S quando o teto do dia foi decisivo e o vão cabia em 26 h. `pares_separados_pelo_teto` lê o resultado final e requer que a S continue órfã e a E continue aberta. Seu único chamador direto de produção é `teto_na_lista.medir`. `lavrar_teto_do_dia` grava `MetricaSnapshot`, `ultimo_lavrado` lê e `cadastro_realidade._teto_do_dia` mostra a C9. O texto da C9 também explicita a regra antiga. `app/ponto/turnos.py:165`, `app/ponto/turnos.py:183`, `app/ponto/turnos.py:1451`, `app/ponto/services/teto_na_lista.py:68`, `app/ponto/services/teto_na_lista.py:110`, `app/ponto/services/teto_na_lista.py:120`, `app/ponto/management/commands/lavrar_teto_do_dia.py:26`, `app/escala/services/cadastro_realidade.py:104`, `app/escala/services/cadastro_realidade.py:344`, `app/escala/services/cadastro_realidade.py:924`. |
| **Outras guardas sobre o vão total** | `_papeis_por_alternancia` recusa `>26 h` ao atribuir papéis a múltiplos períodos na grade. `_orfao_resolvido_cross_meianoite` descarta um **par já fechado** de `>26 h` para decidir se retira a órfã da fábrica. `minutos_noturnos` devolve zero para recorte `>26 h`. `MAX_TURNO_MIN` está declarado no motor, com leitor de produção **[nao verificado]**. No oráculo sem pares informados, existe corte de vão `>14 h`; a alimentação monetária passa `pares=`. `app/escala/utils.py:477`, `app/escala/utils.py:493`, `app/escala/utils.py:608`, `app/chamados/services/disputa_emissao.py:722`, `app/chamados/services/disputa_emissao.py:794`, `app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:204`, `app/ponto/calculador/nucleo.py:110`, `app/ponto/calculador/nucleo.py:215`, `app/ponto/calculador/alimentacao.py:449`, `app/ponto/management/commands/e6_oraculo.py:153`. |

**Onde o cadastro precisa decidir.** O fechamento principal, os dois pós-fechamentos e o envelope de absorção precisam receber a mesma resposta empresarial sempre que decidirem se uma marca pertence à mesma jornada. A atribuição à véspera também precisa consumir essa resposta onde usa a duração admissível para decidir pertencimento, observando que hoje mede a partir de `hi`. A grade por alternância e o filtro de órfã cross-meia-noite têm veto próprio de 26 h e podem contrariar um par admitido acima disso. A proteção de 26 h do noturno também teria efeito monetário nesse cenário. C9, o snapshot e a apresentação leem o **motivo e o teto já decididos**. O valor inicial do cadastro e a possibilidade de superar 26 h ficam **[nao verificado]**. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:216`, `app/ponto/turnos.py:1086`, `app/ponto/turnos.py:1177`, `app/ponto/turnos.py:1349`, `app/ponto/turnos.py:1430`, `app/escala/utils.py:493`, `app/chamados/services/disputa_emissao.py:727`, `app/ponto/motor_calculo_v2.py:94`, `app/ponto/services/teto_na_lista.py:68`.

**Vão de 11 h.** O pareador atual não consulta `S→E >= 11 h`. A linha **2400** do motor itera sobre períodos já montados, e **2405–2423** permitem que um período herde o dia do anterior quando o vão é de **0 a 6 h** e há uma saída anterior real. Isso muda a atribuição de dia. A inferência de pausa do pareador tem outra régua e a expiração da aberta usa **fim previsto + 4 h**. As 11 h aparecem no mapa do art. 66 e no corte de permuta, e o helper de cobertura declara expressamente que não as valida. `app/ponto/motor_calculo_v2.py:2400`, `app/ponto/motor_calculo_v2.py:2405`, `app/ponto/motor_calculo_v2.py:2423`, `app/ponto/turnos.py:382`, `app/ponto/turnos.py:1408`, `app/ponto/turnos.py:2021`, `app/docs/mapa_seletor_cobranca.md:27`, `app/docs/PROMPTS.md:2015`, `app/ponto/utils.py:141`.

## 4. Cadastro por empresa e molde da UI

- **`Empresa`** reúne `regime_trabalhista`, o corte de competência e os parâmetros da janela de HE, incluindo vigência, piso, tetos das pontas e limite de decisão. O motor lê a empresa em `_janela_do_dia`. O pareador recebe batidas, marcos, papéis e parâmetros numéricos, sem campo de empresa na assinatura atual. O campo **“maior jornada contínua real”** no model e no formulário atual **[nao verificado]**. `app/colaboradores/models.py:40`, `app/colaboradores/models.py:46`, `app/colaboradores/models.py:58`, `app/colaboradores/models.py:82`, `app/colaboradores/models.py:117`, `app/colaboradores/models.py:150`, `app/ponto/motor_calculo_v2.py:1380`, `app/ponto/motor_calculo_v2.py:1650`, `app/ponto/turnos.py:1192`.
- **Régua CCT e `Sindicato`.** `regua_para` lê `Empresa.regime_trabalhista`. Em CCT, `_aplicacao_vigente` resolve `AplicacaoConvencao` da empresa e o recorte de praça declarado. `get_motor_cct` injeta a régua de excedente e os flags aplicáveis de noturno e feriado. `Sindicato.teto_prorrogacao_dia_min` de 120 min refere-se à prorrogação diária do art. 59 e está declarado como **sem efeito no cálculo**, assim como os campos de compensação da CCT. `app/core/models.py:63`, `app/core/models.py:70`, `app/core/models.py:119`, `app/core/regua_cct.py:72`, `app/core/regua_cct.py:231`, `app/core/regua_cct.py:352`, `app/core/regua_cct.py:428`, `app/core/regua_cct.py:466`.
- **Molde existente com nome e leitor.** O template `empresa_form.html` exibe **Regime trabalhista** e o campo numérico **Dia de início da apuração (corte da folha)**. As rotas de nova e editar Empresa chamam `form_empresa`. Ela lê o POST, valida e delega para `criar_empresa` ou `atualizar_empresa`. Os leitores operacionais são `regua_para` e `janelas.corte_da_empresa`, respectivamente. Os parâmetros `janela_he_*` são outro precedente no model e no motor. `app/templates/colaboradores/empresa_form.html:39`, `app/templates/colaboradores/empresa_form.html:53`, `app/colaboradores/urls.py:28`, `app/colaboradores/views.py:504`, `app/colaboradores/views.py:531`, `app/colaboradores/views.py:539`, `app/colaboradores/views.py:550`, `app/colaboradores/services/estrutura.py:13`, `app/core/regua_cct.py:352`, `app/ponto/janelas.py:42`, `app/ponto/motor_calculo_v2.py:1650`.

## 5. Como chega à Gestão de HE

**O caminho já existe, mas fechar o par não garante que as duas pontas apareçam.** A tela lê o **retrato lavrado** por empresa e competência, via `he_pendente_lavrado.ler` → `gestao_he.enriquecer` → `gestao_he.html`. O escritor `lavrar_he_pendente` chama `apurar`, que passa por `espelho_do_colab` → `montar_dias` e recolhe `dia['he_fora_da_janela']`. Grava o retrato em `MetricaSnapshot`. A leitura da tela só vê o novo resultado depois dessa lavra. `app/ponto/views.py:948`, `app/ponto/views.py:969`, `app/ponto/views.py:978`, `app/ponto/management/commands/lavrar_he_pendente.py:62`, `app/ponto/management/commands/lavrar_he_pendente.py:70`, `app/ponto/services/he_pendente_lavrado.py:87`, `app/ponto/services/he_pendente_lavrado.py:132`, `app/ponto/services/he_pendente_lavrado.py:177`, `app/ponto/services/he_pendente_lavrado.py:193`, `app/ponto/services/espelho.py:1298`.

Com a janela empresarial **ativa e vigente**, o espelho marca as pontas reconhecidas na grade acima do piso, descontando a extra declarada no vínculo efetivo. Para marco faltante, pode aproveitar o relato de Cadastro × Realidade do motor se ele nomear o mesmo marco e a ponta ainda não estiver marcada. O motor só publica essa causa no relato quando a sobra **passa do teto daquela ponta**. A saída depende ainda de `janela_he_saida_ativa`. Assim, uma ponta sem atribuição na grade e sem relato elegível pode continuar fora da Gestão de HE mesmo com o par fechado. Os flags e o retrato dos quatro casos **[nao verificado]**. `app/ponto/services/espelho.py:735`, `app/ponto/services/espelho.py:754`, `app/ponto/services/espelho.py:790`, `app/ponto/janela_he.py:156`, `app/ponto/janela_he.py:267`, `app/ponto/janela_he.py:289`, `app/ponto/janela_he.py:293`, `app/ponto/motor_calculo_v2.py:1702`, `app/ponto/motor_calculo_v2.py:1733`, `app/ponto/motor_calculo_v2.py:1747`.

**A classificação é do motor**, pela escala efetiva, extra declarada, janela, pausas, excedente e regras da família, inclusive banco no comercial. A Gestão de HE apresenta as pontas para decisão. `decidir_he` grava a decisão por pessoa e dia e aciona o recálculo. A compensação cadastrada em `Sindicato` está sem efeito no cálculo. `aval_v2_anexo.txt:1`, `app/escala/models.py:1221`, `app/ponto/motor_calculo_v2.py:1752`, `app/ponto/motor_calculo_v2.py:2793`, `app/ponto/motor_calculo_v2.py:3386`, `app/ponto/motor_calculo_v2.py:3426`, `app/core/regua_cct.py:77`, `app/ponto/portas/he.py:159`, `app/ponto/portas/he.py:186`, `app/ponto/portas/he.py:230`.

## 6. Censo AST e alcance das mudanças

**13 chamadas diretas a `parear_turnos` em código de produção e comandos**, contando aliases, fora de testes e migrações. As três que passam `marcos_por_dia` são **motor**, **batidas carregadas** e **juiz do colaborador**. As demais podem mudar se o par que elas efetivamente recebem passar a fechar. Para as duas noites reproduzidas, o caminho sem mapa já fechava nas condições isoladas da pergunta 1. `app/ponto/motor_calculo_v2.py:503`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1535`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2295`.

| Apelidos das chamadas diretas | Resultado que pode mudar |
|---|---|
| **Motor v2**, `turnos_via_autoridade` · **Batidas carregadas**, `turnos_de_batidas` · **Juiz do colaborador/lote**, `_turnos_do_colab_puro` | Períodos monetários, mapa de dia e geometria consumida pelos leitores. O mapa do motor 

[truncado em 20000 de 30444 caracteres]

## 23/11 13:49 · sessao dd8f4a95c65d · A-TELA-DO-DIA-LE-UM-JUIZ · leitura · origem=Code

A sonda registra **370 dias com “Em aberto” e outro status**. São 138 `ok`, 231 `escala_suspeita` e 1 `folga_trabalhada`. Para o colaborador 945, os três dias têm `ok` e `resolver_dia_oferecido=false`. Em 24/10 há uma `E` e `minutos_pagos=null`. Em 26 e 28/10 há `E–S–E` e, respectivamente, 238,20 e 229,60 minutos pagos no dado raspado. `sonda.out:4–7` (anexo).

A correspondência da cópia com o hash informado está **[nao verificado]**. A cópia não contém os metadados Git necessários para confirmar o `HEAD`.

## 1. Como o cartório grava `ok` e quem escreve a coluna

O caminho automático é `processar_cartorio` → `julgar_colab` → `classificar_dia` → `lavrar_veredito` → `carimbar`. O lote seleciona células de datas passadas. O cartório monta a grade com batidas apuráveis, conta marcos casados e órfãs e entrega ao classificador, entre outros insumos, `turno_encerrado`. `app/ponto/management/commands/processar_cartorio.py:49–65`, `app/ponto/services/cartorio.py:1016,1180–1184,1199–1204,1453–1455,1500–1511,1898–1922`.

- **Uma `E` e `E–S–E` podem chegar a `ok` pela mesma sequência.** Com `turno_encerrado=False`, os dois ramos de `FURO_PARCIAL` ficam fora, tanto o de realizado positivo quanto o de batida sem par fechado. Se o julgamento ficar sem códigos e sem fio que proteste, `derivar_veredito([])` devolve `concorde`. A regra do status recebe `tem_batida` como booleano. Com batida e sem a precedência de folga trabalhada, `concorde`, `coberta` e `cobranca_indevida` retornam `ok`. A regra também tem o ramo `ok` por isenção sem batida. `app/ponto/supra_juiz.py:291,392–439,450–463`, `app/ponto/services/cartorio.py:1514–1546`, `app/escala/models.py:1635–1639`, `app/ponto/portas/celula.py:1013–1020`, `app/ponto/status_dia.py:147–163,185–192`.
- **Com o turno encerrado**, os mesmos ramos podem emitir `FURO_PARCIAL` se as demais guardas passarem. O código vira veredito `furo` e, no ramo com batida, status `irregular`. A própria rotina pode preservar uma lavra anterior ao pular uma célula cuja impressão segue igual. O marcador de turno em curso entra na impressão normal apenas para `data < hoje`. O caminho avulso também admite julgar no próprio dia depois do fim previsto. O veredito, os protestos, a impressão, a via e o instante que produziram os três `ok` do 945 estão **[nao verificado]**. `app/ponto/supra_juiz.py:392–439,450–463`, `app/escala/models.py:1622–1627`, `app/ponto/status_dia.py:67–70,157–162`, `app/ponto/services/cartorio.py:1425–1444,2081–2090`, `sonda.out:5–7` (anexo).

**Escritores da coluna.** `_aplicar_status` atribui `status_dia` e `status_rotulo`. A lavra normal e o ramo `nunca_bateu` passam pela porta e persistem com `carimbar`. O D5, chamado por `lavrar_status_competencia`, completa a coluna nula com o veredito já gravado e o carimbo em dia. A decisão do admin passa pela mesma porta e mapeia `trabalhou → ok`, `faltou → falta` e `justificado → ausencia`. O cartório preserva essa precedência. A regeneração ou reversão de célula futura também pode escrever `NULL` via `desfazer_julgamento_da_futura`, preservando decisão de admin. A coluna nasce anulável. `app/ponto/portas/celula.py:267,314,877,1045–1065,1096–1134,1148,1182–1203,1217–1237`, `app/ponto/services/cartorio.py:939–956,1321–1326,1438–1441,1898–1922`, `app/ponto/management/commands/lavrar_status_competencia.py:56–57`, `app/ponto/services/veredito_celula.py:352`, `app/ponto/status_dia.py:65,98–101`, `app/escala/models.py:1827`.

## 2. Autoridades e ponto da divergência

| Pergunta | Autoridade registrada |
|---|---|
| A que turno a batida pertence e como ele foi pareado? | `parear_turnos` e `turnos_do_colab`. `app/docs/JUIZES.md:20–24`, `app/core/juizes.py:216` |
| Há turno aberto **vivo agora**? | `turno_aberto_de`. `app/docs/JUIZES.md:20`, `app/core/juizes.py:219`, `app/ponto/turnos.py:2300–2309` |
| Que marco do dia faltou? | `classificar_falta`. Para uma `E` sem `S` devolve falta de saída. Com `E` e `S`, também consulta `t.aberto` antes da contagem, quando há marcos esperados e as guardas deixam chegar ao ramo. `app/docs/JUIZES.md:132`, `app/core/juizes.py:200`, `app/escala/servico_jornada.py:401–415,448–486` |
| Qual é o status do dia? | `status_do_dia_lavrado`, cuja implementação é `status_dos_dias_lavrados`. `app/docs/JUIZES.md:133–134`, `app/core/juizes.py:207`, `app/ponto/status_dia.py:201–221` |

**O D4 consulta a geometria para saber se o dia está em curso.** Ele usa `dias_encerrados`, que consulta `turno_aberto_vivo`. Onde há janela de expiração, a aberta deixa de ser viva depois do fim previsto mais quatro horas. O D4 então pode devolver a coluna lavrada. `classificar_falta` e `datas_em_aberto_de` ficam fora desse caminho. `app/core/juizes.py:243`, `app/ponto/turnos.py:2005–2035,2113–2130,3063–3067`, `app/ponto/status_dia.py:269–295`.

**“Em aberto” usa a pendência histórica.** No calendário, a entrada é `t.aberto` da geometria, mais saídas órfãs e furos apurados pelo motor. `folha_manda` publica a união, subtraindo datas decididas e o dia do turno ainda vivo. O teto de data e a isenção se aplicam à parcela de furo apurado. Assim, uma aberta expirada pode continuar na lista enquanto o D4 já entrega `ok`. O calendário usa a pertinência na lista para a palavra e o D4 para o status. `app/colaboradores/services/calendario.py:600–616,639–640,748–763`, `app/relatorios/cartao_pela_celula.py:373–374,395–422`, `app/ponto/services/dia_decidido.py:241–246,346–355,398–430`.

## 3. `escala_suspeita` com saída órfã

O pareador cria a saída órfã com `entrada=None` e `saida` preenchida. Isso dá `t.aberto=False`, mas `datas_saida_orfa` inclui seu `data_turno` na união de “Em aberto”. Uma saída órfã sozinha, portanto, entra na lista histórica sem acionar o motivo de turno vivo no D4. A lei do furo, se chegar ao ramo de `S` sem `E`, devolve falta de **entrada**. `app/ponto/turnos.py:234–245,312–324,1495–1498,2011–2018`, `app/ponto/services/dia_decidido.py:346–355`, `app/ponto/status_dia.py:273–289`, `app/escala/servico_jornada.py:459–468`.

Na lavra, `escala_suspeita` vem do veredito `indefinida`, depois das guardas de falta, ausência e folga humana. Seu rótulo fixo começa com “Sem batida”, e o ramo pode ser alcançado com `tem_batida=True`. Já `BATIDA_ORFA_FORA_TOLERANCIA` é um código sobre **órfã da grade**. Sozinho, vira `discordante` e, no ramo com batida, `irregular`. Se coexistir com código de `indefinida`, este tem precedência no mapa sobre `discordante`. `app/ponto/status_dia.py:38–39,139–146,157–163,185–190`, `app/ponto/supra_juiz.py:527–537`, `app/escala/models.py:1622–1642`.

A linha de 31/10 do colaborador 941 tem `batidas_do_turno=[]`. O agregado de 231 conta `escala_suspeita` com “Em aberto”, sem identificar a parcela que pôs cada dia na lista. Um caso individual de `escala_suspeita` **com saída órfã** na sonda está **[nao verificado]**. `sonda.out:4,8` (anexo).

## 4. Leitores do status, do botão e de `datas_em_aberto`

| Leitor | O que usa |
|---|---|
| Calendário e espelho | O calendário lê D4 para fundo, ícone e contador. A faixa semanal do espelho também lê D4 e aplica a classe `st-ok`. O miolo do espelho monta `palavra_dia` pela lista do resumo. `app/colaboradores/services/calendario.py:639,696–697,762–763,817`, `app/templates/colaboradores/partials/_calendario_grade.html:68–74,99,152–155`, `app/ponto/services/espelho.py:170–174,1327`, `app/templates/ponto/espelho.html:113,151` |
| **Resolver dia** | Exige dia passado, anterior ao início da competência corrente, sem pergunta viva com resposta, e status em `falta`, `irregular`, `aberto` ou `sem_certidao`. `ok` e `escala_suspeita` ficam fora. O template ainda exige `pode_veredito`. A tarefa do dia reutiliza a oferta do calendário. `app/colaboradores/services/calendario.py:94–104,117–122,493–508,774,812–813`, `app/templates/colaboradores/partials/_calendario_grade.html:134–135`, `app/chamados/services/tarefa_do_dia.py:697–712` |
| Cobrança | A auditoria de espelhos seleciona só `irregular` no D4 e lê os marcos apagados na ata. O ranking “Quem cobrar” agrega `FuroDiario` e fila de chamados. A emissão efetiva segue códigos, guardas e ata no cartório, e a emissão de disputa consulta `classificar_falta`. Cobrança efetiva nos casos da sonda **[nao verificado]**. `app/relatorios/furos.py:127–145,164–168`, `app/relatorios/views.py:1643`, `app/escala/services/furos_diarios.py:178–213`, `app/ponto/services/cartorio.py:1783–1841,1861–1879`, `app/chamados/services/disputa_emissao.py:813–820,1214,1251` |
| Folha | A retenção do TXT consulta `veredito`, `protestos` e ausência de célula ou de veredito. A aprovação também tem trava por `FechamentoMensal.turnos_abertos`. O Raio-X lê marcos da ata, e o detalhamento de destrava pode herdar o D4 via tarefa e oferta de Resolver. Os minutos do calendário vêm de `DiaPago`. O motor tem uma regra própria para creditar pares fechados em dia de turno aberto. A origem dos valores exatos da sonda e seu efeito no TXT estão **[nao verificado]**. `app/folha/export.py:830–848,968–973,1265–1307`, `app/folha/views.py:637`, `app/relatorios/furos.py:190–214`, `app/folha/fora_da_folha.py:273–280`, `app/colaboradores/services/calendario.py:319,688–689,781–783`, `app/ponto/motor_calculo_v2.py:2511–2564` |
| App e Painel Operacional | A API publica o D4 em `status_dia`, traduz para `status` e leva a palavra do espelho. O painel lê D4 para os dias encerrados que selecionou. `app/api/views.py:1361,1387–1395,1496,1548,1561–1563,1621–1626`, `app/ponto/status_dia.py:79–91`, `app/core/services/painel_op.py:329–338` |

- **Censo AST de `status_dia == 'ok'`.** Zero comparações literais ou filtros ORM de execução contra `ok` na coluna. O leitor funcional da coluna é o D4. O leitor Python que compara a projeção do D4 com `ok` é o contador do calendário, `d['status'] == 'ok'`. A porta, o D5 e a fotografia E4B também leem a coluna para seus próprios fins. `app/ponto/status_dia.py:287–289`, `app/colaboradores/services/calendario.py:696–697,762,817`, `app/ponto/portas/celula.py:1056–1065,1082`, `app/ponto/services/cartorio.py:946`, `app/chamados/services/foto_e4b.py:247–252,269`.
- **Censo AST de `datas_em_aberto`.** Os leitores de execução são o calendário, por parâmetro ou pelo resumo, o miolo do espelho, o PDF, o passivo da Pauta DP e o comando `diff_pdf_x_espelho`, que lê a chave dinamicamente e compara comprimentos. A API herda a palavra do espelho. PDF, placar da folha e comando cartão × TXT também leem a contagem derivada `dias_em_aberto`. `app/colaboradores/services/calendario.py:595–616,749`, `app/ponto/services/espelho.py:330–345,1327`, `app/relatorios/pdf_espelho.py:638,791–792`, `app/ponto/services/dias_em_aberto.py:112`, `app/pautas/management/commands/pauta_dia_em_aberto.py:82`, `app/relatorios/management/commands/diff_pdf_x_espelho.py:36,93–102`, `app/api/views.py:1361,1387–1395`, `app/folha/porta_export.py:621`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:64`.

## 5. `Falta (desconta Nh)` e minutos do dia

| Leitura | Unidade e consumidores |
|---|---|
| **Rótulo de falta e cartão diário** | `Ausencia.minutos` é a duração **do registro**. Para falta aprovada com zero minutos, `save` soma o previsto de `data` a `data_fim`. `rotulo_de` passa o total cru para `palavra_do_dia`, que imprime `Falta (desconta Nh)`. `cartao_do_dia` também expõe o total cru, e `decididos_do_periodo` monta esse cartão para cada data da faixa. Assim, o rótulo diário pode repetir o total da faixa no calendário, espelho, PDF e app. `app/ponto/models.py:750–759,887–897`, `app/ponto/services/dia_decidido.py:156–169,249–256,473–478,533–546,561–569,605–611`, `app/colaboradores/services/calendario.py:748–757`, `app/ponto/services/espelho.py:329–346,1327`, `app/relatorios/pdf_espelho.py:638,923–924`, `app/api/views.py:1387–1400` |
| **Leituras diretas com guarda de um dia** | `cobre_parte_do_dia` exige tipo habilitado, uma única data e `0 < minutos < 480`. `creditos_por_dia` soma por `a.data` para abater atraso no próprio dia. `efeitos_da_ausencia` soma essas parciais no período para o PDF. A lista e o painel de ausências exibem os minutos sob a guarda de parcialidade. `app/ponto/turnos.py:2599–2619`, `app/ponto/services/credito_parcial.py:24–28`, `app/ponto/services/fechamento.py:994–1001`, `app/ponto/services/dia_pago.py:188–195`, `app/ponto/services/efeito_ausencia.py:46–50,63–65`, `app/relatorios/pdf_espelho.py:332–336,479`, `app/ponto/views.py:2112,2340–2344`, `app/templates/ponto/ausencias.html:159–165`, `app/templates/ponto/partials/painel_ausencia.html:310–327` |
| **Rateio de falta por dia** | `minutos_de_falta_por_dia` seleciona ausências aprovadas que descontam, divide o total pela quantidade de datas da expansão **inteira** do registro e soma as parcelas por data da janela. A única chamada direta de execução encontrada é no recálculo do fechamento. Alimenta `FechamentoMensal.horas_falta`, `DiaPago.horas_falta` e `detalhe['falta_min']`. A rubrica 8792 do TXT usa `datas_falta` e monta o bloco de datas. `app/folha/export.py:87–92,113–143,740–744`, `app/ponto/services/fechamento.py:873–875,1003,1200–1206,1277–1282`, `app/ponto/services/dia_pago.py:274–277` |

A ocorrência em produção de uma falta de várias datas com o rótulo diário divergente do rateio está **[nao verificado]**. `sonda.out:3–12` (anexo).

## 23/11 13:48 · sessao 492e33b59d8e · - · - · origem=Ronald (tmux)

- A porta que grava `CelulaDia.status_dia` é `lavrar_veredito`, via `_aplicar_status`. Entram por ela o julgamento normal do cartório, o ramo sem batida na vida, o complemento D5 e a decisão humana. O desfazimento de julgamento futuro também grava `NULL` pelo `setattr` da mesma porta. A varredura em memória do selo AST percorreu 828 fontes de produção sem encontrar escritor fora da porta. `app/ponto/portas/celula.py:1045–1065`, `:1096–1134`, `:1148–1149`, `:1182–1203`, `app/ponto/services/cartorio.py:939–956`, `:1321–1326`, `:1898–1922`, `app/ponto/services/veredito_celula.py:352`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:171–194`.

- No fluxo do cartório, lista de códigos vazia produz `concorde`. Havendo batida, `concorde`, `coberta` e `cobranca_indevida` podem produzir `ok`. O status recebe presença como booleano, sem a quantidade de marcas faltantes. `app/ponto/services/cartorio.py:1500–1547`, `app/escala/models.py:1635–1642`, `app/ponto/status_dia.py:157–163`, `:185–192`. Com as demais guardas satisfeitas, o classificador atual acusa `FURO_PARCIAL` tanto para E sem par e realizado zero quanto para E-S-E com realizado positivo. Ambos os ramos dependem de `dia_encerrado` e de `not sem_ancora`. O ramo isento devolve lista vazia. `app/ponto/supra_juiz.py:229–250`, `:392–463`. Qual guarda, decisão humana ou fotografia da lavra escreveu o `ok` do 945 é [não verificado].

- O próprio código permite lavrar enquanto o turno de um dia anterior ainda vive. Nesse caso o término muda `enc=0` para ausente e habilita rejulgamento. Há outra trajetória no avulso do próprio dia, após o fim previsto mas dentro da margem da abertura. Nela `enc=0` fica ausente por `data < hoje`, assim como na próxima avaliação já encerrada. Com o restante da impressão igual, a avaliação seguinte pula o dia. `app/ponto/services/cartorio.py:528–529`, `:1424–1444`, `:1500–1511`, `:2073–2090`, `app/ponto/tests/test_celula_concorde_meio_turno.py:44–67`. Ocorrência dessa trajetória no dado fornecido [não verificado].

- A autoridade registrada de furo é `classificar_falta`, a de presença viva é `turno_aberto_de`, e a de status é `status_do_dia_lavrado`. D4 consulta o juiz de encerramento, que usa `turno_aberto_vivo` e expira a abertura depois do fim da janela do template mais quatro horas, então lê a coluna. `datas_em_aberto_de` usa abertura geométrica, saída órfã e furo do motor, com suas respectivas exclusões. A abertura geométrica expirada pode permanecer nessa lista. `app/docs/JUIZES.md:20–24`, `:132–134`, `app/core/juizes.py:200–219`, `:243`, `app/ponto/status_dia.py:269–290`, `app/ponto/turnos.py:2005–2035`, `:2113–2130`, `app/colaboradores/services/calendario.py:600–616`, `:748–763`, `app/relatorios/cartao_pela_celula.py:373–422`, `app/ponto/services/dia_decidido.py:346–355`.

- A S órfã forma turno com `entrada=None`, `saida` preenchida e `aberto=False`. Pode entrar na palavra por `datas_saida_orfa` sem bloquear a leitura da coluna no D4. `escala_suspeita` deriva de `indefinida` no ramo que passou pelas precedências. A órfã da grade, em jornada prevista de trabalho com zero realizado, pode gerar `BATIDA_ORFA_FORA_TOLERANCIA`, que isolado leva a `discordante` e `irregular`. Se coexistir código de `indefinida`, este tem prioridade sobre `discordante` no mapa. `app/ponto/turnos.py:234–245`, `:312–324`, `:1495–1498`, `app/ponto/status_dia.py:139–146`, `:159–160`, `app/ponto/supra_juiz.py:527–537`, `app/escala/models.py:1622–1642`. A sonda não mostra uma S órfã no exemplo 941 de 31/10, nem discrimina por S órfã os 231 casos [não verificado].

## 23/11 13:47 · sessao 5b8da13f9df7 · - · - · origem=Ronald (tmux)

**Leitores do D4.** O único leitor funcional direto da coluna para classificação é `status_dos_dias_lavrados`, que devolve `cel.status_dia` quando o relógio deixa ler a lavra. A porta e a retrolavra também leem a coluna para comparar, preservar ou completar o registro. `app/ponto/status_dia.py:272–295`, `app/ponto/portas/celula.py:1056–1065,1082`, `app/ponto/services/cartorio.py:946`.

- **Calendário.** Lê o D4 e publica `status`/`status_rotulo`. A palavra vem, em paralelo, da pertinência em `_em_aberto` passada a `do_dia`. O template usa a palavra como título da célula e o status para fundo e ícone. `app/colaboradores/services/calendario.py:639,696–697,748–763`, `app/templates/colaboradores/partials/_calendario_grade.html:68–74,97–104`.
- **Espelho e app.** A faixa semanal do espelho lê status/rotulo do D4, e o miolo aplica a palavra pelo resumo. O app lê esse miolo e consulta o D4 separadamente, publicando os dois sinais e ainda `turno_aberto` por geometria. O adaptador do app mantém `ok` e traduz `escala_suspeita` como `neutro`. O Painel Operacional lê D4 nos dias encerrados. `app/ponto/views.py:162–164,203`, `app/ponto/services/espelho.py:170–174,1327`, `app/templates/ponto/espelho.html:151–153`, `app/api/views.py:1361,1387–1395,1496,1512–1516,1547–1548,1560–1563,1610–1626`, `app/ponto/status_dia.py:79–91`, `app/core/services/painel_op.py:329–338`.
- **Resolver dia.** O gate da oferta exige dia passado, anterior ao início da competência corrente, sem pergunta viva com resposta, e status em `falta`, `irregular`, `aberto` ou `sem_certidao`. `ok` e `escala_suspeita` ficam fora. O template exige também `pode_veredito`, oriundo da permissão `regularizar_batida`. A tarefa pergunta pela mesma oferta nos dias elegíveis sem rota. O POST tem seus próprios controles de permissão, datas e veredito e delega à porta de ação. `app/colaboradores/services/calendario.py:94–104,117–122,493–508,774,812–813,157–161`, `app/colaboradores/views.py:1990–2000`, `app/templates/colaboradores/partials/_calendario_grade.html:134–135`, `app/chamados/services/tarefa_do_dia.py:697–712`, `app/ponto/views.py:3491–3520`.
- **Cobrança.** A auditoria de espelhos seleciona só `irregular` no D4, lê horários apagados da ata e passa `NAO_PERGUNTADO` ao calendário. O ranking «Quem cobrar» agrega `FuroDiario` e a fila de chamados. A emissão efetiva segue códigos do julgamento, guardas e marcos apagados no cartório, e o agrupador de órfãos consulta `classificar_falta` e a ata. Ocorrência efetiva de cobrança nos registros da sonda [nao verificado]. `app/relatorios/furos.py:127–145,164–168`, `app/relatorios/views.py:1643`, `app/escala/services/furos_diarios.py:25–57,178–186,193–213`, `app/ponto/services/cartorio.py:1783–1796,1813–1841,1861–1879`, `app/chamados/services/disputa_emissao.py:813–820,1214–1233,1251–1264`.
- **Folha.** A retenção do TXT usa `CelulaDia.veredito`, protestos e guardas de célula/veredito ausente. O Raio‑X lê os marcos apagados da ata. O detalhamento de destrava lê a tarefa e pode herdar indiretamente a oferta de Resolver produzida pelo calendário. `app/folha/export.py:968–973,1205–1213,1265–1307`, `app/folha/views.py:637`, `app/relatorios/furos.py:190–214`, `app/folha/fora_da_folha.py:273–280`, `app/chamados/services/tarefa_do_dia.py:697–712`.

**Censo AST.** Nas 3.752 árvores Python parseadas em memória, não apareceu comparação literal `status_dia == 'ok'` ou invertida. O consumidor Python do predicado equivalente sobre a projeção D4 é o contador `d['status'] == 'ok'`. No template do calendário, ele pinta o fundo e o check, e aparece também num laço sem saída renderizada antes da leitura do contador. `app/colaboradores/services/calendario.py:696–697,762,817`, `app/templates/colaboradores/partials/_calendario_grade.html:69,99,152–155`.

- **Acessos de execução a `datas_em_aberto`.** Calendário recebe a lista por parâmetro ou lê o resumo e testa pertinência. Espelho transforma a lista em conjunto e a passa à palavra. PDF reaplica com a lista do seu resumo. Pauta DP consome a lista no diagnóstico. O comando de diff acessa a chave indiretamente e compara comprimentos. `app/colaboradores/services/calendario.py:595–597,616,749`, `app/ponto/services/espelho.py:330,341,1327`, `app/relatorios/pdf_espelho.py:638`, `app/ponto/services/dias_em_aberto.py:112`, `app/pautas/management/commands/pauta_dia_em_aberto.py:82`, `app/relatorios/management/commands/diff_pdf_x_espelho.py:36,94–101`.
- `folha_manda` grava a lista efetiva e a contagem. O PDF tem placeholder anterior e lê a palavra na coluna Obs. Auditoria de furos passa uma sentinela para pular a lista. A contagem derivada é lida no PDF, placar e comando cartão × TXT. `app/relatorios/cartao_pela_celula.py:419–422`, `app/relatorios/pdf_espelho.py:468,791–792,923–924`, `app/relatorios/furos.py:168`, `app/folha/porta_export.py:621`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:64`.

**Saída órfã.** `datas_saida_orfa` coleta a `data_turno` quando há saída sem entrada no par. Calendário, espelho e PDF publicam essa parcela, unida com turno aberto e furo apurado, retirando datas decididas e o turno em curso. O teste estático usa `S–E–S` noturno ancorado na véspera e exige `em_aberto` e parcial positivo, sem afirmar `escala_suspeita`. A sonda mostra o agregado de 231 e um exemplo de `escala_suspeita` com `batidas_do_turno=[]`. Caso individual de `escala_suspeita` com saída órfã na sonda [nao verificado]. `app/ponto/turnos.py:234–245`, `app/colaboradores/services/calendario.py:607–613`, `app/ponto/services/espelho.py:630–631`, `app/relatorios/pdf_espelho.py:619–622`, `app/ponto/services/dia_decidido.py:346–355`, `app/ponto/tests/test_saida_orfa_dia_em_aberto.py:91–104`, `sonda.out:4,8` (anexo).

## 23/11 13:46 · sessao dc35802cd5c9 · - · - · origem=Ronald (tmux)

**5. Rótulo de falta e minutos por dia**

- `Ausencia.minutos` é a duração do registro. Ao salvar falta aprovada com zero minutos, `Ausencia.save` soma os previstos entre `data` e `data_fim`. `rotulo_de` entrega esse total a `palavra_do_dia`, que formata `Falta (desconta Nh)`. A única supressão de minutos em `rotulo_de` é para abono que cobre dia inteiro. Um registro descontado que cobre várias datas pode, portanto, exibir seu total em cada linha de dia. `app/ponto/models.py:750-759,887-897`; `app/ponto/services/dia_decidido.py:156-169,249-256,398-399,473-476,533-546`; `app/ponto/turnos.py:2752-2778`.
- `cartao_do_dia` também expõe `minutos` crus e `decididos_do_periodo` põe esse cartão em cada data da faixa. Para o rótulo diário, os leitores são calendário, espelho, PDF e API do app. `app/ponto/services/dia_decidido.py:561-569,605-611`; `app/colaboradores/services/calendario.py:748-757`; `app/templates/colaboradores/partials/_calendario_grade.html:97-98`; `app/ponto/services/espelho.py:329-346,1327`; `app/relatorios/pdf_espelho.py:636-638,923-924`; `app/api/views.py:1387-1400`.
- Outras leituras de minutos como duração **do próprio dia** têm a guarda de ausência parcial, que exige tipo habilitado, uma única data e `0 < minutos < 480`. `cobre_parte_do_dia` lê o campo para essa classificação. `creditos_por_dia` soma o valor por `a.data` para abater o atraso daquele dia, `efeitos_da_ausencia` soma o valor das parciais do período para o PDF, e lista e painel mostram os minutos quando a guarda permite. `app/ponto/turnos.py:2599,2602,2617-2619`; `app/ponto/services/credito_parcial.py:24-28`; `app/ponto/services/fechamento.py:994-1001`; `app/ponto/services/dia_pago.py:188-195`; `app/ponto/services/efeito_ausencia.py:46-50,63-65`; `app/relatorios/pdf_espelho.py:332-336,479`; `app/ponto/views.py:2112,2340-2344`; `app/templates/ponto/ausencias.html:159-165`; `app/templates/ponto/partials/painel_ausencia.html:310-313,323-327`.
- `minutos_de_falta_por_dia` seleciona ausências aprovadas que descontam, divide os minutos do registro pelo número de datas de sua expansão integral e soma os valores por data da janela. A chamada direta de produção está em `recalcular_fechamento_mes`, que usa o total para `FechamentoMensal.horas_falta` e passa o mapa para `DiaPago.horas_falta` e `detalhe['falta_min']`. O resumo do cartão lê o fechamento ou a soma das diárias. A rubrica 8792 do TXT usa `datas_falta` e emite contagem e datas. `app/folha/export.py:87-92,113-122,125-143`; `app/ponto/services/fechamento.py:873-875,1003,1200-1206,1277-1282`; `app/ponto/services/dia_pago.py:274-277`; `app/relatorios/cartao_pela_celula.py:115,143-146,183-185`; `app/folha/export.py:215-221,740-744`.
- Leituras por registro incluem serialização da API, admin, detalhe e edição, auditoria, duração em horas, fotografia do insumo da folha, histórico, CSV de atestados e espelhamento da tabela. O ramo de pendência lê o campo, mas o rótulo pendente não o usa. `app/api/views.py:1882`; `app/ponto/admin.py:44`; `app/templates/ponto/partials/painel_ausencia.html:125,163`; `app/ponto/views.py:2476,2910,2950,2993`; `app/ponto/services/ausencia.py:119,126-139,590`; `app/ponto/models.py:874-876`; `app/ponto/services/dia_decidido.py:259-264,465-469,648,665-675`; `app/ponto/services/atestados_lote.py:54-58,118-128`; `bin/sombra_incremental.sh:36`. Ocorrência em produção de falta de várias datas com o rótulo divergente do valor por dia [nao verificado].

## 23/11 13:37 · sessao c751a3c4fda1 · A-TELA-DO-DIA-LE-UM-JUIZ · sonda · origem=Code

**Script A — sombra.** O anexo registrou 14 dias com pago ≥ previsto e 111 dias com pago e minutos dos pares. O histograma usa a diferença com sinal, arredondada a nove casas, e inclui 10, 60 e 240 na faixa que termina nesse valor. O pago soma `horas_trabalhadas`, `horas_folga_trabalhada` e `detalhe.folga_feriado_min`, após a checagem das rubricas de sustentação. `sonda.out:4`, `sonda_pago_pares_sombra.py:130`, [app/ponto/services/dia_pago.py:861](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/services/dia_pago.py:861).

```python
# sonda_pago_pares_sombra.py
import json
import logging
from bisect import bisect_left
from collections import Counter
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from colaboradores.services.calendario import (
    STATUS_QUE_PEDEM_DECISAO,
    contexto_calendario,
)
from escala.utils import minutos_previstos_do_dia
from ponto.services.dia_pago import _RUBRICAS_DE_DINHEIRO, pago_do_dia
from ponto.turnos import CargaDaJanela, realizado_do_dia

MES, ANO = 11, 2026
LIMITES = (0, 10, 60, 240)
FAIXAS = ("<=0", "0-10", "10-60", "60-240", ">240")


def falhar_em_log_de_erro(_logger, registro):
    if registro.exc_info and registro.exc_info[1] is not None:
        _, erro, traceback = registro.exc_info
        raise erro.with_traceback(traceback)
    if registro.levelno >= logging.ERROR:
        raise RuntimeError(f"Erro registrado em {registro.pathname}:{registro.lineno}")


def ler_grade(colab, agora):
    ctx = contexto_calendario(
        colab, {"modo": "competencia", "mes": str(MES), "ano": str(ANO)},
        agora=agora,
    )
    for dia in ctx["dias"]:
        if not dia["em_aberto_perguntado"] or dia["resolver_dia_oferecido"] is None:
            raise RuntimeError(f"Grade sem resposta para pk={colab.pk}, dia={dia['data']}")
    return ctx


def fonte_do_pago(linha):
    return {
        "dia_pago_pk": linha.pk,
        "versao": linha.versao,
        "evento_origem": linha.evento_origem,
        "parcelas_somadas": [
            "horas_trabalhadas", "horas_folga_trabalhada", "detalhe.folga_feriado_min",
        ],
        "rubricas_horas": {campo: getattr(linha, campo) for campo in _RUBRICAS_DE_DINHEIRO},
        "detalhe": {"folga_feriado_min": (linha.detalhe or {}).get("folga_feriado_min")},
    }


def imprimir(obj):
    print(json.dumps(obj, ensure_ascii=False), flush=True)


def sondar():
    if connection.settings_dict["NAME"] != "sombra":
        raise RuntimeError("Use o banco de ensaio sombra.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transação própria.")

    with (
        patch.object(logging.Logger, "handle", falhar_em_log_de_erro),
        schema_context("juliani"),
        transaction.atomic(),
    ):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco/schema diferente de sombra/juliani.")

        agora = timezone.now()
        qs = Colaborador.objects.select_related("empresa", "posto__praca").order_by("pk")
        colabs = list(qs.filter(situacao="ativo"))
        comparacao = Counter()
        histograma = dict.fromkeys(FAIXAS, 0)
        diferencas = []
        for colab in colabs:
            ctx = ler_grade(colab, agora)
            turnos = None
            for dia in ctx["dias"]:
                status = dia["status"]
                texto = dia["palavra_dia"]
                if "Em aberto" not in texto or status in STATUS_QUE_PEDEM_DECISAO:
                    continue

                d = dia["data"]
                previsto = minutos_previstos_do_dia(colab, d)
                linha = dia["dia_pago"]
                horas = pago_do_dia(linha)
                if horas is None:
                    comparacao["dias_sem_minutos_pagos_sustentados"] += 1
                    continue
                pago = round(horas * 60, 9)
                ge = pago >= previsto
                comparacao["dias_pago_ge_previsto" if ge else "dias_pago_lt_previsto"] += 1

                if turnos is None:
                    turnos = CargaDaJanela([colab], ctx["data_ini"], ctx["data_fim"]).turnos(colab)
                pares = realizado_do_dia(colab, d, turnos=turnos, previsto_min=previsto).minutos
                chave_dia = {
                    "colaborador_pk": colab.pk,
                    "data": d.isoformat(),
                    "status": status,
                }
                if ge:
                    imprimir({
                        **chave_dia,
                        "texto": texto,
                        "batidas_do_turno": [
                            [timezone.localtime(b.timestamp).isoformat(), b.tipo]
                            for b in dia["batidas"]
                        ],
                        "minutos_pagos": pago,
                        "minutos_previstos": previsto,
                        "minutos_dos_pares": pares,
                        "fonte_pago": fonte_do_pago(linha),
                    })

                if pares is None:
                    comparacao["dias_com_pago_sem_minutos_dos_pares"] += 1
                    continue
                diferenca = round(pago - pares, 9)
                histograma[FAIXAS[bisect_left(LIMITES, diferenca)]] += 1
                diferencas.append({**chave_dia, "diferenca_minutos": diferenca})

    diferencas.sort(key=lambda item: (-item["diferenca_minutos"], item["colaborador_pk"], item["data"]))
    imprimir({
        "colaboradores_no_universo": len(colabs),
        "dias_pago_ge_previsto": comparacao["dias_pago_ge_previsto"],
        "dias_pago_lt_previsto": comparacao["dias_pago_lt_previsto"],
        "dias_sem_minutos_pagos_sustentados": comparacao["dias_sem_minutos_pagos_sustentados"],
        "dias_com_pago_sem_minutos_dos_pares": comparacao["dias_com_pago_sem_minutos_dos_pares"],
        "dias_com_pago_e_minutos_dos_pares": len(diferencas),
        "histograma_pago_menos_pares": histograma,
        "maiores_diferencas": diferencas[:10],
    })


sondar()
```

**Script B — produção.** Para `para="folha"`, o campo decisivo é `Ausencia.status`, com valor `aprovada`. Entre as aprovadas que cobrem a data, vence a de maior `data` e, no empate, de maior `pk`. Uma ausência anterior a novembro com `data_fim=NULL` só alcança novembro se o tipo tiver fim indeterminado no catálogo vivo. A sonda imprime essas linhas candidatas com `toca_novembro="[nao verificado]"`. [app/ponto/turnos.py:2528](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/turnos.py:2528), [app/ponto/turnos.py:2786](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/turnos.py:2786), [app/ponto/catalogo/ausencias.py:430](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/catalogo/ausencias.py:430).

Os rótulos de ausência dependem do catálogo vivo de `TipoAusencia`. `rotulo_de` consulta o efeito e, quando aplicável, a regra de parcialidade. `palavra_do_dia` recebe um veredito e pode consultar o nome do tipo. A dependência é do catálogo. Com as duas tabelas pedidas, a sonda emite `[nao verificado]` para os rótulos quando há linhas candidatas. `null` indica que a consulta encontrou zero candidatas naquele dia. [app/ponto/services/dia_decidido.py:217](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/services/dia_decidido.py:217), [app/ponto/services/dia_decidido.py:502](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/services/dia_decidido.py:502), [app/ponto/turnos.py:2602](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/turnos.py:2602), [app/ponto/catalogo/ausencias.py:240](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/catalogo/ausencias.py:240), `sonda_ausencias_941_prod.py:47`.

```python
# sonda_ausencias_941_prod.py
import json
import logging
from datetime import date
from unittest.mock import patch

from django.db import connection, transaction
from django.db.models import F, Q
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from ponto.models import Ausencia

INICIO, FIM = date(2026, 11, 1), date(2026, 11, 30)
DIAS = tuple(date(2026, 11, dia) for dia in range(16, 20))
NAO_VERIFICADO = "[nao verificado]"


def falhar_em_log_de_erro(_logger, registro):
    if registro.exc_info and registro.exc_info[1] is not None:
        _, erro, traceback = registro.exc_info
        raise erro.with_traceback(traceback)
    if registro.levelno >= logging.ERROR:
        raise RuntimeError(f"Erro registrado em {registro.pathname}:{registro.lineno}")


def iso(valor):
    return valor.isoformat() if valor is not None else None


def imprimir(obj):
    print(json.dumps(obj, ensure_ascii=False), flush=True)


def ler_dia(dia, ausencias):
    candidatas = [
        a for a in ausencias
        if a.data <= dia and (a.data_fim is None or dia <= a.data_fim)
    ]
    confirmadas = [a.pk for a in candidatas if a.data_fim is not None or a.data == dia]
    sem_fim_a_confirmar = [a.pk for a in candidatas if a.data_fim is None and a.data < dia]
    aprovadas = [a for a in candidatas if a.status == "aprovada"]
    cobertura_folha_pk = None
    if aprovadas:
        ultima = aprovadas[-1]
        cobertura_folha_pk = ultima.pk if ultima.pk in confirmadas else NAO_VERIFICADO
    rotulo = NAO_VERIFICADO if candidatas else None
    return {
        "data": dia.isoformat(),
        "ausencias_pks_confirmadas": confirmadas,
        "ausencias_pks_sem_fim_a_confirmar": sem_fim_a_confirmar,
        "ausencia_cobertura_folha_pk": cobertura_folha_pk,
        "rotulo_de": rotulo,
        "palavra_do_dia": rotulo,
    }


def sondar():
    if connection.settings_dict["NAME"] != "saas_hasner":
        raise RuntimeError("Use o banco de produção saas_hasner.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transação própria.")

    with (
        patch.object(logging.Logger, "handle", falhar_em_log_de_erro),
        schema_context("juliani"),
        transaction.atomic(),
    ):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("saas_hasner", "juliani"):
                raise RuntimeError("Banco/schema diferente de saas_hasner/juliani.")

        pk = Colaborador.objects.values_list("pk", flat=True).get(pk=941)
        ausencias = list(
            Ausencia.objects.filter(colaborador_id=pk, data__lte=FIM)
            .filter(
                Q(data_fim__isnull=True)
                | Q(data_fim__gte=INICIO, data__lte=F("data_fim"))
            )
            .only("tipo", "status", "data", "data_fim", "minutos", "criado_em", "atualizado_em")
            .order_by("data", "pk")
        )
        for a in ausencias:
            imprimir({
                "colaborador_pk": pk,
                "pk": a.pk,
                "tipo": a.tipo,
                "status": a.status,
                "data": iso(a.data),
                "data_fim": iso(a.data_fim),
                "minutos": a.minutos,
                "criado_em": iso(a.criado_em),
                "atualizado_em": iso(a.atualizado_em),
                "toca_novembro": (
                    True if a.data_fim is not None or a.data >= INICIO else NAO_VERIFICADO
                ),
            })
        for dia in DIAS:
            imprimir({"colaborador_pk": pk, **ler_dia(dia, ausencias)})


sondar()
```

Execução das duas novas sondas em banco e registros de `Ausencia` do 941 em produção [nao verificado].

## 23/11 13:25 · sessao 0ec30af5c5d8 · PAR-PELO-TURNO · leitura · origem=Code

lei: falta o corte que diz **qual vão `S→E` separa duas jornadas reais** para o PAR-PELO-TURNO. Os números existentes respondem a perguntas diferentes. **11h** é descanso entre jornadas citado no repo, **2h** classifica os blocos do turno previsto, **3h** é o teto padrão para inferir pausa, **6h** pode fazer o motor atribuir dois períodos ao mesmo dia e **previsto + 4h / 26h** limita o fechamento `E→S`. Qual desses critérios vale para juntar os blocos do aval fica **[nao verificado]**. `aval.txt:1` (anexo colado), `app/docs/mapa_seletor_cobranca.md:27`, `app/escala/servico_jornada.py:130`, `app/ponto/turnos.py:382`, `app/ponto/motor_calculo_v2.py:2400`, `app/ponto/turnos.py:1430`.

Leitura da cópia no diretório informado. A correspondência com o SHA está **[nao verificado]** porque a cópia não tem metadados Git.

## 1. Onde o par é decidido e por que sobram as marcações

`parear_turnos` ordena por instante e usa o papel da ata quando disponível, senão o tipo gravado. Pausas e ecos reconhecidos são tratados antes das bordas. Uma `E` de borda abre um turno e deixa uma abertura anterior sem saída. Uma `S` de borda fecha a abertura corrente, sujeita às guardas de duração e pausa. Sem abertura corrente, a `S` sai como `Turno` sem entrada. Um vão de **até 60 segundos** também deixa a `S` órfã, mantendo a abertura corrente disponível. `app/ponto/turnos.py:1192`, `app/ponto/turnos.py:1304`, `app/ponto/turnos.py:1377`, `app/ponto/turnos.py:1398`, `app/ponto/turnos.py:1482`, `app/ponto/turnos.py:117`, `app/ponto/management/commands/detectar_par_relampago.py:49`.

- **Teto da `S` que fecha.** Busca `hi/hf` em `marcos_por_dia` pela **data local da entrada**. Com duas bordas distintas, o teto é `(hf − hi) módulo 24h + 4h`, usando o vão previsto bruto. Com borda ausente ou coincidente, usa **26h**. Se **ambas** as pontas distam estritamente mais de **180 min** de seus respectivos marcos, a L-084 volta a usar **26h**. O ramo separa se o vão real for **maior** que o teto escolhido. As duas passagens finais ainda podem recuperar uma saída órfã antes da próxima entrada, inclusive pela exceção posicional, ou uma ida à pausa sem volta com fato encerrado. Ambas conferem o teto do dia, com `cont_max_s` de fallback. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:326`, `app/ponto/turnos.py:1430`, `app/ponto/motor_calculo_v2.py:1931`, `app/docs/LEIS.md:116`, `app/ponto/turnos.py:1029`, `app/ponto/turnos.py:1064`, `app/ponto/turnos.py:1165`.
- **Janela prevista e órfã da grade.** `janela_turno_de` monta os instantes previstos a partir de `hi/hf`, com a virada de data. A grade, no caminho sem alinhamento por papel, busca candidatas entre o primeiro marco **−90 min** e o último **+90 min**, e conserva sobras na lista `orfas`. Na reconciliação, só um par fechado pode recuperar suas bordas, com condições de marco faltante, presença na lista de órfãs, proximidade e L-084. Assim, uma órfã da grade pode existir mesmo quando o pareador fechou outro par no dia. `app/ponto/selecao_periodo.py:132`, `app/escala/utils.py:595`, `app/escala/utils.py:635`, `app/escala/utils.py:1288`, `app/escala/utils.py:1824`, `app/escala/utils.py:1955`, `app/escala/utils.py:2001`.
- **`pares_separados_pelo_teto` é leitor do resultado.** O ramo de fechamento carimba a `S` se o teto do dia foi decisivo e o vão era **≤26h**. O leitor exige que a `S` continue sem entrada e que a `E` carimbada continue aberta no resultado final. Compara por PK quando disponível. A lista Cadastro × Realidade consome esse resultado por `teto_na_lista`. `app/ponto/turnos.py:165`, `app/ponto/turnos.py:183`, `app/ponto/turnos.py:1447`, `app/ponto/services/teto_na_lista.py:47`.

| Caso do aval | O que a cópia permite concluir |
|---|---|
| **col129, 17:30 → 06:30** | A escala 21:30–05:42 tem vão de **492 min** e teto de **732 min**. O par tem **780 min**. As distâncias são **240 e 48 min**, então a L-084 não devolve o teto a 26h. Resultado do ramo: `E` aberta e `S` órfã, com carimbo. O teste da cópia fixa exatamente esses horários e esse resultado. `aval.txt:1`, `app/ponto/turnos.py:78`, `app/ponto/turnos.py:1440`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:52` |
| **col155, 17:29 → 06:32** | Com a mesma escala alimentada, são **783 min** contra **732 min**, com distâncias de **241 e 50 min**. A execução isolada do corpo do pareador com essas duas marcas deu a mesma separação. Os demais insumos do caso real ficam **[nao verificado]**. `aval.txt:1`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1447` |
| **col214, 07:00** | O aval confirma uma órfã. Ele não dá tipo, marca complementar, bordas previstas ou papel da ata. O ramo exato fica **[nao verificado]**. Uma `S` sem abertura e uma marca não atribuída pela grade são caminhos distintos que produzem sobras. `aval.txt:1`, `app/ponto/turnos.py:1495`, `app/escala/utils.py:1824` |
| **col107, 07:05 e 09:15 antes de 10:13–14:25** | As marcas antecedem o início informado em **188 e 58 min**. O ramo E/S não anexa pelo simples fato de estarem antes de um turno já fechado, e a reconciliação da grade recupera as próprias bordas do par. A marca das 09:15 estar a 58 min do início real não prova que estava a 58 min de um **marco previsto**. Tipos, DNA e ramo efetivo ficam **[nao verificado]**. `aval.txt:1`, `app/ponto/turnos.py:1417`, `app/ponto/turnos.py:1491`, `app/escala/utils.py:635`, `app/escala/utils.py:1985`, `app/escala/utils.py:2010` |

## 2. Limites escritos e quem os lê

| Candidata | Número e leitura atual |
|---|---|
| **Interjornada, art. 66** | **11h** no mapa de cobrança e no corte de **permuta** registrado em `PROMPTS.md`. O helper de cobertura cita expressamente que a validação de 11h está fora dele. Leitura pelo juiz do par ou por juiz vivo de interjornada **[nao verificado]**. `app/docs/mapa_seletor_cobranca.md:27`, `app/docs/PROMPTS.md:2015`, `app/ponto/utils.py:141` |
| **Intervalo previsto / turno partido, art. 71** | Vão declarado **≤120 min** é contínuo e **>120 min** é partido. O juiz `eh_turno_partido` usa `hii/hfi` circulares do desenho. Tem corte assinado e leitores de cadastro e rótulo de escala. O predicado classifica **dois blocos do mesmo turno previsto**. `app/escala/servico_jornada.py:114`, `app/escala/servico_jornada.py:130`, `app/escala/servico_jornada.py:193`, `app/escala/services/cadastro_tipo.py:113`, `app/core/juizes.py:228`, `app/docs/CORTES.md:359` |
| **Vão real `S→E` como pausa** | **3h** no pareador e no `MotorBase`, **8h** no `MotorPlantao`, **0** no `MotorTurnoPartido`, que desliga a inferência por duração. Há guardas de distância dos marcos e de continuidade. Com `hii` e `hfi` acesos, a ata pode declarar a pausa acima do teto de inferência. Para descontar como pausa gozada, `intervalos_do_turno` exige **15 min** ou mais. `app/ponto/turnos.py:382`, `app/ponto/turnos.py:1402`, `app/ponto/turnos.py:1466`, `app/ponto/turnos.py:1816`, `app/ponto/turnos.py:537`, `app/ponto/motor_calculo_v2.py:728`, `app/ponto/motor_calculo_v2.py:2859`, `app/ponto/motor_calculo_v2.py:3511` |
| **Duração total `E→S`** | **Previsto bruto + 4h** ou fallback de **26h** no fechamento principal, com a exceção L-084 de **180 min nas duas pontas**. `cont_max_s` vale **14h** por padrão e **26h** no plantão para guardas de absorção e fallback dos pós-fechos. A vigência do turno aberto tem outro relógio, **fim previsto + 4h**. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1192`, `app/ponto/turnos.py:1349`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:2021`, `app/ponto/motor_calculo_v2.py:3513` |
| **Vão entre períodos no motor** | **0 a 6h** desde uma saída real permite herdar `data_jornada` em `MotorBase.calcular_mes`. Esse trecho mantém os dois períodos e altera a atribuição ao dia. `app/ponto/motor_calculo_v2.py:2399`, `app/ponto/motor_calculo_v2.py:2423` |
| **Empresa, Sindicato e régua CCT** | Empresa tem piso de HE **0 min**, limite padrão de decisão **15 min** e tetos de Cadastro × Realidade **60 min** por ponta. Sindicato e `REGUA_LEGAL` têm intrajornada **mínima de 60 min** e teto de **prorrogação de 120 min**. A régua marca estes dois campos de Sindicato como sem efeito no cálculo e `get_motor_cct` injeta outros parâmetros. A aplicação por empresa/praça seleciona a convenção. Um campo que fixe o vão entre jornadas **[nao verificado]**. `app/colaboradores/models.py:58`, `app/colaboradores/models.py:69`, `app/colaboradores/models.py:101`, `app/core/models.py:63`, `app/core/models.py:119`, `app/core/regua_cct.py:39`, `app/core/regua_cct.py:72`, `app/core/regua_cct.py:352`, `app/core/regua_cct.py:428` |

## 3. Gestão de HE e desconto da pausa

- **Minutos e dias da tela.** `views.gestao_he` lê `he_pendente_lavrado.ler` e passa o retrato a `gestao_he.enriquecer`. A fonte da pendência é **`dia['he_fora_da_janela']`** de `espelho.montar_dias`. Com a janela empresarial ativa e vigente, ele publica as pontas da grade, complementa pontas faltantes pelo relato de Cadastro × Realidade do motor e incorpora o dia inteiro sem previsto quando o motor o publica. O retrato da competência soma as pontas por dia e é gravado em `MetricaSnapshot` por `lavrar_he_pendente`. `app/ponto/views.py:948`, `app/ponto/views.py:969`, `app/ponto/services/espelho.py:735`, `app/ponto/services/espelho.py:790`, `app/ponto/janela_he.py:194`, `app/ponto/janela_he.py:267`, `app/ponto/services/he_pendente_lavrado.py:55`, `app/ponto/services/he_pendente_lavrado.py:84`, `app/ponto/services/he_pendente_lavrado.py:132`, `app/ponto/services/he_pendente_lavrado.py:177`, `app/ponto/management/commands/lavrar_he_pendente.py:62`.
- **Decisão e DST-L1.** A porta é `ponto/portas/he.py::decidir_he` e o modelo é `DecisaoHE`, por pessoa e dia. `gestao_he.estado_por_dia` lê `objects.vigentes()` e `estado_do_dia` publica `sem_decisao` quando a chave está ausente. O espelho usa o mesmo estado e o link DST-L1 leva à Gestão de HE com pessoa e dia em foco quando há ponta sem decisão. Para folga ou feriado elegível, com previsto zero, janela ativa e período calculado, o motor publica como pendência os minutos que pagaria, já líquidos. O escritor exige minutos positivos. A entrada isolada que o corte DST-L1 manda à porta do admin, por si só, não produz esse período. `app/docs/CORTES.md:575`, `app/ponto/portas/he.py:159`, `app/ponto/portas/he.py:186`, `app/ponto/models.py:1265`, `app/ponto/models.py:1270`, `app/ponto/services/gestao_he.py:69`, `app/ponto/services/gestao_he.py:98`, `app/ponto/services/espelho.py:936`, `app/ponto/views.py:170`, `app/ponto/motor_calculo_v2.py:1422`, `app/ponto/adaptador_motor.py:24`.
- **Intervalo efetivo.** `intervalos_do_turno` entrega os pares internos julgados ao adaptador, por timestamp de entrada. O motor também pode receber pausas pela ata. `calcular_periodo` aplica a janela de HE às bordas, recorta os intervalos ao trecho efetivo e subtrai `_intra_real(intervalos)` da duração. O desconto aparece no **12x36 acima de 360 min**, no **comercial acima de 240 min**, e o **plantão** delega ao 12x36. A decisão de HE altera o clipe, com recálculo por evento. A configuração de HE e os minutos líquidos dos quatro casos ficam **[nao verificado]**. `app/ponto/turnos.py:502`, `app/ponto/adaptador_motor.py:26`, `app/ponto/motor_calculo_v2.py:685`, `app/ponto/motor_calculo_v2.py:1892`, `app/ponto/motor_calculo_v2.py:2759`, `app/ponto/motor_calculo_v2.py:3224`, `app/ponto/motor_calculo_v2.py:3534`, `app/ponto/portas/he.py:206`.

## 4. Censo AST e ponto comum

O AST encontrou **13 chamadas diretas** a `parear_turnos`, contando aliases e deixando fora testes e migrações. O documento anterior é referenciado no backlog e o RELATO confirma a mesma contagem histórica. O arquivo `logs/PAR-CONFIRMADO/censo.md` está ausente da cópia entregue e seu conteúdo fica **[nao verificado]**. `app/docs/BACKLOG.md:2242`, `app/docs/RELATO.md:2857`, `.gitignore:53`.

| Apelido | Chamada a `parear_turnos` |
|---|---|
| Fila por causa | `particionar` · `app/chamados/services/fila_por_causa.py:197` |
| Observado de meio período | `observado_meio_periodo` · `app/escala/services/meio_periodo.py:168` |
| Semana do posto | `_bateram` · `app/escala/services/semana_do_posto.py:120` |
| Reconciliar fantasmas | `_completude` · `app/ponto/management/commands/reconciliar_fantasmas.py:99` |
| Motor v2 | `turnos_via_autoridade` · `app/ponto/motor_calculo_v2.py:503` |
| Diagnóstico de escala | `diagnostico_escala` · `app/ponto/services/diagnostico_escala.py:142` |
| Lastro | `julgar` · `app/ponto/services/lastro.py:268` |
| Piso de intervalo | `pares_abaixo_do_piso` · `app/ponto/services/piso_intervalo.py:48` |
| Triagem da tela de bater | `triagem_bater_ponto`, alias `_pt` · `app/ponto/services/triagem_batida.py:178` |
| Batidas já carregadas | `turnos_de_batidas` · `app/ponto/turnos.py:1535` |
| Juiz do colaborador / lote | `_turnos_do_colab_puro` · `app/ponto/turnos.py:1997` |
| Turno aberto vivo | `_turno_aberto_puro` · `app/ponto/turnos.py:2295` |
| Bloqueio de órfão | `detectar_bloqueio_turno_orfao` · `app/ponto/utils_turno_orfao.py:31` |

**`batidas_apuraveis` teve 57 chamadas diretas**, incluindo um script de `bin/`. A função seleciona marcações sem retratação e aplica os cortes de instante solicitados. A casca avulsa `turnos_do_colab` usa uma consulta própria com filtro de retratação e janela com bordas, enquanto `CargaDaJanela` chama a porta diretamente. `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:3020`, `app/ponto/turnos.py:3038`, `bin/r3_turno_aberto_alcanca.py:61`.

| Apelidos dos leitores de `batidas_apuraveis` | Sítios |
|---|---|
| APIs e painel | Bater e justificativas `app/api/views.py:531`, `app/api/views.py:2030`, `app/api/views_core.py:517`. Painel operacional e dashboard `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:269`, `app/core/views.py:114`. Detalhe `app/colaboradores/services/detalhe.py:49`. |
| Chamados | Fato `app/chamados/juizes.py:1825`. Fio `app/chamados/services/fio.py:1673`, `app/chamados/services/fio.py:2054`. Vizinhas `app/chamados/services/materializacao.py:112`, `app/chamados/services/materializacao.py:115`. Tarefa e validação `app/chamados/services/tarefa_do_dia.py:443`, `app/chamados/services/validacao.py:1207`. |
| Escala e grade | Defesa `app/escala/regua_defesa.py:153`, `app/escala/regua_defesa.py:523`. Fase, previsto cego, posto e grade `app/escala/services/fase_na_tela.py:47`, `app/escala/services/previsto_cego.py:159`, `app/escala/services/semana_do_posto.py:117`, `app/escala/utils.py:1495`. |
| Folha, férias, pauta e relatórios | `app/folha/export.py:499`, `app/folha/export.py:914`, `app/folha/previa.py:58`, `app/ferias/management/commands/listar_ferias_com_batida.py:47`, `app/pautas/services.py:431`, `app/relatorios/pdf_espelho.py:272`, `app/relatorios/views.py:502`, `app/relatorios/management/commands/diff_extrato_parcial.py:37`. |
| Operação do ponto | Abertura, cartório, cobertura, deslogados e dia `app/ponto/services/abriu_nao_bateu.py:174`, `app/ponto/services/cartorio.py:1201`, `app/ponto/services/cobertura.py:301`, `app/ponto/services/deslogados.py:172`, `app/ponto/services/dia_do_colab.py:60`. Diagnóstico, passivo, esmeril, espelho, faltas e fechamento `app/ponto/services/diagnostico_escala.py:142`, `app/ponto/services/dias_em_aberto.py:122`, `app/ponto/services/esmeril_espelho.py:165`, `app/ponto/services/espelho.py:1058`, `app/ponto/services/faltas_de_hoje.py:57`, `app/ponto/services/fechamento.py:633`, `app/ponto/services/fechamento.py:811`. Triagem, vigia, lote e bloqueio `app/ponto/services/triagem_batida.py:175`, `app/ponto/services/triagem_batida.py:666`, `app/ponto/services/triagem_batida.py:716`, `app/ponto/services/vigia_de_hora.py:79`, `app/ponto/turnos.py:3020`, `app/ponto/turnos.py:3038`, `app/ponto/utils_turno_orfao.py:28`. |
| Comandos de ponto e script | `app/ponto/management/commands/aplicar_09_corte_b.py:126`, `app/ponto/management/commands/censo_col900.py:128`, `app/ponto/management/commands/diff_espelho_alimentacao.py:92`, `app/ponto/management/commands/e6_oraculo.py:152`, `app/ponto/management/commands/e6_oraculo.py:212`, `app/ponto/management/commands/mapa_divergencia.py:96`, `app/ponto/management/commands/reconciliar_fantasmas.py:96`, `app/ponto/management/commands/tripwire_celulas.py:283`, `app/ponto/management/commands/vigiar_motor_autoridade.py:54`, `bin/r3_turno_aberto_alcanca.py:61`. |

**Leitores indiretos e alcance do ponto único.** Espelho e cartão PDF consultam `turnos_do_colab` e também leem a grade. O cartório monta a grade e passa o dia a `supra_juiz.classificar_dia`, que consome `dia['orfas']`. Nos chamados, `perguntar_entradas_pendentes` resolve o dia pelo juiz do turno e consulta `classificar_falta`. `_orfao_resolvido_cross_meianoite` lê os pares fechados, com guarda própria de 26h. O núcleo materializa `TurnoMaterializado` a partir de `turnos_do_colab`. `app/ponto/services/espelho.py:520`, `app/ponto/services/espelho.py:625`, `app/relatorios/pdf_espelho.py:211`, `app/relatorios/pdf_espelho.py:475`, `app/ponto/services/cartorio.py:1201`, `app/ponto/services/cartorio.py:1500`, `app/ponto/supra_juiz.py:522`, `app/chamados/services/disputa_emissao.py:257`, `app/chamados/services/disputa_emissao.py:721`, `app/ponto/nucleo.py:40`.

**O sítio comum da regra geométrica é `parear_turnos`, com `_teto_da_jornada` compartilhado pelos fechamentos.** A mesma resposta para todos exige também a mesma alimentação e o consumo do par devolvido. Há chamadas sem DNA ou ata, `turnos_do_colab` segmenta a janela, e o v2 monta períodos por `turnos_via_autoridade` e pode substituí-los por `juiz_batida.periodos_do_dia`. Ele obtém o mapa de dia em outra chamada e ainda aplica a herança de 6h. Portanto, mudar só o teto alcança a regra compartilhada, mas não garante par idêntico em todos os leitores. `MOTOR-LE-O-TURNO` já registra a convergência de motor e DiaPago para o par e o dia de `turnos_do_colab`. O arquivo do motor é zona inviolável e o aval registrado é para esse escopo. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1997`, `app/ponto/utils_turno_orfao.py:31`, `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:611`, `app/ponto/motor_calculo_v2.py:679`, `app/ponto/motor_calculo_v2.py:2346`, `app/ponto/motor_calculo_v2.py:2400`, `app/core/juizes.py:215`, `app/docs/BACKLOG.md:2173`, `CLAUDE.md:142`.

## 5. Lista de órfãs e vigia

**Existe lista real por dia.** A grade publica `dia['orfas']`, o cartório grava as horas e `n_orfas` em `CelulaDia.ata`, e `grade_da_celula` as devolve por `_orfas_da_ata`. `datas_saida_orfa` é uma leitura mais estreita, só dos dias de turno em que o pareador devolveu uma `S` sem `E`. `app/escala/utils.py:1818`, `app/escala/utils.py:1824`, `app/ponto/services/cartorio.py:755`, `app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:420`, `app/ponto/turnos.py:234`.

O vigia `vigia_pareador_celula.censo` lista `(colab, dia_turno, borda, hora)` quando **a própria borda de um par fechado** consta como órfã na ata e sua lâmpada correspondente está apagada. Ele publica `cego` quando falta universo observável. `tripwire_celulas.medir_sobra_com_tipo_trocado` mede outra classe específica, com órfã no raio de ponta apagada, confirmação por instante e par-relâmpago de **até 10 min**. Um contador geral de **órfã contígua a qualquer turno** e o **zero da frota da comp. 11** ficam **[nao verificado]**. `app/ponto/services/vigia_pareador_celula.py:42`, `app/ponto/services/vigia_pareador_celula.py:93`, `app/ponto/management/commands/tripwire_celulas.py:199`, `aval.txt:1`.

## 6. DNA, ata e competência exportada

- **DNA e janela.** `CelulaDia.dna.marcos` congela o previsto. `EscalaColaborador.marcos_do_dia` lê o DNA e cai no template na ausência de marcos. O lote `marcos_dna_periodo` alimenta o teto do pareador quando disponível. Há caminhos que usam o template, inclusive a guarda de segundo intervalo na alimentação do pareador. A janela prevista pode vir do DNA, enquanto a janela de consulta de `turnos_do_colab` é ou

[truncado em 20000 de 22133 caracteres]

## 23/11 13:19 · sessao a29591adb793 · A-TELA-DO-DIA-LE-UM-JUIZ · leitura · origem=Code

```python
import json
import logging
from collections import Counter
from datetime import date
from math import fsum
from unittest.mock import patch

from django.db import connection, transaction
from django.db.models import Max
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from colaboradores.services.calendario import (
    STATUS_QUE_PEDEM_DECISAO,
    contexto_calendario,
)
from escala.alimentacao import celulas_do_periodo
from escala.utils import minutos_previstos_do_dia
from folha.export import competencia_entregue
from ponto.janelas import janela_atual, janela_fechamento
from ponto.models import Batida
from ponto.services.dia_pago import pago_do_dia
from ponto.turnos import CargaDaJanela, cobertura_ausencia_periodo, realizado_do_dia

MES, ANO = 11, 2026
DIAS_AUSENCIA_941 = tuple(date(2026, 11, d) for d in range(16, 20))
CASOS = {
    945: (date(2026, 10, 24), date(2026, 10, 26), date(2026, 10, 28)),
    941: (date(2026, 10, 31), *DIAS_AUSENCIA_941),
}


def falhar_em_log_de_erro(_logger, registro):
    # A tela tem fallbacks com exc_info, por exemplo em app/colaboradores/services/calendario.py:622.
    if registro.exc_info and registro.exc_info[1] is not None:
        _, erro, traceback = registro.exc_info
        raise erro.with_traceback(traceback)
    if registro.levelno >= logging.ERROR:
        raise RuntimeError(f"Erro registrado em {registro.pathname}:{registro.lineno}")


def ler_grade(colab, mes, ano, agora):
    ctx = contexto_calendario(
        colab, {"modo": "competencia", "mes": str(mes), "ano": str(ano)},
        agora=agora,
    )
    for dia in ctx["dias"]:
        if not dia["em_aberto_perguntado"] or dia["resolver_dia_oferecido"] is None:
            raise RuntimeError(f"Grade sem resposta para pk={colab.pk}, dia={dia['data']}")
    return ctx


def minutos_pagos(dia):
    horas = pago_do_dia(dia["dia_pago"])
    return None if horas is None else round(horas * 60, 9)


def batidas(batidas_da_grade):
    return [
        [timezone.localtime(b.timestamp).isoformat(), b.tipo]
        for b in batidas_da_grade
    ]


def registro_ausencia(a):
    if a is None:
        return None
    return {
        "pk": a.pk,
        "data": a.data.isoformat(),
        "data_fim": a.data_fim.isoformat() if a.data_fim is not None else None,
        "minutos": a.minutos,
        "tipo": a.tipo,
        "status": a.status,
    }


def imprimir(obj):
    print(json.dumps(obj, ensure_ascii=False), flush=True)


def sondar():
    if connection.settings_dict["NAME"] != "sombra":
        raise RuntimeError("Use o banco de ensaio sombra.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transação própria.")

    with (
        patch.object(logging.Logger, "handle", falhar_em_log_de_erro),
        schema_context("juliani"),
        transaction.atomic(),
    ):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco/schema diferente de sombra/juliani.")

        agora = timezone.now()
        ultima = Batida.objects.aggregate(ultima=Max("timestamp"))["ultima"]
        qs = Colaborador.objects.select_related("empresa", "posto__praca").order_by("pk")
        colabs = list(qs.filter(situacao="ativo"))
        por_pk = {c.pk: c for c in colabs}
        colabs_dos_casos = {
            pk: por_pk[pk] if pk in por_pk else qs.get(pk=pk)
            for pk in CASOS
        }
        empresas = {
            c.empresa_id: c.empresa for c in [*colabs, *colabs_dos_casos.values()]
        }
        cabecalho_empresas = []
        for pk, empresa in sorted(empresas.items()):
            ini, fim = janela_fechamento(MES, ANO, empresa)
            cabecalho_empresas.append({
                "empresa_pk": pk,
                "inicio": ini.isoformat(),
                "fim": fim.isoformat(),
                "exportada": competencia_entregue(pk, MES, ANO),
            })
        imprimir({
            "dump_referencia_max_batida_timestamp": (
                timezone.localtime(ultima).isoformat() if ultima is not None else None
            ),
            "competencia": "2026-11",
            "empresas": cabecalho_empresas,
            "colaboradores_no_universo": len(colabs),
        })

        por_status = Counter()
        afetados = 0
        comparacao = Counter()
        excessos = []
        grades_dos_casos = {}
        for colab in colabs:
            ctx = ler_grade(colab, MES, ANO, agora)
            if colab.pk in CASOS:
                grades_dos_casos[colab.pk] = {
                    (MES, ANO): {dia["data"]: dia for dia in ctx["dias"]}
                }
            divergentes = [
                dia for dia in ctx["dias"]
                if "Em aberto" in dia["palavra_dia"]
                and dia["status"] not in STATUS_QUE_PEDEM_DECISAO
            ]
            if not divergentes:
                continue
            afetados += 1
            turnos = None
            for dia in divergentes:
                por_status[dia["status"]] += 1
                d = dia["data"]
                previsto = minutos_previstos_do_dia(colab, d)
                pago = minutos_pagos(dia)
                if pago is None:
                    comparacao["sem_pago"] += 1
                    continue
                comparacao["ge" if pago >= previsto else "lt"] += 1
                if turnos is None:
                    turnos = CargaDaJanela([colab], ctx["data_ini"], ctx["data_fim"]).turnos(colab)
                realizado = realizado_do_dia(colab, d, turnos=turnos, previsto_min=previsto)
                if realizado.minutos is None:
                    comparacao["sem_realizado_dos_pares"] += 1
                else:
                    excessos.append(max(0, pago - realizado.minutos))

        saida_casos = []
        for pk, datas in CASOS.items():
            colab = colabs_dos_casos[pk]
            grades = grades_dos_casos.get(pk, {})
            celulas = celulas_do_periodo([pk], min(datas), max(datas))
            cobertura = (
                cobertura_ausencia_periodo(
                    colab, min(DIAS_AUSENCIA_941), max(DIAS_AUSENCIA_941), para="folha",
                ) if pk == 941 else {}
            )
            for d in datas:
                _, fim = janela_atual(d, colab.empresa)
                competencia = (fim.month, fim.year)
                if competencia not in grades:
                    ctx = ler_grade(colab, fim.month, fim.year, agora)
                    grades[competencia] = {dia["data"]: dia for dia in ctx["dias"]}
                dia = grades[competencia][d]
                celula = celulas.get((pk, d))
                caso = {
                    "colaborador_pk": pk,
                    "data": d.isoformat(),
                    "status": dia["status"],
                    "texto": dia["palavra_dia"],
                    "resolver_dia_oferecido": dia["resolver_dia_oferecido"],
                    "batidas_do_turno": batidas(dia["batidas"]),
                    "batidas_retratadas_do_dia_civil": batidas(dia["retratadas"]),
                    "celula_status_dia": celula.status_dia if celula is not None else None,
                    "minutos_pagos": minutos_pagos(dia),
                    "minutos_previstos": minutos_previstos_do_dia(colab, d, celulas=celulas),
                }
                if pk == 941 and d in DIAS_AUSENCIA_941:
                    caso["ausencia_cobertura_folha"] = registro_ausencia(cobertura.get(d))
                saida_casos.append(caso)

    imprimir({
        "dias_divergentes_por_status": dict(sorted(por_status.items())),
        "colaboradores_com_divergencia": afetados,
        "divergencia_status_x_em_aberto_total": sum(por_status.values()),
        "dias_pago_ge_previsto": comparacao["ge"],
        "dias_pago_lt_previsto": comparacao["lt"],
        "dias_sem_minutos_pagos_sustentados": comparacao["sem_pago"],
        "dias_com_pago_sem_minutos_dos_pares": comparacao["sem_realizado_dos_pares"],
        "dias_com_pago_e_minutos_dos_pares": len(excessos),
        "soma_minutos_pagos_acima_dos_pares": round(fsum(excessos), 9),
    })
    for caso in saida_casos:
        imprimir(caso)


sondar()
```

**Sítios no repositório**

- **Universo.** Adotei a lista padrão de Colaboradores, `situacao="ativo"`, com todas as páginas e sem filtros opcionais. As contagens usam esse universo. Os casos literais são consultados por PK mesmo quando estão fora dele, como permite a entrada do calendário do perfil. `app/colaboradores/views.py:1044`, `app/colaboradores/views.py:1048`, `app/colaboradores/views.py:1066`, `app/colaboradores/views.py:1077`, `app/colaboradores/views.py:1984`.
- **Calendário.** `contexto_calendario` recebe o colaborador, um dicionário de parâmetros e um instante comum à rodada. É a chamada direta usada pelo drawer, sem request ou usuário para produzir os campos pedidos. A sonda lê `palavra_dia`, `status` e `resolver_dia_oferecido` da própria grade. A permissão do usuário é aplicada pelo template ao exibir o botão. `app/colaboradores/services/drawer.py:122`, `app/colaboradores/services/calendario.py:188`, `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:754`, `app/colaboradores/services/calendario.py:809`, `app/templates/colaboradores/partials/_calendario_grade.html:68`, `app/templates/colaboradores/partials/_calendario_grade.html:134`.
- **Competência e exportação.** `janela_fechamento` usa o corte da empresa. `janela_atual` localiza a competência de cada data literal. `competencia_entregue` verifica o TXT vigente para empresa, mês e ano. `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`, `app/colaboradores/services/calendario.py:204`, `app/folha/export.py:556`, `app/folha/export.py:588`.
- **Pago e previsto.** A grade chama `linhas_do_periodo` e `rubricas_do_dia` e entrega o próprio `DiaPago`. Sobre essa linha, `pago_do_dia` lê as horas pagas de trabalho, incluindo folga trabalhada e folga em feriado. A sonda conta separadamente os dias em que o leitor devolve `None`. O previsto vem de `minutos_previstos_do_dia`. `app/colaboradores/services/calendario.py:319`, `app/colaboradores/services/calendario.py:688`, `app/colaboradores/services/calendario.py:781`, `app/ponto/services/dia_pago.py:833`, `app/ponto/services/dia_pago.py:867`, `app/ponto/services/dia_pago.py:901`, `app/escala/utils.py:2241`.
- **Pares fechados.** `CargaDaJanela.turnos` usa o mesmo carregador da grade. `realizado_do_dia` fornece os minutos realizados pelo pareador, como no espelho. A soma acumula diferenças positivas nos dias em que ambas as fontes devolvem minutos. `app/colaboradores/services/calendario.py:249`, `app/ponto/turnos.py:2980`, `app/ponto/turnos.py:3044`, `app/ponto/turnos.py:774`, `app/ponto/services/espelho.py:276`, `app/ponto/services/espelho.py:412`.
- **Casos literais.** `celulas_do_periodo` carrega a `CelulaDia` e seu `status_dia` persistido. As batidas seguem a grade, que atribui as vivas ao dia do turno e as retratadas ao dia civil local. `cobertura_ausencia_periodo(..., para="folha")` usa a mesma cobertura aprovada que alimenta a palavra do dia. `app/escala/alimentacao.py:54`, `app/escala/models.py:1827`, `app/colaboradores/services/calendario.py:643`, `app/colaboradores/services/calendario.py:658`, `app/colaboradores/services/calendario.py:765`, `app/colaboradores/services/calendario.py:561`, `app/colaboradores/services/calendario.py:748`, `app/ponto/turnos.py:2752`, `app/ponto/models.py:749`, `app/ponto/models.py:809`.
- **Sombra e erros.** A configuração `config.settings.sombra` aponta para o banco `sombra`, com schema `juliani`. A transação segue o padrão `REPEATABLE READ, READ ONLY` da sonda da cópia. O tratamento de log relança as exceções registradas com `exc_info` nas degradações da tela. O cabeçalho usa o maior `Batida.timestamp` como referência de data do dump. `app/config/settings/sombra.py:13`, `app/config/settings/sombra.py:29`, `app/config/settings/sombra.py:47`, `app/chamados/tests/cef_c3_sonda.py:21`, `app/colaboradores/services/calendario.py:509`, `app/colaboradores/services/calendario.py:617`, `app/ponto/models.py:23`.

Execução na sombra, contagens, data efetiva do dump e confirmação independente do hash [não verificado].

## 23/11 13:18 · sessao c69dd7bd31d0 · VIGIA-DO-TMUX · leitura · origem=Code

Usei a cópia no diretório `eac7a67419eb54c3af01f061dde311b5bae06717`. O procedimento do repo entrega essas cópias via `git archive`. A confirmação independente do hash por Git e o estado atual do host ficam [nao verificado]. As linhas dos anexos contam a partir do primeiro conteúdo de cada anexo. [CLAUDE.md:354](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/CLAUDE.md:354)

## 1. O contrato cobre o aval?

**Parcialmente.** Ele cobre o agendamento, os dois caminhos de ação e os registros. Faltam condições explícitas para autorizar o `/clear` e critérios do pouso. O contrato também acrescenta escrita no RELATO quando nenhuma tecla foi enviada. `aval.txt:1`, `contrato.txt:2–6`. [nao verificado]

| Exigência ou proibição do aval | Cobertura no contrato |
|---|---|
| Descongelar só esta obra. Manter `AUTO-CLEAR-NO-MARCO` congelado e substituído. | **Lacuna.** As cinco linhas descrevem o vigia novo sem declarar o limite do descongelamento ou a substituição. O item antigo consta no BACKLOG da cópia. `aval.txt:1`, `contrato.txt:2–6`. [app/docs/BACKLOG.md:2853](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/docs/BACKLOG.md:2853) |
| Vigia externo, somente na sessão tmux `code`, lendo o topo a cada poucos minutos. Reversível ao desligar. | **Coberto no desenho.** Cron de host a cada 3 min, nenhum chamado pelo Code, leitura das 15 primeiras linhas e pausa por arquivo. Retirar a declaração exige reinstalar o crontab para surtir efeito. `aval.txt:1`, `contrato.txt:2–3,6`. [bin/crons.sh:29](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/crons.sh:29) |
| `ESPERANDO` com produtor morto → frase exata com PID e arquivo. | **Coberto no texto.** A frase é literal. A prova proposta, `kill -0`, consulta o PID e deixa aberta a identidade do produtor. O Stop atual também admite espera com só `pid=` ou só `arquivo=`. A frase do vigia exige os dois. `aval.txt:1`, `contrato.txt:3–4`. [bin/hook_stop_fila1.py:809](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:809) |
| `/clear` só com `PAREI: ! CLEAR` e “nada roda -- PODE /clear” escrito pelo próprio Code. Depois, ler `HANDOFF-SESSAO` e o topo do RELATO, seguir a fila, parar só em lei ou `!`. | **Parcial.** O item 2 exige o texto autorizador e ordena `/clear` antes da retomada. Falta exigir a autoria do Code e fixar o conteúdo da retomada. O Stop da cópia orienta escrever `PAREI: ! CLEAR \| espera Ronald`, sem a autorização literal nova. `aval.txt:1`, `contrato.txt:3`. [bin/hook_stop_fila1.py:1116](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:1116) |
| Digitar somente o autorizado, nunca no meio de turno ativo, e preservar a config do Code. | **Parcial.** “Nada mais é digitado” é explícito. A guarda de turno ativo fica sem definição do indicador e da caixa vazia. A proibição de mexer na config do Code fica implícita no desenho de host. `aval.txt:1`, `contrato.txt:2–4`. |
| Registrar cada ação no RELATO e em log próprio. Nunca repetir o empurrão da mesma linha. | **Previsto.** O item 4 dá os dois destinos. O item 3 dá o SHA da linha, mas omite a coordenação entre a tecla e a gravação do estado. **Além do aval**, o item 4 manda gravar também cada adiamento e recusa nos dois destinos. `aval.txt:1`, `contrato.txt:4–5`. |
| Contrato de cinco linhas **no RELATO**, selo de host que morde os dois casos e o caso sem ação, pouso próprio e no ar antes da noite se couber no teto. | **Parcial.** O anexo tem os cinco campos e especifica o selo com tmux falso e os três grupos de casos. Ainda faltam no texto o pouso próprio e a condição de prazo e vaga. Registro no RELATO, selo executado, pouso e vaga real [nao verificado]. `aval.txt:1`, `contrato.txt:2–6`. [app/docs/PISTA.md:170](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/docs/PISTA.md:170), [CLAUDE.md:297](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/CLAUDE.md:297), [CLAUDE.md:331](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/CLAUDE.md:331) |

## 2. Onde entra o cron de host de 3 minutos?

- **Aceita comando fora do Django.** `C(...)` gera `tenant_command`. `RAW(...)` aceita script de host e `linhas()` o passa ao `cron_run.sh`, que executa o comando e mede a corrida. `bin/crons.sh` usa Django para **renderizar** e instala o texto resultante no crontab do host. `vigia_arvore.sh` já entra por `RAW`, na hora cheia mais cinco minutos. O alarme da sessão por `RAW` é o precedente mais próximo. [app/config/crons.py:19](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:19), [app/config/crons.py:102](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:102), [app/config/crons.py:292](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:292), [app/config/crons.py:311](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:311), [app/config/crons.py:1895](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:1895), [bin/cron_run.sh:5](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/cron_run.sh:5), [bin/crons.sh:23](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/crons.sh:23)
- **O sítio do contrato é `CRONS` em `app/config/crons.py`, com `RAW('*/3 * * * *', ...)`.** Conferi em memória que `log='logs/vigia_tmux/vigia.log'` resolve para o caminho previsto. Um relativo com barra é resolvido a partir da raiz do repo, então `vigia_tmux/vigia.log` cairia fora de `logs/`. `vigia_esteira.sh` tem outro mecanismo, uma unidade systemd de usuário declarada em `AGENDADOS_FORA_DO_CRON`. O `OnCalendar` versionado é `*:0/5`, embora o comentário diga 10 min. `contrato.txt:2,5`. [app/config/crons.py:1888](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:1888), [app/config/crons.py:760](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:760), [bin/hasner-esteira.timer:1](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hasner-esteira.timer:1), [bin/hasner-esteira.service:13](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hasner-esteira.service:13)
- **O que fica vermelho com só a entrada nova.** `test_contract_tabuleiro` cobra `vigia_tmux.py` em `PAPEL_DO_CRON`. `test_contract_crons` cobra o mesmo nome em `DUR_MAX_S`, por duração medida ou declarada. O gerador do diagrama consulta `papel()` e falha se faltar a chave. Com papel `vigia`, mudam a contagem do Tabuleiro e o bloco Vigias de `ARQUITETURA.mmd`. O selo compara `ARQUITETURA.mmd` e `MAPA.md` com o gerador. O bloco do pipeline diário pula `RAW`. `CRONS.md` pede atualização no mesmo commit como tabela de consulta. Um selo automático de paridade para `CRONS.md` [nao verificado]. [app/core/tests/test_contract_tabuleiro.py:15](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/tests/test_contract_tabuleiro.py:15), [app/chamados/tests/test_contract_crons.py:369](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/chamados/tests/test_contract_crons.py:369), [app/config/crons.py:2375](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:2375), [app/core/management/commands/gerar_diagrama.py:163](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/gerar_diagrama.py:163), [app/core/management/commands/gerar_diagrama.py:242](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/gerar_diagrama.py:242), [app/core/management/commands/gerar_diagrama.py:395](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/gerar_diagrama.py:395), [app/core/management/commands/gerar_diagrama.py:921](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/gerar_diagrama.py:921), [app/docs/CRONS.md:7](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/docs/CRONS.md:7)
- **Há uma trava de pouso separada.** O render integral da cópia, sem alteração, recusou `sombra.sh`: 87 min empurrariam o início para 03:56, antes do piso 04:05. A marca CRON-15 está documentada até **27/11/2026** e veda cron novo enquanto existir. O `install` pode tecnicamente preservar a linha existente da sombra sob condições estritas. O aval fornecido cita o descongelamento de `CONGELA-INSTRUMENTO`, e uma exceção à CRON-15 para esta obra fica [nao verificado]. `aval.txt:1`. [app/config/crons_duracao.json:78](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons_duracao.json:78), [app/config/crons.py:259](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:259), [app/core/tests/marca_cron15.py:7](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/tests/marca_cron15.py:7), [app/core/management/commands/crons.py:91](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/crons.py:91)

## 3. Como o repo reconhece turno ativo no pane?

- **O texto já usado é `esc to interrupt`.** `keepalive.sh` procura nas últimas 25 linhas e também reconhece `background agent`, `shell(s) still running`, `◯ ` e contagem de shells. `alarme_sessao_ociosa.py` pega o primeiro pane cujo comando aparece como `claude`, procura `esc to interrupt` e usa também a idade do transcript para decidir se avisa. São precedentes de heurística. O marcador da versão em produção e um reconhecedor confiável de caixa de entrada vazia ficam [nao verificado]. [bin/keepalive.sh:3](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/keepalive.sh:3), [bin/alarme_sessao_ociosa.py:48](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/alarme_sessao_ociosa.py:48), [bin/alarme_sessao_ociosa.py:66](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/alarme_sessao_ociosa.py:66), [bin/alarme_sessao_ociosa.py:144](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/alarme_sessao_ociosa.py:144)
- **Falsos “ocioso” possíveis.** O texto pode ficar fora da janela, mudar ou faltar durante uma transição. Uma captura com erro pode parecer vazia se o leitor olhar só o `stdout`. O alvo `-t code` ainda pode casar por prefixo com `codex-hasner` quando `code` está ausente, caso reproduzido no selo com tmux falso. `gerar_estado.py` usa `=code:` para casar o nome exato da sessão. Os hooks existentes consultam transcript, processos ou arquivos para suas decisões, conforme a porta. [bin/keepalive.sh:5](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/keepalive.sh:5), [bin/alarme_sessao_ociosa.py:69](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/alarme_sessao_ociosa.py:69), [bin/gerar_estado.py:17](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/gerar_estado.py:17), [bin/tests/test_painel_e_do_code.sh:19](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/tests/test_painel_e_do_code.sh:19), [bin/hook_stop_fila1.py:382](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:382), [bin/hook_stop_fila1.py:936](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:936)

## 4. Quais falhas ficam abertas?

| Falha | O que fica sem regra |
|---|---|
| **Vigia e Code escrevem no RELATO ao mesmo tempo** | O vigia pode agir sobre uma autorização já retirada ou sobrescrever uma edição feita depois da leitura. A porta existente serializa os escritores que a usam com `flock` e `os.replace`. O próprio código registra que a edição manual e `gerar_cortes.py` ficam fora dessa trava. O contrato também omite trava entre dois ticks e o que fazer se a tecla sair antes de falhar a gravação do SHA. `contrato.txt:3–5`. [app/core/esteira_vigia.py:834](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/esteira_vigia.py:834), [app/core/esteira_vigia.py:915](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/esteira_vigia.py:915) |
| **Code declara PID de outro processo** | Se o PID errado estiver vivo, o empurrão pode ficar retido. Se estiver morto, o vigia pode anunciar que o produtor terminou sem prova. O Stop já compara executável e `argv` com o produtor declarado, verifica ancestrais e distingue leitura falha de PID morto. `contrato.txt:3–4`. [bin/hook_stop_fila1.py:577](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:577), [bin/hook_stop_fila1.py:599](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:599) |
| **PID reciclado** | Um novo processo no mesmo número pode segurar a retomada. O Stop confere `starttime` antes e depois de ler o processo, o que pega a troca durante a consulta. O contrato do vigia carece de uma identidade registrada na declaração para distinguir a instância original de outra que já ocupava o número antes da consulta. `contrato.txt:3–4`. [bin/hook_stop_fila1.py:570](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:570), [bin/hook_stop_fila1.py:610](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:610) |
| **`/clear` no meio de texto digitado** | A caixa pode estar vazia na captura e receber texto antes do `send-keys`. A checagem do item 3 cobre só o instante observado. O contrato deixa em aberto como tratar essa janela entre checar e digitar. `contrato.txt:3–4`. [bin/keepalive.sh:5](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/keepalive.sh:5) |
| **Retomada antes de o `/clear` terminar** | “Espera o prompt vazio” carece de um sinal que confirme a transição, de um timeout e de uma regra para o caso em que só o `/clear` saiu. Já existe `SessionStart` com matcher `clear`, que lê o handoff e grava hora, origem e sessão em `ultimo_clear.json`. É evidência de início da nova sessão. A prontidão do pane após o hook fica [nao verificado]. `contrato.txt:3–5`. [.claude/settings.json:48](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/.claude/settings.json:48), [bin/marco_de_clear.py:19](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/marco_de_clear.py:19), [bin/marco_de_clear.py:43](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/marco_de_clear.py:43) |

## 5. Quem escreve no topo e o que `VIGIA-TMUX` quebra?

- **O escritor instrumental do topo útil é o disjuntor.** `disjuntor._parei_censo_no_relato` usa `esteira_vigia.no_topo` com `hook_stop_fila1.painel_do_topo` e relê pela porta do Stop para provar a escrita. A faixa acima do painel só reconhece o literal `PAREI: censo-<ID> | espera Codex`. O Stop lê esse topo e grava sua própria trilha à parte. `pos_push.sh` grava o topo do **TICKETS**. `gerar_cortes.py` reescreve o bloco `SEUS-CORTES` entre marcadores e, se faltarem, o insere antes da primeira seção `## `. [bin/disjuntor.py:299](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/disjuntor.py:299), [app/core/esteira_vigia.py:865](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/esteira_vigia.py:865), [bin/hook_stop_fila1.py:39](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:39), [bin/hook_stop_fila1.py:419](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:419), [bin/pos_push.sh:18](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/pos_push.sh:18), [bin/gerar_cortes.py:114](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/gerar_cortes.py:114)
- **Sim, `VIGIA-TMUX ...` na primeira linha útil quebra a leitura do Stop e do B3.** Conferi em memória com o leitor da cópia. Antes ele devolve `[ESPERANDO, DESENHO]`. Depois da inserção, devolve `[VIGIA-TMUX, ESPERANDO]`. Assim o Stop perde o comando na primeira posição e o B3 perde a pergunta na segunda. Um `PAREI` ou `CLEAR` deslocado também deixa de ser visto naquela primeira posição. No handoff, a lista textual ainda encontra `PAREI`, `lei:` e `!:` até o primeiro `## `, mas os vereditos importados do Stop e a leitura da faixa do censo podem mudar. [bin/hook_stop_fila1.py:408](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:408), [bin/hook_stop_fila1.py:446](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:446), [bin/hook_stop_fila1.py:1058](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:1058), [bin/disjuntor.py:758](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/disjuntor.py:758), [bin/handoff_sessao.sh:85](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/handoff_sessao.sh:85), [bin/handoff_sessao.sh:114](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/handoff_sessao.sh:114)
- **O precedente compatível com a trilha do vigia é `no_relato` na seção `## ALARMES DA ESTEIRA`.** O aval e o contrato pedem uma linha no RELATO. Essa porta registra na seção pinada, abaixo do painel útil. Para decidir o que fazer, as 15 linhas do contrato também precisam respeitar o recorte do Stop. A própria cópia tem, nas linhas 13 e 14, `ESPERANDO` antigos citados em linhas `SUPERADA`. `aval.txt:1`, `contrato.txt:3,5`. [app/core/esteira_vigia.py:815](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/esteira_vigia.py:815), [bin/hook_stop_fila1.py:462](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:462), [app/docs/RELATO.md:13](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/docs/RELATO.md:13)

_7943 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
