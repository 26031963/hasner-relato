# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 17:39** · publicado em **18/11/2026 17:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 17:39 · sessao 8dcf99a94415 · RAIA-INTERVALO · leitura · origem=Code

A sonda abaixo **não foi executada**. A ata está em `CelulaDia.ata`, mas não encontrei um carimbo que registre todas as alterações dela: `veredito_em` não avança quando a ata muda sob decisão administrativa, e `carimbar_precedencia` também altera a ata sem atualizar esse campo. Por isso, a sonda imprime `UA=?` e, pela regra solicitada para dado faltante, `VELHO=?`; não seria correto usar `editada_em`, que carimba edição do dia/DNA ([app/escala/models.py:1732](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1732), [app/escala/models.py:1778](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1778), [app/ponto/portas/celula.py:1060](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1060), [app/ponto/portas/celula.py:1205](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1205)).

`HOJE` é o veredito derivado dos códigos devolvidos pela chamada do **command**, não uma previsão de escrita do cartório: este último pode preservar uma decisão administrativa e tem outros insumos próprios ([app/escala/models.py:1624](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1624), [app/ponto/portas/celula.py:970](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:970), [app/ponto/management/commands/supra_juiz.py:239](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:239)). O leitor da grade lê a ata lavrada; portanto, `HOJE` não prova uma reconstrução independente da ata ([app/escala/services/leitor_celula.py:350](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:350), [app/escala/services/leitor_celula.py:377](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:377)).

A classificação é declarada pura; a transação PostgreSQL `READ ONLY` protege também os carregadores usados pela sonda, recusando escritas ([app/ponto/supra_juiz.py:9](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:9)). A sonda exige o schema ativo `juliani`; o uso em qualquer outro ambiente fica recusado [não verificado].

```python
import datetime as dt
from collections import Counter

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia, EscalaColaborador, derivar_veredito
from escala.services.leitor_celula import grade_da_celula
from ponto.models import Batida
from ponto.turnos import ausencia_cobre, batidas_apuraveis, dias_encerrados
from ponto.supra_juiz import cadastro_do_dia, classificar_dia
from chamados.models import ChamadoColaborador
from chamados.juizes import fio_vivo
from chamados.catalogo.furo_modulos import FURO_MODULES
from chamados.catalogo.modulos import data_do_chamado
from chamados.catalogo.motor import VIVOS

G = {'G1': [(622, d) for d in ('27/10', '29/10', '04/11', '05/11', '12/11', '13/11', '16/11')],
     'G2': [(362, '22/10'), (945, '29/10'), (735, '16/11')],
     'G3': [(583, '12/11'), (829, '11/11'), (369, '12/11')],
     'G4': [(224, '28/10'), (469, '01/11'), (924, '11/11'), (631, '03/11'), (988, '31/10'), (988, '03/11'),
            (836, '04/11'), (1000, '03/11'), (1000, '13/11'), (962, '05/11'), (962, '09/11'), (962, '16/11'),
            (991, '09/11'), (991, '11/11')],
     'G5': [(787, '30/10')]}

erros = Counter()
tz = timezone.get_current_timezone()


def executar_etapa(etapa, funcao):
    # Savepoint permite continuar mesmo apos erro de banco; nao imprime
    # mensagens de excecao, que poderiam conter dados pessoais.
    try:
        with transaction.atomic():
            return True, funcao()
    except Exception as exc:
        erros[(etapa, type(exc).__name__)] += 1
        return False, None


def hora_local(valor):
    if valor is None:
        return '?'
    return timezone.localtime(valor, tz).strftime('%d/%m/%Y_%H:%M:%S')


def classificar_como_command(c, data):
    # Mesmo caminho e argumentos de:
    # ponto/management/commands/supra_juiz.py:102-116,120-156,173-178,
    # 186-202 e 205-234. A janela pedida aqui e um unico dia.
    ini = fim = data
    janela_ini = timezone.make_aware(
        dt.datetime.combine(ini - dt.timedelta(days=1), dt.time.min)
    )
    janela_fim = timezone.make_aware(
        dt.datetime.combine(fim + dt.timedelta(days=3), dt.time.min)
    )
    bs = list(Batida.objects.filter(
        colaborador=c,
        timestamp__gte=janela_ini,
        timestamp__lt=janela_fim,
    ).order_by('timestamp'))
    if not bs:
        # O command pula este colaborador nesta janela.
        raise ValueError('command_sem_batidas_na_janela')

    encerrados = dias_encerrados(c, ini, fim)
    grade = grade_da_celula(c, ini, fim)
    primeira = Batida.objects.filter(
        colaborador=c
    ).order_by('timestamp').first()
    adesao = timezone.localdate(primeira.timestamp) if primeira else None

    vigs = list(EscalaColaborador.objects.filter(
        colaborador=c
    ).select_related('tipo_escala').order_by('data_inicio'))

    def sem_fase(dref):
        ec = None
        for v in vigs:
            if v.data_inicio and v.data_inicio <= dref:
                ec = v
        if ec is None or ec.tipo_escala is None:
            return False
        if ec.tipo_escala.tipo_ciclo not in ('12x36', '24x48'):
            return False
        a = ec.data_ancora_colaborador
        return a is None or a.year <= 1940

    cadastro = cadastro_do_dia(
        c.pk,
        list(EscalaColaborador.objects.filter(
            colaborador_id=c.pk
        ).select_related('tipo_escala')),
        {(c.pk, x.data): x for x in CelulaDia.objects.filter(
            colaborador_id=c.pk, data__range=(ini, fim)
        )},
        data_demissao=c.data_demissao,
    )
    regua_deg = ChamadoColaborador.objects.filter(
        colaborador=c,
        modulo_origem='regua_degenerada',
        status_local__in=VIVOS,
    ).exists()

    chs = {}
    for ch in (ChamadoColaborador.objects.filter(
            colaborador=c, modulo_origem__in=FURO_MODULES
    ).prefetch_related('perguntas_que_resolvem')):
        d = data_do_chamado(ch.modulo_origem, ch.contexto_json)
        if d:
            chs.setdefault(str(d), []).append(ch)

    dias = [
        x for x in (grade.get('dias') or [])
        if x.get('data') == data and ini <= x.get('data') <= fim
    ]
    if len(dias) != 1:
        raise ValueError('grade_nao_devolveu_um_dia')

    dia = dias[0]
    lista = chs.get(data.isoformat(), [])
    vivos = [x for x in lista if x.status_local in VIVOS]
    com_fio = [
        x for x in vivos if fio_vivo(x.perguntas_que_resolvem.all())
    ]
    cob = ausencia_cobre(c, data, para='cobranca')
    n_bat = sum(1 for b in bs if timezone.localdate(b.timestamp) == data)

    resultado = classificar_dia(
        dia, dia.get('marcos') or [],
        chamados_vivos=len(vivos),
        chamados_total=len(lista),
        cobrancas_vivas=len(com_fio),
        cobertura_status=getattr(cob, 'status', None),
        data=data,
        em_operacao_desde=adesao,
        isento_ponto=bool(c.isento_ponto),
        batidas_no_dia=n_bat,
        hoje=timezone.localdate(),
        turno_encerrado=encerrados.get(data, False),
        sem_fase=sem_fase(data),
        regua_degenerada=regua_deg,
        **cadastro(data)
    )
    codigos = [cod for cod, detalhe in resultado]
    veredito = derivar_veredito(codigos)

    # Os dois retornos de classificar_dia sao o retorno antecipado de
    # isento e o retorno final. Nao se afirma qual predicado interno
    # deixou o resultado vazio.
    ramo = None
    if veredito == 'concorde':
        ramo = 'ISENTO_PONTO' if bool(c.isento_ponto) else 'RETORNO_FINAL'
    return veredito, codigos, ramo


def ultima_batida(c, data):
    ini = timezone.make_aware(dt.datetime.combine(data, dt.time.min), tz)
    fim = timezone.make_aware(
        dt.datetime.combine(data + dt.timedelta(days=1), dt.time.min), tz
    )
    # batidas_apuraveis usa fim inclusivo; exclui-se a meia-noite seguinte.
    bats = [b for b in batidas_apuraveis(c, ini, fim) if b.timestamp < fim]
    if not bats:
        return None
    # "Ultima batida" pela ordem de timestamp da autoridade;
    # pk desempata marcas com o mesmo timestamp.
    return max(bats, key=lambda b: (b.timestamp, b.pk)).criado_em


def rodar():
    resumo = {
        grupo: Counter({'sim': 0, 'nao': 0, '?': 0,
                        'acusa': 0, 'concorde': 0, 'hoje_?': 0})
        for grupo in G
    }

    for grupo, casos in G.items():
        for pk, dm in casos:
            dd, mm = dm.split('/')
            data = dt.date(2026, int(mm), int(dd))
            grav = julg = hoje = codigos = ub = ramo = None

            ok_col, col = executar_etapa(
                'colaborador', lambda: Colaborador.objects.get(pk=pk)
            )
            if ok_col:
                ok_cel, cel = executar_etapa(
                    'celula',
                    lambda: CelulaDia.objects.filter(
                        colaborador_id=pk, data=data
                    ).first()
                )
                if ok_cel and cel is not None:
                    grav = cel.veredito
                    julg = cel.julgada_em

                ok_hoje, atual = executar_etapa(
                    'classificacao', lambda: classificar_como_command(col, data)
                )
                if ok_hoje:
                    hoje, codigos, ramo = atual

                ok_ub, ub = executar_etapa(
                    'batida', lambda: ultima_batida(col, data)
                )

            # Nao ha carimbo demonstrado para todas as alteracoes de ata.
            ua = None
            velho = '?'

            r = resumo[grupo]
            r[velho] += 1
            if hoje is None:
                r['hoje_?'] += 1
            elif hoje == 'concorde':
                r['concorde'] += 1
            else:
                r['acusa'] += 1

            cod_txt = '?' if codigos is None else (
                ','.join(codigos) if codigos else '-'
            )
            linha = (
                'D %s col%d %s GRAV=%s JULG=%s HOJE=%s COD=%s '
                'UB=%s UA=%s VELHO=%s'
                % (grupo, pk, dm, grav or '?', hora_local(julg),
                   hoje or '?', cod_txt, hora_local(ub),
                   hora_local(ua), velho)
            )
            if hoje == 'concorde':
                linha += ' RAMO %s' % (ramo or '?')
            print(linha)

    for grupo in G:
        r = resumo[grupo]
        print(
            'RESUMO %s VELHO sim=%d nao=%d ?=%d '
            'HOJE acusa=%d concorde=%d ?=%d'
            % (grupo, r['sim'], r['nao'], r['?'],
               r['acusa'], r['concorde'], r['hoje_?'])
        )


try:
    if connection.in_atomic_block:
        raise RuntimeError('transacao_preexistente')
    if connection.vendor != 'postgresql':
        raise RuntimeError('banco_nao_postgresql')

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute('SET TRANSACTION READ ONLY')
            cursor.execute('SELECT current_schema()')
            if cursor.fetchone()[0] != 'juliani':
                raise RuntimeError('schema_ativo_nao_e_juliani')
        rodar()
except Exception as exc:
    erros[('sonda', type(exc).__name__)] += 1

total_erros = sum(erros.values())
print('ERROS %d' % total_erros)
for (etapa, tipo), quantidade in sorted(erros.items()):
    print('ERROS etapa=%s tipo=%s n=%d' % (etapa, tipo, quantidade))
print('FIM_SONDA_VELHO')
```

As referências principais da sonda são:

- `CelulaDia.veredito`, `veredito_via`, `veredito_em`, `protestos` e `ata`: [app/escala/models.py:1769](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1769). `julgada_em`: [app/escala/models.py:1738](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1738).
- Montagem dos argumentos do command: [app/ponto/management/commands/supra_juiz.py:102](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:102), [linha 120](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:120), [linha 142](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:142), [linha 173](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:173), [linha 186](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:186) e [linha 205](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:205).
- `classificar_dia` e seus retornos: [app/ponto/supra_juiz.py:217](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:217), [linha 233](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:233) e [linha 549](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:549). `cadastro_do_dia`: [linha 155](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:155).
- `batidas_apuraveis`, inclusive ordenação e limite superior inclusivo: [app/ponto/turnos.py:2868](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2868); `Batida.criado_em`: [app/ponto/models.py:79](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/models.py:79). `dias_encerrados` e `ausencia_cobre`: [app/ponto/turnos.py:2133](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2133), [linha 2550](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2550).
- `fio_vivo`, `data_do_chamado` e `FURO_MODULES`: [app/chamados/juizes.py:1510](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:1510), [app/chamados/catalogo/modulos.py:219](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/catalogo/modulos.py:219), [app/chamados/catalogo/furo_modulos.py:15](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/catalogo/furo_modulos.py:15).

## 18/11 17:38 · sessao 328937483a19 · E4C-F6F7 · leitura · origem=Code

A sonda abaixo é uma **proposta não executada**. A compatibilidade com `c8b6d1ec2`, a presença da coluna no banco sombra, os dados e o resultado do diff estão **[não verificados]**: esta cópia não permite conferir o diff pedido contra o main. O script registra incompatibilidades por etapa, inclusive `TypeError`; uma execução com `ERROS > 0` não constitui medida completa.

A 0056 usa o predicado `ato='resposta'` e `exclude(ato_motivo='')` ([migração 0056:12](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12)). A sonda usa SQL para reproduzi-lo porque o manager do modelo atual proíbe `update` ([models.py:4121](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4121), [models.py:4197](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4197)).

O universo `CD` é o solicitado: situação `ativo` **ou** alguma batida com data civil local na janela da empresa, cruzado com todos os dias dessa janela ([models.py:22](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/ponto/models.py:22), [colaboradores/models.py:323](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/colaboradores/models.py:323)). Ele é mais amplo que o universo do vigia: `dias_que_seguram` seleciona retidos por `furo_espelho` e seus dias acusadores ([fora_da_folha.py:125](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:125), [vigia_fio.py:530](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/vigia_fio.py:530)). `G` é o grupo real **da pessoa**, repetido em seus dias `CD`; `-` significa que ela não aparece como retida com grupo ([fora_da_folha.py:94](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:94), [fora_da_folha.py:210](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:210)).

`PQ` cobre todas as perguntas visíveis com **linhas consecutivas** `VALIDACAO`, `RESPOSTA` com `mantem=True`, inclusive fora da competência; a ordem é `(ato_em, pk)` ([models.py:4199](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4199)). Uma pergunta sem dia recebe `T=ERRO P=ERRO` e conta como erro. O instante fixo deve ser igual nas duas rodadas: a tarefa corta decisões por ele ([tarefa_do_dia.py:484](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:484)). Rode as duas medições sobre a mesma fotografia do banco e no mesmo dia civil: `classificar_export` também lê o relógio próprio ([fora_da_folha.py:137](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:137)). Igualdade da fotografia e condições do banco sombra: **[não verificado]**.

```python
# Executar SOMENTE no banco sombra, nas duas versões, com o mesmo AGORA.
# Exemplo no manage.py shell:
#     exec(open('/caminho/sonda.py').read())
#
# A sonda não verifica que o banco é sombra. Essa identificação é
# responsabilidade de quem executa. Não imprime nomes, CPF ou mensagens
# de exceção.

import calendar
import datetime
from collections import defaultdict

from django.db import connection, transaction
from django.db.models import Q
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador, Empresa
from ponto.models import Batida
from chamados.models import DecisaoPergunta, PerguntaDisputa
from chamados.services.tarefa_do_dia import carga_da_tarefa, porta_da_tarefa
from folha.fora_da_folha import fora_da_folha
from ponto.janelas import janela_fechamento


# Mesmo instante nas duas rodadas. Alterar somente antes de AMBAS as rodadas.
AGORA = datetime.datetime(
    2026, 11, 18, 20, 30, 0, tzinfo=datetime.timezone.utc
)
MES, ANO = 11, 2026

erros = []


class RollbackSonda(Exception):
    pass


class EscritaNaoPermitida(Exception):
    pass


def erro(etapa, exc):
    # Não imprime str(exc): mensagens podem conter dados pessoais.
    erros.append((etapa, type(exc).__name__))


def tentar(etapa, funcao):
    # O savepoint permite continuar após um erro SQL sem deixar a
    # transação externa quebrada. O resultado mal sucedido é None.
    try:
        with transaction.atomic():
            return True, funcao()
    except Exception as exc:
        erro(etapa, exc)
        return False, None


def canon(valor):
    """Representação determinística sem serializar objetos desconhecidos."""
    if valor is None or isinstance(valor, (bool, int, float, str)):
        return valor
    if isinstance(valor, (datetime.date, datetime.time)):
        return valor.isoformat()
    if isinstance(valor, dict):
        return tuple(
            (canon(k), canon(v))
            for k, v in sorted(valor.items(), key=lambda item: repr(item[0]))
        )
    if isinstance(valor, (list, tuple)):
        return tuple(canon(v) for v in valor)
    if isinstance(valor, (set, frozenset)):
        return tuple(sorted((canon(v) for v in valor), key=repr))
    raise TypeError("tipo não suportado na assinatura")


def assinatura_tarefa(t):
    # Não inclui agora nem celula_id. Mantém a ordem das instâncias e ações,
    # pois ela pode fazer parte da decisão publicada. [nao verificado]
    campos_dia = (
        "nao_sei", "razao", "rota", "fila", "verbos", "fio",
        "acao", "ausencia",
    )
    campos_instancia = (
        "instancia", "tipo", "hora", "data_civil", "lampada",
        "vencido", "motivo", "pergunta_id", "razao", "acoes",
    )
    return canon({
        "dia": {k: t.get(k) for k in campos_dia},
        "instancias": [
            {k: inst.get(k) for k in campos_instancia}
            for inst in t.get("instancias", ())
        ],
    })


def assinatura_porta(p):
    return canon(p)


def meses_dia(dia):
    ini = dia.replace(day=1)
    fim = dia.replace(day=calendar.monthrange(dia.year, dia.month)[1])
    return ini, fim


def dias(ini, fim):
    for n in range((fim - ini).days + 1):
        yield ini + datetime.timedelta(days=n)


# Bloqueia comandos SQL que possam escrever durante as leituras.
# O único UPDATE autorizado é executado com a chave temporariamente ligada.
permitir_update_0056 = False


def guarda_sql(execute, sql, params, many, context):
    primeiro = sql.lstrip().split(None, 1)[0].upper() if sql.strip() else ""
    permitidos = {
        "SELECT", "SHOW", "SAVEPOINT", "RELEASE", "ROLLBACK",
    }
    if primeiro == "UPDATE" and permitir_update_0056:
        return execute(sql, params, many, context)
    if primeiro not in permitidos:
        raise EscritaNaoPermitida()
    return execute(sql, params, many, context)


def executar():
    global permitir_update_0056

    print("AGORA", AGORA.isoformat())
    print("DIA_CIVIL_EXECUCAO", timezone.localdate().isoformat())
    print("DIA_CIVIL_AGORA", timezone.localdate(AGORA).isoformat())

    tabela = connection.ops.quote_name(DecisaoPergunta._meta.db_table)
    coluna_ato = connection.ops.quote_name("ato")
    coluna_motivo = connection.ops.quote_name("ato_motivo")
    coluna_mantem = connection.ops.quote_name("resposta_mantem_validacao")

    # ato_motivo é NOT NULL no modelo desta cópia. Este SQL reproduz
    # filter(ato='resposta').exclude(ato_motivo='') da 0056.
    predicado = "%s = %%s AND %s <> %%s" % (coluna_ato, coluna_motivo)

    with connection.execute_wrapper(guarda_sql):
        with connection.cursor() as cursor:
            cursor.execute(
                "SELECT COUNT(*) FROM %s WHERE %s" % (tabela, predicado),
                ["resposta", ""],
            )
            print("PRE_0056", cursor.fetchone()[0])

            permitir_update_0056 = True
            try:
                cursor.execute(
                    "UPDATE %s SET %s = %%s WHERE %s"
                    % (tabela, coluna_mantem, predicado),
                    [True, "resposta", ""],
                )
            finally:
                permitir_update_0056 = False

        hoje = timezone.localdate(AGORA)

        ok, empresas = tentar(
            "empresas",
            lambda: list(Empresa.objects.all().order_by("pk")),
        )
        if not ok:
            return

        janelas = {}
        grupos = {}
        pares_cd = set()
        colabs = {}

        for empresa in empresas:
            eid = empresa.pk
            ok, janela = tentar(
                "janela",
                lambda e=empresa: janela_fechamento(MES, ANO, e),
            )
            if not ok:
                continue
            ini, fim = janela
            janelas[eid] = (ini, fim)
            print(
                "EMPRESA", eid,
                "CORTE", empresa.dia_inicio_competencia,
                "INI", ini.isoformat(),
                "FIM", fim.isoformat(),
            )

            # __date usa a zona ativa do Django para o timestamp.
            def carregar_colabs():
                ids_batida = Batida.objects.filter(
                    colaborador__empresa_id=eid,
                    timestamp__date__gte=ini,
                    timestamp__date__lte=fim,
                ).values("colaborador_id")
                return list(
                    Colaborador.objects.filter(empresa_id=eid)
                    .filter(Q(situacao="ativo") | Q(pk__in=ids_batida))
                    .select_related("empresa")
                    .order_by("pk")
                )

            ok, lista = tentar("universo_cd", carregar_colabs)
            if ok:
                for c in lista:
                    colabs[c.pk] = c
                    for dia in dias(ini, fim):
                        pares_cd.add((c.pk, dia))

            # Chama a função real que calcula o grupo da pessoa.
            ok, resultado = tentar(
                "fora_da_folha",
                lambda e=empresa: fora_da_folha(
                    e, MES, ANO, agora=AGORA
                ),
            )
            if ok:
                for linha in resultado["colabs"]:
                    grupos[linha["colab"]] = linha["grupo"] or "-"
            else:
                # Falha da classificação não pode parecer ausência de grupo.
                grupos[eid, "erro_empresa"] = True

        # Ler a coluna diretamente também permite detectar sua ausência no
        # banco sem exigir que o modelo Python do main a declare.
        def pares_validacao_resposta():
            sql = (
                "SELECT pergunta_id, ato, %s "
                "FROM %s ORDER BY pergunta_id, ato_em, id"
                % (coluna_mantem, tabela)
            )
            encontrados = set()
            anterior_pergunta = None
            anterior_ato = None
            with connection.cursor() as cursor:
                cursor.execute(sql)
                while True:
                    bloco = cursor.fetchmany(2000)
                    if not bloco:
                        break
                    for pergunta_id, ato, mantem in bloco:
                        if pergunta_id != anterior_pergunta:
                            anterior_ato = None
                        if (
                            anterior_ato == "validacao"
                            and ato == "resposta"
                            and mantem is True
                        ):
                            encontrados.add(pergunta_id)
                        anterior_pergunta = pergunta_id
                        anterior_ato = ato
            return encontrados

        ok, ids_perguntas = tentar("sequencia_pq", pares_validacao_resposta)
        if not ok:
            ids_perguntas = set()

        def carregar_perguntas():
            return list(
                PerguntaDisputa.objects.filter(pk__in=ids_perguntas)
                .select_related("disputa__colaborador__empresa")
                .order_by("pk")
            )

        ok, perguntas = tentar("perguntas_pq", carregar_perguntas)
        if not ok:
            perguntas = []

        pares_pq = []
        for p in perguntas:
            c = p.disputa.colaborador
            colabs[c.pk] = c
            pares_pq.append((p.pk, c.pk, p.data_evento, c.empresa_id))

        # Uma carga por empresa/janela. O grupo da competência inclui todos
        # os pares CD e as perguntas PQ que caem nela. Perguntas de outros
        # períodos são agrupadas por mês civil.
        lotes = defaultdict(set)
        for cid, dia in pares_cd:
            c = colabs[cid]
            ini, fim = janelas[c.empresa_id]
            lotes[(c.empresa_id, ini, fim)].add(cid)

        for pk, cid, dia, eid in pares_pq:
            if dia is None:
                erro("pq_sem_dia", ValueError())
                continue
            janela = janelas.get(eid)
            if janela and janela[0] <= dia <= janela[1]:
                ini, fim = janela
            else:
                ini, fim = meses_dia(dia)
            lotes[(eid, ini, fim)].add(cid)

        resultados = {}

        for (eid, ini, fim), ids in sorted(lotes.items()):
            lista = [colabs[cid] for cid in sorted(ids)]
            ok, carga = tentar(
                "carga_da_tarefa",
                lambda cs=lista, a=ini, b=fim: carga_da_tarefa(
                    cs, a, b, AGORA
                ),
            )
            if not ok:
                continue

            necessarios = {
                (cid, dia)
                for cid, dia in pares_cd
                if cid in ids and colabs[cid].empresa_id == eid
                and ini <= dia <= fim
            }
            necessarios.update(
                (cid, dia)
                for _, cid, dia, peid in pares_pq
                if dia is not None and peid == eid
                and cid in ids and ini <= dia <= fim
            )

            for cid, dia in sorted(necessarios):
                chave = (cid, dia)
                if chave in resultados:
                    continue

                ok, tarefa = tentar(
                    "tarefa",
                    lambda c=colabs[cid], d=dia: carga.tarefa(c, d),
                )
                if not ok:
                    resultados[chave] = ("ERRO", "ERRO")
                    continue

                ok_t, sig_t = tentar(
                    "assinatura_tarefa",
                    lambda t=tarefa: assinatura_tarefa(t),
                )
                ok_p, porta = tentar(
                    "porta_da_tarefa",
                    lambda t=tarefa: porta_da_tarefa(t),
                )
                if ok_p:
                    ok_p, sig_p = tentar(
                        "assinatura_porta",
                        lambda p=porta: assinatura_porta(p),
                    )
                resultados[chave] = (
                    repr(sig_t) if ok_t else "ERRO",
                    repr(sig_p) if ok_p else "ERRO",
                )

        for cid, dia in sorted(pares_cd):
            c = colabs[cid]
            if grupos.get((c.empresa_id, "erro_empresa")):
                grupo = "ERRO"
            else:
                grupo = grupos.get(cid, "-")
            t, p = resultados.get((cid, dia), ("ERRO", "ERRO"))
            print(
                "CD col%d %s T=%s P=%s G=%s"
                % (cid, dia.isoformat(), t, p, grupo)
            )

        for pk, cid, dia, _eid in sorted(pares_pq):
            if dia is None:
                data = "-"
                t, p = "ERRO", "ERRO"
            else:
                data = dia.isoformat()
                t, p = resultados.get((cid, dia), ("ERRO", "ERRO"))
            print("PQ %s col%d %s T=%s P=%s" % (pk, cid, data, t, p))


try:
    with schema_context("juliani"):
        try:
            with transaction.atomic():
                try:
                    executar()
                except Exception as exc:
                    erro("execucao", exc)
                # Sempre sair do atomic por exceção: desfaz a 0056 simulada.
                raise RollbackSonda()
        except RollbackSonda:
            pass
except Exception as exc:
    erro("schema_ou_rollback", exc)
finally:
    print("ERROS", len(erros))
    for etapa, tipo in erros[:20]:
        print("ERRO", etapa, tipo)
    print("FIM_SONDA_DIFF_RETIDO")
```

Funções reais chamadas diretamente pela sonda nesta cópia:

- `janela_fechamento`: [janelas.py:54](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/ponto/janelas.py:54).
- `fora_da_folha`: [fora_da_folha.py:177](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:177); ela chama `dias_que_seguram` e calcula o grupo por `_grupo` ([fora_da_folha.py:125](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:125), [fora_da_folha.py:94](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:94)).
- `carga_da_tarefa` e `CargaDaTarefa.tarefa`: [tarefa_do_dia.py:795](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:795), [tarefa_do_dia.py:632](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:632). É o mesmo caminho em lote usado pelo vigia ([vigia_fio.py:591](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/vigia_fio.py:591)); a casca `tarefa_do_dia` também delega a `carga.tarefa` ([tarefa_do_dia.py:800](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:800)).
- `porta_da_tarefa`: [tarefa_do_dia.py:779](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:779).

Existência e assinatura dessas funções no main `c8b6d1ec2`: **[não verificado]**. Não é possível afirmar, com esta cópia e o anexo fornecido, que alguma delas exista somente em um dos lados.

## 18/11 17:37 · sessao d471b06a0eb8 · DECLARADO-X-GRAVADO · construtor · origem=Code

**Parei sem implementar a cura:** não identifiquei uma porta existente que devolva ao colaborador, com a frase exigida, uma resposta já validada nos dois ramos abrangidos. A materialização é chamada após o carimbo de validação. [validacao.py:743](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:743), [materializacao.py:1923](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/materializacao.py:1923)

A porta `recusar_pergunta` devolve a pergunta ao colaborador, mas recusa perguntas já validadas. A outra porta, `devolver_validacao_recusada`, devolve a resposta ao **admin**, preserva a resposta e se restringe ao caso de resposta de hora; ela não realiza a devolução pedida ao colaborador para o ramo `data_hora`. [validacao.py:898](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:898), [validacao.py:237](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:237)

A porta de reabertura limpa a resposta e registra o motivo na trilha, mas a representação da pergunta na API examinada não inclui esse motivo. A recusa existente envia um push genérico; a entrega da frase exigida ao colaborador por essa via está **[não verificado]**. [disputa_emissao.py:1584](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/disputa_emissao.py:1584), [api/views.py:2549](/home/ronald/wt-codex-declarado-x-gravado-173308/app/api/views.py:2549), [validacao.py:943](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:943) [nao verificado]

Falta, portanto, uma porta chamável que trate a devolução de resposta **já validada**, para `hora` e `data_hora`, e disponibilize a frase ao colaborador. Sua localização e implementação exigidas estão **[não verificado]**. A frase literal também não consta do dado fornecido **[não verificado]**.

Nenhum arquivo foi alterado; R1–R10, mutantes, compilação e testes não foram executados. A contagem de palavras de band-aid no diff está **[não verificado]**. A entrega e a linha LEI-AKITA solicitadas permanecem pendentes; não apresento os REDs como existentes. [nao verificado]

## 18/11 17:36 · sessao 7a0a1b4d842c · REPROVAS-1811-1740 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: PROCEDE.** O item 2 segue com as reservas abaixo. [não verificado: hash do HEAD por Git; a cópia disponível não tem metadados Git.]

### 1. E4C-F6F7 — PROCEDE

1. **Premissas: conferem parcialmente.** A linha 164 do main devolve a tarefa ao colaborador em caso de recusa ou reabertura; ela não demonstra o laço atribuído à ponta F6F7 ([tarefa_do_dia.py:142](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/tarefa_do_dia.py:142)). A cadeia de leitura existe: `fora_da_folha` lê a porta, deriva o fio e agrupa a pessoa pelos fios dos dias; o retrato consome essa saída ([fora_da_folha.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:70), [fora_da_folha.py:94](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:94), [fora_da_folha.py:265](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:265), [retrato_folha.py:1212](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1212)). Cadastro, vigia e pautas também são leitores, direta ou indiretamente ([cadastro_realidade.py:472](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/cadastro_realidade.py:472), [vigia_fio.py:594](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/vigia_fio.py:594), [pautas_dos_discordantes.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/pautas_dos_discordantes.py:70)).

   **Há vigia de produção onde um contador pode ser integrado:** `vigiar_fio_morto` chama contadores, registra seus valores e esperados, e está encadeado na rotina ([vigiar_fio_morto.py:100](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:100), [vigiar_fio_morto.py:179](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:179), [vigiar_fio_morto.py:244](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:244), [crons.py:1737](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/config/crons.py:1737)). O contador nomeado da F7 e seu caso que morde na ponta: **[não verificado]**.

2. **Colide com o aval vigente.** A autorização de pousar na véspera se o diff der zero conflita com a vedação fornecida de pouso que toque folha ou retenção entre 18/11 às 18h e o fim da simulação, pois a cadeia de leitores alcança folha e retrato ([fora_da_folha.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:70), [retrato_folha.py:1212](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1212)). O zero medido é uma condição adicional do aval anexo; não elimina literalmente a vedação fornecida. A precedência F6F7 antes da F9 e o compact continuam condições informadas; seu cumprimento **[não verificado]**.

3. **O literal não fecha ainda.** Para corte 21, competência 11/2026 compreende **21/10/2026 a 20/11/2026**, inclusive; outros cortes da empresa produzem outros intervalos ([janelas.py:10](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/janelas.py:10), [janelas.py:36](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/janelas.py:36)). O diff existente é de leitura e percorre colaboradores e dias, mas compara C17 com a tarefa da própria cópia; suas projeções não executam os censos e sua saída é agregada e anonimizada. Ele não é a porta pronta para executar **main versus ponta curada** nem entregar a tabela de mudança de grupo e lista nominal pedidas ([diff_e3_sombra.py:1](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:1), [diff_e3_sombra.py:46](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:46), [diff_e3_sombra.py:127](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:127)). Outra porta capaz desse confronto **[não verificado]**.

4. **Exportada, dinheiro ou `!`:** há alcance sobre a apresentação do retido e o retrato lavrado, já reconhecido na retificação do aval; o cron de retrato usa `--apply` ([retrato_folha.py:1207](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1207), [crons.py:1650](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/config/crons.py:1650)). Alteração de competência exportada, dinheiro ou outro `!` causado pela ponta **[não verificado]**.

### 2. ESCALA-NAO-DESCREVE — CONFERE, com alcance limitado

1. **Premissas:** o veredito fornecido relata selo sem mordida e ausência de log do mutante; a cópia da obra e `test_escala_nao_descreve.py:61-62` **[não verificado]**. Também **[não verificado]** se a cura já feita mata o mutante, se o log existe e se os commits citados contêm a linha exigida. [nao verificado]

2. **Colisão:** o pouso indicado para depois da simulação é compatível com a janela vigente fornecida; o rito, o compact e a ordem eventualmente aplicável continuam pendentes de comprovação **[não verificado]**. [nao verificado]

3. **Literal:** o aval exige log do mutante nomeado e linha `LEI-AKITA` com `padrao=`; o fechamento dessas condições **[não verificado]**. O aceite para os pontos do veredito classificados como fila também **[não verificado]**.

4. **Exportada, dinheiro ou `!`:** o dado fornecido declara que a porta de vínculo regenera células e reconcilia, mas deixa **não verificado** o efeito sobre competência entregue; o código presente confirma a chamada de reconciliação ([vinculo.py:346](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/colaboradores/services/vinculo.py:346)). Efeito sobre exportada, dinheiro ou outro `!` **[não verificado]**.

## 18/11 17:33 · sessao ae999d6c0dcd · E4C-F6F7 · construtor · origem=Code

A cura liga o `handle` ao contador uma vez e publica as quatro chaves devolvidas por ele, antes da bifurcação de `--aplicar`, sem nova escrita ([backfill_decisoes_pergunta.py:595](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:595)).

O teste integrado cria uma pergunta validada sem linha de validação e confere a saída do comando ([test_e4c_f6f7_c11_backfill.py:60](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:60)). **RED e GREEN integrados: [não verificado]**, pois precisam de banco e não foram executados. Comando para quem certifica: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f6f7_c11_backfill.BackfillPublicaContadorF7
```

A verificação sem banco foi executada antes da cura e falhou com `AssertionError: RED: handle nao chama o contador`; depois, passou com `GREEN: costura sem banco`. Ela verifica a chamada, a posição anterior ao ramo `--aplicar` e o uso das quatro chaves com valores sentinela ([test_e4c_f6f7_c11_backfill.py:9](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:9)). Comando GREEN: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py
```

Os cinco mutantes em memória morreram: M1 remove chamada e linha; M2–M5 fixam, respectivamente, número, esperado, dono e nome ([test_e4c_f6f7_c11_backfill.py:13](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:13)). Cada comando deve falhar por asserção:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M1
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M2
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M3
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M4
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M5
```

`py_compile` passou nos dois arquivos, com a escrita de bytecode desativada: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import py_compile
py_compile._write_atomic = lambda *args, **kwargs: None
for p in ('chamados/management/commands/backfill_decisoes_pergunta.py',
          'chamados/tests/test_e4c_f6f7_c11_backfill.py'):
    py_compile.compile(p, cfile=p + '.pyc', doraise=True)
    print('py_compile OK:', p)
PY
```

LEI-AKITA: origem=[backfill_decisoes_pergunta.py::handle](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:595), testemunha=[contador_validada_projetada_sem_linha](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:94) (lê `DecisaoPergunta.linhas_da_projecao_sem_linha`), RED=[teste integrado](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:61) [não verificado]; verificação sem banco RED executada, quem-mais-le=[teste F7](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_f7_contador_validada_sem_linha.py:31) e [teste sem banco C7](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:51), juizes novos=0, padrao=[Parallel Change / Expand-Contract](/home/ronald/wt-codex-e4c-f6f7-132608/app/docs/GLOSSARIO.md:18). [nao verificado]

## 18/11 17:33 · sessao 681f0c12c82c · E4C-F6F7 · leitura · origem=Code

A sonda abaixo mede **pares candidato de colaborador e dia que também são dias de retenção**, além de colaboradores distintos nessa interseção. Ela não calcula o número de pessoas que efetivamente mudariam de grupo A–E. Na cópia, o grupo depende dos fios de **todos** os dias retidos, e a seleção da porta depende de mais condições além da sequência de atos. [app/folha/fora_da_folha.py:94](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:94), [app/folha/fora_da_folha.py:210](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:210), [app/chamados/services/tarefa_do_dia.py:694](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:694)

A cópia não tem metadados Git acessíveis; por isso, **não foi possível confirmar nenhuma das funções no main pai**, nem afirmar que alguma esteja ausente nele. [não verificado] Na cópia, as dependências de domínio usadas são `es_da_ressuscitacao`, `empresas_visiveis`, `competencias_a_lavrar` e `dias_que_seguram`. [app/chamados/models.py:4221](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/models.py:4221), [app/colaboradores/queries.py:78](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/colaboradores/queries.py:78), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209), [app/folha/fora_da_folha.py:125](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:125)

O predicado da 0056 é `ato='resposta'` e `ato_motivo` diferente de vazio; a simulação também preserva `True` já existente nas demais respostas. [app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12) A sequência usa o horizonte, a ordem e a exclusão de ressuscitação da carga; o vínculo ao dia usa `conversa=False`, `colaborador_id` e `data_evento`. [app/chamados/services/tarefa_do_dia.py:393](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:393), [app/chamados/services/tarefa_do_dia.py:506](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:506)

O universo usa a função real `dias_que_seguram`, chamada por `fora_da_folha` e pelo vigia; o recorte de empresas e competências segue o vigia. A função de competências inclui a atual e, quando não entregue, a anterior: isso não demonstra cobertura de todas as competências históricas abertas. [app/folha/fora_da_folha.py:200](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:200), [app/chamados/services/vigia_fio.py:529](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/vigia_fio.py:529), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209)

Execute o texto pelo shell de `tenant_command` indicado, por exemplo redirecionando o arquivo de entrada. A sonda não foi executada aqui. [não verificado] Ela bloqueia comandos SQL que não comecem por `SELECT`, não imprime mensagens de exceção que possam conter dados pessoais e invalida os números se `ERROS > 0`; exceções eventualmente capturadas dentro das próprias funções chamadas não são observáveis por esse contador. [não verificado]

```python
# Sonda F6F7 item 1.
# Executar dentro de: manage.py tenant_command shell --schema=juliani
# Somente leitura; nao muda schema, nao abre atomic e nao grava arquivos.

from itertools import groupby

erros = 0
etapa = "inicializacao"


def somente_select(execute, sql, params, many, context):
    # Defesa adicional: rejeita comandos que nao sejam SELECT.
    if not sql.lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL nao SELECT bloqueado")
    return execute(sql, params, many, context)


try:
    from django.db import connection
    from django.utils import timezone

    from chamados.models import DecisaoPergunta as D
    from colaboradores.queries import empresas_visiveis
    from folha.fora_da_folha import dias_que_seguram
    from ponto.services.he_pendente_lavrado import competencias_a_lavrar

    agora = timezone.now()
    hoje = timezone.localdate(agora)

    with connection.execute_wrapper(somente_select):
        etapa = "predicado_0056"
        # Mesmo predicado da migracao, sem update.
        marcadas = set(
            D.objects.filter(ato="resposta")
            .exclude(ato_motivo="")
            .values_list("pk", flat=True)
        )
        print("RESPOSTAS_PREDICADO_0056", len(marcadas))

        etapa = "sequencias"
        # Uma cauda pode conter respostas ja True e respostas que a 0056
        # marcaria. Examinar as perguntas com qualquer uma dessas respostas.
        candidatas = set(
            D.objects.filter(ato="resposta")
            .filter(resposta_mantem_validacao=True)
            .values_list("pergunta_id", flat=True)
        )
        candidatas.update(
            D.objects.filter(pk__in=marcadas)
            .values_list("pergunta_id", flat=True)
        )

        decisoes = (
            D.objects.filter(pergunta_id__in=candidatas, ato_em__lte=agora)
            .select_related("pergunta")
            .order_by("pergunta_id", "ato_em", "pk")
        )

        perguntas_mudam_ato = 0
        pares_perguntas_do_fato = set()
        perguntas_sem_dia_ou_colaborador = 0

        for pergunta_id, linhas in groupby(
            decisoes.iterator(), key=lambda d: d.pergunta_id
        ):
            sequencia = [
                d for d in linhas if not D.es_da_ressuscitacao(d)
            ]
            if not sequencia:
                continue

            k = len(sequencia)
            while k:
                d = sequencia[k - 1]
                mantem_depois = (
                    d.ato == D.RESPOSTA
                    and (d.resposta_mantem_validacao or d.pk in marcadas)
                )
                if not mantem_depois:
                    break
                k -= 1

            if not (
                0 < k < len(sequencia)
                and sequencia[k - 1].ato == D.VALIDACAO
            ):
                continue

            perguntas_mudam_ato += 1
            p = sequencia[-1].pergunta

            # A carga da tarefa usa apenas perguntas do fato.
            if p.conversa:
                continue
            if p.colaborador_id is None or p.data_evento is None:
                perguntas_sem_dia_ou_colaborador += 1
                continue
            pares_perguntas_do_fato.add((p.colaborador_id, p.data_evento))

        print("PERGUNTAS_MUDAM_ATO", perguntas_mudam_ato)
        print("PARES_PERGUNTAS_DO_FATO", len(pares_perguntas_do_fato))
        print(
            "PERGUNTAS_DO_FATO_SEM_DIA_OU_COLABORADOR",
            perguntas_sem_dia_ou_colaborador,
        )

        etapa = "universo_retidos"
        # Mesmo recorte de empresa e competencia do censo de alcance.
        # Conjuntos removem repeticoes entre competencias.
        retidos = set()
        pares_retidos = set()
        competencias = 0

        for empresa in empresas_visiveis():
            for mes, ano in competencias_a_lavrar(empresa, hoje):
                competencias += 1
                universo = dias_que_seguram(empresa, mes, ano, hoje=hoje)
                retidos.update(universo["furo"])
                for cid, (_motivos, dias) in universo["dias"].items():
                    for dia in dias:
                        pares_retidos.add((cid, dia["data"]))

        intersecao = pares_perguntas_do_fato & pares_retidos
        colaboradores_intersecao = {cid for cid, _dia in intersecao}

        print("COMPETENCIAS_EXAMINADAS", competencias)
        print("RETIDOS_DISTINTOS_FURO_ESPELHO", len(retidos))
        print("PARES_DIAS_RETIDOS_DISTINTOS", len(pares_retidos))
        print("INTERSECAO_PARES", len(intersecao))
        print("INTERSECAO_COLABORADORES_DISTINTOS", len(colaboradores_intersecao))
        for cid, dia in sorted(intersecao)[:20]:
            print("PAR", "col%s" % cid, dia.isoformat())

except Exception as exc:
    erros += 1
    # Sem str(exc) ou traceback: podem conter dados pessoais.
    print("ERRO", etapa, type(exc).__name__)
finally:
    print("ERROS", erros)
    print("FIM_SONDA_F6F7_ITEM1")
```

Um resultado zero na interseção só elimina candidatos **nesse recorte e instante**; não substitui o diff de `T`/porta, nem prova ausência de mudança após outros dados ou em competências fora do recorte. [app/chamados/services/tarefa_do_dia.py:484](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:484), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209)

## 18/11 17:32 · sessao c2bbe7b05200 · ESCALA-NAO-DESCREVE · verificador · origem=Code

**O c4 distingue os dois mutantes pela conta da fixture; a evidência de execução dos mutantes permanece [não verificado].** A mudança usa uma órfã que o caminho normal examinado de produção não gera, e o item 7 do veredito contém divergências do literal do aval que não podem ser tratadas como resolvidas sem aceite explícito. [app/escala/tests/test_escala_nao_descreve.py:63; app/escala/utils.py:621; anexo `veredito.md`, item 7]

1. **Mutantes de R1.** A fixture coloca em A `08:10`, `08:20`, `8h` e `14:00`; R1 espera carga **4**. [app/escala/tests/test_escala_nao_descreve.py:63; app/escala/tests/test_escala_nao_descreve.py:91]

   - `n_orfas = 3 + 1 = 4`, pois soma o comprimento das listas. [app/ponto/services/esmeril_espelho.py:130]
   - `_hm` lê `08:10` e `08:20` na faixa 16, lê `14:00` na faixa 28 e retorna `None` para `8h`; assim, `a5 = {16: 2, 28: 1}`. [app/ponto/services/esmeril_espelho.py:31; app/ponto/services/esmeril_espelho.py:132]
   - Portanto, `len(a5) = 2` e `sum(a5.values()) = 3`: ambos divergem do 4 exigido pelo teste, que recebe a medida pela lavra. [app/ponto/services/esmeril_espelho.py:142; app/ponto/services/esmeril_espelho.py:235; app/escala/tests/test_escala_nao_descreve.py:88]

   O diff anexo relata uma falha para cada mutante e restauração do arquivo, mas os logs dessa execução não foram fornecidos: **rodada efetiva [não verificado]**. [anexo `delta.diff`, mensagem do commit] [nao verificado]

2. **Escritor e formato.** O cartório monta `ata['orfas']` com `str(h)` para cada elemento de `dia['orfas']`, passa a ata à operação de lavra e a porta atribui a nova ata à `CelulaDia`. [app/ponto/services/cartorio.py:730; app/ponto/services/cartorio.py:745; app/ponto/services/cartorio.py:1852; app/ponto/portas/celula.py:1049; app/ponto/portas/celula.py:1090]

   Nos caminhos de montagem examinados, as horas vêm de `strftime('%H:%M')`, inclusive as horas de células que depois viram órfãs; logo, **`'8h'` não é forma gerada por esses caminhos**, embora `ata_do_dia` isoladamente aceite e converta qualquer valor recebido. Dados antigos, escritos por outro caminho ou corrompidos: **[não verificado]**. [app/escala/utils.py:621; app/escala/utils.py:662; app/escala/utils.py:1722; app/escala/utils.py:326; app/escala/utils.py:1832; app/ponto/services/cartorio.py:745]

   Duas órfãs legíveis na mesma faixa fazem `len(a5)` diferir de `n_orfas`, **mas não** fazem `sum(a5.values())` diferir: cada órfã lida incrementa o contador uma vez. Para a soma diferir é necessário ao menos um elemento que `_hm` não leia; não identifiquei outra forma produzida pelo caminho normal examinado. [app/ponto/services/esmeril_espelho.py:130; app/ponto/services/esmeril_espelho.py:135]

   O selo pode manter o caso defensivo `'8h'` para a regra literal de contar elementos, mas deve identificá-lo como caso sintético; não há, na evidência examinada, uma forma normal de produção comprovada para substituí-lo e ainda matar esse mutante. [app/escala/tests/test_escala_nao_descreve.py:5; app/ponto/services/esmeril_espelho.py:130]

3. **Outros testes do módulo.** A nova fixture tem apenas **duas** órfãs na faixa 16 e uma na faixa 28; A5 exige ao menos três na mesma faixa, portanto não cria A5 para A. Isso preserva as pré-condições de R1 e R3 de que A não tenha assinatura nem proposta, quanto a essa alteração de A5. [app/escala/tests/test_escala_nao_descreve.py:63; app/ponto/services/esmeril_espelho.py:148; app/escala/tests/test_escala_nao_descreve.py:89; app/escala/tests/test_escala_nao_descreve.py:105] Resultado efetivo dos demais testes: **[não verificado]**.

4. **`HistoricoVinculo.de`.** `executar_vinculo` fotografa o vínculo ativo anterior como `código desde data · turno data-ou-"-"`, passa esse texto a `registrar_mudanca`, que cria `HistoricoVinculo.de`. [app/colaboradores/services/vinculo.py:182; app/colaboradores/services/vinculo.py:349; app/colaboradores/services/historico_vinculo.py:7]

   Na fixture de R4, X começa no template de código `ENDL-5`; portanto, o texto anterior esperado contém `ENDL-5`, como a nova asserção exige. [app/escala/tests/test_escala_nao_descreve.py:127; app/escala/tests/test_escala_nao_descreve.py:135; app/escala/tests/test_escala_nao_descreve.py:140; app/escala/tests/test_escala_nao_descreve.py:288]

5. **Classificação dos itens do veredito**, considerando o literal reproduzido no próprio anexo; o texto integral do aval é **[não verificado]**:

   - **Item 3 — FILA.** Há leitores separados da classificação de ciclo, mas o anexo caracteriza o leitor anterior como herdado e a obra como acrescentando outro leitor; isso, por si só, não demonstra violação da proibição literal de *juiz novo*. [app/escala/services/cadastro_realidade.py:849; app/escala/regua_defesa.py:313; app/colaboradores/services/vinculo.py:128; anexo `veredito.md`, itens 2–3]
   - **Item 4 — BLOQUEIA o selo de R5.** O caso declarado exige que o POST direto seja recusado pela porta, mas o teste só verifica que não houve vínculo; o POST retorna no predicado da view antes de chamar `realocar_vinculo_para_destino`. Assim, não prova a recusa da porta indicada em R4/R5. [app/escala/tests/test_escala_nao_descreve.py:12; app/escala/tests/test_escala_nao_descreve.py:15; app/escala/tests/test_escala_nao_descreve.py:299; app/escala/views.py:498; app/escala/views.py:502; app/escala/regua_defesa.py:313]
   - **Item 6 — FILA**, com risco real de proposta antiga: o leitor busca o último snapshot sem limite de idade e a view usa sua proposta para aplicar a partir de hoje; o anexo não reproduz exigência literal de validade temporal. Existência ou efeito de lavra vencida em produção: **[não verificado]**. [app/ponto/services/esmeril_espelho.py:371; app/escala/views.py:493; app/escala/regua_defesa.py:315; anexo `veredito.md`, item 6]
   - **Item 7 — BLOQUEIA por divergência literal pendente de aceite.** Segundo o anexo, o aval inclui `ciclo` no mapa e manda o link ao wizard; o código omite `ciclo` e aponta à tela de vínculo. A justificativa técnica para ler o ciclo atual não comprova alteração do aval. [anexo `veredito.md`, item 7; app/ponto/services/esmeril_espelho.py:245; app/escala/services/cadastro_realidade.py:13; app/escala/tests/test_escala_nao_descreve.py:299]
   - **Item 8 — FILA**, pelo que é verificável: a L-040 ainda cita o selo antigo, e a L-042 tem a coluna `PROTEGE` vazia; o portão cobra citação quando o diff toca função declarada nessa coluna. Não há prova disponível de uma exigência literal do aval que transforme esses dois apontamentos em bloqueio desta obra. Resultado do portão sobre o range de pouso: **[não verificado]**. [app/docs/LEIS.md:88; app/docs/LEIS.md:90; bin/tests/test_lei_protege_sitio.sh:13; bin/tests/test_lei_protege_sitio.sh:200]

## 18/11 17:25 · sessao c8d57d9f8e0f · LOTE-CONHECE-A-IRMA · sonda · origem=Code

Na saída raspada fornecida:

- Universo de perguntas de intervalo-hora na fila: **550**. [não verificado]
- Em `b_conversa` por conflito: **72**. [não verificado]
- Com conflito somente de batida de irmã gravada: **23**. [não verificado]
  - Com exatamente uma irmã gravada: **23**. [não verificado]
  - Com mais de uma irmã gravada, no contador de irmã ambígua: **0**. [não verificado]
- Erros: **0**. [não verificado]

A conferência do método da sonda ficou **não verificada**: o arquivo `logs/RAIA-INTERVALO/sonda_irma_ambigua.py` não foi encontrado na cópia. [não verificado]

No código, `classe_do_lote` só marca conflito depois de passar pelas verificações anteriores e usa batidas apuráveis próximas ao instante do slot. [validacao.py](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1177) Já `metade_irma_do_intervalo` procura uma única irmã respondida **ainda não materializada**, com filtros adicionais. [materializacao.py](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:139) Isso não permite confirmar que o método da sonda, que relata irmãs gravadas, corresponda ao método pretendido ou detectar eventual divergência. [não verificado]

## 18/11 17:24 · sessao df1a716c32a1 · RAIA-INTERVALO · sonda · origem=Code

Na saída fornecida, **7 perguntas** estão em `espera_declarado_x_gravado`: **7 dentro do U163 e 0 fora**. O contador `respostas_intervalo_recusadas_paridade()['espera_declarado']` também tem **7**; a interseção é **7**, com **0 só na função** e **0 só no contador**. Os IDs informados são 41706, 45598, 45599, 47207, 50176, 50177 e 51526. [não verificado em arquivo:linha; números transcritos da saída fornecida]

Esses conjuntos coincidem **nesta medição**, embora os critérios permitam diferença. O contador começa pelas perguntas do U163 e acrescenta à lista as não carimbadas cujo estado é espera; a função consulta o estado de uma pergunta de intervalo tipo hora sem exigir, ela própria, que o PK consultado pertença ao U163. [materializacao.py:323](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:323), [materializacao.py:341](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:341), [materializacao.py:762](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:762)

O portão examina as metades que seriam gravadas no ato e exige U163 para a **metade divergente**. Quando o ato grava um par, isso permite, em princípio, que a consulta de uma pergunta fora do U163 retorne espera por causa da irmã dentro do U163; essa pergunta ficaria fora do contador, que seleciona pelo próprio PK. Nesta saída, não apareceu nenhum caso assim. [materializacao.py:748](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:748), [materializacao.py:923](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:923), [não verificado em arquivo:linha para o resultado da sonda]

**Erros à parte:** a sonda informa `erros 0` e `ERROS []`; a saída também contém mensagens `ERROR` de `tipo_marco_divergente`, algumas repetidas. Esses registros não devem ser somados às sete esperas nem confundidos com a lista de erros da sonda. [não verificado em arquivo:linha; saída fornecida]

As **79** do aval são identificadas como contagem histórica; a saída atual, por si só, não mostra a interseção nem a evolução entre aquelas 79 e as 7 atuais. Qualquer relação adicional é **[não verificado]**. [RELATO.md:13](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/docs/RELATO.md:13)

## 18/11 17:23 · sessao 9f77027bbad7 · RAIA-INTERVALO · sonda · origem=Code

A sonda fornecida relata **“não” para zero faltantes em todos os 28 dias**. Em nenhum deles ela mostra uma batida ocupando o marco que a ata aponta como faltante. A saída identifica a sonda como `sonda28.py`, mas esse arquivo não foi localizado nesta cópia; os dados abaixo são transcrição do material fornecido e, quanto à execução independente, **[não verificado]**.

Em cada linha: `colaborador data grupo — grade vê 0 faltantes?; marco que a ata dá por apagado; batida que o ocupa (hora, tipo, origem → marco casado)`.

- col622 27/10 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 29/10 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 04/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 05/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 12/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 13/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 16/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col362 22/10 G2 — N; S 12:00; nenhuma mostrada. [não verificado]
- col945 29/10 G2 — N; S 12:00; nenhuma mostrada. [não verificado]
- col735 16/11 G2 — N; S 12:00; nenhuma mostrada. [não verificado]
- col583 12/11 G3 — N; S 11:00; nenhuma mostrada. [não verificado]
- col829 11/11 G3 — N; S 14:40; nenhuma mostrada. [não verificado]
- col369 12/11 G3 — N; S 12:00; nenhuma mostrada. [não verificado]
- col224 28/10 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col469 01/11 G4 — N; E 19:00; nenhuma mostrada. [não verificado]
- col924 11/11 G4 — N; E 19:00; nenhuma mostrada. [não verificado]
- col631 03/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col988 31/10 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col988 03/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col836 04/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col1000 03/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col1000 13/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col962 05/11 G4 — N; E 18:00; nenhuma mostrada. [não verificado]
- col962 09/11 G4 — N; E 18:00; nenhuma mostrada. [não verificado]
- col962 16/11 G4 — N; E 18:00; nenhuma mostrada. [não verificado]
- col991 09/11 G4 — N; E 17:00; nenhuma mostrada. [não verificado]
- col991 11/11 G4 — N; E 17:00; nenhuma mostrada. [não verificado]
- col787 30/10 G5 — N; E 08:00 e E 13:00; nenhuma mostrada para qualquer dos dois. [não verificado]

A saída mostra, por exemplo, que em col369 12/11 as batidas **12:53:12 S/app** e **13:55:44 S/app** casaram com `NENHUM`; em col787 30/10, **10:32:17 E/app** também casou com `NENHUM`. Não há, portanto, marco casado a atribuir a essas batidas no dado fornecido. [não verificado]

**Resposta por grupo:** G1, G2, G3, G4 e G5: **não**, a grade não vê zero faltantes em nenhum dos respectivos dias relatados; e o passo em que uma batida ocuparia o marco apagado é **[não verificado] em todos os grupos**. A saída não mostra essa ocupação e não permite classificá-la como ocorrida dentro ou fora de `escala/utils.py`. [não verificado]

Nesta cópia, o caminho de casamento está em [`_match_marcos`](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/escala/utils.py:595), que pode usar alinhamento por papel ou a rotina de alinhamento; o cartório monta as lâmpadas a partir das células e também contém um ramo de proximidade para marco ausente da régua em [`lampadas_do_dia`](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/ponto/services/cartorio.py:656). O leitor transforma as lâmpadas da ata em células e deriva `missing` de `acesa` em [`_celulas_da_ata`](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/escala/services/leitor_celula.py:114). A existência desses caminhos no código não identifica qual ramo foi executado para cada registro da sonda **[não verificado]**.

## 18/11 17:21 · sessao d73e96b3b221 · LOTE-CONHECE-A-IRMA · sonda · origem=Code

No dado raspado, os contadores são **[não verificado]**:

- Universo de perguntas de intervalo-hora na fila: **550**
- Em `b_conversa` por conflito: **72**
- Conflito só de batida de irmã gravada: **23**
  - Com exatamente uma irmã gravada: **23**
  - Com mais de uma irmã gravada — contador de irmã ambígua: **0**
- Erros: **0**

A conferência do método da sonda ficou **[não verificada]**: o arquivo indicado não está disponível nesta cópia. O código de `classe_do_lote` marca conflito pela existência de qualquer batida apurável no intervalo de 60 minutos em torno do instante do slot, sem identificar sua origem ([validacao.py:1133](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1133), [validacao.py:1204](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1204)).

Há uma **diferença de critério** a considerar: `metade_irma_do_intervalo` procura irmã **ainda não materializada** e só retorna uma quando há exatamente uma candidata; isso não equivale diretamente à contagem de irmãs já gravadas ([materializacao.py:139](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:139)). Sem o código da sonda, não é possível confirmar se ela tratou essa diferença nem se as 23 sairiam do conflito pela cura **[não verificado]**.

## 18/11 17:21 · sessao 6fe46adaff6b · - · - · origem=Ronald (tmux)

A classificação `b_conversa` por conflito só ocorre depois de passar pelos testes de resposta legível, marco, AM/PM e envelope; ela consulta batidas apuráveis em até 60 minutos do instante do slot ([validacao.py:1177](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1177), [validacao.py:1133](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1133)).

`metade_irma_do_intervalo` procura irmã **ainda não materializada** e só retorna uma candidata única, após outros filtros ([materializacao.py:139](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:139)). Seu critério, portanto, não é diretamente a contagem de irmãs já gravadas.

A sonda indicada não foi encontrada nesta cópia; a comparação direta de seu método com essas funções está [não verificado].

## 18/11 17:21 · sessao 30f63ae7fee3 · - · - · origem=Ronald (tmux)

A função [metade_irma_do_intervalo](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:139) procura uma irmã **não gravada** e retorna uma candidata somente se houver exatamente uma; também exige resposta com texto e exclui perguntas fora do fato ou em voo. A contagem pedida de irmãs **gravadas** mede outra população.

A função [classe_do_lote](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1204) considera conflito a existência de qualquer batida apurável perto do instante do slot, sem excluir batida de irmã.

O arquivo da sonda indicado não foi localizado na cópia; a comparação do método efetivamente usado pela sonda com essas funções está [não verificado].

## 18/11 17:21 · sessao da741c9eb6ab · - · - · origem=Ronald (tmux)

A sonda `logs/RAIA-INTERVALO/sonda_irma_ambigua.py` não está presente na cópia consultada. Assim, sua metodologia e o hash informado não puderam ser conferidos; a correspondência entre seus contadores e as funções de produção é **[não verificado]**.

## 18/11 17:20 · sessao a3a69aa0eb6d · DECLARADO-X-GRAVADO · sonda · origem=Code

Na saída fornecida, a função real encontrou **7 perguntas em espera**, todas dentro do U163 e nenhuma fora. A varredura informa `universo_intervalo_hora 19822`, `erros 0` e `ERROS []`. A leitura ocorreu em 18/11 às 17:19; a situação atual é **[não verificado]**. [Fonte: saída raspada fornecida; execução e base não verificadas.]

| Comparação informada pela sonda | Perguntas |
|---|---:|
| Função real: total | 7 |
| Função real: dentro / fora do U163 | 7 / 0 |
| Contador `espera_declarado` | 7 |
| Interseção informada | 0 |
| Só na função / só no contador, informado | 7 / 7 |

Esses últimos três números **não podem ser tomados como interseção confirmada de PKs** sem conferir a comparação feita pela sonda. A saída os apresenta, mas o código da sonda não foi localizado na cópia consultada. [Fonte: saída raspada fornecida; código da comparação não verificado.]

Há uma tensão com o código da cópia: a função consulta a testemunha e retorna detalhe quando seu estado é espera; para uma pergunta não carimbada dentro de U163, o contador consulta essa mesma testemunha e adiciona o PK à lista quando recebe esse mesmo estado. Portanto, **se as duas leituras viram os mesmos dados e a comparação usou PKs dos dois lados**, os sete da função dentro de U163 deveriam aparecer também no contador. A causa concreta do `inter_paridade 0` é **[não verificado]**. [materializacao.py:324](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:324), [materializacao.py:341](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:341), [materializacao.py:762](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:762), [materializacao.py:875](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:875).

Os dois leitores têm, em geral, escopos diferentes: o contador percorre apenas PKs de U163 e trata as carimbadas separadamente; a função aceita uma pergunta de intervalo tipo hora também fora de U163. Na avaliação de um par, a guarda examina as duas metades e pode encontrar a divergência na irmã pertencente a U163. Assim, **em geral**, uma pergunta fora de U163 pode aparecer na função e não no contador. Isso não explica os `7 só função` desta saída, pois ela informa que todas as sete estão dentro de U163. [materializacao.py:324](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:324), [materializacao.py:329](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:329), [materializacao.py:748](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:748), [materializacao.py:938](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:938).

A saída também contém linhas de log `tipo_marco_divergente`; elas são separadas do `erros 0` e de `ERROS []` informados pela sonda. Não se pode concluir, apenas desses campos, que não houve mensagens de erro no log. [Fonte: saída raspada fornecida; execução não verificada.]

Quanto às **79 do aval DECLARADO-NA-VAGA**, a saída fornecida não apresenta sua lista nem uma comparação com os conjuntos atuais. A interseção, quantas continuam em espera e a evolução entre a contagem histórica e esta leitura são **[não verificado]**.

## 18/11 17:18 · sessao 09d4943bb652 · RAIA-INTERVALO · sonda · origem=Code

**A sonda fornecida não mostra nenhuma batida ocupando um marco que a ata informa como apagado.** Em todos os 28 dias, `grade_ve_0_faltantes=False`. As linhas abaixo transcrevem o dado fornecido; como o arquivo da sonda não está nesta cópia, sua execução e seu conteúdo não foram verificados. [não verificado]

Em cada linha, “batida no marco apagado” é **não mostrada**: portanto hora, tipo, origem e marco casado dessa suposta batida ficam **[não verificado]**. O marco entre parênteses é o faltante informado pela sonda.

```text
col622  27/10 G1: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col622  29/10 G1: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col622  04/11 G1: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col622  05/11 G1: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col622  12/11 G1: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col622  13/11 G1: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col622  16/11 G1: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col362  22/10 G2: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col945  29/10 G2: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col735  16/11 G2: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col583  12/11 G3: N; batida no marco apagado S 11:00: não mostrada [não verificado]
col829  11/11 G3: N; batida no marco apagado S 14:40: não mostrada [não verificado]
col369  12/11 G3: N; batida no marco apagado S 12:00: não mostrada [não verificado]
col224  28/10 G4: N; batida no marco apagado E 13:00: não mostrada [não verificado]
col469  01/11 G4: N; batida no marco apagado E 19:00: não mostrada [não verificado]
col924  11/11 G4: N; batida no marco apagado E 19:00: não mostrada [não verificado]
col631  03/11 G4: N; batida no marco apagado E 13:00: não mostrada [não verificado]
col988  31/10 G4: N; batida no marco apagado E 13:00: não mostrada [não verificado]
col988  03/11 G4: N; batida no marco apagado E 13:00: não mostrada [não verificado]
col836  04/11 G4: N; batida no marco apagado E 13:00: não mostrada [não verificado]
col1000 03/11 G4: N; batida no marco apagado E 13:00: não mostrada [não verificado]
col1000 13/11 G4: N; batida no marco apagado E 13:00: não mostrada [não verificado]
col962  05/11 G4: N; batida no marco apagado E 18:00: não mostrada [não verificado]
col962  09/11 G4: N; batida no marco apagado E 18:00: não mostrada [não verificado]
col962  16/11 G4: N; batida no marco apagado E 18:00: não mostrada [não verificado]
col991  09/11 G4: N; batida no marco apagado E 17:00: não mostrada [não verificado]
col991  11/11 G4: N; batida no marco apagado E 17:00: não mostrada [não verificado]
col787  30/10 G5: N; batida nos marcos apagados E 08:00 e E 13:00: não mostrada [não verificado]
```

Por grupo, a resposta à pergunta “a grade vê zero faltantes?” é **não**: G1 0/7, G2 0/3, G3 0/3, G4 0/14 e G5 0/1. Em nenhum grupo a saída identifica um passo que tenha ocupado o marco apagado; logo não é possível atribuir essa suposta ocupação a `escala/utils.py` ou a outro sítio. [não verificado]

O código desta cópia distingue os possíveis sítios, sem provar qual ramo executou em cada caso:

- O casamento em `_match_marcos` pode usar alinhamento por papel ou a DP, produzindo célula `missing` quando não há par ([utils.py:595](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/escala/utils.py:595), [utils.py:622](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/escala/utils.py:622), [utils.py:639](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/escala/utils.py:639)).
- Na lavratura, `lampadas_do_dia` normalmente copia a célula correspondente; existe também um ramo de proximidade quando a chave do DNA não está na régua. A saída fornecida não demonstra que esse ramo tenha ocupado qualquer marco destes dias ([cartorio.py:656](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/ponto/services/cartorio.py:656), [cartorio.py:680](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/ponto/services/cartorio.py:680)). [não verificado]
- O leitor transforma as lâmpadas da ata em células, copiando luz e tipo real e derivando `missing` de `acesa`; esse trecho não casa batidas ([leitor_celula.py:114](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/escala/services/leitor_celula.py:114)).

Há batidas com tipo diferente do marco casado em col735 e nos col583 e col829; a saída as associa a **outros marcos**, não aos indicados como faltantes. Em col369 e col787, há batidas explicitamente marcadas `NENHUM`. [não verificado]

## 18/11 17:10 · sessao 82ae805f088e · B4-OLHA-O-RESET · verificador · origem=Code

**Parecer: não aprovo o diff como está.** Com uma cota estável entre as leituras, ele implementa o corte de três horas. Mas há um contraexemplo em que a ESCADA lê 84% e a segunda leitura recebe 86% com reset próximo: o diff permite o Agent, embora a segunda medida esteja na ESCADA. A troca atômica de `cota.json` permite que as duas leituras vejam versões diferentes. O risco de duas leituras já existe no código anterior; a nova folga cria o caminho que deixa passar uma segunda medida de 86%. ([bin/disjuntor.py:1203](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1203), [bin/disjuntor.py:1215](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1215), [bin/statusline_cota.py:45](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/statusline_cota.py:45); diff fornecido, trecho `b4_decide`: [não verificado em arquivo])

O aval e o diff vieram como texto na pergunta; não há arquivos `aval.txt` ou `diff.patch` para citar com linhas nesta cópia. As referências ao conteúdo desses anexos abaixo são, portanto, **[não verificado em arquivo]**. A cópia disponível contém o código anterior ao diff; também não executei o selo, pois ele cria e remove arquivos temporários. ([bin/tests/test_disjuntor.sh:40](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:40), [bin/tests/test_disjuntor.sh:50](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:50))

1. **Regra e bordas.** Para uma leitura válida e estável, o diff conserva a prioridade da ESCADA, passa abaixo de 75%, e em `75% <= pct < 85%` passa se as horas forem `<= 3.0` e nega se forem maiores; a negativa inclui as horas. Cota ausente, vencida ou ilegível continua retornando `None`. Com folga zero, toda leitura válida tem `reset > agora`, logo horas positivas e a definição antiga é recuperada. A apresentação com uma casa decimal pode, porém, dizer `faltam 3.0 h` numa negativa quando faltam, por exemplo, 3,01 h: a decisão usa o valor sem arredondar. ([bin/disjuntor.py:1159](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1159), [bin/disjuntor.py:1203](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1203); diff fornecido, trechos `ler_cota_e_reset` e `_b4_reset_perto`: [não verificado em arquivo])

2. **Escritores e leitores.**
   - `seven_day_resets_at` e o percentual são escritos pela status line a partir de `rate_limits.seven_day`, com substituição atômica. No código atual, `ler_cota` lê e valida ambos; no diff, `ler_cota_e_reset` faz essa leitura e `ler_cota` vira uma projeção do percentual. `escada`, o painel `ler` e, no diff, `b4_decide` consomem essa via; não identifiquei outra derivação de `cota.json` nos leitores Python pesquisados. ([bin/statusline_cota.py:40](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/statusline_cota.py:40), [bin/disjuntor.py:1159](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1159), [bin/disjuntor.py:2044](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:2044); diff fornecido: [não verificado em arquivo])
   - A nova linha `B4-FAIXA` é escrita por `_b4_reset_perto` via `trilha`; falha de escrita é engolida, portanto a medida não é garantida em toda passagem. `ler` filtra eventos pelo terceiro campo e não conta `B4-FAIXA` como `NEGOU`, `DISJUNTOR`, `FALHA` ou `LIBERA`; o leitor do censo também não casa essa linha. `gerar_estado.py` usa esse mesmo `ler`. No trecho localizado do Stop, o arquivo é identificado como protegido, não interpretado como uma prova de espera. Assim, **não encontrei reclassificação da linha pelos leitores indicados**. ([bin/disjuntor.py:193](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:193), [bin/disjuntor.py:1979](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1979), [bin/disjuntor.py:2049](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:2049), [bin/gerar_estado.py:285](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/gerar_estado.py:285), [bin/hook_stop_fila1.py:664](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/hook_stop_fila1.py:664); diff fornecido: [não verificado em arquivo])
   - `b4.negou_desde` é lido, criado e removido no ramo Agent do PreToolUse, sob a transição de estado existente. Continua marcando o início de uma sequência de negativas, mas agora também é removido quando a folga permite passar com cota ainda acima de 75%; o comentário “cota abaixo do limiar” fica incompleto. Uma negativa posterior pode gerar novo disparo B4. ([bin/disjuntor.py:1858](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1858), [bin/disjuntor.py:335](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:335); diff fornecido: [não verificado em arquivo])

3. **Classes de reprovação.** Não identifiquei segundo juiz nem segundo escritor da cota introduzidos pelo diff: a decisão permanece em `b4_decide`, e a leitura física proposta é centralizada em `ler_cota_e_reset`. Isso não elimina as **duas chamadas de leitura por decisão**, causa do contraexemplo acima. `agora` vem da borda `main_pretool`, é passado por `pretool` e chega a `b4_decide`; a aritmética do diff usa esse mesmo instante. ([bin/disjuntor.py:1895](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1895), [bin/disjuntor.py:1802](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1802), [bin/disjuntor.py:1858](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1858); diff fornecido: [não verificado em arquivo])

   O selo proposto tem mordida contra “sempre passa” e “nunca passa”, segundo sua estrutura e o resultado relatado, mas esse resultado de execução é **[não verificado]**. O caso de borda usa 10.790 segundos, não exatamente 10.800: um mutante que trocasse `<=` por `<` poderia sobreviver a esse caso. Os mutantes antigos dependem das âncoras e casos existentes; sua execução verde relatada também é **[não verificado]**. ([bin/tests/test_disjuntor.sh:1507](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:1507), [bin/tests/test_disjuntor.sh:1567](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:1567), [bin/tests/test_disjuntor.sh:1799](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:1799); diff fornecido, caso `b4_reset`: [não verificado em arquivo])

4. **Corrida do texto `1.0 h`: sim, é possível, mas exige atraso grande.** `_cota` fixa o reset como `date +%s + 3600`, e o PreToolUse captura depois seu próprio `time.time()`. Um atraso próximo de 180 segundos entre esses pontos leva a medida à vizinhança de 0,95 h, onde o arredondamento para uma casa pode produzir `0.9` e fazer a comparação textual falhar, embora a decisão de passar esteja correta. A execução normal ou ocorrência real desse atraso é **[não verificado]**. O mesmo princípio afeta a expectativa textual de `4.0 h`. ([bin/tests/test_disjuntor.sh:435](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:435), [bin/disjuntor.py:1895](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1895); diff fornecido, caso `b4_reset`: [não verificado em arquivo])

## 18/11 16:56 · sessao 5e7ff4e30e77 · B4-OLHA-O-RESET · verificador · origem=Code

**AVAL-DEVOLVIDO — CONTRATO-DE-INSTRUMENTO, pergunta 3:** a medida exigida não existe hoje para cada passagem pela faixa, e `ler_cota` não entrega o instante do reset ao chamador. O contrato precisa ser implementado e demonstrado; não basta alterar a mensagem da negativa. [bin/disjuntor.py:1159](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1159), [bin/disjuntor.py:1858](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1858), [bin/disjuntor.py:1901](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1901).

Uso **CONFERE** quando a premissa ou a ausência do problema é sustentada; **PROCEDE** quando há divergência ou condição que não fecha. O texto do aval foi fornecido na mensagem, sem arquivo e linhas verificáveis; o estado de produção, o prazo e a execução futura são **[não verificado]**.

### 1 CONSTRUIR

1. **Premissas — CONFERE, com ressalva.** Hoje `B4_NEGA = 75`, `B4_PARA = 85`; `b4_decide` consulta primeiro a escada e depois nega a faixa a partir de 75%. A ressalva é que `ler_cota` *lê* o reset para validá-lo, mas devolve apenas o percentual. [bin/disjuntor.py:145](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:145), [bin/disjuntor.py:1159](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1159), [bin/disjuntor.py:1215](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1215).
2. **Colisão — CONFERE como alteração expressa.** A regra pedida contraria a definição anterior para 75–85%, mas o próprio dado fornecido declara o corte que a altera nessa faixa. A escada continua precedendo a decisão; Stop e B4-CODEX também usam `escada`, portanto preservar sua interface e comportamento importa. [bin/disjuntor.py:20](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:20), [bin/disjuntor.py:1297](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1297), [bin/disjuntor.py:1471](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1471). Outra colisão vigente específica: **[não verificado]**.
3. **Literal — PROCEDE.** A implementação precisa obter percentual e reset de uma leitura coerente e manter a validação atual de reset vencido, ausente ou inválido; o retorno atual não contém o reset. Uma passagem que passa também limpa `b4.negou_desde`, inclusive pela nova razão de reset próximo: isso altera quando o próximo disparo B4 será contado. [bin/disjuntor.py:1162](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1162), [bin/disjuntor.py:1858](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1858).
4. **Exportada, dinheiro ou `!` — CONFERE no escopo declarado.** O trecho inspecionado decide chamadas de `Agent`; não evidencia escrita de folha ou exportada. Efeito indireto em trabalhos posteriores ou execução em produção: **[não verificado]**. Os casos que exigem `!` estão em [CLAUDE.md:265](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/CLAUDE.md:265); a unidade do código em [bin/disjuntor.py:1839](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1839).

### RED

1. **Premissas — CONFERE por leitura do código.** Com 80% e reset válido em 1 h ou 4 h, hoje nega; com 86% e 1 h, a escada nega. Os casos existentes usam 86.400 segundos e cobrem inclusive 75%, 85% e reset vencido. Execução real do RED e do verde: **[não verificado]**. [bin/disjuntor.py:1203](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1203), [bin/tests/test_disjuntor.sh:435](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:435).
2. **Colisão — CONFERE.** Os três casos propostos são compatíveis com preservar a escada e os casos existentes, sujeito à execução do selo. [bin/tests/test_disjuntor.sh:443](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:443).
3. **Literal — PROCEDE como cuidado de implementação.** O mutante existente procura uma sequência **textual exata** de `b4_decide`; se a construção remover essa âncora, o selo falha mesmo sem editar o teste. A construção deve preservar a âncora ou resolver essa condição sem afrouxar o selo; que isso foi conseguido é **[não verificado]**. O caso de 4 h distingue o mutante proposto “sempre passa”. [bin/tests/test_disjuntor.sh:1515](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:1515), [bin/tests/test_disjuntor.sh:1567](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:1567).
4. **Exportada, dinheiro ou `!` — CONFERE no escopo do teste.** O selo usa árvore de teste e invoca o hook contra ela; não há ato sobre produção demonstrado. Execução nesta revisão: **[não verificado]**. [bin/tests/test_disjuntor.sh:81](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:81).

### CONTRATO-DE-INSTRUMENTO

1. **Premissas — CONFERE, com ressalva.** O `PreToolUse` chama o disjuntor, e este chama `b4_decide` no ramo `Agent`. Cota ilegível ou sem reset válido faz `ler_cota` devolver `None`; a escada por palavra humana ainda pode negar independentemente da cota. [settings.json:62](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/.claude/settings.json:62), [bin/disjuntor.py:1159](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1159), [bin/disjuntor.py:1209](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1209).
2. **Colisão — CONFERE no corte declarado.** A medida nova deve preservar o significado dos eventos existentes: `ler` conta `DISJUNTOR` por identificador e `NEGOU` como negativa; o ESTADO consome essa leitura. [bin/disjuntor.py:1971](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1971), [bin/gerar_estado.py:285](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/gerar_estado.py:285).
3. **Literal — PROCEDE.** A afirmação de que o log só recebe linha na primeira negativa precisa de precisão: o **disparo `DISJUNTOR B4`** é condicionado a `negou_desde`, mas **cada negativa** também gera `NEGOU`; a passagem que passa não gera a medida exigida de percentual e horas. Além disso, `trilha` engole falhas de escrita, logo “cada passagem” não é garantido pelo mecanismo atual. O contrato diz “quem espera = ninguém”, mas a transição de estado existente tenta adquirir `flock` até 20 vezes, com espera entre tentativas. [bin/disjuntor.py:1859](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1859), [bin/disjuntor.py:1901](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1901), [bin/disjuntor.py:193](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:193), [bin/disjuntor.py:345](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:345). A efetividade de ajustar a constante a zero depende da implementação futura: **[não verificado]**.
4. **Exportada, dinheiro ou `!` — CONFERE no instrumento declarado.** A trilha e o estado examinados são arquivos de sessão; o contrato não demonstra operação sobre gravado exportado ou dinheiro. [bin/disjuntor.py:121](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:121), [CLAUDE.md:265](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/CLAUDE.md:265).

### PRONTO

1. **Premissas — CONFERE como critério futuro; cumprimento [não verificado].** O selo e a condição de uma passagem real existem como critérios no dado fornecido; não há resultado de execução nem passagem real no dado raspado. O selo existente está em [bin/tests/test_disjuntor.sh:1501](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:1501).
2. **Colisão — [não verificado].** A abertura do opus, a árvore indicada e a situação da vaga dependem de fatos não demonstrados pelo dado fornecido. A ordem geral de fila está em [CLAUDE.md:288](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/CLAUDE.md:288).
3. **Literal — PROCEDE.** Verde do selo não prova, sozinho, a **primeira passagem real** nem sua linha no RELATO; a nova medida precisa existir e a passagem precisa ocorrer. [bin/disjuntor.py:1901](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1901).
4. **Exportada, dinheiro ou `!` — [não verificado] quanto à fila subsequente.** Para o selo e o registro do instrumento não foi identificado ato desses tipos; não há dados suficientes para afirmar o alcance do trabalho que vier depois. [CLAUDE.md:265](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/CLAUDE.md:265).

### PROIBIDO

1. **Premissas — CONFERE.** A status line é a escritora declarada de `cota.json`; a escada é compartilhada; e o selo contém casos e mutantes existentes. [bin/statusline_cota.py:4](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/statusline_cota.py:4), [bin/disjuntor.py:1203](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1203), [bin/tests/test_disjuntor.sh:1507](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:1507).
2. **Colisão — CONFERE.** As proibições são compatíveis com preservar a escada e a configuração atual; atalhos como pular teste constam entre os casos que exigem `!`. [CLAUDE.md:265](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/CLAUDE.md:265), [settings.json:62](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/.claude/settings.json:62).
3. **Literal — CONFERE como restrições; cumprimento futuro [não verificado].** Em particular, preservar os testes inclui suas âncoras de mutação, não apenas suas asserções. [bin/tests/test_disjuntor.sh:1515](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:1515).
4. **Exportada, dinheiro ou `!` — CONFERE no escopo declarado.** Não há autorização declarada para ampliar a obra a esses atos; a regra de escopo literal está em [app/docs/LEIS.md:62](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/docs/LEIS.md:62) e os casos de `!` em [CLAUDE.md:265](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/CLAUDE.md:265).

### Escritores e leitores

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `seven_day_pct` | `statusline_cota.py`, a partir de `rate_limits.seven_day.used_percentage`; o teste também fabrica dados de teste. [bin/statusline_cota.py:40](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/statusline_cota.py:40), [bin/tests/test_disjuntor.sh:453](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:453) | `ler_cota`; seu retorno alimenta `escada`, `b4_decide` e a leitura do ESTADO. O teste também inspeciona o JSON. [bin/disjuntor.py:1164](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1164), [bin/disjuntor.py:2044](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:2044), [bin/tests/test_disjuntor.sh:445](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:445) |
| `seven_day_resets_at` | A mesma status line, a partir de `rate_limits.seven_day.resets_at`; o teste fabrica um JSON. [bin/statusline_cota.py:45](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/statusline_cota.py:45), [bin/tests/test_disjuntor.sh:453](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/tests/test_disjuntor.sh:453) | `ler_cota` valida tipo, finitude e vencimento, mas não devolve o reset. Outros leitores diretos na cópia pesquisada: **[não verificado]**. [bin/disjuntor.py:1164](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1164) |
| `b4.negou_desde` | O ramo `Agent` atribui o instante na primeira negativa e remove o objeto `b4` quando uma chamada passa; `_com_estado` grava o arquivo de estado. [bin/disjuntor.py:1859](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1859), [bin/disjuntor.py:335](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:335) | O mesmo ramo lê o campo para limpar o estado ou decidir se registra um novo disparo. O Stop usa `b4_para`, separado, e chama `b4_stop`, não `b4_decide`. [bin/disjuntor.py:1858](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1858), [bin/disjuntor.py:1301](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/disjuntor.py:1301), [bin/hook_stop_fila1.py:1199](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/bin/hook_stop_fila1.py:1199) |

Nenhum arquivo foi criado, editado ou apagado nesta revisão. O diretório disponibilizado não expõe metadados Git para confirmar o hash informado: **[não verificado]**.

## 18/11 16:49 · sessao 2981715c4d0f · DECLARADO-X-GRAVADO · censo · origem=Code

**O censo precisa de atualização antes de ser dado como reconferido:** o portão de espera agora usa os PKs congelados dos 163, há novos leitores e guardas, e os juízes citados em `core/juizes.py` estão em `chamados/juizes.py`. A quantidade de perguntas hoje em espera é **[não verificado]**: não há acesso ao banco nesta cópia. [app/chamados/services/universo_bug_par.py:7; app/chamados/services/materializacao.py:748; app/chamados/juizes.py:1911]

O diretório recebido não contém metadados Git; portanto, o hash informado, a identidade com o HEAD da `main` e um `git diff` contra a fonte do censo são **[não verificado]**. A comparação abaixo usa o censo raspado fornecido e as linhas da cópia disponível. [não verificado]

### R1–R10

**MUDOU** abaixo indica mudança de localização ou de contexto do caminho censado; não significa que o RED aprovado já esteja implementado.

| Item | Reconferência |
|---|---|
| **R1 — MUDOU** | A S139 continua substituindo a resposta pelo marco de intervalo; agora está em `materializacao.py:687–694`. O portão dos 163 pode impedir a gravação antes da criação, mas a regra desejada de retorno ao colaborador não está demonstrada nesse derivador. [app/chamados/services/materializacao.py:687; app/chamados/services/materializacao.py:1476] |
| **R2 — MUDOU** | A substituição pelo marco permanece; portanto, o caminho atual não garante a gravação da hora declarada nesse exemplo. A regra aprovada para a faixa consta do anexo raspado; sua implementação está [não verificado]. [app/chamados/services/materializacao.py:690] |
| **R3 — MUDOU** | A régua `classe_por_delta` está em `chamados/juizes.py`, não no caminho antigo `core/juizes.py`; o derivador de plantio ainda usa a S139. O retorno ao colaborador exigido pelo RED está [não verificado]. [app/chamados/juizes.py:1911; app/chamados/services/materializacao.py:687] |
| **R4 — MUDOU** | O julgamento do par deriva as duas metades; o materializador pode gravar primeiro a irmã e prevê rollback conjunto em colisão. A propriedade completa de soma pedida em R4 está [não verificado]. [app/chamados/services/materializacao.py:175; app/chamados/services/materializacao.py:1393; app/chamados/services/materializacao.py:1511] |
| **R5 — MUDOU** | Reuso permanece em 20 minutos e dedup em 60 segundos. A mensagem de reuso **já mostra as duas horas**, corrigindo a descrição do censo; a trilha examinada não identifica a regra de reuso. Para testar 61 segundos é preciso isolar o dedup: no fluxo de hora, S137 é consultado antes e pode reutilizar a batida. [app/chamados/juizes.py:239; app/ponto/registro_batida.py:130; app/chamados/services/materializacao.py:1372; app/chamados/services/materializacao.py:1547] |
| **R6 — MUDOU** | Além do fio, a testemunha é lida pelo filtro do cartão e pelo explicador; lote e copiloto exibem resposta e previsto. A propriedade integral de idempotência e a exigência de todas as telas lerem o mesmo estado permanecem [não verificado]. [app/chamados/services/fio.py:376; app/chamados/templatetags/chamados_tags.py:134; app/chamados/services/explicador.py:122; app/templates/chamados/partials/_lote_preview.html:24; app/templates/core/_copiloto.html:237] |
| **R7 — MUDOU** | Os pontos atuais de gravação são os ramos hora e `data_hora`; a existência e o resultado do selo abrangente com o mutante exigido estão [não verificado]. [app/chamados/services/materializacao.py:1488; app/chamados/services/materializacao.py:1605] |
| **R8 — MUDOU** | A trilha do ramo hora registra timestamp, tipo e resposta, mas o payload examinado não contém regra aplicada nem marco usado. [app/chamados/services/materializacao.py:1547] | [nao verificado]
| **R9 — MUDOU** | O ramo `data_hora` ainda atribui `00:00` quando só recebe data e tenta registrar uma entrada; o RED aprovado continua necessário. [app/chamados/services/materializacao.py:1593] |
| **R10 — MUDOU** | A comparação do portão continua por HH:MM local e tipo, sem data civil; o derivador aplica o juiz overnight. Quantas das 64 mudariam de classificação por instante é **[não verificado]**. [app/chamados/services/materializacao.py:736; app/chamados/services/materializacao.py:701] |

### Estado, gravação e leitores

`espera_declarado_x_gravado` é um **estado calculado e retornado**, cuja constante e construção ficam em `materializacao.py`. O portão examina as metades que seriam gravadas e só considera divergência de metade de intervalo pertencente ao conjunto congelado por PK; compara hora local e tipo. O estado é produzido onde a testemunha chegaria a `aplicavel`, depois de outras verificações, inclusive reuso e bloqueios. [app/chamados/services/materializacao.py:709; app/chamados/services/materializacao.py:724; app/chamados/services/materializacao.py:748; app/chamados/services/materializacao.py:892]

A porta administrativa chama `espera_declarado_x_gravado`; a porta de escrita chama `espera_na_escrita`, e há nova consulta imediatamente antes da criação para tratar mudança entre as leituras. O contador, o explicador, o cartão e o fio também consultam ou exibem o resultado. [app/chamados/services/validacao.py:695; app/chamados/services/materializacao.py:773; app/chamados/services/materializacao.py:1910; app/chamados/services/materializacao.py:1476; app/chamados/services/materializacao.py:342; app/chamados/services/explicador.py:130; app/templates/chamados/partials/_pergunta_card.html:49; app/templates/chamados/partials/modal_fio.html:242]

A porta que transforma a resposta de hora em batida é `_materializar_resposta_hora`: ela chama `_ts_tipo_do_plantio`, consulta reuso e, se couber criação, chama `registrar_batida`. Para `data_hora`, há ramo próprio. A resposta é escrita por `responder_pergunta_disputa_core` e `responder_automaticamente`, e também é limpa em reabertura e recusa. [app/chamados/services/materializacao.py:1360; app/chamados/services/materializacao.py:1372; app/chamados/services/materializacao.py:1487; app/chamados/services/materializacao.py:1574; app/chamados/services/disputa_emissao.py:1481; app/chamados/services/disputa_emissao.py:1617; app/chamados/services/disputa_emissao.py:1682; app/chamados/services/validacao.py:440]

Entre os leitores da hora estão o próprio derivador de plantio, `_declarado_da_metade`, `delta_da_resposta` e a classificação de validação. `_slot_esperado` agora está em `chamados/juizes.py` e consulta o derivador para resposta de hora; a classificação também é lida por filtros de template e pelo comando de validação de classe A. [app/chamados/services/materializacao.py:637; app/chamados/services/materializacao.py:724; app/chamados/juizes.py:345; app/chamados/juizes.py:1928; app/chamados/juizes.py:2034; app/chamados/templatetags/chamados_tags.py:168; app/chamados/management/commands/validar_classe_a.py:63]

### Sonda na sombra

Para medir **quantas perguntas estão hoje no estado**, a sonda de leitura deve percorrer as `PerguntaDisputa` e contar, uma vez por PK de pergunta, as chamadas `bool(espera_declarado_x_gravado(p))`. Pode limitar previamente a motivos `intervalo_saida`/`intervalo_volta`, `tipo_resposta='hora'` e disputa existente, pois a própria função devolve vazio fora desses critérios. Deve registrar exceções separadamente, sem contá-las como ausência de espera. [app/chamados/services/materializacao.py:762]

Para comparar com o recorte congelado, uma segunda medida deve chamar `respostas_intervalo_recusadas_paridade()` e contar `len(resultado['espera_declarado'])`; a função consulta os PKs de U163 e separa perguntas materializadas e bloqueios de competência. Convém conservar os demais totais retornados para reconciliar o recorte. [app/chamados/services/materializacao.py:295; app/chamados/services/materializacao.py:323; app/chamados/services/materializacao.py:342; app/chamados/services/materializacao.py:363]

Os dois números não são necessariamente iguais: uma pergunta fora dos 163 pode receber espera quando a validação envolve um par cuja outra metade divergente pertence aos 163. As **79** são a contagem histórica informada no anexo, não uma medição atual; sua interseção e evolução frente à sonda de hoje são **[não verificado]**. A separação das 64 por instante também exige medição própria; a função atual compara HH:MM e tipo. [app/chamados/services/materializacao.py:748; app/chamados/services/materializacao.py:923; app/chamados/services/materializacao.py:736; não verificado]

As cinco questões históricas foram cortadas pelo aval raspado; não identifiquei outra decisão de negócio necessária para formular este censo. A exaustividade de leis fora dos anexos e caminhos examinados é **[não verificado]**.

perguntas de lei: 0

## 18/11 16:49 · sessao c73478aa3380 · RAIA-INTERVALO · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: PROCEDE.** A exceção que permite à obra (b) entrar antes da (a) precisa fechar sua relação com a alternância da vaga 2 e com a vez reservada à (a); o censo e a ausência de colisão com raias vivas também ainda não estão provados no material disponível. [Não verificado: arquivo:linha dos anexos e estado das raias.]

Os anexos foram fornecidos como texto raspado, sem numeração de linhas; suas referências abaixo identificam o anexo, mas **arquivo:linha dos anexos é [não verificado]**. A cópia não contém metadados Git acessíveis, portanto o histórico e o HEAD não puderam ser conferidos diretamente. [Não verificado.]

### Item 1 — Construir DECLARADO-X-GRAVADO

1. **Premissas — PROCEDE parcialmente.** DXG-LEIS-CORTADAS responde às cinco linhas de lei e DECLARADO-NA-VAGA dá ordem e condições; o primeiro também exige censo com R1–R10 e “perguntas de lei: 0”. Não há pergunta de lei explicitamente deixada aberta nesses dois textos, mas o censo reconferido contra o HEAD atual não foi fornecido: não se pode concluir que a construção já tenha lei zero comprovada. [Anexos DXG-LEIS-CORTADAS e DECLARADO-NA-VAGA: arquivo:linha não verificado.]

   A BUG-PAR consta como pousada no RELATO e fechada no BACKLOG; a GRADE ainda a mostra em voo. O pouso está, portanto, **registrado**, mas a ancestralidade no Git desta cópia é [não verificado]. ([RELATO.md:535](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/docs/RELATO.md:535), [RELATO.md:468](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/docs/RELATO.md:468), [BACKLOG.md:2140](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/docs/BACKLOG.md:2140), [GRADE-MESTRA.md:1043](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/docs/GRADE-MESTRA.md:1043).)

   A função e o estado existem em [materializacao.py:762](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:762) e [materializacao.py:711](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:711); a validação chama a função em [validacao.py:698](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/validacao.py:698). Os **163** aparecem no complemento BUG-PAR-163-X-DECLARADO e os **79** em DECLARADO-NA-VAGA; os dois números também aparecem no COMPLEMENTO-1. [Arquivo:linha dos anexos não verificado.]

2. **Colisão com aval vigente — PROCEDE quanto à ordem.** RAIA-INTERVALO estabelece alternância na vaga 2 e, se o censo da (a) não estiver fechado quando sua vez chegar, manda entrar a espúria seguinte e dar a vez posterior à (a). O COMPLEMENTO-1 permite que a (b) entre primeiro se só seu censo estiver fechado, sem dizer como essa exceção se encaixa nessa sequência. A ordem da vaga 2 das espúrias está registrada em [BACKLOG.md:2345](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/docs/BACKLOG.md:2345); arquivo:linha dos anexos [não verificado]. O teto de dois construtores e a vedação de pouso antes do fim da simulação permanecem declarados; colisão adicional com eles [não verificado].

3. **Condição literal — PROCEDE.** É preciso esclarecer em qual vez a (b) passa à frente e o destino da vez da (a) e da próxima espúria. A condição (c) é verificável em princípio, mas exige identificar **todas** as raias com agente vivo e comparar seus diffs de nomes no momento da abertura; a lista e os diffs atuais são [não verificado]. [Anexos COMPLEMENTO-1, RAIA-INTERVALO e DECLARADO-NA-VAGA: arquivo:linha não verificado.]

4. **Exportada, dinheiro ou ! — CONFERE quanto ao escopo declarado; efeito concreto [não verificado].** Os avais delimitam lei para frente, excluem o passivo desta obra e encaminham exportadas do passivo à Pauta DP; DXG declara expressamente uma regra que mexe em minuto de intervalo pago. Isso reconhece o alcance financeiro, mas não prova o efeito de uma implementação ainda não examinada nem autoriza inferir que uma eventual escrita em exportada esteja coberta. ([BACKLOG.md:2117](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/docs/BACKLOG.md:2117), [BACKLOG.md:2118](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/docs/BACKLOG.md:2118); arquivo:linha dos anexos [não verificado].)

### Item 2 — Medir a espera

1. **Premissas — CONFERE quanto à existência da função; contagem [não verificado].** O estado é calculado pela função real, e a guarda examina divergências em metades pertencentes ao universo 163; a testemunha pode considerar o par que a validação gravaria. Isso não demonstra que o total de perguntas em espera seja 79, 163 ou qualquer outro número. ([materializacao.py:748](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:748), [materializacao.py:762](/home/ronald/codex_ws/e253f2bf7a505a1d16fc0e4ccdff5ca3ed74edf0/app/chamados/services/materializacao.py:762).)

2. **Colisão com aval vigente — CONFERE no texto fornecido.** A medição é pedida sem construir; não se verifica colisão textual com a alternância, o teto ou o prazo de pouso. [Arquivo:linha dos anexos não verificado.] [nao verificado]

3. **Condição literal — CONFERE com limite de evidência.** A relação entre as contagens é uma pergunta a medir, não um resultado já fornecido; o universo, a data e o resultado da medição efetiva são [não verificado]. [Anexo COMPLEMENTO-1: arquivo:linha não verificado.]

4. **Exportada, dinheiro ou ! — CONFERE no escopo de leitura declarado.** A medição pedida não autoriza construção; a execução concreta e seu acesso aos dados são [não verificado]. [Anexo COMPLEMENTO-1: arquivo:linha não verificado.]

### Item 3 — Registrar a substituição de ordem

1. **Premissas — CONFERE no texto fornecido.** DECLARADO-NA-VAGA contém uma ordem de construtores; o item 3 declara sua substituição apenas para DECLARADO-X-GRAVADO e mantém JUSTIFICATIVA-PAGA-O-PERIODO e HJ-091 conforme RETOMADA-DAS-13. O conteúdo desta última é [não verificado]. [Arquivo:linha dos anexos não verificado.]

2. **Colisão com aval vigente — PROCEDE na medida em que herda a pendência do item 1.** A substituição nominal da ordem está declarada, mas registrar a nova ordem como operacional depende de resolver o encaixe da entrada antecipada da (b) na alternância da vaga 2. [Anexos COMPLEMENTO-1 e RAIA-INTERVALO: arquivo:linha não verificado.]

3. **Condição literal — PROCEDE pela mesma pendência de sequência.** A posição exata da (b), da (a) e da espúria seguinte não fecha sem interpretação adicional; as posições concretas atribuídas pela RETOMADA-DAS-13 são [não verificado]. [Arquivo:linha dos anexos não verificado.]

4. **Exportada, dinheiro ou ! — CONFERE no texto fornecido.** O item pede registro de ordem; não declara aplicação sobre dados, exportação ou pagamento. Os efeitos de qualquer execução posterior são [não verificado]. [Anexo COMPLEMENTO-1: arquivo:linha não verificado.]

_7062 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
