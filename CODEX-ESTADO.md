# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 03:44** · publicado em **20/11/2026 03:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 03:44 · sessao 642381ebabdc · E4C-F5-CONSTROI · construtor · origem=Code

**Volta D parcial.** Migrei 11 das 13 menções. Preservei o `refresh` e a guarda de `devolver_validacao_recusada` pelo bloqueio do item 5. A catraca nova acusa esses dois pontos e três leituras nos dois juízes estruturais. (`chamados/services/validacao.py:262–280`, `chamados/juizes.py:1391`, `chamados/juizes.py:1560`, `chamados/tests/test_e4c_f5_selo_catraca.py:269`)

Compilei em memória os oito arquivos e os 14 mutantes, conferi as âncoras e executei isoladamente os casos sintéticos do selo. REDs de banco, mortes dos mutantes, tetos e impacto em produção ficam **[nao verificado]**. (`chamados/tests/e4c_f5_voltaD_mutantes.py:13`, `chamados/tests/test_e4c_f5_selo_catraca.py:285–322`)

### Sítios

Os caminhos da tabela são relativos a `chamados/`. As linhas à esquerda são da base. Os custos são de leitura das decisões por inspeção, com **0 quando há prefetch e 1 quando a composta precisa ler a relação**. A medição fica **[nao verificado]**. (`chamados/models.py:4321–4334`, `chamados/models.py:4354–4362`)

| Antes → depois | Consultas adicionais | RED em `tests/test_e4c_f5_voltaD_portas.py` | Mutante |
|---|---|---|---|
| `services/acoes_disputa.py:349 → :349` · corrigir usa a composta da pergunta | +1 no `.get()` avulso | `:70` | `corrigir` |
| `services/declaracao_texto.py:228 → :229` · guarda compartilhada usa a composta | +0 com a candidata ou testemunha prefetched. +1 após releitura integral ou se `abrir` devolver instância sem prefetch | `:88`, `:109`, `:165`, `:182` | `declaracao` |
| `services/disputa_emissao.py:2085 → :2085` · seleção S143 usa a composta | +0, prefetch existente em `:2080` | `:136`, `:150` | `fecho`, `fecho_sem_lote` |
| `services/materializacao.py:200 → :201` · plantio exige V da irmã | +1 por irmã encontrada, compartilhada com a guarda seguinte | `:220` | `irma_plantio` |
| `services/materializacao.py:202 → :203` · implausibilidade usa V da mesma irmã | +0 depois da composta em `:200` | `:230` | `irma_implausivel` |
| `services/materializacao.py:2128 → :2130` · usa a composta da instância relida sob trava | +1 por candidata que chega à guarda após o `.get()` de `:2124` | `:244` | `materializar` |
| `services/validacao.py:262 → :262` · refresh parcial preservado | +0 | Diagnóstico em `:370` | — |
| `services/validacao.py:269 → :270` · guarda de devolução preservada | +0 | Diagnóstico em `:382` | — |
| `services/validacao.py:640 → :641` · carimbo do par usa a composta da irmã | +1 por par aceito, pois a irmã chega sem prefetch | `:271` | `validar_par` |
| `services/validacao.py:685 → :687` · reutiliza `leitura` de `:683` | +0 | `:295` | `validar` |
| `services/validacao.py:898 → :901` · antecipa e reutiliza `antes_do_ato` | +0 quando o fluxo já chegava à composta. +1 quando a coluna preenchida antes encerrava a chamada sem consultá-la | `:307` | `recusar` |
| `services/validacao.py:1065 → :1067` · guarda de entrada usa a composta | +1 por lote com perguntas encontradas, +0 por item | `:319`, `:332` | `lote_entrada`, `lote_entrada_sem_prefetch` |
| `services/validacao.py:1086 → :1089` · classificação usa a composta após refresh integral | +1 por item que chamou a ação | `:344` | `lote_releitura` |

### Releituras, lotes e devolução

- **Refresh.** Para o Django pinado, `refresh_from_db(fields=[...])` preserva o prefetch de `decisoes` quando a relação fica fora de `fields`. `refresh_from_db()` integral descarta o prefetch. O diagnóstico exercita as duas formas com um ato posterior à carga. A confirmação no Django da sessão fica **[nao verificado]**. A materialização recebe outra instância pelo `.get()` depois da trava. O descarte explícito de `registrar` alcança a instância passada ao escritor. (`requirements.txt:13`, `chamados/tests/test_e4c_f5_voltaD_portas.py:370–380`, `chamados/services/materializacao.py:2122–2132`, `chamados/models.py:4381–4387`)
- **Declaração em laço.** `_porta_recusaria_por_carimbo` chama `_resposta_validada`. `projecao_por_marco` itera por marco. O fio e a API o chamam por cartão, usando `insumos_em_regularizacao`, que carrega `decisoes` em lote. Sem `por_fato`, a projeção já carrega por marco. `propostas_do_chamado` itera por follow-up e carrega uma vez por fato distinto. Os REDs cobram um SELECT de decisões em 3 e 6 fatos da tela e em 3 e 6 propostas do mesmo fato. O RED do S143 cobra o mesmo com 3 e 6 perguntas. (`chamados/services/declaracao_texto.py:280–287`, `:470–480`, `:585–592`, `:626–639`, `chamados/services/fio.py:415–425`, `api/views.py:1088–1094`, `chamados/tests/test_e4c_f5_voltaD_portas.py:150–206`)
- **Origem da irmã.** Plantio, tela e validação entram por `par_do_intervalo`. A própria função busca a irmã em `metade_irma_do_intervalo`, num queryset sem prefetch, por disputa, motivo complementar e dia. O plantio passa `irma_validada=True`. A validação recebe de volta essa mesma instância. (`chamados/services/materializacao.py:139–150`, `:195–203`, `:925–930`, `:1737–1740`, `chamados/services/validacao.py:632–643`)
- **Bloqueio da devolução.** Com ato validado e coluna nula, a troca gravaria `DEVOLUCAO`; a foto ainda poderia ficar igual e o retorno ser `None`. Com ato sem V e coluna preenchida, a troca deixaria `validada_em` e o autor residuais, onde hoje a porta os limpa. Isso piora a projeção. A fila do materializador e o vigia selecionam pela coluna preenchida, e os recortes de absolvíveis e órfãs do reconciliador exigem coluna nula. Por isso preservei `:262` e `:270`, sem acrescentar descarte de cache a essa porta. (`chamados/services/validacao.py:262–304`, `chamados/services/materializacao.py:2111–2115`, `chamados/services/vigia_carimbo.py:97–105`, `chamados/reconciliador.py:678–682`, `:1096–1103`)
- **Batida.** O fecho alcança S143 pelo `post_save` do chamado. A guarda alterada nesse caminho usa `_todas`, já prefetched. Os quatro tetos da cópia são **234 / 298** para um e três furos que fecham e **146 / 221** para um e três que ficam. O delta executado fica **[nao verificado]**. (`ponto/signals.py:131–135`, `chamados/models.py:308`, `chamados/signals.py:93–117`, `chamados/services/disputa_emissao.py:2080–2085`, `ponto/tests/test_selo_chokepoint_escrita.py:206`, `:230`, `:253`, `:267`)

### Selo e testes existentes [nao verificado]

- A varredura literal ficou vermelha na devolução e nos parâmetros estruturais de `projecao_temporal_da_pergunta` e `pergunta_respondida`. A regra local exige ancestralidade de `.composta()` ou `.leitura()` dentro do corpo. Os juízes recebem a leitura dos chamadores, e o placar entrega a estrutura da face SQL ao segundo juiz. Mantive o universo pedido e o selo anterior. (`chamados/tests/test_e4c_f5_selo_catraca.py:14–28`, `:269–279`, `chamados/juizes.py:1391`, `:1408–1409`, `:1560`, `chamados/services/raiox.py:135–147`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:210–251`)
- **K47** espera carimbo justamente para ato validado com projeção sem V. A expectativa de carimbo conflita com a guarda nova. O mutante existente **`retry_reativa`** também precisa de nova âncora, pois busca literalmente `if perg.validada_em:` em `validar_pergunta`. As duas pendências estão nos arquivos existentes. (`chamados/tests/test_e4c_f5_voltaB_lotes.py:197–216`, `chamados/tests/bug_par_mutantes.py:68–71`, `chamados/services/validacao.py:683–689`)

### Comandos para a sessão

Labels dos REDs, da catraca e dos quatro tetos. A catraca de produção acusa os pontos acima. (`chamados/tests/test_e4c_f5_voltaD_portas.py:23`, `chamados/tests/test_e4c_f5_selo_catraca.py:268–279`, `ponto/tests/test_selo_chokepoint_escrita.py:467–509`)

```bash
export PYTHONDONTWRITEBYTECODE=1

python3 manage.py test chamados.tests.test_e4c_f5_voltaD_portas
python3 manage.py test chamados.tests.test_e4c_f5_selo_catraca

teto=ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest
python3 manage.py test \
  "$teto.test_registrar_batida_com_1_furo_vivo_que_ela_fecha" \
  "$teto.test_registrar_batida_com_3_furos_vivos_que_ela_fecha" \
  "$teto.test_registrar_batida_com_1_furo_vivo_que_fica" \
  "$teto.test_registrar_batida_com_3_furos_vivos_que_ficam"
```

Cada comando abaixo ativa um mutante e executa o teste que deve falhar na asserção de negócio ou de custo. (`chamados/tests/e4c_f5_voltaD_mutantes.py:13–69`, `chamados/tests/test_e4c_f5_voltaD_portas.py:70–368`)

```bash
m() {
  F5_VOLTA_D_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    "chamados.tests.test_e4c_f5_voltaD_portas.PortasVoltaDTest.test_RED_$2"
}

m corrigir corrigir
m declaracao declaracao
m fecho fecho
m irma_plantio irma_plantio
m irma_implausivel irma_implausivel
m materializar materializar_ato_mudou_sob_trava
m validar_par validar_par
m validar validar
m recusar recusar
m lote_entrada lote_entrada
m lote_releitura lote_rele_ato_escrito_em_outra_instancia
m fecho_sem_lote fecho_3x6
m tela_sem_lote tela_3x6
m lote_entrada_sem_prefetch lote_entrada_3x6
```

`git status --porcelain` observado, com os caminhos relativos à raiz Git.

```text
 M app/chamados/services/acoes_disputa.py
 M app/chamados/services/declaracao_texto.py
 M app/chamados/services/disputa_emissao.py
 M app/chamados/services/materializacao.py
 M app/chamados/services/validacao.py
?? app/chamados/tests/e4c_f5_voltaD_mutantes.py
?? app/chamados/tests/test_e4c_f5_selo_catraca.py
?? app/chamados/tests/test_e4c_f5_voltaD_portas.py
```

## 20/11 03:41 · sessao 3c33ad11d24f · E4C-F5-CONSTROI · sonda · origem=Code

## FUNCOES REAIS USADAS

- **M1** — `DecisaoPergunta.composta(p)` e a leitura que usa o cache de `prefetch_related('decisoes')`. `app/chamados/models.py:4354`, `app/chamados/models.py:4329`
- **M2 e M4** — `DisputaSupervisao.aberta`, `_dia_de_turno_da_pergunta(p)`, `competencia_do_dia_entregue(dia, empresa, memo=None)` → `competencia_entregue(empresa_id, mes, ano, registro=False, *, exportacoes=None)` e `dia_da_pergunta_trancado(p)`. `app/chamados/models.py:1979`, `app/chamados/juizes.py:337`, `app/chamados/services/vigia_fio.py:789`, `app/folha/export.py:556`, `app/chamados/juizes.py:979`
- **M3** — `pergunta_respondida`, `_resposta_de_hora`, `metade_irma_do_intervalo`, `q_respondida_com_texto` e `q_respondida_sem_texto`. Cada guarda tem sua referência no script. `app/chamados/juizes.py:1551`, `app/chamados/services/validacao.py:185`, `app/chamados/services/materializacao.py:139`, `app/chamados/juizes.py:1583`, `app/chamados/juizes.py:1573`
- **M4** — `DecisaoPergunta.es_da_ressuscitacao(d)` e `exigencia_das_perguntas(perguntas, agora)`, que alimenta `_pela_decisao` com a carga real da tarefa. `app/chamados/models.py:4246`, `app/chamados/services/tarefa_do_dia.py:461`, `app/chamados/services/tarefa_do_dia.py:142`

```python
from collections import Counter, defaultdict
from functools import lru_cache
from itertools import product
from unittest.mock import patch


# E4C-F5-CONSTROI — hash solicitado f62a39996b3c6e41aa866a08e8d6c51ff17f05af.
# Identidade Git da árvore e execução da sonda [nao verificado].
SIM = "sim"
NAO = "nao"
NAO_AVALIADO = "nao-avaliado"
SEM_DIA = "sem-dia"
FALHOU = object()

VALIDAR = "validar_pergunta:685"
RECUSAR = "recusar_pergunta:898"
DEVOLVER = "devolver_validacao_recusada:269"
VALIDAR_PAR = "_validar_o_par:640"
LOTE = "validar_lote_classe_a:1065"
CORRIGIR = "acao_corrigir_resposta:349"
RESPOSTA = "_resposta_validada:228"
RESOLVER = "_resolver_perguntas_por_chamado_resolvido:2085"
PAR_200 = "par_do_intervalo:200"
PAR_202_PADRAO = "par_do_intervalo:202(irma_validada=False)"
PAR_202_PLANTIO = "par_do_intervalo:202(irma_validada=True)"
MATERIALIZAR = "materializar_perguntas_validadas_da_disputa:2128"

PORTAS = (
    VALIDAR, RECUSAR, DEVOLVER, VALIDAR_PAR, LOTE, CORRIGIR, RESPOSTA,
    RESOLVER, PAR_200, PAR_202_PADRAO, PAR_202_PLANTIO, MATERIALIZAR,
)
ROTULO = {True: SIM, False: NAO}
DIVERGENCIAS = {
    (True, False): "ato-sim/coluna-nao",
    (False, True): "ato-nao/coluna-sim",
}

ERROS = []
m1 = Counter()
m1_pks = defaultdict(list)
m2 = defaultdict(list)
m3 = {porta: Counter() for porta in PORTAS}
m3_irmas = defaultdict(list)
m4_pks = []
m4 = defaultdict(list)


def registrar_erro(pk, exc):
    ERROS.append((pk, type(exc).__name__))


def varrer():
    from django.contrib.auth.models import User
    from django.db import connection, transaction
    from django.utils import timezone
    from django_tenants.utils import schema_context

    from chamados.juizes import (
        _dia_de_turno_da_pergunta,
        dia_da_pergunta_trancado,
        pergunta_respondida,
        q_respondida_com_texto,
        q_respondida_sem_texto,
    )
    from chamados.models import DecisaoPergunta, PerguntaDisputa
    from chamados.services.materializacao import (
        _PAR_DO_INTERVALO,
        metade_irma_do_intervalo,
    )
    from chamados.services.tarefa_do_dia import exigencia_das_perguntas
    from chamados.services.validacao import _resposta_de_hora
    from chamados.services.vigia_fio import competencia_do_dia_entregue

    memo_k6 = {}

    class BancoOuSchemaIncorreto(RuntimeError):
        pass

    def medir(pk, leitura, *, sql=False):
        try:
            if sql:
                # Savepoint por leitura com SQL para seguir após erro de uma pergunta.
                with transaction.atomic():
                    return leitura()
            return leitura()
        except Exception as exc:
            registrar_erro(pk, exc)
            return FALHOU

    @lru_cache(maxsize=1)
    def ler_usuario_sistema():
        # C20 pode pedir esse usuário em app/chamados/juizes.py:1228-1234.
        # O auxiliar de produção usa get_or_create em app/core/utils.py:6-18.
        # A busca exclusiva por leitura mantém o mesmo usuário e propaga ausência.
        return User.objects.only("pk").get(username="sistema")

    def competencias(p):
        # Dia do turno em app/chamados/juizes.py:337. Conversa não tem dia do fato.
        # K6 em app/chamados/services/vigia_fio.py:789:
        #   competencia_do_dia_entregue(dia, empresa, memo=None) -> bool | None
        #   app/ponto/janelas.py:60 calcula a janela pelo corte da empresa.
        #   app/folha/export.py:556:
        #   competencia_entregue(empresa_id, mes, ano, registro=False, *, exportacoes=None)
        # C20 em app/chamados/juizes.py:979:
        #   dia_da_pergunta_trancado(p) -> bool, inclui o toque humano posterior.
        dia = _dia_de_turno_da_pergunta(p)
        k6 = competencia_do_dia_entregue(
            dia, p.disputa.colaborador.empresa, memo=memo_k6
        )
        c20 = dia_da_pergunta_trancado(p)
        return (SEM_DIA if k6 is None else ROTULO[k6], ROTULO[c20])

    def situacao_m2(p):
        # app/chamados/models.py:1979 e app/chamados/models.py:3671.
        return (
            "aberta" if p.disputa.aberta else "fechada",
            ROTULO[p.materializada_em is not None],
            *competencias(p),
        )

    def alcance_materializacao(p):
        # app/chamados/services/materializacao.py:2108-2129.
        # As duas faces SQL reais estão em app/chamados/juizes.py:1583 e :1573.
        prontas = p.disputa.perguntas.filter(
            q_respondida_com_texto() | q_respondida_sem_texto(),
            validada_em__isnull=False,
            materializada_em__isnull=True,
        )
        if not prontas.filter(pk=p.pk).exists():
            return NAO
        # Ordem padrão por id em app/chamados/models.py:3785.
        # [nao verificado] Uma iteração anterior pode materializar a irmã;
        # o laço relê e testa a materialização antes de :2128.
        if prontas.filter(pk__lt=p.pk).exists():
            return NAO_AVALIADO
        return SIM

    def medir_par(p, respondida, classe):
        # app/chamados/services/materializacao.py:136, :139 e :189-202.
        # Mede o prefixo do par. A busca real também identifica a irmã candidata
        # para o relatório quando a primária falha no teste de resposta. [nao verificado]
        q = medir(
            p.pk, lambda: metade_irma_do_intervalo(p.disputa, p), sql=True
        )
        if q is FALHOU:
            return

        chega_200 = (
            p.motivo in _PAR_DO_INTERVALO
            and respondida
            and q is not None
            and not getattr(q, "pendencia_admin", "")
        )
        m3[PAR_200][ROTULO[chega_200]] += 1
        m3[PAR_202_PADRAO][ROTULO[chega_200]] += 1
        m3[PAR_202_PLANTIO][ROTULO[chega_200 and q.validada_em is not None]] += 1

        # app/chamados/services/validacao.py:632-640 exige o par completo.
        # [nao verificado] Para quem passa pelo prefixo, confirmar o alcance
        # de :640 exige executar par_do_intervalo além de :202. A chamada
        # normal também ocorre após carimbo e recálculo em :743-771.
        m3[VALIDAR_PAR][NAO_AVALIADO if chega_200 else NAO] += 1

        if q is not None:
            leitura_irma = medir(q.pk, lambda: DecisaoPergunta.composta(q), sql=True)
            if leitura_irma is not FALHOU:
                chave = (
                    classe,
                    ROTULO[leitura_irma.validada_em is not None],
                    ROTULO[q.validada_em is not None],
                )
                m3_irmas[chave].append((p.pk, q.pk))

    def medir_portas(p, composta, classe):
        # ALCANCA mede a chegada à linha indicada na foto lida. Uma condição
        # testada na própria linha pode barrar o passo seguinte.
        # app/chamados/services/validacao.py:673-685 e app/chamados/juizes.py:1551.
        # Entrada pelo pk da própria pergunta, com a disputa correspondente.
        respondida = medir(p.pk, lambda: pergunta_respondida(composta))
        if respondida is not FALHOU:
            m3[VALIDAR][ROTULO[respondida]] += 1
            medir_par(p, respondida, classe)

        # app/chamados/services/validacao.py:886-898.
        # [nao verificado] Motivo não vazio e seleção vêm da chamada de recusa.
        m3[RECUSAR][NAO_AVALIADO] += 1

        # app/chamados/services/validacao.py:185 e :262-269.
        # A porta relê estes campos. Aqui a entrada é a foto atual.
        chega_devolver = medir(
            p.pk,
            lambda: (
                _resposta_de_hora(p)
                and p.materializada_em is None
                and bool((p.recusa_motivo or "").strip())
            ),
        )
        if chega_devolver is not FALHOU:
            m3[DEVOLVER][ROTULO[chega_devolver]] += 1

        # app/chamados/services/validacao.py:1046-1068.
        # Cenário explícito de entrada unitária brutos=[p.pk]. A linha já
        # existe na foto, e a classificação do lote vem depois da guarda.
        m3[LOTE][SIM] += 1

        # app/chamados/services/acoes_disputa.py:338-349.
        # [nao verificado] A guarda depende do POST de hora, motivo e pk.
        m3[CORRIGIR][NAO_AVALIADO] += 1

        # app/chamados/services/declaracao_texto.py:217-228.
        # Entrada direta com a pergunta. A expressão de :228 é o corpo inteiro.
        m3[RESPOSTA][SIM] += 1

        # app/chamados/services/disputa_emissao.py:1946, :1988, :2031,
        # :2056 e :2071-2085. A linha está no ramo da disputa aberta cujo pai
        # e destinos estão fora de VIVOS (app/chamados/catalogo/motor.py:24).
        # [nao verificado] Falta o chamado disparador. Reaberturas e carimbos
        # anteriores no mesmo fluxo podem mudar a elegibilidade desse ramo.
        m3[RESOLVER][NAO_AVALIADO] += 1

        situacao = medir(p.pk, lambda: alcance_materializacao(p), sql=True)
        if situacao is not FALHOU:
            m3[MATERIALIZAR][situacao] += 1

    def medir_m4(p, agora):
        # app/chamados/models.py:4225 e :4246. O último ato real é escolhido
        # em ordem (ato_em, pk), antes de retirar a marca para a leitura de T.
        ultimo = next(
            (d for d in reversed(list(p.decisoes.all())) if d.ato_em <= agora),
            None,
        )
        if ultimo is None or not DecisaoPergunta.es_da_ressuscitacao(ultimo):
            return

        # app/chamados/services/tarefa_do_dia.py:461-474 chama
        # _pela_decisao (:142) com o horizonte, o passivo e a sequência real
        # da tarefa, que retira a reabertura marcada em :393-401.
        razao = medir(
            p.pk,
            lambda: exigencia_das_perguntas([p.pk], agora)[p.pk][0],
            sql=True,
        )
        # app/chamados/services/tarefa_do_dia.py:182-183.
        if razao is FALHOU or razao != "silencio_decidido":
            return
        m4_pks.append(p.pk)
        faixa = medir(p.pk, lambda: competencias(p), sql=True)
        if faixa is not FALHOU:
            m4[faixa].append(p.pk)

    def medir_pergunta(p, agora):
        # app/chamados/models.py:4354 e :2659.
        composta = medir(p.pk, lambda: DecisaoPergunta.composta(p))
        if composta is not FALHOU:
            celula = (composta.validada_em is not None, p.validada_em is not None)
            m1[celula] += 1
            if celula in DIVERGENCIAS:
                classe = DIVERGENCIAS[celula]
                m1_pks[classe].append(p.pk)
                faixa = medir(p.pk, lambda: situacao_m2(p), sql=True)
                if faixa is not FALHOU:
                    m2[(classe, *faixa)].append(p.pk)
                medir_portas(p, composta, classe)

        medir_m4(p, agora)

    # PostgreSQL tenant-aware em app/config/settings/saas.py:83-93.
    # Banco sombra e schema juliani em app/config/settings/sombra.py:13-16, :29 e :49.
    with schema_context("juliani"), transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise BancoOuSchemaIncorreto()

        agora = timezone.now()
        universo = (
            PerguntaDisputa.objects.select_related("disputa__colaborador__empresa")
            .prefetch_related("decisoes")
            .order_by("pk")
        )
        with patch("core.utils.get_user_sistema", new=ler_usuario_sistema):
            for p in universo.iterator(chunk_size=2000):
                try:
                    medir_pergunta(p, agora)
                except Exception as exc:
                    registrar_erro(p.pk, exc)


try:
    varrer()
except Exception as exc:
    # Falha de conexão, schema, carga de lote ou outra falha sem pk atribuível.
    registrar_erro(None, exc)


def emitir(linha):
    print(("PARCIAL " if ERROS else "") + linha)


emitir(f"M1 TOTAL N={sum(m1.values())}")
for ato, coluna in product((True, False), repeat=2):
    chave = (ato, coluna)
    linha = f"M1 ATO={ROTULO[ato]} COLUNA={ROTULO[coluna]} N={m1[chave]}"
    if chave in DIVERGENCIAS:
        linha += f" PKS={m1_pks[DIVERGENCIAS[chave]]}"
    emitir(linha)

for classe, disputa, materializada, k6, c20 in product(
    DIVERGENCIAS.values(), ("aberta", "fechada"), (SIM, NAO),
    (SIM, NAO, SEM_DIA), (SIM, NAO),
):
    pks = m2[(classe, disputa, materializada, k6, c20)]
    emitir(
        f"M2 CLASSE={classe} DISPUTA={disputa} MATERIALIZADA={materializada} "
        f"K6={k6} C20={c20} N={len(pks)} PKS={pks}"
    )

for porta in PORTAS:
    for alcance in (SIM, NAO, NAO_AVALIADO):
        emitir(f"M3 PORTA={porta} ALCANCA={alcance} N={m3[porta][alcance]}")

for (classe, ato_irma, coluna_irma), pares in sorted(m3_irmas.items()):
    emitir(
        f"M3 CLASSE={classe} IRMA_ATO={ato_irma} IRMA_COLUNA={coluna_irma} "
        f"N={len(pares)} PKS_DIVERGENTE_IRMA={pares}"
    )

emitir(f"M4 SILENCIO_DECIDIDO N={len(m4_pks)} PKS={m4_pks}")
for k6, c20 in product((SIM, NAO, SEM_DIA), (SIM, NAO)):
    pks = m4[(k6, c20)]
    emitir(f"M4 K6={k6} C20={c20} N={len(pks)} PKS={pks}")

for pk, tipo in ERROS:
    print(f"ERRO PK={pk} TIPO={tipo}")
print(f"ERROS={len(ERROS)}")
```

## 20/11 03:39 · sessao 4e111026f006 · - · - · origem=Ronald (tmux)

Para M3, as pré-condições das linhas pedidas são:

- `validar_pergunta:685` é alcançada após localizar a pergunta e obter `pergunta_respondida(DecisaoPergunta.composta(p))`. [app/chamados/services/validacao.py:673](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/validacao.py:673>)
- `recusar_pergunta:898` exige motivo não vazio e pergunta localizada no filtro de pk/disputa. Sem entrada de motivo, a chamada concreta fica `nao-avaliado`. [app/chamados/services/validacao.py:886](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/validacao.py:886>)
- `devolver_validacao_recusada:269` exige `_resposta_de_hora(p)`, `materializada_em is None` e motivo de recusa não vazio após trim. É aferível para o estado atual da pergunta. [app/chamados/services/validacao.py:262](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/validacao.py:262>)
- `_validar_o_par:640` exige retorno com irmã de `par_do_intervalo`, uma composição de leitura. A chamada da porta do admin ocorre depois de carimbar a pergunta e tentar recalcular o fechamento. Diferencie fotografia atual do alcance naquele fluxo. [app/chamados/services/validacao.py:632](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/validacao.py:632>) [app/chamados/services/validacao.py:743](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/validacao.py:743>)
- `validar_lote_classe_a:1065` é alcançada quando o pk está nos parâmetros normalizados do lote e foi encontrado. Depois da guarda, a candidatura requer `classe_do_lote(p)[0] == LOTE_A`. [app/chamados/services/validacao.py:1046](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/validacao.py:1046>)

C20 chama indiretamente `get_user_sistema`, que usa `get_or_create`. A sonda deve buscar o pk por `get` e semear `tranca_da_consulta([p], memo_inicial={('sistema',): sistema_pk})` para chamar o juiz real sem chegar à porta de escrita. [app/chamados/juizes.py:1133](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/juizes.py:1133>) [app/chamados/juizes.py:1228](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/juizes.py:1228>) [app/core/utils.py:18](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/core/utils.py:18>)

M4 pode usar `exigencia_das_perguntas([p.pk], agora)[p.pk][0]`, que chama `_pela_decisao` com os mesmos fatos da tarefa e retira a reabertura marcada antes de escolher o último ato para a tarefa. A seleção da reabertura marcada usa a sequência anterior a esse filtro, em ordem `ato_em, pk`, no mesmo horizonte. [app/chamados/services/tarefa_do_dia.py:393](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/tarefa_do_dia.py:393>) [app/chamados/services/tarefa_do_dia.py:461](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/tarefa_do_dia.py:461>) [app/chamados/models.py:4225](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/models.py:4225>) [app/chamados/models.py:4246](</home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/models.py:4246>)

## 20/11 03:38 · sessao b3ba093e2bcb · - · - · origem=Ronald (tmux)

- `acao_corrigir_resposta` chega à guarda após receber hora no formato exigido, motivo preenchido e PK existente. Para medir uma chamada concreta seria necessário o POST. Disputa fechada e tipo diferente de `hora` são guardas posteriores. [app/chamados/services/acoes_disputa.py:306](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/acoes_disputa.py:306) [app/chamados/services/acoes_disputa.py:338](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/acoes_disputa.py:338)
- `_resposta_validada` avalia `perg.validada_em is not None` assim que recebe a pergunta. Para o alcance indireto pela declaração, a escolha da pergunta vem depois da emissão e pode passar por abertura ou reapontamento. [app/chamados/services/declaracao_texto.py:217](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/declaracao_texto.py:217) [app/chamados/services/declaracao_texto.py:361](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/declaracao_texto.py:361) [app/chamados/services/declaracao_texto.py:660](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/declaracao_texto.py:660)
- `par_do_intervalo` chega à primeira guarda da irmã se o motivo existe em `_PAR_DO_INTERVALO`, a primária responde ao juiz composto, `metade_irma_do_intervalo` acha uma irmã e esta está sem pendência administrativa. A segunda guarda exige também `not irma_validada or q.validada_em is not None`. `par_do_intervalo` é leitor puro e sua aceitação antecede a guarda de `_validar_o_par`. O fluxo normal até `_validar_o_par` já carimbou a primária e tentou recalcular por evento. [app/chamados/services/materializacao.py:136](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/materializacao.py:136) [app/chamados/services/materializacao.py:175](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/materializacao.py:175) [app/chamados/services/validacao.py:632](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/validacao.py:632) [app/chamados/services/validacao.py:743](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/validacao.py:743)
- `_resolver_perguntas_por_chamado_resolvido` usa a guarda pedida no ramo sem credor. Exige chamado de entrada com via diferente da tranca, disputa aberta, pai e todos os destinos fora de `VIVOS`. A seleção de `materializada_em is None and validada_em is None` vem depois de operações capazes de reabrir credores ou mexer em carimbos. O alcance de uma execução concreta é `nao-avaliado`. [app/chamados/services/disputa_emissao.py:1946](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/disputa_emissao.py:1946) [app/chamados/services/disputa_emissao.py:1987](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/disputa_emissao.py:1987) [app/chamados/services/disputa_emissao.py:2071](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/disputa_emissao.py:2071)
- `materializar_perguntas_validadas_da_disputa` enfileira pela união de `q_respondida_com_texto` e `q_respondida_sem_texto`, carimbo de validação presente e materialização nula. Dentro do laço, relê e testa materialização antes da guarda de validação. A foto inicial permite contar a fila. Uma iteração anterior pode alterar o alcance efetivo de `:2128`. [app/chamados/services/materializacao.py:2108](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/materializacao.py:2108)
- Para M4, `exigencia_das_perguntas([p], agora)` chama `_pela_decisao` com a carga oficial. A carga retira a reabertura marcada da sequência entregue ao leitor e usa o horizonte de `agora`. A seleção do último ato bruto para o censo pode usar a ordem do modelo `(ato_em, pk)` no prefetch. [app/chamados/models.py:4224](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/models.py:4224) [app/chamados/services/tarefa_do_dia.py:387](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/tarefa_do_dia.py:387) [app/chamados/services/tarefa_do_dia.py:461](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/tarefa_do_dia.py:461)

## 20/11 03:38 · sessao 5fffde06046a · - · - · origem=Ronald (tmux)

- **M2** usa `DisputaSupervisao.aberta` e `PerguntaDisputa.materializada_em`. O dia do fato vem de `_dia_de_turno_da_pergunta(p)`, que devolve `None` para conversa ou data ausente. [models.py:1979](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/models.py:1979), [models.py:3671](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/models.py:3671), [juizes.py:337](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/juizes.py:337)
- **K6** chama `competencia_do_dia_entregue(dia, empresa, memo=None)`, que calcula a janela da empresa e invoca `competencia_entregue(empresa_id, mes, ano, registro=False, *, exportacoes=None)`. Preserve o retorno `None` como sem dia do fato. [vigia_fio.py:789](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/vigia_fio.py:789), [export.py:556](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/folha/export.py:556)
- **C20** chama `dia_da_pergunta_trancado(p)`. O caminho sob tranca ativa pode invocar `get_user_sistema`, implementado com `get_or_create`. Durante a sonda, a substituição localizada desse acesso por `User.objects.only('pk').get(username='sistema')` mantém a leitura e propaga a ausência para `ERROS`. [juizes.py:979](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/juizes.py:979), [juizes.py:1228](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/juizes.py:1228), [utils.py:18](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/core/utils.py:18)
- **M4** seleciona a última decisão real no horizonte, na ordem `ato_em, pk`. `DecisaoPergunta.es_da_ressuscitacao` verifica o ato e a marca. `exigencia_das_perguntas([p.pk], agora)[p.pk][0]` chama `_pela_decisao` com a carga oficial, que retira o ato compensador da sequência e preserva as regras de passivo e horizonte. [models.py:4225](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/models.py:4225), [models.py:4246](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/models.py:4246), [tarefa_do_dia.py:393](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/tarefa_do_dia.py:393), [tarefa_do_dia.py:461](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/services/tarefa_do_dia.py:461)
- O C6 captura exceções e registra contexto por `registrar_engolido` antes de retornar uma resposta. Interceptar esse ponto durante a sonda permite informar tipo e PK sem expor o conteúdo bruto. [juizes.py:1484](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/chamados/juizes.py:1484), [observ.py:73](/home/ronald/codex_ws/f62a39996b3c6e41aa866a08e8d6c51ff17f05af/app/core/observ.py:73)

## 20/11 03:28 · sessao 16df42e2324f · E4C-F5-CONSTROI · censo · origem=Code

**34 menções. Em produção, 0 leituras decisórias de instância ORM com aval literal e 13 com NENHUM.** As 13 estão nas guardas e no `refresh_from_db` discriminados abaixo. O `refresh_from_db` de `validacao.py:262` conta como menção própria porque carrega a coluna usada na decisão de `validacao.py:269`. A identidade Git da cópia e qualquer comparação com `eb1e49c1e` ficam [nao verificado].

## Censo por menção

Os caminhos de código partem de `app/chamados/`. As linhas de `brief_c3.md` referem-se ao anexo fornecido. **V** = `validada_em`, **R** = `respondida_em`, **T** = `resposta_colab`. A coluna da matriz reproduz o que o brief pedia. O aval de **19/11 15:10, itens 1–2** determina que T permaneça na projeção e que os instantes venham do ato, conforme a composição em `models.py:4360–4362`. **NENHUM** na coluna de teste significa que procurei um assert em que a divergência alterasse o resultado **da menção recenseada**. [nao verificado]

| Arquivo:linha e menção | Objeto e origem | O que o valor faz | O que a matriz mandava ler | Aval literal para ORM decisório | Assert com divergência |
|---|---|---|---|---|---|
| `juizes.py:1391` `p.validada_em` | Parâmetro estrutural de `:1383`; composta nos chamadores de produção (`:351–352`, `:1408–1409`, `../escala/services/cadastro_realidade.py:498–499`). Há ORM em teste de passivo (`tests/test_e4c_f5_volta2_juizes.py:60–64`). | Decide o corte de V no horizonte e o V retornado (`:1395–1396`, `:1403`); V também encerra o ramo do C6 (`:1410–1411`). | V/R/T da leitura, com horizonte; materialização e via da projeção (`brief_c3.md:33`). | —¹ | `tests/test_e4c_f5_volta2_juizes.py:56`, `:85` |
| `juizes.py:1391` `p.respondida_em` | Mesmo parâmetro estrutural, com composta em produção (`:1383`, `:351–352`, `:1408–1409`, `../escala/services/cadastro_realidade.py:498–499`) e ORM em teste de passivo (`tests/test_e4c_f5_volta2_juizes.py:60–64`). | Decide se corta R e T e quais valores retorna (`:1397–1398`, `:1403`). | V/R/T da leitura, com horizonte; materialização e via da projeção (`brief_c3.md:33`). | —¹ | `tests/test_e4c_f5_volta2_juizes.py:41`, `:49`, `:84` |
| `juizes.py:1560` `p.respondida_em` | Parâmetro estrutural de `:1551`; composta, por exemplo, em `:1428` e `models.py:3816`; `SimpleNamespace` de tupla SQL em `services/raiox.py:137–147`; ORM em teste de passivo (`tests/test_e4c_f5_volta2_juizes.py:60–65`). | Decide o retorno booleano junto com T (`:1560`). | R/T da leitura do ato quando o argumento for uma pergunta (`brief_c3.md:34`). | —¹ | `tests/test_e4c_f5_volta2_juizes.py:98`, `:105` |
| `services/acoes_disputa.py:349` `perg.validada_em` | ORM, `PerguntaDisputa.objects.select_related(...).get(...)` (`:345–346`). | Decide se retorna «já validada» antes de corrigir (`:349–350`, `:374–376`). | V da leitura (`brief_c3.md:35`). | NENHUM | NENHUM |
| `services/declaracao_texto.py:228` `perg.validada_em` | ORM, parámetro de `:217`; as portas passam a candidata do universo ORM ou a pergunta de `PerguntaDisputa.abrir` (`:250–255`, `:364–379`, `:663–669`; `services/fio.py:1960–1964`). | Decide o retorno da guarda usada pelas duas portas e pelo leitor de tela (`:228`, `:286`, `:379`, `:669`). | V da leitura (`brief_c3.md:36`). | NENHUM | NENHUM |
| `services/disputa_emissao.py:2097` chave `'respondida_em'` | Outro, chave do dict `_antes` (`:2095–2097`). | Só foto anterior do evento (`:2107–2112`). | V da leitura e materialização da projeção na seleção; a matriz descreve essa seleção (`brief_c3.md:37`). | — | NENHUM |
| `services/disputa_emissao.py:2097` `q.respondida_em`, condição do ternário | ORM, `q` de `_abertas`, vinda de `_d.perguntas.all()` (`:2080`, `:2084–2089`). | Escolhe entre data serializada e `None` na foto anterior (`:2097`, `:2112`). | V da leitura e materialização da projeção na seleção (`brief_c3.md:37`). | — | NENHUM |
| `services/disputa_emissao.py:2085` `q.validada_em` | ORM, `q` da lista de `_d.perguntas.all().prefetch_related('decisoes')` (`:2080`, `:2084–2085`). | Decide entrada em `_abertas`, que alimenta o laço do carimbo (`:2084–2089`, `:2105`). | V da leitura; materialização da projeção (`brief_c3.md:37`). | NENHUM | NENHUM. O assert K47 espera que as perguntas da fixture sejam carimbadas (`tests/test_e4c_f5_voltaB_lotes.py:202–213`). |
| `services/disputa_emissao.py:2114` chave `'respondida_em'` | Outro, chave do dict `depois` (`:2113–2115`). | Só foto posterior ao carimbo (`:2105–2108`, `:2113–2115`). | V da leitura e materialização da projeção na seleção (`brief_c3.md:37`). | — | NENHUM |
| `services/disputa_emissao.py:2097` `q.respondida_em`, receptor de `.isoformat()` | ORM, `q` de `_abertas` (`:2080`, `:2084–2089`). | Só serializa a data da foto anterior (`:2097`, `:2112`). | V da leitura e materialização da projeção na seleção (`brief_c3.md:37`). | — | NENHUM |
| `services/disputa_emissao.py:2114` `q.respondida_em`, condição do ternário | ORM, o mesmo `q` após `q.carimbar(...)` (`:2089`, `:2105`, `:2114`). | Escolhe entre data serializada e `None` na foto posterior (`:2113–2115`). | V da leitura e materialização da projeção na seleção (`brief_c3.md:37`). | — | NENHUM |
| `services/disputa_emissao.py:2114` `q.respondida_em`, receptor de `.isoformat()` | ORM, o mesmo `q` após `q.carimbar(...)` (`:2089`, `:2105`, `:2114`). | Só serializa a data da foto posterior (`:2113–2115`). | V da leitura e materialização da projeção na seleção (`brief_c3.md:37`). | — | NENHUM |
| `services/fio.py:190` `leitura.respondida_em` | Composta, `leitura = DecisaoPergunta.composta(p)` (`:189`; `models.py:4360–4362`). | Decide o retorno «espera texto», com curto-circuito antes de `respondida_sem_texto` (`:190`). | R/T da leitura, preservando o predicado (`brief_c3.md:38`). | — | `tests/test_e4c_f5_volta1_leem_o_ato.py:50`, `:57` |
| `services/materializacao.py:202` `q.validada_em` | ORM, irmã devolvida por `metade_irma_do_intervalo` (`:195`, `:146–150`). | Decide se consulta implausibilidade e pode retornar impedimento (`:202–207`). | V da leitura da irmã (`brief_c3.md:40`). | NENHUM | NENHUM. O RED em `tests/test_e4c_f5_voltaC_sitios.py:97` exercita o outro campo, R da própria metade (`services/materializacao.py:193–197`). |
| `services/materializacao.py:200` `q.validada_em` | ORM, irmã devolvida por `metade_irma_do_intervalo` (`:195`, `:146–150`). | Com `irma_validada=True`, decide se retorna impedimento por falta de validação da irmã (`:200–201`). | V da leitura da irmã (`brief_c3.md:39`). | NENHUM | NENHUM |
| `services/materializacao.py:2128` `p.validada_em` | ORM, `PerguntaDisputa.objects.get(pk=p.pk)` após a trava (`:2120–2122`). | Decide se pula `_materializar_uma` (`:2128–2130`). | V da leitura após a trava; materialização da projeção (`brief_c3.md:41`). | NENHUM | NENHUM |
| `services/validacao.py:640` `q.validada_em` | ORM, irmã devolvida por `par_do_intervalo` (`:632`; `services/materializacao.py:146–150`, `:195`, `:237`). | Decide se carimba a validação da irmã antes de materializar (`:639–643`). | V da leitura da irmã (`brief_c3.md:42`). | NENHUM | NENHUM |
| `services/validacao.py:685` `perg.validada_em` | ORM, `PerguntaDisputa.objects.get(**filtro)` (`:673–677`). | Decide o retorno `ja_validada` antes do carimbo (`:685–687`, `:743`). | V da leitura; materialização da projeção (`brief_c3.md:43`). | NENHUM | NENHUM. O assert em `tests/test_e4c_f5_voltaB_sitios.py:108` sai por `sem_resposta`, na guarda anterior (`services/validacao.py:682–684`). |
| `services/validacao.py:789` `'validada_em'` em `refresh_from_db` | ORM, a mesma `perg` do `.get` (`:677`, `:789`). | Recarrega V. O restante do corpo usa `materializada_em` e os resultados da porta (`:790–837`). | V da leitura na guarda de entrada; materialização da projeção (`brief_c3.md:43`). | — | NENHUM |
| `services/validacao.py:898` `perg.validada_em` | ORM, `PerguntaDisputa.objects.select_related(...).get(...)` (`:894–895`). | Decide o retorno `ja_validada` antes de gravar motivo e reabrir (`:898–900`, `:921–927`). | V da leitura; motivo da recusa na projeção (`brief_c3.md:44`). | NENHUM | NENHUM |
| `services/validacao.py:914` chave `'respondida_em'` | Outro, chave do dict `antes` (`:913–915`). | Só foto anterior do evento (`:935–938`). | V da leitura na guarda; motivo da recusa na projeção (`brief_c3.md:44`). | — | NENHUM |
| `services/validacao.py:914` `perg.respondida_em`, condição do ternário | ORM, `perg` do `.get` (`:894–895`). | Escolhe entre data serializada e `None` na foto anterior (`:913–915`, `:935–938`). | V da leitura na guarda (`brief_c3.md:44`). | — | NENHUM. `tests/test_e4c_f5_voltaB_sitios.py:136–138` verifica a foto da coluna. |
| `services/validacao.py:938` chave `'respondida_em'` | Outro, chave do dict `depois`, com `None` literal (`:935–940`). | Só foto posterior do evento (`:935–940`). | V da leitura na guarda (`brief_c3.md:44`). | — | NENHUM |
| `services/validacao.py:914` `perg.respondida_em`, receptor de `.isoformat()` | ORM, `perg` do `.get` (`:894–895`). | Só serializa a data da foto anterior (`:913–915`, `:935–938`). | V da leitura na guarda (`brief_c3.md:44`). | — | NENHUM. `tests/test_e4c_f5_voltaB_sitios.py:136–138` verifica a foto da coluna. |
| `services/validacao.py:1065` `perg.validada_em` | ORM, `achadas.get(ref)` sobre o mapa de `PerguntaDisputa.objects.filter(...)` (`:1057–1061`). | Decide rejeitar o item antes de chamar a ação de validação (`:1065–1067`, `:1079`). | V da leitura na guarda de entrada do lote (`brief_c3.md:45`). | NENHUM | NENHUM |
| `services/validacao.py:1086` `perg.validada_em` | ORM, a instância do mapa relida após a ação (`:1057–1061`, `:1079–1086`). | Decide os contadores e as listas devolvidos pelo lote (`:1086–1096`). | A matriz descreve V da leitura para a guarda de entrada (`brief_c3.md:45`). | NENHUM | NENHUM |
| `services/validacao.py:269` `perg.validada_em` | ORM, parâmetro de `:237`, vindo do `.get` do chamador e relido por `refresh_from_db` (`:677`, `:781`, `:262–263`). | Decide se limpa V/autor e registra `DEVOLUCAO` (`:269–282`). | V da leitura após o refresh; materialização, motivo e autor na projeção (`brief_c3.md:47`). | NENHUM | NENHUM |
| `services/validacao.py:273` `perg.validada_em` (`Store`) | ORM, a instância recebida e relida (`:237`, `:262–263`, `:781`). | Escreve `None` no próprio campo após a decisão (`:269–276`). | V decisória pela leitura; a linha da matriz descreve a guarda (`brief_c3.md:47`). | — | NENHUM |
| `services/validacao.py:262` `'validada_em'` em `refresh_from_db` | ORM, parâmetro `perg` recebido de `validar_pergunta` (`:237`, `:677`, `:781`). | Lê a coluna que alimenta a decisão de limpar V e registrar a devolução (`:262–279`); também alimenta a foto (`:181`, `:268`). | V decisória pela leitura após refresh; materialização, motivo e autor na projeção (`brief_c3.md:47`). | NENHUM | NENHUM |
| `services/validacao.py:300` chave `'validada_em'` | Outro, chave do dict `antes`; o valor vem de `foto0` (`:268`, `:300`; `_foto_da_validacao` em `:178–182`). | Só serializa a foto anterior para o evento, depois da comparação de fotos (`:294–301`). | V decisória pela leitura na guarda (`brief_c3.md:47`). | — | NENHUM |
| `services/validacao.py:302` chave `'validada_em'` | Outro, chave do dict `depois`, com `None` literal (`:297–304`). | Só foto posterior do evento (`:302–304`). | V decisória pela leitura na guarda (`brief_c3.md:47`). | — | NENHUM |
| `services/validacao.py:276` `'validada_em'` em `update_fields` | ORM, a instância que recebeu `None` em `:273`. | Seleciona o próprio campo a persistir no `save` (`:273–276`). | V decisória pela leitura na guarda (`brief_c3.md:47`). | — | NENHUM |
| `services/historico.py:100` `'respondida_em'` | Outro, `LeituraDoAto` de `DecisaoPergunta.leitura(p)`. A string vira `campo` no `getattr` (`:96–106`; `models.py:4351`). | R decide se o evento de resposta entra no histórico devolvido e fornece sua data (`:106–108`, `:114–117`). | RED-5 fora das 15 linhas: R/V/T da leitura e materialização da projeção (`brief_c3.md:49`). | — | `tests/test_e4c_f5_volta1_leem_o_ato.py:83` |
| `services/historico.py:102` `'validada_em'` | Outro, `LeituraDoAto` de `DecisaoPergunta.leitura(p)`. A string vira `campo` no `getattr` (`:96–106`; `models.py:4351`). | V decide se o evento de validação entra no histórico devolvido e fornece sua data (`:106–108`, `:114–117`). | RED-5 fora das 15 linhas: R/V/T da leitura e materialização da projeção (`brief_c3.md:49`). | — | `tests/test_e4c_f5_volta1_leem_o_ato.py:84` |

¹ Para os dois juízes, o aval **19/11 18:30, item 1c-1** diz literalmente: “ENTRE VOLTAS, chamador ainda nao convertido entrega a instancia ORM e o juiz le a COLUNA, como o main de hoje: estado TRANSITORIO declarado.” Na varredura de chamadas de produção, `RESTAM` está vazio e a única chamada reconhecida sem composta é a do `raiox`, que entrega `SimpleNamespace` de `values_list` (`tests/test_e4c_f5_selo_chamada.py:20–26`, `:124–158`, `:183–187`; `services/raiox.py:135–147`). Para esse caso, o aval **19/11 17:35, item 1(d)** diz literalmente: “allowlist nominal com motivo so para quem entrega tupla de values_list (raiox.py:147).”

## Universo e alcance de `SeloTest`

`SeloTest` abre exatamente duas funções de topo: `services/fio.py:178` (`espera_o_texto_do_colaborador`) e `services/historico.py:69` (`_eventos_dos_chamados`). Exige uma definição de cada e zero violações. Os únicos campos buscados são R e V. O teste auxiliar renomeia o parâmetro do fio e o alvo do histórico; o mutante passa a pergunta inteira a `respondida_sem_texto`. (`tests/test_e4c_f5_volta1_leem_o_ato.py:154–157`, `:210–251`; `tests/e4c_f5_volta2_mutantes.py:18–37`.)

- **Atributo.** Captura `apelido.validada_em` ou `apelido.respondida_em` se o receptor for um `ast.Name` rastreado, inclusive em atribuição ao campo. O rastreio parte do primeiro parâmetro do fio, dos alvos de iteração de coleções cuja raiz remonta a `PerguntaDisputa`, do primeiro argumento nominal de uma chamada `.leitura(...)` e de atribuições simples entre nomes. Passam receptores como `obj.pergunta.validada_em`, `perguntas[0].validada_em` e apelidos introduzidos por atribuição anotada ou desempacotamento. `.composta(...)` por si só não é ponto de partida do rastreio. (`tests/test_e4c_f5_volta1_leem_o_ato.py:159–191`.)
- **`getattr` com apelido.** Captura `apelido = p; getattr(apelido, 'validada_em')` se `p` for rastreado. A regra de string literal vale também para `ler = getattr; ler(apelido, 'validada_em')`, pois inspeciona os argumentos posicionais após o primeiro. Para `getattr(apelido, campo)`, busca constantes em qualquer lista ou tupla dentro da função, sem vincular `campo` à coleção. Passam nomes de campo construídos dinamicamente quando nenhuma constante procurada aparece nessas listas ou tuplas, bem como a chamada por apelido da função com `campo` variável quando o literal só existe fora dos argumentos. Também passa um receptor que o rastreio não reconhece. (`tests/test_e4c_f5_volta1_leem_o_ato.py:173–179`, `:192–207`.)
- **`update_fields=`.** `apelido.save(update_fields=['validada_em'])` passa. O receptor do método não conta como primeiro argumento posicional e o selo não percorre os keywords da chamada. A forma `PerguntaDisputa.save(apelido, update_fields=['validada_em'])` também passa, mesmo com `apelido` rastreado. (`tests/test_e4c_f5_volta1_leem_o_ato.py:192–207`; ocorrência em `services/validacao.py:276`.)
- **`refresh_from_db(fields=...)`.** `apelido.refresh_from_db(fields=['validada_em'])` passa pela mesma regra. Os dois `refresh_from_db` recenseados estão em `validacao.py`, fora dos dois corpos que o selo abre. (`tests/test_e4c_f5_volta1_leem_o_ato.py:192–218`; `services/validacao.py:262`, `:789`.)

## Contagens e sítios ausentes

| Contagem | Total |
|---|---:|
| Menções do `censo_crus.txt`, incluindo repetições e histórico | **34** |
| ORM que decide em produção, com aval literal | **0** |
| ORM que decide em produção, com NENHUM | **13** |

As 13 são `acoes_disputa.py:349`, `declaracao_texto.py:228`, `disputa_emissao.py:2085`, `materializacao.py:200`, `:202`, `:2128` e `validacao.py:262`, `:269`, `:640`, `:685`, `:898`, `:1065`, `:1086`, todos em `services/`.

- **Único dos 15 com zero menções:** `leitura_da_resposta`. O sítio do inventário tem `getattr(perg, 'resposta_colab', '')`; o censo procurou R e V. A definição aparece no anexo com zero ocorrências (`quinze.txt:15`; `brief_c3.md:46`; `templatetags/chamados_tags.py:145–178`, em especial `:166`).
- **Citados à parte no inventário:** `particao_do_app` e o leitor de `becos` constam da nota após os 15. Na cópia, ambos passam `DecisaoPergunta.composta(p)` ao juiz sem mencionar R ou V nesses corpos. O estado deles na base de comparação fica [nao verificado]. (`quinze.txt:19`; `services/pergunta_no_app.py:80–92`; `services/becos.py:25–35`.)

## 20/11 03:27 · sessao f1c8765cc2b0 · E4C-F5-CONSTROI · leitura · origem=Code

A cópia já carrega a marca `da_ressuscitacao`. O ponto em que a tarefa deixa de saber que houve ressuscitação é o `continue` em `_fatos_das_perguntas`. A decisão que sobra pode ser `SILENCIO`, que hoje produz `('silencio_decidido', [])`. A cópia está no diretório indicado, mas a confirmação independente do hash ficou [nao verificado]. A leitura foi estática, sem executar testes ou consultar banco e logs. `app/chamados/models.py:1714`, `app/chamados/services/tarefa_do_dia.py:182`, `app/chamados/services/tarefa_do_dia.py:393`

## 1. Da carga ao dono publicado [nao verificado]

1. **`tarefa_do_dia` → `carga_da_tarefa` → `CargaDaTarefa.tarefa`.** A entrada exige `agora`, abre uma carga de um dia se necessário e devolve o dicionário da tarefa. O construtor carrega perguntas do fato (`conversa=False`) por colaborador e `data_evento`, agrupa por `(colaborador_id, data_evento)` e traz a sequência integral de decisões, ordenada por `ato_em, pk`. **Sim, a distinção já chega à carga**. A ressuscitação grava uma `REABERTURA` compensadora com `da_ressuscitacao=True`, e o predicado existente é `DecisaoPergunta.es_da_ressuscitacao(d)`. O último ato **gravado** pode, portanto, ser a reabertura enquanto o último ato **lido pela tarefa** continua sendo o silêncio. `app/chamados/services/tarefa_do_dia.py:520`, `app/chamados/services/tarefa_do_dia.py:532`, `app/chamados/services/tarefa_do_dia.py:825`, `app/chamados/models.py:1693`, `app/chamados/models.py:1713`, `app/chamados/models.py:4246`

2. **`_fatos_das_perguntas` → fatos em lote.** Recebe a sequência integral, reconhece a marca e pula a linha antes de testar `ato_em <= agora`. Devolve `dec`, `leituras`, `retratada_depois`, `passivo`, `lavra`, `efeitos`, `fila` e `ancoras_terminais`. A marca fica no prefetch original, mas não sai nesse dicionário. A `VistaNoHorizonte` entrega à leitura do ato a sequência já filtrada. `LeituraDoAto` tem três campos e a `LeituraComposta` existente na cópia tem cinco. Nenhuma das duas informa último ato ou ressuscitação. A leitura composta do ORM, usada pelo C6, pode sentir o efeito da compensadora na resposta e na validação. Esse efeito não identifica a origem do ato para a tarefa. `app/chamados/services/tarefa_do_dia.py:374`, `app/chamados/services/tarefa_do_dia.py:393`, `app/chamados/services/tarefa_do_dia.py:405`, `app/chamados/services/tarefa_do_dia.py:457`, `app/chamados/models.py:19`, `app/chamados/models.py:24`, `app/chamados/models.py:4333`, `app/chamados/models.py:4354`, `app/chamados/juizes.py:1406`

3. **`_ler_vida` e `_tarefa_pura` → dia e instâncias.** `_ler_vida` preenche `viva_c6` nas candidatas à exposição administrativa e os IDs pendentes no app. No caminho ordinário, `_tarefa_pura` passa pelas guardas do dia e do marco, retira perguntas fora da identidade do fato e escolhe a última pergunta aplicável à posição. `_pela_decisao` devolve `(razão, ações)`. O passivo tem precedência e devolve `('passivo_sem_linha', None)`. A escolha do ato também pode recuar sobre respostas que preservam validação. Quando o ato escolhido é `SILENCIO`, a instância ordinária recebe o ID da pergunta, `silencio_decidido` e `[]`. No caminho de `expor_admin`, `[]` não preenche a instância com a pergunta. `app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:182`, `app/chamados/services/tarefa_do_dia.py:214`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:273`, `app/chamados/services/tarefa_do_dia.py:338`, `app/chamados/services/tarefa_do_dia.py:350`, `app/chamados/services/tarefa_do_dia.py:625`

4. **`_portas_da_tarefa` → dono do dia.** Junta ações das instâncias com as portas e os verbos do painel. `silencio_decidido` exclui a instância do conjunto de portas atribuídas à pergunta. A âncora ainda pode contribuir pela fila do chamado. A precedência é `colab`, `admin`, `chamado`, `sistema`. Se o silêncio for o único resultado e não houver porta residual nem oferta de Resolver dia, sai `fio='nada'`, `rota=None`, `fila=()` e `acao=None`. Com desconhecimento, sai `fio='nao_sei'`. `porta_da_tarefa` devolve o envelope público quando há rota ou desconhecimento. O silêncio sem ação da **instância** é compatível com outro dono no **dia**. `app/chamados/services/tarefa_do_dia.py:683`, `app/chamados/services/tarefa_do_dia.py:702`, `app/chamados/services/tarefa_do_dia.py:734`, `app/chamados/services/tarefa_do_dia.py:744`, `app/chamados/services/tarefa_do_dia.py:748`, `app/chamados/services/tarefa_do_dia.py:762`, `app/chamados/services/tarefa_do_dia.py:781`, `app/chamados/services/tarefa_do_dia.py:805`, `app/chamados/services/tarefa_do_dia.py:809`

## 2. Entregue e aberta

| Pergunta | Autoridade na cópia | Uso pela tarefa hoje |
|---|---|---|
| **A competência do dia foi entregue?** | `_K8` → `ponto.janelas.janela_atual(dia, empresa)` dá a janela que contém o fato. `_K6` → `folha.export.competencia_entregue(empresa, mes, ano)` verifica o TXT vigente e sem invalidação. Mês e ano vêm do fim da janela. `vigia_fio.competencia_do_dia_entregue` já compõe os dois e devolve `None` se faltar dia. `app/core/juizes.py:342`, `app/ponto/janelas.py:60`, `app/folha/export.py:556`, `app/folha/export.py:591`, `app/chamados/services/vigia_fio.py:789` | **Sim**, em `_ler_exportadas`, por lote. `_ler_vida` chama para os pares candidatos às extensões administrativas existentes. Um dia de trabalho com todas as perguntas encaixadas na grade pode pular essa seleção. O resultado guarda `expor_admin` e o caso `fato_em_ausencia`, sem governar o `SILENCIO` ordinário. `app/chamados/services/tarefa_do_dia.py:273`, `app/chamados/services/tarefa_do_dia.py:363`, `app/chamados/services/tarefa_do_dia.py:563`, `app/chamados/services/tarefa_do_dia.py:589`, `app/chamados/services/tarefa_do_dia.py:616` |
| **A competência do dia está aberta?** | O registro traz respostas distintas para **tranca**, **entrega**, **janela** e **permissão de escrita**. `_K4` é `competencia_trancada`, `_K7` é `periodo_trancado`, `_C20` é `dia_da_pergunta_trancado` e considera o toque posterior do admin. `_F6` é `competencia_fechada` por colaborador, que olha `FechamentoMensal`. `_K9` inclui holerite e tem o recorte da célula. Um registro de juiz cuja pergunta seja literalmente “a competência do dia do fato está aberta?” ficou [nao verificado]. `app/core/juizes.py:175`, `app/core/juizes.py:183`, `app/core/juizes.py:303`, `app/core/juizes.py:314`, `app/core/juizes.py:331`, `app/core/juizes.py:342`, `app/core/juizes.py:352`, `app/ponto/janelas.py:75`, `app/chamados/juizes.py:1009` | A tarefa consulta **C20** em lote por `dias_trancados`, que usa K8 e K7. Um dia trancado sem toque sai cedo com `competencia_trancada`. O `False` de C20 também pode significar que houve toque em competência ainda trancada. A lista C11 usa outro recorte, `competencias_a_lavrar`, com a corrente e a anterior enquanto a anterior não estiver entregue. O vigia também usa `aberta` como rótulo de medida para o complemento de K6, inclusive quando não tem data. Esses usos não definem por si o alcance de “aberta” na F5. `app/chamados/services/tarefa_do_dia.py:227`, `app/chamados/services/tarefa_do_dia.py:550`, `app/chamados/juizes.py:1067`, `app/chamados/juizes.py:1076`, `app/escala/services/cadastro_realidade.py:536`, `app/ponto/services/he_pendente_lavrado.py:209`, `app/chamados/services/vigia_fio.py:767` |

## 3. Ações e portas de ADMIN

| Ação conhecida | Porta, executor e tela | Serve para a silenciada ressuscitada? |
|---|---|---|
| **`VALIDAR`** (`'validar'`) | `_pela_decisao` emite para resposta com texto ou `DEVOLUCAO`. A composição aceita âncora na fila ou âncora não terminal. Gestor com permissão contextual age no modal do fio, pelo POST `validar_pergunta`, que chega a `acoes_disputa.acao_validar_pergunta` e `validacao.validar_pergunta`. Há também o cartão de pergunta e a validação inline da Central. A tarefa emite `VALIDAR` ainda para `celula_acusa_sem_pergunta_viva`, sem âncora própria de pergunta. `app/chamados/services/tarefa_do_dia.py:165`, `app/chamados/services/tarefa_do_dia.py:369`, `app/chamados/services/tarefa_do_dia.py:751`, `app/chamados/services/fio.py:607`, `app/chamados/services/fio.py:634`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/chamados/partials/_pergunta_card.html:103`, `app/templates/chamados/partials/_lista_chamados.html:82` | **Não como ação da ressuscitada sem resposta.** A ressuscitação limpa o par, a porta devolve `sem_resposta` e o formulário do modal fica no ramo de pergunta respondida. `app/chamados/models.py:1721`, `app/chamados/services/validacao.py:681`, `app/templates/chamados/partials/modal_fio.html:217` |
| **`DECIDIR_AUSENCIA`** (`'decidir_ausencia'`) | Sai de absorção pendente ou ausência a decidir no dia. O ID da ausência leva ao painel de ausência. DP com `editar_ausencias` usa Aprovar ou Rejeitar. A composição aceita essa ação direta sem chamado na fila. `app/chamados/services/tarefa_do_dia.py:184`, `app/chamados/services/tarefa_do_dia.py:315`, `app/chamados/services/tarefa_do_dia.py:751`, `app/folha/retrato_folha.py:1015`, `app/ponto/views.py:2745`, `app/ponto/views.py:2829`, `app/templates/ponto/partials/painel_ausencia.html:323`, `app/templates/ponto/partials/painel_ausencia.html:344` | **Só para uma ausência real, pelo caminho que já existe.** O ramo `SILENCIO` não fornece ausência para aprovar ou rejeitar. O link requer esse ID. `app/chamados/services/tarefa_do_dia.py:182`, `app/chamados/services/tarefa_do_dia.py:192`, `app/folha/retrato_folha.py:1044` |
| **`'resolver_dia'`** | A composição cria a ação se faltou rota e o calendário a ofereceu. Publica `porta_acao` e o dia do turno. Quem tem `regularizar_batida` usa Resolver dia no calendário, pelo POST `veredito_celula`. `app/chamados/services/tarefa_do_dia.py:702`, `app/chamados/services/tarefa_do_dia.py:781`, `app/chamados/services/tarefa_do_dia.py:819`, `app/templates/colaboradores/partials/_calendario_grade.html:131`, `app/ponto/views.py:3466` | **Condicional.** Existe ação sobre o dia quando a tela a oferece. A oferta depende do estado do dia e a ação executa um veredito do dia. A marca da ressuscitação não torna essa porta disponível por si. `app/colaboradores/services/calendario.py:97`, `app/chamados/services/tarefa_do_dia.py:702`, `app/ponto/views.py:3493` |
| **`'validar'` e `'decidir'` residuais** | São verbos do **chamado**, carregados do painel. `validar` já traduz para admin. `decidir` conserva a rota do chamado durante a seleção e pode sair como admin se o fio publicado for apenas residual. Ambas as portas abrem o modal. O gestor executa o ato que aquele chamado realmente oferece e para o qual tem permissão. `app/chamados/catalogo/natureza.py:41`, `app/chamados/catalogo/natureza.py:57`, `app/chamados/juizes.py:1602`, `app/chamados/juizes.py:1694`, `app/chamados/services/tarefa_do_dia.py:793`, `app/folha/retrato_folha.py:1015`, `app/chamados/utils.py:68` | `validar` tem a mesma limitação de resposta. `decidir` pode dar acesso quando já há chamado na fila e uma ação real no fio. O verbo não é um POST genérico, nem prova que haja meio de resolver aquela pergunta. A regularização do fio, por exemplo, recusa disputa com pergunta sem resposta. `app/chamados/services/tarefa_do_dia.py:739`, `app/chamados/services/tarefa_do_dia.py:762`, `app/chamados/services/validacao.py:681`, `app/chamados/services/acoes_disputa.py:121` |

**`Cobrar` é o botão reutilizável indicado pelo corte.** O modal já o mostra para disputa aberta com pergunta sem resposta. `cobrar_chamado` exige POST, `gerir_chamados` e permissão de área. Envia push e, se entregue, anexa a ocorrência existente, com limite de uma cobrança por colaborador e dia. O POST não resolve a pergunta nem muda o dono. Hoje o verbo residual `cobrar` é excluído do cálculo do dono, e não existe uma ação explícita `cobrar` de `ADMIN` na composição. Reusar o botão exige publicar o encaminhamento administrativo em leitura, sem confundi-lo com o verbo da fila. `app/docs/PROMPTS.md:2472`, `app/templates/chamados/partials/modal_fio.html:354`, `app/templates/chamados/partials/modal_fio.html:363`, `app/templates/chamados/partials/modal_fio.html:378`, `app/chamados/views_cobrar.py:24`, `app/chamados/views_cobrar.py:34`, `app/chamados/services/tarefa_do_dia.py:751`, `app/chamados/services/tarefa_do_dia.py:762`

## 4. As listas na árvore

- **Central de Chamados.** É a lista operacional de DP/gestor, acessível a staff e aos grupos de gestor. `painel_gestao` chama `montar_contexto_painel` e renderiza `painel_gestao.html`, `_lista_resumo.html` ou `_lista_chamados.html`. A lista traz chamados, agrupados por colaborador no resumo ou em linhas no detalhe. A fila e o balão do admin vêm dos status e dos verbos do painel. Uma instância `silencio_decidido` não vira linha própria. Seu chamado pode aparecer pelas regras atuais do painel. Publicar `fio='admin'` na tarefa, sozinho, não alimenta a Central. `app/chamados/views.py:24`, `app/chamados/views.py:35`, `app/chamados/views.py:721`, `app/chamados/views.py:767`, `app/chamados/services/painel.py:300`, `app/chamados/services/painel.py:335`, `app/chamados/services/verbos.py:20`, `app/templates/chamados/painel_gestao.html:239`
- **Cadastro x Realidade.** A árvore também chama essa tela de “lista do admin” no contexto do C11. `cadastro_x_realidade` chama `lista` para as empresas visíveis e serve HTML ou PDF a quem tem `ver_escala`. O C11 traz o beco específico de folga não definida, com pergunta viva e sem envelope de porta. Uma silenciada pode participar desse universo se cumprir os filtros, mas a razão `silencio_decidido` não é o critério de inclusão nem uma linha própria. O corte da ressuscitação não identifica essa tela pelo nome. `app/escala/views.py:514`, `app/escala/services/cadastro_realidade.py:405`, `app/escala/services/cadastro_realidade.py:459`, `app/escala/services/cadastro_realidade.py:536`, `app/escala/services/cadastro_realidade.py:860`, `app/docs/CORTES.md:545`
- **Lista textual da ressuscitação.** O corte a descreve **por PK e `colNNN`, junto das demais listas de competência paga, sem escrita**. Um gerador, template ou canal já ligado a essa lista específica ficou [nao verificado]. A **Pauta DP** que existe no sistema é outra implementação: `pautas.views.aba` serve registros de `Pauta` por destinatário, e a aba DP de `_copiloto.html` renderiza `p.texto`. O comando `pauta_folga_nao_definida_exportada` é um precedente de K6 mais saída textual em `--dry`, com filtro próprio de C11. Fora de `--dry`, grava pautas. Assim, uma instância `silencio_decidido` não é publicada automaticamente como texto para o DP por esses caminhos. `app/docs/CORTES.md:545`, `app/pautas/views.py:29`, `app/pautas/services.py:304`, `app/templates/core/_copiloto.html:65`, `app/templates/core/_copiloto.html:359`, `app/templates/core/_copiloto.html:405`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:92`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:119`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:130`

## 5. Quem lê a razão ou o resultado que ela produz

| Leitor | Efeito de uma ação de ADMIN na ressuscitada em aberta |
|---|---|
| **`_portas_da_tarefa`**, leitor literal de `perguntas_encerradas` | A tupla só exclui do ramo de âncora **pela pergunta**. Uma ação com porta inteira que esteja na fila já pode entrar em `portas_acoes`. Uma razão nova pode mudar `porta_fila` para `porta_pergunta` quando o acesso depende daquela âncora externa. Remover `silencio_decidido` da tupla inteira mudaria também o silêncio comum. `app/chamados/services/tarefa_do_dia.py:734`, `app/chamados/services/tarefa_do_dia.py:744`, `app/chamados/services/tarefa_do_dia.py:777` |
| **API do app, F6**, por `exigencia_das_perguntas` e `aguarda_admin_pela_tarefa` | Se a mudança estiver na face compartilhada, um item servido pela API passa a ter `aguarda_admin=True` ao receber ação `de='admin'`. Se entrar só na composição do dia, a API continua com a saída por pergunta anterior. A partição pendentes/histórico tem sua própria entrada de C6 e leitura da resposta, anterior a esse campo. `app/chamados/services/tarefa_do_dia.py:461`, `app/chamados/services/tarefa_do_dia.py:482`, `app/api/views.py:2553`, `app/api/views.py:2566`, `app/api/views.py:2584`, `app/chamados/services/pergunta_no_app.py:68`, `app/chamados/services/pergunta_no_app.py:80` |
| **Folha e retrato**, por `fora_da_folha` e `o_que_segura` | Entre os dias que já retêm, pode mudar fio, ação, link e agrupamento da pessoa. O universo de retidos é carregado antes da tarefa. Para uma ação publicada como `'cobrar'`, o mapa atual da Folha não tem entrada própria e pode cair no link legado de admin, “Validar no fio”. `app/folha/fora_da_folha.py:94`, `app/folha/fora_da_folha.py:200`, `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:275`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1015`, `app/folha/retrato_folha.py:1066` |
| **Vigia, furo, C11 e pauta de discordantes** | `vigia_fio` passa a contar a rota e o admin quando publicados. `furo_sem_canal` tira do conjunto sem canal quando chega rota. `beco_em_lote` retira do C11 quando chega envelope. `dias_sem_fio` deixa de selecionar o dia quando o fio deixa de ser `nada`. Todos dependem de o dia já estar no respectivo universo. O comparador `diff_e3_sombra` também vê o delta de porta. `app/chamados/services/vigia_fio.py:359`, `app/chamados/services/vigia_fio.py:594`, `app/chamados/services/furo_sem_canal.py:96`, `app/escala/services/cadastro_realidade.py:469`, `app/ponto/services/pautas_dos_discordantes.py:85`, `app/chamados/management/commands/diff_e3_sombra.py:41` |
| **`backfill_fio_morto_e4b.ato_do_dia`** | Lê razões e ações genericamente. Uma ação humana sem `porta` pode mudar o diagnóstico para `sem_porta_de_sistema`, quando não houver ato de sistema. Uma ação de ADMIN com porta inteira não entra na lista de atos de sistema por esse trecho. `app/chamados/management/commands/backfill_fio_morto_e4b.py:82`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:110` |
| **Testes, golden e mutante** | O teste F1/B1 de SILENCIO é o que contradiz diretamente a mudança pedida. E2, C14 e F6 U4 exercitam silêncio sem a compensadora. A matriz E3 Q12 e o teste de âncora externa exercitam o roteamento de encerradas. O mutante `encerrada_rota_pergunta` substitui a tupla inteira por `()`. O golden F2 de exportada tem silêncio vazio antes e depois. São controles para uma mudança limitada à ressuscitada em aberta. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328`, `app/chamados/tests/test_e2_tarefa_do_dia.py:285`, `app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:34`, `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:59`, `app/chamados/tests/test_e3_portas_tarefa.py:733`, `app/chamados/tests/test_e3_portas_v6_sem_banco.py:98`, `app/chamados/tests/e3_portas_v6_mutantes.py:67`, `app/chamados/tests/f2_ressuscitar_tarefa_main.json:39`, `app/chamados/tests/f2_ressuscitar_tarefa_main.json:114` |

## 6. Asserts existentes, no literal

- **F1/B1, igualdade antes/depois da instância.** `_fila` compara a razão e os trios `(acao, de, porta)` daquela pergunta. O assert é `self.assertEqual(self._fila(p), antes, 'a ressuscitacao mudou a tarefa_do_dia da pergunta')`. Os dois casos chamam `self._ressuscita_e_compara(False, 'aguarda_resposta')` e `self._ressuscita_e_compara(True, 'silencio_decidido')`. O helper afirma a razão inicial por `self.assertEqual(antes[0], razao_esperada, 'fixture fora da classe medida em prod')`. O teste de SILENCIO verifica a igualdade, sem um `[]` literal próprio. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:333`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:338`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:

[truncado em 20000 de 29406 caracteres]

## 20/11 03:16 · sessao 27fa3c0f94d6 · E4C-F5-CONSTROI · censo · origem=Code

| Arquivo e classe principal | O que mudou no hash | Aval literal | Família de harness e efeito sobre “sem tocar” | Asserts e expectativas, antes → depois |
|---|---|---|---|---|
| `app/chamados/tests/e3_mutantes.py:30` · Outro | A âncora `inclui_respondidas` passou a incluir `DecisaoPergunta.composta(p)`. O substituto continua `True`. | **RESPOSTA-F5-DEVOLVIDO-1945-2005**, item 4(ii), “só a âncora e sem assert: e3_mutantes.py:30”. | Catálogo de mutantes do app. Família nominal [nao verificado]. Exceção expressa para a âncora. | Nenhum assert alterado no hunk. |
| `app/chamados/tests/e3_r3_c6_mutantes.py:48–51` · Outro | Dois mutantes de M1 passaram de `projecao_temporal_da_pergunta(p, agora)[1] is not None` a `respondida_no_horizonte(p)`. O segundo também atualiza o predicado preservado no substituto. O alvo está em `app/escala/services/cadastro_realidade.py:508`. | **F5-CLASSE-NAO-CASO-1911-2050**, item 2(i), “ancora de linha ou de texto deslocada pelos commits da F5”. | Mutantes K6/C11. Família nominal [nao verificado]. Exceção de âncora do 2050. | Nenhum assert alterado no hunk. Morte dos mutantes [nao verificado]. |
| `app/chamados/tests/e3_v8_apoio.py:12,26,77,167,197` · Dublê/fixture | Adota `pergunta` e o `DecisaoPergunta` do helper. Retira a implementação local de `prefetch_related`, que passa a ser herdada de `app/chamados/tests/e3_volta7_apoio.py:33`. | **RESPOSTA-F5-DEVOLVIDO-1945-2005**, item 1, “e3_v8_apoio.py entra na excecao nominal”; **F5-CLASSE-V-1911-2230**, item 2, “como identidade ou no-op”. | Apoio de `test_e3_portas_tarefa`, conferido em `app/chamados/tests/test_e3_portas_tarefa.py:632`. Exceção nominal do 2005. | Nenhum assert alterado no diff. |
| `app/chamados/tests/e3_volta7_apoio.py:33,77–79,110–111` · Dublê/fixture | `Query` ganha `prefetch_related` identidade. O módulo falso de modelos recebe `DecisaoPergunta`. O módulo falso de juízes passa a expor o corpo real recompilado de `pergunta_respondida`. | **2050**, item 2(ii), “duble que ganha o que o compositor le, pelo helper unico”; **2230**, item 2, “como identidade ou no-op”. **SEM-AVAL parcial** para incluir a função de juiz na linha 79. | Volta 7/K6, recompilação de juiz nas linhas 77–79. Família nominal [nao verificado]. Exceções para modelo e queryset. | Nenhum assert alterado no diff. |
| `app/chamados/tests/e4b_c12_ast.py:19–27` · Outro | Desloca nove identidades em `CORPO`. | **2005**, item 4(ii), “linha do c12 (462 -> 469)”; **2050**, item 1, “c12 autorizadas as 8 identidades”. | Catraca AST C12 de gates. Família nominal [nao verificado]. Exceções para as nove âncoras. | Âncoras `205→211`, `462→469`, `302→308`, `667→674`, `847→854`, `865→872`, `1501→1508`, `1756→1775`, `1776→1795`. O filtro usa `CORPO` em `app/chamados/tests/e4b_c12_ast.py:223`. O assert de cardinalidade continua em 9 em `app/chamados/tests/test_e4b_c12_selo.py:36`. |
| `app/chamados/tests/test_bug_par_c2_sem_banco.py:13–16,32–34,80–83` · Dublê/fixture | `_Recorte` herda `Linhas`, inclusive o `prefetch_related` identidade exigido pela consulta de `app/chamados/services/materializacao.py:327–328`. A pergunta e o modelo falso passam ao helper. | **2050**, item 2(ii), “pelo helper unico”; **2230**, item 2, “duble de ORM [...] ganha o metodo que o codigo real da F5 passou a chamar”. | BUG-PAR C2, materialização em memória. Família nominal [nao verificado]. Exceções de dublê e queryset. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_bug_par_sem_banco.py:170–171,191–193` · Dublê/fixture | Troca `NS` pelo helper da pergunta e injeta o modelo falso. O juiz real recompilado já era fornecido. | **2050**, item 2(ii), “duble que ganha o que o compositor le, pelo helper unico”. | BUG-PAR sem banco, juiz de resposta recompilado na linha 192. Família nominal [nao verificado]. Exceção para o dublê. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_cef_c1_sem_banco.py:88–89,309–312` · Dublê/fixture + âncora | `Consulta` ganha `prefetch_related` identidade. `upload_arranca_motivo` inclui o prefetch na âncora e conserva `if True:` como substituto. O alvo está em `app/api/foto_recebida.py:81`. | **2230**, item 2, “como identidade ou no-op”; **2050**, item 2(i), “ancora de linha ou de texto deslocada”. | CEF C1/upload sem banco. Família nominal [nao verificado]. Exceções para queryset e âncora. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_contract_dedup_status.py:139–144` · Expectativa de assert | Troca busca de substring no fonte por busca de uma atribuição de primeiro nível em AST. A chamada com universo carregado está em `app/chamados/models.py:2071–2072`. | **2050**, item 2(i), “ancora [...] de texto deslocada”, cobre a atualização da chamada esperada. **SEM-AVAL parcial** para a troca da técnica de verificação por AST. | Selo estrutural de `fechar`. Família nominal [nao verificado]. | `'self.respostas_sem_veredito()' in src` → atribuição `_pend = self.respostas_sem_veredito(perguntas=perguntas)` entre os `ast.Assign` do primeiro nível. A nova checagem exige a atribuição e o argumento. |
| `app/chamados/tests/test_cortes_f2_f9_m4.py:51–65` · Expectativa de assert | Troca o estado esperado na fixture com resposta só no ato e projeção vazia. | **2005**, item 4(i), “M4 muda o esperado de respondida para pendente [...] so neste teste”. | M4/queda final. Família nominal [nao verificado]. Exceção expressa para essa expectativa. | `RESPONDIDA → PENDENTE` na linha 58. Esse caso passa a aceitar `m4_pendente`; o controle alinhado ainda exige `RESPONDIDA` na linha 64. |
| `app/chamados/tests/test_disputa_s127.py:15,22–23` · Dublê/fixture | `.all()` passa de `list` a `Linhas`, com `prefetch_related` identidade, chamado em `app/chamados/juizes.py:2490`. | **2230**, item 2, “duble de ORM [...] ganha o metodo que o codigo real da F5 passou a chamar”. | S127 em memória, com `pergunta_viva` mockada em `app/chamados/tests/test_disputa_s127.py:39–42`. Família nominal [nao verificado]. Exceção para o queryset. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e1_v10.py:23,119–122` · Dublê/fixture | `_ln` passa de `SimpleNamespace` para `pergunta`, preservando os valores fornecidos ao plano de retratação. | **2050**, item 2(ii), “duble que ganha o que o compositor le, pelo helper unico”. | Plano de retratação em memória. Família nominal [nao verificado]. Exceção para o dublê. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e3_c8b_sem_banco.py:14–15,28–34,210–445` · Dublê/fixture | Encapsula `ambiente_c6` para instalar o modelo falso com `DecisaoPergunta` do helper e redireciona os usos ao mesmo ambiente. | **2050**, item 2(ii), “duble que ganha o que o compositor le, pelo helper unico”. | Alimentação C8b/C6. O apoio recompila os juízes em `app/chamados/tests/e3_c8b_apoio.py:110–117`. Família nominal [nao verificado]. Exceção para o modelo falso. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e3_degrau_dono_e_porta.py:264–276,295–312,338–350,510–512` · Dublê/fixture | Usa a pergunta do helper, injeta o modelo falso e fornece `pergunta_respondida` real em dois ambientes que antes omitiam esse juiz. | **2050**, item 2(ii), “pelo helper unico”. **SEM-AVAL parcial** para as novas injeções do juiz nas linhas 274 e 512. | Consulta/carga e leitores da porta do app, com juiz recompilado na linha 512. Família nominal [nao verificado]. Exceção para modelo e pergunta. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e3_pos_suite_sem_banco.py:13–17,54–55` · Dublê/fixture | A pergunta passa ao helper. O import de `ambiente_c6` passa a usar o ambiente C8b com o modelo falso. | **2050**, item 2(ii), “duble que ganha o que o compositor le, pelo helper unico”. | Pós-suíte C6, reutiliza C8b em `app/chamados/tests/test_e3_pos_suite_sem_banco.py:62,84`. Família nominal [nao verificado]. Exceção para o dublê. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e3_r3_c5_sem_banco.py:161–162,173–181` · Dublê/fixture | O ambiente C11 recebe o modelo do helper e recompila `pergunta_respondida` para o módulo falso de juízes. | **2050**, item 2(ii), “pelo helper unico”, cobre o modelo. **SEM-AVAL parcial** para a recompilação adicional na linha 176. | C11/M1 sem banco. Família nominal [nao verificado]. Exceção para o modelo falso. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e3_r3_c6_sem_banco.py:290–300` · Dublê/fixture | O ambiente K6/C11 recebe o modelo do helper e recompila `pergunta_respondida` para o módulo falso de juízes. | **2050**, item 2(ii), “pelo helper unico”, cobre o modelo. **SEM-AVAL parcial** para a recompilação adicional na linha 296. | K6/C11 sem banco. Família nominal [nao verificado]. Exceção para o modelo falso. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e3_r3_c7_sem_banco.py:181–192` · Dublê/fixture | As perguntas ganham `decisoes` por `pergunta(tipo=N, ...)`. O queryset recebe `prefetch_related` identidade. | **2050**, item 2(ii), “pelo helper unico”; **2230**, item 2, “como identidade ou no-op”. | Carga K1/status das âncoras. Família nominal [nao verificado]. Exceções para pergunta e queryset. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:85–87,112–116,184,202–204` · Dublê/fixture | Usa `DecisaoPergunta` do helper no lugar do dublê de literais, instala o mesmo modelo no cenário de limites e monta as perguntas pelo helper. | **2050**, item 2(ii), “duble que ganha o que o compositor le, pelo helper unico”. | Tarefa admin/C6, com recompilação em `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:163–167`. Família nominal [nao verificado]. Exceção para os dublês. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e4a2_c5_memoria.py:118–136` · Dublê/fixture | O ambiente do vigia ganha `contar_texto_resposta=Mock(return_value=(0, 0))`. A cura foi registrada em `app/docs/RELATO.md:138`. | **SEM-AVAL**. **1735**, item 8, pede “RED de consultas por lote” para o vigia. Esse mock fixa o retorno do contador. | Vigia C5 em memória, recompila o `handle` na linha 136. Família nominal [nao verificado]. Exceção para este mock [nao verificado]. | Nenhum assert alterado no diff. O par `(0, 0)` é retorno do mock. |
| `app/chamados/tests/test_e4b_c4v7_memoria.py:355–360,378–382` · Dublê/fixture | A foto troca `modelo()` pelo `DecisaoPergunta` do helper e repõe `.objects` via patch. O sítio da foto usa `DecisaoPergunta.objects.filter` em `app/chamados/services/foto_e4b.py:280`. | **SEM-AVAL**. **2050**, item 2(ii), exige dublê que “ganha o que o compositor le”. Nesta edição, o manager já existia no dublê anterior e é usado pela foto. | E4b C4v7 reutiliza `AlcanceTest` nas linhas 86–105. O trecho editado é o cenário da foto. Exceção para esse trecho [nao verificado]. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_e4b_classe_e_memoria.py:39–40,51,193–194,286–289,336` · Dublê/fixture + âncoras | Atualiza as âncoras S128 e HX, dá `prefetch_related` identidade a `Linhas` e migra pergunta/modelo falso ao helper. Alvos em `app/chamados/services/disputa_emissao.py:1854,1955`. | **2050**, item 1(A), “ancora ao texto vivo”, e item 2(i–ii), “ancora [...] deslocada” e “pelo helper unico”; **2230**, item 1, “Linhas [...] ganha prefetch_related como identidade”. | E4b/Alcance, recompila os juízes/C6 em `app/chamados/tests/test_e4b_classe_e_memoria.py:313–315`. Exceções para âncoras e dublês. | Nenhum assert alterado no diff. Morte dos mutantes [nao verificado]. |
| `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:500–544,578–593` · Teste novo + expectativas e consultas | Tabela por leitor, prova explícita dos instantes, novo RED de reabertura com coluna preenchida e selo de `_ancoradas` com 1 e 5 perguntas. | **RESPOSTAS-F5-CONSTROI-1911-1735**, item 3(a), “RED com divergencia ato x projecao”, e item 7(iv), “passa ao contrato do 1510 item 2”; **RESPOSTAS-F5-CONSTROI-2-1911-1830**, itens 7-iv, “AUTORIZADA a tabela”, e 7-vii, “1 da carga + 1 de decisoes por lote”. | Selos F2 de leitores e ancoradas. `predesenho.md` §4 os situa na volta A. Família nominal [nao verificado]. Exceções expressas para os selos. | Linha 520: `True → False` para `sem_veredito_entre`, CSF, irmãs, `estado == RESPONDIDA` e property `respondida`. `resposta_display` e `q_vivas` continuam `True`. Linha 584: `1 → (2, 2)`. Linha 585: `[viva.pk] → ([viva.pk], [viva.pk] + pks das 4 novas)`. A forma da subquery passa de uma captura a ambas e a segunda consulta de cada captura deve ler decisões, linhas 589–593. |
| `app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:113,125–132` · Outro | Atualiza a âncora do mutante da resposta da API, `app/api/views.py:2601`. Continua substituindo o trecho por `'perguntas': []`. | **2050**, item 2(i), “ancora de linha ou de texto deslocada”. | Costura do app sem Django. O bloco recompila `_costura_do_app` nas linhas 115–119. Família nominal [nao verificado]. Exceção para a âncora. | `fonte.count(ancora) == 1` conserva o valor esperado, agora sobre a chamada com `composta`. |
| `app/chamados/tests/test_estado_aplicacao.py:5,38–45` · Dublê/fixture | `_p` troca `NS` pelo helper, preservando os argumentos explícitos. | **2050**, item 2(ii), “duble que ganha o que o compositor le, pelo helper unico”. | Estado de aplicação. Família nominal [nao verificado]. Exceção para o dublê. | Nenhum assert alterado no diff. |
| `app/chamados/tests/test_f1nova_b2_leitores.py:93–97` · Expectativa de assert | O mock do juiz passa a esperar a leitura composta e ganha verificação de tipo exato. | **1830**, item 7-vi, “passa a esperar o juiz chamado com o tipo proprio do compositor”. | Selo da property F1/B2, listado em `predesenho.md` §4. Família nominal [nao verificado]. Exceção expressa para o selo. | `assert_called_once_with(DecisaoPergunta.leitura(p)) → assert_called_once_with(DecisaoPergunta.composta(p))`, mais `type(arg) is LeituraComposta`. |
| `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–207` · Teto de consultas | O selo existente passa de 1×3 a 3×6 candidatas e sobe o teto unitário. O RED separado para SELECTs de decisões está em `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:187–202`. | **1735**, itens 6(a) e 7(v), “Q(3)/3 == Q(6)/6” e “teto 8 -> 9 por candidata”. **1830**, item 7-viii, “Os REDs 3 x 6 sao casos NOVOS”, atendido pelo caso separado. | Selo ORM de custo O261, classe R no aval. Família nominal [nao verificado]. Exceção expressa para a métrica. | `Q(3)=3×Q(1) → 6×Q(3)=3×Q(6)`. `Q(1)≤8 → Q(3)≤27`, ou 9 por candidata. Sai a verificação direta de uma candidata. |
| `app/chamados/tests/test_o261_vigia_fio.py:816,831,840–849,872–875` · Expectativa de assert | O carimbo passa a checar ausência do campo de universo e a exigir o contador de divergências tanto no censo quanto no dicionário de esperados. | **SEM-AVAL** para editar este teste existente. **RESPOSTAS-F5-E-C7-1911-1510**, item 4, “remover divergencia_texto_resposta_universo”, autoriza a remoção no vigia. **1735**, item 8, pede o RED de consultas por lote. | O261/vigia e carimbo ORM. Família nominal [nao verificado]. Exceção para este arquivo [nao verificado]. | Censo sem a chave → chave com `contar_texto_resposta(...)[0]`. `esperado` sem a chave → `divergencia_texto_resposta: 0`. Acrescenta `assertNotIn('divergencia_texto_resposta_universo', reg)`. |
| `app/chamados/tests/test_pergunta_viva.py:5,9–12` · Dublê/fixture | `_p` passa a montar o dublê pelo helper, que fornece os defaults de leitura. | **RESPOSTA-F5-DUBLE-1911-1945**, MUDA (3), “test_pergunta_viva monta o duble pelo helper”. | `PerguntaVivaOrdemTest`, `app/chamados/tests/test_pergunta_viva.py:15`. Exceção nominal do 1945. Posição entre as 11 [nao verificado]. | Nenhum assert alterado no diff. |
| `app/core/tests/test_fio_mudo_placar.py:125–129` · Dublê/fixture | A lista de patches do vigia ganha `contar_texto_resposta: (0, 0)`. A cura foi registrada em `app/docs/RELATO.md:87`. | **SEM-AVAL**. **1735**, item 8, “RED de consultas por lote”, tem objeto distinto do stub do contador. | Placar/vigia. Família nominal [nao verificado]. Exceção para este stub [nao verificado]. | Nenhum assert alterado no diff. O par `(0, 0)` é retorno do stub. |
| `app/core/tests/test_medida_turno_do_par.py:258–261,271,319` · Outro | Desloca oito endereços de seis entradas B1 em `LINHAS_REGISTRADAS`. | **2050**, item 2(i), “ancora de linha ou de texto deslocada pelos commits da F5”. | Catraca de endereços do turno do par. Família nominal [nao verificado]. Exceção para âncoras. | No `assertEqual` de `app/core/tests/test_medida_turno_do_par.py:559`, valores indiretos `54→53`, `170→169`, `275→274`, `204→203`, `[434,455]→[437,458]`, `[1234,1387]→[1233,1386]`. |
| `app/ponto/tests/test_selo_chokepoint_escrita.py:206,230,253,267,319–328` · Teto de consultas | Sobe quatro entradas de `TETOS` e registra a atribuição dos deltas por sítio. | **F5-CHOKEPOINT-TETO-MEDIDO-2011-0110**, item 2, “o teto passa ao MEDIDO, com cada +1 anotado no proprio selo”. O aval **F5-CHOKEPOINT-PASSAGEM-1911-2325**, item 3, dava passagem com “Teto NAO sobe por este aval”. | Selo ORM da batida. Família nominal [nao verificado]. Exceção expressa para as leituras atribuídas. | No mesmo `assertLessEqual`, `230→234`, `294→298`, `145→146`, `219→221`. A igualdade de `on_commit` da linha 321 usa os mesmos valores das linhas 283–286. |
| `app/ponto/tests/test_supra_juiz_executor.py:15,22–28` · Dublê/fixture | `_P` passa de classe local a fábrica via helper, preservando PK, decisões vazias, carimbos e texto fornecidos ao CSF. | **2050**, item 2(ii), “duble que ganha o que o compositor le, pelo helper unico”. | CSF/supra-juiz em memória. Família nominal [nao verificado]. Exceção para o dublê. | Nenhum assert alterado no diff. |

### (a) Lista nominal SEM-AVAL

A lista inclui arquivos com ao menos uma mudança fora dos literais aplicáveis. No anexo `avais_f5_em_ordem.txt`, o **1735**, item 7, diz “e so estas”, e o **2050**, itens 2–3, amplia a lista por classe e manda parar para “caso fora das classes i-iv”. O **2230**, item 2, acrescenta o dublê de ORM.

- **Parcial** — `app/chamados/tests/e3_volta7_apoio.py:79`, inclusão do juiz recompilado.
- **Parcial** — `app/chamados/tests/test_contract_dedup_status.py:139–144`, troca da checagem textual por comparação de atribuições AST.
- **Parcial** — `app/chamados/tests/test_e3_degrau_dono_e_porta.py:274,512`, novas injeções do juiz.
- **Parcial** — `app/chamados/tests/test_e3_r3_c5_sem_banco.py:176`, inclusão do juiz recompilado.
- **Parcial** — `app/chamados/tests/test_e3_r3_c6_sem_banco.py:296`, inclusão do juiz recompilado.
- **Integral** — `app/chamados/tests/test_e4a2_c5_memoria.py:131`, mock do contador.
- **Integral** — `app/chamados/tests/test_e4b_c4v7_memoria.py:359,381`, troca do modelo falso no cenário da foto e patch do manager.
- **Integral** — `app/chamados/tests/test_o261_vigia_fio.py:831,840–849,872–875`, adaptação do contrato do carimbo existente.
- **Integral** — `app/core/tests/test_fio_mudo_placar.py:127`, stub do contador.

### (b) Asserts enfraquecidos ou removidos

- **M4, perda de poder contra o mutante no caso só-no-ato.** `RESPONDIDA → PENDENTE` faz a asserção aceitar a mutação para `PENDENTE`. A troca de valor tem aval nominal no **2005**, item 4(i). O controle alinhado ainda rejeita esse resultado pelo código. Execução do mutante [nao verificado]. `app/chamados/tests/test_cortes_f2_f9_m4.py:29–31,51–65`.
- **O261, retirada da cobertura direta de uma candidata e aumento do teto.** Saem `Q(3)=3×Q(1)` e `Q(1)≤8`. Entram a igualdade por candidata entre 3 e 6 e o teto de 9 por candidata medido no grupo de 3. Mudança nominal do **1735**, itens 6(a) e 7(v). `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–207`.
- **Chokepoint, quatro limites superiores afrouxados.** O mesmo assert passa a aceitar os intervalos adicionais `231–234`, `295–298`, `146` e `220–221`, respectivamente. Mudança condicionada à atribuição de leituras no **0110**, item 2. `app/ponto/tests/test_sel

[truncado em 20000 de 24239 caracteres]

## 20/11 03:16 · sessao 94395104a83e · E4C-F5-CONSTROI · leitura · origem=Code

### 01 — Matriz dos 15 sítios e censo de entrada

**Segue devido.** O 0815, etapas A e B, exige “MATRIZ POR SITIO, 15 linhas”, “contagem unica de sitios, igual no §2, no selo e no PRONTO” e “CENSO APROVADO do wrapper”. O 1510 item 1 altera a fonte do texto, que fica na projeção. A exigência da matriz permanece. `avais_f5_em_ordem.txt`, 0815 A/B e 1510.1, `relato_f5.txt:362,486`.

**Prova parcial.** O RELATO declara 15 sítios, zero perguntas de lei e fechamento do censo, com 9 de 9 endereços amostrados em `3308f7599`. A abertura descartada às 08:37 foi `CENSO FORA`. A matriz colada é a de 15/11, declarada superada, e a matriz de 96 chamadores declarada no RELATO é outro censo, de `e673e0efe`. O wrapper chama `--censo`, mas a saída de aprovação e as 15 linhas do `brief_c3.md` estão [nao verificado]. `relato_f5.txt:321,486,543,559`, `matriz.md` §1, `bin/codex_pergunta.sh:241–247`.

**PUBLICAR.** Recuperar as 15 linhas do `brief_c3.md` e a saída identificada do wrapper, então cruzar cada sítio com o endereço atual, campo, fonte, porta, laço, RED e mutante. Os dados publicáveis agora são **15 sítios, 0 perguntas de lei e 9/9 endereços amostrados em `3308f7599`**, como declaração de abertura. O selo atual demonstra dois corpos, conforme o item 06. `relato_f5.txt:486`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:210–218`.

### 05 — SILÊNCIO e dono admin

**Segue devido quanto ao comportamento.** O 0815 B manda “admin vira dono em competencia aberta; reescreve a expectativa do teste da F1”. O 1735.7 diz “ADAPTACOES DE TESTE AUTORIZADAS, e so estas” e “Qualquer outra adaptacao de teste = pergunta no topo”. O 1735.8 mantém o PRONTO do 0815 cumulativo. A lista do 1830 autoriza outro teste da F1, `test_f1nova_b2_leitores.py`, e não cita o caso de ressuscitação do B1. `avais_f5_em_ordem.txt`, 0815 B, 1735.7–8 e 1830.7-vi, `relato_f5.txt:1096,1113`. [nao verificado]

**Sem prova na cópia.** `SILENCIO` produz `('silencio_decidido', [])` e entra entre as razões encerradas na rota da tarefa. O teste ainda exige a mesma fila antes e depois da ressuscitação. A sonda antiga dos 213 confirma justamente o resultado vazio, sem identificar a árvore-alvo. `app/chamados/services/tarefa_do_dia.py:182–183,744–756`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320–344`, `relato_f5.txt:725`.

**PERGUNTA-AO-DONO.** A reescrita do B1, exigida nominalmente pelo 0815, é ressalva à lista fechada do 1735/1830? O fechamento técnico precisa fazer a tarefa real oferecer dono e porta de admin para o caso **ressuscitado em aberta**, e o B1 precisa cobrar esse resultado. A expectativa antiga e o código atual deixam o caso sem ação. `app/docs/PROMPTS.md:2472,2475`, `app/chamados/services/tarefa_do_dia.py:182–183,724–756,830–840`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320–344`.

### 06 — Catraca de leitores crus

**Segue devido.** O 0815 B exige “catraca dos leitores crus em 0 nos sitios migrados, varrendo tambem string em getattr/update_fields/refresh_from_db”. O 1435.3d acrescenta “independente do nome da variavel; mutante com apelido tem de morder”. O 1510 mantém o texto na projeção. `avais_f5_em_ordem.txt`, 0815 B, 1435.3d e 1510.1, `relato_f5.txt:362,384,543`.

**Prova parcial na cópia.** O selo percorre `espera_o_texto_do_colaborador` e `_eventos_dos_chamados`. Resolve apelidos, atributos e certas strings em argumentos posicionais. A varredura de `update_fields=` e `refresh_from_db(fields=...)` não está no corpo do selo. O censo antigo de 56 strings é da base `5bad81419`. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:154–251`, `relato_f5.txt:543`.

**CONSTRUIR-TESTE.** Estender esse selo aos sítios migrados do `brief_c3`, com uma asserção de cobertura do inventário e mutantes que reintroduzam `respondida_em` ou `validada_em` via atributo, `getattr` com apelido, `update_fields=` e `refresh_from_db(fields=...)`. A lista nominal do `brief_c3` está [nao verificado]. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:154–218`, `relato_f5.txt:486,543`.

### 12 — Sombra das decisões por porta

**Segue devido.** O 0815 B pede “para cada porta da volta 3, quantas perguntas mudam de decisao (valida, recusa, materializa)”. O 1435.1 conserva as “medidas (a)(b)(c) na sombra pela funcao real”. `avais_f5_em_ordem.txt`, 0815 B e 1435.1, `relato_f5.txt:373`.

**Sem medida da cópia.** Há REDs de validação, recusa e anuência. As sondas publicadas medem a prévia, a property e outros leitores. A tabela de transições das portas está [nao verificado]. `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:98–139`, `app/chamados/tests/test_e4c_f5_voltaC_sitios.py:125–133`, `relato_f5.txt:3–5,299`.

**MEDIR-NA-SOMBRA.** No mesmo retrato, relógio, ator e argumentos nas duas árvores, chamar `validar_pergunta`, `recusar_pergunta` e `materializar_perguntas_validadas_da_disputa`. Cobrir todos os PKs do universo de entrada de cada porta, inclusive os rejeitados antes da escrita. Para materialização, chamar por disputa e atribuir a decisão a cada pergunta, inclusive as excluídas da seleção inicial. Isolar cada chamada com rollback e capturar notificações. Publicar, por porta, universo, elegibilidade, transições de retorno e de efeito gravado, erros e PKs divergentes. A lista integral das portas do `brief_c3` está [nao verificado]. `app/chamados/services/validacao.py:661–712,858–955`, `app/chamados/services/materializacao.py:2074–2141`.

### 13 — Entregues e exportadas

**Segue devido.** O 0815 B determina “as que caem em competencia entregue ou exportada NAO se movem por este caminho: lista para ele”. O 1435.1 preserva a medida. `avais_f5_em_ordem.txt`, 0815 B e 1435.1, `relato_f5.txt:373`.

**Sem prova de zero.** A guarda K6 citada consulta o dia do fato e, nesse caminho, cobre as metades de intervalo. A recusa humana tem caminho até `save` e reabertura sem guarda nominal de competência nesse trecho. A prévia sem diferença em `a3c23d10b` é uma medida de leitura. `app/chamados/services/materializacao.py:791–830,1871–1874`, `app/chamados/services/validacao.py:740–742,894–927`, `relato_f5.txt:4`.

**MEDIR-NA-SOMBRA.** Separar o universo do item 12 pela competência do fato e pelo juiz real `competencia_entregue`, registrando a evidência do TXT vigente. Em cada porta, comparar o retorno antes/depois e o estado persistido antes/depois de cada chamada, inclusive perguntas irmãs, atos, batidas e ausências. Publicar denominador e lista de PKs que se moverem, ou zero explícito por porta. `app/chamados/services/materializacao.py:791–804,2074–2141`, `app/folha/export.py:556–587`, `app/chamados/services/validacao.py:661,858`.

### 14 — CHECK-DE-TELA

**Segue devido.** O 0815 B/PRONTO exige “CHECK-DE-TELA na arvore (espelho, central, tarefa do dia pelo servico, api do app) contra a referencia_1911.md”. O smoke do 1735.8 trata de outra etapa. `avais_f5_em_ordem.txt`, 0815 B/PRONTO, 1435.1 e 1735.8, `relato_f5.txt:516,521`.

**Sem comparativo da F5.** A referência é do main `3f666ad84`. As sete repetições registradas são de F6F7 e ESPURIA-TELA. A tarefa ficou sem medida, e a API da tabela é `api_me`. `relato_f5.txt:516,521`, `app/api/urls.py:64,92`.

**MEDIR-NA-SOMBRA.** Repetir as entradas da referência com mesmos perfis, filtros, datas e universo. Chamar `espelho_ponto`, `painel_gestao`, `carga_da_tarefa(...).tarefa(...)` e as GETs reais da API. Parear `api_me` com a referência e obter a referência da lista `api_disputa_perguntas` e da tarefa pelo serviço. Publicar, para casos unitários e em lista, conteúdo relevante, consultas totais e a decisões, tempo e escritas. `app/ponto/urls.py:10`, `app/ponto/views.py:97`, `app/chamados/views.py:721`, `app/chamados/services/tarefa_do_dia.py:825–840`, `app/api/views.py:284,2536–2553`, `relato_f5.txt:516`.

### 19 — Grep publicado do compositor [nao verificado]

**Segue devido.** O 1710 PRONTO 2 pede “um compositor, e nenhuma composicao local sobrando em fio.py e models.py:3791 (grep no RELATO)”. O 1735.1c–2 fixa o tipo próprio e o compositor junto de `DecisaoPergunta.leitura`. `avais_f5_em_ordem.txt`, 1710 PRONTO 2 e 1735.1c–2, `relato_f5.txt:303`.

**Prova estática na cópia.** **1** definição do compositor, **3** delegações nos sítios nomeados, **0** composições locais nos corpos do fio e da property. O grep publicado está [nao verificado]. `app/chamados/models.py:3810–3816,4353–4362`, `app/chamados/services/fio.py:178–190`, `app/chamados/services/pergunta_no_app.py:80–89`, `relato_f5.txt:5,303`.

**PUBLICAR.** Linha do RELATO com a definição `models.py:4354`, as três chamadas `models.py:3816`, `services/fio.py:189`, `services/pergunta_no_app.py:88`, e zero composição local nesses dois corpos. Os testes de fonte e de composição local estão em `app/chamados/tests/test_e4c_f5_opus_models.py:58–90` e `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:45–71`.

### 32 — Triagem de `casefold` anexa ao Opus

**Segue devido.** O 1435.3e pede “triagem com motivo em uma linha, anexa ao opus”. O replay de 1450 proíbe “usar replay como verificacao da F5”. `avais_f5_em_ordem.txt`, 1435.3e e 1450, `relato_f5.txt:357,360,422`.

**Prova do comportamento na cópia.** O contador compara literalmente e o teste exige `(1, 1)` para `Ato` × `ato`. O registro antigo de encaminhamento era da fase de `6978b3234`. A triagem anexada ao Opus final desta árvore está [nao verificado]. `app/chamados/services/vigia_texto_resposta.py:19–25`, `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `relato_f5.txt:5,8,411,422`.

**PUBLICAR.** Anexar a linha “`casefold` — o contador exige igualdade literal. `Ato` × `ato` conta `(1, 1)`. Normalizar os dois lados esconderia essa divergência.” A execução isolada do mutante `casefold` está [nao verificado]. `app/chamados/services/vigia_texto_resposta.py:23–25`, `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`.

### 33 — Linha consolidada do PRONTO

**Segue devido.** O 1435.4 pede “uma linha no RELATO com o PRONTO do 0815 item a item (feito + onde esta a prova)” e proíbe “reduzir o PRONTO”. O 1710, verificador, exige a linha “Antes de pedir a suite pre-opus”. `avais_f5_em_ordem.txt`, 1435.4/PROIBIDO e 1710 VERIFICADOR, `relato_f5.txt:5,373`. [nao verificado]

**Sem publicação consolidada.** A linha 292 publica o portão da A em `d40f1c9dc`. As linhas 3–5 trazem evidências avulsas e o despacho da leitura que antecede a linha final. `relato_f5.txt:3–5,291–292`.

**PUBLICAR.** Uma linha vinculada ao hash-alvo com cada obrigação cumulativa, o estado e a referência à prova. O RELATO identifica a revisão C6 pelo prefixo `8938552e1`. A confirmação independente do hash Git integral e dos diffs históricos está [nao verificado]. `relato_f5.txt:5,8,292`.

### 34 — Pureza AST dos dois juízes

**Segue devido.** O 1710 PRONTO 1 diz “os dois juizes sem isinstance, sem modelo, sem banco”. O 1830.1c-1 manda “O corpo puro dos dois juizes entra no PRIMEIRO commit da volta A”. A decisão posterior registrada para `VIAS_FORA_DO_FATO` está no RELATO e no selo. Uma troca literal do requisito “sem modelo” está [nao verificado]. `avais_f5_em_ordem.txt`, 1710 PRONTO 1 e 1830.1c-1, `relato_f5.txt:8,294,302`. [nao verificado]

**Prova parcial na cópia.** Os dois corpos estão sem `isinstance`, sem import interno de modelo e sem consulta. `projecao_temporal_da_pergunta` ainda lê `PerguntaDisputa.VIAS_FORA_DO_FATO`. O selo isenta precisamente esse acesso. `app/chamados/juizes.py:1383–1403,1551–1560`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:161–179,265–279`.

**CONSTRUIR-CÓDIGO.** Tirar a dependência de classe em `juizes.py:1401`, preservando a fonte única das vias e o corte temporal. Atualizar o selo para rejeitar a referência ao modelo e conservar a cobertura do horizonte. Mudar só o teste deixaria a dependência no juiz. `app/chamados/models.py:2599–2603,2724`, `app/chamados/juizes.py:1394–1403`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:77–90`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:166–170`.

### 35 — Censo dos carregadores K

**Segue devido com a conta trocada.** O 1710 PRONTO 3 dizia “64 carregadores em lote + 5 R declarados + a sobra, com a lista K marcada”. O 1735.4–5 fixa “29 cobertos, 27 editados pelo Codex na B” e os 59 pontos mais a sobra. O 1830.6b-1 diz “K40 [...] PASSA A CLASSE R” e “M 24, R 6, pontos de toque 58 mais a sobra = 59 editados”. `avais_f5_em_ordem.txt`, 1710 PRONTO 3, 1735.4–5 e 1830.6b-1, `relato_f5.txt:312,315`.

**Censo da cópia.** A tabela abaixo fecha em **L10 + Q29 + M24 = 63 K com lote**, **R6**, mais a **sobra com lote**. São **64 sítios físicos nominais de prefetch**, cinco acima dos 59 pontos do aval porque o predesenho agrupava seis K em um ponto `fio_vivo`. Na cópia, `fio_vivo` faz o `any` sobre a coleção recebida e os seis chamadores têm cargas próprias. **64 sítios físicos** e **59 pontos de desenho** são unidades distintas. `predesenho.md` §§1–2, `app/chamados/juizes.py:1516–1526,2490`, tabela abaixo.

A volta B tem **29 K nominais**, por diferença entre os 39 L/Q e os 10 da A, incluindo K41/K42 em `models.py`. A autoria e os 27 efetivamente editados pelo Codex estão [nao verificado]. O RELATO da C ainda diz **25 K + sobra**. A tabela final tem **24 M + sobra**, com K40 avulso. A divergência do relato é de **um K M**. `predesenho.md` §§1,4, `app/chamados/models.py:1555,2071`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:464–467`, `relato_f5.txt:272,279,303`.

**Tabela nominal dos itens 35 e 36.** Os sítios partem de `app/`. `J` = `chamados/juizes.py`, `Mo` = `chamados/models.py`, `Re` = `chamados/reconciliador.py`, `S/` = `chamados/services/`, `C/` = `chamados/management/commands/`, `P/` = `ponto/management/commands/`. Na coluna RED, `A`, `B`, `C` são `app/chamados/tests/test_e4c_f5_voltaA_lotes.py`, `test_e4c_f5_voltaB_lotes.py` e `test_e4c_f5_voltaC_lotes.py` no mesmo diretório. `OA` é `test_e4c_f5_opus_models.py`. `mA`, `mB`, `mC` retiram o lote do K da linha em `app/chamados/tests/e4c_f5_voltaA_mutantes.py:10–18`, `e4c_f5_voltaB_mutantes.py:13–26` e `e4c_f5_voltaC_mutantes.py:13–22`. **`?` nesta tabela significa `[nao verificado]`.** Classes de `predesenho.md` §1 e aval 1830.6b-1.

| K | Classe | Sítio na cópia | RED ou controle nominal | Mutante |
|---|---|---|---|---|
| 1 | L | `S/tarefa_do_dia.py:537` | `A:42` | mA |
| 2 | L | `J:1665` | `A:85` | mA |
| 3 | R | `S/relavra_via.py:236` | `A:187`, O261 por candidata | ? |
| 4 | L | `J:1512` | `B:54` | mB |
| 5 | M | `C/auditar_invariantes_chamados.py:1592` | ? | ? |
| 6 | M | `C/auditar_invariantes_chamados.py:595` | ? | ? |
| 7 | M | `C/auditar_invariantes_chamados.py:1646` | ? | ? |
| 8 | M | `S/fio.py:2081` | `C:89` | mC |
| 9 | Q | `Re:1575` | `A:94` | mA |
| 10 | Q | `Re:1103` | `A:103` | mA |
| 11 | Q | `S/becos.py:87` | `B:62` | mB |
| 12 | M | `C/backfill_fio_morto_e4b.py:90` | ? | ? |
| 13 | M | `C/backfill_fio_morto_e4b.py:136,143` | `C:239`, releitura após reabertura | ? |
| 14 | M | `C/backfill_fio_morto_e4b.py:178` | ? | ? |
| 15 | L | `Mo:366` | `OA:120` e `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:569`, 1×5 | ? |
| 16 | Q | `S/disputa_emissao.py:1955` | `A:110` | mA só S128, `e4c_f5_voltaA_mutantes.py:37–40` |
| 17 | Q | `S/fio.py:1964` | `A:146` | mA |
| 18 | Q | `S/fio.py:1597` | `A:154` | mA |
| 19 | R | `S/materializacao.py:2122` | `app/chamados/tests/test_bug_par_volta.py:380`, efeito do lote por par | ? |
| 20 | R | `S/validacao.py:677` | `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:98`, decisão da porta | ? |
| 21 | M | `S/materializacao.py:328` | `C:100` | mC |
| 22 | R | `S/materializacao.py:287` | `app/chamados/tests/test_e4c_f5_voltaC_sitios.py:110`, valor da irmã | ? |
| 23 | Q | `folha/previa.py:66` | `B:71` | mB |
| 24 | Q | `ponto/services/cartorio.py:146` | `A:162` | mA |
| 25 | Q | `S/declaracao_texto.py:475` | ? | ? |
| 26 | Q | `S/declaracao_texto.py:589` | `B:97` | mB |
| 27 | Q | `S/declaracao_texto.py:635` | ? | ? |
| 28 | M | `C/backfill_decisoes_pergunta.py:534` | `C:119` | mC |
| 29 | Q | `J:1765` | `B:117` | mB |
| 30 | Q | `J:1773` | `B:131` | mB |
| 31 | Q | `J:2437` | `B:139` | mB |
| 32 | L | `J:2490` | `B:148` | mB |
| 33 | M | `C/auditar_invariantes_chamados.py:1726` | ? | ? |
| 34 | M | `C/auditar_invariantes_chamados.py:540` | `C:141` | mC |
| 35 | M | `C/auditar_invariantes_chamados.py:864` | ? | ? |
| 36 | M | `C/auditar_invariantes_chamados.py:899` | ? | ? |
| 37 | M | `C/auditar_invariantes_chamados.py:1786` | ? | ? |
| 38 | M | `C/auditar_invariantes_chamados.py:1798` | ? | ? |
| 39 | M | `C/disparar_perguntas_competencia.py:109` | ? | ? |
| 40 | R | `C/migrar_data_evento_dia_do_turno.py:466` | `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:102,109`, efeito do descarimbo | ? |
| 41 | Q | `Mo:1555` | `OA:132`, 1×5 | ? |
| 42 | Q | `Mo:2071` | `OA:149`, 1×5 | ? |
| 43 | Q | `Re:703` | `A:176` | mA |
| 44 | Q | `S/acoes_chamado.py:352` | ? | ? |
| 45 | Q | `S/competencia_trancada.py:121` | `B:176` | mB |
| 46 | Q | `S/competencia_trancada.py:166` | `B:179,182` | mB e `k46_sem_julgamento`, `e4c_f5_voltaB_mutantes.py:48–55` |
| 47 | Q | `S/disputa_emissao.py:2080` | `B:197` | mB |
| 48 | Q | `S/fio.py:2297` | ? | ? |
| 49 | Q | `S/fio.py:2301` | ? | ? |
| 50 | Q | `S/pergunta_no_app.py:186` | `app/chamados/tests/test_n1_perguntas_do_app.py:111`, identidade | ? |
| 51 | M | `S/vigia_fio.py:714` | `C:154` | mC |
| 52 | M | `S/vigia_fio.py:774` | `C:167` | mC |
| 53 | Q | `chamados/views.py:1412` | ? | ? |
| 54 | Q | `colaboradores/services/calendario.py:505` | `B:218` | mB |
| 55 | Q | `ponto/services/dia_do_colab.py:65` | `B:229` | mB |
| 56 | M | `C/estoque_l130_instante_sem_texto.py:38` | `app/chamados/tests/test_f1nova_b2_leitores.py:258`, dry/aplica | ? |
| 57 | M | `C/estoque_l130_instante_sem_texto.py:98` | `app/chamados/tests/test_f1nova_b2_leitores.py:380`, universo sob trava | ? |
| 58 | R | `S/validacao.py:431` | `app/chamados/tests/test_f1nova_b2_leitores.py:491`, guarda sob trava | ? |
| 59 | M | `C/arquivar_competencia_encerrada.py:158` | ? | ? |
| 60 | M | `S/vigia_fio.py:571` | `C:181` | mC |
| 61 | Q | `S/explicador.py:161` | ? | ? |
| 62 | L | `api/foto_recebida.py:81` | ? | ? |
| 63 | L | `C/encerrar_perguntas_com_fato.py:84` | ? | ? |
| 64 | L | `S/limbo_folgas.py:205` | ? | ? |
| 65 | Q | `P/supra_juiz.py:192` | ? | ? |
| 66 | M | `P/emitir_furo_retroativo.py:83` | `C:198` | mC |
| 67 | L | `P/reconciliar_grade.py:461` | ? | ? |
| 68 | L | `S/disputa_emissao.py:1436` | `B:239` | mB |
| 69 | M | `C/fechar_becos_disputa.py:33` | `C:218` | mC |
| Sobra | M, fora dos 69 | `C/dedup_perguntas_fato.py:75` | `C:230`, por grupo | mC |

A contagem da tabela é **32 K com RED 3×6 e mutante tira-lote** mais a sobra, ou **33 mutantes**. K13 acrescenta prova de frescor. K15/K41/K42 acrescentam casos 1×5. Isso dá **36 de 63 K** com prova nominal de algum aspecto da carga, e **27 sem RED de lote nominal identificado**. O RELATO registra 33/33 em `8938552e1`, com K16 e K47 discriminados. Os demais resultados individuais estão [nao verificado]. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:10–18`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:13–26`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:13–22`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264`, `app/chamados/tests/test_e4c_f5_opus_models.py:120–167`, `relato_f5.txt:5`.

**PUBLICAR.** A lista K acima, com as duas unidades de contagem, e a correção da linha da C. Para certificar o número histórico de edições e os 27 da B, falta o diff por volta da base e da cópia, com K41/K42 separados. `relato_f5.txt:272,279,303,315`, [nao verificado] para o diff.

### 36 — Os R declarados

**Segue devido com seis R.** O 1710 dizia “Os 5 da classe R (K 3,19,20,22,58) nao mudam”. O 1830.6b-1 manda “K40 [...] PASSA A CLASSE R: uma leitura por item, por natureza” e o PRONTO exige “K40 declarado R”. `avais_f5_em_ordem.txt`, 1710 VOLTA C e 1830.6b-1/PRONTO, `predesenho.md` §1.

**Prova estática na cópia.** Os seis vínculos são os K3, K19, K20, K22, K40 e K58, com sítio, RED e 

[truncado em 20000 de 42433 caracteres]

## 20/11 03:11 · sessao be0eb6d11212 · - · - · origem=Ronald (tmux)

**49.** O aval 1945 autorizou dublês dos módulos nomeados e `test_pergunta_viva`, com extensão por mesma causa e publicação dos módulos novos. 2005, 2050 e 2230 acrescentaram exceções específicas. A prova de 11 famílias verdes dentro desses limites segue devida. O censo histórico tinha 12 famílias e 112 ocorrências em `e673e0efe`. O RELATO separa `test_pergunta_viva` das 11 famílias. `relato_f5.txt:297,321`, `predesenho.md` §6.

| Módulo | Assert base declarada → árvore | Sítio do dublê |
|---|---:|---|
| `e3_v8_apoio` | 0 → 0 | `app/chamados/tests/e3_v8_apoio.py:12` |
| `test_e1_v10` | 31 → 31 | `app/chamados/tests/test_e1_v10.py:23` |
| `test_e3_c8b_sem_banco` | 86 → 86 | `app/chamados/tests/test_e3_c8b_sem_banco.py:14` |
| `test_e3_degrau_dono_e_porta` | 134 → 134 | `app/chamados/tests/test_e3_degrau_dono_e_porta.py:20` |
| `test_e3_pos_suite_sem_banco` | 29 → 29 | `app/chamados/tests/test_e3_pos_suite_sem_banco.py:13` |
| `test_e3_tarefa_admin_sem_banco` | 7 → 7 | `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:23` |
| `test_e4b_c4v7_memoria` | 65 → 65 | `app/chamados/tests/test_e4b_c4v7_memoria.py:19` |
| `test_e4b_classe_e_memoria` | 122 → 122 | `app/chamados/tests/test_e4b_classe_e_memoria.py:17` |
| `test_pergunta_viva` | 12 → 12 | `app/chamados/tests/test_pergunta_viva.py:5,16–73` |

A base cabe no selo e os valores da árvore foram recontados pela mesma AST. RED dos módulos é o selo de contagem e conformidade. Mutantes removem campo, alteram assinatura, alteram o valor de SILENCIO e removem assert. `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–35,38–87`. Outros oito consumidores do helper estão fora da tabela. `app/chamados/tests/e3_volta7_apoio.py:12`, `app/chamados/tests/test_bug_par_c2_sem_banco.py:13`, `app/chamados/tests/test_bug_par_sem_banco.py:14`, `app/chamados/tests/test_e3_r3_c5_sem_banco.py:11`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:17`, `app/chamados/tests/test_e3_r3_c7_sem_banco.py:13`, `app/chamados/tests/test_estado_aplicacao.py:5`, `app/ponto/tests/test_supra_juiz_executor.py:16`. **PUBLICAR** a seção B nominal, diff por família versus base e resultado final por família. O vínculo exaustivo de 17 consumidores a 11 famílias está [nao verificado].

**51.** Segue devido pelo 1710, “a 12a reescrita”. A 12ª família está identificada no `predesenho.md` §6 como a fixture ORM ilegal dos dois REDs 8. Os dois casos usam a porta de atos e explicitam atualização passiva da projeção. O caso do cálculo mede 1×5 e 0 SELECTs durante o juiz. O caso do fio mede 1×5, até um SELECT de decisões e verifica os PKs percorridos. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:119–152,154–186`. A mordida ao remover o prefetch do fio está publicada na A `d40f1c9dc`, com 3 > 1. `relato_f5.txt:292`. Execução nominal em `8938552e1` [nao verificado]. **PUBLICAR** o vínculo nominal 1 família/2 casos na linha consolidada.

**68.** Sete grupos nominais distintos no 1735/1830. A oitava menção detalha o mesmo 7(iv) da tabela de leitores. O 1830 7-viii exige casos novos. A regra segue com as exceções de 1945 e 2005, classes i–iv de 2050, v e vi dos dois avais 2230, passagem do chokepoint 2325 e teto medido 0110. `relato_f5.txt:8,14,266,279,286–295`.

| Autorização | Sítio e caso | Mutante ou prova |
|---|---|---|
| 1735 7(i) | `app/chamados/tests/test_e4c_f5_volta2_juizes.py:119–128,145–152,171–186` | Retira prefetch do fio, 3 > 1 na A. `relato_f5.txt:292`. |
| 1735 7(ii) | Cinco REDs com composta, RED 4 mantém controle ORM passivo. `app/chamados/tests/test_e4c_f5_volta2_juizes.py:37–105` | Divergências ato × projeção. Execução de mutante isolado por caso [nao verificado]. |
| 1735 7(iii) | `app/chamados/tests/e4c_f5_volta2_mutantes.py:31–37` | Troca composta por ORM com apelido, capturada em `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:220–251`. |
| 1735 7(iv) e 1830 7-iv | Sete leitores, 5 False e 2 True. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:498–544` | Retorno ao instante cru contraria RED de validação/reabertura, execução de mutante [nao verificado]. |
| 1735 7(v), K3 R | O261, unitário 3/6 ≤9. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:187–207` | Caso novo de SELECT de decisões 3/6 em `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:187–202`. |
| 1830 7-vi | Property recebe sentinela e `LeituraComposta`. `app/chamados/tests/test_f1nova_b2_leitores.py:87–97` | Remover delegação ou passar ORM contraria asserções. Execução de mutante [nao verificado]. |
| 1830 7-vii, K15 L | `_ancoradas` 1 → 2, fixo em 1×5. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:569–590` | Remover prefetch deixa de produzir a segunda consulta. Execução de mutante [nao verificado]. |

Anexar as exceções M4 e três âncoras 2005, oito identidades e três ajustes nominais do 2050, dublês, deslocamentos e tetos reais, com autorização por hunk. Casos-base `app/chamados/tests/test_cortes_f2_f9_m4.py:51–65`, `app/chamados/tests/e3_mutantes.py:30`, `app/chamados/tests/e4b_c12_ast.py:18–28`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:229–251`, `app/chamados/tests/test_e4b_classe_e_memoria.py:51–52,193–194,1036–1052`, `app/core/tests/test_medida_turno_do_par.py:247–271,558–562`, `app/chamados/tests/test_contract_dedup_status.py:135–147`, `app/chamados/tests/test_e3_r3_c7_sem_banco.py:181–191`, `app/chamados/tests/test_cef_c1_sem_banco.py:85–89,309–312`, `app/chamados/tests/e3_volta7_apoio.py:12,111`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267`. **PUBLICAR** inventário cruzado com o diff final. Contagem total de adaptações [nao verificado].

**32.** Segue devido pelo 1435 3e, “triagem com motivo em uma linha, anexa ao opus”. **PUBLICAR** e anexar a linha da cura. A comparação atual é literal, e o teste exige uma divergência para `Ato` × `ato`. Com `casefold` em ambos os lados, o esperado do mutante seria zero. O caso atual detecta também `casefold` só na leitura. A mordida de `casefold` só na coluna exigiria o caso espelhado. `app/chamados/services/vigia_texto_resposta.py:19–25`, `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`. O laudo anterior e o replay aparecem em `relato_f5.txt:357,360,422`. Anexo final [nao verificado].

## 20/11 03:11 · sessao ac4fe51e9b59 · - · - · origem=Ronald (tmux)

**55. Segue devido.** O 1735 1(c) mantém as assinaturas, exige um sítio para os indiretos e conversão dos diretos por volta. O 1830 1c-1 mantém a conversão e exige a contagem da catraca por volta. A varredura da cópia contou 42 diretos em 22 arquivos, 39 para `pergunta_respondida` e 3 para `projecao_temporal_da_pergunta`. São 41 compostos e 1 `values_list` autorizado em `app/chamados/services/raiox.py:135–147`. As oito provas AST passaram em memória, incluindo a troca do argumento por ORM cru nas 12 chamadas dos seis arquivos da A. `app/chamados/tests/test_e4c_f5_selo_chamada.py:20–26,124–158,182–279`.

A tabela nominal das 42 chamadas e seus REDs foi enviada ao agente principal. O ponto comum dos indiretos é `app/chamados/juizes.py:867,894,899,1406–1409,1428`. O `relato_f5.txt:300` substitui a linha de base AST 19 por 41 e `:288` relata 24 após A. O `:289` prevê 10 e 0 após B e C. A identidade dos 55 indiretos e dos 41 diretos históricos está [nao verificado]. A matriz anexada pertence ao levantamento de 15/11, e o predesenho dá o agregado de 55 sobre a leitura de `e673e0efe`. Falta publicar os 42 da cópia e obter a MATRIZ-F5-A citada no `relato_f5.txt:321` para conciliar por identidade. `matriz.md:4–9`, `predesenho.md` §3(ii).

**59. Segue devido com a exceção posterior.** 1830 1c-2 autoriza `folha/previa.py` “so em :142 (argumento do juiz) e :64 (decisoes em lote)” e exige “Nenhuma outra linha de folha/”. Na cópia, K23/Q carrega decisões em `app/folha/previa.py:64–67`. O RED 3×6 comprova que os PKs foram julgados e conta SELECTs de decisões. Seu mutante retira o prefetch. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:40–52,71–95`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:15,83–115`. O segundo sítio passa o composto em `app/folha/previa.py:143`, com import local em `:142`. O RED semântico está em `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:247–249`. A catraca acusa ORM cru também nesse sítio. `app/chamados/tests/test_e4c_f5_selo_chamada.py:182–187`.

A prova de prévia da competência 11 registra 4 empresas, 450 colaboradores e 5.861 dias, dos quais 4.597 com material, com zero diferenças nas duas datas fixas. Foi feita contra `a3c23d10b`, com base `eb1e49c1e`. O relato declara que `8938552e1` alterou só um teste. A igualdade do conteúdo de `folha/` entre as revisões e o limite histórico de duas linhas estão [nao verificado]. Falta publicar o diff integral da pasta contra a base e vincular a sonda anterior por igualdade comprovada ou repetir `montar_previa` na cópia pedida. `relato_f5.txt:4–5`, `app/folha/previa.py:28`.

## 20/11 03:10 · sessao 1bd04010ae4d · - · - · origem=Ronald (tmux)

**35 e 36.** O inventário nominal completo foi enviado ao agente principal. Na cópia há **63 K em lote, 6 R e a sobra em lote**. Os 63 se distribuem em L10, Q29 e M24. K40 satisfaz a reclassificação literal de 1830 6b-1, com releitura por PK em `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:464–467`. `relato_f5.txt:272` ainda declara 25 K na C. A árvore mostra 24 K M, mais a sobra. O histórico das edições está [nao verificado].

Há **64 consultas com prefetch na tabela nominal**, cinco a mais que os 59 pontos de toque autorizados porque `fio_vivo` faz o `any` sobre a coleção recebida e seis chamadores L fazem seu próprio prefetch. `app/chamados/juizes.py:1516–1526,2490`, `app/api/foto_recebida.py:81`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:84`, `app/chamados/services/limbo_folgas.py:205`, `app/ponto/management/commands/reconciliar_grade.py:461`, `app/chamados/services/disputa_emissao.py:1436`. A varredura AST da cópia retornou 73 chamadas de prefetch em 62 escopos e 37 arquivos, incluindo nove chamadas fora da tabela nominal.

São **32 K com RED 3×6 e mutante tira-lote**, mais a sobra, perfazendo os 33 relatados. K13 tem prova de frescor após reabertura e K15, K41 e K42 têm provas 1×5. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:10–18`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:13–26`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:13–22`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–264`, `app/chamados/tests/test_e4c_f5_opus_models.py:120–167`, `relato_f5.txt:5`.

Os seis R são **K3** `app/chamados/services/relavra_via.py:236`, **K19** `app/chamados/services/materializacao.py:2122`, **K20** `app/chamados/services/validacao.py:677`, **K22** `app/chamados/services/materializacao.py:287`, **K40** `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:466` e **K58** `app/chamados/services/validacao.py:431`. K3 tem RED por candidata. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:187–202`. A declaração atualizada no brief está [nao verificado].

## 20/11 03:05 · sessao 0c1a294b7b12 · E4C-F5-CONSTROI · leitura · origem=Code

| Item | Aval e trecho literal | O que esta árvore demonstra, e o que a sessão declara | Veredito |
|---|---|---|---|
| 01. Matriz dos 15 sítios e censo de entrada | 0815, etapa A, “MATRIZ POR SITIO, 15 linhas” e “contagem unica de sitios, igual no §2, no selo e no PRONTO”. Etapa B, “perguntas de lei 0, matriz com as 15 linhas completas e CENSO APROVADO do wrapper”. | O wrapper tem a porta `--censo` em `bin/codex_pergunta.sh:241–247`. O fechamento de 15 sítios, zero perguntas e aprovação é declarado em `relato_f5.txt:485`. A matriz nominal com campo, fonte, porta, laço, RED e mutante por sítio, e sua correspondência com o selo, estão [nao verificado]. |
| 02. Testemunha da leitura | 0815, etapa A, “testemunha = DecisaoPergunta.leitura -> LeituraDoAto (chamados/models.py), 3 campos”. | **3 campos** em `app/chamados/models.py:19,4351`, `validada_em`, `respondida_em`, `resposta_colab`. O tipo próprio do compositor tem cinco campos e preserva o tipo da testemunha. `app/chamados/models.py:24–25,4354–4362`. | DEMONSTRADO-NA-ARVORE |
| 03. Conservação do passivo sem ato | 0815, etapa A, “pergunta SEM nenhum ato conserva a projecao”. | A leitura incorpora a projeção legada por `linhas_da_projecao_sem_linha`. O teste sem atos exercita os dois juízes, inclusive os cortes temporais. `app/chamados/models.py:4263,4333–4338`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:59–75`. A prova de frota está no item 11. | DEMONSTRADO-NA-ARVORE |
| 04. RED-5 do histórico | 0815, etapa A, “RED-5 do historico fica”. | O teste cobre resposta, validação e materialização do histórico. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:75–103`, `app/chamados/services/historico.py:97–106`. | DEMONSTRADO-NA-ARVORE |
| 05. SILÊNCIO e dono admin | 0815, etapa B, “admin vira dono em competencia aberta; reescreve a expectativa do teste da F1”. | O ramo `SILENCIO` devolve `silencio_decidido` com ações vazias. O teste F1 ainda cobra igualdade da fila antes e depois da ressuscitação e espera `silencio_decidido`. `app/chamados/services/tarefa_do_dia.py:182–183`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328–344`. | NAO-ACHEI |
| 06. Catraca de leitores crus | 0815, etapa B, “catraca dos leitores crus em 0 nos sitios migrados, varrendo tambem string em getattr/update_fields/refresh_from_db”. | O selo da volta 1 inspeciona **2 funções** de produção, `espera_o_texto_do_colaborador` e `_eventos_dos_chamados`. Ele checa atributos e alguns argumentos de `getattr`. A cobertura dos 15 sítios e de `update_fields=` e `refresh_from_db(fields=…)` está [nao verificado]. O selo de chamada aos juízes tem outro objeto, no item 56. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:154–218`. | NAO-ACHEI |
| 07. Três voltas verdes | 0815, PRONTO, “3 voltas verdes”. 1710, forma e PRONTO 8, “Cada volta so abre com a suite da anterior verde” e “suite verde por volta”. 1830, 1c-1, “a suite de cada volta e so PASSAGEM”. | `medidas_da_sessao.txt:3–4` cobre uma volta intermediária verde em `a3c23d10b` e uma C3 vermelha nos quatro tetos. `relato_f5.txt:278,285,287` declara recortes de A/B e a passagem excepcional do chokepoint pelo 2325, mantida pelo 0110. Faltam logs que fechem as três passagens A/B/C por hash e ordem na pista 2. | SO-POR-EXECUCAO |
| 08. Ordem, commits e RED antes da cura | 0815, etapa B, “cada uma com commit, RED e verificador do delta”. 1435, item 1, “ordem das voltas, commit + RED + verificador do delta por volta”. 1710, RED, “por volta, antes do codigo”. 1830, 1c-1, “O corpo puro dos dois juizes entra no PRIMEIRO commit da volta A”. | `relato_f5.txt:271,278,287,299,302` e `medidas_da_sessao.txt:2` declaram commits e rodadas parciais. A cópia veio sem metadados Git. A identidade `8938552e1339750519afbb5f47b66c2846acff14`, a ordem exata dos commits e o RED anterior à cura de cada volta estão [nao verificado]. A prova exige o histórico e os logs RED por base e volta. | SO-POR-EXECUCAO |
| 09. Verificador do delta por volta | 0815, etapa B, “verificador do delta”. 1435, item 1, “verificador do delta por volta”. | `medidas_da_sessao.txt:10` cobre o delta **C4+C5, BLOQUEIA=5**. `relato_f5.txt:271,278` declara triagem de C e verificador de B. Faltam os laudos por delta que fechem A, B e C com suas triagens. | SO-POR-EXECUCAO |
| 10. Mutantes mortos com log | 0815, PRONTO, “mutantes mortos com log em disco”. 1710, RED, “caso que MORDE por volta: tirar o lote de UM carregador da volta faz o teste dele crescer”. | **33 mutantes tira-lote** no código. **A, 9:** K1, K2, K9, K10, K16, K17, K18, K24, K43. **B, 14:** K4, K11, K23, K26, K29, K30, K31, K32, K45, K46, K47, K54, K55, K68. **C, 10:** K8, K21, K28, K34, K51, K52, K60, K66, K69 e sobra. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–21`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23`. `medidas_da_sessao.txt:5` cobre o resultado declarado na C6, **33/33**, K16 `9 != 15`, K47 `8 != 14`, `Ran 76 OK`. O log de mordida de cada mutante está [nao verificado]. | SO-POR-EXECUCAO |
| 11. Sombra (a), mudança dos juízes e passivo | 0815, etapa B e PRONTO, “JUIZES: quantas perguntas mudam de resposta; esperado so as 253 ja medidas (40 + 213)” e “passivo sem ato que mude = PAREI”. | Os dois juízes estão em `app/chamados/juizes.py:1383,1551`. `relato_f5.txt:720,724,832,844` traz recortes anteriores, incluindo a classificação de 213 como `SILENCIO` e uma previsão. `medidas_da_sessao.txt:7` cobre a **property**, que é outra comparação. Falta a sombra antes × depois pela função real, por volta, com as classes dos 40/213, eventuais classes novas e a contagem do passivo sem ato. | SO-POR-EXECUCAO |
| 12. Sombra (b), decisões por porta | 0815, etapa B e PRONTO, “para cada porta da volta 3, quantas perguntas mudam de decisao (valida, recusa, materializa)”. | Há caminhos de validação, recusa e materialização em `app/chamados/services/validacao.py:661,858` e `app/chamados/services/materializacao.py:1286,2074`, com casos de divergência em `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:98,116` e `app/chamados/tests/test_e4c_f5_voltaC_sitios.py:117–145`. Falta a medida de sombra antes × depois, por porta. `medidas_da_sessao.txt:1–10` não a declara. | SO-POR-EXECUCAO |
| 13. Sombra (b), entregues e exportadas | 0815, etapa B, “as que caem em competencia entregue ou exportada NAO se movem por este caminho: lista para ele”. | Há guardas no código. Falta a lista ou a contagem zero da sonda **das portas da F5** para o universo entregue/exportado. A igualdade da prévia não mede mutação dessas portas. `app/chamados/services/validacao.py:231–234,692`, `app/chamados/services/materializacao.py:818–828`, `medidas_da_sessao.txt:6`. | SO-POR-EXECUCAO |
| 14. Sombra (c), CHECK-DE-TELA | 0815, etapa B e PRONTO, “CHECK-DE-TELA na arvore (espelho, central, tarefa do dia pelo servico, api do app) contra a referencia_1911.md”. | Os **4 pontos de entrada** identificáveis são espelho, Central, carga da tarefa e API de perguntas. `app/ponto/urls.py:10`, `app/chamados/views.py:721`, `app/chamados/services/tarefa_do_dia.py:825`, `app/api/urls.py:92`. `relato_f5.txt:515,520` declara referência e ensaios de outras árvores. Falta o comparativo das quatro superfícies na F5 com a referência, incluindo custo e escritas. `medidas_da_sessao.txt:1–10` não o cobre. | SO-POR-EXECUCAO |
| 15. Verificador final inteiro e zero bloqueios | 0815, PRONTO, “verificador Codex 0 BLOQUEIA”. 1710, verificador, “pelo ESCOPO INTEIRO, nao pelo delta da ultima volta”. | `medidas_da_sessao.txt:10` cobre execução de escopo inteiro em `aa3bdd88c` com **9 bloqueios** e delta C4+C5 com **5**. `relato_f5.txt:7` declara curas e triagens. Falta laudo de **escopo inteiro** da árvore final, com matriz, predesenho e avais, e resultado **0 BLOQUEIA**. | SO-POR-EXECUCAO |
| 16. Suite pre-opus verde da árvore inteira | 0815, PRONTO, “suite pre-opus na PISTA 2”. 1435, item 2, “A arvore que vai a suite e ao opus e a das tres voltas”. 1710, suites e PRONTO 8, “pre-opus da espinha na VM” e “pre-opus verde”. | **1710 substitui a pista 2 pela VM para a pre-opus da espinha**, mantendo a pista 2 para passagens. `bin/suite.sh:23–29,236–257` fornece a porta. `medidas_da_sessao.txt:10` declara que a pre-opus ainda não rodou. O `Ran 17040 OK` do merge anterior é sinal, conforme 1435. Falta log da suite inteira na VM ligado ao hash final. `relato_f5.txt:385,392,409`. | SO-POR-EXECUCAO |
| 17. Opus final único | 0815 e 1710, PRONTO, “opus final”. 1435, PRONTO, “opus final unico sobre a arvore inteira”. | `medidas_da_sessao.txt:10` declara que o Opus final ainda não rodou. O laudo anterior julgou o merge parcial. Falta o laudo único da árvore inteira, ligado ao mesmo hash da suite, com as triagens anexas. `relato_f5.txt:372,385`. | SO-POR-EXECUCAO | [nao verificado]
| 18. Uma fonte e um compositor nos três sítios | 1435, 3a, “os tres sitios do item 1 leem a mesma autoridade”. 1710, desenho e PRONTO 2, “UM compositor sobre DecisaoPergunta.leitura” e “nenhuma composicao local sobrando”. 1735, 1c, “TIPO PROPRIO”. | **1 compositor**, `DecisaoPergunta.composta`, entrega `LeituraComposta` com instantes do ato e texto da projeção. **3 sítios** delegam a ele, `particao_do_app`, `PerguntaDisputa.respondida` e `espera_o_texto_do_colaborador`. `app/chamados/models.py:24–25,3810–3816,4354–4362`, `app/chamados/services/pergunta_no_app.py:80–92`, `app/chamados/services/fio.py:187–190`, `app/chamados/tests/test_e4c_f5_opus_models.py:57–72`. | DEMONSTRADO-NA-ARVORE |
| 19. Grep publicado do compositor | 1710, PRONTO 2, “grep no RELATO”. | O código permite verificar os três sítios do item 18. O **grep publicado** para o PRONTO está [nao verificado]. `relato_f5.txt:7,299,302` declara despacho, comissão e provas parciais. | NAO-ACHEI | [nao verificado]
| 20. RED de divergência no app | 1435, 3a, “RED por sitio com o caso que morde (ato diz X, projecao diz Y)”. 1735, 3a, “RED com divergencia ato x projecao em CADA um dos 3 sitios”. | O caso de partição testa texto do ato × texto da coluna e dois mutantes de fonte. O ato passa pela porta e o desvio de projeção é marcado como `PASSIVO`. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:32–53`. Falta log nominal do caso e das duas mordidas. `medidas_da_sessao.txt:5` cobre tira-lote. | SO-POR-EXECUCAO |
| 21. RED de divergência na property | 1435, 3a, “RED por sitio”. 1735, 3a, “models.py:3791”. | O teste da property cobre texto somente no ato e ato reaberto com projeção ainda carimbada, com projeção passiva por `update`. `app/chamados/tests/test_e4c_f5_opus_models.py:37–54,87–89`. `relato_f5.txt:302` declara RED/GREEN agregado do commit Opus. Falta o log nominal deste caso. `medidas_da_sessao.txt:7` cobre a sonda da property. | SO-POR-EXECUCAO |
| 22. RED de divergência no fio | 1435, 3a, “RED por sitio”. 1735, 3a, “fio.py:192”. | Há guarda de divergência e mutante que restaura a composição local. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–71`. `relato_f5.txt:461` declara uma mordida anterior de texto do fio. Faltam logs nominais das três variantes atuais. `medidas_da_sessao.txt:5` cobre tira-lote. | SO-POR-EXECUCAO |
| 23. RED de `sem_veredito_entre` | 1735, 1a e 3a, “a fonte unica alcanca sem_veredito_entre” e “e em :2004 e :3235”. | Compositor no caminho real e RED para texto e instante divergentes em `app/chamados/models.py:2022–2027` e `app/chamados/tests/test_e4c_f5_opus_models.py:76–81`. `relato_f5.txt:302` declara resultado agregado. Falta o log RED/GREEN deste caso. `medidas_da_sessao.txt:1–10` não o individualiza. | SO-POR-EXECUCAO |
| 24. RED de `estado` | 1735, 1a e 3a, “e estado (:3235); passam pelo compositor” e “e em :2004 e :3235”. | Compositor no caminho real e RED que espera `PENDENTE` nas duas divergências. `app/chamados/models.py:3249–3268`, `app/chamados/tests/test_e4c_f5_opus_models.py:83–85`. `relato_f5.txt:302` declara resultado agregado. Falta o log RED/GREEN deste caso. `medidas_da_sessao.txt:1–10` não o individualiza. | SO-POR-EXECUCAO |
| 25. Consumidores por consequência | 1735, 1a, “Os consumidores deles (fechamento, CSF [...] Pauta [...]) entram por consequencia, sem edicao propria”. | Os **3 caminhos** delegam à cadeia de `sem_veredito_entre`, fechamento por `respostas_sem_veredito`, CSF diretamente, Pauta por `respostas_sem_veredito`. O histórico de edição própria está [nao verificado]. `app/chamados/models.py:1982–2002,2029–2035`, `app/ponto/supra_juiz.py:573–574`, `app/chamados/services/competencia_trancada.py:124–126`. | DEMONSTRADO-NA-ARVORE |
| 26. Vigia em lotes | 1435, 3b, “deixa de carregar as 51.563 linhas de uma vez (iterador ou lote)”. 1735, PRONTO 8, “com iterator(chunk_size) e decisoes por lote”. | `contar` usa `prefetch_related('decisoes')` com `iterator(chunk_size=lote)`. O padrão é **2.000** por lote. As 51.563 linhas são universo declarado de sombra, [nao verificado] pela árvore. `app/chamados/services/vigia_texto_resposta.py:8–25`, `medidas_da_sessao.txt:7`. | DEMONSTRADO-NA-ARVORE |
| 27. Contagem preservada no vigia | 1435, 3b, “RED de contagem igual antes e depois”. | O teste guarda os resultados `(0,2)`, `(1,2)`, `(1,1)`, `(1,3)` nos cenários de passivo, divergência, recorte e dois atos. `app/chamados/tests/test_e4c_f5_contador_texto.py:26–38`. O 1735 acrescenta a prova de custo do item 28, e a contagem fica como guarda, conforme `relato_f5.txt:363`. Falta log comparativo do mesmo teste antes e depois. `medidas_da_sessao.txt:1–10` não o cobre. | SO-POR-EXECUCAO |
| 28. RED de consultas do vigia por lote | 1735, PRONTO 8, “com RED de consultas por lote”. | Cinco perguntas, lote de dois, exatamente **3 SELECTs de decisões**. `app/chamados/tests/test_e4c_f5_contador_texto.py:87–95`. `relato_f5.txt:7` declara RED/GREEN agregado da C5, incluindo a cura do vigia. `medidas_da_sessao.txt:3,5` declara baterias verdes sem individualizar este teste. Falta o log nominal de `test_RED_decisoes_em_lotes_de_chunk`. | SO-POR-EXECUCAO |
| 29. Remoção da chave sem leitor | 1435, 3c, “divergencia_texto_resposta_universo [...] ganha leitor no ESTADO ou sai no mesmo commit”. | A saída mantém `divergencia_texto_resposta` e o teste exige ausência da chave `divergencia_texto_resposta_universo`. A remoção no commit da volta 2 é declarada em `relato_f5.txt:350,364`, com identidade histórica [nao verificado]. `app/chamados/management/commands/vigiar_fio_morto.py:264–287`, `app/chamados/tests/test_e4c_f5_contador_texto.py:56–70`. | DEMONSTRADO-NA-ARVORE |
| 30. Decisão técnica da chave no RELATO | 1435, 3c, “decisao tecnica da sessao, uma linha no RELATO”. | A linha registra a escolha pela remoção, o efeito sobre o alarme e a reversão. `app/docs/RELATO.md:37`. | DEMONSTRADO-NA-ARVORE |
| 31. Selo AST por identidade da variável | 1435, 3d, “reconhece o sitio por AST independente do nome da variavel; mutante com apelido tem de morder”. | O selo acompanha o parâmetro, os aliases e os alvos de iteração. Há **2 testes** dedicados à renomeação, `test_MORDE_parametro_renomeado_e_pergunta_inteira` e `test_renomear_parametro_ou_alvo_nao_muda_o_selo`. `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:164–185,220–251`, `app/chamados/tests/e4c_f5_volta2_mutantes.py:11–38`. | DEMONSTRADO-NA-ARVORE |
| 32. Triagem de `casefold` anexa ao Opus | 1435, 3e, “triagem com motivo em uma linha, anexa ao opus”. | O teste atual exige que `Ato` × `ato` conte como divergência. Há registro de triagem anterior e intenção de anexar. A triagem final anexada está [nao verificado]. `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `app/docs/RELATO.md:29,32,94`, `medidas_da_sessao.txt:10`. | NAO-ACHEI |
| 33. Linha do PRONTO publicada | 1435, item 4 e PRONTO, “uma linha no RELATO com o PRONTO do 0815 item a item (feito + onde esta a prova)”. 1710, verificador, “Antes de pedir a suite pre-opus, a sessao publica a linha do PRONTO item a item”. | `relato_f5.txt:291` publica uma linha parcial do portão da A. `relato_f5.txt:7` ainda registra pendências, e `relato_f5.txt:3–4` publica provas avulsas. A linha consolidada da árvore inteira, com cada item e sua prova, está [nao verificado]. | NAO-ACHEI | [nao verificado]
| 34. Pureza AST dos dois juízes | 1710, PRONTO 1, “os dois juizes sem isinstance, sem modelo, sem banco”. 1830, 1c-1, “O corpo puro dos dois juizes entra no PRIMEIRO commit da volta A”. | **2 juízes**, `projecao_temporal_da_pergunta` e `pergunta_respondida`. Ambos estão sem `isinstance`, import local de modelo e consulta. O primeiro referencia `PerguntaDisputa.VIAS_FORA_DO_FATO`; o selo isenta nominalmente essa constante. Portanto, a leitura literal de “sem modelo” tem essa exceção na árvore. A sessão registra a decisão em `relato_f5.txt:293`. `app/chamados/juizes.py:1383–1403,1551–1560`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:161–179,265–279`. | NAO-ACHEI |
| 35. Censo de carregadores em lote e lista K | 1710, PRONTO 3, “64 carregadores em lote + 5 R declarados + a sobra, com a lista K marcada”. 1735, itens 4–5, “29 cobertos, 27 editados pelo Codex na B” e “os 59 pontos de toque mais a SOBRA”. 1830, 6b-1, “M 24, R 6, pontos de toque 58 mais a sobra = 59 editados”. | **1830 retira K40 do lote.** No universo nominal de 69, passam a ser **63 não-R**, com **58 pontos de toque + sobra**, no lugar dos 60 editáveis de 1735. Na árvore, os **33 K distintos identificados em testes de lote** são os 32 do item 10 mais **K13**, com RED de releitura após reabertura. `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239–263`. Também contei **73 sítios explícitos de prefetch de decisões, em 62 escopos e 37 arquivos**, listados abaixo. Esse censo inclui cargas existentes e a do vigia. A associação exaustiva aos 69 K, os 58 pontos editados, os 29/27 da B e os 24 M estão [nao verificado] sem a matriz nominal e o histórico. `relato_f5.txt:314,320`. | NAO-ACHEI |
| 36. Lista dos R declarados | 1710, volta C e PRONTO 3, “Os 5 da classe R (K 3,19,20,22,58) nao mudam” e “5 R declarados”. 1830, 6b-1 e PRONTO, “K40 [...] PASSA A CLASSE R” e “K40 declarado R”. | A lista avalizada passou de **5 para 6**, **K3, K19, K20, K22, K40, K58**. Dela consegui vincular **1 K ao sítio nominal** pela árvore, K40, no item 37. O261 aparece explicitamente como classe R no teste, sem um K associado ali. A declaração dos seis K no brief e os cinco vínculos restantes estão [nao verificado]. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:199–207`, `relato_f5.txt:314`. | NAO-ACHEI |
| 37. K40 por item | 1830, 6b-1, “K40 [...] PASSA A CLASSE R: uma leitura por item, por natureza; nao reestruturar o comando”. | `_descarimbados` itera os PKs, faz `.get(pk=pk)` por item e chama `pergunta_viva(p)`. Esse caminho preserva o desenho R identificado pelo aval. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:448–467`. | DEMONSTRADO-NA-ARVORE |
| 38. Sobra de dedup por grupo | 1710, volta C, “a SOBRA chamados/management/commands/dedup_perguntas_fato.py:75”. 1830, 6b-1, “a sobra mede por grupo”. | **1 sobra**, `dedup_perguntas_fato`. O comando carrega decisões por grupo, o RED compara 3 e 6 perguntas no grupo, e a entrada `sobra` tem mutante próprio. `app/chamados/management/commands/dedup_perguntas_fato.py:75–76`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:230–237`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:22`. | DEMONSTRADO-NA-ARVORE |
| 39. REDs 3 × 6 da A | 1710, RED e PRONTO 4, “consultas IGUAIS com 3 e com 6 perguntas” e “REDs 3 x 6 verdes”. 1830, 6b-1, “conta SO as consultas a chamados_decisaopergunta” e “prova no proprio teste de que as 3 e as 6 foram julgadas”. | **9 casos de igualdade**, K1, K2, K9, K10, K16, K17, K18, K24 e K43, mais o caso unitário O261 do item 44. O comparador confirma o conjunto dos PKs julgados e o total de SELECTs de decisões. K15 aparece na lista do aval e o fio tem REDs de **1 × 5**; seu RED nominal de 3 × 6 está [nao verificado]. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:21–38

[truncado em 20000 de 43332 caracteres]

## 20/11 02:48 · sessao f227b1196105 · E4C-F5-CONSTROI · verificador · origem=Code

**BLOQUEIA o pouso de E4C-F5-CONSTROI.** O delta corrige o memo do C6 na instância entregue a `registrar`, mas a mesma operação por `pergunta_id` ainda pode deixar um julgamento obsoleto. O RED do vigia aceita a materialização de todos os lotes. O K16 passou a mutar também o sítio do K47. E a triagem descarta uma divergência real entre a sonda da ressuscitação e o ato gravado sob trava. `app/chamados/models.py:4373,4381–4387,1558,1592,1651,1702` · `app/chamados/tests/test_e4c_f5_contador_texto.py:90–95` · `app/chamados/tests/e4c_f5_voltaA_mutantes.py:21,39–48` · `app/chamados/tests/e4c_f5_voltaB_mutantes.py:23,98–108`

Conferi em leitura os seis blobs da cópia contra `a3c23d10b`. O intervalo desde `aa3bdd88c` contém C4 `3c351fee4` e C5 `a3c23d10b`. A verificação dos mutantes foi estática, por AST. Execuções Django, SQL capturado, dados de produção e resultado do pouso **[nao verificado]**. A contagem final usa os seis quesitos do pedido.

## 1. Linhas do delta e contratos — BLOQUEIA

| Linhas alteradas | Comportamento, escritores e leitores |
|---|---|
| `app/chamados/models.py:4378–4380` | Comentários explicam a dependência nova do memo em relação à trilha e declaram o alcance à mesma instância. A chave é escrita por `pergunta_viva`, que também a lê para devolver o booleano memorizado. A chave contém colunas e atributos da célula e do destino, mais `hoje` e `agora`. O caminho com `carga` pula o memo. O julgamento efetivo lê validação e resposta compostas. `app/chamados/juizes.py:846–864,887–907,1406–1411` |
| `app/chamados/models.py:4381–4385` | O novo local `_perg` guarda o mesmo `ln.get('pergunta')` que antes era usado diretamente no `getattr`. Continua removendo só a entrada `decisoes` do cache de relações da instância recebida. O prefetch alimenta esse cache, a sonda também o escreve na cópia, e `leitura` e `respostas_anteriores` o consomem via `.all()`. `pergunta` é o objeto FK e `pergunta_id` é o PK. `registrar` aceita os dois formatos, usa os PKs para o outbox e só tem um objeto para invalidar no primeiro formato. `app/chamados/models.py:1555,1591–1595,3824–3826,4191,4329–4333,4373–4389` |
| `app/chamados/models.py:4386–4387` | Remove `_pv_memo` mesmo quando a instância não tem prefetch. O C6 volta a calcular no próximo acesso avulso. Os outros descartes locais estão em `_desfazer_em_memoria` e `descarimbar`. As colunas usadas na chave são escritas por portas como resposta, validação e descarimbo. A trilha é gravada pelo `bulk_create` dentro de `registrar`. `app/chamados/models.py:3379–3386,3423–3430,4373–4387` · `app/chamados/services/disputa_emissao.py:1481,1494,1527–1534` · `app/chamados/services/validacao.py:168–175` |
| `app/chamados/services/vigia_texto_resposta.py:8,11–16` | `LOTE` passa a declarar o default `2000` de `lote`, em **perguntas por chunk**. `vigiar_fio_morto` passa um queryset, inteiro ou recortado por empresa, e usa o default. O RED fornece `lote=2`. A docstring declara a intenção de memória limitada por lote. `app/chamados/management/commands/vigiar_fio_morto.py:192–196` · `app/chamados/tests/test_e4c_f5_contador_texto.py:90–95` |
| `app/chamados/services/vigia_texto_resposta.py:17–25` | A troca executável é de `for p in universo` para `universo.iterator(chunk_size=lote)`. O queryset continua prefetcheando `decisoes`. O próprio contador incrementa `total` por pergunta e `divergentes` por diferença literal, tratando a projeção falsa como `''`. A projeção é escrita pela resposta humana, automática e reabertura. O texto da trilha vem de `ato_texto` das linhas registradas, com a reconstrução do passivo feita por `leitura`. O comando imprime os dois números e grava o divergente no censo. São dois textos com origens declaradas e dois contadores em perguntas, sem recorte de dia ou instante nesse vigia. `app/chamados/services/disputa_emissao.py:1481,1532–1534,1617,1656,1682–1688` · `app/chamados/models.py:4285–4294,4333–4351` · `app/chamados/management/commands/vigiar_fio_morto.py:228–229,275–277` |
| `app/chamados/tests/e4c_f5_voltaA_mutantes.py:21,37–51` | `ANCORAS` declara a cardinalidade por chave, com `2` para K16 e default `1`. Antes o `assert` rejeitava K16 e o desempacotamento exigia um alvo. Agora o transformador lê a lista de nós coletados e troca todos os nós de identidade correspondente. Em K24, reescreve o argumento textual para preservar o prefetch da relação intermediária. Nos demais, devolve o receptor da chamada e retira o prefetch. `sem_lote` lê o seletor de ambiente e o par módulo/função de `CARREGADORES`, compila a árvore em memória e troca o atributo do módulo durante o teste. As unidades são nós AST por chave e identidade de nó. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19,24–36,54–68` |
| `app/chamados/tests/test_e4c_f5_contador_texto.py:87–95` | O novo método e sua docstring declaram a prova de lote. O laço cria cinco perguntas e grava uma resposta por pergunta. `CaptureQueriesContext` fornece os registros cujo campo `sql` o teste filtra por substring. As asserções leem `(divergentes, total)` e a cardinalidade dessa lista de SQL, com esperados `(0, 5)` e `3`. `app/chamados/tests/test_e4c_f5_contador_texto.py:20–24,87–95` |
| `app/chamados/tests/test_e4c_f5_voltaFINAL.py:1–22` | A docstring descreve a invalidação local. Os imports trazem o C6, a porta, o modelo, a fixture e `calado`. A classe cria uma pergunta, exige vida e memo, insere `D.VALIDACAO` por `pergunta=p`, relê por PK e compara a instância original à fresca. `ato` identifica o evento, `ato_em` é seu instante, `ato_por` é o autor e `ato_texto` é o texto da linha. A validação usa o instante para decidir C6. `AGORA` e `VALIDACAO_EM` são datetimes UTC da fixture, com `VALIDACAO_EM < AGORA`. O C6 conserva igualdade com o horizonte e deriva `hoje` como dia civil local quando recebe `agora`. `app/chamados/models.py:4191–4195,4342–4351` · `app/chamados/tests/cortes_f2_f9_apoio.py:30–34,58–64` · `app/chamados/juizes.py:890–895,1394–1402,1410–1411` · `app/chamados/veredito_lavrado.py:95–104` |
| `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,248–253,260–267` | Os quatro blocos de comentários registram aval, atribuição, classe, parcelas e soma. As quatro linhas de dicionário substituem exclusivamente os inteiros `230→234`, `294→298`, `145→146` e `219→221`. Os cenários fornecem a chave textual e a contagem capturada. `_cobrar` lê `TETOS[nome]` e aceita `medido <= teto`. A unidade é SQL capturado no trecho da batida. `on_commit` tem outra asserção, de cardinalidade exata. `app/ponto/tests/test_selo_chokepoint_escrita.py:319–330,459–475,495–510` |

**Defeito residual do memo.** No RED novo, trocar apenas `dict(pergunta=p, ...)` por `dict(pergunta_id=p.pk, ...)` deixa a inserção válida e deixa `_perg=None`. A instância original conserva `_pv_memo`, a chave continua igual e o C6 devolve `True`. A instância fresca vê a validação e devolve `False`. O mesmo limite vale para outra instância já carregada com o mesmo PK. O formato por ID é usado, por exemplo, na ressuscitação e na reabertura para correção. Reuso obsoleto em produção **[nao verificado]**. `app/chamados/tests/test_e4c_f5_voltaFINAL.py:14–22` · `app/chamados/models.py:1713–1719,4373–4387` · `app/chamados/juizes.py:856–864,895–898,1408–1411` · `app/chamados/services/disputa_emissao.py:1639–1645`

## 2. Mordida dos testes — BLOQUEIA [nao verificado]

| Teste novo ou afetado | Mutante mínimo e resultado pela leitura | [nao verificado]
|---|---|
| `test_RED_registrar_descarta_o_memo_do_c6` | Apagar `pop('_pv_memo', None)`. **Morde.** A fixture exige memo com resultado `True`, o controle fresco exige `False` e a última asserção compara ambos. O mutante `if _perg is not None and not _cache:` antes do `pop` **passa** com a instância sem prefetch da fixture. Ele falha em invalidar o memo numa instância que chega com os dois caches. O caminho por ID também fica fora da fixture. `app/chamados/tests/test_e4c_f5_voltaFINAL.py:14–22` · `app/chamados/tests/cortes_f2_f9_apoio.py:58–64` · `app/chamados/models.py:4382–4387` |
| `test_RED_decisoes_em_lotes_de_chunk` | Restaurar `for p in universo:`. **Morde** pela carga única frente às três esperadas. Trocar por `for p in list(universo.iterator(chunk_size=lote)):` **passa**, retendo todas as perguntas e seus prefetches em memória enquanto preserva a contagem. A fixture chega ao sítio e exige cinco perguntas processadas. A medida verifica cardinalidade de SQL por substring, sem inspecionar tamanho de cada `IN`, verbo ou retenção dos objetos. `(0, 5)` também aceita divergências sempre iguais a zero neste caso alinhado. Os testes existentes com texto divergente cobrem esse defeito semântico. `app/chamados/services/vigia_texto_resposta.py:19–25` · `app/chamados/tests/test_e4c_f5_contador_texto.py:21–38,40–53,87–95` |
| `test_RED_k16_pendentes_s128` | Retirar só o prefetch S128. **Morde** na igualdade 3×6 porque o laço chama o compositor por candidata e a captura mede SELECTs de decisões. A remoção atual dos dois prefetches mede a combinação. Ela não prova a mordida de `:2080` isoladamente. Há um RED separado para esse sítio, K47. `app/chamados/services/disputa_emissao.py:1955,2000,2025,2080–2093` · `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:21–38,110–144` · `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–216` |
| Batida, `1 fecha`, `3 fecham`, `1 FICA`, `3 FICAM` | Acrescentar um `SELECT 1` antes do retorno de sucesso de `registrar_batida`. **Morde cada caso se ele ocupa o teto anotado.** Ocupação no C5 **[nao verificado]**. Os dois casos que fecham exigem exatamente 1 e 3 fechados, e `1 FICA` exige o único vivo. **`3 FICAM` aceita um ou dois vivos**, se custo e `on_commit` couberem. Nos quatro, uma consulta adicional compensada por uma retirada pode passar. Um SQL de escrita que substitua um SELECT de mesmo custo sem alterar o resultado inspecionado ou `on_commit` também pode passar. `app/ponto/registro_batida.py:176–181` · `app/ponto/tests/test_selo_chokepoint_escrita.py:319–330,459–475,495–510` |

## 3. `CARREGADORES` de A, B e C — BLOQUEIA

As 33 chaves resolvem para os corpos nomeados e suas cardinalidades atuais satisfazem os seletores. **A exceção de escopo é K16**. Ele agora retira S128 **e** a varredura sem credor, que B registra como K47. Assim o antigo erro de âncora foi corrigido e o mutante de A passou a mexer em um segundo sítio já nomeado em B. Execução dos 33 REDs **[nao verificado]**. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–21,24–51,61–68` · `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12–27,87–115` · `app/chamados/tests/e4c_f5_voltaC_mutantes.py:12–23,35–59`

| Volta | Chaves e sítios efetivamente alterados |
|---|---|
| A | `k1` tarefa `app/chamados/services/tarefa_do_dia.py:537`; `k2` portas `app/chamados/juizes.py:1665`; `k9` reatáveis, `k10` órfãs, `k43` absolvidas `app/chamados/reconciliador.py:1575,1103,703`; `k16` S128 **e K47** `app/chamados/services/disputa_emissao.py:1955,2080`; `k17` universo do fato, `k18` vizinhos `app/chamados/services/fio.py:1964,1597`; `k24` cartório, preservando `perguntas_que_resolvem`, `app/ponto/services/cartorio.py:146`. |
| B | `k4`, `k29`, `k30`, `k31`, `k32` em `app/chamados/juizes.py:1512,1765,1773,2437,2490`; `k11` beco `app/chamados/services/becos.py:87`; `k23` prévia `app/folha/previa.py:66`; `k26` declaração `app/chamados/services/declaracao_texto.py:589`; `k45` primeiro e `k46` segundo prefetch de `medir` `app/chamados/services/competencia_trancada.py:121,166`; `k47` segundo prefetch e `k68` emissão `app/chamados/services/disputa_emissao.py:2080,1436`; `k54` calendário `app/colaboradores/services/calendario.py:505`; `k55` dia do colaborador `app/ponto/services/dia_do_colab.py:65`. |
| C | `k8` grupo `app/chamados/services/fio.py:2081`; `k21` paridade `app/chamados/services/materializacao.py:328`; `k28` backfill `app/chamados/management/commands/backfill_decisoes_pergunta.py:534`; `k34` auditoria `app/chamados/management/commands/auditar_invariantes_chamados.py:540`; `k51`, `k52`, `k60` no vigia `app/chamados/services/vigia_fio.py:714,774,571`; `k66` emissão retroativa `app/ponto/management/commands/emitir_furo_retroativo.py:83`; `k69` fecho de becos `app/chamados/management/commands/fechar_becos_disputa.py:33`; `sobra` dedup `app/chamados/management/commands/dedup_perguntas_fato.py:75`. |

As outras cinco âncoras textuais de B2 também têm uma ocorrência cada nos corpos visados. As duas mutações da varredura de chamada miram o argumento de C21 no GET e em `par_do_intervalo`. Nenhuma dessas sete âncoras mostrou erro de cardinalidade na fonte. `app/chamados/tests/e4c_f5_voltaB_mutantes.py:28–55,68–79,118–127` · `app/chamados/tests/e4c_f5_voltaC_mutantes.py:62–70` · `app/api/views.py:2581,2590` · `app/chamados/services/validacao.py:914` · `app/escala/services/cadastro_realidade.py:499–500` · `app/chamados/services/competencia_trancada.py:164–167` · `app/chamados/services/materializacao.py:193`

## 4. Anotações do selo — NAO BLOQUEIA

Os quatro números têm as parcelas positivas anotadas com sítio, commit e classe. O `+2` do cartório está em uma única linha para o mesmo sítio. Os `−2` trazem método e commit, com classe no cabeçalho e sem linha própria. O item 2 do aval 0110 exige a anotação de cada acréscimo por sítio. Os sítios positivos conferem. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,248–253,260–267`

| Parcela no selo | Conferência da origem e do verbo |
|---|---|
| `+1` absolvidas, `eaf69fe33` | Prefetch montado em `:703` e avaliado na iteração em `:706`. SELECT de decisões. `app/chamados/reconciliador.py:702–706` |
| `+1` plano e `+1` S143, `18bf86f01` | Ambos chamam `_ancoradas`. O SELECT extra nasce no prefetch avaliado pelo `list`. `app/chamados/models.py:364–366,400` · `app/chamados/services/disputa_emissao.py:1985` |
| `+2` cartório, `eaf69fe33` | O laço avalia o prefetch aninhado. O nível adicional é `decisoes`, que produz um SELECT por avaliação com perguntas. A pilha que conta as duas ocorrências em cada cenário de fecho **[nao verificado]**. `app/ponto/services/cartorio.py:145–146,1347` |
| `+1` `fechar` e `−2` compartilhados, `9d386d1b1` | A carga da lista e o prefetch são SELECTs. O prefetch em `fechar` entrou em `18bf86f01`. `9d386d1b1` antecipou a carga e passou a lista a `respostas_sem_veredito`, evitando sua aquisição separada de perguntas e decisões. As escritas da porta ficam no registro de silêncio e no `save`, em linhas distintas da atribuição de custo. `app/chamados/models.py:1998,2018–2021,2071–2072,2110–2112,2122` |
| `+1` universo do fato, `eaf69fe33` | SELECT de decisões no prefetch que o `list` avalia, chamado por `_nascer`. `app/chamados/services/fio.py:1963–1964` · `app/chamados/models.py:3122` |
| `+1` avulso, `18bf86f01` e `0db6afc73` | O SELECT nasce na avaliação de `p.decisoes.all()` sem prefetch. `18bf86f01` introduz o compositor e `0db6afc73` a chamada na guarda. `app/chamados/models.py:4333,4360` · `app/chamados/reconciliador.py:367` |

As somas dão `+4, +4, +1, +2` na ordem do selo. O aval aceita a atribuição fornecida, e o selo cita a comparação `eb1e49c1e → aa3bdd88c`. A captura final e a medida no C5 **[nao verificado]**. A frase “o proximo +1 falha onde nascer” é mais forte que a asserção agregada discutida no quesito 2. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,248–253,260–267,319–330`

## 5. Descartes da triagem — BLOQUEIA

| Achado | Veredito e trecho |
|---|---|
| **D1 — Faces SQL** | **NAO SUSTENTA.** “`q_vivas/resposta_display -> True`” no 1830 item 7-iv autoriza uma expectativa para a fixture de projeção vazia. A fila e as corrigíveis filtram `respondida_em__isnull=False` antes do prefetch. Com texto na projeção e instante só no ato, o C21 responde sim e ambas excluem a pergunta. `q_vivas` também pode perder a continência quando a célula absolve e C6 conserva a resposta por falta de concordância. O veto a atalhos SQL do 1735 sustenta o veto à técnica, e a autorização da divergência geral e os números citados **[nao verificado]**. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:488–520` · `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:41–53,66–80` · `app/chamados/juizes.py:1347–1348,1428–1440,1588,1765–1775` · `app/chamados/models.py:3604–3611,3622–3639` |
| **D2 — `raiox`** | **SUSTENTA.** “O placar [...] FICA FORA da F5”, 1735 item 1(b). O `values_list` lê as colunas, entrega a tupla ao C21 e consta da exceção nominal. O registro específico em E4D **[nao verificado]**. `app/chamados/services/raiox.py:135–147` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–26` · `app/docs/BACKLOG.md:2023` |
| **D3 — Horizonte** | **NAO SUSTENTA.** “A reabertura em T2 também limpa” explica o caso com coluna sincronizada. A árvore já tem fixture com `validada_em=T1`, `VALIDACAO` em T1 e `RECUSA` em T2. Com `T1 <= agora < T2`, a base ainda via T1 e encerrava no degrau da validação. O HEAD aplica a recusa futura antes do corte, perde T1 e pode dar viva se as demais guardas permitem. T corta os atos primeiro. `eb1e49c1e:app/chamados/juizes.py:1389–1406` · `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:105–109` · `app/chamados/juizes.py:1394–1411,1487–1488` · `app/chamados/models.py:4302,4339–4351` · `app/chamados/services/tarefa_do_dia.py:398–429` |
| **D4 — `fechar` pré-trava** | **NAO SUSTENTA.** “Só juntou as duas cargas” omite dois efeitos. A segunda observação da base podia ver uma resposta chegada após a primeira. E no ramo de silêncio a base consultava a sequência da segunda instância após a trava, quando sem prefetch. O HEAD usa a primeira foto em `_pend`, em `vivas` e também no `ato_texto` de `SILENCIO`. Uma resposta concorrente após essa foto pode ficar fora dos julgamentos e da foto anexada ao silêncio. `eb1e49c1e:app/chamados/models.py:2048,2057–2067,2083–2089` · `app/chamados/models.py:2071–2087,2095,2103–2112,3270–3284,3405–3410,4333` |
| **D5 — Pureza** | **SUSTENTA** a preexistência do acesso à tupla e a ausência de consulta nesse trecho, `if via not in PerguntaDisputa.VIAS_FORA_DO_FATO`. O selo contém a exceção. Aval formal para a dependência do modelo sob a exigência literal de pureza **[nao verificado]**. `eb1e49c1e:app/chamados/juizes.py:1398` · `app/chamados/juizes.py:1401` · `app/chamados/models.py:2721–2724` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:161–179` |
| **D6 — M1** | **SUSTENTA.** “Respondida = o juiz” encontra a L-130. M1 agora exige texto e instante pelo C21. O RED força chegada ao M1 sem porta e distingue texto vazio, instante igual ao horizonte e instante posterior ao horizonte. A mudança semântica é real. `app/docs/LEIS.md:151` · `app/escala/services/cadastro_realidade.py:497–509` · `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:276–296` |
| **D7 — JSON** | **NAO DA PARA DIZER** quanto à autorização. “Desenho 1710 [...] matriz.md item 94” **[nao verificado]**. O 1735 fala do argumento dos juízes. A troca na serialização é real e o RED exige o instante do ato mesmo com a coluna vazia. A serialização usa a composição corrente, embora a entrada tenha capturado `agora` para a consulta e T. `app/api/views.py:2552–2556,2579–2587,2601` · `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:66–80` |
| **D8 — Memo do modal** | **SUSTENTA** o motivo técnico. “Memo de render, juiz intacto” confere. A tag guarda a contagem por disputa no `render_context` e chama o mesmo contador. O teto 90 existe e os testes conferem dois cartões, disputas diferentes e novo render após a escrita. Medida `90→92` e aval específico de C3 **[nao verificado]**. `app/chamados/templatetags/chamados_tags.py:222–238` · `app/templates/chamados/partials/modal_fio.html:396–401` · `app/core/tests/test_selo_performance.py:143,377–380` · `app/chamados/tests/test_e4c_f5_voltaC3_reabrir.py:73–116` |
| **D9 — Sonda e relógio** | **NAO SUSTENTA.** “Só linha com `ato_em` no FUTURO da sonda” significa apenas `ato_em > S`. Uma linha gravada normalmente após a leitura de S pode estar visível quando o prefetch roda. “Posterior a toda linha confirmada antes 

[truncado em 20000 de 27709 caracteres]

## 20/11 02:47 · sessao 987528a4fa0c · - · - · origem=Ronald (tmux)

Conferi por AST as 33 chaves A/B/C. Todas atingem funções existentes e têm a cardinalidade esperada. K16 elimina duas âncoras, incluindo a segunda, que tem seletor próprio K47. K47 elimina somente a segunda. O RED K16 exige todos os PKs julgados e igualdade de consultas entre 3 e 6, mas a fixture dá às perguntas um destino vivo, então `_todas` passa pelo `continue` antes do C6. A mordida de K16 não isola as duas remoções. A fixture específica de K47 usa perguntas sem destino no ramo sem credor. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:14,21,39,44` · `app/chamados/tests/e4c_f5_voltaB_mutantes.py:23,98,100,104` · `app/chamados/services/disputa_emissao.py:1955,2080,2083,2093` · `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:21,28,110,130` · `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197,216`

O RED do memo morde a remoção do `pop('_pv_memo', None)` com o ato registrado na mesma instância e o mesmo horizonte. Fora desse recorte, uma segunda instância com o mesmo PK ou uma linha recebida somente com `pergunta_id` conserva o memo. A ressuscitação usa essa forma por ID. `app/chamados/tests/test_e4c_f5_voltaFINAL.py:14,22` · `app/chamados/juizes.py:895,898` · `app/chamados/models.py:1714,4373,4381,4387`

O RED do contador morde `for p in universo` por prever 1 consulta de decisões em vez de 3. Morde retirar o prefetch sozinho por prever 5 consultas em vez de 3. Passa com `for p in list(universo.iterator(chunk_size=lote))`, mantendo toda a frota na memória e as mesmas três consultas. A fixture alinhada também aceita o leitor produzir zero divergências após descartar o resultado da leitura, embora o teste preexistente morda esse erro. `app/chamados/services/vigia_texto_resposta.py:19,23` · `app/chamados/models.py:4333` · `app/chamados/tests/test_e4c_f5_contador_texto.py:26,38,87,95`

O selo cobra contagem total e callbacks, de modo que um SELECT redundante no caminho ultrapassa o teto se o cenário já está no teto. Pode passar com a troca de um SELECT por uma escrita de custo igual sem novo callback. No cenário 3 FICAM, `vivos >= 1` aceita dois fechamentos. Os cenários que fecham verificam todos os estados finais, mas só consultam esses estados depois da batida medida. Runs e contagens efetivas [nao verificado]. `app/ponto/tests/test_selo_chokepoint_escrita.py:319,330,448,475,488,510`

## 20/11 02:46 · sessao a021eb5414ee · CADEIA-20-PASSO-1 · sonda · origem=Code

`FOTO` traz a leitura dos insumos nas duas bordas da etapa. `LEITURA` registra a chamada que o passe realmente fez, inclusive quando a guarda pulou a célula. A string preserva o fuso que entrou no hash. As horas de observação usam `timezone.localtime`. `app/ponto/services/cartorio.py:493–578,1399–1407,1527–1558`
`DELTA` mostra as partes alteradas e o intervalo observado. `ESCRITA` mostra o antes/depois no SQL, a pilha e a fila naquele instante. `FILA` registra inclusões, consumos e descartes posteriores. `JUIZO` mostra os insumos efetivamente entregues ao classificador. `app/ponto/services/cartorio.py:1126–1164,1209–1231,1605–1627,2035–2037` `app/chamados/models.py:3344–3352,3484–3487`
`RESUMO` tem uma linha por célula com P1, P2 e P3. A contagem é por linha de chamado ou pergunta com mudança monitorada, após a última `LEITURA` do passe. Se a releitura cair dentro do intervalo em que a mudança foi detectada, a ordem fica `[nao verificado]`. `app/ponto/services/cartorio.py:1123,1156–1157,1222,1399–1401,1541–1547`
A sonda mantém a frota e a ordem do comando. A cascata de materialização pode alcançar clones por PK sem filtro de colaborador, e a equivalência de cortar a frota está `[nao verificado]`. `app/ponto/management/commands/processar_cartorio.py:51–69` `app/chamados/signals.py:149–167` `app/chamados/services/materializacao.py:1232–1279`
`BRACO` seleciona três rodadas com relógio fixo ou o controle que avança em P3. A resposta efetiva do juiz do turno, os deltas transitórios entre fotos e os efeitos de callbacks `on_commit`, consumidores ou escritores externos estão `[nao verificado]` até onde as linhas da sonda os alcançarem. O ensaio termina por rollback e a execução desta entrega está `[nao verificado]`. `app/ponto/management/commands/processar_cartorio.py:35–41` `app/ponto/turnos.py:2117–2134` `app/chamados/signals.py:134–146,229–240` `bin/sombra.sh:483–492`

=====SCRIPT=====
```python
import datetime as dt
import hashlib
import io
import json
import logging
import re
import sys
import threading
from collections import defaultdict
from contextlib import ExitStack, redirect_stderr, redirect_stdout
from functools import wraps
from pathlib import Path
from types import SimpleNamespace
from unittest.mock import patch

from django.core.management import call_command
from django.db import connection, transaction
from django.db.models import Q
from django.utils import timezone
from freezegun import freeze_time  # app/requirements.txt:43

from chamados import juizes, models as mc, reconciliador
from chamados.catalogo.modulos import data_do_chamado
from chamados.services import disputa_emissao, fabrica_da_celula, fecho_runtime, materializacao
from colaboradores.models import Colaborador
from core import observ
from escala import servico_jornada, utils as eu
from escala.models import CelulaDia, EscalaColaborador
from ponto import supra_juiz, turnos
from ponto.management.commands import detectar_ausencias, processar_cartorio
from ponto.models import Ausencia, Batida
from ponto.portas import celula as portas
from ponto.services import cartorio, cluster_espurio, justificativa

CELULAS = (128355, 129943, 138359, 129476, 129490, 138541, 142995, 143945, 129134)
EMPRESAS = (2, 3, 4)
RODADAS = ('P1', 'P2', 'P3')
BRACO = 'encerrado'  # 'em_curso', 'encerrado' ou 'cruza'
CEDO = '2026-11-20T00:30:00-03:00'
TARDE = '2026-11-20T06:27:00-03:00'
RELOGIOS = {
    'em_curso': (CEDO, CEDO, CEDO),
    'encerrado': (TARDE, TARDE, TARDE),
    'cruza': (CEDO, CEDO, TARDE),
}
NV = '[nao verificado]'

CH = mc.ChamadoColaborador
PD = mc.PerguntaDisputa
EMISSOR = detectar_ausencias.Command
CMD = processar_cartorio.Command

CORPO = cartorio._julgar_colab_corpo             # app/ponto/services/cartorio.py:1264
ATO = cartorio.julgar_colab                     # app/ponto/services/cartorio.py:1059
IMPRESSAO = cartorio.impressao_insumos          # app/ponto/services/cartorio.py:493
CONTADORES = cartorio.contadores               # app/ponto/services/cartorio.py:2117
CLASSIFICAR = supra_juiz.classificar_dia        # app/ponto/supra_juiz.py:217
APP = str(Path(cartorio.__file__).resolve().parents[2]) + '/'
ET_COMANDO = 'app/ponto/management/commands/processar_cartorio.py:35'

CH_STATUS = dict(CH.STATUS_CHOICES)
PD_VIAS = dict(PD.VIAS)
VEREDITOS = CelulaDia.VEREDITOS_CARTORIO + CelulaDia.VEREDITOS_ADMIN
BAT_TIPOS = dict(Batida.TIPO_CHOICES)
AUS_STATUS = dict(Ausencia.STATUS_CHOICES)
P_CARIMBO = (
    'materializada_em', 'materializada_por_id', 'via_resolucao',
    'validada_em', 'validada_por_id', 'respondida_em',
)
P_VAZIO = (None, None, '', None, None, None)
CH_ESCOPO = ('colaborador_id', 'celula_id', 'celula__data', 'modulo_origem', 'contexto_json')

DML = re.compile(
    r'^\s*(UPDATE|INSERT\s+INTO|DELETE\s+FROM)\s+'
    r'(?:"?\w+"?\.)?"?([a-z_][a-z_0-9]*)"?\s',
    re.IGNORECASE,
)
ESTORNO = re.compile(r'^\s*ROLLBACK TO SAVEPOINT\s+"?(\w+)"?', re.IGNORECASE)


class Desfazer(Exception):
    pass


class FalhaSonda(Exception):
    pass


def local(valor):
    return timezone.localtime(valor).isoformat()


def json_extra(valor):
    if isinstance(valor, dt.datetime):
        return local(valor)
    if isinstance(valor, dt.date):
        return valor.isoformat()
    raise TypeError('tipo inesperado na saida')


def fonte(codigo, linha):
    arquivo = codigo.co_filename
    if arquivo.startswith(APP):
        arquivo = 'app/' + arquivo[len(APP):]
    else:
        arquivo = Path(arquivo).name
    return '%s:%s' % (arquivo, linha)


def pilha(quadro):
    app, outros = [], []
    while quadro:
        codigo = quadro.f_code
        item = '%s:%s' % (fonte(codigo, quadro.f_lineno), codigo.co_name)
        (app if codigo.co_filename.startswith(APP) else outros).append(item)
        quadro = quadro.f_back
    return (app + outros)[:8]


def codigo_publico(valor, permitidos):
    if valor is not None and valor != '' and valor not in permitidos:
        raise FalhaSonda('codigo persistido fora do vocabulario publicado') [nao verificado]
    return valor


class Silencio(io.TextIOBase):
    def write(self, texto):
        return len(texto)


class Sonda:
    def __init__(self):
        self.saida = sys.stdout
        self.erros = []
        self.seq = 0
        self.rodada = None
        self.empresa = None
        self.cels = {}
        self.por_colab = defaultdict(list)
        self.por_dia = {}
        self.etapas = []
        self.corpos = []
        self.capturas = []
        self.prova = None
        self.prova_turno = None
        self.c1 = []
        self.versao_sql = 0
        self.cache_fotos = {}
        self.ultima_foto = {}
        self.deltas = defaultdict(list)
        self.leituras = defaultdict(list)
        self.escritas = defaultdict(list)
        self.savepoints = defaultdict(set)
        self.estornadas = set()

    def linha(self, tipo, **dados):
        self.seq += 1
        registro = dict(seq=self.seq, rodada=self.rodada, empresa=self.empresa,
                        hora=local(timezone.now()), **dados)
        print(tipo, json.dumps(registro, ensure_ascii=True, separators=(',', ':'),
                               default=json_extra), file=self.saida, flush=True)
        return self.seq

    def erro(self, etapa, ids, exc):
        if any(exc is anterior for anterior in self.erros):
            return
        self.erros.append(exc)
        # Mensagens externas podem conter nomes, CPF, SQL ou texto de resposta.
        seguro = repr(exc) if type(exc) is FalhaSonda else (
            "%s('[redigido]')" % type(exc).__name__
        )
        celula = ','.join(map(str, ids)) if ids else NV
        print('ERRO', etapa, celula, seguro, file=self.saida, flush=True)

    def abrir_universo(self):
        self.cels = {
            pk: (cid, data, emp)
            for pk, cid, data, emp in CelulaDia.objects.filter(pk__in=CELULAS)
            .values_list('pk', 'colaborador_id', 'data', 'colaborador__empresa_id')
        }
        for pk in CELULAS:
            if pk not in self.cels:
                self.erro(ET_COMANDO, (pk,), FalhaSonda('celula ausente na sombra'))
                continue
            cid, data, emp = self.cels[pk]
            if emp not in EMPRESAS:
                self.erro(ET_COMANDO, (pk,), FalhaSonda('empresa fora do lote'))
            self.por_colab[cid].append(pk)
            self.por_dia[cid, str(data)] = pk

    def fila(self, pk):
        # Fila fisica e contador do MESMO ato. app/ponto/services/cartorio.py:1114,1149–1164,1209–1231
        quadro = sys._getframe(1)
        while quadro and quadro.f_code is not ATO.__code__:
            quadro = quadro.f_back
        if quadro is None:
            return {'ato': None}
        l = quadro.f_locals
        cid = l['c'].pk
        out = l['out']
        conv = l.get('_conv')
        corpo = self.corpos[-1] if self.corpos else None
        escolhido = corpo is not None and pk in corpo.selecionadas
        return {
            'ato': cid,
            'rejulgar': any(c.pk == pk for c in out.get('rejulgar', ())),
            'rejulgar_na_releitura': conv is not None and
                any(c.pk == pk for c in conv.get('rejulgar', ())),
            'segunda_em_execucao': escolhido and corpo.tipo == 'segunda',
            'releitura_em_execucao': escolhido and corpo.tipo == 'releitura',
            'releitura_convocada': conv is not None and any(c.pk == pk for c in l['cels']),
            'emitidos_entrada': l.get('_e0'),
            'emitidos': out.get('emitidos'),
            'emitidos_na_releitura': None if conv is None else conv['emitidos'],
            'rejulgar_descartado': out.get('rejulgar_descartado'),
        }

    def contadores(self):
        sonda = self

        class Fila(list):
            def append(self, cel):
                super().append(cel)
                if cel.pk in sonda.cels:
                    f = sys._getframe(1)
                    sonda.linha('FILA', celula=cel.pk, acao='incluiu',
                                ponto=fonte(f.f_code, f.f_lineno), estado=sonda.fila(cel.pk))

        class Out(dict):
            def setdefault(self, chave, padrao=None):
                # Todos os produtores reais usam setdefault(...).append(...).
                # app/ponto/services/cartorio.py:1149–1150,1703–1704,1723,1737,1815,1831,1860
                if chave == 'rejulgar' and chave not in self:
                    padrao = Fila(padrao)
                return super().setdefault(chave, padrao)

            def pop(self, chave, *padrao):
                valor = super().pop(chave, *padrao)
                if chave == 'rejulgar' and valor:
                    f = sys._getframe(1)
                    acao = 'consumiu' if f.f_code is ATO.__code__ and f.f_lineno in (1156, 1157) else 'descartou'
                    for cel in valor:
                        if cel.pk in sonda.cels:
                            sonda.linha('FILA', celula=cel.pk, acao=acao,
                                        ponto=fonte(f.f_code, f.f_lineno), estado=sonda.fila(cel.pk))
                return valor

        return Out(CONTADORES())

    def sha256(self, dado=b''):
        # Leitura dos bytes e da lista local no ponto real do SHA.
        # app/ponto/services/cartorio.py:502–507,508–578
        if self.capturas:
            quadro = sys._getframe(1)
            if quadro.f_code is IMPRESSAO.__code__:
                self.capturas[-1].append((dado, tuple(quadro.f_locals['partes'])))
        return hashlib.sha256(dado)

    def impressao(self, *args, **kw):
        f = sys._getframe(1)
        if f.f_code is not CORPO.__code__:
            return IMPRESSAO(*args, **kw)
        l = f.f_locals
        cel = l['cel']
        universo = self.prova if self.prova is not None else self.cels
        if cel.pk not in universo:
            return IMPRESSAO(*args, **kw)

        etapas = self.etapas[-1:]
        etapa = etapas[0].fonte if etapas else fonte(f.f_code, f.f_lineno)
        captura = []
        self.capturas.append(captura)
        try:
            batidas, aus, chamados, dna = args[:4]
            codigo_publico(aus, AUS_STATUS)
            for batida in batidas:
                codigo_publico(batida.tipo, BAT_TIPOS)
            d = dna or {}
            versao, marcos = d.get('dna_versao'), d.get('marcos')
            chaves = {'hi', 'hf'} | {k for par in CelulaDia.PARES_PAUSA_DNA for k in par}
            if versao is not None and type(versao) is not int:
                raise FalhaSonda('versao de DNA fora do formato')
            if marcos is not None and (
                not isinstance(marcos, dict) or set(marcos) - chaves
                or any(v is not None and (not isinstance(v, str) or not re.fullmatch(r'\d\d:\d\d', v))
                       for v in marcos.values())
            ):
                raise FalhaSonda('marcos de DNA fora do formato')
            codigo_publico(kw.get('veto'), ('VETO_DEGENERADA', 'VETO_REGUA'))
            parcial = kw.get('cob_parcial')
            if isinstance(parcial, str) and not re.fullmatch(
                r'(?:sem_periodo|dentro|fora|nao_sei|1)(?:;disp=\d+(?:,\d+)*)?', parcial
            ):
                raise FalhaSonda('cobertura parcial fora do formato')

            h = IMPRESSAO(*args, **kw)  # app/ponto/services/cartorio.py:493–578
            if len(captura) != 1:
                raise FalhaSonda('chamada real nao entregou exatamente um preimage')
            bruto, partes = captura[0]
            por_insumo = dict(zip(('batidas', 'ausencia_status', 'chamados', 'dna'), partes[:4]))
            por_insumo.update((p.partition(':')[0], p) for p in partes[4:])
            # Sao as listas ja calculadas pelo corpo, inclusive no caminho da guarda.
            # app/ponto/services/cartorio.py:1399–1404,1527–1547
            vivos = {ch.pk for ch in l.get('vivos', ())}
            fio = {ch.pk for ch in l.get('_com_fio', ())}
            turno = self.prova_turno if self.prova is not None else (
                self.corpos[-1].turnos if self.corpos else {}
            )
            foto = {
                'insumos': bruto.decode('utf-8'), 'hash': h, 'partes': por_insumo,
                'chamados': [
                    {'pk': ch.pk, 'status_local': codigo_publico(ch.status_local, CH_STATUS),
                     'viva': ch.pk in vivos, 'viva_com_fio': ch.pk in fio}
                    for ch in chamados
                ],
                'turno': turno.get(cel.pk),
                'hoje': l['hoje'], 'agora': l['agora'],
            }
            if self.prova is not None:
                self.prova[cel.pk] = foto
            else:
                tipo = self.corpos[-1].tipo if self.corpos else NV
                seq = self.linha('LEITURA', celula=cel.pk, etapa=fonte(f.f_code, f.f_lineno),
                                 tipo=tipo, impressao_objeto=cel.impressao,
                                 veredito_objeto=codigo_publico(cel.veredito, VEREDITOS),
                                 fila=self.fila(cel.pk), **foto)
                self.leituras[self.rodada, cel.pk].append((seq, tipo, h))
            return h
        except Exception as exc:
            self.erro(etapa, (cel.pk,), exc)
            raise
        finally:
            self.capturas.pop()

    def classificar(self, *args, **kw):
        f = sys._getframe(1)
        cel = f.f_locals.get('cel') if f.f_code is CORPO.__code__ else None
        if cel is None or cel.pk not in self.cels or self.prova is not None:
            return CLASSIFICAR(*args, **kw)
        etapa = 'app/ponto/supra_juiz.py:217'
        try:
            resultado = CLASSIFICAR(*args, **kw)  # app/ponto/services/cartorio.py:1411–1415,1605–1627
            l = f.f_locals
            self.linha('JUIZO', celula=cel.pk, etapa=etapa, chamada=fonte(f.f_code, f.f_lineno),
                       total=kw['chamados_total'], protestam=kw['chamados_vivos'],
                       com_fio=kw['cobrancas_vivas'],
                       pks_protestam=[ch.pk for ch in l.get('_protestam', ())],
                       pks_com_fio=[ch.pk for ch in l.get('_com_fio', ())],
                       turno_encerrado=kw['turno_encerrado'], codigos=[c for c, _ in resultado])
            return resultado
        except Exception as exc:
            self.erro(etapa, (cel.pk,), exc)
            raise

    def motivo_turno(self, real):
        @wraps(real)
        def embrulho(turnos_dia, ec, data, agora, celulas=None):
            f = sys._getframe(1)
            c = f.f_locals.get('colaborador') if f.f_code is turnos._dias_encerrados_puro.__code__ else None
            pk = self.por_dia.get((c.pk, str(data))) if c is not None else None
            destino = self.prova_turno if self.prova is not None else (
                self.corpos[-1].turnos if self.corpos else None
            )
            if pk is None or destino is None:
                return real(turnos_dia, ec, data, agora, celulas=celulas)
            dados = {}
            self.c1.append(dados)
            try:
                # app/ponto/turnos.py:2117–2134,2170–2183
                motivo = real(turnos_dia, ec, data, agora, celulas=celulas)
                destino[pk] = dict(motivo=motivo, agora=agora, **dados)
                return motivo
            except Exception as exc:
                self.erro('app/ponto/turnos.py:2117', (pk,), exc)
                raise
            finally:
                self.c1.pop()

        return embrulho

    def testemunha_turno(self, real, chave):
        @wraps(real)
        def embrulho(*args, **kw):
            valor = real(*args, **kw)
            if self.c1:
                self.c1[-1][chave] = (
                    getattr(valor.entrada, 'pk', None)
                    if chave == 'entrada_turno_aberto' and valor is not None else valor
                )
            return valor

        return embrulho

    def fotos_do_colab(self, cid):
        versao = (self.rodada, self.agora, self.versao_sql)
        memo = self.cache_fotos.get(cid)
        if memo is not None and memo[0] == versao:
            return memo[1]
        c = Colaborador.objects.get(pk=cid)
        # Mesma lista e ordem da porta de lote. app/ponto/management/commands/processar_cartorio.py:64–69
        cels = list(CelulaDia.objects.filter(colaborador=c, data__lt=self.hoje).order_by('data'))
        alvo = {cel.pk: None for cel in cels if cel.pk in self.cels}
        esperadas = set(self.por_colab[cid])
        if set(alvo) != esperadas:
            raise FalhaSonda('celula alvo fora da janela de leitura do comando')
        tl = threading.local()
        tl.agora = self.agora
        tl.colaborador = c
        self.prova, self.prova_turno = alvo, {}
        try:
            with patch.object(cartorio, '_tl', tl):
                # O corpo real calcula ambos os caminhos da impressao antes de
                # completar_status continuar. apply_=False veta a lavra de status.
                # app/ponto/services/cartorio.py:942–959,1264–1317,1399–1409,1541–1558
                CORPO(
                    c, cels, hoje=self.hoje, agora=self.agora, apply_=False, emissor=None,
                    out=CONTADORES(), prefix='', _w=lambda texto: None, forcar=False,
                    EscalaColaborador=EscalaColaborador, Batida=Batida, ChamadoColaborador=CH,
                    eu=eu, ausencia_cobre=turnos.ausencia_cobre, classificar_dia=CLASSIFICAR,
                    completar_status=True,
                )
        finally:
            self.prova, self.prova_turno = None, None
        for cel in cels:
            if cel.pk not in alvo:
                continue
            if alvo[cel.pk] is None:
                raise FalhaSonda('a foto nao chegou a chamada real da impressao')
            alvo[cel.pk].update(
                impressao_gravada=cel.impressao,
                veredito=codigo_publico(cel.veredito, VEREDITOS),
            )
        self.cache_fotos[cid] = (versao, alvo)
        return alvo

    def fotografar(self, ids, etapa, borda):
        fotos = {}
     

[truncado em 20000 de 38478 caracteres]

## 20/11 02:45 · sessao 4d4b82a49aca · E4C-F5-CONSTROI · sonda · origem=Code

**Limite do envelope.** Os dois scripts usam a guarda da sombra e uma transação `REPEATABLE READ, READ ONLY`. O `bin/sombra.sh --rodar` chama `lavra_de_prod` antes de iniciar o Python. Essa etapa lê produção, cria arquivos e grava snapshots na sombra. Se a restrição de leitura se aplica à rodada inteira, o envelope atual impede a execução literal nos termos pedidos. O `echo` final do wrapper também pode mascarar o status do Python. [bin/sombra.sh:136] [bin/sombra.sh:151] [bin/sombra.sh:157] [bin/sombra.sh:483] [app/config/settings/sombra.py:29] [app/config/settings/sombra.py:47]

**Prévia.** A sonda chama `montar_previa` uma vez por empresa de `empresas_visiveis()`. O único valor diário calculado a partir das perguntas é `material`, com os rótulos literais `resposta do colaborador`, `pergunta viva (sem resposta)` e `nada`. Os campos `tipo`, `hora_prevista` e `hora_proposta` vêm da lâmpada, `proposta` é constante e `veredito` vem da célula. A saída agrupa as lâmpadas em uma linha por empresa, colaborador e data, nas duas seções devolvidas. `colaboradores` conta os colaboradores devolvidos nessas seções, inclusive os de `mes_vazio` sem datas. [app/colaboradores/queries.py:78] [app/folha/previa.py:28] [app/folha/previa.py:83] [app/folha/previa.py:97] [app/folha/previa.py:136] [app/folha/previa.py:139]

- **Relógio.** Fixei `hoje=2026-11-28`, depois do maior fim possível da competência 11, dia 27. O script fixa também `15:00 UTC` desse dia. `classificar_export` chega à retenção sem repassar `hoje`, e `fio_vivo` chega à janela viva sem `hoje` e à classificação de falta sem `agora`. Esses caminhos podem ler `localdate()`, `date.today()` e `now()`. [app/ponto/janelas.py:10] [app/folha/previa.py:41] [app/folha/previa.py:79] [app/folha/previa.py:145] [app/folha/export.py:968] [app/folha/export.py:1301] [app/folha/export.py:1238] [app/chamados/juizes.py:597] [app/chamados/juizes.py:1476] [app/chamados/juizes.py:1526] [app/escala/servico_jornada.py:387] [app/requirements.txt:39]
- **Ordem e cache.** A carga direta de células e perguntas usa a ordem dos modelos, com célula única por colaborador e data, e perguntas por `id`. As decisões têm ordem por `ato_em, pk`, e a leitura ordena a sequência de novo. A ordem por nome do export e da prévia admite empates. A sonda ordena a saída por PK e data. Há ainda um empate possível na leitura de rubricas globais por evento, antes da montagem das linhas. A existência desse empate no banco é [nao verificado]. O cache de decisões, o memo de vida e o da tranca têm escopo de instância ou consulta nesse caminho. O catálogo de ausências tem cache de processo com TTL e fallback, e a sonda começa com esse cache vazio. A igualdade do estado da sombra entre as rodadas é [nao verificado]. [app/folha/previa.py:48] [app/folha/previa.py:64] [app/folha/previa.py:124] [app/escala/models.py:1833] [app/chamados/models.py:3785] [app/chamados/models.py:4225] [app/chamados/models.py:4333] [app/chamados/juizes.py:895] [app/chamados/juizes.py:1101] [app/folha/export.py:788] [app/folha/export.py:984] [app/folha/models.py:50] [app/folha/models.py:67] [app/ponto/catalogo/ausencias.py:240] [app/ponto/catalogo/ausencias.py:261] [app/ponto/turnos.py:2609]
- **Falhas.** A API devolve o resultado da empresa inteiro. Se a chamada aborta, a sonda recupera o PK quando a pilha aponta para um laço por colaborador. Falhas de lote saem com `colab=?`. Os demais resultados daquela empresa ficam indisponíveis, e a rodada termina como inconclusiva. A representação do erro usa a classe da exceção, porque a mensagem pode conter dados pessoais. A sonda também conta as falhas registradas pelo mecanismo de best effort. [app/folha/previa.py:43] [app/folha/previa.py:57] [app/folha/previa.py:71] [app/folha/previa.py:136] [app/folha/export.py:495] [app/folha/export.py:817] [app/chamados/juizes.py:1484] [app/core/observ.py:60] [app/core/observ.py:73]

=====SCRIPT f5_previa_11.py=====
```python
import ast
import inspect
import logging
import os
import sys
from datetime import date
from textwrap import dedent

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone


HOJE = date(2026, 11, 28)
AGORA_UTC = "2026-11-28T15:00:00Z"
MATERIAIS = (
    "resposta do colaborador",
    "pergunta viva (sem resposta)",
    "nada",
)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if (banco.get("NAME"), banco.get("HOST")) != ("sombra", "sombra_db"):
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        if cursor.fetchone() != ("sombra", "juliani"):
            raise RuntimeError("conexao")


def faixas_do_laco(funcao, variavel):
    linhas, inicio = inspect.getsourcelines(funcao)
    corpo = ast.parse(dedent("".join(linhas))).body[0].body
    return tuple(
        (inicio + no.body[0].lineno - 1, inicio + no.end_lineno - 1)
        for no in corpo
        if isinstance(no, ast.For)
        and any(
            isinstance(alvo, ast.Name) and alvo.id == variavel
            for alvo in ast.walk(no.target)
        )
    )


def pk_do_erro(exc, escopos):
    tb = exc.__traceback__
    while tb is not None:
        for codigo, variavel, faixas, atributo in escopos:
            if tb.tb_frame.f_code is codigo and any(
                inicio <= tb.tb_lineno <= fim for inicio, fim in faixas
            ):
                dono = tb.tb_frame.f_locals.get(variavel)
                pk = getattr(dono, atributo, None) if atributo else dono
                if type(pk) is int:
                    return pk
        tb = tb.tb_next
    return "?"


def materiais_por_dia(item):
    por_dia = {}
    for detalhe in item["dias"]:
        dia = detalhe["data"].isoformat()
        material = detalhe["material"]
        if material not in MATERIAIS:
            raise ValueError("material fora do contrato")
        if por_dia.setdefault(dia, material) != material:
            raise ValueError("materiais distintos no mesmo dia")
    return sorted(por_dia.items())


def main():
    print(f"HOJE={HOJE.isoformat()} AGORA_UTC={AGORA_UTC}", flush=True)
    etapa = "GUARDA_SOMBRA"
    empresas = colaboradores = linhas = erros = 0
    nivel_anterior = logging.root.manager.disable
    logging.disable(sys.maxsize)

    try:
        with transaction.atomic(durable=True):
            guarda_sombra()
            etapa = "PREPARO"
            from freezegun import freeze_time

            from colaboradores.queries import empresas_visiveis
            from core.observ import engolidos
            from folha.export import classificar_export, dias_ferias_com_batida
            from folha.previa import montar_previa
            from ponto.catalogo.ausencias import invalidar_cache

            # A pilha serve somente para identificar o dono de uma falha.
            escopos = (
                (montar_previa.__code__, "cid",
                 faixas_do_laco(montar_previa, "cid"), None),
                (classificar_export.__code__, "fech",
                 faixas_do_laco(classificar_export, "fech"), "colaborador_id"),
                (dias_ferias_com_batida.__code__, "cid",
                 faixas_do_laco(dias_ferias_com_batida, "cid"), None),
            )

            with freeze_time(AGORA_UTC), timezone.override("America/Sao_Paulo"):
                invalidar_cache()
                etapa = "EMPRESAS"
                visiveis = list(empresas_visiveis().order_by("pk"))
                empresas = len(visiveis)

                etapa = "PREVIA"
                for empresa in visiveis:
                    antes = engolidos()["total"]
                    itens = ()
                    try:
                        with transaction.atomic():
                            dados = montar_previa(empresa, 11, 2026, hoje=HOJE)
                            itens = sorted(
                                dados["linhas"] + dados["mes_vazio"],
                                key=lambda item: item["colaborador_id"],
                            )
                    except Exception as exc:
                        itens = ()
                        erros += 1
                        pk = pk_do_erro(exc, escopos)
                        print(f"ERRO emp={empresa.pk} colab={pk} {type(exc).__name__!r}")

                    falhas = engolidos()["total"] - antes
                    if falhas:
                        erros += falhas
                        resumo = ("best_effort_registrado", falhas)
                        print(f"ERRO emp={empresa.pk} colab=? {resumo!r}")

                    vistos = set()
                    for item in itens:
                        pk = item["colaborador_id"]
                        try:
                            if pk in vistos:
                                raise ValueError("colaborador repetido na previa")
                            vistos.add(pk)
                            colaboradores += 1
                            dias = materiais_por_dia(item)
                        except Exception as exc:
                            erros += 1
                            print(f"ERRO emp={empresa.pk} colab={pk} {type(exc).__name__!r}")
                            continue

                        for dia, material in dias:
                            print(f"emp={empresa.pk} colab={pk} dia={dia} material={material}")
                            linhas += 1

    except Exception as exc:
        erros += 1
        print(f"ERRO etapa={etapa} {type(exc).__name__!r}")
    finally:
        logging.disable(nivel_anterior)

    print(f"UNIVERSO empresas={empresas} colaboradores={colaboradores} linhas={linhas}")
    print(f"ERROS={erros}")
    if erros:
        print("SONDA INCONCLUSIVA")
    return int(erros > 0)


sys.exit(main())
```
=====FIM f5_previa_11.py=====

**Property e display.** Na medida A, o sentido impresso é **ATO → PROJEÇÃO**, como no aval. O lado ATO passa `DecisaoPergunta.leitura(p)` ao juiz real. O lado PROJEÇÃO chama `p.respondida`, que já entrega `DecisaoPergunta.composta(p)` ao mesmo juiz. As duas leituras usam o mesmo instante vigente, inclusive o tratamento do passivo feito pela leitura real. [app/chamados/models.py:3810] [app/chamados/models.py:4320] [app/chamados/models.py:4354] [app/chamados/juizes.py:1551]

Na medida B, `B_DISPLAY_LITERAL_VS_COLUNA` é a comparação literal do aval. A property `resposta_display` pode converter PKs de posto e escala em rótulos e resolver `opcoes` de `valor` para `rotulo`. `B_DISPLAY_COM_MESMA_FORMATACAO` compara os dois textos com a mesma formatação. Para o contrafactual, a sonda chama a property real com a leitura composta fornecida em memória. `B_TEXTO_LEITURA_VS_COLUNA` conta separadamente as diferenças de fonte antes da formatação. As listas têm até 50 PKs para A e até 50 para cada contagem de B. [app/chamados/models.py:2657] [app/chamados/models.py:3837] [app/chamados/models.py:4354]

=====SCRIPT f5_property_e_display.py=====
```python
import logging
import os
import sys
from collections import Counter
from unittest.mock import patch

from django.conf import settings
from django.db import DatabaseError, InterfaceError, connection, transaction


LOTE = 500
LIMITE = 50
MEDIDAS_B = (
    "B_DISPLAY_LITERAL_VS_COLUNA",
    "B_DISPLAY_COM_MESMA_FORMATACAO",
    "B_TEXTO_LEITURA_VS_COLUNA",
)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if (banco.get("NAME"), banco.get("HOST")) != ("sombra", "sombra_db"):
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        if cursor.fetchone() != ("sombra", "juliani"):
            raise RuntimeError("conexao")


def main():
    etapa = "GUARDA_SOMBRA"
    universo = None
    lidas = casos_com_erro = erros = 0
    contagem_a = Counter()
    contagem_b = Counter()
    exemplos_a = []
    exemplos_b = {nome: [] for nome in MEDIDAS_B}
    nivel_anterior = logging.root.manager.disable
    logging.disable(sys.maxsize)

    try:
        with transaction.atomic(durable=True):
            guarda_sombra()
            etapa = "PERGUNTAS"
            from chamados.juizes import pergunta_respondida
            from chamados.models import DecisaoPergunta, PerguntaDisputa

            perguntas = PerguntaDisputa.objects.all().order_by("pk")
            universo = perguntas.count()
            for p in perguntas.prefetch_related("decisoes").iterator(chunk_size=LOTE):
                lidas += 1
                try:
                    leitura_ato = DecisaoPergunta.leitura(p)
                    respondida_ato = pergunta_respondida(leitura_ato)
                    respondida_projecao = p.respondida

                    projecao = p.resposta_colab
                    display_ato = p.resposta_display
                    texto_diverge = leitura_ato.resposta_colab != projecao
                    display_projecao = display_ato
                    if texto_diverge:
                        leitura_projecao = DecisaoPergunta.composta(p)
                        with patch.object(
                            DecisaoPergunta, "leitura", return_value=leitura_projecao
                        ):
                            display_projecao = p.resposta_display

                    diferencas_b = (
                        display_ato != projecao,
                        display_ato != display_projecao,
                        texto_diverge,
                    )
                except Exception as exc:
                    casos_com_erro += 1
                    erros += 1
                    print(f"ERRO pergunta_pk={p.pk} {type(exc).__name__!r}")
                    if isinstance(exc, (DatabaseError, InterfaceError)):
                        break
                    continue

                sentido = (respondida_ato, respondida_projecao)
                contagem_a[sentido] += 1
                if sentido[0] != sentido[1] and len(exemplos_a) < LIMITE:
                    exemplos_a.append((p.pk, sentido))

                for nome, diverge in zip(MEDIDAS_B, diferencas_b):
                    if diverge:
                        contagem_b[nome] += 1
                        if len(exemplos_b[nome]) < LIMITE:
                            exemplos_b[nome].append(p.pk)

    except Exception as exc:
        erros += 1
        print(f"ERRO etapa={etapa} {type(exc).__name__!r}")
    finally:
        logging.disable(nivel_anterior)

    total = universo if universo is not None else "?"
    medidas = sum(contagem_a.values())
    soma = medidas + casos_com_erro
    conferiu = universo is not None and soma == lidas == universo
    if not conferiu and erros == 0:
        erros += 1

    print(f"UNIVERSO perguntas={total} lidas={lidas} medidas={medidas}")
    print(f"CASOS_COM_ERRO={casos_com_erro}")
    print(f"A ATO->PROJECAO True->False={contagem_a[True, False]}")
    print(f"A ATO->PROJECAO False->True={contagem_a[False, True]}")
    for pk, (ato, projecao) in exemplos_a:
        print(f"A_EXEMPLO pergunta_pk={pk} ATO->PROJECAO={ato}->{projecao}")

    for nome in MEDIDAS_B:
        pks = ",".join(str(pk) for pk in exemplos_b[nome])
        print(f"{nome} contagem={contagem_b[nome]} pks=[{pks}]")

    print(f"CONFERENCIA soma==universo={'s' if conferiu else 'n'} soma={soma} universo={total}")
    print(f"ERROS={erros}")
    if erros:
        print("SONDA INCONCLUSIVA")
    return int(erros > 0)


sys.exit(main())
```
=====FIM f5_property_e_display.py=====

Diff MAIN × CÓPIA, universos, contagens e PKs divergentes [nao verificado].

## 20/11 02:44 · sessao fc325d08d437 · - · - · origem=Ronald (tmux)

| Achado | Classificação |
|---|---|
| D1 | **NAO SUSTENTA.** O aval 1830 §7-iv fixa a expectativa de `q_vivas` para uma fixture com texto projetado vazio. A fila e as corrigíveis aplicam a coluna antes de carregar a trilha. Com texto projetado e instante só no ato, o juiz reconhece a resposta e ambas a excluem. A autorização específica dessa divergência e a medida citada de 15/11 estão [nao verificado]. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:488-520` · `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:41-53,66-80` · `app/chamados/juizes.py:1588,1765-1775` |
| D2 | **SUSTENTA.** O aval 1735 §1(b) põe expressamente o placar fora da F5. `values_list` da projeção alimenta a exceção nominal. O registro individual em E4D está [nao verificado]. `app/chamados/services/raiox.py:135-147` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:23-26` · `app/docs/BACKLOG.md:2023` |
| D3 | **NAO SUSTENTA como descarte geral.** Uma reabertura sincronizada limpa a coluna, portanto o exemplo particular é falho nos dois lados. Com a fixture existente de coluna validada em T1 e RECUSA na trilha em T2, para T1 ≤ `agora` < T2 a base encerra no degrau da validação e o HEAD remove T1 ao aplicar a RECUSA futura antes do teto. Sem outra guarda, o HEAD dá viva. A tarefa corta a trilha primeiro. `app/chamados/services/disputa_emissao.py:1617,1639-1656` · `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:105-109` · `eb1e49c1e:app/chamados/juizes.py:1389-1406` · `app/chamados/juizes.py:1408-1411,1487-1488` · `app/chamados/models.py:4302,4339-4351` · `app/chamados/services/tarefa_do_dia.py:398-429` |
| D4 | **NAO SUSTENTA como descarte geral.** A guarda pré-trava existia. Em instância fresca, a base lia a foto do ato de silêncio pós-trava sem o prefetch da primeira lista. Agora, se outro ato respondeu depois da carga e antes da trava, a porta de exceção pode fechar uma pergunta que julgou muda e a foto F2 também usa a sequência antiga. O carimbo não limpa o prefetch. `eb1e49c1e:app/chamados/models.py:2048,2057-2067,2072-2089` · `app/chamados/models.py:2071-2081,2095,2103-2112,3270-3284,3405-3410,3691-3707,4333` |
| D5 | **SUSTENTA** a preexistência e a ausência de consulta. O selo permite nominalmente a referência à tupla de classe. A dispensa formal da pureza literal do aval A está [nao verificado]. `eb1e49c1e:app/chamados/juizes.py:1398` · `app/chamados/juizes.py:1401` · `app/chamados/models.py:2721-2724` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:166-170` |
| D6 | **SUSTENTA.** L-130 exige instante e texto no juiz único. M1 já se declarava composto de perguntas respondidas, mas a base testava somente instante. O RED força a chegada ao M1 sem porta e distingue texto vazio, igualdade com o horizonte e instante futuro. `app/docs/LEIS.md:151` · `eb1e49c1e:app/escala/services/cadastro_realidade.py:500-503` · `app/escala/services/cadastro_realidade.py:497-509` · `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:276-296` |
| D7 | **NAO DA PARA DIZER** sobre o aval do campo JSON. O 1735 manda passar a leitura composta aos juízes. O literal do 1710 e o item 94 da matriz estão [nao verificado]. O campo ISO efetivamente passa de coluna para ato e o teste verifica a saída quando a coluna não tem instante. `eb1e49c1e:app/api/views.py:2581` · `app/api/views.py:2579-2581,2587,2601` · `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:66-80` |
| D8 | **SUSTENTA** o motivo técnico. O memo por disputa vive no render e chama o contador do mesmo juiz. O teto 90 existe, e os testes verificam dois cartões da mesma disputa, disputas com contagens diferentes e novo render pós-POST. A medida 90→92 e o aval específico de C3 estão [nao verificado]. `app/core/tests/test_selo_performance.py:143,377-380` · `app/chamados/templatetags/chamados_tags.py:222-238` · `app/templates/chamados/partials/modal_fio.html:396-401` · `app/chamados/tests/test_e4c_f5_voltaC3_reabrir.py:73-116` |
| D9 | **NAO SUSTENTA.** A seleção usa o recorte estrito no relógio S, antes de a consulta ser avaliada, e julga C6 sem horizonte. A porta só revalida materialização/via para os IDs aceitos após a trava e grava a compensadora no relógio R. Uma VALIDACAO em `(S,R]` pode ser visível no SELECT de decisões da sonda, matar a pergunta no C6 e excluí-la da porta, embora a compensadora em R a limpasse. Isso pode ocorrer com uma gravação no intervalo entre o relógio e o SELECT, sem timestamp futuro ao ler a linha. No empate com R, o PK decide. Os selos vedam construir o ORM ou escrever sua marca fora da porta, e a autoridade já recebe uma vista de leitura em T. `app/chamados/models.py:1555-1561,1581-1595,1632-1638,1651-1674,1702-1722,4159-4169,4193,4225,4302-4303,4333-4351` · `app/chamados/juizes.py:1408-1411` · `app/chamados/tests/test_e1_entradas.py:185-222` · `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:21-44,65-71` · `app/chamados/services/tarefa_do_dia.py:404-429` |
| D10 | **NAO SUSTENTA** a equivalência dos REDs. O antigo compara SQL total de um e cinco dias, com uma pergunta por dia. Um mutante `ancs[:1]` no M1 sobrevive. O aval 1830 §§6b-1 e 7-viii pede casos novos com 3×6, SELECTs de decisões e prova de que todas foram julgadas. `app/escala/services/cadastro_realidade.py:508` · `app/escala/tests/test_f3_beco1_folga_na_lista.py:84-107,378-402` |

## 20/11 02:43 · sessao 562592a28004 · - · - · origem=Ronald (tmux)

**Quesito 4.** Os quatro tetos de SQL são 234, 298, 146 e 221. Cada parcela positiva tem sítio, commit e classe no selo. A parcela `+2` do cartório usa uma só anotação para as duas ocorrências. Os `−2` registram método e commit sem linha nem classe individual. O cabeçalho dá classe geral `LEITURA`, e o aval 0110 item 2 exige anotação individual de cada `+1`. O detalhamento por execução e a medição no C5 ficam **[nao verificado]**. Os seis blobs da cópia correspondem ao hash pedido. O ponto de inserção dos prefetches e do compositor confere nos commits citados. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,248–253,260–267`

| Parcela no selo | Origem conferida |
|---|---|
| `+1` absolvidas, `eaf69fe33` | O prefetch é montado na linha anotada e avaliado pela iteração. SELECT de decisões. `app/chamados/reconciliador.py:702–706` |
| `+1` âncoras no plano e `+1` no S143, `18bf86f01` | O SELECT extra está no prefetch dentro de `list`. As duas linhas de chamada apontam para a mesma fábrica. `app/chamados/models.py:364–366,400` · `app/chamados/services/disputa_emissao.py:1985` |
| `+2` cartório, `eaf69fe33` | O laço avalia o prefetch aninhado. O commit adicionou somente o nível `decisoes` ao relacionamento já prefetcheado `perguntas_que_resolvem`, portanto cada avaliação com perguntas acrescenta um SELECT, por estrutura. As duas ocorrências nos cenários de fecho exigem a pilha da medida **[nao verificado]**. `app/ponto/services/cartorio.py:145–146,1347` |
| `+1` fechar e `−2` compartilhamento, `9d386d1b1` | A carga de perguntas e decisões usa SELECT. O prefetch em `fechar` entrou em `18bf86f01`, e `9d386d1b1` antecipou a carga e passou a lista ao juiz. Isso evita a primeira aquisição da relação em `respostas_sem_veredito` e a busca das decisões pelo prefetch de `sem_veredito_entre`. A própria porta tem escritas distintas em `registrar` e `self.save`; as linhas anotadas são de leitura. `app/chamados/models.py:1998,2018–2021,2071–2072,2110,2122` |
| `+1` universo do fato, `eaf69fe33` | SELECT de decisões no prefetch avaliado por `list`, chamado por `_nascer`. `app/chamados/services/fio.py:1963–1964` · `app/chamados/models.py:3122` |
| `+1` leitura avulsa, `18bf86f01` e `0db6afc73` | O SELECT nasce no `list(p.decisoes.all())` quando o prefetch não existe. O primeiro SHA introduz o compositor e o segundo introduz a chamada dele na guarda. `app/chamados/models.py:4333,4360` · `app/chamados/reconciliador.py:367` |

O selo compara a contagem conjunta de SQL com `<=` e exige cardinalidade exata de registros `on_commit`. A frase “o próximo +1 falha onde nascer” excede a asserção. Um `+1` compensado por `−1` em outro sítio fica verde pela contagem. Uma troca de SELECT por escrita de mesmo custo pode ficar verde. O mutante mínimo compartilhado pelos quatro tetos é um `SELECT 1` antes do retorno de sucesso de `registrar_batida`. Cai se o cenário efetivamente ocupa seu teto, como afirma a anotação para a medida de `aa3bdd88c`. A execução no C5 fica **[nao verificado]**. A fixture `3 chamados FICAM` exige apenas um vivo e pode passar com dois fechados se custo e `on_commit` couberem. As duas fixtures de fecho exigem o número exato. `app/ponto/registro_batida.py:176–181` · `app/ponto/tests/test_selo_chokepoint_escrita.py:319–330,459–475,495–510`

**D9.** A sonda toma `S` antes de avaliar o queryset e seus prefetches. Um ato gravado com `S < ato_em` pode aparecer normalmente nessa própria carga. O C6 recebe a cópia sem `agora=` e aceita a validação retida como vigente. Por exemplo, com as outras condições elegíveis, uma `VALIDACAO` visível com `S < ato_em <= R` tira a candidata da seleção, ao passo que a `REABERTURA` hipotética em `R` teria limpado a validação. Se era a única candidata, o caminho de produção retorna antes da trava e `R` sequer é lido. Para os efetivamente selecionados, a releitura sob trava testa só materialização e via. `ato_em > R` sobrevive ao ato real mesmo se a linha já foi confirmada antes da trava. A confirmação e o carimbo são ordens independentes nas portas que aceitam `instante`. Ocorrência em produção **[nao verificado]**. `app/chamados/models.py:1555–1561,1581–1596,1632–1651,1669–1674,1702–1722,4193,4333–4350` · `app/chamados/juizes.py:1408–1411` · `app/chamados/services/disputa_emissao.py:1662,1686,1692,1708`

## 20/11 02:26 · sessao fbd5107dcd84 · E4C-F5-CONSTROI · verificador · origem=Code

**BLOQUEIA o pouso de E4C-F5-CONSTROI.** A face SQL e o compositor dão respostas diferentes para a mesma pergunta, o C6 aplica o horizonte depois de reduzir a sequência de atos, e o mutante K16 para antes de alterar a função real. Os três defeitos são verificáveis na cópia. `app/chamados/juizes.py:1408,1588,1765,1772` · `app/chamados/models.py:4333,4360` · `app/chamados/tests/e4c_f5_voltaA_mutantes.py:35`

Comparei em leitura as cópias de `eb1e49c1e` e `aa3bdd88c`. Os blobs conferidos de `models.py`, `juizes.py` e `api/views.py` batem com o `prod.diff`. A comparação das cópias reproduz os 96 arquivos do `stat.txt`, incluindo os 50 de teste. A identidade dos commits por metadados Git, a ordem dos commits, os runs de suíte e os efeitos em produção ficam **[nao verificado]**. A contagem final usa os nove quesitos principais.

## 1. Escopo — BLOQUEIA

- **Pureza literal do aval.** O juiz temporal ainda depende do modelo importado no módulo. Trecho exato `if via not in PerguntaDisputa.VIAS_FORA_DO_FATO:`. A catraca abre uma exceção para essa referência. O aval A pede os dois juízes “sem importar modelo”; o aval nominal que liberaria essa exceção está **[nao verificado]**. `app/chamados/juizes.py:11,1401` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:166`
- **Mudanças que precisam de autorização além dos lotes.** O diff troca o critério de M1 de presença do instante para instante **e** texto, passa a devolver o instante da trilha no JSON e cria o memo do modal. Trechos exatos `m1 = all(a is None and respondida_no_horizonte(p) for p, a in ancs)`, `'respondida_em': leitura.respondida_em.isoformat() if leitura.respondida_em else None` e `context.render_context.setdefault(respostas_que_o_reabrir_apaga_no_modal, {})`. O aval A autoriza os pontos da tabela para carga em lote e preserva partição e rota. A tabela nominal e o aval complementar desses três efeitos estão **[nao verificado]**. A autorização do contador de texto tem registro no `RELATO.md:141`. `app/escala/services/cadastro_realidade.py:507` · `app/api/views.py:2581` · `app/chamados/templatetags/chamados_tags.py:234` · `app/docs/RELATO.md:141`
- **Leitores de decisão vigente que escapam da composição.** A exceção de `raiox` aparece na allowlist do teste. O trecho é `pergunta_respondida(SimpleNamespace(respondida_em=p[3], resposta_colab=p[4]))`. Sua autorização literal nos avais fornecidos está **[nao verificado]**. A mesma divergência de fonte fica nas duas faces SQL da resposta, no estoque de resposta vazia usado por `porta_alcancavel_do_dia` e no ramo B do vigia. Os pontos que consomem essas faces para escolher o universo incluem `q_vivas`, corrigíveis, fila de validação, reabertura, calendário, irmã do intervalo e lote de materialização. `app/chamados/services/raiox.py:135,147` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:23` · `app/chamados/juizes.py:1580,1588,1599,1752,1765,1772` · `app/chamados/models.py:1552,3622` · `app/chamados/views.py:1412` · `app/colaboradores/services/calendario.py:505` · `app/chamados/services/materializacao.py:146,2109` · `app/chamados/services/vigia_fio.py:635`
- **Outras leituras diretas com efeito de decisão ou apresentação.** `_slot_esperado` consulta o instante da coluna depois da projeção temporal. `validar_pergunta` e `recusar_pergunta` mantêm a validação da coluna como guarda. A irmã do intervalo e o lote de materialização também usam a coluna. O reconciliador seleciona órfãs pela coluna; S128 faz o mesmo na passagem sem credor. O modal, o cartão do colaborador e o cartão de pergunta exibem carimbos da coluna. A cobertura dessas leituras pela lista fechada dos avais fica **[nao verificado]**. `app/chamados/juizes.py:351,353,365` · `app/chamados/services/validacao.py:685,898` · `app/chamados/services/materializacao.py:200,202,2111,2128` · `app/chamados/reconciliador.py:680,1101` · `app/chamados/services/disputa_emissao.py:2085` · `app/templates/chamados/partials/modal_fio.html:219,228,345` · `app/templates/chamados/partials/_card_atendimento.html:106,143` · `app/templates/chamados/partials/_pergunta_card.html:20,23,32,42`

Entre os leitores diretos da relação `decisoes`, encontrei a seleção simulada da ressuscitação, a foto do autor da validação na ressuscitação real, o histórico de respostas anteriores, o marcador de devolução U103, a carga e a seleção do último ato de T, a foto E4b, o contador e a checagem de atos no backfill. `historico`, `resposta_display`, fotos de silêncio/retratação e o contador de divergência passam por `leitura`. A confirmação nominal de cada exceção contra os K fica **[nao verificado]**. `app/chamados/models.py:1591,1710,2112,2244,3826,3840,4333` · `app/chamados/services/materializacao.py:277` · `app/chamados/services/tarefa_do_dia.py:388,398,429,545` · `app/chamados/services/foto_e4b.py:280` · `app/chamados/services/historico.py:98` · `app/chamados/services/vigia_texto_resposta.py:16` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:126,342` · `app/chamados/management/commands/validar_classe_a.py:63,83`

## 2. Escritores, leitores e significado — BLOQUEIA

| Campo ou valor | Escritores e leitores conferidos |
|---|---|
| `respondida_em`, `resposta_colab` | Resposta humana, automática, recuperação por trilha e reabertura em `app/chamados/services/disputa_emissao.py:1481,1527,1532,1617,1639,1656,1682,1705`. Nascimento, carimbo de fato e limpeza por ressuscitação em `app/chamados/models.py:1721,3229,3350`. Limpeza do estoque L-130 em `app/chamados/services/validacao.py:408,440`. A sequência reduz os atos em `app/chamados/models.py:4333,4350`. As faces SQL, a integridade do par e a ordenação da fila leem colunas em `app/chamados/juizes.py:1538,1580,1588,1599,1774` e `app/chamados/models.py:3716`. |
| `validada_em` | Validação e devoluções em `app/chamados/services/validacao.py:161,168,173,273,279,349,354`. Nova resposta humana limpa a assinatura em `app/chamados/services/disputa_emissao.py:1494`. Carimbo e ressuscitação em `app/chamados/models.py:3354,1721`. Leitores da trilha em `app/chamados/models.py:2025,3260,3571,4345` e `app/chamados/juizes.py:1409`. Leitores da coluna nas guardas de `app/chamados/services/validacao.py:685,898`, `app/chamados/services/materializacao.py:200,2111,2128` e `app/chamados/services/disputa_emissao.py:2085`. |
| `materializada_em`, `via_resolucao` | Carimbo, identidade, descarimbo e limpeza em lote em `app/chamados/models.py:3319,3321,3388,3423,3511,3520`. A relavra tem escrita direta só da via em `app/chamados/services/relavra_via.py:125,127`. A leitura permanece na coluna em `app/chamados/models.py:2025,3262,3623,4362`, `app/chamados/juizes.py:1393`, `app/chamados/services/materializacao.py:263,292,878,891` e `app/chamados/services/historico.py:106`. As vias de identidade podem continuar no horizonte mesmo quando o carimbo futuro é ocultado. `app/chamados/juizes.py:1399` |
| `LeituraComposta` | Um produtor, `return LeituraComposta(lido.validada_em, lido.respondida_em, p.resposta_colab, p.materializada_em, p.via_resolucao)`. Consumidores de estado, vitalidade e retenção em `app/chamados/models.py:2024,3259,3816` e `app/chamados/juizes.py:352,1408`. Consumidores de app e UI em `app/api/views.py:2587,2601,2646`, `app/chamados/views.py:1275`, `app/chamados/views_cobrar.py:170,178`, `app/chamados/services/pergunta_no_app.py:88`, `app/colaboradores/services/ficha.py:173`. Consumidores de escrita e fila em `app/chamados/reconciliador.py:367,1136`, `app/chamados/services/fio.py:189,712,1288,1301`, `app/chamados/services/acoes_disputa.py:134`, `app/chamados/services/becos.py:32`, `app/chamados/services/declaracao_texto.py:418,688`, `app/chamados/services/disputa_emissao.py:1854,2025`, `app/chamados/services/validacao.py:682,902`, `app/chamados/services/materializacao.py:193,261,293,889,1305`, `app/ponto/services/cartorio.py:1689`. Consumidores de auditoria e prévia em `app/chamados/management/commands/dedup_perguntas_fato.py:76`, `app/chamados/management/commands/auditar_invariantes_chamados.py:179,598,1672`, `app/chamados/services/vigia_fio.py:637`, `app/escala/services/cadastro_realidade.py:498`, `app/folha/previa.py:143`. |
| Universo de `fechar` | `perguntas = list(self.perguntas.all().prefetch_related('decisoes'))` alimenta `_pend`, `vivas`, a lista de destinos da trava, o laço que carimba silêncio e os PKs da trilha. O escritor grava perguntas e depois o fechamento da disputa. `app/chamados/models.py:2071,2072,2081,2095,2104,2110,2122,2125` |
| Memo do modal | A tag guarda uma contagem por PK da disputa. A função chamada conta `perguntas_corrigiveis`; confirmação e botão leem o mesmo `n_apaga`. A rota constrói o render em `app/chamados/views.py:927`. `app/chamados/templatetags/chamados_tags.py:208,223,232,237` · `app/templates/chamados/partials/modal_fio.html:396,398,401` |

Três incompatibilidades concretas de significado:

1. **Instante vigente.** `return Q(respondida_em__isnull=False) & Q(resposta_colab__regex=_RE_TEM_TEXTO)` exclui uma resposta com instante só no ato e texto na projeção. A fila de validação e `perguntas_corrigiveis` perdem a linha antes do Python. No inverso, uma `REABERTURA` vigente pode limpar o instante da trilha e o SQL ainda aceitar a coluna velha. Há fixtures das duas divergências. `app/chamados/juizes.py:1588,1765,1772` · `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:43` · `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:534`
2. **Horizonte inclusivo, feito em ordens diferentes.** O C6 executa `_lido = DecisaoPergunta.composta(p)` e só depois corta carimbos `> agora`. Com uma resposta em T1 e uma reabertura em T2, uma consulta entre T1 e T2 já recebe `respondida_em=None`. A carga de T corta os atos com `d.ato_em <= agora` **antes** de chamar `leitura`. O slot ainda mistura as duas fontes em `if respondida is None and p.respondida_em is not None:`. O GET passa `agora` ao C6, mas a partição e o JSON voltam a usar a composição corrente. `app/chamados/juizes.py:353,1394,1408` · `app/chamados/models.py:4342,4350` · `app/chamados/services/tarefa_do_dia.py:400,429,626` · `app/chamados/services/pergunta_no_app.py:77,88` · `app/api/views.py:2552,2581,2587`
3. **Texto e data.** Na mesma resposta JSON, `'resposta_colab': leitura.resposta_colab` vem da projeção e `'resposta_display': p.resposta_display` pode exibir o texto do ato. T também decide o dono usando `texto_vazio(ult.ato_texto)`. Os carimbos são instantes com `USE_TZ=True`; `data_evento` da conversa é o dia civil local, e o da pergunta de fato é o dia do turno. O fallback C6 ainda lê `date.today()` da máquina quando não recebe `hoje`. A ocorrência de divergências de texto ou fuso em produção está **[nao verificado]**. `app/api/views.py:2579` · `app/chamados/models.py:3011,3840` · `app/chamados/services/tarefa_do_dia.py:165` · `app/config/settings/base.py:93,95` · `app/chamados/juizes.py:337,597`

## 3. Cache e prefetch depois da escrita — BLOQUEIA

- **Memo do C6 ficou incompleto.** Trechos exatos `if _m is not None and _m[0] == _k: return _m[1]` e `_cache.pop('decisoes', None)`. A chave cobre os campos da projeção e a célula. Depois da F5, o resultado também depende da trilha. A porta `registrar` invalida o prefetch da instância que recebeu, mas deixa `_pv_memo` intacto e não alcança outras instâncias com o mesmo PK. Um segundo julgamento na mesma instância, com os mesmos campos projetados e um ato que muda a validação ou a resposta vigente, pode devolver o C6 anterior. `app/chamados/juizes.py:856,895,898,1408` · `app/chamados/models.py:4373,4379,4381`
- **`fechar` usa o universo pré-trava até a escrita.** Trecho exato `perguntas = list(self.perguntas.all().prefetch_related('decisoes'))`, seguido de `_pend = self.respostas_sem_veredito(perguntas=perguntas)` e `vivas = [q for q in perguntas if _pv(q)]`. Só depois entram `atomic()` e `trava_na_ordem`. Uma resposta gravada por outro ato entre a carga e a trava fica fora dos dois julgamentos. No ramo que carimba silêncio, a foto do texto volta a ler a sequência da instância que chegou com prefetch. A trilha do fechamento usa intencionalmente `vivas` como foto do antes. `app/chamados/models.py:2071,2081,2090,2095,2106,2112,2125`
- **Resolver e backfill.** S128 materializa ambos os lotes antes de reabrir credores, e o laço seguinte continua sobre as instâncias já carregadas. A passagem `_todas` e o C7 fazem releituras posteriores. O backfill relê o restante quando `res == 'reaberto'`, e o teste força uma escrita por outro objeto antes desse retorno. A interação com todas as cascatas reais e retornos está **[nao verificado]**. `app/chamados/services/disputa_emissao.py:1985,1992,2000,2080,2127` · `app/chamados/management/commands/backfill_fio_morto_e4b.py:137,140,143` · `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:239,244,260`
- **Portas com releitura identificada.** O upload de foto faz `d.refresh_from_db()` depois da validação. A declaração aplicada chama `validar_pergunta` por PK, e o lote de materialização relê a pergunta sob trava. O reabrir do modal executa a porta antes de renderizar a resposta. O memo novo fica em `context.render_context`, com mapa construído no render. Não encontrei compartilhamento por request ou processo nesse caminho. O teste cobre render novo após reabrir, em outro request. A execução desse teste está **[nao verificado]**. `app/api/foto_recebida.py:74,76,81` · `app/chamados/services/declaracao_texto.py:694,697` · `app/chamados/services/validacao.py:677` · `app/chamados/services/materializacao.py:2120,2122` · `app/chamados/views.py:1393,1432` · `app/chamados/templatetags/chamados_tags.py:234` · `app/chamados/tests/test_e4c_f5_voltaC3_reabrir.py:103`

## 4. Classes de reprovação — BLOQUEIA

| Classe | Resultado |
|---|---|
| **a. Segundo juiz** | **BLOQUEIA.** A fila responde a pergunta de C21 pela face que ainda julga `respondida_em` da coluna. O registro permanece único, mas a resposta pode ser dupla. `app/core/juizes.py:305` · `app/chamados/juizes.py:1560,1588,1772` |
| **b. Segundo escritor** | **NAO BLOQUEIA pelo diff.** A sequência continua escrita por `registrar` e o diff acrescenta uma manipulação de cache em memória. A escrita direta de `via_resolucao` da relavra já existia na base. `app/chamados/models.py:1591,1594,4365,4375` · `app/chamados/services/relavra_via.py:125,127` |
| **c. Relógio fora da borda** | **BLOQUEIA.** `_agora_sonda = _tz_s.now()` e `_agora_r = _tz_r.now()` são duas leituras, separadas pela aquisição da trava. A sonda conserva `d.ato_em > _agora_sonda`; o ato que limpa a sequência é gravado com `_agora_r`. Uma linha entre os instantes pode sobreviver na sonda e ser apagada pelo ato. `app/chamados/models.py:1558,1592,1651,1702,1714` |
| **d. Derivação paralela** | **BLOQUEIA.** `_dec._result_cache = [d for d in p.decisoes.all() if d.ato_em > _agora_sonda]` simula uma reabertura recortando a relação. A autoridade ordena por instante e PK e aplica o ato. No empate exato, a sonda descarta todas as linhas; a leitura real ainda distingue o ato de maior PK. `app/chamados/models.py:1592,1714,4335,4339,4349` |
| **e. RED sem mordida** | **BLOQUEIA.** Executei em memória o `tirar_lote` em `_resolver_perguntas_por_chamado_resolvido`. `assert len(alvos) == 1` encontrou **2** e parou antes da mutação. Os sítios reais são os lotes S128 e da varredura sem credor. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:35,60` · `app/chamados/services/disputa_emissao.py:1955,2080` |
| **f. Ordem de trava** | **BLOQUEIA pela validade da foto pré-trava de `fechar`.** A ordem C → Disputa → Chamado → Pergunta continua em `trava_na_ordem`. O diff consolida os dois julgamentos sobre a mesma foto anterior à trava, sem releitura sob ela. `app/chamados/models.py:2071,2090,2095,2876,2885,2898,2902` |
| **g. Dois significados** | **BLOQUEIA.** A tupla `LeituraComposta` é distinta de `LeituraDoAto`; a divergência fica nos consumidores, como os dois textos do mesmo item JSON e a guarda de validação da coluna após a guarda de resposta da trilha. `app/chamados/models.py:19,24,3840,4361` · `app/api/views.py:2579,2580` · `app/chamados/services/validacao.py:682,685` |
| **h. Manager, subconsulta ou anotação** | **NAO BLOQUEIA por manager novo.** O `prefetch_related` é local. A face de validação pré-existente lê `DecisaoPergunta` por `Exists`, inclusive em `_ancoradas`. A face de resposta é a divergência bloqueante do item 1. `app/chamados/models.py:364,3576,3578,3580,3582,4222` |
| **i. Pré-requisito** | **NAO BLOQUEIA por migration nova na cópia.** As migrations 0054 e 0055 estão na base e o deploy tem checagem de pendência antes do reload. A sequência efetiva de rebase com CADEIA-20, migrations, backfill e deploy desta obra está **[nao verificado]**. `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:11,17` · `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12,19` · `bin/deploy.sh:185,217,317,324` |

## 5. Adaptações de teste — BLOQUEIA [nao verificado]

A comparação dos 33 arquivos preexistentes encontrou zero queda na contagem de `assert` e `assert*`. Três módulos aumentaram a contagem, em **76 → 84**, **78 → 79** e **161 → 162**, respectivamente. O selo de igualdade literal do 2050, portanto, não fecha. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:519,532,584,589` · `app/chamados/tests/test_f1nova_b2_leitores.py:97` · `app/chamados/tests/test_o261_vigia_fio.py:831`

| Classe | Arquivos preexistentes compatíveis pelo conteúdo |
|---|---|
| **i — Âncoras** | `app/chamados/tests/e3_mutantes.py:30`, `e3_r3_c6_mutantes.py:49,51`, `e4b_c12_ast.py:19`, `test_cef_c1_sem_banco.py:309`, `test_e4b_classe_e_memoria.py:40,51`, `test_e4c_f6f7_c7_sem_banco.py:113`, `app/core/tests/test_medida_turno_do_par.py:258,271,319`. A nona âncora c12, `462 → 469`, é deslocamento real da chamada e cabe na classe geral (i). `app/chamados/tests/e4b_c12_ast.py:20` · `app/chamados/models.py:469` |
| **ii/v — Dublês** | Em `app/chamados/tests/`: `e3_v8_apoio.py:31,167`, `e3_volta7_apoio.py:33,79,111`, `test_bug_par_c2_sem_banco.py:32,80`, `test_bug_par_sem_banco.py:170,193`, `test_cef_c1_sem_banco.py:88`, `test_disputa_s127.py:23`, `test_e1_v10.py:121`, `test_e3_c8b_sem_banco.py:28`, `test_e3_degrau_dono_e_porta.py:264,272,512`, `test_e3_pos_suite_sem_banco.py:16,54`, `test_e3_r3_c5_sem_banco.py:175`, `test_e3_r3_c6_sem_banco.py:295`, `test_e3_r3_c7_sem_banco.py:181,191`, `test_e3_tarefa_admin_sem_banco.py:87,112,185,202`, `test_e4a2_c5_memoria.py:131`, `test_e4b_c4v7_memoria.py:359,381`, `test_e4b_classe_e_memoria.py:193,286,336`, `test_estado_aplicacao.py:41`, `test_pergunta_viva.py:12`. Também `app/core/tests/test_fio_mudo_placar.py:127` e `app/ponto/tests/test_supra_juiz_executor.py:22`. |
| **iii — Tipo** | `LeituraDoAto → LeituraComposta` cabe em (iii). O `assertIs` adicional altera a contagem do selo. `app/chamados/tests/test_f1nova_b2_leitores.py:95,97` |
| **i com reforço estrutural** | `test_contract_dedup_status.py` troca busca de string por uma atribuição AST com `perguntas=perguntas`, preservando a checagem de chamada a `respostas_sem_veredito`. `app/chamados/tests/test_contract_dedup_status.py:139,142` |

**Mudanças de contrato fora da cobertura literal de i–vi.** `test_cortes_f2_f9_m4.py:58` troca `RESPONDIDA → PENDENTE`. O mutante `_sempre_pendente` agora passa nesse caso; continua mordendo no controle alinhado. `test_e4c_f2_leitura_do_ato.py:501,503,505,507,509,520` troca cinco valores esperados de `True → False`. `test_o261_vigia_fio.py:831,846,876` acrescenta chave, leitura e assert do contador ao carimbo. Os comentários dos testes citam exceções complementares; os respectivos literais estão **[nao verificado]**. `app/chamados/tests/test_cortes_f2_f9_m4.py:54,57,64` · `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:498` · `app/chamados/tests/test_o261_vigia_fio.py:831,846,876`

| Teto ou selo de custo | Antes → depois |
|---|---|
| `_ancoradas`, total da carga | **1 exata → 2 exatas** com 1 e 5 perguntas, sendo uma consulta de decisões por lote. `app/chamados/tests/test_e4c_f2_leit

[truncado em 20000 de 30498 caracteres]

_7466 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
