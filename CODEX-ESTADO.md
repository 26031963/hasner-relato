# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 14:31** · publicado em **16/11/2026 14:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 14:31 · sessao 920e00533953 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

Referências `script.py:N` indicam linhas da **saída fornecida**, contando a primeira `sonda_leitura` como linha 1; no anexo, `aval Ronald` é a linha 1.

1. **O199:** 0 em 09/2026, 0 em 10/2026 e 0 em 11/2026. Nenhum caso individual aparece listado. (`script.py:4`, `script.py:5`, `script.py:6`)

2. **SC11 (col305):** o total informado para 11/2026 é **0**. Quantidades separadas de dias sem célula e com célula julgada errada: **[nao verificado]**, pois não há detalhamento desses grupos. A cobertura informa apenas `existentes_ate_hoje_sem_juizo=0`; cobertura integral de novembro e julgamento dos dias sem célula: **[nao verificado]**. (`script.py:7`, `script.py:8`)

3. **Erros:** não aparece linha `ERRO`; os totais zero não apresentam divergência visível com o detalhamento vazio; `rc_sombra=0`. Não há erro explícito na saída que invalide os números reportados. (`script.py:4`, `script.py:5`, `script.py:6`, `script.py:7`, `script.py:8`, `script.py:9`)

O destino por competência e a exigência de **! de escala** vêm do corte C. (`prioridade_espelho_verdade_tres_cortes_1611.txt:13`)

| obra | competencia | n | exportada | destino |
|---|---|---:|---|---|
| O199 | 09/2026 | 0 (`script.py:4`) | Sim, conforme enunciado | lista DP |
| O199 | 10/2026 | 0 (`script.py:5`) | Sim, conforme enunciado | lista DP |
| O199 | 11/2026 | 0 (`script.py:6`) | Não; aberta, conforme enunciado | apply na 11 com **! de escala** |
| SC11 (col305) | 11/2026 | 0 (`script.py:7`) | Não; aberta, conforme enunciado | apply na 11 com **! de escala** |

## 16/11 14:30 · sessao 114bf448442c · PRIORIDADE-EV-ITEM4 · leitura · origem=Code

**O dado fornecido não permite fechar nenhuma das três obras por “uso > 0 e erro 0”: as contagens posteriores aos pousos permanecem [nao verificado].** A cópia permite montar a sonda, mas a auditoria não guarda URL nem status HTTP, e vários endpoints compartilham o mesmo escritor. (`app/core/models.py:295-307`, `app/ponto/portas/he.py:263-268`, `app/chamados/services/acoes_disputa.py:113-114`)

**1. Portas e evidências**

Os prefixos são `/ponto/`, `/chamados/`, `/relatorios/` e `/api/`. (`app/config/urls.py:28-37`)

| Obra / porta | Rota, view | Uso persistido e escritor | Erro ou recusa persistidos |
|---|---|---|---|
| O-JANELA-HE — listagem | `ponto:gestao_he`; `/ponto/gestao-he/`; `gestao_he`. (`app/ponto/urls.py:17`, `app/ponto/views.py:887`) | A view lê e renderiza, sem gravar abertura. O retrato existente foi escrito pelo lavrador, não pelo acesso à tela. (`app/ponto/views.py:932-966`, `app/ponto/services/he_pendente_lavrado.py:177-189`) | `MetricaSnapshot.detalhe["erros"]`: falhas de apuração na **foto atual**, não erros HTTP nem histórico desde o pouso. Incremento em `he_pendente_lavrado.py:94-100`; gravação em `:177-189`; campos em `app/inteligencia/models.py:95-96`. |
| O-JANELA-HE — Autorizar/Não diário | `ponto:decidir_he`; `/ponto/gestao-he/decidir/`; `decidir_he`. (`app/ponto/urls.py:19`, `app/ponto/views.py:499`) | `DecisaoHE.estado`, `minutos_na_decisao`, `origem`, timestamps; criação/alteração em `_gravar`. Histórico: `LogAuditoria`, ação `decidir_he`, modelo `DecisaoHE`, `valor_antes/depois`, `criado_em`. (`app/ponto/models.py:1300-1317`, `app/ponto/portas/he.py:66-99`, `app/core/models.py:295-307`) | `estado="nao"` é ciência/decisão de negócio. A exceção `DecisaoHERecusada` apenas vira resposta da view; não há gravação desse retorno nesses caminhos. (`app/ponto/models.py:1283-1288`, `app/ponto/portas/he.py:175-193`, `app/ponto/views.py:535-537`) |
| O-JANELA-HE — Não em lote | `ponto:recusar_he_em_lote`; `/ponto/gestao-he/recusar-lote/`; `recusar_he_em_lote`. (`app/ponto/urls.py:21`, `app/ponto/views.py:549`) | Chama a mesma porta diária, produzindo a mesma trilha `decidir_he`, por dia alterado. **Não identifica o lote/URL.** (`app/ponto/portas/he.py:263-273`, `app/ponto/portas/he.py:94-99`) | Recusas ficam numa lista em memória e na resposta; itens malformados também só aparecem na resposta. (`app/ponto/portas/he.py:267-268`, `app/ponto/views.py:621-629`) |
| O-JANELA-HE — PDF | `ponto:gestao_he_pdf`; `/ponto/gestao-he/pdf/`; `gestao_he_pdf`. (`app/ponto/urls.py:27`, `app/ponto/views.py:772`) | `RegistroPDFAuditavel.tipo="outro"`, `filtros_json`, `gerado_em`; escritor em `pdf_base.py:152-166`. A chamada usa filtros `empresa/mes/ano/q/praca/posto/estado`, mas não persiste a rota. (`app/ponto/views.py:870-878`, `app/relatorios/models.py:36-55`) | O registro precede a segunda montagem do PDF; sua existência não prova conclusão nem entrega HTTP. (`app/relatorios/pdf_base.py:152-180`) |
| O-JANELA-HE — atalho na Central | `relatorios:index`; `/relatorios/`; `index`. (`app/relatorios/urls.py:9`, `app/relatorios/views.py:22`) | Lê contador e renderiza, sem trilha de abertura nessa view. (`app/relatorios/views.py:33-51`) | Falha do contador chama `logger.exception`; não grava erro de acesso no banco. (`app/relatorios/views.py:44-50`) |
| BO-HE-RECUSAR-LOTE-500 | As portas de decisão diária e recusa em lote acima; a listagem é a entrada da tela. (`app/docs/BACKLOG.md:1737`, `app/ponto/urls.py:17-21`) | As mesmas evidências anteriores, recortadas pelo pouso desta obra. A transação está na porta, antes do `select_for_update`. (`app/ponto/portas/he.py:151-159`, `app/ponto/portas/he.py:195`) | O caminho não registra 5xx no banco; o tratamento das recusas permanece em retorno/UI. (`app/ponto/views.py:535-537`, `app/ponto/views.py:621-629`, `app/config/settings/saas.py:173-181`) |

O pouso da O-JANELA nomeia as quatro rotas da Gestão de HE; os “13 passos” são referidos nesse registro. A inclusão exata de outras telas nos 13 passos é [nao verificado]. (`app/docs/RELATO-ARQUIVO.md:28278-28285`)

Na REABRIR, **o modelo atual é `DecisaoPergunta`**: a migration renomeou `RespostaAnterior`, preservou o instante como `ato_em` e atribuiu `ato="reabertura"` às linhas existentes. (`app/chamados/migrations/0051_decisao_pergunta.py:41-61`)

| Porta / fluxo REABRIR-NAO-DESTROI | Rota, view | Uso persistido e escritor | Erro ou recusa |
|---|---|---|---|
| Reabrir pela supervisão | `chamados:reabrir_disputa_supervisao`; `/chamados/local/<id>/reabrir-disputa/`; `reabrir_disputa_supervisao`. (`app/chamados/urls.py:20`, `app/chamados/views.py:1372`) | `DecisaoPergunta(ato="reabertura", ato_em)`; preparação em `disputa_emissao.py:1631-1646`, INSERT em `models.py:4120`. Auditoria `reabrir_disputa/DisputaSupervisao` em `views.py:1456`. | Sem permissão/sem disputa/sem pergunta corrigível viram respostas; não são gravados como recusas nesses ramos. (`app/chamados/views.py:1403-1434`) |
| Reabrir pelo fio/modal | `chamados:fio_colaborador`, `/chamados/colaborador/<id>/`, `fio_colaborador`; `chamados:modal_fio`, `/chamados/modal/<id>/`, `modal_fio_colaborador`. (`app/chamados/urls.py:21,39`, `app/chamados/views.py:659,869`) | POST delegado ao fio; mesma reabertura e mesma auditoria, sem distinguir a URL. (`app/chamados/views.py:667-674,951-954`, `app/chamados/services/fio.py:679-682`, `app/chamados/services/acoes_disputa.py:102-114`) | As recusas anteriores ao ato só retornam mensagem. (`app/chamados/services/acoes_disputa.py:95-101`) |
| Pedir correção — HTML/API | `chamados:solicitar_correcao_disputa`, `/chamados/disputa/<pk>/solicitar-correcao/`; `api:api_solicitar_correcao_disputa`, `/api/disputa/<pk>/solicitar-correcao/`. Views em `app/chamados/views.py:1317` e `app/api/views.py:2670`; rotas em `app/chamados/urls.py:19`, `app/api/urls.py:94`. | Ambas usam `reabrir_por_colab`; grava snapshot, contador e nota `[Solicitacao de correcao - `. HTML/API ficam indistinguíveis nessa evidência. (`app/chamados/views.py:1334`, `app/api/views.py:2686`, `app/chamados/models.py:2318-2325`) | Fechada/limite/sem resposta retornam estado, sem evento de recusa nesse método. (`app/chamados/models.py:2304-2312`) |
| Corrigir administrativamente | POST do modal com `acao=corrigir_resposta`; também fio e detalhe local. (`app/templates/chamados/partials/modal_fio.html:301-312`, `app/chamados/services/fio.py:663-677`, `app/chamados/views.py:525-536`) | Auditoria `corrigir_resposta_admin/PerguntaDisputa`, após gravar a resposta, **antes da validação/materialização**. (`app/chamados/services/acoes_disputa.py:370-383`) | `LogAuditoria(acao="val_perg_nao_gravou", modelo="PerguntaDisputa")` registra recusa de materialização, compartilhada com outras validações. (`app/chamados/services/validacao.py:513-521`) |
| Cobrar resposta | `chamados:cobrar_chamado`; `/chamados/cobrar/<cid>/`; `cobrar_chamado`. (`app/chamados/urls.py:25`, `app/chamados/views_cobrar.py:24`) | Nota `FollowupChamado.conteudo`, `criado_em`, prefixo `[COBRANCA]`; chamada em `views_cobrar.py:54-56`, INSERT em `services/nota.py:103,107`; campos em `models.py:1732-1738`. | Permissão, área, limite diário e falha de push retornam antes da nota. (`app/chamados/views_cobrar.py:25-53`) |
| Recobrar questionário pendente | Mesma rota `reabrir_disputa_supervisao`, ramo pendente. (`app/chamados/views.py:1421-1425`) | Auditoria `recobrar_questionario/DisputaSupervisao` e nota `[Re-cobranca do questionario - `. (`app/chamados/services/disputa_emissao.py:1741-1759`) | Falhas de nota/push usam `engolir`, que registra logging e contador de processo. (`app/chamados/services/disputa_emissao.py:1741-1754`, `app/core/observ.py:73-91`) |
| Encerrar/regularizar | POST do modal com `acao=regularizar_disputa`; também fio e `chamados:detalhe_local`, `/chamados/local/<id>/`, `detalhe_chamado_local`. (`app/templates/chamados/partials/modal_fio.html:353-359`, `app/chamados/urls.py:14`, `app/chamados/views.py:359,469-475`) | Auditoria `regularizar_disputa_granular/DisputaSupervisao`. (`app/chamados/services/fio.py:713-714`, `app/chamados/services/acoes_disputa.py:144-145`) | A descrição guarda `Mat=N Pend=N Fechada=B Erros=N`; guardas anteriores ao ato só retornam mensagem. (`app/chamados/services/fio.py:685-714`, `app/chamados/services/acoes_disputa.py:123-145`) |
| Abrir o histórico no modal | GET `chamados:modal_fio`, acima. (`app/chamados/urls.py:39`) | Carimba `FollowupChamado.lido_dp_em`, mas somente notas ainda não lidas; não é contador de aberturas nem evidência exclusiva desta tela. (`app/chamados/views.py:877-880`, `app/chamados/services/nota.py:126-133`, `app/chamados/models.py:1736`) | A presença do histórico não prova sua renderização; o template chama o leitor de respostas anteriores. (`app/templates/chamados/partials/modal_fio.html:204-205`, `app/chamados/models.py:3667-3683`) |

Uma reabertura produz linhas **por pergunta afetada**, não por clique; pendente limpa não produz versão. Decisão HE repetida também retorna sem trilha nova. (`app/chamados/services/disputa_emissao.py:1631-1648`, `app/ponto/portas/he.py:197-204`)

**2. Texto da sonda**

O script mantém `[nao verificado]` para contagens HTTP sem atribuição; imprime contagens separadas das portas de domínio compartilhadas. Para os horários incompletos, calcula limites: se houver registros dentro do minuto/faixa incerta, informa `n_min/n_max`, sem inventar o instante do pouso. Os horários publicados são os do backlog. (`app/docs/BACKLOG.md:1208`, `app/docs/BACKLOG.md:1737`, `app/docs/BACKLOG.md:1168`)

O envelope deve estar previamente inicializado; banco PROD `saas_hasner`, role `leitor` e tenant `juliani` correspondem ao envelope da cópia. **O wrapper não deve ser executado nesta tarefa:** ele próprio contém criação de arquivos e administração da role. (`bin/sonda_leitura.sh:28-29,42-46,216-218,239-240,248-250`)

```python
# OBRA: PRIORIDADE-EV-ITEM4
# Somente TEXTO para envelope PROD previamente inicializado; nao executado.
# Execucao, desempenho, Django/PostgreSQL e compatibilidade em PROD:
# [nao verificado].
# Nao chama views, servicos, juizes, lavradores nem django.setup().
# Biblioteca padrao e implementacoes externas: [nao verificado].
#
# Ambiente esperado: bin/sonda_leitura.sh:42-46,237-240.
# Datas publicadas: app/docs/BACKLOG.md:1208,1737,1168.
# "x" nao e convertido em minuto exato: contam-se limites inferior/superior.
# Offset -03:00 aplicado ao horario local informado: [nao verificado]. [nao verificado]
# TOTAL conta todas as linhas emitidas ANTES da propria linha TOTAL.
# Falha de sonda nunca e rotulada como erro ocorrido no produto.

import sys
sys.dont_write_bytecode = True

import datetime as dt
import logging
import re
from contextlib import redirect_stdout, redirect_stderr
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout
LINHAS = []

JANELA = "O-JANELA-HE"
BO = "BO-HE-RECUSAR-LOTE-500"
REABRIR = "REABRIR-NAO-DESTROI"

# Rotulo do pouso, menor instante possivel, primeiro instante certamente posterior. [nao verificado]
# Segundos exatos dos pousos: [nao verificado].
CORTES = {
    JANELA: (
        "2026-10-01T09:2x-03:00",
        "2026-10-01T09:20:00-03:00",
        "2026-10-01T09:30:00-03:00",
    ),
    BO: (
        "2026-10-22T10:26-03:00",
        "2026-10-22T10:26:00-03:00",
        "2026-10-22T10:27:00-03:00",
    ),
    REABRIR: (
        "2026-10-25T07:4x-03:00",
        "2026-10-25T07:40:00-03:00",
        "2026-10-25T07:50:00-03:00",
    ),
}

# Endpoints HTTP: nao converter trilha compartilhada em acesso atribuido.
# Rotas/views:
# app/ponto/urls.py:17-27; app/ponto/views.py:499,549,772,887.
# app/relatorios/urls.py:9; app/relatorios/views.py:22-51.
# app/chamados/urls.py:14,19-21,25,39.
# app/api/urls.py:94; app/api/views.py:2670.
PORTAS = {
    JANELA: (
        ("relatorios:index", "app/relatorios/views.py:33-51"),
        ("ponto:gestao_he", "app/ponto/views.py:932-966"),
        ("ponto:decidir_he", "app/ponto/portas/he.py:94-99"),
        ("ponto:recusar_he_em_lote", "app/ponto/portas/he.py:263-268"),
        ("ponto:gestao_he_pdf", "app/ponto/views.py:870-878"),
    ),
    BO: (
        ("ponto:gestao_he", "app/ponto/views.py:932-966"),
        ("ponto:decidir_he", "app/ponto/portas/he.py:94-99"),
        ("ponto:recusar_he_em_lote", "app/ponto/portas/he.py:263-268"),
    ),
    REABRIR: (
        ("chamados:reabrir_disputa_supervisao",
         "app/chamados/views.py:1435-1456"),
        ("chamados:fio_colaborador",
         "app/chamados/views.py:667-674"),
        ("chamados:modal_fio",
         "app/chamados/views.py:877-880"),
        ("chamados:modal_fio@corrigir_resposta",
         "app/chamados/services/acoes_disputa.py:375-383"),
        ("chamados:modal_fio@regularizar_disputa",
         "app/chamados/services/fio.py:713-714"),
        ("chamados:detalhe_local",
         "app/chamados/views.py:469-475"),
        ("chamados:solicitar_correcao_disputa",
         "app/chamados/models.py:2318-2325"),
        ("api:api_solicitar_correcao_disputa",
         "app/api/views.py:2686"),
        ("chamados:cobrar_chamado",
         "app/chamados/views_cobrar.py:54-56"),
    ),
}


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


def classe_segura(exc):
    nome = type(exc).__name__
    return nome if re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]{0,79}", nome) else "Excecao"


def adicionar(*campos):
    LINHAS.append("|".join(str(c) for c in campos))


def emitir_medida(tipo, obra, porta, valor, extras=(), subtipo=None):
    campos = [tipo, obra, porta]
    if subtipo is not None:
        campos.append(subtipo)
    if tipo == "USO":
        campos.extend(("desde=" + CORTES[obra][0], "n=" + str(valor)))
    elif tipo in ("ERRO", "RECUSA"):
        campos.append(str(valor))
    else:
        campos.append("n=" + str(valor))
    campos.extend(extras)
    adicionar(*campos)


def principais():
    # Ausencia de campo URL/status: app/core/models.py:295-307.
    for obra, portas in PORTAS.items():
        for porta, ref in portas:
            emitir_medida(
                "USO", obra, porta, NV,
                ("atribuicao_http=nao_disponivel", "ref=" + ref),
            )
            emitir_medida(
                "ERRO", obra, porta, NV,
                ("cobertura=banco_nao_conta_http",
                 "ref=app/config/settings/saas.py:173-181"),
                subtipo="5xx_traceback",
            )

    # Recusas do POST, distintas de decisao de negocio "Nao".
    # app/ponto/views.py:535-537,621-629.
    for obra in (JANELA, BO):
        for porta in ("ponto:decidir_he", "ponto:recusar_he_em_lote"):
            emitir_medida(
                "RECUSA", obra, porta, NV,
                ("criterio=nao_ha", "escopo=recusa_do_post_nao_persistida",
                 "ref=app/ponto/views.py:535-537,621-629"),
            )


def sonda():
    principais()
    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, transaction

            # Configuracao tenant-aware: app/config/settings/saas.py:83-95.
            # Identidade real do envelope e GRANTs em PROD: [nao verificado].
            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.saas"
                or getattr(settings, "BANCO_DE_ENSAIO", False)
                or connection.settings_dict["NAME"] != "saas_hasner"
                or connection.schema_name != "juliani"
                or connection.in_atomic_block
                or not connection.get_autocommit()
            ):
                raise RuntimeError("AmbienteIncompativel")

            # Modelos/campos efetivamente consultados:
            # LogAuditoria: acao, modelo, valor_depois, descricao, criado_em
            # app/core/models.py:295-307.
            # Escritor efetivo: app/core/observ.py:133-138.
            Log = apps.get_model("core", "LogAuditoria")

            # DecisaoPergunta: ato, ato_em.
            # app/chamados/models.py:4039-4040.
            # Rename de RespostaAnterior:
            # app/chamados/migrations/0051_decisao_pergunta.py:41-61.
            # Escritor: app/chamados/models.py:4110-4122.
            Decisao = apps.get_model("chamados", "DecisaoPergunta")

            # FollowupChamado: conteudo, criado_em, lido_dp_em.
            # app/chamados/models.py:1735-1738.
            # Escritores: app/chamados/services/nota.py:103,107,126-133.
            Nota = apps.get_model("chamados", "FollowupChamado")

            # MetricaSnapshot: chave, escopo, detalhe, calculado_em.
            # app/inteligencia/models.py:87-96.
            # Escritor: app/ponto/services/he_pendente_lavrado.py:177-189.
            Snapshot = apps.get_model("inteligencia", "MetricaSnapshot")

            # RegistroPDFAuditavel: tipo, filtros_json, gerado_em.
            # app/relatorios/models.py:36-55.
            # Escritor: app/relatorios/pdf_base.py:152-166.
            PDF = apps.get_model("relatorios", "RegistroPDFAuditavel")

            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                    )
                    cursor.execute("SET LOCAL statement_timeout = '30s'")
                    cursor.execute(
                        "SELECT current_database(), current_user, session_user, "
                        "current_schema(), "
                        "current_setting('transaction_read_only'), "
                        "CURRENT_TIMESTAMP"
                    )
                    banco, papel, sessao, schema, leitura, fim = cursor.fetchone()
                    if (banco, papel, sessao, schema, leitura) != (
                        "saas_hasner", "leitor", "leitor", "juliani", "on"
                    ):
                        raise RuntimeError("BancoPapelOuTenantIncompativel")

                def medir(tipo, obra, porta, qs, campo, ref, *,
                          subtipo=None, extras=(), reduzir=None):
                    # Conta somente fatos anteriores ao inicio desta transacao.
                    # Duas leituras na mesma fotografia transacional.
                    try:
                        with transaction.atomic():
                            _, texto_lo, texto_hi = CORTES[obra]
                            lo = dt.datetime.fromisoformat(texto_lo)
                            hi = dt.datetime.fromisoformat(texto_hi)
                            base = qs.filter(**{campo + "__lt": fim})
                            desde_lo = base.filter(**{campo + "__gte": lo})
                            desde_hi = base.filter(**{campo + "__gte": hi})
                            maior = (reduzir(desde_lo) if reduzir
                                     else desde_lo.count())
                            menor = (reduzir(desde_hi) if reduzir
                                     else desde_hi.count())
                            if menor < 0 or maior < menor:
                                raise RuntimeError("ContagemIncompativel")
                        valor = menor if menor == maior else NV
                        emitir_medida(
                            tipo, obra, porta, valor,
                            (
                                "desde=" + CORTES[obra][0]
                                if tipo != "USO" else "unidade=fatos_persistidos",
                                "n_min=" + str(menor),
                                "n_max=" + str(maior),
                                "n_janela_incerta=" + str(maior - menor),
                                "ref=" + ref,
                            ) + tuple(extras),
               

[truncado em 20000 de 31626 caracteres]

## 16/11 14:28 · sessao 8531c1e56279 · PRIORIDADE-EV-ITEM6 · leitura · origem=Code

1. **O199:** o juiz da pendência é `o_dia_ja_e_folga`, usado pelo próprio aceite; ele delega a `folga_lancada_do_dia`, registrado em JUIZES. Universo: respostas `FOLGA_CONTESTA` aceitas (`validada_em` preenchido), deduplicadas por colaborador e `data_evento`. (`app/ponto/services/declarar_dia.py:78`, `app/chamados/services/validacao.py:305`, `app/docs/JUIZES.md:127`, `app/chamados/services/validacao.py:259`, `app/chamados/services/validacao.py:159`, `app/chamados/services/validacao.py:293`.)

2. **SC11:** a obrigação de existir célula vem de `vinculo_cobre_a_data`; `vinculo_do_dia` identifica seu dono. Universo: dias de col305 na competência empresarial 11/2026 cobertos por vínculo, sem filtro por situação. (`app/docs/JUIZES.md:135`, `app/docs/JUIZES.md:137`, `app/escala/alimentacao.py:205`, `app/escala/alimentacao.py:228`, `app/ponto/portas/celula.py:870`.)

**Limite do pedido:** juiz geral de “célula errada” para toda a competência: `[nao verificado]`. O medidor existente julga futuras; comparar o template atual com células passadas não certifica erro. Por isso o script distingue casos confirmados de cobertura incompleta e não publica total integral falso. (`app/ponto/services/vigia_cadastro_futuro.py:24`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/management/commands/tripwire_celulas.py:4`.)

O estado abaixo é o **gravado na célula**; status atual pelo juiz e possibilidade de declarar ficam `[nao verificado]`: seus caminhos alcançam carregadores/cache vedados pelo molde. Exportada significa `competencia_entregue`, sem confundir com fechamento por holerite. (`app/escala/models.py:1816`, `app/ponto/status_dia.py:270`, `app/ponto/services/declarar_dia.py:120`, `app/ponto/catalogo/ausencias.py:279`, `app/folha/export.py:556`, `app/folha/export.py:597`.)

```python
# OBRA: PRIORIDADE-EV-ITEM6
# Copia solicitada: a5765647153708b3fdd1c5dae4e6003bbc8fb4bf.
# Texto para envelope JA inicializado em SOMBRA / juliani / leitor.
# Nao executado; compatibilidade, desempenho e resultados: [nao verificado].
# Auxiliares deste texto, biblioteca padrao e internos do Django:
# [nao verificado].
#
# O199: inventario por competencia; 09/10 somente lista do DP.
# Elegibilidade para eventual apply restrita a 11, conforme anexo ITEM 6.
# Nenhuma porta de escrita e chamada.
#
# SC11:
# - ausentes: obrigacao de existencia pelo juiz de cobertura;
# - existentes futuras: medidor real, chamado numa janela de um dia;
# - existentes ate hoje: juizo geral de erro [nao verificado].
# TOTAL_PARCIAL conta linhas confirmadas.
# TOTAL recebe [nao verificado] quando a cobertura nao permite total integral.
#
# NAO chamar:
# checar_declaracao -> veto_de_lancamento -> ausencias_sobrepostas
# -> TIPOS_EM_ABERTO -> vigente, que escreve cache.
# app/ponto/services/declarar_dia.py:120,147
# app/ponto/precedencia.py:461
# app/ponto/turnos.py:2843
# app/ponto/catalogo/ausencias.py:267,279-280,406,430
#
# NAO chamar status_do_dia_lavrado com carga/insumos fabricados.
# app/ponto/status_dia.py:201,221,270,273,298
#
# NAO chamar lavrar(medida).
# app/ponto/services/vigia_cadastro_futuro.py:99-108

import sys
sys.dont_write_bytecode = True

import datetime as dt
import json
import logging
from collections import Counter
from contextlib import redirect_stderr, redirect_stdout
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout
ALVO = (2026, 11)


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


def emitir(*campos):
    print("|".join(str(c) for c in campos), file=SAIDA, flush=True)


def compacto(valor):
    return json.dumps(
        valor, ensure_ascii=True, allow_nan=False, separators=(",", ":")
    )


def consumir(qs):
    it = qs.iterator(chunk_size=128)
    try:
        return list(it)
    finally:
        it.close()


def competencia(ano_mes):
    ano, mes = ano_mes
    return "%02d/%04d" % (mes, ano)


def sonda():
    linhas = []
    totais_o199 = Counter({(2026, 9): 0, (2026, 10): 0, ALVO: 0})
    total_sc11 = 0
    sem_data_o199 = 0
    existentes_sem_juizo = 0

    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, transaction
            from django.utils import timezone

            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.settings_dict["HOST"] != "sombra_db"
                or connection.schema_name != "juliani"
            ):
                raise RuntimeError("AmbienteIncompativel")

            # Configuracao: app/config/settings/sombra.py:47-50,65.
            # Inicializacao efetiva do envelope: [nao verificado].

            # Predicado exato do aceite:
            # app/chamados/services/validacao.py:259-261.
            from chamados.services.validacao import _e_aceite_de_folga

            # Pergunta usada pela porta real de aceite:
            # app/ponto/services/declarar_dia.py:78-95.
            # Chamador real: app/chamados/services/validacao.py:305.
            from ponto.services.declarar_dia import o_dia_ja_e_folga

            # Juiz de competencia exportada:
            # app/folha/export.py:556-594.
            from folha.export import competencia_entregue

            # Competencia empresarial, sem corte 21 duplicado:
            # app/ponto/janelas.py:54-63.
            from ponto.janelas import janela_atual, janela_fechamento

            # Carregadores reais:
            # app/escala/alimentacao.py:54-78,163-186.
            # Juizes:
            # app/escala/alimentacao.py:205-216,228-286.
            from escala.alimentacao import (
                celulas_do_periodo,
                escalas_do_periodo,
                vinculo_cobre_a_data,
                vinculo_do_dia,
            )

            # Leitor real; nao importar/chamar lavrar:
            # app/ponto/services/vigia_cadastro_futuro.py:24-87.
            from ponto.services.vigia_cadastro_futuro import medir

            # Modelos:
            # app/chamados/models.py:2511,2528,2552-2554.
            # app/colaboradores/models.py:283.
            # app/escala/models.py:1635,1681-1695,1813-1816.
            Pergunta = apps.get_model("chamados", "PerguntaDisputa")
            Colaborador = apps.get_model("colaboradores", "Colaborador")
            CelulaDia = apps.get_model("escala", "CelulaDia")

            status_permitidos = {k for k, _ in CelulaDia.STATUS_DIA_CHOICES}

            def estado_gravado(cel):
                # Colunas reais; nenhuma chamada ao juiz de status.
                # app/escala/models.py:1686-1688,1816.
                return {
                    "folga_decidida_pela_porta": False,
                    "celula_existe": cel is not None,
                    "trabalha_gravado": None if cel is None else cel.trabalha,
                    "status_gravado": (
                        None if cel is None else
                        cel.status_dia if (
                            cel.status_dia is None
                            or cel.status_dia in status_permitidos
                        ) else NV
                    ),
                    "status_atual_pelo_juiz": NV,
                    "pode_declarar_agora": NV,
                }

            def retrato_vinculo(ec):
                # Apenas identificador e vigencia; nunca __str__.
                # Campos consumidos pelo juiz:
                # app/escala/alimentacao.py:214-216.
                return {
                    "id": ec.pk,
                    "ativa": ec.ativa,
                    "inicio": (
                        ec.data_inicio.isoformat()
                        if ec.data_inicio is not None else None
                    ),
                    "fim": (
                        ec.data_fim.isoformat()
                        if ec.data_fim is not None else None
                    ),
                }

            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, "
                        "READ ONLY"
                    )
                    cursor.execute(
                        "SELECT current_database(), current_user, "
                        "current_schema(), "
                        "current_setting('transaction_read_only')"
                    )
                    if cursor.fetchone() != (
                        "sombra", "leitor", "juliani", "on"
                    ):
                        raise RuntimeError("BancoPapelOuTenantIncompativel")

                # Relogio Django/envelope: [nao verificado].
                hoje = timezone.localdate()

                # O aceite real usa disputa.colaborador e data_evento.
                # app/chamados/services/validacao.py:293-294.
                # Carimbo da aceitacao:
                # app/chamados/services/validacao.py:152-166.
                perguntas = consumir(
                    Pergunta.objects.filter(validada_em__isnull=False)
                    .select_related("disputa__colaborador__empresa")
                    .order_by("pk")
                )
                feitos = set()

                for perg in perguntas:
                    # app/chamados/services/validacao.py:259-261.
                    if not _e_aceite_de_folga(perg):
                        continue

                    c = perg.disputa.colaborador
                    dia = perg.data_evento
                    if dia is None:
                        sem_data_o199 += 1
                        linhas.append((
                            "PENDENCIA", "O199", "col%d" % c.pk,
                            NV, "aceite_sem_data_evento",
                        ))
                        continue

                    chave = (c.pk, dia)
                    if chave in feitos:
                        continue
                    feitos.add(chave)

                    if c.empresa_id is None:
                        raise RuntimeError("EmpresaAusente")

                    # app/ponto/janelas.py:60-63.
                    ini, fim = janela_atual(dia, c.empresa)
                    par = (fim.year, fim.month)
                    if not ini <= dia <= fim:
                        raise RuntimeError("CompetenciaIncompativel")
                    totais_o199.setdefault(par, 0)

                    # Juiz real de pendencia do backfill:
                    # app/ponto/services/declarar_dia.py:78-95.
                    if o_dia_ja_e_folga(c, dia):
                        continue

                    # app/folha/export.py:556-594.
                    exportada = competencia_entregue(
                        c.empresa_id, fim.month, fim.year
                    )

                    # Leitura da coluna; nao julgamento do status.
                    # app/escala/models.py:1681-1688,1816.
                    cs = consumir(CelulaDia.objects.filter(
                        colaborador_id=c.pk, data=dia
                    ))
                    if len(cs) > 1:
                        raise RuntimeError("CelulaDuplicada")
                    cel = cs[0] if cs else None

                    linhas.append((
                        "O199", "col%d" % c.pk, dia.isoformat(),
                        competencia(par), "s" if exportada else "n",
                        compacto(estado_gravado(cel)),
                    ))
                    totais_o199[par] += 1

                # SC11: exclusivamente col305 na competencia empresarial 11.
                # Modelo: app/colaboradores/models.py:283.
                c305 = Colaborador.objects.select_related("empresa").get(pk=305)
                if c305.empresa_id is None:
                    raise RuntimeError("EmpresaAusente")

                # app/ponto/janelas.py:54-63.
                ini, fim = janela_fechamento(11, 2026, c305.empresa)
                if janela_atual(hoje, c305.empresa) != (ini, fim):
                    raise RuntimeError("OnzeNaoECompetenciaCorrente")

                # app/escala/alimentacao.py:54-78.
                celulas = celulas_do_periodo([305], ini, fim)

                # Incluir geradoras nomeadas fora da candidatura da janela.
                # app/escala/alimentacao.py:163-186.
                escalas = escalas_do_periodo(
                    305, ini, fim,
                    mais={
                        cel.escala_geradora_id
                        for cel in celulas.values()
                        if cel.escala_geradora_id is not None
                    },
                )

                # app/folha/export.py:556-594.
                exp305 = competencia_entregue(c305.empresa_id, 11, 2026)

                dia = ini
                while dia <= fim:
                    cel = celulas.get((305, dia))

                    # Cobertura nao se presume pelo flag ativa.
                    # app/escala/alimentacao.py:205-216.
                    cobrem = [
                        ec for ec in escalas
                        if vinculo_cobre_a_data(ec, dia)
                    ]

                    # Nao escolher outro vinculo por fora da autoridade.
                    # app/escala/alimentacao.py:228-286.
                    dono = vinculo_do_dia(
                        305, dia, escalas, celulas=celulas
                    )

                    motivos = []
                    if cel is None:
                        if cobrem:
                            motivos.append("sem_celula_com_vinculo")
                    elif dia > hoje:
                        # Janela de UM dia: obtem casos usando o medidor
                        # existente, sem copiar seu predicado.
                        # app/ponto/services/vigia_cadastro_futuro.py:24-87.
                        medida = medir(
                            dia, dia, hoje=hoje, colaborador_id=305
                        )
                        motivos = sorted(
                            k
                            for grupo in ("contadores", "selos")
                            for k, n in medida[grupo].items()
                            if n > 0
                        )
                    else:
                        # Medidor nao possui jurisdicao neste ramo.
                        # app/ponto/services/vigia_cadastro_futuro.py:31.
                        existentes_sem_juizo += 1

                    if motivos:
                        linhas.append((
                            "SC11", "col305", dia.isoformat(), "11/2026",
                            "s" if exp305 else "n",
                            compacto({
                                "existe": cel is not None,
                                "trabalha": (
                                    None if cel is None else cel.trabalha
                                ),
                            }),
                            compacto({
                                "do_dia": (
                                    None if dono is None else dono.pk
                                ),
                                "cobrem": [
                                    retrato_vinculo(ec)
                                    for ec in sorted(cobrem, key=lambda e: e.pk)
                                ],
                                "motivos": motivos,
                            }),
                        ))
                        total_sc11 += 1

                    dia += dt.timedelta(days=1)

        # Publicar somente depois de a leitura inteira terminar.
        for linha in linhas:
            emitir(*linha)

        for par, n in sorted(totais_o199.items()):
            emitir(
                "TOTAL", "O199", competencia(par),
                NV if sem_data_o199 else n,
            )
            if sem_data_o199:
                emitir("TOTAL_PARCIAL", "O199", competencia(par), n)

        emitir(
            "TOTAL", "SC11", "11/2026",
            NV if existentes_sem_juizo else total_sc11,
        )
        if existentes_sem_juizo:
            emitir("TOTAL_PARCIAL", "SC11", "11/2026", total_sc11)

        emitir(
            "COBERTURA", "SC11", "11/2026",
            "existentes_ate_hoje_sem_juizo=%d" % existentes_sem_juizo,
            NV if existentes_sem_juizo else "medidores_nomeados",
        )
        if sem_data_o199:
            emitir(
                "COBERTURA", "O199",
                "aceites_sem_data=%d" % sem_data_o199, NV,
            )

    except Exception:
        # Nao imprimir excecao, traceback, objeto ou texto livre:
        # podem conter nomes. Nenhum total parcial vira total integral.
        emitir("ERRO", "SONDA", NV)
        emitir("TOTAL", "O199", "11/2026", NV)
        emitir("TOTAL", "SC11", "11/2026", NV)


sonda()
```

3. **Portas do eventual apply:** O199 → `declarar_dia` (`app/ponto/services/declarar_dia.py:153`); SC11 por dia → `regenerar_celulas_dia`, que delega a `regenerar_celulas_vinculo` sem estender horizonte (`app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:773`). Números atuais de produção: `[nao verificado]`.

## 16/11 14:26 · sessao 7ac5cdb58d24 · PRIORIDADE-EV-ITEM3 · leitura · origem=Code

| entrada | item O54 | cadeia (arquivo:linha) | confianca |
|---|---|---|---|
| BO-ADMIN-2710/3 | - | Justificativa limpa marcos faltantes (`app/escala/utils.py:1254-1259`) → resposta “dia já estava completo” (`app/ponto/services/veredito_celula.py:242-247`). E6 compara minutos realizados/pagos (`app/ponto/management/commands/e6_oraculo.py:263-268,362-381`); falta demonstrar que essa cura reduziria os divergentes de `E6-sistema-bate` (`app/core/espelho_verdade.py:180-181`). | [nao verificado] |
| BO2910/01-a | - | Troca de posto e histórico (`app/colaboradores/views.py:325-336`) → possível mudança de destino empresarial do TXT, cujo leitor filtra empresa (`app/folha/export.py:719-722`). Redução de número O54 por corrigir esse destino não demonstrada. | [nao verificado] |
| BO2910/01-b | - | O ramo relê o banco e devolve linha/toast de erro (`app/colaboradores/views.py:337-361`); falta ligação demonstrada desse tratamento de falha até uma medida O54. | [nao verificado] |
| BO2910/02 | - | Célula e marcos absolvem o dia (`app/chamados/reconciliador.py:1024-1032`) → retratação do chamado (`app/chamados/reconciliador.py:1001-1011`). Falta demonstrar que ampliar essa retratação reduziria algum número O54. | [nao verificado] |
| BO2910/05 | - | Validação condicionada ao envio de hora (`app/ponto/services/ausencia.py:1068-1070`) → atualização/invalidação da projeção (`app/ponto/services/ausencia.py:1101-1124`). `E5-le-da-celula` espera status `encerrado` (`app/core/espelho_verdade.py:150-151`), e E6 exclui dias com ausência (`app/ponto/management/commands/e6_oraculo.py:339-341`); redução não demonstrada. | [nao verificado] |
| BO2910/06 | - | Desligamento chama fechamento dos vínculos (`app/colaboradores/services/desligamento.py:147-149`); reativação exige revínculo administrativo (`app/colaboradores/services/desligamento.py:235-247`). A menção às portas em `R4` (`app/core/placar_estrutural.py:389-390`) não demonstra redução sem identificar vínculo/início e a divergência correspondente. | [nao verificado] |
| BO2910/08 | - | Regeneração futura chama desfazimento (`app/ponto/portas/celula.py:297-315`), que conserva decisão administrativa (`app/ponto/portas/celula.py:1155-1164`). Falta ligar essa conservação a um divergente efetivamente contado pelo E6 (`app/ponto/management/commands/e6_oraculo.py:362-381`). | [nao verificado] |
| BO-ADMIN-2710/5 | - | Leitura de `missing` identifica marco a cobrar (`app/chamados/services/declaracao_texto.py:130-139`). `R3` obtém a frase diretamente dos marcos da grade (`app/ponto/services/dia_decidido.py:74-87`); fazer nascer a pergunta não demonstra redução desse item (`app/core/placar_estrutural.py:146-153`). | [nao verificado] |
| HJ-017-U1 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-074 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-005-U1 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-012-U2 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-005-U2 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-005-U4 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| BO-ADMIN-2710/1 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| BO-ADMIN-2710/8.1 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |

## 16/11 14:26 · sessao 24490930eb08 · - · - · origem=Ronald (tmux)

- O pouso de O-JANELA-HE nomeia quatro rotas: `gestao_he`, `decidir_he`, `recusar_he_em_lote`, `gestao_he_pdf`; os “13 passos” estão referidos, sem roteiro enumerado encontrado. `app/docs/RELATO-ARQUIVO.md:28278-28285`, `app/docs/RELATO-ARQUIVO.md:19705`.

- Central original: `relatorios:index`, `/relatorios/`, `relatorios.views.index`; lê contador, renderiza sem trilha de uso; falha do contador chama `logger.exception`, não grava contador de erro no banco. `app/relatorios/urls.py:9`, `app/relatorios/views.py:22-51`. O painel de chamados também lê esse contador; seu `except` apenas esconde o atalho. `app/chamados/urls.py:9`, `app/chamados/views.py:759-767`.

- Listagem: `ponto:gestao_he`, `/ponto/gestao-he/`, view `ponto.views.gestao_he`; corpo lê e renderiza, sem escrita de uso. `app/ponto/urls.py:17`, `app/ponto/views.py:887-966`.

- PDF: `ponto:gestao_he_pdf`, `/ponto/gestao-he/pdf/`, view `ponto.views.gestao_he_pdf`; chama pipeline auditável com `tipo='outro'` e filtros `{empresa, mes, ano, q, praca, posto, estado}`. `app/ponto/urls.py:27`, `app/ponto/views.py:772`, `app/ponto/views.py:870-878`. Escritor `RegistroPDFAuditavel.objects.create`, campos `tipo`, `gerado_em`, `filtros_json`, `total_registros`; não há campo título/URL. `app/relatorios/pdf_base.py:152-166`, `app/relatorios/models.py:36-55`. Registro nasce antes da segunda passagem do PDF, portanto registro não prova resposta HTTP concluída. `app/relatorios/pdf_base.py:168-180`.

- Assinatura de filtros do PDF HE pode fornecer contagem candidata: demais produtores `tipo='outro'` encontrados usam filtros distintos — RetratoFolha `{empresa,mes,ano,recorte,n,total}` e férias `{empresa_id,situacao}`. Isso é inferência sobre a cópia atual, sem garantia de atribuição histórica da rota. `app/ponto/views.py:872-876`, `app/folha/views_retrato.py:443-449`, `app/relatorios/services.py:338-346`.

- Portas laterais, caso incluídas no escopo: `ponto:espelho` → `/ponto/espelho/` → `espelho_ponto`; `colaboradores:calendario_ponto` → `/colaboradores/<cid>/calendario/` → `calendario_ponto`; `folha:exportar` → `/folha/exportar/` → `tela_exportar_dominio`; `folha:gerar_txt` → `/folha/exportar/gerar/` → `gerar_dominio_txt`. `app/ponto/urls.py:10`, `app/ponto/views.py:97`, `app/colaboradores/urls.py:24`, `app/colaboradores/views.py:1965`, `app/folha/urls.py:13-14`, `app/folha/views.py:83`, `app/folha/views.py:102`. Prefixos em `app/config/urls.py:26-34`. Inclusão dessas portas nos 13 passos específicos: `[nao verificado]`.

- Export TXT: sucesso grava `LogAuditoria(acao='export_txt', modelo='ExportacaoDominio', valor_depois={empresa,mes,ano,colabs,linhas,retidos,arquivo})`; recusa do portão apenas retorna mensagem. `app/folha/views.py:128-133`, `app/folha/views.py:285-300`. `MetricaSnapshot` do portão guarda falhas e pendências, sem registrar chamada ao export ou recusa ocorrida. `app/folha/porta_export.py:772-796`, `app/folha/porta_export.py:800-814`, `app/folha/porta_export.py:851-867`.

- Data de REABRIR mais precisa na cópia: deploy com migrate em **25/10 07:49**, sem segundos; isso permanece relato histórico, não medição desta leitura. `app/docs/RELATO.md:4766`, `app/docs/RELATO.md:4775`.

## 16/11 14:26 · sessao 98d2e0d17bff · - · - · origem=Ronald (tmux)

REABRIR-NAO-DESTROI — achados para consolidação:

- `RespostaAnterior` foi renomeada para `DecisaoPergunta`; `versao_reaberta_em` virou `ato_em` e os registros antigos recebem `ato='reabertura'`. A sonda da cópia deve consultar o modelo atual. `app/chamados/migrations/0051_decisao_pergunta.py:41-61`
- Campos consultáveis: `pergunta`, `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo`, fotos da resposta/validação. Escritor único: `registrar()`, cujo INSERT ocorre em `bulk_create`. `app/chamados/models.py:4038-4051`; `app/chamados/models.py:4110-4122`
- Reabrir gera uma linha por pergunta que perdeu texto, resposta, validação ou carimbo; não gera linha para pendente limpa. Logo, número de linhas não é número de cliques. `app/chamados/services/disputa_emissao.py:1631-1648`
- Modelo de auditoria não guarda URL, request ID nem status HTTP. Portanto não permite separar rotas que compartilham a mesma ação. `app/core/models.py:295-307`

| Porta/fluxo | Rota e view | Evidência de uso / escritor |
|---|---|---|
| Reabrir supervisão | `chamados:reabrir_disputa_supervisao`, `/chamados/local/<id>/reabrir-disputa/`; view `views.py:1372` — `urls.py:20`, prefixo `config/urls.py:28` | `DecisaoPergunta(ato='reabertura', ato_em)` pelo escritor acima; `LogAuditoria(acao='reabrir_disputa', modelo='DisputaSupervisao')` em `views.py:1456` |
| Reabrir pelo fio/modal | `chamados:fio_colaborador`, `/chamados/colaborador/<id>/`, view `views.py:659`; `chamados:modal_fio`, `/chamados/modal/<id>/`, view `views.py:869` — `urls.py:21,39` | POST delegado em `views.py:667-674,951-954`; ação `reabrir_disputa` em `services/fio.py:679-682`; mesmo LogAuditoria acima em `services/acoes_disputa.py:113-114`, portanto indistinguível da rota própria |
| Colaborador pede correção HTML | `chamados:solicitar_correcao_disputa`, `/chamados/disputa/<pk>/solicitar-correcao/`, view `views.py:1317` — `urls.py:19` | Chama `reabrir_por_colab` em `views.py:1334`; snapshot e contador em `models.py:2318-2321`; nota com prefixo `[Solicitacao de correcao - ` em `models.py:2325` |
| Colaborador pede correção API | `api_solicitar_correcao_disputa`, `/api/disputa/<pk>/solicitar-correcao/`, view `api/views.py:2670` — `api/urls.py:94`, prefixo `config/urls.py:37` | Mesmo método em `api/views.py:2686`; indistinguível do HTML no fato persistido |
| Corrigir resposta | POST `chamados:modal_fio`, `acao=corrigir_resposta`; botão `templates/chamados/partials/modal_fio.html:301-312` | `LogAuditoria(acao='corrigir_resposta_admin', modelo='PerguntaDisputa')` em `services/acoes_disputa.py:375-378`; é escrito antes da validação, que pode recusar em `:379-383` |
| Cobrar resposta | `chamados:cobrar_chamado`, `/chamados/cobrar/<cid>/`, view `views_cobrar.py:24` — `urls.py:25`; botão `modal_fio.html:349-351` | `FollowupChamado.conteudo` começa `[COBRANCA]`, `criado_em`; chamada escritora em `views_cobrar.py:54-56`; INSERT em `services/nota.py:103,107`; campos em `models.py:1732-1738` |
| Encerrar/regularizar | POST `chamados:modal_fio`, `acao=regularizar_disputa`; botão `modal_fio.html:353-359`; também fio e `chamados:detalhe_local` (`urls.py:14`) | `LogAuditoria(acao='regularizar_disputa_granular', modelo='DisputaSupervisao')`; descrição `Mat=N Pend=N Fechada=B Erros=N` em `services/fio.py:713-714` e `services/acoes_disputa.py:144-145` |

Filtros indicados, sempre com `criado_em__gte=corte` para auditoria/followup e `ato_em__gte=corte` para decisões:

- Reaberturas por pergunta: `DecisaoPergunta.objects.filter(ato="reabertura", ato_em__gte=corte).count()`. `app/chamados/models.py:4025,4039-4040`
- Reabertura supervisão/fio conjunta: auditoria `acao="reabrir_disputa", modelo="DisputaSupervisao"`. `app/chamados/views.py:1456`; `app/chamados/services/acoes_disputa.py:113`
- Correção administrativa: auditoria `acao="corrigir_resposta_admin", modelo="PerguntaDisputa"`. `app/chamados/services/acoes_disputa.py:375`
- Cobrança: followup `conteudo__startswith="[COBRANCA]"`; não usar `tipo`, que não é coluna de FollowupChamado. `app/chamados/views_cobrar.py:36-39,55`; `app/chamados/models.py:1732-1755`
- Regularização conjunta: auditoria `acao="regularizar_disputa_granular", modelo="DisputaSupervisao"`; contar tentativas com erro pelo `Erros=N` persistido, sem equiparar erro a indevido. `app/chamados/services/fio.py:713-714`
- Recusa humana de resposta: `DecisaoPergunta(ato="recusa")`; preferível à auditoria `recusar_pergunta`, gravada duas vezes pelo mesmo caminho. `app/chamados/services/validacao.py:628-629,637-645`
- Recusa de materialização: auditoria `acao="val_perg_nao_gravou", modelo="PerguntaDisputa"`; compartilhada entre validações, sem atribuição segura à correção/modal. `app/chamados/services/validacao.py:517-521`

As recusas por limite, disputa fechada ou ausência de resposta apenas retornam estado; não geram fato de recusa nesse método. `app/chamados/models.py:2304-2312` As recusas de cobrança por permissão, área, throttle ou push são respostas HTTP anteriores à nota. `app/chamados/views_cobrar.py:25-53` As guardas da regularização também retornam sem a auditoria posterior. `app/chamados/services/fio.py:685-699`

`recusa_motivo` representa estado atual e pode ser sobrescrito; notas de materialização são deduplicadas por pergunta/código, portanto não contam tentativas. `app/chamados/services/materializacao.py:1095-1113` Não há nesses registros um critério que classifique a recusa como indevida. [nao verificado]

A abertura do modal chama `marcar_lidas`, escrevendo no banco; a sonda deve ler modelos diretamente e não chamar a view. `app/chamados/views.py:877-880`; `app/chamados/services/nota.py:126-133` O histórico é renderizado pelo template, mas sua existência no banco não prova que foi mostrado corretamente. `app/templates/chamados/partials/modal_fio.html:204-205`; `app/chamados/models.py:3667-3683`

Falhas de auditoria/best-effort vão para logging e contador de memória, sem registro adicional no banco. `app/core/observ.py:73-82,105,134-139` A cobertura de logs da VM desde 04/11 13:22 veio apenas do dado fornecido. [nao verificado]

## 16/11 14:26 · sessao 6a1bdea30721 · - · - · origem=Ronald (tmux)

| entrada | item O54 | cadeia / motivo | confiança |
|---|---|---|---|
| BO-ADMIN-2710/3 | - | `app/escala/utils.py:1254-1259` apaga os marcos faltantes por justificativa → `app/ponto/services/veredito_celula.py:242-247` declara o dia completo. Não se demonstrou redução do número E6: ele lê realizado/pago (`app/ponto/management/commands/e6_oraculo.py:263-268,342-373`), enquanto o abono prometido é outra medida (`app/docs/BACKLOG.md:1976`; `app/ponto/motor_calculo_v2.py:2606-2622`). | [nao verificado] |
| BO2910/06 | - | O desligamento atual chama `fechar_vinculos_ativos` (`app/colaboradores/services/desligamento.py:147-149`), cuja porta já recusa fim anterior ao início (`app/colaboradores/services/vinculo.py:855-864`); a reativação exige revínculo pelo admin (`app/colaboradores/services/desligamento.py:235-247`). Sem definir vínculo/início da cura, não se demonstra redução de R4; sua referência às portas (`app/core/placar_estrutural.py:389-390`) não fecha essa cadeia. | [nao verificado] |
| HJ-017-U1, HJ-074, HJ-005-U1, HJ-012-U2, HJ-005-U2, HJ-005-U4, BO-ADMIN-2710/1, BO-ADMIN-2710/8.1 | - | O quadro fornecido não comprova produtor atual; substituir essa lacuna por causas históricas não demonstra a cadeia da entrada atual. [nao verificado] | [nao verificado] |

O placar registra expressamente que a causa dos 26+9 dias ESTRUTURA não foi nomeada (`app/core/espelho_verdade.py:190-191`; `app/core/placar_estrutural.py:130`). Portanto, compartilhar motor, célula ou cadastro com o medidor não comprova participação nesses números.

## 16/11 14:26 · sessao c5a20957395e · - · - · origem=Ronald (tmux)

| entrada | item O54 | cadeia (arquivo:linha) | confianca |
|---|---|---|---|
| BO2910/05 | - | Período opcional na decisão (`app/ponto/services/ausencia.py:1068-1070`) → invalidação da projeção (`app/ponto/services/ausencia.py:1123-1124`); não demonstra redução de item: `E5-le-da-celula` espera especificamente status encerrado (`app/core/espelho_verdade.py:150-151`). Além disso, o espelho publica a ausência (`app/ponto/services/espelho.py:582,845`) e o medidor E6 exclui dias com ausência (`app/ponto/management/commands/e6_oraculo.py:339-341`). | [nao verificado] |
| BO2910/08 | - | Regeneração de futura chama desfazimento (`app/ponto/portas/celula.py:312-315`), que conserva expressamente decisão administrativa (`app/ponto/portas/celula.py:1155-1164`); não há vínculo demonstrado entre essa conservação e um divergente contado no E6 (`app/ponto/management/commands/e6_oraculo.py:362-381`). | [nao verificado] |
| BO-ADMIN-2710/5 | - | `_marcos_faltantes` lê `missing` para identificar marco a cobrar (`app/chamados/services/declaracao_texto.py:130-139`); R3 compõe sua frase diretamente dos marcos faltantes (`app/ponto/services/dia_decidido.py:74-87,102-127`), e E6 compara minutos (`app/ponto/management/commands/e6_oraculo.py:350-381`). Fazer nascer a pergunta não demonstra alteração desses números. | [nao verificado] |

## 16/11 14:26 · sessao 67deb7deb439 · - · - · origem=Ronald (tmux)

SC11 tem duas autoridades registradas:

- Cobertura: `vinculo_cobre_a_data(ec,dia)` — `app/escala/alimentacao.py:205-216`; registro `app/core/juizes.py:1322-1323`, tabela `app/docs/JUIZES.md:137-138`.
- Vínculo do dia: `vinculo_do_dia(cid,dia,escalas,celulas=...)` — `app/escala/alimentacao.py:228-286`; tabela `app/docs/JUIZES.md:135-136`. Carregador completo disponível em `vinculo_do_dia_carregado`:289-317.

Universo de existência: dias cobertos por vínculo, sem filtro por situação pessoal; lei e delegação estão em `app/ponto/portas/celula.py:870-880,906`. Sem célula e sem cobertura não é pendência desse gerador; contador existente confirma em `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:51-57`.

Não localizei juiz geral puro registrado que diga “esta célula está errada” em toda a competência: `[nao verificado]`. Cuidado com substitutos:

- `EscalaColaborador.eh_dia_trabalho` lê a própria `cel.trabalha` quando existe; compará-los seria tautologia — `app/escala/models.py:1069-1096`.
- Divergência do cadastro tem medidor real `ponto.services.vigia_cadastro_futuro.medir(ini,fim,*,hoje,colaborador_id=None)` — `app/ponto/services/vigia_cadastro_futuro.py:24-87`. A jurisdição é **futuro**, e os baldes são **geradas**:31,49-50. Ele mede cobertura, cadastro/DNA e selos. Pode ser chamado por dia para obter casos sem copiar o predicado: `medir(d,d,hoje=hoje,colaborador_id=305)`. Não chamar `lavrar`:99-108.
- Comparar template vivo com todo passado contraria o próprio tripwire: passado divergente pode ser irretroatividade — `app/ponto/management/commands/tripwire_celulas.py:4-6`.
- `supra_juiz.classificar_dia` é puro, mas sua docstring diz que detecta contradições e **não decide quem está certo** — `app/ponto/supra_juiz.py:217-224`.
- `celula_veredito_velho` não serve: chama julgamento com escrita — `app/ponto/management/commands/celula_veredito_velho.py:19-21,144-145`.

Porta para eventual apply por dia: `ponto.portas.celula.regenerar_celulas_dia` — `app/ponto/portas/celula.py:741-776`; delega ao escritor `regenerar_celulas_vinculo`:112, com horizonte desligado:773-776. Cria ausente e corrige gerada em :289-310. Não chamá-la na leitura: até recusa de vínculo sem vigência grava evento — :147-162.

Sutileza: com célula existente, `vinculo_do_dia` pode devolver geradora inativa sem fim (:279-284), enquanto `vinculo_cobre_a_data` a recusa (:214-216). Exibir essas respostas sem presumir equivalência.

## 16/11 14:25 · sessao 3ac856a91cc7 · - · - · origem=Ronald (tmux)

- **Portas da HE:** prefixo `/ponto/` em `app/config/urls.py:29`. `ponto:gestao_he` = `/ponto/gestao-he/`, view `gestao_he` em `app/ponto/views.py:887`; `ponto:decidir_he` = `/ponto/gestao-he/decidir/`, view em `app/ponto/views.py:499`; `ponto:recusar_he_em_lote` = `/ponto/gestao-he/recusar-lote/`, view em `app/ponto/views.py:549`; `ponto:gestao_he_pdf` = `/ponto/gestao-he/pdf/`, view em `app/ponto/views.py:772`. Rotas em `app/ponto/urls.py:17-27`.

- **GET listagem:** lê snapshot em `app/ponto/views.py:932-946` e renderiza em `app/ponto/views.py:954-966`; não escreve trilha de abertura nessa view. Portanto número de aberturas não é obtido desse caminho no banco. O snapshot disponível não significa que a tela foi aberta: seu escritor é `lavrar`, `app/ponto/services/he_pendente_lavrado.py:177-189`.

- **Uso de decisão:** escritor único `_gravar` cria/atualiza `DecisaoHE` em `app/ponto/portas/he.py:66-86` e solicita `LogAuditoria(acao='decidir_he', modelo='DecisaoHE', objeto_id=pk, valor_antes, valor_depois)` em `app/ponto/portas/he.py:94-99`, chamado pela porta em `app/ponto/portas/he.py:206-208`. Modelo/campos `DecisaoHE`: `colaborador`, `data`, `estado`, `minutos_na_decisao`, `origem`, `decidida_em`, `atualizada_em` em `app/ponto/models.py:1300-1317`. Modelo/campos auditoria: `acao`, `modelo`, `objeto_id`, JSONs, `criado_em` em `app/core/models.py:295-307`; escritor efetivo em `app/core/observ.py:133-138`.

- **Não há atribuição inequívoca ao endpoint:** lote chama `decidir_he` por dia em `app/ponto/portas/he.py:263-266`, e autorização por período também chama a mesma porta em `app/ponto/portas/he.py:595-596`. A trilha não guarda URL/rota entre os campos escritos em `app/core/observ.py:134-137`. Assim, contar `acao='decidir_he'` mede alterações por dia da porta compartilhada; não mede POSTs individuais, lotes ou cliques separadamente. Repetição idempotente retorna sem trilha em `app/ponto/portas/he.py:197-204`.

- **“Não” gravado é decisão de negócio:** estado `'nao'` em `DecisaoHE` é ciência, não erro, segundo `app/ponto/models.py:1283-1288`. O estado atual não conserva cada troca; a porta atualiza a mesma linha em `app/ponto/portas/he.py:75-86`. Para histórico por instante usar `LogAuditoria.criado_em` e `valor_depois.estado`, cujos escritores estão em `app/ponto/portas/he.py:94-99`.

- **Recusa da porta:** validações levantam `DecisaoHERecusada` em `app/ponto/portas/he.py:175-193`; a view individual converte em resposta em `app/ponto/views.py:535-537`. Lote conserva recusas apenas em lista de retorno em `app/ponto/portas/he.py:267-268`, exibida em `app/ponto/views.py:621-629`. Esses caminhos não persistem a recusa nem um indicador “indevida”. Não contar tais recusas como zero a partir da ausência de trilha. Critério persistido que distingue recusa indevida de devida nesses caminhos: **[nao verificado]**.

- **Erro persistido do retrato:** `apurar` incrementa `erros` quando falha `espelho_do_colab` em `app/ponto/services/he_pendente_lavrado.py:94-100`, devolve `detalhe['erros']` em `app/ponto/services/he_pendente_lavrado.py:169-174`, e grava por `MetricaSnapshot.objects.update_or_create(... detalhe=retrato)` em `app/ponto/services/he_pendente_lavrado.py:177-189`. Campos `MetricaSnapshot.detalhe` e `calculado_em` em `app/inteligencia/models.py:95-96`. É foto atual por empresa/competência, sobrescrita; não soma histórica de erros HTTP desde deploy nem evidência de abertura da tela. Se reportar, usar tipo explícito como `apuracao_snapshot_atual`.

- **Falha de trilha:** `evento` é best effort, envolve criação em `engolir` em `app/core/observ.py:105-138`; erro vira contador de processo e logging em `app/core/observ.py:73-82`, não outra linha de banco. `registrar_log` também captura e chama `logger.exception` em `app/core/auditoria.py:26-30`. Portanto ausência de auditoria pode ser falha da própria auditoria, além de ausência de uso.

- **Relavratura:** depois da decisão, `recalcular_por_evento` é chamado em `app/ponto/portas/he.py:229-230`; agenda `on_commit` em `app/ponto/services/fechamento.py:1823-1827`; falha de execução apenas gera warning em `app/ponto/services/fechamento.py:1818-1821`. Decisão gravada não prova relavratura bem-sucedida.

- **PDF:** grava `RegistroPDFAuditavel` pelo pipeline em `app/relatorios/pdf_base.py:152-166`, chamado com `tipo='outro'`, `colaborador=None` e filtros empresa/mes/ano/q/praca/posto/estado em `app/ponto/views.py:870-878`. Campos de registro em `app/relatorios/models.py:30-57`; não há título/rota persistido nessa criação. `tipo='outro'` e formato dos filtros não bastam para identificação inequívoca da porta HE. Registro nasce antes da segunda montagem do PDF em `app/relatorios/pdf_base.py:168-178`, portanto também não prova resposta HTTP entregue.

- **500 histórico da atomicidade:** explicação e correção visíveis em `app/ponto/portas/he.py:151-159`; `@transaction.atomic` está na porta em `app/ponto/portas/he.py:158`, antes do `select_for_update` em `app/ponto/portas/he.py:195`. Dados de erros reais posteriores ao pouso não estão na cópia **[nao verificado]**. Contexto de access log vai em cabeçalhos para Gunicorn, não em modelo, conforme `app/core/middleware.py:96-105` e `app/core/middleware.py:111-128`.

## 16/11 14:25 · sessao 37406318bec5 · - · - · origem=Ronald (tmux)

O bloqueio do R2 foi baseado em escrita Python em memória, não em escrita no banco:

- `vigente()` consulta `TipoAusencia` e escreve somente o dicionário global `_CACHE`; retorna o catálogo real calculado. `app/ponto/catalogo/ausencias.py:240-254,257,267-281`.
- `_ult_cache.append(...)`, `_ub_cache.append(...)` e `_mpd_cache.append(...)` são listas locais. As fontes são valor calculado, `SELECT aggregate(Max(...))` e `marcos_dna_periodo(...)`. Impedir esses `append` quebra seus retornos indexados. `app/ponto/turnos.py:1154-1164,1901-1918`.
- `fechamento_lido()` chama o escritor real com `somente_leitura=True, retornar_objeto=True`; o ramo devolve o objeto sem salvá-lo e o ramo de lavratura fica excluído. `app/ponto/services/fechamento.py:1128-1144,1191-1194,1735-1761`.

Para cumprir o patch nominal exigido, a interceptação mínima pode substituir **somente o objeto `ponto.catalogo.ausencias._CACHE`** por um proxy que:

1. Delega `__getitem__` ao `_CACHE` original.
2. Intercepta `__setitem__` exclusivamente das chaves `"v"` e `"t"`, contabilizando e descartando as escritas.
3. Não inventa catálogo, não troca `vigente()` e não troca turnos.

Esse patch intercepta precisamente `app/ponto/catalogo/ausencias.py:279-280`. Em falta de cache, a função continua calculando `v` pelo leitor real e retornando-o; apenas deixa de memoizá-lo. `app/ponto/catalogo/ausencias.py:275-281`.

O tripwire precisa guardar uma **flag persistente por dia**: `_do_banco()` captura `Exception` e pode engolir sua exceção, retornando fallback. Após a chamada/dia, a flag deve obrigar `ERRO|escrita_nao_interceptada`, mesmo que a função tenha retornado. `app/ponto/catalogo/ausencias.py:246-254`.

Não encontrei chamadas a cache persistente nos arquivos investigados; cobertura completa de todos os caminhos transitivos: [nao verificado].

## 16/11 14:25 · sessao a0cb305f5b49 · - · - · origem=Ronald (tmux)

O199 tem duas perguntas distintas:

- **Já está decidida como folga?** A porta usa `o_dia_ja_e_folga(colaborador, data)`, em `app/ponto/services/declarar_dia.py:78-95`; o aceite real a consulta em `app/chamados/services/validacao.py:305`. Ela delega ao juiz registrado em `app/docs/JUIZES.md:127` e `app/core/juizes.py:213`: `folga_lancada_do_dia`, corpo puro em `app/ponto/precedencia.py:212-219`, predicado em `:222-239`, alimentado por todas as folgas da pessoa em `:194-209`. Folga à mão sobrevive à troca; calendário exige vínculo do dia, `app/ponto/precedencia.py:225-239`.
- **Pode declarar agora?** `checar_declaracao`, `app/ponto/services/declarar_dia.py:126-150`. Não equivale à primeira pergunta: `o_dia_ja_e_folga` distingue “não precisa” de “não posso”, conforme `:84-93`.

Universo correto: perguntas cuja resposta, após `strip`, seja `RESPOSTA_DE_FOLGA`, conforme `app/chamados/services/validacao.py:259-261` e constante `:79`; aceite gravado em `validada_em`, conforme escritor `:152-166`; colaborador e dia usados pelo ato são `perg.disputa.colaborador` e `perg.data_evento`, `:293-294`. Campos em `app/chamados/models.py:2521-2538,2552-2554`. Deduplicar por colaborador-dia, porque o próprio ato trata quatro perguntas do mesmo dia, `app/chamados/services/validacao.py:301-305`.

**Restrição para o script:** `checar_declaracao` chega a cache de catálogo: `declarar_dia.py:147,120` → `precedencia.py:461` → `turnos.py:2843-2847` (`Ausencia.TIPOS_EM_ABERTO`) → `catalogo/ausencias.py:406-407,430-435` → `vigente`, que escreve `_CACHE` em `:279-280`. `fatos_do_dia` também alcança o catálogo por `precedencia.py:256,273-278`. Se o molde proíbe inclusive essas escritas de cache, não chamar nem fabricar resultado equivalente.

O juiz de status é `status_do_dia_lavrado`, registrado em `app/docs/JUIZES.md:125`; avulso `app/ponto/status_dia.py:201-218` delega ao lote `:221`, que carrega turnos e pode consultar a natureza pela precedência (`:230-235`). Ler coluna atual de `CelulaDia` pode ser apresentado como snapshot gravado, mas não como resultado desse juiz.

Porta de apply O199: `ponto.services.declarar_dia.declarar_dia`, `app/ponto/services/declarar_dia.py:153`; ramo folga chama `_escrever_folga` em `:167-168`, que passa por `escala.folgas.lancar_folga` em `:179-193` e fecha o fio em `:205-209`.

## 16/11 14:25 · sessao a67016e6deab · - · - · origem=Ronald (tmux)

| entrada | item O54 | cadeia (arquivo:linha) | confiança |
|---|---|---|---|
| BO2910/01-a | - | A porta atribui `posto`, salva e registra histórico (`app/colaboradores/views.py:325-336`); a outra porta também atribui `empresa` (`app/colaboradores/views.py:1404-1407`). O TXT filtra pela empresa (`app/folha/export.py:719-722`), mas falta demonstrar que corrigir esse destino reduziria algum número O54; o termômetro compara minutos (`app/ponto/management/commands/e6_oraculo.py:342-379`). | [nao verificado] |
| BO2910/01-b | - | O ramo relê o colaborador e responde com linha/toast de erro (`app/colaboradores/views.py:337-361`); não há cadeia demonstrada desse tratamento de erro até medida de item O54. | [nao verificado] |
| BO2910/02 | - | A célula e seus marcos servem de predicado para retratar o chamado (`app/chamados/reconciliador.py:1024-1032` → `app/chamados/reconciliador.py:1001-1011`); falta cadeia demonstrada dessa retratação até redução de número O54. | [nao verificado] |

Complementos úteis: a troca de posto cadastral deixou de mudar o lugar do dia e regenerar células (`app/colaboradores/services/colaborador.py:118-123`). O item `R6` usa contratos declarados e reatividade (`app/core/placar_estrutural.py:433-449,623-625`); a matriz declara sete famílias sem família própria de colaborador/posto (`app/core/contratos_estruturais.py:45-53`), portanto contar portas de posto por semelhança com “um escritor” também não demonstra redução desse item.

## 16/11 14:23 · sessao 085d5bc63b58 · BUG-PAR-O-PAR · verificador · origem=Code

A certificação Git de HEAD/base `6d3a9ae10` fica **[nao verificado]**: esta cópia não contém `.git`. As conclusões abaixo usam o código disponível e o delta fornecido.

**1. Escritores, leitores e significados**

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| **U60/U103/U163, hash** | Dados no JSON congelado; autoria da transcrição **[nao verificado]**. O carregador valida hash, chaves, inteiros, tamanhos, repetição, interseção e serialização; produz conjuntos imutáveis. (`app/chamados/services/universo_bug_par.py:7`, `app/chamados/services/universo_bug_par.py:14`, `app/chamados/services/universo_bug_par.py:30`.) | Pertencimento por PK; leitores: espera DXG, guarda K6, elegibilidade U103, contador e comando. Recusa, lâmpada e devolução não alteram pertencimento. (`app/chamados/services/universo_bug_par.py:35`, `app/chamados/services/materializacao.py:259`, `app/chamados/services/materializacao.py:313`, `app/chamados/services/materializacao.py:710`, `app/chamados/services/materializacao.py:755`, `app/chamados/management/commands/bug_par_103.py:50`.) |
| **Identidade, motivo, disputa, destinos e célula** | Nascimento por `_nascer`; recostura por `reapontar_resolvedora`. (`app/chamados/models.py:2930`, `app/chamados/services/fio.py:849`.) | Identificam a metade, sua irmã, colaborador, empresa e célula consultada. (`app/chamados/services/materializacao.py:142`, `app/chamados/services/materializacao.py:405`, `app/chamados/management/commands/bug_par_103.py:55`.) |
| **`data_evento`** | Atribuição central em `gravar_dia_do_fato`. (`app/chamados/services/fio.py:835`, `app/chamados/services/fio.py:845`.) | K6 recebe a competência desse **dia do turno**, pela janela da empresa; a guarda posicional usa célula explícita, depois `data_evento`; sem pergunta, usa dia civil. (`app/chamados/services/materializacao.py:403`, `app/chamados/services/materializacao.py:759`, `app/chamados/services/passivo_validado.py:93`.) |
| **`resposta_colab`, `respondida_em`** | Resposta humana, automática e simetria do carimbo. (`app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3240`.) | Irmã/par/elegibilidade consultam C21 ou sua face SQL: instante **e** texto após trim. (`app/chamados/services/materializacao.py:145`, `app/chamados/services/materializacao.py:192`, `app/chamados/services/materializacao.py:259`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1580`.) |
| **`validada_em`, `validada_por`** | Ato de validar em `_carimbar_validacao`; devoluções retiram; resposta nova desfaz; simetria preenche nulos. (`app/chamados/services/validacao.py:162`, `app/chamados/services/validacao.py:227`, `app/chamados/services/validacao.py:303`, `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/models.py:3244`.) | Retry, seleção da materialização e fechamento da disputa. O lookup foi corretamente ajustado para “escritor único **do ato**”. (`app/chamados/services/validacao.py:530`, `app/chamados/services/materializacao.py:1919`, `app/chamados/models.py:1923`, `app/docs/JUIZES.md:38`.) |
| **Carimbo: `materializada_em/por`, `via_resolucao`** | `_escrever_carimbo`, acionado pelas portas de carimbo/descarimbo; devolução U103 usa `descarimbar`. (`app/chamados/models.py:3207`, `app/chamados/models.py:3191`, `app/chamados/services/validacao.py:300`.) | Testemunha, seleção pendente, elegibilidade U103 e explicador. “Materializada” pode encerrar por identidade sem batida; a testemunha distingue essas vias. (`app/chamados/services/materializacao.py:261`, `app/chamados/services/materializacao.py:796`, `app/chamados/services/explicador.py:116`.) |
| **`recusa_motivo`, `pendencia_admin`** | Pendência/materializador, recusa humana, escape da resposta e duplicata; o carimbo pode receber o motivo. (`app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1561`, `app/chamados/services/materializacao.py:1564`, `app/chamados/services/validacao.py:760`, `app/chamados/services/disputa_emissao.py:1511`, `app/chamados/services/fio.py:1404`, `app/chamados/models.py:3298`.) | Devolução, explicador, modal e invariantes I18/I19. O campo continua sendo motivo explicativo de origens diferentes; deixou de selecionar o universo. (`app/chamados/services/validacao.py:220`, `app/chamados/services/explicador.py:113`, `app/templates/chamados/partials/modal_fio.html:226`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`, `app/chamados/services/universo_bug_par.py:35`.) |
| **Batida: instante, tipo, origem, aprovação, autoria e vínculo** | Materializador fornece argumentos; `registrar_batida` cria pela porta existente. (`app/chamados/services/materializacao.py:1363`, `app/ponto/registro_batida.py:176`.) | Deduplicação, colisão, F6 e lastro usam esses valores; a deduplicação inclui as bordas da janela. (`app/ponto/registro_batida.py:131`, `app/ponto/registro_batida.py:158`, `app/chamados/juizes.py:247`.) |
| **`criada_neste_toque`, coleção do par** | A porta de batida marca somente criação; materializador coleta os PKs durante a escrita da irmã. (`app/ponto/registro_batida.py:173`, `app/ponto/registro_batida.py:179`, `app/chamados/services/materializacao.py:1372`, `app/chamados/services/materializacao.py:1604`.) | Rollback/classificação da colisão do par; leitor anterior da marca permanece em `resposta_batida`. Unidade: criação **deste ato**, não “PK maior”. (`app/chamados/services/materializacao.py:1390`, `app/chamados/services/materializacao.py:1621`, `app/ponto/services/resposta_batida.py:14`.) |
| **Decisão, lavra e auditoria** | `DecisaoPergunta.registrar`, `veredito_lavrado.lavrar` e `observ.evento`; devolução U103 fornece ato, autor, texto, motivo e antes/depois. (`app/chamados/models.py:4110`, `app/chamados/veredito_lavrado.py:270`, `app/core/observ.py:94`, `app/chamados/services/validacao.py:308`, `app/chamados/services/validacao.py:313`.) | Rótulo BUG-PAR lê a decisão de devolução; explicador lê fato/lavra. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/explicador.py:116`.) |
| **Estado de aplicação, `ok/msg/erros` e `bloqueada_competencia`** | Testemunha retorna estado/detalhe; fio e filtro adaptam indisponibilidade; validação retorna o resultado e declara o vocabulário. (`app/chamados/services/materializacao.py:817`, `app/chamados/services/fio.py:374`, `app/chamados/templatetags/chamados_tags.py:136`, `app/chamados/services/validacao.py:58`, `app/chamados/services/validacao.py:589`.) | Casca lê `ok`; templates leem o estado. Há dois significados de bloqueio reunidos nesse rótulo: **F1 abaixo**. (`app/chamados/services/acoes_disputa.py:50`, `app/templates/chamados/partials/_pergunta_card.html:46`, `app/templates/chamados/partials/modal_fio.html:274`.) |
| **Contador, relatório e reversão** | Contador produz listas/baldes e `total_aval`; comando produz seleção U103, competências, tuples da pauta e fotografia de reversão. (`app/chamados/services/materializacao.py:336`, `app/chamados/management/commands/bug_par_103.py:50`, `app/chamados/management/commands/bug_par_103.py:61`, `app/chamados/management/commands/bug_par_103.py:93`.) | Comando publica stdout e, com apply, chama a porta. O mesmo nome `total_aval` recebe recortes diferentes: **F4 abaixo**. (`app/chamados/management/commands/bug_par_103.py:76`, `app/chamados/management/commands/bug_par_103.py:100`.) |
| **`aviso_das_irmas`** | Composto de donos distintos dos erros, separados por PK; citações internas não contam como donos. (`app/chamados/services/validacao.py:121`, `app/chamados/services/validacao.py:690`.) | Casca acrescenta o aviso à mensagem de sucesso. A retirada de “outros dias” corresponde ao contrato real; o teste tem lacuna: **F3 abaixo**. (`app/chamados/services/acoes_disputa.py:58`, `app/chamados/services/validacao.py:696`.) |

Os contratos temporais não são intercambiáveis: K6 usa competência do dia do fato; F6 usa a data local do instante plantado; o corte U103 exige carimbo **estritamente anterior** a 18/10 00:00 no fuso configurado. Essa diferença está explícita nas chamadas, mas a apresentação funde os dois bloqueios. (`app/chamados/services/materializacao.py:759`, `app/ponto/janelas.py:84`, `app/chamados/services/materializacao.py:258`, `app/chamados/services/materializacao.py:262`.)

Há leitor fora do diff recebendo mudança: o explicador consulta a espera DXG e deixa de apresentar essa espera para fora163, passando à explicação seguinte. (`app/chamados/services/explicador.py:132`, `app/chamados/services/explicador.py:137`, `app/chamados/services/materializacao.py:710`.)

**2. Juízes, escritores e relógio**

Não identifiquei segundo juiz registrado nem segundo escritor de batida/carimbo neste delta: K6 já está registrado; C21 é consultado diretamente; `par_do_intervalo` permanece composição, e as escritas continuam pelas portas existentes. (`app/core/juizes.py:343`, `app/chamados/services/materializacao.py:192`, `app/chamados/services/materializacao.py:176`, `app/chamados/services/materializacao.py:1363`, `app/chamados/services/validacao.py:300`.)

Não identifiquei nova leitura do relógio no delta produtivo: o corte U103 é constante, e os instantes de aprovação/validação/devolução continuam nos escritores existentes. (`app/chamados/services/materializacao.py:246`, `app/chamados/services/materializacao.py:1369`, `app/chamados/services/validacao.py:162`, `app/chamados/services/validacao.py:309`.)

**PROCEDE — F1: o mesmo estado passa a barrar duas coisas diferentes.** A guarda nova limita K6 a metades fora163, mas a testemunha também retorna `bloqueada_competencia` por F6 para outras perguntas de hora. Os dois templates agora escondem Validar indistintamente. Consequência concreta: competência apenas aprovada/fechada, sem entrega K6, perde na tela o toque administrativo que a porta conserva por O44; alcança também motivos fora do intervalo e U163. (`app/chamados/services/materializacao.py:755`, `app/chamados/services/materializacao.py:862`, `app/templates/chamados/partials/_pergunta_card.html:46`, `app/templates/chamados/partials/modal_fio.html:274`, `app/chamados/services/validacao.py:533`.)

**3. Mordida dos testes** [nao verificado]

Executei sem banco: **14 testes verdes, cinco mutantes c2 mortos e 15 mutantes não integração mortos**; as mutações são aplicadas às fontes em memória, com falha por asserção exigida pelos runners. (`app/chamados/tests/test_bug_par_c2_sem_banco.py:41`, `app/chamados/tests/test_bug_par_sem_banco.py:21`, `app/chamados/tests/test_universo_bug_par.py:10`, `app/chamados/tests/bug_par_c2_mutantes.py:56`, `app/chamados/tests/bug_par_c2_mutantes.py:62`, `app/chamados/tests/bug_par_mutantes.py:146`, `app/chamados/tests/bug_par_mutantes.py:164`.)

As mortes dos dez mutantes de integração — par, ordem, retry, contador, S139, comando, devolução e HTML — ficam **[nao verificado]**. O runner sem `--django` os exclui, inclusive quando se pede `todos`; portanto seu RC0 não certifica todos os declarados. (`app/chamados/tests/bug_par_mutantes.py:93`, `app/chamados/tests/bug_par_mutantes.py:129`.)

A catraca executou **35 testes verdes** pelo molde em memória; o hunk conserva a igualdade das ocorrências e registra a consulta civil em 404. (`app/core/tests/test_medida_turno_do_par.py:20`, `app/core/tests/test_medida_turno_do_par.py:271`, `app/core/tests/test_medida_turno_do_par.py:557`.)

**PROCEDE — F2: o caso 5644 continua sem absolvição, e o RED aceita isso.** Com os slots e marcas da própria fixture, executei o propositor real: 01:00 e 02:00 retornam `False`. O teste calcula seu `expected` por esse mesmo propositor, aceita os dois `False` e cobra a consulta mockada. A guarda recebe os horários overnight crus e chama o atribuidor civil. (`app/chamados/tests/test_bug_par_sem_banco.py:26`, `app/chamados/tests/test_bug_par_sem_banco.py:47`, `app/chamados/tests/test_bug_par_sem_banco.py:51`, `app/chamados/services/materializacao.py:417`, `app/ponto/propositor.py:249`.)

Apliquei adicionalmente em memória `if False` no ponto que poderia absolver: **o teste continuou verde**. Consequência: ele não detecta uma guarda que nunca absolve. O teste de integração verifica par gravado/dia perfeito, sem exigir que essa guarda tenha absolvido; sua execução fica **[nao verificado]**. (`app/chamados/services/materializacao.py:420`, `app/chamados/tests/test_bug_par_sem_banco.py:50`, `app/chamados/tests/test_bug_par_volta.py:186`.)

**PROCEDE — F3: o RED do aviso deixa voltar a mentira sobre o dia.** O mutante declarado insere “outra(s)”, mas o assert procura literalmente `outro dia`. Restaurei em memória o texto antigo `de outro(s) dia(s)` e o teste continuou verde. Consequência: a regressão que atribui outro dia a uma separação apenas por PK escapa desse RED. (`app/chamados/tests/bug_par_c2_mutantes.py:23`, `app/chamados/tests/test_bug_par_c2_sem_banco.py:100`, `app/chamados/services/validacao.py:125`, `app/chamados/services/validacao.py:696`.)

**4. Competência exportada, dinheiro, escala e avais**

A devolução U103 consulta K6 depois da trava e antes de descarimbo, desvalidação ou DEVOLUÇÃO; a unidade fora163 consulta K6 antes de recusa/carimbo, e a validação também consulta antes do carimbo. Isso atende à ordem de proteção exigida pelos anexos, sem usar F6 como substituto de K6. (`app/chamados/services/validacao.py:285`, `app/chamados/services/validacao.py:293`, `app/chamados/services/validacao.py:300`, `app/chamados/services/materializacao.py:1714`, `app/chamados/services/validacao.py:587`.)

A escrita restaurada cria batidas e pode alimentar recálculo financeiro pelas portas existentes; portanto o efeito não se limita à apresentação. (`app/chamados/services/materializacao.py:1363`, `app/ponto/registro_batida.py:180`, `app/chamados/services/validacao.py:601`.)

Alteração direta de `motor_calculo_v2.py`, escala/vínculo ou fórmula financeira: nada no delta fornecido; confronto Git independente **[nao verificado]**. A contradição funcional identificada com O44 é **F1**; a absolvição pedida por TRAVA-E-SITIO-1911 não é comprovada nem realizada pela guarda no caso ensaiado em **F2**. (`app/chamados/services/validacao.py:533`, `app/chamados/services/materializacao.py:420`, `app/chamados/tests/test_bug_par_sem_banco.py:47`.)

**5. Demais problemas**

**PROCEDE — F4: `total_aval` mistura censo global, recorte e pendência.** O contador agora inclui perguntas já materializadas no total e aplica o recorte de disputas; o comando aplica `--empresa` à seleção, mas imprime sempre `len(U163)` e `len(U60)` globais. Consequência concreta: uma execução recortada pode mostrar elegíveis da empresa junto de `total_aval=163`, enquanto o contador do mesmo recorte devolve outro total. (`app/chamados/services/materializacao.py:315`, `app/chamados/services/materializacao.py:318`, `app/chamados/services/materializacao.py:338`, `app/chamados/management/commands/bug_par_103.py:48`, `app/chamados/management/commands/bug_par_103.py:77`.)

Além disso, o contrato documental ainda exige contador que chegue a zero pela validação; contar todas as PKs presentes, inclusive materializadas, impede esse zeramento. Congelar pertencimento não exige congelar a medida de trabalho restante. (`app/docs/PROMPTS.md:2235`, `app/chamados/services/materializacao.py:310`, `app/chamados/services/materializacao.py:338`.)

**PROCEDE — F5: uma pergunta sem data derruba a publicação inteira da LISTA.** A nova varredura fora163 não exclui `data_evento=None`, permitido no modelo, e chama `_competencia` sem tratamento; a janela termina acessando `data.day`. Consequência concreta dessa entrada: o DRY aborta antes de imprimir a pauta, enquanto a guarda da materialização trata a mesma ausência como impedimento falante. Existência dessa entrada no retrato de produção: **[nao verificado]**. (`app/chamados/models.py:2528`, `app/chamados/management/commands/bug_par_103.py:68`, `app/chamados/management/commands/bug_par_103.py:72`, `app/chamados/services/passivo_validado.py:94`, `app/ponto/janelas.py:27`, `app/chamados/services/materializacao.py:757`.)

VEREDITO: DEVOLVE F1, F2, F3, F4, F5

## 16/11 14:20 · sessao 8d8f76d6f967 · - · - · origem=Ronald (tmux)

PROCEDE — RED 5644 não prova a absolvição autorizada. O teste usa o próprio propositor como gabarito e compara dois `False`; a morte de `dia_civil` ocorre somente na expectativa da consulta mockada. Executei também a mutação em memória `materializacao.py:420 → if False:`: o teste continuou verde, embora a guarda jamais absolvesse. (`app/chamados/tests/test_bug_par_sem_banco.py:47-58`; `app/chamados/services/materializacao.py:420`; `app/ponto/propositor.py:249-258`.) O teste de integração verifica o par final, mas não exige que a guarda tenha absolvido nem testa essa mutação; execução dele permanece `[nao verificado]`. (`app/chamados/tests/test_bug_par_volta.py:186-196`.)

PROCEDE — RED do aviso não mata a regressão que pretendia impedir. O mutante declarado acrescenta “outra(s)”, enquanto o assert procura a sequência literal “outro dia”. Restaurei em memória o texto antigo “de outro(s) dia(s)” e o teste continuou verde; portanto ele permite voltar a atribuir outro dia a erros separados somente por PK. (`app/chamados/tests/bug_par_c2_mutantes.py:23`; `app/chamados/tests/test_bug_par_c2_sem_banco.py:95-100`; `app/chamados/services/validacao.py:696`.)

Verificação executada sem banco/bytecode/arquivos: 14 testes sem banco verdes, cinco mutantes c2 mortos por asserção e 15 mutantes não integração mortos por asserção. Os dez mutantes de integração foram explicitamente excluídos pelo runner sem `--django`; suas mortes permanecem `[nao verificado]`. (`app/chamados/tests/bug_par_c2_mutantes.py:49-68`; `app/chamados/tests/bug_par_mutantes.py:93-94`; `app/chamados/tests/bug_par_mutantes.py:129-132`; `app/chamados/tests/bug_par_mutantes.py:164-166`.)

Catraca executada conforme o molde em memória: 35 testes verdes; o delta mantém a comparação exata das linhas e a consulta civil registrada em 404. (`app/core/tests/test_medida_turno_do_par.py:20-28`; `app/core/tests/test_medida_turno_do_par.py:270-271`; `app/core/tests/test_medida_turno_do_par.py:557-561`.)

Não sustentaria sobrevivência de `command_txt`: a fixture responde somente à saída U103; a volta nasce sem resposta e não entra no laço fora163. Remover a classificação da saída esvazia a pauta e elimina `col<pk>`, exigido pelo teste. Execução de integração `[nao verificado]`. (`app/chamados/tests/test_bug_par_103.py:19-21`; `app/chamados/tests/test_bug_par_projetado.py:87-89`; `app/chamados/tests/test_bug_par_projetado.py:521-523`; `app/chamados/management/commands/bug_par_103.py:68-74`; `app/chamados/tests/test_bug_par_103.py:73`.)

## 16/11 14:20 · sessao 349274e94bb0 · - · - · origem=Ronald (tmux)

PROCEDE — `bloqueada_competencia` mistura a recusa autorizada pelo K6 com a recusa F6, e a apresentação passa a barrar ambas. `_competencia_que_barra` restringe K6 às metades fora163 (`app/chamados/services/materializacao.py:754-760`), mas a testemunha também retorna o mesmo estado por F6 para qualquer pergunta de hora (`app/chamados/services/materializacao.py:862-864`). Os novos condicionais escondem Validar indistintamente (`app/templates/chamados/partials/_pergunta_card.html:46-47`; `app/templates/chamados/partials/modal_fio.html:274`). Consequência: competência apenas fechada/aprovada, sem entrega K6, perde o botão de toque administrativo que a porta conserva por O44 (`app/chamados/services/validacao.py:533-537`).

PROCEDE — O RED 5644 não comprova a absolvição pedida: aceita `False` como resultado correto. Ele calcula `expected` pelo propositor e compara a guarda com esse valor (`app/chamados/tests/test_bug_par_sem_banco.py:44-50`). Executando o propositor real com os slots e marcas dessa fixture, ambos os plantios, 01:00 e 02:00, retornam `False` (`app/chamados/tests/test_bug_par_sem_banco.py:26-29`; `app/ponto/propositor.py:238-258`). A guarda passa horários overnight crus (`app/chamados/services/materializacao.py:417-420`); o predicado usa `atribuir_global` sem a normalização overnight existente em `atribuir_global_turno` (`app/ponto/propositor.py:87-109`; `app/ponto/propositor.py:249-250`). O teste de integração verifica o plantio do par e o dia perfeito, sem afirmar que a guarda absolveu (`app/chamados/tests/test_bug_par_volta.py:186-196`).

Na hipótese p dentro163/q fora163 entregue, não identifiquei escrita indevida em q: a unidade retorna antes de qualquer escrita (`app/chamados/services/materializacao.py:1714-1718`) e a tentativa da irmã desfaz seu savepoint quando ela não grava (`app/chamados/services/materializacao.py:1616-1619`).

## 16/11 14:19 · sessao 792b7f1bfe41 · - · - · origem=Ronald (tmux)

- `recusa_motivo`: escritores existentes em `app/chamados/services/materializacao.py:1261`, `:1561`, `:1564`; `app/chamados/services/validacao.py:760`; `app/chamados/services/disputa_emissao.py:1511`; `app/chamados/services/fio.py:1404`; atribuição pelo carimbo em `app/chamados/models.py:3297`. O campo continua sendo consumido como motivo/auditoria pelo explicador, modal e invariantes I18/I19; deixou de identificar pertencimento ao universo. (`app/chamados/services/explicador.py:108-113`; `app/templates/chamados/partials/modal_fio.html:226`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1074-1075`, `:1111-1112`; `app/chamados/services/universo_bug_par.py:33-35`.)

- Universo: escritor material é o JSON congelado; carregador valida hash, estrutura e interseção e devolve `frozenset`; pertencimento só lê PK. Seus leitores são DXG, K6, elegibilidade U103, contador e comando. (`app/chamados/services/universo_bug_par.py:7-35`; `app/chamados/services/materializacao.py:259`, `:313`, `:710`, `:755`; `app/chamados/management/commands/bug_par_103.py:46`, `:50`, `:69`.)

- `bloqueada_competencia`: vocabulário escrito em `validacao.ESTADOS`; retornos novos em validação; apresentação da casca lê `ok=False`, e os dois HTMLs leem o estado. A própria testemunha já usava o mesmo rótulo para F6 e passa também a usá-lo para K6. (`app/chamados/services/validacao.py:57-58`, `:589`, `:625`; `app/chamados/services/acoes_disputa.py:50-51`; `app/templates/chamados/partials/_pergunta_card.html:46`; `app/templates/chamados/partials/modal_fio.html:274`; `app/chamados/services/materializacao.py:817`, `:840`, `:864`.) Não há confusão entre os juízes nos argumentos: K6 recebe competência do `data_evento`, F6 recebe instante local; são perguntas diferentes, compostas explicitamente. (`app/chamados/services/materializacao.py:751-769`; `app/chamados/services/passivo_validado.py:86-95`; `app/ponto/janelas.py:84-88`.)

- `aviso_das_irmas`: conta PKs distintos de donos dos erros, não dias nem referências internas a outras perguntas; seu leitor produtivo é a casca de validação. A remoção de “outro(s) dia(s)” corresponde ao contrato real. (`app/chamados/services/validacao.py:685-700`; `app/chamados/services/acoes_disputa.py:58-63`.)

**Achado de sentido:** `total_aval` tem dois recortes no código: contador retorna PKs existentes no recorte de disputas, enquanto comando imprime sempre 163, mesmo com `--empresa`. Além disso, o contrato documental ainda promete que o contador cai a zero por validação, mas o contador novo preserva todas as materializadas no total. (`app/chamados/services/materializacao.py:310-338`; `app/chamados/management/commands/bug_par_103.py:48-50`, `:76-77`; `app/docs/PROMPTS.md:2235`.) Consequência: após a validação o indicador não pode demonstrar zeramento; com empresa recortada os dois números denominados `total_aval` podem divergir. (`app/chamados/services/materializacao.py:318-321`, `:338`; `app/chamados/management/commands/bug_par_103.py:77`.)

**Achado condicional do comando:** a nova varredura fora163 aceita `data_evento=None`, campo permitido pelo modelo, e o entrega sem tratamento a `_competencia`; `janela_atual` acaba acessando `None.day`, abortando o DRY inteiro antes de publicar a lista. (`app/chamados/models.py:2528-2530`; `app/chamados/management/commands/bug_par_103.py:68-79`; `app/chamados/services/passivo_validado.py:93-95`; `app/ponto/janelas.py:27`.) A guarda de produção trata esse mesmo caso explicitamente como impedimento falante. (`app/chamados/services/materializacao.py:757-758`.) Existência desse registro no retrato: `[nao verificado]`.

**Possível excesso na LISTA:** a nova seleção fora163 não exige ausência de materialização, nem exclui linha fora do fato; perguntas já aplicadas e respondidas também satisfazem a consulta e entram na Pauta DP. (`app/chamados/management/commands/bug_par_103.py:68-74`.) A quantidade e a presença dessas linhas no retrato: `[nao verificado]`.

## 16/11 14:14 · sessao 05339093263a · MIUDOS-REABRE-O-ITEM-3 · leitura · origem=Code

| Entrada | (0) Causa nesta cópia | (1) Classe e alcance de produção | (2) Dinheiro | (3) Pede `!` | (4) Lei aberta? | (5) Cruza obra em voo¹ |
|---|---|---|---|---|---|---|
| **BO-ADMIN-2710/3** | Persiste: justificativas aprovadas viram datas inteiras; o crédito parcial soma Ausência; motor conta abono pelo previsto dos dias. `app/ponto/services/efeito_ausencia.py:40–50`; `app/ponto/services/fechamento.py:660–665`; `app/ponto/motor_calculo_v2.py:2606–2622`. | **OBRA**: produtores e consumidores em arquivos distintos, incluindo motor; não cabe no limite de um arquivo sem motor. Mesmos sítios. | **s** — falta, abono e DSR. `app/ponto/motor_calculo_v2.py:2600–2630`. | **s** — motor é zona inviolável. `CLAUDE.md:142`. | **n para a regra desta causa**: pagamento completa o previsto descontando trabalho do dia; pergunta já respondida. `app/docs/RELATO.md:491`. | **n** nos sítios localizados, frente ao manifesto declarado. `anexo em_voo_1611.txt:2–43`. |
| **BO2910/01-a** | Persiste: `vincular_posto` atribui posto e salva sem acompanhar empresa; a transferência escreve ambos. `app/colaboradores/views.py:325–336,1405–1407`. | **MIUDO — 1 arquivo**, `app/colaboradores/views.py`: as duas portas e as atribuições correspondentes estão nele; não exige modelo, migration ou juiz novo. `:325–336,1405–1407`. | **s** — empresa seleciona fechamentos do TXT. `app/folha/export.py:719–722`. | **n no mecanismo localizado** — trata cadastro posto/empresa; a alteração do posto cadastral não muda o vínculo que responde pelo dia. `app/colaboradores/services/colaborador.py:118–123`; `app/colaboradores/views.py:1405–1407`. | **n** — comportamento já estabelecido pela outra porta. `app/colaboradores/views.py:1397–1407`. | **s — painel do vínculo**, `app/colaboradores/views.py`. O pouso da R3 não elimina este cruzamento atual. `anexo em_voo_1611.txt:37–40`. |
| **BO2910/01-b** | Persiste a ausência de registro da exceção; **mudou o ramo**: agora relê o banco antes de devolver linha/toast, mas continua sem registrar a falha. `app/colaboradores/views.py:337–361`. | **MIUDO — 1 arquivo**, `app/colaboradores/views.py`: tratamento de exceção local, sem alteração de modelo, motor ou autoridade. `:337–361`. | **n no ramo causal** — releitura, renderização e resposta de erro. `app/colaboradores/views.py:337–361`. | **n** — o ramo causal não altera escala, vínculo, motor ou gravado de exportada. `app/colaboradores/views.py:337–361`. | **n** — falta de registro já identificada como defeito, sem decisão de negócio pendente nesta causa. `app/docs/BACKLOG.md:1133`. | **s — painel do vínculo**, `app/colaboradores/views.py`, apesar do pouso da R3. `anexo em_voo_1611.txt:37–40`. |
| **BO2910/02** | Persiste: a retratação pela célula filtra somente os três módulos de turno e exclui texto livre. `app/chamados/reconciliador.py:337–338,997–999`. | **MIUDO no mecanismo localizado — 1 arquivo**, `app/chamados/reconciliador.py`: seleção, predicado e retratação estão nele; já usa a autoridade de dia e abstém sem dia, sem exigir juiz novo. `:997–1032`. Alcance sobre nominal sem dia/célula: **[nao verificado]**; `app/docs/RELATO.md:3404`. | **n no mecanismo** — retrata chamado/perguntas e rejeita apenas justificativa pendente; disputa/chamado não determina a elegibilidade do cálculo. `app/chamados/reconciliador.py:523–541`; `app/ponto/services/fechamento.py:576–579`. | **s** — ampliar o universo altera limite de negócio expressamente mantido até corte do Ronald. `app/chamados/reconciliador.py:977–981`. | **s** — quais chamados de texto livre têm o dia como premissa suficiente para absolvição? O código mantém os demais fora até corte. `app/chamados/reconciliador.py:977–981`. | **n** no arquivo causal necessário localizado. `anexo em_voo_1611.txt:2–43`. |
| **BO2910/05** | Persiste, com linhas deslocadas: formulário permite omitir ambas as horas; view passa `None`; serviço só valida quando alguma hora veio. `app/templates/ponto/partials/painel_ausencia.html:325–330`; `app/ponto/views.py:2700–2715`; `app/ponto/services/ausencia.py:1068–1070`. | **MIUDO quanto à aceitação indevida — 1 arquivo**, `app/ponto/services/ausencia.py`: concentra decisão e validador já existente; template e view participarem do caminho não exige alterar os três para impedir a aceitação. `:1039–1048,1057–1081`. | **s** — decisão/período alimentam folha e invalidam a projeção. `app/ponto/services/ausencia.py:1112–1124`. | **n no escopo registrado** — obrigatoriedade na aprovação já está escrita; não exige regra nova, motor ou alteração de vínculo. `app/docs/BACKLOG.md:1133`; `app/ponto/services/ausencia.py:1039–1081`. | **n para novas aprovações** — período obrigatório já registrado; tratamento de estoque sem período também existe. `app/docs/BACKLOG.md:1133`; `app/templates/ponto/partials/painel_ausencia.html:310–317`. | **n no arquivo causal mínimo**, `app/ponto/services/ausencia.py`. `anexo em_voo_1611.txt:2–43`. |
| **BO2910/06** | Persiste a não reabertura, mas ela é também contrato explícito atual: reativação restaura cadastro/acesso e manda o admin revincular. `app/colaboradores/services/desligamento.py:235–259`. | **Tamanho [nao verificado]**. Há **1 produtor localizado**, `desligamento.py`, mas o número **exigido** depende da escolha de vínculo/início; a autoridade existente já consegue reativar vínculo pela mesma data inicial, portanto não há prova de dois arquivos obrigatórios. `app/colaboradores/services/vinculo.py:100–124,259–283`. | **s pelo encadeamento existente de vínculo → regeneração → fechamento**; impacto nominal **[nao verificado]**. `app/colaboradores/services/vinculo.py:315–348`; `app/ponto/portas/celula.py:483–496`. | **s** — dado de vínculo e mudança do automatismo explícito “admin revincula”. `app/colaboradores/services/desligamento.py:237–238`. | **s** — reabrir qual vínculo, com qual início, preservando ou preenchendo o intervalo desligado? Contrato atual não define reabertura automática. `app/colaboradores/services/desligamento.py:235–238`; escolha por início: `app/colaboradores/services/vinculo.py:100–124`. | **n nos produtores localizados** (`desligamento.py`/`vinculo.py`); envelope final **[nao verificado]**. `anexo em_voo_1611.txt:2–43`. |
| **BO2910/08** | Persiste para decisão humana; **mudou para futuras automáticas**: DNA reescrito já desfaz julgamento futuro, mas conserva expressamente o administrativo e exclui humanos do rejulgamento. `app/ponto/portas/celula.py:297–315,465–470,1135–1139,1155–1164`. | **MIUDO técnico — 1 arquivo**, `app/ponto/portas/celula.py`: reescrita, preservação e autoridade de veredito estão nele, com cartório existente; não exige juiz novo. `:297–315,462–472,970–987,1060–1067`. | **s** — recalcula fechamento por competência. `app/ponto/portas/celula.py:483–496`. | **s** — DNA de escala, inclusive passado, e alteração da preservação administrativa explícita. `CLAUDE.md:149–154`; `app/ponto/portas/celula.py:973–980`. | **s** — quando DNA novo invalida decisão humana, hoje expressamente preservada? `app/ponto/portas/celula.py:973–980,1135–1139`. | **n** no arquivo causal localizado. `anexo em_voo_1611.txt:2–43`. |
| **BO-ADMIN-2710/5** | Persiste: `_marcos_faltantes` pula célula não `missing` sem confrontar `tipo_real`, que o leitor já entrega. `app/chamados/services/declaracao_texto.py:130–139`; `app/escala/services/leitor_celula.py:122–133`. | **MIUDO — 1 arquivo**, `app/chamados/services/declaracao_texto.py`: dados necessários já chegam ao leitor; há teste próprio desse leitor. `:130–139`; `app/chamados/tests/test_hx_marcos_faltantes_celula.py:42–64`. | **s** — registro causal do intervalo e exigência de medir a hora de intrajornada indenizada no DIFF. `app/docs/RELATO.md:3984`; `app/docs/BACKLOG.md:848`. | **n no leitor localizado** — não altera escala/vínculo, motor ou gravado de exportada; consome dados já fornecidos. `app/chamados/services/declaracao_texto.py:100–139`; `app/escala/services/leitor_celula.py:122–133`. | **n na causa localizada** — defeito de leitura já identificado, sem escolha nova de negócio demonstrada. `app/docs/RELATO.md:3984`. | **n** no produtor e teste citados. `anexo em_voo_1611.txt:2–43`. |
| **HJ-017-U1** | Causa atual **[nao verificado]**: código já trata saída por posição e reconciliação da ata; pendência documental não prova defeito remanescente. `app/ponto/turnos.py:1067–1100`; `app/escala/utils.py:1945–2040`; `app/docs/BACKLOG.md:1270`. | — | — | — | — | — |
| **HJ-074** | Produtor atual **[nao verificado]**: default continua verdadeiro, mas nascimento consulta o juiz; nominal não identifica quem gravou a flag. `app/escala/models.py:896–917`; `app/colaboradores/services/vinculo.py:287–296`; `app/docs/BACKLOG.md:2652`. | — | — | — | — | — |
| **HJ-005-U1** | Causa **[nao verificado]**: registro ainda exige censo e declara cobertura parcial não demonstrada para o dia. `app/docs/BACKLOG.md:849`. | — | — | — | — | — |
| **HJ-012-U2** | Causa **[nao verificado]**: classificação da ingestão e medição anterior como cobrança legítima seguem sem reconciliação demonstrada. `app/docs/BACKLOG.md:1572`; `app/docs/RELATO.md:5603`. | — | — | — | — | — |
| **HJ-005-U2** | Produtor **[nao verificado]**: `sem_periodo` demonstra falta de hora, sem identificar sua porta de origem. `app/ponto/turnos.py:2632–2637,2646–2647`. | — | — | — | — | — |
| **HJ-005-U4** | Produtor **[nao verificado]**: mesmo limite; resposta `sem_periodo` não comprova escritor defeituoso. `app/ponto/turnos.py:2646–2647`. | — | — | — | — | — |
| **BO-ADMIN-2710/1** | Produtor atual **[nao verificado]**: causa histórica era cobertura crua; leitor atual distingue `sem_periodo` e classifica cadastro. `app/docs/RELATO.md:3980`; `app/ponto/supra_juiz.py:395–402,453–455`. | — | — | — | — | — |
| **BO-ADMIN-2710/8.1** | Defeito atual **[nao verificado]**: escritor já fotografa antes, distingue editar/criar e registra diferenças; view delega à porta. `app/colaboradores/services/vinculo.py:187–188,368–383`; `app/colaboradores/views.py:247–249`. | — | — | — | — | — |

¹ Cruzamentos restritos às listas fornecidas de BUG-PAR, painel e editor. **O10-D nao verificavel**, pois não tem lista declarada; portanto os `n` dessa coluna não certificam ausência de cruzamento com O10-D. (`anexo em_voo_1611.txt:2–44`.)

**Contagem por classe:**

- **6 MIUDO:** /01-a, /01-b, /02, /05, /08 e BO-ADMIN-2710/5. São os seis mecanismos concentrados em um arquivo identificados acima. (`app/colaboradores/views.py:325–361`; `app/chamados/reconciliador.py:997–1032`; `app/ponto/services/ausencia.py:1039–1081`; `app/ponto/portas/celula.py:297–315,970–987`; `app/chamados/services/declaracao_texto.py:130–139`.)
- **1 OBRA:** BO-ADMIN-2710/3. (`app/ponto/services/efeito_ausencia.py:40–50`; `app/ponto/services/fechamento.py:660–665`; `app/ponto/motor_calculo_v2.py:2606–2622`.)
- **1 tamanho [nao verificado]:** /06. Das quatro pendências, /02, /05 e /08 fecham em **1 arquivo de produção cada**; /06 não permite afirmar número exigido sem decidir vínculo/início. A porta existente impede concluir que dois arquivos sejam obrigatórios. (`app/colaboradores/services/desligamento.py:235–259`; `app/colaboradores/services/vinculo.py:100–124,259–283`.)
- **8 sem causa atual comprovada**, sem classificação posterior: as oito últimas linhas, com os limites de prova nelas citados.

**ELEGÍVEL pela regra literal: nenhuma entrada demonstrada.** Os seis MIUDO têm dinheiro, `!`/lei aberta ou cruzamento declarado; O10-D também permanece não verificável. (`app/folha/export.py:719–722`; `app/chamados/reconciliador.py:977–981`; `app/ponto/services/ausencia.py:1112–1124`; `app/ponto/portas/celula.py:483–496,973–980`; `app/docs/BACKLOG.md:848`; `anexo em_voo_1611.txt:39,44`.)

Motivo por não elegível, uma linha por entrada:

- **BO-ADMIN-2710/3:** OBRA, dinheiro e motor sujeito a `!`. (`app/ponto/motor_calculo_v2.py:2606–2622`; `CLAUDE.md:142`.)
- **BO2910/01-a:** dinheiro/TXT e cruzamento com painel em `app/colaboradores/views.py`. (`app/folha/export.py:719–722`; `anexo em_voo_1611.txt:39`.)
- **BO2910/01-b:** cruza painel em `app/colaboradores/views.py`; o pouso da R3 não o libera deste cruzamento. (`anexo em_voo_1611.txt:37–40`.)
- **BO2910/02:** ampliação do universo depende de corte de negócio, portanto `!` e pergunta de lei. (`app/chamados/reconciliador.py:977–981`.)
- **BO2910/05:** toca insumo e projeção de fechamento. (`app/ponto/services/ausencia.py:1112–1124`.)
- **BO2910/06:** tamanho não demonstrado, vínculo sujeito a `!` e decisão de reabertura ausente. (`app/colaboradores/services/desligamento.py:235–259`; `app/colaboradores/services/vinculo.py:100–124`.)
- **BO2910/08:** dinheiro, DNA de escala e mudança de regra explícita sobre decisão humana. (`app/ponto/portas/celula.py:483–496,973–980,1135–1139`.)
- **BO-ADMIN-2710/5:** envolve intrajornada indenizada no DIFF. (`app/docs/BACKLOG.md:848`; causa: `app/docs/RELATO.md:3984`.)
- **HJ-017-U1:** causa atual **[nao verificado]**, diante dos tratamentos já presentes. (`app/ponto/turnos.py:1067–1100`; `app/escala/utils.py:1945–2040`.)
- **HJ-074:** escritor causal da flag **[nao verificado]**. (`app/colaboradores/services/vinculo.py:287–296`; `app/docs/BACKLOG.md:2652`.)
- **HJ-005-U1:** causa **[nao verificado]**; censo ainda requerido. (`app/docs/BACKLOG.md:849`.)
- **HJ-012-U2:** causa **[nao verificado]**; classificações documentais não reconciliadas. (`app/docs/BACKLOG.md:1572`; `app/docs/RELATO.md:5603`.)
- **HJ-005-U2:** origem da omissão **[nao verificado]**. (`app/ponto/turnos.py:2646–2647`.)
- **HJ-005-U4:** origem da omissão **[nao verificado]**. (`app/ponto/turnos.py:2646–2647`.)
- **BO-ADMIN-2710/1:** defeito atual **[nao verificado]**; leitor já distingue cadastro sem período. (`app/ponto/supra_juiz.py:395–402,453–455`.)
- **BO-ADMIN-2710/8.1:** persistência atual do defeito **[nao verificado]**; escritor já registra edição e diferenças. (`app/colaboradores/services/vinculo.py:368–383`.)

## 16/11 14:11 · sessao 8d2c783a5f2e · - · - · origem=Ronald (tmux)

| Entrada | Resultado da leitura |
|---|---|
| **BO2910/06** | A reativação continua restaurando cadastro/acesso sem escala; isso também é o contrato explícito atual: “admin revincula” (`app/colaboradores/services/desligamento.py:235-259`). **Não há prova de OBRA ≥2 arquivos**: o juiz existente identifica vínculo pela data inicial (`app/colaboradores/services/vinculo.py:100-124`) e o executor já reativa a mesma linha (`app/colaboradores/services/vinculo.py:259-283`). **Um arquivo de produção pode bastar por composição**, mas a escolha de vínculo/data/âncora não está definida pelo dado; número exigido e certificação de MIUDO ficam **[nao verificado]**. **! s**, por vínculo e mudança do automatismo contratual (`app/colaboradores/services/desligamento.py:237-238`). **Lei s**: reabrir a vigência original, incluindo intervalo desligado, ou iniciar novo trecho; qual vínculo/data? **[nao verificado]**. Dinheiro da reabertura **s**, se passar pela autoridade existente que regenera células e provoca recálculo (`app/colaboradores/services/vinculo.py:315-348`; `app/ponto/portas/celula.py:483-496`); alcance nominal **[nao verificado]**. Cruzamento direto de `desligamento.py`/`vinculo.py` com manifestos declarados: **n** (`em_voo_1611.txt:1-44`, anexo fornecido); **O10-D nao verificavel**. |
| **BO2910/08** | A troca do DNA permanece e agora apaga julgamento automático de futuras, conservando admin; a causa humana permanece (`app/ponto/portas/celula.py:297-315,1135-1138,1155-1163`). **MIUDO técnico: 1 arquivo de produção**, `celula.py`, que contém a reescrita e a autoridade que conserva/escreve o veredito; não exige juiz novo, porque o cartório atual já é chamado nesse caminho (`app/ponto/portas/celula.py:462-472,970-986,1060-1067`). A exclusão em `pos_vinculo.py` lê o estado gravado, sem obrigatoriedade demonstrada de editar esse consumidor (`app/colaboradores/services/pos_vinculo.py:62-73`). **Dinheiro s**, recalcula por competência (`app/ponto/portas/celula.py:483-496`). **! s / lei s**: substituir a preservação de decisão humana diante de DNA novo altera a regra explícita “admin decidiu vence o cartório” (`app/ponto/portas/celula.py:973-980,1135-1138`). Cruzamento direto do produtor com manifestos declarados: **n** (`em_voo_1611.txt:1-44`, anexo fornecido); **O10-D nao verificavel**. |

**Cuidado com as citações do manifesto:** não conferi sua numeração linha a linha; use a contagem que o agente principal atribuir ao anexo. **[nao verificado]**

## 16/11 14:11 · sessao 8bc91becb95e · - · - · origem=Ronald (tmux)

| Entrada | Causa atual | Tamanho / arquivos de produção necessários | Dinheiro | ! / lei | Cruzamento |
|---|---|---|---|---|---|
| BO2910/02 | O universo da retratação pela célula continua limitado aos três módulos de turno, excluindo texto livre (`app/chamados/reconciliador.py:337-338`, `:997-999`). | **MIUDO quanto ao mecanismo localizado: 1 produção**, `app/chamados/reconciliador.py`; seleção, predicado e chamada de retratação estão nele (`:997-1032`). **Não exige juiz novo para chamados datados:** já importa `dia_do_fato` (`:45`), cuja leitura genérica suporta célula, contexto, batida e disputa (`app/chamados/juizes.py:88-139`). O alcance sobre o nominal sem data/célula permanece **[nao verificado]** (`app/docs/RELATO.md:3404`). | **n no mecanismo localizado:** retrata chamado/perguntas e rejeita apenas justificativa ainda pendente (`app/chamados/reconciliador.py:523-541`); chamado/disputa não determina elegibilidade do cálculo (`app/ponto/services/fechamento.py:576-579`). | **s / pergunta aberta:** a exclusão dos outros módulos é declarada “até Ronald cortar”, pois neles o dia não é a premissa inteira (`app/chamados/reconciliador.py:977-981`). Pergunta: qual subconjunto de texto livre a célula absolve? | **n nos arquivos necessários localizados**, comparando `app/chamados/reconciliador.py` com o manifesto fornecido; referência numerada do anexo **[nao verificado]**. **O10-D nao verificavel.** |
| BO2910/05 | Ambas as horas podem faltar no formulário; a view conserva `None`, e `decidir` só valida período se alguma hora vier (`app/templates/ponto/partials/painel_ausencia.html:325-330`; `app/ponto/views.py:2700-2715`; `app/ponto/services/ausencia.py:1068-1070`). | **MIUDO quanto à causa de aprovação indevida: 1 produção**, `app/ponto/services/ausencia.py`, escritor único da decisão, com validador de período já existente (`:1039-1048`, `:1057-1081`). A presença de template, view e serviço não prova necessidade de alterar os três; a view já encaminha a omissão à autoridade (`app/ponto/views.py:2700-2715`). Exigir também alteração visual do formulário seria escopo adicional da fila UI registrada em `app/docs/BACKLOG.md:1133`; sua necessidade técnica para impedir a aprovação não está demonstrada. | **s:** decisão e período são insumos da folha e acordam a projeção do fechamento (`app/ponto/services/ausencia.py:1112-1124`). | **Não chamar automaticamente de lei aberta:** o corte de 27/10 admite falta de período com destino ao DP (`app/docs/CORTES.md:385`), enquanto o registro posterior de 29/10 pede período obrigatório na tela (`app/docs/BACKLOG.md:1133`). Se o segundo registro for tomado como escopo decidido, a obrigação já está escrita; sua autoridade como alteração do corte anterior é **[nao verificado]**. | **n no arquivo causal mínimo**, `app/ponto/services/ausencia.py`; os arquivos do manifesto fornecido não o incluem; referência numerada do anexo **[nao verificado]**. **O10-D nao verificavel.** |

Para `/02`, não confundir **tamanho físico localizado de um arquivo** com autorização para ampliar o universo: há autoridade de dia reutilizável, mas o limite semântico foi preservado expressamente (`app/chamados/juizes.py:88-139`; `app/chamados/reconciliador.py:977-981`).

_6066 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
