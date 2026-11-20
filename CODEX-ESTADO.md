# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 19:21** · publicado em **20/11/2026 19:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 19:21 · sessao 282f814c9fa2 · E4C-F5-CONSTROI · sonda · origem=Code

```python
# Uso na sombra com bin/sombra.sh --rodar sonda.py.
# SOMBRA_ARVORE seleciona o app da outra copia.
# bin/sombra.sh:179, bin/sombra.sh:182, bin/sombra.sh:483
#
# O --rodar chama lavra_de_prod antes do shell Python. Essa etapa le a
# producao, grava arquivos auxiliares e atualiza a sombra fora do rollback
# desta sonda. O lancador tambem grava a saida com tee.
# bin/sombra.sh:136, bin/sombra.sh:151, bin/sombra.sh:157, bin/sombra.sh:483
#
# Nomes do anexo ausentes no main, resolvidos individualmente por getattr.
# - PerguntaDisputa.RECUSA_FORA_DO_FATO. O main tem VIAS_FORA_DO_FATO e
#   carimbar devolve False nesse caso. A sonda registra a falta do nome.
#   app/chamados/models.py:2701, app/chamados/models.py:3316
# - PerguntaDisputa.RECUSA_C6. O main devolve False no ramo do C6.
#   A sonda registra a falta do nome. app/chamados/models.py:3334
# - PerguntaDisputa.RECUSA_PAR_ATO_COLUNA. O main grava o ato da irma
#   pelo ramo de sucesso e emite a recusa sem motivo. A sonda registra
#   a falta do nome. app/chamados/services/materializacao.py:1055
#
# A trilha do main traz escape_pk, ausencia_pk, via_tentada e data_evento.
# Quando falta motivo, a sonda guarda os PKs observados e marca a
# classificacao INCONCLUSIVO. A rejeicao segue o resultado da passagem.
# app/chamados/services/materializacao.py:1065
#
# A prova foto 3 == foto 1 usa hash integral e numero de linhas.
# A saida mostra 12 caracteres do hash. O delta usa multiconjuntos
# por matricula e contabiliza duplicatas. A ordem fica refletida no hash.
# O registro 11 pertence ao ultimo 10, cuja matricula ocupa [2:12].
# app/folha/export.py:34, app/folha/export.py:215, app/folha/export.py:246,
# app/folha/export.py:525, app/folha/export.py:654
#
# Rejeicao pela porta real, seguida apenas do ramo de devolucao que o
# anexo executava dentro do rollback. app/ponto/services/ausencia.py:1057,
# app/ponto/signals.py:254, app/ponto/signals.py:272
#
# Execucao no banco da sombra e compatibilidade de runtime com c8eb8eddf
# [nao verificado].

import hashlib
import io
import json
import logging
import socket
import sys
from collections import Counter, defaultdict
from contextlib import ExitStack, closing, redirect_stderr, redirect_stdout
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.storage import FileSystemStorage
from django.db import DatabaseError, connection, transaction
from django.db.models import F, Q, TextField
from django.db.models.functions import Cast
from django.urls import reverse
from django.utils import timezone
from rest_framework.test import APIRequestFactory, force_authenticate

from api.views import api_disputa_responder
from chamados.models import PerguntaDisputa
from chamados.services import disputa_emissao, materializacao as M
from colaboradores.mixins import tem_acao
from core import observ
from folha import export as E
from folha.models import ExportacaoDominio
from ponto.models import Ausencia, Batida, FechamentoMensal
from ponto.services import ausencia as A


TETO = 8
FOTOS = ("antes", "depois_passagem", "depois_rejeicao")
NOMES_DE_RECUSA = ("RECUSA_FORA_DO_FATO", "RECUSA_C6", "RECUSA_PAR_ATO_COLUNA")
REVERSOES = {"par_ato_coluna_divergente", "sem_grupo_recuperavel"}
MOTIVOS_COMUNS = REVERSOES | {
    "fora_da_releitura", "resposta_posterior_a_passagem", "ja_carimbada",
}

# app/api/views.py:2603, app/api/views.py:2631,
# app/chamados/services/disputa_emissao.py:1439.
# A mensagem de absorcao vigente vem do anexo para c8eb8eddf [nao verificado].
RECUSAS = {
    (404, "Colaborador nao encontrado."): "sem_vinculo",
    (404, "Pergunta nao encontrada ou ja regularizada."): "nao_encontrada_ou_nao_viva",
    (400, "Resposta obrigatoria."): "resposta_vazia",
    (400, "Selecione/preencha uma resposta."): "resposta_vazia",
    (400, "Opcao invalida."): "opcao_invalida",
    (400, "Hora deve ser HH:MM."): "hora_invalida",
    (400, "A pergunta já foi resolvida pela ausência declarada."): "absorcao_vigente",
}


class Descartar(io.TextIOBase):
    def write(self, texto):
        return len(texto)


class GuardaSombra(RuntimeError):
    pass


class GuardaDeSequencias(RuntimeError):
    pass


class SemUsuarioAtivo(RuntimeError):
    pass


class SemDecisorDP(RuntimeError):
    pass


class EvidenciaInesperada(RuntimeError):
    pass


class LayoutDeExportInesperado(RuntimeError):
    pass


class RedeBloqueada(OSError):
    pass


class EscritaEmArquivoBloqueada(OSError):
    pass


def resolver_recusas(ausentes):
    motivos = set(MOTIVOS_COMUNS)
    for nome in NOMES_DE_RECUSA:
        valor = getattr(PerguntaDisputa, nome, None)
        if valor is None:
            ausentes.add(f"PerguntaDisputa.{nome}")
        else:
            motivos.add(valor)
    return motivos


def guardar_sequencias(cursor):
    # A sombra usa PostgreSQL 17. As portas podem inserir linhas com PK
    # gerado. A sonda usa RESTART dentro do atomic externo para isolar
    # os avanços de sequencia, preservando o proximo valor inicial.
    # bin/sombra_db.sh:45, app/config/settings/base.py:104,
    # app/ponto/services/ausencia.py:560, app/chamados/models.py:4338.
    # Permissao e comportamento das sequencias instaladas [nao verificado].
    try:
        cursor.execute("""
            DO $sonda$
            DECLARE
                s record;
                qualificada text;
                ultimo bigint;
                usada boolean;
                proximo numeric;
            BEGIN
                FOR s IN
                    SELECT n.nspname, c.relname, p.seqcache, p.seqincrement,
                           p.seqmin, p.seqmax, p.seqcycle
                    FROM pg_catalog.pg_sequence p
                    JOIN pg_catalog.pg_class c ON c.oid = p.seqrelid
                    JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
                    WHERE n.nspname IN ('juliani', 'public')
                    ORDER BY n.nspname, c.relname
                LOOP
                    qualificada := format('%I.%I', s.nspname, s.relname);
                    EXECUTE format('ALTER SEQUENCE %s CACHE %s', qualificada, s.seqcache);
                    EXECUTE 'SELECT last_value, is_called FROM ' || qualificada
                        INTO ultimo, usada;
                    proximo := ultimo::numeric;
                    IF usada THEN
                        proximo := proximo + s.seqincrement;
                    END IF;
                    IF proximo NOT BETWEEN s.seqmin AND s.seqmax THEN
                        IF NOT s.seqcycle THEN
                            CONTINUE;
                        END IF;
                        proximo := CASE WHEN s.seqincrement > 0
                                       THEN s.seqmin ELSE s.seqmax END;
                    END IF;
                    EXECUTE format('ALTER SEQUENCE %s RESTART WITH %s', qualificada, proximo);
                END LOOP;
            END;
            $sonda$;
        """)
    except DatabaseError as erro:
        raise GuardaDeSequencias() from erro


def sha(valor):
    texto = json.dumps(valor, sort_keys=True, ensure_ascii=True, separators=(",", ":"))
    return hashlib.sha256(texto.encode("ascii")).hexdigest()


def assinatura(campos):
    por_campo = {nome: sha(valor) for nome, valor in campos.items()}
    return sha(por_campo), por_campo


def fotografar_linhas(qs):
    campos = [f for f in qs.model._meta.concrete_fields if not f.primary_key]
    nomes = [f.name for f in campos]
    colunas = [Cast(F(f.attname), TextField()) for f in campos]
    return {
        pk: assinatura(dict(zip(nomes, valores)))
        for pk, *valores in qs.order_by("pk").values_list("pk", *colunas)
    }


def foto(colab, dia, mes, ano, montar):
    empresa = colab.empresa
    vigente = E.competencia_entregue(empresa.pk, mes, ano, registro=True)
    montagens = []

    def montar_observada(*args, **kwargs):
        resultado = montar(*args, **kwargs)
        linhas, _ = resultado
        montagens.append(tuple(linhas) if isinstance(linhas, (list, tuple)) else None)
        return resultado

    # Captura as linhas da propria chamada que alimenta o comparador.
    # app/folha/export.py:631, app/folha/export.py:654, app/folha/export.py:690
    if montar is None:
        txt = E.txt_vigente_x_gravado(empresa, mes, ano)
    else:
        with patch.object(E, "montar_linhas_export", montar_observada):
            txt = E.txt_vigente_x_gravado(empresa, mes, ano)

    par_txt = {chave: txt[chave] for chave in ("hash_hoje", "linhas_hoje")}
    linhas = montagens[0] if len(montagens) == 1 else None
    if linhas is not None and len(linhas) != par_txt["linhas_hoje"]:
        linhas = None

    criterio_dia = (
        Q(data=dia)
        | Q(data__lte=dia, data_fim__gte=dia)
        | Q(data__lte=dia, data_fim__isnull=True, tipo__in=Ausencia.TIPOS_EM_ABERTO)
    )
    fechamentos = FechamentoMensal.objects.filter(
        colaborador_id=colab.pk, mes=mes, ano=ano,
    )
    modelos = {
        "FechamentoMensal": fotografar_linhas(fechamentos),
        "Batida": fotografar_linhas(Batida.objects.filter(
            colaborador_id=colab.pk, timestamp__date=dia,
        )),
        "Ausencia": fotografar_linhas(Ausencia.objects.filter(
            criterio_dia, colaborador_id=colab.pk,
        )),
        "ExportacaoDominio": fotografar_linhas(ExportacaoDominio.objects.filter(
            pk=vigente.pk if vigente is not None else None,
        )),
        "TXT_do_gravado": {empresa.pk: assinatura(par_txt)},
    }
    return {
        "modelos": modelos,
        "par_txt": par_txt,
        "linhas": linhas,
        "publica": {
            "hash_hoje": par_txt["hash_hoje"][:12],
            "linhas_hoje": par_txt["linhas_hoje"],
            "divergente": txt["divergente"],
            # app/ponto/models.py:398, app/ponto/models.py:481
            "fechamento_mensal_status": fechamentos.order_by("pk").values_list(
                "status", flat=True,
            ).first(),
        },
    }


def diferencas(antes, depois):
    resultado = {}
    for modelo, a in antes["modelos"].items():
        b = depois["modelos"][modelo]
        novas = sorted(b.keys() - a.keys())
        removidas = sorted(a.keys() - b.keys())
        mudadas = [
            {"pk": pk, "campos": sorted(
                nome for nome in a[pk][1] if a[pk][1][nome] != b[pk][1][nome]
            )}
            for pk in sorted(a.keys() & b.keys()) if a[pk][0] != b[pk][0]
        ]
        resultado[modelo] = {
            "novas": {"n": len(novas), "pks": novas},
            "mudadas": {"n": len(mudadas), "linhas": mudadas},
            "removidas": {"n": len(removidas), "pks": removidas},
        }
    return resultado


def linhas_por_matricula(linhas):
    contagem = Counter()
    matricula = None
    for linha in linhas:
        registro = linha[:2]
        if registro == "10" and len(linha) == 43:
            matricula = linha[2:12]
        elif registro != "11" or len(linha) != 11 or matricula is None:
            raise LayoutDeExportInesperado()
        contagem[(registro, matricula, linha)] += 1
    return contagem


def diferencas_txt(antes, depois, matricula, status_etapa, montar):
    if status_etapa == "NAO_EXERCITADA":
        return {"status": "NAO_EXERCITADA"}
    if montar is None:
        return {"status": "INCONCLUSIVO", "motivo": "folha.export.montar_linhas_export_ausente"}
    if antes is None or depois is None:
        return {"status": "INCONCLUSIVO", "motivo": "FotoIndisponivel"}
    if antes["linhas"] is None or depois["linhas"] is None:
        return {"status": "INCONCLUSIVO", "motivo": "MontagemDoComparadorIndisponivel"}

    try:
        a = linhas_por_matricula(antes["linhas"])
        b = linhas_por_matricula(depois["linhas"])
    except Exception as erro:
        return {"status": "INCONCLUSIVO", "motivo": type(erro).__name__}

    por_tipo = {reg: {"adicionadas": 0, "removidas": 0} for reg in ("10", "11")}
    afetadas = set()
    for sentido, mudancas in (("adicionadas", b - a), ("removidas", a - b)):
        for (registro, mat, _), n in mudancas.items():
            por_tipo[registro][sentido] += n
            afetadas.add(mat)

    return {
        "status": "CONCLUSIVO",
        "por_tipo": por_tipo,
        "matriculas_distintas_afetadas": len(afetadas),
        "matricula_do_caso_afetada": matricula in afetadas,
    }


def censo(escapes):
    # Mesmo teto, filtros e ordem do anexo. O leitor da competencia esta em
    # app/chamados/services/materializacao.py:788 e os escapes em :1095.
    por_escape = dict.fromkeys(escapes, 0)
    selecionadas = {escape: [] for escape in escapes}
    universo = sem_dia = 0
    memo = {}
    qs = (
        PerguntaDisputa.objects.filter(tipo_resposta__in=("hora", "escolha"))
        .select_related("disputa__colaborador__empresa")
        .only("tipo_resposta", "opcoes", "data_evento",
              "disputa__colaborador__empresa__dia_inicio_competencia")
        .order_by(F("materializada_em").asc(nulls_first=True), "-data_evento", "pk")
    )
    with closing(qs.iterator(chunk_size=2000)) as perguntas:
        for p in perguntas:
            opcoes = p.opcoes or []
            valores = [o.get("valor") if isinstance(o, dict) else o for o in opcoes]
            oferecidos = [e for e in escapes if e in valores]
            if not oferecidos:
                continue
            empresa = p.disputa.colaborador.empresa
            chave = (empresa.pk, p.data_evento)
            if chave not in memo:
                memo[chave] = M.competencia_do_fato(p, empresa)
            comp = memo[chave]
            if comp is None:
                sem_dia += 1
                continue
            mes, ano, entregue = comp
            if not entregue:
                continue
            universo += 1
            for escape in oferecidos:
                por_escape[escape] += 1
                if len(selecionadas[escape]) < TETO:
                    selecionadas[escape].append((p.pk, escape, mes, ano))
    casos = [caso for escape in escapes for caso in selecionadas[escape]]
    return universo, sem_dia, por_escape, casos


def tipos_de_falha(falhas):
    contagem = Counter(x for x in falhas if isinstance(x, str))
    excecoes = {id(x): x for x in falhas if isinstance(x, BaseException)}
    contagem.update(type(x).__name__ for x in excecoes.values())
    return dict(contagem)


def medir_fatia(r, nome, acao, falhas):
    inicio = len(falhas)
    try:
        dados = acao()
        if transaction.get_rollback():
            falhas.append("TransacaoMarcadaParaRollback")
    except Exception as erro:
        falhas.append(erro)
    erros = tipos_de_falha(falhas[inicio:])
    del falhas[inicio:]
    r[nome] = (
        {"status": "INCONCLUSIVO", "excecoes_por_tipo": erros}
        if erros else {"status": "CONCLUSIVO", **dados}
    )


def decisor_dp():
    usuarios = get_user_model().objects.filter(is_active=True).order_by("-is_superuser", "pk")
    with closing(usuarios.iterator(chunk_size=200)) as ativos:
        for usuario in ativos:
            if tem_acao(usuario, "editar_ausencias"):
                return usuario
    raise SemDecisorDP()


def medir_caso(caso, falhas, motivos_irmas, montar, ausentes):
    pk, escape, mes, ano = caso
    r = {
        "registro": "caso", "pergunta_pk": pk, "escape": escape,
        "tentou_porta": 0, "tentou_rejeicao": 0, "ausencia": None,
        "passagem": {"status": "NAO_EXERCITADA"},
        "rejeicao": {"status": "NAO_EXERCITADA"},
    }
    fotos = {}
    ausencias = []
    recusas_irmas = defaultdict(set)
    criar_real, evento_real = A.criar_ausencia, observ.evento
    colab = dia = matricula = None

    def criar_observada(**kw):
        # app/chamados/services/materializacao.py:994,
        # app/chamados/services/materializacao.py:1011
        chamada_da_passagem = (
            sys._getframe(1).f_code is M._materializar_ausencia_de_resposta.__code__
        )
        ausencia = criar_real(**kw)
        if chamada_da_passagem:
            ausencias.append(getattr(ausencia, "pk", None))
        return ausencia

    def evento_observado(acao, modelo, objeto_id, **kw):
        if acao == M.ACAO_IRMA_VIVA_NA_AUSENCIA and modelo == "PerguntaDisputa":
            dados = kw.get("depois") or {}
            if dados.get("escape_pk") == pk:
                motivo = dados.get("motivo")
                if "motivo" not in dados:
                    ausentes.add("ACAO_IRMA_VIVA_NA_AUSENCIA.depois.motivo")
                    recusas_irmas["motivo_indisponivel"].add(objeto_id)
                elif isinstance(motivo, str) and motivo in motivos_irmas:
                    recusas_irmas[motivo].add(objeto_id)
                    if motivo in REVERSOES:
                        falhas.append("_Reprovada")
                else:
                    falhas.append("MotivoDeRecusaInesperado")
        return evento_real(acao, modelo, objeto_id, **kw)

    def passagem():
        nonlocal colab, dia, matricula
        p = PerguntaDisputa.objects.select_related(
            "disputa__colaborador__empresa", "disputa__colaborador__usuario",
        ).get(pk=pk)
        colab, dia = p.disputa.colaborador, p.data_evento
        usuario = colab.usuario
        if usuario is None or not usuario.is_active:
            raise SemUsuarioAtivo()
        r.update(colaborador_pk=colab.pk, empresa_pk=colab.empresa_id)
        # Mesma normalizacao da montagem. app/folha/export.py:728
        codigo = (colab.codigo_dominio or "").strip()
        matricula = codigo.zfill(10) if codigo else None
        fotos["antes"] = foto(colab, dia, mes, ano, montar)
        req = APIRequestFactory().post(
            reverse("api:disputa_responder", kwargs={"pk": pk}),
            {"resposta": escape},
            format="json",
        )
        force_authenticate(req, user=usuario)
        with patch.object(A, "criar_ausencia", criar_observada), \
                patch.object(observ, "evento", evento_observado):
            r["tentou_porta"] = 1
            resposta = api_disputa_responder(req, pk=pk)

        if resposta.status_code == 200 and resposta.data.get("status") == "ok":
            r["porta"] = "ACEITA"
            if len(ausencias) != 1 or ausencias[0] is None:
                raise EvidenciaInesperada()
            r["ausencia_pk"] = ausencias[0]
        else:
            motivo = RECUSAS.get((resposta.status_code, resposta.data.get("erro")))
            if motivo is None:
                raise EvidenciaInesperada()
            r.update(porta="RECUSADA", motivo_recusa=motivo)

        fotos["depois_passagem"] = foto(colab, dia, mes, ano, montar)
        if "ausencia_pk" in r:
            # A porta pode criar, devolver ou reabrir uma linha.
            # app/ponto/services/ausencia.py:392, app/ponto/services/ausencia.py:436,
            # app/ponto/services/ausencia.py:560, app/ponto/models.py:749,
            # app/ponto/models.py:809
            ausencia = Ausencia.objects.values("tipo", "status").get(pk=r["ausencia_pk"])
            r["ausencia"] = {
                "tipo": ausencia["tipo"],
                "status_apos_passagem": ausencia["status"],
                "status_apos_rejeicao": None,
                "nova_na_foto": r["ausencia_pk"] not in fotos["antes"]["modelos"]["Ausencia"],
            }
        return {"diferencas": diferencas(fotos["antes"], fotos["depois_passagem"])}

    def rejeicao():
        ausencia = Ausencia.objects.get(pk=r["ausencia_pk"])
        dp = decisor_dp()
        r["tentou_rejeicao"] = 1
        A.decidir(ausencia, "rejeitada", por=dp, decisor=dp, aprovada_em=timezone.now())
        ausencia.refresh_from_db()
        r["ausencia"]["status_apos_rejeicao"] = ausencia.status
        if ausencia.status != "rejeitada":
            raise EvidenciaInesperada()
        devolvidas = M.devolver_absorvidas_da_ausencia_rejeitada(ausencia)
        fotos["depois_rejeicao"] = foto(colab, dia, mes, ano

[truncado em 20000 de 28189 caracteres]

## 20/11 19:13 · sessao a29af3c9c491 · E4C-F5-CONSTROI · leitura · origem=Code

## E4C-F5-CONSTROI

O anexo registra mudança **nos dois campos** `hash_hoje` e `linhas_hoje`, na passagem e na rejeição, para `FALTA_DIA` nas perguntas `33668`, `33669` e `33686`. Nas outras cinco `FALTA_DIA` e nas oito `ATESTADO`, o par ficou igual. Nos 16 casos nasceu uma `Ausencia` e as fotos recortadas de `FechamentoMensal`, `Batida` e `ExportacaoDominio` ficaram iguais. O resumo traz os nomes dos campos mudados. A direção do delta, as rubricas afetadas, a volta ao hash inicial e o valor de `divergente` em cada foto ficam **[nao verificado]**. A identidade Git da cópia com o SHA informado também fica **[nao verificado]**. `medida_6a1_resumo.txt:1–18` · `sonda_6a1.py.txt:104–148`

## 1. Fontes de `montar_linhas_export`

| Fonte | Campos e efeito |
|---|---|
| **Gravado — `FechamentoMensal`** | Seleciona por `colaborador_id` ligado à empresa, `mes`, `ano` e `status` em `aberto`/`aprovado`. Os eventos leem `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_folga_trabalhada`, `horas_noturnas`, `horas_intra_indenizada`, `horas_atraso` e `horas_saida_antecipada`. A HE100 normal é o total menos a parcela de feriado. `horas_trabalhadas` também decide `cadastro_zero`. `horas_trabalhadas` e `turnos_abertos` alimentam `travas_aprovacao`. `turnos_abertos` e `inconsistencias` são passados ao retentor do espelho, que ignora esses argumentos e consulta as células. `app/folha/export.py:399–452`, `:687–739`, `:830–848`, `:902–968`, `:1301–1307` |
| **Viva — `Colaborador` e `Empresa`** | `empresa_id` define a população. `codigo_dominio` decide presença de matrícula e fornece a matrícula da linha. `nome` ordena os fechamentos. `data_demissao` decide a saída por rescisão e, junto de `data_admissao`, recorta a auditoria das células. `situacao` e `isento_ponto` influem na retenção. A isenção também suprime os eventos de horas, enquanto o bloco de falta tem sua própria condição. `Empresa.dia_inicio_competencia` define a janela, e `regime_trabalhista` seleciona a régua. `app/folha/export.py:54–60`, `:417–419`, `:718–744`, `:837–875`, `:1205–1213`, `:1238–1247` · `app/ponto/models.py:481–485` · `app/ponto/janelas.py:36–57` · `app/core/regua_cct.py:352–374` · `app/core/constants.py:13–18` · `app/colaboradores/services/desligamento.py:23–32` |
| **Viva — `EmpresaIntegracao`, `RubricaDominio`, `CatalogoDominio`** | Integração por `empresa`, `sistema='dominio'` e `ativo=True`, fornecendo `codigo_no_sistema`. Rubricas ativas por `evento`, com a da empresa prevalecendo sobre a global, fornecem `codigo_fopag` e `tipo`. O catálogo da empresa fornece os `codigo` homologados. Catálogo vazio desliga a retenção por homologação. `app/folha/export.py:23–45`, `:258–260`, `:730–744`, `:984–1019` |
| **Viva — `Ausencia` e `TipoAusencia`** | `Ausencia.colaborador_id`, `tipo`, `status`, `data` e `data_fim` decidem quais datas de falta entram. O catálogo usa `TipoAusencia.ativo`, `codigo`, `efeito` e `fim` para selecionar tipos que descontam e tratar intervalos em aberto. A leitura usa cache de processo de 60 s e o catálogo em código como fallback. O bloco usa a contagem das datas e emite um registro `11` por ocorrência de data, em ordem. `Ausencia.minutos` e `FechamentoMensal.horas_falta` ficam fora dessa montagem. `datas_abonadas` existe no módulo, mas fica fora do caminho de `montar_linhas_export`. `app/folha/export.py:63–92`, `:146–221`, `:725–746` · `app/ponto/turnos.py:2802–2818` · `app/ponto/catalogo/ausencias.py:198–205`, `:240–281`, `:331–333`, `:458` |
| **Viva — `CelulaDia`, `EscalaColaborador`, `TipoEscala`** | Para `furo_espelho`, lê a célula por `colaborador_id` e `data`, seu `veredito`, `protestos`, `trabalha` e, na exceção de furo só de pausa, `ata.lampadas` e `dna.marcos`. Para saber se um dia sem célula deveria ter uma, lê os vínculos e `ativa`, `data_inicio`, `data_fim`. Para o ciclo no fim da janela e para o previsto de `cadastro_zero`, lê `CelulaDia.escala_geradora_id`, o vínculo e seu `tipo_escala`; o previsto usa `trabalha`, marcos e pausas do DNA ou, quando aplicável, ciclo, dias, horários e pausas do template. A consulta de âncora nula em vínculo ativo `12x36` alimenta `travas_aprovacao`. `app/folha/export.py:357–380`, `:805–836`, `:936–949`, `:1148–1173`, `:1247–1298` · `app/ponto/juiz_batida.py:149–176` · `app/escala/alimentacao.py:148–159`, `:205–216`, `:269–286`, `:316–331` · `app/escala/utils.py:2267–2302` · `app/escala/models.py:281–345`, `:816–867`, `:1087–1133`, `:1182–1208` |
| **Viva — `FolgaDia`, `Feriado`, `Posto`, `Municipio`** | No previsto sem célula, usa `FolgaDia.escala_colaborador_id`/`data`, a âncora do vínculo e `trabalha_em_feriado`. A consulta de feriado usa `data`, `abrangencia`, `uf` e `municipio`, com o município do posto do vínculo e seus `uf`/`nome`. `app/escala/models.py:1271–1319`, `:1356–1395` · `app/core/feriados.py:27–29`, `:40–83`, `:124–132` |
| **Viva — `AgendamentoFerias`, `PeriodoAquisitivo`, `Batida`** | `data_inicio`, `data_fim`, `status` e `periodo_aquisitivo.colaborador_id` definem férias na janela, excluindo canceladas. Cruza com `Batida.colaborador_id`, `timestamp` e `retratada_em IS NULL`. A mesma leitura de batidas apuráveis serve a `cadastro_zero`, que exige pelo menos duas batidas, zero horas trabalhadas gravadas e nenhum dia com previsto. `app/folha/export.py:482–505`, `:902–967` · `app/ferias/services.py:49–56` · `app/ponto/turnos.py:2891–2899` |
| **Viva — `AplicacaoConvencao` e `Sindicato`** | O vínculo fornece `posto.praca` e `tipo_escala.tipo_ciclo`. A régua consulta aplicação por `empresa`, `ativo`, `praca` e `sindicato`, com `cct_vigencia_inicio`/`cct_vigencia_fim` no fim da janela. A montagem consome `feriado_12x36_em_dobra` para decidir se emite a parcela de feriado no `12x36`. `app/folha/export.py:357–380`, `:420–443` · `app/core/regua_cct.py:251–275`, `:328–380` |

`ExportacaoDominio` entra **no comparador**. A vigência usa empresa/mês/ano, maior `(gerado_em, pk)` e `invalidada_em`. O comparador usa `hash_sha256` e `total_linhas` desse registro para o lado vigente. O lado `hoje` é a montagem da empresa inteira, serializada em CRLF com terminador final quando há linhas, e devolve SHA-256 e contagem. `app/folha/models.py:203–225` · `app/folha/export.py:539–553`, `:580–594`, `:655–679`

## 2. Caminhos que mudam o par com o fechamento fotografado igual

1. **Emissão direta do bloco de falta.** Uma ausência `aprovada`, de tipo que o catálogo diz que `desconta`, acrescenta as datas em que intercepta a janela. Se o colaborador continua elegível e há mapeamento `falta_diurna`, o primeiro bloco acrescenta um registro `10` e os `11` das datas. Se o bloco já existia, muda sua contagem e entram os `11` adicionais. Ao rejeitar, a linha deixa a consulta de aprovadas. A consulta acumula ocorrências de datas, inclusive quando duas ausências qualificadas cobrem o mesmo dia. `app/folha/export.py:86–92`, `:159–185`, `:215–221`, `:725–745`
2. **Homologação muda a entrada do colaborador.** A mesma falta viva ativa a checagem da rubrica `falta_diurna`. Se o catálogo de homologação tem códigos e o `codigo_fopag` dessa rubrica está fora dele, `classificar_export` pode passar a devolver `fora/nao_homologado`. Com isso, podem sumir **todas** as linhas do colaborador. Rejeitar pode liberar a entrada quando esse era o último motivo de retenção. `app/folha/export.py:714–727`, `:968–979`, `:994–1019`
3. **Rejulgamento da célula muda `furo_espelho`.** Cada `save` de `Ausencia` chama o cartório de forma síncrona. A cobertura aprovada pode transformar um furo em dia decidido ou mudar as lâmpadas da ata. Também pode surgir uma acusação, como `FATO_EM_AUSENCIA`, conforme o fato no dia. A cobertura para cobrança inclui ausências abertas e pode mudar a impressão e o julgamento, inclusive pela regra que verifica `cobertura_status is None`. Há ainda a rota indireta pelo chamado: a passagem pode encerrar destinos ou a disputa, o signal de mudança de status do chamado julga sua célula, e o cartório pode rejulgar após reconciliar chamados ou religar perguntas. Em todos esses ramos, é `veredito`/`protestos`/ata que chegam ao retentor e podem mudar a entrada do bloco inteiro. `app/ponto/signals.py:85–93`, `:115–147` · `app/ponto/services/cartorio.py:818–830`, `:1381–1443`, `:1499–1510`, `:1898–1922`, `:1044–1048` · `app/ponto/supra_juiz.py:317–329`, `:395–418`, `:507–512` · `app/chamados/services/materializacao.py:1105–1114` · `app/chamados/signals.py:19–46`, `:189–218` · `app/folha/export.py:1264–1298`
4. **Depois do commit, há caminhos adicionais fora da medida.** A reconciliação de chamados por ausência pode disparar novo julgamento. Se a disputa fechar e houver regularização externa confirmada por aplicar, outro callback pode criar, corrigir ou remover batidas. Isso pode mudar os filtros vivos de `ferias_com_batida` e `cadastro_zero`, além de disparar o cartório. Uma inclusão pela porta de batida também pode agendar recálculo mensal. A ocorrência desses ramos nos casos do anexo fica **[nao verificado]**. `app/ponto/signals.py:254–272` · `app/chamados/services/silencio_ausencia.py:94–128` · `app/chamados/signals.py:122–146`, `:189–218` · `app/chamados/services/regularizacao_ext.py:71–83`, `:122–189`, `:211–227` · `app/ponto/registro_batida.py:174–198` · `app/folha/export.py:491–504`, `:902–967`

## 3. O que distingue os casos

- **Tipo, estado e cobertura.** Na criação nova, `FALTA_DIA` pede `falta/aprovada`. `ATESTADO` pede `atestado` sem anexo e nasce em `aguardando_documento` ou `aguardando_decisao`, conforme `exige_documento` do catálogo. Assim, o atestado recém-criado fica fora da busca de faltas aprovadas, mas entra na cobertura de cobrança enquanto estiver aberto. Para o cartório, quando várias ausências cobrem o dia, vence maior `data` e depois maior `pk`. Para o bloco de falta, valem todas as ocorrências aprovadas cujo tipo desconta. Os minutos da falta podem ser derivados da jornada no `save`, mas o bloco usa datas. `app/chamados/services/materializacao.py:1027–1035`, `:1125–1131` · `app/ponto/services/ausencia.py:473–491`, `:926–944` · `app/ponto/turnos.py:2523–2532`, `:2550–2571` · `app/ponto/models.py:878–897` · `app/folha/export.py:86–92`, `:159–185`, `:215–221`
- **Entrega, fechamento e elegibilidade.** `ENTREGUE` decorre da exportação vigente da empresa. A emissão exige um `FechamentoMensal` do colaborador em `aberto` ou `aprovado` e classificação `entra`. A classificação testa rescisão, matrícula, férias com batida, `cadastro_zero`, `furo_espelho` e homologação. Um segundo dia que ainda retenha, a falta de rubrica ou um colaborador que continue fora podem deixar o par igual. Para o ramo da célula, importam também existência da célula, relógio do turno, impressão anterior, cobertura, batidas, chamados e perguntas vivas, e as exceções de protesto não cobrável ou só de pausa. `app/folha/export.py:580–594`, `:687–745`, `:866–979`, `:1264–1298` · `app/ponto/services/cartorio.py:1381–1443`, `:2062–2090`
- **O que explica cada PK fica [nao verificado].** O anexo traz empresa, escape e campos alterados. Ficam sem evidência por caso o `status` do fechamento, os vereditos, a elegibilidade, a rubrica resolvida e as linhas que entraram ou saíram. Como `linhas_hoje` mudou nos três casos, uma mera permutação das mesmas linhas é insuficiente para explicar o achado. `medida_6a1_resumo.txt:11–18` · `sonda_6a1.py.txt:125–128` · `app/folha/export.py:658–677`

## 4. Escritores alcançáveis fora da foto

| Etapa | Escritas que o código permite |
|---|---|
| **Passagem pela API** | O core salva resposta e carimbos de `PerguntaDisputa`, registra `DecisaoPergunta`, pode levantar o silêncio do chamado e anexa `FollowupChamado`. O materializador insere `Ausencia` ou, em outros casos, reaproveita/reabre uma existente, carimba irmãs, registra absorções e pode fechar chamados e `DisputaSupervisao`. Fechar o chamado pode disparar S143, relavra de perguntas e rebaixamento de urgência. Fechar a disputa pode disparar o fechamento do pai e S144, que carimba perguntas. Há trilha em `LogAuditoria`. `app/api/views.py:2624–2632`, `:2652–2658` · `app/chamados/services/disputa_emissao.py:1499–1581` · `app/ponto/services/ausencia.py:392–443`, `:559–598` · `app/chamados/services/materializacao.py:1027–1114`, `:1241–1253`, `:2207–2228` · `app/chamados/models.py:304–325`, `:2118–2126`, `:3400`, `:4438` · `app/chamados/signals.py:19–73`, `:91–119`, `:149–165` · `app/chamados/services/nota.py:103–107` · `app/core/observ.py:133–138` |
| **Passagem e rejeição, síncronas** | O cartório pode gravar `CelulaDia` e `EventoCelula`, inclusive veredito, protestos, ata, ausência e carimbos. Também pode criar ou alterar chamados, disputas e perguntas, rejeitar `Justificativa` pendente em fechos específicos e escrever `Pauta`/`ContadorPauta`, por exemplo no limbo de competência entregue ou em recusa encaminhada ao DP. O evento da pergunta também pode inserir `EventoCelula`. Nenhuma dessas tabelas tem foto própria na sonda. `app/ponto/portas/celula.py:945–965`, `:1090–1104`, `:1181–1216` · `app/ponto/services/cartorio.py:1035–1074`, `:1580–1585`, `:1876–1897` · `app/ponto/services/cluster_espurio.py:176–196` · `app/ponto/services/justificativa.py:327–353` · `app/chamados/services/fabrica_da_celula.py:160–174`, `:214–219`, `:274–284` · `app/chamados/services/fecho_runtime.py:130–141` · `app/pautas/services.py:180`, `:344–365` · `app/chamados/signals.py:252–259` · `sonda_6a1.py.txt:104–129` |
| **`FechamentoMensal`** | Tanto o nascimento quanto `decidir` podem chamar `_acordar_projecao` quando muda o insumo aprovado. O escritor zera `derivado_em` e atualiza `atualizado_em` nas competências tocadas **do mesmo colaborador**, onde `derivado_em` estava preenchido. A consulta aceita faixa de competências e independe do status de fechamento e da vigência do TXT. Em fato de um dia, esse escritor alcança a competência desse dia. Em faixa que atravessa competências, alcança linhas fora da foto. O exportador não consome esses dois carimbos. Se a linha fotografada existia e tinha `derivado_em` preenchido, a invalidação bem-sucedida apareceria no diff. Qual condição ocorreu em cada caso fica **[nao verificado]**. `app/ponto/services/ausencia.py:126–151`, `:177–195`, `:594–598`, `:1121–1124` · `app/ponto/services/fechamento.py:1573–1599`, `:1648–1665` · `app/folha/export.py:717–746` · `sonda_6a1.py.txt:92–116`, `:132–148` |
| **Rejeição exercitada** | `decidir` salva status, limpa `prazo_documento` e salva os carimbos do decisor. A chamada manual de devolução só segue se a ausência está rejeitada, o dia ficou sem outra cobertura para cobrança e há perguntas absorvidas recuperáveis. Pode limpar resposta, validação e materialização de `PerguntaDisputa`, inserir `DecisaoPergunta` e `EventoCelula`, reabrir `DisputaSupervisao` e gravar nota e auditoria. `get_user_sistema` pode criar `auth.User` se faltar. `app/ponto/services/ausencia.py:975–998`, `:1084–1124` · `app/chamados/services/materializacao.py:1151–1167`, `:1187–1222` · `app/chamados/services/disputa_emissao.py:1630–1685` · `app/chamados/models.py:2359–2366` · `app/core/utils.py:6–18` · `sonda_6a1.py.txt:293–305` |
| **Após commit ou pela tela** | O callback da ausência também reconcilia chamados e encerra os ligados à decisão, com estado, silêncio, nota e trilha. Há callbacks do beco da disputa e do filho que acorda o pai. O callback de regularização pode escrever `RegularizacaoExterna`, seus itens, `Batida` de outros dias, `TurnoMaterializado` e, quando a inclusão aciona recálculo, `FechamentoMensal` e `DiaPago`. Na tela, rejeitar individualmente percorre as ausências abertas da mesma raiz documental. Cancelar aceita também `aprovada` e grava descrição e trilha. O push web da rejeição individual pode apagar uma inscrição expirada. `app/ponto/signals.py:254–272`, `:19–50` · `app/ponto/services/ausencia.py:832–876` · `app/chamados/signals.py:122–146`, `:229–240`, `:351–371` · `app/chamados/services/regularizacao_ext.py:122–206` · `app/ponto/nucleo.py:40–59` · `app/ponto/services/fechamento.py:1144–1205`, `:1798–1824` · `app/ponto/views.py:2829–2893` · `app/chamados/push_utils.py:38–51`, `:114–118` |

No caminho síncrono rastreado, a escrita no fechamento é a invalidação acima. `ExportacaoDominio.registrar` e `invalidar` aparecem na porta do regerador, enquanto a montagem e o comparador só consultam. A foto cobre apenas o vigente encontrado naquele instante. Escrita efetiva em exportações de fora desse recorte fica **[nao verificado]**. `app/ponto/services/fechamento.py:1648–1656` · `app/folha/export.py:655–679`, `:711–746` · `app/folha/management/commands/regerar_txt_dominio.py:118–124` · `sonda_6a1.py.txt:104–128`

## 5. O que a sonda mede e onde pode errar

- **Mede o delta líquido em três instantes.** A passagem chama a view real com autenticação forçada, e a rejeição chama o serviço real mais a devolução manual. As tabelas fotografadas são comparadas por PK e por assinatura de todos os campos concretos, com `Cast` para texto. A ordem das linhas da foto não muda esse diff. Escritas intermediárias desfeitas antes da foto, tabelas omitidas, batidas de outro dia civil e exportações fora do vigente ficam invisíveis. Dia civil e dia do turno podem divergir na virada da meia-noite. A escolha é dos primeiros oito por escape na ordenação do censo, logo a proporção observada descreve a amostra. `sonda_6a1.py.txt:92–148`, `:152–188`, `:257–305` · `app/ponto/turnos.py:2902–2909`
- **O TXT mede bytes e quantidade.** A montagem herda ordenação por ano/mês/nome sem desempate explícito por PK entre nomes iguais. Uma troca só de ordem pode mudar o hash com a contagem igual. Aqui a contagem também mudou. O nome `TXT_do_gravado` na foto guarda apenas o lado `hoje`; mudanças no booleano `divergente`, retorno ao hash inicial e diferenças por matrícula ou rubrica ficam fora desse resultado. `app/ponto/models.py:481–485` · `app/folha/export.py:717–745`, `:539–553`, `:673–679` · `sonda_6a1.py.txt:125–128`, `:132–148` · `medida_6a1_resumo.txt:13–15`
- **Relógio e cache podem interferir.** A sonda fixa o fuso e deixa o relógio correr. A virada de data local pode mudar `sem_celula` na montagem, e o fim do turno pode mudar o que o cartório julga. O catálogo de ausência vive em cache de processo, compartilhado entre casos. Num snapshot de banco estável, expirar e carregar os mesmos dados é neutro; um cache inicialmente diferente do snapshot ou a entrada/saída do fallback pode alterar a leitura. O cache `_JANELA_MEMO` está no caminho de `prontidao`, fora desta montagem. Ocorrência dessas interferências fica **[nao verificado]**. `sonda_6a1.py.txt:357–358`, `:387–394` · `app/folha/export.py:1022–1067`, `:1238–1242`, `:1264–1296` · `app/ponto/services/cartorio.py:2064–2090` · `app/ponto/catalogo/ausencias.py:240–281`
- **Entre casos há rollback; entre as duas etapas do mesmo caso há continuidade.** Cada caso termina revertendo seu savepoint dentro do `REPEATABLE READ` externo. Isso isola as linhas de um caso do seguinte, inclusive quando a pergunta se repete para outro escape. Cache Python e consumo de IDs de sequência podem sobreviver ao rollback. A rejeição vê os chamados, perguntas e células deixados pela passagem do próprio caso. Os callbacks `on_commit` são descartados; a sonda invoca manualmente só a devolução das perguntas. A rejeição de `FALTA_DIA` aprovada é válida pelo serviço e corresponde ao flip do cancelamento, pois a view de rejeição seleciona apenas `ABERTAS`. `sonda_6a1.py.txt:293–316`, `:383–398` · `app/chamados/services/trava_do_colaborador.py:12–17` · `app/ponto/signals.py:254–272` · `app/ponto/services/ausencia.py:912–922` · `app/ponto/views.py:2835–2851`, `:2879–2893`
- **“Conclusivo” tem o alcance do detector.** O wrapper conta um PK devolvido por `criar_ausencia`; a prova de inserção é `Ausencia.novas` na foto. O detector recolhe exceções, logs com `exc_info` e logs de nível `ERROR` ou acima. A invalidação de fechamento captura exceção e pode emitir apenas `WARNING` sem `exc_info`; o catálogo pode

[truncado em 20000 de 22825 caracteres]

## 20/11 18:55 · sessao 51a596ddf1e6 · E4C-F5-CONSTROI · leitura · origem=Code

### `app/chamados/tests/test_main_dois_casos.py`

```python
import json
from contextlib import contextmanager
from datetime import timedelta
from unittest.mock import patch

from django.contrib.auth.models import User
from django.db import transaction
from django.test import TransactionTestCase
from freezegun import freeze_time

from chamados.models import DecisaoPergunta as D
from chamados.models import DisputaSupervisao, PerguntaDisputa as P, _OPCOES_ESCAPE_FALTA
from chamados.services import disputa_emissao as E
from chamados.services import materializacao as M
from chamados.services import validacao
from chamados.tests import cortes_f2_f9_apoio as apoio
from core.models import LogAuditoria
from escala.models import CelulaDia
from ponto.models import Ausencia
from ponto.services.ausencia import decidir
from ponto.services.cartorio import calado

# apoio_a1c_c8eb.py.txt:24–28 e app/chamados/tests/cortes_f2_f9_apoio.py:30
T1 = apoio.AGORA
T0 = T1 - timedelta(seconds=2)
T2 = T1 + timedelta(seconds=1)
T3 = T1 + timedelta(seconds=2)
DIA = T1.date()


@contextmanager
def medida(cenario):
    fatos = dict(
        apos_passagem=None,
        antes=None,
        depois=None,
        retorno_retirada=None,
        retornos_devolucao=[],
        nao_devolvidas=None,
        sem_contagem_na_trilha=None,
    )
    try:
        yield fatos
    except Exception as erro:
        fatos["tipo_excecao"] = type(erro).__name__
        raise
    finally:
        print(
            f"MEDIDA: {cenario} | "
            + json.dumps(fatos, default=lambda valor: valor.isoformat()),
            flush=True,
        )


class MainDoisCasosTest(TransactionTestCase):
    def _preparar(self):
        # apoio_a1c_c8eb.py.txt:32–39 e app/chamados/tests/cortes_f2_f9_apoio.py:39–44
        self.relogio = self.enterContext(freeze_time(T0))
        self.enterContext(calado())
        apoio.BaseCortes.setUpTestData.__func__(type(self))
        self.usuario = User.objects.create_user(username="col01")
        self.colab.usuario = self.usuario
        self.colab.save(update_fields=["usuario"])
        self.disputa = DisputaSupervisao.objects.create(colaborador=self.colab, contexto={})

    def _escape(self, tipo="escolha"):
        # apoio_a1c_c8eb.py.txt:41–47
        p, criada = P.abrir_conversa(
            disputa=self.disputa, motivo="saida_sem_entrada", pergunta="Por que faltou?",
            tipo_resposta=tipo, opcoes=list(_OPCOES_ESCAPE_FALTA),
        )
        if not criada or p.data_evento != DIA:
            self.fail("CENARIO NAO MONTAVEL: o escape não nasceu no dia da ausência.")
        return p

    def _irma_muda(self):
        # apoio_a1c_c8eb.py.txt:73–78
        p, criada = P.abrir_conversa(
            disputa=self.disputa, motivo="intervalo_saida",
            pergunta="A que horas saiu para o intervalo?", tipo_resposta="hora",
        )
        if not criada or p.data_evento != DIA:
            self.fail("CENARIO NAO MONTAVEL: a irmã muda não nasceu no dia da ausência.")
        return p

    def _irma_com_luz(self):
        # apoio_a1c_c8eb.py.txt:49–63
        celula = CelulaDia.objects.create(
            colaborador=self.colab, data=DIA, trabalha=True, origem="gerada",
            dna={"marcos": {"hf": "19:00"}},
        )
        celula.lavrar_veredito(
            via="cartorio", veredito="concorde", agora=T0,
            ata={"lampadas": [{"tipo": "S", "hora": "19:00", "acesa": True, "luz": "19:00"}]},
        )
        p, criada = P.abrir(
            disputa=self.disputa, motivo="orfao_14h", data_evento=DIA,
            pergunta="A que horas saiu?", tipo_resposta="hora",
        )
        if not criada or p.celula_id != celula.pk:
            self.fail("CENARIO NAO MONTAVEL: a irmã de hora não nasceu vinculada à célula.")
        return p

    def _responder(self, p, texto):
        # apoio_a1c_c8eb.py.txt:80–83
        p.refresh_from_db()
        ok, erro = E.responder_pergunta_disputa_core(
            p, texto, self.usuario, ja_respondida=p.respondida_em is not None,
        )
        if not ok:
            self.fail(f"CENARIO NAO MONTAVEL: a porta de resposta recusou {texto!r} ({erro}).")

    @staticmethod
    def _trilha(p):
        # apoio_a1c_c8eb.py.txt:138–140 e app/chamados/services/materializacao.py:1067–1071
        return LogAuditoria.objects.filter(
            acao=M.ACAO_IRMA_VIVA_NA_AUSENCIA, modelo="PerguntaDisputa", objeto_id=p.pk,
        )

    def _foto_grupo(self, ausencia):
        ausencia.refresh_from_db()
        perguntas = {}
        contadas = {}
        qs = P.objects.filter(disputa_id=self.disputa.pk).order_by("pk").prefetch_related("decisoes")
        for p in qs:
            leitura = D.leitura(p)
            perguntas[p.pk] = dict(
                tipo=p.tipo_resposta,
                estado=p.estado,
                via_resolucao=p.via_resolucao,
                materializada_em=p.materializada_em,
                resposta_composta=leitura.resposta_colab,
                respondida_composta_em=leitura.respondida_em,
                validada_composta_em=leitura.validada_em,
                atos=[
                    dict(pk=ato.pk, ato=ato.ato, instante=ato.ato_em)
                    for ato in p.decisoes.all()
                ],
            )
            contadas[p.pk] = self._trilha(p).filter(valor_depois__ausencia_pk=ausencia.pk).count()
        return dict(
            ausencia_pk=ausencia.pk,
            status_ausencia=ausencia.status,
            perguntas_absorvidas_por=[p.pk for p in M.perguntas_absorvidas_por(ausencia)],
            perguntas=perguntas,
            contadas_na_trilha=contadas,
        )

    @staticmethod
    def _novas_reaberturas(antes, depois):
        atos_antes = {
            ato["pk"] for p in antes["perguntas"].values() for ato in p["atos"]
        }
        return {
            pk for pk, p in depois["perguntas"].items()
            if p["estado"] == P.PENDENTE and any(
                ato["ato"] == D.REABERTURA and ato["pk"] not in atos_antes
                for ato in p["atos"]
            )
        }

    def _medir_rejeicao(self, fatos, escape, irma, *, retirar=False):
        self.relogio.move_to(T1)
        self._responder(escape, "ATESTADO")
        try:
            ausencia = Ausencia.objects.get(colaborador=self.colab, data=DIA, tipo="atestado")
        except Ausencia.DoesNotExist:
            self.fail("CENARIO NAO MONTAVEL: a passagem não criou o atestado do dia.")

        montado = fatos["apos_passagem"] = self._foto_grupo(ausencia)
        if retirar:
            self.relogio.move_to(T2)
            fatos["retorno_retirada"] = E.reabrir_para_correcao(
                P.objects.filter(pk=escape.pk), por=self.admin, motivo="Retirada da âncora",
            )
        antes = fatos["antes"] = self._foto_grupo(ausencia)

        devolver_real = M.devolver_absorvidas_da_ausencia_rejeitada
        chamadas = fatos["retornos_devolucao"]

        def observar_devolucao(a):
            chamada = dict(ausencia_pk=a.pk, status=a.status)
            chamadas.append(chamada)
            chamada["retorno"] = devolver_real(a)
            return chamada["retorno"]

        self.relogio.move_to(T3)
        # app/ponto/signals.py:259–279
        with patch.object(M, "devolver_absorvidas_da_ausencia_rejeitada", new=observar_devolucao):
            rejeitada = decidir(ausencia, "rejeitada", por=self.admin, motivo="Teste de rejeição") [nao verificado]
        fatos["retorno_decidir"] = dict(pk=rejeitada.pk, status=rejeitada.status)
        depois = fatos["depois"] = self._foto_grupo(ausencia)

        reabertas_antes = self._novas_reaberturas(montado, antes)
        reabertas_na_rejeicao = self._novas_reaberturas(antes, depois)
        candidatas = {pk for pk, p in antes["perguntas"].items() if p["tipo"] == "hora"}
        nao_devolvidas = candidatas - reabertas_antes - reabertas_na_rejeicao
        contadas = {pk for pk, n in depois["contadas_na_trilha"].items() if n}
        fatos.update(
            reabertas_antes=sorted(reabertas_antes),
            reabertas_na_rejeicao=sorted(reabertas_na_rejeicao),
            nao_devolvidas=sorted(nao_devolvidas),
            sem_contagem_na_trilha=sorted(nao_devolvidas - contadas),
        )

        self.assertEqual(depois["status_ausencia"], "rejeitada", "[M1] A ausência ficou rejeitada.")
        self.assertTrue(
            any(c["ausencia_pk"] == ausencia.pk and c["status"] == "rejeitada"
                and "retorno" in c for c in chamadas),
            "[M2] O callback real foi chamado com esta ausência rejeitada e retornou.",
        )
        vias_esperadas = {irma.pk: P.VIA_AUSENCIA_DIA}
        if retirar:
            vias_esperadas = {irma.pk: P.VIA_DECLARACAO, escape.pk: P.VIA_AUSENCIA_DIA}
        fotos = montado["perguntas"]
        campos = ("tipo", "estado", "via_resolucao", "materializada_em")
        observado = {"perguntas": {
            pk: tuple(fotos[pk][campo] for campo in campos) for pk in vias_esperadas
        }}
        esperado = {"perguntas": {
            pk: ("hora", P.RESOLVIDA, via, T1) for pk, via in vias_esperadas.items()
        }}
        if retirar:
            observado["grupo"] = sorted(montado["perguntas_absorvidas_por"])
            esperado["grupo"] = sorted([escape.pk, irma.pk])
            observado["retorno_retirada"] = fatos["retorno_retirada"]
            esperado["retorno_retirada"] = 1
        else:
            observado["escape_tipo"] = fotos[escape.pk]["tipo"]
            esperado["escape_tipo"] = "escolha"
        self.assertEqual(observado, esperado, "[M3] A montagem divergiu da foto do anexo.")
        self.assertEqual(
            fatos["sem_contagem_na_trilha"], [],
            "[R] Há perguntas de hora sem reabertura e sem linha de trilha desta ausência.",
        )

    def test_escolha_sem_ancora_e_atestado_rejeitado(self):
        with medida("grupo escolha sem âncora") as fatos:
            self._preparar()
            escape = self._escape()
            irma = self._irma_muda()
            self._medir_rejeicao(fatos, escape, irma)

    def test_ancora_retirada_e_atestado_rejeitado(self):
        with medida("grupo âncora retirada após formação") as fatos:
            self._preparar()
            irma = self._irma_com_luz()
            ancora = self._escape("hora")
            self._responder(irma, "19:02")
            with transaction.atomic():
                P.trava_do_colaborador(self.colab.pk)
                irma.refresh_from_db()
                validacao._carimbar_validacao(irma, self.admin)
            self._medir_rejeicao(fatos, ancora, irma, retirar=True)
```

Label para rodar a partir de `app/` na cópia descartável. (`app/manage.py:18`, `app/config/settings/ci.py:6`)

```bash
python3 manage.py test chamados.tests.test_main_dois_casos --settings=config.settings.ci
```

O `[M3]` usa a foto `apos_passagem`. No cenário sem âncora, `ausencia_dia` é a via esperada da irmã muda. No cenário ancorado, é a via esperada da própria pergunta de hora que recebe `ATESTADO`. A irmã com luz tem `declaracao` na foto do anexo. (`medida_dos_dois_em_c8eb.txt:1–2`, `app/chamados/models.py:2655`, `app/chamados/models.py:2683`)

### Nome ausente → substituto

| Nome procurado | Substituto e referência |
|---|---|
| `chamados.tests.test_e4c_f5_a1c_passagem_da_ausencia` e `PassagemDaAusenciaTest` | Fixtures locais com `BaseCortes.setUpTestData` do MAIN. Import procurado em `app/chamados/tests/`. (`medida_c8eb.py.txt:20`, `apoio_a1c_c8eb.py.txt:31–39`, `app/chamados/tests/cortes_f2_f9_apoio.py:37–44`) |
| `a1c.T0`, `a1c.T1`, `a1c.T2`, `a1c.T3`, `a1c.DIA` | Constantes locais derivadas de `apoio.AGORA`, com os deslocamentos do anexo. (`apoio_a1c_c8eb.py.txt:24–28`, `app/chamados/tests/cortes_f2_f9_apoio.py:30`) |
| `PassagemDaAusenciaTest._escape`, `._irma_muda`, `._irma_da_celula`, `._irma_com_luz` | `_escape` e `_irma_muda` locais. `_irma_com_luz` local incorpora a fixture da célula com luz acesa. (`apoio_a1c_c8eb.py.txt:41–63`, `apoio_a1c_c8eb.py.txt:73–78`, `app/chamados/models.py:2963`, `app/chamados/models.py:2981`, `app/escala/models.py:1819`) |
| `PassagemDaAusenciaTest._responder`, `._passagem`, `._ausencia` | `_responder` local, avanço para `T1` com resposta `ATESTADO` e consulta direta do atestado pela mesma chave. (`apoio_a1c_c8eb.py.txt:80–104`, `apoio_a1c_c8eb.py.txt:135–136`, `app/chamados/services/disputa_emissao.py:1439`, `app/chamados/services/disputa_emissao.py:1560–1563`, `app/ponto/models.py:660`) |
| `PassagemDaAusenciaTest._trilha` | `_trilha` local sobre o mesmo `LogAuditoria`, filtrando ação, modelo, PK e `valor_depois__ausencia_pk`. O PK é passado como inteiro, conforme o campo `objeto_id`. (`apoio_a1c_c8eb.py.txt:138–140`, `app/chamados/services/materializacao.py:1067–1071`, `app/chamados/services/materializacao.py:1089`, `app/core/models.py:296–301`) |
| `DecisaoPergunta.composta` | `DecisaoPergunta.leitura` para os três campos compostos da foto. (`apoio_a1c_c8eb.py.txt:120`, `app/chamados/models.py:4295–4326`) |
| `chamados.services.vigia_ato_x_coluna.contar` | Sem substituto para o placar A/B do anexo. O módulo foi procurado em `app/chamados/services/`. A medida pedida lê `PerguntaDisputa.estado`, os atos persistidos e a trilha da ausência. (`medida_c8eb.py.txt:18`, `app/chamados/models.py:3226`, `app/chamados/models.py:4166–4168`, `app/chamados/services/materializacao.py:1067–1071`) |

### Passos sem caminho de produção localizado

- Nenhum nos dois cenários. As referências para passagem, assinatura da irmã, retirada administrativa, rejeição e callback são `app/chamados/services/disputa_emissao.py:1560–1563`, `app/chamados/services/validacao.py:161–175`, `app/chamados/services/disputa_emissao.py:1584`, `app/ponto/services/ausencia.py:1057–1088` e `app/ponto/signals.py:259–279`.
- Montagem e resultados em execução no MAIN [nao verificado].

## 20/11 18:54 · sessao 04615e8207bf · E4C-F5-CONSTROI · sonda · origem=Code

```python
"""
Sonda para bin/sonda_leitura.sh em produção, sem --sombra. O envelope
executa o Python no shell Django, schema juliani, com a role leitor e
a árvore montada em leitura. bin/sonda_leitura.sh:93, :140, :153, :235, :239.

Autoridades do funil

- Pergunta de hora: PerguntaDisputa.tipo_resposta e a entrada 'hora' do
  catálogo. É o recorte do próprio leitor da devolução.
  app/chamados/models.py:2629; app/chamados/catalogo/respostas.py:20;
  app/chamados/services/materializacao.py:1121.
- Resolvida neste funil: materializada_em preenchido, o carimbo que o leitor
  da devolução exige. O rótulo PerguntaDisputa.estado também consulta a
  validação da sequência e, conforme o caso, o C6. A sonda informa
  separadamente quantas desse recorte têm o rótulo confirmado pelas faces
  SQL e quantas ficam indeterminadas sem rodar o motor.
  app/chamados/services/materializacao.py:1121;
  app/chamados/models.py:3226, :3539, :3569; app/chamados/juizes.py:1470.
- Via: via_resolucao e as constantes VIA_AUSENCIA_DIA e VIA_DECLARACAO.
  app/chamados/models.py:2655, :2683, :2748.
- Ato e ausência ligada: DecisaoPergunta.ABSORCAO e o FK absorcao_ausencia
  da linha selecionada. A ordem da sequência é (ato_em, pk).
  app/chamados/models.py:4152, :4166, :4178, :4199, :4202.
- REJEITADA: Ausencia.status == 'rejeitada'.
  app/ponto/models.py:704, :809; app/chamados/services/materializacao.py:1154.
- Outra ausência: efeito 'cobranca', no colaborador e no dia da ausência
  ligada, como na guarda da devolução. A carga chama _pelo_efeito e
  _q_cobertura da árvore. O próprio efeito exclui a rejeitada.
  app/chamados/services/materializacao.py:1146, :1161;
  app/ponto/turnos.py:2523, :2768, :2773;
  app/chamados/services/carga_pergunta_viva.py:99.
- Competência: data_evento e empresa do colaborador da pergunta.
  _competencia chama janela_atual e usa o mês e ano do fim da janela.
  Entregue é o resultado de competencia_entregue com as exportações já
  anotadas por anotar_vigencia. Sem data_evento, o K6 fica sem consulta.
  app/chamados/models.py:2610, :2620;
  app/chamados/services/materializacao.py:788;
  app/chamados/services/passivo_validado.py:86; app/ponto/janelas.py:36, :60;
  app/folha/export.py:556; app/folha/models.py:192, :203.

LACUNA DE AUTORIDADE — ABSORÇÃO VIGENTE

A árvore tem um escritor do carimbo, escritores que o retiram ou trocam
a via e um leitor da decisão efetiva da tarefa. Um juiz único que defina
“ABSORÇÃO vigente” para esta contagem [nao verificado]. A sonda publica
as três leituras abaixo, cada uma com o funil completo.

- carimbo_do_ato: ABSORCAO cujo ato_em coincide com materializada_em na
  via atual. Se há várias linhas no mesmo instante, a sonda usa a última
  pela ordem do modelo. O escritor grava o mesmo instante no carimbo e
  no ato. Descarimbar tira instante e via. Retratar pode trocar só a via.
  app/chamados/services/materializacao.py:1047, :1059;
  app/chamados/models.py:2217, :3399, :3485, :4199.
- sequencia_sem_desfazimento: última ABSORCAO após o último ato que
  registra a retirada do carimbo ou a troca da via por retratação.
  REABERTURA e RECUSA vêm de ATOS_COM_FOTO, usado pelo escritor que
  descarimba. A REABERTURA compensadora também tira o carimbo.
  app/chamados/models.py:4164, :1694, :2217;
  app/chamados/services/disputa_emissao.py:1627, :1639, :1656.
- ato_lido_na_tarefa: ABSORCAO que _pela_decisao lê como efetiva.
  O leitor pode conservar o ato diante de respostas que preservam a
  validação, ignora a reabertura compensadora e distingue o passivo sem
  linha. Quando lê ABSORCAO, ela é a última ABSORCAO da sequência visível.
  app/chamados/services/tarefa_do_dia.py:142, :154, :158, :184, :390, :425;
  app/chamados/models.py:4220, :4237, :4295.
"""

import json


LEITURAS = (
    "carimbo_do_ato",
    "sequencia_sem_desfazimento",
    "ato_lido_na_tarefa",
)
DEGRAUS = (
    "perguntas_de_hora",
    "resolvidas",
    "nas_duas_vias",
    "com_ato_absorcao",
    "com_absorcao_vigente",
    "com_ausencia_ligada",
    "ligada_rejeitada",
    "sem_outra_ausencia_cobrindo_o_dia",
)
REJEITADA = "rejeitada"  # app/ponto/models.py:704, :809.


def ler_absorcoes(alvo, agora):
    # app/chamados/models.py:4199; app/chamados/services/tarefa_do_dia.py:390.
    linhas = (
        D.objects.filter(pergunta_id__in=alvo.order_by().values("pk"))
        .select_related("pergunta__disputa", "absorcao_ausencia")
        .only(
            "pk", "pergunta", "ato", "ato_em", "ato_texto",
            "da_ressuscitacao", "resposta_mantem_validacao",
            "absorcao_ausencia", "absorcao_ausencia__status",
            "absorcao_ausencia__colaborador", "absorcao_ausencia__data",
            "pergunta__disputa", "pergunta__disputa__chamado",
            "pergunta__chamado_destino", "pergunta__materializada_em",
            "pergunta__respondida_em", "pergunta__validada_em",
            "pergunta__validada_por", "pergunta__resposta_colab",
        )
        .order_by("pergunta_id", *D._meta.ordering)
        .iterator(chunk_size=5000)
    )
    desfazem = (*D.ATOS_COM_FOTO, D.RETRATACAO)
    leituras_absorcao = {
        "absorvida_aprovada", "absorcao_rejeitada",
        "absorcao_aguarda_documento", "absorcao_pendente",
    }  # app/chamados/services/tarefa_do_dia.py:184.

    por_pergunta = {}
    for pk, grupo in groupby(linhas, key=attrgetter("pergunta_id")):
        atos = list(grupo)
        if not any(d.ato == D.ABSORCAO for d in atos):
            continue
        p = atos[0].pergunta
        carimbo = sequencia = ultima_visivel = None
        visiveis = []
        for d in atos:
            visivel = d.ato_em <= agora and not D.es_da_ressuscitacao(d)
            if visivel:
                visiveis.append(d)
            if d.ato in desfazem:
                sequencia = None
            elif d.ato == D.ABSORCAO:
                sequencia = d
                if d.ato_em == p.materializada_em:
                    carimbo = d
                if visivel:
                    ultima_visivel = d

        tarefa = None
        if ultima_visivel is not None:
            # O passivo usa as linhas reais; a leitura usa a vista cortada.
            # app/chamados/services/tarefa_do_dia.py:402, :425, :431.
            ja = {(pk, d.ato, d.ato_em) for d in atos
                  if d.ato in (D.RESPOSTA, D.VALIDACAO)}
            passivo = (pk,) if any(
                linha["ato_em"] <= agora
                for linha in D.linhas_da_projecao_sem_linha([p], ja)
            ) else ()
            vista = SimpleNamespace(
                pk=pk, resposta_colab=p.resposta_colab,
                respondida_em=(
                    p.respondida_em
                    if p.respondida_em is not None and p.respondida_em <= agora
                    else None
                ),
                validada_em=(
                    p.validada_em
                    if p.validada_em is not None and p.validada_em <= agora
                    else None
                ),
                validada_por_id=p.validada_por_id,
                decisoes=SimpleNamespace(all=lambda: visiveis),
            )
            # Só se usa a classificação do ato. Fila, lavra e efeitos não
            # decidem o ramo ABSORCAO. app/chamados/services/tarefa_do_dia.py:152, :173, :184.
            razao, _ = _pela_decisao(
                p, visiveis, {}, frozenset(), passivo,
                leitura=D.leitura(vista),
            )
            if razao in leituras_absorcao:
                tarefa = ultima_visivel

        por_pergunta[pk] = dict(zip(LEITURAS, (carimbo, sequencia, tarefa)))
    return por_pergunta


def ler_cobertura(absorcoes):
    pares = {
        (ausencia.colaborador_id, ausencia.data)
        for leituras in absorcoes.values()
        for ato in leituras.values()
        if ato is not None
        for ausencia in (ato.absorcao_ausencia,)
        if ausencia is not None and ausencia.status == REJEITADA
    }
    if not pares:
        return set()
    dias = [dia for _, dia in pares]
    # O mesmo recorte SQL é aplicado em memória para cada par distinto. [nao verificado]
    # app/ponto/turnos.py:2523, :2773;
    # app/chamados/services/carga_pergunta_viva.py:10, :99.
    qs = _pelo_efeito(
        Ausencia.objects.filter(colaborador_id__in={cid for cid, _ in pares}),
        "cobranca",
    ).filter(_q_cobertura(min(dias), max(dias))).only(
        "colaborador", "data", "data_fim", "tipo",
    ).order_by("colaborador_id")
    por_colaborador = {
        cid: LinhasLidas(grupo)
        for cid, grupo in groupby(
            qs.iterator(chunk_size=5000), key=attrgetter("colaborador_id"),
        )
    }
    return {
        (cid, dia) for cid, dia in pares
        if cid in por_colaborador
        and por_colaborador[cid].filter(_q_cobertura(dia, dia)).exists()
    }


def ler_entregues(chaves):
    # app/chamados/services/tarefa_do_dia.py:558;
    # app/folha/models.py:203; app/folha/export.py:580.
    if not chaves:
        return {}
    registros = list(ExportacaoDominio.objects.filter(
        empresa_id__in={e for e, _, _ in chaves},
        mes__in={m for _, m, _ in chaves},
        ano__in={a for _, _, a in chaves},
    ).only("pk", "empresa", "mes", "ano", "invalidada_em").order_by())
    por_chave = defaultdict(list)
    for registro in anotar_vigencia(registros):
        por_chave[registro.empresa_id, registro.mes, registro.ano].append(registro)
    return {
        chave: competencia_entregue(*chave, exportacoes=por_chave[chave])
        for chave in chaves
    }


def nivel_com_absorcao(ato, cobertos):
    if ato is None:
        return 4
    ausencia = ato.absorcao_ausencia
    if ausencia is None:
        return 5
    if ausencia.status != REJEITADA:
        return 6
    if (ausencia.colaborador_id, ausencia.data) in cobertos:
        return 7
    return 8


def novo_funil():
    return {leitura: Counter(dict.fromkeys(DEGRAUS, 0)) for leitura in LEITURAS}


def medir():
    vias = (P.VIA_AUSENCIA_DIA, P.VIA_DECLARACAO)
    # app/chamados/catalogo/respostas.py:20;
    # app/chamados/services/materializacao.py:1121.
    base = P.objects.filter(tipo_resposta="hora")
    alvo = base.filter(materializada_em__isnull=False, via_resolucao__in=vias)
    agora = timezone.now()
    absorcoes = ler_absorcoes(alvo, agora)
    cobertos = ler_cobertura(absorcoes)

    # q_vivas é um superconjunto por contrato. Estar fora dele, ou ter
    # validação pelo ato, basta para confirmar RESOLVIDA com carimbo.
    # app/chamados/models.py:3235, :3539, :3580.
    perguntas = base.annotate(
        estado_confirmado=Case(
            When(
                Q(materializada_em__isnull=False)
                & (P.q_validada_pelo_ato() | ~P.q_vivas()),
                then=Value(True),
            ),
            default=Value(False),
            output_field=BooleanField(),
        ),
    ).order_by("pk").values_list(
        "pk", "colaborador__empresa_id",
        "colaborador__empresa__dia_inicio_competencia",
        "data_evento", "materializada_em", "via_resolucao", "estado_confirmado",
    ).iterator(chunk_size=5000)

    total = novo_funil()
    empresas = defaultdict(novo_funil)
    competencias = defaultdict(novo_funil)
    finais = {leitura: defaultdict(list) for leitura in LEITURAS}
    for pk, empresa, corte, dia, carimbo, via, confirmado in perguntas:
        mes = ano = None
        if dia is not None:
            mes, ano = _competencia(
                dia, SimpleNamespace(dia_inicio_competencia=corte),
            )
        chave = (empresa, mes, ano)
        destinos = (total, empresas[empresa], competencias[chave])
        nivel_base = 1
        if carimbo is not None:
            nivel_base = 3 if via in vias else 2
        atos = absorcoes.get(pk) if nivel_base == 3 else None

        for leitura in LEITURAS:
            nivel = nivel_base if atos is None else nivel_com_absorcao(
                atos[leitura], cobertos,
            )
            for destino in destinos:
                contagem = destino[leitura]
                contagem.update(DEGRAUS[:nivel])
                if nivel >= 2:
                    faixa = "confirmadas" if confirmado else "indeterminadas"
                    contagem["rotulo_resolvida_" + faixa] += 1
                    if nivel == len(DEGRAUS):
                        contagem["rotulo_resolvida_no_final_" + faixa] += 1
            if nivel == len(DEGRAUS):
                finais[leitura][chave].append(pk)

    entregues = ler_entregues({c for c in competencias if c[1] is not None})
    saida = []
    for empresa in sorted(empresas):
        saida.append({
            "tipo": "empresa", "empresa_pk": empresa, "funis": empresas[empresa],
        })
    for chave in sorted(competencias, key=lambda c: (c[0], c[2] or 0, c[1] or 0)):
        empresa, mes, ano = chave
        saida.append({
            "tipo": "competencia",
            "empresa_pk": empresa,
            "competencia": f"{ano:04d}-{mes:02d}" if mes is not None else None,
            "entregue": entregues.get(chave),
            "funis": competencias[chave],
        })
    for leitura, grupos in finais.items():
        pks = {True: [], False: [], None: []}
        for chave, lista in grupos.items():
            pks[entregues.get(chave)].extend(lista)
        saida.append({
            "tipo": "casos_finais", "leitura": leitura,
            "pergunta_pks_entregue": sorted(pks[True]),
            "pergunta_pks_nao_entregue": sorted(pks[False]),
            "pergunta_pks_sem_competencia": sorted(pks[None]),
        })
    saida.append({"tipo": "total", "status": "OK", "funis": total})
    return saida


try:
    from collections import Counter, defaultdict
    from itertools import groupby
    from operator import attrgetter
    from types import SimpleNamespace

    from django.db import connection, transaction
    from django.db.models import BooleanField, Case, Q, Value, When
    from django.utils import timezone

    from chamados.models import DecisaoPergunta as D, PerguntaDisputa as P
    from chamados.services.carga_pergunta_viva import LinhasLidas
    from chamados.services.passivo_validado import _competencia
    from chamados.services.tarefa_do_dia import _pela_decisao
    from folha.export import competencia_entregue
    from folha.models import ExportacaoDominio, anotar_vigencia
    from ponto.models import Ausencia
    from ponto.turnos import _pelo_efeito, _q_cobertura

    # Até 5 SELECTs de dados: decisões e perguntas (1 cada), cobertura
    # (0 ou 1), exportações (0 ou 1), anotação da vigência (0 ou 1).
    # Mais um SET TRANSACTION e o controle da transação. A contagem é de
    # consultas lógicas da sonda, à parte do bootstrap e das operações
    # de cursor do backend. app/folha/models.py:203; bin/sonda_leitura.sh:235.
    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        linhas_json = [
            json.dumps(linha, ensure_ascii=True, allow_nan=False)
            for linha in medir()
        ]
    for linha_json in linhas_json:
        print(linha_json, flush=True)
except BaseException as exc:
    print(json.dumps({
        "tipo": "total",
        "status": "INCONCLUSIVO",
        "tipo_excecao": type(exc).__name__,
    }, ensure_ascii=True), flush=True)
    raise SystemExit(1) from None


# O que a sonda não mede
#
# - O estado RESOLVIDA exato nos casos indeterminados e a materialização de
#   fato. Essas respostas podem exigir o C6 e a apuração de testemunhas.
#   Os campos rotulo_resolvida_* mostram o limite inferior e a parcela
#   indeterminada do recorte. Campo de contagem ausente nessa família vale
#   zero. app/chamados/models.py:3226; app/chamados/juizes.py:666, :1470.
# - Ligações inferidas do legado sem ato. O carimbo legado pode aparecer
#   até o degrau das vias. A reconstrução pelo escape, carimbo e batida
#   pertence a outro leitor. app/chamados/services/materializacao.py:1101, :1121.
# - Entrega quando falta o dia do fato. Nessa saída, competência e entregue
#   são null. K6 mede TXT vigente para a tripla da empresa.
#   app/chamados/services/materializacao.py:794; app/folha/export.py:556.
# - Execução e contagens de produção [nao verificado]. O anexo é um molde
#   de script e não contém resultados desta contagem [nao verificado].
#   O JSONL e o tratamento de exceções são do Python. O envelope emite
#   mensagens próprias e provisiona credencial, permissões e trilha.
#   bin/sonda_leitura.sh:126, :140, :153, :216, :221, :239, :248.
```

## 20/11 18:52 · sessao 8c14b0a44809 · E4C-F5-CONSTROI · censo · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 20/11 18:51 · sessao 4450c6faacf5 · E4C-F5-CONSTROI · verificador · origem=Code

# AVAL-DEVOLVIDO — RESPOSTAS-2011-1845

**Item 1**, pela condição comum de remedição e pelo critério de acusação em (06). **Item 2**, no trecho recebido, pela lacuna declarada e pelas condições dos dois ramos do main. **Item 3**, pelo referente incompleto da medida 6-A1 e pela condição de ordem. Os dois testes pedidos no item 1 têm lacunas confirmadas na árvore. (`respostas_2011_1845.txt`, cabeçalho e itens 1–3; `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:434–440`; `app/chamados/tests/test_e4c_f5_selo_catraca.py:180–197`.)

Usei a árvore fornecida como archive de `c8eb8eddf`. A identidade do commit pelo Git e o HEAD da worktree de construção são [nao verificado]. As execuções mencionadas abaixo vêm dos anexos. A revisão foi só de leitura, sem alteração de arquivos.

**Avais lidos.** Em `avais_vigentes.txt`, li os 12 `respostas_2011_*.txt` de 1100, 1135, 1255, 1320, 1355, 1400, 1410, 1415, 1440, 1450, 1545 e 1700, e os cabeçalhos `f5_a1c_e_silencio_q4_2011_1230.txt`, `f5_ato_x_coluna_invariante_2011_0545.txt`, `f5_chokepoint_teto_medido_2011_0110.txt`, `forca_na_f5_e_cadeia_2011_1340.txt`, `e4c_f5_constroi_1911_0815.txt`, `f5_classe_nao_caso_1911_2050.txt` e `f5_classe_v_1911_2230.txt`. As referências por horário abaixo apontam para esses cabeçalhos literais.

## Item 1 (78) — Selo do par no sentido ato → coluna

1. **Premissas. CONFERE** que a coluna é `validada_em` e que `RECUSA`, `REABERTURA` e `DEVOLUCAO` limpam a validação na leitura do ato. O comparador verifica presença na composta e na coluna. O selo principal parte das escritas reconhecidas da coluna, e o selo que parte do ato aceita apenas `validacao`. Portanto, um compensador que perde toda menção à coluna pode escapar dessa enumeração. **NÃO CONFERE** a frase do RELATO de que os três testes de `volta3_selos` cobrem validação iniciada pelo ato. O teste de `:33` é do literal S14, e os de `:41` e `:61` cobrem a validação. (`app/chamados/models.py:4325–4345,4393–4399`; `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:148–172,434–440`; `app/chamados/tests/test_e4c_f5_volta3_selos.py:12–29,33–70`.)
2. **Colisão.** Para o selo, não achei colisão no conjunto lido. O 0545 prevê “o ato sem a coluna, derruba o teste”, e o 1100 preserva “a equivalencia que o LEITOR ve”. O novo literal, “o escritor que grava ato que DESFAZ [...] e omite a coluna no UPDATE”, é compatível com ambos. **Há colisão na condição comum ao item 1.** O 1845 diz “A/B e instante_3formas remedidos SO se algum .py de producao mudar”. O 1440 item 1 exige “rodar a MESMA sonda [...] na arvore final, junto do A/B do item 82”, como “Condicao de pouso”. O 1255 também exige “A=0 e B=0 remedidos na arvore final”. A suficiência de uma medição anterior para cumprir esses literais na árvore final é [nao verificado]. A repetição da suite é compatível com o 1700 item 2, que diz “a suite teria de rodar de novo” quando a falta pede código. (`respostas_2011_1845.txt`, item 1; `avais_vigentes.txt`, 0545 item 1, 1100 item 1, 1255, 1440 item 1 e 1700 item 2.) [nao verificado]
3. **Literal.** “Caso que MORDE por mutante” deixa em aberto se a prova precisa exercer a lacuna nova. O mutante existente `d_devolucao_mantem_coluna` troca o `save` por `pass`, mas deixa a atribuição à coluna, que já entra no censo e é examinada quanto à falta de persistência. O RELATO atribui a morte desse mutante ao teste de comportamento. O caso novo precisa ser distinguido desse controle para demonstrar a enumeração pedida. “UPDATE” também passa por duas formas na árvore, `save(update_fields=...)` na devolução e `descarimbar_em_lote(..., **campos)` na reabertura e na recusa. A condição comum de remedição tem o conflito acima. (`app/chamados/tests/e4c_f5_volta3_mutantes.py:61–64`; `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:148–172,379–393`; `app/chamados/services/validacao.py:284–296,938–944`; `app/chamados/services/disputa_emissao.py:1630–1642,1664–1684`; `app/chamados/models.py:3536–3539`; `parei_codigo_e_acrescimo.txt`, “o mutante d_devolucao_mantem_coluna morre por teste de comportamento”.)
4. **Alcance.** O literal “so .py de TESTE” e “PAREI com a lista, nao cura por conta propria” restringe o trabalho ao selo e à leitura das fontes. Não identifiquei ordem de tocar exportada, dinheiro, escala, vínculo ou arquivo protegido. Se “na copia” significar o próprio archive desta revisão, a execução da construção ali não fecha. O wrapper exige uma worktree registrada no hash, segundo o contrato do construtor. (`respostas_2011_1845.txt`, item 1; `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:443–465`; `bin/codex_pergunta.sh:27–37,216–235`; `CLAUDE.md:265–268`.)

## Item 1 (06) — Catraca e `update_fields`

1. **Premissas. CONFERE** que o visitante cobre `getattr` e trata `refresh_from_db(fields=...)`, sem tratamento de `update_fields`. A única ocorrência de `update_fields` no arquivo da catraca está no controle que espera conjunto vazio. **CONFERE** o escopo estático de 13 funções em oito arquivos. O resultado “0 acusacoes, Ran 35 OK” é o que o RELATO registra. A execução independente é [nao verificado]. (`app/chamados/tests/test_e4c_f5_selo_catraca.py:11–26,81–105,180–197,291–307,360–369`; `parei_codigo_e_acrescimo.txt`, trecho “Catraca hoje”.)
2. **Colisão.** Não achei colisão para acrescentar a varredura. O 1845 diz “varrer string em update_fields, alem de getattr e refresh_from_db”, e o 0815 exige “varrendo tambem string em getattr/update_fields/refresh_from_db”. Há o mesmo conflito de remedição do (78). Uma leitura que proíba toda ocorrência do campo ainda entra em tensão com o contrato de escrita do 0545, “para CADA escritor da coluna validada_em [...] dizer se o ato e gravado ou desfeito na MESMA transacao”. (`respostas_2011_1845.txt`, item 1; `avais_vigentes.txt`, 0815 etapa B, 0545 item 1, 1255 e 1440 item 1.)
3. **Literal.** “Varrer string” distingue mal a ocorrência do campo e a violação que deve fazer o caso morder. O controle existente aceita `perg.save(update_fields=['validada_em'])` depois da atribuição. A catraca inclui `devolver_validacao_recusada`, que tem esse padrão em produção. Se toda ocorrência acusar, esse sítio aciona o `PAREI` e o controle que passa falha. Se houver uma distinção, o 1845 deixa seu critério implícito. (`app/chamados/tests/test_e4c_f5_selo_catraca.py:21–24,265–279,360–369`; `app/chamados/services/validacao.py:284–296`; `respostas_2011_1845.txt`, “Se qualquer dos dois acusar sitio de PRODUCAO”.)
4. **Alcance.** A catraca lê o arquivo de produção para julgar as funções cadastradas. Não identifiquei ordem de alterar o escritor apontado, nem alcance adicional sobre exportada, dinheiro, escala, vínculo ou arquivo protegido. O próprio aval manda listar e parar se acusar produção. (`app/chamados/tests/test_e4c_f5_selo_catraca.py:298–307`; `respostas_2011_1845.txt`, item 1; `CLAUDE.md:265–268`.)

## Item 2 (b) — Frequência em produção

1. **Premissas. CONFERE** que a cópia tem as duas vias, carimbo atual e vínculo da ausência no ato. `absorcao_vigente(p)` exige ABSORÇÃO da mesma pergunta no instante do carimbo, com os dois vínculos, e invalida a candidata por `REABERTURA` ou `RECUSA` posterior na ordem de inserção. Rejeitar a ausência, por si, deixa essa vigência de pé. **CONFERE** que a porta de devolução verifica `rejeitada` e `ausencia_cobre(..., para='cobranca')`, mas depois encontra o grupo pela resposta de escape numa pergunta de hora do mesmo grupo. **NÃO CONFERE** usar esse leitor de grupo ou o zero do vigia como substituto da sonda. O vigia filtra disputa aberta e pergunta viva, e o A/B mede só o par de validação. Total, estratos e casos em exportada em produção são [nao verificado]. (`app/chamados/models.py:4229–4232,4402–4425`; `app/chamados/services/materializacao.py:1072–1090,1151–1167,1187–1198`; `app/chamados/services/vigia_fio.py:755–785`; `app/chamados/services/vigia_ato_x_coluna.py:8–24`; `parei_codigo_e_acrescimo.txt`, “nem quantas ha na frota”.)
2. **Colisão.** Não achei colisão no conjunto lido. O 1450 já diz “Caso em competencia exportada = lista no topo, nada se toca”, literal repetido no 1845. Sua sonda anterior procura “RESPOSTA inserida depois de ABSORCAO vigente do mesmo PK”. A deste item pede “cuja ausencia ligada esta REJEITADA e sem outra ausencia cobrindo o dia”. São populações definidas por condições diferentes. (`avais_vigentes.txt`, 1450 item 1; `respostas_2011_1845.txt`, bloco (b).)
3. **Literal.** O cabeçalho e a alínea (a) faltam, assim como “linha PROIBIDO / NAO alcanca / LEI-AKITA, se havia”. O predicado H4 é booleano e o carimbo e seus dois vínculos têm referente na cópia, mas o item não explicita qual linha fornece a “ausencia ligada” se houver mais de uma ABSORÇÃO elegível no mesmo instante. O leitor H4 percorre por PK e substitui a candidata, retornando só `bool`. “Cobrindo o dia” tem dois efeitos na árvore, cobrança e folha. A devolução usa cobrança, que admite ausência não rejeitada. A competência do dia também tem referente, pela janela da empresa e pelo TXT vigente do K6. O aval não explicita se “exportada” é esse corte ou o status `exportado` do `FechamentoMensal`, que tem leitor próprio. A sonda executada e sua árvore efetiva são [nao verificado]. (`respostas_2011_1845.txt`, cabeçalho e (b); `app/chamados/models.py:4253–4259,4417–4425`; `app/ponto/turnos.py:2523–2532,2550–2571`; `app/chamados/services/materializacao.py:1194–1198`; `app/chamados/services/vigia_fio.py:789–811`; `app/folha/export.py:556–594`; `app/ponto/janelas.py:75–89`.)
4. **Alcance.** A sonda toca exportada como leitura e lista, como declarado. A role do envelope recebe `SELECT`. As portas de decidir ausência, devolver perguntas e reabrir disputa têm escrita e pertencem ao ensaio de reprodução, fora do envelope. Não há no trecho recebido autorização para aplicá-las em produção, nem ordem para gravar dinheiro, escala ou vínculo. (`respostas_2011_1845.txt`, “pelo envelope de leitura” e “nada se toca”; `bin/sonda_leitura.sh:54–59,155–162,226–240`; `app/ponto/services/ausencia.py:1079–1088,1112–1125`; `app/chamados/services/materializacao.py:1206–1222`; `CLAUDE.md:265–268,406–416`.)

## Item 2 — “Main REPRODUZ os dois”

1. **Premissas. CONFERE** que há dois casos relatados na F5. No primeiro, escolha sem âncora, a irmã fica resolvida por `ausencia_dia`. No segundo, âncora retirada, a irmã fica resolvida por `declaracao`. O anexo relata grupo vazio e devolução zero nos dois. A árvore mostra como isso pode ocorrer mesmo com a linha de ABSORÇÃO gravada, pois a recuperação do grupo exige uma âncora de hora ainda presente na seleção. A identidade byte a byte do leitor é afirmação do RELATO. A identidade do caminho que cria o grupo e a reprodução de ambos no main são [nao verificado]. (`parei_codigo_e_acrescimo.txt`, “VERMELHO 2” e “O QUE SEI”; `app/chamados/services/materializacao.py:1036–1042,1083–1090,1154–1167,1194–1198`.)
2. **Colisão.** A inscrição na fila é compatível com o 1255 item 2, “registrada na E4D junto da migracao do grupo para a linha ABSORCAO”. O “fica como esta, CONTADA” daquele aval tem a premissa literal “O dia esta coberto por ausencia aprovada”. O 1320 item 2 especifica “CONTADA = trilha da tentativa + linha na E4D” e “FALTA aprovada (nao atestado aberto)”. Os dois casos relatados aqui são de ausência rejeitada. Para a sequência do opus, “A F5 segue ao opus” deixa de fechar **se** for lido como liberação dos requisitos ainda pendentes. O 1230 diz “os seis pontos [...] sao medida devida ANTES do PRONTO da F5” e o 1700 proíbe “reduzir o PRONTO”. O 1440 ainda põe INSTANTE “ANTES de qualquer fatia da E4d”. Registrar na fila é compatível com essa ordem. (`respostas_2011_1845.txt`, ramo “REPRODUZ”; `avais_vigentes.txt`, 1230, 1255 item 2, 1320 item 2, 1440 item 2 e 1700 item 2; `parei_codigo_e_acrescimo.txt`, “INCOMPLETO”.)
3. **Literal.** Falta o critério recebido de reprodução comparável no main. Reproduzir apenas a falha de `sem_contagem_na_trilha` deixa sem prova o mecanismo, porque o instrumento forma o conjunto com **todas** as perguntas de hora que não foram reabertas e verifica a ausência de trilha nesse conjunto. O próprio RELATO diz “O asserto e do instrumento, nao de aval”. O item 93 tem o referente citado no RELATO, “fila E4D: grupo pela linha ABSORCAO, ancora fora”. O texto integral do item 93, os REDs portados e o número de (b) são [nao verificado]. A entrada na fila com o número depende do resultado de (b). (`respostas_2011_1845.txt`, lacuna e ramo “REPRODUZ”; `teste_das_seis_medidas.py.txt`, “`nao_devolvidas = candidatas - reabertas_antes - reabertas_na_rejeicao`” e “`self.assertEqual(fatos["sem_contagem_na_trilha"], [])`”; `parei_codigo_e_acrescimo.txt`, “O QUE SEI” e “NAO SEI”.)
4. **Alcance.** O ramo manda registrar e anexar os casos. Não identifiquei nele autorização de executar a futura migração E4D ou alterar gravado, dinheiro, escala, vínculo ou arquivo protegido. A lista de exportadas continua sujeita ao literal “nada se toca”. (`respostas_2011_1845.txt`, ramo “REPRODUZ” e linha de exportada; `avais_vigentes.txt`, 1440 item 2; `CLAUDE.md:265–268`.)

## Item 2 — “Main NAO reproduz algum”

1. **Premissas. [nao verificado]** que o main deixe de reproduzir algum caso e que uma diferença seja atribuível à F5. O teste fornecido reutiliza `PassagemDaAusenciaTest` e a porta real da cópia. A passagem toma decisões antes de consultar o leitor de grupo, incluindo releitura sob trava, escolha da via, guarda do par e exigência de grupo recuperável quando ancorado. Confirmar só a igualdade do leitor deixa essas decisões sem comparação. (`teste_das_seis_medidas.py.txt`, “`_passagem = a1c.PassagemDaAusenciaTest._passagem`”; `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:80–104`; `app/chamados/services/materializacao.py:1036–1090`; `parei_codigo_e_acrescimo.txt`, “o teste usa apoio que so existe na copia”.)
2. **Colisão.** A parada é compatível com o 1700 item 2, “FALTA que pede codigo: PAREI com a lista ANTES do opus”. “Para a cura entrar na mesma rodada” colide com “so .py de TESTE” e “nao cura por conta propria” do próprio 1845 **se** a rodada for a única volta já autorizada. A sequência também precisa respeitar o 1700, “PROIBIDO [...] mudar .py da copia entre o veredito da suite e o pouso”. Não achei aval anterior que autorize uma cura de produção para esses casos de ausência rejeitada. O 1255 e o 1320 nomeiam o controle de ausência aprovada. (`respostas_2011_1845.txt`, item 1 e ramo “NAO reproduz”; `avais_vigentes.txt`, 1255 item 2, 1320 item 2 e 1700 item 2.) [nao verificado]
3. **Literal.** “NAO reproduz algum” deixa sem critério a diferença entre uma execução válida com resultado diferente e uma execução inconclusiva por fixture, apoio ou instrumento. O RELATO já registra “4 erros de INSTRUMENTO” na primeira rodada. “Antes da suite do item 1” estabelece o prazo de parada, mas o despacho que produz o veredito do main está na parte ausente. “Mesma rodada” carece de referente inequívoco entre a volta do construtor e a rodada de certificação. (`respostas_2011_1845.txt`, lacuna, item 1 e ramo “NAO reproduz”; `parei_codigo_e_acrescimo.txt`, “4 erros de INSTRUMENTO”.) [nao verificado]
4. **Alcance.** O `PAREI` com o caso é o ato expresso. O alcance de uma cura futura é [nao verificado]. A porta relacionada reabre perguntas e disputas, acorda células e grava trilha, sem checagem explícita de K6 no corpo da devolução. Assim, a restrição “Caso de (b) em competencia exportada = lista no topo, nada se toca” tem efeito prático sobre esse ramo. Não há autorização recebida para alterar código protegido ou gravado de exportada. (`respostas_2011_1845.txt`, ramo “NAO reproduz” e linha de exportada; `app/chamados/services/materializacao.py:1187–1222`; `app/chamados/services/disputa_emissao.py:1682–1685`; `app/chamados/models.py:3529–3539`; `CLAUDE.md:265–268,411–416`.)

## Item 3 — Ordem na sombra

1. **Premissas. CONFERE** que a árvore tem o contador A/B, que recebe um queryset, e um leitor da competência entregue pelo K6. A sombra tem trava compartilhada, com espera e timeout. O RELATO diz “sonda pronta em logs/sombra/e4c_f5_ab_entregue.py, espera a sombra” e descreve 6-A1 como “gravado, batidas, ausencias e TXT antes/depois na sombra”. O script externo, a medida 6-A1 e o estado da rodada da CADEIA são [nao verificado]. (`app/chamados/services/vigia_ato_x_coluna.py:8–24`; `app/chamados/services/vigia_fio.py:789–811`; `bin/sombra_trava.sh:15–38`; `parei_codigo_e_acrescimo.txt`, “INCOMPLETO”.)
2. **Colisão.** Não achei colisão de prioridade no conjunto lido. O 0545 item 4 já diz “sonda da espinha F5 tem a vez sobre o proximo braco de medida de outra obra; medida em curso nao se interrompe”. O 1845 especializa a vez diante da CADEIA e diz “Nao derrubar rodada em curso”. O 1355 item 1(i) e o 1415 item 1 tratam da vaga do construtor. O 1545 item 4 pede medidas da CADEIA na sombra, sem fixar prioridade sobre a F5. (`respostas_2011_1845.txt`, item 3; `avais_vigentes.txt`, 0545 item 4, 1355 item 1, 1415 item 1 e 1545 item 4.)
3. **Literal.** “Ponto 6-A1” tem a descrição parcial acima. Porta a medir, recorte, referência do antes e critério de comparação são [nao verificado]. “ANTES de qualquer rodada nova” fecha como ordem de lançamento. A trava mostrada garante exclusão, sem prioridade por obra ou reserva conjunta das duas medidas. Se a CADEIA já tiver outra chamada na espera, a garantia da ordem pelo mecanismo é [nao verificado]. (`respostas_2011_1845.txt`, item 3; `parei_codigo_e_acrescimo.txt`, “INCOMPLETO”; `bin/sombra.sh:452–461,483–493`; `bin/sombra_trava.sh:21–38`.)
4. **Alcance.** O A/B em entregue lê perguntas e atos de um recorte de competência exportada. O 6-A1 observa superfícies ligadas a dinheiro, inclusive gravado e TXT, no ambiente declarado de sombra. As operações efetivas do ensaio 6-A1 são [nao verificado]. O item de ordem não contém autorização para alterar gravado em produção, dinheiro, escala, vínculo ou arquivo protegido. (`app/chamados/services/vigia_ato_x_coluna.py:8–24`; `app/folha/export.py:556–574`; `bin/sombra.sh:25–29,42–44`; `parei_codigo_e_acrescimo.txt`, “gravado, batidas, ausencias e TXT antes/depois na sombra”; `CLAUDE.md:265–268,411–420`.)

| Item | Resultado | Achado em uma linha |
|---|---|---|
| 1 (78) | **PROCEDE** | Lacuna AST confirmada, mas o mutante citado preserva a atribuição que o selo já enumera e a remedição comum conflita com o 1440. (`app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:148–172,379–393,434–440`; `avais_vigentes.txt`, 1440 item 1.) |
| 1 (06) | **PROCEDE** | Falta a varredura e o literal não distingue violação da escrita que hoje passa, inclusive em função de produção cadastrada. (`app/chamados/tests/test_e4c_f5_selo_catraca.py:21–24,180–197,360–369`; `app/chamados/services/validacao.py:284–296`.) |
| 2 (b) | **PROCEDE** | A frequência é [nao verificado] e o literal deixa implícitos o vínculo a contar, o efeito da cobertura e a classificação de exportada. (`respostas_2011_1845.txt`, lacuna e (b); `app/chamados/models.py:4414–4425`; `app/ponto/turnos.py:2523–2532`; `app/ponto/janelas.py:75–89`.) |
| 2 — Main REPRODUZ | **PROCEDE** | A equivalência dos dois percursos no main é [nao verificado] e o encaminhamento ao opus não explicita a dependência do PRONTO. (`parei_codigo_e_acrescimo.txt`, “NAO SEI” e “INCOMPLETO”; `avais_vigentes.txt`, 1230 e 1700 item 2.) |
| 2 — Main NÃO reproduz | **PROCEDE** | O ramo não distingue falha de instrumento de diferença de comportamento e deixa ambígua a rodada em que entraria a cura. (`parei_codigo_e_acrescimo.txt`, “4 erros de INSTRUMENTO”; `respostas_2011_1845.txt`, lacuna, item 1 e ramo “NAO reproduz”.) |
| 3 | **PROCEDE** | A prioridade é compatível com o 0545, mas o protocolo de 6-A1 é [nao verificado] e a trava não garante precedência sobre chamadas já em espera. (`avais_vigentes.txt`, 0545 item 4; `parei_codigo_e_acrescimo.txt`, “INCOMPLETO”; `bin/sombra_trava.sh:21–38`.) |

## 20/11 18:40 · sessao 23943dbef309 · E4C-F5-CONSTROI · leitura · origem=Code

A sonda usa `competencia_do_fato(pergunta, empresa)` para selecionar o dia entregue pelo K6. Para responder, chama a própria view `api_disputa_responder` com o usuário vinculado à pergunta. A view verifica o vínculo e a vitalidade e chama a entrada que veta a absorção vigente. `app/chamados/services/materializacao.py:792–805` · `app/folha/export.py:556–594` · `app/api/urls.py:93` · `app/api/views.py:2607–2655` · `app/chamados/models.py:4593–4596` · `app/chamados/services/disputa_emissao.py:1485–1490`

O teto escolhido é **oito perguntas por escape, até 16 tentativas**. O censo conta todas as perguntas com `ATESTADO` ou `FALTA_DIA` oferecido em competência entregue. O ensaio prioriza perguntas sem carimbo e dias mais recentes. Uma pergunta que oferece os dois escapes pode ser exercitada duas vezes, cada vez a partir da mesma fotografia inicial. O teto limita as remontagens do TXT da empresa. Com `universo=0`, sai apenas o resumo `SEM_CANDIDATAS`, sem linhas de diferenças. Universo e resultados efetivos **[nao verificado]**. `app/chamados/services/materializacao.py:1125–1131` · `app/chamados/services/disputa_emissao.py:1449–1466` · `app/folha/export.py:654–679,711–746`

Corpo para `tenant_command shell --schema=juliani --settings=config.settings.sombra`. A sonda foi entregue em texto. Execução e identidade Git da cópia **[nao verificado]**. `bin/sonda_leitura.sh:232–241` · `app/config/settings/sombra.py:27–60`

```python
# E4C-F5 · SHA informado c8eb8eddf898116f0dc3d7f936f194020a809e36

import hashlib
import io
import json
import logging
import socket
import sys
from collections import Counter, defaultdict
from contextlib import ExitStack, closing, redirect_stderr, redirect_stdout
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.storage import FileSystemStorage
from django.db import connection, transaction
from django.db.models import F, Q, TextField
from django.db.models.functions import Cast
from django.urls import reverse
from django.utils import timezone
from rest_framework.test import APIRequestFactory, force_authenticate

from api.views import api_disputa_responder
from chamados.models import PerguntaDisputa
from chamados.services import disputa_emissao, materializacao as M
from colaboradores.mixins import tem_acao
from core import observ
from folha.export import competencia_entregue, txt_vigente_x_gravado
from folha.models import ExportacaoDominio
from ponto.models import Ausencia, Batida, FechamentoMensal
from ponto.services import ausencia as A


TETO = 8
ESCAPES = tuple(sorted(M.ESCAPES_QUE_VIRAM_AUSENCIA))

# app/api/views.py:2615–2655 e app/chamados/services/disputa_emissao.py:1446–1490
RECUSAS = {
    (404, "Colaborador nao encontrado."): "sem_vinculo",
    (404, "Pergunta nao encontrada ou ja regularizada."): "nao_encontrada_ou_nao_viva",
    (400, "Resposta obrigatoria."): "resposta_vazia",
    (400, "Selecione/preencha uma resposta."): "resposta_vazia",
    (400, "Opcao invalida."): "opcao_invalida",
    (400, "Hora deve ser HH:MM."): "hora_invalida",
    (400, "A pergunta já foi resolvida pela ausência declarada."): "absorcao_vigente",
}
REVERSOES = {"par_ato_coluna_divergente", "sem_grupo_recuperavel"}
MOTIVOS_IRMAS = REVERSOES | {
    "fora_da_releitura", "resposta_posterior_a_passagem", "ja_carimbada",
    PerguntaDisputa.RECUSA_FORA_DO_FATO, PerguntaDisputa.RECUSA_C6,
    PerguntaDisputa.RECUSA_PAR_ATO_COLUNA,
}


class Descartar(io.TextIOBase):
    def write(self, texto):
        return len(texto)


class GuardaSombra(RuntimeError):
    pass


class SemUsuarioAtivo(RuntimeError):
    pass


class SemDecisorDP(RuntimeError):
    pass


class EvidenciaInesperada(RuntimeError):
    pass


class RedeBloqueada(OSError):
    pass


class EscritaEmArquivoBloqueada(OSError):
    pass


def sha(valor):
    texto = json.dumps(valor, sort_keys=True, ensure_ascii=True, separators=(",", ":"))
    return hashlib.sha256(texto.encode("ascii")).hexdigest()


def assinatura(campos):
    por_campo = {nome: sha(valor) for nome, valor in campos.items()}
    return sha(por_campo), por_campo


def fotografar_linhas(qs):
    campos = [f for f in qs.model._meta.concrete_fields if not f.primary_key]
    nomes = [f.name for f in campos]
    # Cast no PostgreSQL preserva a precisão dos timestamps e distingue
    # NULL SQL de um valor JSON null.
    colunas = [Cast(F(f.attname), TextField()) for f in campos]
    return {
        pk: assinatura(dict(zip(nomes, valores)))
        for pk, *valores in qs.order_by("pk").values_list("pk", *colunas)
    }


def foto(colab, dia, mes, ano):
    empresa = colab.empresa
    vigente = competencia_entregue(empresa.pk, mes, ano, registro=True)
    txt = txt_vigente_x_gravado(empresa, mes, ano)
    criterio_dia = (
        Q(data=dia)
        | Q(data__lte=dia, data_fim__gte=dia)
        | Q(data__lte=dia, data_fim__isnull=True, tipo__in=Ausencia.TIPOS_EM_ABERTO)
    )
    return {
        "FechamentoMensal": fotografar_linhas(FechamentoMensal.objects.filter(
            colaborador_id=colab.pk, mes=mes, ano=ano,
        )),
        "Batida": fotografar_linhas(Batida.objects.filter(
            colaborador_id=colab.pk, timestamp__date=dia,
        )),
        "Ausencia": fotografar_linhas(Ausencia.objects.filter(
            criterio_dia, colaborador_id=colab.pk,
        )),
        "ExportacaoDominio": fotografar_linhas(ExportacaoDominio.objects.filter(
            pk=vigente.pk if vigente is not None else None,
        )),
        "TXT_do_gravado": {empresa.pk: assinatura({
            "hash_hoje": txt["hash_hoje"], "linhas_hoje": txt["linhas_hoje"],
        })},
    }


def diferencas(antes, depois):
    resultado = {}
    for modelo, a in antes.items():
        b = depois[modelo]
        novas = sorted(b.keys() - a.keys())
        removidas = sorted(a.keys() - b.keys())
        mudadas = [
            {"pk": pk, "campos": sorted(
                nome for nome in a[pk][1] if a[pk][1][nome] != b[pk][1][nome]
            )}
            for pk in sorted(a.keys() & b.keys()) if a[pk][0] != b[pk][0]
        ]
        resultado[modelo] = {
            "novas": {"n": len(novas), "pks": novas},
            "mudadas": {"n": len(mudadas), "linhas": mudadas},
            "removidas": {"n": len(removidas), "pks": removidas},
        }
    return resultado


def censo():
    por_escape = dict.fromkeys(ESCAPES, 0)
    selecionadas = {escape: [] for escape in ESCAPES}
    universo = sem_dia = 0
    memo = {}
    qs = (
        PerguntaDisputa.objects.filter(tipo_resposta__in=("hora", "escolha"))
        .select_related("disputa__colaborador__empresa")
        .only("tipo_resposta", "opcoes", "data_evento",
              "disputa__colaborador__empresa__dia_inicio_competencia")
        .order_by(F("materializada_em").asc(nulls_first=True), "-data_evento", "pk")
    )
    with closing(qs.iterator(chunk_size=2000)) as perguntas:
        for p in perguntas:
            opcoes = p.opcoes or []
            valores = [o.get("valor") if isinstance(o, dict) else o for o in opcoes]
            oferecidos = [e for e in ESCAPES if e in valores]
            if not oferecidos:
                continue
            empresa = p.disputa.colaborador.empresa
            chave = (empresa.pk, p.data_evento)
            if chave not in memo:
                memo[chave] = M.competencia_do_fato(p, empresa)
            comp = memo[chave]
            if comp is None:
                sem_dia += 1
                continue
            mes, ano, entregue = comp
            if not entregue:
                continue
            universo += 1
            for escape in oferecidos:
                por_escape[escape] += 1
                if len(selecionadas[escape]) < TETO:
                    selecionadas[escape].append((p.pk, escape, mes, ano))
    casos = [caso for escape in ESCAPES for caso in selecionadas[escape]]
    return universo, sem_dia, por_escape, casos


def tipos_de_falha(falhas):
    contagem = Counter(x for x in falhas if isinstance(x, str))
    excecoes = {id(x): x for x in falhas if isinstance(x, BaseException)}
    contagem.update(type(x).__name__ for x in excecoes.values())
    return dict(contagem)


def medir_fatia(r, nome, acao, falhas):
    inicio = len(falhas)
    try:
        dados = acao()
        if transaction.get_rollback():
            falhas.append("TransacaoMarcadaParaRollback")
    except Exception as erro:
        falhas.append(erro)
    erros = tipos_de_falha(falhas[inicio:])
    del falhas[inicio:]
    r[nome] = (
        {"status": "INCONCLUSIVO", "excecoes_por_tipo": erros}
        if erros else {"status": "CONCLUSIVO", **dados}
    )


def decisor_dp():
    usuarios = get_user_model().objects.filter(is_active=True).order_by("-is_superuser", "pk")
    with closing(usuarios.iterator(chunk_size=200)) as ativos:
        for usuario in ativos:
            if tem_acao(usuario, "editar_ausencias"):
                return usuario
    raise SemDecisorDP()


def medir_caso(caso, falhas):
    pk, escape, mes, ano = caso
    r = {
        "registro": "caso", "pergunta_pk": pk, "escape": escape,
        "tentou_porta": 0, "tentou_rejeicao": 0,
        "passagem": {"status": "NAO_EXERCITADA"},
        "rejeicao": {"status": "NAO_EXERCITADA"},
    }
    ausencias = []
    recusas_irmas = defaultdict(set)
    criar_real, evento_real = A.criar_ausencia, observ.evento

    def criar_observada(**kw):
        chamada_da_passagem = (
            sys._getframe(1).f_code is M._materializar_ausencia_de_resposta.__code__
        )
        ausencia = criar_real(**kw)
        if chamada_da_passagem:
            ausencias.append(ausencia.pk)
        return ausencia

    def evento_observado(acao, modelo, objeto_id, **kw):
        if acao == M.ACAO_IRMA_VIVA_NA_AUSENCIA and modelo == "PerguntaDisputa":
            dados = kw.get("depois") or {}
            if dados.get("escape_pk") == pk:
                motivo = dados.get("motivo")
                if motivo in MOTIVOS_IRMAS:
                    recusas_irmas[motivo].add(objeto_id)
                    if motivo in REVERSOES:
                        falhas.append("_Reprovada")
                else:
                    falhas.append("MotivoDeRecusaInesperado")
        return evento_real(acao, modelo, objeto_id, **kw)

    def passagem():
        nonlocal colab, dia, depois
        p = PerguntaDisputa.objects.select_related(
            "disputa__colaborador__empresa", "disputa__colaborador__usuario",
        ).get(pk=pk)
        colab, dia = p.disputa.colaborador, p.data_evento
        usuario = colab.usuario
        if usuario is None or not usuario.is_active:
            raise SemUsuarioAtivo()
        r.update(colaborador_pk=colab.pk, empresa_pk=colab.empresa_id)
        antes = foto(colab, dia, mes, ano)
        req = APIRequestFactory().post(
            reverse("api:disputa_responder", kwargs={"pk": pk}),
            {"resposta": escape},
            format="json",
        )
        force_authenticate(req, user=usuario)
        with patch.object(A, "criar_ausencia", criar_observada), \
                patch.object(observ, "evento", evento_observado):
            r["tentou_porta"] = 1
            resposta = api_disputa_responder(req, pk=pk)

        if resposta.status_code == 200 and resposta.data.get("status") == "ok":
            r["porta"] = "ACEITA"
            if len(ausencias) != 1 or ausencias[0] is None:
                raise EvidenciaInesperada()
            r["ausencia_pk"] = ausencias[0]
        else:
            motivo = RECUSAS.get((resposta.status_code, resposta.data.get("erro")))
            if motivo is None:
                raise EvidenciaInesperada()
            r.update(porta="RECUSADA", motivo_recusa=motivo)

        depois = foto(colab, dia, mes, ano)
        return {"diferencas": diferencas(antes, depois)}

    def rejeicao():
        ausencia = Ausencia.objects.get(pk=r["ausencia_pk"])
        dp = decisor_dp()
        r["tentou_rejeicao"] = 1
        A.decidir(ausencia, "rejeitada", por=dp, decisor=dp, aprovada_em=timezone.now())
        ausencia.refresh_from_db()
        if ausencia.status != "rejeitada":
            raise EvidenciaInesperada()
        # Corpo real do primeiro ramo do callback de ponto/signals.py:254–262.
        devolvidas = M.devolver_absorvidas_da_ausencia_rejeitada(ausencia)
        return {
            "perguntas_devolvidas": devolvidas,
            "diferencas": diferencas(depois, foto(colab, dia, mes, ano)),
        }

    colab = dia = depois = None
    try:
        with transaction.atomic():
            try:
                medir_fatia(r, "passagem", passagem, falhas)
                if r["passagem"]["status"] == "CONCLUSIVO" and r.get("porta") == "ACEITA":
                    medir_fatia(r, "rejeicao", rejeicao, falhas)
            finally:
                transaction.set_rollback(True)
    except Exception as erro:
        r["passagem"] = {"status": "INCONCLUSIVO"}
        r["rejeicao"] = {"status": "INCONCLUSIVO"}
        r["savepoint"] = {"excecoes_por_tipo": tipos_de_falha([erro])}

    r["recusas_de_irmas"] = {
        motivo: {"n": len(pks), "pks": sorted(pks)}
        for motivo, pks in sorted(recusas_irmas.items())
    }
    r["status"] = (
        "INCONCLUSIVO"
        if any(r[f]["status"] == "INCONCLUSIVO" for f in ("passagem", "rejeicao"))
        else r["porta"]
    )
    return r


def principal():
    saida = sys.stdout
    universo = None
    falhas = []

    def observar_log(_logger, registro):
        erro = registro.exc_info[1] if registro.exc_info else None
        if erro is not None:
            falhas.append(erro)
        elif registro.levelno >= logging.ERROR:
            falhas.append("ErroRegistradoSemTipo")

    def bloquear(tipo):
        def chamada(*args, **kw):
            erro = tipo()
            falhas.append(erro)
            raise erro
        return chamada

    try:
        with ExitStack() as pilha:
            pilha.enter_context(redirect_stdout(Descartar()))
            pilha.enter_context(redirect_stderr(Descartar()))
            pilha.enter_context(patch.object(logging.Logger, "handle", observar_log))
            pilha.enter_context(timezone.override(settings.TIME_ZONE))
            if not (
                settings.SETTINGS_MODULE == "config.settings.sombra"
                and getattr(settings, "HASNER_SOMBRA", False) is True
                and set(settings.DATABASES) == {"default"}
                and connection.vendor == "postgresql"
                and connection.settings_dict["NAME"] == "sombra"
                and connection.settings_dict["HOST"] == "sombra_db"
                and getattr(connection, "schema_name", None) == "juliani"
                and not connection.in_atomic_block
                and connection.get_autocommit()
            ):
                raise GuardaSombra()

            # get_autocommit() já abriu a conexão do banco.
            pilha.enter_context(disputa_emissao.push_disputa_silenciado()) [nao verificado]
            bloquear_rede = bloquear(RedeBloqueada)
            pilha.enter_context(patch.object(socket, "getaddrinfo", bloquear_rede))
            for metodo in ("connect", "connect_ex", "sendto", "sendmsg"):
                if hasattr(socket.socket, metodo):
                    pilha.enter_context(patch.object(socket.socket, metodo, bloquear_rede))
            for metodo in ("_save", "delete"):
                pilha.enter_context(patch.object(
                    FileSystemStorage, metodo, bloquear(EscritaEmArquivoBloqueada),
                ))

            with transaction.atomic():
                try:
                    with connection.cursor() as cursor:
                        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ WRITE")
                        cursor.execute("SELECT pg_catalog.current_database(), pg_catalog.current_schema()")
                        if cursor.fetchone() != ("sombra", "juliani"):
                            raise GuardaSombra()
                    universo, sem_dia, por_escape, casos = censo()
                    if falhas:
                        raise EvidenciaInesperada()
                    linhas = [medir_caso(caso, falhas) for caso in casos]
                    if falhas:
                        raise EvidenciaInesperada()
                finally:
                    transaction.set_rollback(True)

        erros = Counter()
        for r in linhas:
            for etapa in ("passagem", "rejeicao", "savepoint"):
                erros.update(r.get(etapa, {}).get("excecoes_por_tipo", {}))
        recusas = Counter(r["motivo_recusa"] for r in linhas if r.get("porta") == "RECUSADA")
        inconclusivos = sum(r["status"] == "INCONCLUSIVO" for r in linhas)
        passagens = sum(
            r.get("porta") == "ACEITA" and r["passagem"]["status"] == "CONCLUSIVO"
            for r in linhas
        )
        if universo == 0:
            status = "SEM_CANDIDATAS"
        elif inconclusivos:
            status = "INCONCLUSIVO"
        elif passagens == 0:
            status = "SEM_PASSAGEM_ACEITA"
        else:
            status = "CONCLUSIVO"
        resumo = {
            "registro": "resumo", "status": status, "universo": universo,
            "candidatas_por_escape": por_escape, "com_escape_sem_dia": sem_dia,
            "planejadas": len(casos),
            "tentativas": sum(r["tentou_porta"] for r in linhas),
            "exercitadas": len({r["pergunta_pk"] for r in linhas if r["tentou_porta"]}),
            "recusadas": len({r["pergunta_pk"] for r in linhas if r.get("porta") == "RECUSADA"}),
            "tentativas_recusadas_por_motivo": dict(recusas),
            "passagens_conclusivas": passagens,
            "rejeicoes_tentadas": sum(r["tentou_rejeicao"] for r in linhas),
            "rejeicoes_conclusivas": sum(r["rejeicao"]["status"] == "CONCLUSIVO" for r in linhas),
            "casos_inconclusivos": inconclusivos, "excecoes_por_tipo": dict(erros),
        }
    except Exception as erro:
        linhas = []
        resumo = {
            "registro": "resumo", "status": "INCONCLUSIVO", "universo": universo,
            "excecoes_por_tipo": tipos_de_falha([*falhas, erro]),
        }

    for linha in [resumo, *linhas]:
        print(json.dumps(linha, ensure_ascii=True, sort_keys=True), file=saida, flush=True)
    return int(resumo["status"] == "INCONCLUSIVO")


raise SystemExit(principal())
```

### Leitura das duas fatias e limites

- **Fotos.** Passagem compara antes e depois da view. Rejeição compara o pós-resposta com o resultado de `decidir` seguido do corpo real de `devolver_absorvidas_da_ausencia_rejeitada`. O decisor é o primeiro usuário ativo com a ação exigida pela view do DP, priorizando superusuários. A rejeição exercita uma única ausência, inclusive uma já aprovada se a passagem a reutilizar. A rejeição pela tela tem recorte próprio de ausências abertas e percorre as irmãs de documento. `app/ponto/views.py:2828–2852` · `app/colaboradores/mixins.py:322–331` · `app/ponto/services/ausencia.py:436–443,914–923,1057–1125` · `app/chamados/services/materializacao.py:1187–1222`
- **Chave e valor.** O pareamento usa PK e os hashes ficam em memória. As fotos cobrem todos os campos concretos do fechamento do colaborador na competência, as batidas do dia civil, as ausências cujo intervalo inclui o dia em qualquer estado e o registro vigente pelo K6. “Nova” e “removida” significam entrada e saída da foto. Uma troca de vigente, por exemplo, aparece com o PK antigo removido e o novo incluído. `TXT_do_gravado` é a remontagem da empresa com o gerador da casa, identificada pelo PK da empresa. Uma mudança ali pode vir das datas de falta mesmo com o registro entregue intacto. `app/ponto/models.py:22–42,398–485,826–841` · `app/ponto/turnos.py:2550–2571` · `app/folha/models.py:75–102` · `app/folha/export.py:588–594,654–679,725–746`
- **`on_commit`.** O rollback descarta a fila. A chamada explícita

[truncado em 20000 de 22555 caracteres]

## 20/11 18:39 · sessao 519e39149137 · E4C-F5-CONSTROI · leitura · origem=Code

**E4C-F5-CONSTROI · cópia `c8eb8eddf`**

A auditoria cobre as 29 faltas de publicação, incluindo a parte de publicação do 06. O 78 está marcado apenas como `FALTA-CODIGO` e fica fora da contagem. Os estados abaixo usam os dois anexos vivos. As tabelas seguintes usam o código da cópia e indicam onde falta o histórico. `faltas.txt:3–32`

## Estado da publicação

| Nº | O que o aval pede que esteja publicado | O que os anexos mostram | Estado | [nao verificado]
|---|---|---|---|
| 01 | Matriz nominal dos 15 sítios conciliada com censo, selo e PRONTO. `faltas.txt:3` | «matriz 15 sitios» e «CENSO fechado pelo wrapper» em `relato_a.txt:1410`. «Sao 11 das 15 linhas da matriz» em `relato_a.txt:694`. Falta a matriz nominal com o vínculo de cada sítio ao censo, ao selo e ao PRONTO. | PUBLICADO EM PARTE | [nao verificado]
| 06 | Mapa dos 15 no RELATO, com a cobertura da catraca. O literal do 0815 citado no relato é «varrendo tambem string em getattr/update_fields/refresh_from_db». `faltas.txt:4` · `relato_a.txt:3` | A linha mais recente descreve a inspeção e registra «13 funcoes em 8 arquivos, 0 acusacoes», mas termina em «MUDOU DE CLA». O mapa ainda aparecia como pendência em `relato_a.txt:74`. O corte da linha mais recente impede fechar a situação da publicação. `relato_a.txt:3` | CORTADO |
| 19 | Saída do grep do compositor e das três delegações. `faltas.txt:5` | «PUBLICAR NO RELATO: 19 grep do compositor» em `relato_a.txt:74`. A fonte e os três consumidores são citados, mas a saída do grep está [nao verificado] nos recortes. `relato_a.txt:1227,1275,1287` | AUSENTE |
| 32 | Triagem motivada de `casefold` para anexar ao Opus final. `faltas.txt:6` | «casefold sobrevivente = plausivel, nao executado -> anexo ao opus» em `relato_a.txt:1346`. «Terra NAO BLOQUEIA, Sol BLOQUEIA» em `relato_a.txt:1281` e «Sol 6.2 BLOQUEIA» em `relato_a.txt:1284`. Falta a decisão final motivada diante dos pareceres divergentes e a triagem anexada. | PUBLICADO EM PARTE | [nao verificado]
| 33 | Linha «PRONTO E4C-F5 item a item» e lista atual de faltas no topo. `faltas.txt:7` | «INCOMPLETO: […] linha do PRONTO» em `relato_a.txt:5,11,13`. A leitura posterior traz «113 itens […] 30 FALTA», detalha dois itens e termina em «MUDOU DE CLA». O corte impede ver se a linha atual completa a lista e traz o literal pedido. `relato_a.txt:3` | CORTADO |
| 35 | Mapa dos 69 K com classes, carregadores e pontos de toque. `faltas.txt:8` | «69 carregadores a tocar» e «L/Q/R/M = 10/29/5/25» em `relato_a.txt:1245,1239`. Falta a correspondência nominal K → classe → carregador → ponto de toque. | PUBLICADO EM PARTE | [nao verificado]
| 36 | Declaração dos seis R e vínculo nominal dos cinco além de K40. `faltas.txt:9` | «K40 virou R no 1830» em `relato_a.txt:695` e cinco R no agregado «L/Q/R/M = 10/29/5/25» em `relato_a.txt:1239`. Faltam a declaração conjunta dos seis e os cinco vínculos nominais. | PUBLICADO EM PARTE | [nao verificado]
| 40 | Tabela B com 29 cobertos, 27 editados na B, K41/K42 e 5 L + 3 Q. `faltas.txt:10` | «K41/K42 sao da B» e «VOLTA B (29 x 27)» aparecem na devolução do aval. «lotes 24->27» se refere aos asserts da B2. Faltam a conciliação nominal dos 29 e dos 27, os carregadores K41/K42 e os nomes dos 5 L + 3 Q. `relato_a.txt:1236,1203` | PUBLICADO EM PARTE | [nao verificado]
| 41 | Tabela C com manutenção nominal, as três M e a sobra. `faltas.txt:11` | «25 K + sobra dedup» e «manutencao: auditar x3, dedup, materializacao x5, vigia_fio = 10». Faltam o vínculo nominal dos 25 K e a identificação das três M. `relato_a.txt:1196,1213` | PUBLICADO EM PARTE | [nao verificado]
| 45 | Tabela consolidada da remedição e da atribuição SQL dos selos. `faltas.txt:12` | «230->234, 145->146, 294->298, 219->221» e «ESCRITOR = 0» na remedição C4. O detalhe «A […] B […] C […] D […] E […] F […] G» é da comparação anterior. Falta a atribuição nominal final por cenário na tabela consolidada. `relato_a.txt:700,1206` | PUBLICADO EM PARTE | [nao verificado]
| 49 | Mapa das 11 famílias e diff por exceção autorizada. `faltas.txt:13` | «harness 12 familias / 112 ocorrencias», «tocar harness das 11 familias» e «duble helper unico […] nos 9 modulos». Há usos de exceções em prosa. Faltam a partição nominal das 11 e o diff completo vinculado a cada autorização. `relato_a.txt:1245,1221,1211–1216` | PUBLICADO EM PARTE | [nao verificado]
| 51 | Identificação da 12ª família e dos dois RED-8. `faltas.txt:14` | «restam […] red_8 2» e «calc.call_count>=N -> consultas […] <=1». Faltam a identificação expressa da 12ª família e a publicação dos dois nomes vinculados a ela. `relato_a.txt:1219,1216,1245` | PUBLICADO EM PARTE | [nao verificado]
| 55 | Conciliação histórica dos 41 diretos e 55 indiretos por volta. `faltas.txt:15` | «41 -> esperado 24», «24 -> 10 -> 0» e «os 55 indiretos trocam em _pergunta_viva_calc na A». Falta a conciliação nominal das identidades e dos marcos por volta. `relato_a.txt:1224,1213,1221` | PUBLICADO EM PARTE | [nao verificado]
| 68 | Inventário das adaptações, com antes/depois e aval por uso. `faltas.txt:16` | «USOS DA CLASSE» com deltas pontuais e «33 arquivos de teste EXISTENTES modificados (271+/142-)» numa medição intermediária. Falta o inventário integral da cópia final com o antes/depois e o aval de cada uso. `relato_a.txt:1211–1212,1216,690` | PUBLICADO EM PARTE | [nao verificado]
| 70 | Fila E4D do raio-X. `faltas.txt:17` | «placar chamados/services/raiox.py:135 (values_list da coluna, face SQL) le a projecao, fora da F5» em `backlog_vivo.txt:2047`. «raiox registrado na E4D-PROJECAO-UM-ESCRITOR» em `relato_a.txt:1233`. | PUBLICADO |
| 72 | Fila E4D de `resposta_display`. `faltas.txt:18` | «resposta_display […] fica FORA da F5 (medida 20/11: literal 16, like-for-like 0)» em `backlog_vivo.txt:2047`. «resposta_display registrada na E4D-PROJECAO-UM-ESCRITOR» em `relato_a.txt:690`. | PUBLICADO | [nao verificado]
| 80 | Registro dos esperados substituídos em K47 e na D. `faltas.txt:20` | «esperado antigo dos 18 testes citado no corpo do commit» e «zero DEVOLUCAO em A) se INVERTE». Falta o registro nominal dos esperados antigos e novos de K47 e dos REDs da D. `relato_a.txt:190,669` | PUBLICADO EM PARTE | [nao verificado]
| 85 | Fila E4D da opção B de pureza e dos nove harnesses. `faltas.txt:21` | «VIAS_FORA_DO_FATO vira constante de modulo […] 9 harnesses que injetam a classe falsa mudam; a isencao nominal com tripwire […] sai junto». `backlog_vivo.txt:2048` | PUBLICADO | [nao verificado]
| 93 | Fila E4D da corrida entre irmãs, grupo via ABSORÇÃO, âncora fora e descarte das recusadas. `faltas.txt:22` | «recusa entre irmas da ausencia + leitor do grupo para a linha ABSORCAO» e «ancora fora, dependente sem reentrada CONTADA […] o unico chamador de producao descarta as recusadas». `backlog_vivo.txt:2055–2056` · `relato_a.txt:80` | PUBLICADO | [nao verificado]
| 94 | Fila E4D do empate automático e das três janelas de elegibilidade. `faltas.txt:23` | «responder_automaticamente em empate; elegibilidade antes da trava em validar, recusar, corrigir». `backlog_vivo.txt:2055` | PUBLICADO | [nao verificado]
| 95 | Fila E4D de validação física e backfill. `faltas.txt:24` | «todo escritor que carimba validar grava a linha VALIDACAO fisica, junto do backfill das validadas sem linha». `backlog_vivo.txt:1978` | PUBLICADO | [nao verificado]
| 101 | Declarar a redução de S1, registrar a fatia e encaminhar as quatro perguntas. `faltas.txt:25` | «1 retirado por aval (S1)» no topo, «o SILENCIO dono admin SAI da F5 e vira a fatia E4C-F5-SILENCIO-ADMIN» e «As quatro perguntas […] vao ao caderno de cortes». O BACKLOG confirma «4 perguntas NO CADERNO 12:5x». `relato_a.txt:3,181,81` · `backlog_vivo.txt:1966` | PUBLICADO | [nao verificado]
| 102 | Censo DECIDE/EXIBE e `N_DECIDE` no topo. `faltas.txt:26` | «N_DECIDE = 42, N_EXIBE = 1», «3 dos 42 sao DECIDE POR DUVIDA» e «Mais 2 EXIBE por leitura direta». A remissão declara «ver a PERGUNTA DE DESENHO F5-INSTANTE no topo». Os totais e a ressalva estão antes do corte. `relato_a.txt:56,64` | PUBLICADO | [nao verificado]
| 103 | Literal do limite de `validada_em` na linha do PRONTO. `faltas.txt:27` | «a linha do PRONTO declara o limite 'guarda SO validada_em; instante, autor e texto fora'» e «A/B mede SO presenca de validada_em». Falta o literal na própria linha do PRONTO. `relato_a.txt:55,73,13` | PUBLICADO EM PARTE | [nao verificado]
| 105 | Entrada `E4C-F5-INSTANTE` com dependência, insumos, saída e A/B em `ALARMA_SEMPRE` na fatia. `faltas.txt:28` | «vaga apos o pouso da F5, antes da E4d; entrada 42 consumidores […], 3 formas da Q0, achado 2a do verif2, 16.886 e 71; saida comparador ato x coluna, contador 0, selo; ato_x_coluna_a/_b em ALARMA_SEMPRE». `backlog_vivo.txt:2053` · `relato_a.txt:55` | PUBLICADO | [nao verificado]
| 110 | Entrada E4D dos escritores H4 fora da trava e respectivo anexo ao Opus. `faltas.txt:29` | «escritor de fantasma […] e encerrar_por_fato_no_espelho gravam carimbo/via fora da trava do dono» e «vai anexo ao opus». A entrada está publicada. Falta o anexo efetivamente entregue ao Opus. `backlog_vivo.txt:2057` · `relato_a.txt:44,51` | PUBLICADO EM PARTE | [nao verificado]
| 111 | Literal de S144 na linha do PRONTO. `faltas.txt:30` | «PRONTO do sitio = RED afirma a recusa, o motivo na trilha e o PK encontravel pelo contador do par». A decisão e a prova estão registradas. Falta o literal na própria linha do PRONTO. `relato_a.txt:52,19,17` | PUBLICADO EM PARTE | [nao verificado]
| 112 | Dois seletores, seus mutantes e literal do plantio na linha do PRONTO. `faltas.txt:31` | «os seletores (reconciliador.py:678-681 e :1100-1103) exigem validada_em nula», «A2 celula e orfa sem filtro de coluna, plantio motivo c6» e «RED so pelo passivo». Falta reunir os seletores, os mutantes e o literal aprovado do plantio na linha do PRONTO. A redação literal do aval está [nao verificado]. `relato_a.txt:23,17` | PUBLICADO EM PARTE | [nao verificado]
| 113 | Fila da reprodução pela tela do controle H4. `faltas.txt:32` | «item 1 como linha de fila (F5-H4-CAMINHO-PELA-TELA)» e «o caminho inteiro pela tela ainda nao foi reproduzido». O RELATO dá nome e escopo à fila. Falta ver a entrada no BACKLOG indicada pelo item, que está [nao verificado] no recorte fornecido. `relato_a.txt:44,50` | PUBLICADO EM PARTE | [nao verificado]

## 01 e 06 · Matriz dos 15 sítios e cobertura da catraca

A reconstrução cruza o registro da catraca com os dois juízes puros e com o RED-5 histórico, acrescentado à matriz inicial. `par_do_intervalo` ocupa dois sítios e `validar_lote_classe_a` ocupa um sítio com duas guardas. Os IDs e a ordem do `brief_c3` estão [nao verificado]. `app/chamados/tests/test_e4c_f5_selo_catraca.py:11–26` · `app/chamados/tests/e4c_f5_voltaD_mutantes.py:20–37` · `relato_a.txt:1297,1410`

| Sítio | Leitura na cópia | Selo ou prova vinculável |
|---|---|---|
| `espera_o_texto_do_colaborador` | Instante pela composta, texto corrente em `respondida_sem_texto`. `app/chamados/services/fio.py:178–190` | Catraca. RED em `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–65`. |
| `leitura_da_resposta` | Guarda de texto corrente e delegação a `classificar_resposta_validacao`. `app/chamados/templatetags/chamados_tags.py:145,166–171` | Catraca. Passagem da tag registrada em `relato_a.txt:1409–1410`. |
| `projecao_temporal_da_pergunta` | Juiz recebe a leitura e aplica o horizonte. `app/chamados/juizes.py:1383,1391–1409` | Selo de chamada. RED em `app/chamados/tests/test_e4c_f5_volta2_juizes.py:77–92`. |
| `pergunta_respondida` | Juiz de instante e texto sobre o objeto recebido. `app/chamados/juizes.py:1551–1560` | Selo de chamada. RED em `app/chamados/tests/test_e4c_f5_volta2_juizes.py:94–105`. |
| `acao_corrigir_resposta` | Guarda de validação pela composta. `app/chamados/services/acoes_disputa.py:349` | Catraca. RED em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:84`. |
| `_resposta_validada` | Guarda comum às duas declarações, pela composta. `app/chamados/services/declaracao_texto.py:217–229` | Catraca. REDs em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:103,122`. |
| `_resolver_perguntas_por_chamado_resolvido`, S143 sem credor | Validação pela composta após carga de decisões. `app/chamados/services/disputa_emissao.py:2107–2112` | Catraca. REDs em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:142,155`. |
| `par_do_intervalo`, irmã exigida validada | Primeiro ramo consulta a composta da irmã. `app/chamados/services/materializacao.py:200–202` | Catraca no corpo comum. RED em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:228`. |
| `par_do_intervalo`, irmã implausível | Segundo ramo consulta a mesma composta. `app/chamados/services/materializacao.py:203–208` | Catraca no corpo comum. RED em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:238`. |
| `materializar_perguntas_validadas_da_disputa` | Seleção inicial SQL e guarda composta sob trava. `app/chamados/services/materializacao.py:2142–2163` | Catraca. RED em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:248`. |
| `_validar_o_par` | Guarda composta da outra metade. `app/chamados/services/validacao.py:656–657` | Catraca. RED em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:272`. |
| `validar_pergunta` | Instante, texto e validação pela composta. `app/chamados/services/validacao.py:698–704` | Catraca. RED em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:300`. |
| `recusar_pergunta` | Guarda pela composta e coluna na foto. `app/chamados/services/validacao.py:915–923,930–932` | Catraca. RED em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:312`. |
| `validar_lote_classe_a` | Guardas compostas na entrada e na releitura. `app/chamados/services/validacao.py:1074–1084,1102–1105` | Catraca. REDs em `app/chamados/tests/test_e4c_f5_voltaD_portas.py:324,350`. |
| `devolver_validacao_recusada` | Guarda por ato ou coluna e escrita da própria coluna. `app/chamados/services/validacao.py:269–293` | Catraca. REDs em `app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:69,91,114`. |

| Conciliação | Evidência |
|---|---|
| Registro atual | 13 funções em oito arquivos. Inclui `_eventos_dos_chamados`, o RED-5 adicional à matriz inicial. `app/chamados/tests/test_e4c_f5_selo_catraca.py:15–26` · `app/chamados/services/historico.py:96–111` · `relato_a.txt:1297,1410` |
| Juízes puros | O selo de chamada confere a origem composta pelo chamador. `app/chamados/tests/test_e4c_f5_selo_catraca.py:12–14` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–149,188–193` |
| Strings na catraca | O visitante trata `getattr` e `refresh_from_db(fields=...)`. `update_fields` aparece no exemplo que passa. O visitante não inspeciona nominalmente esse argumento. `app/chamados/tests/test_e4c_f5_selo_catraca.py:174–197,360–369` |
| Censo e PRONTO históricos | A matriz original é citada como `logs/ESPINHA-BRIEFS/F5/brief_c3.md`. O vínculo à numeração original está [nao verificado]. `relato_a.txt:74,694,1410` |

## 19 e 32 · Compositor, delegações e `casefold`

| Trecho do código | Ocorrência |
|---|---|
| Compositor | `def composta(cls, p)` → `lido = cls.leitura(p)` → `return LeituraComposta(lido.validada_em, lido.respondida_em, p.resposta_colab, p.materializada_em, p.via_resolucao)`. `app/chamados/models.py:4382–4390` |
| Pergunta no app | `_pergunta_respondida(DecisaoPergunta.composta(p))`. `app/chamados/services/pergunta_no_app.py:83–88` |
| Property `respondida` | `return pergunta_respondida(DecisaoPergunta.composta(self))`. `app/chamados/models.py:3837–3844` |
| Fio | `leitura = DecisaoPergunta.composta(p)` → `respondida_sem_texto(leitura)`. `app/chamados/services/fio.py:187–190` |
| Outros dois sítios do modelo enumerados com a property no teste do Opus | `sem_veredito_entre` e `estado` também passam a composta ao juiz. `app/chamados/tests/test_e4c_f5_opus_models.py:1–6,75–89` · `app/chamados/models.py:2022–2026,3256–3268` |

Para a triagem do 32, a comparação atual é literal. O caso `Ato` × `ato` exige uma divergência e acusa uma normalização que iguale esse par. Os casos com espaço variam o texto da projeção. O ponto exato do mutante histórico `casefold` e o resultado dele nesta cópia estão [nao verificado]. `app/chamados/services/vigia_texto_resposta.py:19–25` · `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–53` · `relato_a.txt:1281,1284,1346`

## 35, 36, 40 e 41 · Mapa dos K e cobertura das voltas

Há 33 K com carregador identificável pelos testes da cópia e mais K40 pelo cruzamento com o RELATO. A coluna **Volta** indica a fonte que vincula o K à volta. As classes L/Q/R/M por K estão [nao verificado], salvo o vínculo de K40 a R. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–18` · `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–26` · `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–22` · `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264` · `relato_a.txt:695,1232,1236,1239`

| K | Volta ou classe | Carregador e ponto de toque | Vínculo nominal |
|---|---|---|---|
| K1 | A | `_carga_da_tarefa_cls`. `app/chamados/services/tarefa_do_dia.py:533–545` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:10` |
| K2 | A | `alimentar_portas`. `app/chamados/juizes.py:1664–1669` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:11` |
| K9 | A | `reconciliar_perguntas_orfas_colab`. `app/chamados/reconciliador.py:1575` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:12` |
| K10 | A | `_orfas_abertas`. `app/chamados/reconciliador.py:1100–1103` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:13` |
| K16 | A | Resolução por chamado, ramo S128. `app/chamados/services/disputa_emissao.py:1978–1982` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:14,37–39` |
| K17 | A | `universo_do_fato`. `app/chamados/services/fio.py:1963–1964` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:15` |
| K18 | A | `_vizinhos_do_fato`. `app/chamados/services/fio.py:1595–1597` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:16` |
| K24 | A | `chamados_que_falam_de_batida`. `app/ponto/services/cartorio.py:145–146` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:17` |
| K43 | A | `perguntas_absolvidas_pela_celula`. `app/chamados/reconciliador.py:702–703` | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:18` |
| K4 | B | `perguntas_vivas_de`. `app/chamados/juizes.py:1512` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:54–60` |
| K11 | B | `fechar_beco_da_disputa`. `app/chamados/services/becos.py:86–87` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:62–69` |
| K23 | B | `montar_previa`. `app/folha/previa.py:64–66` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:71–95` |
| K26 | B | `insumos_em_regularizacao`. `app/chamados/services/declaracao_texto.py:589–590` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:97–115` |
| K29 | B | `perguntas_corrigiveis`. `app/chamados/juizes.py:1765` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:117–129` |
| K30 | B | `perguntas_respondidas_a_validar`. `app/chamados/juizes.py:1772–1773` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:131–137` |
| K31 | B | `_dias_das_perguntas_vivas`. `app/chamados/juizes.py:2435–2437` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:139–146` |
| K32 | B | `disputa_viva_do_chamado`. `app/chamados/juizes.py:2490` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:148–155` |
| K41/K42 | B | Carregadores e pontos de toque [nao verificado]. | `relato_a.txt:1236` |
| K45 | B | `medir`, perguntas da competência. `app/chamados/services/competencia_trancada.py:119–121` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:165–177` |
| K46 | B | `medir`, perguntas do chamado. `app/chamados/services/competencia_trancada.py:165–166` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:179–195` |
| K47 | B | Resolução sem credor. `app/chamados/services/disputa_emissao.py:2107–2112` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222` |
| K54 | B | `contexto_calendario`. `app/colaboradores/services/calendario.py:504–505` | `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:224–233` |
| K55 | B | `dia_do_colab`. `app/ponto/services/dia_do_colab.py:64–65` | `app/chamados/t

[truncado em 20000 de 39123 caracteres]

## 20/11 18:38 · sessao a70008157aa2 · E4C-F5-CONSTROI · leitura · origem=Code

Usei os vereditos do `inventario_logs.txt` para classificar as execuções e as linhas numeradas do `relato_b.txt` para ligar cada rodada ao hash e ao escopo. Quando um item pede várias provas, o estado cobre a execução identificada na linha. As partes sem prova estão marcadas como `[nao verificado]`. As horas dos logs são as de modificação no inventário. A cópia está sem metadados Git, então a comparação direta entre hashes ficou `[nao verificado]`.

Os caminhos iniciados por `./` são relativos a `logs/ESPINHA-BRIEFS/F5`.

| Nº | Execução pedida | Evidência | Hash da execução | Estado |
|---:|---|---|---|---|
| 07 | Passagens A → B → C por hash, com a exceção do chokepoint na A. `bin/volta_intermediaria.sh:2–9` | A `./vA/volta/vi1.log` (19/11 22:56), `Ran 5582 tests in 851.720s`, `FAILED (failures=5, errors=7, expected failures=21)`. O aval aceitou a passagem da A com a exceção. B `./vB/volta.log` (20/11 00:18), `FAILED (failures=4, errors=4, expected failures=12)` e `./vB3/volta.log` (00:51), `FAILED (failures=4, expected failures=12)`. C3 `./vC3/volta.log` (02:16), `FAILED (failures=4, skipped=1, expected failures=10)`. C5 `./vC5/volta.log` (02:52), `Ran 6748 tests in 622.974s`, `OK (skipped=1, expected failures=10)`, `rc=0 tempo_s=896 fim=02:52`. `relato_b.txt:697,704,1196,1203,1210–1211` | A aceita em `ec7134368`. B inicial `d0210a400`. C3 `aa3bdd88c`. C5 `a3c23d10b`. SHA exato de `vi1` e B3 `[nao verificado]`. `relato_b.txt:697,704,1196,1203,1206,1210–1211` | **RODOU EM ARVORE ANTERIOR.** Houve alterações posteriores em `models.py` e `materializacao.py`. A passagem verde da B está `[nao verificado]`. `relato_b.txt:17,62` |
| 08 | Ordem dos commits, RED na base de cada cura e pureza no primeiro commit da A. `app/chamados/tests/test_e4c_f5_selo_chamada.py:271–285` | B `./vB/base_novos.log` → `./vB/B_novos.log` (19/11 23:57), `Ran 30 tests in 3.088s`, `FAILED (failures=28)` → `Ran 30 tests in 3.135s`, `OK`. C2 `./vC2/base_alvo.log` (20/11 01:47) → `./vC2/c2_verde.log` (01:48), `FAILED (failures=2, errors=15)` → `OK`. C3 `./vC3/base_red.log` (01:58) → `./vC3/c3_verde.log` (01:59), `FAILED (failures=3)` → `OK`. A1 tem commit declarado e ensaio separado `./vA/prova/puro_mods.log` (19/11 19:21), `FAILED (failures=15, errors=3)`. `relato_b.txt:705–706,1203,1219,1221` | A1 `0db6afc73`, com ensaio `PURO-SO` sobre `18bf86f01` + diff. B `ec7134368` → `d0210a400`. C2 `9d386d1b1` → `0e3b701a6`. C3 `0e3b701a6` + teste → `aa3bdd88c`. `relato_b.txt:705–706,1203,1219,1221` | **RODOU EM ARVORE ANTERIOR.** A pureza no próprio primeiro commit da A e a sequência completa de REDs `[nao verificado]`. Houve mudanças posteriores em `models.py` e `materializacao.py`. `relato_b.txt:17,62,1219,1221` |
| 09 | Verificador e triagem de cada delta A/B/C, incluindo a volta 3/c3. `app/docs/RELATO.md:94–95,131,134` | `./vE/volta3/verif_c3/triagem.txt` (20/11 16:11), `TRIAGEM verif_c3 (delta 05cb513bb..c08017000; resposta r.txt rc=0)` e `PROCEDENTES (curados em c8eb8eddf, so testes):`. O RELATO dá 3 procedentes e 7 descartados. Para C4+C5, declara `BLOQUEIA=5 NAO_BLOQUEIA=1`. Os `r.rc` isolados de B e C trazem `rc=0`. `relato_b.txt:7,697,1196` | `c08017000` para c3, curas em `c8eb8eddf`. Delta C4+C5 até `a3c23d10b`. `relato_b.txt:7,17,697,700` | **RODOU EM ARVORE ANTERIOR.** A triagem c3 registra mudanças posteriores em testes. Os laudos e as triagens completos de A/B/C `[nao verificado]`. `relato_b.txt:7,697` |
| 10 | 33 tira-lote A/B/C, incluindo K16 e K47. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–21`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23` | `./vPRONTO/medidas_da_sessao.txt` (20/11 02:54), `33 de 33 MORDE pela medida; k16 9 != 15; k47 8 != 14 (vC5/mutantes_c6.txt); Ran 76 OK`. `./vC5/green_c6.log` (02:53), `Ran 76 tests in 10.461s`, `OK`. O recorte de `./vC5/mutantes_c6.txt` (02:53) mostra `MORDE` para K17, K18, K24 e K43. Saída nominal íntegra `[nao verificado]`. `relato_b.txt:697` | C6 `8938552e1`. `relato_b.txt:697` | **RODOU EM ARVORE ANTERIOR.** Depois, `test_e4c_f5_voltaA_lotes.py` ganhou dois casos C11 e K47 ganhou a asserção de equivalência pela porta. `relato_b.txt:190,690` |
| 11 | Sombra antes × depois dos dois juízes, por volta, classe e passivo sem ato. `app/chamados/juizes.py:1383–1403,1551–1560` | A sonda anterior separou 213 casos de SILÊNCIO e deixou o subtotal de passivo em aberto. Outro recorte de 40 usou um contrafactual em memória. O comparativo pela F5 real, por volta, está `[nao verificado]`. `relato_b.txt:1649,1660,1757` | `[nao verificado]` para a comparação pedida. | **SEM EVIDENCIA.** |
| 12 | Sombra das decisões de validar, recusar e materializar pelas portas reais. `app/chamados/services/validacao.py:677–704,875–923`, `app/chamados/services/materializacao.py:1318–1336,2106–2162` | O RELATO ainda aponta a sombra por porta como lacuna. Saída comparativa `[nao verificado]`. `relato_b.txt:695` | `[nao verificado]` | **SEM EVIDENCIA.** |
| 13 | Lista ou zero de decisões alteradas em entregue/exportada. `app/chamados/services/validacao.py:225–231,692,757–759`, `app/chamados/services/materializacao.py:815–847,1900–1904` | O RELATO inclui entregue/dinheiro nas medidas pedidas. `./vE/volta3/medida_seis/medida.log` (20/11 18:28) tem cabeçalho no inventário. O resultado do recorte está `[nao verificado]`. `relato_b.txt:5` | `c8eb8eddf` consta no `arvore.txt` da rodada, às 18:28. Hash de um resultado do item `[nao verificado]`. | **SEM EVIDENCIA.** |
| 14 | CHECK-DE-TELA das quatro superfícies, com resultado, consultas e escritas contra a referência. `app/ponto/urls.py:10`, `app/chamados/views.py:721`, `app/chamados/services/tarefa_do_dia.py:825–830`, `app/api/urls.py:92` | Referência e rodadas de outras obras declaradas. A rodada da F5 estava prevista para quando sua árvore fosse ao Opus. Resultado da F5 `[nao verificado]`. `relato_b.txt:1440,1445` | Referência `3f666ad84`. Hash da rodada F5 `[nao verificado]`. `relato_b.txt:1440` | **DEPENDE DE ETAPA FUTURA** — Opus, etapa indicada para o comparativo de tela. `relato_b.txt:1440` |
| 15 | Verificador de escopo inteiro, com matriz, predesenho, avais e `0 BLOQUEIA`. `r_vPRONTO5.txt:17` | `./vPRONTO/medidas_da_sessao.txt` (20/11 02:54), `verificador Codex escopo inteiro (aa3bdd88c) BLOQUEIA=9; delta C4+C5 BLOQUEIA=5; triagens escritas`. A triagem c3 foi de delta. Laudo integral com zero bloqueios nesta árvore `[nao verificado]`. `relato_b.txt:7,700,704` | `aa3bdd88c` no escopo inteiro. `relato_b.txt:700,704` | **RODOU EM ARVORE ANTERIOR.** Veredito de 9 bloqueios. Depois houve mudanças no selo do chokepoint, no vigia de texto e em `models.py`. `relato_b.txt:62,700` |
| 16 | Suíte inteira pré-Opus, com recibo da Pista 2 e precedência da guarda verde e do PRONTO. `r_vPRONTO5.txt:18` | `./vE/volta3/c3/preopus.fim` (20/11 16:54), `rc=0 Ran 17268 tests in 2303.169s OK (skipped=42, expected failures=41) c8eb8eddf 16:54`. Recorte da guarda registrado às 16:15 e lançamento às 16:16. Recibo da Pista 2 `[nao verificado]`. Às 16:54 o RELATO ainda dava a linha do PRONTO como incompleta. `relato_b.txt:9,11` | `c8eb8eddf`. `relato_b.txt:11` | **RODOU NA ARVORE FINAL.** O veredito cobre a suíte. `relato_b.txt:11` |
| 17 | Opus final único no hash da pré-Opus, com triagens e isenção de pureza. `r_vPRONTO5.txt:19` | O Opus final aparece pendente depois da suíte e parado às 18:28. O Opus anterior sobre a volta 1 foi reprovado por escopo. `relato_b.txt:3,11,1297` | Alvo `c8eb8eddf`. Hash de execução final `[nao verificado]`. `relato_b.txt:3,11` | **DEPENDE DE ETAPA FUTURA** — Opus final. `relato_b.txt:3` |
| 20 | Mordidas de `a1_app_coluna` e `a1_app_texto_do_ato`. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:45–53` | Veredito nominal `[nao verificado]`. O log `api_p_cru` documenta outro seletor, `test_MORDE_get_do_app_com_pergunta_crua`. `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:34–36` | `[nao verificado]` | **SEM EVIDENCIA.** |
| 21 | RED da property com texto só no ato e projeção carimbada após reabertura. `app/chamados/tests/test_e4c_f5_opus_models.py:37–54,87–89` | `./opus_models/red.log` (19/11 18:33), `Ran 54 tests in 1.121s`, `FAILED (failures=8, errors=2)`, `rc=1`. É o RED agregado da etapa de `models.py`. A falha nominal da property `[nao verificado]`. `relato_b.txt:1227` | RED na árvore só com o compositor, SHA `[nao verificado]`. Cura `18bf86f01`. `relato_b.txt:1227` | **RODOU EM ARVORE ANTERIOR**, no recorte agregado. `models.py` mudou depois. O delta na property `[nao verificado]`. `relato_b.txt:62,1227` |
| 22 | Mordidas de `a1_fio_coluna`, `a1_fio_texto_do_ato` e `a1_fio_composicao_local`. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–71` | Vereditos dos três seletores `[nao verificado]`. A mordida anterior `M_fio_texto` é registrada para `test_fio_TEXTO_da_projecao`. `relato_b.txt:1386` | `[nao verificado]` para os três seletores. | **SEM EVIDENCIA.** |
| 23 | RED na base de `sem_veredito_entre` com texto e instante divergentes. `app/chamados/tests/test_e4c_f5_opus_models.py:76–81` | `./opus_models/red.log` (19/11 18:33), `Ran 54 tests in 1.121s`, `FAILED (failures=8, errors=2)`, `rc=1`. Falha nominal desse caso `[nao verificado]`. `relato_b.txt:1227` | RED na árvore só com o compositor, SHA `[nao verificado]`. Cura `18bf86f01`. `relato_b.txt:1227` | **RODOU EM ARVORE ANTERIOR**, no recorte agregado. O próprio leitor foi alterado depois para reaproveitar as instâncias do fecho. `app/ponto/tests/test_selo_chokepoint_escrita.py:203–204` |
| 24 | RED na base de `estado` que exige `PENDENTE`. `app/chamados/tests/test_e4c_f5_opus_models.py:83–85` | `./opus_models/red.log` (19/11 18:33), `Ran 54 tests in 1.121s`, `FAILED (failures=8, errors=2)`, `rc=1`. Falha nominal desse caso `[nao verificado]`. `relato_b.txt:1227` | RED na árvore só com o compositor, SHA `[nao verificado]`. Cura `18bf86f01`. `relato_b.txt:1227` | **RODOU EM ARVORE ANTERIOR**, no recorte agregado. `models.py` mudou depois. O delta em `estado` `[nao verificado]`. `relato_b.txt:62,1227` |
| 27 | Comparar a contagem do vigia antes × depois com a mesma fixture e os quatro pares. `app/chamados/tests/test_e4c_f5_contador_texto.py:26–38` | O RELATO aponta que o teste de contagem já passava antes do loteamento. Saídas comparáveis da mesma fixture nas duas árvores `[nao verificado]`. `relato_b.txt:1288` | `[nao verificado]` | **SEM EVIDENCIA.** |
| 28 | RED nominal na base para cinco perguntas, lote dois e três SELECTs de decisões. `app/chamados/tests/test_e4c_f5_contador_texto.py:87–95` | `./vFINAL/red_c5.log` (20/11 02:35), `Ran 27 tests in 3.199s`, `FAILED (failures=1, errors=1)`. A C5 alterou o loteamento. A saída que atribua uma falha a esse teste `[nao verificado]`. `relato_b.txt:700` | C5 `a3c23d10b`. SHA do RED nominal `[nao verificado]`. `relato_b.txt:700` | **SEM EVIDENCIA** do RED nominal. |
| 39 | REDs 3 × 6 e verde da passagem A, incluindo O261 por candidata. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:44–240` | `./vA/quatro/run12_base_k16.log` (19/11 22:33), `FAILED (failures=1)`. `./vA/quatro/run12_copia_k16.log` e `run12_mut_k16.log` (22:34), ambos `OK`. A bateria posterior `./vA/quatro/run14.log` (22:36) traz `Ran 286 tests in 41.221s`, `OK`. O RELATO declara as nove mordidas na rodada posterior a esse `OK` do mutante K16 isolado. `relato_b.txt:1212,1214,1217` | Ensaio verde sobre `acdcd556d` + diff. SHA de cada RED de base `[nao verificado]`. `relato_b.txt:1212,1217` | **RODOU EM ARVORE ANTERIOR.** O módulo A ganhou depois os dois casos C11. Saída nominal de todos os REDs de base `[nao verificado]`. `relato_b.txt:690,1212` |
| 43 | Preservar os tetos existentes E2 e C11, com diff. `app/chamados/tests/test_e2_tarefa_do_dia.py:913–938`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:378–402` | `./vA/quatro/run6.log` (19/11 21:50), `Ran 286 tests in 21.808s`, `FAILED (failures=2, errors=34)`. O RELATO declara os tetos direcionados verdes nessa rodada. `./vPRONTO/c11_green.log` (20/11 03:11), `Ran 64 tests in 13.456s`, `OK`. O RELATO registra o teto C11 de 1 × 5 intacto. Diff dos dois tetos até a final `[nao verificado]`. `relato_b.txt:690,1214` | A2 sobre `acdcd556d` + diff. C7 `f62a39996`. `relato_b.txt:690,1212,1214,1217` | **RODOU EM ARVORE ANTERIOR.** Mudanças posteriores nos arquivos dos dois tetos `[nao verificado]`. |
| 44 | Remedição nominal O261, teto 9 e um SELECT de decisões por candidata. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–207`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:188–204` | `./vA/quatro/run6.log` (19/11 21:50), `Ran 286 tests in 21.808s`, `FAILED (failures=2, errors=34)`. O RELATO declara especificamente `o261 9/candidata` verde na A2. Saída nominal do SELECT por candidata `[nao verificado]`. `relato_b.txt:1214` | A2 sobre `acdcd556d` + diff. `relato_b.txt:1212,1217` | **RODOU EM ARVORE ANTERIOR.** `models.py` mudou depois. O delta no comando O261 e no teste dedicado `[nao verificado]`. `relato_b.txt:62` |
| 47 | Remedição dos quatro casos do chokepoint, 1 e 3 furos × fecha e fica. `app/ponto/tests/test_selo_chokepoint_escrita.py:467–475,502–510` | `./chokepoint_final/wt-f5-chkf-copia-021731.log` (20/11 02:27), `Ran 6 tests in 2.173s`, `FAILED (failures=4)`. `./chokepoint_final/selo_c4.log` (02:29), `Ran 14 tests in 4.064s`, `OK`. Tetos medidos `234/146/298/221`, registrados na C4. `relato_b.txt:700,704` | Medida `eb1e49c1e` × `aa3bdd88c`. Selo C4 `3c351fee4`. `relato_b.txt:700,704` | **RODOU EM ARVORE ANTERIOR.** Depois mudaram `models.py` e caminhos de disputa e materialização. Remedição nominal na final `[nao verificado]`. `relato_b.txt:17,38,62` |
| 48 | Captura de SQL por verbo e sítio, incluindo contagem de novos escritores. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–205,220–229,248–252,260–266` | MAIN `./chokepoint_final/wt-f5-chkf-main-021731.log` (20/11 02:27), `Ran 6 tests in 2.151s`, `OK`. Cópia `./chokepoint_final/wt-f5-chkf-copia-021731.log` (02:27), `Ran 6 tests in 2.173s`, `FAILED (failures=4)`. A atribuição registrada dá só SELECTs e zero `INSERT`, `UPDATE`, `DELETE` ou `SAVEPOINT` adicionais naquela comparação. `relato_b.txt:700,704,1206`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196–205` | Primeira atribuição `eb1e49c1e` × `ec7134368`. Remedição `eb1e49c1e` × `aa3bdd88c`, incorporada na C4 `3c351fee4`. `relato_b.txt:700,704,1206` | **RODOU EM ARVORE ANTERIOR.** A captura por SQL em `c8eb8eddf` `[nao verificado]`. Houve mudanças nos caminhos exercitados, incluindo `models.py` e `disputa_emissao.py`. `relato_b.txt:38,62` |
| 50 | Placar nominal verde das onze famílias de harness. `app/chamados/tests/test_pergunta_viva.py:16–76`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:51–87` | Há ensaios de dublês e um recorte A com alguns módulos nomeados. O placar das onze famílias `[nao verificado]`. `relato_b.txt:1211–1212,1219` | `[nao verificado]` para o placar pedido. | **SEM EVIDENCIA.** |
| 52 | Comparar `respondida` nos dois sentidos. `app/chamados/models.py:3838–3844` | 51.563 percorridas, zero erros, zero `True→False` e zero `False→True`, na sombra anterior. `relato_b.txt:689,1237` | `eb1e49c1e` × `a3c23d10b` na rodada Q2. SHA da primeira medição às 17:3x `[nao verificado]`. `relato_b.txt:689,1237` | **SO DECLARADO NO RELATO SEM LOG NO INVENTARIO.** `models.py` mudou depois. Resultado dessa medida na final `[nao verificado]`. `relato_b.txt:62,689` |
| 53 | Comparar a decisão de `sem_veredito_entre` na frota. `app/chamados/models.py:2002–2027` | `PERCORRIDAS=51563 ERROS=0`, `MUDA_SEM_VEREDITO=0`, sonda 1(e) das 18:4x. `relato_b.txt:1223` | SHA da sonda `[nao verificado]`. `18bf86f01` é o commit Opus citado no contexto. `relato_b.txt:1223,1227` | **SO DECLARADO NO RELATO SEM LOG NO INVENTARIO.** Há alteração posterior no percurso do leitor. `app/ponto/tests/test_selo_chokepoint_escrita.py:203–204` |
| 54 | Comparar os rótulos de `estado` na frota. `app/chamados/models.py:3249–3268` | `PERCORRIDAS=51563 ERROS=0`, `MUDA_ESTADO=0`, na mesma sonda 1(e). `relato_b.txt:1223` | SHA da sonda `[nao verificado]`. `relato_b.txt:1223` | **SO DECLARADO NO RELATO SEM LOG NO INVENTARIO.** Mudanças no trecho exato e remedição final `[nao verificado]`. |
| 57 | Série da catraca de chamadas por identidade na base Opus e em A/B/C. `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160,188–193` | O RELATO declara base 19 e recontagem AST com apelido de 41. A sequência `24 → 10 → 0` está descrita como esperada. `./vE/volta3/c3/catraca_c4.log` (20/11 16:15), `Ran 35 tests in 42.556s`, `OK`, é da catraca `test_medida_turno_do_par`. O placar por volta do selo de chamadas `[nao verificado]`. `relato_b.txt:7,9,1213,1224,1227` | `18bf86f01` no registro Opus. `c8eb8eddf` na catraca dos 35. Hashes do selo por rodada A/B/C `[nao verificado]`. `relato_b.txt:7,9,1227` | **SO DECLARADO NO RELATO SEM LOG NO INVENTARIO**, quanto à linha de base da série pedida. `relato_b.txt:1224,1227` |
| 58 | Commit Opus próprio de `models.py` anterior à A, seis REDs na base e mapa K41/K42. `app/chamados/tests/test_e4c_f5_opus_models.py:57–167` | `./opus_models/red.log` (19/11 18:33), `Ran 54 tests in 1.121s`, `FAILED (failures=8, errors=2)`, `rc=1`. `./opus_models/green.log` (18:34), `Ran 54 tests in 1.152s`, `FAILED (errors=2)`, `rc=1`. O RELATO atribui os dois erros da cura às fixtures RED-8. Mapa K41/K42 e placar nominal dos seis REDs `[nao verificado]`. `relato_b.txt:1219,1227,1233,1236` | `18bf86f01` antes da A1 `0db6afc73`. SHA da base só com compositor `[nao verificado]`. `relato_b.txt:1219,1227` | **RODOU EM ARVORE ANTERIOR.** A rodada da cura teve dois erros. `models.py` mudou depois. `relato_b.txt:62,1227` |
| 59 | Diff de `folha/` contra a base, demonstrando o limite de dois sítios. `app/folha/previa.py:64–67,139–146` | O RELATO declara diff restrito à carga e ao argumento do juiz em `previa.py`. A saída do diff `[nao verificado]`. `relato_b.txt:690` | `eb1e49c1e` × `f62a39996`. `relato_b.txt:690` | **SO DECLARADO NO RELATO SEM LOG NO INVENTARIO.** Mudanças posteriores em `app/folha/` `[nao verificado]`. |
| 60 | Prévia da competência 11, MAIN × cópia, por colaborador e dia. `app/folha/previa.py:28,64–67,135–146` | `./vPRONTO/medidas_da_sessao.txt` (20/11 02:54), `4 empresas, 450 colaboradores, 5861 linhas colab x dia, diferenca 0 nas duas datas de referencia (hoje=28/11 e hoje=20/11), ERROS=0`. `relato_b.txt:689` | MAIN `eb1e49c1e` × C5 `a3c23d10b`. `relato_b.txt:689` | **RODOU EM ARVORE ANTERIOR.** `models.py`, consumido pela prévia, mudou depois. Delta em `previa.py` e remedição final `[nao verificado]`. `relato_b.txt:62`, `app/folha/previa.py:139–146` |
| 73 | Remedir `resposta_display`, com a comparação literal e os dois zeros aceitos. `app/chamados/models.py:3865–3891` | Medida anterior declarada às 02:48, com 16 diferenças literais em 51.563, zero com o mesmo formatador e zero para texto do ato × coluna. `relato_b.txt:656` | SHA dessa rodada de display `[nao verificado]`. `relato_b.txt:656` | **SO DECLARADO NO RELATO SEM LOG NO INVENTARIO.** Aplicabilidade a `c8eb8eddf` `[nao verificado]`. |
| 74 | Certificação na VM e pouso único integral. `r_vPRONTO5.txt:76`, `bin/suite.sh:23–29,236–257` | Há recibo da pré-Opus. Certificação de pouso e pouso final `[nao verificado]`. `relato_b.txt:3,11,674` | `c8eb8eddf` na pré-Opus. Hash certificado para pouso `[nao verificado]`. `relato_b.txt:11` | **DEPENDE DE ETAPA FUTURA** — certificação na VM e pouso, após Opus final. `relato_b.txt:3,674` |
| 75 | Smoke após pouso, GET com colaborador de teste, badge e pendentes nas duas cascas, cinco minutos e zero traceback. `r_vPRONTO5.txt:77`, `app/api/views.py:2536–2556` | Saída do smoke `[nao verificado]`. | `[nao verificado]` | **DEPENDE DE ETAPA FUTURA** — pouso e smoke de leitura do app. `r_vPRONTO5.txt:77` |
| 76 | Abertura humana do modal após pouso, com registro no RELATO. `r_vPRONTO5.txt:78`, `app/chamados/views.py:869–880` | Registro da abertura na F5 `[nao verificad

[truncado em 20000 de 34102 caracteres]

## 20/11 18:35 · sessao 4dd232d65b45 · - · - · origem=Ronald (tmux)

**49, 51, 55, 68 e 80 — PUBLICADO EM PARTE.** Os anexos mostram resultados, mudanças pontuais e referências aos artefatos. A classificação nominal histórica das famílias e dos chamadores, o diff integral por exceção e os esperados anteriores por teste seguem [nao verificado]. `relato_a.txt:190,669,690,1211–1216,1219,1221,1224,1245` [nao verificado]

- **49.** O selo registra nove módulos e 486 asserts. A contagem AST atual reproduz todos os valores registrados: `e3_v8_apoio` 0, `test_e1_v10` 31, `test_e3_c8b_sem_banco` 86, `test_e3_degrau_dono_e_porta` 134, `test_e3_pos_suite_sem_banco` 29, `test_e3_tarefa_admin_sem_banco` 7, `test_e4b_c4v7_memoria` 65, `test_e4b_classe_e_memoria` 122 e `test_pergunta_viva` 12. O último está explicitamente fora das 11 famílias. `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35,76–80`, `relato_a.txt:1221`
- **51.** Os dois RED-8 pertencem a `JuizesVolta2Test`: `test_red_8_custo_pergunta_viva_calc_prefetch_uma_e_cinco` e `test_red_8_custo_montar_chamados_fio_uma_e_cinco`. O primeiro compara 1 e 5 e exige argumento `LeituraComposta`. O segundo também exige até um SELECT de decisão, igualdade do conjunto de PKs lidos e uso de cache prefetch. A identificação como **12ª** família em ordem histórica segue [nao verificado]. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:25–26,119–186`, `relato_a.txt:1215–1216,1219,1245`
- **55.** A transição publicada é base 41, após A 24, após B 10, após C 0. Para os 55 indiretos, o relato nomeia a troca em `_pergunta_viva_calc`, sem lista por chamador. A cópia final tem 44 chamadas reconhecidas pela regra AST atual, com 39 `pergunta_respondida` compostas, quatro projeções temporais compostas e uma exceção no raio-X. `relato_a.txt:1213,1221,1224`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:22–28,126–160`, `app/chamados/services/raiox.py:135–147`
- **68.** A cópia preserva as adaptações nominais citadas nos anexos: âncoras C12, E3, F5 volta 1 e `retry_reativa`, dublê único, RED-8 de tipo/fixture/medição, prefetch identidade em `Linhas` e `Perguntas`, exceção M4 e registro de linhas da catraca. O diff por uso e o aval de cada adaptação além das nominadas seguem [nao verificado]. `app/chamados/tests/e4b_c12_ast.py:18–27`, `app/chamados/tests/e3_mutantes.py:30`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:229–233`, `app/chamados/tests/bug_par_mutantes.py:68–71`, `app/chamados/tests/duble_pergunta.py:7–41`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:119–186`, `app/chamados/tests/test_e4b_classe_e_memoria.py:193–194`, `app/chamados/tests/test_e3_r3_c7_sem_banco.py:181–192`, `app/chamados/tests/test_cortes_f2_f9_m4.py:51–65`, `app/core/tests/test_medida_turno_do_par.py:258–271,319`, `relato_a.txt:1211–1219`
- **80.** K47 valida pela porta real, confirma equivalência de presença e carimbo `AGORA`, espera 0 perguntas resolvidas, uma disputa fechada e nenhuma pergunta materializada. Na D, o helper mede presença equivalente e instante com a porta real. A devolução é a mudança anterior explícita no anexo, de zero para uma linha DEVOLUÇÃO. O relacionamento nominal dos 18 esperados antigos anunciados no relato segue [nao verificado]. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:35–67,388–396`, `relato_a.txt:190,669`

O histórico está [nao verificado] nesta cópia. Em repositório com os objetos, `git diff -U3 eb1e49c1e c8eb8eddf -- ':(glob)app/**/tests/**'` e `git log -p eb1e49c1e..c8eb8eddf -- ':(glob)app/**/tests/**'` recuperam as mudanças para 49/68. `git show --format=fuller 604505878 -- app/chamados/tests/test_e4c_f5_voltaB_lotes.py app/chamados/tests/test_e4c_f5_voltaD_portas.py` recupera corpo e mudanças de 80. Para 55, `git grep -n -E 'pergunta_respondida|projecao_temporal_da_pergunta|pergunta_viva|_pergunta_viva_calc|_slot_esperado' <rev> -- app` em cada marco fornece os candidatos ao cruzamento AST com a matriz histórica. `relato_a.txt:190,690,1216,1224,1245`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160`

## 20/11 18:35 · sessao 26d5bb30be29 · - · - · origem=Ronald (tmux)

A matriz reconstruível tem 15 sítios. O selo registra 13 funções em oito arquivos, inclui o histórico como RED-5 adicional, representa dois sítios por `par_do_intervalo` e deixa os dois juízes puros para o selo da chamada. O censo executado apenas em memória retornou zero acusações. A numeração e a ordem da matriz original ficam [nao verificado]. `app/chamados/tests/test_e4c_f5_selo_catraca.py:11–26,291–307` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160,188–193` · `relato_a.txt:1297,1410`

| Sítio | Leitura na cópia |
|---|---|
| Fio | Instante da composta e texto corrente em `app/chamados/services/fio.py:178–190`. |
| Tag | Guarda pelo texto corrente antes de chamar o classificador. `app/chamados/templatetags/chamados_tags.py:145,166–171` |
| Juiz temporal | Lê o objeto recebido. O C6 passa a composta. `app/chamados/juizes.py:1383–1409` |
| Juiz respondida | Confere instante e texto no objeto recebido. `app/chamados/juizes.py:1428,1551–1560` |
| Correção | Guarda de validação pela composta. `app/chamados/services/acoes_disputa.py:349–350` |
| Declaração | Guarda comum de validação pela composta. `app/chamados/services/declaracao_texto.py:217–229` |
| S143 sem credor | Guarda de validação pela composta das perguntas carregadas em lote. `app/chamados/services/disputa_emissao.py:2107–2112` |
| Par, irmã exigida validada | Usa a validação da composta da irmã. `app/chamados/services/materializacao.py:200–202` |
| Par, irmã implausível | Usa a validação da composta antes de perguntar ao juiz de implausibilidade. `app/chamados/services/materializacao.py:200,203–208` |
| Materializador | Seleção inicial SQL pela coluna, releitura sob trava e guarda pela composta. `app/chamados/services/materializacao.py:2142–2163` |
| Validação do par | Só carimba a irmã se a composta não trouxer validação. `app/chamados/services/validacao.py:656–657` |
| Validar pergunta | Confere resposta e validação da composta. `app/chamados/services/validacao.py:698–704` |
| Recusar pergunta | Confere validação e resposta da composta. O instante cru aparece na foto anterior. `app/chamados/services/validacao.py:915–932` |
| Validar lote A | Lê a composta para admitir a pergunta e volta a ler depois da ação e do refresh. `app/chamados/services/validacao.py:1074–1084,1102–1105` |
| Devolução | Aceita guarda ato ou coluna e é escritora da própria coluna. `app/chamados/services/validacao.py:269–293` |

O histórico adicional lê instantes do ato em lote e o texto corrente. A catraca verifica strings em `getattr` e `refresh_from_db(fields=...)`. `update_fields` aparece em um caso aceito, sem tratamento no visitante. `app/chamados/services/historico.py:96–111` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:174–197,360–369`

| Item 19 | Grep da cópia |
|---|---|
| Compositor | `lido = cls.leitura(p)` e retorno `LeituraComposta(lido.validada_em, lido.respondida_em, p.resposta_colab, p.materializada_em, p.via_resolucao)`. `app/chamados/models.py:4382–4390` |
| App | `_pergunta_respondida(DecisaoPergunta.composta(p))`. `app/chamados/services/pergunta_no_app.py:83–88` |
| Property | `return pergunta_respondida(DecisaoPergunta.composta(self))`. `app/chamados/models.py:3837–3844` |
| Fio | `leitura = DecisaoPergunta.composta(p)` e `respondida_sem_texto(leitura)`. `app/chamados/services/fio.py:187–190` |
| `sem_veredito_entre` e `estado` | Os dois sítios restantes do trio de modelos nomeado no teste, além de `respondida`. `app/chamados/models.py:2002–2027,3249–3268` · `app/chamados/tests/test_e4c_f5_opus_models.py:1–6,75–89` |

A remedição e os vínculos complementares para 32, 45, 55, 80, 111 e 112 foram enviados ao agente principal com as respectivas referências. `app/chamados/tests/test_e4c_f5_contador_texto.py:40–53` · `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160` · `relato_a.txt:190` · `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:146–204` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:69–76`

## 20/11 18:34 · sessao 0934caa38e47 · - · - · origem=Ronald (tmux)

Os registros nominais recuperáveis são 33 K com carregador e ponto de toque, mais K40 vinculado à classe R pelo RELATO e ao laço por PK no código. K41/K42 são atribuídos à B pelo RELATO. Os respectivos carregadores e as classes individuais dos demais K ficam [nao verificado]. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–18`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–26`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–22`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`, `relato_a.txt:695,1232,1236,1239,1245`.

A B tem 14 carregadores nomeados nos mutantes e 14 sítios diretos descritos no RELATO e exercitados na cópia. O total de 29 cobertos, 27 editados e a composição de 5 L + 3 Q ficam [nao verificado]. A C tem dez sítios diretos, nove K do ensaio de lotes, K13 com releitura após escrita, a sobra dedup e a passagem separada D do fecho. A lista completa dos 25 K da C e as três M ficam [nao verificado]. `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:66–296`, `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:40–251`, `app/chamados/tests/test_e4c_f5_voltaC_sitios.py:65–143`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:74–264`, `app/chamados/models.py:2068–2072`, `relato_a.txt:1196,1213,1236`.

Os comandos para apurar as edições no repositório com objetos são `git diff --unified=0 ec7134368 d0210a400 -- app`, `git diff --unified=0 d0210a400 a81f888dc -- app` e `git diff --unified=0 d9319a215 57582ee14 -- app`. A lista nominal e as classes dependem também dos artefatos históricos citados no RELATO. A presença deles em objetos Git fica [nao verificado]. `relato_a.txt:1196,1203,1239,1245`.

## 20/11 18:27 · sessao cb5422d2333e · E4C-F5-CONSTROI · leitura · origem=Code

## 1. `test_e4c_f5_medida_seis.py`

Montar em `app/chamados/tests/` na cópia descartável e rodar pelo label `chamados.tests.test_e4c_f5_medida_seis` com `config.settings.ci`, que usa PostgreSQL. O teste reutiliza a fixture de `BaseCortes`, os relógios e auxiliares de A1(c), e o padrão de duas threads com conexões próprias de `CorridaDoAbrirTest`. `app/config/settings/ci.py:6–19` · `app/chamados/tests/cortes_f2_f9_apoio.py:30–44` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:24–104,135–140` · `app/chamados/tests/test_c2_corrida_do_abrir.py:62,75–124`

Na corrida, a declaração inicial do atestado conclui a transação dela antes de entrar no materializador. A barreira põe a passagem e a resposta da irmã diante da mesma trava. A sessão que chega primeiro só prossegue depois de o PostgreSQL mostrar a outra sessão bloqueada por ela. A resposta usa o core direto, conforme o recorte A1(c). A entrada web/API liga o veto H4 quando a absorção já está vigente. `app/chamados/services/disputa_emissao.py:1478–1499,1532–1537,1561–1566,1587–1594` · `app/chamados/services/materializacao.py:1036–1058` · `app/chamados/models.py:2830–2836,2874–2878`

No teste de rejeição, **CONTADO** exige a trilha persistida `irma_viva_na_ausencia` para o PK e o atestado da passagem. O código acumula `recusadas` e tenta gravar essa trilha. O auxiliar existente lê as linhas por ação e PK. A foto também mostra o leitor de carimbos sem lastro em disputa aberta, cujo universo é próprio. Um vínculo histórico `ABSORCAO` sozinho não comprova que a pergunta entrou na contagem das recusas. `app/chamados/services/materializacao.py:1083–1104,1115–1119` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:138–140,253–264` · `app/chamados/services/vigia_fio.py:755–786,814–818`

```python
import json
import threading
from contextlib import contextmanager
from unittest.mock import patch

from django.contrib.auth.models import User
from django.db import IntegrityError, connection, transaction
from django.db.models import Q
from django.test import TransactionTestCase
from django.utils import timezone
from freezegun import freeze_time

from chamados.models import DecisaoPergunta as D
from chamados.models import DisputaSupervisao, PerguntaDisputa as P
from chamados.services import disputa_emissao as E
from chamados.services import materializacao as M
from chamados.services import validacao, vigia_fio
from chamados.services.vigia_ato_x_coluna import contar
from chamados.tests import cortes_f2_f9_apoio as apoio
from chamados.tests import test_e4c_f5_a1c_passagem_da_ausencia as a1c
from core.models import LogAuditoria
from escala.models import EventoCelula
from ponto.services.ausencia import decidir
from ponto.services.cartorio import calado


HORA = "19:02"
ACAO_DEVOLUCAO = "perguntas_devolvidas_ausencia_rejeitada"


@contextmanager
def medida(subitem):
    fatos = {}
    try:
        yield fatos
    except Exception as erro:
        fatos["tipo_excecao"] = type(erro).__name__
        raise
    finally:
        print(
            f"MEDIDA: {subitem} | "
            + json.dumps(fatos, default=lambda valor: valor.isoformat()),
            flush=True,
        )


def par(qs):
    placar = contar(perguntas=qs)
    return dict(A=placar["a"]["numero"], B=placar["b"]["numero"],
                universo=placar["universo"])


def pergunta_foto(p):
    leitura = D.composta(p)
    return dict(
        tipo=p.tipo_resposta, estado=p.estado,
        resposta=p.resposta_colab, respondida_em=p.respondida_em,
        resposta_composta=leitura.resposta_colab,
        respondida_composta_em=leitura.respondida_em,
        validada_em=p.validada_em, validada_por_id=p.validada_por_id,
        validada_composta_em=leitura.validada_em,
        materializada_em=p.materializada_em,
        materializada_por_id=p.materializada_por_id, via=p.via_resolucao,
        atos=list(p.decisoes.order_by("ato_em", "pk").values()),
    )


class MedidaSeisTest(TransactionTestCase):
    # Reúso sem herdar o TestCase nem recolher os testes da classe vizinha. [nao verificado]
    # app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:114–124
    _escape = a1c.PassagemDaAusenciaTest._escape
    _irma_da_celula = a1c.PassagemDaAusenciaTest._irma_da_celula
    _irma_com_luz = a1c.PassagemDaAusenciaTest._irma_com_luz
    _irma_muda = a1c.PassagemDaAusenciaTest._irma_muda
    _responder = a1c.PassagemDaAusenciaTest._responder
    _passagem = a1c.PassagemDaAusenciaTest._passagem
    _ausencia = a1c.PassagemDaAusenciaTest._ausencia
    _trilha = a1c.PassagemDaAusenciaTest._trilha

    def setUp(self):
        self.relogio = self.enterContext(freeze_time(a1c.T0))
        self.enterContext(calado())
        # app/chamados/tests/cortes_f2_f9_apoio.py:37–44
        apoio.BaseCortes.setUpTestData.__func__(type(self))
        # app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:31–39
        self.usuario = User.objects.create_user(username="e4cf5_medida_colab")
        self.colab.usuario = self.usuario
        self.colab.save(update_fields=["usuario"])
        self.disputa = DisputaSupervisao.objects.create(
            colaborador=self.colab, contexto={},
        )

    def _corrida(self, primeira):
        with medida(f"A1(c) corrida {primeira} primeiro") as fatos:
            self.assertEqual(connection.vendor, "postgresql")
            escape, irma = self._escape(), self._irma_com_luz()
            usuario_pk = self.usuario.pk
            fatos["pergunta_pk"] = irma.pk
            connection.close()

            segunda = "resposta" if primeira == "passagem" else "passagem"
            relogios = {"passagem": a1c.T1, "resposta": a1c.T2}
            local = threading.local()
            barreira = threading.Barrier(2)
            primeira_travou = threading.Event()
            pausa = threading.Event()
            sessoes, erros, retornos = {}, {}, {}
            commits, bloqueios = [], [] [nao verificado]
            fatos.update(sessoes=sessoes, commits=commits, [nao verificado]
                         bloqueios=bloqueios, retornos=retornos)
            trava_real = P.trava_do_colaborador
            passagem_real = M._materializar_ausencia_de_resposta

            def observar_passagem(*args):
                local.na_passagem = True
                try:
                    recusadas = passagem_real(*args)
                    retornos["recusadas"] = sorted(set(recusadas))
                    return recusadas
                finally:
                    local.na_passagem = False

            def interpor_trava(dono):
                papel = local.papel
                # A trava anterior, da declaração do escape, segue direto.
                if local.interposta or (papel == "passagem" and not local.na_passagem):
                    return trava_real(dono)
                local.interposta = True
                self.assertEqual(dono, self.colab.pk)
                self.assertTrue(connection.in_atomic_block)
                barreira.wait(timeout=12)
                if papel == segunda:
                    self.assertTrue(primeira_travou.wait(timeout=12))
                trava_real(dono)
                if papel == primeira:
                    primeira_travou.set()
                    with connection.cursor() as cursor:
                        for _ in range(320):
                            cursor.execute("SELECT pg_blocking_pids(%s)", [sessoes[segunda]])
                            if sessoes[primeira] in cursor.fetchone()[0]:
                                bloqueios.append([primeira, segunda])
                                break
                            pausa.wait(0.025)
                        else:
                            self.fail("O PostgreSQL não mostrou a segunda sessão na trava")
                transaction.on_commit(lambda: commits.append(papel)) [nao verificado]

            def sessao(papel, pk, texto):
                local.papel = papel
                local.interposta = local.na_passagem = False
                try:
                    with calado(), connection.cursor() as cursor:
                        cursor.execute("SET lock_timeout = '15s'")
                        cursor.execute("SET statement_timeout = '20s'")
                        cursor.execute("SELECT pg_backend_pid()")
                        sessoes[papel] = cursor.fetchone()[0]
                    with calado():
                        p = P.objects.get(pk=pk)
                        usuario = User.objects.get(pk=usuario_pk)
                        retornos[papel] = E.responder_pergunta_disputa_core(
                            p, texto, usuario, ja_respondida=p.respondida_em is not None,
                        )
                except Exception as erro:
                    erros[papel] = erro
                    barreira.abort()
                finally:
                    connection.close()

            threads = [
                threading.Thread(target=sessao, args=("passagem", escape.pk, "ATESTADO")),
                threading.Thread(target=sessao, args=("resposta", irma.pk, HORA)),
            ]
            with (
                patch.object(P, "trava_do_colaborador", new=staticmethod(interpor_trava)),
                patch.object(E, "_materializar_ausencia_de_resposta", new=observar_passagem),
                patch.object(timezone, "now", new=lambda: relogios.get(
                    getattr(local, "papel", None), a1c.T0)),
            ):
                for thread in threads:
                    thread.start()
                for thread in threads:
                    thread.join(timeout=45)
                self.assertTrue(all(not thread.is_alive() for thread in threads))
            fatos["erros"] = {papel: type(erro).__name__ for papel, erro in erros.items()}
            if erros:
                raise next(iter(erros.values()))

            # Releitura em uma conexão aberta após o fim das duas sessões.
            with connection.cursor() as cursor:
                cursor.execute("SELECT pg_backend_pid()")
                fatos["sessao_releitura"] = cursor.fetchone()[0]
            qs = P.objects.filter(pk=irma.pk)
            foto = pergunta_foto(qs.get())
            sql_validada = qs.filter(P.q_validada_pelo_ato()).exists()
            fatos.update(
                pergunta=foto, q_validada_pelo_ato=sql_validada, par=par(qs),
                pk_em_recusadas=irma.pk in retornos["recusadas"],
            )

            self.assertEqual(len(set(sessoes.values())), 2)
            self.assertEqual(bloqueios, [[primeira, segunda]])
            self.assertCountEqual(commits, ["passagem", "resposta"]) [nao verificado]
            self.assertEqual(retornos["passagem"], (True, None))
            self.assertEqual(retornos["resposta"], (True, None))
            self.assertEqual(fatos["par"], {"A": 0, "B": 0, "universo": 1})
            composta_validada = foto["validada_composta_em"] is not None
            self.assertEqual(foto["validada_em"] is not None, composta_validada)
            self.assertEqual(sql_validada, composta_validada)
            self.assertEqual(
                (foto["resposta"], foto["respondida_em"],
                 foto["resposta_composta"], foto["respondida_composta_em"]),
                (HORA, a1c.T2, HORA, a1c.T2),
            )
            self.assertIn(
                (D.RESPOSTA, a1c.T2, HORA, usuario_pk),
                [(ato["ato"], ato["ato_em"], ato["ato_texto"], ato["ato_por_id"])
                 for ato in foto["atos"]],
            )

    def test_resposta_chega_primeiro_a_trava(self):
        self._corrida("resposta")

    def test_passagem_chega_primeiro_a_trava(self):
        self._corrida("passagem")

    def _foto_grupo(self, ausencia):
        ausencia.refresh_from_db()
        qs = P.objects.filter(disputa_id=self.disputa.pk).order_by("pk")
        perguntas = {p.pk: pergunta_foto(p) for p in qs}
        disputa = DisputaSupervisao.objects.values(
            "pk", "fechada_em", "fechada_por_id", "motivo_fechamento",
        ).get(pk=self.disputa.pk)
        auditoria = list(LogAuditoria.objects.filter(
            Q(modelo="PerguntaDisputa", objeto_id__in=perguntas)
            | Q(modelo="Ausencia", objeto_id=ausencia.pk)
            | Q(modelo="DisputaSupervisao", objeto_id=self.disputa.pk),
        ).order_by("criado_em", "pk").values(
            "pk", "criado_em", "acao", "modelo", "objeto_id", "usuario_id",
            "valor_antes", "valor_depois",
        ))
        for linha in auditoria:
            for campo in ("valor_antes", "valor_depois"):
                if isinstance(linha[campo], dict):
                    linha[campo].pop("por", None)
        eventos = list(EventoCelula.objects.filter(
            colaborador_id=self.colab.pk, data=a1c.DIA,
        ).order_by("pk").values(
            "pk", "celula_id", "data", "criado_em", "processado_em", "tentativas",
        ))
        contadas = {
            p.pk: self._trilha(p).filter(valor_depois__ausencia_pk=ausencia.pk).count()
            for p in qs
        }
        return dict(
            ausencia_pk=ausencia.pk, status_ausencia=ausencia.status,
            grupo=[p.pk for p in M.perguntas_absorvidas_por(ausencia)],
            perguntas=perguntas, disputa=disputa, auditoria=auditoria,
            eventos_celula=eventos, contadas_na_trilha=contadas, par=par(qs),
            q_validada_pelo_ato=list(qs.filter(P.q_validada_pelo_ato())
                                    .values_list("pk", flat=True)),
        )

    @staticmethod
    def _novas_reaberturas(antes, depois):
        atos_antes = {
            ato["pk"] for p in antes["perguntas"].values() for ato in p["atos"]
        }
        return {
            pk for pk, p in depois["perguntas"].items()
            if p["estado"] == P.PENDENTE and any(
                ato["ato"] == D.REABERTURA and ato["pk"] not in atos_antes
                for ato in p["atos"]
            )
        }

    def _medir_rejeicao(self, fatos, *, retirar_pk=None, falhar_auditoria=False):
        ausencia = self._ausencia()
        montado = self._foto_grupo(ausencia)
        fatos["apos_passagem"] = montado
        if retirar_pk is not None:
            self.assertIn(retirar_pk, montado["grupo"], "A âncora precisa estar no grupo")
            self.assertGreater(len(montado["grupo"]), 1, "O grupo precisa ter dependente")
            self.relogio.move_to(a1c.T2)
            fatos["retorno_retirada"] = E.reabrir_para_correcao(
                P.objects.filter(pk=retirar_pk), por=self.admin, motivo="Retirada da âncora",
            )
            self.assertEqual(fatos["retorno_retirada"], 1)
        antes = fatos["antes"] = self._foto_grupo(ausencia)

        devolver = M.devolver_absorvidas_da_ausencia_rejeitada
        criar = LogAuditoria.objects.create
        chamadas, falhas = [], []
        fatos.update(retornos_devolucao=chamadas, falhas_auditoria=falhas)

        def observar_devolucao(a):
            chamada = {"ausencia_pk": a.pk, "status": a.status}
            chamadas.append(chamada)
            chamada["retorno"] = devolver(a)
            return chamada["retorno"]

        def criar_auditoria(**campos):
            if falhar_auditoria and campos["acao"] == ACAO_DEVOLUCAO:
                campos["modelo"] = None  # INSERT real, como no teste vizinho. [nao verificado]
            try:
                return criar(**campos)
            except IntegrityError as erro:
                falhas.append(type(erro).__name__)
                raise

        self.relogio.move_to(a1c.T3)
        # O callback real roda no commit de decidir, em TransactionTestCase. [nao verificado]
        with (
            patch.object(M, "devolver_absorvidas_da_ausencia_rejeitada",
                         new=observar_devolucao),
            patch.object(LogAuditoria.objects, "create", new=criar_auditoria),
        ):
            rejeitada = decidir(ausencia, "rejeitada", por=self.admin, motivo="Teste de rejeição") [nao verificado]
        fatos["retorno_decidir"] = {"pk": rejeitada.pk, "status": rejeitada.status}
        depois = fatos["depois"] = self._foto_grupo(ausencia)

        reabertas_antes = self._novas_reaberturas(montado, antes)
        reabertas_na_rejeicao = self._novas_reaberturas(antes, depois)
        candidatas = {pk for pk, p in antes["perguntas"].items() if p["tipo"] == "hora"}
        nao_devolvidas = candidatas - reabertas_antes - reabertas_na_rejeicao
        contadas = {pk for pk, n in depois["contadas_na_trilha"].items() if n}
        vigia = vigia_fio.carimbos_sem_lastro_por_dono(empresas=[self.emp.pk])
        fatos.update(
            reabertas_antes=sorted(reabertas_antes),
            reabertas_na_rejeicao=sorted(reabertas_na_rejeicao),
            nao_devolvidas=sorted(nao_devolvidas),
            sem_contagem_na_trilha=sorted(nao_devolvidas - contadas),
            vigia_carimbos_sem_lastro=vigia,
        )

        self.assertEqual(depois["status_ausencia"], "rejeitada")
        self.assertTrue(any(c["ausencia_pk"] == ausencia.pk and c["status"] == "rejeitada"
                            and "retorno" in c for c in chamadas))
        if falhar_auditoria:
            self.assertIn("IntegrityError", falhas, "A falha da trilha precisa ser exercitada")
        self.assertEqual(depois["par"], {"A": 0, "B": 0, "universo": len(antes["perguntas"])})
        self.assertEqual(fatos["sem_contagem_na_trilha"], [])

    def _formar_grupo_ancorado(self):
        irma, ancora = self._irma_com_luz(), self._escape("hora")
        self._responder(irma)
        with transaction.atomic():
            P.trava_do_colaborador(self.colab.pk)
            irma.refresh_from_db()
            validacao._carimbar_validacao(irma, self.admin)
        return ancora

    def test_ancora_unica_recusada_e_atestado_rejeitado(self):
        with medida("grupo única âncora recusada") as fatos:
            self._irma_com_luz()
            ancora = self._escape("hora")

            def responder_ancora():
                self.relogio.move_to(a1c.T2)
                self._responder(ancora)

            recusadas, travas = self._passagem(ancora, responder_ancora)
            fatos.update(recusadas=recusadas, travas=travas, ancora_pk=ancora.pk)
            self._medir_rejeicao(fatos)

    def test_escolha_sem_ancora_e_atestado_rejeitado(self):
        with medida("grupo escolha sem âncora") as fatos:
            escape = self._escape()
            self._irma_muda()
            recusadas, travas = self._passagem(escape)
            fatos.update(recusadas=recusadas, travas=travas, escape_pk=escape.pk)
            self._medir_rejeicao(fatos)

    def test_ancora_retirada_e_atestado_rejeitado(self):
        with medida("grupo âncora retirada após formação") as fatos:
            ancora = self._formar_grupo_ancorado()
            recusadas, travas = self._passagem(ancora)
            fatos.update(recusadas=recusadas, travas=travas, ancora_pk=ancora.pk)
            self._medir_rejeicao(fatos, retirar_pk=ancora.pk)

    def test_rejeicao_com_falha_na_auditoria_da_devolucao(self):
        with medida("trilha falha no INSERT da devolução") as fatos:
            ancora = self._formar_grupo_ancorado()
            recusadas, travas = self._passagem(ancora)
            fatos.update(recusadas=recusadas, travas=travas, ancora_pk=ancora.pk)
            self._medir_rejeicao(fatos, falhar_auditoria=True)
```

A retirada usa `reabrir_para_correcao`. A rejeição usa `decidir` e observa o retorno do callback real. O caso extra provoca a violação `NOT NULL` somente no evento da devolução, dentro do savepoint real da auditoria. As fotos registram os retornos e os estados antes das asserções de negócio. `app/chamados/services/disputa_emissao.py:1597–1630,1664–1685` · `app/ponto/services/ausencia.py:1057–1088` · `app/ponto/signals.py:250–279` · `app/core/observ.py:124–139` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:189–220,482–508`

Para produzir o comprovante por SHA, rodar a p

[truncado em 20000 de 33066 caracteres]

## 20/11 18:26 · sessao 03ad11b31d75 · E4C-F5-CONSTROI · leitura · origem=Code

| Item | Estado | Prova na cópia ou execução devida |
|---|---|---|
| 01. Matriz e censo de entrada | FALTA | O RELATO registra **15 sítios, amostra 9/9 e 0 perguntas de lei** na base `3308f7599`. A matriz nominal para recontar os 15 e cruzá-los com o selo e o PRONTO está [nao verificado]. `app/docs/RELATO.md:158`, `bin/codex_pergunta.sh:241–247`. |
| 02. Testemunha da leitura | DEMONSTRADO | `leitura` retorna `LeituraDoAto` de **3 campos**. `composta` retorna `LeituraComposta` de **5 campos**. `app/chamados/models.py:19–25,4348–4390`. |
| 03. Conservação do passivo sem ato | DEMONSTRADO | A leitura incorpora as linhas sintéticas da projeção. O teste fixa os **2 juízes** e os cortes temporais do passivo. `app/chamados/models.py:4291–4323,4355–4379`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:59–75`. |
| 04. RED-5 do histórico | DEMONSTRADO | O teste cobre **3 eventos**, resposta, validação e materialização, e contém o mutante `historico`. `app/chamados/services/historico.py:96–111`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:75–103`. |
| 05. SILÊNCIO e dono ADMIN | RETIRADO-POR-AVAL | RESPOSTAS-2011-1255 item 3: “o SILENCIO dono admin SAI da F5 e vira fatia propria E4C-F5-SILENCIO-ADMIN”. O teste de regressão permanece na cópia. `avais_posteriores.txt:129–131`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:343–344`. |
| 06. Catraca de leitores crus | FALTA | Recontadas por AST **13 funções em 8 arquivos, 0 acusações**. Há inspeção de `getattr` e `refresh_from_db`. `update_fields` aparece no controle de escrita permitida. Faltam a inspeção nominal de `update_fields` e o mapa dos 15. Catraca executada em `execucoes.txt:16`. `app/chamados/tests/test_e4c_f5_selo_catraca.py:11–26,174–197,291–307,314–369`. |
| 07. Três passagens | SO POR EXECUCAO | Passagens A/B/C, em ordem e por hash, com a exceção do chokepoint na A. **EXECUCAO NAO LISTADA**. `bin/volta_intermediaria.sh:4,32`, `avais_posteriores.txt:3–5,10–13`. |
| 08. Ordem, commits e RED antes da cura | SO POR EXECUCAO | Histórico A/B/C, RED na base de cada cura e pureza no primeiro commit da A. **EXECUCAO NAO LISTADA**. `app/docs/RELATO.md:27–28`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:271–285`. |
| 09. Verificador do delta por volta | SO POR EXECUCAO | Triagem da volta 3/c3 listada, **3 procedentes curados e 7 descartados com motivo**. Deltas A/B/C e respectivas triagens: **EXECUCAO NAO LISTADA**. `execucoes.txt:19`, `app/docs/RELATO.md:94–95,131,134`. |
| 10. Mutantes tira-lote mortos | SO POR EXECUCAO | **33 seletores, 9 A + 14 B + 10 C**. Saídas nominais, inclusive K16 e K47: **EXECUCAO NAO LISTADA**. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–21`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`. |
| 11. Sombra dos juízes e do passivo | SO POR EXECUCAO | Antes × depois por volta, classe e passivo sem ato: **EXECUCAO NAO LISTADA**. A referência é **40 + 213 = 253**. O RELATO anterior registra que as 213 divergências de SILÊNCIO conservaram o par do juiz. `app/chamados/juizes.py:1383–1403,1551–1560`, `app/docs/RELATO.md:393,397`. |
| 12. Sombra das decisões por porta | SO POR EXECUCAO | Comparativo de validar, recusar e materializar pela função real: **EXECUCAO NAO LISTADA**. `app/chamados/services/validacao.py:677–704,875–923`, `app/chamados/services/materializacao.py:1318–1336,2106–2162`. |
| 13. Entregues e exportadas | SO POR EXECUCAO | Lista ou zero de decisões alteradas nesse recorte: **EXECUCAO NAO LISTADA**. `app/chamados/services/validacao.py:225–231,692,757–759`, `app/chamados/services/materializacao.py:815–847,1900–1904`. |
| 14. CHECK-DE-TELA | SO POR EXECUCAO | **4 superfícies**: espelho, Central, tarefa pelo serviço e API. Comparativo de resultado, consultas e escritas contra a referência: **EXECUCAO NAO LISTADA**. `app/ponto/urls.py:10`, `app/chamados/views.py:721`, `app/chamados/services/tarefa_do_dia.py:825–830`, `app/api/urls.py:92`, `app/docs/RELATO.md:188`. |
| 15. Verificador inteiro, zero bloqueios | SO POR EXECUCAO | O teste da volta final cobre a invalidação do memo C6. A triagem c3 está listada. Laudo de escopo inteiro com matriz, predesenho, avais e **0 BLOQUEIA**: **EXECUCAO NAO LISTADA**. `app/chamados/models.py:4439–4450`, `app/chamados/tests/test_e4c_f5_voltaFINAL.py:12–13`, `app/docs/RELATO.md:94`, `execucoes.txt:19`. |
| 16. Suíte inteira pré-Opus | SO POR EXECUCAO | Listada para `c8eb8eddf` às 16:54: `rc=0`, **17.268 testes**, **42 skips**, **41 falhas esperadas**, **2.303,169 s**. Pista 2 e precedência da guarda verde e da publicação do PRONTO [nao verificado]. `execucoes.txt:2`, `avais_depois_do_vPRONTO4.txt:7`, `bin/suite.sh:23–29,236–257`. |
| 17. Opus final único | SO POR EXECUCAO | Laudo único no hash da pré-Opus, com triagens e isenção de pureza: **EXECUCAO NAO LISTADA**. `app/docs/RELATO.md:45,94`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:163–185`, `avais_posteriores.txt:31,168`. |
| 18. Compositor nos três sítios | DEMONSTRADO | **1 compositor tipado, 3 delegações**: partição do app, property `respondida` e fio. Instantes do ato e texto da projeção. `app/chamados/models.py:24–25,3838–3844,4382–4390`, `app/chamados/services/pergunta_no_app.py:80–92`, `app/chamados/services/fio.py:187–190`. |
| 19. Grep publicado | FALTA | Falta a saída nominal do compositor e das delegações no RELATO. `app/docs/RELATO.md:34–35`, `app/chamados/models.py:3844,4382–4390`. |
| 20. RED da partição do app | SO POR EXECUCAO | Verde geral listado. Mordidas dos **2 mutantes**, `a1_app_coluna` e `a1_app_texto_do_ato`: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:32–53`. |
| 21. RED da property | SO POR EXECUCAO | Verde geral listado. RED da property com texto só no ato e projeção carimbada após reabertura: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_e4c_f5_opus_models.py:37–54,87–89`. |
| 22. RED do fio | SO POR EXECUCAO | Verde geral listado. Mordidas dos **3 mutantes**, `a1_fio_coluna`, `a1_fio_texto_do_ato` e `a1_fio_composicao_local`: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–71`. |
| 23. RED de `sem_veredito_entre` | SO POR EXECUCAO | Verde geral listado. RED na base do caso de texto e instante divergentes: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/models.py:2022–2027`, `app/chamados/tests/test_e4c_f5_opus_models.py:76–81`. |
| 24. RED de `estado` | SO POR EXECUCAO | Verde geral listado. RED na base do caso que espera `PENDENTE`: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/models.py:3249–3268`, `app/chamados/tests/test_e4c_f5_opus_models.py:83–85`. |
| 25. Consumidores por consequência | DEMONSTRADO | **3 caminhos** chegam ao leitor comum: fechamento, CSF e Pauta. Preservação histórica dos arquivos por diff [nao verificado]. `app/chamados/models.py:1998–2027,2035,2072`, `app/ponto/supra_juiz.py:573–574`, `app/chamados/services/competencia_trancada.py:124–126`. |
| 26. Vigia de texto em lotes | DEMONSTRADO | `iterator(chunk_size=lote)` com prefetch de decisões e padrão de **2.000** por lote. `app/chamados/services/vigia_texto_resposta.py:8–25`. |
| 27. Contagem preservada no vigia | SO POR EXECUCAO | **4 pares** no teste: `(0,2)`, `(1,2)`, `(1,1)`, `(1,3)`. Verde geral listado. Comparação antes × depois com a mesma fixture: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_e4c_f5_contador_texto.py:26–38`. |
| 28. RED de consultas do vigia | SO POR EXECUCAO | **3 SELECTs** de decisões para **5 perguntas** em lotes de **2**. Verde geral listado. RED nominal na base: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_e4c_f5_contador_texto.py:87–95`. |
| 29. Remoção da chave sem leitor | DEMONSTRADO | O carimbo mantém `divergencia_texto_resposta` e o teste cobra a ausência de `divergencia_texto_resposta_universo`. Identidade do commit [nao verificado]. `app/chamados/management/commands/vigiar_fio_morto.py:264–284`, `app/chamados/tests/test_e4c_f5_contador_texto.py:56–70`. |
| 30. Decisão técnica publicada | DEMONSTRADO | O RELATO registra a remoção escolhida, o motivo e a reversão. `app/docs/RELATO.md:37`. |
| 31. Selo AST por identidade | DEMONSTRADO | O selo acompanha parâmetro, aliases e alvo de iteração. **2 testes** de renomeação e mutante que renomeia o parâmetro. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:164–185,220–251`, `app/chamados/tests/e4c_f5_volta2_mutantes.py:11–38`. |
| 32. Triagem de `casefold` | FALTA | Falta a linha motivada para o Opus final. A cópia tem o caso de caixa e registros de triagens anteriores. `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `app/docs/RELATO.md:29,32,94`. |
| 33. PRONTO publicado | FALTA | Falta no topo do RELATO a linha “PRONTO E4C-F5 item a item”, com provas e faltas desta árvore. `app/docs/RELATO.md:1–42,158`, `avais_depois_do_vPRONTO4.txt:5,108–110`. |
| 34. Pureza AST dos dois juízes | DEMONSTRADO | **2 juízes, 0 acusações** na leitura AST, **1 isenção nominal** no temporal e **5 variantes** de forma, localização ou quantidade. Identidade textual com a base [nao verificado]. `app/chamados/juizes.py:1383–1403,1551–1560`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:163–185,271–304`. |
| 35. Censo dos carregadores e mapa K | FALTA | Recontadas **77 cargas explícitas de decisões em 66 escopos e 39 arquivos**, **75** de queryset + **2** por helper. **33 K identificáveis**: 9 A + 14 B + 9 C + K13. Falta o vínculo nominal dos **69 K** à conta avalizada de **63 não-R + 6 R**, **58 pontos + sobra**. `app/chamados/models.py:2020–2021`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–21`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264`. |
| 36. Seis R declarados | FALTA | **6 rótulos** nos avais: K3, K19, K20, K22, K40 e K58. A cópia liga K40 ao percurso avulso. Faltam os outros **5 vínculos** e a declaração por sítio. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:199–207`. |
| 37. K40 por item | DEMONSTRADO | `get(pk=pk)` e julgamento de cada candidata em `_descarimbados`. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`. |
| 38. Sobra por grupo | DEMONSTRADO | **1 sobra**, deduplicação, com prefetch por grupo, teste 3 × 6 e seletor de mutação. `app/chamados/management/commands/dedup_perguntas_fato.py:75–76`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:230–237`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:22`. |
| 39. REDs 3 × 6 da A | SO POR EXECUCAO | **11 casos 3 × 6, 9 K distintos**, mais O261 por candidata. O caso do fio é **1 × 5**. Verde geral listado. RED e verde da passagem A: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:44–240`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:154–186`. |
| 40. REDs e mutantes da B | FALTA | **15 métodos, 14 K, 14 tira-lote**, K46 em dois métodos, mais **5 mutantes funcionais**. Falta tabela nominal dos **29 K cobertos**, **27 editados na B**, K41/K42 no commit Opus e **5 L + 3 Q**. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:40–251`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–52`. |
| 41. REDs e mutantes da C | FALTA | **10 métodos 3 × 6**, **9 K + 1 sobra**, **10 tira-lote** e **1** caso adicional K13 de releitura. Falta tabela que vincule os K de manutenção, as **3 M** e a sobra. `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:74–264`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`. |
| 42. RED novo 3 × 6 do C11 | DEMONSTRADO | **2 casos**, K2 e K1, com SELECTs de decisões, PKs julgados e PKs devolvidos pelo C11. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:23–40,207–240`. |
| 43. Tetos existentes E2 e C11 | SO POR EXECUCAO | **2 selos** presentes, E2 com 1 colaborador/1 dia × 2/3, C11 com 1 × 5 dias. Verde geral listado. Diff de preservação dos tetos: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_e2_tarefa_do_dia.py:913–938`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:378–402`. |
| 44. O261 por candidata, teto 9 | SO POR EXECUCAO | **2 testes** fixam igualdade 3 × 6 por candidata, teto **9** e **1 SELECT** de decisões por candidata. Verde geral listado. Saída nominal da remedição: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–207`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:188–204`. |
| 45. Tabela de selos remedidos | FALTA | Falta publicação consolidada de antes/depois, motivo e rodada por selo. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:205–207`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:578–593`. |
| 46. Quatro tetos do chokepoint | DEMONSTRADO | **4 tetos anotados**, **230→234**, **145→146**, **294→298**, **219→221**, com sítios, commits e classe de leitura. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,248–253,260–267`. |
| 47. Quatro casos verdes do chokepoint | SO POR EXECUCAO | **4 testes**, 1 e 3 furos × fecha e fica. Verde geral listado. Remedição nominal dos quatro: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/ponto/tests/test_selo_chokepoint_escrita.py:467–475,502–510`. |
| 48. Atribuição SQL e escritores novos | SO POR EXECUCAO | O selo anota delta de leitura e **0** INSERT/UPDATE/DELETE/SAVEPOINT a mais na comparação que cita. Captura final por SQL, verbo e sítio: **EXECUCAO NAO LISTADA**. Publicação da tabela no item 45. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–205,220–229,248–252,260–266`, `avais_posteriores.txt:4,11–13`. |
| 49. Harness dentro das exceções autorizadas | FALTA | **9 módulos**, asserts recontados: **0, 31, 86, 134, 29, 7, 65, 122 e 12**. Falta mapa das **11 famílias** e vínculo de cada diff à exceção que o autoriza. `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35,76–80`. |
| 50. Onze famílias verdes | SO POR EXECUCAO | **11 métodos** em `test_pergunta_viva` e **6 casos** no selo de dublê. Verde geral listado. Placar nominal por família: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_pergunta_viva.py:16–76`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:51–87`. |
| 51. Décima segunda família | FALTA | As **2 fixtures RED-8** estão presentes. Falta nomeá-las no inventário das **12 famílias**. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:119–186`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35`. |
| 52. Medida de `respondida` | SO POR EXECUCAO | Comparação da property nos dois sentidos, **51.563 e 0/0** como referência anterior: **EXECUCAO NAO LISTADA** para esta árvore. `lista_101_do_vPRONTO4.txt:54`, `app/chamados/models.py:3838–3844`. |
| 53. Medida de `sem_veredito_entre` | SO POR EXECUCAO | Comparação da decisão, **0 em 51.563** como referência anterior: **EXECUCAO NAO LISTADA** para esta árvore. `lista_101_do_vPRONTO4.txt:55`, `app/chamados/models.py:2002–2027`. |
| 54. Medida de `estado` | SO POR EXECUCAO | Comparação de rótulos, **0 em 51.563** como referência anterior: **EXECUCAO NAO LISTADA** para esta árvore. `lista_101_do_vPRONTO4.txt:56`, `app/chamados/models.py:3249–3268`. |
| 55. Conversão dos 96 chamadores | FALTA | AST atual: **44 chamadas diretas**, **40** de respondida e **4** temporais, **43 compostas + 1 exceção**. A1(c) tem dois usos identificáveis. Falta matriz histórica **41 diretos + 55 indiretos = 96**, por volta. `app/chamados/juizes.py:1406–1428`, `app/chamados/services/materializacao.py:1060–1065`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160`. |
| 56. Selo de chamada final em zero | DEMONSTRADO | **0 chamadas acusadas** na varredura atual. **1 exceção nominal**, a face `values_list` de `creditos_perguntas`. Catraca listada. `app/chamados/tests/test_e4c_f5_selo_chamada.py:20–28,126–160,188–193`, `app/chamados/services/raiox.py:135–147`, `execucoes.txt:16`. |
| 57. Catraca publicada por volta | SO POR EXECUCAO | Catraca final listada. Série base Opus/A/B/C, com hash e identidade em cada rodada: **EXECUCAO NAO LISTADA**. `execucoes.txt:16`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160,188–193`. |
| 58. Commit Opus de `models.py` | SO POR EXECUCAO | **9 métodos**, dos quais **6 REDs** nas âncoras históricas 357, 1542, 2004, 2057, 3235 e 3791. Verde geral listado. Commit próprio anterior à A, mapa K41/K42 e REDs na base: **EXECUCAO NAO LISTADA**. `execucoes.txt:2`, `app/chamados/tests/test_e4c_f5_opus_models.py:57–167`, `bin/codex_pergunta.sh:126,346`. |
| 59. Limite da exceção de folha | SO POR EXECUCAO | **2 sítios** atuais, carga e argumento do juiz. Diff com a base para provar o limite das edições: **EXECUCAO NAO LISTADA**. `app/folha/previa.py:64–67,139–146`, `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:71–95`. |
| 60. Prévia da competência 11 | SO POR EXECUCAO | MAIN × cópia, por colaborador e dia: **EXECUCAO NAO LISTADA**. **4 empresas, 450 colaboradores, 5.861 linhas, 2 referências, 0 diferenças** são os números de referência da C5. `lista_101_do_vPRONTO4.txt:62`, `app/folha/previa.py:64–67,135–146`. |
| 61. Fixtures dos dois RED-8 | DEMONSTRADO | **2 casos**, ato pela porta e projeção passiva. A fixture do fio chega à **5ª pergunta**. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:27–35,119–186`. |
| 62. Chamadas ORM da volta 2 | DEMONSTRADO | Os casos de fonte passam `D.composta(p)`. O passivo usa ORM cru como controle. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37–117`. |
| 63. Mutante mira o compositor | DEMONSTRADO | O mutante renomeia o parâmetro e troca a chamada ao compositor pelo ORM cru. `app/chamados/tests/e4c_f5_volta2_mutantes.py:11–38`. |
| 64. Tabela F2 por leitor | DEMONSTRADO | **7 leitores**, **5 `False`**, **2 `True`**, com motivo por leitor. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:498–521`. |
| 65. Instante e validação pelo ato | DEMONSTRADO | **2 casos** opõem o ato à projeção. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:523–544`. |
| 66. Leitor F1 B2 | DEMONSTRADO | O teste exige **1 chamada** e o tipo `LeituraComposta`. `app/chamados/tests/test_f1nova_b2_leitores.py:87–97`. |
| 67. `_ancoradas` | DEMONSTRADO | **2 consultas** com **1** e com **5** ancoradas. O selo verifica a carga e o prefetch. `app/chamados/models.py:349–366`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:569–593`. |
| 68. Inventário das adaptações | FALTA | A lista anterior agrupa **7** tipos de alteração. O cadastro C12 tem **9 identidades** no trecho indicado. Falta inventário por uso, antes/depois e aval, inclusive classes v, vi e vii. `app/chamados/tests/e4b_c12_ast.py:18–27`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:76–80`, `avais_posteriores.txt:23`. |
| 69. Placar raio-X pela face SQL | DEMONSTRADO | A exceção preserva `values_list` da projeção e adapta a tupla ao juiz. `app/chamados/services/raiox.py:135–147`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28`. |
| 70. Registro nominal do raio-X | FALTA | Falta a entrada nominal do raio-X na E4D. `app/docs/BACKLOG.md:2023`, `app/chamados/services/raiox.py:135–147`. |
| 71. `resposta_display` lê o ato | DEMONSTRADO | A property formata para exibição o texto lido do ato. O selo F2 fixa a expectativa. `app/chamados/models.py:3865–3891`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:511–521`. |
| 72. Registro nominal de `resposta_display` | FALTA | Falta a entrada nominal da property na E4D. `app/docs/BACKLOG.md:2023`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:511–514`. |
| 73. Medida vigente de display | SO POR EXECUCAO | **16 PKs distintos** no literal do 0545, **16/51.563** na referência anterior. O aval aceita os dois zeros normalizados daquela medida. Comparação aplicável à árvore final: **EXECUCAO NAO LISTADA**. Renderização nominal no 84. `avais_posteriores.txt:29`, `lista_101_do

[truncado em 20000 de 52655 caracteres]

## 20/11 18:21 · sessao 4aaba5418ba9 · - · - · origem=Ronald (tmux)

77–101 somam **8 DEMONSTRADO, 10 SO POR EXECUCAO e 7 FALTA** pela classificação abaixo. O repositório Git e os logs de execução estão [nao verificado].

| Item | Estado | Prova ou pendência |
|---|---|---|
| 77 | DEMONSTRADO | Censo recontado em 9 escopos e 14 ocorrências, 7 atribuições, 5 `save`, 2 descarimbos em lote. Zero acusações na leitura AST em memória. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:34–44,434–465`. |
| 78 | FALTA | O selo novo pega `VALIDACAO` sem coluna, inclusive sem menção à coluna. O filtro `ato != 'validacao'` deixa de fora o caso geral de ato que desfaz a validação com `UPDATE` que omite a coluna. O selo original parte da escrita da coluna. FALTA-CODIGO. `app/chamados/tests/test_e4c_f5_volta3_selos.py:12–29,41–70`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:434–440`. |
| 79 | SO POR EXECUCAO | Suite final em `execucoes.txt:2`. Três mutantes E geram uma acusação AST cada em memória. Morte nominal Django desses e de `v_ato_sem_coluna` EXECUCAO NAO LISTADA. `app/chamados/tests/e4c_f5_voltaE_mutantes.py:6–26`, `app/chamados/tests/test_e4c_f5_volta3_selos.py:41–70`. |
| 80 | FALTA | K47 e D afirmam o par com fixture pela porta. D tem 18 métodos, 16 REDs e 14 seletores, 11 leitores e 3 de carga. A tabela da volta 3 acrescenta 3 seletores que mexem na equivalência. Morte nominal EXECUCAO NAO LISTADA. FALTA-PUBLICACAO do esperado substituído da classe vii, corpo do commit [nao verificado]. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:35–67,84–398`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:13–46`, `app/chamados/tests/e4c_f5_volta3_mutantes.py:53–64`. |
| 81 | DEMONSTRADO | Dois contadores, lote 2.000, esperado zero e dono ESTRUTURA. O carimbo publica os dois. `app/chamados/services/vigia_ato_x_coluna.py:5–25`, `app/chamados/management/commands/vigiar_fio_morto.py:194–199,233–234,274–283`. |
| 82 | SO POR EXECUCAO | Frota final informada em 51.953, A=0, B=0, tabela 51.953. `execucoes.txt:17`, `app/chamados/services/vigia_ato_x_coluna.py:8–25`. |
| 83 | SO POR EXECUCAO | Mordida `retry_reativa` em `test_saida_primeiro_par_inteiro_retry_idempotente` EXECUCAO NAO LISTADA. `app/chamados/tests/bug_par_mutantes.py:68–71`, `app/chamados/tests/test_bug_par_volta.py:61–63`, `app/chamados/services/validacao.py:698–704`. |
| 84 | SO POR EXECUCAO | Comparação MAIN × cópia do texto renderizado nos 16 PKs distintos do 0545 EXECUCAO NAO LISTADA. `app/chamados/models.py:3865–3891`, `app/templates/chamados/partials/_pergunta_card.html:31`, `app/templates/chamados/partials/_card_atendimento.html:106,143,186,394`. |
| 85 | FALTA | FALTA-PUBLICACAO da fila E4D para constante de módulo e 9 harnesses. `app/chamados/tests/test_e4c_f5_selo_chamada.py:168–171`, `app/docs/BACKLOG.md:2023`. |
| 86 | DEMONSTRADO | Devolução por ato ou coluna, com releitura e controles de A, B, prefetch velho e validação nos dois lados. `app/chamados/services/validacao.py:267–296`, `app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:69–144`, `app/chamados/tests/test_e4c_f5_selo_catraca.py:21–23,298–307`. |
| 87 | SO POR EXECUCAO | 3 REDs, 1 controle e 4 seletores. Pré-Opus `execucoes.txt:2`. Mortes EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_item1_devolucao_ato_ou_coluna.py:69–144`. |
| 88 | SO POR EXECUCAO | 4 REDs e 2 seletores `a1_linhas_antes_da_trava`, `a1_refresh_sem_destino`. Pré-Opus `execucoes.txt:2`. Mortes, RED na base e delta desse recorte EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:38–94,97–109,114–168`. |
| 89 | DEMONSTRADO | Releitura da irmã sob a trava, relógio comum, proteção da resposta completa depois de T1, trilha e grupo legado. `app/chamados/services/materializacao.py:1036–1112,1134–1167`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:142–187,271–322,482–536`. |
| 90 | SO POR EXECUCAO | `a1c_sem_releitura` troca a consulta pela candidata e o RED com resposta T2 exige preservação. `a1c_ignora_ancora` também consta da tabela. Mortes EXECUCAO NAO LISTADA. `app/chamados/tests/e4c_f5_volta3_mutantes.py:25–30`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:142–169`, `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:102–126`. |
| 91 | SO POR EXECUCAO | 15 testes próprios, 12 RED e 3 outros. Na volta 3 há 9 adicionais, 5 RED. Pré-Opus `execucoes.txt:2` e triagem C3 `execucoes.txt:19`. RED na base e laudo próprio EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:142–536`, `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:61–244`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88–104`. |
| 92 | DEMONSTRADO | FALTA aprovada, dependente sem carimbo, trilha identificada, tarefa sem ação e teste de reentrada após decisão da âncora. `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:240–269`, `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:45–100`. |
| 93 | FALTA | FALTA-PUBLICACAO de corrida entre irmãs, leitor de ABSORCAO, âncora fora e retorno ignorado. `app/docs/BACKLOG.md:2023`, `app/chamados/services/materializacao.py:1134–1167`, `app/chamados/services/disputa_emissao.py:1566`. |
| 94 | FALTA | FALTA-PUBLICACAO dos 4 achados da E4D, empate automático e janelas de validar, recusar, corrigir. `app/docs/BACKLOG.md:2023`, `app/chamados/services/disputa_emissao.py:1689`, `app/chamados/services/validacao.py:677,875`, `app/chamados/services/acoes_disputa.py:306`. |
| 95 | FALTA | FALTA-PUBLICACAO da linha de validação física e backfill. `app/docs/BACKLOG.md:2023`, `app/chamados/models.py:4393–4399`, `avais_posteriores.txt:69`. |
| 96 | SO POR EXECUCAO | Seis pontos pedidos no 1230, definição da seção 5 e recorte restante após corte do S1 [nao verificado]. EXECUCAO NAO LISTADA. `avais_posteriores.txt:117,129–131`. |
| 97 | DEMONSTRADO | Guarda comum devolve falso antes de persistir e restaura a foto em memória. `app/chamados/models.py:3384–3414`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:515–532`. |
| 98 | SO POR EXECUCAO | Oito chamadores atuais em `app/chamados/reconciliador.py:408,646`, `app/chamados/models.py:3560`, `app/chamados/services/materializacao.py:1073,2015,2220`, `app/chamados/services/disputa_emissao.py:2058,2132`. REDs e seletores conforme 1400/1545/1700. Mortes A2 `execucoes.txt:10–11`. Morte de `tirar_guarda` EXECUCAO NAO LISTADA. `app/chamados/tests/e4c_f5_volta3_mutantes.py:65–76`. |
| 99 | DEMONSTRADO | S143 executa recusa e reabertura no mesmo PK. S144 ficou com recusa, trilha e detector por aval. Órfã ficou inalcansável pelo seletor por aval. `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:90–137`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:158–204`, `app/chamados/reconciliador.py:1096–1101`, `avais_depois_do_vPRONTO4.txt:92,107`. |
| 100 | DEMONSTRADO | Trilha usa `recusa_do_carimbo`, teste exige falso com `par_ato_coluna`, foto preservada e recusa T3 no mesmo PK. `app/chamados/services/materializacao.py:1073–1078`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:433–480`. |
| 101 | FALTA | FALTA-PUBLICACAO do corte no topo, linha da fatia S1 e quatro perguntas no caderno. `app/docs/RELATO.md:1–42`, `app/docs/BACKLOG.md:2023–2032`, `app/docs/CORTES.md:561`, `avais_posteriores.txt:129–131`. |

## 20/11 18:20 · sessao c47a3d4594b6 · - · - · origem=Ronald (tmux)

Itens 35–76 conferidos por leitura e AST, com 14 demonstrados, 17 só por execução e 11 faltas de publicação. No item 59, classifiquei o diff histórico de folha como execução de verificação pendente. A árvore mostra os dois sítios permitidos. `app/folha/previa.py:64–67,139–146`, `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:71–95`.

- **DEMONSTRADO** — 37, 38, 42, 46, 56, 61–67, 69, 71. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`, `app/chamados/management/commands/dedup_perguntas_fato.py:74–76`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:207–240`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28,126–160,188–193`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37–186`, `app/chamados/tests/e4c_f5_volta2_mutantes.py:11–38`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:498–544,569–593`, `app/chamados/tests/test_f1nova_b2_leitores.py:87–97`, `app/chamados/services/raiox.py:135–147`, `app/chamados/models.py:3865–3891`.
- **FALTA-PUBLICACAO** — 35, 36, 40, 41, 45, 49, 51, 55, 68, 70, 72. O RELATO contém a entrada histórica da matriz. O BACKLOG contém a E4D sem os registros nominais de raio-X e `resposta_display`. `app/docs/RELATO.md:158`, `app/docs/BACKLOG.md:2023`. Inventários e diffs externos [nao verificado].
- **SO POR EXECUCAO** — 39, 43, 44, 47, 48, 50, 52–54, 57–60, 73–76. O verde da suíte final consta de `execucoes.txt:2`. O recorte final da catraca consta de `execucoes.txt:16`. Série histórica, diffs, sombras específicas, captura SQL final, certificação de pouso e smokes têm EXECUCAO NAO LISTADA. `app/chamados/tests/test_e4c_f5_selo_chamada.py:188–193`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267`, `bin/suite.sh:23–29,236–257`, `app/api/urls.py:92`, `app/chamados/views.py:869–880`.

Recontagens entregues ao agente principal. O censo estático atual tem 77 chamadas de prefetch explícito em 66 escopos e 39 arquivos, com 75 usos em queryset e 2 via `prefetch_related_objects`. Os testes e as tabelas de mutantes identificam 33 K distintos, com a sobra à parte. A catraca encontra 44 chamadas diretas, das quais 40 são de `pergunta_respondida` e 4 de projeção temporal. São 43 compostas, 1 exceção SQL e 0 acusações. Os 9 módulos do selo de asserts têm 0, 31, 86, 134, 29, 7, 65, 122 e 12. `app/chamados/models.py:366,2020–2024`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–21`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28,126–160,188–193`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35,76–80`.

## 20/11 18:18 · sessao 85d8e395df9e · - · - · origem=Ronald (tmux)

Itens 01–34 conferidos e enviados à consolidação. São 10 DEMONSTRADO, 18 SO POR EXECUCAO, 5 FALTA e 1 RETIRADO-POR-AVAL, total 34. A abertura e o fechamento da suíte pré-Opus estão cobertos pelo registro agregado. A sequência com a guarda e o PRONTO permanece [nao verificado]. `execucoes.txt:2`, `avais_depois_do_vPRONTO4.txt:7`. [nao verificado]

A inspeção AST em memória encontrou 13 funções em 8 arquivos, com 0 acusações. O selo exercita `getattr` e `refresh_from_db`. O único `update_fields` está no controle que aceita escrita, e a inspeção nominal exigida continua faltando. `app/chamados/tests/test_e4c_f5_selo_catraca.py:11–26,174–197,291–307,314–369`.

## 20/11 18:17 · sessao 821419e9cc03 · E4C-F5-CONSTROI · leitura · origem=Code

A leitura encontrou testes para o selo, o contador, os dois casos diretos e parte das comparações de A1(c). A corrida com dois commits independentes, a remedição da frota final e a comparação financeira em sombra seguem **SEM TESTE** para a medida pedida. O golden de entregue compara a tarefa anterior e a posterior com suas respectivas referências e exige igualdade antes/depois na lista CSF. [`secao5.txt:6,10`; `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:85–104`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:149–163`; resultados de execução **[nao verificado]**]

O **Fica** da tabela aplica as frases do 1255 “O A1 (releitura por irma no materializador) continua” e “com A=0 e B=0 remedidos na arvore final e o selo do par inteiro verde”. Para a dependente da âncora recusada, vale “fica como esta, CONTADA, e registrada na E4D”. O **Sai** aplica “o SILENCIO dono admin SAI da F5 e vira fatia propria E4C-F5-SILENCIO-ADMIN”. O mesmo item preserva “os 7 controles + golden” e remete à fatia nova a medição por K6 e C20, o botão por pergunta, o universo do contador, o retrato e a lista DP. [`respostas_2011_1255.txt:3–7`; `f5_a1c_e_silencio_q4_2011_1230.txt:9`] [nao verificado]

Os nomes de arquivo de teste abaixo são relativos a `app/chamados/tests/`. Nos testes citados como cobertura da F5, a leitura das classes, métodos e bases não mostrou marcação `skip` ou `expectedFailure`. **0 conexões** indica teste estático ou em memória e **1 conexão** indica o ORM da fixture, inclusive quando a trava é interposta ou um callback é executado explicitamente. O anexo `skips_xfail_da_suite.txt` veio vazio. SHA atestado por Git, placar e resultado efetivo de cada teste **[nao verificado]**. [`cortes_f2_f9_apoio.py:23,37`; `test_e4c_f5_selo_par_ato_coluna.py:454–575`; `test_e4c_f5_a1c_passagem_da_ausencia.py:31,85–104,211–220`; `secao5.txt:5`] [nao verificado]

| Linha da seção 5 | Sub-item | Fica/sai | Evidência (teste ou SEM TESTE) | Observação | [nao verificado]
|---|---|---|---|---|
| 1 · Identidade e execução | Carimbar o SHA com metadados Git | **Fica** · 1255, PRONTO | **SEM TESTE** · proveniência da cópia **[nao verificado]** | Em `logs/`, anexar a saída de `git rev-parse HEAD` de um checkout com metadados e ligá-la ao conteúdo testado. `c8eb8eddf898116f0dc3d7f936f194020a809e36` é o hash informado no pedido. `secao5.txt:5` |
| 1 · Identidade e execução | Anexar resultado de testes por SHA | **Fica** · 1255, PRONTO | **SEM TESTE** · artefato da execução por SHA **[nao verificado]** | Em `logs/`, anexar comando, SHA, status de saída e placar com skips e falhas esperadas. A seção 5 menciona uma falha da sonda interposta em `medida4.log:5–23` **[nao verificado]**. O anexo filtrado vazio não fornece esse placar. `secao5.txt:5` | [nao verificado]
| 2 · Par e frota final | Selo dos escritores | **Fica** · 1255, PRONTO | `test_e4c_f5_selo_par_ato_coluna.py::SeloParAtoColunaTest::test_escritores_fecham_o_par:455` | **0 conexões.** Varre a fonte de produção por AST, exige que encontre escritores e todos os sítios do censo, e que a lista de erros seja vazia. Mesmo selo tem controles de descoberta por AST, atos com foto e repasse de campos no lote. `test_e4c_f5_selo_par_ato_coluna.py:443–513` |
| 2 · Par e frota final | Contador: caminho normal e sentido A | **Fica** · 1255, PRONTO | `test_e4c_f5_contador_ato_x_coluna.py::ContadorAtoXColunaTest::test_porta_real_valida_a_pergunta_nos_dois_lados:47` e `::test_RED_a_ato_validado_coluna_vazia:55` | **1 conexão.** Validação real deixa coluna e composta no mesmo instante, com ato `VALIDACAO`, e produz `(A,B,universo)=(0,0,1)`. Coluna zerada na fixture com ato vigente produz `(1,0,1)`. `test_e4c_f5_contador_ato_x_coluna.py:21–37,47–57` |
| 2 · Par e frota final | Contador: sentido B e passivo | **Fica** · 1255, PRONTO | `test_e4c_f5_contador_ato_x_coluna.py::ContadorAtoXColunaTest::test_RED_b_coluna_preenchida_ato_devolvido:59` e `::test_passivo_sem_linha_le_validacao_sintetica_da_coluna:63` | **1 conexão.** Coluna preenchida com ato devolvido produz `(0,1,1)`. Coluna passiva sem linha do ato produz `(0,0,1)`. `test_e4c_f5_contador_ato_x_coluna.py:39–45,59–67` |
| 2 · Par e frota final | Contador: universo filtrado | **Fica** · 1255, PRONTO | `test_e4c_f5_contador_ato_x_coluna.py::ContadorAtoXColunaTest::test_recorte_do_chamador_define_o_universo:69` | **1 conexão.** Asserções de `(1,1,3)` no conjunto, `(0,1,1)` só no PK de B e `(0,0,0)` na interseção vazia. `test_e4c_f5_contador_ato_x_coluna.py:69–76` |
| 2 · Par e frota final | Contador: publicação e custo por lote | **Fica** · 1255, PRONTO | `test_e4c_f5_contador_ato_x_coluna.py::ContadorAtoXColunaTest::test_vigia_carimba_a_e_b_sem_gritar_com_alarme:78` e `::test_RED_decisoes_em_lotes_de_chunk:99` | **1 conexão.** O vigia imprime `A=1 B=2` e `universo=3`, com `_carimbar` interceptado. Cinco perguntas em lotes de dois exigem cinco chamadas a `composta` e três cargas SQL de decisões. `test_e4c_f5_contador_ato_x_coluna.py:78–106` |
| 2 · Par e frota final | Direto 1: `carimbar` com resposta posterior | **Fica** · 1255, item 1 | `test_e4c_f5_selo_par_ato_coluna.py::SeloParAtoColunaTest::test_carimbo_sintetico_preserva_equivalencia_com_resposta_mais_recente:515` | **0 conexões.** Com decisões em memória, concordância com a luz simulada e `save` interceptado, exige `gravou == salvar.called` e igualdade da presença de validação na composta e na coluna. A recusa explícita `False`, sem salvar, está na guarda e nos testes dos chamadores. `test_e4c_f5_selo_par_ato_coluna.py:517–532`; `app/chamados/models.py:3384–3405` |
| 2 · Par e frota final | Direto 2: reabertura em lote de validação só no ato | **Fica** · 1255, PRONTO | `test_e4c_f5_selo_par_ato_coluna.py::ReaberturaCompensaOAtoTest::test_RED_reabertura_em_lote_compensa_validacao_so_no_ato:550` | **1 conexão.** Parte de coluna nula e ato válido. Após `reabrir_para_correcao`, composta e coluna ficam nulas, com uma `REABERTURA` que guarda o instante original. Os controles verificam pendente sem linha e validada pela coluna com uma única reabertura. `test_e4c_f5_selo_par_ato_coluna.py:540–575` |
| 2 · Par e frota final | Falso tratado por S143 | **Fica** · 1255, item 1 | `test_e4c_f5_guarda_par_s143.py::GuardaParS143Test::test_RED_destino_recusa_b_persistido_e_a_mesa_recusa_o_mesmo_pk:90` e `::test_RED_sem_credor_recusa_b_persistido_e_titular_reabre_o_mesmo_pk:119` | **1 conexão.** A tentativa retorna falso com motivo do par, preserva foto e atos e deixa a disputa aberta. A mesa recusa ou o titular reabre o mesmo PK, chegando ao par `(False,False)`. `test_e4c_f5_guarda_par_s143.py:69–137` |
| 2 · Par e frota final | Falso tratado pelo plantio passivo | **Fica** · 1255, item 1 | `test_e4c_f5_guarda_plantio_s144.py::GuardaPlantioS144Test::test_RED_passivo_foto_velha_recusa_par_e_mesa_valida_mesmo_pk:146` e `::test_RED_passivo_b_persistido_recusa_par_e_mesa_valida_mesmo_pk:152` | **1 conexão.** Falso registrado como recusa do par, zero plantio e resposta preservada. Validação posterior pela mesa opera no mesmo PK e chega a `(True,True)`. `test_e4c_f5_guarda_plantio_s144.py:93–156` |
| 2 · Par e frota final | Falso tratado pela absorção | **Fica** · 1255, item 1 | `test_e4c_f5_volta3_ausencia.py::AusenciaVolta3Test::test_RED_absorcao_recusa_coluna_persistida_e_a_mesa_recusa_mesmo_pk:207` | **1 conexão.** Exige falso com motivo do par, PK em `recusadas`, trilha, ausência de novo ato ou carimbo e disputa aberta. A recusa posterior pela mesa limpa o par no mesmo PK. `test_e4c_f5_volta3_ausencia.py:207–245` |
| 2 · Par e frota final | Classe vii: custo de 1 × 5 ancoradas | **Fica** · 1255, PRONTO | `test_e4c_f2_leitura_do_ato.py::AncoradasCustoTest::test_RED_ancora_em_subquery_de_pk_uma_query:569` | **1 conexão.** O comentário identifica o item 7-vii. São **2 consultas em cada recorte**, carga e decisões. Compara os PKs retornados, exclui a validada só pelo ato e exige subquery de PK na carga. `test_e4c_f2_leitura_do_ato.py:560–593`; `app/chamados/models.py:349` |
| 2 · Par e frota final | `retry_reativa` | **Fica** · 1255, PRONTO | `test_bug_par_volta.py::BugParVoltaTest::test_saida_primeiro_par_inteiro_retry_idempotente:61` | **1 conexão.** Valida saída primeiro, repete a validação três vezes, conserva o número de atos e exige o par gravado com as duas batidas esperadas. `retry_reativa` é o seletor do mutante que desliga a guarda `ja_validada`. Sua execução e morte seguem **[nao verificado]**. `test_bug_par_volta.py:45–67`; `test_bug_par_projetado.py:99–110`; `bug_par_mutantes.py:21,68–71,201–239`; `app/chamados/services/validacao.py:697–704` |
| 2 · Par e frota final | A1(c): resposta humana, automática e já validada depois de T1 | **Fica** · 1255, item 1 | `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_RED_resposta_t2_concordante_fica_fora_da_passagem:142`, `::test_resposta_automatica_t2_tambem_fica_fora:170` e `::test_RED_resposta_t2_ja_validada_conserva_resposta_e_assinatura:271` | **1 conexão.** T2 fica preservado e o PK sai da passagem nos três casos. No terceiro, a validação T3, seu autor e o carimbo da batida ficam preservados. A interposição roda a ação concorrente em linha, antes de chamar a trava real. `test_e4c_f5_a1c_passagem_da_ausencia.py:85–104,142–187,271–311` |
| 2 · Par e frota final | A1(c): empate em T1 e recusa anterior à trava | **Fica** · 1255, item 1 | `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_RED_empate_com_t1_fica_na_passagem:313` e `::test_RED_recusa_antes_da_trava_muda_a_via:324` | **1 conexão.** Empate entra em `VIA_DECLARACAO`, sem recusa. Com `RECUSA` em T2 antes da releitura, resposta fica vazia e a absorção T1 usa `VIA_AUSENCIA_DIA`. `test_e4c_f5_a1c_passagem_da_ausencia.py:313–338` |
| 2 · Par e frota final | A1(c): duas conexões e barreiras antes da trava | **Fica** · 1255, item 1 | **SEM TESTE** · a interposição existente usa **1 conexão**. `test_e4c_f5_a1c_passagem_da_ausencia.py:85–104` | Avulso em `logs/`, com duas sessões PostgreSQL e relógio por sessão. A declara pelo `responder_pergunta_disputa_core` e pausa entre a foto T1 e `trava_do_colaborador`. B responde à irmã e confirma o commit. A retoma a trava real e termina. Registrar os dois commits e reler em consulta nova. Repetir para os controles temporais aplicáveis. `app/chamados/services/disputa_emissao.py:1439,1478–1499,1561–1566`; `app/chamados/services/materializacao.py:1036–1067`; `app/chamados/models.py:2830–2848` |
| 2 · Par e frota final | Após ambos os commits: sequência de atos | **Fica** · 1255, item 1 | **SEM TESTE** · cobertura em **1 conexão** em `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_RED_resposta_t2_concordante_fica_fora_da_passagem:142` | No avulso, reler `decisoes` por PK e instante e chamar `DecisaoPergunta.composta`. Comparar `RESPOSTA`, eventual assinatura e presença ou ausência de `ABSORCAO` por PK. `test_e4c_f5_a1c_passagem_da_ausencia.py:125–133,153–160,296–311`; `app/chamados/models.py:4382–4390` |
| 2 · Par e frota final | Após ambos os commits: coluna | **Fica** · 1255, item 1 | **SEM TESTE** · `_reler_par` compara presença e instante em **1 conexão**. `test_e4c_f5_a1c_passagem_da_ausencia.py:118–123` | No mesmo avulso, reler `validada_em` e `validada_por_id` e comparar com `composta` e `par_ato_coluna` nos mesmos PKs. `app/chamados/models.py:4382–4399` |
| 2 · Par e frota final | Após ambos os commits: `q_validada_pelo_ato` | **Fica** · 1255, item 1 | **SEM TESTE** para A1(c) nesse ponto. Controle isolado em `test_e4c_f2_leitura_do_ato.py::FaceXLeituraTest::test_face_casa_leitura_em_todas_as_perguntas_do_caso:178` | O controle em **1 conexão** confronta os PKs da face SQL com a leitura. No avulso, filtrar o mesmo conjunto de PKs com `PerguntaDisputa.q_validada_pelo_ato()` e confrontar com o estado pós-commit. `test_e4c_f2_leitura_do_ato.py:178–201`; `app/chamados/models.py:3591–3613` |
| 2 · Par e frota final | `recusadas` devolvida pela passagem e conferida após ambos os commits | **Fica** · 1255, item 1 | **SEM TESTE** para os dois commits. Cobertura em **1 conexão** em `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_RED_resposta_t2_concordante_fica_fora_da_passagem:142` | Capturar a lista no retorno do materializador real e compará-la com os PKs e atos relidos. O core público devolve `(True,None)` e a lista sai de `_materializar_ausencia_de_resposta`. `test_e4c_f5_a1c_passagem_da_ausencia.py:98–104,151–160`; `app/chamados/services/materializacao.py:1094–1115`; `app/chamados/services/disputa_emissao.py:1563–1584` |
| 2 · Par e frota final | Número final de A | **Fica** · 1255, PRONTO | **SEM TESTE** para o número real **[nao verificado]** | Script de sombra somente leitura chama `vigia_ato_x_coluna.contar()` e publica `a.numero`, com esperado **0**. `app/chamados/services/vigia_ato_x_coluna.py:8–25` |
| 2 · Par e frota final | Número final de B | **Fica** · 1255, PRONTO | **SEM TESTE** para o número real **[nao verificado]** | Na mesma fotografia, publicar `b.numero`, com esperado **0**. `app/chamados/services/vigia_ato_x_coluna.py:8–25` |
| 2 · Par e frota final | Universo final | **Fica** · 1255, PRONTO | **SEM TESTE** para o denominador real **[nao verificado]** | Publicar `universo` da mesma chamada com SHA, instante e população da sombra. **51.953, A=0, B=0** é a medição histórica informada na seção 5. `secao5.txt:6`; `app/chamados/services/vigia_ato_x_coluna.py:8–25` |
| 2 · Par e frota final | A, B e universo no recorte entregue | **Fica** · 1255, PRONTO | **SEM TESTE** para o recorte real **[nao verificado]** | No script, formar os PKs entregues com `competencia_do_fato` e K6, registrar o número de casos sem dia apurável e passar o queryset a `contar(perguntas=...)`. Publicar **A, B e universo** desse recorte. O teste de recorte usa PKs sintéticos. `app/chamados/services/materializacao.py:792`; `app/folha/export.py:556`; `app/chamados/services/vigia_ato_x_coluna.py:8–25`; `test_e4c_f5_contador_ato_x_coluna.py:69–76` |
| 3 · Grupo e trilha | Rejeição real com âncora de hora preservada | **Fica** · 1255, item 1 | `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_controle_ancorado_em_t1_volta_na_rejeicao:189` | **1 conexão.** Antes da rejeição, o grupo é `{escape.pk, irma.pk}`, com vínculos `ABSORCAO` e assinatura anterior preservada na irmã. Chama `decidir` e executa callbacks. Depois, grupo vazio, par limpo, ambas pendentes e com `REABERTURA` em T2. `test_e4c_f5_a1c_passagem_da_ausencia.py:189–220` |
| 3 · Grupo e trilha | Única âncora recusada antes da releitura | **Fica** · 1255, itens 1 e 2 | `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_RED_ancora_unica_fora_deixa_dependente_sem_carimbo:222` e `::test_controle_ancora_fora_em_FALTA_aprovada_fica_contada_e_sem_acao:240` | **1 conexão.** Âncora mantém a resposta T2 e a dependente fica pendente sem carimbo. O controle `FALTA_DIA` conta ambos os PKs, verifica a trilha `sem_grupo_recuperavel` e tarefa sem cobrança. Uma nova passagem explícita é exercitada em `test_e4c_f5_volta3_ausencia.py::AusenciaVolta3Test::test_RED_ancora_decidida_reabre_o_caminho_da_dependente:61`, também em **1 conexão**. `test_e4c_f5_a1c_passagem_da_ausencia.py:222–269`; `test_e4c_f5_volta3_ausencia.py:45–75` |
| 3 · Grupo e trilha | Única âncora recusada, seguida de rejeição real do atestado | **Fica** · 1255, itens 1 e 2 | **SEM TESTE** para a combinação; o cenário de atestado para após a passagem. `test_e4c_f5_a1c_passagem_da_ausencia.py:222–238` | Avulso em `logs/` estende esse cenário com `decidir(..., 'rejeitada')`. Captura o grupo, a trilha e a devolução pelo callback real e compara os mesmos PKs antes e depois. `app/ponto/services/ausencia.py:1057`; `app/ponto/signals.py:250–272`; `app/chamados/services/materializacao.py:1134,1170–1222` |
| 3 · Grupo e trilha | Entrada `escolha` sem âncora | **Fica** · 1255, item 1 | Passagem em `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_RED_falha_da_trilha_temporal_deixa_a_passagem_seguir:482`. **SEM TESTE** para a rejeição real subsequente | **1 conexão** na passagem. O escape padrão é `escolha` e a irmã de controle ganha `ABSORCAO`. Avulso em `logs/` declara `ATESTADO` pelo core, verifica os PKs retornados por `perguntas_absorvidas_por` e chama `decidir` para observar a devolução real. `test_e4c_f5_a1c_passagem_da_ausencia.py:41–47,482–508`; `app/chamados/services/disputa_emissao.py:1439`; `app/chamados/services/materializacao.py:1041–1042,1083–1090,1134–1167`; `app/ponto/services/ausencia.py:1057` |
| 3 · Grupo e trilha | Retirada da âncora depois de o grupo se formar | **Fica** · 1255, itens 1 e 2 | **SEM TESTE** para retirada seguida de rejeição. `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_RED_rechaveada_deixa_trilha_e_reconcilia_destino_da_foto:371` cobre rechaveamento de irmã **antes** da releitura, em **1 conexão** | Avulso em `logs/` forma o grupo, retira a única âncora pela porta `reabrir_para_correcao`, relê `perguntas_absorvidas_por` e só então rejeita com `decidir`. Compara a dependente, seus atos, a disputa e o número devolvido. `test_e4c_f5_a1c_passagem_da_ausencia.py:371–395`; `app/chamados/services/disputa_emissao.py:1597`; `app/chamados/services/materializacao.py:1134–1167,1170–1222`; `app/ponto/services/ausencia.py:1057` |
| 3 · Grupo e trilha | Rejeição com irmã que tem batida viva | **Fica** · 1255, item 1 | `test_o261_e1_ausencia_rejeitada_devolve.py::AusenciaRejeitadaDevolveAbsorvidas::test_MORDE_pergunta_com_batida_plantada_nao_volta:141` | **1 conexão.** Insere batida aprovada com `pergunta_origem` da irmã, chama a rejeição real e exige escape pendente e carimbo da irmã preservado. O filtro do grupo exclui batidas com `retratada_em` nulo. `test_o261_e1_ausencia_rejeitada_devolve.py:65–70,141–151`; `app/chamados/services/materializacao.py:1164–1167` |
| 3 · Grupo e trilha | Falha provocada da auditoria da irmã recusada | **Fica** · 1255, item 1; 1230, item 2 | `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_RED_falha_da_trilha_temporal_deixa_a_passagem_seguir:482` | **1 conexão.** Provoca `NOT NULL` no INSERT real da auditoria. Exige `IntegrityError` no logger, nenhuma linha de auditoria da irmã, PK em `recusadas`, resposta T2 preservada e absorção da irmã de controle. Se a falha pedida for no evento da **devolução por rejeição**, esse cenário é **SEM TESTE** na leitura. O avulso injeta a mesma falha só em `perguntas_devolvidas_ausencia_rejeitada`, chama `decidir` e confere o estado e o retorno da devolução. `test_e4c_f5_a1c_passagem_da_ausencia.py:482–508`; `app/core/observ.py:94–139`; `app/chamados/services/materializacao.py:1213–1222`; `app/ponto/services/ausencia.py:1057` |
| 3 · Grupo e trilha | Comparar PKs do grupo antes/depois | **Fica** · 1255, item 1 | `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_controle_ancorado_em_t1_volta_na_rejeicao:189` | **1 conexão.** Compara o conjunto exato antes e o vazio depois no controle ancorado. Para completar as variantes da linha, usar `perguntas_absorvidas_por` antes e depois em cada avulso. `test_e4c_f5_a1c_passagem_da_ausencia.py:196–220`; `app/chamados/services/materializacao.py:1134–1167` |
| 3 · Grupo e trilha | Comparar atos antes/depois | **Fica** · 1255, item 1 | `test_e4c_f5_a1c_passagem_da_ausencia.py::PassagemDaAusenciaTest::test_controle_ancorado_em_t1_volta_na_rejeicao:189`; **SEM TESTE** para a foto integral da sequência na matriz de rejeição | **1 conexão.** O controle exige os vínculos da `ABSORCAO` e a `REABERTURA` em T2. No avulso, comparar a sequência ordenada dos atos de cada PK, seus vínculos e a trilha de devolução. `test_e4c_f5_a1c_passagem_da_ausencia.py:201–220`; `app/chamados/models.py:4382`; `app/chamados/services/materializacao.py:1213–1222` |
| 3 · Grupo e trilha | Comparar coluna antes/depois | **Fica** · 1255, item 1 | `test_e4c_f5_a1c_pass

[truncado em 20000 de 34619 caracteres]

## 20/11 18:13 · sessao 5a8eebcfc3f5 · CADEIA-20-PASSO-1 · leitura · origem=Code

A divergência vem da instrumentação da sonda. O comando importa `julgar_colab` para o próprio módulo. Se esse primeiro import ocorre com o patch de `cartorio.julgar_colab` ativo, o comando guarda o método ligado à `Sonda` daquele braço. O `ExitStack` restaura o atributo de `cartorio`, mas o nome no módulo do comando continua apontando para a instância antiga. Assim, a guarda compara duas coisas de braços diferentes, mesmo quando as duas leituras do relógio congelado funcionam. [processar_cartorio.py:20](/home/ronald/codex_ws/a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6/app/ponto/management/commands/processar_cartorio.py:20) · `processar_cartorio.py:69` · `cadeia20_ficha_1545_medida2.py:137–145,317–340,475–492`

## 1. Por que diverge

- **Rodada 1.** Se o primeiro import ocorreu em `encerrado`, o método retido ficou com a borda `06:27`. Na primeira chamada de `em_curso` e de `cruza`, o comando já estaria em `00:30`, e a guarda ainda consultaria a instância de `encerrado`. Isso explica as duas falhas imediatas. Os horários dessas exceções e o ponto exato do primeiro import em sombra são [nao verificado]. O `main` do anexo roda apenas `em_curso` e `cruza`, portanto a variante que executou os cinco braços também é [nao verificado]. `erros.txt:1–2` · `processar_cartorio.py:20–22,40–41,69` · `cadeia20_ficha_1545_medida2.py:35–40,342–360,511–523`
- **Rodada 2.** Se o primeiro import ocorreu em `em_curso`, a borda retida foi `00:30`. As duas primeiras rodadas de `cruza` também pedem `00:30`, então a igualdade passa mesmo com a instância errada. Na terceira, o comando passa `06:27` e a guarda ainda vê `00:30`, exatamente o par do anexo. O dono efetivo do método no processo de sombra é [nao verificado]. Passar nas primeiras rodadas também é compatível com o vazamento, pois o classificador é repatchado para a sonda corrente e pode preencher `fotografados` mesmo com `Sonda.julgar` ligado à anterior. `erros.txt:3` · `cadeia20_ficha_1545_medida2.py:35–40,137–145,166–187,317–337,351–374`

**Escritores e leituras de relógio**

| Ponto | Leitura ou escrita | Fonte |
|---|---|---|
| `Sonda.borda` | Único escritor de `self.agora`, por `timezone.localtime(timezone.now())`. Também escreve `self.agora_a1 = self.agora`. É chamada dentro do `freeze_time` em `frota` e em `com_relogio`. | `cadeia20_ficha_1545_medida2.py:342–344,351–360,505–508` |
| `Sonda.relogio` | Único outro escritor de `self.agora_a1`, com a borda menos dois minutos. O instante `futuro` é derivado dessa referência. | `cadeia20_ficha_1545_medida2.py:389–407` |
| `Sonda.julgar` e fotos | A guarda lê os dois argumentos do comando e `self.agora`, deriva a data com `.date()` e só consulta o nome do fuso na mensagem de erro. Após passar, põe `self.agora` em `kw["agora"]`. As fotos reutilizam os argumentos do ato. | `cadeia20_ficha_1545_medida2.py:117–145` |
| Primeira lavra, A1 e fixture | O controle sintético e as leituras antes/depois do carimbo usam `self.agora`. A comparação de C6 usa a chamada original e uma leitura explícita com `self.agora_a1`. A fixture monta batidas com datas e horas fixas no fuso ativo e deriva a hora da lavra do argumento `agora`. A chamada sem instante de C6 pode chegar ao fallback `date.today()`. | `cadeia20_ficha_1545_medida2.py:208–219,239–248,376–382,389–410,421–449,515` · `app/chamados/models.py:364–380` · `app/chamados/juizes.py:588–597,886–906,1467–1472` |
| `processar_cartorio.handle` | Lê `_tz.localdate()` e depois `_tz.localtime(_tz.now())`, uma vez por chamada do comando. Usa `hoje` nas consultas e passa o mesmo par a todos os colaboradores daquela chamada. | `processar_cartorio.py:17,35–45,51–69` |

**Estado que atravessa rodadas ou braços**

- **Dentro de um braço**, a mesma `Sonda`, transação e pilha de patches cobrem as três rodadas. As escritas estão na mesma transação. `alvos`, `para_celula`, `anterior` e `saidas` persistem. `rodada` avança, `agora` e `agora_a1` são regravados e os quatro acumuladores `chamadas`, `alcance`, `alcance_celula` e `fotografados` são limpos. `ato`, `fase` e `foto` têm restauração nos wrappers. Cada chamada de E2, repasse ou fábrica ganha um `memo` temporário de competência e devolve o anterior. `cadeia20_ficha_1545_medida2.py:98–164,181–205,222–237,346–374,475–492` · `app/chamados/services/vigia_fio.py:786–808`
- **Patches e closures.** `instalar` captura os métodos originais e cria observadores ligados à instância corrente. A pilha cobre `cartorio` (`julgar_colab`, corpo e carimbo), supra-juiz, C6, `PD.save`, `PD.carimbar`, `PD._sob_contrato`, `QuerySet.update`, descarimbo, fecho, E2, fábrica e as duas portas do repasse. Também cobre silêncio de push, logging e sockets. Esses atributos são restaurados ao sair do braço. O alias importado pelo comando fica fora da pilha original e mantém a sonda antiga alcançável pelo método ligado. `cadeia20_ficha_1545_medida2.py:255–340,475–492` · `processar_cartorio.py:20–22,69` · `app/chamados/services/disputa_emissao.py:1773–1786`
- **Entre braços**, `main` conserva `saidas` e `rc`. O objeto global `erros` é reutilizado com `n` zerado. Também permanecem as constantes, os objetos de módulo e classe importados, `Emissor` e as referências originais `JULGAR`, `CORPO`, `C6`, `VIVAS_ENTRE`, `CARIMBAR` e `supra_classificar`. A função passada por `main` fecha sobre `relogios` e é chamada antes de o laço avançar. Cada braço cria outra `Sonda` e encerra a transação com rollback. `cadeia20_ficha_1545_medida2.py:19–46,475–502,511–532`
- **Caches do percurso.** Fila, emissor, contadores e lavra são locais a cada `handle`. O cartório guarda `agora` no `_tl` do ato e restaura o anterior no `finally`. A foto usa um `_tl` temporário. Os memos do cartório são locais ao julgamento ou ao corpo. `_pv_memo` é por instância de pergunta, inclui `hoje` e `agora` na chave e é retirado da cópia usada pela leitura comparativa. A cópia fixa a versão de `freezegun` e contém um teste das pilhas globais de freeze. O estado efetivo dessas pilhas e dos caches internos do Django nas duas execuções é [nao verificado]. `processar_cartorio.py:40–69` · `app/ponto/services/cartorio.py:35,1090–1123,1235–1261,1280–1297,1329–1337,1467–1472,1489–1495` · `app/chamados/juizes.py:886–906` · `cadeia20_ficha_1545_medida2.py:84–87,117–131,356–360` · `app/requirements.txt:43` · `app/escala/tests/test_vfase_dono_as_of.py:310–327`

## 2. Qual valor é do comando

**`cmd_agora` é o valor lido e passado pelo comando.** Na terceira rodada de `cruza`, foi **20/11/2026 às 06:27 em `America/Sao_Paulo`**, com `cmd_hoje=20/11`. O `00:30` exibido como `borda` é o atributo da instância que recebeu a chamada. A guarda levantou antes de executar `JULGAR` para aquela chamada. `erros.txt:3` · `processar_cartorio.py:40–41,69` · `cadeia20_ficha_1545_medida2.py:137–145`

## 3. Patch mínimo

Instalar o patch no nome que o comando consulta **antes** do patch em `cartorio`. Ao entrar nesse primeiro contexto, `patch` resolve o módulo do comando. Se for o primeiro import, ele ainda encontra o `julgar_colab` original em `cartorio`. A pilha passa a restaurar os dois nomes. A guarda, os instantes, as fotos e as proteções de sombra, rollback e rede continuam com o mesmo código. Há um teste na árvore que intercepta esse mesmo nome do comando. `processar_cartorio.py:20–22,69` · `cadeia20_ficha_1545_medida2.py:68–81,117–145,317–340,351–360,475–492` · `app/escala/tests/test_cadeia20_volta3.py:180`

```diff
--- a/cadeia20_ficha_1545_medida2.py
+++ b/cadeia20_ficha_1545_medida2.py
@@ -333,6 +333,8 @@
              self.etapa("fabrica", fabrica_da_celula.perguntar_pela_celula)),
             (disputa_emissao, "criar_disputa_a_partir_de_chamados_orfaos", medir_repasse),
         )
+        pilha.enter_context(patch(
+            "ponto.management.commands.processar_cartorio.julgar_colab", self.julgar))
         for objeto, nome, funcao in instrumentos:
             pilha.enter_context(patch.object(objeto, nome, funcao))
         # Também cobre a fachada usada por detectar_ausencias.
```

Em memória, com import tardio e substitutos para ORM e relógio, reproduzi as duas sequências de falha com o anexo original. Com o diff, a guarda passou nas três rodadas de `cruza` em ambas as ordens da frota. A conclusão das medições em sombra é [nao verificado]. `processar_cartorio.py:20–22,35–69` · `cadeia20_ficha_1545_medida2.py:137–145,317–360` [nao verificado]

## 20/11 16:09 · sessao 37ffb83e1278 · E4C-F5-CONSTROI · verificador · origem=Code

**Veredito: a trilha de S144 tem um RED que distingue o diff da base, mas há um bloqueio na régua.** O arquivo de mutantes tem nome de teste e zero casos. Quando rastreado, ele reprova o selo de host antes da suíte. A promessa de uma linha por PK também é mais forte que a garantia do código fora do fecho serializado. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:186–194`, `app/chamados/tests/test_e4c_f5_volta3_fecho_mutantes.py:1–33`, `bin/tests/test_selo_nao_nasce_vazio.sh:37–45`, `bin/regua.sh:138–144`, `app/chamados/services/materializacao.py:2222–2228`.

Conferi os quatro blobs da cópia pelos hashes do `delta.diff` e reconstruí em memória o blob anterior de `materializacao.py`. O resultado bate com `80b5edec2`. A cópia não tem metadados Git para confirmar o HEAD `c080170007ddd25bfb64c7220cf84489a35d9a8a` [nao verificado]. Os resultados abaixo são de leitura estática. Execução de testes e mutantes, banco e logs de produção [nao verificado].

## 1. Escritores e leitores

| Campo ou valor | Escritor | Leitor |
|---|---|---|
| `recusa_do_carimbo = None` | Default de classe e reset por instância em `app/chamados/models.py:3322,3353`. | A passagem de ausência captura o valor sem filtrar previamente. Os espiões de teste também capturam o valor e o teste A2 exige `None` no sucesso. `app/chamados/services/materializacao.py:1073–1078`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:85–89,213–217`. |
| `fora_do_fato` | `app/chamados/models.py:3319,3356–3358`. | A passagem de ausência lê o motivo e o põe no JSON da trilha. Os demais leitores de produção encontrados comparam com `par_ato_coluna`. `app/chamados/services/materializacao.py:1074–1078,1098–1104,2017,2222`, `app/chamados/services/passivo_validado.py:144–147`. |
| `c6` | Porta real em `app/chamados/models.py:3320,3380–3383`. O stub escreve esse valor em `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:209–211`. | A ausência dá descrição própria a `c6`. Plantio, passivo e S144 distinguem o par por igualdade. `app/chamados/services/materializacao.py:1074–1078,2017–2022,2222–2228`, `app/chamados/services/passivo_validado.py:144–147`. |
| `par_ato_coluna` | `app/chamados/models.py:3321,3390–3393`. | Ausência, plantio, passivo e S144 nos sítios acima. Leitores de teste do atributo em `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:455`, `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:223`, `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:59`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:73` e `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:88`. |
| `acao='par_recusado_no_fecho'`, `modelo='PerguntaDisputa'`, `objeto_id=p.pk` | S144 fornece os três valores. `evento` faz o `objects.create`. `app/chamados/services/materializacao.py:1122,2226`, `app/core/observ.py:133–138`. | A deduplicação e o RED filtram pelos três. A auditoria os exibe. A foto E4B lê por modelo e PK, sem restringir ação. `app/chamados/services/materializacao.py:2222–2223`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:186–194`, `app/templates/core/log_auditoria.html:55–73`, `app/chamados/services/foto_e4b.py:310–323`. |
| `depois={'motivo': ..., 'via_tentada': 'juiz_aprovou'}` | `depois=` vira `LogAuditoria.valor_depois`, um `JSONField`. `app/chamados/services/materializacao.py:2226–2228`, `app/chamados/models.py:2682`, `app/core/observ.py:134–137`, `app/core/models.py:301`. | O RED confere o JSON inteiro junto com `usuario_id`. A foto E4B copia todos os campos concretos. A tabela da auditoria exibe a descrição e omite o JSON. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:186–190`, `app/chamados/services/foto_e4b.py:247–252,322–323`, `app/templates/core/log_auditoria.html:55–74`. |
| Retorno de `carimbar` | A porta devolve `bool`. `app/chamados/models.py:3358,3383,3393,3405`. | Os sete sítios de produção estão no item 5. Os espiões de S144 e do reconciliador leem e devolvem o resultado real. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:64–76`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:82–91`. |

**Quem passa a ver a linha.** A tela de auditoria lista a ação livre no resultado geral ou filtrado por modelo. `?acao=par_recusado_no_fecho` funciona pelo filtro exato do GET. O seletor de ações lê `LogAuditoria.ACOES`, que omite o verbo novo, e o `create()` aceita verbos fora dos `choices`. O atalho em Relatórios leva à mesma tela. `app/core/views.py:697–738`, `app/core/models.py:285–301`, `app/templates/core/log_auditoria.html:23–33,60–73`, `app/templates/relatorios/index.html:234–241`.

A foto E4B inclui a linha quando a pergunta está no recorte. O agregador `escritas_por_cron` também a incluiria se rodasse com o `ator` e a janela correspondentes. Um chamador ativo desse agregador [nao verificado]. Os leitores do C6 e da costura filtram seus próprios verbos, o histórico filtra ações de estado de chamados, e o contador do par lê `PerguntaDisputa` e suas decisões. `app/chamados/services/foto_e4b.py:310–323`, `app/chamados/services/vigia_loop.py:71–99`, `app/chamados/juizes.py:285–296`, `app/chamados/services/carga_pergunta_viva.py:110–112`, `app/chamados/models.py:3203–3211`, `app/chamados/services/historico.py:86–95`, `app/chamados/services/vigia_ato_x_coluna.py:8–24`. Outro leitor específico do verbo novo ou validador contra `choices` [nao verificado].

## 2. Vida de `recusa_do_carimbo`

Há um default na classe. A chamada escreve na **instância**, em memória, e zera o atributo logo após os imports, antes de qualquer guarda. Uma instância reutilizada começa a nova chamada com `None`. Todos os `False` da porta real atribuem um motivo antes de retornar. `app/chamados/models.py:3317–3322,3351–3360,3375–3405`.

| Retorno | Valor na saída |
|---|---|
| `False` em `app/chamados/models.py:3358` | `fora_do_fato` |
| `False` em `app/chamados/models.py:3383` | `c6`, após desfazer o carimbo em memória |
| `False` em `app/chamados/models.py:3393` | `par_ato_coluna`, após desfazer o carimbo em memória |
| `True` em `app/chamados/models.py:3405` | `None`, após salvar |

Um `False` sem atribuição, **depois do reset**, deixaria `None` e o ramo novo não gravaria. O `False` antecipado de `encerrar_fantasma` pode sair sem chamar `carimbar`, mas S144 chama `carimbar` diretamente. `app/chamados/models.py:3351–3358,3552–3560`, `app/chamados/services/materializacao.py:2220–2228`.

## 3. Transação, travas, duplicata e falhas

- **Fecho canônico.** `DisputaSupervisao.fechar()` salva dentro de `atomic`, sob a ordem Colaborador → Disputas → Chamados. Essa chamada não passa `perguntas=`, portanto não toma um `SELECT FOR UPDATE` explícito sobre a pergunta nesse ponto. S144 é síncrono no `post_save` e herda a transação e as travas do chamador. O serviço já selecionava as pendentes por leitura simples. O diff acrescenta `exists()` e a inserção com o `atomic` interno de `evento`. `app/chamados/models.py:2089–2097,2118–2126,2874–2904`, `app/chamados/signals.py:149–167`, `app/chamados/services/materializacao.py:2200–2229`, `app/core/observ.py:124–138`, `delta.diff`, hunks de `materializacao.py`.
- **Dois escritores concorrentes do mesmo PK.** Dois fechos canônicos do mesmo dono se serializam e, em `READ COMMITTED`, o segundo `exists()` pode ver a linha já confirmada pelo primeiro. Fora dessa serialização, dois `exists()` podem ler vazio e os dois `INSERT`s podem confirmar. A ação não tem unicidade com modelo e PK. Isso cabe em chamadas diretas concorrentes e em dois `save()` em autocommit cujos receivers se sobreponham após os respectivos updates. Um `save()` dentro de transação que ainda detenha a linha da mesma disputa também fornece serialização pelo update. Ocorrência em produção e isolamento efetivo [nao verificado]. `app/chamados/models.py:2089–2122,2874–2886`, `app/chamados/signals.py:149–167`, `app/chamados/services/materializacao.py:2207–2228`, `app/core/models.py:309–316`, `app/core/observ.py:133–138`.
- **Rollback e erro.** Se a transação externa desfizer, a linha some junto. Uma falha comum no `create()` volta pelo savepoint de `evento`, que registra a falha e devolve `None`. O laço segue, e o save da disputa e os carimbos anteriores ainda podem confirmar. Se uma exceção escapar de `evento`, por exemplo na própria emissão do log de erro, o receiver captura `Exception` e o laço para. O receiver não desfaz os carimbos anteriores por si. O `exists()` fica fora desse savepoint, então uma falha SQL ali que aborte a transação externa pode levar junto o save da disputa e os carimbos ainda sem commit. Em autocommit, escritas anteriores já confirmadas permanecem. `app/core/observ.py:73–91,103–139`, `app/chamados/signals.py:157–167`, `app/chamados/models.py:2089–2126,3719–3735`, `app/chamados/services/materializacao.py:2220–2229`.

## 4. Chamadores, usuário e tipo do ID

- **Produção.** A chamada direta é do receiver S144, com `instance.fechada_por`, em qualquer update de disputa com `fechada_em` preenchido, exceto `raw`. `fechar()` exige usuário, mas a FK admite `NULL` e usa `SET_NULL`. O serviço aceita `None` e busca ou cria `User(username='sistema')`. `app/chamados/signals.py:149–165`, `app/chamados/models.py:1922–1928,2066–2067,2118–2122`, `app/chamados/services/materializacao.py:2177,2203–2205`, `app/core/utils.py:6–18`.
- **Testes.** Chamadas diretas em `app/chamados/tests/test_porta_desfecho_pergunta.py:226,244,248,263,292` com `self.user`, `outro` ou `None`, em `app/chamados/tests/test_pep_reentrada.py:354` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:693` com `self.user`, e no controle novo com `self.admin`. Pelo receiver, o RED novo usa `fechar(self.admin)` e repete o save, o teste de desfecho salva com `self.user` e a blindagem salva sem preencher `fechada_por`. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:167–169,192–194,213–215`, `app/chamados/tests/test_porta_desfecho_pergunta.py:268–282`, `app/chamados/tests/test_s144_blindagem.py:28–34`.
- **Ator e ID.** `evento` aceita `ator=None`, pode aproveitar o usuário autenticado do request e, fora dele, preencher o ator de processo. `usuario` é anulável. O PK de pergunta é `BigAutoField` e `objeto_id` é `IntegerField`. Aceita o inteiro enquanto couber na coluna de 32 bits do PostgreSQL. O domínio inteiro de `p.pk` é maior. Faixa dos PKs da frota [nao verificado]. `app/core/observ.py:94–118`, `app/core/models.py:295–306`, `app/chamados/migrations/0014_perguntadisputa.py:17–20`, `app/config/settings/saas.py:83–91`.

## 5. Mesmo fato em outras trilhas e tratamento do `False`

`par_recusado_no_fecho` significa que **a tentativa de carimbar pela via `juiz_aprovou` durante o S144 foi recusada pelo par**. A ausência já registra a mesma classe de recusa da porta como `irma_viva_na_ausencia`, com `motivo='par_ato_coluna'`, em outra tentativa. A validação também pode registrar o erro de plantio do mesmo par em `val_perg_nao_gravou`, na descrição. `par_ato_coluna_divergente` da ausência é a reprovação posterior de um carimbo aceito, em outro ponto do fluxo. Um segundo escritor do fato específico de S144 [nao verificado]. `app/chamados/services/materializacao.py:1072–1104,1118–1122,2015–2027,2220–2228`, `app/chamados/services/validacao.py:786–804,821–830`, `app/core/auditoria.py:17–25`.

| Chamador de produção | O que faz no `False` |
|---|---|
| S144 | Omite o PK de `feitas`. Só o motivo do par passa pelo `exists()` e pela trilha nova. `app/chamados/services/materializacao.py:2220–2229`. |
| Passagem de ausência | Guarda motivo, anota a recusa e escreve `irma_viva_na_ausencia`. `app/chamados/services/materializacao.py:1072–1078,1094–1104`. |
| Plantio | Levanta `_Reprovada`, desfaz o plantio daquele bloco e registra o erro. O `finally` leva o motivo à pergunta e, se houver destino, pode anexar nota no fio. O passivo categoriza o erro pelo mesmo atributo. `app/chamados/services/materializacao.py:2013–2040,1717–1741`, `app/chamados/services/passivo_validado.py:131–148,532–548`. |
| Reconciliador, sem turno aberto | Devolve `False` antes de tentar fechar a disputa. Sem trilha dessa recusa nesse ramo. `app/chamados/reconciliador.py:400–417`. |
| Reconciliador, célula absolveu | Pula a pergunta, sem soma, fecho por ela ou trilha da recusa. `app/chamados/reconciliador.py:635–653`. |
| Fantasma pela lâmpada | `encerrar_fantasma` propaga o resultado. O chamador pula a pergunta antes da trilha de sucesso. `app/chamados/models.py:3552–3560`, `app/chamados/reconciliador.py:1188–1211`. |
| Resolver/S143 e fecho sem credor | Ambos dão `continue`. O segundo só escreve `fecho_sem_credor` após sucesso. `app/chamados/services/disputa_emissao.py:2052–2062,2111–2142`. |

## 6. As três provas do PRONTO

As previsões de falha abaixo comparam o código da base com o teste atual. `delta.diff`, hunks de S144 e do teste. [nao verificado]

| Parte | Asserção | Falharia com a produção anterior? |
|---|---|---|
| Recusa | O espião exige a tentativa pela via alvo com `(False, True)`, retorno `False` e `RECUSA_PAR_ATO_COLUNA`. Após `refresh`, exige ausência de carimbo e lavra intacta. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:64–81,167–176`. | **Não.** A porta já recusava na base. `app/chamados/models.py:3384–3393`, `delta.diff`, hunk de S144. |
| Motivo na trilha | Filtra ação, modelo e PK e exige exatamente a tupla com autor, motivo e via. O segundo save deve deixar a contagem em um. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:186–194`. | **Sim.** A lista viria vazia. Avaliada isoladamente, a contagem também seria zero. `delta.diff`, hunk de S144. |
| PK encontrável pelo contador | Inclui só aquele PK e exige `(A, B, universo) == (0, 1, 1)`, depois o exclui e exige `B == 0`. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:197–201`. | **Não.** A divergência da fixture já existe e o contador já a lê. O teste localiza a contribuição por filtragem. O retorno do contador contém totais. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:56–61`, `app/chamados/services/vigia_ato_x_coluna.py:8–24`. |

É a **mesma função**, com o mesmo `lote` padrão e querysets diferentes. O teste recorta pelo PK e pelo seu complemento. `vigiar_fio_morto` passa a frota ou o recorte por empresa. O cron publica A, B e universo no stdout e grava A e B no JSON de frota. O ESTADO mostra `FORA DO ESPERADO` quando divergem de zero. A e B ficam fora das chaves que disparam `SystemExit(2)`, mesmo com `--alarme-passivo`. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:14–15,197–201`, `app/chamados/management/commands/vigiar_fio_morto.py:79,85–110,194–199,210–211,233–234,263–303`, `app/config/crons.py:1737–1739`, `bin/fio_estado.py:122–126,191–194`, `bin/gerar_estado.py:253–261`.

O anexo exige a linha “S144 inalcancavel com A=0 e B=0 na frota; o contador do par alarma”. A medida de frota e a publicação dessa linha [nao verificado]. `respostas_2011_1545.txt:4`, ITEM 3.

## 7. Mordida, controles e testes existentes [nao verificado]

| Teste | O que distingue | [nao verificado]
|---|---|
| RED S144 existente | Ganha trilha, motivo, autor, deduplicação sequencial e contribuição a B. Remover a trilha ou trocar o motivo derruba `:188`. Remover a deduplicação derruba `:194` se o receiver tentar de novo. Ignorar B derruba `:200`. A recusa original e o contador passam isoladamente sem o ramo novo. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:158–201`. |
| Controle C6 novo | Passa sem a cura e acusa o mutante que trilha toda recusa, desde que a chamada chegue ao stub. O patch reproduz `False` e `RECUSA_C6`. Ele dispensa o julgamento real, a aplicação e reversão em memória, e o teste não exige que o stub tenha sido chamado. Há um teste anterior com recusa real do C6, mas ele só verifica o retorno e o carimbo ausente. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:203–215`, `app/chamados/models.py:3363–3383,3407–3414`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:687–696`. |
| RED C1 existente | Ganha a continuação pelo mesmo PK após a reabertura. Exige retorno `1`, carimbo pela célula, par limpo e só a `REABERTURA`. Acusa o mutante sem avanço. A continuação não depende da trilha de S144. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:112–145`, `app/chamados/reconciliador.py:635–668`. |
| Dois A2 novos | Intercalam validação por outra instância depois da seleção ou do veredito da órfã. Exigem que a porta receba e devolva a instância com coluna nula, carimbe, e que o banco mantenha a validação. Mordem uma releitura nesse intervalo. Exercitam comportamento já presente na base e independem da cura S144. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:169–229,236–238,262–264`, `app/chamados/reconciliador.py:635–646,1188–1203`. |
| Dois A2 existentes | O helper ganhou `validar_apos_selecao=False`. Os chamadores antigos preservam fixture e asserções. A fixture de turno encerrado da órfã foi extraída para um helper e mantém as batidas e a via. Os nomes dos testes existentes foram preservados. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:200–234,250–264`, `delta.diff`, hunks do reconciliador. |

Os casos novos de S144 usam `freeze_time(AGORA)` no `setUp`, e os A2 têm `@freeze_time(AGORA)` na classe. O relógio que `_carimbar_validacao` lê nesses casos fica sob esse congelamento. `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:28–32`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:31–47,169–177`, `app/chamados/services/validacao.py:161–175`.

## 8. Arquivo de mutantes

É uma **CLI de mutação em memória**. Executado como `__main__`, registra seis substituições e chama o helper. O helper aplica uma mutação, roda os labels que recebeu e restaura o código. A descoberta do Django pode importar o módulo pelo nome `test_*.py`, mas recolhe zero casos dele e não entra no bloco `__main__`. A seleção `--volta` ainda o inclui como label de arquivo tocado. Conferi estaticamente uma ocorrência de cada âncora e zero `def test`. Execução dos mutantes [nao verificado]. `app/chamados/tests/test_e4c_f5_volta3_fecho_mutantes.py:1–33`, `app/chamados/tests/e4c_f5_volta3_mutantes.py:80–112`, `app/core/runner_quarentena.py:72–86`, `bin/vizinhos_da_volta.py:33–45,92–94`.

**O selo de host cobra o arquivo.** `test_selo_nao_nasce_vazio.sh` enumera os `test_*.py` rastreados e reprova os que não declaram um caso. A régua chama esse selo antes do Django. O censo de `test_contract_*.py` usa outro glob, e o selo do par exclui `tests/` da sua varredura de produção. `bin/tests/test_selo_nao_nasce_vazio.sh:24–45`, `bin/regua.sh:134–145`, `app/core/tests/test_o20_contrato_julga_codigo.py:158–164`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:443–465`.

## 9. Escopo, corte e dinheiro

O delta de produção cria o verbo e o payload da trilha. Reusa a porta, o juiz do lastro, o comparador e o escritor de auditoria. Não cria juiz, contador, selo de produção, trava, porta, lista de elegibilidade ou ato de reparo, nem muda a ordem de trava. O 1545 diz “a porta recusa (par_ato_coluna) e NAO nasce porta para avancar PK em estado ilegal” e proíbe “trava nova; porta nova; ampliar o escopo da F5”. O 1400 item 1 exige “ACAO EXISTENTE e demonstravel” e “Sem ato de reparo, trava nem lista nova”. `app/chamados/services/materializacao.py:1120–1122,2207–2229`, `app/chamados/models.py:3384–3393,4393–4399`, `app/core/observ.py:94–138`, `respostas_2011_1545.txt:4,13`, `respostas_2011_1400.txt:3`.

O ramo novo escreve só auditoria, sem alteração direta em `folha/`, competência, exportação, batida ou insumo de cálculo. **Há alcance indireto na cerca E4B.** O backfill pode fechar disputa pela materialização, fotografa a auditoria e atribui à linha nova o alcance da pergunta. Se a linha entrar no delta de um dia e seu alcance for indeterminado, fora da competência-alvo ou entregue, a cerca pode motivar o rollback do dia. Ocorrência e impacto monetário [nao verificado]. `app/chamados/services/materializacao.py:2080,2165–2166,2222–2228`, `app/chamados/services/foto_e4b.py:145–163,310–333`, `app/chamados/mana

[truncado em 20000 de 24212 caracteres]

_7594 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
