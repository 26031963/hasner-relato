# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 10:47** · publicado em **26/11/2026 10:52** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 26/11 10:33 · sessao 44f6f0b7dcf6 · PUXA-VARREDURA-2611 · leitura · origem=Code

| Veredito | Obra e linha no BACKLOG | Pré-requisito literal | Pousou? | [nao verificado]
|---|---|---|---|
| **PUXA** | **FERIADO-SEM-LIMITE-DO-SUPRIMIDO** — `BACKLOG.md:1998` | `NOTURNO-A` | Sim. A é `NOTURNO-A-JANELA-NO-DIA-DO-TURNO`, NO AR em 25/11. `BACKLOG.md:1988` e `BACKLOG.md:1989`, `pousos.txt:7` — `0d1e5ddf5`. |
| **PUXA** | NOTURNO-NAO-APURADO, etapa B RAMO2 — `BACKLOG.md:1988` | `A = NOTURNO-A-JANELA-NO-DIA-DO-TURNO` | Sim. `pousos.txt:7` — `0d1e5ddf5`. |
| **PUXA** | ESCRITOR-UNICO-DE-CHAMADO, volta 2 de CHAMADO-PRESO — `BACKLOG.md:832` | `A-TELA` | Sim. `A-TELA-DO-DIA-LE-UM-JUIZ`, NO AR em 25/11. `BACKLOG.md:1986`, `pousos.txt:4` — `4bac7b0f9`. |
| **PUXA** | RESOLVER-PASSA-NA-FRENTE — `BACKLOG.md:2023` | `E4C-F5`, `F5` e `A-TELA-DO-DIA` na ordem das laterais | Sim. `pousos.txt:13` — `ce4a9887e`. A-TELA consta NO AR em 25/11. `BACKLOG.md:1986` e `BACKLOG.md:2540`, `pousos.txt:4` — `4bac7b0f9`. |
| **PUXA** | PORTA-CONFERE-TRAVA — `BACKLOG.md:1985` | `E4C-F5` | Sim. `pousos.txt:13` — `ce4a9887e`. |
| **PUXA** | CRON-NAO-CABE-VAGA-POS-F5 — `BACKLOG.md:859`<br>GUARDA-LE-O-JUIZ — `BACKLOG.md:2021`<br>VIGIA-DO-TMUX, portão da F5 — `BACKLOG.md:2027`<br>EXPLICADOR-EM-LOTE — `BACKLOG.md:2054`<br>E4C-F5-SILENCIO-ADMIN — `BACKLOG.md:2055`<br>ATO-EM-ESCRITOR-UNICO — `BACKLOG.md:2056`<br>RAIZ-DA-DISPUTA-F1-RASCUNHO — `BACKLOG.md:2064`<br>RESTORE-CONFERE-RELOGIO — `BACKLOG.md:2154`<br>E4C-F5-INSTANTE — `BACKLOG.md:2155`<br>LOTE-MOTIVO-DO-SUCESSO — `BACKLOG.md:2173`<br>CADEIA-20-PASSO-1 — `BACKLOG.md:2190`<br>F5-BECO-PENDENTE-SEM-ACAO — `BACKLOG.md:2220` | `F5` | Sim. `E4C-F5` pousou em 23/11. `pousos.txt:13` — `ce4a9887e`. |
| **PUXA** | AUTO-CLEAR-NO-MARCO — `BACKLOG.md:2932`<br>CLAUDE-MD-ENXUTO-2 — `BACKLOG.md:2933`<br>MODELO-DO-SUBAGENTE — `BACKLOG.md:2934`<br>TRAVA-H03-TRILHA-POR-BLOQUEIO — `BACKLOG.md:2935`<br>TRAVA-D03-VEREDITO-FINAL-EFETIVO — `BACKLOG.md:2936`<br>TRAVA-PP05-CURA-P71-DECLARATIVA — `BACKLOG.md:2937` | `1o pouso de produto da E4C-F5 no ar` | Sim. `pousos.txt:13` — `ce4a9887e`. |
| **PUXA** | E4C-F8 — `BACKLOG.md:2180` | `F5, C7, B2b F0 e CLASSE-D` | Sim. `pousos.txt:13` — `ce4a9887e`, `pousos.txt:23` — `1c4e50fce` (C7 em `BACKLOG.md:2182`), `pousos.txt:11` — `7a4e79eef`, `pousos.txt:10` — `16630e80c` (CLASSE-D em `BACKLOG.md:2208`). |
| **PUXA** | PODA-7B — `BACKLOG.md:2046`<br>QUARENTENA-VENCIDA — `BACKLOG.md:2047` | `SIMPLIFICA-V3` | Sim. `SIMPLIFICA-V3-2211-1726` consta NO AR em 22/11. `BACKLOG.md:2044`, `pousos.txt:17` — `6f48ecf3b`. |
| **PUXA** | LATERAIS-EM-PARALELO, segunda pista — `BACKLOG.md:2540` | `PISTA-CAMADAS` | Sim. `pousos.txt:12` — `051e1b5b4`. |
| **PUXA** | DESLIGAR-LIQUIDA-TUDO — `BACKLOG.md:732`<br>DESLIGADOS-RETIDOS-CONTADOR — `BACKLOG.md:735`<br>RAIOX-DIAS-DERIVA — `BACKLOG.md:737`<br>DESLIGADOS-RETIDOS-UNIVERSO — `BACKLOG.md:739`<br>RETRATO-DOIS-RELOGIOS — `BACKLOG.md:740`<br>O50-DESLIGAR-ENCERRA-TUDO — `BACKLOG.md:1462` | `LEI-DESLIGADO`, `a lei` ou `o pouso`, conforme a linha | Sim. `LEI-DESLIGADO-NAO-TRAVA`, NO AR em 13/11. `BACKLOG.md:731`, `pousos.txt:35` — `9e2c54017`. |
| **PUXA** | HJ-091-FILA-1 — `BACKLOG.md:747`<br>HJ-091-FILA-2 — `BACKLOG.md:748`<br>HJ-091-FILA-3 — `BACKLOG.md:749`<br>HJ-091-FILA-4 — `BACKLOG.md:750` | `HJ-091` | Sim. `pousos.txt:32` — `0389cf649` e `pousos.txt:27` — `8376da7aa`. |
| **PUXA** | JANELA-NO-PRE-VOO — `BACKLOG.md:756`<br>RMDIR-FILA — `BACKLOG.md:765`<br>RMDIR-FILA-2 — `BACKLOG.md:766` | `RMDIR-COM-GUARDA` / `RMDIR` | Sim. `pousos.txt:28` — `8dbe7db20`. A linha da v5 nomeia a obra completa. `BACKLOG.md:766`. |
| **PUXA** | [nome]-FILA-PORTAO-MARCADOR — `BACKLOG.md:784` | `[nome]` | Sim. A linha identifica `[nome]-ANDAM-JUNTO` pela árvore `830146e72`, também identificada no pouso do anexo. `pousos.txt:30` — `2b0f888d5`. |
| **PUXA** | AUSENCIA-EFEITO-PELO-ESTADO — `BACKLOG.md:851` | `UI-VINCULO` | Sim. `UI-VINCULO-MOSTRA-O-CALENDARIO` consta NO AR em 16/11, `7849ee7d2`. `BACKLOG.md:842` e `BACKLOG.md:850`. |
| **PUXA** | CORTES-F2-F9-1611, itens 1 e 8 — `BACKLOG.md:862` | `F1 B1` | Sim. NO AR em 16/11, `3adf703e3`. `BACKLOG.md:873`. |
| **PUXA** | N1-ALIMENTACAO-UNICA — `BACKLOG.md:886` | `N1` | Sim. `N1-PERGUNTAS-DO-APP`, etapa (c)(1), NO AR em 04/11. `BACKLOG.md:884`. |
| **PUXA** | COBPARC-AUSENCIA-CALA-DOIS-JUIZES — `BACKLOG.md:904`<br>COBPARC-FUROS-LEITOR-PARCIAL — `BACKLOG.md:905`<br>COBPARC-FILA-MATERIALIZACAO — `BACKLOG.md:975`<br>COBPARC-FILA-CENSO-V7 — `BACKLOG.md:976`<br>COBPARC-FILA-V8-PARCIAL-DEMISSAO — `BACKLOG.md:977`<br>COBPARC-FILA-V8-VOCABULARIO — `BACKLOG.md:978`<br>EMIS-LEITORES-RESTANTES — `BACKLOG.md:1056` | `COBPARC` / `COBERTURA-PARCIAL-UM-JUIZ` | Sim. `BACKLOG.md:898`, `pousos.txt:46` — `97bcb01c3`. |
| **PUXA** | RETRATADA-CONVERSA-E-REABRIR — `BACKLOG.md:914`<br>E1-A2-VAZIO-MEDIR — `BACKLOG.md:915`<br>E1-RECUSAS-SEM-CONTADOR — `BACKLOG.md:916`<br>E1-DUPLICATA-SOBRE-RETRATADA — `BACKLOG.md:917` | `E1` | Sim. `FIO-E0-V4-E1-ENTRADAS`, núcleo NO AR em 08/11, `a8a692f55`, e linha FECHADA em 15/11. `BACKLOG.md:2246`. |
| **PUXA** | ODOMETRO-DE-REGRAS — `BACKLOG.md:941` | `CLAUDE-MD-RACIONAL` | Sim. ID explicitamente nomeado no comboio. `pousos.txt:59` — `5693e4622`. |
| **PUXA** | FASE-CONFLITANTE-VINCULO-DO-DIA — `BACKLOG.md:958` | `GRADE` | Sim. A separação dessa fatia consta na linha `GRADE-FOLGA-JUIZ`. `BACKLOG.md:957`, `pousos.txt:48` — `6f583cbc2`. |
| **PUXA** | CODEX-CUSTO-POR-OBRA — `BACKLOG.md:967`<br>CODEX-CONTEXTO-SEM-CONDUTA — `BACKLOG.md:1091` | `CLAUDE-MD-ENXUTO` | Sim. NO AR em 31/10, `9ce5181d`. `BACKLOG.md:1088`. |
| **PUXA** | E4-CAL-DIA-COM-FIM-LE-A-FOLHA — `BACKLOG.md:968`<br>PARCIAL-NA-PORTA — `BACKLOG.md:970` | `E4` / `E4-CALENDARIO-L1` | Sim. A primeira linha explicita que é complemento da L1. `pousos.txt:40` — `c960973b1`. |
| **PUXA** | CLAUDE-MD-RACIONAL — `BACKLOG.md:1090` | `B3` | Sim. B3 v4, `REPROVA-E-DESENHO` e `REPROVA-E-DESENHO-CODEX`. `BACKLOG.md:943` e `BACKLOG.md:950`, `pousos.txt:58` — `e1ed614dc`. |
| **PUXA** | V3-HE-NOTURNA-POR-PECA — `BACKLOG.md:1096` | `V3-P5` | Sim. `V3-P5-PRORROGACAO-POR-INTERVALO`, caminho b. `BACKLOG.md:1095`, `pousos.txt:50` — `16c27de8c`. |
| **PUXA** | O261-V-FASE-NA-LISTA — `BACKLOG.md:1127`<br>DIA-DA-PERGUNTA-E-O-TURNO — `BACKLOG.md:1153` | `pouso A do VETO` | Sim. A fatia A da `O261-VETO-NA-PORTA` consta NO AR em 30/10. `BACKLOG.md:1154`, `pousos.txt:60` — `af811fe3c`. |
| **PUXA** | ENTRADA-BLOQUEADA-SEM-DATA-TURNO — `BACKLOG.md:1176` | `JUSTIFICATIVA-ENCERRA-ORIGEM` | Sim. NO AR em 29/10, `77eb28ce`. `BACKLOG.md:1202`. |
| **PUXA** | GRUPO-1-DATA-SAIDA-ESPERADA — `BACKLOG.md:1190`<br>DETECTOR-SO-A-1A-PAUSA — `BACKLOG.md:1197` | `sitio 1` do `grupo (1)` | Sim. Pouso 1 do `JUIZ-DO-PAR-GRUPO-1` NO AR em 29/10, `9ed75130`. `BACKLOG.md:924`. |
| **PUXA** | PAUTAS-PREFILTRO-REPETIDO — `BACKLOG.md:1198` | `COL129` | Sim. `COL129-DONO-PELO-JUIZ`, NO AR em 29/10, `98dea532`. `BACKLOG.md:1203`. |
| **PUXA** | CONTRATO-20-E2-QUAL-ATO — `BACKLOG.md:1201` | `O-OUTBOX` | Sim. NO AR em 29/10, `e0bb9cf8`. `BACKLOG.md:808`. |
| **PUXA** | EXPIRACAO-DA-ABERTA-COPIA-ANTIGA — `BACKLOG.md:1204` | `ORFAS` | Sim. A linha `CONTRATO-20-LASTRO-E-ORFAS` registra a metade ORFAS em `ac721607` e consta FECHADA em 08/11. `BACKLOG.md:810`. |
| **PUXA** | O256 — `BACKLOG.md:1228` | `O261` e `complemento da O206` | Sim. O261 FECHADA em 24/10 e O206 NO AR em 09/10. `BACKLOG.md:1625` e `BACKLOG.md:1592`. |
| **PUXA** | O-HAIKU-INVESTIGA — `BACKLOG.md:1258` | `O-DIA-PAGO S4` | Sim. FECHADA em 30/09. `BACKLOG.md:1257`. |
| **PUXA** | E6-14 — `BACKLOG.md:1290` | `troca da S3` | Sim. `O-DIA-PAGO S3` FECHADA em 29/09. `BACKLOG.md:1252`. |
| **PUXA** | O214 — `BACKLOG.md:1300` | `O211` | Sim. FECHADA em 08/10, pousos A e B. `BACKLOG.md:1347`. |
| **PUXA** | O261-E-F2-FILA — `BACKLOG.md:1323`<br>O261-E-F2-FILA-V2 — `BACKLOG.md:1324` | `F2` | Sim. O texto das duas linhas nomeia `O261-E-F2`. `BACKLOG.md:1006`, `pousos.txt:47` — `a8709a532`. |
| **PUXA** | O127 — `BACKLOG.md:1352` | `O124` | Sim. FECHADA e NO AR em 03/10, `4088657e`. `BACKLOG.md:1361`. |
| **PUXA** | S5b-4-REGRAS — `BACKLOG.md:1418` | `O12` | Sim. FECHADO em 01/10. `BACKLOG.md:1395`. |
| **PUXA** | O192 — `BACKLOG.md:1578` | `O135` | Sim. NO AR em 04/10, `fb1c78ac`. `BACKLOG.md:1519`. |
| **PUXA** | O229 — `BACKLOG.md:1597` | `O146` | Sim. FECHADA e NO AR em 09/10, `cfd4ff83` e `ce212bb8`. `BACKLOG.md:1299`. |
| **PUXA** | O216 — `BACKLOG.md:1599` | `O195` | Sim. FECHADA em 05/10. `BACKLOG.md:1581`. |
| **PUXA** | O235 — `BACKLOG.md:1604`<br>O251 — `BACKLOG.md:1614` | `O207` | Sim. NO AR em 10/10, `353818c5`. `BACKLOG.md:1593`. |
| **PUXA** | O270 — `BACKLOG.md:1686` | `O263-FT` | Sim. FECHADA e NO AR em 13/10. `BACKLOG.md:1679`. |
| **PUXA** | V3-BATERIA-O-RESTO-DOS-38 — `BACKLOG.md:1796` | `O261` | Sim. FECHADA em 24/10. `BACKLOG.md:1625`. |
| **PUXA** | TERMOMETRO-VIRA-LISTA — `BACKLOG.md:1818` | `porta da REABRIR` | Sim. A linha nomeia `REABRIR-PERGUNTA-A-PREMISSA`, cuja porta consta NO AR no marco de 26/10, `bdfad18f`. `BACKLOG.md:1817`. |
| **PUXA** | O122-ETAPA-2 — `BACKLOG.md:1849` | `FECHAR` | Sim. `FECHAR-PERGUNTA-A-PREMISSA` consta NO AR em 27/10, `927d8a02`. `BACKLOG.md:1829`. |
| **PUXA** | F3-RECORTE-E-CAUDA, etapa B — `BACKLOG.md:1910` | `MODO-COMUM` | Sim. `MODO-COMUM-OUT-DO-LOTE` consta NO AR em 04/11, `fc62165a`. `BACKLOG.md:1074`. |
| **PUXA** | FIO-DUP — `BACKLOG.md:2082` | `O168` | Sim. A linha de O168 identifica o pouso de 13/11 pela referência `89a5b0d85`, que também está no anexo. `BACKLOG.md:1553`, `pousos.txt:34` — `d33c0a899`. |
| **PUXA** | CORTES-LOTE7-E-BUG-PAR — `BACKLOG.md:2207`<br>PRE-DESENHO-BUG-PAR-VOLTA, etapa de código — `BACKLOG.md:2325` | `R3 c17` / `R3` | Sim. `E3-RESTO-R3` c17 NO AR em 16/11, `f8ee770a5`. `BACKLOG.md:771`. |
| **PUXA** | BUG-PAR-PREPARA-VOLTA — `BACKLOG.md:2330` | `F1 B1 e R3` | Sim. F1 B1 NO AR em 16/11, `3adf703e3`, e R3 c17 NO AR em 16/11, `f8ee770a5`. `BACKLOG.md:873` e `BACKLOG.md:771`. |
| **PUXA** | MOTOR-LE-O-TURNO — `BACKLOG.md:2234`<br>RETRATO-POR-EVENTO — `BACKLOG.md:2457` | `E2 pousar com E3` / `E2+E3` | Sim, para o comboio citado. A linha da E2 registra o pouso com a E3 em 09/11, `7c16da320`. `BACKLOG.md:2259`. |
| **PUXA** | FIO-E0-V4-E4-ESCRITORES, etapa E4b — `BACKLOG.md:2273` | `E4a no ar` | Sim. E4a-1 NO AR em 09/11, `892159c5f`, e E4a-2 NO AR em 10/11, `613a51498`. `BACKLOG.md:2273` e `BACKLOG.md:2283`. |
| **PUXA** | TELA-DIZ-O-QUE-O-SERVIDOR-DISSE — `BACKLOG.md:2310` | `BUG-PAR` | Sim, para a volta indicada na ordem de 15/11. `BUG-PAR-VOLTA-AGORA` consta FECHADA em 17/11, com pouso `41f5648af` em 16/11. `BACKLOG.md:2329`. |
| **PUXA** | E4C-D04-D05 — `BACKLOG.md:2336`<br>TRIAGEM-OFFLINE-SEM-CAMERA — `BACKLOG.md:2370`<br>BO-0611-07-E4B-COBRE — `BACKLOG.md:2395` | `E4b` / `DRY da E4b` / `DRY E4b` | Sim. A linha `FIO-E0-V4-E4B` registra o DRY e o pouso com deploy em 12/11, `087fe758a`. `BACKLOG.md:2284`. |
| **PUXA** | O169-FECHADA, volta 5 — `BACKLOG.md:2426` | `E4a-2` | Sim. `FIO-E0-V4-E4A-2` NO AR em 10/11, `613a51498`. `BACKLOG.md:2283`. |
| **PUXA** | OTIMIZA-DUPLA, itens 1–5 — `BACKLOG.md:2490` | `DEPLOY-ELO` | Sim. `DEPLOY-[nome]-ELO-DA-CORRENTE` consta FECHADA e NO AR em 04/11, `af710af2`. `BACKLOG.md:1078`. |
| **NAO SEI** | FERIADO-1609-FILA — `BACKLOG.md:730` | `FERIADO`, no contexto de `FERIADO-USA-A-LEI-DE-1609` | [nao verificado]. O registro datado de `BACKLOG.md:1978` identifica `FERIADO-POR-POSTO`. |
| **NAO SEI** | OTIMIZA-1-COMPLEMENTO-CARGA — `BACKLOG.md:741`<br>JANELA-VIRA-SMOKE — `BACKLOG.md:758`<br>E3-RESTO-I3a — `BACKLOG.md:763`<br>E3-RESTO-I3b — `BACKLOG.md:764`<br>E3-STATUS-EXPORTADA — `BACKLOG.md:1070`<br>E3-ISENTO-INSUMOS — `BACKLOG.md:1082` | `E3` | [nao verificado] para o pouso de cada E3 referida. A linha da E3 v10 ainda registra o pouso como pendente. `BACKLOG.md:770`. | [nao verificado]
| **NAO SEI** | TRAVA-PELA-MORDIDA — `BACKLOG.md:755` | `CORTE-DAS-TRAVAS v5` | [nao verificado] para a v5. |
| **NAO SEI** | FRAC5-FILA — `BACKLOG.md:788` | `FRAC5` | [nao verificado]. O BACKLOG distingue o texto autorizado da cura guardada. `BACKLOG.md:787` e `BACKLOG.md:789`. |
| **NAO SEI** | PERGUNTA-SEM-SELO, itens 2 e 3 — `BACKLOG.md:807` | `FECHAR2` | [nao verificado]. |
| **NAO SEI** | O-PROJECOES — `BACKLOG.md:812` | `REJULGAR-COMP10-RESTO` | [nao verificado] como pouso. `BACKLOG.md:854` documenta o ato e as reversões. | [nao verificado]
| **NAO SEI** | EV-NA-BRECHA-1611, itens i1/i2/i3b — `BACKLOG.md:823` | `PREPARA`, com `todos.fim` | [nao verificado]. |
| **NAO SEI** | PERSPECTIVA-DA-ESPINHA — `BACKLOG.md:871` | `ENSAIO-DA-FOLHA-11` | [nao verificado] como pouso. `BACKLOG.md:867` registra as entregas do ensaio. | [nao verificado]
| **NAO SEI** | LEIS-INDICE-72-SEM-LINHA — `BACKLOG.md:875` | `B1`, no contexto `B1 c3` | [nao verificado] para a c3. `BACKLOG.md:873` registra F1 B1 c2. |
| **NAO SEI** | GRADE-POS-POUSO, item 1 — `BACKLOG.md:901` | `rebase do COBPARC` | [nao verificado] para o rebase exigido. O pouso da obra está em `pousos.txt:46` — `97bcb01c3`. |
| **NAO SEI** | MOTOR-PASSA-O-TURNO-A-VOLTA — `BACKLOG.md:925` | `grupo (1)` | [nao verificado] para o grupo inteiro. `BACKLOG.md:924` documenta o pouso 1 e deixa o 2 a seguir. | [nao verificado]
| **NAO SEI** | DISPARAR-PERGUNTAS-NO-CONTRATO-20 — `BACKLOG.md:926` | `E3-A e PAR v5` | [nao verificado] para a dupla literal. |
| **NAO SEI** | E4-APP-MAPA — `BACKLOG.md:998` | `leitores 1-3 da E4-CALENDARIO` | [nao verificado] para os três leitores. A L1 é o leitor 1. `BACKLOG.md:969`, `pousos.txt:40`. |
| **NAO SEI** | CONTENCAO-V2-HORA-DE-LINHA — `BACKLOG.md:1003` | `CONTENCAO` ganhar a vaga | [nao verificado]. `BACKLOG.md:1002`. |
| **NAO SEI** | O261-E-CELULA-20-UM-DONO — `BACKLOG.md:1006` | `CALENDARIO-HOJE` e `F5` para o aceite | Calendário NO AR em 30/10, `8c23399a`. Pouso da F5 interna da O261-E [nao verificado]. `BACKLOG.md:1157` e `BACKLOG.md:1006`. |
| **NAO SEI** | O261-E-F5B-JUIZ-PORTA-ALCANCAVEL — `BACKLOG.md:1008` | `F5` | [nao verificado]. A mesma linha traz `ANTES da F5` no texto e `depois da F5` no estado. |
| **NAO SEI** | FILA-POR-CAUSA-AS-OF — `BACKLOG.md:1046` | `F1/F5` | [nao verificado] para as duas fatias no contexto dessa linha. |
| **NAO SEI** | E4-CARGA-PARIDADE — `BACKLOG.md:1060` | `F2` | [nao verificado] para a F2 referida nessa linha. |
| **NAO SEI** | GRADE-09-PAUTA — `BACKLOG.md:1068` | `GRADE-v5` | [nao verificado] para a v5. `pousos.txt:48` identifica `GRADE-FOLGA-JUIZ vb`. |
| **NAO SEI** | E6-POR-DONO — `BACKLOG.md:1112` | `F1C4` | [nao verificado] para a fatia exata. `pousos.txt:52` e `pousos.txt:57` nomeiam partes distintas. |
| **NAO SEI** | FIO-DO-ADMIN-NO-ESTADO — `BACKLOG.md:1134` | `produto`, no contexto de `FIO-MORTO-DONO-NA-LISTA` | [nao verificado]. |
| **NAO SEI** | O261-VETO-NA-PORTA, lotes B e C — `BACKLOG.md:1154` | `NOTURNO` | [nao verificado] para a NOTURNO citada no aval de 30/10. |
| **NAO SEI** | SUITE-POR-IMPACTO — `BACKLOG.md:1166` | `CALENDARIO` e `E3` | [nao verificado] para os dois pré-requisitos na versão referida. | [nao verificado]
| **NAO SEI** | CODEX-CONTEXTO-DA-CASA — `BACKLOG.md:1168` | `CALENDARIO` e `F1/V-FASE` | [nao verificado] para o conjunto referido. |
| **NAO SEI** | O121-A1-VINCULO-CORRIGE-O-PASSADO — `BACKLOG.md:1171` | `marco em curso` e `VETO A` | VETO A pousou, `pousos.txt:60` — `af811fe3c`. Identidade e pouso do outro marco [nao verificado]. |
| **NAO SEI** | VIGENCIA-FUTURA — `BACKLOG.md:1183` | `complemento vinculo_do_dia do grupo (1)` | [nao verificado] para esse complemento. |
| **NAO SEI** | SELO-DIA-PELO-SIGNIFICADO — `BACKLOG.md:1184` | `#4` do grupo (1) | [nao verificado] para o item 4. |
| **NAO SEI** | RETRATO-PARADO-2010 — `BACKLOG.md:1220` | `RAIA C` | [nao verificado] para a cura dessa raia. |
| **NAO SEI** | VINCULO-LINHA-DO-TEMPO — `BACKLOG.md:1255` | `ESMERIL-2` | [nao verificado] para a obra inteira. |
| **NAO SEI** | UI-ANEXO-AUSENCIA — `BACKLOG.md:1277` | `catalogo` e `O30 F3` para HEIC | [nao verificado] para os dois marcos exigidos. |
| **NAO SEI** | O227 — `BACKLOG.md:1340` | `pouso de instrumento depois da O211` | [nao verificado] para o instrumento. A O211 consta FECHADA em 08/10. `BACKLOG.md:1347`. | [nao verificado]
| **NAO SEI** | O121 — `BACKLOG.md:1358` | `remedicao e O65` | [nao verificado] para o conjunto. `BACKLOG.md:1481`. |
| **NAO SEI** | O113 — `BACKLOG.md:1368` | `marco em curso da S5b` | [nao verificado] para o marco específico. |
| **NAO SEI** | O24 — `BACKLOG.md:1435` | `CARTAO=ESPELHO 6/6` | [nao verificado] para o marco 6/6. |
| **NAO SEI** | O30 — `BACKLOG.md:1441` | `CATALOGO-SAIDA-ANTECIPADA` | A linha de O30 diz `fechado 17:xx`. Pouso datado na linha da obra [nao verificado]. `BACKLOG.md:1439`. | [nao verificado]
| **NAO SEI** | O43 — `BACKLOG.md:1454` | `ESTEIRA-SECA` | [nao verificado]. |
| **NAO SEI** | O50-CELULA-APOS-DESLIGAMENTO, etapa PREPARA — `BACKLOG.md:1463` | `competencia_ja_paga` | [nao verificado] para o pouso do juiz. `BACKLOG.md:2229`. | [nao verificado]
| **NAO SEI** | O53 — `BACKLOG.md:1468` | `4 itens do bloco FILA` e `O37` para a fase 2 | [nao verificado] para o conjunto. |
| **NAO SEI** | O51b — `BACKLOG.md:1486`<br>O70 — `BACKLOG.md:1488` | `O68` | [nao verificado]. `BACKLOG.md:1485`. |
| **NAO SEI** | O87 — `BACKLOG.md:1500` | `push do E1` | [nao verificado] para o E1 de setembro citado nessa linha. | [nao verificado]
| **NAO SEI** | O82 — `BACKLOG.md:1513` | `deploy das sete curas` | [nao verificado] para o conjunto. | [nao verificado]
| **NAO SEI** | O190 — `BACKLOG.md:1576` | `cert-ast` | [nao verificado] para o pouso de `cert-ast`. `BACKLOG.md:1308` registra fechamento na raia. | [nao verificado]
| **NAO SEI** | RELATO-CURTO-SELO — `BACKLOG.md:1606` | `O219` e `pouso do instrumento do O228` | O219 NO AR em 10/10. Data do pouso do instrumento da O228 [nao verificado]. `BACKLOG.md:1327` e `BACKLOG.md:1331`. | [nao verificado]
| **NAO SEI** | O246 — `BACKLOG.md:1609` | `passo 2 da O48`, `RECALC-09` e `virada` | [nao verificado] para o conjunto. |
| **NAO SEI** | O252 — `BACKLOG.md:1615` | `item 3 (batida x um juiz)` | [nao verificado] para esse item. |
| **NAO SEI** | BO-B-DIAPAGO-SE-CONTRADIZ — `BACKLOG.md:1632` | `BO-A` | [nao verificado] para o ID exato. |
| **NAO SEI** | VARREDURA-VIRA-EVENTO-24-CRONS — `BACKLOG.md:1636` | `O168` e `O170 C1` | O168 pousou. Pouso de `O170 C1` [nao verificado]. `BACKLOG.md:1556`, `pousos.txt:34`. | [nao verificado]
| **NAO SEI** | ESPELHO-VERDADE-REMEDIDO — `BACKLOG.md:1653` | `K6/E1` | [nao verificado] para o conjunto. `BACKLOG.md:1637`. |
| **NAO SEI** | PORTAO-FATURAMENTO-RESTO — `BACKLOG.md:1669` | `lote EVENTO` | [nao verificado]. |
| **NAO SEI** | O264 — `BACKLOG.md:1680` | `parte B do BUG-LINHA-X-BADGE` | [nao verificado] para a parte B. `BACKLOG.md:1622`. |
| **NAO SEI** | O282 — `BACKLOG.md:1699` | `censo do bloco A item (3)` | [nao verificado] para o item específico. |
| **NAO SEI** | O319 — `BACKLOG.md:1749` | `plantio dos 1.019 (O317)` e `pouso do O318` | O318 NO AR em 18/10. Conclusão do lote de 1.019 [nao verificado]. `BACKLOG.md:1743` e `BACKLOG.md:1744`. | [nao verificado]
| **NAO SEI** | PAUTA-DIA-EM-ABERTO-USA-O-PACKER — `BACKLOG.md:1768` | `partir_em_pautas` estar no ar | A linha diz `pousa neste marco`. Pouso no formato de prova pedido [nao verificado]. | [nao verificado]
| **NAO SEI** | APLICACAO-CONVENCAO-SEM-VIGENCIA — `BACKLOG.md:1773` | `REGUA-DO-PERIODO` | [nao verificado] para o pouso exato. | [nao verificado]
| **NAO SEI** | O249-RETRATADOR-NO-DIA-IMPERFEITO — `BACKLOG.md:1823` | `comboio` | [nao verificado] para o comboio citado. |
| **NAO SEI** | DIA-DA-BATIDA-JUIZ-UNICO — `BACKLOG.md:1858` | `rejulgamento [nome]` | [nao verificado]. |
| **NAO SEI** | ESMERIL-V2 — `BACKLOG.md:1861` | `ESPURIA-TELA, F6F7, F5` | F6F7 consta NO AR e E4C-F5 consta no anexo. O pouso da `ESPURIA-TELA` no escopo da ordem [nao verificado]. `BACKLOG.md:2196`, `pousos.txt:9`, `pousos.txt:13` e `pousos.txt:16`. | [nao verificado]
| **NAO SEI** | BUG-LASTRO-CLASSE-2 — `BACKLOG.md

[truncado em 20000 de 26662 caracteres]

## 26/11 10:31 · sessao 53c8f45d4499 · ABONO-X-SAIDA-ANTECIPADA · sonda · origem=Code

**(a) Abono lavrado e saída antecipada > 0**

| Competência | Colabs | Dias | Soma de `excesso_lei` | Fonte |
|---|---:|---:|---:|---|
| 11/2026 | 5 | 9 | **22,23 h** | `sonda/anexos:19` |
| 12/2026 | 2 | 2 | **3,74 h** | `sonda/anexos:25` |

As referências `sonda/anexos` numeram o bloco fornecido a partir de `sonda_leitura`, na linha 1.

**(b) Sim.** `col306` aparece em **24/11/2026**, na competência **12/2026**, com **186 min de abono**, **3,09 h de saída antecipada**, **660 min previstos** e **474 min realizados**. As linhas `PROVA` mostram **3,089 h** no motor e **3,0889219444444445 h** no oráculo. O tipo da ausência de `col306` é [nao verificado]. (`sonda/anexos:22`, `sonda/anexos:26`, `sonda/anexos:27`)

**(c) O teto diário é calculado por `MotorBase._aplicar_teto_pontualidade`.** Ele limita a soma de atraso e saída antecipada a `max(0, prev - trab)`, usando como `trab` os minutos trabalhados mais os minutos fora da janela de HE. Se a soma excede o teto, reduz as duas rubricas proporcionalmente. **O teto não desconta abono parcial.** O mapa `dias_com_ausencia_inteira` só inclui ausências abonadoras que cobrem o dia inteiro. Quando há uma entrada nesse mapa, o método zera as duas rubricas. O oráculo usa o mesmo método por meio de `_ContextoPontualidade`. (`app/ponto/motor_calculo_v2.py:1041`, `app/ponto/motor_calculo_v2.py:1102–1122`, `app/ponto/motor_calculo_v2.py:1187–1213`, `app/ponto/turnos.py:2738–2753`, `app/ponto/calculador/regras.py:115–120`, `app/ponto/calculador/regras.py:270–275`)

**(d) Sim.** A regra de completar o previsto aparece na conta do abono da grade. A saída antecipada segue por outros pontos do código. (`app/docs/CORTES.md:558`, `app/ponto/services/dia_pago.py:393–401`)

- **Escrita diária.** `lavrar` soma a saída antecipada dos períodos e, quando recebe as rubricas do oráculo, grava o valor fornecido por ele. No caminho do motor, o crédito parcial abate somente `horas_atraso`. (`app/ponto/services/dia_pago.py:173–195`, `app/ponto/services/dia_pago.py:322–330`, `app/ponto/services/dia_pago.py:345–360`, `app/ponto/calculador/regras.py:855–860`)
- **Escrita mensal e leitura na folha.** O fechamento aplica o crédito parcial ao atraso e grava a saída antecipada arredondada a duas casas. O exportador do Domínio lê os dois campos e os soma na **8069**, sem consultar o abono. O cartão usa essa mesma soma na conferência. (`app/ponto/services/fechamento.py:1022–1026`, `app/ponto/services/fechamento.py:1184–1196`, `app/folha/export.py:383–399`, `app/folha/export.py:449–452`, `app/relatorios/cartao_pela_celula.py:270–273`)

A correspondência da cópia com o hash informado e a emissão efetiva da 8069 em produção são [nao verificado].

## 26/11 10:30 · sessao 710723db0708 · PAR-RELAMPAGO-A-LISTA-DO-ADMIN · verificador · origem=Code

**Bloqueia o pouso por escopo.** O código entrega os dois comportamentos pedidos para o C13 que a lista publica, mas o diff também muda a origem do estoque histórico, a ordem das guardas, o relógio do juiz do par, a transação do flip, o recálculo da restauração e a reabertura da medição do TXT. São mudanças de comportamento além das duas respostas autorizadas para c7. `app/ponto/services/par_relampago_na_lista.py:68`, `:244`, `:316`, `:431`; `app/ponto/turnos.py:2193`; `app/ponto/services/flip_batida.py:17`; `app/ponto/registro_batida.py:305`; `app/ponto/services/fechamento.py:1843`.

A conferência usou o diff anexado e os arquivos da cópia. Os 22 blobs conferem com os prefixos do lado novo do diff. A identidade dos commits e o diff integral contra a base ficaram **[nao verificado]**, pois a cópia veio sem `.git`. Os anexos não trazem uma rodada de produção com C13, colisões ou totais do vigia. Impacto em produção **[nao verificado]**. [nao verificado]

### 1. Fila · A/B — Onde nascem o destino e o dia exibido

| Campo ou valor | Escritor e leitores |
|---|---|
| `DESTINO_PAUTA_DP` e `destino` do C13 | A constante vale `'pauta-dp'`. `verbos_do_sinal` consulta K6, pelo adaptador `competencia_exportada`, e devolve `ENTREGUE` sem verbos. `pendentes` publica `destino` somente para esse rótulo. `_par_relampago` e a montagem de `assinaturas` repassam o valor. `dias_na_lista` o usa para excluir a assinatura e `dias_da_pauta_dp` o indexa. `RESTAURAR_ENTREGUE` mantém Ratificar e fica sem `destino`, pois só a faixa da restauração está entregue. `app/folha/export.py:565`; `app/ponto/services/par_relampago_na_lista.py:26`, `:190`, `:321`, `:330`, `:334`, `:374`; `app/escala/services/cadastro_realidade.py:423`, `:1116`, `:1268`, `:1279`. |
| `data_ref` | `lavrar` grava o dia de turno em `MetricaSnapshot.data_ref`, um `DateField` cuja chave é `(chave, escopo, escopo_id, data_ref)`. O detector fornece o dia pelo `par_completo` nos dois ramos. O comando de estoque histórico fornece o dia pelo `dia_do_turno_da_batida`. `pendentes` lê e ordena pelo `data_ref`, usa-o na conferência e o publica. `_par_relampago` também o serializa em ISO. O POST relê o `data_ref` do snapshot e o entrega a `_conferir_sinal`. `app/inteligencia/models.py:91`, `:101`; `app/ponto/services/par_relampago_na_lista.py:159`, `:253`, `:350`, `:369`, `:388`, `:412`, `:425`; `app/ponto/management/commands/detectar_par_relampago.py:178`, `:245`, `:260`; `app/ponto/management/commands/lavrar_estoque_par_relampago.py:57`, `:66`; `app/escala/services/cadastro_realidade.py:424`. |
| `dia`, `dia_lavrado` e `data` | `verbos_do_sinal` devolve o dia de turno atual ou `None`. Seu parâmetro `dia_lavrado` é uma **data** do snapshot. Já a chave `dia_lavrado` publicada por `pendentes` é um **booleano**. Nesse payload, `dia` é a data a exibir, com `data_ref` quando a resposta veio `None`. `_par_relampago` grava `data` como ISO e prefixa `detalhe` com `dia lavrado` quando o booleano está ligado. `um_dono_por_dia` e os dois índices do vigia leem `data`. O POST usa a resposta de `verbos_do_sinal` para a ação e a restauração repassa o dia de turno ao fechamento e à medição do TXT. `app/ponto/services/par_relampago_na_lista.py:301`, `:321`, `:326`, `:374`, `:425`, `:488`, `:496`, `:501`; `app/escala/services/cadastro_realidade.py:415`, `:424`, `:1202`, `:1268`, `:1279`. |

O HTML e o PDF recebem `dados` da mesma função `lista`. O HTML lê `detalhe`, `acao`, `rotulo` e `verbos`. O formulário envia o PK do snapshot, os PKs das batidas, a trilha e a decisão, sem enviar data. O PDF lê `detalhe`, `acao` e `dono`. A recusa de ação para `ENTREGUE` acontece antes da escrita de batida ou trilha. No caminho conferido, Pauta DP é o encaminhamento publicado na lista e no vigia. Criação de um registro `Pauta` para esse C13 **[nao verificado]**. `app/escala/views.py:525`; `app/templates/escala/cadastro_x_realidade.html:31`, `:33`, `:35`, `:40`; `app/escala/services/cadastro_realidade.py:768`, `:1110`, `:1323`; `app/ponto/views.py:3481`, `:3496`; `app/ponto/services/par_relampago_na_lista.py:425`, `:465`, `:488`, `:513`; `app/pautas/services.py:311`.

### 2. Fila · B/C/D — Índices, colisões e significado de `destino`

- **Mesma chave nos dois índices.** `dias_na_lista` escreve `(colab_id, AAAA-MM-DD) → dono ou None`; `dias_da_pauta_dp` escreve a mesma chave com o destino. `ListaDoAdmin` inicializa `_pauta_dp` e carrega os dois mapas da mesma saída, com o mesmo recorte de empresas. `dia_do_admin` consulta `_dias` e aceita exclusivamente `BATIDA` e `CADASTRO`. `dia_na_lista` e `papel_do_dia` também consultam `_dias`. `destino_fora_da_mesa` é o leitor de `_pauta_dp` e devolve o destino apenas se a chave estiver ausente de `_dias`. Seu chamador é `dono_fora_do_fio`, lido pelos dois censos. A causa `(d)` do alcance ainda consulta `dia_na_lista` e `colab_na_lista`. `app/escala/services/cadastro_realidade.py:1249`, `:1274`; `app/chamados/services/vigia_fio.py:150`, `:182`, `:192`, `:202`, `:210`, `:218`, `:228`, `:238`, `:276`, `:419`, `:656`, `:680`.
- **Para a mesma chave, a outra assinatura vence.** Conferi em memória as funções da cópia: C13 entregue sozinho dá `pauta_dp`, com C12 `CADASTRO` dá `fio_do_admin`, e com C12 `ESTRUTURA` ou dono `None` não recebe balde nessa etapa e segue para fio morto. A guarda é por presença da chave, inclusive quando o dono é `None`. Ausência aberta, folga contestada e porta publicada têm precedência nas etapas já existentes. As outras assinaturas que publicam `data` são C5, C9, C10, C11 e C12. `app/chamados/services/vigia_fio.py:236`, `:268`, `:401`, `:433`, `:438`, `:642`, `:656`, `:698`; `app/escala/services/cadastro_realidade.py:242`, `:436`, `:460`, `:481`, `:599`, `:1181`; `app/ponto/tests/test_revisar_par_relampago.py:772`.
- **O `destino` da assinatura tem dois significados.** A assinatura C7 do termômetro já usa a chave para um objeto de escala sugerida, lido pelo HTML e pela ação de realocar. Ela publica `data_exemplo` na lavra e nenhuma `data` na assinatura, por isso escapa dos índices na cópia. Reproduzi em memória o limite: se uma C7 com esse objeto publicar `data`, os dois filtros genéricos classificam o dia como `pauta_dp` e o objeto vira o papel do balde. É um contraexemplo ao contrato dos leitores, sem ocorrência pelo escritor atual. Há ainda o `destino` **da linha**, texto do propositor lido pelo HTML/PDF, fora da chave da assinatura. O C13 entregue pode continuar exibindo o dono `BATIDA` ao lado do encaminhamento. Isso afeta a apresentação, enquanto a mesa do vigia usa o índice filtrado. `app/escala/services/regua_na_lista.py:31`, `:50`; `app/escala/views.py:464`; `app/escala/services/cadastro_realidade.py:135`, `:376`, `:1076`, `:1109`, `:1268`, `:1279`, `:1327`; `app/ponto/services/esmeril_espelho.py:242`; `app/templates/escala/cadastro_x_realidade.html:31`, `:51`.
- **Dia lavrado e dia vivo podem divergir.** A primeira guarda K6 devolve `None` antes de perguntar o turno. Nesse caminho, o vigia indexa o dia lavrado, assim como a lista o exibe. Se o turno tiver migrado de data desde a lavra, o sinal só cobre a chave lavrada. Há também o caso `COMPETENCIA_MUDOU` com dia vivo `None`: ele exibe o dia lavrado, fica sem destino e pode contar na mesa por `BATIDA`. A diferença entre os dois casos vem de `rotulo`, enquanto ambos têm `dia_lavrado=True`. Ocorrências desses casos em produção **[nao verificado]**. `app/ponto/services/par_relampago_na_lista.py:321`, `:326`, `:374`; `app/escala/services/cadastro_realidade.py:424`, `:1268`, `:1279`; `app/chamados/services/vigia_fio.py:208`.

Os fechadores de Cadastro x Realidade leem `postos → assinaturas`, selecionam C7 ou C8 pelo código e usam o detalhe. O cron `detectar_par_relampago --dias 2 --apply` é produtor de sinais, e o estoque histórico é comando avulso. Ambos reportam a exportada sem lavrar um C13 novo. O balde do vigia depende de um sinal já lavrado que chega à lista. `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:31`; `app/config/crons.py:605`, `:771`, `:1465`; `app/ponto/management/commands/detectar_par_relampago.py:248`, `:261`; `app/ponto/management/commands/lavrar_estoque_par_relampago.py:61`, `:94`; `app/ponto/services/par_relampago_na_lista.py:350`.

### 3. Fila · B/C — L-110 fecha nos dois universos

- **Censo de lâmpadas.** `pauta_dp` começa em zero e soma uma vez por célula acesa elegível. `celulas_pauta_dp` guarda PKs até `limite`; o teto da amostra não corta o total. O `continue` impede que a mesma célula some em `fio_morto` ou nas suas aberturas. A soma tem oito parcelas, incluindo `pauta_dp`; a mensagem da asserção ainda diz “seis”. `linhas` lê o total e a amostra em `VFM|PAUTA_DP`. O comando imprime e carimba `dict(d)` na frota. `materializar_passivo_validado` lê os inteiros do censo, incluindo `pauta_dp`, no comparativo antes/depois. `app/docs/LEIS.md:136`; `app/chamados/services/vigia_fio.py:352`, `:364`, `:395`, `:419`, `:433`, `:893`; `app/chamados/tests/test_o261_vigia_fio.py:788`; `app/chamados/management/commands/vigiar_fio_morto.py:214`, `:270`, `:281`; `app/chamados/management/commands/materializar_passivo_validado.py:204`, `:227`.
- **Alcance.** `BALDES_FORA_DO_FIO` inclui `pauta_dp`. `censo_alcance` inicializa `alcance_pauta_dp`, soma um por chave e publica a rota e o papel em `alcance_por_rota_papel` e, até o limite, nas linhas de detalhe. `linhas_alcance` lê o balde pelo mesmo registro, assim como a asserção de soma. O universo vem dos dias retidos da competência atual e da anterior enquanto esta estiver sem entrega. Nos dois censos, a triagem sem porta só chega a Pauta DP para dia anterior a `hoje`. `app/chamados/services/vigia_fio.py:409`, `:518`, `:566`, `:573`, `:622`, `:650`, `:656`, `:723`; `app/ponto/services/he_pendente_lavrado.py:215`; `app/chamados/tests/test_o261e_f5_alcance.py:122`.
- O cron `vigiar_fio_morto` compartilha a lista entre os dois censos e carimba o bloco `alcance`. O alarme usa as chaves de `ALARMA_SEMPRE` e `ALARMA_EM_0`. O backfill lê as linhas `sem_porta` e o total de fio morto do alcance. O ESTADO lê os totais e o universo do carimbo, sem consumir nominalmente os novos campos. `app/config/crons.py:1746`; `app/chamados/management/commands/vigiar_fio_morto.py:87`, `:180`, `:190`, `:211`, `:294`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:79`, `:316`, `:500`; `bin/fio_estado.py:74`, `:121`, `:148`; `bin/gerar_estado.py:260`.

### 4. Bloqueia por escopo · E — Checklist das classes de reprovação

- **Juiz, escritor e fallback.** O caminho reaproveita K6, `par_completo` e o juiz do vínculo pelo carregador. O recálculo novo chama o escritor existente e a reabertura chama `porta.pedir` e `porta.encerrar`. As capturas do vigia e do fechamento por evento já constavam do diff antes da mudança. O fallback novo fora de c7 é `origem_lavrada` inferir empresa e competência quando snapshot e trilha não as informam. O comando de estoque ainda descreve esse caso como dependente da escrita anterior. O anexo também muda `acao('C13')` para propagar rótulos além de `ENTREGUE`, como `COMPETENCIA_MUDOU` e `RESTAURAR_ENTREGUE`. Motor e migrations estão ausentes do diff fornecido. `app/core/juizes.py:185`, `:249`, `:1319`; `app/ponto/services/par_relampago_na_lista.py:79`, `:88`, `:328`, `:337`, `:444`; `app/ponto/services/fechamento.py:1831`, `:1838`, `:1865`; `app/chamados/services/vigia_fio.py:199`; `app/ponto/management/commands/lavrar_estoque_par_relampago.py:35`; `app/escala/services/cadastro_realidade.py:768`.
- **Travas, relógio e pré-requisito.** A revisão pega empresas por PK, depois colaborador e batidas por PK. O flip muda para colaborador antes da batida. Restaurar recalcula dentro do `atomic`, confere a trilha, pede a medição e agenda o disparo após o commit. O teste do flip observa uma conexão. Concorrência real e pré-requisitos efetivamente executados em produção **[nao verificado]**. O instante do juízo desce da rodada, do detector e do POST. A medição reaberta ainda lê seu próprio dia e instante em `porta.pedir`, e `dias_encerrados` ainda relê o relógio se receber `None` por variável, condição que o selo sintático não exclui. Os três testes de varredura do selo passaram em memória. `app/ponto/services/par_relampago_na_lista.py:172`, `:431`, `:479`; `app/ponto/services/flip_batida.py:20`; `app/ponto/tests/test_flip_batida.py:36`; `app/ponto/management/commands/detectar_par_relampago.py:221`; `app/ponto/views.py:3496`; `app/chamados/management/commands/vigiar_fio_morto.py:180`; `app/folha/porta_export.py:21`, `:1102`; `app/ponto/turnos.py:2156`; `app/escala/tests/test_selo_grade_pede_o_instante.py:46`, `:137`.

### 5. Fila · E — Os REDs cobrem o roteamento, com duas lacunas

Os cinco mutantes c7 têm alvo textual único. As asserções miram a queda a fio morto, a entrada indevida na mesa, a perda da precedência de `ESTRUTURA`, a ausência do prefixo e a omissão de destino. O último, apesar do nome `destino_sem_ser_entregue`, força `destino=None` e portanto cobre omissão, sem cobrir destino indevido em C13 aberto. Os REDs param antes dos laços dos censos: a soma existente não planta C13 entregue e o teste chamado `entregue_no_pdf` inspeciona `acao` da lista, sem renderizar o PDF. Assim, a execução dos 93 mutantes com Django e banco e a mordida de ponta a ponta ficam **[nao verificado]**. `app/ponto/tests/par_relampago_mutantes.py:370`, `:423`; `app/ponto/tests/test_revisar_par_relampago.py:737`, `:749`, `:758`, `:772`, `:782`; `app/chamados/tests/test_o261_vigia_fio.py:770`.

### 6. Fila · F — A catraca passa. A data civil continua distinta do dia de turno

`Batida.timestamp` é `DateTimeField`, com `USE_TZ=True` e fuso padrão `America/Sao_Paulo`. Para um instante **aware**, `_so_a_data` e `turnos.data_local` aplicam `timezone.localtime(...).date()`. Para um `datetime` **naive**, `_so_a_data` tira `.date()` sem conversão. Conferi em memória o caso `21/07 01:00 UTC`: aware dá `20/07`; os mesmos números naive dão `21/07`. Na origem histórica, a geometria chama `data_local` **antes** de `_so_a_data`, portanto essa troca não torna o caminho compatível com timestamp naive. `app/ponto/models.py:23`; `app/config/settings/base.py:93`; `app/ponto/services/fechamento.py:1571`; `app/ponto/turnos.py:45`, `:2970`; `app/ponto/services/par_relampago_na_lista.py:98`.

O carregador recebe duas possíveis datas, civil e de turno, deduplicadas por valor, e encaminha cada `(colaborador, data)` ao juiz que prioriza a geradora da célula. A vigência inclui início e fim. A competência usa o dia do turno e o corte da empresa encontrada. O código exige a mesma **empresa** nas duas datas e aceita dois vínculos distintos da mesma empresa. A catraca passou **35 testes em memória** e registra a ocorrência de normalização em `_so_a_data`. A equivalência de fuso vem dos corpos das funções, além do que a catraca mede. `app/ponto/services/par_relampago_na_lista.py:101`, `:106`, `:109`, `:113`; `app/escala/alimentacao.py:205`, `:219`, `:228`, `:316`, `:329`; `app/ponto/services/dia_pago.py:702`; `app/ponto/services/fechamento.py:1608`, `:1860`; `app/core/tests/test_medida_turno_do_par.py:28`, `:329`, `:546`, `:562`.

## 26/11 10:26 · sessao f528468448f6 · PAR-PELO-TURNO · leitura · origem=Code

# Dossiê antes do construtor — PAR-PELO-TURNO

**Base:** `558de6bd4bba318bac4130a14936946e218f885d`. Conferi os blobs centrais contra o commit e usei a V6 `2311-1958` como aval vigente. A V6 mantém o separador por classe, transforma o corte `previsto + 4 h` em sinal C9, conserva o limite de 26 h e usa as portas existentes. O cadastro de duração da V2 e o alerta de interjornada ficam fora desta obra. A fila vigente põe PAR depois de FERIADO-SEM-LIMITE e antes de INTERVALO-PRE-ASSINALADO V2. `CLAUDE.md:412`, `app/docs/BACKLOG.md:2020`, `app/docs/BACKLOG.md:2022`, `app/docs/PROMPTS.md:2852`, `app/docs/CORTES.md:637`.

## 1. Mapa

**O sistema já faz parte do trabalho.** O pareador já reúne entrada, saída e pausa efetiva, inclusive na virada de dia. O corte ocorre quando a saída chega ao fecho principal e o vão desde a entrada ultrapassa o teto do DNA do dia. O teto é o vão entre `hi` e `hf`, em minutos do relógio com virada, mais 240 min. Se as duas pontas estiverem a mais de 180 min dos respectivos marcos, a L-084 faz esse ramo usar o envelope. No caso que só o teto do dia separa, saem uma entrada aberta e uma saída órfã, com carimbo transitório na saída. `app/ponto/turnos.py:66–82`, `app/ponto/turnos.py:312–324`, `app/ponto/turnos.py:1430–1457`, `app/ponto/motor_calculo_v2.py:1947`.

1. **Caminho por colaborador.** `turnos_do_colab` carrega batidas apuráveis, células e vínculos. `_turnos_do_colab_puro` marca ecos de flush, obtém os papéis da ata, divide por vínculo do dia e chama `parear_turnos` sobre cópias. O pareador ordena por instante, classifica o miolo, passa por E/S e roda, nesta ordem, `_fechar_aberto_com_saida_seguinte` e `_fechar_aberto_na_pausa_sem_volta`. Ambas ainda leem `previsto + 4 h` como guarda de fecho. `_cabe_na_jornada_da_vespera` lê a mesma conta para atribuição de dia. `app/ponto/turnos.py:1867–1920`, `app/ponto/turnos.py:1940–2002`, `app/ponto/turnos.py:1217–1221`, `app/ponto/turnos.py:1304–1502`, `app/ponto/turnos.py:1084–1095`, `app/ponto/turnos.py:1176–1188`, `app/ponto/turnos.py:216–231`.
2. **Caminho do dinheiro.** `calcular_mes` prepara o mapa de dia via `_turnos_do_juiz` → `turnos_de_batidas`. Para montar períodos, `turnos_via_autoridade` chama o mesmo pareador com os parâmetros da classe, entrega a `periodos_de_turnos` e passa por `_periodos_pelo_marco`. A ata pode substituir o período quando explica as batidas e as pontas correspondem a instantes reais. O adaptador registra a saída órfã como anomalia, sem criar período para ela. NOTURNO-A já usa o dia do turno para ancorar a janela de HE. `app/ponto/motor_calculo_v2.py:867–895`, `app/ponto/motor_calculo_v2.py:2357–2373`, `app/ponto/motor_calculo_v2.py:453–531`, `app/ponto/motor_calculo_v2.py:632–688`, `app/ponto/adaptador_motor.py:28–45`, `app/ponto/motor_calculo_v2.py:1545–1549`.
3. **C9 e Gestão de HE.** A C9 atual exige que a saída continue órfã **e** que a entrada continue aberta no resultado final. Só liberar o fecho faria o item sumir. A V6 exige testar, juntos, **par fechado e C9 presente**, com o motivo e os horários vindo do pareador. A C9 chega a Cadastro x Realidade pela lavra existente. A Gestão de HE percorre outra cadeia existente: `espelho_do_colab` → `he_fora_da_janela` → retrato lavrado → `gestao_he.enriquecer`, com a decisão vigente sobreposta. O espelho lê as pontas da grade e também o que o motor já publicou quando a grade não casou a ponta. `app/ponto/turnos.py:165–213`, `app/ponto/services/teto_na_lista.py:68–86`, `app/escala/services/cadastro_realidade.py:405–418`, `app/ponto/services/espelho.py:734–763`, `app/ponto/services/he_pendente_lavrado.py:71–83`, `app/ponto/services/he_pendente_lavrado.py:138–158`, `app/ponto/views.py:948–970`, `app/ponto/services/gestao_he.py:272–302`, `app/docs/PROMPTS.md:2852`.

**Uso constatável.** Há 13 sítios diretos de `parear_turnos` fora de testes e migrações, incluindo motor, turno aberto do app, triagem, escala e reconciliação. A C9 está configurada para lavra diária às 07:42 após `supra_juiz`, e a lavra de HE está encadeada em `lavrar_deslogados`. A rota de Gestão de HE lê o retrato em cada consulta à listagem. A configuração da C9 registra uma medição histórica de 578 colaboradores em 12,8 s em 29/10. Frequência efetiva de execução, tráfego das telas e estoque atual em produção **[nao verificado]**. `app/config/crons.py:516–522`, `app/config/crons.py:1576–1578`, `app/api/views_core.py:289–296`, `app/ponto/urls.py:17–27`, `app/ponto/views.py:948–970`; sítios do censo na seção 2.

## 2. Censo fechado da superfície da V6

**Critério.** AST de 3.699 arquivos Python versionados em `app/`, `bin/` e `scripts/`, por sítio de chamada e com aliases de importação explícitos. Cruzei os símbolos e as chaves literais com `git grep -n -w` no commit, incluindo templates para a projeção de HE. As contagens fora de testes e migrações incluem comandos operacionais. Reflexão, reexportações indiretas, SQL externo, payloads já gravados e execução efetiva em produção **[nao verificado]**. O censo fecha a superfície identificada abaixo; as escritas de cadastro e de decisão entram como fronteiras existentes. `CLAUDE.md:197`, `CLAUDE.md:412`. [nao verificado]

| Campo ou função | Escritores e leitores |
|---|---|
| `_teto_da_jornada`, `_teto_s_da_jornada` | Um corpo calcula. Leitores diretos do corpo: o wrapper numérico e o fecho principal. Os três leitores do wrapper são a guarda da véspera e os dois pós-fechos. `_data_do_turno` consulta a guarda em dois ramos. Os dois pós-fechos são chamados no final do pareador. `app/ponto/turnos.py:58–82`, `:230`, `:298`, `:302`, `:1086`, `:1177`, `:1434`, `:1501–1502`. |
| `_separada_pelo_teto` e resultado para C9 | `_declara_separado_pelo_teto` escreve no objeto da saída, por um único sítio. O próprio escritor lê para preservar o primeiro carimbo. `pares_separados_pelo_teto` é o leitor de negócio e devolve `data_turno`, `entrada`, `saida`, `hi`, `hf`, `teto_min`, `vao_min`. Fora de testes, só `teto_na_lista.medir` chama esse leitor. `app/ponto/turnos.py:165–213`, `:1452`, `app/ponto/services/teto_na_lista.py:68–86`. |
| Snapshot C9 | `medir` monta `itens`, contagens e erros, e lê o dono pelo `InsumosDoDono`. `lavrar` escreve `MetricaSnapshot` sob `par_separado_pelo_teto`, e `ultimo_lavrado` o lê. O comando chama `medir` e `lavrar`, e `_teto_do_dia` traduz a foto para `lista`, consumida pela tela e pelo PDF. O rótulo de hoje ainda diz que o par ficou separado. `app/ponto/services/teto_na_lista.py:25`, `:39–130`, `app/ponto/management/commands/lavrar_teto_do_dia.py:26–36`, `app/escala/services/cadastro_realidade.py:405–418`, `:983–989`, `app/escala/views.py:512–529`. |
| Batida, DNA e papel no pareador | `Batida` fornece `pk`, `timestamp`, `tipo`, `is_intervalo` e `retratada_em`. O cadastro é lido por `vinculo_do_dia` e `marcos_dna_periodo`; `papel_por_minuto_da_ata` produz `E/S/X/Xi`, e `_papel_da_ata` consome o mapa. A criação persistente passa por `registrar_batida`; `anotar` admite `tipo` e `retratada_em`, e as portas de retratar e desfazer usam essa escrita. `semear` tem guarda de banco de ensaio. `app/ponto/models.py:8–26`, `:65–67`, `app/ponto/registro_batida.py:118`, `:170–177`, `:223–225`, `:255–274`, `:287–297`, `:339–355`, `app/ponto/services/retratar_humano.py:197–208`, `app/escala/alimentacao.py:228`, `app/escala/utils.py:84`, `app/ponto/turnos.py:890–909`, `:1565–1599`, `:1867–1871`. |
| Carimbos de pausa e de eco | `_marca_ecos_flush` escreve `_eco_flush`; o laço de `parear_turnos` lê. `_marca_intra_duracao` e o laço escrevem `_intra_dur`; o laço também escreve `_papel_ata`, `_intra_ata` e `_intra_declarado`. Leitores diretos incluem `_pares_marcados`, a saída posicional, a pausa sem volta e o vigia do motor. `turnos_de_batidas` limpa `_intra_dur` e `_eco_flush` na cópia. `app/ponto/turnos.py:344–372`, `:415–488`, `:798–879`, `:932–998`, `:1170–1173`, `:1310–1415`, `:1477–1479`, `:1524–1535`, `app/ponto/management/commands/vigiar_motor_autoridade.py:84`. |
| HE publicada e decisão vigente | `montar_dias` escreve `he_fora_da_janela`. Leitores literais: espelho, lavra HE, exportação, view de ponto, PDF e template. `he_pendente_lavrado.lavrar` escreve `he_pendente`; `ler` serve lista, PDF de HE, itens do lote, Central e o vigia de ponta pequena. A porta de HE escreve estado e revogação em `_gravar` e `_marcar_revogada`. O cron, a Gestão e a janela do motor leem `DecisaoHE` vigente. `app/ponto/services/espelho.py:790`, `:939`, `:946`, `app/ponto/services/he_pendente_lavrado.py:68`, `:138`, `:183–212`, `app/folha/porta_export.py:190`, `app/ponto/views.py:180`, `:820`, `:950`, `app/relatorios/pdf_espelho.py:932`, `app/templates/ponto/espelho.html:189`, `app/ponto/services/gestao_he.py:93`, `:123`, `:428`, `:493`, `app/ponto/management/commands/recusar_ponta_pequena.py:42–69`, `app/ponto/portas/he.py:44–82`, `:394–422`, `app/ponto/management/commands/lavrar_he_pendente.py:48`, `app/ponto/motor_calculo_v2.py:1415–1429`. |

**Sítios de chamada diretos.** `parear_turnos` tem **13** fora de testes e migrações e **98** em testes. Os 13 são `app/ponto/turnos.py:1535`, `:1997`, `:2295`, `app/ponto/motor_calculo_v2.py:503`, `app/chamados/services/fila_por_causa.py:197`, `app/escala/services/meio_periodo.py:168`, `app/escala/services/semana_do_posto.py:120`, `app/ponto/management/commands/reconciliar_fantasmas.py:99`, `app/ponto/services/diagnostico_escala.py:142`, `app/ponto/services/lastro.py:268`, `app/ponto/services/piso_intervalo.py:48`, `app/ponto/services/triagem_batida.py:178`, `app/ponto/utils_turno_orfao.py:31`. `turnos_de_batidas` tem **4** sítios fora de testes: `app/ponto/motor_calculo_v2.py:882`, `app/escala/services/propositor.py:117`, `:152`, `:522`. `_turnos_do_colab_puro` tem **3**: `app/ponto/turnos.py:1920`, `:3101`, `app/chamados/services/carga_pergunta_viva.py:182`. `_periodos_via_autoridade` tem **4**: `app/ponto/motor_calculo_v2.py:2764`, `:2894`, `:3232`, `:3552`; seu leitor do trio é `_periodos_pelo_marco` em `:531`.

<details>
<summary>51 sítios diretos de turnos_do_colab fora de testes e migrações</summary> [nao verificado]

- API e chamados — `app/api/views.py:1447`, `app/chamados/detector_anomalias.py:157`, `app/chamados/juizes.py:447`, `:2303`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:145`, `app/chamados/models.py:1378`, `app/chamados/services/disputa_emissao.py:722`, `app/chamados/services/materializacao.py:399`, `:513`.
- Colaboradores e escala — `app/colaboradores/services/drawer.py:47`, `app/colaboradores/services/feriado_do_posto.py:473`, `app/escala/detector_proposta.py:65`, `app/escala/regua_defesa.py:361`, `app/escala/services/escala_auto.py:87`, `app/escala/services/impacto_troca.py:18`, `app/escala/servico_jornada.py:249`, `:403`, `app/escala/utils.py:1627`, `:1952`.
- Comandos de ponto — `app/ponto/management/commands/censo_fase_12x36.py:180`, `detectar_ausencias.py:337`, `detectar_par_relampago.py:101`, `dna_x_batida_real.py:69`, `e6_oraculo.py:159`, `emitir_turno_aberto_retroativo.py:88`, `marcar_abandono_provisorio.py:46`, `verificar_nucleo.py:29`, `vigia_plantio_overnight.py:38`, todos no mesmo diretório.
- Núcleo e serviços de ponto — `app/ponto/nucleo.py:47`, `app/ponto/services/bordas_realizado.py:66`, `esmeril_espelho.py:102`, `espelho.py:628`, `passivo_s84.py:82`, `regularizacao.py:61`, `teto_na_lista.py:68`, `triagem_batida.py:553`, `vigia_pareador_celula.py:97`, estes oito no diretório `app/ponto/services/`.
- Leitura, telas e relatórios — `app/ponto/turno_leitura.py:89`, `:95`, `app/ponto/turnos.py:780`, `:2104`, `:2162`, `:2192`, `:2978`, `:3169`, `app/ponto/views.py:396`, `:1613`, `:3356`, `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`, `app/relatorios/pdf_espelho.py:213`, `:451`.

</details>

## 3. Leis herdadas

- **L-001, L-002, L-007, L-011 e L-099.** A origem decide, a testemunha lê e o dono da divergência sai do juiz já existente. O corte `COL129-TETO-x-PORTAO-26H` explica por que a C9 lê o carimbo do pareador. A V6 muda a consequência do teto do dia. O corte de 28/10 e o aval V2 explicam o comportamento anterior. `app/docs/LEIS.md:59–69`, `:122`, `CLAUDE.md:179–180`, `app/docs/PROMPTS.md:1484`, `:1599`, `:2852`, `app/docs/CORTES.md:633–638`.
- **L-020, L-024, L-025, L-030, L-084, L-085, L-087, L-088 e L-115.** Preservam DNA, previsto da grade, universo apurável, batidas de chão, pontualidade quando o cadastro descreve o dia, dia de início, papel do marco, furo em aberto e saída posicional depois de pausa fechada. O corte `LAUDOS-[nome]-2810` vincula col129 de 25/10 ao contrato de juiz único do par. O piso de par degenerado é de 60 s na fonte vigente. `app/docs/LEIS.md:71–78`, `:116–120`, `:153`, `app/docs/CORTES.md:497`, `app/ponto/turnos.py:117–131`, `app/ponto/management/commands/detectar_par_relampago.py:49`.
- **L-031, L-032, L-034, L-035 e L-097.** A apuração vigente trata intervalo, regime, adicional noturno e janela de HE. A autorização por colaborador e período já tem contrato de prévia por rubrica, motivo único e decisão por dia. A V6 manda passar pela apuração e pela Gestão existentes. `app/docs/LEIS.md:79–83`, `:115`, `app/docs/DOSSIES.md:215–241`, `app/docs/PROMPTS.md:2852`.
- **L-092, L-113, L-121 e DINHEIRO.** A regra geral distingue exportada guardada da exportada fora da guarda. Nesta obra prevalece o veto explícito da V6 a tocar exportada. A L-110 rege o gabarito dos REDs e a L-114 distingue prova pela regra de impacto em sombra. Um eventual diff em `motor_calculo_v2.py` depende do `!` específico da zona inviolável. `app/docs/LEIS.md:125`, `:136`, `:142`, `:154–156`, `CLAUDE.md:271–279`, `:423–435`, `app/docs/CORTES.md:646`, `app/docs/PROMPTS.md:2852`.

## 4. Contrato de entrada

| Dimensão | Contrato |
|---|---|
| **Fonte** | O laudo existente é o insumo de prova dos 29 casos. Em execução, a fonte é a batida apurável e a célula com DNA e ata, pelo vínculo do dia. O motor tem caminho sem mapa de DNA para intervalo2 e sem papel da ata quando ela não está alimentada. `app/docs/BACKLOG.md:2020`, `app/ponto/turnos.py:1867–1893`, `:1988–1999`, `app/ponto/motor_calculo_v2.py:467–506`. |
| **Unidade** | Batida é instante com identidade. O papel da ata usa o minuto local; o fecho mede segundos entre as pontas. O par pertence ao `data_turno`; o item C9 leva vão e teto em minutos inteiros, e a pendência HE soma os minutos das pontas por dia-colaborador. `app/ponto/turnos.py:183–212`, `:312–324`, `:890–909`, `:1447–1452`, `app/ponto/services/he_pendente_lavrado.py:55–68`. |
| **Universo e escala** | O laudo tem 29 dia-colaborador e 14 colaboradores na triagem registrada. A C9 atual varre empresas na competência corrente e nas duas anteriores, com candidatos por batida e erros contados. A lavra HE padrão varre colaboradores ativos das empresas ativas para a competência atual e a anterior enquanto não entregue. O lote integral raspado, seus negativos completos e o antes × depois em sombra **[nao verificado]**. `app/docs/RELATO.md:348–350`, `app/docs/BACKLOG.md:2020`, `app/ponto/services/teto_na_lista.py:25–68`, `:88–99`, `app/ponto/management/commands/lavrar_he_pendente.py:34–39`, `app/ponto/services/he_pendente_lavrado.py:110–121`, `:215–231`. |
| **Exclusões** | Retratadas ficam fora da apuração. A correção tem veto a mexer em batidas, exportadas ou pagar novamente noite já creditada. A C9 existente exclui os casos separados pelo próprio envelope de 26 h. O cadastro novo e a nova porta ficaram fora pelo aval. `app/ponto/turnos.py:2921–2948`, `:1447–1457`, `app/ponto/services/teto_na_lista.py:100–103`, `app/docs/PROMPTS.md:2852`, `app/docs/BACKLOG.md:2020`. |

## 5. Matriz de transições e contraprova

A triagem versionada atribui ao **G1** seis ocorrências de col129 e cinco de col155, todas no corte por duração, com vãos de **779–784 min** frente a **732 min**. Essa linha dá o mecanismo do grupo, sem discriminar a sequência e o papel de cada batida por dia. A série HJ-026 de col155 registra cinco domingos de 18/10 a 15/11, **781–783 min** contra **732 min**. O caso de **04/10** aparece em outro registro histórico e fica identificado separadamente. `app/docs/RELATO.md:348–350`, `app/docs/BACKLOG.md:745`, `app/docs/RELATO-ARQUIVO.md:863`.

Na tabela, **fecho V6** significa fechar a jornada se as pontas forem as do mesmo turno pelas regras de classe, preservar o intervalo efetivo, emitir C9 quando só o teto do dia barraria o par, e enviar o período à apuração vigente. Os nomes são de REDs propostos. `app/docs/LEIS.md:136`, `app/docs/PROMPTS.md:2852`, `app/ponto/turnos.py:1447–1481`.

| Caso | Por que parte hoje e o que a V6 muda | RED proposto |
|---|---|---|
| col129 · 11/10 | O caso é nominalmente citado e o G1 aponta o corte por duração. A linha individual com pontas, ata e ramo executado **[nao verificado]**. Se reproduzir o corte G1, fecho V6. `app/docs/PROMPTS.md:1867`, `app/docs/RELATO.md:350`, `app/ponto/turnos.py:1430–1457`. | `test_col129_1011_extensao_e_c9` |
| col129 · 25/10 | Fixture nominal: 17:30 → 06:30 de 26/10 dá **780 > 732 min**. A entrada fica 240 min antes de `hi`, a saída 48 min após `hf`, então a exceção das duas pontas afastadas não incide. Reproduzi entrada aberta, saída órfã e C9 em memória. Fecho V6. `app/ponto/tests/test_col129_par_separado_pelo_teto.py:52–69`, `app/docs/CORTES.md:497`, `app/ponto/turnos.py:1430–1457`. | `test_col129_1025_fecha_e_mantem_c9` |
| col129 · 01/11 | Corte por duração é a evidência agregada de G1. Pontas e ramo individual **[nao verificado]**. Se reproduzir G1, fecho V6 no dia de início. `app/docs/RELATO.md:350`, `app/ponto/turnos.py:312–324`, `:1430–1457`. | `test_col129_1101_extensao_e_c9` |
| col129 · 02/11 | Corte por duração é a evidência agregada de G1. Batidas vizinhas e vínculo com o turno de 01/11 **[nao verificado]**. Se reproduzir G1, fecho V6 da jornada válida, com cada noite creditada uma vez. `app/docs/RELATO.md:350`, `app/ponto/turnos.py:1430–1457`, `app/docs/PROMPTS.md:2852`. | `test_col129_1102_extensao_sem_duplicar_noite` |
| col129 · 08/11 | Corte por duração é a evidência agregada de G1. Pontas e ramo individual **[nao verificado]**. Se reproduzir G1, fecho V6. `app/docs/RELATO.md:350`, `app/ponto/turnos.py:1430–1457`. | `test_col129_1108_extensao_e_c9` |
| col129 · 15/11 | Com as pontas do anexo, 17:30 → 06:30 de 16/11, reproduzi **780 > 732 min**, entrada aberta e saída órfã, com C9. A saída está a 48 min de `hf` e a L-084 não afasta o teto. Fecho V6 e apuração do noturno pela régua vigente. Pausas e valor por rubrica **[nao verificado]**. `app/docs/RELATO.md:372–374`, `app/docs/CORTES.md:638`, `app/ponto/turnos.py:1430–1457`, `app/ponto/motor_calculo_v2.py:2775–2808`. | `test_col129_1115_fecha_c9_e_apura_noturno_uma_vez` |
| col155 · 04/10 | O registro histórico inclui 04/10 entre os pares completos acima do teto do dia por menos de 5 min. DNA, vão exato e reprodução no HEAD **[nao verificado]**. Se o ramo continuar sendo só o teto do dia, fecho V6. `app/docs/RELATO-ARQUIVO.md:863`, `app/ponto/turnos.py:1430–1457`. | `test_col155_1004_excesso_curto_fecha_e_sinaliza` |
| col155 · 18/10 | Domingo da série HJ-026: teto **732** contra **781–783 min** na série, entrada aberta e saída órfã. Vão individual **[nao verificado]**. Fecho V6. `app/docs/BACKLOG.md:745`, `app/ponto/turnos.py:1430–1457`. | `test_col155_1018_fecha_e_mantem_c9` |
| col155 · 25/10 | Domingo da mesma série e mesmo mecanismo registrado. Vão individual e papel de eventuais batidas internas **[nao verificado]**. Fecho V6. `app/docs/BACKLOG.md:745`, `app/ponto/turnos.py:1430–1457`. | `test_col155_1025_fecha_e_mantem_c9` |
| col155 · 01/11 | Domingo da mesma série e mesmo mecanismo registrado. Vão individual **[nao verificado]**. Fecho V6, madrugada no dia de início. `app/docs/BACKLOG.md:745`, `app/ponto/turnos.py:312–324`, `:1430–1457`. | `test_col155_1101_fecha_e_mantem_dia_do_turno` |
| col155 · 15/11 | Com as pontas do anexo, 17:29 → 06:32 de 16/11, reproduzi **783 > 732 min**, entrada aberta e saída 

[truncado em 20000 de 27813 caracteres]

## 26/11 10:25 · sessao 034edfafc5b6 · INTERVALO-PRE-ASSINALADO-CONTRATO · leitura · origem=Code

# Dossiê antes do construtor — INTERVALO-PRE-ASSINALADO-CONTRATO

**Base `558de6bd4bba318bac4130a14936946e218f885d`.** Conferi os 4.716 arquivos regulares da cópia contra os objetos Git desse commit. O aval vigente é o **INTERVALO-PRE-ASSINALADO-CONTRATO-V2**. A entrega é em leitura, no formato MÃO ÚNICA. `CLAUDE.md:412` · `app/docs/BACKLOG.md:2554` · `app/docs/RELATO.md:230`

## 1. Mapa

**O sistema faz parte da regra.** O motor já desconta a pausa batida que reconhece e indeniza o suprimido. No B1, quando a lista de pausas vem vazia, a soma do intervalo real dá zero. Nos ramos 12x36 e comercial que alcançam o cálculo de indenização, isso deixa a jornada sem desconto de pausa e pode indenizar o mínimo inteiro. Em HE ordinária, `_limite_com_suprimido` já evita a duplicação até o valor que o previsto descontou. No turno partido, a indenização é calculada entre dois períodos do mesmo dia e o ramo pula dias com menos de dois períodos. `app/ponto/motor_calculo_v2.py:1801–1819,1908–1928,2780–2801,2984–3024,3256–3275` · `app/docs/LEIS.md:193–194`

**Onde se decide “sem batida de intervalo”.** `calcular_mes` prepara o dia da jornada e chama `montar_periodos`. O caminho normal passa por `turnos_via_autoridade` → `parear_turnos` → `periodos_de_turnos`, que entrega os pares reconhecidos por `intervalos_do_turno`. Em seguida, `_periodos_pelo_marco` pode substituir o resultado pela ata. Nesse ramo, o motor só copia a pausa quando encontra as duas pontas e seus instantes nas batidas reais. A lista ausente chega como `()` a `calcular_periodo` e ainda passa pelo recorte da janela efetiva. Portanto, lista vazia também pode significar meia batida, par rejeitado pela geometria ou pausa fora da janela. `app/ponto/motor_calculo_v2.py:503–531,558–567,611–616,684–723,1897–1928,2362–2372,2474–2476` · `app/ponto/adaptador_motor.py:26–45` · `app/ponto/turnos.py:537–546`

**Onde se lê o cadastrado.** Há três respostas distintas no HEAD:

| Pergunta | Leitura atual |
|---|---|
| Qual é o mínimo usado na indenização? | `MotorBase.__init__` lê `intervalo_cadastrado(tipo_escala)` por fatia, sem passar o dia. `_intra_minima` combina esse número com o piso do previsto diário, com precedência para `parametros.intrajornada_minutos` quando fornecido. `app/ponto/motor_calculo_v2.py:831–865,1821–1847` |
| Quanto a grade S133 já descontou do previsto? | `_intervalo_descontado_no_dia` lê `escala.utils.intervalo_declarado_do_dia`. O caminho é célula → `vinculo_do_dia` → dia de trabalho → marcos `hi/hf/hii/hfi`. Esse leitor devolve só a primeira janela e devolve zero sem seus quatro marcos. Já `minutos_previstos_do_dia` percorre todas as pausas declaradas e tem fallback para os marcos. `app/ponto/motor_calculo_v2.py:1324–1352` · `app/escala/utils.py:2222–2248,2251–2281,2320–2363` · `app/escala/models.py:1135–1144,1177–1219` |
| O que a ata diz sobre a pausa daquele dia? | `periodos_do_dia` entrega a pausa batida ou um valor cadastrado quando falta uma ponta. Para `dna.intervalo_livre=True`, pede o valor com `celula.data`, sem produzir uma janela ou uma pergunta nesse ramo. A grade de realizado também já desconta a janela fixa que intersecta o turno fechado quando falta pausa reconhecida. `app/ponto/juiz_batida.py:273–336` · `app/ponto/turnos.py:756–795` |

**Quem chama.** No caminho de dinheiro, a tela **Recalcular** e o comando de fechamento chamam `recalcular_fechamento_mes` → `apurar_por_fatia` → `get_motor_cct`/`get_motor` → `calcular_mes`. O resultado alimenta `FechamentoMensal` e `DiaPago`; o TXT lê a rubrica gravada. O espelho compartilha `apurar_por_fatia` e atende admin, app e calendário. O PDF também tem chamada direta ao motor. São portas verificadas no código. Volume de chamadas por porta, distribuição por classe de motor e valores por rubrica em produção ficam **[nao verificado]**. A medida fornecida de 24/11 é **B1 207 dias / 74 colaboradores** e **B2 93 dias / 43 colaboradores**. `app/ponto/views.py:162,1280` · `app/ponto/management/commands/recalcular_fechamento.py:130` · `app/ponto/services/fechamento.py:306,755–775,1028,1224–1254` · `app/core/regua_cct.py:428–479` · `app/ponto/services/espelho.py:1198–1216` · `app/api/views.py:1361` · `app/colaboradores/services/calendario.py:602` · `app/relatorios/pdf_espelho.py:434` · `app/folha/export.py:448` · `app/docs/BACKLOG.md:2554`

## 2. Censo fechado dos sítios da fatia

**Critério.** AST nos 3.823 arquivos Python rastreados, com zero falhas de parse, cruzado com `git grep -n -w` no commit. O inventário abaixo cobre definições, chamadas diretas, aliases de importação, escritas e leituras de atributos, além de chaves literais nos sítios de integração. Testes são referência de contrato, fora da lista de chamadores de runtime. SQL externo, alterações por reflexão com nome calculado, estado do banco e execução real dos chamadores ficam **[nao verificado]**. Como o desenho ainda cabe ao construtor, o censo fecha os símbolos abaixo, sem presumir uma alteração nas portas de cadastro, cobrança ou persistência. `CLAUDE.md:412` · `app/docs/DOSSIES.md:42,61`

| Símbolo ou dado | Definição, escritores e leitores |
|---|---|
| `intervalo_cadastrado` | Definição em `app/ponto/juiz_batida.py:106`. Chamadas na própria ata `:274,317`, na inicialização do motor `app/ponto/motor_calculo_v2.py:832`, no calculador `app/ponto/calculador/alimentacao.py:407,498` e `app/ponto/calculador/periodo.py:520`, e na sonda `app/ponto/management/commands/diff_calculador.py:170`. |
| `intervalo_declarado_do_dia` e `_intervalo_desc_min` | Definição em `app/escala/utils.py:2251`. Leitores em `app/ponto/motor_calculo_v2.py:1344`, `app/ponto/services/espelho.py:1193`, `app/folha/porta_export.py:566`, `app/ponto/calculador/alimentacao.py:517` e `app/ponto/calculador/periodo.py:522`. O motor cria e preenche o memo em `app/ponto/motor_calculo_v2.py:1331–1352`; o espelho também o alimenta em `app/ponto/services/espelho.py:1180–1194`. `_limite_com_suprimido` é o único chamador de `_intervalo_descontado_no_dia` no censo. `app/ponto/motor_calculo_v2.py:1819` |
| Pausa no DNA e na ata | `montar_dna` produz marcos e `intervalo_livre`; as portas de geração, edição, reversão e regeneração gravam a célula. `EscalaColaborador.pausas_do_dia` lê os pares congelados. `periodos_do_dia` produz `intervalo` e `papeis`; chamadores estáticos no motor, em turnos, em Cadastro × Realidade e na exceção da folha. `app/ponto/management/commands/gerar_celulas.py:73–113` · `app/ponto/portas/celula.py:261,278,290–306,641–659,863–866,953–961` · `app/escala/models.py:1177–1219` · `app/ponto/juiz_batida.py:153,284–336` · `app/ponto/motor_calculo_v2.py:616` · `app/ponto/turnos.py:1633` · `app/escala/services/cadastro_realidade.py:299` · `app/folha/export.py:1150` |
| Campos de mínimo no motor | `__init__` escreve `intrajornada_cadastrada`, `intrajornada_fonte`, `intrajornada_minutos` e `_intra_do_perfil`. `_intra_minima` lê `intrajornada_cadastrada` e `_intra_do_perfil`; os três chamadores são 12x36, partido e comercial. O AST encontrou somente escritas no atributo `intrajornada_fonte` e nenhuma leitura externa de `MotorBase.intrajornada_minutos`. A flag `TipoEscala.intervalo_indenizavel` existe, participa da validação e é repassada pela ata. O motor não a lê. `app/ponto/motor_calculo_v2.py:822–865,1845–1847,2788,3014,3265` · `app/escala/models.py:108,574` · `app/ponto/juiz_batida.py:333` |
| Pares, `intervalos_do_motor` e `intra_descontada` | O adaptador e `_periodos_pelo_marco` escrevem o mapa de pausas por entrada. Os três corpos recortam e chamam `_carimbar_segmento`, escritor de `intervalos_do_motor`. `_intra_real` tem dois chamadores, 12x36 e comercial, que escrevem `intra_descontada`. Leem os pares o split de noturno e o calculador. O calculador também lê o desconto e tem escrita própria ao materializar sua projeção. `app/ponto/adaptador_motor.py:44` · `app/ponto/motor_calculo_v2.py:687–723,1035,1858,2780–2791,2910–2911,3256–3263` · `app/ponto/calculador/alimentacao.py:108,127,133` · `app/ponto/calculador/periodo.py:361,744–754` |
| `minutos_intrajornada_indenizada` | Default no período. Escritas do motor em 12x36, partido e comercial. Leituras diretas no total mensal, no limite de HE, no espelho, em `DiaPago`, no vigia de export e no DIFF. O PDF também lê pelo nome da rubrica. O calculador tem escrita própria e soma por dia. `app/ponto/motor_calculo_v2.py:235,370,1801,2800,3019–3020,3274` · `app/ponto/services/espelho.py:104` · `app/ponto/services/dia_pago.py:186,225` · `app/folha/porta_export.py:549` · `app/ponto/management/commands/diff_calculador.py:254` · `app/relatorios/pdf_espelho.py:847` · `app/ponto/calculador/periodo.py:362,747,821` |
| Jornada líquida, HE e alertas | `minutos_trabalhados` é escrito pelos três corpos, pelo caminho de crédito e na projeção do calculador. A soma, a HE, o fechamento, `DiaPago`, espelho e vigia leem o valor. `_limite_com_suprimido` tem quatro chamadas. O alerta de intra é escrito junto da indenização e lido pelo espelho e pelo PDF. `app/ponto/motor_calculo_v2.py:296–300,2575,2795–2801,2859,2912,3019–3024,3163,3269–3275,3304,3429` · `app/ponto/calculador/periodo.py:360,745` · `app/ponto/services/fechamento.py:862,978,990,1007` · `app/ponto/services/dia_pago.py:178,239–267` · `app/ponto/services/espelho.py:136,671` · `app/folha/porta_export.py:550` · `app/relatorios/pdf_espelho.py:975` |
| Cobrança por batida de intervalo | O cartório chama `perguntar_pela_celula`; a fábrica lê faltantes da ata, aplica vencimento e dedup, e chama `perguntar`. Os motivos são `intervalo_saida` e `intervalo_volta`. O comando `detectar_intervalo_ausente --alarme` é uma via de vigia, com leitura própria de faltantes. Zerar a rubrica no motor, por si só, não cala a fábrica. `app/ponto/services/cartorio.py:1061–1073` · `app/chamados/services/fabrica_da_celula.py:52–74,139–157,218–219` · `app/chamados/services/disputa_emissao.py:901–910` · `app/chamados/services/declaracao_texto.py:43–47,87–104` · `app/config/crons.py:491` · `app/ponto/management/commands/detectar_intervalo_ausente.py:96–108,120` |

## 3. Leis herdadas

- **L-031, L-032 e L-033.** A ficha de L-031 registra a regra histórica da flag e da pergunta, com a leitura da flag adiada. O V2 substitui o aval anterior desta obra, retira o cadastro novo e dá o resultado B1. L-032 protege a dedução do cadastrado e o reconhecimento da pausa dentro do envelope. L-033 mantém pagável o dia fechado cujo único furo é a pausa. O corte **LEI-DO-MIOLO-CEDE-A-LAMPADA** preserva a via da pergunta pela célula e descreve expressamente o caso com retorno batido e ida faltante. `app/docs/LEIS.md:79–81` · `app/docs/CORTES.md:416` · `app/docs/BACKLOG.md:1634,2554` · `app/folha/export.py:1150–1171`
- **L-020, L-024/S133, L-025, L-085 e L-087.** Protegem DNA congelado, grade como fonte do previsto, universo de batidas apuráveis, dia de início da jornada e papel da batida pela ata com instante do fato. O corte **JUIZ-ESCALA-NASCE** reafirma S133. O dossiê de célula e turno registra o precedente de ler o realizado e as pausas pela autoridade de turno. `app/docs/LEIS.md:71,75–76,117,119` · `app/docs/CORTES.md:367` · `app/docs/DOSSIES.md:73`
- **L-086, L-095, L-097 e L-099.** Protegem o feriado previsto, a lavra do dinheiro do dia, a janela de HE já emendada pela NOTURNO-A e a coerência entre leitores. A forma atual da janela mantém trabalhado e noturno após o marco de saída e trata a HE conforme a L-097. `app/docs/LEIS.md:113,115,118,122` · `app/docs/RELATO.md:143`
- **L-090, L-092, L-110 e DINHEIRO.** A competência vem da janela da empresa. Para esta obra, 09/10 ficam em Pauta DP; apply da 11 segue DINHEIRO item 1, com reversão, prova de preservação da exportada e DIFF de frota da intrajornada no RELATO. O corte **REFERENCIA-E-A-LEI** e os precedentes dos dossiês O146/O214 dão a forma do RED e do contrato de rubricas existentes. `app/docs/LEIS.md:123,125,136` · `app/docs/CORTES.md:383` · `app/docs/DOSSIES.md:182,195–205,229–241` · `CLAUDE.md:418–427` · `app/docs/BACKLOG.md:2554`

## 4. Contrato de entrada

| Item | Contrato |
|---|---|
| Fonte | Vínculo do dia e DNA da célula para o desenho daquele dia; batidas apuráveis e ata/geometria para o fato. O cadastro já tem janelas e modalidade por duração. `intervalo_duracao_min` tem default 60; um valor positivo isolado no template não distingue declaração explícita de default, nem prova pausa naquele dia. `app/escala/alimentacao.py:228` · `app/escala/models.py:52–56,108–110,281–308,1177–1219` · `app/ponto/turnos.py:2940–2948` · `app/ponto/juiz_batida.py:129–150,307–327` |
| Unidade | Um **dia de jornada por colaborador**, com pares medidos em minutos, preservando segundos das batidas. A rubrica gravada é em horas decimais. A competência é dada por `janela_fechamento` com o corte da empresa. `app/ponto/motor_calculo_v2.py:602–609,936–951,1925–1928,2440–2448,370` · `app/ponto/services/fechamento.py:641,1028` · `app/ponto/janelas.py:54–63` |
| Universo e exclusões | Da competência 11 em diante, para dia com intervalo cadastrado: B1 recebe dedução e intra zero; B2 mantém indenização do suprimido. Dia sem intervalo cadastrado é controle. Para 09/10, somente Pauta DP. O comando de pauta **da exportada** exige competência entregue, portanto sua aplicabilidade à 10 por empresa depende dessa condição; a guarda geral do HEAD registra a retirada da 10. `app/docs/BACKLOG.md:2554` · `app/ponto/services/fechamento.py:36–49` · `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:107–135` |
| Escala | A apuração existente agrega por fatia do vínculo e o espelho alimenta os memos por janela. O DIFF tem como referência operacional a função real de fechamento e deve cobrir a frota, além dos três REDs do aval. Denominador da sonda, sobreposição de colaboradores entre B1 e B2, composição por empresa/ciclo e impacto em horas ou reais ficam **[nao verificado]**. `app/ponto/services/fechamento.py:152,306,774–776` · `app/ponto/services/espelho.py:1167–1194` · `app/ponto/services/diff_frota.py:45,84–87` · `CLAUDE.md:423–426` · `app/docs/BACKLOG.md:2554` |

## 5. Matriz de transições — REDs propostos

Os horários abaixo são **fixtures propostas**. B1, B2 e o controle sem cadastro são os três casos mínimos do aval. Os demais fixam fronteiras já respondidas. `app/docs/BACKLOG.md:2554` · `app/docs/LEIS.md:136`

| Entrada | Resposta pela regra | Teste proposto | [nao verificado]
|---|---|---|
| **B1**, comp. 11, 07:00–19:00, pausa fixa 12:00–13:00 no dia, só entrada e saída final | 660 min trabalhados, 60 deduzidos, intra 0, sem HE pela pausa e sem cobrança de intervalo no alcance do aval. Ver §8.1 para a fábrica de perguntas. `app/docs/BACKLOG.md:2554` · `app/docs/LEIS.md:79–81` | `test_b1_sem_batidas_de_pausa_deduz_60_e_zera_intra` |
| **B2**, mesma jornada, pausa reconhecida 12:15–12:45 | 690 min trabalhados, 30 deduzidos, 30 indenizados e zero HE pelos mesmos 30 min. `app/docs/BACKLOG.md:2554` · `app/docs/LEIS.md:80` · `app/ponto/motor_calculo_v2.py:1801–1819` | `test_b2_pausa_curta_indeniza_30_sem_he_dobrada` |
| **Sem cadastro**, 12x36, 07:00–19:00, sem janela, duração 0 e só as pontas | 720 min trabalhados, desconto cadastral 0 e indenização ordinária de 60 pelo piso nesse cenário. `app/docs/BACKLOG.md:2554` · `app/escala/models.py:6` · `app/ponto/motor_calculo_v2.py:1840–1847,2782–2801` | `test_sem_intervalo_cadastrado_preserva_o_piso_e_o_trabalhado` |
| Pausa cadastrada de 60, batida reconhecida de 60 deslocada dentro do envelope | Deduz os 60 batidos, intra 0, sem HE pela pausa. `app/docs/LEIS.md:80` · `app/ponto/motor_calculo_v2.py:2780–2801` | `test_pausa_completa_no_envelope_continua_batida` |
| B2 com suprimido de apenas 5 min, em ramo sujeito à tolerância vigente | Deduz o gozado e mantém intra 0 pela tolerância de 10 min. `app/docs/BACKLOG.md:1686,2554` · `app/ponto/motor_calculo_v2.py:69,2787–2794,3261–3267` | `test_b2_preserva_tolerancia_do_suprimido` |
| Mesmo B1 em 09/10 e, no teste de borda, turno iniciado na última data da 10 com saída na madrugada da 11 | Gravados de 09/10 intactos, divergência encaminhada à Pauta DP. O turno de borda pertence à 10. `app/docs/BACKLOG.md:2554` · `app/docs/LEIS.md:117,123` · `app/ponto/motor_calculo_v2.py:2440–2448` | `test_b1_competencias_09_10_so_pauta_dp` e `test_virada_da_competencia_respeita_inicio_do_turno` |

## 6. Padrão nomeado

**Single Source of Truth / Aggregate Root — célula soberana / fonte única.** O desenho do dia vem da célula e do vínculo que a gerou. O cálculo consome esse desenho com o fato já julgado pela ata ou pela geometria. O padrão de prova de frota é **Shadow mode / Parallel run**, com o resultado da função real de fechamento. `app/docs/GLOSSARIO.md:11,22` · `app/docs/LEIS.md:71,75,136,144` · `app/ponto/services/diff_frota.py:45,84–87`

## 7. Arquivos tocados × obras em voo

**Escopo previsto para a construção:** `app/ponto/motor_calculo_v2.py` e os REDs em `app/ponto/tests/`. O caminho de teste é uma proposta. O `!` do motor é exclusivo deste item, e o aval exclui cadastro novo na UI. O pouso da NOTURNO-A consta do RELATO e o commit `0d1e5ddf5` é ancestral do HEAD. A ordem registrada para o motor põe FERIADO e PAR-PELO-TURNO V6 antes do INTERVALO V2. `CLAUDE.md:143,315,412` · `app/docs/BACKLOG.md:2020,2554` · `app/docs/PROMPTS.md:2943` · `app/docs/RELATO.md:143`

Comparei as cópias existentes com seu ancestral comum ao HEAD e li os dois arquivos, incluindo a alteração local do PAR c7. As linhas a seguir são do HEAD; os hashes identificam as cópias conferidas. `app/docs/RELATO.md:3,5–6,14,16–17`

| Obra e cópia conferida | `app/ponto/motor_calculo_v2.py` | `app/ponto/turnos.py` |
|---|---|---|
| **FERIADO-SEM-LIMITE-DO-SUPRIMIDO**, c4d `28eb07f5d` | **Colisão de arquivo e dependência de dados.** Na faixa pedida **3070–3129**, o diff troca o corpo da dobra em **3115–3123** e a mensagem em **3126**; há outro trecho logo adiante. A detecção da pausa e as escritas de intra ficam em outros trechos do HEAD. A dobra lê justamente `minutos_trabalhados` e `minutos_intrajornada_indenizada`, então há efeito conjunto mesmo onde os hunks não se cruzam. A c4e fica **[nao verificado]**. `app/ponto/motor_calculo_v2.py:689–723,2787–2801,3014–3024,3070–3133,3258–3275` · `app/docs/BACKLOG.md:1998` · `app/docs/RELATO.md:5,13251` | Sem hunk próprio da obra. A diferença direta contra o HEAD nessa cópia corresponde à base anterior à PARCIAL-BORDA. `app/docs/RELATO.md:13,13246–13252` · `app/ponto/turnos.py:2644–2729` |
| **E1-PREVISAO-24H**, v6 `f8d398282` | **Colisão de arquivo.** A v6 troca **3404–3428**, no limite diário do comercial, pela leitura do previsto S133. Esse limite alimenta o mesmo `_limite_com_suprimido` que lê o resultado da intra. A versão v4 descrita no RELATO preservava o motor; o diff da v6 já toca o arquivo. `app/ponto/motor_calculo_v2.py:3404–3429` · `app/docs/RELATO.md:16–17` | Sem diff nesse arquivo na cópia v6. A interface de interesse para esta fatia continua sendo previsto e intervalo declarado. `app/escala/utils.py:2251–2284` · `app/docs/BACKLOG.md:2006` |
| **PAR-RELAMPAGO-A-LISTA-DO-ADMIN**, `61c5323a0` + alterações locais c7 | Sem diff no motor inspecionado. `app/docs/RELATO.md:6,132` · `app/ponto/motor_calculo_v2.py:453–506` | **Arquivo de dependência com diff próprio.** Toca `turnos_do_colab`, reapresentação de batida, consulta de par completo e dia da batida. Colide por arquivo com DIA-DA-BATIDA; o escopo previsto do INTERVALO apenas consome a geometria. `app/ponto/turnos.py:1850–1920,2185–2218,2951–3001` · `app/docs/BACKLOG.md:2337` |
| **DIA-DA-BATIDA-JUIZ-UNICO**, `a4d3675c7` | Sem diff no motor inspecionado. O mapa do dia do par continua sendo um insumo do caminho de cálculo. `app/ponto/motor_calculo_v2.py:936–951,2362–2365` · `app/docs/RELATO.md:3` | **Arquivo de dependência com diff próprio.** Muda as consultas de dia por batida e a carga em lote. Cruza o PAR na região de `dia_das_batidas` e `dia_do_turno_da_batida`. `app/ponto/turnos.py:2951–3020,3113–3116` · `app/docs/RELATO.md:3` |

##

[truncado em 20000 de 21772 caracteres]

## 26/11 10:22 · sessao 0cbbfb568f2f · DIA-DA-BATIDA-JUIZ-UNICO · verificador · origem=Code

1. **nota | O achado 1 da F1 está curado na origem do mapa por PK.** A casca chama `_carregar_dias_das_batidas([cid], ini, fim)`. O lote chama a mesma função com todos os IDs, reaproveita `_ata` e guarda o resultado. Ambos passam por `_turnos_do_colab_puro` → `parear_turnos` → `_dia_das_batidas_puro`. O adaptador unitário consulta esse mapa por PK. A assinatura perdeu `turnos=` e a varredura dos chamadores Python, incluindo os aliases da API, encontrou apenas chamadas com colaborador e datas na casca, ou com colaborador no lote. `app/ponto/turnos.py:2951`, `app/ponto/turnos.py:2975`, `app/ponto/turnos.py:2991`, `app/ponto/turnos.py:3056`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3157`, `app/api/views.py:1444`, `app/api/views.py:2042`, `app/colaboradores/services/pos_vinculo.py:43`, `app/ponto/services/espelho.py:465`, `app/core/services/painel_op.py:330`, `app/ferias/management/commands/listar_ferias_com_batida.py:54`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`

2. **nota | O histórico é inteiro e o pad seleciona as chaves.** A consulta usa `batidas_apuraveis(None)` sem datas, filtra pelos IDs e ordena por colaborador, timestamp e PK. Retratada fica fora e pendência de aprovação continua apurável. Para cada colaborador com alguma PK apresentada, os limites do pareamento são a véspera da primeira marca e o dia civil da última. O lote consulta células numa faixa agregada e devolve a cada colaborador seu recorte histórico. Com os mesmos fatos e fuso, `ini/fim` só determina quais PKs saem. O pad civil é `[ini−2, fim+1]`, inclusivo nas duas pontas. Dentro do pareador, o recorte por **instante** do segmento inclui a meia-noite de `seg_ini−2` e exclui a de `seg_fim+2`. O mapa começa com o dia civil e põe o `data_turno` nas chaves reclamadas. `app/ponto/turnos.py:45`, `app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1980`, `app/ponto/turnos.py:1988`, `app/ponto/turnos.py:2924`, `app/ponto/turnos.py:2941`, `app/ponto/turnos.py:3020`, `app/ponto/turnos.py:3026`, `app/ponto/turnos.py:3040`, `app/ponto/turnos.py:3056`, `app/ponto/turnos.py:3003`

3. **nota | Há mudança além dos três casos do RED.** Reproduzi em memória, com o pareador da cópia e carga simulada, um vínculo 19h–07h sem DNA, com E às 02h e S às 07h **ambas em 25/10**. Para `[25/10, 25/10]`, a F1 pareia as duas, descarta o turno de 24/10 pelo recorte e devolve **25/10** para as duas PKs. A F2 devolve **24/10**. Também reproduzi uma mudança causada pela borda direita: E em 25/10 às 05h e S em 26/10 às 06h, no mesmo tipo de vínculo. Para `[24/10, 24/10]`, a F1 vê a E como turno aberto de **24/10**. A F2 carrega a S, fecha o par e publica **25/10** para a E, pois a saída excede o envelope da jornada da véspera. A S fica fora das chaves apresentadas nessa janela. `app/ponto/turnos.py:229`, `app/ponto/turnos.py:299`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:1497`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3027`, `app/ponto/turnos.py:3056`

4. **bloqueia | A resposta ainda diverge da autoridade registrada.** `_T1` aponta para `turnos_do_colab`, que conserva a carga recortada. No próprio RED das órfãs, reproduzi a última S com `Turno.data_turno = 24/10` em `turnos_do_colab(24/10, 26/10)` e **25/10** no mapa F2 para a mesma PK e janela. É o mesmo pareador com cargas que produzem respostas diferentes. O espelho usa o mapa para marcas e cartões, mas usa os turnos recortados para `datas_saida_orfa`. O painel também cruza o mapa histórico com `dias_encerrados` calculado sobre turnos da janela. `app/core/juizes.py:216`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3056`, `app/ponto/turnos.py:3142`, `app/ponto/turnos.py:3148`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:66`, `app/ponto/services/espelho.py:465`, `app/ponto/services/espelho.py:589`, `app/ponto/services/espelho.py:628`, `app/core/services/painel_op.py:330`

5. **bloqueia | O signal do cartório e o evento do fechamento ainda escolhem pelo dia civil.** `_dias_batida` entrega `(D−1, D)` ao cartório no `post_save` e no `post_delete`. Para a S de 25/10 do RED de 24h10, esse receiver pede 24 e 25/10, enquanto o mapa atribui a S a **23/10**. O receiver do núcleo pede outro recorte, `D−2..D+1`. Já o evento de fechamento recebe o timestamp na criação e na retratação, converte para a data civil e agenda uma única competência. Com corte em 24/10, a jornada de 23/10 e a saída de 25/10 caem em competências distintas, e esse evento agenda a da saída. Efeito final nas células e no gravado em produção [nao verificado]. `app/ponto/signals.py:23`, `app/ponto/signals.py:26`, `app/ponto/signals.py:92`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:61`, `app/ponto/registro_batida.py:176`, `app/ponto/registro_batida.py:301`, `app/ponto/services/fechamento.py:1593`, `app/ponto/services/fechamento.py:1619`, `app/ponto/services/fechamento.py:1833`, `app/ponto/janelas.py:24`

6. **nota | Escritores e leitores.**
   - **Mapa `{pk: dia}`.** `_carregar_dias_das_batidas` monta a projeção civil de entrada e `_dia_das_batidas_puro` publica o mapa em memória. Leem a adesão reativa, o espelho, as duas rotas da API, o painel, o diagnóstico de férias, o vigia de plantio e o adaptador unitário. O signal da adesão chega ao mapa pelo serviço pós-vínculo. O PDF lê as linhas do espelho. `app/ponto/turnos.py:3001`, `app/ponto/turnos.py:3026`, `app/colaboradores/services/pos_vinculo.py:43`, `app/ponto/signals.py:234`, `app/ponto/services/espelho.py:589`, `app/api/views.py:1461`, `app/api/views.py:2046`, `app/core/services/painel_op.py:330`, `app/ferias/management/commands/listar_ferias_com_batida.py:54`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`, `app/ponto/turnos.py:2991`, `app/relatorios/pdf_espelho.py:596`
   - **`Turno.data_turno` e `TurnoMaterializado.data_turno`.** `_mk` publica o dia calculado por `_data_do_turno`. O mapa copia esse dia, o espelho e a API também leem turnos diretamente, e o núcleo copia o campo para `TurnoMaterializado` via `recompute_turnos`, a porta registrada. Painel e reconciliador leem a materialização. No fechamento mensal, o motor monta outro mapa, por instante da entrada e depois por identidade do período, e o fechamento passa `resultado.dia_da_jornada` à lavratura. `app/ponto/turnos.py:285`, `app/ponto/turnos.py:312`, `app/ponto/turnos.py:3009`, `app/ponto/services/espelho.py:628`, `app/api/views.py:1447`, `app/ponto/nucleo.py:47`, `app/ponto/nucleo.py:59`, `app/core/portas.py:118`, `app/core/services/painel_op.py:222`, `app/chamados/reconciliador.py:97`, `app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:2961`, `app/ponto/services/fechamento.py:1241`
   - **Outros campos chamados `data_turno`.** Os dois kernels da API gravam a **data civil** no `contexto_json['data_turno']` de `batida_fora_escala_leve`, e o catálogo lê essa chave como dia do chamado. A regularização também usa o dia civil da entrada em `Justificativa.data_turno`, lido pelo fechamento. O instante da marca está em `Batida.timestamp`. Na adesão, o emissor grava `periodo` e `data_inicio`, e o pós-vínculo compara os valores do mapa aos dois limites inclusivos de `periodo`. `app/api/views.py:717`, `app/api/views_core.py:711`, `app/chamados/catalogo/modulos.py:168`, `app/chamados/catalogo/modulos.py:219`, `app/ponto/services/regularizacao.py:73`, `app/ponto/services/regularizacao.py:91`, `app/ponto/services/fechamento.py:680`, `app/ponto/models.py:23`, `app/chamados/services/adesao.py:86`, `app/colaboradores/services/pos_vinculo.py:38`

7. **fila | A adesão continua com duas regras de evidência.** O fluxo reativo exige dia do mapa dentro do período do pai quando há dois limites. O cron aceita qualquer batida apurável do colaborador. Ambos passam por `transicionar_com_pauta` → `pai.reconciliar` → `_gravar_estado`. A F2 escreve o mapa em memória e o fechamento mantém a ordem **Colaborador → Disputa → Chamado → Pergunta**. A evidência é consultada antes da trava do gate. Comportamento sob retratação concorrente [nao verificado]. `app/colaboradores/services/pos_vinculo.py:37`, `app/colaboradores/services/pos_vinculo.py:49`, `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/chamados/services/adesao.py:143`, `app/chamados/services/fecho_runtime.py:157`, `app/chamados/models.py:670`, `app/chamados/models.py:777`, `app/chamados/models.py:2939`, `app/chamados/models.py:2948`, `app/chamados/models.py:2961`, `app/chamados/models.py:2965`

8. **nota | Relógio e mutantes.** A carga F2 calcula os limites pelos timestamps das marcas. Há uma leitura de relógio herdada em `periodos_do_dia` → `intervalo_cadastrado`, que chama `date.today()` sem `dia`. Ela alimenta intervalo e perguntas, enquanto a projeção de papéis usa as bordas e luzes da ata. O cartório também lê o relógio no julgamento disparado pelo signal. Nos testes em memória, com a carga simulada e os métodos F1 reconstituídos do diff, os três casos F2 passaram. **`borda`** falhou na igualdade entre janelas nos dois REDs, **`lote`** na igualdade dos mapas nos dois, **`civil`** no dia esperado da saída de 24h10, e **`sem_pad`** na igualdade do conjunto de PKs, com três chaves a mais. Execução com Django e banco [nao verificado]. `app/ponto/turnos.py:3030`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1682`, `app/ponto/juiz_batida.py:129`, `app/ponto/juiz_batida.py:274`, `app/ponto/juiz_batida.py:328`, `app/ponto/services/cartorio.py:2084`, `app/ponto/services/cartorio.py:2105`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:53`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:54`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:58`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:72`

9. **fila | Menos consultas por chamada, mais histórico relido nos laços.** Pela leitura do código, a casca F2 faz **4–5 consultas** com PK apresentada, ou **1** com o pad vazio, ante **5–8** na F1. São batidas, células, candidatos, vínculo da ata e eventualmente geradoras faltantes. Conte uma busca pelos pais e uma carga completa **por pai com período**, inclusive para períodos iguais. O signal repete isso a cada `save` de `Batida`, e o pós-vínculo chama a adesão depois do laço diário. Há outras multiplicações por dia/ausência no diagnóstico de férias, por marca na emissão de disputa e por marca na foto E4B, sob trava do colaborador. Na foto, **B** chamadas que leem **B** marcas cada já dão volume quadrático só na leitura das marcas. O painel e o vigia de plantio reutilizam uma carga por lote e por colaborador, respectivamente. SQL executado, latência e frequência em produção [nao verificado]. `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:1903`, `app/ponto/turnos.py:1914`, `app/ponto/turnos.py:3020`, `app/ponto/turnos.py:3040`, `app/ponto/turnos.py:3045`, `app/escala/alimentacao.py:148`, `app/colaboradores/services/pos_vinculo.py:37`, `app/colaboradores/services/pos_vinculo.py:95`, `app/ponto/signals.py:206`, `app/ponto/signals.py:234`, `app/ferias/management/commands/listar_ferias_com_batida.py:43`, `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/foto_e4b.py:225`, `app/chamados/services/foto_e4b.py:250`, `app/chamados/services/foto_e4b.py:282`, `app/core/services/painel_op.py:327`, `app/ponto/management/commands/vigia_plantio_overnight.py:82`

10. **nota | A catraca e o R6 batem com a cópia.** O B1 está em `_carregar_dias_das_batidas:3026` e `dia_do_turno_da_batida:2989`, com identidade e linhas registradas. Rodei em memória os **35 testes** da catraca, com **9 pareadores e 81 derivações**, os **10 testes** do contrato de projeção civil e o selo R6. Todos passaram. O R6 da cópia publica **18/20 verdes, 20 declaradas**, e a célula de juiz único segue vermelha pelo registro de dívida. Os quatro arquivos do diff batem com os blobs de destino do anexo. Hash do commit HEAD, suíte com banco e contagens F2 das competências 10 e 11 [nao verificado]. `app/core/tests/test_medida_turno_do_par.py:108`, `app/core/tests/test_medida_turno_do_par.py:238`, `app/core/tests/test_medida_turno_do_par.py:338`, `app/core/tests/test_medida_turno_do_par.py:546`, `app/core/tests/test_medida_turno_do_par.py:562`, `app/ponto/tests/test_contract_no_batida_date.py:650`, `app/core/tests/test_selo_r6_frase_do_juiz.py:20`, `app/core/placar_estrutural.py:443`, `app/core/contratos_estruturais.py:225`

_8337 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
