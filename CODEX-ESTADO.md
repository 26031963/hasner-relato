# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 15:18** · publicado em **17/11/2026 15:19** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 15:18 · sessao b5989c47cce4 · E4C-F2 · censo · origem=Code

Nos anexos colados, conto a primeira linha de cada anexo como 1, incluindo linhas vazias. As referências à cópia usam seus próprios números de linha.

**1 — Escritor antigo `responder_automaticamente`: (b), com ressalva sobre “descartou”.** O Codex viu exatamente o defeito: “RESPOSTA automática ou pela trilha [...] inserida pelo código antigo após o backfill, omitindo a coluna. Ela recebe `False`, e a leitura nova desfaz a validação”. A triagem respondeu “PROCEDENTE, FORA DA CONDICAO 1, sobe ao coordenador” e “Nao aplicado (escopo do aval literal: condicao 1)”. Portanto, houve exclusão da cura por escopo, não rejeição técnica do achado. (`v5.r:25`; `triagem_v5.txt:6–10`.) [nao verificado]

O escritor antigo está confirmado: grava RESPOSTA com motivo sem declarar a coluna nova. ([disputa_emissao.py:1678](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/disputa_emissao.py:1678).)

**1 — Escritor antigo `carimbar_resposta_pela_trilha`: (b), na mesma acepção.** O trecho “RESPOSTA automática ou pela trilha” identifica também este escritor; a triagem aceita o defeito e deixa a correção fora do escopo autorizado. O código antigo confirma a RESPOSTA com motivo e sem a coluna. (`v5.r:25`; `triagem_v5.txt:6–10`; [disputa_emissao.py:1699](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/disputa_emissao.py:1699).)

Há uma ressalva factual ao laudo: a cópia descreve `curar_perguntas_em_limbo` como “cura de acervo que roda UMA VEZ”, enquanto a linha 1482 nomeia outro comando. Seu agendamento como cron e sua execução durante a janela são **[nao verificado]**. ([crons.py:1476](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/config/crons.py:1476); `app/config/crons.py:1477`; `app/config/crons.py:1482`.)

**1 — Escritor antigo da foto da projeção: (a).** O tema estava presente: “automática, carimbo pela trilha e foto da projeção escrevem `True`”. A fábrica e seus backfills também estavam no censo anterior. Entretanto, o contraexemplo temporal do v5 nomeia apenas automática e trilha. Assim, “TERCEIRO nao citado pelo Codex” deve ser restringido a **não identificado como terceiro escritor antigo afetado pela janela**; ele foi citado como montador. (`v5.r:22,25`; `v2.r:25,29`; `opus_final_c8f5.txt:4–5`.)

A cópia confirma a fábrica antiga produzindo RESPOSTA com `ato_motivo`, sem a coluna nova. ([models.py:4100](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/models.py:4100); `app/chamados/models.py:4101`.)

Como complemento comum aos três escritores, a correção sugerida pela triagem era executar novamente o backfill “DEPOIS do HUP”; porém o deploy documenta que comandos por `docker exec`, incluindo cron em curso, não recebem esse sinal. Esse prolongamento da janela não foi explicitado no contraexemplo do v5. (`triagem_v5.txt:9`; `bin/deploy.sh:329–330`; `v5.r:25`.)

**2 — Janela inversa merge→migrate: (a) apenas quanto ao mecanismo concreto do cron.** A incompatibilidade geral **já foi vista**: “Código novo executado contra banco sem 0055 é incompatível, pois já exige a coluna”. Portanto, seria incorreto classificá-la integralmente como despercebida. O trecho não identifica o cron que nasce após o merge e antes da migration; é essa especificação que o opus acrescenta. (`v2.r:90`; `opus_final_c8f5.txt:13`.)

Descarte dessa janela por alguma triagem fornecida: **[nao verificado]**. A expressão “classe da casa, nao bloqueia” pertence ao próprio opus, não a uma triagem do Codex. (`opus_final_c8f5.txt:13`.)

**3 — Condição documental aberta: (e), provisória.** O suporte literal da pendência é “SEM ato de devolucao” e “lei NAO decidida”; o laudo registra ainda zero ocorrências dos dois cortes em `CORTES.md/json`. ([LEIS.md:151](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/docs/LEIS.md:151); `opus_final_c8f5.txt:14–15`.)

Não identifiquei nas respostas fornecidas uma análise dessa pendência documental; que ela tenha sido perguntada é **[nao verificado]**. **Sem as `.q`, é impossível distinguir com segurança (a) de (e).** A triagem mencionar o corte como justificativa funcional não comprova uma pergunta sobre sua documentação. (`triagem_v1.txt:18–20`; conteúdo das perguntas: **[nao verificado]**.)

**4 — Decisão por prefixo de `ato_motivo` em materialização: (a).** O Codex descreveu o comportamento, mas não o denunciou como contradição: “O leitor decisório de motivo [...] restringe-se a **DEVOLUCAO BUG-PAR**” e “permanece o reconhecimento textual de DEVOLUCAO BUG-PAR, fora dessa cura”. (`v2.r:41,91`.)

A cópia confirma literalmente `.startswith(MOTIVO_BUG_PAR_103)`. A triagem “8-11: conferencia, sem defeito apontado” não documenta descarte de uma acusação desse defeito. ([materializacao.py:275](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/materializacao.py:275); `triagem_v1.txt:22`.)

- Resposta automática antiga → **(b), excluída da cura por escopo**. (`triagem_v5.txt:6–10`)
- Carimbo antigo pela trilha → **(b), excluído da cura por escopo**. (`triagem_v5.txt:6–10`)
- Foto antiga da projeção → **(a)**. (`v5.r:22,25`)
- Janela inversa → **(a) parcial: mecanismo do cron; incompatibilidade já vista**. (`v2.r:90`; `opus_final_c8f5.txt:13`)
- Pendência documental → **(e) provisória; (a)/(e) indistinguíveis sem `.q`**. [nao verificado]
- Prefixo de motivo na materialização → **(a)**. (`v2.r:41,91`)

## 17/11 15:17 · sessao 46f631bd2ab0 · FIDELIDADE-BATIDA-X-ESPELHO · leitura · origem=Code

**A) Não localizei um contador de frota que verifique o contrato completo por identidade de batida.** A inexistência absoluta fora dos caminhos inspecionados permanece [não verificado]. Os instrumentos encontrados verificam fixtures, minutos ou resumos, como detalhado abaixo. `app/relatorios/tests/test_cartao_nao_perde_batida.py:57`; `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:78`; `app/folha/porta_export.py:496`.

A cadeia real preserva objetos `Batida` em `dia['batidas']`: o espelho agrupa por `pk → dia`, o coletor do PDF consome esses dias e acrescenta `b.orfa`, e o desenho imprime tanto as normais quanto as órfãs. A identidade correta é **PK**, pois timestamps podem colidir. `app/ponto/services/espelho.py:450`; `app/ponto/services/espelho.py:574`; `app/ponto/services/espelho.py:764`; `app/relatorios/pdf_espelho.py:596`; `app/relatorios/pdf_espelho.py:604`; `app/relatorios/pdf_espelho.py:876`; `app/ponto/turnos.py:2916`.

| Instrumento | Universo, exclusões e esperado | Quem chama; placar; o que não cobre |
|---|---|---|
| `CartaoNaoPerdeBatidaTest` | Uma pessoa, três turnos noturnos e uma espúria; somente os PKs plantados na fixture. Espera faltantes vazios e nenhuma repetição. `app/relatorios/tests/test_cartao_nao_perde_batida.py:24`; `:39`; `:57`; `:67`. | Os testes chamam `_coletar_dados_espelho`. Não consultam `batidas_apuraveis`, não varrem frota nem comparam o dia de cada PK com o juiz. `app/relatorios/tests/test_cartao_nao_perde_batida.py:58`; `:61`; `:68`; `:71`. Vínculo nominal desse teste com item do placar: [não verificado]. |
| `pdf_x_espelho_divergentes` | Empresas em geral, mas somente pessoas classificadas como `entra` no TXT; classificação que falha e pessoa não encontrada são puladas. Compara **minutos e aberto por dia**, esperando zero divergentes. `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:56`; `:78`; `:97`; `:114`. | Comando manual, declarado como não cron; chama coletor do PDF e `espelho_do_colab`. Está em **E4-pdf-x-espelho**. Não compara PKs, completude, duplicação ou procedência. `app/config/crons.py:1139`; `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:73`; `:92`; `app/core/espelho_verdade.py:124`. |
| `selo_leitores_no_mesmo_numero` → `folha.porta_export.medir` | Empresas ativas; a comparação tela/PDF considera `classificar_export(... status='entra')`. Compara cinco escalares do resumo, tolerância `0.01`; o selo espera zero divergências. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`; `app/folha/porta_export.py:469`; `:496`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:178`. | O comando chama `medir` **e lavra**. É selo de porta/manual, relacionado ao **E4** e fonte do **R4 estrutural**. Uma perda compartilhada pelos leitores pode conservar os escalares: o predicado não contém identidade de batida. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88`; `app/config/crons.py:887`; `app/core/espelho_verdade.py:120`; `app/core/placar_estrutural.py:414`; `app/folha/porta_export.py:497`. |
| `e6_oraculo` | Empresas ativas, pessoas com `FechamentoMensal`; opcionalmente só TXT. Exclui isenção/ausência, falta de lavratura, zero dos dois lados e diferença explicada pela janela. Compara minutos, aceitando diferença até dez minutos. `app/ponto/management/commands/e6_oraculo.py:303`; `:339`; `:365`. | Lê apuráveis, mas usa pareamento independente; não certifica cada PK impresso contra `turnos_do_colab`. Relaciona-se a **E6-sistema-bate** e **E6-batida-impar**. `app/ponto/management/commands/e6_oraculo.py:8`; `app/core/espelho_verdade.py:180`; `:188`. |
| `esmeril_espelho` | Pessoas em operação, excluindo isentos; janela da competência mais antiga não entregue até ontem. Conta assinaturas recorrentes; A5 usa órfãs da ata com repetição mínima. `app/ponto/services/esmeril_espelho.py:38`; `:125`; `:139`; `:206`. | Há cron declarado com `--apply`. Seus insumos são células, turnos e ata; não compara `dia['batidas']` com o universo apurável. `app/config/crons.py:640`; `app/ponto/services/esmeril_espelho.py:95`. Item E1–E6/R específico dessa fidelidade: [não verificado]. |

Portanto, **E4/R4 são os mais próximos na frota; o teste `CartaoNaoPerdeBatidaTest` é o mais próximo por PK**. O R4 enumera comparações entre leitores e valores, sem declarar o par “apuráveis × marcações impressas”. `app/core/placar_estrutural.py:211`; `app/relatorios/tests/test_cartao_nao_perde_batida.py:61`.

**B) Script proposto, sem execução no banco. [não verificado]**

Há dois recortes necessários: `ESPELHO_ATE_HOJE` mede o espelho solicitado até hoje; `PDF_REAL` respeita o teto em ontem do coletor. A classe **e** pertence a `PDF_REAL`, pois esse coletor atribui `orfa`. Assim, batidas de hoje não são acusadas como perdas do PDF por causa do teto previsto. `app/ponto/services/espelho.py:1194`; `app/relatorios/pdf_espelho.py:285`; `:598`; `:604`.

A janela vem de `janela_competencia(empresa, 11, 2026)`; “ativa” significa `situacao='ativo'`. O universo apurável inclui pendentes de DP e exclui retratadas; o script não exclui isentos nem exige entrada no TXT. `app/folha/export.py:47`; `app/colaboradores/models.py:296`; `:323`; `app/ponto/turnos.py:2871`.

| Classe | Identificação usada |
|---|---|
| a | `b.pk` esperado ausente de **todos os dias retornados** da pessoa. Universo por `batidas_apuraveis` e dia por `dia_das_batidas`, incluindo as não reclamadas por turno. `app/ponto/turnos.py:2868`; `:2898`; `:2957`. |
| b | Mesmo `b.pk` em mais de um `dia['data']`. Há diagnóstico adicional para repetição no mesmo dia. `app/ponto/services/espelho.py:764`; `app/ponto/turnos.py:2916`. |
| c | `b.pk` em `t.batidas`, comparando `dia['data']` com `t.data_turno`. Batida sem turno é publicada separadamente, sem inventar dia de turno. `app/ponto/turnos.py:43`; `:2961`. |
| d | `b.pk` fora de `batidas_apuraveis(colab)` **sem recorte temporal**, e `b.procedencia` vazio. `app/ponto/turnos.py:2879`; `app/ponto/services/espelho.py:755`; `app/relatorios/pdf_espelho.py:951`. |
| e | `b.pk` cujo `b.orfa` foi atribuído pelo coletor real. **A atribuição atual usa HH:MM**, embora a contagem abaixo preserve PK. `app/relatorios/pdf_espelho.py:604`. |
| borda | Subconjunto de a: `t.saida.pk`, com `t.cross_meianoite` e `t.data_turno == fim` daquele recorte. Publica também o denominador elegível. `app/ponto/turnos.py:43`; `app/relatorios/pdf_espelho.py:271`. |

O script mede as estruturas consumidas pelo desenho; não inspeciona bytes renderizados. Em particular, d verifica o campo de procedência, enquanto o PDF limita o texto agregado de observações a 120 caracteres. As células sintéticas de isenção são impressas por outro campo e não entram como `Batida`. `app/relatorios/pdf_espelho.py:887`; `:951`; `:974`.

O código abaixo propõe transação PostgreSQL somente leitura, savepoint por pessoa, contagens antes/depois no mesmo snapshot e rollback deliberado. Captura exceções propagadas, erros SQL e avisos registrados; exceções internas sem log, como a leitura degradada de `DiaPago`, continuam sendo uma limitação observável do código da casa. **Validação em execução: [não verificado].** `app/ponto/services/espelho.py:409`.

```python
# Proposta para colar como um bloco no manage.py shell.
# Execucao e resultados no tenant: [nao verificado].
# Nenhuma funcao geradora de PDF, lavra ou comando --apply e chamada.

import logging
from collections import Counter, defaultdict
from datetime import date

_incidentes = set()
_handle_original = logging.Logger.handle


def _log_sem_arquivo(self, registro):
    # Somente memoria; nao encaminha mensagens/PII aos handlers.
    if registro.levelno >= logging.WARNING:
        exc = registro.exc_info
        classe = exc[0].__name__ if exc and exc[0] else (
            "Log" + registro.levelname
        )
        _incidentes.add(classe)


class _RollbackFinal(Exception):
    pass


class _LeituraDegradada(Exception):
    pass


def executar_fidelidade():
    from django.db import connection, transaction
    from django.utils import timezone
    from django_tenants.utils import schema_context
    from colaboradores.models import Colaborador, Empresa
    from folha.export import janela_competencia
    from ponto.turnos import (
        batidas_apuraveis, turnos_do_colab, dia_das_batidas,
    )
    from ponto.services.espelho import espelho_do_colab
    from relatorios.pdf_espelho import _coletar_dados_espelho
    from relatorios.cartao_pela_celula import ate_ontem

    empresas_ids = (2, 3, 4)
    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    falhas = set()
    falhas_empresa = Counter()
    medidos = Counter()
    totais = defaultdict(lambda: {
        "contagem": 0, "pessoas": set(),
        "empresa": Counter(), "pessoas_empresa": defaultdict(set),
        "exemplos": [],
    })
    classes = (
        "a", "b", "c", "d", "e", "borda_a",
        "universo_apuravel", "impressas",
        "sem_turno", "juiz_multidia",
        "repetida_mesmo_dia", "borda_elegiveis",
    )

    def vigiar_sql(execute, sql, params, many, context):
        try:
            return execute(sql, params, many, context)
        except Exception as erro:
            _incidentes.add(type(erro).__name__)
            raise

    def contar_tabelas(rotulo):
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT table_name
                  FROM information_schema.tables
                 WHERE table_schema = %s
                   AND table_type = 'BASE TABLE'
                 ORDER BY table_name
            """, ["juliani"])
            tabelas = [r[0] for r in cursor.fetchall()]
            if not tabelas:
                raise RuntimeError("SchemaSemTabelas")
            q = connection.ops.quote_name
            contagens = {}
            for tabela in tabelas:
                cursor.execute(
                    "SELECT COUNT(*) FROM %s.%s"
                    % (q("juliani"), q(tabela))
                )
                contagens[tabela] = cursor.fetchone()[0]
        print("TABELAS_%s quantidade=%d" % (rotulo, len(contagens)))
        for tabela, quantidade in contagens.items():
            print("%s juliani.%s=%d" % (rotulo, tabela, quantidade))
        return contagens

    def medir_pessoa(colab, ini, fim, dias, apuraveis, pdf=False):
        # Universo de apresentacao: autoridade real, sem copiar seu
        # tratamento de orfas/espurias/borda.
        # app/ponto/turnos.py:2898,2957
        turnos = turnos_do_colab(colab, ini, fim)
        mapa = dia_das_batidas(colab, ini, fim, turnos=turnos)

        esperadas = {
            pk: b for pk, b in apuraveis.items()
            if b.timestamp <= agora
            and ini <= mapa.get(pk, date.min) <= fim
        }

        juiz = defaultdict(set)
        bordas = set()
        for turno in turnos:
            for b in turno.batidas:
                juiz[b.pk].add(turno.data_turno)
            if (
                turno.data_turno == fim
                and turno.cross_meianoite
                and turno.saida is not None
                and turno.saida.pk in esperadas
            ):
                bordas.add(turno.saida.pk)

        # Nao recorta antes de indexar: batida presente em outro dia
        # retornado deve entrar em c, em vez de virar falsa ausencia.
        impressas = defaultdict(list)
        objetos = {}
        for dia in dias:
            for b in dia["batidas"]:
                if b.pk is None:
                    raise ValueError("MarcacaoSemPK")
                impressas[b.pk].append(dia["data"])
                objetos.setdefault(b.pk, b)

        alvo = set(esperadas) | {
            pk for pk, datas in impressas.items()
            if any(ini <= d <= fim for d in datas)
        }
        achados = {classe: {} for classe in classes}

        def incluir(classe, pk, dia, b):
            achados[classe][pk] = (
                dia, timezone.localtime(b.timestamp).isoformat()
            )

        faltantes = set(esperadas) - set(impressas)
        for pk, b in esperadas.items():
            incluir("universo_apuravel", pk, mapa[pk], b)
            if pk in faltantes:
                incluir("a", pk, mapa[pk], b)

        for pk in bordas:
            incluir("borda_elegiveis", pk, fim, esperadas[pk])
            if pk in faltantes:
                incluir("borda_a", pk, fim, esperadas[pk])

        for pk in sorted(alvo & set(impressas)):
            b = objetos[pk]
            datas = impressas[pk]
            dias_distintos = set(datas)
            primeiro_dia = min(datas)
            incluir("impressas", pk, primeiro_dia, b)

            if len(dias_distintos) > 1:
                incluir("b", pk, primeiro_dia, b)
            if any(n > 1 for n in Counter(datas).values()):
                incluir("repetida_mesmo_dia", pk, primeiro_dia, b)

            dias_juiz = juiz.get(pk, set())
            if not dias_juiz:
                incluir("sem_turno", pk, primeiro_dia, b)
            else:
                if len(dias_juiz) > 1:
                    incluir("juiz_multidia", pk, primeiro_dia, b)
                errados = [
                    d for d in datas
                    if any(d != esperado for esperado in dias_juiz)
                ]
                if errados:
                    incluir("c", pk, min(errados), b)

            if pk not in apuraveis and not getattr(b, "procedencia", None):
                incluir("d", pk, primeiro_dia, b)

            if pdf:
                # Usa o atributo posto pelo coletor; nao refaz HH:MM.
                # app/relatorios/pdf_espelho.py:604-609
                if not hasattr(b, "orfa"):
                    raise AttributeError("ColetorSemFlagOrfa")
                if b.orfa:
                    incluir("e", pk, primeiro_dia, b)

        return achados

    def acumular(superficie, colab, achados):
        for classe, casos in achados.items():
            if superficie == "ESPELHO_ATE_HOJE" and classe == "e":
                continue
            total = totais[(superficie, classe)]
            total["contagem"] += len(casos)
            if casos:
                total["pessoas"].add(colab.pk)
                total["empresa"][colab.empresa_id] += len(casos)
                total["pessoas_empresa"][colab.empresa_id].add(colab.pk)
            for pk, (dia, instante) in sorted(casos.items()):
                if len(total["exemplos"]) >= 3:
                    break
                total["exemplos"].append(
                    "col%s %s batida=%s instante=%s"
                    % (colab.pk, dia.isoformat(), pk, instante)
                )

    if connection.in_atomic_block:
        raise RuntimeError("ExigeShellForaDeTransacao")

    with schema_context("juliani"):
        try:
            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL "
                        "REPEATABLE READ, READ ONLY"
                    )
                    cursor.execute("SELECT current_schema()")
                    if cursor.fetchone()[0] != "juliani":
                        raise RuntimeError("SchemaIncorreto")

                antes = contar_tabelas("ANTES")
                try:
                    empresas = Empresa.objects.in_bulk(empresas_ids)
                    if set(empresas) != set(empresas_ids):
                        raise LookupError("EmpresaSolicitadaAusente")

                    janelas = {}
                    for eid in empresas_ids:
                        ini, fim_competencia = janela_competencia(
                            empresas[eid], 11, 2026
                        )
                        fim = min(fim_competencia, hoje)
                        if fim < ini:
                            raise ValueError("CompetenciaAindaNaoIniciada")
                        fim_pdf = max(ini, ate_ontem(fim, hoje=hoje))
                        janelas[eid] = (ini, fim, fim_pdf)
                        print(
                            "JANELA emp%s competencia=11/2026 "
                            "inicio=%s fim_competencia=%s "
                            "espelho_ate=%s pdf_ate=%s"
                            % (eid, ini, fim_competencia, fim, fim_pdf)
                        )

                    frota = list(
                        Colaborador.objects.filter(
                            empresa_id__in=empresas_ids,
                            situacao="ativo",
                        ).select_related("empresa").order_by("pk")
                    )
                    frota_empresa = Counter(c.empresa_id for c in frota)
                    print("INSTANTE", timezone.localtime(agora).isoformat())
                    print("FROTA", len(frota), dict(frota_empresa))
                    if not frota:
                        raise ValueError("UniversoVazio")

                    for colab in frota:
                        _incidentes.clear()
                        try:
                            # Savepoint: erro SQL de uma pessoa nao
                            # inutiliza a transacao das demais.
                            with transaction.atomic():
                                with connection.execute_wrapper(vigiar_sql):
                                    ini, fim, fim_pdf = janelas[colab.empresa_id]

                                    # Vida inteira para d nao confundir
                                    # fora da janela com nao apuravel.
                                    # app/ponto/turnos.py:2879
                                    apuraveis = {
                                        b.pk: b
                                        for b in batidas_apuraveis(colab)
                                    }

                                    espelho = espelho_do_colab(
                                        colab, hoje, ini, fim,
                                        ano=2026, agora=agora,
                                    )
                                    resultado_espelho = medir_pessoa(
                                        colab, ini, fim,
                                        espelho["dias"], apuraveis,
                                    )

                                    # O coletor chama o montador real e
                                    # atribui o flag exato usado no papel.
                                    # app/relatorios/pdf_espelho.py:596,604
                                    pdf = _coletar_dados_espelho(
                                        colab, ini, fim, agora=agora
                                    )
                                    resultado_pdf = medir_pessoa(
                                        colab, ini, fim_pdf,
                                        pdf["dias"], apuraveis, pdf=True,
                                    )

                                    if _incidentes:
                                        raise _LeituraDegradada()

                            # So publica a pessoa depois das duas leituras.
                            acumular(
                                "ESPELHO_ATE_HOJE", colab, resultado_espelho
                            )
                            acumular("PDF_REAL", colab, resultado_pdf)
                            medidos[colab.empresa_id] += 1

                        except Exception as erro:
                            falhas.add(colab.pk)
                            falhas_empresa[colab.empresa_id] += 1
                            print(
                                "ERRO col%s emp%s classe=%s internas=%s"
                                % (
                                    colab.pk, colab.empresa_id,
                                    type(erro).__name__,
                                    ",".join(sorted(_incidentes)) or "-",
                                )
                            )
                            

[truncado em 20000 de 22503 caracteres]

## 17/11 15:15 · sessao 51bc0855b4b9 · E4C-F1-B2-CERCA · verificador · origem=Code

**REPROVA.** Há falhas na seleção do dia consultado, no alcance da guarda e na certificação por mutantes/AST. Os caminhos síncronos dos antigos achados 1 e 2 passaram a abrir cerca. (`app/chamados/services/fio.py:1409`; `app/chamados/models.py:3576`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:565`; `selo_diff.txt:+102`; `app/chamados/services/disputa_emissao.py:1909`)

Nas referências ao selo, `+N` indica a linha do arquivo novo apresentado no anexo. Execução integral da suíte, resultados dos logs citados na triagem e ocorrência dos contraexemplos em produção: **[nao verificado]**. [nao verificado]

**1. Contorno por retenção — NAO PROCEDE como falha remanescente nesse caminho.**

O fallback continua sendo chamado quando a retratação retorna falsa por `retencao` ou `transicao`, mas agora entra na porta pública do resolver; essa porta trava e executa o corpo sob cerca. A recusa volta no dicionário e o comando a conta antes do `continue`. (`app/ponto/management/commands/reconciliar_fantasmas.py:288`; `app/chamados/services/disputa_emissao.py:1903`; `app/chamados/services/disputa_emissao.py:1909`; `app/chamados/services/disputa_emissao.py:1914`; `app/ponto/management/commands/reconciliar_fantasmas.py:291`)

As escritas ordinárias de perguntas existentes desse resolver passam pelo `save` vigiado; o descarimbo em lote também tem vigilância. Não identifiquei, nesse fluxo síncrono, escrita do resolver pelo bulk de lavras. (`app/chamados/models.py:3572`; `app/chamados/models.py:3401`; `app/chamados/services/disputa_emissao.py:2019`; `app/chamados/services/disputa_emissao.py:2093`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`)

**2. Resolver sem cerca — NAO PROCEDE para as entradas nomeadas.**

| Entrada | Caminho efetivo |
|---|---|
| Beco | O callback inclui resolvers e `d.fechar`, dentro da cerca do beco. (`app/chamados/services/becos.py:55`; `app/chamados/services/becos.py:66`) |
| HIST | Costura e resolver estão no mesmo callback cercado. (`app/chamados/reconciliador.py:1690`; `app/chamados/reconciliador.py:1701`) |
| S143 | Chama a porta pública, que abre a cerca do resolver. (`app/chamados/signals.py:117`; `app/chamados/services/disputa_emissao.py:1909`) |
| Retratação | Chama o resolver dentro da cerca externa da unidade. (`app/chamados/reconciliador.py:553`; `app/chamados/reconciliador.py:580`) |
| Fantasmas | Chama a mesma porta pública. (`app/ponto/management/commands/reconciliar_fantasmas.py:288`) |

As chamadas de produção encontradas ao corpo privado estão na própria porta: a chamada cercada e a exceção da via trancada. Nessa exceção, o corpo retorna antes das consultas e escritas de perguntas. Portanto, **chamar o corpo sem cerca nessa via não demonstra bypass de escrita**. (`app/chamados/services/disputa_emissao.py:1897`; `app/chamados/services/disputa_emissao.py:1909`; `app/chamados/services/disputa_emissao.py:1934`)

**3. Sobrealcance — PROCEDE na porta; a cura não seleciona somente aquilo que será persistido.**

O `save` chama a vigilância antes de encaminhar `update_fields`. A vigilância examina o objeto inteiro em memória e a data gravada, sem receber os campos efetivamente enviados ao banco. (`app/chamados/models.py:3572`; `app/chamados/models.py:3576`; `app/chamados/models.py:3581`; `app/chamados/services/fio.py:1403`; `app/chamados/services/fio.py:1409`)

Isso permite dois contraexemplos estáticos:

- Pergunta ENTREGUE com `save(update_fields=[])`: a guarda pode recusar antes de chegar à operação que não persistiria campos. (`app/chamados/models.py:3576`; `app/chamados/models.py:3581`)
- Banco com dia ABERTO, objeto com dia ENTREGUE somente em memória e `update_fields=['pergunta']`: a guarda consulta também o dia ENTREGUE, embora `data_evento` não fosse gravada. (`app/chamados/services/fio.py:1409`; `app/chamados/services/fio.py:1411`; `app/chamados/models.py:3581`)

O descarimbo individual e o lote também não eliminam previamente linhas cujos campos já estejam nos valores finais. Assim, podem consultar o veto para um descarimbo sem mudança de valores. (`app/chamados/models.py:3327`; `app/chamados/models.py:3333`; `app/chamados/models.py:3401`; `app/chamados/models.py:3406`)

Os controles HX evitam o sobrealcance anteriormente provocado pela previsão; isso não prova ausência dos contraexemplos acima em toda retratação real. Ocorrência integrada desses padrões em produção: **[nao verificado]**. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:331`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:345`)

**4. Mordida dos testes — PROCEDE parcialmente: alvo corrigido, certificação ainda defeituosa.** [nao verificado]

A asserção específica foi implementada: compara a linha completa pelo PK da pergunta-alvo antes das outras fotografias; a classificação `ALVO` também exige esse PK na mensagem. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:249`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:258`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:535`)

| Ponto | Conclusão |
|---|---|
| `sem_escopo:fechar_beco = {}` | **Coerente com o caso, mas sem mordida.** Retirar apenas a cerca do beco preserva a cerca interna do resolver. Falta caso que exija a proteção adicional do `d.fechar`. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:435`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:563`; `app/chamados/services/becos.py:60`; `app/chamados/services/disputa_emissao.py:1909`) |
| `engolido_sobrevive → e=ALVO` | **Incoerente.** No caso `e`, o veto direto sobe ao `except` do próprio mutante, que devolve as vetadas. O wrapper registra a recusa. A guarda continua anterior à persistência; o fluxo desse caso deve permanecer verde, não escrever o alvo. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:94`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:418`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:565`; `app/chamados/services/disputa_emissao.py:1913`; `app/chamados/models.py:3576`) |
| Caso `s` | **C21 completo:** texto não vazio, `respondida_em` e validação. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:382`) |
| Geofence | **Rótulo coerente:** o relatório particiona pelos mesmos três motivos impressos pelo comando. (`app/ponto/services/geofence.py:444`; `app/ponto/services/geofence.py:448`; `app/ponto/management/commands/reconciliar_geofence.py:46`) |

A fotografia também tem limites: decisões, pautas, ocorrências e auditoria são comparadas por contagem; a auditoria exclui todas as ações `retratacao_retida_*`. Ela não demonstra identidade integral do conteúdo dessas tabelas. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:215`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:219`)

**5. Quem escreve e quem lê — PROCEDE a existência de escritor sem vigilância, declarado como backfill.**

O mapa abaixo reúne os campos explícitos do diff e dos caminhos examinados. Enumeração global de todos os leitores e escritores indiretos da aplicação: **[nao verificado]**.

| Modelo / campos | Escritores | Leitores relevantes |
|---|---|---|
| Pergunta: `disputa`, `motivo`, `pergunta`, `tipo_resposta`, `conversa` | Nascimento em `app/chamados/models.py:3134`; fixtures em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:146` | Guarda em `app/chamados/services/fio.py:1408`; HIST em `app/chamados/reconciliador.py:1602`; leitura integral da pergunta em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:258` |
| Pergunta: `celula`, `chamado_destino` | Nascimento/reuso em `app/chamados/models.py:3086`, `app/chamados/models.py:3102`, `app/chamados/models.py:3134`; costura em `app/chamados/services/fio.py:974` | Seleção direta em `app/chamados/services/disputa_emissao.py:1939`; HX em `app/chamados/services/disputa_emissao.py:2069`; relavra em `app/chamados/models.py:309` |
| Pergunta: `data_evento` | Atribuição na porta em `app/chamados/services/fio.py:849`; costura em `app/chamados/services/fio.py:928`; nascimento em `app/chamados/models.py:3136` | Guarda em `app/chamados/services/fio.py:1403`, `app/chamados/services/fio.py:1409`; HIST em `app/chamados/reconciliador.py:1612`; resolver em `app/chamados/services/disputa_emissao.py:2019` |
| Pergunta: `materializada_em`, `materializada_por`, `via_resolucao` | Porta do carimbo em `app/chamados/models.py:3220`; persistências em `app/chamados/models.py:3273`, `app/chamados/models.py:3311`, `app/chamados/models.py:3333`; lote em `app/chamados/models.py:3406` | Resolver em `app/chamados/services/disputa_emissao.py:1941`, `app/chamados/services/disputa_emissao.py:2073`, `app/chamados/services/disputa_emissao.py:2083`; exclusão do lote em `app/chamados/models.py:3402` |
| Pergunta: `resposta_colab`, `respondida_em` | Resposta em `app/chamados/services/disputa_emissao.py:1481`; automática em `app/chamados/services/disputa_emissao.py:1674`; reparo em `app/chamados/services/disputa_emissao.py:1696`; carimbo em `app/chamados/models.py:3253`; limpeza em `app/chamados/models.py:3408` | C21 em `app/chamados/juizes.py:1552`; resolver em `app/chamados/services/disputa_emissao.py:2013`; HX em `app/chamados/services/disputa_emissao.py:2090`; porta do par em `app/chamados/models.py:3603` |
| Pergunta: `validada_em`, `validada_por` | Validação em `app/chamados/services/validacao.py:168`; devolução em `app/chamados/services/validacao.py:273`; nova resposta em `app/chamados/services/disputa_emissao.py:1494`; carimbo em `app/chamados/models.py:3256`; limpeza em `app/chamados/models.py:3408` | Relavra em `app/chamados/models.py:310`; HX em `app/chamados/services/disputa_emissao.py:2073`; nova resposta em `app/chamados/services/disputa_emissao.py:1484` |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar` em `app/chamados/veredito_lavrado.py:270`; backfill em `app/chamados/management/commands/backfill_veredito_lavrado.py:138`; bulk em `app/chamados/veredito_lavrado.py:250` | `esta_stale` em `app/chamados/veredito_lavrado.py:224`; fotografia em `app/chamados/veredito_lavrado.py:281`; teste em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:258` |
| Chamado: `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`, `atualizado_em` | `_gravar_estado` em `app/chamados/models.py:228`, `app/chamados/models.py:231`, `app/chamados/models.py:244`; persistência em `app/chamados/models.py:296` | Resolver em `app/chamados/services/disputa_emissao.py:1896`, `app/chamados/services/disputa_emissao.py:2066`; S143 em `app/chamados/signals.py:97`; fotografia em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:211` |
| Chamado: `urgencia`, `urgencia_original` | Ritual em `app/chamados/models.py:460`; restauração em `app/chamados/models.py:251`; update do receiver em `app/chamados/models.py:637` | Rebaixamento em `app/chamados/models.py:469`; fotografia em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:211` |
| Chamado: `colaborador`, `justificativa_relacionada`, `categoria`, `subcategoria`, `modulo_origem`, `contexto_json`, `assunto`, `titulo`, `area` | Nascimento parametrizado em `app/chamados/models.py:1142`; fixtures em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:139` | Retratação em `app/chamados/reconciliador.py:550`, `app/chamados/reconciliador.py:573`; HIST em `app/chamados/reconciliador.py:1633`, `app/chamados/reconciliador.py:1642`; fotografia em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:211` |
| Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | Fecho em `app/chamados/models.py:2023`; reaberturas em `app/chamados/models.py:2240`, `app/chamados/models.py:2264` | Beco em `app/chamados/services/becos.py:59`; resolver em `app/chamados/services/disputa_emissao.py:2063`; receivers em `app/chamados/signals.py:29`, `app/chamados/signals.py:165` |
| Disputa: `colaborador`, `chamado`, `motivos`, `contexto` | Emissão em `app/chamados/services/disputa_emissao.py:550`; alterações em `app/chamados/models.py:2057`, `app/chamados/models.py:2154`, `app/chamados/models.py:2195`; fixtures em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:201` | Guarda em `app/chamados/services/fio.py:1414`; beco em `app/chamados/services/becos.py:44`; HIST em `app/chamados/reconciliador.py:1605`; resolver em `app/chamados/services/disputa_emissao.py:2061` |
| Auditoria: `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `usuario`, `ip`, `ator` | `evento` em `app/core/observ.py:134`; adaptador em `app/core/observ.py:146`; retenção prepara e delega em `app/chamados/reconciliador.py:478`, `app/chamados/reconciliador.py:493` | Deduplicação lê ação/modelo/objeto, `id` e payload em `app/chamados/reconciliador.py:489`; testes filtram ações em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:219`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:224` |

**Bypass concreto:** `gravar_lavras_em_lote` usa `bulk_update` sem `vigiar`. Os chamadores encontrados são os dois pontos do backfill; não encontrei ligação dos atos cercados até esse escritor. (`app/chamados/veredito_lavrado.py:250`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`; `app/chamados/management/commands/backfill_veredito_lavrado.py:146`)

O `update` de descarimbo passa pela guarda; os `save` de nascimento não passam, por decisão explícita da implementação. Ausência absoluta de escrita dinâmica/raw: **[nao verificado]**. (`app/chamados/models.py:3401`; `app/chamados/models.py:3406`; `app/chamados/models.py:3572`)

**6. Classes recorrentes — PROCEDE a falha semântica; ordem dos abridores não é uniforme.**

**ESMERIL-SEMANTICA:** o helper utilizado pela guarda simplesmente devolve `data_evento`. Entretanto, o próprio módulo distingue essa chave gravada da identidade do fato, obtida pelo juiz do turno para linhas com episódio. A migração reconhece dia da saída diferente do dia do turno e preserva grupos que toquem competência exportada. Logo, o código não garante que a chave consultada tenha sempre o significado exigido pelo veto. (`app/chamados/juizes.py:342`; `app/chamados/services/fio.py:1409`; `app/chamados/services/fio.py:1605`; `app/chamados/services/fio.py:1620`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:17`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:30`)

Contraexemplo estático: turno de 23/09 ENTREGUE, saída em 24/09 ABERTA e pergunta legada com `data_evento=24/09`. Uma pergunta materializada, não validada, pode ser relavrada pela transição do destino; a guarda consulta somente 24/09 e libera. Existência desse caso em produção: **[nao verificado]**. (`app/chamados/models.py:305`; `app/chamados/models.py:310`; `app/chamados/veredito_lavrado.py:275`; `app/chamados/services/fio.py:1409`; `app/chamados/services/vigia_fio.py:802`)

| Abridor | Savepoint da cerca em relação à trava |
|---|---|
| Retratação | Depois da trava. (`app/chamados/reconciliador.py:552`; `app/chamados/reconciliador.py:553`) |
| Beco | Depois da trava. (`app/chamados/services/becos.py:52`; `app/chamados/services/becos.py:66`) |
| Resolver | Depois da trava. (`app/chamados/services/disputa_emissao.py:1903`; `app/chamados/services/disputa_emissao.py:1909`) |
| HIST | Antes da trava interna da costura. (`app/chamados/reconciliador.py:1701`; `app/chamados/reconciliador.py:1691`; `app/chamados/services/fio.py:938`) |

Essa diferença do HIST, isoladamente, não prova deadlock. Deadlock observado: **[nao verificado]**.

Não nasceu outro juiz de competência: a guarda chama o veto existente, que compõe janela e K6. As funções novas da cerca não leem relógio; os caminhos continuam tendo `now()` internos no resolver e na lavra. Portanto, “nenhum relógio fora da borda” não foi demonstrado. (`app/chamados/services/fio.py:1419`; `app/chamados/services/fio.py:1365`; `app/chamados/services/vigia_fio.py:800`; `app/chamados/services/disputa_emissao.py:2019`; `app/chamados/services/disputa_emissao.py:2076`; `app/chamados/veredito_lavrado.py:271`)

A `_CERCA` recebe estado por abertura e restaura o token em `finally`; aninhamentos reutilizam o estado externo. Não encontrei fundamento nesse código para afirmar vazamento normal entre requests, threads independentes ou testes. Certificação no ambiente real: **[nao verificado]**. (`app/chamados/services/fio.py:1433`; `app/chamados/services/fio.py:1435`; `app/chamados/services/fio.py:1448`)

Cumprimento dos pré-requisitos de pouso: **[nao verificado]**. [nao verificado]

**7. Selo AST — PROCEDE a insuficiência da adaptação como certificação.**

A implementação consulta na porta, depois de a transição já ter iniciado; o rollback substitui a consulta anterior à transição. Isso não implementa o instante literal descrito no aval reproduzido na triagem. (`app/chamados/reconciliador.py:553`; `app/chamados/reconciliador.py:567`; `app/chamados/models.py:3576`)

Quanto aos requisitos do próprio selo:

- **(a) Insuficientemente provado.** O selo aceita chamada sob `if False` antes do save; procura qualquer `If/Raise` para a guarda e qualquer `With/If/Raise` para a cerca, sem demonstrar a condição de veto ou o `atomic` correto. O abridor só precisa mencionar as vetadas em algum `if`. (`selo_diff.txt:+277`; `selo_diff.txt:+290`; `selo_diff.txt:+296`; `selo_diff.txt:+239`)
- **(b) Falha concreta de subaproximação.** `_indexa` e `_liga` não atravessam `if`/`with` para indexar definições. Perdem `_ato` do beco e `_costura_e_fecho` do HIST; o corpo desses callbacks também é excluído do chamador. Callbacks sem definição são depois ignorados. Esse limite não é o limite declarado de atributos homônimos. (`selo_diff.txt:+102`; `selo_diff.txt:+139`; `selo_diff.txt:+148`; `selo_diff.txt:+308`; `app/chamados/services/becos.py:42`; `app/chamados/services/becos.py:55`; `app/chamados/reconciliador.py:1603`; `app/chamados/reconciliador.py:1690`)
- **(c) Coerente para `lavrar(salvar=False)`:** a persistência fica no save do chamador, mas isso depende de o grafo encontrar esse chamador. (`selo_diff.txt:+163`; `app/chamados/services/fio.py:975`; `app/chamados/services/fio.py:976`)
- **(d) Coerente tratar `registrar_retencao` como trilha:** ela delega a escrita de auditoria, não escreve pergunta. (`selo_diff.txt:+47`; `app/chamados/reconciliador.py:493`)

Os 14 nomes declarados têm explicações compatíveis com entradas avulsas sem cobertura integral. Os dois nomes do resolver incluem falsos positivos conservadores da via trancada, cujo corpo não escreve. A quantidade efetivamente emitida pelo selo completo: **[nao verificado]**. Essa lista não certifica completude, pois os callbacks acima desaparecem do grafo. (`app/chamados/services/disputa_emissao.py:1897`; `app/chamados/services/disputa_emissao.py:1934`; `selo_diff.txt:+102`; `selo_diff.txt:+308`)

**8. Matriz dos 15 sítios — PROCEDE uma lacuna de cobertura da unidade.**

| Sítio | Situação |
|---|---|
| 1. `retratar_com_motivo` | Abre cerca antes do corpo, depois da trava. (`app/chamados/reconciliador.py:552`; `app/chamados/reconciliador.py:553`) |
| 2. `fechar_beco` | Cerca resolver e fecho. (`app/chamados/services/becos.py:55`; `app/chamados/services/becos.py:66`) |
| 3. Resolver | Cerca o corpo; via trancada retorna sem escrita. (`app/chamados/services/disputa_emissao.py:1909`; `app/chamados/services/disputa_emissao.py:1934`) |
| 4. `reconciliar` | Herda cerca quando aninhado; avulso escreve estado/relavra. (`app/chamados/models.py:655`; `app/chamados/models.py:296`; `app/chamados/models.py:312`) |
| 5. `transicionar` | Cobertura herdada; converge no escritor de estado. (`app/chamados/models.py:450`; `app/chamados/models.py:296`) |
| 6. `encerrar` | Cobertura herdada. (`app/chamados/models.py:1686`; `app/chamados/models.py:296`) |
| 7. Encerramento por tranca | Via especial; resolver não escreve. (`app/chamados/models.py:1702`; `app/chamados/services/disputa_emissao.py:1934`) |
| 8. `reabrir` | Descarimbo vigiado quando há cerca externa. (`app/chamados/models.py:1639`; `app/chamados/models.py:3401`) |
| 9. `Disputa.fechar` | Herda cerca; avulso pode escrever perguntas e disputa. (`app/chamados/models.py:2012`; `app/chamados/models.py

[truncado em 20000 de 22820 caracteres]

## 17/11 15:14 · sessao 65345d776547 · - · - · origem=Ronald (tmux)

Item 4: o comportamento já estava descrito, mas não foi tratado como defeito. Literais: v2 §3, “O leitor decisório de motivo encontrado em `materializacao.py` restringe-se a DEVOLUCAO BUG-PAR, não RESPOSTA”; v2 §6, “permanece o reconhecimento textual de DEVOLUCAO BUG-PAR, fora dessa cura”. A cópia confirma `(d.ato_motivo or '').startswith(MOTIVO_BUG_PAR_103)` em `app/chamados/services/materializacao.py:275`. Recomendo **(a)**: tema presente; contradição não apontada. “8-11: conferencia, sem defeito apontado” da triagem v1 não documenta descarte desse defeito.

Item 2: v2 §6 já afirma literalmente “Código novo executado contra banco sem 0055 é incompatível, pois já exige a coluna”. Assim, **não cabe dizer que a incompatibilidade geral passou despercebida**. O mecanismo específico merge → nascimento de cron → migrate não é explicitado; **(a)** só cabe delimitando essa omissão. Nenhuma triagem fornecida documenta descarte dessa janela inversa; forçar **(b)** seria inadequado. [nao verificado]

A cópia confirma árvore como bind mount (`bin/deploy.sh:95`), migration (`bin/deploy.sh:317`) antes do HUP (`bin/deploy.sh:360`) e que comandos via exec, incluindo cron, não recebem esse HUP (`bin/deploy.sh:329`).

Ressalvas para consolidação:

- `models.py:4175/4182` do laudo são linhas da raia, não desta cópia: localmente pertencem a `RegularizacaoExterna`. Não usar como verificação local da alegada contradição.
- O laudo chama `curar_perguntas_em_limbo` de cron; nesta cópia ele aparece como cura de execução única, “--apply e ato do Ronald” (`app/config/crons.py:1476`). Sua chamada à porta existe (`app/chamados/management/commands/curar_perguntas_em_limbo.py:67`); agendamento rotineiro e ocorrência efetiva são **[nao verificado]**.

## 17/11 15:14 · sessao e7054ee1dc3a · - · - · origem=Ronald (tmux)

Item 1: automática e trilha cabem em **(b)**: v5 identifica literalmente “RESPOSTA automática ou pela trilha [...] inserida pelo código antigo após o backfill”; a triagem registra “PROCEDENTE, FORA DA CONDICAO 1” e “Nao aplicado (escopo do aval literal: condicao 1)”. (`v5.r`, item 5; `triagem_v5.txt:5–9`, numeração a partir da primeira linha do anexo.) [nao verificado]

A foto da projeção cabe em **(a)**: o tema aparece no censo — “automática, carimbo pela trilha e foto da projeção escrevem `True`” —, mas o contraexemplo da janela nomeia apenas automática e trilha. Portanto, “TERCEIRO nao citado pelo Codex” precisa significar não citado **como escritor antigo afetado pela janela**, pois o montador foi citado nas respostas. (`v5.r`, item 5; `v2.r`, seção 2; `v3.r`, seção 5.)

Código antigo confirmado na cópia:

- Automática: RESPOSTA com motivo e sem campo novo em `app/chamados/services/disputa_emissao.py:1678–1679`; entrada em `app/api/foto_recebida.py:48`.
- Trilha: RESPOSTA com motivo e sem campo em `app/chamados/services/disputa_emissao.py:1699–1700`; chamada em `app/chamados/management/commands/curar_perguntas_em_limbo.py:67–68`.
- Projeção: RESPOSTA com motivo em `app/chamados/models.py:4100–4101`; usos em `app/chamados/services/validacao.py:167/272/345`, `app/chamados/services/disputa_emissao.py:1480/1673`, `app/chamados/models.py:1613/3129`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.

O deploy migra em `bin/deploy.sh:317` e envia HUP em `bin/deploy.sh:360`; seus comentários explicam que o comando por `docker exec` não recebe o sinal em `bin/deploy.sh:329–330`. A imutabilidade antiga consta em `app/chamados/models.py:4070–4072`.

Ressalva relevante ao laudo: `curar_perguntas_em_limbo` está listado como cura que roda “UMA VEZ” e exige ato do Ronald em `app/config/crons.py:1476–1477`; `:1482` identifica outro comando. Seu agendamento efetivo como cron é **[nao verificado]**. A ancestralidade dos hashes também é **[nao verificado]**, pois a cópia não contém repositório Git acessível.

## 17/11 15:13 · sessao 515c143223d7 · - · - · origem=Ronald (tmux)

- **L-130 desatualizada na cópia:** texto literal `SEM ato de devolucao` e ``RESSUSCITAR-DEVOLVE-AO-COLABORADOR` e lei NAO decidida` em `app/docs/LEIS.md:151`.
- A busca literal por `RESSUSCITAR-GRAVA-ATO` e `RESPOSTAS-1711-1050` retornou zero ocorrências em `app/docs/CORTES.md` e `app/docs/CORTES.json`; os arquivos terminam em `:497` e `:1888`, respectivamente. A afirmação sobre a árvore **c8f58df30** continua [nao verificado]; esta é a cópia main fornecida.
- Nas respostas v1/v2/v3/v5 fornecidas, não há análise da pendência documental da L-130, ausência desses cortes ou condição 2 do opus anterior. A existência de perguntas sobre isso é [nao verificado].
- `triagem_v1.txt`, item 6, menciona `CORTE-F2-RESSUSCITAR-GRAVA-ATO` como justificativa funcional; isso não demonstra pergunta ao Codex sobre o registro documental. Linha física do anexo [nao verificado].
- **Classe sugerida: (e), provisória — ausência do tema nas respostas; sem as .q, impossível distinguir (a) de (e).** Não há fundamento para afirmar categoricamente “não perguntado”. [nao verificado]

## 17/11 15:12 · sessao 7aa33ea3be7b · E4C-F2 · censo · origem=Code

**Os quatro montadores de RESPOSTA encontrados coincidem com a 0055 nos valores que produzem; essa equivalência não é uma obrigação do modelo.** A porta aceita as duas discordâncias, demonstradas nos testes. (`app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/models.py:4251`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`)

O censo histórico exato do **pai da obra** permanece `[nao verificado]`: o material disponível não fornece sua fonte integral. O teste que menciona a main `fbc5c76ce` contém uma comparação com resultado salvo, não a árvore daquele commit. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:2`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:10`)

1. **Quem escreve o campo nesta árvore**

   | Montador de RESPOSTA | Valor e regra | Evidência |
   |---|---|---|
   | `responder_pergunta_disputa_core` | `False`; resposta humana nova desfaz a validação anterior. Omite `ato_motivo`, cujo default é `''`. | `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/models.py:4162` |
   | `responder_automaticamente` | `True`; grava texto/instante sem zerar validação. Motivo fixo: `resposta automatica (fato entregue por outro caminho)`. | `app/chamados/services/disputa_emissao.py:1685`, `app/chamados/services/disputa_emissao.py:1686` |
   | `carimbar_resposta_pela_trilha` | `True`; carimba uma resposta cujo texto já existe, sem zerar validação. Motivo sempre prefixado por `pela trilha: `. | `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/services/disputa_emissao.py:1707`, `app/chamados/services/disputa_emissao.py:1708` |
   | `linhas_da_projecao_sem_linha` | `True` na foto RESPOSTA: existe `respondida_em`, mas falta linha correspondente ao mesmo instante. Usa `MOTIVO_PROJECAO_SEM_LINHA`. | `app/chamados/models.py:4224`, `app/chamados/models.py:4249`, `app/chamados/models.py:4251` |

   A escrita física comum é `registrar`: constrói `cls(**ln)` e executa `bulk_create`; não calcula o booleano pelo motivo. Campo omitido recebe `default=False`; INSERT que omite a coluna recebe `db_default=False`. (`app/chamados/models.py:4328`, `app/chamados/models.py:4185`)

   Os sítios que persistem as fotos produzidas pelo helper são: nascimento; ressuscitação; resposta humana e automática; validação; devolução; devolução BUG-PAR; tratamento do estoque L130; backfill E4b. (`app/chamados/models.py:3207`, `app/chamados/models.py:1650`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:354`, `app/chamados/services/validacao.py:439`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)

   Os demais atos omitem o campo e recebem `False`: REABERTURA compensadora; SILENCIO; RETRATACAO; REABERTURA/RECUSA; VALIDACAO; DEVOLUCAO; ABSORCAO; SILENCIO do backfill e do veredito da célula. (`app/chamados/models.py:1691`, `app/chamados/models.py:2083`, `app/chamados/models.py:2215`, `app/chamados/services/disputa_emissao.py:1639`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:354`, `app/chamados/services/materializacao.py:1048`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `app/ponto/services/veredito_celula.py:195`, `app/chamados/models.py:4185`)

   `save()` permite inserir uma instância nova, mas recusa regravar instância com PK; `update()` e `bulk_update()` do manager vivo também recusam. Não há nesses métodos uma regra adicional baseada em `ato_motivo`. (`app/chamados/models.py:4216`, `app/chamados/models.py:4115`, `app/chamados/models.py:4121`)

   A **0055** adiciona a coluna com `False` e faz o único UPDATE específico encontrado: `ato='resposta' AND ato_motivo!='' → True`, pelo modelo histórico de migrations. A **0054** adiciona apenas `da_ressuscitacao`; a **0051** atribui `ato='reabertura'` ao acervo renomeado. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:11`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:26`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:17`, `app/chamados/migrations/0051_decisao_pergunta.py:49`)

2. **Equivalência com o backfill**

   **Não são regras idênticas, linha a linha:** os escritores declaram o efeito da sua operação; a 0055 classifica pelo conteúdo de `ato_motivo`. Entretanto, os quatro montadores atuais produzem valores coincidentes: humana = motivo vazio/False; automática, trilha e projeção = motivo não vazio/True. Isso inclui expressamente `MOTIVO_PROJECAO_SEM_LINHA` e “resposta automatica”. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/models.py:4251`)

   **Nos quatro montadores de produção encontrados, nenhum grava o contrário do predicado. Pela porta genérica, ambas as discordâncias são permitidas:** a constraint só proíbe `True` fora de RESPOSTA. Os testes gravam motivo preenchido/False e motivo vazio/True, e exigem que o leitor siga o campo. (`app/chamados/models.py:4205`, `app/chamados/models.py:4328`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:143`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:150`)

   Logo, a 0055 mudaria o primeiro caso para `True`; deixaria o segundo `True`, pois ela não zera as linhas fora do filtro. A ocorrência desses casos em produção é `[nao verificado]`. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`)

3. **Quem lê; derivação versus persistência; momento**

   Os dois leitores diretos de decisão são `q_validada_pelo_ato`, que devolve um filtro SQL, e `_resposta_desfaz_validacao`, consumido por `leitura`. O `validada = None` dentro de `leitura` altera uma variável local; o resultado é `LeituraDoAto`, sem salvar a pergunta. (`app/chamados/models.py:3547`, `app/chamados/models.py:3557`, `app/chamados/models.py:4281`, `app/chamados/models.py:4299`, `app/chamados/models.py:4312`, `app/chamados/models.py:4315`)

   | Consumidor | Resultado |
   |---|---|
   | `estado`, `respondida`, `resposta_display` | Derivam rótulo, booleano e texto. (`app/chamados/models.py:3231`, `app/chamados/models.py:3782`, `app/chamados/models.py:3806`) |
   | `sem_veredito_entre` / `pode_fechar` | Derivam lista e autorização para fechar. (`app/chamados/models.py:2000`, `app/chamados/models.py:2011`) |
   | `_ancoradas`, `q_vivas` | Selecionam perguntas usando a face SQL. (`app/chamados/models.py:357`, `app/chamados/models.py:3609`) |
   | `_gravar_estado` | Usa a face para selecionar perguntas e chamar a lavra persistida. (`app/chamados/models.py:317`, `app/chamados/models.py:319`, `app/chamados/veredito_lavrado.py:275`) |
   | `ressuscitar_perguntas` | Lê a foto, grava REABERTURA, limpa resposta/validação/carimbo, emite evento e reabre disputa. (`app/chamados/models.py:1684`, `app/chamados/models.py:1696`, `app/chamados/models.py:1698`, `app/chamados/models.py:1701`, `app/chamados/models.py:1708`) |
   | `fechar` | Usa a leitura na foto do SILENCIO, grava fechamento e evento. (`app/chamados/models.py:2085`, `app/chamados/models.py:2095`, `app/chamados/models.py:2096`) |
   | `remover_motivo` | Usa o texto lido na RETRATACAO, carimba identidade e salva a disputa. (`app/chamados/models.py:2217`, `app/chamados/models.py:2220`, `app/chamados/models.py:2224`) |

   Há também leitura genérica para fotografia de todos os campos das decisões, sem decidir validação pelo booleano. (`app/chamados/services/foto_e4b.py:247`, `app/chamados/services/foto_e4b.py:280`)

   A cadeia roda em **request** — upload consulta `pode_fechar` e fecha; desarquivar chama ressuscitação —, em **signal** — `post_save` da pergunta agenda fechamento de beco no commit — e em **cron configurado** — consumidor de eventos, por minuto, alcança consultas e fechamentos de disputas. Execução efetiva desses caminhos em produção: `[nao verificado]`. (`app/api/foto_recebida.py:83`, `app/chamados/views_arquivar.py:86`, `app/chamados/signals.py:229`, `app/chamados/signals.py:240`, `app/chamados/services/becos.py:34`, `app/chamados/services/becos.py:58`, `app/config/crons.py:394`, `app/chamados/reconciliador.py:1368`, `app/chamados/reconciliador.py:613`)

   **C6 Python não lê esse booleano:** lê a projeção da pergunta. Seu filtro SQL `q_vivas`, nesta árvore, usa a validação pela sequência. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1404`, `app/chamados/models.py:3609`)

4. **Escritores no main anterior**

   **Censo certificado do pai: `[nao verificado]`.** Na árvore disponível, os produtores de RESPOSTA com motivo não vazio são os três da tabela: automática, trilha e foto da projeção. Atribuí-los integralmente ao pai seria extrapolar a evidência. (`app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/models.py:4251`)

   Os caminhos atuais comprovados são:

   - **Automática:** request de upload → kernel da foto → `responder_automaticamente`. (`app/api/views.py:860`, `app/api/views_core.py:844`, `app/api/foto_recebida.py:74`, `app/api/foto_recebida.py:48`)
   - **Trilha:** comando `curar_perguntas_em_limbo --apply`; o cadastro o classifica como cura de execução única, não rotina. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:64`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `app/config/crons.py:1482`)
   - **Projeção sem linha:** os sítios enumerados na pergunta 1; entre as entradas comprovadas estão request de desarquivamento e ressuscitação pelo cartório. A cadeia histórica completa request/signal/cron no pai permanece `[nao verificado]`. (`app/chamados/views_arquivar.py:86`, `app/ponto/services/cartorio.py:1674`)

5. **Divisão P1/P2**

   **Para o booleano, os quatro montadores atuais não apresentam incompatibilidade com o predicado do backfill.** A 0055 não tem corte por data, PK ou origem: respostas escritas entre P1 e P2, presentes na execução e com motivo não vazio, entram no UPDATE; respostas humanas ficam corretamente em `False`. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/models.py:4251`)

   A omissão das colunas por código antigo tem `db_default=False`; existe teste que simula esse INSERT. Isso não prova execução em produção nem todas as compatibilidades do pai `[nao verificado]`. (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:19`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:26`, `app/chamados/tests/test_f2_db_default_das_colunas_novas.py:25`)

   **Para a obra inteira, há uma dependência que impede declarar “leitor velho” genericamente seguro:** o escritor novo grava REABERTURA compensadora; a carga nova precisa ignorá-la antes de escolher o último ato. Se P1 conservar um leitor que entregue essa linha a `_pela_decisao`, ela vira `devolvida_ao_colaborador`. Essa consequência é inferida dos ramos presentes; a implementação exata do leitor no pai é `[nao verificado]`. O filtro da tarefa precisa acompanhar esse escritor no P1 para conservar esse comportamento. (`app/chamados/models.py:1691`, `app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:163`)

   Hoje AddField e backfill estão na mesma 0055; a divisão proposta exige separar essas operações. A cobertura acima vale para linhas presentes na execução do UPDATE, não demonstra o comportamento de um pouso concorrente efetivamente realizado `[nao verificado]`. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:22`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:28`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`)

1 — Quatro montadores de RESPOSTA; persistência por `registrar/bulk_create`; defaults False e UPDATE da 0055. (`app/chamados/models.py:4328`, `app/chamados/models.py:4185`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`)  
2 — Coincidem nos valores emitidos atualmente; equivalência universal não existe no contrato. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`)  
3 — Leitores centrais derivam; consumidores também persistem, em requests, signals e consumidor cronificado. (`app/chamados/models.py:4315`, `app/chamados/models.py:1698`, `app/chamados/signals.py:240`, `app/config/crons.py:394`)  
4 — Censo exato dos escritores e caminhos do pai da obra: `[nao verificado]`.  
5 — O intervalo não é excluído pelo backfill; o escritor da REABERTURA exige o filtro correspondente da tarefa no P1. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/services/tarefa_do_dia.py:426`)

## 17/11 15:12 · sessao 402d3089e3f6 · - · - · origem=Ronald (tmux)

Interfaces e limites confirmados, sem executar banco ou escrever arquivo:

- Tenant: `schema_context('juliani')`, padrão usado por `app/tenants/cron.py:7` e `app/tenants/cron.py:25`; os dados de tenant ficam em schema próprio, segundo `app/tenants/models.py:4`.
- Frota ativa: `Colaborador.objects.filter(empresa_id__in=(2, 3, 4), situacao='ativo').select_related('empresa').order_by('pk')`; FK empresa e campo situação em `app/colaboradores/models.py:321` e `app/colaboradores/models.py:323`, escolha ativo em `app/colaboradores/models.py:296`.
- Janela: `janela_competencia(empresa, 11, 2026)`, assinatura e delegação em `app/folha/export.py:47`; corte por empresa real em `app/ponto/janelas.py:36` e `app/ponto/janelas.py:54`.
- `batidas_apuraveis(colab, ini_datetime, fim_datetime)` aceita timestamps inclusivos, sempre exclui retratadas e inclui pendentes de DP: `app/ponto/turnos.py:2868`, `app/ponto/turnos.py:2871`, `app/ponto/turnos.py:2888`. Não passar datas diretamente; o filtro é `timestamp__gte`/`timestamp__lte`, em `app/ponto/turnos.py:2890` e `app/ponto/turnos.py:2892`.
- `turnos_do_colab(colab, ini_date, fim_date)` recebe datas e devolve turnos cujo `data_turno` pertence à janela; carrega pad real em `app/ponto/turnos.py:1850` e `app/ponto/turnos.py:1867`. Entrada e saída são objetos Batida, evidenciado por acesso a `.timestamp` em `app/ponto/turnos.py:209` e `app/ponto/turnos.py:740`.
- Montador exato que deixa as batidas prontas para impressão: `_coletar_dados_espelho(colab, data_ini, data_fim, _turnos_juiz=turnos, agora=agora)`, assinatura em `app/relatorios/pdf_espelho.py:187`; usa `espelho_do_colab` em `app/relatorios/pdf_espelho.py:596` e devolve os dias recortados em `app/relatorios/pdf_espelho.py:602`.
- Atenção à fronteira temporal: PDF real clipa o fim em ontem, em `app/relatorios/pdf_espelho.py:285`–`288`; `ate_ontem` é `min(data_fim, hoje-1)` em `app/relatorios/cartao_pela_celula.py:37`. Deve-se declarar janela pedida até hoje e janela efetivamente impressa até ontem; medir batidas de hoje contra PDF acusa perdas previstas pelo próprio teto.
- O PDF reescreve `b.orfa` por HH:MM da grade, em `app/relatorios/pdf_espelho.py:604`–`609`; só chamar `espelho_do_colab` não assegura o flag exato que o PDF imprimirá.
- A saída de turno noturno do último dia pode estar na tarde do dia seguinte; a classe é explicitamente documentada em `app/relatorios/pdf_espelho.py:271`–`281`. Não restringir o universo esperado exclusivamente ao timestamp dentro da janela: unir batidas dos turnos reclamados pelo juiz; `dia_das_batidas` já documenta fim+1 em `app/ponto/turnos.py:2919`.
- Borda pode ser identificada por `t.cross_meianoite`, `t.data_turno == fim`, `t.saida is not None`, `t.saida.pk in ausentes`; campos oficiais do Turno em `app/ponto/turnos.py:43` e construção em `app/ponto/turnos.py:323`.
- Identidade correta é PK; timestamp não deve ser chave, conforme contrato explícito em `app/ponto/turnos.py:2916`.
- `espelho_do_colab` pode montar dias anteriores à competência porque usa `min(piso_visual, apur_ini)`, em `app/ponto/services/espelho.py:1225`; o PDF filtra os dias em `app/relatorios/pdf_espelho.py:602`.
- Existem exceções engolidas internamente por funções reais: leitura de DiaPago em `app/ponto/services/espelho.py:409`–`412`, grade em `app/ponto/services/espelho.py:569`–`571`, palavra do dia em `app/ponto/services/espelho.py:645`–`651`. O script deve ao menos contabilizar exceções propagadas por pessoa; afirmar que captura todas as exceções internas seria incorreto.

Sugestão de defesa do script: transação externa com `SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY`, savepoint por pessoa para sobreviver a erro SQL, exceção sentinela ao final para rollback, e contagens de linhas de todas as tabelas do schema antes/depois. Essa montagem é proposta, não validada executando o ambiente. [não verificado]

## 17/11 15:11 · sessao 88bde2b706ce · - · - · origem=Ronald (tmux)

Não encontrei medidor que responda a conjunção inteira por identidade de batida na frota; os instrumentos próximos abaixo comparam fixtures, minutos ou resumos. A inexistência absoluta fora dos caminhos pesquisados permanece [não verificado].

| Instrumento | Universo e exclusões | Esperado / quem chama | Lacuna |
|---|---|---|---|
| `CartaoNaoPerdeBatidaTest` | Fixture de **um colaborador**, três turnos noturnos de junho/2026 e uma batida espúria, construídos em `app/relatorios/tests/test_cartao_nao_perde_batida.py:24` e `:39`. | `test_toda_batida_do_periodo_aparece_no_documento` chama `_coletar_dados_espelho`, compara `self.ids` com `b.pk`, espera conjunto faltante vazio (`:57`). `test_nao_imprime_batida_duplicada` espera comprimento da lista igual ao conjunto de PKs (`:67`). | Não consulta `batidas_apuraveis`, não mede frota e não compara cada dia com `turnos_do_colab`; os predicados efetivos estão em `:61` e `:71`. |
| `test_pdf_le_o_juiz_do_dia` | Um cenário 12x36 19h–07h, um colaborador construído no teste (`app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:40`). | Chama `turnos_do_colab` e espera `data_turno=11/09`, `cross_meianoite=True` (`:56`); outro teste inspeciona o código para exigir referência ao juiz e ausência da heurística antiga (`:74`). | O teste comportamental mostrado verifica o turno; o teste do PDF é estrutural, sem censo por identidade em produção (`:61`, `:84`). |
| `pdf_x_espelho_divergentes` | Todas as empresas, mas só pessoas com `classificar_export(status='entra')`; empresa cuja classificação falha é excluída e colaborador ausente é pulado (`app/relatorios/management/commands/pdf_x_espelho_divergentes.py:56`). | Chama `_coletar_dados_espelho` (`:73`) e `espelho_do_colab` (`:92`); compara por dia **soma de minutos e flag de aberto**, esperado zero divergentes (`:78`, `:97`, `:114`). É comando manual, não cron (`app/config/crons.py:1139`). | Não lê `dia['batidas']`, não compara PKs, completude, duplicidade ou proveniência; os campos comparados são somente os de `:83` e `:101`. Registrado em **E4-pdf-x-espelho** (`app/core/espelho_verdade.py:124`). |
| `selo_leitores_no_mesmo_numero` / `folha.porta_export.medir` | Empresas ativas no comando (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`); comparação tela/PDF só entre `classificar_export(status='entra')` (`app/folha/porta_export.py:469`). | O comando chama `medir` e **lavra** resultado (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88`). `medir` chama os leitores reais e compara cinco escalares do resumo, com tolerância 0,01 (`app/folha/porta_export.py:476`, `:496`). É selo de porta/manual, não pipeline agendado (`app/config/crons.py:887`). | Pode haver a mesma perda nos dois leitores com resumo coincidente; identidade das marcações não participa do predicado (`app/folha/porta_export.py:497`). Está relacionado ao **E4** (`app/core/espelho_verdade.py:120`) e é fonte do **R4** (`app/core/placar_estrutural.py:211`, `:414`). |
| `e6_oraculo` | Empresas ativas e colaboradores com `FechamentoMensal` na competência, opcionalmente restritos ao TXT (`app/ponto/management/commands/e6_oraculo.py:303`). Exclui dias de isenção/ausência, sem lavratura, zero nos dois e diferença de janela declarada (`:339`). | Compara minutos do espelho com calculadora independente, tolerância até 10 minutos (`:362`). Lê `batidas_apuraveis`, mas forma turnos com regra própria (`:8`). | Não confere marcação impressa por identidade nem usa `turnos_do_colab` como autoridade do esperado para cada PK. Itens **E6-sistema-bate**, **E6-batida-impar** (`app/core/espelho_verdade.py:180`, `:188`); valor é termômetro **R1** no estrutural (`app/core/placar_estrutural.py:58`). |
| `esmeril_espelho` | `colaboradores_em_operacao`, exclui `isento_ponto`; janela mais antiga não entregue até ontem (`app/ponto/services/esmeril_espelho.py:38`, `:206`). | Conta assinaturas recorrentes A1–A11 (`:14`); A5 vem de órfãs da ata e requer repetição (`:125`, `:139`). Cron declarado às 08:30 com `--apply` (`app/config/crons.py:640`); comando lê ou lavra (`app/ponto/management/commands/esmeril_espelho.py:15`). | Não monta `dia['batidas']`, portanto não verifica fidelidade do impresso; seus insumos são células, turnos e ata (`app/ponto/services/esmeril_espelho.py:95`). |

O **R4** declara pares entre leitores e meta zero em cada par; a lista é tela/PDF, cartão/TXT, espelho/DiaPago, fechamento/soma, topo/linhas, app/tela e linhas/badge, sem um par “apuráveis × marcações impressas” (`app/core/placar_estrutural.py:211`). O item de “dia com batida apurável e sem DiaPago” aparece como **cura ainda necessária**, e diz respeito à lavratura, não ao espelho impresso (`app/core/placar_estrutural.py:406`).

## 17/11 15:10 · sessao ff67f8be2721 · - · - · origem=Ronald (tmux)

Leitores diretos de produção:

- `PerguntaDisputa.q_validada_pelo_ato`: lê a flag no predicado SQL que identifica RESPOSTA que limpa validação (`app/chamados/models.py:3547`); devolve `Q`, sem persistência (`app/chamados/models.py:3557`). Sua negação está em `app/chamados/models.py:3562`.
- `DecisaoPergunta._resposta_desfaz_validacao`: `ato == RESPOSTA and not resposta_mantem_validacao` (`app/chamados/models.py:4281`); usado para montar a sequência em `leitura` (`app/chamados/models.py:4299`). `validada = None` é variável local (`app/chamados/models.py:4312`), e a função devolve namedtuple (`app/chamados/models.py:4315`; definição em `app/chamados/models.py:19`).

Todos os consumidores diretos de `leitura` e da face SQL encontrados em produção ficam em `app/chamados/models.py`:

| Consumidor | Referência | Efeito |
|---|---|---|
| `_gravar_estado` consulta face SQL | `:317` | Seleciona perguntas para persistir lavra por `_lavrar_s2` (`:319`; grava campos em `app/chamados/veredito_lavrado.py:270` e `:275`). |
| `_ancoradas` consulta face SQL | `:357` | Retorna lista usada pelo gate de estado; o método apenas seleciona (`:343`). |
| `q_vivas` consulta face SQL | `:3609` | Retorna filtro SQL; não grava nesse método (`:3609`). |
| `ressuscitar_perguntas` | `:1684` | Usa leitura na foto da REABERTURA (`:1691`), registra ato (`:1696`), limpa projeção (`:1698`), acorda células (`:1701`) e reabre disputas (`:1708`). |
| `sem_veredito_entre` | `:2000` | Lista derivada (`:1999`), consumida por `pode_fechar` (`:2011`) e `fechar` (`:2044`). |
| `fechar` | `:2085` | Foto persistida no SILENCIO (`:2083`); evento da pergunta (`:2086`), grava fechamento de disputa (`:2095`) e evento (`:2096`). |
| `remover_motivo` | `:2217` | Texto persistido em RETRATACAO (`:2215`), carimba identidade (`:2220`) e salva disputa (`:2224`). |
| `estado` | `:3231` | Deriva rótulo (`:3232`). |
| `respondida` | `:3782` | Deriva booleano pelo juiz. |
| `resposta_display` | `:3806` | Deriva texto exibido. |

Portanto, o leitor central não desfaz `PerguntaDisputa.validada_em` no banco; seus consumidores incluem escritores reais, especialmente ressuscitação e fechamento (`app/chamados/models.py:4312`, `app/chamados/models.py:1698`, `app/chamados/models.py:2095`).

Caminhos concretos:

- **Request:** upload de foto chama kernel em `app/api/views.py:861` e `app/api/views_core.py:845`; kernel consulta `pode_fechar` e fecha (`app/api/foto_recebida.py:83`). Desarquivar é POST (`app/chamados/views_arquivar.py:56`), chama reabrir (`:70`) e ressuscitar (`:86`).
- **Signal:** `post_save` de PerguntaDisputa registra `on_commit` (`app/chamados/signals.py:229`, `:240`), que chama fechamento de beco (`:239`); classifica pelo juiz (`app/chamados/services/becos.py:34`) e fecha (`:57`). Salvar fechamento da disputa dispara receiver que tenta fechar pai (`app/chamados/signals.py:19`, `:43`).
- **Cron configurado:** consumidor a cada minuto (`app/config/crons.py:394`) chama reconciliação (`app/chamados/reconciliador.py:1368`), que consulta juiz e fecha (`:613`). Também chama encerramento de órfãs (`:1378`), cujo fechamento consulta leitores (`:1166`, `:1173`). `fechar_becos_disputa` está configurado com `--alarme` (`app/config/crons.py:663`), portanto só lê nessa configuração; a escrita exige `--apply` sem `--alarme` (`app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54`, `:57`). Cron instalado e execução efetiva em produção: `[nao verificado]`.

**Nuance C6:** não lê esta flag: `projecao_temporal_da_pergunta` lê as três colunas da projeção (`app/chamados/juizes.py:1389`), e `_pergunta_viva_calc` decide com essa projeção (`:1404`). Diferenciar do filtro SQL novo `q_vivas`, que usa a sequência (`app/chamados/models.py:3609`).

**P1/P2:** para a flag isolada, backfill não tem corte de data/pk: alcança toda RESPOSTA de motivo não vazio existente na execução (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`). Coluna e backfill estão juntos atualmente (`:22`, `:28`); divisão exige separar operações, não reaplicar `AddField` (`:22`). `db_default=False` permite omissão da coluna no INSERT (`:26`), com teste que simula INSERT da main (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:25`), sem execução nesta auditoria.

**Risco P1 da obra inteira:** escritor novo grava REABERTURA compensadora (`app/chamados/models.py:1691`). A tarefa atual precisa ignorá-la antes de escolher último ato (`app/chamados/services/tarefa_do_dia.py:423`, `:426`), pois qualquer REABERTURA entregue ao corpo vira `devolvida_ao_colaborador` (`:163`). Assim, **se “leitor velho” significa carregar esses atos sem o filtro novo, P1 muda a fila do colaborador**, mesmo sem trocar o leitor da flag; é inferência demonstrável desses ramos, não verificação do pai indisponível. O teste declara main `fbc5c76ce` e comparação com dump (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:2`, `:10`), mas não fornece a fonte integral do pai.

## 17/11 15:10 · sessao 1c17ecf2b98a · - · - · origem=Ronald (tmux)

Censo dos escritores de produção:

| Escritor | Regra gravada | Comparação com 0055 |
|---|---|---|
| `responder_pergunta_disputa_core` | `False`; omite motivo, cujo default é `''`; zera `validada_em/por` quando havia validação. | Coincide: RESPOSTA sem motivo não entra no UPDATE. `app/chamados/services/disputa_emissao.py:1484`, `:1494`, `:1532`; `app/chamados/models.py:4162`. |
| `responder_automaticamente` | `True`, motivo fixo não vazio; salva somente resposta/texto, preservando validação. | Coincide, inclusive “resposta automatica”. `app/chamados/services/disputa_emissao.py:1682`, `:1685`, `:1686`. |
| `carimbar_resposta_pela_trilha` | `True`, motivo prefixado por `"pela trilha: "`; salva somente `respondida_em`. | Coincide mesmo com argumento `motivo=''`, pois o prefixo permanece. `app/chamados/services/disputa_emissao.py:1705`, `:1707`, `:1708`. |
| `linhas_da_projecao_sem_linha` | `True` para RESPOSTA faltante, motivo `MOTIVO_PROJECAO_SEM_LINHA`; VALIDACAO omitindo campo recebe `False`. | Coincide, inclusive o motivo especial. `app/chamados/models.py:4224`, `:4249`, `:4251`, `:4254`. |

Porta física: `registrar` instancia `cls(**ln)` e chama `objects.bulk_create`; não deduz booleano a partir do motivo (`app/chamados/models.py:4318`, `:4328`). Campo omitido recebe `default=False`, e INSERT antigo sem coluna recebe `db_default=False` (`app/chamados/models.py:4185`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:26`). Demais atos emitidos por essa porta recebem `False`; constraint proíbe `True` fora de RESPOSTA (`app/chamados/models.py:4205`).

`save` aceita nascimento e bloqueia regravação com PK (`app/chamados/models.py:4216`). `QuerySet.update` e `bulk_update` são bloqueados (`app/chamados/models.py:4115`, `:4121`). Não localizei outro create/save/update/bulk de produção do booleano; AST do tripwire, executado isoladamente com stdlib e `python3 -B`, retornou exatamente os quatro montadores acima; o próprio teste declara limite para montagem indireta (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:4`, `:7`, `:18`, `:59`).

Migration: 0055 cria com False e atualiza **todas** RESPOSTAS cujo motivo difere de `''`, sem recorte por PK/data/origem e sem checar valor anterior (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `:22`, `:28`). O model histórico via `apps.get_model` permite esse UPDATE, diferentemente do manager vivo (`:11`; `app/chamados/models.py:4115`).

0051 renomeia modelo/campos e atribui `ato='reabertura'` ao acervo; não cria RESPOSTA e não conhece o booleano (`app/chamados/migrations/0051_decisao_pergunta.py:41`, `:49`, `:61`). Seu RunPython só preenche chave de `PerguntaDisputa` (`:22`, `:25`, `:27`). Máscara da sombra pode usar `_base_manager.bulk_update`, mas inventário restringe DecisaoPergunta a texto/motivo, não ao booleano (`bin/sombra_mascara_pessoal.py:47`, `:62`; `bin/sombra_regras_pessoais.py:142`).

Ressalva essencial para Q2/P1: os quatro escritores vivos coincidem **por valor emitido**, mas o contrato do modelo não impõe equivalência universal com o predicado 0055. Há fixtures deliberadas gravando motivo preenchido+False e motivo vazio+True pela mesma porta; leitura segue campo (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`, `:143`, `:150`). Não são escritores de produção. A constraint permite ambas (`app/chamados/models.py:4205`). Em P2, UPDATE alteraria o primeiro caso para True; deixaria o segundo True, pois não faz reset dos não selecionados (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`).

## 17/11 15:09 · sessao 3072bb3d73be · - · - · origem=Ronald (tmux)

1. **NAO PROCEDE como contorno remanescente.** O fallback chama a porta pública (`app/ponto/management/commands/reconciliar_fantasmas.py:288`), que trava e abre a cerca (`app/chamados/services/disputa_emissao.py:1903`, `:1909`). A recusa é devolvida no dict e contada antes de continuar (`:1914`; `app/ponto/management/commands/reconciliar_fantasmas.py:291`).

2. **NAO PROCEDE para os caminhos nomeados.** Beco abre a cerca sobre resolver e fecho (`app/chamados/services/becos.py:55`, `:66`); HIST cerca costura e resolver (`app/chamados/reconciliador.py:1690`, `:1701`); S143 chama a porta pública (`app/chamados/signals.py:117`). As referências de produção ao corpo privado são as duas chamadas da porta (`app/chamados/services/disputa_emissao.py:1897`, `:1909`). A exceção `VIA_COMPETENCIA_TRANCADA` devolve sem escrita (`:1934`).

3. **PROCEDE como sobrealcance da porta.** `save` vigia antes de considerar `update_fields`, que só é encaminhado depois (`app/chamados/models.py:3572`, `:3578`). Consequentemente, consulta até data alterada exclusivamente em memória, embora o campo não seja persistido; também vigia `save(update_fields=[])` (`app/chamados/services/fio.py:1409`; `app/chamados/models.py:3576`). `descarimbar` não testa se os três campos já estão vazios (`app/chamados/models.py:3327`), e o lote não exclui linhas cujos valores já coincidem com os valores finais (`:3401`, `:3406`).

   Contraprova em memória, AST real dessas funções, imports/ORM/atomic/super simulados: `update_fields=[]` com pergunta ENTREGUE foi vetado antes do super; banco ABERTA + data ENTREGUE apenas em memória + `update_fields=['pergunta']` também foi vetado. Integração Django e ocorrência desse padrão dentro de retratação real: **[nao verificado]**.

6. **Ordem dos abridores:** retratação, beco e resolver travam antes do savepoint da cerca (`app/chamados/reconciliador.py:552`; `app/chamados/services/becos.py:52`; `app/chamados/services/disputa_emissao.py:1903`; abertura em `app/chamados/services/fio.py:1439`). HIST abre antes da trava feita internamente pela costura (`app/chamados/reconciliador.py:1701`, `:1691`; `app/chamados/services/fio.py:938`).

   **ESMERIL-SEMANTICA — contradição estática relevante:** a guarda chama `_dia_de_turno_da_pergunta`, que apenas retorna `data_evento` (`app/chamados/services/fio.py:1409`; `app/chamados/juizes.py:342`). O mesmo módulo declara que, para episódios, `data_evento` pode ser chave divergente do dia real; a identidade vem do juiz da batida (`app/chamados/services/fio.py:1605`, `:1620`). A migração reconhece explicitamente dia de saída diferente do dia do turno e deixa fora grupos com qualquer dia exportado (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:17`, `:30`). Portanto, normalização universal não está garantida pelo código: turno D23 ENTREGUE com chave D24 ABERTA permite consulta apenas da competência ABERTA; a relavra S2 de pergunta materializada continua alcançável (`app/chamados/models.py:305`; `app/chamados/veredito_lavrado.py:275`). Existência dessa linha em produção: **[nao verificado]**.

   **ContextVar:** há estado novo por abertura e reset em `finally`, inclusive com erro (`app/chamados/services/fio.py:1435`, `:1448`); aninhamentos reutilizam o contexto externo (`:1433`). AST real com atomic simulado confirmou reset após sucesso, exceção e veto engolido; thread nova observou `None`. Vazamento real entre requests/testes: **[nao verificado]**.

   **Relógio/juiz:** funções novas não introduzem leitura de relógio; a competência continua composta pelo veto existente e pelos juízes da janela/K6 (`app/chamados/services/fio.py:1358`, `:1389`; `app/chamados/services/vigia_fio.py:800`). Os `now()` do resolver e da lavra permanecem em `app/chamados/services/disputa_emissao.py:2019`, `:2076` e `app/chamados/veredito_lavrado.py:271`.

   **Limite adicional a investigar:** HIST avulso pode fazer da cerca a transação externa; `_CERCA` só é resetada após sair do atomic (`app/chamados/reconciliador.py:1701`; `app/chamados/services/fio.py:1439`, `:1449`). Há callbacks `on_commit` que chamam beco/regularizações (`app/chamados/signals.py:240`, `:146`). Uma recusa tardia nesses callbacks não teve cenário completo demonstrado: **[nao verificado]**.

## 17/11 15:09 · sessao f14db117992d · - · - · origem=Ronald (tmux)

Q4 histórico exato: `[nao verificado]`. A cópia não tem `.git` nem fonte identificável do pai; comentários/testes documentam MAIN sem colunas, mas não provam seu censo completo (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:1`).

Censo comprovável na árvore atual:

- Automática: grava RESPOSTA, motivo constante não vazio `'resposta automatica (fato entregue por outro caminho)'`, flag True (`app/chamados/services/disputa_emissao.py:1686`). Request POST upload: `app/api/views.py:837`, chamada `:860`; equivalente `app/api/views_core.py:821`, chamada `:844`; kernel chama `_validar_pelo_dono` (`app/api/foto_recebida.py:74`), que chama escritor (`:48`) e validador (`:51`).
- Trilha: RESPOSTA, motivo `'pela trilha: %s'`, flag True (`app/chamados/services/disputa_emissao.py:1708`). Comando `curar_perguntas_em_limbo --apply` (`app/chamados/management/commands/curar_perguntas_em_limbo.py:64`, `:67`). Classificado como ato único, não rotina (`app/config/crons.py:1482`).
- Foto da projeção sem linha: RESPOSTA com `MOTIVO_PROJECAO_SEM_LINHA` (`app/chamados/models.py:4224`), flag True, se existe `respondida_em` sem correspondência `(pergunta, RESPOSTA, instante)` (`:4249`, escrita `:4251`). Helper apenas monta dicionários; persistências atuais:
  - nascimento `_nascer`: `app/chamados/models.py:3202`, `:3207`;
  - ressuscitação: `app/chamados/models.py:1650`;
  - resposta humana: `app/chamados/services/disputa_emissao.py:1480`, `:1532`;
  - resposta automática: `app/chamados/services/disputa_emissao.py:1681`, `:1686`;
  - validação: `app/chamados/services/validacao.py:167`, `:173`;
  - devolução: `app/chamados/services/validacao.py:272`, `:279`;
  - BUG-PAR: `app/chamados/services/validacao.py:345`, `:354`;
  - estoque L130: `app/chamados/services/validacao.py:436`, `:439`;
  - backfill E4b: `app/chamados/management/commands/backfill_fio_morto_e4b.py:138`, `:141`, classificado sob ordem, sem rotina (`app/config/crons.py:797`).

Ressuscitação tem request explícito em `app/chamados/views_arquivar.py:52`, `:86`, também via `reabrir` (`app/chamados/models.py:1510`), e cartório (`app/ponto/services/cartorio.py:1674`). A cadeia histórica exata por signal/cron no pai permanece `[nao verificado]`.

`backfill_decisoes_pergunta` **só mede** as linhas RESPOSTA do helper: `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:67`; seu apply explicitamente não escreve essas linhas (`:3`). Documentação histórica de regras/fluxos pode apoiar inferência, mas não substitui o pai ausente: `[nao verificado]`.

## 17/11 15:08 · sessao a24eba6cf524 · - · - · origem=Ronald (tmux)

Item 5 — censo e mapa dos campos

O censo estático executado em memória (`python3 -B`, sem Django) encontrou um `update` atribuível a PerguntaDisputa em `app/chamados/models.py:3406` e um `bulk_update` em `app/chamados/veredito_lavrado.py:250`; o primeiro chama a vigilância em `app/chamados/models.py:3401`, o segundo não chama. Os únicos chamadores encontrados de `gravar_lavras_em_lote` são o backfill em `app/chamados/management/commands/backfill_veredito_lavrado.py:143` e `:146`.

| Modelo / campos relevantes | Escritores concretos | Leitores relevantes do diff/caminho |
|---|---|---|
| PerguntaDisputa `disputa`, `motivo`, `celula`, `conversa`, `chamado_destino` no nascimento | `_nascer`, `app/chamados/models.py:3134`, persistência `:3137` | Guarda lê `conversa`/`disputa` em `app/chamados/services/fio.py:1403`, `:1408`, `:1414`; resolver seleciona destino/disputa/célula em `app/chamados/services/disputa_emissao.py:1939`; HIST lê motivo/disputa em `app/chamados/reconciliador.py:1602`, `:1605`, `:1649` |
| PerguntaDisputa `chamado_destino`, `celula` no reuso | `app/chamados/models.py:3086`, `:3102`, save `:3126` | `_ancoradas`, `app/chamados/models.py:344`; HX, `app/chamados/services/disputa_emissao.py:2069` |
| PerguntaDisputa `chamado_destino` na costura | `app/chamados/services/fio.py:974`, save `:976` | Resolver direto `app/chamados/services/disputa_emissao.py:1940`; HX `:2069`; HIST `app/chamados/reconciliador.py:1691` |
| PerguntaDisputa `data_evento` | Atribuição única `app/chamados/services/fio.py:849`; nascimento chama em `app/chamados/models.py:3136`; costura em `app/chamados/services/fio.py:928` | Vigilância lê memória e banco em `app/chamados/services/fio.py:1403`, `:1409`, `:1411`; juiz do dia em `app/chamados/juizes.py:342`; resolver em `app/chamados/services/disputa_emissao.py:2019`, `:2091`; HIST em `app/chamados/reconciliador.py:1612` |
| PerguntaDisputa `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição do trio `app/chamados/models.py:3220`; `carimbar` salva em `:3273`, identidade em `:3311`, descarimbar em `:3333`; lote `:3406` | Seleção direta `app/chamados/services/disputa_emissao.py:1941`; HX `:2073`, fotografia/trilha `:2083`; filtro do lote `app/chamados/models.py:3402`; guarda executa antes do save em `:3576` |
| PerguntaDisputa `respondida_em`, `resposta_colab` | Resposta humana `app/chamados/services/disputa_emissao.py:1481`, save `:1522`; automática `:1674`, save `:1677`; reparo pela trilha `:1696`, save `:1698`; preenchimento de instante pela porta `app/chamados/models.py:3253`; limpeza por lote `:3408` com `**outros` | C21 `app/chamados/juizes.py:1552`; resolver direto `app/chamados/services/disputa_emissao.py:2013`; HX `:2085`, `:2090`; porta de par `app/chamados/models.py:3603` |
| PerguntaDisputa `validada_em`, `validada_por` | Validação `app/chamados/services/validacao.py:168`, save `:171`; devolução `:273`, save `:276`; resposta nova limpa em `app/chamados/services/disputa_emissao.py:1494`; auto-validação `app/chamados/models.py:3256`; descarimbo/lote pode limpar via `**outros`, `:3408` | `_ancoradas` `app/chamados/models.py:344`; relavra S2 `:310`; HX `app/chamados/services/disputa_emissao.py:2073`; resposta nova `:1484` |
| PerguntaDisputa `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`, `app/chamados/veredito_lavrado.py:270`, save `:275`; backfill atribui em `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, persiste pelo bulk `app/chamados/veredito_lavrado.py:250` | `esta_stale`, `app/chamados/veredito_lavrado.py:224`, `:228`, `:232`; foto `:281`; testes leem `.values()` em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:258` |
| ChamadoColaborador `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`, `urgencia`, `urgencia_original`, `atualizado_em` | `_gravar_estado`, `app/chamados/models.py:228`, `:231`, `:244`, `:245`, `:246`, `:251`, persistência `:296`; urgência via update `:637` | Resolver tranca `app/chamados/services/disputa_emissao.py:1896`, `:1934`; HX lê status `:2040`, `:2066`, `:2069`; S143 `app/chamados/signals.py:97`; recarregar estado `app/chamados/models.py:759`, `:761` |
| ChamadoColaborador `colaborador`, `justificativa_relacionada`, `categoria`, `subcategoria`, `contexto_json` | Nascimento/reuso pelo modelo; todos os escritores desses campos na árvore: **[nao verificado]** | Ato lê colaborador/justificativa em `app/chamados/reconciliador.py:550`, `:573`; HIST lê contexto/categoria/subcategoria `:1633`, `:1634`, `:1642`; callback do filho lê contexto `app/chamados/signals.py:361` |
| DisputaSupervisao `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `fechar`, `app/chamados/models.py:2023`, save `:2027`; reaberturas também escrevem — enumeração completa **[nao verificado]** | Beco `app/chamados/services/becos.py:59`; resolver `app/chamados/services/disputa_emissao.py:2063`, `:2124`; Defesa 1 `app/chamados/signals.py:29`, `:46` |
| DisputaSupervisao `colaborador`, `chamado`, `contexto`, `motivos` | Emissão/reuso tem múltiplos ramos: `app/chamados/services/disputa_emissao.py:550`, `:576`, `:1097`, `:1145`, `:1269`, `:1380`; modelo modifica motivos/contexto em `app/chamados/models.py:2057`, `:2154`, `:2195`, `:2250` | Resolver `app/chamados/services/disputa_emissao.py:2061`, `:2065`; HIST contexto `app/chamados/reconciliador.py:1605`; beco pai `app/chamados/services/becos.py:44`; guarda colaborador `app/chamados/services/fio.py:1414` |
| LogAuditoria `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `usuario`, `ip`, `ator` | `core.observ.evento`, criação em `app/core/observ.py:134`; adaptação `evento_kw` em `:146`; retenção prepara payload e chama em `app/chamados/reconciliador.py:478`, `:493` | Dedup da retenção filtra ação/modelo/objeto e lê `valor_depois`, ordenando id, em `app/chamados/reconciliador.py:489`; fotografia de teste em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:219` |

PKs/IDs são lidos na guarda (`app/chamados/services/fio.py:1404`) e nos filtros; o nascimento usa PK gerada pelo modelo, sem atribuição explícita no trecho (`app/chamados/models.py:3134`). Enumeração de todos os leitores da árvore, inclusive apresentação e ferramentas, **[nao verificado]**; a tabela nomeia os leitores relevantes à alteração.

**Bypass real:** `gravar_lavras_em_lote` não passa por `vigiar`; a ausência é concreta em `app/chamados/veredito_lavrado.py:247`. Não encontrei caminho dos atos cercados até esse bulk; seus chamadores encontrados são exclusivamente `backfill_veredito_lavrado.py:143` e `:146`. Não foi demonstrada escrita SQL/raw de PerguntaDisputa; ausência absoluta de escrita dinâmica **[nao verificado]**.

Item 8 — matriz

| Sítio | Cobertura e evidência |
|---|---|
| 1 retratar_com_motivo | Abre cerca `app/chamados/reconciliador.py:553`, após trava `:552`, antes do corpo/transição `:567` |
| 2 fechar_beco | Abre cerca `app/chamados/services/becos.py:66`, após trava `:52`; callback inclui resolver e disputa `:57`, `:60` |
| 3 resolver público | Cerca `app/chamados/services/disputa_emissao.py:1909`, após trava `:1903` |
| 4 reconciliar | Herda quando aninhado; avulso chama `_gravar_estado` em `app/chamados/models.py:655` e relavra em `:312`, sem abrir cerca própria |
| 5 transicionar / 6 encerrar | Convergem no escritor de estado; save e relavra `app/chamados/models.py:296`, `:312`; cobertura apenas herdada |
| 7 encerrar_por_competencia_trancada | Via especial do resolver retorna sem escrita em `app/chamados/services/disputa_emissao.py:1934` |
| 8 reabrir | Escritas por descarimbo ficam guardadas quando há contexto; lote vigia em `app/chamados/models.py:3401`, persiste `:3406` |
| 9 Disputa.fechar | Escreve perguntas mudas em `app/chamados/models.py:2012`, disputa em `:2027`; cerca herdada nos atos, avulso permanece fora |
| 10 absolvidas pela célula | Carimba em `app/chamados/reconciliador.py:639`, fecha disputa em `:656`; avulso não abre cerca |
| 11 reatar órfã | HIST cercado em `app/chamados/reconciliador.py:1701`; c1 chama `lavrar_sem_turno_aberto` em `:1664`; derivável costura fora em `:1712` |
| 12 órfãs pela lâmpada | Carimba `app/chamados/reconciliador.py:1192`, fecha disputa `:1216`; sem abridor próprio |
| 13 reapontar | Save guardado quando aninhado em `app/chamados/services/fio.py:976` |
| 14 lavrar | Save guardado quando aninhado em `app/chamados/veredito_lavrado.py:275` |
| 15 gravar_lavras_em_lote | Bulk sem vigilância, `app/chamados/veredito_lavrado.py:250`; backfill avulso `app/chamados/management/commands/backfill_veredito_lavrado.py:143` |

**Caminho temporal que merece achado nos itens 7–8:** a transição cercada pode registrar o callback `_filho_morto_acorda_o_pai`, por `transaction.on_commit(_reconciliar)` em `app/chamados/signals.py:371`; o callback chama `reconciliar_chamados_colab` em `:370`, que chama **antes** de qualquer retratação própria `retratar_perguntas_absolvidas_por_celula` e `reconciliar_perguntas_orfas_colab` em `app/chamados/reconciliador.py:1477`. A primeira grava pergunta em `:639`; a segunda pode seguir c1 em `:1664` ou derivável em `:1712`. No fluxo com atomic externo de retratar (`:543`), o savepoint da cerca termina antes desse commit e `_CERCA` é restaurado em `app/chamados/services/fio.py:1449`. Portanto, essas escritas diferidas não herdam a cerca que originou o callback, e a guarda retorna cedo em `:1397`.

O selo anexo liga callbacks como arestas estáticas sem modelar a execução após commit; esse limite temporal não está declarado entre os limites descritos no anexo **selo_diff.txt: `_liga_corpo` / `_via`**. A ocorrência com dados reais e uma pergunta ENTREGUE elegível nesse callback é **[nao verificado]**; o caminho de código está presente nos pontos acima. [nao verificado]

## 17/11 15:08 · sessao 3923d3e3219f · - · - · origem=Ronald (tmux)

Item 4 — **PROCEDE parcialmente: mordida específica foi corrigida, mas há expectativa incoerente e mutante sem mordida.**

- As recusas conferem a linha completa da pergunta pelo PK antes das fotografias restantes; a classificação ALVO também confere o PK na mensagem. Portanto a mordida específica está implementada. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:249-267,523-540`.
- `sem_escopo:fechar_beco = {}` é coerente com o caso construído: ele tenta resolver a pergunta direta ENTREGUE; retirado apenas o escopo do beco, o resolver interno conserva sua própria cerca. Entretanto o teste aceita explicitamente que retirar a cerca do beco não derrube caso nenhum; falta prova da proteção adicional do `d.fechar`. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:435-448,550-563`; `app/chamados/services/becos.py:55-66`; `app/chamados/services/disputa_emissao.py:1909-1914`.
- **`engolido_sobrevive -> e=ALVO` está errado pelo fluxo.** O mutante conserva a guarda anterior ao save e captura a exceção propagada do callback, retornando vetadas. No caso e, a pergunta validada não entra na relavra do chamado; o carimbo direto do resolver encontra a guarda, que levanta antes da persistência. O wrapper recebe vetadas e registra a trilha. O desenho deste caso conduz a **verde**, não ALVO: não existe aqui uma exceção engolida dentro do callback do resolver. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:84-99,418-433,565`; `app/chamados/services/disputa_emissao.py:1909-1914,2019-2020`; `app/chamados/models.py:3270-3277,3572-3581`. Execução integral dos testes: **[nao verificado]**.
- O caso s tem C21 completo, com texto não vazio, instante e validação. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:382-384`.
- O relatório do geofence usa os mesmos três motivos que a saída imprime; o caso g exige a saída particionada. `app/ponto/services/geofence.py:444-448`; `app/ponto/management/commands/reconciliar_geofence.py:46-49`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:477-478`.

Item 7 — **PROCEDE: o selo tem subaproximação concreta não declarada e suas verificações estruturais não demonstram todos os requisitos.**

1. `_indexa` e `_liga` do anexo só entram em statements diretamente FunctionDef/AsyncFunctionDef/ClassDef; não atravessam If/With/Try para descobrir definições aninhadas. `_liga_corpo`, por sua vez, cria referências para essas definições via `ast.walk`, mas exclui seus corpos do chamador. O resultado são callbacks apontando para IDs sem definição; `vermelhos` ignora callbacks ausentes que não sejam SITIOS. Evidência: `selo_diff.txt` anexo, métodos `_indexa`, `_liga`, `_liga_corpo` e trecho `if s not in g.defs` **[linhas do anexo nao verificadas]**.
2. Dois exemplos reais:
   - `_ato` de fechar_beco está dentro do With; desaparecem suas chamadas aos resolvers e ao `d.fechar`. `app/chamados/services/becos.py:42,55-60,66`.
   - `_costura_e_fecho` está dentro de `if ch is None`; desaparecem `reapontar_resolvedora` e resolver. `app/chamados/reconciliador.py:1603,1690-1696,1702`.
   - Fiz reprodução somente AST, em memória: o algoritmo de indexação do selo encontra zero definições `_costura_e_fecho`, embora o AST real a encontre na linha 1690. Não houve banco, teste Django nem arquivo criado. **[nao verificado: resultado de ferramenta sem artefato persistido]** [nao verificado]
3. Outros limites não descritos:
   - a1 conta chamada dentro de `if False` antes da persistência; não prova passagem efetiva pela guarda.
   - a2 procura qualquer If contendo Raise; não confere que o teste seja `vetadas`. [nao verificado]
   - a3 procura qualquer With contendo If/Raise; não confere `transaction.atomic` nem a condição de veto.
   - a4 exige que o nome das vetadas apareça em algum If; não demonstra que isso devolva/publique a recusa.
   Essas propriedades decorrem literalmente dos predicados de `vermelhos` e `_abridor_ok` do anexo. **[linhas do anexo nao verificadas]**
4. Os 14 nomes enumerados são compatíveis com a própria classificação declarada; a confirmação da quantidade calculada executando o selo completo permanece **[nao verificado]**. A via trancada explica dois vermelhos conservadores: wrapper chama o corpo sem cerca, e o corpo retorna antes de qualquer escrita. `app/chamados/services/disputa_emissao.py:1896-1897,1931-1935`.
5. A adaptação substitui consulta prévia por rollback; ela não demonstra o instante literal do aval. A implementação chama transição dentro do callback e só consulta na porta do save. `app/chamados/reconciliador.py:553,569-574`; `app/chamados/models.py:3572-3581`.

## 17/11 15:03 · sessao 5dcc15cd69b2 · CENSO-FECHA-DE-VERDADE · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 3, 4, 5 e CONTRATO**, pelas lacunas abaixo. Não devolvo novamente a completude ao portão nem exijo esperado zero para recusas informativas (`bin/disjuntor.py:674`; `app/docs/PISTA.md:173`; decisão do complemento fornecido: [nao verificado em arquivo local]).

Revisão estática: os selos criam e apagam arquivos temporários; foram somente inspecionados. Resultado dos novos casos: **[nao verificado]** (`bin/tests/test_censo_antes_da_vaga.sh:27`, `28`; `bin/tests/test_disjuntor.sh:39`, `41`).

**1 — forma dos caminhos**

1. **Premissas:** o juiz atual verifica aparência de referência, sem conferir existência; o aperto proposto ainda precisa entrar nesse juiz. A cópia já possui `_sitio_existe`, portanto não falta um instrumento para verificar existência e limite da linha (`bin/disjuntor.py:1501`, `1565`, `674`).

2. **Colisão:** não há incompatibilidade necessária em atribuir a completude à leitura do Codex: censo e rastreamento já pertencem a esse papel, e o escopo continua alcançando escritores e leitores fora do diff (`CLAUDE.md:358`, `379`). Conferir somente existência do `.r` não certifica seu conteúdo; essa limitação foi expressamente assumida no complemento **[nao verificado em arquivo local]**.

3. **Literal:** “linha cabe” já tem definição operacional disponível: número inteiro positivo, até o total de quebras `LF`, acrescido de uma última linha sem quebra final. Arquivo vazio tem zero linhas; `x\n` tem uma; `x\ny` tem duas. O predicado também exige caminho relativo, dentro da raiz, sem symlink e legível (`bin/disjuntor.py:678`, `683`, `685`, `693`, `698`). Há casos e mutantes para zero, fim exato, além do fim e última linha sem quebra (`bin/tests/test_disjuntor.sh:714`, `720`, `1665`). Igualar contagens não comprova cobertura dos destinos; não devolvo essa objeção como completude automática, pois o complemento a retirou do portão **[nao verificado em arquivo local]**. A ligação desse predicado ao CENSO continua futura (`bin/disjuntor.py:1565`, `1577`).

4. **Exportada/dinheiro/!:** verificar referências e existência do relatório não autoriza escrita de negócio; permanecem os limites de competência exportada, escala/vínculo e escopo literal (`CLAUDE.md:265`, `268`).

**3 — busca fixa de ataques**

1. **Premissas:** o juiz atual lê o brief e examina seu bloco; não pesquisa ataques nos logs. A busca fixa é alteração futura do mesmo juiz (`bin/disjuntor.py:1525`, `1544`, `1577`). Substituir comando declarado por termos literais fecha a objeção anterior de executar ou confiar no “grep usado” **[nao verificado em arquivo local]**.

2. **Colisão:** responder achados anteriores é compatível com a revisão vigente. Cobrar resposta também para falso positivo é custo declarado no complemento, não colisão independente (`CLAUDE.md:373`, `375`; declaração desse custo: **[nao verificado em arquivo local]**).

3. **Literal — PROCEDE:** o universo ainda não fecha: `logs/AVAIS-*` significa arquivos, diretórios percorridos recursivamente ou ambos? A pasta da obra inclui subpastas? `RELATO.md` significa o arquivo canônico `app/docs/RELATO.md`? Este último caminho existe como constante; o algoritmo de busca proposto permanece **[nao verificado]** (`bin/disjuntor.py:125`, `1525`, `1577`).

   Também falta fixar se basta **qualquer** termo, sensibilidade à caixa, termo vazio, significado de N e consequência de arquivo ilegível ou busca interrompida **[nao verificado]**. “Busca fixa” impede executar o brief, mas ainda não especifica essas respostas.

   **O próprio brief casa**, se todos os arquivos da pasta forem pesquisados: ele contém obrigatoriamente o ID entre os termos. Sem exclusão declarada, precisará de `respondido: <seu próprio caminho>`. Isso é atendível, não impossibilidade lógica; o achado efetivo depende do futuro algoritmo **[nao verificado]**. A coleta atual inclui justamente os briefs dessa pasta (`bin/disjuntor.py:1604`, `1613`).

   O custo depende dos arquivos/bytes, termos e briefs julgados; repetir a busca por brief pode repetir a leitura do universo, pois a porta julga todos os coletados. Volume, duração e achados reais são **[nao verificado]** (`bin/disjuntor.py:1717`). Não há prova desses logs pelo material raspado fornecido; `logs/` é ignorado na cópia (`.gitignore:53`; `bin/codex_pergunta.sh:315`, `331`).

4. **Exportada/dinheiro/!:** a busca textual não amplia autorização de negócio. O risco residual identificado é de alcance, recusa e custo do instrumento (`CLAUDE.md:265`, `268`; custo concreto: **[nao verificado]**).

**4 — complemento como obra própria**

1. **Premissas:** confirma o coletor preservado: todos os `brief*.md` da pasta literal da obra, unidos aos citados no **texto da abertura**. Não há coleta recursiva das citações encontradas dentro dos briefs (`bin/disjuntor.py:1587`, `1604`, `1607`, `1619`). No CLI, nomes e conteúdo dos anexos compõem esse texto (`bin/disjuntor.py:2296`, `2305`).

2. **Colisão:** separar ID/pasta elimina a inclusão automática da pasta da mãe; voltar com o mesmo ID continua submetendo a pasta inteira ao juiz. Isso resolve aquela parte da devolução anterior sem mudar o coletor (`bin/disjuntor.py:1604`, `1619`, `1717`). O encaixe técnico de complemento continua atribuído à sessão (`CLAUDE.md:276`, `278`).

3. **Literal — PROCEDE:** a forma do censo admite hífens indistintos; ela não separa `<MAE>` de `<COMPL>`. Por exemplo, `MAE-A-B` pode representar mãe `MAE-A` ou mãe `MAE`; critério de identificação, mãe ausente e múltiplas mães candidatas permanecem **[nao verificado]** (`bin/disjuntor.py:162`).

   A pertença atual também não fornece herança por prefixo: a fronteira de ID exclui hífen, portanto reconhecer `MAE` não reconhece automaticamente `MAE-COMPL` (`bin/hook_stop_fila1.py:110`, `117`; `bin/disjuntor.py:1688`, `1689`).

   “O complemento citar a mãe” precisa significar citação na abertura/anexo; somente escrever o caminho dentro de um brief descoberto pelo glob não a coleta. Exigir brief próprio continua sendo aperto da porta: hoje uma união composta apenas por brief citado de outra pasta pode passar (`bin/disjuntor.py:1611`, `1613`, `1715`). Preservar o coletor não impede cobrar essa condição, mas o reconhecimento necessário ainda não está fechado **[nao verificado]**.

4. **Exportada/dinheiro/!:** herdar pertença não herda autorização adicional de negócio; permanecem os limites literais do aval (`CLAUDE.md:265`, `268`).

**5 — uma pertença nas duas bocas**

1. **Premissas:** a costura apontada confere: CLI e porta usam `_pertence_fila1`, mas o CLI retorna `FORA` antes da porta. Alterar somente `_censo_nega` deixa o escape identificado na revisão anterior (`bin/disjuntor.py:1707`, `2306`, `2307`, `2311`, `2315`). A fila viva vem do BACKLOG e do predicado de estado do hook (`bin/disjuntor.py:1630`, `1634`; `bin/hook_stop_fila1.py:180`, `250`).

2. **Colisão:** a precedência explícita de fila 1 resolve tela∩fila1; distinguir CONDUTA de alcance do portão também responde à divergência anterior **[nao verificado em arquivo local: complemento fornecido]**. Não há necessidade de segunda pertença para isso (`bin/disjuntor.py:1672`, `1674`).

3. **Literal — PROCEDE:** coluna 2 é `balde` nas tabelas de obras, mas não em toda tabela da GRADE: o placar usa coluna 2 numérica, e complementos têm tabela própria (`app/docs/GRADE-MESTRA.md:13`, `35`, `125`, `532`). Falta definir o universo de tabelas e o casamento entre o ID recebido e a coluna 1; ausência, qualificação, duplicatas, baldes divergentes e `?` ainda não têm resultado especificado **[nao verificado]**. O predicado atual só resolve ambiguidades das células da fila 1 (`bin/disjuntor.py:1675`, `1681`, `1694`).

   “Ambígua = recusa” fixa a consequência, mas não quais situações tornam a GRADE ambígua **[nao verificado]**. Também falta resolver a combinação entre pertença própria e herdada: a GRADE contém mãe e complemento com baldes diferentes, portanto ler diretamente a linha do complemento pode contrariar a herança proposta (`app/docs/GRADE-MESTRA.md:119`, `531`, `534`). Filho na fila 1 e mãe fora exige precedência explícita entre os itens 4 e 5 **[nao verificado]**.

4. **Exportada/dinheiro/!:** ampliar quem precisa de censo não amplia o aval de negócio (`CLAUDE.md:265`, `268`).

**CONTRATO — medida**

1. **Premissas:** negativas atuais são agregadas; uma recusa pode reunir motivos de vários briefs. Aprovação do brief, tentativa do wrapper e abertura Agent têm significados distintos (`bin/disjuntor.py:1714`, `1728`, `2072`, `2077`, `2104`, `2145`).

2. **Colisão:** não procede exigir esperado zero para recusas informativas. O contrato pode ter contador auxiliar informativo e um indicador de segurança com esperado zero; a regra exige contador e esperado no contrato (`app/docs/PISTA.md:170`, `173`; nova distribuição dos indicadores: **[nao verificado em arquivo local]**).

3. **Literal — PROCEDE:** “por motivo” ainda precisa definir a unidade: uma recusa com dois motivos conta em ambos, somente no primeiro ou por brief? O código produz essa multiplicidade numa única negativa (`bin/disjuntor.py:1714`, `1717`, `1728`).

   “Abertura sem APROVADO” também precisa preservar o significado observável: `ini` é tentativa, não prova execução do Codex. O indicador atual declara isso e exclui chamadas casadas com `FORA`; desligamento tem tratamento próprio. Renomear para abertura efetiva ou contar indistintamente obras dispensadas produziria outra medida (`bin/disjuntor.py:2072`, `2081`, `2143`, `2159`). A definição futura desses pontos é **[nao verificado]**.

4. **Exportada/dinheiro/!:** o delta de medição não autoriza escrita de negócio nem alteração das hipóteses de `!` (`CLAUDE.md:265`, `268`).

**RED**

1. **Premissas:** existem casos de juiz/CLI, pretool, pertença, controles e mutantes no selo específico. O selo geral tem outra lista de casos; não comprova hoje os novos REDs do complemento (`bin/tests/test_censo_antes_da_vaga.sh:650`, `660`, `662`, `668`; `bin/tests/test_disjuntor.sh:1501`).

2. **Colisão:** os quatro casos adicionais são compatíveis com esse método; não há objeção independente por serem testes futuros. A infraestrutura já exige controle sem mutação e queda do caso quando a costura sai (`bin/tests/test_censo_antes_da_vaga.sh:662`, `680`, `685`).

3. **Literal:** o caso lateral pelo CLI ataca o retorno antecipado correto; o controle de complemento continua dependente das definições devolvidas nos itens 3–5 (`bin/disjuntor.py:2311`, `2315`; comportamento futuro: **[nao verificado]**). “Conforme” precisa incluir os campos antigos, ainda exigidos pelo juiz (`bin/disjuntor.py:1565`, `1568`, `1570`). `RED=test_disjuntor.sh` pode ser destino futuro; não é prova de ligação já existente com o selo específico (`bin/tests/test_disjuntor.sh:1501`; `bin/tests/test_censo_antes_da_vaga.sh:650`). Mordida dos novos casos e mutantes: **[nao verificado]**.

4. **Exportada/dinheiro/!:** o molde inspecionado usa árvores de mentira; não demonstra toque em produção (`bin/tests/test_censo_antes_da_vaga.sh:20`, `36`; `bin/tests/test_disjuntor.sh:47`, `59`).

Item 1 — **NAO PROCEDE** (`bin/disjuntor.py:674`; `CLAUDE.md:358`).
Item 3 — **PROCEDE**: universo e semântica da busca ainda **[nao verificado]**.
Item 4 — **PROCEDE** (`bin/disjuntor.py:162`, `1611`; `bin/hook_stop_fila1.py:117`).
Item 5 — **PROCEDE** (`app/docs/GRADE-MESTRA.md:35`, `532`; `bin/disjuntor.py:1675`).
CONTRATO — **PROCEDE** (`bin/disjuntor.py:1728`, `2072`, `2159`).
RED — **NAO PROCEDE** como objeção independente; execução e mordida novas **[nao verificado]**.

## 17/11 15:01 · sessao 3c5a184a33b5 · - · - · origem=Ronald (tmux)

**CONTRATO**

1. **Premissas:** a medida vigente distingue aprovação do brief, tentativa no wrapper e abertura Agent; uma aprovação cobre todos os briefs coletados (`bin/disjuntor.py:1701`, `1703`, `2072`, `2077`, `2163`). As negativas atuais são agregadas, sem abertura por motivo (`bin/disjuntor.py:2102`, `2145`, `2164`).

2. **Colisão:** não devolveria apenas por tornar recusas informativas. O contrato exige contador e esperado, e o complemento fornece esperado 0 para o indicador de segurança; o literal não exige esperado 0 para cada contador auxiliar (`app/docs/PISTA.md:170`, `173`). A decisão nova foi fornecida no anexo; existência local desse aval: **[nao verificado]**.

3. **Literal:** persiste uma distinção relevante: “abertura sem CENSO APROVADO” não pode ser apresentada como execução efetiva de construtor Codex. O `ini` é tentativa e não prova que o Codex rodou; o próprio ESTADO declara essa limitação (`bin/disjuntor.py:2072`, `2159`). Também falta definir a unidade de “por motivo”: uma chamada pode acumular vários motivos de vários briefs; contar eventos, motivos ou briefs produz números diferentes (`bin/disjuntor.py:1714`, `1717`, `1728`). Sugestão: manter o indicador vigente de **tentativas**, declarar recusas por motivo como contagem de eventos associados a cada motivo, ou explicitar outra unidade; a escolha concreta permanece **[nao verificado]**.

4. **Exportada/dinheiro/!:** o delta de medida não autoriza escrita de negócio; continuam os limites literais de competência exportada, escala/vínculo, atalhos e dinheiro (`CLAUDE.md:265`, `268`, `404`).

**Conclusão sugerida: CONTRATO — PROCEDE**, exclusivamente pelo significado operacional de “abertura”/unidade de contagem, **não** por ausência de esperado 0 nas recusas (`bin/disjuntor.py:2072`, `1714`; `app/docs/PISTA.md:173`).

**RED**

1. **Premissas:** o selo específico atual exercita juiz/CLI, pretool, topo, importação, medida e pertença, incluindo controle sem mutação e mutantes (`bin/tests/test_censo_antes_da_vaga.sh:13`, `650`, `662`, `668`). `test_disjuntor.sh` tem outra lista de casos e outra declaração final de cobertura (`bin/tests/test_disjuntor.sh:1501`, `1836`).

2. **Colisão:** casos novos e mutantes são compatíveis com o método existente; a exigência original de mutantes permanece no dado “nada mais muda” **[nao verificado em arquivo local]**. Não trataria a ausência atual dos testes futuros como reprovação do desenho; o selo existente já separa casos do juiz e casos da costura (`bin/tests/test_censo_antes_da_vaga.sh:18`, `668`).

3. **Literal:** os quatro casos novos resolvem alvos da devolução anterior, mas seu resultado depende das definições ainda discutidas nos itens 1, 3, 4 e 5 **[nao verificado]**. O caso CLI lateral alcança justamente a saída antecipada atual (`bin/disjuntor.py:2311`, `2315`). O controle de complemento precisa conservar os campos antigos do CENSO: hoje até a fixture “conforme” contém leitores, escritores, decisões e perguntas (`bin/tests/test_censo_antes_da_vaga.sh:54`). A indicação `RED=bin/tests/test_disjuntor.sh` deve ser entendida como destino futuro ou ganhar ligação explícita com o selo específico; a cópia não comprova que esse arquivo já executa os casos de censo (`bin/tests/test_disjuntor.sh:1501`; `bin/tests/test_censo_antes_da_vaga.sh:650`). Resultados novos: **[nao verificado]**.

4. **Exportada/dinheiro/!:** o molde existente usa árvore de mentira; executar os selos criaria e apagaria arquivos temporários, motivo para somente inspecioná-los nesta revisão (`bin/tests/test_censo_antes_da_vaga.sh:20`, `27`, `28`, `36`; `bin/tests/test_disjuntor.sh:39`).

**Conclusão sugerida: RED — NAO PROCEDE como objeção independente**, preservadas as devoluções dos itens de que os casos dependem; cobertura executada dos novos casos e seus mutantes: **[nao verificado]**.

## 17/11 15:01 · sessao fef53bc871a8 · - · - · origem=Ronald (tmux)

Item 1:

- Q1: juiz atual só reconhece aparência de referência (`_REF_RE`) e exige uma em leitores/escritores; ainda não valida caminhos ou existência (`bin/disjuntor.py:1501`, `bin/disjuntor.py:1565`, `bin/disjuntor.py:1577`). Isso é implementação futura autorizada, não achado por si.
- “Linha cabe” tem predicado existente preciso: `_sitio_existe` exige arquivo legível, regular, relativo e dentro da raiz, sem symlink em nenhum trecho; `1 <= linha <= total`, total = quantidade de `\n` + última linha sem `\n` (`bin/disjuntor.py:674`, `bin/disjuntor.py:683`, `bin/disjuntor.py:693`, `bin/disjuntor.py:698`; regular vem de `_abre`, `bin/disjuntor.py:187`, `bin/orcamento_contexto.py:77`). Arquivo vazio tem zero linhas; `x\n` tem uma, `x\ny` tem duas, `x\r y` sem LF conta uma segundo esse predicado (`bin/disjuntor.py:695`, `bin/disjuntor.py:698`).
- Existem REDs de B3 para zero, última linha exata, além do fim, última sem LF, fora da raiz, todas referências, sintaxe truncada e symlinks (`bin/tests/test_disjuntor.sh:714`, `bin/tests/test_disjuntor.sh:720`, `bin/tests/test_disjuntor.sh:723`, `bin/tests/test_disjuntor.sh:731`, `bin/tests/test_disjuntor.sh:735`). Não provam a costura futura no CENSO, cujo teste conforme ainda usa referências fictícias (`bin/tests/test_censo_antes_da_vaga.sh:54`, `bin/tests/test_censo_antes_da_vaga.sh:86`).
- Q2: completude por leitura do Codex é compatível com seu papel de censo e rastreamento; escopo continua incluindo leitores/escritores fora do diff (`CLAUDE.md:358`, `CLAUDE.md:379`). O anexo retira explicitamente do portão a prova de “TODO caminho”; não devolver novamente essa objeção como se o portão ainda a prometesse [nao verificado].
- Q3: o complemento não diz se incorpora integralmente `_sitio_existe` ou só “arquivo existe/linha cabe”; vincular explicitamente ao predicado evita divergência entre `splitlines()` (usado no brief, `bin/disjuntor.py:1526`) e contagem LF (referências, `bin/disjuntor.py:695`). Também não explicita se N conta linhas ou destinos distintos: duas linhas podem apontar ao mesmo sítio e satisfazer só igualdade numérica; o código atual não oferece essa verificação (`bin/disjuntor.py:1544`, `bin/disjuntor.py:1577`). “ponto único”, autorizado no aval-base, deixa de ter fechamento se a nova exigência obrigar sempre “lista fechada”; esclarecer se o único é lista de um [nao verificado].
- Q4: validar arquivos/linhas e existência do `.r` não toca diretamente negócio; a leitura atual retorna motivo, e limites de exportada/escala/vínculo permanecem (`bin/disjuntor.py:1525`, `bin/disjuntor.py:1577`, `CLAUDE.md:265`).

Item 3:

- Q1: juiz atual só abre o brief e examina seu bloco; não pesquisa logs (`bin/disjuntor.py:1525`, `bin/disjuntor.py:1544`, `bin/disjuntor.py:1577`). Fixar algoritmo, ler termos como texto e não executar comando fecha o defeito anterior de “grep declarado = busca provada” [nao verificado].
- Q2: custo de responder falso positivo foi assumido no anexo, portanto não é colisão por si [nao verificado]. Requisito de levar classes anteriores e responder achados tem apoio vigente (`CLAUDE.md:373`, `CLAUDE.md:375`).
- Q3: faltam definir universo concreto de arquivos: `logs/AVAIS-*` inclui arquivos diretamente, diretórios recursivos ou ambos? `logs/<OBRA>/` é recursivo? Qual `RELATO.md`? A constante existente é `app/docs/RELATO.md`, não arquivo na raiz (`bin/disjuntor.py:125`). Faltam semântica OR/AND dos termos, caixa/normalização, termo vazio e tratamento de arquivo ilegível/symlink/limite; a infra já distingue arquivo regular e symlink (`bin/disjuntor.py:187`, `bin/orcamento_contexto.py:68`).
- Busca obrigatoriamente casa o próprio brief se ele contém o ID exigido e todos os arquivos da pasta são pesquisados; sem exclusão declarada, requer `respondido: logs/<OBRA>/brief*.md` para si mesmo [nao verificado]. Isso é atendível sob o custo de falsos positivos; não é impossibilidade lógica. Contudo, qualquer exigência de “ataques cruzados: 0” precisa distinguir número de termos, de arquivos encontrados e de respostas, pois ID obrigatório gera termo e o brief pode gerar achado [nao verificado].
- Custo depende do total de arquivos/bytes pesquisados, termos e quantidade de briefs julgados: a porta julga cada brief coletado (`bin/disjuntor.py:1717`); busca refeita em cada juiz pode repetir a leitura desse universo. Volume e duração reais são [nao verificado]. Não inventar benchmark de produção.
- Logs vivos não estão na cópia: `.gitignore:53`; leitor nasce via archive e recebe anexos raspados (`bin/codex_pergunta.sh:315`, `bin/codex_pergunta.sh:331`, `bin/codex_pergunta.sh:334`). Logo não é possível provar achados efetivos nesta revisão.
- Q4: busca textual e recusa não ampliam aval de negócio; limites continuam literais (`CLAUDE.md:265`, `CLAUDE.md:268`).

Juízo sugerido: PROCEDE no item 3 pelas lacunas de universo/semântica/custo; item 1 exige separar lacuna operacional real de escolha já feita pelo aval. A necessidade de ligar formalmente o novo validador ao `_sitio_existe`, a correspondência caminhos→sítios e a sobrevivência de “ponto único” justificam devolução pontual, sem repetir cobrança de completude pelo portão [nao verificado].

## 17/11 15:01 · sessao 7d9abfee0495 · - · - · origem=Ronald (tmux)

**Item 4 — PROCEDE, pelas lacunas restantes.**

1. O coletor confirma a união de todos os `brief*.md` da pasta literal da obra com os briefs citados no **texto da abertura**, sem coleta recursiva do conteúdo dos briefs (`bin/disjuntor.py:1587`, `1604`, `1607`, `1619`). No CLI, o conteúdo dos anexos também compõe esse texto (`bin/disjuntor.py:2296`, `2305`).
2. Separar o complemento em outro ID/pasta resolve a coleta automática do brief antigo da mãe; a volta com mesmo ID continua julgando a pasta inteira (`bin/disjuntor.py:1604`, `1619`, `1717`). Não há colisão necessária com o encaixe técnico de complemento atribuído à sessão (`CLAUDE.md:276`, `278`).
3. Restam três lacunas:
   - `OBRA_CENSO_ID = [A-Za-z0-9-]+` não distingue mãe de complemento: `MAE-A-B` admite mais de uma decomposição; campo declarativo, candidatos, regra de seleção e mãe inexistente são **[nao verificado]** (`bin/disjuntor.py:162`).
   - O reconhecimento atual por ID inteiro exclui hífen da fronteira; portanto `MAE-COMPL` não herda automaticamente a pertença de `MAE` (`bin/hook_stop_fila1.py:110`, `117`; `bin/disjuntor.py:1688`, `1689`).
   - Pasta própria não assegura **brief próprio**: a porta aceita apenas brief citado de outra pasta, pois exige somente união não vazia. A obrigação precisa ser cobrada na porta existente, preservando o coletor (`bin/disjuntor.py:1713`, `1715`, `1717`).
   
   “Se o complemento citar” precisa indicar abertura/anexo; citar dentro de um brief apenas encontrado pelo glob não produz coleta recursiva (`bin/disjuntor.py:1587`, `1611`, `1613`; `bin/disjuntor.py:2300`).
4. Não há autorização adicional sobre negócio; exportada, escala/vínculo, dinheiro e demais hipóteses de `!` continuam subordinados ao escopo literal (`CLAUDE.md:265`, `268`, `286`).

**Item 5 — PROCEDE, pelas lacunas restantes.**

1. As duas bocas atuais efetivamente compartilham `_pertence_fila1`; o CLI consulta antes da porta e retorna `FORA` antecipadamente. O complemento aponta a costura correta a alterar (`bin/disjuntor.py:1707`, `2306`, `2307`, `2311`, `2315`). Fila viva usa o predicado do hook sobre estado, não classificação por balde (`bin/disjuntor.py:1623`, `1634`; `bin/hook_stop_fila1.py:180`, `231`, `250`).
2. A precedência expressa de fila 1 resolve a objeção anterior de tela∩fila1, e a separação CONDUTA/portão resolve a divergência declarada anterior **[nao verificado em arquivo local: delta fornecido]**. Permanece necessária uma única resposta compartilhada pelas duas bocas, como já estabelece o código (`bin/disjuntor.py:1672`, `1674`, `2307`).
3. “Coluna 2” identifica o campo balde nas tabelas de obras (`app/docs/GRADE-MESTRA.md:35`, `125`), mas não define:
   - quais tabelas entram: o placar tem coluna 2 numérica, complementos têm baldes próprios, OBSOLETAS também têm baldes e o quadro final tem coluna 2 de estado (`app/docs/GRADE-MESTRA.md:13`, `532`, `1561`, `1674`, `1759`);
   - como se casa o ID, como se tratam IDs qualificados, ausência, duplicatas concordantes/divergentes e balde `?`; esses casos não são resolvidos pelo predicado atual, que só recebe células do BACKLOG (`bin/disjuntor.py:1672`, `1688`; `app/docs/GRADE-MESTRA.md:505`, `521`);
   - como a herança da mãe prevalece sobre a linha própria do complemento. Há exemplo concreto de mãe `20/20` e complemento `INSTRUMENTO` (`app/docs/GRADE-MESTRA.md:119`, `531`, `534`). Como combinar “filho fila 1 viva” com “pertença é a da mãe” permanece **[nao verificado]**.
   
   “Ambígua = recusa” define a consequência, mas os critérios da ambiguidade da GRADE continuam **[nao verificado]**; a ambiguidade implementada hoje pertence às células da fila 1 (`bin/disjuntor.py:1681`, `1694`).
4. Ampliar o universo do portão não amplia a autorização sobre negócio ou as hipóteses de `!` (`CLAUDE.md:265`, `268`).

Item 4: **PROCEDE**.  
Item 5: **PROCEDE**.

## 17/11 14:59 · sessao 944bdb1b0fda · CENSO-FECHA-DE-VERDADE · censo · origem=Code

**Não certifico fechamento global das quatro fatias.** Os mapas abaixo registram as arestas conferidas na cópia; os contratos F1/F2 citados nos anexos, os hashes de origem e a execução dos backfills em produção permanecem **[nao verificado]**. A cópia já contém caminhos do par de intervalo que ampliam o alcance dos briefs (`app/chamados/services/materializacao.py:214`, `app/chamados/services/validacao.py:526`).

Contagem: uma linha do mapa é uma aresta de chamada, registro de evento ou entrega explícita de callback; caminhos maiores resultam da composição dessas arestas. Isso não transforma um fechamento transitivo incompleto em lista fechada. **[nao verificado]**

Para caber sem perder as referências, as abreviações expandem literalmente para:

| Abreviação | Arquivo ou prefixo |
|---|---|
| `D` | `app/chamados/services/disputa_emissao.py` |
| `M` | `app/chamados/services/materializacao.py` |
| `V` | `app/chamados/views.py` |
| `VC` | `app/chamados/views_cobrar.py` |
| `MD` | `app/chamados/models.py` |
| `J` | `app/chamados/juizes.py` |
| `K` | `app/config/crons.py` |
| `S/` | `app/chamados/services/` |
| `C/` | `app/chamados/management/commands/` |
| `P/` | `app/ponto/management/commands/` |
| `T/` | `app/templates/chamados/` |

**F4 — mapa de caminhos**

```text
_depois app/ponto/signals.py:262 -> devolver_absorvidas_da_ausencia_rejeitada M:1121
_validar_pelo_dono app/api/foto_recebida.py:48 -> responder_automaticamente D:1654
api_disputa_responder app/api/views.py:2627 -> responder_pergunta_disputa_core D:1439
reabrir_por_colab MD:2318 -> reabrir_para_correcao D:1576
_slot_esperado J:376 -> _ts_tipo_do_plantio M:599
responder_pergunta_disputa V:1283 -> responder_pergunta_disputa_core D:1439
reabrir_disputa_supervisao V:1425 -> recobrar_questionario D:1712
reabrir_disputa_supervisao V:1436 -> reabrir_para_correcao D:1576
materializa_perguntas_ao_fechar_disputa app/chamados/signals.py:165 -> materializar_pendentes_aprovadas M:2098
_plantar_o_dia S/passivo_validado.py:537 -> _materializar_uma M:1809
par_do_intervalo M:214 -> _ts_tipo_do_plantio M:599
par_do_intervalo M:222 -> _ts_tipo_do_plantio M:599
respostas_intervalo_recusadas_paridade M:339 -> estado_aplicacao_pergunta M:852
_ts_tipo_do_plantio M:656 -> _tipo_pelo_marco M:512
espera_declarado_x_gravado M:757 -> estado_aplicacao_pergunta M:852
_estado_pelo_derivador M:895 -> _ts_tipo_do_plantio M:599
devolver_absorvidas_da_ausencia_rejeitada M:1147 -> perguntas_absorvidas_por M:1085
devolver_absorvidas_da_ausencia_rejeitada M:1164 -> reabrir_para_correcao D:1576
_materializar_resposta_hora M:1328 -> _ts_tipo_do_plantio M:599
_materializar_resposta_escolha M:1641 -> _declarar_ausencia_de_pergunta M:944
_gravar_irma_do_par M:1707 -> _materializar_uma M:1809
_materializar_uma M:1889 -> _materializar_resposta_hora M:1266
_materializar_uma M:1891 -> _materializar_resposta_data_hora M:1541
_materializar_uma M:1893 -> _materializar_resposta_escolha M:1626
materializar_perguntas_validadas_da_disputa M:2084 -> _materializar_uma M:1809
_validar_o_par S/validacao.py:555 -> _materializar_uma M:1809
validar_pergunta S/validacao.py:687 -> _materializar_uma M:1809
validar_pergunta S/validacao.py:698 -> materializar_perguntas_validadas_da_disputa M:2032
recusar_pergunta S/validacao.py:836 -> reabrir_para_correcao D:1576
declarar_hora_colab S/declaracao_texto.py:419 -> responder_pergunta_disputa_core D:1439
aplicar_declaracao S/declaracao_texto.py:693 -> responder_pergunta_disputa_core D:1439
responder_pergunta_disputa_core D:1555 -> _materializar_ausencia_de_resposta M:978
resolver_perguntas_por_chamado_resolvido D:1897 -> _resolver_perguntas_por_chamado_resolvido D:1906
resolver_perguntas_por_chamado_resolvido D:1903 -> _resolver_perguntas_por_chamado_resolvido D:1906
_resolver_perguntas_por_chamado_resolvido D:1969 -> reabrir_credor_de_pergunta_viva D:1827
explicar_pergunta S/explicador.py:124 -> estado_aplicacao_pergunta M:852
acao_reabrir_disputa S/acoes_disputa.py:105 -> reabrir_para_correcao D:1576
acao_regularizar_disputa S/acoes_disputa.py:141 -> materializar_perguntas_validadas_da_disputa M:2032
acao_corrigir_resposta S/acoes_disputa.py:373 -> responder_pergunta_disputa_core D:1439
montar_chamados_fio S/fio.py:376 -> estado_aplicacao_pergunta M:852
processar_post_fio S/fio.py:716 -> materializar_perguntas_validadas_da_disputa M:2032
aplicacao_da_resposta app/chamados/templatetags/chamados_tags.py:136 -> estado_aplicacao_pergunta M:852
handle C/curar_perguntas_em_limbo.py:67 -> carimbar_resposta_pela_trilha D:1683
handle C/curar_perguntas_em_limbo.py:69 -> reabrir_para_correcao D:1576
ato_do_dia C/backfill_fio_morto_e4b.py:90 -> reabrir_credor_de_pergunta_viva D:1827
_executar_dia C/backfill_fio_morto_e4b.py:135 -> reabrir_credor_de_pergunta_viva D:1827
_executar_dia C/backfill_fio_morto_e4b.py:147 -> materializar_perguntas_validadas_da_disputa M:2032
_exportadas_classe_e C/backfill_fio_morto_e4b.py:175 -> reabrir_credor_de_pergunta_viva D:1827
resolver_disputas_de_foto app/api/foto_recebida.py:74 -> _validar_pelo_dono app/api/foto_recebida.py:28
api_upload_foto app/api/views_core.py:845 -> resolver_disputas_de_foto app/api/foto_recebida.py:57
api_upload_foto app/api/views.py:861 -> resolver_disputas_de_foto app/api/foto_recebida.py:57
_validar_pelo_dono app/api/foto_recebida.py:51 -> validar_pergunta S/validacao.py:574
api_solicitar_correcao_disputa app/api/views.py:2686 -> reabrir_por_colab MD:2263
solicitar_correcao_disputa V:1334 -> reabrir_por_colab MD:2263
responder_chamado_colaborador V:1090 -> declarar_hora_colab S/declaracao_texto.py:306
detalhe_chamado_local V:475 -> acao_regularizar_disputa S/acoes_disputa.py:120
detalhe_chamado_local V:480 -> aplicar_declaracao S/declaracao_texto.py:641
detalhe_chamado_local V:501 -> acao_validar_pergunta S/acoes_disputa.py:13
detalhe_chamado_local V:518 -> acao_recusar_pergunta S/acoes_disputa.py:67
detalhe_chamado_local V:536 -> acao_corrigir_resposta S/acoes_disputa.py:304
meu_atendimento V:569 -> montar_chamados_fio S/fio.py:190
fio_colaborador V:674 -> processar_post_fio S/fio.py:531
fio_colaborador V:677 -> montar_chamados_fio S/fio.py:190
_resp_modal_full V:889 -> montar_chamados_fio S/fio.py:190
modal_fio_colaborador V:954 -> processar_post_fio S/fio.py:531
contexto_parcial_fio S/fio.py:492 -> montar_chamados_fio S/fio.py:190
acao_validar_pergunta S/acoes_disputa.py:19 -> validar_pergunta S/validacao.py:574
acao_recusar_pergunta S/acoes_disputa.py:82 -> recusar_pergunta S/validacao.py:770
acao_corrigir_resposta S/acoes_disputa.py:381 -> validar_pergunta S/validacao.py:574
_validar_o_par S/validacao.py:545 -> par_do_intervalo M:175
_gravar_irma_do_par M:1695 -> par_do_intervalo M:175
_materializar_resposta_hora M:1371 -> _gravar_irma_do_par M:1687
validar_pergunta S/validacao.py:683 -> _validar_o_par S/validacao.py:526
_pergunta_materializada_de_fato J:756 -> _slot_esperado J:345
classificar S/vigia_carimbo.py:110 -> _slot_esperado J:345
classe_do_lote S/validacao.py:1115 -> _slot_esperado J:345
resolve_perguntas_ao_resolver app/chamados/signals.py:117 -> resolver_perguntas_por_chamado_resolvido D:1887
handle P/reconciliar_fantasmas.py:281 -> resolver_perguntas_por_chamado_resolvido D:1887
handle C/validar_classe_a.py:100 -> validar_pergunta S/validacao.py:574
handle C/bug_par_103.py:79 -> respostas_intervalo_recusadas_paridade M:295
include app/config/urls.py:28 -> urlpatterns app/chamados/urls.py:8
include app/config/urls.py:37 -> urlpatterns app/api/urls.py:9
include app/config/urls_core.py:6 -> urlpatterns app/api/urls_core.py:7
rota app/chamados/urls.py:14 -> detalhe_chamado_local V:359
rota app/chamados/urls.py:16 -> meu_atendimento V:559
rota app/chamados/urls.py:17 -> responder_pergunta_disputa V:1228
rota app/chamados/urls.py:19 -> solicitar_correcao_disputa V:1317
rota app/chamados/urls.py:20 -> reabrir_disputa_supervisao V:1372
rota app/chamados/urls.py:21 -> fio_colaborador V:659
rota app/chamados/urls.py:39 -> modal_fio_colaborador V:869
rota app/chamados/urls.py:42 -> responder_chamado_colaborador V:1041
rota app/api/urls.py:59 -> api_upload_foto app/api/views.py:839
rota app/api/urls.py:93 -> api_disputa_responder app/api/views.py:2584
rota app/api/urls.py:94 -> api_solicitar_correcao_disputa app/api/views.py:2670
rota app/api/urls_core.py:8 -> api_upload_foto app/api/views_core.py:823
handle_ensaio C/materializar_passivo_validado.py:104 -> reparar S/passivo_validado.py:289
handle_apply_dry C/materializar_passivo_validado.py:113 -> reparar S/passivo_validado.py:289
reparar S/passivo_validado.py:380 -> _trabalhar_o_par S/passivo_validado.py:431
_trabalhar_o_par S/passivo_validado.py:443 -> _plantar_o_dia S/passivo_validado.py:489
post_save_Chamado app/chamados/signals.py:91 -> resolve_perguntas_ao_resolver app/chamados/signals.py:92
post_save_Disputa app/chamados/signals.py:149 -> materializa_perguntas_ao_fechar_disputa app/chamados/signals.py:150
post_save_Ausencia app/ponto/signals.py:275 -> _consumidor_post_save_ausencia app/ponto/signals.py:276
post_delete_Ausencia app/ponto/signals.py:282 -> _consumidor_post_delete_ausencia app/ponto/signals.py:283
_consumidor_post_save_ausencia app/ponto/signals.py:279 -> _consumidor_ausencia app/ponto/signals.py:250
_consumidor_post_delete_ausencia app/ponto/signals.py:284 -> _consumidor_ausencia app/ponto/signals.py:250
on_commit app/ponto/signals.py:272 -> _depois app/ponto/signals.py:254
julgar_colab app/ponto/services/cartorio.py:1045 -> perguntar_pela_celula S/fabrica_da_celula.py:178
perguntar_pela_celula S/fabrica_da_celula.py:207 -> motivos_a_perguntar S/fabrica_da_celula.py:92
perguntar_pela_celula S/fabrica_da_celula.py:219 -> perguntar S/fabrica_da_celula.py:160
perguntar S/fabrica_da_celula.py:169 -> desfazer_carimbo_sem_lastro S/relavra_via.py:187
perguntar_pela_celula S/fabrica_da_celula.py:209 -> desfazer_carimbo_sem_lastro S/relavra_via.py:187
handle C/relavrar_via_i6.py:102 -> relavrar S/relavra_via.py:106
cron K:392 -> handle P/reconciliar_fantasmas.py:152
handle C/backfill_fio_morto_e4b.py:238 -> _exportadas_classe_e C/backfill_fio_morto_e4b.py:153
handle C/backfill_fio_morto_e4b.py:255 -> ato_do_dia C/backfill_fio_morto_e4b.py:82
handle C/backfill_fio_morto_e4b.py:309 -> ato_do_dia C/backfill_fio_morto_e4b.py:82
handle C/backfill_fio_morto_e4b.py:316 -> _executar_dia C/backfill_fio_morto_e4b.py:125
```

Os registros de `backfill_fio_morto_e4b`, `materializar_passivo_validado`, `validar_classe_a` e `curar_perguntas_em_limbo` são declarações de **não rotina**, não chamadas de cron (`K:791`, `K:853`, `K:1363`, `K:1476`). O fechamento de todos os chamadores transitivos dos juízes e de todos os disparadores dos signals permanece **[nao verificado]**.

**caminhos: 117**  
**lista fechada: [nao verificado]** — o mapa identifica 20 funções receptoras do núcleo, além das pontes e das exceções conservadas; não demonstra que sejam todos os sítios de campo alcançados.

**F4 — ataques adversariais**

1. **Par fora dos “três chamadores”: derivação paralela / 2º juiz.** O par chama o derivador duas vezes e decide validade por `q.validada_em` cru (`M:199`, `M:201`, `M:214`, `M:222`). A validação e o plantio da irmã passam por esse par (`S/validacao.py:545`, `M:1695`, `M:1707`). Migrar apenas os três caminhos do RED-4 permite que principal e irmã usem autoridades diferentes. **Caminho omitido:** `par_do_intervalo -> _ts_tipo_do_plantio`. Resposta da cura a esse ataque **[nao verificado]**.

2. **Outro parser de `data_hora`: derivação paralela.** O juiz deriva o timestamp diretamente da projeção (`J:364`, `J:370`); o materializador repete o parser (`M:1558`, `M:1565`). Trocar somente o texto do materializador permite plantar o timestamp do ato e reprovar esse mesmo plantio pelo timestamp velho do juiz, na conferência final (`M:1899`, `M:1906`). **Caminho não coberto pelo RED dos três chamadores:** ramo `data_hora` de `_slot_esperado`.

3. **Instante histórico ≠ processamento: relógio fora da borda.** O upload passa seu instante de processamento à resposta automática (`app/api/foto_recebida.py:42`, `:48`; `D:1675`, `D:1678`). A cura pela trilha usa o instante histórico recebido (`D:1696`, `D:1699`). Substituir ambos por “última resposta” sem preservar essa origem temporal pode promover uma resposta anterior à reabertura a resposta vigente. Ordem efetiva desses casos em produção **[nao verificado]**.

4. **Dia do turno ≠ dia civil: relógio fora da borda.** A derivação usa `data_evento` e ajusta overnight (`M:685`, `M:692`); o envelope inclui as duas bordas (`J:328`, `J:333`). O passivo consulta competência por `data_evento`, com `localdate(ts)` como fallback (`S/passivo_validado.py:98`, `:113`). A cura não pode trocar isso pelo dia de `ato_em`. Falta caso do último dia da competência, saída no dia seguinte e instante exatamente na borda.

5. **ABSORÇÃO tem vínculo e geração: 2º juiz / 2º escritor fora da porta.** A escrita guarda `absorcao_escape` e `absorcao_ausencia` (`M:1045`, `M:1048`); o leitor atual associa a geração pelo carimbo e exclui batidas vivas (`M:1109`, `M:1118`). A rejeição chega por `on_commit` e reabre efetivamente as perguntas (`app/ponto/signals.py:262`, `:272`; `M:1164`). Um fold “existe ABSORCAO” pode atingir geração antiga. A cura desse vínculo continua **[nao verificado]**.

6. **Backfill não fecha escritor vivo: 2º escritor fora da porta.** `carimbar(resposta=...)` sintetiza R/V e salva sem registrar RESPOSTA/VALIDACAO (`MD:3240`, `MD:3247`, `MD:3262`). Além dos dois furos citados pelo brief, há o carimbo central e o resolver (`M:1944`, `M:1945`; `D:2008`, `D:2009`). A conservação recompõe apenas quando chamada, por `(ato, instante)` (`MD:4094`, `MD:4105`). **Caminho omitido na lista de dois furos:** resolver → `carimbar`.

7. **Unidade real do plantio: dia/par, não pergunta: selo sem mordida.** O passivo usa savepoint do dia e conferência geométrica antes/depois (`S/passivo_validado.py:505`, `:513`, `:526`, `:550`); o materializador pode plantar também a irmã (`M:1371`, `M:1707`). RED que confira uma pergunta e apenas contagens pode passar com uma metade usando texto ou timestamp diferente da outra. Resposta do brief sobre paridade de atos das duas metades **[nao verificado]**.

8. **Selo dos cinco arquivos não protege os consumidores: selo sem mordida.** O retorno é consumido por explicador, filtro, vigia e classificador de lote (`S/explicador.py:124`; `app/chamados/templatetags/chamados_tags.py:136`; `S/vigia_carimbo.py:110`; `S/validacao.py:1115`). Um selo limitado à fatia não detecta o parser cru em `J:364`, nem a validade crua da irmã em `M:199`.

9. **Leitura alcança escrita financeira: derivação paralela / selo sem mordida.** O caminho do passivo chega ao mesmo materializador (`C/materializar_passivo_validado.py:104`, `:113`; `S/passivo_validado.py:380`, `:443`, `:537`). A configuração classifica esse ato como movimentação de dinheiro em competência exportada (`K:853`, `K:858`). “Só muda leitura” não prova ausência de mudança na seleção e no resultado dessa escrita. Impacto financeiro efetivo **[nao verificado]**.

**ataques cruzados: 9**

**F7 — mapa de caminhos**

```text
rota app/chamados/urls.py:16 -> meu_atendimento V:559
meu_atendimento V:569 -> montar_chamados_fio S/fio.py:190
meu_atendimento V:633 -> pagina T/meu_atendimento.html:1
include T/meu_atendimento.html:127 -> card T/partials/_card_atendimento.html:1
include T/meu_atendimento.html:144 -> card T/partials/_card_atendimento.html:1
rota app/chamados/urls.py:14 -> detalhe_chamado_local V:359
detalhe_chamado_local V:551 -> montar_contexto_detalhe S/acoes_chamado.py:337
detalhe_chamado_local V:550 -> pagina T/detalhe_local.html:1
include T/detalhe_local.html:310 -> card T/partials/_pergunta_card.html:1
detalhe_chamado_local V:501 -> acao_validar_pergunta S/acoes_disputa.py:13
detalhe_chamado_local V:518 -> acao_recusar_pergunta S/acoes_disputa.py:67
detalhe_chamado_local V:536 -> acao_corrigir_resposta S/acoes_disputa.py:304
injeta_resp V:501 -> _resp_card V:377
injeta_resp V:518 -> _resp_card V:377
injeta_resp V:536 -> _resp_card V:377
_resp_card V:404 -> card T/partials/_pergunta_card.html:1
rota app/chamados/urls.py:39 -> modal_fio_colaborador V:869
modal_GET V:956 -> _resp_modal_full V:882
modal_POST V:954 -> processar_post_fio S/fio.py:531
injeta_resp_final V:954 -> _resp_modal_full V:882
_resp_modal_full V:889 -> montar_chamados_fio S/fio.py:190
_resp_modal_full V:927 -> modal T/partials/modal_fio.html:1
rota app/chamados/urls.py:21 -> fio_colaborador V:659
fio_POST V:674 -> processar_post_fio S/fio.py:531
processar_post_fio S/fio.py:611 -> _resp_parcial S/fio.py:540
_resp_parcial S/fio.py:543 -> contexto_parcial_fio S/fio.py:483
_resp_parcial S/fio.py:542 -> modal T/partials/modal_fio.html:1
contexto_parcial_fio S/fio.py:492 -> montar_chamados_fio S/fio.py:190
injeta_resp S/fio.py:597 -> _resp_parcial S/fio.py:540
injeta_resp S/fio.py:629 -> _resp_parcial S/fio.py:540
injeta_resp S/fio.py:655 -> _resp_parcial S/fio.py:540
injeta_resp S/fio.py:684 -> _resp_parcial S/fio.py:540
injeta_resp S/fio.py:685 -> _resp_parcial S/fio.py:540
injeta_resp S/fio.py:690 -> _resp_parcial S/fio.py:540
rota app/chamados/urls.py:20 -> reabrir_disputa_supervisao V:1372
reabrir_disputa_supervisao V:1458 -> _fio V:1395
_fio V:1400 -> contexto_parcial_fio S/fio.py:483
_fio V:1399 -> modal T/partials/modal_fio.html:1
rota app/chamados/urls.py:36 -> lote_preview VC:353
lote_preview VC:361 -> fila_validar_classificada S/validacao.py:1155
lote_recusa VC:358 -> previa T/partials/_lote_preview.html:1
lote_preview VC:362 -> previa T/partials/_lote_preview.html:1
rota app/chamados/urls.py:37 -> validar_lote_tela VC:372
validar_lote_tela VC:381 -> validar_lote_classe_a S/validacao.py:938
lote_recusa VC:378 -> previa T/partials/_lote_preview.html:1
lote_resultado VC:386 -> previa T/partials/_lote_preview.html:1
```

Os terminais de leitura declarados pelo brief estão nestas **22 linhas** da cópia:

| Template | Sítios |
|---|---|
| `T/partials/_card_atendimento.html` | `:106`, `:114`, `:141`, `:143`, `:148`, `:154`, `:182`, `:183`, `:189`, `:391` |
| `T/partials/modal_fio.html` | `:185`, `:217`, `:219`, `:220`, `:228` |
| `T/partials/_pergunta_card.html` | `:20`, `:23`, `:24`, `:29`, `:32`, `:42` |
| `T/partials/_lote_preview.html` | `:24` |

A escolha das duas cascas vem do context processor registrado nas settings (`app/config/settings/base.py:82`; `app/core/context_processors.py:210`) e do `extends base_tpl` (`T/meu_atendimento.html:1`). O detalhe usa `base.html` fixo (`T/detalhe_local.html:1`).

Não localizei uma entrada direta de signal ou cron que renderize esses quatro templates; ausência de renderização dinâmica por essas vias **[nao verificado]**. O mapa também não fecha todas as invocações de retorno dentro dos callbacks nem o acessor novo ainda não implementado **[nao verificado]**.

**caminhos: 46**  
**lista fechada: 22 sitios** nos quatro templates declarados; fechamento do alcance Python e dinâmico **[nao verificado]**.

**F7 — ataques adversariais**

1. **Texto pelo ato, Enviar/Reenviar pela projeção: 2º juiz.** `resposta_enviada` continua vindo de `respondida_em` (`S/fio.py:187`, `S/fio.py:359`), enquanto o brief troca texto/data exibidos. Com RESPOSTA no ato e R nulo, o formulário pode dizer “Enviar resposta” ao lado de resposta vigente; com REABERTURA e projeção velha, o ramo pode continuar “enviado” (`T/partials/_card_atendimento.html:100`, `:161`, `:390`). A dependência F5 está citada, mas a paridade efetiva **[nao verificado]**.

2. **Um texto público não substitui valor e rótulo: derivação paralela.** O dropdown compara ID serializado (`T/partials/_card_atendimento.html:154`); `resposta_display` transforma ID em nome e códigos em rótulos (`MD:3693`, `MD:3701`, `MD:3708`). Reutilizar o display no prefill quebra `selected`; reutilizar o ID no SHOW perde o nome. A cura precisa preservar separadamente valor original e apresentação. Contrato desse acessor **[nao verificado]**

[truncado em 20000 de 45964 caracteres]

_6614 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
