# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 19:53** · publicado em **27/11/2026 19:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 27/11 19:01 · sessao c3b509094378 · FERIADO-UM-JUIZ · verificador · origem=Code

# REPROVADO

1. **O c9 ainda reverte um fechamento válido.** Reproduzi em memória um dia 6x1 com batidas `07:02 E`, `12:59 S`, `13:53 E`. O motor reconheceu **357 min** do par fechado e manteve o turno aberto. O oráculo devolveu `{}` porque o segmento aberto fica sem saída e o núcleo só soma segmentos fechados. No c9 e na base imediata `8639de6ca`, o fechamento retornou `0` e reverteu o mensal e a lavra `motor`. Com a lavra de `7706835c4` carregada em memória, retornou `1` e gravou **5,95 h** nas duas versões. Acrescentei um dia fechado à mesma competência. O oráculo descreveu apenas esse segundo dia, e o c9 gravou as **5,95 h** do primeiro sem declaração do oráculo para ele. A decisão sobre o primeiro dia depende, portanto, de haver outra data no mapa da competência. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:131` `app/ponto/motor_calculo_v2.py:2662` `app/ponto/calculador/alimentacao.py:83` `app/ponto/calculador/nucleo.py:142` `app/ponto/services/dia_pago.py:258` `app/ponto/services/dia_pago.py:322` `app/ponto/services/fechamento.py:1329` `7706835c4:app/ponto/services/dia_pago.py:311`
2. **A exceção literal de pontualidade pode divergir do catálogo e os REDs atuais deixam essa divergência passar.** Na cópia, a exclusão concorda com os dois nomes emitidos por `do_dia`. A política para `{}` está escrita separadamente em `lavrar`. `SEM_ENTRADA` declara ausência de insumo numa chamada de `do_dia`, e `motivo_nao_julga_pontualidade` responde sobre famílias de motor. Nenhum dos dois define os campos dispensados quando o mapa inteiro vem vazio. `horas_atraso` só é preenchido *depois* da guarda, então hoje o literal tem efeito apenas para `horas_saida_antecipada`. Troquei a seleção em memória por apenas `horas_trabalhadas`. Os **46 testes** do controle mais a asserção de B2 conservaram o mesmo resultado, inclusive a falha esperada na borda. O mutante retirou oito campos da guarda sem ser detectado pelos REDs. `app/ponto/services/dia_pago.py:184` `app/ponto/services/dia_pago.py:258` `app/ponto/services/dia_pago.py:270` `app/ponto/calculador/regras.py:527` `app/ponto/calculador/regras.py:608` `app/ponto/calculador/regras.py:627` `app/ponto/calculador/regras.py:939` `app/ponto/tests/test_feriado_um_juiz.py:248` `app/ponto/tests/test_feriado_um_juiz.py:292` `CLAUDE.md:28`

## B1 e B2 do ver6

- **B1 curado no caso original.** Com escala `07h–15h` e batidas `06h–06h30`, o resultado foi `{}`, zero trabalhado e **420 min de saída antecipada** nas duas lavras. O fechamento retornou `1`. A base imediata e o mutante que recoloca saída antecipada na guarda retornaram `0`. `app/ponto/tests/test_feriado_um_juiz.py:248` `app/ponto/tests/test_feriado_um_juiz.py:284` `app/ponto/tests/feriado_um_juiz_mutantes.py:121` `8639de6ca:app/ponto/services/dia_pago.py:259`
- **B2 curado na asserção.** Executei a própria asserção do teste sobre as chamadas do serviço real. Julho e agosto de 2026 receberam `propagar_falha=True`. A asserção antiga falhou com o argumento a mais, e o mutante que retira o argumento falhou com o argumento faltando. O teste ORM completo está [nao verificado]. `app/ponto/tests/test_revisar_par_relampago.py:153` `app/ponto/tests/test_revisar_par_relampago.py:163` `app/ponto/services/fechamento.py:1892` `app/ponto/tests/feriado_um_juiz_mutantes.py:97` `app/ponto/tests/feriado_um_juiz_mutantes.py:124`
- **Provas executadas.** Dois REDs c9 verdes e duas falhas de asserção esperadas com as fontes da base imediata carregadas em memória. Na base c7, os quatro REDs deram cinco falhas esperadas. Os 13 mutantes reexecutados morderam. O controle teve 45 testes, com uma falha esperada na borda 20/21, e os 41 estruturais passaram. Usei os arquivos das cópias locais para repor as bases em memória. A identidade delas pelo Git está [nao verificado]. `app/ponto/tests/feriado_um_juiz_mutantes.py:120` `app/ponto/tests/feriado_um_juiz_mutantes.py:165` `app/ponto/tests/feriado_um_juiz_mutantes.py:407` `app/ponto/tests/feriado_um_juiz_mutantes.py:500` `app/ponto/tests/feriado_um_juiz_mutantes.py:520` `app/ponto/tests/feriado_um_juiz_mutantes.py:568` `app/ponto/tests/test_feriado_um_juiz.py:786`

## Campos e procedência

| Nome | Quem declara, quem lê e com qual significado |
|---|---|
| `regras.RUBRICAS` | `regras.py` declara o catálogo, que inclui campos mensais e rubricas condicionais. `do_dia` usa o catálogo para iniciar `valores`, o `diff_calculador` monta seu universo de comparação e `lavrar` deriva os campos que exigem procedência quando recebe `{}`. Catálogo e procedência efetiva têm alcances diferentes, como demonstra o bloqueio 1. `app/ponto/calculador/regras.py:506` `app/ponto/calculador/regras.py:748` `app/ponto/management/commands/diff_calculador.py:79` `app/ponto/services/dia_pago.py:263` |
| `regras.NAO_DECIDE` | `regras.py` declara falta, DSR e banco como rubricas de outra fonte. `do_dia` copia a declaração e acrescenta causas da chamada. O DIFF exclui as declarações do módulo e registra as por dia. A guarda vazia lê a constante. Com mapa preenchido, `_DONOS_DECLARADOS` aceita `Rubricas.nao_decide` como declaração de dono. Confirmei em chamada direta que `horas_falta` passa e grava zero sem `faltas_por_dia`, embora o contrato da função exija o mapa. O fechamento passa esse mapa explicitamente. `app/ponto/calculador/regras.py:532` `app/ponto/calculador/regras.py:709` `app/ponto/management/commands/diff_calculador.py:79` `app/ponto/management/commands/diff_calculador.py:508` `app/ponto/management/commands/diff_calculador.py:599` `app/ponto/services/dia_pago.py:50` `app/ponto/services/fechamento.py:1313` |
| `do_motor` | O chamador pode declarar campos cujos valores vêm dos períodos. As duas chamadas de produção usam o padrão `()`. `_DONOS_DECLARADOS` aceita os campos, a guarda vazia os exclui e a sobreposição de `valores` os preserva. O significado é consistente nesses três usos. `app/ponto/services/dia_pago.py:69` `app/ponto/services/dia_pago.py:140` `app/ponto/services/dia_pago.py:263` `app/ponto/services/dia_pago.py:339` `app/ponto/services/fechamento.py:1235` `app/ponto/services/fechamento.py:1309` |
| `CAMPOS_DIA` | `dia_pago.py` declara o esquema de 15 campos de dia. A lavra usa o esquema para inicializar linhas, validar procedência e limitar a sobreposição. `soma_do_periodo` usa os campos para somar o gravado, e o relatório usa o mesmo esquema para montar o objeto de exibição. As unidades conferem com o modelo, em horas para `horas_*` e minutos para `minutos_*`. `app/ponto/services/dia_pago.py:34` `app/ponto/services/dia_pago.py:166` `app/ponto/services/dia_pago.py:323` `app/ponto/services/dia_pago.py:339` `app/ponto/services/dia_pago.py:797` `app/relatorios/views.py:547` `app/relatorios/views.py:586` `app/ponto/models.py:325` `app/ponto/models.py:345` |

**Perda silenciosa com `{}`.** Nenhuma demonstrada nas exclusões atuais. Conferi que a saída antecipada fica na linha e que atraso líquido, órfãos, falta, grade, DSR e banco entram pelas fontes do chamador. Com `{}`, nenhum `valores` é aplicado sobre esses números. O caso positivo demonstrado é o inverso, com rollback de horas já reconhecidas. Ocorrência na frota e perda em produção estão [nao verificado]. `app/ponto/services/dia_pago.py:184` `app/ponto/services/dia_pago.py:270` `app/ponto/services/dia_pago.py:296` `app/ponto/services/dia_pago.py:332` `app/ponto/services/dia_pago.py:355` `app/ponto/services/fechamento.py:1332`

## Outras classes de reprovação

- **Juiz, escritor, relógio e travas.** O c9 acrescenta uma decisão de procedência. O juiz de feriado registrado e a porta de gravação por versão são os mesmos. A guarda nova não lê relógio, e o relógio do teste em memória é fixado pela fixture. A revisão mantém Empresa por PK → Colaborador → Batida por PK, dentro do `atomic`. `app/core/juizes.py:318` `app/ponto/services/dia_pago.py:258` `app/ponto/services/dia_pago.py:369` `app/ponto/tests/test_feriado_um_juiz.py:237` `app/ponto/services/par_relampago_na_lista.py:172` `app/ponto/services/par_relampago_na_lista.py:479`
- **Import e `except`.** Há dependência recíproca entre `lavrar` e `regras.particionar_ft`, com imports locais. Forcei a lavra com `{}` em processos novos nas duas ordens de importação e ambas passaram. O diff tem **0 `except` adicionados e 0 removidos**, por comparação textual e AST nos quatro arquivos. O selo estrutural antigo do oráculo verifica o `try/except` separado. A prova de rollback vem dos REDs de comportamento. `app/ponto/services/dia_pago.py:260` `app/ponto/calculador/regras.py:386` `diff.txt:1` `app/ponto/tests/test_s5b_troca_ligada.py:98` `app/ponto/tests/test_feriado_um_juiz.py:43`

Suíte inteira, banco, logs, medição da frota, apply, reversão em produção e DIFF do TXT 11 [nao verificado]. [nao verificado]

## 27/11 18:54 · sessao 10767bc89811 · FERIADO-UM-JUIZ · construtor · origem=Code

Entreguei as duas correções da c9. O diff, o censo dos campos-fonte do oráculo, os chamadores da guarda e os comandos com seus códigos de retorno estão em [r.md](/home/ronald/wt-codex-feriado-um-juiz-152427/app/r.md:3).

- **B1.** O recálculo com `{}` e 420 minutos de saída antecipada retorna `1` e conserva o valor nas duas lavras em memória. O RED de 4 h trabalhadas com `{}` continua exigindo `ValueError`. [test_feriado_um_juiz.py:248](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:248), [test_feriado_um_juiz.py:292](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:292)
- **B2.** A asserção da revisão exige `propagar_falha=True` em julho e agosto de 2026. [test_revisar_par_relampago.py:163](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_revisar_par_relampago.py:163)

Os dois REDs falharam contra o c8 e passaram após a cura. Os dois mutantes novos produziram as falhas esperadas. Teste ORM completo, suíte inteira e ocorrência na frota [nao verificado]. [r.md:27](/home/ronald/wt-codex-feriado-um-juiz-152427/app/r.md:27) [nao verificado]

## 27/11 18:20 · sessao 07e6fd3fdca8 · FERIADO-UM-JUIZ · verificador · origem=Code

# REPROVADO

1. **A nova guarda do oráculo pode reverter um recálculo válido.** Reproduzi em memória um dia de trabalho 6x1 comercial, com escala 07h–15h, janela de HE ativa e batidas 06h–06h30. O motor recorta a entrada para 07h, apura zero trabalhado e conserva **420 min de saída antecipada**. O oráculo descarta o segmento invertido e devolve `{}`. A guarda inclui saída antecipada entre as rubricas que disparam a recusa. Com a lavra anterior, o recálculo devolveu `1` e gravou as duas versões. Com a atual, devolveu `0` e preservou o mensal e as duas lavras anteriores. A revisão recebeu `RuntimeError` com causa `ValueError`. A ocorrência na frota está [nao verificado]. `app/ponto/motor_calculo_v2.py:1813`, `app/ponto/motor_calculo_v2.py:3337`, `app/ponto/motor_calculo_v2.py:3423`, `app/ponto/calculador/alimentacao.py:114`, `app/ponto/calculador/alimentacao.py:512`, `app/ponto/services/dia_pago.py:259`, `app/ponto/services/dia_pago.py:316`, `app/ponto/services/dia_pago.py:868`, `app/ponto/services/fechamento.py:1329`
2. **A revisão quebrou uma asserção existente da suíte.** O teste `test_MORDE_restaurar_historica_reusa_anotar_e_post_save_com_trilha` compara os `kwargs` completos das duas competências e espera apenas `colaborador_ids`. Executei em memória a asserção original contra as chamadas da função atual. Ela falha nas duas por causa de `propagar_falha=True`. A execução do teste ORM completo está [nao verificado]. `app/ponto/tests/test_revisar_par_relampago.py:153`, `app/ponto/tests/test_revisar_par_relampago.py:163`, `app/ponto/services/fechamento.py:1892`

## Ver5, itens 2 e 3

- **2, contagem e revisão curadas.** As duas capturas marcam rollback e guardam a exceção. `processados` só sobe depois da saída do `atomic` e quando nenhuma lavra falhou. O teste injeta falha em cada versão, confere o estado anterior e exige retorno `0`. Na revisão, exige também a exceção e a reversão da batida e da trilha. `app/ponto/services/fechamento.py:926`, `app/ponto/services/fechamento.py:1273`, `app/ponto/services/fechamento.py:1329`, `app/ponto/tests/test_feriado_um_juiz.py:43`, `app/ponto/tests/test_feriado_um_juiz.py:217`
- **3, o caso de 4 h sem procedência está curado.** O RED exige `ValueError` antes do `delete` e do `bulk_create`. O controle com zero pares e mapa vazio ainda grava o ajuste. O bloqueio 1 deste laudo é um caso adicional, em que o próprio produtor do mapa devolve `{}`. `app/ponto/tests/test_feriado_um_juiz.py:234`, `app/ponto/tests/test_feriado_um_juiz.py:250`, `app/ponto/services/dia_pago.py:316`
- **Mordida executada.** Foram 44 controles, com uma falha esperada na borda, e 41 testes estruturais verdes. Contra os arquivos da cópia `7706835c4`, os quatro REDs deram cinco falhas de asserção, duas delas nos subcasos da revisão. Os sete mutantes novos sem banco morderam: os dois de contagem devolveram `1`, os três de propagação deixaram de levantar, o de mapa vazio deixou de recusar e o de interpretar zero como falha abortou a restauração fora do universo. Os três mutantes c5 reancorados também morderam. Para outubro, executei o mutante na partição em memória: **5,5 h comuns → 5,5 h de HE100 feriado**. A ponta ORM desse teste, a suíte inteira e o blob intermediário `a8eec39b` estão [nao verificado]. `app/ponto/tests/feriado_um_juiz_mutantes.py:86`, `app/ponto/tests/feriado_um_juiz_mutantes.py:126`, `app/ponto/tests/feriado_um_juiz_mutantes.py:267`, `app/ponto/tests/feriado_um_juiz_mutantes.py:367`, `app/ponto/tests/feriado_um_juiz_mutantes.py:459`, `app/ponto/tests/test_he100_dia_sem_previsto.py:234`

## Base `59600163c`

Conferi os conteúdos dos arquivos na cópia local da base. A classificação **4a/4b/4c procede**. A identidade do commit pelo Git está [nao verificado]. [nao verificado]

| Item | Evidência na base |
|---|---|
| **4a — soma** | O mensal já arredondava as horas regulares antes de somar a FT incerta e arredondar de novo. Com 241 min em cada parcela, dá **8,04 h** no mensal e **8,03 h** na soma dos dias, arredondada ao final. `59600163c:app/ponto/services/fechamento.py:922`, `59600163c:app/ponto/services/fechamento.py:1007`, `59600163c:app/ponto/services/dia_pago.py:178`, `59600163c:app/ponto/services/dia_pago.py:264`, `59600163c:app/ponto/services/dia_pago.py:550` |
| **4b — estado após relavra** | A base já apagava e recriava as linhas do colaborador, competência e versão. `lavrado_em` já era `auto_now=True`. `59600163c:app/ponto/services/dia_pago.py:359`, `59600163c:app/ponto/models.py:380` |
| **4c — fotografia do TXT** | O POST já recalculava em leitura, classificava os objetos destacados e os entregava à montagem. O TXT já podia refletir uma apuração posterior à gravada. `59600163c:app/folha/views.py:214`, `59600163c:app/folha/views.py:275` |

## Chamadores e efeitos

- **`propagar_falha`.** O padrão é `False`. O único chamador que passa `True` é `recalcular_na_revisao`, que descarta o retorno. O serviço lê a opção após o `atomic` e no `except` externo. Um universo vazio retorna `0` e a restauração segue. A porta de `par_relampago_na_lista` envolve batida, trilha e recálculos da revisão no mesmo `atomic`, sob Empresa → Colaborador → Batida. A reabertura da porta de export vem depois. O RED em memória cobre as duas falhas de lavra e o caso fora do universo. `app/ponto/services/fechamento.py:510`, `app/ponto/services/fechamento.py:619`, `app/ponto/services/fechamento.py:1335`, `app/ponto/services/fechamento.py:1884`, `app/ponto/registro_batida.py:317`, `app/ponto/services/par_relampago_na_lista.py:172`, `app/ponto/services/par_relampago_na_lista.py:479`, `app/ponto/tests/test_feriado_um_juiz.py:49`
- **Outros erros agora abortam a revisão.** A opção alcança qualquer exceção que chegue ao `except` externo, inclusive de `get_or_create`, `save`, `update` ou saída do `atomic`. Antes o serviço registrava e seguia. As capturas internas de grade e causa continuam degradando os campos. Erros de apuração anteriores ao `try` já escapavam. A view de revisão chama a porta sem capturar o `RuntimeError`. `7706835c4:app/ponto/services/fechamento.py:1342`, `app/ponto/services/fechamento.py:819`, `app/ponto/services/fechamento.py:969`, `app/ponto/services/fechamento.py:1117`, `app/ponto/services/fechamento.py:1179`, `app/ponto/services/fechamento.py:1196`, `app/ponto/services/fechamento.py:1339`, `app/ponto/views.py:3497`
- **Quem lê o retorno.** A tela, o comando de recálculo e os scripts de relavra consomem a contagem menor quando há rollback. O recálculo por evento descarta o retorno e continua após o log da lavra. Em leitura, o retorno segue sendo um mapa por colaborador, consumido por fechamento lido, TXT, simulação, Pauta DP, DIFF de frota, apuração de furos e backfill. No backfill, a entrada é posta no mapa **antes** da lavra. Reproduzi o comando subjacente devolvendo um colaborador lido com ambas as lavras revertidas. `app/ponto/views.py:1280`, `app/ponto/management/commands/recalcular_fechamento.py:130`, `bin/relavrar_o119_col221.py:52`, `app/ponto/services/fechamento.py:1846`, `app/ponto/services/fechamento.py:1170`, `app/ponto/services/fechamento.py:1348`, `app/ponto/services/fechamento.py:1798`, `app/folha/views.py:215`, `app/folha/management/commands/simular_folha.py:130`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`, `app/ponto/services/diff_frota.py:84`, `app/ponto/management/commands/apurar_furo_da_exportada.py:111`, `app/ponto/management/commands/lavrar_dias_pagos.py:104`
- **Quem fornece `{}` com minutos.** No código da cópia, o chamador de produção é a segunda lavra de `recalcular_fechamento_mes`, da 10/2026 em diante, via `_rub_o`. A primeira lavra usa o padrão `rubricas=None`. O comando de recálculo e o backfill de aberta chegam à segunda lavra. O backfill de exportada usa `retratar_exportada`. Os mapas de atraso, órfãos, faltas, grade e o ajuste entram depois da checagem, então valores vindos só desses insumos preservam o caminho de mapa vazio. `app/ponto/services/fechamento.py:1235`, `app/ponto/services/fechamento.py:1295`, `app/ponto/services/fechamento.py:1320`, `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:259`, `app/ponto/services/dia_pago.py:352`, `app/ponto/management/commands/lavrar_dias_pagos.py:104`, `app/ponto/management/commands/lavrar_dias_pagos.py:123`

## Classes de reprovação e histórico

- **Segundo juiz, segundo escritor, relógio, derivação paralela, trava e campo com dois significados.** Nenhuma dessas classes foi introduzida no delta. A dobra continua no juiz registrado, a gravação segue pelas portas existentes, a guarda consulta rubricas já acumuladas e o caminho de revisão mantém a ordem de trava. O c8 elimina a interpretação ambígua de `0` como falha. O problema de teste encontrado é a asserção desatualizada do bloqueio 2. `app/core/juizes.py:318`, `app/ponto/services/dia_pago.py:259`, `app/ponto/services/dia_pago.py:365`, `app/ponto/services/fechamento.py:679`, `app/ponto/services/fechamento.py:1335`, `app/ponto/services/fechamento.py:1892`, `app/ponto/services/par_relampago_na_lista.py:172`, `app/ponto/tests/test_revisar_par_relampago.py:163`
- **Borda e pouso.** A borda 20/21 com troca 6x1 → 12x36 continua perdendo 4 h em memória nos dois cenários. A falha veio da asserção do `expectedFailure`. Pela triagem, segue na fila do motor. Pouso, ordem E1 → K1, medição dos 75 dias/42 colabs, corte dos intermitentes, apply, reversão, antes × depois e DIFF do TXT 11 estão [nao verificado]. `app/ponto/tests/test_feriado_um_juiz.py:728`, `app/ponto/motor_calculo_v2.py:2513`, `triagem_ver5.txt:1`, `app/docs/RELATO.md:252`, `app/docs/PROMPTS.md:3016`, `FERIADO-UM-JUIZ-2711-1210.txt:1`
- **`except` e S5b.** **0 cláusulas adicionadas, 0 removidas**, tanto no diff literal quanto por AST, inclusive nos testes. Mudou o corpo dos três handlers existentes. O comentário encurtado preserva o aval, a vigência desde a 10, a chave por versão, a porta única e a ordem em relação à S3. Saíram a afirmação histórica geral de que nenhum leitor lia `oraculo` e o exemplo da porta irmã rejeitada. O contrato e o isolamento por versão continuam expressos. `diff.txt:1`, `7706835c4:app/ponto/services/fechamento.py:1278`, `app/ponto/services/fechamento.py:1273`, `app/ponto/services/fechamento.py:1279`, `app/ponto/services/fechamento.py:1329`, `app/ponto/services/dia_pago.py:365`, `app/ponto/models.py:386`

## 27/11 18:13 · sessao ba609a2fce97 · FERIADO-UM-JUIZ · construtor · origem=Code

## FERIADO-UM-JUIZ c8

A revisão volta a restaurar a batida e registrar a trilha quando o colaborador está fora do universo da competência. As duas lavras registram a exceção capturada e marcam o rollback. Após sair do `atomic` do colaborador, `propagar_falha=True` faz a revisão receber um `RuntimeError` com a causa original. A revisão é o único chamador que ativa essa opção. A contagem continua disponível para a tela e o comando. `ponto/tests/test_feriado_um_juiz.py:57` · `ponto/services/fechamento.py:513,1277,1334,1342,1350,1892`

| Sítio | RED | Mutante |
|---|---|---|
| Revisão fora do universo · `ponto/services/fechamento.py:619,1892` | Na c7 `a8eec39b3`, a restauração aborta com `RuntimeError`. Na c8, a batida e a trilha são registradas, sem escrita de mensal ou lavra. `ponto/tests/test_feriado_um_juiz.py:57` | `c8_revisao_usa_zero` recoloca `== 0` e reproduz o aborto. `ponto/tests/feriado_um_juiz_mutantes.py:87` |
| Falha real na lavra do motor ou do oráculo · `ponto/services/fechamento.py:1277,1334` | As duas injeções exigem rollback do mensal, das lavras, da restauração e da trilha. `ponto/tests/test_feriado_um_juiz.py:49,224` | `c8_falha_lavra_nao_propaga`, `c8_falha_colab_nao_propaga` e `c7_revisao_ignora_contagem` fazem os dois subcasos perderem o `RuntimeError`. `ponto/tests/feriado_um_juiz_mutantes.py:94,97,106` |
| Contagem e rollback nos demais chamadores · `ponto/services/fechamento.py:1276,1333,1335` | Cada lavra injetada mantém o estado anterior e devolve `0` na chamada comum. `ponto/tests/test_feriado_um_juiz.py:43,46,224` | `c7_motor_conta_falha` e `c7_oraculo_conta_falha` voltam a contar `1`. `c5_motor_sem_sinal_de_rollback` e `c5_oraculo_sem_sinal_de_rollback` deixam gravações parciais na simulação. `ponto/tests/feriado_um_juiz_mutantes.py:100,103,128,132` |

## Censo dos chamadores

As 22 chamadas do aplicativo, incluindo as importações com alias, estão abaixo. Os endereços são relativos a `app/`.

| Chamador | Uso do retorno |
|---|---|
| `ponto/services/fechamento.py:1892` | Revisão. Descarta a contagem e ativa a propagação de falha. |
| `ponto/services/fechamento.py:1849` | Evento após commit. Descarta a contagem e registra o evento. |
| `ponto/services/fechamento.py:1798` | `fechamento_lido`. Devolve o objeto destacado do colaborador via `.get()`. |
| `ponto/views.py:1280` | Exibe a contagem na resposta da tela. |
| `ponto/management/commands/recalcular_fechamento.py:130` | Imprime a contagem e inclui o valor no JSON. |
| `folha/views.py:215` | Usa os objetos destacados para classificar e montar o TXT. |
| `folha/management/commands/simular_folha.py:123,130` | `--recalcular` descarta a contagem. `--lido` usa os objetos destacados para classificar e montar o TXT. |
| `pautas/management/commands/pauta_pago_x_devido_exportada.py:135` | Busca o devido por colaborador e recusa o ato se faltar alguma apuração pedida. |
| `ponto/services/diff_frota.py:84,87` | Incorpora os dicionários de leitura às fotos `hoje` e `novo`. |
| `ponto/management/commands/apurar_furo_da_exportada.py:111` | Imprime `len(proc)` e usa o dicionário de saída `coletar_furos` para a apuração. |
| `ponto/management/commands/lavrar_dias_pagos.py:106` | Soma e imprime a quantidade de entradas no dicionário de leitura. |
| `ponto/management/commands/diff_calculador.py:131` | Descarta o dicionário e usa as lavras da captura. |
| `ponto/portas/regen_exportada.py:184` | Descarta a contagem. |
| `ponto/management/commands/aplicar_09_corte_b.py:212`, `ponto/management/commands/celula_veredito_velho.py:149`, `ponto/management/commands/desvio_o68b.py:94`, `ponto/management/commands/diff_reclassificar_partido.py:161,206`, `ponto/management/commands/folga_que_sumiu.py:70`, `ponto/management/commands/plano_b_no_dinheiro.py:110` | Descartam a contagem. |

**Efeito da c7 em outros chamadores.** A tela e o comando passaram a publicar uma contagem que exclui o colaborador cuja lavra falhou. Os chamadores que descartam a contagem não tomam decisão sobre esse valor. No modo leitura com lavra, o dicionário é preenchido antes da tentativa de escrita, de modo que a entrada ainda pode compor o total de **lidos**. A c7 também passou a recusar a lavra do oráculo quando há minutos em rubricas de dinheiro sem a declaração de procedência exigida. Ocorrência desses casos na frota **[nao verificado]**. `7706835c4:app/ponto/services/fechamento.py:1276,1340,1341` · `a8eec39b3:app/ponto/services/fechamento.py:1276,1340,1341` · `ponto/views.py:1281` · `ponto/management/commands/recalcular_fechamento.py:134,144` · `ponto/services/fechamento.py:1171,1227,1349` · `ponto/management/commands/lavrar_dias_pagos.py:108` · `ponto/services/dia_pago.py:259,316`

### Chamadas nos testes [nao verificado]

- **Contagem verificada** em `ponto/tests/test_fechamento_escopo_colabs.py:52`, `ponto/tests/test_feriado_um_juiz.py:222` e `ponto/tests/test_o80_selo_l092.py:103,113`.
- **Dicionário de leitura inspecionado** em `pautas/tests/test_pauta_pago_x_devido_exportada.py:113`, `ponto/tests/test_bob_b2_abono_feriado_guarda.py:208`, `ponto/tests/test_feriado_um_juiz_porta.py:167,262` e `ponto/tests/test_o80_selo_l092.py:162`.
- **Contagem repassada por helper e descartada pelos testes** em `ponto/tests/test_ferias_dia21.py:124,212`. A chamada real instrumentada por `wraps` em `ponto/tests/test_guarda_10_b.py:126` verifica os argumentos e deixa o comando consumir a contagem em `ponto/management/commands/recalcular_fechamento.py:130`.
- **Retorno descartado** em `chamados/tests/test_o317_passivo_pela_porta.py:629,684`, `colaboradores/tests/test_calendario_badge_minutos.py:68`, `colaboradores/tests/test_calendario_carimbo_intra.py:90`, `colaboradores/tests/test_calendario_le_dia_pago.py:159`, `escala/tests/test_escala_certa_deterministica.py:85`, `pautas/tests/test_pauta_pago_x_devido_exportada.py:61,336`, `relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:408`, `relatorios/tests/test_pdf_linha_do_dia_e_o_pago.py:69`, `relatorios/tests/test_pdf_realizado_do_dia.py:104,133` e `relatorios/tests/test_s3_cartao_le_dia_pago.py:87,424`.
- **Retorno descartado nos testes de ponto** em `ponto/tests/test_abono_dia_inteiro_zera_pontualidade.py:72`, `ponto/tests/test_abono_parcial_abate_a_ponta.py:87`, `ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:604`, `ponto/tests/test_bob_b2_abono_feriado_guarda.py:111`, `ponto/tests/test_credito_no_proprio_dia.py:63`, `ponto/tests/test_dia_pago_soma.py:66`, `ponto/tests/test_dupla_contagem_dia20.py:39,40`, `ponto/tests/test_fechamento_ausencia.py:190`, `ponto/tests/test_fechamento_escopo_competencia.py:30,47,59,69,80`, `ponto/tests/test_feriado_um_juiz_porta.py:65,270,494,525`, `ponto/tests/test_h12h06b_parcial_pelo_juiz.py:138`, `ponto/tests/test_he100_dia_sem_previsto.py:189`, `ponto/tests/test_o140_furo_lavrado.py:363`, `ponto/tests/test_o280_bateria_da_lei.py:1751`, `ponto/tests/test_o48_fechamento_lido.py:120`, `ponto/tests/test_o48p6_projecao_carimbada.py:83`, `ponto/tests/test_o80_selo_l092.py:122,136,147,179,193`, `ponto/tests/test_o85_buraco_de_vinculo.py:78,155`, `ponto/tests/test_recalculo_ignora_carimbo.py:32,38`, `ponto/tests/test_revisar_par_relampago.py:217` e `ponto/tests/test_semselo5_causa_espelho_pelo_vinculo_da_janela.py:60`.

## Arquivos e comandos

- Cura com os handlers existentes. Condensei o comentário da S5b para preservar os endereços congelados pela catraca. [ponto/services/fechamento.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/fechamento.py:1277) · `core/tests/test_medida_turno_do_par.py:565`
- RED da restauração fora do universo, com instante fixo. [ponto/tests/test_feriado_um_juiz.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:57)
- Reprodução na base e mutantes em memória. [ponto/tests/feriado_um_juiz_mutantes.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:87)

Comandos executados a partir de `app/`. `base_c8` carrega em memória o fechamento da c7 e exige o erro exato. `controle` rodou 44 testes com uma falha esperada. Os 41 estruturais passaram e cada mutante abaixo produziu a falha prevista. `ponto/tests/feriado_um_juiz_mutantes.py:367,459,515,518`

```bash
export PYTHONDONTWRITEBYTECODE=1
python3 -m ponto.tests.feriado_um_juiz_mutantes base_c8
python3 -m ponto.tests.feriado_um_juiz_mutantes controle
python3 -m ponto.tests.feriado_um_juiz_mutantes estruturais
python3 -m ponto.tests.feriado_um_juiz_mutantes c8_revisao_usa_zero
python3 -m ponto.tests.feriado_um_juiz_mutantes c8_falha_lavra_nao_propaga
python3 -m ponto.tests.feriado_um_juiz_mutantes c8_falha_colab_nao_propaga
python3 -m ponto.tests.feriado_um_juiz_mutantes c7_revisao_ignora_contagem
python3 -m ponto.tests.feriado_um_juiz_mutantes c7_motor_conta_falha
python3 -m ponto.tests.feriado_um_juiz_mutantes c7_oraculo_conta_falha
python3 -m ponto.tests.feriado_um_juiz_mutantes c5_motor_sem_sinal_de_rollback
python3 -m ponto.tests.feriado_um_juiz_mutantes c5_oraculo_sem_sinal_de_rollback
```

Suíte completa, execução ORM e certificação pelo Code e pelo Opus final **[nao verificado]**. [nao verificado]

## 27/11 18:04 · sessao b3655e2c03f2 · FERIADO-UM-JUIZ · construtor · origem=Code

## FERIADO-UM-JUIZ c7

| Bloqueio | Sítio da cura ou classificação | RED e mutante |
|---|---|---|
| **1. Borda 20/21 com troca 6x1 → 12x36** | Registrei o `expectedFailure` com `PAREI ! FERIADO-L097-MOTOR`. Ao executar a asserção sem o marcador, a competência 12 ficou vazia nos dois cenários. `ponto/tests/test_feriado_um_juiz.py:683` | RED em `ponto/tests/test_feriado_um_juiz.py:699`, executado por `controle`. |
| **2. Falha de lavra** | Os dois handlers marcam rollback e pulam a contagem. O retorno `0` faz `recalcular_na_revisao` levantar `RuntimeError`. O erro sai pelo caminho existente de `restaurar_batida` e desfaz o `atomic` da revisão. `ponto/services/fechamento.py:1272`, `:1335`, `:1892`, `ponto/registro_batida.py:319`, `ponto/services/par_relampago_na_lista.py:479` | Na c6, as duas injeções devolveram `1` e a restauração seguiu sem erro. Os três mutantes c7 de contagem e revisão reproduziram essas falhas. `ponto/tests/test_feriado_um_juiz.py:43`, `:49`, `:187`, `ponto/tests/feriado_um_juiz_mutantes.py:78` |
| **3. Oráculo com `rubricas={}`** | Períodos com minutos acionam a validação de procedência mesmo com mapa vazio. Sem pares, o ajuste continua sendo lavrado. `ponto/services/dia_pago.py:259`, `:316`, `ponto/tests/test_feriado_um_juiz.py:205` | Na c6, o `bulk_create` simulado recebeu uma linha diária de 4 h e o ajuste. O RED agora exige `ValueError` antes de qualquer escrita. O mutante que restaura `if rubricas:` reproduziu a falha. `ponto/tests/test_feriado_um_juiz.py:189`, `ponto/tests/feriado_um_juiz_mutantes.py:91` |
| **4a. Mensal 8,04 h × lavra 8,03 h** | **Pré-existente em `59600163c`.** O mensal arredondava as horas regulares antes de somar a FT sem escala certa. A lavra acumulava os minutos dos dois períodos sem arredondar por dia. `59600163c:app/ponto/services/fechamento.py:922`, `:1007`, `59600163c:app/ponto/services/dia_pago.py:178`, `:264`, `:550` | Controle de 8,04 h preservado em `ponto/tests/test_feriado_um_juiz_porta.py:421`. |
| **4b. Relavra recria linhas e carimbos** | **Pré-existente em `59600163c`.** A lavra já usava `delete` e `bulk_create`, o `DiaPago` já tinha `lavrado_em` automático e o mensal já carimbava `derivado_em`. `59600163c:app/ponto/services/dia_pago.py:359`, `59600163c:app/ponto/models.py:380`, `59600163c:app/ponto/services/fechamento.py:1167`, `:1213` | Origem conferida no código da base. |
| **4c. POST do TXT lê apuração mais nova** | **Pré-existente em `59600163c`.** O POST já recalculava em modo leitura e passava os objetos recém-apurados à montagem. `59600163c:app/folha/views.py:214`, `:275` | Origem conferida no código da base. |
| **5. Portão da sessão** | Medição de frota, pouso, apply, reversão e DIFF do TXT **[nao verificado]**. | — | [nao verificado]

**Cobertura de outubro.** O teste adicionado leva 330 min de FT em feriado 6x1 com escala incerta, sob L-097, das batidas ao mensal e às duas lavras. Exige 5,5 h comuns. A fixture de novembro foi preservada. A execução ORM de outubro e seu mutante ficam **[nao verificado]**. `ponto/tests/test_he100_dia_sem_previsto.py:179`, `:234`, `:251`, `ponto/tests/feriado_um_juiz_mutantes.py:250`

**Execução local.** A base c6 apresentou cinco falhas de asserção em quatro testes. Na cópia, o controle rodou 43 testes com uma falha esperada e os 41 estruturais passaram. Os quatro mutantes c7 sem banco e os três c5 reancorados deram o resultado previsto. `ponto/tests/feriado_um_juiz_mutantes.py:77`, `:110`, `:349`, `:441`, `:489`

### Comandos recortados

RED na base, controle e estruturais. `ponto/tests/feriado_um_juiz_mutantes.py:349`, `:441`, `:489`

```bash
export PYTHONDONTWRITEBYTECODE=1
python3 -m ponto.tests.feriado_um_juiz_mutantes base_c7
python3 -m ponto.tests.feriado_um_juiz_mutantes controle
python3 -m ponto.tests.feriado_um_juiz_mutantes estruturais
```

Mutantes sem banco. `ponto/tests/feriado_um_juiz_mutantes.py:77`, `:110`

```bash
python3 -m ponto.tests.feriado_um_juiz_mutantes c7_motor_conta_falha
python3 -m ponto.tests.feriado_um_juiz_mutantes c7_oraculo_conta_falha
python3 -m ponto.tests.feriado_um_juiz_mutantes c7_revisao_ignora_contagem
python3 -m ponto.tests.feriado_um_juiz_mutantes c7_rubricas_vazias_dispensam_procedencia
python3 -m ponto.tests.feriado_um_juiz_mutantes c5_motor_sem_sinal_de_rollback
python3 -m ponto.tests.feriado_um_juiz_mutantes c5_oraculo_sem_sinal_de_rollback
python3 -m ponto.tests.feriado_um_juiz_mutantes c5_recusa_lavra_sem_rubricas
```

Recorte ORM para o Code. `ponto/tests/test_he100_dia_sem_previsto.py:234`, `ponto/tests/test_feriado_um_juiz_porta.py:462`, `ponto/tests/feriado_um_juiz_mutantes.py:250`

```bash
python3 manage.py test --settings=config.settings.ci \
  ponto.tests.test_he100_dia_sem_previsto.DiaSemPrevistoNoOraculoTest \
  ponto.tests.test_feriado_um_juiz_porta.FeriadoUmJuizPortaTest.test_RED_c5_falha_no_bulk_create_do_oraculo_preserva_as_tres_gravacoes
python3 -m ponto.tests.feriado_um_juiz_mutantes c7_classifica_feriado_em_outubro --settings=config.settings.ci
```

### Arquivos alterados

- `ponto/services/fechamento.py:1272`
- `ponto/services/dia_pago.py:259`
- `ponto/tests/test_feriado_um_juiz.py:43`
- `ponto/tests/test_he100_dia_sem_previsto.py:234`
- `ponto/tests/feriado_um_juiz_mutantes.py:77`

**`except` no diff:** 0 adicionados, 0 removidos. `ponto/services/fechamento.py:1272`, `:1335`  
**Suíte completa e certificação Code/Opus:** **[nao verificado]**. [nao verificado]

## 27/11 17:56 · sessao 92429ba11e11 · FERIADO-UM-JUIZ · verificador · origem=Code

# REPROVADO

1. **A combinação de borda 20/21 com troca de 6x1 para 12x36 perde 4 h.** Reproduzi em memória dois turnos fechados, 20/11 das 18h às 22h e 21/11 das 03h às 07h, com corte 21 e troca de ciclo no dia 21. Com feriado só no dia 20, a 11 recebe 4 h de HE100 feriado e a 12 fica vazia. Com feriado só no dia 21, a 11 recebe 4 h comuns e a 12 também fica vazia. Restaurar apenas o filtro anterior, que olhava só o dia 21, conserva as 8 h no primeiro cenário, embora na janela errada. **Depende do motor:** o 6x1 retira o turno externo, o 12x36 pula o filtro e o motor herda a jornada da véspera e descarta o par antes da partição. Ocorrência na frota [nao verificado]. (`app/ponto/tests/test_feriado_um_juiz.py:560–623`, `app/ponto/services/fechamento.py:155–173,340–351`, `app/ponto/motor_calculo_v2.py:2513–2545`)
2. **Falha de lavra agora desfaz o mensal, mas o recálculo ainda confirma o colaborador como processado.** Nas duas injeções em memória, o estado anterior foi preservado e o retorno foi `1`. A tela e o comando publicam essa contagem. A revisão administrativa ignora o retorno e pode confirmar a batida restaurada com a projeção antiga, embora a porta da revisão prometa desfazer o ato se o recálculo falhar. (`app/ponto/tests/test_feriado_um_juiz.py:43–47,138–145`, `app/ponto/services/fechamento.py:1272–1277,1335–1350,1869–1891`, `app/ponto/views.py:1280–1281`, `app/ponto/management/commands/recalcular_fechamento.py:130–145`, `app/ponto/registro_batida.py:305–321`)
3. **`rubricas={}` desarma a validação de procedência também quando há dinheiro no dia.** Em memória, `lavrar(versao='oraculo', periodos=[par_de_4h], rubricas={})` entregou ao `bulk_create` uma linha diária de 4 h vindas do motor e a linha de ajuste. O RED novo só verifica o ajuste com zero pares. O alcance desse caso em produção está [nao verificado]. (`app/ponto/services/dia_pago.py:164–185,307–362`, `app/ponto/calculador/alimentacao.py:512–527,571–598`, `app/ponto/tests/test_feriado_um_juiz.py:147–169`)
4. **As propriedades de soma, estado e leitura ainda falham.** O mensal sem promoção conserva 8,04 h para dois pares de 241 min, cuja soma diária dá 8,03 h. A relavra recria linhas e carimbos, e o POST do TXT pode ler uma apuração mais nova que o mensal e a lavra gravados. O `atomic` assegura a reversão conjunta das escritas, mas essas propriedades continuam abertas. (`app/ponto/tests/test_feriado_um_juiz_porta.py:396–433`, `app/ponto/services/fechamento.py:1011–1019,1172–1225`, `app/ponto/services/dia_pago.py:334–362,552–565`, `app/ponto/models.py:373–387`, `app/folha/views.py:214–219,269–275`)
5. **Pouso e apply seguem no portão da sessão.** A medição anterior dos 75 dias de 42 colaboradores como hora comum, o corte efetivo dos intermitentes, a reversão, o antes × depois de horas e rubricas por colaborador, o DIFF do TXT 11 e o pouso estão [nao verificado]. Para **K1**, o registro exige E1 antes e DIFF de frota por rubrica antes do pouso. O cumprimento dessa ordem está [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `triagem_ver4.txt:1`, `app/docs/RELATO.md:252,255`, `app/docs/PROMPTS.md:3016,3032`, `CLAUDE.md:453–467`)

## Achados do ver4

| Achado | Resultado |
|---|---|
| **1. K1, turno partido** | **Fora.** A triagem remete a fatia própria. **Depende do K1 e do motor.** O motor apura o suprimido no laço dos períodos regulares, a promoção soma os minutos da FT, e o teste de 440 min de HE100 feriado + 30 min de intra continua `expectedFailure`. (`triagem_ver4.txt:1`, `app/docs/RELATO.md:255`, `app/ponto/motor_calculo_v2.py:3078–3118`, `app/ponto/services/fechamento.py:999–1005`, `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394`) |
| **2. L-097 e certeza da escala** | **Fora.** A triagem registra `PAREI ! FERIADO-L097-MOTOR`. **Depende do motor:** sem autorização, o ramo com previsto zero pode zerar a FT com escala certa antes da partição. O teste que exige 4 h em ambas as rubricas continua em `skip`. (`triagem_ver4.txt:1`, `app/docs/LEIS.md:115`, `app/ponto/motor_calculo_v2.py:1562–1597`, `app/ponto/tests/test_feriado_um_juiz_porta.py:538–554`) |
| **3. Regravação de exportada** | **Fora.** A triagem remete à LEI-EXPORTADA-B-2511/L-113 e ao FOCO-COMP11. A lei permite recálculo genérico fora da guarda, com histórico do TXT, DIFF prévio ao ato de frota e aviso ao DP quando houver mudança. Preservação das exportadas no apply desta obra [nao verificado]. (`triagem_ver4.txt:1`, `app/docs/LEIS.md:155–156`, `app/docs/PROMPTS.md:3032`, `app/ponto/services/fechamento.py:600–639`) |
| **4. Borda, data e vínculo** | **Não curado.** O filtro passou a olhar os dois lados do corte. A partição passou a consultar o motor do dia do turno e impede que o 12x36 herde a dobra da origem 6x1. Os REDs mordem separadamente a borda sem troca e a troca dentro da mesma competência. O caso combinado do bloqueio 1 perde o par antes dessa correção. **Depende do motor.** (`app/ponto/services/fechamento.py:155–173,227–231`, `app/ponto/calculador/regras.py:410–432`, `app/ponto/tests/test_feriado_um_juiz.py:607–623`, `app/ponto/tests/feriado_um_juiz_mutantes.py:75–88`, `app/ponto/motor_calculo_v2.py:2513–2545`) |
| **5. Mensal e duas lavras atômicos** | **Curado quanto ao rollback das três gravações.** O `atomic` começa antes do `get_or_create`, inclui `save` ou `update` do mensal e as duas chamadas a `lavrar`; o oráculo só roda após a primeira lavra, da 10 em diante. Os dois REDs em memória mordem falha na primeira e na segunda. O teste ORM compara o estado completo para mensal aberto e aprovado, com execução [nao verificado]. O retorno incorreto permanece no bloqueio 2. (`app/ponto/services/fechamento.py:925–929,967–971,1172–1228,1272–1277,1301–1340`, `app/ponto/tests/test_feriado_um_juiz.py:43–145`, `app/ponto/tests/test_feriado_um_juiz_porta.py:462–499`) |
| **6. Três selos estruturais** | **Curado.** A catraca atualizou os cinco endereços. Os dois selos do dia passaram com a escrita no mapa seguida da leitura por `_dia_de`. Repor `505` fez cair uma asserção. Simular `dia_jornada = dia_turno` fez cair duas. (`app/core/tests/test_medida_turno_do_par.py:289,329–334,565–569`, `app/ponto/calculador/regras.py:424–431`, `app/ponto/tests/test_noturno_v8_ft_no_dia_da_lavra.py:33–71`, `app/ponto/tests/feriado_um_juiz_mutantes.py:325–346`) |
| **7. Pouso e apply** | **Fora do aceite de código**, por triagem. Continua pendente no portão do bloqueio 5. (`triagem_ver4.txt:1`, `FERIADO-UM-JUIZ-2711-1210.txt:1`) | [nao verificado]

## Fixtures c6 e cobertura histórica

- **HE100 6x1.** A fixture de novembro exercita fechamento e ambas as lavras e exige 5,5 h em HE100 feriado. Outubro tem um RED de partição que exige hora comum para FT 6x1 incerta e um teste de porta com mensal e duas lavras. A combinação exata anterior de **330 min, 6x1 e L-097 em outubro** perdeu a cobertura de ponta a ponta. Em outubro, o caso de 330 min que ficou na mesma classe é 12x36. A fixture de porta da 10 também não prova emissão de uma linha histórica pelo POST. (`app/ponto/tests/test_he100_dia_sem_previsto.py:176–252`, `app/ponto/tests/test_feriado_um_juiz.py:396–435`, `app/ponto/tests/test_feriado_um_juiz_porta.py:239–296`)
- **C3b.** Setembro continua no teste. O modo histórico explícito preserva os três resultados exigidos: cláusula ligada paga, piso 12x36 afirma zero e atributo ausente deixa a rubrica sem decisão. O teste chama `rubricas_por_dia` diretamente. A escolha do modo pelo chamador fica fora dessa asserção. (`app/ponto/tests/test_o280_bateria_da_lei.py:1230,3100–3158`, `app/ponto/services/fechamento.py:601–603,1301–1312`)
- **59-A e E5.** O selo 59-A acompanha montagem e homologação até a cláusula histórica. A sonda em memória de outubro/12x36 confirmou supressão nas duas portas com a cláusula desligada e emissão/verificação com a cláusula ligada. E5 ajusta o limite do recorte textual à nova indentação e conserva as verificações de instância destacada e ausência de persistência. Nessas duas reancoragens, os controles mantiveram o comportamento cobrado. (`app/folha/tests/test_export_59a.py:19–78`, `app/folha/export.py:388–394,748–760,1018–1034`, `app/ponto/tests/test_e5_fechamento_como_leitura.py:35–60`)

## Quem escreve e quem lê

| Campo ou valor | Escrita e leitura |
|---|---|
| **`Batida.timestamp`, `tipo`, `data_turno`, `dia_da_jornada`** | `registrar_batida` persiste instante e tipo. O fechamento lê a data civil local para montar a busca. O juiz de turnos fornece `data_turno`, que o motor indexa pela entrada crua. O motor atribui o dia de jornada, com herança de até 6 h, e o fechamento mescla os mapas. A partição reescreve o mapa após o corte. Lavra, oráculo e DIFF o leem. `_dia_de` pode devolver data civil quando falta chave. São três valores distintos que podem acabar sob o mesmo campo `DiaPago.data`. (`app/ponto/registro_batida.py:118,170–179`, `app/ponto/services/fechamento.py:254–257,297–315,371–383`, `app/ponto/motor_calculo_v2.py:894–922,963–978,2513–2545`, `app/ponto/calculador/regras.py:410–428`, `app/ponto/services/dia_pago.py:109–132,339–346`, `app/ponto/calculador/alimentacao.py:353–388`, `app/ponto/management/commands/diff_calculador.py:140–152,244–257`) |
| **Competência, corte, vigência, ciclo/base e contexto** | O cadastro da empresa fornece `dia_inicio_competencia`. `janelas` traduz `(ano, mes)` em datas. A vigência vem de `EscalaColaborador`, cujo escritor de vínculo grava tipo e datas. A juíza `vinculo_do_dia` usa a geradora válida ou a vigência. A composição escreve `(motor, vínculo)` por dia e por identidade da FT. A partição lê os dois mapas, ciclo e base. O terceiro corte, `(2026, 11)`, sai de `folha/export.py` e é lido pelo fechamento, partição, emissão e DIFF. (`app/colaboradores/views.py:514–518,538–545`, `app/colaboradores/services/vinculo.py:287–299`, `app/ponto/janelas.py:36–63`, `app/escala/alimentacao.py:205–264`, `app/ponto/services/fechamento.py:337–351,600–603`, `app/ponto/calculador/regras.py:367–368,386–432`, `app/folha/export.py:383–394`, `app/ponto/management/commands/diff_calculador.py:112–152`) |
| **Feriado, dobra, certeza e cláusula** | O cadastro grava `Feriado`; `feriados_do_colab` carrega o calendário pela praça do vínculo. O motor lê calendário e `feriado_em_dobra`, alimentado pelo ciclo e pela régua CCT. A borda e a partição consultam esse método. `escala_certa_no_dia` lê vínculo e assinaturas da competência do dia. A partição usa esse resultado e `feriado_vira_folga` para escolher a rubrica. No histórico, o TXT ainda lê a cláusula 59-A pela régua da data final da competência. (`app/core/views_config.py:83`, `app/core/feriados.py:135–167`, `app/core/regua_cct.py:466–479`, `app/ponto/motor_calculo_v2.py:941–961`, `app/ponto/services/fechamento.py:159–167,789–806`, `app/escala/services/escala_certa.py:19–51`, `app/escala/models.py:1384–1406`, `app/ponto/calculador/regras.py:415–432`, `app/folha/export.py:363–394`) |
| **Horas trabalhadas, folga, HE50, HE100, feriado, splits e noturno** | O motor produz períodos, minutos legais e splits. `particionar_ft` produz as três listas e os minutos noturnos da promoção. O fechamento grava o mensal; `lavrar` grava os dias; a alimentação projeta a partição e `rubricas_por_dia` fornece os valores que a porta substitui no oráculo. TXT e cartão leem o mensal que recebem; calendário e vigia leem a lavra. `horas_extras_100_feriado` agrega feriado regular e FT promovida. `detalhe.folga_feriado_min` identifica só a promoção. `pago_do_dia` inclui parcelas que `horas_trabalhadas` deixa em rubricas próprias. (`app/ponto/motor_calculo_v2.py:303–344,1013–1049`, `app/ponto/calculador/regras.py:380–383,433–453`, `app/ponto/services/fechamento.py:984–1019,1195–1225`, `app/ponto/calculador/alimentacao.py:353–389,571–598`, `app/ponto/services/dia_pago.py:172–185,233–270,321–346,869–900`, `app/folha/export.py:434–466`, `app/relatorios/cartao_pela_celula.py:79–118`, `app/colaboradores/services/calendario.py:25–26,777–782`, `app/folha/porta_export.py:384–405,592–603`) |
| **Demais valores dentro do `atomic`** | O fechamento lê períodos, crédito parcial, faltas e grade. Grava atraso, saída antecipada, falta, intra, turnos abertos, inconsistências, semanas/reflexo DSR, banco, previsto/realizado/abono, dias previstos/incertos, motivos/causa e carimbos. A lavra lê os mapas, o furo do motor com teto e a impressão da régua da fatia, e grava dias e ajuste de DSR/banco, com versão e origem. O export lê rubricas, prontidão e retenção. O comando de comparação lê os campos de `CAMPOS`; o vigia lê soma, furo e impressão da régua. A leitura destacada também copia os fatos do mensal, como status e aprovação; `aprovar`/`aprovar_lote` escrevem esses carimbos. (`app/ponto/services/fechamento.py:337–339,942–971,1025–1045,1078–1171,1195–1271,1315–1334,1362–1380`, `app/ponto/services/dia_pago.py:187–229,267–290,334–363`, `app/folha/export.py:453–472,992,1047–1100`, `app/ponto/management/commands/aplicar_09_corte_b.py:67–74`, `app/folha/porta_export.py:290–292,384–443`) |

## Controles estruturais e propriedades

- **Juiz, porta e fonte.** A dobra continua consultando o F3 registrado e a lista de ciclos do motor. O problema de autoridade está no **dia**: o fechamento pergunta ao juiz sobre um conjunto de batidas, entrega ao motor outro conjunto e a partição ainda sobrescreve o mapa depois da decisão de competência. É uma segunda atribuição em memória. As três gravações continuam nas portas existentes de fechamento e `DiaPago`; o calendário e o TXT podem consumir fotografias diferentes. (`app/core/juizes.py:318–320`, `app/ponto/calculador/regras.py:314–322,410–432`, `app/ponto/services/fechamento.py:167–173,340–351,1172–1236,1315–1334`, `app/ponto/motor_calculo_v2.py:2459–2467,2513–2545`, `app/ponto/services/dia_pago.py:358–362`, `app/folha/views.py:214–219,269–275`)
- **Relógio e travas.** O delta acrescenta zero leituras de relógio. O teto do furo usa `_agora_da_passada`; os `now()` movidos carimbam a escrita. A revisão toma Empresa → Colaborador → Batida e o TXT toma Empresa. O evento genérico roda via `on_commit`, e a apuração precede o novo `atomic`. Inversão explícita introduzida pelo delta não aparece nesses caminhos. Serialização da apuração com entrega ou backfill concorrente [nao verificado]. (`diff.txt:1`, `app/ponto/services/fechamento.py:679–681,819–830,925–929,1121,1150–1153,1178,1196,1224,1846–1863`, `app/ponto/services/par_relampago_na_lista.py:172–187`, `app/folha/export.py:570–576`)
- **Soma, idempotência, previsto e leitores.** As três listas são disjuntas e somam os minutos que **chegaram** à partição. A perda na borda e o arredondamento mensal quebram a igualdade mais ampla. Duas execuções podem repetir os valores monetários testados, mas mudam as linhas e os carimbos. O previsto comum deriva da grade; o realizado limita realizado e abono. Há uma exceção preexistente no intermitente convocado, cujo previsto copia o realizado. O comparador usa mensal gravado, o cartão com mensal recalcula online e o calendário lê a lavra. A igualdade entre todos esses leitores segue sem garantia. (`app/ponto/calculador/regras.py:392–453`, `app/ponto/services/fechamento.py:1011–1019,1078–1099,1178–1225`, `app/ponto/services/dia_pago.py:334–362,388–404,552–565`, `app/escala/utils.py:2099–2124`, `app/relatorios/cartao_pela_celula.py:69–76,250–273`, `app/colaboradores/services/calendario.py:777–782`)
- **Mordida.** Em memória passaram **40 controles e 41 estruturais**. Os **8 mutantes c5 de produção** executaram funções alteradas e deram seis falhas de asserção e os dois erros esperados. Dos dois estruturais c5, um muda o registro do teste e o outro simula fonte de produção para o AST. Os **4 c6 de E5/59-A** morderam o texto entregue a `inspect`, sem executar a função alterada. Nos três c6 de porta, dois têm alvo de produção e o de setembro altera a fixture. Os três B023 são lint de teste, incluindo um caso em `skip`. Execução dos mutantes de porta, B023, RED contra `6d568d221`, suíte inteira e identidade Git da cópia [nao verificado]. (`app/ponto/tests/feriado_um_juiz_mutantes.py:53–107,231–247,325–427,453–481`, `app/ponto/tests/test_feriado_um_juiz_porta.py:538–549`)
- **`except`.** No diff literal, **3 linhas de cláusula adicionadas e 3 removidas**, com **0/0 em testes**. Uma linha idêntica de `except Exception:` ficou como contexto no diff. Após alinhar os blocos reindentados, são **0 handlers novos e 0 abolidos**. Os dois handlers das lavras agora sinalizam rollback. Incluindo menções em comentário e docstring, são **4 linhas adicionadas e 5 removidas**. (`diff.txt:1`, `app/ponto/services/fechamento.py:1113–1134,1243,1272–1277,1335–1342`, `app/ponto/services/dia_pago.py:144–148`)

## 27/11 17:54 · sessao 1e2a2a759d32 · - · - · origem=Ronald (tmux)

**Achado 5 curado quanto às três gravações do recálculo.** A transação começa antes de `get_or_create`, cobre `save` e `update` do mensal e envolve as lavras `motor` e `oraculo`. A falha em qualquer lavra marca rollback. O oráculo só roda da competência 10 em diante e após sucesso da primeira lavra. O backfill também usa a transação, gravando só as lavras. (`app/ponto/services/fechamento.py:925–929,967–971,1172–1228,1272–1277,1301–1340`)

- Rodei os controles e cinco mutantes em memória. Remover a transação comum ou qualquer sinal de rollback gerou uma falha de asserção por mutante. Exigir mapa em resultado sem pares ou recusar `rubricas={}` gerou o erro-alvo esperado. O teste ORM injeta falha na segunda lavra, comprova que as 5 h já chegaram ao mensal e à lavra `motor` no momento da falha, e exige fotos completas originais para mensal `aberto` e `aprovado`. Execução ORM [nao verificado]. (`app/ponto/tests/feriado_um_juiz_mutantes.py:89–106,456–481`, `app/ponto/tests/test_feriado_um_juiz.py:43–169`, `app/ponto/tests/test_feriado_um_juiz_porta.py:462–499`)

- **Bloqueio novo no retorno.** Instrumentei os dois testes sem banco e, em ambos, o recálculo preservou o estado antigo e retornou `1`. A tela comunica sucesso e o evento registra conclusão. A revisão administrativa segue com a batida restaurada mesmo se o savepoint inteiro do recálculo voltar, embora a própria porta prometa desfazer toda a revisão quando o recálculo falha. (`app/ponto/tests/test_feriado_um_juiz.py:43–47,138–145`, `app/ponto/services/fechamento.py:1272–1277,1335–1350,1846–1852,1869–1891`, `app/ponto/views.py:1280–1281`, `app/ponto/registro_batida.py:305–321`)

- **`rubricas={}` amplia a porta além do ajuste isolado.** `lavrar` agrega períodos, órfãos, faltas, furos e grade antes da guarda dos donos. Com o dicionário vazio, publica as linhas-base e o ajuste com o rótulo solicitado. O fluxo de produção pode retornar dicionário vazio quando o calculador não tem par válido ou dia bloqueado no recorte, mesmo havendo entradas próprias da grade ou órfãos. O teste introduzido usa somente a linha de ajuste. (`app/ponto/services/dia_pago.py:164–173,267–321,334–362`, `app/ponto/calculador/alimentacao.py:494–526,571–598`, `app/ponto/tests/test_feriado_um_juiz.py:147–169`)

- **Idempotência literal falha.** As duas rodadas testadas exigem igualdade de valores. Cada relavra faz `delete` e `bulk_create`, com novo carimbo, e o mensal também recarimba. O caso sem promoção espera 8,04 h para dois pares de 241 min, contra 8,03 h na soma diária, por arredondamento em etapas. A promoção de 10 s muda o mensal comum para 8,03 h, mesmo aparecendo como 0,00 h de feriado. (`app/ponto/tests/test_feriado_um_juiz_porta.py:396–433`, `app/ponto/services/fechamento.py:1011–1019,1121,1178–1179,1195–1196,1224`, `app/ponto/services/dia_pago.py:177,262,334–362,552–565`, `app/ponto/models.py:373–387`)

- **Ordem e fontes.** A revisão toma Empresa → Colaborador → Batida e o TXT toma Empresa. O recálculo genérico por evento usa `on_commit` e apura antes do novo `atomic`. Não observei inversão explícita no delta. A serialização das leituras com o TXT e teste concorrente de frescor [nao verificado]. O previsto comum sai da grade, enquanto o realizado limita realizado/abono. A exceção preexistente do intermitente convocado copia o realizado para o previsto. (`app/ponto/services/par_relampago_na_lista.py:172–187`, `app/folha/views.py:102–104,132–135`, `app/ponto/services/fechamento.py:819–830,925–929,1078–1099,1846–1863`, `app/ponto/services/dia_pago.py:388–404`, `app/escala/utils.py:2099–2124`)

- **Contagem de `except`.** O diff literal tem 3 cláusulas adicionadas e 3 removidas por reindentação, 0 em testes. No código há os mesmos handlers, com rollback acrescentado nos dois das lavras. Incluindo menções em comentário/docstring, 4 linhas adicionadas e 5 removidas. (`diff.txt`, `app/ponto/services/fechamento.py:1113–1134,1272–1277,1335–1342`, `app/ponto/services/dia_pago.py:144–148`)

## 27/11 17:53 · sessao fed243fe2afc · - · - · origem=Ronald (tmux)

**Achado 4 não curado no caso combinado.** Executei em memória o helper de c5 com dois turnos fechados de 4 h, corte 21 e troca 6x1 → 12x36 em 21/11. Com feriado apenas no dia 20, a competência 11 recebeu 4 h de HE100 feriado e a 12 ficou vazia. Com feriado apenas no dia 21, a 11 recebeu 4 h comuns e a 12 ficou vazia. Em ambos os casos, perderam-se as 4 h do segundo turno. A fatia 6x1 retira o turno externo. A fatia 12x36 pula o filtro e o motor herda a jornada da véspera, descartando o par antes da partição. Depende do motor. Ocorrência em produção [nao verificado]. (`app/ponto/tests/test_feriado_um_juiz.py:560–623`, `app/ponto/services/fechamento.py:155–173,337–351`, `app/ponto/motor_calculo_v2.py:2513–2522,2537–2545`)

Os REDs isolados cobrem feriado só no dia 20 sem troca e troca de ciclo dentro da mesma competência. O primeiro verifica as duas janelas. O segundo verifica rubrica e data após a composição. A partição troca o motor do julgamento da dobra pelo motor do dia do turno quando há contexto disponível, mas ainda reescreve `dia_da_jornada` depois do corte. A janela já foi decidida pela jornada herdada. A dupla chamada a `_turnos_do_juiz` usa a mesma autoridade com conjuntos distintos de batidas. (`app/ponto/tests/test_feriado_um_juiz.py:607–623`, `app/ponto/calculador/regras.py:410–432`, `app/ponto/services/fechamento.py:167–173,340–349`, `app/ponto/motor_calculo_v2.py:2459–2467,2537–2545`)

Outubro conserva FT 6x1 incerta como hora comum no RED em memória e no teste de fechamento e duas lavras. A fotografia histórica dessa fixture e seu registro anterior são comparados diretamente. O POST do TXT segue sem prova nessa fixture. Na borda de outubro, reproduzi 8 h comuns no dia 20 na competência 10 e nenhuma na 11 com feriado em qualquer um dos dois dias. O filtro da borda inicial só começa na 12. (`app/ponto/tests/test_feriado_um_juiz.py:428–435,560–605`, `app/ponto/tests/test_feriado_um_juiz_porta.py:247–296`, `app/ponto/services/fechamento.py:227–231`)

O `atomic` resolve o rollback das três gravações no caminho normal, mas ambas as falhas seguem para `processados += 1`. O comando imprime e anota esse total mesmo quando o colaborador teve as gravações revertidas. A guarda `if rubricas:` também aceita `{}` junto com períodos remunerados e permite montar uma lavra `oraculo` com valores do motor sem declaração de dono. O RED cobre a ausência de períodos e a preservação do ajuste. Ocorrência de rubricas vazias com períodos remunerados em produção [nao verificado]. (`app/ponto/services/fechamento.py:1272–1277,1335–1350`, `app/ponto/management/commands/recalcular_fechamento.py:130–146`, `app/ponto/services/dia_pago.py:172–185,310–346`, `app/ponto/tests/test_feriado_um_juiz.py:146–169`)

## 27/11 17:53 · sessao 50bfc707e9a0 · - · - · origem=Ronald (tmux)

Os 40 controles sem banco e os 41 testes estruturais passaram. Os oito mutantes c5 que executam funções de produção falharam como previsto. Os dois mutantes estruturais c5 e os quatro mutantes textuais c6 também falharam como previsto. A execução contra a base c4, os mutantes de porta e o B023 ficam [nao verificado]. (`app/ponto/tests/feriado_um_juiz_mutantes.py:53-107,231-247,325-427,453-481`)

- **HE100.** A fixture de novembro continua exercitando fechamento e as duas lavras, com 5,5 h em feriado. A regra histórica de outubro para 6x1 e escala incerta tem RED na partição e prova de porta para outra fixture. O caso exato de 330 min sob a L-097 continua em outubro em 12x36. (`app/ponto/tests/test_he100_dia_sem_previsto.py:27-48,179-193,208-252`, `app/ponto/tests/test_feriado_um_juiz.py:396-435`, `app/ponto/tests/test_feriado_um_juiz_porta.py:239-296`)
- **C3b.** A fixture conserva setembro e testa três resultados distintos, inclusive a ausência do atributo de CCT. O modo histórico explícito corresponde ao corte. O teste chama a alimentação diretamente e a seleção de modo pelo fechamento tem sítio próprio. O mutante de modo altera a fixture. O mutante do default `True` altera produção. (`app/ponto/tests/test_o280_bateria_da_lei.py:1230,3100-3158`, `app/ponto/services/fechamento.py:601-603,1299-1312`, `app/ponto/tests/feriado_um_juiz_mutantes.py:231-247`)
- **59-A e E5.** A âncora 59-A segue as duas entradas do TXT. Exercitei as duas entradas em memória para outubro/12x36 e confirmei que ambas respeitam a cláusula, ligada e desligada. E5 conserva as guardas do ramo de criação e do ramo de persistência. Os quatro mutantes c6 exercitam os selos por fonte simulada. (`app/folha/tests/test_export_59a.py:19-78`, `app/folha/export.py:388-394,748-760,1018-1034`, `app/ponto/tests/test_e5_fechamento_como_leitura.py:35-60`, `app/ponto/tests/feriado_um_juiz_mutantes.py:53-67,349-364`)
- **Achado 6 curado.** A catraca atualiza cinco entradas de endereço. Os dois selos AST voltaram ao verde com a atualização do mapa seguida da leitura por `_dia_de`. Voltar ao endereço antigo falha uma asserção. Trocar a consulta ao mapa por `dia_jornada = dia_turno` falha duas. (`app/core/tests/test_medida_turno_do_par.py:289,329-334,565-569`, `app/ponto/calculador/regras.py:424-429`, `app/ponto/tests/test_noturno_v8_ft_no_dia_da_lavra.py:33-71`, `app/ponto/tests/feriado_um_juiz_mutantes.py:325-346`)
- **Falha da lavra e confirmação ao chamador.** A reversão agora alcança o mensal, e o colaborador revertido segue contado em `processados`. O comando publica essa contagem. O backfill conta objetos de leitura publicados antes da reversão. A revisão ignora o retorno do recálculo, embora prometa desfazer o ato em caso de falha. (`app/ponto/services/fechamento.py:1138-1148,1272-1276,1335-1350,1869-1891`, `app/ponto/management/commands/recalcular_fechamento.py:130-145`, `app/ponto/management/commands/lavrar_dias_pagos.py:104-109`)
- **`except`.** Quatro linhas de cláusula entram e quatro saem por recuo, com zero cláusula nova ou suprimida e zero em teste. Na contagem literal da palavra, entram cinco e saem seis, incluindo comentário recuado e menção retirada da docstring. (`diff.txt:1`, `app/ponto/services/fechamento.py:1116,1122,1243,1272,1335`, `app/ponto/services/dia_pago.py:144-148`)

## 27/11 17:34 · sessao 723ef8477e8c · FERIADO-UM-JUIZ · construtor · origem=Code

A c6 está no diff da cópia sobre `5792a7bfe`, sem commit pela regra explícita da volta. Corrigi cinco falhas nos testes, com as asserções originais. A origem das 79 queries segue **[nao verificado]**. O teto continua em 55. `core/tests/test_selo_performance.py:257`

| Falha | Causa | O que fiz |
|---|---|---|
| 1. Ruff `B023` | **Obra.** A c4 introduziu as capturas de `linhas` e `decisoes`. A c5 introduziu a de `ordem`. `ponto/tests/test_feriado_um_juiz_porta.py:447,484,549` | Liguei cada variável por argumento padrão no callback. `ponto/tests/test_feriado_um_juiz_porta.py:446,482,549` |
| 2. Selo E5 | **Obra c5.** O `with transacao` levou o `else` de 12 para 16 espaços. O ramo de leitura faz `filter().first()` e constrói uma instância excluindo a chave primária. O `get_or_create` fica no ramo de escrita. `ponto/services/fechamento.py:928–929,946–968` | Reancorei só o fim do recorte. `ponto/tests/test_e5_fechamento_como_leitura.py:41–48` |
| 3. Calendário, 79/55 | **Sem nexo com c1–c5 no caminho do selo.** A fixture abre setembro civil em 12x36 e chama a autoridade sem competência. O ramo novo de feriado na borda fica inativo. O motor já recebe o calendário da janela em lote. `core/tests/test_selo_performance.py:281,291–295,409–413`, `colaboradores/services/calendario.py:194–197,239–241,602`, `ponto/services/fechamento.py:227–231,337–343`, `ponto/services/espelho.py:1067,1149–1153` | Sem alteração na rota. O agregado do anexo deixa o SQL responsável e a contagem na base **[nao verificado]**. |
| 4. 6x1, 5,5 h de feriado | **Obra c4.** O RED da c1 rodava em outubro. O corte da c4 preserva a rubrica histórica de horas comuns até outubro. A escala incerta retorna antes do zero da L-097. `ponto/calculador/regras.py:393–409`, `folha/export.py:385`, `ponto/motor_calculo_v2.py:1579–1590` | Levei célula, feriado e batidas do RED à competência 11, com relógio congelado. O teste continua exigindo 5,5 h de feriado, 0 h comuns e igualdade entre as duas lavras. `ponto/tests/test_he100_dia_sem_previsto.py:234–252` |
| 5. C3b, guarda de vacuidade | **Obra c4.** O modo classificado usa a dobra já apurada pelo primeiro motor. A fixture altera a flag no segundo motor, que o leitor consulta. `ponto/calculador/alimentacao.py:519–523,584`, `ponto/tests/test_o280_bateria_da_lei.py:3103–3119` | Fiz a fixture de setembro pedir o modo histórico. O teste continua exigindo dobra com `True`, zero com a flag real do 12x36 e rubricas declaradas quando a flag está ausente. `ponto/tests/test_o280_bateria_da_lei.py:3119,3143–3157` |
| 6. Selo do export 59-A | **Obra c4.** Montagem e homologação passaram a usar o mesmo leitor da cláusula histórica. O selo que contava duas chamadas diretas passou a contar uma. `folha/export.py:388–394,753,1026` | Reancorei a fonte do selo nas duas portas, seguindo a delegação de cada uma. As duas contagens originais continuam valendo. `folha/tests/test_export_59a.py:58–68` |
| 7. Smoke de tipo de ausência | **Infraestrutura.** O diff c1–c5 não altera as rotas, views ou templates do fluxo. O anexo registra timeout de 120 s no Chromium antes da asserção do POST. A causa interna do timeout é **[nao verificado]**. `ponto/views_tipos_ausencia.py:37–48`, `ponto/tests/test_smoke_clique_tipo_ausencia.py:97–101,150–154` | Sem edição no smoke. |

Reproduzi em memória o `ValueError` e o `1 != 2` da c5. Na c6, passaram os controles de 40 e 41 testes sem banco, e os quatro mutantes estruturais falharam nas asserções esperadas. Ruff, os testes com banco e a contagem do calendário ficam **[nao verificado]** nesta cópia. A certificação fica com Code e Opus. `ponto/tests/feriado_um_juiz_mutantes.py:54–67,325–364,453–480`

**Recorte para o Code** (`manage.py:7–18`, `config/settings/ci.py:40`)

```bash
PYTHONDONTWRITEBYTECODE=1 RUFF_NO_CACHE=1 python3 manage.py test ponto core folha relatorios --settings=config.settings.ci
```

**Mutantes da c6** (`ponto/tests/feriado_um_juiz_mutantes.py:54–72,232–247`)

```bash
export PYTHONDONTWRITEBYTECODE=1
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_laco_sem_linhas
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_laco_sem_ordem
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_laco_sem_decisoes
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_leitura_faz_get_or_create
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_txt_pula_clausula_na_montagem
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_txt_pula_clausula_na_homologacao
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_txt_perde_clausula_no_historico
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_escala_incerta_fica_no_historico_em_novembro
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_cct_classifica_o_feriado_de_setembro
python3 -m ponto.tests.feriado_um_juiz_mutantes c6_cct_ausente_vira_true
```

## 27/11 17:32 · sessao a479b64b62da · - · - · origem=Ronald (tmux)

A falha 3 registra 79 consultas contra o teto de 55 no anexo. A origem dessas consultas fica [nao verificado].

A fixture usa 12x36, corte de competência 21 e uma requisição pelo mês civil de setembro. Nessa rota, a chamada à autoridade omite `competencia`, o conjunto de bordas fica vazio e o juiz da borda introduzido na c4 fica fora do caminho. As mudanças alcançadas em `apurar_por_fatia` preenchem dicionários em memória. O calendário do motor já chega em lote, com as duas bordas. [test_selo_performance.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/core/tests/test_selo_performance.py:281) [models.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/colaboradores/models.py:46) [calendario.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/colaboradores/services/calendario.py:602) [fechamento.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/fechamento.py:227) [fechamento.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/fechamento.py:337) [espelho.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/espelho.py:1057)

A janela civil também fica fora da leitura dos totais mensais em `folha_manda`, que exige coincidência com a janela da competência. O SQL agregado do anexo não sustenta editar essa rota ou criar mutante de performance por hipótese. [cartao_pela_celula.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/relatorios/cartao_pela_celula.py:27) [cartao_pela_celula.py](/home/ronald/wt-codex-feriado-um-juiz-152427/app/relatorios/cartao_pela_celula.py:324)

_8766 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
