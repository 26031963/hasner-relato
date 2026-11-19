# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 21:22** · publicado em **18/11/2026 21:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 21:22 · sessao 2197ad35232a · RAIA-ESPURIAS · leitura · origem=Code

A sonda abaixo está **não executada**; seus resultados e `ERROS=N` permanecem [não verificado]. Ela conserva as ocorrências duplicadas dos dois literais e usa a janela local `dia..dia+1` para as candidatas físicas. O leitor fornece a lâmpada lavrada, mas não sua PK; a PK emitida em `OCUPADO_PK` resulta do critério de unicidade solicitado. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868))

```python
import json
from collections import Counter
from datetime import date, datetime, time, timedelta

from django.db import connection
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.turnos import batidas_apuraveis, turnos_do_colab


# Projecao literal: colaborador_id;dia_turno;hora_orfa;marco_proximo;papel_marco.
# A duplicata de col451 em 31/10 e intencional.
ATE5 = """\
col63;2026-10-31;19:05;19:00;S
col67;2026-11-14;23:59;00:00;S
col67;2026-11-10;00:01;00:00;S
col81;2026-11-11;00:05;00:00;S
col118;2026-11-13;12:05;12:00;S
col121;2026-10-30;02:04;02:00;S
col211;2026-11-16;21:00;21:00;S
col211;2026-10-29;21:00;21:00;S
col211;2026-10-25;02:55;03:00;S
col220;2026-11-13;06:00;06:00;S
col220;2026-11-11;06:00;06:00;S
col278;2026-11-09;11:58;12:00;S
col281;2026-11-09;06:58;07:00;S
col282;2026-11-02;07:03;07:00;S
col300;2026-10-21;06:00;06:00;S
col366;2026-11-09;02:00;02:00;S
col366;2026-11-09;03:00;03:00;E
col366;2026-11-09;07:00;07:00;S
col366;2026-11-05;02:00;02:00;S
col366;2026-11-05;03:00;03:00;E
col366;2026-11-05;07:00;07:00;S
col366;2026-11-03;02:00;02:00;S
col366;2026-11-03;03:00;03:00;E
col366;2026-11-03;07:00;07:00;S
col366;2026-10-30;02:00;02:00;S
col366;2026-10-30;03:00;03:00;E
col366;2026-10-30;07:00;07:00;S
col366;2026-10-28;02:00;02:00;S
col366;2026-10-28;03:00;03:00;E
col366;2026-10-28;07:00;07:00;S
col407;2026-10-23;07:00;07:00;S
col444;2026-11-16;13:55;14:00;E
col451;2026-11-14;07:01;07:00;S
col451;2026-11-12;07:02;07:00;S
col451;2026-11-10;06:59;07:00;S
col451;2026-11-06;07:02;07:00;S
col451;2026-11-04;06:59;07:00;S
col451;2026-11-02;06:59;07:00;S
col451;2026-10-31;07:01;07:00;S
col451;2026-10-31;07:01;07:00;S
col451;2026-10-29;06:58;07:00;S
col451;2026-10-25;06:58;07:00;S
col451;2026-10-21;06:55;07:00;S
col489;2026-11-10;00:00;00:00;S
col491;2026-10-26;02:02;02:00;S
col491;2026-10-26;03:02;03:00;E
col552;2026-11-04;19:02;19:00;S
col587;2026-10-22;19:03;19:00;S
col627;2026-10-22;02:00;02:00;S
col627;2026-10-22;03:00;03:00;E
col627;2026-10-22;07:00;07:00;S
col673;2026-10-21;06:57;07:00;S
col843;2026-11-08;03:00;03:00;S
col843;2026-11-08;04:03;04:00;E
col843;2026-11-08;07:01;07:00;S
col859;2026-11-09;07:00;07:00;S
col876;2026-11-07;06:55;07:00;S
col881;2026-11-13;12:00;12:00;S
col909;2026-11-07;12:00;12:00;S
col913;2026-10-26;01:00;01:00;S
col913;2026-10-26;02:00;02:00;E
col913;2026-10-26;07:01;07:00;S
col922;2026-11-01;07:00;07:00;S
col947;2026-10-31;13:00;13:00;E
col1002;2026-11-09;18:59;19:00;S
col1010;2026-11-05;05:59;06:00;S
col1031;2026-11-17;06:55;07:00;S
col1031;2026-11-15;00:01;00:00;E
col1031;2026-11-15;06:55;07:00;S
"""

# Projecao literal: colaborador_id;dia_turno;hora_orfa.
NOMINAIS = """\
col673;2026-10-21;06:57
col82;2026-10-21;07:01
col82;2026-10-22;06:56
col82;2026-10-23;06:58
col82;2026-10-24;06:58
col82;2026-10-25;06:50
col89;2026-10-25;06:55
col107;2026-10-25;07:01
col107;2026-10-25;15:38
col129;2026-10-25;17:30
col155;2026-10-25;17:28
col214;2026-10-29;21:13
col214;2026-10-30;13:54
col82;2026-10-30;06:56
col214;2026-11-01;06:59
col107;2026-11-01;07:54
col107;2026-11-01;09:02
col129;2026-11-01;17:30
col155;2026-11-01;17:29
col129;2026-11-02;17:31
col214;2026-11-04;14:08
col89;2026-11-08;06:55
col129;2026-11-08;17:30
col214;2026-11-15;07:00
col82;2026-11-15;09:55
col82;2026-11-15;14:05
col82;2026-11-15;15:00
col82;2026-11-15;18:22
col89;2026-11-15;06:55
col107;2026-11-15;07:05
col107;2026-11-15;09:15
col129;2026-11-15;17:30
col155;2026-11-15;17:29
col107;2026-11-17;10:42
"""

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
ERROS = Counter()
parados = set()
colabs = {}
grades = {}
fisicas = {}


def emitir(*campos):
    # Os campos de saida sao produzidos pela propria sonda; nunca imprime
    # mensagem de excecao, nome, CPF ou matricula.
    print(";".join(str(x) for x in campos))


def erro(col, etapa, exc):
    ERROS[(etapa, type(exc).__name__)] += 1
    emitir("ERRO", col, etapa, type(exc).__name__)
    atual = exc
    vistos = set()
    while atual is not None and id(atual) not in vistos:
        vistos.add(id(atual))
        codigo = getattr(atual, "sqlstate", None) or getattr(atual, "pgcode", None)
        if codigo in ("42501", "25006") or isinstance(atual, PermissionError):
            parados.add(col)
            break
        atual = getattr(atual, "__cause__", None) or getattr(atual, "__context__", None)


def obter_colab(col):
    if col not in colabs:
        if not col.startswith("col") or not col[3:].isdigit():
            raise ValueError("identificador invalido")
        colabs[col] = Colaborador.objects.get(pk=int(col[3:]))
    return colabs[col]


def obter_dia(col, dia):
    chave = (col, dia)
    if chave not in grades:
        colaborador = obter_colab(col)
        cels = list(CelulaDia.objects.filter(colaborador=colaborador, data=dia))
        if len(cels) > 1:
            raise ValueError("celulas duplicadas")
        grade = grade_da_celula(colaborador, dia, dia)
        dias = [d for d in grade["dias"] if d["data"] == dia]
        if len(dias) != 1:
            raise ValueError("dia ausente ou duplicado na grade")
        grades[chave] = (cels[0] if cels else None, dias[0])
    return grades[chave]


def instante(dia):
    return timezone.make_aware(
        datetime.combine(dia, time.min), timezone.get_current_timezone()
    )


def obter_batidas(col, dia):
    chave = (col, dia)
    if chave not in fisicas:
        lo = instante(dia)
        hi = instante(dia + timedelta(days=2))
        # A porta usa fim inclusivo; o filtro final exclui a meia-noite
        # posterior ao segundo dia.
        qs = batidas_apuraveis(obter_colab(col), lo, hi)
        fisicas[chave] = sorted(
            (b for b in qs if b.timestamp < hi),
            key=lambda b: (b.timestamp, b.pk),
        )
    return fisicas[chave]


def minuto(valor):
    if not isinstance(valor, str) or len(valor) != 5 or valor[2] != ":":
        return None
    try:
        h, m = int(valor[:2]), int(valor[3:])
    except ValueError:
        return None
    if not (0 <= h < 24 and 0 <= m < 60):
        return None
    return 60 * h + m


def distancia(a, b):
    # Menor distancia circular entre horarios; o sinal indica orfa - marco.
    return (a - b + 720) % 1440 - 720


def retrato(b):
    local = timezone.localtime(b.timestamp)
    return "{}:{}:{}".format(b.pk, b.tipo, local.strftime("%H:%M:%S"))


def no_minuto(batidas, hhmm, tipo=None):
    return [
        b for b in batidas
        if timezone.localtime(b.timestamp).strftime("%H:%M") == hhmm
        and (tipo is None or b.tipo == tipo)
    ]


def lampada_crua(cel, indice):
    # Somente campos da lampada pertinentes a pergunta. A ata pode conter
    # outros dados; nao serializar a ata inteira.
    if cel is None or not isinstance(cel.ata, dict):
        return "None"
    lampadas = cel.ata.get("lampadas")
    if not isinstance(lampadas, list) or indice >= len(lampadas):
        return "None"
    item = lampadas[indice]
    if not isinstance(item, dict):
        return "None"
    chaves = ("tipo", "hora", "luz", "tipo_real", "acesa", "dispensada", "isencao")
    seguro = {k: item.get(k) for k in chaves if k in item}
    return json.dumps(seguro, ensure_ascii=True, separators=(",", ":"))


def secao_c():
    linhas_ate5 = [x.split(";") for x in ATE5.strip().splitlines()]
    linhas_nominais = [x.split(";") for x in NOMINAIS.strip().splitlines()]
    if len(linhas_ate5) != 69 or len(linhas_nominais) != 34:
        raise ValueError("contagem dos literais incorreta")

    for linha in linhas_ate5 + linhas_nominais:
        col, data_txt, orfa = linha[:3]
        if col in parados:
            emitir("NAO_FEITO", col, data_txt, orfa, "permissao_negada")
            continue

        try:
            dia = date.fromisoformat(data_txt)
            bs = obter_batidas(col, dia)
            candidatas_orfa = no_minuto(bs, orfa)
            emitir(
                "ORFA_FISICA", col, data_txt, orfa,
                ",".join(retrato(b) for b in candidatas_orfa),
            )
        except Exception as exc:
            erro(col, "ORFA_FISICA", exc)
            emitir("ORFA_FISICA", col, data_txt, orfa, "NAO_FEITO")
            emitir(
                "MARCO", col, data_txt, orfa, "", "", "NAO_DETERMINAVEL",
                "", "", "", "", "None",
            )
            continue

        try:
            cel, d = obter_dia(col, dia)
            alvo = minuto(orfa)
            celulas = d.get("celulas") or []
            opcoes = []
            for indice, luz in enumerate(celulas):
                if not isinstance(luz, dict):
                    continue
                previsto = minuto(luz.get("hora_prevista"))
                if previsto is not None and alvo is not None:
                    opcoes.append((abs(distancia(alvo, previsto)), indice, luz))

            marco_hh = papel = pk = tipo = dist = candidatos = ""
            crua = "None"
            estado = "NAO_DETERMINAVEL"

            if opcoes:
                menor = min(x[0] for x in opcoes)
                proximas = [x for x in opcoes if x[0] == menor]
                # Empate entre marcos nao autoriza escolher um ocupante.
                if len(proximas) == 1:
                    _, indice, luz = proximas[0]
                    marco_hh = luz.get("hora_prevista") or ""
                    papel = luz.get("papel") or luz.get("tipo") or ""
                    dist = distancia(alvo, minuto(marco_hh))
                    crua = lampada_crua(cel, indice)

                    if (
                        cel is not None
                        and orfa in (d.get("orfas") or [])
                        and not luz.get("dispensada")
                        and not luz.get("isencao")
                    ):
                        acesa = luz.get("acesa_lavrada")
                        if acesa is False:
                            estado = "VAZIO"
                        elif acesa is True:
                            hora_real = luz.get("hora")
                            tipo_real = luz.get("tipo_real")
                            if minuto(hora_real) is not None and tipo_real in ("E", "S"):
                                matches = no_minuto(bs, hora_real, tipo_real)
                                candidatos = ",".join(retrato(b) for b in matches)
                                if len(matches) == 1:
                                    estado = "OCUPADO_PK"
                                    pk = matches[0].pk
                                    tipo = matches[0].tipo
                                else:
                                    estado = "OCUPADO_SEM_PK"

            emitir(
                "MARCO", col, data_txt, orfa, marco_hh, papel, estado,
                pk, tipo, dist, candidatos, crua,
            )
        except Exception as exc:
            erro(col, "MARCO", exc)
            emitir(
                "MARCO", col, data_txt, orfa, "", "", "NAO_DETERMINAVEL",
                "", "", "", "", "None",
            )


def secao_d():
    pedidos = {
        "col673": ("2026-10-21", "2026-10-22"),
        "col129": ("2026-10-25", "2026-11-01", "2026-11-02",
                   "2026-11-08", "2026-11-15"),
        "col155": ("2026-10-25", "2026-11-01", "2026-11-02",
                   "2026-11-15"),
        "col82": ("2026-11-15",),
        "col107": ("2026-11-15",),
        "col89": ("2026-11-15",),
        "col214": ("2026-11-15",),
    }
    try:
        ler_cxr = cxr_lavrado()
    except Exception as exc:
        erro("GLOBAL", "cxr_lavrado", exc)
        for col, datas in pedidos.items():
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
        return

    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    for col, datas in pedidos.items():
        if col in parados:
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
            continue
        try:
            colaborador = obter_colab(col)
            cxr = ler_cxr(colaborador.pk, INI)
            ins = InsumosDoDono(
                colaborador, INI, FIM, hoje, agora=agora,
                sem_motor=True, cxr=cxr,
            )
        except Exception as exc:
            erro(col, "InsumosDoDono", exc)
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
            continue

        for data_txt in datas:
            if col in parados:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
                continue
            try:
                dia = date.fromisoformat(data_txt)
                _, d = obter_dia(col, dia)
                dono = ins.dono(dia)
                emitir("DONO", col, data_txt, dono, d.get("tipo_dia"))
            except Exception as exc:
                erro(col, "dono", exc)
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")


def secao_e():
    col = "col214"
    pk_alvo = 155565
    if col in parados:
        emitir("TURNO214", "NAO_FEITO", "permissao_negada")
        emitir("PAR214", pk_alvo, "NAO_FEITO")
        return

    try:
        colaborador = obter_colab(col)
        dia = date(2026, 11, 15)
        alvos = [b for b in no_minuto(obter_batidas(col, dia), "07:00")
                 if b.pk == pk_alvo
                 and timezone.localtime(b.timestamp).date() == dia]
        turnos = turnos_do_colab(
            colaborador, date(2026, 11, 14), date(2026, 11, 16)
        )
        for t in turnos:
            emitir(
                "TURNO214", t.data_turno,
                t.entrada.pk if t.entrada is not None else "None",
                t.saida.pk if t.saida is not None else "None",
                ",".join(str(b.pk) for b in t.batidas),
                t.aberto, t.cross_meianoite,
            )

        ocorrencias = []
        for t in turnos:
            e = t.entrada.pk if t.entrada is not None else None
            s = t.saida.pk if t.saida is not None else None
            if pk_alvo == e or pk_alvo == s:
                ocorrencias.append((e, s))

        if len(alvos) != 1 or len(ocorrencias) != 1:
            emitir("PAR214", pk_alvo, "NAO_DETERMINAVEL")
        else:
            e, s = ocorrencias[0]
            outro = s if e == pk_alvo else e
            emitir(
                "PAR214", pk_alvo,
                "pareada_com={}".format(outro) if outro is not None else "SOZINHA",
            )
    except Exception as exc:
        erro(col, "turnos_do_colab", exc)
        emitir("TURNO214", "NAO_FEITO", "erro")
        emitir("PAR214", pk_alvo, "NAO_FEITO")


try:
    if connection.settings_dict.get("NAME") != "sombra":
        raise RuntimeError("banco nao e sombra")
    for nome, funcao in (("C", secao_c), ("D", secao_d), ("E", secao_e)):
        try:
            funcao()
        except Exception as exc:
            erro("GLOBAL", "secao_" + nome, exc)
            emitir(nome, "NAO_FEITO", type(exc).__name__)
except Exception as exc:
    erro("GLOBAL", "guarda_sombra", exc)
    for nome in ("C", "D", "E"):
        emitir(nome, "NAO_FEITO", "guarda_sombra")
finally:
    print("ERROS={}".format(sum(ERROS.values())))
    print("FIM_FERNANDO_S54")
```

**C — o que não prova:** a PK encontrada pela coincidência de minuto e tipo não demonstra qual batida o casador efetivamente usou; empate entre marcos também fica indeterminado. O literal não demonstra que a ata da sombra ainda contém a mesma órfã; isso depende da execução. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [não verificado])

**D — o que não prova:** `None` é uma resposta possível quando a lavra necessária não cobre o caso e o motor está desligado; não equivale a um dono identificado. Os valores específicos permanecem [não verificado]. ([e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182))

**E — o que não prova:** o resultado descreve os turnos devolvidos pela porta na janela pedida, não estabelece o estado atual da produção. O par de `155565` permanece [não verificado]. ([turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850))

O envelope indicado não foi encontrado na cópia; a garantia de não escrever em `logs/` do host permanece [não verificado]. O script direto citado pelo pedido escreve arquivos temporários e mantém trilha, de modo que essa garantia depende do envelope. ([sonda_leitura.sh:52](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:52), [sonda_leitura.sh:217](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:217), [não verificado])

## 18/11 21:19 · sessao 572fd5abab45 · RAIA-ESPURIAS · leitura · origem=Code

Não concluí a montagem e a revisão da sonda única. As duas faltas apontadas na volta 1 foram respondidas pelo material desta volta; não seria correto repeti-las como bloqueios. As contagens de referência e os quatro estados de MARCO estão no dado fornecido. [não verificado]

Não vou apresentar um script parcial como se cumprisse o contrato. Em particular, ainda falta integrar e conferir a transcrição completa de `s1_v4`, os dois literais de casos, o tratamento de todas as lâmpadas e a emissão de erros sem expor dados pessoais. A execução na sombra e o resultado `ERROS=N` permanecem [não verificado].

O que consegui confirmar na cópia para orientar a conclusão:

- **NOSSA:** a porta `grade_da_celula` lê as células e devolve os dias com órfãs da ata; isso não estabelece, sem rodar a sonda, a igualdade das contagens com a referência fornecida. ([leitor_celula.py:350](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:350), [leitor_celula.py:418](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:418))
- **VIVA:** `grade_espelho_janela` é a porta de recálculo e `classificar_orfas` é a porta de classificação; uma execução na sombra não prova o resultado da extração de produção. ([utils.py:2334](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:2334), [orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53))
- **MARCO:** o leitor transporta `acesa_lavrada`, hora, tipo real, dispensa e isenção, mas não uma PK de ocupante. A regra de candidato único fornecida permite emitir a PK segundo o critério pedido; não prova por si só a decisão interna do casador. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114))
- **DONO:** `InsumosDoDono.dono` pode retornar `None` com `sem_motor=True`; esse valor deve ser mantido. O resultado para cada caso continua [não verificado]. ([e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182))
- **col214:** `turnos_do_colab` chama `parear_turnos` e retorna turnos com entrada, saída e batidas; o par da batida indicada continua [não verificado] sem a execução. ([turnos.py:43](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:43), [turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850), [turnos.py:1997](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1997))

Também não verifiquei o `envelope.sh` na cópia. O `bin/sonda_leitura.sh` diretamente cria ou reescreve arquivos em `logs/` e acrescenta uma trilha; portanto, a exigência de não escrever em `logs/` do host depende do envelope mencionado no contrato. ([sonda_leitura.sh:115](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:115), [sonda_leitura.sh:248](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:248), [não verificado])

## 18/11 21:17 · sessao 9d4eba666fe9 · RAIA-ESPURIAS · leitura · origem=Code

Não consigo entregar uma sonda que satisfaça integralmente o contrato de **MARCO** e a conferência classe a classe com as evidências disponíveis. Por isso, não apresento um script incompleto como se estivesse pronto para a rodada na sombra.

O bloqueio principal é a identidade do ocupante. `grade_da_celula` devolve, para cada lâmpada da ata, a hora realizada, o tipo real e o estado, mas não devolve a PK da batida; tampouco uma hora, por si só, identifica uma batida quando há mais de uma candidata ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [cartorio.py:699](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/cartorio.py:699)). Além disso, `acesa_lavrada` pode ser `None`, e há lâmpadas dispensadas ou de isenção; portanto, reduzir todos os casos a **OCUPADO por uma PK** ou **VAZIO** pode inventar uma conclusão ([leitor_celula.py:130](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:130)).

A referência fornecida informa **1.292 no total**, mas não traz as contagens de referência de cada classe do `s1_v4`; logo, a sonda pode calcular as contagens na sombra e a diferença do total, mas não determinar a diferença *por classe* em relação à rodada anterior. [não verificado]

O que as portas verificadas permitem medir e seus limites:

- **NOSSA:** a ata fornece as órfãs e a grade as reapresenta; a lógica integral de classificação está no `s1_v4` fornecido. O resultado seria o da sombra na nova execução, sem provar que coincide classe a classe com a rodada de referência ([leitor_celula.py:180](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:180)). A referência de 1.292 é do texto fornecido. [não verificado]
- **VIVA:** `grade_espelho_janela` e `classificar_orfas` permitem recalcular e classificar a grade, alimentada pelas batidas apuráveis; isso não reproduz por si só o estado ou o instante da extração de produção ([utils.py:2334](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:2334), [orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53), [turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868)).
- **MARCO:** a ata permite relatar marcos, papéis, horas e estados lavrados disponíveis, além de candidatos físicos separados; não prova necessariamente qual PK ocupou um marco nem por que o casador tomou uma decisão ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114)).
- **DONO:** `InsumosDoDono.dono(dia)` é a porta pedida; com `sem_motor=True`, pode devolver `None` quando falta a lavra necessária, e isso deve permanecer sem resposta ([e6_oraculo.py:128](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182)). Para col214, a porta de produção é `turnos_do_colab`, que alimenta `parear_turnos`; o resultado específico permanece [não verificado] sem execução ([turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850), [turnos.py:1997](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1997)).

Para fechar a entrega sem aproximar, falta permitir um estado explícito **NÃO DETERMINÁVEL** em MARCO, com candidatos identificados como candidatos, e fornecer as contagens por classe da rodada `s1_v4` de referência. A execução e seus `ERROS=N` continuam [não verificado].

## 18/11 21:16 · sessao 03c313a57f5f · E4C-F9 · censo · origem=Code

A auditoria das perguntas e triagens mostra duas distinções necessárias para a contagem: a falta de controle em M6 estava dentro do tema pedido, mas não foi apontada; e o limite de formas indiretas do selo foi apontado e mantido em fila. Este último não cabe literalmente em nenhuma das classes propostas: a classe **(b)** exige descarte pela triagem, e a triagem o encaminhou ao opus. [anexos `construtor_c1.q`, `q.txt` da volta 1 e `triagem.txt` da volta 3: sem linhas numeradas; **não verificado como arquivo:linha**]

Os anexos foram fornecidos no texto da solicitação e não têm numeração de linhas. Abaixo os identifico pelo nome e item; **suas referências exatas em arquivo:linha são [não verificado]**. As referências ao código são da cópia examinada.

**Auditoria de quem perguntou e de quem triou**

- O brief c1 pediu, para cada M, divergência entre projeção e atos, controle sem divergência e o mutante que o caso mataria. A pergunta ao verificador da volta 1 também pediu exame de “selo/RED sem mordida”, mas seu mutante explícito era o retorno de **M2** à coluna, não a remoção do predicado de M6. [anexos `construtor_c1.q`, parágrafos RED e selo, e `q.txt` da volta 1, itens 3–4: **não verificado como arquivo:linha**]
- O brief c1 pediu `ALLOW-iii` no backfill e no limbo e, ao mesmo tempo, proibiu editar arquivos fora dos três comandos e testes. A triagem da volta 1 reconheceu o conflito como erro do brief; c2 autorizou somente comentários nos dois arquivos e repetiu as classes `ALLOW-iii / ALLOW-C`. [anexos `construtor_c1.q`, FICA e PROIBIDO; `triagem_ver1.txt`, B3; `construtor_c2.q`, B3: **não verificado como arquivo:linha**] [nao verificado]
- A triagem da volta 1 considerou procedente a mistura em M7 e escolheu a guarda restritiva; a da volta 2 aceitou o fechamento de B1 e B3; a da volta 3 preservou em **FILA** as formas indiretas não vistas pelo selo. [anexos `triagem_ver1.txt`, B1; `triagem.txt` da volta 2, item 1; `triagem.txt` da volta 3, item 3: **não verificado como arquivo:linha**]

**1. M6: mutante sem predicado sobrevive — classe (a).**  
**Pergunta:** “para cada M, um caso onde PROJECAO e SEQUENCIA divergem”; “Controle”; “Diga o mutante de cada M” e, ao verificador, “selo/RED sem mordida”. O mutante pedido no brief era voltar ao filtro da coluna; a remoção completa do predicado não foi especificada. [anexos `construtor_c1.q`, RED, e `q.txt` da volta 1, item 4: **não verificado como arquivo:linha**]

**Resposta:** o construtor disse “inclui controles” e propôs para M6 o mutante que troca a face pelo filtro da coluna; o verificador disse que “o teste novo cobre o universo em dry-run”. **Não apontaram** a ausência de pergunta validada que deveria sair nem o mutante sem predicado sobrevivente. Na cópia, M6 cria a divergente e uma controle e espera contagem 2; M2–M5 também criam uma pergunta validada e verificam sua exclusão. [anexos `construtor_c1.r` e `r.txt` da volta 1, item 2: **não verificado como arquivo:linha**; código: [teste M6](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:96), [controle M2–M5](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:73)]

A sobrevivência relatada pelo opus é resultado do anexo; não há execução independente nesta auditoria. [anexo `veredito.md`, achado 1: **não verificado como arquivo:linha**]

**2. M1: falta controle positivo — classe (a).**  
**Pergunta:** o mesmo brief pediu controle e mutante para **cada M**; a pergunta da volta 1 incluiu “selo/RED sem mordida”. [anexos `construtor_c1.q`, RED, e `q.txt` da volta 1, item 4: **não verificado como arquivo:linha**]

**Resposta:** o construtor identificou `test_M1_rotulo_e_controle` como caso que deveria matar o mutante de retorno à coluna, mas **não apontou** a falta do controle positivo nem o ramo que sempre retorna `None`. O teste na cópia verifica apenas que as duas saídas **não** contêm `validada_em 1`. [anexo `construtor_c1.r`, tabela de mutantes: **não verificado como arquivo:linha**; [teste M1](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:85)]

**3. Comentários ALLOW incorretos ou incompletos — classe (a), para o defeito específico final.**  
**Pergunta:** c1 determinou `ALLOW-iii` para o backfill e os três sítios do limbo; c2 pediu comentários “como no brief c1”; o verificador da volta 2 foi perguntado se os achados da triagem estavam fechados e como o selo tratava comentários. [anexos `construtor_c1.q`, FICA; `construtor_c2.q`, B3; `q.txt` da volta 2, itens 1 e 3: **não verificado como arquivo:linha**]

**Resposta:** na volta 1, o verificador **viu a falta de comentários** e a triagem a considerou procedente; depois, o construtor disse ter acrescentado comentários e o verificador da volta 2 disse “B3 — PROCEDE o acréscimo dos dois comentários”. Ele também viu que o selo F9 não lê esses comentários. **Não apontou** que o backfill estava rotulado `ALLOW-C` contra o pedido `ALLOW-iii`, nem a cobertura por sítio descrita pelo opus. A triagem da volta 2 marcou B3 “comentarios: OK”. [anexos `r.txt` da volta 1, FICA; `triagem_ver1.txt`, B3; `construtor_c2.r`, B3; `r.txt` da volta 2, itens 1 e 3; `triagem.txt` da volta 2, item 1: **não verificado como arquivo:linha**; [comentário do backfill](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [outro sítio do backfill](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/backfill_decisoes_pergunta.py:104), [comentário do limbo](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/curar_perguntas_em_limbo.py:46)]

Não classifico o defeito específico como **(b)**: a triagem aceitou como fechado B3, mas os textos não mostram que ela tenha identificado e descartado o rótulo errado ou a falta de cobertura por sítio. [anexo `triagem.txt` da volta 2, item 1: **não verificado como arquivo:linha**]

**4. RED de c3 sem log Django — classe (c), alcance de execução.**  
**Pergunta:** c3 pediu mutantes para cada forma nova que fizessem o selo falhar; a volta 3 perguntou se cada forma tinha mutante que fazia o selo falhar. **Não pediu explicitamente um log Django do RED.** [anexos `construtor_c3.q` e `q.txt` da volta 3, item 1: **não verificado como arquivo:linha**]

**Resposta:** o construtor declarou: “O RED isolado mostrou” as formas escapando antes da cura e informou que a execução pelo Django ficou impedida; o verificador relatou 34/34 mutantes em memória e marcou a suíte Django completa como não verificada. Assim, **viram e declararam o limite de execução**, mas o dado não contém o log Django de RED apontado pelo opus. [anexos `construtor_c3.r`, parágrafo RED, e `r.txt` da volta 3, item 1: **não verificado como arquivo:linha**] [nao verificado]

**5. Formas indiretas escapam ao selo — sem classe aplicável literalmente.**  
**Pergunta:** a volta 3 perguntou expressamente se restavam formas como “variavel com o nome do campo, values(*lista), Meta/only via constante de modulo” e se os comandos as usavam. [anexo `q.txt` da volta 3, item 3: **não verificado como arquivo:linha**]

**Resposta:** o verificador **viu** variável, lista expandida, constante e `__dict__` fora dos padrões, e relatou não encontrar essas formas nos três comandos atuais. A triagem registrou “**FILA, anexo ao opus**”; o próprio opus concordou com ela. Portanto, não é (b): houve encaminhamento em fila, não descarte. Tampouco é (a), (c), (d) ou (e). [anexos `r.txt` da volta 3, item 3; `triagem.txt` da volta 3, item 3; `veredito.md`, achado 5: **não verificado como arquivo:linha**; [padrões reconhecidos pelo selo](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:145)]

**6. Guarda M7 e mutante equivalente da impressão — classe (b) para a mistura aceita; componente do mutante: (e).**  
**Pergunta:** a volta 1 pediu divergência entre leitores e derivação paralela; c2 determinou expressamente a guarda restritiva e pediu o mutante que **retira a guarda**. A volta 2 perguntou se B1 estava fechado. **Não foi perguntado** se o mutante da impressão era equivalente dentro da classe A ou se morria apenas pelo selo. [anexos `q.txt` da volta 1, itens 4–5; `construtor_c2.q`, B1; `q.txt` da volta 2, item 1: **não verificado como arquivo:linha**]

**Resposta e triagem:** o verificador da volta 1 **viu a mistura** entre leitura pelo ato e classificação pela projeção; a triagem reconheceu B1 e determinou a guarda como “cura ... mais RESTRITIVA”; o verificador da volta 2 considerou fechada a guarda solicitada. Para essa parte, **(b)** representa a aceitação explícita da composição pela triagem, embora ela a tenha tratado como cura, não simplesmente ignorado o achado. A equivalência do mutante da impressão **não foi vista nem especificamente perguntada: (e)**. [anexos `r.txt` da volta 1, item 5; `triagem_ver1.txt`, B1; `r.txt` da volta 2, item 1: **não verificado como arquivo:linha**; [guarda](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:63), [impressão](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:81)]

| Achado | Classe para o censo |
|---|---|
| 1 — M6 sem controle que exclui | **(a)** |
| 2 — M1 sem controle positivo | **(a)** |
| 3 — ALLOW errado / cobertura por sítio | **(a)** |
| 4 — RED c3 sem log Django | **(c)** |
| 5 — formas indiretas | **Sem classe aplicável:** visto e mantido em fila |
| 6 — guarda M7 / mutante da impressão | **(b)** para a guarda; **(e)** para o mutante |

Contando **achados**, com o 6 atribuído a **(b)** pela decisão de triagem sobre a guarda: **(a) 3; (b) 1; (c) 1; (d) 0; (e) 0; sem classe aplicável 1**. O componente não perguntado do achado 6 está registrado separadamente acima e não é contado uma segunda vez. Os achados 1–3 também são casos em que nenhum dos textos de resposta anexados apontou o defeito específico; foram atribuídos a **(a)** porque o tema estava presente na pergunta. [anexos citados nos achados 1–6: **não verificado como arquivo:linha**]

## 18/11 20:46 · sessao 153092d6b9cc · E4C-F6F7 · verificador · origem=Code

1. **PROCEDE para os acessos que `DecisaoPergunta.leitura` faz hoje; NÃO PROCEDE como equivalência integral dos objetos.** O delta fornecido mostra que c15 cortava os dois carimbos somente quando posteriores a `agora`; a vista aplica a mesma condição. A consulta mantém as decisões não excluídas até o horizonte, ordenadas por pergunta, instante e PK. A vista recebe essas mesmas decisões, e `leitura` as converte em lista e volta a ordenar a sequência combinada com a projeção. [tarefa_do_dia.py:390](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:390), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:403), [models.py:4308](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4308).

   Nos corpos atuais, `leitura` e `linhas_da_projecao_sem_linha` usam `pk`, `decisoes.all()`, os dois carimbos, `resposta_colab` e `validada_por_id` da pergunta; a vista entrega esses valores por delegação, com o corte declarado. Esses corpos não consultam `_state`, `_prefetched_objects_cache`, identidade ou `isinstance` da pergunta. [models.py:4251](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4251), [models.py:4308](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4308), [tarefa_do_dia.py:414](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:414).

   A vista é outro objeto, não uma instância da classe da pergunta; `decisoes.all()` retorna tupla, e o acesso a `_prefetched_objects_cache` delegaria ao objeto original, em vez de fornecer o cache substituído da cópia c15. Essas diferenças não afetam os acessos examinados, mas impedem afirmar identidade geral ou equivalência para leitores futuros. [tarefa_do_dia.py:403](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:403), [models.py:4308](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4308).

2. **NÃO PROCEDE que a vista acrescente uma exceção ao selo.** Atribuição direta ao par e `setattr(p, 'respondida_em', ...)` ou `setattr(p, 'resposta_colab', ...)` em `tarefa_do_dia.py` continuam detectáveis e fora dos sítios permitidos. A vista usa `object.__setattr__` apenas para seus três campos internos e rejeita atribuição comum. [test_e3_porta_A_escritor_do_par.py:124](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:124), [test_e3_porta_A_escritor_do_par.py:203](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:203), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:403).

   **Limite do selo:** `object.__setattr__(p, 'respondida_em', valor)` escapa dessa detecção, pois o scanner examina o primeiro argumento de `__setattr__`, que nessa forma é `p`, não o nome do campo. Esse limite não foi criado pela vista, mas impede afirmar que *toda forma possível* de escrever o par está coberta. [test_e3_porta_A_escritor_do_par.py:140](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:140).

   O anexo relata a falha de c15 no `setattr` da cópia; resultado da suíte completa para c16 **[não verificado]**. [nao verificado]

3. **PROCEDE para estes quatro mutantes na execução local; NÃO PROCEDE que o teste garanta a origem do `AssertionError`.** Cada padrão apareceu uma vez e as quatro fontes mutadas compilaram. Na execução local em memória, falharam respectivamente: [nao verificado]

   - `>=`: na verificação da validação exatamente no horizonte. [teste:150](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:150).
   - `if False`: no resultado esperado para validação futura. [teste:149](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:149).
   - Inclusão da decisão futura: na igualdade da sequência cortada. [teste:147](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:147).
   - Escrita na pergunta: no bloqueio explícito de escrita do carimbo. [teste:118](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:118).

   O `assertRaises(AssertionError)` envolve o teste inteiro, sem conferir mensagem ou local: outro `AssertionError` dentro dele também satisfaria a catraca. `ImportError` e `SyntaxError`, por si, não a satisfazem. [teste:174](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:174).

4. **Custo ou efeito colateral relevante em lote: [não verificado].** A classe é criada uma vez por chamada de `_fatos_das_perguntas`, fora do laço; cada pergunta recebe uma instância e uma tupla das suas decisões mantidas. Há, portanto, custo adicional de alocação proporcional às perguntas e decisões, mas o impacto prático não foi medido. [tarefa_do_dia.py:403](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:403), [tarefa_do_dia.py:425](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:425).

5. **Fluxo dos valores do delta:**

   - **Projeção `respondida_em` / `resposta_colab`:** os sítios e delegações autorizados estão enumerados no selo; as portas de resposta humana, automática e pela trilha aparecem em `disputa_emissao.py`. A vista lê o carimbo e delega o texto; a leitura usa ambos para construir o passivo sem linha correspondente. [selo:40](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:40), [disputa_emissao.py:1481](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1682](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/disputa_emissao.py:1682), [disputa_emissao.py:1701](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/disputa_emissao.py:1701), [models.py:4259](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4259).
   - **`validada_em` / `validada_por_id`:** a porta de validação grava o carimbo e registra a decisão; outras operações o removem. A vista corta o carimbo futuro, e a projeção sem linha lê também o identificador de quem validou. [validacao.py:168](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/validacao.py:168), [validacao.py:273](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/validacao.py:273), [models.py:4265](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4265).
   - **Decisões e resultado:** `registrar` persiste as linhas; a carga lê e corta a sequência, a vista a entrega à `leitura`, e o mapa resultante é consumido por `_pela_decisao`. O passivo é calculado separadamente a partir das perguntas originais e também cortado por instante. [models.py:4328](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4328), [tarefa_do_dia.py:390](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:390), [tarefa_do_dia.py:425](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:425), [tarefa_do_dia.py:471](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:471).

## 18/11 20:46 · sessao b0fefc5da3c6 · - · - · origem=Ronald (tmux)

Os quatro mutantes falharam pelos motivos esperados em execução local, em memória e sem banco:

- `>=` falhou na verificação do carimbo exatamente no horizonte ([teste:150](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:150)).
- `if False` falhou no resultado esperado para validação futura ([teste:149](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:149)).
- `if True` incluiu a decisão futura e falhou na comparação da sequência ([teste:147](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:147)).
- O `setattr` inserido acionou o bloqueio explícito de escrita ([teste:118](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:118)).

**Não procede** que o teste garanta sempre a causa correta: o `assertRaises(AssertionError)` aceita qualquer `AssertionError` emitido durante a chamada inteira, inclusive de outro assert ([teste:181](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:181)). `ImportError` e `SyntaxError` não satisfazem esse contexto; a fonte mutada é compilada em memória ([e4_apoio.py:8](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/e4_apoio.py:8)).

## 18/11 20:45 · sessao be0b0c59d597 · - · - · origem=Ronald (tmux)

Verificação do selo e do fluxo dos valores enviada ao agente principal, incluindo a limitação de detecção de `object.__setattr__` em [test_e3_porta_A_escritor_do_par.py:140](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:140).

## 18/11 20:45 · sessao 07e237c42604 · - · - · origem=Ronald (tmux)

A vista preserva os valores usados pela implementação atual de `DecisaoPergunta.leitura`, com as diferenças de objeto e de tipo informadas ao agente principal. A equivalência foi verificada por inspeção de [models.py](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4251) e [tarefa_do_dia.py](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:390).

## 18/11 20:44 · sessao 6f6dae60755f · E4C-F6F7 · construtor · origem=Code

A cura está no diff local: `_fatos_das_perguntas` entrega a `DecisaoPergunta.leitura` uma vista somente de leitura. Ela delega os atributos à pergunta, oculta os dois carimbos posteriores a `agora` e fornece as decisões já cortadas; a igualdade permanece no horizonte ([tarefa_do_dia.py:398](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:398), [tarefa_do_dia.py:402](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:402)).

A verificação pedida de `models.py` confirma que `leitura` lê `p.decisoes.all()` e passa a pergunta ao auxiliar; o auxiliar lê os atributos da projeção e constrói linhas em variáveis locais, sem escrever na pergunta ([models.py:4308](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/models.py:4308), [models.py:4251](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/models.py:4251)).

O teste de horizonte bloqueia escrita dos carimbos mesmo em cópia e verifica que o original segue intacto ([teste:118](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:118), [teste:138](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:138)). Na base c15, ele ficou **RED para ambos os carimbos**; com a cura, o módulo passou com 7 testes. Outros 13 testes relacionados sem banco passaram. O corpo do selo do par passou em execução sem Django, com substitutos de infraestrutura; nele, o mutante que reinsere `setattr(p, 'respondida_em', None)` foi detectado. O selo cobre essa forma de escrita, mas declara `obj.__dict__` fora de alcance ([selo:23](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:23)). [nao verificado]

Os quatro mutantes — fronteira `>=`, ausência do corte dos carimbos, inclusão de decisão futura e retorno da escrita — são executados pelo mesmo comando ([teste:161](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:161)): [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura.HorizonteProjecao.test_mutantes_da_vista_morrem
```

Para reproduzir o módulo completo e consultar o diff:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura
git diff -- chamados/services/tarefa_do_dia.py chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py
```

**LEI-AKITA:** padrão = juiz único; a vista apenas adapta os fatos ao horizonte e conserva `DecisaoPergunta.leitura` como autoridade da leitura ([tarefa_do_dia.py:427](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:427)). Suite Django e certificação pela pista: **[não verificado]**. [nao verificado]

## 18/11 20:40 · sessao 9255151745e5 · E4C-F9 · verificador · origem=Code

1. **PROCEDE, para as formas literais descritas.** O selo conta `getattr`, `setattr` e `hasattr` quando o nome do campo é uma string literal no segundo argumento posicional; também conta chaves literais de um dicionário expandido com `**` diretamente em qualquer chamada, incluindo sufixos `__lookup` ([selo, linhas 156–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:156)). Há mutantes para as três funções, para `getattr` com e sem valor padrão, para chave simples em outra chamada e para lookup em `filter`, com ambos os campos; eles exigem contagem diferente da base ([selo, linhas 207–226](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:207)). Executei a lógica desses 34 mutantes em memória: **34/34 alteraram a contagem**. [não verificado: execução da suíte Django completa]

2. **NÃO PROCEDE que o delta cause falso positivo nos três comandos atuais ou deixe de contar alguma forma anterior.** Reconstituindo c2 a partir do patch fornecido e executando ambas as versões em memória sobre os arquivos atuais, as contagens foram iguais: duas ocorrências no auditor, nenhuma em `encerrar_perguntas_com_fato` e uma em `validar_classe_a`; são exatamente as três permitidas ([selo, linhas 194–205](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:194); [auditor, I18](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1084); [auditor, I20](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1116); [validador, guarda](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:63)). A alteração acrescenta condições à condição anterior, sem retirar as anteriores ([selo, linhas 166–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:166)). Em código futuro, contudo, a detecção é sintática: uma chamada ou objeto sem relação com a coluna pode ser contado se usar esses literais ([selo, linhas 146–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:146)).

3. **PROCEDE: restam formas não vistas.** Confirmei em memória que o selo retorna contagem vazia para `nome = 'validada_em'; getattr(q, nome)`, `values(*['validada_em'])`, `CAMPO = 'validada_em'; only(CAMPO)` e `q.__dict__['validada_em']`. A análise exige formatos específicos de nós e não resolve variáveis ou listas expandidas ([selo, linhas 148–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:148)). Uma referência ao nome em `Meta` via constante também fica fora desses padrões ([selo, linhas 156–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:156)).

   Nos três comandos atuais, **não encontrei uso dessas formas para os dois campos**; as ocorrências literais relevantes encontradas são as exceções citadas e acessos através de `DecisaoPergunta.leitura` ([auditor](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:206); [validador](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:63)). Nomes construídos dinamicamente ou recebidos de fora desses arquivos: **[não verificado]**.

## 18/11 20:38 · sessao dea090c8635b · E4C-F9 · construtor · origem=Code

O diff está em [test_e4c_f9_comandos_leem_o_ato.py](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:141); a base da cópia é `c939f3bfefb7d2844c4eb0ae9a22c7388efa4696`. Os comandos de produção não foram alterados (`git status --short`).

- `getattr`, com ou sem default, `setattr` e `hasattr`: reconhecimento em [linha 168](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:168); mutantes em [linha 216](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:216).
- Chaves de dict literal expandido em qualquer chamada, com campo ou campo e lookup: reconhecimento em [linha 162](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:162); mutantes em [linha 218](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:218).
- String com lookup nas chamadas já reconhecidas: normalização em [linha 172](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:172); mutante adicional em [linha 220](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:220). Essa forma já era reconhecida antes do diff.

O RED isolado mostrou que as seis primeiras formas novas escapavam antes da cura; após a alteração, os dois métodos do selo passaram isolados, incluindo os mutantes para ambos os campos e as contagens dos comandos reais. A execução pelo Django ficou impedida: `python3` não encontrou `django.core`. Certificação pela suíte Code e pelo opus: **[não verificado]**. [nao verificado]

O comando abaixo executa **todos os mutantes**, cada um como subteste, e o selo sobre os comandos reais, sem Django nem banco:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
import unittest
from pathlib import Path

p = Path('chamados/tests/test_e4c_f9_comandos_leem_o_ato.py')
tree = ast.parse(p.read_text())
selected = [n for n in tree.body
            if isinstance(n, (ast.FunctionDef, ast.ClassDef))
            and n.name in ('leituras_cruas', 'SeloProjecaoTest')]
ns = {'ast': ast, 'SimpleTestCase': unittest.TestCase,
      'COMANDOS': Path('chamados/management/commands')}
exec(compile(ast.Module(body=selected, type_ignores=[]), str(p), 'exec'), ns)
suite = unittest.defaultTestLoader.loadTestsFromTestCase(ns['SeloProjecaoTest']) [nao verificado]
result = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
raise SystemExit(not result.wasSuccessful())
PY
```

Para inspecionar o diff: `git diff -- chamados/tests/test_e4c_f9_comandos_leem_o_ato.py`.

LEI-AKITA: origem=[leituras_cruas:141](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:141); RED e mutantes=[teste:207](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:207); padrao=n/a (instrumento). [nao verificado]

## 18/11 20:36 · sessao 3b6a85200046 · E4C-F9 · verificador · origem=Code

**Resultado: NÃO PROCEDE que todos os achados da volta 1 estejam fechados.** A guarda de B1 e os comentários de B3 estão no delta; B2 foi parcialmente tratado, mas ainda há leituras por string que o selo F9 não detecta ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63), [test_e4c_f9_comandos_leem_o_ato.py:141](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L141), [backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py#L56), [curar_perguntas_em_limbo.py:46](app/chamados/management/commands/curar_perguntas_em_limbo.py#L46)).

1. **Fechamento dos achados da triagem**

   - **B1 — PROCEDE o fechamento da guarda solicitada.** A divergência é contada e interrompe o processamento antes da classificação e da inclusão na lista validável; o teste exercita `--apply` com uma divergente e uma controle ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63), [validar_classe_a.py:100](app/chamados/management/commands/validar_classe_a.py#L100), [test_e4c_f9_comandos_leem_o_ato.py:122](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L122)). A saída identifica a **contagem**, não imprime a identidade de cada divergente ([validar_classe_a.py:77](app/chamados/management/commands/validar_classe_a.py#L77)).
   - **B2 — NÃO PROCEDE o fechamento integral.** O delta acrescenta detecção de várias strings e contagem por ocorrência, mas deixa passar, por exemplo, `getattr(q, 'resposta_colab')` e `filter(**{'resposta_colab': 'x'})`: `getattr` e `filter` não estão na lista de chamadas reconhecidas para constantes string ([test_e4c_f9_comandos_leem_o_ato.py:146](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L146), [test_e4c_f9_comandos_leem_o_ato.py:156](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L156)).
   - **B3 — PROCEDE o acréscimo dos dois comentários**, exatamente nos arquivos indicados ([backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py#L56), [curar_perguntas_em_limbo.py:46](app/chamados/management/commands/curar_perguntas_em_limbo.py#L46)). O efeito desses comentários no selo é tratado no item 3.
   - A inclusão do censo da fila LEI-AKITA no commit c2: **[não verificado]**. O anexo de delta fornecido não mostra esse censo [não verificado]. [nao verificado]

2. **Contagem e universo — PROCEDE.** A comparação usa o valor devolvido por `DecisaoPergunta.leitura(q).resposta_colab` e a coluna `q.resposta_colab` ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63)). Essa leitura não é necessariamente composta só de linhas reais: ela também incorpora, como linhas faltantes, valores da projeção sem correspondência na sequência ([models.py:4295](app/chamados/models.py#L4295), [models.py:4308](app/chamados/models.py#L4308)).

   **PROCEDE:** o `continue` retira cada divergente da classe que o classificador lhe atribuiria — A, B, C ou `(sem hora)` — e a coloca no novo contador ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63), [juizes.py:2034](app/chamados/juizes.py#L2034)). **PROCEDE:** somando o novo contador às demais classes, cada entrada de `pendentes` incrementa exatamente uma classe, desde que o laço termine sem exceção; esse universo é a lista construída pelo comando, não uma contagem verificada de toda a produção ([validar_classe_a.py:44](app/chamados/management/commands/validar_classe_a.py#L44), [validar_classe_a.py:60](app/chamados/management/commands/validar_classe_a.py#L60)). `--limite` corta a lista de classe A depois da contagem ([validar_classe_a.py:71](app/chamados/management/commands/validar_classe_a.py#L71)).

3. **Comentários ALLOW — NÃO PROCEDE que o selo os aceite por texto livre.** O selo F9 analisa a AST de apenas três comandos; os dois arquivos que receberam comentários não integram essa lista, e os comentários não fazem parte da AST analisada ([test_e4c_f9_comandos_leem_o_ato.py:143](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L143), [test_e4c_f9_comandos_leem_o_ato.py:196](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L196)). A catraca E3, por sua vez, declara as exceções por **caminho e função**, com contagem por sítio ([test_e3_catraca_respondedores.py:66](app/chamados/tests/test_e3_catraca_respondedores.py#L66), [test_e3_catraca_respondedores.py:81](app/chamados/tests/test_e3_catraca_respondedores.py#L81), [test_e3_catraca_respondedores.py:393](app/chamados/tests/test_e3_catraca_respondedores.py#L393)).

   **NÃO PROCEDE** que colar o mesmo comentário em outro comando conceda exceção: o texto não altera a chave declarada na E3 nem as permissões da F9 ([test_e3_catraca_respondedores.py:66](app/chamados/tests/test_e3_catraca_respondedores.py#L66), [test_e4c_f9_comandos_leem_o_ato.py:189](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L189)).

4. **Selo F9 — PROCEDE que varre AST**, construída com `ast.parse`, para os dois campos e três arquivos explicitamente enumerados ([test_e4c_f9_comandos_leem_o_ato.py:141](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L141), [test_e4c_f9_comandos_leem_o_ato.py:196](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L196)).

   **NÃO PROCEDE que haja caso que morde para cada forma perguntada.** Há mutantes para `values`, `values_list`, `only`, `defer`, `order_by`, `F`, `Q` e `annotate`, além de argumentos nomeados extras dentro de `add`; não há mutante de `getattr`, e essa forma não é detectada pelo analisador ([test_e4c_f9_comandos_leem_o_ato.py:147](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L147), [test_e4c_f9_comandos_leem_o_ato.py:202](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L202)). Atributos diretos são analisados, mas o teste de mutação apresentado não injeta um atributo direto; `filter(resposta_colab=...)` é reconhecido como keyword, enquanto a chave string em `filter(**{...})` escapa ([test_e4c_f9_comandos_leem_o_ato.py:150](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L150), [test_e4c_f9_comandos_leem_o_ato.py:154](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L154), [test_e4c_f9_comandos_leem_o_ato.py:161](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L161)).

   **PROCEDE com limite:** uma ocorrência nova de uma forma **reconhecida** em comando varrido altera a contagem ou cria chave e falha na igualdade; o mutante verifica ocorrências extras dentro de `add`. Uma forma não reconhecida, como `getattr`, não é pega por essa contagem ([test_e4c_f9_comandos_leem_o_ato.py:184](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L184), [test_e4c_f9_comandos_leem_o_ato.py:196](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L196), [test_e4c_f9_comandos_leem_o_ato.py:217](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L217)).

5. **Campos envolvidos — PROCEDE a seguinte rastreabilidade dos caminhos examinados; inventário completo de todos os escritores e leitores da aplicação: [não verificado].**

   | Campo | Escrita e leitura relevantes |
   |---|---|
   | `PerguntaDisputa.resposta_colab` | A porta de resposta atribui e salva a coluna; a reabertura prepara seu esvaziamento ([disputa_emissao.py:1481](app/chamados/services/disputa_emissao.py#L1481), [disputa_emissao.py:1529](app/chamados/services/disputa_emissao.py#L1529), [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py#L1617)). A guarda nova e o classificador leem a coluna; backfill e cura também a leem ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63), [juizes.py:2041](app/chamados/juizes.py#L2041), [models.py:4260](app/chamados/models.py#L4260), [curar_perguntas_em_limbo.py:47](app/chamados/management/commands/curar_perguntas_em_limbo.py#L47)). |
   | `respondida_em` | A porta de resposta e a cura pela trilha o gravam; a reabertura prepara sua limpeza ([disputa_emissao.py:1528](app/chamados/services/disputa_emissao.py#L1528), [disputa_emissao.py:1705](app/chamados/services/disputa_emissao.py#L1705), [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py#L1617)). Backfill e cura filtram a coluna, e `leitura` a usa na reconstrução do passivo sem linha ([backfill_decisoes_pergunta.py:57](app/chamados/management/commands/backfill_decisoes_pergunta.py#L57), [curar_perguntas_em_limbo.py:47](app/chamados/management/commands/curar_perguntas_em_limbo.py#L47), [models.py:4251](app/chamados/models.py#L4251)). |
   | `validada_em` | `_carimbar_validacao` grava a coluna e registra o ato; resposta nova e reabertura podem limpá-la ([validacao.py:161](app/chamados/services/validacao.py#L161), [disputa_emissao.py:1484](app/chamados/services/disputa_emissao.py#L1484), [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py#L1617)). O backfill lê a coluna; a seleção de pendentes lê a validação devolvida por `leitura` ([backfill_decisoes_pergunta.py:57](app/chamados/management/commands/backfill_decisoes_pergunta.py#L57), [models.py:2003](app/chamados/models.py#L2003)). |
   | `DecisaoPergunta.ato`, `ato_em`, `ato_texto` e `resposta_mantem_validacao` | `registrar` insere as linhas recebidas; a porta de resposta fornece esses valores ([models.py:4329](app/chamados/models.py#L4329), [disputa_emissao.py:1532](app/chamados/services/disputa_emissao.py#L1532)). `leitura` os percorre para reconstruir resposta e validação; também acrescenta o passivo da projeção sem linha ([models.py:4280](app/chamados/models.py#L4280), [models.py:4308](app/chamados/models.py#L4308)). |

Os testes não foram executados [não verificado]. A identidade da cópia pelo hash informado também está **[não verificado]**: o diretório disponível não é um repositório Git [não verificado]. [nao verificado]

## 18/11 20:34 · sessao 4d39f1c0a7d7 · E4C-F6F7 · verificador · origem=Code

**Resultado: os três achados da triagem estão corrigidos no código do delta, com uma ressalva de execução dos testes novos:** as três classes acrescentadas depois de `unittest.main()` são descobertas por `python -m unittest`, mas não quando o arquivo é executado diretamente. Os seis testes do módulo c14 passaram pela primeira forma; a catraca geral dos mutantes E3 não pôde ser importada neste ambiente por falta de `django.test`. [test_e4c_f6f7_c14_lei3_pela_leitura.py:101; test_e3_mutantes_alvo.py:9; execução local, não registrada em arquivo] [nao verificado]

1. **Fechamento dos achados e chamadores**

   - **PROCEDE — default silencioso fechado.** `leitura` e `leituras` são argumentos nomeados obrigatórios; os dois pontos de `_tarefa_pura` indexam o mapa, de modo que uma chave ausente falha em vez de produzir leitura vazia. O teste cobre ausência dos argumentos e mapa incompleto. [tarefa_do_dia.py:142; tarefa_do_dia.py:196; tarefa_do_dia.py:279; tarefa_do_dia.py:340; test_e4c_f6f7_c14_lei3_pela_leitura.py:151] [nao verificado]
   - **PROCEDE — projeção futura cortada para a leitura.** A cópia recebe `None` nos dois carimbos posteriores a `agora`, antes de `DecisaoPergunta.leitura`; há teste para futuro e igualdade nos dois campos. [tarefa_do_dia.py:405; test_e4c_f6f7_c14_lei3_pela_leitura.py:134] [nao verificado]
   - **PROCEDE — alvo velho corrigido.** A string do mutante inclui a nova chamada nomeada e corresponde a uma ocorrência na cópia; a catraca exige exatamente uma ocorrência. A execução completa dessa catraca está **[não verificado]**, pois sua importação falhou por falta de `django.test`. [e3_v8_mutantes.py:76; tarefa_do_dia.py:279; test_e3_mutantes_alvo.py:14; execução local, não registrada em arquivo]
   - **NÃO PROCEDE — chamador omitido encontrado.** A busca textual no repositório não revelou chamada de produção ou fixture à função real sem o novo argumento: produção passa pelas chamadas em `279`, `340`, `458` e `659–663`; os testes que omitem argumento o fazem para verificar `TypeError`. A busca não prova ausência de chamada construída dinamicamente sem o nome literal. [tarefa_do_dia.py:279; tarefa_do_dia.py:340; tarefa_do_dia.py:458; tarefa_do_dia.py:659; test_e4c_f6f7_c14_lei3_pela_leitura.py:151; busca textual, não registrada em arquivo] [nao verificado]

2. **Alcance do corte**

   **PROCEDE — o original fica intacto quanto aos dois carimbos nesse trecho.** O código faz `copy(p)`, altera atributos da cópia e passa essa cópia exclusivamente à leitura; o teste verifica os dois valores originais após as chamadas. A cópia é superficial, portanto isso não é prova geral de isolamento de todos os objetos relacionados. [tarefa_do_dia.py:403; test_e4c_f6f7_c14_lei3_pela_leitura.py:137] [nao verificado]

   Depois da leitura, os originais `_ps` ainda alimentam `linhas_da_projecao_sem_linha`, `esta_stale`, a seleção de âncoras e chamados; a carga também conserva as perguntas originais em `_perg` e as passa à tarefa. A leitura resultante pode, porém, mudar a razão e as ações publicadas: o efeito observável do corte não se limita ao valor interno da leitura. A face por pergunta chega à API; a carga alimenta, entre outros, fora da folha, retrato e vigia. [tarefa_do_dia.py:417; tarefa_do_dia.py:422; tarefa_do_dia.py:432; tarefa_do_dia.py:521; tarefa_do_dia.py:659; tarefa_do_dia.py:163; api/views.py:2544; folha/fora_da_folha.py:62; folha/retrato_folha.py:1212; chamados/services/vigia_fio.py:357]

   **PROCEDE — mesma fronteira temporal e mesmo `agora`:** o corte descarta `instante > agora`, conservando igualdade; o passivo conserva `ato_em <= agora`. Os predicados completos têm entradas diferentes: o passivo primeiro exige projeção sem linha correspondente, enquanto o novo corte atua diretamente sobre os dois campos da cópia. [tarefa_do_dia.py:407; tarefa_do_dia.py:417; models.py:4251]

3. **Exceção no contrato de vitalidade**

   - **NÃO PROCEDE — allowlist anterior zero para serviços.** Já havia entradas para funções de serviços, inclusive `validacao.py`, `relavra_via.py` e `raiox.py`. [test_contract_leitores_vitalidade.py:19; test_contract_leitores_vitalidade.py:40; test_contract_leitores_vitalidade.py:52]
   - **PROCEDE para os caminhos de produção examinados — a razão descreve a origem do valor.** A carga preenche o mapa com `DecisaoPergunta.leitura`, que retorna `LeituraDoAto`; `_pela_decisao` recebe esse resultado. A assinatura, por si só, não impõe o tipo, e fixtures passam objetos sintéticos. [tarefa_do_dia.py:413; tarefa_do_dia.py:458; tarefa_do_dia.py:528; models.py:4326; test_e3_tarefa_admin_sem_banco.py:126]
   - **PROCEDE — há alternativa sem essa entrada.** O contrato AST reconhece o nome do atributo e a forma sintática, sem distinguir a origem do objeto. Uma alteração explícita do contrato para reconhecer a leitura do ato permitiria preservar a verificação de carimbo cru sem isentar toda `_pela_decisao`; essa alternativa é uma proposta, não uma alteração aplicada nem validada. [test_contract_leitores_vitalidade.py:299; test_contract_leitores_vitalidade.py:333; test_contract_leitores_vitalidade.py:346] [nao verificado]

4. **Fixtures**

   **NÃO PROCEDE — não identifiquei assert removido, caso retirado ou tolerância ampliada no delta fornecido.** As alterações acrescentam principalmente mapas de leituras sintéticas, campos e um stub de `leitura`; os asserts mostrados permanecem. Isso não demonstra equivalência geral de cobertura: por exemplo, a fixture V8 fornece `validada_em=None` para todas as perguntas, sem reproduzir a leitura real. [e3_v8_apoio.py:126; test_e3_r3_c7_sem_banco.py:179; test_e3_r3_c7_sem_banco.py:195; test_e3_c8b_sem_banco.py:246]

5. **Mutantes do aval**

   **PROCEDE no conjunto dos quatro casos enumerados; fora dele, [não verificado].** Cada mutante é exigido como diferente do resultado correto no caso designado, e igual à função original nos outros três casos enumerados. Assim, a exclusividade demonstrada é apenas sobre essa matriz, não sobre todas as sequências possíveis. [test_e4c_f6f7_c14_lei3_pela_leitura.py:173; test_e4c_f6f7_c14_lei3_pela_leitura.py:192]

   **PROCEDE — sobrevivência faz o teste falhar**, desde que o teste seja executado: `assertNotEqual` falha se o mutante devolver o resultado esperado; `subTest` registra a falha no resultado do `unittest`, sem `try/except` que a suprima. O teste passou na execução por descoberta de módulo. Há a ressalva já indicada: execução direta do arquivo chega a `unittest.main()` antes da definição de `MutantesDoAval`, `HorizonteProjecao` e `LeituraObrigatoria`. [test_e4c_f6f7_c14_lei3_pela_leitura.py:101; test_e4c_f6f7_c14_lei3_pela_leitura.py:104; test_e4c_f6f7_c14_lei3_pela_leitura.py:166; test_e4c_f6f7_c14_lei3_pela_leitura.py:183; execução local, não registrada em arquivo] [nao verificado]

6. **Escritores e leitores dos valores relevantes**

   | Campo ou valor | Escrita e leitura verificadas |
   |---|---|
   | `respondida_em` e `resposta_colab` | A resposta humana escreve os dois e registra ato de resposta; resposta automática e carimbo pela trilha também os escrevem. A leitura usa a projeção para compor linhas passivas e devolve resposta vigente. [disputa_emissao.py:1481; disputa_emissao.py:1532; disputa_emissao.py:1682; disputa_emissao.py:1705; models.py:4259; models.py:4312] |
   | `validada_em` | A validação escreve o carimbo e o ato; devolução e resposta humana podem limpá-lo; o modelo também pode carimbá-lo. O corte escreve apenas `None` na cópia e a leitura usa o campo na composição da sequência. [validacao.py:161; validacao.py:269; disputa_emissao.py:1484; models.py:3325; tarefa_do_dia.py:407; models.py:4265] |
   | `ato`, `ato_em`, `resposta_mantem_validacao` | Escritores registram atos pela porta `registrar`; resposta humana grava `False`, automática e trilha gravam `True`, assim como a linha passiva de resposta. A carga filtra `ato_em`, a leitura reconstrói a vigência e `_pela_decisao` percorre a cauda. [models.py:4328; disputa_emissao.py:1532; disputa_emissao.py:1685; disputa_emissao.py:1707; models.py:4260; tarefa_do_dia.py:390; models.py:4308; tarefa_do_dia.py:158] |
   | `leituras[p.pk]`, `leitura.validada_em` | `_fatos_das_perguntas` constrói o mapa; a carga o guarda e o entrega a `_tarefa_pura`; `_pela_decisao` usa a validação vigente para selecionar o ato decisório. A face por pergunta usa o mesmo mapa. [tarefa_do_dia.py:404; tarefa_do_dia.py:528; tarefa_do_dia.py:659; tarefa_do_dia.py:163; tarefa_do_dia.py:458] |
   | `passivo`, `lavra`, `efeitos`, `fila` | `_fatos_das_perguntas` constrói esses insumos; `_pela_decisao` os lê para escolher razão, ações e porta. A lavra vem de `veredito_lavrado` sujeito a `esta_stale`; efeitos vêm de batidas; fila vem de `qs_na_fila`. [tarefa_do_dia.py:414; tarefa_do_dia.py:420; tarefa_do_dia.py:424; tarefa_do_dia.py:439; tarefa_do_dia.py:152] |

A lista acima identifica os escritores e leitores necessários para avaliar este delta; uma enumeração exaustiva de todos os consumidores e de todos os caminhos de escrita desses campos no repositório é **[não verificado]**. Não há resultado de produção nem execução da suíte inteira nos dados fornecidos: **[não verificado]**. [nao verificado]

## 18/11 20:31 · sessao 752bc2796335 · E4C-F6F7 · construtor · origem=Code

**AVAL-DEVOLVIDO — PRONTO (ii)–(iv):** esses casos já tinham o resultado esperado na ponta anterior; sua mordida foi exercitada com mutantes próprios. A contagem de produção solicitada no aval permanece **[não verificado]**: o dado fornecido não traz a contagem nem há acesso ao banco.

O diff está na cópia, sem commit. A certificação pela suite Code e pelo opus final permanece **[não verificado]**. [nao verificado]

- **A — leitura obrigatória:** assinaturas sem default e acesso por `leituras[p.pk]` nos dois pontos de decisão; o teste verifica ausência de argumento e chave ausente. [serviço:142](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:142), [serviço:196](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:196), [teste:151](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:151). [nao verificado]
- **B — horizonte:** a cópia entregue à leitura corta `respondida_em` e `validada_em` posteriores a `agora`; o corte do passivo permanece no lugar. O RED, executado antes da cura, retornou `silencio_decidido` onde se esperava `aguarda_validacao`; depois passou, inclusive o espelho no limite. [serviço:405](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:405), [teste:104](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:104). [nao verificado]
- **C — alvo:** a string do mutante de absorção acompanha a chamada nova; o teste de alvo único passou pela pista AST. [mutante:76](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/e3_v8_mutantes.py:76), [pista:96](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_volta7_sem_banco.py:96). [nao verificado]
- **D — contrato e fixtures:** o contrato detecta comparações com campos chamados `validada_em`, sem distinguir a origem. Registrei a exceção porque aqui o valor vem de `DecisaoPergunta.leitura`, juiz da sequência; não é o carimbo cru da pergunta nem julgamento de vitalidade por `pergunta_viva`. O contrato isolado passou com `SimpleTestCase` substituído por `unittest.TestCase`. [contrato:290](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_contract_leitores_vitalidade.py:290), [juiz:4303](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/models.py:4303). [nao verificado]
- **Fixtures:** `_leituras` faltava nas cargas sintéticas que pulam o construtor; uma delas também precisava preencher o mapa para as perguntas efetivamente carregadas. O dublê de `DecisaoPergunta` não fornecia `leitura`; e a fixture de pergunta fornecia `celula`, mas não a FK `celula_id`. Foram corrigidas nas fixtures, sem fallback no serviço. [carga:360](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_degrau_dono_e_porta.py:360), [mapa:246](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_c8b_sem_banco.py:246), [dublê:197](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_r3_c7_sem_banco.py:197), [FK:115](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:115).

Os mutantes **(ii), (iii) e (iv)** abaixo foram executados **em memória**, sem alterar o serviço. Para cada um, o teste também compara os outros três casos do aval com o comportamento original. [teste:166](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:166). [nao verificado]

**(ii) Recuar no máximo uma resposta**, substituição única em `tarefa_do_dia.py`:

```text
antiga:     while k and decisoes[k - 1].ato == D.RESPOSTA and decisoes[k - 1].resposta_mantem_validacao:
nova:       if k and decisoes[k - 1].ato == D.RESPOSTA and decisoes[k - 1].resposta_mantem_validacao:
```

**(iii) Dispensar leitura validada**, substituição única:

```text
antiga:     if 0 < k < len(decisoes) and leitura.validada_em is not None:
nova:       if 0 < k < len(decisoes) and True:
```

**(iv) Tornar transparente a resposta que desfaz**, substituição única do bloco:

```python
    while k and decisoes[k - 1].ato == D.RESPOSTA and decisoes[k - 1].resposta_mantem_validacao:
        k -= 1
    if 0 < k < len(decisoes) and leitura.validada_em is not None:
```

por:

```python
    while k and decisoes[k - 1].ato == D.RESPOSTA:
        k -= 1
    if 0 < k < len(decisoes) and (leitura.validada_em is not None or not decisoes[-1].resposta_mantem_validacao):
```

Comando da prova em memória para **cada um dos três**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura.MutantesDoAval -v
```

O teste completo da leitura e o teste da pista de alvos também passaram: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura -v
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_volta7_sem_banco -v
```

Os mutantes específicos das alterações A e B ainda **não foram executados** [não verificado]. A verificação local dos módulos sem banco selecionados passou: 60 testes. A execução integral com Django e o DIFF-RETIDO permanecem **[não verificado]**. [nao verificado]

Por erro no comando destinado ao contrato isolado, iniciei descoberta ampla de testes, contrariando a instrução de não rodar a suite; interrompi a execução ao perceber. Ela apresentou erros de importação por ausência de Django e não constitui resultado de suite. A extensão de possíveis efeitos dessa descoberta interrompida é **[não verificado]**. [nao verificado]

LEI-AKITA: origem=`chamados/services/tarefa_do_dia.py::_pela_decisao`, testemunha=`DecisaoPergunta.leitura`, RED=item 2 e horizonte, juizes novos=0, **padrao=juiz unico**. [serviço:142](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:142)

## 18/11 20:25 · sessao 69896ae881ff · E4C-F6F7 · verificador · origem=Code

**AVAL-DEVOLVIDO — BLOQUEIA.** A c14 consulta o juiz existente, mas a leitura alimentada à tarefa não é necessariamente a leitura da sequência completa da pergunta; além disso, pode incorporar uma validação da projeção posterior a `agora`. Os REDs (ii)–(iv) também não ficam vermelhos na ponta anterior, embora o aval exija que cada caso morda e que os quatro estejam vermelhos antes da cura. [tarefa_do_dia.py:387](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:387), [models.py:4295](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4295), [teste c14:34](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:34), [aval fornecido: não verificado em arquivo]

1. **BLOQUEIA — nem todos os caminhos garantem leitura.** Os três pontos de chamada são `expor_admin`, o ramo da instância e `exigencia_das_perguntas`. Os dois primeiros usam `(leituras or {}).get(p.pk)`: se o mapa ou a chave faltar, passam `None`; o terceiro indexa a leitura obrigatoriamente. A carga normal constrói e passa o mapa, mas `_tarefa_pura` aceita `leituras=None`, e há chamadas diretas de teste sem esse argumento. [tarefa_do_dia.py:196](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:196), [tarefa_do_dia.py:279](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:279), [tarefa_do_dia.py:340](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:340), [tarefa_do_dia.py:454](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:454), [tarefa_do_dia.py:522](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:522), [test_e3_c8b_sem_banco.py:187](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_c8b_sem_banco.py:187).

   Sim: é **default silencioso**. Sem leitura, a guarda falha e `V→R(m)` termina em `aguarda_validacao`, mesmo que a leitura correta fosse validada. Isso não demonstra que a carga normal omita a leitura; demonstra que a interface permite a omissão e produz uma resposta comum, sem sinalizar desconhecimento. [tarefa_do_dia.py:142](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:158](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:158).

2. **BLOQUEIA — sequência e horizonte alteram a resposta do juiz.** A carga remove atos de ressuscitação e atos reais posteriores a `agora`; a leitura comum usa todas as decisões da pergunta e não tem relógio. Portanto, por exemplo, uma reabertura de ressuscitação ou uma resposta que desfaz posterior ao horizonte pode limpar a validação na leitura completa e estar ausente da leitura alimentada à tarefa; uma validação real posterior pode fazer o inverso. [tarefa_do_dia.py:387](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:387), [models.py:4221](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4221), [models.py:4295](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4295).

   O literal fornecido manda perguntar a `DecisaoPergunta.leitura(p)` pela sequência de atos de uma pergunta; não autoriza expressamente substituir sua sequência pela filtrada. A exigência temporal da carga está documentada no código, mas a conciliação das duas obrigações **não fecha no literal apresentado**; não proponho reabrir a regra. [aval fornecido: não verificado em arquivo], [tarefa_do_dia.py:489](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:489).

   Há ainda um efeito temporal específico: **o ato real futuro sai de `dec`, mas pode voltar como linha passiva sintética**. A cópia conserva `p.validada_em`; como o `ja` é formado somente pelos atos cortados, uma validação projetada posterior a `agora` pode ser considerada sem linha e adicionada à leitura, sem corte temporal. O conjunto `passivo`, calculado separadamente, descarta essa linha futura e não bloqueia a decisão. Exemplo condicionado: `S→R(m)` até agora e validação projetada depois podem fazer a leitura dizer validada e a tarefa escolher `silencio_decidido`. [tarefa_do_dia.py:398](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:398), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:403), [tarefa_do_dia.py:413](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:413), [models.py:4265](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4265), [models.py:4308](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4308).

3. **OK quanto ao cache; BLOQUEIA quanto ao conteúdo, pelo item 2.** `leitura` chama `p.decisoes.all()` e seu próprio contrato diz que essa chamada usa o cache de prefetch. Ela monta `ja` a partir dos reais recebidos e o passa a `linhas_da_projecao_sem_linha`, evitando a query própria que essa função faria se `ja` fosse omitido. Assim, neste caminho, lê a lista `dec` injetada **mais a projeção passiva em memória**, inclusive a ressalva temporal acima. [models.py:4249](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4249), [models.py:4304](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4304), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:403).

4. **FILA — as duas fixtures apontadas não comprovam `V→R(m)`.** Na fixture c8b alterada, a sequência construída é uma resposta que **não** mantém validação; nas duas r3/c5, `_tarefa_pura` é substituída por um stub e `_dec` está vazio. Portanto, não há base nesses trechos para afirmar que seus casos antigos `V→R(m)` passam pelo motivo errado: **não identifiquei tal caso nessas fixtures**. [test_e3_c8b_sem_banco.py:200](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_c8b_sem_banco.py:200), [test_e3_r3_c5_sem_banco.py:305](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_r3_c5_sem_banco.py:305), [test_e3_r3_c5_sem_banco.py:336](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_r3_c5_sem_banco.py:336).

   Já a fixture c13 fornece uma leitura **sintética**, calculada pela última linha; seu `V→R(m)` verifica a propagação da leitura fornecida e o resultado, mas não verifica que a carga obteve a leitura real. O default silencioso continua sendo um risco para outros testes que omitirem o mapa. [test_e4c_f6f7_c13_caso_pela_regra.py:24](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:24), [test_e4c_f6f7_c13_caso_pela_regra.py:49](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:49).

5. **BLOQUEIA — só (i) é RED contra a ponta anterior.** Pelo diff fornecido, a guarda anterior pulava a cauda quando o ato anterior era validação. Assim, (ii) já chegava à lavra; (iii) e (iv) já chegavam a `aguarda_validacao`. Isso contraria o requisito literal de os quatro estarem vermelhos antes da cura; não basta que sejam úteis como regressão. [diff fornecido: não verificado em arquivo], [teste c14:34](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:34), [aval fornecido: não verificado em arquivo].

   Mutantes que cada asserção mataria, por inspeção:

   - **(i)** trocar a consulta à leitura pela adjacência à validação; o teste explícito demonstra o resultado mutado esperado. [teste c14:85](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:85).
   - **(ii)** impedir o recuo sobre duas respostas mantidas, por exemplo permitindo recuar apenas uma. [tarefa_do_dia.py:160](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:160), [teste c14:37](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:37).
   - **(iii)** retirar a exigência de leitura validada e sempre pular a cauda mantida. [tarefa_do_dia.py:163](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:163), [teste c14:38](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:38).
   - **(iv)** tratar também resposta que desfaz como transparente e pular para a validação. [tarefa_do_dia.py:161](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:161), [teste c14:39](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:39).

   Essas mortes, exceto a adjacência expressamente codificada, são previsões por inspeção; execução dos mutantes **[não verificado]**. O selo AST cobre as formas sintáticas previstas e seu mutante, não prova toda possível forma de derivar validação. [teste c14:56](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:56).

6. **FILA — escritores e leitores; mudanças possíveis nos consumidores.**

   | Valor | Quem escreve ou produz | Quem lê |
   |---|---|---|
   | `validada_em` da projeção e ato `VALIDACAO` | `_carimbar_validacao` grava ambos; devolução e resposta humana podem limpar o carimbo e registrar outro ato. [validacao.py:161](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/validacao.py:161), [validacao.py:269](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/validacao.py:269), [disputa_emissao.py:1494](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/disputa_emissao.py:1494) | `linhas_da_projecao_sem_linha` usa o carimbo; `leitura` produz seu próprio `validada_em`; a c14 consulta este último. [models.py:4251](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4251), [models.py:4317](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4317), [tarefa_do_dia.py:163](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:163) |
   | `decisoes` e `resposta_mantem_validacao` | `registrar` é a porta declarada para a sequência; resposta humana registra `False`, automática e pela trilha registram `True`. [models.py:4329](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4329), [disputa_emissao.py:1532](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/disputa_emissao.py:1532), [disputa_emissao.py:1686](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/disputa_emissao.py:1686) | `leitura` interpreta os atos; a tarefa carrega e filtra a sequência e examina a cauda. [models.py:4308](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4308), [tarefa_do_dia.py:390](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:390) |
   | `leituras` e razão/ações do dia | `_fatos_das_perguntas` produz o mapa em memória; `_pela_decisao` produz razão e ações, inseridas na tarefa. A alteração não grava esses valores no banco. [tarefa_do_dia.py:404](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:404), [tarefa_do_dia.py:340](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:340) | `_portas_da_tarefa` transforma ações e razões em fio/rota; os consumidores leem a porta publicada. [tarefa_do_dia.py:701](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:701) |

   **Sim, podem ver valores diferentes**, dependendo das demais ações e portas do dia: `V→S→R(m)` troca uma ação de validar por silêncio sem ação; isso pode mudar fio, rota e classificações derivadas. Folha lê a porta; retrato lê folha; cadastro, vigia e pautas também consomem essa saída. Não se pode afirmar que cada consumidor mudará em toda ocorrência, pois a porta agrega outras ações. [tarefa_do_dia.py:165](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:165), [tarefa_do_dia.py:711](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:711), [fora_da_folha.py:70](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/folha/fora_da_folha.py:70), [retrato_folha.py:1212](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/folha/retrato_folha.py:1212), [cadastro_realidade.py:466](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/escala/services/cadastro_realidade.py:466), [vigia_fio.py:359](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/vigia_fio.py:359), [pautas_dos_discordantes.py:85](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/ponto/services/pautas_dos_discordantes.py:85).

7. **BLOQUEIA / FILA — classes de reprovação.**

   - **Presente:** RED sem mordida na ponta anterior, item 5; e leitura com universo diferente do juiz comum, item 2. A função do juiz é reutilizada, mas o cache adulterado faz sua resposta depender de uma sequência parcial: isso é uma derivação divergente da fonte, mesmo sem criar outra função juiz. [models.py:4295](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4295), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:403).
   - **Presente como risco de selo:** a alteração da chamada deixou o alvo literal do mutante `absorcao_admin_sem_rotulo` com a assinatura antiga; a catraca exige exatamente um alvo. A inclusão desse mutante na matriz cobrada pela catraca **[não verificado]**. [e3_v8_mutantes.py:76](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/e3_v8_mutantes.py:76), [test_e3_mutantes_alvo.py:20](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_mutantes_alvo.py:20), [tarefa_do_dia.py:279](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:279).
   - **Não demonstrados no diff:** novo escritor persistente, relógio lido fora da borda, mudança da ordem de trava ou pouso. Ausência global dessas classes e situação do pouso **[não verificado]**. O campo de leitura pode significar leitura integral em `models.py` e leitura de sequência cortada nesta carga; a diferença semântica está demonstrada no item 2. [tarefa_do_dia.py:374](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:374), [models.py:4295](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4295).

8. **FILA — a regra do ramo existe, mas o passivo intercepta o caminho normal.** Sem linha passiva posterior, `V→DEVOLUCAO→R(m)` dá leitura não validada e, portanto, `aguarda_validacao`; o mesmo vale para recusa e reabertura, pois limpam a validação. Se uma leitura fornecida disser validada, a função recua e escolhe o último ato não transparente: devolução → `materializacao_recusada`; recusa/reabertura → `devolvida_ao_colaborador`. Isso corresponde ao literal do aval e à triagem fornecida. [models.py:4272](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4272), [tarefa_do_dia.py:160](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:160), [triagem fornecida: não verificado em arquivo].

   Porém, se a validação vier de passivo cuja data é até `agora`, a carga inclui a pergunta em `passivo` e `_pela_decisao` retorna `passivo_sem_linha` **antes** de aplicar aquele ramo. Assim, a afirmação da triagem de que o caso passivo chega ao ramo do ato não está demonstrada para a carga normal; há tensão com o literal nesse caminho. Passivo futuro tem a anomalia separada do item 2. [tarefa_do_dia.py:154](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:154), [tarefa_do_dia.py:413](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:413).

A sombra fornecida reporta zero perguntas com validação antes de silêncio nas 51.212 examinadas e zero candidatas `V→S` para a segunda medição; são resultados dos anexos raspados, sem acesso independente ao banco ou aos programas da sonda **[não verificado]**. Não executei a suíte nem os mutantes; seus resultados de execução **[não verificado]**. [nao verificado]

## 18/11 20:21 · sessao 4fb0642f9619 · E4C-F9 · construtor · origem=Code

Implementei os três ajustes na cópia. A certificação pela suite e pelo opus final permanece pendente. [não verificado] [nao verificado]

- **B1:** a guarda conta respostas divergentes e as exclui antes da classificação e da lista usada pelo `--apply`. O novo teste usa escolha com projeção `Sim`, ato `Nao`, classificador real e um controle concordante. ([comando](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:62), [teste](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:122)) [nao verificado]
- **B2:** o selo conta ocorrências por arquivo, função, campo e exceção; examina também strings nas formas solicitadas. O teste injeta cada forma em memória para ambos os campos e acrescenta ocorrências dentro de I18 e I20. ([selo](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:141), [mutações em memória](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:202)) [nao verificado]
- **B3:** acrescentei somente os comentários de classe nos dois sítios FICA. ([backfill](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [limbo](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/curar_perguntas_em_limbo.py:46))

**RED/GREEN e mutantes.** O RED funcional foi preparado antes da guarda, mas a tentativa local parou na importação: o Python disponível não encontrou Django. Portanto, RED e GREEN funcionais são **[não verificado]**. Sem banco, executei isoladamente os dois testes do selo: passaram; os mutantes do selo abaixo produziram respectivamente 20 e 2 falhas de subtestes, sem erros. ([teste funcional](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:122), [teste do selo](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:202)) [nao verificado]

Para a pista, use `--only "chamados.tests.test_e4c_f9_comandos_leem_o_ato"` para GREEN e, separadamente, cada mutante:

1. **Guarda B1:** em [validar_classe_a.py](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), remova as três linhas do `if` até o `continue`. O teste `test_M7_divergencia_escolha_nao_valida` deve falhar. Resultado na pista **[não verificado]**. [nao verificado]
2. **Strings B2:** em [teste do selo](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:156), substitua `elif isinstance(node, ast.Constant) and isinstance(node.value, str):` por `elif False:`. O teste de mutações deve falhar para as formas em string. [nao verificado]
3. **Contagem B2:** no [mesmo selo](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:185), substitua `contagens[chave] = contagens.get(chave, 0) + 1` por `contagens[chave] = 1`. O teste deve falhar para a ocorrência extra em cada `add`. [nao verificado]

**Escritores e leitores nos sítios tocados.** A resposta é escrita pela porta de resposta; validação é escrita pela porta de validação, e há limpeza desses campos em devolução e reabertura. O comando de classe A lê o ato na seleção e na guarda, compara a projeção e chama o classificador que ainda lê a projeção; a escrita sob `--apply` continua na porta de validação. O backfill lê a projeção para identificar linhas ausentes, e o comando de limbo lê `resposta_colab` da projeção. ([resposta](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/disputa_emissao.py:1480), [validação](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/validacao.py:167), [devolução](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/validacao.py:273), [reabertura](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/disputa_emissao.py:1617), [classificador](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:2034), [backfill](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/backfill_decisoes_pergunta.py:57), [limbo](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/curar_perguntas_em_limbo.py:47))

**LEI-AKITA:** origem=[validar_classe_a.py](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63); testemunha=[DecisaoPergunta.leitura](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4295); RED=[test_e4c_f9_comandos_leem_o_ato](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:122); quem-mais-le, no alcance desta composição: [sem_veredito_entre](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:2003), [classificador](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:2034), chamado também por [API](/home/ronald/wt-codex-e4c-f9-191111/app/api/views_mensageria.py:545) e [tags](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/templatetags/chamados_tags.py:171) / [tags](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/templatetags/chamados_tags.py:198); I21 e C2 combinam validação pelo ato com faces de resposta existentes ([I21](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1586), [C2](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1617)); juízes novos=0; padrão=projeção reconstruível. O censo de todos os leitores do repositório é **[não verificado]**.

## 18/11 20:21 · sessao 9d3adcfafcd1 · E4C-F6F7 · sonda · origem=Code

`SILENCIO` pode ser uma linha de `DecisaoPergunta`: ela é registrada no fechamento da disputa em [models.py:2083](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/models.py:2083), no fechamento do fio do dia em [veredito_celula.py:190](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/ponto/services/veredito_celula.py:190) e no backfill em [backfill_decisoes_pergunta.py:349](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/management/commands/backfill_decisoes_pergunta.py:349).

**Há também carimbo sem linha `SILENCIO`**: `fechar_por_admin` grava a via `admin_decidiu` por `carimbar_identidade`, sem registrar o ato por si só ([models.py:3246](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/models.py:3246), [models.py:3364](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/models.py:3364)). No aceite de folga, há chamadas a essa porta após o carimbo de validação, sem registro explícito de `SILENCIO` para a própria pergunta ([validacao.py:519](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/services/validacao.py:519), [validacao.py:551](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/services/validacao.py:551)). O backfill também identifica projeções `admin_decidiu` sem linha de silêncio ou validação ([backfill_decisoes_pergunta.py:120](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/management/commands/backfill_decisoes_pergunta.py:120)). A quantidade dessas situações em produção é **[não verificado]**.

A sonda abaixo usa as constantes `ATOS` e o predicado `es_da_ressuscitacao` do modelo ([models.py:4152](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/models.py:4152), [models.py:4221](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/models.py:4221)). A contagem por ato inclui **todas** as linhas; as sequências usam o teto `agora`, como a sonda fornecida pelo usuário **[não verificado: anexo fornecido na conversa]**. O resultado da nova sonda é **[não verificado: script não executado]**.

```python
from collections import Counter
from django.db import connection
from django.utils import timezone
from chamados.models import DecisaoPergunta as D, PerguntaDisputa

def antivac():
    erros = Counter()
    atos = Counter()
    bigramas = Counter()
    totais = Counter()
    completo = False

    def erro(etapa, exc):
        erros[etapa] += 1
        # Não imprime a mensagem da exceção: ela pode conter dados pessoais.
        print("ERRO", "etapa=%s" % etapa, "tipo=%s" % type(exc).__name__)

    try:
        if getattr(connection, "schema_name", None) != "juliani":
            raise RuntimeError("schema diferente de juliani")

        agora = timezone.now()
        conhecidos = {valor for valor, _ in D.ATOS}
        universo = PerguntaDisputa.objects.count()
        print("SCHEMA juliani")
        print("AGORA", agora.isoformat())
        print("UNIVERSO", universo)

        pergunta_atual = None
        sequencia = []

        def fechar_sequencia(seq):
            if not seq:
                return
            totais["perguntas_com_sequencia"] += 1
            viu_validacao = False
            tem_silencio = False
            validacao_antes = False
            for ato, mantem in seq:
                if ato == D.SILENCIO:
                    tem_silencio = True
                    if viu_validacao:
                        validacao_antes = True
                if ato == D.VALIDACAO:
                    viu_validacao = True

            totais["algum_silencio"] += int(tem_silencio)
            totais["validacao_antes_silencio"] += int(validacao_antes)
            for esquerda, direita in zip(seq, seq[1:]):
                bigramas[(esquerda[0], direita[0])] += 1

            k = len(seq)
            while k and seq[k - 1][0] == D.RESPOSTA and seq[k - 1][1]:
                k -= 1
            totais["silencio_ultimo_nao_R_mantem"] += int(
                k > 0 and seq[k - 1][0] == D.SILENCIO
            )

        linhas = (
            D.objects.only(
                "pk", "pergunta_id", "ato", "ato_em",
                "da_ressuscitacao", "resposta_mantem_validacao",
            )
            .order_by("pergunta_id", "ato_em", "pk")
            .iterator(chunk_size=2000)
        )
        for d in linhas:
            atos[d.ato] += 1
            if d.ato_em > agora or D.es_da_ressuscitacao(d):
                continue
            if pergunta_atual != d.pergunta_id:
                fechar_sequencia(sequencia)
                pergunta_atual = d.pergunta_id
                sequencia = []
            sequencia.append((d.ato, d.resposta_mantem_validacao))

        fechar_sequencia(sequencia)
        completo = True

        print("ATOS_TODAS_AS_LINHAS")
        for valor, _ in D.ATOS:
            print("ato=%s quantidade=%s" % (valor, atos[valor]))
        print("ATOS_FORA_DAS_CONSTANTES", sum(
            n for ato, n in atos.items() if ato not in conhecidos
        ))

        print("PERGUNTAS_ATE_AGORA_SEM_RESSUSCITACAO")
        for chave in (
            "perguntas_com_sequencia",
            "algum_silencio",
            "validacao_antes_silencio",
            "silencio_ultimo_nao_R_mantem",
        ):
            print("%s=%s" % (chave, totais[chave]))

        print("TOP_10_BIGRAMAS_ATE_AGORA_SEM_RESSUSCITACAO")
        # Não imprime valores desconhecidos, que poderiam conter dados pessoais.
        seguros = Counter({
            par: n for par, n in bigramas.items()
            if par[0] in conhecidos and par[1] in conhecidos
        })
        for (a, b), n in sorted(
            seguros.items(), key=lambda item: (-item[1], item[0])
        )[:10]:
            print("ato_i=%s ato_i+1=%s quantidade=%s" % (a, b, n))
        print("BIGRAMAS_COM_ATO_DESCONHECIDO", sum(
            n for (a, b), n in bigramas.items()
            if a not in conhecidos or b not in conhecidos
        ))

    except Exception as exc:
        erro("execucao", exc)
    finally:
        print("COMPLETO", completo)
        print("ERROS", sum(erros.values()), "POR_ETAPA", dict(erros))
        print("FIM_ANTIVAC")

antivac()
```

A validação anterior é histórica: outro ato entre ela e o silêncio não a invalida para essa contagem. A retirada de respostas que mantêm validação vale apenas para encontrar o último ato não-`R(mantem)`; os bigramas conservam essas respostas, conforme a distinção de `resposta_mantem_validacao` no modelo ([models.py:4189](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/models.py:4189)). O script sinaliza valores de ato desconhecidos sem imprimi-los; se existirem, o top 10 exibido cobre apenas pares de constantes conhecidas **[não verificado: script proposto]**.

## 18/11 20:18 · sessao a31e6ebb15f5 · E4C-F6F7 · sonda · origem=Code

A sequência considerada abaixo usa os atos reais até o instante da sonda, na ordem `ato_em, pk`, e exclui o ato compensador de ressuscitação como faz a carga da tarefa. O padrão exige `VALIDACAO` imediatamente seguida de `SILENCIO`, antes da cauda de respostas que mantêm a validação. [app/chamados/services/tarefa_do_dia.py:393](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/services/tarefa_do_dia.py:393)

Em B, a lavra `'nao'` é o campo `veredito_lavrado` com valor `'nao'` e vigente segundo `esta_stale`; a carga usa `marcos_do_lote` para essa verificação. [app/chamados/services/tarefa_do_dia.py:410](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/services/tarefa_do_dia.py:410)

Funções do sistema chamadas diretamente pelo script:

- `DecisaoPergunta.es_da_ressuscitacao`: [app/chamados/models.py:4220](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/models.py:4220).
- `DecisaoPergunta.leitura`: [app/chamados/models.py:4294](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/models.py:4294).
- `marcos_do_lote` e `esta_stale`: [app/chamados/veredito_lavrado.py:151](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/veredito_lavrado.py:151), [app/chamados/veredito_lavrado.py:213](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/veredito_lavrado.py:213).
- `tarefa_do_dia`: [app/chamados/services/tarefa_do_dia.py:800](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/services/tarefa_do_dia.py:800). Sua razão pode estar no dia ou em instâncias associadas à pergunta; o script distingue pergunta não exposta. [app/chamados/services/tarefa_do_dia.py:217](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/services/tarefa_do_dia.py:217), [app/chamados/services/tarefa_do_dia.py:341](/home/ronald/codex_ws/146323e2509a7be94e0861402b207c442694cd65/app/chamados/services/tarefa_do_dia.py:341)

Os números de produção e a execução do script estão **[não verificado]**. `colN` é um apelido criado apenas na execução, compartilhado entre as duas listas; empresa é identificada por PK. A competência é rotulada pelo mês em que termina o intervalo de 21 a 20.

```python
from collections import Counter
from django.db import connection
from django.utils import timezone

from chamados.models import DecisaoPergunta as D, PerguntaDisputa
from chamados.services.tarefa_do_dia import tarefa_do_dia
from chamados.veredito_lavrado import esta_stale, marcos_do_lote


def sonda():
    erros = Counter()
    totais = Counter()
    razoes_a = Counter()
    grupos_b = Counter()
    linhas_a = []
    linhas_b = []
    apelidos = {}

    def erro(pk, etapa, exc):
        erros[etapa] += 1
        print("ERRO", "pk=%s" % pk, "etapa=%s" % etapa,
              "tipo=%s" % type(exc).__name__)

    def col_n(colaborador_id):
        if colaborador_id not in apelidos:
            apelidos[colaborador_id] = "col%s" % (len(apelidos) + 1)
        return apelidos[colaborador_id]

    def competencia_21_20(data):
        if data is None:
            return "data_evento_nula"
        ano, mes = data.year, data.month
        if data.day >= 21:
            if mes == 12:
                ano, mes = ano + 1, 1
            else:
                mes += 1
        return "%04d-%02d" % (ano, mes)

    try:
        if getattr(connection, "schema_name", None) != "juliani":
            raise RuntimeError("schema diferente de juliani")

        agora = timezone.now()
        universo = PerguntaDisputa.objects.count()
        print("SCHEMA juliani")
        print("AGORA", agora.isoformat())
        print("UNIVERSO", universo)

        perguntas = (
            PerguntaDisputa.objects.select_related(
                "colaborador",
                "disputa__colaborador__empresa",
                "disputa__chamado",
                "celula",
                "chamado_destino",
            )
            .prefetch_related("decisoes")
            .order_by("pk")
        )

        for p in perguntas.iterator(chunk_size=500):
            totais["examinadas"] += 1
            try:
                decisoes = [
                    d for d in p.decisoes.all()
                    if d.ato_em <= agora and not D.es_da_ressuscitacao(d)
                ]
                # A ordenação explícita coincide com a carga da tarefa.
                decisoes.sort(key=lambda d: (d.ato_em, d.pk))

                k = len(decisoes)
                while (
                    k > 0
                    and decisoes[k - 1].ato == D.RESPOSTA
                    and decisoes[k - 1].resposta_mantem_validacao
                ):
                    k -= 1

                padrao_vs = (
                    k >= 2
                    and decisoes[k - 2].ato == D.VALIDACAO
                    and decisoes[k - 1].ato == D.SILENCIO
                )
                if not padrao_vs:
                    continue

                tem_cauda = k < len(decisoes)
                if tem_cauda:
                    totais["A_padrao"] += 1
                totais["B_candidatas_VS"] += 1
            except Exception as exc:
                erro(p.pk, "sequencia", exc)
                continue

            # A leitura é independente da chamada à tarefa e da lavra:
            # uma falha em uma delas não esconde os demais resultados.
            validada = None
            try:
                validada = D.leitura(p).validada_em is not None
                if tem_cauda and validada:
                    totais["A_leitura_validada"] += 1
            except Exception as exc:
                erro(p.pk, "leitura", exc)

            if tem_cauda:
                try:
                    if p.data_evento is None:
                        raise ValueError("data_evento nula")
                    tarefa = tarefa_do_dia(p.colaborador, p.data_evento, agora)
                    razao_dia = tarefa["razao"]
                    razoes_pergunta = tuple(sorted(
                        linha["razao"]
                        for linha in tarefa["instancias"]
                        if linha.get("pergunta_id") == p.pk
                    ))
                    if not razoes_pergunta:
                        razoes_pergunta = ("pergunta_nao_exposta",)
                    chave = (razao_dia, razoes_pergunta)
                    razoes_a[chave] += 1
                    if len(linhas_a) < 30:
                        linhas_a.append(
                            (p.pk, col_n(p.colaborador_id), p.data_evento,
                             razao_dia, razoes_pergunta)
                        )
                except Exception as exc:
                    erro(p.pk, "tarefa_do_dia", exc)
                    razoes_a[("ERRO", ())] += 1
                    if len(linhas_a) < 30:
                        linhas_a.append(
                            (p.pk, col_n(p.colaborador_id), p.data_evento,
                             "ERRO", ())
                        )

            # B exige validação vigente pela leitura e lavra 'nao' vigente.
            # Com leitura desconhecida, registra a falha e não classifica B.
            if validada is True:
                try:
                    if p.veredito_lavrado != "nao":
                        continue
                    marcos = marcos_do_lote([p.colaborador_id], ate=agora)
                    if esta_stale(p, marcos, agora):
                        continue

                    competencia = competencia_21_20(p.data_evento)
                    empresa_pk = p.disputa.colaborador.empresa_id
                    grupos_b[(competencia, empresa_pk)] += 1
                    totais["B_lavra_nao_vigente"] += 1
                    if len(linhas_b) < 30:
                        linhas_b.append(
                            (p.pk, col_n(p.colaborador_id), p.data_evento,
                             competencia, empresa_pk)
                        )
                except Exception as exc:
                    erro(p.pk, "lavra_B", exc)

        print("A_POR_RAZAO")
        for (razao_dia, razoes_pergunta), quantidade in sorted(
            razoes_a.items(), key=lambda item: repr(item[0])
        ):
            print(
                "razao_dia=%s" % (razao_dia or "<vazia>"),
                "razoes_pergunta=%s" % (",".join(razoes_pergunta) or "<nenhuma>"),
                "quantidade=%s" % quantidade,
            )

        print("A_LISTA_ATE_30")
        for pk, col, data, razao_dia, razoes_pergunta in linhas_a:
            print(
                "pk=%s" % pk, col, "data_evento=%s" % data,
                "razao_dia=%s" % (razao_dia or "<vazia>"),
                "razoes_pergunta=%s" % (",".join(razoes_pergunta) or "<nenhuma>"),
            )

        print("B_POR_COMPETENCIA_EMPRESA_PK")
        for (competencia, empresa_pk), quantidade in sorted(
            grupos_b.items(), key=lambda item: (item[0][0], str(item[0][1]))
        ):
            print(
                "competencia=%s" % competencia,
                "empresa_pk=%s" % empresa_pk,
                "quantidade=%s" % quantidade,
            )

        print("B_LISTA_ATE_30")
        for pk, col, data, competencia, empresa_pk in linhas_b:
            print(
                "pk=%s" % pk, col, "data_evento=%s" % data,
                "competencia=%s" % competencia, "empresa_pk=%s" % empresa_pk,
            )

    except Exception as exc:
        erro("GLOBAL", "execucao", exc)
    finally:
        print(
            "TOTAIS",
            "universo=%s" % locals().get("universo", "ERRO"),
            "examinadas=%s" % totais["examinadas"],
            "A_padrao=%s" % totais["A_padrao"],
            "A_leitura_validada=%s" % totais["A_leitura_validada"],
            "B_candidatas_VS=%s" % totais["B_candidatas_VS"],
            "B_lavra_nao_vigente=%s" % totais["B_lavra_nao_vigente"],
            "erros=%s" % sum(erros.values()),
            "erros_por_etapa=%s" % dict(erros),
        )
        print("FIM_SONDA_LEI3")


sonda()
```

## 18/11 20:18 · sessao f7ae3099fc8f · E4C-F6F7 · construtor · origem=Code

A cura está na cópia: `_pela_decisao` usa `leitura.validada_em` para decidir se deve atravessar a cauda de respostas que mantêm a validação; o ato encontrado continua seguindo seu ramo existente ([tarefa_do_dia.py:142](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:142)).

As duas contagens solicitadas da frota são **[não verificado]**: o dado fornecido não contém as sequências nem os totais necessários.

- A carga calcula `DecisaoPergunta.leitura` para cada pergunta usando uma cópia com as decisões já carregadas no cache, e passa o resultado junto com as decisões ([tarefa_do_dia.py:374](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:374), [models.py:4295](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/models.py:4295)). Pela implementação, não há consulta adicional por pergunta; a contagem de queries **[não verificado]**.
- O RED executado antes da cura falhou em `V→S→R(m)`, `V→RETRATAÇÃO→R(m)` e `V→ABSORÇÃO→R(m)`; os três controles passaram ([teste c14:34](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:34)). [nao verificado]
- Após a cura, c14 e c13 passaram: 4 testes. A catraca AST e seus casos mutados em memória estão no teste c14 ([teste c14:56](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:56)). Os testes de carga em memória ajustados também passaram: 40 testes ([c8b:244](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_c8b_sem_banco.py:244), [c5:312](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_r3_c5_sem_banco.py:312)). [nao verificado]

Comandos, a partir de `app/`:

```bash
# RED: executar sobre a base, com o teste c14 novo presente e antes da cura [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura.PelaLeitura

# GREEN, incluindo c13 e selo
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura chamados.tests.test_e4c_f6f7_c13_caso_pela_regra

# Mutantes em memória: adjacência e leitor intruso da catraca
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura.Selo

# Verificação das cargas sintéticas ajustadas
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_c8b_sem_banco chamados.tests.test_e3_r3_c5_sem_banco
```

Mutante solicitado, em [tarefa_do_dia.py:163](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:163), substitui exatamente:

```python
leitura is not None and leitura.validada_em is not None
```

por:

```python
decisoes[k - 1].ato == D.VALIDACAO
```

Com essa substituição, o caso `V→S→R(m)` volta a `aguarda_validacao`, contrariando o RED; o teste do mutante demonstra isso em memória ([teste c14:85](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:85)). [nao verificado]

Censo relevante: `validada_em` é escrito, entre outros pontos, pela validação e pelos fluxos de resposta/reabertura ([validacao.py:168](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/validacao.py:168), [disputa_emissao.py:1494](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/disputa_emissao.py:1494)); `resposta_mantem_validacao` é declarado pelos escritores de resposta e lido pela leitura do ato ([disputa_emissao.py:1534](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/disputa_emissao.py:1534), [disputa_emissao.py:1688](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/disputa_emissao.py:1688), [models.py:4292](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/models.py:4292)). Chamam `_pela_decisao` os dois pontos de `_tarefa_pura` e `exigencia_das_perguntas` ([tarefa_do_dia.py:279](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:279), [tarefa_do_dia.py:340](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:340), [tarefa_do_dia.py:454](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:454)). O censo exaustivo de todos os leitores dos campos **[não verificado]**.

O DIFF-RETIDO específico do REPROVAS **[não verificado]**: comparei arquivos com `main`, mas o conteúdo que define aquele diff não foi fornecido. Suite inteira e certificação final **[não verificado]**. [nao verificado]

LEI-AKITA: origem=chamados/services/tarefa_do_dia.py::_pela_decisao, testemunha=DecisaoPergunta.leitura, RED=test_e4c_f6f7_c14_lei3_pela_leitura, quem-mais-le=tarefa/porta/fora_da_folha ([c13:57](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:57)); demais leitores exaustivos=[não verificado], juizes novos=0 ([diff da cura](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:404)), padrao=juiz unico.

## 18/11 20:14 · sessao 2830c5318874 · E4C-F9 · verificador · origem=Code

**Resultado: BLOQUEIA.** Os sete pontos M1–M7 foram alterados nos destinos indicados, mas há três achados procedentes: M7 pode exibir uma resposta diferente daquela usada para classificar e validar; o selo AST não cobre todas as formas de leitura crua; e faltam os comentários exigidos nos dois comandos FICA que o próprio brief proíbe editar. ([validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [test_e4c_f9_comandos_leem_o_ato.py:123](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:123), [backfill_decisoes_pergunta.py:56](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [curar_perguntas_em_limbo.py:46](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/curar_perguntas_em_limbo.py:46); brief fornecido [não verificado como arquivo])

Usei a cópia indicada, cujo `HEAD` corresponde ao hash informado; os resultados de execução abaixo são os **vereditos fornecidos**, não execuções independentes. [não verificado]

1. **M1–M7 e FICA**

| Item | Resultado |
|---|---|
| **M1 — OK** | O rótulo consulta `leitura(_p).validada_em`. ([auditar_invariantes_chamados.py:206](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:206)) |
| **M2 — OK** | I21 usa `q_nao_validada_pelo_ato()`. ([auditar_invariantes_chamados.py:1586](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1586)) |
| **M3 — OK** | C2 usa a mesma face SQL. ([auditar_invariantes_chamados.py:1617](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1617)) |
| **M4 — OK** | F2 usa a mesma face SQL. ([auditar_invariantes_chamados.py:1781](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1781)) |
| **M5 — OK** | F3 usa a mesma face SQL. ([auditar_invariantes_chamados.py:1794](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1794)) |
| **M6 — OK quanto à troca solicitada** | O filtro usa a mesma face SQL e conserva o filtro de materialização. O efeito da escrita está no item 2. ([encerrar_perguntas_com_fato.py:31](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:31)) |
| **M7 — BLOQUEIA pela composição do comando** | A impressão usa o texto do ato, como pedido, mas a classificação continua usando a coluna; veja item 5. ([validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [validar_classe_a.py:80](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:80)) |

**FICA — BLOQUEIA por instruções incompatíveis.** Não identifiquei mudança de predicado nos FICA do diff: I18 e I20 permanecem na projeção, com comentários; os dois comandos FICA externos aos três comandos alterados também permaneceram sem alteração. Porém, neles faltam os comentários de uma linha pedidos pelo brief, que ao mesmo tempo proíbe editá-los. ([auditar_invariantes_chamados.py:1081](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1081), [auditar_invariantes_chamados.py:1113](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1113), [backfill_decisoes_pergunta.py:56](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [curar_perguntas_em_limbo.py:46](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/curar_perguntas_em_limbo.py:46); brief fornecido [não verificado como arquivo])

2. **M6 e `--apply` — OK quanto à classe do efeito; impacto em produção não verificado**

Entra no universo a pergunta **materializada** cuja coluna `validada_em` está preenchida, mas cuja validação vigente foi desfeita por ato posterior — por exemplo, uma validação seguida de devolução. Sai a materializada com coluna nula e validação vigente na sequência. A face também considera o passivo da projeção sem linha correspondente; portanto, coluna preenchida sem ato não entra automaticamente. As demais condições do comando continuam aplicáveis. ([models.py:3539](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3539), [models.py:3551](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3551), [encerrar_perguntas_com_fato.py:31](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:31))

Com `--apply`, entre as elegíveis, o comando exige via sem história e marco aceso; chama a porta que troca `via_resolucao`, registra evento e pode fechar a disputa pela porta com suas guardas. A troca de via demonstrada não planta batida nem altera diretamente resposta ou validação. ([encerrar_perguntas_com_fato.py:41](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:41), [encerrar_perguntas_com_fato.py:66](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:66), [encerrar_perguntas_com_fato.py:81](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:81), [models.py:3489](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3489))

Quantidade de perguntas que entram ou saem em produção e efeito final sobre dinheiro, folha, retenção ou competência entregue: **[não verificado]**. O teste novo cobre o universo em *dry-run*, não o resultado de `--apply`. ([test_e4c_f9_comandos_leem_o_ato.py:96](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:96)) [nao verificado]

3. **Selo e mutante — BLOQUEIA**

O selo tem allowlist por arquivo, função, campo e ancestral `add('I18'/'I20')`; ele detecta atributos e palavras-chave ORM, mas não detecta, por exemplo, nomes de campo passados como strings a `values()` ou `values_list()`. Também permite ocorrências adicionais do campo dentro do mesmo `add` permitido. Assim, não garante a proibição geral de leituras cruas pedida no brief. ([test_e4c_f9_comandos_leem_o_ato.py:123](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:123), [test_e4c_f9_comandos_leem_o_ato.py:145](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:145))

**O mutante literal de M2 morreria:** `validada_em__isnull=True` é uma keyword capturada pelo selo e rejeitada fora de I18; o teste funcional de M2 também espera incluir a pergunta divergente que esse filtro excluiria. ([test_e4c_f9_comandos_leem_o_ato.py:51](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:51), [test_e4c_f9_comandos_leem_o_ato.py:70](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:70), [test_e4c_f9_comandos_leem_o_ato.py:133](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:133)) [nao verificado]

O anexo relata RED `rc=1`, GREEN `rc=0` e falha para cada mutante M1–M7; sem os logs completos, a causa individual de cada falha é **[não verificado]**. Para M2–M5, o teste extrai e executa isoladamente o predicado de validação; M7 simula o classificador. ([test_e4c_f9_comandos_leem_o_ato.py:23](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:23), [test_e4c_f9_comandos_leem_o_ato.py:49](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:49), [test_e4c_f9_comandos_leem_o_ato.py:112](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:112); vereditos fornecidos [não verificado como arquivo]) [nao verificado]

4. **Classes de reprovação**

- **Segundo juiz — OK:** as trocas chamam faces existentes; o juiz de resposta registrado continua sendo `pergunta_respondida`. ([models.py:3539](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3539), [core/juizes.py:306](/home/ronald/wt-codex-e4c-f9-191111/app/core/juizes.py:306))
- **Segundo escritor — OK no diff:** M6 continua chamando a porta existente e a sequência tem `registrar` como porta de escrita. ([encerrar_perguntas_com_fato.py:69](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:69), [models.py:4329](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4329))
- **Relógio — OK:** o teste novo usa datas fixas e congela `AGORA`; não contém chamada a `timezone.now()`. ([test_e4c_f9_comandos_leem_o_ato.py:17](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:17), [test_e4c_f9_comandos_leem_o_ato.py:59](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:59), [cortes_f2_f9_apoio.py:30](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/cortes_f2_f9_apoio.py:30)) [nao verificado]
- **Derivação paralela — BLOQUEIA em M7**, pela divergência concreta entre impressão e classificação descrita no item 5; não foi criada uma função nova de juiz. ([validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [juizes.py:2034](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:2034))
- **Selo/RED — BLOQUEIA pelo alcance do selo:** o mutante literal morde, mas a cobertura estrutural não é completa, conforme item 3. ([test_e4c_f9_comandos_leem_o_ato.py:127](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:127))
- **Ordem de trava — OK quanto ao diff:** não há alteração da ordem de travas nas portas existentes; segurança concorrente de M6 sob `--apply` **[não verificado]**. ([encerrar_perguntas_com_fato.py:66](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:66))
- **Semântica — FILA:** os nomes representam os mesmos conceitos de validação e resposta vigentes, mas não necessariamente os mesmos valores quando projeção e atos divergem. DEVOLUÇÃO limpa validação e conserva resposta; REABERTURA limpa ambas. A leitura ainda incorpora o passivo projetado sem linha. ([models.py:4272](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4272), [models.py:4295](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4295), [models.py:4317](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4317))

5. **Quem escreve, quem lê e discordância — BLOQUEIA**

A validação humana escreve `validada_em` e registra VALIDACAO; devolução limpa a coluna e registra DEVOLUCAO. A resposta humana escreve `resposta_colab`, pode limpar validação e registra RESPOSTA; a reabertura limpa o par e registra o ato. Há outros escritores da projeção, inclusive `carimbar`; `registrar` grava as linhas da sequência. ([validacao.py:168](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/validacao.py:168), [validacao.py:273](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/validacao.py:273), [disputa_emissao.py:1481](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1617](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/disputa_emissao.py:1617), [models.py:3299](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3299), [models.py:4329](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4329))

Os M passam a ler linhas por `leitura` ou pela sua face SQL: ambas consideram VALIDACAO, atos que a limpam e o passivo projetado; `leitura` também processa RESPOSTA e os atos que a limpam. O juiz registrado de respostas sem veredito já lê `leitura` e aplica `pergunta_respondida`. ([models.py:3542](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3542), [models.py:4308](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4308), [models.py:2003](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:2003), [core/juizes.py:308](/home/ronald/wt-codex-e4c-f9-191111/app/core/juizes.py:308))

**Discordância concreta:** em M7, `respostas_sem_veredito()` seleciona pela resposta vigente do ato, a impressão mostra essa resposta, mas `classificar_resposta_validacao(q)` lê `q.resposta_colab` da projeção. Para uma pergunta de escolha com texto do ato divergente do texto projetado, a classe pode corresponder a uma resposta enquanto a linha impressa mostra outra; com `--apply`, a classe alimenta a chamada de validação. O teste mascara o classificador, portanto não cobre essa composição. ([validar_classe_a.py:54](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:54), [validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [validar_classe_a.py:97](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:97), [juizes.py:2034](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:2034), [test_e4c_f9_comandos_leem_o_ato.py:112](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:112)) [nao verificado]

**FILA:** M2 e M3 também combinam a validação pelo ato com faces SQL de resposta que leem a projeção; em divergência de resposta, isso pode produzir universo diferente de um leitor que passe `LeituraDoAto` ao juiz. A migração dessas faces não foi autorizada nesta fatia. ([auditar_invariantes_chamados.py:1586](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1586), [auditar_invariantes_chamados.py:1617](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1617), [juizes.py:1575](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:1575), [models.py:2003](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:2003))

6. **LEI-AKITA — FILA:** origem, testemunhas, teste e zero juiz novo cabem na forma exigida; `quem-mais-le=<censo>` fica incompleto sem entregar o censo efetivo e incluir a discordância acima. O padrão existe no glossário e no plano como CQRS/projeção reconstruível; isso não demonstra, por si só, que esta obra implementou um mecanismo novo de reconstrução. ([CLAUDE.md:62](/home/ronald/wt-codex-e4c-f9-191111/CLAUDE.md:62), [GLOSSARIO.md:24](/home/ronald/wt-codex-e4c-f9-191111/app/docs/GLOSSARIO.md:24), [PLANO-PADROES.md:33](/home/ronald/wt-codex-e4c-f9-191111/app/docs/PLANO-PADROES.md:33)) [nao verificado]

**Fecho: BLOQUEIA procedentes = 3; FILA = 2.** Os bloqueios são M7, alcance do selo e comentários FICA incompatíveis com o escopo de arquivos; as filas são a composição das faces SQL de resposta e o censo completo da linha LEI-AKITA. ([validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [test_e4c_f9_comandos_leem_o_ato.py:123](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:123), [backfill_decisoes_pergunta.py:56](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [juizes.py:1575](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:1575), [CLAUDE.md:62](/home/ronald/wt-codex-e4c-f9-191111/CLAUDE.md:62))

_7158 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
