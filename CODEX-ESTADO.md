# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 23:47** · publicado em **16/11/2026 23:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 23:47 · sessao 0e37165938ec · - · - · origem=Ronald (tmux)

Achado remanescente: há decisão própria de produção fora de C21, embora equivalente no resultado. `espera_o_texto_do_colaborador()` retorna `p.respondida_em is None or respondida_sem_texto(p)`, em vez de delegar a `not pergunta_respondida(p)`. Alimenta `resposta_enviada`, aviso/correção do cartão e `_calados`, que alimenta pauta da supervisão. Deve migrar por unicidade; não é exposição do cru. (`app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:187`, `app/chamados/services/fio.py:359`, `app/chamados/services/fio.py:386`, `app/chamados/services/cobranca.py:227`, `app/chamados/services/cobranca.py:241`.)

Classificação dos demais usos diretos encontrados:

| Sítio | Classificação |
|---|---|
| `app/api/views.py:1165`, `app/api/views.py:2563`; `app/api/views_mensageria.py:569` | Serialização do instante, legítima; não decide respondida. |
| `app/chamados/veredito_lavrado.py:207`; `app/chamados/services/painel.py:67`; `app/chamados/services/verbos.py:344`; `app/chamados/services/validacao.py:1256` | Tempo, Max, idade e ordenação: legítimos. |
| `app/chamados/models.py:4095`, `app/chamados/models.py:4102`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/services/historico.py:99` | Trilha/projeção de ato ocorrido, legítima; não substituir pela situação atual C21. |
| `app/chamados/models.py:3241`, `app/chamados/models.py:3242`; `app/chamados/models.py:3371`; `app/chamados/models.py:3586` | Porta de escrita, construção/conferência do par: legítimos. |
| `app/chamados/services/disputa_emissao.py:1663` | Preservação do texto existente na resposta automática: legítima proteção de conteúdo, inclusive legado; não transformar em C21 e permitir sobrescrever texto sem I. |
| `app/chamados/services/disputa_emissao.py:1692`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46` | Cura histórica do meio-par e conferência da lista fixa: legítima pergunta de integridade/trilha. |
| `app/chamados/services/disputa_emissao.py:1861` | Detecta limbo para impedir reabertura e preservar texto; legítima integridade, embora possa chamar `texto_sem_carimbo`. |
| `app/chamados/services/disputa_emissao.py:2079` | Texto para relato do ato e porta que carimba resposta: legítimo contexto de escritor. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1106` | Alarme legado de meio-par: não é decisão respondida. Contudo usa `!= ''`, sem trim; também conta espaço/NBSP sem I como “preenchida”, divergindo da integridade L130. |
| `app/chamados/services/vigia_fio.py:747`, `app/chamados/services/vigia_fio.py:750` | Censo de integridade I sem T, com `texto_vazio` real: legítimo. |
| `app/chamados/models.py:3692`; `app/templates/chamados/partials/_card_atendimento.html:141`, `app/templates/chamados/partials/_card_atendimento.html:182`; seletores em `:114` e `:154` | Renderização/seleção do conteúdo cru: legítimos para esse propósito. As frases “Você respondeu”/“Sua resposta” ainda aparecem por texto truthy, independentemente de I. |
| `app/chamados/services/fio.py:377`; `app/chamados/templatetags/chamados_tags.py:139` | Guarda conservadora de indisponibilidade da testemunha de aplicação, não classificação geral de respondida: legítima. |
| `app/chamados/templatetags/chamados_tags.py:166`; `app/chamados/services/materializacao.py:770` | Atalho por ausência de conteúdo para análise/parsing da hora: legítimo; não afirmar C21 por ele. |
| `app/chamados/services/validacao.py:467`, `:1183`, `:1194`; `app/chamados/services/materializacao.py:625`, `:953`, `:1562` | Interpretação do valor (folga, escape, hora/data), legítima. |
| `app/chamados/services/acoes_disputa.py:357`; `app/ponto/services/dia_do_colab.py:70` | Foto/texto exibido ou conservado: legítimos. |

Os templates que decidem estado por `.respondida` passam pela property delegadora (`app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/_pergunta_card.html:24`; `app/chamados/models.py:3664`). Não encontrei `~q_respondida_com_texto()` executável nos leitores pesquisados; há menção proibitiva na docstring (`app/chamados/juizes.py:1585`). A varredura global dos campos não encontrou usos executáveis adicionais em folha/escala/ferias fora das famílias catalogadas; isso é alcance de busca, não prova dinâmica. [não verificado]

NBSP está contemplado no código: Python usa `.strip()`; SQL constrói a classe completa a partir de `str.isspace()`, em vez de `\s` do banco. As faces positiva e negativa empregam essa mesma classe (`app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1560`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1590`). Há teste pk a pk da face negativa com `' \xa0 '` e testes da face positiva/ilegais, mas não executei testes de banco (`app/chamados/tests/test_f1nova_b2_leitores.py:137`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:89`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:34`). Execução SQL nesta cópia: [não verificado].

O selo de respondedores tem ponto cego concreto: conta apenas decisões sobre `respondida_em`, considera `PAR` qualquer expressão que também leia texto, e o teste explicitamente preserva `espera_o_texto_do_colaborador` nessa classe. Logo zero não significa “todos delegam ao juiz”; um clone de I∧T continua verde (`app/chamados/tests/test_e3_catraca_respondedores.py:51`, `:356`, `:381`, `:505`, `:512`). A mudança de `PAROU_EM` para zero não fortaleceu esse contrato (`app/chamados/tests/test_e3_catraca_respondedores.py:54`).

Executei o módulo sem banco e sem bytecode: 17 testes, OK, rc=0. O resultado da execução não tem artefato porque a tarefa proíbe criar arquivos. [não verificado] [nao verificado]

## 16/11 23:31 · sessao 68ac2e2d6427 · ESPELHO-VERDADE-NA-BRECHA · leitura · origem=Code

**Os 12 donos permanecem [nao verificado].** O registro disponível mantém esses casos pendentes justamente por faltar a leitura dos papéis das batidas; não autoriza converter as candidatas em causas provadas (`app/docs/RELATO.md:99-101`).

A L-099 exige separar ESTRUTURA, CADASTRO e BATIDA; somente ESTRUTURA vira obra de código (`app/docs/LEIS.md:122`). A decisão deve sair de `InsumosDoDono(..., espelho=esp).dono(dia)`, que alimenta o juiz existente, sem reconstruir seus predicados (`app/ponto/management/commands/e6_oraculo.py:182-195`).

A precedência **executada** é:

1. Folga não definida ou ausência de vínculo → **CADASTRO** (`app/ponto/management/commands/e6_oraculo.py:98-99`).
2. Aberto com entrada solta apurável confirmado por `batida_fora_de_turno` → **BATIDA**; “não apareceu no turno” sozinho não basta (`app/ponto/management/commands/e6_oraculo.py:198-212`).
3. Quantidade ímpar nas horas **atribuídas pelo E6** → **BATIDA** (`app/ponto/management/commands/e6_oraculo.py:106-107`).
4. Fase deslocada lavrada → **CADASTRO** (`app/ponto/management/commands/e6_oraculo.py:108-109`, `app/ponto/management/commands/e6_oraculo.py:174-179`).
5. Presença em `dias_cadastro_x_realidade` → **CADASTRO**; restante → **ESTRUTURA** (`app/ponto/management/commands/e6_oraculo.py:110`, `app/ponto/management/commands/e6_oraculo.py:170-171`).

O resíduo ESTRUTURA não identifica sozinho qual testemunha errou: o próprio núcleo registra cortes suspeitos do oráculo, e o RELATO distingue esses defeitos do espelho (`app/ponto/calculador/nucleo.py:218-223`, `app/docs/RELATO.md:100`). “ORACULO” não é uma quarta saída desse juiz (`app/ponto/management/commands/e6_oraculo.py:98-110`).

As identidades abaixo vêm do anexo; a causa e o dono individual continuam [nao verificado]. Em cada linha, os sinais indicados alimentam a precedência acima.

| Caso | O que o dado impresso precisa decidir |
|---|---|
| col369 — 12/10 | Se o vão pertence a pausa reconhecida e qual intervalo o dia declara; isso decide o desconto efetivamente aplicado (`app/ponto/turnos.py:756-766`). |
| col864 — 12/10 | Se as batidas internas foram marcadas como pausa ou ficaram como trabalho; a sequência gravada S→E sozinha não decide (`app/ponto/turnos.py:798-883`). |
| col114 — 08/11 | Quais papéis receberam as pontas do vão e qual pausa o leitor real reconheceu (`app/ponto/turnos.py:875-883`, `app/ponto/turnos.py:542-546`). |
| col114 — 15/11 | Se a célula declara intervalo e se houve pausa marcada; célula existente sem marcos não herda pausa do template (`app/escala/models.py:1189-1208`). |
| col723 — 08/10 | Se o vão integra uma pausa reconhecida, foi limitado pelo fim contado ou substituído pela janela declarada (`app/ponto/turnos.py:735-764`). |
| col746 — 24/09 | A mesma conferência entre bordas, pausa reconhecida e janela efetivamente descontada (`app/ponto/turnos.py:735-764`). |
| col876 — 21/09 | Se a batida excluída deixa aberto com entrada solta real; somente esse resultado aciona a porta específica BATIDA (`app/ponto/management/commands/e6_oraculo.py:208-212`). |
| col130 — 29/10 | A que `data_turno` pertencem os segmentos vizinhos e se existe aberto com entrada solta apurável (`app/ponto/turnos.py:719-740`, `app/ponto/management/commands/e6_oraculo.py:208-212`). |
| col220 — 10/11 | Se a batida fora do segmento produz o insumo real BATIDA, preservadas as precedências de cadastro (`app/ponto/management/commands/e6_oraculo.py:98-110`, `app/ponto/management/commands/e6_oraculo.py:208-212`). |
| col868 — 22/09 | Qual vínculo/célula responde pelo dia e se o motor publicou esse dia em CXR; horário noturno contra template diurno não substitui essa consulta (`app/ponto/management/commands/e6_oraculo.py:170-171`, `app/escala/servico_jornada.py:66`). |
| col516 — 23/10 | Fonte dos minutos do espelho, turnos atribuídos ao dia e fragmentos que o E6 atribuiu às datas vizinhas (`app/ponto/management/commands/e6_oraculo.py:263-269`, `app/ponto/turnos.py:719-740`, `app/ponto/calculador/nucleo.py:236-247`). |
| col884 — 08/11 | Papel real das pontas, pausa reconhecida e cobertura efetiva da ausência; tipo invertido e atestado rejeitado não substituem os insumos do juiz (`app/ponto/turnos.py:875-883`, `app/ponto/turnos.py:2523-2531`). |

A sonda abaixo mantém a janela real da competência e as margens do carregador, imprime as três datas e observa as instâncias consumidas pelo pareador, inclusive as que não chegaram aos turnos retornados (`app/ponto/management/commands/e6_oraculo.py:149-159`, `app/ponto/turnos.py:1371-1397`, `app/ponto/turnos.py:1988-2000`). Ela imprime o dono **condicionado à persistência da divergência**, sem copiar a classificação ou mudar tolerâncias (`app/ponto/management/commands/e6_oraculo.py:354-378`).

Execução Django, envelope V0 e atualidade do dump: **[nao verificado]**. O contador abaixo cobre exceções capturadas pela sonda; funções reais também contêm capturas internas, como a abstenção do leitor de papéis (`app/ponto/turnos.py:1631-1635`).

```python
# Base: 520d7b55720888d80b46ffbca2ca73e59da40277.
# Django shell do envelope V0, SOMBRA, dump de 16/11/2026.
# Envelope/dump efetivos: [nao verificado] nesta copia.
import sys
sys.dont_write_bytecode = True
import contextlib
import datetime as dt
import io
import json
import logging
import re
from decimal import Decimal
from collections import defaultdict
from zoneinfo import ZoneInfo

BASE = "520d7b55720888d80b46ffbca2ca73e59da40277"
CASOS = (
    (369, 10, "2026-10-12"), (864, 10, "2026-10-12"),
    (114, 11, "2026-11-08"), (114, 11, "2026-11-15"),
    (723, 10, "2026-10-08"), (746, 10, "2026-09-24"),
    (876, 10, "2026-09-21"), (130, 11, "2026-10-29"),
    (220, 11, "2026-11-10"), (868, 10, "2026-09-22"),
    (516, 11, "2026-10-23"), (884, 11, "2026-11-08"),
)  # Identidades do anexo: [nao verificado] nesta copia.

saida = sys.stdout
erros = 0
lidos = set()
donos = {}
segredos = {}
timezone = None


def raspar(texto):
    for segredo in sorted(segredos, key=len, reverse=True):
        texto = re.sub(
            re.escape(segredo), segredos[segredo], texto,
            flags=re.IGNORECASE,
        )
    return re.sub(
        r"(?<!\d)(?:\d{3}\.?\d{3}\.?\d{3}-?\d{2})(?!\d)",
        "[CPF removido]", texto,
    )


def serializar(valor):
    if isinstance(valor, dt.datetime):
        return timezone.localtime(valor).isoformat()
    if isinstance(valor, (dt.date, dt.time)):
        return valor.isoformat()
    if isinstance(valor, Decimal):
        return str(valor)
    if isinstance(valor, (set, frozenset)):
        return sorted(valor)
    raise TypeError(
        "objeto sem serializacao segura: " + type(valor).__name__
    )


def emitir(evento, fonte, **dados):
    linha = dict(evento=evento, base=BASE, fonte=fonte, **dados)
    print(
        raspar(json.dumps(
            linha, ensure_ascii=False, default=serializar,
        )),
        file=saida, flush=True,
    )


def erro(etapa, exc, **contexto):
    global erros
    erros += 1
    emitir(
        "ERRO", "[nao verificado]", etapa=etapa,
        mensagem=type(exc).__name__ + ": " + str(exc),
        **contexto,
    )


def executar():
    global timezone
    from django.apps import apps
    from django.db import connection, transaction
    from django.db.models import Q
    from django.utils import timezone
    from django_tenants.utils import schema_context
    from ponto import janelas, turnos as T
    from ponto.models import Batida, Ausencia
    from escala.models import CelulaDia
    from escala.alimentacao import (
        celulas_do_periodo, escalas_do_periodo, vinculo_do_dia,
    )
    from ponto.services.espelho import (
        espelho_do_colab, _realizado_do_dia_tela,
    )
    from ponto.management.commands.e6_oraculo import (
        InsumosDoDono, minutos_do_espelho, batida_fora_de_turno,
        diferenca_e_a_janela_declarada,
    )
    from ponto.supra_juiz import cadastro_do_dia_carregado

    with timezone.override(ZoneInfo("America/Sao_Paulo")):
        agora = timezone.localtime(timezone.now())
        hoje = agora.date()
        if hoje != dt.date(2026, 11, 16):
            raise RuntimeError(
                "esta sonda pede o envelope V0 de 16/11/2026"
            )
        if agora.utcoffset() != dt.timedelta(hours=-3):
            raise RuntimeError("localtime nao esta em UTC-3")
        if connection.in_atomic_block:
            raise RuntimeError(
                "executar em shell sem transacao externa"
            )

        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION READ ONLY")
                cursor.execute("SELECT current_database()")
                if cursor.fetchone()[0] != "sombra":
                    raise RuntimeError("banco diferente de sombra")

            with schema_context("juliani"):
                Colaborador = apps.get_model(
                    "colaboradores", "Colaborador"
                )
                colabs = {
                    c.pk: c
                    for c in Colaborador.objects.filter(
                        pk__in={cid for cid, _, _ in CASOS}
                    ).select_related("empresa")
                }
                for cid, c in colabs.items():
                    if c.nome:
                        segredos[c.nome] = "col%d" % cid
                    if c.cpf:
                        segredos[c.cpf] = "[CPF removido]"

                emitir(
                    "CONTEXTO",
                    "app/ponto/management/commands/e6_oraculo.py:304",
                    agora=agora, banco="sombra", schema="juliani",
                    dump_de_hoje=(
                        "[nao verificado pelo Python; "
                        "conferir no envelope V0]"
                    ),
                )

                for cid, mes, iso in CASOS:
                    chave = (cid, iso)
                    contexto = dict(
                        colaborador="col%d" % cid, alvo=iso
                    )
                    try:
                        with transaction.atomic():
                            c = colabs[cid]
                            dia = dt.date.fromisoformat(iso)
                            ini, fim = janelas.janela_fechamento(
                                mes, 2026, c.empresa
                            )
                            if not ini <= dia <= fim:
                                raise ValueError(
                                    "alvo fora da competencia da empresa"
                                )

                            esp = espelho_do_colab(
                                c, hoje, ini, fim, agora=agora
                            )
                            ins = InsumosDoDono(
                                c, ini, fim, hoje,
                                espelho=esp, agora=agora,
                            )

                            # Observa instancias REAIS, sem recalcular papel.
                            # Inclui batidas consumidas sem entrar no retorno.
                            # app/ponto/turnos.py:1217,1315,1371,1988
                            observadas = defaultdict(list)
                            codigo = T.parear_turnos.__code__

                            def observar(frame, evento, argumento):
                                if (
                                    evento == "return"
                                    and frame.f_code is codigo
                                ):
                                    pai = frame.f_back.f_locals
                                    segmento = (
                                        pai.get("seg_ini"),
                                        pai.get("seg_fim"),
                                    )
                                    for b in frame.f_locals.get("bs", ()):
                                        observadas[b.pk].append(
                                            (b, segmento)
                                        )

                            perfil_anterior = sys.getprofile()
                            try:
                                sys.setprofile(observar)
                                ts = ins.turnos()
                            finally:
                                sys.setprofile(perfil_anterior)

                            vizinhas = [
                                dia + dt.timedelta(days=i)
                                for i in (-1, 0, 1)
                            ]
                            a, z = vizinhas[0], vizinhas[-1]
                            cels = celulas_do_periodo([cid], a, z)
                            ecs = escalas_do_periodo(
                                cid, a, z,
                                mais=[
                                    cel.escala_geradora_id
                                    for cel in cels.values()
                                ],
                            )
                            por_data = {
                                d["data"]: d for d in esp["dias"]
                            }
                            lo = timezone.make_aware(
                                dt.datetime.combine(a, dt.time.min)
                            )
                            hi = timezone.make_aware(
                                dt.datetime.combine(
                                    z + dt.timedelta(days=1),
                                    dt.time.min,
                                )
                            )
                            cruas = list(
                                Batida.objects.filter(
                                    colaborador=c,
                                    timestamp__gte=lo,
                                    timestamp__lt=hi,
                                ).order_by("timestamp", "pk")
                            )
                            emitir(
                                "CASO",
                                "app/ponto/management/commands/e6_oraculo.py:328",
                                **contexto,
                                competencia=mes, ini=ini, fim=fim,
                            )

                            for d in vizinhas:
                                cel = cels.get((cid, d))
                                ec = vinculo_do_dia(
                                    cid, d, ecs, celulas=cels
                                )
                                ata = (cel.ata or {}) if cel else {}
                                emitir(
                                    "CELULA",
                                    "app/escala/models.py:1672;"
                                    "app/escala/models.py:1163",
                                    **contexto, dia=d,
                                    existe=cel is not None,
                                    trabalha=cel.trabalha if cel else None,
                                    geradora_id=(
                                        cel.escala_geradora_id
                                        if cel else None
                                    ),
                                    vinculo_do_dia_id=ec.pk if ec else None,
                                    tipo_base=(
                                        ec.tipo_escala.tipo_base
                                        if ec and ec.tipo_escala else None
                                    ),
                                    marcos_dna=(
                                        CelulaDia.marcos_presentes(
                                            (cel.dna or {}).get("marcos")
                                        ) if cel else None
                                    ),
                                    marcos_do_vinculo=(
                                        ec.marcos_do_dia(d, celulas=cels)
                                        if ec else None
                                    ),
                                    intervalo_efetivo=(
                                        ec.intervalo_do_dia(d, celulas=cels)
                                        if ec else None
                                    ),
                                    pausas_declaradas=(
                                        ec.pausas_do_dia(d, celulas=cels)
                                        if ec else None
                                    ),
                                    veredito_gravado=(
                                        cel.veredito if cel else None
                                    ),
                                    ata={
                                        k: ata[k] for k in (
                                            "tipo_dia",
                                            "minutos_previstos",
                                            "minutos_realizados",
                                            "n_missing", "n_orfas",
                                            "sem_ancora",
                                        ) if k in ata
                                    },
                                )

                                de = por_data.get(d)
                                m, fonte = minutos_do_espelho(de)
                                k = d.isoformat()
                                v = ins.oraculo
                                real = _realizado_do_dia_tela(
                                    c, d, ts, ecs, cels
                                )
                                emitir(
                                    "ESPELHO_E6",
                                    "app/ponto/services/espelho.py:267;"
                                    "app/ponto/management/commands/e6_oraculo.py:263;"
                                    "app/ponto/calculador/nucleo.py:236",
                                    **contexto, dia=d,
                                    linha_existe=de is not None,
                                    espelho={
                                        campo: (de or {}).get(campo)
                                        for campo in (
                                            "minutos_realizados", "pago_h",
                                            "veredito_dia", "palavra_dia",
                                            "folga", "feriado",
                                            "realizado_aberto",
                                            "realizado_sem_turno",
                                            "realizado_janela_descontada",
                                            "falta_marcos",
                                            "realizado_grade",
                                        )
                                    },
                                    batidas_cruas_no_dia=sum(
                                        timezone.localtime(
                                            b.timestamp
                                        ).date() == d
                                        for b in cruas
                                    ),
                                    minutos_lidos=m,
                                    fonte_minutos=fonte,
                                    realizado_com_turnos_do_juiz=(
                                        real._asdict()
                                    ),
                                    turnos_exibidos=[
                                        dict(
                                            entrada=p.entrada,
                                            saida=p.saida,
                                            aberto=p.turno_aberto,
                                        )
                                        for p in (de or {}).get("turnos", ())
                                    ],
                                    e6_minutos=v.minutos.get(k, 0.0),
                                    e6_horas=v.horas.get(k, []),
                                    e6_envelope=v.envelope.get(k, 0.0),
                                    e6_pausa=v.pausa.get(k

[truncado em 20000 de 30653 caracteres]

## 16/11 23:30 · sessao 08cb9dbda68f · ESPELHO-VERDADE-NA-BRECHA · leitura · origem=Code

**As listas se separam no código; a reprodução de col922 e os números da frota nesta cópia continuam [nao verificado].** A separação está em `app/ponto/services/espelho.py:421-422` versus `:614`.

- **(a) REALIZADO:** `minutos_do_espelho` prefere `minutos_realizados`, depois `pago_h × 60` (`app/ponto/management/commands/e6_oraculo.py:263-269`). O primeiro nasce em `_realizado_do_dia_tela` → `realizado_do_dia` → `realizado_dos_turnos`, que seleciona `data_turno == data` e soma os líquidos (`app/ponto/services/espelho.py:267-294`, `:675-676`, `:783`; `app/ponto/turnos.py:719`, `:735-771`, `:795`).

- **(a) TELA:** a view `espelho_ponto` chama `espelho_do_colab` e entrega seus dias a `ponto/espelho.html` (`app/ponto/views.py:160-163`, `:195-200`). `montar_dias` monta `dia.turnos` com **períodos do motor**, remapeados por entrada → batida → dia do juiz (`app/ponto/services/espelho.py:421-422`, `:450-460`, `:656`, `:767`). Precisão: o template usa esses turnos para status/alertas; os horários vêm de `realizado_grade`, adaptado por `montar_realizado_grade`, ou de `dia.batidas` (`app/templates/ponto/espelho.html:192-204`, `:231-247`, `:298-305`; `app/escala/utils.py:1078-1110`).

- **(b) SEPARAÇÃO:** o cálculo recebe diretamente os turnos do juiz; o desenho recebe o resultado do motor (`app/ponto/services/espelho.py:614`, `:421-422`). Dentro do motor, `_periodos_pelo_marco` pode substituir **todos os períodos daquele dia** por **um período da ata**; o cálculo do realizado não percorre essa transformação (`app/ponto/motor_calculo_v2.py:530-534`, `:616`, `:679-688`; `app/ponto/services/espelho.py:614`). Sua execução como causa dos dois casos: [nao verificado].

  Não é uma oposição simples entre dia civil e dia do turno: a tela também consulta o juiz; batidas não reclamadas recebem dia civil, e a transformação do motor usa dia civil quando falta a resposta do mapa (`app/ponto/services/espelho.py:450-460`; `app/ponto/turnos.py:2957-2967`; `app/ponto/motor_calculo_v2.py:684-686`). Ambos excluem retratadas, mas os envelopes diferem: motor começa em `ini−1`; juiz em `ini−2` (`app/ponto/services/espelho.py:1019-1022`; `app/ponto/turnos.py:1847`, `:1867-1871`). O realizado lê célula/vínculo para intervalo; o pareador lê o papel da ata; o motor também lê a célula para substituir períodos (`app/ponto/services/espelho.py:286-294`; `app/ponto/turnos.py:1944`, `:1997-2000`; `app/ponto/motor_calculo_v2.py:616`). Qual dessas diferenças decidiu col922: [nao verificado].

- **(c) PADRÃO:** a omissão relatada contraria **Single Source of Truth / um juiz por pergunta**; a montagem paralela da testemunha corresponde ao termo **Duplicated business logic / logic drift**. A separação de listas, isoladamente, não prova a omissão nesta base (`app/docs/GLOSSARIO.md:11`, `:25`; `app/docs/PLANO-PADROES.md:55-59`; reprodução: [nao verificado]).

A sonda define `classe_mesma` como realizado completo diferente do **mesmo realizado real restrito às entradas de `dia.turnos`**. Isso evita confundir realizado com dinheiro do motor; o adaptador da grade não fornece um somador de duração (`app/ponto/services/espelho.py:398-402`; `app/escala/utils.py:1078-1110`, `:1113-1125`). A frota segue a seleção do E6: empresas ativas e colaboradores com fechamento na competência, sem `--so-txt`, usando o corte de cada empresa (`app/ponto/management/commands/e6_oraculo.py:303-314`; `app/ponto/janelas.py:54-57`). Execução na sombra e contagens: [nao verificado].

```python
# Django shell, SOMBRA/V0, base 520d7b557. Sem escrita de arquivos.
# Fontes: app/ponto/services/espelho.py:614,675,767,783,1225
#         app/ponto/turnos.py:707,719,725,770,795
# classe_mesma: realizado completo != mesmo realizado restrito
# as entradas de dia.turnos. Nao compara realizado com dinheiro.
# app/ponto/services/espelho.py:398-402; app/escala/utils.py:1113-1125

import datetime as dt
import inspect
import logging
import re
import sys
from collections import Counter
from zoneinfo import ZoneInfo

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador, Empresa
from ponto.models import Batida, FechamentoMensal
from ponto.janelas import janela_fechamento
from ponto.services import espelho as svc
from ponto import turnos as tj
from ponto import motor_calculo_v2 as mv2
from ponto.management.commands.e6_oraculo import minutos_do_espelho


ALVOS = {dt.date(2026, 11, 1), dt.date(2026, 11, 4)}
erros = 0
exemplos = []
numeros = {}
cobertura = {}
redacoes = []
vistos_alvos = set()


def erro(contexto, exc):
    global erros
    erros += 1
    msg = str(exc)
    for nome, alias in redacoes:
        msg = re.sub(re.escape(nome), alias, msg, flags=re.I)
    msg = re.sub(
        r"(?<!\d)\d{3}\.?\d{3}\.?\d{3}-?\d{2}(?!\d)",
        "[CPF removido]", msg,
    )
    print("ERRO", contexto, type(exc).__name__, msg.replace("\n", " "))


def hora(x):
    return "-" if x is None else timezone.localtime(x).isoformat()


def borda(b):
    return None if b is None else b.timestamp


def linha_unica(funcao, trecho):
    linhas, inicio = inspect.getsourcelines(funcao)
    achadas = [inicio + i for i, s in enumerate(linhas) if trecho in s]
    if len(achadas) != 1:
        raise RuntimeError("base divergente: trecho nao univoco: " + trecho)
    return achadas[0]


def ler_espelho(c, ini, fim, hoje, agora, detalhe):
    # Observa os locais das funcoes reais; nao altera seus argumentos.
    # app/ponto/turnos.py:770; app/ponto/motor_calculo_v2.py:685,689
    soma_ln = linha_unica(tj.realizado_dos_turnos, "total += liquido")
    antes_ln = linha_unica(
        mv2._periodos_pelo_marco,
        "periodos = [(e, x) for (e, x) in periodos",
    )
    depois_ln = linha_unica(
        mv2._periodos_pelo_marco, "itv = resp.get('intervalo')",
    )
    obs = {"reais": {}, "marco": []}
    parcelas = {}
    anteriores = {}

    def observar(frame, evento, valor):
        codigo = frame.f_code
        if evento == "call":
            if codigo is svc.montar_dias.__code__:
                frame.f_trace_lines = False
                return observar
            if codigo is tj.realizado_dos_turnos.__code__:
                pai = frame.f_back
                avo = pai.f_back if pai else None
                if (
                    avo is not None
                    and avo.f_code is svc._realizado_do_dia_tela.__code__
                ):
                    parcelas[id(frame)] = []
                    frame.f_trace_lines = detalhe
                    return observar
            if detalhe and codigo is mv2._periodos_pelo_marco.__code__:
                frame.f_trace_lines = True
                return observar
            return None

        v = frame.f_locals
        if codigo is tj.realizado_dos_turnos.__code__:
            if evento == "line" and frame.f_lineno == soma_ln:
                parcelas[id(frame)].append((
                    v["t"], v["fim"], v["bruto"], v["pausas"], v["liquido"],
                ))
            if evento == "return" and valor is not None:
                obs["reais"][v["data"]] = {
                    "resultado": valor,
                    "turnos": list(v["do_dia"]),
                    "args": {
                        k: v[k] for k in ("livre", "previsto_min", "intervalo")
                    },
                    "parcelas": parcelas.pop(id(frame), []),
                    "pos": v.get("_pos"),
                }

        elif codigo is svc.montar_dias.__code__ and evento == "return":
            obs["juiz"] = list(v["_turnos_juiz_tela"])
            obs["dia_pk"] = dict(v["_dia_por_pk"])

        elif codigo is mv2._periodos_pelo_marco.__code__:
            chave = id(frame)
            if evento == "line" and v.get("dia") in ALVOS:
                if frame.f_lineno == antes_ln:
                    anteriores[chave] = tuple(v["periodos"])
                elif frame.f_lineno == depois_ln:
                    obs["marco"].append((
                        v["dia"],
                        anteriores.pop(chave),
                        tuple(v["periodos"]),
                    ))
            if evento == "return":
                anteriores.pop(chave, None)
        return observar

    anterior = sys.gettrace()
    if anterior is not None:
        raise RuntimeError("ha tracer ativo; observacao nao instalada")
    try:
        sys.settrace(observar)
        esp = svc.espelho_do_colab(
            c, hoje, ini, fim, ano=2026, modo_gestor=False, agora=agora,
        )
    finally:
        sys.settrace(anterior)
    if "juiz" not in obs:
        raise RuntimeError("montar_dias nao foi observado")
    return esp, obs


def projetar(dia, real):
    # Recorta representacao; o calculo continua na funcao real.
    # app/ponto/services/espelho.py:767; app/ponto/turnos.py:707
    entradas = {p.entrada for p in dia["turnos"]}
    todos = real["turnos"]
    faltam = entradas - {borda(t.entrada) for t in todos}
    if faltam:
        raise RuntimeError(
            "periodo da tela sem turno correspondente neste dia: "
            + ", ".join(hora(x) for x in sorted(faltam))
        )
    recorte = [t for t in todos if borda(t.entrada) in entradas]
    resposta = tj.realizado_dos_turnos(
        recorte, dia["data"], **real["args"],
    )
    # Conjunto vazio soma zero; None da fonte original nao vira zero.
    minutos = 0 if not recorte else resposta.minutos
    if minutos is None:
        raise RuntimeError("recorte nao vazio sem minutos")
    return minutos


def mostrar(c, dia, real, obs, minutos_tela):
    data = dia["data"]
    alias = "col%d" % c.pk
    print(
        "ALVO", alias, data,
        "realizado", real["resultado"].minutos,
        "realizado_restrito_a_dia.turnos", minutos_tela,
        "fonte_e6", minutos_do_espelho(dia),
        "intervalo", real["args"]["intervalo"],
        "livre", real["args"]["livre"],
    )

    # Papeis das instancias reais do pareador.
    # app/ponto/turnos.py:1315,1868,2888; app/ponto/models.py:67
    papeis = {}
    for t in obs["juiz"]:
        for b in t.batidas:
            papeis.setdefault(b.pk, []).append((
                t.data_turno, getattr(b, "_papel_ata", None),
            ))

    lo = timezone.make_aware(dt.datetime.combine(
        data - dt.timedelta(days=2), dt.time(),
    ))
    hi = timezone.make_aware(dt.datetime.combine(
        data + dt.timedelta(days=3), dt.time(),
    ))
    for b in Batida.objects.filter(
        colaborador=c, timestamp__gte=lo, timestamp__lt=hi,
    ).order_by("timestamp", "pk"):
        print(
            "BATIDA", alias, "pk", b.pk, hora(b.timestamp),
            "tipo", b.tipo,
            "retratada", hora(b.retratada_em),
            "papel_ata_e_data_turno", papeis.get(b.pk, "[nao verificado]"),
            "dia_tela", obs["dia_pk"].get(b.pk, "[nao verificado]"),
        )

    for t in real["turnos"]:
        print(
            "TURNO_REALIZADO", alias, "data_turno", t.data_turno,
            hora(borda(t.entrada)), hora(borda(t.saida)),
            "aberto", t.aberto,
        )
    for t, fim, bruto, pausa, liquido in real["parcelas"]:
        print(
            "PARCELA_REAL", alias, "data_turno", t.data_turno,
            hora(borda(t.entrada)), hora(borda(fim)),
            "minutos", liquido, "bruto", bruto, "pausa", pausa,
        )
    if real["pos"] is not None:
        t, ult, pausa = real["pos"]
        print(
            "PARCELA_DIARIA_L115", alias, t.data_turno,
            hora(borda(t.entrada)), hora(borda(ult)),
            "minutos", real["resultado"].minutos,
            "pausa", hora(borda(pausa[0])), hora(borda(pausa[1])),
        )

    for p in dia["turnos"]:
        correspondentes = [
            t for t in real["turnos"] if borda(t.entrada) == p.entrada
        ]
        r = tj.realizado_dos_turnos(
            correspondentes, data, **real["args"],
        )
        print(
            "TURNO_TELA", alias, "dia_tela", data,
            "data_turno", [t.data_turno for t in correspondentes],
            hora(p.entrada), hora(p.saida),
            "minutos_mesmo_juiz_isolado", r.minutos,
            "minutos_motor_dinheiro", p.minutos_trabalhados,
        )

    for d, antes, depois in obs["marco"]:
        if d == data:
            print(
                "MARCO_REAL", alias, d,
                "antes", [(hora(a), hora(z)) for a, z in antes],
                "depois", [(hora(a), hora(z)) for a, z in depois],
            )

    # Saida real do adaptador: app/escala/utils.py:1078
    print("GRADE_TELA", alias, data, [
        {k: x.get(k) for k in (
            "hora", "tipo", "tipo_real", "missing", "status",
        )}
        for x in dia["realizado_grade"]
    ])
    print("ORFAS_GRADE_TELA", alias, data, dia["orfas_grade"])
    print("BATIDAS_FALLBACK_TELA", alias, data, [
        (b.pk, hora(b.timestamp), b.tipo) for b in dia["batidas"]
    ])


nivel_log = logging.root.manager.disable
logging.disable(logging.CRITICAL)
try:
    with timezone.override(ZoneInfo("America/Sao_Paulo")), schema_context("juliani"):
        agora = timezone.now()
        hoje = timezone.localtime(agora).date()
        print("ATO", hora(agora), "hoje", hoje, "base", "520d7b557")

        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION READ ONLY")

            redacoes = [
                (nome, "col%d" % pk)
                for pk, nome in Colaborador.objects.values_list("pk", "nome")
                if nome
            ]
            redacoes.sort(key=lambda x: len(x[0]), reverse=True)

            for mes in (10, 11):
                n = 0
                stats = Counter()

                # Universo E6: e6_oraculo.py:303-314; sem --so-txt.
                for empresa in Empresa.objects.filter(ativa=True).order_by("pk"):
                    ini, fim = janela_fechamento(mes, 2026, empresa)
                    ids = FechamentoMensal.objects.filter(
                        mes=mes, ano=2026, colaborador__empresa=empresa,
                    ).values_list("colaborador_id", flat=True)

                    for c in Colaborador.objects.filter(pk__in=ids).order_by("pk"):
                        contexto = "col%d comp%d" % (c.pk, mes)
                        try:
                            with transaction.atomic():
                                esp, obs = ler_espelho(
                                    c, ini, fim, hoje, agora,
                                    c.pk == 922 and mes == 11,
                                )
                        except Exception as exc:
                            erro(contexto, exc)
                            stats["colabs_erro"] += 1
                            continue

                        por_data = {x["data"]: x for x in esp["dias"]}
                        data = ini
                        while data <= fim:
                            atual = data
                            data += dt.timedelta(days=1)
                            if atual > hoje:
                                stats["futuros"] += 1
                                continue
                            try:
                                with transaction.atomic():
                                    dia = por_data[atual]
                                    real = obs["reais"][atual]
                                    if (
                                        real["resultado"].minutos
                                        != dia["minutos_realizados"]
                                    ):
                                        raise RuntimeError(
                                            "retorno observado difere do campo da tela"
                                        )

                                    m = projetar(dia, real)
                                    if (
                                        c.pk == 922 and mes == 11
                                        and atual in ALVOS
                                    ):
                                        mostrar(c, dia, real, obs, m)
                                        vistos_alvos.add(atual)

                                    if real["resultado"].minutos is None:
                                        stats["sem_realizado"] += 1
                                        continue

                                    stats["comparados"] += 1
                                    if real["resultado"].minutos != m:
                                        n += 1
                                        if len(exemplos) < 10:
                                            exemplos.append((
                                                mes, c.pk, atual,
                                                real["resultado"].minutos, m,
                                            ))
                            except Exception as exc:
                                erro(contexto + " " + str(atual), exc)
                                stats["dias_erro"] += 1

                numeros[mes] = n
                cobertura[mes] = dict(stats)

            for data in sorted(ALVOS - vistos_alvos):
                erro(
                    "col922 " + str(data),
                    RuntimeError(
                        "alvo nao medido; conferir universo ou erros anteriores"
                    ),
                )

except Exception as exc:
    erro("sonda", exc)

finally:
    logging.disable(nivel_log)
    for mes in (10, 11):
        print(
            "NUMERO classe_mesma comp%d: %s"
            % (mes, numeros.get(mes, "[nao verificado]"))
        )
        print(
            "COBERTURA comp%d" % mes,
            cobertura.get(mes, "[nao verificado]"),
        )
    for mes, pk, data, m_real, m_tela in exemplos:
        print(
            "EXEMPLO", "col%d" % pk, data, "comp%d" % mes,
            "realizado", m_real, "tela_mesmo_juiz", m_tela,
        )
    print(
        "VEREDITO",
        "INCOMPLETO [nao verificado]" if erros else (
            "DIVERGENCIA REPRODUZIDA"
            if sum(numeros.values()) else "NAO REPRODUZ"
        ),
        "| erros: %d" % erros,
    )
```

## 16/11 23:29 · sessao 2a2b912ac709 · ESPELHO-VERDADE-NA-BRECHA · censo · origem=Code

**E4 permanece EM CURSO:** há exibição do tipo gravado fora da pílula, nos dois ramos do wizard abaixo; o placar também registra esse estado em `app/core/espelho_verdade.py:142–143`.

| Achado | Quem vê | Pílula ou outra coisa |
|---|---|---|
| **Criar sob pai:** imprime `{{ pai.tipo_base }}` em “canônico herdado” — `app/templates/escala/wizard_tipo_escala.html:56`. O pai é carregado pelo ID — `app/escala/views_wizard.py:74`. | Usuário autenticado com ação `editar_escalas` — `app/escala/views_wizard.py:63–64`. | Outra coisa: texto auxiliar do cadastro — `app/templates/escala/wizard_tipo_escala.html:56`. |
| **Editar escala:** imprime `{{ tipo.tipo_base }}` em “Canônico (não editável)” — `app/templates/escala/wizard_tipo_escala.html:61`. O tipo é carregado pelo ID — `app/escala/views_wizard.py:67`. | Mesmo público do wizard — `app/escala/views_wizard.py:63–64`. | Outra coisa: texto auxiliar do cadastro — `app/templates/escala/wizard_tipo_escala.html:61`. |
| **Django admin, formulário de TipoEscala:** configuração implica campo `tipo_base` com seleção do valor gravado e labels das choices: `ModelAdmin` sem exclusão de campos — `app/escala/admin.py:7–15`; campo com choices — `app/escala/models.py:163–174`. **Inferência estática; renderização efetiva [nao verificado].** | Admin do modelo, registrado em `app/escala/admin.py:7`, pela rota `app/config/urls.py:24`. | Outra coisa: seletor de formulário, pela configuração de `app/escala/admin.py:8`. |
| **Mensagens condicionais:** quando `tipo_base == 'comercial'`, a validação escreve “tipo base 'Comercial'” em duas recusas — `app/escala/models.py:358–367`; o wizard encaminha essas mensagens ao usuário — `app/escala/views_wizard.py:110–112`. | Usuário com `editar_escalas` — `app/escala/views_wizard.py:95–98`; entrega por toast ou mensagem — `app/core/respostas.py:63–69`. | Outra coisa: aviso de validação, sem consultar o desenho para nomear a base — `app/escala/models.py:358–367`. |

Os dois textos do wizard aparecem tanto na página inteira quanto no drawer, pelo mesmo template — `app/escala/views_wizard.py:88–92`.

Também inventariados, com distinção de origem:

- **Seletor “Base”:** imprime labels de `TipoEscala.TIPO_BASE_CHOICES` — `app/templates/escala/wizard_tipo_escala.html:36–37`, fonte em `app/escala/views_wizard.py:79`. É vocabulário de cadastro, disponível somente na criação sem pai, e não leitura de registro existente — `app/templates/escala/wizard_tipo_escala.html:26`.
- **“Escala-base de referência”:** labels literais das bases — `app/templates/escala/wizard_tipo_escala.html:42–47`; também está nesse ramo de criação — `app/templates/escala/wizard_tipo_escala.html:26`.
- **Hidden, legenda e manual:** não são rótulos derivados de um registro exibido: hidden em `app/templates/escala/wizard_tipo_escala.html:19`; legenda fixa em `app/templates/escala/tipos_lista.html:106–109`; texto fixo em `app/templates/core/manual.html:127`.

**A pílula protegida:** o texto lê `desenho_do_turno` — `app/templates/escala/tipos_lista.html:55`; o filtro delega a `rotulo_do_desenho` — `app/core/templatetags/hasner_filters.py:59–60`, que decide pelos horários e intervalo — `app/escala/servico_jornada.py:189–205`.

**A cor vem do tipo gravado:** `12x36` azul, `comercial` verde, `plantao` amarelo, demais cinza — `app/templates/escala/tipos_lista.html:51–54`.

Nos caminhos de PDF, CSV e API examinados, não apareceu outro emissor de `tipo_base`: PDF usa código/ciclo/horário — `app/relatorios/pdf_espelho.py:730–740`; builder de relatórios usa `str(tipo_escala)` — `app/relatorios/views.py:1317–1335`, cuja implementação retorna nome canônico ou código/descrição — `app/escala/models.py:801–804`; APIs usam nome e ciclo — `app/api/views_core.py:254–267`, `app/api/views.py:948–957`.

O selo O74 verifica o texto nas linhas-raiz da lista — `app/escala/tests/test_o74_pilula_le_desenho.py:59–76`. **Verde no HEAD: [nao verificado]**, não executado nesta leitura.

## 16/11 23:24 · sessao ee701c2b51bc · - · - · origem=Ronald (tmux)

Achado indireto em Python:

| Saída | Quem vê | Evidência | Pílula? |
|---|---|---|---|
| Formulário de detalhe/edição do `TipoEscala` no Django admin: campo `tipo_base`, com seleção do valor gravado e rótulos das choices | Admin com acesso ao modelo | Registro com `ModelAdmin` sem exclusão/customização de campos: `app/escala/admin.py:7`; campo e choices: `app/escala/models.py:163`, `app/escala/models.py:170`; comentário confirma ModelForm do admin: `app/escala/services/cadastro_tipo.py:383`; rota: `app/config/urls.py:24` | Outra coisa: campo de formulário |

As choices são “12x36 — vigilância padrão”, “Turno partido — dois períodos/dia”, “Comercial — horário fixo semanal”, “Plantão — 24x48 ou similar” e “Intermitente-horista — apenas horas cumpridas (par E/S)” (`app/escala/models.py:164`).

Caso limítrofe: mensagens de validação dizem “tipo base 'Comercial'” quando a base é `comercial` (`app/escala/models.py:358`, `app/escala/models.py:361`, `app/escala/models.py:366`). A edição preserva a base (`app/escala/services/cadastro_tipo.py:179`), chama `save()` (`app/escala/services/cadastro_tipo.py:307`), que executa `full_clean()` (`app/escala/models.py:798`); a view entrega `e.messages` ao usuário (`app/escala/views_wizard.py:110`) por toast/messages (`app/core/respostas.py:63`, `app/core/respostas.py:68`). É aviso condicional de validação, não identificador do tipo nem pílula (`app/escala/models.py:360`).

Saídas verificadas que **não apresentam `tipo_base`**:

- API `/me`: `nome_canonico()` e `tipo_ciclo` (`app/api/views_core.py:254`); API de escala: `template_nome`/`descricao` e display de `tipo_ciclo` (`app/api/views.py:948`).
- PDF do espelho: código, `tipo_ciclo` e horários (`app/relatorios/pdf_espelho.py:730`).
- Builder de relatórios, coluna Escala: `str(ec.tipo_escala)` (`app/relatorios/views.py:1317`, `app/relatorios/views.py:1335`); `__str__` retorna nome canônico ou código/descrição (`app/escala/models.py:801`), e nome canônico usa `tipo_ciclo_canonico()` (`app/escala/models.py:424`).
- Tag `rotulo_escala`: parte `tipo` vem de `nome_canonico_partes()` (`app/core/templatetags/hasner_filters.py:72`), que lê ciclo (`app/escala/models.py:484`); o descritor adicional chama `rotulo_do_desenho()` (`app/core/templatetags/hasner_filters.py:145`).

## 16/11 23:24 · sessao ad6cef41af58 · - · - · origem=Ronald (tmux)

Tela: `espelho_ponto` chama `espelho_do_colab` e entrega `dias` ao template; use diretamente esse serviço na sonda, preservando o caminho real sem simular request. `app/ponto/views.py:160-163`, `app/ponto/views.py:195-200`, `app/ponto/services/espelho.py:1294-1296`.

Separação concreta: o realizado carrega `turnos_do_colab` diretamente e usa `_realizado_do_dia_tela`; a geometria `dia.turnos` provém dos períodos do motor. `app/ponto/services/espelho.py:614`, `app/ponto/services/espelho.py:675-676`, `app/ponto/services/espelho.py:421-422`, `app/ponto/services/espelho.py:450-460`, `app/ponto/services/espelho.py:767`.

O motor começa pelos turnos da autoridade e passa por `_periodos_pelo_marco`; quando a ata responde, remove **todos** os períodos cujo dia do juiz corresponde ao dia e acrescenta **um** período entrada–saída da ata. Esse mecanismo pode produzir a forma “dois turnos no realizado, um no motor”; sua execução específica para col922 depende da sonda [nao verificado]. `app/ponto/motor_calculo_v2.py:530-534`, `app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:636-640`, `app/ponto/motor_calculo_v2.py:679-688`.

Não atribuir isso a retratada: ambos os carregamentos excluem `retratada_em` preenchida. `app/ponto/services/espelho.py:1022`, `app/ponto/turnos.py:2888-2895`, `app/ponto/turnos.py:1868-1871`.

Precisão necessária na resposta: o template não desenha início–fim de `dia.turnos`; usa essa lista para badges/alertas. Os horários visíveis vêm prioritariamente de `realizado_grade`, depois de `dia.batidas`. `app/templates/ponto/espelho.html:192-204`, `app/templates/ponto/espelho.html:298-305`, `app/templates/ponto/espelho.html:231-247`.

`montar_realizado_grade` só adapta células e órfãs; não devolve turnos nem minutos. A função antiga que calculava minutos da grade foi removida para evitar um segundo juiz. Portanto, a métrica “minutos dos turnos desenhados” precisa declarar que lê `dia.turnos` e restringe o realizado real a esses turnos, ou a contagem confundirá dinheiro do motor com geometria visível. `app/escala/utils.py:1078-1110`, `app/escala/utils.py:1113-1125`.

A janela real do request atual vai de `min(piso_visual, apur_ini)` até hoje, mesmo navegando competência anterior; passar `hoje=apur_fim` muda esse caminho. `app/ponto/views.py:100-101`, `app/ponto/views.py:161`, `app/ponto/services/espelho.py:1225-1227`, `app/ponto/services/espelho.py:1262-1263`.

## 16/11 23:24 · sessao 54c77838a647 · - · - · origem=Ronald (tmux)

Achados independentes:

| Saída | Prova | Quem vê | Classe |
|---|---|---|---|
| Wizard criando sob pai: mostra `pai.tipo_base` gravado, em “canônico herdado” | `app/templates/escala/wizard_tipo_escala.html:56` | Usuário autenticado autorizado em `editar_escalas` — `app/escala/views_wizard.py:63` e `:64` | Outra coisa: texto auxiliar |
| Wizard editando: mostra `tipo.tipo_base` gravado | `app/templates/escala/wizard_tipo_escala.html:61` | Mesmo público — `app/escala/views_wizard.py:63` e `:64` | Outra coisa: texto auxiliar |
| Wizard criação sem pai: opções de Base vêm de `TIPO_BASE_CHOICES` | `app/templates/escala/wizard_tipo_escala.html:37`; fonte `app/escala/views_wizard.py:79`, rótulos em `app/escala/models.py:163` | Mesmo público | Seletor de cadastro: exibe vocabulário gravável, mas não rótulo de registro já gravado; ramo restrito por `wizard_tipo_escala.html:26` |
| Wizard criação personalizada: opções “escala-base de referência” são rótulos literais | `app/templates/escala/wizard_tipo_escala.html:42`, `:44`, `:45`, `:46`, `:47` | Mesmo público | Seletor de cadastro, mesma ressalva; também dentro do ramo de criação de `:26` |

Os achados do wizard existem tanto na página inteira quanto no drawer: `app/escala/views_wizard.py:88`, `:89`, `:90`, `:91`, `:92`.

A pílula protegida lê `desenho_do_turno` em `app/templates/escala/tipos_lista.html:55`, delega a `rotulo_do_desenho` em `app/core/templatetags/hasner_filters.py:59` e `:60`; a função decide pelo intervalo e cruzamento da meia-noite em `app/escala/servico_jornada.py:193` e `:194`, monta rótulo em `:195` a `:205`.

A **cor continua vindo do tipo gravado**: azul para `12x36` (`app/templates/escala/tipos_lista.html:51`); verde para `comercial` (`:52`); amarelo para `plantao` (`:53`); cinza para os demais (`:54`).

Exclusões fundamentadas:

- `tipo_base` em inputs hidden não é exibição ao usuário: `app/templates/escala/wizard_tipo_escala.html:19`.
- A pílula azul do tag `rotulo_escala` usa `p['tipo']` (`app/core/templatetags/hasner_filters.py:81`), que vem de `tipo_ciclo_canonico()` (`app/escala/models.py:484`), mapeamento do ciclo (`app/escala/models.py:389`), e não de `tipo_base`.
- `rotulo_escala_texto` também lê `nome_canonico_partes()` (`app/core/templatetags/hasner_filters.py:207`), portanto sua parte “tipo” tem a mesma origem em ciclo (`app/escala/models.py:484`).
- O teste O74 verifica texto nas linhas-raiz (`app/escala/tests/test_o74_pilula_le_desenho.py:59`, `:65`, `:66`, `:69`, `:74`, `:75`); verde no HEAD não foi executado [nao verificado].

## 16/11 23:23 · sessao 1181ba86e409 · - · - · origem=Ronald (tmux)

Censo dos templates/tags: **2 pontos de exibição de valor gravado**, ambos no wizard, fora da pílula protegida:

| Ponto | Quem vê | Conteúdo |
|---|---|---|
| `app/templates/escala/wizard_tipo_escala.html:56` | Usuário autenticado com ação `editar_escalas` — `app/escala/views_wizard.py:63-64` | Ao criar sob pai, mostra `{{ pai.tipo_base }}` cru em “canônico herdado”. Pai carregado do banco — `app/escala/views_wizard.py:73-74`. |
| `app/templates/escala/wizard_tipo_escala.html:61` | Usuário autenticado com ação `editar_escalas` — `app/escala/views_wizard.py:63-64` | Ao editar, mostra `{{ tipo.tipo_base }}` cru em “Canônico (não editável)”. Tipo carregado do banco — `app/escala/views_wizard.py:67`. |

São a mesma página, também servida como drawer conforme `HX-Request` — `app/escala/views_wizard.py:88-92`; rotas criar e editar em `app/escala/urls.py:19,23`.

**Vocabulário de cadastro, separado do censo de valores gravados:** o `<select>` Base lê `bases` e imprime suas labels — `app/templates/escala/wizard_tipo_escala.html:36-37`; `bases` vem de `TipoEscala.TIPO_BASE_CHOICES` — `app/escala/views_wizard.py:79`, labels em `app/escala/models.py:163-168`. Esse ramo só aparece ao criar sem pai — `app/templates/escala/wizard_tipo_escala.html:26`, portanto não mostra o tipo gravado de registro existente. O seletor `tipo_base_ref` também só é cadastro; labels literais em `app/templates/escala/wizard_tipo_escala.html:42-47`.

**Pílula O74:** texto usa `t|desenho_do_turno` — `app/templates/escala/tipos_lista.html:55`; filtro delega a `rotulo_do_desenho` — `app/core/templatetags/hasner_filters.py:52-60`. Cor vem do **tipo gravado**: `12x36` azul, `comercial` verde, `plantao` amarelo, demais cinza — `app/templates/escala/tipos_lista.html:51-54`. Público autenticado com `ver_escalas` — `app/escala/views.py:77-79`.

**Outra pílula/tag que não é achado:** `rotulo_escala` constrói a pílula com `p['tipo']` — `app/core/templatetags/hasner_filters.py:72-81`; esse valor vem de `tipo_ciclo_canonico()`, não `tipo_base` — `app/escala/models.py:479-486`; seu descritor adicional também lê o desenho — `app/core/templatetags/hasner_filters.py:145-150`.

**Admin implícito a verificar na consolidação:** `TipoEscalaAdmin` é `admin.ModelAdmin` sem `fields`, `fieldsets`, `exclude` ou formulário próprio — `app/escala/admin.py:7-15`; modelo declara `tipo_base` com choices — `app/escala/models.py:170-174`. Isso implica widget padrão de choices na edição Django Admin, mas a renderização concreta do framework nesta cópia é **[não verificado]**.

## 16/11 23:23 · sessao 0de1d50dfbd0 · - · - · origem=Ronald (tmux)

Recomendações para a sonda:

- Chamar `turnos_do_colab(c, ini, fim)`; o wrapper carrega células, vínculos e papéis reais e exclui retratadas. O retorno contém `Turno(data_turno, entrada, saida, batidas, aberto, cross_meianoite)`. Serializar IDs das bordas e batidas, sem recalcular casamento. `app/ponto/turnos.py:43`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1920`.
- Ler os carimbos nas instâncias de `t.batidas`: `_papel_ata`, `_intra_dur`, `_intra_ata`, `_intra_declarado`, `_eco_flush`, mais `is_intervalo`. São respostas do pareador; ele trabalha com cópias por segmento. `app/ponto/turnos.py:1310`, `app/ponto/turnos.py:1325`, `app/ponto/turnos.py:1370`, `app/ponto/turnos.py:1984`.
- `intervalos_do_turno(t)` entrega as pausas reconhecidas; `pausas_abaixo_do_piso(t)` entrega os pares marcados rejeitados pelo piso. Não derivar pausas examinando tipos adjacentes. `app/ponto/turnos.py:542`, `app/ponto/turnos.py:563`.
- Batida crua ausente dos turnos retornados deve sair como “ausente dos turnos retornados”; isso não demonstra que não recebeu papel. O pareador consome intervalo/`X` sem turno corrente e pode não incorporá-lo a retorno algum. Para imprimir o papel da ata desses casos, chamar `papel_por_minuto_da_ata(...)` e `_papel_da_ata(b, mapa)`, preservando o nome “papel da ata”. `app/ponto/turnos.py:1371`, `app/ponto/turnos.py:1395`, `app/ponto/turnos.py:890`, `app/ponto/turnos.py:1565`.
- Universo cru com retratadas deve ser lido separadamente por `Batida`; imprimir `timestamp` e `retratada_em` por `timezone.localtime`. Retratada não entra na geometria, mas continua exibível. `app/ponto/models.py:23`, `app/ponto/models.py:64`, `app/ponto/turnos.py:1868`, `app/ponto/turnos.py:45`.
- Célula: imprimir `trabalha`, `escala_geradora_id` e `CelulaDia.marcos_presentes((cel.dna or {}).get("marcos"))`. Evitar imprimir DNA inteiro. `app/escala/models.py:1671`, `app/escala/models.py:1685`, `app/escala/models.py:1692`, `app/escala/models.py:1697`.
- Pausa efetivamente declarada: chamar `ec.intervalo_do_dia(d, celulas=mapa_composto)` ou `ec.pausas_do_dia(...)`. Célula existente sem marcos responde nenhuma pausa; `marcos_do_dia` pode retornar marcos do template nessa mesma situação, por isso imprimir ambos com rótulos distintos. `app/escala/models.py:1124`, `app/escala/models.py:1160`, `app/escala/models.py:1182`, `app/escala/models.py:1202`.
- Número do realizado pelo caminho exato da tela: `_realizado_do_dia_tela(c, d, turnos, escalas_periodo, celulas)`, que chama o juiz real passando vínculo, modo livre e intervalo declarado do dia. `app/ponto/services/espelho.py:267`, `app/ponto/services/espelho.py:286`, `app/ponto/services/espelho.py:290`.

Armadilha de erros: `papel_por_minuto_da_ata` captura silenciosamente exceções de `periodos_do_dia` e se abstém; o `except` externo da sonda não verá esses erros internos. Caso a sonda pretenda contabilizá-los, precisa chamar também `periodos_do_dia(cel, te)` sob seu próprio contador, sem substituir a regra real. `app/ponto/turnos.py:1631`.

## 16/11 23:23 · sessao fae23013399b · - · - · origem=Ronald (tmux)

- O leitor do E6 é `minutos_do_espelho`: prioriza `minutos_realizados`; só usa `pago_h * 60` quando o primeiro é `None`; sem ambos retorna ausência, não zero (`app/ponto/management/commands/e6_oraculo.py:242`, `:263-269`).
- O campo `minutos_realizados` vem da chamada de `_realizado_do_dia_tela`, que usa `vinculo_do_dia`, `tipo_escala._intervalo_livre()` e `ec.intervalo_do_dia(...)`; não há cálculo próprio no wrapper (`app/ponto/services/espelho.py:267-294`, `:675-676`, `:783`).
- A autoridade final é `realizado_do_dia → realizado_dos_turnos` (`app/ponto/turnos.py:774-795`). A seleção diária usa **`t.data_turno == data`** (`:719`). O ramo usual obtém o fim por `_fim_fechado`, desconta `intervalos_do_turno` ou janela declarada e acumula `liquido` (`:735-771`). Há ramo diário especial L-115 por `_borda_posicional`, anterior à soma usual (`:725-732`).
- Os turnos usados por essa conta vêm de `turnos_do_colab(colaborador, data_ini, data_fim_mes)` (`app/ponto/services/espelho.py:611-614`). Essa função consulta batidas **sem retratação**, com janela de pad de dois dias antes e um depois, carrega células e escalas e delega ao corpo único `_turnos_do_colab_puro` (`app/ponto/turnos.py:1847`, `:1850-1920`). O corpo lê o papel da ata em `papel_por_minuto_da_ata` (`:1940-1944`).
- O desenho usa **outra lista**: o dicionário `resultados_por_entrada` recebe `resultado_v2.periodos + resultado_v2.periodos_ft` (`app/ponto/services/espelho.py:421-422`). A ponte entrada → pk da batida → `dia_das_batidas` atribui cada período ao dia (`:450-460`); `turnos_dia` é essa lista diária (`:655-656`), exportada como `dia['turnos']` (`:767`).
- Portanto, existe separação verificável de fontes entre períodos do motor (`app/ponto/services/espelho.py:421-422`) e turnos do juiz (`:614`). O uso do juiz para **atribuir datas** aos cards (`:450-460`) não converte os períodos do motor nos turnos do juiz. A causa específica da ausência dos turnos de col922 precisa da execução da sonda [nao verificado].
- As duas cadeias filtram retratadas: o motor recebe `batidas_apuraveis` (`app/ponto/services/espelho.py:1019-1022`); os turnos do juiz filtram `retratada_em__isnull=True` (`app/ponto/turnos.py:1867-1871`). A diferença de filtros de retratação não está provada como causa.
- Os envelopes carregados diferem: motor usa `[ini - 1 dia, fim 23:59:59 + 1 dia]` (`app/ponto/services/espelho.py:1019-1022`); juiz usa `janela_com_pad`, que começa em `ini - 2 dias` (`app/ponto/turnos.py:1847`, `:1867-1871`). Isso é diferença de carga comprovada; sua influência em col922 é [nao verificado].
- Para obter parcelas **sem replicar regras**, uma sonda pode observar `realizado_dos_turnos.__code__` via `sys.settrace`: no evento de linha de `total += liquido` captura `t`, `fim`, `bruto`, `pausas` e `liquido` já calculados pelo código real (`app/ponto/turnos.py:770`). No retorno especial captura `_t0`, `_ult`, `_bruto`, `_pausa` e o resultado L-115 (`:725-732`). Deve rotular esse ramo como parcela diária, pois ele não distribui contribuição por turno.
- Para contar a **mesma classe de geometria**, não comparar diretamente `minutos_realizados` com `p.minutos_trabalhados`: o próprio código distingue FATO de dinheiro apurado e registra exemplo de valores diferentes legítimos (`app/ponto/services/espelho.py:398-402`). Comparação fiel: chamar novamente `_realizado_do_dia_tela` com os **mesmos** escalas/células e somente os turnos do juiz representados pelos cards reais, preservando a autoridade de intervalo e a regra L-115 (`:267-294`; `app/ponto/turnos.py:725-732`). Eventual card sem correspondência deve gerar `ERRO`, não ser descartado em silêncio [nao verificado].

## 16/11 23:22 · sessao 75ff2010922c · - · - · origem=Ronald (tmux)

Padrões pertinentes:

- **Single Source of Truth / Aggregate Root**, alias “célula soberana / fonte única”: `app/docs/GLOSSARIO.md:11`; o norte explicita **single source of truth / um juiz por pergunta** em `app/docs/PLANO-PADROES.md:55-59`. Se as montagens do mesmo turno entregam universos incompatíveis para número e testemunha, esse é o padrão principal quebrado — interpretação da separação a demonstrar no código.
- **Duplicated business logic / logic drift**, alias “derivação paralela”, proibida pela LEI-AKITA 2: `app/docs/GLOSSARIO.md:25`. Aplicável se a separação revelar dois derivadores concorrentes da mesma pergunta; não basta constatar duas funções com finalidades diferentes.
- **CQRS com projeções reconstruíveis** inclui explicitamente espelho, DiaPago, fechamento, retrato e pílulas: `app/docs/GLOSSARIO.md:24`; O-PROJECOES exige fonte, versão do juiz e comando de reconstrução: `app/docs/PLANO-PADROES.md:11`. A divergência sozinha não prova defeito de versão ou reconstrução.
- **Immutable event log + compensating event** vincula “batida só retratada” ao universo `batidas_apuraveis`: `app/docs/GLOSSARIO.md:27`. Só nomear como quebra se houver diferença concreta no tratamento da retratada.
- O selo deve verificar o **significado da classe** — dia da batida, competência, vínculo do dia, retratada — nunca apenas a forma: `app/docs/PLANO-PADROES.md:9`.
- Não atribuir automaticamente quebra de **Single Writer Principle**: o glossário identifica esse padrão em `app/docs/GLOSSARIO.md:12`, mas duas projeções divergentes não provam dois escritores.

Universo da frota usado pelo E6:

```python
for empresa in Empresa.objects.filter(ativa=True).order_by("pk"):
    ini, fim = janelas.janela_fechamento(mes, ano, empresa)
    ids = FechamentoMensal.objects.filter(
        mes=mes, ano=ano, colaborador__empresa=empresa
    ).values_list("colaborador_id", flat=True)
    for colaborador in Colaborador.objects.filter(pk__in=ids):
        ...
```

É a seleção real em `app/ponto/management/commands/e6_oraculo.py:303-314`: empresas ativas e colaboradores com FechamentoMensal daquela competência/empresa; não filtra `Colaborador.ativo`. `--so-txt` é filtro adicional opcional em `app/ponto/management/commands/e6_oraculo.py:307-313`.

As competências devem chamar `janela_fechamento(mes, ano, empresa)`, que aplica o corte próprio da empresa: `app/ponto/janelas.py:54-57`. A delimitação por corte está em `app/ponto/janelas.py:10-21`; não são outubro/novembro civis.

Fonte numérica efetivamente comparada pelo E6: prefere `minutos_realizados`; sem ele usa `pago_h * 60`; sem ambos retorna ausência de lavratura: `app/ponto/management/commands/e6_oraculo.py:263-269`.

A borda usada pelos insumos E6 não é idêntica à janela da tela: `InsumosDoDono` lê `batidas_apuraveis` de início menos 12 h a fim mais um dia e 14 h (`app/ponto/management/commands/e6_oraculo.py:149-153`), e oferece `turnos_do_colab` de início menos um dia a fim mais um dia (`app/ponto/management/commands/e6_oraculo.py:156-160`). Isso pertence à comparação E6 e não deve substituir as funções reais da tela no censo solicitado.

## 16/11 23:22 · sessao d01e48c2ba58 · - · - · origem=Ronald (tmux)

Na cópia, a regra é L-099: separar defeito estrutural de escala/batida; apenas ESTRUTURA entra como obra, CADASTRO/BATIDA vão à lista do administrador (`app/docs/LEIS.md:122`).

A precedência **executada**, que deve orientar a resposta, é:

1. `folga_nao_definida`, `sem_vinculo_no_dia` ou `sem_vinculo_em_dia_de_trabalho` → CADASTRO (`app/ponto/management/commands/e6_oraculo.py:98`).
2. `batida_fora_de_turno=True` → BATIDA (`app/ponto/management/commands/e6_oraculo.py:104`).
3. Quantidade ímpar em `horas_do_dia` atribuídas **pelo E6** → BATIDA (`app/ponto/management/commands/e6_oraculo.py:106`).
4. `fase_deslocada=True`, lida da lavra mais recente que cobriu o dia → CADASTRO (`app/ponto/management/commands/e6_oraculo.py:108`, `app/ponto/management/commands/e6_oraculo.py:174`).
5. Dia presente na lista `dias_cadastro_x_realidade` → CADASTRO; restante → ESTRUTURA (`app/ponto/management/commands/e6_oraculo.py:110`).

A docstring que diz BATIDA primeiro e o comentário do laço de comparação estão incompletos diante das precedências acrescentadas; citar o corpo executável acima (`app/ponto/management/commands/e6_oraculo.py:61`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:374`).

**Chamada correta, sem copiar regra:**

```python
from ponto import janelas
from ponto.services.espelho import espelho_do_colab
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono, minutos_do_espelho, batida_fora_de_turno,
)
from ponto.supra_juiz import cadastro_do_dia_carregado

ini, fim = janelas.janela_fechamento(mes, ano, c.empresa)
esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
ins = InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
turnos = ins.turnos()
dono = ins.dono(dia)
```

Essas mesmas chamadas aparecem no comando real: janela por empresa, espelho e carregador único (`app/ponto/management/commands/e6_oraculo.py:304`, `app/ponto/management/commands/e6_oraculo.py:316`, `app/ponto/management/commands/e6_oraculo.py:328`, `app/ponto/management/commands/e6_oraculo.py:378`).

**Insumos úteis para imprimir:**

- `ins.oraculo.minutos`, `.horas`, `.impares`, `.envelope`, `.brutas`, `.cortes`, `.pausa`; o E6 soma pares por posição e atribui pela primeira batida do fragmento (`app/ponto/calculador/nucleo.py:236`, `app/ponto/calculador/nucleo.py:245`, `app/ponto/calculador/nucleo.py:254`).
- `ins.cxr()`: conjunto das chaves `dia`, sem recalcular distâncias (`app/ponto/management/commands/e6_oraculo.py:162`).
- Linhas originais de `esp["resultado"].dias_cadastro_x_realidade`: cada uma pode trazer causa, real/cadastrado e minutos fora, ou ambas as pontas e deltas (`app/ponto/motor_calculo_v2.py:1622`, `app/ponto/motor_calculo_v2.py:1670`, `app/ponto/motor_calculo_v2.py:1992`).
- `ins.fase()` e `cadastro_do_dia_carregado(c.pk, dia)` são as autoridades reais das outras entradas do juiz (`app/ponto/management/commands/e6_oraculo.py:174`, `app/ponto/supra_juiz.py:179`).
- `batida_fora_de_turno(c, dia, turnos)` não é sinônimo de “qualquer batida não usada”: exige `realizado_do_dia(... livre=False, intervalo=(), turnos=turnos).aberto` **e** entrada real de turno aberto sem saída no universo apurável (`app/ponto/management/commands/e6_oraculo.py:198`, `app/ponto/management/commands/e6_oraculo.py:208`).

CXR não deve ser inferido só por “escala diurna versus batidas noturnas”: o juiz lê a lista publicada pelo motor. A regra das duas pontas exige ambas a mais de 180 minutos dos marcos, e o motor também publica causas de janela/HE na mesma lista (`app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/motor_calculo_v2.py:1946`, `app/ponto/motor_calculo_v2.py:1670`).

`minutos_do_espelho(de)` lê `minutos_realizados`, depois `pago_h*60`; ausentes ambos retorna `(None, "sem lavratura")`, nunca zero (`app/ponto/management/commands/e6_oraculo.py:263`).

O universo E6 usa batidas apuráveis da competência com pad de 12 horas antes e 14 horas depois; `ins.turnos()` usa `turnos_do_colab` com um dia a mais em cada extremidade. Portanto não construir o E6 apenas com as três datas impressas (`app/ponto/management/commands/e6_oraculo.py:149`, `app/ponto/management/commands/e6_oraculo.py:156`).

Nenhum dado fornecido decide os insumos concretos atuais dos 12 casos; seus donos permanecem [nao verificado].

## 16/11 23:20 · sessao 0204aa9d588e · PREPARA-F2-F4-F7 · censo · origem=Code

## 1 DEPENDE e premissas

**BASE examinada: `520d7b55720888d80b46ffbca2ca73e59da40277`.** Os arquivos centrais foram conferidos contra os blobs desse commit. A cópia fornecida não tem `.git`; seu `HEAD` local: [nao verificado]. Banco, execução de sondas, ocorrência em produção e resultado de suíte: [nao verificado].

Os leitores continuam projetivos: `estado`, `respondida`, `resposta_display`, retenção e SQL de vitalidade ainda leem os campos. A migração descrita pela coluna c3 não está nesses corpos. (`app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/models.py:1923`; `app/chamados/models.py:3496`)

Nos quadros abaixo, `estados.md` e `matriz.md` são os anexos fornecidos; as linhas incluem vazias. Todos os demais caminhos são da BASE.

| Origem | Premissa julgada | Veredito na BASE |
|---|---|---|
| `matriz.md:14` | F1 vigente é `be2b8d733` | **CAIU.** A projeção vigente lê os campos atuais; o descarimbo apaga o instante. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1390`; `app/chamados/models.py:3318`) |
| `matriz.md:27` | Existem leitura conservada, helper dos atos e materialização conservada descritos ali | **CAIU.** Os corpos vigentes são leitura dos campos; aqueles helpers não foram localizados nesses módulos. (`app/chamados/juizes.py:1382`; `app/chamados/models.py:3664`; `app/chamados/models.py:3686`) |
| `[DEPENDE-F1]` 1 — `matriz.md:70` | `[3]` conserva o instante, inclusive após descarimbo | **CAIU.** `[3]` é `materializada_em` atual; descarimbar escreve `None`. (`app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1400`; `app/chamados/models.py:3318`) |
| `[DEPENDE-F1]` 2 — `matriz.md:72` | A assinatura conservada é reaplicada no caso passivo | **CAIU como premissa da BASE.** A projeção recorta valores futuros dos campos, sem reaplicar essa assinatura. Resultado do antigo T148: [nao verificado]. (`app/chamados/juizes.py:1391`; `app/chamados/juizes.py:1400`) |
| `[DEPENDE-F1]` 3 — `matriz.md:74` | Não há testemunha E em `[3]`, eliminando aquele relógio | **CONFERE o resultado**, embora a explicação pela F1 histórica tenha caído: `[3]` vem do campo. (`app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1400`) |
| `[DEPENDE-F1]` 4 — `matriz.md:75` | F1 sozinha já lê atos e rompe a continência de `q_vivas` por essa divergência | **CAIU.** C6 e `q_vivas` continuam projetivos; continência continua sendo o contrato, não igualdade SQL/C6. (`app/chamados/juizes.py:1404`; `app/chamados/models.py:3496`; `app/chamados/models.py:3467`) |
| `[DEPENDE-F1]` 5 — `matriz.md:76` | A guarda de PK nula daquele helper determina os resultados de instâncias não salvas | **CAIU para os leitores atuais.** Eles leem campos diretamente; o teste de opções usa instância não salva. (`app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/tests/test_valor_rotulo.py:7`) |
| `[DEPENDE-F1]` 6 — `matriz.md:77` | O NameError identificável do harness F1 c1 continua herdado | **CAIU por leitura.** O ambiente agora fornece `SimpleNamespace`, `texto_vazio` e o juiz real de respondida. Execução: [nao verificado]. (`app/chamados/tests/test_e4b_classe_e_memoria.py:308`; `app/chamados/tests/test_e4b_classe_e_memoria.py:310`; `app/chamados/tests/test_e4b_classe_e_memoria.py:374`) |
| `matriz.md:112` | L1 ainda é residual por falta de instante histórico conservado | **CAIU como pendência vigente**, pelos complementos fornecidos. O resultado a conservar é o do campo atual, leitor a leitor; não recuperação histórica. (`app/chamados/juizes.py:1390`; `app/chamados/models.py:3318`; `app/docs/CORTES.md:487`) |
| `matriz.md:116` | Os leitores do main continuam por campo | **CONFERE.** O resumo histórico não ativa os helpers da F1 antiga. (`app/chamados/models.py:1923`; `app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/models.py:3496`) |

**Resultado dos seis itens numerados: CAIU 5 de 6**, com a ressalva causal do item 3 indicada acima. (`app/chamados/juizes.py:1390`; `app/chamados/models.py:3496`; `app/chamados/tests/test_e4b_classe_e_memoria.py:308`)

As premissas da BUG-PAR que a F2 deve carregar agora são estas. Os nomes c4 pedidos não estavam explicitados nas linhas antigas dos consumidores; sua atribuição literal a uma linha de brief/matriz: [nao verificado]. As referências abaixo indicam as linhas dos anexos cujo consumo precisa dessa atualização.

| Origem afetada | Premissa vigente | Julgamento |
|---|---|---|
| `estados.md:7`, `estados.md:8`; M1a/M1b | Os novos nomes são **estados de aplicação**, não novos valores de `PerguntaDisputa.estado` | **CONFERE.** `estado` mantém seus retornos; a testemunha possui vocabulário próprio. (`app/chamados/models.py:3151`; `app/chamados/services/materializacao.py:705`; `app/chamados/services/materializacao.py:706`; `app/chamados/services/materializacao.py:852`) |
| Mesmos consumidores | Sem dia do fato deve retornar `bloqueada_sem_dia_do_fato`, sem chamar K6 | **CONFERE.** Não equivale a competência entregue. (`app/chamados/services/materializacao.py:784`; `app/chamados/services/materializacao.py:810`; `app/chamados/services/validacao.py:233`; `app/chamados/tests/test_bug_par_volta.py:319`) |
| Mesmos consumidores | Competência entregue retorna `bloqueada_competencia_entregue` | **CONFERE**, em ramo separado do anterior. (`app/chamados/services/materializacao.py:811`; `app/chamados/services/materializacao.py:812`; `app/chamados/services/validacao.py:234`) |
| `estados.md:7`, `estados.md:8`, `estados.md:17`; explicação/tela | BUG-PAR **L2** — F6 fechada/aprovada, não entregue — conserva o toque Validar; a recusa deve conservar `COMPETENCIA_APROVADA` | **CONFERE.** A testemunha retorna `bloqueada_competencia`; a porta pode produzir VALIDACAO+DEVOLUCAO sem batida, com o motivo de competência. (`app/chamados/services/materializacao.py:909`; `app/chamados/services/materializacao.py:935`; `app/chamados/tests/test_bug_par_volta.py:249`; `app/chamados/tests/test_bug_par_volta.py:258`; `app/chamados/tests/test_bug_par_volta.py:261`) |
| Mesmos consumidores | O motivo L2 vale também no laço e no passivo | **CONFERE por provas existentes lidas**; execução: [nao verificado]. (`app/chamados/tests/test_bug_par_volta.py:378`; `app/chamados/tests/test_bug_par_volta.py:391`; `app/chamados/services/materializacao.py:1469`) |
| Leitura da guarda pela testemunha | `k6_consulta_a_pergunta` significa pergunta não nula, metade de intervalo, fora U163 | **CONFERE.** Reduzi-lo apenas a “fora U163” **CAIU**. (`app/chamados/services/materializacao.py:792`; `app/chamados/services/materializacao.py:795`) |
| Leitura da irmã | A irmã é sempre buscada para K6 | **CAIU.** A c4c dispensa a busca quando K6 já consulta a própria pergunta; caso contrário, procura a irmã. (`app/chamados/services/validacao.py:219`; `app/chamados/services/validacao.py:224`; `app/chamados/services/materializacao.py:139`) |
| Porta visual administrativa | Só competência entregue retira Validar | **CAIU como descrição suficiente.** Também retiram: ausência do dia do fato, espera DECLARADO-X-GRAVADO e indisponibilidade; F6 conserva o toque. (`app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/_pergunta_card.html:49`; `app/templates/chamados/partials/modal_fio.html:276`) |
| Ordem da testemunha | K6 precede F6, que precede a espera declarado/gravado | **CONFERE nos ramos correspondentes.** (`app/chamados/services/materializacao.py:885`; `app/chamados/services/materializacao.py:909`; `app/chamados/services/materializacao.py:922`; `app/chamados/services/materializacao.py:933`; `app/chamados/services/materializacao.py:936`) |
| C1, item 9a, retirado pelo C2 | `_escrever_carimbo` é o único persistidor do trio | **CAIU.** Há lote direto e escritores somente da via. (`app/chamados/models.py:3391`; `app/chamados/models.py:3393`; `app/chamados/models.py:3402`; `app/chamados/services/relavra_via.py:125`) |
| C2, item 9a, corrigido pelo C3; `estados.md:18` | Descarimbar basta para voltar à lista | **CAIU.** Continuam necessárias ausência de validação, instante ou texto e tratamento da tranca; a via terminal também impede o descarimbo. (`app/chamados/models.py:1923`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926`; `app/chamados/models.py:3316`; `app/chamados/models.py:3392`) |
| C3; `estados.md:29` | Ressuscitar é o controle que zera instante e texto e não retorna à retenção por resposta | **CONFERE.** Não escreve REABERTURA. (`app/chamados/models.py:1622`; `app/chamados/models.py:1629`; `app/chamados/models.py:1630`; `app/chamados/models.py:1924`) |

O **CENSO C2 = 0** confere somente para o predicado completo “leitor da F2 dependente do instante apagado para consultar antes do descarimbo”. A projeção parte do campo atual; lista e estado não recebem esse horizonte histórico. Isso não significa ausência de leitores temporais. (`app/chamados/juizes.py:1389`; `app/chamados/models.py:1910`; `app/chamados/models.py:3154`; `app/ponto/supra_juiz.py:573`; `app/chamados/veredito_lavrado.py:203`)

**Delta `71e9576bf → 520d7b557` nos sítios citados:**

| Sítio | Resultado da conferência |
|---|---|
| `models.py`, `juizes.py`, `tarefa_do_dia.py`, `pergunta_no_app.py`, API, `supra_juiz.py`, `_card_atendimento.html` | Sem alteração nesses arquivos no diff pedido; referências de `estados.md` continuam casando. Os leitores centrais continuam nos mesmos locais. (`app/chamados/models.py:3151`; `app/chamados/juizes.py:1404`; `app/chamados/services/tarefa_do_dia.py:158`; `app/api/views.py:2553`; `app/ponto/supra_juiz.py:573`) |
| `explicador.py` | Antigas **72→95, 73→96, 74→97, 77→100, 85→108, 93→116, 139→177**. A antiga 102 mudou de conteúdo: agora há tabela/helper, recusa com `gravou=False` e espera declarada antes de cadastro/sistema. (`app/chamados/services/explicador.py:33`; `app/chamados/services/explicador.py:95`; `app/chamados/services/explicador.py:108`; `app/chamados/services/explicador.py:124`; `app/chamados/services/explicador.py:131`; `app/chamados/services/explicador.py:142`; `app/chamados/services/explicador.py:177`) |
| `fio.py` | Antiga **187 permanece; 357→359; 652→660**. Entraram carregamento de empresa e tratamento restritivo da testemunha ausente/excepcional. (`app/chamados/services/fio.py:187`; `app/chamados/services/fio.py:217`; `app/chamados/services/fio.py:359`; `app/chamados/services/fio.py:378`; `app/chamados/services/fio.py:660`) |
| `_pergunta_card.html` | **23/25/29/31 permanecem**; aplicação e bloqueios foram acrescentados no corpo da ação. (`app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:31`; `app/templates/chamados/partials/_pergunta_card.html:43`) |
| `modal_fio.html` | Antigas **436→463, 440→467**; antiga 442 mudou: agora “Não gravado” e `it.quem_resolve`. Novos estados também atingem o botão Validar. (`app/templates/chamados/partials/modal_fio.html:230`; `app/templates/chamados/partials/modal_fio.html:276`; `app/templates/chamados/partials/modal_fio.html:463`; `app/templates/chamados/partials/modal_fio.html:467`; `app/templates/chamados/partials/modal_fio.html:469`) |
| Consumidores adicionais | A ação usa a mensagem real da porta; desenlace lê `gravou=False`; passivo reconhece a metade já materializada pela irmã no mesmo ato. (`app/chamados/services/acoes_disputa.py:63`; `app/chamados/services/desenlace.py:67`; `app/chamados/services/passivo_validado.py:534`) |
| UI-VINCULO / EDITOR-CAL | O diff acrescenta apresentação do calendário padrão e confirmação de substituição da grade; não muda os corpos F2 acima. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/templates/escala/folga_calendario_editar.html:126`) |

## 2 Censo escritores e leitores

Convenções: caminhos abaixo relativos a `app/`; **R**=`respondida_em`, **T**=`resposta_colab`, **V/VP**=`validada_em/validada_por`, **M/MP**=`materializada_em/materializada_por`. **NOVO** significa ausente da enumeração dos anexos, não necessariamente criado no diff.

O levantamento alcançou atributos AST, chamadas, aliases, `getattr`, campos ORM e templates. Completude para nomes construídos dinamicamente: [nao verificado].

**Escritores físicos e portas:**

| Campos | Escritores encontrados |
|---|---|
| R/T no nascimento — **NOVO** | `_nascer` recebe os campos, verifica o par, instancia e persiste; conserva linhas legadas. (`chamados/models.py:2949`; `chamados/models.py:3124`; `chamados/models.py:3127`; `chamados/models.py:3129`) |
| R/T — resposta humana | `responder_pergunta_disputa_core`: atribuição, persistência e RESPOSTA. (`chamados/services/disputa_emissao.py:1481`; `chamados/services/disputa_emissao.py:1483`; `chamados/services/disputa_emissao.py:1525`) |
| R/T — automática | `responder_automaticamente`, com RESPOSTA. (`chamados/services/disputa_emissao.py:1674`; `chamados/services/disputa_emissao.py:1677`; `chamados/services/disputa_emissao.py:1678`) |
| R — **NOVO** | `carimbar_resposta_pela_trilha`, com RESPOSTA. (`chamados/services/disputa_emissao.py:1696`; `chamados/services/disputa_emissao.py:1698`; `chamados/services/disputa_emissao.py:1699`) |
| R/V/VP pelo carimbo | `carimbar`, condicionado a texto não vazio; persiste após a guarda C6. (`chamados/models.py:3240`; `chamados/models.py:3242`; `chamados/models.py:3245`; `chamados/models.py:3253`; `chamados/models.py:3262`) |
| V/VP | `_carimbar_validacao`; classe A da resposta humana; devolução recusada; devolução BUG-PAR **NOVO**. (`chamados/services/validacao.py:168`; `chamados/services/disputa_emissao.py:1494`; `chamados/services/validacao.py:273`; `chamados/services/validacao.py:349`) |
| M/MP/via individual | `_escrever_carimbo` atribui; persistem `carimbar`, `carimbar_identidade` e `descarimbar`. (`chamados/models.py:3209`; `chamados/models.py:3210`; `chamados/models.py:3211`; `chamados/models.py:3262`; `chamados/models.py:3299`; `chamados/models.py:3322`) |
| Trio e outros campos em lote | `descarimbar_em_lote`; chamadores que zeram R/T/V/VP: ressuscitar e reabrir para correção. (`chamados/models.py:3391`; `chamados/models.py:3393`; `chamados/models.py:1629`; `chamados/services/disputa_emissao.py:1609`; `chamados/services/disputa_emissao.py:1648`) |
| Somente via | `encerrar_por_fato_no_espelho`; `relavra_via.relavrar`. (`chamados/models.py:3402`; `chamados/models.py:3403`; `chamados/services/relavra_via.py:125`; `chamados/services/relavra_via.py:127`) |
| Apenas memória | Simulação da ressuscitação e rollback do carimbo. (`chamados/models.py:1553`; `chamados/models.py:1554`; `chamados/models.py:1556`; `chamados/models.py:3272`) |

`RegularizacaoExterna` é outro modelo: suas escritas não pertencem ao censo da pergunta. (`chamados/services/regularizacao_ext.py:199`; `chamados/models.py:4152`)

**Sequência DP e pré-requisito de conservação:**

| Item | Censo |
|---|---|
| Escritor físico | `DecisaoPergunta.registrar`; escrita em lote centralizada. (`chamados/models.py:4110`; `chamados/models.py:4120`) |
| Produtores dos atos | RESPOSTA: `disputa_emissao.py:1525,1678,1699`; VALIDACAO: `validacao.py:173`; DEVOLUCAO: `validacao.py:279,354`; ABSORCAO: `materializacao.py:1045`; RECUSA/REABERTURA: `disputa_emissao.py:1631`; SILENCIO: `models.py:2005`, `ponto/services/veredito_celula.py:195`, `management/commands/backfill_decisoes_pergunta.py:324`; RETRATACAO: `models.py:2137`. Todos os serviços/comandos sem prefixo nessa célula estão sob `chamados/`. |
| Leitores de `linhas_da_projecao_sem_linha` | `chamados/models.py:1613,3129`; `chamados/services/disputa_emissao.py:1480,1673`; `chamados/services/validacao.py:167,272,345`; `chamados/services/tarefa_do_dia.py:430`; `chamados/management/commands/backfill_decisoes_pergunta.py:67`; **NOVO** `chamados/management/commands/backfill_fio_morto_e4b.py:141`. |
| Outros leitores DP — **NOVO** | Histórico `chamados/models.py:3675`; motivo BUG-PAR `chamados/services/materializacao.py:275`; prefetch `chamados/services/fio.py:263`; tarefa `chamados/services/tarefa_do_dia.py:421`; fotografia `chamados/services/foto_e4b.py:280`; backfill `chamados/management/commands/backfill_decisoes_pergunta.py:99,315`. |
| Limites | Os tipos continuam sem MATERIALIZACAO; ressuscitar continua sem REABERTURA. (`chamados/models.py:4024`; `chamados/models.py:4025`; `chamados/models.py:1622`) |

**Consumidores das funções da fatia:**

| Função | Chamadores produtivos encontrados |
|---|---|
| `PerguntaDisputa.estado` | `api/views.py:2550`; `chamados/services/explicador.py:95`; `chamados/services/fio.py:660`; `chamados/services/vigia_fio.py:259`; `templates/chamados/partials/_card_atendimento.html:189`. |
| `respondida` | Property em `chamados/models.py:3664`; nenhum acesso produtivo direto localizado. Cobertura dinâmica: [nao verificado]. |
| `resposta_display` | `api/views.py:2562`; `_pergunta_card.html:31`; **NOVO** `api/views.py:1164`, `chamados/services/disputa_emissao.py:1560`, `templates/chamados/partials/_card_atendimento.html:106,143,183,391`. |
| `sem_veredito_entre` | `chamados/models.py:1906`; `chamados/juizes.py:2105`; `ponto/supra_juiz.py:573,574`. |
| `respostas_sem_veredito` | `chamados/models.py:1934,1967`; `chamados/reconciliador.py:1164`; `chamados/management/commands/validar_classe_a.py:54`; `chamados/services/competencia_trancada.py:126`; `chamados/services/fila_por_causa.py:226`; `colaboradores/services/desligamento.py:165`; `colaboradores/services/ficha.py:169`. |
| `pode_fechar` | `chamados/relatorios_disputa.py:24`; **NOVO** `api/foto_recebida.py:83`; `chamados/services/explicador.py:177`; **NOVO** `chamados/services/becos.py:33,56`, `materializacao.py:1067,1989`, `disputa_emissao.py:1292,2113`, `competencia_trancada.py:178,227`, `chamados/reconciliador.py:412,612,1171`, `ponto/services/entrada_sincronizada.py:59`. Os três serviços abreviados estão sob `chamados/services/`. |
| `q_vivas` | `chamados/juizes.py:1751`; `chamados/services/fio.py:256`; `pergunta_no_app.py:53`; `vigia_fio.py:711`; **NOVO** `janela_colab.py:63`, `cobranca.py:237`, `raiox.py:74`, `ponto/services/veredito_celula.py:181`. Serviços abreviados sob `chamados/services/`. |
| `_ancoradas` | `chamados/models.py:223,334,381`; **NOVO** `chamados/services/disputa_emissao.py:1821,1962`. Não chama `q_vivas` no corpo atual. (`chamados/models.py:344`) |
| `_gravar_estado` | `chamados/models.py:450,655,835,853,1489,1683,1703`. |
| `DisputaSupervisao.fechar` | `chamados/models.py:2031,2153`; `colaboradores/services/desligamento.py:167`; `chamados/services/materializacao.py:1068,2006`; `disputa_emissao.py:1298,2114`; `competencia_trancada.py:229`; `becos.py:57`; `chamados/management/commands/encerrar_perguntas_com_fato.py:88`; `chamados/reconciliador.py:413,613,1172`; `api/foto_recebida.py:84`; `ponto/services/entrada_sincronizada.py:60`. Serviços abreviados sob `chamados/services/`. |
| `remover_motivo` | `colaboradores/signals.py:77`; `escala/signals.py:174`. |
| `ressuscitar_perguntas` | `chamados/models.py:1498`; **NOVO** `chamados/views_arquivar.py:86`; `ponto/services/cartorio.py:1673`. |
| `_universo_da_ressuscitacao` | `chamados/models.py:1507,1512,1593`. |
| `projecao_temporal_da_pergunta` | `chamados/juizes.py:351,1404`; `escala/services/cadastro_realidade.py:502`. |
| `irmas_respondidas_sem_consumo` | `chamados/reconciliador.py:448`; `chamados/services/materializacao.py:1244`; alias em `chamados/services/disputa_emissao.py:2032`; alias no auditor **NOVO** `chamados/management/commands/auditar_invariantes_chamados.py:984`. |

**Leitores diretos dos campos, além dos corpos e portas já enumerados.** Cada grupo inclui cargas/fotos/filtros ORM; não significa que todo sítio julgue o estado.

| Arquivo | Campos e sítios |
|---|---|
| `api/views.py` | R `:1165,2563,2577,2620,2637`; T `:1163,2561`. |
| **NOVO** `api/views_mensageria.py` | R `:569,570`; T `:567`. |
| **NOVO** `chamados/canal.py` |

[truncado em 20000 de 59742 caracteres]

## 16/11 23:20 · sessao f2d6b5cd3a7e · PREPARA-F2-F4-F7 · censo · origem=Code

## 1 DEPENDE e premissas

**A premissa central caiu: a F1 pousada fornece o juiz `pergunta_respondida`, baseado em instante e texto da projeção; não fornece o contrato D01 descrito nos anexos.** O juiz do slot continua passando a pergunta real ao derivador. (`app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:376`)

BASE fornecida: `520d7b55720888d80b46ffbca2ca73e59da40277`. A cópia não contém metadados Git que permitam confirmar independentemente esse SHA. [nao verificado]

As linhas dos anexos abaixo foram contadas sobre o texto fornecido, começando pelo título de cada anexo; correspondência com arquivos originais externos à cópia: [nao verificado]. Nenhuma medição atual de produção nem execução de testes foi fornecida para esta leitura. [nao verificado] [nao verificado]

Para compactar as referências seguintes:

| Sigla | Arquivo |
|---|---|
| M | `app/chamados/services/materializacao.py` |
| D | `app/chamados/services/disputa_emissao.py` |
| J | `app/chamados/juizes.py` |
| V | `app/chamados/services/validacao.py` |
| MD | `app/chamados/models.py` |
| PV | `app/chamados/services/passivo_validado.py` |
| RV | `app/chamados/services/relavra_via.py` |
| FC | `app/chamados/services/fabrica_da_celula.py` |
| T | Diretório `app/chamados/tests/` |

Cada referência `M:795`, por exemplo, significa `app/chamados/services/materializacao.py:795`.

**Cada ocorrência de `[DEPENDE-F1]`:**

| Linha do anexo | Premissa | Julgamento e BASE |
|---|---|---|
| brief:38 | Absorção depende do D01 sem ramo ABSORCAO | **CAIU o contrato D01. CONFERE permanecer no carimbo**, por corte posterior. (`J:1544`, `M:1105`, `app/docs/CORTES.md:487`) |
| brief:106–107 | Assinatura `leitura(...decisoes,agora,materializada_conservada_em)` | **CAIU.** O contrato pousado é `pergunta_respondida(p) → bool`. (`J:1544`, `J:1552`) |
| brief:117 | Carga fornece decisões ao D01 do juiz | **CAIU.** A carga fornece `agora` e slots; chama `_slot_esperado(p,carga=self)`, que recebe a projeção. (`app/chamados/services/carga_pergunta_viva.py:88`, `app/chamados/services/carga_pergunta_viva.py:119`, `J:364`, `J:376`) |
| brief:137 | Ordem `(ato_em,pk)` é contrato da F1 | **CONFERE a ordem da sequência; CAIU atribuí-la ao redutor F1 pousado.** (`MD:4056`, `J:1544`) |
| brief:144 | RED de absorção não pode ser servido pelo D01 | **CAIU a dependência dessa interface.** A permanência no carimbo está decidida separadamente. (`J:1544`, `app/docs/CORTES.md:487`) |
| brief:162–164 | Absorção ainda exige pergunta de lei | **CAIU.** M9b/M10 ficam no carimbo na F4; o predicado por ato pertence à E4d. (`app/docs/CORTES.md:487`) |
| matriz:20, contrato:22–28 | `LeituraDosAtos` e redutor D01 constituem a entrada | **CAIU.** A entrada real é o par projetado; ordenação da sequência continua existindo, sem provar esse redutor. (`J:1382`, `J:1423`, `J:1544`, `MD:4056`) |
| matriz:40, M7 | Testemunha recebe decisões/agora da F1 | **CAIU.** A assinatura é `(disputa,p)`; lê projeção e chama o derivador. (`M:852`, `M:869`, `M:873`) |
| matriz:43, M9b | “Ninguém lê ABSORCAO hoje” | **CAIU.** `tarefa_do_dia` lê ABSORCAO e o estado da ausência. (`app/chamados/services/tarefa_do_dia.py:178`) |
| matriz:44, M10 | Absorção permanece pergunta de lei dependente da F1 | **CAIU a pergunta aberta; CONFERE manter o carimbo.** (`app/docs/CORTES.md:487`, `M:1105`) |
| matriz:72, E9 | REABERTURA escrita alimenta D01 | **CONFERE a escrita; CAIU o leitor D01.** (`D:1631`, `D:1648`, `J:1389`, `J:1544`) |
| matriz:91, J1 | Juiz usa `_PerguntaParaPlantio` com texto do ato | **CAIU.** Passa `p` diretamente; `data_hora` também lê `p.resposta_colab`. (`J:364`, `J:376`) |
| matriz:94–99 | Juiz D01 e materializador já leem fontes diferentes | **CAIU na BASE.** Ambos recebem a projeção. A obrigação documental de ordem/contador permanece escrita. (`J:376`, `M:895`, `M:1328`, `app/docs/CORTES.md:487`) |
| matriz:106 | Núcleo pode migrar para `leitura().resposta` da F1 pousada | **CAIU o suporte desse contrato.** L-130 define “respondida”; não determina, sozinha, substituir o texto vigente pelo último ato. (`J:1544`, `app/docs/LEIS.md:151`) |
| matriz:107–108 | Fila M23 selecionará por D01 | **CAIU o suporte D01.** A fila atual seleciona R/V/C da projeção. (`M:2063`, `M:2064`, `M:2065`, `M:2066`) |
| matriz:110 | Três chamadores entregarão decisões/agora iguais | **CAIU nesse formato e nessa abrangência.** Existem cinco sítios de chamada; o helper recebe `carga`, sem parâmetros `decisoes/agora`. (`M:599`, `M:214`, `M:222`, `M:895`, `M:1328`, `J:376`) |
| matriz:121–122 | Lei de absorção permanece aberta | **CAIU.** Respondida pelo corte posterior. (`app/docs/CORTES.md:487`) |
| matriz:131 | Resumo das marcas do contrato antigo | **CAIU como descrição das dependências atuais.** Não acrescenta um contrato ao juiz pousado. (`J:1544`, `app/docs/LEIS.md:151`) |

**Premissas sobre a BUG-PAR e premissas adicionais afetadas:**

| Linha do anexo | Julgamento |
|---|---|
| brief:35;148–151; matriz:40;48 — BUG-PAR reescreve testemunha, plantio e acrescenta leitores | **CONFERE o comportamento na BASE.** Existem derivador compartilhado, composição K6 da pergunta/irmã, par projetado e contador. Os offsets antigos precisam ser substituídos pelos atuais. (`M:175`, `M:295`, `M:876`, `M:885`, `M:907`, `M:1830`) |
| brief:148–153;184; matriz:12–14 — BUG-PAR ainda fora do main | **CAIU.** Há registro de pouso c4+c4c e seus estados estão implementados. Comparação histórica do diff de `6e2fc9133`, sem Git local: [nao verificado]. (`app/docs/BACKLOG.md:2081`, `M:705`, `M:706`) |
| brief:152–153 — passivo também recebe alterações da BUG-PAR | **CONFERE.** O passivo relê o carimbo e evita materializar novamente a metade gravada pela irmã. (`PV:531`, `PV:535`, `PV:537`) |
| brief:153;184 — refazer censo depois da BUG-PAR | **CONFERE.** Há leitores e chamadores adicionais que o censo antigo não cobria. (`M:199`, `M:214`, `M:718`, `M:770`, `M:1707`, `V:349`) |
| brief:156–157 — F1 adicionará D01 e costura pelo embrulho | **CAIU a versão da F1; CONFERE a chamada juiz → derivador.** (`J:376`, `J:1544`) |
| brief:115–118; matriz:110 — três caminhos bastam para paridade | **CAIU.** O par de intervalo acrescenta duas derivações, uma por metade. (`M:214`, `M:222`) |
| brief:166–169; matriz:123–124 — guardas de reparo ainda pedem lei | **CAIU.** A allowlist de reparo já foi cortada. (`app/docs/CORTES.md:487`) |
| brief:176–186 — números antigos demonstram o requisito atual de frota | **CAIU como medição atual.** Os valores são históricos fornecidos; estoque atual e efeito após B2: [nao verificado]. O medidor real continua disponível. (`MD:4081`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`) |

## 2 Censo escritores e leitores

`NOVO` significa **não nomeado como esse sítio no brief**, sem afirmar que nasceu recentemente. O censo abaixo combina AST de atributos/keywords/strings ORM, grep de templates e conferência das portas; chamadas dinâmicas fora dessas formas têm exaustividade semântica [nao verificado].

**Escritores, distinguindo persistência de simulação:**

| Campo | Escritores/portas na BASE |
|---|---|
| `resposta_colab` | Resposta `D:1481`; automática `D:1674`; reabertura por lote `D:1609`→`D:1648`; **NOVO** ressuscitação `MD:1629`→`MD:1630`; nascimento por `**campos` em `MD:3124`/`MD:3127`. `MD:1556` é simulação para julgamento. |
| `respondida_em` | `D:1482`, `D:1675`, reparo retroativo `D:1696`, lote `D:1609`→`D:1648`, ressuscitação `MD:1629`, `carimbar(resposta=)` em `MD:3242`, nascimento `MD:3124`. Simulação/reversão em memória: `MD:1555`, `MD:3273`. |
| `validada_em/por` | `V:168`/`V:169`; devolução `V:273`/`V:274`; **NOVO BUG-PAR** `V:349`/`V:350`; nova resposta `D:1494`/`D:1495`; lote `D:1609`; ressuscitação `MD:1629`; `carimbar('validar')` em `MD:3245`/`MD:3246`; nascimento `MD:3124`. |
| `materializada_em/por` | Porta `_escrever_carimbo`, `MD:3209`/`MD:3210`; portas de descarimbo e lote, `MD:3320`, `MD:3393`. `regularizacao_ext.py:199` pertence a outro modelo. (`app/chamados/services/regularizacao_ext.py:199`, `MD:4152`) |
| `via_resolucao` | `MD:3211`, lote `MD:3393`, encerramento específico `MD:3402`, relavra `RV:125`. |
| `recusa_motivo` | Escape do colaborador `D:1511`; motivo técnico `M:1332`; recusa `M:1665` ou extra de `carimbar_identidade` em `MD:3298`; recusa administrativa `V:827`. |
| `pendencia_admin` | Materializador escreve junto do motivo técnico. **NOVO leitor:** par rejeita irmã com pendência. (`M:1332`, `M:1333`, `M:197`) |
| `data_conferida_em/por` | Porta de conferência `V:907`/`V:908`; leitores **NOVO** `_data_em_disputa` e unidade de materialização. (`M:169`, `M:1851`) |
| `fechada_em` da Disputa | `fechar`, `reabrir_admin`, `reabrir_sistema`. (`MD:2013`, `MD:2230`, `MD:2254`) |
| Linhas de `DecisaoPergunta` | Escritor persistente único `registrar`, com `bulk_create` interno. (`MD:4110`, `MD:4120`) |

**O furo `carimbar(resposta=)` continua:** atribui R/V sem registrar RESPOSTA/VALIDACAO; absorção registra somente ABSORCAO, e resolução de chamado usa `resposta='responder'`. A quantidade atual desses casos não foi medida. (`MD:3240`, `MD:3242`, `MD:3245`, `MD:3262`, `M:1043`, `M:1045`, `D:2082`) [nao verificado quanto à frota]

**Chamadores das funções alcançadas pela fatia:**

| Função | Sítios diretos encontrados |
|---|---|
| `_tipo_pelo_marco` | `M:656` |
| `_ts_tipo_do_plantio` | `J:376`; **NOVO** `M:214`, `M:222`; `M:895`, `M:1328` |
| `_materializar_uma` | **NOVO** irmã em `M:1707`, par em `V:555`, chamada individual em `V:687`; `PV:537`, `M:2084` |
| `_materializar_resposta_hora/data_hora/escolha` | `M:1889`, `M:1891`, `M:1893` |
| `_materializar_ausencia_de_resposta` | `D:1555` |
| `materializar_perguntas_validadas_da_disputa` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:147`; **NOVO na lista do brief:** `app/chamados/services/acoes_disputa.py:141`, `app/chamados/services/fio.py:716`, `V:698` |
| `materializar_pendentes_aprovadas` | `app/chamados/signals.py:165` |
| `estado_aplicacao_pergunta` | `app/chamados/services/fio.py:376`; **NOVO:** contador `M:339`, espera `M:757`, explicador `app/chamados/services/explicador.py:124`, templatetag `app/chamados/templatetags/chamados_tags.py:136` |
| **NOVO** `par_do_intervalo` | `M:907`, `M:1695`, `V:545` |
| **NOVO** `metade_irma_do_intervalo` | `M:194`, `M:2019`, `V:206` |
| **NOVO** `competencia_do_fato` desta materialização | `M:330`, `M:808`, `V:338`; `app/chamados/management/commands/bug_par_103.py:58`, `:74`, `:116` |
| **NOVO** `k6_consulta_a_pergunta` | `M:806`, `V:224` |
| **NOVO** `_k6_que_barra` | `M:825`, `V:225` |
| **NOVO** `_competencia_que_barra` | `M:909`, `M:933`, `M:1450`, `M:1830` |
| **NOVO** `_declarado_da_metade` / `divergencia_declarado_x_gravado` | `M:728` / `M:741` |
| **NOVO** `_espera_do_aval_163` / `espera_na_escrita` | `M:922`, `M:936`, `M:1451` / `M:1877` |
| **NOVO** `espera_declarado_x_gravado` | `V:611`; `app/chamados/services/explicador.py:132`; `app/chamados/templatetags/chamados_tags.py:128` |
| **NOVO** `respostas_intervalo_recusadas_paridade` | `app/chamados/management/commands/bug_par_103.py:79` |
| **NOVO** `paridade_absolvida_bug_par_103` / `rotulo_bug_par_103` | `M:327`, `V:334` / `M:880` |
| `perguntas_absorvidas_por` / devolução | `M:1147` / `app/ponto/signals.py:262` |
| `_carimbar_recusa` | `M:1318`, `M:1430`, `M:1871`, `M:1966`, `PV:566`, `V:562` |
| `responder_pergunta_disputa_core` | `app/api/views.py:2627`; `app/chamados/services/acoes_disputa.py:373`; `app/chamados/services/declaracao_texto.py:419`, `:693`; `app/chamados/views.py:1283` |
| `responder_automaticamente` | `app/api/foto_recebida.py:48` |
| `carimbar_resposta_pela_trilha` | `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` |
| `reabrir_para_correcao` | `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`; `MD:2318`; `app/chamados/services/acoes_disputa.py:105`; `M:1164`; `V:836`; `app/chamados/views.py:1436` |
| `recobrar_questionario` | `app/chamados/views.py:1425` |
| `reabrir_credor_de_pergunta_viva` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:90`, `:135`, `:175`; `D:1969` |
| `resolver_perguntas_por_chamado_resolvido` | `app/chamados/reconciliador.py:537`, `:1649`; `app/chamados/services/becos.py:54`; `app/chamados/signals.py:117`; `app/ponto/management/commands/reconciliar_fantasmas.py:281` |
| `_resolver_perguntas_por_chamado_resolvido` / `_fechar_disputa_se_quitada` | `D:1897`, `D:1903` / `M:2086`, `PV:413` |
| `relavrar` / `via_de_hoje` | `app/chamados/management/commands/relavrar_via_i6.py:102`, `RV:157` / comando `:64`, `RV:120` |
| `q_carimbo_desfazivel` | `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:446`; `FC:150`; `RV:238`; `app/chamados/services/vigia_fio.py:769` |
| `desfazer_carimbo_sem_lastro` | `FC:169`, `FC:209` |
| `reparar` / `_plantar_o_dia` | `app/chamados/management/commands/materializar_passivo_validado.py:104`, `:113` / `PV:443` |
| `motivos_a_perguntar` / `perguntar` | `app/chamados/management/commands/disparar_perguntas_competencia.py:117`, `FC:207`, `FC:242` / `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`, `FC:219` |

Os usos homônimos de `competencia_do_fato` no backfill e em `ponto` importam outra função; não são leitores da composição BUG-PAR acima. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:53`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:111`, `M:776`)

**Censo global dos campos — referências de produção por nome.** Inclui leitura, escrita, filtros, fotos e listas de campos; a classificação dos escritores está acima. Homônimos não provam identidade de modelo. As referências exteriores à lista nominal do brief constituem ampliação **NOVO do censo**, sem afirmação sobre quando entraram.

Legenda: `R=resposta_colab`, `r=respondida_em`, `V=validada_em`, `v=validada_por[/_id]`, `C=materializada_em`, `M=recusa_motivo`, `I=via_resolucao`, `D=data_conferida_em`, `F=fechada_em`.

```text
app/api/foto_recebida.py:66,78 [F]
app/api/views.py:1018,1034,2678 [F]; :1163,2561 [R]; :1165,2563,2577,2620,2637 [r]
app/api/views_mensageria.py:567 [R]; :569,570 [r]
app/chamados/canal.py:65 [R]
app/chamados/juizes.py:292,683,711,858,1390,1502 [C]; :2466 [F];
 :683,695,857,1390 [I];
 :364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052 [R];
 :856,1389 [V]; :352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753 [r]
app/chamados/management/commands/auditar_chamados_orfaos.py:41 [F]
app/chamados/management/commands/auditar_disputas.py:71,150 [C]; :87,136,167 [F]
app/chamados/management/commands/auditar_invariantes_chamados.py:
 :470,538,588,905,955,1075,1156,1577,1604,1769 [C]; :592,806 [F];
 :202,206,433,542,955,1712 [I]; :536,1075,1112 [M]; :1106,1657 [R];
 :205,1075,1576,1604,1768,1780 [V]; :177,592,1105,1605 [r]; :1076 [v]
app/chamados/management/commands/backfill_decisoes_pergunta.py:101,325 [C];
 :101,314,330,335,336,398,504 [I]; :61,326,504 [R]; :56,60,102 [V]; :56,60 [r]; :60 [v]
app/chamados/management/commands/bug_par_103.py:66 [C,I]; :69 [M]; :68 [V,v]
app/chamados/management/commands/curar_perguntas_em_limbo.py:32,46 [R]; :46,69 [r]
app/chamados/management/commands/dedup_perguntas_fato.py:75 [R]
app/chamados/management/commands/encerrar_perguntas_com_fato.py:32 [C,V];
 :82 [F]; :33,42,49,66,75,76 [I]
app/chamados/management/commands/fechar_becos_disputa.py:32 [F]
app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147 [C]; :70,71,72 [I]
app/chamados/management/commands/reabrir_orfas_enterradas.py:67,88,91,129,142,160 [C];
 :96,131,164,165 [F]; :67,129,142,160 [I]; :68 [V]
app/chamados/management/commands/relavrar_via_i6.py:58,59,62,66,68,88 [I]
app/chamados/management/commands/validar_classe_a.py:80 [R]
app/chamados/models.py:
 :1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485 [C];
 :190,1638,1870,1888,1960,1964,2013,2017,2020,2112,2114,2219,2230,2235,2248,2254,2258,2305,2343,2778,2781 [F];
 :1535,2001,2010,2011,2136,2141,2829,2860,3165,3211,3236,3238,3296,3311,3320,3331,3365,3392,3393,3400,3402,3403,3411,3423,3440,3447,3483,3485,3652,3658 [I];
 :3298,3299 [M];
 :1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105 [R];
 :310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104 [V];
 :1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100 [r];
 :1614,1629,3237,3246,3247,3275,4105 [v]
app/chamados/reconciliador.py:367,630,1050,1145,1155,1505 [C];
 :412,600,611,612,1161,1162 [F]; :1144,1155 [I]; :365,1083 [R]; :631,1050 [V]
app/chamados/relatorios_disputa.py:46 [F]
app/chamados/services/acoes_chamado.py:346 [F]
app/chamados/services/acoes_disputa.py:257,349 [F]; :83 [M]; :355 [R]; :347 [V]; :97,132 [r]
app/chamados/services/becos.py:27,56,59 [F]; :30 [r]
app/chamados/services/competencia_trancada.py:176,221 [F]; :276,282 [I]
app/chamados/services/declaracao_texto.py:417 [R]; :227 [V]; :417,419,687 [r]
app/chamados/services/disputa_emissao.py:
 :228,776,1084,1104,1340,1637,1930,2062,2073,2090 [C];
 :41,541,992,1111,1347,1536,2026,2050,2052,2111,2113 [F];
 :337,409,428,446,1637,2072,2073,2087,2090 [I]; :1511,1513 [M];
 :1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079 [R];
 :1484,1485,1494,1496,1537,1538,1609,1636,2062 [V];
 :1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091 [r];
 :1484,1495,1496,1537,1538,1609,1636 [v]
app/chamados/services/explicador.py:118 [I]; :113 [M]; :55,102,103 [R]
app/chamados/services/fabrica_da_celula.py:171 [C]; :149 [I]
app/chamados/services/fila_por_causa.py:225,259,262 [F]
app/chamados/services/fio.py:131,646 [F];
 :1147,1214,1290,1397,1480,1921,1957,1979,2024 [I]; :564,1407 [M];
 :377,1279,1291,1613 [R]; :187,706,1613 [r]
app/chamados/services/foto_e4b.py:180 [C,V,r]; :182 [F]
app/chamados/services/historico.py:103 [C]; :109 [R]; :101 [V]; :99 [r]
app/chamados/services/materializacao.py:
 :148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127 [C];
 :169,1851 [D]; :1067,1989 [F]; :261,292,862,868,1108,1150,1652 [I];
 :1332,1333,1652,1655,1663,1665,1666 [M];
 :546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619 [R];
 :199,201,1152,2065,2082 [V]; :1151,2064 [r]; :1152 [v]
app/chamados/services/painel.py:67 [C,r]; :67,270,271,272,273 [V]; :262 [v]
app/chamados/services/passivo_validado.py:64,534,535,536 [C];
 :64,140,141,142,530,535 [I]; :66 [M,V,r,v]
app/chamados/services/pergunta_no_app.py:81 [F,r]
app/chamados/services/raiox.py:135,182 [C]; :138,147 [R,r]
app/chamados/services/regularizacao_ext.py:199,201,220 [C, outro modelo]; :60 [F]
app/chamados/services/relavra_via.py:181,244,252 [C]; :260 [F];
 :118,121,123,125,127,134,136,155,181,245,252 [I]
app/chamados/services/tarefa_do_dia.py:256,257 [I]
app/chamados/services/validacao.py:262,264,341,363,556,681,701,717 [C];
 :905,907,909 [D]; :182,288,291,293 [F]; :341,363 [I];
 :181,262,266,282,299,301,302,344,364,825,827,833,847 [M];
 :175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109 [R];
 :168,171,173,181,262,269,273,276,300,302,343,349,351,364,489,553,597,701,810,975,996 [V];
 :594,813,824,848,1167 [r]; :169,171,181,262,274,276,300,302,343,350,351,364,489 [v]
app/chamados/services/verbos.py:264,270 [V]; :335,336,342,345 [r]; :264,265,266,268 [v]
app/chamados/services/vigia_carimbo.py:98 [V]
app/chamados/services/vigia_fio.py:749 [R]; :636,746 [r]
app/chamados/signals.py:29,131,155 [F]
app/chamados/templatetags/chamados_tags.py:139,166 [R]
app/chamados/veredito_lavrado.py:203,204 [C]; :207,208 [r]
app/chamados/views.py:1257 [C]; :471,911,1329 [F]; :1280,1416 [r]
app/chamados/views_cobrar.py:161,164 [F]; :291 [R]; :169,176 [r]
app/colaboradores/painel_cell.

[truncado em 20000 de 57180 caracteres]

## 16/11 23:20 · sessao ad1e7f942ffb · PREPARA-F2-F4-F7 · censo · origem=Code

## 1 DEPENDE e premissas

**BASE examinada: cópia identificada no pedido como `520d7b55720888d80b46ffbca2ca73e59da40277`.** A cópia não contém `.git`; a confirmação independente do HEAD e de sua ancestralidade fica **[nao verificado]**. Os anexos foram fornecidos no enunciado; as referências abaixo usam sua numeração textual, contando o cabeçalho como linha 1. Não houve acesso a banco nem execução de testes **[nao verificado quanto aos resultados em execução]**.

**Achado principal:** o contrato F1 antigo do brief não corresponde ao contrato atual. A BASE declara `pergunta_respondida(p)` como juiz de instante **e** texto; declara também que os leitores B/C/D serão migrados na B2. `estado`, `respondida` e `resposta_display` ainda leem a projeção. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`; `app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`.)

Nesta tabela, **CAIU** significa que a premissa não pode orientar a F7 contra esta BASE; não significa que o texto descrevesse incorretamente sua cópia histórica.

| Origem | Marca ou premissa | Julgamento na BASE |
|---|---|---|
| brief:85–87,171 | `[DEPENDE-F1]`: medir U1/U2 somente depois de pousar a antiga `leitura` | **CAIU.** Há funções reais para medir projeção sem linha e identidade; a comparação entre os antigos folds puro/conservado permanece **[nao verificado]**. (`app/chamados/models.py:3197`; `app/chamados/models.py:4081`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`.) |
| brief:89 | `[DEPENDE-F1]`: escolher `leitura` pura ou `_leitura_conservada` | **CAIU como contrato disponível.** O contrato atual nomeia `pergunta_respondida`, sobre o par projetado. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`.) |
| brief:89–94 | `[DEPENDE-F1]`: accessor público de `LeituraDosAtos`; F2 já forneceria propriedades pelo ato | **CAIU.** As propriedades atuais continuam na projeção; a classificação B/C/D está destinada à B2. (`app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/juizes.py:1550`.) |
| brief:100 | `[DEPENDE-F1]`: juiz da matriz = `leitura(p)` | **CAIU.** Vitalidade lê `projecao_temporal_da_pergunta`; resposta usa `pergunta_respondida`. (`app/chamados/juizes.py:1382`; `app/chamados/juizes.py:1404`; `app/chamados/juizes.py:1423`.) |
| brief:116–119 | `[DEPENDE-F1]`: R1b deve escolher entre duas portas de fold | **CAIU como alternativa executável confirmada.** A conservação por correspondência existe, mas isso não prova a disponibilidade daqueles dois leitores. (`app/chamados/models.py:4081`; `app/chamados/models.py:4094`; `app/chamados/models.py:4102`.) |
| matriz:13–17,30 | `[DEPENDE-F1]`: assinatura e resultado `LeituraDosAtos` | **CAIU.** O contrato pousado descrito pelo código é o juiz L-130. (`app/chamados/juizes.py:1544`; `app/docs/LEIS.md:151`.) |
| matriz:38 — C1 | `[DEPENDE-F1]`: vitalidade passaria a ler o ato | **CAIU.** V ainda encerra no primeiro degrau. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1405`.) |
| matriz:44 — C2 | `[DEPENDE-F1]`: `resposta_enviada` viria de instantes/texto do fold | **CAIU.** Seu escritor continua negando `espera_o_texto_do_colaborador`, que consulta I e o predicado de resposta sem texto. (`app/chamados/services/fio.py:187`; `app/chamados/services/fio.py:359`.) |
| matriz:50,55,58,64,67,75,83 — C3/C4/C5/C7/C8/C10/C12 | Cada `[DEPENDE-F1]`: texto, data e prefill pelo ato | **CAIU como dependência satisfeita.** As leituras cruas continuam nos mesmos pontos. (`app/templates/chamados/partials/_card_atendimento.html:106`; `app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:143`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:154`; `app/templates/chamados/partials/_card_atendimento.html:182`; `app/templates/chamados/partials/_card_atendimento.html:391`.) |
| matriz:79 — C11 | `[DEPENDE-F1]`: V pelo ato e estado herdado da F2 | **CAIU como implementação presente.** O estado continua dando precedência a V/materialização. (`app/chamados/models.py:3151`; `app/chamados/models.py:3156`.) |
| matriz:88 — P0 | `[DEPENDE-F1]`: F1 faria U2 entrar na lista | **CAIU.** O juiz atual ainda mata por V; o detalhe filtra por esse juiz. (`app/chamados/juizes.py:1405`; `app/chamados/services/acoes_chamado.py:358`.) |
| matriz:93–97,100 — P1/P2/P3/P4/P5/P6 | Cada `[DEPENDE-F1]`: fase, texto, instante e gate pelo fold | **CAIU como migração presente.** I/V continuam literais no template. (`app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/_pergunta_card.html:32`; `app/templates/chamados/partials/_pergunta_card.html:42`.) |
| matriz:103 — P7 | `[DEPENDE-F1]`: filtro leria texto do ato | **CAIU.** O filtro lê R por `getattr` e chama a classificação atual. (`app/chamados/templatetags/chamados_tags.py:166`; `app/chamados/templatetags/chamados_tags.py:171`.) |
| matriz:107 — M0 | `[DEPENDE-F1]`: universo pelo ato; prefetch disponível | **CAIU quanto ao ato; CONFERE quanto ao prefetch.** (`app/chamados/juizes.py:1405`; `app/chamados/services/fio.py:263`.) |
| matriz:109,113,114 — M1/M3/M4 | Cada `[DEPENDE-F1]`: I/R/V pelo fold | **CAIU como migração presente.** (`app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/modal_fio.html:219`; `app/templates/chamados/partials/modal_fio.html:220`; `app/templates/chamados/partials/modal_fio.html:228`.) |
| matriz:115,117 — M5/M6 | Cada `[DEPENDE-F1]`: aplicação/frase derivadas do contrato antigo | **CAIU como contrato.** Aplicação vem da testemunha real; frase ainda depende da projeção. (`app/chamados/services/fio.py:376`; `app/chamados/templatetags/chamados_tags.py:166`; `app/chamados/templatetags/chamados_tags.py:171`.) |
| matriz:123 — L1 | `[DEPENDE-F1]`: preview pelo texto do ato | **CAIU como migração presente.** A célula continua lendo R. (`app/templates/chamados/partials/_lote_preview.html:24`.) |
| matriz:130,133,135 | Marcas do contrato de saída: accessor único antigo, universo pelo ato, R1b puro/conservado | **CAIU quanto à API pressuposta.** Manter um juiz único continua decidido; a API antiga não pode ser presumida. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`; `app/chamados/services/acoes_chamado.py:358`.) |
| matriz:147 | Contagem dos 25 degraus `[DEPENDE-F1]` | **CAIU como conjunto liberado pelo antigo fold:** os 25 destinos precisam ser relidos pelo contrato atual. A B2 está explicitamente encarregada dos leitores B/C/D. (`app/chamados/juizes.py:1550`; `app/docs/LEIS.md:151`.) |

As premissas sobre a BUG-PAR ficam assim:

| Origem | Premissa | Julgamento na BASE |
|---|---|---|
| brief:15; matriz:8–10 | Os quatro templates continuam iguais ao censo; BUG-PAR está fora da base | **CAIU como premissa atual.** Card do colaborador e lote conservam os pontos antigos; pergunta/modal receberam novos gates. A ancestralidade dos hashes antigos é **[nao verificado]**. (`app/templates/chamados/partials/_card_atendimento.html:106`; `app/templates/chamados/partials/_lote_preview.html:24`; `app/templates/chamados/partials/_pergunta_card.html:43`; `app/templates/chamados/partials/modal_fio.html:230`.) |
| brief:77–78,149; matriz:103 | `chamados_tags.py` é compartilhado com BUG-PAR | **CONFERE como dependência atual.** Contém os adaptadores da espera e da aplicação. (`app/chamados/templatetags/chamados_tags.py:121`; `app/chamados/templatetags/chamados_tags.py:132`.) |
| brief:149 | Card ganharia apenas `{% with esp=perg\|espera_declarado_x_gravado %}` envolvendo o gate | **CAIU.** O código atual usa `ap=perg|aplicacao_da_resposta`, além da leitura da frase. (`app/templates/chamados/partials/_pergunta_card.html:43`.) |
| brief:149; matriz:100 | Colisão no gate exterior de Validar do card | **CONFERE.** V continua sendo o gate exterior; os novos impedimentos estão dentro dele. (`app/templates/chamados/partials/_pergunta_card.html:42`; `app/templates/chamados/partials/_pergunta_card.html:44`.) |
| brief:149,156–158; matriz:117 | Colisão no ramo “Validada” e nas ações do modal | **CONFERE**, com escopo maior: há ramos de aplicação sob V e sem V, além do gate das ações. (`app/templates/chamados/partials/modal_fio.html:228`; `app/templates/chamados/partials/modal_fio.html:230`; `app/templates/chamados/partials/modal_fio.html:252`; `app/templates/chamados/partials/modal_fio.html:274`.) |
| brief:149 | BUG-PAR altera contexto do fio | **CONFERE.** A testemunha e o fallback indisponível são anexados à pergunta. (`app/chamados/services/fio.py:376`; `app/chamados/services/fio.py:377`; `app/chamados/services/fio.py:384`.) |
| brief:95–96; matriz:87–91,134 | Detalhe/fragmento sem prefetch; modal já tem | **CONFERE.** O custo adicional de um accessor futuro é **[nao verificado]**. (`app/chamados/services/acoes_chamado.py:352`; `app/chamados/views.py:399`; `app/chamados/services/fio.py:263`.) |
| brief:80–87 | Todos os escritores mantêm concordância; divergência só no legado | **CAIU como conclusão universal.** `carimbar` também preenche I/V e salva, sem registrar decisão nesse corpo. Ocorrência divergente em produção: **[nao verificado]**. (`app/chamados/models.py:3242`; `app/chamados/models.py:3245`; `app/chamados/models.py:3262`.) |
| brief:175–176; matriz:136 | Template e Python precisam pousar juntos | **CONFERE como regra escrita de publicação.** Execução de deploy nesta tarefa: nenhuma **[nao verificado por registro externo]**. (`CLAUDE.md:90`.) | [nao verificado]

## 2 Censo escritores e leitores

R = `resposta_colab`; I = `respondida_em`; V = `validada_em`; autor = `validada_por`. A separação entre projeção e sequência está declarada no model. (`app/chamados/models.py:4007`; `app/chamados/models.py:4011`.)

**NOVO** identifica sítio ou consumidor ausente do censo do brief, inclusive dentro de arquivo que o brief já mencionava.

| Escritor | Escrita e conservação | Delta |
|---|---|---|
| `responder_pergunta_disputa_core` | Escreve R/I, desfaz V/autor e registra RESPOSTA. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`.) | **NOVO** |
| `reabrir_para_correcao` | Limpa os quatro campos; conserva snapshot/decisão e usa a porta de lote. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648`.) | Já listado |
| `responder_automaticamente` | Conserva projeção anterior, escreve R/I e RESPOSTA. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1678`.) | **NOVO** |
| `carimbar_resposta_pela_trilha` | Preenche I e registra RESPOSTA. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1699`.) | **NOVO** |
| `_carimbar_validacao` | Escreve V/autor e VALIDACAO no mesmo atomic. (`app/chamados/services/validacao.py:167`; `app/chamados/services/validacao.py:168`; `app/chamados/services/validacao.py:170`; `app/chamados/services/validacao.py:173`.) | **NOVO** |
| `devolver_validacao_recusada` | Limpa V/autor e registra DEVOLUCAO. (`app/chamados/services/validacao.py:272`; `app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:279`.) | Já listado; linhas mudaram |
| `devolver_paridade_bug_par_103` | Consulta K6 antes da escrita; limpa V/autor e registra DEVOLUCAO. (`app/chamados/services/validacao.py:338`; `app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:354`.) | **NOVO** |
| `ressuscitar_perguntas` | Conserva linhas anteriores e zera o par inteiro pela porta de lote, sem REABERTURA. (`app/chamados/models.py:1613`; `app/chamados/models.py:1615`; `app/chamados/models.py:1629`.) | **NOVO** |
| `PerguntaDisputa.carimbar` | Pode preencher I e V/autor antes do `save`; seu rollback restaura memória. (`app/chamados/models.py:3242`; `app/chamados/models.py:3245`; `app/chamados/models.py:3262`; `app/chamados/models.py:3273`.) | **NOVO** |
| `descarimbar_em_lote` | Valida a composição do par e escreve os campos recebidos em `outros`. (`app/chamados/models.py:3370`; `app/chamados/models.py:3383`; `app/chamados/models.py:3391`.) | **NOVO**, infraestrutura |
| Nascimento da pergunta | Salva os campos de entrada e conserva projeção sem linha. (`app/chamados/models.py:3124`; `app/chamados/models.py:3127`; `app/chamados/models.py:3129`.) | **NOVO**, infraestrutura |
| `DecisaoPergunta.registrar` | Porta material da sequência, por `bulk_create`. (`app/chamados/models.py:4110`; `app/chamados/models.py:4120`.) | Porta a acompanhar |

Os caminhos de entrada também precisam acompanhar esses escritores:

| Porta | Chamadores de produção encontrados |
|---|---|
| Resposta manual | `app/api/views.py:2627`; `app/chamados/views.py:1283`; **NOVO** `app/chamados/services/declaracao_texto.py:419`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/services/acoes_disputa.py:373` |
| Resposta automática/trilha | **NOVO** `app/api/foto_recebida.py:48`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` |
| Reabertura/recusa | `app/chamados/models.py:2318`; `app/chamados/views.py:1436`; `app/chamados/services/acoes_disputa.py:105`; `app/chamados/services/materializacao.py:1164`; `app/chamados/services/validacao.py:836`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69` |
| Carimbo da pergunta | **NOVO** `app/chamados/reconciliador.py:406`; `app/chamados/reconciliador.py:596`; `app/chamados/models.py:3414`; `app/ponto/services/cartorio.py:1314`; `app/ponto/services/cartorio.py:1873`; `app/chamados/services/materializacao.py:1043`; `app/chamados/services/materializacao.py:1944`; `app/chamados/services/materializacao.py:2139`; `app/chamados/services/disputa_emissao.py:2008`; `app/chamados/services/disputa_emissao.py:2082` |
| Devolução BUG-PAR | **NOVO** `app/chamados/management/commands/bug_par_103.py:106` |

Os homônimos `carimbar` das APIs de tokens foram excluídos: importam `api.credencial`, não a porta de pergunta. (`app/api/views.py:23`; `app/api/views_core.py:22`.)

O grep/AST ampliado encontrou os seguintes sítios de leitura, seleção ou transporte de R/I/V/autor. Strings de auditoria e nomes de campos são candidatos estáticos, não necessariamente julgamento; resolução de acessos inteiramente dinâmicos permanece **[nao verificado]**.

| Arquivo | Sítios BASE |
|---|---|
| Model/juízes | `app/chamados/models.py:310`, `:344`, `:1533`, `:1923`, `:2007`, `:2139`, `:2949`, `:3151`, `:3237`, `:3331`, `:3370`, `:3496`, `:3568`, `:3584`, `:3665`, `:3688`, `:4091`; `app/chamados/juizes.py:352`, `:364`, `:856`, `:1360`, `:1389`, `:1423`, `:1534`, `:1541`, `:1552`, `:1572`, `:1580`, `:1744`, `:1753`, `:1779`, `:1926`, `:2030`, `:2035`, `:2052` |
| Views/contexto conhecidos | `app/chamados/views.py:1280`, `:1416`; `app/chamados/views_cobrar.py:169`, `:176`, `:291`; `app/chamados/services/fio.py:187`, `:377`, `:706`, `:1279`, `:1291`, `:1613`; `app/chamados/templatetags/chamados_tags.py:139`, `:166` |
| Escritores também leitores — **NOVOS sítios** | `app/chamados/services/validacao.py:173`, `:181`, `:262`, `:269`, `:343`, `:380`, `:489`, `:553`, `:594`, `:597`, `:701`, `:810`, `:823`, `:843`, `:975`, `:996`, `:1094`, `:1167`; `app/chamados/services/disputa_emissao.py:1484`, `:1537`, `:1635`, `:1663`, `:1692`, `:1700`, `:1735`, `:1843`, `:1861`, `:2062`, `:2074`, `:2091` |
| Materialização — **NOVOS sítios** | `app/chamados/services/materializacao.py:199`, `:201`, `:546`, `:574`, `:625`, `:671`, `:678`, `:718`, `:733`, `:770`, `:869`, `:950`, `:1047`, `:1109`, `:1150`, `:1311`, `:1352`, `:1534`, `:1558`, `:1567`, `:1619`, `:2064`, `:2082` |
| **NOVO** ações/declarador | `app/chamados/services/acoes_disputa.py:97`, `:132`, `:347`, `:355`; `app/chamados/services/declaracao_texto.py:227`, `:417`, `:687`; `app/chamados/services/becos.py:30` |
| **NOVO** explicação/histórico/foto | `app/chamados/services/explicador.py:55`, `:102`; `app/chamados/services/historico.py:99`, `:101`, `:109`; `app/chamados/services/foto_e4b.py:180`; `app/chamados/services/raiox.py:138`, `:147` |
| **NOVO** filas/app | `app/chamados/services/painel.py:67`, `:262`, `:270`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/verbos.py:264`, `:335`; `app/chamados/canal.py:65` |
| **NOVO** reparo/vigias | `app/chamados/services/passivo_validado.py:66`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/vigia_fio.py:636`, `:746`, `:749`; `app/chamados/reconciliador.py:365`, `:631`, `:1050`, `:1083`; `app/chamados/veredito_lavrado.py:207` |
| **NOVO** APIs | `app/api/views.py:1163`, `:2561`, `:2577`, `:2620`, `:2637`; `app/api/views_mensageria.py:567`, `:569` |
| **NOVO** colaboradores/folha | `app/colaboradores/services/calendario.py:504`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142` |
| **NOVO** ponto | `app/ponto/services/cartorio.py:1641`; `app/ponto/services/dia_do_colab.py:70`; `app/ponto/services/flip_auto.py:130`; `app/ponto/services/procedencia_batida.py:71`; `app/ponto/services/veredito_celula.py:197`; `app/ponto/management/commands/reconciliar_grade.py:461` |
| **NOVO** auditoria | `app/chamados/management/commands/auditar_invariantes_chamados.py:177`, `:205`, `:592`, `:1075`, `:1105`, `:1576`, `:1604`, `:1657`, `:1768`, `:1780` |
| **NOVO** comandos de acervo | `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `:60`, `:102`, `:326`, `:504`; `app/chamados/management/commands/bug_par_103.py:68`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:32`, `:46`, `:69`; `app/chamados/management/commands/dedup_perguntas_fato.py:75`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:68`; `app/chamados/management/commands/validar_classe_a.py:80` |
| **NOVO** sondas existentes | `bin/censo_folha.py:28`; `bin/censo_folha.py:29`; `bin/sombra_regras_pessoais.py:85` |

Para as funções e propriedades intermediárias, o grafo atual é:

| Campo/função | Escritor ou autoridade → leitores |
|---|---|
| `viva` / `estado` / `respondida` / `resposta_display` | `viva` delega; os outros três ainda consultam projeção. (`app/chamados/models.py:3512`; `app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`.) Os leitores de template estão integralmente na seção TEMPLATES. |
| `resposta_enviada` | Escritor `fio.py:359`; leitores card:100,143,161,390. A função subjacente também é lida pela cobrança. (`app/chamados/services/fio.py:359`; `app/chamados/services/fio.py:174`; `app/chamados/services/cobranca.py:241`; `app/templates/chamados/partials/_card_atendimento.html:100`; `:143`; `:161`; `:390`.) |
| `estado_aplicacao` | Escritor de apresentação `fio.py:376`, fallbacks:377/384; leitores modal:230–274. **NOVO** adaptador equivalente no filtro do card. (`app/chamados/services/fio.py:376`; `:377`; `:384`; `app/chamados/templatetags/chamados_tags.py:132`; `app/templates/chamados/partials/modal_fio.html:230`; `:274`.) |
| `estado_aplicacao_pergunta` | Leitores diretos: contador, espera, explicador, filtro e fio. (`app/chamados/services/materializacao.py:339`; `:757`; `app/chamados/services/explicador.py:124`; `app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/services/fio.py:376`.) |
| `espera_declarado_x_gravado` | Leitores: filtro, validação e explicador. (`app/chamados/templatetags/chamados_tags.py:127`; `app/chamados/services/validacao.py:611`; `app/chamados/services/explicador.py:132`.) |
| `l

[truncado em 20000 de 58269 caracteres]

## 16/11 23:13 · sessao a9ed8c02f370 · - · - · origem=Ronald (tmux)

Numeração dos anexos contada por transcrição integral em strings em memória; nenhum arquivo escrito. `brief.md` tem 186 linhas e `matriz.md` 136 linhas nesta transcrição; equivalência com arquivos locais dos anexos = [nao verificado].

| Origem | Julgamento | Evidência na BASE |
|---|---|---|
| brief:38 — F1 sem ramo absorção | **CAIU como dependência do contrato D01**: a F1 real é `pergunta_respondida`, sem leitura de atos. **CONFERE** permanecer no carimbo, por corte posterior. | `app/chamados/juizes.py:1544`; `app/chamados/services/materializacao.py:1105`; `app/docs/CORTES.md:487` |
| brief:106-107 — assinatura `leitura(...decisoes,agora,materializada_conservada_em)` | **CAIU**: o contrato positivo pousado é `pergunta_respondida(p)` → booleano I∧T; não é assinatura substituída por outra versão de D01. | `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552` |
| brief:117 — carga fornece decisões ao juiz e texto D01 | **CAIU**: carga mantém `agora` e `slots`, mas chama `_slot_esperado(p,carga=self)`; juiz lê `p.resposta_colab` ou passa `p` real ao derivador. | `app/chamados/services/carga_pergunta_viva.py:88`; `app/chamados/services/carga_pergunta_viva.py:119`; `app/chamados/juizes.py:364`; `app/chamados/juizes.py:376` |
| brief:137 — ordem `(ato_em,pk)` “do F1” | **CONFERE na sequência**, **CAIU atribuição ao D01/F1 pousado**. | `app/chamados/models.py:4056`; `app/chamados/juizes.py:1544` |
| brief:144 — teste de absorção não servido pelo D01 | **CAIU contrato D01**; permanência do leitor por carimbo **CONFERE pelo corte**, sem necessidade de perguntar de novo. | `app/chamados/services/materializacao.py:1105`; `app/docs/CORTES.md:487` |
| brief:162-164 — absorção ainda pergunta de lei e sem ato que desfaz | **CAIU pergunta aberta**: corte manda M9b/M10 permanecer no carimbo. A devolução efetivamente registra REABERTURA antes de limpar a projeção. | `app/docs/CORTES.md:487`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648` |
| matriz:20,22-28 — contrato de entrada D01 completo | **CAIU**: juiz real I∧T e projeção temporal, sem `LeituraDosAtos`. Ordem da sequência **CONFERE**, mas não prova o redutor descrito. | `app/chamados/juizes.py:1382`; `app/chamados/juizes.py:1544`; `app/chamados/models.py:4056` |
| matriz:40 M7 — decisões/agora da F1 no estado de aplicação | **CAIU**: testemunha recebe `(disputa,p)` e consulta projeção/derivador. | `app/chamados/services/materializacao.py:852`; `app/chamados/services/materializacao.py:869`; `app/chamados/services/materializacao.py:873` |
| matriz:43 M9b — “ninguém lê ABSORCAO hoje” | **CAIU**: tarefa lê ABSORCAO e status da ausência, retornando quatro estados específicos. | `app/chamados/services/tarefa_do_dia.py:178` |
| matriz:44 M10 — absorção como pergunta de lei dependente F1 | **CAIU pergunta aberta**; **CONFERE leitor mantido no carimbo**, por corte. | `app/docs/CORTES.md:487`; `app/chamados/services/materializacao.py:1105` |
| matriz:72 E9 — REABERTURA alimenta D01 | **CONFERE escrita da REABERTURA**, **CAIU leitor D01**. | `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1544` |
| matriz:91 J1 — embrulho `_PerguntaParaPlantio` na F1 pousada | **CAIU**: juiz passa `p` diretamente; ramo data_hora lê projeção. | `app/chamados/juizes.py:364`; `app/chamados/juizes.py:376` |
| matriz:94-99 — divergência instalada juiz D01 × materializador projeção | **CAIU na BASE**: juiz e materializador continuam recebendo projeção; a obrigação documental de ordem/contador permanece escrita. | `app/chamados/juizes.py:376`; `app/chamados/services/materializacao.py:895`; `app/docs/CORTES.md:487` |
| matriz:106 — leitores do núcleo passam a `leitura().resposta` | **CAIU suporte da F1 pousada**. O contrato vigente define um juiz de “respondida”, sem decidir por si só que texto do último ato substitui a projeção. | `app/chamados/juizes.py:1544`; `app/docs/LEIS.md:151` |
| matriz:107-108 — fila M23 por D01 | **CAIU suporte D01**; fila atual documenta R/V/C da projeção. | `app/chamados/services/materializacao.py:2032`; `app/chamados/services/materializacao.py:2035` |
| matriz:110 — três chamadores com decisões/agora D01 | **CAIU nesse formato**: juiz continua chamando o derivador com pergunta real e carga opcional. | `app/chamados/juizes.py:376` |
| matriz:121-122 — lei de absorção aberta | **CAIU**: respondida no adendo matriz:133-136 e no corte da BASE. | `app/docs/CORTES.md:487` |
| matriz:131 — contagem das marcas | É resumo editorial, não dependência adicional; contagem de marcas atuais do contrato = [nao verificado]. | [nao verificado] |

Premissas BUG-PAR:

- **brief:35;148-153 e matriz:40;48 — CONFERE que a BUG-PAR reescreveu a testemunha e introduziu leitores**: `_estado_pelo_derivador`, K6 da pergunta/irmã, par projetado e classificação do contador existem na BASE. Os offsets e identidade do diff `6e2fc9133` **CAÍRAM como descrição da versão pousada**; o RELATO registra c4+c4c e os estados L1/L5 estão implementados. (`app/chamados/services/materializacao.py:876`; `app/chamados/services/materializacao.py:885`; `app/chamados/services/materializacao.py:907`; `app/chamados/services/materializacao.py:339`; `app/docs/RELATO.md:5`)
- **brief:152-153;184 e matriz:12-14 — CONFERE a necessidade de recensear depois da BUG-PAR**; premissa “BUG-PAR fora do main/aguardando” **CAIU**, pois a BASE registra pouso. Comparação histórica dos commits sem `.git` = [nao verificado]. (`app/docs/RELATO.md:5`)
- **brief:156-157 — CAIU F1 como adição de `atos_pergunta`/embrulho**, embora **CONFIRA** o vínculo de chamada juiz → `_ts_tipo_do_plantio`. (`app/chamados/juizes.py:376`; `app/chamados/juizes.py:1544`)

Leis:

- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 3 | contestavel**: absorção M9b/M10 permanece no carimbo na F4; predicado por ato pertence à E4d. (`app/docs/CORTES.md:487`)
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 4 | contestavel**: guardas E11/E14/E17 permanecem na projeção por allowlist exata; saída futura na E4d com lápide. (`app/docs/CORTES.md:487`)
- **DECIDIDO PELA LEI L-130 | contestavel**: respondida exige instante e texto após trim; os dois meios pares são ilegais; reabrir/ressuscitar limpa o par. Isso não fornece sozinho a antiga semântica D01. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`)
- As perguntas sobre absorção e limbo do brief:162-169 / matriz:121-124 não sobram. A condição de pouso/J1 continua documentada, mas o estado atual da costura tem de ser reconferido depois da B2. (`app/docs/CORTES.md:487`; `app/chamados/juizes.py:376`)
- A questão “ressuscitar devolve ao colaborador” está registrada como não decidida, porém fora deste pedido de leitura da F4; não ampliar a lista de lei com ela. (`app/docs/LEIS.md:151`)

PRE-BACKFILL:

- Mora em `DecisaoPergunta.linhas_da_projecao_sem_linha`, **`app/chamados/models.py:4081`**. É leitura que devolve dicionários; exige correspondência **pergunta + ato + instante**, sem esconder resposta anterior atrás de qualquer RESPOSTA existente. Produz RESPOSTA para `respondida_em` e VALIDACAO para `validada_em`, preservando autor da validação. (`app/chamados/models.py:4091`; `app/chamados/models.py:4094`; `app/chamados/models.py:4098`; `app/chamados/models.py:4102`)
- O medidor real `linhas_a_criar()` usa esse helper, mas mede horizonte/sem dia/conversa, distribuindo competência pela função real; portanto precisa distinguir esse recorte do total integral da frota. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:79`)
- **O command atual não aplica as linhas R/V do item 1**: o texto declara outro ato, com aval próprio; `--aplicar` cobre itens 2 e 4. Não dizer que executar `backfill_decisoes_pergunta --aplicar` satisfaz esse pré-requisito. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`)
- Exigência verificável antes de trocar um leitor por atos: zerar as faltas correspondentes no universo efetivamente migrado, incluindo re-respostas posteriores à reabertura; cobertura atual e necessidade quantitativa = [nao verificado]. A função conserva projeção vigente e não reconstrói texto já sobrescrito. (`app/chamados/models.py:4084`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:12`)
- Números do brief são históricos anexados; números atuais = [nao verificado]. As duas sondas necessárias são helper direto em lotes sobre todas as perguntas e `linhas_a_criar()` para o recorte oficial; nenhuma precisa executar o `Command.handle`, que no DRY faz simulação de apply com escrita/rollback. (`app/chamados/models.py:4081`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:578`)

## 16/11 23:12 · sessao ecdfda28a78b · - · - · origem=Ronald (tmux)

Censo feito por `rg` em todo `app/` e AST de todos os `.py`, incluindo filtros ORM, strings de campos, `getattr`, aliases/imports e templates; sem execução de Django, suite ou banco. Nenhuma ocorrência dos campos/funções F2 apareceu em arquivo `.js`; cobertura de nomes construídos dinamicamente não demonstrável: [nao verificado]. [nao verificado]

Convenção abaixo: caminhos relativos a `app/`; **NOVO** significa ausente da enumeração do brief/matriz fornecidos, não necessariamente introduzido entre os hashes. Linhas são desta cópia.

**Escritores da pergunta, com persistência e escritores apenas em memória**

| Campo | Escritor real e forma |
|---|---|
| R/T no nascimento | `chamados/models.py:2949` valida `campos.get('respondida_em')`/`resposta_colab`; `_nascer` recebe `**campos`, instancia e salva em `chamados/models.py:3124`, `chamados/models.py:3127`; conserva atos em `chamados/models.py:3129`. **NOVO no censo**. |
| R/T resposta humana | `responder_pergunta_disputa_core`: atribuições `chamados/services/disputa_emissao.py:1481`, `:1482`; `save` em `:1483`; DP RESPOSTA em `:1525`. |
| R/T automática | `responder_automaticamente`: `chamados/services/disputa_emissao.py:1674`, `:1675`; `save` `:1677`; DP RESPOSTA `:1678`; retorna `False` sem sobrescrever texto em `:1663`, `True` em `:1680`. |
| R só | `carimbar_resposta_pela_trilha`: `chamados/services/disputa_emissao.py:1696`, save `:1698`, DP RESPOSTA `:1699`; guarda idempotência `:1692`. **NOVO**. |
| R/V/Vpor pelo carimbo | `PerguntaDisputa.carimbar`: `chamados/models.py:3242`, `:3245`, `:3246`; somente com resposta não vazia `:3240`; salva `:3262`; recusa viva em `:3253`. |
| V/Vpor validação | `_carimbar_validacao`: `chamados/services/validacao.py:168`, `:169`; save `:171`; DP VALIDACAO `:173`. |
| V/Vpor resposta automática classe A | `responder_pergunta_disputa_core`: `chamados/services/disputa_emissao.py:1494`, `:1495`; save `:1496`. |
| V/Vpor devolução | `devolver_validacao_recusada`: `chamados/services/validacao.py:273`, `:274`; save `:276`; DP DEVOLUCAO `:279`. **NOVO**. |
| V/Vpor devolução BUG-PAR | `devolver_paridade_bug_par_103`: descarimba em `chamados/services/validacao.py:346`; limpa V/Vpor em `:349`, `:350`; save `:351`; DP DEVOLUCAO com motivo BUG-PAR em `:354`. **NOVO**. |
| M/Mpor/via individual | `_escrever_carimbo`: atribui trio em `chamados/models.py:3209`, `:3210`, `:3211`; persistidores são `carimbar` `:3262`, `carimbar_identidade` `:3299`, `descarimbar` `:3322`. |
| M/Mpor/via em lote | `descarimbar_em_lote`: `update` direto `chamados/models.py:3391`, `:3393`; exclui terminais `:3392`; devolve inteiro da operação ORM. |
| Lote zera também R/T/V/Vpor | `ressuscitar_perguntas`: chama lote em `chamados/models.py:1629`, `:1630`; DP legado anterior em `:1613`; não escreve REABERTURA, conforme `:1622` e execução `:1629`. `reabrir_para_correcao`: campos `chamados/services/disputa_emissao.py:1609`; DP RECUSA/REABERTURA/foto em `:1631`; chama lote em `:1648`. |
| Só via | `encerrar_por_fato_no_espelho`: `chamados/models.py:3402`, save `:3403`; `relavra_via.relavrar`: `chamados/services/relavra_via.py:125`, save `:127`. |
| Só memória | Simulação da ressuscitação `chamados/models.py:1554`, `:1555`, `:1556`; rollback `_desfazer_em_memoria` `:3272`, `:3273`, `:3274`, `:3275`. |

`chamados/services/regularizacao_ext.py:199`, `:200`, `:201` escreve **RegularizacaoExterna**, não PerguntaDisputa; excluir esse falso positivo. A outra declaração `materializada_em/materializada_por` de `chamados/models.py:4152`, `:4153` também é de RegularizacaoExterna.

**DP: escritores, leitores e pré-requisito**

- Escritor físico: `DecisaoPergunta.registrar`, `chamados/models.py:4110`, `bulk_create` em `:4120`; retorno de `linhas_da_projecao_sem_linha` é lista de dicts em `:4095`, `:4106`; atos disponíveis em `:4024`, `:4025`; proibições de atualização/apagamento em `:3997`, `:4000`, `:4003`, `:4072`, `:4076`.
- RESPOSTA: `chamados/services/disputa_emissao.py:1525`, `:1678`, `:1699`; VALIDACAO `chamados/services/validacao.py:173`; DEVOLUCAO `:279`, `:354`; ABSORCAO `chamados/services/materializacao.py:1045`; RECUSA/REABERTURA `chamados/services/disputa_emissao.py:1631`; SILENCIO `chamados/models.py:2005`, `ponto/services/veredito_celula.py:195`, `chamados/management/commands/backfill_decisoes_pergunta.py:324`; RETRATACAO `chamados/models.py:2137`.
- Consumidores completos de `linhas_da_projecao_sem_linha`: `chamados/models.py:1613`, `:3129`; `chamados/services/disputa_emissao.py:1480`, `:1673`; `chamados/services/validacao.py:167`, `:272`, `:345`; `chamados/services/tarefa_do_dia.py:430`; comandos `backfill_decisoes_pergunta.py:67`, **NOVO** `backfill_fio_morto_e4b.py:141`.
- Leitores DP adicionais **NOVO**: `PerguntaDisputa.respostas_anteriores`, `chamados/models.py:3675`; motivo BUG-PAR na L2, `chamados/services/materializacao.py:275`, `:276`; prefetch `chamados/services/fio.py:263`; carga da tarefa `chamados/services/tarefa_do_dia.py:421`; fotografia `chamados/services/foto_e4b.py:280`; comando backfill `chamados/management/commands/backfill_decisoes_pergunta.py:99`, `:315`.
- Helpers c3 `_leitura_dos_atos`, `_q_validada_pelo_ato`, `_leitura_conservada`: nenhuma definição localizada nesta cópia; ausência de hash Git local: [nao verificado]. Não confundir `respostas_anteriores` existente em `chamados/models.py:3668` com helper c3.

**Todos os consumidores das funções F2, enumerados concretamente**

| Função | Consumidores produtivos |
|---|---|
| `PerguntaDisputa.estado` | `api/views.py:2550`; `chamados/services/explicador.py:95`; `chamados/services/fio.py:660`; `chamados/services/vigia_fio.py:259`; `templates/chamados/partials/_card_atendimento.html:189`. Os três primeiros já constavam; referências antigas de vigia caíram de linha/forma. |
| `respondida` | Property em `chamados/models.py:3664`, lê somente R `:3665`. Nenhum acesso produtivo `.respondida` localizado; a menção `chamados/juizes.py:1551` é docstring, não consumidor. Cobertura dinâmica [nao verificado]. |
| `resposta_display` | Já listado: `api/views.py:2562`, `templates/chamados/partials/_pergunta_card.html:31`; **NOVO** `api/views.py:1164`, `chamados/services/disputa_emissao.py:1560`, `_card_atendimento.html:106`, `:143`, `:183`, `:391`. |
| `sem_veredito_entre` | `chamados/models.py:1906`; selo7 `chamados/juizes.py:2105`; `ponto/supra_juiz.py:573`, `:574`. |
| `respostas_sem_veredito` | `chamados/models.py:1934`, `:1967`; `chamados/reconciliador.py:1164`; `chamados/management/commands/validar_classe_a.py:54`; `chamados/services/competencia_trancada.py:126`; `chamados/services/fila_por_causa.py:226`; `colaboradores/services/desligamento.py:165`; `colaboradores/services/ficha.py:169`. |
| `pode_fechar` | `chamados/relatorios_disputa.py:24`; `api/foto_recebida.py:83`; `chamados/services/explicador.py:177`; `becos.py:33`, `:56`; `materializacao.py:1067`, `:1989`; `disputa_emissao.py:1292`, `:2113`; `competencia_trancada.py:178`, `:227`; `chamados/reconciliador.py:412`, `:612`, `:1171`; `ponto/services/entrada_sincronizada.py:59`. Fora os explicitamente enumerados em M8, **NOVO**. |
| `q_vivas` | `chamados/juizes.py:1751`; `chamados/services/fio.py:256`; `pergunta_no_app.py:53`; `vigia_fio.py:711`; **NOVO** `janela_colab.py:63`, `cobranca.py:237`, `raiox.py:74`, `ponto/services/veredito_celula.py:181`. `_ancoradas` não chama `q_vivas` nesta base (`chamados/models.py:344`). |
| `_ancoradas` | `chamados/models.py:223`, `:334`, `:381`; **NOVO** `chamados/services/disputa_emissao.py:1821`, `:1962`. |
| `_gravar_estado` | `chamados/models.py:450`, `:655`, `:835`, `:853`, `:1489`, `:1683`, `:1703`. |
| `DisputaSupervisao.fechar` | `chamados/models.py:2031`, `:2153`; `colaboradores/services/desligamento.py:167`; `chamados/services/materializacao.py:1068`, `:2006`; `disputa_emissao.py:1298`, `:2114`; `competencia_trancada.py:229`; `becos.py:57`; `chamados/management/commands/encerrar_perguntas_com_fato.py:88`; `chamados/reconciliador.py:413`, `:613`, `:1172`; `api/foto_recebida.py:84`; `ponto/services/entrada_sincronizada.py:60`. `ponto/management/commands/fechar_chamados_meio_periodo.py:110` chama **ChamadoColaborador.fechar**, excluir homônimo. |
| `remover_motivo` | `colaboradores/signals.py:77`; `escala/signals.py:174`. |
| `ressuscitar_perguntas` | `chamados/models.py:1498`; `chamados/views_arquivar.py:86`; `ponto/services/cartorio.py:1673`. |
| `_universo_da_ressuscitacao` | `chamados/models.py:1507`, `:1512`, `:1593`. |
| `projecao_temporal_da_pergunta` | `chamados/juizes.py:351`, `:1404`; `escala/services/cadastro_realidade.py:502`. |
| `irmas_respondidas_sem_consumo` | `chamados/reconciliador.py:448`; `chamados/services/materializacao.py:1244`; alias `_irmas_hx`, `chamados/services/disputa_emissao.py:2024`, chamada `:2032`; alias `_irmas12` importado em `chamados/management/commands/auditar_invariantes_chamados.py:975` (conferir sua chamada no bloco desse invariante). **NOVO em relação à enumeração inicial M8**, já incorporado ao selo7 pelo complemento. |

**Leitores de campos: inventário produtivo completo por arquivo**

R=`respondida_em`; T=`resposta_colab`; V=`validada_em`; VP=`validada_por` e `_id`; M=`materializada_em`; MP=`materializada_por` e `_id`; via=`via_resolucao`. Entram leituras por atributos, `getattr`, filtros, `only`, `values`, `update_fields`, fotos e parâmetros de portas; os escritores foram separados acima. Cada referência abaixo é concreta; arquivos não enumerados no brief são **NOVO**.

- `api/views.py`: R `:1165`, `:2563`, `:2577`, `:2620`, `:2637`; T `:1163`, `:2561`.
- **NOVO** `api/views_mensageria.py`: R `:569`, `:570`; T `:567`.
- **NOVO** `chamados/canal.py`: T `:65`.
- `chamados/juizes.py`: M `:292`, `:683`, `:711`, `:858`, `:1390`, `:1502`; R `:352`, `:856`, `:1389`, `:1423`, `:1534`, `:1541`, `:1552`, `:1572`, `:1580`, `:1744`, `:1753`; T `:364`, `:857`, `:1360`, `:1389`, `:1423`, `:1534`, `:1541`, `:1552`, `:1572`, `:1580`, `:1779`, `:1926`, `:2030`, `:2035`, `:2052`; V `:856`, `:1389`; via `:683`, `:695`, `:857`, `:1390`.
- `chamados/models.py`: `_gravar_estado` V `:310`; `_ancoradas` V `:344`; `_universo_da_ressuscitacao` M `:1531`, T `:1533`, via `:1535`, simulação V/R/T `:1554`–`:1556`; ressuscitar foto `:1614`, parâmetros V/VP/R/T `:1629`, `:1630`; lista V/M `:1923`, R `:1924`, T `:1925`; fechar via `:2001`, M `:2006`, T `:2007`, via `:2010`, `:2011`; remover via `:2136`, T `:2139`, via `:2141`; identidade via `:2829`, `:2860`; nascimento R/T `:2949`, `:2950`; estado V/M/R `:3151`, `:3153`, `:3155`, `:3156`, `:3158`; fechar_admin via/M `:3165`, `:3166`; portas carimbo foto trio/R/V/VP `:3236`, `:3237`, lista trio `:3238`, T `:3240`, R `:3241`, V `:3244`; identidade M/MP `:3292`, `:3293`, lista trio `:3296`; fora_fato via `:3311`; descarimbo trio `:3320`; CAMPOS_QUE_ACORDAM `:3331`; lote trio `:3365`, par `:3370`, `:3375`, `:3376`, `:3379`, `:3380`, `:3382`, `:3383`, via `:3392`; encerrar fato via `:3400`; fantasma via/M `:3411`; reabrir fantasma via/M `:3423`; reabrir resposta via/M `:3440`; reabrir competência via `:3447`; q_vivas via/M `:3483`, `:3485`, V `:3496`; guarda par `:3568`, `:3584`, R/T `:3585`, `:3586`; via `:3652`, `:3658`; respondida R `:3665`; display T `:3688`, `:3693`, `:3697`, `:3701`, `:3705`, `:3708`, `:3709`, `:3710`; legado R/V `:4091`, R `:4098`, `:4099`, `:4100`, T `:4101`, V `:4102`, `:4103`, `:4104`, VP/T `:4105`.
- `chamados/reconciliador.py`: M `:367`, `:630`, `:1050`, `:1145`, `:1155`, `:1505`; MP `:1146`, `:1156`; T `:365`, `:1083`; V `:631`, `:1050`; via `:1144`, `:1155`.
- **NOVO** `chamados/services/acoes_disputa.py`: R `:97`, `:132`; T `:355`; V `:347`.
- **NOVO** `chamados/services/becos.py`: R `:30`.
- `chamados/services/competencia_trancada.py`: via `:276`, `:282`.
- **NOVO** `chamados/services/declaracao_texto.py`: R `:417`, `:419`, `:687`; T `:417`; V `:227`.
- `chamados/services/disputa_emissao.py`: M `:228`, `:776`, `:1084`, `:1104`, `:1340`, `:1637`, `:1930`, `:2062`, `:2073`, `:2090`; R `:1483`, `:1609`, `:1635`, `:1677`, `:1692`, `:1698`, `:1703`, `:1735`, `:1843`, `:1861`, `:2074`, `:2091`; T `:1483`, `:1609`, `:1635`, `:1663`, `:1677`, `:1692`, `:1700`, `:1861`, `:2079`; V `:1484`, `:1485`, `:1496`, `:1537`, `:1538`, `:1609`, `:1636`, `:2062`; VP `:1484`, `:1496`, `:1537`, `:1538`, `:1609`, `:1636`; via `:337`, `:409`, `:428`, `:446`, `:1637`, `:2072`, `:2073`, `:2087`, `:2090`.
- `chamados/services/explicador.py`: T `:55`, `:102`, `:103`; via `:118`.
- **NOVO** `chamados/services/fabrica_da_celula.py`: M `:171`; via `:149`.
- `chamados/services/fio.py`: R `:187`, `:706`, `:1613`; T `:377`, `:1279`, `:1291`, `:1613`; via `:1147`, `:1214`, `:1290`, `:1397`, `:1480`, `:1921`, `:1957`, `:1979`, `:2024`.
- **NOVO** `chamados/services/foto_e4b.py`: M/R/V `:180`.
- **NOVO** `chamados/services/historico.py`: R `:99`, V `:101`, M `:103`, T `:109`.
- `chamados/services/materializacao.py`: M `:148`, `:261`, `:262`, `:289`, `:326`, `:862`, `:871`, `:1015`, `:1107`, `:1109`, `:1114`, `:1153`, `:1652`, `:1656`, `:2066`, `:2079`, `:2127`; MP `:1154`; R `:1151`, `:2064`; T `:546`, `:574`, `:625`, `:671`, `:678`, `:718`, `:733`, `:770`, `:869`, `:950`, `:1047`, `:1109`, `:1150`, `:1311`, `:1352`, `:1534`, `:1558`, `:1567`, `:1619`; V `:199`, `:201`, `:1152`, `:2065`, `:2082`; VP `:1152`; via `:261`, `:292`, `:862`, `:868`, `:1108`, `:1150`, `:1652`.
- **NOVO** `chamados/services/painel.py`: M/R/V `:67`, V `:270`, `:271`, `:272`, `:273`; VP `:262`.
- **NOVO** `chamados/services/passivo_validado.py`: M/MP/via `:64`, R/V/VP `:66`, via `:140`, `:141`, `:142`, `:530`, M `:534`, trio `:535`, M `:536`.
- `chamados/services/pergunta_no_app.py`: R `:81`.
- **NOVO** `chamados/services/raiox.py`: M `:135`, `:182`; R/T `:138`, `:147`.
- `chamados/services/relavra_via.py`: via `:118`, `:121`, `:123`, `:127`, `:134`, `:136`, `:155`, `:181`, `:245`, `:252`; M `:181`, `:244`, `:252`.
- `chamados/services/tarefa_do_dia.py`: via `:256`, `:257`.
- `chamados/services/validacao.py`: M `:262`, `:264`, `:341`, `:363`, `:556`, `:681`, `:701`, `:717`; MP `:342`, `:363`; R `:594`, `:813`, `:824`, `:848`, `:1167`; T `:175`, `:263`, `:281`, `:356`, `:380`, `:823`, `:843`, `:848`, `:1094`, `:1096`, `:1098`, `:1103`, `:1104`, `:1105`, `:1106`, `:1109`; V `:171`, `:173`, `:181`, `:262`, `:269`, `:276`, `:300`, `:302`, `:343`, `:351`, `:364`, `:489`, `:553`, `:597`, `:701`, `:810`, `:975`, `:996`; VP `:171`, `:181`, `:262`, `:276`, `:300`, `:302`, `:343`, `:351`, `:364`, `:489`; via `:341`, `:363`.
- **NOVO** `chamados/services/verbos.py`: R `:335`, `:336`, `:342`, `:345`; V `:264`, `:270`; VP `:264`, `:265`, `:266`, `:268`.
- **NOVO** `chamados/services/vigia_carimbo.py`: V `:98`.
- `chamados/services/vigia_fio.py`: R `:636`, `:746`; T `:749`.
- **NOVO** `chamados/templatetags/chamados_tags.py`: T `:139`, `:166`.
- **NOVO** `chamados/veredito_lavrado.py`: M `:203`, `:204`; R `:207`, `:208`.
- **NOVO** `chamados/views.py`: M `:1257`; R `:1280`, `:1416`.
- **NOVO** `chamados/views_cobrar.py`: R `:169`, `:176`; T `:291`.
- **NOVO** `colaboradores/services/calendario.py`: T `:504`.
- `colaboradores/services/ficha.py`: T `:171`.
- **NOVO** `folha/previa.py`: T `:142`.
- **NOVO** `ponto/services/cartorio.py`: T `:1641`; via `:1650`.
- **NOVO** `ponto/services/dia_do_colab.py`: T `:70`.
- **NOVO** `ponto/services/flip_auto.py`: V `:130`; via `:129`.
- **NOVO** `ponto/services/procedencia_batida.py`: MP/VP `:71`, `:72`; M/V `:74`.
- **NOVO** `ponto/services/veredito_celula.py`: M `:196`; T `:197`.

**Comandos adicionais, também entram no censo**

Todos sob `app/chamados/management/commands/`, salvo indicado.

- **NOVO** `auditar_disputas.py`: M `:71`, `:150`.
- **NOVO** `auditar_invariantes_chamados.py`: M `:470`, `:538`, `:588`, `:905`, `:955`, `:1075`, `:1156`, `:1577`, `:1604`, `:1769`; R `:177`, `:592`, `:1105`, `:1605`; T `:1106`, `:1657`; V `:205`, `:1075`, `:1576`, `:1604`, `:1768`, `:1780`; VP `:1076`; via `:202`, `:206`, `:433`, `:542`, `:955`, `:1712`.
- **NOVO** `backfill_decisoes_pergunta.py`: M `:101`, `:325`; MP `:326`; R `:56`, `:60`; T `:61`, `:326`, `:504`; V `:56`, `:60`, `:102`; VP `:60`; via `:101`, `:314`, `:330`, `:335`, `:336`, `:398`, `:504`.
- **NOVO** `bug_par_103.py`: M/via `:66`; MP `:67`; V/VP `:68`.
- **NOVO** `curar_perguntas_em_limbo.py`: R `:46`, `:69`; T `:32`, `:46`.
- **NOVO** `dedup_perguntas_fato.py`: T `:75`.
- **NOVO** `encerrar_perguntas_com_fato.py`: M/V `:32`; via `:33`, `:42`, `:49`, `:66`, `:75`, `:76`.
- **NOVO** `migrar_data_evento_dia_do_turno.py`: M `:147`; via `:70`, `:71`, `:72`.
- **NOVO** `reabrir_orfas_enterradas.py`: M `:67`, `:88`, `:91`, `:129`, `:142`, `:160`; MP `:130`, `:143`, `:160`; V `:68`; via `:67`, `:129`, `:142`, `:160`.
- **NOVO** `relavrar_via_i6.py`: via `:58`, `:59`, `:62`, `:66`, `:68`, `:88`.
- `validar_classe_a.py`: T `:80`.
- **NOVO** `ponto/management/commands/reconciliar_fantasmas.py:213`: M.
- **NOVO** `ponto/management/commands/reconciliar_grade.py:461`: R.

**Templates, incluindo uso em formulário**

- `_pergunta_card.html`: V `:20`, `:23`, `:42`; R `:24`, `:29`, `:32`; display `:31`; novos estados BUG-PAR `:44`, `:47`.
- `modal_fio.html`: R `:185`, `:217`, `:220`; T `:219`; V `:228`; novos estados BUG-PAR `:230`, `:252`, `:274`; `pode_fechar` já transformado pelo explicador em `:462`, `:473`.
- `_card_atendimento.html`: R `:106`, `:143`, `:391`; T `:106`, `:114`, `:141`, `:143`, `:148`, `:154`, `:182`, `:183`, `:391`; display `:106`, `:143`, `:183`, `:391`; estado `:189`; via `:192`. A seleção de formulário usa o valor atual T, não apenas display; entra no selo se F2 mudar seu valor.
- **NOVO** `_lote_preview.html:24`: T.

**C2 temporal e BUG-PAR**

- C2 estrito: três chamadores produtivos da projeção temporal: `_slot_esperado`, `chamados/juizes.py:351` (usa R em `:352`); `_pergunta_viva_calc`, `:1404` (usa M para vitalidade); `beco_em_lote`, `escala/services/cadastro_realidade.py:502` (somente índice `[1]`, R). `sem_veredito_entre` não recebe `agora` `chamados/models.py:1910`; `estado` chama C6 sem ele `:3154`; supra chama lista sem ele `ponto/supra_juiz.py:573`, `:574`. Nenhum deles reconstrói instante apagado; a projeção começa nos campos atuais `chamados/juizes.py:1389`, `:1390`. O **CENSO=0 do predicado estrito** confere por leitura desses caminhos; produção [nao verificado].
- **NOVO leitor temporal fora do C2 estrito:** `chamados/veredito_lavrado.py:163`, `:166`, `:203`, `:204` filtra `materializada_em <= ate` do campo atual; não confundir com reconstrução após descarimbo.
- BUG-PAR L2 lê a trilha, não recalcula: `chamados/services/materializacao.py:267`, `:275`, `:276`; escritor `chamados/services/validacao.py:354`, `:355`, `:357`; display atual via `chamados/services/materializacao.py:339`/testemunha.
- `k6_consulta_a_pergunta`: `chamados/services/materializacao.py:792`, predicado `:795`; `_k6_que_barra` chama em `:806`; `validacao.entrega_que_barra_a_validacao` chama em `chamados/services/validacao.py:224`. A otimização dispensa buscar irmã quando a própria pergunta é consultada; justificativa do mesmo fato/mesma disputa em `:219`, `:222`.
- `bloqueada_sem_dia_do_fato`: constante `chamados/services/materializacao.py:706`; `_k6_que_barra` retorna em `:810`; porta traduz em `chamados/services/validacao.py:233`; testemunha retorna em `chamados/services/materializacao.py:888`; telas leem `_pergunta_card.html:44`, `:47`, `modal_fio.html:230`, `:252`, `:274`. Não equiparar a `bloqueada_competencia_entregue`, cujo ramo distinto está em `chamados/services/materializacao.py:811`, `:812`.
- Censo/testes existentes inclui a classe de guarda sem dia: `chamados/tests/test_bug_par_volta.py:306`, asserções `:312`, `:317`, `:318`, `:338`; extrator sem banco carrega helper real em `chamados/tests/test_bug_par_sem_banco.py:140`. Execução [nao verificado].

## 16/11 23:10 · sessao 74a6e4ce50a2 · - · - · origem=Ronald (tmux)

Censo realizado por AST de todos os `.py` em `app/` (excluídos `tests/`, `migrations/`, `__pycache__` e `tests.py`) e grep de todos os templates; referências por nome são teto, com homônimos identificados abaixo. O script reproduzível está ao fim. Não executei banco nem testes e não criei arquivos. [nao verificado] [nao verificado]

**Achados que derrubam o brief**

- **CAIU “três chamadores”**: `_ts_tipo_do_plantio` tem **cinco sítios de chamada**, em quatro funções: juiz `juizes.py:376`; **NOVO** `par_do_intervalo` em `materializacao.py:214,222`; testemunha `materializacao.py:895`; materializador `materializacao.py:1328`. O RED de paridade precisa alcançar também as duas metades do par. (`app/chamados/juizes.py:376`; `app/chamados/services/materializacao.py:214`; `app/chamados/services/materializacao.py:222`; `app/chamados/services/materializacao.py:895`; `app/chamados/services/materializacao.py:1328`)
- **NOVO** `data_conferida_em` tem leitor em `_data_em_disputa`, além de `_materializar_uma`; A20 não se resume ao sítio do brief. (`app/chamados/services/materializacao.py:169`; `app/chamados/services/materializacao.py:1851`)
- **NOVOS leitores R/V/C BUG-PAR**: `par_do_intervalo` lê V da irmã; `_declarado_da_metade`, `divergencia_declarado_x_gravado`, `espera_na_escrita` leem R; `_situacao_da_irma`, `paridade_absolvida_bug_par_103`, contador e nova testemunha leem C/VIA. (`app/chamados/services/materializacao.py:199`; `app/chamados/services/materializacao.py:201`; `app/chamados/services/materializacao.py:718`; `app/chamados/services/materializacao.py:733`; `app/chamados/services/materializacao.py:770`; `app/chamados/services/materializacao.py:261`; `app/chamados/services/materializacao.py:289`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:862`)
- **NOVO escritor de V**: `devolver_paridade_bug_par_103` limpa V/autor, conserva R, registra DEVOLUCAO, sem limpar `recusa_motivo`. (`app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:350`; `app/chamados/services/validacao.py:354`; `app/chamados/services/validacao.py:364`)
- **NOVOS chamadores `_materializar_uma`**: `_gravar_irma_do_par`, além de `_validar_o_par` e `validar_pergunta`; o passivo agora chama em 537 e a fila em 2084. (`app/chamados/services/materializacao.py:1707`; `app/chamados/services/validacao.py:555`; `app/chamados/services/validacao.py:687`; `app/chamados/services/passivo_validado.py:537`; `app/chamados/services/materializacao.py:2084`)
- `estado_aplicacao_pergunta` tem leitores **NOVOS** no contador, na espera de declarado×gravado e no templatetag; o explicador recebe a mesma função por import no início do arquivo. (`app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:757`; `app/chamados/templatetags/chamados_tags.py:134`; `app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/services/explicador.py:124`)

**Escritores por estado/entidade**

| Campo | Escritores persistentes/portas; simulações separadas |
|---|---|
| `resposta_colab` | Respondedor `disputa_emissao.py:1481`; automático `:1674`; lote de reabertura `:1609`→`:1648`; ressuscitação `models.py:1629`→`:1630`; nascimento por `**campos` `models.py:3124`→`:3127`. `models.py:1556` é simulação em cópia para julgamento. |
| `respondida_em` | Respondedor `disputa_emissao.py:1482`; automático `:1675`; reparo da trilha `:1696`; lote `:1609`→`:1648`; ressuscitação `models.py:1629`; `carimbar(resposta=)` `models.py:3242`; nascimento `models.py:3124`. Simulação `:1555`, reversão em memória `:3273`. |
| `validada_em/por` | `validacao.py:168`→`:169`; devolução normal `:273`→`:274`; **NOVO** devolução BUG-PAR `:349`→`:350`; nova resposta limpa `disputa_emissao.py:1494`→`:1495`; lote `:1609`; ressuscitação `models.py:1629`; `carimbar(resposta='validar')` `models.py:3245`→`:3246`; nascimento `:3124`. Simulação `:1554`, reversão em memória `:3274`→`:3275`. |
| `materializada_em` | Atribuição canônica `_escrever_carimbo` `models.py:3209`, acionada por portas; lote `models.py:3393`. `regularizacao_ext.py:199` escreve **outro modelo**, `reg`, não PerguntaDisputa. |
| `via_resolucao` | `_escrever_carimbo` `models.py:3211`; lote `:3393`; porta específica `encerrar_por_fato_no_espelho` `:3402`; relavra `relavra_via.py:125`. |
| `recusa_motivo` | Resposta escape `disputa_emissao.py:1511`; pendência técnica `materializacao.py:1332`; recusa `materializacao.py:1665` ou extra da porta de identidade `models.py:3298`; recusa administrativa `validacao.py:827`. |
| `data_conferida_em` | `validacao.py:907`; dois leitores do materializador `materializacao.py:169,1851`. |
| `fechada_em` **Disputa** | `models.py:2013` (`fechar`), `:2230` (`reabrir_admin`), `:2254` (`reabrir_sistema`). `models.py:190` e outros nomes de atributos genéricos são teto por nome, sem atribuição automática a Disputa. |

**Furo de escritor continua**: `carimbar(resposta=)` atribui R/V e salva sem registrar atos RESPOSTA/VALIDACAO; absorção chama com `resposta='validar'` e registra só ABSORCAO; resolução de chamado chama com `resposta='responder'`. (`app/chamados/models.py:3240`; `app/chamados/models.py:3242`; `app/chamados/models.py:3245`; `app/chamados/models.py:3262`; `app/chamados/services/materializacao.py:1043`; `app/chamados/services/materializacao.py:1045`; `app/chamados/services/disputa_emissao.py:2082`)

**Todos os sítios AST por nome — produção**

Legenda: `R=resposta_colab`, `r=respondida_em`, `V=validada_em`, `v=validada_por[/_id]`, `C=materializada_em`, `M=recusa_motivo`, `I=via_resolucao`, `D=data_conferida_em`, `F=fechada_em`. A lista inclui escritores, leitores, filtros, listas de campos e fotos; classificação de escrita acima. Cada `arquivo:linha` abaixo é diretamente da BASE.

```text
app/api/foto_recebida.py F:66,78
app/api/views.py F:1018,1034,2678 R:1163,2561 r:1165,2563,2577,2620,2637
app/api/views_mensageria.py R:567 r:569,570
app/chamados/canal.py R:65
app/chamados/juizes.py C:292,683,711,858,1390,1502 F:2466 I:683,695,857,1390 R:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052 V:856,1389 r:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753
app/chamados/management/commands/auditar_chamados_orfaos.py F:41
app/chamados/management/commands/auditar_disputas.py C:71,150 F:87,136,167
app/chamados/management/commands/auditar_invariantes_chamados.py C:470,538,588,905,955,1075,1156,1577,1604,1769 F:592,806 I:202,206,433,542,955,1712 M:536,1075,1112 R:1106,1657 V:205,1075,1576,1604,1768,1780 r:177,592,1105,1605 v:1076
app/chamados/management/commands/backfill_decisoes_pergunta.py C:101,325 I:101,314,330,335,336,398,504 R:61,326,504 V:56,60,102 r:56,60 v:60
app/chamados/management/commands/bug_par_103.py C:66 I:66 M:69 V:68 v:68
app/chamados/management/commands/curar_perguntas_em_limbo.py R:32,46 r:46,69
app/chamados/management/commands/dedup_perguntas_fato.py R:75
app/chamados/management/commands/encerrar_perguntas_com_fato.py C:32 F:82 I:33,42,49,66,75,76 V:32
app/chamados/management/commands/fechar_becos_disputa.py F:32
app/chamados/management/commands/migrar_data_evento_dia_do_turno.py C:147 I:70,71,72
app/chamados/management/commands/reabrir_orfas_enterradas.py C:67,88,91,129,142,160 F:96,131,164,165 I:67,129,142,160 V:68
app/chamados/management/commands/relavrar_via_i6.py I:58,59,62,66,68,88
app/chamados/management/commands/validar_classe_a.py R:80
app/chamados/models.py C:1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485 F:190,1638,1870,1888,1960,1964,2013,2017,2020,2112,2114,2219,2230,2235,2248,2254,2258,2305,2343,2778,2781 I:1535,2001,2010,2011,2136,2141,2829,2860,3165,3211,3236,3238,3296,3311,3320,3331,3365,3392,3393,3400,3402,3403,3411,3423,3440,3447,3483,3485,3652,3658 M:3298,3299 R:1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105 V:310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104 r:1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100 v:1614,1629,3237,3246,3247,3275,4105
app/chamados/reconciliador.py C:367,630,1050,1145,1155,1505 F:412,600,611,612,1161,1162 I:1144,1155 R:365,1083 V:631,1050
app/chamados/relatorios_disputa.py F:46
app/chamados/services/acoes_chamado.py F:346
app/chamados/services/acoes_disputa.py F:257,349 M:83 R:355 V:347 r:97,132
app/chamados/services/becos.py F:27,56,59 r:30
app/chamados/services/competencia_trancada.py F:176,221 I:276,282
app/chamados/services/declaracao_texto.py R:417 V:227 r:417,419,687
app/chamados/services/disputa_emissao.py C:228,776,1084,1104,1340,1637,1930,2062,2073,2090 F:41,541,992,1111,1347,1536,2026,2050,2052,2111,2113 I:337,409,428,446,1637,2072,2073,2087,2090 M:1511,1513 R:1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079 V:1484,1485,1494,1496,1537,1538,1609,1636,2062 r:1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091 v:1484,1495,1496,1537,1538,1609,1636
app/chamados/services/explicador.py I:118 M:113 R:55,102,103
app/chamados/services/fabrica_da_celula.py C:171 I:149
app/chamados/services/fila_por_causa.py F:225,259,262
app/chamados/services/fio.py F:131,646 I:1147,1214,1290,1397,1480,1921,1957,1979,2024 M:564,1407 R:377,1279,1291,1613 r:187,706,1613
app/chamados/services/foto_e4b.py C:180 F:182 V:180 r:180
app/chamados/services/historico.py C:103 R:109 V:101 r:99
app/chamados/services/materializacao.py C:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127 D:169,1851 F:1067,1989 I:261,292,862,868,1108,1150,1652 M:1332,1333,1652,1655,1663,1665,1666 R:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619 V:199,201,1152,2065,2082 r:1151,2064 v:1152
app/chamados/services/painel.py C:67 V:67,270,271,272,273 r:67 v:262
app/chamados/services/passivo_validado.py C:64,534,535,536 I:64,140,141,142,530,535 M:66 V:66 r:66 v:66
app/chamados/services/pergunta_no_app.py F:81 r:81
app/chamados/services/raiox.py C:135,182 R:138,147 r:138,147
app/chamados/services/regularizacao_ext.py C:199,201,220 F:60
app/chamados/services/relavra_via.py C:181,244,252 F:260 I:118,121,123,125,127,134,136,155,181,245,252
app/chamados/services/tarefa_do_dia.py I:256,257
app/chamados/services/validacao.py C:262,264,341,363,556,681,701,717 D:905,907,909 F:182,288,291,293 I:341,363 M:181,262,266,282,299,301,302,344,364,825,827,833,847 R:175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109 V:168,171,173,181,262,269,273,276,300,302,343,349,351,364,489,553,597,701,810,975,996 r:594,813,824,848,1167 v:169,171,181,262,274,276,300,302,343,350,351,364,489
app/chamados/services/verbos.py V:264,270 r:335,336,342,345 v:264,265,266,268
app/chamados/services/vigia_carimbo.py V:98
app/chamados/services/vigia_fio.py R:749 r:636,746
app/chamados/signals.py F:29,131,155
app/chamados/templatetags/chamados_tags.py R:139,166
app/chamados/veredito_lavrado.py C:203,204 r:207,208
app/chamados/views.py C:1257 F:471,911,1329 r:1280,1416
app/chamados/views_cobrar.py F:161,164 R:291 r:169,176
app/colaboradores/painel_cell.py F:32
app/colaboradores/services/calendario.py R:504
app/colaboradores/services/desligamento.py F:164
app/colaboradores/services/drawer.py F:72
app/colaboradores/services/ficha.py R:171
app/colaboradores/services/situacional.py F:117
app/colaboradores/signals.py F:73
app/escala/signals.py F:170
app/folha/previa.py R:142
app/ponto/management/commands/alertar_disputas_pendentes.py F:34
app/ponto/management/commands/escalonar_chamados_supervisao.py F:58,149
app/ponto/management/commands/reconciliar_fantasmas.py C:213
app/ponto/management/commands/reconciliar_grade.py r:461
app/ponto/services/cartorio.py I:1650 R:1641
app/ponto/services/dia_do_colab.py R:70
app/ponto/services/entrada_sincronizada.py F:57
app/ponto/services/flip_auto.py I:129 M:132 V:130
app/ponto/services/procedencia_batida.py C:74 V:74 v:71,72
app/ponto/services/veredito_celula.py C:196 R:197
```

Definições dos campos não entram na listagem de referências AST acima: `models.py:1821` (Disputa F); `:2552`/`:2553`/`:2554` (R/r/V); `:2563` (D); `:2665`/`:2666` (M/I); `:3514` (v); `:3530` (C). `models.py:4152` é outro modelo. (`app/chamados/models.py:1821`; `app/chamados/models.py:2552`; `app/chamados/models.py:2563`; `app/chamados/models.py:2665`; `app/chamados/models.py:3514`; `app/chamados/models.py:3530`; `app/chamados/models.py:4152`)

**Leitores de templates — grep completo dos nomes**

- `_pergunta_card.html`: V `20,23,42`; r `24,29,32`; estados de aplicação **NOVOS BUG-PAR** `44,47`. (`app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/_pergunta_card.html:32`; `app/templates/chamados/partials/_pergunta_card.html:42`; `app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:47`)
- `modal_fio.html`: D `170,171`; r `185,217,220`; R `219`; M `226`; V `228`; estados **NOVOS** `230,232,234,236,238,240,242,244,246,248,252,254,256,258,260,262,264,266,268,270,274`; F `406`. Inputs M são `61,322`, não leitores do valor. (`app/templates/chamados/partials/modal_fio.html:170`; `app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/modal_fio.html:228`; `app/templates/chamados/partials/modal_fio.html:230`; `app/templates/chamados/partials/modal_fio.html:252`; `app/templates/chamados/partials/modal_fio.html:274`; `app/templates/chamados/partials/modal_fio.html:406`)
- `_lote_preview.html`: R `24`. (`app/templates/chamados/partials/_lote_preview.html:24`)
- `_card_atendimento.html`: R/r `106`; R `114,141,143,148,154,182,183`; VIA/M `192`; R/r `391`. Linhas `95,134,188,190` são comentários. (`app/templates/chamados/partials/_card_atendimento.html:106`; `app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:143`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:154`; `app/templates/chamados/partials/_card_atendimento.html:182`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/templates/chamados/partials/_card_atendimento.html:391`)

**Funções e autoridades: chamadores com identidade conferida**

| Função | Todos os sítios diretos de produção encontrados |
|---|---|
| `_ts_tipo_do_plantio` | `juizes.py:376`; `materializacao.py:214,222,895,1328` |
| `_tipo_pelo_marco` | `materializacao.py:656` |
| `_materializar_uma` | `materializacao.py:1707,2084`; `passivo_validado.py:537`; `validacao.py:555,687` |
| `_materializar_resposta_hora/data_hora/escolha` | `materializacao.py:1889/1891/1893` |
| `_materializar_ausencia_de_resposta` | `disputa_emissao.py:1555` |
| `materializar_perguntas_validadas_da_disputa` | `backfill_fio_morto_e4b.py:147`; **NOVO vs lista do brief** `acoes_disputa.py:141`, `fio.py:716`, `validacao.py:698` |
| `materializar_pendentes_aprovadas` | `signals.py:165` |
| `estado_aplicacao_pergunta` | `fio.py:376`; `materializacao.py:339,757`; `explicador.py:124`; `templatetags/chamados_tags.py:136` |
| `par_do_intervalo` **NOVO** | `materializacao.py:907,1695`; `validacao.py:545` |
| `metade_irma_do_intervalo` **NOVO** | `materializacao.py:194,2019`; `validacao.py:206` |
| `competencia_do_fato` **NOVO, desta materialização** | `materializacao.py:330,808`; `validacao.py:338`; `bug_par_103.py:58,74,116` (import `:41`). Os usos nos comandos backfill/migração e `ponto` são homônimos de `ponto.services.fechamento`, **excluídos**. |
| `k6_consulta_a_pergunta` **NOVO** | `materializacao.py:806`; `validacao.py:224` |
| `_k6_que_barra` **NOVO** | `materializacao.py:825`; `validacao.py:225` |
| `_competencia_que_barra` **NOVO** | `materializacao.py:909,933,1450,1830` |
| `espera_declarado_x_gravado` **NOVO** | `explicador.py:132`; `validacao.py:611`; `templatetags/chamados_tags.py:128` |
| `espera_na_escrita` **NOVO** | `materializacao.py:1877` |
| `divergencia_declarado_x_gravado` **NOVO** | `materializacao.py:741` |
| `_declarado_da_metade` **NOVO** | `materializacao.py:728` |
| `_espera_do_aval_163` **NOVO** | `materializacao.py:922,936,1451` |
| `respostas_intervalo_recusadas_paridade` **NOVO** | **um leitor direto** `bug_par_103.py:79` |
| `paridade_absolvida_bug_par_103` **NOVO** | `materializacao.py:327`; `validacao.py:334` |
| `rotulo_bug_par_103` **NOVO** | `materializacao.py:880` |
| `perguntas_absorvidas_por` | `materializacao.py:1147` |
| `devolver_absorvidas_da_ausencia_rejeitada` | `ponto/signals.py:262` |
| `_carimbar_recusa` | `materializacao.py:1318,1430,1871,1966`; `passivo_validado.py:566`; `validacao.py:562` |
| `responder_pergunta_disputa_core` | `api/views.py:2627`; `acoes_disputa.py:373`; `declaracao_texto.py:419,693`; `views.py:1283` |
| `responder_automaticamente` | `api/foto_recebida.py:48` |
| `carimbar_resposta_pela_trilha` | `curar_perguntas_em_limbo.py:67` |
| `reabrir_para_correcao` | `curar_perguntas_em_limbo.py:69`; `models.py:2318` (alias import `:2300`); `acoes_disputa.py:105`; `materializacao.py:1164`; `validacao.py:836`; `views.py:1436` |
| `recobrar_questionario` | `views.py:1425` |
| `reabrir_credor_de_pergunta_viva` | `backfill_fio_morto_e4b.py:90,135,175`; `disputa_emissao.py:1969` |
| `resolver_perguntas_por_chamado_resolvido` | `reconciliador.py:537,1649`; `becos.py:54`; `signals.py:117`; `ponto/management/commands/reconciliar_fantasmas.py:281` |
| `_resolver_perguntas_por_chamado_resolvido` | `disputa_emissao.py:1897,1903` |
| `_fechar_disputa_se_quitada` | `materializacao.py:2086`; `passivo_validado.py:413` |
| `relavrar` / `via_de_hoje` | `relavrar_via_i6.py:102` e `relavra_via.py:157` / `relavrar_via_i6.py:64` e `relavra_via.py:120` |
| `q_carimbo_desfazivel` | `migrar_data_evento_dia_do_turno.py:446`; `fabrica_da_celula.py:150`; `relavra_via.py:238`; `vigia_fio.py:769` |
| `desfazer_carimbo_sem_lastro` | `fabrica_da_celula.py:169,209` |
| `reparar` / `_plantar_o_dia` | `materializar_passivo_validado.py:104,113` / `passivo_validado.py:443` |
| `motivos_a_perguntar` | `disparar_perguntas_competencia.py:117`; `fabrica_da_celula.py:207,242` |
| `perguntar` | `perguntar_chamados_sem_pergunta.py:29`; `fabrica_da_celula.py:219` |

**Contador e U163**

Há um leitor direto de produção do contador (`bug_par_103.py:79`), que imprime `pendentes_aval`/`congelados_no_recorte`; a montagem da Pauta é outro percurso, não leitor dos baldes do contador. (`app/chamados/management/commands/bug_par_103.py:53`; `app/chamados/management/commands/bug_par_103.py:71`; `app/chamados/management/commands/bug_par_103.py:79`; `app/chamados/management/commands/bug_par_103.py:81`; `app/chamados/management/commands/bug_par_103.py:85`)

Retirar U163 do pulo K6 altera tanto `_k6_que_barra` quanto a decisão de procurar irmã em `entrega_que_barra_a_validacao`; é o mesmo predicado, sem cópia. (`app/chamados/services/materializacao.py:806`; `app/chamados/services/validacao.py:224`)

O novo estado L1/L5 de U163 não carimbada seria `outras`, logo continuaria em `universo` e `pendentes_aval`; os baldes especiais atuais são só do ramo carimbado elegível U103. Destino futuro é pendência do Ronald, não decisão deste censo. (`app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:331`; `app/chamados/services/materializacao.py:334`; `app/chamados/services/materializacao.py:347`; `app/chamados/services/materializacao.py:351`; `app/chamados/services/materializacao.py:356`; anexo `revisao.r`, item 3 [nao verificado em arquivo local])

**Script AST 

[truncado em 20000 de 22785 caracteres]

## 16/11 23:10 · sessao 08b2e3bb47c8 · - · - · origem=Ronald (tmux)

Revalidação estritamente por leitura; nenhuma suíte, banco ou arquivo alterado. [nao verificado]

**Delta certificado:** comparei os bytes da cópia com `git show 520d7b557:<arquivo>`: `models.py`, `juizes.py`, `tarefa_do_dia.py`, `explicador.py`, `_pergunta_card.html`, `modal_fio.html`, `CORTES.md` coincidem. A cópia não tem `.git`; o histórico foi lido somente por objetos no repositório `/home/ronald/saas-hasner`. Evidência persistida em arquivo: [nao verificado].

**Linhas do anexo matriz.md**, contadas do conteúdo fornecido incluindo vazias:

- Premissa F1=be2b8d733: **14**.
- Premissa helper/leitura conservada: **27–29**.
- Título DEPENDE-F1: **69**.
- Itens 1–6: **70–71; 72–73; 74; 75; 76; 77**, respectivamente.
- Re-medida: **112–117**; resumo DEPENDE-F1: **116**.
- Essa contagem concorda com o próprio pedido, que localiza o parágrafo residual em `matriz.md:112–115`.

| Premissa do anexo | Julgamento na BASE | Evidência BASE |
|---|---|---|
| `matriz.md:14`, `:27–29`: F1 vigente é be2b8d733, helpers e instante conservado existem | **CAIU** | Projeção lê os campos atuais e devolve `materializada`, `app/chamados/juizes.py:1389`, `:1390`, `:1400`; descarimbo apaga o instante, `app/chamados/models.py:3318`. Os identificadores `_leitura_conservada`, `_leitura_dos_atos`, `_q_validada_pelo_ato`, `materializada_conservada_em` não foram encontrados nos módulos atuais; localização positiva deles: [nao verificado]. |
| DEP 1 `:70–71`: `[3]` é instante conservado, inclusive depois de descarimbo | **CAIU** | Campo atual em `app/chamados/juizes.py:1390`; descarimbo em `app/chamados/models.py:3318`. A conservação vigente C3 conserva antes/depois da F2; não recupera instante apagado. |
| DEP 2 `:72–73`: assinatura conservada reaplicada no caso passivo | **CAIU** como premissa da BASE | A projeção apenas recorta campos futuros; não reaplica assinatura, `app/chamados/juizes.py:1391–1400`. Execução do antigo T148: [nao verificado]. |
| DEP 3 `:74`: relógio via E desaparece de `[3]` | **CONFERE** para o resultado, **CAIU** a explicação “pela F1 be2” | `[3]` é campo atual, sem testemunha E, `app/chamados/juizes.py:1390`, `:1400`. |
| DEP 4 `:75`: F1 sozinha já lê ato e rompe continência de q_vivas | **CAIU** | C6 usa projeção dos campos, `app/chamados/juizes.py:1404`; `q_vivas` usa validação projetada, `app/chamados/models.py:3496`; continência continua o contrato declarado, `:3467`. |
| DEP 5 `:76`: guarda PK nula do helper muda resultado das instâncias não salvas | **CAIU** para os leitores atuais | `estado` lê campos em `app/chamados/models.py:3151`; `respondida` em `:3665`; display em `:3688`. Não há chamada àquele helper nesses corpos. |
| DEP 6 `:77`: NameError do teste de memória F1c1 herdado | **CAIU** para o defeito identificável por leitura | O diff `40052af46..520d7b557` adiciona `SimpleNamespace=NS`, `texto_vazio` e carrega `pergunta_respondida` real no harness: `app/chamados/tests/test_e4b_classe_e_memoria.py:308–312`; teste temporal existente `::AlcanceTest.test_RED_harness_carrega_projecao_real_sem_alterar_pergunta`, `:374–391`. Resultado por execução: [nao verificado]. |

**BUG-PAR: premissas que precisam entrar na parte 1**

| Premissa | Julgamento e prova |
|---|---|
| “Entregue e sem dia do fato têm o mesmo estado” | **CAIU**: constantes distintas `bloqueada_competencia_entregue` L1 e `bloqueada_sem_dia_do_fato` L5 em `app/chamados/services/materializacao.py:705`, `:706`; retornos distintos em `:809–814`. |
| Sem `data_evento`, K6 não é consultado | **CONFERE**: `competencia_do_fato` retorna antes da consulta, `app/chamados/services/materializacao.py:784–789`; teste `app/chamados/tests/test_bug_par_volta.py::BugParVoltaTest.test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio` — conferir nome da classe — assertions `:312–321`, incluindo K6 `assert_not_called` em `:319`. |
| L2 F6 fechado/aprovado não entregue conserva toque Validar e não vira paridade | **CONFERE**: testemunha `bloqueada_competencia`, `app/chamados/services/materializacao.py:909–911`, `:933–935`; motivo verdadeiro preservado depois do rollback em `:1469–1477`; formatador canônico `:1789–1795`. |
| L2 deve produzir `COMPETENCIA_APROVADA` também no laço e passivo | **CONFERE**, provas existentes: `app/chamados/tests/test_bug_par_volta.py::test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo`, `:237–273`; `::test_matriz_linha2_laco_conserva_o_motivo`, `:378–389`; `::test_matriz_linha2_passivo_conserva_o_motivo`, `:391–403`. Asserções explicitam VALIDACAO+DEVOLUCAO, nenhuma batida e motivo COMPETENCIA_APROVADA. |
| `k6_consulta_a_pergunta` pode ser substituído por “fora U163” genérico | **CAIU**: predicado exige pergunta não nula **e metade de intervalo** fora U163, `app/chamados/services/materializacao.py:792–795`. |
| c4c sempre procura a irmã para K6 | **CAIU**: `app/chamados/services/validacao.py:224` pula a busca quando a guarda consulta a própria pergunta; senão usa `irma_da_validacao`. O universo da irmã é mesma disputa/data, `app/chamados/services/materializacao.py:139–149`. |
| Só entregue tira Validar | **CAIU**: sem dia também tira, assim como espera e indisponibilidade; `app/templates/chamados/partials/_pergunta_card.html:44–50`; `app/templates/chamados/partials/modal_fio.html:276–279`. F6 `bloqueada_competencia` conserva toque nesses mesmos sítios. |
| Precedência K6 → F6 → espera declarado | **CONFERE**: K6 em `app/chamados/services/materializacao.py:885–888`; F6 do par em `:909–911` antes de espera `:922–924`; metade isolada F6 em `:933–935` antes da espera `:936–938`; criação tardia conserva ordem em `:1447–1453`. Prova `app/chamados/tests/test_bug_par_volta.py::test_matriz_precedencia_k6_antes_de_f6_antes_de_declarado`, `:323–345`. |

**Diff `71e9576bf..520d7b557` nos sítios citados**

- **Sem alteração de conteúdo nem linha:** `app/chamados/models.py`, `app/chamados/juizes.py`, `app/chamados/services/tarefa_do_dia.py`, `app/chamados/services/pergunta_no_app.py`, `app/api/views.py`, `app/ponto/supra_juiz.py`, `_card_atendimento.html`. Portanto todas as referências de estados.md a esses arquivos ainda casam; a divergência “leitor por campos versus c3 por atos” continua evidenciada por `models.py:3151`, `:3665`, `:3688`, `:1923`, `:3496`.
- **`explicador.py`**: referências antigas 72→95; 73→96; 74→97; 77→100; 85→108; 93→116; 139→177. Antiga 102 **mudou de conteúdo**: agora `_sem_gravar(...,'sistema')` em `app/chamados/services/explicador.py:142`. Entraram tabela `SEM_GRAVAR_POR_DONO` `:33`, helper `_sem_gravar` `:43`, recusa com `gravou=False` `:124–126`, espera declarado `:131–136`.
- **`fio.py`**: antiga187=187; 357→359; 652→660. Prefetch `colaborador__empresa` em `app/chamados/services/fio.py:217`; testemunha ausente/exceção agora produz indisponível em `:378–385`.
- **`_pergunta_card.html`**: antigas23/25/29/31 continuam exatamente nessas linhas; bloco novo da aplicação em `app/templates/chamados/partials/_pergunta_card.html:43–51`.
- **`modal_fio.html`**: antiga436→463; 440→467. Antiga442 **mudou**: `app/templates/chamados/partials/modal_fio.html:469` agora lê `it.quem_resolve`, sem tradução própria TI/DP por dono. Novos estados e supressão de Validar em `:230–279`.
- **UI-VINCULO/EDITOR-CAL:** diff também alcança painel_vinculo e folga_calendario_editar; não altera os corpos F2 acima. Ligação causal além do diff: [nao verificado].
- **Consumidores novos relevantes:** `app/chamados/services/desenlace.py:65–82` lê `gravou=False` e estado dos itens; `app/chamados/services/acoes_disputa.py:63–66` passa a devolver a mensagem da porta, inclusive par.

**Leis já respondem às perguntas antigas**

- Desenho 1, 4, 5, 6, 7 e 9, mais F2 L1: **DECIDIDO PELA LEI CORTES-F2-F9-1611 + RESPOSTAS-F2-ESTADOS/C1–C3 | contestavel**. Corte BASE em `app/docs/CORTES.md:487`; leitores conservados em `app/chamados/models.py:3151`, `:1923`, `:3665`, `:3688`, `:3496`; materialização atual em `app/chamados/juizes.py:1390`.
- Desenho 2 e 3, significado de respondida e ilegalidade do par: **DECIDIDO PELA LEI L-130 | contestavel**, `app/docs/LEIS.md:151`; juiz real `app/chamados/juizes.py:1552`. C1/C2 preservam aguarda_admin e tarefa, e mandam o ramo RESPOSTA vazia para B2.
- Desenho 8, **resultado exigido para esta F2**: conservação da B1, decidido pelo aval fornecido; implementação `app/chamados/models.py:1622–1630`; SILENCIO permanece sem ações `app/chamados/services/tarefa_do_dia.py:176–177`. **Lei geral futura “ressuscitar devolve ao colaborador?” continua registrada não decidida** em `app/docs/LEIS.md:151`, `app/chamados/models.py:1625–1626`; não é bloqueio F2.
- F2 L2: **DECIDIDO PELA LEI CORTES-F2-F9-1611 item 2 | contestavel**, `app/docs/CORTES.md:487`: face SQL, zero juiz novo, selo integral por pergunta.
- Dono futuro do ramo último ato RESPOSTA vazia continua pergunta explicitamente aberta no C2 fornecido; situação atual responde colaborador em `app/chamados/services/tarefa_do_dia.py:159–161`. Frota desse ramo: [nao verificado].

## 16/11 23:09 · sessao 5545f6e324a9 · - · - · origem=Ronald (tmux)

Censo estático executado com `python3`/AST e `rg`, sem importar Django nem executar testes. Critério AST: atributos, keywords e strings de campo/lookup; fixtures/testes/migrations separados. Não trate a lista como prova de ausência de chamadas dinâmicas: isso é [nao verificado]. [nao verificado]

**Escritores da projeção R/I/V/autor — os dois do brief não bastam**

| Escritor/porta | O que escreve | NOVO em relação à lista do brief |
|---|---|---|
| `app/chamados/services/disputa_emissao.py:1439` `responder_pergunta_disputa_core` | R/I em `:1481-1483`; limpa V/autor em `:1494-1496`; RESPOSTA em `:1525` | **NOVO** |
| `app/chamados/services/disputa_emissao.py:1576` `reabrir_para_correcao` | quatro campos limpos em `:1609`, snapshot/decisão em `:1631-1646`, escreve pela porta de lote `:1648` | já estava |
| `app/chamados/services/disputa_emissao.py:1654` `responder_automaticamente` | R/I `:1674-1677`, conserva passivo `:1673`, RESPOSTA `:1678` | **NOVO** |
| `app/chamados/services/disputa_emissao.py:1683` `carimbar_resposta_pela_trilha` | I `:1696-1698`, RESPOSTA `:1699-1700` | **NOVO** |
| `app/chamados/services/validacao.py:161` `_carimbar_validacao` | V/autor `:168-171`, VALIDACAO `:173-175` | **NOVO** |
| `app/chamados/services/validacao.py:237` `devolver_validacao_recusada` | limpa V/autor `:273-276`, DEVOLUCAO `:279-282` | já estava, linhas mudaram |
| `app/chamados/services/validacao.py:308` `devolver_paridade_bug_par_103` | limpa V/autor `:349-351`, DEVOLUCAO `:354-358`; K6 antes da escrita `:338-340` | **NOVO** |
| `app/chamados/models.py:1560` `ressuscitar_perguntas` | conserva R/V sem linha `:1613-1614`, limpa par inteiro e V/autor pela porta `:1629-1630` | **NOVO** |
| `app/chamados/models.py:3213` `carimbar` | I condicional `:3242-3243`, V/autor condicionais `:3245-3247`, `save` `:3262` | **NOVO** |
| `app/chamados/models.py:3362` `descarimbar_em_lote` | `update(**outros)` `:3391-3393`, valida composição do par `:3370-3383` | **NOVO**, infraestrutura |
| `app/chamados/models.py:3124` nascimento `_nascer` | recebe campos no construtor, salva `:3127`, conserva linhas R/V `:3129` | **NOVO**, infraestrutura |

Atribuições só em memória devem ser separadas: universo da ressuscitação usa cópia `app/chamados/models.py:1541-1557`; desfazimento do `carimbar` restaura memória `app/chamados/models.py:3269-3276`. A porta `carimbar` escreve R/V no corpo `:3242-3262` sem registrar `DecisaoPergunta` nesse método; portanto a frase do brief “todos mantêm concordância / divergência só no LEGADO” **CAIU como descrição universal do código** (`app/chamados/models.py:3213-3267`). Não há dado para afirmar ocorrência em produção: [nao verificado].

**Chamadores desses escritores/portas — inclui aliases resolvidos pelo AST**

- `responder_pergunta_disputa_core`: `app/api/views.py:2627`; `app/chamados/views.py:1283`; `app/chamados/services/declaracao_texto.py:419,693`; `app/chamados/services/acoes_disputa.py:373`.
- `responder_automaticamente`: `app/api/foto_recebida.py:48`.
- `carimbar_resposta_pela_trilha`: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.
- `reabrir_para_correcao`: `app/chamados/models.py:2318`; `app/chamados/views.py:1436`; `app/chamados/services/acoes_disputa.py:105`; `app/chamados/services/materializacao.py:1164`; `app/chamados/services/validacao.py:836`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.
- `_carimbar_validacao`: `app/chamados/services/validacao.py:432,464,554,655`.
- `devolver_validacao_recusada`: `app/chamados/services/validacao.py:693`.
- `devolver_paridade_bug_par_103`: `app/chamados/management/commands/bug_par_103.py:106`.
- `descarimbar_em_lote`: `app/chamados/models.py:1629`; `app/chamados/services/disputa_emissao.py:1648`.
- `carimbar` da pergunta: `app/chamados/reconciliador.py:406,596`; `app/chamados/models.py:3414`; `app/ponto/services/cartorio.py:1314,1873`; `app/chamados/services/materializacao.py:1043,1944,2139`; `app/chamados/services/disputa_emissao.py:2008,2082`. O AST também acha `app/api/views.py:24` e `app/api/views_core.py:23`, mas esses `carimbar` nominais precisam desambiguação [nao verificado].

**Todos os arquivos de produção encontrados pelo AST com leituras/seleções de R/I/V/autor**

Legenda: referências abaixo incluem strings de auditoria/seleção de campos, além de leituras operativas. Arquivos ausentes da lista fechada do brief são **NOVO**; mesmo nos arquivos conhecidos, o brief não listava todas essas funções. Não confundir com o censo dos quatro templates, que o outro explorer está fazendo.

| Arquivo BASE | Linhas de acesso/lookup/seleção |
|---|---|
| `app/chamados/models.py` | `310,344,1533,1614,1923-1925,2007,2139,2949-2950,3151-3158,3237-3247,3331,3370-3383,3496,3568,3584-3586,3665,3688-3710,4091-4105` |
| `app/chamados/juizes.py` | `352,364,856-857,1360,1389,1423,1534,1541,1552,1572,1580,1744,1753,1779,1926,2030,2035,2052` |
| `app/chamados/views.py` | `1280,1416` |
| `app/chamados/views_cobrar.py` | `169,176,291` |
| `app/chamados/templatetags/chamados_tags.py` | `139,166` |
| `app/chamados/services/fio.py` | `187,377,706,1279,1291,1613` |
| **NOVO** `app/chamados/services/validacao.py` | `173,175,181,262-263,269,281,343,356,380,489,553,594,597,701,810,813,823-824,843,848,975,996,1094-1109,1167` |
| **NOVO** `app/chamados/services/materializacao.py` | `199,201,546,574,625,671,678,718,733,770,869,950,1047,1109,1150-1152,1311,1352,1534,1558,1567,1619,2064-2065,2082` |
| **NOVO** `app/chamados/services/disputa_emissao.py` | `1484-1485,1537-1538,1635-1636,1663,1692,1700,1703,1735,1843,1861,2062,2074,2079,2091` |
| **NOVO** `app/chamados/services/acoes_disputa.py` | `97,132,347,355` |
| **NOVO** `app/chamados/services/becos.py` | `30` |
| **NOVO** `app/chamados/services/declaracao_texto.py` | `227,417,419,687` |
| **NOVO** `app/chamados/services/explicador.py` | `55,102-103` |
| **NOVO** `app/chamados/services/foto_e4b.py` | `180` |
| **NOVO** `app/chamados/services/historico.py` | `99,101,109` |
| **NOVO** `app/chamados/services/painel.py` | `67,262,270-273` |
| **NOVO** `app/chamados/services/passivo_validado.py` | `66` |
| **NOVO** `app/chamados/services/pergunta_no_app.py` | `81` |
| **NOVO** `app/chamados/services/raiox.py` | `138,147` |
| **NOVO** `app/chamados/services/verbos.py` | `264-270,335-345` |
| **NOVO** `app/chamados/services/vigia_carimbo.py` | `98` |
| **NOVO** `app/chamados/services/vigia_fio.py` | `636,746,749` |
| **NOVO** `app/chamados/reconciliador.py` | `365,631,1050,1083` |
| **NOVO** `app/chamados/canal.py` | `65` |
| **NOVO** `app/chamados/veredito_lavrado.py` | `207-208` |
| **NOVO** `app/api/views.py` | `1163-1165,2561-2563,2577,2620,2637` |
| **NOVO** `app/api/views_mensageria.py` | `567,569-570` |
| **NOVO** `app/colaboradores/services/calendario.py` | `504` |
| **NOVO** `app/colaboradores/services/ficha.py` | `171` |
| **NOVO** `app/folha/previa.py` | `142` |
| **NOVO** `app/ponto/services/cartorio.py` | `1641` |
| **NOVO** `app/ponto/services/dia_do_colab.py` | `70` |
| **NOVO** `app/ponto/services/flip_auto.py` | `130` |
| **NOVO** `app/ponto/services/procedencia_batida.py` | `71-74` |
| **NOVO** `app/ponto/services/veredito_celula.py` | `197` |
| **NOVO** `app/ponto/management/commands/reconciliar_grade.py` | `461` |
| **NOVO** `app/chamados/management/commands/auditar_invariantes_chamados.py` | `177,205,592,1075-1076,1105-1106,1576,1604-1605,1657,1768,1780` |
| **NOVO** `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `56,60-61,102,326,504` |
| **NOVO** `app/chamados/management/commands/bug_par_103.py` | `68` |
| **NOVO** `app/chamados/management/commands/curar_perguntas_em_limbo.py` | `32,46,69` |
| **NOVO** `app/chamados/management/commands/dedup_perguntas_fato.py` | `75` |
| **NOVO** `app/chamados/management/commands/encerrar_perguntas_com_fato.py` | `32` |
| **NOVO** `app/chamados/management/commands/reabrir_orfas_enterradas.py` | `68` |
| **NOVO** `app/chamados/management/commands/validar_classe_a.py` | `80` |
| **NOVO** `bin/censo_folha.py` | `28-29` |
| **NOVO** `bin/sombra_regras_pessoais.py` | `85` |

`config/crons.py` só apareceu em comentários para os campos crus, mas os crons podem consumir as funções reais indiretamente; por isso não deve ser inventado escritor ali a partir do grep (`app/config/crons.py:1753-1759`).

**Derivados/portas que o censo precisa acompanhar**

- `estado` segue cru: V tem precedência `app/chamados/models.py:3151-3153`, I classifica pendente/respondida `:3155,:3158`; `respondida` segue apenas I `:3663-3665`; `resposta_display` continua a resolver o texto de R `:3686-3710`. O contrato de F2/antiga F1 não pousou nesses corpos (`app/chamados/models.py:3151,:3665,:3688`).
- `viva` delega a `pergunta_viva` (`app/chamados/models.py:3512`). `q_vivas` lê V cru em `app/chamados/models.py:3496`.
- `resposta_enviada` tem um escritor: `app/chamados/services/fio.py:359`; chama `espera_o_texto_do_colaborador`, que lê I cru e `respondida_sem_texto` (`app/chamados/services/fio.py:177-187`). Outros leitores dessa função: `app/chamados/services/fio.py:174` e `app/chamados/services/cobranca.py:241`.
- `estado_aplicacao` tem escritor de apresentação `app/chamados/services/fio.py:376`; os fallbacks estão em `:377-384`. **NOVO** filtro `aplicacao_da_resposta` faz a mesma consulta à testemunha `app/chamados/templatetags/chamados_tags.py:132-141`; nenhum dos dois é juiz independente.
- `estado_aplicacao_pergunta`: consumidores diretos `app/chamados/services/materializacao.py:339,757`; `app/chamados/services/explicador.py:124`; `app/chamados/templatetags/chamados_tags.py:136`; alias de `models` em `app/chamados/services/fio.py:376`.
- **NOVO** `espera_declarado_x_gravado` filtro `app/chamados/templatetags/chamados_tags.py:121-128` delega à função real; outros consumidores diretos da função: `app/chamados/services/validacao.py:611` e `app/chamados/services/explicador.py:132`.
- `leitura_da_resposta` mudou de `:121` no brief para `app/chamados/templatetags/chamados_tags.py:145`; lê R via `getattr` em `:166` e chama classificação real `:171`. O irmão `frase_resposta` também chama classificação real `:198` (**NOVO no censo**).
- `respostas_anteriores` lê sequência `app/chamados/models.py:3667-3683`; consumidor Python `app/chamados/services/disputa_emissao.py:2123`.
- `todas_respondidas`: escritor `app/chamados/services/fio.py:386-391`.
- Exceção A20: campos declarados `app/chamados/models.py:2563-2564`; único escritor encontrado `app/chamados/services/validacao.py:907-909`; leitores Python `app/chamados/services/validacao.py:905`, `app/chamados/services/materializacao.py:169,1851`.

**Conservação e sequência**

Chamadas de `linhas_da_projecao_sem_linha`, além das duas citadas no brief: `app/chamados/models.py:1613,3129`; `app/chamados/services/validacao.py:167,272,345`; `app/chamados/services/disputa_emissao.py:1480,1673`; **leitor NOVO** `app/chamados/services/tarefa_do_dia.py:430`; commands `app/chamados/management/commands/backfill_decisoes_pergunta.py:67` e `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`. Implementação atual compara `(pergunta, ato, instante)` em `app/chamados/models.py:4091-4105`.

Escritor material das decisões é `DecisaoPergunta.registrar` → `bulk_create`, `app/chamados/models.py:4120`. Outros chamadores além dos acima: `app/chamados/models.py:2005,2137`; `app/chamados/services/materializacao.py:1045`; `app/ponto/services/veredito_celula.py:195`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`. Leitura adicional de sequência **NOVO** em `app/chamados/services/tarefa_do_dia.py:421`, foto/exportação em `app/chamados/services/foto_e4b.py:280`, backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`.

**R2 e REDs — cuidado com fixture**

- R2 ainda pode morder o template porque prefill lê R em `app/templates/chamados/partials/_card_atendimento.html:114,148,154`; mas o estado `R='velho', I=None` agora não pode nascer por `save` normal: `app/chamados/models.py:3574-3590`. Legado intocado é exceção explícita `:3587-3588`. Para RED futuro usar legado caracterizado/objeto em memória, sem burlar a trava: decisão de construção é do responsável; aqui só registro.
- A reabertura real limpa o par em `app/chamados/services/disputa_emissao.py:1609`; a ressuscitação também limpa o par e conserva anteriores sem gerar REABERTURA em `app/chamados/models.py:1615-1630`. Logo “todo legado divergente nasce hoje pela reabertura” seria falso.
- Provas existentes para esse corte: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py::RecusaTextoSemCarimboTest::test_RED_create_com_texto_sem_instante_e_recusado` (`:103`), `::test_MORDE_legado_texto_sem_instante_intocado_segue_e_sair_dele_e_legal` (`:177`), `::test_RED_lote_que_zera_so_o_instante_de_quem_tem_texto_e_recusado` (`:206`); ressuscitação sem REABERTURA `::RessuscitacaoZeraAResposta::test_RED_ressuscitar_zera_o_par_com_o_anterior_na_trilha_sem_REABERTURA` (`:270`).
- BUG-PAR já tem provas de interface novas: `app/chamados/tests/test_bug_par_projetado.py::BugPar163XDeclaradoTest::test_RED_testemunha_que_falha_no_modal_nao_oferece_validar` (`:1155`), `::test_RED_modal_mostra_o_rotulo_e_nao_o_botao_de_validar` (`:1287`), `::test_RED_card_da_pagina_mostra_o_rotulo_e_nao_o_botao_de_validar` (`:1299`). Esses não provam migração de R/I/V para ato.

**Interseção B2 para a F7 e seu grafo ampliado**

Diretos: `app/templates/chamados/partials/_pergunta_card.html:42`, `app/templates/chamados/partials/modal_fio.html:228`, `app/chamados/models.py:3144`, `app/chamados/juizes.py:1389`, `app/chamados/services/fio.py:190`, `app/chamados/views.py:397`, `app/chamados/views_cobrar.py:291`.

Autoridades/escritores do censo ampliado também listados pelo anexo B2: `app/api/views.py:2627`, `app/chamados/canal.py:65`, `app/chamados/reconciliador.py:631`, `app/chamados/services/acoes_disputa.py:105`, `app/chamados/services/becos.py:30`, `app/chamados/services/declaracao_texto.py:419`, `app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/materializacao.py:852`, `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/services/validacao.py:161`, `app/chamados/services/verbos.py:264`, `app/chamados/services/vigia_fio.py:636`, commands de auditoria `:1075`, backfill `:67`, dedup `:75`, `app/colaboradores/services/calendario.py:504`, `app/colaboradores/services/ficha.py:171`, `app/folha/previa.py:142`, `app/ponto/services/cartorio.py:1641`, `app/ponto/management/commands/reconciliar_grade.py:461`. O fato de estarem em voo vem exclusivamente do anexo, não há árvore B2 disponível: [nao verificado].

_6225 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
