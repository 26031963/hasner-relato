# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 23:31** · publicado em **15/11/2026 23:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 23:31 · sessao 0a3fd6325a9e · EM-DISPUTA-TRES-DERIVACOES · leitura · origem=Code

**(a) Censo.** O aval confere: há **três derivações variáveis de `em_disputa`**, com perguntas e fontes diferentes; também há **cinco escritores de `False` constante** nos retornos de duplicata, colisão e reuso. (`app/api/views_core.py:753-765`; `app/api/views.py:759-771`; `app/api/views.py:2569`; `app/api/views.py:1336`; `app/ponto/services/resposta_batida.py:29`.)

| Derivação | Escritor e fonte | Pergunta efetivamente respondida | Leitor/publicação |
|---|---|---|---|
| Anomalia, core | Lista local `motivos_anomalia`; se não vazia, chama emissão da disputa e atribui `True`. | “Este processamento da batida terminou com algum motivo de anomalia?” Não consulta existência de pergunta. | Atribuição: `app/api/views_core.py:753-765`; JSON: `app/api/views_core.py:815-816`. |
| Anomalia, UI | Mesmo procedimento no gêmeo. | Mesma pergunta. | Atribuição: `app/api/views.py:759-771`; JSON: `app/api/views.py:821-822`. |
| Pendência no app | `bool(consulta_do_app(colab)['pendentes'])`. | “A consulta atual oferece alguma pergunta pendente?” | Consulta: `app/api/views.py:2537-2539`; flag/lista: `app/api/views.py:2569-2578`. |
| Disputa aberta | `colaborador_em_disputa`: primeira disputa do colaborador com `fechada_em IS NULL`. | “O colaborador tem disputa aberta?” Não consulta perguntas. | Autoridade: `app/chamados/services/disputa_emissao.py:34-42`; chamada: `app/api/views.py:1336`; JSON: `app/api/views.py:1670`. |

A lista de anomalias **não vem apenas de `detectar_anomalias`**: recebe contribuições de selfie, mock, assinatura/liveness, entrada/saída incompleta e desvio grave; pode perder o motivo de saída sem turno. A fonte do flag é a lista **final**, depois desses passos. (`app/api/views.py:433-495`; `app/api/views.py:604-645`; `app/api/views.py:703-707`; `app/api/views.py:748-771`.)

| Escritores constantes/propagadores | Significado |
|---|---|
| `app/api/views_core.py:529`; `app/api/views.py:543` | Duplicata: `em_disputa=False`, motivos vazios. |
| `app/api/views_core.py:681`; `app/api/views.py:690` | Colisão oposta ignorada: `False`, motivos vazios. |
| `app/ponto/services/resposta_batida.py:7-30` | Reuso sem marca transitória `criada_neste_toque`: `False`, motivos vazios. |
| `app/api/views_core.py:684-686`; `app/api/views.py:693-695`; `app/ponto/services/triagem_batida.py:723-725` | Propagam o retorno do serviço de reuso. |

O comentário da selfie permanece exatamente em `app/api/views_core.py:426-427`; a regra executável está em `app/api/views_core.py:433-439`: consulta `selfie_ausente`, com canal derivado do `device_id` e tempo decorrido zero. Portanto, aquela referência trata a contribuição **selfie** para os motivos; não estabelece equivalência entre anomalia e pergunta disponível. (`app/ponto/registro_batida.py:53-61`; `app/ponto/registro_batida.py:80-91`.)

O roteamento das cascas é este:

| Casca | Caminho verificado na cópia |
|---|---|
| Caddy | Encaminha `/api/ponto/*`, `/api/auth/*`, `/api/me/`, `/api/regularizacao/*`, `/api/ping-geo/`, `/api/registrar-fcm/` e `/health/` ao core; restante à UI. Juliani importa esse bloco. (`Caddyfile:8-10`; `Caddyfile:32-38`.) |
| Core | `config/urls_core.py` inclui `api.urls_core`, que importa `views_core`; `/me/` e `/ponto/bater/` usam esse módulo. (`app/config/urls_core.py:5-6`; `app/api/urls_core.py:5`; `app/api/urls_core.py:13-14`.) |
| UI | Inclui `api.urls`; oferece espelho/v2, chamados/v2, perguntas e resposta. O gêmeo `/ponto/bater/` existe nesse URLconf, mas a regra do Caddy encaminha essa URL ao core. (`app/config/urls.py:37`; `app/api/urls.py:67-93`; `Caddyfile:8-10`.) |

Essa divisão coincide com a seção 2 do primer; o ambiente efetivamente carregado em produção permanece [nao verificado]. (`CLAUDE.md:84-90`; seleção por variável em `app/config/settings/saas.py:76-78`.)

Os equivalentes de apresentação e seus leitores são:

| Chave/valor | Quem calcula | Quem lê/propaga |
|---|---|---|
| `pendentes`, `historico` | `consulta_do_app` aplica C6 e separa as vivas; pendente exige ainda resposta ausente e disputa aberta. (`app/chamados/services/pergunta_no_app.py:45-85`.) | GET perguntas: `em_disputa`, `perguntas`, `respondidas`, `historico`; `disputa_id` e `chamado_id` ancoram as pendentes. (`app/api/views.py:2566-2578`.) |
| `questionarios_pendentes` | Comprimento de `pendentes`. (`app/chamados/services/pergunta_no_app.py:100-101`.) | `/me/` core e UI; dashboard; painel administrativo do colaborador. (`app/api/views_core.py:303-307`; `app/api/views_core.py:346-348`; `app/api/views.py:315-319`; `app/api/views.py:358-360`; `app/core/views.py:252-268`; `app/colaboradores/views.py:940-941`.) |
| `chamados_badge` | Prioriza pendentes com “Responder DP”; depois chamados com nota não lida; senão `None`. (`app/chamados/services/pergunta_no_app.py:102-118`.) | Propagado pelos dois `/me/`. (`app/api/views_core.py:348`; `app/api/views.py:360`.) |
| Link, badge e rótulo web | Leem `questionarios_pendentes`. | Decidem `?aba=cham`, badge e “Responder DP”. (`app/templates/core/dashboard_colaborador_v2.html:78-80`.) |
| Apresentação “No app dele agora” | Lê a projeção central. | Total e descrição no painel administrativo. (`app/templates/colaboradores/partials/painel_colaborador.html:39`; `app/templates/colaboradores/partials/painel_colaborador.html:190-191`.) |
| `perguntas_disputa` em chamados/v2 | Reutiliza pendentes + histórico vivo, exclui disputa fechada e ancora no destino vivo ou pai. | Serialização em cada chamado. (`app/api/views.py:996-1040`; `app/api/views.py:1148-1168`.) |
| `pode_solicitar_correcao` | Exige perguntas, nenhuma aguardando colaborador e limite de reaberturas. | Payload de chamados/v2. (`app/api/views.py:1174-1180`.) |
| Aviso de pergunta pendente | `aviso_texto_livre` lê a mesma lista pendente. | Orienta abrir o chamado da pergunta; não interpreta mensagem como resposta. (`app/chamados/services/aviso_pendencia.py:7-20`.) |
| Push de disputa | Emissor envia URL `/meu-atendimento/` e FCM `tipo='disputa'`; recobrança também envia esse tipo. | Tratamento nativo desses sinais [nao verificado]. (`app/chamados/services/disputa_emissao.py:1747-1752`; `app/chamados/services/disputa_emissao.py:1778-1806`; `app/chamados/fcm_utils.py:75-79`.) |

Há leitores adicionais da derivação **disputa aberta**, fora da abertura do questionário:

- O reexport em `chamados.models` alimenta `gate_disputa`, que lê a disputa para recusar POST; GET e operações isentas passam antes. (`app/chamados/models.py:4234-4236`; `app/colaboradores/mixins.py:291-313`.)
- Os decoradores não isentos aparecem em holerite e operações de colaboradores. (`app/holerite/views.py:1144`; `app/colaboradores/views.py:1368`; `app/colaboradores/views.py:1603`; `app/colaboradores/views.py:1769`; `app/colaboradores/views.py:1877`.)
- `motor_bloqueado_por_disputa` delega ao mesmo helper; o espelho o lê e publica `disputa_aberta`, consumida pela faixa do template. (`app/ponto/services/disputa.py:15-31`; `app/ponto/views.py:140-144`; `app/ponto/views.py:207`; `app/templates/ponto/espelho.html:13-18`.)
- O PDF lê o wrapper para bloquear emissão. (`app/relatorios/pdf_espelho.py:1082-1090`; `app/relatorios/pdf_espelho.py:1113`.)
- O painel tem derivadores próprios do mesmo predicado: `exists(disputa aberta)` ou pertencimento ao conjunto recebido; o leitor transforma isso em LED vermelho. O lote monta seu próprio conjunto de disputas abertas. (`app/colaboradores/painel_cell.py:20-32`; `app/colaboradores/painel_cell.py:68`; `app/colaboradores/services/situacional.py:56-59`; `app/colaboradores/services/situacional.py:117`; `app/colaboradores/services/situacional.py:210`.)

Esses homônimos não devem ser confundidos: `em_disputa_fechada` controla ações administrativas; `pode_responder` de chamados/v2 testa chamado não terminal; a regularização externa lista outro modelo. (`app/chamados/services/fio.py:264-268`; `app/templates/chamados/partials/modal_fio.html:289`; `app/api/views.py:1145`; `app/api/views_core.py:1271-1289`.)

O `HANDOFF.md` anuncia contratos do app, mas suas seções tratam login, ausência, plataforma e senha: não documenta o contrato de `em_disputa`/perguntas. A condição executada pelo Android 3.9.3 para abrir a Activity, o campo que ele efetivamente lê e a ocorrência relatada permanecem **[nao verificado]**; o comentário da selfie não substitui esse leitor. (`app/docs/HANDOFF.md:1-219`; `app/api/views_core.py:426-427`.)

**(b) Sonda.** A contagem histórica exata pedida está **[nao verificado]**. `Batida` não conserva o flag/lista daquele retorno; `ultima_batida` é contexto corrente que pode ser substituído. Além disso, o detector inicial não recebe instante e lê cadastro atual. (`app/ponto/models.py:8-95`; `app/api/views_core.py:753-765`; `app/chamados/services/disputa_emissao.py:569-575`; `app/chamados/detector_anomalias.py:19-123`.)

Os juízes aceitam `agora` em parte do caminho, mas não reconstituem todo o estado passado: a projeção preserva identidade atual, a tranca e o veredito são atuais, e a partição lê os carimbos atuais. Passar o instante da batida não produziria uma contagem contemporânea exata. (`app/chamados/juizes.py:1381-1419`; `app/chamados/juizes.py:1450-1466`; `app/chamados/services/pergunta_no_app.py:52-85`.)

O script abaixo, **somente texto e não executado**, deixa a métrica histórica como `None` e entrega duas aproximações separadas: detector inicial atual e desvio grave recalculado. São as funções reais chamadas pelo escritor; não representam seu flag final. Cada uma é cruzada com os dois candidatos de (c), no estado atual. (`app/api/views_core.py:419-420`; `app/api/views_core.py:694-698`; `app/api/views_core.py:753-765`.)

O canal é o gravado; `NULL` permanece desconhecido, inclusive no trecho anterior ao nascimento do campo, sem inferência por timestamp do aparelho. (`app/ponto/models.py:46-55`.)

```python
# SOMENTE TEXTO. Para shell Django ja selecionado no tenant juliani.
# Nao executado nesta leitura.
#
# Fontes:
# app/api/views_core.py:419-420       detector inicial do escritor
# app/api/views_core.py:694-698       componente desvio grave
# app/api/views_core.py:753-765       flag usa motivos FINAIS, nao so estes componentes
# app/chamados/detector_anomalias.py:19-123  detector atual, sem instante passado
# app/ponto/utils.py:103-133         escrita/push interceptados antes da tentativa
# app/chamados/juizes.py:865,1402-1492       C6 real
# app/chamados/services/pergunta_no_app.py:45-85  pendencia atual do app
# app/core/observ.py:60-82           contador de erros engolidos
# app/ponto/models.py:23,45-55,67,79 universo, canal e carimbos
#
# APROXIMACAO:
# unidade = Batida persistida, timestamp nos ultimos 30 dias corridos;
# elegibilidade = origem ATUAL == "app", sem afirmar qual endpoint a recebeu;
# inclui retratadas: nao e universo de apuracao nem contagem de requests/retries;
# sinais = dois componentes reais, recalculados separadamente no snapshot atual;
# portas = C6 atual e pendentes atuais do app, sem escolher entre eles;
# NAO reconstitui em_disputa historico nem pergunta disponivel naquele instante.
#
# Cada componente soma o mesmo universo elegivel, inclusive indecidiveis.
# Resultados por batida: um colaborador com varias batidas pesa varias vezes.
# Nenhum nome, CPF, resposta, contexto ou texto de excecao vai para a saida.

import io
import json
import logging
from collections import Counter, defaultdict
from contextlib import redirect_stdout, redirect_stderr
from datetime import timedelta
from types import SimpleNamespace
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone

universo = None
processadas = 0
elegiveis = 0
janela = None
fatal = None
erros = []
interceptadas = Counter()
canais = defaultdict(Counter)
contagens = defaultdict(Counter)
memo_base = {}
memo_portas = {}
nivel_logging = logging.root.manager.disable


def emitir(obj):
    print(json.dumps(obj, ensure_ascii=False, sort_keys=True))


def canal_gravado(b):
    if b.canal in ("apk", "pwa"):
        return b.canal
    if b.canal is None:
        return "NULL_desconhecido"
    return "fora_das_choices"


def interceptar_criacao(*args, **kwargs):
    # bloquear_se_em_ferias usa somente pk no push e devolve o objeto. [nao verificado]
    # O detector usa o booleano; seu contexto nao e emitido nem medido.
    interceptadas["ChamadoColaborador.criar"] += 1
    return SimpleNamespace(pk=0)


def interceptar_push(*args, **kwargs):
    interceptadas["push_dp"] += 1 [nao verificado]
    return None


class ErroInternoDoDetector(Exception):
    pass


def medir(etapa, func, *args, pessoa=None, batida=None, **kwargs):
    antes = engolidos()["total"]
    valor = None
    falha = None
    try:
        # Savepoint permite continuar se uma tentativa incidental de SQL
        # proibido for recusada pela transacao READ ONLY.
        with transaction.atomic():
            with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
                valor = func(*args, **kwargs)
    except Exception as exc:
        falha = type(exc).__name__

    delta = engolidos()["total"] - antes
    if falha is not None or delta:
        erro = {
            "etapa": etapa,
            "erro": falha or "erro_engolido",
            "engolidos": delta,
        }
        if pessoa is not None:
            erro["pessoa"] = pessoa
        if batida is not None:
            erro["batida"] = batida
        erros.append(erro)
        return None
    return valor


def ler_base(colab):
    motivos, contexto = detectar_anomalias(colab)
    # Estes erros sao capturados pelo detector sem passar por engolidos().
    # Nunca interpretar resultado parcial como ausencia de anomalia.
    if any(str(k).startswith("_erro") for k in contexto):
        raise ErroInternoDoDetector()
    return bool(motivos)


def ler_grave(b):
    estado, _contexto = detectar_batida_fora_escala(
        b.colaborador, b.timestamp, b.tipo
    )
    if estado not in ("ok", "leve", "grave", "informativa"):
        raise ErroInternoDoDetector()
    return estado == "grave"


def ler_portas(colab, agora):
    # Universo C6 completo do colaborador, sem substituir C6 por q_vivas.
    qs = (
        PerguntaDisputa.objects
        .filter(disputa__colaborador=colab)
        .select_related("disputa", "celula", "chamado_destino")
        .order_by("pk")
    )
    perguntas = list(
        um_colaborador_por_identidade(qs, {colab.pk: colab})
    )
    with tranca_da_consulta(perguntas):
        n_c6 = sum(
            1 for p in perguntas if pergunta_viva(p, agora=agora)
        )

    # Le a particao real; nao reproduz seus filtros na sonda.
    consulta = consulta_do_app(colab, agora=agora)
    return n_c6, len(consulta["pendentes"])


def contabilizar(canal, componente, sinal, portas):
    c = contagens[(canal, componente)]
    c["universo_elegivel"] += 1

    if sinal is None:
        c["sinal_indecidivel"] += 1
        return
    if not sinal:
        c["sinal_ausente"] += 1
        return

    c["sinal_presente"] += 1
    if portas is None:
        c["sinal_presente_portas_indecidiveis"] += 1
        return

    n_c6, n_pendentes = portas
    c["sinal_presente_portas_decididas"] += 1
    if n_c6 == 0:
        c["sinal_presente_sem_C6_viva_no_snapshot"] += 1
    if n_pendentes == 0:
        c["sinal_presente_sem_pendente_no_snapshot"] += 1


try:
    logging.disable(logging.CRITICAL)

    from chamados.models import ChamadoColaborador, PerguntaDisputa
    from chamados.detector_anomalias import (
        detectar_anomalias,
        detectar_batida_fora_escala,
    )
    from chamados.juizes import (
        pergunta_viva,
        tranca_da_consulta,
        um_colaborador_por_identidade,
    )
    from chamados.services.pergunta_no_app import consulta_do_app
    from core.observ import engolidos
    from ponto.models import Batida

    if connection.vendor != "postgresql":
        fatal = "exige_postgresql"
    elif connection.in_atomic_block:
        fatal = "iniciar_fora_de_atomic"
    elif getattr(connection, "schema_name", None) != "juliani":
        fatal = "exige_tenant_juliani"

    if fatal is None:
        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )

            agora = timezone.now()
            inicio = agora - timedelta(days=30)
            janela = {
                "inicio_inclusivo": timezone.localtime(inicio).isoformat(),
                "fim_exclusivo": timezone.localtime(agora).isoformat(),
                "campo_do_recorte": "Batida.timestamp",
                "instante_das_portas": timezone.localtime(agora).isoformat(),
            }

            qs = (
                Batida.objects
                .filter(timestamp__gte=inicio, timestamp__lt=agora)
                .select_related(
                    "colaborador__empresa",
                    "colaborador__posto",
                )
                .order_by("colaborador_id", "pk")
            )
            universo = qs.count()

            # Intercepta os efeitos conhecidos ANTES de chamar o detector.
            # Nao chama views, emissao de disputa nem endpoints HTTP.
            with patch.object(
                ChamadoColaborador,
                "criar",
                side_effect=interceptar_criacao,
            ), patch(
                "chamados.push_utils.push_dp", [nao verificado]
                side_effect=interceptar_push,
            ):
                for b in qs.iterator(chunk_size=500):
                    canal = canal_gravado(b)
                    canais[canal]["universo"] += 1
                    processadas += 1

                    if b.retratada_em is not None:
                        canais[canal]["retratadas_no_snapshot"] += 1

                    if b.origem != "app":
                        canais[canal]["outras_origens_atuais"] += 1
                        continue

                    elegiveis += 1
                    canais[canal]["origem_app_atual"] += 1
                    cid = b.colaborador_id
                    pessoa = f"col{cid:03d}"

                    if cid not in memo_base:
                        memo_base[cid] = medir(
                            "detector_inicial_atual",
                            ler_base,
                            b.colaborador,
                            pessoa=pessoa,
                        )
                    if cid not in memo_portas:
                        memo_portas[cid] = medir(
                            "portas_atuais",
                            ler_portas,
                            b.colaborador,
                            agora,
                            pessoa=pessoa,
                        )

                    grave = medir(
                        "desvio_grave_recalculado",
                        ler_grave,
                        b,
                        pessoa=pessoa,
                        batida=b.pk,
                    )
                    portas = memo_portas[cid]
                    contabilizar(
                        canal, "detector_inicial_atual",
                        memo_base[cid], portas,
                    )
                    contabilizar(
                        canal, "desvio_grave_recalculado",
                        grave, portas,
                    )

        if processadas != universo:
            fatal = "universo_incompleto"

except Exception as exc:
    fatal = fatal or type(exc).__name__
finally:
    logging.disable(nivel_logging)

if fatal is not None:
    erros.append({"etapa": "fatal", "erro": fatal})

componentes = (
    "detector_inicial_atual",
    "desvio_grave_recalculado",
)
campos = (
    "universo_elegivel",
    "sinal_indecidivel",
    "sinal_ausente",
    "sinal_presente",
    "sinal_presente_portas_indecidiveis",
    "sinal_presente_portas_decididas",
    "sinal_presente_sem_C6_viva_no_snapshot",
    "sinal_prese

[truncado em 20000 de 25241 caracteres]

## 15/11 23:26 · sessao ba4226c49212 · - · - · origem=Ronald (tmux)

Censo de derivadores e propagadores, com referências atuais:

| Categoria | Escritor / fonte | Pergunta real / leitor |
|---|---|---|
| Anomalia, UI | `app/api/views.py:759-771` | Lista local `motivos_anomalia` não vazia; chama criador da disputa, ignora retorno e publica em `:821`. |
| Anomalia, core | `app/api/views_core.py:753-765` | Mesmo predicado; publica em `:815`. |
| Perguntas pendentes | `app/api/views.py:2537-2539,2569` | `bool(consulta_do_app(colab)['pendentes'])`; lista publicada em `:2576`. |
| Disputa aberta | `app/chamados/services/disputa_emissao.py:34-42` | `DisputaSupervisao` do colaborador com `fechada_em IS NULL`; retorno `.first()`. `api/views.py:1336` chama e `:1670` publica booleano. |
| Constante de dedup | `app/api/views.py:543`; `app/api/views_core.py:529` | Resposta da duplicata: `False`, motivos vazios em `:544` / `:530`. |
| Constante de colisão oposta | `app/api/views.py:690`; `app/api/views_core.py:681` | Resposta de batida ignorada: `False`, motivos vazios em `:691` / `:682`. |
| Constante de reuso | `app/ponto/services/resposta_batida.py:7-30` | `False` quando não há marca transiente `criada_neste_toque`; propagadores: `api/views.py:693-695`, `api/views_core.py:684-686`, `ponto/services/triagem_batida.py:723-725`. |

Portanto, **três derivações variáveis** do campo JSON, mais **cinco sítios de constante `False`**; as quatro citações de derivação do aval conferem nas linhas atuais acima. A segunda variável significa pendente, mais restrita que simplesmente “pergunta viva”: `consulta_do_app` chama C6 em `app/chamados/services/pergunta_no_app.py:73`; `particao_do_app` exige ainda `respondida_em is None` e disputa aberta em `:76-85`.

Fonte completa de motivos da anomalia: detector inicial em `app/api/views.py:433-434`; selfie em `:447-453`; mock em `:457-464`; assinatura/liveness em `:465-495`; E/S incompleta em `:604-645`; desvio grave em `:703-707`; ajuste da saída sem turno em `:748-756`. Só chamar `detectar_anomalias(colab)` não reproduz o flag final — os sítios posteriores acrescentam e removem motivos nessas linhas.

A referência da selfie também confere: `app/api/views_core.py:426-427` é comentário do desvio APK/POST separado; a regra executável está em `:433-439` [essas linhas finais precisam conferência do root; equivalentes verificados em `app/api/views.py:447-453`].

Aliases e leitores da derivação “disputa aberta”:

- Reexport compatível: `app/chamados/models.py:4234-4236`.
- Leitor REST: `app/api/views.py:1335-1336,1670-1672`.
- Leitor de autorização: `app/colaboradores/mixins.py:304-312`, só POST; isentos passam antes em `:301-303`. Decoradores ativos: `app/holerite/views.py:1144`; `app/colaboradores/views.py:1368,1603,1769,1877`. Decoradores isentos: `app/colaboradores/views.py:1410,1425`; `app/ferias/views.py:287,347,686,773`.
- Wrapper `motor_bloqueado_por_disputa`: `app/ponto/services/disputa.py:15-31`. Leitores: `app/ponto/views.py:140-144,207` → faixa em `app/templates/ponto/espelho.html:13-18`; PDF `app/relatorios/pdf_espelho.py:1082-1090,1113` → execução dos bloqueios em `app/relatorios/pdf_base.py:129-131`.
- Variável homônima do painel: `app/colaboradores/painel_cell.py:23` lê conjunto recebido ou `:31-32` calcula `exists(disputa aberta)`; `:68` transforma em LED vermelho. Único chamador de produção encontrado passa sem mapas em `:81`; template lê LED em `app/templates/colaboradores/partials/_ps_linha.html:1,6`.
- Fonte paralela do LED em lote: `app/colaboradores/services/situacional.py:117` monta conjunto de colaboradores com disputa aberta; `:56-59` lê e `:210` publica LED.
- `em_disputa_fechada` é outra variável: `app/chamados/services/fio.py:264-268`; consumidor admin em `app/templates/chamados/partials/modal_fio.html:289`; não equivale ao sinal de abertura da tela de perguntas.

Equivalentes de apresentação verificados:

- Autoridade `app/chamados/services/pergunta_no_app.py:88-119`: `questionarios_pendentes = len(pendentes)` em `:101`; `chamados_badge` prioriza pendentes em `:108-113`; sem perguntas usa chamados não lidos calculados em `:102-107`.
- Propagação `/me`: `app/api/views.py:315-319,359-360`; `app/api/views_core.py:303-307,347-348`.
- Dashboard web: `app/core/views.py:239,252-253,268`; leitores do link/badge/rótulo em `app/templates/core/dashboard_colaborador_v2.html:78-80`.
- Painel do colaborador: `app/colaboradores/views.py:940-941` → `app/templates/colaboradores/partials/painel_colaborador.html:39,190-191`.
- Listagem `/chamados/v2`: consulta em `app/api/views.py:997-998`; pendentes e histórico vivo de disputa aberta em `:1033-1035`; destino vivo ou pai em `:1036-1040`; serialização `perguntas_disputa` em `:1148-1167`.

Não localizei consumidor HTML/JS literal de `em_disputa` ou `motivos_disputa`; consumidor nativo precisa ser sustentado pelo HANDOFF ou marcado `[não verificado]`. Os únicos usos locais de `motivos_disputa` são os payloads citados e a prosa de `app/ponto/services/aviso_da_batida.py:14`; o serviço lê a lista interna para a frase em `:69-71`, chamado em `app/api/views.py:812` / `app/api/views_core.py:806`.

Impedimentos concretos para uma sonda histórica exata:

- `Batida` não guarda flag nem lista de motivos; campos em `app/ponto/models.py:8-95`.
- `detectar_anomalias(colaborador)` não aceita instante (`app/chamados/detector_anomalias.py:19`), lê escalas ativas atuais em `:58-62`, `timezone.now()` em `:73` e cadastro de posto atual em `:101-111`.
- A função **pode escrever e enviar push**: chama `bloquear_se_em_ferias` em `app/chamados/detector_anomalias.py:114-121`; este cria chamado em `app/ponto/utils.py:103-125` e envia push em `:127-133`. O detector engole o erro em `contexto['_erro_ferias']`, sem contador `engolidos`, em `detector_anomalias.py:120-121`. Interceptar escritor antes da tentativa é necessário se a sonda chamar esse caminho; barreira SQL somente leitura deve ser adicional.
- `ultima_batida` guarda episódio corrente em `app/api/views.py:761-765`, mas o agregado substitui outro episódio em `app/chamados/services/disputa_emissao.py:569-575`; não é histórico completo.
- Canal é dado persistido em `app/ponto/models.py:46-55`: `NULL` anterior a 27/10/2026 ou sem aparelho; o próprio contrato proíbe inferir canal por `timestamp_dispositivo` em `:48`.

## 15/11 23:25 · sessao 24e606ed885e · - · - · origem=Ronald (tmux)

Roteamento e contrato, somente pela cópia:

- `CLAUDE.md:84-89` descreve as cascas; `Caddyfile:8-10` confirma: `/api/ponto/*`, `/api/auth/*`, `/api/me/`, `/api/regularizacao/*`, `/api/ping-geo/`, `/api/registrar-fcm/`, `/health/` → core; restante → UI. O domínio Juliani importa esse bloco em `Caddyfile:32-38`.
- Core: `app/config/urls_core.py:5-6` inclui `api.urls_core`; `app/api/urls_core.py:5` importa `views_core`; `:13` aponta `/api/me/`, `:14` `/api/ponto/bater/`.
- UI: `app/config/urls.py:37` inclui `api.urls`; `app/api/urls.py:2` importa `views`; `:81` espelho/v2; `:92` disputa/perguntas; `:93` disputa/responder; `:73` chamados/v2. Seu gêmeo bater (`:67`) existe no URLconf UI, mas o Caddy encaminha essa URL ao core.
- Configuração aceita a escolha do URLconf por variável em `app/config/settings/saas.py:76-78`; definição real do ambiente/container rodando não foi inspecionada [não verificado].

Equivalentes de apresentação:

| Fonte e escritor | Leitores/serializadores |
|---|---|
| `consulta_do_app` retorna partição pendentes/histórico (`app/chamados/services/pergunta_no_app.py:64-85`). Pendente = C6 viva, `respondida_em is None`, disputa aberta (`:73`, `:80-85`). | `/api/disputa/perguntas/`: consulta em `app/api/views.py:2537-2539`; `em_disputa=bool(perguntas)` em `:2569`; `perguntas` em `:2576`; `respondidas` como subconjunto histórico em `:2577`; `historico` em `:2578`. |
| `projecao_do_app`: `questionarios_pendentes=len(pendentes)` (`app/chamados/services/pergunta_no_app.py:100-101`); badge prioriza pendentes com rótulo “Responder DP”, depois chamados não lidos, depois `None` (`:102-113`). | Core `/me`: recebe em `app/api/views_core.py:303-307`, transmite em `:346-348`; UI gêmeo `app/api/views.py:315-319`, `:358-360`. Android usando esses campos para abrir tela [não verificado]. |
| Dashboard conta a mesma lista (`app/core/views.py:252-253`) e transmite em `:268`. | `app/templates/core/dashboard_colaborador_v2.html:78-80`: decide sufixo do link `?aba=cham`, badge e rótulo “Responder DP”. |
| Projeção administrativa (`app/colaboradores/views.py:940-941`). | `app/templates/colaboradores/partials/painel_colaborador.html:39`, `:190-191`: total de mensagens e descrição “No app dele agora”. |
| Chamados/v2 reutiliza `consulta_do_app`, junta pendentes+histórico, conserva só disputas abertas e ancora em destino vivo ou pai (`app/api/views.py:996-1040`). | Serializa `perguntas_disputa` em `:1148-1168`; `pode_solicitar_correcao` usa `aguarda_o_colaborador` em `:1174-1180`. `pode_responder` em `:1145` diz chamado não terminal, portanto não é autorização individual de PerguntaDisputa. |

Outras chaves que devem ser distinguidas:

- `/me` mantém `regularizacao_obrigatoria=False` e, por isso, `entradas_pendentes=[]` (`app/api/views_core.py:314-341`).
- `/api/regularizacao/pendente/` lista **RegularizacaoExterna** com `status='aguardando'`, retornando `pendentes`; trata outro modelo/universo (`app/api/views_core.py:1271-1289`; rota `app/api/urls_core.py:15`).
- `aviso_texto_livre` também consulta as pendentes atuais e retorna orientação para abrir chamado/pergunta, sem novo predicado (`app/chamados/services/aviso_pendencia.py:7-20`).

Contrato e relato:

- `app/docs/HANDOFF.md:1-219` cobre login, ausência, plataforma, troca de senha; não contém contrato de `em_disputa` ou `/api/disputa/perguntas/` [não verificado como contrato existente].
- `app/api/views_core.py:426-427` e `app/api/views.py:440-441` mencionam em comentário o APK preso em `PerguntasDisputaActivity`. Isso documenta a intenção/comportamento relatado; não demonstra um leitor Android executável.
- Não foi localizado código Kotlin/Java/Swift/Dart na busca da cópia [não verificado]. Android 3.9.3, a condição executada para abrir a Activity e o texto mostrado no aparelho continuam [não verificado].
- O texto web “Nenhuma pendência” está condicionado a `not pendencias` em `app/templates/chamados/meu_atendimento.html:117-120`; não prova a tela nativa mencionada no relato.

## 15/11 23:25 · sessao 2f274523329a · - · - · origem=Ronald (tmux)

Candidatos distintos, sem escolher:

1. **Receber ou corrigir resposta pela porta:** `pergunta_viva(p, hoje=None, *, carga=None, agora=None)`, registrado como C6 em `app/core/juizes.py:112` e `app/core/juizes.py:286`; implementação/memo em `app/chamados/juizes.py:865`, predicado em `app/chamados/juizes.py:1402`. A propriedade `.viva` explicita “Pode receber (ou corrigir) resposta” e delega ao juiz (`app/chamados/models.py:3473-3484`). A porta POST efetivamente consulta C6 para a pergunta pertencente ao colaborador, sem exigir disputa aberta nem resposta ausente (`app/api/views.py:2609-2620`).

   Predicado em ordem: validação mata; tranca atual mata; vias terminais materializadas vencem resposta; resposta carimbada com texto mantém viva salvo decisão administrativa e concordância com luz; identidade materializada mata; marco materializado no espelho mata; célula acusadora vive, célula absolutória completa mata; falta na janela viva mantém; sem materialização vive; com materialização depende testemunha externa (`app/chamados/juizes.py:1403-1492`). **Universo:** uma pergunta recebida pelo juiz, podendo ser de conversa e podendo estar em disputa fechada — nenhuma guarda de fechamento consta no corpo (`app/chamados/juizes.py:1402-1492`).

2. **Pergunta pendente que a lista do APK oferece:** `consulta_do_app(colab, *, agora=None)` → `particao_do_app(perguntas_do_app(...))` (`app/chamados/services/pergunta_no_app.py:64-85`). `perguntas_do_app` consulta perguntas de todas as disputas daquele colaborador, pré-filtra pelo generoso `q_vivas`, ordena por id e confirma C6 (`app/chamados/services/pergunta_no_app.py:45-61`). `particao_do_app` considera pendente **C6 viva E `respondida_em is None` E `disputa.fechada_em is None`**; demais vivas vão ao histórico (`app/chamados/services/pergunta_no_app.py:76-85`). GET `/disputa/perguntas/` usa essa lista e escreve `em_disputa=bool(pendentes)` (`app/api/views.py:2537-2539`, `app/api/views.py:2566-2578`). **Diferença:** C6 admite correção de viva já respondida e viva sob disputa fechada; a lista pendente exclui ambas (`app/chamados/models.py:3475`, `app/api/views.py:2612-2620`, `app/chamados/services/pergunta_no_app.py:81-84`).

`fio_vivo(perguntas)` é a redução registrada C15: `any(pergunta_viva(p) for p in perguntas)`; universo é explicitamente do chamador, vazio dá falso; não acrescenta os filtros da lista pendente (`app/core/juizes.py:123-125`, `app/core/juizes.py:295`, `app/chamados/juizes.py:1507-1516`).

C19 `tarefa_do_dia` é outro domínio: ações exigíveis por **instância de marco de um colaborador-dia**, após decisões e conferência de efeitos, com estado `nao_sei`; não substitui a pergunta de alcance geral do questionário (`app/core/juizes.py:299`, `app/docs/JUIZES.md:78-93`, `app/chamados/services/tarefa_do_dia.py:1-26`). Seu `fio=colab` exige ação `responder` de colaborador **e pergunta pendente no app**, delegando a C6 + `particao_do_app` (`app/chamados/services/tarefa_do_dia.py:37-50`). C17 exige âncora na fila e pergunta sobre porta do dia/painel, não disponibilidade geral de responder (`app/chamados/juizes.py:1667-1696`, `app/chamados/juizes.py:1704-1713`).

**Tempo passado: aceita parâmetro, mas não replay integral.** `agora` chega ao C6 e muda o dia da janela (`app/chamados/juizes.py:875-890`); a projeção apenas oculta validação/resposta/materialização posteriores e conserva identidade atual (`app/chamados/juizes.py:1381-1399`). Permanecem:

- Pré-filtro `q_vivas` do estado atual, anterior ao C6 (`app/chamados/services/pergunta_no_app.py:52-61`).
- Partição lê `respondida_em` e `fechada_em` atuais, sem projeção (`app/chamados/services/pergunta_no_app.py:73-85`).
- Tranca não recebe `agora`; lê `periodo_trancado` atual e toques posteriores ao fechamento atual (`app/chamados/juizes.py:977-995`, `app/chamados/juizes.py:1065-1082`).
- C6 lê `celula.veredito/veredito_via` atuais (`app/chamados/juizes.py:1417-1419`, `app/chamados/juizes.py:1450-1466`).

Portanto rotular `consulta_do_app(colab, agora=batida.criado_em)` como “pergunta existente e respondível no instante da batida” seria incorreto pelas próprias leituras acima. A sonda pode entregar **aproximação explicitamente atual**: detector de anomalia da batida chamado agora × ausência de pendentes atuais do colaborador, e em paralelo ausência de C6 vivas atuais; resultado contemporâneo exato continua `[nao verificado]` sem reconstrução histórica integral. Se preferir projeção parcial, rotulá-la literalmente como tal e conservar os dois resultados separados (`app/chamados/services/pergunta_no_app.py:45-85`, `app/chamados/juizes.py:1381-1399`).

Para sonda sem escrita: leitores do app retornam listas sem `.save()` (`app/chamados/services/pergunta_no_app.py:45-85`); C6 apenas memoiza na instância (`app/chamados/juizes.py:893-905`), embora erros possam ser engolidos nos degraus inferiores (`app/chamados/juizes.py:1476-1492`). `engolidos()` fornece fotografia cumulativa, não zerar (`app/core/observ.py:60-70`); `registrar_engolido` incrementa contador e emite logging (`app/core/observ.py:73-82`). Assim comparar contador antes/depois da função e desabilitar logging como no molde; exceção ou incremento torna classificação indecidível (`app/core/observ.py:60-82`).

O helper terceira derivação é só existência de disputa aberta, sem perguntas: `colaborador_em_disputa(colaborador)` retorna primeira `DisputaSupervisao` com `fechada_em__isnull=True` (`app/chamados/services/disputa_emissao.py:34-42`).

## 15/11 23:18 · sessao bcbcdfa003d8 · E4C-F1NOVA-B1 · verificador · origem=Code

**Persistem dois problemas: cache desatualizado na guarda do `save` e falsa absolvição no contrato AST.** (`app/chamados/models.py:3588`; `app/chamados/tests/test_contract_par_resposta.py:92`)

Identidade independente do HEAD, comparação Git e execução com Django/banco: **[não verificado]**. Os contraexemplos abaixo decorrem da cópia; os helpers foram executados isoladamente em memória, sem banco. (`app/chamados/models.py:3592`; `app/chamados/tests/test_contract_par_resposta.py:101`)

**1. Fechamento dos achados procedentes da volta 1**

| Achado | Veredito sobre o fechamento |
|---|---|
| **1 — par persistido** | **NÃO PROCEDE o fechamento integral.** Fecha o exemplo original: a metade excluída de `update_fields` passa a vir de `_par_do_banco`. Entretanto, esse método pode devolver cache desatualizado; contraexemplos no item 2. (`app/chamados/models.py:3601`; `app/chamados/models.py:3602`; `app/chamados/models.py:3588`) |
| **4 — guarda do contrato** | **NÃO PROCEDE o fechamento integral.** Os dois exemplos antigos agora são acusados, mas basta um `if` com saída antecipada em linha anterior, mesmo num caminho que não será executado. (`app/chamados/tests/test_contract_par_resposta.py:156`; `app/chamados/tests/test_contract_par_resposta.py:157`; `app/chamados/tests/test_contract_par_resposta.py:92`) |
| **5 — fixture mista** | **PROCEDE.** O teste adicional recria ambas com texto, retira os instantes por `update` e verifica uma materialização, nenhuma resolução adicional, nenhum fechamento e disputa aberta. Identidade histórica dos asserts com `e84a167f4`: **[não verificado]**. (`app/chamados/tests/test_fio_disputa_resolvido.py:62`; `app/chamados/tests/test_fio_disputa_resolvido.py:100`; `app/chamados/tests/test_fio_disputa_resolvido.py:103`; `app/chamados/tests/test_fio_disputa_resolvido.py:105`; `app/chamados/tests/test_fio_disputa_resolvido.py:108`) |
| **6 — JUIZES.md** | **PROCEDE.** A tabela registra o juiz, sua face SQL e o escopo B1/B2/E-F, acompanhando o registro vivo. (`app/docs/JUIZES.md:99`; `app/docs/JUIZES.md:103`; `app/core/juizes.py:306`) |
| **7 — instante automático** | **PROCEDE.** O comentário agora diz PROCESSAMENTO, conforme o relógio usado pelo chamador. (`app/chamados/services/disputa_emissao.py:1671`; `app/api/foto_recebida.py:42`; `app/api/foto_recebida.py:48`) |

**2. Sequências que quebram a garantia — PROCEDE**

**a) Duas instâncias, sem concorrência nem escrita crua.** Usando `_cria` da fixture existente e `I` como instante válido: (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:58`)

```python
p = self._cria(respondida_em=I, resposta_colab='bati')
q = PD.objects.get(pk=p.pk)

q.respondida_em, q.resposta_colab = None, ''
q.save(update_fields=['respondida_em', 'resposta_colab'])

p.save(update_fields=['respondida_em'])
```

Após salvar `q`, o banco contém `(None, '')`, mas o cache de `p` continua `(I, 'bati')`. No último save, `_par_gravado` calcula `(I, 'bati')`; a comparação com o mesmo cache dispensa a recusa. Só o instante é escrito: o banco fica **`(I, '')`, ilegal**. (`app/chamados/models.py:3554`; `app/chamados/models.py:3588`; `app/chamados/models.py:3601`; `app/chamados/models.py:3608`)

**b) Rollback externo seguido de save repetido.** Partindo de `(None, '')`, salvar `(I, 'bati')` dentro de um `atomic` externo atualiza `_par_carregado`; se esse `atomic` depois falhar, o atributo não é restaurado. Repetir `save(update_fields=['respondida_em'])` produz a mesma divergência do exemplo anterior. O `atomic` interno não adia a atualização do cache até o commit externo. (`app/chamados/models.py:3552`; `app/chamados/models.py:3554`; `app/chamados/models.py:3608`)

**c) Legado ilegal intocado passa a ser recusado.** A fixture `_legado` fornece o estado inicial: (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:62`)

```python
p = self._legado(resposta_colab='legado')  # banco: (None, 'legado')

try:
    with transaction.atomic():
        p.respondida_em = I
        p.save()                        # cache: (I, 'legado')
        raise RuntimeError()
except RuntimeError:
    pass

p.respondida_em = None                  # memória igual ao banco novamente
p.pergunta = 'outro enunciado'
p.save()
```

O último save compara `(None, 'legado')` com o cache `(I, 'legado')`, considera uma transição e levanta `TextoSemCarimbo`, embora o par gravado permaneça intocado. Isso viola a decisão (1) expressa no teste. (`app/chamados/models.py:3608`; `app/chamados/models.py:3611`; `app/chamados/services/fio.py:1610`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:5`)

O refresh integral recompõe o cache a partir dos campos carregados; refresh parcial que toca o par o torna desconhecido. Essas medidas cobrem o caso diferido acrescentado no RED, mas não invalidam outras instâncias nem revertem o cache após rollback externo. (`app/chamados/models.py:3573`; `app/chamados/models.py:3577`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:137`; `app/chamados/models.py:3554`)

**3. `_par_carregado` — NÃO PROCEDE que represente sempre “banco agora”**

| Acesso | Local |
|---|---|
| Escreve após save | `app/chamados/models.py:3554` |
| Escreve no `from_db` | `app/chamados/models.py:3566` |
| Escreve após refresh integral | `app/chamados/models.py:3573` |
| Escreve `None` após refresh parcial do par | `app/chamados/models.py:3577` |
| Lê em `_par_do_banco` | `app/chamados/models.py:3587` |
| Lê em `_par_gravado`, quando nenhum campo do par será salvo | `app/chamados/models.py:3600` |

**NÃO PROCEDE que algum desses leitores interprete `None` como par vazio.** `_par_do_banco` trata como desconhecido e consulta a linha existente; `_par_gravado` apenas propaga o desconhecido no caminho que não escreve o par. Porém, valor conhecido significa efetivamente uma fotografia carregada, não necessariamente o banco atual. (`app/chamados/models.py:3588`; `app/chamados/models.py:3590`; `app/chamados/models.py:3600`)

**4. Custo — PROCEDE, condicionado ao estado do cache**

Contagem de consultas feitas especificamente por `_par_do_banco`, para instância existente: (`app/chamados/models.py:3585`)

| Estado e save | SELECTs |
|---|---:|
| Cache conhecido | **0** — retorna o cache. (`app/chamados/models.py:3588`) |
| Cache desconhecido; `update_fields` sem o par | **0** — ambos os caminhos dispensam consulta. (`app/chamados/models.py:3600`; `app/chamados/models.py:3606`) |
| Cache desconhecido; ambas as metades em `update_fields`, ou save sem `update_fields` | **1** — comparação da recusa. (`app/chamados/models.py:3608`) |
| Cache desconhecido; somente uma metade em `update_fields` | **2** — composição e comparação; a primeira leitura não é memorizada. (`app/chamados/models.py:3601`; `app/chamados/models.py:3608`; `app/chamados/models.py:3590`) |

Sem `update_fields`, carregar atributos ainda diferidos pode acrescentar consultas próprias antes da comparação. A contagem real com Django/banco é **[não verificado]**. (`app/chamados/models.py:3597`; `app/chamados/models.py:3577`)

Nos fluxos de produção localizados após refresh parcial:

- `devolver_validacao_recusada` atualiza parcialmente `resposta_colab`, invalidando o cache, mas salva só validação: **zero SELECTs do par**. (`app/chamados/services/validacao.py:213`; `app/chamados/services/validacao.py:227`; `app/chamados/models.py:3606`)
- `_carimbar_recusa` atualiza parcialmente campos fora do par e salva recusa ou carimbo de identidade: **zero SELECTs do par**. (`app/chamados/services/materializacao.py:1095`; `app/chamados/services/materializacao.py:1105`; `app/chamados/services/materializacao.py:1109`; `app/chamados/models.py:3291`)
- `_aceitar_a_folga` pode repetir a operação após refresh dos campos de validação; as escritas correspondentes excluem o par. (`app/chamados/services/validacao.py:370`; `app/chamados/services/validacao.py:374`; `app/chamados/services/validacao.py:162`; `app/chamados/models.py:3291`)

Não localizei fluxo `only/defer → save` que efetivamente introduza essas consultas. Os `only` relevantes alimentam atos, contadores ou caminhos DRY; frequência real em produção e completude contra chamadas dinâmicas: **[não verificado]**. (`app/chamados/models.py:1613`; `app/chamados/services/fio.py:1235`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:60`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`)

**5. Contrato — PROCEDE falsa absolvição; NÃO PROCEDE acusar os dois escritores citados**

Este contraexemplo é absolvido pelo helper real: (`app/chamados/tests/test_contract_par_resposta.py:92`; `app/chamados/tests/test_contract_par_resposta.py:108`)

```python
def f(p, qs, habilitado):
    if habilitado:
        if not p.resposta_colab:
            return
    qs.update(respondida_em=agora)
```

Com `habilitado=False` e a pergunta vazia, a guarda nem executa. A absolvição ocorre porque o `return` está numa linha anterior à escrita, sem comprovação do caminho de controle. **É uma falha adicional à polaridade expressamente excluída do selo.** A leitura da outra metade também não exige identidade entre as instâncias. (`app/chamados/tests/test_contract_par_resposta.py:92`; `app/chamados/tests/test_contract_par_resposta.py:22`; `app/chamados/tests/test_contract_par_resposta.py:76`)

As duas absolvições de produção examinadas são corretas pela forma:

- `PerguntaDisputa.carimbar` escreve o instante dentro da guarda de texto após trim e inclui o campo no save. (`app/chamados/models.py:3231`; `app/chamados/models.py:3233`; `app/chamados/models.py:3234`; `app/chamados/models.py:3253`)
- `carimbar_resposta_pela_trilha` retorna antes da escrita quando já existe instante ou falta texto após trim; depois salva o instante. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1698`)

**6. Descartes 2 e 3 — PROCEDEM sob as classificações declaradas na triagem; conferência nas fontes originais [não verificado]**

- **2, I21:** a regra documentada permite à classe A nomear o juiz no B1; o código reconhece explicitamente a mudança de predicado para trim. A classificação específica de I21 no censo, a exceção no predesenho e “brancos = 0” constam da triagem fornecida, mas sua confirmação nos documentos originais é **[não verificado]**. (`app/docs/JUIZES.md:102`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`)
- **3, espera pelo texto:** a regra documentada exclui E/F da migração, portanto o descarte é coerente **se** a função é classe E, como informa a triagem. Essa classificação nominal no censo é **[não verificado]**. O complemento continua no código; sua existência, isoladamente, não demonstra violação desse escopo. (`app/docs/JUIZES.md:103`; `app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1552`)

## 15/11 23:16 · sessao d9a18c11cd9b · - · - · origem=Ronald (tmux)

Item 3:

- Escrevem `_par_carregado`: `save` (`app/chamados/models.py:3554`), `from_db` (`:3566`), `refresh_from_db` integral (`:3573`) e parcial que toca o par (`:3577`).
- Leem diretamente: `_par_do_banco` (`:3587`) e `_par_gravado` quando nenhum campo do par vai ao UPDATE (`:3600`).
- `None` significa desconhecido: `_par_do_banco` consulta a linha existente (`:3588-3590`); `_par_gravado` apenas o propaga no caminho sem escrita do par (`:3599-3600`). Nenhum desses leitores transforma `None` em `(None, '')`.
- O significado “banco agora” depende de o cache continuar válido; `save` o atualiza antes de eventual rollback de transação externa, sem mecanismo de reversão do atributo (`:3552-3554`).

Item 4:

| Estado/caminho | SELECTs de `_par_do_banco` |
|---|---|
| Cache conhecido | 0 (`app/chamados/models.py:3587-3589`) |
| Cache desconhecido, `update_fields` sem par | 0 (`:3599-3600`, `:3606-3607`) |
| Cache desconhecido, ambas as metades ou `save()` | 1 na comparação (`:3608`, `:3590`) |
| Cache desconhecido, somente uma metade | **2**, pois há chamadas em `:3601` e `:3608`, sem memorizar o resultado em `:3590` |

Sem `update_fields`, o acesso às duas metades acontece antes da comparação (`app/chamados/models.py:3596-3597`); cargas diferidas podem somar consultas próprias, além do SELECT acima. Execução com Django/banco: [não verificado].

Não identifiquei chamador de produção atual que efetivamente acrescente essas consultas:

- `only` com **ambas** as metades: ressuscitação usa instâncias apenas para construir atos (`app/chamados/models.py:1612-1613`); DRY do backfill também inclui ambas (`app/chamados/management/commands/backfill_decisoes_pergunta.py:60`).
- `only` **sem ambas**: contador de destino (`app/chamados/services/fio.py:1235-1244`) e DRY de colisões (`app/chamados/management/commands/backfill_decisoes_pergunta.py:504-510`); não salvam essas instâncias nesses fluxos.
- `devolver_validacao_recusada` invalida o cache ao atualizar só `resposta_colab` do par (`app/chamados/services/validacao.py:213-214`), mas salva apenas validação (`:227`): **0** consultas do par. Depois, o materializador usa novas instâncias de um queryset (`:494`; `app/chamados/services/materializacao.py:1375-1382`).
- `_carimbar_recusa` faz refresh parcial sem o par (`app/chamados/services/materializacao.py:1095`) e salva recusa (`:1109`) ou carimbo de identidade (`:1105`; `app/chamados/models.py:3287-3291`): **0** consultas do par.
- `_aceitar_a_folga` faz refresh de validação (`app/chamados/services/validacao.py:370`) e pode repetir a operação (`:374`); saves seguem sem o par (`:162`; `app/chamados/models.py:3165`, `:3291`).
- Outros refreshs parciais terminam em leitura/retorno: validação (`app/chamados/services/validacao.py:497`) e classificação do passivo (`app/chamados/services/passivo_validado.py:140-145`).

Quantidade/frequência real de consultas em produção: [não verificado].

## 15/11 23:15 · sessao a574bcd3a4b2 · - · - · origem=Ronald (tmux)

**Achado 4: PROCEDE; o delta fecha os exemplos antigos, mas não fecha a exigência “guarda governa a escrita”.** Os novos mutantes cobrem `if` solto e escrita anterior à guarda; `_governada` ainda absolve qualquer guarda com `return`/`raise` cuja linha preceda a escrita, sem verificar bloco, caminho de execução ou escopo. (`app/chamados/tests/test_contract_par_resposta.py:156`; `app/chamados/tests/test_contract_par_resposta.py:157`; `app/chamados/tests/test_contract_par_resposta.py:86`; `app/chamados/tests/test_contract_par_resposta.py:92`)

Contraexemplo absolvido pelo helper real (`meia_par` retorna `None`), executado em memória com `python3 -B`:

```python
def f(p, qs, habilitado):
    if habilitado:
        if not p.resposta_colab:
            return
    qs.update(respondida_em=agora)
```

Com `habilitado=False`, `p` e `qs` apontando para uma pergunta `(None, '')`, grava `(agora, '')`. A guarda não foi executada; a absolvição decorre exclusivamente de `_sai_cedo(g)` e `g.lineno < escrita.lineno`. Não é o limite declarado de polaridade. (`app/chamados/tests/test_contract_par_resposta.py:79`; `app/chamados/tests/test_contract_par_resposta.py:92`; `app/chamados/tests/test_contract_par_resposta.py:22`)

Também são absolvidos: guarda numa função interna nunca chamada; guarda que lê `q.resposta_colab` enquanto escreve `p.respondida_em`. A busca usa `ast.walk(fn)` e `_le` compara somente o nome do atributo. (`app/chamados/tests/test_contract_par_resposta.py:75`; `app/chamados/tests/test_contract_par_resposta.py:86`)

**Item 5: PROCEDE quanto a falsos negativos; NÃO PROCEDE quanto a acusação dos dois escritores legítimos citados.** A varredura real extraída por AST retornou `[]`; ambos foram absolvidos. A implementação da varredura está em `app/chamados/tests/test_contract_par_resposta.py:113`; evidência persistida dessa execução: **[não verificado]**.

- `PerguntaDisputa.carimbar`: só atribui instante sob guarda de texto não vazio após trim; inclui `respondida_em` nos campos salvos. A absolvição é correta. (`app/chamados/models.py:3231`; `app/chamados/models.py:3233`; `app/chamados/models.py:3234`; `app/chamados/models.py:3253`)
- `carimbar_resposta_pela_trilha`: retorna antes da escrita se já tem instante ou não tem texto após trim; depois atribui e salva somente o instante. A absolvição é correta. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1693`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1698`)

## 15/11 23:15 · sessao ff3804764101 · - · - · origem=Ronald (tmux)

Achados 5, 6 e 7: **fechados pelo delta**.

- **5:** o teste novo recria duas perguntas com texto pelo helper, que fornece texto e instante, e depois zera os instantes de ambas por `update`; conserva os asserts de uma materialização, zero novas resoluções, zero fechamentos e disputa aberta. (`app/chamados/tests/test_fio_disputa_resolvido.py:56`, `:62`, `:94`, `:100`, `:103`, `:105`, `:106`, `:108`). Igualdade com os asserts históricos de `e84a167f4`: **[não verificado]**, pois não há Git na cópia.
- **6:** a tabela agora registra `pergunta_respondida`, a face SQL, a recusa simétrica e o escopo B1/B2/E-F; corresponde ao registro `_C21`. (`app/docs/JUIZES.md:99`, `:100`, `:101`, `:102`, `:103`; `app/core/juizes.py:306`).
- **7:** o comentário agora diz instante do **PROCESSAMENTO**, conferindo com `agora = _tz.now()` passado pelo chamador. (`app/chamados/services/disputa_emissao.py:1671`; `app/api/foto_recebida.py:42`, `:48`).

Descartes 2 e 3: **conferência documental específica [não verificado]**. Os arquivos `logs/JUIZ-RESPONDIDA/censo.md`, predesenho e aval citados não foram encontrados na cópia; portanto não é possível confirmar independentemente “I21 = linha 29/classe A”, “espera = linha 87/classe E”, a exceção “I21 sem trim”, ou a medição de brancos zero. **[não verificado]**

O que a cópia confirma:

- I21 usa a face SQL com trim, e o comentário reconhece expressamente a diferença em relação ao predicado anterior. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`, `:1573`, `:1575`).
- `espera_o_texto_do_colaborador` continua calculando o complemento do juiz por instante ausente ou instante sem texto. (`app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1534`, `:1552`).
- A regra geral registrada para B1 é migrar classe A, postergar B/C/D e conservar E/F; isso corrobora a lógica dos descartes se as classificações nominais da triagem estiverem corretas, mas não prova essas classificações. (`app/docs/JUIZES.md:102`, `:103`; `app/chamados/juizes.py:1550`, `:1551`).

## 15/11 23:10 · sessao 8edb157097ed · BUG-PAR-O-PAR · leitura · origem=Code

BRIEF — BUG-PAR-O-PAR, volta para `logs/BUG-PAR-O-PAR/volta/brief.md`. **Condição de construção não fecha: há uma pergunta de lei no bloco (d).** O aval exige zero perguntas para construir; revisão do aval, cruzamento atual e vaga livre continuam condições independentes. (`prepara_volta_bug_par.txt:2-4`.)

A base é a cópia fornecida como HEAD de main; sua identidade Git não pôde ser certificada nesta árvore sem `.git`: **[nao verificado]**. Referências com prefixo `6e2fc9133:` pertencem exclusivamente à árvore antiga, conforme a entrega; não são linhas do main atual. (`logs/BUG-PAR-O-PAR/predesenho/entrega.md:5`.)

**(a) UNIVERSO**

A fonte fechada são **as duas listas integrais de `u163.py:1-2`**, medidas em 13/11. Conferência em memória dessas listas: U60 tem 60 elementos distintos; U103 tem 103 elementos distintos; interseção vazia; união com 163 PKs. Nenhuma consulta a `recusa_motivo` participa dessa identificação. (`u163.py:1-2`; `cortes_lote7_e_bug_par.txt:1`, item 2.)

Contrato de arquivo: criar `app/chamados/data/bug_par_163_2026_11_13.json` contendo exatamente os arrays do anexo, nesta ordem de chaves: `U60`, `U103`; conservar a ordem dos elementos; JSON compacto, sem espaços, ASCII, uma linha e LF final. São **986 bytes**, com SHA-256 calculado sobre essa serialização:

```text
0486b00590a46fd9e08cf664deaed66f223b80e753c480a100ce59e4e9431669
```

Esse hash identifica a serialização especificada das listas recebidas, não um arquivo já construído. (`u163.py:1-2`; destino e leitor novos: **[nao verificado]**.)

Contrato de leitor: `app/chamados/services/universo_bug_par.py::_carregar_universo_bug_par` carrega esse único arquivo, confere hash, chaves, inteiros, cardinalidades e ausência de repetição antes de devolver conjuntos imutáveis U60, U103 e U163. `no_universo_163(p)` consulta exclusivamente `p.pk`; o comando dos 103 consome U103 desse mesmo carregamento. Falha de integridade interrompe a operação; não reconstrói a lista pelo banco. São nomes propostos, ainda sem implementação. (`cortes_lote7_e_bug_par.txt:1`, item 2; implementação: **[nao verificado]**.)

F1* ∪ F2 fica somente como conferência histórica: a entrega registra igualdade PK a PK, mas também documenta que F1 cresce com novos cliques, F1* depende de notas idempotentes e F2 muda depois da devolução. Nenhum desses predicados determina pertencimento na volta. (`logs/BUG-PAR-O-PAR/predesenho/entrega.md:10-45`.)

RED adicional do universo: conservar os mesmos 163 depois de nova recusa de paridade, mudança de motivo, alteração da lâmpada e devolução dos 103; matar os mutantes “selecionar pela recusa”, “incluir nova nota PARIDADE”, “recalcular F2” e “aceitar arquivo adulterado”. (`logs/BUG-PAR-O-PAR/predesenho/entrega.md:28-45`; testes propostos: **[nao verificado]**.)

**(b) TESTES** [nao verificado]

Cada linha abaixo corresponde a uma linha da tabela fornecida. As fixtures de plantio fora163 devem declarar pertencimento **falso** e competência aberta; os casos dentro163 devem declarar pertencimento **verdadeiro**, sem simular isso escrevendo `recusa_motivo`. (`logs/BUG-PAR-O-PAR/predesenho/entrega.md:48-60`; contrato de fixture: **[nao verificado]**.)

| Linha da tabela antiga | RED exigido na volta | Sítio mordido e mutante a matar |
|---|---|---|
| `test_bug_par_projetado.py:1107` — resposta nova divergente | Fora163, resposta S 01:20 para marco S 01:00: testemunha retorna `aplicavel`; a escrita segue o marco. | Portão DXG e testemunha em `materializacao.py`. Mata retirar a condição de pertencimento e voltar à espera sobre toda a frota. |
| `:1117` — `_as_duas_ordens` | Criar um teste que exercite o helper: validações e repetição deixam exatamente S 01:00/E 02:00, sem duplicar efeitos. | Validação/materialização do par. Mata retornar sem escrever, gravar uma só metade ou duplicar no retry. | [nao verificado]
| `:1130` — saída primeiro | Fora163, validar saída antes da volta grava o par esperado. | Orquestração do par. Mata conservar a espera geral nessa ordem. |
| `:1135` — volta primeiro | Fora163, ordem inversa produz os mesmos tipos e instantes, com repetição idempotente. | Mesmo escritor do par. Mata assimetria por ordem de clique e gravação parcial. |
| `:1140` — testemunha indisponível no modal | Conservar a expectativa: falha/ausência da testemunha impede oferecer Validar, inclusive “assim mesmo”. | Alimentação da testemunha e os dois templates. Mata tratar falha como autorização ou deixar um formulário alternativo escapar. |
| `test_s139.py:26` | Conservar o teste atual do main: declaração 02:00 produz E 03:00, no marco. | `_ts_tipo_do_plantio`, atualmente `app/chamados/services/materializacao.py:409-419`. Mata usar a hora digitada e reintroduzir espera fora163. |
| `test_s139.py:49` — comentário/assert antigo | Caso separado na fixture S139: existe batida E 03:00, inexiste E 02:00 e não retorna mensagem DXG fora163. | Resultado da escrita e mensagem. Mata conservar o assert invertido ou comunicar espera depois de plantar. |

Fonte das sete linhas e expectativas: `logs/BUG-PAR-O-PAR/predesenho/entrega.md:52-60`. Os mutantes acima são contrato proposto; execução e mortes: **[nao verificado]**. A expectativa S139 já está no main em `app/chamados/tests/test_s139.py:26` e `:49-56`.

Os dois locais de apresentação precisam ser exercitados: a alimentação atual captura exceção e deixa `estado_aplicacao=None`; os formulários dos cartões oferecem validação por outros condicionais. O teste precisa alcançar o HTML resultante dos dois locais. (`app/chamados/services/fio.py:373-376`; `app/templates/chamados/partials/_pergunta_card.html:42-73`; `:92-98`; `app/templates/chamados/partials/modal_fio.html:230-246`.)

**(c) `bug_par_103 --apply`**

O comando documentado pertence à árvore antiga: `6e2fc9133:app/chamados/management/commands/bug_par_103.py:51-54` apenas conta exportadas, enquanto `:83-91` percorre todas as selecionadas. A devolução antiga descarimba e desvalida sem perguntar pela competência. (`logs/BUG-PAR-O-PAR/predesenho/entrega.md:89-106`.) Implementação desse comando no main fornecido: **[nao verificado]**.

O juiz exato é **`folha/export.py::competencia_entregue`**, registrado como `_K6` em `app/core/juizes.py:337`, implementado em `app/folha/export.py:556`. Seu resultado vem da exportação vigente não invalidada; não é `passivo_validado._esta_exportada` nem `ponto/janelas.py::competencia_fechada`. (`app/folha/export.py:588-594`; `app/chamados/services/passivo_validado.py:116-127`.)

Contrato da volta, nos arquivos do comando e de `validacao.py`:

1. Selecionar candidatos exclusivamente por U103 congelada; calcular competência pelo dia do fato e pelo juiz da janela, preservando a proteção contra deslocamento overnight. (`u163.py:2`; `app/chamados/services/passivo_validado.py:86-113`.)
2. O comando chama a porta de devolução; a porta pergunta ao K6 **antes de qualquer descarimbo, desvalidação ou ato DEVOLUÇÃO**. Uma classificação anterior usada pelo relatório não substitui essa pergunta na porta. (`cortes_lote7_e_bug_par.txt:1`, item 3; ordem proposta: **[nao verificado]**.)
3. K6 verdadeiro: recusar a devolução, conservar validação/carimbo e trilha existentes, e incluir a linha na saída para Pauta DP. K6 falso: permitir a devolução já autorizada, sem reabrir automaticamente a disputa. (`cortes_lote7_e_bug_par.txt:1`, item 3; `logs/BUG-PAR-O-PAR/predesenho/entrega.md:89-106`.)
4. No retrato fornecido, isso separa **84 entregues recusadas e 19 abertas elegíveis**. Esses números são expectativa daquele retrato; o comando deve perguntar novamente ao juiz no ato futuro. (`logs/BUG-PAR-O-PAR/predesenho/entrega.md:94-101`.)

A publicação deve conter `(pk, colNNN, competência)` das recusadas. O anexo fornece os 84 PKs e totais por competência, mas não todas as correspondências individuais com `colNNN`; completar esses tuples agora seria **[nao verificado]**. (`cortes_lote7_e_bug_par.txt:1`, item 3; `logs/BUG-PAR-O-PAR/predesenho/entrega.md:94-101`.)

REDs: K6 verdadeiro mantém integralmente pergunta e atos; K6 falso permite devolução; mudança da resposta do K6 entre classificação e entrada na porta recusa a operação; repetição não duplica DEVOLUÇÃO; chamada direta da porta também protege entregue. Matar “só contar exportada”, “guardar apenas no command” e “usar TXT paralelo”. Nenhum apply integra esta leitura ou a construção. (`prepara_volta_bug_par.txt:6`; testes propostos: **[nao verificado]**.) [nao verificado]

**(d) METADES FORA DOS 163 EM COMPETÊNCIA EXPORTADA**

**Não há garantia por juiz existente de que nenhuma delas plante depois da volta.** O dado fornecido identifica 247 metades fora163, sendo 75 exportadas: admin 10, laço 7 e passivo 58. A entrega chama isso de saída da espera e retorno ao caminho de plantio; não prova 75 gravações efetivamente realizadas. (`logs/BUG-PAR-O-PAR/predesenho/entrega.md:108-130`.)

Os caminhos atuais são:

| Entrada | Caminho até o escritor |
|---|---|
| Admin unitário | `app/chamados/services/acoes_disputa.py:13-22` → `validacao.py:398` → `_materializar_uma` em `:486-488`. |
| Lote classe A | `app/chamados/services/validacao.py:730`, `:747`, `:781` → mesma validação unitária. |
| Laço | `app/chamados/services/materializacao.py:1375-1383` → `_materializar_uma` → ramo hora em `:1227-1228`. Outros chamadores: `fio.py:708`, `acoes_disputa.py:139`, `backfill_fio_morto_e4b.py:147`. |
| Passivo | `app/chamados/services/passivo_validado.py:433-435` → `_plantar_o_dia` → mesma unidade em `:522`; o ramo exportado de regeneração vem depois, em `:442-449`. |
| Irmã do par | Na árvore antiga: `_gravar_irma_do_par`, chamada dentro da escrita da outra metade. Caminho documentado em `logs/BUG-PAR-O-PAR/predesenho/entrega.md:119-122`; implementação reaplicada no HEAD: **[nao verificado]**. |

O ramo hora chama `registrar_batida` com `fato_de_chao=False`; essa porta pergunta a **F6 `competencia_fechada`**. F6 consulta `FechamentoMensal.status` aprovado/exportado, enquanto K6 consulta `ExportacaoDominio` vigente não invalidada. O código não estabelece que toda competência entregue pelo K6 será barrada por F6. (`app/chamados/services/materializacao.py:932-940`; `app/ponto/registro_batida.py:132-137`; `app/ponto/janelas.py:84-89`; `app/folha/export.py:588-594`.)

Também há diferença de dia: F6 deriva competência da data local do timestamp; o passivo documenta por que o dia correto do fato pode ser anterior no overnight. A própria unidade declara ausência de portão de competência na porta administrativa por O44. Portanto “todos passam por `registrar_batida`” não fecha o proibido do aval. (`app/ponto/janelas.py:84-85`; `app/chamados/services/passivo_validado.py:98-113`; `app/chamados/services/materializacao.py:1172-1174`; `:1221-1223`.)

A decisão necessária está formulada no fechamento. Até ela, o brief não autoriza substituir F6, acrescentar bloqueio K6 à materialização nem liberar essas 75. (`prepara_volta_bug_par.txt:2`, item d; `:6`.)

**(e) ACHADOS DO OPUS E DA C4VOLTA**

Os quatro achados ALTA/MÉDIA estão registrados em `app/docs/RELATO.md:134`; a devolução da revisão C4 está preservada em `logs/BUG-PAR-O-PAR/c4volta/revisao.r:1` e resumida em `app/docs/RELATO.md:64`.

| Achado | Tratamento da volta e arquivo |
|---|---|
| **ALTA — alcance DXG** | `materializacao.py` passa a consultar o universo congelado antes da espera, usando a mesma decisão na testemunha e na escrita. `universo_bug_par.py`, arquivo de dados e testes selam pertencimento. A restauração fora163 continua condicionada à lei do bloco (d). (`cortes_lote7_e_bug_par.txt:1`, item 2; `prepara_volta_bug_par.txt:2`.) | [nao verificado]
| **ALTA — C7** | Não alterar `pode_fechar`, `sem_veredito_entre`, S127 ou `transicionar` nesta volta: a decisão posterior mantém o predicado e atribui a classe à RAIZ-DA-DISPUTA F1. Registrar essa limitação em `app/docs/RELATO.md`. (`app/docs/RELATO.md:80`; `app/chamados/models.py:1905-1917`.) |
| **C7 — alegação documental falsa** | A antiga `RELATO:262` está hoje em `app/docs/RELATO.md:602`, já acompanhada da correção. Conservar o histórico e registrar a volta com a afirmação correta: C7 não consulta `estado_aplicacao_pergunta`. (`app/docs/RELATO.md:602`; `app/chamados/models.py:1915-1917`.) |
| **MÉDIA — “JUIZO UNICO” em `par_do_intervalo`** | Na reaplicação em `materializacao.py`, retirar a alegação de autoridade registrada. O helper deve permanecer composição das autoridades existentes, sem ganhar decisão autônoma; juiz novo está proibido. A implementação reaplicada precisa ser conferida, não certificada pela docstring. (`app/docs/RELATO.md:134`; `app/docs/JUIZES.md:10-14`; `prepara_volta_bug_par.txt:6`; implementação: **[nao verificado]**.) |
| **MÉDIA — padrão fora do glossário** | Corrigir a declaração para `Single Writer Principle`, conforme bloco (f), nos registros da volta em `app/docs/RELATO.md`. (`app/docs/GLOSSARIO.md:12`; `app/docs/RELATO.md:134`.) |
| **C4 — selecionador lia recusa e lista integral faltava** | Substituir a origem do universo pelo arquivo/hash do bloco (a). A lista integral agora fornecida permite conferir 60+103; não aproveitar o selecionador antigo. (`app/docs/RELATO.md:64`; `u163.py:1-2`.) |
| **C4 — REDs incompatíveis com fora163** | Reconciliar as sete linhas explicitamente pelo bloco (b); não prometer conservar verdes as expectativas invertidas da cópia antiga. (`app/docs/RELATO.md:64`; `logs/BUG-PAR-O-PAR/predesenho/entrega.md:52-60`.) |
| **C4 — C2 e produtor de “nova versus legado”** | C2 foi retirado; não reutilizar sua condição como prova fechada. I18 mede validação sem carimbo e sem motivo, não batida ausente nem nova/legado. Qualquer medição posterior desses estados exige contrato próprio; nesta entrega, sua execução é **[nao verificado]**. (`app/docs/RELATO.md:77`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1074-1076`.) |
| **C4 — cruzamento, base e suíte** | Usar o PREPARA vigente para a abertura; não transportar autorização do C4 retirado nem resultados de testes antigos para a árvore reaplicada. Cruzamento atual F1 B1/R3 c17 e certificação da árvore resultante: **[nao verificado]**. (`app/docs/RELATO.md:64-65`; `prepara_volta_bug_par.txt:3-6`.) |
| **C4 — exportadas no apply e na escrita futura** | Fechar mecanicamente a devolução dos 103 pelo K6, conforme (c); conservar aberta a pergunta de lei sobre escrita fora163, conforme (d). Proibir apply durante a construção não resolve o efeito posterior do código. (`logs/BUG-PAR-O-PAR/predesenho/entrega.md:89-130`; `prepara_volta_bug_par.txt:6`.) |

A matriz C7 não autoriza ampliar esta volta: os problemas de V sem A, leitura anterior à trava e S127 continuam localizáveis no código; a decisão registrada manda manter C7. Preservar as caracterizações correspondentes e a atribuição à outra obra, sem anunciar cura. (`app/chamados/models.py:1905-1917`; `:1950`; `:1959-1974`; `app/chamados/juizes.py:2432-2446`; `app/docs/RELATO.md:80`.)

Dois achados adicionais da matriz entram no delta de `materializacao.py`/`validacao.py`: usar a marca `Batida.criada_neste_toque` para identificar criação deste ato, em vez de inferir por teto de PK; descrever os erros das demais perguntas sem afirmar que pertencem a outro dia. A marca já tem escritor e leitor existentes; `separar_erros` separa por PK, não por data. (`app/ponto/registro_batida.py:155`; `:161`; `app/ponto/services/resposta_batida.py:14`; `app/chamados/services/validacao.py:122-127`; aplicação à irmã: **[nao verificado]**.)

**(f) PADRÃO**

Corrigir `padrao=Single Writer` para **`padrao=Single Writer Principle`**, termo literal de `app/docs/GLOSSARIO.md:12`. O diff proposto realiza o padrão ao manter uma fonte congelada do universo, compartilhar sua leitura entre testemunha/escrita/comando, perguntar ao K6 existente e continuar usando as portas de validação, carimbo e batida. A realização efetiva precisa ser conferida no diff construído; hoje é **[nao verificado]**. (`app/docs/GLOSSARIO.md:12`; `app/docs/JUIZES.md:38-39`; `app/core/juizes.py:337`; `app/chamados/models.py:3180-3186`; `app/ponto/registro_batida.py:152-161`.)

Linha contratual para a volta: `LEI-AKITA: origem=universo congelado + portão da materialização; testemunha=estado_aplicacao_pergunta e competencia_entregue; RED=blocos (a)-(c); quem-mais-le=bloco (g); juizes novos=0; padrao=Single Writer Principle`. A declaração não elimina a pergunta do bloco (d). (`CLAUDE.md:62-70`; `prepara_volta_bug_par.txt:6`.)

**(g) QUEM ESCREVE / QUEM LÊ**

Este é o envelope de campos da volta e das portas existentes que ela aciona. `JUIZES.md` é lookup; o registro vivo é `core/juizes.py`, e divergências do lookup devem ser corrigidas no mesmo commit. (`app/docs/JUIZES.md:10-14`.)

| Campo ou grupo | Quem escreve | Quem lê / contrato da volta |
|---|---|---|
| **U60/U103/U163 e hash** | Construtor transcreve exclusivamente `u163.py:1-2` para o arquivo congelado. | Carregador único proposto em `universo_bug_par.py`; portão, testemunha, contador e comando consomem seus conjuntos. Implementação: **[nao verificado]**. |
| **`pk`, `motivo`, identidade da pergunta e vínculos** | Identidade no nascimento, com contrato em `app/chamados/models.py:2505-2510`. | Pertencimento usa só PK; materializador lê motivo/chamado em `app/chamados/services/materializacao.py:342-347`. |
| **`data_evento` e data do contexto** | Porta `gravar_dia_do_fato`, `app/chamados/services/fio.py:829-840`. | `data_do_fato_de`, `fio.py:40`; derivação do plantio em `materializacao.py:347-352`; competência em `passivo_validado.py:86-113`. |
| **`resposta_colab`, `respondida_em`** | Resposta humana em `app/chamados/services/disputa_emissao.py:1481-1483`; automática em `:1673-1678`; simetria do carimbo em `app/chamados/models.py:3213-3216`. | Validação em `validacao.py:418`; plantio em `materializacao.py:359-360`; seleção do laço em `:1375-1378`; C7 em `models.py:1907-1908`. |
| **`validada_em`, `validada_por`** | Ato de validar em `app/chamados/services/validacao.py:152-166`; devolução em `:213-233`; resposta nova desfaz em `disputa_emissao.py:1494-1496`; simetria preenche nulos em `models.py:3217-3220`. | Validação em `validacao.py:421`; laço em `materializacao.py:1377`; C6 em `juizes.py:1388`; C7 em `models.py:1906`. |
| **`materializada_em`, `materializada_por`, `via_resolucao`** | `_escrever_carimbo`, `app/chamados/models.py:3180-3184`, pelas portas `carimbar`, identidade e descarimbo (`:3186`, `:3251`, `:3286-3295`); relavra de via em `services/relavra_via.py:125-128`. | Testemunha em `materializacao.py:440-448`; seleção do laço em `:1378`; C6 em `juizes.py:1388-1389`. Devolução dos 103 deve usar essas portas, sob K6. |
| **`pendencia_admin`, `recusa_motivo`** | Materializador em `app/chamados/services/materializacao.py:856-857` e `:1105-1113`; recusa humana em `validacao.py:619-629`; outros escritores documentados em `validacao.py:42-45`. | Devolução em `validacao.py:213-233`; explicação em `explicador.py:85-90`. **Não são fonte de pertencimento.** (`cortes_lote7_e_bug_par.txt:1`, item 2.) |
| **`veredito_lavrado`, `julgado_em`, `janela_expira_em`** | Lavrador em `app/chamados/veredito_lavrado.py:254-275`, acionado pelo carimbo em `models.py:3229-3233`. | Controle de validade em `veredito_lavrado.py:213-232`; foto de reversão existente em `passivo_validado.py:64-66`. |
| **`Batida.timestamp`, `tipo`, `origem`, aprovação/autoria** | `registrar_batida`, criação em `app/ponto/registro_batida.py:152-161`; materializador fornece argumentos em `materializacao.py:932-940`. | Dedup, colisão e F6 em `registro_batida.py:107-150`; derivação compartilhada em `materializacao.py:333-428`. |
| **`Batida.pergunta_origem`, `criada_neste_toque`** | Vínculo em `app/chamados/services/materializacao.py:952-956`; marca de criação em `app/ponto/registro_batida.py:155` e `:161`. | Lastro em `app/chamados/juizes.py:726-728`; leitor da marca em `app/ponto/services/resposta_batida.py:14`; rollback do par deve consumir a mesma marca. |
| **`DecisaoPergunta.ato/ato_em/ato_por/ato_texto/a

[truncado em 20000 de 23304 caracteres]

## 15/11 23:06 · sessao 4f0a0ecbc769 · BUG-PAR-O-PAR · verificador · origem=Code

Uso **PROCEDE para objeção procedente**. Condição futura ainda não comprovada não equivale, sozinha, a contradição do aval: o item 3 manda registrar a condição que faltar e seguir a fila. (`prepara_volta_bug_par.txt:4-5`)

**Item 1 — leitura e brief**

1. **NAO PROCEDE.** As referências correspondem ao contrato: F1* ∪ F2 é conferência dos 163, com limitações documentadas; a tabela reconcilia os testes; o apply antigo não excluía mecanicamente as 84; há 247 metades fora dos 163, sendo 75 em exportada. São resultados da sonda fornecida, referentes à árvore histórica declarada, não uma medição atual desta cópia. (`entrega.md:5`, `entrega.md:10-45`, `entrega.md:52-60`, `entrega.md:89-130`)   [nao verificado]
   O juiz **existe com o nome `competencia_entregue`**, implementado em `app/folha/export.py:556`, registrado pela pergunta `_K6`, “a competencia ja foi entregue (lavrada)?”. Seu resultado consulta exportação vigente não invalidada. (`app/core/juizes.py:181`, `app/core/juizes.py:337`, `app/folha/export.py:588-594`) A lista integral congelada com seu hash e o brief resultante: **[nao verificado]**.

2. **NAO PROCEDE.** O contrato preserva os cortes materiais: lista congelada, F1* ∪ F2 apenas para conferência, 19 abertas elegíveis à devolução e 84 entregues preservadas e publicadas em lista. A leitura não abre construtor nem disputa a ordem das vagas. (`prepara_volta_bug_par.txt:1-2`, `cortes_lote7_e_bug_par.txt:1`, itens 2 e 3; `app/docs/RELATO.md:47`)

3. **NAO PROCEDE.** Este item produz o brief e apura “perguntas de lei: N”; não pressupõe N=0. Se não conseguir garantir a proteção das metades exportadas por juiz existente, manda registrar pergunta de lei. Isso deixa uma saída literal para o problema ainda aberto. (`prepara_volta_bug_par.txt:2`) O conteúdo final do brief e sua revisão: **[nao verificado]**.

4. **NAO PROCEDE.** O item autoriza leitura e exige proteção contra plantio nas exportadas; executar apply permanece proibido. (`prepara_volta_bug_par.txt:2`, `prepara_volta_bug_par.txt:6`) Essa proteção **não pode ser certificada apenas por `competencia_fechada`**: ele consulta `FechamentoMensal` aprovado/exportado, enquanto `competencia_entregue` consulta TXT vigente; o próprio passivo distingue os dois fatos. (`app/ponto/janelas.py:75-89`, `app/chamados/services/passivo_validado.py:116-127`) A garantia operacional da futura volta: **[nao verificado]**.

**Item 2 — medir cruzamento**

1. **NAO PROCEDE.** É uma ordem de medir contra as cópias nomeadas, cuja abertura está registrada; não declara um resultado já obtido. (`prepara_volta_bug_par.txt:3`, `app/docs/RELATO.md:6`, `app/docs/RELATO.md:22`) Os HEADs atuais dessas cópias e a interseção da nova volta: **[nao verificado]**.

2. **NAO PROCEDE.** Medir arquivos em comum atende ao critério de cruzamento da casa; não altera universo, tratamento das 84 ou prioridade de construção. (`CLAUDE.md:309-311`, `app/docs/BACKLOG.md:2049`, `prepara_volta_bug_par.txt:3`)

3. **NAO PROCEDE.** O item permite produzir a medida necessária à condição futura. O resultado antigo não a substitui: ele comparou `main...6e2fc9133` com R3 `e08db44a6` e F1 `be2b8d733`, revisões diferentes das nomeadas agora. (`app/docs/RELATO.md:65`, `prepara_volta_bug_par.txt:3`)

4. **NAO PROCEDE.** O ato declarado é medir cruzamento de arquivos; apply e toque em exportada continuam proibidos. (`prepara_volta_bug_par.txt:3`, `prepara_volta_bug_par.txt:6`)

**Item 3 — construção condicional**

1. **NAO PROCEDE como defeito do aval.** As quatro condições são futuras e cumulativas. N=0, revisão do brief sem PROCEDE, cruzamento atual e vaga livre estão **[nao verificado]**; portanto esta revisão não certifica abertura de construtor. A consequência escrita para condição não fechada é registrar e seguir a fila. (`prepara_volta_bug_par.txt:4`)

2. **NAO PROCEDE.** A exceção é nominal para **construir em cópia própria**, mantendo o pouso depois da R3. A mudança da ordem das vagas também é expressa: voltas R3/F1, BUG-PAR, LOTE7 c6, LOTE3 c6. Logo essas duas alterações não são colisões omitidas; os cortes materiais dos itens 2 e 3 continuam preservados. (`prepara_volta_bug_par.txt:1`, `prepara_volta_bug_par.txt:4`; ordem anterior em `app/docs/BACKLOG.md:1952` e `app/docs/RELATO.md:13`)

3. **NAO PROCEDE como contradição; a condição ainda não está certificada.** Os dois arquivos aparecem no **cruzamento histórico**, editados dos dois lados; isso não demonstra sua presença na nova volta. (`app/docs/RELATO.md:65`)  
   `test_medida_turno_do_par.py` registra identidades e posições de ocorrências, inclusive de materialização/passivo, e cobra atualização quando elas mudam; isso explica uma possível alteração do teste. (`app/core/tests/test_medida_turno_do_par.py:9-18`, `app/core/tests/test_medida_turno_do_par.py:174-177`, `app/core/tests/test_medida_turno_do_par.py:549-553`)  
   `crons.py` é a fonte do crontab; a necessidade concreta de alterá-lo nesta volta é **[nao verificado]**. (`app/config/crons.py:1-14`) **Os dois pertencem à nova volta? [nao verificado].** Na leitura estrita de igualdade, uma interseção diferente — inclusive vazia — não fecha a condição escrita; não cabe presumir os dois nem acrescentá-los para fazê-la fechar. (`prepara_volta_bug_par.txt:4`, `CLAUDE.md:37`)

4. **NAO PROCEDE no alcance autorizado.** O aval proíbe tocar exportada, apply e pouso antes da R3. (`prepara_volta_bug_par.txt:6`) O risco posterior é concreto no código: o passivo planta e, em exportada com plantio, segue para REGEN e Pauta; por isso proibir apenas o comando apply não provaria ausência de efeito. (`app/chamados/services/passivo_validado.py:433-459`) Alteração monetária efetiva e quantas das 75 seriam barradas: **[nao verificado]**. O gravado exportado permanece sujeito ao `!` e às provas da L-092. (`CLAUDE.md:411-417`)

ITEM 1: NAO PROCEDE  
ITEM 2: NAO PROCEDE  
ITEM 3: NAO PROCEDE

## 15/11 23:04 · sessao 74fa07eb48eb · - · - · origem=Ronald (tmux)

**NAO PROCEDE como defeito do aval:** a condição é prospectiva e tem saída expressa quando não fecha: registrar e seguir a fila (`prepara_volta_bug_par.txt`, item 3).

O cruzamento histórico não certifica o atual (`app/docs/RELATO.md:65`). A presença dos dois arquivos na nova volta e a interseção atual permanecem **[nao verificado]**; isso impede declarar a condição satisfeita, sem provar contradição do aval.

A exceção para construir em cópia própria e a alteração da ordem são nominais; o pouso continua após R3 (`prepara_volta_bug_par.txt`, item 3). Não cabe devolver o item apenas por contrariar a regra geral que ele expressamente excepciona. [nao verificado]

## 15/11 23:03 · sessao c1095e21ab69 · - · - · origem=Ronald (tmux)

Inventário do HEAD fornecido, para o bloco (g):

| Campo/insumo | Quem escreve | Quem lê / autoridade |
|---|---|---|
| `PerguntaDisputa.pk` no universo congelado | Lista nova do brief, derivada exclusivamente de `u163.py:1-2`; não há selecionador dessa lista no código examinado `[nao verificado]` | Leitor novo proposto deve consumir apenas PK e conferir hash; não usar `recusa_motivo` (`cortes_lote7_e_bug_par.txt:1`, item 2). |
| `motivo`, `colaborador_id`, `instancia_marco`, `conversa` | Identidade escrita no nascimento; contrato está em `app/chamados/models.py:2505-2510` | Derivação do plantio lê motivo, resposta e dia em `app/chamados/services/materializacao.py:274-285`; a fonte dos marcos é `grade_da_celula`, nessa mesma passagem. |
| `data_evento` | Porta `gravar_dia_do_fato`, atribuição em `app/chamados/services/fio.py:829-840` | `data_do_fato_de`, `app/chamados/services/fio.py:40`; materializador prioriza o campo em `app/chamados/services/materializacao.py:347-352`. |
| `resposta_colab`, `respondida_em` | Resposta humana: `app/chamados/services/disputa_emissao.py:1481-1483`; automática: `:1673-1678`; carimbo pela trilha: `:1695-1701`; reabertura prepara limpeza em `:1609`, pela porta `descarimbar_em_lote` (`:1600-1605`) | Validação: `app/chamados/services/validacao.py:418-423`; seleção do laço: `app/chamados/services/materializacao.py:1375-1382`; C6: `app/chamados/juizes.py:1388-1389`; C7: `app/chamados/models.py:1906-1908`. |
| `validada_em`, `validada_por` | Ato de validar: `_carimbar_validacao`, `app/chamados/services/validacao.py:152-166`; devolução: `:213-233`; resposta nova desfaz: `app/chamados/services/disputa_emissao.py:1494-1496`; simetria do carimbo pode preencher nulos: `app/chamados/models.py:3213-3220` | Validação: `app/chamados/services/validacao.py:421`; materialização: `app/chamados/services/materializacao.py:1375-1378`; C6: `app/chamados/juizes.py:1388`; C7: `app/chamados/models.py:1906`. |
| `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição agrupada: `_escrever_carimbo`, `app/chamados/models.py:3180-3184`; identidade: `:3251-3273`; descarimbar: `:3286-3295`; lote: `:3364-3365`; relavra também escreve via: `app/chamados/services/relavra_via.py:125-128` | Testemunha da aplicação: `app/chamados/services/materializacao.py:440-448`; laço: `:1378`; C6: `app/chamados/juizes.py:1388-1389`; lastro: `:682-710`; explicador: `app/chamados/services/explicador.py:95`. |
| `pendencia_admin`, `recusa_motivo` | Pendência conjunta: `app/chamados/services/materializacao.py:856-857`; recusa/identidade: `:1105-1113`; recusa humana: `app/chamados/services/validacao.py:619-629`; escape: `app/chamados/services/disputa_emissao.py:1511`; duplicata: `app/chamados/services/fio.py:1399` | Devolução lê motivo e materialização: `app/chamados/services/validacao.py:213-233`; explicação: `app/chamados/services/explicador.py:90`; universo novo não deve lê-los (`cortes_lote7_e_bug_par.txt:1`, item 2). |
| `Batida.timestamp`, `tipo`, `origem`, aprovação/autoria | `registrar_batida`, `app/ponto/registro_batida.py:94`, criação em `:152-161`; materializador fornece valores e `fato_de_chao=False` em `app/chamados/services/materializacao.py:933-939` | Derivação do plantio: `app/chamados/services/materializacao.py:346-379`; dedup/colisão/competência: `app/ponto/registro_batida.py:107-150`. |
| `Batida.pergunta_origem` | Materializador chama `anotar`: `app/chamados/services/materializacao.py:952-956` | Juiz do lastro declara consumo do vínculo: `app/chamados/juizes.py:726-728`. |
| `Batida.criada_neste_toque` | `registrar_batida` marca a criação em `app/ponto/registro_batida.py:155` e `:161` | Leitor informado no anexo: `ponto/services/resposta_batida.py:14` (`MATRIZ-C7-BUGPAR/entrega.md:105`, numeração exata do anexo `[nao verificado]`). |
| `DecisaoPergunta` — `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo` | Porta única `registrar`, `app/chamados/models.py:4085-4099`; validação chama em `app/chamados/services/validacao.py:164-166`; devolução em `:230-233` | Tela: `PerguntaDisputa.respostas_anteriores`, `app/chamados/models.py:3644-3657`; contrato de imutabilidade/fato: `:3982-3998`. |
| `LogAuditoria` — ação, objeto, antes/depois, autor | Porta `core.observ.evento`, `app/core/observ.py:94-138`; `registrar_log` delega em `app/core/auditoria.py:22-24` | Exemplo de leitor por ação: `app/chamados/models.py:401-405`; trilha de recusa humana produzida em `app/chamados/services/validacao.py:637-642`. |
| `ExportacaoDominio.empresa_id`, `mes`, `ano`, `gerado_em`, `hash_sha256` | `ExportacaoDominio.registrar`, `app/folha/models.py:174-187`; chamado da view em `app/folha/views.py:297` | Juiz registrado `folha/export.py::competencia_entregue`, registro `app/core/juizes.py:337`, implementação `app/folha/export.py:556-594`. |
| `ExportacaoDominio.invalidada_em`, `invalidada_motivo`, `substituida_por` | Porta `invalidar`, `app/folha/models.py:143-170` | Juiz K6 usa vigência + não invalidada: `app/folha/export.py:588-594`; vigência: `app/folha/models.py:192-225`. |

**Correção necessária em `JUIZES.md`:** a tabela diz “escritor único de `validada_em`” (`app/docs/JUIZES.md:38-39`), mas o serviço distingue expressamente escritor único do **ato** de validar de múltiplos escritores do **campo** (`app/chamados/services/validacao.py:19-26`), demonstrados acima. O próprio lookup manda corrigir divergência no mesmo commit (`app/docs/JUIZES.md:14`). A volta deve tornar essa descrição precisa, sem inventar autoridade. (`app/docs/JUIZES.md:10-14`.)

**Ausências no main examinado:** a busca literal em `app/chamados` não encontrou `bug_par_103`, `devolver_paridade_bug_par_103`, `no_universo_163`, `par_do_intervalo` nem `espera_na_escrita`; a ausência não dispõe de `arquivo:linha`, portanto a certificação dessas peças nesta cópia fica `[nao verificado]`. Os endereços antigos devem ser citados com o prefixo `6e2fc9133:` conforme `predesenho/entrega.md:5`.

## 15/11 23:02 · sessao 89c4371234f4 · - · - · origem=Ronald (tmux)

Não há juiz existente que garanta zero plantio nas 75 metades fora163 em competência exportada: o chokepoint pergunta F6, que lê `FechamentoMensal.status`; a condição exportada do contrato é K6, que lê `ExportacaoDominio` vigente não invalidada. O código não estabelece K6 ⇒ F6. (`app/ponto/registro_batida.py:132-137`; `app/ponto/janelas.py:84-89`; `app/folha/export.py:588-594`; registros em `app/core/juizes.py:309` e `:337`.)

Caminhos atuais:

- Admin: `app/chamados/services/acoes_disputa.py:13-22` → `validacao.py:398` → unidade `:486-488` → laço `:494`.
- Lote: `validacao.py:730`, `:747`, `:781` → mesma casca e mesma validação.
- Laço: `materializacao.py:1375-1383` → `_materializar_uma` (`:1164`, `:1227-1232`) → ramo hora `:790` → `registrar_batida` (`:932-940`, `fato_de_chao=False` em `:936`) → F6 (`registro_batida.py:132-137`).
- Outros chamadores do laço: `fio.py:701-708`, `acoes_disputa.py:139`, `backfill_fio_morto_e4b.py:144-147`.
- Passivo: `_esta_exportada` apenas classifica (`passivo_validado.py:358`), `_trabalhar_o_par` planta antes de examinar o ramo exportado (`:433-435`), `_plantar_o_dia` chama a mesma unidade (`:522`). Depois pode seguir a regeneração exportada e Pauta (`:442-449`).
- Irmã: `_gravar_irma_do_par` não existe na cópia HEAD fornecida; está testemunhada somente na árvore antiga pelos anexos (`predesenho/entrega.md:119-122`; matriz C7, seção 1). Linha atual [nao verificado].
- O próprio materializador declara que a porta admin não tem portão de competência por O44 (`materializacao.py:1172-1174`; `validacao.py:424-428`).

Pergunta de lei sugerida: “Como preservar o proibido de nenhuma metade fora163 em competência entregue plantar depois da volta, se restaurar o caminho do main as entrega a F6 e O44 permite o toque administrativo? Autoriza-se uma restrição específica pelo K6 nos caminhos alcançados, ou qual outra solução deve vigorar?” Não afirmar que substituir F6 por K6 já está autorizado.

C7 permanece fora da cura desta volta: a decisão registrada manda manter seu predicado e atribui a classe à RAIZ-DA-DISPUTA F1 (`app/docs/RELATO.md:80`). Hoje, V sem A continua excluída de `sem_veredito_entre` (`models.py:1905-1909`) e `pode_fechar` segue `!respostas_sem_veredito && !fio_vivo` (`:1911-1917`). `fechar` lê pendências/vivas antes da trava (`:1950`, `:1959-1960`, `:1969-1974`), e S127 só consulta fio/vazio (`juizes.py:2432-2446`). Registrar as limitações da matriz e corrigir a prosa falsa; não incluir alteração desses predicados sob este aval.

## 15/11 23:02 · sessao 74fa07eb48eb · - · - · origem=Ronald (tmux)

Achados somente por leitura:

- O cruzamento dos dois arquivos é **histórico**: `main...6e2fc9133` contra R3 `e08db44a6` e F1 `be2b8d733`; a F1 carregava os mesmos commits da R3. Isso está documentado em `app/docs/RELATO.md:65` e `app/docs/BACKLOG.md:2046`. Não demonstra cruzamento da nova volta contra F1 B1/R3 c17 **[nao verificado]**.
- As cópias atuais são distintas das históricas: F1 B1 `wt-f1nova-b1-203152` consta em `app/docs/RELATO.md:22`; R3 c17 `wt-codex-e3-resto-r3-221106`, em `app/docs/RELATO.md:6`. O relato afirma zero arquivo comum entre essas duas cópias, sem medir ali o delta da BUG-PAR (`app/docs/RELATO.md:6`).
- `test_medida_turno_do_par.py` é uma catraca de identidade e posições AST: exige atualizar o registro no mesmo commit quando as linhas mudam (`app/core/tests/test_medida_turno_do_par.py:9`, `:17`, `:549`). Registra funções de `materializacao.py` e `passivo_validado.py`, inclusive suas linhas (`:174`, `:175`, `:271`, `:272`). Isso explica por que uma mudança nesses módulos pode exigir atualizar o teste; que a nova volta efetivamente o alterará **[nao verificado]**.
- `crons.py` é a fonte do crontab, com comandos e seus contratos (`app/config/crons.py:1`, `:10`). A razão concreta para estar no delta histórico, e sua presença na lista da nova volta, permanecem **[nao verificado]**. A entrega fornecida declara o passivo manual fora de `config/crons.py` (`entrega.md:116`, conforme numeração fornecida pelo usuário; conferir contagem).
- A regra de cruzamento é arquivo em comum, sem exceção por ser teste (`CLAUDE.md:309`). A política das laterais exige comparar o diff da raia às obras E em voo (`app/docs/BACKLOG.md:2049`). Portanto não cabe reutilizar automaticamente a interseção histórica.
- A nova exceção é nominal e limitada à construção, preservando o pouso após R3; sua colisão com a regra geral não procede pelo próprio literal de `prepara_volta_bug_par.txt`, item 3. A regra geral está em `app/docs/BACKLOG.md:2049` e `CLAUDE.md:309`.
- A ordem antiga é R3 → LOTE7 → LOTE3 (`app/docs/BACKLOG.md:1952`, `app/docs/RELATO.md:13`). O novo aval declara expressamente alterar essa ordem no item 3, preservando precedência das voltas R3/F1; não há colisão meramente por inserir BUG-PAR.
- Os cortes 2 e 3 permanecem registrados: lista congelada, F1*∪F2 somente conferência, 19 abertas, 84 entregues preservadas e listadas (`app/docs/RELATO.md:47`).
- A ocupação corrente da vaga é **[nao verificado]**: há registros de abertura de R3 e F1, mas eles não certificam disponibilidade atual (`app/docs/RELATO.md:6`, `:22`).

## 15/11 23:01 · sessao 947bd4e0266e · - · - · origem=Ronald (tmux)

- **Juiz existente confirmado:** `competencia_entregue` está em `app/folha/export.py:556`; devolve existência de TXT vigente não invalidado em `app/folha/export.py:588-594`. Registro: pergunta `_K6 = 'a competencia ja foi entregue (lavrada)?'` em `app/core/juizes.py:181`, vinculada a `folha/export.py::competencia_entregue` em `app/core/juizes.py:337`.

- **Não confundir com competência fechada:** `competencia_fechada` consulta `FechamentoMensal` com status `aprovado`/`exportado`, em `app/ponto/janelas.py:75-89`. O plantio consulta essa guarda em `app/ponto/registro_batida.py:130-137`; isso não certifica impedir plantio em toda competência entregue por TXT. O próprio passivo distingue expressamente os dois fatos em `app/chamados/services/passivo_validado.py:116-127`.

- **Existe também juiz composto:** `competencia_fechada_para_celula` combina `competencia_entregue` e holerite publicado em `app/folha/export.py:597-628`; registro `_K9` em `app/core/juizes.py:347`. Seu uso como bloqueio nos caminhos da volta é `[nao verificado]`.

- **Caminho com alcance monetário potencial:** passivo planta em `app/chamados/services/passivo_validado.py:433-434`; havendo plantio e competência exportada, chama `regenerar_em_exportada` em `app/chamados/services/passivo_validado.py:435-445` e gera Pauta em `app/chamados/services/passivo_validado.py:448-459`. Quantas das 75 seriam barradas e alteração monetária concreta: `[nao verificado]`.

- **Lei do gravado exportado:** `CLAUDE.md:411-417` exige `!`, DIFF antecipado, reversão, prova posterior e porta REGEN-EM-EXPORTADA.

- **Atenção à diferença entre árvore e anexos:** não encontrei nesta cópia `app/chamados/management/commands/bug_par_103.py`, `_espera_do_aval_163`, `espera_na_escrita` nem `devolver_paridade_bug_par_103`; presença desses elementos no HEAD alegado: `[nao verificado]`. O materializador atual escreve diretamente pelos ramos em `app/chamados/services/materializacao.py:1224-1232`, com declaração de ausência de portão de competência do admin em `app/chamados/services/materializacao.py:1168-1174`. As linhas da entrega que citam tais elementos pertencem explicitamente à árvore histórica `6e2fc9133`, segundo `entrega.md:5`.

## 15/11 22:42 · sessao 13487bd579d9 · E3-RESTO-R3 · verificador · origem=Code

A correção fecha os caminhos **diretos** de injeção de relógio e amplia a cobertura estrutural do vigia. Ela não fecha todos os relógios alcançáveis pelo lavrador: há um resíduo declarado no caminho de `classificar_export`. (`app/relatorios/furos.py:103`, `app/folha/vigia_retido_calado.py:54`, `app/folha/fora_da_folha.py:137`.)

A comparação abaixo usa o diff fornecido e os arquivos da cópia. Identidade Git do HEAD, ancestralidade, RED histórico e ocorrências em produção: **[nao verificado]**.

**1. Quem escreve, quem lê e assinaturas**

| Campo, parâmetro ou função | Quem fornece/escreve | Quem lê |
|---|---|---|
| `agora` e `hoje` do lavrador | `handle` lê `now()` uma vez e deriva o dia local. | `competencias_do_retrato`; `montar_linhas`, que encaminha instante ao fio e dia ao informativo. (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:61`, `app/folha/management/commands/lavrar_retrato_folha.py:65`, `app/folha/retrato_folha.py:1214`, `app/folha/retrato_folha.py:1220`.) |
| `competencias_do_retrato(hoje, empresa)` | Constrói pares pelo fim de `janela_anterior` e `janela_atual`, eliminando repetição. | Lavrador automático, frescor e vigia. A seleção explícita `--mes/--ano` do lavrador continua separada. (`app/folha/retrato_folha.py:1378`, `app/folha/management/commands/lavrar_retrato_folha.py:57`, `app/folha/management/commands/lavrar_retrato_folha.py:61`, `app/folha/retrato_folha.py:1412`, `app/folha/vigia_retido_calado.py:66`.) |
| `RetratoFolha.empresa/mes/ano/linhas/lavrado_em` | Escritor de produção encontrado: `lavrar_retrato_folha`, por `update_or_create`, somente com `--apply`; `lavrado_em` recebe o início da rodada. | Tela/PDF/Raio-X pelo recorte; drawer do colaborador; frescor; vigia. Há unicidade por empresa e competência. (`app/folha/management/commands/lavrar_retrato_folha.py:72`, `app/folha/management/commands/lavrar_retrato_folha.py:77`, `app/folha/models.py:365`, `app/folha/views_retrato.py:194`, `app/colaboradores/views.py:982`, `app/folha/retrato_folha.py:1413`, `app/folha/vigia_retido_calado.py:59`.) |
| `agora`/`hoje` do recorte | `_recorte_da_barra` lê o instante uma vez e deriva o dia local. | Instante alimenta frescor; dia alimenta recorte, LED, barra, PDF e Raio-X. O envelope devolve `hoje`, mas não devolve `agora`. (`app/folha/views_retrato.py:179`, `app/folha/views_retrato.py:212`, `app/folha/views_retrato.py:232`, `app/folha/views_retrato.py:247`, `app/folha/views_retrato.py:286`, `app/folha/views_retrato.py:325`, `app/folha/views_retrato.py:418`, `app/folha/views_retrato.py:501`.) |
| `frescor_dos_retratos(*, agora)` | Produz o diagnóstico comparando a lavra com o maior carimbo do fechamento. | Recorte da tela e `retratos_desatualizados`; este último apenas conta diagnósticos atrasados/ausentes. (`app/folha/retrato_folha.py:1415`, `app/folha/retrato_folha.py:1424`, `app/folha/views_retrato.py:212`, `app/folha/retrato_folha.py:1455`.) |
| `censo(empresas=None, *, hoje)` | Command fornece IDs e um `localdate()` da borda; `censo` constrói o resultado em memória. | `linhas` formata; command consulta as chaves de alarme. (`app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/vigia_retido_calado.py:73`, `app/folha/vigia_retido_calado.py:105`, `app/folha/management/commands/vigiar_retido_calado.py:38`.) |
| `escalas_em_limbo(empresa=None, *, hoje)` | `furos_espelho` fornece o dia do instante da borda. | Consulta `eh_dia_trabalho` para hoje/amanhã e a existência da foto mensal de `FolgaDia`; resultado entra no contexto da tela. (`app/relatorios/views.py:1519`, `app/relatorios/views.py:1554`, `app/relatorios/furos.py:67`, `app/relatorios/furos.py:86`, `app/relatorios/views.py:1558`.) |
| `dataset_furos(..., *, agora)` | `furos_espelho` fornece `_agora`; dataset recusa omissão, `None` e valores que não sejam `datetime`. | Encaminha o mesmo instante ao D4 e ao calendário; cards alimentam HTML e PDF de furos. (`app/relatorios/views.py:1543`, `app/relatorios/furos.py:113`, `app/relatorios/furos.py:127`, `app/relatorios/furos.py:168`, `app/relatorios/views.py:1552`, `app/relatorios/views.py:1564`.) |

O envelope persistido das linhas é montado em um lugar: `colab/codigo/status/motivo`, `dias/datas`, `dias_sem_leitura/dias_sem_porta`, `dono/acoes/tipo/saldo/informativo`. Os leitores principais são: identificação e apresentação na tela; classificação pelos acessores do módulo; período por `no_periodo`; ações por `portas_da_linha`; testemunhos no vigia e no drawer. (`app/folha/retrato_folha.py:1237`, `app/folha/views_retrato.py:278`, `app/folha/retrato_folha.py:446`, `app/folha/retrato_folha.py:500`, `app/folha/retrato_folha.py:510`, `app/folha/retrato_folha.py:1338`, `app/folha/retrato_folha.py:1044`, `app/folha/vigia_retido_calado.py:84`, `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/templates/folha/_retrato_o_que_fazer.html:56`.)

No envelope do recorte, `_recorte_da_barra` escreve empresa/competência/navegação, retratos/linhas/carimbo, frescor/filtros/universo/grupos/visíveis, URLs e `hoje`; os três consumidores são tela, PDF e Raio-X. (`app/folha/views_retrato.py:240`, `app/folha/views_retrato.py:260`, `app/folha/views_retrato.py:388`, `app/folha/views_retrato.py:489`.)

As nove chaves do censo têm estes escritores e leitores:

| Chaves | Escrita e leitura |
|---|---|
| `retratos`, `competencias_sem_retrato` | `censo` produz trios **empresa,mês,ano**, ordenados por empresa/ano/mês; `linhas` conta/exibe; command também consulta faltantes para alarmar. (`app/folha/vigia_retido_calado.py:69`, `app/folha/vigia_retido_calado.py:88`, `app/folha/vigia_retido_calado.py:106`, `app/folha/management/commands/vigiar_retido_calado.py:16`.) |
| `bloqueados`, `calados`, `roteados` | Acumulados por pessoa/retrato/competência; formatador exibe; command consulta `calados` no alarme passivo e usa `bloqueados` indiretamente por `cego`. (`app/folha/vigia_retido_calado.py:77`, `app/folha/vigia_retido_calado.py:89`, `app/folha/vigia_retido_calado.py:101`, `app/folha/vigia_retido_calado.py:111`, `app/folha/management/commands/vigiar_retido_calado.py:17`.) |
| `dias_sem_leitura_sistema`, `linhas_sem_campo_leitura` | Somados das linhas bloqueadas; formatador exibe; command consulta dias no alarme passivo. (`app/folha/vigia_retido_calado.py:84`, `app/folha/vigia_retido_calado.py:92`, `app/folha/vigia_retido_calado.py:100`, `app/folha/vigia_retido_calado.py:115`, `app/folha/management/commands/vigiar_retido_calado.py:17`.) |
| `por_empresa` | `censo` escreve chaves **empresa,ano,mês**; `linhas` desempacota nessa mesma ordem. (`app/folha/vigia_retido_calado.py:95`, `app/folha/vigia_retido_calado.py:110`.) |
| `cego` | Calculado por ausência de retratos ou bloqueados; formatador exibe e command alarma. (`app/folha/vigia_retido_calado.py:101`, `app/folha/vigia_retido_calado.py:114`, `app/folha/management/commands/vigiar_retido_calado.py:16`.) |

Não encontrei chamador de produção com a assinatura antiga dos dois alvos alterados: a view fornece `hoje=` ao limbo e `agora=` ao dataset. Os testes funcionais também foram ajustados; o teste fora do diff em `ponto` já fornece `agora=`. (`app/relatorios/views.py:1543`, `app/relatorios/views.py:1554`, `app/relatorios/tests/test_selo_ancora_do_vinculo.py:45`, `app/relatorios/tests/test_e4_8_furos_le_a_lavra.py:94`, `app/ponto/tests/test_cobparc_v4_procedentes.py:288`.)

Os demais consumidores de teste incluem vigia com banco; fixtures sem banco de c4/c5/c6/c14/c15/c16; command em c11; e contratos novos c17. As chamadas sem `hoje` de c5 são negativas deliberadas, esperando `TypeError`. (`app/folha/tests/test_vigia_retido_calado.py:29`, `app/chamados/tests/test_e3_r3_c4_sem_banco.py:158`, `app/chamados/tests/test_e3_r3_c5_sem_banco.py:245`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:231`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:306`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:53`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:43`, `app/chamados/tests/test_e3_r3_c11_sem_banco.py:273`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:65`.)

O cron aciona o lavrador com `--apply` e o vigia com `--alarme`, dependendo da lavra; não chama diretamente as funções que mudaram de assinatura. (`app/config/crons.py:1650`, `app/config/crons.py:1681`.)

**2. Significado de `hoje`, `agora`, fuso e limites**

Nos caminhos diretos examinados, `agora` é instante e `hoje` é seu **dia civil local**. Lavrador, recorte, frescor e relatório usam `localtime(agora).date()`; D4 e folha usam `localdate(agora)`, conversão equivalente para o mesmo instante aware no mesmo fuso ativo. O padrão configurado é `America/Sao_Paulo`, com `USE_TZ=True`. (`app/folha/management/commands/lavrar_retrato_folha.py:50`, `app/folha/views_retrato.py:180`, `app/folha/retrato_folha.py:1409`, `app/relatorios/views.py:1520`, `app/ponto/status_dia.py:254`, `app/folha/fora_da_folha.py:200`, `app/config/settings/base.py:93`.)

O drawer chama sua variável local `hoje`, mas ela contém um **datetime local**; entrega `hoje.date()` ao leitor do retrato. Há diferença de tipo/nome, sem diferença de dia demonstrada nesse transporte. (`app/colaboradores/views.py:930`, `app/colaboradores/views.py:986`.)

O **dia do turno** permanece separado: D4 consulta turnos por `data_turno`, e as ações publicadas transportam `dia_do_turno` ao link. O balde Hoje compara as datas gravadas com o dia civil recebido e não declara encerramento do turno. (`app/ponto/status_dia.py:273`, `app/folha/retrato_folha.py:1055`, `app/folha/retrato_folha.py:480`.)

Os limites são coerentes com cada pergunta:

- Janela de folha: corte até corte−1; no dia do corte começa a próxima competência. (`app/ponto/janelas.py:16`, `app/ponto/janelas.py:27`.)
- Dataset e recorte de dias: ambas as pontas inclusivas. (`app/relatorios/furos.py:126`, `app/folha/retrato_folha.py:1344`.)
- Balde Hoje: mínimo das datas `>= hoje`; C5 consulta somente até ontem. (`app/folha/retrato_folha.py:491`, `app/escala/services/cadastro_realidade.py:162`, `app/escala/services/cadastro_realidade.py:167`.)

A resposta global a “todos usam o mesmo relógio?” é **não**: o caminho do export ainda pode consultar outro dia, e o placar de aceite mantém relógios próprios. (`app/folha/export.py:968`, `app/folha/export.py:1238`, `app/chamados/services/verbos.py:286`.)

**3. Classes que já reprovaram obra e mordida dos selos**

Não encontrei segundo juiz **novo** nos alvos: competências delegam a `ponto/janelas`; limbo consulta `eh_dia_trabalho`; dataset consulta D4 e lê os marcos apagados da ata. A pendência antiga do limbo continua registrada, apenas com a âncora de assinatura atualizada. (`app/folha/retrato_folha.py:1378`, `app/relatorios/furos.py:67`, `app/relatorios/furos.py:127`, `app/relatorios/furos.py:136`, `app/core/juizes.py:794`.)

Não encontrei segundo escritor de `RetratoFolha` nos caminhos de produção examinados. O recorte grava uma preferência por `PreferenciaUI.set_pref`; o PDF auditável grava outra entidade, `RegistroPDFAuditavel`. Nenhum desses atos regrava o retrato. (`app/folha/management/commands/lavrar_retrato_folha.py:77`, `app/folha/views_retrato.py:157`, `app/relatorios/pdf_base.py:152`.)

Não há nova derivação paralela das competências anterior/atual entre lavrador, frescor e vigia. Permanecem aritméticas distintas fora desse contrato: navegação mês anterior/seguinte na barra e normalização de intervalo inválido do relatório para o início do mês. Esta última tela aceita período livre; isso, sozinho, não demonstra substituição indevida da competência. (`app/folha/retrato_folha.py:1380`, `app/folha/views_retrato.py:191`, `app/relatorios/views.py:1540`, `app/relatorios/tests/test_relatorio_janela_da_empresa.py:52`.)

Os testes novos **não são só texto**: [nao verificado]

- 01–05 executam o censo real e verificam interseção, empresas sem foto, alarme parcial, competências, unidades e legado. (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:65`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:97`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:142`.)
- 06 executa o limbo e verifica dia, amanhã, mês da foto e ausência de consulta ao relógio substituído. (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:182`.)
- 07 executa o dataset, recusa parâmetros inválidos e verifica identidade de instante/carga nos fornecedores. D4 e calendário são mocks: prova transporte, não julgamento transitivo. (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:200`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:212`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:219`.)
- 08 exercita chamadas repetidas, soltas, aliases e conversões; 09 cobra **somente substrings de docstrings**. (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:231`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:252`.)

O detector melhorou, mas continua sintático: trata `localtime(variavel)` como conversão sem provar que a variável não será `None`. Portanto não certifica fechamento transitivo nem rejeição de `None` em runtime. (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:169`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:184`.)

**4. As duas exclusões**

`gerar_pdf_auditavel` é alcançada pelo PDF do retrato **depois** de `_recorte_da_barra`. Recebe a história fechada sobre `_rec` e os filtros, mas não recebe seu instante; lê `timezone.now()` para a geração e usa esse carimbo nas duas passagens do rodapé. Não volta a selecionar as linhas. (`app/folha/views_retrato.py:388`, `app/folha/views_retrato.py:417`, `app/folha/views_retrato.py:443`, `app/relatorios/pdf_base.py:133`, `app/relatorios/pdf_base.py:139`, `app/relatorios/pdf_base.py:170`.)

`placar_aceite` é alcançada pelo Raio-X quando Dono=Colab. Recebe **somente empresa**, sem instante, competência, praça, pessoas ou período do recorte; lê `now()` e `localdate()` separadamente e calcula suas janelas hoje/7d. (`app/folha/views_retrato.py:489`, `app/folha/views_retrato.py:513`, `app/chamados/services/verbos.py:277`, `app/chamados/services/verbos.py:286`, `app/chamados/services/verbos.py:299`.)

Contraexemplo possível por código: recorte às 23:59:59 e placar após 00:00 usam dias diferentes; dentro do próprio placar, `now()` antes e `localdate()` depois da virada também podem divergir. Ocorrência em produção: **[nao verificado]**. (`app/folha/views_retrato.py:179`, `app/chamados/services/verbos.py:286`.)

A exclusão é coerente com a promessa **direta, sem fecho transitivo**, e o reuso do placar foi ordenado no corte O256. Ela não sustenta uma afirmação de relógio único para todos os componentes da tela. O PDF de furos, por sua vez, usa `gerar_pdf_furos_bytes`, sem passar por essas duas funções. (`app/folha/views_retrato.py:146`, `app/docs/CORTES.md:309`, `app/relatorios/views.py:1563`.)

**5. Vigia: papel e universo**

Continua contador e alarme: consulta retratos e C5, soma resultados e retorna; o command escreve stdout e eventualmente sai com código 2. Não há transição nem escrita de estado nesses corpos. (`app/folha/vigia_retido_calado.py:59`, `app/folha/vigia_retido_calado.py:72`, `app/folha/vigia_retido_calado.py:89`, `app/folha/vigia_retido_calado.py:102`, `app/folha/management/commands/vigiar_retido_calado.py:35`, `app/folha/management/commands/vigiar_retido_calado.py:42`.)

O universo mudou em três pontos:

- Parte das empresas visíveis, intersectadas com o recorte; inclui empresas sem foto. Visibilidade exclui inativas e empresas de diagnóstico. (`app/folha/vigia_retido_calado.py:54`, `app/folha/vigia_retido_calado.py:61`, `app/colaboradores/queries.py:85`.)
- Nomeia todos os pares de competência faltantes; C5 recebe sempre os IDs efetivos, inclusive `[]`. (`app/folha/vigia_retido_calado.py:69`, `app/folha/vigia_retido_calado.py:72`.)
- Ausência parcial de retrato tornou-se alarme estrutural. Calados e dias continuam esperado zero, mas só participam do alarme com `--alarme-passivo`. (`app/folha/management/commands/vigiar_retido_calado.py:16`, `app/folha/management/commands/vigiar_retido_calado.py:37`.)

A unidade não foi confundida: calados/roteados/bloqueados são pessoa por competência; dias sem leitura são dia-colab. Campo ausente marca legado desconhecido; campo presente `None` soma zero; positivo conhecido sobrevive ao desconhecido. (`app/folha/vigia_retido_calado.py:44`, `app/folha/vigia_retido_calado.py:84`, `app/folha/vigia_retido_calado.py:100`.)

**6. Leis e cortes**

A implementação direta respeita leitura da autoridade, escritor único e papel do vigia. As competências continuam saindo do corte por empresa. A foto mensal civil de `FolgaDia` é diagnóstico de cadastro, não outra competência de folha. (`app/docs/LEIS.md:60`, `CLAUDE.md:172`, `CLAUDE.md:178`, `app/ponto/janelas.py:63`, `app/relatorios/furos.py:56`, `app/relatorios/furos.py:86`.)

O resíduo de relógio abaixo impede certificar a regra “uma leitura na borda até o julgamento” para o caminho completo do lavrador; a própria cópia já o declara. (`app/docs/PLANO-PADROES.md:92`, `app/folha/fora_da_folha.py:137`.)

**VEREDITO**

**PROCEDENTE — resíduo verificável, sem atribuição de regressão ao c17:**

- **P1 — relógio interno ainda alcançado pelo lavrador.** Caminho: lavrador → `montar_linhas` → `fora_da_folha` → `dias_que_seguram` → `classificar_export` → `motivos_retencao_espelho` sem `hoje` → `_dias_sem_decisao`, que chama `localdate()`. (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `app/folha/retrato_folha.py:1214`, `app/folha/fora_da_folha.py:200`, `app/folha/fora_da_folha.py:147`, `app/folha/export.py:968`, `app/folha/export.py:1307`, `app/folha/export.py:1238`.)

  Contraexemplo: rodada inicia em 14/11 às 23:59:59; ao classificar após a virada, uma célula de 14/11 com `veredito=None` e `trabalha=True` passa em `d < hoje` e pode produzir retenção. A enumeração seguinte recebe o dia injetado 14/11 e não inclui essa mesma célula. Assim, “quem é retido” e “quais dias retêm” podem discordar. É a classe vedada pela injeção de relógio, já reconhecida no resíduo do código; ocorrência real **[nao verificado]**. (`app/folha/export.py:1295`, `app/folha/fora_da_folha.py:165`, `app/folha/fora_da_folha.py:137`, `app/docs/PLANO-PADROES.md:94`.)

**Observações:**

- Comentário do cron ficou incorreto: diz alarme “só pelo cego”. Contraexemplo já coberto pelo teste: uma competência presente, outra ausente, `cego=0`, mas saída 2 por falta parcial. (`app/config/crons.py:1679`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:119`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:123`.)
- `frescor_dos_retratos(agora=None)` ainda permite relógio interno via `localtime(None)`; nenhum chamador real fornecendo esse valor foi demonstrado. (`app/folha/retrato_folha.py:1387`, `app/folha/retrato_folha.py:1409`.)
- Dataset aceita `datetime` naive na guarda, embora downstream espere instante aware; nenhum chamador real naive foi demonstrado. (`app/relatorios/furos.py:113`, `app/ponto/status_dia.py:254`, `app/ponto/turnos.py:2071`.)
- As exclusões são explícitas e limitadas; o item 09 prova sua documentação, não sua segurança transitiva. (`app/folha/views_retrato.py:146`, `app/chamados/tests/test_e3_r3_c17_sem_banco.py:268`.)
- **Não demonstrada regressão nova de assinatura, segundo escritor ou segundo juiz criada pelo c17** nos caminhos inventariados. (`app/relatorios/views.py:1543`, `app/relatorios/views.py:1554`, `app/folha/management/commands/lavrar_retrato_folha.py:77`, `app/folha/retrato_folha.py:1378`, `app/relatorios/furos.py:127`.)

## 15/11 22:39 · sessao d907d4904e3b · - · - · origem=Ronald (tmux)

Classificaria `app/relatorios/views.py:1540-1541` como **observação**, sem violação demonstrada de L-090: a tela aceita período livre, e o contrato distingue competência default de datas explícitas. O contraexemplo prova a normalização para mês civil, mas não que esse resultado seja apresentado como competência. As datas efetivas aparecem nos campos e nos cards. (`app/relatorios/tests/test_relatorio_janela_da_empresa.py:44-60`; `app/templates/relatorios/furos.html:12-13`; `:41`.)

Não encontrei relógio adicional ativo nos caminhos examinados com `agora` válido: o calendário recebe o instante, usa-o no desfazer e no D4; `NAO_PERGUNTADO` pula o ramo que consultaria `autoridade_do_periodo`/`folha_manda`. (`app/relatorios/furos.py:168`; `app/colaboradores/services/calendario.py:192-193`; `:279-283`; `:594-600`; `:638`.)

O `localdate()` sem argumento de `flip_batida.py:38` pertence ao escritor `flip_tipo`, não à leitura `flips_de` usada pelo calendário; esta apenas converte carimbo explícito. (`app/ponto/services/flip_batida.py:10`; `:38`; `:56`; `:77`; `app/colaboradores/services/calendario.py:644`.)

## 15/11 22:39 · sessao 58f3c671885b · - · - · origem=Ronald (tmux)

Auditoria dos selos e leis, só leitura:

- **Testes c17 têm mordida comportamental.** `funcao` extrai e compila o corpo real, substituindo infraestrutura (`app/chamados/tests/e4_apoio.py:8`); os casos 01–05 verificam universo visível/interseção, ausência de foto, alarme estrutural parcial, exclusão de competências antigas e unidades/legado/ordenação (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:97`, `:107`, `:116`, `:129`, `:142`). Os casos 06–07 verificam dia obrigatório do limbo e transporte idêntico de instante/carga ao D4 e calendário, inclusive recusa de omissão/None/tipos inválidos (`:167`, `:193`, `:219`). O caso 08 exercita o detector de chamadas repetidas, soltas e aliases (`:225`).
- **Caso 09 é documental somente.** Ele verifica substrings de docstrings; não demonstra por si mesmo que exclusões, universo ou relógios estejam corretos (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:252`–`:269`). Isso não torna os outros oito casos vazios.
- Executei os nove testes c17 com `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ... -v`: todos passaram. Execução RED na base e ordem histórica “testes antes das curas” continuam **[nao verificado]**; são afirmações do docstring (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:3`). Cópia local não tem `.git`; `git show f31a8d09e` falhou **[nao verificado por arquivo:linha]**.
- **Limite real do selo de relógio:** `_chamadas_relogio` considera `localdate(valor)`/`localtime(valor)` conversões sempre que o argumento não seja o literal `None`, mesmo quando `valor` possa ser `None` em runtime (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:184`). Contraexemplo: `frescor_dos_retratos(agora=None)` satisfaz a assinatura obrigatória e chama `timezone.localtime(None)`, permitindo novo relógio (`app/folha/retrato_folha.py:1387`, `:1409`); o selo exige só ausência de chamadas sintáticas diretas e ausência de default (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:231`). **Observação de fragilidade preexistente ao c17 segundo o diff fornecido**, sem chamador real que forneça None demonstrado.
- **Dataset valida datetime, não awareness.** `dataset_furos(..., agora=datetime.datetime(2026,11,15))` passa pela guarda de tipo (`app/relatorios/furos.py:113`), mas downstream `timezone.localdate(agora)` exige instante aware (`app/ponto/status_dia.py:254`). Teste 07 não exercita instante naive (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:213`). **Observação de contrato incompleto**, não ocorrência em chamador real demonstrada.
- **Teste 07 não prova significado transitivo de “hoje”.** Seus HOJE e AGORA são propositalmente datas diferentes, com D4/calendário substituídos por mocks; ele prova identidade de transporte (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:25`, `:200`, `:219`). O próprio selo declara “sem fecho transitivo” (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:169`).
- **C5 não alarga []:** `colabs_nunca_bateu([], hoje)` usa `Empresa.objects.filter(pk__in=[])` (`app/escala/services/cadastro_realidade.py:163`). Tem fallback próprio `hoje=None -> localdate()` (`:137`, `:161`), mas o vigia injeta hoje explicitamente (`app/folha/vigia_retido_calado.py:72`), sem acioná-lo no caminho normal.
- **Teste c5 R2 não ficou stale:** ele espera TypeError por ausência de `hoje`, antes de qualquer import/ORM; o mock já possui `.filter` (`app/chamados/tests/test_e3_r3_c5_sem_banco.py:232`, `:236`, `:245`).
- **Limbo segue pendência antiga registrada**, não juiz novo criado por c17: `core/juizes.py` mantém o sítio e apenas atualiza a assinatura (`app/core/juizes.py:794`). Limbo consulta `eh_dia_trabalho(hoje)` e amanhã (`app/relatorios/furos.py:67`); `FolgaDia` mensal é diagnóstico de foto civil, não decisão de competência (`:86`, doc `:56`), portanto não é automaticamente violação L-090.

Leis relevantes para consolidar:

- Testemunha lê autoridade única: `app/docs/LEIS.md:60`; consumidor lê lâmpada: `:95`.
- Competência por corte da empresa, não calendário civil: `app/docs/LEIS.md:123`.
- Relógio uma vez na borda e parâmetro até julgamento: `app/docs/GLOSSARIO.md:19`, `app/docs/PLANO-PADROES.md:92`; default None/fallback interno é armadilha explicitada em `:94`.
- Vigia somente alarma, contador esperado zero: `CLAUDE.md:172`; um juiz e um escritor por domínio: `CLAUDE.md:178`.
- UTC no banco/apresentação local: `CLAUDE.md:146`; configuração `America/Sao_Paulo`, `USE_TZ=True`: `app/config/settings/base.py:93`, `:95`.
- Corte de publicação do Retrato não prova pré-requisitos de deploy pela inspeção desta cópia: `app/docs/CORTES.md:78`; ocorrência/deploy **[nao verificado]**.

Nenhum **PROCEDENTE novo de c17** demonstrado nesta fatia de selos/leis; as fragilidades de None/naive e a prova documental do caso 09 devem constar como observações com os limites acima.

## 15/11 22:38 · sessao 31006ac546a5 · - · - · origem=Ronald (tmux)

Inventário e conclusões do vigia:

- **Escritor persistente do retrato:** `lavrar_retrato_folha.handle` grava `empresa/mes/ano`, `linhas` e `lavrado_em` por `update_or_create`, somente com `--apply`; lê um único `_agora=timezone.now()`, deriva `_hoje=timezone.localtime(_agora).date()`, passa ambos a `montar_linhas`, e conserva `_agora` como início da rodada (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:50`, `:65`, `:72`, `:77`). Há constraint por empresa/competência (`app/folha/models.py:365`). Nos sítios de produção que nomeiam `RetratoFolha`, não encontrei segundo escritor; os demais usos são consultas (`app/folha/views_retrato.py:194`, `app/folha/retrato_folha.py:1413`, `app/colaboradores/views.py:982`, `app/folha/vigia_retido_calado.py:59`).

- **Competências:** escritor da lista = `competencias_do_retrato`; lê `janela_anterior` e `janela_atual`, extrai mês/ano do fim e elimina repetição (`app/folha/retrato_folha.py:1369`, `:1378`, `:1380`, `:1382`). Leitores de produção = lavrador automático, frescor e vigia (`app/folha/management/commands/lavrar_retrato_folha.py:61`; `app/folha/retrato_folha.py:1412`; `app/folha/vigia_retido_calado.py:66`). `--mes/--ano` é seleção explícita do lavrador, não derivação concorrente (`app/folha/management/commands/lavrar_retrato_folha.py:57`). O juiz respeita corte por empresa e datas inclusivas corte→corte−1 (`app/ponto/janelas.py:10`, `:16`, `:20`, `:60`, `:66`).

- **Universo mudou:** agora parte de todas as `empresas_visiveis`, inclusive sem nenhuma foto, intersecta IDs pedidos e consulta retratos só desses IDs (`app/folha/vigia_retido_calado.py:54`, `:55`, `:58`, `:59`, `:61`). `empresas_visiveis` exclui inativas e CNPJ diagnóstico `11222333*` (`app/colaboradores/queries.py:78`, `:85`). Antes, segundo o diff anexado, partia das empresas encontradas em retratos e C5 recebia recorte original apenas havendo foto; comparação independente via Git [nao verificado] porque a cópia não tem repositório Git. Agora pares faltantes saem até para empresa sem foto, ordenados empresa/ano/mês; C5 recebe sempre IDs efetivos (`app/folha/vigia_retido_calado.py:65`, `:69`, `:72`).

- **Quem escreve/lê o envelope de nove chaves:** só `censo` constrói e acumula `retratos`, `competencias_sem_retrato`, `bloqueados`, `calados`, `roteados`, `dias_sem_leitura_sistema`, `linhas_sem_campo_leitura`, `por_empresa` e `cego` (`app/folha/vigia_retido_calado.py:73`, `:88`, `:95`, `:100`, `:101`). `linhas` lê o envelope e emite texto; `handle` lê as chaves de alarme (`app/folha/vigia_retido_calado.py:105`; `app/folha/management/commands/vigiar_retido_calado.py:34`, `:38`). Trios das listas são `(empresa,mes,ano)`; chaves de `por_empresa` são `(empresa,ano,mes)`, distintos e coerentes no consumidor (`app/folha/vigia_retido_calado.py:88`, `:95`, `:108`, `:110`).

- **Unidades e desconhecimento:** bloqueados/calados/roteados contam pessoa por retrato/competência; dias sem leitura somam listas das linhas bloqueadas; chave ausente marca legado desconhecido, chave presente `None` soma zero; positivo conhecido sobrevive ao legado (`app/folha/vigia_retido_calado.py:77`, `:79`, `:80`, `:84`, `:85`, `:87`, `:100`). Tests efetivos desses contratos: `app/chamados/tests/test_e3_r3_c17_sem_banco.py:97`, `:107`, `:116`, `:129`, `:142`. Incluem interseção, empresa sem foto, alarme parcial, preservação do positivo, unidade repetida por competência e virada do ano (`:103`, `:110`, `:123`, `:150`, `:162`).

- **Relógio do vigia:** único chamador de produção encontrado é o command, que fornece `hoje=timezone.localdate()` uma vez; `censo` exige keyword `hoje` e o repassa a competência, dono e C5 (`app/folha/management/commands/vigiar_retido_calado.py:33`; `app/folha/vigia_retido_calado.py:41`, `:66`, `:72`, `:78`). `dono_da_linha` compara datas gravadas com o dia civil local, inclusive `>=hoje` para o balde Hoje; não decide fim de turno (`app/folha/retrato_folha.py:474`, `:480`, `:491`). Frescor e lavrador usam a mesma conversão local (`app/folha/retrato_folha.py:1409`; `app/folha/management/commands/lavrar_retrato_folha.py:50`). Drawer também usa instante convertido localmente antes de `.date()` (`app/colaboradores/views.py:930`, `:986`).

- **Fallbacks existentes, não acionados pelo caminho normal:** `colabs_nunca_bateu(hoje=None)` lê relógio internamente (`app/escala/services/cadastro_realidade.py:137`, `:161`), mas vigia fornece o dia (`app/folha/vigia_retido_calado.py:72`). `montar_linhas(hoje=None)`→`_informativo_rescisao`→`informativo_rescisao`→`_dias_sem_decisao` pode ler relógio no interior; dívida expressamente declarada (`app/folha/retrato_folha.py:1174`, `:1220`, `:1271`; `app/folha/export.py:1216`, `:1238`). Lavrador fornece dia e evita fallback (`app/folha/management/commands/lavrar_retrato_folha.py:65`).

- **Continua vigia:** sem escrita ORM nem transição no `censo`; só contagem e retorno; command apenas stdout e `SystemExit(2)` (`app/folha/vigia_retido_calado.py:59`, `:77`, `:102`; `app/folha/management/commands/vigiar_retido_calado.py:35`, `:42`). Cron chama com `--alarme`, depois do retrato, declarado papel vigia (`app/config/crons.py:1681`, `:1683`, `:2260`). `calados` e dias continuam esperado zero, mas só entram no alarme com `--alarme-passivo`; alarme estrutural inclui agora cego e competência sem foto (`app/folha/management/commands/vigiar_retido_calado.py:16`, `:17`, `:37`). Coerente com L-060/CLAUDE 4a (`app/docs/LEIS.md:94`; `CLAUDE.md:172`).

**Observação concreta:** comentário do cron ficou velho: diz `--alarme` “só pelo cego”, mas falta parcial de retrato também produz rc2 no command (`app/config/crons.py:1679`; `app/folha/management/commands/vigiar_retido_calado.py:16`, `:38`, `:42`). Contraexemplo já construído pelo teste: foto só de uma das duas competências, bloqueados positivos e cego zero, mesmo assim alarma `competencias_sem_retrato` (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:117`, `:119`, `:123`).

**PROCEDENTES de comportamento neste subconjunto:** nenhum identificado. Não há evidência disponível de número real de produção nem de execução dos testes com banco [nao verificado]. [nao verificado]

## 15/11 22:38 · sessao d907d4904e3b · - · - · origem=Ronald (tmux)

Inventário dos alvos:

| Alvo | Quem fornece / quem lê | Assinatura antiga |
|---|---|---|
| `escalas_em_limbo(empresa=None, *, hoje)` | Produção: `furos_espelho` fornece `hoje` em `app/relatorios/views.py:1554`; a borda lê `now()` uma vez e converte com `localtime(...).date()` em `:1519-1520`. A função pergunta ao juiz `ec.eh_dia_trabalho` para hoje e amanhã em `app/relatorios/furos.py:67` e usa o mês civil da foto `FolgaDia` em `:86-89`. | Não identificada. Teste atualizado em `app/relatorios/tests/test_selo_ancora_do_vinculo.py:45`, com dia fixo em `:23`. |
| `dataset_furos(..., *, agora)` | Produção: `app/relatorios/views.py:1543` fornece `_agora`; dataset envia o mesmo objeto ao D4 em `app/relatorios/furos.py:127` e ao calendário em `:168`. | Não identificada. Testes em `app/relatorios/tests/test_e4_8_furos_le_a_lavra.py:94` e `app/ponto/tests/test_cobparc_v4_procedentes.py:288` fornecem instante. |

O D4 deriva o dia com `timezone.localdate(agora)`; o calendário deriva com `timezone.localtime(_agora).date()`. Ambos usam o fuso ativo; a configuração padrão é `America/Sao_Paulo`, `USE_TZ=True`. São conversões equivalentes para um instante aware no mesmo contexto. (`app/ponto/status_dia.py:252-254`; `app/colaboradores/services/calendario.py:192-193`; `app/config/settings/base.py:93-95`.)

Os intervalos do dataset são inclusivos nas duas pontas; o julgamento de encerramento compara `agora >= fim` e usa `data_turno` como dia do turno. Portanto dia civil e dia do turno são insumos distintos, sem evidência de confusão nova neste caminho. (`app/relatorios/furos.py:126`; `app/ponto/status_dia.py:273-274`; `app/ponto/turnos.py:2111`; `app/ponto/turnos.py:2124`.)

As exclusões têm caminhos reais:

- PDF do retrato: `retrato_pdf` chama `_recorte_da_barra` e depois `gerar_pdf_auditavel`, sem transmitir o instante do recorte. A função auditável lê `timezone.now()` e usa esse valor nos rodapés das duas passagens; também grava um `RegistroPDFAuditavel`. Isso não altera a seleção das linhas, já capturada em `_rec`. (`app/folha/views_retrato.py:388`; `:443`; `app/relatorios/pdf_base.py:133`; `:139`; `:152`; `:170`.)
- Aceite: `retrato_raiox` chama `_recorte_da_barra`, mas transmite somente `empresa_id` ao placar quando Dono=Colab. `placar_aceite` lê `now()` e `localdate()` separadamente, permitindo atravessar meia-noite entre as leituras. Ele usa janelas próprias hoje/7d e não recebe competência, praça, pessoas ou intervalo do recorte. (`app/folha/views_retrato.py:489`; `:513-515`; `app/chamados/services/verbos.py:277-288`; `:295-301`.)
- O PDF da auditoria de espelhos passa os cards a `gerar_pdf_furos_bytes`; não usa nenhuma dessas duas funções excluídas. (`app/relatorios/views.py:1562-1564`; `app/relatorios/furos_pdf.py:73-103`.)

A exclusão é coerente com a promessa limitada de transporte direto, expressamente “sem fecho transitivo”; não permite afirmar relógio único em toda a tela e seus componentes. O reuso do placar no Raio-X é ordem expressa do corte O256. (`app/folha/views_retrato.py:146-147`; `app/relatorios/furos.py:111`; `app/docs/CORTES.md:309`.)

Não confundir o mês civil da foto `FolgaDia` com competência de folha: a função diagnostica cadastro e consulta `eh_dia_trabalho`, sem construir competência. A leitura da foto é nominalmente permitida na catraca, e o registro FABRICANTE explica que substituir por `janela_atual` contrariaria o contrato da foto. (`app/relatorios/furos.py:67`; `:86-89`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:169`; `app/docs/FABRICANTE.md:63`.)

Os testes C17 do limbo e dataset exercitam comportamento: conferem o dia recebido, o mês da consulta, ausência de leitura do mock de relógio, obrigatoriedade do parâmetro, recusa dos tipos inválidos e identidade do instante nos dois fornecedores. Já o item 09 cobra somente termos dos docstrings. (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:183-191`; `:212-223`; `:252-269`.)

Observações:

- A validação de `dataset_furos` aceita qualquer `datetime`, inclusive naive. Os leitores seguintes exigem conversão local de instante aware; o contrato do juiz também documenta aware. Nenhum chamador real passando naive foi identificado, portanto isso é limite de validação, não regressão demonstrada. (`app/relatorios/furos.py:113-114`; `app/ponto/status_dia.py:254`; `app/ponto/turnos.py:2071`.)
- D4 e calendário conservam fallback de relógio com `agora=None`, mas o caminho atualizado do dataset transmite instante obrigatório e não aciona esses fallbacks. (`app/ponto/status_dia.py:252-254`; `app/colaboradores/services/calendario.py:192`; `app/relatorios/furos.py:103`; `:127`; `:168`.)
- `escalas_em_limbo` permanece registrado em PENDENTES; o diff ajusta sua âncora à assinatura nova. O critério atual já consulta o juiz, e a leitura de vínculos abertos é declarada como diagnóstico de cadastro. (`app/core/juizes.py:794`; `app/relatorios/furos.py:67`; `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:207-208`.)

Achados PROCEDENTES novos de execução nesses dois alvos: nenhum demonstrado pela leitura acima; ocorrência em produção [nao verificado].

_5846 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
