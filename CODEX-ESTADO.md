# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 11:06** · publicado em **26/11/2026 11:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 11:06 · sessao f9d3e5e1594c · - · - · origem=Ronald (tmux)

1. **Bloqueia | A sobreposição de segmentos ainda desloca a órfã da véspera.** Reproduzi em memória com o pareador e `vinculo_do_dia` reais. Vínculo noturno 19h–07h até 23/10, diurno 08h–17h desde 24/10, corte 24, sem DNA nem ata, e uma única S em 24/10 às 07h. O segmento noturno atribui a S a 23/10. O diurno atribui a mesma PK a 24/10, sobrescreve o mapa e o filtro F3 descarta o turno de 23/10. Na criação, o cartório visita 23 e 24 pela regra de vizinhança, o núcleo recebe só o turno de 24 e o evento agenda 11/2026. Produção [nao verificado]. `app/ponto/turnos.py:285–298`, `app/ponto/turnos.py:1253–1302`, `app/ponto/turnos.py:1977–2002`, `app/ponto/turnos.py:3001–3009` na base e hunk F3 `+2950`, `app/ponto/signals.py:124`, `app/ponto/services/fechamento.py:1831`, `app/ponto/janelas.py:10–33`.

2. **Bloqueia | C6 conserva outro caminho para o mesmo veredito.** O lote chama o corpo puro com batidas do próprio recorte. O avulso chama `turnos_do_colab`. Reproduzi com 19h–07h, E em 25/10 às 05h e S em 26/10 às 06h, corte 25. O histórico fecha o par em 25/10, competência 11. O recorte C6 `[24/10,24/10]` enxerga só a entrada e devolve aberto em 24/10, competência 10. A diferença chega a `_turno_tem_saida` e `classificar_falta`. `app/chamados/services/carga_pergunta_viva.py:165–184`, `app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/juizes.py:447–449`, `app/escala/servico_jornada.py:249–259`, `app/escala/servico_jornada.py:403–415`, `app/ponto/turnos.py:285–309`, `app/ponto/turnos.py:1492–1498`.

3. **Bloqueia | A lavra ainda recebe o dia derivado no motor.** O motor usa a carga de batidas limitada a ±1 dia da competência, deduplica e monta mapa por instante de entrada e tipo gravado. Na falta da chave usa o civil. Para o par de 24h10 com E em 23/10 e S em 25/10, corte 24, o evento pode agendar outubro enquanto o cálculo de outubro sequer carrega a S. `fechamento.py:1241` e `:1311` repassam a resposta do motor à lavra. Resultado final do gravado [nao verificado]. `app/ponto/services/fechamento.py:641–653`, `app/ponto/motor_calculo_v2.py:867–894`, `app/ponto/motor_calculo_v2.py:2961`, `app/ponto/motor_calculo_v2.py:3346`, `app/ponto/services/fechamento.py:1241`, `app/ponto/services/fechamento.py:1311`, `app/ponto/services/dia_pago.py:126–132`, `app/ponto/services/dia_pago.py:173–177`.

4. **Nota | Escritores diretos e censo.** O censo AST da base localizou escrita de `Batida` apenas em `app/ponto/registro_batida.py:170,176,274,311,355`. O núcleo cria `TurnoMaterializado` e apaga o recorte via SQL na porta, com exclusão indireta herdada pelas FKs `CASCADE`. A varredura conferiu 50 chamadas diretas ou por alias, mais `_T1`. Calendário, status e painel também são consumidores via lote. Variação efetiva dos consumidores em produção [nao verificado]. `app/ponto/nucleo.py:48`, `app/ponto/nucleo.py:59`, `app/ponto/nucleo.py:80`, `app/ponto/models.py:1214`, `app/ponto/models.py:1217`, `app/ponto/models.py:1219`, `app/core/portas.py:118`, `app/core/juizes.py:216`, `app/colaboradores/services/calendario.py:252`, `app/ponto/status_dia.py:273`, `app/core/services/painel_op.py:327–333`.

5. **Fila | A guarda do chokepoint interpreta a competência pelo timestamp civil.** Para uma S órfã de 24/10 às 07h num vínculo noturno estável 19h–07h, corte 24, o pareador devolve 23/10 e o novo evento agenda 10/2026. A guarda das gravações institucionais e do flip consulta 11/2026 pelo civil. É um caminho herdado fora da autorização de alteração do chokepoint nesta fatia. `app/ponto/turnos.py:285–298`, `app/ponto/janelas.py:75–86`, `app/ponto/registro_batida.py:147–162`, `app/ponto/services/flip_batida.py:21–24`, `app/chamados/services/materializacao.py:857–859`.

## 26/11 11:03 · sessao f3d1e6c7b2e6 · O327-C11-93 · leitura · origem=Code

Na competência **11/2026**, o anexo registra **10 cortes** nos sete PKs. Pelo dia do **mapa interno do motor**, **3** estão dentro da competência e **7** estão fora. Os três de dentro são do `col900` e têm `par_fechado_positivo=false`. A atribuição desses cortes pela chamada direta a `turnos_do_colab` e a perda de minutos trabalhados estão [nao verificado]. `cortados.jsonl:1–7`, `DONO-2611/r.md` (sonda, `ler_motor`), `app/ponto/motor_calculo_v2.py:2362–2365`.

### Quem decide o dia e por que corta

1. **O juiz decide `data_turno`.** `core/juizes.py` aponta para `ponto.turnos.turnos_do_colab`. Essa função lê batidas, células e vínculos, pareia por segmentos de vínculo e devolve os turnos pelo `data_turno`. Na regra da madrugada, `_data_do_turno` pode atribuir o turno à véspera se os marcos indicarem cruzamento de meia-noite e, havendo saída, ela couber no envelope da jornada da véspera. `app/core/juizes.py:214–216`, `app/ponto/turnos.py:1850–1920,1961–2002,216–231,285–324`.
2. **O `MotorBase` decide o `data_jornada` usado no corte.** Ele monta seu mapa com `turnos_de_batidas`, usando as batidas daquela fatia, e indexa `data_turno` pelo instante localizado das `E`. O laço começa com esse dia ou, sem chave, com a data civil da entrada. Se houve saída no período anterior e o intervalo até a entrada atual está entre **0 e 6 horas, inclusive**, `_prev_dj` prevalece. O estado do período anterior é atualizado antes da guarda, inclusive quando esse período é cortado. `app/ponto/motor_calculo_v2.py:867–895,2362–2373,2397–2398,2415–2448`.
3. **A guarda aplica os limites da fatia.** O fechamento recorta a fatia pela competência, vigência, início da operação e dias atribuídos pelo vínculo. A entrada do motor aceita batidas da véspera e do dia seguinte. Depois de montar os períodos, o motor executa `continue` se `data_jornada < data_ini` ou `data_jornada > data_fim`, antes de calcular e registrar o período em `dia_da_jornada`. Nos dez cortes do anexo, `dia_usado` coincide com `dia_juiz` do mapa interno. A herança como causa dos cortes observados está [nao verificado]. `app/ponto/services/fechamento.py:204–230,244–311`, `app/ponto/motor_calculo_v2.py:2445–2448,2473–2476,1090–1092`, `cortados.jsonl:1–7`.

### Dono pela L-085 e pela L-099

| Classe solicitada | Dono e leitura do anexo |
|---|---|
| **Dia do juiz dentro da competência e período cortado** | **ESTRUTURA**, no critério do corte. Pelo mapa interno, são **3 ocorrências**, em `28/10`, `15/11` e `16/11`, todas do `col900` e sem par fechado positivo. O `BATIDA` de `28/10` é o dono da divergência diária registrada pelo E6. `cortados.jsonl:6`, `app/docs/LEIS.md:117,122`, `app/ponto/management/commands/e6_oraculo.py:98–110,182–195`. |
| **Dia do juiz fora da competência** | **Corte legítimo, sem dono de divergência na 11.** A jornada pertence à competência que contém seu dia de início. Pelo mapa interno, são **7 ocorrências**, seis em `20/10` e uma em `21/11`. `cortados.jsonl:1–7`, `app/docs/LEIS.md:117,122`, `app/ponto/janelas.py:54–57`. |

A sonda abaixo observa os dois `continue` no fechamento em modo de leitura. Ela consulta `turnos_do_colab` com dias de borda e procura o instante exato da entrada entre as batidas dos turnos devolvidos. Quando o juiz devolve zero ou mais de um dia para o instante, a linha e o total registram `[nao verificado]`. A execução da sonda e a confirmação independente do hash estão [nao verificado]. `app/ponto/services/fechamento.py:468–475,1152–1160,1215–1225`, `app/ponto/motor_calculo_v2.py:2445–2448`, `app/ponto/turnos.py:1834–1852,1997–2002`.

```python
import sys

_pyc_anterior = sys.dont_write_bytecode
sys.dont_write_bytecode = True
try:
    import json
    import logging
    from collections import Counter, defaultdict
    from contextlib import ExitStack

    from django.conf import settings
    from django.db import connection, transaction
    from django.utils import timezone

    from colaboradores.models import Colaborador
    from ponto.janelas import janela_fechamento
    from ponto.motor_calculo_v2 import MotorBase, localizar
    from ponto.services import fechamento
    from ponto.turnos import janela_com_pad, turnos_do_colab

    def sonda():
        mes, ano = 11, 2026
        pks = (90, 274, 704, 755, 761, 900, 984)  # cortados.jsonl:1–7
        desconhecido = "[nao verificado]"

        # app/config/settings/sombra.py:13–14,47–51,60
        if (
            getattr(settings, "HASNER_SOMBRA", False) is not True
            or connection.settings_dict["NAME"] != "sombra"
            or connection.settings_dict["HOST"] != "sombra_db"
            or getattr(connection, "schema_name", None) != "juliani"
        ):
            raise RuntimeError("Abra o shell da sombra no schema juliani.")

        codigo_fechamento = fechamento.recalcular_fechamento_mes.__code__
        codigo_fatia = fechamento.apurar_por_fatia.__code__
        codigo_motor = MotorBase.calcular_mes.__code__
        cortes = defaultdict(list)

        def ler_motor(frame, evento, argumento):
            # app/ponto/motor_calculo_v2.py:2415–2448
            if evento == "line" and frame.f_lineno in (2446, 2448):
                local = frame.f_locals
                vinculo = local["escala_colaborador"]
                entrada = localizar(local["entrada"])
                cortes[local["colaborador_id"]].append({
                    "entrada": entrada,
                    "saida": localizar(local["saida"]),
                    "data_turno_mapa_do_motor": local["_dia_turno_juiz"].get(entrada),
                    "dia_da_jornada_motor": local["data_jornada"],
                    "fatia": {
                        "inicio": local["data_ini"],
                        "fim": local["data_fim"],
                        "vinculo_id": vinculo.pk,
                        "vinculo_inicio": vinculo.data_inicio,
                        "vinculo_fim": vinculo.data_fim,
                    },
                    "linha_do_corte": frame.f_lineno,
                })
            return ler_motor

        def rastrear(frame, evento, argumento):
            if evento != "call" or frame.f_code is not codigo_motor:
                return None

            # app/ponto/services/fechamento.py:306–311,774–775,847
            # As subclasses podem chamar MotorBase por super().
            pai = frame.f_back
            while pai is not None and pai.f_code is not codigo_fechamento:
                if pai.f_code is codigo_fatia:
                    if pai.f_back is not None and pai.f_back.f_code is codigo_fechamento:
                        return ler_motor
                    return None
                pai = pai.f_back
            return None

        saidas = []
        totais = Counter(
            perda_estrutural_l085=0,
            corte_legitimo=0,
            dia_do_juiz_nao_verificado=0,
        )
        pares_fechados_positivos_dentro = 0

        with ExitStack() as sessao:
            sessao.callback(logging.disable, logging.root.manager.disable)
            logging.disable(sys.maxsize)
            sessao.enter_context(transaction.atomic(durable=True))

            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cursor.execute("SELECT current_database(), current_schema()")
                if cursor.fetchone() != ("sombra", "juliani"):
                    raise RuntimeError("Banco ou schema fora da sombra esperada.")

            colabs = Colaborador.objects.select_related("empresa").in_bulk(pks)
            faltantes = sorted(set(pks).difference(colabs))
            if faltantes:
                raise RuntimeError(f"PKs ausentes na sombra: {faltantes}")

            # app/ponto/services/fechamento.py:559–560,1152–1160,1215–1225
            with ExitStack() as observacao:
                observacao.callback(sys.settrace, sys.gettrace())
                sys.settrace(rastrear)
                leituras = fechamento.recalcular_fechamento_mes(
                    mes, ano, colaborador_ids=pks,
                    somente_leitura=True, lavrar_dias_pagos=False,
                )

            faltantes = sorted(set(pks).difference(leituras))
            if faltantes:
                raise RuntimeError(f"Fechamento de leitura sem retorno para: {faltantes}")

            for pk in pks:
                colab = colabs[pk]
                inicio, fim = janela_fechamento(mes, ano, colab.empresa)
                cortes_pk = cortes.get(pk, ())
                dias_por_instante = defaultdict(set)

                if cortes_pk:
                    # app/ponto/turnos.py:1834–1852,1997–2002
                    juiz_inicio, juiz_fim = janela_com_pad(inicio, fim)
                    for turno in turnos_do_colab(colab, juiz_inicio, juiz_fim):
                        if turno.data_turno is not None:
                            for batida in turno.batidas:
                                instante = localizar(batida.timestamp)
                                dias_por_instante[instante].add(turno.data_turno)

                for corte in cortes_pk:
                    entrada, saida = corte["entrada"], corte["saida"]
                    dias = sorted(dias_por_instante.get(entrada, ()))
                    dia_juiz = dias[0] if len(dias) == 1 else None
                    dentro = inicio <= dia_juiz <= fim if dia_juiz is not None else None
                    par_fechado_positivo = saida is not None and saida > entrada

                    # app/docs/LEIS.md:117,122
                    if dentro is None:
                        classe = "dia_do_juiz_nao_verificado"
                        dono = desconhecido
                    elif dentro:
                        classe = "perda_estrutural_l085"
                        dono = "ESTRUTURA"
                        pares_fechados_positivos_dentro += int(par_fechado_positivo)
                    else:
                        classe = "corte_legitimo"
                        dono = None
                    totais[classe] += 1

                    saidas.append({
                        **corte,
                        "pk": pk,
                        "entrada": timezone.localtime(entrada).isoformat(),
                        "saida": timezone.localtime(saida).isoformat() if saida is not None else None,
                        "data_turno_juiz": dia_juiz if dia_juiz is not None else desconhecido,
                        "datas_turno_candidatas": dias,
                        "janela_fechamento": [inicio, fim],
                        "dia_juiz_dentro_da_competencia": dentro,
                        "par_fechado_positivo": par_fechado_positivo,
                        "classe_do_corte": classe,
                        "dono_do_corte_l099": dono,
                    })

                if not cortes_pk:
                    saidas.append({
                        "pk": pk,
                        "janela_fechamento": [inicio, fim],
                        "n_periodos_cortados": 0,
                    })

        for linha in saidas:
            print(json.dumps(linha, default=str, sort_keys=True))
        print(json.dumps({
            "competencia": "2026-11",
            "total_pks": len(pks),
            "total_periodos_cortados": sum(totais.values()),
            "por_classe": dict(totais),
            "pares_fechados_positivos_dentro": pares_fechados_positivos_dentro,
        }, sort_keys=True))

    sonda()
finally:
    sys.dont_write_bytecode = _pyc_anterior
```

## 26/11 11:02 · sessao c8a671674d30 · O30-F3-CONSTRUIR · construtor · origem=Code

A cura c5 está na cópia sobre `8d48f1f35393dcd86345b5c866bfd4978d668181`. `rederivar_fim` calcula o fim uma vez, confere o intervalo e entrega a mesma data ao `Ausencia.save`. O modelo deriva minutos, dias corridos e carimbo. O teste executa o `save` do modelo com persistência em memória e confirma **1.440 minutos** pela R8 (`0 + 480 + 480 + 480`). [ausencia.py:1915](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1915) · [models.py:892](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/models.py:892) · [test_atestado_f3_contrato.py:735](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:735)

- **L-111.** Removi `_fixar_raiz_documento`, seus dois chamadores e `d1_fixar_aceita_filha`. A lápide ficou junto à trava que resolve a raiz. [ausencia.py:296](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:296) · [mutantes_atestado_f3.py:198](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:198)
- **Painel.** A view chama `raiz_documento` uma vez. O template usa a raiz recebida no contexto. Removi a tag e sua biblioteca. [views.py:2095](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2095) · [painel_ausencia.html:71](/home/ronald/wt-codex-o30-f3-construir-010143/app/templates/ponto/partials/painel_ausencia.html:71)
- **Verificação local.** Passaram 46 testes de contrato sem banco e 11 testes do selo de escritores com `unittest.TestCase` no lugar de `SimpleTestCase`. Alinhei também o teste de integração de três dias à R8. Integração com banco, renderização real do painel e produção [nao verificado]. A certificação cabe ao Code e ao Opus. [test_atestado_f3_contrato.py:241](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:241) · [test_lote7_selo_escritor_acorda.py:296](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_lote7_selo_escritor_acorda.py:296) · [test_lote7_fato_de_ausencia_acorda_projecao.py:618](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:618) · [LEIS.md:147](/home/ronald/wt-codex-o30-f3-construir-010143/app/docs/LEIS.md:147) [nao verificado]

### Diff, RED e mutantes

O diff passou em `git diff --check`. Cada `FAIL` abaixo saiu com `rc=1` por asserção e zero `ERROR`. O RED histórico `red_c2_c3` deu `ERROR` de importação também ao executar o runner da base c4. [mutantes_atestado_f3.py:66](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:66) · [mutantes_atestado_f3.py:216](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:216) · [atestados_lote.py:19](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/atestados_lote.py:19) [nao verificado]

```bash
git diff 8d48f1f35393dcd86345b5c866bfd4978d668181 -- .

export PYTHONDONTWRITEBYTECODE=1
python3 -m unittest -q ponto.tests.test_atestado_f3_contrato
python3 -m ponto.tests.mutantes_atestado_f3 red_c4                       # 5 FAIL
python3 -m ponto.tests.mutantes_atestado_f3 d5_ignora_fim_cercado         # FAIL: 28/09 no lugar de 26/09
python3 -m ponto.tests.mutantes_atestado_f3 c5_minutos_usam_fim_anterior  # FAIL: 0 != 1440
python3 -m ponto.tests.mutantes_atestado_f3 c5_rederivar_save_base       # FAIL: selo AST
python3 -m ponto.tests.mutantes_atestado_f3 c5_rederivar_timezone_now    # FAIL: selo AST
python3 -m ponto.tests.mutantes_atestado_f3 c5_regrava_referencia         # FAIL: selo da escrita da referência
python3 -m ponto.tests.mutantes_atestado_f3 c5_painel_recalcula_raiz      # FAIL: selo do template
python3 -m ponto.tests.mutantes_atestado_f3 save_integral                 # FAIL: selo de escritor único
python3 -m ponto.tests.mutantes_atestado_f3 d4_append                    # FAIL: selo de escritor único
python3 -m ponto.tests.mutantes_atestado_f3 d4_atribuicao_em_ramo         # FAIL: selo de escritor único
python3 -m ponto.tests.mutantes_atestado_f3 red_c3_d1                    # 2 FAIL
python3 -m ponto.tests.mutantes_atestado_f3 red_c3_d5                    # 2 FAIL
```

## 26/11 11:00 · sessao 48cb3acf6f01 · R1-SEM-DONO-2611 · leitura · origem=Code

```python
import json
from collections import Counter
from datetime import date

from django.apps import apps
from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono,
    batida_fora_de_turno,
    diferenca_e_a_janela_declarada,
    minutos_do_espelho,
)
from ponto.services.espelho import espelho_do_colab
from ponto.supra_juiz import cadastro_do_dia_carregado


NV = "[nao verificado]"
MES, ANO = 11, 2026

# PUBLICADO.md, cabeçalho e §1, identifica a foto de referência como sombra de 26/11. [nao verificado]
# A data da referência é usada como hoje. A hora original e a identidade da
# foto conectada ficam [nao verificado] até serem informadas pela sessão.
# app/ponto/management/commands/e6_oraculo.py:284-297
HOJE = date(2026, 11, 26)
AGORA_DA_RODADA = None  # datetime com fuso, se a sessão tiver o instante original
FOTO_LIDA = NV         # identificador do dump, se a sessão o tiver

# PUBLICADO.md §3 traz as classes das 18 chaves. Os deltas inteiros dessas [nao verificado]
# chaves estão [nao verificado]. As referências abaixo são as citadas no anexo.
# Para col200 e col382, PUBLICADO.md §2 traz espelho × piso–teto. O comando [nao verificado]
# arredonda o delta separadamente dos operandos.
# app/ponto/management/commands/e6_oraculo.py:362-363,401-404
DELTA_PUBLICADO = {
    (112, "2026-10-21"): NV,  # depois_c11.txt:14
    (112, "2026-10-22"): NV,  # depois_c11.txt:15
    (200, "2026-11-03"): NV,  # depois_c11.txt:23
    (382, "2026-11-11"): NV,  # depois_c11.txt:36
    (114, "2026-11-08"): NV,  # depois_c11.txt:18-19
    (252, "2026-11-18"): NV,  # depois_c11.txt:26-32
    (252, "2026-11-19"): NV,  # depois_c11.txt:26-32
    (884, "2026-11-08"): NV,  # depois_c11.txt:41
    (922, "2026-11-01"): NV,  # depois_c11.txt:43-46
    (922, "2026-11-04"): NV,  # depois_c11.txt:43-46
    (302, "2026-11-20"): NV,  # depois_c11.txt:34
    (996, "2026-10-28"): NV,  # depois_c11.txt:47
    (114, "2026-11-15"): NV,  # depois_c11.txt:18-19
    (130, "2026-10-29"): NV,  # depois_c11.txt:20
    (220, "2026-11-10"): NV,  # depois_c11.txt:25
    (874, "2026-10-22"): NV,  # depois_c11.txt:40
    (989, "2026-10-23"): NV,  # depois_c11.txt:43-46
    (996, "2026-11-10"): NV,  # depois_c11.txt:48-49
}
DONO_PUBLICADO = "ESTRUTURA"  # PUBLICADO.md §1, §3 e r.md, censo das 18 chaves [nao verificado]


def sonda():
    assert len(DELTA_PUBLICADO) == 18

    # app/config/settings/sombra.py:13-14,29,49-60
    # bin/sombra_mascara.py:32-38
    assert getattr(settings, "HASNER_SOMBRA", False) is True
    assert settings.DATABASES["default"]["NAME"] == "sombra"
    assert connection.settings_dict["NAME"] == "sombra"
    assert connection.schema_name == "juliani"
    assert not connection.in_atomic_block and connection.get_autocommit()

    # app/ponto/management/commands/e6_oraculo.py:284-297
    # CLAUDE.md:147-148
    agora = timezone.localtime(AGORA_DA_RODADA or timezone.now())
    origem_agora = (
        "informado pela sessão" if AGORA_DA_RODADA is not None else "execução da sonda"
    )

    Colaborador = apps.get_model("colaboradores", "Colaborador")
    FechamentoMensal = apps.get_model("ponto", "FechamentoMensal")
    pks = {pk for pk, _ in DELTA_PUBLICADO}
    linhas = []

    with transaction.atomic():
        # app/chamados/tests/cef_c3_sonda.py:16-22
        # bin/sombra_db.sh:45-47
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema(), pg_current_snapshot()::text")
            banco, schema, snapshot_mvcc = cursor.fetchone()
        assert banco == "sombra" and schema == "juliani"

        # Universo de empresas ativas e fechamentos da competência.
        # app/ponto/management/commands/e6_oraculo.py:292-305,314
        colabs = Colaborador.objects.select_related("empresa").in_bulk(pks)
        fechamentos = set(
            FechamentoMensal.objects.filter(
                mes=MES, ano=ANO, colaborador_id__in=pks
            ).values_list("colaborador_id", flat=True)
        )

        por_colab = {}
        for pk, colab in colabs.items():
            # Mesmas chamadas e mesmo objeto espelho passado ao carregador.
            # app/ponto/management/commands/e6_oraculo.py:303-320,327-330
            ini, fim = janela_fechamento(MES, ANO, colab.empresa)
            espelho = espelho_do_colab(colab, HOJE, ini, fim, agora=agora)
            ins = InsumosDoDono(colab, ini, fim, HOJE, espelho=espelho, agora=agora)
            dias_espelho = {
                str(linha.get("data"))[:10]: linha for linha in (espelho.get("dias") or [])
            }
            por_colab[pk] = (ins, dias_espelho)

        for (pk, k), publicado in DELTA_PUBLICADO.items(): [nao verificado]
            dia = date.fromisoformat(k)
            colab = colabs.get(pk)
            ins, dias_espelho = por_colab.get(pk, (None, {}))
            de = dias_espelho.get(k)
            campos = de if de is not None else {}

            # app/ponto/services/espelho.py:801,829-837,863-874
            # app/ponto/management/commands/e6_oraculo.py:236-239,263-269,339-363
            isencao = campos.get("isencao")
            ausencia = campos.get("ausencia") is not None if de is not None else None
            leitura_espelho = minutos_do_espelho(de)
            m_esp, _ = leitura_espelho
            veredito = ins.oraculo if ins is not None else None
            piso = veredito.minutos.get(k, 0.0) if veredito is not None else None
            teto = veredito.envelope.get(k, 0.0) if veredito is not None else None
            delta = piso - m_esp if piso is not None and m_esp is not None else None
            guarda = diferenca_e_a_janela_declarada(delta, de) if delta is not None else None
            delta_inteiro = round(delta) if delta is not None else None

            # app/ponto/management/commands/e6_oraculo.py:98-110,149-154,156-212,393-404
            # app/ponto/supra_juiz.py:179-191
            horas = veredito.horas.get(k, []) if veredito is not None else None
            cadastro = dict.fromkeys((
                "folga_nao_definida",
                "sem_vinculo_no_dia",
                "sem_vinculo_em_dia_de_trabalho",
            ))
            entrada_solta = paridade = fase = cxr = dono = None
            if ins is not None:
                cadastro = cadastro_do_dia_carregado(pk, dia)
                entrada_solta = batida_fora_de_turno(colab, dia, ins.turnos())
                paridade = "impar" if len(horas) % 2 else "par"
                fase = k in ins.fase()
                cxr = k in ins.cxr()
                dono = ins.dono(dia)

            # Saídas do laço do R1, na ordem do command. A sonda também lê
            # os campos de diagnóstico das chaves que ficam fora da lista.
            # app/ponto/management/commands/e6_oraculo.py:303-305,331-378
            exclusao = None
            if colab is None:
                exclusao = "colaborador_ausente"
            elif not colab.empresa.ativa:
                exclusao = "empresa_inativa"
            elif pk not in fechamentos:
                exclusao = "sem_fechamento_da_competencia"
            elif not ins.ini <= dia <= ins.fim:
                exclusao = "fora_da_janela_de_fechamento"
            elif de is None:
                exclusao = "dia_sem_linha_no_espelho"
            elif isencao or ausencia:
                exclusao = "dia_isento_ou_ausencia"
            elif m_esp is None:
                exclusao = "dia_sem_lavratura_no_espelho"
            elif m_esp == 0 and piso == 0:
                exclusao = "dia_sem_trabalho_ambos"
            elif guarda:
                exclusao = "esp_desconta_janela_declarada"
            elif abs(delta) <= 10:
                exclusao = "bate_ate_10min"

            chave = f"col{pk} {k}"
            linhas.append({
                "chave": chave,
                "linha_no_espelho": de is not None,
                "isencao": isencao,
                "ausencia_presente": ausencia,
                "minutos_realizados": campos.get("minutos_realizados"),
                "pago_h": campos.get("pago_h"),
                "minutos_do_espelho": leitura_espelho,
                "piso_min": piso,
                "teto_min": teto,
                "horas": horas,
                "brutas": veredito.brutas.get(k, 0) if veredito is not None else None,
                "impares": k in veredito.impares if veredito is not None else None,
                "cortes": k in veredito.cortes if veredito is not None else None,
                "realizado_janela_descontada": campos.get("realizado_janela_descontada"),
                "guarda_janela": guarda,
                "delta_antes_de_arredondar_min": delta,
                "delta_arredondado_min": delta_inteiro,
                "delta_publicado_min": publicado, [nao verificado]
                "delta_bate_publicado": (
                    delta_inteiro == publicado [nao verificado]
                    if delta_inteiro is not None and publicado != NV else NV [nao verificado]
                ),
                **cadastro,
                "entrada_solta_apuravel": entrada_solta,
                "paridade_de_horas": paridade,
                "fase_deslocada_as_of": fase,
                "em_cadastro_x_realidade": cxr,
                "ins_dono": dono,
                "dono_r1": dono if exclusao is None else None,
                "exclusao_r1": exclusao,
            })

    assert len(linhas) == len({linha["chave"] for linha in linhas}) == 18
    for linha in linhas:
        print(json.dumps(linha, ensure_ascii=False, allow_nan=False))

    # O R1 conta dono nas divergências que passaram pelo laço.
    # app/ponto/management/commands/e6_oraculo.py:364-381,427-439
    contagem = Counter(linha["dono_r1"] for linha in linhas if linha["exclusao_r1"] is None)
    print(json.dumps({
        "chaves_conferidas": len(linhas),
        "banco": banco,
        "schema": schema,
        "snapshot_mvcc": snapshot_mvcc,
        "foto_lida": FOTO_LIDA,
        "foto_de_referencia": "sombra de 26/11/2026 (PUBLICADO.md, cabeçalho)", [nao verificado]
        "hoje": HOJE.isoformat(),
        "agora_local": agora.isoformat(),
        "origem_agora": origem_agora,
        "contagem_por_dono": {
            dono: contagem[dono] for dono in ("ESTRUTURA", "CADASTRO", "BATIDA")
        },
        "chaves_fora_do_r1": [
            {"chave": linha["chave"], "motivo": linha["exclusao_r1"]}
            for linha in linhas if linha["exclusao_r1"] is not None
        ],
        "chaves_com_dono_diferente_do_publicado": [
            linha["chave"] for linha in linhas
            if linha["exclusao_r1"] is None and linha["dono_r1"] != DONO_PUBLICADO
        ],
        "chaves_cujo_delta_nao_bate_o_publicado": [
            linha["chave"] for linha in linhas if linha["delta_bate_publicado"] is False
        ],
        "chaves_com_confronto_do_delta_nao_verificado": [
            linha["chave"] for linha in linhas if linha["delta_bate_publicado"] == NV
        ],
    }, ensure_ascii=False, allow_nan=False))


# Execução na sombra [nao verificado].
sonda()
```

## 26/11 10:57 · sessao 6fd2e2a15712 · O327-C11-93 · leitura · origem=Code

Os 95 PKs fornecidos chegaram ao ramo 1. Na saída raspada, todos têm `v.minutos` vazio, zero bloqueios na janela e 11 campos sem dono. A sonda anterior mediu a condição da guarda por `_DONOS_DECLARADOS`. A causa por PK e a ocorrência efetiva da exceção em produção estão [nao verificado]. `recusados.txt:1–95`, `app/ponto/calculador/alimentacao.py:449,467–475`, `app/ponto/services/dia_pago.py:50–76,312–321`.

### 1. Caminhos até `v.minutos` vazio

O universo admite quem tem situação diferente de `desligado` ou quem está desligado com demissão a partir do início da janela da empresa. A seleção dispensa testes de admissão, vigência, célula e batida. Já `rubricas_por_dia` passa **sempre** `pares=` ao núcleo. Nesse ramo, uma chave de `v.minutos` só nasce quando chega uma tupla com entrada e saída. As batidas avulsas alimentam o diagnóstico, e uma entrada sem saída só marca o dia como ímpar. Um par entregue ao núcleo com duração zero ainda criaria uma chave. `app/ponto/services/fechamento.py:448–465,640–653,1289–1300`, `app/ponto/calculador/alimentacao.py:449`, `app/ponto/calculador/nucleo.py:109–155`.

| Caminho | O que o código faz |
|---|---|
| **Sem batida apurável, inclusive no envelope** | A porta exclui retratadas. O fechamento busca também a véspera e o dia seguinte, então zero no recorte civil da janela por si só é insuficiente. Sem batidas no envelope não há par no motor. `app/ponto/turnos.py:2921–2948`, `app/ponto/services/fechamento.py:640–653`, `app/ponto/motor_calculo_v2.py:897–912,2362–2373`, `app/ponto/adaptador_motor.py:28–45`. |
| **Vínculo fora da janela ou nenhuma fatia executada** | O acumulador começa vazio. A fatia é limitada pela vigência, pelo início da operação e pela atribuição da célula. Pode ser pulada por inteiro, ou rodar sem receber as batidas que ficaram fora de sua cobertura. No O85, a recuperação de dias órfãos, quando admitida, segue por `orfaos_por_dia`, fora dos períodos que alimentam o calculador. O caso sem vínculo algum tem tratamento cadastral explícito. `app/ponto/services/fechamento.py:200–225,225–311,787–863,1293–1305`, `app/escala/alimentacao.py:148–159,205–216,269–286`. |
| **Sem célula** | A falta da célula desvia o pareamento da ata para a autoridade de geometria. A fatia ainda pode apurar pela vigência. No recorte de sobreposição em que já há dias atribuídos pela célula, um dia sem dono entra pela vigência única. A grade sem células dentro do vínculo tem uma saída própria, que pode deixar o previsto e o realizado sem resposta enquanto o motor ainda consegue parear. `app/ponto/motor_calculo_v2.py:558–576,611–638`, `app/ponto/services/fechamento.py:228–290,1044–1069,1111–1123`, `app/escala/models.py:1080–1107`. |
| **Ausência cobrindo** | A cobertura da folha usa ausências aprovadas. Ela alimenta justificação, supressão do previsto ou desconto segundo o tipo. O abono integral zera atraso e saída antecipada e preserva o trabalho apurado. A cobertura chamada pelo fechamento atua sobre esses insumos, enquanto as batidas já foram lidas em uma porta própria. A ausência pode explicar o dia sem par ou a falta de previsto, mas sua presença isolada não prova por que o par faltou. `app/ponto/turnos.py:2528–2537,2741–2753`, `app/ponto/services/fechamento.py:652–660,700–721,744–775`, `app/ponto/motor_calculo_v2.py:1095–1122`. |
| **Empresa ou regime** | A empresa define o corte da competência e a aplicação da janela de HE. CLT, CCT ausente e regime vazio têm caminhos que devolvem motor. Intermitente também tem motor e apura pares fechados. `CicloSemMotor` na montagem da fatia interrompe o cálculo antes da chamada de rubricas. Uma janela de HE aplicável pode colapsar o par, que então segue pelo caminho de segmento descartado. `app/ponto/janelas.py:38–57`, `app/core/regua_cct.py:351–373,428–489`, `app/ponto/motor_calculo_v2.py:1389–1412,3589–3605,3657–3676`, `app/ponto/services/fechamento.py:744–775,1327–1332`. |
| **Batida apurável sem par fechado** | O motor restringe a lista a E/S e remove duplicatas antes de parear. Saída órfã vira anomalia sem período. Entrada em aberto pode chegar ao núcleo com `saida=None`. O motor pode creditar uma parte já fechada a um período que segue aberto. A sonda mede esse crédito separadamente, pois o segmento aberto enviado ao núcleo ainda fica sem saída. `app/ponto/motor_calculo_v2.py:897–912,2362–2373,2527–2580`, `app/ponto/adaptador_motor.py:28–45`, `app/ponto/calculador/alimentacao.py:83–113`, `app/ponto/calculador/nucleo.py:139–147`. |
| **Período cortado por dia ou descartado pela alimentação** | O motor corta o período cujo dia de jornada caiu fora da fatia. A alimentação pula períodos sem mapa do dia, fechados sem carimbo e pares efetivos com `saída <= entrada`. O bloqueio HE100 sem previsto também colapsa o par, mas, se publicado dentro da janela, ganha um item de rubrica com zero. Esse último mecanismo explica `v.minutos` vazio com rubrica, e está ausente nas 95 linhas fornecidas. `app/ponto/motor_calculo_v2.py:2415–2448,1479–1505`, `app/ponto/calculador/alimentacao.py:73–123,459–475`, `recusados.txt:1–95`. |

### 2. Dono e decisão sobre a recusa

- **A guarda está correta para a chamada medida.** Com `{}` e 11 campos sem dono, `dia_pago.py:317` deve recusar antes de substituir as linhas da versão oráculo. Se faltam os fatos necessários para afirmar trabalho ou uma rubrica, inventar zero seria incorreto. Se já há trabalho apurado, abono, falta decidida ou outro fato afirmado, a guarda continua correta e a perda desse fato na composição é o que precisa ser explicado. O escritor já tem entradas próprias para esses fatos e para o ajuste mensal. `app/ponto/services/dia_pago.py:50–76,173–186,234–292,312–321,348–361`, `app/docs/LEIS.md:61,65,113,122`.
- **O E6 julga divergência de minutos por dia.** A recusa por campos sem dono é da chamada inteira. O E6 só chama o juiz depois de encontrar um dia comparável com diferença acima de dez minutos e excluir a diferença explicada pela janela declarada. Em um dia divergente, vínculo ausente ou folga indefinida têm precedência como `CADASTRO`, batida solta ou ímpar pode dar `BATIDA`, e fase lavrada ou cadastro × realidade podem dar `CADASTRO`. `ESTRUTURA` é o resultado residual de uma divergência comparável. A sonda aplica esse mesmo filtro por dia e chama `InsumosDoDono.dono()`. O dono individual das divergências está [nao verificado]. `app/ponto/management/commands/e6_oraculo.py:98–110,128–154,182–195,328–379`, `app/docs/LEIS.md:122`.
- **O corte legítimo usa o dia do juiz e a célula soberana.** Pela L-085, um turno do juiz fora da competência fica na competência a que pertence. Se um par fechado com dia do juiz dentro da fatia é descartado porque a herança de 0–6 h o levou para fora, a perda é estrutural. A resposta NOTURNO-A também manda preservar o trabalhado após o marco quando a saída na madrugada fecha um turno que não cruza. A L-097 classifica a HE desse trecho. Ausência aprovada e vínculo fora da vigência mantêm o tratamento que o cadastro e a célula sustentam. A presença desses casos entre os 95 está [nao verificado]. `app/docs/LEIS.md:71,115,117,121–122`, `app/docs/PROMPTS.md:2928`, `app/escala/alimentacao.py:269–286`, `app/ponto/motor_calculo_v2.py:936–951,2415–2448`, `app/ponto/janela_he.py:173–191`.

A sonda usa a janela de cada empresa, intercepta as duas lavras em memória e imprime uma linha JSON por PK, seguida da contagem. O total por caminho admite sobreposição quando duas condições foram observadas no mesmo PK. A execução da sonda e a confirmação independente do hash da cópia estão [nao verificado]. `app/ponto/services/fechamento.py:574–577,640–653,1215–1225,1289–1320`, `app/ponto/services/dia_pago.py:359–360`.

```python
import sys

_pyc_anterior = sys.dont_write_bytecode
sys.dont_write_bytecode = True

import json
import logging
from collections import Counter, defaultdict
from contextlib import ExitStack
from datetime import datetime, time, timedelta
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.alimentacao import (
    celulas_do_periodo,
    escalas_do_periodo,
    vinculo_cobre_a_data,
    vinculo_do_dia,
)
from ponto.calculador import alimentacao
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono,
    diferenca_e_a_janela_declarada,
    minutos_do_espelho,
)
from ponto.motor_calculo_v2 import MotorBase, localizar
from ponto.services import dia_pago, fechamento
from ponto.services.espelho import espelho_do_colab
from ponto.turnos import (
    ausencias_vigentes,
    batidas_apuraveis,
    cobertura_ausencia_periodo,
    dias_da_ausencia,
)


MES, ANO = 11, 2026
UM_DIA = timedelta(days=1)
# recusados.txt:1–95
PKS = tuple(map(int, """
041 053 066 075 076 083 090 128 142 144 181 186 199 209 210 216
225 234 236 257 268 271 274 291 298 312 317 318 379 391 427 440
499 518 534 574 607 638 641 642 644 645 646 647 649 650 663 667
674 675 677 678 697 699 704 717 718 751 752 755 758 761 832 883
900 915 921 938 966 973 980 984 1009 1016 1025 1027 1033 1043
1049 1053 1054 1055 1056 1057 1058 1059 1060 1061 1062 1063
1064 1065 1066 1067 1068
""".split()))


def donos_e6(colab, ini, fim, dias, agora):
    # app/ponto/management/commands/e6_oraculo.py:328–379
    hoje = timezone.localtime(agora).date()
    espelho = espelho_do_colab(colab, hoje, ini, fim, agora=agora)
    insumos = InsumosDoDono(colab, ini, fim, hoje, espelho=espelho, agora=agora)
    linhas = {str(d.get("data"))[:10]: d for d in espelho.get("dias") or ()}
    donos = {}
    saidas = Counter()

    for dia in dias:
        chave = dia.isoformat()
        linha = linhas.get(chave)
        if linha is None:
            saidas["sem_linha_no_espelho"] += 1
            continue
        if linha.get("isencao") or linha.get("ausencia") is not None:
            saidas["isento_ou_ausencia"] += 1
            continue
        minutos_espelho, _ = minutos_do_espelho(linha)
        if minutos_espelho is None:
            saidas["sem_lavratura_no_espelho"] += 1
            continue
        minutos_e6 = insumos.oraculo.minutos.get(chave, 0.0)
        diferenca = minutos_e6 - minutos_espelho
        if minutos_espelho == minutos_e6 == 0:
            saidas["sem_trabalho_em_ambos"] += 1
        elif diferenca_e_a_janela_declarada(diferenca, linha):
            saidas["janela_declarada"] += 1
        elif abs(diferenca) <= 10:
            saidas["ate_10_minutos"] += 1
        else:
            donos[chave] = insumos.dono(dia)
    return donos, dict(saidas)


def caminhos_da_recusa(obs, apuracao, motor):
    if obs is None or "campos_sem_dono" not in obs:
        return ["rubricas_ou_guarda_nao_observadas"]
    if obs["ramo"] != 1:
        return [f"ramo_{obs['ramo']}_na_sombra"]

    caminhos = []
    cnt = obs["segmentos"]
    if obs["n_batidas_envelope"] == 0:
        caminhos.append("sem_batida_apuravel_no_envelope")
    if apuracao["n_fatias"] == 0:
        caminhos.append("sem_fatia_executada")
    elif obs["n_batidas_envelope"] and motor["batidas_nas_fatias"] == 0:
        caminhos.append("batidas_fora_da_entrada_das_fatias")
    if motor["batidas_nas_fatias"] and motor["batidas_apos_filtro_motor"] == 0:
        caminhos.append("batidas_filtradas_no_motor")
    if motor["batidas_apos_filtro_motor"] and motor["periodos_montados"] == 0:
        caminhos.append("pareador_sem_periodo")
    if obs["cortes_da_fatia"]:
        caminhos.append("periodo_cortado_pelo_dia_da_fatia")
    for motivo in (
        "turno_aberto",
        "sem_dia_da_jornada",
        "sem_carimbo_do_motor",
        "par_invertido_pela_janela",
        "par_bloqueado_sem_previsto",
    ):
        if cnt.get(motivo):
            caminhos.append(motivo)
    return caminhos or ["outro [nao verificado]"]


def sonda():
    # app/config/settings/sombra.py:13–14,29,47–51,60
    if (
        getattr(settings, "HASNER_SOMBRA", False) is not True
        or connection.settings_dict["NAME"] != "sombra"
        or connection.settings_dict["HOST"] != "sombra_db"
        or connection.schema_name != "juliani"
    ):
        raise RuntimeError("Abra o shell da sombra no schema juliani.")
    if (MES, ANO) in fechamento.GUARDA_COMPETENCIA:
        raise RuntimeError("Competencia presente em GUARDA_COMPETENCIA.")
    if len(PKS) != 95 or len(set(PKS)) != 95:
        raise ValueError("Lista de PKs diferente de recusados.txt.")

    codigo_fechamento = fechamento.recalcular_fechamento_mes.__code__
    codigo_fatia = fechamento.apurar_por_fatia.__code__
    codigo_motor = MotorBase.calcular_mes.__code__
    codigo_rubricas = alimentacao.rubricas_por_dia.__code__

    apuracoes, observacoes = {}, {}
    motores = defaultdict(Counter)
    cortes = defaultdict(list)
    pk_em_fatia = None

    def ler_motor(frame, evento, retorno):
        locais = frame.f_locals
        # app/ponto/motor_calculo_v2.py:2415–2448
        if evento == "line" and frame.f_lineno in (2446, 2448):
            entrada, saida = locais["entrada"], locais["saida"]
            dia_juiz = locais["_dia_turno_juiz"].get(localizar(entrada))
            cortes[pk_em_fatia].append({
                "dia_usado": locais["data_jornada"],
                "dia_juiz": dia_juiz,
                "fatia": [locais["data_ini"], locais["data_fim"]],
                "par_fechado_positivo": saida is not None and saida > entrada,
                "juiz_dentro_da_fatia": (
                    dia_juiz is not None
                    and locais["data_ini"] <= dia_juiz <= locais["data_fim"]
                ),
            })
        if evento == "return" and retorno is not None:
            # app/ponto/motor_calculo_v2.py:2362–2373,2733–2737
            motores[pk_em_fatia].update({
                "batidas_nas_fatias": len(locais["batidas"]),
                "batidas_apos_filtro_motor": len(locais["batidas_limpas"]),
                "periodos_montados": len(locais["periodos"]),
            })
        return ler_motor

    def ler_fatia(frame, evento, retorno):
        nonlocal pk_em_fatia
        if evento == "return":
            if retorno is not None:
                # app/ponto/services/fechamento.py:283–311,338–341
                apuracoes[pk_em_fatia] = {
                    "n_fatias": len(retorno.regua_por_fatia),
                    "dias_cobertos": sorted(retorno.dias_cobertos),
                    "motores": sorted(frame.f_locals["_classes_observadas"])
                    if "_classes_observadas" in frame.f_locals else [],
                }
            pk_em_fatia = None
        return ler_fatia

    def ler_rubricas(frame, evento, retorno):
        if evento == "return" and isinstance(retorno, dict):
            # app/ponto/calculador/alimentacao.py:449–475,533
            locais = frame.f_locals
            chamador = frame.f_back.f_locals
            pk = locais["c"].pk
            itens, bloqueios = locais["_itens"], locais["_bloq"]
            n_minutos = len(locais["v"].minutos)
            ramo = (
                (3 if len(itens) > n_minutos else 0)
                if retorno else (2 if itens else 1)
            )
            observacoes[pk] = {
                "ramo": ramo,
                "rubricas_objeto": retorno,
                "n_dias_rubricas": len(retorno),
                "n_dias_v_minutos": n_minutos,
                "n_bloqueios_na_janela": len(bloqueios),
                "n_batidas_envelope": len(locais["batidas_locais"] or ()),
                "segmentos": dict(locais["insumos"]["contadores"]),
                "cortes_da_fatia": cortes.get(pk, []),
                "dias_previsto_suprimido_por_ausencia": sorted(
                    chamador["_datas_suprimidas"]
                ),
                "dias_abono_integral": sorted(chamador["_dias_aus_inteira"]),
                "grade": Counter(
                    d.get("tipo_dia") or "sem_tipo"
                    for d in (chamador.get("_dias_g") or ())
                ),
            }
        return ler_rubricas

    def rastrear(frame, evento, argumento):
        if evento != "call":
            return None
        codigo = frame.f_code
        pai = frame.f_back
        if codigo is codigo_fatia and pai.f_code is codigo_fechamento:
            nonlocal pk_em_fatia
            pk_em_fatia = frame.f_locals["colab"].pk
            frame.f_trace_lines = False
            return ler_fatia
        if codigo is codigo_motor and pk_em_fatia is not None:
            return ler_motor
        if codigo is codigo_rubricas and pai.f_code is codigo_fechamento:
            frame.f_trace_lines = False
            return ler_rubricas
        return None

    def interceptar_lavra(colab, mes, ano, *, versao, rubricas=None, **dados):
        # app/ponto/services/fechamento.py:1223–1261,1301–1320
        # app/ponto/services/dia_pago.py:50–76,312–321,359–360
        if versao == "motor":
            return 0
        obs = observacoes[colab.pk]
        if versao != "oraculo" or (mes, ano) != (MES, ANO):
            raise ValueError("Chamada de lavra fora do escopo da sonda.")
        if rubricas is not obs.pop("rubricas_objeto"):
            raise ValueError("Lavra recebeu outro objeto de rubricas.")
        fontes = (
            "faltas_por_dia", "abono_por_dia",
            "previsto_por_dia", "realizado_por_dia",
        )
        mapas = [dados[campo] for campo in fontes]
        donos = dia_pago._DONOS_DECLARADOS(
            rubricas, *mapas, do_motor=dados.get("do_motor", ()),
        )
        obs["campos_sem_dono"] = sorted(set(dia_pago.CAMPOS_DIA) - donos)
        obs["recusa_317"] = bool(obs["campos_sem_dono"])
        obs["dias_por_mapa_da_folha"] = {
            campo: None if mapa is None else len(mapa)
            for campo, mapa in zip(fontes, mapas)
        }
        obs["n_furos_afirmaveis"] = len(dados.get("furos_do_motor") or ())
        obs["minutos_orfaos_o85"] = sum(
            (dados.get("orfaos_por_dia") or {}).values()
        )
        obs["trabalho_positivo_em_periodos"] = any(
            (p.minutos_trabalhados or 0) > 0
            for campo in ("periodos", "ft_certa", "ft_feriado", "ft_sem_escala")
            for p in (dados.get(campo) or ())
        )
        return 0

    linhas = []
    total_caminhos, total_contextos = Counter(), Counter()
    with ExitStack() as sessao:
        sessao.callback(logging.disable, logging.root.manager.disable)
        logging.disable(sys.maxsize)
        sessao.enter_context(transaction.atomic(durable=True))
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco ou schema fora da sombra esperada.")

        universo = set(
            fechamento.universo_da_competencia(MES, ANO)
            .filter(pk__in=PKS).values_list("pk", flat=True)
        )
        colabs = Colaborador.objects.select_related("empresa").in_bulk(PKS)
        agora = timezone.now()

        with ExitStack() as observacao:
            observacao.enter_context(patch.object(dia_pago, "lavrar", interceptar_lavra))
            observacao.callback(sys.settrace, sys.gettrace())
            sys.settrace(rastrear)
            fechamento.recalcular_fechamento_mes(
                MES, ANO, colaborador_ids=PKS,
                somente_leitura=True, lavrar_dias_pagos=True,
            )

        for pk in PKS:
            linha = {"pk": pk, "col": f"col{pk:03d}", "no_universo": pk in universo}
            colab = colabs.get(pk)
            if colab is None:
                linha["caminhos"] = ["pk_ausente_na_sombra"]
                linhas.append(linha)
                total_caminhos.update(linha["caminhos"])
                continue

            # app/ponto/janela

[truncado em 20000 de 25542 caracteres]

## 26/11 10:53 · sessao bce89212e720 · R1-SEM-DONO-2611 · censo · origem=Code

# R1-SEM-DONO-2611 · censo da competência 11

**Dono pelo E6: ESTRUTURA nas 18 chaves.** O mapa tira as 18 da lista ESTRUTURA de 26/11. A obra responsável pela cura é uma atribuição diferente. A fonte selecionada pelo E6 para o número do espelho, os insumos individuais do dono e o nexo de cada mecanismo com o delta de 26/11 estão **[nao verificado]**. [Anexo r.md §1 e §3, `depois_c11.txt:12–49`] [e6_oraculo.py:342–380](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/management/commands/e6_oraculo.py:342)

## Caminho dos dois números em cada linha

| Número | Sítio produtor e ponto de separação |
|---|---|
| **E6** | `InsumosDoDono` lê só os instantes locais de `batidas_apuraveis` e chama `minutos_do_oraculo(ts)` sem `pares` ou `envelopes`. O núcleo corta a sequência por gap ou vão, chaveia cada trecho pela data civil do primeiro instante e soma os pares por posição, em minutos. O comando lê `e6.get(k, 0.0)`. `app/ponto/management/commands/e6_oraculo.py:149–154,350` · `app/ponto/calculador/nucleo.py:109–110,186–225,235–254` |
| **Espelho, fonte prioritária** | `montar_dias` lê `turnos_do_colab` na janela visual e publica o resultado de `realizado_do_dia` em `minutos_realizados`. Aqui a sequência ganha papel da ata, marcos e vínculo do dia. O realizado agrupa por `Turno.data_turno`, valoriza a pausa e soma os turnos. Zero numérico nessa fonte tem prioridade. `app/ponto/services/espelho.py:267–294,625–628,689–690,801,1298–1302` · `app/ponto/turnos.py:719–771,1940–1944,1977–2002` · `app/ponto/management/commands/e6_oraculo.py:263–265` |
| **Espelho, quando o realizado é nulo** | `montar_dias` publica `pago_h` a partir do `DiaPago` de versão `motor`. O fechamento entrega períodos e mapa do dia a `lavrar`, que grava as rubricas. `pago_do_dia` soma trabalhadas, folga trabalhada e folga-feriado. O espelho arredonda em horas, e o E6 converte por ×60. Com as duas fontes nulas, o comando exclui a chave. Os períodos do motor corrente também alimentam os cartões e a lista Cadastro × Realidade. `app/ponto/services/fechamento.py:1224–1251` · `app/ponto/services/dia_pago.py:173–178,235–272,359–360,833–856,890–898` · `app/ponto/services/espelho.py:420–426,435–474,829–830,1330–1331` · `app/ponto/management/commands/e6_oraculo.py:162–172,263–269,342–349` |

A regra efetiva do dono roda nesta ordem: **CADASTRO** por folga indefinida, sem vínculo ou sem vínculo em dia de trabalho → **BATIDA** por turno aberto com entrada solta apurável ou contagem ímpar em `horas[k]` → **CADASTRO** por fase deslocada lavrada ou presença do dia na lista do motor → **ESTRUTURA** no resíduo. A contagem relevante é a atribuída pelo oráculo à chave, inclusive zero, e o marcador `IMPAR` do trecho é apurado à parte. Essa precedência explica o dono publicado. Os valores dos predicados por chave estão **[nao verificado]**. `app/ponto/management/commands/e6_oraculo.py:98–110,174–212,378–404` · `app/docs/LEIS.md:122`

## Dia → mecanismo → dono → obra

As letras remetem ao ponto de separação descrito abaixo, após os caminhos comuns acima. “Família registrada” indica o encaixe do achado disponível. A atribuição do **delta R1 de 26/11** à obra permanece **[nao verificado]** onde a tabela indica candidatura. [Anexo r.md §1 e §3] `app/ponto/management/commands/e6_oraculo.py:342–378`

| Dia de 2026 | Mecanismo ou evidência disponível | Dono R1 | Obra |
|---|---|---|---|
| **col112 21/10** | **A · Pareamento.** A leitura anterior traz `17:03,07:01` no E6 e `06:57→17:03` no pareador cru. Período efetivo usado no R1 **[nao verificado]**. `RELATO.md:13115–13117` · `depois_c11.txt:14–15` | ESTRUTURA | Família registrada em `EV-NA-BRECHA-1611` → `ORACULO-E6-43-DEFEITOS`, “pareamento posicional”. Nexo R1 **[nao verificado]**. `BACKLOG.md:823` · `GRADE-MESTRA.md:303` |
| **col112 22/10** | **A · Pareamento.** A leitura anterior traz `17:02,06:56` no E6 e `07:01→17:02` no pareador cru. Período efetivo usado no R1 **[nao verificado]**. `RELATO.md:13116–13117` · `depois_c11.txt:14–15` | ESTRUTURA | `EV-NA-BRECHA-1611` → `ORACULO-E6-43-DEFEITOS`, mesma família. Nexo R1 **[nao verificado]**. `BACKLOG.md:823` · `GRADE-MESTRA.md:303` |
| **col200 03/11** | **B · Chave da madrugada.** Há um par civil de 03/11 atribuído ao turno de 02/11 e uma colisão nominal às 02:00. Qual deles entra no `420 × 598–720` **[nao verificado]**. `RELATO.md:189,2360` · `depois_c11.txt:23` | ESTRUTURA | A chave do par tem escopo em `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` e na família de madrugada registrada em `EV-NA-BRECHA-1611`. Obra do delta de 03/11 **[nao verificado]**. `BACKLOG.md:823,1989,1991` · `GRADE-MESTRA.md:303` |
| **col382 11/11** | **B · Chave da madrugada.** O teste nominal dá o par `00:02→07:51` civil de 11/11 ao turno de 10/11. Sua contribuição para `437 × 854–949` **[nao verificado]**. `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:70–77` · `depois_c11.txt:36` | ESTRUTURA | Mesmas famílias registradas. `P0-[nome]-RELAVRA-10` inclui o colaborador, com efeito em 11/11 **[nao verificado]**. `BACKLOG.md:823,1989,1991,2303` · `GRADE-MESTRA.md:303` |
| **col114 08/11** | **C · Comparação de pausa.** Consta do recorte anterior de intervalo, que ficou incompleto. Par, piso e janela descontada **[nao verificado]**. `RELATO.md:13118–13119` · `depois_c11.txt:18–19` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. `RELATO.md:13118–13119` |
| **col252 18/11** | **C · Comparação de pausa.** O colaborador tem família documentada em outras datas. Pausa curta nesta chave **[nao verificado]**. `RELATO.md:2390,3096` · `depois_c11.txt:26–32` | ESTRUTURA | Candidata `EV-NA-BRECHA-1611` → `ORACULO-E6-43-DEFEITOS`, “pausa L-032”. Encaixe nesta data **[nao verificado]**. `BACKLOG.md:823` · `GRADE-MESTRA.md:303` |
| **col252 19/11** | **C · Comparação de pausa.** Par, piso e janela descontada nesta chave **[nao verificado]**. `RELATO.md:2390,3096` · `depois_c11.txt:26–32` | ESTRUTURA | Mesma candidata registrada. Encaixe nesta data **[nao verificado]**. `BACKLOG.md:823` · `GRADE-MESTRA.md:303` |
| **col884 08/11** | **C · Comparação de pausa.** Aparece no recorte de intervalo e no levantamento “sem nome”, entre as batidas fora de turno. Ramo que produziu o delta **[nao verificado]**. `RELATO.md:3097,13118–13119` · `depois_c11.txt:41` | ESTRUTURA | `EV-NA-BRECHA-1611` cobre a investigação “SEM NOME 12”. Obra da cura ou classe nova **[nao verificado]**. `BACKLOG.md:823` |
| **col922 01/11** | **D · Turnos somados e cartões exibidos.** Diagnóstico nominal anterior, registrado “a confirmar”. Nexo com o R1 **[nao verificado]**. `RELATO.md:2926,3097` · `depois_c11.txt:43–46` | ESTRUTURA | Candidata `O54`, etapa `E3-geometria`, investigada em `EV-NA-BRECHA-1611`, “funcao que soma x que desenha”. `BACKLOG.md:823,1469,2529` |
| **col922 04/11** | **D · Turnos somados e cartões exibidos.** Mesmo diagnóstico nominal. Turno ou período responsável pelo delta R1 **[nao verificado]**. `RELATO.md:2926,3097` · `depois_c11.txt:43–46` | ESTRUTURA | Mesma candidata `O54` / `E3-geometria`, via `EV-NA-BRECHA-1611`. Atribuição R1 **[nao verificado]**. `BACKLOG.md:823,1469,2529` |
| **col302 20/11** | **E · Origem do zero no E6.** Piso zero e espelho positivo. Trecho, chave e fonte do positivo **[nao verificado]**. `depois_c11.txt:34` · `app/ponto/management/commands/e6_oraculo.py:350–372` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. `MOTOR-LE-O-TURNO` registra outras chaves com esse sintoma. `BACKLOG.md:2234` · [Anexo r.md §1] |
| **col996 28/10** | **E · Origem do zero no espelho.** Zero selecionado pelo leitor e piso E6 positivo. Fonte do zero **[nao verificado]**. `depois_c11.txt:47` · `app/ponto/management/commands/e6_oraculo.py:263–269,370–372` | ESTRUTURA | `O54` / `E1-sem-previsao` acompanha o contador. Obra da cura ou classe nova **[nao verificado]**. `BACKLOG.md:1469` · `app/core/espelho_verdade.py:60–61` |
| **col114 15/11** | **F · Separação específica [nao verificado].** O recorte anterior de intervalo nomeia 08/11. `depois_c11.txt:18–19` · `RELATO.md:13118` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. [Anexo r.md §1] |
| **col130 29/10** | **F · Separação específica [nao verificado].** A leitura anterior deixou o caso fora do envelope. `RELATO.md:2926` · `depois_c11.txt:20` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. `RELATO.md:2926` |
| **col220 10/11** | **F · Separação específica [nao verificado].** A leitura anterior deixou o caso fora do envelope. `RELATO.md:2926` · `depois_c11.txt:25` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. `RELATO.md:2926` |
| **col874 22/10** | **F · Separação específica [nao verificado].** `depois_c11.txt:40` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. [Anexo r.md §1] |
| **col989 23/10** | **F · Separação específica [nao verificado].** `depois_c11.txt:43–46` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. [Anexo r.md §1] |
| **col996 10/11** | **F · Separação específica [nao verificado].** `depois_c11.txt:48–49` | ESTRUTURA | Obra ou classe nova **[nao verificado]**. [Anexo r.md §1] |

## A · Pareamento por posição e pareamento do turno

O primeiro ponto observado de separação em **col112 21/10 e 22/10** é a escolha das pontas. O produtor de `Veredito.minutos` soma posições consecutivas do trecho, e `e6_oraculo` lê o acumulado por dia. No realizado, `_mk` produz o `Turno` com entrada, saída e `data_turno`, e `realizado_dos_turnos` lê essas pontas, incluindo o último par fechado de um turno aberto quando existir. O motor monta seus períodos em `calcular_mes` e pode substituir os do dia pelas pontas julgadas pela ata. A leitura anterior de col112 usou `parear_turnos` **sem marcos/ata**. Os períodos do motor e os turnos do espelho na rodada de 26/11 estão **[nao verificado]**. `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/management/commands/e6_oraculo.py:350` · `app/ponto/turnos.py:312–324,609–622,719–741,1997–2000` · `app/ponto/motor_calculo_v2.py:632–645,669–688,2362–2370` · `app/docs/RELATO.md:13115–13117`

**Escritores e leitores.** `registrar_batida` grava as pontas de origem, e `retratar_batida` muda a participação no universo de apuração. O cartório lavra a ata, `papel_por_minuto_da_ata` a lê para o pareador, e os turnos alimentam o realizado e a guarda de batida solta do E6. Do lado do motor, os períodos seguem para os cartões e para `DiaPago`. `app/ponto/registro_batida.py:118–127,162–180,287–301` · `app/ponto/turnos.py:1867–1871,1940–1944,2940–2948` · `app/ponto/services/cartorio.py:1907–1909` · `app/ponto/management/commands/e6_oraculo.py:198–212` · `app/ponto/services/espelho.py:435–474` · `app/ponto/services/dia_pago.py:173–178`

**Leis e padrão.** L-002 manda ler a autoridade, L-030 guarda a origem da batida, L-115 define a saída posicional no caso de pausa fechada e L-114 dá ao E6 o papel de termômetro. O padrão para corrigir a família registrada é **Single Source of Truth / um juiz por pergunta**, com **Architecture Conformance Test** sobre o par e o dia produzidos. `EV-NA-BRECHA-1611` registra o instrumento `ORACULO-E6-43-DEFEITOS`, cuja frase inclui “pareamento posicional 11”. `app/docs/LEIS.md:60,78,153–154` · `app/docs/CORTES.md:455,461` · `app/docs/GLOSSARIO.md:11–15` · `app/docs/PLANO-PADROES.md:9,53–65` · `app/docs/BACKLOG.md:823` · `app/docs/GRADE-MESTRA.md:303`

## B · Dia civil do trecho, dia do turno e dia do período pago

A separação pode ser lida em três locais. O **E6** chaveia pelo primeiro instante civil do trecho. O **realizado** soma pelo `data_turno` produzido por `_data_do_turno`, que consulta as bordas da véspera. O **motor** tem um mapa do juiz para a janela de HE e um caminho de agregação que pode herdar `_prev_dj` após um período fechado com gap de até seis horas. O fechamento passa `resultado.dia_da_jornada` à lavra, e `dia_pago._dia_de` usa esse mapa quando há chave para o período. Cada mudança de chave precisa ser seguida em D e na véspera para fechar o delta. `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:285–324,719–721` · `app/ponto/motor_calculo_v2.py:936–951,1545–1588,2415–2429` · `app/ponto/services/fechamento.py:1241` · `app/ponto/services/dia_pago.py:126–132`

**Escritores e leitores.** `_mk` produz `Turno.data_turno`. O realizado e `dia_das_batidas` consomem essa chave. O motor monta e publica seu mapa de saída, lido pela lavra e pela testemunha da exportação. A colisão registrada de col200 tem outra origem, `colisoes_tipo_oposto` e a porta de gravação. Sua participação no par comparado em 03/11 está **[nao verificado]**. `app/ponto/turnos.py:312–324,719–721,2951–2982` · `app/ponto/motor_calculo_v2.py:1085–1092,2415–2429,2950–2978` · `app/ponto/services/fechamento.py:1241` · `app/folha/porta_export.py:101–121` · `app/ponto/registro_batida.py:94–127,162–180` · `app/docs/RELATO.md:2360`

**Leis, padrão e obras.** L-085 dá a cauda ao dia de início do plantão. L-097 e L-084 regem a janela e a avaliação das pontas cruas. O corte `NOTURNO-A-JANELA-NO-DIA-DO-TURNO-2411-0210` fixa a âncora da janela. O padrão é **Single Source of Truth / um juiz por pergunta** para o dia, com **CQRS com projeções reconstruíveis** para o valor já lavrado. `app/docs/LEIS.md:115–117,142` · `app/docs/CORTES.md:329,646` · `app/docs/GLOSSARIO.md:11–12,25–26` · `app/docs/PLANO-PADROES.md:11,28–34`

- `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` registra “janela de HE ancora no dia do turno”. `MOTOR-LE-O-TURNO-DIA-DO-PAR` registra a herança de 0–6 h na lavra/FT. A obra-mãe `MOTOR-LE-O-TURNO` manda que “motor e DiaPago leem par e dia SO de core/juizes.py::turnos_do_colab”. O defeito de atribuição da madrugada no **instrumento** já está na família registrada em `EV-NA-BRECHA-1611`. Qual desses caminhos explica os dois deltas da tabela está **[nao verificado]**. `app/docs/BACKLOG.md:823,1989,1991,2234` · `app/docs/GRADE-MESTRA.md:303`
- `P0-[nome]-RELAVRA-10` registra a relavra dos dez colaboradores, incluindo col200 e col382. `COMP11-RELAVRA-FROTA` registra a relavra da frota. A contribuição de cada ato para estas duas chaves está **[nao verificado]**. `app/docs/BACKLOG.md:2303–2304` · `app/ponto/services/fechamento.py:1215–1241`

## C · Pausa observada, piso e janela declarada

O E6 deixa o vão entre pares fora da soma. O realizado lê `intervalos_do_turno`, que só reconhece pausa marcada de pelo menos 15 min. Na ausência de pausa reconhecida, o ramo aplicável de intervalo fixo desconta a janela declarada que intercepta o turno. Esse ramo produz `janela_descontada`, que o espelho publica como `realizado_janela_descontada`. O E6 lê o carimbo e exclui a comparação quando `abs((E6 − espelho) − janela) <= 10`, com janela diferente de zero. A discriminação entre pausa abaixo do piso, par/turno diferente e ausência de carimbo nas quatro chaves está **[nao verificado]**. `app/ponto/calculador/nucleo.py:243–254` · `app/ponto/turnos.py:495–499,542–546,756–771` · `app/ponto/services/espelho.py:837` · `app/ponto/management/commands/e6_oraculo.py:215–239,350–358`

**Escritores e leitores.** A célula guarda a régua e a ata. As portas de célula e o cartório são os sítios de geração, regeneração e lavra pertinentes. O espelho lê `vinculo_do_dia` e `intervalo_do_dia`. As marcas de pausa vêm do pareamento, lidas por `intervalos_do_turno`, pelo realizado e pelo motor. O leitor direto do carimbo de desconto na comparação é `diferenca_e_a_janela_declarada`. A guarda de rajada e a impressão da ata têm obras próprias, com participação nos quatro deltas **[nao verificado]**. `app/ponto/portas/celula.py:112,899` · `app/ponto/services/cartorio.py:1907–1909` · `app/ponto/services/espelho.py:286–294,837` · `app/ponto/turnos.py:502–546,798–815` · `app/ponto/motor_calculo_v2.py:2563,3006` · `app/ponto/management/commands/e6_oraculo.py:236–239` · `app/docs/BACKLOG.md:2538,2558,2560`

**Leis e padrão.** L-032 rege a pré-assinalação e o intervalo batido dentro do envelope. O corte de 14/09 está transcrito no realizado, com regra para a janela fixa e para a pausa livre. O corte D1 vigente trata da guarda do casador. São pontos distintos do cálculo. O padrão da cura do instrumento é **Single Source of Truth / um juiz por pergunta**, com **Architecture Conformance Test** sobre qual pausa foi reconhecida e qual desconto foi publicado. A obra registrada cita “pausa L-032 12”. A leitura nominal anterior de col252 cita 09, 10, 11, 13 e 17/11. `app/docs/LEIS.md:60,80,136` · `app/docs/CORTES.md:601` · `app/ponto/turnos.py:707–717` · `app/docs/GLOSSARIO.md:11–15` · `app/docs/PLANO-PADROES.md:9,61–65` · `app/docs/BACKLOG.md:823` · `app/docs/GRADE-MESTRA.md:303` · `app/docs/RELATO.md:2390,3096`

## D · Soma do realizado e desenho dos cartões

Em **col922 01/11 e 04/11**, o diagnóstico nominal anterior é “realizado soma turno que a tela nao exibe”, a confirmar. A separação no código está entre `turnos_do_colab → realizado_do_dia → minutos_realizados` e `resultado.periodos + periodos_ft → índice por entrada → instante → PK → dia → dia.turnos`. Período sem dia nessa ponte fica fora de `dia.turnos`. O template e os leitores do papel consomem a lista. O E6 consome o número selecionado no leitor, sem somar os cartões. Assim, a diferença entre soma e desenho, mesmo quando reproduzida, requer uma segunda comparação para demonstrar o delta com o E6. `app/docs/RELATO.md:2926,3097` · `app/ponto/services/espelho.py:435–474,625–628,670,689–690,778–801` · `app/templates/ponto/espelho.html:194–203` · `app/relatorios/pdf_espelho.py:902,965` · `app/ponto/management/commands/e6_oraculo.py:263–269`

**Escritores, leitores e leis.** O motor produz `periodos` e `periodos_ft`. `montar_dias` produz os campos `turnos` e `minutos_realizados` do espelho. O realizado tem ainda leitores no estado do dia e na soma `total_relogio`. L-002 rege a fonte comum dos leitores, `CARTAO-E-ESPELHO` manda o PDF desenhar o mesmo `dias`, e L-095 rege o valor pago. O padrão é **CQRS com projeções reconstruíveis**, com **Single Source of Truth** para a geometria usada por cada projeção. `O54` registra o programa. `EV-NA-BRECHA-1611` registra a leitura, e `PRIORIDADE-ESPELHO-VERDADE-E-TRES-CORTES` mantém `E3-geometria` “a confirmar” para col922. O item histórico `E3-geometria` registra a entrega `O65`. `app/ponto/motor_calculo_v2.py:2508–2510` · `app/ponto/services/espelho.py:778–801,930–932` · `app/ponto/status_dia.py:190` · `app/docs/LEIS.md:60,113,142` · `app/docs/CORTES.md:341` · `app/docs/GLOSSARIO.md:11,25` · `app/docs/PLANO-PADROES.md:11,28–34` · `app/docs/BACKLOG.md:823,1469,1481,2529` · `app/core/espelho_verdade.py:76–78`

## E · Proveniência dos dois zeros

- **col302 20/11.** A classe publicada exige piso E6 exatamente zero e espelho positivo. O sítio do zero é `e6.get(k, 0.0)` sobre a soma posicional. O zero pode vir da chave ausente ou de uma soma sem minutos positivos. O produtor do positivo, a chave dos trechos vizinhos e eventual correspondência com os casos de `MOTOR-LE-O-TURNO` estão **[nao verificado]**. `depois_c11.txt:34` · `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/management/commands/e6_oraculo.py:350–372` · `app/docs/BACKLOG.md:2234`
- **col996 28/10.** O sítio de seleção do zero é `minutos_do_espelho`. A primeira origem possível é o `RealizadoDoDia.minutos` numérico, por exemplo quando há turno do dia e a soma fecha em zero. Sem turno do dia, o realizado devolve `None`, e o leitor passa ao `pago_h`. Para esse segundo caminho participar do R1, o pago precisa ser numérico. Linha de `DiaPago` ausente ou com todas as rubricas de dinheiro zeradas devolve `None`. A origem efetiva do zero está **[nao verificado]**. `depois_c11.txt:47` · `app/ponto/turnos.py:719–721,733–771` · `app/ponto/services/dia_pago.py:890–898` · `app/ponto/management/commands/e6_oraculo.py:263–269,342–347`

**Escritores, leitores, leis e padrão.** O censo dos campos é o dos caminhos comuns. Em particular, `DiaPago.lavrar` é a porta de escrita das rubricas de trabalho, `linhas_do_periodo` e `pago_do_dia` as leem para o espelho, e PDF e relatóri

[truncado em 20000 de 28032 caracteres]

## 26/11 10:47 · sessao 3af52882fa76 · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

Implementei a F3 na cópia `a4d3675c7a187493c006eb291ee40580a3ea7a77`. Os cinco REDs falharam na base por asserção. Na cura, os oito testes em memória passaram, incluindo troca de vínculo, retratação e mudança de competência. Os três mutantes voltaram a falhar por asserção. A certificação fica com o Code e o opus final. `ponto/tests/test_juiz_unico_eventos.py:61`

## Cura

- `turnos_do_colab`, `dia_das_batidas` e `CargaDaJanela` usam o carregador histórico da F2. O mapa inclui as PKs de cada turno apresentado, inclusive a saída de 25/10 quando a janela termina em 23/10. Se dois segmentos reivindicam a mesma PK em dias diferentes, vale a precedência que o mapa F2 já aplicava. A publicação de turnos respeita essa atribuição. `ponto/turnos.py:1850` `ponto/turnos.py:2891` `ponto/turnos.py:2941` `ponto/turnos.py:3007` `ponto/turnos.py:3119`
- O juiz compara a geometria antes e depois do evento. O núcleo reaproveita os turnos julgados, e o cartório e o fechamento reaproveitam os dias afetados pelo `save`. A exclusão reintroduz a marca em memória para julgar a versão anterior. O fechamento usa o corte da empresa e chama o recálculo uma vez por competência afetada, após o commit. `ponto/turnos.py:3025` `ponto/signals.py:20` `ponto/signals.py:30` `ponto/signals.py:124` `ponto/nucleo.py:40` `ponto/registro_batida.py:195` `ponto/registro_batida.py:303` `ponto/services/fechamento.py:1831`
- A contagem AST ficou em **zero `except` novos**, incluindo testes. No chokepoint, o diff se limita às chamadas e à preparação do agendamento. Os selos permitidos foram reancorados na carga histórica. `ponto/registro_batida.py:174` `ponto/registro_batida.py:195` `ponto/registro_batida.py:303` `ponto/tests/test_contract_no_batida_date.py:447` `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:178` `ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:195` `chamados/tests/test_e3_c8b_sem_banco.py:522` `chamados/tests/test_e3_c8c_sem_banco.py:104`

## RED → GREEN

| Caso | Saída na base | Resultado na cura |
|---|---|---|
| R4 · par de 24h10 | `Items in the second set but not the first: datetime.date(2026, 10, 23)` | A saída aparece no turno de 23/10 em `[23/10, 23/10]` e na competência 10. O mapa e o lote concordam. `ponto/tests/test_juiz_unico_eventos.py:137` |
| R4 · saídas órfãs | `datetime.date(2026, 10, 24) != datetime.date(2026, 10, 25) : PK 6` | A PK 6 recebe 25/10 em `[25/10, 25/10]`, `[24/10, 26/10]` e na competência 11. `ponto/tests/test_juiz_unico_eventos.py:156` |
| R5a · `post_save` | Cartório chamou `[24/10, 25/10]`. O núcleo iniciou em `23/10`, com âncora civil. | O cartório julga 23/10 e o núcleo ancora a recomputação na jornada de 23/10. `ponto/tests/test_juiz_unico_eventos.py:172` |
| R5a · `post_delete` | As mesmas duas asserções falharam após remover a saída. | O julgamento anterior à exclusão recupera a jornada de 23/10. `ponto/tests/test_juiz_unico_eventos.py:178` |
| R5b · corte em 24/10 | `Expected: recalcular_fechamento_mes(10, 2026, ...)` / `Actual: recalcular_fechamento_mes(11, 2026, ...)` | Agenda 10/2026. O caso adicional com mudança de dono agenda 10 e 11, e a retratação agenda a competência anterior. `ponto/tests/test_juiz_unico_eventos.py:196` `ponto/tests/test_juiz_unico_eventos.py:202` `ponto/tests/test_juiz_unico_eventos.py:211` |

O comando da cura retornou `Ran 8 tests` e `OK`. Os dublês cobrem ORM, despacho, gravação do núcleo e execução agendada do fechamento. `ponto/tests/test_juiz_unico_eventos.py:61`

```sh
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest -v ponto.tests.test_juiz_unico_eventos
```

## Mutantes

`f3 base` retornou `rc=1`, com 10 falhas nos cinco casos. `f3 recorte` retornou `rc=1`, com cinco falhas de R4. `f3 signal` retornou `rc=1`, com quatro falhas de R5a. `f3 evento` retornou `rc=1`, com duas falhas de R5b. O comando aplica cada substituição somente em memória. `ponto/tests/test_juiz_unico_eventos.py:153` `ponto/tests/test_juiz_unico_eventos.py:172` `ponto/tests/test_juiz_unico_eventos.py:196`

```sh
f3() {
  F3_MODO="$1" PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import ast
import os
import subprocess
import unittest
from importlib.machinery import SourceFileLoader
from pathlib import Path
from unittest.mock import patch

modo = os.environ["F3_MODO"]
raiz = Path.cwd()
base_hash = "a4d3675c7a187493c006eb291ee40580a3ea7a77"
turnos = "ponto/turnos.py"
signal = "ponto/signals.py"
registro = "ponto/registro_batida.py"
fechamento = "ponto/services/fechamento.py"

def base(arquivo):
    return subprocess.check_output(["git", "show", base_hash + ":app/" + arquivo], text=True)

def trocar(fonte, antes, depois):
    assert fonte.count(antes) == 1, antes
    return fonte.replace(antes, depois, 1)

def casca(fonte):
    no = next(n for n in ast.parse(fonte).body if getattr(n, "name", "") == "turnos_do_colab")
    return ast.get_source_segment(fonte, no)

r4 = ["r4_par_24h10", "r4_saidas_orfas"]
r5a = ["r5a_save", "r5a_delete"]
if modo == "base":
    fontes = {f: base(f) for f in (turnos, signal, registro, fechamento)}
    casos = r4 + r5a + ["r5b_evento"]
elif modo == "recorte":
    s = Path(turnos).read_text()
    fontes = {turnos: trocar(s, casca(s), casca(base(turnos)))}
    casos = r4
elif modo == "signal":
    s = Path(signal).read_text()
    s = trocar(s, "        if dias:\n            recompute_turnos",
               "        from ponto.turnos import data_local\n"
               "        dias = (data_local(batida.timestamp),)\n"
               "        if dias:\n            recompute_turnos")
    s = trocar(s, "    ontem = datetime.timedelta(days=1)",
               "    from ponto.turnos import data_local\n"
               "    dias = (data_local(batida.timestamp),)\n"
               "    ontem = datetime.timedelta(days=1)")
    fontes, casos = {signal: s}, r5a
else:
    assert modo == "evento"
    s = Path(registro).read_text()
    fontes = {registro: trocar(s, "dias_da_jornada=dias_do_salvamento(batida, criada=criada)",
                               "dias_da_jornada=None")}
    casos = ["r5b_evento", "r5b_mudanca_do_dono_agenda_duas_competencias"]

fontes = {raiz / f: s.encode() for f, s in fontes.items()}
ler_real = SourceFileLoader.get_data

def ler(loader, caminho):
    p = Path(caminho)
    return fontes[p] if p in fontes else ler_real(loader, caminho)

prefixo = "ponto.tests.test_juiz_unico_eventos.JuizUnicoEventosTest.test_"
with patch.object(SourceFileLoader, "get_data", ler):
    suite = unittest.defaultTestLoader.loadTestsFromNames([prefixo + c for c in casos]) [nao verificado]
    raise SystemExit(not unittest.TextTestRunner(verbosity=2).run(suite).wasSuccessful()) [nao verificado]
PY
}

f3 base
f3 recorte
f3 signal
f3 evento
```

## Custo e catraca

- **Por chamada singular**, a estimativa estática é **4–5 consultas** com marcas na janela e **1** quando o pad está vazio. A leitura de marcas percorre o histórico apurável. Em lote, turnos e mapa dividem **3–4 consultas históricas** e reutilizam o vínculo da ata já lido. Chamadas singulares independentes abrem cargas independentes. `ponto/turnos.py:2953` `ponto/turnos.py:2987` `ponto/turnos.py:3070` `ponto/turnos.py:3119` `escala/alimentacao.py:148`
- **Por `save`**, o caminho normal dos receivers faz uma carga histórica compartilhada. Criação, retratação e edição de insumo podem executar dois pareamentos sobre essa carga. Edição de insumo em PK existente, ou `save` completo dessa PK, acrescenta **1 consulta** para ler a versão anterior. O núcleo reaproveita a geometria. Cada dia adicional julgado pode aumentar o custo do cartório. SQL efetivo, latência e teto do chokepoint ficam **[nao verificado]**. `ponto/signals.py:20` `ponto/signals.py:30` `ponto/signals.py:124` `ponto/turnos.py:3025` `ponto/nucleo.py:40` `ponto/tests/test_selo_chokepoint_escrita.py:343`
- **Catraca protegida.** O censo passou de **81 → 78 derivações**, com **9 pareadores**. B1 passou de **78 → 75**. `_recompute_around:23` e `_dias_batida:111` saem. `_carregar_dias_das_batidas:3026 → 2979` entra na composição do juiz e sai de B1. `dia_do_turno_da_batida:2989 → 2929` e `par_completo:2217 → 2156` continuam em B1. A composição ganha `_carregar_dias_das_batidas`, `_dia_das_batidas_puro`, `batidas_apuraveis` e `marcos_dna_periodo`, e perde `escalas_do_periodo`. O registro congelado ainda acusa o delta. `core/tests/test_medida_turno_do_par.py:108` `core/tests/test_medida_turno_do_par.py:110` `core/tests/test_medida_turno_do_par.py:336` `core/tests/test_medida_turno_do_par.py:508` `ponto/turnos.py:2979`

## Impacto nos 51 sítios

Há mudança observada na resposta da autoridade que esses sítios consultam. O espelho usa essa resposta para as datas de saídas órfãs, a API usa para agrupar turnos por dia, e o núcleo usa para materializar `data_turno`. A mudança efetiva de cada saída consumidora nas competências 10 e 11 está **[nao verificado]**. `ponto/tests/test_juiz_unico_eventos.py:137` `ponto/services/espelho.py:628` `api/views.py:1447` `ponto/nucleo.py:47`

A varredura encontrou 50 chamadas diretas ou por alias, além do registro `_T1`. Sítios expostos à mudança de resposta:

- **API e autoridade** — `api/views.py:1447`, `core/juizes.py:216`.
- **Chamados** — `chamados/detector_anomalias.py:157`, `chamados/juizes.py:447,2303`, `chamados/models.py:1378`, `chamados/management/commands/migrar_data_evento_dia_do_turno.py:145`, `chamados/services/disputa_emissao.py:722`, `chamados/services/materializacao.py:399,513`.
- **Colaboradores e escala** — `colaboradores/services/drawer.py:47`, `colaboradores/services/feriado_do_posto.py:473`, `escala/detector_proposta.py:65`, `escala/regua_defesa.py:361`, `escala/services/escala_auto.py:87`, `escala/services/impacto_troca.py:18`, `escala/servico_jornada.py:249,403`, `escala/utils.py:1627,1952`.
- **Ponto** — `ponto/nucleo.py:48`, `ponto/turno_leitura.py:89,95`, `ponto/turnos.py:780,2043,2101,2131,3185`, `ponto/views.py:396,1613,3356`, `ponto/services/bordas_realizado.py:66`, `ponto/services/esmeril_espelho.py:102`, `ponto/services/espelho.py:628`, `ponto/services/passivo_s84.py:82`, `ponto/services/regularizacao.py:61`, `ponto/services/teto_na_lista.py:68`, `ponto/services/triagem_batida.py:553`, `ponto/services/vigia_pareador_celula.py:97`.
- **Comandos e relatórios** — `ponto/management/commands/censo_fase_12x36.py:180`, `ponto/management/commands/detectar_ausencias.py:337`, `ponto/management/commands/detectar_par_relampago.py:101`, `ponto/management/commands/dna_x_batida_real.py:69`, `ponto/management/commands/e6_oraculo.py:159`, `ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `ponto/management/commands/marcar_abandono_provisorio.py:46`, `ponto/management/commands/verificar_nucleo.py:29`, `ponto/management/commands/vigia_plantio_overnight.py:38`, `relatorios/management/commands/vinculo_do_dia_divergentes.py:124`, `relatorios/pdf_espelho.py:213,451`.

## Sonda de sombra para o Code

Executar o texto no shell Django do envelope de sombra com `PYTHONDONTWRITEBYTECODE=1`. A sonda chama as duas versões reais de `turnos_do_colab`, usa o corte de cada empresa e captura em memória o lote que `recompute_turnos` entregaria ao ORM. A comparação inclui as linhas já gravadas na sombra e usa multiplicidade. A sintaxe foi conferida. Resultado da execução na sombra **[nao verificado]**. `ponto/janelas.py:55` `ponto/turnos.py:1850` `ponto/nucleo.py:40` `ponto/models.py:1208`

```python
from collections import Counter, defaultdict
import json
import subprocess
from unittest.mock import patch

from colaboradores.models import Colaborador
from ponto import nucleo, turnos
from ponto.janelas import janela_fechamento
from ponto.models import TurnoMaterializado

hash_base = "a4d3675c7a187493c006eb291ee40580a3ea7a77"
fonte = subprocess.check_output(["git", "show", hash_base + ":app/ponto/turnos.py"])
base = {"__name__": "ponto.turnos_base_f3", "__package__": "ponto"}
exec(compile(fonte, "ponto/turnos.py@" + hash_base, "exec"), base)
campos = ("data_turno", "entrada_id", "saida_id", "aberto",
          "cross_meianoite", "dt_fim_previsto")

def foto(juiz, colab, ini, fim):
    pares = juiz(colab, ini, fim)
    dias = defaultdict(set)
    for turno in pares:
        for batida in turno.batidas:
            dias[batida.pk].add(turno.data_turno)
    rows = []
    with patch.object(nucleo, "_apagar_recorte_devolvendo_abertos", return_value=set()), \
         patch.object(TurnoMaterializado.objects, "bulk_create", side_effect=rows.extend):
        nucleo.recompute_turnos(colab, ini, fim, turnos=pares)
    return dias, Counter(tuple(getattr(row, f) for f in campos) for row in rows)

def emitir(**linha):
    print(json.dumps(linha, default=str, sort_keys=True))

def contagem(linhas):
    return [{"dados": dados, "n": n} for dados, n in sorted(linhas.items(), key=str)]

totais = {m: Counter() for m in (10, 11)}
for c in Colaborador.objects.select_related("empresa").order_by("pk").iterator(chunk_size=200):
    fotos, dias_base, dias_cura = {}, defaultdict(set), defaultdict(set)
    for m in totais:
        ini, fim = janela_fechamento(m, 2026, c.empresa)
        pk_base, mat_base = foto(base["turnos_do_colab"], c, ini, fim)
        pk_cura, mat_cura = foto(turnos.turnos_do_colab, c, ini, fim)
        for pk in pk_base:
            dias_base[pk].update(pk_base[pk])
        for pk in pk_cura:
            dias_cura[pk].update(pk_cura[pk])
        fotos[m] = ini, fim, pk_base, pk_cura, mat_base, mat_cura

    for m, (ini, fim, pk_base, pk_cura, mat_base, mat_cura) in fotos.items():
        for pk in sorted(pk_base.keys() | pk_cura.keys()):
            if dias_base[pk] != dias_cura[pk]:
                totais[m]["pks"] += 1
                emitir(comp=m, colab=c.pk, pk=pk, base=sorted(dias_base[pk]),
                       cura=sorted(dias_cura[pk]))
        gravado = Counter(TurnoMaterializado.objects.filter(
            colaborador=c, data_turno__range=(ini, fim)).values_list(*campos))
        if mat_base != mat_cura or gravado != mat_cura:
            saiu, entrou = mat_base - mat_cura, mat_cura - mat_base
            totais[m].update(saidas_base_cura=sum(saiu.values()),
                             entradas_base_cura=sum(entrou.values()))
            emitir(comp=m, colab=c.pk, tipo="materializacao", sai=contagem(saiu),
                   entra=contagem(entrou), sai_do_gravado=contagem(gravado - mat_cura),
                   entra_no_gravado=contagem(mat_cura - gravado))

for m, total in totais.items():
    emitir(comp=m, total=dict(total))
```

**Fora da F3.** O C6 de chamados ainda chama o corpo puro com seu recorte próprio. O mapa interno do motor mensal segue em sua porta. A suíte Django com banco, os valores lavrados em produção e o comportamento sob concorrência estão **[nao verificado]**. `chamados/services/carga_pergunta_viva.py:165` `ponto/motor_calculo_v2.py:867` `ponto/services/fechamento.py:1241`

## 26/11 10:46 · sessao 78751ae7cf11 · ORDEM-PELO-R1 · leitura · origem=Code

**`MOTOR-LE-O-TURNO` tem o maior escopo nominal, com 12 dos 38 dias ESTRUTURA da comp. 11.** O mapa fecha em **19 chaves com uma obra identificada, 1 chave com duas candidatas e 18 SEM DONO**. Quantos dias cada obra efetivamente zera no R1 de 26/11 está **[nao verificado]**. O E6 pode deixar de listar uma chave por acordo de minutos, mudança de dono ou saída da comparação. `depois_c11.txt:12–49` [BACKLOG.md:2234](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:2234) [e6_oraculo.py:331](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/management/commands/e6_oraculo.py:331)

## 1. Dia → obra e prova

Os caminhos `depois_c11.txt` são as referências do PUBLICADO fornecido. A fonte escolhida pelo E6 para o valor do espelho em cada chave está **[nao verificado]**. O comando prioriza `minutos_realizados`, usa `pago_h` quando o primeiro campo é nulo e publica a classe a partir dos valores comparados. `app/ponto/management/commands/e6_oraculo.py:263–269,342–372,401–404,450–451`

**Provas de código usadas na tabela**

- **Chave do turno.** O BACKLOG enumera “11 dia-colab em que oraculo E pareador dao [] e o espelho tem 103..418 min” e registra separadamente “col841 2026-11-16”. O E6 chama `minutos_do_oraculo(ts)` sem pares e agrupa pela data civil do primeiro instante do trecho. O espelho carrega `turnos_do_colab` e soma pela `data_turno`. A leitura r4 localizou esses caminhos e deixou a causa individual dos 11 **[nao verificado]**. `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:2198,2203,13103–13114` `app/ponto/management/commands/e6_oraculo.py:149–154,350` `app/ponto/calculador/nucleo.py:235–247` `app/ponto/services/espelho.py:625–628,689–690,801` `app/ponto/turnos.py:719–721,733–771`
- **Pausa curta.** A linha **`EV-NA-BRECHA-1611`** registra **`ORACULO-E6-43-DEFEITOS`** na GRADE. O RELATO nomeia “pausa abaixo do piso com intervalo pre-assinalado L-032” para col252 e dá as cinco datas. O E6 soma os pares por posição e deixa o vão entre pares fora da soma. O realizado do espelho exige 15 minutos para reconhecer a pausa batida e, na ausência de pausa reconhecida, desconta a janela fixa declarada quando ela se aplica. O E6 ainda tem uma guarda para excluir a diferença explicada pela janela declarada. O valor dessa guarda por chave na rodada de 26/11 está **[nao verificado]**. `app/docs/BACKLOG.md:823` `app/docs/GRADE-MESTRA.md:303` `app/docs/RELATO.md:2355,2390,3096` `app/ponto/calculador/nucleo.py:243–253` `app/ponto/turnos.py:499,542–546,756–771` `app/ponto/management/commands/e6_oraculo.py:236–239,350–358`
- **Janela de HE.** A linha **`NOTURNO-A-JANELA-NO-DIA-DO-TURNO`** diz “janela de HE ancora no dia do turno”. O motor ancora os marcos nesse dia e aplica a L-084 às pontas cruas. O RELATO documenta os deltas dos pares de col922 e col1010. O efeito sobre a fonte do valor ou a transição de dono no R1 está **[nao verificado]**. `app/docs/BACKLOG.md:1989` `app/docs/RELATO.md:205` `app/ponto/motor_calculo_v2.py:1543–1588` `app/ponto/janela_he.py:41–61` `app/ponto/management/commands/e6_oraculo.py:162–172,182–195,263–269`

| Dia de 2026 | Classe do PUBLICADO | Obra | Prova | [nao verificado]
|---|---|---|---|
| col37 14/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. Caso nominal. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13114` |
| col110 12/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. Caso nominal. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13111` |
| col113 30/10 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também tem teste nominal da janela do par na madrugada de 31/10. O efeito desse teste sobre o zero do E6 está **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13112` `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:61–69` |
| col113 01/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também registra inversão da janela em medição de DiaPago. O vínculo dessa medição com o zero do E6 está **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13113,13200–13207` |
| col200 29/10 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. O mapa de DiaPago também registra folga trabalhada gravada em D+1. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13103,13204–13207` `app/ponto/motor_calculo_v2.py:2416–2424` |
| col200 02/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também registra inversão da janela em medição de DiaPago. Efeito sobre o zero do E6 **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13104,13200–13207` |
| col218 14/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também registra inversão da janela em medição de DiaPago. Efeito sobre o zero do E6 **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13105,13200–13207` |
| col296 02/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também registra inversão da janela em medição de DiaPago. Efeito sobre o zero do E6 **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13106,13200–13207` |
| col302 20/11 | `e6_zero_esp_trabalho` | **SEM DONO** | Classe publicada. Causa e obra por chave **[nao verificado]**. `depois_c11.txt:34` `app/ponto/management/commands/e6_oraculo.py:350–372` |
| col382 10/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A NOTURNO-A também tem teste nominal da janela do par civil de 11/11, atribuído a 10/11. Efeito sobre o zero do E6 **[nao verificado]**. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234,1989` `app/docs/RELATO.md:13107` `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:70–77` |
| col516 23/10 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. A sonda anterior registra saída órfã às 07:01. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13108` |
| col727 10/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Chave do turno. O mapa de DiaPago também registra folga trabalhada gravada em D+1. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:13109,13204–13207` `app/ponto/motor_calculo_v2.py:2416–2424` |
| col841 16/11 | `e6_zero_esp_trabalho` | `MOTOR-LE-O-TURNO` | Caso nominal da chave do dia. O pareador cru deu BATIDA na sonda v3. A v3b com `turnos_do_colab` manteve ESTRUTURA. `depois_c11.txt:12–39` `app/docs/BACKLOG.md:2234` `app/docs/RELATO.md:2157,2203,13110` `app/ponto/management/commands/e6_oraculo.py:98–110,182–195` |
| col996 28/10 | `esp_zero_e6_trabalho` | **SEM DONO** | `O54`, etapa `E1-sem-previsao`, acompanha o contador. A causa do zero nesta chave está **[nao verificado]**. `depois_c11.txt:47` `app/docs/BACKLOG.md:1469,1303` `app/core/espelho_verdade.py:60–61` `app/ponto/management/commands/e6_oraculo.py:263–269,370–372` |
| col114 08/11 | `diverge_10_60` | **SEM DONO** | Citado entre os dias de intervalo na sonda anterior. Mecanismo específico e obra **[nao verificado]**. `depois_c11.txt:18–19` `app/docs/RELATO.md:13118–13119` |
| col114 15/11 | `diverge_10_60` | **SEM DONO** | Classe publicada. Causa e obra **[nao verificado]**. `depois_c11.txt:18–19` `app/ponto/management/commands/e6_oraculo.py:362–372` |
| col252 09/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 10/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 11/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 13/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 17/11 | `diverge_10_60` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Pausa curta nominal. `depois_c11.txt:26–32` `app/docs/BACKLOG.md:823` `app/docs/RELATO.md:2355,2390,3096` |
| col252 18/11 | `diverge_10_60` | **SEM DONO** | Pausa curta nesta data **[nao verificado]**. `depois_c11.txt:26–32` `app/docs/RELATO.md:2390,3096` |
| col252 19/11 | `diverge_10_60` | **SEM DONO** | Pausa curta nesta data **[nao verificado]**. `depois_c11.txt:26–32` `app/docs/RELATO.md:2390,3096` |
| col884 08/11 | `diverge_10_60` | **SEM DONO** | A chave aparece em “sem nome” e no recorte de intervalo. Causa e obra **[nao verificado]**. `depois_c11.txt:41` `app/docs/RELATO.md:3097,13118–13119` |
| col112 21/10 | `diverge_acima_60` | **SEM DONO** | A sonda registra pares diferentes entre E6 e pareador cru na chave. A inclusão nominal na família de madrugada do oráculo está **[nao verificado]**. `depois_c11.txt:14–15` `app/docs/RELATO.md:13115–13117` `app/docs/GRADE-MESTRA.md:303` `app/ponto/calculador/nucleo.py:235–247` |
| col112 22/10 | `diverge_acima_60` | **SEM DONO** | A sonda registra pares diferentes entre E6 e pareador cru na chave. A inclusão nominal na família de madrugada do oráculo está **[nao verificado]**. `depois_c11.txt:14–15` `app/docs/RELATO.md:13115–13117` `app/docs/GRADE-MESTRA.md:303` `app/ponto/calculador/nucleo.py:235–247` |
| col130 29/10 | `diverge_acima_60` | **SEM DONO** | A leitura anterior deixou col130 fora do envelope. Causa e obra **[nao verificado]**. `depois_c11.txt:20` `app/docs/RELATO.md:2926` |
| col200 03/11 | `diverge_acima_60` | **SEM DONO** | A NOTURNO-A documenta um par civil de 03/11 cujo turno é de 02/11. Há também uma colisão nominal de batidas às 02:00. O nexo de qualquer um dos achados, ou da relavra, com o delta de 03/11 está **[nao verificado]**. `depois_c11.txt:23` `app/docs/RELATO.md:189,2360` `app/docs/BACKLOG.md:2304` `app/ponto/motor_calculo_v2.py:936–951` `app/ponto/registro_batida.py:118–127` |
| col220 10/11 | `diverge_acima_60` | **SEM DONO** | A leitura anterior deixou col220 fora do envelope. Causa e obra **[nao verificado]**. `depois_c11.txt:25` `app/docs/RELATO.md:2926` |
| col382 11/11 | `diverge_acima_60` | **SEM DONO** | O teste da NOTURNO-A atribui o par civil de 11/11 ao turno de 10/11. `P0-[nome]-RELAVRA-10` cita o colaborador. Efeito sobre a chave R1 de 11/11 **[nao verificado]**. `depois_c11.txt:36` `app/docs/BACKLOG.md:2303–2304` `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:70–77` |
| col874 22/10 | `diverge_acima_60` | **SEM DONO** | Classe publicada. Causa e obra **[nao verificado]**. `depois_c11.txt:40` `app/ponto/management/commands/e6_oraculo.py:362–372` |
| col922 29/10 | `diverge_acima_60` | **Duas candidatas** `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` e `MOTOR-LE-O-TURNO` | **NOTURNO-A** cobre o par cru liberado pela L-084, `+174,6` min no motor frente a `−175` no R1. **MOTOR-LE-O-TURNO** cobre o mapa de dia, com 29/10 no juiz e 27/10 no resultado do motor para o par documentado. A contribuição de cada caminho ao delta R1 está **[nao verificado]**. `depois_c11.txt:43` `app/docs/BACKLOG.md:1989,1996,2234` `app/docs/RELATO.md:194,205,290` `app/ponto/motor_calculo_v2.py:1543–1588,2416–2424` `app/ponto/services/dia_pago.py:126–132` |
| col922 01/11 | `diverge_acima_60` | **SEM DONO**. Candidata `O54`, etapa `E3-geometria`, investigada em `EV-NA-BRECHA-1611` | “realizado soma turno que a tela nao exibe”, registrado **a confirmar**. O código soma turnos do juiz e monta os cartões por outro caminho, a partir dos períodos do motor. Nexo com o delta R1 **[nao verificado]**. `depois_c11.txt:43–46` `app/docs/BACKLOG.md:823,1469,2529` `app/docs/RELATO.md:2926,3097` `app/ponto/services/espelho.py:435–436,464–474,689–690,801` `app/templates/ponto/espelho.html:194` |
| col922 04/11 | `diverge_acima_60` | **SEM DONO**. Candidata `O54`, etapa `E3-geometria`, investigada em `EV-NA-BRECHA-1611` | Mesmo diagnóstico nominal de soma e exibição, **a confirmar**. Nexo com o delta R1 **[nao verificado]**. `depois_c11.txt:43–46` `app/docs/BACKLOG.md:823,1469,2529` `app/docs/RELATO.md:2926,3097` `app/ponto/services/espelho.py:435–436,464–474,689–690,801` `app/templates/ponto/espelho.html:194` |
| col989 23/10 | `diverge_acima_60` | **SEM DONO** | Classe publicada. Causa e obra **[nao verificado]**. `depois_c11.txt:43–46` `app/ponto/management/commands/e6_oraculo.py:362–372` |
| col996 10/11 | `diverge_acima_60` | **SEM DONO** | Classe publicada. Causa e obra **[nao verificado]**. `depois_c11.txt:48–49` `app/ponto/management/commands/e6_oraculo.py:362–372` |
| col1010 05/11 | `diverge_acima_60` | `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` | Janela de HE. O teste nominal põe a saída prevista em D+1 e conta cru o par da madrugada. O DIFF mede `+294,8` min no motor e o R1 mostra `−295`. `depois_c11.txt:49` `app/docs/BACKLOG.md:1989` `app/docs/RELATO.md:205` `app/ponto/janela_he.py:41–61` `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:93–107` |
| col909 11/11 | `diverge_acima_60/FORA` | `ORACULO-E6-43-DEFEITOS`, registrado em `EV-NA-BRECHA-1611` | Família “corte por vao”. O anexo traz `CORTE_do_oraculo` e `−586` min. O núcleo marca o corte que deixa um trecho ímpar e soma só os pares fechados. A chave também consta na lista BATIDA de `HJ-092`. O nexo desse segundo registro com o delta, e o delta após corrigir o corte, estão **[nao verificado]**. `depois_c11.txt:42` `app/docs/BACKLOG.md:797,823` `app/docs/GRADE-MESTRA.md:303` `app/docs/RELATO.md:3936` `app/ponto/calculador/nucleo.py:215–225,236–247` `app/ponto/management/commands/e6_oraculo.py:393–404` |

Nos cinco dias de col252, `[nome]-NAO-DESFAZ-O-PAPEL` e `[nome]-NA-IMPRESSAO` também aparecem no registro da **guarda da rajada e do marco vazio na ata**. O RELATO deixa o resíduo D1 em pergunta de desenho. O efeito dessas obras no delta do E6 está **[nao verificado]**. `app/docs/BACKLOG.md:2538,2558,2560` `app/docs/RELATO.md:2355,2390`

## 2. Por obra, estado e pré-requisitos

| ID exato | Dias ESTRUTURA no escopo e dias que zera | Estado e pré-requisitos registrados |
|---|---|---|
| `MOTOR-LE-O-TURNO` | **12 nominais** da classe `e6_zero_esp_trabalho`. Também candidata para a chave compartilhada col922 29/10. Zera **[nao verificado]**. `app/docs/BACKLOG.md:1996,2234` `app/docs/RELATO.md:2157,2198` | **Registrada**, fila 1 após E2 pousar com E3. O instrumento anterior à Fatia 1 consta como pousado. A ordem de código é Fatia 1 **PAR**, depois Fatia 2 **DIA**, com motor e `DiaPago` lendo o par e o dia da autoridade do turno. O BACKLOG exige RED pela regra antes, DIFF 10/11 por dono e Pauta DP para exportada. `app/docs/BACKLOG.md:2234–2235` `app/docs/GRADE-MESTRA.md:105` `app/core/juizes.py:748` `app/ponto/motor_calculo_v2.py:867–884,2416–2424` `app/ponto/services/dia_pago.py:109–132` |
| `ORACULO-E6-43-DEFEITOS` | **6 cobertos pela família documentada**, cinco de pausa de col252 e o corte de col909. Zera **[nao verificado]**. `app/docs/BACKLOG.md:823` `app/docs/GRADE-MESTRA.md:303` `app/docs/RELATO.md:2390,3096` `depois_c11.txt:42` | **Registrada** na GRADE como instrumento. O ID da linha que a registra no BACKLOG é `EV-NA-BRECHA-1611`. O pré-requisito declarado é `CONTRATO-DE-INSTRUMENTO` quando chegar a vez. Os pontos de cálculo são o agrupamento e a soma posicional para a pausa, e o corte por vão para col909. `app/docs/BACKLOG.md:823` `app/docs/GRADE-MESTRA.md:303` `app/ponto/calculador/nucleo.py:215–225,235–253` |
| `NOTURNO-A-JANELA-NO-DIA-DO-TURNO` | **1 chave exclusiva do mecanismo**, col1010 05/11, e **1 compartilhada**, col922 29/10. Zera **[nao verificado]**. `depois_c11.txt:43,49` `app/docs/RELATO.md:194,205` | **No ar** conforme BACKLOG. O escopo autorizado é a âncora da janela na comp. 11 em diante. O cálculo usa o mapa do juiz, o marco de saída em D+1 quando aplicável e a L-084 nas pontas cruas. O BACKLOG registra dependência de `MOTOR-LE-O-TURNO` para B-I28/v0815_7. A atualização do gravado depende de relavra por evento ou botão. `app/docs/BACKLOG.md:1989` `app/docs/RELATO.md:143,270` `app/ponto/motor_calculo_v2.py:936–951,1543–1588` `app/ponto/janela_he.py:41–61` |
| `EV-NA-BRECHA-1611` e `O54` | **2 candidaturas sem dono confirmado**, col922 01/11 e 04/11. `EV-NA-BRECHA-1611` é a leitura da soma e da exibição. `O54`, etapa `E3-geometria`, é a fatia apontada pelo diagnóstico. Zera **[nao verificado]**. `app/docs/BACKLOG.md:823,1469,2529` `app/docs/RELATO.md:3097` | **EV registrada como leitura**, com i1/i2/i3b aguardando `todos.fim` da PREPARA na linha. **O54 em curso** na linha do programa, com portas sequenciais. O registro de código marca a entrega histórica de `E3-geometria` como feita pela `O65`. A pendência nominal é confirmar, nas duas chaves, o turno que entra na soma e qual período chega ao cartão. `app/docs/BACKLOG.md:823,1469,1481` `app/core/espelho_verdade.py:67–78` `app/docs/RELATO.md:2926,3097` `app/ponto/services/espelho.py:464–474,689–690,801` |
| `R1-SEM-DONO-2611` | **18 chaves da comp. 11 neste mapa**, agrupadas abaixo. Zera **[nao verificado]**. `depois_c11.txt:12–49` | **Registrada**, investigação por família, sem vaga, conforme o BACKLOG fornecido e o PUBLICADO §5. Linha desse ID na cópia **[nao verificado]**. Pré-requisito de código por família **[nao verificado]**. | [nao verificado]

**Pré-requisitos da vaga.** O anexo BACKLOG põe a ordem após o voo e os pré-requisitos de E4d. A linha local de `ORDEM-PELO-R1-2611` está **[nao verificado]**. Na cópia, `E4C-F5-INSTANTE` e `PORTA-CONFERE-TRAVA` estão registrados antes da E4d, e `E4D-CONTRATO-ROTA-B` registra a matriz respondida e o brief como próximo passo. `app/docs/BACKLOG.md:830,1985,2155` `app/docs/RELATO.md:98,13237`

## 3. SEM DONO por classe

- **`e6_zero_esp_trabalho` — 1.** col302 20/11. `depois_c11.txt:34` `app/docs/BACKLOG.md:2234`
- **`esp_zero_e6_trabalho` — 1.** col996 28/10. `depois_c11.txt:47` `app/core/espelho_verdade.py:60–61`
- **`diverge_10_60` — 5.** col114 08/11 e 15/11, col252 18/11 e 19/11, col884 08/11. `depois_c11.txt:18–19,26–32,41` `app/docs/RELATO.md:2390,3097,13118`
- **`diverge_acima_60` — 11.** col112 21/10 e 22/10, col130 29/10, col200 03/11, col220 10/11, col382 11/11, col874 22/10, col922 01/11 e 04/11, col989 23/10, col996 10/11. `depois_c11.txt:14–15,20,23,25,36,40,43–46,48–49` `app/docs/RELATO.md:189,2926,3097,13115–13117`
- **`diverge_acima_60/FORA` — 0 SEM DONO.** A única chave, col909 11/11, tem o corte documentado. `depois_c11.txt:42` `app/docs/GRADE-MESTRA.md:303` `app/ponto/calculador/nucleo.py:215–225`

## 26/11 10:42 · sessao 16453a22ec76 · ABONO-X-SAIDA-ANTECIPADA · leitura · origem=Code

## 1. Qual valor entra como parcela paga da `Ausencia` parcial?

**LEI-NOVA.** As fontes fixam os minutos declarados como crédito do atraso de 17/09, o período declarado como critério de cobertura e o déficit da linha `ausencia` como abono projetado pela grade. Para o **teto líquido da obra**, falta a regra que escolhe entre esses valores ou exige coincidência com a ponta coberta. A escolha muda o valor do desconto e fica fora de **DEFINIÇÃO TÉCNICA** de chave, aceite ou encaixe. A chave do dia de início da jornada já está decidida pela L-085 e pelo corte da parcial noturna. `r.md:54,72` (anexo) · `app/ponto/services/credito_parcial.py:1–5,15–29` · `app/ponto/services/dia_pago.py:372–396` · `app/docs/CORTES.md:462,473` · `app/docs/LEIS.md:117` · `CLAUDE.md:285–291`

## 2. Como o crédito exclusivo do atraso convive com o teto líquido?

**LEI-NOVA.** A L-093 manda preservar a proporção quando seu teto reduz a soma. O corte de 17/09 manda abater o atraso do próprio dia. Falta a precedência entre as duas operações e a regra de como cumprir o abatimento quando o teto líquido já comporta toda a pontualidade. A escolha muda as rubricas e fica fora de **DEFINIÇÃO TÉCNICA** de chave, aceite ou encaixe. `r.md:73` (anexo) · `app/docs/LEIS.md:111` · `app/ponto/services/credito_parcial.py:1–5,50–61` · `CLAUDE.md:285–291`

**Conta literal de `CORTES.md:558`.** “a justificativa aprovada paga o que FALTA para o trabalhado do dia chegar ao previsto, nunca mais que isso e nunca negativo”. Com `P−T=120`, a frase manda a **Justificativa aprovada pagar 120 min**. O `B=60` dado é o valor de crédito parcial do exemplo. Usando esse `B` na conta do dossiê, sobram `120−60=60 min` no total. **A linha 558 sozinha fixa nenhum dos dois rateios, nem `15/45` nem `0/60`.** `app/docs/CORTES.md:558` · `app/docs/PROMPTS.md:2312` · `r.md:54,73` (anexo)

| Opção do dossiê | Conta em minutos de atraso / saída antecipada |
|---|---|
| Só teto líquido proporcional | `A+S=120`, fator `60/120=0,5`, resultado **15/45**. `r.md:54,73` (anexo) · `app/docs/LEIS.md:111` |
| Crédito primeiro no atraso | `min(30,60)=30` abate todo o atraso. Os `30` restantes cobrem parte dos `90` de saída, resultado **0/60**. `r.md:73` (anexo) · `app/ponto/services/credito_parcial.py:50–61` |
| Caminho atual da cópia, com o crédito na chave do próprio dia | O teto bruto é `120` e mantém `30/90`. O abatimento posterior resulta em **0/90** no fechamento e no DiaPago `motor`. `app/ponto/motor_calculo_v2.py:1187–1213` · `app/ponto/services/fechamento.py:1013–1026` · `app/ponto/services/dia_pago.py:173–195` |

## Leis e cortes candidatos, por pergunta

Na cópia, `CORTES.md:462` e `:473` são de **27/10**. O corte **JUSTIFICATIVA-COMPLETA-ATE-O-PREVISTO** é de **14/11**, na linha `:558`. O texto do crédito de **17/09** está registrado na docstring do serviço e identificado pelo mesmo nome em `TICKETS.md`. `app/docs/CORTES.md:462,473,558` · `app/ponto/services/credito_parcial.py:1–10` · `app/docs/TICKETS.md:1108`

| Candidata e trecho literal | 1. Valor da parcela paga | 2. Precedência e rateio |
|---|---|---|
| **L-093** — “`atraso + saida antecipada` do DIA nunca passam de `(previsto do dia - trabalhado do dia)`” e “se mantem a PROPORCAO em que elas estavam”. `app/docs/LEIS.md:111` | **não responde porque** define o teto por previsto e trabalhado, sem escolher `B`. | **não responde porque** fixa a proporção da redução pelo teto e deixa aberta a ordem em relação ao crédito. No exemplo, seu teto literal de `120` já comporta `30+90`. |
| **L-084** — “entrada **E** saida, as DUAS, a mais de **3 h** dos seus marcos = o TURNO INTEIRO esta em outro horario” e “UMA ponta longe desconta normalmente”. `app/docs/LEIS.md:116` | **não responde porque** decide quando os marcos descrevem o dia para julgar pontualidade. | **não responde porque** a distância aos marcos decide a incidência do julgamento, sem ordenar crédito e teto. |
| **L-085** — “A JORNADA PERTENCE AO DIA DE INICIO” e “hora depois da meia-noite herda o dia em que o plantao COMECOU”. `app/docs/LEIS.md:117` | **não responde porque** fixa a chave da jornada e deixa o valor de `B` em aberto. | **não responde porque** fixa o dia da conta e deixa a ordem dentro do dia em aberto. |
| **L-006 e L-024** — “regra que varia por cliente/praca/empresa nasce como cadastro pela UI, com nome e leitor” e “A GRADE E FONTE UNICA DE PREVISTO”. `app/docs/LEIS.md:64,75` | **não responde porque** disciplinam o cadastro e a fonte de `P`, sem escolher a fonte da parcela paga. | **não responde porque** cadastro e fonte de `P` deixam em aberto a ordem dos abatimentos. |
| **L-039 e L-088** — “FALTA TEM UM SIGNIFICADO = decidida” e “falta e dia previsto sem trabalho e sem justificativa”. `app/docs/LEIS.md:87,120` | **não responde porque** definem falta e furo, sem quantificar abono parcial. | **não responde porque** definem a classificação do dia, sem distribuir desconto entre as pontas. |
| **L-089** — “ausencia que abona/desconta so alcanca dia que a escala previa; em folga nao gera efeito nem consome saldo”. `app/docs/LEIS.md:121` | **não responde porque** delimita os dias elegíveis e deixa o valor de `B` em aberto. | **não responde porque** a elegibilidade do dia deixa o rateio em aberto. |
| **L-073 e tolerância de 14/11** — “so valem comparar INSTANTE ou exigir FATO ENCERRADO” e “5 min por marcacao com teto de 10 no dia”. `app/docs/LEIS.md:100` · `app/docs/CORTES.md:521` | **não responde porque** tratam do momento de julgar e da tolerância de marcação. | **não responde porque** nenhum desses tetos ordena a aplicação do crédito parcial. |
| **L-097 emendada e corte do trabalhado real** — “o trecho apos o marco de saida CONTA no trabalhado e no noturno” e “o teto da L-093 usa o trabalhado REAL do dia (ANTES da janela de HE da L-097)”. `app/docs/LEIS.md:115` · `app/docs/PROMPTS.md:14` | **não responde porque** definem `T`, sem definir `B`. | **não responde porque** definem a base trabalhada do teto e deixam em aberto a aplicação do crédito. |
| **17/09 — CREDITO-NO-PROPRIO-DIA** — “a declaracao parcial paga abate o atraso DO PROPRIO DIA, nunca de outro” e “o dia conta pelas batidas e os minutos viram credito”. `app/ponto/services/credito_parcial.py:1–5` · `app/docs/TICKETS.md:1108` | **não responde porque** os minutos declarados são a opção escrita para o crédito histórico do atraso. O texto não escolhe a parcela que entra no teto líquido das duas pontas. | **não responde porque** manda abater o atraso do próprio dia, mas não diz como compor esse abatimento com o teto líquido nem como tratar a antecipada. |
| **B-B21** — “as horas batidas contam e o abono cobre so o previsto que as batidas nao cobriram; o dia nunca passa de previsto + HE decidida”. `app/docs/CORTES.md:88,420` · `app/ponto/services/dia_pago.py:372–396` | **não responde porque** fixa o abono da linha `ausencia` na grade. A aplicação desse valor a toda parcial elegível para o teto fica em aberto. | **não responde porque** limita o abono pelo que faltou no dia, sem ordenar abatimento e rateio. |
| **BO-ADMIN-2710 e COBERTURA-PARCIAL-PERIODO** — “FATO_EM_AUSENCIA so com batida DENTRO do periodo declarado”, “o periodo de ausencia PARCIAL e cadastro” e “ponta pelo tipo e inferencia, a declaracao nao diz a ponta”. `app/docs/CORTES.md:455,458,462–463,466,477` | **não responde porque** decide a cobertura pelo período declarado. A recusa de inferir a ponta pelo tipo não estabelece coincidência com a ponta como requisito financeiro. | **não responde porque** julgar uma batida ou um marco dentro do período deixa em aberto a precedência entre crédito e teto. |
| **COBERTURA-ABONO-DO-PERIODO** — “pousa agora so a parte da AUSENCIA parcial […] que nao muda o abono do motor” e “o periodo cruza a meia-noite e pertence ao dia de INICIO do turno”. `app/docs/CORTES.md:473` | **não responde porque** separa a cobertura da `Ausencia` do pagamento da `Justificativa` e fixa a chave da parcial noturna. | **não responde porque** separar as obras e fixar o dia da jornada deixa a ordem financeira em aberto. |
| **PARCIAL-BORDA-V2-2511** — “a dispensa do MARCO PREVISTO fica como hoje (periodo_da_parcial_no_dia inclusivo)” e “S no minuto do inicio e E no minuto do fim estao FORA, E no inicio e S no fim estao DENTRO”. `app/docs/CORTES.md:648` | **não responde porque** decide a borda de cobertura, sem quantificar parcela paga. | **não responde porque** a borda da batida não ordena os abatimentos. |
| **ABONADO-COM-SAIDA-NA-ZONA-INVIOLAVEL** — “ausencia que explica o dia INTEIRO zera atraso/saida antecipada; parcial so se os minutos ja saem de juiz existente”. `app/docs/RELATO.md:7039` · `app/ponto/motor_calculo_v2.py:1102–1122` | **não responde porque** exige juiz existente para a parcial, sem identificar qual valor desse juiz entra no teto líquido. | **não responde porque** fixa zero nas duas pontas para a inteira, sem regra de precedência para a parcial. |
| **JUSTIFICATIVA-PAGA-O-PERIODO-B / B-NAO-FECHA** — “a justificativa aprovada vira efeito PARCIAL pela MESMA porta da Ausencia” e “motor_calculo_v2 le o periodo pago”. `app/docs/CORTES.md:479,496` | **não responde porque** o texto dá uma porta de leitura para `Justificativa`, sem escolher `B` da `Ausencia` parcial. | **não responde porque** o aval de leitura do período pago deixa a ordem das duas operações em aberto. |
| **JUSTIFICATIVA-COMPLETA-ATE-O-PREVISTO** — “a justificativa aprovada paga o que FALTA para o trabalhado do dia chegar ao previsto, nunca mais que isso e nunca negativo” e “O motor e o periodos_justificados leem o MESMO juiz: um numero so.” A própria linha diz “substitui o aval JUSTIFICATIVA-PELA-SOMA das 18:08”. `app/docs/CORTES.md:558` · `app/docs/PROMPTS.md:2295,2312` | **não responde porque** quantifica o pagamento de `Justificativa` aprovada e não estende essa fórmula à `Ausencia` parcial. | **não responde porque** fixa o total pago pela justificativa e não determina a precedência nem o par de rubricas. |
| **CATALOGO-SAIDA-ANTECIPADA-DESCONTA** — “saida_antecipada = DESCONTA em minutos”. `app/docs/CORTES.md:345` | **não responde porque** define o efeito do tipo e a unidade do desconto. | **não responde porque** o efeito `DESCONTA` não fixa a prioridade do crédito nem o valor final da saída. |
| **Contrato da obra no dossiê** — “A soma esperada nesse contrato é `min(A + S, max(0, P − T − B))`.” Contrato aprovado independente do dossiê [nao verificado]. `r.md:54` (anexo) | **não responde porque** recebe `B` como entrada e deixa expressamente a aplicação a toda parcial para a pergunta 1. | **não responde porque** a fórmula entrega uma soma. A seção 7 apresenta os dois pares para essa mesma soma. `r.md:73` (anexo) |
| **Célula soberana / L-020** — “dia + marcos sao DADO com DNA congelado, irretroativo”. O ADR diz “O VALOR do dia NAO entra na celula: fica no `DiaPago`”. `app/docs/LEIS.md:71` · `app/docs/ADR/0002-celula-e-o-dia.md:8–10` | **não responde porque** fixa a autoridade do dia e dos marcos, sem escolher o valor da parcial. | **não responde porque** a autoridade dos dados do dia deixa a fórmula financeira em aberto. |
| **Juiz único / L-001 e L-002** — “1 JUIZ por pergunta” e “tela, PDF, TXT, badge e contador leem a MESMA autoridade”. `CLAUDE.md:17–22,179` · `app/docs/LEIS.md:59–60` · `app/docs/PLANO-PADROES.md:53–62` | **não responde porque** exige uma autoridade para o valor de `B`, sem selecionar qual valor ela deve entregar. | **não responde porque** exige uma decisão comum aos leitores, sem escolher a ordem da conta. |
| **Escritor único / L-007 e L-095** — “campo de estado tem 1 escritor canonico + tripwire” e “e lavrado **uma vez**, pelo motor v2, no evento”. `app/docs/LEIS.md:65,113` · `CLAUDE.md:179` | **não responde porque** fixa a autoridade da escrita, sem escolher o insumo financeiro. | **não responde porque** lavrar uma vez não determina qual dos dois pares deve ser lavrado. |
| **Lâmpada 4a / L-061** — “CELULA/ATA por marco = o que aconteceu (a LAMPADA)” e “CONSUMIDOR (chamado, pergunta, tela, folha) le a lampada, nunca re-julga”. `CLAUDE.md:170–174` · `app/docs/LEIS.md:95` | **não responde porque** fixa a leitura do fato lavrado por marco, sem converter cobertura em minutos pagos para esse teto. | **não responde porque** ler a lâmpada não fixa a precedência entre crédito e redução proporcional. |
| **L-110 e L-117** — “o codigo de hoje **nao e gabarito** do codigo novo” e “Lei nova so nasce de **corte LITERAL do Ronald**”. `app/docs/LEIS.md:136,138` | **não responde porque** o comportamento atual prova a fonte do crédito histórico e a regra de conduta exige corte literal para a escolha ainda aberta. | **não responde porque** o comportamento atual prova a sequência da cópia e a regra de conduta exige corte literal para a composição ainda aberta. |

A identidade da cópia com o hash informado e os valores efetivamente emitidos em TXT entregue ficam [nao verificado].

## 26/11 10:41 · sessao e840e4e72b81 · CLEAR-NAO-PARA-A-NOITE · verificador · origem=Code

Os cinco arquivos da cópia conferem com os hashes de conteúdo do lado novo do diff. A identidade Git de `HEAD` e do pai, a execução integral dos selos e o comportamento em produção ficam **[nao verificado]**. Conferi a sintaxe dos dois selos e reproduzi em memória os contraexemplos de handoff repetido, retorno com handoff antigo, fallback de CLEAR e contador com linha truncada. O selo integral cria e apaga arquivos. `bin/tests/test_orcamento_contexto.sh:32`, `bin/tests/test_orcamento_contexto.sh:611`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1250`, `bin/disjuntor.py:2121`.

## Achados da v1.r

| Achado | O que a volta 2 resolve |
|---|---|
| Janela de 23:00 a 08:00 ou 30 min sem resposta | **Em parte.** A janela continua incluindo 23:00 e excluindo 08:00, agora com selo nas quatro bordas. De dia continua exigindo **mais de** 30 min desde a fala selecionada, sem verificar se ela responde a uma pergunta. `bin/orcamento_contexto.py:89`, `bin/orcamento_contexto.py:113`, `bin/tests/test_orcamento_contexto.sh:516`. |
| Disparo diurno atravessava a entrada da noite em silêncio | **Resolve o caminho apontado.** Com topo neutro e sem handoff elegível, há anúncio próprio uma vez. Com handoff elegível, sai o recado de fim de marco. Com `PAREI` no topo ou DIFF/apply detectado, esses ramos têm prioridade. O AVISO efetivamente escrito no RELATO fica **[nao verificado]**. `bin/hook_stop_fila1.py:1082`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1118`. |
| Instrução de handoff faltava nas entradas tardias e só havia um recado de fim de marco | **Em parte.** As entradas agora pedem o handoff e o recado se repete a cada `mtime` elegível. O hook usa o `mtime` do arquivo como prova de marco, inclusive quando o mesmo arquivo ainda satisfaz o corte. `bin/hook_stop_fila1.py:929`, `bin/hook_stop_fila1.py:1070`, `bin/hook_stop_fila1.py:1106`, `bin/hook_stop_fila1.py:1110`. |
| Troca de `PAREI` e fim de marco passavam antes da guarda de DIFF/apply | **Em parte.** A guarda agora precede os dois. Com a trava detectada, há um `siga` próprio para concluir o ato, que também carrega a instrução de handoff e AVISO. A sondagem libera a trava antes da decisão e da emissão. `bin/hook_stop_fila1.py:1089`, `bin/hook_stop_fila1.py:1096`, `bin/disjuntor.py:410`, `bin/hook_stop_fila1.py:1439`. |
| Retorno diurno perdia novo pedido por causa de `fim_visto` e herdava o tempo de espera | **Em parte.** O retorno limpa `fim_visto` e `clear_parei_desde`, e com topo neutro um handoff elegível volta a provocar o pedido. O caminho direto de `PAREI` pode reaproveitar handoff diurno, e uma leitura inconclusiva também dispara a transição de volta. `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1161`, `bin/hook_stop_fila1.py:1184`, `bin/orcamento_contexto.py:95`. |
| Dois classificadores de fala e duas aberturas da trilha do orçamento no mesmo laço | **Resolve.** ESCADA e janela usam `ultima_fala`. `ATENDIDO` e `CLEAR-NOITE` são lidos na mesma abertura em `disjuntor.ler`. A ESCADA passa a aceitar `enqueue` como entrada, inclusive se houver `remove` posterior. `bin/disjuntor.py:1342`, `bin/disjuntor.py:1394`, `bin/disjuntor.py:1409`, `bin/orcamento_contexto.py:105`, `bin/disjuntor.py:2115`. |
| Classificação das linhas de transcript | **Em parte.** Slash e eco bash iniciados por `<`, `enqueue` vazio e `enqueue` de sidechain ficam excluídos. Imagem sem texto e resposta representada por `tool_result` ainda ficam excluídas. Um `enqueue` válido seguido de `remove` continua valendo por regra explícita. O formato real dessas entradas fica **[nao verificado]**. `bin/disjuntor.py:1342`, `bin/disjuntor.py:1348`, `bin/disjuntor.py:1355`, `bin/disjuntor.py:1360`, `bin/tests/fixtures/orcamento_transcript.jsonl:1`. |
| Transcript ilegível ou sem instante e contenção de exceções | **Em parte.** `OSError` e última fala reconhecida sem instante dão `None` de dia. JSON truncado ainda é pulado e pode deixar uma fala antiga decidir a ausência. A janela horária ainda decide antes de ler o arquivo. `bin/orcamento_contexto.py:99`, `bin/orcamento_contexto.py:106`, `bin/disjuntor.py:1376`, `bin/hook_stop_fila1.py:1234`. |
| Zero ambíguo, futuro aceito e rótulo do contador | **Em parte.** `OSError` e data inválida numa linha reconhecida agora renderizam `nao sei`, o futuro é excluído e o rótulo diz “recados”. Linha curta ou com o evento fora da terceira coluna ainda pode virar zero. A trilha ainda grava hora local sem offset, interpretada como local pelo leitor. O fuso efetivo dos processos em produção fica **[nao verificado]**. `bin/disjuntor.py:2121`, `bin/disjuntor.py:2134`, `bin/disjuntor.py:2153`, `bin/disjuntor.py:2421`, `bin/hook_stop_fila1.py:887`. |
| Selo aceitava dia vazio, `siga` repetido e 08:00 dentro da noite | **Resolve esses três buracos.** O caso diurno exige a frase inteira, o caso `PAREI` exige o `ALLOW` comum no segundo Stop e o caso de borda exige `None` às 08:00 com fala recente. `test_disjuntor.sh` agora acrescenta fala recente e fixa a hora para os casos diurnos. Outras lacunas do selo constam abaixo. `bin/tests/test_orcamento_contexto.sh:519`, `bin/tests/test_orcamento_contexto.sh:529`, `bin/tests/test_orcamento_contexto.sh:598`, `bin/tests/test_disjuntor.sh:87`. |
| Registro no 7b, `PROMPTS.md` e config | **Não resolve a entrega documental.** O 7b da cópia contém a regra geral anterior e `PROMPTS.md` termina sem o registro deste aval. O diff fornecido preserva a config, que já chama o handoff no `PreCompact`. `CLAUDE.md:343`, `app/docs/PROMPTS.md:3005`, `.claude/settings.json:14`, `.claude/settings.json:25`. |

## 1. Escritores, leitores e ESMERIL-SEMÂNTICA

O escritor em operação é `_orcamento_travado`, no Stop. Ele roda sob a trava do estado, salva por `os.replace` e depois apenda os eventos. O disjuntor lê `ultima_medida` do mesmo JSON, sem consumir esses campos. Os selos semeiam `clear_parei_desde` diretamente para simular espera. `bin/hook_stop_fila1.py:903`, `bin/hook_stop_fila1.py:972`, `bin/disjuntor.py:2165`, `bin/tests/test_orcamento_contexto.sh:197`, `bin/tests/test_disjuntor.sh:212`.

| Campo | Escrita, leitura e significado |
|---|---|
| `noite` | O Stop grava `True` ao armar ou processar disparo na janela. Lê na saída da janela. Marca que o disparo esteve na conduta sem Ronald, por horário ou ausência. `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1222`. |
| `noite_anunciada` | O Stop grava no disparo noturno, no fim de marco ou no anúncio de entrada. Lê para suprimir o anúncio de entrada. É um registro de recado emitido por esses ramos. A troca de `PAREI` e o aviso de DIFF também carregam a instrução e deixam o campo intacto. `bin/hook_stop_fila1.py:1093`, `bin/hook_stop_fila1.py:1103`, `bin/hook_stop_fila1.py:1112`, `bin/hook_stop_fila1.py:1118`, `bin/hook_stop_fila1.py:1222`. |
| `noite_clear_visto` | O ramo de `PAREI` noturno lê e grava `True` ao **pedir** a troca. Suprime outro pedido no mesmo trecho noturno, mesmo se o topo mudar e depois voltar a `PAREI`. `bin/hook_stop_fila1.py:1077`, `bin/hook_stop_fila1.py:1101`. |
| `adiado_noite` | O ramo DIFF lê e grava a string do motivo para suprimir repetição. Apaga na primeira observação fora desse ramo. A string identifica o motivo, sem identificar o ato que tomou a trava. `bin/hook_stop_fila1.py:1089`, `bin/hook_stop_fila1.py:1092`, `bin/hook_stop_fila1.py:1100`. |
| `fim_visto` | O ramo diurno lê e grava `True` quando **pede** `PAREI` após handoff elegível. A flag prova emissão do pedido. `bin/hook_stop_fila1.py:1184`. |
| `fim_dia_em` | O ramo diurno grava o epoch `agora` junto com `fim_visto`. A noite lê no corte de handoff e a volta incorpora em `handoff_min`. É o instante do Stop que pediu a linha. O caminho diurno que recebe o par diretamente libera sem gravá-lo. `bin/hook_stop_fila1.py:1076`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1161`, `bin/hook_stop_fila1.py:1186`. |
| `handoff_min` | A noite e o adiamento diurno gravam `agora`. A volta grava o máximo com `fim_dia_em`. Os leitores são fim noturno, par diurno e fim diurno. A unidade continua epoch em segundos, com limite inferior inclusivo sobre `st_mtime`. As escritas usam o instante dos Stops. `bin/hook_stop_fila1.py:929`, `bin/hook_stop_fila1.py:1076`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1147`, `bin/hook_stop_fila1.py:1184`. |
| `clear_parei_desde` | O ramo diurno grava o primeiro Stop com o par apto a liberar e zera em adiamento. O mesmo ramo lê para expirar e o fechamento do ciclo lê para medir o clear. O sentido é o início da espera válida pelo `/clear`. `bin/hook_stop_fila1.py:1032`, `bin/hook_stop_fila1.py:1129`, `bin/hook_stop_fila1.py:1147`, `bin/hook_stop_fila1.py:1161`. |
| `sem_handoff_visto` | O ramo diurno lê e grava ao emitir a primeira cobrança de handoff para `PAREI` sem o par. Marca a cobrança emitida. `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1135`. |
| `adiado` | O ramo diurno lê e grava o motivo textual da guarda, DIFF/apply ou agente de fundo, para suprimir recado repetido pelo mesmo motivo. `bin/hook_stop_fila1.py:936`, `bin/hook_stop_fila1.py:1145`. |

Na volta, o Stop remove todos esses campos salvo `handoff_min`, que atualiza. O marco novo descarta o disparo inteiro. As unidades dos campos de tempo são coerentes. As ambiguidades procedentes são usar o instante do pedido como testemunha do handoff consumido, usar `mtime` como identidade de marco e tratar o `None` de presença **ou** leitura inconclusiva como “Ronald presente de novo”. `bin/hook_stop_fila1.py:1046`, `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1186`, `bin/orcamento_contexto.py:95`.

## 2. Juízes em `bin/`

Há um classificador de fala, `disjuntor._fala_do_ronald`, e um juiz de janela, `orcamento_contexto.janela_sem_ronald`. Os outros leitores de `type == 'user'` encontrados usam a linha para identificar o prompt inicial do verificador B3 ou o comando `/clear` para o piso. O leitor de `queue-operation` para fala é o classificador compartilhado. `bin/disjuntor.py:649`, `bin/disjuntor.py:1342`, `bin/disjuntor.py:1350`, `bin/orcamento_contexto.py:95`, `bin/orcamento_contexto.py:289`.

A centralização mudou dois critérios da janela. A cauda cai de 4 MiB para 2.000.000 bytes e “última” passa do maior timestamp para a última fala na ordem do arquivo. Em memória, uma linha de 1 min atrás seguida no arquivo por outra de 40 min abriu ausência. O próprio selo também contém o `user` sintético `Async agent launched successfully...`, que o leitor de agentes identifica como retorno do harness e o juiz de fala aceita como texto. Sem timestamp, essa entrada sobrepõe a fala anterior e dá `None` na janela diurna. Ocorrência desses formatos em produção **[nao verificado]**. `bin/orcamento_contexto.py:65`, `bin/disjuntor.py:469`, `bin/disjuntor.py:1352`, `bin/disjuntor.py:1370`, `bin/disjuntor.py:1376`, `bin/orcamento_contexto.py:111`, `bin/tests/test_disjuntor.sh:235`.

## 3. Repetição, trava e liberação

- **Repetição confirmada para o mesmo handoff.** O Stop aceita `mtime >= corte` e grava o seu próprio `agora` como próximo corte. Um `mtime` igual ao corte pode ser aceito de novo. Um `mtime` no futuro gerou dois `siga` consecutivos em memória, sem escrita nova de handoff. O cursor avançou nos dois Stops, mas o marco e o recado eram os mesmos. `bin/hook_stop_fila1.py:929`, `bin/hook_stop_fila1.py:999`, `bin/hook_stop_fila1.py:1110`.
- **Sai `siga` com `sombra.lock` tomada.** É o recado específico para concluir o ato em curso. Ele proíbe abrir outro DIFF/apply e inclui instrução de handoff e AVISO. Há também uma corrida para o recado normal, pois a guarda solta o `flock` compartilhado antes de salvar e imprimir. `bin/hook_stop_fila1.py:1070`, `bin/hook_stop_fila1.py:1091`, `bin/hook_stop_fila1.py:1114`, `bin/disjuntor.py:421`, `bin/hook_stop_fila1.py:1439`.
- **O Stop pode encerrar de noite.** Quando o orçamento retorna `None`, o fluxo comum pode dar `ALLOW` silencioso. O ramo noturno normal não dá `CLEAR-LIBERA`. Já o fallback de exceção dá `CLEAR-LIBERA` com `PAREI`, handoff recente e guarda livre, sem consultar a janela. Reproduzi esse fallback em memória. `bin/hook_stop_fila1.py:132`, `bin/hook_stop_fila1.py:1125`, `bin/hook_stop_fila1.py:1234`, `bin/hook_stop_fila1.py:1250`, `bin/hook_stop_fila1.py:1436`, `bin/hook_stop_fila1.py:1480`.

## 4. Transições e aval

| Transição | Saída e aderência |
|---|---|
| Dia → noite, disparo ativo e topo neutro | **Atende no fluxo nominal.** Sem handoff elegível, um `siga` pede handoff e AVISO. Com handoff elegível, o recado assume fim de marco. Depois, sem novidade, segue o caminho comum. `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1118`, `bin/hook_stop_fila1.py:1480`. |
| Noite → dia, retorno | **Parcial.** Registra `CLEAR-VOLTA`, limpa as marcas anteriores e, com topo neutro, volta a pedir `PAREI` quando o handoff é elegível. O corte usa os Stops anteriores, portanto um handoff anterior à fala de retorno pode qualificar. Transcript inconclusivo também aciona a volta. Uma fala dentro de 23:00–08:00 mantém a regra horária. `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1184`, `bin/orcamento_contexto.py:95`. |
| DIFF/apply começa e acaba de noite | **Parcial.** Se o Stop observa a trava tomada, sai um `siga` de conclusão e os seguintes caem no fluxo comum enquanto o motivo persiste. Ao observar a trava livre, apaga `adiado_noite` e reavalia topo, handoff e anúncio. Dois atos separados por uma folga que nenhum Stop observou compartilham a supressão. A corrida da sondagem impede garantir a ordem até a emissão. `bin/hook_stop_fila1.py:1089`, `bin/hook_stop_fila1.py:1100`, `bin/disjuntor.py:421`. |
| `PAREI` de dia atravessa para a noite | **Parcial.** Com a guarda livre, sai `siga` pedindo a troca por AVISO. Reproduzi em memória a sequência em que o par já havia liberado de dia pela rota direta, sem `fim_dia_em`. Após a troca, o mesmo handoff produziu um falso fim de marco noturno. Se o `PAREI` persistir até a volta, o mesmo par pode liberar de dia novamente. `bin/hook_stop_fila1.py:1076`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1161`. |
| Disparo pendente entra à noite | **Atende quando os impedimentos cessam.** Com DIFF/apply ou outro impedimento reconhecido, mantém `PENDENTE`. No primeiro Stop elegível, pode armar mesmo com a medida abaixo do limiar e sai `siga` de AVISO, handoff e fila. `bin/hook_stop_fila1.py:1194`, `bin/hook_stop_fila1.py:1205`, `bin/hook_stop_fila1.py:1218`. |
| Marco de `/clear` novo à noite | **Fecha o ciclo antigo.** A classificação `ATENDIDO` ou `SEGUIU` ocorre antes da janela. Se carregou `clear_parei_desde` de dia, o intervalo inclui o tempo noturno e pode registrar clear tardio. Com novo gatilho elegível, arma outro ciclo noturno no mesmo Stop. Sem gatilho, segue o fluxo comum. O registro comprova que o hook observou o marco. A execução do handoff antes do ato fica **[nao verificado]**. `bin/hook_stop_fila1.py:1025`, `bin/hook_stop_fila1.py:1059`, `bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1221`. |

## 5. `disjuntor.ler`

- **`nao sei` é parcial.** `OSError` e uma linha reconhecida como `CLEAR-NOITE` com data inválida viram `None`. `lixo hora CLEAR-NOITE`, com três palavras, é descartada antes da validação e deu `0` na sonda em memória. Uma trilha existente e legível sem eventos na janela também dá zero. `FileNotFoundError` indica caminho ausente no momento da leitura, inclusive se a trilha tiver sido removida. `bin/disjuntor.py:2113`, `bin/disjuntor.py:2121`, `bin/disjuntor.py:2153`, `bin/disjuntor.py:2422`.
- **Há teto superior.** A janela implementada é `[agora − 86400, agora]`. O código inclui a borda inferior que o comentário descreve como exclusiva. `bin/disjuntor.py:2105`, `bin/disjuntor.py:2111`, `bin/disjuntor.py:2132`.
- **A `TRILHA` principal preserva os filtros e contagens para arquivo estável**, inclusive o tratamento de erro e a aceitação de linhas futuras. A mudança unifica as quatro leituras desse laço. O censo ainda abre `TRILHA` separadamente para outra medida. `bin/disjuntor.py:2115`, `bin/disjuntor.py:2124`, `bin/disjuntor.py:2136`, `bin/disjuntor.py:2155`, `bin/disjuntor.py:2237`.

## 6. Mordida do selo, seção 7

| Caso | O que derruba e o que ainda passa |
|---|---|
| **(a) Fala e janela** | Derruba rejeitar `enqueue` válido e incluir 08:00 na noite. Uma cópia do juiz com outro nome e os mesmos resultados passa. A fala digitada da amostra fica atrás do `enqueue`. `bin/tests/test_orcamento_contexto.sh:494`, `bin/tests/test_orcamento_contexto.sh:497`, `bin/tests/test_orcamento_contexto.sh:509`, `bin/tests/test_orcamento_contexto.sh:519`. |
| **(b) Dia** | Derruba saída vazia, frase alterada e evento `CLEAR-NOITE` diurno. O zero numérico do contador fica sem medição neste caso. `bin/tests/test_orcamento_contexto.sh:529`, `bin/tests/test_orcamento_contexto.sh:542`. |
| **(c) Disparo e contador** | Derruba remover o handoff da mensagem, contar o futuro ou transformar em número o erro de leitura amostrado. Remover o corte inferior ou ignorar a linha truncada de três palavras passa. `bin/tests/test_orcamento_contexto.sh:534`, `bin/tests/test_orcamento_contexto.sh:549`, `bin/disjuntor.py:2121`. |
| **(d) Marcos e volta** | Derruba perder um dos dois recados ou repetir no Stop intermediário da amostra. Passa reaproveitar um mesmo `mtime` futuro. O assert da volta também aceita a mensagem diurna sem o `PAREI` no sufixo, pois confere só o prefixo. `bin/tests/test_orcamento_contexto.sh:560`, `bin/tests/test_orcamento_contexto.sh:573`. |
| **(d2) Dia → noite → dia** | Derruba silêncio de entrada, anúncio repetido e `fim_visto` antigo impedindo a volta com handoff novo. Remover o AVISO do fim do anúncio ainda passa pelo prefixo exigido. `bin/tests/test_orcamento_contexto.sh:576`, `bin/tests/test_orcamento_contexto.sh:584`. |
| **(g) DIFF/apply** | Derruba troca de `PAREI` sob trava e repetição do recado de DIFF. Um mutante que aplica a guarda noturna **só se `_eh_clear(linha)`** passa, pois o teste sempre põe `PAREI` no topo durante a trava. Com topo neutro e handoff elegível, esse mutante dá `siga` de fim de marco sob trava. `bin/tests/test_orcamento_contexto.sh:586`, `bin/tests/test_orcamento_contexto.sh:596`, `bin/hook_stop_fila1.py:1091`, `bin/hook_stop_fila1.py:1110`. |
| **(e) `PAREI` noturno** | O mutante `if True` chega ao hook selecionado por `local H` e cai no passo 2 se repetir a troca. O caso não exercita o fallback de exceção nem o `PAREI` reaparecendo após topo neutro. O `siga-aviso` do selo vem do `ALLOW` na trilha. `bin/tests/test_orcamento_contexto.sh:112`, `bin/tests/test_orcamento_contexto.sh:598`, `bin/tests/test_orcamento_contexto.sh:614`, `bin/tests/_saida_do_stop.sh:13`, `bin/hook_stop_fila1.py:1250`. |
| **(f) Ausência de dia** | Derruba remover a ausência diurna ou usar a última linha do assistente como fala. A troca de `>` por `>=` no limiar de 30 min passa pelos valores amostrados. `bin/tests/test_orcamento_contexto.sh:513`, `bin/tests/test_orcamento_contexto.sh:623`, `bin/orcamento_contexto.py:113`. |

## 7. Classes de reprovação

**Procedem** mordida incompleta do selo, ambiguidade na transição de retorno e prova insuficiente do pré-requisito de handoff. A ordem local da guarda melhorou, mas a trava de DIFF/apply fica livre entre sondagem e emissão. Pouso ou deploy efetivo **[nao verificado]**. `bin/tests/test_orcamento_contexto.sh:586`, `bin/hook_stop_fila1.py:1071`, `bin/hook_stop_fila1.py:1110`, `bin/disjuntor.py:421`, `bin/hook_stop_fila1.py:1439`.

**Não procedem no diff** segundo juiz registrado, segundo escritor dos campos fora da porta ou relógio novo fora da borda. A derivação paralela da fala e a abertura duplicada de `orcamento.log` dentro de `disjuntor.ler` foram removidas. O timestamp do append co

[truncado em 20000 de 21557 caracteres]

## 26/11 10:40 · sessao 03ce12260be3 · O327 · leitura · origem=Code

```sh
#!/bin/sh
# Executar na raiz da copia. Entrada do manage.py em app/manage.py:7–18.
# Execucao na sombra e correspondencia com as 93 recusas do relato [nao verificado].
cd app || exit 1

exec python -B -c '
import logging
import os
import sys
from collections import Counter
from contextlib import ExitStack
from unittest.mock import patch


def sonda():
    from django.conf import settings
    from django.db import connection, transaction
    from django_tenants.utils import schema_context
    from ponto.calculador import alimentacao
    from ponto.services import dia_pago, fechamento

    mes, ano = 11, 2026
    # Banco e host literais em app/config/settings/sombra.py:29,47–51,60.
    # Schema juliani em app/config/settings/sombra.py:13–14.
    assert settings.HASNER_SOMBRA is True
    assert connection.settings_dict["NAME"] == "sombra"
    assert connection.settings_dict["HOST"] == "sombra_db"
    assert (mes, ano) not in fechamento.GUARDA_COMPETENCIA  # app/ponto/services/fechamento.py:36–50,578–595

    codigo_rubricas = alimentacao.rubricas_por_dia.__code__
    observadas = {}
    visitas = Counter()
    linhas = []

    def observar_rubricas(frame, evento, retorno):
        if evento != "return" or frame.f_code is not codigo_rubricas or not isinstance(retorno, dict):
            return

        locais = frame.f_locals
        pk = locais["c"].pk
        visitas[pk] += 1

        # Le os itens e bloqueios da chamada real. app/ponto/calculador/alimentacao.py:449,467–475,514–533
        inicio, fim = locais["ini"], locais["fim"]
        itens = locais["_itens"]
        n_minutos = len(locais["v"].minutos)
        bloqueados_na_janela = locais["_bloq"]
        bloqueados_publicados = {
            item["dia"]
            for item in (getattr(locais["resultado"], "dias_sem_previsto_fora_da_janela", None) or ())
            if item.get("dia") is not None
        }
        n_adicionados = len(itens) - n_minutos

        # 0 = com rubricas, sem bloqueado acrescentado aos itens
        # 1 = itens vazios: v.minutos vazio e zero bloqueados na janela
        # 2 = itens presentes, todos fora da janela
        # 3 = com rubricas e pelo menos um bloqueado acrescentado aos itens
        if retorno:
            ramo = 3 if n_adicionados else 0
        else:
            ramo = 2 if itens else 1

        dias = (
            (fim - inicio).days + 1 - len(retorno),
            len(retorno),
            n_minutos,
            len(itens) - len(retorno),
            len(bloqueados_na_janela),
            len(bloqueados_publicados - bloqueados_na_janela),
            n_adicionados,
        )
        observadas[pk] = (retorno, ramo, dias)

    def interceptar_lavra(colab, mes_lavra, ano_lavra, *, versao, rubricas=None, **dados):
        # Intercepta as duas chamadas. app/ponto/services/fechamento.py:1223–1261,1301–1320
        if versao == "motor":
            return 0

        assert versao == "oraculo" and (mes_lavra, ano_lavra) == (mes, ano)
        retorno, ramo, dias = observadas[colab.pk]
        assert rubricas is retorno

        # Avalia a guarda pura com os quatro mapas recebidos.
        # app/ponto/services/dia_pago.py:50–76,135–141,312–321
        sem_dono = set()
        if rubricas is not None:
            donos = dia_pago._DONOS_DECLARADOS(
                rubricas,
                dados["faltas_por_dia"],
                dados["abono_por_dia"],
                dados["previsto_por_dia"],
                dados["realizado_por_dia"],
                dados.get("do_motor", ()),
            )
            sem_dono = set(dia_pago.CAMPOS_DIA) - donos

        linhas.append((colab.pk, ramo, len(sem_dono), dias))
        return 0

    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            assert cursor.fetchone() == ("sombra", "juliani")

        # app/ponto/services/fechamento.py:448–465
        universo = set(fechamento.universo_da_competencia(mes, ano).values_list("pk", flat=True))
        assert universo

        with ExitStack() as controles:
            controles.enter_context(patch.object(dia_pago, "lavrar", new=interceptar_lavra))
            controles.callback(sys.setprofile, sys.getprofile())
            sys.setprofile(observar_rubricas)
            # O fechamento monta os insumos e chama rubricas_por_dia.
            # app/ponto/services/fechamento.py:896–921,1152–1168,1215–1225,1289–1300
            leitura = fechamento.recalcular_fechamento_mes(
                mes, ano, somente_leitura=True, lavrar_dias_pagos=True,
            )

        assert set(leitura) == universo
        assert visitas == Counter({pk: 1 for pk in universo})
        assert len(linhas) == len(universo)
        assert {pk for pk, _, _, _ in linhas} == universo

    # colNNN usa o PK, conforme a convencao de app/ponto/management/commands/censo_fase_12x36.py:263.
    # Linha individual: colNNN ramo campos_sem_dono, seguida das sete contagens de dias.
    # Dias: sem rubrica na janela, com rubrica, em v.minutos, itens fora da janela,
    # bloqueados na janela, bloqueados fora da janela, bloqueados acrescentados aos itens.
    for pk, ramo, campos, dias in sorted(linhas):
        print(f"col{pk:03d}", ramo, campos, *dias)

    # Total: 0 universo processados recusados recusados_com_11_campos.
    print(0, len(universo), len(linhas),
          sum(campos > 0 for _, _, campos, _ in linhas),
          sum(campos == 11 for _, _, campos, _ in linhas))

    # Por ramo: 1 ramo colaboradores recusados recusados_com_11_campos,
    # seguido das somas das sete contagens de dias na mesma ordem.
    for ramo in range(4):
        grupo = [(campos, dias) for _, r, campos, dias in linhas if r == ramo]
        print(1, ramo, len(grupo),
              sum(campos > 0 for campos, _ in grupo),
              sum(campos == 11 for campos, _ in grupo),
              *(sum(dias[i] for _, dias in grupo) for i in range(7)))


# Evita a criacao do diretorio e a abertura dos arquivos de log no bootstrap.
# app/config/settings/sombra.py:27 e app/config/settings/saas.py:118–119,136–143,160–167
os.environ["DJANGO_SETTINGS_MODULE"] = "config.settings.sombra"
logging.disable(sys.maxsize)
with patch.object(os, "makedirs"):
    from config.settings import sombra
sombra.LOGGING_CONFIG = None

from manage import main

sys.argv = [
    "manage.py", "shell", "--settings=config.settings.sombra",
    "--no-imports", "--verbosity=0",
    "--command=from __main__ import sonda\nsonda()",
]
main()
'
```

## 26/11 10:39 · sessao b12082e302b7 · - · - · origem=Ronald (tmux)

A seção 7 corrige a mordida contra 08:00 inclusivo, saída diurna vazia e repetição da troca de `PAREI` pela mutação `if True`. O último caso testa o arquivo mutado porque a variável local `H` fica visível à chamada de `_roda` pelo escopo dinâmico do Bash. O teste final de liberação diurna reutiliza a árvore do mutante e chama o hook original. `bin/tests/test_orcamento_contexto.sh:112`, `:519`, `:529`, `:598`, `:617`, `:620`.

| Caso | Mordida e lacuna |
|---|---|
| (a) | Cobra `enqueue` válido, filtros, arquivo ausente e limites horários. Rejeitar todo `user` cujo conteúdo seja lista passa, pois a lista da fixture contém `tool_result` e é negativa. Um juiz duplicado sob outro nome também passa se produzir os mesmos resultados no exemplo. `bin/tests/test_orcamento_contexto.sh:494`, `:497`, `:502`, `:509`, `:519`. |
| (b) | Compara o recado diurno inteiro e a ausência de `CLEAR-NOITE`. O zero numérico diurno ainda fica sem assert. `bin/tests/test_orcamento_contexto.sh:529`, `:532`, `:541`. |
| (c) | Exige handoff, AVISO, evento, contagem, teto futuro, `None` com erro de abertura e timestamp inválido em linha com quatro tokens. Passa remover o corte inferior ou devolver zero para linha truncada com três tokens. `bin/tests/test_orcamento_contexto.sh:534`, `:549`, `:553`, `:555`, `bin/disjuntor.py:2121`. |
| (d) | Cobra dois marcos noturnos separados por um ALLOW comum, o evento de volta e a reabertura diurna. Os asserts de prefixo aceitam item incorreto no fim do recado e omissão do pedido literal de `PAREI` no recado diurno. `bin/tests/test_orcamento_contexto.sh:560`, `:563`, `:568`, `:573`. |
| (d2) | Cobra anúncio na entrada, ausência de repetição e novo fim diurno. O assert do anúncio termina no comando de handoff e aceita a omissão de AVISO e continuidade da fila. `bin/tests/test_orcamento_contexto.sh:576`, `:578`, `:580`, `:583`. |
| (g) | Cobra prioridade do recado da trava com topo `PAREI`, ausência de repetição durante a mesma tomada e troca quando ela solta. Não separa topo neutro com handoff novo nem disparo pendente. A primeira resposta é explicitamente um `siga` com a trava tomada. `bin/tests/test_orcamento_contexto.sh:586`, `:589`, `:590`, `:593`, `:596`. |
| (e) | Cobra troca, depois ALLOW comum, ausência de `CLEAR-LIBERA` e queda precisa do `if True` no segundo passo. Deixa de fora falha contida e nova escrita do `PAREI` na mesma noite. `bin/tests/test_orcamento_contexto.sh:598`, `:604`, `:617`. |
| (f) | Cobra ausência de dia apesar do último assistente recente. Trocar `>` por `>=` no limite de 30 min passa com exemplos de 25 e 40 min. `bin/tests/test_orcamento_contexto.sh:514`, `:623`, `:631`, `bin/orcamento_contexto.py:114`. |

O helper converte um ALLOW real, sem saída do hook, em `siga-aviso` a partir da trilha. `bin/tests/_saida_do_stop.sh:13`, `:15`, `bin/hook_stop_fila1.py:1480`. O selo do disjuntor restaura fala recente sintética e hora diurna padrão para C1 e ajusta âncoras dos dois mutantes da Escada. `bin/tests/test_disjuntor.sh:87`, `:98`, `:121`, `:129`, `:1723`. A sintaxe dos dois scripts passou em `bash -n`. Execução completa **[nao verificado]**, pois os scripts escrevem e apagam arquivos. `bin/tests/test_orcamento_contexto.sh:32`, `:551`, `:611`.

Em memória, `disjuntor.ler` devolveu `0` para trilha ausente, vazia, somente futura e linha `lixo hora CLEAR-NOITE` truncada. Devolveu `None` para `OSError` e a mesma linha com um quarto token. Contou a linha exatamente em `agora-86400` e exatamente em `agora`. `bin/disjuntor.py:2105`, `:2113`, `:2121`, `:2125`, `:2132`, `:2134`, `:2153`. Os contadores da `TRILHA` mantêm a semântica com arquivo estável. `_censo_24h` ainda reabre `TRILHA` para a contagem própria. `bin/disjuntor.py:2136`, `:2161`, `:2192`, `:2237`.

Em memória, o leitor de presença ainda declarou ausência com assistente datado de 40 min antes seguido de linha `user` truncada. Também declarou ausência com fala de 5 s antes seguida no arquivo por fala de 40 min antes. Última fala reconhecida sem timestamp e fala exatamente aos 30 min retornaram `None`. O leitor novo toma a última fala pela ordem do arquivo e reduz a janela lida de 4 MiB para 2.000.000 bytes em relação à v1. `bin/disjuntor.py:1370`, `:1376`, `:1391`, `bin/orcamento_contexto.py:65`, `:111`, `:118`.

O recado novo de DIFF/apply inclui instrução de handoff e AVISO no topo com a trava tomada. O script de handoff relata a trava e prossegue até escrever `HANDOFF-SESSAO.md`. `bin/hook_stop_fila1.py:1070`, `:1091`, `:1097`, `bin/handoff_sessao.sh:18`, `:251`, `:318`, `:327`.

## 26/11 10:39 · sessao b3f1530d333c · - · - · origem=Ronald (tmux)

- A duplicação do juiz de fala foi resolvida. B4 e CLEAR usam `_fala_do_ronald` via `ultima_fala`. Os outros leitores de `user` em `bin/` extraem o prompt do verificador B3 e o marco `/clear`. A Escada passa a considerar `enqueue`, inclusive quando seguido de `remove`. `bin/disjuntor.py:649`, `bin/disjuntor.py:1342`, `bin/disjuntor.py:1365`, `bin/disjuntor.py:1394`, `bin/disjuntor.py:1402`, `bin/orcamento_contexto.py:105`, `bin/orcamento_contexto.py:289`.
- Classificação da v1 parcialmente corrigida. Slash, bash iniciado por `<`, vazio e sidechain enfileirado são excluídos. Imagem sem texto e `AskUserQuestion` com `tool_result` continuam excluídos. O harness demonstrado no selo `Async agent launched successfully...` é aceito pelo predicado se chegar sem flags. A nova seleção escolhe a última linha em ordem de arquivo e limita a cauda a 2 MB. Em memória, gravei fala de 1 min e depois fala de 40 min e a janela abriu pela antiga. Formatos em produção [nao verificado]. `bin/disjuntor.py:472`, `bin/disjuntor.py:1348`, `bin/disjuntor.py:1365`, `bin/orcamento_contexto.py:113`, `bin/tests/test_disjuntor.sh:235`.
- Dois `except ValueError` novos em `ultima_fala`. Timestamp ilegível da última fala reconhecida produz `None` de dia. JSON truncado é pulado e uma cauda antiga ainda pode produzir ausência, confirmado em memória. OSError de dia já produzia `None`, e a janela do relógio precede o I/O. O hook interpreta esse mesmo `None` como volta comprovada, grava `CLEAR-VOLTA` e apaga as travas por estado. `bin/disjuntor.py:1376`, `bin/orcamento_contexto.py:95`, `bin/orcamento_contexto.py:107`, `bin/hook_stop_fila1.py:1071`.
- Contador parcialmente corrigido. OSError e linha reconhecida com timestamp ruim produzem `None` e o painel mostra `nao sei`. `lixo hora CLEAR-NOITE` e `CLEAR-NOITE despedida` produzem zero, pois falham na identificação antes do parse. ENOENT só mostra caminho ausente no ato. Trilha válida sem eventos na janela também produz zero legítimo. O intervalo efetivo inclui os dois extremos e exclui futuro. Para arquivo principal estável os filtros e contagens se mantêm, inclusive futuro e efeito de OSError. Cada trilha é aberta uma vez no laço de agregação, e o censo continua com leitura própria da TRILHA. `bin/disjuntor.py:2105`, `bin/disjuntor.py:2113`, `bin/disjuntor.py:2121`, `bin/disjuntor.py:2132`, `bin/disjuntor.py:2153`, `bin/disjuntor.py:2192`, `bin/disjuntor.py:2237`, `bin/disjuntor.py:2422`.
- Pendência documental permanece. A regra geral no 7b e o PreCompact automático já existem. O aval específico segue ausente da cópia de `PROMPTS.md`. Os cinco hashes de blob conferem com o anexo. Identidade Git do commit/pai, selo integral e comportamento de produção [nao verificado]. `CLAUDE.md:343`, `app/docs/PROMPTS.md:3005`, `.claude/settings.json:25`, `bin/tests/test_orcamento_contexto.sh:32`.

## 26/11 10:38 · sessao 09732ae72b93 · O30-F3-CONSTRUIR · construtor · origem=Code

A cura D1–D5 está na cópia sobre `0191ece77c2383771232eb4c449cc33dfc20055b`. O `HEAD` foi conferido e a entrega ficou no workspace sem commit, conforme a proibição expressa de `git commit` e de escrita em `.git`. [Base dos REDs](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:18)

**Prova local.** Passaram 44 testes de contrato sem banco e 37 testes estruturais com `SimpleTestCase` substituído por `unittest.TestCase`. Os cinco comandos RED contra a c3 e os 14 mutantes terminaram com `rc=1` pelas asserções esperadas, sem erros de execução. O diff acrescentou **0 `except`**, incluindo testes, e passou em `git diff --check`. Três testes estruturais dependentes de Django, a integração e a concorrência real no PostgreSQL ficam [nao verificado]. A certificação fica com o Code e o Opus. [Contrato F3](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:128) · [Mutantes](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:190) · [Selo estrutural](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_lote7_selo_escritor_acorda.py:296) [nao verificado]

### Decisões e REDs

| Item | Decisão e falha na c3 |
|---|---|
| **D1** | Criar, fixar e reabrir recusam referência que já tem referência. O RED mantém `C → B → A`, comprova a recusa e compara os bytes do ZIP com a entrega protegida pelo vínculo válido `B → A`. [Porta](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:283) · [RED](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:551) |
| **D2** | A porta trava raiz, linha pedida e irmãs antes de autorizar a troca própria. Na c3, uma irmã aberta recebia `200` mesmo quando uma irmã aprovada ou rejeitada lia o arquivo compartilhado. [Porta](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1181) · [RED](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:791) |
| **D3** | O upload próprio em ausência decidida devolve `409`, com ou sem comprovante gravado. O RED da aprovada sem arquivo exige zero gravações e confirma que o DP pode anexar. Na c3, o upload próprio devolvia `200`. [Registro da decisão](/home/ronald/wt-codex-o30-f3-construir-010143/app/docs/RELATO.md:7112) · [RED](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:769) |
| **D4** | O selo usa AST e exige, para `update_fields=<nome>`, lista ou tupla literal atribuída uma vez no mesmo escopo, sem mutação. Na c3, `append` e atribuição só dentro de `if` passavam sem achado. [Selo](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:85) · [REDs](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:316) |
| **D5** | O rejulgamento consulta o documento da raiz nos dois sentidos. A cura persiste via `save_base` o fim conferido pela cerca e preserva a duração legada da falta. O RED da c3 conferiu `26/09` e encontrou `28/09` gravado. [Comando](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/management/commands/rejulgar_status_por_tipo.py:21) · [Cura](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1923) · [REDs](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:688) |

### Comandos de reprodução

Execute em `app/`. Cada chamada do módulo de mutantes deve retornar `1`. [Seletores e alvos](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:66)

```bash
export PYTHONDONTWRITEBYTECODE=1
python3 -m unittest -q ponto.tests.test_atestado_f3_contrato

for caso in \
  red_c3_d1 d1_insert_achata d1_insert_ignora_raiz_relida d1_fixar_aceita_filha \
    d1_reabrir_achata d1_zip_le_filha d1_historico_le_filha \
  red_c3_d2 d2_so_linha_pedida \
  red_c3_d3 d3_permite_primeiro_arquivo \
  red_c3_d4 d4_append d4_atribuicao_em_ramo \
  red_c3_d5 d5_rejulga_le_filha d5_inverso_le_filha \
    d5_ignora_fim_cercado d5_minutos_usam_fim_novo
do
  python3 -m ponto.tests.mutantes_atestado_f3 "$caso"
done
```

### Arquivos alterados

- Portas e leitores. [ausencia.py:283](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:283) · [atestados_lote.py:99](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/atestados_lote.py:99) · [dia_decidido.py:653](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/dia_decidido.py:653) · [views_atestado.py:30](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views_atestado.py:30) · [painel_ausencia.html:2](/home/ronald/wt-codex-o30-f3-construir-010143/app/templates/ponto/partials/painel_ausencia.html:2) · [ausencia_tags.py:6](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/templatetags/ausencia_tags.py:6) · [api/views.py:3087](/home/ronald/wt-codex-o30-f3-construir-010143/app/api/views.py:3087) · [rejulgar_status_por_tipo.py:21](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/management/commands/rejulgar_status_por_tipo.py:21)
- Censo e testes. [censo_escritas.py:399](/home/ronald/wt-codex-o30-f3-construir-010143/app/core/censo_escritas.py:399) · [test_lote7_selo_escritor_acorda.py:37](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_lote7_selo_escritor_acorda.py:37) · [test_atestado_f3_contrato.py:85](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:85) · [test_atestado_f3.py:151](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3.py:151) · [test_anexar_ausencia_colab.py:74](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_anexar_ausencia_colab.py:74) · [mutantes_atestado_f3.py:66](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:66) [nao verificado]

**Entrega protegida.** `servir_documento_ausencia` usa `a.documento_referencia or a`, suficiente sob a invariante de um elo. A sonda fornecida registra **0 cadeias legadas** de profundidade ≥ 2. Produção [nao verificado]. [ponto/views.py:2147](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2147)

**Fila.** Itens 9, 12 e 14 do `r_v3.txt`. Nome do arquivo no LogAuditoria e na ficha admin, storage sem tenant, vigência aberta anterior ao recorte, `TIPOS_EXIGEM_DOC` cru, JSON E4b, fuso de `decidido_em`, prazo de reabrir, cron de prazo sem releitura e mês civil como padrão do lote. A máscara da sombra fica com a sessão. [Anexo fornecido, itens 9, 12 e 14]

## 26/11 10:38 · sessao d29fee2cd5d2 · - · - · origem=Ronald (tmux)

- O caminho noturno normal retorna recados `siga` ou cai no fluxo comum. O `ALLOW` comum continua possível. Em exceção, o fallback ainda libera pelo CLEAR com `PAREI` e handoff recente, independentemente da janela. `bin/hook_stop_fila1.py:1082`, `bin/hook_stop_fila1.py:1125`, `bin/hook_stop_fila1.py:1234`, `bin/hook_stop_fila1.py:1250`, `bin/hook_stop_fila1.py:1436`, `bin/hook_stop_fila1.py:1480`.
- Com `sombra.lock` tomada, sai um `siga` explícito para concluir o DIFF/apply, sem abrir outro, já incluindo instrução de handoff e AVISO. A guarda veta troca/fim nesse ramo, mas só sonda a trava e a solta antes de responder. Uma aquisição concorrente pode atravessar a decisão. `bin/hook_stop_fila1.py:1070`, `bin/hook_stop_fila1.py:1089`, `bin/hook_stop_fila1.py:1096`, `bin/disjuntor.py:410`, `bin/disjuntor.py:421`, `bin/hook_stop_fila1.py:1439`.
- Reproduzi em memória `siga` de fim noturno três vezes para o mesmo mtime futuro. O `>=` verifica o mtime contra um limite atualizado para o instante do Stop. O código também não distingue handoff de marco real de outro acionamento do script. `bin/hook_stop_fila1.py:929`, `bin/hook_stop_fila1.py:999`, `bin/hook_stop_fila1.py:1110`, `bin/handoff_sessao.sh:320`, `.claude/settings.json:14`.
- Reproduzi em memória disparo diurno, PAREI diurno que libera com handoff sem passagem por topo neutro e troca noturna desse PAREI. Após a troca, o handoff diurno antigo ainda produziu um falso fim de marco noturno. Se a linha PAREI antiga persistir até a volta, também pode liberar de dia com o mesmo handoff. Nessa rota diurna direta, `fim_dia_em` nunca foi gravado. `bin/hook_stop_fila1.py:1076`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1131`, `bin/hook_stop_fila1.py:1161`, `bin/hook_stop_fila1.py:1184`.
- A volta agora apaga `fim_visto` e reinicia o relógio do PAREI, curando o fluxo nominal. O limite de handoff é dos Stops anteriores, inclusive para permitir par escrito no próprio turno, e não da fala do Ronald. Fora do horário, presença e transcript ilegível resultam igualmente em `None`; o Stop pode registrar `CLEAR-VOLTA` e rearmar recados quando a leitura se recuperar. `bin/orcamento_contexto.py:95`, `bin/orcamento_contexto.py:104`, `bin/hook_stop_fila1.py:1071`.
- Disparo pendente aguarda guarda livre, preserva o motivo original apenas quando não há gatilho atual e inicializa a rota noturna no Stop que o arma. Marco novo encerra o ciclo antes da avaliação da janela, com `ATENDIDO`/`SEGUIU` para clear conforme os instantes ou `RESET` sem clear, e o novo ciclo pode disparar no mesmo Stop. `bin/hook_stop_fila1.py:1025`, `bin/hook_stop_fila1.py:1032`, `bin/hook_stop_fila1.py:1059`, `bin/hook_stop_fila1.py:1194`, `bin/hook_stop_fila1.py:1205`, `bin/hook_stop_fila1.py:1218`.
- No selo, (g) deixa passar `if _g.startswith('DIFF/apply') and _eh_clear(linha):`, pois sempre testa a trava ocupada com PAREI no topo. O mutante emitiria fim noturno com topo neutro, trava ocupada e handoff elegível. O (e) usa efetivamente o hook mutante pelo `local H` do Bash e checa queda no passo 2. `bin/tests/test_orcamento_contexto.sh:112`, `bin/tests/test_orcamento_contexto.sh:587`, `bin/tests/test_orcamento_contexto.sh:598`, `bin/tests/test_orcamento_contexto.sh:617`, `bin/hook_stop_fila1.py:1091`, `bin/hook_stop_fila1.py:1110`.

## 26/11 10:37 · sessao 61488b6052a3 · ABONO-X-SAIDA-ANTECIPADA · censo · origem=Code

# ABONO-X-SAIDA-ANTECIPADA

A falha está na apuração da pontualidade. O motor limita **atraso + saída antecipada** pelo déficit diário antes do abono parcial. O fechamento e a lavratura `motor` do DiaPago usam o crédito parcial só no atraso. A 8069 soma os dois valores que recebe do fechamento. `app/ponto/motor_calculo_v2.py:1102–1122,1187–1213` · `app/ponto/services/fechamento.py:1013–1026` · `app/ponto/services/dia_pago.py:173–195` · `app/folha/export.py:383–399,449–452`

O anexo registra **22,23 h de excesso estimado na comp. 11** e **3,74 h na comp. 12**. No caso col306, em 24/11, `660 − 474 − 186 = 0 min`, com saída antecipada registrada de `3,089 h` no motor e `3,0889 h` no oráculo. A emissão dessas horas em TXT entregue e a identidade do conteúdo da cópia com o hash informado ficam [não verificado]. `MEDIDA.md:14,17,19` (anexo colado, contando a partir do título)

## 1. Mapa dos dois campos até o TXT

| Etapa | `horas_atraso` e `horas_saida_antecipada` | Fonte |
|---|---|---|
| Nascimento em minutos | `aplicar_tolerancia` calcula atraso como entrada real menos marco e antecipada como marco menos saída real. `_julgar_pontualidade` grava os dois no período. A Zona 5 do turno partido reassocia os marcos e regrava ambos. O tratamento de vários blocos sem marcos de intervalo retira a cobrança das pontas internas. | `app/ponto/motor_calculo_v2.py:1986–2026,2231–2247,2722–2732,3077–3099` |
| Decisão diária | `_aplicar_teto_pontualidade` agrupa por dia da jornada, zera ambas as pontas em dia integralmente abonado ou turno aberto/par nulo e limita a soma a `max(0, previsto − trabalhado real)`, com redução proporcional. Leva ao trabalhado os minutos fora da janela que o período registrou à parte. O teto é chamado pela base, pelo turno partido e pelo comercial. | `app/ponto/motor_calculo_v2.py:1095–1122,1163–1218,2734,3214,3528` |
| Agregação mensal | `apurar_por_fatia` concatena períodos e mescla o mapa de jornada. As propriedades de `ResultadoMes` somam cada campo em `periodos` e dividem por 60. `periodos_ft` fica fora das duas somas. | `app/ponto/services/fechamento.py:298–336` · `app/ponto/motor_calculo_v2.py:346–360` |
| `FechamentoMensal` | `creditos_por_dia` → `atraso_por_dia` → `abater_no_proprio_dia`. `horas_atraso` recebe o total menos os minutos usados, com piso zero. `horas_saida_antecipada` recebe o total do motor. Ambos são arredondados a duas casas. O serviço devolve o objeto calculado em modo leitura ou persiste por `save`/`update`. | `app/ponto/services/fechamento.py:1018–1026,1152–1168,1184–1196` |
| `DiaPago` | A versão `motor` soma a antecipada pelo mapa de jornada e refaz `min(atraso, crédito)` no atraso a partir dos mapas recebidos. Na versão `oraculo`, quando `Rubricas.valores` afirma as duas pontas, seus valores **sobrescrevem** os dois campos, inclusive o atraso que já tinha recebido crédito na montagem inicial. | `app/ponto/services/fechamento.py:1224–1241,1289–1319` · `app/ponto/services/dia_pago.py:173–195,322–330,345–360` · `app/ponto/calculador/regras.py:184–188,853–862` |
| Emissão | `gerar_dominio_txt` apura `FechamentoMensal` em memória e entrega os mesmos objetos à classificação e à montagem. `montar_linhas_export` também aceita o gravado. `eventos_do_fechamento` chama `rubrica_atraso_parcial`, que soma os dois campos para `atraso_diurno`, evento da 8069. A montagem usa a rubrica ativa empresa/global, pula valor zero e serializa a linha 10. A homologação lê a mesma lista de eventos. | `app/folha/views.py:209–214,267–287` · `app/folha/export.py:34–45,383–399,449–452,525–539,714–739,984–1008` |

A montagem comum também atende o PDF de validação, o vigia, a regeneração, a simulação e o comando de materialização do passivo. O DiaPago participa de uma conferência separada da porta, que confronta **atraso** com o badge. O valor da linha 8069 sai dos campos do fechamento. `app/folha/validacao_pdf.py:61–67` · `app/folha/services/vigia_txt.py:232` · `app/folha/management/commands/regerar_txt_dominio.py:85` · `app/folha/management/commands/simular_folha.py:134` · `app/chamados/management/commands/materializar_passivo_validado.py:293` · `app/folha/porta_export.py:251–282,449` · `app/folha/export.py:449–452`

## 2. Censo do abono parcial

| Sítio | Escrita e leitura |
|---|---|
| `Ausencia.minutos` e `credito_parcial.py` | A criação recebe os minutos e a edição os grava. Para folha, entram ausências **aprovadas**. O predicado de parcial exige tipo habilitado no catálogo, um só dia e `0 < minutos < 480`. `creditos_por_dia` soma todas as parciais pagas elegíveis em `a.data`. `atraso_por_dia` agrupa pela **data civil da entrada**. `abater_no_proprio_dia` usa até o atraso daquele dia e a sobra segue como abono, sem saldo de crédito para atraso. `app/ponto/services/ausencia.py:483–491,654–659` · `app/ponto/turnos.py:2528–2552,2599–2619` · `app/ponto/services/credito_parcial.py:15–61` |
| `motor_calculo_v2.py` e `calculador/regras.py` | O motor inicializa o mapa de inteiras e o lê no teto. Esse mapa exige cobertura aprovada, efeito `ABONA` e ausência que explica o dia inteiro. O oráculo recebe a inteira, empresta o mesmo teto do motor e publica os dois valores em horas. A releitura de períodos também chama `pontualidade_do_dia` e grava as duas pontas no período portador. Essa chamada passa marcos e trabalhado, sem insumo de ausência. `app/ponto/motor_calculo_v2.py:805–808,1102–1122` · `app/ponto/turnos.py:2731–2758` · `app/ponto/calculador/alimentacao.py:514–532` · `app/ponto/calculador/regras.py:112–120,170–195,264–275,853–862` · `app/ponto/calculador/periodo.py:434–477,786–813` |
| `fechamento.py` e `dia_pago.py` | O fechamento pula a parcial paga na montagem de `datas_justificadas`, alimenta o mapa de inteiras nos motores da fatia e de órfãos e o repassa ao oráculo. O abatimento parcial fica nos sítios do mapa acima. **`minutos_abonados` é outro número**. `por_dia_da_grade` reconstitui o déficit `previsto − min(realizado, previsto)` em linha de tipo `ausencia`, e alimenta FM e DiaPago. Isso separa o abono projetado da grade dos minutos declarados que entram em `creditos_por_dia`. `app/ponto/services/fechamento.py:700–721,756–771,837–849,1018–1026,1084–1088,1227–1241,1295–1309` · `app/ponto/services/dia_pago.py:285–292,386–402` |
| `relatorios/` | O PDF lê o total escalar `min_parcial_pagos` de `efeitos_da_ausencia` para `horas_abono` e alimenta o motor com o mapa de inteiras. O abatimento próprio de atraso foi retirado. No resumo da competência, o cartão lê FM e, na falta dele, a lavratura. Para conferir a 8069, chama a mesma função do TXT. O extrato lê atraso agregado, e `furos.py` lê a cobertura parcial para mostrar marcos faltantes. `app/ponto/services/efeito_ausencia.py:40–65` · `app/relatorios/pdf_espelho.py:332–336,403–406,478–479,516–520,624–628,912–915` · `app/relatorios/cartao_pela_celula.py:102–118,181–190,270–273,321–346` · `app/relatorios/views.py:520–531` · `app/relatorios/furos.py:190–213` |

## 3. Origem pela LEI-AKITA 1

**A origem do desconto diário é `MotorBase._aplicar_teto_pontualidade`.** Ele tem as duas pontas, o dia da jornada, o previsto e o trabalhado. O oráculo já lê esse mesmo método. A alimentação pode vir de fora, pelo mapa diário de crédito que já existe, mas o teto atual só lê o mapa de inteiras e o previsto. `CLAUDE.md:17–22,177–180` · `app/ponto/motor_calculo_v2.py:1041–1070,1095–1122,1187–1213` · `app/ponto/calculador/regras.py:112–120,264–275` · `app/ponto/services/credito_parcial.py:15–29`

| Opção | O que muda |
|---|---|
| **Alimentar o teto com o crédito diário** | O mesmo juiz passa a considerar a parcela paga. FM e DiaPago passam a agregar o resultado já decidido. Os dois abatimentos atuais precisam ser reconciliados com essa decisão para evitar consumir de novo o mesmo crédito. Extrair a conta para uma função externa preserva a autoridade se o teto delegar a ela. As duas formas exigem ligação no motor e `!` específico. `app/ponto/motor_calculo_v2.py:1102–1122,1187–1213` · `app/ponto/services/fechamento.py:1018–1026` · `app/ponto/services/dia_pago.py:188–195` · `CLAUDE.md:142–143,269–274` |
| **Mudar só `efeito_ausencia` ou a alimentação atual** | O total mensal `min_parcial_pagos` alimenta o resumo de abono do PDF. Colocar parcial no mapa de inteiras zera ambas as pontas por completo. Reduzir `_previsto_min` alteraria também a jornada usada em outras contas. Nenhuma dessas entradas expressa o crédito parcial no teto atual. `app/ponto/services/efeito_ausencia.py:46–65` · `app/relatorios/pdf_espelho.py:332–336,406,479` · `app/ponto/motor_calculo_v2.py:1102–1122,1244–1277,1279–1311` |
| **Abater depois, em FM, DiaPago, relatório ou TXT** | Corrige apenas os consumidores alcançados por esse abatimento. O teto e seu leitor oráculo continuam produzindo o veredito anterior. Isso deixa duas decisões sobre a mesma pontualidade. O `!` histórico de `JUSTIFICATIVA-PAGA-O-PERIODO` tinha escopo de uma mudança para ler a fonte do período pago. `CLAUDE.md:17–22,271–274` · `app/ponto/calculador/regras.py:184–188,264–275` · `app/folha/export.py:383–399` · `app/docs/CORTES.md:479,496` |

## 4. Leis herdadas

> a justificativa aprovada paga o que FALTA para o trabalhado do dia chegar ao previsto, nunca mais que isso e nunca negativo; todo trabalho do dia entra na soma, inclusive turno curto anterior; o que passar do previsto NAO e pago pela justificativa, vai para a lista do admin como hora alem do turno (porta da HE). Nenhum dia vira falta nem perde DSR por causa desta regra. Esqueci_saida aprovada sem saida informada segue a mesma conta. O motor e o periodos_justificados leem o MESMO juiz: um numero so.

`app/docs/CORTES.md:558` — **JUSTIFICATIVA-COMPLETA-ATE-O-PREVISTO**, unidade dia-colab.

| Registro | Texto aplicável |
|---|---|
| **L-093 e L-084** | O teto limita a **soma diária** ao que faltou e preserva a proporção ao reduzi-la. Previsto zero zera a cobrança e previsto `None` deixa o dia sem teto. Na L-084, as **duas** pontas a mais de três horas dos marcos afastam a cobrança por pontualidade. Uma só ponta distante pode descontar. `app/docs/LEIS.md:111,116` |
| **L-095, L-097, L-085 e L-089** | Dinheiro lavrado uma vez e lido pelas testemunhas. A emenda da L-097 conta a saída após o marco no trabalhado e no noturno e mantém a classificação da HE. A jornada pertence ao dia de início. O efeito financeiro da ausência alcança dia de trabalho previsto. `app/docs/LEIS.md:113,115,117,121` |
| **Saída antecipada** | O corte manda `saida_antecipada = DESCONTA em minutos`, com rótulo via `palavra_do_dia`. A semente da cópia ainda traz `SUPRIME` para esse tipo de lançamento e o código lê o catálogo vigente. O efeito cadastrado em prod fica [não verificado]. O dossiê da HE declarada fixa saída no marco como zero HE e zero antecipada. `app/docs/CORTES.md:345` · `app/ponto/catalogo/ausencias.py:86–92,278–295` · `app/docs/DOSSIES.md:182–190` |
| **Abono e crédito parcial** | B-B21 confirma que o abono cobre só o previsto que as batidas deixaram descoberto. Os cortes de cobertura distinguem `Ausencia` parcial de `Justificativa`, exigem período declarado para julgar cobertura e ancoram a parcial noturna no início do turno. O corte do crédito no próprio dia está registrado no serviço, com abatimento de atraso. O aval de 27/10 registra “parcial so se os minutos ja saem de juiz existente”. `app/docs/CORTES.md:88,420,455,458,462,473` · `app/ponto/services/credito_parcial.py:1–10,50–61` · `app/docs/RELATO.md:7039` |

## 5. Matriz RED

Para testar o **teto líquido usado na estimativa do anexo**, sejam `P` o previsto, `T` o trabalhado real, `B` os minutos pagos no dia e `A/S` o atraso e a antecipada antes do teto. A soma esperada nesse contrato é `min(A + S, max(0, P − T − B))`. Isso combina o limite diário da L-093 com o saldo após abono. A aplicação desse contrato a toda `Ausencia` parcial é a questão 1 abaixo. A matriz é uma especificação de RED por leitura. Resultado de execução [não verificado]. `app/docs/LEIS.md:111` · `app/docs/CORTES.md:420,473,558` · `MEDIDA.md:5,19` (anexo)

| Caso | Resposta |
|---|---|
| **`B < S`** | Com `A=0`, `P−T=S=120` e `B=60`, ficam **60 min** de antecipada. Se `P−T=180` com os mesmos `S` e `B`, o teto permite os **120 min**. Comparar `B` e `S` isoladamente não determina o saldo. Hoje a saída passa pelo fechamento sem o crédito em ambos os casos. `app/docs/LEIS.md:111` · `app/docs/CORTES.md:420,558` · `app/ponto/services/fechamento.py:1022–1026` |
| **`B = S`** | Com `A=0` e `P−T=S=B=120`, ficam **0 min**. O caso col306 tem o mesmo saldo zero pelos campos raspados. `app/docs/LEIS.md:111` · `app/docs/CORTES.md:420,558` · `MEDIDA.md:19` (anexo) |
| **`B > S`** | Com `A=0`, `P−T=S=120` e `B=180`, ficam **0 min**. A sobra não cruza dia. `app/docs/CORTES.md:558` · `app/ponto/services/credito_parcial.py:50–61` |
| **Abono cobre atraso e saída** | Com `P−T=120`, `A=30`, `S=90` e `B=120`, o teto líquido dá **0/0**. Hoje, com o crédito na chave do próprio dia, FM e DiaPago `motor` dão **0/90** nesse cenário. `app/docs/LEIS.md:111` · `app/ponto/services/fechamento.py:1022–1026` · `app/ponto/services/dia_pago.py:185–195` |
| **Ausência inteira** | Com cobertura aprovada e efeito `ABONA`, já resulta **0/0** e o trabalho segue contando. O teste existente exige os zeros nas duas versões do DiaPago e no FM. `app/ponto/turnos.py:2741–2753` · `app/ponto/motor_calculo_v2.py:1102–1122` · `app/ponto/tests/test_abono_dia_inteiro_zera_pontualidade.py:89–104` |
| **Noturno cruza meia-noite** | Aplicar o saldo ao **dia de início da jornada**, inclusive à saída e à volta do intervalo em D+1. O RED precisa cobrir um período cuja entrada civil seja D+1 e cujo `dia_da_jornada` seja D. Hoje a saída lavrada lê o mapa da jornada, enquanto o agrupamento do atraso para crédito usa a entrada civil. `app/docs/LEIS.md:117` · `app/docs/CORTES.md:473` · `app/ponto/motor_calculo_v2.py:2415–2448,2474–2476` · `app/ponto/services/credito_parcial.py:32–47` · `app/ponto/services/dia_pago.py:109–132,173–195` |
| **12x36** | Com escala e marcos, passa pelo mesmo juiz. Usar o previsto daquele dia, sem fixar 720 min. Em folga o previsto zero zera a pontualidade. O `Motor12x36` sem escala vinculada fica fora do julgamento por falta de marco. A classificação vigente da parcial continua com limite `<480 min`. `app/ponto/motor_calculo_v2.py:1220–1270,2734,3560–3577,3660–3678` · `app/ponto/calculador/regras.py:486–502` · `app/ponto/turnos.py:2599–2619` · `app/docs/LEIS.md:111` |

## 6. Padrão realizado

**`padrao=juiz unico`** — *Single Source of Truth / um juiz por pergunta*. O teto compartilhado já realiza o padrão para a pontualidade bruta. A mesma decisão precisa alcançar a pontualidade líquida usada nas projeções `DiaPago` e `FechamentoMensal`. `app/docs/PLANO-PADROES.md:53–62` · `app/docs/GLOSSARIO.md:11–12,25` · `app/ponto/calculador/regras.py:112–120,264–275`

## 7. Perguntas de lei abertas

1. **Qual valor entra como parcela paga da `Ausencia` parcial?** O corte de 14/11 nomeia `Justificativa`; o código tem `Ausencia.minutos` por dia, período declarado para cobertura e `minutos_abonados` projetados da grade. O critério para usar os minutos declarados, exigir coincidência com a ponta coberta ou usar o abono projetado fica [não verificado]. `app/docs/CORTES.md:462,473,558` · `app/ponto/services/credito_parcial.py:15–29` · `app/ponto/services/dia_pago.py:386–402`
2. **Como o crédito histórico exclusivo do atraso convive com o teto líquido quando sobra desconto?** A L-093 já fixa a proporção **quando o teto reduz a soma**. O corte de 17/09 manda abater o atraso do próprio dia. Em `A=30`, `S=90`, `P−T=120`, `B=60`, aplicar somente o teto proporcional dá `15/45`; consumir primeiro 30 de crédito no atraso e limitar a saída ao saldo dá `0/60`. A precedência entre essas duas operações, assim como o abatimento obrigatório quando o teto líquido já comporta toda a pontualidade, fica [não verificado]. `app/docs/LEIS.md:111` · `app/ponto/services/credito_parcial.py:1–10,50–61` · `app/docs/CORTES.md:558`

## 26/11 10:37 · sessao f1c566c00d7c · - · - · origem=Ronald (tmux)

- Atraso e saída antecipada nascem em minutos nos períodos do motor. `_julgar_pontualidade` escreve as duas pontas, o turno partido reatribui pelos marcos e `_aplicar_teto_pontualidade` dá a palavra final por jornada. `ResultadoMes` soma `periodos` e converte para horas. [motor_calculo_v2.py](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/motor_calculo_v2.py:2233) `app/ponto/motor_calculo_v2.py:3092-3099,1102-1122,1187-1218,347-360`.
- `FechamentoMensal` abate crédito somente de atraso e arredonda as duas pontas. `DiaPago` motor repete `min(atraso, crédito)` por dia e grava saída direta. O oráculo usa o mesmo teto do motor e, se tem valores, a lavratura substitui as duas pontas pelos valores oraculares sem abatimento parcial. `app/ponto/services/fechamento.py:1018-1026,1161-1168,1184-1196,1224-1254,1289-1319`, `app/ponto/services/dia_pago.py:173-195,322-360`, `app/ponto/calculador/regras.py:115-120,184-195,264-275,855-862`.
- A porta HTTP recompõe os fechamentos em memória e passa os mesmos objetos ao classificador e à montagem. O montador também aceita FM gravado quando chamado sem injeção. O export soma as duas pontas em `atraso_diurno`/8069, consulta mapeamento de rubrica com precedência da empresa, emite a linha 10 centesimal e serializa CRLF. `DiaPago` participa do cotejo da porta, enquanto a 8069 lê FM. `app/folha/views.py:121-135,209-214,267-287`, `app/folha/export.py:34-45,383-399,449-452,525-539,717-739,984-1012`, `app/folha/porta_export.py:251-282,449-466`.
- O crédito soma `Ausencia.minutos` de aprovadas pagas que atendem ao juiz da parcial. A chave do crédito é `a.data`; o atraso correspondente é agrupado pela data **civil** da entrada. O teto e a saída do `DiaPago` usam o mapa do dia da **jornada**. Isso importa na volta de intervalo após meia-noite e identifica a divergência de chave para o RED. `app/ponto/turnos.py:2533-2553,2599-2619`, `app/ponto/services/credito_parcial.py:15-61`, `app/ponto/services/dia_pago.py:109-132,173-195`, `app/ponto/motor_calculo_v2.py:1072-1092`.
- `efeito_ausencia` entrega minutos parciais escalares ao PDF para `horas_abono`, além do mapa integral consumido pelo motor do PDF e da tela. O fechamento faz sua própria alimentação e usa o mapa diário de `credito_parcial`. O `minutos_abonados` de FM/DiaPago é outra conta, da grade, para `tipo_dia='ausencia'`, igual a previsto menos realizado com cap. O cartão lê FM e DiaPago, e usa a mesma soma 8069 para cotejar. `app/ponto/services/efeito_ausencia.py:46-65`, `app/relatorios/pdf_espelho.py:332-336,406,479,516-520,844-859,912-915`, `app/ponto/services/espelho.py:1104-1106,1169`, `app/ponto/services/fechamento.py:700-721,1023,1084-1088,1236`, `app/ponto/services/dia_pago.py:285-292,386-402`, `app/relatorios/cartao_pela_celula.py:102-103,118,181-186,270-273,321-346`.
- O precedente literal requer “parcial so se os minutos ja saem de juiz existente” e registra que a cura anterior deixou as parciais intocadas. A nova decisão no teto deve consumir o insumo, com os abatimentos descendentes reconciliados para evitar usar o mesmo crédito duas vezes. `efeito_ausencia` sozinho não chega ao teto. Enfiar parcial no mapa integral zera tudo, e adulterar `_previsto_do_dia` alcança duração, guarda de dia sem previsto e HE. Alterar o motor exige `!` explícito. `app/docs/RELATO.md:7039,7475-7479`, `app/ponto/motor_calculo_v2.py:1102-1122,1187-1218,1290,1470,1841`, `CLAUDE.md:17-22,143,271-274`.
- `CORTES.md:558` fala literalmente em `justificativa aprovada` e `Esqueci_saida`, ao passo que `:462-473` distingue cadastro de período da `Ausencia` parcial e a trilha da `Justificativa`. A antiga porta sem motor consta em `:479`, seguida por `JUSTIFICATIVA-B-NAO-FECHA` em `:496`, com `!` específico. O teto tem rateio proporcional documentado na L-093, mas o índice não explicita como combinar esse teto com a lei anterior de crédito somente sobre atraso. `app/docs/CORTES.md:462-473,479,496,558`, `app/docs/LEIS.md:111`, `app/ponto/services/credito_parcial.py:1-10,50-61`.

## 26/11 10:37 · sessao 061f53fdd671 · O327 · leitura · origem=Code

## 1. Porta que recusa a lavratura

O registro local da relavratura da competência 11 diz **93 colaboradores com “11 campos de dia sem dono”**. A lista dos 93, os argumentos de cada chamada e a existência de linhas antigas de `DiaPago` oráculo em produção ficam **[nao verificado]**. [app/docs/RELATO.md:174](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/RELATO.md:174)

O `raise ValueError` está em **`ponto/services/dia_pago.py:317`**. Com `rubricas is not None`, `lavrar` tira dos 15 `CAMPOS_DIA` a união dos campos declarados em `valores`, `nao_decide`, nos quatro mapas de falta e grade, e em `do_motor`. Qualquer campo que sobre dispara a recusa. O texto sobre “ancoramento do trecho extra” é fixo na mensagem, qualquer que seja a lista de campos. O `raise` vem antes do `delete` e do `bulk_create`, portanto uma tentativa recusada preserva eventual lavra anterior. [app/ponto/services/dia_pago.py:34,50–76,312–321,359–360]

O chamador é `recalcular_fechamento_mes`. A partir da competência 10/2026, ele chama `insumos_do_calculador` → `rubricas_por_dia` → `lavrar(rubricas=_rub_o, versao='oraculo')`, sem passar `do_motor`. O `except` registra `dia_pago oraculo: lavratura falhou` e o fechamento segue. [app/ponto/services/fechamento.py:468,1215–1225,1289–1326]

Na cópia, conferi a guarda com `rubricas={}` e os quatro mapas presentes. Ficam declarados `horas_falta`, `minutos_abonados`, `minutos_previstos` e `minutos_realizados`. Sobram **11** campos. Além dos dois splits noturnos, sobram `horas_atraso`, `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_folga_trabalhada`, `horas_intra_indenizada`, `horas_noturnas`, `horas_saida_antecipada` e `horas_trabalhadas`. [app/ponto/services/dia_pago.py:34–42,50–76; app/ponto/services/fechamento.py:873–878,1301–1318]

## 2. Lei escrita

- **Dia da jornada, L-085.** “**A JORNADA PERTENCE AO DIA DE INICIO**, inclusive para domingo, feriado, contagem de semana e DSR: hora depois da meia-noite herda o dia em que o plantao COMECOU, e nunca cria trabalho no dia seguinte”. [app/docs/LEIS.md:117]
- **Dia do marco da janela de HE, NOTURNO-A.** “a janela de HE ancora no DIA DO TURNO do par (data_jornada), nao no dia civil da entrada”. A L-097 emendada confirma que “o marco cai no dia do turno do juiz (L-085)”. [app/docs/CORTES.md:306,646; app/docs/LEIS.md:115]
- **Trecho usado para o split noturno, resposta de 02/10.** “na troca o calculador ancora o trecho extra **exatamente como o motor faz hoje** -- no fim da jornada de CADA PERIODO, pela mesma funcao (`he_noturna`); **a troca nao muda numero**.” O mesmo registro mantém “deveria ser no ultimo par do dia?” como pergunta em aberto. Em `DOSSIES.md`, a ocorrência pertinente lista as rubricas do DIFF. A resposta específica do trecho está em `PROMPTS.md` e também consta no código. [app/docs/PROMPTS.md:344; app/docs/DOSSIES.md:229–232; app/ponto/calculador/regras.py:414–424]

## 3. Onde os dois caminhos divergem e onde fica a cura

- **Dia.** O motor aplica NOTURNO-A em `_janela_do_dia`, que consulta `_dia_do_turno_do_par`. O oráculo agrupa os períodos pelo `resultado.dia_da_jornada`. [app/ponto/motor_calculo_v2.py:936–951,1545–1567; app/ponto/calculador/alimentacao.py:73–83]
- **Trecho.** Os dois chamam `he_noturna` por período. O motor passa `periodo.entrada` e `periodo.saida`, as pontas brutas. O oráculo passa `efetivo_entrada` e `efetivo_saida` em `he_pontas`. Como `he_noturna` usa a saída recebida como fim do trecho, as chamadas têm âncoras diferentes quando a janela desloca a saída. O ponto de alinhamento à resposta de 02/10 é **`segmentos_do_motor`**, na montagem de `he_pontas_por_dia`. [app/ponto/motor_calculo_v2.py:435–443,1031–1035,2767–2781; app/ponto/calculador/alimentacao.py:83–108; app/ponto/calculador/regras.py:819–839]
- **Recusa dos 11 campos.** O sítio da cura de cobertura é **`rubricas_por_dia`**, onde o universo vem de `v.minutos` mais os dias que o motor publicou como bloqueados. Se a janela fica sem itens, a função devolve `{}`. Já num dia visitado, a falta das pontas ou da prorrogação declara os dois splits em `nao_decide`, que a guarda aceita como declaração. Qual entrada deixou cada um dos 93 sem rubricas **[nao verificado]**. [app/ponto/calculador/alimentacao.py:449,454–475,514–533; app/ponto/calculador/regras.py:630–636; app/ponto/services/dia_pago.py:60–63]

## 4. Escritores e leitores dos dois splits

- **Escritores.** `MotorBase._aplicar_he_noturna` preenche os minutos no período. `ResultadoMes` soma esses minutos e `recalcular_fechamento_mes` grava as horas no `FechamentoMensal`, incluindo a parcela de folga-feriado no split de 100%. `dia_pago.lavrar` lê os minutos para a linha diária e, na versão oráculo, aplica os valores declarados por `regras.do_dia`. [app/ponto/motor_calculo_v2.py:338–344,1024–1039; app/ponto/services/fechamento.py:941–944,986–988,1184–1193; app/ponto/services/dia_pago.py:173–184,244–252,322–333,359–360]
- **Leitores.** `soma_da_competencia` e `soma_do_periodo` agregam os campos. O contador da porta de exportação compara a soma com `FechamentoMensal` campo a campo. `pago_do_dia` consulta os splits como sinal de dinheiro lavrado. `diff_calculador` lê `valores` e `nao_decide`, e `simular_folha` lê os campos mensais. O emissor do TXT usa os totais de HE e `horas_noturnas` para o adicional. [app/ponto/services/dia_pago.py:550–563,752,784–789,861–864,890–898; app/folha/porta_export.py:394,457–473; app/ponto/management/commands/diff_calculador.py:417–423,501–525; app/folha/management/commands/simular_folha.py:59–60,84; app/folha/export.py:420–447]

## 26/11 10:36 · sessao 1ba29fc65eb2 · - · - · origem=Ronald (tmux)

**Origem.** `MotorBase._aplicar_teto_pontualidade` fecha o desconto por dia de jornada nos três caminhos do motor. O oráculo chama o mesmo método. O teto atual zera as duas rubricas para ausência integral abonante e limita a soma a `max(0, previsto − trabalhado real)` nos demais dias, com redução proporcional. [app/ponto/motor_calculo_v2.py:1041](cur) [app/ponto/motor_calculo_v2.py:1102](cur) [app/ponto/motor_calculo_v2.py:1187](cur) [app/ponto/motor_calculo_v2.py:2734](cur) [app/ponto/motor_calculo_v2.py:3214](cur) [app/ponto/motor_calculo_v2.py:3528](cur) [app/ponto/calculador/regras.py:115](cur) [app/ponto/calculador/regras.py:270](cur)

**Entradas.** `creditos_por_dia` fornece os minutos pagos da parcial, por data. `efeitos_da_ausencia` fornece o total mensal parcial, além de datas justificadas, suprimidas e integrais. Nenhuma dessas entradas hoje leva os minutos parciais ao teto. Acrescentar essa leitura ao teto mantém o juiz e exige o `!` do motor. Mudar o mapa de integralidade zeraria ambas as rubricas mesmo com cobertura insuficiente. Alterar o memo de previsto mudaria a jornada que alimenta também outras contas. Corrigir no fechamento, na lavratura ou no relatório deixaria o motor respondendo outro valor. [app/ponto/services/credito_parcial.py:15](cur) [app/ponto/services/efeito_ausencia.py:28](cur) [app/ponto/motor_calculo_v2.py:1102](cur) [app/ponto/motor_calculo_v2.py:1244](cur) [app/ponto/motor_calculo_v2.py:1279](cur) [CLAUDE.md:17](cur) [CLAUDE.md:142](cur) [CLAUDE.md:271](cur)

**RED.** Pela leitura combinada de B-B21, L-093 e do corte da justificativa, o desconto agregado tem teto `max(0, previsto − trabalhado real − abono)`. Nos exemplos controlados com `previsto=660, trabalhado=540, atraso=0, saída=120`, abonos de `60`, `120` e `180` deixam saída de `60`, `0` e `0`. Com a mesma diferença e `atraso=30, saída=90`, abono `120` deixa ambas em zero. A comparação `abono` versus `saída` isolada não determina o saldo em dia arbitrário, porque o teto incide sobre a soma contra o déficit real. O caminho histórico do crédito é específico para atraso e precisa ser considerado ao compor a cura, evitando descontar o mesmo crédito duas vezes. [app/docs/CORTES.md:88](cur) [app/docs/CORTES.md:558](cur) [app/docs/LEIS.md:111](cur) [app/ponto/services/dia_pago.py:375](cur) [app/ponto/services/credito_parcial.py:50](cur)

- **Integral abonante:** zero em atraso e saída. O teste da cópia afirma ambos para Fechamento e DiaPago motor/oráculo. [app/ponto/turnos.py:2741](cur) [app/ponto/tests/test_abono_dia_inteiro_zera_pontualidade.py:89](cur) [nao verificado]
- **Noturno:** crédito e desconto recaem sobre o dia de início da jornada. O RED deve incluir retorno de pausa após a meia-noite, pois o mapa do crédito de atraso hoje usa a data civil da entrada do período, que pode divergir do mapa do motor usado para a antecipada. [app/docs/LEIS.md:117](cur) [app/docs/CORTES.md:473](cur) [app/ponto/motor_calculo_v2.py:2415](cur) [app/ponto/services/credito_parcial.py:32](cur) [app/ponto/services/dia_pago.py:109](cur)
- **12x36:** o plantão com escala e marcos usa o mesmo teto e o previsto líquido do dia. A folga tem previsto zero. A classe 12x36 sem escala não emite pontualidade por falta de marco. A classificação de parcial usa tipo habilitado, um dia e `0 < minutos < 480`, inclusive no 12x36. [app/ponto/motor_calculo_v2.py:2734](cur) [app/ponto/motor_calculo_v2.py:3560](cur) [app/ponto/motor_calculo_v2.py:3677](cur) [app/escala/utils.py:2284](cur) [app/ponto/calculador/regras.py:486](cur) [app/ponto/turnos.py:2599](cur)

**Recorte da lei.** `CORTES.md:558` nomeia literalmente a `Justificativa` aprovada e o juiz compartilhado por motor e `periodos_justificados`; `CORTES.md:473` distingue essa obra da `Ausencia` parcial. Para a `Ausencia`, B-B21 dá o princípio geral e o aval anterior registra “parcial so se os minutos ja saem de juiz existente”. O mapa referido acima já fornece esses minutos. [app/docs/CORTES.md:473](cur) [app/docs/CORTES.md:558](cur) [app/docs/RELATO.md:7039](cur) [app/ponto/services/credito_parcial.py:15](cur)

## 26/11 10:36 · sessao 345a448dbe92 · - · - · origem=Ronald (tmux)

- `JUSTIFICATIVA-COMPLETA-ATE-O-PREVISTO` fixa o dia-colab e diz literalmente “a justificativa aprovada paga o que FALTA para o trabalhado do dia chegar ao previsto, nunca mais que isso e nunca negativo”. Todo trabalho entra na soma. O excedente vai à porta da HE. Nenhum dia vira falta ou perde DSR pela regra. Motor e `periodos_justificados` leem o mesmo juiz. O corte B-B21 confirma o princípio para abono, “abono cobre so o previsto que as batidas nao cobriram”. `app/docs/CORTES.md:558`, `app/docs/CORTES.md:420`.
- L-093 limita a soma diária de atraso e antecipada, preserva a proporção quando o teto atua, zera com previsto zero e deixa seguir com previsto `None`. L-095 determina lavratura única do dinheiro, consumida por leitores. L-084 dispensa pontualidade se ambas as pontas distam mais de três horas dos marcos. L-097, com emenda de 25/11, manda contar o trabalho após o marco na duração trabalhada e no noturno e tratar o trecho separadamente para HE. L-085 mantém a jornada no dia de início. L-089 restringe efeito financeiro da ausência a dia de trabalho previsto. `app/docs/LEIS.md:111`, `app/docs/LEIS.md:113`, `app/docs/LEIS.md:115`, `app/docs/LEIS.md:116`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:121`.
- Saída antecipada “DESCONTA em minutos”, com rótulo via `palavra_do_dia` e zero novos juízes. Saída no marco tem zero HE e zero antecipada. `app/docs/CORTES.md:345`, `app/docs/DOSSIES.md:182-190`.
- Para cobertura, a parcial exige o período declarado e a justificação cobre tipo e período. O fim menor que o início cruza a meia-noite e pertence ao dia de início, com caso nominal 12x36 de Londrina. A borda na batida tem corte posterior de direção, com estado registrado e sem pouso. `app/docs/CORTES.md:455`, `app/docs/CORTES.md:458`, `app/docs/CORTES.md:462`, `app/docs/CORTES.md:473`, `app/docs/CORTES.md:648`.
- O corte histórico do crédito diz “a declaracao parcial paga abate o atraso DO PROPRIO DIA, nunca de outro”, sem saldo de crédito para atraso. Fechamento e DiaPago hoje descontam apenas o atraso com essa lógica. O aval anterior de dia inteiro cita “parcial so se os minutos ja saem de juiz existente”, com teste que exige zero para inteira e saída positiva para parcial de 150 minutos, sem afirmar a magnitude da parcial. `app/ponto/services/credito_parcial.py:1-10`, `app/ponto/services/credito_parcial.py:50-61`, `app/ponto/services/fechamento.py:1013-1026`, `app/ponto/services/dia_pago.py:185-195`, `app/ponto/tests/test_abono_dia_inteiro_zera_pontualidade.py:2-12`, `app/ponto/tests/test_abono_dia_inteiro_zera_pontualidade.py:89-104`.
- O aval histórico da porta externa previa justificativa aprovada como efeito parcial de ausência via `efeito_ausencia.min_parcial`. O aval seguinte permitiu uma mudança no motor para ler a fonte do período pago do PDF. Nesta cópia, a função soma `Ausencia.minutos` em um valor para o período e registra `Justificativa` aprovada como conjunto de datas. O teto consome apenas mapa de inteiras, previsto e realizado. Alterar `_previsto_min` contaminaria a duração prevista usada também por `_jornada_do_dia` e excedentes. `app/docs/CORTES.md:479`, `app/docs/CORTES.md:496`, `app/ponto/services/efeito_ausencia.py:28-65`, `app/ponto/motor_calculo_v2.py:1102-1122`, `app/ponto/motor_calculo_v2.py:1187-1213`, `app/ponto/motor_calculo_v2.py:1244-1253`, `app/ponto/motor_calculo_v2.py:1279-1311`, `app/ponto/motor_calculo_v2.py:1841`.
- Padrão **Single Source of Truth / um juiz por pergunta**. Para teto líquido único, mantendo a proporção da L-093, **0 perguntas de lei novas**. O `!` específico para editar o motor é autorização. Priorizar uma ponta ou prometer abatimento linear integral onde a pontualidade bruta difere do déficit do dia carece de regra demonstrada no material. `app/docs/GLOSSARIO.md:11-12`, `app/docs/PLANO-PADROES.md:53-59`, `app/docs/LEIS.md:111`, `app/docs/CORTES.md:558`, `CLAUDE.md:142-143`, `CLAUDE.md:271-279`.

_8343 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
