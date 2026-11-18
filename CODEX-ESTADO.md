# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 19:46** · publicado em **18/11/2026 19:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 19:46 · sessao a784106a2534 · RAIA-ESPURIAS · leitura · origem=Code

```python
# Executar no shell Django do tenant, na sombra.
# Referencias da copia:
# app/ponto/models.py:22, :40, :45, :55, :67, :79
# app/colaboradores/models.py:321
# app/ponto/registro_batida.py:94
# app/escala/utils.py:631, :673
# app/ponto/services/cartorio.py:730, :745
#
# Pares: batidas adjacentes de cada colaborador na ordem (timestamp, pk).
# Em empate de timestamp, o menor pk vem primeiro. Em A-B-C, A-B e B-C
# contam separadamente quando cada par atende aos criterios; A-C nao conta.

from collections import Counter
from datetime import date, datetime, time, timedelta, timezone as dt_timezone

from django.utils import timezone
from ponto.models import Batida
from ponto.registro_batida import colisoes_tipo_oposto

ERROS = 0
INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
FAIXAS = ("0-10s", "11-60s", "61s-10min")
DIAS_COL67 = (
    date(2026, 11, 10),
    date(2026, 11, 11),
    date(2026, 11, 14),
    date(2026, 11, 15),
)


def erro(etapa, exc):
    global ERROS
    ERROS += 1
    print(f"ERRO etapa={etapa} tipo={type(exc).__name__}")


def instante(valor):
    if valor is None:
        return None
    if timezone.is_naive(valor):
        raise ValueError("datetime sem timezone")
    local = timezone.localtime(valor)
    # Milissegundos truncados; nao altera os instantes usados nos calculos.
    return local.strftime("%Y-%m-%d %H:%M:%S.") + (
        f"{local.microsecond // 1000:03d} " + local.strftime("%z")
    )


def faixa(segundos):
    if 0 <= segundos <= 10:
        return FAIXAS[0]
    if 10 < segundos <= 60:
        return FAIXAS[1]
    if 60 < segundos <= 600:
        return FAIXAS[2]
    return None


def limites_amplos(inicio, fim):
    # Margem em UTC; o teste definitivo e a data local de cada timestamp. [nao verificado]
    inicio_utc = datetime.combine(
        inicio - timedelta(days=2), time.min, tzinfo=dt_timezone.utc
    )
    fim_utc = datetime.combine(
        fim + timedelta(days=3), time.min, tzinfo=dt_timezone.utc
    )
    return inicio_utc, fim_utc


def dados_batida(b):
    return {
        "pk": b.pk,
        "timestamp_local": instante(b.timestamp),
        "tipo": b.tipo,
        "origem": b.origem,
        "canal": b.canal,
        "criado_em_local": instante(b.criado_em),
        "pergunta_origem_id": b.pergunta_origem_id,
        "retratada_em_local": instante(b.retratada_em),
        "retratada": b.retratada_em is not None,
    }


def executar():
    pares = []
    universo_disponivel = False

    print(
        "CRITERIO pares_adjacentes=(timestamp,pk) "
        "empate=menor_pk_primeiro encadeamento=AB_e_BC_se_elegiveis"
    )

    try:
        inicio_utc, fim_utc = limites_amplos(INI, FIM)
        candidatos = (
            Batida.objects.filter(
                retratada_em__isnull=True,
                colaborador__empresa_id__in=EMPRESAS,
                timestamp__gte=inicio_utc,
                timestamp__lt=fim_utc,
            )
            .select_related("colaborador")
            .order_by("colaborador_id", "timestamp", "pk")
        )
        batidas = []
        for b in candidatos:
            dia = timezone.localtime(b.timestamp).date()
            if INI <= dia <= FIM:
                batidas.append(b)

        anterior = None
        for b in batidas:
            if anterior is not None and anterior.colaborador_id == b.colaborador_id:
                segundos = (b.timestamp - anterior.timestamp).total_seconds()
                grupo = faixa(segundos)
                if grupo is not None and anterior.tipo != b.tipo:
                    pares.append((anterior, b, segundos, grupo))
            anterior = b
        universo_disponivel = True
        print(f"UNIVERSO batidas={len(batidas)} pares={len(pares)}")
    except Exception as exc:
        erro("universo_e_pares", exc)
        pares = []

    if universo_disponivel:
        try:
            for grupo in FAIXAS:
                itens = [p for p in pares if p[3] == grupo]
                print(
                    f"ITEM1 faixa={grupo} pares={len(itens)} "
                    f"pessoas={len({a.colaborador_id for a, _, _, _ in itens})}"
                )
                origens = Counter(
                    (
                        a.origem,
                        a.pergunta_origem_id is None,
                        b.origem,
                        b.pergunta_origem_id is None,
                    )
                    for a, b, _, _ in itens
                )
                for chave, quantidade in sorted(
                    origens.items(), key=lambda item: repr(item[0])
                ):
                    origem1, pergunta1_nula, origem2, pergunta2_nula = chave
                    print(
                        f"ITEM1_ORIGENS faixa={grupo} "
                        f"origem1={origem1!r} pergunta1_nula={pergunta1_nula} "
                        f"origem2={origem2!r} pergunta2_nula={pergunta2_nula} "
                        f"pares={quantidade}"
                    )
        except Exception as exc:
            erro("item1", exc)

        colidem_por_faixa = Counter()
        janelas_por_faixa = {grupo: Counter() for grupo in FAIXAS}
        avaliados_por_faixa = Counter()
        falhas_por_faixa = Counter()

        for a, b, segundos, grupo in pares:
            try:
                janela, ids = colisoes_tipo_oposto(
                    b.colaborador, b.timestamp, b.tipo
                )
                janela_s = janela.total_seconds()
                colide = a.pk in ids
                avaliados_por_faixa[grupo] += 1
                janelas_por_faixa[grupo][janela_s] += 1
                if colide:
                    colidem_por_faixa[grupo] += 1
                    print(
                        f"COLIDE_GRAVADO col{a.colaborador_id} "
                        f"faixa={grupo} segundos={segundos} "
                        f"janela_segundos={janela_s} "
                        f"b1={dados_batida(a)!r} b2={dados_batida(b)!r}"
                    )
            except Exception as exc:
                falhas_por_faixa[grupo] += 1
                erro("item2_par", exc)

        try:
            for grupo in FAIXAS:
                print(
                    f"ITEM2 faixa={grupo} avaliados={avaliados_por_faixa[grupo]} "
                    f"colidem={colidem_por_faixa[grupo]} "
                    f"falhas={falhas_por_faixa[grupo]}"
                )
                for janela_s, quantidade in sorted(janelas_por_faixa[grupo].items()):
                    print(
                        f"ITEM2_JANELA faixa={grupo} "
                        f"segundos={janela_s} pares={quantidade}"
                    )
        except Exception as exc:
            erro("item2_resumo", exc)

        try:
            for a, b, segundos, grupo in pares:
                print(
                    f"ITEM3 nao_mensuravel col{a.colaborador_id} "
                    f"b1_pk={a.pk} b2_pk={b.pk} "
                    f"faixa={grupo} segundos={segundos}"
                )
            print(f"ITEM3 total_nao_mensuravel={len(pares)}")
        except Exception as exc:
            erro("item3", exc)
    else:
        print("ITENS_1_2_3 indisponiveis=universo_nao_calculado")

    # O caso col67 e independente do filtro de empresas e inclui retratadas.
    for dia in DIAS_COL67:
        try:
            inicio_utc, fim_utc = limites_amplos(dia, dia)
            candidatos = (
                Batida.objects.filter(
                    colaborador_id=67,
                    timestamp__gte=inicio_utc,
                    timestamp__lt=fim_utc,
                )
                .select_related("colaborador")
                .order_by("timestamp", "pk")
            )
            do_dia = [
                b for b in candidatos
                if timezone.localtime(b.timestamp).date() == dia
            ]
            print(f"ITEM4 col67 dia={dia.isoformat()} batidas={len(do_dia)}")
            for b in do_dia:
                try:
                    registro = dados_batida(b)
                    try:
                        janela, ids = colisoes_tipo_oposto(
                            b.colaborador, b.timestamp, b.tipo
                        )
                        resultado = {
                            "janela_segundos": janela.total_seconds(),
                            "batidas_pks": ids,
                        }
                    except Exception as exc:
                        erro("item4_colisao", exc)
                        resultado = "indisponivel"
                    print(
                        f"ITEM4_BATIDA col67 dia={dia.isoformat()} "
                        f"batida={registro!r} colisao={resultado!r}"
                    )
                except Exception as exc:
                    erro("item4_batida", exc)
        except Exception as exc:
            erro("item4_dia", exc)


try:
    executar()
except Exception as exc:
    erro("execucao", exc)
finally:
    print(f"ERROS={ERROS}")
    print("FIM_S5II_V3")
```

- O caminho da empresa é `Batida.colaborador.empresa_id`: [app/ponto/models.py:22](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/models.py:22), [app/colaboradores/models.py:321](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/colaboradores/models.py:321). A função de colisão consulta batidas e devolve janela e IDs; seu corpo não grava: [app/ponto/registro_batida.py:94](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:94).

- A ata recebe as órfãs como texto; a formação examinada fornece `HH:MM`, sem instante completo. Por isso o item 3 não faz casamento: [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:730), [app/escala/utils.py:631](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:631), [app/escala/utils.py:673](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:673).

- **Caminhos de escrita identificados para interpretar a saída:** `app`, com canal derivado `apk` ou `pwa`, passa por `registrar_batida` nas duas APIs e na triagem web: [app/api/views.py:654](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:654), [app/api/views_core.py:645](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views_core.py:645), [app/ponto/services/triagem_batida.py:700](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/triagem_batida.py:700). `manual` passa pela porta nos caminhos de [veredito](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/veredito_celula.py:323), [justificativa](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/justificativa.py:188), [ajuste retroativo](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/justificativa.py:361) e [aprovação em lote](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/views.py:1818). Também passam pela porta `excecao_autorizada`, `reg_externa`, `disputa_s84_retro` e `disputa_ancora`: [app/chamados/services/acoes_chamado.py:44](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_chamado.py:44), [app/chamados/services/regularizacao_ext.py:78](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/regularizacao_ext.py:78), [app/chamados/services/materializacao.py:1488](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:1488), [app/chamados/services/materializacao.py:1605](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:1605). A porta pode gravar colisão com autoridade humana: [app/ponto/registro_batida.py:162](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:162).

- `pergunta_origem` pode ser vinculada posteriormente, inclusive a batida reutilizada, por `anotar`: [app/chamados/services/materializacao.py:1537](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:1537). O campo `canal=NULL` admite mais de uma explicação na árvore: [app/ponto/models.py:46](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/models.py:46). Há também escrita em lote de dados sintéticos na sombra, sem passar por `registrar_batida`: [app/ponto/registro_batida.py:351](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:351), [app/ponto/management/commands/gerar_batidas_smoke_motor.py:107](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/gerar_batidas_smoke_motor.py:107).

- **Não confirmado:** [não verificado] quais valores aparecem nos pares reais, seus totais, e se existem valores adicionais ou históricos cujo caminho de escrita precise ser identificado. [não verificado] O caminho efetivamente percorrido por cada batida, ou se ela é anterior à regra, não pode ser atribuído apenas aos valores atuais. [não verificado] A sonda não foi executada na sombra nesta sessão.

## 18/11 19:43 · sessao 4a795076c73e · HOOK-B3-PAREI-LEGITIMO · leitura · origem=Code

A trilha fornecida contém **21 linhas `BLOCK` entre 18:54:01 e 18:55:28**, cada uma seguida de `TETO-NAO-CONTA`, além do par `BLOCK`/`ALLOW` de 18:51:07. O anexo descreve nove bloqueios seguidos, mas esse número não corresponde à quantidade de linhas `BLOCK` no recorte apresentado; a relação entre as duas contagens é **[não verificado]**. [Trilha fornecida, linhas 5–48; COMPLEMENTO-1 fornecido, item 1]

A cópia não tem metadados Git disponíveis: `git rev-parse HEAD` falhou. Portanto, a igualdade entre o HEAD da cópia e o hash informado é **[não verificado]**. A correspondência dos eventos com o código abaixo é direta quanto aos textos dos ramos, mas depende de o código executado ser o desta cópia; isso é especialmente relevante para 18:51, quando a trilha registra outro prefixo de HEAD. [Trilha fornecida, linhas 5–8; bin/hook_stop_fila1.py:1404; bin/hook_stop_fila1.py:1448]

### Bloqueio → ramo → motivo

| Instante | Ramo e motivo na trilha | Desfecho que a sequência mostra |
|---|---|---|
| 18:51:07 | `BLOCK fila 1 com item aberto`: registro em [bin/hook_stop_fila1.py:1404]. | Em seguida, `ALLOW ALARME teto...`: ramo `n > TETO and not _esperando`, em [bin/hook_stop_fila1.py:1439] e [bin/hook_stop_fila1.py:1448]; `_libera` registra e sai em [bin/hook_stop_fila1.py:145]. A versão executada com prefixo `c8b6d1ec25` é **[não verificado]**. |
| 18:54:01 | `BLOCK fila 1 com item aberto`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1427]. |
| 18:54:14 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:34 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:38 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:40 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA` às 18:54:41, [bin/hook_stop_fila1.py:1429]. |
| 18:54:46 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:53 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:55 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:57 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:59 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:02 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:05 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:10 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:12 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:15 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:17 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:19 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:21 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:24 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:26 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:28 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |

O `BLOCK` da trilha é escrito **antes** do teto e do B2: sozinho, ele ainda não prova a decisão final. O par das 18:51 ilustra isso: houve `BLOCK` seguido de `ALLOW`. As linhas vizinhas de 18:46:31 e 18:47:52 mostram a mesma sequência com o prefixo antigo. [bin/hook_stop_fila1.py:1404; bin/hook_stop_fila1.py:1439; bin/hook_stop_fila1.py:1450; trilha fornecida, linhas 1–6]

Para os pares de 18:54–18:55, o código mostra que o ramo de trabalho em curso impede tanto a aplicação do teto quanto a consulta ao B2; depois disso, o caminho normal emite `decision: block`. Uma exceção posterior, porém, chama o caminho de falha aberta, e a trilha fornecida não registra a saída JSON do cliente; assim, o resultado final de cada chamada, considerado isoladamente, é **[não verificado]**. [bin/hook_stop_fila1.py:1427; bin/hook_stop_fila1.py:1439; bin/hook_stop_fila1.py:1450; bin/hook_stop_fila1.py:1459; bin/hook_stop_fila1.py:1460]

Antes do `BLOCK`, o fluxo passou pelas portas de B4, orçamento, PAREI, ESPERANDO comprovado e fila vazia sem sair por elas. A trilha não mostra o motivo particular da recusa de cada uma: por exemplo, não distingue ESPERANDO ausente de outras condições sem prova, nem informa o estado interno de B4 ou orçamento. [bin/hook_stop_fila1.py:1361; bin/hook_stop_fila1.py:1372; bin/hook_stop_fila1.py:1381; bin/hook_stop_fila1.py:1386; bin/hook_stop_fila1.py:1394]

### Veredito das hipóteses

- **(a) `[não verificável pela trilha]` quanto ao texto real; procede quanto ao código.** Se a primeira linha útil lida fosse `! PAREI: ...`, ela não começaria por `PAREI:` e `_parei_do_em_curso` retornaria falso. A trilha não guarda o topo do RELATO, então ela não prova que essa era a linha nem que esse foi o teste que falhou. Essa explicação é possível, mas sua probabilidade também é **[não verificado]**. [bin/hook_stop_fila1.py:413; bin/hook_stop_fila1.py:1260; bin/hook_stop_fila1.py:43]

- **(b) Procede como regra do código; sua ocorrência concreta é `[não verificável pela trilha]`.** Para o PAREI genérico liberar, o item em curso precisa ser identificado por ID inteiro na linha e não pode haver outro item andável; há ainda filtros como `!`, exclusão de CLEAR/COTA e SONDA. A trilha não registra o texto do PAREI nem a lista de andáveis, portanto não identifica qual condição falhou. [bin/hook_stop_fila1.py:1243; bin/hook_stop_fila1.py:1263; bin/hook_stop_fila1.py:1275; bin/hook_stop_fila1.py:1280; bin/hook_stop_fila1.py:1286; bin/hook_stop_fila1.py:1310; bin/hook_stop_fila1.py:270]

- **(c) Procede para teto e B2, condicionada à correspondência com a versão executada; “não sobra válvula” não procede como afirmação geral.** `TETO-NAO-CONTA` mostra que o detector retornou verdadeiro naquele ramo, mas não identifica qual processo foi detectado nem prova, por si só, sua natureza real. Com `_esperando` verdadeiro, o teto não libera e B2 é pulado. Ainda existem saídas anteriores, como PAREI ou ESPERANDO comprovado, e a falha aberta posterior; elas não explicam uma liberação na sequência normal observada dos pares de 18:54–18:55. [bin/hook_stop_fila1.py:1427; bin/hook_stop_fila1.py:1439; bin/hook_stop_fila1.py:1450; bin/hook_stop_fila1.py:1381; bin/hook_stop_fila1.py:1386; bin/hook_stop_fila1.py:1460]

### Quem mais lê e regras de pista

O alcance de uma cura concreta é **[não verificado]**, pois nenhuma mudança foi definida. Se tocar a leitura do topo, o B3 também depende dela: `disjuntor._topo_consistente` chama `_bloco_do_topo`, e `decide_b3` passa as linhas a `linhas_libera_b3`, que reconhece `DESENHO` e `ERRO-BOBO` com requisitos próprios. O handoff também chama os dois juízes de PAREI e lê a linha do topo; `_linha_do_topo` serve ainda ao ESPERANDO. [bin/disjuntor.py:764; bin/disjuntor.py:788; bin/disjuntor.py:728; bin/disjuntor.py:1029; bin/handoff_sessao.sh:120; bin/handoff_sessao.sh:129; bin/hook_stop_fila1.py:413]

A regra **HOOK-PAREI-COM-ANDAVEL** impede a liberação pelo PAREI genérico enquanto houver outro item andável; **HOOK-TETO-NAO-CONTA-ESPERA** impede contar a espera como avanço do teto e impede aplicá-lo enquanto o detector indica trabalho em curso. Elas estão explicitadas no código; não encontrei esses nomes literais em `CORTES.md` ou `CLAUDE.md` da cópia. A consolidação de parada e autonomia em `CLAUDE.md` aponta para a forma viva no hook e no disjuntor. [bin/hook_stop_fila1.py:1232; bin/hook_stop_fila1.py:1418; CLAUDE.md:251; resultado da busca nos dois arquivos: **[não verificado por arquivo:linha]**]

**Regra de pista que a cura muda: N = [não verificado]**; candidatas a avaliar conforme a cura forem definidas: `HOOK-PAREI-COM-ANDAVEL` e `HOOK-TETO-NAO-CONTA-ESPERA`. Alterar a interpretação do prefixo, por si só, não demonstra mudança em qualquer uma das duas regras. [bin/hook_stop_fila1.py:1232; bin/hook_stop_fila1.py:1263; bin/hook_stop_fila1.py:1418]

## 18/11 19:43 · sessao 4e4d144f130c · RAIA-ESPURIAS · leitura · origem=Code

**Sonda parcial, não executada:** as decisões da sessão permitem medir identidade, dono e destino, mas não encontrei na cópia uma função que produza as seis classes com a precedência pedida. A porta `classificar_orfas` devolve `folga_desmentida` para todas as órfãs de um dia sem trabalho; ela não distingue as classes (a)–(f). [app/escala/orfa_classe.py:53]

A sonda abaixo aplica o casamento autorizado, chama o dono para todos os colaboradores e mede os destinos. Ela conserva todos os dias em `NAO_CLASSIFICADO`, sem chamar casos ambíguos de descartados. A definição operacional necessária para distinguir as seis classes usando funções reais permanece **[nao verificado]**.

Para chamados, usa `dia_do_fato`, a porta da casa, com `fortes=True`; para a lista do admin, a presença é agregada por colaborador. A própria `lista` captura internamente algumas exceções; portanto, mesmo que a chamada retorne, a completude de seus resultados permanece **[nao verificado]**. [app/chamados/juizes.py:88] [app/escala/services/cadastro_realidade.py:773] [app/escala/services/cadastro_realidade.py:869]

A montagem do dono pode chamar `espelho_do_colab`; os efeitos transitivos completos dessa cadeia e da lista não foram verificados. Ambas são chamadas em transações revertidas por exceção. Rollback de banco não garante reversão de eventual efeito externo **[nao verificado]**. [app/ponto/management/commands/e6_oraculo.py:162] [app/ponto/management/commands/e6_oraculo.py:182]

```python
# SONDA PARCIAL — AUDITORIA-ESPURIAS ITEM 2 — NAO EXECUTADA
# Executar somente pelo envelope autorizado da sessao.
#
# Fontes:
# app/escala/orfa_classe.py:53 — porta de folga.
# app/ponto/turnos.py:2868 — batidas_apuraveis.
# app/ponto/management/commands/e6_oraculo.py:113,182 — dono.
# app/chamados/juizes.py:88 — dia_do_fato.
# app/escala/services/cadastro_realidade.py:773 — lista por colaborador.
#
# Casamento autorizado: uma hora da ata no dia D procura batidas
# apuraveis no dia civil D ou D+1. E a inversao da consulta do bloco B:
# cada batida olha a ata de seu dia civil e da vespera.
# 1 candidata = identificada; 0 ou >1 = ambigua.
# A ata continua sendo a fonte da contagem, inclusive sem lista coerente.
#
# Esta sonda NAO implementa um juiz novo para as classes (a)-(f).
# Precedencia solicitada, ainda nao aplicada: a > b > d > e > c > f. [nao verificado]

from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
from time import monotonic
import re

from django.apps import apps
from django.db import transaction
from django.utils import timezone

from chamados.juizes import dia_do_fato
from chamados.models import ChamadoColaborador, PerguntaDisputa
from escala.models import CelulaDia
from escala.orfa_classe import FOLGA_DESMENTIDA, classificar_orfas
from escala.services.cadastro_realidade import lista
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.turnos import batidas_apuraveis

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
UM_DIA = timedelta(days=1)
ERROS = Counter()


class ReverterSonda(Exception):
    pass


def erro(etapa, exc):
    chave = (etapa, type(exc).__name__)
    ERROS[chave] += 1
    print("ERRO etapa={} tipo={}".format(*chave))


def limite(dia):
    return timezone.make_aware(
        datetime.combine(dia, time.min),
        timezone.get_current_timezone(),
    )


def local(ts):
    if timezone.is_naive(ts):
        raise ValueError("datetime sem timezone")
    return timezone.localtime(ts)


def instante(ts):
    return local(ts).isoformat()


def executar():
    # Inclui modelos que as funcoes da casa podem ler indiretamente.
    for modelo in sorted(apps.get_models(), key=lambda m: m._meta.label_lower):
        print("CAMPOS {} {}".format(
            modelo._meta.label,
            [f.name for f in modelo._meta.get_fields()],
        ))

    # Relogio de leitura para os leitores que o exigem; nao limita o universo.
    agora = timezone.now()
    hoje = local(agora).date()
    print("RELOGIO_LEITURA", instante(agora))
    print("UNIVERSO empresas=2,3,4 inicio={} fim={} corte_hoje=nenhum".format(
        INI.isoformat(), FIM.isoformat()))
    print("PRECEDENCIA a>b>d>e>c>f NAO_APLICADA")
    print("COMPARACAO_17_11 valor=1267 unidade=sobras_todas_as_classes "
          "comparavel_com_folga=nao")
    print("LISTA_ADMIN_AGREGACAO=colaborador")

    celulas = list(
        CelulaDia.objects.filter(
            colaborador__empresa_id__in=EMPRESAS,
            data__range=(INI, FIM),
            ata__n_orfas__gt=0,
            ata__tipo_dia__in=("folga", "ausencia"),
        )
        .select_related("colaborador", "colaborador__empresa")
        .order_by("colaborador_id", "data", "pk")
    )

    por_col = defaultdict(list)
    for cel in celulas:
        por_col[cel.colaborador_id].append(cel)

    # Os indicadores abaixo sao triestado: True, False ou None (desconhecido).
    donos = {}
    perguntas = None
    chamados = None
    listados = None

    try:
        perguntas = set(
            PerguntaDisputa.objects.filter(
                colaborador_id__in=por_col,
                data_evento__range=(INI, FIM),
            ).values_list("colaborador_id", "data_evento")
        )
    except Exception as exc:
        erro("perguntas", exc)

    try:
        chamados = set()
        # Nao se restringe por criado_em: o dia do fato nao e o da criacao.
        qs = ChamadoColaborador.objects.filter(
            colaborador_id__in=por_col
        ).select_related("celula").order_by("colaborador_id", "pk")
        for chamado in qs.iterator():
            try:
                dia = dia_do_fato(chamado, fortes=True)
                if dia:
                    if isinstance(dia, datetime):
                        dia = local(dia).date()
                    elif isinstance(dia, str):
                        dia = date.fromisoformat(dia[:10])
                    if INI <= dia <= FIM:
                        chamados.add((chamado.colaborador_id, dia))
            except Exception as exc:
                erro("dia_do_fato", exc)
                # Uma falha impede afirmar ausencia de chamado.
                chamados = None
                break
    except Exception as exc:
        erro("chamados", exc)
        chamados = None

    try:
        resultado_lista = None
        try:
            with transaction.atomic():
                resultado_lista = lista(
                    hoje=hoje, empresas=EMPRESAS, agora=agora
                )
                raise ReverterSonda()
        except ReverterSonda:
            pass
        listados = set()
        if resultado_lista:
            for _posto, linhas in resultado_lista.get("postos", []):
                for linha in linhas:
                    listados.add(linha["colab_id"])
        print("LISTA_ADMIN estado=retornou "
              "falhas_internas_capturadas_pela_funcao=[nao_verificado]")
    except Exception as exc:
        erro("lista_admin", exc)
        listados = None

    # Monta uma vez por colaborador e chama dono para TODOS os seus dias.
    for cid, cels in sorted(por_col.items()):
        inicio_tempo = monotonic()
        try:
            try:
                with transaction.atomic():
                    ins = InsumosDoDono(
                        cels[0].colaborador, INI, FIM, hoje, agora=agora
                    )
                    for cel in cels:
                        try:
                            valor = ins.dono(cel.data)
                            donos[(cid, cel.data)] = (
                                valor if valor in (
                                    "ESTRUTURA", "CADASTRO", "BATIDA"
                                ) else "nao_sei"
                            )
                        except Exception as exc:
                            erro("dono", exc)
                            donos[(cid, cel.data)] = "nao_sei"
                    raise ReverterSonda()
            except ReverterSonda:
                pass
        except Exception as exc:
            erro("insumos_dono", exc)
            for cel in cels:
                donos[(cid, cel.data)] = "nao_sei"
        finally:
            print("TEMPO_DONO col{} segundos={:.3f}".format(
                cid, monotonic() - inicio_tempo))

    total_orfas = 0
    identificadas = 0
    ambiguas = 0
    dono_total = Counter()
    destino_total = Counter()
    pessoas = set()
    dias_contados = 0

    for cid, cels in sorted(por_col.items()):
        colab = cels[0].colaborador
        indice = None
        try:
            indice = defaultdict(list)
            bs = batidas_apuraveis(
                colab, limite(INI), limite(FIM + timedelta(days=2))
            ).order_by("timestamp", "pk")
            for batida in bs.iterator():
                dt = local(batida.timestamp)
                indice[(dt.date(), dt.strftime("%H:%M"))].append(batida)
        except Exception as exc:
            erro("batidas_apuraveis", exc)
            indice = None

        for cel in cels:
            try:
                ata = cel.ata
                n = ata["n_orfas"]
                if isinstance(n, bool) or not isinstance(n, int) or n <= 0:
                    raise ValueError("n_orfas invalido")

                dias_contados += 1
                total_orfas += n
                pessoas.add(cid)
                chave = (cid, cel.data)
                dono = donos.get(chave, "nao_sei")
                dono_total[dono] += 1

                p = None if perguntas is None else chave in perguntas
                c = None if chamados is None else chave in chamados
                l = None if listados is None else cid in listados
                if True in (p, c, l):
                    destino = "com_destino"
                elif None in (p, c, l):
                    destino = "desconhecido"
                else:
                    # A lista pode ocultar falha interna: nao afirmar ausencia.
                    destino = "sem_destino_aparente_lista_nao_verificada"
                destino_total[destino] += 1

                horas = ata.get("orfas")
                coerente = (
                    isinstance(horas, list)
                    and len(horas) == n
                    and all(
                        isinstance(h, str)
                        and re.fullmatch(r"\d{2}:\d{2}", h)
                        for h in horas
                    )
                )

                if coerente:
                    porta = classificar_orfas({
                        "tipo_dia": ata.get("tipo_dia"),
                        "orfas": horas,
                    })
                    if any(v != FOLGA_DESMENTIDA for v in porta.values()):
                        raise ValueError("porta_folga_nao_confirmada")

                    # Repeticao da mesma hora na ata tambem e ambigua:
                    # nao atribui uma mesma batida a duas ocorrencias.
                    frequencia = Counter(horas)
                    for hora in horas:
                        candidatas = []
                        if indice is not None:
                            candidatas = (
                                indice.get((cel.data, hora), [])
                                + indice.get((cel.data + UM_DIA, hora), [])
                            )
                        if (
                            indice is not None
                            and frequencia[hora] == 1
                            and len(candidatas) == 1
                        ):
                            identificadas += 1
                        else:
                            ambiguas += 1
                else:
                    # Cada ocorrencia contada na ata permanece no universo.
                    ambiguas += n

                print(
                    "DIA col{} data={} orfas={} dono={} pergunta={} "
                    "chamado={} lista_colab={} destino={} classe=nao_classificado".format(
                        cid, cel.data.isoformat(), n, dono, p, c, l, destino
                    )
                )
            except Exception as exc:
                erro("celula", exc)

    print("UNIVERSO dias={} pessoas={} orfas={}".format(
        dias_contados, len(pessoas), total_orfas))
    print("IDENTIDADE identificadas={} ambiguas={}".format(
        identificadas, ambiguas))
    print("DONO_NAO_CLASSIFICADO", dict(sorted(dono_total.items())))
    print("DESTINO_NAO_CLASSIFICADO", dict(sorted(destino_total.items())))
    print("SOMA_IDENTIDADE_CONFERE={}".format(
        identificadas + ambiguas == total_orfas))
    print("SOMA_CLASSES_CONFERE=[nao_verificado]")
    print("NAO_CLASSIFICADO={}".format(dias_contados))
    print("ITEM2=INCOMPLETO")


try:
    executar()
except Exception as exc:
    erro("execucao", exc)
finally:
    print("ERROS_DETALHE", {
        "{}:{}".format(etapa, tipo): n
        for (etapa, tipo), n in sorted(ERROS.items())
    })
    print("ERROS={}".format(sum(ERROS.values())))
    print("FIM_ITEM2")
```

A regra acima trata horas repetidas na própria ata como ambíguas para evitar atribuir uma única batida a duas ocorrências; essa restrição adicional não consta expressamente da decisão da sessão **[nao verificado]**. Os valores de produção, a soma das seis classes e `NAO_CLASSIFICADO` só poderão ser conhecidos após a execução e a definição verificável das classes **[nao verificado]**.

## 18/11 19:43 · sessao 7cdc59c5f93f · RAIA-ESPURIAS · leitura · origem=Code

O ponto principal é que **tela e PDF podem classificar a mesma batida por fontes diferentes**: na competência, a tela normalmente lê a ata; o PDF chama novamente o construtor da grade e usa seu resultado para marcar as batidas impressas ([espelho.py:506](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/espelho.py:506), [pdf_espelho.py:193](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:193), [utils.py:2334](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:2334)). **Caso real atual: [não verificado].**

A leitura abaixo é da cópia identificada no pedido; não executei aplicação nem sondas, e não tenho acesso ao banco ou aos logs de produção. O anexo informa 1.267 sobras na medição de 17/11, mas não fornece os casos necessários para verificar os pares de leitores hoje: **número atual, diferença e causa: [não verificado]**. [nao verificado]

| Leitor | Fonte e alcance observado | Unidade |
|---|---|---|
| Espelho, marca `orfas_grade` | Normalmente `grade_da_celula` transporta `ata['orfas']`; ata antiga sem horas pode virar uma lista de `?`. Antes do início da apuração a tela usa o construtor; também cai nele se `remapear_na_regua` levantar `AtaAgregadaSemMarcos`. Sem grade ou marcos, o adaptador retorna lista vazia. ([leitor_celula.py:180](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:180), [leitor_celula.py:420](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:420), [espelho.py:519](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/espelho.py:519), [espelho.py:528](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/espelho.py:528), [utils.py:1095](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1095), [espelho.html:240](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/ponto/espelho.html:240)). | Hora órfã na linha de dia-colab. |
| PDF/cartão, marca de espúria | `grade_espelho_janela` chama `montar_grade_prevista_periodo`; o PDF compara as horas dessa grade com as batidas das linhas trazidas pelo espelho e imprime as marcadas separadamente. A comparação é por `HH:MM`, não por identificador da batida. ([pdf_espelho.py:193](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:193), [utils.py:2348](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:2348), [pdf_espelho.py:602](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:602), [pdf_espelho.py:606](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:606), [pdf_espelho.py:874](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:874)). | Batida marcada, agrupada por dia-colab. |
| `furos_diarios.apurar_colab` | Conta `len(orfas)` da grade lavrada, mas só devolve dias elegíveis: trabalho ou ausência parcial que cobra resto, até ontem, com marco previsto; também exclui isentos e escala sob a régua indicada. O ranking soma o campo persistido de `FuroDiario`, portanto é uma projeção e pode depender de quando foi alimentada. ([furos_diarios.py:12](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:12), [furos_diarios.py:25](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:25), [furos_diarios.py:123](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:123)). | Órfãs por dia-colab; ranking agrega por colab ou outro eixo. |
| Vigia de cluster | Lê diretamente a ata; exige pelo menos duas horas órfãs a até 15 minutos e uma lâmpada apagada, e consulta o juiz de dia sem trabalho. Ata sem as listas necessárias é **não medida**. ([cluster_espurio.py:15](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cluster_espurio.py:15)). | Dia-colab com assinatura; acompanha lista de horas órfãs. |
| Vigia pareador × célula | Compara `CelulaDia.ata` com `turnos_do_colab`: procura borda de turno fechado que também consta como órfã, com lâmpada de ponta apagada. Exclui turnos não posicionáveis ou com mais de um turno no dia e informa `cego`. ([vigia_pareador_celula.py:42](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:42), [vigia_pareador_celula.py:93](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:93), [vigia_pareador_celula.py:113](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:113)). | Borda de turno; comando também conta colaboradores distintos ([comando:60](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/vigia_pareador_celula.py:60)). |
| `sobras_sem_razao` do aval | **Implementação e universo da função real da medida de 17/11: [não verificado].** Não consegui identificá-la na leitura realizada; não equiparo os vigias acima a esse instrumento. | [não verificado]. |
| Lista Cadastro × Realidade | A assinatura C9 mostra uma classe específica de turno aberto e saída órfã separada pelo teto, a partir da lavra de `teto_na_lista`, que lê o carimbo do pareador; a lista também combina outras assinaturas e filtra vínculos/empresas visíveis. Não é demonstrada como lista de todas as sobras da ata. ([cadastro_realidade.py:99](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:99), [cadastro_realidade.py:869](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:869)). | Linha por colab, com assinatura C9 de dia e ocorrências ([cadastro_realidade.py:890](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:890)). |
| Placar estrutural R2 | Guarda texto e número de medida; declara `cadastro_realidade.lista` como fonte para remedição. A linha não é uma contagem ao vivo de todas as órfãs. ([placar_estrutural.py:32](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/placar_estrutural.py:32), [placar_estrutural.py:135](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/placar_estrutural.py:135)). | Colaboradores na medida R2; não é medida geral de órfãs. |
| API `api_espelho_v2` | Usa o espelho para dados e totais, e monta batidas por dia com a geometria de `turnos_do_colab`; na estrutura examinada, cada batida tem tipo, hora, timestamp e origem, sem marca de órfã. O app recebe batidas e status, mas uma classificação de sobra nessa resposta não foi encontrada. ([views.py:1350](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:1350), [views.py:1430](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:1430), [views.py:1461](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:1461), [views.py:1535](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:1535)). | Batida e dia; classificação explícita de órfã: [não verificado]. |

**Pares e sondas.** As sondas abaixo são propostas, **não executadas**. Usam datas fixas de 21/10 a 18/11/2026, que cabem na janela de competência 11 informada no aval; a janela efetiva por empresa deve ser conferida antes de interpretar ausência de caso. O envelope aceita `--sombra`, mas o próprio script documenta que mantém artefatos de infraestrutura e trilha: não o executei nesta tarefa de somente leitura de arquivos ([sonda_leitura.sh:4](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:4), [sonda_leitura.sh:29](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:29)).

**Tela × PDF.** Além da diferença ata versus construtor, o PDF só marca uma órfã quando encontra uma batida da linha com a mesma hora; a tela pode apresentar a hora da grade mesmo sem essa correspondência ([pdf_espelho.py:602](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:602), [utils.py:1121](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1121)). **Caso real: [não verificado].**

```python
import datetime as dt
from django.utils import timezone
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from ponto.models import Batida
from ponto.services.espelho import espelho_do_colab
from relatorios.pdf_espelho import _coletar_dados_espelho

INI, FIM = dt.date(2026, 10, 21), dt.date(2026, 11, 18)
AGORA = timezone.make_aware(dt.datetime(2026, 11, 19, 12))
for modelo in (Colaborador, CelulaDia, Batida):
    print("CAMPOS", modelo.__name__, [f.name for f in modelo._meta.fields])
print("INSTANTE", timezone.localtime(AGORA).isoformat())
erros, achou = 0, False
ids = CelulaDia.objects.filter(data__range=(INI, FIM)).values_list(
    "colaborador_id", flat=True
).distinct().order_by("colaborador_id")
for c in Colaborador.objects.filter(pk__in=ids).order_by("pk").iterator():
    try:
        tela = espelho_do_colab(c, FIM, INI, FIM, agora=AGORA)
        papel = _coletar_dados_espelho(c, INI, FIM, agora=AGORA)
        td = {d["data"]: d for d in tela["dias"] if INI <= d["data"] <= FIM}
        for d in papel["dias"]:
            dia = d["data"]
            if dia not in td:
                continue
            a = set(td[dia].get("orfas_grade") or [])
            b = {timezone.localtime(x.timestamp).strftime("%H:%M")
                 for x in d.get("batidas", []) if getattr(x, "orfa", False)}
            if a != b:
                print("DIVERGE col%s %s tela_n=%s pdf_horas_n=%s" %
                      (c.pk, dia.isoformat(), len(a), len(b)))
                achou = True
                break
        if achou:
            break
    except Exception as exc:
        erros += 1
        print("ERRO col%s %s" % (c.pk, type(exc).__name__))
print("CASO_ENCONTRADO=%s" % int(achou))
print("ERROS=%s" % erros)
```

**Ata × apuração de furos.** A mesma ata pode conter órfãs em folga ou outro dia excluído da apuração; nesse caso a ausência de linha em `apurar_colab` significa *fora do universo do contador*, não zero órfãs ([furos_diarios.py:32](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:32), [furos_diarios.py:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:49)). **Caso real: [não verificado].**

```python
import datetime as dt
from django.utils import timezone
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from escala.services.furos_diarios import apurar_colab

INI, FIM = dt.date(2026, 10, 21), dt.date(2026, 11, 18)
AGORA = timezone.make_aware(dt.datetime(2026, 11, 19, 12))
for modelo in (Colaborador, CelulaDia):
    print("CAMPOS", modelo.__name__, [f.name for f in modelo._meta.fields])
print("INSTANTE", timezone.localtime(AGORA).isoformat())
erros, achou = 0, False
ids = CelulaDia.objects.filter(data__range=(INI, FIM)).values_list(
    "colaborador_id", flat=True
).distinct().order_by("colaborador_id")
for c in Colaborador.objects.filter(pk__in=ids).order_by("pk").iterator():
    try:
        grade = grade_da_celula(c, INI, FIM)
        apurados = {x[0]: x[3] for x in apurar_colab(c, INI, FIM, hoje=AGORA.date())}
        for d in grade["dias"]:
            n = len(d.get("orfas") or [])
            if n and (d["data"] not in apurados or apurados[d["data"]] != n):
                print("DIVERGE col%s %s ata_n=%s apurado=%s tipo=%s" %
                      (c.pk, d["data"].isoformat(), n,
                       apurados.get(d["data"], "fora_do_universo"), d.get("tipo_dia")))
                achou = True
                break
        if achou:
            break
    except Exception as exc:
        erros += 1
        print("ERRO col%s %s" % (c.pk, type(exc).__name__))
print("CASO_ENCONTRADO=%s" % int(achou))
print("ERROS=%s" % erros)
```

**Ata × vigia de cluster.** Uma ou mais órfãs podem existir sem preencher a assinatura do vigia; uma ata sem listas também não pode ser tratada como ausência de cluster ([cluster_espurio.py:15](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cluster_espurio.py:15)). **Caso real: [não verificado].**

```python
import datetime as dt
from django.utils import timezone
from escala.models import CelulaDia
from ponto.services.cluster_espurio import _leitura_da_celula

INI, FIM = dt.date(2026, 10, 21), dt.date(2026, 11, 18)
AGORA = timezone.make_aware(dt.datetime(2026, 11, 19, 12))
print("CAMPOS CelulaDia", [f.name for f in CelulaDia._meta.fields])
print("INSTANTE", timezone.localtime(AGORA).isoformat())
erros, achou = 0, False
for cel in CelulaDia.objects.filter(data__range=(INI, FIM)).order_by(
        "colaborador_id", "data").iterator():
    try:
        n = len((cel.ata or {}).get("orfas") or [])
        if not n:
            continue
        resultado = _leitura_da_celula(cel)
        if not resultado:
            estado = "nao_medido" if resultado is None else "sem_assinatura"
            print("DIVERGE col%s %s ata_n=%s vigia=%s" %
                  (cel.colaborador_id, cel.data.isoformat(), n, estado))
            achou = True
            break
    except Exception as exc:
        erros += 1
        print("ERRO col%s %s" % (cel.colaborador_id, type(exc).__name__))
print("CASO_ENCONTRADO=%s" % int(achou))
print("ERROS=%s" % erros)
```

**Pareador × ata.** O próprio `censo` procura a divergência entre borda de par fechado e órfã lavrada, com filtros específicos; `cego=1` impede interpretar ausência de casos como confirmação de igualdade ([vigia_pareador_celula.py:42](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:42), [vigia_pareador_celula.py:97](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:97)). **Caso real: [não verificado].**

```python
import datetime as dt
from django.utils import timezone
from colaboradores.models import Colaborador, Empresa
from escala.models import CelulaDia
from ponto.models import Batida
from ponto.services.vigia_pareador_celula import censo

AGORA = timezone.make_aware(dt.datetime(2026, 11, 19, 12))
for modelo in (Colaborador, Empresa, CelulaDia, Batida):
    print("CAMPOS", modelo.__name__, [f.name for f in modelo._meta.fields])
print("INSTANTE", timezone.localtime(AGORA).isoformat())
erros = 0
try:
    r = censo(11, 2026)
    print("CEGO=%s" % r["cego"])
    if r["casos"]:
        cid, dia, borda, _hora = r["casos"][0]
        print("DIVERGE col%s %s borda=%s" % (cid, dia.isoformat(), borda))
    else:
        print("CASO_ENCONTRADO=0")
except Exception as exc:
    erros += 1
    print("ERRO", type(exc).__name__)
print("ERROS=%s" % erros)
```

Quanto aos juízes, o registro vivo declara a pergunta **“a batida é espúria (cluster)?” em `SEM_JUIZ`**, com decisões em sítios diferentes no detector e no cluster-guard; portanto há derivação fora de juiz único registrado para essa pergunta ([juizes.py:1224](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/juizes.py:1224)). Isso não demonstra, por si só, que toda divergência entre PDF e tela seja uma violação desse mesmo juiz: a classificação e os caminhos específicos precisam ser confrontados com o caso ([pdf_espelho.py:606](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:606), [espelho.py:519](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/espelho.py:519)).

O item **não está fechado**: faltam a identificação da função real de `sobras_sem_razao`, a conferência completa dos leitores “e afins”, sondas para os pares envolvendo lista/placar/API e a execução das sondas na sombra para apresentar casos reais. Tudo isso permanece **[não verificado]**.

## 18/11 19:42 · sessao 210aba8b0ce7 · RAIA-ESPURIAS · leitura · origem=Code

**A sonda completa está bloqueada pela identificação de D1.** O anexo informado dá o total de sete dias e três pessoas, mas não fornece as datas nem a função e o predicado da medida de 17/11. [não verificado] A cópia contém `escala.orfa_classe.classificar_orfas` e o leitor de cluster `dias_com_cluster`, mas não confirmei que qualquer um deles, isoladamente, reproduza a classe D1 daquela medida. [app/escala/orfa_classe.py:53; app/ponto/services/cluster_espurio.py:40]

A L-032 registra **90 minutos do marco de intervalo**; o pareador também compara a distância ao marco de borda. Esses são dois fatos distintos: o raio numérico consta da lei, enquanto a comparação adicional consta do código. [app/docs/LEIS.md:80; app/ponto/turnos.py:1246] O resultado por par, inclusive a distância fora do envelope, permanece [não verificado].

Abaixo vai uma **sonda parcial**, que imprime a função e o predicado reais de cluster, os dias que essa porta encontra e as batidas apuráveis ao redor deles. Ela marca esses dias como candidatos, não como os sete D1, e termina com erro explícito pela parte que falta. Não foi executada. [não verificado]

```python
# SONDA PARCIAL -- AUDITORIA-ESPURIAS item 5.
# Executar somente pelo envelope bin/sonda_leitura.sh --sombra <arquivo.py>.
# bin/sonda_leitura.sh:4-5, 54-59
#
# NAO identifica D1: a funcao e o predicado da medida de 17/11
# nao foram encontrados no material fornecido. [nao verificado]
#
# Porta de cluster: ponto/services/cluster_espurio.py:15,40
# Universo apuravel: ponto/turnos.py:2868
# Janela da competencia por empresa: ponto/janelas.py:54
# L-032: docs/LEIS.md:80, envelope de 90 min do marco de intervalo.
# Classe de orfas: escala/orfa_classe.py:28,40,53.

import datetime
import inspect

from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from escala.models import CelulaDia
from escala import orfa_classe
from ponto.models import Batida
from ponto.janelas import janela_fechamento
from ponto.services.cluster_espurio import (
    _leitura_da_celula,
    dias_com_cluster,
)
from ponto.turnos import batidas_apuraveis

ENVELOPE_L032_MIN = 90  # app/docs/LEIS.md:80
COLS = (252, 964, 444)
ERROS = 0


def erro(etapa, exc):
    global ERROS
    ERROS += 1
    # Apenas o tipo: a mensagem da excecao pode conter dados pessoais.
    print("ERRO etapa=%s tipo=%s" % (etapa, type(exc).__name__))


def instante(dt):
    return timezone.localtime(dt).isoformat()


def limite_local(dia):
    return timezone.make_aware(
        datetime.datetime.combine(dia, datetime.time.min),
        timezone.get_current_timezone(),
    )


def main():
    global ERROS

    # Nomes de campos antes das leituras dos modelos.
    for modelo in (Colaborador, Empresa, CelulaDia, Batida):
        print(
            "CAMPOS %s: %s"
            % (
                modelo._meta.label,
                ", ".join(f.name for f in modelo._meta.concrete_fields),
            )
        )

    print("L032 envelope_min=%s fonte=app/docs/LEIS.md:80" % ENVELOPE_L032_MIN)
    print(
        "JANELA_CLUSTER_MIN=%s fonte=app/escala/orfa_classe.py:28"
        % orfa_classe.JANELA_CLUSTER_MIN
    )
    for funcao in (
        orfa_classe._runs_de_rajada,
        orfa_classe.classificar_orfas,
        _leitura_da_celula,
        dias_com_cluster,
    ):
        print("FUNCAO %s.%s" % (funcao.__module__, funcao.__name__))
        print(inspect.getsource(funcao))

    print(
        "LIMITE: os dias retornados por dias_com_cluster sao candidatos "
        "de cluster; equivalencia com os 7 D1 [nao verificado]"
    )

    for cid in COLS:
        try:
            colab = Colaborador.objects.select_related("empresa").get(pk=cid)
            inicio, fim = janela_fechamento(11, 2026, colab.empresa)
            print("col%s competencia=11/2026 inicio=%s fim=%s" % (cid, inicio, fim))

            achados = dias_com_cluster(
                colab, None, fim, (fim - inicio).days + 1
            )
            print("col%s candidatos_cluster=%s" % (cid, len(achados)))

            for dia, orfas, missing in sorted(achados):
                try:
                    cel = CelulaDia.objects.get(colaborador_id=cid, data=dia)
                    dna = cel.dna if isinstance(cel.dna, dict) else {}
                    print(
                        "CANDIDATO col%s dia=%s orfas_hora_da_ata=%s "
                        "missing=%s trabalha=%s"
                        % (cid, dia, orfas, missing, cel.trabalha)
                    )
                    # Campos seguros e pertinentes do DNA; nao imprimir objetos
                    # ou repr de colaborador/batida, que podem conter nome.
                    print(
                        "DNA col%s dia=%s versao=%s marcos=%s "
                        "minutos_jornada=%s intervalo_livre=%s"
                        % (
                            cid, dia, dna.get("dna_versao"), dna.get("marcos"),
                            dna.get("minutos_jornada"), dna.get("intervalo_livre"),
                        )
                    )

                    # Contexto civil D-1 ate D+1. Nao afirma que todas essas
                    # batidas pertencem ao turno do dia.
                    ini = limite_local(dia - datetime.timedelta(days=1))
                    fim_exclusivo = limite_local(dia + datetime.timedelta(days=2))
                    bats = batidas_apuraveis(colab, ini=ini).filter(
                        timestamp__lt=fim_exclusivo
                    )
                    for batida in bats:
                        print(
                            "BATIDA_CONTEXTO col%s dia=%s instante=%s tipo=%s"
                            % (cid, dia, instante(batida.timestamp), batida.tipo)
                        )
                    print(
                        "PAR_S_E e ENVELOPE: [nao verificado]; "
                        "predicado D1 e atribuicao ao turno nao comprovados"
                    )
                except Exception as exc:
                    erro("dia_col%s_%s" % (cid, dia), exc)
        except Exception as exc:
            erro("col%s" % cid, exc)

    # Contabiliza a lacuna conhecida mesmo se todas as consultas funcionarem.
    ERROS += 1
    print(
        "ERRO etapa=identificacao_D1 tipo=FonteDaMedidaNaoVerificada "
        "esperado=7_dias"
    )


try:
    main()
except Exception as exc:
    erro("principal", exc)
finally:
    print("ERROS=%s" % ERROS)
    print("FIM_ITEM_5")
```

Por leitura de código, estes são os pontos relacionados que consegui verificar; **não é possível afirmar quais deles classificam os sete casos como intervalo sem as datas e os resultados da sonda D1**. [não verificado]

- `classificar_orfas` chama de `eco_cluster` uma corrida de ao menos duas órfãs em dia de trabalho com marco faltante; a corrida usa distância de até 15 minutos entre órfãs consecutivas. [app/escala/orfa_classe.py:26; app/escala/orfa_classe.py:40; app/escala/orfa_classe.py:53]
- `_leitura_da_celula`, usado por `dias_com_cluster`, chama de assinatura de cluster ao menos duas órfãs próximas e uma lâmpada apagada na ata. `pendencias` consome esses achados. [app/ponto/services/cluster_espurio.py:15; app/ponto/services/cluster_espurio.py:40; app/ponto/services/cluster_espurio.py:165]
- `papel_por_minuto_da_ata` usa `JANELA_CLUSTER_MIN` para reconhecer como eco de cluster uma batida próxima de luz acesa; esse predicado não é o mesmo do leitor de duas órfãs. [app/ponto/turnos.py:1565; app/ponto/turnos.py:1711]
- `_protegida_da_guarda` usa a mesma constante na exceção da guarda de rajada; o casamento de marcos contém a guarda chamada `cluster-guard`. [app/escala/utils.py:502; app/escala/utils.py:643]

Para fechar o item sem inventar a seleção, falta a função ou o texto da sonda da **medida de 17/11** que produziu os sete dias D1. [não verificado]

## 18/11 19:42 · sessao ef75106255e3 · RAIA-ESPURIAS · leitura · origem=Code

**Item 6 permanece aberto.** Não encontrei na cópia nem no anexo fornecido a porta da medida de 17/11, o intervalo exato da competência usado por ela ou os 73 pares identificados. Assim, não posso afirmar que uma consulta de pares reproduziria os **73 turnos / 34 pessoas**, nem conferir os **9 turnos reais colados antes**. [não verificado]

**Predicado de trabalho, ainda não verificado contra a medida:** turno formado pela função real da casa, com as duas pontas contendo `origem='disputa_s84_retro'` **ou** `pergunta_origem_id` preenchido; em cada ponta, a combinação e os campos devem ser impressos. A inclusão por `pergunta_origem` é necessária para não presumir que todo plantio conservou aquela origem; o campo também admite `NULL` para plantios antigos. [app/ponto/turnos.py:1850; app/ponto/models.py:33]

**Critério proposto de fase, não aplicado:** comparar, nos 14 dias anteriores a cada turno, os marcos das células de trabalho com os turnos completos formados a partir das batidas não plantadas; chamar “invertida” somente quando uma fase for inequivocamente diurna e a outra noturna. Os limites horários e o tratamento de fase mista não estão definidos no material fornecido. [não verificado] [nao verificado]

A sonda abaixo entrega a parte independente do universo: as células de **col366 entre 14 e 22/11/2026**, com DNA atual e anterior e carimbos de regeneração. Ela **não fecha o item** nem atribui causa à célula invertida. O DNA anterior conserva apenas a versão imediatamente anterior, de modo que não demonstra o histórico completo. [app/escala/models.py:1714]

A cópia registra que a sombra consultada em 18/11 às 18:47 era anterior à cura das 08:19; se essa ainda for a base usada pelo envelope, seus dados não demonstram o alcance da cura. A atualidade da sombra no momento da execução é [não verificado]. [app/docs/RELATO.md:28]

`_curar` grava arquivo e executa alteração de vínculo; não é chamada nesta sonda. [app/ponto/management/commands/censo_fase_12x36.py:339]

```python
# AUDITORIA-ESPURIAS, item 6 -- SONDA PARCIAL, NAO EXECUTADA.
#
# Predicado candidato do universo, NAO VERIFICADO contra a medida de 17/11:
# turno da funcao real com ambas as pontas satisfazendo, individualmente,
# origem == "disputa_s84_retro" OU pergunta_origem_id != None.
# Falta identificar janela, fonte das sobras e porta da medida.
#
# Criterio de fase proposto, NAO APLICADO: escala diurna x real noturno, [nao verificado]
# ou escala noturna x real diurno; limites horarios e fase mista indefinidos.
#
# Esta parte consulta exclusivamente CelulaDia de col366, 14 a 22/11/2026.
# Nao chama regeneracao, cura, construtor nem funcao que grave.
# Referencias: escala/models.py:1635, :1672, :1680, :1714;
# ponto/management/commands/gerar_celulas.py:73.

import datetime
import json

from django.utils import timezone
from escala.models import CelulaDia

erros = 1  # Item incompleto: universo e criterio de fase nao verificados.


def instante(valor):
    if valor is None:
        return None
    return timezone.localtime(valor).isoformat()


print("ITEM_6 SONDA_PARCIAL")
print("UNIVERSO=NAO_VERIFICADO; FASE=NAO_VERIFICADA")
print("ESPERADO_NO_ANEXO turnos=73 pessoas=34 reais_colados=9")
print("CAMPOS CelulaDia:", ", ".join(f.name for f in CelulaDia._meta.fields))

try:
    inicio = datetime.date(2026, 11, 14)
    fim = datetime.date(2026, 11, 22)
    celulas = list(
        CelulaDia.objects.filter(
            colaborador_id=366,
            data__gte=inicio,
            data__lte=fim,
        ).order_by("data", "pk")
    )
    por_dia = {}
    for cel in celulas:
        por_dia.setdefault(cel.data, []).append(cel)

    dia = inicio
    while dia <= fim:
        encontrados = por_dia.get(dia, [])
        if not encontrados:
            print("col366 data=%s CELULA_AUSENTE" % dia.isoformat())
        for cel in encontrados:
            try:
                dna = cel.dna
                marcos = (dna or {}).get("marcos")
                registro = {
                    "colaborador": "col366",
                    "celula_pk": cel.pk,
                    "data": cel.data.isoformat(),
                    "trabalha": cel.trabalha,
                    "origem": cel.origem,
                    "escala_geradora_id": cel.escala_geradora_id,
                    "gerada_em": instante(cel.gerada_em),
                    "editada_em": instante(cel.editada_em),
                    "regenerada_em": instante(cel.regenerada_em),
                    "regeneracoes": cel.regeneracoes,
                    "marcos_presentes": CelulaDia.marcos_presentes(marcos),
                    "dna": dna,
                    "dna_anterior": cel.dna_anterior,
                }
                print(json.dumps(registro, ensure_ascii=False, default=str))
            except Exception as exc:
                erros += 1
                print(
                    "ERRO col366 data=%s celula_pk=%s tipo=%s"
                    % (dia.isoformat(), cel.pk, type(exc).__name__)
                )
        dia += datetime.timedelta(days=1)
except Exception as exc:
    erros += 1
    print("ERRO leitura_col366 tipo=%s" % type(exc).__name__)

print("ALCANCE_CURA_COL366=NAO_VERIFICADO")
print("CAUSA_CELULA_INVERTIDA_COL366=NAO_VERIFICADA")
print("FASES_E_CONTAGENS_ITEM_6=NAO_MEDIDAS")
print("ERROS=%d" % erros)
print("FIM_ITEM_6")
```

A sonda não foi executada. Para completá-la é necessário recuperar a função e o recorte da medida de 17/11, além do critério de fase; até lá, listas e vereditos individuais permanecem [não verificado].

## 18/11 19:42 · sessao 3593b933b4bd · QUEIMA-CODEX-B · leitura · origem=Code

A sonda fornecida mostra que, nos 28 dias, a grade e a ata apontam os mesmos marcos faltantes; ela **não mostra** o veredito gravado, o julgamento atual nem os horários necessários para distinguir veredito velho de julgamento atual concorde ([RELATO.md:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:49); dados por dia no anexo `sonda28.20261118_172212.out`, numeração de linhas [nao verificado]). O item 5 do aval fornecido registra a opção 2 e pede essa medida adicional; a pergunta posterior mantém as duas opções abertas para este levantamento ([PROMPTS.md:2571](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/PROMPTS.md:2571), [RELATO.md:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:49)).

A árvore fornecida não tem metadados Git acessíveis no diretório informado; sua identidade com o hash pedido está [nao verificado].

**Opção 1 — (c) sai da raia e a divergência é atribuída a outro leitor, após censo.** A mudança expressa é de destino e investigação: retirar a obra (c) da raia e identificar o leitor responsável; a opção não especifica uma alteração de código ([RELATO.md:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:49), [BACKLOG.md:2352](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2352)). A divergência registrada de EFEITOS-REAIS #2 é entre **ata e veredito**: a ata acusa o marco e o emissor obedece ao veredito concorde ([RELATO.md:121](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:121), [RELATO.md:123](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:123)). O leitor exato usado pelo `guarda29` permanece [nao verificado].

Os leitores conferidos não fazem todos a mesma pergunta:

- [leitor_celula.py:350](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:350) expõe a grade a partir da ata; [utils.py:1089](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1089) expõe `missing`; [declaracao_texto.py:110](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/declaracao_texto.py:110) usa marcos faltantes para declaração, com filtros próprios.
- [supra_juiz.py:317](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:317) conta `missing` para julgar o dia; [fabrica_da_celula.py:120](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:120) lê o **veredito gravado** para decidir se cala; [furos.py:103](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/furos.py:103) usa status lavrado e ata para finalidades distintas. Assim, concordância no marco `missing` não demonstra concordância no julgamento ou no dever de perguntar ([supra_juiz.py:387](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:387), [fabrica_da_celula.py:120](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:120)).

**Opção 2 — (c) fica e a sonda mede o leitor usado pelo guarda29.** O aval fornecido determina comparar, por dia, veredito gravado e `julgada_em`, retorno atual de `classificar_dia` e datas dos últimos fatos; se o julgamento atual acusar e o gravado for anterior ao último fato, o ramo é “veredito velho”; se o atual também for concorde, pede o ramo que cala e pré-desenho próprio ([PROMPTS.md:2571](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/PROMPTS.md:2571); literal do anexo `reprovas_1811_1740.txt`, numeração de linhas [nao verificado]).

Os sítios para essa medida e eventual desenho são:

- [models.py:1745](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/models.py:1745) guarda `julgada_em`; [celula.py:1181](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/portas/celula.py:1181) mostra que o carimbo é idempotente e não necessariamente avança numa repetição sem mudança. Logo, seu significado é o do carimbo dessa porta, não uma prova isolada de que não houve execução posterior ([celula.py:1188](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/portas/celula.py:1188)).
- [cartorio.py:1398](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1398) contém a guarda de julgamento em dia; [cartorio.py:1416](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1416) prepara fatos da grade; [cartorio.py:1464](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1464) chama o juiz; [cartorio.py:1851](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1851) lavra o resultado e a ata.
- [supra_juiz.py:217](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:217) devolve códigos; o ramo de furo parcial depende de outros fatos além de `missing` ([supra_juiz.py:387](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:387), [supra_juiz.py:429](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:429)). Também chamam o juiz [cartorio.py:1269](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1269), [supra_juiz.py:223](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/supra_juiz.py:223) e [emitir_furo_retroativo.py:101](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/emitir_furo_retroativo.py:101). A equivalência de todos os insumos desses chamadores está [nao verificado].
- A fábrica lê `veredito`, `ata` e `julgada_em` para perguntas diferentes ([fabrica_da_celula.py:120](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:120)); a re-lavra das perguntas também usa marcos temporais da célula ([veredito_lavrado.py:186](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/veredito_lavrado.py:186)). Esses consumidores não tratam todos os campos como equivalentes. O censo completo dos leitores e escritores dos campos tocados está [nao verificado].

**Lei existente.** Nenhuma linha encontrada em `LEIS.md` ou `CORTES.md` decide expressamente se (c) sai ou fica, ou identifica o leitor usado pelo `guarda29`. Busquei `RAIA-INTERVALO`, `REPROVAS-1811-1740`, `EFEITOS-REAIS`, `guarda29`, `sonda28`, `projeção reconstruível`, `rejulgar`, `re-julgar`, `julgada_em` e `fato muda`. Há regras que respondem **parcialmente ao desenho condicional**:

- L-061: “O CONSUMIDOR LE A LAMPADA, nunca re-julga: chamado, pergunta, tela e folha leem a celula/ata” ([LEIS.md:95](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:95)).
- L-124: “fato novo rejulga normal em qualquer competencia” ([LEIS.md:145](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:145), [CORTES.md:461](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:461)). Isso não demonstra que houve fato posterior nos 28 dias [nao verificado].

| Opção | Sítios | Leitores | Lei existente |
|---|---|---|---|
| 1: (c) sai; censo nomeia leitor | [RELATO.md:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:49), [BACKLOG.md:2352](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2352) | Grade, juiz, fábrica e relatório usam os fatos para decisões distintas; leitor do guarda29 e censo completo [nao verificado] | Nenhuma linha encontrada decide a saída; L-061 trata consumidores ([LEIS.md:95](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:95)). |
| 2: (c) fica; medir julgamento | [cartorio.py:1398](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1398), [supra_juiz.py:217](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:217), [celula.py:1181](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/portas/celula.py:1181) | Cartório e comandos chamam o juiz; fábrica e re-lavra consomem campos gravados. Equivalência completa [nao verificado] | Nenhuma linha encontrada decide a permanência; L-124 responde ao caso de fato novo ([LEIS.md:145](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:145)). |

## 18/11 19:42 · sessao 98dbc781cb85 · RAIA-ESPURIAS · leitura · origem=Code

**Não consigo entregar uma sonda que garanta a partição pedida com `NAO_CLASSIFICADA=0` sem inventar a identidade ou o instante de algumas órfãs.** A decisão da sessão resolve qual porta usar; ela não acrescenta à ata os dados necessários às classes T1–T5. [app/escala/services/leitor_celula.py:180; app/ponto/services/cartorio.py:730]

- A ata grava as órfãs como strings de hora, sem identificador da batida, tipo E/S ou data do instante. A porta devolve `?` para cada órfã de uma ata que tenha apenas contagem. Assim, essas órfãs podem ser contadas, mas não há hora para medir distância nem tipo para decidir T5 ou T4. [app/ponto/services/cartorio.py:742; app/escala/services/leitor_celula.py:180]
- Consultar `batidas_apuraveis` fornece batidas com timestamp e tipo, mas essa função não estabelece qual delas corresponde a cada string da ata. Uma correspondência por minuto pode ter mais de uma candidata; o raio-x fornecido mostra, por exemplo, duas batidas de tipos opostos no mesmo minuto para `col67` em 14/11. Escolher uma exigiria uma regra de associação ainda não demonstrada. [app/ponto/turnos.py:2868; app/ponto/models.py:22; não verificado]
- A data da célula é a do dia da grade; atribuí-la automaticamente à hora da órfã também não resolve o instante em turnos que atravessam a meia-noite. O próprio dado fornecido mostra uma órfã apresentada no dia 10 com batidas após a virada para o dia 11. [app/escala/services/leitor_celula.py:375; não verificado]
- A função real `classificar_orfas` permite identificar `folga_desmentida` pela grade, mas classifica qualquer `tipo_dia` diferente de `trabalho` nessa classe, inclusive `indefinido`; não é equivalente, em todos os casos, ao juiz `dia_sem_trabalho`. Além disso, seu retorno é um dicionário por hora, portanto não preserva como entradas distintas duas órfãs com a mesma hora. [app/escala/orfa_classe.py:53; app/chamados/juizes.py:445]
- O rótulo T4 exige um par E→S de órfãs, mas o pedido não determina como escolher pares quando há várias candidatas; tampouco identifiquei uma função real que associe as strings da ata a batidas e devolva exatamente essa classificação. [app/ponto/services/cartorio.py:745; não verificado]

Há também uma diferença de definição que impede chamar o resultado de reprodução da medida de 17/11: o anexo daquela medida descreve T5 como **outra batida até 5 minutos**, enquanto a instrução atual determina **mesmo tipo até 10 minutos**. A instrução atual pode governar uma nova medida, mas sua diferença para 1.267 não pode ser atribuída a uma causa específica sem execução e sem o script perdido. [não verificado]

Para tornar a sonda completa e auditável, falta uma decisão sobre **como associar cada ocorrência de hora da ata a uma batida datada e tipada, inclusive candidatos múltiplos e virada de dia**, e como parear as ocorrências para T4. As órfãs sem hora ainda precisariam permanecer numa linha própria, sem afirmar uma das classes que dependem de hora ou tipo. Até essas decisões, `NAO_CLASSIFICADA` pode ser maior que zero; imprimir zero por construção ocultaria a falta de evidência. [app/escala/services/leitor_celula.py:180; não verificado]

Não executei sonda nem alterei arquivos. Não há resultado atual, `ERROS=0` ou causa comprovada para eventual diferença em relação a 1.267. [não verificado] [nao verificado]

## 18/11 19:42 · sessao 28146cf43027 · QUEIMA-CODEX-B · leitura · origem=Code

A pergunta de lei continua aberta: **não encontrei em LEIS.md ou CORTES.md uma linha que escolha A ou B ou forneça a frase literal da DXG**. O anexo também informa que a frase do corte de 12/11 não foi encontrada nos arquivos pesquisados; seu texto permanece [não verificado]. [anexo 5.linha; anexo censo_c2.r, item 5]

A árvore disponível não tem metadados Git acessíveis para confirmar o hash informado; a identidade exata da cópia é **[não verificado]**. A análise abaixo aponta os sítios existentes relacionados às candidatas; a implementação concreta da porta DXG, seus campos novos e todos os arquivos que ela acabaria alterando são **[não verificado]**. [anexo 5.linha; app/chamados/services/disputa_emissao.py:1584]

**Base comum às duas candidatas.** A reabertura de baixo nível limpa resposta e validação, registra a foto e o `motivo` em `ato_motivo`, limitado a 240 caracteres, e descarimba pela porta de lote; uma pergunta já vazia não produz essa linha de decisão. Isso permite reabrir uma resposta validada, mas não constitui, por si só, a entrega da frase ao colaborador. [app/chamados/services/disputa_emissao.py:1617; app/chamados/services/disputa_emissao.py:1639; app/chamados/services/disputa_emissao.py:1653; app/chamados/services/disputa_emissao.py:1656]

A API das perguntas devolve `p.motivo`, mas não `ato_motivo` nem `recusa_motivo`; a seleção separa pendentes e histórico pelo estado da pergunta e da disputa. No HTML examinado, `recusa_motivo` aparece no ramo de encerrada/validada, não como demonstração de que o motivo da reabertura aparece na pergunta pendente. A entrega no aplicativo instalado permanece **[não verificado]**. [app/api/views.py:2549; app/chamados/services/pergunta_no_app.py:80; app/templates/chamados/partials/_card_atendimento.html:179; anexo censo_c2.r, item 3]

A recusa existente exige motivo, mas para em `ja_validada`; a outra devolução citada no censo devolve ao admin, preservando a resposta. Assim, os sítios existentes não demonstram uma porta completa para a DXG. [app/chamados/services/validacao.py:884; app/chamados/services/validacao.py:898; app/chamados/services/validacao.py:185; anexo censo_c2.r, item 4]

**A — frase igual ao `ato_motivo`, livre, escrito pelo admin.** O efeito de apresentação seria entregar ao colaborador o texto gravado na decisão de reabertura; seria necessário ligar a entrada do admin e a decisão pertinente à representação e à tela que o colaborador usa. A entrada, a regra para escolher a decisão quando houver várias e os sítios exatos dessa ligação são **[não verificado]**. Os pontos existentes a examinar são a gravação em [disputa_emissao.py:1639](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1639), a API em [views.py:2549](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:2549) e o ramo pendente da tela web em [_card_atendimento.html:120](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/_card_atendimento.html:120). [anexo 5.linha]

Os leitores **não supõem todos “mensagem livre do admin ao colaborador”**: o modal do admin mostra o motivo como parte da trilha; a reabertura pelo próprio colaborador grava texto de origem do colaborador; a rejeição de ausência grava texto de sistema; e um leitor de materialização testa prefixo de `ato_motivo` em ato de devolução. Há ainda chamadores que não passam motivo. Expor genericamente qualquer `ato_motivo` como frase DXG mudaria, portanto, o significado atribuído a esses dados; o alcance de uma seleção específica DXG permanece **[não verificado]**. [app/templates/chamados/partials/modal_fio.html:205; app/templates/chamados/partials/modal_fio.html:212; app/chamados/models.py:2400; app/chamados/services/materializacao.py:1171; app/chamados/services/materializacao.py:275; app/chamados/services/acoes_disputa.py:105]

**B — frase fixa da casa mais o motivo.** Além da ligação de entrada, decisão e apresentação descrita em A, esta candidata precisa de uma frase fixa e de uma regra de composição com o motivo. O texto fixo, a ordem, a pontuação, o comportamento com motivo vazio e o local de composição são **[não verificado]**. A composição poderia afetar o texto exibido no modal se fosse gravada no próprio `ato_motivo`; se fosse feita apenas na apresentação, esse efeito seria diferente. A escolha do sítio é **[não verificado]**. [anexo 5.linha; app/chamados/services/disputa_emissao.py:1642; app/templates/chamados/partials/modal_fio.html:212]

Para ambas, `p.motivo` não deve ser presumido equivalente ao motivo textual da reabertura: a API o expõe como campo da pergunta e outros leitores o usam para classificar fatos. `recusa_motivo` também tem usos de pendência e auditoria; um leitor chega a extrair dele o identificador de uma batida. Esses leitores não têm todos o significado de frase da DXG. A lista completa de leitores de qualquer campo que venha a ser escolhido é **[não verificado]**. [app/api/views.py:2557; app/chamados/reconciliador.py:363; app/templates/chamados/partials/modal_fio.html:226; app/ponto/services/flip_auto.py:132]

Na busca em **LEIS.md e CORTES.md**, usei `DXG`, `RAIA.INTERVALO`, `12/11` com `15:0`, `frase fixa`, `com a frase` e `ato_motivo`: **nenhuma linha responde à escolha A/B ou fornece a frase DXG**. Há uma restrição relacionada, mas não uma resposta à escolha: CORTES registra literalmente **“ato_motivo com um significado so, com teste que morda”** para a obra F2. O modelo descreve esse campo como texto da trilha e distingue dele os campos usados para decisões de estado. [app/docs/CORTES.md:543; app/chamados/models.py:4183; app/chamados/models.py:4188]

| Opção | Sítios existentes relacionados | Leitores e significado | Lei existente para a escolha |
|---|---|---|---|
| A | Entrada DXG **[não verificado]**; gravação em [disputa_emissao.py:1639](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1639); apresentação em [views.py:2549](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:2549) e [_card_atendimento.html:120](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/_card_atendimento.html:120); alterações exatas **[não verificado]**. | Modal lê trilha; outros atos e chamadores usam motivos de origens distintas; materialização testa prefixo em devolução. Não há significado universal de mensagem livre do admin. [app/templates/chamados/partials/modal_fio.html:212; app/chamados/models.py:2400; app/chamados/services/materializacao.py:275] | **Nenhuma responde A/B**; restrição relacionada em [CORTES.md:543](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:543). |
| B | Os mesmos pontos de A, mais texto fixo e composição em sítio **[não verificado]**. [anexo 5.linha; app/api/views.py:2549] | Os mesmos de A; gravar a composição na trilha também mudaria o texto mostrado pelo modal. Sítio e efeitos completos **[não verificado]**. [app/templates/chamados/partials/modal_fio.html:212] | **Nenhuma responde A/B ou fornece a frase fixa**; restrição relacionada em [CORTES.md:543](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:543). |

## 18/11 19:41 · sessao 92e327131580 · - · - · origem=Ronald (tmux)

Levantamento de `classificar_dia`, da gravação e dos leitores enviado ao agente principal, com os limites de verificação.

## 18/11 19:41 · sessao 587846a6d3d3 · - · - · origem=Ronald (tmux)

A busca em [LEIS.md](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:145) e [CORTES.md](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:461) não encontrou linha que decida expressamente entre as opções 1 e 2. Termos buscados: `RAIA.INTERVALO`, `REPROVAS.1811.1740`, `EFEITOS.REAIS`, `guarda29`, `sonda28`, `projecao reconstruivel`, `rejulg`, `re-julg`, `julgada_em` e `fato muda`.

Há respostas gerais relevantes:

- [LEIS.md:145](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:145), L-124: “fato novo rejulga normal em qualquer competencia”. [CORTES.md:461](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:461) repete a regra. Ela não demonstra que os 28 vereditos estão velhos.
- [LEIS.md:95](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:95), L-061: “O CONSUMIDOR LE A LAMPADA, nunca re-julga”. A linha não identifica o leitor da divergência nem o leitor usado pelo guarda29.
- [LEIS.md:136](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:136), L-110: “a referencia e a REGRA”, e inclui entre as propriedades “todo leitor le o mesmo numero”.
- [LEIS.md:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:142), L-121, trata da reconstrução de projeções no pouso de cura de juiz; [LEIS.md:144](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:144), L-123, exige declarar o padrão realizado pela cura. Nenhuma dessas linhas decide a permanência de (c) na raia.

## 18/11 19:41 · sessao 3f8167dcb578 · - · - · origem=Ronald (tmux)

Levantamento enviado ao agente principal com os leitores de `missing`, os ramos de `classificar_dia`, os chamadores de produção e a distinção entre fato da ata e status lavrado. O censo completo de todos os leitores permanece **[nao verificado]**.

## 18/11 19:37 · sessao 262b866904af · QUEIMA-CODEX-B · leitura · origem=Code

A linha anexada traz **duas perguntas de lei**, não um menu explícito de opções para cada uma. Ela também fixa três classes por decisões existentes e mantém o restante como `INDEFINIDA` até o corte [dado fornecido: `3.linha`, sem arquivo:linha verificável]. A identidade desta cópia com o hash informado é **[não verificado]**.

Não há resultado de produção para col214, col263, col87 ou col975 no material fornecido: se suas batidas saem pareadas ou sozinhas, e o enquadramento de cada caso, são **[não verificado]**.

**Pergunta 1 — batida depois da saída final.** O pareador já forma turnos, mas sua resposta depende da alimentação recebida; isso não estabelece uma distância geral após a saída final que, por si só, transforme uma avulsa em novo turno. No laço, uma `E` não absorvida inicia `cur`; uma `S` fecha o turno corrente ou forma saída órfã quando não há turno corrente ([turnos.py:1192](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1192), [turnos.py:1402](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1402), [turnos.py:1491](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1491)). Antes disso, papel da ata, intervalo e outras guardas podem absorver a batida; sem mapa de papel, o pareador recorre ao tipo gravado ([turnos.py:1304](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1304), [turnos.py:1377](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1377)).

Os números relacionados respondem a perguntas distintas:

- `TETO_PAUSA_SEGUNDOS = 3 * 3600`, ou **3 horas**, é o padrão de `intra_max_s`; `cont_max_s` tem padrão **14 horas** ([turnos.py:377](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:377), [turnos.py:1192](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1192)). O motor pode fornecer outros valores: pausa de **0 horas** no turno partido ou **8 horas** no plantão, e continuidade de **26 horas** no plantão ([motor_calculo_v2.py:503](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:503), [motor_calculo_v2.py:728](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:728), [motor_calculo_v2.py:2859](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:2859), [motor_calculo_v2.py:3511](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:3511)).
- O envelope de fechamento usa jornada prevista **+ 4 horas**, ou o envelope recebido quando faltam bordas válidas; no laço de fechamento, o envelope de referência é **26 horas**, sujeito às guardas descritas no código ([turnos.py:66](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:66), [turnos.py:1421](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1421)).
- `VESPERA_LONGE_DO_INICIO_MIN = 90`, em **minutos**, participa da atribuição da data de um turno noturno; `_data_do_turno` também examina se a saída cabe no envelope da véspera. É uma decisão sobre **a qual dia pertence o turno formado**, não uma distância geral entre saída final e avulsa ([turnos.py:55](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:55), [turnos.py:248](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:248)).

A L-115 responde a uma figura específica: **“essa batida e a SAIDA do turno pela posicao”**, quando há pausa fechada e última batida posterior; ela não fornece uma regra geral para a avulsa *depois* da saída final ([LEIS.md:153](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:153)). A L-085 determina que a jornada pertence ao dia do início, mas isso também não resolve a condição geral de formação de um novo turno ([LEIS.md:117](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:117)). Para a pergunta completa, **nenhuma linha identificada responde**; foram buscados `RAZAO-DA-SOBRA`, `turno novo`, `novo turno`, `sobra`, `órfã` e `orfa` em LEIS e CORTES. Equivalentes não encontrados nessa busca são **[não verificado]**.

**Pergunta 2 — pausa, jornada adicional ou cadastro.** Já existem decisões parciais, com domínios diferentes:

- `_marca_intra_duracao` deduz pausa por duração; sem bordas, exige `S→E` dentro de `intra_max_s` e uma `S` posterior dentro de `cont_max_s`. Com bordas, há guardas adicionais de distância e continuidade ([turnos.py:385](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:385), [turnos.py:421](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:421)). O próprio pareador também reconhece uma pausa deslocada com prova de continuidade ([turnos.py:1458](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1458)).
- `papel_por_minuto_da_ata` considera sobras **dentro do vão**: proximidade de luz acesa até `JANELA_CLUSTER_MIN`, par restante sob `intra_max_s`, ou perna restante próxima de luz de intervalo. Se as condições não fecham, o dia segue sem aquele mapa de papel ([turnos.py:1711](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1711), [turnos.py:1735](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1735), [turnos.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1761)).
- `cadastro_realidade` apresenta, entre outras, as lavras C9 e C12; C12 mostra o dia publicado pelo motor como cadastro x realidade ([cadastro_realidade.py:100](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:100), [cadastro_realidade.py:128](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:128), [cadastro_realidade.py:360](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:360)). `dono_da_divergencia` atribui `CADASTRO`, `BATIDA` ou `ESTRUTURA` por insumos e precedência próprios; não é um classificador geral entre pausa e jornada adicional ([e6_oraculo.py:53](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/e6_oraculo.py:53), [e6_oraculo.py:98](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/e6_oraculo.py:98)).
- A L-032 diz que **“batida de intervalo fora do horario exato mas dentro do envelope E o intervalo”**; a L-084 diz que entrada **e** saída a mais de três horas dos respectivos marcos levam o dia a cadastro x realidade ([LEIS.md:80](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:80), [LEIS.md:116](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:116)). São respostas para figuras específicas. Para a pergunta completa sobre sobra que já não se enquadrou nas constantes, **nenhuma linha identificada responde**; foram buscados também `jornada adicional`, `cadastro inadequado`, `mais batidas que marcos`, `pausa` e `intervalo` em LEIS e CORTES. Equivalentes não encontrados são **[não verificado]**.

O significado dos leitores **não é uniforme**. `JANELA_CLUSTER_MIN = 15` serve ao classificador para agrupar órfãs consecutivas; no mapa da ata, a mesma constante mede proximidade de uma luz acesa dentro do vão ([orfa_classe.py:26](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:26), [orfa_classe.py:40](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:40), [turnos.py:1729](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1729)). O classificador retorna classes por `HH:MM`, trata qualquer `tipo_dia` diferente de `trabalho` como `FOLGA_DESMENTIDA` e hoje usa `ECO_CLUSTER`/`GEOMETRIA`; isso não é a mesma resposta que o mapa `X`/`Xi` do pareador ([orfa_classe.py:53](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:53), [turnos.py:1201](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1201)). O censo completo de leitores e equivalência semântica continua **[não verificado]**; o dado anexo traz uma matriz preliminar, não uma prova de completude.

A sonda abaixo **não foi executada**. Ela assume que `col214` e `col263` são PKs e que o ano é 2026: ambas as premissas são **[não verificado]**. A janela cobre cinco dias civis por caso, pela porta `batidas_apuraveis`; imprime o pareamento direto com parâmetros padrão e, separadamente, o caminho `turnos_do_colab`, que alimenta o pareador com contexto de ata e escala ([turnos.py:2868](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:2868), [turnos.py:1940](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1940), [turnos.py:1997](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1997)). Diferenças entre os dois resultados precisam ser interpretadas com essa alimentação; nenhum deles é resultado conhecido antes da execução.

```python
# Para manage.py shell no envelope de sombra somente leitura.
# NÃO executado. Confirmar ano e que colN corresponde a colaborador_id.
import copy
from datetime import date, datetime, time, timedelta

from django.utils import timezone
from ponto.turnos import batidas_apuraveis, parear_turnos, turnos_do_colab

CASOS = (
    (214, date(2026, 10, 29)),
    (263, date(2026, 10, 31)),
)

def local(b):
    if b is None:
        return None
    return {
        "instante": timezone.localtime(b.timestamp).isoformat(),
        "tipo": getattr(b, "tipo", None),
    }

def mostrar_turno(t):
    return {
        "data_turno": str(t.data_turno) if t.data_turno else None,
        "entrada": local(t.entrada),
        "saida": local(t.saida),
        "aberto": t.aberto,
        "cross_meianoite": t.cross_meianoite,
        "batidas": [local(b) for b in t.batidas],
    }

for col_id, dia in CASOS:
    # [dia - 2 dias, dia + 3 dias): cinco dias civis completos.
    inicio = timezone.make_aware(
        datetime.combine(dia - timedelta(days=2), time.min)
    )
    limite = timezone.make_aware(
        datetime.combine(dia + timedelta(days=3), time.min)
    )
    batidas = list(
        batidas_apuraveis(col_id, inicio, limite)
        .filter(timestamp__lt=limite)
    )

    print("\nCASO", f"col{col_id}", dia.isoformat())
    print("JANELA", inicio.isoformat(), limite.isoformat(), "(fim exclusivo)")
    print("BATIDAS_APURAVEIS", [local(b) for b in batidas])

    # Cópias: o pareador carimba atributos transitórios nas instâncias.
    diretos = parear_turnos([copy.copy(b) for b in batidas])
    print("PAREADOR_DIRETO_PADROES",
          [mostrar_turno(t) for t in diretos])

    # Caminho da aplicação para o dia, com sua própria carga e pad.
    contextualizados = turnos_do_colab(col_id, dia, dia)
    print("TURNOS_DO_COLAB_COM_CONTEXTO",
          [mostrar_turno(t) for t in contextualizados])
```

| Opção ou classe da linha | Sítios a considerar | Leitores e significado | Lei existente |
|---|---|---|---|
| `FOLGA_DESMENTIDA` | [orfa_classe.py:62](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:62) | Classificador usa `tipo_dia != trabalho`; equivalência para `indefinido` e todos os leitores **[não verificado]** | Lei específica para a classe **[não verificado]** |
| `ECO_PROXIMO_DE_LUZ` | [orfa_classe.py:28](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:28), [turnos.py:1742](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1742) | Rajada de órfãs e proximidade de luz são significados distintos | Lei específica para estender a classe **[não verificado]** |
| `PAR_DE_PAUSA_DEDUZIDO` / perna | [turnos.py:385](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:385), [turnos.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1761) | Pareador e mapa da ata aplicam guardas diferentes; motor fornece teto variável ([motor_calculo_v2.py:503](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:503)) | [L-032, parcial para intervalo cadastrado](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:80) |
| Após saída final: pergunta 1 | [turnos.py:1402](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1402), [turnos.py:1491](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1491) | Pareador direto e caminho alimentado por ata/escala podem receber contextos diferentes ([turnos.py:1940](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1940)) | Nenhuma linha identificada responde à condição geral; [L-115](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:153) e [L-085](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:117) são parciais |
| Excedente sem enquadramento: pergunta 2 | [turnos.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1761), [e6_oraculo.py:98](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/e6_oraculo.py:98) | Dedução de pausa e dono da divergência respondem perguntas diferentes; demais leitores **[não verificado]** | Nenhuma linha identificada resolve a separação geral; [L-032](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:80) e [L-084](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:116) são parciais |

## 18/11 19:37 · sessao e05d25598695 · QUEIMA-CODEX-B · leitura · origem=Code

**As duas perguntas de lei permanecem abertas no material conferido:** quem fecha o chamado órfão quando não nasce disputa; e o que fazer quando chega motivo novo cuja dona está fechada. O registro da árvore mantém as duas questões na F1 e registra a decisão “não nasce” para o primeiro caso. ([BACKLOG.md:2115](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2115))

Este é um **pré-desenho parcial**: os sítios e leitores abaixo foram conferidos, mas não completei o censo de *todos* os leitores e caminhos indiretos. Logo, a exaustividade pedida para cada opção é **[nao verificado]**. A identidade Git da cópia com o hash informado também é **[nao verificado]**.

1. **Órfão sem disputa — quem fecha?**

O órfão tratado pelo agrupador é um **chamado**, inclusive um chamado criado diretamente sem disputa; não é necessariamente uma `PerguntaDisputa` sem dona. A FK de `PerguntaDisputa` para disputa não é declarada anulável. ([disputa_emissao.py:804](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:804), [models.py:2603](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2603))

- **O que muda:** o ponto de decisão existente filtra chamados regularizáveis, escolhe uma disputa aberta do colaborador e, se não houver, cria uma antes do laço de perguntas. Ao fim, tenta fechá-la pelo C7. A decisão registrada de não nascer exige tratar esse caminho sem contar com a disputa recém-criada; o responsável pelo fechamento do chamado continua sendo a pergunta, não uma escolha demonstrada pelo código. ([disputa_emissao.py:1100](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1100), [disputa_emissao.py:1110](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1110), [disputa_emissao.py:1137](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1137), [disputa_emissao.py:1292](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1292))
- **Leitores e portas:** o agrupador é chamado pela ação administrativa, pelo app, pela cobrança do dia e por comandos de detecção; o reconciliador lê `chamados_orfaos_ids` e os estados dos filhos; existe uma porta humana de fechamento do chamado. O C7, porém, responde se **a disputa** pode fechar, a partir de suas perguntas, e não identifica por si quem deve fechar um chamado sem disputa. Portanto, esses leitores não respondem todos à mesma pergunta. ([acoes_disputa.py:226](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_disputa.py:226), [pergunta_no_app.py:261](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/pergunta_no_app.py:261), [cobrar_dia.py:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/cobrar_dia.py:49), [processar_alertas_turno.py:385](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/processar_alertas_turno.py:385), [detectar_ausencias.py:803](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/detectar_ausencias.py:803), [processar_alertas_avancados.py:159](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/processar_alertas_avancados.py:159), [reconciliador.py:239](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/reconciliador.py:239), [models.py:464](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:464), [models.py:2009](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2009))
- **Lei existente:** nas buscas em `LEIS.md` e `CORTES.md` por `órf`, `orf`, `disputa`, `raiz`, `fechar`, `dona` e combinações relacionadas, **nenhuma linha encontrada responde especificamente quem fecha esse órfão**. A exaustividade semântica da busca é **[nao verificado]**.

2. **Motivo novo com dona fechada — três opções**

O núcleo hoje procura uma disputa **aberta** por colaborador, agrega nela os motivos ou cria outra quando não encontra aberta; depois gera perguntas. Isso descreve o caminho conferido, mas não prova que ele implemente a decisão de lei para toda chegada de motivo com dona fechada. ([disputa_emissao.py:540](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:540), [disputa_emissao.py:601](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:601), [disputa_emissao.py:676](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:676))

- **Reabrir a dona.** O seletor e a decisão de emissão precisariam considerar a dona fechada. Já existem duas portas distintas: a administrativa exige usuário, limita reaberturas e pode acrescentar motivo; a de sistema não consome esse limite. Ambas limpam os campos de fechamento, com consequências para seus leitores. Qual porta, condições e demais sítios necessários: **[nao verificado]**. ([models.py:2289](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2289), [models.py:2326](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2326))
- **Nascer outra.** O núcleo e o agrupador já têm caminhos que criam disputa quando não acham aberta; é necessário confrontar a eleição da dona e o reuso de perguntas existentes, pois `_nascer` pode devolver uma pergunta existente e seu construtor de pergunta nova recebe a disputa. Quais alterações seriam suficientes para esta opção: **[nao verificado]**. ([disputa_emissao.py:601](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:601), [disputa_emissao.py:1137](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1137), [models.py:3198](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3198), [models.py:3206](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3206))
- **Pauta DP.** Seria necessário decidir onde detectar esse caso e encaminhá-lo à porta de pauta, com identidade e conteúdo do fato **[nao verificado]**. Há porta de escrita de pauta e há encaminhamento para DP em outro caso específico — limbo em competência entregue —, mas esse caso não demonstra que motivo novo de dona fechada já seja encaminhado. ([pautas/services.py:152](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/pautas/services.py:152), [fabrica_da_celula.py:210](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:210), [fabrica_da_celula.py:268](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:268))

**Leitores comuns às três opções:** `fechada_em` significa aberta quando nulo; o app usa esse estado para separar pendentes de histórico; o fio do admin inclui perguntas vivas de disputas fechadas; o vigia conta essas perguntas; e a regularização bloqueia por existência de disputa aberta do colaborador. Todos leem o mesmo estado de abertura, mas **não o usam para a mesma finalidade nem selecionam o mesmo universo**. A comparação de todos os demais leitores é **[nao verificado]**. ([models.py:1959](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:1959), [pergunta_no_app.py:80](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/pergunta_no_app.py:80), [fio.py:245](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fio.py:245), [vigia_fio.py:700](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/vigia_fio.py:700), [regularizacao_ext.py:57](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/regularizacao_ext.py:57))

**Lei existente:** o corte registra literalmente a **“pergunta de lei N5 (motivo novo chegando em disputa ja fechada)”**, sem escolher entre as três opções. Outro corte diz **“NAO reabrir em lote”** no tratamento das perguntas vivas já existentes em disputas fechadas; ele não responde expressamente à chegada de motivo novo. Nas buscas por `dona`, `fechada`, `motivo novo`, `reabrir`, `disputa`, `raiz` e `pauta` em `LEIS.md` e `CORTES.md`, nenhuma linha encontrada escolhe especificamente entre as três; exaustividade semântica **[nao verificado]**. ([CORTES.md:515](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:515), [CORTES.md:415](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:415))

| Opção | Sítios conferidos | Leitores conferidos | Lei existente para a pergunta |
|---|---|---|---|
| Não nasce disputa; quem fecha o órfão? | [agrupador:1100](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1100), [criação:1137](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1137) | [ação:226](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_disputa.py:226), [app:261](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/pergunta_no_app.py:261), [reconciliador:239](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/reconciliador.py:239) | Nenhuma linha encontrada sobre **quem fecha**; busca exaustiva **[nao verificado]** |
| Reabrir dona | [reabertura admin:2289](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2289), [sistema:2326](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2326) | App, fio, vigia e regularização, acima; censo completo **[nao verificado]** | Nenhuma escolha específica encontrada; [corte de vivas:415](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:415) trata de outro recorte |
| Nascer outra | [núcleo:601](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:601), [agrupador:1137](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1137), [pergunta:3206](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3206) | Os mesmos leitores de estado e dono, acima; censo completo **[nao verificado]** | Nenhuma escolha específica encontrada; [N5:515](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:515) registra a pergunta |
| Pauta DP | [porta:152](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/pautas/services.py:152); detector para esta opção **[nao verificado]** | Leitores de pauta e efeitos sobre emissão: censo **[nao verificado]** | Nenhuma escolha específica encontrada; [N5:515](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:515) registra a pergunta |

## 18/11 19:37 · sessao e392c37855b2 · QUEIMA-CODEX-B · leitura · origem=Code

A pergunta tem duas decisões independentes: **(i)** qual universo de motivos as três portas consultariam; **(ii)** qual comportamento teriam quando falta o dia do fato. Na cópia, a composição atual consulta apenas metades de intervalo e distingue ausência de dia de competência entregue; a testemunha atual também tem alcance mais estreito que o das três portas. ([materializacao.py:804](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:804), [materializacao.py:811](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:811), [materializacao.py:884](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:884))

O texto do aval, da revisão e da linha de pergunta foi recebido raspado no enunciado; sua localização e suas linhas originais são **[nao verificado]**. As referências abaixo são da árvore desta cópia.

### (i) Composição como está ou K6 para todo motivo

**Composição como está.** As três portas precisariam passar a consultá-la antes de suas escritas: `recusar_pergunta` antes de gravar o motivo e reabrir; `conferir_data_do_fato` antes dos carimbos; e `_aceitar_a_folga` antes dos carimbos e da declaração, inclusive no ramo em que o dia já é folga. O retorno antecipado para folga ocorre antes da consulta comum de validar. ([validacao.py:916](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:916), [validacao.py:996](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:996), [validacao.py:511](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:511), [validacao.py:551](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:551), [validacao.py:711](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:711))

Com o filtro atual, a consulta ignora qualquer pergunta cujo `motivo` não seja `intervalo_saida` ou `intervalo_volta`. Assim, a mera chamada não barra os demais motivos por entrega nem por ausência de dia. A identificação da folga é pela **resposta**, não pelo motivo; o efeito concreto depende do motivo da pergunta. ([materializacao.py:136](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:136), [materializacao.py:804](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:804), [validacao.py:465](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:465))

**K6 do dia para todo motivo.** Além dos três pontos de porta, seria necessária uma consulta que alcance os outros motivos, usando a competência do `data_evento` e o K6 existente. O sítio exato da mudança compartilhada ou de uma composição específica das portas **[nao verificado: depende do desenho escolhido]**. `competencia_do_fato` já combina a janela do dia com `competencia_entregue` e devolve `None` sem data; ela própria não filtra motivo. ([materializacao.py:788](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:788))

**Quem mais lê e significado.**

- `entrega_que_barra_a_validacao` é lida por validar antes da escrita e novamente após a materialização, e pela testemunha de aplicação. Alargar essa função compartilhada pode alterar também esses leitores; manter uma consulta específica às três portas teria alcance diferente. ([validacao.py:740](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:740), [validacao.py:777](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:777), [materializacao.py:901](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:901))
- `_k6_que_barra` também é lida por `_competencia_que_barra`, que a combina com a guarda F6; essa composição é usada na previsão e na materialização. Hoje sua documentação diz expressamente “metades de intervalo”: alargar o predicado compartilhado mudaria o universo esperado desses caminhos. ([materializacao.py:831](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:831), [materializacao.py:925](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:925), [materializacao.py:1867](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:1867))
- `competencia_do_fato` também alimenta a devolução de paridade, a triagem L-130 e o comando da pauta. Todos recebem a mesma forma `None` ou `(mês, ano, entregue)`, mas a usam para finalidades diferentes; não se pode concluir que uma ampliação de guarda deva alterar esses universos. ([validacao.py:338](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:338), [validacao.py:400](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:400), [bug_par_103.py:59](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/management/commands/bug_par_103.py:59))
- A testemunha é lida pelo card, pelo modal e pelo contador da pauta. Ela só chega ao derivador para resposta de tipo hora, respondida e ainda não materializada; portanto, não representa hoje todas as situações das três portas. Rejeitar no card e no modal e Conferir data no modal têm condições próprias, sem a consulta ao bloqueio nesses pontos. ([chamados_tags.py:131](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/templatetags/chamados_tags.py:131), [fio.py:376](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fio.py:376), [materializacao.py:342](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:342), [materializacao.py:884](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:884), [_pergunta_card.html:117](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/_pergunta_card.html:117), [modal_fio.html:163](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/modal_fio.html:163), [modal_fio.html:316](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/modal_fio.html:316))
- A casca de recusar classifica estados não enumerados como `sucesso`; um novo retorno de bloqueio exige examinar essa tradução. A casca de conferir lê `ok` e `msg`. Portanto, os leitores das duas portas não supõem o mesmo contrato de apresentação. ([acoes_disputa.py:81](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_disputa.py:81), [views.py:1313](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/views.py:1313))

**Lei ou corte existente.** Para as três portas e todos os motivos, **nenhuma linha encontrada responde à escolha inteira**. O corte das 163 responde explicitamente para a unidade “metade de intervalo” e diz: “competencia do fato entregue = BLOQUEADA_COMPETENCIA_ENTREGUE (L1), nada gravado”; o registro posterior mantém as seis portas humanas sem obra até corte. A L-113 altera a L-092 quanto ao recálculo em exportada, mas seu texto não escolhe entre estas duas implementações das três portas. Buscas: `portas humanas`, `recusar_pergunta`, `conferir_data_do_fato`, `aceitar_a_folga`, `metade de intervalo`, `U163`, `entregue` e `exportada`. ([CORTES.md:530](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:530), [CORTES.md:556](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:556), [LEIS.md:155](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:155))

### (ii) Sem dia do fato: recusa ou não?

**Recusa.** Nas três portas, a ausência de `data_evento` teria de ser tratada antes das escritas apontadas acima. No universo atual da composição, ela já devolve `BLOQUEADA_SEM_DIA_DO_FATO`, pois não consulta K6 sem data; `_recusa_da_entrega` preserva esse estado distinto. Para folga, já existe uma recusa própria `folga_sem_dia` antes das escritas: a escolha também afeta qual estado e mensagem prevalecem. ([materializacao.py:821](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:821), [validacao.py:228](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:228), [validacao.py:499](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:499))

**Não recusa por essa razão.** A nova guarda das portas teria de tratar separadamente o retorno sem dia, em vez de repassar todo resultado não vazio da composição. As demais regras de cada porta continuariam relevantes; em particular, o aceite de folga já recusa sem dia independentemente da nova guarda. A ordem exata entre essa regra e a nova consulta **[nao verificado: depende do desenho escolhido]**. ([validacao.py:499](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:499), [validacao.py:740](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:740))

**Quem mais lê e significado.** Validar e a testemunha usam os dois estados para distinguir “entregue” de “não foi possível consultar por falta de dia”; card e modal escondem Validar para ambos. O contador coloca o segundo no balde sem dia, enquanto o comando também lista perguntas sem dia para a pauta. Assim, chamar os dois casos de `bloqueada_competencia_entregue` mudaria o significado entendido por esses leitores. Por outro lado, se só algumas portas deixarem passar sem dia, a testemunha atual de validar não pode, sozinha, significar que **todas** as portas recusam. ([validacao.py:228](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:228), [_pergunta_card.html:47](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/_pergunta_card.html:47), [modal_fio.html:274](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/modal_fio.html:274), [materializacao.py:343](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:343), [bug_par_103.py:73](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/management/commands/bug_par_103.py:73))

**Lei ou corte existente.** Para as metades de intervalo das 163, há resposta literal: “sem dia do fato = BLOQUEADA_SEM_DIA_DO_FATO (L5)”. **Nenhuma linha encontrada responde se recusar e conferir data devem recusar sem dia para todo motivo**, nem resolve a precedência com `folga_sem_dia`. Buscas: `SEM.DIA.DO.FATO`, `sem dia do fato`, `portas humanas`, nomes das três portas e `U163`. ([CORTES.md:530](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:530), [CORTES.md:556](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:556))

| Opção | Sítios a considerar | Leitores e significado | Lei ou corte existente |
|---|---|---|---|
| (i) Composição como está | Três portas: [validacao.py:858](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:858), [validacao.py:957](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:957), [validacao.py:470](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:470); telas citadas acima | Guarda atual só consulta intervalo; testemunha tem alcance próprio. ([materializacao.py:804](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:804), [materializacao.py:884](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:884)) | Nenhuma linha encontrada escolhe para as três portas. ([CORTES.md:556](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:556)) |
| (i) K6 para todo motivo | Mesmas portas e telas; composição a definir **[nao verificado]**. ([materializacao.py:788](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:788)) | Alterar função compartilhada pode atingir validar, materialização e pauta, cujos universos não são idênticos. ([validacao.py:740](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:740), [materializacao.py:831](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:831)) | Nenhuma linha encontrada determina esse alcance para as três portas; corte localizado nas metades. ([CORTES.md:530](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:530)) |
| (ii) Recusa sem dia | Três portas, retorno e testemunha por botão; folga já tem recusa própria. ([validacao.py:499](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:499)) | Estado sem dia já é distinto e tem leitores de tela e contador. ([validacao.py:228](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:228), [materializacao.py:343](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:343)) | L5 responde para metades das 163; extensão às três portas: nenhuma linha encontrada. ([CORTES.md:530](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:530)) |
| (ii) Não recusa por falta de dia | Tratamento separado do resultado nas portas; precedência da folga **[nao verificado]**. ([materializacao.py:821](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:821), [validacao.py:499](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:499)) | A testemunha de validar não equivaleria automaticamente à decisão das outras portas. ([materializacao.py:892](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:892)) | Nenhuma linha encontrada responde para as três portas. ([CORTES.md:556](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:556)) |

## 18/11 19:36 · sessao 3012f86bda0b · QUEIMA-CODEX-B · leitura · origem=Code

A atribuição da migração de `pergunta_respondida` e de sua face SQL a uma fatia **permanece em aberto**: a E4c está descrita como fase de migração de leitores, a F1 estabelece o juiz e seus leitores, a F2 entrega uma leitura por ato para determinados usos, e a E4d exige que juízes registrados leiam o ato; as linhas consultadas não atribuem expressamente as duas funções a uma fatia específica. [app/docs/BACKLOG.md:2093; app/docs/BACKLOG.md:824; app/chamados/models.py:4295; app/docs/RELATO.md:10; não verificado]

A árvore disponível contém a pergunta em [RELATO.md:10](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:10). Os arquivos `logs/` citados nela não estão presentes nesta cópia; os literais dos avais e da revisão abaixo são os fornecidos no enunciado ou reproduzidos pelo RELATO. A identidade Git da cópia como o hash informado não pôde ser confirmada: a árvore não contém um repositório Git acessível. [não verificado]

**(i) O item 3 também retifica o selo 2 da E4c?**

- O ponto a mudar é o **alcance da regra e do selo**, não um sítio de código determinado pela pergunta: o RELATO registra a colisão entre “substituir SO o E4d” / “NAO muda: E4c” e a troca do selo 2. [RELATO.md:10](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:10)
- O BACKLOG ainda descreve a sequência anterior como “E4c LEITORES DIVERGE + selo 2 -> E4d parar projecao”; a linha da nova E4d registra o aval de projetor único. São dois registros cujo enquadramento depende da resposta. [BACKLOG.md:2093](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2093); [BACKLOG.md:2017](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2017)
- O selo antigo e os novos selos, como implementações ou consumidores concretos, não foram localizados; seus leitores e todos os sítios eventualmente afetados são **[não verificado]**. O juiz C6 está registrado e lê a projeção hoje, mas isso não demonstra qual selo futuro o abrangerá. [core/juizes.py:291](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/juizes.py:291); [chamados/juizes.py:1389](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1389)
- **Lei existente:** nenhuma linha encontrada responde expressamente se há retificação do selo 2. Busquei em `LEIS.md` e `CORTES.md` por `E4c`, `E4d`, `selo 2`, `E4-EXPAND-CONTRACT`, `projecao` e `RESPOSTAS-1811-1800`. A regra geral de reconstrução de projeções da L-121 não resolve essa retificação. [LEIS.md:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:142)

**(ii) Juiz e linha de exceção ficam fora do selo (b) e do contador até o fato próprio?**

- O possível alcance é duplo: **quais leituras de juiz entram no selo** e **quais linhas entram na comparação de reconstrução**. A pergunta não identifica implementação existente do selo ou contador; seus sítios futuros são **[não verificado]**. [RELATO.md:10](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:10)
- O exemplo verificável é `pergunta_viva`: a função temporal lê `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em` e `via_resolucao`, corta valores futuros quando recebe `agora`, e alimenta o cálculo do C6. [chamados/juizes.py:1382](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1382); [chamados/juizes.py:1403](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1403)
- Outros leitores não presumem todos o mesmo significado: `cadastro_realidade` usa o instante temporal de resposta para um ramo M1, sem testar texto nesse ponto; o juiz de resposta exige instante **e** texto não vazio. `q_vivas` é uma seleção SQL deliberadamente mais ampla que o julgamento exato de vitalidade. [cadastro_realidade.py:500](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:500); [chamados/juizes.py:1544](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1544); [models.py:3580](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3580)
- As seis exceções registradas são A03 reabertura administrativa, A05 assinatura, A06 duplicata/identidade, A17 pendência técnica de materialização, A18 recusa técnica de materialização e A20 conferência de data; cada linha mantém a projeção como fonte até o fato próprio. A correspondência exata de cada exceção com cada campo, juiz e linha do contador é **[não verificado]**. [BACKLOG.md:2097](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2097); [BACKLOG.md:2098](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2098); [BACKLOG.md:2099](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2099); [BACKLOG.md:2100](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2100); [BACKLOG.md:2101](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2101); [BACKLOG.md:2102](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2102)
- **Lei existente:** nenhuma linha encontrada responde expressamente à exclusão do juiz do selo **e** da linha do contador. Busquei `excecao`, `fato proprio`, `E4c`, `E4d`, `selo 2`, `projecao` e `reconstru` em `LEIS.md` e `CORTES.md`. A L-121 trata de reconstrução no pouso de cura de juiz, sem definir esse universo de exceções. [LEIS.md:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:142)

**Fatia que move as duas faces para o ato**

As candidatas e o que já dizem são:

| Candidata | Registro existente |
|---|---|
| **E4c, em geral** | É a fase de leitores divergentes e selo 2; a linha não nomeia a fatia dessas duas funções. [BACKLOG.md:2093](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2093) |
| **F1 nova / B2** | Institui um juiz para instante e texto e manda leitores B, C e D e a property delegarem; B2b tem 39 sítios pendentes segundo o registro. Isso não atribui expressamente a troca interna das duas faces para o ato. [BACKLOG.md:2021](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2021); [BACKLOG.md:824](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:824) |
| **F2** | `DecisaoPergunta.leitura` já deriva resposta e validação vigentes da sequência e inclui passivo da projeção sem linha; ela própria declara os leitores atendidos e uma face SQL **da validação**. Uma face SQL equivalente para resposta não está identificada nesses sítios. [models.py:4295](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:4295); [models.py:3539](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3539) |
| **F9** | O dado fornecido diz que o brief não toca juiz **[não verificado: brief ausente]**. O RELATO registra uma decisão específica de manter o seletor de limbo na projeção, não uma atribuição da migração do juiz. [RELATO.md:89](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:89) |
| **E4d, projetor único** | O item 3 fornecido exige juiz registrado lendo ato **[não verificado: arquivo do aval ausente]**; o BACKLOG situa a abertura da E4d depois de F6F7, F9, F5, F3 e B2b, sem atribuir esta migração a um projetor ou pouso concreto. [BACKLOG.md:2017](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2017) |

O sítio Python do juiz avalia os atributos do objeto **que recebe**; portanto, a fonte depende do chamador: a property `respondida` passa `DecisaoPergunta.leitura(self)`, enquanto o C6 passa uma tupla construída com a projeção temporal. A face SQL, por sua vez, consulta diretamente as colunas da pergunta. [chamados/juizes.py:1552](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1552); [models.py:3791](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3791); [chamados/juizes.py:1423](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1423); [chamados/juizes.py:1580](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1580)

Entre os demais leitores estão `q_vivas`, a fila de respostas a validar, a escolha da pergunta canônica e a prévia de folha; usam o predicado para decisões distintas, embora compartilhem a definição de resposta. A face negativa `q_nao_respondida` também depende do par projetado, de modo que sua equivalência precisa entrar na análise de uma eventual mudança da face positiva. O censo completo de leitores, inclusive indiretos e testes, é **[não verificado]**. [models.py:3597](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3597); [chamados/juizes.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1761); [fio.py:1274](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fio.py:1274); [previa.py:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/folha/previa.py:142); [chamados/juizes.py:1583](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1583)

A lei já fixa o **significado** de respondida: “tem instante (respondida_em) E texto nao vazio” e “Um juiz so para todas as finalidades”. O corte identifica também a face SQL. Nenhuma dessas linhas atribui a migração das duas funções a uma fatia; busquei `L-130`, `JUIZ-RESPONDIDA`, `E4c`, `E4d`, `projecao` e `reconstru` em `LEIS.md` e `CORTES.md`. [LEIS.md:151](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:151); [CORTES.md:518](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:518)

| Opção / pergunta | Sítios | Leitores | Lei existente |
|---|---|---|---|
| **(i) Retificação da E4c** | Registros de E4c e E4d: [BACKLOG:2093](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2093), [2017](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2017); implementação do selo **[não verificado]** | Consumidores do selo **[não verificado]** | Nenhuma linha encontrada responde à retificação; L-121 é geral. [LEIS:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:142) |
| **(ii) Exceções no selo e contador** | C6 e projeção temporal: [juizes.py:1382](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1382); selo/contador futuros **[não verificado]** | C6, M1 e SQL de vivas fazem perguntas distintas. [juizes.py:1403](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1403); [cadastro_realidade.py:502](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:502); [models.py:3569](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3569) | Nenhuma linha encontrada resolve ambas as exclusões; exceções registradas no [BACKLOG:2097](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2097). |
| **Fatia para juiz e SQL** | [juizes.py:1544](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1544), [1575](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1575), [1583](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1583) | Exemplos acima; censo completo **[não verificado]** | L-130 define significado e juiz único, sem nomear fatia. [LEIS:151](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:151) |

## 18/11 19:36 · sessao 49a9cd60665e · E4C-F6F7 · verificador · origem=Code

**Não aprovo a obra com base nesta verificação: o censo completo de escritores e leitores e a avaliação dos 15 achados não foram concluídos.** Há divergências estáticas que merecem triagem, mas seu alcance e, em alguns casos, a regra aplicável permanecem **[não verificado]**.

A cópia acessível não contém metadados Git: não foi possível confirmar o hash nem calcular o merge-base [não verificado]. O teste `test_e4c_f6f7_c13_caso_pela_regra.py` não foi encontrado na árvore acessível; seu corpo foi fornecido no anexo, mas sua presença na cópia e sua execução ficam [não verificado]. Não executei testes nem mutantes; os resultados abaixo são de inspeção estática. [nao verificado]

### Fecho por achado dos postmortens

A coluna “situação” trata da **classe no sítio desta obra**, não declara que o caso do postmortem se repete. “Não concluído” não equivale a “não ocorre”.

| Achado | Classe | Sítio nesta obra e prova | Situação |
|---|---|---|---|
| F6F7-1: significado entre leitores | (a) | A tarefa é projetada para a folha em [fora_da_folha.py:70](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/fora_da_folha.py:70); a folha também agrupa por pessoa em [fora_da_folha.py:94](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/fora_da_folha.py:94). O teste c13 anexado compara a projeção e `_fio_da_porta`, mas não demonstra efeito em grupos ou frota [não verificado]. | **BLOQUEIA a conclusão do censo** |
| F6F7-2: função sem chamador | (a) | O contador é chamado pelo vigia em [vigiar_fio_morto.py:192](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:192) e seu número entra no carimbo em [vigiar_fio_morto.py:262](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:262). | **Não ocorre nesse contador** |
| F6F7-3: sequência interrompida | (a) | O laço só recua por respostas contíguas que mantêm validação em [tarefa_do_dia.py:163](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:163); a leitura do ato conserva validação através de `SILENCIO` em [models.py:4272](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4272). A prioridade exigida para a tarefa nessa sequência é [não verificado]. | **FILA**, divergência sintética a decidir |
| F6F7-4: mutação indireta no selo | (a) | O selo examina escrita e chamadas cuja raiz é o nome `perguntas` em [test_contract_pergunta_viva.py:46](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_contract_pergunta_viva.py:46). O teste enumera vários mutantes diretos em [test_e4c_f6f7_c7_sem_banco.py:88](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:88). Alcance por alias ou chamada indireta e os quatro mutantes anteriores: [não verificado]. | **FILA**, selo não demonstrado exaustivo |
| F6F7-5: omissão de pergunta | (e) | O postmortem fornecido não identifica pergunta nem resposta para este item [não verificado]. Os pontos de leitura por fora indicados abaixo são candidatos desta classe, não prova de que sejam o mesmo caso [não verificado]. | **BLOQUEIA a conclusão da verificação** |
| F6F7-6: evidência de teste/fixture | (c) | O teste de endpoint do passivo só verifica `True` em [test_f6_u4_aguarda_admin_pela_tarefa.py:67](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:67); o teste unitário verifica também `False` em [test_f6_u4_aguarda_admin_pela_tarefa.py:80](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:80). `ERROR` da fixture e execução atual: [não verificado]. | **FILA**, sem resultado de execução |
| Escala-1: mutante sobrevive ao RED | (a) | Um mutante `fim_medido = None` passaria pelo teste que só confere contador e universo em [test_residuo_alem_da_competencia.py:114](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/tests/test_residuo_alem_da_competencia.py:114), por inspeção; outro teste confere o máximo em [test_cadastro_futuro_sem_banco.py:800](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/tests/test_cadastro_futuro_sem_banco.py:800). Sobrevivência à suíte: [não verificado]. | **FILA**, teste isolado sem essa mordida |
| Escala-2: rótulo LEI-AKITA | (a) | A linha exige `juizes novos` em [CLAUDE.md:63](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/CLAUDE.md:63). Sem histórico Git, a linha de cada commit da obra e sua correspondência com o diff são [não verificado]. | **BLOQUEIA a conclusão do selo** |
| Escala-3: segundo juiz herdado | (a) | A face SQL de validação reexpressa a regra Python em [models.py:3551](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:3551) e [models.py:4292](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4292). Equivalência para todo estado e eventual caracterização como juiz duplicado: [não verificado]. | **FILA**, equivalência integral não demonstrada |
| Escala-4: teste não chega à porta | (a) | O teste unitário de `aguarda_admin_pela_tarefa` chama diretamente a função em [test_f6_u4_aguarda_admin_pela_tarefa.py:76](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:76); há testes de endpoint separados em [test_f6_u4_aguarda_admin_pela_tarefa.py:52](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:52). Cobertura de todos os ramos até a porta: [não verificado]. | **FILA** |
| Escala-5: teste não confere um campo | (a) | O teste do contador confere o objeto de resultado em [test_f7_contador_validada_sem_linha.py:38](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_contador_validada_sem_linha.py:38); o censo de todos os campos de trilha alterados e respectivos asserts é [não verificado]. | **FILA** |
| Escala-6: lavra vencida | Sem classe compatível | A carga usa `esta_stale` para aceitar a lavra em [tarefa_do_dia.py:416](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:416). Correção para todas as causas de vencimento e teste correspondente: [não verificado]. | **FILA** |
| Escala-7: literal do aval | (e) | O alcance futuro usa `data__gt=hoje`, sem teto, em [cadastro_futuro.py:21](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/cadastro_futuro.py:21). A conferência integral contra os avais referidos no diff é [não verificado]. | **BLOQUEIA a conclusão da revisão do aval** |
| Escala-8a: rótulo do selo | (a) | O selo AST e seus mutantes estão em [test_contract_pergunta_viva.py:46](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_contract_pergunta_viva.py:46) e [test_e4c_f6f7_c7_sem_banco.py:88](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:88). Rótulo e alcance de **todos** os selos da obra: [não verificado]. | **FILA** |
| Escala-8b: citação de lei | (e) | Correspondência completa das citações de lei com o diff e com os selos: [não verificado]. | **BLOQUEIA a conclusão da revisão documental** |

As classificações “BLOQUEIA a conclusão” indicam **verificação incompleta**, não uma demonstração independente de defeito funcional.

### Escritores, leitores e significado: recorte verificado

Este é **um censo parcial**, não a resposta solicitada para todo campo, chave e função do diff. Os demais escritores, leitores e sua concordância na árvore inteira permanecem [não verificado].

- **`resposta_mantem_validacao`:** a resposta humana grava `False` em [disputa_emissao.py:1534](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/disputa_emissao.py:1534); portas automática e de trilha gravam `True` em [disputa_emissao.py:1688](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/disputa_emissao.py:1688) e [disputa_emissao.py:1710](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/disputa_emissao.py:1710); a fotografia da projeção grava `True` em [models.py:4260](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4260), e a migração histórica usa motivo não vazio em [0056:14](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:14). Leem-no a leitura Python, a face SQL e a tarefa em [models.py:4292](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4292), [models.py:3551](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:3551) e [tarefa_do_dia.py:164](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:164). A diferença em sequências interrompidas consta no achado F6F7-3.

- **`da_ressuscitacao`:** a ressuscitação grava a marca em [models.py:1699](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:1699); o predicado a lê em [models.py:4221](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4221), e a carga exclui esses atos em [tarefa_do_dia.py:399](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:399). Exclusividade em toda a árvore: [não verificado].

- **Passivo e `validada_projetada_sem_linha`:** a correspondência usa pergunta, ato e instante em [models.py:4254](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4254). O contador usa essa autoridade em [backfill_decisoes_pergunta.py:108](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/backfill_decisoes_pergunta.py:108); o vigia o lê e grava no carimbo em [vigiar_fio_morto.py:192](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:192) e [vigiar_fio_morto.py:262](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:262). Outros consumidores do carimbo e interpretação externa: [não verificado].

- **`hoje`, `ini`, `fim` e `ultimo_dia_medido`:** a porta seleciona células geradas com data civil **estritamente posterior** a `hoje` em [cadastro_futuro.py:21](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/cadastro_futuro.py:21). O vigia inclui células a partir de `ini`, sem usar `fim` como teto, e calcula `ultimo_dia_medido` como máximo ou `None` em [vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/vigia_cadastro_futuro.py:30) e [vigia_cadastro_futuro.py:46](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/vigia_cadastro_futuro.py:46). O detalhe é gravado e recuperado em [vigia_cadastro_futuro.py:107](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/vigia_cadastro_futuro.py:107) e [vigia_cadastro_futuro.py:119](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/vigia_cadastro_futuro.py:119). Concordância de todos os chamadores e leitores externos: [não verificado].

- **`agora` e `dias_esperando`:** a borda da fila obtém um instante em [views_mensageria.py:543](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/views_mensageria.py:543); o item calcula dias inteiros pela diferença entre instantes em [views_mensageria.py:572](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/views_mensageria.py:572). Se todos os consumidores esperam dias transcorridos, e não mudança de dia civil, é [não verificado].

### Achados novos desta volta

1. **FILA — leitura da projeção ao perguntar ao juiz de resposta.** `validar_inline` passa a pergunta ao juiz em [views_cobrar.py:170](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/views_cobrar.py:170) e [views_cobrar.py:178](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/views_cobrar.py:178); o juiz lê os campos recebidos em [juizes.py:1552](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/juizes.py:1552), enquanto a property entrega a leitura da sequência em [models.py:3791](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:3791). Se projeção e sequência divergirem, as respostas podem diferir; alcance real e regra para essa porta são [não verificado]. Um teste apenas com projeção e sequência concordantes passaria com a divergência; teste específico que sobreviva: [não verificado].

2. **FILA — modal mistura resposta da sequência com campos projetados.** A condição `perg.respondida` usa a property, mas o texto, o instante e o selo de validação são lidos diretamente em [modal_fio.html:217](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/templates/chamados/partials/modal_fio.html:217) e [modal_fio.html:345](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/templates/chamados/partials/modal_fio.html:345). O teste F7 cria projeção sem resposta e valida o rótulo em [test_f7_validada_conservada.py:44](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_validada_conservada.py:44); ele não demonstra o comportamento quando uma sequência posterior contradiz essa projeção. Existência e apresentação de casos reais: [não verificado].

3. **FILA — teste do endpoint não morde passivo sempre verdadeiro isoladamente.** Substituir o ramo passivo por `True` preservaria a expectativa do teste de endpoint em [test_f6_u4_aguarda_admin_pela_tarefa.py:67](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:67), por inspeção; o teste unitário de `False` em [test_f6_u4_aguarda_admin_pela_tarefa.py:80](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:80) deve rejeitar esse mutante, mas execução e resultado efetivo são [não verificado].

A verificação de todos os juízes registrados, de todos os campos e funções do diff e de todos os mutantes pedidos permanece **[não verificado]**. Não há base neste retorno para afirmar aprovação, ausência de outros bloqueantes ou impacto em produção.

## 18/11 19:35 · sessao a0368c88b4f9 · ESCALA-NAO-DESCREVE · verificador · origem=Code

**Resultado parcial: há uma lacuna de teste confirmada por inspeção, uma classificação cíclica duplicada e uma proposta antiga que continua aplicável sem verificação de idade.** Não posso dar um aval completo: não concluí o censo de **todos** os campos, chaves e funções do diff, nem a análise de todos os juízes e mutantes. Esses pontos ficam **[não verificado]**. [nao verificado]

A cópia não contém metadados Git; portanto, a identidade do hash e o merge-base com `main` não foram confirmados. Usei o diff fornecido e as linhas da cópia presente. Não executei testes nem mutantes; as conclusões sobre sobrevivência são por inspeção. Produção, banco e logs: **[não verificado]**. [nao verificado]

### Achados da árvore

- **FILA — classificação de destino cíclico duplicada.** O botão classifica pela pertinência a `TipoEscala.CICLICOS` em `app/escala/services/cadastro_realidade.py:849`; a porta repete a classificação em `app/escala/regua_defesa.py:313`; e `julgar_vinculo` contém ainda a tupla literal `('12x36', '24x48')` para exigir âncora em `app/colaboradores/services/vinculo.py:128`. As condições não são idênticas: a última se aplica à criação sem âncora. Hoje a constante contém os mesmos dois valores em `app/escala/models.py:386`; a possibilidade de divergência futura decorre das cópias. O teste R5 passaria se apenas a recusa de `regua_defesa` fosse removida, pois o POST é barrado antes em `app/escala/views.py:498` e o teste só verifica que não houve vínculo em `app/escala/tests/test_escala_nao_descreve.py:299`. Sobrevivência executada: **[não verificado]**.

- **FILA — proposta de lavra antiga pode ser aplicada.** `ler_lavra` devolve o snapshot mais recente disponível sem comparar a data com hoje em `app/ponto/services/esmeril_espelho.py:371`; o POST lê proposta e assinaturas dessa lavra e verifica o botão, mas não sua idade, em `app/escala/views.py:493`. A data da lavra é registrada no motivo em `app/escala/views.py:504`. Se isso viola um prazo autorizado ou ocorreu em produção: **[não verificado]**. O teste de lavra antiga remove as chaves novas, mas não exercita uma lavra de outro dia **com** proposta em `app/escala/tests/test_escala_nao_descreve.py:358`.

- **FILA — o teste R1 não fixa a borda da janela independentemente.** A produção filtra `data__gte=ini` e `data__lte=fim` em `app/ponto/services/esmeril_espelho.py:99`; a fixture põe células em 10 e 11/11 em `app/escala/tests/test_escala_nao_descreve.py:63`, e compara o `desde` com a própria função `janela` em `app/escala/tests/test_escala_nao_descreve.py:94`. Por inspeção, um mutante que deslocasse o início da janela, preservando essas células no intervalo, poderia passar nesse teste; a execução e o efeito sobre a bateria inteira são **[não verificado]**.

- **FILA — o selo antigo de agrupamento não prova agrupamento.** Ele verifica a presença do nome do posto em `app/escala/tests/test_cadastro_x_realidade.py:53`, enquanto o template apresenta posto como coluna em `app/templates/escala/cadastro_x_realidade.html:26`. O nome do teste ainda figura como selo de L-040 em `app/docs/LEIS.md:88`. A mudança de layout está declarada na própria L-040; o problema observado é o alcance do nome e da asserção do selo.

### Escritores, leitores e significado: recorte verificado

Isto **não é o censo completo solicitado**; os demais campos, consumidores indiretos e compatibilidade global ficam **[não verificado]**.

| Dado ou função | Escritor e leitores encontrados | Significado e limite verificado |
|---|---|---|
| `ata.orfas` | O cartório compõe a ata em `app/ponto/services/cartorio.py:738`; `assinaturas_do_colab` lê em `app/ponto/services/esmeril_espelho.py:130`. | A carga soma o número de itens, inclusive horas não interpretáveis, em `app/ponto/services/esmeril_espelho.py:131`. Outros escritores e leitores na árvore inteira: **[não verificado]**. |
| `medida['orfas']`, `orfas`, `orfas_desde` | Escritos em `app/ponto/services/esmeril_espelho.py:142` e `app/ponto/services/esmeril_espelho.py:236`; lidos pela lista em `app/escala/services/cadastro_realidade.py:969`. | Contagem por pessoa; zero é medida sem órfãs; ausência da chave é sem medida. A janela consulta datas de célula com ambas as bordas inclusivas e termina ontem: `app/ponto/services/esmeril_espelho.py:38`, `app/ponto/services/esmeril_espelho.py:63`, `app/ponto/services/esmeril_espelho.py:99`. Equivalência entre dia da célula, dia civil e dia do turno para todas as órfãs: **[não verificado]**. |
| `proposta` e `destino` | `propor_escala` é chamado em `app/ponto/services/esmeril_espelho.py:241`; a lavra escreve mapa estruturado e frase em `app/ponto/services/esmeril_espelho.py:242`; a lista e o POST leem a proposta em `app/escala/services/cadastro_realidade.py:971` e `app/escala/views.py:494`. | A proposta cobre os 60 dias até ontem em `app/ponto/services/esmeril_espelho.py:241`; a carga usa outra janela em `app/ponto/services/esmeril_espelho.py:233`. O ciclo da proposta não é guardado; o botão consulta o cadastro atual em `app/escala/services/cadastro_realidade.py:819`. Todos os demais consumidores e sua interpretação: **[não verificado]**. |
| `carga`, `carga_desde`, `carga_texto`, `linhas`, `carga_resumo` | A lista monta a linha em `app/escala/services/cadastro_realidade.py:981`, ordena em `app/escala/services/cadastro_realidade.py:1055` e publica o resumo em `app/escala/services/cadastro_realidade.py:1067`; tela e PDF leem a frase em `app/templates/escala/cadastro_x_realidade.html:17` e `app/escala/services/cadastro_realidade.py:1184`. | O resumo soma cargas positivas das linhas listadas, usa a menor data delas e conta sem medida separadamente em `app/escala/services/cadastro_realidade.py:788`. Ele não representa necessariamente toda a frota: o universo das linhas é formado a partir de assinaturas e cadastro em `app/escala/services/cadastro_realidade.py:961`. |
| `postos` e `linhas` | Ambos saem de `lista` em `app/escala/services/cadastro_realidade.py:1062`; a tela e o PDF usam `linhas`, enquanto o vigia usa `postos` em `app/chamados/services/vigia_fio.py:196`. | São duas apresentações dos registros montados pela lista, com ordenações diferentes, conforme `app/escala/services/cadastro_realidade.py:1055`. Todos os leitores de `postos`: **[não verificado]**. |
| `tipo_ciclo`, `template_id`, `confirm_txt` | `_vinculos` lê o vínculo do dia e o ciclo em `app/escala/services/cadastro_realidade.py:766`; a porta consulta o template em `app/escala/regua_defesa.py:310`, envia `confirm_txt` em `app/escala/regua_defesa.py:316`, e a execução o inclui em `obs` em `app/colaboradores/services/vinculo.py:350`. | `hoje` é uma data local passada do POST à porta em `app/escala/views.py:496` e `app/escala/views.py:506`. O censo completo dos escritores e leitores dessas colunas é **[não verificado]**. |

Quanto aos juízes registrados, `_vinculos` chama explicitamente os juízes de cobertura e vínculo do dia em `app/escala/services/cadastro_realidade.py:756` e `app/escala/services/cadastro_realidade.py:770`, correspondentes ao registro em `app/docs/JUIZES.md:146`. A duplicação cíclica descrita acima está comprovada entre as três condições citadas; que ela seja duplicação de um juiz **registrado em JUIZES.md**, ou que não haja outro juiz registrado contornado no diff inteiro, é **[não verificado]**.

### Matriz dos 15 achados dos postmortens

A classe e a identificação vêm dos anexos fornecidos; suas linhas originais são **[não verificado]**. “FILA” abaixo indica trabalho de verificação ou defeito de teste/documentação identificado, não uma aprovação da obra. [nao verificado]

| Achado | Classe | Sítio nesta árvore e resultado por classe | Triagem |
|---|---|---|---|
| F6F7 1 — significado entre leitores | (a) | Carga e janelas em `app/ponto/services/esmeril_espelho.py:99` e `app/escala/services/cadastro_realidade.py:981`: **[não verificado]** para todos os leitores e dias. | FILA |
| F6F7 2 — ausência de chamador | (a) | `medida` é passada pelo chamador de frota em `app/ponto/services/esmeril_espelho.py:235`; ausência de outros chamadores relevantes: **[não verificado]**. | FILA |
| F6F7 3 — sequência de eventos omitida | (a) | Sequência snapshot → mudança cadastral → POST em `app/escala/views.py:493`: lavra antiga é aceita; validade exigida: **[não verificado]**. | FILA |
| F6F7 4 — mutação que escapa ao selo | (a) | R1 e borda em `app/escala/tests/test_escala_nao_descreve.py:86`: mutante de janela descrito acima sobrevive **por inspeção**; execução **[não verificado]**. | FILA |
| F6F7 5 — pergunta omitida | (e) | Aval literal e citação em `app/docs/LEIS.md:90`: completude das perguntas contra os avais, cujos textos integrais não foram fornecidos, **[não verificado]**. | FILA |
| F6F7 6 — erro de fixture omitido | (c) | Fixtures em `app/escala/tests/test_escala_nao_descreve.py:43`: execução e eventuais erros de preparação **[não verificado]**. | FILA |
| Escala 1 — mutante R1 | (a) | O caso de quatro órfãs agora é afirmado em `app/escala/tests/test_escala_nao_descreve.py:91`; o mutante anterior específico e a bateria executada são **[não verificado]**. A borda de janela continua sem prova independente. | FILA |
| Escala 2 — rótulo LEI-AKITA | (a) | Novos predicados em `app/escala/services/cadastro_realidade.py:809` e `app/escala/services/cadastro_realidade.py:830`; rótulo da obra e sua correspondência integral **[não verificado]**. | FILA |
| Escala 3 — segundo juiz cíclico | (a) | **Sim**, há condições repetidas em `app/escala/services/cadastro_realidade.py:849`, `app/escala/regua_defesa.py:313` e `app/colaboradores/services/vinculo.py:128`; condições e propósito não são idênticos. | FILA |
| Escala 4 — R5 não chega à porta | (a) | **Sim**: guarda anterior em `app/escala/views.py:498`; R5 em `app/escala/tests/test_escala_nao_descreve.py:299`. Remover a recusa da porta sobrevive ao R5 por inspeção. | FILA |
| Escala 5 — R4 não confere `h.de` | (a) | **Não nesse teste atual**: R4 confere `h.de` em `app/escala/tests/test_escala_nao_descreve.py:288`. Cobertura de todos os formatos do antes **[não verificado]**. | não se aplica ao defeito específico |
| Escala 6 — lavra vencida | sem classe compatível | **Sim**, não há guarda de idade nos sítios `app/ponto/services/esmeril_espelho.py:371` e `app/escala/views.py:493`; prazo obrigatório **[não verificado]**. | FILA |
| Escala 7 — literal do aval | (e) | A lavra omite `ciclo` em `app/ponto/services/esmeril_espelho.py:245`; o link aponta ao vínculo em `app/escala/tests/test_escala_nao_descreve.py:305`. Conformidade com o aval integral: **[não verificado]**. | FILA |
| Escala 8 — rótulo do selo | (a) | **Sim**, o teste verifica presença, não agrupamento, em `app/escala/tests/test_cadastro_x_realidade.py:53`; ainda é citado em `app/docs/LEIS.md:88`. | FILA |
| Escala 8 — citação L-042 | (e) | L-042 afirma que proposta é recalculada em `app/docs/LEIS.md:90`; o novo mapa é guardado em `app/ponto/services/esmeril_espelho.py:246`. A célula PROTEGE vazia não obriga citação pelo selo, conforme `app/docs/LEIS.md:43` e `bin/tests/test_lei_protege_sitio.sh:77`; aplicabilidade material da lei e citações nos commits **[não verificado]**. | FILA |

**Achados novos desta volta:** a borda da janela sem prova independente é **FILA**; a exaustividade dos leitores, dos juízes e dos mutantes permanece **FILA [não verificado]**. A duplicação cíclica, o R5 que não alcança a porta, a lavra vencida e o selo de agrupamento já constavam dos postmortens fornecidos, portanto não os conto como novos. Não há base suficiente nesta verificação parcial para declarar que nenhum achado **BLOQUEIA** exista.

## 18/11 19:35 · sessao 284d4a129c31 · - · - · origem=Ronald (tmux)

A auditoria da tarefa e dos selos encontrou:

- Os dois selos AST aceitam quatro mutantes que esvaziam ou alteram a lista `perguntas` por alias ou por métodos de `list`. Verifiquei os helpers em memória; a execução da suíte inteira é **[não verificada]**. A lista é criada em [pergunta_no_app.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/pergunta_no_app.py:84), e os selos estão em [test_contract_pergunta_viva.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_contract_pergunta_viva.py:26) e [test_pergunta_no_app.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_pergunta_no_app.py:29).

- A sequência `VALIDACAO → SILENCIO → RESPOSTA` que mantém validação produz `aguarda_validacao` na tarefa, pois o laço só recua por respostas contíguas; a leitura do ato conserva a validação. Qual resultado a lei exige para essa sequência é **[não verificado]**. [tarefa_do_dia.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:163), [models.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4313).

- A face por pergunta chama `_pela_decisao` diretamente, enquanto a tarefa do dia aplica guardas adicionais de vida, lâmpada, teto e ausência. A equivalência das duas saídas em todos os casos é **[não verificada]**. [tarefa_do_dia.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:276), [face por pergunta](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:436).

- O teste de endpoint do passivo cobre apenas `conservado=True`, mas outro teste no mesmo arquivo cobre também `False`; portanto, o mutante sempre `True` sobrevive ao primeiro teste, não à tabela unitária por inspeção. [test_f6_u4_aguarda_admin_pela_tarefa.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:67).

- A partição do app passa a projeção diretamente ao juiz de resposta; a property `respondida` passa a leitura da sequência. Sua concordância quando projeção e sequência divergem é **[não verificada]**. [pergunta_no_app.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/pergunta_no_app.py:80), [models.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:3785).

## 18/11 19:34 · sessao 568d698c9c3e · - · - · origem=Ronald (tmux)

A auditoria parcial de decisões e seus testes foi enviada ao agente principal com referências de arquivo e linha. A divergência potencial entre a face SQL e o C6 depende das demais guardas de vitalidade; sua incidência em produção permanece **[não verificado]**. [nao verificado]

_7127 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
