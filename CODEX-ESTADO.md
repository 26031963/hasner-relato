# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 11:46** · publicado em **16/11/2026 11:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 11:46 · sessao ae3a5fdf0c1e · ROTULO-APLICADA-SEM-BATIDA · censo · origem=Code

**O retorno incorreto está confirmado:** havendo resposta em pergunta de hora, `materializada_em` basta para devolver `aplicada`, salvo `paridade_recusada`; esse ramo não verifica batida nem `recusa_motivo`. (`app/chamados/services/materializacao.py:438–448`)

**Total na frota: [não verificado].** O anexo informa três perguntas — 47199, 43643 e 44051 —, mas não fornece censo completo nem permite reconstituir suas transações. [não verificado]

**(a) O que a função devolve**

As condições abaixo estão na ordem de execução; “qualquer” inclui batida criada, inexistente ou posteriormente desfeita. (`app/chamados/services/materializacao.py:438–465`)

| Condição | Retorno | Evidência |
|---|---|---|
| Tipo diferente de `hora`, ou resposta vazia após `strip()`; quaisquer carimbos, vias e batidas | `None` | `app/chamados/services/materializacao.py:438–439` |
| Hora com resposta; materializada; via `paridade_recusada`; com ou sem recusa/batida | `{'estado': 'recusada_paridade', 'detalhe': 'encerrada SEM criar batida: fato de chao prevalece; quem discordar corrige a batida vizinha'}` | `app/chamados/services/materializacao.py:440–447` |
| Hora com resposta; materializada; **qualquer outra via**, inclusive vazia; com ou sem recusa/batida | `{'estado': 'aplicada', 'detalhe': ''}` | `app/chamados/services/materializacao.py:448` |
| Hora com resposta; não materializada; derivação não produz timestamp | `invalida`; detalhe = primeiro erro, ou vazio | `app/chamados/services/materializacao.py:449–452` |
| Não materializada; timestamp derivado; batida encontrada na janela e aprovada | `ja_satisfeita`; detalhe explica que validar só encerra a pendência | `app/chamados/services/materializacao.py:453–457` |
| Não materializada; sem satisfação anterior; vizinha do mesmo tipo; plantio não preenche furo | `bloqueada_paridade`; detalhe informa o par quebrado | `app/chamados/services/materializacao.py:458–462` |
| Não materializada; demais casos | `aplicavel`; detalhe anuncia tipo e hora que validar registraria | `app/chamados/services/materializacao.py:463–465` |

`recusa_motivo` não participa dessa decisão. Sem `materializada_em`, a via também não participa; `respondida_em` e `validada_em` não são condições dessa função. (`app/chamados/services/materializacao.py:438–465`)

Além disso, a busca de satisfação usa `Batida.objects` sem excluir retratadas; a guarda de vizinhança também consulta batidas cruas. Portanto, essa previsão não equivale a uma consulta de “batida viva”. (`app/chamados/juizes.py:238–250`; `app/chamados/services/materializacao.py:95–104`)

**(b) Censo dos escritores**

O campo da pergunta nesta cópia é `PerguntaDisputa.materializada_em`. O próprio `help_text` ainda associa processamento a “batida criada + chamado fechado”, embora existam resoluções sem plantio. (`app/chamados/models.py:3502–3505`; `app/chamados/services/materializacao.py:1069–1086`)

A tabela distingue persistência física de chamadores operacionais: estes últimos chegam às portas abaixo. (`app/chamados/models.py:3156–3167`)

| Escritor físico | Forma e comportamento |
|---|---|
| `_escrever_carimbo` — `app/chamados/models.py:3180–3184` | Atribui instante, autor e via **em memória**; não cria Batida. |
| `carimbar` — `app/chamados/models.py:3235` | Persiste por `save(update_fields=campos)`; aplica o carimbo, consulta C6 e recusa se a pergunta continuar viva. Isso não é uma exigência física universal de Batida. (`:3211–3228`) |
| `carimbar_identidade` — `app/chamados/models.py:3273` | Persiste por `save(update_fields)`; aceita as vias de identidade, sem consultar C6. (`:3251–3273`) |
| `descarimbar` — `app/chamados/models.py:3291–3295` | Zera os três campos; pode devolver os campos ao chamador sem salvar. |
| `descarimbar_em_lote` — `app/chamados/models.py:3363–3365` | `QuerySet.update(materializada_em=None, materializada_por=None, via_resolucao='', ...)`; exclui linhas fora do fato. |

`_desfazer_em_memoria` restaura o estado anterior após recusa/exceção, sem persistência própria. (`app/chamados/models.py:3242–3249`)

| Via/função operacional que carimba | Cria Batida? Pode ficar sem correspondente viva? |
|---|---|
| `_materializar_ausencia_de_resposta`: `declaracao` ou `ausencia_dia` — `app/chamados/services/materializacao.py:555–567` | Cria **Ausencia**, carimba perguntas irmãs; não cria Batida para elas. Pode resolver sem Batida. (`:527–535`) |
| `_materializar_resposta_hora`: `paridade_recusada` — `app/chamados/services/materializacao.py:906–931` | Não planta; exige marco aceso e retorna após o carimbo de identidade. O estado de aplicação distingue essa via. (`:444–447`) |
| `_carimbar_recusa`: `fato_ocupado` — `app/chamados/services/materializacao.py:1095–1109` | Não cria Batida. Só `COLISAO`, com carimbo ainda nulo, gera esse carimbo. Outras recusas gravam somente o motivo; **uma recusa posterior preserva o carimbo anterior**. |
| `_materializar_uma`: `declaracao` ou `via_pelo_fato`, com fallback `chamado_encerrado` — `app/chamados/services/materializacao.py:1267–1287` | Hora cria ou reutiliza; `data_hora` registra Batida; escolha resolve expressamente sem Batida. (`:865–877`, `:932–940`, `:1012–1025`, `:1069–1086`) |
| `materializar_pendentes_aprovadas`: `juiz_aprovou` — `app/chamados/services/materializacao.py:1424–1438` | Não cria; consulta lastro e carimba. O juiz admite resoluções sem testemunha física da pergunta. (`app/chamados/juizes.py:682–710`) |
| `lavrar_sem_turno_aberto`: `sem_turno_aberto` — `app/chamados/reconciliador.py:363–406` | Não cria; pode encerrar sem Batida correspondente. |
| `retratar_perguntas_absolvidas_por_celula`: `celula_absolveu` — `app/chamados/reconciliador.py:550–596` | Não cria; a absolvição da célula fundamenta o carimbo. Pode terminar sem Batida. |
| `_resolver_perguntas_por_chamado_resolvido`, ramo destino — `app/chamados/services/disputa_emissao.py:1987–2009` | Não cria; usa `via_pelo_fato` e `resposta='validar'`. A função declara encerramento sem plantio. (`:1907–1913`) |
| Mesma função, ramo sem credor — `app/chamados/services/disputa_emissao.py:2065–2082` | Não cria; usa `via_pelo_fato` ou `chamado_encerrado`, com `resposta='responder'`. |
| `fechar_por_admin`: `admin_decidiu` — `app/chamados/models.py:3133–3147` | Não cria. Chamado pelo encerramento administrativo/desligamento da disputa (`app/chamados/models.py:1982–1985`), fechamento do fio do dia (`app/ponto/services/veredito_celula.py:190`) e aceite de folga (`app/chamados/services/validacao.py:314`, `:354`). Pode resolver por folga, sem Batida. |
| `DisputaSupervisao.remover_motivo`: `motivo_retratado` — `app/chamados/models.py:2118–2126` | Não cria; pode manter instante e autor do carimbo anterior. |
| `fio.retratar_duplicata`: `duplicata` — `app/chamados/services/fio.py:1384–1401` | Não cria; registra referência à canônica e pode preservar carimbo anterior. |
| `encerrar_fantasma`: `fantasma` — `app/chamados/models.py:3378–3386` | Não cria; usa `carimbar`/C6. Chamador em `app/chamados/reconciliador.py:1148`. |

`cadastro_folga` não possui uma atribuição literal separada ao carimbo: nasce de `via_pelo_fato`, quando há agrupamento sob limbo/folga e nenhuma batida apurável no dia; o mesmo derivador pode retornar `adesao`, `ausencia_dia`, `completude_turno` ou `chamado_encerrado`. Entra pelos chamadores desse derivador listados acima. (`app/chamados/juizes.py:1772–1816`)

Sobre **plantio desfeito pelo juiz**, o caminho atual de `_materializar_uma` reprova dentro do `atomic`: desfaz o plantio e o carimbo que pertencerem àquela transação. Depois, o `finally` grava a recusa, cuja porta relê e preserva eventual carimbo já existente. Não se pode concluir que um carimbo criado naquele mesmo `atomic` sobreviva ao rollback. (`app/chamados/services/materializacao.py:1224–1249`, `:1281–1303`, `:1095–1109`)

Há também retração posterior: `retratar_batida` grava `aprovada=False` e `retratada_em`; essa escrita não altera diretamente o carimbo da pergunta. Assim, plantio ocorrido e Batida atualmente viva são fatos diferentes. A sequência concreta das perguntas 43643/44051 permanece restrita ao relato do anexo. (`app/ponto/registro_batida.py:269–282`) [não verificado]

Os caminhos de **limpeza** também integram o censo:

| Chamador | Escrita alcançada |
|---|---|
| `ressuscitar_perguntas` | Lote em `app/chamados/models.py:1613`. |
| `reabrir_para_correcao` | Lote em `app/chamados/services/disputa_emissao.py:1648`. |
| `desfazer_carimbo_sem_lastro` | `descarimbar` em `app/chamados/services/relavra_via.py:242–246`. |
| `reabrir_fantasma` | `descarimbar` em `app/chamados/models.py:3395–3397`; comando chama em `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`. |
| `reabrir_por_resposta` | Limpa em memória e devolve campos em `app/chamados/models.py:3412–3414`; escritor da resposta os incorpora em `app/chamados/services/disputa_emissao.py:1499`. |
| `reabrir_por_competencia` | `descarimbar` em `app/chamados/models.py:3419–3421`; chamador em `app/chamados/services/competencia_trancada.py:279`. |

A limpeza de `_universo_da_ressuscitacao` ocorre apenas numa cópia em memória para julgamento. (`app/chamados/models.py:1541–1556`)

As demais formas pedidas ficaram assim:

- **`bulk_update`:** nenhum escritor operacional adicional do carimbo foi localizado. O detector cobre essa forma, kwargs indiretos, construtores, `update` e `setattr`; a exaustividade para reflexão arbitrária permanece [não verificado], pois o detector declara limites para `__dict__` e nomes externos. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:99–115`, `:174–197`)
- **Migrations:** `0016` adiciona o campo, sem materialização de dados; `0051` atualiza colaborador/instância, e `0053` consulta colisões. Não foi localizada migration de dados escrevendo esse carimbo. (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21–24`; `app/chamados/migrations/0051_decisao_pergunta.py:25–27`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36–46`) [não verificado quanto a escritor dinâmico não identificável]
- **Passivo:** chama a mesma `_materializar_uma`; `CAMPOS_REVERSAO` é fotografia para saída, não restauração do carimbo nesse serviço. (`app/chamados/services/passivo_validado.py:39`, `:64–83`, `:370`, `:522`)
- **Vias sem escritor temporal novo:** `competencia_trancada` mantém apenas acervo/leitura; `fato_materializado` só troca a via. (`app/chamados/models.py:3149–3154`, `:3367–3375`)
- **Homônimo:** `RegularizacaoExterna.materializada_em` pertence a outro modelo; seu escritor é `app/chamados/services/regularizacao_ext.py:199–201`. Pode apenas remover/corrigir Batida, sem criar outra. (`app/chamados/models.py:4128`; `app/chamados/services/regularizacao_ext.py:132–164`)
- **Testes:** fixtures e mutantes não foram contados como vias operacionais; exemplificam gravação artificial de estados, como a pergunta materializada sem plantio do teste da função. (`app/chamados/tests/test_estado_aplicacao.py:37–39`, `:66–72`)

**(c) Leitores e apresentação**

| Caminho | O que lê/mostra |
|---|---|
| Reexportação | `chamados.models` exporta a função real. (`app/chamados/models.py:4231`) |
| `montar_chamados_fio` | Único chamador operacional localizado: calcula `p.estado_aplicacao`; sem disputa ou em exceção, atribui `None`. (`app/chamados/services/fio.py:373–376`) |
| Modal administrativo | Monta o fio e renderiza o modal; o fragmento usa o mesmo montador. (`app/chamados/views.py:889`, `:927–931`; `app/chamados/services/fio.py:484`, `:534–535`) |
| Template consumidor | Exibe detalhe vermelho de `bloqueada_paridade` e azul de `ja_satisfeita`. Não imprime os outros estados nesse bloco. (`app/templates/chamados/partials/modal_fio.html:230–248`) |
| Precedência visual | `pendencia_admin` mostra pendência e motivo; senão, `validada_em` mostra **“✓ Validada”**. Logo, a legenda literal “aplicada” do relato não está confirmada por esse template. (`app/templates/chamados/partials/modal_fio.html:222–229`) |
| Meu Atendimento / fio em página inteira | Executam o montador, mas não foi localizado consumo visual desse atributo nos respectivos templates. (`app/chamados/views.py:569`, `:632–635`, `:677`, `:697–703`) [não verificado fora da cópia] |
| API de perguntas | Publica `p.estado` e `aguarda_admin`, não esse estado de aplicação. (`app/api/views.py:2550–2578`) |
| Contadores do modal | Derivam de fila de chamados, followups e disputas abertas; não desse retorno. (`app/chamados/views.py:900–912`) |
| Testes | Exercitam satisfação, paridade, aplicabilidade, aplicação e fora de escopo; há teste próprio da via recusada. (`app/chamados/tests/test_estado_aplicacao.py:44–72`; `app/chamados/tests/test_badge_via_paridade.py:30–32`) |

Não foi localizado contador operacional de `estado='aplicada'` nem outro consumidor desse retorno nas APIs da cópia. [não verificado quanto a clientes externos] [nao verificado]

**(d) Sonda proposta, somente como texto**

Universo: todas as perguntas `hora` de `juliani`, sem restringir empresa, competência, situação do colaborador ou disputa aberta. A função real decide quais são `aplicada`; tipos diferentes ficam fora. Resposta vazia e paridade recusada ficam fora do numerador pela própria função. (`app/chamados/services/materializacao.py:438–448`)

Batida viva usa `batidas_apuraveis`, incluindo pendentes do DP e excluindo retratadas. Ausência de FK sozinha não basta: o acervo anterior pode não ter `pergunta_origem`. A correspondência usa os auxiliares reais de slot, vínculo e janela. (`app/ponto/turnos.py:2868–2895`; `app/ponto/models.py:30–44`; `app/chamados/juizes.py:238–282`, `:344–377`)

Slot não derivável ou falha engolida fica **indeterminado**, separado do total sem correspondente demonstrável. Não uso `_pergunta_materializada_de_fato` como prova física, pois ele pode absolver sem Batida. (`app/chamados/juizes.py:682–708`; `app/core/observ.py:57–82`)

O texto abaixo não foi executado nem validado contra produção. Os comandos de dados são SELECT; `schema_context` pode emitir `SET search_path`, além do controle transacional. [não verificado]

```bash
PGOPTIONS='-c default_transaction_read_only=on -c default_transaction_isolation=repeatable\ read' \
PYTHONDONTWRITEBYTECODE=1 python manage.py shell <<'PY'
from collections import Counter, defaultdict
from django.db import connection, transaction
from django_tenants.utils import schema_context

from chamados.models import PerguntaDisputa
from chamados.services.materializacao import estado_aplicacao_pergunta
from chamados.juizes import (
    _slot_esperado,
    _plantio_na_hora_do_slot,
    _batida_existente_na_janela,
)
from ponto.turnos import batidas_apuraveis
from core.observ import engolidos

por_via = defaultdict(Counter)
estados = Counter()

with schema_context("juliani"), transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("""
            SELECT current_schema(),
                   current_setting('transaction_read_only'),
                   current_setting('transaction_isolation')
        """)
        schema, readonly, isolamento = cur.fetchone()

    assert schema == "juliani", schema
    assert readonly == "on", readonly
    assert isolamento == "repeatable read", isolamento

    perguntas = (
        PerguntaDisputa.objects.filter(tipo_resposta="hora")
        .select_related("disputa__colaborador", "chamado_destino")
        .order_by("pk")
    )

    for p in perguntas.iterator(chunk_size=1000):
        via = p.via_resolucao or "<vazia>"
        c = por_via[via]
        c["universo_hora"] += 1

        # A classificação vem integralmente da função real.
        r = estado_aplicacao_pergunta(p.disputa, p)
        estado = r["estado"] if r is not None else "<fora_escopo>"
        estados[(via, estado)] += 1
        if estado != "aplicada": [nao verificado]
            continue

        c["aplicada"] += 1 [nao verificado]
        vivas = batidas_apuraveis(p.disputa.colaborador)
        vinculadas = vivas.filter(pergunta_origem_id=p.pk)
        if not vinculadas.exists():
            c["sem_vinculo_vivo"] += 1

        antes = engolidos()["total"]
        ts, tipo = _slot_esperado(p)
        if engolidos()["total"] != antes or ts is None:
            c["indeterminada"] += 1
            continue

        corresponde = (
            _plantio_na_hora_do_slot(p, ts, vinculadas)
            or _batida_existente_na_janela(
                p.disputa.colaborador, ts, tipo, batidas=vivas
            ) is not None
        )
        chave = (
            "com_correspondente_viva" if corresponde
            else "sem_correspondente_viva"
        )
        c[chave] += 1

    assert not connection.needs_rollback
    for c in por_via.values():
        assert c["aplicada"] == ( [nao verificado]
            c["com_correspondente_viva"]
            + c["sem_correspondente_viva"]
            + c["indeterminada"]
        )

campos = (
    "universo_hora", "aplicada", "sem_vinculo_vivo", [nao verificado]
    "com_correspondente_viva", "sem_correspondente_viva",
    "indeterminada",
)
print("via", *campos, sep="\t")
total = Counter()
for via, c in sorted(por_via.items()):
    print(via, *(c[k] for k in campos), sep="\t")
    total.update(c)
print("<TOTAL>", *(total[k] for k in campos), sep="\t")

print("via\testado_real\tquantidade")
for (via, estado), n in sorted(estados.items()):
    print(via, estado, n, sep="\t")
PY
```

`sem_correspondente_viva` será a contagem conforme a correspondência atual do código; `sem_vinculo_vivo` é apenas diagnóstico adicional. Os indeterminados impedem declarar um total físico absoluto sem investigação complementar. (`app/chamados/juizes.py:238–282`, `:344–377`) **Resultado numérico: [não verificado].**

**(e) Encaixe, em uma linha:** **F4 da E4c**, pois o defeito está no leitor do materializador e F4 cobre materialização/passivo; RAIZ-DA-DISPUTA trata agregado, destinos, fechamento e pares. (`app/chamados/services/materializacao.py:431–448`; `app/docs/RELATO.md:254`, `:292`; `app/docs/BACKLOG.md:2031`)

O conteúdo literal de `logs/ESPINHA-BRIEFS/F4` está **[não verificado]**; o brief e a matriz aparecem referidos no registro documental da cópia. (`app/docs/RELATO.md:133`, `:254`)

## 16/11 11:45 · sessao f38ef04e825c · APP-LE-PROXIMA-BATIDA · sonda · origem=Code

**1. A saída fecha aritmeticamente.** Janela fornecida: `2026-11-09T14:42:13.404576+00:00` a `2026-11-16T14:42:13.404576+00:00`. Os valores abaixo são do envelope; execução, consulta e correspondência com a base: **[nao verificado]**.

| origem | provider | total | sem_accuracy | sem_accuracy_dentro_False | sem_accuracy_dentro_None |
|---|---|---:|---:|---:|---:|
| apk | - | 11 | 11 | 0 | 2 |
| sem_provider(pwa?) | - | 20 | 0 | 0 | 0 |
| **TOTAL calculado** | | **31** | **11** | **0** | **2** |

Conferência sobre o texto: `11 + 20 = 31`, igual ao TOTAL; `11 + 0 = 11` sem accuracy. A saída informa `total_erros 0`; comprovação independente desse contador: **[nao verificado]**.

**2. Existem fontes melhores de contexto, mas nenhuma identifica diretamente a origem de cada PingGeo persistido.** O modelo guarda colaborador, entrada e sinais técnicos; não tem `origem`, `canal`, `device_id` ou vínculo com aparelho. O comentário atribui os campos técnicos ao APK Kotlin e os descreve vazios em PWA. [app/ponto/models.py:1005](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/models.py:1005)

As três portas de criação da aplicação gravam `accuracy` recebido, opcional; ausência pode ficar `NULL`, permitido pelo modelo. [app/ponto/models.py:1017](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/models.py:1017)

| Porta | Quem escreve PingGeo | Leitura → gravação de accuracy |
|---|---|---|
| `/api/ping-geo/`, core | [app/api/views_core.py:951](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:951) | [864](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:864) → [957](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:957) |
| `/api/ping-geo/`, API completa | [app/api/views.py:2421](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2421) | [2334](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2334) → [2427](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2427) |
| `/ponto/ping-geo/`, web | [app/ponto/views.py:3065](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3065) | [3014](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3014) → [3071](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3071) |

As fontes alternativas têm estes limites:

- **`batida_entrada.canal`** registra `apk`/`pwa`, mas identifica a porta da **batida de entrada**, não a origem dos pings seguintes. [app/ponto/models.py:46](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/models.py:46), [app/ponto/models.py:1010](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/models.py:1010)
- **`DeviceColaborador.plataforma`** distingue app, APK, iOS e PWA. A API recebe `device_id` para validar a assinatura, mas não o persiste no ping. [app/colaboradores/models.py:868](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/colaboradores/models.py:868), [app/colaboradores/models.py:915](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/colaboradores/models.py:915), [app/api/views_core.py:879](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:879), [app/api/views_core.py:951](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:951)
- **Cliente declarado na requisição/sessão:** existem `X-Hasner-Client`, `plataforma_sessao` e `cliente_app`; esses valores não são gravados pelos escritores acima. O app também abre sessão Django em WebView, portanto a porta web não comprova PWA. [app/core/middleware.py:117](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/core/middleware.py:117), [app/core/views.py:1280](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/core/views.py:1280)
- **Assinatura é indício insuficiente:** ausência retorna sucesso com rótulo `pwa_sem_assinatura`; assinatura inválida gera warning e a criação prossegue. [app/api/views_core.py:64](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:64), [app/api/views_core.py:885](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:885)

O PWA desta cópia envia `accuracy: pos.coords.accuracy`, sem provider/assinatura. A origem real dos 31 pings continua **[nao verificado]**; mantenho os rótulos inferidos da sonda. [app/static/pwa.js:404](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/static/pwa.js:404)

Fora das portas de captura, há criação em teste sem accuracy, máscara que altera apenas coordenadas e comando que apaga pings. [app/ponto/tests/test_selo_presenca_lote.py:54](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/tests/test_selo_presenca_lote.py:54), [bin/sombra_regras_pessoais.py:79](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/bin/sombra_regras_pessoais.py:79), [bin/sombra_mascara_pessoal.py:35](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/bin/sombra_mascara_pessoal.py:35), [app/ponto/management/commands/zerar_dados_operacionais.py:98](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/management/commands/zerar_dados_operacionais.py:98)

**3. Confirmada para o alerta de fora gerado pelo próprio ping:** fora do raio efetivo + `accuracy=None` resulta em `gps_incerto`; as portas persistem `dentro_geofence=None` e exigem `is False` para alertar. [app/ponto/services/geofence.py:114](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/services/geofence.py:114)

| Porta | Conversão, juiz e estado persistido | Decisão de alerta |
|---|---|---|
| Core | [app/api/views_core.py:931](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:931) | Atual False, não mock, dois últimos não mock em 2h ambos False → `push_dp`. [967](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:967) |
| API completa | [app/api/views.py:2401](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2401) | Mesma condição → `push_dp`. [2437](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2437) |
| Web | [app/ponto/views.py:3045](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3045) | Atual False, não mock, ao menos dois falsos no turno → chamado e `push_supervisao`. [3081](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3081) |

Precisões necessárias:

- No web, a consulta filtra os falsos antes de contar: um ping incerto não dispara, mas também não interrompe essa contagem histórica. [app/ponto/views.py:3082](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3082)
- O HTTP pode responder `dentro_geofence=false` enquanto o registro fica `None`; resposta e estado persistido são distintos nas três portas. [app/api/views_core.py:943](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:943), [app/api/views.py:2413](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2413), [app/ponto/views.py:3057](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3057)
- Os dois `dentro_None` fornecidos não comprovam dois pings fora: intervalo também força `None`. [app/api/views_core.py:948](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:948)
- “Não alerta” não deve ser generalizado para **batidas**: esse outro fluxo cria `AlertaGeofence(tipo='gps_incerto')`. [app/ponto/services/geofence.py:301](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/services/geofence.py:301)

## 16/11 11:45 · sessao d83f05d050b7 · E4C-F1-NOVA-POUSO-B1 · verificador · origem=Code

**Veredito: o bloqueante documental está fechado na cópia; a certificação integral do delta fica [não verificado].** A frase agora está na fonte e sobrevive à regeneração (`app/docs/CORTES.json:1871`, `bin/gerar_cortes.py:107`). A cópia disponível não contém histórico Git: os dois diffs e `git log -2 --format=%B 99eccedf9` não puderam ser conferidos **[não verificado]**.

**1) FECHADO — frase na fonte e preservada pelo gerador.**

Quem **escreve** o Markdown é `bin/gerar_cortes.py`: lê `app/docs/CORTES.json`, renderiza todas as entradas e sobrescreve `app/docs/CORTES.md`; também atualiza o RELATO (`bin/gerar_cortes.py:23`, `bin/gerar_cortes.py:31`, `bin/gerar_cortes.py:106`, `bin/gerar_cortes.py:116`, `bin/gerar_cortes.py:136`).

Quem **lê a frase exigida** é o selo: extrai autoridades de `core/juizes.py`, subtrai a base e procura literalmente, sem distinguir maiúsculas, `corte Ronald: juiz <nome> nasce` no Markdown (`bin/tests/test_juiz_novo_tem_corte.sh:28`, `bin/tests/test_juiz_novo_tem_corte.sh:38`, `bin/tests/test_juiz_novo_tem_corte.sh:41`).

A conferência em memória encontrou **83 autoridades extraídas pelo selo, 54 na base e 29 exigindo frase; nenhuma ficaria sem frase após regenerar**. Abaixo estão todas as 29 e a localização da frase na fonte, seguindo a extração e comparação do selo (`bin/tests/test_juiz_novo_tem_corte.sh:28`, `bin/tests/test_juiz_novo_tem_corte.sh:38`).

| Autoridade exigida | Frase no JSON |
|---|---|
| `chamados/juizes.py::dia_da_pergunta_trancado` | `app/docs/CORTES.json:1729` |
| `chamados/juizes.py::dia_sem_trabalho` | `app/docs/CORTES.json:1736` |
| `chamados/juizes.py::fio_vivo` | `app/docs/CORTES.json:614` — campo `estado` |
| `chamados/juizes.py::pergunta_respondida` | `app/docs/CORTES.json:1855` e `app/docs/CORTES.json:1871` |
| `chamados/juizes.py::porta_alcancavel_do_dia` | `app/docs/CORTES.json:1771` |
| `chamados/models.py::DisputaSupervisao.pode_fechar` | `app/docs/CORTES.json:765` |
| `chamados/models.py::DisputaSupervisao.respostas_sem_veredito` | `app/docs/CORTES.json:1414` |
| `chamados/services/fabrica_da_celula.py::pauta_cadastro_existente` | `app/docs/CORTES.json:1764` |
| `chamados/services/tarefa_do_dia.py::tarefa_do_dia` | `app/docs/CORTES.json:1785` |
| `colaboradores/services/calendario.py::resolver_dia_oferecido` | `app/docs/CORTES.json:758` |
| `colaboradores/services/desligamento.py::esta_desligado` | `app/docs/CORTES.json:1743` |
| `escala/alimentacao.py::vinculo_cobre_a_data` | `app/docs/CORTES.json:875` |
| `escala/alimentacao.py::vinculo_do_dia` | `app/docs/CORTES.json:854` |
| `escala/servico_jornada.py::eh_turno_partido` | `app/docs/CORTES.json:477` |
| `folha/export.py::competencia_entregue` | `app/docs/CORTES.json:786` |
| `folha/export.py::competencia_fechada_para_celula` | `app/docs/CORTES.json:791` |
| `ponto/juiz_batida.py::periodos_do_dia` | `app/docs/CORTES.json:469` |
| `ponto/precedencia.py::folga_lancada_do_dia` | `app/docs/CORTES.json:1750` |
| `ponto/registro_batida.py::selfie_ausente` | `app/docs/CORTES.json:846` — campo `estado` |
| `ponto/services/furos_vetados.py::motivo_do_veto` | `app/docs/CORTES.json:1757` |
| `ponto/services/geofence.py::classificar_posicao` | `app/docs/CORTES.json:841` |
| `ponto/services/geofence.py::tem_gps` | `app/docs/CORTES.json:1792` |
| `ponto/status_dia.py::status_do_dia_lavrado` | `app/docs/CORTES.json:1778` |
| `ponto/supra_juiz.py::pode_executar_csf` | `app/docs/CORTES.json:1407` |
| `ponto/turnos.py::dia_encerrado` | `app/docs/CORTES.json:533` |
| `ponto/turnos.py::lembrete_fim_turno` | `app/docs/CORTES.json:868` |
| `ponto/turnos.py::par_completo` | `app/docs/CORTES.json:541` |
| `ponto/turnos.py::periodo_da_parcial_no_dia` | `app/docs/CORTES.json:1125` |
| `ponto/turnos.py::turno_aberto_ha` | `app/docs/CORTES.json:861` |

As duas frases em `estado` também sobrevivem, porque o gerador reproduz esse campo tanto na tabela dos pendentes quanto na tabela completa (`bin/gerar_cortes.py:90`, `bin/gerar_cortes.py:107`).

O Markdown inteiro coincidiu, em memória, com `markdown(JSON, agora=2026-11-16 11:38)`; portanto o conteúdo atual é integralmente reproduzível pelo gerador (`bin/gerar_cortes.py:97`, `app/docs/CORTES.md:3`). O selo shell não foi executado: ele cria e remove um temporário, contrariando a regra desta revisão; sua execução literal fica **[não verificado]** (`bin/tests/test_juiz_novo_tem_corte.sh:57`, `bin/tests/test_juiz_novo_tem_corte.sh:60`).

**2) [NÃO VERIFICADO] — preservação histórica dos `.py` pelo merge.**

Sem os objetos Git, não foi possível comparar, por arquivo de `py_b1.txt`, `ab616d1c7..51678cbb2` contra `f76eee7c6..99eccedf9`. Logo, não certifico que nenhum conteúdo da obra mudou **[não verificado]**. O RELATO registra a certificação antiga e um ensaio do comboio, sem substituir esses diffs (`app/docs/RELATO.md:18`, `app/docs/RELATO.md:19`).

No arquivo compartilhado, o retrato **atual** contém os registros dos sítios da obra e do main: `_dia_da_batida`, `_dia_do_contexto_fraco` e `regularizar_retorno`, com suas linhas correspondentes (`app/core/tests/test_medida_turno_do_par.py:258`, `app/core/tests/test_medida_turno_do_par.py:259`, `app/core/tests/test_medida_turno_do_par.py:339`).

Executei os **35 testes dessa catraca em memória: zero falhas e zero erros**, pelo procedimento documentado, sem banco ou bytecode. Isso verifica a coerência atual de identidades, composição e linhas; não prova equivalência histórica dos dois diffs (`app/core/tests/test_medida_turno_do_par.py:20`, `app/core/tests/test_medida_turno_do_par.py:541`, `app/core/tests/test_medida_turno_do_par.py:557`).

As duas intenções funcionais estão presentes no resultado: posto e histórico no mesmo `atomic`; ressurreição conserva a trilha, zera o par inteiro e evita fabricar REABERTURA (`app/colaboradores/views.py:333`, `app/colaboradores/views.py:335`, `app/chamados/models.py:1613`, `app/chamados/models.py:1622`, `app/chamados/models.py:1629`). **Que o merge preservou exatamente ambas continua [não verificado].**

**3) HÁ INTERAÇÃO — os caminhos examinados são compatíveis na leitura estática.**

Existe compartilhamento explícito da porta de travas: o serviço de colaboradores chama `PerguntaDisputa.trava_na_ordem` para travar colaboradores; a ressurreição usa a mesma autoridade com Colaborador → Disputa → Chamado → Pergunta (`app/colaboradores/services/colaborador.py:72`, `app/colaboradores/services/colaborador.py:80`, `app/chamados/models.py:1612`, `app/chamados/models.py:2770`, `app/chamados/models.py:2779`, `app/chamados/models.py:2793`, `app/chamados/models.py:2797`).

| Entidade/campos | Quem escreve | Quem lê |
|---|---|---|
| **Pergunta**: `respondida_em`, `resposta_colab`, validação e materialização | Ressurreição e correção passam por `descarimbar_em_lote`, que efetiva o update (`app/chamados/models.py:1629`, `app/chamados/services/disputa_emissao.py:1648`, `app/chamados/models.py:3391`). | `_C21` aponta para `pergunta_respondida`; a face SQL serve `q_vivas` e a fila de validação (`app/core/juizes.py:306`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1580`, `app/chamados/models.py:3481`, `app/chamados/juizes.py:1751`). |
| **DecisaoPergunta**: ato, instante e resposta anterior | `registrar` grava as linhas; ressurreição preserva RESPOSTA/VALIDAÇÃO, correção registra seu ato, e fechamento pode registrar SILENCIO (`app/chamados/models.py:4120`, `app/chamados/models.py:1613`, `app/chamados/services/disputa_emissao.py:1631`, `app/chamados/models.py:2005`). | `tarefa_do_dia` carrega a sequência ordenada e julga pelo ato (`app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/services/tarefa_do_dia.py:142`). |
| **CelulaDia**: veredito, ata e trabalho | A porta `lavrar_veredito` escreve o veredito; reabertura/ressurreição emitem o evento para acordar células (`app/escala/models.py:1826`, `app/ponto/portas/celula.py:1093`, `app/chamados/models.py:3358`). | `q_vivas` lê veredito/ata; `tarefa_do_dia` carrega as células; telas do main também as leem (`app/chamados/models.py:3488`, `app/chamados/models.py:3493`, `app/chamados/services/tarefa_do_dia.py:406`, `app/core/views.py:395`). |
| **HistoricoVinculo**: antes/depois, vigência, posto e autor | `registrar_mudanca`/`registrar_evento_de_vinculo`, chamados por posto e desligamento (`app/colaboradores/services/historico_vinculo.py:7`, `app/colaboradores/services/historico_vinculo.py:25`, `app/colaboradores/views.py:335`, `app/colaboradores/services/desligamento.py:138`). | A tela de detalhe lê o histórico (`app/colaboradores/views.py:306`). Ausência global de outros leitores **[não verificado]**. |

A interação mais relevante é **desligamento → fechamento → SILENCIO → tarefa**: desligamento preserva disputas com respostas sem veredito; nos demais casos chama `fechar`, que registra SILENCIO. A tarefa distingue esse ato de REABERTURA, que devolve ao colaborador. A B1 conserva essa distinção ao ressuscitar (`app/colaboradores/services/desligamento.py:164`, `app/chamados/models.py:2005`, `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/models.py:1622`).

`vincular_posto` grava posto e trilha no mesmo `atomic`; o save preserva a situação lida sob trava. A regeneração antiga por simples troca do posto cadastral está explicitamente aposentada nesse serviço (`app/colaboradores/views.py:333`, `app/colaboradores/services/colaborador.py:152`, `app/colaboradores/services/colaborador.py:158`, `app/colaboradores/services/colaborador.py:118`).

Também há caminho indireto por INSS: alteração de situação e criação da ausência dentro do mesmo `atomic`, seguida do signal que solicita julgamento das células (`app/ponto/views.py:2469`, `app/ponto/views.py:2472`, `app/ponto/views.py:2489`, `app/ponto/signals.py:143`). Execução concorrente e efeitos no hash em produção ficam **[não verificado]**.

**4) ESTRUTURA COERENTE; proveniência dos logs e ausência histórica de dado pessoal novo [não verificado].**

O patch fornecido acrescenta **19 entradas**, conferidas na fonte:

| Grupo | Entradas conferidas |
|---|---|
| 10 restaurações | `app/docs/CORTES.json:1725`, `1732`, `1739`, `1746`, `1753`, `1760`, `1767`, `1774`, `1781`, `1788` |
| 8 avais | `app/docs/CORTES.json:1802`, `1810`, `1818`, `1826`, `1834`, `1842`, `1850`, `1858` |
| Nascimento de `pergunta_respondida` | `app/docs/CORTES.json:1866` |

A validação em memória encontrou **255 IDs únicos**, nenhum campo `id/quando/estado/fatia/frase` vazio nas 19 adições e datas aceitas pela regra do gerador, inclusive minutos com `x` (`app/docs/CORTES.json:1725`, `app/docs/CORTES.json:1871`, `bin/gerar_cortes.py:35`).

Há duas ressalvas de campos:

- As **10 restaurações não têm chave `onde`**; guardam referências históricas em `estado`/`frase`. Portanto, se `onde` for obrigatório em toda entrada, essas dez não atendem. O gerador não exige esse campo (`app/docs/CORTES.json:1725`, `app/docs/CORTES.json:1788`, `bin/gerar_cortes.py:107`).
- As outras **9 possuem `onde` em `logs/AVAIS-1515/`**. Existência e conteúdo desses anexos ficam **[não verificado]**; `logs/` é ignorado pelo repositório (`app/docs/CORTES.json:1806`, `app/docs/CORTES.json:1814`, `app/docs/CORTES.json:1822`, `app/docs/CORTES.json:1830`, `app/docs/CORTES.json:1838`, `app/docs/CORTES.json:1846`, `app/docs/CORTES.json:1854`, `app/docs/CORTES.json:1862`, `app/docs/CORTES.json:1870`, `.gitignore:53`).

Além das adições, há **13 alterações de estado**, todas citando o censo `logs/CORTES-RECEBIDO-FORA-DA-PAUSA/c1.r`; a comprovação desse censo fica **[não verificado]** (`app/docs/CORTES.json:902`, `909`, `916`, `923`, `1021`, `1028`, `1056`, `1063`, `1070`, `1091`, `1098`, `1154`, `1161`).

Quanto a dados pessoais, a associação de primeiro nome a uma pergunta na restauração já aparece em outros documentos da cópia; os interlocutores nomeados nos avais também têm ocorrências anteriores no JSON (`app/docs/CORTES.json:1771`, `app/docs/BACKLOG.md:961`, `app/docs/RELATO.md:2856`, `app/docs/CORTES.json:1807`, `app/docs/CORTES.json:1188`, `app/docs/CORTES.json:1223`). Não identifiquei CPF, telefone ou email nas 19 adições examinadas; certificar ausência de dado pessoal adicional contra as árvores anteriores fica **[não verificado]**.

**5) PARCIALMENTE VERIFICADO — nenhuma dessas falhas demonstrada no conteúdo atual; histórico dos commits pendente.** [nao verificado]

| Classe de reprovação | Veredito |
|---|---|
| Frase só no derivado | **Ausente para as 29 autoridades cobradas:** todas têm lastro no JSON; a da obra está em duas entradas (`app/docs/CORTES.json:1855`, `app/docs/CORTES.json:1871`; tabela da resposta 1). |
| Segundo juiz duplicando registrado | **Não identificado no caminho examinado:** `_C21` registra `pergunta_respondida`; `q_respondida_com_texto` é sua face SQL, com teste de igualdade por PK. A execução desse teste com banco fica **[não verificado]** (`app/core/juizes.py:306`, `app/chamados/juizes.py:1575`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:73`). |
| Derivado escrito à mão | **Nenhuma divergência atual:** o Markdown inteiro é reproduzível pelo gerador. Que o commit efetivamente o produziu executando o gerador fica **[não verificado]** (`bin/gerar_cortes.py:97`, `app/docs/CORTES.md:3`). |
| Hash errado no corpo do commit | **[Não verificado]** — os corpos dos dois commits não estão disponíveis nesta cópia sem Git. | [nao verificado]

A prova das **283 no hash novo**, a suite inteira desse hash e a aprovação opus do delta permanecem **[não verificado]**. A prova registrada no RELATO pertence a `51678cbb2`, enquanto o aval exige repeti-la no hash que pousa (`app/docs/RELATO.md:19`, `app/docs/CORTES.json:1855`).

## 16/11 11:44 · sessao d3ef81fb503c · LOGIN-PUBLICO-HERDA-SESSAO · censo · origem=Code

(a) **Confirmo a premissa no código:** os dois `api_login` são `AllowAny`, aceitam POST e não declaram `authentication_classes`; o padrão contém `JWTComCredencial` seguido de `SessionAuthentication`. Fontes: [views_core.py:188](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:188), [views.py:202](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:202), [base.py:32](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings/base.py:32).

O módulo padrão de inicialização é `config.settings.saas`, que importa `base`. O seletor `HASNER_TENANT_URLCONF` muda o **URLconf**, preservando essa cadeia de settings; seu fallback é `config.urls`. Não encontrei sobrescrita de `REST_FRAMEWORK` nessa cadeia. Fontes: [wsgi.py:14](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/wsgi.py:14), [saas.py:10](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings/saas.py:10), [saas.py:76](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings/saas.py:76).

| Kernel previsto na cópia | URLconf | API |
|---|---|---|
| `saas_core` | `config.urls_core`, selecionado pelo ambiente | `api.urls_core` → `views_core`: [urls_core.py:6](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/urls_core.py:6), [api/urls_core.py:5](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls_core.py:5) |
| `saas_ui` | `config.urls`, fallback | `api.urls` → `views`: [urls.py:37](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/urls.py:37), [api/urls.py:2](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls.py:2) |

Esse desenho também está registrado em [test_rota_pede_sessao.py:50](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/core/tests/test_rota_pede_sessao.py:50). O Caddy encaminha `/api/auth/*` ao core; portanto, no desenho versionado, o login externo chega ao core, embora a UI também tenha implementação: [Caddyfile:8](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/Caddyfile:8).

Os valores **efetivos** de `DJANGO_SETTINGS_MODULE` e `HASNER_TENANT_URLCONF` dos containers são [nao verificado]. O entrypoint usa `setdefault`, e o compose de produção e os arquivos de ambiente são ignorados pelo versionamento: [wsgi.py:14](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/wsgi.py:14), [.gitignore:15](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/.gitignore:15), [.gitignore:40](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/.gitignore:40).

(b) **Censo local: três implementações públicas sem declaração de autenticação.** Nos três blocos, também não há `@authentication_classes` nem `@csrf_exempt` explícito:

| Kernel | Endpoint e método de escrita | Permissão e implementação | Rota |
|---|---|---|---|
| core | **POST** `/api/auth/login/` | `AllowAny`: [views_core.py:188](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:188) | [api/urls_core.py:9](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls_core.py:9) |
| UI | **POST** `/api/auth/login/` | `AllowAny`: [views.py:202](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:202) | [api/urls.py:60](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls.py:60) |
| UI | **POST** `/api/diag/cam/` | `permission_classes([])`: [views.py:2642](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2642) | [api/urls.py:102](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls.py:102) |

Há uma ressalva para fechar o universo de classes: os dois `HasnerTokenRefreshView` herdam de `TokenRefreshView` externo. A ausência de atributo **local** não prova que herdem o padrão do projeto. Estão em [views_core.py:125](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:125) e [views.py:69](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:69), roteados em [api/urls_core.py:12](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls_core.py:12) e [api/urls.py:63](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls.py:63). Os atributos efetivos da base externa são [nao verificado]; a cópia declara a dependência, mas não traz sua implementação: [requirements.txt:15](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/requirements.txt:15).

Exclusões relevantes:

- `api_app_version` é função Django que retorna `JsonResponse`, sem `@api_view`: [views.py:3042](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:3042).
- `api_mensageria_push` tem `@csrf_exempt` e `@require_POST`, mas é view Django, fora do censo DRF: [views_mensageria.py:476](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_mensageria.py:476).
- `comunicados.api_pendentes` é DRF, mas exige `IsAuthenticated` e aceita apenas GET: [comunicados/views.py:83](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/comunicados/views.py:83).

(c) **Para cada uma das três linhas, a previsão é bloqueio por `CSRF Failed`**, supondo sessão reconhecida de usuário ativo, ausência de `Authorization` e verificações CSRF ativas. Isso coincide com o relato fornecido para login; reprodução nesta cópia e ocorrência em `diag/cam` são [nao verificado].

O mecanismo esperado é: autenticação ocorre antes da permissão; sem JWT, `SessionAuthentication` encontra o usuário da sessão e chama `enforce_csrf`; a falta do token interrompe a requisição antes do handler. `AllowAny` e permissões vazias não desligam autenticação. A isenção de CSRF aplicada pelo wrapper DRF tampouco elimina essa checagem interna. **A implementação desse mecanismo não está na cópia; portanto, sua confirmação na versão instalada é [nao verificado].** A inclusão de SessionAuthentication está comprovada em [base.py:39](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings/base.py:39); a versão declarada do DRF está em [requirements.txt:14](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/requirements.txt:14).

(d) **Não localizei cobertura da combinação login + sessão válida + CSRF ativo; cobertura global dessa combinação: [nao verificado].** Os testes encontrados deixam lacunas concretas: [nao verificado]

| Teste existente | O que monta | [nao verificado]
|---|---|
| [test_api_login.py:58](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/tests/test_api_login.py:58) | `APIClient()` novo por POST, sem estabelecer sessão; verifica sucesso e tokens em `:167`. |
| [test_login_campos.py:27](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/tests/test_login_campos.py:27) | `Client()` novo, sem ativar checagem CSRF. |
| [test_contrato_trocar_senha.py:127](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/core/tests/test_contrato_trocar_senha.py:127) | Chamada direta ao login core com `APIRequestFactory`, sem montar sessão. |
| [test_api_mensageria_push.py:67](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/tests/test_api_mensageria_push.py:67) | Ativa CSRF, mas testa mensageria, sem cookie de sessão. |

A suite ainda usa `config.urls_test`, que concatena core antes da UI. Assim, testar apenas `/api/auth/login/` nessa configuração não cobre as duas implementações: [settings_test.py:9](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings_test.py:9), [urls_test.py:11](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/urls_test.py:11).

Esboço do RED, **não criado nem executado [nao verificado]**. Fixtures: dois colaboradores ativos distintos, A para a sessão e B para as credenciais:

```python
for urlconf in ("config.urls_core", "config.urls"):
    with self.subTest(urlconf=urlconf):
        with override_settings(ROOT_URLCONF=urlconf):
            client = APIClient(enforce_csrf_checks=True)
            client.force_login(usuario_a)

            self.assertIn(settings.SESSION_COOKIE_NAME, client.cookies)
            self.assertEqual(
                client.session["_auth_user_id"], str(usuario_a.pk)
            )
            client.cookies.pop(settings.CSRF_COOKIE_NAME, None)

            # Sem Authorization, X-CSRFToken ou csrfmiddlewaretoken.
            response = client.post(
                "/api/auth/login/",
                {"username": usuario_b.username, "password": senha_b},
                format="json",
                HTTP_HOST="juliani.hasner.com.br",
            )

            self.assertEqual(response.status_code, 200, response.content)
            self.assertEqual(
                response.data["colaborador"]["id"], colaborador_b.pk
            )
            self.assertTrue(
                {"access", "refresh"} <= response.data["tokens"].keys()
            )
```

A expectativa de responder pelas credenciais de B acompanha os dois handlers, que consultam `autenticar_colaborador` com os campos do corpo: [views_core.py:195](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:195), [views.py:209](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:209). O RED esperado antes da correção é falhar na exigência de 200 por recusa CSRF; resultado executado: [nao verificado].

## 16/11 11:44 · sessao 55cbce51d5ec · - · - · origem=Ronald (tmux)

Proposta de sonda, **não executada**. [não verificado]

A ausência de `pergunta_origem` não basta: o campo também fica vazio em batidas de chão e plantios anteriores a 06/09 (`app/ponto/models.py:30-44`). A autoridade de batidas vivas é `batidas_apuraveis`, que inclui pendentes do DP e exclui retratadas (`app/ponto/turnos.py:2868-2895`).

Para correspondência, usar os auxiliares reais: `_slot_esperado` delega a derivação ao materializador (`app/chamados/juizes.py:344-377`); `_plantio_na_hora_do_slot` verifica a hora do vínculo preservando o overnight (`app/chamados/juizes.py:253-282`); `_batida_existente_na_janela` aceita QuerySet alimentado (`app/chamados/juizes.py:238-250`). Não usar `_pergunta_materializada_de_fato`: vias sem testemunha podem retornar `True` sem batida (`app/chamados/juizes.py:682-708`).

Universo proposto: todas as perguntas `tipo_resposta='hora'` do schema `juliani`, sem corte de empresa, competência, situação do colaborador ou estado da disputa; cada pergunta passa pela função real. Perguntas de `data_hora`, texto e escolha ficam fora. Resposta vazia e paridade recusada não entram no numerador porque a função retorna respectivamente `None` e `recusada_paridade` (`app/chamados/services/materializacao.py:438-448`). Slot não derivável sem vínculo vivo fica **indeterminado**, evitando transformar falta de informação em inexistência física (`app/ponto/models.py:38-44`; `app/chamados/juizes.py:755-764`).

```bash
PGOPTIONS='-c default_transaction_read_only=on -c default_transaction_isolation=serializable' \
PYTHONDONTWRITEBYTECODE=1 python manage.py shell <<'PY'
from collections import Counter, defaultdict

from django.db import connection, transaction
from django_tenants.utils import schema_context

from chamados.models import PerguntaDisputa
from chamados.services.materializacao import estado_aplicacao_pergunta
from chamados.juizes import (
    _slot_esperado,
    _plantio_na_hora_do_slot,
    _batida_existente_na_janela,
)
from ponto.turnos import batidas_apuraveis
from core.observ import engolidos

contagens = defaultdict(Counter)
estados = Counter()

with schema_context("juliani"), transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute("""
            SELECT current_schema(),
                   current_setting('transaction_read_only'),
                   current_setting('transaction_isolation')
        """)
        schema, leitura, isolamento = cursor.fetchone()
    assert schema == "juliani", schema
    assert leitura == "on", leitura
    assert isolamento == "serializable", isolamento

    perguntas = (
        PerguntaDisputa.objects
        .filter(tipo_resposta="hora")
        .select_related("disputa__colaborador", "chamado_destino")
        .order_by("pk")
    )

    for p in perguntas.iterator(chunk_size=1000):
        via = p.via_resolucao or "<vazia>"
        c = contagens[via]
        c["universo_hora"] += 1

        resultado = estado_aplicacao_pergunta(p.disputa, p)
        estado = resultado["estado"] if resultado is not None else "<fora_escopo>"
        estados[(via, estado)] += 1
        if estado != "aplicada": [nao verificado]
            continue

        c["aplicada"] += 1 [nao verificado]
        vivas = batidas_apuraveis(p.disputa.colaborador)
        vinculadas = vivas.filter(pergunta_origem_id=p.pk)
        tem_vinculo = vinculadas.exists()
        if not tem_vinculo:
            c["sem_vinculo_vivo"] += 1

        antes = engolidos()["total"]
        ts, tipo = _slot_esperado(p)
        if engolidos()["total"] != antes:
            c["indeterminada"] += 1
            continue

        if ts is None:
            # O juiz preserva o vínculo vivo quando o slot não é derivável.
            if tem_vinculo:
                c["com_correspondente_viva"] += 1
            else:
                c["indeterminada"] += 1
            continue

        corresponde = (
            _plantio_na_hora_do_slot(p, ts, vinculadas)
            or _batida_existente_na_janela(
                p.disputa.colaborador, ts, tipo, batidas=vivas
            ) is not None
        )
        c[
            "com_correspondente_viva" if corresponde
            else "sem_correspondente_viva"
        ] += 1

    assert not connection.needs_rollback
    for c in contagens.values():
        assert c["aplicada"] == ( [nao verificado]
            c["com_correspondente_viva"]
            + c["sem_correspondente_viva"]
            + c["indeterminada"]
        )

print("schema", schema)
print("via\tuniverso_hora\taplicada\tsem_vinculo_vivo\t"
      "com_correspondente_viva\tsem_correspondente_viva\tindeterminada")
campos = (
    "universo_hora", "aplicada", "sem_vinculo_vivo", [nao verificado]
    "com_correspondente_viva", "sem_correspondente_viva", "indeterminada",
)
for via, c in sorted(contagens.items()):
    print(via, *(c[k] for k in campos), sep="\t")

print("via\testado_real\tquantidade")
for (via, estado), quantidade in sorted(estados.items()):
    print(via, estado, quantidade, sep="\t")
PY
```

`PGOPTIONS` solicita transações somente leitura e snapshot serializable; os `assert`s impedem apresentar resultado se a configuração não tiver sido aplicada. Validação de execução nesse ambiente: [não verificado]. `schema_context` é o mecanismo utilizado pelo próprio projeto, inclusive com default `juliani` (`app/ponto/management/commands/propor_reorganizacao.py:17-23`); ele pode emitir `SET search_path`, comando de controle de conexão, sem DML. O script proposto consulta dados apenas por SELECT; essa execução futura não foi realizada. [não verificado]

O contador de falhas engolidas existe em memória e `registrar_engolido` apenas incrementa o contador e registra logging, sem escrever no banco (`app/core/observ.py:57-82`). Essa verificação evita contar como ausência de batida uma correspondência derivada com erro silencioso. [não verificado]

Limite importante: `sem_correspondente_viva` é a contagem segundo os auxiliares de correspondência atuais; `indeterminada` precisa permanecer separada. O total físico absoluto da frota não é verificável somente pelo anexo. [não verificado]

## 16/11 11:44 · sessao 503e17526781 · - · - · origem=Ronald (tmux)

Censo da árvore ativa concluído só por leitura; não importei Django nem consultei banco.

**Escritores físicos atuais do carimbo de PerguntaDisputa**

| Sítio | Escrita |
|---|---|
| `app/chamados/models.py:3180-3184` `_escrever_carimbo` | Única atribuição em memória de `materializada_em`, autor e via. |
| `app/chamados/models.py:3235` `carimbar` | `save(update_fields=campos)`; lista começa em `:3211`. Antes de persistir aplica carimbo e consulta C6 em `:3222`; se viva restaura em memória e retorna `False` em `:3226-3228`. |
| `app/chamados/models.py:3273` `carimbar_identidade` | `save(update_fields=campos)`; campos em `:3269`; aceita somente vias de identidade (`:3261-3262`), sem consultar C6. |
| `app/chamados/models.py:3295` `descarimbar` | `save(update_fields)` zerando os três campos (`:3291-3295`). |
| `app/chamados/models.py:3363-3365` `descarimbar_em_lote` | Único `QuerySet.update(materializada_em=None, ...)`; exclui vias fora do fato. |
| `app/chamados/models.py:3245` `_desfazer_em_memoria` | Restaura apenas memória após carimbo recusado/exceção, sem persistência própria. |

Executei apenas Python AST (`python3 -B`), extraindo o detector existente em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:99-202`, sem imports do app. Varri todos `.py` fora `tests`, `test_*`, `logs`, `node_modules`, `__pycache__`, **incluindo migrations**. Resultado: somente `_escrever_carimbo`, `descarimbar_em_lote` e o homônimo de regularização abaixo. Nenhum `bulk_update` do carimbo na árvore ativa. O detector cobre atribuição, `setattr`, construtor/create/update, kwargs indiretos e bulk_update (`:174-197`), com limitações declaradas (`:111-115`).

**Todas as vias/chamadores operacionais que carimbam**

| Função/via | Evidência | Cria Batida? / pode carimbar sem Batida |
|---|---|---|
| `_materializar_ausencia_de_resposta`: `declaracao` ou `ausencia_dia` | `materializacao.py:555-567` | Cria **Ausencia** (`:527-535`), sem criar Batida para as irmãs. Pode terminar sem Batida. |
| `_materializar_resposta_hora`: `paridade_recusada` | `materializacao.py:906-931` | Não cria; exige marco aceso (`:907`), carimba identidade (`:930`) e retorna antes do registrar. |
| `_carimbar_recusa`: `fato_ocupado` | `materializacao.py:1095-1109` | Não cria; só COLISAO, e só sem carimbo anterior, carimba (`:1099-1106`). Demais recusas atualizam somente motivo (`:1107-1109`); **recusa posterior preserva carimbo anterior**. |
| `_materializar_uma`: `declaracao` ou `via_pelo_fato`, fallback `chamado_encerrado` | `materializacao.py:1267-1287` | Hora: cria ou reusa (`:865-877`, `:932-940`); data_hora: registrar (`:1012-1025`); escolha: explicitamente sem Batida (`:1069-1086`). Carimbo negado/lastro reprovado causa rollback (`:1238-1249`, `:1281-1287`). |
| `materializar_pendentes_aprovadas`: `juiz_aprovou` | `materializacao.py:1424-1438` | Não cria; consulta lastro e carimba. Juiz admite resolução sem testemunha para vias próprias (`juizes.py:682-710`), portanto não exige necessariamente Batida da pergunta. |
| `lavrar_sem_turno_aberto`: `sem_turno_aberto` | `reconciliador.py:363-406` | Não cria; somente sem resposta e sem turno aberto, com C6 morto; pode terminar sem Batida. |
| `retratar_perguntas_absolvidas_por_celula`: `celula_absolveu` | `reconciliador.py:550-596` | Não cria; absolvição da célula e C6; pode terminar sem Batida. |
| `_resolver_perguntas_por_chamado_resolvido`, ramo destino | `disputa_emissao.py:1987-2009` | Não cria; `via_pelo_fato`, resposta validar. Função declara SEM criar Batida (`:1907-1913`). |
| Mesma função, ramo sem credor | `disputa_emissao.py:2065-2082` | Não cria; `via_pelo_fato` ou `chamado_encerrado`, resposta responder. |
| `PerguntaDisputa.fechar_por_admin`: `admin_decidiu` | `models.py:3133-3147` | Não cria; porta de identidade. Chamadores: `DisputaSupervisao.fechar` desligamento/admin_encerra (`models.py:1982-1985`); `veredito_celula.fechar_o_fio_do_dia` (`ponto/services/veredito_celula.py:190`); aceite folga (`services/validacao.py:314`, `:354`). Esses atos podem resolver por folga/ausência, sem Batida. |
| `DisputaSupervisao.remover_motivo`: `motivo_retratado` | `models.py:2118-2126` | Não cria; preserva carimbo anterior quando existe (`manter_carimbo=True`). |
| `fio.retratar_duplicata`: `duplicata` | `services/fio.py:1384-1401` | Não cria; grava recusa apontando canônica; preserva carimbo anterior quando existe. |
| `PerguntaDisputa.encerrar_fantasma`: `fantasma` | `models.py:3378-3386` | Não cria; carimbar/C6. Chamador `reconciliador.py:1148`. |

`via_pelo_fato` devolve `cadastro_folga` para agrupamento sob pai limbo/folga sem batida apurável no dia (`juizes.py:1772-1798`), senão `adesao` (`:1799`); ausência aprovada → `ausencia_dia` (`:1805-1807`); TM com saída e fora intervalo → `completude_turno` (`:1808-1815`); restante → `chamado_encerrado` (`:1816`). Portanto `cadastro_folga` não tem escritor literal separado: entra pelos chamadores de `via_pelo_fato` listados acima.

`competencia_trancada` **não tem escritor novo**: porta antiga removida, via mantida para acervo e leitura, declarado em `models.py:3149-3154`. `fato_materializado` só troca via de pergunta já carimbada, sem gravar `materializada_em` (`models.py:3367-3375`); `relavra_via.relavrar` também só altera via (não é escritor do campo temporal).

**Todos os limpadores/chamadores**

- `ChamadoColaborador.ressuscitar_perguntas` → lote `models.py:1613`.
- `_universo_da_ressuscitacao` → `descarimbar(salvar=False)` apenas na cópia para julgar (`models.py:1541-1556`).
- `disputa_emissao.reabrir_para_correcao` → lote `services/disputa_emissao.py:1648`.
- `relavra_via.desfazer_carimbo_sem_lastro` → `descarimbar` depois C6 vivo (`services/relavra_via.py:242-246`).
- `PerguntaDisputa.reabrir_fantasma` → descarimbar (`models.py:3395-3397`); comando chama em `management/commands/reabrir_orfas_enterradas.py:157`.
- `reabrir_por_resposta` → descarimbar sem salvar (`models.py:3412-3414`); escritor da resposta agrega os campos em `services/disputa_emissao.py:1499`.
- `reabrir_por_competencia` → descarimbar (`models.py:3419-3421`); chamador `services/competencia_trancada.py:279`.

**Comandos, migrations e homônimos**

- `passivo_validado` é fila da mesma unidade: importa `_materializar_uma` em `services/passivo_validado.py:39` e chama em `:522`; não escritor adicional. `CAMPOS_REVERSAO` (`:64-66`) só lido por `_reversao_de` (`:81-83`) e guardado no resultado (`:370`); nenhuma restauração dinâmica ali.
- Migrations `0016_s84_pergunta_chamado_destino.py:23` e `0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26` apenas definem campos. Não achei migration de dado escrevendo `materializada_em`, inclusive fora chamados.
- Migration de dado com PD: `0051_decisao_pergunta.py:25-27` altera somente colaborador/instância; `0053_pergunta_unica_por_familia.py:36-46` só consulta e recusa colisões.
- Homônimo **RegularizacaoExterna**, outro modelo (`models.py:4128`): `services/regularizacao_ext.py:199-201` atribui e `save(update_fields)`. Pode apenas remover (`:132-141`) ou corrigir (`:151-164`) sem nova Batida; criação ocorre em `:172-189`. Não é pergunta nem campo equivalente de aplicação da pergunta.
- `tests/e4b_c6_bases` contém snapshots `.py.txt`, não runtime; nenhum `materializada_em =` nessa subárvore. Fixtures e mutantes dos testes não devem ser misturados com os escritores operacionais; o selo explica esse propósito em `tests/test_o261e_f2_porta_carimbo.py:17-23`, e exclui testes/migrations da própria varredura em `:207-211` (minha AST incluiu migrations).
- Retração posterior tem porta própria: `ponto/registro_batida.py:269-276` grava `aprovada=False` e `retratada_em`, sem alterar carimbo da Pergunta. Logo existência no momento do plantio não garante Batida viva depois; para atribuir “juiz desfez depois” a IDs concretos, somente o anexo fornecido, **[não verificado na cópia]**.

## 16/11 11:44 · sessao b51331eb1607 · - · - · origem=Ronald (tmux)

Censo das portas que **criam PingGeo** no código:

| Porta | Escritor | Accuracy |
|---|---|---|
| `/ponto/ping-geo/` | rota `app/ponto/urls.py:78`; `app/ponto/views.py:3065` | Lê opcional em `:3014`; grava em `:3071` |
| `/api/ping-geo/`, kernel core | `app/api/urls_core.py:5,17`; `app/api/views_core.py:951` | Lê opcional em `:864`; grava em `:957` |
| `/api/ping-geo/`, kernel UI | `app/api/urls.py:2,98`; `app/api/views.py:2421` | Lê opcional em `:2334`; grava em `:2427` |

Todas gravam o valor recebido; ausência vira `None`, permitido pelo modelo (`app/ponto/models.py:1017`). As três criações persistem os mesmos campos anti-manipulação, sem guardar origem, canal, device, header ou resultado da validação da assinatura (`app/ponto/views.py:3065-3078`; `app/api/views_core.py:951-964`; `app/api/views.py:2421-2434`).

**Há sinais melhores no instante da request, mas nenhum vínculo persistido por ping para resolver retrospectivamente a origem:**

- `DeviceColaborador.plataforma` distingue `app`, `apk`, `ios`, `pwa-android`, `pwa-ios`, com `device_id` (`app/colaboradores/models.py:868-878,915`). A API lê `device_id` para validar assinatura (`app/api/views_core.py:879-885`) e não o grava em `PingGeo` (`:951-964`). O modelo do ping tampouco tem FK para o device (`app/ponto/models.py:1005-1025`).
- `X-Hasner-Client` é um sinal declarado da request (`app/core/middleware.py:117-119`). O bridge grava plataforma/cliente no token e depois na sessão (`app/core/models.py:335-347`; `app/core/views.py:1281-1288`); os escritores do ping não leem nem persistem esses valores (`app/ponto/views.py:3012-3019,3065-3078`; `app/api/views_core.py:862-885,951-964`).
- `Batida.canal` é persistido como `apk`/`pwa`, mas descreve **a batida**, não o ping (`app/ponto/models.py:46-55`); o ping apenas se liga à entrada ativa (`app/ponto/models.py:1010-1013`; `app/ponto/views.py:3067`). Usá-lo como origem do ping seria inferência.
- A porta web também pode receber sessão aberta pelo app em WebView (`app/core/models.py:323-326`; `app/core/views.py:1280-1288`); portanto URL web não prova PWA.
- Sem assinatura, o validador simplesmente devolve sucesso com `pwa_sem_assinatura=True` (`app/api/views_core.py:64-67`); assinatura inválida apenas produz warning e a criação segue (`:885-890,951`). Logo presença/ausência da assinatura é heurística, não origem autenticada persistida.

O PWA da cópia envia `latitude`, `longitude` e `accuracy`, sem provider/assinatura (`app/static/pwa.js:404-411`). A atribuição Kotlin está no comentário do modelo (`app/ponto/models.py:1020-1024`); código Kotlin/Swift nesta cópia: **[nao verificado]**.

Outras mutações encontradas, sem constituir portas de captura: máscara da sombra zera latitude/longitude (`bin/sombra_regras_pessoais.py:79`; `bin/sombra_mascara_pessoal.py:35-36`); comando de zeramento apaga pings (`app/ponto/management/commands/zerar_dados_operacionais.py:98`); fixture de teste cria ping sem accuracy (`app/ponto/tests/test_selo_presenca_lote.py:54-58`).

## 16/11 11:44 · sessao f62eab06339f · REGISTROS-E-O10D-1611 · verificador · origem=Code

**Parecer: AVAL-DEVOLVIDO — REGISTROS-E-O10D-1611, somente item 4.** Qualquer PROCEDE impede executar esse item; os itens 1–3 seguem, conforme o anexo `registros_e_o10d_1611.txt:6`. As referências aos anexos abaixo contam as linhas textuais fornecidas.

1. **PROCEDE — execução literal sem referência reproduzível.** “Re-rodar o censo_v2” pressupõe identificar seu conteúdo e sua base. A cópia registra `censo_v2 rc 0` e resume suas conclusões, mas isso não fornece o instrumento nem o relatório integral (`app/docs/RELATO.md:1630`). O artefato original e sua reprodução no HEAD atual estão **[nao verificado]**. Portanto, não há suporte para declarar agora “perguntas de lei: 0”; o registro histórico apenas afirma que censo e lei estavam prontos (`app/docs/RELATO.md:1592`).

2. **PROCEDE — faltam destinos para resultados inconclusivos.** O item prevê `N>0 → topo/não construir` e `cruzou → espera`, mas não explicita o tratamento de censo indisponível, cruzamento não mensurável ou RED sem mordida (`anexo registros_e_o10d_1611.txt:5`). O molde determina registrar **qual condição não fechou** e seguir a fila (`anexo prepara_volta_bug_par.txt:4`). Há precedente de devolução justamente por faltar o destino de RED sem mordida (`app/docs/RELATO.md:16`). É necessário distinguir “não medido” de zero antes de liberar construção.

3. **PROCEDE — compatibilidade do construtor precisa ser resolvida; colisão física da O10-D permanece [nao verificado].** O item manda Codex construir uma obra cujo pouso pede `!` (`anexo registros_e_o10d_1611.txt:5`), enquanto L-126 proíbe ao construtor “item que pede `!`”, além de `ponto/portas/celula.py` (`app/docs/LEIS.md:147`). O brief precisa delimitar a etapa permitida e compatibilizar essa restrição; o wrapper também anuncia a proibição desse arquivo (`bin/codex_pergunta.sh:346`).

   Quanto às obras em voo:

   - **F1 B1/B2, BUG-PAR e F2/F4:** BUG-PAR × B1 tem arquivos comuns documentados; B2 e F2/F4 aguardam B1 (`app/docs/RELATO.md:36`, `app/docs/RELATO.md:51`, `app/docs/RELATO.md:6`). Isso **não comprova** cruzamento com O10-D, cuja lista atual de alterações está **[nao verificado]**.
   - **LOTE7:** inclui `ponto/services/ausencia.py` e está em PAREI (`app/docs/RELATO.md:50`, `app/docs/RELATO.md:5`). Ele deve entrar expressamente na medição; “atrás do LOTE7” não documenta interseção de arquivos (`anexo registros_e_o10d_1611.txt:5`).
   - **Alcance funcional:** a troca chama `editar_dia`, marca cobranças órfãs e agenda rejunção de chamados; isso demonstra dependência, não alteração física compartilhada (`app/ponto/portas/celula.py:713`, `app/ponto/portas/celula.py:663`, `app/ponto/portas/celula.py:534`).

4. **PROCEDE — referência errada e comprovação parcial.**

   - `BACKLOG:2159` é **SOMBRA-HOJE-NA-BORDA**; O10-D está em **`app/docs/BACKLOG.md:2172`**.
   - Quatro células, duas chamadas no mesmo `atomic`, rollback, recusa falante e um RED por regra 1–6 constam nessa linha; os RED ainda eram listados como faltantes (`app/docs/BACKLOG.md:2172`, `app/docs/RELATO.md:1592`).
   - O molde PREPARA existe como registro, inclusive com construção bloqueada por cruzamento (`app/docs/RELATO.md:70`).
   - **O10-C INTERROMPIDA confere** (`app/docs/BACKLOG.md:2219`).

**Localização do censo e da resposta de 09/11:** o censo_v2 aparece **resumido** em `app/docs/RELATO.md:1630`; seu arquivo integral está **[nao verificado]**. A resposta de **09/11, 12:0x** aparece resumida em `app/docs/PROMPTS.md:2015`, com ponteiro para `logs/O10-D-PERMUTA-NA-TELA/aval_lei_permuta.txt`; esse original na cópia está **[nao verificado]**. A identidade Git do HEAD também está **[nao verificado]**.

## 16/11 11:43 · sessao 297e08cd88ae · - · - · origem=Ronald (tmux)

Leitores e decisão:

| Porta | Leitura/conversão de `accuracy` | Classificação/persistência | Alerta de fora |
|---|---|---|---|
| Web | `app/ponto/views.py:3014`, `3045-3048` | chama juiz e mapeia `gps_incerto` para `dentro_geofence=None`: `3052-3057`; grava accuracy original: `3065-3072` | exige atual `is False` e `not from_mock`: `3081`; consulta somente falsos, conta ≥2: `3082-3088`; chamado `ping_geo_fora` e push: `3091-3107` |
| API completa | `app/api/views.py:2334`, `2401-2404` | juiz/mapeamento: `2408-2413`; grava: `2421-2428` | atual False e não mock; dois últimos não mock em 2h, ambos False: `2437-2449` |
| API core | `app/api/views_core.py:864`, `931-934` | juiz/mapeamento: `938-943`; grava: `951-958` | equivalente API: `967-979` |

**Confirmada para o ping fora do raio efetivo:** `classificar_posicao` primeiro normaliza o raio (`app/ponto/services/geofence.py:114`), retorna `gps_incerto` se está fora e `accuracy is None` (`115-118`); as três portas gravam `None` e não entram nos gates de alerta citados. O raio padrão é 200 m, usado quando nulo/ilegível/não positivo (`169`, `190-196`).

Ressalvas que evitam afirmação excessiva:

- Sem accuracy **dentro** do raio retorna `dentro`, porque esse ramo vem antes do teste da accuracy (`app/ponto/services/geofence.py:115-118`).
- “Incerto” é o estado persistido; o retorno HTTP usa `_dentro_resp = _tipo_pos == 'dentro'`, portanto devolve **False**, não None, para o ping incerto fora do raio: web `app/ponto/views.py:3058,3109`; API `app/api/views.py:2414,2454`; core `app/api/views_core.py:944,984`.
- Intervalo também força `dentro_geofence=None`, independentemente da accuracy: web `app/ponto/views.py:3061-3063`; API `app/api/views.py:2418-2420`; core `app/api/views_core.py:948-950`. Portanto `dentro_None` não prova sozinho “fora com GPS incerto”.
- Web não exige realmente “consecutivos”: consulta já filtrada por False e sem janela; um None intermediário não dispara, mas também não quebra sua contagem de falsos históricos (`app/ponto/views.py:3081-3088`). API/core examinam os dois últimos não mock em 2h, de modo que None intermediário quebra a condição (`app/api/views.py:2438-2444`; core `968-974`).
- O painel de presença **não reavalia accuracy**: lê `dentro_geofence is False` e pode retornar verde com pings válidos todos None (`app/ponto/presenca.py:57-62,70-77`). O resumo expõe `p.accuracy`, apenas como dado (`123`); contagem fora usa `is False` (`113`).
- Não generalizar para batidas: `verificar_geofence` lê `Batida.gps_accuracy` (`app/ponto/services/geofence.py:287-292`) e cria registro `AlertaGeofence(tipo='gps_incerto')` (`301-306`). É fluxo distinto de PingGeo.
- Reconciliações consultadas são de chamados/alertas ligados a batidas, não releem PingGeo: `app/ponto/management/commands/reconciliar_geofence.py:24-33`, `app/ponto/services/geofence.py:428-432`; `reconciliar_alertas_geofence.py:28-45`.

## 16/11 11:43 · sessao 6cd97d087b6c · - · - · origem=Ronald (tmux)

Q4 — **estrutura coerente; proveniência dos logs e comparação histórica de dados pessoais [não verificado].**

- São **19 entradas acrescentadas**: 10 restauradas (`app/docs/CORTES.json:1725`, `1732`, `1739`, `1746`, `1753`, `1760`, `1767`, `1774`, `1781`, `1788`); 8 avais (`1802`, `1810`, `1818`, `1826`, `1834`, `1842`, `1850`, `1858`); nascimento de `pergunta_respondida` (`1866`). A validação em memória encontrou 255 IDs únicos no JSON inteiro e nenhum campo obrigatório vazio nas 19 entradas; todas as datas são aceitas pela conversão de `x` para `0` do gerador (`bin/gerar_cortes.py:35`).

- As 10 restaurações não têm chave `onde`; registram origem histórica e/ou referências na frase/estado (`app/docs/CORTES.json:1727`, `1729`, `1734`, `1741`, `1748`, `1755`, `1762`, `1769`, `1776`, `1783`, `1790`). As outras 9 têm `onde` apontando para `logs/AVAIS-1515/` (`app/docs/CORTES.json:1806`, `1814`, `1822`, `1830`, `1838`, `1846`, `1854`, `1862`, `1870`). Esses arquivos e o diretório `logs/` estão ausentes da cópia: existência e conteúdo **[não verificado]**; `logs/` é ignorado pelo repositório (`.gitignore:53`).

- As autoridades referidas pelas restaurações existem: `dia_da_pergunta_trancado` (`app/chamados/juizes.py:978`), `dia_sem_trabalho` (`445`), `esta_desligado` (`app/colaboradores/services/desligamento.py:23`), `folga_lancada_do_dia` (`app/ponto/precedencia.py:212`), `motivo_do_veto` (`app/ponto/services/furos_vetados.py:27`), `pauta_cadastro_existente` (`app/chamados/services/fabrica_da_celula.py:317`), `porta_alcancavel_do_dia` (`app/chamados/juizes.py:1693`), `status_do_dia_lavrado` (`app/ponto/status_dia.py:201`), `tarefa_do_dia` (`app/chamados/services/tarefa_do_dia.py:733`), `tem_gps` (`app/ponto/services/geofence.py:126`).

- Há também **13 alterações de estado**, todas atribuindo comprovação ao censo `logs/CORTES-RECEBIDO-FORA-DA-PAUSA/c1.r`; os resultados desse censo ficam **[não verificado]** (`app/docs/CORTES.json:902`, `909`, `916`, `923`, `1021`, `1028`, `1056`, `1063`, `1070`, `1091`, `1098`, `1154`, `1161`).

- Dados pessoais: a restauração associa primeiro nome a `p#34375`, mas essa mesma associação já consta em outros documentos da cópia (`app/docs/CORTES.json:1771`; `app/docs/BACKLOG.md:961`, `963`; `app/docs/RELATO.md:2856`). Os interlocutores nomeados nos novos avais também aparecem em entradas anteriores (`app/docs/CORTES.json:1807`, `1815`, `1831`; anteriores em `1188`, `1223`, `1342`, `1470`). A busca de padrões não encontrou CPF, telefone ou email nas 19 novas entradas; ausência de dado pessoal adicional em relação às árvores anteriores: **[não verificado]**, porque não há histórico Git disponível.

O Markdown é reproduzível integralmente pela função pura `markdown(JSON, agora=2026-11-16 11:38)`; a seção “Todos” coincide independentemente do horário (`bin/gerar_cortes.py:97`, `106`). Isso comprova reprodutibilidade do derivado atual; a execução histórica do gerador permanece **[não verificado]**.

## 16/11 11:43 · sessao b0195f44117c · - · - · origem=Ronald (tmux)

Pergunta 2 — **comparação histórica [não verificado]**. A cópia não contém `.git`; não consegui executar os dois `git diff` pedidos nem comparar o conteúdo histórico dos 56 arquivos do anexo. Os 56 caminhos existem na cópia; isso não prova preservação do merge. [não verificado]

O `RELATO.md` documenta a certificação anterior em `51678cbb2` e um ensaio de merge do comboio, mas não contém os dois diffs solicitados nem prova do delta `be4ebe85b → 99eccedf9`: `app/docs/RELATO.md:18`, `app/docs/RELATO.md:19`, `app/docs/RELATO.md:21`.

Para o arquivo compartilhado, o resultado final preserva a catraca de identidades, quantidades, composição e linhas reais: `app/core/tests/test_medida_turno_do_par.py:100`, `:151`, `:247`, `:541`, `:557`. Há registros correspondentes a ambos os grupos:

- Obra: `_dia_da_batida` e `_dia_do_contexto_fraco` registrados em `app/core/tests/test_medida_turno_do_par.py:258` e `:259`, correspondendo às expressões em `app/chamados/juizes.py:54` e `:170`.
- Main: `regularizar_retorno` registrado em `app/core/tests/test_medida_turno_do_par.py:339`, correspondendo à expressão em `app/ponto/views.py:3311`.

Executei o teste inteiro em memória, sem Django, banco ou bytecode, conforme o procedimento documentado em `app/core/tests/test_medida_turno_do_par.py:20`: **35 testes passaram, zero falhas/erros**. Isso confirma coerência do retrato final, mas **“preservou as duas intenções do merge” continua [não verificado]** sem as versões históricas.

A intenção funcional explícita do LOTE3 c7 está presente: posto e histórico dentro do mesmo `atomic`, com releitura do gravado após recusa (`app/colaboradores/views.py:331`, `:333`, `:343`); seu teste exercita falha depois da escrita real do histórico e exige rollback dos dois (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:230`, `:253`, `:256`). A intenção B1 c2 também está explícita no resultado: ressuscitar zera o par inteiro, conserva a trilha anterior e evita o ato `REABERTURA` que mudaria o dono da tarefa (`app/chamados/models.py:1613`, `:1622`, `:1629`). Isso comprova presença no código atual, não equivalência com os diffs anteriores.

## 16/11 11:42 · sessao 3147dc5e9e8d · - · - · origem=Ronald (tmux)

(c) Censo da cópia, referências relativas à raiz:

| Leitor/caminho | Evidência e efeito |
|---|---|
| Exportação da função por `chamados.models` | `app/chamados/models.py:4231` reexporta a função real. |
| Único chamador de produção localizado | `montar_chamados_fio`, `app/chamados/services/fio.py:373-376`: importa como `_eap`, grava `p.estado_aplicacao`; sem disputa ou em exceção, usa `None`. |
| Modal admin | `app/chamados/views.py:889` monta o fio com explicação; `:927-931` renderiza `modal_fio.html`. Re-render parcial passa pelo mesmo montador em `app/chamados/services/fio.py:484`, renderizado em `:534-535`. |
| Template consumidor efetivo | `app/templates/chamados/partials/modal_fio.html:230-246`: só mostra `detalhe` dos estados `bloqueada_paridade` (vermelho) e `ja_satisfeita` (azul), tanto validada quanto ainda não validada. |
| Precedência visual | `modal_fio.html:222-226`: `pendencia_admin` mostra “Pendencia do admin”, seu display e `recusa_motivo`, antes do ramo validada. `:228-229` mostra “✓ Validada”. Não há ramo que imprima `aplicada`, `recusada_paridade`, `aplicavel` ou `invalida` nesse bloco (`:230-248`). Portanto o anexo prova o retorno lógico `'aplicada'`, mas a legenda literal “aplicada” na tela não está confirmada pelo template desta cópia. | [nao verificado]
| Meu Atendimento | Também executa o montador (`app/chamados/views.py:569`) e renderiza `chamados/meu_atendimento.html` (`:632-635`), porém não foi localizado consumo de `estado_aplicacao` nos templates dessa tela. `[não verificado]` quanto a outros clientes fora da cópia. |
| Fio admin em página inteira | Executa o montador em `app/chamados/views.py:677` e renderiza `fio_colaborador.html` em `:697-703`; esse template não lê `estado_aplicacao`. `[não verificado]` quanto a alguma apresentação fora da cópia. |
| API de perguntas | `app/api/views.py:2550-2564` serve `p.estado` e `aguarda_admin`, não `estado_aplicacao_pergunta`; `:2568-2578` monta a resposta com pendentes/histórico. Não confundir esses estados com `'aplicada'`. |
| Contadores do modal | `app/chamados/views.py:900-912`: ativos vêm de `qs_na_fila`, mensagens do queryset de followups e disputas abertas de `fechada_em__isnull`; nenhum deriva do estado de aplicação. Não localizei contador de `'aplicada'` na cópia. `[não verificado]` quanto a fontes externas. |
| Testes que chamam a função | `app/chamados/tests/test_estado_aplicacao.py:48`, `:56`, `:63`, `:68-70`; `app/chamados/tests/test_badge_via_paridade.py:30-32`. O teste de paridade confirma apenas o dict da função; não verifica render de um badge. |
| Registro estrutural do leitor | `app/chamados/tests/test_contract_leitores_vitalidade.py:102` classifica a função como “exibicao: preve efeito da validacao”. |

Corroboração documental, sem substituir leitura do código: `app/docs/RELATO.md:653` diz que estado_aplicacao só era consumido no modal; `:1009` registra que o modal omite `aplicavel` e que `_pergunta_card`/detalhe/central/lote/copiloto não recebem esse estado. Atenção: `RELATO.md:673` contém uma afirmação anterior sobre C7 usar essa testemunha, mas a própria linha a corrige e remete a RAIZ F1; não usar a frase anterior como prova de chamador atual.

(e) **Encaixe: F4 da E4c**, pois o defeito está no leitor de efeito do materializador (`app/chamados/services/materializacao.py:431-448`), e o escopo registrado de F4 é “materializacao/passivo” (`app/docs/RELATO.md:292`), incluindo o desacordo ato/projeção explicitamente referido à função em `:115` e `:133`; RAIZ trata a relação agregado/filhos, destino, fechamento e pares (`app/docs/BACKLOG.md:2031`).

O diretório `logs/ESPINHA-BRIEFS/F4` não está disponível nesta cópia; seu conteúdo literal é `[não verificado]`. Há descrição documental do brief em `app/docs/RELATO.md:254`, sem acesso à matriz referida.

## 16/11 11:42 · sessao a62ff595dc63 · - · - · origem=Ronald (tmux)

Não localizei teste existente de **login DRF com cookie de sessão válido e CSRF ativado**; os testes examinados não montam essa combinação: `app/api/tests/test_api_login.py:58-63`, `app/api/tests/test_login_campos.py:27-32`, `app/api/tests/test_login_app_sem_colaborador.py:25-30`, `app/core/tests/test_contrato_trocar_senha.py:127-142`.

| Cobertura existente | Evidência e limite |
|---|---|
| Login bem-sucedido, erros e tokens | `app/api/tests/test_api_login.py:58-63` cria `APIClient()` novo por requisição, sem sessão; `:167-180` verifica 200 e payload. |
| Campos alternativos e JSON/form | `app/api/tests/test_login_campos.py:27-32` cria `Client()` novo, sem `enforce_csrf_checks`; casos em `:34-51`. |
| Login app/gestão | `app/api/tests/test_login_app_sem_colaborador.py:25-30` usa `self.client`, sem estabelecimento prévio de sessão; sucesso em `:38-46`. |
| Login core e flag de senha | `app/core/tests/test_contrato_trocar_senha.py:127-142` chama apenas `api.views_core.api_login` diretamente com `APIRequestFactory()`, sem passagem pelos middlewares. |
| Paridade dos dois kernels | `app/api/tests/test_r15_token_por_credencial.py:149-169` testa emissão de tokens e fonte de refresh; não faz login HTTP com sessão. |
| CSRF ativo existente próximo | `app/api/tests/test_api_mensageria_push.py:67-78` usa `Client(enforce_csrf_checks=True)`, mas POSTa mensageria sem cookie de sessão; não cobre login. |

**Armadilha de cobertura:** a suite configura `config.urls_test` em `app/config/settings_test.py:9`; esse URLconf concatena API core antes da UI em `app/config/urls_test.py:8-15`. Assim, `/api/auth/login/` nessa configuração escolhe core primeiro (`app/api/urls_core.py:9`), embora a docstring diga `api.views.api_login` (`app/api/tests/test_api_login.py:2`). Para testar UI é necessário selecionar explicitamente seu URLconf, cuja inclusão está em `app/config/urls.py:37` e rota em `app/api/urls.py:60`.

Esboço RED proposto, **não criado nem executado [nao verificado]**:

```python
# Fixture: usuario_sessao ativo; outro colaborador ativo com credenciais válidas.
# Repetir com ROOT_URLCONF="config.urls_core" e "config.urls".
with override_settings(ROOT_URLCONF=urlconf):
    client = APIClient(enforce_csrf_checks=True)
    client.force_login(usuario_sessao)

    assert settings.SESSION_COOKIE_NAME in client.cookies
    client.cookies.pop(settings.CSRF_COOKIE_NAME, None)
    # Não enviar Authorization, X-CSRFToken nem csrfmiddlewaretoken.

    response = client.post(
        "/api/auth/login/",
        {"username": usuario_login.username, "password": senha_login},
        format="json",
        HTTP_HOST="juliani.hasner.com.br",
    )

    assert response.status_code == 200, response.content
    assert response.data["colaborador"]["id"] == colaborador_login.pk
    assert {"access", "refresh"} <= response.data["tokens"].keys()
```

Usar usuários distintos verifica também que o login responde pelas credenciais do corpo, conforme os handlers consultam `autenticar_colaborador` em `app/api/views_core.py:195-198` e `app/api/views.py:209-212`. O resultado RED esperado antes de correção é o bloqueio CSRF descrito no anexo; execução e mensagem exata nesta cópia **[nao verificado]**.

## 16/11 11:42 · sessao be3cf7792cd1 · - · - · origem=Ronald (tmux)

- **Referência incorreta:** `BACKLOG:2159` identifica SOMBRA-HOJE-NA-BORDA; O10-D-PERMUTA-RECIPROCA está em `app/docs/BACKLOG.md:2172`.
- **Contrato O10-D:** quatro células; duas chamadas a `trocar_plantao` num atomic; validar 11h/sobreposição/permissão/exportada antes da primeira escrita; rollback integral; recusa falante; correlação; seis RED e confirmação DP 1/3/6 — `app/docs/BACKLOG.md:2172`.
- **Cruzamento físico O10-D com obras em voo:** [nao verificado]. O registro disponível declara ausência de cópia de construção — `app/docs/RELATO.md:1592`. Não há conjunto de alterações O10-D para comparar.
- **B1/B2 e BUG-PAR:** cruzamento entre BUG-PAR e B1 está documentado em `fio.py`, `test_s139.py`, `test_medida_turno_do_par.py` — `app/docs/RELATO.md:36`; medida anterior inclui `JUIZES.md` — `app/docs/RELATO.md:70`. BUG-PAR não constrói e B2 espera pouso B1 — `app/docs/RELATO.md:51`. F2/F4 também esperam B1 — `app/docs/RELATO.md:6`.
- **LOTE7:** conjunto documentado abrange `ponto/services/ausencia.py`, `regularizacao.py`, `retratar_humano.py`, `ferias/views.py` — `app/docs/RELATO.md:50`; pouso está em PAREI — `app/docs/RELATO.md:5`. Cruzamento físico O10-D nesses arquivos: [nao verificado].
- **Alcance transitivo real de O10-D:** `trocar_plantao` chama `editar_dia` — `app/ponto/portas/celula.py:713`; esta marca cobranças órfãs e chama `_o_chao_do_dia_mudou` — `app/ponto/portas/celula.py:663`; o fluxo rejulga célula — `app/ponto/portas/celula.py:463`, recalcula fechamento — `app/ponto/portas/celula.py:496` e agenda rejunção/reavaliação de chamados — `app/ponto/portas/celula.py:531`. Isso prova alcance em chamados, sem provar arquivos alterados em comum.
- **Interface ausência/BUG-PAR:** materialização importa e chama `criar_ausencia` — `app/chamados/services/materializacao.py:472`, `app/chamados/services/materializacao.py:491`, `app/chamados/services/materializacao.py:519`, `app/chamados/services/materializacao.py:527`. Alteração de `ausencia.py` pela O10-D: [nao verificado].

Para Q3, distinguir **colisão demonstrada** de **dependência que exige medição**: as provas disponíveis mostram a segunda para O10-D; o próprio item 4 manda esperar quando houver cruzamento [nao verificado — anexo fornecido sem numeração].

## 16/11 11:42 · sessao 9acbf789d5c7 · - · - · origem=Ronald (tmux)

Achados confirmados:

- **Citação errada:** `app/docs/BACKLOG.md:2159` é SOMBRA-HOJE-NA-BORDA; O10-D está em `app/docs/BACKLOG.md:2172`.
- **Contrato O10-D existente:** quatro células, duas chamadas a `trocar_plantao` no mesmo `atomic`, validação prévia, rollback integral, recusa falante e correlação da trilha constam em `app/docs/BACKLOG.md:2172`.
- **Lei de 09/11:** a linha da O10-D registra LEI-O10-D-PERMUTA 12:0x, seis perguntas mais quatro residuais, caminho `logs/O10-D-PERMUTA-NA-TELA/aval_lei_permuta.txt` e RED por regras 1–6; o texto integral dessa resposta não foi conferido. `app/docs/BACKLOG.md:2172`.
- **Censo_v2:** o RELATO registra conclusão e resultado `rc 0`, com ressalvas de rollback, ordenação das quatro células e datas iguais permitidas. `app/docs/RELATO.md:1630`. O RELATO também afirma que censo e lei estão prontos, enquanto os RED 1–6 ainda faltam. `app/docs/RELATO.md:1592`.
- **O10-C:** a condição INTERROMPIDA está comprovada em `app/docs/BACKLOG.md:2219`; a tabela também a classifica PARADA em `app/docs/RELATO.md:975`.
- **Molde PREPARA:** há despacho e resultado documentados, com construção bloqueada pelo cruzamento de arquivos, espera pelos pousos da F1 B1/R3 e continuidade da fila. `app/docs/RELATO.md:70`, `app/docs/RELATO.md:71`.
- **Ordem:** o registro anterior proíbe lateral por iniciativa, mas o aval fornecido é decisão nominal de Ronald; a entrada O10-D, isoladamente, não demonstra colisão. `app/docs/BACKLOG.md:9`. A precedência da espinha consta em `app/docs/RELATO.md:293`.
- **Pista:** lateral usa PISTA como pré-filtro; o pouso continua certificado pela VM. Esse requisito precisa permanecer no brief. `app/docs/RELATO.md:27`, `app/docs/BACKLOG.md:830`.
- **Restrição do construtor:** L-126 proíbe ao Codex `ponto/portas/celula.py`, `core/juizes.py`, competência exportada e item que pede `!`; o brief precisará esclarecer arquivos/atos permitidos, sem presumir que o `!` futuro autoriza essas escritas. `app/docs/LEIS.md:147`.

## 16/11 11:42 · sessao 9134330fae03 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 16/11 11:42 · sessao 3a666dfaf3c9 · - · - · origem=Ronald (tmux)

Q3 — **Há interação; a leitura estática não mostrou conflito de intenção. A atribuição exata dessas linhas ao delta do main fica [não verificado], porque a cópia não contém `.git`.**

- **Travas compartilhadas:** a porta do main chama `PerguntaDisputa.trava_na_ordem(pks)`, usando a autoridade da obra; passa apenas colaboradores, sem travar/escrever perguntas nesse caminho (`app/colaboradores/services/colaborador.py:72`, `:80`). A implementação trava Colaborador, Disputa, Chamado e Pergunta nessa sequência (`app/chamados/models.py:2770`, `:2779`, `:2793`, `:2797`). A ressurreição usa a mesma porta com os quatro conjuntos (`app/chamados/models.py:1612`).

- **`vincular_posto`:** posto e trilha ficam no mesmo `atomic` (`app/colaboradores/views.py:333`); chama `salvar_inteiro` e `registrar_mudanca` (`:334`, `:335`). O save completo guarda `situacao` da linha travada antes de salvar (`app/colaboradores/services/colaborador.py:152`, `:158`). A operação não altera diretamente `PerguntaDisputa`/`DecisaoPergunta`/`CelulaDia` (`app/colaboradores/views.py:311`). A antiga regeneração por troca do posto cadastral está expressamente sepultada: o lugar do dia vem do vínculo (`app/colaboradores/services/colaborador.py:118`).

- **Interação concreta via desligamento:** a cascata usa `respostas_sem_veredito()` para preservar disputas com resposta por julgar, senão chama a porta `fechar` (`app/colaboradores/services/desligamento.py:164`). Essa porta fecha perguntas e registra `DecisaoPergunta.SILENCIO` (`app/chamados/models.py:1999`, `:2005`). `tarefa_do_dia` lê SILENCIO como `silencio_decidido`, sem ação, enquanto REABERTURA devolve ao colaborador (`app/chamados/services/tarefa_do_dia.py:163`, `:176`). Portanto importa preservar a distinção adotada pela B1: ressurreição não chama `reabrir_para_correcao` nem fabrica REABERTURA (`app/chamados/models.py:1622`, `:1629`).

| Entidade/campos | Quem escreve | Quem lê |
|---|---|---|
| `PerguntaDisputa.respondida_em/resposta_colab`, validação e materialização | Ressurreição salva a projeção anterior em decisões e zera o par pela porta (`app/chamados/models.py:1613`, `:1629`); correção registra o ato e chama a mesma porta (`app/chamados/services/disputa_emissao.py:1609`, `:1631`, `:1648`); update efetivo em `descarimbar_em_lote` (`app/chamados/models.py:3391`). | Juiz `pergunta_respondida`: instante **E** texto após trim (`app/chamados/juizes.py:1544`, `:1552`), registrado como `_C21` (`app/core/juizes.py:306`). Face SQL `q_respondida_com_texto` (`app/chamados/juizes.py:1575`), lida por `q_vivas` (`app/chamados/models.py:3481`) e fila de validação (`app/chamados/juizes.py:1751`). |
| `DecisaoPergunta.ato`, instante e texto/foto anterior | Porta `registrar`, via `bulk_create` (`app/chamados/models.py:4110`, `:4120`); chamada pela ressurreição (`:1613`), correção (`app/chamados/services/disputa_emissao.py:1631`) e desligamento pela porta da disputa (`app/chamados/models.py:2005`). | Carga de `tarefa_do_dia` ordena por pergunta/instante/pk e corta pelo `agora` (`app/chamados/services/tarefa_do_dia.py:421`); julgamento em `_pela_decisao` (`:142`). Histórico de respostas: `respostas_anteriores` (`app/chamados/models.py:3668`). |
| `CelulaDia` | Veredito pela porta `lavrar_veredito`, delegada pelo modelo (`app/escala/models.py:1819`, `:1826`); escrita de veredito/via/instante (`app/ponto/portas/celula.py:1093`). Reabertura/ressurreição acordam células emitindo evento, não escrevem diretamente o veredito (`app/chamados/models.py:3341`, `:3358`). | `q_vivas` lê veredito e ata (`app/chamados/models.py:3488`, `:3493`); `tarefa_do_dia` carrega perguntas com célula (`app/chamados/services/tarefa_do_dia.py:406`). Telas do main também leem células, incluindo `trabalha` (`app/core/views.py:395`, `app/colaboradores/views.py:2900`). |
| `HistoricoVinculo` | `registrar_mudanca` escreve tipo, antes/depois, data, vigência, posto e autor (`app/colaboradores/services/historico_vinculo.py:7`, `:13`); `registrar_evento_de_vinculo` escreve eventos (`:20`, `:25`). Chamados pelo posto (`app/colaboradores/views.py:335`) e desligamento (`app/colaboradores/services/desligamento.py:138`). | Tela de detalhe lê histórico e contagem (`app/colaboradores/views.py:306`). Nenhuma leitura dessa entidade foi encontrada nas funções da obra citadas; ausência global de leitores [não verificado]. |

Outra interação do main: INSS agora guarda situação e cria ausência dentro do mesmo `atomic` (`app/ponto/views.py:2469`, `:2471`, `:2472`, `:2489`); o signal da ausência chama julgamento das células (`app/ponto/signals.py:143`, `:147`). Isso alcança o domínio que os juízes da obra leem, com Colaborador travado antes da cascata (`app/colaboradores/services/colaborador.py:94`).

`core/auditoria.py` delega a escrita da trilha a `evento`, sem alterar os campos de resposta ou decisões (`app/core/auditoria.py:8`, `:22`). Execução concorrente, suite no hash e efeitos em produção: **[não verificado]**.

## 16/11 11:42 · sessao 61988738bd5d · - · - · origem=Ronald (tmux)

Censo estático: **3 implementações**, todas aceitam **POST**, sem `@authentication_classes` e sem `@csrf_exempt` explícitos nos blocos de decoração:

| Kernel | Endpoint | Implementação | Rota |
|---|---|---|---|
| core | `/api/auth/login/` | `app/api/views_core.py:188-190`: `@api_view(['POST'])`, `@permission_classes([AllowAny])` | `app/config/urls_core.py:6` + `app/api/urls_core.py:9` |
| ui | `/api/auth/login/` | `app/api/views.py:202-204`: `@api_view(['POST'])`, `@permission_classes([AllowAny])` | `app/config/urls.py:37` + `app/api/urls.py:60` |
| ui | `/api/diag/cam/` | `app/api/views.py:2642-2644`: `@api_view(['POST'])`, `@permission_classes([])` | `app/config/urls.py:37` + `app/api/urls.py:102` |

O padrão contém `JWTComCredencial` primeiro e `SessionAuthentication` depois; permissão padrão `IsAuthenticated`: `app/config/settings/base.py:32-44`.

Exclusões:

- `HasnerTokenRefreshView`: as duas subclasses locais não declaram autenticação, mas herdam de **`TokenRefreshView`**, não diretamente de `APIView`; não basta olhar a classe local para concluir herança de `SessionAuthentication`. Bases importadas em `app/api/views.py:7`, `app/api/views_core.py:8`; subclasses em `app/api/views.py:69-128`, `app/api/views_core.py:125-184`; rotas `app/api/urls.py:63`, `app/api/urls_core.py:12`. Fonte da biblioteca ausente da cópia pesquisada: atributos efetivos da base **[nao verificado]**.
- `api_app_version`: função Django simples, sem `@api_view`, retorna `JsonResponse`: `app/api/views.py:3042-3054`.
- Mensageria push: função Django `@csrf_exempt` e `@require_POST`, com guarda de token; fora do universo DRF: `app/api/views_mensageria.py:476-481`.
- Único DRF localizado fora de `app/api`: `api_pendentes`, apenas GET e `IsAuthenticated`; fora do censo público: `app/comunicados/views.py:83-88`, rota `app/api/urls.py:85`.

A declaração de rotas públicas registra login para as duas cascas e telemetria de câmera anônima: `app/core/rotas_publicas.py:49-62`.

Pesquisa global não localizou ViewSets, outras subclasses DRF, `authentication_classes`, `get_authenticators` ou outro `REST_FRAMEWORK` no código produtivo; a ausência global não possui uma linha única comprobatória **[nao verificado]**.

## 16/11 10:39 · sessao c49bf044a323 · E4C-F1-NOVA-POUSO-B1 · censo · origem=Code

**(e) NÃO PERGUNTADO** — nenhuma `.q` fornecida pede conferir se a frase de nascimento está na fonte `CORTES.json` e sobreviverá à regeneração; as listas incluem apenas classes genéricas próximas. (`codex_verificador_v1.q:23–26`; `codex_verificador_v2.q:5–18`; `codex_verificador_v3.q:4–14`; `codex_verificador_c2_v1.q:6–12`)

Nos anexos colados, numerei a primeira linha após cada delimitador como linha 1, contando linhas vazias.

O bloqueante é a **persistência do corte na fonte**: a frase aparece no Markdown, e o gerador reescreve esse arquivo inteiro a partir do JSON; a ausência no JSON é registrada pelo opus. (`app/docs/CORTES.md:461`; `bin/gerar_cortes.py:23–24`; `bin/gerar_cortes.py:114–116`; `opus_final.md:4–5`)

**Trechos de cada `.q`**

| Pergunta | Trecho exato e relação com o bloqueante |
|---|---|
| `codex_verificador_v1.q` | “O que o diff diz fazer: nasce o juiz `chamados/juizes.py::pergunta_respondida`” (`:5`). A lista contém “derivacao paralela de fonte unica; selo/RED sem mordida” (`:24`) e “pouso antes do pre-requisito” (`:25`). **Menciona nascimento e classes genéricas; não pergunta pela inscrição no JSON nem pela sobrevivência à geração.** (`:1–30`) |
| `codex_verificador_v2.q` | “PAPEL: verificador, VOLTA 2 (so o delta)” (`:1`); “Para cada achado da volta 1 marcado PROCEDENTE na triagem (1, 4, 5, 6, 7): o diff fecha o achado?” (`:6`). **Não menciona diretamente o bloqueante.** (`:5–18`). Eventual inclusão indireta pelos achados anteriores: **[nao verificado]**, pois a resposta anterior e sua triagem não foram transcritas. |
| `codex_verificador_v3.q` | “PAPEL: verificador, VOLTA 3 (so o delta 1fcb5d0b6..HEAD)” (`:1`); “Os itens 2a, 2b, 2c, 3 e 5 da volta 2: o diff os fecha?” (`:4`). **Não menciona diretamente o bloqueante.** (`:4–14`). Eventual inclusão indireta pelos itens anteriores: **[nao verificado]**. |
| `codex_verificador_c2_v1.q` | “Proibido na obra: tipo de ato novo, juiz novo, lista paralela, regra em tarefa_do_dia que reconheca ressuscitacao por texto de motivo.” (`:3`). O checklist contém “derivacao paralela de fonte unica; selo/RED sem mordida; ordem de trava; pouso antes do pre-requisito” (`:12`). **Não pergunta pela fonte canônica do corte.** (`:6–12`) | [nao verificado]

**Trechos de cada `.r`**

Das quatro respostas nomeadas, somente `codex_verificador_c2_v1.r` foi transcrita; o conteúdo de `codex_verificador_v1.r`, `codex_verificador_v2.r` e `codex_verificador_v3.r` está **[nao verificado]**. Não é possível fornecer trechos exatos dessas três respostas nem afirmar que omitiram o tema. **[nao verificado]**

Na resposta c2, os trechos mais próximos são:

> “**NAO HA nova no trecho c2:** universo, preservação e integridade reutilizam as respectivas autoridades” — classe “Derivação paralela de fonte única”. (`codex_verificador_c2_v1.r:97`)

> “**NAO HA quanto à volta de REABERTURA:** há proibição direta e comparação com SILENCIO; cobertura integral de fila/trilha permanece insuficiente” — classe “Selo/RED sem mordida”. (`codex_verificador_c2_v1.r:98`)

> “**[nao verificado]:** os anexos estabelecem os pré-requisitos, mas não fornecem sua execução nem prova do pouso.” — classe “Pouso antes do pré-requisito”. (`codex_verificador_c2_v1.r:100`) [nao verificado]

**A resposta c2 não menciona `CORTES.md`, `CORTES.json`, `gerar_cortes.py` ou a perda da frase na regeneração.** (`codex_verificador_c2_v1.r:1–101`)

**Triagem correspondente**

Não há item de triagem correspondente ao bloqueante entre os seis descartes apresentados; eles tratam de campos/leitores, projeção legada, preservação/concorrência, testes, chamadores e relógio. (`entrega.md:47–56`) [nao verificado]

Existe, fora dessa triagem, este alerta exato:

> “Resolucao proposta (nao aplicada): LEIS L-130 = a linha da copia; CORTES = manter as DUAS linhas (conferir o escritor   [nao verificado]
> canonico CORTES.json/gerar_cortes.py lendo a fonte -- nao rodar). Zero `.py` em conflito.” (`entrega.md:9–10`)

Esse trecho pede uma conferência; não registra a descoberta da entrada ausente nem seu descarte. (`entrega.md:9–10`)

**O tema estava na lista de classes?** **Não como classe específica.** Estava a expressão genérica **“derivacao paralela de fonte unica”**, nas perguntas v1 e c2; não estava a classe “frase de corte de juiz novo somente em arquivo derivado, ausente da fonte geradora”. (`codex_verificador_v1.q:23–26`; `codex_verificador_c2_v1.q:12`)

## 16/11 10:02 · sessao 780bea312735 · CORTES-F2-F9-1611 · construtor · origem=Code

1. **Confirmei inalcançável.** A porta descarta a pergunta não viva e retorna; a segunda guarda consulta a mesma autoridade sem alteração intermediária de `p` ([views.py:1238](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/views.py:1238), [views.py:1257](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/views.py:1257)). A autoridade memoiza por instância e estado ([juizes.py:893](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/juizes.py:893)).

2. Escrito [test_f6_u9b_ramo_inalcancavel.py:23](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/tests/test_f6_u9b_ramo_inalcancavel.py:23): verificador AST, `SimpleTestCase`, fonte localizada pelo módulo importado e asserção RED. `test_a_costura_MORDE` cobre o ramo completo e suas duas condições isoladas; há controle positivo da lápide em comentário ([teste:91](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/tests/test_f6_u9b_ramo_inalcancavel.py:91), [teste:126](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/tests/test_f6_u9b_ramo_inalcancavel.py:126)). [nao verificado]

3. **MUTANTE declarado:** ramo presente ⇒ **FAIL esperado pela asserção**; ramo removido com lápide ⇒ **OK esperado** ([teste:84](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/tests/test_f6_u9b_ramo_inalcancavel.py:84)). Comando para ambos os estados: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_f6_u9b_ramo_inalcancavel --settings=config.settings.ci -v 2
```

Comando dos mutantes sintéticos:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_f6_u9b_ramo_inalcancavel.RamoInalcancavelU9bTest.test_a_costura_MORDE --settings=config.settings.ci -v 2
```

4. Não executei testes. Resultados RED/OK, importação e setup em execução: **[nao verificado]**. [nao verificado]

_6006 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
