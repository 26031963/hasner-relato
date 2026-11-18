# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 18:05** · publicado em **18/11/2026 18:05** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 18:05 · sessao 5935b3e9779c · B2B-RETAGUARDA-1811-1810 · sonda · origem=Code

O caminho localizado no vigia é `vigiar_fio_morto`, que abre `tranca_em_lote()` antes dos censos; o censo de alcance usa `alimentar_portas`, e seu caminho para as perguntas do colaborador passa por `_no_app` → `perguntas_do_app` → `pergunta_viva`. A sonda percorre todos os colaboradores com perguntas pelo trecho `_no_app` → `perguntas_do_app` dentro do mesmo lote, inclusive perguntas sem célula; essa ampliação do universo em relação ao censo de alcance é necessária para medir todas as perguntas de disputa. [app/chamados/management/commands/vigiar_fio_morto.py:176; app/chamados/services/vigia_fio.py:560; app/chamados/juizes.py:1667; app/chamados/services/pergunta_no_app.py:45]

- `pergunta_viva` é o juiz C6; `q_vivas()` é somente seu pré-filtro generoso. A sonda chama `perguntas_do_app`, que usa ambos, sem reproduzir o predicado. [app/chamados/juizes.py:866; app/chamados/models.py:3569; app/chamados/services/pergunta_no_app.py:52]
- `_dia_de_turno_da_pergunta` exclui conversas do dia de turno. Para converter o dia em competência, a sonda chama `competencia_do_fato`, usada pela guarda de validação; ela usa a janela da empresa, não necessariamente o mês civil. [app/chamados/juizes.py:338; app/chamados/services/validacao.py:209; app/chamados/services/materializacao.py:788; app/chamados/services/passivo_validado.py:86]
- `competencia_entregue` responde **entregue** e, com `registro=True`, devolve o TXT que vale; `competencia_trancada` responde **trancada**. O TXT mora em `ExportacaoDominio`, por empresa, mês e ano, e seu instante é `gerado_em`. [app/folha/export.py:556; app/ponto/services/fechamento.py:1473; app/folha/models.py:75]
- O instante do ato é `DecisaoPergunta.ato_em`. O modelo não tem campo de porta; `ato_motivo` é texto de trilha, não uma identificação geral confiável da porta. A saída usa `porta=?` para todos os atos. [app/chamados/models.py:4166; app/chamados/models.py:4188]

A janela da medida II é `[agora − 60 dias, agora]`. `II_SEM_TXT` conta **atos**, não dias distintos; atos sem dia de turno contam como erro, pois sua competência não é determinável. Em I, `vivas` conta as vivas com dia por empresa e competência, e `entregue_nao_trancada` é o subconjunto pedido; vivas sem dia aparecem separadamente. [app/chamados/juizes.py:338; não verificado]

Não executei a sonda nem medi os números: o dado fornecido não contém as linhas necessárias. [não verificado] O próprio `--rodar` exige arquivo em `logs/sombra`, escreve saída e chama `lavra_de_prod`; executá-lo nesta sessão contrariaria a regra de não criar ou editar arquivos e a restrição de acesso ao dado fornecido. [bin/sombra.sh:483]

```python
import os
from collections import Counter, defaultdict
from datetime import timedelta
from unittest.mock import patch


def main():
    from django.db import connection, transaction
    from django.utils import timezone

    from chamados.juizes import _dia_de_turno_da_pergunta, tranca_em_lote
    from chamados.models import DecisaoPergunta, PerguntaDisputa
    from chamados.services.materializacao import competencia_do_fato
    from chamados.services.pergunta_no_app import perguntas_do_app
    from colaboradores.models import Colaborador
    from folha.export import competencia_entregue
    from ponto.services.fechamento import competencia_trancada

    erros = Counter()
    grupos_i = defaultdict(lambda: [0, 0])
    grupos_ii = Counter()
    sem_dia_i = 0
    sem_txt_ii = 0
    total_ii = 0
    inicio = None
    agora = None

    # Alguns degraus do C6 registram excecoes e continuam. Contabiliza esses
    # erros sem imprimir mensagens que possam conter dados pessoais.
    def registrar_erro_interno(*args, **kwargs):
        exc = next((x for x in args if isinstance(x, BaseException)), None)
        tipo = type(exc).__name__ if exc is not None else "ErroInterno"
        erros[tipo] += 1

    try:
        if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
            raise RuntimeError("SettingsNaoSombra")

        # A guarda consulta o banco efetivamente conectado, alem dos settings.
        with connection.cursor() as cursor:
            cursor.execute("SELECT current_database(), current_schema()")
            banco, schema = cursor.fetchone()
        if banco != "sombra" or schema != "juliani":
            raise RuntimeError("BancoOuSchemaIncorreto")

        agora = timezone.now()
        inicio = agora - timedelta(days=60)

        # O banco impede escrita nesta transacao; a fotografia tambem mantem
        # as duas medidas coerentes entre si.
        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )

            cache_txt = {}
            cache_tranca = {}

            def txt(emp_id, mes, ano):
                chave = (emp_id, mes, ano)
                if chave not in cache_txt:
                    cache_txt[chave] = competencia_entregue(
                        emp_id, mes, ano, registro=True
                    )
                return cache_txt[chave]

            def trancada(emp_id, mes, ano):
                chave = (emp_id, mes, ano)
                if chave not in cache_tranca:
                    cache_tranca[chave] = competencia_trancada(
                        emp_id, mes, ano
                    )
                return cache_tranca[chave]

            with patch("core.observ.registrar_engolido", registrar_erro_interno):
                # Mesmo lote do vigia e mesmo leitor usado por _no_app.
                # O universo de colaboradores nao depende de ter celula.
                colab_ids = (
                    PerguntaDisputa.objects.order_by()
                    .values_list("disputa__colaborador_id", flat=True)
                    .distinct()
                )
                colabs = (
                    Colaborador.objects.filter(pk__in=colab_ids)
                    .select_related("empresa")
                    .order_by("pk")
                )
                with tranca_em_lote():
                    for colab in colabs.iterator(chunk_size=500):
                        for pergunta in perguntas_do_app(colab, agora=agora):
                            dia = _dia_de_turno_da_pergunta(pergunta)
                            if dia is None:
                                sem_dia_i += 1
                                continue
                            if colab.empresa_id is None:
                                erros["EmpresaAusente"] += 1
                                continue
                            fato = competencia_do_fato(pergunta, colab.empresa)
                            if fato is None:
                                erros["CompetenciaIndeterminada"] += 1
                                continue
                            mes, ano, entregue = fato
                            chave = (colab.empresa_id, ano, mes)
                            grupos_i[chave][0] += 1
                            if entregue and not trancada(colab.empresa_id, mes, ano):
                                grupos_i[chave][1] += 1

                decisoes = (
                    DecisaoPergunta.objects.filter(
                        ato=DecisaoPergunta.RESPOSTA,
                        ato_em__gte=inicio,
                        ato_em__lte=agora,
                    )
                    .select_related(
                        "pergunta",
                        "pergunta__disputa",
                        "pergunta__disputa__colaborador",
                        "pergunta__disputa__colaborador__empresa",
                    )
                    .order_by("pk")
                )
                for decisao in decisoes.iterator(chunk_size=500):
                    pergunta = decisao.pergunta
                    dia = _dia_de_turno_da_pergunta(pergunta)
                    if dia is None:
                        erros["RespostaSemDiaDeTurno"] += 1
                        continue
                    colab = pergunta.disputa.colaborador
                    if colab.empresa_id is None:
                        erros["EmpresaAusente"] += 1
                        continue
                    fato = competencia_do_fato(pergunta, colab.empresa)
                    if fato is None:
                        erros["CompetenciaIndeterminada"] += 1
                        continue
                    mes, ano, _ = fato
                    registro = txt(colab.empresa_id, mes, ano)
                    if registro is None:
                        sem_txt_ii += 1
                    elif decisao.ato_em > registro.gerado_em:
                        grupos_ii[(colab.empresa_id, ano, mes, "?")] += 1
                        total_ii += 1

    except Exception as exc:
        # Nao imprimir str(exc): pode conter dados pessoais ou SQL.
        if isinstance(exc, RuntimeError) and str(exc) in {
            "SettingsNaoSombra", "BancoOuSchemaIncorreto"
        }:
            erros[str(exc)] += 1
        else:
            erros[type(exc).__name__] += 1

    if agora is not None:
        print("INICIO_II=" + timezone.localtime(inicio).isoformat())
        print("AGORA=" + timezone.localtime(agora).isoformat())

    for (emp, ano, mes), (vivas, elegiveis) in sorted(grupos_i.items()):
        print(
            f"I emp={emp} comp={ano:04d}-{mes:02d} "
            f"vivas={vivas} entregue_nao_trancada={elegiveis}"
        )
    print(
        "I_TOTAL vivas={} entregue_nao_trancada={}".format(
            sum(v[0] for v in grupos_i.values()),
            sum(v[1] for v in grupos_i.values()),
        )
    )
    print(f"I_SEM_DIA={sem_dia_i}")

    for (emp, ano, mes, porta), n in sorted(grupos_ii.items()):
        print(f"II emp={emp} comp={ano:04d}-{mes:02d} porta={porta} n={n}")
    print(f"II_TOTAL n={total_ii}")
    print(f"II_SEM_TXT={sem_txt_ii}")

    for tipo, n in sorted(erros.items()):
        print(f"ERRO tipo={tipo} n={n}")
    print(f"ERROS={sum(erros.values())}")
    if erros:
        print("INCONCLUSIVO")
    print("FIM_SONDA_PORTAS_HUMANAS")


main()
```

## 18/11 18:04 · sessao fe4e5fa9bae3 · B2B-RETAGUARDA-1811-1810 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 2:** a composição indicada não cobre toda pergunta em competência entregue; ela só consulta K6 para metades de intervalo. Além disso, a tela atual não usa o estado de bloqueio para esconder Rejeitar ou Conferir data. Isso precisa ser resolvido antes de executar o corte como proteção geral das três portas ([materializacao.py:804](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:804), [modal_fio.html:316](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:316), [modal_fio.html:163](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:163)).

Para a contagem final, **PROCEDE** significa que a pergunta encontrou ao menos uma divergência ou condição que deve voltar para decisão; **NAO PROCEDE** significa que não encontrei essa divergência no material examinado. A contagem é por pergunta, uma vez por item.

A tabela volta 8 e o aval B2B foram recebidos como texto raspado, sem arquivo local verificável nesta cópia; suas linhas de origem são **[nao verificado]**. O diretório fornecido também não contém metadados Git para reconfirmar o HEAD: **[nao verificado]**. As linhas abaixo são as desta cópia de código.

### 1. Premissas, funções e estados

**Item 2 — PROCEDE.** As portas existem, mas a premissa de que a composição indicada barra genericamente perguntas entregues não confere:

- `entrega_que_barra_a_validacao` chama `_k6_que_barra` para a pergunta e eventual irmã ([validacao.py:209](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:209), trecho: `return _k6_que_barra(disputa.colaborador, (perg, irma))`). Porém `k6_consulta_a_pergunta` retorna verdadeiro apenas para `p.motivo in _PAR_DO_INTERVALO`; as demais são puladas ([materializacao.py:804](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:804), [materializacao.py:818](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:818)). Portanto, chamar essa composição não garante bloquear toda pergunta entregue, inclusive o aceite de folga quando seu motivo estiver fora desse conjunto.
- `recusar_pergunta` verifica motivo, existência, validação, resposta e saída do fato; depois grava `recusa_motivo` e reabre para correção ([validacao.py:885](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:885), [validacao.py:898](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:898), trecho: `perg.save(update_fields=['recusa_motivo'])`, seguido de `reabrir_para_correcao` em [validacao.py:922](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:922)).
- `conferir_data_do_fato` exige justificativa e divergência ainda não conferida, então grava `data_conferida_em/por` ([validacao.py:983](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:983), trecho: `p.save(update_fields=['data_conferida_em', 'data_conferida_por'])` em [validacao.py:998](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:998)).
- `validar_pergunta` retorna antecipadamente para `_aceitar_a_folga` antes da consulta comum à composição ([validacao.py:711](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:711), trecho: `return _aceitar_a_folga(perg, autor, request)`; a consulta comum está em [validacao.py:740](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:740)). O aceite já carimba e fecha perguntas no ramo de folga existente, e no outro carimba antes de `declarar_dia` ([validacao.py:511](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:511), [validacao.py:545](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:545)).
- `admin_encerra` exige motivo e delega ao fecho ([models.py:2106](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2106), trecho: `return self.fechar(... motivo=self.FECHOU_ADMIN_ENCERRA)`). O fecho trata esse motivo e desligamento no mesmo ramo de vivas ([models.py:2080](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2080)).
- `responder_pergunta_disputa_core` valida e grava a resposta e a decisão; alguns escapes chamam materialização de ausência ([disputa_emissao.py:1439](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:1439), [disputa_emissao.py:1527](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1560](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:1560)). `declarar_hora_colab` pode reapontar a pergunta e depois chamar esse core ([declaracao_texto.py:371](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/declaracao_texto.py:371), [declaracao_texto.py:420](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/declaracao_texto.py:420)).

**Item 4 — NAO PROCEDE** como achado de premissa falsa: o estado existe e pode ser lido, com alcance condicionado. É um **estado derivado de aplicação**, não uma marca de recusa gravada na pergunta: a testemunha chama a mesma composição e devolve `{'estado': _entregue[0], 'detalhe': _entregue[1]}` ([materializacao.py:892](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:892)). Ela só chega a essa derivação para resposta tipo hora, respondida e não materializada, após os atalhos de recusa ([materializacao.py:875](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:875)).

### 2. Colisão com aval ou lei vigente

**Item 2 — NAO PROCEDE**, quanto a colisão direta demonstrada. O registro vigente de 17/11 deixa as seis portas sem obra “**ate corte do Ronald**”; o item 2 fornecido é apresentado como esse corte posterior ([CORTES.md:556](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.md:556); texto do novo aval: **[nao verificado]**, pois fornecido no enunciado). O mesmo registro confirma para a tranca: “a disputa FECHA” e a pergunta entregue da cadeia S144 “nao e gravada e conta” ([CORTES.md:556](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.md:556)).

A cerca atual é no-op fora de ato cercado: trecho `if estado is None: return` ([fio.py:1402](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/fio.py:1402)). A regra própria sem abrir cerca é compatível com essa distinção. A tabela fornecida registra, contudo, que o alcance de coletor herdado sobre desligamento e cadastro ainda está **a demonstrar**; tratar R4 como provada em todos os contextos seria **[nao verificado]**.

**Item 4 — NAO PROCEDE.** A leitura pedida não altera as portas nem contradiz o registro que as mantém sem obra ([CORTES.md:556](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.md:556)). A L-004 exige parar quando condição literal não fecha ([LEIS.md:62](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/LEIS.md:62)).

### 3. Condições literais, ordem e tela

**Item 2 — PROCEDE**, por três condições que não fecham automaticamente:

1. **Cobertura da composição.** A mesma função de validar tem o filtro de motivo descrito na pergunta 1. Acrescentá-la às três portas, sem mudar seu universo, deixa passar perguntas fora de `_PAR_DO_INTERVALO` mesmo que sua competência esteja entregue ([materializacao.py:804](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:804)).
2. **“Mesmo estado”.** A composição distingue dois fatos: para ausência de dia retorna `BLOQUEADA_SEM_DIA_DO_FATO`; para entregue, `BLOQUEADA_COMPETENCIA_ENTREGUE` ([materializacao.py:821](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:821)). A porta de validar preserva essa distinção: trecho `if barra[0] == BLOQUEADA_SEM_DIA_DO_FATO` ([validacao.py:228](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:228)). O literal “recusam com o MESMO estado bloqueada_competencia_entregue” precisa dizer como tratar o resultado sem dia.
3. **Testemunha e botões.** Hoje o estado é calculado na leitura, não gravado pelo retorno da porta ([materializacao.py:901](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:901)). No card, a condição do estado esconde Validar, mas o formulário Rejeitar vem depois desse bloco ([\_pergunta_card.html:42](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/_pergunta_card.html:42), [\_pergunta_card.html:117](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/_pergunta_card.html:117)). No modal, Rejeitar e Corrigir dependem de `not perg.em_disputa_fechada`, enquanto Conferir data depende de divergência, ausência de conferência e permissão; nenhum desses testes consulta o bloqueio de competência ([modal_fio.html:163](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:163), [modal_fio.html:316](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:316)). Logo, será necessário definir a testemunha e o alcance por botão, inclusive para casos fora do universo atual dela.

A ordem das escritas também importa: em recusar, a guarda precisa anteceder o `save` **e** a reabertura; em conferir, o `save`; em folga, os carimbos e a declaração, inclusive o ramo de folga já existente ([validacao.py:920](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:920), [validacao.py:996](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:996), [validacao.py:511](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:511), [validacao.py:551](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:551)). No fecho administrativo, as vivas são tratadas antes do `save` da disputa, que aciona S144 ([models.py:2080](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2080), [models.py:2099](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2099), [signals.py:150](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/signals.py:150)).

**Item 4 — NAO PROCEDE** quanto à condição “nenhuma ação disponível” como conclusão geral. A resposta depende da tela e dos demais atributos:

- **Card:** para respondida, não validada e `pode_agir`, mostra resposta e detalhe; o bloqueio esconde Validar, mas o formulário Rejeitar permanece ([\_pergunta_card.html:29](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/_pergunta_card.html:29), [\_pergunta_card.html:42](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/_pergunta_card.html:42), [\_pergunta_card.html:117](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/_pergunta_card.html:117)).
- **Modal do fio:** mostra resposta e, nos blocos correspondentes, o detalhe; esconde Validar no estado entregue. Se a disputa não estiver fechada, oferece Rejeitar e, para tipo hora, Corrigir. Se houver divergência não conferida e permissão, oferece Conferir data ([modal_fio.html:217](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:217), [modal_fio.html:274](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:274), [modal_fio.html:316](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:316), [modal_fio.html:163](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:163)).
- **Lista do chamado:** seleciona perguntas vivas de uma disputa aberta; o estado entregue, por si, não é o filtro de inclusão ([acoes_chamado.py:345](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/acoes_chamado.py:345), trecho: `[_p for _p in _todas_perg if _viva(_p)]`).
- **Pauta DP identificada no código:** o contador coloca `pk` e competência no balde de entregue; o comando monta linhas `pk, colNNN, competencia` ([materializacao.py:342](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:342), [bug_par_103.py:143](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/bug_par_103.py:143)). Uma tela operacional adicional dessa pauta e suas ações são **[nao verificado]**.

Há situações em que o modal retira Rejeitar e Corrigir por disputa fechada; se sobra outra saída utilizável em cada caso concreto é **[nao verificado]**. Após o corte proposto para Rejeitar e Conferir, a existência de saída para todo supervisor também é **[nao verificado]**; a condição de espera do item 4 deve ser avaliada por tela e estado, sem presumir que Corrigir resolve todos os casos ([modal_fio.html:316](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/modal_fio.html:316)).

### 4. Exportada, dinheiro ou `!` fora do declarado

**Item 2 — NAO PROCEDE** quanto a toque adicional demonstrado. O objeto declarado é precisamente escrita de pergunta em competência entregue **[nao verificado: aval fornecido no enunciado]**. K6 consulta a existência de TXT vigente por empresa e competência ([export.py:556](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/export.py:556)). O aceite de folga hoje declara dia e pede recálculo de fechamento, portanto tem efeito indireto potencial sobre a folha ([validacao.py:551](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:551), [validacao.py:585](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:585)); efeito monetário concreto e impacto de frota são **[nao verificado]**. A lei distingue apply de dinheiro e exige examinar seu alcance ([LEIS.md:67](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/LEIS.md:67)). Não há execução ou alteração de dado neste trabalho.

**Item 4 — NAO PROCEDE.** O item solicita leitura; não encontrei nele instrução de aplicar alteração de competência ou dinheiro **[nao verificado: aval fornecido no enunciado]**. Valores, pessoas e quantidades em produção são **[nao verificado]**.

REVISAO item 2: PROCEDE 2 / NAO PROCEDE 2.  
REVISAO item 4: PROCEDE 0 / NAO PROCEDE 4.

## 18/11 17:59 · sessao b83f2b7c7015 · RESPOSTAS-1811-1800 · verificador · origem=Code

1. **Premissas — PROCEDE parcialmente; a medida não foi verificável.**

   - O padrão está nomeado: [PLANO-PADROES.md:33](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/PLANO-PADROES.md:33) diz “*projection rebuild* / *read model replay* (CQRS)”; [GLOSSARIO.md:24](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/GLOSSARIO.md:24), “CQRS com projecoes reconstruiveis”.
   - As três entidades têm campos de estado: `status_local` em [models.py:58](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:58), `fechada_em` em [models.py:1894](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:1894) e `validada_em` em [models.py:2636](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2636). Há múltiplos caminhos de gravação: criação e transição do chamado em [models.py:1154](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:1154) e [models.py:234](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:234); fechamento e reabertura da disputa em [models.py:2095](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2095) e [models.py:2312](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2312); carimbo individual e retirada em lote da pergunta em [models.py:3324](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3324) e [models.py:3485](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3485). Isso não constitui censo completo de escritores. [não verificado]
   - `E4D-ATO-DE-CONCLUSAO` consta no [BACKLOG.md:2015](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2015): “choice de ato de conclusao em DecisaoPergunta + escritor unico materializacao.py”. As seis exceções constam em [BACKLOG.md:2095](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2095), [2096](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2096), [2097](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2097), [2098](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2098), [2099](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2099) e [2100](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2100), cada uma com “a projecao segue fonte lida”.
   - “1588 leituras nao cobertas (1336 fora do dono)” aparece no item 3 raspado, mas `logs/PRE-MAPA-E4D/` não está disponível nesta cópia. **A contagem e sua classificação na fonte: [não verificado].** O anexo foi fornecido no texto da pergunta, sem arquivo:linha verificável. [não verificado]

2. **Colisão com aval vigente — PROCEDE.** O item 3 raspado diz que substitui “SO a clausula ‘E4d CONTRACT’”, mas também diz “O selo 2 do aval de 09/11 passa a ser dois” e mantém “E4c”. O anexo de 09/11 situa o selo 2 em **E4c**: “nenhum leitor le campo abolido”; o novo selo (b) diz “nenhum juiz registrado le coluna projetada”, enquanto tela, filtro e relatório podem lê-la. Há, portanto, alteração expressa de uma condição situada fora da cláusula E4d, apesar da delimitação “SO” e de “NAO muda: E4c”. Os literais dos dois anexos vêm do texto fornecido; seus arquivo:linha são [não verificado].

   O pré-mapa raspado também descreve “E4d = PARAR A PROJECAO” e pede uma matriz do que se grava depois. A mudança de regra torna esse contexto desatualizado; o texto fornecido não esclarece uma retificação do pré-mapa. [não verificado: arquivo:linha dos anexos] A ordem da CLASSE-D antes da E4d continua declarada no item 4 raspado e é compatível com [BACKLOG.md:2027](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2027), “CLASSE-D-NA-FRENTE-DA-E4D”.

3. **Condições que não fecham no literal — PROCEDE.**

   - O item 3 exige “Juiz registrado [...] le o ATO [...] nunca a coluna”, mas conserva as seis exceções “com a projecao como fonte lida ate o fato proprio”. O texto não delimita como o selo (b), “nenhum juiz registrado le coluna projetada”, convive com um juiz que precise de uma dessas fontes excepcionadas. [não verificado: arquivo:linha do anexo] Hoje há leitura concreta de coluna por juiz registrado: [core/juizes.py:291](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/core/juizes.py:291) registra `pergunta_viva`; seu cálculo usa `projecao_temporal_da_pergunta` em [chamados/juizes.py:1404](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:1404), que lê `p.validada_em`, `p.respondida_em` e `p.materializada_em` em [chamados/juizes.py:1389](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:1389). Isso prova o estado atual, não que a condição futura seja impossível. [não verificado]
   - “Coluna reconstruida dos atos == coluna gravada, por linha, frota inteira” não define como reconstruir os estados das seis exceções antes de existir o fato próprio. [não verificado: arquivo:linha do anexo] O [BACKLOG.md:2095](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2095) descreve A03 como “reabrir_admin sem DP proprio da reabertura”; para A20, [BACKLOG.md:2100](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2100) diz “fato proprio para conferencia de data”.
   - Há chamadas de registro de decisão em vários sítios, por exemplo [validacao.py:173](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:173) e [disputa_emissao.py:1532](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:1532). Isso exige demonstrar que **cada caminho** chama o único projetor na transação do ato; vários sítios, por si só, não provam impossibilidade. A matriz e a prova transacional futura são [não verificado].

4. **Exportada, dinheiro ou `!` fora do declarado — NAO PROCEDE quanto ao toque autorizado agora.** O item 3 diz “L-092” não muda; o R5/26 diz “o projetor nao regrava linha de competencia entregue sem ! meu”, e o texto proíbe “tocar competencia entregue”. O item 5 condiciona construção a aval próprio. Esses literais são do anexo raspado, sem arquivo:linha verificável. [não verificado] A proteção é relevante também para o ato de conclusão: [BACKLOG.md:2015](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2015) exige “`!` se alcancar exportada”; [LEIS.md:155](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/LEIS.md:155) registra a alteração da L-092 pela L-113 e preserva a versão anterior do TXT e a trilha. Impacto futuro sobre dinheiro ou linhas efetivas de produção: [não verificado].

REVISAO item 3: PROCEDE 3 / NAO PROCEDE 1.

## 18/11 17:55 · sessao 987e32e7bb2f · ESCALA-NAO-DESCREVE · censo · origem=Code

**1. BLOQUEIA — classe (a)**

- Pergunta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`: “selo/RED sem mordida (os testes de escala/tests/test_escala_nao_descreve.py mordem? quais passariam com a cura removida?)” [não verificado: linha do anexo].
- Resposta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.r`: “Os testes R1–R7 exercitam resultados concretos e, por inspeção, removendo suas respectivas curas centrais falhariam”; “mutações e execução dos testes: **[não verificado]**.” [app/escala/tests/test_escala_nao_descreve.py:85](/home/ronald/codex_ws/106920782c9521af2545f0b31fffeec0398eae56/app/escala/tests/test_escala_nao_descreve.py:85).

**2. FILA, LEI-AKITA — classe (a)**

- Pergunta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`: “Qual seria o rotulo honesto de "juizes novos" na linha LEI-AKITA?” [não verificado: linha do anexo].
- Resposta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.r`: “O rótulo honesto é `juizes novos=folga_trabalhada_no_6x1, botao_da_proposta (inclui duplicação da classificação cíclica)`, não `0`.” [app/escala/services/cadastro_realidade.py:788](/home/ronald/codex_ws/106920782c9521af2545f0b31fffeec0398eae56/app/escala/services/cadastro_realidade.py:788).
- Triagem — `logs/ESCALA-NAO-DESCREVE/v2/triagem.md`: “rotulo juizes novos=0: PROCEDENTE -> corrigido no c3 (botao_da_proposta, folga_trabalhada_no_6x1).” [não verificado: linha do anexo].

**3. FILA, segundo juiz cíclico herdado — classe (a)**

- Pergunta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`: “Algum deles duplica uma decisao que ja tem autoridade (core/juizes.py, JUIZES.md, a porta `regua_defesa.realocar_vinculo_para_destino`, `colaboradores/services/vinculo.py::julgar_vinculo`, `acao()`, Plano de Folgas)?” [não verificado: linha do anexo].
- Resposta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.r`: “O botão decide se o destino é cíclico pela fotografia `proposta.ciclo`, enquanto a porta consulta o `TipoEscala` atual”; nenhum trecho identifica a tupla manual como outro juiz. [app/escala/regua_defesa.py:310](/home/ronald/codex_ws/106920782c9521af2545f0b31fffeec0398eae56/app/escala/regua_defesa.py:310), [não verificado: linha do anexo].

**4. FILA, R5 não chega à porta — classe (a)**

- Pergunta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`: “selo/RED sem mordida (os testes de escala/tests/test_escala_nao_descreve.py mordem? quais passariam com a cura removida?)” [não verificado: linha do anexo].
- Resposta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.r`: “Os testes R1–R7 exercitam resultados concretos e, por inspeção, removendo suas respectivas curas centrais falhariam”; “mutações e execução dos testes: **[não verificado]**.” [app/escala/tests/test_escala_nao_descreve.py:194](/home/ronald/codex_ws/106920782c9521af2545f0b31fffeec0398eae56/app/escala/tests/test_escala_nao_descreve.py:194).

**5. FILA, R4 não confere `h.de` — classe (a)**

- Pergunta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`: “Trilha: o `!` pede quem, antes/depois, motivo. Onde cada um e gravado (arquivo:linha) e algum fica em branco?”; “os testes de escala/tests/test_escala_nao_descreve.py mordem? quais passariam com a cura removida?” [não verificado: linha do anexo].
- Resposta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.r`: “`registrado_por` recebe o usuário, `de` e `para` recebem fotografias textuais”; “Os testes R1–R7 exercitam resultados concretos e, por inspeção, removendo suas respectivas curas centrais falhariam”. [app/colaboradores/services/vinculo.py:182](/home/ronald/codex_ws/106920782c9521af2545f0b31fffeec0398eae56/app/colaboradores/services/vinculo.py:182), [app/escala/tests/test_escala_nao_descreve.py:194](/home/ronald/codex_ws/106920782c9521af2545f0b31fffeec0398eae56/app/escala/tests/test_escala_nao_descreve.py:194).

**6. FILA, lavra vencida — sem classe compatível**

- Pergunta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`: “lavra velha (sem as chaves novas), lavra de outro dia: o que acontece em cada um?” [não verificado: linha do anexo].
- Resposta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.r`: “Lavra antiga sem proposta é recusada; lavra de outro dia é aceita sem teste de idade.” [app/ponto/services/esmeril_espelho.py:369](/home/ronald/codex_ws/106920782c9521af2545f0b31fffeec0398eae56/app/ponto/services/esmeril_espelho.py:369).
- Triagem: nenhum descarte desse achado no anexo fornecido. [não verificado: linha do anexo].

**7. FILA, literal do aval — classe (e)**

- Pergunta sobre a divergência literal do aval para `ciclo` ou sobre o destino do link para o wizard: nenhum. [não verificado: linha do anexo].
- Resposta — `logs/ESCALA-NAO-DESCREVE/v2/ver2.r`: “`anomalias_da_frota` escreve o mapa sem `ciclo`; `lista` e a view de aplicação o leem”. [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/4536fe1615991f3d4bc4e75cd3b37f346ed997d3/app/ponto/services/esmeril_espelho.py:241).

**8. FILA, rótulo do selo e citação L-042**

- **Rótulo do selo — classe (a).**
  - Pergunta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`: “O template mudou de `postos` para `linhas`: algum selo de front, contador `data-contador` ou teste que dependia de `data-posto` ou do agrupamento?” [não verificado: linha do anexo].
  - Resposta — `logs/ESCALA-NAO-DESCREVE/v2/ver1.r`: “O selo antigo de agrupamento só procura o nome do posto e continua passando com o posto como coluna”; nenhum trecho identifica a citação do selo em LEIS.md. [app/escala/tests/test_cadastro_x_realidade.py:53](/home/ronald/codex_ws/106920782c9521af2545f0b31fffeec0398eae56/app/escala/tests/test_cadastro_x_realidade.py:53), [não verificado: linha do anexo].
- **Citação L-042 — classe (e).**
  - Pergunta sobre a ausência da citação L-042 no LEI-PROTEGE-SITIO: nenhum. [não verificado: linha do anexo].
  - Resposta sobre essa ausência: nenhum. [não verificado: linha do anexo].

| Achado | Classe | Arquivo |
|---|---|---|
| 1. Mutante R1 sobrevive | (a) | `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`, `ver1.r` |
| 2. LEI-AKITA | (a) | `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`, `ver1.r`, `triagem.md` |
| 3. Juiz cíclico herdado | (a) | `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`, `ver1.r` |
| 4. R5 não chega à porta | (a) | `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`, `ver1.r` |
| 5. R4 não confere `h.de` | (a) | `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`, `ver1.r` |
| 6. Lavra vencida | Sem classe compatível | `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`, `ver1.r`, `triagem.md` |
| 7. Literal do aval | (e) | `logs/ESCALA-NAO-DESCREVE/v2/ver2.r` |
| 8. Rótulo do selo | (a) | `logs/ESCALA-NAO-DESCREVE/v2/ver1.q`, `ver1.r` |
| 8. Citação L-042 | (e) | nenhum |

## 18/11 17:54 · sessao b8ce844dad6a · E4C-F6F7 · construtor · origem=Code

Criei o [teste do caso pela regra](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:16), sem alterar produção. Ele compara os três leitores para V, VR-mantém e VR-não-mantém e verifica duas cargas e ordens de leitura ([teste:68](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:68)). [nao verificado]

Comando e saída do teste sem banco: [nao verificado]

```text
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c13_caso_pela_regra -v

Ran 1 test in 0.109s
OK
rc=0
```

O mutante que troca o `while` de [tarefa_do_dia.py:164](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:164) por `while False:` foi mordido **em memória**: o teste falhou na igualdade VR-mantém == V ([teste:78](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:78)), com `FAILED (failures=1)`, **rc=1**. Para reproduzir: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import unittest
from pathlib import Path
from chamados.tests.e4_catracas import FONTES_MUTADAS

caminho = 'chamados/services/tarefa_do_dia.py'
fonte = Path(caminho).read_text()
original = 'while k and decisoes[k - 1].ato == D.RESPOSTA and decisoes[k - 1].resposta_mantem_validacao:'
assert fonte.count(original) == 1
FONTES_MUTADAS[caminho] = fonte.replace(original, 'while False:')
suite = unittest.defaultTestLoader.loadTestsFromName( [nao verificado]
    'chamados.tests.test_e4c_f6f7_c13_caso_pela_regra')
resultado = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
raise SystemExit(not resultado.wasSuccessful())
PY
```

A execução solicitada sobre uma cópia em `$TMPDIR` ficou **incompleta**: `mktemp` retornou `Read-only file system`, rc=1, antes da cópia. O resultado em memória não substitui essa execução solicitada.

**LEI-AKITA:** origem=n/a (caso pela regra, sem cura); testemunha=tarefa_do_dia/porta_da_tarefa/fio do fora_da_folha; RED=[teste](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:16); quem-mais-lê=[face por pergunta](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:449); juízes novos=0; padrão=n/a (instrumento). [nao verificado]

## 18/11 17:54 · sessao cc604205c6d5 · E4C-F6F7 · censo · origem=Code

1. **BLOQUEIA — classe (a).** Pergunta, `logs/E4C-F6F7/codex/q1.txt` [linha não verificada]: “Para cada campo/funcao listado em quem_escreve_le.txt: QUEM ESCREVE e QUEM LE no HEAD; todos supoem o mesmo significado?”

   Resposta, `logs/E4C-F6F7/codex/r1.txt` [linha não verificada]: “`_pela_decisao` também é chamado dentro da tarefa por dia; a porta publicada é lida, entre outros, por Folha, vigia e beco.” Trecho sobre troca de grupo de retenção ou medida de frota: **nenhum**.

2. **BLOQUEIA — classe (a).** Pergunta, `logs/E4C-F6F7/codex/q1.txt` [linha não verificada]: “Para cada campo/funcao listado em quem_escreve_le.txt: QUEM ESCREVE e QUEM LE no HEAD”.

   Resposta, `logs/E4C-F6F7/codex/r1.txt` [linha não verificada]: “o contador pergunta ao mesmo predicado de correspondência do backfill se falta o ato no mesmo instante.” Trecho sobre ausência de chamador de produção: **nenhum**.

3. **FILA — classe (a).** Pergunta, `logs/E4C-F6F7/codex/q1.txt` [linha não verificada]: “Em _pela_decisao, o laco sobre RESPOSTA com resposta_mantem_validacao: ha sequencia […] em que a escolha do ultimo ato fica errada?”

   Resposta, `logs/E4C-F6F7/codex/r1.txt` [linha não verificada]: “o laço de resposta que mantém validação não cobre interrupções da sequência.” Trecho sobre `SILENCIO/ABSORCAO/RETRATACAO` e o predicado `_resposta_desfaz_validacao`: **nenhum**.

4. **FILA — classe (a).** Pergunta, `logs/E4C-F6F7/codex_c7/construtor_c10.q` [linha não verificada]: “os selos aceitam `perguntas = []` (ou qualquer reatribuicao/mutacao de `perguntas`) entre a leitura de ['pendentes'] e o uso de `perguntas` na resposta”.

   Resposta, `logs/E4C-F6F7/codex_c7/construtor_c10.r` [linha não verificada]: “A detecção de mutação indireta por aliases ou por funções chamadas com `perguntas` é **[não verificado]**”. Trecho com os quatro mutantes sobreviventes: **nenhum**.

5. **FILA — classe (e).** Pergunta: **nenhum** [não verificado]. Resposta: **nenhum** [não verificado].

6. **FILA — classe (c).** Pergunta, `logs/E4C-F6F7/codex/q1.txt` [linha não verificada]: “Os testes (RED c1 + contador) mordem o defeito, ou passam com ele? Aponte teste sem mordida.”

   Resposta, `logs/E4C-F6F7/codex/r1.txt` [linha não verificada]: “O teste de passivo U4 só verifica o caso cujo conservado é `True`: uma implementação que devolvesse sempre `True` para passivo passaria nesse teste.” Trecho sobre o `ERROR` da fixture `DecisaoImutavel`: **nenhum**.

| Achado | Classe | Arquivo |
|---|---|---|
| 1 | (a) | `logs/E4C-F6F7/codex/q1.txt`, `r1.txt` |
| 2 | (a) | `logs/E4C-F6F7/codex/q1.txt`, `r1.txt` |
| 3 | (a) | `logs/E4C-F6F7/codex/q1.txt`, `r1.txt` |
| 4 | (a) | `logs/E4C-F6F7/codex_c7/construtor_c10.q`, `construtor_c10.r` |
| 5 | (e) | nenhum [não verificado] |
| 6 | (c) | `logs/E4C-F6F7/codex/q1.txt`, `r1.txt` |

## 18/11 17:54 · sessao ebd05b3637cc · RAIA-INTERVALO · sonda · origem=Code

A ata fica no campo JSON `CelulaDia.ata`; as chaves são `ata['orfas']` e `ata['lampadas']`. [app/escala/models.py:1778](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1778), [app/ponto/services/cartorio.py:738](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:738)

O vocabulário é `E` para entrada e `S` para saída, tanto para batidas quanto para marcos. [app/ponto/models.py:9](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/models.py:9), [app/escala/models.py:1653](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1653)

A chave é a **hora `HH:MM` de `ata.orfas`, não a identidade da Batida**. A distância usa minutos do dia: `23:55` e `00:05` distam 1.430 minutos, sem circular pela meia-noite. Em empate entre apagadas de tipos diferentes, o tipo esperado e o resultado de `oposto` ficam desconhecidos. O script não foi executado em produção; resultados são [não verificado].

```python
import datetime as dt
import io
import re
from collections import Counter, defaultdict

from django.core.management import call_command
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia, derivar_veredito
from ponto.models import Batida
from ponto.turnos import batidas_apuraveis


G = {
    'G1': [(622, d) for d in (
        '27/10', '29/10', '04/11', '05/11', '12/11', '13/11', '16/11'
    )],
    'G2': [(362, '22/10'), (945, '29/10'), (735, '16/11')],
    'G3': [(583, '12/11'), (829, '11/11'), (369, '12/11')],
    'G4': [
        (224, '28/10'), (469, '01/11'), (924, '11/11'),
        (631, '03/11'), (988, '31/10'), (988, '03/11'),
        (836, '04/11'), (1000, '03/11'), (1000, '13/11'),
        (962, '05/11'), (962, '09/11'), (962, '16/11'),
        (991, '09/11'), (991, '11/11'),
    ],
    'G5': [(787, '30/10')],
}

INI = dt.date(2026, 10, 22)
FIM = dt.date(2026, 11, 16)
tz = timezone.get_current_timezone()
erros = Counter()

# O texto entre o id e a data e o detalhe apos "|" nunca sao impressos.
DUMP_RE = re.compile(
    r'^([A-Z][A-Z0-9_]*)\|col([0-9]+) .*? '
    r'([0-9]{4}-[0-9]{2}-[0-9]{2}) \|'
)
AVALIADOS_RE = re.compile(r'^dias-colab avaliados: ([0-9]+)$')
HORA_RE = re.compile(r'^(?:[01][0-9]|2[0-3]):[0-5][0-9]$')


def executar_etapa(etapa, funcao):
    # O savepoint permite continuar apos erro de banco.
    # Nunca imprime a mensagem da excecao.
    try:
        with transaction.atomic():
            return True, funcao()
    except Exception as exc:
        erros[(etapa, type(exc).__name__)] += 1
        return False, None


def hora_local(valor):
    if valor is None:
        return '?'
    return timezone.localtime(valor, tz).strftime('%d/%m/%Y_%H:%M:%S')


def data_caso(dm):
    dd, mm = dm.split('/')
    return dt.date(2026, int(mm), int(dd))


def carregar_command(empresa):
    buf = io.StringIO()
    call_command(
        'supra_juiz',
        empresa=empresa,
        ini=INI.isoformat(),
        fim=FIM.isoformat(),
        dump=True,
        stdout=buf,
    )

    codigos = defaultdict(list)
    avaliados = None
    no_dump = False

    for linha in buf.getvalue().splitlines():
        if linha == '--- DUMP (HX-DUMP) ---':
            no_dump = True
            continue

        if not no_dump:
            m = AVALIADOS_RE.fullmatch(linha)
            if m:
                if avaliados is not None:
                    raise ValueError('contador_duplicado')
                avaliados = int(m.group(1))
            continue

        m = DUMP_RE.match(linha)
        if not m:
            # Nao aceitar silenciosamente uma linha de dump em formato novo.
            raise ValueError('formato_dump')
        codigo, col_id, iso = m.groups()
        data = dt.date.fromisoformat(iso)
        codigos[(int(col_id), data)].append(codigo)

    if not no_dump or avaliados is None:
        raise ValueError('dump_ou_contador_ausente')

    return codigos, avaliados


def tem_batida_na_janela_command(col):
    inicio = timezone.make_aware(
        dt.datetime.combine(INI - dt.timedelta(days=1), dt.time.min), tz
    )
    fim = timezone.make_aware(
        dt.datetime.combine(FIM + dt.timedelta(days=3), dt.time.min), tz
    )
    return Batida.objects.filter(
        colaborador=col, timestamp__gte=inicio, timestamp__lt=fim
    ).exists()


def batidas_do_dia(col, data):
    inicio = timezone.make_aware(dt.datetime.combine(data, dt.time.min), tz)
    fim = timezone.make_aware(
        dt.datetime.combine(data + dt.timedelta(days=1), dt.time.min), tz
    )
    # A autoridade usa fim inclusivo; excluir a meia-noite seguinte.
    return [
        b for b in batidas_apuraveis(col, inicio, fim)
        if b.timestamp < fim
    ]


def minutos_do_dia(hora):
    if not isinstance(hora, str) or not HORA_RE.fullmatch(hora):
        raise ValueError('hora_invalida')
    return int(hora[:2]) * 60 + int(hora[3:5])


def imprimir_medida_sem_marco(grupo, pk, dm, cel, bats):
    # A chave e a hora HH:MM de ata.orfas, nao a identidade da Batida.
    # Distancias usam minutos do dia, sem circular pela meia-noite.
    ata = cel.ata if cel is not None else None
    if not isinstance(ata, dict) or not ata.get('lampadas'):
        print('B %s col%d %s ata_ausente' % (grupo, pk, dm))
        return None

    lampadas = ata['lampadas']
    orfas = ata.get('orfas')
    if not isinstance(lampadas, list) or not isinstance(orfas, list):
        print('B %s col%d %s ata_invalida' % (grupo, pk, dm))
        return None

    try:
        horas_orfas = [(h, minutos_do_dia(h)) for h in orfas]
        apagadas = []
        for lampada in lampadas:
            if not isinstance(lampada, dict):
                raise ValueError('lampada_invalida')
            if lampada.get('acesa') not in (True, False, None):
                raise ValueError('estado_lampada_invalido')
            if lampada.get('acesa') is False:
                apagadas.append(
                    (minutos_do_dia(lampada.get('hora')), lampada.get('tipo'))
                )
    except (TypeError, ValueError):
        print('B %s col%d %s ata_invalida' % (grupo, pk, dm))
        return None

    por_hora = defaultdict(list)
    for batida in bats:
        hora = timezone.localtime(batida.timestamp, tz).strftime('%H:%M')
        por_hora[hora].append(batida)

    origens_validas = {valor for valor, _ in Batida.ORIGEM_CHOICES}
    total_oposto = 0
    oposto_desconhecido = False
    ambiguas = 0
    sem_batida = 0

    for hora, minuto in horas_orfas:
        candidatas = por_hora.get(hora, [])
        if len(candidatas) >= 2:
            chave = 'ambigua'
            tipo = origem = '?'
            ambiguas += 1
        elif not candidatas:
            chave = 'sem_batida'
            tipo = origem = '?'
            sem_batida += 1
        else:
            chave = 'unica'
            batida = candidatas[0]
            tipo = batida.tipo if batida.tipo in ('E', 'S') else '?'
            origem = (
                batida.origem if batida.origem in origens_validas else '?'
            )

        if apagadas:
            distancia = min(abs(minuto - m) for m, _ in apagadas)
            tipos_proximos = {
                t for m, t in apagadas if abs(minuto - m) == distancia
                and t in ('E', 'S')
            }
            tipos_invalidos = any(
                t not in ('E', 'S')
                for m, t in apagadas if abs(minuto - m) == distancia
            )
            esperado = (
                next(iter(tipos_proximos))
                if len(tipos_proximos) == 1 and not tipos_invalidos
                else '?'
            )
            dist_txt = str(distancia)
        else:
            distancia = None
            dist_txt = '-'
            esperado = '-'

        if tipo in ('E', 'S') and esperado in ('E', 'S'):
            oposto = 'sim' if tipo != esperado else 'nao'
        else:
            oposto = '?'

        if distancia is not None and distancia <= 90:
            if oposto == 'sim':
                total_oposto += 1
            elif oposto == '?':
                oposto_desconhecido = True

        print(
            'B %s col%d %s orfa=%s tipo=%s origem=%s chave=%s '
            'apagadas=%d dist_min=%s tipo_esperado=%s oposto=%s'
            % (
                grupo, pk, dm, hora, tipo, origem, chave,
                len(apagadas), dist_txt, esperado, oposto,
            )
        )

    return (
        len(horas_orfas), total_oposto, ambiguas, sem_batida,
        oposto_desconhecido,
    )


def rodar():
    resumo = {
        grupo: Counter({
            'sim': 0, 'nao': 0, '?': 0,
            'acusa': 0, 'concorde': 0,
            'fora_do_command': 0, 'hoje_?': 0,
            'sem_marco_total': 0,
            'sem_marco_ate90_oposto': 0,
            'ambiguas': 0,
            'sem_batida': 0,
            'medida_desconhecida': 0,
            'oposto_desconhecido': 0,
        })
        for grupo in G
    }

    ids = sorted({pk for casos in G.values() for pk, _ in casos})
    colaboradores = {}
    for pk in ids:
        ok, col = executar_etapa(
            'colaborador', lambda pk=pk: Colaborador.objects.get(pk=pk)
        )
        if ok:
            colaboradores[pk] = col

    empresas = sorted({
        col.empresa_id for col in colaboradores.values()
        if col.empresa_id is not None
    })
    comandos = {}
    for empresa in empresas:
        ok, resultado = executar_etapa(
            'command', lambda empresa=empresa: carregar_command(empresa)
        )
        if ok:
            comandos[empresa] = resultado
            print(
                'COMMAND empresa=%s dias-colab avaliados: %d'
                % (empresa, resultado[1])
            )
        else:
            print('COMMAND empresa=%s dias-colab avaliados: ?' % empresa)

    elegibilidade = {}
    for pk, col in colaboradores.items():
        if col.situacao != 'ativo':
            elegibilidade[pk] = 'inativo'
        else:
            ok, tem = executar_etapa(
                'janela_command',
                lambda col=col: tem_batida_na_janela_command(col)
            )
            if ok:
                elegibilidade[pk] = None if tem else 'sem_batida'
            else:
                elegibilidade[pk] = '?'

    for grupo, casos in G.items():
        for pk, dm in casos:
            data = data_caso(dm)
            col = colaboradores.get(pk)
            cel = None
            grav = julg = ub = None
            hoje = codigos = motivo_hoje = None
            ok_cel = ok_bats = False
            bats = None

            if col is not None:
                ok_cel, cel = executar_etapa(
                    'celula',
                    lambda pk=pk, data=data: CelulaDia.objects.filter(
                        colaborador_id=pk, data=data
                    ).first()
                )
                if ok_cel and cel is not None:
                    grav = cel.veredito
                    julg = cel.julgada_em

                estado = elegibilidade.get(pk, '?')
                if estado in ('inativo', 'sem_batida'):
                    hoje = 'fora_do_command'
                    motivo_hoje = estado
                elif estado is None and col.empresa_id in comandos:
                    codigos = comandos[col.empresa_id][0].get((pk, data), [])
                    if not codigos:
                        hoje = 'concorde'
                    else:
                        ok_ver, hoje = executar_etapa(
                            'veredito_dump',
                            lambda codigos=codigos: derivar_veredito(codigos)
                        )
                        if not ok_ver:
                            hoje = None

                ok_bats, bats = executar_etapa(
                    'batidas_apuraveis',
                    lambda col=col, data=data: batidas_do_dia(col, data)
                )
                if ok_bats and bats:
                    ub = max(
                        bats, key=lambda b: (b.timestamp, b.pk)
                    ).criado_em

            # Ausencia de JULG e ausencia de UB so sao conclusoes se as
            # respectivas leituras terminaram com sucesso.
            motivo_velho = None
            if not ok_cel:
                velho = '?'
                motivo_velho = 'leitura_celula'
            elif julg is None:
                velho = '?'
                motivo_velho = 'sem_julgamento'
            elif not ok_bats:
                velho = '?'
                motivo_velho = 'leitura_batidas'
            elif ub is not None and ub > julg:
                velho = 'sim'
            else:
                velho = '?'
                motivo_velho = 'ata_sem_carimbo'

            r = resumo[grupo]
            r[velho] += 1
            if hoje is None:
                r['hoje_?'] += 1
            elif hoje == 'fora_do_command':
                r['fora_do_command'] += 1
            elif hoje == 'concorde':
                r['concorde'] += 1
            else:
                r['acusa'] += 1

            cod_txt = '?' if codigos is None else (
                ','.join(codigos) if codigos else '-'
            )
            linha = (
                'D %s col%d %s GRAV=%s JULG=%s HOJE=%s COD=%s '
                'UB=%s UA=? VELHO=%s'
                % (
                    grupo, pk, dm, grav or '?', hora_local(julg),
                    hoje or '?', cod_txt, hora_local(ub), velho,
                )
            )
            if motivo_hoje:
                linha += ' motivo_hoje=%s' % motivo_hoje
            if motivo_velho:
                linha += ' motivo_velho=%s' % motivo_velho
            if hoje == 'concorde':
                linha += ' RAMO ?'
            print(linha)

            if not ok_cel:
                r['medida_desconhecida'] += 1
                print('B %s col%d %s leitura_celula=?' % (grupo, pk, dm))
            elif cel is None or not isinstance(cel.ata, dict) or not cel.ata.get('lampadas'):
                r['medida_desconhecida'] += 1
                print('B %s col%d %s ata_ausente' % (grupo, pk, dm))
            elif not ok_bats:
                r['medida_desconhecida'] += 1
                print('B %s col%d %s leitura_batidas=?' % (grupo, pk, dm))
            else:
                ok_medida, medida = executar_etapa(
                    'medida_sem_marco',
                    lambda: imprimir_medida_sem_marco(grupo, pk, dm, cel, bats)
                )
                if not ok_medida:
                    r['medida_desconhecida'] += 1
                    print('B %s col%d %s medida=?' % (grupo, pk, dm))
                elif medida is None:
                    r['medida_desconhecida'] += 1
                else:
                    total, oposto, ambiguas, sem_batida, oposto_incerto = medida
                    r['sem_marco_total'] += total
                    r['sem_marco_ate90_oposto'] += oposto
                    r['ambiguas'] += ambiguas
                    r['sem_batida'] += sem_batida
                    if oposto_incerto:
                        r['oposto_desconhecido'] += 1

    for grupo in G:
        r = resumo[grupo]
        # Um total so e inteiro quando todos os dias do grupo sao conhecidos.
        total = (
            '?' if r['medida_desconhecida']
            else str(r['sem_marco_total'])
        )
        oposto = (
            '?' if r['medida_desconhecida'] or r['oposto_desconhecido']
            else str(r['sem_marco_ate90_oposto'])
        )
        ambiguas = (
            '?' if r['medida_desconhecida'] else str(r['ambiguas'])
        )
        sem_batida = (
            '?' if r['medida_desconhecida'] else str(r['sem_batida'])
        )
        print(
            'RESUMO %s VELHO sim=%d nao=%d ?=%d '
            'HOJE acusa=%d concorde=%d fora_do_command=%d ?=%d '
            'sem_marco_ate90_oposto=%s sem_marco_total=%s '
            'ambiguas=%s sem_batida=%s '
            'medida_dias_desconhecidos=%d'
            % (
                grupo, r['sim'], r['nao'], r['?'],
                r['acusa'], r['concorde'], r['fora_do_command'],
                r['hoje_?'], oposto, total, ambiguas, sem_batida,
                r['medida_desconhecida'],
            )
        )


try:
    if connection.in_atomic_block:
        raise RuntimeError('transacao_preexistente')
    if connection.vendor != 'postgresql':
        raise RuntimeError('banco_nao_postgresql')

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute('SET TRANSACTION READ ONLY')
            cursor.execute('SELECT current_schema()')
            if cursor.fetchone()[0] != 'juliani':
                raise RuntimeError('schema_ativo_nao_e_juliani')
        rodar()
except Exception as exc:
    erros[('sonda', type(exc).__name__)] += 1

print('ERROS %d' % sum(erros.values()))
for (etapa, tipo), quantidade in sorted(erros.items()):
    print('ERROS etapa=%s tipo=%s n=%d' % (etapa, tipo, quantidade))
print('FIM_SONDA_VELHO')
```

## 18/11 17:49 · sessao f3840f73219c · RAIA-INTERVALO · sonda · origem=Code

A ata registra, por marco, `acesa`, `luz` e `tipo_real`, mas a estrutura produzida pelo cartório não registra a identidade da `Batida` casada; `orfas` também contém horas, não IDs. Assim, não é possível identificar com segurança cada batida sem marco sem refazer uma associação por horário. A sonda sinaliza `ata_sem_chave` e mantém os totais correspondentes como desconhecidos quando há batidas apuráveis. [app/ponto/services/cartorio.py:711](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:711), [app/ponto/services/cartorio.py:738](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:738)

A sonda não foi executada e seus resultados em produção são [não verificado]. `UA` permanece desconhecida porque não foi demonstrado um carimbo para toda alteração da ata. [app/ponto/portas/celula.py:1060](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1060), [app/ponto/portas/celula.py:1205](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1205)

```python
import datetime as dt
import io
import re
from collections import Counter, defaultdict

from django.core.management import call_command
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia, derivar_veredito
from ponto.models import Batida
from ponto.turnos import batidas_apuraveis


G = {
    'G1': [(622, d) for d in (
        '27/10', '29/10', '04/11', '05/11', '12/11', '13/11', '16/11'
    )],
    'G2': [(362, '22/10'), (945, '29/10'), (735, '16/11')],
    'G3': [(583, '12/11'), (829, '11/11'), (369, '12/11')],
    'G4': [
        (224, '28/10'), (469, '01/11'), (924, '11/11'),
        (631, '03/11'), (988, '31/10'), (988, '03/11'),
        (836, '04/11'), (1000, '03/11'), (1000, '13/11'),
        (962, '05/11'), (962, '09/11'), (962, '16/11'),
        (991, '09/11'), (991, '11/11'),
    ],
    'G5': [(787, '30/10')],
}

INI = dt.date(2026, 10, 22)
FIM = dt.date(2026, 11, 16)
tz = timezone.get_current_timezone()
erros = Counter()

# O texto entre o id e a data e o detalhe apos "|" nunca sao impressos.
DUMP_RE = re.compile(
    r'^([A-Z][A-Z0-9_]*)\|col([0-9]+) .*? '
    r'([0-9]{4}-[0-9]{2}-[0-9]{2}) \|'
)
AVALIADOS_RE = re.compile(r'^dias-colab avaliados: ([0-9]+)$')


def executar_etapa(etapa, funcao):
    # O savepoint permite continuar apos erro de banco.
    # Nunca imprime a mensagem da excecao.
    try:
        with transaction.atomic():
            return True, funcao()
    except Exception as exc:
        erros[(etapa, type(exc).__name__)] += 1
        return False, None


def hora_local(valor):
    if valor is None:
        return '?'
    return timezone.localtime(valor, tz).strftime('%d/%m/%Y_%H:%M:%S')


def data_caso(dm):
    dd, mm = dm.split('/')
    return dt.date(2026, int(mm), int(dd))


def carregar_command(empresa):
    buf = io.StringIO()
    call_command(
        'supra_juiz',
        empresa=empresa,
        ini=INI.isoformat(),
        fim=FIM.isoformat(),
        dump=True,
        stdout=buf,
    )

    codigos = defaultdict(list)
    avaliados = None
    no_dump = False

    for linha in buf.getvalue().splitlines():
        if linha == '--- DUMP (HX-DUMP) ---':
            no_dump = True
            continue

        if not no_dump:
            m = AVALIADOS_RE.fullmatch(linha)
            if m:
                if avaliados is not None:
                    raise ValueError('contador_duplicado')
                avaliados = int(m.group(1))
            continue

        m = DUMP_RE.match(linha)
        if not m:
            # Nao aceitar silenciosamente uma linha de dump em formato novo.
            raise ValueError('formato_dump')
        codigo, col_id, iso = m.groups()
        data = dt.date.fromisoformat(iso)
        codigos[(int(col_id), data)].append(codigo)

    if not no_dump or avaliados is None:
        raise ValueError('dump_ou_contador_ausente')

    return codigos, avaliados


def tem_batida_na_janela_command(col):
    inicio = timezone.make_aware(
        dt.datetime.combine(INI - dt.timedelta(days=1), dt.time.min), tz
    )
    fim = timezone.make_aware(
        dt.datetime.combine(FIM + dt.timedelta(days=3), dt.time.min), tz
    )
    return Batida.objects.filter(
        colaborador=col, timestamp__gte=inicio, timestamp__lt=fim
    ).exists()


def batidas_do_dia(col, data):
    inicio = timezone.make_aware(dt.datetime.combine(data, dt.time.min), tz)
    fim = timezone.make_aware(
        dt.datetime.combine(data + dt.timedelta(days=1), dt.time.min), tz
    )
    # A autoridade usa fim inclusivo; excluir a meia-noite seguinte.
    return [
        b for b in batidas_apuraveis(col, inicio, fim)
        if b.timestamp < fim
    ]


def imprimir_medida_sem_marco(grupo, pk, dm, bats):
    if not bats:
        # Nao ha batida apuravel que possa estar sem marco.
        return False, False

    # Procurou-se em CelulaDia.ata e no produtor:
    # app/escala/models.py:1778
    # app/ponto/portas/celula.py:88-105
    # app/ponto/services/cartorio.py:599-725 e 730-747
    #
    # lampadas[].luz guarda HH:MM, nao a identidade da Batida;
    # ata.orfas tambem guarda horas. Associar essas horas a objetos
    # Batida seria refazer a associacao, sobretudo com horas duplicadas.
    print('B %s col%d %s ata_sem_chave' % (grupo, pk, dm))
    return None, None


def rodar():
    resumo = {
        grupo: Counter({
            'sim': 0, 'nao': 0, '?': 0,
            'acusa': 0, 'concorde': 0,
            'fora_do_command': 0, 'hoje_?': 0,
            'sem_marco_total': 0,
            'sem_marco_ate90_oposto': 0,
            'medida_desconhecida': 0,
        })
        for grupo in G
    }

    ids = sorted({pk for casos in G.values() for pk, _ in casos})
    colaboradores = {}
    for pk in ids:
        ok, col = executar_etapa(
            'colaborador', lambda pk=pk: Colaborador.objects.get(pk=pk)
        )
        if ok:
            colaboradores[pk] = col

    empresas = sorted({
        col.empresa_id for col in colaboradores.values()
        if col.empresa_id is not None
    })
    comandos = {}
    for empresa in empresas:
        ok, resultado = executar_etapa(
            'command', lambda empresa=empresa: carregar_command(empresa)
        )
        if ok:
            comandos[empresa] = resultado
            print(
                'COMMAND empresa=%s dias-colab avaliados: %d'
                % (empresa, resultado[1])
            )
        else:
            print('COMMAND empresa=%s dias-colab avaliados: ?' % empresa)

    elegibilidade = {}
    for pk, col in colaboradores.items():
        if col.situacao != 'ativo':
            elegibilidade[pk] = 'inativo'
        else:
            ok, tem = executar_etapa(
                'janela_command',
                lambda col=col: tem_batida_na_janela_command(col)
            )
            if ok:
                elegibilidade[pk] = None if tem else 'sem_batida'
            else:
                elegibilidade[pk] = '?'

    for grupo, casos in G.items():
        for pk, dm in casos:
            data = data_caso(dm)
            col = colaboradores.get(pk)
            grav = julg = ub = None
            hoje = codigos = motivo_hoje = None
            ok_cel = ok_bats = False
            bats = None

            if col is not None:
                ok_cel, cel = executar_etapa(
                    'celula',
                    lambda pk=pk, data=data: CelulaDia.objects.filter(
                        colaborador_id=pk, data=data
                    ).first()
                )
                if ok_cel and cel is not None:
                    grav = cel.veredito
                    julg = cel.julgada_em

                estado = elegibilidade.get(pk, '?')
                if estado in ('inativo', 'sem_batida'):
                    hoje = 'fora_do_command'
                    motivo_hoje = estado
                elif estado is None and col.empresa_id in comandos:
                    codigos = comandos[col.empresa_id][0].get((pk, data), [])
                    if not codigos:
                        hoje = 'concorde'
                    else:
                        ok_ver, hoje = executar_etapa(
                            'veredito_dump',
                            lambda codigos=codigos: derivar_veredito(codigos)
                        )
                        if not ok_ver:
                            hoje = None

                ok_bats, bats = executar_etapa(
                    'batidas_apuraveis',
                    lambda col=col, data=data: batidas_do_dia(col, data)
                )
                if ok_bats and bats:
                    ub = max(
                        bats, key=lambda b: (b.timestamp, b.pk)
                    ).criado_em

            # Ausencia de JULG e ausencia de UB so sao conclusoes se as
            # respectivas leituras terminaram com sucesso.
            motivo_velho = None
            if not ok_cel:
                velho = '?'
                motivo_velho = 'leitura_celula'
            elif julg is None:
                velho = '?'
                motivo_velho = 'sem_julgamento'
            elif not ok_bats:
                velho = '?'
                motivo_velho = 'leitura_batidas'
            elif ub is not None and ub > julg:
                velho = 'sim'
            else:
                velho = '?'
                motivo_velho = 'ata_sem_carimbo'

            r = resumo[grupo]
            r[velho] += 1
            if hoje is None:
                r['hoje_?'] += 1
            elif hoje == 'fora_do_command':
                r['fora_do_command'] += 1
            elif hoje == 'concorde':
                r['concorde'] += 1
            else:
                r['acusa'] += 1

            cod_txt = '?' if codigos is None else (
                ','.join(codigos) if codigos else '-'
            )
            linha = (
                'D %s col%d %s GRAV=%s JULG=%s HOJE=%s COD=%s '
                'UB=%s UA=? VELHO=%s'
                % (
                    grupo, pk, dm, grav or '?', hora_local(julg),
                    hoje or '?', cod_txt, hora_local(ub), velho,
                )
            )
            if motivo_hoje:
                linha += ' motivo_hoje=%s' % motivo_hoje
            if motivo_velho:
                linha += ' motivo_velho=%s' % motivo_velho
            if hoje == 'concorde':
                linha += ' RAMO ?'
            print(linha)

            if ok_bats:
                total, oposto = imprimir_medida_sem_marco(
                    grupo, pk, dm, bats
                )
                if total is None or oposto is None:
                    r['medida_desconhecida'] += 1
                else:
                    r['sem_marco_total'] += int(total)
                    r['sem_marco_ate90_oposto'] += int(oposto)
            else:
                r['medida_desconhecida'] += 1
                print('B %s col%d %s leitura_batidas=?' % (grupo, pk, dm))

    for grupo in G:
        r = resumo[grupo]
        # Um total so e inteiro quando todos os dias do grupo sao conhecidos.
        total = (
            '?' if r['medida_desconhecida']
            else str(r['sem_marco_total'])
        )
        oposto = (
            '?' if r['medida_desconhecida']
            else str(r['sem_marco_ate90_oposto'])
        )
        print(
            'RESUMO %s VELHO sim=%d nao=%d ?=%d '
            'HOJE acusa=%d concorde=%d fora_do_command=%d ?=%d '
            'sem_marco_ate90_oposto=%s sem_marco_total=%s '
            'medida_dias_desconhecidos=%d'
            % (
                grupo, r['sim'], r['nao'], r['?'],
                r['acusa'], r['concorde'], r['fora_do_command'],
                r['hoje_?'], oposto, total, r['medida_desconhecida'],
            )
        )


try:
    if connection.in_atomic_block:
        raise RuntimeError('transacao_preexistente')
    if connection.vendor != 'postgresql':
        raise RuntimeError('banco_nao_postgresql')

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute('SET TRANSACTION READ ONLY')
            cursor.execute('SELECT current_schema()')
            if cursor.fetchone()[0] != 'juliani':
                raise RuntimeError('schema_ativo_nao_e_juliani')
        rodar()
except Exception as exc:
    erros[('sonda', type(exc).__name__)] += 1

print('ERROS %d' % sum(erros.values()))
for (etapa, tipo), quantidade in sorted(erros.items()):
    print('ERROS etapa=%s tipo=%s n=%d' % (etapa, tipo, quantidade))
print('FIM_SONDA_VELHO')
```

## 18/11 17:48 · sessao 5fea559ba4a8 · E4C-F6F7 · construtor · origem=Code

A ligação do contador ao vigia está implementada em [vigiar_fio_morto.py](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:186): uma chamada por rodada, com o recorte por empresa usado pela irmã ([vigia_fio.py:770](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/vigia_fio.py:770)). O valor vai à linha impressa ([linha 216](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:216)) e ao censo do carimbo ([linha 262](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:262)), com esperado zero e sem alarme ([linha 101](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:101)).

O [teste novo](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:38) cobre ausência e presença da linha, impressão, esperado, alarme, recorte e chamada única. **RED, GREEN e mutantes não foram executados**, pois usam banco; o resultado de cada rodada permanece [não verificado]. O `py_compile` passou com a escrita de bytecode interceptada; Ruff não está disponível [não verificado em outros ambientes]. [nao verificado]

Comando para o Code executar o RED sobre a base com o teste novo e depois o GREEN com a cura: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 manage.py test chamados.tests.test_f7_vigia_validada_sem_linha
```

Mutantes propostos, cada um aplicado isoladamente a [vigiar_fio_morto.py](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:186), seguidos pelo mesmo comando acima e pela restauração da alteração; morte [não verificada]: [nao verificado]

- Gravar `validada_projetada_sem_linha=0` no censo: o caso sem linha deve falhar ([teste:38](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:38)). [nao verificado]
- Remover o filtro `perguntas.filter(...)`: o caso do recorte deve falhar ([teste:55](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:55)). [nao verificado]
- Chamar o contador duas vezes: a verificação de chamada única deve falhar ([teste:59](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:59)). [nao verificado]

Há uma incompatibilidade previsível para a suíte: o teste existente compara os dicionários completos do carimbo sem a nova chave ([test_o261_vigia_fio.py:835](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_o261_vigia_fio.py:835), [linha 859](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_o261_vigia_fio.py:859)). Não foi alterado por estar fora do escopo literal; falha em execução [não verificado]. [nao verificado]

LEI-AKITA: origem=vigiar_fio_morto.py::handle ([linha 160](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:160)), testemunha=contador_validada_projetada_sem_linha, que lê DecisaoPergunta.linhas_da_projecao_sem_linha ([backfill:94](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/backfill_decisoes_pergunta.py:94)), RED=[teste:38](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:38) [não executado], quem-mais-le=bin/fio_estado.py ([linha 98](/home/ronald/wt-codex-e4c-f6f7-174627/bin/fio_estado.py:98)) e bin/gerar_estado.py ([linha 260](/home/ronald/wt-codex-e4c-f6f7-174627/bin/gerar_estado.py:260)), juizes novos=0, padrao=Single Source of Truth ([GLOSSARIO.md:11](/home/ronald/wt-codex-e4c-f6f7-174627/app/docs/GLOSSARIO.md:11)).

## 18/11 17:47 · sessao 125392a09247 · DECLARADO-X-GRAVADO · censo · origem=Code

1. **Não há `save()` ou `update()` posterior na cauda de `validar_pergunta` que regrave esses campos.** A validação é carimbada antes da materialização; depois da chamada da unidade há uma possível devolução ao admin e, em seguida, a varredura da disputa. Ao voltar da varredura, o código faz `refresh_from_db` de `validada_em` e `materializada_em`, registra logs e retorna. Portanto, nessa cauda, um save posterior não desfaria uma reabertura. `estado` é uma propriedade calculada a partir da sequência de decisões e dos carimbos, não um campo salvo ali. A análise não comprova o efeito de callbacks ou outros caminhos indiretos: **[não verificado]**. [validacao.py:743](app/chamados/services/validacao.py:743), [validacao.py:769](app/chamados/services/validacao.py:769), [validacao.py:786](app/chamados/services/validacao.py:786), [validacao.py:805](app/chamados/services/validacao.py:805), [validacao.py:827](app/chamados/services/validacao.py:827), [models.py:3225](app/chamados/models.py:3225)

2. **`reabrir_para_correcao` aceita pergunta já validada**, desde que esteja no queryset fornecido e não seja linha fora do fato; a própria trilha contempla uma validação anterior. Ela limpa `respondida_em`, `validada_em`, `validada_por` e `resposta_colab`; a porta de lote também limpa `materializada_em`, `materializada_por` e `via_resolucao`. Não grava `estado`, que é calculado. [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py:1617), [disputa_emissao.py:1639](app/chamados/services/disputa_emissao.py:1639), [disputa_emissao.py:1653](app/chamados/services/disputa_emissao.py:1653), [models.py:3448](app/chamados/models.py:3448), [models.py:3225](app/chamados/models.py:3225)

   Para cada linha elegível da qual retira resposta, validação ou carimbo, grava uma `DecisaoPergunta`: por padrão, ato `reabertura`, com a resposta e os carimbos anteriores, instante, autor e `motivo` truncado a 240 caracteres em **`ato_motivo`**. Uma pergunta já vazia não produz linha. A função não copia esse argumento para `PerguntaDisputa.recusa_motivo`; a porta de recusa faz essa escrita separadamente e passa `ato='recusa'`. [disputa_emissao.py:1618](app/chamados/services/disputa_emissao.py:1618), [disputa_emissao.py:1639](app/chamados/services/disputa_emissao.py:1639), [validacao.py:916](app/chamados/services/validacao.py:916), [validacao.py:920](app/chamados/services/validacao.py:920)

3. **O modal do admin lê o motivo da trilha:** `respostas_anteriores` devolve as decisões com foto, e o modal exibe `ato_motivo` quando preenchido. Ele também pode mostrar `recusa_motivo` em bloco de pendência do admin. [models.py:3793](app/chamados/models.py:3793), [modal_fio.html:204](app/templates/chamados/partials/modal_fio.html:204), [modal_fio.html:222](app/templates/chamados/partials/modal_fio.html:222)

   `consulta_do_app` seleciona e particiona perguntas; a API serializa `motivo` **da pergunta**, mas não inclui `ato_motivo` da reabertura nem `recusa_motivo` no item. Assim, essa representação não entrega o texto passado à reabertura. [pergunta_no_app.py:45](app/chamados/services/pergunta_no_app.py:45), [pergunta_no_app.py:68](app/chamados/services/pergunta_no_app.py:68), [views.py:2549](app/api/views.py:2549)

   A tela web do colaborador pode exibir `recusa_motivo` em condições específicas para uma pergunta apresentada no ramo de encerrada/validada; isso **não demonstra** que exiba `ato_motivo` quando a pergunta volta pendente. Na recusa, o push é genérico; a porta também cria uma nota no chamado com o motivo, mas a visualização dessa nota pelo colaborador neste caso está **[não verificado]**. Quanto ao aplicativo instalado e à entrega efetiva do texto: **[não verificado]**. [ _card_atendimento.html:179](app/templates/chamados/partials/_card_atendimento.html:179), [validacao.py:927](app/chamados/services/validacao.py:927), [validacao.py:943](app/chamados/services/validacao.py:943)

4. `recusar_pergunta` devolve ao colaborador uma resposta recusada, mas retorna `ja_validada` antes da reabertura se `validada_em` estiver preenchido; por isso não atende à resposta já validada. [validacao.py:898](app/chamados/services/validacao.py:898), [validacao.py:925](app/chamados/services/validacao.py:925)

   `devolver_validacao_recusada` aceita o caso de resposta já validada recusada pela materialização, mas devolve **ao admin**, preservando a resposta; exige resposta declarada de tipo `hora`, ausência de materialização e motivo de recusa. Não cobre `data_hora` por esse predicado e não devolve a resposta ao colaborador. [validacao.py:185](app/chamados/services/validacao.py:185), [validacao.py:237](app/chamados/services/validacao.py:237), [validacao.py:262](app/chamados/services/validacao.py:262)

   A porta de baixo nível `reabrir_para_correcao` consegue limpar uma pergunta validada, mas, isoladamente, guarda o motivo na trilha e a API examinada não o fornece ao colaborador; a existência de outra porta completa para os dois ramos está **[não verificado]**. [disputa_emissao.py:1584](app/chamados/services/disputa_emissao.py:1584), [views.py:2549](app/api/views.py:2549)

5. **frase literal: nao escrita** nos cinco arquivos indicados, conforme a busca pedida pelos termos e pela identificação da obra. O texto fornecido relata a exigência de voltar ao colaborador “com a frase”, mas não fornece a frase literal. A formulação exigida pela lei permanece **[não verificado]**. [LEIS.md](app/docs/LEIS.md), [CORTES.json](app/docs/CORTES.json), [CORTES.md](app/docs/CORTES.md), [DOSSIES.md](app/docs/DOSSIES.md), [RELATO-ARQUIVO.md](app/docs/RELATO-ARQUIVO.md)

6. A unidade que processa `hora`, `data_hora` e `escolha` é `_materializar_uma`; os chamadores de produção encontrados são:

   - Validação corrente e par: [validacao.py:642](app/chamados/services/validacao.py:642), [validacao.py:775](app/chamados/services/validacao.py:775).
   - Irmã do intervalo já validada: [materializacao.py:1724](app/chamados/services/materializacao.py:1724), [materializacao.py:1744](app/chamados/services/materializacao.py:1744).
   - Varredura da disputa: [materializacao.py:2104](app/chamados/services/materializacao.py:2104), [materializacao.py:2125](app/chamados/services/materializacao.py:2125).
   - Lote do passivo: [passivo_validado.py:529](app/chamados/services/passivo_validado.py:529), [passivo_validado.py:537](app/chamados/services/passivo_validado.py:537).

   A varredura é chamada também pelo fluxo do admin, pela ação de disputa e pelo comando de backfill: [fio.py:718](app/chamados/services/fio.py:718), [acoes_disputa.py:143](app/chamados/services/acoes_disputa.py:143), [backfill_fio_morto_e4b.py:147](app/chamados/management/commands/backfill_fio_morto_e4b.py:147); além da validação, [validacao.py:786](app/chamados/services/validacao.py:786). A varredura filtra `validada_em` preenchido e confere novamente antes da unidade; a irmã puxada pelo materializador é exigida validada. O estado efetivo de todas as perguntas no lote do passivo está **[não verificado]**. [materializacao.py:2104](app/chamados/services/materializacao.py:2104), [materializacao.py:2123](app/chamados/services/materializacao.py:2123), [materializacao.py:1724](app/chamados/services/materializacao.py:1724)

   Há ainda **outra materialização**, `materializar_pendentes_aprovadas`, chamada pelo signal de fechamento da disputa. Seu filtro não exige `validada_em`; ela consulta o juiz e tenta carimbar perguntas aprovadas, sem fabricar batida. Portanto há caminho de materialização de pendente que pode começar sem validação, mas ele é distinto da unidade dos ramos `hora`/`data_hora`. [signals.py:149](app/chamados/signals.py:149), [signals.py:165](app/chamados/signals.py:165), [materializacao.py:2139](app/chamados/services/materializacao.py:2139), [materializacao.py:2167](app/chamados/services/materializacao.py:2167)

A contagem abaixo considera uma questão de lei: o texto literal da frase exigida. [não verificado]

perguntas de lei: 1

## 18/11 17:46 · sessao 45232c15941a · REPROVAS-1811-1740 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 2:** as perguntas **1 e 3 PROCEDEM**. A cópia registra uma ponta F6F7 com suite c10 verde, mas não comprova a c12 nem define o resultado necessário para abrir a F9. [app/docs/RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) [não verificado]

1. **Premissas — PROCEDE.** O brief da F9 foi registrado como refeito contra o main `d6da98509`; o censo originalmente tinha uma pergunta de lei, depois respondida com “perguntas de lei 0”. Os números 24/68 do brief, porém, são registrados como não remedidos. [app/docs/RELATO.md:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:45) [app/docs/RELATO.md:42](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:42) [app/docs/RELATO.md:35](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:35)

   A F9 depende de F1/F2 na ponta `e78031f79`; nessa árvore há registro de **suite c10** verde, e o opus final estava aberto. Isso não comprova a existência, o hash ou o estado verde da ponta **c12** que o item condiciona à abertura. [app/docs/RELATO.md:32](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:32) [app/docs/RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) [app/docs/RELATO.md:4](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:4) [não verificado]

2. **Colisão com o aval vigente — CONFERE, com guardas cumulativas.** A decisão das 16:13 efetivamente manda a F9 nascer sobre a ponta **pousada**; o anexo fornecido declara substituí-la somente nesse ponto. O despacho POUSOS-COMBOIO registra F9 e F6F7 após as 18h; a vedação de ESPURIAS-ABRE proíbe, até o fim da simulação, pousos que toquem folha, export, retenção ou DiaPago. O anexo determina F6F7 após a simulação e F9 depois dela. [app/docs/RELATO.md:32](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:32) [app/docs/RELATO.md:65](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:65) [app/docs/CORTES.json:2019](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:2019) [anexo fornecido; arquivo na cópia não verificado]

   RAIA-INTERVALO registra a vez de DECLARADO-X-GRAVADO e manda medir arquivo comum com raia viva na abertura. O teto geral permite dois construtores **sem arquivo em comum**; além dele, o wrapper recusa uma segunda chamada de **Codex construtor** enquanto a vaga única estiver ocupada. Ocupação e cruzamento na abertura da F9: [não verificado]. [app/docs/RELATO.md:8](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:8) [app/docs/RELATO.md:15](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:15) [app/docs/CORTES.json:1349](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:1349) [bin/codex_pergunta.sh:253](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:253)

3. **Condições no literal — PROCEDE.** “C12 GREEN” não identifica no dado fornecido qual recorte ou suite deve estar verde, nem aponta resultado ou hash da c12. O registro disponível é a suite inteira **c10**, `Ran 16885`, sem `rc` gravado em arquivo. [app/docs/RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) [não verificado]

   “Codex verificador dela sem bloqueio” também não aponta laudo da c12 nem esclarece o tratamento de achados procedentes triados ou pendentes. O registro de verificador com zero bloqueantes é de volta anterior; a recusa do wrapper por B3 parado ou estado ilegível é outra guarda e não prova um laudo sem bloqueantes. [app/docs/RELATO.md:111](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:111) [bin/codex_pergunta.sh:393](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:393) [não verificado]

4. **Exportada, dinheiro ou `!` fora do declarado — CONFERE quanto ao texto da abertura; efeitos da obra [não verificado].** O item fornecido autoriza abertura em cópia e posterga o pouso; não declara apply ou alteração em produção. A conferência histórica da F6F7 registrou zero arquivos de código em folha, export, retenção e DiaPago, mas ela não demonstra o conteúdo da futura c12 nem os efeitos da F9. [app/docs/RELATO.md:43](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:43) [anexo fornecido; arquivo na cópia não verificado] [não verificado]

A cópia disponibilizada não contém metadados Git para confirmar independentemente o hash informado como HEAD. [não verificado]

## 18/11 17:45 · sessao f71e2d7a1389 · - · - · origem=Ronald (tmux)

- O wrapper aplica o limite de **um Codex construtor por vez** por `flock` no repositório vivo; vaga ocupada causa recusa com `rc 2`. Ele também exige censo aprovado antes da vaga e que o `HEAD` da cópia corresponda ao hash informado. [bin/codex_pergunta.sh:241](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:241) [bin/codex_pergunta.sh:248](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:248)

- A evidência registrada é de **suite inteira verde da F6F7 c10**, na árvore `e78031f79`, com `Ran 16885` e sem `rc` gravado; não demonstra uma cura c12 verde. [app/docs/RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) O teste, o hash e o resultado de “c12 GREEN” são [não verificado].

- O papel verificador é de leitura; o wrapper também recusa sua volta se a obra estiver parada pelo B3 ou se o estado/trilha for ilegível. Isso não comprova o resultado de uma revisão da c12 nem define, por si só, um laudo sem bloqueantes. [bin/codex_pergunta.sh:2](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:2) [bin/codex_pergunta.sh:393](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:393)

- A conferência histórica da F6F7 registra zero arquivos de código em folha, exportação, retenção e DiaPago; isso não comprova o conteúdo futuro da c12 nem o efeito da F9. [app/docs/RELATO.md:43](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:43) Efeitos futuros sobre exportada, dinheiro ou `!`: [não verificado].

## 18/11 17:45 · sessao b8d10940a2d4 · - · - · origem=Ronald (tmux)

- A decisão das 16:13 diz que a F9 nasce sobre a ponta **pousada**, depois da F6F7; o item 2 do anexo declara substituir essa decisão somente nesse ponto. A divergência é expressa pelo próprio anexo. [app/docs/RELATO.md:32](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:32); anexo `aval.txt` fornecido pelo usuário: [não verificado em arquivo da cópia].

- A vedação de véspera proíbe pousos que toquem folha, export, retenção ou DiaPago desde 18/11 às 18h até o fim da simulação de 19/11. O item 1 do anexo determina que a F6F7 pouse depois da simulação; o item 2 determina que a F9 pouse depois dela. [app/docs/CORTES.json:2019](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:2019); anexo: [não verificado em arquivo da cópia].

- O despacho de POUSOS-COMBOIO registra ESCALA, F6F7, F9 e EVAL após 18h, com F9 e F6F7 antes; ele não registra exceção à vedação de véspera. [app/docs/RELATO.md:65](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:65); [app/docs/CORTES.json:2019](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:2019).

- A RAIA-INTERVALO registra a vez da **(b) DECLARADO-X-GRAVADO**, seguida pela (a), e determina medir arquivo comum com raia viva na abertura. O item 2 do anexo prevê a F9 como próxima a entrar caso a (b) ocupe a segunda vaga. A disponibilidade e o cruzamento concretos na abertura são [não verificado]. [app/docs/RELATO.md:8](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:8); [app/docs/RELATO.md:15](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:15).

- O teto de dois construtores não elimina, por si só, a condição de não cruzar arquivo: o registro vigente admite dois construtores **sem arquivo em comum**. [app/docs/CORTES.json:1349](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:1349); [app/docs/CORTES.json:1350](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:1350).

## 18/11 17:45 · sessao ea0e7d131448 · - · - · origem=Ronald (tmux)

A cópia registra o brief da F9 refeito contra o main e a resposta à pergunta de lei, com “perguntas de lei 0”, em [RELATO.md:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:45) e [RELATO.md:42](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:42). A revisão registra que os números 24/68 do brief não foram remedidos em [RELATO.md:35](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:35).

A ponta `e78031f79` da F6F7 tem suite **c10** verde registrada, e a F9 depende de F1/F2 nessa ponta, conforme [RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) e [RELATO.md:32](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:32). Uma ponta **c12 GREEN** e o resultado do verificador dessa c12: **[não verificado]**. O registro de verificador sem bloqueante em [RELATO.md:111](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:111) refere-se a uma volta anterior, não à c12.

## 18/11 17:39 · sessao 8dcf99a94415 · RAIA-INTERVALO · leitura · origem=Code

A sonda abaixo **não foi executada**. A ata está em `CelulaDia.ata`, mas não encontrei um carimbo que registre todas as alterações dela: `veredito_em` não avança quando a ata muda sob decisão administrativa, e `carimbar_precedencia` também altera a ata sem atualizar esse campo. Por isso, a sonda imprime `UA=?` e, pela regra solicitada para dado faltante, `VELHO=?`; não seria correto usar `editada_em`, que carimba edição do dia/DNA ([app/escala/models.py:1732](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1732), [app/escala/models.py:1778](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1778), [app/ponto/portas/celula.py:1060](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1060), [app/ponto/portas/celula.py:1205](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1205)).

`HOJE` é o veredito derivado dos códigos devolvidos pela chamada do **command**, não uma previsão de escrita do cartório: este último pode preservar uma decisão administrativa e tem outros insumos próprios ([app/escala/models.py:1624](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1624), [app/ponto/portas/celula.py:970](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:970), [app/ponto/management/commands/supra_juiz.py:239](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:239)). O leitor da grade lê a ata lavrada; portanto, `HOJE` não prova uma reconstrução independente da ata ([app/escala/services/leitor_celula.py:350](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:350), [app/escala/services/leitor_celula.py:377](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:377)).

A classificação é declarada pura; a transação PostgreSQL `READ ONLY` protege também os carregadores usados pela sonda, recusando escritas ([app/ponto/supra_juiz.py:9](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:9)). A sonda exige o schema ativo `juliani`; o uso em qualquer outro ambiente fica recusado [não verificado].

```python
import datetime as dt
from collections import Counter

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia, EscalaColaborador, derivar_veredito
from escala.services.leitor_celula import grade_da_celula
from ponto.models import Batida
from ponto.turnos import ausencia_cobre, batidas_apuraveis, dias_encerrados
from ponto.supra_juiz import cadastro_do_dia, classificar_dia
from chamados.models import ChamadoColaborador
from chamados.juizes import fio_vivo
from chamados.catalogo.furo_modulos import FURO_MODULES
from chamados.catalogo.modulos import data_do_chamado
from chamados.catalogo.motor import VIVOS

G = {'G1': [(622, d) for d in ('27/10', '29/10', '04/11', '05/11', '12/11', '13/11', '16/11')],
     'G2': [(362, '22/10'), (945, '29/10'), (735, '16/11')],
     'G3': [(583, '12/11'), (829, '11/11'), (369, '12/11')],
     'G4': [(224, '28/10'), (469, '01/11'), (924, '11/11'), (631, '03/11'), (988, '31/10'), (988, '03/11'),
            (836, '04/11'), (1000, '03/11'), (1000, '13/11'), (962, '05/11'), (962, '09/11'), (962, '16/11'),
            (991, '09/11'), (991, '11/11')],
     'G5': [(787, '30/10')]}

erros = Counter()
tz = timezone.get_current_timezone()


def executar_etapa(etapa, funcao):
    # Savepoint permite continuar mesmo apos erro de banco; nao imprime
    # mensagens de excecao, que poderiam conter dados pessoais.
    try:
        with transaction.atomic():
            return True, funcao()
    except Exception as exc:
        erros[(etapa, type(exc).__name__)] += 1
        return False, None


def hora_local(valor):
    if valor is None:
        return '?'
    return timezone.localtime(valor, tz).strftime('%d/%m/%Y_%H:%M:%S')


def classificar_como_command(c, data):
    # Mesmo caminho e argumentos de:
    # ponto/management/commands/supra_juiz.py:102-116,120-156,173-178,
    # 186-202 e 205-234. A janela pedida aqui e um unico dia.
    ini = fim = data
    janela_ini = timezone.make_aware(
        dt.datetime.combine(ini - dt.timedelta(days=1), dt.time.min)
    )
    janela_fim = timezone.make_aware(
        dt.datetime.combine(fim + dt.timedelta(days=3), dt.time.min)
    )
    bs = list(Batida.objects.filter(
        colaborador=c,
        timestamp__gte=janela_ini,
        timestamp__lt=janela_fim,
    ).order_by('timestamp'))
    if not bs:
        # O command pula este colaborador nesta janela.
        raise ValueError('command_sem_batidas_na_janela')

    encerrados = dias_encerrados(c, ini, fim)
    grade = grade_da_celula(c, ini, fim)
    primeira = Batida.objects.filter(
        colaborador=c
    ).order_by('timestamp').first()
    adesao = timezone.localdate(primeira.timestamp) if primeira else None

    vigs = list(EscalaColaborador.objects.filter(
        colaborador=c
    ).select_related('tipo_escala').order_by('data_inicio'))

    def sem_fase(dref):
        ec = None
        for v in vigs:
            if v.data_inicio and v.data_inicio <= dref:
                ec = v
        if ec is None or ec.tipo_escala is None:
            return False
        if ec.tipo_escala.tipo_ciclo not in ('12x36', '24x48'):
            return False
        a = ec.data_ancora_colaborador
        return a is None or a.year <= 1940

    cadastro = cadastro_do_dia(
        c.pk,
        list(EscalaColaborador.objects.filter(
            colaborador_id=c.pk
        ).select_related('tipo_escala')),
        {(c.pk, x.data): x for x in CelulaDia.objects.filter(
            colaborador_id=c.pk, data__range=(ini, fim)
        )},
        data_demissao=c.data_demissao,
    )
    regua_deg = ChamadoColaborador.objects.filter(
        colaborador=c,
        modulo_origem='regua_degenerada',
        status_local__in=VIVOS,
    ).exists()

    chs = {}
    for ch in (ChamadoColaborador.objects.filter(
            colaborador=c, modulo_origem__in=FURO_MODULES
    ).prefetch_related('perguntas_que_resolvem')):
        d = data_do_chamado(ch.modulo_origem, ch.contexto_json)
        if d:
            chs.setdefault(str(d), []).append(ch)

    dias = [
        x for x in (grade.get('dias') or [])
        if x.get('data') == data and ini <= x.get('data') <= fim
    ]
    if len(dias) != 1:
        raise ValueError('grade_nao_devolveu_um_dia')

    dia = dias[0]
    lista = chs.get(data.isoformat(), [])
    vivos = [x for x in lista if x.status_local in VIVOS]
    com_fio = [
        x for x in vivos if fio_vivo(x.perguntas_que_resolvem.all())
    ]
    cob = ausencia_cobre(c, data, para='cobranca')
    n_bat = sum(1 for b in bs if timezone.localdate(b.timestamp) == data)

    resultado = classificar_dia(
        dia, dia.get('marcos') or [],
        chamados_vivos=len(vivos),
        chamados_total=len(lista),
        cobrancas_vivas=len(com_fio),
        cobertura_status=getattr(cob, 'status', None),
        data=data,
        em_operacao_desde=adesao,
        isento_ponto=bool(c.isento_ponto),
        batidas_no_dia=n_bat,
        hoje=timezone.localdate(),
        turno_encerrado=encerrados.get(data, False),
        sem_fase=sem_fase(data),
        regua_degenerada=regua_deg,
        **cadastro(data)
    )
    codigos = [cod for cod, detalhe in resultado]
    veredito = derivar_veredito(codigos)

    # Os dois retornos de classificar_dia sao o retorno antecipado de
    # isento e o retorno final. Nao se afirma qual predicado interno
    # deixou o resultado vazio.
    ramo = None
    if veredito == 'concorde':
        ramo = 'ISENTO_PONTO' if bool(c.isento_ponto) else 'RETORNO_FINAL'
    return veredito, codigos, ramo


def ultima_batida(c, data):
    ini = timezone.make_aware(dt.datetime.combine(data, dt.time.min), tz)
    fim = timezone.make_aware(
        dt.datetime.combine(data + dt.timedelta(days=1), dt.time.min), tz
    )
    # batidas_apuraveis usa fim inclusivo; exclui-se a meia-noite seguinte.
    bats = [b for b in batidas_apuraveis(c, ini, fim) if b.timestamp < fim]
    if not bats:
        return None
    # "Ultima batida" pela ordem de timestamp da autoridade;
    # pk desempata marcas com o mesmo timestamp.
    return max(bats, key=lambda b: (b.timestamp, b.pk)).criado_em


def rodar():
    resumo = {
        grupo: Counter({'sim': 0, 'nao': 0, '?': 0,
                        'acusa': 0, 'concorde': 0, 'hoje_?': 0})
        for grupo in G
    }

    for grupo, casos in G.items():
        for pk, dm in casos:
            dd, mm = dm.split('/')
            data = dt.date(2026, int(mm), int(dd))
            grav = julg = hoje = codigos = ub = ramo = None

            ok_col, col = executar_etapa(
                'colaborador', lambda: Colaborador.objects.get(pk=pk)
            )
            if ok_col:
                ok_cel, cel = executar_etapa(
                    'celula',
                    lambda: CelulaDia.objects.filter(
                        colaborador_id=pk, data=data
                    ).first()
                )
                if ok_cel and cel is not None:
                    grav = cel.veredito
                    julg = cel.julgada_em

                ok_hoje, atual = executar_etapa(
                    'classificacao', lambda: classificar_como_command(col, data)
                )
                if ok_hoje:
                    hoje, codigos, ramo = atual

                ok_ub, ub = executar_etapa(
                    'batida', lambda: ultima_batida(col, data)
                )

            # Nao ha carimbo demonstrado para todas as alteracoes de ata.
            ua = None
            velho = '?'

            r = resumo[grupo]
            r[velho] += 1
            if hoje is None:
                r['hoje_?'] += 1
            elif hoje == 'concorde':
                r['concorde'] += 1
            else:
                r['acusa'] += 1

            cod_txt = '?' if codigos is None else (
                ','.join(codigos) if codigos else '-'
            )
            linha = (
                'D %s col%d %s GRAV=%s JULG=%s HOJE=%s COD=%s '
                'UB=%s UA=%s VELHO=%s'
                % (grupo, pk, dm, grav or '?', hora_local(julg),
                   hoje or '?', cod_txt, hora_local(ub),
                   hora_local(ua), velho)
            )
            if hoje == 'concorde':
                linha += ' RAMO %s' % (ramo or '?')
            print(linha)

    for grupo in G:
        r = resumo[grupo]
        print(
            'RESUMO %s VELHO sim=%d nao=%d ?=%d '
            'HOJE acusa=%d concorde=%d ?=%d'
            % (grupo, r['sim'], r['nao'], r['?'],
               r['acusa'], r['concorde'], r['hoje_?'])
        )


try:
    if connection.in_atomic_block:
        raise RuntimeError('transacao_preexistente')
    if connection.vendor != 'postgresql':
        raise RuntimeError('banco_nao_postgresql')

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute('SET TRANSACTION READ ONLY')
            cursor.execute('SELECT current_schema()')
            if cursor.fetchone()[0] != 'juliani':
                raise RuntimeError('schema_ativo_nao_e_juliani')
        rodar()
except Exception as exc:
    erros[('sonda', type(exc).__name__)] += 1

total_erros = sum(erros.values())
print('ERROS %d' % total_erros)
for (etapa, tipo), quantidade in sorted(erros.items()):
    print('ERROS etapa=%s tipo=%s n=%d' % (etapa, tipo, quantidade))
print('FIM_SONDA_VELHO')
```

As referências principais da sonda são:

- `CelulaDia.veredito`, `veredito_via`, `veredito_em`, `protestos` e `ata`: [app/escala/models.py:1769](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1769). `julgada_em`: [app/escala/models.py:1738](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1738).
- Montagem dos argumentos do command: [app/ponto/management/commands/supra_juiz.py:102](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:102), [linha 120](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:120), [linha 142](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:142), [linha 173](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:173), [linha 186](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:186) e [linha 205](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:205).
- `classificar_dia` e seus retornos: [app/ponto/supra_juiz.py:217](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:217), [linha 233](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:233) e [linha 549](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:549). `cadastro_do_dia`: [linha 155](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:155).
- `batidas_apuraveis`, inclusive ordenação e limite superior inclusivo: [app/ponto/turnos.py:2868](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2868); `Batida.criado_em`: [app/ponto/models.py:79](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/models.py:79). `dias_encerrados` e `ausencia_cobre`: [app/ponto/turnos.py:2133](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2133), [linha 2550](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2550).
- `fio_vivo`, `data_do_chamado` e `FURO_MODULES`: [app/chamados/juizes.py:1510](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:1510), [app/chamados/catalogo/modulos.py:219](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/catalogo/modulos.py:219), [app/chamados/catalogo/furo_modulos.py:15](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/catalogo/furo_modulos.py:15).

## 18/11 17:38 · sessao 328937483a19 · E4C-F6F7 · leitura · origem=Code

A sonda abaixo é uma **proposta não executada**. A compatibilidade com `c8b6d1ec2`, a presença da coluna no banco sombra, os dados e o resultado do diff estão **[não verificados]**: esta cópia não permite conferir o diff pedido contra o main. O script registra incompatibilidades por etapa, inclusive `TypeError`; uma execução com `ERROS > 0` não constitui medida completa.

A 0056 usa o predicado `ato='resposta'` e `exclude(ato_motivo='')` ([migração 0056:12](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12)). A sonda usa SQL para reproduzi-lo porque o manager do modelo atual proíbe `update` ([models.py:4121](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4121), [models.py:4197](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4197)).

O universo `CD` é o solicitado: situação `ativo` **ou** alguma batida com data civil local na janela da empresa, cruzado com todos os dias dessa janela ([models.py:22](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/ponto/models.py:22), [colaboradores/models.py:323](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/colaboradores/models.py:323)). Ele é mais amplo que o universo do vigia: `dias_que_seguram` seleciona retidos por `furo_espelho` e seus dias acusadores ([fora_da_folha.py:125](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:125), [vigia_fio.py:530](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/vigia_fio.py:530)). `G` é o grupo real **da pessoa**, repetido em seus dias `CD`; `-` significa que ela não aparece como retida com grupo ([fora_da_folha.py:94](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:94), [fora_da_folha.py:210](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:210)).

`PQ` cobre todas as perguntas visíveis com **linhas consecutivas** `VALIDACAO`, `RESPOSTA` com `mantem=True`, inclusive fora da competência; a ordem é `(ato_em, pk)` ([models.py:4199](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4199)). Uma pergunta sem dia recebe `T=ERRO P=ERRO` e conta como erro. O instante fixo deve ser igual nas duas rodadas: a tarefa corta decisões por ele ([tarefa_do_dia.py:484](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:484)). Rode as duas medições sobre a mesma fotografia do banco e no mesmo dia civil: `classificar_export` também lê o relógio próprio ([fora_da_folha.py:137](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:137)). Igualdade da fotografia e condições do banco sombra: **[não verificado]**.

```python
# Executar SOMENTE no banco sombra, nas duas versões, com o mesmo AGORA.
# Exemplo no manage.py shell:
#     exec(open('/caminho/sonda.py').read())
#
# A sonda não verifica que o banco é sombra. Essa identificação é
# responsabilidade de quem executa. Não imprime nomes, CPF ou mensagens
# de exceção.

import calendar
import datetime
from collections import defaultdict

from django.db import connection, transaction
from django.db.models import Q
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador, Empresa
from ponto.models import Batida
from chamados.models import DecisaoPergunta, PerguntaDisputa
from chamados.services.tarefa_do_dia import carga_da_tarefa, porta_da_tarefa
from folha.fora_da_folha import fora_da_folha
from ponto.janelas import janela_fechamento


# Mesmo instante nas duas rodadas. Alterar somente antes de AMBAS as rodadas.
AGORA = datetime.datetime(
    2026, 11, 18, 20, 30, 0, tzinfo=datetime.timezone.utc
)
MES, ANO = 11, 2026

erros = []


class RollbackSonda(Exception):
    pass


class EscritaNaoPermitida(Exception):
    pass


def erro(etapa, exc):
    # Não imprime str(exc): mensagens podem conter dados pessoais.
    erros.append((etapa, type(exc).__name__))


def tentar(etapa, funcao):
    # O savepoint permite continuar após um erro SQL sem deixar a
    # transação externa quebrada. O resultado mal sucedido é None.
    try:
        with transaction.atomic():
            return True, funcao()
    except Exception as exc:
        erro(etapa, exc)
        return False, None


def canon(valor):
    """Representação determinística sem serializar objetos desconhecidos."""
    if valor is None or isinstance(valor, (bool, int, float, str)):
        return valor
    if isinstance(valor, (datetime.date, datetime.time)):
        return valor.isoformat()
    if isinstance(valor, dict):
        return tuple(
            (canon(k), canon(v))
            for k, v in sorted(valor.items(), key=lambda item: repr(item[0]))
        )
    if isinstance(valor, (list, tuple)):
        return tuple(canon(v) for v in valor)
    if isinstance(valor, (set, frozenset)):
        return tuple(sorted((canon(v) for v in valor), key=repr))
    raise TypeError("tipo não suportado na assinatura")


def assinatura_tarefa(t):
    # Não inclui agora nem celula_id. Mantém a ordem das instâncias e ações,
    # pois ela pode fazer parte da decisão publicada. [nao verificado]
    campos_dia = (
        "nao_sei", "razao", "rota", "fila", "verbos", "fio",
        "acao", "ausencia",
    )
    campos_instancia = (
        "instancia", "tipo", "hora", "data_civil", "lampada",
        "vencido", "motivo", "pergunta_id", "razao", "acoes",
    )
    return canon({
        "dia": {k: t.get(k) for k in campos_dia},
        "instancias": [
            {k: inst.get(k) for k in campos_instancia}
            for inst in t.get("instancias", ())
        ],
    })


def assinatura_porta(p):
    return canon(p)


def meses_dia(dia):
    ini = dia.replace(day=1)
    fim = dia.replace(day=calendar.monthrange(dia.year, dia.month)[1])
    return ini, fim


def dias(ini, fim):
    for n in range((fim - ini).days + 1):
        yield ini + datetime.timedelta(days=n)


# Bloqueia comandos SQL que possam escrever durante as leituras.
# O único UPDATE autorizado é executado com a chave temporariamente ligada.
permitir_update_0056 = False


def guarda_sql(execute, sql, params, many, context):
    primeiro = sql.lstrip().split(None, 1)[0].upper() if sql.strip() else ""
    permitidos = {
        "SELECT", "SHOW", "SAVEPOINT", "RELEASE", "ROLLBACK",
    }
    if primeiro == "UPDATE" and permitir_update_0056:
        return execute(sql, params, many, context)
    if primeiro not in permitidos:
        raise EscritaNaoPermitida()
    return execute(sql, params, many, context)


def executar():
    global permitir_update_0056

    print("AGORA", AGORA.isoformat())
    print("DIA_CIVIL_EXECUCAO", timezone.localdate().isoformat())
    print("DIA_CIVIL_AGORA", timezone.localdate(AGORA).isoformat())

    tabela = connection.ops.quote_name(DecisaoPergunta._meta.db_table)
    coluna_ato = connection.ops.quote_name("ato")
    coluna_motivo = connection.ops.quote_name("ato_motivo")
    coluna_mantem = connection.ops.quote_name("resposta_mantem_validacao")

    # ato_motivo é NOT NULL no modelo desta cópia. Este SQL reproduz
    # filter(ato='resposta').exclude(ato_motivo='') da 0056.
    predicado = "%s = %%s AND %s <> %%s" % (coluna_ato, coluna_motivo)

    with connection.execute_wrapper(guarda_sql):
        with connection.cursor() as cursor:
            cursor.execute(
                "SELECT COUNT(*) FROM %s WHERE %s" % (tabela, predicado),
                ["resposta", ""],
            )
            print("PRE_0056", cursor.fetchone()[0])

            permitir_update_0056 = True
            try:
                cursor.execute(
                    "UPDATE %s SET %s = %%s WHERE %s"
                    % (tabela, coluna_mantem, predicado),
                    [True, "resposta", ""],
                )
            finally:
                permitir_update_0056 = False

        hoje = timezone.localdate(AGORA)

        ok, empresas = tentar(
            "empresas",
            lambda: list(Empresa.objects.all().order_by("pk")),
        )
        if not ok:
            return

        janelas = {}
        grupos = {}
        pares_cd = set()
        colabs = {}

        for empresa in empresas:
            eid = empresa.pk
            ok, janela = tentar(
                "janela",
                lambda e=empresa: janela_fechamento(MES, ANO, e),
            )
            if not ok:
                continue
            ini, fim = janela
            janelas[eid] = (ini, fim)
            print(
                "EMPRESA", eid,
                "CORTE", empresa.dia_inicio_competencia,
                "INI", ini.isoformat(),
                "FIM", fim.isoformat(),
            )

            # __date usa a zona ativa do Django para o timestamp.
            def carregar_colabs():
                ids_batida = Batida.objects.filter(
                    colaborador__empresa_id=eid,
                    timestamp__date__gte=ini,
                    timestamp__date__lte=fim,
                ).values("colaborador_id")
                return list(
                    Colaborador.objects.filter(empresa_id=eid)
                    .filter(Q(situacao="ativo") | Q(pk__in=ids_batida))
                    .select_related("empresa")
                    .order_by("pk")
                )

            ok, lista = tentar("universo_cd", carregar_colabs)
            if ok:
                for c in lista:
                    colabs[c.pk] = c
                    for dia in dias(ini, fim):
                        pares_cd.add((c.pk, dia))

            # Chama a função real que calcula o grupo da pessoa.
            ok, resultado = tentar(
                "fora_da_folha",
                lambda e=empresa: fora_da_folha(
                    e, MES, ANO, agora=AGORA
                ),
            )
            if ok:
                for linha in resultado["colabs"]:
                    grupos[linha["colab"]] = linha["grupo"] or "-"
            else:
                # Falha da classificação não pode parecer ausência de grupo.
                grupos[eid, "erro_empresa"] = True

        # Ler a coluna diretamente também permite detectar sua ausência no
        # banco sem exigir que o modelo Python do main a declare.
        def pares_validacao_resposta():
            sql = (
                "SELECT pergunta_id, ato, %s "
                "FROM %s ORDER BY pergunta_id, ato_em, id"
                % (coluna_mantem, tabela)
            )
            encontrados = set()
            anterior_pergunta = None
            anterior_ato = None
            with connection.cursor() as cursor:
                cursor.execute(sql)
                while True:
                    bloco = cursor.fetchmany(2000)
                    if not bloco:
                        break
                    for pergunta_id, ato, mantem in bloco:
                        if pergunta_id != anterior_pergunta:
                            anterior_ato = None
                        if (
                            anterior_ato == "validacao"
                            and ato == "resposta"
                            and mantem is True
                        ):
                            encontrados.add(pergunta_id)
                        anterior_pergunta = pergunta_id
                        anterior_ato = ato
            return encontrados

        ok, ids_perguntas = tentar("sequencia_pq", pares_validacao_resposta)
        if not ok:
            ids_perguntas = set()

        def carregar_perguntas():
            return list(
                PerguntaDisputa.objects.filter(pk__in=ids_perguntas)
                .select_related("disputa__colaborador__empresa")
                .order_by("pk")
            )

        ok, perguntas = tentar("perguntas_pq", carregar_perguntas)
        if not ok:
            perguntas = []

        pares_pq = []
        for p in perguntas:
            c = p.disputa.colaborador
            colabs[c.pk] = c
            pares_pq.append((p.pk, c.pk, p.data_evento, c.empresa_id))

        # Uma carga por empresa/janela. O grupo da competência inclui todos
        # os pares CD e as perguntas PQ que caem nela. Perguntas de outros
        # períodos são agrupadas por mês civil.
        lotes = defaultdict(set)
        for cid, dia in pares_cd:
            c = colabs[cid]
            ini, fim = janelas[c.empresa_id]
            lotes[(c.empresa_id, ini, fim)].add(cid)

        for pk, cid, dia, eid in pares_pq:
            if dia is None:
                erro("pq_sem_dia", ValueError())
                continue
            janela = janelas.get(eid)
            if janela and janela[0] <= dia <= janela[1]:
                ini, fim = janela
            else:
                ini, fim = meses_dia(dia)
            lotes[(eid, ini, fim)].add(cid)

        resultados = {}

        for (eid, ini, fim), ids in sorted(lotes.items()):
            lista = [colabs[cid] for cid in sorted(ids)]
            ok, carga = tentar(
                "carga_da_tarefa",
                lambda cs=lista, a=ini, b=fim: carga_da_tarefa(
                    cs, a, b, AGORA
                ),
            )
            if not ok:
                continue

            necessarios = {
                (cid, dia)
                for cid, dia in pares_cd
                if cid in ids and colabs[cid].empresa_id == eid
                and ini <= dia <= fim
            }
            necessarios.update(
                (cid, dia)
                for _, cid, dia, peid in pares_pq
                if dia is not None and peid == eid
                and cid in ids and ini <= dia <= fim
            )

            for cid, dia in sorted(necessarios):
                chave = (cid, dia)
                if chave in resultados:
                    continue

                ok, tarefa = tentar(
                    "tarefa",
                    lambda c=colabs[cid], d=dia: carga.tarefa(c, d),
                )
                if not ok:
                    resultados[chave] = ("ERRO", "ERRO")
                    continue

                ok_t, sig_t = tentar(
                    "assinatura_tarefa",
                    lambda t=tarefa: assinatura_tarefa(t),
                )
                ok_p, porta = tentar(
                    "porta_da_tarefa",
                    lambda t=tarefa: porta_da_tarefa(t),
                )
                if ok_p:
                    ok_p, sig_p = tentar(
                        "assinatura_porta",
                        lambda p=porta: assinatura_porta(p),
                    )
                resultados[chave] = (
                    repr(sig_t) if ok_t else "ERRO",
                    repr(sig_p) if ok_p else "ERRO",
                )

        for cid, dia in sorted(pares_cd):
            c = colabs[cid]
            if grupos.get((c.empresa_id, "erro_empresa")):
                grupo = "ERRO"
            else:
                grupo = grupos.get(cid, "-")
            t, p = resultados.get((cid, dia), ("ERRO", "ERRO"))
            print(
                "CD col%d %s T=%s P=%s G=%s"
                % (cid, dia.isoformat(), t, p, grupo)
            )

        for pk, cid, dia, _eid in sorted(pares_pq):
            if dia is None:
                data = "-"
                t, p = "ERRO", "ERRO"
            else:
                data = dia.isoformat()
                t, p = resultados.get((cid, dia), ("ERRO", "ERRO"))
            print("PQ %s col%d %s T=%s P=%s" % (pk, cid, data, t, p))


try:
    with schema_context("juliani"):
        try:
            with transaction.atomic():
                try:
                    executar()
                except Exception as exc:
                    erro("execucao", exc)
                # Sempre sair do atomic por exceção: desfaz a 0056 simulada.
                raise RollbackSonda()
        except RollbackSonda:
            pass
except Exception as exc:
    erro("schema_ou_rollback", exc)
finally:
    print("ERROS", len(erros))
    for etapa, tipo in erros[:20]:
        print("ERRO", etapa, tipo)
    print("FIM_SONDA_DIFF_RETIDO")
```

Funções reais chamadas diretamente pela sonda nesta cópia:

- `janela_fechamento`: [janelas.py:54](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/ponto/janelas.py:54).
- `fora_da_folha`: [fora_da_folha.py:177](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:177); ela chama `dias_que_seguram` e calcula o grupo por `_grupo` ([fora_da_folha.py:125](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:125), [fora_da_folha.py:94](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:94)).
- `carga_da_tarefa` e `CargaDaTarefa.tarefa`: [tarefa_do_dia.py:795](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:795), [tarefa_do_dia.py:632](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:632). É o mesmo caminho em lote usado pelo vigia ([vigia_fio.py:591](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/vigia_fio.py:591)); a casca `tarefa_do_dia` também delega a `carga.tarefa` ([tarefa_do_dia.py:800](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:800)).
- `porta_da_tarefa`: [tarefa_do_dia.py:779](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:779).

Existência e assinatura dessas funções no main `c8b6d1ec2`: **[não verificado]**. Não é possível afirmar, com esta cópia e o anexo fornecido, que alguma delas exista somente em um dos lados.

## 18/11 17:37 · sessao d471b06a0eb8 · DECLARADO-X-GRAVADO · construtor · origem=Code

**Parei sem implementar a cura:** não identifiquei uma porta existente que devolva ao colaborador, com a frase exigida, uma resposta já validada nos dois ramos abrangidos. A materialização é chamada após o carimbo de validação. [validacao.py:743](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:743), [materializacao.py:1923](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/materializacao.py:1923)

A porta `recusar_pergunta` devolve a pergunta ao colaborador, mas recusa perguntas já validadas. A outra porta, `devolver_validacao_recusada`, devolve a resposta ao **admin**, preserva a resposta e se restringe ao caso de resposta de hora; ela não realiza a devolução pedida ao colaborador para o ramo `data_hora`. [validacao.py:898](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:898), [validacao.py:237](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:237)

A porta de reabertura limpa a resposta e registra o motivo na trilha, mas a representação da pergunta na API examinada não inclui esse motivo. A recusa existente envia um push genérico; a entrega da frase exigida ao colaborador por essa via está **[não verificado]**. [disputa_emissao.py:1584](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/disputa_emissao.py:1584), [api/views.py:2549](/home/ronald/wt-codex-declarado-x-gravado-173308/app/api/views.py:2549), [validacao.py:943](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:943) [nao verificado]

Falta, portanto, uma porta chamável que trate a devolução de resposta **já validada**, para `hora` e `data_hora`, e disponibilize a frase ao colaborador. Sua localização e implementação exigidas estão **[não verificado]**. A frase literal também não consta do dado fornecido **[não verificado]**.

Nenhum arquivo foi alterado; R1–R10, mutantes, compilação e testes não foram executados. A contagem de palavras de band-aid no diff está **[não verificado]**. A entrega e a linha LEI-AKITA solicitadas permanecem pendentes; não apresento os REDs como existentes. [nao verificado]

## 18/11 17:36 · sessao 7a0a1b4d842c · REPROVAS-1811-1740 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: PROCEDE.** O item 2 segue com as reservas abaixo. [não verificado: hash do HEAD por Git; a cópia disponível não tem metadados Git.]

### 1. E4C-F6F7 — PROCEDE

1. **Premissas: conferem parcialmente.** A linha 164 do main devolve a tarefa ao colaborador em caso de recusa ou reabertura; ela não demonstra o laço atribuído à ponta F6F7 ([tarefa_do_dia.py:142](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/tarefa_do_dia.py:142)). A cadeia de leitura existe: `fora_da_folha` lê a porta, deriva o fio e agrupa a pessoa pelos fios dos dias; o retrato consome essa saída ([fora_da_folha.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:70), [fora_da_folha.py:94](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:94), [fora_da_folha.py:265](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:265), [retrato_folha.py:1212](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1212)). Cadastro, vigia e pautas também são leitores, direta ou indiretamente ([cadastro_realidade.py:472](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/cadastro_realidade.py:472), [vigia_fio.py:594](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/vigia_fio.py:594), [pautas_dos_discordantes.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/pautas_dos_discordantes.py:70)).

   **Há vigia de produção onde um contador pode ser integrado:** `vigiar_fio_morto` chama contadores, registra seus valores e esperados, e está encadeado na rotina ([vigiar_fio_morto.py:100](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:100), [vigiar_fio_morto.py:179](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:179), [vigiar_fio_morto.py:244](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:244), [crons.py:1737](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/config/crons.py:1737)). O contador nomeado da F7 e seu caso que morde na ponta: **[não verificado]**.

2. **Colide com o aval vigente.** A autorização de pousar na véspera se o diff der zero conflita com a vedação fornecida de pouso que toque folha ou retenção entre 18/11 às 18h e o fim da simulação, pois a cadeia de leitores alcança folha e retrato ([fora_da_folha.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:70), [retrato_folha.py:1212](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1212)). O zero medido é uma condição adicional do aval anexo; não elimina literalmente a vedação fornecida. A precedência F6F7 antes da F9 e o compact continuam condições informadas; seu cumprimento **[não verificado]**.

3. **O literal não fecha ainda.** Para corte 21, competência 11/2026 compreende **21/10/2026 a 20/11/2026**, inclusive; outros cortes da empresa produzem outros intervalos ([janelas.py:10](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/janelas.py:10), [janelas.py:36](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/janelas.py:36)). O diff existente é de leitura e percorre colaboradores e dias, mas compara C17 com a tarefa da própria cópia; suas projeções não executam os censos e sua saída é agregada e anonimizada. Ele não é a porta pronta para executar **main versus ponta curada** nem entregar a tabela de mudança de grupo e lista nominal pedidas ([diff_e3_sombra.py:1](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:1), [diff_e3_sombra.py:46](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:46), [diff_e3_sombra.py:127](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:127)). Outra porta capaz desse confronto **[não verificado]**.

4. **Exportada, dinheiro ou `!`:** há alcance sobre a apresentação do retido e o retrato lavrado, já reconhecido na retificação do aval; o cron de retrato usa `--apply` ([retrato_folha.py:1207](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1207), [crons.py:1650](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/config/crons.py:1650)). Alteração de competência exportada, dinheiro ou outro `!` causado pela ponta **[não verificado]**.

### 2. ESCALA-NAO-DESCREVE — CONFERE, com alcance limitado

1. **Premissas:** o veredito fornecido relata selo sem mordida e ausência de log do mutante; a cópia da obra e `test_escala_nao_descreve.py:61-62` **[não verificado]**. Também **[não verificado]** se a cura já feita mata o mutante, se o log existe e se os commits citados contêm a linha exigida. [nao verificado]

2. **Colisão:** o pouso indicado para depois da simulação é compatível com a janela vigente fornecida; o rito, o compact e a ordem eventualmente aplicável continuam pendentes de comprovação **[não verificado]**. [nao verificado]

3. **Literal:** o aval exige log do mutante nomeado e linha `LEI-AKITA` com `padrao=`; o fechamento dessas condições **[não verificado]**. O aceite para os pontos do veredito classificados como fila também **[não verificado]**.

4. **Exportada, dinheiro ou `!`:** o dado fornecido declara que a porta de vínculo regenera células e reconcilia, mas deixa **não verificado** o efeito sobre competência entregue; o código presente confirma a chamada de reconciliação ([vinculo.py:346](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/colaboradores/services/vinculo.py:346)). Efeito sobre exportada, dinheiro ou outro `!` **[não verificado]**.

## 18/11 17:33 · sessao ae999d6c0dcd · E4C-F6F7 · construtor · origem=Code

A cura liga o `handle` ao contador uma vez e publica as quatro chaves devolvidas por ele, antes da bifurcação de `--aplicar`, sem nova escrita ([backfill_decisoes_pergunta.py:595](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:595)).

O teste integrado cria uma pergunta validada sem linha de validação e confere a saída do comando ([test_e4c_f6f7_c11_backfill.py:60](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:60)). **RED e GREEN integrados: [não verificado]**, pois precisam de banco e não foram executados. Comando para quem certifica: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f6f7_c11_backfill.BackfillPublicaContadorF7
```

A verificação sem banco foi executada antes da cura e falhou com `AssertionError: RED: handle nao chama o contador`; depois, passou com `GREEN: costura sem banco`. Ela verifica a chamada, a posição anterior ao ramo `--aplicar` e o uso das quatro chaves com valores sentinela ([test_e4c_f6f7_c11_backfill.py:9](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:9)). Comando GREEN: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py
```

Os cinco mutantes em memória morreram: M1 remove chamada e linha; M2–M5 fixam, respectivamente, número, esperado, dono e nome ([test_e4c_f6f7_c11_backfill.py:13](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:13)). Cada comando deve falhar por asserção:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M1
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M2
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M3
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M4
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M5
```

`py_compile` passou nos dois arquivos, com a escrita de bytecode desativada: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import py_compile
py_compile._write_atomic = lambda *args, **kwargs: None
for p in ('chamados/management/commands/backfill_decisoes_pergunta.py',
          'chamados/tests/test_e4c_f6f7_c11_backfill.py'):
    py_compile.compile(p, cfile=p + '.pyc', doraise=True)
    print('py_compile OK:', p)
PY
```

LEI-AKITA: origem=[backfill_decisoes_pergunta.py::handle](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:595), testemunha=[contador_validada_projetada_sem_linha](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:94) (lê `DecisaoPergunta.linhas_da_projecao_sem_linha`), RED=[teste integrado](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:61) [não verificado]; verificação sem banco RED executada, quem-mais-le=[teste F7](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_f7_contador_validada_sem_linha.py:31) e [teste sem banco C7](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:51), juizes novos=0, padrao=[Parallel Change / Expand-Contract](/home/ronald/wt-codex-e4c-f6f7-132608/app/docs/GLOSSARIO.md:18). [nao verificado]

## 18/11 17:33 · sessao 681f0c12c82c · E4C-F6F7 · leitura · origem=Code

A sonda abaixo mede **pares candidato de colaborador e dia que também são dias de retenção**, além de colaboradores distintos nessa interseção. Ela não calcula o número de pessoas que efetivamente mudariam de grupo A–E. Na cópia, o grupo depende dos fios de **todos** os dias retidos, e a seleção da porta depende de mais condições além da sequência de atos. [app/folha/fora_da_folha.py:94](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:94), [app/folha/fora_da_folha.py:210](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:210), [app/chamados/services/tarefa_do_dia.py:694](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:694)

A cópia não tem metadados Git acessíveis; por isso, **não foi possível confirmar nenhuma das funções no main pai**, nem afirmar que alguma esteja ausente nele. [não verificado] Na cópia, as dependências de domínio usadas são `es_da_ressuscitacao`, `empresas_visiveis`, `competencias_a_lavrar` e `dias_que_seguram`. [app/chamados/models.py:4221](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/models.py:4221), [app/colaboradores/queries.py:78](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/colaboradores/queries.py:78), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209), [app/folha/fora_da_folha.py:125](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:125)

O predicado da 0056 é `ato='resposta'` e `ato_motivo` diferente de vazio; a simulação também preserva `True` já existente nas demais respostas. [app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12) A sequência usa o horizonte, a ordem e a exclusão de ressuscitação da carga; o vínculo ao dia usa `conversa=False`, `colaborador_id` e `data_evento`. [app/chamados/services/tarefa_do_dia.py:393](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:393), [app/chamados/services/tarefa_do_dia.py:506](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:506)

O universo usa a função real `dias_que_seguram`, chamada por `fora_da_folha` e pelo vigia; o recorte de empresas e competências segue o vigia. A função de competências inclui a atual e, quando não entregue, a anterior: isso não demonstra cobertura de todas as competências históricas abertas. [app/folha/fora_da_folha.py:200](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:200), [app/chamados/services/vigia_fio.py:529](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/vigia_fio.py:529), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209)

Execute o texto pelo shell de `tenant_command` indicado, por exemplo redirecionando o arquivo de entrada. A sonda não foi executada aqui. [não verificado] Ela bloqueia comandos SQL que não comecem por `SELECT`, não imprime mensagens de exceção que possam conter dados pessoais e invalida os números se `ERROS > 0`; exceções eventualmente capturadas dentro das próprias funções chamadas não são observáveis por esse contador. [não verificado]

```python
# Sonda F6F7 item 1.
# Executar dentro de: manage.py tenant_command shell --schema=juliani
# Somente leitura; nao muda schema, nao abre atomic e nao grava arquivos.

from itertools import groupby

erros = 0
etapa = "inicializacao"


def somente_select(execute, sql, params, many, context):
    # Defesa adicional: rejeita comandos que nao sejam SELECT.
    if not sql.lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL nao SELECT bloqueado")
    return execute(sql, params, many, context)


try:
    from django.db import connection
    from django.utils import timezone

    from chamados.models import DecisaoPergunta as D
    from colaboradores.queries import empresas_visiveis
    from folha.fora_da_folha import dias_que_seguram
    from ponto.services.he_pendente_lavrado import competencias_a_lavrar

    agora = timezone.now()
    hoje = timezone.localdate(agora)

    with connection.execute_wrapper(somente_select):
        etapa = "predicado_0056"
        # Mesmo predicado da migracao, sem update.
        marcadas = set(
            D.objects.filter(ato="resposta")
            .exclude(ato_motivo="")
            .values_list("pk", flat=True)
        )
        print("RESPOSTAS_PREDICADO_0056", len(marcadas))

        etapa = "sequencias"
        # Uma cauda pode conter respostas ja True e respostas que a 0056
        # marcaria. Examinar as perguntas com qualquer uma dessas respostas.
        candidatas = set(
            D.objects.filter(ato="resposta")
            .filter(resposta_mantem_validacao=True)
            .values_list("pergunta_id", flat=True)
        )
        candidatas.update(
            D.objects.filter(pk__in=marcadas)
            .values_list("pergunta_id", flat=True)
        )

        decisoes = (
            D.objects.filter(pergunta_id__in=candidatas, ato_em__lte=agora)
            .select_related("pergunta")
            .order_by("pergunta_id", "ato_em", "pk")
        )

        perguntas_mudam_ato = 0
        pares_perguntas_do_fato = set()
        perguntas_sem_dia_ou_colaborador = 0

        for pergunta_id, linhas in groupby(
            decisoes.iterator(), key=lambda d: d.pergunta_id
        ):
            sequencia = [
                d for d in linhas if not D.es_da_ressuscitacao(d)
            ]
            if not sequencia:
                continue

            k = len(sequencia)
            while k:
                d = sequencia[k - 1]
                mantem_depois = (
                    d.ato == D.RESPOSTA
                    and (d.resposta_mantem_validacao or d.pk in marcadas)
                )
                if not mantem_depois:
                    break
                k -= 1

            if not (
                0 < k < len(sequencia)
                and sequencia[k - 1].ato == D.VALIDACAO
            ):
                continue

            perguntas_mudam_ato += 1
            p = sequencia[-1].pergunta

            # A carga da tarefa usa apenas perguntas do fato.
            if p.conversa:
                continue
            if p.colaborador_id is None or p.data_evento is None:
                perguntas_sem_dia_ou_colaborador += 1
                continue
            pares_perguntas_do_fato.add((p.colaborador_id, p.data_evento))

        print("PERGUNTAS_MUDAM_ATO", perguntas_mudam_ato)
        print("PARES_PERGUNTAS_DO_FATO", len(pares_perguntas_do_fato))
        print(
            "PERGUNTAS_DO_FATO_SEM_DIA_OU_COLABORADOR",
            perguntas_sem_dia_ou_colaborador,
        )

        etapa = "universo_retidos"
        # Mesmo recorte de empresa e competencia do censo de alcance.
        # Conjuntos removem repeticoes entre competencias.
        retidos = set()
        pares_retidos = set()
        competencias = 0

        for empresa in empresas_visiveis():
            for mes, ano in competencias_a_lavrar(empresa, hoje):
                competencias += 1
                universo = dias_que_seguram(empresa, mes, ano, hoje=hoje)
                retidos.update(universo["furo"])
                for cid, (_motivos, dias) in universo["dias"].items():
                    for dia in dias:
                        pares_retidos.add((cid, dia["data"]))

        intersecao = pares_perguntas_do_fato & pares_retidos
        colaboradores_intersecao = {cid for cid, _dia in intersecao}

        print("COMPETENCIAS_EXAMINADAS", competencias)
        print("RETIDOS_DISTINTOS_FURO_ESPELHO", len(retidos))
        print("PARES_DIAS_RETIDOS_DISTINTOS", len(pares_retidos))
        print("INTERSECAO_PARES", len(intersecao))
        print("INTERSECAO_COLABORADORES_DISTINTOS", len(colaboradores_intersecao))
        for cid, dia in sorted(intersecao)[:20]:
            print("PAR", "col%s" % cid, dia.isoformat())

except Exception as exc:
    erros += 1
    # Sem str(exc) ou traceback: podem conter dados pessoais.
    print("ERRO", etapa, type(exc).__name__)
finally:
    print("ERROS", erros)
    print("FIM_SONDA_F6F7_ITEM1")
```

Um resultado zero na interseção só elimina candidatos **nesse recorte e instante**; não substitui o diff de `T`/porta, nem prova ausência de mudança após outros dados ou em competências fora do recorte. [app/chamados/services/tarefa_do_dia.py:484](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:484), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209)

_7076 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
