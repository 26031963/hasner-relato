# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 01:45** · publicado em **19/11/2026 01:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 01:45 · sessao 778b831b7b28 · - · - · origem=Ronald (tmux)

A análise das ocorrências de `materializacao.py`, incluindo as classificações, os trechos e os chamadores comprovados, foi enviada ao agente responsável pela consolidação. A leitura da cópia foi apenas em memória; não houve escrita em arquivos. [não verificado]

## 19/11 01:45 · sessao 3b84bf5df64f · - · - · origem=Ronald (tmux)

Classificação do subconjunto de comandos, migração e serviços enviada ao agente principal: **35 linhas**, todas com receptor `PerguntaDisputa`; classes **(ii) 9, (iv) 3, (v) 2 e (vi) 21**. As evidências e os trechos foram enviados junto à classificação. ([backfill_decisoes_pergunta.py:44](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/backfill_decisoes_pergunta.py:44), [encerrar_perguntas_com_fato.py:27](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/encerrar_perguntas_com_fato.py:27), [relavrar_via_i6.py:44](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/relavrar_via_i6.py:44), [0053_pergunta_unica_por_familia.py:31](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/migrations/0053_pergunta_unica_por_familia.py:31), [reconciliar_fantasmas.py:210](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/management/commands/reconciliar_fantasmas.py:210), [flip_auto.py:116](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/flip_auto.py:116))

## 19/11 01:45 · sessao 85eb35a46c3f · - · - · origem=Ronald (tmux)

Resultados e ressalvas enviados ao agente principal, com referências ao código e aos chamadores.

## 19/11 01:45 · sessao c61a08d6dac7 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

**BLOQUEIA — o selo morde os mutantes declarados, mas não garante a ausência de leituras de `DecisaoPergunta`.** A busca percorre apenas o AST do corpo de `marcos_do_lote` e reconhece um conjunto específico de nomes, atributos, palavras-chave e strings; ela não acompanha chamadas nem resolve todas as formas de acesso ao modelo. [app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:14](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:14)

**1. Mordida e lacunas**

Os testes M1–M4 apresentam construções que o detector reconhece: nome ou import direto, apelido direto, atributo `DecisaoPergunta` e string literal contendo esse nome. M5, M6, função aninhada e lookup `decisoes__...` também têm padrões reconhecidos. Isso é uma conclusão por inspeção; execução dos testes e logs RED/controle: **[não verificado]**. [app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:88](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:88)

As formas abaixo escapam. A lista descreve classes de acesso, não uma enumeração finita de todos os programas Python possíveis. Em cada linha, a cobertura indicada é trabalho necessário, não uma proteção existente verificada.

| Forma que escaparia | Alcançável no corpo atual? | Quem cobriria |
|---|---|---|
| Chamar helper do módulo ou importado que faz a leitura; o detector não entra no corpo do helper. | Não há chamada a helper próprio com essa leitura no corpo visível; comportamento interno de bibliotecas chamadas: **[não verificado]**. | Selo com análise transitiva dos chamados ou contrato separado que vigie o helper. [teste:39](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:39), [produção:151](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:151) |
| Importar no módulo outro símbolo que já represente o modelo, por exemplo reexportação `from outro_modulo import DP`, e usar `DP.objects`; só o import direto de `chamados.models` registra apelido. O conjunto `modulos` é preenchido, mas não é consultado. | Esse import e uso não aparecem hoje no corpo ou imports mostrados. | Resolução de símbolos e reexportações. [teste:24](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:24), [produção:88](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:88) |
| `getattr` ou `get_model` com nome montado, por exemplo `'Decisao' + 'Pergunta'`; aliases obtidos por atribuição ou acesso dinâmico também podem escapar. | Não há essa construção no corpo atual. | Análise de fluxo e avaliação conservadora de strings; acesso dinâmico arbitrário exige política explícita. [teste:46](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:46), [produção:151](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:151) |
| `prefetch_related('decisoes')`, lookup passado como string ou via `**dict`, e consultas pelo outro relacionamento `absorcoes_feitas`. A string `decisoes` não contém `DecisaoPergunta`, e esse segundo relacionamento não consta nos atributos procurados. | Nenhuma dessas construções aparece no corpo atual. | Detector de caminhos ORM em strings e argumentos dinâmicos, considerando os relacionamentos reais do modelo. [teste:52](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:52), [modelo:4176](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4176) |
| `Subquery`/`OuterRef` ou consulta sobre outro modelo que alcance a tabela por caminho de relacionamento em string; SQL bruto ou abstração que consulte a tabela também pode escapar. | Não aparecem essas construções no corpo atual. A execução interna do ORM: **[não verificado]**. | Análise dos caminhos ORM e SQL, ou restrição verificável às formas permitidas de consulta. [produção:177](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:177), [teste:59](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:59) |

`select_related('decisoes')` não é, por si, um exemplo comprovado de leitura válida desse relacionamento reverso de múltiplas linhas; `prefetch_related('decisoes')` é a forma relevante aqui. Validação em execução: **[não verificado]**. [modelo:4166](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4166)

**2. Valores assumidos pelo selo**

- **NÃO BLOQUEIA — relacionamento:** `decisoes` confere: é o `related_name` de `DecisaoPergunta.pergunta`, FK para `PerguntaDisputa`. O modelo também declara `absorcoes_feitas` para outra FK à pergunta, não vigiada pelo selo. [app/chamados/models.py:4166](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4166), [app/chamados/models.py:4176](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4176)
- **NÃO BLOQUEIA — escrita e leitura:** o modelo declara `registrar` como porta de escrita; há chamada concreta no backfill e leitura concreta por `self.decisoes.all()` em `PerguntaDisputa`. Censo exaustivo de todos os escritores e leitores: **[não verificado]**. [app/chamados/models.py:4141](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4141), [backfill_decisoes_pergunta.py:351](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/management/commands/backfill_decisoes_pergunta.py:351), [app/chamados/models.py:3801](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:3801)
- **NÃO BLOQUEIA — nome e caminho:** o teste forma o caminho para `chamados/veredito_lavrado.py` e procura exatamente uma função de topo chamada `marcos_do_lote`; ambos conferem nesta cópia. O nome e o caminho são declarações de código, não valores escritos em banco. [teste:8](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:8), [teste:17](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:17), [produção:151](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:151)

**3. Classes de reprovação**

- **BLOQUEIA — selo/RED com mordida parcial:** os mutantes exercitam padrões que o detector reconhece, mas as leituras indiretas e por caminhos ORM acima permanecem sem cobertura demonstrada. É o mesmo bloqueio contado na seção 1. [teste:46](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:46)
- **NÃO BLOQUEIA — demais classes na obra apresentada:** o diff fornecido contém apenas um arquivo de teste; nele não identifiquei novo juiz de domínio, escritor de produção, leitura de relógio, derivação paralela ou campo de produção com dois significados. A ausência dessas classes no restante do sistema é **[não verificada]**. [teste:1](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:1)

**4. Dependências e falsos verdes**

- **NÃO BLOQUEIA — relógio, banco e ordem:** por inspeção, os testes leem o arquivo, fazem parse e analisam strings em memória; não invocam `marcos_do_lote`, banco nem relógio. Cada mutante parte de `self.fonte`, sem depender de alteração feita por teste anterior. Execução da suíte: **[não verificado]**. [teste:75](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:75)
- **NÃO BLOQUEIA — vacuidade e âncoras nesta cópia:** erro de parse levanta exceção; função ausente ou duplicada também. As três âncoras ocorrem uma vez, e a âncora de corpo está dentro do alvo. A lista vazia pode, contudo, ser falso verde para as formas de leitura que o detector não reconhece, já contadas no bloqueio. [teste:16](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:16), [teste:70](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:70), [produção:160](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:160)

A cópia acessível não possui metadados Git no diretório consultado; identidade do HEAD com o hash informado: **[não verificado]**.

BLOQUEIA: 1  
perguntas de lei: 0

## 19/11 01:44 · sessao 0f3fd24bbc1d · CADEIA-20 · sonda · origem=Code

**1. Marcador da pausa.** A informação que distingue os caminhos existe nas condições do pareador; o resultado comum `_intra_dur=True` não registra qual delas foi satisfeita.

- No caminho **S→E**, a próxima batida é lida como `tn` e o intervalo em segundos como `gap`: `tn = (getattr(nx, 'tipo', '') or '').upper()[:1]` e `gap = (nx.timestamp - b.timestamp).total_seconds()` ([turnos.py:433](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:433)). A distinção existe na condição `tn == 'E' and 0 <= gap <= intra_max_s` ([turnos.py:453](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:453)); a origem deixa de ser distinguível pelo marcador quando ambas as pontas recebem `b._intra_dur = True` e `nx._intra_dur = True` ([turnos.py:456](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:456)). Há outro caminho S→E sem bordas, com condição de S posterior e o mesmo marcador ([turnos.py:407](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:407)), além do caminho com marcos em [turnos.py:1468](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1468).
- No caminho **S,S**, a informação existe em `if tn == 'S' and (ds is None or ds > 90):` ([turnos.py:476](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:476)). Ao encontrar a S posterior próxima da borda, a primeira recebe o mesmo `b._intra_dur = True` ([turnos.py:484](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:484)).
- O laço também usa esse atributo para papel vindo da ata e para absorção por marco: `b._intra_dur = True` ([turnos.py:1316](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1316), [turnos.py:1364](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1364)). O consumo reúne as origens em `_iv = getattr(b, "is_intervalo", False) or getattr(b, "_intra_dur", False)` ([turnos.py:1326](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1326)).

A batida gravada tem `tipo` e `is_intervalo` ([models.py:23](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/models.py:23)); a célula grava `ata` em JSON ([models.py:1774](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/models.py:1774)). O período do motor expõe `intervalos_do_motor`, isto é, intervalos já julgados, e o adaptador os entrega como pares de instantes ([motor_calculo_v2.py:239](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/motor_calculo_v2.py:239), [adaptador_motor.py:13](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/adaptador_motor.py:13)). **Não foi verificado um campo gravado que registre qual daqueles dois ramos do pareador produziu `_intra_dur`**; os tipos e dados da ata podem conter fatos relacionados, mas não provam por si o caminho executado. [não verificado]

**2. ATA_DESALINHADA.** No anexo `sonda3.py`, o ramo é literalmente `if resultado.get("desalinhado"):` seguido de `nao_medida[(emp, "ATA_DESALINHADA")] += 1` e `continue`; o número da linha do anexo está [não verificado]. A condição vem de `periodos_do_dia(cel, None)` e conta uma ocorrência por célula cuja ata é um `dict` e cujo resultado tem `desalinhado` verdadeiro. [não verificado: linha do anexo]

O motor retorna esse valor em quatro causas exclusivas pela precedência do código: sem marcos esperados após a tentativa posicional; sem lâmpadas, se há marcos; quantidade diferente, se ambos existem; ou discordância de horário de um marco, após passar pelas guardas anteriores ([juiz_batida.py:190](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:190), [juiz_batida.py:195](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:195), [juiz_batida.py:227](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:227)). Os contadores por causa e exemplos ainda são [não verificado].

A sonda abaixo usa o mesmo recorte e guarda do anexo, classifica pelo `porque` devolvido pelo próprio motor e compara a soma por causa com o universo contado diretamente pelo ramo. Cada exemplo é uma célula; a hora é a primeira batida apurável **na data local da célula**, quando existir. A ausência de batida é explicitada. A execução e seus resultados são [não verificado].

```python
# Executar pelo mesmo envelope de leitura da sombra, tenant juliani.
# Não cria, altera ou apaga arquivos. Não imprime nome nem CPF.

import collections
import datetime
import os
import sys

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from escala.models import CelulaDia
from ponto.juiz_batida import periodos_do_dia
from ponto.turnos import batidas_apuraveis


INI = datetime.date(2026, 10, 21)
FIM = datetime.date(2026, 11, 20)
UM_DIA = datetime.timedelta(days=1)

CAUSAS = (
    "SEM_MARCOS",
    "SEM_LAMPADAS",
    "QUANTIDADE_DIFERENTE",
    "HORARIO_DIVERGENTE",
)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def hora_valida(hora):
    if not isinstance(hora, str) or len(hora) != 5 or hora[2] != ":":
        return False
    try:
        return datetime.datetime.strptime(hora, "%H:%M").strftime("%H:%M") == hora
    except ValueError:
        return False


def causa_do_resultado(resultado):
    porque = resultado.get("porque")
    if porque == "sem marcos no dna":
        return "SEM_MARCOS"
    if porque == "sem lampadas na ata":
        return "SEM_LAMPADAS"
    if isinstance(porque, str) and porque.startswith("ata com "):
        return "QUANTIDADE_DIFERENTE"
    if isinstance(porque, str) and porque.startswith("marco "):
        return "HORARIO_DIVERGENTE"
    raise RuntimeError("causa_desconhecida")


def main():
    etapa = "GUARDA_SOMBRA"
    try:
        with transaction.atomic():
            guarda_sombra()

            etapa = "CELULAS"
            celulas = list(
                CelulaDia.objects.filter(data__gte=INI, data__lte=FIM)
                .select_related("colaborador", "colaborador__empresa")
                .order_by("colaborador_id", "data")
            )
            por_pessoa = collections.defaultdict(list)
            empresas = set()
            for cel in celulas:
                por_pessoa[cel.colaborador_id].append(cel)
                empresas.add(cel.colaborador.empresa_id)

            universo = collections.Counter()
            contagem = collections.Counter()
            # Primeiro guarda exemplos com hora; depois completa com células
            # sem batida no dia, se necessário.
            com_hora = collections.defaultdict(list)
            sem_hora = collections.defaultdict(list)

            for cid, cels in sorted(por_pessoa.items()):
                colab = cels[0].colaborador
                emp = colab.empresa_id
                classificadas = []

                for cel in cels:
                    etapa = "ATA"
                    if not isinstance(cel.ata, dict):
                        continue

                    orfas = cel.ata.get("orfas")
                    if orfas is not None and not isinstance(orfas, list):
                        raise RuntimeError("orfas_formato")
                    if orfas is not None:
                        for hora in orfas:
                            if not hora_valida(hora):
                                raise RuntimeError("hora_orfa")

                    etapa = "INVERSA_ATA"
                    resultado = periodos_do_dia(cel, None)
                    if not resultado.get("desalinhado"):
                        continue

                    # Mesmo ponto de contagem da sonda3, antes da partição.
                    universo[emp] += 1
                    causa = causa_do_resultado(resultado)
                    contagem[(emp, causa)] += 1
                    classificadas.append((cel.data, causa))

                if not classificadas:
                    continue

                # Só procura horas enquanto ainda faltam exemplos com hora
                # para alguma causa desta pessoa.
                precisa_hora = any(
                    len(com_hora[causa]) < 2
                    for _, causa in classificadas
                )
                por_dia = collections.defaultdict(list)
                if precisa_hora:
                    etapa = "BATIDAS_EXEMPLOS"
                    lo = timezone.make_aware(
                        datetime.datetime.combine(INI, datetime.time.min)
                    )
                    hi = timezone.make_aware(
                        datetime.datetime.combine(FIM + UM_DIA, datetime.time.min)
                    )
                    for batida in batidas_apuraveis(colab, lo, hi):
                        local = timezone.localtime(batida.timestamp)
                        if INI <= local.date() <= FIM:
                            por_dia[local.date()].append(local)

                for dia, causa in classificadas:
                    if len(com_hora[causa]) >= 2:
                        continue
                    instantes = por_dia.get(dia, [])
                    if instantes:
                        hora = min(instantes).strftime("%H:%M")
                        com_hora[causa].append((cid, dia, hora))
                    elif len(sem_hora[causa]) < 2:
                        sem_hora[causa].append((cid, dia, None))

            etapa = "SAIDA"
            linhas = []
            soma_geral = 0
            universo_geral = 0

            for emp in sorted(empresas, key=lambda valor: str(valor)):
                soma = sum(contagem[(emp, causa)] for causa in CAUSAS)
                total = universo[emp]
                soma_geral += soma
                universo_geral += total
                for causa in CAUSAS:
                    linhas.append(
                        "CAUSA emp=%s causa=%s ocorrencias=%d"
                        % (emp, causa, contagem[(emp, causa)])
                    )
                linhas.append(
                    "CONFERENCIA emp=%s contador=%d universo=%d contador==universo=%s"
                    % (emp, soma, total, "s" if soma == total else "n")
                )
                if soma != total:
                    raise RuntimeError("soma_empresa")

            linhas.append(
                "CONFERENCIA_TOTAL contador=%d universo=%d contador==universo=%s"
                % (
                    soma_geral,
                    universo_geral,
                    "s" if soma_geral == universo_geral else "n",
                )
            )
            if soma_geral != universo_geral:
                raise RuntimeError("soma_total")

            # Referência da saída fornecida; pode divergir se a sombra mudou.
            linhas.append(
                "REFERENCIA_SONDA3 universo=6454 atual=%d confere=%s"
                % (universo_geral, "s" if universo_geral == 6454 else "n")
            )

            for causa in CAUSAS:
                exemplos = (com_hora[causa] + sem_hora[causa])[:2]
                for numero, (cid, dia, hora) in enumerate(exemplos, 1):
                    linhas.append(
                        "EXEMPLO causa=%s numero=%d col%d dia_celula=%s hora_local=%s"
                        % (
                            causa,
                            numero,
                            cid,
                            dia.strftime("%d/%m/%Y"),
                            hora if hora is not None else "sem_batida_no_dia",
                        )
                    )
                for numero in range(len(exemplos) + 1, 3):
                    linhas.append(
                        "EXEMPLO causa=%s numero=%d indisponivel"
                        % (causa, numero)
                    )

        for linha in linhas:
            print(linha)
        print("ERROS=0")
        print("FIM_SONDA_PASSO2")
        return 0

    except Exception:
        print("ERRO " + etapa)
        print("ERROS=1")
        print("FIM_SONDA_PASSO2")
        return 1


sys.exit(main())
```

perguntas de lei: 0

## 19/11 01:44 · sessao 7975ea5f169d · QUEIMA-B4 · sonda · origem=Code

**A. Código e limite da identificação**

O caminho histórico de entrada das 14 continua **[não verificado]**. A classe atual informada para elas é `b_conversa`; isso, por si só, não identifica a porta que atuou no passado. [não verificado: saída histórica independente não fornecida; app/chamados/services/validacao.py:1067]

Na cópia, as portas abaixo compartilham o materializador. A assinatura permite reconhecer **compatibilidade com um ramo**, mas não distinguir todas as portas que o chamaram:

| Caminho | Assinatura e trilha gravada |
|---|---|
| Resposta `hora`: `_materializar_resposta_hora` | Ao criar: `origem='disputa_s84_retro'`, `aprovada=True`, `aprovada_por=request_user`, `aprovada_em=now()`; depois vincula `pergunta_origem` e tenta log `materializar_batida`, com `batida_pk` e `pergunta_pk`. Não informa justificativa à criação. [app/chamados/services/materializacao.py:1487] [app/chamados/services/materializacao.py:1542] |
| Resposta `data_hora`: `_materializar_resposta_data_hora` | `origem='disputa_ancora'`, tipo `E`, `sequencia=1`, justificativa sobre âncora, `aprovada=True`, `aprovada_por=request_user`, `aprovada_em=now()`; tenta log `materializar_ancora`, com `batida_pk` e `pergunta_pk`. Esse trecho não atribui `pergunta_origem`. [app/chamados/services/materializacao.py:1604] [app/chamados/services/materializacao.py:1637] |
| Validação individual e par | A validação grava `validada_em`, `validada_por` e decisão `ato='validacao'`; para hora, chama `_materializar_uma` ou o caminho do par, e depois a fila da disputa. A assinatura da batida é a do ramo acima; não há, nesses campos, marca exclusiva de validação individual. [app/chamados/services/validacao.py:161] [app/chamados/services/validacao.py:769] |
| Lote da tela/chat | Reclassifica cada pergunta e chama `acao_validar_pergunta` para classe A. Compartilha a trilha da validação; uma assinatura exclusiva do lote **[não verificado]**. [app/chamados/services/validacao.py:1027] [app/chamados/services/validacao.py:1067] |
| Comando `validar_classe_a` | Seleciona pela classificação usada pelo comando e chama `validar_pergunta(q.pk, autor, disputa=d)`. Compartilha a trilha de validação; assinatura exclusiva do comando **[não verificado]**. [app/chamados/management/commands/validar_classe_a.py:60] [app/chamados/management/commands/validar_classe_a.py:97] |
| Regularização da disputa, inclusive pelo fio | Ambas chamam `materializar_perguntas_validadas_da_disputa` com o usuário da requisição; a primeira registra também `regularizar_disputa_granular` na disputa. Compartilham a assinatura do ramo materializado. [app/chamados/services/acoes_disputa.py:121] [app/chamados/services/acoes_disputa.py:143] [app/chamados/services/fio.py:711] |
| Passivo validado e comando `materializar_passivo_validado` | O reparo chama `_materializar_uma` com `user`; o comando chama `reparar` com o autor. Compartilha a assinatura do ramo materializado; marca individual exclusiva de criação pelo reparo **[não verificado]**. [app/chamados/services/passivo_validado.py:529] [app/chamados/services/materializacao.py:1846] [app/chamados/management/commands/materializar_passivo_validado.py:113] |
| `backfill_fio_morto_e4b` | No ato `materializar`, chama a mesma fila da disputa com `user`; no ato separado `registrar_decisao`, registra linhas de projeção. Uma linha de decisão, portanto, não prova sozinha qual porta criou a batida. [app/chamados/management/commands/backfill_fio_morto_e4b.py:138] [app/chamados/management/commands/backfill_fio_morto_e4b.py:144] |
| Relavra de via e carimbo de pendentes aprovadas | `relavrar` altera `via_resolucao` e registra `relavra_via_i6`; `materializar_pendentes_aprovadas` carimba perguntas aprovadas e declara que não fabrica batida. São modificadores de trilha, não uma assinatura de criação de batida nesses trechos. [app/chamados/services/relavra_via.py:106] [app/chamados/services/relavra_via.py:125] [app/chamados/services/materializacao.py:2139] |

Trechos literais decisivos:

- Criação de hora: `disputa.colaborador, ts, _tipo, 'disputa_s84_retro'` e `aprovada_por=request_user`. [app/chamados/services/materializacao.py:1488]
- Criação de âncora: `disputa.colaborador, ts, 'E', 'disputa_ancora'`. [app/chamados/services/materializacao.py:1605]
- Reuso: `if mesma is not None: return mesma, None`; o materializador pode depois fazer `anotar(... {'pergunta_origem': p} ...)`. Assim, vínculo e log não provam, isoladamente, que aquele toque criou a linha. [app/ponto/registro_batida.py:137] [app/chamados/services/materializacao.py:1542]
- Carimbo: `p.carimbar(PerguntaDisputa.VIA_DECLARACAO if _tem_resp_via else _via_sem_decl, request_user, ...)`; a relavra pode alterar a via depois. [app/chamados/services/materializacao.py:1981] [app/chamados/services/relavra_via.py:125]
- Auditoria da materialização: `evento(acao, 'Batida', batida_pk, ... depois=valor_depois, ator=request_user)`; o próprio auxiliar declara que falha do log não derruba a materialização. [app/chamados/services/materializacao.py:1223]

A busca acima localizou as portas explicitadas; a exaustividade de **todos** os possíveis curadores e chamadores históricos permanece **[não verificado]**. O modelo de `Batida` expõe aprovador, justificativa e criação, mas não um campo de criador no trecho do modelo. [app/ponto/models.py:8] [app/ponto/models.py:56] [app/ponto/models.py:63] [app/ponto/models.py:79]

**B. Sonda proposta — não executada [não verificado].**

A sonda abaixo retorna assinaturas **compatíveis**, sem atribuir uma porta específica. Para evitar imprimir nomes ou CPF que possam estar em texto livre, ela mostra presença e SHA-256 de justificativa e motivos, em vez do texto. Isso limita a inspeção literal desses campos. A busca de auditoria inclui referências numéricas em descrição e JSON: são **candidatas**, pois um número isolado pode coincidir com outro identificador; também não prova ausência de referências escritas sem o número. [app/core/models.py:295] [app/core/models.py:299] [não verificado]

```python
# Executar dentro do Django pelo envelope de leitura, no tenant juliani.
# Não chama materializadores, validadores ou escritores.
# Não imprime nomes, CPF, textos livres, respostas ou conteúdo dos logs.

import hashlib
import os
import re

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Q, TextField
from django.db.models.functions import Cast
from django.utils import timezone

from core.models import LogAuditoria
from ponto.models import Batida


CASOS = (
    (40, 139931), (60, 152873), (200, 146569),
    (263, 146777), (301, 151816), (444, 140817),
    (446, 144666), (549, 151894), (727, 140816),
    (881, 154095), (899, 151735), (909, 149856),
    (996, 153862), (996, 153847),
)
ERROS = 0


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("SETTINGS")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("CONFIGURACAO")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("CONEXAO")


def local(valor):
    if valor is None:
        return None
    if not timezone.is_aware(valor):
        raise ValueError("INSTANTE_SEM_FUSO")
    return timezone.localtime(valor).isoformat()


def usuario(pk):
    return None if pk is None else "u%d" % pk


def texto_protegido(valor):
    if valor is None:
        return None
    valor = str(valor)
    return {
        "vazio": not bool(valor),
        "sha256": hashlib.sha256(valor.encode("utf-8")).hexdigest(),
    }


def codigo(valor):
    # Os campos de código devem conter identificadores, não texto livre.
    # Valor inesperado é protegido para evitar exposição acidental.
    if valor is None:
        return None
    valor = str(valor)
    if re.fullmatch(r"[a-z0-9_]{1,64}", valor):
        return valor
    return {"valor_protegido": texto_protegido(valor)}


def campo_texto(obj, nome):
    if not any(f.name == nome for f in obj._meta.concrete_fields):
        return "CAMPO_INEXISTENTE"
    return texto_protegido(getattr(obj, nome))


def logs_candidatos(batida_pk, pergunta_pk):
    # Busca referência numérica em texto/JSON e referência estruturada.
    # As referências numéricas são candidatas, não identificação inequívoca.
    filtro = Q(modelo="Batida", objeto_id=batida_pk)
    numeros = [batida_pk]
    if pergunta_pk is not None:
        filtro |= Q(modelo="PerguntaDisputa", objeto_id=pergunta_pk)
        numeros.append(pergunta_pk)

    for numero in numeros:
        padrao = r"(^|[^0-9])%d([^0-9]|$)" % numero
        filtro |= (
            Q(descricao__regex=padrao)
            | Q(antes_texto__regex=padrao)
            | Q(depois_texto__regex=padrao)
        )

    return (
        LogAuditoria.objects.annotate(
            antes_texto=Cast("valor_antes", output_field=TextField()),
            depois_texto=Cast("valor_depois", output_field=TextField()),
        )
        .filter(filtro)
        .order_by("criado_em", "pk")
    )


def medir(cid, pk):
    b = Batida.objects.select_related("pergunta_origem").get(
        pk=pk, colaborador_id=cid
    )
    p = b.pergunta_origem

    print(
        "BATIDA",
        {
            "colaborador": "col%d" % cid,
            "pk": b.pk,
            "instante_local": local(b.timestamp),
            "tipo": codigo(b.tipo),
            "origem": codigo(b.origem),
            "aprovada": b.aprovada,
            "aprovada_por": usuario(b.aprovada_por_id),
            "aprovada_em_local": local(b.aprovada_em),
            "justificativa": campo_texto(b, "justificativa"),
            "pergunta_origem_id": b.pergunta_origem_id,
            "retratada_em_local": local(b.retratada_em),
            "criado_em_local": local(b.criado_em),
            "criador": "CAMPO_INEXISTENTE",
        },
    )

    pergunta_pk = None
    if p is None:
        print("PERGUNTA", {"batida_pk": pk, "pergunta_origem": None})
    else:
        pergunta_pk = p.pk
        print(
            "PERGUNTA",
            {
                "batida_pk": pk,
                "pk": p.pk,
                "motivo": campo_texto(p, "motivo"),
                "recusa_motivo": campo_texto(p, "recusa_motivo"),
                "tipo_resposta": codigo(p.tipo_resposta),
                "via_resolucao": codigo(p.via_resolucao),
                "respondida_em_local": local(p.respondida_em),
                "validada_em_local": local(p.validada_em),
                "validada_por": usuario(p.validada_por_id),
                "materializada_em_local": local(p.materializada_em),
                "materializada_por": usuario(p.materializada_por_id),
            },
        )
        decisoes = []
        for d in p.decisoes.order_by("ato_em", "pk"):
            decisoes.append(
                {
                    "pk": d.pk,
                    "ato": codigo(d.ato),
                    "ato_em_local": local(d.ato_em),
                    "autor": usuario(d.ato_por_id),
                    "ato_motivo": campo_texto(d, "ato_motivo"),
                }
            )
        print("DECISOES", {"pergunta_pk": p.pk, "sequencia": decisoes})

    logs = []
    for linha in logs_candidatos(pk, pergunta_pk):
        referencia_direta = (
            (linha.modelo == "Batida" and linha.objeto_id == pk)
            or (
                pergunta_pk is not None
                and linha.modelo == "PerguntaDisputa"
                and linha.objeto_id == pergunta_pk
            )
        )
        logs.append(
            {
                "pk": linha.pk,
                "acao": codigo(linha.acao),
                "quando_local": local(linha.criado_em),
                "usuario": usuario(linha.usuario_id),
                "referencia": (
                    "OBJETO_DIRETO" if referencia_direta
                    else "NUMERO_EM_TEXTO_OU_JSON_CANDIDATO"
                ),
            }
        )
    print("LOGS_CANDIDATOS", {"batida_pk": pk, "linhas": logs})

    # Compatibilidade com o ramo escritor atual, não prova de autoria:
    # a porta pode devolver uma batida preexistente e o vínculo pode mudar.
    if b.origem == "disputa_s84_retro":
        assinatura = "COMPATIVEL_RAMO_HORA_ORIGEM"
    elif b.origem == "disputa_ancora" and b.tipo == "E":
        assinatura = "COMPATIVEL_RAMO_DATA_HORA_ORIGEM_E"
    else:
        assinatura = "SEM_CASA"
    print(
        "ASSINATURA",
        {
            "batida_pk": pk,
            "resultado": assinatura,
            "porta_especifica": "NAO_VERIFICADA",
            "criacao_pelo_ato": "NAO_VERIFICADA",
        },
    )


def main():
    global ERROS
    try:
        with transaction.atomic():
            guarda_sombra()
            for cid, pk in CASOS:
                try:
                    # O savepoint permite continuar após erro de SQL.
                    with transaction.atomic():
                        medir(cid, pk)
                except Exception as exc:
                    ERROS += 1
                    print(
                        "ERRO_CASO",
                        "col%d" % cid,
                        "batida_pk=%d" % pk,
                        type(exc).__name__,
                    )
    except Exception as exc:
        ERROS += 1
        print("ERRO_GUARDA_OU_TRANSACAO", type(exc).__name__)

    print("ERROS=%d" % ERROS)
    print("FIM_SONDA_B4V3")


main()
```

`ERROS=0` indicaria apenas que a sonda terminou sem exceções capturadas; não converteria candidatos de log em prova nem identificaria uma porta específica compartilhada. [não verificado: sonda não executada]

perguntas de lei: 0

## 19/11 01:43 · sessao 5fd4cbef7389 · E4C-F5 · censo · origem=Code

O censo **não está fechado**: não classifiquei as 415 linhas do TSV por receptor e finalidade. Por isso, os totais por classe, a lista final exaustiva da classe (i) e o número de perguntas de lei permanecem **[não verificado]**. Abaixo estão os sítios e os limites que consegui conferir diretamente na cópia.

O diretório recebido não contém metadados Git; portanto, a identidade da cópia pelo hash `5bad81419` está **[não verificado]**. Não criei, alterei ou apaguei arquivos.

### Classificações comprovadas e candidatos à migração

`LeituraDoAto` tem exatamente os campos `validada_em`, `respondida_em` e `resposta_colab`; não oferece `materializada_em` nem `via_resolucao`. O código declara: `LeituraDoAto = namedtuple('LeituraDoAto', 'validada_em respondida_em resposta_colab')`. ([models.py:17](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:17))

| Sítio do TSV | Classificação verificada e consequência |
|---|---|
| [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), `_resposta_validada` | **PerguntaDisputa, classe (i):** `return perg.validada_em is not None`. A leitura correspondente é `leitura.validada_em`. Chamam-na, entre outros, `declarar_hora_colab` e `aplicar_declaracao`; a guarda pode impedir que essas portas avancem até a escrita. ([declaracao_texto.py:379](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:379), [declaracao_texto.py:669](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:669)) |
| [juizes.py:1552](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1552), `pergunta_respondida` | **Receptor polimórfico:** o literal é `p.respondida_em is not None and not texto_vazio(p.resposta_colab)`. A função aceita tanto a projeção quanto `LeituraDoAto`; a migração deve ser examinada nos chamadores, pois o próprio modelo declara que o juiz lê a leitura. `aplicar_declaracao` usa o resultado para decidir se chama o escritor de resposta. ([models.py:17](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:17), [declaracao_texto.py:688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:688)) |
| [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188), `espera_o_texto_do_colaborador` | **PerguntaDisputa, classe (i):** `p.respondida_em is None or respondida_sem_texto(p)`. Para julgar pelo ato, o instante e o par usado pelo predicado precisam vir da mesma `LeituraDoAto`. Chamadores comprovados incluem `_calados` e `montar_chamados_fio`; efeito desses resultados sobre o momento de uma escrita **[não verificado]**. ([cobranca.py:241](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/cobranca.py:241), [fio.py:360](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:360)) |
| [validacao.py:685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:685), `validar_pergunta` | **PerguntaDisputa, classe (i):** `if perg.validada_em:` decide o retorno `ja_validada`; o campo correspondente é `leitura.validada_em`. A própria função é uma porta de validação; seus chamadores completos e o efeito concreto da troca sobre as escritas **[não verificado]**. |
| [materializacao.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:199), `par_do_intervalo` | **PerguntaDisputa, candidato classe (i):** `if irma_validada and q.validada_em is None`; a linha 201 também testa `q.validada_em`. O campo correspondente é `leitura.validada_em`. Um chamador comprovado é `_validar_o_par`, que pode carimbar e materializar; a classificação final dos dois testes e seus demais chamadores está **[não verificado]**. ([validacao.py:632](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:632)) |
| [vigia_fio.py:747](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vigia_fio.py:747), `respondida_sem_resposta` | **PerguntaDisputa, classe (iii):** a finalidade declarada é ler “os dois campos que a porta escreve” e “não julga a pergunta”; a query filtra `respondida_em` e lê `resposta_colab`. ([vigia_fio.py:738](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vigia_fio.py:738)) |
| [veredito_lavrado.py:203](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:203), `marcos_do_lote` | **PerguntaDisputa, classe (iv):** lê marcos `materializada_em` e `respondida_em`; o comentário os identifica como “escritores da PROPRIA pergunta” usados para o veredito. ([veredito_lavrado.py:197](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:197)) |
| [painel.py:270](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/painel.py:270), `_jan_val` | **PerguntaDisputa, classe (v):** conta atividade humana de validação por janela, usando o carimbo projetado. ([painel.py:256](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/painel.py:256)) |
| [raiox.py:138](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:138), `creditos_perguntas` | **PerguntaDisputa, classe (vi), com julgamento posterior:** a consulta em lote extrai o par e depois chama `pergunta_respondida` sobre um `SimpleNamespace`. A fonte do par para esse julgamento precisa de desenho em lote; a substituição direta por uma propriedade em cada linha não está demonstrada. ([raiox.py:135](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:135), [raiox.py:147](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:147)) |

A guarda de declaração mostra por que uma mudança de leitura pode mudar **quando uma porta existente escreve**: `if not _pergunta_respondida(perg)` antecede `responder_pergunta_disputa_core(...)`; depois a função chama `validar_pergunta(...)`. ([declaracao_texto.py:688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:688))

Não tratei `projecao_temporal_da_pergunta` como substituição mecânica: ela declara ler valores do C6 “no instante”, corta carimbos posteriores a `agora` e também transporta materialização e via, que não estão na leitura de três campos. Sua classificação e o desenho temporal de migração ficam **[não verificado]**. ([juizes.py:1382](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1382))

Os demais receptores homônimos, as demais linhas e os totais das seis classes: **[não verificado]**.

### Silêncio ressuscitado → dono admin

O corte fornecido determina admin na lista para a **ressuscitada** cujo silêncio decidido hoje a deixa sem dono, com dia do fato em competência aberta; para competência entregue pelo K6, determina lista textual do DP. Essa redação vem do anexo `respostas_1711_1120.txt` fornecido no pedido; sua existência como arquivo nesta cópia está **[não verificado]**. A cópia registra o corte anterior em [CORTES.md:545](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:545).

O caminho atual tem três obstáculos concretos:

1. `_pela_decisao` retorna literalmente `return 'silencio_decidido', []`. ([tarefa_do_dia.py:182](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:182))
2. A carga retira a reabertura marcada como ressuscitação **antes** de escolher a última decisão. Logo, mudar indistintamente o ramo `SILENCIO` alcançaria também silêncios para os quais a ressuscitação não foi demonstrada; como transportar essa distinção ao ponto escolhido é parte do desenho. ([tarefa_do_dia.py:390](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:390))
3. `_portas_da_tarefa` inclui `silencio_decidido` em `perguntas_encerradas` e aceita como ações administrativas alcançáveis `DECIDIR_AUSENCIA`, `resolver_dia` ou `VALIDAR` nas condições de porta/âncora ali expressas. ([tarefa_do_dia.py:739](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:739), [tarefa_do_dia.py:746](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:746))

**Encaixes para examinar, sem escolher:**

- **Ação `resolver_dia` no caso identificado.** É um nome de ação administrativa já reconhecido pela composição, e a própria composição já o cria quando a autoridade do calendário oferece essa porta. A disponibilidade dessa porta para cada silêncio ressuscitado e sua adequação para levar a pergunta à lista estão **[não verificado]**. Seria necessário distinguir o caso ressuscitado e ajustar o tratamento de encerrada conforme a rota escolhida. ([tarefa_do_dia.py:746](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:746), [tarefa_do_dia.py:776](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:776))
- **Ação ligada à âncora da pergunta, reconhecida pela composição.** `VALIDAR` já existe e tem verificação de âncora, mas a porta de validação recusa pergunta sem resposta: `estado: 'sem_resposta'`. Portanto, sua adequação ao silêncio ressuscitado **[não verificado]**; não basta rotular a ação como `VALIDAR`. O botão `cobrar` existe, mas o código o exclui dos verbos residuais que dão dono. ([tarefa_do_dia.py:746](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:746), [validacao.py:681](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:681), [tarefa_do_dia.py:757](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:757), [views_cobrar.py:44](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/views_cobrar.py:44))
- **Composição de dono a partir de um verbo residual existente.** A composição já deriva `admin` de verbos residuais, e publica o nome do verbo; porém a existência de um verbo residual apropriado para cada caso e de uma porta que cumpra a lista determinada pelo corte está **[não verificado]**. Retirar apenas `silencio_decidido` da tupla de encerradas não cria, por si, uma ação nem prova o dono. ([tarefa_do_dia.py:757](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:757), [tarefa_do_dia.py:790](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:790))

Para qualquer encaixe, já existe carga em lote que consulta `competencia_entregue` sobre a empresa e a janela do dia. Porém o filtro `exportada` atual é explicitamente limitado a **novas exposições administrativas daquela obra**, preservando tarefas E2; não prova sozinho que bloqueará o novo caminho de silêncio. ([tarefa_do_dia.py:207](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:207), [tarefa_do_dia.py:558](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:558), [tarefa_do_dia.py:273](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:273))

O teste existente compara a tarefa antes e depois da ressuscitação para os dois casos. O caso sem decisão deve conservar sua expectativa; o caso com silêncio precisa passar a verificar o resultado exigido para competência aberta, incluindo dono e lista, e o recorte de competência entregue precisa de prova própria. O fixture e a asserção atuais estão em [test_f1nova_b1_juiz_respondida.py:328](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328).

**`chamados/models.py` por desenho:** para os três encaixes acima, a necessidade de alterar o arquivo permanece **[não verificado]**. A leitura já existe e a composição examinada fica em `tarefa_do_dia.py`, mas ainda falta demonstrar como a marca de ressuscitação chega ao encaixe e como todos os consumidores do censo são atendidos. ([models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295), [tarefa_do_dia.py:390](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:390))

Não executei testes. A classificação completa, a lista final de sítios e a análise de todas as leis exigem continuação da leitura; não há base para declarar ausência de perguntas de lei. [nao verificado]

perguntas de lei: [não verificado]

## 19/11 01:42 · sessao daadceabae56 · QUEIMA-B6 · leitura · origem=Code

**Não consigo recuperar quais eram as três perguntas anunciadas em `r.txt`: a matriz termina com a contagem, mas não as enumera.** Também não provei que o número correto seja zero. [não verificado]

A releitura encontrou três conflitos possíveis que **já têm resposta**. Eles não podem ser apresentados como as três perguntas originais, pois essa identificação não está no anexo. [não verificado]

1. **Admin `trabalhou` versus fato do cartório.** O código responde à precedência na célula: `if cel.veredito_via == 'admin':` impede que a lavra do cartório substitua o veredito administrativo. [app/ponto/portas/celula.py:1060] A L-007 trata de escritor único por estado e trilha; não é necessário supor uma escolha nova para essa precedência. [app/docs/LEIS.md:71]

   A sonda não mede colisões entre esses dois escritores: conta o **estado atual** da célula, não as tentativas anteriores de escrita. O predicado necessário seria: mesmo colaborador e data, decisão administrativa vigente e tentativa posterior do cartório com veredito diferente, incluindo tentativas que não alteraram a célula. A disponibilidade de trilha suficiente para medi-lo é [não verificado]. [app/ponto/portas/celula.py:1060]

2. **Trabalho versus ausência ou justificativa.** O corte já responde que a justificativa cobre seu tipo e período e que, havendo trabalho, paga-se o realizado mais o abonado do período; para ausência parcial, há regras específicas sobre a batida dentro do período e a falta de horário declarado. [app/docs/CORTES.md:411] [app/docs/CORTES.md:418] A L-089 trata do efeito da ausência em dia previsto de trabalho. [app/docs/LEIS.md:121]

   A sonda mede uma sobreposição mais ampla, sem demonstrar valores de realizado ou abono nem se os períodos coincidem. As linhas literais fornecidas são:

   ```text
   PAR|empresa=2|categoria1=CELULA_cartorio_fato_em_ausencia|categoria2=DIAPAGO_motor_TRABALHO|dias=48
   PAR|empresa=3|categoria1=CELULA_cartorio_fato_em_ausencia|categoria2=DIAPAGO_motor_TRABALHO|dias=16
   PAR|empresa=4|categoria1=CELULA_cartorio_fato_em_ausencia|categoria2=DIAPAGO_motor_TRABALHO|dias=4
   ```

   Linha numerada de `sonda.out`: [não verificado]. O predicado para medir a aplicação financeira da regra seria: mesmo colaborador e data, ausência ou justificativa aprovada com seu período, trabalho realizado e `DiaPago` da versão motor, comparando os minutos realizados e abonados com os respectivos períodos. [não medido]

3. **Folga trabalhada versus hora trabalhada sem escala certa.** O código já explicita que, sem escala certa, a hora entra em `horas_trabalhadas` sem o adicional de 100%; com escala certa, a folga é encaminhada à rubrica própria, com separação para feriado. [app/ponto/services/fechamento.py:925] [app/ponto/services/fechamento.py:936] [app/ponto/services/fechamento.py:965] A L-086 chega perto ao tratar da dobra em domingo e feriado, com ressalva sobre o texto de feriado indicada na própria lei. [app/docs/LEIS.md:118]

   A sonda conta rubrica positiva de folga, mas não mede a alternativa sem escala certa nem o impacto de uma mudança de regra. [não verificado] Linhas literais:

   ```text
   CENSO|empresa=2|categoria=DIAPAGO_motor_FOLGA|atuais=186|vigentes_HE=0|revogados=NA|dias_sobrepostos=184
   CENSO|empresa=3|categoria=DIAPAGO_motor_FOLGA|atuais=35|vigentes_HE=0|revogados=NA|dias_sobrepostos=35
   CENSO|empresa=4|categoria=DIAPAGO_motor_FOLGA|atuais=2|vigentes_HE=0|revogados=NA|dias_sobrepostos=2
   ```

   Linha numerada de `sonda.out`: [não verificado]. O predicado para medir o outro ramo seria: períodos de `resultado.periodos_ft` cujo julgamento de `escala_certa_no_dia` seja falso, por empresa e dia de jornada, comparados à lavratura correspondente em `DiaPago`; para medir dinheiro alterado, seria necessário comparar as rubricas antes e depois de uma regra definida. [app/ponto/services/fechamento.py:933] [app/ponto/services/fechamento.py:967]

A sobreposição entre categorias da sonda, por si só, não estabelece pagamento duplicado nem responde quantos dias mudariam sob uma opção de regra. [não verificado] Sem a enumeração anterior ou três lacunas demonstradas na releitura, atribuir um número corrigido seria uma hipótese. [não verificado]

perguntas de lei: [não verificado]

## 19/11 01:41 · sessao 2d8feb0d356d · E4C-F3-LAVRA-FOTO · construtor · origem=Code

Criado [test_f3_lavra_foto_sem_decisao.py](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:1). Os testes não foram executados, conforme solicitado; os resultados são [não verificado]. [nao verificado]

Os mutantes usam a âncora `    ids = list(colaborador_ids)\n`, exceto M6, que também usa `import datetime\n` para inserir o apelido no módulo. As inserções verificam que cada âncora ocorre uma única vez ([arquivo:9](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:9), [arquivo:70](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:70)).

Para executar cada mutante individualmente, a partir de `app/`:

```bash
T=chamados.tests.test_f3_lavra_foto_sem_decisao.LavraFotoSemDecisaoTest
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m1_import_direto"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m2_import_apelidado"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m3_modulo_apelidado"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m4_get_model"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m5_related_manager"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m6_apelido_no_modulo"
```

O arquivo também contém os métodos `test_fonte_real_sem_leitura`, `test_alvo_ausente_falha`, `test_controle_leitura_na_vizinha_nao_conta`, `test_MORDE_funcao_aninhada`, `test_MORDE_lookup_relacionado` e `test_MORDE_get_model_string_unica` ([arquivo:81](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:81)). O controle usa a âncora `def esta_stale(p, marcos, agora):\n`; os três mutantes adicionais usam a âncora do corpo ([arquivo:122](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:122)).

## 19/11 01:35 · sessao 97f3c0098f5b · ESPURIA-TELA · verificador · origem=Code

1. **NÃO BLOQUEIA.** Os quatro valores novos batem com os sítios B1 medidos nesta árvore:

   | Função | Linha do `def` | Sítio B1 / novo valor |
   |---|---:|---:|
   | `autoridade_do_periodo` | [987](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:987) | [1045](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:1045) |
   | `montar_dias` | [379](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:379) | [440](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:440) |
   | `_dia_das_batidas_puro` | [2961](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/turnos.py:2961) | [2971](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/turnos.py:2971) |
   | `dia_do_turno_da_batida` | [2943](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/turnos.py:2943) | [2949](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/turnos.py:2949) |

   Para B1, o medidor registra `n.lineno` da **chamada AST** `.date()`, `localdate` ou `data_local` cujo operando recebe marca lexical; não registra a linha do `def` ou do decorador ([medida_turno_do_par.py:197](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/medida_turno_do_par.py:197), [medida_turno_do_par.py:217](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/medida_turno_do_par.py:217)). Os quatro valores constam no registro da cópia ([test_medida_turno_do_par.py:324](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:324), [test_medida_turno_do_par.py:336](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:336)).

2. **NÃO BLOQUEIA — zero divergências.** Executei `censo('app')` em memória, sem bytecode, e comparei as duas tabelas inteiras com `LINHAS_REGISTRADAS`, usando a mesma chave `(chave, sitio ou criterio)` e as mesmas listas de linhas usadas pelo teste: **9/9 pareadores e 85/85 derivações**, sem chave ausente, extra ou com linha diferente ([test_medida_turno_do_par.py:558](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:558), [medida_turno_do_par.py:448](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/medida_turno_do_par.py:448)). O resultado vale para o universo e os critérios do medidor, que têm limitações declaradas ([medida_turno_do_par.py:32](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/medida_turno_do_par.py:32)).

3. **NÃO BLOQUEIA, quanto ao diff fornecido.** O anexo transcrito mostra alteração somente nas quatro listas do arquivo de teste; nessa transcrição, não há alteração de `.py` fora de `app/*/tests/`. Ele atualiza a fotografia das linhas esperadas, preservando a comparação por igualdade e as demais catracas de número e identidade ([test_medida_turno_do_par.py:504](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:504), [test_medida_turno_do_par.py:542](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:542), [test_medida_turno_do_par.py:558](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:558)). A correspondência do anexo com o diff Git entre os hashes **[não verificado]**: a cópia disponibilizada não contém repositório Git.

4. **NÃO BLOQUEIA.** `LINHAS_REGISTRADAS` é definido como literal no teste e lido pela asserção de igualdade; o medidor não o consulta ([test_medida_turno_do_par.py:9](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:9), [test_medida_turno_do_par.py:247](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:247), [test_medida_turno_do_par.py:558](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:558)). O resultado de `censo()` é construído pelo medidor a partir das fontes ([medida_turno_do_par.py:336](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/medida_turno_do_par.py:336), [medida_turno_do_par.py:448](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/medida_turno_do_par.py:448)); é consumido pela suite na catraca ([test_medida_turno_do_par.py:499](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_medida_turno_do_par.py:499)) e pelo leitor do ESTADO, que o formata e publica via `gerar_estado.py` ([turno_do_par_estado.py:28](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/bin/turno_do_par_estado.py:28), [turno_do_par_estado.py:54](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/bin/turno_do_par_estado.py:54), [gerar_estado.py:307](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/bin/gerar_estado.py:307)).

## 19/11 01:28 · sessao 9886687ca8aa · E4C-F5 · censo · origem=Code

A cópia tem dois leitores com responsabilidades diferentes: `DecisaoPergunta.leitura` recompõe **resposta e validação**, enquanto `_pela_decisao` já reconhece **`silencio_decidido`** e hoje devolve uma lista vazia de ações. A migração precisa ligar esse resultado ao dono e à porta do admin; trocar apenas os três campos da leitura não faz essa ligação. ([models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295), [tarefa_do_dia.py:142](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:142))

A análise abaixo identifica os sítios comprovados, mas **não fecha um inventário exaustivo de todos os leitores e escritores de produção**: o censo por nome encontra também campos homônimos de outros modelos, e a atribuição de cada receptor exige revisão. O script entregue em C produz candidatos; sua saída e a classificação completa, receptor por receptor, estão **[não verificado]**. Não executei testes nem alterei arquivos. [nao verificado]

## A. Matriz de estado, leitura e prova

A ordem dos campos abaixo é a **real**: `LeituraDoAto(validada_em, respondida_em, resposta_colab)`. A leitura ordena atos reais e passivo por instante; no empate, a linha virtual do passivo vem depois dos atos reais. Ela não devolve último ato, materialização ou via. ([models.py:19](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:19), [models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295))

Os retornos de `_pela_decisao` na tabela pressupõem que a pergunta chegue à função, sem prevalência de `passivo_sem_linha`; o resultado final da tarefa também depende da célula, lâmpada, horizonte e portas. ([tarefa_do_dia.py:154](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:154), [tarefa_do_dia.py:227](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:227))

| Estado ou último ato | Juiz e retorno hoje | Persistido e prova localizada |
|---|---|---|
| Sem ato nem projeção R/V | `leitura` retorna `(None, None, '')`; `_pela_decisao` retorna `aguarda_resposta` e ação `RESPONDER` do `COLABORADOR`. ([models.py:4315](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4315), [tarefa_do_dia.py:156](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:156)) | Sem linha R/V nem carimbo R/V, pela premissa. O teste de ressuscitação sem decisão espera `aguarda_resposta`. ([test_f1nova_b1_juiz_respondida.py:340](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:340)) |
| Projeção R/V sem linha correspondente | `leitura` conserva o carimbo e texto como linha virtual; `_pela_decisao` dá precedência a `passivo_sem_linha`, com ações `None`. ([models.py:4238](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4238), [tarefa_do_dia.py:154](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:154)) | Projeção em `PerguntaDisputa`; a linha virtual da leitura não é, por si, uma gravação. O teste cobre resposta só na projeção. ([models.py:4260](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4260), [test_e4c_f2_leitura_do_ato.py:172](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:172)) |
| `RESPOSTA` com texto | `leitura` põe `ato_em` em `respondida_em` e `ato_texto` em `resposta_colab`; resposta humana pode limpar `validada_em`. `_pela_decisao` retorna `aguarda_validacao`, ação `VALIDAR` do `ADMIN`. ([models.py:4317](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4317), [tarefa_do_dia.py:165](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:165)) | A porta humana grava projeção e ato no mesmo atomic. O teste cobre resposta nova depois da validação. ([disputa_emissao.py:1527](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1527), [test_e4c_f2_leitura_do_ato.py:239](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:239)) |
| `RESPOSTA` com texto vazio | A leitura conserva instante e texto vazio; `pergunta_respondida` exige instante **e** texto não vazio. `_pela_decisao` retorna `respondida_sem_texto`, ação `RESPONDER` do colaborador. ([juizes.py:1544](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1544), [tarefa_do_dia.py:165](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:165)) | O ato tem `ato_em` e `ato_texto`; o teste afirma que a leitura não o considera resposta. ([models.py:4166](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4166), [test_e4c_f2_leitura_do_ato.py:425](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:425)) |
| `VALIDACAO` | `leitura.validada_em = ato_em`. Na tarefa, sem lavra vigente: `veredito_sem_lavra_vigente`, ações `None`; com lavra `nao`: `validada_sem_efeito`, ação de materializar do sistema; os demais ramos estão em `:179–181`. ([models.py:4320](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4320), [tarefa_do_dia.py:173](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:173)) | A face SQL `q_validada_pelo_ato` inclui ato e passivo; há teste da linha sem carimbo e comparação da face com leitura. ([models.py:3539](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3539), [test_e4c_f2_leitura_do_ato.py:60](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:60), [test_e4c_f2_leitura_do_ato.py:178](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:178)) |
| `DEVOLUCAO` | Limpa `validada_em`, preservando R; `_pela_decisao` retorna `materializacao_recusada`, ação `VALIDAR` do admin. ([models.py:4277](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4277), [tarefa_do_dia.py:171](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:171)) | A sequência guarda o ato; teste: validação seguida de devolução resulta não validada. ([test_e4c_f2_leitura_do_ato.py:65](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:65)) |
| `RECUSA` ou `REABERTURA` comum | Limpa R e V; `_pela_decisao` retorna `devolvida_ao_colaborador` e `RESPONDER`. A reabertura **marcada como ressuscitação** é retirada da sequência usada para escolher o último ato da tarefa. ([models.py:4277](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4277), [tarefa_do_dia.py:169](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:169), [tarefa_do_dia.py:390](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:390)) | `reabrir_para_correcao` registra foto e limpa a projeção pela porta do lote; teste de resposta seguida de recusa verifica o par. ([disputa_emissao.py:1617](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1617), [test_e4c_f2_leitura_do_ato.py:165](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:165)) |
| `SILENCIO` | Não altera os três campos R/V da leitura; `_pela_decisao` retorna literalmente `('silencio_decidido', [])`. ([models.py:4272](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4272), [tarefa_do_dia.py:182](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:182)) | O ato `silencio` está no enum persistido. Testes cobrem preservação da validação e a tarefa antes/depois da ressuscitação; **não provam admin dono**. ([models.py:4152](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4152), [test_e4c_f2_leitura_do_ato.py:95](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:95), [test_f1nova_b1_juiz_respondida.py:338](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:338)) |
| `ABSORCAO` | Não muda os três campos da leitura. A tarefa distingue ausência aprovada, rejeitada, aguardando documento e pendente, retornando respectivamente nenhuma ação, `RESPONDER`, `ENTREGAR_DOCUMENTO` ou `DECIDIR_AUSENCIA`. ([models.py:4276](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4276), [tarefa_do_dia.py:184](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:184)) | O ato possui FKs `absorcao_escape` e `absorcao_ausencia`. Teste específico de cada ramo da tarefa **[não verificado]**. ([models.py:4176](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4176)) |
| Materializada / carimbada | `LeituraDoAto` não contém `materializada_em`; materialização não é um dos atos enumerados. A tarefa consulta a lavra vigente e seus efeitos, não infere materialização dos três campos. ([models.py:19](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:19), [models.py:4152](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4152), [tarefa_do_dia.py:173](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:173)) | O carimbo e a via permanecem na projeção; `carimbar_identidade` usa `_escrever_carimbo`. Teste específico da matriz de materialização **[não verificado]**. ([models.py:3364](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3364)) |
| `RETRATACAO` ou ato desconhecido | A leitura R/V não muda com retratação; `_tarefa_pura` exclui pergunta cuja última decisão é `retratacao`. Se outro ato chegar a `_pela_decisao` sem ramo reconhecido, retorna `ato_desconhecido:<ato>`, ações `None`. ([models.py:4276](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4276), [tarefa_do_dia.py:260](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:260), [tarefa_do_dia.py:193](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:193)) | O enum inclui `retratacao`; teste da foto da retratação existe. ([models.py:4161](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4161), [test_e4c_f2_leitura_do_ato.py:511](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:511)) |

## B. Silêncio até a lista do admin

O juiz existente é `tarefa_do_dia.py::_pela_decisao`: seu ramo literal é `return 'silencio_decidido', []`. A carga ordena decisões por pergunta, `ato_em`, `pk`, corta pelo `agora` e exclui a reabertura marcada de ressuscitação antes de escolher a última decisão. ([tarefa_do_dia.py:182](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:182), [tarefa_do_dia.py:390](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:390))

Hoje essa decisão **não entrega uma ação de admin por esse ramo**. `_portas_da_tarefa` ainda inclui `silencio_decidido` entre `perguntas_encerradas`; ela calcula o dono a partir das ações alcançáveis e de verbos residuais. Portanto não se pode afirmar que todo dia com silêncio necessariamente termine sem dono por qualquer outro caminho, mas o ramo do silêncio não implementa, por si, “admin vira dono”. ([tarefa_do_dia.py:729](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:729), [tarefa_do_dia.py:739](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:739), [tarefa_do_dia.py:746](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:746))

O dado fornecido determina para a ressuscitada com silêncio decidido e fato em competência aberta: “o dono passa a ser o ADMIN, na lista dele”; para competência entregue, prevê lista textual do DP. Esse anexo foi fornecido no pedido, sem arquivo:linha verificável nesta cópia **[não verificado como arquivo]**. Falta, no caminho comprovado, fazer a decisão de silêncio chegar a uma ação/porta de admin reconhecida pela composição de dono e lista, respeitando os cortes de competência e os demais filtros existentes; também falta o teste da F5 que prove esse resultado. O teste atual conserva a igualdade antes/depois e espera `silencio_decidido`. ([test_f1nova_b1_juiz_respondida.py:328](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328), [test_f1nova_b1_juiz_respondida.py:343](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:343))

O dado raspado diz `CLASSE_ULT silencio 213` e que não mediu quantas estão em passivo **[não verificado em produção]**. A carga identifica passivo por correspondência de ato e instante e o corta por `agora`; não há base para atribuir uma quantidade dessas 213 ao ramo `passivo_sem_linha`. ([tarefa_do_dia.py:425](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:425))

## C. Censo AST e leitores

O script abaixo procura os cinco **nomes de campo** na árvore Python, inclusive atributos, argumentos ORM e strings usadas diretamente em chamadas. É um **sobrecenso sintático**: não resolve o tipo do receptor, SQL dinâmico, aliases, templates, nem atribui com segurança toda string a leitura ou escrita. `escrita` em `update`, `create` e similares significa candidato sintático; a revisão semântica é necessária. A exclusão é apenas do corpo de `DecisaoPergunta.leitura`. Sua execução e saída TSV **[não verificado]**.

```python
import ast
import pathlib
import sys

ROOT = pathlib.Path(sys.argv[1]).resolve()
FIELDS = {
    "respondida_em", "resposta_colab", "validada_em",
    "via_resolucao", "materializada_em",
}
WRITE_CALLS = {"update", "create", "get_or_create", "update_or_create"}
READ_STRING_CALLS = {
    "getattr", "values", "values_list", "only", "defer",
    "order_by", "latest", "earliest",
}


def field_name(value):
    if not isinstance(value, str):
        return None
    name = value.lstrip("-").split("__", 1)[0]
    return name if name in FIELDS else None


class Census(ast.NodeVisitor):
    def __init__(self, path):
        self.path = path
        self.stack = []
        self.rows = set()

    def record(self, node, field, kind):
        if not field:
            return
        classes = [name for typ, name in self.stack if typ == "class"]
        funcs = [name for typ, name in self.stack if typ == "function"]
        if classes and classes[-1] == "DecisaoPergunta" and funcs and funcs[-1] == "leitura":
            return
        function = ".".join(funcs) if funcs else "<modulo>"
        rel = self.path.relative_to(ROOT).as_posix()
        parts = self.path.relative_to(ROOT).parts
        test = (
            any(part in {"test", "tests"} for part in parts)
            or self.path.name.startswith("test_")
            or self.path.name.endswith("_test.py")
        )
        self.rows.add((rel, node.lineno, function, field, kind, "teste" if test else "prod")) [nao verificado]

    def visit_ClassDef(self, node):
        self.stack.append(("class", node.name))
        self.generic_visit(node)
        self.stack.pop()

    def visit_FunctionDef(self, node):
        self.stack.append(("function", node.name))
        self.generic_visit(node)
        self.stack.pop()

    visit_AsyncFunctionDef = visit_FunctionDef

    def visit_Attribute(self, node):
        if node.attr in FIELDS:
            if isinstance(node.ctx, ast.AugLoad):
                kind = "leitura"
            elif isinstance(node.ctx, (ast.Store, ast.Del, ast.AugStore)):
                kind = "escrita"
            else:
                kind = "leitura"
            self.record(node, node.attr, kind)
        self.generic_visit(node)

    def visit_Call(self, node):
        func = node.func
        call = func.attr if isinstance(func, ast.Attribute) else (
            func.id if isinstance(func, ast.Name) else ""
        )
        for kw in node.keywords:
            field = field_name(kw.arg)
            if field:
                self.record(kw.value, field, "escrita" if call in WRITE_CALLS else "leitura")
        if call in READ_STRING_CALLS:
            for arg in node.args:
                if isinstance(arg, ast.Constant):
                    self.record(arg, field_name(arg.value), "leitura")
        if call == "setattr" and len(node.args) >= 2:
            arg = node.args[1]
            if isinstance(arg, ast.Constant):
                self.record(arg, field_name(arg.value), "escrita")
        self.generic_visit(node)


rows = set()
for path in sorted(ROOT.rglob("*.py")):
    try:
        tree = ast.parse(path.read_text(encoding="utf-8"), filename=str(path))
    except (OSError, UnicodeError, SyntaxError):
        continue
    visitor = Census(path)
    visitor.visit(tree)
    rows.update(visitor.rows)

print("arquivo\tlinha\tfuncao\tcampo\ttipo\tprod/teste") [nao verificado]
for row in sorted(rows):
    print("\t".join(map(str, row)))
print("FIM_CENSO")
```

**Leitores de produção comprovados que precisam entrar no desenho da migração**, sem afirmar que esta lista seja exaustiva:

- `pergunta_respondida` lê o par recebido; `particao_do_app` e `becos.classificar` já o chamam com a pergunta projetada. A troca deve preservar o juiz e alterar a fonte apropriada, sem tomar os chamadores como acessos crus. ([juizes.py:1552](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1552), [pergunta_no_app.py:83](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/pergunta_no_app.py:83), [becos.py:29](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/becos.py:29))
- `fio.espera_o_texto_do_colaborador` ainda combina `p.respondida_em` com outro juiz; `declaracao_texto._resposta_validada` lê V cru, e a declaração compara o texto projetado. A guarda de `aplicar_declaracao` chama o juiz com a pergunta e pode acionar o escritor. ([fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188), [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), [declaracao_texto.py:418](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:418), [declaracao_texto.py:688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:688))
- `reconciliador` e `vigia_carimbo` filtram V em SQL; a face existente é `q_validada_pelo_ato`. Os filtros de `materializada_em` têm outro significado e não podem ser substituídos por um campo inexistente na leitura. ([reconciliador.py:673](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:67

[truncado em 20000 de 29421 caracteres]

## 19/11 01:27 · sessao ff71187dba85 · DST-L2-JZ1 · verificador · origem=Code

**AVAL-DEVOLVIDO para o item 3.** Há duas respostas literais que precisam ser corrigidas antes de tratar o desenho como fechado: o fixture atual não cria a justificativa com origem, e o contador D4 não identifica exatamente os fechamentos “não protesto” com os campos examinados. O RED R5 do brief também precisa ser reescrito para a decisão posterior de que o ponteiro manda. A aprovação do censo pelo wrapper é **[não verificado]**.

Os anexos `aval.txt` e `brief_c1.md` foram fornecidos no pedido, mas não localizados como arquivos nesta cópia; suas passagens abaixo são identificadas como **anexo fornecido [não verificado em arquivo]**. O diretório também não expõe metadados Git; a correspondência do conteúdo ao hash informado é **[não verificado]**.

Os vereditos abaixo significam: **PROCEDE** quando a afirmação ou condição examinada confere; **NÃO PROCEDE** quando o literal não confere ou não fecha com a prova disponível.

1. **PROCEDE — Fix A atual.** [`_ato_da_retratacao` em reconciliador.py:563](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563) primeiro exige sucesso da transição e, nas linhas 573–579, lê `ch.justificativa_relacionada`, testa `status == 'pendente'`, grava `'rejeitada'` e salva apenas `['status', 'descricao']`. Não verifica `chamado_origem` nem inclui `atualizado_em`.

2. **PROCEDE — os dois sítios nomeados são os explicitamente alimentados pelo conjunto “não protestam”.** [`cartorio.py:1540`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1540) forma `_nao_protestam = [x for x in vivos if not _protesta(x)]`; `1546` fecha os não adiados, e `1541–1543` envia os adiados a `1011`. Os outros têm critérios próprios: marcos inexistentes em `1557–1559`, códigos do dia e `vivos` em `1572–1580`, duplicatas em `1653–1659` e outros duplicados em `1669–1674`. Isso **não prova que os conjuntos sejam disjuntos em todos os estados**: os quatro não são sítios cujo critério literal de seleção seja `_nao_protestam`.

3. **PROCEDE — reentrada condicionada.** [`cartorio.py:40`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:40) define `em_julgamento()` pela flag local; ela é ativada em `973` e limpa em `1123`. [`signals.py:161`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161) liga o `post_save` da Justificativa a `_julgar_dias`; em `90–93`, a flag causa retorno, senão chama `julgar_celula`. O resultado exato “0 chamadas extras/1 chamada” no novo caso ainda depende do teste exigido: **[não verificado]**.

4. **NÃO PROCEDE — premissa do fixture no item 1(b).** [`test_jz1_autoridade_unica.py:42`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_jz1_autoridade_unica.py:42) cria com `colaborador`, `tipo`, `data_turno`, `descricao` e `status`; nas linhas `45–46` grava o ponteiro, sem `chamado_origem`. O campo aceita `null=True` em [`ponto/models.py:173`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173). A resposta “SIM” pode orientar a **mudança futura** do fixture, mas a frase de que ele já cria com origem é falsa nesta cópia. A existência de três caminhos de produção que fornecem origem também não garante origem não nula em toda execução: [`api/views.py:1995`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:1995), [`ponto/views.py:1634`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1634) e [`processar_alertas_turno.py:394`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:394).

5. **PROCEDE — localização e significado dos tetos.** Ambos moram no mesmo selo, [`test_selo_chokepoint_escrita.py:196`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/tests/test_selo_chokepoint_escrita.py:196) e `:210`: `registrar_batida (furo vivo, 1 chamado fecha): 224` e `(... 3 chamados fecham): 288`. O selo usa `CaptureQueriesContext` em `:428` e cobra os cenários em `:438` e `:443`; são tetos de consultas capturadas do cenário, inclusive os comandos de transação descritos no próprio selo, não limites de registros. Medições antes/depois da cura: **[não verificado]**.

6. **PROCEDE — não é necessário editar a porta do ponteiro para ler a relação.** [`vinculos.py:7`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:7) declara escritor único do apontamento fora do nascimento; `:81–89` implementa `apontar_justificativa`, com “PRIMEIRO vale”. O item 3 proíbe **tocar o arquivo**, segundo o anexo fornecido **[não verificado em arquivo]**. A cura descrita pode ler os dois vínculos e escrever status, descrição e relógio da Justificativa sem alterar o ponteiro; se uma implementação quiser alterar o apontamento, deve respeitar a porta existente.

7. **NÃO PROCEDE — D4 exato para “nascida depois do fecho” não está definido pelo literal.** Há `Justificativa.criado_em` e origem em [`ponto/models.py:173`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), e `Chamado.resolvido_em` e `via_encerramento` em [`chamados/models.py:52`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:52). Pode-se definir um **superconjunto operacional**, por exemplo pendentes ligadas a chamado terminal com `criado_em > resolvido_em`, tratando nulos e indicando se conta pares ou justificativas. Mas `RECONCILIAR` usa a via sistema em [`motor.py:88`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/catalogo/motor.py:88); isso não identifica exclusivamente os dois sítios de não protesto. Reabertura também limpa o instante em [`chamados/models.py:242`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:242). Logo o universo exato pretendido e o tratamento histórico exigem definição; a contagem fornecida pela sonda é **[não verificado]**.

8. **NÃO PROCEDE — R5 do brief permanece contraditório se tomado literalmente.** O anexo fornecido diz que R5 preserva a pendente de origem alheia e conta 1 **se** a resposta for “preserva” **[não verificado em arquivo]**. A resposta posterior fornecida escolhe “o ponteiro manda”: rejeita a apontada e reserva D4 à nascida depois do fecho **[não verificado em arquivo]**. O caso de origem alheia deve, portanto, testar rejeição; o mutante do contador deve morder um caso de nascimento posterior definido conforme o achado 7. O Fix A atual confirma a ausência de teste da origem em [`reconciliador.py:573`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573).

9. **PROCEDE — função única é compatível se “só” qualificar os fechos do cartório.** A extração precisa substituir também o Fix A de [`reconciliador.py:569`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:569), além de ser chamada após sucesso nos dois sítios do cartório. Interpretar “chamada só em 1011 e 1546” como proibição de chamada no reconciliador contradiz D1 do anexo fornecido **[não verificado em arquivo]**.

10. **PROCEDE — “sem S128 no fecho”, “sem followup novo” e não editar `vinculos.py` são tecnicamente distinguíveis.** Os fechos do cartório chamam `transicionar_com_pauta` diretamente em [`cartorio.py:1011`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011) e `:1546`; S128 é a unidade `_retratar` em [`reconciliador.py:517`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:517). Uma função que apenas seleciona e salva justificativas não precisa chamar essa unidade nem criar nota. Porém o `save()` dispara o signal do achado 3: “só escrita” não significa ausência de efeitos indiretos.

11. **NÃO PROCEDE — o resultado exigido em R4 ainda não está demonstrado.** A S128 verifica `retencao_da_retratacao`, inclusive `irma_orfa`, em [`reconciliador.py:439`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:439) e `:538–541`; os dois fechos nomeados usam outro caminho em [`cartorio.py:1011`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011) e `:1546`. Isso mostra uma diferença de guarda, mas não prova que a irmã ficará intacta nem que a justificativa será rejeitada no caso concreto. A compatibilidade desse resultado com a lei aplicável permanece **[não verificado]**; o próprio R4, no anexo fornecido, manda demonstrar e perguntar se divergir **[não verificado em arquivo]**.

12. **NÃO PROCEDE — “mutante por sítio para R1–R6” ainda não está especificado por completo.** Os mutantes de remoção das chamadas para R1/R3 são distinguíveis pelos sítios [`cartorio.py:1011`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011) e `:1546`; R2 pode remover a seleção por origem. R4 é descrito como demonstração, e R6 como medição de tetos, sem mutante por sítio definido no anexo fornecido **[não verificado em arquivo]**. Para R5 falta primeiro fechar o universo do achado 7. A morte de qualquer mutante e os logs em disco são **[não verificado]**.

13. **PROCEDE — não identifiquei colisão documental comprovada que autorize ignorar as restrições remanescentes; a revisão de abrangência é limitada.** O corte vigente DST-L2 manda o cartório consultar `dia_sem_trabalho` em [`CORTES.md:533`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/CORTES.md:533); o código o faz em [`cartorio.py:1462`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1462). O item 0 fornecido substitui apenas a cláusula de construção dos avais nomeados **[não verificado em arquivo]**. **Há proibições que ele não alcança:** as próprias vedações do item 3 a `vinculos.py`, S128 no fecho e followup novo **[não verificado em arquivo]**, e as regras de competência exportada, cuja leitura deve considerar conjuntamente [`LEIS.md:125`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/LEIS.md:125) e a alteração L-113 em [`LEIS.md:155`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/LEIS.md:155). Não posso afirmar ausência de todo outro aval vigente sem os demais avais e seu estado completo: **[não verificado]**.

Quanto ao alcance, os **arquivos de produção diretamente indicados** pelo desenho são [`chamados/reconciliador.py`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:563), [`ponto/services/cartorio.py`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1011) e [`ponto/services/justificativa.py`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:262). D4 exige também um local de produção para o contador, mas seu caminho não foi especificado no material fornecido **[não verificado]**. A lista final depende da implementação e do censo **[não verificado]**.

O desenho não nomeia edição de motor, folha, export, DiaPago, escala ou porta de vínculo **[não verificado em arquivo]**. Isso não demonstra impacto indireto zero: o `save` rejulga fora do cartório ([`signals.py:85`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:85)); e a retratação existente envolve cerca para competência entregue ([`reconciliador.py:542`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:542)). Efeito em competência exportada, dinheiro e necessidade de `!` para uma cura concreta: **[não verificado]**.

A volta 1 sobre escritores e leitores fica assim; é um inventário **confirmado por exemplos, não um censo completo**:

- **`Justificativa.status`:** escrevem aprovação em [`justificativa.py:205`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:205) e `:383`; rejeição no mesmo arquivo `:283`, em [`ponto/views.py:1877`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1877), [`triagem_batida.py:605`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:605), [`entrada_sincronizada.py:38`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:38) e [`reconciliador.py:573`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:573). Leem, entre outros, a fila e os cartões em [`ponto/views.py:1739`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1739), o SLA em [`alertar_justificativas_sla.py:19`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/alertar_justificativas_sla.py:19) e o cartório em [`cartorio.py:387`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:387). Universo completo por receptor: **[não verificado]**.

- **`Justificativa.chamado_origem`:** escrevem os caminhos em [`api/views.py:1995`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:1995), [`ponto/views.py:1634`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1634) e [`processar_alertas_turno.py:394`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:394). Leem, entre outros, [`justificativa.py:33`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:33), `:110`, e [`ponto/views.py:1686`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1686). `SET_NULL` na exclusão da origem consta do modelo em [`ponto/models.py:173`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173).

- **`ChamadoColaborador.justificativa_relacionada`:** a porta escreve em [`vinculos.py:81`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/services/vinculos.py:81); os três chamadores estão em [`api/views.py:2007`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/api/views.py:2007), [`ponto/views.py:1648`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1648) e [`processar_alertas_turno.py:426`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/management/commands/processar_alertas_turno.py:426). Leem, entre outros, [`reconciliador.py:453`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:453) e `:573`, [`chamados/models.py:136`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:136), `:1313`, e os dois serviços de entrada citados acima. O modelo também prevê `SET_NULL` em [`chamados/models.py:69`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:69).

- **`Justificativa.atualizado_em`:** é `auto_now=True` em [`ponto/models.py:179`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:179). Saves integrais como [`justificativa.py:283`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:283) o atualizam; os saves restritos de [`reconciliador.py:579`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:579), [`triagem_batida.py:613`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/triagem_batida.py:613) e [`entrada_sincronizada.py:43`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/entrada_sincronizada.py:43) não o incluem. O cartão “hoje” lê `atualizado_em__date=hoje` em [`ponto/views.py:1741`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1741). Outros leitores e escritores: **[não verificado]**.

[`JUIZES.md:7`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/docs/JUIZES.md:7) diz que a tabela é *lookup*, que o registro vivo fica em `app/core/juizes.py` e que “o VIVO manda”; [`core/juizes.py:316`](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/core/juizes.py:316) registra o juiz do **prazo** da justificativa. Isso não estabelece um escritor único global para `Justificativa.status`; os escritores confirmados acima mostram vários sítios.

**REVISÃO item 3: PROCEDE 8 / NÃO PROCEDE 5.**

## 19/11 01:26 · sessao a8965f7566b4 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

A revisão encontra base para o selo pedido, mas **não fecha integralmente a volta 1**: o censo completo de escritores e leitores de cada campo e a verificação dos avais `SANDUICHE-DO-20` e `RETOMA-POS-CLEAR` permanecem [não verificado]. O aval foi fornecido no texto da solicitação; suas referências abaixo identificam o anexo raspado, que não tem numeração de linhas fornecida.

1. **Premissas e selo — PROCEDE, com limite no censo.** `marcos_do_lote` mora em [app/chamados/veredito_lavrado.py:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151). Seu corpo importa `CelulaDia`, `Batida` e `PerguntaDisputa as _PD`, consulta os campos listados abaixo e devolve `marcos`; não há ali referência direta, importação, chamada ou apelido de `DecisaoPergunta` ([linhas 158–210](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:158)). Leitura indireta por comportamento interno do ORM ou de código não examinado: **[não verificado]**.

   `carimbar_resposta_pela_trilha` mora em [app/chamados/services/disputa_emissao.py:1692](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1692): “`p.respondida_em = instante`” e “`DecisaoPergunta.registrar`”. O comando de produção `curar_perguntas_em_limbo` importa e chama a função com `nota.criado_em` ([linhas 45 e 68–69](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/curar_perguntas_em_limbo.py:45)). Há também chamadas em testes, por exemplo [test_e4_expand_orm.py:182](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e4_expand_orm.py:182).

   **Já existem testes relacionados**, mas a existência de um selo que garanta a ausência de *toda* leitura de `DecisaoPergunta` nesse sítio fica **[não verificado]**. A catraca atual inclui `marcos_do_lote` como `VALOR` para a “guarda de nulo do INSTANTE” ([test_e3_catraca_respondedores.py:505](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_e3_catraca_respondedores.py:505)); outro contrato o declara “marco de staleness: max(carimbo) por colab” ([test_contract_leitores_vitalidade.py:54](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_contract_leitores_vitalidade.py:54)).

   Aqui, **selo inverso** significa afirmar que o sítio **não lê** `DecisaoPergunta`; o mutante introduziria essa leitura e o selo deveria falhar. Por exemplo, inserir no corpo uma consulta a `DecisaoPergunta.objects` seria uma mudança concreta para o selo detectar. Isso é uma interpretação do requisito “`0 leitura de DecisaoPergunta, selo inverso`” do anexo `aval.txt`, item 2; a cobertura efetiva do futuro teste depende de sua implementação **[não verificado]**. [nao verificado]

   **Quem escreve e quem lê os valores — censo parcial obrigatório:**

   | Valor lido por `marcos_do_lote` | Escrita identificada | Leitura e autoridade |
   |---|---|---|
   | `Batida.criado_em` | Campo `auto_now_add=True` em [ponto/models.py:79](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/models.py:79); porta `registrar_batida` em [registro_batida.py:118](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/registro_batida.py:118). Outros escritores: **[não verificado]**. | Máximo por colaborador em [veredito_lavrado.py:177](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:177). |
   | `Batida.retratada_em` | `retratar_batida` passa `timezone.now()` à porta `anotar` em [registro_batida.py:289](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/registro_batida.py:289); o desfazer passa `None` em [retratar_humano.py:207](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/retratar_humano.py:207). Outros escritores: **[não verificado]**. | Máximo em [veredito_lavrado.py:180](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:180). |
   | `CelulaDia.veredito_em`, `regenerada_em`, `gerada_em`, `editada_em`, `julgada_em` | Respectivamente [ponto/portas/celula.py:1095](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/portas/celula.py:1095), [linha 263](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/portas/celula.py:263), `auto_now_add` em [escala/models.py:1702](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/escala/models.py:1702), [celula.py:645](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/portas/celula.py:645) e [linha 1194](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/portas/celula.py:1194). Outros sítios: **[não verificado]**. | `Greatest` dos cinco, depois `Max`, em [veredito_lavrado.py:191](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:191). Para o par de reescrita, `JUIZES.md` identifica `cobranca_orfa` e `carimbos_de_reescrita` ([linhas 44–49](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/JUIZES.md:44)). |
   | `PerguntaDisputa.materializada_em` | A porta atribui `self.materializada_em = quando` em [chamados/models.py:3295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3295); o próprio módulo registra que há outros escritores ([veredito_lavrado.py:197](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:197)). Lista completa: **[não verificado]**. | Máximo em [veredito_lavrado.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:201). |
   | `PerguntaDisputa.respondida_em` | Há atribuições em [chamados/models.py:3328](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3328) e [disputa_emissao.py:1705](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1705). Lista completa: **[não verificado]**. | Máximo em [veredito_lavrado.py:206](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:206). A pergunta “respondida?” tem autoridade própria em [JUIZES.md:102](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/JUIZES.md:102); o máximo aqui é marco, não julgamento de vitalidade ([test_contract_leitores_vitalidade.py:54](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_contract_leitores_vitalidade.py:54)). |

   Além desses campos, a função lê os IDs de colaborador, `ate` e a relação `disputa__colaborador_id` ([veredito_lavrado.py:151–167](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151), [linha 202](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:202)); seus escritores e demais leitores: **[não verificado]**. O resultado é usado por `esta_stale` ([linha 230](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:230)) e é chamado por `fio`, `raiox` e `tarefa_do_dia` ([fio.py:282](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:282), [raiox.py:38](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:38), [tarefa_do_dia.py:434](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:434)). Censo de **todos** os demais leitores de cada campo: **[não verificado]**.

2. **Colisão com aval ou lei — NAO PROCEDE como conclusão fechada.** O backlog da cópia registra “`marcos_do_lote ... nao muda; carimbo pela trilha FORA`” e “`depois de F9 e F5`” ([BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018)). O novo item 2 manda construir agora; a aplicação dessa ordem posterior à espera por F5 é uma inferência do texto fornecido (`aval.txt`, itens 1 e 2). A `ORDEM-VIVA-1511-NOITE` aparece como substituída pela ordem de 18/11 ([BACKLOG.md:2357](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2357)). O confronto literal completo com `SANDUICHE-DO-20` e `RETOMA-POS-CLEAR` permanece **[não verificado]**.

   O item 0 fornecido substitui **somente** a cláusula “`nada constroi / sem construtor ate o fim da simulacao`” dos avais nomeados. Continuam expressamente proibidos o pouso do que toca folha, export, retenção ou DiaPago antes do fim da simulação, mais de dois construtores e tomar a pista do comboio (`aval.txt`, item 0 e bloco `PROIBIDO`; sem linha fornecida). Outros `PROIBIDO` vigentes fora dos trechos conferidos: **[não verificado]**. [nao verificado]

3. **Condições literais — PROCEDE quanto à compatibilidade proposta; execução não verificada.** O mutante tem uma mudança concreta a detectar: acrescentar leitura de `DecisaoPergunta` ao corpo hoje mostrado em [veredito_lavrado.py:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151). Um teste externo que inspecione o sítio pode preservar “`leitor NAO muda`” (`aval.txt`, item 2). O mesmo item exige “`suite pre-opus na pista 2 atras do comboio`”; disponibilidade da pista, resultado da suíte, mutante morto e log em disco: **[não verificado]**. A construção ainda não está demonstrada nesta revisão **[não verificado]**.

4. **Alcance e arquivos — NAO PROCEDE como afirmação de delta já verificado.** O escopo declarado é “`SO o selo com mutante`”, com leitor e `carimbar_resposta_pela_trilha` fora (`aval.txt`, item 2); isso não declara alteração de competência exportada, dinheiro, motor, folha, export, DiaPago, escala ou vínculo. Porém, **quais arquivos concretos o selo e o mutante criarão ou tocarão, e se o delta real alcança algum desses domínios ou `!`, estão [não verificado]**: o item não fornece caminhos de saída para o teste ou para o log. O sítio inspecionado seria [app/chamados/veredito_lavrado.py](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151); o item determina que ele permaneça sem mudança (`aval.txt`, item 2).

**REVISAO item 2: PROCEDE 2 / NAO PROCEDE 2.**

## 19/11 01:26 · sessao ea68c280ade9 · ESCALA-NAO-DESCREVE · verificador · origem=Code

1. **PROCEDE, com ressalvas.** Os dois diffs fornecidos mostram alterações apenas em valores de `LINHAS_REGISTRADAS` no arquivo em conflito; não mostram mudança de lógica nesse arquivo. O diff da ESCALA registra `entradas_fora_do_inicio` de `[521]` para `[524]` e `assinaturas_do_colab` de `[170, 177]` para `[179, 186]`. Ambos correspondem a arquivos da lista de produção fornecida. Os anexos foram fornecidos no enunciado sem numeração de linhas; sua independência e completude em relação aos commits são **[não verificado]**. [nao verificado]

   O **MEDIDOR** é `core.medida_turno_do_par.censo(raiz)`: sua entrada recebe a pasta `app/`, lê os fontes e chama `censo_fontes` ([medida_turno_do_par.py:448](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/medida_turno_do_par.py:448)); o teste usa `cls.c = m.censo(_app())` e compara as linhas por igualdade ([test_medida_turno_do_par.py:502](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:502), [test_medida_turno_do_par.py:558](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:558)). Para obter o registro medido, executar dentro do `app/` da árvore desejada:

   ```bash
   PYTHONDONTWRITEBYTECODE=1 python3 -B -c "from core import medida_turno_do_par as m; from pprint import pprint; c=m.censo('.'); pprint({k:{(i['chave'], i.get('sitio', i['criterio'])): i['linhas'] for i in c[k]['itens']} for k in ('pareadores','derivacoes_de_dia')}, sort_dicts=False)"
   ```

   Rodei o medidor **na cópia disponível**, não numa árvore mergeada. Além de `autoridade_do_periodo` `[1029] → [1045]`, ele encontrou outras três divergências preexistentes na catraca: [nao verificado]

   | Item B1 | Registrado | Medido |
   |---|---:|---:|
   | `espelho.py::montar_dias` | `[426]` | `[440]` |
   | `turnos.py::_dia_das_batidas_puro` | `[2967]` | `[2971]` |
   | `turnos.py::dia_do_turno_da_batida` | `[2945]` | `[2949]` |

   Os registros estão em [test_medida_turno_do_par.py:324](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:324) e [test_medida_turno_do_par.py:336](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:336); as ocorrências reais estão em [espelho.py:1045](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/ponto/services/espelho.py:1045), [espelho.py:440](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/ponto/services/espelho.py:440), [turnos.py:2971](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/ponto/turnos.py:2971) e [turnos.py:2949](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/ponto/turnos.py:2949). A atribuição de cada deslocamento à ESPURIA-TELA por diff de commits é **[não verificado]**.

2. **NÃO PROCEDE a afirmação de colisão demonstrada com aval ou lei.** Os cortes vigentes da ESCALA tratam de botão, folga e ordenação ([CORTES.md:551](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/docs/CORTES.md:551)); o BACKLOG registra a obra e “pouso so depois de 19/11” ([BACKLOG.md:2341](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/docs/BACKLOG.md:2341)). A L-126 exige, entre outras condições, opus final e preserva proibições ao construtor ([LEIS.md:147](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/docs/LEIS.md:147)). O item 4 fornecido prevê opus final e restringe a resolução a não mudar `.py` de produção; não identifiquei colisão literal nesses pontos. Conformidade do **delta futuro** e condições efetivas de pouso: **[não verificado]**.

3. **NÃO PROCEDE que as condições sejam impossíveis no literal; PROCEDE a ressalva de escopo.** O item 4 fornecido exige “numeros da catraca saem do MEDIDOR rodado na arvore mergeada, nunca digitados” e “nenhum .py de producao muda” [anexo `aval.txt`, numeração de linha não fornecida]. Essas condições são compatíveis com atualizar o arquivo de teste a partir da medição da árvore mergeada ([test_medida_turno_do_par.py:17](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:17)).

   Contudo, **resolver só os trechos conflitantes não basta para tornar a catraca verde na cópia medida**: há quatro registros já defasados, inclusive `[1029] → [1045]`, listados acima. Se persistirem na árvore mergeada, o medidor exigirá também esses quatro ajustes ([test_medida_turno_do_par.py:558](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:558)). O texto “sobre a ponta” e “numeros ... na arvore mergeada” sustenta incluí-los na **atualização mecânica da catraca do item 4**; se a intenção era reservar os registros herdados à outra obra, o aval fornecido não o esclarece **[não verificado]**. Os valores finais após o merge também são **[não verificado]**.

4. **NÃO PROCEDE que o delta de resolução declarado toque, por si, competência exportada, dinheiro, motor, folha, export, DiaPago, dados de escala, vínculo ou `!` fora do declarado.** O item 4 fornecido limita o trabalho ao conflito do teste e diz “nenhum .py de producao muda” [anexo `aval.txt`, numeração de linha não fornecida]. Isso **não certifica a raia ESCALA inteira**: a lista fornecida inclui arquivos de produção, e seu conteúdo e efeitos no delta completo entre commits não foram disponibilizados **[não verificado]**. A L-126 mantém as proibições pertinentes ao construtor ([LEIS.md:147](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/docs/LEIS.md:147)).

A cópia disponível não tem metadados Git acessíveis; portanto, hash, merge-base, conflito efetivo e árvore mergeada não puderam ser verificados independentemente **[não verificado]**. Não executei a suite nem alterei arquivos. [nao verificado]

**REVISAO item 4: PROCEDE 1 / NAO PROCEDE 3.**

## 19/11 00:56 · sessao 8e6cc7d6cd1a · QUEIMA-B3 · censo · origem=Code

**O censo avançou, mas ainda não está pronto para abrir todas as fatias.** A cópia permite localizar os sítios listados e identificar mudanças relevantes, mas não permite verificar o hash informado por Git, os pousos em produção, os casos literais da tabela única nem o efeito concreto da futura F5 da espinha em cada leitor. O levantamento de leitores e escritores abaixo é **parcial**, não um censo exaustivo. [não verificado]

A correção mais relevante em relação à volta 1 é que **A5 mudou de premissa**, não apenas de linha: o laço agora usa `q_nao_validada_pelo_ato()`, enquanto o anexo antigo descrevia `validada_em__isnull=True`. E a F3 da espinha, conforme o corte registrado, **não altera `marcos_do_lote` nem passa a ler a trilha**: constrói um selo bitemporal com mutante. [models.py:315](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:315), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

**Critério dos sítios:** `CONFERE` significa que a operação indicada no anexo foi encontrada na linha indicada; `MUDOU` significa linha ou trecho diferente. A classificação não afirma que todos os efeitos e condições da tabela única foram reconferidos. Os anexos fornecem a atribuição dos códigos; onde não fornecem a tradução, ela fica [não verificado].

**Condições comuns.** A ordem fornecida é F0 → F1 → F2 → F3 → F4 → F5 → F6 → F7; a B2b espera F6F7, F9, F5 e F3 da espinha. O backlog também registra a espera, mas o estado real dos pousos está [não verificado]. Antes de cada abertura, é necessário reconferir as linhas no HEAD do dia. Cada fatia tem pouso e suíte próprios, teto de dez arquivos; F4 vai sozinha e F5 admite até dois pousos. [BACKLOG.md:826](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:826); demais condições: anexo `BRIEF-FATIAS.md` fornecido.

A matriz fornecida deriva **39 de 45** retirando P21, X1 duplicado de A9c, P14 duplicado de P13, B9a/B9b e B8; ela própria qualifica a derivação como contestável. B8 continua explicitamente no brief de F5: a dedução para a contagem **não o retira por si só da fatia**. A abrangência efetiva exigida para B8 deve ser reconferida na tabela única [não verificado].

### F0 — instrumentos desligados

- **Sítios:** o brief nomeia instrumentos F6, F8, F9/F9b e F4, sem mapeá-los a sítios concretos existentes; identidade e contratos futuros [não verificado]. A cerca atual tem `ContextVar` com `default=None` e a guarda retorna sem consultar fora de contexto; seu estado contém `vetadas`, `memo` e `empresa`, sem identificador de sítio. [fio.py:1381](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1381), [fio.py:1402](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1402), [fio.py:1445](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1445).
- **Campos e dependências:** a guarda lê `conversa`, `data_evento`, colaborador e empresa, inclusive dia gravado e proposto; leitores e escritores completos dos futuros instrumentos [não verificado]. [fio.py:1408](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1408).
- **RED candidato:** coletor desligado preserva retorno, escrita e exceção observáveis, inclusive fora da cerca. Casos literais e resultado [não verificado].
- **Após F5/F3 da espinha:** a F3 registrada mantém `marcos_do_lote`; o que a F5 mudará nos leitores concretos destes instrumentos futuros [não verificado]. [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F1 — carimbo no fecho

| Sítio | Conferência nesta cópia |
|---|---|
| X1 / A9c | **MUDOU:** mesmo laço S144, `p.carimbar` agora em [materializacao.py:2180](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2180). X1=A9c conforme matriz fornecida. |
| A9d | **CONFERE:** fecho por competência trancada em [competencia_trancada.py:229](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/competencia_trancada.py:229). |
| B1a / B1b | **CONFERE:** carimbo e fecho em [reconciliador.py:639](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:639) e [reconciliador.py:656](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:656). |
| B2a | **CONFERE:** carimbo e fecho em [reconciliador.py:407](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:407) e [reconciliador.py:414](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:414). |
| B3a / B3b | **CONFERE:** encerramento e fecho em [reconciliador.py:1192](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:1192) e [reconciliador.py:1216](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:1216). |
| X13 | **MUDOU:** fecho em [materializacao.py:2043](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2043). |
| X14 | **CONFERE:** fecho em [disputa_emissao.py:1298](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1298). |

- **Campos:** `carimbar` escreve `materializada_em`, `materializada_por`, `via_resolucao` e, conforme o argumento e texto, resposta e validação; pode relavrar. Um leitor de produção desses marcos é `marcos_do_lote`. O fecho escreve também os campos da disputa e, em certos motivos, registra SILÊNCIO. Censo completo dos demais escritores e leitores [não verificado]. [models.py:3299](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3299), [models.py:2080](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2080), [veredito_lavrado.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:201).
- **Dependência e RED:** depois de F0. Para A9d, candidato: disputa fecha, pergunta entregue que S144 tentaria carimbar permanece preservada e é contada. `RED-NOVO-2/3` são exigidos pelo brief, mas seus casos literais estão [não verificado]. [CORTES.md:556](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:556).
- **Após F5/F3 da espinha:** os marcos atuais seguem lendo os timestamps da projeção; a F3 prevista sela o comportamento bitemporal, sem adicionar leitura de `DecisaoPergunta`. Mudanças concretas nos leitores da F5 [não verificado]. [veredito_lavrado.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:201), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F2 — relavra

| Sítio | Conferência nesta cópia |
|---|---|
| A4 | **MUDOU:** `reconciliar` em [models.py:654](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:654), decisão e gate em [models.py:676](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:676). |
| A5 | **MUDOU:** laço usa face SQL da validação pelo ato, em [models.py:315](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:315). |
| A6 / A7 | **MUDOU:** entradas em [models.py:1742](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1742) e [models.py:1759](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1759). |
| B5 / B6 | **CONFERE:** `lavrar` salva em [veredito_lavrado.py:275](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:275); lote usa `bulk_update` em [veredito_lavrado.py:250](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:250). |
| B7 | **CONFERE:** save de relavra em [relavra_via.py:127](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/relavra_via.py:127). |

- **Campos:** `lavrar` lê o julgamento e `data_evento`, escreve `veredito_lavrado`, `julgado_em` e `janela_expira_em`; B7 também escreve `via_resolucao`. `esta_stale` e `marcos_do_lote` são leitores relevantes. B6 não passa pela guarda de `save`; inserção e migration histórica também são exceções apontadas pela matriz, sem pretensão de exaustividade. [veredito_lavrado.py:213](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:213), [veredito_lavrado.py:250](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:250), [veredito_lavrado.py:270](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:270), [relavra_via.py:127](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/relavra_via.py:127), [models.py:3209](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3209), [models.py:3674](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3674).
- **Dependência e RED:** depois de F1. Candidato: lote com entregue e aberta, preservando a primeira e relavrando a segunda; incluir B6 e a seleção pelo ato de A5. A equivalência ao `RED-NOVO-1` literal [não verificado].
- **Após F5/F3 da espinha:** a F3 mantém os marcos atuais e sela a foto bitemporal; a F5 pode alterar leitores de resposta, mas o diff e seus pontos exatos [não verificado]. [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F3 — reaponte da B2b

| Sítio | Conferência nesta cópia |
|---|---|
| B4 | **CONFERE:** atribuição e save em [fio.py:974](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:974). |
| B2b / B2d | **CONFERE:** chamadas em [reconciliador.py:998](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:998) e [reconciliador.py:1712](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:1712). |
| X6 / X7 | **MUDOU:** carimbo de duplicata em [fio.py:1502](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1502); reaponte da migração em [fio.py:2338](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:2338). |
| X8 | **MUDOU:** gate reaponta em [models.py:449](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:449). |
| X9 / X10 | **CONFERE:** chamadas em [limbo_folgas.py:245](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/limbo_folgas.py:245) e [limbo_folgas.py:283](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/limbo_folgas.py:283). |
| X12 | **CONFERE:** chamada do comando em [reatar_beco_folga_ao_limbo.py:135](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:135). |

- **Campos:** B4 escreve `chamado_destino` e a lavra, podendo datar a pergunta; X6 carimba identidade de duplicata e motivo. A guarda distingue conversa e lê dia anterior e proposto. Chamadores adicionais de produção incluem declaração humana: portanto, identificar o ator apenas pela porta B4 seria insuficiente. Censo completo dos leitores [não verificado]. [fio.py:955](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:955), [fio.py:974](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:974), [declaracao_texto.py:371](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:371), [fio.py:1408](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1408).
- **Dependência e RED:** depois de F2. Candidatos: dia anterior entregue versus dia novo aberto, e vice-versa; conversa; chamada humana e de sistema; duplicata preservada quando reaponte é recusado. Casos literais da tabela [não verificado].
- **Após F5/F3 da espinha:** o selo F3 protege a leitura bitemporal existente da lavra; o conjunto de leitores alterados pela F5 para cada chamador [não verificado]. [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F4 — laço de materialização

| Sítio | Conferência nesta cópia |
|---|---|
| P11 / P12 | **MUDOU:** save de pendência em [materializacao.py:1366](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1366); carimbo de paridade em [materializacao.py:1474](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1474). |
| P13 / P14 | **MUDOU:** ramos da mesma função em [materializacao.py:1699](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1699) e [materializacao.py:1703](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1703); P14=P13 na dedução da matriz fornecida. |
| P15 | **MUDOU:** carimbo em [materializacao.py:1981](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1981). |

- **Campos:** P11 escreve `pendencia_admin` e `recusa_motivo`; P13/P14 escrevem identidade ou motivo da recusa; P15 carimba materialização e pode escrever resposta/validação pela porta comum. O laço lê resposta, validação e materialização e relê sob trava. Outros escritores e leitores por campo [não verificado]. [materializacao.py:1366](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1366), [materializacao.py:1683](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1683), [materializacao.py:2100](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2100).
- **Dependência e RED:** depois de F3 da B2b e em pouso isolado. Candidato: uma pergunta retida e outra aplicável no mesmo laço; verificar resultado individual, fecho e o `finally` que tenta registrar recusa. Efeito exato da segunda exceção [não verificado]. [materializacao.py:1993](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1993).
- **Após F5/F3 da espinha:** a lei de resposta exige instante **e** texto também para materialização; a migração concreta dos leitores pela F5 e seu diff para este laço [não verificado]. A F3 mantém os marcos existentes. [LEIS.md:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/LEIS.md:151), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F5 — comandos e avulsos da B2b

| Sítio | Conferência nesta cópia |
|---|---|
| A8 | **MUDOU:** ressuscitação relê sob trava e grava compensação; o lote guarda `alvo_lido`, mas escreve só os IDs relidos. [models.py:1627](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1627), [models.py:1700](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1700). |
| P1 | **MUDOU:** save de reuso em [models.py:3198](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3198). |
| B8 | **CONFERE:** save em [fio.py:1092](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1092). |
| P3 | **MUDOU:** carimbo e fecho em [models.py:2224](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2224) e [models.py:2235](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2235). |
| P4 / P5 | **MUDOU:** portas em [models.py:3489](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3489) e [models.py:3530](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3530); correspondência completa dos chamadores e tratamento de exceção [não verificado]. |
| P17 | **MUDOU:** lote e células em [disputa_emissao.py:1656](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1656). |
| P18 / P19 | **MUDOU:** saves em [disputa_emissao.py:1685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1685) e [disputa_emissao.py:1707](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1707). |
| P20 | **CONFERE:** descarimbo em [relavra_via.py:246](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/relavra_via.py:246). |

- **Campos:** A8 e P17 limpam carimbo e estados de resposta/validação e registram decisões; P18 escreve `resposta_colab` e `respondida_em`; P19 escreve o instante apoiado no texto e registra ato; B8 escreve dia, célula e lavra. Os leitores e escritores completos de cada campo [não verificado]. [models.py:1700](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1700), [disputa_emissao.py:1640](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1640), [disputa_emissao.py:1680](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1680), [fio.py:1092](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1092).
- **Dependência e RED:** depois de F4; divisão em até dois pousos ainda [não verificado]. Candidatos: A8 com conjunto alterado entre leitura e trava, inclusive relido vazio; P17 com entregue e aberta sem trilha mentirosa; P18/P19 preservando o par legal; P4 com falha por item. Casos literais da tabela [não verificado].
- **Após F5/F3 da espinha:** A8 **já** lê `DecisaoPergunta.leitura` para a compensação nesta cópia; não atribuir essa leitura ao futuro pouso F5. O que cada outro sítio passará a ler pela F5 [não verificado]; F3 mantém os marcos. [models.py:1688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1688), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F6 — callback depois da cerca

- **X15 — MUDOU:** a evidência disponível identifica o problema como reset da cerca e callback, mas o anexo bruto não traduz X15 em função e linha. O reset atual ocorre no `finally` **depois** da saída do `atomic` interno; há callbacks `on_commit` em signals. A identidade do callback específico X15 permanece [não verificado]. [fio.py:1447](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1447), [fio.py:1459](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1459), [signals.py:240](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/signals.py:240), [RELATO.md:326](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/RELATO.md:326).
- **Campos:** estado transitório `vetadas`, `memo`, `empresa`; campos da pergunta efetivamente tocados pelo callback X15 e respectivos leitores/escritores [não verificado]. [fio.py:1445](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1445).
- **Dependência e RED:** depois de F5 da B2b. Candidato: callback executado no commit observa cerca já encerrada, sem herdar veto/memo do ato anterior; cobrir também transação externa e aninhamento. Caso literal [não verificado]. [fio.py:1431

[truncado em 20000 de 24084 caracteres]

## 19/11 00:54 · sessao 87eee57fdaea · QUEIMA-B2-chamado · censo · origem=Code

O script abaixo produz um inventário **sintático e conservador**; sua execução e seus totais estão [não verificado]. Os quatro campos selecionados correspondem às declarações em [app/chamados/models.py:58](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:58). O formato segue o molde fornecido no enunciado [não verificado].

```python
#!/usr/bin/env python3
# Uso: python3 -B script.py RAIZ
# Somente leitura; RAIZ deve conter app/.
import ast
import collections
import os
import re
import sys
import tokenize

sys.dont_write_bytecode = True

if len(sys.argv) != 2:
    sys.stderr.write("Uso: python3 -B script.py RAIZ\n")
    sys.exit(2)

RAIZ = os.path.abspath(sys.argv[1])
APP = os.path.join(RAIZ, "app")
if not os.path.isdir(APP):
    sys.stderr.write("ERRO: RAIZ deve conter app/\n")
    sys.exit(2)

CAMPOS = (
    "status_local",
    "via_encerramento",
    "respondido_por",
    "resolvido_em",
)
ALIAS = {campo: campo for campo in CAMPOS}
ALIAS["respondido_por_id"] = "respondido_por"

PADRAO = re.compile(
    r"(?<![A-Za-z0-9_])(?:"
    + "|".join(re.escape(c) for c in sorted(ALIAS, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)
SQL = re.compile(
    r"\b(select|insert|update|delete|with|returning|where|from|join)\b",
    re.I,
)
TAGS = re.compile(r"{{.*?}}|{%.*?%}", re.S)
ATRIBUTO_TEMPLATE = re.compile(
    r"\.\s*("
    + "|".join(re.escape(c) for c in sorted(ALIAS, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)

LEITURAS = {
    "filter", "exclude", "get", "values", "values_list", "order_by",
    "annotate", "alias", "only", "defer", "distinct", "earliest", "latest",
}
CADEIA = LEITURAS | {
    "all", "none", "select_for_update", "select_related", "prefetch_related",
    "using", "reverse", "iterator", "first", "last", "create",
    "get_or_create", "update_or_create", "update", "bulk_create", "bulk_update",
}
ESCOPOS = (ast.Module, ast.ClassDef, ast.FunctionDef, ast.AsyncFunctionDef,
           ast.Lambda)
FUNCOES = (ast.FunctionDef, ast.AsyncFunctionDef, ast.Lambda)
REGISTROS = set()
FALHAS = []
ARVORES = []
MODELOS = {"ChamadoColaborador"}


def limpar(valor):
    return str(valor).replace("\t", " ").replace("\r", " ").replace("\n", " ")


def categoria(relativo):
    partes = relativo.lower().replace("\\", "/").split("/")
    nome = partes[-1]
    if "migrations" in partes:
        return "migration"
    if ("tests" in partes or "test" in partes or nome.startswith("test_")
            or nome.endswith("_test.py")):
        return "teste" [nao verificado]
    return "prod"


def chamada(no):
    if not isinstance(no, ast.Call):
        return ""
    if isinstance(no.func, ast.Name):
        return no.func.id
    if isinstance(no.func, ast.Attribute):
        return no.func.attr
    return ""


def ocorrencias(texto):
    for achado in PADRAO.finditer(texto):
        yield ALIAS[achado.group()], achado.start()


def campo_caminho(texto):
    """Retorna os campos presentes em segmentos Django, inclusive lookups."""
    if not isinstance(texto, str):
        return []
    partes = texto.lstrip("-").split("__")
    return [(ALIAS[p], i) for i, p in enumerate(partes) if p in ALIAS]


def nome_base(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return ""


def emitir(arquivo, linha, funcao, campo, papel, escopo, receptor, motivo, forma):
    REGISTROS.add((
        arquivo, int(linha), funcao or "<modulo>", campo, papel, escopo,
        receptor, motivo, forma,
    ))


def filhos_sem_escopos(no):
    pilha = list(ast.iter_child_nodes(no))
    while pilha:
        atual = pilha.pop()
        yield atual
        if not isinstance(atual, ESCOPOS):
            pilha.extend(ast.iter_child_nodes(atual))


# Examina a cópia inteira, incluindo testes e templates fora de app/. [nao verificado]
# Diretórios de metadados e caches são ignorados.
IGNORAR_DIRS = {".git", "__pycache__", ".venv", "venv", "node_modules"}
TEMPLATES = []
for diretorio, dirs, arquivos in os.walk(RAIZ, followlinks=False):
    dirs[:] = sorted(d for d in dirs if d not in IGNORAR_DIRS)
    for nome in sorted(arquivos):
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        if nome.endswith((".html", ".htm", ".django", ".jinja", ".jinja2")):
            TEMPLATES.append((relativo, caminho))
        if not nome.endswith(".py"):
            continue
        try:
            with tokenize.open(caminho) as arquivo:
                fonte = arquivo.read()
            arvore = ast.parse(fonte, filename=relativo)
        except (OSError, SyntaxError, UnicodeError, LookupError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        ARVORES.append((relativo, arvore))
        for no in ast.walk(arvore):
            if (isinstance(no, ast.ClassDef)
                    and any(nome_base(base) == "Model" for base in no.bases)):
                MODELOS.add(no.name)


for relativo, arvore in ARVORES:
    escopo_arquivo = categoria(relativo)
    pais = {}
    ambientes = {}
    nomes_escopo = {}
    classes = {}

    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    def indexar(no, pilha, classe, ambiente):
        if isinstance(no, ast.ClassDef):
            pilha = pilha + [no.name]
            classe = no.name if no.name in MODELOS else None
            ambiente = no
        elif isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            pilha = pilha + [no.name]
            ambiente = no
        elif isinstance(no, ast.Lambda):
            pilha = pilha + ["<lambda>"]
            ambiente = no
        ambientes[no] = ambiente
        nomes_escopo[no] = ".".join(pilha) or "<modulo>"
        classes[no] = classe
        for filho in ast.iter_child_nodes(no):
            indexar(filho, pilha, classe, ambiente)

    indexar(arvore, [], None, arvore)

    atribuicoes = collections.defaultdict(lambda: collections.defaultdict(list))
    anotacoes = collections.defaultdict(lambda: collections.defaultdict(list))
    acrescentados = collections.defaultdict(lambda: collections.defaultdict(list))

    for ambiente in [n for n in ast.walk(arvore) if isinstance(n, ESCOPOS)]:
        if isinstance(ambiente, FUNCOES):
            args = ambiente.args
            todos = list(args.posonlyargs) + list(args.args) + list(args.kwonlyargs)
            if args.vararg:
                todos.append(args.vararg)
            if args.kwarg:
                todos.append(args.kwarg)
            for arg in todos:
                if arg.annotation is not None:
                    anotacoes[ambiente][arg.arg].append(arg.annotation)

        for no in filhos_sem_escopos(ambiente):
            if ambientes.get(no) is not ambiente:
                continue
            if isinstance(no, ast.Assign):
                for alvo in no.targets:
                    if isinstance(alvo, ast.Name):
                        atribuicoes[ambiente][alvo.id].append(no.value)
            elif isinstance(no, ast.AnnAssign) and isinstance(no.target, ast.Name):
                anotacoes[ambiente][no.target.id].append(no.annotation)
                if no.value is not None:
                    atribuicoes[ambiente][no.target.id].append(no.value)
            elif isinstance(no, (ast.For, ast.AsyncFor)) and isinstance(no.target, ast.Name):
                atribuicoes[ambiente][no.target.id].append(no.iter)
            elif (isinstance(no, ast.Call) and isinstance(no.func, ast.Attribute)
                  and isinstance(no.func.value, ast.Name)
                  and no.func.attr in ("append", "extend") and no.args):
                acrescentados[ambiente][no.func.value.id].append(no.args[0])

    def modelo_anotacao(no):
        # Somente uma anotação direta de modelo é tratada como prova.
        if isinstance(no, ast.Name) and no.id in MODELOS:
            return no.id
        if isinstance(no, ast.Attribute) and no.attr in MODELOS:
            return no.attr
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            partes = no.value.split(".")
            if partes[-1] in MODELOS and all(p.isidentifier() for p in partes):
                return partes[-1]
        return None

    def inferir(no, contexto, visitados=None):
        if no is None:
            return None
        if visitados is None:
            visitados = set()
        ambiente = ambientes.get(contexto, arvore)
        if isinstance(no, ast.Name):
            if no.id == "self" and classes.get(contexto):
                return classes[contexto]
            if no.id in MODELOS:
                if no.id not in atribuicoes[ambiente]:
                    return no.id
                return None
            chave = (id(ambiente), no.id)
            if chave in visitados:
                return None
            visitados = visitados | {chave}
            anns = anotacoes[ambiente].get(no.id, [])
            vals = atribuicoes[ambiente].get(no.id, [])
            resultados = [modelo_anotacao(x) for x in anns]
            resultados += [inferir(x, x, visitados) for x in vals]
            if resultados and None not in resultados and len(set(resultados)) == 1:
                return resultados[0]
            return None
        if isinstance(no, ast.Attribute):
            if no.attr in MODELOS and isinstance(no.value, ast.Name):
                return no.attr
            if no.attr in ("objects", "_default_manager", "_base_manager"):
                return inferir(no.value, contexto, visitados)
            return None
        if isinstance(no, ast.Subscript):
            return inferir(no.value, contexto, visitados)
        if isinstance(no, ast.Call):
            if chamada(no) == "type" and len(no.args) == 1:
                return inferir(no.args[0], contexto, visitados)
            if (chamada(no) == "get_model" and no.args
                    and isinstance(no.args[-1], ast.Constant)
                    and isinstance(no.args[-1].value, str)):
                nome = no.args[-1].value.split(".")[-1]
                return nome if nome in MODELOS else None
            if isinstance(no.func, ast.Name) and no.func.id in MODELOS:
                return inferir(no.func, contexto, visitados)
            if isinstance(no.func, ast.Attribute):
                if no.func.attr in MODELOS:
                    return inferir(no.func, contexto, visitados)
                if no.func.attr in CADEIA:
                    return inferir(no.func.value, contexto, visitados)
        return None

    def receptor(no, contexto, caminho=None):
        if caminho and any(pos > 0 for _, pos in campo_caminho(caminho)):
            return "AMBIGUO", "caminho de relacao nao resolvido"
        modelo = inferir(no, contexto)
        if modelo:
            return "RECEPTOR=" + modelo, "inferencia sintatica"
        return "AMBIGUO", "receptor nao resolvido"

    def receptor_chamada(call, contexto, caminho=None):
        if isinstance(call, ast.Call) and isinstance(call.func, ast.Attribute):
            return receptor(call.func.value, contexto, caminho)
        return "AMBIGUO", "chamada sem receptor de modelo"

    def registrar(no, campo, papeis, forma, rec=None, motivo=None, linha=None):
        if rec is None:
            rec, motivo = "AMBIGUO", "receptor nao resolvido"
        for papel in papeis:
            emitir(
                relativo, linha or getattr(no, "lineno", 1),
                nomes_escopo.get(no, "<modulo>"), campo, papel,
                escopo_arquivo, rec, motivo or "", forma,
            )

    def chamada_externa(no):
        atual = no
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, ast.Call) and chamada(atual) in CADEIA:
                return atual
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        return None

    def literais_resolvidos(no, contexto, visitados=None):
        """Reúne literais visíveis; não afirma que o fluxo os executa."""
        if visitados is None:
            visitados = set()
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            yield no
        elif isinstance(no, (ast.List, ast.Tuple, ast.Set)):
            for item in no.elts:
                yield from literais_resolvidos(item, contexto, visitados)
        elif isinstance(no, ast.Dict):
            for chave in no.keys:
                if chave is not None:
                    yield from literais_resolvidos(chave, contexto, visitados)
        elif isinstance(no, ast.Name):
            ambiente = ambientes.get(contexto, arvore)
            chave = (id(ambiente), no.id)
            if chave in visitados:
                return
            visitados = visitados | {chave}
            for valor in (atribuicoes[ambiente].get(no.id, [])
                          + acrescentados[ambiente].get(no.id, [])):
                yield from literais_resolvidos(valor, valor, visitados)
        elif isinstance(no, ast.BinOp) and isinstance(no.op, ast.Add):
            yield from literais_resolvidos(no.left, contexto, visitados)
            yield from literais_resolvidos(no.right, contexto, visitados)

    def registrar_lista(call, valor, forma):
        rec, motivo = receptor_chamada(call, call)
        for literal in literais_resolvidos(valor, call):
            for campo, _ in campo_caminho(literal.value):
                registrar(call, campo, ("ESCRITA",), forma, rec, motivo)

    def classificar_string(no):
        valor = no.value
        atual = no
        palavra = None
        chave_dicionario = False
        while atual in pais:
            anterior = atual
            atual = pais[atual]
            if isinstance(atual, ast.Dict) and anterior in atual.keys:
                chave_dicionario = True
            if isinstance(atual, ast.keyword) and palavra is None:
                palavra = atual.arg
            if isinstance(atual, ast.Call):
                nome = chamada(atual)
                if nome in ("getattr", "setattr") and len(atual.args) >= 2:
                    if atual.args[1] is no:
                        rec, motivo = receptor(atual.args[0], no)
                        papel = "LEITURA" if nome == "getattr" else "ESCRITA"
                        return (papel,), nome, rec, motivo
                if nome in ("F", "OuterRef"):
                    externo = chamada_externa(atual)
                    rec, motivo = receptor_chamada(externo, no, valor)
                    return ("LEITURA",), nome, rec, motivo
                if nome in ("raw", "execute", "executemany") and SQL.search(valor):
                    return (("LEITURA", "ESCRITA"), "SQL cru",
                            "AMBIGUO", "SQL nao interpretado")
                if nome == "save" and palavra == "update_fields":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "save(update_fields)", rec, motivo
                if nome == "bulk_update":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "bulk_update(fields)", rec, motivo
                if nome == "bulk_create" and palavra in ("update_fields", "unique_fields"):
                    rec, motivo = receptor_chamada(atual, no)
                    papeis = ("ESCRITA",) if palavra == "update_fields" else ("LEITURA",)
                    return papeis, "bulk_create(" + palavra + ")", rec, motivo
                if chave_dicionario and nome in ("create", "update"):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    return ("ESCRITA",), nome + " dicionario", rec, motivo
                if chave_dicionario and nome in ("get_or_create", "update_or_create"):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    if palavra in ("defaults", "create_defaults"):
                        return ("ESCRITA",), nome + " defaults", rec, motivo
                    return ("LEITURA", "ESCRITA"), nome + " dicionario", rec, motivo
                if nome in LEITURAS and campo_caminho(valor):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    return ("LEITURA",), nome, rec, motivo
                if nome == "Q" and campo_caminho(valor):
                    externo = chamada_externa(atual)
                    rec, motivo = receptor_chamada(externo, no, valor)
                    return ("LEITURA",), "Q", rec, motivo
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        if SQL.search(valor):
            return (("LEITURA", "ESCRITA"), "possivel SQL",
                    "AMBIGUO", "string SQL nao interpretada")
        return (("LEITURA", "ESCRITA"), "string fora de contexto reconhecido",
                "AMBIGUO", "uso da string nao resolvido")

    for no in ast.walk(arvore):
        if isinstance(no, ast.Call):
            nome = chamada(no)
            if nome == "save":
                for kw in no.keywords:
                    if kw.arg == "update_fields":
                        registrar_lista(no, kw.value, "save(update_fields) resolvido")
            elif nome == "bulk_update":
                if len(no.args) >= 2:
                    registrar_lista(no, no.args[1], "bulk_update(fields) resolvido")
                for kw in no.keywords:
                    if kw.arg == "fields":
                        registrar_lista(no, kw.value, "bulk_update(fields) resolvido")
            elif nome == "bulk_create":
                for kw in no.keywords:
                    if kw.arg == "update_fields":
                        registrar_lista(no, kw.value, "bulk_create(update_fields) resolvido")
            elif nome in ("create", "update", "get_or_create", "update_or_create"):
                for kw in no.keywords:
                    if kw.arg is None or kw.arg in ("defaults", "create_defaults"):
                        rec, motivo = receptor_chamada(no, no)
                        for literal in literais_resolvidos(kw.value, no):
                            for campo, pos in campo_caminho(literal.value):
                                r, m = (rec, motivo) if pos == 0 else (
                                    "AMBIGUO", "caminho de relacao nao resolvido"
                                )
                                papeis = ("ESCRITA",)
                                if kw.arg is None and nome in (
                                        "get_or_create", "update_or_create"):
                                    papeis = ("LEITURA", "ESCRITA")
                                registrar(no, campo, papeis,
                                          nome + " argumentos resolvidos", r, m)

        if isinstance(no, ast.Attribute) and no.attr in ALIAS:
            campo = ALIAS[no.attr]
            rec, motivo = receptor(no.value, no)
            if isinstance(no.ctx, ast.Store):
                papeis = ("ESCRITA",)
                if isinstance(pais.get(no), ast.AugAssign):
                    papeis = ("LEITURA", "ESCRITA")
            elif isinstance(no.ctx, ast.Del):
                papeis = ("ESCRITA",)
            else:
                papeis = ("LEITURA",)
            registrar(no, campo, papeis, "atributo", rec, motivo)

        elif isinstance(no, ast.keyword) and no.arg:
            campos = campo_caminho(no.arg)
            if not campos:
                continue
            call = pais.get(no)
            nome = chamada(call)
            for campo, _ in campos:
                if nome in ("Q", "When"):
                    externo = chamada_externa(call)
                    rec, motivo = receptor_chamada(externo, no, no.arg)
                    papeis = ("LEITURA",)
                elif nome in ("create", "update"):
                    rec, motivo = receptor_chamada(call, no, no.arg)
                    papeis = (

[truncado em 20000 de 23752 caracteres]

## 19/11 00:54 · sessao 13f739a36bbe · QUEIMA-B2-pergunta · censo · origem=Code

```python
#!/usr/bin/env python3
# Uso: python3 -B script.py RAIZ
# RAIZ deve conter app/. O programa apenas le arquivos.
import ast
import collections
import os
import re
import sys
import tokenize

sys.dont_write_bytecode = True

if len(sys.argv) != 2:
    sys.stderr.write("Uso: python3 -B script.py RAIZ\n")
    sys.exit(2)

RAIZ = os.path.abspath(sys.argv[1])
APP = os.path.join(RAIZ, "app")
if not os.path.isdir(APP):
    sys.stderr.write("ERRO: RAIZ deve conter app/\n")
    sys.exit(2)

CAMPOS = (
    "resposta_colab", "respondida_em", "validada_em", "validada_por",
    "data_conferida_em", "data_conferida_por", "pendencia_admin",
    "recusa_motivo", "via_resolucao", "veredito_lavrado", "julgado_em",
    "janela_expira_em", "materializada_em", "materializada_por",
)
ALIASES = {campo: campo for campo in CAMPOS}
for campo in ("validada_por", "data_conferida_por", "materializada_por"):
    ALIASES[campo + "_id"] = campo

PADRAO = re.compile(
    r"(?<![A-Za-z0-9_])(?:"
    + "|".join(re.escape(c) for c in sorted(ALIASES, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)
SQL = re.compile(
    r"\b(select|insert|update|delete|with|returning|where|from|join)\b",
    re.I,
)
TAGS = re.compile(r"{{.*?}}|{%.*?%}", re.S)
LEITURAS = {
    "filter", "exclude", "get", "values", "values_list", "order_by",
    "annotate", "alias", "only", "defer", "distinct", "earliest", "latest",
}
QUERYSET = LEITURAS | {
    "all", "none", "select_for_update", "select_related",
    "prefetch_related", "using", "reverse", "iterator", "first", "last",
    "create", "update", "get_or_create", "update_or_create",
    "bulk_create", "bulk_update",
}
ESCOPOS = (
    ast.Module, ast.ClassDef, ast.FunctionDef, ast.AsyncFunctionDef, ast.Lambda
)
REGISTROS = set()
FALHAS = []
ARVORES = []
MODELOS = {"PerguntaDisputa", "RegularizacaoExterna"}


def limpar(valor):
    return str(valor).replace("\t", " ").replace("\r", " ").replace("\n", " ")


def categoria(relativo):
    partes = relativo.lower().replace("\\", "/").split("/")
    nome = partes[-1]
    if "migrations" in partes:
        return "migration"
    if ("tests" in partes or "test" in partes
            or nome.startswith("test_") or nome.endswith("_test.py")):
        return "teste" [nao verificado]
    return "prod"


def nome_chamada(no):
    if not isinstance(no, ast.Call):
        return ""
    if isinstance(no.func, ast.Name):
        return no.func.id
    if isinstance(no.func, ast.Attribute):
        return no.func.attr
    return ""


def nome_base(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return ""


def campo_caminho(texto):
    """Aceita um campo ou caminho ORM literal, com lookups e ordenacao."""
    if not isinstance(texto, str):
        return []
    partes = texto.lstrip("-").split("__")
    return [(ALIASES[p], i) for i, p in enumerate(partes) if p in ALIASES]


def achados(texto):
    if isinstance(texto, str):
        for m in PADRAO.finditer(texto):
            yield ALIASES[m.group()], m.start()


def emitir(arquivo, linha, funcao, campo, papel, escopo, rec, motivo, forma):
    REGISTROS.add((
        arquivo, int(linha), funcao or "<modulo>", campo, papel, escopo,
        rec, motivo, forma,
    ))


# Primeiro passe: fontes Python e nomes de modelos declarados.
for diretorio, dirs, arquivos in os.walk(APP):
    dirs.sort()
    for nome in sorted(arquivos):
        if not nome.endswith(".py"):
            continue
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        try:
            with tokenize.open(caminho) as arquivo:
                fonte = arquivo.read()
            arvore = ast.parse(fonte, filename=relativo)
        except (OSError, SyntaxError, UnicodeError, LookupError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        ARVORES.append((relativo, arvore))
        for no in ast.walk(arvore):
            if isinstance(no, ast.ClassDef):
                if any(nome_base(base) == "Model" for base in no.bases):
                    MODELOS.add(no.name)


for relativo, arvore in ARVORES:
    escopo_arquivo = categoria(relativo)
    pais = {}
    ambientes = {}
    funcoes = {}
    classes = {}

    def indexar(no, ambiente, pilha, classe):
        if isinstance(no, ast.ClassDef):
            ambiente = no
            pilha = pilha + [no.name]
            classe = no.name if no.name in MODELOS else None
        elif isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            ambiente = no
            pilha = pilha + [no.name]
        elif isinstance(no, ast.Lambda):
            ambiente = no
            pilha = pilha + ["<lambda>"]
        ambientes[no] = ambiente
        funcoes[no] = ".".join(pilha) or "<modulo>"
        classes[no] = classe
        for filho in ast.iter_child_nodes(no):
            pais[filho] = no
            indexar(filho, ambiente, pilha, classe)

    indexar(arvore, arvore, [], None)

    atribuicoes = collections.defaultdict(lambda: collections.defaultdict(list))
    anotacoes = collections.defaultdict(lambda: collections.defaultdict(list))

    for no in ast.walk(arvore):
        ambiente = ambientes[no]
        if isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef, ast.Lambda)):
            args = no.args
            todos = list(args.posonlyargs) + list(args.args) + list(args.kwonlyargs)
            if args.vararg:
                todos.append(args.vararg)
            if args.kwarg:
                todos.append(args.kwarg)
            for arg in todos:
                if arg.annotation is not None:
                    anotacoes[no][arg.arg].append(arg.annotation)
        if isinstance(no, ast.Assign):
            for alvo in no.targets:
                if isinstance(alvo, ast.Name):
                    atribuicoes[ambiente][alvo.id].append(no.value)
        elif isinstance(no, ast.AnnAssign) and isinstance(no.target, ast.Name):
            anotacoes[ambiente][no.target.id].append(no.annotation)
            if no.value is not None:
                atribuicoes[ambiente][no.target.id].append(no.value)
        elif isinstance(no, (ast.For, ast.AsyncFor)):
            if isinstance(no.target, ast.Name):
                atribuicoes[ambiente][no.target.id].append(no.iter)
        elif isinstance(no, ast.comprehension) and isinstance(no.target, ast.Name):
            atribuicoes[ambiente][no.target.id].append(no.iter)

    def modelo_anotacao(no):
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            nomes = set(re.findall(r"[A-Za-z_]\w*", no.value))
        else:
            nomes = {
                x.id if isinstance(x, ast.Name) else x.attr
                for x in ast.walk(no)
                if isinstance(x, (ast.Name, ast.Attribute))
            }
        encontrados = nomes & MODELOS
        return next(iter(encontrados)) if len(encontrados) == 1 else None

    def inferir(no, contexto, visitados=None):
        """Inferencia local; nao executa codigo nem resolve relacoes Django."""
        if no is None:
            return None
        visitados = set() if visitados is None else visitados
        ambiente = ambientes.get(contexto, arvore)
        if isinstance(no, ast.Name):
            if no.id == "self" and classes.get(contexto):
                return classes[contexto]
            if no.id == "cls" and classes.get(contexto):
                if isinstance(ambiente, (ast.FunctionDef, ast.AsyncFunctionDef)):
                    if any(nome_base(d) == "classmethod" for d in ambiente.decorator_list):
                        return classes[contexto]
            if no.id in MODELOS and no.id not in atribuicoes[ambiente]:
                return no.id
            chave = (id(ambiente), no.id)
            if chave in visitados:
                return None
            visitados = visitados | {chave}
            resultados = [
                modelo_anotacao(x) for x in anotacoes[ambiente].get(no.id, [])
            ]
            resultados += [
                inferir(x, x, visitados)
                for x in atribuicoes[ambiente].get(no.id, [])
            ]
            if resultados and None not in resultados and len(set(resultados)) == 1:
                return resultados[0]
            return None
        if isinstance(no, ast.Attribute):
            if no.attr in MODELOS and isinstance(no.value, ast.Name):
                return no.attr
            if no.attr in ("objects", "_default_manager", "_base_manager"):
                return inferir(no.value, contexto, visitados)
            return None
        if isinstance(no, ast.Subscript):
            return inferir(no.value, contexto, visitados)
        if isinstance(no, ast.Call):
            if isinstance(no.func, ast.Name) and no.func.id in MODELOS:
                return inferir(no.func, contexto, visitados)
            if isinstance(no.func, ast.Attribute):
                if no.func.attr in MODELOS:
                    return inferir(no.func, contexto, visitados)
                if no.func.attr in QUERYSET:
                    return inferir(no.func.value, contexto, visitados)
        return None

    def receptor(no, contexto, caminho=None):
        if caminho:
            if any(posicao > 0 for _, posicao in campo_caminho(caminho)):
                return "AMBIGUO", "caminho de relacao nao resolvido"
        modelo = inferir(no, contexto)
        if modelo:
            return "RECEPTOR=" + modelo, "inferencia sintatica local"
        return "AMBIGUO", "receptor nao resolvido"

    def receptor_call(call, contexto, caminho=None):
        if isinstance(call, ast.Call) and isinstance(call.func, ast.Attribute):
            return receptor(call.func.value, contexto, caminho)
        return "AMBIGUO", "chamada sem receptor resolvido"

    def registrar(no, campo, papeis, forma, rec, motivo, linha=None):
        for papel in papeis:
            emitir(
                relativo, linha or getattr(no, "lineno", 1),
                funcoes.get(no, "<modulo>"), campo, papel, escopo_arquivo,
                rec, motivo, forma,
            )

    def literais(no, contexto, visitados=None):
        """Segue apenas nomes com uma unica atribuicao local."""
        visitados = set() if visitados is None else visitados
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            yield no.value, no
        elif isinstance(no, (ast.List, ast.Tuple, ast.Set)):
            for item in no.elts:
                yield from literais(item, contexto, visitados)
        elif isinstance(no, ast.Name):
            ambiente = ambientes.get(contexto, arvore)
            chave = (id(ambiente), no.id)
            valores = atribuicoes[ambiente].get(no.id, [])
            if chave not in visitados and len(valores) == 1:
                yield from literais(valores[0], contexto, visitados | {chave})

    def pares_dict(no, contexto, visitados=None):
        """Extrai chaves literais, inclusive de **dicionario local simples."""
        visitados = set() if visitados is None else visitados
        if isinstance(no, ast.Dict):
            for chave, valor in zip(no.keys, no.values):
                if chave is None:
                    yield from pares_dict(valor, contexto, visitados)
                elif isinstance(chave, ast.Constant) and isinstance(chave.value, str):
                    yield chave.value, chave
        elif isinstance(no, ast.Name):
            ambiente = ambientes.get(contexto, arvore)
            chave = (id(ambiente), no.id)
            valores = atribuicoes[ambiente].get(no.id, [])
            if chave not in visitados and len(valores) == 1:
                yield from pares_dict(valores[0], contexto, visitados | {chave})

    def registrar_caminho(no, texto, papeis, forma, rec_no, contexto):
        for campo, _ in campo_caminho(texto):
            rec, motivo = receptor(rec_no, contexto, texto)
            registrar(no, campo, papeis, forma, rec, motivo)

    def chamada_orm_externa(no):
        atual = no
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, ast.Call) and isinstance(atual.func, ast.Attribute):
                if nome_chamada(atual) in QUERYSET:
                    return atual
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        return None

    for no in ast.walk(arvore):
        if isinstance(no, ast.Attribute) and no.attr in ALIASES:
            rec, motivo = receptor(no.value, no)
            if isinstance(no.ctx, ast.Store):
                papeis = ("ESCRITA",)
                if isinstance(pais.get(no), ast.AugAssign):
                    papeis = ("LEITURA", "ESCRITA")
            elif isinstance(no.ctx, ast.Del):
                papeis = ("ESCRITA",)
            else:
                papeis = ("LEITURA",)
            registrar(no, ALIASES[no.attr], papeis, "atributo", rec, motivo)

        if not isinstance(no, ast.Call):
            continue

        nome = nome_chamada(no)
        rec_no = no.func.value if isinstance(no.func, ast.Attribute) else None

        if nome in ("getattr", "setattr") and len(no.args) >= 2:
            papel = "LEITURA" if nome == "getattr" else "ESCRITA"
            for texto, origem in literais(no.args[1], no):
                for campo, pos in campo_caminho(texto):
                    rec, motivo = receptor(no.args[0], no, texto)
                    registrar(no, campo, (papel,), nome, rec, motivo)

        if nome in LEITURAS:
            for arg in no.args:
                for texto, origem in literais(arg, no):
                    registrar_caminho(
                        no, texto, ("LEITURA",), nome + " argumento",
                        rec_no, no,
                    )

        if nome in ("F", "Q"):
            externo = chamada_orm_externa(no)
            externo_rec = (
                externo.func.value
                if externo is not None and isinstance(externo.func, ast.Attribute)
                else None
            )
            if nome == "F":
                for arg in no.args:
                    for texto, origem in literais(arg, no):
                        registrar_caminho(
                            no, texto, ("LEITURA",), "F",
                            externo_rec, no,
                        )
            for kw in no.keywords:
                if kw.arg:
                    registrar_caminho(
                        no, kw.arg, ("LEITURA",), nome + " kwarg",
                        externo_rec, no,
                    )

        if nome in ("save", "bulk_update", "bulk_create"):
            argumentos_campos = []
            if nome == "save":
                argumentos_campos += [
                    kw.value for kw in no.keywords if kw.arg == "update_fields"
                ]
            elif nome == "bulk_update":
                if len(no.args) >= 2:
                    argumentos_campos.append(no.args[1])
                argumentos_campos += [
                    kw.value for kw in no.keywords if kw.arg == "fields"
                ]
            else:
                argumentos_campos += [
                    kw.value for kw in no.keywords
                    if kw.arg in ("update_fields", "unique_fields")
                ]
            for argumento in argumentos_campos:
                for texto, origem in literais(argumento, no):
                    papel = (
                        "LEITURA"
                        if nome == "bulk_create" and any(
                            kw.arg == "unique_fields" and kw.value is argumento
                            for kw in no.keywords
                        )
                        else "ESCRITA"
                    )
                    registrar_caminho(
                        no, texto, (papel,), nome + " campos", rec_no, no,
                    )

        for kw in no.keywords:
            if kw.arg is None:
                if nome in ("update", "create", "get_or_create", "update_or_create"):
                    for texto, origem in pares_dict(kw.value, no):
                        papeis = (
                            ("ESCRITA",) if nome in ("update", "create")
                            else ("LEITURA", "ESCRITA")
                        )
                        registrar_caminho(
                            no, texto, papeis, nome + " **dict", rec_no, no,
                        )
                continue

            if nome in ("get_or_create", "update_or_create") and kw.arg in (
                "defaults", "create_defaults"
            ):
                for texto, origem in pares_dict(kw.value, no):
                    registrar_caminho(
                        no, texto, ("ESCRITA",), nome + " " + kw.arg,
                        rec_no, no,
                    )
                continue

            if nome in ("Q", "F"):
                continue
            if nome in ("create", "update"):
                papeis = ("ESCRITA",)
            elif nome in ("get_or_create", "update_or_create"):
                papeis = ("LEITURA", "ESCRITA")
            elif nome in LEITURAS:
                papeis = ("LEITURA",)
            elif nome in MODELOS:
                papeis = ("ESCRITA",)
                rec_no = no.func
            else:
                continue
            registrar_caminho(no, kw.arg, papeis, nome + " kwarg", rec_no, no)

        # Um bulk_create sem lista explicita de campos pode inserir todas as
        # colunas. A evidencia sintatica isolada nao prova quais valores foram
        # enviados, portanto essas possibilidades permanecem ambiguas.
        if nome == "bulk_create":
            for campo in CAMPOS:
                registrar(
                    no, campo, ("ESCRITA",), "bulk_create insercao possivel",
                    "AMBIGUO", "campos inseridos nao resolvidos",
                )

    # Varredura conservadora de todas as strings, inclusive SQL, migrations,
    # nomes guardados em variaveis e contextos nao classificados acima.
    for no in ast.walk(arvore):
        if not isinstance(no, ast.Constant) or not isinstance(no.value, str):
            continue
        forma = "possivel SQL" if SQL.search(no.value) else "string literal"
        motivo = (
            "SQL nao interpretado" if forma == "possivel SQL"
            else "uso da string nao resolvido"
        )
        for campo, inicio in achados(no.value):
            linha = no.lineno + no.value[:inicio].count("\n")
            registrar(
                no, campo, ("LEITURA", "ESCRITA"), forma,
                "AMBIGUO", motivo, linha,
            )


# Templates: toda ocorrencia nas expressoes de tags e inventariada.
for diretorio, dirs, arquivos in os.walk(APP):
    dirs.sort()
    for nome in sorted(arquivos):
        if not nome.endswith((".html", ".htm", ".django", ".jinja", ".jinja2")):
            continue
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        try:
            with open(caminho, encoding="utf-8-sig") as arquivo:
                texto = arquivo.read()
        except (OSError, UnicodeError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        for tag in TAGS.finditer(texto):
            for campo, inicio in achados(tag.group()):
                linha = texto.count("\n", 0, tag.start() + inicio) + 1
                emitir(
                    relativo, linha, "<template>", campo, "LEITURA",
                    "template", "AMBIGUO",
                    "tipo e contexto da expressao nao resolvidos",
                    "expressao de template",
                )


print("arquivo\tlinha\tfuncao\tcampo\tpapel\tescopo\treceptor\tmotivo\tforma")
contagens = collections.Counter()
escritores = collections.defaultdict(set)
ambiguos = collections.Counter()

for registro in sorted(REGISTROS):
    print("\t".join(limpar(item

[truncado em 20000 de 21787 caracteres]

## 19/11 00:50 · sessao b9b3bde26b6e · QUEIMA-B1 · sonda · origem=Code

A condição “montar igual ao chamador de produção” é impossível nesta cópia: a busca encontrou a definição e chamadas nos testes, mas nenhum chamador de produção. O script abaixo usa uma **reconstrução a partir da ata lavrada**, pelo leitor existente; a matriz resultante mede a resposta do juiz a essa reconstrução, não uma chamada de produção nem necessariamente a grade original. Quando faltam campos para reconstruir o insumo, registra `NAO_MEDIDA_<motivo>`. A execução e os resultados estão [não verificado]. [app/escala/tests/test_orfa_classe.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/tests/test_orfa_classe.py:114) [app/escala/services/leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114)

```python
# Executar dentro do envelope Django de leitura da sombra, tenant juliani.
# Insumo do juiz: reconstrução da ata pelo leitor existente, não chamador
# de produção. Não cria, altera nem apaga arquivos.

import collections
import datetime
import os
import sys

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from escala.models import CelulaDia
from escala.orfa_classe import classificar_orfas
from escala.services.leitor_celula import _celulas_da_ata
from ponto.juiz_batida import periodos_do_dia
from ponto.turnos import batidas_apuraveis, turnos_do_colab


INI = datetime.date(2026, 10, 21)
FIM = datetime.date(2026, 11, 20)
UM_DIA = datetime.timedelta(days=1)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def minuto_local(instante):
    if timezone.is_aware(instante):
        instante = timezone.localtime(instante).replace(tzinfo=None)
    return instante.replace(second=0, microsecond=0)


def hora_valida(hora):
    if not isinstance(hora, str) or len(hora) != 5 or hora[2] != ":":
        return False
    try:
        return datetime.datetime.strptime(hora, "%H:%M").strftime("%H:%M") == hora
    except ValueError:
        return False


def indice_turnos(turnos):
    por_pk = collections.defaultdict(list)
    for turno in turnos:
        for batida in turno.batidas:
            por_pk[batida.pk].append((turno, batida))
    return por_pk


def situacao_pareador(registros):
    # Mesma precedência da sonda2.
    if len(registros) != 1:
        return "NAO_MEDIDA_MULTIPLOS_OU_SEM_TURNO"
    turno, batida = registros[0]
    if getattr(batida, "_intra_dur", False):
        return "NAO_MEDIDA_ORIGEM_PAUSA"
    if turno.entrada is not None and turno.saida is not None:
        return "PAR_FECHADO"
    if turno.entrada is not None and turno.saida is None:
        if batida.pk == turno.entrada.pk:
            return "TURNO_ABERTO"
        return "NAO_MEDIDA_BATIDA_INTERNA_TURNO_ABERTO"
    if turno.entrada is None and turno.saida is not None:
        if batida.pk == turno.saida.pk:
            return "SAIDA_SEM_ENTRADA"
        return "NAO_MEDIDA_BATIDA_INTERNA_SEM_ENTRADA"
    return "FORA_DA_ESCALA_SEM_ENQUADRAMENTO"


def classes_da_ata(ata, orfas):
    # Não chamar com tipo_dia ausente: o juiz assumiria folga.
    tipo = ata.get("tipo_dia")
    if not isinstance(tipo, str) or not tipo:
        return None, "NAO_MEDIDA_TIPO_DIA_AUSENTE"

    # Para tipo != trabalho, o juiz não consulta celulas.
    if tipo != "trabalho":
        dia = {"tipo_dia": tipo, "orfas": orfas, "celulas": []}
        return classificar_orfas(dia), None

    lampadas = ata.get("lampadas")
    if lampadas:
        if not isinstance(lampadas, list) or not all(
            isinstance(item, dict) for item in lampadas
        ):
            return None, "NAO_MEDIDA_LAMPADAS_FORMATO"
        # O leitor trata acesa=None como não missing. Se a ata não
        # permite afirmar o estado, não transformar desconhecido em fato.
        if any(item.get("acesa") is None for item in lampadas):
            return None, "NAO_MEDIDA_ESTADO_LAMPADA"
    else:
        n_missing = ata.get("n_missing")
        if type(n_missing) is not int or n_missing < 0:
            return None, "NAO_MEDIDA_N_MISSING_AUSENTE_OU_INVALIDO"

    # n_ok=0 basta para a pergunta booleana _tem_missing do juiz.
    celulas, _ = _celulas_da_ata(ata, 0)
    dia = {"tipo_dia": tipo, "orfas": orfas, "celulas": celulas}
    return classificar_orfas(dia), None


def registra(tabela, chave, exemplo):
    tabela[chave].append(exemplo)


def imprime_tabela(prefixo, tabela):
    for chave in sorted(tabela, key=lambda x: tuple(str(v) for v in x)):
        exemplos = sorted(set(tabela[chave]))[:2]
        campos = " ".join(str(v) for v in chave)
        print("%s %s ocorrencias=%d" % (prefixo, campos, len(tabela[chave])))
        for cid, dia, hora in exemplos:
            print(
                "%s_EXEMPLO %s col%d %s %s"
                % (prefixo, campos, cid, dia.strftime("%d/%m"), hora)
            )
        if len(exemplos) < 2:
            print("%s_EXEMPLOS_DISPONIVEIS %s n=%d"
                  % (prefixo, campos, len(exemplos)))


def main():
    onde = "GUARDA_SOMBRA"
    try:
        with transaction.atomic():
            guarda_sombra()

            onde = "CELULAS"
            celulas = list(
                CelulaDia.objects.filter(data__gte=INI, data__lte=FIM)
                .select_related("colaborador", "colaborador__empresa")
                .order_by("colaborador_id", "data")
            )
            por_pessoa = collections.defaultdict(list)
            for celula in celulas:
                por_pessoa[celula.colaborador_id].append(celula)

            matriz = collections.defaultdict(list)
            pausa = collections.defaultdict(list)
            inversa_dias = collections.defaultdict(set)
            inversa_batidas = collections.defaultdict(set)
            inversa_exemplos = collections.defaultdict(set)
            inversa_motivos = collections.Counter()
            empresas = set()
            pausa_total = 0

            lo = timezone.make_aware(datetime.datetime.combine(INI, datetime.time.min))
            hi = timezone.make_aware(
                datetime.datetime.combine(FIM + UM_DIA, datetime.time.min)
            )
            lo_pad = timezone.make_aware(
                datetime.datetime.combine(INI - UM_DIA, datetime.time.min)
            )
            hi_pad = timezone.make_aware(
                datetime.datetime.combine(FIM + 2 * UM_DIA, datetime.time.min)
            )

            for cid, cels in sorted(por_pessoa.items()):
                colab = cels[0].colaborador
                emp = colab.empresa_id
                empresas.add(emp)

                onde = "BATIDAS"
                por_minuto_sonda2 = collections.defaultdict(list)
                for batida in batidas_apuraveis(colab, lo, hi):
                    local = timezone.localtime(batida.timestamp)
                    if INI <= local.date() <= FIM:
                        por_minuto_sonda2[
                            (local.date(), local.strftime("%H:%M"))
                        ].append(batida)

                por_minuto_pad = collections.defaultdict(list)
                for batida in batidas_apuraveis(colab, lo_pad, hi_pad):
                    por_minuto_pad[minuto_local(batida.timestamp)].append(batida)

                onde = "TURNOS"
                turnos = turnos_do_colab(colab, INI - UM_DIA, FIM + UM_DIA)
                por_pk = indice_turnos(turnos)

                for cel in cels:
                    onde = "ATA"
                    if not isinstance(cel.ata, dict):
                        inversa_motivos[(emp, "ATA_AUSENTE")] += 1
                        continue

                    orfas = cel.ata.get("orfas")
                    if orfas is not None and not isinstance(orfas, list):
                        raise RuntimeError("orfas_formato")

                    if orfas is not None:
                        if not all(hora_valida(h) for h in orfas):
                            raise RuntimeError("hora_orfa")

                        onde = "CLASSIFICADOR"
                        classes, motivo = classes_da_ata(cel.ata, orfas)

                        for hora in orfas:
                            candidatos = por_minuto_sonda2.get(
                                (cel.data, hora), []
                            )
                            if not candidatos:
                                situacao = "SEM_BATIDA"
                                registros = []
                            elif len(candidatos) >= 2:
                                situacao = "AMBIGUO"
                                registros = []
                            else:
                                registros = por_pk.get(candidatos[0].pk, [])
                                situacao = situacao_pareador(registros)

                            razao = motivo if motivo else classes.get(hora)
                            if not razao:
                                razao = "NAO_MEDIDA_RESPOSTA_SEM_HORA"

                            exemplo = (cid, cel.data, hora)
                            chave = (
                                "emp=%s" % emp,
                                "pareador=%s" % situacao,
                                "razao_ata_reconstruida=%s" % razao,
                            )
                            registra(matriz, chave, exemplo)

                            if situacao == "NAO_MEDIDA_ORIGEM_PAUSA":
                                pausa_total += 1
                                batida_turno = registros[0][1]
                                if getattr(batida_turno, "_intra_ata", False):
                                    marca = "INTRA_ATA_SIM"
                                else:
                                    marca = "NAO_MEDIDA_OUTRA_ORIGEM_PAUSA"
                                chave_pausa = (
                                    "emp=%s" % emp,
                                    "marca=%s" % marca,
                                    "razao_ata_reconstruida=%s" % razao,
                                )
                                registra(pausa, chave_pausa, exemplo)

                    onde = "INVERSA_ATA"
                    resultado = periodos_do_dia(cel, None)
                    if resultado.get("desalinhado"):
                        inversa_motivos[(emp, "ATA_DESALINHADA")] += 1
                        continue
                    papeis = resultado.get("papeis")
                    if not isinstance(papeis, list):
                        raise RuntimeError("papeis_formato")
                    if orfas is None:
                        inversa_motivos[(emp, "LISTA_ORFAS_AUSENTE")] += 1
                        continue

                    for papel in papeis:
                        if not isinstance(papel, dict):
                            raise RuntimeError("papel_formato")
                        if papel.get("acesa") is not True:
                            continue
                        instante = papel.get("instante_luz")
                        if instante is None:
                            inversa_motivos[(emp, "LUZ_SEM_INSTANTE")] += 1
                            continue
                        if not isinstance(instante, datetime.datetime):
                            raise RuntimeError("instante_formato")
                        minuto = minuto_local(instante)
                        candidatos = por_minuto_pad.get(minuto, [])
                        if len(candidatos) != 1:
                            motivo_inv = (
                                "LUZ_SEM_BATIDA" if not candidatos
                                else "MINUTO_AMBIGUO"
                            )
                            inversa_motivos[(emp, motivo_inv)] += 1
                            continue
                        if minuto.strftime("%H:%M") in orfas:
                            inversa_motivos[(emp, "LUZ_TAMBEM_EM_ORFAS")] += 1
                            continue

                        batida = candidatos[0]
                        registros = por_pk.get(batida.pk, [])
                        if len(registros) != 1:
                            inversa_motivos[(emp, "MULTIPLOS_OU_SEM_TURNO")] += 1
                            continue

                        turno, bt = registros[0]
                        entrada_orfa = (
                            turno.entrada is not None
                            and turno.saida is None
                            and bt.pk == turno.entrada.pk
                        )
                        saida_orfa = (
                            turno.entrada is None
                            and turno.saida is not None
                            and bt.pk == turno.saida.pk
                        )
                        if not (entrada_orfa or saida_orfa):
                            continue

                        tipo_inv = (
                            "TURNO_ABERTO" if entrada_orfa else "SAIDA_SEM_ENTRADA"
                        )
                        chave_inv = (emp, tipo_inv)
                        inversa_dias[chave_inv].add((cid, cel.data))
                        inversa_batidas[chave_inv].add(batida.pk)
                        inversa_exemplos[chave_inv].add(
                            (cid, minuto.date(), minuto.strftime("%H:%M"))
                        )

            onde = "SAIDA"
            imprime_tabela("MATRIZ", matriz)
            imprime_tabela("PAUSA", pausa)
            print("PAUSA_TOTAL sobras=%d referencia_453=%s" % (
                pausa_total, "confere" if pausa_total == 453 else "diverge"
            ))

            for emp in sorted(empresas, key=str):
                total_dias = set()
                total_batidas = set()
                for tipo_inv in ("TURNO_ABERTO", "SAIDA_SEM_ENTRADA"):
                    chave_inv = (emp, tipo_inv)
                    dias = inversa_dias[chave_inv]
                    bats = inversa_batidas[chave_inv]
                    total_dias.update(dias)
                    total_batidas.update(bats)
                    print(
                        "INVERSA emp=%s pareador=%s dias=%d batidas=%d "
                        "razao=NAO_APLICAVEL_LUZ_NAO_ORFA"
                        % (emp, tipo_inv, len(dias), len(bats))
                    )
                    for cid, dia, hora in sorted(inversa_exemplos[chave_inv])[:2]:
                        print(
                            "INVERSA_EXEMPLO emp=%s pareador=%s col%d %s %s"
                            % (emp, tipo_inv, cid, dia.strftime("%d/%m"), hora)
                        )
                print(
                    "INVERSA_TOTAL emp=%s dias=%d batidas=%d"
                    % (emp, len(total_dias), len(total_batidas))
                )

            for (emp, motivo_inv), total in sorted(
                inversa_motivos.items(), key=lambda item: (str(item[0][0]), item[0][1])
            ):
                print(
                    "INVERSA_NAO_MEDIDA emp=%s motivo=%s ocorrencias=%d"
                    % (emp, motivo_inv, total)
                )

        print("ERROS=0")
        print("FIM_SONDA_B1")
        return 0

    except Exception:
        print("ERRO " + onde)
        print("ERROS=1")
        print("FIM_SONDA_B1")
        return 1


sys.exit(main())
```

Cada célula `MATRIZ` conta **ocorrências da lista de órfãs da ata**, por empresa: à esquerda, a situação da batida encontrada pelo mesmo casamento por minuto e precedência da sonda2 fornecida [não verificado]; à direita, a resposta do juiz sobre o `tipo_dia`, as órfãs e as células reconstruídas da ata. A classe não prova a causa da decisão do pareador. O juiz dá `folga_desmentida` para qualquer `tipo_dia` diferente de `trabalho`; para trabalho, dá `indefinida` sem missing, ou `eco_cluster`/`geometria` conforme a sequência de órfãs quando há missing. [app/escala/orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53)

A reconstrução tem limite material: o cartório lavra `n_missing` a partir das lâmpadas quando elas existem, e o leitor reconstrói células dessas lâmpadas ou da contagem agregada. Isso não demonstra que a lista reconstruída seja idêntica à grade que originou a ata. Por isso os rótulos dizem `razao_ata_reconstruida`. [app/ponto/services/cartorio.py:736](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/cartorio.py:736) [app/escala/services/leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114)

Na tabela `PAUSA`, `INTRA_ATA_SIM` informa que o pareador deixou o carimbo associado ao papel `X` ou `Xi`; não prova exclusividade de origem, pois outras regras também podem ter marcado `_intra_dur`. Sem esse carimbo, `NAO_MEDIDA_OUTRA_ORIGEM_PAUSA` preserva a origem desconhecida. A comparação com 453 é uma conferência da referência transcrita, cujo resultado nesta execução está [não verificado]. [app/ponto/turnos.py:1305](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1305) [app/ponto/turnos.py:453](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:453)

Na inversa, a batida medida acendeu uma luz e seu horário não consta nas órfãs daquela ata; o classificador recebe somente órfãs, portanto a classificação dessa luz é declarada não aplicável. Os motivos de exclusão são contagens de ocorrências, não necessariamente de dias distintos. [app/escala/orfa_classe.py:58](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:58) [não verificado]

**perguntas de lei: N** [não verificado]

## 19/11 00:49 · sessao fcbab515d637 · QUEIMA-B2-disputa · censo · origem=Code

O script abaixo é para ser executado pela sessão com `python3 -B script.py RAIZ`. Sua execução e seus resultados estão [não verificado].

```python
#!/usr/bin/env python3
# Uso: python3 -B script.py RAIZ
# Somente leitura. A saida e um inventario sintatico conservador.
import ast
import collections
import os
import re
import sys
import tokenize

sys.dont_write_bytecode = True

if len(sys.argv) != 2:
    sys.stderr.write("Uso: python3 -B script.py RAIZ\n")
    sys.exit(2)

RAIZ = os.path.abspath(sys.argv[1])
APP = os.path.join(RAIZ, "app")
if not os.path.isdir(APP):
    sys.stderr.write("ERRO: RAIZ deve conter app/\n")
    sys.exit(2)

CAMPOS = (
    "motivos",
    "fechada_em",
    "reaberturas_colab",
    "reaberturas_admin",
    "fechada_por",
    "resolucao",
    "motivo_fechamento",
)
ALVOS = set(CAMPOS)
# Django permite acessar uma ForeignKey tambem pelo sufixo _id.
ALIASES = {c: c for c in CAMPOS}
ALIASES["fechada_por_id"] = "fechada_por"

PADRAO = re.compile(
    r"(?<![A-Za-z0-9_])(?:"
    + "|".join(re.escape(c) for c in sorted(ALIASES, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)
SQL = re.compile(
    r"\b(select|insert|update|delete|with|returning|where|from|join)\b",
    re.I,
)
TAGS_TEMPLATE = re.compile(r"{{.*?}}|{%.*?%}", re.S)
ATRIBUTO_TEMPLATE = re.compile(
    r"\.\s*("
    + "|".join(re.escape(c) for c in sorted(ALIASES, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)

LEITURAS = {
    "filter", "exclude", "get", "values", "values_list", "order_by",
    "annotate", "alias", "only", "defer", "distinct", "earliest", "latest",
}
QUERYSET = LEITURAS | {
    "all", "none", "select_for_update", "select_related", "prefetch_related",
    "using", "exclude", "reverse", "iterator", "first", "last",
    "get_or_create", "update_or_create", "create", "update", "bulk_create",
    "bulk_update",
}
REGISTROS = set()
FALHAS = []
MODELOS = set()
ARVORES = []

def limpar(valor):
    return str(valor).replace("\t", " ").replace("\r", " ").replace("\n", " ")

def categoria(relativo):
    partes = relativo.lower().replace("\\", "/").split("/")
    nome = partes[-1]
    if "migrations" in partes:
        return "migration"
    if (
        "tests" in partes or "test" in partes
        or nome.startswith("test_") or nome.endswith("_test.py")
    ):
        return "teste" [nao verificado]
    return "prod"

def chamada(no):
    if not isinstance(no, ast.Call):
        return ""
    if isinstance(no.func, ast.Name):
        return no.func.id
    if isinstance(no.func, ast.Attribute):
        return no.func.attr
    return ""

def campo_exato(texto):
    if not isinstance(texto, str):
        return None
    return ALIASES.get(texto.lstrip("-"))

def ocorrencias(texto):
    for achado in PADRAO.finditer(texto):
        yield ALIASES[achado.group()], achado.start()

def emit(relativo, linha, escopo, campo, papel, classe, receptor, motivo, forma):
    REGISTROS.add((
        relativo, int(linha), escopo or "<modulo>", campo, papel, classe,
        receptor, motivo, forma,
    ))

def filhos_sem_escopos(no):
    """Percorre um escopo sem entrar em funcoes ou classes aninhadas."""
    pilha = list(ast.iter_child_nodes(no))
    while pilha:
        atual = pilha.pop()
        yield atual
        if not isinstance(
            atual, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef, ast.Lambda)
        ):
            pilha.extend(ast.iter_child_nodes(atual))

def nome_base(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return ""

# Primeiro passe: ler fontes e identificar classes com base models.Model/Model.
for diretorio, dirs, arquivos in os.walk(APP):
    dirs.sort()
    for nome in sorted(arquivos):
        if not nome.endswith(".py"):
            continue
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        try:
            with tokenize.open(caminho) as arquivo:
                fonte = arquivo.read()
            arvore = ast.parse(fonte, filename=relativo)
        except (OSError, SyntaxError, UnicodeError, LookupError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        ARVORES.append((relativo, arvore))
        for no in ast.walk(arvore):
            if isinstance(no, ast.ClassDef):
                if any(nome_base(base) == "Model" for base in no.bases):
                    MODELOS.add(no.name)

MODELOS.add("DisputaSupervisao")

for relativo, arvore in ARVORES:
    classe_arquivo = categoria(relativo)
    pais = {}
    escopos = {}
    classes = {}
    ambientes = {}

    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    def indexar(no, pilha, classe_atual, ambiente):
        if isinstance(no, ast.ClassDef):
            pilha = pilha + [no.name]
            classe_atual = no.name if no.name in MODELOS else None
            ambiente = no
        elif isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            pilha = pilha + [no.name]
            ambiente = no
        elif isinstance(no, ast.Lambda):
            ambiente = no
        escopos[no] = ".".join(pilha) or "<modulo>"
        classes[no] = classe_atual
        ambientes[no] = ambiente
        for filho in ast.iter_child_nodes(no):
            indexar(filho, pilha, classe_atual, ambiente)

    indexar(arvore, [], None, arvore)

    # Cada nome guarda todas as atribuicoes visiveis no mesmo escopo.
    # Atribuicoes contraditorias ou nao resolvidas tornam o receptor ambiguo.
    atribuicoes = collections.defaultdict(lambda: collections.defaultdict(list))
    anotacoes = collections.defaultdict(lambda: collections.defaultdict(list))

    for ambiente in [
        n for n in ast.walk(arvore)
        if isinstance(n, (ast.Module, ast.ClassDef, ast.FunctionDef,
                          ast.AsyncFunctionDef, ast.Lambda))
    ]:
        if isinstance(ambiente, (ast.FunctionDef, ast.AsyncFunctionDef, ast.Lambda)):
            args = ambiente.args
            todos = list(args.posonlyargs) + list(args.args) + list(args.kwonlyargs)
            if args.vararg:
                todos.append(args.vararg)
            if args.kwarg:
                todos.append(args.kwarg)
            for arg in todos:
                if arg.annotation is not None:
                    anotacoes[ambiente][arg.arg].append(arg.annotation)

        for no in filhos_sem_escopos(ambiente):
            if ambientes.get(no) is not ambiente:
                continue
            if isinstance(no, ast.Assign):
                for alvo in no.targets:
                    if isinstance(alvo, ast.Name):
                        atribuicoes[ambiente][alvo.id].append(no.value)
            elif isinstance(no, ast.AnnAssign) and isinstance(no.target, ast.Name):
                anotacoes[ambiente][no.target.id].append(no.annotation)
                if no.value is not None:
                    atribuicoes[ambiente][no.target.id].append(no.value)
            elif isinstance(no, (ast.For, ast.AsyncFor)) and isinstance(no.target, ast.Name):
                atribuicoes[ambiente][no.target.id].append(no.iter)

    def modelo_anotacao(no):
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            texto = no.value
            encontrados = {m for m in MODELOS if re.search(
                r"(?<![A-Za-z0-9_])" + re.escape(m) + r"(?![A-Za-z0-9_])",
                texto,
            )}
        else:
            encontrados = {
                x.id for x in ast.walk(no)
                if isinstance(x, ast.Name) and x.id in MODELOS
            }
            encontrados |= {
                x.attr for x in ast.walk(no)
                if isinstance(x, ast.Attribute) and x.attr in MODELOS
            }
        return next(iter(encontrados)) if len(encontrados) == 1 else None

    def inferir(no, contexto, visitados=None):
        """Retorna nome de modelo apenas quando a sintaxe oferece evidencia."""
        if no is None:
            return None
        if visitados is None:
            visitados = set()
        ambiente = ambientes.get(contexto, arvore)
        if isinstance(no, ast.Name):
            if no.id == "self" and classes.get(contexto):
                return classes[contexto]
            if no.id in MODELOS:
                # Um nome local atribuido pode sombrear o nome da classe.
                if no.id not in atribuicoes[ambiente]:
                    return no.id
                return None
            chave = (id(ambiente), no.id)
            if chave in visitados:
                return None
            visitados = visitados | {chave}
            anns = anotacoes[ambiente].get(no.id, [])
            vals = atribuicoes[ambiente].get(no.id, [])
            resultados = [modelo_anotacao(x) for x in anns]
            resultados += [inferir(x, x, visitados) for x in vals]
            if resultados and None not in resultados and len(set(resultados)) == 1:
                return resultados[0]
            return None
        if isinstance(no, ast.Attribute):
            if no.attr in MODELOS and isinstance(no.value, ast.Name):
                return no.attr
            if no.attr in ("objects", "_default_manager", "_base_manager"):
                return inferir(no.value, contexto, visitados)
            return None
        if isinstance(no, ast.Subscript):
            return inferir(no.value, contexto, visitados)
        if isinstance(no, ast.Call):
            if isinstance(no.func, ast.Name) and no.func.id in MODELOS:
                return inferir(no.func, contexto, visitados)
            if isinstance(no.func, ast.Attribute):
                if no.func.attr in MODELOS:
                    return inferir(no.func, contexto, visitados)
                if no.func.attr in QUERYSET:
                    return inferir(no.func.value, contexto, visitados)
        return None

    def receptor(no, contexto, caminho=None):
        modelo = inferir(no, contexto)
        # Um caminho de relacao pode apontar para outro modelo.
        if caminho:
            partes = caminho.lstrip("-").split("__")
            posicoes = [i for i, p in enumerate(partes) if p in ALIASES]
            if posicoes and posicoes[0] > 0:
                return ("AMBIGUO", "caminho de relacao nao resolvido")
        if modelo:
            return ("RECEPTOR=" + modelo, "inferencia sintatica")
        return ("AMBIGUO", "receptor nao resolvido")

    def registrar(no, campo, papeis, forma, rec=None, motivo=None, linha=None):
        if rec is None:
            rec, motivo = "AMBIGUO", "receptor nao resolvido"
        for papel in papeis:
            emit(
                relativo, linha or getattr(no, "lineno", 1),
                escopos.get(no, "<modulo>"), campo, papel, classe_arquivo,
                rec, motivo or "", forma,
            )

    def contexto_externo(no):
        """Acha uma chamada ORM externa, por exemplo update(x=F(...))."""
        atual = no
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, ast.Call) and isinstance(atual.func, ast.Attribute):
                if chamada(atual) in QUERYSET:
                    return atual
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        return None

    def receptor_chamada(call, contexto, caminho=None):
        if call and isinstance(call.func, ast.Attribute):
            return receptor(call.func.value, contexto, caminho)
        return ("AMBIGUO", "chamada sem receptor de modelo")

    def identificar_string(no):
        """Classifica uma string literal pelo contexto sintatico mais proximo."""
        valor = no.value
        atual = no
        keyword_visto = None
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, ast.keyword) and keyword_visto is None:
                keyword_visto = atual.arg
            if isinstance(atual, ast.Call):
                nome = chamada(atual)
                if nome in ("getattr", "setattr"):
                    if len(atual.args) >= 2 and atual.args[1] is no:
                        rec, motivo = receptor(atual.args[0], no)
                        papel = "LEITURA" if nome == "getattr" else "ESCRITA"
                        return (papel,), nome, rec, motivo
                if nome in ("F", "Q"):
                    externo = contexto_externo(atual)
                    rec, motivo = receptor_chamada(externo, no, valor)
                    return ("LEITURA",), nome, rec, motivo
                if nome in ("raw", "execute", "executemany") and SQL.search(valor):
                    return (
                        ("LEITURA", "ESCRITA"), "SQL cru",
                        "AMBIGUO", "SQL nao interpretado",
                    )
                if nome == "save" and keyword_visto == "update_fields":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "save(update_fields)", rec, motivo
                if nome == "bulk_update":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "bulk_update(fields)", rec, motivo
                if nome == "bulk_create" and keyword_visto == "update_fields":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "bulk_create(update_fields)", rec, motivo
                if nome in ("create", "update"):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    return ("ESCRITA",), nome, rec, motivo
                if nome in ("get_or_create", "update_or_create"):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    if keyword_visto in ("defaults", "create_defaults"):
                        return ("ESCRITA",), nome + " defaults", rec, motivo
                    return ("LEITURA", "ESCRITA"), nome, rec, motivo
                if nome in LEITURAS:
                    rec, motivo = receptor_chamada(atual, no, valor)
                    return ("LEITURA",), nome, rec, motivo
                if nome in MODELOS:
                    rec, motivo = receptor(atual.func, no)
                    return ("ESCRITA",), "construtor de modelo", rec, motivo
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        if SQL.search(valor):
            return (
                ("LEITURA", "ESCRITA"), "possivel SQL",
                "AMBIGUO", "string SQL nao interpretada",
            )
        return (
            ("LEITURA", "ESCRITA"), "string fora de contexto reconhecido",
            "AMBIGUO", "uso da string nao resolvido",
        )

    for no in ast.walk(arvore):
        if isinstance(no, ast.Attribute) and no.attr in ALIASES:
            campo = ALIASES[no.attr]
            rec, motivo = receptor(no.value, no)
            if isinstance(no.ctx, ast.Store):
                papeis = ("ESCRITA",)
                if isinstance(pais.get(no), ast.AugAssign):
                    papeis = ("LEITURA", "ESCRITA")
            elif isinstance(no.ctx, ast.Del):
                papeis = ("ESCRITA",)
            else:
                papeis = ("LEITURA",)
            registrar(no, campo, papeis, "atributo", rec, motivo)

        elif isinstance(no, ast.keyword) and no.arg:
            campo = next((c for c, _ in ocorrencias(no.arg)), None)
            if not campo:
                continue
            call = pais.get(no)
            nome = chamada(call)
            if nome in ("Q", "F"):
                externo = contexto_externo(call)
                rec, motivo = receptor_chamada(externo, no, no.arg)
                papeis = ("LEITURA",)
            elif nome in ("create", "update"):
                rec, motivo = receptor_chamada(call, no, no.arg)
                papeis = ("ESCRITA",)
            elif nome in ("get_or_create", "update_or_create"):
                rec, motivo = receptor_chamada(call, no, no.arg)
                papeis = ("LEITURA", "ESCRITA")
            elif nome in LEITURAS:
                rec, motivo = receptor_chamada(call, no, no.arg)
                papeis = ("LEITURA",)
            elif nome in MODELOS:
                rec, motivo = receptor(call.func, no)
                papeis = ("ESCRITA",)
            else:
                rec, motivo = "AMBIGUO", "kwarg em chamada nao reconhecida"
                papeis = ("LEITURA", "ESCRITA")
            registrar(no, campo, papeis, "kwarg " + (nome or "?"), rec, motivo)

        elif isinstance(no, ast.Constant) and isinstance(no.value, str):
            achados = list(ocorrencias(no.value))
            if not achados:
                continue
            papeis, forma, rec, motivo = identificar_string(no)
            for campo, inicio in achados:
                linha = no.lineno + no.value[:inicio].count("\n")
                registrar(no, campo, papeis, forma, rec, motivo, linha)

# Templates em qualquer diretorio sob app/, inclusive templates de aplicativos.
for diretorio, dirs, arquivos in os.walk(APP):
    dirs.sort()
    for nome in sorted(arquivos):
        if not nome.endswith((".html", ".htm", ".django", ".jinja", ".jinja2")):
            continue
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        try:
            with open(caminho, encoding="utf-8-sig") as arquivo:
                texto = arquivo.read()
        except (OSError, UnicodeError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        for tag in TAGS_TEMPLATE.finditer(texto):
            conteudo = tag.group()
            if conteudo.startswith("{%") and not re.match(
                r"{%\s*(if|elif)\b", conteudo
            ):
                continue
            for achado in ATRIBUTO_TEMPLATE.finditer(conteudo):
                linha = texto.count("\n", 0, tag.start() + achado.start()) + 1
                emit(
                    relativo, linha, "<template>", ALIASES[achado.group(1)],
                    "LEITURA", "template", "AMBIGUO",
                    "tipo da variavel do template nao resolvido",
                    "expressao de template",
                )

print("arquivo\tlinha\tfuncao\tcampo\tpapel\tescopo\treceptor\tmotivo\tforma")
contagens = collections.Counter()
for registro in sorted(REGISTROS):
    print("\t".join(limpar(item) for item in registro))
    _, _, _, campo, papel, escopo, _, _, _ = registro
    contagens[(campo, papel, escopo)] += 1

print("RESUMO\tcampo\tpapel\tescopo\tquantidade")
for campo in CAMPOS:
    for papel in ("ESCRITA", "LEITURA"):
        for escopo in ("prod", "teste", "migration", "template"): [nao verificado]
            print(
                "CONTAGEM\t%s\t%s\t%s\t%d"
                % (campo, papel, escopo, contagens[(campo, papel, escopo)])
            )

if FALHAS:
    for arquivo, erro in sorted(FALHAS):
        sys.stderr.write("ERRO\t%s\t%s\n" % (limpar(arquivo), limpar(erro)))
    sys.stderr.write("CENSO_INCOMPLETO\n")
    sys.exit(1)

print("FIM_CENSO_B2-disputa")
```

Limites declarados [não verificado]: o script não resolve fluxo entre funções, nomes ou SQL montados dinamicamente, conteúdo de variáveis usadas como `fields`, `defaults` ou `update_fields`, relações Django, herança de modelos, nem o tipo de variáveis de templates. Um `Q` guardado em variável, como o de [models.py:2860](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2860), permanece ambíguo mesmo quando aplicado depois em [models.py:2862](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2862). Strings fora dos contextos reconhecidos e SQL são classificados conservadoramente; o resumo conta ocorrências sintáticas, inclusive ambíguas, e não comprova persistência ou execução em produção [não verificado].

## 19/11 00:48 · sessao ccb234384925 · QUEIMA-B4 · sonda · origem=Code

A sonda abaixo não foi executada nesta sessão. [não verificado] Ela usa a competência que contém cada dia e o instante atual da leitura; os resultados serão a medida atual da sombra, sujeitos às falhas ou ausências que ela imprimir. [ponto/janelas.py:60] [ponto/management/commands/e6_oraculo.py:182]

```python
# Executar dentro do Django pelo envelope de leitura, no tenant juliani.
# Não cria, edita ou apaga arquivos; não imprime nomes nem CPF.
# A transação READ ONLY também impede escritas feitas por funções chamadas.
# Resultado atual, não reconstrução da decisão histórica.

import datetime as dt
import os

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.alimentacao import vinculo_do_dia_carregado
from escala.models import CelulaDia
from chamados.juizes import _slot_esperado
from chamados.services.validacao import classe_do_lote
from ponto import janelas
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono,
    batida_fora_de_turno,
    dono_da_divergencia,
)
from ponto.models import Batida
from ponto.supra_juiz import cadastro_do_dia_carregado
from ponto.turnos import batidas_apuraveis


CASOS_A = {
    301: ("2026-10-22", "2026-10-29", "2026-11-05"),
    634: ("2026-10-27",),
    968: ("2026-11-02", "2026-11-03", "2026-11-05",
          "2026-11-10", "2026-11-12"),
}
CASOS_B = {
    129: ("2026-10-25", "2026-11-01", "2026-11-02",
          "2026-11-08", "2026-11-15"),
    155: ("2026-10-25", "2026-11-01", "2026-11-02", "2026-11-15"),
}
PARES = (
    (40, 139931, 135364),
    (60, 152873, 144882),
    (200, 146569, 144414),
    (263, 146777, 142735),
    (301, 151816, 149559),
    (444, 140817, 139776),
    (446, 144666, 136990),
    (549, 151894, 151065),
    (727, 140816, 136998),
    (881, 154095, 154081),
    (899, 151735, 136112),
    (909, 149856, 148650),
    (996, 153862, 140804),
    (996, 153847, 146242),
)

ARQ_DONO = "app/ponto/management/commands/e6_oraculo.py"
ARQ_LOTE = "app/chamados/services/validacao.py"
ERROS = 0


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("SETTINGS")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("CONFIGURACAO")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("CONEXAO")


def local(instante):
    if instante is None:
        return None
    if timezone.is_aware(instante):
        instante = timezone.localtime(instante)
    return instante.isoformat()


def batida_resumo(b):
    return {
        "pk": b.pk,
        "instante_local": local(b.timestamp),
        "tipo": b.tipo,
        "origem": b.origem,
    }


def janela_civil(dia):
    zona = timezone.get_current_timezone()
    inicio = timezone.make_aware(dt.datetime.combine(dia, dt.time.min), zona)
    fim = timezone.make_aware(
        dt.datetime.combine(dia + dt.timedelta(days=1), dt.time.min), zona
    )
    return inicio, fim


def clausula(horas, cadastro, fora, fase, cxr):
    if (cadastro["folga_nao_definida"]
            or cadastro["sem_vinculo_no_dia"]
            or cadastro["sem_vinculo_em_dia_de_trabalho"]):
        return ARQ_DONO + ":98"
    if fora:
        return ARQ_DONO + ":104"
    if len(horas or ()) % 2:
        return ARQ_DONO + ":106"
    if fase:
        return ARQ_DONO + ":108"
    if cxr is None:
        return "NAO_MEDIDA_CXR"
    return ARQ_DONO + ":110"


def medir_dono(etapa, cid, dia, agora):
    colab = Colaborador.objects.select_related("empresa").get(pk=cid)
    ini, fim = janelas.janela_atual(dia, colab.empresa)
    hoje = timezone.localtime(agora).date()
    ins = InsumosDoDono(colab, ini, fim, hoje, agora=agora)

    horas = ins.horas.get(str(dia), [])
    cadastro = cadastro_do_dia_carregado(cid, dia)
    fora = batida_fora_de_turno(colab, dia, ins.turnos())
    fase = str(dia)[:10] in ins.fase()

    # A chamada oficial decide quando precisa consultar CXR.
    dono = ins.dono(dia)

    # Também tentamos medir CXR quando a precedência não precisou dele.
    # O savepoint isola uma eventual tentativa de escrita ou erro do motor.
    cxr = None
    falha_cxr = None
    try:
        with transaction.atomic():
            cxr = str(dia) in ins.cxr()
    except Exception as exc:
        falha_cxr = type(exc).__name__

    linha = clausula(horas, cadastro, fora, fase, cxr)
    if linha != "NAO_MEDIDA_CXR":
        conferido = dono_da_divergencia(
            horas, cxr if cxr is not None else False,
            batida_fora_de_turno=fora,
            fase_deslocada=fase,
            **cadastro,
        )
        if conferido != dono:
            raise RuntimeError("DONO_DIVERGE_DA_RECONSTITUICAO")

    print(
        etapa, "col%d" % cid, dia,
        "competencia=", (str(ini), str(fim)),
        "hoje=", str(hoje), "agora=", local(agora),
        "horas_do_dia=", horas,
        "folga_nao_definida=", cadastro["folga_nao_definida"],
        "sem_vinculo_no_dia=", cadastro["sem_vinculo_no_dia"],
        "sem_vinculo_em_dia_de_trabalho=",
        cadastro["sem_vinculo_em_dia_de_trabalho"],
        "batida_fora_de_turno=", fora,
        "fase_deslocada=", fase,
        "em_cadastro_x_realidade=",
        cxr if falha_cxr is None else "NAO_MEDIDA_" + falha_cxr,
        "dono=", dono, "clausula=", linha,
        "porta=", ARQ_DONO + ":182",
    )

    if etapa != "B":
        return

    vinc = vinculo_do_dia_carregado(cid, dia)
    if vinc is None:
        print("B_VINCULO", "col%d" % cid, dia, "juiza=None")
    else:
        print(
            "B_VINCULO", "col%d" % cid, dia,
            "pk=", vinc.pk, "inicio=", vinc.data_inicio,
            "fim=", vinc.data_fim, "ativa=", vinc.ativa,
            "tipo_escala_id=", vinc.tipo_escala_id,
            "fonte=app/escala/alimentacao.py:296",
        )

    celulas = list(CelulaDia.objects.filter(
        colaborador_id=cid, data=dia
    ).order_by("pk"))
    if not celulas:
        print("B_CELULA", "col%d" % cid, dia, "AUSENTE")
    for cel in celulas:
        dna = cel.dna if isinstance(cel.dna, dict) else None
        # Chaves conhecidas do produtor; evita imprimir eventual texto pessoal.
        chaves = (
            "dna_versao", "vinculo_id", "tipo_escala_id", "tipo_ciclo",
            "marcos", "minutos_jornada", "intervalo_livre",
            "lampadas", "extra_declarada",
        )
        dna_saida = None if dna is None else {
            chave: dna[chave] for chave in chaves if chave in dna
        }
        print(
            "B_CELULA", "col%d" % cid, dia,
            "pk=", cel.pk, "trabalha=", cel.trabalha,
            "origem=", cel.origem,
            "escala_geradora_id=", cel.escala_geradora_id,
            "dna=", dna_saida,
            "fonte=app/escala/models.py:1681",
        )

    lo, hi = janela_civil(dia)
    batidas = batidas_apuraveis(colab, lo, hi).filter(timestamp__lt=hi)
    print(
        "B_BATIDAS_DIA_CIVIL", "col%d" % cid, dia,
        [batida_resumo(b) for b in batidas],
        "fonte=app/ponto/turnos.py:2868",
    )


def pergunta_resumo(p):
    return {
        "pergunta_pk": p.pk,
        "disputa_id": p.disputa_id,
        "celula_id": p.celula_id,
        "chamado_destino_id": p.chamado_destino_id,
        "batida_origem_id": p.batida_origem_id,
        "data_evento": str(p.data_evento) if p.data_evento else None,
        "motivo": p.motivo,
        "instancia_marco": p.instancia_marco,
        "tipo_resposta": p.tipo_resposta,
        "respondida_em": local(p.respondida_em),
        "validada_em": local(p.validada_em),
        "validada_por_id": p.validada_por_id,
        "materializada_em": local(p.materializada_em),
        "materializada_por_id": p.materializada_por_id,
        "via_resolucao": p.via_resolucao,
    }


def medir_c():
    dia = dt.date(2026, 11, 3)
    lo, hi = janela_civil(dia)
    batidas = list(Batida.objects.filter(
        colaborador_id=200,
        origem="disputa_s84_retro",
        timestamp__gte=lo,
        timestamp__lt=hi,
    ).select_related("pergunta_origem").order_by("timestamp", "pk"))

    if not batidas:
        print("C col200 2026-11-03 NENHUMA_BATIDA_COM_ESSA_ORIGEM")
    for b in batidas:
        print(
            "C_BATIDA", batida_resumo(b),
            "aprovada=", b.aprovada,
            "aprovada_por_id=", b.aprovada_por_id,
            "aprovada_em=", local(b.aprovada_em),
            "criada_em=", local(b.criado_em),
            "criador_id=NAO_EXISTE_CAMPO_NO_MODELO",
            "pergunta_origem_id=", b.pergunta_origem_id,
            "retratada_em=", local(b.retratada_em),
            "fonte=app/ponto/models.py:8",
        )
        p = b.pergunta_origem
        if p is None:
            print(
                "C_PERGUNTA batida_pk=", b.pk,
                "NAO_MEDIDA_PERGUNTA_ORIGEM_NULA",
                "fonte=app/ponto/models.py:40",
            )
            continue
        print(
            "C_PERGUNTA batida_pk=", b.pk, pergunta_resumo(p),
            "fonte=app/chamados/models.py:2593",
        )
        atos = list(p.decisoes.order_by("ato_em", "pk"))
        print(
            "C_ATOS pergunta_pk=", p.pk,
            [{
                "pk": a.pk,
                "ato": a.ato,
                "ato_em": local(a.ato_em),
                "ato_por_id": a.ato_por_id,
                "validada_original_em": local(a.validada_original_em),
                "validada_original_por_id": a.validada_original_por_id,
            } for a in atos],
            "fonte=app/chamados/models.py:4166",
            "vinculo_ato_batida=NAO_VERIFICADO",
        )


def medir_d(cid, plantada_pk, real_pk):
    b = Batida.objects.select_related(
        "colaborador", "pergunta_origem",
        "pergunta_origem__disputa",
        "pergunta_origem__disputa__colaborador",
    ).get(pk=plantada_pk, colaborador_id=cid)

    print(
        "D_PLANTADA", "col%d" % cid,
        "real_referencia_pk=", real_pk,
        "batida=", batida_resumo(b),
        "pergunta_origem_id=", b.pergunta_origem_id,
        "fonte=app/ponto/models.py:40",
    )
    p = b.pergunta_origem
    if p is None:
        print(
            "D_RESULTADO plantada_pk=", plantada_pk,
            "NAO_MEDIDA_PERGUNTA_ORIGEM_NULA",
            "slot=NAO_MEDIDO batidas=NAO_MEDIDAS classe=NAO_MEDIDA",
        )
        return
    if p.disputa.colaborador_id != cid:
        print(
            "D_RESULTADO plantada_pk=", plantada_pk,
            "NAO_MEDIDA_COLABORADOR_DA_DISPUTA_DIVERGENTE",
        )
        return

    print("D_PERGUNTA plantada_pk=", plantada_pk, pergunta_resumo(p))

    # Cada chamada fica isolada: uma falha do slot não impede tentar a classe.
    slot = None
    try:
        with transaction.atomic():
            slot, tipo_slot = _slot_esperado(p)
            if slot is None:
                print(
                    "D_SLOT plantada_pk=", plantada_pk,
                    "NAO_MEDIDA_SLOT_INDISPONIVEL",
                    "fonte=app/chamados/juizes.py:345",
                )
            else:
                perto = dt.timedelta(minutes=60)
                proximas = list(batidas_apuraveis(
                    p.disputa.colaborador, slot - perto, slot + perto
                ))
                print(
                    "D_SLOT plantada_pk=", plantada_pk,
                    "instante_local=", local(slot), "tipo=", tipo_slot,
                    "batidas_apuraveis_inclusivas_mais_menos_60min=",
                    [batida_resumo(x) for x in proximas],
                    "fonte=app/chamados/services/validacao.py:1204",
                )
    except Exception as exc:
        print(
            "D_SLOT plantada_pk=", plantada_pk,
            "NAO_MEDIDA_" + type(exc).__name__,
        )

    try:
        with transaction.atomic():
            classe, detalhe = classe_do_lote(p)
            # Não imprimir resposta livre, que pode conter nome ou CPF.
            detalhe_seguro = {
                k: detalhe[k] for k in
                ("delta_min", "ampm", "esperada", "sinal_min", "tipo",
                 "conflito", "porque")
                if k in detalhe
            }
            conflito = (
                detalhe["conflito"] if "conflito" in detalhe
                else "NAO_INFORMADO_PELA_FUNCAO"
            )
            print(
                "D_RESULTADO plantada_pk=", plantada_pk,
                "classe=", classe, "conflito=", conflito,
                "detalhe=", detalhe_seguro,
                "fonte=", ARQ_LOTE + ":1177",
                "estado=ATUAL",
            )
    except Exception as exc:
        print(
            "D_RESULTADO plantada_pk=", plantada_pk,
            "NAO_MEDIDA_" + type(exc).__name__,
        )


def executar_caso(rotulo, funcao, *args):
    global ERROS
    try:
        # Isola erro de SQL sem contaminar os demais casos.
        with transaction.atomic():
            funcao(*args)
    except Exception as exc:
        ERROS += 1
        print(rotulo, "NAO_MEDIDA_" + type(exc).__name__)


def main():
    global ERROS
    try:
        with transaction.atomic():
            guarda_sombra()
            agora = timezone.now()

            for etapa, casos in (("A", CASOS_A), ("B", CASOS_B)):
                for cid, dias in casos.items():
                    for texto_dia in dias:
                        dia = dt.date.fromisoformat(texto_dia)
                        executar_caso(
                            "%s col%d %s" % (etapa, cid, dia),
                            medir_dono, etapa, cid, dia, agora,
                        )

            executar_caso("C col200 2026-11-03", medir_c)

            for cid, plantada_pk, real_pk in PARES:
                executar_caso(
                    "D col%d plantada_pk=%d" % (cid, plantada_pk),
                    medir_d, cid, plantada_pk, real_pk,
                )
    except Exception as exc:
        ERROS += 1
        print("ERRO_GUARDA_OU_TRANSACAO", type(exc).__name__)

    # ERROS conta exceções não tratadas por caso ou na guarda; os campos
    # NAO_MEDIDA devem ser examinados mesmo quando ERROS=0.
    print("ERROS=%d" % ERROS)
    print("FIM_SONDA_B4")


main()
```

perguntas de lei: 0

_7231 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
