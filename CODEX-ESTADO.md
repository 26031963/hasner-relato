# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 13:42** · publicado em **18/11/2026 13:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 13:42 · sessao 411db8141832 · LAUDO-[nome]-1811-1340 · leitura · origem=Code

Os dados fornecidos **não permitem informar o total de dias e minutos do universo pedido**, nem confirmar os dois casos nominais: o anexo os152 traz um subconjunto de marcações ímpares e não inclui as linhas completas dos casos. A sonda abaixo prepara essa medição; ela não foi executada aqui. [não verificado]

## Campos antes da leitura

Campos usados diretamente pela sonda e pelas funções chamadas, com suas definições:

- `Colaborador.empresa`: [colaboradores/models.py:321](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:321); a identificação `pk` é a chave primária implícita do modelo. [colaboradores/models.py:283](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:283)
- `Batida.colaborador`, `timestamp`, `tipo` e `retratada_em`: [ponto/models.py:22](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:22), [ponto/models.py:23](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:23), [ponto/models.py:24](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:24), [ponto/models.py:67](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:67).
- `DiaPago.colaborador`, `mes`, `ano`, `data`, `tipo`, `versao` e `horas_intra_indenizada`: [ponto/models.py:315](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:315), [ponto/models.py:317](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:317), [ponto/models.py:318](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:318), [ponto/models.py:319](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:319), [ponto/models.py:321](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:321), [ponto/models.py:322](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:322), [ponto/models.py:337](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:337).
- `TipoEscala.intervalo_modo`, `intervalo_indenizavel`, `intervalo_duracao_min` e horários de intervalo: [escala/models.py:103](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:103), [escala/models.py:108](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:108), [escala/models.py:110](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:110), [escala/models.py:52](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:52).
- `EscalaColaborador.colaborador`, `tipo_escala`, `data_inicio`, `data_fim`, `ativa`: [escala/models.py:872](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:872). `CelulaDia.colaborador`, `data` e `origem`: [escala/models.py:1681](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:1681).
- `PerguntaDisputa.celula`, `motivo`, `data_evento`, `colaborador`, `resposta_colab` e `respondida_em`: [chamados/models.py:2527](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2527), [chamados/models.py:2534](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2534), [chamados/models.py:2536](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2536), [chamados/models.py:2546](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2546), [chamados/models.py:2560](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2560).

## Funções reais e achado do código

- **Espelho:** a view chama `espelho_do_colab`; esta chama `autoridade_do_periodo`, que carrega `batidas_apuraveis` e, havendo escalas, chama `apurar_por_fatia`. [ponto/views.py:160](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:160), [ponto/services/espelho.py:1227](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1227), [ponto/services/espelho.py:1022](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1022), [ponto/services/espelho.py:1162](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1162).
- **Gestão de HE:** a tela chama `he_pendente_lavrado.ler` e `gestao_he.enriquecer`; esta consulta `estado_por_dia`, que lê `DecisaoHE.objects.vigentes()`. Isso responde à pendência de HE e sua decisão, não é a função que decide a intrajornada. [ponto/views.py:932](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:932), [ponto/views.py:945](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:945), [ponto/services/gestao_he.py:63](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:63).
- **Motor:** não há uma única implementação de indenização para todos os regimes. `Motor12x36.calcular_periodo` escreve os minutos suprimidos; `MotorComercial.calcular_periodo` tem sua própria condição; `MotorTurnoPartido.calcular_mes` trata o intervalo entre períodos. O mínimo vem de `_intra_minima`, e o intervalo reconhecido vem do pareador. [ponto/motor_calculo_v2.py:2750](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2750), [ponto/motor_calculo_v2.py:3215](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:3215), [ponto/motor_calculo_v2.py:2964](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2964), [ponto/motor_calculo_v2.py:1805](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1805), [ponto/motor_calculo_v2.py:1892](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1892).
- **PDF:** `_coletar_dados_espelho` chama `apurar_por_fatia` para seu resultado e chama `espelho_do_colab` para as linhas diárias. [relatorios/pdf_espelho.py:417](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:417), [relatorios/pdf_espelho.py:596](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:596).

O motor declara que **não usa `TipoEscala.intervalo_indenizavel` para impedir a indenização**; portanto, `False` é um filtro do censo pedido, não uma condição aplicada pelo motor. [ponto/motor_calculo_v2.py:817](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:817)

O anexo informa **152 dias** no recorte os152, dos quais **151 com a flag `False` e um com `True`**. Ele seleciona número ímpar de batidas, exige `DiaPago` e `pago_do_dia`, e exige pago maior que o realizado do pareador em mais de um minuto. Assim, ficaram fora todos os dias pares — inclusive eventuais dias de duas ou quatro batidas — e também os ímpares que não satisfazem as demais condições. A quantidade e os minutos deixados de fora não aparecem no dado fornecido. [não verificado: anexo os152.r e os152_os24.py fornecidos no pedido, sem arquivo e linhas acessíveis]

Para **col868 em 14/11, pergunta 39550**, e **col934 em 15/11, pergunta 39556**, “duas batidas e 60 minutos” é a afirmação do aval; não há no recorte apresentado resultado que a confirme. [não verificado]

## Sonda para o envelope na sombra

A sonda mede o **motor recalculado em leitura**, para todos os colaboradores encontrados, usando a janela da competência de cada empresa. Mostra também a rubrica `DiaPago` gravada nos dois casos, pois ela pode representar outro momento de cálculo. Agrupa as batidas apuráveis pelo `dia_das_batidas`, a mesma autoridade usada pelo espelho, e informa também as classes 0 e 1 para não escondê-las. [ponto/turnos.py:2868](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:2868), [ponto/turnos.py:2898](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:2898), [ponto/services/dia_pago.py:109](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:109).

Execute o bloco como entrada do `manage.py tenant_command shell --schema=juliani`. Ele exige PostgreSQL para estabelecer a transação `READ ONLY`; se isso falhar, não prossegue com a leitura. Os tracebacks mostram quadros e tipo da exceção, sem mensagem de exceção ou conteúdo de variáveis, para reduzir a exposição de dados pessoais. [não verificado: execução no envelope]

```python
import sys
import traceback
import logging
from collections import Counter, defaultdict

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from chamados.models import PerguntaDisputa
from escala.alimentacao import escalas_do_periodo, vinculo_do_dia
from ponto.janelas import janela_fechamento
from ponto.models import DiaPago
from ponto.services.dia_pago import _dia_de
from ponto.services.espelho import autoridade_do_periodo, espelho_do_colab
from ponto.services import gestao_he, he_pendente_lavrado
from ponto.turnos import dia_das_batidas
from relatorios.pdf_espelho import _coletar_dados_espelho

logging.disable(logging.CRITICAL)

def erro(item):
    exc = sys.exc_info()[1]
    print("TRACEBACK", item)
    traceback.print_tb(exc.__traceback__, file=sys.stdout)
    print("TIPO_ERRO", type(exc).__name__)

def faixa(n):
    if n >= 4:
        return "4+"
    return str(n)

def linha_motor(col, ini, fim):
    aut = autoridade_do_periodo(col, ini, fim)
    mapa_batida = dia_das_batidas(col, ini, fim)
    contagem = Counter(
        mapa_batida[b.pk]
        for b in aut.batidas
        if b.pk in mapa_batida and ini <= mapa_batida[b.pk] <= fim
    )
    minutos = defaultdict(float)
    resultado = aut.resultado
    mapa_periodo = getattr(resultado, "dia_da_jornada", None)
    for periodo in list(resultado.periodos) + list(resultado.periodos_ft):
        dia = _dia_de(periodo, mapa_periodo)
        if dia is not None and ini <= dia <= fim:
            minutos[dia] += float(
                periodo.minutos_intrajornada_indenizada or 0
            )
    return contagem, minutos

try:
    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )

        agora = timezone.now()
        totais = defaultdict(lambda: [0, 0.0])
        sem_escala = [0, 0.0]
        erros = 0

        # Censo: todos os colaboradores cadastrados; a janela é da empresa.
        try:
            ids = list(
                Colaborador.objects.order_by("pk")
                .values_list("pk", flat=True)
            )
            for pk in ids:
                try:
                    # Savepoint permite continuar após erro de uma consulta.
                    with transaction.atomic():
                        col = Colaborador.objects.select_related(
                            "empresa"
                        ).get(pk=pk)
                        ini, fim = janela_fechamento(11, 2026, col.empresa)
                        contagem, minutos = linha_motor(col, ini, fim)
                        escalas = escalas_do_periodo(pk, ini, fim)

                        for dia, valor in minutos.items():
                            if valor <= 0:
                                continue
                            vinculo = vinculo_do_dia(pk, dia, escalas)
                            te = vinculo.tipo_escala if vinculo else None
                            if te is None:
                                sem_escala[0] += 1
                                sem_escala[1] += valor
                                continue
                            if te.intervalo_indenizavel is False:
                                grupo = faixa(contagem.get(dia, 0))
                                totais[grupo][0] += 1
                                totais[grupo][1] += valor
                except Exception:
                    erros += 1
                    print("ERRO_COL col%d" % pk)
                    erro("CENSO_COL")

            print("CENSO_COLABORADORES", len(ids))
            for grupo in ("0", "1", "2", "3", "4+"):
                dias, mins = totais[grupo]
                print(
                    "MOTOR_FALSE batidas=%s dias=%d minutos=%.2f"
                    % (grupo, dias, mins)
                )
            print(
                "SEM_ESCALA dias=%d minutos=%.2f"
                % (sem_escala[0], sem_escala[1])
            )
            print("ERROS_COLABORADORES", erros)
            if erros:
                print("RESULTADO_CENSO_INCOMPLETO")
        except Exception:
            erro("CENSO")
        finally:
            print("FIM_SONDA_CENSO")

        # Conferência dos dois dias e da identidade das perguntas.
        for pk, dia_iso, pergunta_pk in (
            (868, "2026-11-14", 39550),
            (934, "2026-11-15", 39556),
        ):
            item = "col%d" % pk
            try:
                with transaction.atomic():
                    from datetime import date

                    dia = date.fromisoformat(dia_iso)
                    col = Colaborador.objects.select_related(
                        "empresa"
                    ).get(pk=pk)
                    ini, fim = janela_fechamento(11, 2026, col.empresa)
                    contagem, minutos = linha_motor(col, ini, fim)
                    escalas = escalas_do_periodo(pk, ini, fim)
                    vinculo = vinculo_do_dia(pk, dia, escalas)
                    te = vinculo.tipo_escala if vinculo else None

                    print(
                        "CASO %s dia=%s batidas_apuraveis=%d "
                        "motor_minutos=%.2f intervalo_indenizavel=%s"
                        % (
                            item, dia_iso, contagem.get(dia, 0),
                            minutos.get(dia, 0.0),
                            te.intervalo_indenizavel if te else "SEM_ESCALA",
                        )
                    )

                    linhas = DiaPago.objects.filter(
                        colaborador=col, mes=11, ano=2026,
                        versao="motor", tipo=DiaPago.TIPO_DIA, data=dia,
                    ).values_list("horas_intra_indenizada", flat=True)
                    for horas in linhas:
                        print(
                            "DIA_PAGO %s dia=%s minutos=%.2f"
                            % (item, dia_iso, float(horas or 0) * 60)
                        )

                    p = PerguntaDisputa.objects.filter(
                        pk=pergunta_pk
                    ).values(
                        "colaborador_id", "motivo",
                        "data_evento", "celula__data"
                    ).first()
                    if p is None:
                        print("PERGUNTA %s AUSENTE" % item)
                    else:
                        print(
                            "PERGUNTA %s mesma_pessoa=%s motivo=%s "
                            "data_evento=%s data_celula=%s"
                            % (
                                item, p["colaborador_id"] == pk,
                                p["motivo"], p["data_evento"],
                                p["celula__data"],
                            )
                        )

                    # Leitores reais: apenas características não pessoais.
                    esp = espelho_do_colab(
                        col, fim, ini, fim, ano=2026, agora=agora
                    )
                    ed = next(
                        (d for d in esp["dias"] if d["data"] == dia), None
                    )
                    print(
                        "ESPELHO %s dia_presente=%s batidas=%s"
                        % (
                            item, ed is not None,
                            len(ed["batidas"]) if ed else "AUSENTE",
                        )
                    )

                    pdf = _coletar_dados_espelho(
                        col, ini, fim, agora=agora
                    )
                    pd = next(
                        (d for d in pdf["dias"] if d["data"] == dia), None
                    )
                    print(
                        "PDF %s dia_presente=%s batidas=%s"
                        % (
                            item, pd is not None,
                            len(pd["batidas"]) if pd else "AUSENTE",
                        )
                    )

                    retrato, calculado_em = he_pendente_lavrado.ler(
                        col.empresa, 11, 2026
                    )
                    vis = gestao_he.enriquecer(
                        retrato,
                        limite=int(
                            getattr(
                                col.empresa, "limite_decisao_he_min", 0
                            ) or 0
                        ),
                    )
                    presente = any(
                        c["colab"] == pk for c in vis["colabs"]
                    )
                    print(
                        "GESTAO_HE %s retrato_existe=%s "
                        "calculado_em=%s colab_presente=%s"
                        % (
                            item, retrato is not None,
                            calculado_em, presente,
                        )
                    )
            except Exception:
                erro(item)
            finally:
                print("FIM_SONDA_%s" % item.upper())

        # A transação é somente leitura; não há chamada de lavratura.
except Exception:
    erro("ENVELOPE")
finally:
    print("FIM_SONDA_ENVELOPE")
```

A comparação numérica com os **151** exige o resultado do censo e, para uma interseção exata, as linhas completas D2 ou a reprodução do filtro os152 no mesmo instante. A diferença entre os totais, por si só, não mede essa interseção: o os152 usa `DiaPago` gravado e a sonda acima usa o motor recalculado. [não verificado]

## 18/11 13:42 · sessao 05766d97b939 · LAUDO-[nome]-1811-1340 · leitura · origem=Code

Os dados raspados relatam que **col868, em 14/11, aparecia no espelho com 18 min antes da entrada e 3 min depois da saída, e não aparecia na Gestão de HE**. A causa, a hora do retrato, a presença do dia nele, a situação cadastral, os filtros efetivamente usados e o instante da mudança permanecem **[não verificado]**. O anexo é o dado fornecido na solicitação, sem arquivo:linha disponível **[não verificado]**.

A cópia disponível não tem metadados Git acessíveis no diretório examinado; portanto, não pude confirmar que corresponde ao hash informado **[não verificado]**.

**Campos relevantes antes da leitura**

- `MetricaSnapshot`: `chave`, `escopo`, `escopo_id`, `data_ini`, `data_ref`, `numerador`, `denominador`, `valor`, `detalhe` e `calculado_em`. O retrato é o JSON `detalhe`; a data e hora são `calculado_em`. [app/inteligencia/models.py:81](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/inteligencia/models.py:81)
- `Colaborador`: `empresa`, `posto`, `situacao`, `data_admissao` e `data_demissao`; o identificador interno é a chave primária. [app/colaboradores/models.py:283](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:283) [app/colaboradores/models.py:319](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:319)
- `Empresa`: `dia_inicio_competencia`, `janela_he_ativa`, `janela_he_piso_min`, `limite_decisao_he_min`, `janela_he_teto_min`, `janela_he_saida_ativa`, `janela_he_saida_teto_min` e `janela_he_desde`. [app/colaboradores/models.py:46](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:46) [app/colaboradores/models.py:58](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:58) [app/colaboradores/models.py:69](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:69) [app/colaboradores/models.py:82](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:82) [app/colaboradores/models.py:90](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:90) [app/colaboradores/models.py:101](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:101) [app/colaboradores/models.py:117](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:117)
- `DecisaoHE`: `colaborador`, `data`, `estado`, `minutos_na_decisao`, `motivo`, `decidida_por`, `origem`, `revogada_em`, `revogada_motivo` e `minutos_na_revogacao`. `vigentes()` exclui as revogadas. [app/ponto/models.py:1265](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:1265) [app/ponto/models.py:1300](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:1300) [app/ponto/models.py:1335](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:1335)
- `Batida`: `colaborador`, `timestamp`, `tipo`, `sequencia`, `is_intervalo`, `escala` e `origem`. O resultado do motor não é modelo: `PeriodoCalculo` contém `entrada`, `saida` e `minutos_fora_janela`. [app/ponto/models.py:22](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:22) [app/ponto/models.py:45](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:45) [app/ponto/motor_calculo_v2.py:208](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:208)

**Funções reais e efeito dos filtros**

- A tela do espelho chama `espelho_do_colab`; este chama `autoridade_do_periodo` e `montar_dias`. O montador chama `marcar_pontas_fora`, acrescenta pontas publicadas pelo motor e publica `he_fora_da_janela`. O texto “sem decisão” está fixo no template; ele não comprova ausência de decisão vigente. [app/ponto/views.py:156](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:156) [app/ponto/services/espelho.py:1225](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1225) [app/ponto/services/espelho.py:721](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:721) [app/templates/ponto/espelho.html:189](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/templates/ponto/espelho.html:189)
- A Gestão chama `he_pendente_lavrado.ler` e `gestao_he.enriquecer`; a decisão viva vem de `estado_por_dia` e `DecisaoHE.objects.vigentes()`. O PDF da Gestão chama os mesmos leitores. O cartão PDF chama `espelho_do_colab`. [app/ponto/views.py:932](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:932) [app/ponto/services/gestao_he.py:63](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:63) [app/ponto/views.py:804](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:804) [app/relatorios/pdf_espelho.py:596](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:596)
- O espelho obtém o resultado por `autoridade_do_periodo`, que usa `apurar_por_fatia` quando há escalas e, no outro ramo, `motor.calcular_mes`. Na implementação `Motor12x36.calcular_periodo`, a linha indicada soma os minutos retirados da entrada e da saída; não há evidência fornecida de que esse seja o ramo específico de col868. [app/ponto/services/espelho.py:1162](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1162) [app/ponto/motor_calculo_v2.py:2750](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2750) [app/ponto/motor_calculo_v2.py:2787](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2787) **[não verificado]**
- Na apuração do retrato, o código seleciona colaboradores com `situacao='ativo'` no momento da apuração e só inclui dias dentro da competência que tenham pontas publicadas pelo espelho. Na leitura da Gestão, o limite classifica a ponta por `acima_do_limite`; os filtros de estado e dono podem retirar dias e, se não sobrar nenhum, a linha. Busca, praça e posto também podem retirar a linha. Isso não identifica qual condição ocorreu no caso. [app/ponto/services/he_pendente_lavrado.py:89](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:89) [app/ponto/services/he_pendente_lavrado.py:114](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:114) [app/ponto/services/gestao_he.py:274](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:274) [app/ponto/services/gestao_he.py:317](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:317) **[não verificado]**

A sonda abaixo assume **2026** e interpreta `col868` como chave primária 868; essas correspondências precisam ser confirmadas no envelope **[não verificado]**. Ela imprime primeiro os nomes de todos os campos dos modelos diretamente consultados, usa as funções reais, abre uma transação PostgreSQL `READ ONLY` e força rollback. Os períodos do motor são impressos com suas datas de entrada e saída, sem atribuí-los por conta própria ao dia 14. O traceback preserva a pilha e a classe da exceção, omitindo a mensagem, que pode conter dado pessoal **[não verificado]**.

Execute o texto pelo envelope na sombra com `manage.py tenant_command shell --schema=juliani`. Não execute a geração do PDF: a view da Gestão chama o gerador auditável, enquanto a sonda chama os leitores que a precedem. [app/ponto/views.py:870](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:870)

```python
import datetime as dt
import sys
import traceback

ITEM = "A"
COL = 868
DIA = dt.date(2026, 11, 14)
ANO, MES = 2026, 11


def falha(etapa):
    print("col868 ERRO", etapa)
    traceback.print_tb(sys.exc_info()[2], file=sys.stdout)
    print("TIPO_EXCECAO", sys.exc_info()[0].__name__,
          "(mensagem omitida para proteção de dados)")


def passo(nome, funcao):
    try:
        # Um savepoint permite continuar após erro de consulta.
        with transaction.atomic():
            return funcao()
    except Exception:
        falha(nome)
        return None


try:
    from django.db import connection, transaction
    from django.utils import timezone
    from colaboradores.models import Colaborador, Empresa
    from inteligencia.models import MetricaSnapshot
    from ponto.models import Batida, DecisaoHE
    from ponto.janelas import janela_fechamento
    from ponto.services import gestao_he as G
    from ponto.services import he_pendente_lavrado as L
    from ponto.services.espelho import espelho_do_colab

    # Metadados locais dos modelos, antes de qualquer consulta ao banco.
    for modelo in (Colaborador, Empresa, MetricaSnapshot, Batida, DecisaoHE):
        print("col868 [nome]", modelo._meta.label,
              [campo.name for campo in modelo._meta.fields])

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION READ ONLY")

        def cadastro():
            c = Colaborador.objects.select_related("empresa").get(pk=COL)
            e = c.empresa
            print(
                "col868 CADASTRO",
                "situacao", c.situacao,
                "admissao", c.data_admissao,
                "demissao", c.data_demissao,
                "empresa_corresponde_jsp",
                "jsp" in (e.razao_social or "").lower()
                or "jsp" in (e.nome_fantasia or "").lower(),
                "corte", e.dia_inicio_competencia,
                "janela_ativa", e.janela_he_ativa,
                "janela_desde", e.janela_he_desde,
                "piso", e.janela_he_piso_min,
                "saida_ativa", e.janela_he_saida_ativa,
                "teto_entrada", e.janela_he_teto_min,
                "teto_saida", e.janela_he_saida_teto_min,
                "limite_decisao", e.limite_decisao_he_min,
            )
            return c

        c = passo("cadastro", cadastro)

        if c is not None:
            e = c.empresa
            janela = passo(
                "janela",
                lambda: janela_fechamento(MES, ANO, e),
            )
            if janela is not None:
                ini, fim = janela
                print("col868 JANELA", ini, fim,
                      "contem_dia", ini <= DIA <= fim)

                def retrato():
                    foto, calculado_em = L.ler(e, MES, ANO)
                    print("col868 RETRATO_EXISTE", foto is not None)
                    print("col868 RETRATO_CALCULADO_EM", calculado_em)
                    if foto is None:
                        return None
                    print("col868 RETRATO_JANELA",
                          foto.get("data_ini"), foto.get("data_ref"),
                          "erros_apuracao", foto.get("erros"))
                    linhas = [
                        x for x in (foto.get("colabs") or [])
                        if x.get("colab") == COL
                    ]
                    print("col868 RETRATO_COLAB_EXISTE", bool(linhas))
                    for linha in linhas:
                        dias = [
                            d for d in (linha.get("dias") or [])
                            if d.get("data") == str(DIA)
                        ]
                        print("col868 RETRATO_DIA_EXISTE", bool(dias))
                        for d in dias:
                            pontas = [
                                {k: p.get(k) for k in
                                 ("hora", "marco", "minutos", "sentido")}
                                for p in (d.get("pontas") or [])
                            ]
                            print("col868 RETRATO_DIA",
                                  "data", d.get("data"),
                                  "minutos", d.get("minutos"),
                                  "estado_na_foto", d.get("estado"),
                                  "pontas", pontas)
                    return foto

                foto = passo("retrato", retrato)

                def decisoes():
                    todos = list(
                        DecisaoHE.objects.filter(
                            colaborador_id=COL, data=DIA
                        ).values(
                            "estado", "origem", "minutos_na_decisao",
                            "revogada_em"
                        )
                    )
                    vivos = G.estado_por_dia([COL], DIA, DIA)
                    print("col868 DECISOES", todos)
                    print("col868 ESTADO_GESTAO",
                          vivos.get((COL, str(DIA)), "sem_decisao"))

                passo("decisoes", decisoes)

                def gestao():
                    vis = G.enriquecer(
                        foto,
                        limite=int(e.limite_decisao_he_min or 0),
                    )
                    linhas = [
                        x for x in vis["colabs"]
                        if x.get("colab") == COL
                    ]
                    print("col868 GESTAO_SEM_FILTROS_COLAB", bool(linhas))
                    for linha in linhas:
                        dias = [
                            d for d in (linha.get("dias") or [])
                            if d.get("data") == str(DIA)
                        ]
                        print("col868 GESTAO_SEM_FILTROS_DIA", bool(dias))
                        for d in dias:
                            print("col868 GESTAO_DIA",
                                  "minutos", d.get("minutos"),
                                  "estado", d.get("estado"),
                                  "acima_limite", d.get("acima"))

                    # Os valores efetivamente usados na tela observada não
                    # foram fornecidos. Estes dois recortes medem os filtros
                    # reais de dono sobre a foto atual.
                    for dono in ("admin", "sistema"):
                        filtrado = G.enriquecer(
                            foto,
                            limite=int(e.limite_decisao_he_min or 0),
                            dono=dono,
                        )
                        achou = any(
                            x.get("colab") == COL
                            and any(d.get("data") == str(DIA)
                                    for d in (x.get("dias") or []))
                            for x in filtrado["colabs"]
                        )
                        print("col868 GESTAO_FILTRO_DONO", dono, achou)

                passo("gestao", gestao)

                def espelho_e_motor():
                    agora = timezone.now()
                    hoje = timezone.localtime(agora).date()
                    esp = espelho_do_colab(
                        c, hoje, ini, fim,
                        ano=ANO, modo_gestor=True, agora=agora,
                    )
                    dias = [
                        d for d in (esp.get("dias") or [])
                        if d.get("data") == DIA
                    ]
                    print("col868 ESPELHO_DIA_EXISTE", bool(dias))
                    for d in dias:
                        pontas = [
                            {k: p.get(k) for k in
                             ("hora", "marco", "minutos", "sentido")}
                            for p in (d.get("he_fora_da_janela") or [])
                        ]
                        print("col868 ESPELHO_PONTAS", pontas)
                        print("col868 ESPELHO_TOTAL_REAL",
                              L.minutos_fora_do_dia(d))

                    resultado = esp["resultado"]
                    for grupo in ("periodos", "periodos_ft"):
                        for p in (getattr(resultado, grupo, None) or []):
                            entrada = getattr(p, "entrada", None)
                            saida = getattr(p, "saida", None)
                            if entrada is None:
                                continue
                            data_entrada = timezone.localtime(entrada).date()
                            if abs((data_entrada - DIA).days) <= 1:
                                print(
                                    "col868 MOTOR_PERIODO",
                                    "grupo", grupo,
                                    "entrada", entrada,
                                    "saida", saida,
                                    "minutos_fora_janela",
                                    getattr(p, "minutos_fora_janela", None),
                                )

                passo("espelho_e_motor", espelho_e_motor)

        # Também assegura rollback ao sair do bloco, inclusive se algum
        # leitor chamado vier a tentar mudar dados.
        transaction.set_rollback(True)

except Exception:
    falha("envelope")
finally:
    print("FIM_SONDA_" + ITEM)
```

A sonda mede o retrato atual e os valores atuais; **ela não estabelece, por si só, quando ocorreu a mudança nem quais filtros estavam na tela observada**. Sem esses instantes e filtros, não é possível atribuir um único dono causal ao caso **[não verificado]**.

## 18/11 13:41 · sessao ef03d825469f · LAUDO-[nome]-1811-1340 · leitura · origem=Code

**Resultado do item 1(b):** o material fornecido relata que col882 viu **10 min** no espelho e ciência na Gestão, mas não traz a saída das funções nem a linha de `DecisaoHE`. Portanto, os valores atuais das duas leituras, o estado, a origem e a vigência da decisão estão **[não verificado]**. Também não é possível confirmar pelo material que os “9,6 min” do RELATO se referem ao mesmo cálculo e às mesmas entradas do aviso de 10 min. **[não verificado]**

**Campos antes da leitura**

Os campos abaixo são os diretamente relevantes ao recorte e à sonda; as funções reais consultam também outros modelos e campos. A lista não pretende ser um inventário de todas as consultas internas. [app/ponto/services/espelho.py:971; app/relatorios/pdf_espelho.py:187]

- `Colaborador`: `empresa`, `posto`, `situacao`, além do identificador primário usado pela sonda; o modelo também contém `nome` e `cpf`, que não devem ser impressos. [app/colaboradores/models.py:319]
- `Empresa`: `janela_he_ativa`, `janela_he_piso_min`, `janela_he_teto_min`, `janela_he_saida_ativa`, `janela_he_saida_teto_min`, `janela_he_desde` e `limite_decisao_he_min`. [app/colaboradores/models.py:58; app/colaboradores/models.py:69; app/colaboradores/models.py:82; app/colaboradores/models.py:90; app/colaboradores/models.py:101; app/colaboradores/models.py:105; app/colaboradores/models.py:117]
- `Batida`: `colaborador`, `timestamp`, `tipo`, `is_intervalo`, `escala` e `origem`. [app/ponto/models.py:22; app/ponto/models.py:45]
- `EscalaColaborador`: `colaborador`, `posto`, `tipo_escala`, `data_inicio`, `data_fim`, `ativa`; `TipoEscala` inclui `intervalo_indenizavel`. A `CelulaDia` inclui `colaborador`, `data`, `trabalha`, `origem` e `escala_geradora`. [app/escala/models.py:871; app/escala/models.py:108; app/escala/models.py:1681]
- `MetricaSnapshot`: `chave`, `escopo`, `escopo_id`, `data_ini`, `data_ref`, `numerador`, `denominador`, `valor`, `detalhe`, `calculado_em`. A Gestão lê `detalhe` e `calculado_em` pela função `ler`. [app/inteligencia/models.py:81; app/ponto/services/he_pendente_lavrado.py:193]
- `DecisaoHE`: `colaborador`, `data`, `estado`, `minutos_na_decisao`, `motivo`, `decidida_por`, `origem`, `decidida_em`, `atualizada_em`, `revogada_em`, `revogada_motivo`, `minutos_na_revogacao`. `objects.vigentes()` filtra `revogada_em__isnull=True`. [app/ponto/models.py:1265; app/ponto/models.py:1300; app/ponto/models.py:1335]
- Saída do motor, em memória: `ResultadoMes.periodos` e `periodos_ft`; em cada `PeriodoCalculo`, `entrada`, `saida` e `minutos_fora_janela`. O motor pode publicar o mapa `dia_da_jornada` para relacionar período e dia. [app/ponto/motor_calculo_v2.py:207; app/ponto/motor_calculo_v2.py:234; app/ponto/motor_calculo_v2.py:268; app/ponto/motor_calculo_v2.py:1073]

**Funções reais e o que cada uma entrega**

- **Espelho:** a view chama `espelho_do_colab`, que obtém a autoridade do período e chama `montar_dias`. Este chama `marcar_pontas_fora`, acrescenta pontas publicadas pelo motor quando aplicável e entrega `he_fora_da_janela` ao template. A função marcadora testa a ponta contra o piso e arredonda os minutos para inteiro. O template imprime o valor e a expressão fixa “sem decisão”. [app/ponto/views.py:156; app/ponto/services/espelho.py:1225; app/ponto/services/espelho.py:1262; app/ponto/services/espelho.py:721; app/ponto/janela_he.py:215; app/templates/ponto/espelho.html:189]
- **Gestão:** chama `he_pendente_lavrado.ler` para obter a foto e `gestao_he.enriquecer`. A quantidade e os dias vêm da foto; `enriquecer` chama `estado_por_dia`, que consulta `DecisaoHE.objects.vigentes()`, e sobrepõe o estado vivo. `estado='nao'` é a decisão que representa ciência registrada. O valor da foto e o estado desta pessoa permanecem **[não verificado]**. [app/ponto/views.py:932; app/ponto/services/gestao_he.py:245; app/ponto/services/gestao_he.py:278; app/ponto/services/gestao_he.py:63; app/ponto/models.py:1283]
- **Motor:** a autoridade da tela chama `apurar_por_fatia` quando há escalas; no ramo sem escalas chama `motor.calcular_mes`. O cálculo de período passa por `_janela_do_par`, que chama `_entrada_efetiva` e `_saida_efetiva`; no ramo `Motor12x36`, as duas parcelas alimentam `minutos_fora_janela`. O valor do período de col882 é **[não verificado]**. [app/ponto/services/espelho.py:1162; app/ponto/motor_calculo_v2.py:1752; app/ponto/motor_calculo_v2.py:2750; app/ponto/motor_calculo_v2.py:2787]
- **PDF do espelho:** `_coletar_dados_espelho` chama `espelho_do_colab` para montar os dias; o desenho lê `dia['he_fora_da_janela']`. O coletor também apura o motor para seu próprio resultado. O valor do PDF para o caso é **[não verificado]**. [app/relatorios/pdf_espelho.py:187; app/relatorios/pdf_espelho.py:417; app/relatorios/pdf_espelho.py:596; app/relatorios/pdf_espelho.py:932]

A sonda abaixo assume **2026** e que `col882` representa a **PK 882**; a correspondência da etiqueta raspada com essa PK é **[não verificado]**. Ela usa a competência que `janela_atual` atribuir a 21/10 e informa seus limites. O espelho usa a data atual, como a view; isso não reconstrói necessariamente o que foi visto no momento do relato. [app/ponto/views.py:156; app/ponto/views.py:161; app/ponto/views.py:928]

Execute o bloco pelo envelope, no shell `manage.py tenant_command shell --schema=juliani`. Cada passo abre uma transação PostgreSQL somente de leitura; o traceback conserva arquivos, linhas e tipo da exceção, omitindo mensagem e texto-fonte para evitar dados pessoais. A sonda não foi executada aqui. **[não verificado]**

```python
import datetime as dt
import io
import traceback
from contextlib import redirect_stdout, redirect_stderr

from django.db import connection, transaction
from django.utils import timezone

DIA = dt.date(2026, 10, 21)
COLAB_PK = 882


def executar(item, funcao):
    try:
        # Cada passo tem sua própria transação; falha em um não impede os demais.
        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION READ ONLY")
            # As funções podem produzir mensagens não controladas: não publicá-las.
            with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
                resultado = funcao()
        print("col882", item, resultado)
    except Exception as exc:
        print("col882", item, "Traceback (most recent call last):")
        for frame in traceback.extract_tb(exc.__traceback__):
            print('  File "%s", line %s, in %s' %
                  (frame.filename, frame.lineno, frame.name))
        print("col882", item, "EXCECAO", type(exc).__name__,
              "(mensagem omitida)")
    finally:
        print("FIM_SONDA_" + item)


def contexto():
    from colaboradores.models import Colaborador
    from ponto.janelas import janela_atual

    colab = Colaborador.objects.select_related("empresa").get(pk=COLAB_PK)
    ini, fim = janela_atual(DIA, colab.empresa_id)
    return colab, ini, fim


def identificar():
    colab, ini, fim = contexto()
    return {
        "pk_encontrada": "col882",
        "dia": DIA.isoformat(),
        "competencia_inicio": ini.isoformat(),
        "competencia_fim": fim.isoformat(),
        "rotulo_da_competencia": "%02d/%04d" % (fim.month, fim.year),
    }


def espelho_e_motor():
    from ponto.services.espelho import espelho_do_colab

    colab, ini, fim = contexto()
    agora = timezone.now()
    esp = espelho_do_colab(
        colab, timezone.localdate(), ini, fim,
        ano=fim.year, agora=agora,
    )
    dias = [d for d in esp["dias"] if d.get("data") == DIA]
    resultado = esp["resultado"]
    mapa = getattr(resultado, "dia_da_jornada", None)
    periodos = list(resultado.periodos) + list(resultado.periodos_ft)

    # O mapa é a atribuição publicada pelo motor. Sem ele, não se infere [nao verificado]
    # o dia do turno a partir da data civil da entrada.
    if mapa is None:
        motor = "dia_da_jornada nao publicado; valor do dia nao verificado" [nao verificado]
    else:
        sem_atribuicao = sum(1 for p in periodos if id(p) not in mapa)
        motor = {
            "minutos_fora_por_periodo_atribuido_ao_dia": [
                p.minutos_fora_janela
                for p in periodos if mapa.get(id(p)) == DIA
            ],
            "periodos_sem_atribuicao": sem_atribuicao,
        }

    return {
        "linhas_do_dia_no_espelho": len(dias),
        "avisos_minutos_e_sentido": [
            {
                "minutos": ponta.get("minutos"),
                "sentido": ponta.get("sentido"),
            }
            for dia in dias
            for ponta in (dia.get("he_fora_da_janela") or [])
        ],
        "motor": motor,
    }


def gestao():
    from ponto.services.he_pendente_lavrado import ler
    from ponto.services.gestao_he import enriquecer, estado_por_dia

    colab, ini, fim = contexto()
    retrato, calculado_em = ler(colab.empresa, fim.month, fim.year)
    estado = estado_por_dia([COLAB_PK], DIA, DIA)

    if retrato is None:
        return {
            "retrato": "ausente",
            "estado_por_dia": estado.get((COLAB_PK, DIA.isoformat())),
        }

    vis = enriquecer(
        retrato,
        limite=int(colab.empresa.limite_decisao_he_min or 0),
    )
    linhas = [c for c in vis.get("colabs", []) if c.get("colab") == COLAB_PK]
    dias = [
        d for c in linhas for d in (c.get("dias") or [])
        if str(d.get("data")) == DIA.isoformat()
    ]
    return {
        "retrato_calculado_em": calculado_em.isoformat() if calculado_em else None,
        "colab_no_resultado": bool(linhas),
        "dia_na_foto_enriquecida": bool(dias),
        "dias": [
            {"minutos": d.get("minutos"), "estado": d.get("estado")}
            for d in dias
        ],
        "estado_por_dia": estado.get((COLAB_PK, DIA.isoformat())),
    }


def decisao_vigente():
    from ponto.models import DecisaoHE

    linhas = list(
        DecisaoHE.objects.vigentes()
        .filter(colaborador_id=COLAB_PK, data=DIA)
        .values("estado", "origem", "minutos_na_decisao", "revogada_em")
    )
    return {
        "quantidade_vigente": len(linhas),
        "decisoes": [
            {
                "estado": linha["estado"],
                "origem": linha["origem"],
                "minutos_na_decisao": linha["minutos_na_decisao"],
                "vigente_pela_porta": True,
            }
            for linha in linhas
        ],
    }


def pdf():
    from relatorios.pdf_espelho import _coletar_dados_espelho_mes

    colab, ini, fim = contexto()
    dados = _coletar_dados_espelho_mes(
        colab, fim.month, fim.year, agora=timezone.now()
    )
    dias = [d for d in dados["dias"] if d.get("data") == DIA]
    return {
        "linhas_do_dia": len(dias),
        "avisos_minutos_e_sentido": [
            {
                "minutos": ponta.get("minutos"),
                "sentido": ponta.get("sentido"),
            }
            for dia in dias
            for ponta in (dia.get("he_fora_da_janela") or [])
        ],
    }


executar("CONTEXTO", identificar)
executar("ESPELHO_MOTOR", espelho_e_motor)
executar("GESTAO", gestao)
executar("DECISAO_VIGENTE", decisao_vigente)
executar("PDF", pdf)
```

Se o mapa de atribuição do motor não estiver publicado ou deixar períodos sem atribuição, a sonda declara essa limitação: uma lista vazia, nesse caso, não prova zero para o dia. A consulta de decisão vigente também não identifica eventual linha revogada; uma lista vazia significa apenas que a porta `vigentes()` não retornou decisão para o dia. [app/ponto/motor_calculo_v2.py:1073; app/ponto/models.py:1265]

## 18/11 13:32 · sessao 269d7cb7f1fb · POUSOS-COMBOIO-1811-1332 · verificador · origem=Code

**Item 1 — PROCEDE: AVAL-DEVOLVIDO para a condição literal, enquanto ela não for comprovada no resultado.** Os scripts refeitos melhoram o recorte, mas o dado não permite concluir que o texto registrado para **cada** corte será o literal exigido. Os demais pontos abaixo não demonstram, por si, colisão com o complemento. [anexos `doc_comum.py`, `aplica_doc_p2.py`, `aplica_doc_f4.py` e aval principal: não verificado em arquivo]

1. **Premissas — parcialmente conferem; resultado ainda não verificado.** `RESPOSTAS-DEVOLVIDOS-1711` já consta no JSON da cópia em [CORTES.json:1910](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/CORTES.json:1910). O gerador lê o JSON e aponta `CORTES.md` como destino, em [gerar_cortes.py:22](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/gerar_cortes.py:22). Isso não comprova que a geração será executada no pouso nem que o Markdown resultante estará atualizado. [não verificado]

   O censo dos diffs, a identidade com `81f678c7e`, a aplicação dos scripts nos commits, os selos e os fatos de produção não são conferíveis pelos dados disponíveis. O complemento os coloca como condições ou premissas, mas sua satisfação permanece [não verificado].

2. **Colisão — não demonstrada.** Incluir os três cortes adicionais **não excede, por si só, o escopo**: a alínea **(b) do item 1** exige que cada corte citado nos `.py` e testes esteja no JSON, independentemente de ter sido nomeado no aval principal. A completude do censo e a fidelidade dos três registros às fontes permanecem [não verificado]. [nao verificado]

   A leitura de que a conferência pela sessão principal substitui o opus curto **procede no ponto de divergência**: o complemento atribui a conferência à sessão principal, sem Agent, e determina sua prevalência onde divergir do principal. Outras exigências vigentes de revisão não fornecidas são [não verificado].

   Quanto ao item 3 de `LIBERA-B3-F2-P2`, a omissão no complemento não demonstra revogação. A decisão informada de cumpri-lo é compatível com essa leitura; o texto integral e a vigência do aval anterior, bem como o cumprimento efetivo de drenagem, medições e parada, são [não verificado].

3. **Literal — condição ainda não fecha comprovadamente.** Recortar linhas pode preservar **literalmente os trechos escolhidos** sem reproduzir o arquivo inteiro. Isso não prova que a seleção seja suficiente para a exigência de “o texto de cada corte” nem que não omita contexto necessário; os avais de origem não foram fornecidos nesta volta para confronto. [não verificado]

   Há também uma limitação concreta no script fornecido: `literal()` usa `strip()` e une as linhas com ` | `, portanto o campo resultante não preserva exatamente a formatação nem a continuidade original. Além disso, `registra()` **não atualiza um ID já existente**: mesmo que leia o aval de origem, pode conservar no JSON uma frase anterior que não foi conferida como literal. Assim, a verificação de prefixo único não basta para garantir o conteúdo final de cada entrada. [anexo `doc_comum.py`: não verificado em arquivo]

   Para prefixo ausente ou repetido, o `assert` interrompe a avaliação antes de `registra()` receber a lista daquele script; isso sustenta a devolução **para essa falha específica**, mas não comprova atomicidade de toda a operação documental. [anexos `doc_comum.py`, `aplica_doc_p2.py` e `aplica_doc_f4.py`: não verificado em arquivo]

4. **Exportada, dinheiro ou `!` — toque fora do declarado não demonstrado.** O complemento proíbe tocar exportada; o deploy normal pode executar migrations antes do reload, conforme [deploy.sh:30](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/deploy.sh:30) e [deploy.sh:316](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/deploy.sh:316). Os diffs efetivos e efeitos das migrations e do pouso sobre exportada, dinheiro ou `!` são [não verificado].

## 18/11 13:31 · sessao 922407afe52f · E4C-F6F7 · construtor · origem=Code

As curas C1–C4 estão aplicadas. As verificações pontuais sem banco passaram; o fechamento dos 18 vermelhos da suíte permanece **[não verificado]**. A suíte não foi executada. [nao verificado]

- **C1:** escolhi o default `False` nas fixtures, igual ao campo real, mantendo as asserções existentes. Arquivos: [test_e3_c8b_sem_banco.py:68](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_c8b_sem_banco.py:68), [test_e3_pos_suite_sem_banco.py:27](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_pos_suite_sem_banco.py:27), [test_e3_portas_tarefa.py:725](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_portas_tarefa.py:725), [test_e3_r3_c7_sem_banco.py:247](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_r3_c7_sem_banco.py:247), [test_e3_tarefa_admin_sem_banco.py:123](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:123). A escolha acompanha o [default do modelo:4195](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4195); o mutante que retira o campo volta a provocar o erro no selo ([teste:74](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:74)). [nao verificado]

- **C2:** a fixture compila `_fatos_das_perguntas` no namespace da classe, usando o corpo do módulo ([test_e3_r3_c7_sem_banco.py:211](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_r3_c7_sem_banco.py:211)). As asserções de status e relações continuam no selo; retirar a função reproduz `NameError` ([mutante:82](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:82)).

- **C3:** os selos agora verificam por AST a chamada com `colab` como primeiro argumento e o uso de `pendentes`, aceitando argumentos nomeados ([test_pergunta_no_app.py:29](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:29), [test_contract_pergunta_viva.py:26](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:26)). Cada arquivo contém `test_a_costura_MORDE`, que troca a fonte ou remove a chamada e exige rejeição ([primeiro:189](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:189), [segundo:110](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:110)). A cadeia adicional até `q_vivas` e o juiz continua no segundo selo ([linha 94](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:94)).

- **C4:** o contador entrega o universo à autoridade e conta apenas as linhas de validação que ela devolve ([backfill_decisoes_pergunta.py:104](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:104), [autoridade:4238](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4238)). Escolhi essa opção porque a autoridade já aceita perguntas sem carimbo; não acrescentei exceção à allowlist. O teste sem banco exercita a autoridade real e distingue validação sem linha, validação correspondente e resposta ([teste:30](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:30)). A varredura agora percorre mais perguntas; o custo em produção é **[não verificado]**. [nao verificado]

Para G5, a seleção de células geradas futuras usa `data__gt=hoje` sem teto de competência ([cadastro_futuro.py:22](/home/ronald/wt-codex-e4c-f6f7-132608/app/ponto/services/cadastro_futuro.py:22)); o resultado dos dois testes de folga nesta árvore é **[não verificado]**. [nao verificado]

Os mutantes executados estão em [test_e4c_f6f7_c7_sem_banco.py](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:53). Cada comando abaixo passou, confirmando que o caso detecta o respectivo mutante: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.MutantesDasFixtures.test_sem_default_da_resposta_MORDE
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.MutantesDasFixtures.test_sem_funcao_no_namespace_MORDE
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.CosturasSemDjango.test_selos_e_mutantes_MORDEM
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.ContadorSemBanco.test_mutante_conta_tambem_respostas_MORDE
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.ContadorSemBanco.test_mutante_reintroduz_filtro_cru_MORDE
```

Também passaram 36 testes pontuais de C1/C2, os seis testes sem banco do arquivo novo e o selo C4 com `SimpleTestCase` substituído por `unittest.TestCase`. `py_compile` passou nos nove arquivos alterados, com a escrita de bytecode desativada. A matriz e os selos que importam Django não iniciaram no interpretador disponível porque `django.contrib` e `django.test` não estavam disponíveis; seu resultado permanece **[não verificado]**. [nao verificado]

LEI-AKITA: origem=[tarefa_do_dia.py:164](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/tarefa_do_dia.py:164) + [backfill_decisoes_pergunta.py:94](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:94), testemunha=[models.py:4195](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4195) + [models.py:4238](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4238) + [pergunta_no_app.py:68](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/pergunta_no_app.py:68), RED=18 vermelhos da suíte b66587c26 fornecidos no anexo **[não verificado]**, quem-mais-le=[aviso_pendencia.py:9](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/aviso_pendencia.py:9) + [cobranca.py:48](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/cobranca.py:48) + [tarefa_do_dia.py:436](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/tarefa_do_dia.py:436), juizes novos=0, padrao=Single Source of Truth / Aggregate Root ([GLOSSARIO.md:11](/home/ronald/wt-codex-e4c-f6f7-132608/app/docs/GLOSSARIO.md:11)).

## 18/11 13:30 · sessao 49422d7c0b03 · POUSOS-COMBOIO-1811-1332 · verificador · origem=Code

**Item 1 — PROCEDE: AVAL-DEVOLVIDO.** A condição de registrar o texto **literal** de cada corte não fecha com os scripts fornecidos. **Item 3 — inconclusivo:** o dado fornecido não permite comprovar uma colisão nem certificar todas as premissas necessárias para chamá-lo de CONFERE. [anexo `pousos_comboio_1811_1332.txt`, itens 1 e 3, dado raspado; arquivo:linha não conferível]

Os anexos abaixo estão no dado raspado, mas não foram encontrados como arquivos nesta cópia. Por isso, posso comparar seu conteúdo fornecido, sem atribuir-lhes números de linha. A cópia também não tem metadados Git acessíveis: os hashes, diffs e certificações informados são **não conferíveis** aqui. [não verificado]

**Item 1 — E4C-F2-P2 e F4**

1. **Premissas.** `LEIS.md:151` é de fato a L-130, e contém os dois trechos que o script P2 procura alterar. A L-106 exige docs no commit do marco. [app/docs/LEIS.md:151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:151), [app/docs/LEIS.md:132](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:132)

   Os cinco avais de origem constam do dado raspado, com texto identificável; sua existência nos caminhos `logs/AVAIS-*` não é conferível nesta cópia. O item 1 menciona, entre parênteses, RESPOSTAS-1711-1050 e LIBERA-B3-F2-P2 para a P2, enquanto o script insere CORTE-F2-RESSUSCITAR-GRAVA-ATO e RESPOSTAS-1711-1050: a correspondência pretendida precisa ser esclarecida. [anexos `pousos_comboio_1811_1332.txt`, `aplica_doc_p2.py` e cinco avais fornecidos; arquivo:linha não conferível]

2. **Colisão com aval vigente.** LIBERA-B3-F2-P2 item 3 exige drenar os crons antes do migrate e do reload; depois do reload, medir N1 e N2 com denominadores e a corrida de `respondida_em`; qualquer resultado maior que zero mantém os crons desligados e exige PAREI. O item 1 manda fazer deploy e smoke, mas não repete essas condições. A omissão, por si só, não as revoga; executar apenas a sequência resumida do item 1 sem cumpri-las colidiria com o aval anterior. [anexos `libera_b3_f2p2_1758.txt`, item 3, e `pousos_comboio_1811_1332.txt`, item 1; arquivo:linha não conferível] [nao verificado]

3. **Literal.** O aval exige texto literal e devolução se a origem não for achada no literal. As frases dos scripts P2 e F4 são resumos: por exemplo, a frase do COMPLEMENTO-1 reúne os três itens em uma redação condensada, em vez de reproduzir o texto fornecido; a frase RESPOSTAS-1711-1050 também seleciona e resume itens. Essa é uma incompatibilidade comprovável no próprio dado fornecido e sustenta a devolução. Os scripts fornecidos escrevem `CORTES.json` e, no caso P2, `LEIS.md`; não escrevem diretamente `CORTES.md`, embora o item o mencione. Outro mecanismo que atualize o `.md` é **não verificado**. [anexos `aplica_doc_p2.py`, `aplica_doc_f4.py`, `complemento1_corte_u163_no_k6_1611.txt` e `respostas_1711_1050.txt`; arquivo:linha não conferível]

   A instrução de que o opus confira apenas a entrada do doc está expressa no aval; uma obrigação vigente de releitura integral de código que a contradiga é **não verificada**. [anexo `pousos_comboio_1811_1332.txt`, item 1; arquivo:linha não conferível]

4. **Exportada, dinheiro ou `!`.** O deploy normal pode aplicar migrations pendentes antes do reload; portanto, o pouso não é uma operação exclusivamente documental. O conteúdo e efeito da migration 0056 da P2, ausente desta cópia, são **não conferíveis**. Não há dado suficiente para afirmar toque em exportada, dinheiro ou `!` fora do declarado. [bin/deploy.sh:30](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/deploy.sh:30), [bin/deploy.sh:317](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/deploy.sh:317), [não verificado]

**Item 3 — Cirineu sem ESCALA**

1. **Premissas.** A certificação de `fa49abfb6`, a composição de `7bc85383d` e o diff entre esta e a suite A são **não conferíveis** sem os objetos Git ou logs. O aval fornecido reconhece que a árvore sem ESCALA não foi a certificada e exige suite inteira e opus aprovados antes do pouso. [anexo `pousos_comboio_1811_1332.txt`, item 3; arquivo:linha não conferível]

2. **Colisões.** O texto integral e a vigência de [nome]-12H e COMBOIO-DAS-SUITES, bem como a existência e o estado da suite em `4ec3e15d0`, são **não conferíveis**. A ordem de rodar outra suite em paralelo não demonstra, sozinha, que a suite em curso será interrompida ou que a Cirineu tomará a vez da espinha; essas colisões permanecem **não verificadas**. [anexo `pousos_comboio_1811_1332.txt`, item 3; arquivo:linha não conferível]

3. **Literal.** Commits adicionais **apenas de documentação** não contradizem, por si, a proibição de mudar `.py`. São duas verificações distintas: comparar os `.py` com a árvore certificada e demonstrar que a suite verde cobre a árvore efetivamente pousada segundo a regra aplicável. O conteúdo dos commits adicionais, a regra completa de certificação e o resultado dessas verificações são **não conferíveis**. O script de doc da Cirineu fornecido altera `CORTES.json`, sem escrever `.py`. [anexos `pousos_comboio_1811_1332.txt`, item 3, e `aplica_doc_cirineu.py`; arquivo:linha não conferível] [nao verificado]

4. **Exportada, dinheiro ou `!`.** O item proíbe tocar exportada e regerar célula antes da simulação. O efeito real do diff Cirineu e do pouso em exportada, dinheiro ou `!` é **não conferível** nesta cópia. [anexo `pousos_comboio_1811_1332.txt`, item 3; arquivo:linha não conferível] [nao verificado]

## 18/11 13:25 · sessao 53c0ec5d1e4d · E4C-F6F7 · leitura · origem=Code

A triagem aponta **quatro grupos de causa**. Os 11 `AttributeError`, incluindo os dois subtestes Q3/Q4, também explicam o `FAIL` agregado da matriz; não há evidência no anexo de uma segunda divergência naquela matriz. A cura de cada grupo e seu resultado na suíte permanecem **[não verificado]**. [nao verificado]

A cópia não tem metadados Git. Comparei os arquivos com a cópia local identificada por `dd489c627`, mas não executei a suíte naquela base; portanto, a reprodução dos vermelhos nela é **[não verificado]**.

### Tabela por teste [nao verificado]

Os caminhos abaixo são relativos à árvore `b66587c26`.

| Teste vermelho | Causa e classe | Cura mínima proposta | [nao verificado]
|---|---|---|
| `BordasDaCura.test_grave_respondida_em_disputa_fechada_nao_e_filtrada_por_motivo` | **G1 — A:** decisão sintética sem o atributo agora lido diretamente. [teste:68](app/chamados/tests/test_e3_c8b_sem_banco.py:68), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `BordasDaCura.test_idempotente` | **G1 — A:** mesma fixture. [teste:63](app/chamados/tests/test_e3_c8b_sem_banco.py:63), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `BordasDaCura.test_schema_sem_chave_nova` | **G1 — A:** mesma fixture. [teste:68](app/chamados/tests/test_e3_c8b_sem_banco.py:68), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `BordasDaCura.test_sem_trabalho_sem_lampadas_nao_esconde_resposta_viva` | **G1 — A:** resposta sintética sem o atributo. [teste:188](app/chamados/tests/test_e3_c8b_sem_banco.py:188), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `AlimentacaoC6.test_carga_da_tarefa_pergunta_ao_c6_avulso_lote_e_idempotencia` | **G1 — A:** a carga sintética entrega decisão sem o atributo. [teste:215](app/chamados/tests/test_e3_c8b_sem_banco.py:215), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `PosSuiteSemBanco.test_linha_sintetica_de_pergunta_sem_marco_tem_data_civil_desconhecida` | **G1 — A:** resposta sintética sem o atributo. [teste:26](app/chamados/tests/test_e3_pos_suite_sem_banco.py:26), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `MatrizV8Test.test_v8_R15_produto_decisoes_residual_A`, subtestes **Q3 e Q4** | **G1 — A:** os dois `SimpleNamespace` de resposta omitem o atributo. [teste:723](app/chamados/tests/test_e3_portas_tarefa.py:723), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `MatrizV8Test.test_v8_R15_produto_decisoes_residual_A`, **FAIL agregado** | **G1 — A, consequência:** Q3 e Q4 abortam antes de `coletor.append`; a comparação final recebe duas entradas a menos. [teste:741](app/chamados/tests/test_e3_portas_tarefa.py:741), [teste:754](app/chamados/tests/test_e3_portas_tarefa.py:754), [teste:758](app/chamados/tests/test_e3_portas_tarefa.py:758). | C1; sem produção. Outras divergências após corrigir as exceções: **[não verificado]**. |
| `F3Terminal.test_beco_resposta_E1_ancora_resolvida_sem_porta` | **G1 — A:** resposta sintética sem o atributo. [teste:246](app/chamados/tests/test_e3_r3_c7_sem_banco.py:246), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `RedBRespostaTextual.test_B_252_29_set_hipotese_ultima_resposta_textual` | **G1 — A:** hipótese E1 construída sem o atributo. [teste:119](app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:119), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `RedBRespostaTextual.test_B_841_10_out_hipotese_ultima_resposta_textual` | **G1 — A:** mesma construção. [teste:119](app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:119), [produção:164](app/chamados/services/tarefa_do_dia.py:164). | C1; sem produção. |
| `CargaStatusDasAncoras.test_status_em_lote_destino_prevalece_container_sem_N_mais_1` | **G2 — A:** a obra extraiu `_fatos_das_perguntas`; o teste compila a classe isolada, mas seu namespace não contém a nova função global. [produção:377](app/chamados/services/tarefa_do_dia.py:377), [produção:517](app/chamados/services/tarefa_do_dia.py:517), [fixture:80](app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:80), [teste:209](app/chamados/tests/test_e3_r3_c7_sem_banco.py:209). | C2; sem produção. |
| `ContadorPassivoECopiloto.test_a_view_do_app_le_a_mesma_fonte` | **G3 — A:** selo exige a chamada literal antiga; a view agora passa `agora=agora`. [teste:168](app/chamados/tests/test_pergunta_no_app.py:168), [view:2540](app/api/views.py:2540). | C3; com produção. |
| `ExibicaoUsaADerivacaoTest.test_endpoint_do_app_filtra_por_q_vivas` | **G3 — A:** mesma mudança de grafia da chamada inspecionada pelo selo. [teste:69](app/chamados/tests/test_contract_pergunta_viva.py:69), [view:2540](app/api/views.py:2540). | C3; com produção. |
| `LeitoresVitalidadeContract.test_ninguem_julga_vitalidade_por_carimbo_cru` | **G4 — A:** o novo contador filtra `validada_em__isnull=False`; o verificador AST identifica essa forma sem distinguir o universo de um censo de um julgamento de vitalidade. [contador:94](app/chamados/management/commands/backfill_decisoes_pergunta.py:94), [selo:329](app/chamados/tests/test_contract_leitores_vitalidade.py:329), [asserção:343](app/chamados/tests/test_contract_leitores_vitalidade.py:343). | C4; sem produção. |
| `TorneiraFolgasTest.test_plano_manual_relavra_a_celula_ja_gerada` | **G5 — D:** a fixture escolhe hoje + 3 dias; no traceback isso dá 21/11, fora da janela corrente que termina em 20/11 para corte 21. A regeneração futura limita a busca à janela. [teste:71](app/escala/tests/test_torneira_folgas.py:71), [janela:10](app/ponto/janelas.py:10), [futuras:26](app/ponto/services/cadastro_futuro.py:26). Código e teste correspondentes são iguais na cópia da base; vermelho executado na base **[não verificado]**. | C5; sem produção para estabilizar a fixture. |
| `TorneiraFolgasTest.test_calendario_relavra_a_celula_ja_gerada` | **G5 — D:** mesma data e mesmo limite de regeneração, por outro escritor de folga. [teste:90](app/escala/tests/test_torneira_folgas.py:90), [relavra:289](app/escala/folgas.py:289), [futuras:26](app/ponto/services/cadastro_futuro.py:26). Reprodução na base **[não verificado]**. | C5; sem produção para estabilizar a fixture. |

### Grupos e curas

- **C1 — decisões sintéticas:** acrescentar `resposta_mantem_validacao=False` às respostas das fixtures antigas, sem trocar suas asserções. O campo do modelo real tem esse default; os testes novos que exercitam `True` devem continuar cobrindo esse caso. [modelo:4072](app/chamados/models.py:4072), [teste F6:19](app/chamados/tests/test_f6_t_resposta_mantem_validacao.py:19). **Produção: não.** A alteração na obra que desencadeia a incompatibilidade está em [tarefa_do_dia.py:159](app/chamados/services/tarefa_do_dia.py:159).

- **C2 — função extraída:** incluir `_fatos_das_perguntas` no namespace compilado pela fixture e fornecer as dependências sintéticas necessárias. O `NameError` é causado pela chamada nova em [tarefa_do_dia.py:517](app/chamados/services/tarefa_do_dia.py:517); se surgirá outro erro depois da injeção é **[não verificado]**. **Produção: não.**

- **C3 — selos literais do app:** manter o mesmo instante para ambas as consultas, vinculando `agora` à função localmente, por exemplo com `functools.partial`, e conservar a chamada literal `consulta = consulta_do_app(colab)` exigida pelos dois selos. A função continua usando a seleção e partição do app em [pergunta_no_app.py:45](app/chamados/services/pergunta_no_app.py:45); os tracebacks, por si, não demonstram seleção incorreta. **Produção: sim; resultado [não verificado].**

- **C4 — contador censitário:** adicionar à `ALLOWLIST` do selo uma exceção específica e justificada para `contador_validada_projetada_sem_linha`, preservando a asserção. O contador seleciona projeções carimbadas e delega a comparação ato/instante à autoridade; há exceção semelhante para outro censo do backfill. [contador:100](app/chamados/management/commands/backfill_decisoes_pergunta.py:100), [autoridade:4122](app/chamados/models.py:4122), [exceção existente:282](app/chamados/tests/test_contract_leitores_vitalidade.py:282). **Produção: não.**

- **C5 — data da fixture de folga:** controlar o relógio e escolher um dia futuro dentro da janela corrente, mantendo a célula previamente criada e as asserções dos dois selos. Isso elimina a dependência de `hoje + 3` atravessar o corte. [teste:54](app/escala/tests/test_torneira_folgas.py:54), [teste:72](app/escala/tests/test_torneira_folgas.py:72), [janela:290](app/ponto/management/commands/gerar_celulas.py:290). **Produção: não para essa cura.** Se o contrato exigir relavrar também células futuras já existentes *fora* da janela, a implementação atual não as seleciona e seria necessária alteração de produção; essa exigência está **[não verificado]**. [cadastro_futuro.py:26](app/ponto/services/cadastro_futuro.py:26).

### Quem escreve e quem lê os campos e funções relevantes

| Campo ou função | Escrita / produção | Leitura |
|---|---|---|
| `resposta_mantem_validacao` | Porta humana grava `False`; resposta automática e trilha gravam `True`; a foto da projeção produz `True`. [disputa_emissao.py:1525](app/chamados/services/disputa_emissao.py:1525), [disputa_emissao.py:1674](app/chamados/services/disputa_emissao.py:1674), [disputa_emissao.py:1698](app/chamados/services/disputa_emissao.py:1698), [models.py:4139](app/chamados/models.py:4139). | A obra passa a lê-lo na cauda de respostas de `_pela_decisao`. [tarefa_do_dia.py:159](app/chamados/services/tarefa_do_dia.py:159). |
| `_fatos_das_perguntas` | A função monta fatos em memória a partir de decisões, projeção, lavra, efeitos e fila. [tarefa_do_dia.py:377](app/chamados/services/tarefa_do_dia.py:377). | `exigencia_das_perguntas` e `CargaDaTarefa` consomem seu retorno. [tarefa_do_dia.py:436](app/chamados/services/tarefa_do_dia.py:436), [tarefa_do_dia.py:517](app/chamados/services/tarefa_do_dia.py:517). |
| `consulta_do_app` e `agora` | A view obtém o instante e o passa à consulta; a função produz a partição de perguntas. [api/views.py:2538](app/api/views.py:2538), [pergunta_no_app.py:64](app/chamados/services/pergunta_no_app.py:64). | A view lê `pendentes` e `historico` e passa o mesmo instante à exigência por pergunta. Outros leitores incluem [aviso_pendencia.py:9](app/chamados/services/aviso_pendencia.py:9) e [cobranca.py:48](app/chamados/services/cobranca.py:48). [api/views.py:2541](app/api/views.py:2541). |
| `aguarda_admin` | A view escreve a chave do JSON a partir de `aguarda_admin_pela_tarefa`. [api/views.py:2554](app/api/views.py:2554), [tarefa_do_dia.py:457](app/chamados/services/tarefa_do_dia.py:457). | Consumidores externos da chave: **[não verificado]**. |
| `validada_em` e `linhas_da_projecao_sem_linha` | `_carimbar_validacao` grava o carimbo e registra a decisão correspondente; a autoridade produz linhas faltantes a partir da projeção. [validacao.py:161](app/chamados/services/validacao.py:161), [models.py:4122](app/chamados/models.py:4122). | O novo contador lê o carimbo para formar seu universo e chama a autoridade; a carga de fatos também chama a autoridade. [backfill_decisoes_pergunta.py:94](app/chamados/management/commands/backfill_decisoes_pergunta.py:94), [tarefa_do_dia.py:408](app/chamados/services/tarefa_do_dia.py:408). |

Nenhuma cura foi aplicada ou executada; o fechamento efetivo dos vermelhos e possíveis falhas encobertas são **[não verificado]**. [nao verificado]

## 18/11 13:24 · sessao bf6ef124d773 · - · - · origem=Ronald (tmux)

Resultados de escala e do contrato de vitalidade enviados ao agente principal, com referências de arquivo e linha. A suíte da base não foi executada; a reprodução nela permanece [não verificado]. [nao verificado]

## 18/11 13:24 · sessao 3401826c5161 · - · - · origem=Ronald (tmux)

Análise enviada ao agente principal, com causas agrupadas, locais das fixtures, escritores e leitor do campo, comparação com a base e curas propostas. O resultado após as curas permanece [não verificado].

## 18/11 13:24 · sessao 6fdfcb3b8125 · - · - · origem=Ronald (tmux)

As duas falhas dos selos do app decorrem da mesma alteração na chamada de `consulta_do_app`: a view passou a fornecer `agora`, enquanto os testes exigem a chamada literal anterior ([api/views.py:2537](/home/ronald/codex_ws/b66587c26b63b818e192f0e00d3cf4c20a6e0c60/app/api/views.py:2537), [test_pergunta_no_app.py:168](/home/ronald/codex_ws/b66587c26b63b818e192f0e00d3cf4c20a6e0c60/app/chamados/tests/test_pergunta_no_app.py:168), [test_contract_pergunta_viva.py:69](/home/ronald/codex_ws/b66587c26b63b818e192f0e00d3cf4c20a6e0c60/app/chamados/tests/test_contract_pergunta_viva.py:69)).

A cura proposta preserva os selos e o relógio comum: vincular `agora` à função localmente e manter a chamada literal exigida; ela altera código de produção e seu resultado ainda não foi executado [não verificado].

## 18/11 13:00 · sessao 6e1ba8910912 · EFEITOS-REAIS-1811 · leitura · origem=Code

1. **Corrente real.** O cron chama `processar_cartorio --apply` por empresa; ele executa `julgar_colab` nas células passadas. Depois vêm, encadeados, `apurar_furos_diarios` e `disparar_perguntas_competencia --comp-corrente --alarme`; este último é **vigia, não escritor de perguntas**. [app/config/crons.py:616] [app/config/crons.py:1527] [app/config/crons.py:1539] [app/ponto/management/commands/processar_cartorio.py:33] [app/chamados/management/commands/disparar_perguntas_competencia.py:130]

   No julgamento, se houver `FURO_PARCIAL`, o cartório identifica os marcos apagados e chama `detectar_ausencias.Command._abrir_chamado`, que abre ou reaproveita o `batida_ausente` e grava seus marcos. Esse emissor chama `criar_disputa_a_partir_de_chamados_orfaos`; para chamado elegível de dia passado, `cobra_marcos_da_ata` encaminha à leitura da ata e `_complementar_marcos_faltantes` cria a pergunta do marco. Há também nascimento **direto pela célula julgada**, por `perguntar_pela_celula` → `perguntar` → `gerar_perguntas_disputa`. Alterações de batida ou de status de chamado também podem acionar julgamento por signals. [app/ponto/services/cartorio.py:1814] [app/ponto/services/cartorio.py:1887] [app/ponto/management/commands/detectar_ausencias.py:694] [app/ponto/management/commands/detectar_ausencias.py:788] [app/chamados/services/disputa_emissao.py:736] [app/chamados/services/disputa_emissao.py:1180] [app/chamados/services/disputa_emissao.py:864] [app/ponto/services/cartorio.py:1029] [app/chamados/services/fabrica_da_celula.py:160] [app/ponto/signals.py:132] [app/chamados/signals.py:189]

   O ramo para categoria `intervalo_ausente` lê os marcos do contexto, mas o detector específico está com emissão aposentada; seu cron roda em alarme. [app/chamados/services/disputa_emissao.py:1150] [app/ponto/management/commands/detectar_intervalo_ausente.py:150] [app/config/crons.py:491]

2. **Não é possível identificar uma guarda única que tenha impedido cada caso com o anexo fornecido.** O caminho direto exige que o veredito da célula acuse; se houver veredito fora de `VEREDITOS_ACUSAM`, retorna antes de consultar os motivos da ata. O caminho do chamado depende de `FURO_PARCIAL`, entre outras condições; esse código exige, por exemplo, dia encerrado, fato e célula da grade com `missing`. A existência de uma lâmpada apagada na ata, por si só, não prova quais insumos, código e veredito foram usados no julgamento anterior. [app/chamados/services/fabrica_da_celula.py:119] [app/ponto/supra_juiz.py:318] [app/ponto/supra_juiz.py:392] [app/ponto/supra_juiz.py:429]

   - **(a) Sem chamado nem pergunta:** não há chamado para o ramo de repasse; o nascimento direto ainda pode ocorrer em julgamento elegível. Qual guarda incidiu no caso concreto: **[não verificado]**. [app/chamados/services/disputa_emissao.py:1057] [app/chamados/services/fabrica_da_celula.py:178]
   - **(b) Chamado resolvido e pergunta `saida_sem_entrada` resolvida:** chamado encerrado fica fora da fábrica de órfãos; a abertura de chamado usa identidade por colaborador, módulo e dia e pode tentar renascimento, sujeito às guardas da porta. Isso não demonstra que o chamado antigo bloqueou definitivamente um novo marco. A pergunta de outro motivo não é, por si, deduplicação da pergunta de intervalo: a identidade considera motivo e dia. [app/chamados/services/disputa_emissao.py:767] [app/ponto/management/commands/detectar_ausencias.py:694] [app/ponto/management/commands/detectar_ausencias.py:740] [app/chamados/services/disputa_emissao.py:1399]
   - **(c) `orfao_14h` resolvida:** a via registra a resolução daquela pergunta; não comprova, sozinha, bloqueio de `intervalo_saida` ou `intervalo_volta`. `MOTIVOS_DO_DIA_SEM_FECHAR` permite associar motivos ao chamado do dia, não transforma motivos diferentes em uma pergunta só. [app/chamados/models.py:3880] [app/chamados/services/disputa_emissao.py:403]

   O teto é por instante do marco, inclusive para dia passado; não se pode inferir que esteja vencido apenas pela data. A janela `--comp-corrente` limita o universo do vigia e não escreve perguntas; `_pendentes_app` filtra perguntas já existentes e vivas para apresentação, não as cria. [app/chamados/services/fabrica_da_celula.py:35] [app/chamados/management/commands/disparar_perguntas_competencia.py:147] [app/chamados/services/tarefa_do_dia.py:530]

   Há uma correção no enunciado: o próprio anexo mostra **col787 em 30/10 com dois marcos faltantes**, `inicio` e `int_volta`, e `hi` apagado; portanto não são 28 casos com exatamente um marco faltante. [não verificado: transcrição fornecida, sem arquivo:linha]

3. **col878: a causa específica da luz não está demonstrada pelo recorte.** A sonda informa quatro papéis acesos e `intervalo.inicio=None`, `fim=None`, mas omite os campos completos `luz`, `tipo_real`, `dispensada` e `isencao` das lâmpadas da **ata**. Assim, não dá para afirmar reaproveitamento de batida nem absolvência para esse caso. [não verificado: transcrição fornecida, sem arquivo:linha]

   O cartório acende normalmente a lâmpada a partir da célula alinhada; há também dispensa por ausência ou justificativa, que pode deixá-la acesa **sem luz**, e crédito por isenção. O alinhamento normal consome índices de batidas; a tolerância de 90 minutos é envelope de atribuição, não prova de uma mesma batida em dois papéis. É preciso a ata completa e os insumos do julgamento para escolher o ramo. [app/ponto/services/cartorio.py:680] [app/ponto/services/cartorio.py:781] [app/escala/utils.py:812] [app/escala/utils.py:585] [app/escala/utils.py:625]

4. **Escritores e leitores.**

   - `dna['lampadas']` contém **marcos esperados**, chave, tipo, hora e instante; o gerador não grava ali `acesa`. O estado `acesa` fica em **`CelulaDia.ata['lampadas']`**, produzido por `lampadas_do_dia` e lavrado pelo cartório; `leitor_celula` e `juiz_batida` o leem. [app/ponto/management/commands/gerar_celulas.py:63] [app/ponto/management/commands/gerar_celulas.py:103] [app/ponto/services/cartorio.py:599] [app/ponto/services/cartorio.py:1851] [app/escala/services/leitor_celula.py:114] [app/ponto/juiz_batida.py:157]
   - `contexto_json['marcos_faltantes']`: `_abrir_chamado` o grava na abertura e `_espelhar_marcos` o atualiza; o agrupador de disputa e a grade estão entre os leitores. [app/ponto/management/commands/detectar_ausencias.py:694] [app/ponto/management/commands/detectar_ausencias.py:544] [app/chamados/services/disputa_emissao.py:1157] [app/escala/utils.py:1066]
   - `PerguntaDisputa.motivo` é gravado no nascimento por `abrir`/`_nascer`; `via_resolucao` é atribuída pela porta de carimbo. **`estado` é propriedade calculada**, não coluna escrita: lê validação, materialização e o juiz de pergunta viva. [app/chamados/models.py:2534] [app/chamados/models.py:2889] [app/chamados/models.py:3131] [app/chamados/models.py:3216] [app/chamados/models.py:3151]

O hash do HEAD não pôde ser confirmado: a cópia disponível não tem metadados Git acessíveis. **[não verificado]**

## 18/11 12:56 · sessao 03b37b05405b · EFEITOS-REAIS-1811 · leitura · origem=Code

1. **Escritor:** `dia_pago.lavrar` grava o `DiaPago`; na versão do motor, soma `PeriodoCalculo.minutos_intrajornada_indenizada / 60`. Há também caminho opcional em que rubricas do calculador substituem o valor: [dia_pago.py:135](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:135), [dia_pago.py:186](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:186), [dia_pago.py:312](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:312), [dia_pago.py:345](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:345). No motor contínuo, jornada superior a seis horas gera indenização pelo mínimo menos a pausa reconhecida, zerada se a supressão for até dez minutos; o mínimo é cadastro ou piso, o maior, salvo perfil específico: [motor_calculo_v2.py:1805](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1805), [motor_calculo_v2.py:2750](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2750), [motor_calculo_v2.py:3239](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:3239). Duas batidas ou marca na célula **não são requisitos para indenizar**; um par reconhecido pode reduzir a pausa suprimida: [turnos.py:542](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:542), [motor_calculo_v2.py:1892](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1892).

2. **Depende do caso:** valida o par inteiro se a metade isolada quebraria a paridade e a irmã respondida passar os testes do par; se não, tenta materializar só a metade solicitada. A irmã permanece respondida quando não é validada por esse ramo; a varredura posterior seleciona apenas perguntas já validadas: [validacao.py:613](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:613), [validacao.py:769](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:769), [materializacao.py:189](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:189), [materializacao.py:2091](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:2091). Existe guarda contra vizinha imediata do mesmo tipo, com exceções e possibilidade de gravar o par: [materializacao.py:95](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:95), [materializacao.py:1373](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:1373). O lote **não exclui `intervalo_volta` por motivo**: as duas views chamam a mesma validação, que aceita as referências classificadas A segundo hora, marco e conflito: [views_cobrar.py:263](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/views_cobrar.py:263), [views_cobrar.py:383](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/views_cobrar.py:383), [validacao.py:1067](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:1067), [validacao.py:1177](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:1177).

3. **Sim, pode tratar como interna**, apesar de `is_intervalo=False`: o pareador pode derivar o papel pela ata ou pela proximidade dos marcos; isso não prova uma pausa completa: [ponto/models.py:26](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:26), [turnos.py:1310](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:1310), [turnos.py:1356](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:1356). Com `E, S-int, S-fim`, o par de pausa reconhecido pode ficar vazio; o **motor** decide a supressão a partir dos intervalos fornecidos pelo pareador, conforme os predicados do item 1: [turnos.py:542](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:542), [motor_calculo_v2.py:2764](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2764).

4. **Não foi encontrado esse caminho sem batida.** O BACKLOG registra `INTERVALO-DECLARADO` como “nada construído”; no caminho de resposta de hora examinado, a materialização exige fato de batida e o motor consome intervalos reconhecidos pelo pareador: [BACKLOG.md:1622](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/BACKLOG.md:1622), [materializacao.py:1920](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:1920), [motor_calculo_v2.py:1892](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1892). Ausência de **qualquer outro caminho** em todo o código: **[não verificado]**.

5. **Exceções — ramo correspondente na cópia, sem afirmar a execução histórica exata:**
   - **col219 13/11:** `paridade_recusada` corresponde à vizinha de mesmo tipo com marco cobrado aceso; a devolução ocorre para resposta de hora sem materialização e com motivo de recusa, limpando a validação e registrando o ato. O motivo raspado das duas devoluções não permite identificar sua causa específica: [materializacao.py:1411](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:1411), [materializacao.py:1437](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:1437), [validacao.py:261](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:261).
   - **col908 08/11:** a volta isolada pode ser materializada como declaração quando passa as guardas; a saída não validada não entra na varredura de materialização: [validacao.py:769](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:769), [materializacao.py:1474](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:1474), [materializacao.py:1968](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:1968).
   - **col171 27/10, col947 06/11 e col1024 04/11:** `celula_absolveu` corresponde ao reconciliador que consulta `pergunta_viva`, exclui dia trancado e carimba pela porta: [reconciliador.py:628](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/reconciliador.py:628), [reconciliador.py:689](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/reconciliador.py:689). A célula e o código **no instante histórico** de cada carimbo: **[não verificado]**.

6. **Escrita e leitura:** `estado` é propriedade calculada, não campo gravado; lê validação, materialização, vitalidade e resposta: [models.py:3151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:3151). `validada_em` é gravada na validação e pode ser limpa na devolução; também pode ser preenchida por `carimbar(..., resposta='validar')`: [validacao.py:161](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:161), [validacao.py:269](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:269), [models.py:3248](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:3248). `via_resolucao` e `materializada_em` são escritos juntos pela porta de carimbo e lidos, por exemplo, pelo estado e pela seleção de materialização: [models.py:3216](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:3216), [models.py:3287](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:3287), [materializacao.py:2091](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:2091). A origem retro é passada ao registrador e `pergunta_origem` é vinculada depois, inclusive em reuso; a procedência lê ambos: [materializacao.py:1474](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:1474), [materializacao.py:1524](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/materializacao.py:1524), [procedencia_batida.py:62](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/procedencia_batida.py:62).

**Ressalva sobre o anexo:** o texto raspado mostra col993 com volta `pendente` e col996 sem pergunta de volta listada; portanto a contagem afirmada de 16 dias com irmã `respondida` não foi confirmada pela transcrição. **[não verificado]** A identidade Git da cópia também não pôde ser conferida: não há metadados Git disponíveis. **[não verificado]**

## 18/11 12:47 · sessao e36752fddc88 · EVAL-NOTURNO-SEM-VEREDITO · verificador · origem=Code

**AVAL-DEVOLVIDO — item 3:** procede devolver as perguntas **1 e 3** para completar o censo e fechar os casos do desenho. Não identifiquei colisão com o aval ou com a lei escrita, nem toque declarado em exportada ou dinheiro, nos limites da cópia e dos anexos. O próprio aval mantém a obra parada até o rearme do B3. [anexo aval.txt, item 3: “A obra segue PARADA ate eu rearmar o B3”]

**1. Premissas do código e censo — PROCEDE devolver parcialmente.**

- As linhas citadas existem e explicam a reprovação: o código escolhe `max(datas)` **antes** de verificar se a data é futura. Assim, um futuro sozinho, ou coexistindo com um relatório anterior válido, produz `medido=False`; a linha publicada diz “sem medida”, sem `ALERTA`. [bin/eval_estado.py:51](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:51) [bin/eval_estado.py:56](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:56) [bin/eval_estado.py:63](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:63)
- Pasta vazia e pasta sem data válida também dão `sem medida`, sem `ALERTA`; pasta ausente cai no tratamento de `OSError` e tem a mesma saída geral. [bin/eval_estado.py:38](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:38) [bin/eval_estado.py:51](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:51) [bin/eval_estado.py:63](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:63)
- `quem-mais-le=bin/gerar_estado.py` identifica o consumidor de publicação: ele importa `ler` e `linha`, e inclui o resultado no ESTADO. O censo textual também encontra o selo, que executa a CLI do medidor, lê seu JSON e suas linhas, e verifica a integração por `grep`; `bin/relato.sh` chama o gerador, indiretamente. Devolver a linha de censo com esses papéis distinguidos. [bin/gerar_estado.py:316](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:316) [bin/gerar_estado.py:402](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:402) [bin/tests/test_eval_estado.sh:19](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:19) [bin/tests/test_eval_estado.sh:30](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:30) [bin/relato.sh:38](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/relato.sh:38)
- A identidade Git da cópia com o hash informado é **[não verificado]**: a cópia disponibilizada não tem metadados Git acessíveis.

**2. Colisão com aval ou lei escrita — NÃO PROCEDE**, quanto ao desenho declarado. O item 4 original pede a idade pelo nome, limite `<= 1`, publicação no ESTADO e caso de três dias; o complemento especifica como tratar futuro e ausência. Não encontrei, no censo textual desses nomes e da fonte, outra implementação que responda à mesma idade: o medidor existente é `eval_estado.ler`, e o gerador consome seu resultado. Isso é conclusão limitada ao censo textual, não prova contra chamadas dinâmicas desconhecidas. [anexo aval_origem.txt, item 4: “esperado <= 1, fonte = data do ultimo eval/runs/relatorio_*.md”] [anexo aval.txt, item 3: “relatorio com data FUTURA nao conta”] [bin/eval_estado.py:32](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:32) [bin/gerar_estado.py:316](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:316)

A implementação deve continuar usando essa autoridade, com contrato de instrumento e pouso próprio previstos nas regras; a dispensa de padrão para instrumento depende de não tocar código de negócio. [CLAUDE.md:394](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/CLAUDE.md:394) [app/docs/LEIS.md:131](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/app/docs/LEIS.md:131) [app/docs/LEIS.md:144](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/app/docs/LEIS.md:144)

**3. Condições que não fecham no literal — PROCEDE devolver os pontos abaixo.**

- **Hoje e futuro sozinho.** Na publicação atual, a borda captura `date.today()` ou recebe data explícita e a passa ao medidor; o escritor usa `time.strftime` no nome. Ambos usam a data local do processo, mas a configuração efetiva de fuso em produção é **[não verificado]**. O literal manda excluir o futuro e dar-lhe linha própria, e manda `ALERTA sem medida` para pasta vazia, ausente ou “sem nome valido”; não diz expressamente se um futuro sozinho, cujo nome e data são válidos mas inelegíveis, também recebe `ALERTA sem medida`. Devolver essa saída combinada para ser fixada. [bin/gerar_estado.py:417](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:417) [eval/noturno.py:113](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/eval/noturno.py:113) [anexo aval.txt, item 3: “data <= hoje” e “pasta vazia, ausente ou sem nome valido = ALERTA ‘sem medida’”]
- **Limite.** “Mais de 1 dia” coincide com o código: `ESPERADO_MAX = 1` e `dias > ESPERADO_MAX`. Dois dias já alertam; o selo atual verifica esse limite. Não há condição a devolver nesse ponto. [bin/eval_estado.py:29](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:29) [bin/eval_estado.py:59](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:59) [bin/tests/test_eval_estado.sh:51](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:51)
- **Nome inválido.** `relatorio_2026-13-45.md` casa com a expressão, falha na construção da data e entra em `invalidos`; `relatorio_x.md` não casa e é ignorado. Ambos isoladamente deixam a pasta sem data válida, mas não percorrem o mesmo ramo. Devolver a definição de quais nomes inválidos devem ser reconhecidos e testados. [bin/eval_estado.py:28](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:28) [bin/eval_estado.py:43](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:43)
- **Cobertura.** Os quatro exemplos nomeados não especificam, por si, futuro coexistindo com relatório válido, nem pasta ausente ou as duas classes de nome acima. “Mutante por ramo” exige mordida nos ramos que forem implementados, mas não descreve esses arranjos de entrada. Devolver a inclusão explícita do caso misto: idade e eventual alerta do válido **mais** linha do futuro; acrescentar casos para ausência e para as classes de nome que o desenho vier a exigir. [anexo aval.txt, item 3: “Selo com caso que MORDE em cada um dos quatro (...) e mutante por ramo”] [bin/eval_estado.py:43](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:43)
- **RED pelo caso.** É possível nesta cópia: o medidor existe e o selo já o chama com data e raiz injetadas. Um teste das saídas novas pode falhar pelo comportamento atual, sem depender da ausência do arquivo. O selo atual, porém, só exige `sem medida` nos casos sem relatório e só muta o limite; não prova os novos alertas nem o ramo futuro. [bin/tests/test_eval_estado.sh:19](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:19) [bin/tests/test_eval_estado.sh:66](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:66) [bin/tests/test_eval_estado.sh:79](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:79)

**4. Exportada, dinheiro ou `!` fora do declarado — NÃO PROCEDE**, para o desenho e os arquivos do contador examinados: o medidor lê nomes em `eval/runs`, calcula a idade e compõe texto; o gerador o publica no ESTADO. Não há, nesse fluxo examinado, escrita de competência ou operação de dinheiro. Uma implementação futura fora desses arquivos ou seus efeitos indiretos são **[não verificado]**. A liberação do B3 continua sendo de Ronald, conforme o anexo. [bin/eval_estado.py:26](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:26) [bin/eval_estado.py:32](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:32) [bin/gerar_estado.py:316](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:316) [CLAUDE.md:265](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/CLAUDE.md:265) [anexo aval.txt, item 3: “A obra segue PARADA ate eu rearmar o B3”]

## 18/11 12:34 · sessao a709fac15f77 · COMBOIO-DAS-SUITES-1811 · verificador · origem=Code

**Resultado:** as premissas medidas sustentam a devolução dos itens **1 e 2**. A colisão com os avais vigentes e o alcance sobre exportada, dinheiro e `!` não podem ser fechados integralmente com os diffs disponíveis. O anexo prevê devolução por item e continuidade do outro item quando apenas um proceder: “PROCEDE = AVAL-DEVOLVIDO por item; o outro item segue” (*aval.txt*, revisão).

A cópia do HEAD fornecida não contém metadados Git; por isso, **o diff de cada raia contra main está [não verificado]**. As listas de arquivos abaixo são as que constam literalmente de *medidas.md*, não uma substituição por um diff Git.

### 1. Premissas — **PROCEDE: devolver itens 1 e 2**

- **Suite da P2:** o aval diz “a suite da P2 que esta rodando”; a medida diz “NAO esta rodando: terminou FAILED 2” e que sua base não continha `d54160999`. A afirmação do aval não confere (*aval.txt*, item 1; *medidas.md*, parágrafo da suite da P2).
- **F4 e F6F7 sobre P2:** `main+P2+F4` foi medido limpo, mas acrescentar F6F7 deu conflito em `app/chamados/services/tarefa_do_dia.py`; F6F7 também conflita sobre `main+P2`, identificando o conflito com a P2 (*medidas.md*, parágrafo “merge-tree”). O próprio aval prevê excluir a raia em conflito (*aval.txt*, item 1). Assim, a árvore com as três raias não está disponível sem resolver o conflito.
- **Cirineu c4c:** a identificação confere: a raia medida é `raia/cirineu-pouso-115210`, `4ec3e15d0 = main + 843b57e43 c4c` (*medidas.md*, primeira lista).
- **ESPURIA c9:** a condição de entrada após RED e GREEN está declarada no aval; a medida registra RED existente e GREEN não rodado. Portanto, a condição **ainda não está satisfeita** (*aval.txt*, item 2; *medidas.md*, parágrafo “ESPURIA c9”).
- **Deslocamento de `escala/utils.py`:** o arquivo consta da própria Cirineu, e a medida diz que nenhuma de P2, F4, F6F7 ou ESCALA toca esse arquivo ou a catraca. A atribuição do deslocamento à soma das árvores não está demonstrada; **[não verificado]** que a catraca seria o único vermelho ou qual seria sua causa (*aval.txt*, item 2; *medidas.md*, parágrafo “Comboio B”).

### 2. Colisão com aval vigente — **NÃO PROCEDE, com alcance parcial [não verificado]**

O texto do comboio preserva a ordem P2 → F4 → F6F7 e a janela de véspera (*aval.txt*, itens 1 e 3; *ordem_viva_1811_manha_0855.txt*, itens 0 e 3). A decisão de Cirineu mantém a espinha à frente e determina pouso antes das 18h se fechar verde, ou permanência em raia até o fim da simulação; o comboio B conserva o prazo, mas sua posição efetiva de pouso deve respeitar também essa ordem (*cirineu_12h_decidida_1811_1110.txt*; *aval.txt*, item 2). **A execução efetiva dessa ordem está [não verificado].** [nao verificado]

Quanto aos arquivos contra `main`, o pedido de listar o diff completo de cada raia **não pôde ser cumprido**:

| Raia | Arquivos fornecidos no dado raspado |
|---|---|
| P2 | Diff completo **[não verificado]**. |
| F4 | Diff completo **[não verificado]**. |
| F6F7 | `app/chamados/services/tarefa_do_dia.py` aparece como local do conflito; diff completo **[não verificado]**. |
| Cirineu | `core/tests/test_medida_turno_do_par.py`, `escala/tests/test_cirineu_nao_desfaz_o_papel.py`, `escala/utils.py`. |
| ESCALA | `escala/regua_defesa.py`, `escala/services/cadastro_realidade.py`, `escala/urls.py`, `escala/views.py`, `ponto/services/esmeril_espelho.py`, `templates/escala/cadastro_x_realidade.html` e “teste novo”, sem caminho informado. |
| ESPURIA c9 | Diff completo **[não verificado]**. |

Fonte das listas: *medidas.md*, parágrafos “merge-tree” e “Comboio B”. Portanto, **quais raias tocam folha, export, retenção ou DiaPago está [não verificado]**; não há base para declarar uma colisão concreta com a janela nem para descartá-la em todas as raias. A janela é expressa em *ordem_viva_1811_manha_0855.txt*, item 0.

### 3. Condições literais — **PROCEDE: devolver itens 1 e 2**

**a. Árvore do B.** A medida do B foi feita sobre `main+P2+F4`, chegando a `8a60c3c31` com Cirineu e ESCALA; c9 ainda não tinha GREEN (*medidas.md*, parágrafo “Comboio B” e “ESPURIA c9”). Se F6F7 pousar antes de B, a árvore resultante incluirá F6F7 e **não será automaticamente a árvore medida para B**. Se só P2+F4 pousarem antes, a base medida corresponde à espinha descrita, mas a igualdade da árvore final, inclusive com as raias efetivamente incluídas, exige comparação: **[não verificado]**. A regra exige que a árvore certificada seja a que pousa e `cmp` antes do commit ([CLAUDE.md:320](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:320)). “Mesma medida de conflito” não certifica essa igualdade (*aval.txt*, item 2).

**b. F6F7 fora do comboio A.** Para pousar depois de P2 e F4, sua suite precisa certificar a árvore que resultará desse pouso sobre a ponta então vigente, com o conflito resolvido; uma suite da raia isolada, sem P2, não demonstra isso ([CLAUDE.md:320](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:320)). A suite isolada medida usava base sem `d54160999` e sem P2 e terminou FAILED (*medidas.md*, parágrafo “Suite do F6F7 sozinho”). A resolução e a árvore resultante estão **[não verificado]**.

**c. Cancelamento do job.** O job começou às 12:30:09 e foi cancelado às 12:31:56, antes do aval das 12:35 (*medidas.md*, parágrafo do job; *aval.txt*, cabeçalho). O aval posterior proíbe interromper suite em curso, mas o dado não sustenta aplicar essa proibição retroativamente (*aval.txt*, “PROIBIDO”). A pré-aprovação permite cancelar job em fila ou rodando **de volta superada da mesma obra**, com linha no RELATO ([CLAUDE.md:288](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:288)). Que fosse volta superada da mesma obra e que a linha exigida exista está **[não verificado]**; não se pode concluir que o cancelamento estava coberto.

**d. Opus por obra.** O aval exige “cada um com o seu opus final sobre o proprio diff” (*aval.txt*, item 1), e a regra exige suite inteira verde antes do opus final por obra ([CLAUDE.md:372](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:372)). O literal consultado **não fixa** se o comando de diff é contra `main` original ou contra a ponta anterior: **[não verificado]**. Para identificar o que cada obra acrescenta, o diff contra a ponta anterior isola sua contribuição; a revisão também precisa considerar os sítios e leitores/escritores exigidos pela regra, inclusive fora do diff ([CLAUDE.md:379](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:379)). Isso é uma interpretação operacional, não uma base de comparação prescrita literalmente.

### 4. Exportada, dinheiro ou `!` fora do declarado — **[não verificado]; sem base para PROCEDE ou NÃO PROCEDE**

A regra exige `!` para, entre outros casos, tocar o gravado de competência exportada ou dado de escala/vínculo ([CLAUDE.md:265](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:265)). Há autorização expressa para o botão de vínculo pela porta existente com trilha (*ordem_viva_1811_manha_0855.txt*, última linha), e Cirineu não deve regerar célula da competência 11 antes do corte posterior à simulação (*cirineu_12h_decidida_1811_1110.txt*). Sem os diffs completos e sem dado de produção além do raspado, **o alcance real sobre exportada, dinheiro ou outro `!` permanece [não verificado]**.

**Fecho:** 1 **PROCEDE — itens 1 e 2 devolvidos**; 2 **NÃO PROCEDE no texto examinado, com alcance dos diffs [não verificado]**; 3 **PROCEDE — itens 1 e 2 devolvidos**, pelas condições de árvore que precisam fechar; 4 **[não verificado] — não classificável**.

## 18/11 12:16 · sessao d2c13875bd5b · DSTL1-PORTA-HE · sonda · origem=Code

No dado fornecido, não aparece erro nem traceback. `CONCLUSAO_LIBERADA` está presente e `rc_sombra=0`. [não verificado]

Cópia das três linhas solicitadas: [não verificado]

```text
FUNIL_HOJE_CAP0611 {'B_trabalha': 277, 'B_sem_trabalho': 139, 'C_pago>0': 78, 'C_ESTRUTURA_DINHEIRO': 54, 'C_ESTR_2mais': 36, 'C_decisao_he': 4, 'C_porta': 3}
HOJE_ESTR_2mais_MARCADORES {'regen_pos': 4, 'regeneracoes': 12, 'dna_anterior': 12, 'julgada_pos': 9, 'diapago_lavrado_pos': 33, 'sem_marcador': 3, 'colab_desligado': 4} de 36
HOJE_ESTR_POR_DIA {'2026-11-02': 11, '2026-11-05': 1, '2026-10-30': 1, '2026-11-03': 1, '2026-10-22': 4, '2026-10-29': 1, '2026-11-01': 2, '2026-10-21': 2, '2026-10-23': 1, '2026-10-31': 1, '2026-10-24': 2, '2026-10-25': 3, '2026-10-28': 1, '2026-10-27': 1, '2026-11-06': 1, '2026-11-04': 1, '2026-10-26': 2}
```

Contagem das 19 linhas `AMOSTRA` por `razao_hoje`: [não verificado]

| `razao_hoje` | Contagem |
|---|---:|
| `pago_sem_decisao_ou_pago` | 11 |
| `ESTRUTURA_DINHEIRO` | 8 |

Dicionário `marc` de cada linha, agrupado por `razao_hoje`: [não verificado]

| `razao_hoje` | Amostra | `marc` |
|---|---|---|
| `pago_sem_decisao_ou_pago` | `col43` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col207` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col369` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col384` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col789` | `{'regeneracoes': 5, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col822` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col962` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col967` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col991` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col1003` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col749` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col97` | `{'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col242` | `{'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col278` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col617` | `{'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col820` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col864` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col944` | `{'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col900` | `{'regen_pos': 1, 'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |

## 18/11 12:15 · sessao 365c53fd8db0 · DSTL1-PORTA-HE · sonda · origem=Code

A saída fornecida não contém erro nem traceback e chegou a `CONCLUSAO_LIBERADA`, seguida de `rc_sombra=0`. [não verificado]

As tabelas abaixo transcrevem ou contam o dado fornecido no enunciado, que não tem arquivo e linha associados. [não verificado]

**I4**

| Contagem | Valor |
|---|---:|
| Linhas com `causaC=True` | 65 |
| Total de linhas | 65 |

| `classe_e6` entre as `causaC` | Quantidade |
|---|---:|
| `bate_ate_10min` | 52 |
| `DIVERGE` | 7 |
| `sem_trabalho_ambos` | 6 |

**DONOS_DOS_DIVERGENTES**

| Dono | Quantidade |
|---|---:|
| `ESTRUTURA` | 2 |
| `BATIDA` | 5 |

**I2 e I2_RELATO**

| Colaborador | Dia | `janela_he_ativa` | `desde` | `piso` | `saida_ativa` | `dia_ge_desde` | `he_fora_espelho` | `motor_cxr_do_dia` | `motor_sem_previsto_fora` | `I2_RELATO` |
|---|---|---|---|---:|---|---|---|---|---|---|
| col741 | 2026-11-15 | True | 2026-08-21 | 0 | True | True | `[]` | `[]` | `[]` | `[] -> []` |
| col899 | 2026-11-01 | True | 2026-08-21 | 0 | True | True | `[]` | `[]` | `[]` | `[] -> []` |
| col944 | 2026-11-02 | True | 2026-08-21 | 0 | True | True | `[]` | `[]` | `[]` | `[]` | `[] -> []` |

**I4SOMA**

| Colaborador | `soma_min_real` | `fech_min_real` |
|---|---:|---:|
| col55 | 8352.0 | 8352 |
| col61 | 5758.0 | 5758 |
| col114 | 9221.0 | 9221 |
| col123 | 9356.0 | 9356 |
| col130 | 8915.0 | 8915 |
| col137 | 5572.0 | 5572 |
| col198 | 8639.0 | 8639 |
| col203 | 8819.0 | 8819 |
| col206 | 8485.0 | 8485 |
| col227 | 9113.0 | 9113 |
| col237 | 9360.0 | 9360 |
| col242 | 6150.0 | 6150 |
| col250 | 5931.0 | 5931 |
| col270 | 2083.0 | 2083 |
| col278 | 8731.0 | 8731 |
| col281 | 8639.0 | 8639 |
| col282 | 8528.0 | 8528 |
| col328 | 6871.0 | 6871 |
| col331 | 6894.0 | 6894 |
| col334 | 7325.0 | 7325 |
| col364 | 8630.0 | 8630 |
| col373 | 1880.0 | 1880 |
| col398 | 9097.0 | 9097 |
| col399 | 9299.0 | 9299 |
| col424 | 9356.0 | 9356 |
| col444 | 8061.0 | 8061 |
| col451 | 5040.0 | 5040 |
| col476 | 9455.0 | 9455 |
| col489 | 8698.0 | 8698 |
| col515 | 9229.0 | 9229 |
| col617 | 6300.0 | 6300 |
| col627 | 8313.0 | 8313 |
| col634 | 8114.0 | 8114 |
| col696 | 0.0 | 0 |
| col741 | 711.0 | 711 |
| col835 | 9217.0 | 9217 |
| col843 | 9234.0 | 9234 |
| col845 | 8580.0 | 8580 |
| col849 | 9232.0 | 9232 |
| col864 | 10056.0 | 10056 |
| col899 | 7081.0 | 7081 |
| col913 | 8889.0 | 8889 |
| col932 | 7763.0 | 7763 |
| col935 | 8580.0 | 8580 |
| col936 | 7597.0 | 7597 |
| col944 | 7730.0 | 7730 |
| col950 | 4427.0 | 4427 |
| col985 | 9452.0 | 9452 |
| col996 | 9271.0 | 9271 |
| col998 | 8074.0 | 8074 |
| col999 | 7692.0 | 7692 |
| col1001 | 6748.0 | 6748 |
| col1012 | 4710.0 | 4710 |
| col1045 | 0.0 | 0 |

Linhas `I4SOMA` com valores numericamente diferentes: **0 de 54**. [não verificado]

## 18/11 12:15 · sessao 24f25bb8b191 · DSTL1-PORTA-HE · leitura · origem=Code

**Resultado principal:** o código distingue o `DiaPago.minutos_realizados` dos minutos da ata e dos minutos calculados pelo pareador. `pago_do_dia` e `rubricas_do_dia` **não leem** esse campo do `DiaPago`; portanto, não é possível inferir o pagamento ou uma pendência de HE apenas a partir de ele ser zero. ([dia_pago.py:859](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:859), [espelho.py:775](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:775))

A lista abaixo registra os leitores e caminhos identificados na inspeção. **A exaustividade de todos os leitores indiretos ou dinâmicos de produção permanece [não verificado].** Não houve consulta a banco nem a logs de produção.

### A. Leitores e origem dos valores

**Produção do campo.** `DiaPago.minutos_realizados` é `FloatField(default=0.0)`, sem `null=True` na definição. `por_dia_da_grade` forma o mapa apenas com dias `trabalho` e `ausencia`, usando `min(realizado ou 0, previsto ou 0)`; assim, `folga` e `feriado` não entram nesse mapa. `fechamento` o passa à lavratura. Isso descreve a construção do mapa, não prova o conteúdo de uma linha concreta. ([models.py:351](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:351), [dia_pago.py:386](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:386), [fechamento.py:1060](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1060), [fechamento.py:1200](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1200))

**Leituras efetivas de `DiaPago.minutos_realizados` identificadas:**

| Leitor | Uso de 0 ou nulo em folga/feriado |
|---|---|
| `soma_da_competencia` | Faz `Sum` dinâmico de todos os `CAMPOS`, inclusive minutos realizados e linha de ajuste. Zero contribui zero; agregado nulo vira zero. Sem nenhuma linha, retorna `None`. Não filtra tipo de dia. ([dia_pago.py:34](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:34), [dia_pago.py:550](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:550)) |
| `soma_do_periodo` | Faz `values`/`annotate(Sum(...))` de `CAMPOS_DIA`, agrupando por colaborador. Zero contribui zero e agregado nulo vira zero; a função também devolve conjuntos para distinguir ausência de lavratura e de apuração. Não filtra tipo de dia. ([dia_pago.py:752](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:752), [dia_pago.py:784](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:784)) |
| `folha.porta_export.medir`, função interna `_medir_dia_pago` | Compara cada campo da soma da competência com o homônimo do `FechamentoMensal`; pode registrar divergência e classificá-la conforme exportação e comparação com a grade. Compara o **total**, não decide isoladamente o estado de uma folga. ([porta_export.py:378](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:378), [porta_export.py:451](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:451)) |
| `foto_do_colaborador` / `mudancas` | Fotografa todos os campos concretos das linhas `DiaPago` e compara os dicionários completos. Uma mudança no campo pode aparecer como linha alterada, sem regra especial para folga ou feriado. ([foto_e4b.py:209](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:209), [foto_e4b.py:247](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:247), [foto_e4b.py:289](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:289), [foto_e4b.py:116](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:116)) |

**Quem recebe `DiaPago`, mas não usa esse campo para o valor exibido:**

- `linhas_do_periodo` devolve objetos `DiaPago` por data. `pago_do_dia` testa rubricas de dinheiro e devolve `None` se a linha falta ou se todas essas rubricas são zero; caso contrário, soma horas trabalhadas, folga trabalhada e a parcela de folga-feriado no detalhe. `rubricas_do_dia` lê outras rubricas e, sem linha, devolve zeros com `lavrada=False`. Nenhuma das duas funções consulta `DiaPago.minutos_realizados`. ([dia_pago.py:833](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:833), [dia_pago.py:859](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:859), [dia_pago.py:890](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:890), [dia_pago.py:901](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:901))
- No **espelho**, `montar_dias` entrega `pago_h` e `rubricas` por essas funções. O template omite rubricas zeradas e, sem lavratura, mostra “sem apuração ainda”. O **PDF** lê `pago_h` e rubricas; rubricas zero ficam vazias. O drill do relatório transforma `pago_h=None` em zero interno e exibe vazio se zero. Esses comportamentos dependem das rubricas, não do minuto gravado em `DiaPago`. ([espelho.py:805](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:805), [espelho.html:268](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/templates/ponto/espelho.html:268), [pdf_espelho.py:901](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:901), [services.py:142](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/services.py:142))
- No **app**, `horas_do_dia` converte `pago_do_dia=None` em `0.0` quando existe linha; sem linha, usa o período do motor e marca a fonte como não lavrada. A testemunha `espelho_x_dia_pago` da folha também usa `pago_do_dia`, convertendo seu `None` em zero para comparar com os períodos do motor. ([espelho.py:120](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:120), [api/views.py:1531](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/api/views.py:1531), [porta_export.py:101](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:101))
- O relatório e o cartão chamam as agregações que incluem o minuto, mas os usos identificados para os valores apresentados selecionam outras chaves de rubricas. O efeito de **todos** os demais consumidores indiretos dessas agregações sobre o minuto está [não verificado]. ([relatorios/views.py:481](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/views.py:481), [cartao_pela_celula.py:141](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/cartao_pela_celula.py:141))

**Homônimos que não são o campo de `DiaPago`:**

- `dia['minutos_realizados']` do espelho vem de `_real_dia.minutos`, calculado por `realizado_do_dia`; pode ser `None`. O E6 usa esse valor primeiro e só recorre a `pago_h` quando ele é `None`. ([espelho.py:675](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:675), [espelho.py:783](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:783), [e6_oraculo.py:263](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:263))
- `CelulaDia.ata['minutos_realizados']`: `referencia_do_espelho` no PDF de conferência converte zero ou `None` em zero e só incrementa dias e minutos quando o valor é verdadeiro. ([validacao_pdf.py:29](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/validacao_pdf.py:29), [validacao_pdf.py:44](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/validacao_pdf.py:44))
- O `dia` da **grade** em `status_dia.fatos_do_status` converte zero ou `None` em zero para `tem_batida`; ainda pode haver `tem_batida=True` por células ou órfãs. `supra_juiz.classificar_dia` também lê o valor da grade com `or 0`. `folha.causas.classificar_causa` soma o realizado dos dias úteis e pode emitir `realizado_zero` conforme as outras guardas. ([status_dia.py:166](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/status_dia.py:166), [status_dia.py:185](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/status_dia.py:185), [supra_juiz.py:217](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/supra_juiz.py:217), [causas.py:28](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/causas.py:28), [causas.py:84](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/causas.py:84))
- `folha.export` lê também **`FechamentoMensal.minutos_realizados`**: zero é usado como zero; `None` faz fallback para `trab`. No fallback pela grade, o cálculo do realizado seleciona dias de trabalho. Isso não é leitura do campo diário `DiaPago`. ([export.py:1076](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:1076), [export.py:1095](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:1095))
- `core/juizes.py` registra a autoridade `realizado_do_dia`; esse registro não lê o valor de um `DiaPago`. Leitura do campo por `veredito_celula` ou pelo motor: **[não verificado]**. ([juizes.py:203](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/core/juizes.py:203))

### B. Juiz e rótulo `ESTRUTURA_DINHEIRO`

O predicado exato que **produz `ESTRUTURA_DINHEIRO` está [não verificado]**: a busca pelo literal em `app/` encontrou referências documentais, mas não um produtor executável identificado. A descrição documental não substitui o predicado do código. ([RELATO.md:2674](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:2674))

O que está verificado é o juiz E6, que retorna `CADASTRO`, `BATIDA` ou `ESTRUTURA`. Para retornar **`ESTRUTURA`**, todas estas condições devem valer: os três indicadores cadastrais são falsos; `batida_fora_de_turno` é falso; a quantidade de `horas_do_dia` é par, inclusive zero; `fase_deslocada` é falso; e `em_cadastro_x_realidade` é falso. A função, por si, não exige que haja divergência numérica. ([e6_oraculo.py:53](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:53), [e6_oraculo.py:98](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:98))

Os insumos montados por `InsumosDoDono` são:

- **Horas/paridade:** timestamps de `batidas_apuraveis` na janela ampliada, processados por `minutos_do_oraculo`. ([e6_oraculo.py:149](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:149))
- **Cadastro:** callback cadastral fornecido ou `cadastro_do_dia_carregado`, que fornece os indicadores passados ao juiz. ([e6_oraculo.py:182](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:182), [supra_juiz.py:173](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/supra_juiz.py:173))
- **Batida fora de turno:** `realizado_do_dia(...).aberto`, mais a existência de entrada solta que permaneça apurável; os turnos vêm de `turnos_do_colab`. ([e6_oraculo.py:156](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:156), [e6_oraculo.py:198](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:198))
- **Fase:** dias informados pelo chamador ou lidos por `dias_de_fase_as_of`. **Cadastro x realidade:** conjunto fornecido ou `resultado.dias_cadastro_x_realidade` do espelho/motor. Com `sem_motor=True` e sem esse conjunto, uma resposta que dependeria dele pode ser `None`. ([e6_oraculo.py:128](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:162](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:162), [e6_oraculo.py:174](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:174), [e6_oraculo.py:189](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:189))

**Nenhum desses insumos é `DiaPago.minutos_realizados`.** Na comparação que antecede a chamada ao dono, o E6 lê primeiro o minuto do **espelho/pareador**; se for `None`, usa `pago_h`, que vem das rubricas descritas acima. Alterar somente o minuto de `DiaPago` não altera diretamente essa entrada nem o juiz. Se o minuto **do espelho** for zero, ele é comparado como zero; se for `None`, há fallback para `pago_h`, e, sem ambos, o dia sai da comparação. ([e6_oraculo.py:242](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:242), [e6_oraculo.py:332](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:332), [espelho.py:783](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:783), [dia_pago.py:890](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:890))

### C. Condições para não aparecer pendência de HE

Ter duas ou mais batidas apuráveis **não basta** para publicar uma pendência: a lista do dia é a união de três vias, e só fica vazia quando nenhuma delas produz item. As condições abaixo são gerais; não atribuem causa a nenhum dia concreto. ([espelho.py:720](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:720))

1. **Cadastro e vigência.** A lista começa vazia e as três vias só são executadas se `janela_he_ativa` for verdadeira, `janela_he_desde` existir e a data for igual ou posterior a ela. Saída desligada impede as pontas de saída, mas não impede por si uma ponta de entrada ou o item de dia inteiro. ([espelho.py:384](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:384), [espelho.py:720](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:720), [janela_he.py:194](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:194))

2. **Vínculo e grade.** Sem vínculo **do dia**, as extras declaradas permanecem zero; o código continua e ainda tenta produzir pendência. Se não houver montador de realizado, `_real_grade` fica vazia, eliminando as duas vias que precisam de células-alvo, mas isso não elimina necessariamente a via de dia inteiro. ([espelho.py:688](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:688), [espelho.py:720](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:720), [janela_he.py:272](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:272))

3. **Ponta diretamente atribuída.** `marcar_pontas_fora` considera a primeira célula de marco tipo `E` e a última tipo `S`; ignora célula `missing`, demais marcos, saída com a opção desligada e desvio no sentido errado ou zero. Da sobra no sentido correto desconta a extra declarada; se a sobra for zero ou não exceder o piso, não produz aquela ponta. Igualdade com o piso fica dentro da janela. ([janela_he.py:51](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:51), [janela_he.py:73](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:73), [janela_he.py:194](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:194))

4. **Recuperação de ponta pelo relato.** `missing` sozinho **não elimina a pendência**: `pontas_do_relato` pode recuperá-la. Essa via não acrescenta item se faltar relato do mesmo dia com causa de janela e sentido reconhecido, se a saída estiver desligada, se o sentido já tiver sido marcado, se não existir célula-alvo `missing`, se o marco não coincidir ou se os minutos arredondados não forem positivos. A ausência nessa via tampouco elimina as outras duas. ([janela_he.py:267](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:267), [janela_he.py:293](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:293), [janela_he.py:311](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:311))

5. **Dia inteiro sem previsto.** A terceira via traduz itens do motor com a mesma data e minutos verdadeiros. O motor deixa de produzir esse item, entre outras guardas, se faltar entrada ou minutos trabalhados, se não for folga nem feriado, se a empresa não estiver na janela, se o previsto não for exatamente zero, se houver autorização, ou, para folga com colaborador conhecido, se `escala_certa_no_dia` for falsa. Essas guardas são específicas dessa via. ([espelho.py:743](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:743), [motor_calculo_v2.py:1440](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1440), [motor_calculo_v2.py:1465](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1465))

6. **Apuração e competência.** `apurar` percorre apenas colaboradores da empresa com `situacao='ativo'`; erro ao obter o espelho pula o colaborador e incrementa erros. Para os dias recebidos, rejeita data ilegível ou fora da janela da competência e pula lista `he_fora_da_janela` vazia. Uma decisão existente muda o estado e a contagem de “sem decisão”, mas não remove automaticamente o dia que já tem pontas da fotografia. ([he_pendente_lavrado.py:89](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:89), [he_pendente_lavrado.py:114](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:114), [he_pendente_lavrado.py:132](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:132))

7. **Publicação e visualização.** A Gestão lê uma fotografia que pode não existir; a leitura devolve também quando foi calculada. O cron mantém a competência atual e a anterior enquanto esta não estiver entregue. Na Gestão, busca, praça, posto, estado e dono podem ocultar uma linha ou dia já fotografado; isso é filtro de exibição, não prova de que a pendência não foi lavrada. ([he_pendente_lavrado.py:193](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:193), [he_pendente_lavrado.py:209](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:209), [gestao_he.py:236](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:236), [gestao_he.py:317](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:317))

## 18/11 12:13 · sessao 807b8f662b4b · - · - · origem=Ronald (tmux)

A busca em `app/` encontrou o literal `ESTRUTURA_DINHEIRO` em documentação, mas não em um produtor executável. O predicado exato do rótulo composto está **[não verificado]**. A documentação o descreve como dias sem pago, decisão de HE, porta ou verbo de administrador, mas isso não substitui o predicado da sonda que os classificou ([app/docs/RELATO.md:2674](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:2674)).

O juiz E6 responde outra pergunta e retorna apenas `CADASTRO`, `BATIDA` ou `ESTRUTURA`. Ele retorna `ESTRUTURA` quando os três indicadores de cadastro são falsos, `batida_fora_de_turno` é falso, a quantidade de `horas_do_dia` é par, `fase_deslocada` é falso e o dia não está em cadastro versus realidade; zero batidas também é par ([e6_oraculo.py:98](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:98)). Com `sem_motor=True`, sem conjunto CXR fornecido, o resultado que dependeria dessa lista fica `None` ([e6_oraculo.py:189](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:189)).

Os insumos de `InsumosDoDono` são:

- `horas`: timestamps das batidas apuráveis na janela ampliada, agrupados pelo oráculo; batidas retratadas são excluídas ([e6_oraculo.py:149](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:149), [nucleo.py:230](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/calculador/nucleo.py:230), [turnos.py:2868](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:2868)).
- `batida_fora_de_turno`: o estado `aberto` de `realizado_do_dia` e a existência de entrada solta apurável, usando turnos carregados para a janela ampliada ([e6_oraculo.py:156](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:156), [e6_oraculo.py:198](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:198)).
- Indicadores cadastrais: vínculo do dia e defeito de folga; na ausência de vínculo, célula e data de demissão alimentam o indicador específico de dia de trabalho ([supra_juiz.py:179](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/supra_juiz.py:179), [supra_juiz.py:206](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/supra_juiz.py:206)).
- `fase_deslocada`: dias da lavra mais recente que cobre cada data, em `itens` e `itens_reabertos` ([e6_oraculo.py:174](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:174), [fase_12x36.py:117](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/services/fase_12x36.py:117)).
- Cadastro versus realidade: conjunto fornecido ao carregador ou extraído de `resultado.dias_cadastro_x_realidade` do motor no espelho ([e6_oraculo.py:142](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:142), [e6_oraculo.py:162](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:162), [motor_calculo_v2.py:2719](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2719)).

Nenhum desses insumos lê diretamente `DiaPago.minutos_realizados`. A comparação feita pelo comando E6 antes de perguntar o dono lê o homônimo do espelho, proveniente de `_real_dia.minutos` do pareador; se ele for `None`, usa `pago_h`, quando disponível ([espelho.py:777](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:777), [e6_oraculo.py:263](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:263)). `pago_do_dia` usa rubricas monetárias e detalhe, não esse campo de minutos ([dia_pago.py:859](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:859), [dia_pago.py:890](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:890)). Assim, alterar apenas `DiaPago.minutos_realizados` para `0` ou `None` não muda diretamente esses insumos nem esse fallback; o efeito sobre o rótulo composto da sonda permanece **[não verificado]**.

## 18/11 12:13 · sessao dc1dc864d1a9 · - · - · origem=Ronald (tmux)

Investigação da pergunta A enviada ao agente principal, com as referências de código e a distinção entre `DiaPago.minutos_realizados`, o valor da ata e o valor do espelho.

## 18/11 12:13 · sessao 0720f43be677 · - · - · origem=Ronald (tmux)

Investigação da pergunta C concluída e enviada ao agente principal, com as condições de cada uma das três vias de publicação e os filtros posteriores da Gestão de HE.

_6958 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
