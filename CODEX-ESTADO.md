# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 12:07** · publicado em **27/11/2026 12:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 12:07 · sessao 914f194adfe1 · ORFAS-VOLTOU · sonda · origem=Code

- **Defeito.** A sonda mostrou dois `reg10`, das rubricas `0025` e `0243`, com `00000VIGIA` no campo `matric`. O layout exige que os dez caracteres desse campo passem em `isdigit()`. As letras explicam os dois erros `reg10 campo matric nao-numerico`. [app/folha/export.py:272](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:272)
- **Escritor e fonte.** `montar_linhas_export` lê `fech.colaborador.codigo_dominio` na linha 737. O `strip().zfill(10)` preenche com zeros e preserva as letras. No mapa exemplificado na cópia, `0025` é adicional noturno e `0243` é intrajornada, ambos emitidos pela chamada a `montar_linha_10` na linha 747. O serializador grava a matrícula na linha 45. O mapa ativo da empresa 2 na sondagem é [nao verificado]. [app/folha/export.py:737](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:737) [app/folha/export.py:747](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:747) [app/folha/export.py:45](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:45) [app/folha/tests/test_sm1_monster_export.py:75](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/tests/test_sm1_monster_export.py:75)
- **Dono pela L-099: CADASTRO.** O valor inválido vem do campo cadastral `Colaborador.codigo_dominio`. `classificar_export` só verifica se esse campo está vazio, então a exigência de matrícula numérica é aplicada na validação do TXT. `dono_da_divergencia` julga divergências de dia por colaborador e fica fora desse caminho. Quem gravou o valor é [nao verificado]. [CLAUDE.md:183](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/CLAUDE.md:183) [app/colaboradores/models.py:311](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/models.py:311) [app/folha/export.py:881](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:881) [app/ponto/management/commands/e6_oraculo.py:53](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:53)
- **TXT real de 11/2026, empresa 2.** Com o `sem_medicao` visto na sombra, o clique solicita a conferência e a porta segura a geração até ficar `pronta`. Mesmo com a porta pronta, basta que a montagem real reproduza uma dessas linhas para `gerar_dominio_txt` recusar por falha de layout, antes de entregar ou registrar o arquivo. A porta recalcula os fechamentos antes de montar. O estado da porta real hoje, as linhas que esse recálculo produziria e o resultado efetivo da tentativa ficam [nao verificado]. [app/folha/porta_export.py:817](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:817) [app/folha/porta_export.py:1423](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:1423) [app/folha/views.py:135](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:135) [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:275](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:275) [app/folha/views.py:282](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:282)

## 27/11 12:03 · sessao 22ad6ec9cbe8 · ORFAS-VOLTOU · sonda · origem=Code

Na montagem em memória do TXT de **11/2026**, a sonda identificou **87 células com o colaborador fora**, **5 com o colaborador dentro e a data sem `reg11`** e **337 que acionaram `furo_espelho`**. A presença no TXT ficou **[nao verificado]** para as 561 células da empresa 2. `sonda.py`, saída fornecida, linhas 4–7.

| Empresa | Células | Colab. fora do TXT | Colab. dentro, data sem `reg11` na janela | Células que retêm | Efeito direto em rubrica/horas | Fonte |
|---|---:|---:|---:|---:|---:|---|
| 2 | 561 | [nao verificado] | [nao verificado] | 286 | 0 | `sonda.py`, saída linha 4 |
| 3 | 91 | 87 | 4 | 51 | 0 | `sonda.py`, saída linha 5 |
| 4 | 1 | 0 | 1 | 0 | 0 | `sonda.py`, saída linha 6 |
| **Total** | **653** | **87 verificados** | **5 verificados** | **337** | **0** | `sonda.py`, saída linha 7 |

**As somas fecham nas 653 células**, com zero fora das empresas 2–4. Por colaborador, são 5 dentro + 87 fora + 561 com presença indeterminada. Por data, são 137 fora da janela + 0 com `reg11` verificado + 5 sem `reg11` com colaborador dentro + 68 sem `reg11` com colaborador fora + 443 datas indeterminadas na janela. As 337 retenções se sobrepõem a essas contagens. A sonda encontrou 1.895 perguntas, 189 colaboradores no filtro, 179 com célula e 12 perguntas sem FK de célula. A referência do anexo traz 184 colaboradores e 658 células. A identidade entre os dois conjuntos de células é **[nao verificado]**. `sonda.py`, saída linhas 1–8. [app/folha/export.py:977](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:977)

A saída trouxe `layout_invalido` na empresa 2 e `sem_medicao` nas três empresas. A retenção vem da classificação, que exclui o colaborador inteiro da montagem quando o motivo é `furo_espelho`. O erro específico de layout da empresa 2 e a emissão efetiva do TXT são **[nao verificado]**. `sonda.py`, saída linhas 4–6. [app/folha/export.py:266](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:266) [app/folha/export.py:725](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:725) [app/folha/export.py:977](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:977) [app/folha/porta_export.py:1368](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:1368)

O **zero direto** vale para fechar apenas a pergunta, mantendo os insumos da folha. O `reg11` registra data de falta. A contribuição de um dia sem `reg11` às rubricas mensais e o efeito de materializar uma resposta ou decidir o dia são **[nao verificado]**. [app/folha/export.py:215](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:215) [app/folha/export.py:734](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:734) [app/chamados/services/validacao.py:777](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/validacao.py:777)

## 27/11 12:02 · sessao aac47beae035 · FERIADO-BOTAO-CORTADO · construtor · origem=Code

Adicionei um único arquivo, [test_smoke_clique_feriado_vira_trabalho.py](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:1). A fixture usa datas fixas e confere que a função real oferece a ação em 20/11/2026, com a célula em folga e quatro batidas. [Fixture e conferência](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:144)

- **Página × `base.html` e `base_app.html`** — renderiza o calendário mensal real no conteúdo de cada casca. [Renderização](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:209)
- **Drawer × `base.html` e `base_app.html`** — abre o drawer genérico e clica em Espelho, com as respostas das duas views fornecidas em memória. [Abertura do drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:60)

Nos quatro cenários, o Chromium confere o `summary` dentro da célula e da viewport, clica no elemento atingido no centro do botão e verifica abertura, painel inteiro na viewport, distância de até 16 px e `action` do formulário igual à porta real. A rolagem só registra deslocamento, evento e fechamento. [Medições](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:89)

O **RED esperado no drawer** vem de `popover-acao.js:44` e `:75–76`. O script aplica `position:fixed` e grava coordenadas da viewport, mas o `transform` do drawer muda a referência do `fixed`. O `backdrop-filter` de `.hx-shell` também merece leitura nas duas páginas. O efeito geométrico em ambos os casos segue [nao verificado]. [Posicionador](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:38) [Drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:16) [CSS da casca](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/css/hasner-ponto.css:283)

**Comandos para o Code**, a partir de `app/`. Os mutantes alteram o HTML do ensaio. [Mutantes CSS](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:38) [Mutante da porta](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:192)

```bash
alvo=core.tests.test_smoke_clique_feriado_vira_trabalho
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=gatilho-cortado PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=painel-recortado PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=painel-distante PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=porta-trocada PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
```

A sintaxe Python passou. A execução na cópia parou no import de `django.core`, antes do banco de teste e do Chromium. Resultado dos quatro cenários, rolagem, mutantes, envio do POST e comportamento em produção seguem [nao verificado]. [Import do Django](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/manage.py:11) [nao verificado]

## 27/11 11:59 · sessao ba05ad29f6be · ORFAS-VOLTOU · leitura · origem=Code

## 1. TXT do Domínio — competência 11/2026

**A pergunta órfã aberta, por si só, não tira colaborador ou dia, não altera rubrica ou horas e não bloqueia a geração. A célula pode tirar o colaborador inteiro.** O POST calcula `FechamentoMensal` em memória, classifica e monta com os mesmos objetos. A foto gravada ainda serve de guarda para abrir a geração e de alvo do carimbo de aprovação. O `DiaPago` gravado entra na conferência da porta, sem alimentar as linhas do TXT. `app/folha/views.py:132–173,214–219,249–288` `app/folha/porta_export.py:319–341,392–398,693–706,1364–1398`

- **Retenção.** `classificar_export → motivos_retencao_espelho → motivos_retencao_celula → _dias_sem_decisao` lê veredito, protestos, vínculo e, na exceção da pausa, ata/DNA. `indefinida`, `furo` e `nunca_bateu` acusam. Aplicados o recorte e as dispensas, um dia que retém põe o colaborador inteiro em `fora/furo_espelho`. A guarda de cadastro zero também consulta a existência de células para explicar a retenção. `app/escala/models.py:1780–1786` `app/folha/export.py:945–982,1157–1182,1214–1222,1244–1316`
- **Apuração e emissão.** O fechamento alimenta o motor com célula/ata, batidas apuráveis, escalas e ausências. Também lê a grade da célula para os totais de previsto, abono e realizado e chama o mesmo retentor. A montagem traduz os totais mensais pelo catálogo e lê ausências aprovadas para o bloco datado de faltas. Nenhum desses caminhos consulta `PerguntaDisputa` ou o estado da disputa. A conferência do POST lê o carimbo da porta. O caminho de badge que consulta chamados por célula é de modo gestor, enquanto o medidor chama o espelho no modo padrão. `app/ponto/services/fechamento.py:617–659,762–776,1037–1092` `app/ponto/motor_calculo_v2.py:638–675` `app/folha/export.py:71–87,169–185,402–461,720–755` `app/folha/porta_export.py:482–502,1423–1437` `app/ponto/services/espelho.py:613–621,1230`
- **Medida honesta.** Presença do colaborador é matrícula em `reg10` efetivamente emitido. Presença de data é `reg11` ligado ao `reg10` do bloco de faltas. Ausência de `reg11` deixa a contribuição daquele dia às rubricas mensais **[nao verificado]**. Para o contrafactual, fechar só a pergunta, mantendo os insumos, tem **efeito direto zero por leitura do código**. Validar uma resposta ou decidir o dia pode plantar batida, aprovar ausência ou mudar a célula. O efeito desse ato exige definir o desfecho e rodar o DIFF antes/depois em sombra com escrita. Função pura que reproduza esse ato até o TXT **[nao verificado]**. `app/folha/export.py:215–255,734–755,977–982` `app/chamados/services/validacao.py:777–805` `app/chamados/services/materializacao.py:1584–1592` `app/ponto/services/veredito_celula.py:324–357`

O anexo registra **1.895 perguntas, 184 colaboradores, 658 células e 12 perguntas sem FK de célula**. A sonda refaz o mesmo filtro e mede o estoque que encontrar na sombra. Identidade com as 658 originais, totais por empresa e resultado do TXT **[nao verificado]**. A coluna `celulas_retendo` conta causas lidas no retentor quando `furo_espelho` é o motivo efetivo. As colunas de presença contam células, inclusive quando várias pertencem ao mesmo colaborador. `autopsia_v2.md:3–4` `r2.md:3–17` `app/folha/export.py:977–982,1202–1204,1268–1271`

### Rascunho da sonda

Rodar via stdin no `manage.py tenant_command shell --schema=juliani --settings=config.settings.sombra`. O comentário inicial dá o arranque sem configurar os handlers de arquivo. O código lê o estado da porta sem dispará-la e mostra a montagem em memória mesmo se a porta estiver fechada. Execução da sonda **[nao verificado]**. `bin/sonda_leitura.sh:235–241` `app/config/settings/saas.py:118–143,160–184` `app/config/settings/sombra.py:27–50` `app/folha/porta_export.py:1360–1398,1423–1437`

```python
# Do diretório app/, com /app/logs já existente, passar este bloco por stdin:
# python -B -c 'import os,runpy; from django.conf import settings; os.path.isdir("/app/logs") or __import__("sys").exit("[nao verificado]"); os.environ["DJANGO_SETTINGS_MODULE"]="config.settings.sombra"; settings.LOGGING_CONFIG=None; runpy.run_path("manage.py",run_name="__main__")' tenant_command shell --schema=juliani --settings=config.settings.sombra

from collections import Counter, defaultdict
from datetime import date
import logging

from django.db import connection, transaction
from chamados.models import PerguntaDisputa
from colaboradores.models import Empresa
from core.observ import engolidos
from escala.models import CelulaDia
from folha.export import (
    STATUS_NO_TXT, classificar_export, codigo_no_sistema_dominio,
    conteudo_do_txt, janela_competencia, montar_linhas_export,
    motivos_retencao_celula, partes_da_linha, validar_layout_dominio,
)
from folha.porta_export import estado_da_porta
from ponto.models import FechamentoMensal
from ponto.services.fechamento import (
    recalcular_fechamento_mes, universo_da_competencia,
)

COMP = (11, 2026)
EMPRESAS = (2, 3, 4)
NV = "[nao verificado]"
CAMPOS = (
    "celulas", "colabs_com_celula", "colab_no_txt", "colab_fora_txt",
    "colab_nv", "fora_janela", "reg11", "sem_reg11_colab_no_txt",
    "sem_reg11_colab_fora_txt", "dia_nv", "celulas_retendo",
    "rubrica_horas_efeito_direto", "contrafactual_material_nv",
)


class ContadorDeAvisos(logging.Handler):
    def __init__(self):
        super().__init__(logging.WARNING)
        self.total = 0

    def emit(self, record):
        self.total += 1


def medir_empresa(empresa, celulas):
    ini, fim = janela_competencia(empresa, *COMP)
    n = Counter(celulas=len(celulas),
                colabs_com_celula=len({colab for _, colab, _ in celulas}),
                contrafactual_material_nv=len(celulas))
    porta = estado_da_porta(empresa, *COMP)["codigo"]
    tem_gravado = FechamentoMensal.objects.filter(
        colaborador__empresa=empresa, mes=COMP[0], ano=COMP[1],
        status__in=STATUS_NO_TXT,
    ).exists()

    matriculas, datas, retendo = {}, defaultdict(set), set()
    try:
        codigo_no_sistema_dominio(empresa)
    except ValueError:
        montagem = "sem_integracao"
    else:
        fechs = recalcular_fechamento_mes(
            *COMP, empresa_id=empresa.pk, somente_leitura=True,
            lavrar_dias_pagos=False, retornar_objeto=True,
        )
        esperados = universo_da_competencia(*COMP, empresa.pk).values_list("pk", flat=True)
        if fechs.keys() != set(esperados):
            raise RuntimeError("universo_fechamento_incompleto")

        itens = classificar_export(empresa, *COMP, fechamentos=list(fechs.values()))
        linhas, elegiveis = montar_linhas_export(
            empresa, *COMP, itens=itens, fechamentos=list(fechs.values()),
        )
        montagem = "layout_invalido" if validar_layout_dominio(linhas) else (
            "valido" if linhas else "sem_linhas"
        )
        if montagem != "layout_invalido":
            for colab in elegiveis:
                fech = fechs[colab]
                if fech.status in STATUS_NO_TXT:
                    matriculas[colab] = fech.colaborador.codigo_dominio.strip().zfill(10)
            atual = None
            for linha in conteudo_do_txt(linhas).splitlines():
                parte = partes_da_linha(linha)
                if parte is None:
                    raise RuntimeError("linha_ilegivel")
                if parte["reg"] == "10":
                    atual = parte["matricula"]
                    datas.setdefault(atual, set())
                elif atual is not None:
                    datas[atual].add(parte["data"])
                else:
                    raise RuntimeError("reg11_sem_reg10")

        alvos = {colab for _, colab, _ in celulas}
        for item in itens:
            colab = item["colaborador_id"]
            if colab in alvos and item["motivo"] == "furo_espelho":
                _, dias = motivos_retencao_celula(
                    fechs[colab].colaborador, ini, fim, com_dias=True,
                )
                retendo.update(d["celula_id"] for d in dias)

    donos = Counter(matriculas.values())
    for pk, colab, dia in celulas:
        mat = matriculas.get(colab)
        incerto = montagem in ("sem_integracao", "layout_invalido") or (
            mat in datas and donos[mat] > 1
        )
        presente = not incerto and mat in datas
        n["colab_nv" if incerto else "colab_no_txt" if presente else "colab_fora_txt"] += 1
        n["celulas_retendo"] += pk in retendo

        if not ini <= dia <= fim:
            n["fora_janela"] += 1
        elif incerto:
            n["dia_nv"] += 1
        elif not presente:
            n["sem_reg11_colab_fora_txt"] += 1
        elif dia.strftime("%Y%m%d") in datas[mat]:
            n["reg11"] += 1
        else:
            n["sem_reg11_colab_no_txt"] += 1

    if n["celulas"] != sum(n[k] for k in CAMPOS[2:5]):
        raise RuntimeError("soma_colab")
    if n["celulas"] != sum(n[k] for k in CAMPOS[5:10]):
        raise RuntimeError("soma_dia")
    return n, (porta, int(tem_gravado), montagem)


def sondar():
    cfg = connection.settings_dict
    if (cfg["NAME"], cfg["HOST"], connection.schema_name) != (
        "sombra", "sombra_db", "juliani",
    ) or not connection.get_autocommit() or connection.in_atomic_block:
        raise RuntimeError("envelope")

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cur.execute(
                "SELECT current_database(), current_schema(), "
                "current_setting('transaction_isolation'), "
                "current_setting('transaction_read_only')"
            )
            if cur.fetchone() != ("sombra", "juliani", "repeatable read", "on"):
                raise RuntimeError("envelope_sql")

        perguntas = PerguntaDisputa.objects.filter(
            data_evento__gte=date(2026, 10, 21), chamado_destino__isnull=True,
            via_resolucao="", validada_em__isnull=True, materializada_em__isnull=True,
        ).order_by()
        com_fk = perguntas.filter(celula_id__isnull=False)
        por_empresa = defaultdict(list)
        for pk, emp, colab, dia in CelulaDia.objects.filter(
            pk__in=com_fk.values("celula_id"),
        ).values_list("pk", "colaborador__empresa_id", "colaborador_id", "data"):
            por_empresa[emp].append((pk, colab, dia))

        universo = (perguntas.count(),
                    perguntas.values("colaborador_id").distinct().count(),
                    sum(map(len, por_empresa.values())),
                    perguntas.filter(celula_id__isnull=True).count())
        saida = [
            ("universo_perguntas_colabs_celulas_sem_fk", universo),
            ("referencia_anexo", (1895, 184, 658, 12)),
            ("colunas", CAMPOS),
        ]
        total = Counter()
        for emp_id in EMPRESAS:
            n, estado = medir_empresa(Empresa.objects.get(pk=emp_id), por_empresa[emp_id])
            total.update(n)
            saida.append((emp_id, tuple(n[k] for k in CAMPOS), estado))
        fora = sum(len(v) for emp, v in por_empresa.items() if emp not in EMPRESAS)
        if total["celulas"] + fora != universo[2]:
            raise RuntimeError("soma_universo")
        return saida + [("total_234", tuple(total[k] for k in CAMPOS)),
                        ("celulas_fora_234", fora)]


raiz = logging.getLogger()
handlers, nivel = raiz.handlers[:], raiz.level
avisos = ContadorDeAvisos()
raiz.handlers = [avisos]
raiz.setLevel(logging.WARNING)
antes = engolidos()
try:
    saida = sondar()
    if avisos.total or engolidos() != antes:
        raise RuntimeError("leitura_incompleta")
except Exception:
    raise SystemExit(f"SONDA_INCOMPLETA {NV}") from None
finally:
    raiz.handlers, raiz.level = handlers, nivel

for registro in saida:
    print(*registro)
print("CONTRAFACTUAL_MATERIAL", NV)
```

## 2. Onde mora o número

- **Autoridade.** `perguntas_orfas_reatavies(colab)` define o universo elegível. O comando soma esse queryset por colaborador e publica a medida fora da soma que dá RC 2. Quem tenta o reate de fato é `reatar_pergunta_orfa`. O cron encaminha a saída a `logs/reconciliador.log` a cada 15 minutos. **3.735 às 11h** é a leitura do anexo. Valor atual **[nao verificado]**. `app/chamados/reconciliador.py:1640–1695` `app/chamados/management/commands/reconciliar_chamados.py:57–58,97–98,115–124` `app/config/crons.py:395,1913–1917` `autopsia_v2.md:17`
- **Publicação.** `app/core/placar_estrutural.py::placar()` lê `RESULTADOS` em memória. Para esta medida, hoje ele **nem chama a função real nem lê o log**. O placar estrutural que chega ao leitor é o do **`ESTADO.md`**, gerado em `bin/gerar_estado.py` e publicado junto com o RELATO por `bin/relato.sh`. `app/core/esteira_vigia.py` fornece o escritor e a trava do RELATO. `hook_stop_fila1` lê as duas linhas operacionais do topo. `handoff_sessao.sh` escreve o handoff e hoje exibe o R6 e a linha de disco. O `PLACAR.md` publicado à parte deriva da GRADE-MESTRA. `app/core/placar_estrutural.py:627–654` `bin/gerar_estado.py:60–104,408–414` `bin/relato.sh:48–49,112,125–130,147–158` `app/core/esteira_vigia.py:815–892` `bin/hook_stop_fila1.py:446–484` `bin/handoff_sessao.sh:85–123,195–218,328–337`
- **Encaixe mínimo — fila de `bin/`.** Ler uma vez a última linha válida emitida pelo vigia da frota e publicar a mesma frase no `ESTADO.md` e no topo visível do RELATO, abaixo das duas linhas operacionais, pelo mecanismo de publicação usado pelo disco. O handoff pode ler essa projeção. O comando continua sendo o único produtor do total. A linha atual do contador sai sem hora própria, então o leitor só deve publicar um horário se puder vinculá-lo à mesma rodada. Essa ligação no log fornecido **[nao verificado]**. `app/docs/LEIS.md:60–61` `app/chamados/management/commands/reconciliar_chamados.py:97–98,115` `bin/cron_run.sh:7–13` `bin/disco_estado.py:25–33,413–453` `bin/gerar_estado.py:320–326` `aval.txt:1`

## 27/11 11:52 · sessao a6f52086aa3d · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

**Os sete casos apontam para outra causa: a apuração do intermitente, sem dobra automática de feriado.** Com o cadastro da sonda, a cópia apura as horas em `resultado.periodos`, como trabalho realizado sem jornada prevista na escala. O caminho dos 75 passa por `periodos_ft` → `ft_sem_escala` e deixa `folga_sem_escala_min` na lavra. Os sete estão sem essa marca. `saida-sonda.tsv:88–89` · [motor_calculo_v2.py:2560](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2560) · [motor_calculo_v2.py:2605](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2605) · [dia_pago.py:259](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:259)

### Os sete casos

Em todos, a sonda mostra município `4103`, a data nos dois conjuntos de feriados impressos, célula gerada com `trabalha=False` e `dna.marcos=None`, base e ciclo `intermitente`, `trabalha_em_feriado=True` e `feriado_vira_folga=False`. A janela de HE estava ativa desde `2026-08-21`, com `decisao_he_vigente=[]`. Os três campos `horas_extras_100`, `horas_extras_100_feriado` e `horas_folga_trabalhada` são `0.0` em cada lavra. As referências à saída contam `sonda_leitura` como linha 1. `Saída da sonda anexada:1–86`

| Empresa / colab | Data | Vínculo / tipo | Motivo na sonda | Trabalhadas (h) | Classificação pela cópia | Fonte |
|---|---|---|---|---:|---|---|
| 2 / 365 | 02/11/2026 | 318 / 470 | A2 | 12.0 | Outra causa, intermitente sem dobra | `Saída da sonda:3–14` · `saida-sonda.tsv:16` |
| 2 / 365 | 15/11/2026 | 318 / 470 | A2 | 11.036849166666666 | Outra causa, intermitente sem dobra | `Saída da sonda:15–26` · `saida-sonda.tsv:17` |
| 2 / 365 | 20/11/2026 | 318 / 470 | A2 | 11.008139166666666 | Outra causa, intermitente sem dobra | `Saída da sonda:27–38` · `saida-sonda.tsv:18` |
| 2 / 830 | 15/11/2026 | 1074 / 466 | A2 | 11.885994444444444 | Outra causa, intermitente sem dobra | `Saída da sonda:39–50` · `saida-sonda.tsv:42` |
| 3 / 85 | 02/11/2026 | 1140 / 483 | A2 | 11.897632222222223 | Outra causa, intermitente sem dobra | `Saída da sonda:51–62` · `saida-sonda.tsv:76` |
| 3 / 85 | 20/11/2026 | 1140 / 483 | A2 | 11.545001388888888 | Outra causa, intermitente sem dobra | `Saída da sonda:63–74` · `saida-sonda.tsv:77` |
| 3 / 146 | 02/11/2026 | 845 / 383 | A8 | 13.50255722222222 | Outra causa, intermitente sem dobra | `Saída da sonda:75–86` · `saida-sonda.tsv:81` |

### (a) Por onde entram as horas

- **Folga trabalhada.** O fechamento passa o vínculo da fatia a `calcular_mes`. Para ciclo `intermitente`, o motor pula `EC.eh_dia_trabalho` na classificação da folga do par. Ainda pode classificar como folga pelo dia da semana cadastrado no template. Na reconstrução destes sete, o caminho compatível com as horas positivas e a marca vazia é `resultado.periodos` → `DiaPago.horas_trabalhadas`. O filtro de escala certa só recebe `periodos_ft`. A2 e A8 fariam reprovar um período que chegasse ao filtro. [fechamento.py:306](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:306) · `motor_calculo_v2.py:2436–2450,2474–2478,2558–2566,2581–2607` · `fechamento.py:953–959,1225–1228` · `dia_pago.py:173–184,234–272` · `escala_certa.py:31–51`
- **Feriado e janela de HE.** O fechamento fornece os feriados do colaborador, e a fábrica escolhe `MotorIntermitente` pela base ou pelo ciclo. O construtor substitui o conjunto recebido por um conjunto vazio que o motor trata como injetado. `eh_feriado` responde `False`, então a dobra direta falha mesmo com `fd=1`, que carimba o flag `feriado_em_dobra`. Para o par apurado fora de folga, o bloqueio integral da janela de HE retorna antes de consultar a decisão. A promoção pelo fechamento exige `periodos_ft`, escala certa e `feriado_vira_folga=True`. [fechamento.py:756](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:756) · `regua_cct.py:421–425,457–479` · [motor_calculo_v2.py:3694](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:3694) · `motor_calculo_v2.py:961,1007–1011,1562–1571,2925–2929,3764–3767` · `fechamento.py:953–980`

### (b) De onde vem o previsto

**Na reconstrução, as sete datas ficam fora de `datas_previstas_trabalho` e têm zero minuto previsto na consulta do motor para a própria data.** O fechamento usa `EC.eh_dia_trabalho`, que devolve `celula.trabalha`. O juiz de minutos também consulta esse método e retorna `0` antes de ler os marcos quando a resposta é `False`. Já a janela de HE pode ler `EC.marcos_do_dia` para ajustar as pontas do par. Com `dna.marcos=None`, essa leitura cai em `TipoEscala.marcos_do_dia`, que fornece horários. Os `720` do DNA não são a jornada prevista usada nessa consulta. [fechamento.py:749](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:749) · `escala/models.py:326–345,1098–1107,1128–1144` · `escala/utils.py:2310–2325` · `motor_calculo_v2.py:1362–1367,1646–1660,2403–2404`

**[nao verificado] na lavra de 25/11:** se o cadastro era idêntico ao da sombra, quais pares formaram cada total, se houve algum período de zero minuto em `periodos_ft`, os horários efetivos do template e eventual recorte de entrada ou saída. A sonda mostra IDs de batidas por dia e totais lavrados. Também ficam **[nao verificado]** o motivo individual dos 75, o pagamento efetivo e a confirmação do hash via Git na cópia. `Saída da sonda:1–86` · `saida-sonda.tsv:88–89` · `motor_calculo_v2.py:2513–2521,2872–2878` · `dia_pago.py:173–184,259–272`

## 27/11 11:50 · sessao 2b67a6c991ff · FERIADO-BOTAO-CORTADO · verificador · origem=Code

## Veredito — REPROVADO

O diff preserva o predicado e dá ao botão um espaço próprio na célula mensal. O bloqueio está no painel da ação: no drawer, o JavaScript mede pela janela e posiciona dentro de um ancestral transformado. O verde informado cobre a estrutura do HTML, mas o clique no feriado de 20/11 dentro do drawer e nas duas cascas segue [não verificado]. [app/templates/colaboradores/partials/_calendario_grade.html:68 e :95–99, no diff] [app/templates/core/_drawer_generico.html:3 e :16] [app/static/js/popover-acao.js:38–45 e :75–76] [nao verificado]

1. **ALTA — O painel pode abrir deslocado e cortado no drawer.** Na ficha aberta como página, o `position:fixed` permite escapar do recorte da célula e da nova região rolável. No drawer, `transform:translateX(0)` faz do próprio drawer a referência do `fixed`, mas o script continua calculando `left` e `top` pela janela. O drawer também tem `overflow-y:auto`. Isso atinge a fita de diagnóstico e o calendário mensal aberto na aba Espelho ou pelo atalho da Folha. A incompatibilidade já existia no posicionador. Sem o JS, o painel usa `position:absolute` dentro da célula recortada. A geometria do clique real segue [não verificado]. [app/templates/core/_drawer_generico.html:3, :16 e :132–142] [app/folha/retrato_folha.py:1019] [app/templates/folha/_retrato_o_que_fazer.html:73] [app/static/js/popover-acao.js:38–76] [app/templates/ponto/partials/_feriado_vira_trabalho.html:7]

2. **MÉDIA — A região é focável e nomeada, mas o fluxo de teclado do popover tem lacunas.** `tabindex="0"`, `role="region"` e o `aria-label` com a data dão à região uma parada de Tab e um nome, inclusive em células sem conteúdo suficiente para rolar. O listener global em captura fecha todos os popovers em qualquer `scroll`, inclusive ao rolar a nova região ou o próprio painel quando o script lhe dá `overflow-y:auto`. `Escape` fecha o painel sem devolver explicitamente o foco ao `summary` e também dispara o fechamento do drawer. Foco visível, rolagem por teclado e anúncio em leitor de tela seguem [não verificado]. [app/templates/colaboradores/partials/_calendario_grade.html:99, no diff] [app/static/js/popover-acao.js:64–72 e :100–110] [app/templates/core/_drawer_generico.html:75–82]

3. **MÉDIA — O teste sela a estrutura e deixa de fora a interação que decide o aceite.** Os controles são localizados pelo destino do POST e pela data. O teste exige um bloco irmão anterior às quatro marcações, verifica o CSS e o `tabindex`, e também confere o rótulo literal. Usa datas e horários fixos e objetos em memória, sem relógio, banco ou CNPJ. Depende da ordem explícita das três células e das quatro marcações. O CNPJ `11222333…` pertence ao smoke de retratação, que usa uma casca HTML própria. O teste novo não mede visibilidade, clique, rolagem, impressão ou posicionamento do painel. RED, verde na pista e mutantes foram informados no pedido. O smoke específico de 20/11 pedido no aval e o clique nas duas cascas seguem [não verificado]. [app/colaboradores/tests/test_feriado_botao_cortado.py:10–15, :32–70 e :72–115, no diff] [app/core/tests/test_smoke_retratar_1366.py:94–96, :114–118 e :138–179] [aval.txt:1] [CLAUDE.md:207]

### Predicado, consumidores e efeitos

- **Predicado e esmeril.** A oferta continua sendo `pode_editar_dia and dia.feriado_vira_trabalho_oferecido`, com a mesma URL e os mesmos dados do formulário. Perfil, drawer e POST continuam usando `editar_escalas`. O POST consulta o campo do mesmo `contexto_calendario`, que decide pelo feriado do colaborador e pela célula existente em folga. O diff repete a condição para aplicar CSS e atributos de acessibilidade. Não introduz `except`, regra de negócio paralela ou mudança no motor/models. [app/templates/colaboradores/partials/_calendario_grade.html:131–133, cópia local, e :68 e :95–99, no diff] [app/colaboradores/views.py:2005–2010] [app/colaboradores/services/drawer.py:156–159] [app/colaboradores/services/calendario.py:785–793] [app/ponto/views.py:3607 e :3622–3633]

- **Consumidores.** `_feriado_vira_trabalho.html` tem um único include direto, na grade. A grade tem três: `calendario_partial.html` usa `mes` em Mês, Semana, Quinzena e Competência, na ficha, na aba Espelho do painel aberto por Colaboradores, Benefícios ou Retrato da Folha, e no atalho “Resolver dia no calendário” da Folha. O drawer de diagnóstico usa `fita` nas linhas e nas pendências do Painel Situacional, acessível pelo Painel Operacional, Central de Chamados e Vínculos. Auditoria de espelhos usa a grade padrão com `only`, sem permissão de edição no include. `base.html` e `base_app.html` incluem o mesmo drawer e carregam o mesmo script. Uma entrada concreta da grade a partir de uma página da casca app segue [não verificado]. [app/templates/colaboradores/partials/_calendario_grade.html:132] [app/templates/colaboradores/calendario_partial.html:50–67] [app/templates/colaboradores/detalhe.html:215] [app/templates/colaboradores/partials/_lista_tabela.html:16] [app/templates/core/_drawer_generico.html:104 e :140] [app/templates/colaboradores/beneficios_lista.html:86] [app/templates/folha/_retrato_lista.html:61] [app/templates/folha/_retrato_raiox.html:68–70] [app/templates/folha/_retrato_o_que_fazer.html:73] [app/templates/colaboradores/partials/_drawer_diagnostico.html:35] [app/templates/colaboradores/partials/_ps_linha.html:4] [app/templates/colaboradores/partials/pendencias_disputa.html:9] [app/templates/core/painel_operacional.html:139] [app/templates/chamados/painel_gestao.html:14] [app/templates/colaboradores/vincular.html:25] [app/templates/relatorios/furos.html:42] [app/templates/base.html:399 e :440] [app/templates/base_app.html:154 e :194]

- **Células e saídas.** Sem oferta, inclusive em mês sem feriados, a nova `div` fica sem flex, rolagem ou parada de Tab, e a ordem relativa do conteúdo é preservada. Com oferta, a fita muda a ordem. No mês, a ação fica fora do scroll e 6+ marcações permanecem no HTML dentro da região rolável. Em tela estreita, o rótulo quebra linha, mas a célula continua com 132 px e a ficha tem uma coluna lateral fixa de 420 px. Visibilidade nos extremos de largura e impressão pelo navegador seguem [não verificado]. A altura fixa e o recorte podem cortar conteúdo na impressão. O PDF de Auditoria segue pelo gerador ReportLab, que recebe os dados diretamente. [app/templates/colaboradores/partials/_calendario_grade.html:65–68 e :95–99, no diff] [app/templates/ponto/partials/_feriado_vira_trabalho.html:5–6, no diff] [app/templates/colaboradores/detalhe.html:87] [app/relatorios/views.py:1628–1634] [app/relatorios/furos_pdf.py:73–103]

- **LEIS-UI.** L2 e L4 são preservadas pelo predicado compartilhado e pela renderização condicional. O diff não introduz diálogo nativo, botão PDF ou mudanças nos ícones e tamanhos de estado, conforme L8–L11. O novo `aria-label` nomeia a região. Pela L7, a ajuda explicativa literal do formulário continua sendo uma pendência anterior ao diff. Pela L5, o formulário não tem o componente de espera e a duração da ação em produção segue [não verificado]. [app/docs/LEIS-UI.md:23, :39, :56, :77, :87, :95, :120 e :174] [app/templates/ponto/partials/_feriado_vira_trabalho.html:7–12] [app/templates/colaboradores/partials/_calendario_grade.html:95–103, no diff]

## 27/11 11:48 · sessao 30285ce09825 · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

## FERIADO-NAO-PREVISTO-L086 — sonda na sombra

Apenas dias-colab com `horas_trabalhadas > 0`. As referências a `saida-sonda.tsv` seguem a numeração do anexo, com `sonda_leitura` na linha 1. `saida-sonda.tsv:1–4,87–90`

| Empresa | Dias no recorte | (a) `folga_sem_escala_min > 0` | (a) Soma dos minutos em horas | (b) Dias com rubrica alternativa positiva | (b) Soma das duas rubricas | (c) Dias no ramo não explicado | Fonte |
|---:|---:|---:|---:|---:|---:|---:|---|
| 2 | 69 | 65 | 459.7395 h (27584.37 min) | 0 | 0.0 h | 4 | `saida-sonda.tsv:5–73,88` |
| 3 | 13 | 10 | 74.201 h (4452.06 min) | 0 | 0.0 h | 3 | `saida-sonda.tsv:74–86,89` |
| 4 | 0 | 0 | 0 h (0 min) | 0 | 0.0 h | 0 | `saida-sonda.tsv:87–90` |

Na cópia, os períodos barrados pelo filtro de escala certa entram em `horas_trabalhadas`, com os minutos registrados em `detalhe.folga_sem_escala_min`. O item (b) soma `horas_folga_trabalhada` e `horas_extras_100_feriado` registradas na sonda. Pagamento efetivo [nao verificado]. [fechamento.py:959](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:959) · [fechamento.py:1004](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1004) · [dia_pago.py:259](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:259) · `saida-sonda.tsv:4–86`

### (c) Ramo não explicado

| Empresa | Colab ID | Data | Horas trabalhadas | Fonte |
|---:|---:|---|---:|---|
| 2 | 365 | 2026-11-02 | 12.0 | `saida-sonda.tsv:16` |
| 2 | 365 | 2026-11-15 | 11.036849166666666 | `saida-sonda.tsv:17` |
| 2 | 365 | 2026-11-20 | 11.008139166666666 | `saida-sonda.tsv:18` |
| 2 | 830 | 2026-11-15 | 11.885994444444444 | `saida-sonda.tsv:42` |
| 3 | 85 | 2026-11-02 | 11.897632222222223 | `saida-sonda.tsv:76` |
| 3 | 85 | 2026-11-20 | 11.545001388888888 | `saida-sonda.tsv:77` |
| 3 | 146 | 2026-11-02 | 13.50255722222222 | `saida-sonda.tsv:81` |

### (d) Colab 1036 em 20/11

**Confere.** Na empresa 2, em 2026-11-20, constam **437.1 min** de `folga_sem_escala_min` (**7.285 h** pela conversão) e **7.28502 h** trabalhadas. As duas rubricas de (b) estão em `0.0 h` e `folga_feriado_min` está como `None`. O dia entra em (a). `saida-sonda.tsv:72`

## 27/11 11:48 · sessao 9781db85a25e · RC-SEM-PROXIMA · verificador · origem=Code

## REPROVADO — RC-SEM-PROXIMA, ver4, c4b `bd4f156cb`

P1 cura o `.rc` vencido ilegível, P2 entrega o diagnóstico no patch do leitor e P4 fixa os instantes dos dois testes. P3 corrige os três casos de Nova York do pedido, mas ainda pode listar após **10 s reais** ou ocultar uma parada de **20 min reais**. `bin/rc_sem_colher.py:94–102,126–162`, `bin/tests/test_rc_sem_colher.sh:179–216,282–321`, `vercodex.py:165–175` no anexo.

### Achados numerados

1. **ALTA — P3 ainda perde ocorrências reais.** `_instante_da_marca` tenta os dias `0,1,2` com `isdst=0,1`, confere o `HH:MM` retornado por `localtime` e escolhe o maior instante no intervalo inclusivo `[piso, agora]`. A regra de prazo funciona quando a ocorrência real entra no conjunto. Confirmei dois contraexemplos no `main`, com relógio e entrada injetados em memória. `bin/rc_sem_colher.py:126–140,157–162,184–198`.

   - **Listagem precoce, `America/Caracas`, 09/12/2007.** As duas `02:40` têm `isdst=0`. Com `.rc` às `02:20 −0400`, marca real às `02:40:10 −0430` e `agora` às `02:40:20 −0430`, o juiz escolheu `02:40 −0400`, imprimiu `RC SEM PROXIMA` e devolveu `3` após **10 s reais**. Com `.rc` às `02:35 −0430` e `agora` às `03:00:11 −0430`, a candidata antiga caiu abaixo do piso e a mesma marca sumiu das duas listas. `bin/rc_sem_colher.py:113–122,133–140,153–162,191–198`.
   - **Parada oculta no fim do DST, `America/Moncton`.** `.rc` em `29/10/2006 00:00:30 −0300`. O relógio voltou para o dia 28, a marca foi escrita às `23:10:10 −0400` e a consulta ocorreu às `23:30:11 −0400`. A marca sucedeu ao `.rc` em tempo real, mas sua data civil é `k=-1`. O juiz devolveu `0`, stdout vazio, após **20 min e 1 s**. Aqui as duas leituras têm `isdst` diferentes. `bin/rc_sem_colher.py:113–122,133–140,153–162,191–198`.

2. **MÉDIA — P1 cura o filtro de idade, com erro residual em candidato que perde a seleção.** `caso_vencido_ilegivel` exige a linha exata do órfão e rc `3`. Na sonda em memória, a c3 reconstruída e `abre-vencido` levantaram `PermissionError`. `caso_troca_stat_open` exige `10:10 rc=7`. `mtime-do-stat` produziu `10:00 rc=7`. A troca é uma guarda do novo `stat`. A c3 reconstruída respondeu `10:00 rc=0`, pois o gancho que efetua a troca dependia desse `stat`. As guardas de remoção e troca **depois** da visita continuam com `le-tarde-removido` e `le-tarde-trocado`. `bin/rc_sem_colher.py:94–107,115–122`, `bin/tests/test_rc_sem_colher.sh:173–178,225–240,266–272,295–301,438–447`.

   O pedido manda propagar o erro do **escolhido**, inclusive se estiver colhido. Isso cobre o exemplo do colhido ilegível do ver3 quando ele é o `.rc` escolhido. O código também abre um `.rc` dentro das 24 h que já perde para um mais novo. Confirmei `PermissionError` nas duas ordens da visita. O recibo descreve corretamente esse comportamento. O RED pedido e entregue só cobra o corte por idade. `pedido.md:6`, `recibo.md:40–43`, `bin/rc_sem_colher.py:94–102,184–190,201–202`, `bin/tests/test_rc_sem_colher.sh:295–297`.

3. **MÉDIA — O selo de P3 depende do comportamento da libc.** Em Nova York, confirmei os pares **10 s → vazio** e **prazo vencido → lista** para os dois dias civis e para a hora repetida. Na libc desta cópia, caíram `marca-dois-dias`, os três `marca-palpite-repetida*` e `marca-so-edt`. `marca-sem-conferir-hora` também caiu, em um dia comum de EST, ao aceitar um instante que se lia como `09:30` para a marca `10:30`. O selo dá `TZ` ao processo e prepara uma chamada anterior a `mktime` para enviesar `isdst=-1`. Cada mutante tem de cair individualmente. Uma libc que escolha sempre a primeira ocorrência faz o mutante sobreviver no caso `_1a`. Uma que escolha sempre a segunda faz o mutante sobreviver nos casos de segunda ocorrência. Outro host/libc e outra versão de tzdata ficam [nao verificado]. `bin/tests/test_rc_sem_colher.sh:131–134,189–216,303–329,389–403,448–454`, `recibo.md:23–26,49–50`.

4. **BAIXA — P4 curado na preparação dos casos, sem mutante da cura.** `caso_resumo` e `caso_resumo_vazio` usam `_cenario`, `agoras` fixos e o `main` real. Os quatro mutantes de resumo mordem a ordem das contas, a soma indevida, a perda de `min_idade` e o silêncio no zero. Eles não testam a volta do relógio de parede ao fixture. A c3 reconstruída passou com o fixture novo, portanto isso não demonstra um RED de P4. O teste sem `--resumo` só exige ausência da palavra e aceita saída vazia ou erro. `bin/tests/test_rc_sem_colher.sh:179–188,213–216,282–293,442–455`, `pedido.md:9,11`.

### P2, escritores e leitores

- **P2 curado no anexo, sem selo no repo conforme o pedido.** `_a` fornece exatamente os três argumentos impressos e enviados, `--min-idade 900 --resumo`. O cabeçalho mostra `bin/rc_sem_colher.py` como caminho relativo. A chamada envia também `python3` e o caminho montado com `R`. No bloco isolado, confirmei que rc `0` e `3` imprimem o último `RESUMO` recebido ou `SEM RESUMO do juiz`, com até 15 detalhes de 120 caracteres. Outro rc acrescenta a última linha de stderr ou `(stderr vazio)`. O bloco informa o rc, sem propagá-lo. `pedido.md:7`, `vercodex.py:165–177` no anexo.
- **Tupla e mtime.** `codex_pergunta.sh::_saida` escreve `rc=` no temporário e publica por `mv`. `_obras_vivas` monta `(ob, d, rc_m, rc_p, rc_txt, outro)`. O `getmtime` anterior ao `open` só decide se abre. O `fstat` do descritor fornece o mtime que passa pelo segundo corte e disputa a seleção, e `read(200)` lê o texto desse mesmo descritor. `main` materializa a tupla uma vez. Os leitores diretos são `sem_colher`, que usa os seis campos, e `sem_proxima`, que descarta o texto. Ambos usam o mtime para a folga, o `HH:MM` e a ordenação. `sem_colher` também o usa para a idade e extrai o `rc=` do texto. `sem_proxima` usa o mtime como base da inferência da marca. `bin/codex_pergunta.sh:565–574`, `bin/rc_sem_colher.py:67–70,77–123,133–164,184–198`.
- **Instante da marca.** A entrada do `RELATO` fornece o `HH:MM` da linha, descrito como vindo do `date` da sessão. O processo concreto que escreveu uma marca de produção fica [nao verificado]. `marcas_datadas` lê a linha. `sem_colher` usa a chave para excluir o `.rc`, mesmo quando falta um instante válido. `_instante_da_marca` produz o instante inferido e `sem_proxima` é seu leitor em produção. O `main` imprime o `HH:MM`, sem expor o instante inferido. `bin/rc_sem_colher.py:19–23,44–64,113–140,146–163,191–194`.
- **`ESPERA_UNICA`, sem `--resumo`.** Lê stdout e o rc do juiz, sem acessar a tupla. Com `0` entra no laço. Com `3` pula o laço. Em ambos os casos executa o `find` final e termina com sucesso. Outro rc resulta em falha da espera. A igualdade byte a byte é **falsa**. No caso `dst-dois-dias`, a c3 reconstruída imprimiu `RC SEM PROXIMA OBRA-P r.rc 23:50 colhido=00:10\n` e pulou o laço. A c4b teve stdout vazio e entrou no laço. Confirmei com o Bash da constante e dependências simuladas. `bin/disjuntor.py:1963–1969,2157–2174`, `bin/tests/test_rc_sem_colher.sh:189–193,303–309`, `bin/rc_sem_colher.py:191–198`.

### Bordas, escopo e esmeril

- **Hora pulada e virada comum.** Em Nova York, `02:10` na hora pulada dá `None`, e uma marca real às `03:10` lista depois do prazo. Um `.rc` de ontem às `23:50` aceita marca de hoje às `00:10`. O caso de março com dois dias civis ainda cabe em `84.020 s` reais. Acima de `86.400 s` reais, o `.rc` sai no filtro de idade. `bin/rc_sem_colher.py:40,81–101,133–140,157–162`, `bin/tests/test_rc_sem_colher.sh:104–110,189–193,303–309`.
- **O terceiro marcador do ver3 ficou fora de P3.** O `.rc` pode vencer antes de a marca amadurecer. Uma marca antiga pode ficar oculta quando o mesmo `HH:MM` volta a caber no teto, inclusive na segunda ocorrência da hora repetida. Isso segue a regra pedida da candidata mais recente. Um `rc_m` futuro pode ganhar a seleção e suprimir o `.rc` anterior. Com piso futuro a inferência dá `None`. No mesmo minuto, o prazo ainda não amadureceu. Esses três comportamentos são distintos dos contraexemplos do achado 1. `pedido.md:8`, `recibo.md:44–47`, `bin/rc_sem_colher.py:94–102,115–122,133–140,148–162`.
- **Esmeril.** `^\+.*except` dá **0** em cada anexo de diff. O `except Exception` do leitor é contexto. O juiz lê o relógio uma vez, no `main`, e não há derivação paralela das contas no leitor ou na espera. Dentro do juiz, as duas classificações ainda percorrem o parser comum e repetem a folga. Os casos antigos seguem lendo `date`. P4 nomeou os dois casos de resumo, ambos corrigidos. `recibo.md:37–38,51`, `vercodex.py:165–177` no anexo, `bin/rc_sem_colher.py:51–64,113–119,146–152,184–198`, `bin/tests/test_rc_sem_colher.sh:48–51,74–77,282–293`, `pedido.md:9,12`.

Conferi o código da cópia, os patches fornecidos, as sondas em memória e a sintaxe Bash. RED/GREEN integrais, execução de `test_disjuntor.sh`, Ruff, confirmação do hash por Git, instalação do vercodex e ocorrência dos cenários em produção ficam [nao verificado].

## 27/11 11:45 · sessao 9b2b58bedbd6 · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

**(a) O zero veio de `periodos_ft` → `ft_sem_escala`. O motivo que a sonda encontrou foi `LIMBO`.** O fechamento testa `escala_certa_no_dia` antes de consultar `feriado_vira_folga`. A linha lavrada tem `folga_sem_escala_min=437,10`, que corresponde às `7,28502 h` trabalhadas na precisão do campo. Esse ramo grava as trabalhadas e preserva o noturno, sem acrescentar HE 100% ou folga trabalhada. A sonda leu `LIMBO` no cadastro da sombra. A lavra prova a reprovação no filtro, mas o motivo histórico exato é [nao verificado]. [fechamento.py:953](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:953) · [fechamento.py:1004](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1004) · [escala_certa.py:31](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/services/escala_certa.py:31) · [dia_pago.py:259](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:259)

- `fd=1` carimba a dobra habilitada. A sonda obteve `feriado_vira_folga=True` para o vínculo 1552, `6x1`, `trabalha_em_feriado=False`. Os períodos barrados pelo filtro ficam fora da promoção. A janela de HE estava ativa e a consulta de decisões vigentes voltou vazia. Para um par em folga sem escala certa, a janela retorna antes do bloqueio integral, que zeraria também trabalhadas e noturnas. [regua_cct.py:421](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/regua_cct.py:421) · [models.py:1400](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/models.py:1400) · [fechamento.py:958](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:958) · [motor_calculo_v2.py:1562](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:1562)
- Com a célula 143963 em `trabalha=False`, 20/11 fica fora de `datas_previstas_trabalho` na reconstrução. O conjunto é fornecido explicitamente, então `_previstas_fornecidas=True`. A dobra direta consulta o ciclo e o feriado no dia de início do turno. [models.py:1098](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/models.py:1098) · [fechamento.py:749](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:749) · [motor_calculo_v2.py:826](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:826) · [motor_calculo_v2.py:941](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:941)

**(b) Sim, coincidem em 20/11 na reconstrução.** Os dois conjuntos impressos no recorte de 19 a 21/11 contêm apenas `{20/11/2026}`. O fechamento injeta `feriados_do_colab` e o calendário chama a mesma função. Para esse dia, o alcance usa o município 4103 do posto 460 do vínculo 1552. A referência à praça na docstring do motor ficou desatualizada. O método executável lê o conjunto injetado. [fechamento.py:668](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:668) · [fechamento.py:756](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:756) · [calendario.py:347](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/services/calendario.py:347) · [feriados.py:124](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:124) · [feriados.py:160](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:160) · [motor_calculo_v2.py:1000](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:1000)

**(c) Há um juiz para o alcance do feriado.** O registro aponta `core.feriados.eh_feriado`, que delega a `feriados_de`. `feriados_do_colab` resolve o município de cada dia e usa esse mesmo filtro. Nenhum dos dois sai. `escala_certa_no_dia` julga a confiabilidade da escala. O ponto que produz a divergência de pagamento é aplicar essa guarda à promoção do feriado, caso já registrado em `FERIADO-SO-NA-ESCALA-CERTA`. [juizes.py:196](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/juizes.py:196) · [feriados.py:101](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:101) · [feriados.py:135](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:135) · [escala_certa.py:49](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/services/escala_certa.py:49) · [BACKLOG.md:2164](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/BACKLOG.md:2164)

**(d) Sim, pela regra escrita de enquadramento, o trabalho de 20/11 cabe em HE 100% de feriado.** O BO-B (3) manda usar essa rubrica para feriado trabalhado em vínculo que folga no feriado. O cadastro fornecido satisfaz o predicado com `trabalha_em_feriado=False`, ciclo `6x1` e feriado no município do posto. O corte de 27/09 às 03h revogou a cláusula de feriado da L-086 e manteve a dobra pelo ciclo e pelo dia de início. `CICLOS_FERIADO_SIMPLES` contém só `12x36`. A régua CCT pode ligar a dobra desse ciclo pela cláusula cadastrada `feriado_12x36_em_dobra`. Para o `6x1`, vale a dobra pelo ciclo. O corte das 05h sobre escala certa trata do adicional de folga trabalhada. O BO-B (3) reserva essa rubrica aos dias de folga. [PROMPTS.md:1370](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/PROMPTS.md:1370) · [models.py:1384](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/models.py:1384) · [LEIS.md:118](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:118) · [motor_calculo_v2.py:944](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:944) · [motor_calculo_v2.py:3740](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:3740) · [regua_cct.py:473](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/regua_cct.py:473) · [fechamento.py:991](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:991)

**(e) Ficam [nao verificado]** o conjunto integral efetivamente injetado na lavra de 25/11, a identidade entre o cadastro da sombra e o usado nessa lavra, a classe do motor, o pareamento e os valores intermediários por período, a fonte exata da régua CCT e o que saiu no mensal, no TXT ou no pagamento. O mapa impresso associa IDs de batidas a dias. A trilha por período e o carimbo da régua são produzidos em pontos próprios do cálculo. O impacto nas empresas 2, 3 e 4 também fica [nao verificado]. [turnos.py:2971](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/turnos.py:2971) · [motor_calculo_v2.py:2571](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2571) · [fechamento.py:298](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:298)

## 27/11 11:44 · sessao ed0d5daab456 · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

# FERIADO-NAO-PREVISTO-L086 — medida na sombra

## Totais por empresa

O universo da competência **11/2026**, de **21/10 a 20/11**, reúne os dias em que `feriados_do_colab` inclui a data e a célula tem `trabalha=False`. Os totais de horas cobrem os registros diários `DiaPago` da versão `motor` com `horas_trabalhadas > 0`. `r.md:28–36` `r.md:41–89` `app/core/feriados.py:135–167`

Nas referências, `saida-sonda.tsv` é a saída raspada da mensagem, numerada a partir de `sonda_leitura` na linha 1. As colunas de noturnas são somas dos valores decimais impressos nos detalhes. O resumo da sonda imprime os totais de trabalhadas e de `extras_100`. `saida-sonda.tsv:1–91` `r.md:126–143`

| Empresa | Dias-colab no universo | Com trabalhadas > 0 | Com lavra e trabalhadas ≤ 0 | Sem lavra | Horas trabalhadas | Horas noturnas | Horas `extras_100` | Horas `extras_100_noturna` | Fonte |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| 2 | 753 | 69 | 72 | 612 | 505.6704230627778 | 21.15548214285714177 | 0.0 | 0.0 | `saida-sonda.tsv:5–73,88` |
| 3 | 182 | 13 | 12 | 157 | 111.14582922305556 | 29.699891428571429 | 0.0 | 0.0 | `saida-sonda.tsv:74–86,89` |
| 4 | 39 | 0 | 9 | 30 | 0.0 | 0.0 | 0.0 | 0.0 | `saida-sonda.tsv:87–90` |

O universo total impresso é **974 dias-colab**. As **82 linhas** detalhadas têm `extras_100 = 0.0` e `extras_100_noturna = 0.0`. `saida-sonda.tsv:5–86,91`

## Colab 1036 em 20/11

**Sim, aparece na empresa 2.** A linha traz **7.28502 h trabalhadas**, **1.163055238095238 h noturnas**, **0.0 h de `extras_100`** e **0.0 h de `extras_100_noturna`**. Os valores conferem com **7,29 / 1,16 / 0,0** do aval, com as duas primeiras medidas arredondadas a duas casas. `saida-sonda.tsv:72` `aval.txt:1`

## Horário e limites da medida

- **`medido_em = 2026-11-27T11:40:09-03:00`**. A sonda registra esse horário antes de abrir a transação de leitura. A rotina da cópia aponta para o backup diário das **04:00**, então **27/11 às 04:00** é uma hipótese se a rotina seguiu o fluxo diário. O código também prevê `--dump-agora` e atualização incremental. A data e a hora efetivas da base lida estão **[nao verificado]**. `saida-sonda.tsv:2` `r.md:113–119` `app/config/crons.py:182` `bin/sombra.sh:194–215,236–253,299–305`
- **HE100 no restante do universo [nao verificado].** A saída só conta os dias com lavra e `horas_trabalhadas <= 0` e os dias sem lavra. O código pode lançar horas diretamente em HE100 de feriado sem somá-las a `horas_trabalhadas`, deixando o dia fora dos detalhes. O campo específico `horas_extras_100_feriado` também ficou fora da consulta. `saida-sonda.tsv:87–90` `r.md:66–98` `app/ponto/models.py:326–332` `app/ponto/services/dia_pago.py:241–253`
- **Causa do zero de 1036 [nao verificado].** A saída traz a lavra diária. O conjunto de feriados recebido na apuração original, `datas_previstas_trabalho`, as batidas, a régua aplicada e o resultado do crivo de escala certa para esse dia ficam fora da leitura. `r.md:55–76` `app/ponto/services/fechamento.py:953–974` `aval.txt:1`
- **Valores atuais em produção, resultado de recálculo, TXT e pagamento efetivo [nao verificado].** A sonda consulta o `DiaPago` gravado na sombra. A geração do TXT na cópia usa os objetos do fechamento mensal. `r.md:66–76,103–124` `app/folha/views.py:214` `app/folha/export.py:420`

## Lista completa com `horas_trabalhadas > 0`

A lista está ordenada por empresa, data e ID do colaborador. O negrito marca `extras_100 = 0.0`. A empresa 4 tem zero linhas nesse recorte. `saida-sonda.tsv:5–90`

| Empresa | Data | Colab ID | Trabalhadas (h) | Noturnas (h) | `extras_100` (h) | Fonte |
|---:|---|---:|---:|---:|---:|---|
| 2 | 2026-11-02 | 43 | 4.088383333333334 | 0.0 | **0.0** | `saida-sonda.tsv:5` |
| 2 | 2026-11-02 | 203 | 7.069715555555556 | 0.0 | **0.0** | `saida-sonda.tsv:6` |
| 2 | 2026-11-02 | 207 | 5.991126388888889 | 0.0 | **0.0** | `saida-sonda.tsv:10` |
| 2 | 2026-11-02 | 278 | 7.0710894444444445 | 0.0 | **0.0** | `saida-sonda.tsv:13` |
| 2 | 2026-11-02 | 365 | 12.0 | 0.0 | **0.0** | `saida-sonda.tsv:16` |
| 2 | 2026-11-02 | 366 | 10.998848333333335 | 5.997973333333333 | **0.0** | `saida-sonda.tsv:19` |
| 2 | 2026-11-02 | 369 | 7.0080025 | 0.0 | **0.0** | `saida-sonda.tsv:20` |
| 2 | 2026-11-02 | 384 | 7.31101361111111 | 0.0 | **0.0** | `saida-sonda.tsv:22` |
| 2 | 2026-11-02 | 583 | 8.078643055555556 | 0.0 | **0.0** | `saida-sonda.tsv:26` |
| 2 | 2026-11-02 | 622 | 6.974738382777777 | 0.0 | **0.0** | `saida-sonda.tsv:27` |
| 2 | 2026-11-02 | 784 | 4.133950277777777 | 0.0 | **0.0** | `saida-sonda.tsv:32` |
| 2 | 2026-11-02 | 788 | 16.0160475 | 1.1082016666666665 | **0.0** | `saida-sonda.tsv:34` |
| 2 | 2026-11-02 | 789 | 7.3611770241666665 | 0.0 | **0.0** | `saida-sonda.tsv:35` |
| 2 | 2026-11-02 | 822 | 7.177437222222222 | 0.9663374603174603 | **0.0** | `saida-sonda.tsv:38` |
| 2 | 2026-11-02 | 830 | 12.066969811944444 | 0.0 | **0.0** | `saida-sonda.tsv:41` |
| 2 | 2026-11-02 | 961 | 7.146785555555555 | 0.0 | **0.0** | `saida-sonda.tsv:51` |
| 2 | 2026-11-02 | 962 | 7.5270697222222225 | 0.0 | **0.0** | `saida-sonda.tsv:53` |
| 2 | 2026-11-02 | 967 | 7.394691666666666 | 0.0 | **0.0** | `saida-sonda.tsv:55` |
| 2 | 2026-11-02 | 968 | 6.9958141666666664 | 0.9541666666666667 | **0.0** | `saida-sonda.tsv:56` |
| 2 | 2026-11-02 | 975 | 3.9582752777777777 | 0.0 | **0.0** | `saida-sonda.tsv:58` |
| 2 | 2026-11-02 | 988 | 7.342193096111111 | 0.0 | **0.0** | `saida-sonda.tsv:59` |
| 2 | 2026-11-02 | 990 | 7.317387678888889 | 0.0 | **0.0** | `saida-sonda.tsv:61` |
| 2 | 2026-11-02 | 991 | 7.042927222222222 | 0.0 | **0.0** | `saida-sonda.tsv:62` |
| 2 | 2026-11-15 | 203 | 7.1011894444444446 | 0.0 | **0.0** | `saida-sonda.tsv:7` |
| 2 | 2026-11-15 | 207 | 6.187036434444445 | 0.0 | **0.0** | `saida-sonda.tsv:11` |
| 2 | 2026-11-15 | 278 | 7.403098888888889 | 0.0 | **0.0** | `saida-sonda.tsv:14` |
| 2 | 2026-11-15 | 365 | 11.036849166666666 | 0.0 | **0.0** | `saida-sonda.tsv:17` |
| 2 | 2026-11-15 | 369 | 6.9213000000000005 | 0.0 | **0.0** | `saida-sonda.tsv:21` |
| 2 | 2026-11-15 | 384 | 6.144064166666666 | 0.0 | **0.0** | `saida-sonda.tsv:23` |
| 2 | 2026-11-15 | 622 | 4.0098577777777775 | 0.0 | **0.0** | `saida-sonda.tsv:28` |
| 2 | 2026-11-15 | 735 | 3.9637504116666666 | 0.0 | **0.0** | `saida-sonda.tsv:30` |
| 2 | 2026-11-15 | 789 | 7.428601666666666 | 0.0 | **0.0** | `saida-sonda.tsv:36` |
| 2 | 2026-11-15 | 822 | 8.112541666666667 | 1.065664761904762 | **0.0** | `saida-sonda.tsv:39` |
| 2 | 2026-11-15 | 830 | 11.885994444444444 | 0.0 | **0.0** | `saida-sonda.tsv:42` |
| 2 | 2026-11-15 | 899 | 8.89184510277778 | 0.0 | **0.0** | `saida-sonda.tsv:44` |
| 2 | 2026-11-15 | 903 | 4.243828611111111 | 0.0 | **0.0** | `saida-sonda.tsv:46` |
| 2 | 2026-11-15 | 944 | 6.867059856944445 | 0.0 | **0.0** | `saida-sonda.tsv:49` |
| 2 | 2026-11-15 | 968 | 7.0078652777777775 | 0.9576526984126983 | **0.0** | `saida-sonda.tsv:57` |
| 2 | 2026-11-15 | 991 | 7.026201944444444 | 0.0 | **0.0** | `saida-sonda.tsv:63` |
| 2 | 2026-11-15 | 1000 | 3.9718216666666666 | 0.0 | **0.0** | `saida-sonda.tsv:65` |
| 2 | 2026-11-15 | 1023 | 8.376993055555555 | 0.0 | **0.0** | `saida-sonda.tsv:67` |
| 2 | 2026-11-15 | 1026 | 7.02381 | 0.956779365079365 | **0.0** | `saida-sonda.tsv:68` |
| 2 | 2026-11-20 | 203 | 7.032373333333334 | 0.0 | **0.0** | `saida-sonda.tsv:8` |
| 2 | 2026-11-20 | 206 | 4.199098888888889 | 0.0 | **0.0** | `saida-sonda.tsv:9` |
| 2 | 2026-11-20 | 207 | 6.012959166666666 | 0.0 | **0.0** | `saida-sonda.tsv:12` |
| 2 | 2026-11-20 | 278 | 7.165294444444444 | 0.0 | **0.0** | `saida-sonda.tsv:15` |
| 2 | 2026-11-20 | 365 | 11.008139166666666 | 0.0 | **0.0** | `saida-sonda.tsv:18` |
| 2 | 2026-11-20 | 384 | 7.568888055555556 | 0.0 | **0.0** | `saida-sonda.tsv:24` |
| 2 | 2026-11-20 | 415 | 6.995728888888889 | 0.0 | **0.0** | `saida-sonda.tsv:25` |
| 2 | 2026-11-20 | 727 | 6.987754444444445 | 0.0 | **0.0** | `saida-sonda.tsv:29` |
| 2 | 2026-11-20 | 735 | 4.312736620277778 | 0.0 | **0.0** | `saida-sonda.tsv:31` |
| 2 | 2026-11-20 | 784 | 3.956977222222222 | 0.0 | **0.0** | `saida-sonda.tsv:33` |
| 2 | 2026-11-20 | 789 | 3.544456188888889 | 0.0 | **0.0** | `saida-sonda.tsv:37` |
| 2 | 2026-11-20 | 822 | 7.05717611111111 | 0.9719514285714285 | **0.0** | `saida-sonda.tsv:40` |
| 2 | 2026-11-20 | 830 | 12.009573333333332 | 0.0 | **0.0** | `saida-sonda.tsv:43` |
| 2 | 2026-11-20 | 899 | 7.019682479444445 | 0.0 | **0.0** | `saida-sonda.tsv:45` |
| 2 | 2026-11-20 | 903 | 8.047641666666667 | 0.0 | **0.0** | `saida-sonda.tsv:47` |
| 2 | 2026-11-20 | 924 | 3.115772222222222 | 0.0 | **0.0** | `saida-sonda.tsv:48` |
| 2 | 2026-11-20 | 944 | 6.701955833333332 | 0.0 | **0.0** | `saida-sonda.tsv:50` |
| 2 | 2026-11-20 | 961 | 7.668902500000001 | 0.0 | **0.0** | `saida-sonda.tsv:52` |
| 2 | 2026-11-20 | 962 | 7.2847275 | 0.0 | **0.0** | `saida-sonda.tsv:54` |
| 2 | 2026-11-20 | 988 | 10.377472973611113 | 0.0 | **0.0** | `saida-sonda.tsv:60` |
| 2 | 2026-11-20 | 991 | 7.0027872222222225 | 0.0 | **0.0** | `saida-sonda.tsv:64` |
| 2 | 2026-11-20 | 1000 | 3.9380341666666667 | 0.0 | **0.0** | `saida-sonda.tsv:66` |
| 2 | 2026-11-20 | 1026 | 7.228945 | 0.9553495238095238 | **0.0** | `saida-sonda.tsv:69` |
| 2 | 2026-11-20 | 1031 | 10.85031111111111 | 6.014245833333333 | **0.0** | `saida-sonda.tsv:70` |
| 2 | 2026-11-20 | 1035 | 11.299347000833333 | 0.04410416666666667 | **0.0** | `saida-sonda.tsv:71` |
| 2 | 2026-11-20 | 1036 | 7.28502 | 1.163055238095238 | **0.0** | `saida-sonda.tsv:72` |
| 2 | 2026-11-20 | 1050 | 8.322671111111113 | 0.0 | **0.0** | `saida-sonda.tsv:73` |
| 3 | 2026-11-02 | 85 | 11.897632222222223 | 10.16872253968254 | **0.0** | `saida-sonda.tsv:76` |
| 3 | 2026-11-02 | 114 | 7.004955277777778 | 0.0 | **0.0** | `saida-sonda.tsv:78` |
| 3 | 2026-11-02 | 146 | 13.50255722222222 | 9.765453015873016 | **0.0** | `saida-sonda.tsv:81` |
| 3 | 2026-11-02 | 749 | 7.395433621944446 | 0.0 | **0.0** | `saida-sonda.tsv:82` |
| 3 | 2026-11-15 | 82 | 7.559408888888888 | 0.0 | **0.0** | `saida-sonda.tsv:74` |
| 3 | 2026-11-15 | 114 | 8.323086386944444 | 0.0 | **0.0** | `saida-sonda.tsv:79` |
| 3 | 2026-11-15 | 911 | 6.466329444444445 | 0.0 | **0.0** | `saida-sonda.tsv:83` |
| 3 | 2026-11-15 | 1024 | 7.3869547580555555 | 0.0 | **0.0** | `saida-sonda.tsv:85` |
| 3 | 2026-11-20 | 82 | 7.109137536666666 | 0.0 | **0.0** | `saida-sonda.tsv:75` |
| 3 | 2026-11-20 | 85 | 11.545001388888888 | 9.765715873015873 | **0.0** | `saida-sonda.tsv:77` |
| 3 | 2026-11-20 | 114 | 7.974111795277778 | 0.0 | **0.0** | `saida-sonda.tsv:80` |
| 3 | 2026-11-20 | 911 | 7.600584166666666 | 0.0 | **0.0** | `saida-sonda.tsv:84` |
| 3 | 2026-11-20 | 1024 | 7.3806365130555545 | 0.0 | **0.0** | `saida-sonda.tsv:86` |

## 27/11 11:41 · sessao 004a35688bb5 · FERIADO-NAO-PREVISTO-L086 · leitura · origem=Code

## (a) De onde vêm os feriados da competência

**Na cópia, o fechamento e o calendário chamam `feriados_do_colab`.** A chave territorial do conjunto é o **município do posto do vínculo de cada dia**. A referência à “praça do colab” em `MotorBase.eh_feriado` está na docstring. O corpo do método consulta o conjunto recebido. `app/ponto/services/fechamento.py:668`, `app/colaboradores/services/calendario.py:347`, `app/core/feriados.py:131`, `app/core/feriados.py:160`, `app/ponto/motor_calculo_v2.py:1003`, `app/ponto/motor_calculo_v2.py:1007`.

1. `recalcular_fechamento_mes` resolve a janela da competência pelo corte da **empresa**. Para os feriados, carrega um envelope que cobre os cortes 2 a 28, com um dia a mais em cada ponta. Para cada colaborador, chama `feriados_do_colab` com as linhas de feriado e as células carregadas em lote. `app/ponto/services/fechamento.py:468`, `app/ponto/services/fechamento.py:604`, `app/ponto/services/fechamento.py:615`, `app/ponto/services/fechamento.py:625`, `app/ponto/services/fechamento.py:641`, `app/ponto/services/fechamento.py:668`.
2. `feriados_do_colab` resolve `vinculo_do_dia` → `lugar_do_vinculo` → `vinculo.posto.municipio` → `feriados_de`, com cache por `Municipio.pk`. `Posto.praca` e `Posto.municipio` são FKs distintas. A praça entra na resolução da **CCT**, pelo vínculo na data de referência da régua, passada pelo fechamento como início da fatia. `app/core/feriados.py:124`, `app/core/feriados.py:160`, `app/core/feriados.py:164`, `app/colaboradores/models.py:230`, `app/colaboradores/models.py:231`, `app/ponto/services/fechamento.py:756`, `app/core/regua_cct.py:329`, `app/core/regua_cct.py:330`, `app/core/regua_cct.py:458`.
3. `apurar_por_fatia` chama `_montar_motor` e depois `motor.calcular_mes`. `_montar_motor` passa `feriados=` a `get_motor_cct`, que o repassa a `get_motor`, que o entrega ao construtor. `MotorBase` copia o conjunto para `self.feriados` e guarda o mesmo objeto em `_feriados_injetados`. Data presente retorna `True`. Data ausente com esse objeto injetado retorna `False` na linha 1010, inclusive se o conjunto for vazio. O `MotorIntermitente` substitui ambos por um conjunto vazio. `app/ponto/services/fechamento.py:298`, `app/ponto/services/fechamento.py:306`, `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:758`, `app/core/regua_cct.py:457`, `app/core/regua_cct.py:479`, `app/ponto/motor_calculo_v2.py:815`, `app/ponto/motor_calculo_v2.py:822`, `app/ponto/motor_calculo_v2.py:1007`, `app/ponto/motor_calculo_v2.py:1011`, `app/ponto/motor_calculo_v2.py:3698`, `app/ponto/motor_calculo_v2.py:3815`.
4. O resultado alimenta `FechamentoMensal` e a lavra diária `DiaPago(versao='motor')`. A geração do TXT chama a **mesma apuração** em `somente_leitura=True, retornar_objeto=True`, passa os objetos mensais a `montar_linhas_export` e lê deles as HE 100% para as rubricas. O calendário lê a linha diária já gravada. `app/ponto/services/fechamento.py:942`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/fechamento.py:1184`, `app/ponto/services/fechamento.py:1225`, `app/ponto/services/fechamento.py:1251`, `app/folha/views.py:215`, `app/folha/views.py:275`, `app/folha/views.py:288`, `app/folha/export.py:420`, `app/folha/export.py:739`, `app/colaboradores/services/calendario.py:319`, `app/colaboradores/services/calendario.py:688`.

## (b) O universo dos dois conjuntos

`feriados_do_colab` usa a geradora da célula se o vínculo for encontrado e a data estiver dentro da sua vigência. Se a célula nomeia uma geradora ausente ou fora das datas, o juiz devolve `None`. Sem geradora, escolhe entre os vínculos que cobrem a data, priorizando ativo e depois início mais recente. O município vem do posto desse vínculo. Sem município, só o nacional alcança o dia. `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:286`, `app/core/feriados.py:57`, `app/core/feriados.py:131`, `app/core/feriados.py:161`.

`feriados_de` aplica **um município fixo** à janela. O nacional vale para todos, o estadual exige a UF e o municipal exige UF e município normalizado. `feriados_do_colab` retém, para cada data, a resposta desse mesmo filtro no município daquele dia. Se o município permanece o mesmo, com a mesma janela e as mesmas linhas, os conjuntos coincidem. Numa troca de município, o conjunto fixo pode incluir datas do posto antigo após a troca e omitir datas do posto novo. Com `carregados=`, `feriados_de` confia no recorte entregue, enquanto `feriados_do_colab` só adiciona dias que percorre na janela pedida. `app/core/feriados.py:27`, `app/core/feriados.py:32`, `app/core/feriados.py:57`, `app/core/feriados.py:81`, `app/core/feriados.py:150`, `app/core/feriados.py:160`.

## (c) O que chega em `datas_previstas_trabalho`

O fechamento inicia um `set()` por colaborador. Dentro de cada fatia, `_montar_motor` só adiciona a data se `esc.eh_dia_trabalho(data)` for verdadeiro e ela estiver fora das datas suprimidas. `eh_dia_trabalho` retorna `cel.trabalha` quando há célula. Logo, **com a célula fornecida, 20/11 fica fora do conjunto e `_previstas_fornecidas=True`**, pois o construtor recebeu um conjunto, mesmo se vazio. O motor de dias órfãos também recebe um conjunto vazio explícito. A origem e o DNA concretos da célula do 1036 são [nao verificado]. `app/ponto/services/fechamento.py:726`, `app/ponto/services/fechamento.py:749`, `app/ponto/services/fechamento.py:760`, `app/ponto/services/fechamento.py:838`, `app/escala/models.py:1098`, `app/escala/models.py:1106`, `app/ponto/motor_calculo_v2.py:826`.

## (d) Onde o 100% pode ficar em zero

**O ramo que produziu o zero do 1036 é [nao verificado].** Os 7,29 h trabalhados e 1,16 h noturnos vêm do dado raspado. A lavra pode registrar ambos com HE 100% zero, inclusive quando o período vai para `ft_sem_escala`. Nesse caminho, a condição decisiva é `escala_certa_no_dia(...) == False` no filtro da linha **959**. O dado fornecido ainda permite outros caminhos. `app/ponto/services/fechamento.py:958`, `app/ponto/services/fechamento.py:1005`, `app/ponto/services/dia_pago.py:223`, `app/ponto/services/dia_pago.py:259`.

| Ordem da dobra direta | Valor no caso |
|---|---|
| 1. O cálculo usa o dia de início do turno. Nos cálculos por par, usa o mapa do juiz ou a data local da entrada crua quando falta a chave. Nos recálculos por dia, exige períodos no grupo. | Há batidas no recorte informado. O dia atribuído a cada par e a classe do motor são [nao verificado]. `app/ponto/motor_calculo_v2.py:976`, `app/ponto/motor_calculo_v2.py:3197`, `app/ponto/motor_calculo_v2.py:3442`, `app/ponto/motor_calculo_v2.py:3461`, `app/docs/LEIS.md:117`. |
| 2. O chamador exige `duracao > 0` ou `trab_dia > 0`. | A lavra mostra trabalhadas positivas. O valor na guarda do ramo que rodou é [nao verificado]. Os quatro chamadores são 12x36 por par, Turno Partido por dia, Comercial por par e Comercial por dia. `app/ponto/motor_calculo_v2.py:2925`, `app/ponto/motor_calculo_v2.py:3208`, `app/ponto/motor_calculo_v2.py:3385`, `app/ponto/motor_calculo_v2.py:3467`. |
| 3. `bool(self.feriado_em_dobra)`. Se falso, a expressão da linha 961 encerra a decisão. | [nao verificado]. O padrão vem do ciclo, com `12x36` no conjunto de feriado simples. A CCT ativa pode ligar a dobra para esse ciclo. `app/ponto/motor_calculo_v2.py:961`, `app/ponto/motor_calculo_v2.py:3740`, `app/ponto/motor_calculo_v2.py:3810`, `app/core/regua_cct.py:440`, `app/core/regua_cct.py:477`. |
| 4. `self.eh_feriado(dia)`. Data presente dá `True`. Data ausente no conjunto injetado dá `False` na linha 1010. | Para **20/11**, com os mesmos insumos do calendário e o conjunto preservado, dá `True` na linha 1008. O conjunto efetivo da lavra histórica e o dia consultado são [nao verificado]. O intermitente injeta vazio e retorna `False`. `app/ponto/services/fechamento.py:668`, `app/colaboradores/services/calendario.py:347`, `app/ponto/motor_calculo_v2.py:1007`, `app/ponto/motor_calculo_v2.py:1010`, `app/ponto/motor_calculo_v2.py:3698`. |

Se a dobra direta passa, os chamadores atribuem o trabalhado a `minutos_extra_100` e a `minutos_extra_100_feriado`. Para `resultado.periodos`, a lavra soma esses minutos divididos por 60 em `DiaPago.horas_extras_100` e no subconjunto de feriado. O leitor expõe `horas_extras_100` como `extras_100`. **A previsão fica fora da expressão da dobra**. A consulta a `datas_previstas_trabalho` na linha 996 pertence à folga semanal. `app/ponto/motor_calculo_v2.py:961`, `app/ponto/motor_calculo_v2.py:996`, `app/ponto/motor_calculo_v2.py:2927`, `app/ponto/motor_calculo_v2.py:3217`, `app/ponto/motor_calculo_v2.py:3387`, `app/ponto/motor_calculo_v2.py:3475`, `app/ponto/services/dia_pago.py:182`, `app/ponto/services/dia_pago.py:915`.

Para o caso com célula `False`, a sequência que pode mudar a rubrica depois da dobra direta é esta. `app/ponto/motor_calculo_v2.py:2558`, `app/ponto/services/fechamento.py:953`.

1. Com vínculo não intermitente e `data_jornada=20/11`, o `False` da célula faz `folga_do_vinculo=True` e o período vai para `periodos_ft`. Se foi esse o percurso do 1036, [nao verificado]. Antes da separação, a janela de HE pode zerar um par inteiro se houver folga ou feriado, janela de empresa vigente, previsto zero e ausência de autorização, com a exceção da folga sem escala certa. Um par zerado ali também perde seus minutos trabalhados e noturnos. Esses insumos por par são [nao verificado]. `app/ponto/motor_calculo_v2.py:1554`, `app/ponto/motor_calculo_v2.py:1563`, `app/ponto/motor_calculo_v2.py:1583`, `app/ponto/motor_calculo_v2.py:1587`, `app/ponto/motor_calculo_v2.py:2560`, `app/ponto/motor_calculo_v2.py:2574`, `app/ponto/motor_calculo_v2.py:2605`.
2. Em `periodos_ft`, **primeiro** passa pela guarda de escala certa. `LIMBO`, `A2` ou `A8` barram o período na linha 959. A lavra de `ft_sem_escala` conserva trabalhadas e noturnas e deixa o 100% desse período em zero. O motivo e a presença de `folga_sem_escala_min` na linha do 1036 são [nao verificado]. `app/ponto/services/fechamento.py:958`, `app/ponto/services/fechamento.py:1005`, `app/escala/services/escala_certa.py:31`, `app/escala/services/escala_certa.py:42`, `app/escala/services/escala_certa.py:51`, `app/ponto/services/dia_pago.py:259`.
3. **Depois**, se há vínculo e `feriado_vira_folga` dá `True`, o fechamento promove o período a HE 100% de feriado. O predicado exige `trabalha_em_feriado=False`, ciclo da lista `6x1`/`5x2`/`semanal` e feriado no município do posto. Se a promoção falha após passar pela escala certa, o período fica na rubrica de folga trabalhada. Vínculo, ciclo, chave e rubrica efetiva no caso são [nao verificado]. `app/ponto/services/fechamento.py:970`, `app/ponto/services/fechamento.py:979`, `app/escala/models.py:32`, `app/escala/models.py:1400`, `app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`.

## (e) Autoridade e censo em `app/`

**São duas interfaces da mesma regra de alcance, com entradas diferentes.** `feriados_de` responde por município fixo. `feriados_do_colab` resolve o município da pessoa no dia e chama `feriados_de`. `JUIZES.md` e o registro vivo nomeiam `core.feriados.eh_feriado` para a existência do feriado. O próprio `eh_feriado` delega a `feriados_de`. A dobra é outra pergunta, registrada em `CICLOS_FERIADO_SIMPLES`. `app/core/feriados.py:70`, `app/core/feriados.py:101`, `app/core/feriados.py:135`, `app/core/feriados.py:164`, `app/docs/JUIZES.md:115`, `app/docs/JUIZES.md:130`, `app/core/juizes.py:196`, `app/core/juizes.py:320`.

| Função | Chamadas diretas na cópia |
|---|---|
| `feriados_de` | `eh_feriado` e `feriados_do_colab` em `app/core/feriados.py:105` e `app/core/feriados.py:164`. Fila de chamados em `app/chamados/services/fila_por_causa.py:134`. Dias úteis de VT/VA em `app/colaboradores/models.py:1003`. Consulta e presença por posto em `app/colaboradores/services/feriado_do_posto.py:57` e `app/colaboradores/services/feriado_do_posto.py:112`. |
| `feriados_do_colab` | Fechamento em `app/ponto/services/fechamento.py:668`, calendário em `app/colaboradores/services/calendario.py:347`, precedência em `app/ponto/precedencia.py:331`, espelho em `app/ponto/services/espelho.py:1065`, PDF em `app/relatorios/pdf_espelho.py:298`. Comandos em `app/ponto/management/commands/desvio_o68b.py:113`, `app/ponto/management/commands/diff_espelho_alimentacao.py:93`, `app/relatorios/management/commands/diff_extrato_parcial.py:58`. Testes em `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:84`, `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:93`, `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:105`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:120`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:133`. |

**Nenhuma das duas funções deve sair sob a L-002.** A derivação paralela que a lei mandaria retirar seria uma escolha independente de praça fixa para responder pelo colaborador. Nessa rota, a chamada executável já usa o vínculo do dia. A própria L-086 registra que a cláusula original de feriado foi superada pela regra de ciclo e dia de início. A promoção de folga-feriado tem a guarda adicional de escala certa, registrada no BACKLOG como `FERIADO-SO-NA-ESCALA-CERTA`. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:118`, `app/ponto/services/fechamento.py:668`, `app/ponto/motor_calculo_v2.py:944`, `app/docs/BACKLOG.md:2164`.

O BACKLOG registra o pouso de `FERIADO-POR-POSTO` sem P4 e a porta única **da marca**. Essa porta propaga `Posto.opera_em_feriado` para `vinculo.trabalha_em_feriado` nos vínculos elegíveis. O corte da O275 restringe “trabalha em feriado” às escalas semanais e remete plantões à precedência existente. Esses registros tratam da escala, enquanto o alcance geográfico segue em `core.feriados`. `app/docs/BACKLOG.md:1984`, `app/docs/CORTES.md:616`, `app/colaboradores/services/feriado_do_posto.py:179`, `app/colaboradores/services/feriado_do_posto.py:368`, `app/core/feriados.py:160`.

## (f) Sonda de leitura

Para distinguir os ramos, a sonda mostra o conjunto reconstruído com o cadastro atual, a chave de feriado do vínculo, o motivo do juiz de escala certa, a decisão vigente de HE e o `DiaPago` gravado. Na linha lavrada, `fd=` em `versao_motor` é o carimbo da régua e `folga_sem_escala_min` identifica os minutos que passaram pelo ramo sem escala certa. `folga_feriado_min` identifica a promoção e `o85_dia_sem_vinculo_min` identifica trabalhadas órfãs. A coincidência entre o cadastro atual e o da lavra histórica é [nao verificado]. `app/ponto/services/fechamento.py:304`, `app/core/regua_cct.py:421`, `app/ponto/services/dia_pago.py:257`, `app/ponto/services/dia_pago.py:266`, `app/ponto/services/dia_pago.py:269`, `app/ponto/services/dia_pago.py:342`, `app/ponto/models.py:1265`.

```python
# Envie o bloco ao manage.py shell no tenant do caso.
from datetime import date, timedelta

from django.db import connection, transaction

from colaboradores.models import Colaborador
from core.feriados import carregar, feriados_de, feriados_do_colab, lugar_do_vinculo
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from escala.services.escala_certa import motivo_escala_errada
from ponto.janelas import janela_fechamento, periodo_apuracao
from ponto.models import DecisaoHE, DiaPago
from ponto.turnos import dia_das_batidas

cid, mes, ano = 1036, 11, 2026
a, d, b = (date(ano, mes, n) for n in (19, 20, 21))
um = timedelta(days=1)

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")

    col = Colaborador.objects.select_related("empresa").get(pk=cid)
    ini, fim = periodo_apuracao(mes, ano, 2)[0], periodo_apuracao(mes, ano, 28)[1]
    celulas = celulas_do_periodo([cid], ini, fim)
    vinculos = escalas_do_periodo(
        cid, ini - um, fim + um, mais=[c.escala_geradora_id for c in celulas.values()]
    )
    v = vinculo_do_dia(cid, d, vinculos, celulas=celulas)
    posto, mun = getattr(v, "posto", None), lugar_do_vinculo(v)
    fer = feriados_do_colab(
        col, ini - um, fim + um, celulas=celulas, vinculos=vinculos,
        carregados=carregar(ini - um, fim + um),
    )

    print("colab, empresa, janela", cid, col.empresa_id, janela_fechamento(mes, ano, col.empresa))
    print("motor e calendario (vinculo, posto, praca, municipio usado)",
          getattr(v, "pk", None), getattr(posto, "pk", None),
          getattr(posto, "praca_id", None), getattr(mun, "pk", None))
    print("feriados_de_municipio_fixo", sorted(feriados_de(mun, a, b)))
    print("feriados_do_colab_injecao", sorted(x for x in fer if a <= x <= b))

    cel = celulas.get((cid, d))
    print("celula", None if cel is None else (cel.pk, cel.trabalha, cel.origem, cel.dna))
    print("vinculo (tipo_id, base, ciclo, trabalha_em_feriado, feriado_vira_folga)",
          None if v is None else (v.tipo_escala_id, v.tipo_escala.tipo_base,
                                  v.tipo_escala.tipo_ciclo, v.trabalha_em_feriado,
                                  v.feriado_vira_folga(d)))
    print("motivo_escala_errada", motivo_escala_errada(cid, d))
    print("dia_por_id_batida", sorted(dia_das_batidas(col, a, b).items()))
    print("janela_he (ativa, desde)", getattr(col.empresa, "janela_he_ativa", None),
          getattr(col.empresa, "janela_he_desde", None))
    print("decisao_he_vigente", list(DecisaoHE.objects.vigentes().filter(
        colaborador_id=cid, data=d).values_list("pk", "data", "estado")))

    dp = DiaPago.objects.filter(
        colaborador_id=cid, mes=mes, ano=ano, data=d, tipo=DiaPago.TIPO_DIA, versao="motor"
    ).values(
        "pk", "data", "lavrado_em", "versao_motor", "horas_trabalhadas",
        "horas_noturnas", "horas_extras_100", "horas_extras_100_feriado",
        "horas_folga_trabalhada", "detalhe",
    ).first()
    if dp is not None:
        detalhe = dp.pop("detalhe") or {}
        for chave in ("folga_sem_escala_min", "folga_feriado_min", "o85_dia_sem_vinculo_min"):
            if chave in detalhe:
                dp[chave] = detalhe[chave]
    print("dia_lavrado", dp)
```

## 27/11 11:39 · sessao b4b559a914e3 · FERIADO-NAO-PREVISTO-L086 · leitura · origem=Code

## 1. Sonda de medida

A tela usa `core.feriados.feriados_do_colab` e cruza a data retornada com `CelulaDia.trabalha is False`. A função resolve o município pelo posto do vínculo que responde pelo dia. `app/colaboradores/services/calendario.py:346` `app/colaboradores/services/calendario.py:785` `app/core/feriados.py:124` `app/core/feriados.py:135` `app/core/feriados.py:161`

A fonte diária gravada é `DiaPago`, com `tipo='dia'` e `versao='motor'`. O fechamento grava essas linhas a partir da mesma apuração que alimenta `FechamentoMensal`. A geração do TXT nesta cópia monta as rubricas a partir dos objetos mensais que recebe de `recalcular_fechamento_mes`; a sonda atende ao recorte de **lavra gravada** lendo `DiaPago`, sem chamar essa rotina. `app/ponto/models.py:311` `app/ponto/services/fechamento.py:1219` `app/ponto/services/fechamento.py:1251` `app/folha/views.py:214` `app/folha/views.py:275` `app/folha/export.py:420`

O resumo distingue dia com lavra e `horas_trabalhadas <= 0` de dia sem linha lavrada. O filtro usa o campo literal `horas_trabalhadas > 0`. Assim, um dia cuja hora foi registrada integralmente em HE100 de feriado pode ficar fora dos detalhes e ainda contar no universo. `app/colaboradores/services/calendario.py:777` `app/ponto/models.py:325` `app/ponto/services/dia_pago.py:244`

Bloco para a entrada padrão de `python -B manage.py shell --settings=config.settings.sombra`. A configuração aponta para `sombra_db`, banco `sombra`, onde o schema do tenant é `juliani`. Execução e totais da sombra **[nao verificado]**. `app/config/settings/sombra.py:7` `app/config/settings/sombra.py:29` `app/config/settings/sombra.py:47`

```python
from collections import defaultdict
from datetime import date
from math import fsum

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Empresa
from core.feriados import carregar, feriados_do_colab
from escala.models import CelulaDia
from ponto.janelas import janela_fechamento
from ponto.models import DiaPago


MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA_PEDIDA = (date(2026, 10, 21), date(2026, 11, 20))


def medir_empresa(empresa):
    inicio, fim = janela_fechamento(MES, ANO, empresa)
    if (inicio, fim) != JANELA_PEDIDA:
        raise RuntimeError(
            f"Empresa {empresa.pk}: janela encontrada {inicio} a {fim}"
        )

    # O mapa entregue a feriados_do_colab cobre a janela inteira.
    # app/core/feriados.py:135
    celulas = {
        (celula.colaborador_id, celula.data): celula
        for celula in CelulaDia.objects.filter(
            colaborador__empresa_id=empresa.pk,
            data__range=(inicio, fim),
        ).only("colaborador", "data", "trabalha", "escala_geradora")
    }
    folgas_por_colab = defaultdict(set)
    for (colab_id, dia), celula in celulas.items():
        if celula.trabalha is False:
            folgas_por_colab[colab_id].add(dia)

    feriados_cadastrados = carregar(inicio, fim)
    alvos = set()
    for colab_id, folgas in folgas_por_colab.items():
        feriados = feriados_do_colab(
            colab_id, inicio, fim,
            carregados=feriados_cadastrados,
            celulas=celulas,
        )
        alvos.update((colab_id, dia) for dia in folgas & feriados)

    # DiaPago.data ja e uma data. A chave do dia vem da lavra.
    # app/ponto/models.py:315 e app/ponto/services/dia_pago.py:109
    linhas = DiaPago.objects.filter(
        colaborador_id__in={colab_id for colab_id, _ in alvos},
        mes=MES,
        ano=ANO,
        tipo=DiaPago.TIPO_DIA,
        versao="motor",
        data__range=(inicio, fim),
    ).values_list(
        "colaborador_id", "data", "horas_trabalhadas", "horas_noturnas",
        "horas_extras_100", "horas_extras_100_noturna",
    )
    lavra = {(colab_id, dia): tuple(horas) for colab_id, dia, *horas in linhas}

    detalhes = []
    sem_lavra = 0
    com_lavra_sem_horas_positivas = 0
    for colab_id, dia in sorted(alvos):
        horas = lavra.get((colab_id, dia))
        if horas is None:
            sem_lavra += 1
        elif horas[0] > 0:
            detalhes.append((colab_id, empresa.pk, dia.isoformat(), *horas))
        else:
            com_lavra_sem_horas_positivas += 1

    resumo = (
        empresa.pk,
        len(alvos),
        len(detalhes),
        com_lavra_sem_horas_positivas,
        sem_lavra,
        fsum(linha[3] for linha in detalhes),
        fsum(linha[5] for linha in detalhes),
    )
    return detalhes, resumo


def sondar():
    if (
        not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict["NAME"] != "sombra"
        or connection.settings_dict["HOST"] != "sombra_db"
    ):
        raise RuntimeError("Use a configuracao do banco sombra.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transacao externa propria.")

    medido_em = timezone.localtime(timezone.now())
    with schema_context("juliani"), transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco ou schema diferente do solicitado.")

        resultados = []
        for empresa_id in EMPRESAS:
            empresa = Empresa.objects.only("dia_inicio_competencia").get(pk=empresa_id)
            resultados.append(medir_empresa(empresa))

    print("medido_em", medido_em.isoformat(timespec="seconds"), sep="\t")
    print(
        "colab_id", "empresa_id", "data", "horas_trabalhadas", "horas_noturnas",
        "horas_extras_100", "horas_extras_100_noturna", sep="\t",
    )
    for detalhes, _ in resultados:
        for linha in detalhes:
            print(*linha, sep="\t")

    print(
        "empresa_id", "universo", "com_horas_positivas",
        "com_lavra_sem_horas_positivas", "sem_lavra",
        "total_horas_trabalhadas", "total_horas_extras_100", sep="\t",
    )
    resumos = [resumo for _, resumo in resultados]
    for resumo in resumos:
        print(*resumo, sep="\t")
    print("universo_total", sum(resumo[1] for resumo in resumos), sep="\t")


sondar()
```

## 2. Dono e regra da dobra

**O78** é o registro explícito da L-086, **fechado como selo**. **FERIADO-POR-POSTO** é a obra da marca cadastral por posto, registrada **no ar em 22/11 às 13:34**, com o P4 destacado como `P4-SYNC-FERIADO`. Para a fatia de dinheiro, o antecedente direto é **ESPELHO-FERIADO-DOBRA**, fechado, e o ramo de pagamento do **BO-B (3)** que alimenta o fechamento e o `DiaPago`. `app/docs/BACKLOG.md:1501` `app/docs/BACKLOG.md:1984` `app/docs/BACKLOG.md:1985` `app/docs/BACKLOG.md:2044` `app/docs/BACKLOG.md:1667` `app/ponto/services/fechamento.py:960`

O grep também localiza **FERIADO-DIA-CHAVE**, registrado para a atribuição do dia na virada da meia-noite, e **O275/O277**, respectivamente interrompida e registrada, com O277 dependente da O275 no registro. O aval anexo põe a cura de 1036 na fila de dinheiro de `SO-DOIS-ALIVIOS-v2`. O registro formal dessa fatia pelo identificador `FERIADO-NAO-PREVISTO-L086` nos três arquivos é **[nao verificado]**. `app/docs/BACKLOG.md:1668` `app/docs/GRADE-MESTRA.md:91` `app/docs/GRADE-MESTRA.md:233` `app/docs/GRADE-MESTRA.md:234` `app/docs/CORTES.md:616` `aval.txt:1`

**Quem decide no motor.** `MotorBase._feriado_dobra_no_dia(dia)` retorna `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`. No caminho normal do fechamento, **sim, é a mesma fonte que a tela usa**. O fechamento chama `feriados_do_colab` e injeta o conjunto no motor, cobrindo também as bordas da janela de batidas. Sem injeção, `eh_feriado` recorre a `is_feriado(data)` sem município, que enxerga só o nacional. `MotorIntermitente` injeta um conjunto vazio próprio. `app/ponto/motor_calculo_v2.py:941` `app/ponto/motor_calculo_v2.py:1000` `app/ponto/motor_calculo_v2.py:30` `app/ponto/motor_calculo_v2.py:3694` `app/ponto/services/fechamento.py:611` `app/ponto/services/fechamento.py:668` `app/ponto/services/fechamento.py:754`

| Campo ou insumo | Quem escreve e quem lê |
|---|---|
| `feriados` e `_feriados_injetados` | `feriados_do_colab` compõe as datas. `MotorBase.__init__` grava os atributos em memória e `MotorBase.eh_feriado` os lê. `MotorIntermitente.__init__` grava o vazio nos dois. `app/core/feriados.py:159` `app/ponto/motor_calculo_v2.py:817` `app/ponto/motor_calculo_v2.py:1007` `app/ponto/motor_calculo_v2.py:3698` |
| `Feriado.data`, `abrangencia`, `uf`, `municipio` e lugar do vínculo | `form_feriado` cria e edita o cadastro, e o importador insere nacionais. `_qs`, `alcanca` e `feriados_de` leem as datas e o alcance. `executar_vinculo` grava o posto no vínculo. A porta de estrutura grava `Posto.municipio`, e `lugar_do_vinculo` lê esse município do vínculo escolhido por `vinculo_do_dia`. `app/core/views_config.py:64` `app/core/views_config.py:83` `app/core/views_config.py:144` `app/core/feriados.py:27` `app/core/feriados.py:57` `app/core/feriados.py:70` `app/colaboradores/services/vinculo.py:275` `app/colaboradores/services/vinculo.py:287` `app/colaboradores/services/estrutura.py:67` `app/colaboradores/services/estrutura.py:99` `app/escala/alimentacao.py:269` `app/core/feriados.py:124` |
| `TipoEscala.tipo_ciclo` e `feriado_em_dobra` | `salvar_tipo` grava o ciclo. `get_motor` lê o ciclo e resolve o parâmetro por `CICLOS_FERIADO_SIMPLES`, hoje `{'12x36'}`. `MotorBase.__init__` guarda o booleano, e `_feriado_dobra_no_dia` o lê. Com a régua CCT ativa e cláusula aplicável, `get_motor_cct` fornece `True` para 12x36. O formulário de sindicato grava `feriado_12x36_em_dobra` e `regua_para` lê a cláusula da convenção aplicável. Escritor da chave global `regua_cct_ativa` **[nao verificado]**. `app/escala/services/cadastro_tipo.py:152` `app/escala/services/cadastro_tipo.py:307` `app/ponto/motor_calculo_v2.py:3740` `app/ponto/motor_calculo_v2.py:3765` `app/ponto/motor_calculo_v2.py:3810` `app/ponto/motor_calculo_v2.py:814` `app/ponto/motor_calculo_v2.py:961` `app/core/views_config.py:282` `app/core/views_config.py:312` `app/core/views_config.py:323` `app/core/regua_cct.py:366` `app/core/regua_cct.py:444` `app/core/regua_cct.py:473` |
| Dia do turno e minutos trabalhados | `_dia_do_turno` monta o mapa de instante da entrada para dia do turno. `calcular_mes` guarda o mapa e `_dia_do_turno_do_par` o lê, usando a data local da entrada crua se a chave faltar. A apuração do período escreve `minutos_trabalhados`, e os ramos de dobra exigem duração positiva antes de gravar HE100. `app/ponto/motor_calculo_v2.py:911` `app/ponto/motor_calculo_v2.py:2510` `app/ponto/motor_calculo_v2.py:963` `app/ponto/motor_calculo_v2.py:2898` `app/ponto/motor_calculo_v2.py:2925` `app/ponto/motor_calculo_v2.py:3208` `app/ponto/motor_calculo_v2.py:3372` `app/ponto/motor_calculo_v2.py:3385` `app/ponto/motor_calculo_v2.py:3467` |
| `CelulaDia.trabalha`, `escala_geradora`, `trabalha_em_feriado` e `opera_em_feriado` | A porta da célula grava `trabalha` e `escala_geradora` na geração, regeneração e edição. A tela lê `trabalha`, o juiz do vínculo lê `escala_geradora`, e o motor lê a resposta de `eh_dia_trabalho` para classificar folga. A porta da marca grava `opera_em_feriado` e propaga para vínculos elegíveis via `marcar_trabalha_em_feriado`. O nascimento do vínculo também preenche a chave. `feriado_vira_folga` lê essa chave, o ciclo e o feriado local. `app/ponto/portas/celula.py:289` `app/ponto/portas/celula.py:655` `app/colaboradores/services/calendario.py:791` `app/escala/alimentacao.py:269` `app/escala/models.py:1105` `app/ponto/motor_calculo_v2.py:2560` `app/colaboradores/services/estrutura.py:90` `app/colaboradores/services/feriado_do_posto.py:334` `app/colaboradores/services/feriado_do_posto.py:368` `app/colaboradores/services/vinculo.py:297` `app/colaboradores/services/vinculo.py:903` `app/escala/models.py:1400` |

**O ramo que importa quando a célula diz folga.** O motor pode encaminhar o período para `periodos_ft`. O fechamento filtra esses períodos por `escala_certa_no_dia` e promove a HE100 de feriado aqueles cujo vínculo do dia diz `feriado_vira_folga`. O fechamento grava os totais mensais e `dia_pago.lavrar` grava a mesma partição por dia. Nesse ramo, `feriado_vira_folga` consulta `core.feriados.eh_feriado` pelo lugar do vínculo, a mesma autoridade geográfica usada por `feriados_do_colab`. `app/ponto/motor_calculo_v2.py:2560` `app/ponto/motor_calculo_v2.py:2581` `app/ponto/motor_calculo_v2.py:2605` `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:968` `app/ponto/services/fechamento.py:978` `app/ponto/services/dia_pago.py:244` `app/escala/models.py:1400` `app/core/feriados.py:101`

### O que os registros mandam para feriado do colaborador, célula `False` e batida

| Lei, corte ou obra | Efeito para o caso |
|---|---|
| **L-086** | O texto original mandava dobrar o feriado trabalhado fora da escala. A própria linha registra a revogação da cláusula de feriado. A regra remanescente trata do domingo e preserva o dia de início da jornada. `app/docs/LEIS.md:117` `app/docs/LEIS.md:118` `app/ponto/motor_calculo_v2.py:980` |
| **Corte de 27/09 03:xx** | Para o feriado, valem o dia de início do turno e a régua de dobra do ciclo. `trabalha=False` por si só é insuficiente para concluir pela dobra. No 12x36, o padrão é simples e uma CCT aplicável pode autorizar a dobra. `app/ponto/motor_calculo_v2.py:941` `app/ponto/motor_calculo_v2.py:3740` `app/core/regua_cct.py:473` |
| **O78** | O registro histórico esperava dobra fora da escala. O selo atual testa início no feriado com dobra habilitada, véspera, ciclo sem dobra e a regra de domingo. O selo atual admite dobra mesmo com trabalho previsto quando a régua do ciclo permite. `app/docs/BACKLOG.md:1501` `app/ponto/tests/test_selo_l086_feriado_previsto_nao_dobra.py:43` `app/ponto/tests/test_selo_l086_feriado_previsto_nao_dobra.py:74` |
| **O277 — SDF-FERIADO-SEM-DOBRA** | O corte manda hora sem dobra para SDF e intermitente, e manda o cartão omitir a afirmação de pagamento em dobro. Para SDF, o registro vincula a aplicação ao cadastro da O275. Para intermitente, a cópia já mostra a neutralização do calendário de dobra no motor v2. `app/docs/BACKLOG.md:1698` `app/docs/BACKLOG.md:2245` `app/docs/GRADE-MESTRA.md:234` `app/ponto/motor_calculo_v2.py:3694` |
| **CORTE-CADERNO-1330-4-FERIADO-SO-SEMANAL** | Restringe a chave de previsão “trabalha em feriado” à escala semanal, citando 6x1 e 5x2. Plantões 12x36 e 24x48 seguem a precedência existente. O corte trata da previsão do dia e deixa a rubrica a cargo da regra de pagamento aplicável. `app/docs/CORTES.md:276` `app/docs/CORTES.md:616` |
| **BO-B (3), 23/10 15:0x** | Para trabalho em feriado numa escala que folga em feriado, o corte manda lançar HE100 de feriado. O ramo implementado exige um período em `periodos_ft` que passe por `escala_certa_no_dia` e pelo `feriado_vira_folga` do vínculo do dia. `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:960` `app/ponto/services/fechamento.py:973` `app/ponto/services/dia_pago.py:244` |

O aval relata para **1036 em 20/11** feriado do colaborador, célula `False`, batidas, **7,29 h trabalhadas, 1,16 h noturnas e 0,0 h de HE100**. O ciclo, a régua aplicável, a chave `trabalha_em_feriado` do vínculo e o resultado do crivo de escala certa para esse dia estão **[nao verificado]**. Esses insumos são necessários para atribuir o zero ao ramo exato. `aval.txt:1` `app/ponto/motor_calculo_v2.py:961` `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:973`

## 27/11 11:37 · sessao 318029dd4fa3 · FERIADO-BOTAO-CORTADO · construtor · origem=Code

**FERIADO-BOTAO-CORTADO** — O botão fica logo abaixo da data sempre que a célula o oferece. No mês, a altura continua em 132 px e o conteúdo abaixo do botão rola com mouse, toque ou teclado. Na fita, a ação vem antes dos horários e o rótulo quebra linha. Esse arranjo usa a mesma ordem nos dois layouts e mantém disponíveis as marcações originais, inclusive as retratadas. [Grade](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68) · [Ação e rolagem](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95) · [Marcações](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:133)

### Diff e RED

- A grade move o include acima das marcações, reserva espaço para a ação no mês e torna o restante da célula rolável. A condição do include continua `pode_editar_dia and dia.feriado_vira_trabalho_oferecido`. [Grade](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95)
- O partial limita a largura do controle à célula e permite a quebra do rótulo. [Controle do feriado](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/ponto/partials/_feriado_vira_trabalho.html:5)
- O teste renderiza o include real com uma fixture fixa no formato do contexto do calendário. Em `mes` e `fita`, confere o formulário na célula de 20/11, as quatro marcações e a ação em um bloco irmão anterior às marcações. Também confere o botão em 15/11 sem batidas e sua ausência no dia comum. Na fonte pré-cura, a ação vinha depois dos horários, invertendo a ordem exigida pelo `assertLess`. Execução RED [nao verificado]. [Fixture](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/colaboradores/tests/test_feriado_botao_cortado.py:32) · [Asserção RED](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/colaboradores/tests/test_feriado_botao_cortado.py:92) · [Controles](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/colaboradores/tests/test_feriado_botao_cortado.py:109) [nao verificado]

### Execução e mutantes

O vizinho `test_relogio_riscado_tamanho` rodou **8 testes, todos passaram**, com `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v core.tests.test_relogio_riscado_tamanho`. RED, cura, M1–M3 e a rodada dos vizinhos com [nome] pararam na importação de `django.core`. O resultado dessas asserções permanece [nao verificado]. [Vizinho](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_relogio_riscado_tamanho.py:61) · [Entrada da suíte](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/manage.py:11) [nao verificado]

Comandos da suíte Code, a partir de `app/`. Cada mutante restaura o template ao sair. M1 retira o layout em coluna, M2 corta a rolagem e M3 volta a impedir a quebra do rótulo. [Asserções correspondentes](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/colaboradores/tests/test_feriado_botao_cortado.py:95) [nao verificado]

```bash
teste() { [nao verificado]
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
    colaboradores.tests.test_feriado_botao_cortado
}
mutar() (
  alvo=$1
  original=$(<"$alvo")
  trap 'printf "%s\n" "$original" > "$alvo"' EXIT
  alterado=$(sed "$2" <<<"$original")
  [[ "$alterado" != "$original" ]] || exit 2
  printf '%s\n' "$alterado" > "$alvo"
  teste [nao verificado]
)

grade=templates/colaboradores/partials/_calendario_grade.html
acao=templates/ponto/partials/_feriado_vira_trabalho.html

teste [nao verificado]
mutar "$grade" 's/display:flex;flex-direction:column;{% endif %}/display:block;{% endif %}/'
mutar "$grade" 's/min-height:0;overflow-y:auto;/min-height:0;overflow-y:hidden;/'
mutar "$acao" 's/white-space:normal;overflow-wrap:anywhere;/white-space:nowrap;/'

mapfile -t vizinhos < <(rg -l '_calendario_grade' -g 'test_*.py' */tests |
  sort | sed 's|/|.|g; s|\.py$||')
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  "${vizinhos[@]}" core.tests.test_smoke_retratar_1366 ponto.tests.test_kaua_feriado_pela_tela
```

### Smoke do Ronald

1. No perfil do colaborador do anexo, abrir **Calendário de Ponto → Mês → novembro → 20/11**. Conferir o rótulo inteiro, abrir o formulário sem enviar e percorrer as quatro marcações dentro da célula. Em **15/11**, conferir o botão sem batidas. O identificador `colNN` é [nao verificado]. [Tela do perfil](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:204) · Anexo `FERIADO-BOTAO-CORTADO-2711-1120.txt:1`.
2. No drawer de diagnóstico do mesmo colaborador, abrir **Escala x batidas**, deslizar até **20/11** e conferir o rótulo inteiro, a abertura do formulário e os quatro horários. Smoke em navegador [nao verificado]. [Fita do diagnóstico](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_drawer_diagnostico.html:33)

## 27/11 11:31 · sessao 307a837071f5 · CASO-LITERAL-14 · sonda · origem=Code

A sonda fecha **163 em 18/11 como (a)**. A resposta de volta às `05:07`, a batida `158099` às `05:06` e a pausa reconhecida pelo pareador identificam a marca que ficou sem casar na ata. `app/chamados/services/materializacao.py:595` · `app/ponto/turnos.py:502`

| Dia | Classe | Prova na saída da sonda | Porta |
|---|---|---|---|
| **163 · 18/11** | **(a)** | `ERROR pergunta_id=59341 motivo=intervalo_volta resposta=05:07`, com `marcos_faltantes=E x ata=S`. `PAUSAS 1` liga `158092` às `04:07:13` a `158099` às `05:06:08`, e `TURNO 17/11` fecha em `158201`. `LUZ 140478 3` mostra `03:00` apagada e `PAPEL_X` lista `158099`. A volta declarada ficou sem casar na ata. `app/chamados/services/materializacao.py:595` · `app/ponto/turnos.py:502` | **obra que cura** |
| **163 · 20/11** | **(d)** | `PAUSAS 1` liga `160127` a `160135`, `TURNO 19/11` fecha em `160202` e `LUZ 140480 3` está apagada. `PERG 60094` mostra o slot `03:00/E`, que pode vir do marco fixo. `PERG 60098` liga `160135` à pergunta e `ATO 6391` indica texto presente. Falta a declaração que confirme `160135` como volta da pausa [nao verificado]. `app/chamados/services/materializacao.py:702` · `app/ponto/turnos.py:1316` | não sei |
| **334 · 10/11** | **(d)** | `LUZ 132554 2` atribui `02:02` ao fechamento previsto às `08:00`, mas `TURNO 09/11` fecha em `150563` e outro abre em `150585`, com `saida=None`. `PERG 55996` está sem resposta e tem `via_celula_absolveu=1`. Falta confirmar qual marca fechou o plantão ou localizar uma saída posterior a `150585` [nao verificado]. `app/ponto/juiz_batida.py:263` · `app/chamados/models.py:2847` | não sei |
| **334 · 16/11** | **(d)** | `TURNO 15/11` fecha `155980 → 156062`, mas `LUZ 132560 2` mostra o fechamento apagado. `TRILHA 1012709`, `1012700` e `1012703` registram retratação humana de `156083`, `157001` e `157080`. Falta confirmar o papel de `156062` e esclarecer se houve trabalho na sequência iniciada por `157001` e como terminou [nao verificado]. `app/ponto/juiz_batida.py:263` · `app/ponto/services/retratar_humano.py:89` | não sei |
| **334 · 17/11** | **(d)** | `BAT 157080` tem `retratada_em=27/11 11:08:35`. `TURNO 17/11` deixa `157102` e `157339` em registros separados, ambos com `entrada=None`. `PERG 58935` tem `via_celula_absolveu=1`. Falta atribuir as saídas ao trabalho efetivamente realizado [nao verificado]. `app/ponto/turnos.py:312` · `app/chamados/models.py:2847` | não sei |
| **446 · 01/11** | **(d)** | `TURNO 01/11` abre em `143320`, contém `143451` e `143543` e tem `saida=None`. `LUZ 136277 4` mostra `23:00` apagada. Falta a saída verdadeira após `143543` [nao verificado]. `app/ponto/turnos.py:321` · `app/ponto/juiz_batida.py:263` | não sei |
| **219 · 14/11** | **(d)** | `BAT 159534` tem `pergunta_origem_id=57759`, e `ATO 6118` confirma a validação. `TURNO 13/11` fecha em `154644` e `TURNO 14/11` separa `154790 → 154793`. `PERG 57827/57828` têm `via_celula_absolveu=1`. Falta comprovar se o par de `07:03–07:04` foi trabalho ou toque excedente [nao verificado]. `app/chamados/models.py:2847` · `app/chamados/juizes.py:771` | não sei |
| **446 · 25/10** | **(d)** | `TURNO 25/10` deixa `136793` aberto e usa `136990` como entrada do período que fecha em `144667`. `BAT 144666/144667` apontam para `PERG 52823/52825`, com slots `18:00/S` e `23:00/S`. Falta confirmar se `136793 → 136990` foi trabalho e a qual jornada pertencem as duas saídas plantadas [nao verificado]. `app/ponto/turnos.py:312` · `app/chamados/juizes.py:771` | não sei |
| **446 · 27/10** | **(d)** | `TURNO 27/10` deixa `138687` aberto e fecha `138784 → 145038`, com `144664` no meio. `BAT 144664/145038` apontam para `PERG 52820/53401`; `PERG 52821` tem slot `19:00/E` e `validada_em=None`. Falta a declaração do `CHAMADO 36462` que identifique o trabalho e as saídas de cada sequência [nao verificado]. `app/chamados/juizes.py:360` · `app/chamados/juizes.py:771` | não sei |
| **465 · 12/11** | **(d)** | `TURNO 12/11` separa `152760 → 152798` de `152988 → 153457`, com pausa `152993 → 153079`. `LUZ 130759 1–4` acende às `07:33`, `12:31`, `13:34` e `19:32`. Falta confirmar o trabalho antes de `152798` e o papel de `152988` frente à pausa [nao verificado]. `app/ponto/juiz_batida.py:230` · `app/ponto/turnos.py:502` | não sei |
| **707 · 27/10** | **(d)** | `DNA 130929` mostra bordas anteriores `11:00–23:00` e atuais `07:00–19:00`. `CEL 130929` registra `regenerada_em=27/11 10:11:06`. O marco anterior de `16:00` coincide com `BAT 152915`, retratada na `TRILHA 1012218`. O turno atual fecha `152914 → 151794` e inclui `152916`, cujo flip consta na `TRILHA 1012246`. A troca de régua está provada. Falta o lastro que atribua as marcas concorrentes ao trabalho ou à escala anterior [nao verificado]. `app/ponto/portas/celula.py:297` · `app/ponto/services/flip_batida.py:89` | não sei |
| **1010 · 04/11** | **(d)** | `TRILHA 932956` confirma a retratação humana de `146307`, com `motivo_retratacao.id=1` e observação presente. `CEL 143296` tem `trabalha=0`. O `TURNO` vigente deixa `146083` aberto. `TURNO 146307` fecha `146083 → 146380` só na reapresentação. Faltam o conteúdo do motivo e a confirmação de trabalho iniciado em `146083` [nao verificado]. `app/ponto/services/retratar_humano.py:95` · `app/ponto/turnos.py:1920` | não sei |
| **1010 · 05/11** | **(d)** | `TURNO` vigente atribui `146321 → 146380` a `05/11`, separado de `147009 → 147338`. `TURNO 146307` desloca as duas primeiras marcas para o plantão de `04/11` ao reapresentar a batida retratada. Falta confirmar se eram volta e saída da véspera ou um período próprio [nao verificado]. `app/ponto/turnos.py:285` · `app/ponto/turnos.py:1920` | não sei |
| **1010 · 07/11** | **(d)** | `BAT 151808` tem `pergunta_origem_id=54816`. `PERG 54816` declara `data_evento=06/11` e slot `07/11 05:54/S`, com validação no `ATO 1137`. `CEL 143298` tem `trabalha=0` e o `TURNO 07/11` mostra `151808` com `entrada=None`. Falta provar o trabalho de `06/11` e se `151808` completou ou duplicou a saída [nao verificado]. `app/chamados/juizes.py:771` · `app/ponto/turnos.py:312` | não sei |
| **989 · 25/10** | **(d) nesta volta** | A saída traz `DIA 2 446 2026-10-25`. Para **989**, os campos de `BAT 137193`, o modo de intervalo, a ata anterior e o consumo das marcas pelos dias vizinhos seguem [nao verificado]. A classe histórica (a) vem de `r.md`; o predicado exato segue sem prova nesta rodada. `app/escala/models.py:243` · `app/escala/utils.py:1686` | não sei |

**Totais da tabela, 15 dias** — **(a) 1 · (b) 0 · (c) 0 · (d) 14**.

## 27/11 11:28 · sessao ede27dcbb6db · FERIADO-BOTAO-CORTADO · censo · origem=Code

## CENSO

- leitores: A grade lê `pode_editar_dia` e `dia.feriado_vira_trabalho_oferecido` na mesma condição e inclui o formulário com colaborador, data e URL de retorno (`app/templates/colaboradores/partials/_calendario_grade.html:131-132`). O POST verifica `editar_escalas` e relê a oferta para o dia solicitado (`app/ponto/views.py:3607-3609`, `app/ponto/views.py:3623-3633`). Três templates incluem a grade. O calendário do perfil usa `layout='mes'` (`app/colaboradores/views.py:1990-2010`, `app/templates/colaboradores/calendario_partial.html:67`). O drawer de diagnóstico usa `layout='fita'` (`app/colaboradores/views.py:2987-2992`, `app/colaboradores/services/drawer.py:122-124`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33-35`). A Auditoria de Espelhos usa a grade automática com `only`, passando `dias` e `resumo`, sem a permissão do botão (`app/relatorios/views.py:1609`, `app/relatorios/views.py:1634`, `app/relatorios/furos.py:164-168`, `app/templates/relatorios/furos.html:37-42`). O perfil carrega o fragmento por HTMX e a aba Espelho do drawer genérico carrega a mesma rota por `fetch` (`app/templates/colaboradores/detalhe.html:207-215`, `app/templates/colaboradores/partials/painel_colaborador.html:181-185`, `app/templates/core/_drawer_generico.html:132-142`). O Painel Situacional e a lista de pendências abrem o drawer de diagnóstico (`app/templates/colaboradores/partials/_ps_linha.html:4`, `app/templates/colaboradores/partials/pendencias_disputa.html:9`). `fita-mes.js` mede os filhos diretos da fita e lê `data-mes`. `popover-acao.js` mede o `summary` para posicionar o painel (`app/static/js/fita-mes.js:5-17`, `app/static/js/popover-acao.js:33-45`). Os leitores de estrutura e geometria são os selos de altura e grade, sete colunas, chips, pílulas, popovers e oferta do feriado, discriminados nas NOTAS.
- escritores: `contexto_calendario` grava `feriado_vira_trabalho_oferecido` quando a data é feriado daquele colaborador e a célula existe com `trabalha is False` (`app/colaboradores/services/calendario.py:343-347`, `app/colaboradores/services/calendario.py:785-793`). `calendario_ponto` põe `pode_editar_dia=True` quando `editar_escalas` é permitida. `contexto_drawer` põe o booleano retornado pela mesma permissão (`app/colaboradores/views.py:2005-2010`, `app/colaboradores/services/drawer.py:156-162`). Os includes escolhem o layout e a grade escreve a geometria inline. O modo `mes` usa sete colunas e célula com `height:132px;min-height:132px;overflow:hidden`. A fita usa contêiner horizontal rolável e `flex:0 0 78px` na célula. A grade escreve também o bloco de horários e o contêiner da ação, após os horários (`app/templates/colaboradores/calendario_partial.html:67`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`, `app/templates/relatorios/furos.html:42`, `app/templates/colaboradores/partials/_calendario_grade.html:64-68`, `app/templates/colaboradores/partials/_calendario_grade.html:124-132`). O partial da ação escreve o `summary`, o formulário e seus estilos. Ao abrir, o JS existente aplica `position:fixed` e calcula a posição do painel (`app/templates/ponto/partials/_feriado_vira_trabalho.html:5-12`, `app/static/js/popover-acao.js:35-76`, `app/static/js/popover-acao.js:88-92`).
- decisoes:
  - **Posição da ação e altura.** O brief delega o desenho. A opção mais simples para `mes` é reservar espaço para a ação no rodapé da própria célula e dar rolagem vertical à área de conteúdo acima, mantendo os 132 px. Na `fita`, usar o mesmo controle e manter a altura natural. Permitir que o rótulo quebre dentro da largura disponível, pois o `summary` hoje usa `white-space:nowrap`. O corte UI-GRADE-CALENDARIO fixa a altura em `mes`, e UI-PILULAS-QUEBRAM-LINHA registra que a célula não cresce. A oferta vale para qualquer número de batidas nos dois layouts (`q.md:4`, `FERIADO-BOTAO-CORTADO-2711-1120.txt:1`, `app/templates/colaboradores/partials/_calendario_grade.html:2-8`, `app/templates/colaboradores/partials/_calendario_grade.html:65-68`, `app/templates/colaboradores/partials/_calendario_grade.html:83`, `app/templates/ponto/partials/_feriado_vira_trabalho.html:6`).
  - **Acesso às marcações.** O brief delega rolagem, resumo com acesso ao detalhe ou outro desenho equivalente. A rolagem interna reaproveita as linhas existentes e mantém cada horário acessível, inclusive as retratadas riscadas e as ações das marcações. O corte é a exibição do cru da Portaria 671, registrada na L-025 (`q.md:4`, `app/docs/LEIS.md:76`, `CLAUDE.md:171`, `app/templates/colaboradores/partials/_calendario_grade.html:113-129`).
  - **Predicado, formulário e escopo.** O aval fecha a oferta existente. A L-002 exige que a tela leia a mesma autoridade do POST. Reaproveitar `details`, `summary` e os ganchos do popover mantém o formulário ligado à mesma rota e dispensa JS novo. O brief limita a mudança a template/CSS e ao teste pedido (`FERIADO-BOTAO-CORTADO-2711-1120.txt:1`, `q.md:5-6`, `app/docs/LEIS.md:60`, `app/ponto/views.py:3623-3633`, `app/templates/ponto/partials/_feriado_vira_trabalho.html:5-12`, `app/static/js/popover-acao.js:33-45`).
  - **Prova da regressão.** O brief deixa escolher entre a view real e o include com contexto real. Para o desenho acima, a asserção estrutural que morde a cópia é exigir, dentro da célula do feriado, o controle como irmão da região rolável que contém as quatro marcações. Hoje o bloco de horários e o controle seguem no fluxo normal da célula com corte, sem essa região. Cobrir `mes` e `fita`, o feriado sem batida e o dia comum sem oferta, com datas fixas. O selo de front cobre as duas cascas por render ou grafo de includes (`q.md:5`, `app/templates/colaboradores/partials/_calendario_grade.html:68`, `app/templates/colaboradores/partials/_calendario_grade.html:124-132`, `CLAUDE.md:207`).
  - **Smoke.** O aval pede o clique do Ronald no Calendário de Ponto do perfil de `[nome]`, em novembro, no dia 20/11. Conferir o controle inteiro, abrir o formulário sem enviar e acessar os quatro horários. Usar 15/11 como controle e repetir a checagem de layout na fita do diagnóstico. O smoke da obra vem do aval. A trava geral FRONT SEM SMOKE NAO SOBE cobre JS, service worker, templates-base e os partials que eles incluem (`FERIADO-BOTAO-CORTADO-2711-1120.txt:1`, `q.md:4`, `q.md:7`, `app/templates/colaboradores/detalhe.html:201-215`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33-35`, `CLAUDE.md:206-209`, `app/docs/LEIS.md:99`).

perguntas de lei: 0

## NOTAS

- **`fita`.** O código não aplica à fita o corte vertical de 132 px do mês. O `overflow-y:hidden` está no contêiner horizontal, que não declara altura fixa, e a célula da fita tem base horizontal de 78 px. O rótulo sem quebra merece conferência visual nessa largura. Corte do botão na fita em navegador ou em produção [nao verificado]. (`app/templates/colaboradores/partials/_calendario_grade.html:65-68`, `app/templates/ponto/partials/_feriado_vira_trabalho.html:5-7`).
- **Render direto encontrado pela busca.** `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:52-62` mede a fileira de ações e a pílula do chamado. `app/colaboradores/tests/test_espuria_tela_retratar_orfa.py:29-32` verifica as portas por batida. `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:39-54` verifica os horários. `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:113-121` verifica os títulos de status. `app/colaboradores/tests/test_calendario_mostra_o_carro.py:75-85` e `app/colaboradores/tests/test_calendario_mostra_o_carro.py:144-165` verificam título, chamado e Resolver dia. `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:29-50` verifica os rótulos. `app/colaboradores/tests/test_e4cal_l1_r8_rodape_le_o_nucleo.py:39-55` renderiza o rodapé com `dias=[]`. `app/core/tests/test_selo_marca_do_veredito.py:105-128` verifica as marcas de decisão. `app/ponto/tests/test_espelho_decidiu_icone.py:161-174` verifica o ícone e o título de quem decidiu.
- **Render pelas rotas.** Sete colunas abre o perfil e busca a grade (`app/colaboradores/tests/test_calendario_sete_colunas.py:138-148`). Chips usa o perfil, a fita e a Auditoria de Espelhos (`app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:171-181`, `app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:403-429`). [nome] testa `mes` e `fita`, oferta, permissão e negativos (`app/ponto/tests/test_kaua_feriado_pela_tela.py:83-86`, `app/ponto/tests/test_kaua_feriado_pela_tela.py:103-144`, `app/ponto/tests/test_kaua_feriado_pela_tela.py:249-258`). Os selos de sinais e de retratação também obtêm a grade pela view (`app/core/tests/test_selo_espelho_sinais.py:69-95`, `app/core/tests/test_smoke_retratar_1366.py:143-156`). Outros consumidores HTTP encontrados conferem contexto, resposta ou ações vizinhas (`app/colaboradores/tests/test_calendario_caracterizacao.py:13`, `app/colaboradores/tests/test_calendario_falta_futura.py:51-52`, `app/colaboradores/tests/test_calendario_badge_minutos.py:73-82`, `app/colaboradores/tests/test_calendario_cross_midnight.py:47-48`, `app/colaboradores/tests/test_drawer_caracterizacao.py:61-86`, `app/colaboradores/tests/test_get_sanitizado.py:42-47`, `app/colaboradores/tests/test_guarda_views_smoke.py:43-44`, `app/colaboradores/tests/test_guarda_views_smoke.py:60-61`, `app/core/tests/test_selo_performance.py:411-413`, `app/ponto/tests/test_porta_retratar_batida.py:238-243`, `app/ponto/tests/test_veredito_view.py:72-76`, `app/ponto/tests/test_veredito_view.py:94-105`, `app/ponto/tests/test_o137_esmeril_ausencia_le_o_cadastro.py:176-181`).
- **O que pode quebrar ao mover a ação.** [nome] compara o `action` e a sequência literal `name="dia" value="…" data-feriado-vira-trabalho`. Mudar só a posição do include preserva essas asserções, que não medem visibilidade nem posição na célula (`app/ponto/tests/test_kaua_feriado_pela_tela.py:105-144`). O selo de altura lê a fonte e exige `data-celula-dia` e a sequência inline de altura e overflow. Mover o CSS pode quebrar a regex mesmo com a geometria preservada (`app/colaboradores/tests/test_ui_grade_calendario.py:27-49`). Sete colunas depende de `[data-mes]`, mede o transbordamento e altera estilos literais para provocar a regressão. Chips mede a altura de 132 px e a contenção nos três layouts. Pílulas mede a largura dos filhos da fileira de ações. Mudanças na estrutura compartilhada podem afetar esses selos (`app/colaboradores/tests/test_calendario_sete_colunas.py:78`, `app/colaboradores/tests/test_calendario_sete_colunas.py:170-199`, `app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:240-254`, `app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:403-429`, `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:25-38`). Se o rearranjo mexer nas marcações, CAL09 exige horários no HTML, o selo de sinais exige o `summary` e dois botões no trecho das batidas, e o smoke de retratação mede os painéis abertos. O selo de popover varre os ganchos na fonte e renderiza uma grade com CSS copiado, usando o partial de Resolver dia (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:43-54`, `app/core/tests/test_selo_espelho_sinais.py:114-122`, `app/core/tests/test_smoke_retratar_1366.py:171-186`, `app/core/tests/test_selo_popover_acao.py:42-59`, `app/core/tests/test_selo_popover_acao.py:76-86`, `app/core/tests/test_selo_popover_acao.py:124-125`).
- **Demais ocorrências da busca.** São inspeções de fonte, de includes ou comentários sobre a grade (`app/pautas/tests/test_pauta_do_dia.py:65-70`, `app/relatorios/tests/test_palavra_do_dia.py:307-314`, `app/folha/tests/test_retrato_folha_quadros.py:966-975`, `app/chamados/tests/test_o261_vigia_fio.py:1067-1075`, `app/colaboradores/tests/test_relogio_riscado_calendario.py:132-150`, `app/core/tests/test_relogio_riscado_tamanho.py:104-109`, `app/ponto/tests/test_contract_espelho_sem_emoji.py:111-114`, `app/ponto/tests/test_contract_espelho_sem_emoji.py:227-230`, `app/ponto/tests/test_contract_espelho_sem_emoji.py:323-332`, `app/ponto/tests/test_glifo_saida.py:8`, `app/ponto/tests/test_o130_em_aberto_diz_o_que_falta.py:138`).
- **Limites do censo.** Na cópia, a frase “a célula não cresce” está na linha 83 (`app/templates/colaboradores/partials/_calendario_grade.html:83`). O relato de 15/11 e 20/11 vem do aval fornecido (`FERIADO-BOTAO-CORTADO-2711-1120.txt:1`). Hash Git da cópia, identificação de `[nome]` como `colNN`, reprodução visual e resultados de execução dos testes [nao verificado].

## 27/11 11:27 · sessao 79188cd4f4b4 · CASO-LITERAL-14 · leitura · origem=Code

# CASO-LITERAL-14 — fase 3

**As duas entradas diurnas reproduzem o mesmo descasamento na cópia. A sobra noturna passa por um ramo diferente da grade, mas o predicado que deixou `137193` órfã em produção continua [nao verificado].** Os 14 dias da classe **(d)** continuam **não sei**. O ensaio foi feito em memória com os horários colados, sem a ata, as flags e a janela original de produção. O hash Git da cópia está [nao verificado]. `app/escala/utils.py:638`, `app/escala/utils.py:1634`, `app/escala/utils.py:1686`, `app/ponto/turnos.py:1192`

## 1. Os três dias da classe (a)

| Dia e batida | Sítio, predicado e casamento pela regra |
|---|---|
| **446 · 20/11 · `160414`** | No ensaio, `parear_turnos` fechou `160414 → 160528` e `160559 → 160790`. A `S` das 15:52 está a 72 min 59 s de `hi`, então falha a guarda `distância à borda > 90` que permitiria juntar os períodos pela pausa deslocada. Na grade, `_turno_do_juiz_no_dia` exige **um** turno fechado. Com dois, `_match_marcos` cai no alinhamento posicional. Seu filtro `timestamp >= hi − 90 min` exclui `13:00:43`, e o ensaio deixa `15:52` ocupar a entrada. A reconciliação de pontas só recupera a entrada se o marco estiver `missing`. O casamento correto preserva `160414` como entrada real e `160790` como saída final, com `160528 → 160559` no miolo se confirmada a pausa. A L-084 mantém a comparação com o cadastro, a L-087 governa o papel, e a L-097 governa a HE da antecipação. Esse ramo na lavra de produção está [nao verificado]. `app/ponto/turnos.py:1466`, `app/escala/utils.py:1634`, `app/escala/utils.py:642`, `app/escala/utils.py:2053`, `app/docs/LEIS.md:115`, `app/docs/LEIS.md:116`, `app/docs/LEIS.md:119` |
| **1036 · 14/11 · `154907`** | O mesmo caminho reproduzido. `155066`, às 15:50:47, está a 70 min 47 s de `hi`, e o pareador fecha `154907 → 155066` e `155091 → 155363`. A grade recusa o papel de turno único e exclui `12:00:48` do envelope que começa às 13:10. No ensaio, `15:50` ocupa a entrada e impede a recuperação pelo predicado `missing`. Pela L-084 e pela L-087, `154907` é a entrada da sequência e `155363` é a saída final. A pausa candidata é `155066 → 155091`. A entrada fica a 2 h 39 min 12 s de `hi` e a saída a 24 s de `hf`. Esse ramo na lavra de produção está [nao verificado]. `app/ponto/turnos.py:1466`, `app/ponto/turnos.py:1491`, `app/escala/utils.py:1634`, `app/escala/utils.py:642`, `app/escala/utils.py:2053`, `app/docs/LEIS.md:116`, `app/docs/LEIS.md:119` |
| **989 · 25/10 · `137193`** | O pareador em modo duração reconhece `137193 → 137210` como pausa de 51 min 33 s e fecha `136992 → 137291` no dia **25/10**. O ponto a conferir é `montar_grade_prevista_periodo_por_turno`: com `hii/hfi` nulos, a chamada a `_match_marcos` passa `turno=None`. A grade cria marcos internos por flags `is_intervalo` na janela ou, no ramo de intervalo livre, pelas sobras do primeiro alinhamento que passam os filtros de posição e distância. A reconciliação final trata só das bordas externas. Com dois marcos e sem flags, o ensaio **não reproduziu a sobra exata**: `22:11` ocupou a entrada e `23:02` sobrou. Faltam `TipoEscala.intervalo_modo`, `Batida.is_intervalo`, a ata anterior e o consumo das batidas pelos dias vizinhos para identificar o predicado efetivo [nao verificado]. A L-085 atribui a cauda de 26/10 ao plantão de 25/10. A L-087 governa os papéis, e `intervalos_do_turno` lê a pausa efetiva sob a regra de intrajornada do art. 71 registrada na cópia. `app/ponto/turnos.py:385`, `app/ponto/turnos.py:431`, `app/ponto/turnos.py:502`, `app/ponto/turnos.py:537`, `app/escala/models.py:243`, `app/escala/utils.py:1686`, `app/escala/utils.py:1748`, `app/escala/utils.py:2029`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:119` |

**Dono no BACKLOG.** Para **446 e 1036**, o antecedente direto é `SAIDA-VALIDA-VIRA-ORFA`, registrada como fechada. `CONTADOR-PAREADOR-X-CELULA` registra o resíduo de borda pareada que a grade deixa órfã. A cobertura destes dois dias pela cura D2 está [nao verificado]. Para **989**, `ATA-X-TURNO-PAUSA` é o item mais próximo: trata a pausa que o juiz reconhece e a ata deixa órfã. A entrada desse caso específico, com pausa nula nos marcos do envelope, no escopo da obra está [nao verificado]. `PAR-PELO-TURNO` cobre o corte pelo teto de duração e a atribuição à véspera. `RAMO2-B` cobre o trabalhado após a âncora de saída, inclusive em turno que cruza. Os ensaios acima fecham os pares sem necessidade dessas duas mudanças. `app/docs/BACKLOG.md:1225`, `app/docs/BACKLOG.md:1646`, `app/docs/BACKLOG.md:1865`, `app/docs/BACKLOG.md:2027`, `app/docs/BACKLOG.md:2159`, `app/docs/RELATO.md:241`, `app/ponto/turnos.py:1447`

## 2. Os 14 dias da classe (d)

| Grupo | Dia | Prova que falta |
|---|---|---|
| **1 · Continuidade** | **163 · 18/11** | Na célula `140478` de 17/11, quais `ata.lampadas` acenderam e quais `dna.marcos` valiam? Entre `158092`, `158099` e `158201`, qual encerrou, integrou ou abriu jornada? Conferir o ato e o slot da pergunta `59348`. [nao verificado] `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1631`, `app/chamados/juizes.py:360` |
| **1** | **163 · 20/11** | A mesma prova na célula `140480` de 19/11, para `160127`, `160135` e `160202`, com a pergunta `60098`. [nao verificado] `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1631`, `app/chamados/juizes.py:360` |
| **1** | **334 · 10/11** | Saída apurável depois de `150585`, marcos e ata da célula `132554`, e lastro da pergunta `55996` para distinguir pausa de saída em `150563`. No ramo sem bordas, a continuidade exige uma `S` posterior dentro do envelope. [nao verificado] `app/ponto/turnos.py:393`, `app/ponto/turnos.py:413`, `app/ponto/turnos.py:2961` |
| **1** | **334 · 16/11** | Qual `S` fechou a jornada que começou em 15/11, e se `156062 → 156083` foi pausa. Separadamente, qual `S` fechou a jornada aberta por `157001`. Cruzar março da célula, papéis e perguntas `58903` e `58935`. [nao verificado] `app/ponto/turnos.py:312`, `app/ponto/turnos.py:1491`, `app/ponto/juiz_batida.py:240` |
| **1** | **334 · 17/11** | Qual de `157080`, `157102` e `157339` encerrou o trabalho iniciado por `157001`, e qual foi o fato correspondente às demais. Conferir as atas de 16 e 17 e a pergunta `58935`. [nao verificado] `app/ponto/turnos.py:293`, `app/ponto/turnos.py:1491`, `app/ponto/turnos.py:1655` |
| **1** | **446 · 01/11** | `Batida.timestamp/tipo/retratada_em` da saída após `143543` e a lâmpada de fechamento da célula `136277`. Isso distingue uma jornada que começou em `143320` e continuou após a pausa de uma sequência com outra borda ou ainda sem fechamento. [nao verificado] `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1466`, `app/ponto/juiz_batida.py:260` |
| **2 · Identidade das marcas concorrentes** | **219 · 14/11** | Fechamento e ata de 13/11, origem de `159534` e decisão sobre o par `154790/154793`. Os 70 s passam o piso geométrico de 60 s. A via `celula_absolveu` das perguntas `57827/57828` é um veredito sobre a célula. A confirmação de trabalho ou de toque indevido continua [nao verificado]. `app/ponto/turnos.py:130`, `app/ponto/turnos.py:1487`, `app/chamados/models.py:2847` |
| **2** | **446 · 25/10** | Se `136793 → 136990` foi trabalho completo, parte da jornada ou marca sem trabalho, e a quais fatos pertencem as saídas plantadas `144666/144667`. Conferir as perguntas de origem, atos, bordas efetivas e ata `136270`. [nao verificado] `app/ponto/models.py:40`, `app/chamados/juizes.py:759`, `app/ponto/juiz_batida.py:260` |
| **2** | **446 · 27/10** | O mesmo confronto para `138687 → 138784` e as saídas `144664/145038`, com a declaração associada ao chamado `36462` e os marcos da célula `136272`. [nao verificado] `app/chamados/juizes.py:360`, `app/chamados/juizes.py:802`, `app/ponto/juiz_batida.py:240` |
| **2** | **465 · 12/11** | O papel de `152798` e `152988` frente à pausa candidata `152993 → 153079`. É preciso identificar qual sequência foi trabalho antes de atribuir uma classe única às duas sobras. As luzes e os atos vinculados podem discriminar as sequências. [nao verificado] `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1606`, `app/ponto/juiz_batida.py:240` |
| **3 · Estado anterior ao ato** | **707 · 27/10** | `CelulaDia.dna_anterior`, data da regeneração, declaração `50832` e sequência dos atos de `152914`, `152915`, `152916` e `151794`. Falta a decisão sobre qual `E` iniciou a jornada e qual `S` a encerrou. `dna_anterior` guarda só a foto imediatamente anterior. [nao verificado] `app/ponto/portas/celula.py:297`, `app/chamados/models.py:4507`, `app/ponto/services/flip_batida.py:72` |
| **3** | **1010 · 04/11** | Motivo e lastro da exclusão de `146307`, e prova de que `146083` abriu trabalho em 04/11. Conferir a folga da célula `143296` e o fato do chamado `36783`. A reapresentação em memória mede a dependência geométrica. A decisão sobre a realidade do ato continua [nao verificado]. `app/ponto/services/retratar_humano.py:76`, `app/ponto/turnos.py:1920`, `app/escala/models.py:1105` |
| **3** | **1010 · 05/11** | A mesma decisão sobre `146307`, agora para dizer se `146321/146380` eram volta e saída de 04/11 ou período próprio de 05/11. Separar do plantão que começa em `147009`. [nao verificado] `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1920`, `app/ponto/turnos.py:2961` |
| **4 · Plantio com dia declarado** | **1010 · 07/11** | Para `151808`, conferir o slot e a validação de `54816`, a entrada e os marcos da célula `143298` de **06/11**, e se aquela jornada já tinha saída verdadeira. O `data_evento` identifica o dia declarado pela pergunta. A realidade da jornada e se o plantio completou ou duplicou a saída continuam [nao verificado]. `app/chamados/models.py:3095`, `app/chamados/juizes.py:360`, `app/chamados/juizes.py:802`, `app/docs/LEIS.md:117` |

**Como a prova muda a classe.** Se a mesma linha contiver batidas com explicações diferentes, a classificação única do dia continua **(d)** até resolver a divergência. O critério de dono segue a L-099 e as classes do aval fornecido. `app/docs/LEIS.md:122`

| Grupo | Resposta que leva a (a), (b) ou (c) |
|---|---|
| **1** | **(a)** A batida é borda ou pausa comprovada e o juiz a perde ou a põe no dia errado com cadastro coerente. **(b)** Há lastro explícito de toque excedente. **(c)** O trabalho se confirma e a escala autorizada diverge da célula ou do vínculo. Uma saída ainda ausente, duas `S` sem atribuição ou uma ata incompleta deixam **(d)**. `app/docs/LEIS.md:116`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:122`, `app/ponto/turnos.py:407` |
| **2** | **(a)** A prova identifica o par ou a pausa verdadeiros que o sistema descasou. **(b)** A prova identifica a marca excedente ou o plantio duplicado, com a sequência verdadeira identificada. **(c)** A jornada verdadeira foi em horário ou em dia diferente do cadastro autorizado na célula. Duração curta, aprovação e encerramento automático isolados deixam **(d)**. `app/ponto/turnos.py:130`, `app/chamados/models.py:2847`, `app/chamados/juizes.py:771`, `app/docs/LEIS.md:122` |
| **3** | **(a)** A declaração ou o histórico comprova a marca verdadeira e isola o descasamento no juiz. **(b)** A decisão sobre a própria sobra prova que foi indevida. Para `1010`, uma retratação legítima de `146307` decide somente sobre `146307`. **(c)** O DNA vigente à época ou a escala autorizada explica a divergência. Se só a reapresentação produz fechamento, sem prova do fato, fica **(d)**. `app/ponto/turnos.py:1920`, `app/ponto/services/retratar_humano.py:76`, `app/ponto/portas/celula.py:305`, `app/docs/LEIS.md:122` |
| **4** | **(a)** `151808` é a saída verdadeira de 06/11 e o juiz a deixa sobrando ou a atribui a 07/11. **(b)** A saída já foi cumprida e há prova de que `151808` é plantio indevido ou duplicado. **(c)** A jornada comprovada diverge do cadastro autorizado de 06/11. Vínculo com a pergunta e validação, por si, deixam **(d)**. `app/docs/LEIS.md:117`, `app/docs/LEIS.md:122`, `app/chamados/juizes.py:771` |

### Sonda de leitura para os quatro grupos

O bloco roda os quatro grupos em `python -B manage.py shell`, com uma transação PostgreSQL `READ ONLY`. `BAT`, `PAPEL`, `TURNO` e `PAUSAS` dão a geometria atual. `CEL`, `LUZ`, `DNA` e `GRADE` permitem comparar a ata gravada com a montagem atual. `PERG`, `ATO`, `CHAMADO` e `TRILHA` apontam o lastro por PK. A saída usa PKs, contagens, indicadores numéricos e instantes em `timezone.localtime`. Quando o fato está exclusivamente em texto livre, a sonda aponta a linha para conferência no sistema. O texto e a conclusão factual permanecem [nao verificado]. Validei a sintaxe e as 14 chaves de dia. A execução com banco está [nao verificado]. `app/config/settings/prod.py:11`, `app/ponto/services/cartorio.py:1187`, `app/ponto/turnos.py:1850`, `app/chamados/juizes.py:360`, `app/chamados/models.py:4344`

```python
from datetime import date, datetime, time, timedelta

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Q
from django.utils import timezone

from chamados.catalogo.motor import VIVOS
from chamados.juizes import _slot_esperado, dia_do_fato, pergunta_respondida
from chamados.models import ChamadoColaborador, DecisaoPergunta, PerguntaDisputa
from colaboradores.models import Colaborador
from core.models import LogAuditoria
from escala import utils
from escala.alimentacao import celulas_do_periodo
from escala.models import CelulaDia
from escala.servico_jornada import escala_vigente
from escala.services.regeneracao import janelas_de_regeneracao
from ponto.juiz_batida import periodos_do_dia
from ponto.models import Batida
from ponto.services.flip_batida import ACOES_FLIP
from ponto.services.retratar_humano import ACAO, ACAO_DESFAZER
from ponto.turnos import (
    batidas_apuraveis, intervalos_do_turno, janela_com_pad,
    papel_por_minuto_da_ata, pausas_abaixo_do_piso, turnos_do_colab,
)

# Colaborador, data, batidas de interesse, perguntas e chamados conhecidos.
CASOS = {
    1: [
        (163, "11-18", (158092, 158099, 158201), (59348,), (35571,)),
        (163, "11-20", (160127, 160135, 160202), (60098,), (35571,)),
        (334, "11-10", (150563, 150585), (55996,), ()),
        (334, "11-16", (156062, 156083, 157001, 157080), (58903, 58935), (17636,)),
        (334, "11-17", (157001, 157080, 157102, 157339), (58935,), (17636, 39479)),
        (446, "11-01", (143320, 143451, 143543), (), ()),
    ],
    2: [
        (219, "11-14", (154790, 154793, 159534), (57827, 57828), (16611, 39246)),
        (446, "10-25", (136793, 136990, 144666, 144667), (), ()),
        (446, "10-27", (138687, 138784, 144664, 145038), (), (36462,)),
        (465, "11-12", (152760, 152798, 152988, 152993, 153079), (), ()),
    ],
    3: [
        (707, "10-27", (152914, 152915, 152916, 151794), (50832,), (30747, 35226)),
        (1010, "11-04", (146083, 146307, 146321, 146380), (), (36783,)),
        (1010, "11-05", (146083, 146307, 146321, 146380), (), (36783,)),
    ],
    4: [(1010, "11-07", (151808, 148746, 149056), (54816,), (36652, 37414))],
}


def local(ts):
    if ts is None:
        return None
    if timezone.is_naive(ts):
        ts = timezone.make_aware(ts)
    return timezone.localtime(ts).isoformat()


def dia0(d):
    return timezone.make_aware(datetime.combine(d, time.min))


def pk(obj):
    return obj.pk if obj is not None else None


def inspecionar(grupo, cid, md, alvos, qids, hids, agora):
    d = date.fromisoformat("2026-" + md)
    um = timedelta(days=1)
    us = timedelta(microseconds=1)
    ini, fim = d - um, d + um
    lo, hi = dia0(ini - um), dia0(fim + 3 * um)
    pad_ini, pad_fim = janela_com_pad(ini, fim)
    c = Colaborador.objects.get(pk=cid)
    print("DIA", grupo, cid, local(dia0(d)), local(agora))

    bs = list(Batida.objects.filter(
        colaborador_id=cid, timestamp__gte=dia0(pad_ini), timestamp__lt=hi
    ).order_by("timestamp", "pk"))
    ap = list(batidas_apuraveis(c, lo, hi - us))
    ap_turno = list(batidas_apuraveis(c, dia0(pad_ini), dia0(pad_fim + um) - us))
    vivas = {b.pk for b in ap} | {b.pk for b in ap_turno}
    por_pk = {b.pk: b for b in bs}
    print("BAT_CONTAGEM", len(bs), len(ap), len(ap_turno))
    print("ALVOS_PRESENTES", sum(bid in por_pk for bid in alvos))
    for b in bs:
        print("BAT", b.pk, local(b.timestamp), int(b.tipo == "E"), int(b.tipo == "S"),
              int(b.is_intervalo), int(b.pk in vivas), b.aprovada_por_id,
              b.pergunta_origem_id, local(b.retratada_em))

    cels = celulas_do_periodo([cid], pad_ini, pad_fim)
    papeis = papel_por_minuto_da_ata(
        c, pad_ini, pad_fim, batidas=ap_turno,
        celulas={dia: cel for (_, dia), cel in cels.items()},
    )
    for papel in ("E", "S", "X", "Xi"):
        ids = [b.pk for b in ap_turno if papeis.get(
            timezone.localtime(b.timestamp).replace(tzinfo=None, second=0, microsecond=0)
        ) == papel]
        print("PAPEL_" + papel, len(ids), ids)

    for dia in (ini, d, fim):
        cel = cels.get((cid, dia))
        ec = escala_vigente(c, dia, celulas=cels)
        te = ec.tipo_escala if ec else None
        cadeia = utils.cadeia_do_dia(
            dia, ec.marcos_do_dia(dia, celulas=cels), ec.pausas_do_dia(dia, celulas=cels)
        ) if ec else []
        print("VINCULO", local(dia0(dia)), pk(cel), pk(ec), pk(te),
              len(cadeia), [local(ts) for _, ts in cadeia])
        if cel is None:
            continue
        at = periodos_do_dia(cel, te)
        print("CEL", cel.pk, cel.escala_geradora_id,
              int(cel.trabalha is True), int(cel.trabalha is False),
              int(bool(te and te._intervalo_livre())), int(at["desalinhado"]),
              len(at["orfas"]), local(cel.julgada_em),
              local(cel.regenerada_em), local(cel.editada_em))
        for i, luz in enumerate(at["papeis"], 1):
            print("LUZ", cel.pk, i, int(luz.get("acesa") is True),
                  int(luz.get("acesa") is False),
                  local(luz.get("instante_marco")), local(luz.get("instante_luz")))
        for foto, dna in enumerate((cel.dna, cel.dna_anterior), 1):
            marcos = CelulaDia.marcos_presentes((dna or {}).get("marcos"))
            seq = [(tipo, time.fromisoformat(h)) for tipo, _, h in marcos]
            print("DNA", cel.pk, foto, int(dna is not None), len(seq),
                  [local(ts) for _, ts in utils.encadear_marcos(dia, seq)])

    def mostrar_turnos(reapresentada=None):
        ts = turnos_do_colab(c, ini, fim, batida_reapresentada=reapresentada)
        print("TURNOS_CONTAGEM", pk(reapresentada), len(ts))
        for t in ts:
            print("TURNO", pk(reapresentada), local(dia0(t.data_turno)),
                  pk(t.entrada), pk(t.saida), int(t.aberto),
                  len(t.batidas), [b.pk for b in t.batidas])
            for i, pares in enumerate((intervalos_do_turno(t), pausas_abaixo_do_piso(t)), 1):
                print("PAUSAS", i, len(pares), [(local(a), local(z)) for a, z in pares])

    mostrar_turnos()
    if grupo == 3 and cid == 1010 and 146307 in por_pk and 146307 not in vivas:
        mostrar_turnos(por_pk[146307])

    ref = fim + um
    grade = utils.montar_grade_prevista_periodo(
        c, ap, dias=(fim - ini).days + 2, ref=ref, agora=agora,
    )
    for i, linha in enumerate(grade["dias"]):
        print("GRADE", local(dia0(ref - i * um)), len(linha.get("orfas") or []),
              sum(bool(x and x.get("missing")) for x in linha.get("celulas") or []))

    ces = [x.pk for x in cels.values() if ini <= x.data <= fim]
    qset = PerguntaDisputa.objects.filter(colaborador_id=cid).filter(
        Q(pk__in=qids) | Q(data_evento__range=(ini, fim)) | Q(celula_id__in=ces) |
        Q(batida_origem_id__in=alvos) | Q(batidas_plantadas__pk__in=alvos) |
        ((Q(chamado_destino_id__in=hids) | Q(disputa__chamado_id__in=hids)) &
         Q(data_evento__isnull=True))
    ).distinct().select_related("disputa", "chamado_destino").prefetch_related("decisoes")

    chamados = set(hids)
    for p in qset.order_by("pk"):
        

[truncado em 20000 de 28989 caracteres]

## 27/11 11:24 · sessao 9158a4607d0c · PAR-PELO-TURNO · construtor · origem=Code

Adicionei os cinco REDs em um único arquivo. No HEAD `4ba908393`, os cinco falharam em asserções na execução em memória, com dez falhas de subteste. O runner Django parou no import de `django.core`. Suíte integrada e certificação [nao verificado]. [test_par_c1v8_reds.py](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:77) · [manage.py:11](/home/ronald/wt-codex-par-pelo-turno-111717/app/manage.py:11)

| Item | O que o teste afirma | Por que dá RED no HEAD | [nao verificado]
|---|---|---|
| 1 | A entrada de 23/10 fica em **23/10** e, no cenário da troca de vínculo, a última saída fica em **22/10** em todos os leitores. | A janela de 21/10 deixa a primeira entrada sem a saída e ela herda 22/10. Na troca, `turnos=` muda a ordem dos turnos e a última atribuição grava 21/10 para a saída. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:81) · [turnos.py:287](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:287) · [turnos.py:3040](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:3040) · [turnos.py:3085](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:3085) | [nao verificado]
| 2 | Com e sem bordas, o par é **19:00→07:00**, com **60 min** de pausa. | Com bordas, a saída de 30 s zera `limite` na pré-passagem. O resultado são dois pares, ambos sem pausa. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:111) · [turnos.py:508](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:508) | [nao verificado]
| 3 | A casca e a folha datam as duas marcas em **21/10** e o saldo fica em **0,00 h** com ou sem elas. | A projeção da folha passa `ultima_batida=None`, perde o fato posterior da O95 e data as marcas em 20/10. Isso conta como movimento e o saldo cai para **−193,60 h**. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:124) · [fechamento.py:165](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:165) · [fechamento.py:679](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:679) · [fechamento.py:1123](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:1123) | [nao verificado]
| 4 | O par fechado de 26 h é reconhecido com vínculo noturno, diurno ou ausente em 22/10. | A guarda retorna `False` nos braços diurno e sem vínculo antes de consultar o par. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:178) · [servico_jornada.py:232](/home/ronald/wt-codex-par-pelo-turno-111717/app/escala/servico_jornada.py:232) · [servico_jornada.py:236](/home/ronald/wt-codex-par-pelo-turno-111717/app/escala/servico_jornada.py:236) | [nao verificado]
| 5 | A janela de 21/10 soma **8,00 h** com a borda antiga do PDF e com `ini−4`. | Com a carga ampla, o vão de 5 h faz o motor sobrescrever o dia do juiz com o da jornada anterior. O recorte exclui o segundo par e soma **0,00 h**. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:200) · [motor_calculo_v2.py:2521](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/motor_calculo_v2.py:2521) · [motor_calculo_v2.py:2542](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/motor_calculo_v2.py:2542) | [nao verificado]

Na fixture, usei `08:00–17:48` para o cadastro diurno e representei a marca posterior de 23/10 às 08:00 como `E`. O item 3 executa os trechos reais de projeção, movimento e saldo em memória. [test_par_c1v8_reds.py:26](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:26) · [test_par_c1v8_reds.py:135](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:135) · [test_par_c1v8_reds.py:140](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:140)

### Comandos em `app/`

Cada `red` retornou `1`. Cada `mutante` retirou em memória a condição que dispara o RED e retornou `0`. O executor reutiliza os dublês da sonda vizinha. [par_pelo_turno_c1v6_verificacao.py:59](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:59) · [test_par_c1v8_reds.py:77](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:77)

```bash
rodar() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
import importlib
import sys
import unittest
from pathlib import Path

from ponto.tests.par_pelo_turno_c1v6_verificacao import _instalar_dubles_de_infraestrutura

_instalar_dubles_de_infraestrutura()
mod = importlib.import_module('ponto.tests.test_par_c1v8_reds')
modo, item = sys.argv[1:]
mutantes = {
    '1': [('for dia in dias:', 'for dia in dias[1:]:')],
    '2': [('in (None, (dt.time(19), dt.time(7))):', 'in (None,):')],
    '3': [('for marcas in ([], [e, s]):', 'for marcas in ([],):'),
          ('self.assertEqual(saldos, [0.0, 0.0])', 'self.assertEqual(saldos, [0.0])')],
    '4': [("in ('noturno', 'diurno', 'sem vínculo'):", "in ('noturno',):")],
    '5': [("            ('ini−4', meia_noite - 4 * UM_DIA),\n", '')],
}
if modo == 'mutante':
    fonte = Path(mod.__file__).read_text()
    for antes, depois in mutantes[item]:
        assert fonte.count(antes) == 1
        fonte = fonte.replace(antes, depois, 1)
    exec(compile(fonte, mod.__file__, 'exec'), vars(mod))
testes = unittest.defaultTestLoader.loadTestsFromTestCase(mod.ParPeloTurnoC1v8RedsTest) [nao verificado]
suite = unittest.TestSuite(t for t in testes if t.id().split('.')[-1].startswith(f'test_{item}_')) [nao verificado]
assert suite.countTestCases() == 1 [nao verificado]
raise SystemExit(not unittest.TextTestRunner().run(suite).wasSuccessful()) [nao verificado]
PY
}

rodar red 1
rodar red 2
rodar red 3
rodar red 4
rodar red 5

rodar mutante 1
rodar mutante 2
rodar mutante 3
rodar mutante 4
rodar mutante 5
```

## 27/11 11:23 · sessao 80619ebac554 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Adicionei somente [test_ramo2b_motor_reds.py](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:1). No ensaio isolado, o HEAD `1f9541e71` deu **2 REDs por asserção e 2 testes de paridade verdes**. Com o motor do main `59600163c` carregado em memória, os quatro passaram. O ensaio usou adaptações mínimas para carregar o motor sem Django completo. A execução na suíte do Code fica [nao verificado]. [Bateria de testes](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:68)

| Item e teste | Valor afirmado e derivação | Resultado no HEAD | [nao verificado]
|---|---|---|
| 1 · Intermitente | **720 trabalhados, 540 noturnos de relógio, 30 fora**. O main corta às 07h. São 12h desde 19h e 9h noturnas desde 22h, com prorrogação. Main `app/ponto/motor_calculo_v2.py:1863`, `:2898`. [Asserção](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:69) | **RED.** A classe herda a cauda e devolve **750, 570, 0**. [Herança](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:3841) · [Recorte](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:1917) |
| 2 · Comercial mensal | **610 trabalhados, 130 min de HE, saldo −36,00 h**. O previsto é `720 − 60 = 660`. Com saída às 07h, o main aplica `610 − 480` para a HE e `(610 − 130 − 2640) / 60` para o saldo. Main `app/ponto/motor_calculo_v2.py:3504`, `:3526`, `:3570`. [Asserção](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:82) | **RED.** O limite sobe para 660 mesmo com saída no marco. O resultado é **0 min de HE e −33,83 h**. [Limite](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:3676) · [Saldo](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:3709) |
| 3 · Partido e Partido Noturno, caso de 490 | **490 trabalhados** (`250 + 240`), **360 noturnos de relógio** (`120 + 240`), **HE, intra e dobra zero**. O vão de 180 min foi gozado, e `490 < 540`. O abono reduz o atraso de 50 para 20 min. As saídas às 07h e 07h20 deixam 0 e 20 min fora, respectivamente. Main `app/ponto/motor_calculo_v2.py:3002`, `:3103`, `:3244`. [Asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:106) | **Verde em ambos os motores.** Com a cauda reintroduzida em memória, a saída às 07h20 sobe para **510 trabalhados** e quebra a asserção. [Recorte do Partido](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:3103) |
| 3 · Partido e Partido Noturno, HE legal e dobra | **560 trabalhados** (`320 + 240`), **380 noturnos de relógio** (`140 + 240`), **20 min de intra** (`180 − 160`). Em dia comum, a HE50 é `560 + 380/7 − (480 + 20) = 800/7` min e a dobra é zero. No feriado, a HE100 de dobra é **320 + 240 min** e a HE50 é zero. As saídas às 07h e 07h30 preservam essas rubricas. Main `app/ponto/motor_calculo_v2.py:3103`, `:3213`, `:3248`. [Asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:120) | **Verde em ambos os motores.** Os mutantes de cauda, intra, ganho noturno da HE e dobra quebram as respectivas asserções. [Campos conferidos](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:93) |

Os dois primeiros mutantes já estão presentes no HEAD. Os quatro do Partido foram aplicados em memória. Cada variante saiu com `rc 1` por asserção, com zero erros. Os comandos abaixo usam o Django do ambiente do Code, cuja execução aqui fica [nao verificado]. [Alvos](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:68) [nao verificado]

```bash
ensaio_motor() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.base python3 - "$1" <<'PY'
from contextlib import nullcontext
import sys
import unittest
from unittest.mock import patch

import django
django.setup()
from ponto import motor_calculo_v2 as motor

classe = "ponto.tests.test_ramo2b_motor_reds.Ramo2bMotorRedsTest"
intermitente = ".test_intermitente_mantem_o_recorte_no_marco"
comercial = ".test_comercial_mensal_com_saida_no_marco_mantem_limite_de_480"
partido_490 = ".test_partidos_preservam_os_490_minutos_com_abono"
partido = ".test_partidos_preservam_intra_he_legal_e_dobra"
variantes = {
    "head": (nullcontext(), [""]),
    "intermitente_cauda": (nullcontext(), [intermitente]),
    "comercial_previsto_sem_cauda": (nullcontext(), [comercial]),
    "partido_cauda": (patch.object(motor.MotorTurnoPartido, "CAUDA_DO_TURNO_QUE_CRUZA_NO_TRABALHADO", True),
                      [partido_490, partido]),
    "partido_intra": (patch.object(motor, "TOLERANCIA_INTRA_MIN", 20), [partido]),
    "partido_he": (patch.object(motor, "_ganho_noturno_da_base_da_he", return_value=0), [partido]),
    "partido_dobra": (patch.object(motor.MotorTurnoPartido, "_feriado_dobra_no_dia", return_value=False), [partido]),
}
mutacao, alvos = variantes[sys.argv[1]]
with mutacao:
    suite = unittest.defaultTestLoader.loadTestsFromNames([classe + alvo for alvo in alvos]) [nao verificado]
    resultado = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
raise SystemExit(2 if resultado.errors else int(bool(resultado.failures)))
PY
}

ensaio_motor head
ensaio_motor intermitente_cauda
ensaio_motor comercial_previsto_sem_cauda
ensaio_motor partido_cauda
ensaio_motor partido_intra
ensaio_motor partido_he
ensaio_motor partido_dobra
```

## 27/11 11:15 · sessao 50281f6e60fe · RC-SEM-PROXIMA · verificador · origem=Code

## REPROVADO

### 1. MÉDIA — R1 curado na origem para remoção ou troca após a visita, com nova regressão de leitura

`_obras_vivas` captura mtime com `fstat` e lê o texto pelo mesmo descritor. `sem_colher` extrai o `rc=` do texto já capturado. Nas sondas em memória, a c2 perdeu a saída após remoção e misturou `10:00` com `rc=7` após troca. A c3 preservou `10:00 rc=1` nos dois casos. `caso_leitura_removido` e `caso_leitura_trocado` exigem essa linha e rc `3`. Os mutantes `le-tarde-removido` e `le-tarde-trocado` reintroduzem a abertura tardia e caíram nas sondas. [bin/rc_sem_colher.py:90–100](/home/ronald/codex_ws/1e824c3f77f507fdc1a0ee18f7ac268e18d1b3d9/bin/rc_sem_colher.py:90), `bin/rc_sem_colher.py:103–116`, `bin/tests/test_rc_sem_colher.sh:152–156,179–183,212–218,349–350`.

A c3 abre **todo** `.rc` regular antes de filtrar idade e escolher o mais novo. Na sonda, um `.rc` vencido com `PermissionError` derrubou a chamada inteira, enquanto a c2 listou o órfão válido. O mesmo ocorreu com um `.rc` colhido e ilegível que a c2 listava em `SEM PROXIMA`, classe que dispensa seu conteúdo. O selo cobre remoção e troca **após** a leitura e erro de diretório. Esses dois casos de erro de arquivo ficam descobertos. `c2:bin/rc_sem_colher.py:77–91,97–106,124–138`, `bin/rc_sem_colher.py:84–100,137–152,176–187`, `bin/tests/test_rc_sem_colher.sh:179–183,212–218,257–260,297`.

### 2. MÉDIA — R2 curado na origem, com selo no juiz e lacuna no leitor

O juiz imprime uma linha final opcional a partir de `len(colher)`, `len(proxima)` e `min_idade`, usando as próprias listas que acabou de emitir. `caso_resumo` exige uma linha de órfão, três de parada, um único resumo final, `min_idade` correto e ausência de resumo na chamada sem a flag. `caso_resumo_vazio` exige os dois zeros com rc `0`. Os mutantes `resumo-trocado`, `resumo-soma`, `resumo-sem-min-idade` e `resumo-mudo-no-zero` caíram nas sondas em memória. `bin/rc_sem_colher.py:156–187`, `bin/tests/test_rc_sem_colher.sh:228–240,353–356`.

O `vercodex.diff` chama `--min-idade 900 --resumo` e incorpora ao cabeçalho a última linha `RESUMO ` tal como veio. O limite de 15 linhas e 120 caracteres vale para os detalhes. Confirmei com uma resposta simulada contendo uma linha de detalhe e resumo `7/21`. O leitor imprimiu `7/21`. Para rc `0` e `3`, imprime cabeçalho e detalhes. Para outro rc, imprime também a última linha do stderr, ou `(stderr vazio)`. Se faltar resumo, imprime `SEM RESUMO do juiz`, inclusive com rc `0` ou `3`. Esse bloco não propaga o rc do juiz. O selo do juiz continuaria verde se o vercodex perdesse as flags ou voltasse a contar linhas. `vercodex.py:165–176` no anexo `vercodex.diff`, `bin/tests/test_rc_sem_colher.sh:64,228–240,353–356`.

### 3. ALTA — R3 corrige a colisão original, mas ainda lista com 10 segundos reais em DST

A função gera um instante por dia civil, no dia do `.rc` e no seguinte, com `mktime(..., tm_isdst=-1)`. Aplica o piso inclusivo do início do minuto do `.rc` e o teto inclusivo de `agora`, e escolhe o maior instante restante. A listagem exige **mais de 900 s desde o fim** do minuto escolhido. `caso_dia_da_marca` passou de rc `3` na c2 para rc `0` vazio na c3. `caso_dia_da_marca_ontem` manteve rc `3` às `00:06:01`. Os mutantes `marca-mais-antiga` e `marca-sem-teto` caíram, respectivamente, nesses casos. `bin/rc_sem_colher.py:119–129,143–152`, `bin/tests/test_rc_sem_colher.sh:157–163,220–226,351–352`.

- **Dois dias civis com menos de 24 h reais.** Em `America/New_York`, `.rc` em `07/03/2026 23:50`, marca real em `09/03 00:10:10` e `agora` às `00:10:20`. O `.rc` tinha `84.020 s` e passou pelo corte. O juiz inferiu `08/03 00:10` e o `main` retornou rc `3` após **10 s reais**. Um `.rc` com mais de `86.400 s` reais fica fora já na passagem pela árvore. [bin/rc_sem_colher.py:77–95](/home/ronald/codex_ws/1e824c3f77f507fdc1a0ee18f7ac268e18d1b3d9/bin/rc_sem_colher.py:77), `bin/rc_sem_colher.py:119–129,146–152`.
- **Hora repetida e todas as marcas `None`.** Em processos novos com `America/New_York`, em `01/11/2026`, um `.rc` às `00:50` e uma marca às `01:10:10 -0500` deram listagem após **10 s**, porque a candidata foi `01:10 -0400`. Com `.rc` às `01:00 -0500` e a mesma marca, a primeira ocorrência caiu abaixo do piso e o dia seguinte passou do teto. A parada real de **20 min** sumiu das duas listas. É coerente deixar o prazo indeterminado quando faltam candidatas. Aqui faltou representar uma ocorrência válida, e `sem_colher` já excluiu a obra pela chave da marca. `bin/rc_sem_colher.py:106–115,119–129,141–151`.
- **Virada comum e mtime futuro.** `.rc` às `23:50` e marca às `00:10` do dia seguinte funcionam. Se a marca entra perto do fim da janela de 24 h, o `.rc` pode vencer antes de o prazo amadurecer. A colisão das `23:50` também pode ocultar uma marca realmente antiga quando a candidata de hoje acaba de entrar no teto. Já um `rc_m` futuro pode ganhar a seleção: com piso futuro recebe `None`, e no mesmo minuto ainda fica aquém do prazo. `bin/rc_sem_colher.py:77–95,108–115,119–129,146–152`, `bin/tests/test_rc_sem_colher.sh:97–104,157–163`.

### 4. BAIXA — O selo novo do resumo usa relógio de parede

O instrumento lê `time.time()` uma vez no `main` e passa o mesmo `agora` à varredura e às duas classificações. `_cenario` injeta instantes fixos. Já `caso_resumo` e `caso_resumo_vazio` usam `_arvore_proxima`, que lê `date +%s`, e chamam o juiz, que consulta o relógio novamente. O caso misto faz três chamadas em instantes distintos. `bin/rc_sem_colher.py:173–179`, `bin/tests/test_rc_sem_colher.sh:64–82,122–131,167–170,228–240`.

## Escritores, leitores e compatibilidade

| Valor | Caminho |
|---|---|
| Tupla `(ob, d, rc_m, rc_p, rc_txt, outro)` | `_obras_vivas` monta e emite. `main` materializa uma vez. `sem_colher` usa os seis campos. `sem_proxima` usa os outros cinco e descarta `_txt`. Esses são os consumidores encontrados na cópia. `bin/rc_sem_colher.py:74–115,132–142,176–179`. |
| `rc=` do `.rc` | `codex_pergunta.sh::_saida` grava o valor e publica por `mv`. `_obras_vivas` lê até 200 caracteres. `_rc_de` procura `rc=(\d+)`, depois o primeiro grupo de dígitos, ou devolve `?`. `sem_colher` entrega o valor a `main`, que imprime `rc=…`. `bin/codex_pergunta.sh:565–574`, `bin/rc_sem_colher.py:64–67,90–95,108–115,180–181`. |
| `RESUMO` e rc do processo | `main` escreve o resumo quando solicitado. `--min-idade` filtra só `SEM COLHER`. O vercodex proposto lê a linha pronta. `ESPERA_UNICA` chama sem flags, deixa passar stdout, entra no laço com rc `0`, pula com `3` e termina com sucesso para ambos. O `find` final também pode imprimir caminhos. `espera_nega` examina o comando Bash. `bin/rc_sem_colher.py:108–115,166–187`, `bin/disjuntor.py:1963–1969,2157–2174`, `vercodex.py:165–174` no anexo. |

**Saída sem `--resumo`, byte a byte:** a constante `ESPERA_UNICA` e os formatos das duas linhas ficaram iguais. A sonda mista comum deu stdout idêntico entre c2 e c3. A igualdade geral é falsa: no caso das `23:50`, a c2 emitiu `SEM PROXIMA` e pulou o laço, enquanto a c3 teve stdout vazio no juiz e entrou no laço. Confirmei esse desvio com o Bash da espera e dependências simuladas. `bin/disjuntor.py:1967–1969`, `c2:bin/rc_sem_colher.py:164–169`, `bin/rc_sem_colher.py:180–187`, `bin/tests/test_rc_sem_colher.sh:157–163,220–226`.

**Esmeril final.** Contagem de `^\+.*except`: **0** no `diff.patch` e **0** no `vercodex.diff`. O `except Exception` é contexto. A classificação e as contas ficam no juiz. Dentro dele, o parser puro roda duas vezes sobre o mesmo texto e o predicado de folga aparece nas duas listas. As sondas foram em memória e `bash -n` passou. RED/GREEN integrais, `test_disjuntor.sh`, Ruff, instalação do vercodex e ocorrência desses cenários em produção ficam [nao verificado]. `bin/rc_sem_colher.py:48–61,106–115,135–150,173–187`, `bin/tests/test_rc_sem_colher.sh:35–37,297–314`, `vercodex.py:167–176` no anexo.

## 27/11 11:11 · sessao c71da07a488e · E4C-F5-INSTANTE · verificador · origem=Code

## APROVADO — ver3 do E4C-F5-INSTANTE

A volta cura P3 na origem e fecha a lacuna de trava na auditoria pós-rollback. O teste multiempresa confirma a regra de estoque já presente na cura2. O item 6 continua fora desta volta. [materializacao.py:846](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:846) · [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [test_e4c_f5_c4_red.py:932](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:932) · [test_e4c_f5_c4_red.py:634](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:634)

### Procedentes do ver2

1. **MÉDIA · P3 curado na origem.** Os dois caminhos de veto passam `unica=True` pelo ponto comum `trilha_do_veto_k6`. O escritor procura uma linha com a mesma identidade e só insere se ela faltar. O RED original exige estado preservado e uma linha após duas validações. O RED fora de request repete o mesmo usuário e comando e exige uma linha, além de exigir uma linha distinta para outro comando. **M44** desliga a opção no chamador e **M46** a ignora no escritor. Ambos atingem a contagem do segundo RED. A cura evita novas duplicatas e reaproveita uma linha se já houver histórico igual. [materializacao.py:846](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:846) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138) · [test_e4c_f5_c4_red.py:1055](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1055) · [e4c_f5_c4red2_mutantes.py:757](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:757)
2. **MÉDIA · Pós-rollback curado na origem.** O novo `atomic` toma a mesma trava do dono pela porta existente e relê a pergunta antes de auditar. O RED força o veto tardio em duas tentativas, exige uma linha, confere a foto após o rollback e exige o dono registrado nas duas recusas. **M45** remove a aquisição. A mordida vem da asserção do sentinela, mesmo que a contagem ainda dê um em execução serial. O teste retira previamente os sentinelas da fixture e simula o momento da leitura do K6. Uma disputa real entre conexões nesse ramo fica **[nao verificado]**. [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [test_e4c_f5_c4_red.py:1087](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1087) · [test_f5_v3_guarda_sob_trava.py:28](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_f5_v3_guarda_sob_trava.py:28) · [e4c_f5_c4red2_mutantes.py:762](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:762)
3. **BAIXA · Multiempresa é controle.** A ordem do teste é Empresa A → dono A → Empresa B → dono B. A regra herdada já recusa B por vir depois do **primeiro** dono do lote. O teste exige a mensagem específica e as duas fotos intactas. **M40** continua ligado ao RED de uma Empresa e um dono. O controle multiempresa veio sem mutante próprio. [test_e4c_f5_c4_red.py:932](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:932) · [export.py:602](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/folha/export.py:602) · [e4c_f5_c4red2_mutantes.py:724](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:724)
4. **BAIXA · M16 conserva a mordida.** Cada passagem de materialização e passivo ganha seu savepoint revertido. As asserções ainda exigem uma trilha nova com fotos e motivo para cada metade. M16 apaga fotos e descrição e derruba a asserção de conteúdo nos quatro subcasos. O teste sem banco já substitui a função inteira de trilha e continua provando o encaminhamento de uma tentativa. [test_e4c_f5_c4_red.py:1017](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1017) · [test_e4c_f5_c4_red.py:1170](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1170) · [e4c_f5_c4red2_mutantes.py:254](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:254) · [e4c_f5_c4red2_mutantes.py:475](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:475) · [test_bug_par_sem_banco.py:153](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_bug_par_sem_banco.py:153)

### Campos de `evento(unica=True)`

`evento` escreve todos os campos pelo mesmo `create`. A consulta de duplicata lê os oito campos de uma cópia de `registro`. A tabela aponta quem fornece cada valor e onde a trilha é lida no recorte. [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138)

| Campo | Quem fornece | Leitor da trilha |
|---|---|---|
| `acao` | `trilha_do_veto_k6` fornece `veto_competencia_do_fato`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | RED seleciona a ação. [test_e4c_f5_c4_red.py:1065](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1065) |
| `modelo` | A trilha fornece `PerguntaDisputa`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_trilha` seleciona o modelo. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) |
| `objeto_id` | A trilha passa `p.pk`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_trilha` seleciona o PK. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) |
| `descricao` | A guarda produz o motivo e a trilha passa `texto[:500]`. [materializacao.py:826](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:826) · [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_ver_trilha` verifica o motivo e a tela o exibe. [test_e4c_f5_c4_red.py:1025](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1025) · [log_auditoria.html:73](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/templates/core/log_auditoria.html:73) |
| `valor_antes` | A trilha monta a foto do texto e dos três instantes. [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854) | RED pós-rollback compara a foto. [test_e4c_f5_c4_red.py:1119](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1119) |
| `valor_depois` | A trilha passa `dict(foto)`. [materializacao.py:859](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:859) | O mesmo RED exige igualdade com a foto anterior. [test_e4c_f5_c4_red.py:1119](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1119) |
| `usuario` | `evento` usa o autor explícito com PK ou, se o argumento `ator` for `None`, o usuário autenticado da request. Sem autor resolvido com PK, grava nulo. [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) | `_trilha` seleciona `self.admin` e a tela filtra pela FK. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) · [views.py:705](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/views.py:705) |
| `ator` | `evento` grava `sistema:<comando>` quando reconhece o processo fora de request, inclusive com usuário explícito. Em request ou sem comando reconhecido, o campo fica nulo. [observ.py:22](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:22) · [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) | O RED distingue os dois comandos pelo campo `ator`. [test_e4c_f5_c4_red.py:1081](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1081) |

**Mesma autoria no filtro e no `create`, sim.** `usuario` e `ator` são resolvidos uma vez. Deixar `ip` fora da identidade é coerente com o veto por pergunta, motivo, fotos e autoria. Uma repetição por outro IP reutiliza a linha encontrada e conserva o IP dela. `unica` é opt-in e a trilha K6 é o único chamador que o ativa na cópia. Os demais, inclusive `evento_kw` e `registrar_log`, seguem direto ao `create` com os valores resolvidos. [observ.py:94](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:94) · [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138) · [observ.py:157](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:157) · [auditoria.py:22](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/auditoria.py:22)

**JSONField.** No Django fixado pela cópia, `create` com `None` simples numa `JSONField(null=True)` grava **SQL NULL**. `campo__isnull=True` casa esse valor. Um `null` dentro do dicionário continua sendo parte do JSON comparado por igualdade. A trilha K6 sempre passa dois dicionários, então seus REDs deixam o ramo de `None` no campo inteiro sem exercício. SQL efetivamente emitido **[nao verificado]**. [requirements.txt:13](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/requirements.txt:13) · [models.py:300](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/models.py:300) · [observ.py:143](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:143) · [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854)

### Travas em todos os caminhos da trilha

- **`validacao.py:242`, antes do carimbo.** A recusa de `:797` roda dentro do `atomic` que já chamou `travar_o_ato_do_plantio`. Essa função toma o dono pela `trava_na_ordem`. [validacao.py:242](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:242) · [validacao.py:755](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:755) · [validacao.py:795](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:795) · [materializacao.py:2223](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2223)
- **`validacao.py:242`, depois do rollback.** O rollback elimina a aquisição e o sentinela feitos naquele bloco. Uma trava já detida pelo `atomic` externo sobrevive, como pode ocorrer na validação em lote. O novo bloco usa a porta reentrante e relê a pergunta. Há precedente de `trava_na_ordem(colab_id)` só para proteger consulta de identidade e escrita em `lavrar`. [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [trava_do_colaborador.py:16](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/trava_do_colaborador.py:16) · [validacao.py:1172](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:1172) · [par_relampago_na_lista.py:116](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/ponto/services/par_relampago_na_lista.py:116)
- **`materializacao.py:2004`.** O laço da disputa toma a trava antes da unidade. O passivo `reparar` toma a trava antes de `_trabalhar_o_par` → `_plantar_o_dia` → `_materializar_uma`. A validação chega à mesma unidade pela tentativa do par ou pela metade avulsa sob a trava de `:758`. A chamada recursiva para a irmã ocorre dentro do ato da principal, sob o mesmo dono. O RED do passivo chama `_plantar_o_dia` diretamente e prova conteúdo da trilha, enquanto a aquisição da trava no caminho de produção se verifica em `reparar`. [materializacao.py:2004](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2004) · [materializacao.py:2301](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2301) · [passivo_validado.py:393](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:393) · [passivo_validado.py:491](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:491) · [passivo_validado.py:585](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:585) · [validacao.py:687](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:687) · [validacao.py:826](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:826) · [materializacao.py:1841](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:1841) · [test_e4c_f5_c4_red.py:1182](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1182)

**Empresa antes do dono.** Nenhuma das duas entradas de `_recusa_da_entrega` exige a trava da Empresa. A primeira já tem o dono e chega com a decisão da guarda. A segunda toma só o dono e reutiliza o veto obtido antes do rollback. A exigência explícita de Empresa antes do dono pertence ao registro de **estoque**, que confere a ordem antes do K6. Assim, nos caminhos da recusa, a ausência da trava da Empresa é compatível com o contrato da trilha. A trava do dono serializa consulta e inserção da auditoria. O motivo do veto tardio continua sendo o da tentativa anterior ao rollback, enquanto a foto vem da releitura posterior. [validacao.py:237](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:237) · [validacao.py:795](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:795) · [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [models.py:4873](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4873) · [models.py:4892](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4892)

### Esmeril

5. **BAIXA · Semântica e autoria.** O parâmetro `ator` recebe o autor humano, enquanto a coluna `ator` identifica o processo. Essa diferença é herdada e o filtro usa as duas colunas já resolvidas. Também seguem herdados `data_evento` como dia do turno ou dia civil da conversa e `entrada_em` como instante de inserção do ato ou marco convencional do estoque. [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) · [models.py:3086](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3086) · [models.py:3097](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3097) · [models.py:4402](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4402)
6. **BAIXA · Alcance dos selos.** M44 e M46 provam que a dedupe é usada. O RED fora de request varia o comando e mantém sempre o mesmo usuário, portanto a retirada isolada de `usuario` do filtro escaparia desse teste. M45 prova a presença da trava pelo registro transacional. M20 continua apontando para o RED já vermelho do item 6, onde o `post_save` e `DecisaoPergunta.registrar` chegam ao mesmo emissor. [test_e4c_f5_c4_red.py:1068](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1068) · [test_e4c_f5_c4_red.py:1106](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1106) · [e4c_f5_c4red2_mutantes.py:489](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:489) · [signals.py:253](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/signals.py:253) · [models.py:4791](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4791)
7. **BAIXA · Relógio, derivação, escritor e `except`.** O delta usa os instantes da pergunta na foto e o `criado_em` já definido no modelo. Os testes novos usam o congelamento da classe. Segue herdado `carimbar` chamar `pergunta_viva(self)` sem passar o relógio que resolveu, com possibilidade de `localdate()` no degrau da janela. K6 segue no juiz registrado. A identidade da trilha deriva do mesmo `registro` que alimenta o único `create`. No diff fornecido, **`^\+.*except` = 0**. A captura existente continua em `engolir`. [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854) · [models.py:307](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/models.py:307) · [test_e4c_f5_c4_red.py:68](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:68) · [models.py:3460](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3460) · [models.py:3496](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3496) · [juizes.py:642](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/juizes.py:642) · [juizes.py:350](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/juizes.py:350) · [observ.py:85](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:85) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138)

O veredito é técnico, sobre o delta fornecido. A alteração em `core/observ.py` extrapola a lista dos briefs 3 e 3b anexados. O aval específico da 3c, a reprodução independente dos números da pista e a ocorrência em produção ficam **[nao verificado]**. [observ.py:94](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:94)

_8650 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
