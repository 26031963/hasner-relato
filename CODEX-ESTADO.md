# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 10:19** · publicado em **19/11/2026 10:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 10:19 · sessao 214707845bc3 · CADEIA-20-PASSO-1 · censo · origem=Code

# brief.md — CADEIA-20-PASSO-1, etapa A

**Estado: incompleto para aprovação do censo.** A ata guarda cada sobra como hora, sem PK nem data completa da batida; o pareador também pode consumir uma batida de intervalo sem devolvê-la dentro de um turno. A ligação de toda sobra a uma única batida viva, portanto, ainda precisa ser medida ([app/ponto/services/cartorio.py:749](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:749); [app/ponto/turnos.py:1371](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1371)).

Os anexos foram fornecidos no texto do pedido sem numeração de linhas. As referências a seus resultados abaixo ficam marcadas **[não verificado]**; não houve acesso ao banco ou aos logs. O arquivo `brief.md` não foi criado, conforme a regra de somente leitura.

## 1. Matriz: estado, autoridade, operação, persistência, retorno e prova

A persistência pretendida é comum às sete linhas: o cartório monta a ata e a entrega a `cel.lavrar_veredito(..., ata=_ata)`; `CelulaDia.ata` é o JSON da lavra. As seis razões e a marca, com esses nomes, são o resultado **proposto no aval fornecido**, não um retorno demonstrado do código atual ([app/ponto/services/cartorio.py:1898](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:1898); [app/escala/models.py:1772](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1772); aval fornecido: **[não verificado]**).

| Estado e retorno pretendido | Autoridade existente e operação | Entrega por batida? | Prova existente |
|---|---|---|---|
| Dia sem trabalho → `TRABALHO_EM_DIA_SEM_TRABALHO` | `dia_sem_trabalho(cel)` lê `cel.trabalha`, `ata.tipo_dia` e a exceção de cobertura parcial; retorna booleano. Não é uma leitura exclusiva de `tipo_dia`. | **Por dia.** Aplicar a resposta a cada sobra exige identificá-la. | [app/chamados/juizes.py:445](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/chamados/juizes.py:445); [app/escala/tests/test_orfa_classe.py:37](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_orfa_classe.py:37). |
| Rajada → `BATIDA_REPETIDA` | `classificar_orfas` agrupa horas consecutivas a até 15 minutos e devolve mapa hora → classe, quando há `missing`; o leitor de produção de cluster devolve uma assinatura **do dia**, contendo a lista inteira de órfãs e a contagem de lâmpadas apagadas. | **Por hora, com limite:** o classificador antigo identifica horas do grupo, mas não PKs e pode colidir em horas iguais. O leitor de produção responde só pelo dia. A equivalência integral à razão individual do aval está **[não verificada]**. | [app/escala/orfa_classe.py:26](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:26); [app/ponto/services/cluster_espurio.py:15](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cluster_espurio.py:15); [app/escala/tests/test_orfa_classe.py:47](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_orfa_classe.py:47). |
| Pausa → `INTERVALO_FORA_DA_ESCALA` | `parear_turnos` lê ou carimba `is_intervalo`/`_intra_dur` na instância da batida. Os carimbos podem ter mais de uma origem. | **Na instância de batida processada**, inclusive quando ela não entra em turno; isso não fornece a ligação da hora da ata à instância. A origem da pausa não é separada pelo carimbo comum. | [app/ponto/turnos.py:1310](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1310); [app/ponto/turnos.py:1371](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1371); [app/ponto/tests/test_parear_intra_duracao.py:23](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_parear_intra_duracao.py:23). |
| Par fechado → `PERIODO_ADICIONAL` | `parear_turnos` retorna `Turno` com `entrada`, `saida` e `batidas`; a leitura das pontas permite identificar um par fechado. | **Por batida associada ao turno**, após a junção; não retorna a razão pronta para a hora da ata. | [app/ponto/turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43); [app/ponto/tests/test_turnos_borda_direita.py:36](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos_borda_direita.py:36). |
| Turno aberto → `TURNO_ABERTO` | `parear_turnos` retorna turno com entrada e sem saída, com campo `aberto`. | **Por batida associada ao turno**, após a junção; não retorna a razão pronta para a hora da ata. | [app/ponto/turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43); [app/ponto/turnos.py:1497](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1497); [app/ponto/tests/test_turnos.py:19](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos.py:19). |
| Saída sem entrada → `SAIDA_SEM_ENTRADA` | `parear_turnos` retorna turno sem entrada e com saída. | **Por batida associada ao turno**, após a junção; não retorna a razão pronta para a hora da ata. | [app/ponto/turnos.py:1495](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1495); [app/ponto/tests/test_turnos.py:37](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos.py:37). |
| Dia publicado em cadastro × realidade ou na lista → marca `ESCALA_NAO_DESCREVE` | Ler `ResultadoMes.dias_cadastro_x_realidade` ou as assinaturas **datadas** já presentes na lista; `dias_na_lista` devolve chaves `(colab_id, data)`. | **Por dia**, não por batida. A presença da pessoa numa assinatura sem data não identifica o dia. | [app/ponto/motor_calculo_v2.py:289](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:289); [app/ponto/services/cxr_na_lista.py:79](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cxr_na_lista.py:79); [app/escala/services/cadastro_realidade.py:1135](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:1135). |

Os testes citados exercitam as autoridades atuais; não são testes das seis razões propostas nem provam a junção completa. **[não verificado]** [nao verificado]

**Ligação da sobra.** A grade formata a sobra como `HH:MM`, e a ata grava essa string ([app/escala/utils.py:673](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:673); [app/ponto/services/cartorio.py:764](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:764)). A hora, sozinha, **não identifica uma batida**: pode não haver candidata viva ou pode haver mais de uma no minuto. Comparar por minuto é uma operação de junção a medir, e a data civil da batida também pode diferir da data do turno ([app/ponto/turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43); [app/ponto/tests/test_turnos_borda_direita.py:36](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos_borda_direita.py:36)).

## 2. Casos nominais pela regra — L-110

A precedência fornecida é: dia sem trabalho; depois rajada; depois papel do pareador. O anexo informa a classificação **reconstruída** e o papel do pareador para cada hora, mas não fornece o cadastro completo do dia nem a lista completa de batidas. Assim, abaixo há **candidatos condicionais**, não casos fechados para RED. A identificação `colN`, data, hora e cada resultado atribuído ao anexo estão **[não verificados]**. A exigência de casos pela regra antes do código está em [app/docs/LEIS.md:136](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/LEIS.md:136).

| Razão pretendida | Dois candidatos do anexo; cadastro e batidas disponíveis |
|---|---|
| `TRABALHO_EM_DIA_SEM_TRABALHO` | `col950 15/11 11:25` e `col950 15/11 16:59`: reconstrução `folga_desmentida`; pareador `PAR_FECHADO`. Pela precedência, **se confirmado o dia sem trabalho pela ata**, ambos recebem esta razão. Demais batidas e `tipo_dia` literal: **[não verificado]**. |
| `BATIDA_REPETIDA` | `col836 06/11 08:25` e `col836 06/11 08:38`: reconstrução `eco_cluster`; pareador `PAR_FECHADO`. **Se confirmado dia de trabalho e pertencimento à rajada**, recebem esta razão antes do par fechado. Cadastro literal e demais batidas: **[não verificado]**. |
| `INTERVALO_FORA_DA_ESCALA` | `col203 21/10 12:35` e `col204 17/11 14:44`: reconstrução `geometria`; pareador `NAO_MEDIDA_ORIGEM_PAUSA`; anexo também informa `INTRA_ATA_SIM`. **Se confirmados dia de trabalho e ausência de precedência de rajada**, recebem esta razão. Cadastro literal e demais batidas: **[não verificado]**. |
| `PERIODO_ADICIONAL` | `col174 15/11 05:30` e `col174 15/11 06:33`: reconstrução `geometria`; pareador `PAR_FECHADO`. **Se confirmados dia de trabalho e ausência de rajada**, recebem esta razão. Cadastro literal e demais batidas: **[não verificado]**. |
| `TURNO_ABERTO` | `col174 11/11 18:59` e `col220 11/11 06:00`: reconstrução `geometria`; pareador `TURNO_ABERTO`. **Se confirmados dia de trabalho e ausência de rajada**, recebem esta razão. Cadastro literal e demais batidas: **[não verificado]**. |
| `SAIDA_SEM_ENTRADA` | `col174 12/11 07:01` e `col300 21/10 06:00`: reconstrução `geometria`; pareador `SAIDA_SEM_ENTRADA`. **Se confirmados dia de trabalho e ausência de rajada**, recebem esta razão. Cadastro literal e demais batidas: **[não verificado]**. |

A classificação antiga trata `folga_desmentida`, `geometria` e `eco_cluster` segundo as entradas da grade, inclusive `tipo_dia` e `missing`; o texto raspado não demonstra, por si só, todos esses campos para cada caso ([app/escala/orfa_classe.py:53](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:53)).

## 3. Leitores de órfãs

**Censo parcial: 25 entradas listadas = 25 contadas; o total de 49 não foi comprovado.** A unidade desta lista é um local de leitura ou passagem identificado, não uma contagem demonstrada de todos os consumidores finais. A cópia registra um censo AST anterior e sua remoção, mas nesse trecho não apresenta a lista nominal dos 49 ([app/docs/RELATO.md:10239](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/RELATO.md:10239)). A classificação dos locais abaixo é preliminar onde indicada por **[não verificado]**.

**MOSTRA ou mede — 13 entradas**

1. [app/escala/services/leitor_celula.py:187](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/leitor_celula.py:187) — adapta ata e contagem.
2. [app/escala/utils.py:1054](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:1054) — passagem da grade.
3. [app/ponto/services/espelho.py:702](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:702) — prepara órfãs para o espelho.
4. [app/templates/ponto/espelho.html:151](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/ponto/espelho.html:151) — apresentação.
5. [app/colaboradores/services/calendario.py:406](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/colaboradores/services/calendario.py:406) — marcação no calendário.
6. [app/relatorios/pdf_espelho.py:477](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/relatorios/pdf_espelho.py:477) — apresentação em PDF.
7. [app/relatorios/cartao_pela_celula.py:288](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/relatorios/cartao_pela_celula.py:288) — leitura para cartão.
8. [app/ponto/juiz_batida.py:161](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/juiz_batida.py:161) — exposição intermediária; destino completo **[não verificado]**.
9. [app/ponto/services/esmeril_espelho.py:130](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:130) — medição.
10. [app/escala/services/furos_diarios.py:22](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/furos_diarios.py:22) — contagem.
11. [app/ponto/services/vigia_pareador_celula.py:74](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/vigia_pareador_celula.py:74) — vigia.
12. [app/ponto/management/commands/tripwire_celulas.py:264](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/tripwire_celulas.py:264) — verificação.
13. [app/ponto/services/bordas_realizado.py:49](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/bordas_realizado.py:49) — contador.

**DECIDE — 12 entradas**

1. [app/chamados/juizes.py:514](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/chamados/juizes.py:514) — guarda de furo.
2. [app/ponto/supra_juiz.py:392](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/supra_juiz.py:392).
3. [app/ponto/status_dia.py:188](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/status_dia.py:188).
4. [app/ponto/services/simular_dia.py:12](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/simular_dia.py:12).
5. [app/ponto/propositor.py:39](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/propositor.py:39).
6. [app/ponto/turnos.py:2679](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2679).
7. [app/chamados/services/materializacao.py:449](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/chamados/services/materializacao.py:449).
8. [app/ponto/services/cluster_espurio.py:29](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cluster_espurio.py:29) — assinatura de cluster.
9. [app/ponto/management/commands/detectar_par_relampago.py:140](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/detectar_par_relampago.py:140).
10. [app/ponto/management/commands/propor_reorganizacao.py:104](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/propor_reorganizacao.py:104).
11. [app/ponto/services/cartorio.py:858](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:858).
12. [app/escala/utils.py:1979](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:1979).

**PAGA — 0 entradas comprovadas nesta lista parcial.** Isso não significa que não haja leitores com efeito em dinheiro: a própria descrição do campo `ata` informa consumidores de folha; o caminho completo de cada leitura de órfãs até pagamento ainda precisa ser censado ([app/escala/models.py:1774](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1774)). `folha/causas.py` lê a informação, mas o destino de pagamento não foi demonstrado nesta revisão ([app/folha/causas.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/folha/causas.py:43)).

## 4. Sonda das sobras sem batida viva

A medida anterior informa **36**, mas a causa e a reprodução da medida estão **[não verificadas]**. A sonda abaixo usa a data civil da célula como hipótese operacional: a ata só guarda `HH:MM`, enquanto turnos podem atravessar meia-noite. Portanto, o universo impresso pode diferir dos 36 e **não prova**, sozinho, a atribuição correta das sobras noturnas ([app/ponto/services/cartorio.py:764](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:764); [app/ponto/tests/test_turnos_borda_direita.py:36](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos_borda_direita.py:36)).

A lavra usa `veredito_em`; batidas têm `criado_em` e `retratada_em`; a autoridade de batidas apuráveis exclui retratadas ([app/escala/models.py:1772](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1772); [app/ponto/models.py:63](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/models.py:63); [app/ponto/models.py:79](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/models.py:79); [app/ponto/turnos.py:2872](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2872)). O script foi preparado para Django shell com PostgreSQL em transação própria; execução no envelope da sombra **[não verificada]**.

```python
from collections import Counter
from datetime import datetime, time, timedelta

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa
from escala.models import CelulaDia
from ponto.janelas import janela_fechamento
from ponto.models import Batida
from ponto.turnos import batidas_apuraveis

cont = Counter()
erros = 0
tz = timezone.get_current_timezone()


def minuto(ts):
    return timezone.localtime(ts, tz).strftime("%H:%M")


def estado(valor):
    if valor is None:
        return "indeterminada"
    return "sim" if valor else "nao"


def sonda():
    global erros

    for emp in Empresa.objects.all().order_by("pk").iterator():
        try:
            ini, fim = janela_fechamento(11, 2026, emp)
            celulas = (
                CelulaDia.objects.filter(
                    colaborador__empresa_id=emp.pk,
                    data__gte=ini,
                    data__lte=fim,
                )
                .select_related("colaborador")
                .order_by("pk")
            )

            for cel in celulas.iterator():
                try:
                    horas = (cel.ata or {}).get("orfas") or []
                    if not horas:
                        continue

                    inicio = timezone.make_aware(
                        datetime.combine(cel.data, time.min), tz
                    )
                    final = timezone.make_aware(
                        datetime.combine(cel.data + timedelta(days=1), time.min),
                        tz,
                    )

                    todas = list(
                        Batida.objects.filter(
                            colaborador_id=cel.colaborador_id,
                            timestamp__gte=inicio,
                            timestamp__lt=final,
                        ).only("pk", "timestamp", "criado_em", "retratada_em")
                    )
                    vivas = list(
                        batidas_apuraveis(cel.colaborador)
                        .filter(timestamp__gte=inicio, timestamp__lt=final)
                        .only("pk", "timestamp")
                    )
                    minutos_vivos = {minuto(b.timestamp) for b in vivas}
                    eventos = [
                        evento
                        for b in todas
                        for evento in (b.criado_em, b.retratada_em)
                        if evento is not None
                    ]
                    lavra = cel.veredito_em
                    velha = (
                        None if lavra is None
                        else any(evento > lavra for evento in eventos)
                    )

                    for hora in horas:
                        if not isinstance(hora, str) or len(hora) != 5:
                            raise ValueError("hora invalida")
                        datetime.strptime(hora, "%H:%M")

                        if hora in minutos_vivos:
                            continue

                        retratadas = [
                            b for b in todas
                            if b.retratada_em is not None
                            and minuto(b.timestamp) == hora
                        ]
                        depois = (
                            None if lavra is None
                            else any(b.retratada_em > lavra for b 

[truncado em 20000 de 23403 caracteres]

## 19/11 10:17 · sessao 1678d57c124c · - · - · origem=Ronald (tmux)

Script e referências enviados ao agente principal, com a ressalva de que a ata conserva apenas `HH:MM` e não identifica a data civil da sobra em turnos que atravessam meia-noite ([cartorio.py:764](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:764); [utils.py:641](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:641)). O universo resultante e a execução do script permanecem [não verificado].

## 19/11 10:16 · sessao 8b94c50458ba · - · - · origem=Ronald (tmux)

O censo solicitado de 49 leitores não ficou comprovado: a cópia registra um instrumento de censo anterior e sua remoção, mas não apresenta ali a lista nominativa ([app/docs/RELATO.md:10239](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/RELATO.md:10239)). A lista parcial e as classificações verificadas foram enviadas ao agente principal.

## 19/11 10:16 · sessao 6354fae5edb3 · - · - · origem=Ronald (tmux)

Investigação da matriz e referências enviadas ao agente principal.

## 19/11 10:14 · sessao b238cb6bdf2b · CADEIA-20-PASSO-1 · verificador · origem=Code

**AVAL-DEVOLVIDO — PROCEDE nos itens 1, 2 e 3.** A principal lacuna é que a cópia não demonstra o corte nem o registro do juiz `razao_da_sobra`, e o retorno do pareador não cobre necessariamente cada sobra da ata ([app/core/juizes.py:116](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/core/juizes.py:116); [app/ponto/turnos.py:1371](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1371)).

1. **PROCEDE — as premissas conferem apenas em parte.**

   - **(a) Juiz e chamadores.** `classificar_orfas` existe em [app/escala/orfa_classe.py:53](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:53) e a busca na cópia encontrou **zero chamadas de produção**; os usos encontrados são testes, enquanto produção importa a constante do módulo ([app/escala/tests/test_orfa_classe.py:20](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_orfa_classe.py:20); [app/escala/utils.py:504](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:504)). Não localizei `razao_da_sobra` em `core/juizes.py` nem o corte `RAZAO-DA-SOBRA-1811-1910` nos arquivos consultados: **[não verificado]**. A função atual devolve quatro classes diferentes das seis razões propostas ([app/escala/orfa_classe.py:21](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:21)).
   - **(b) Ata.** `ata_do_dia` monta `tipo_dia`, `lampadas`, minutos previstos e realizados, `sem_ancora`, `n_orfas`, `orfas` e `n_missing` ([app/ponto/services/cartorio.py:749](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:749)). A lista guarda **horas em texto `HH:MM`**, não identificação inequívoca nem instante completo por batida; a grade também formata as sobras nesse formato ([app/escala/utils.py:673](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:673)). Existência de batida viva correspondente para cada hora: **[não verificado]**.
   - **(c) Pareador.** Ele devolve `Turno(data_turno, entrada, saida, batidas, aberto, cross_meianoite)` ([app/ponto/turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43)). Há carimbos de intervalo nas instâncias, e a forma do turno permite reconhecer várias das situações ([app/ponto/turnos.py:1310](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1310); [app/ponto/turnos.py:312](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:312)). Mas **não devolve um papel para cada entrada**: uma batida absorvida como intervalo com `cur is None` segue por `continue` sem entrar em turno ([app/ponto/turnos.py:1371](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1371)). Assim, a suficiência para todas as sobras, sem julgamento complementar, não confere.
   - **(d) Fontes de cadastro.** `dias_cadastro_x_realidade` existe no resultado do motor e é lida para a lavra CXR ([app/ponto/motor_calculo_v2.py:289](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:289); [app/ponto/services/cxr_na_lista.py:77](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cxr_na_lista.py:77)). A lista existe e a incorpora como C12 ([app/escala/services/cadastro_realidade.py:360](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:360); [app/escala/services/cadastro_realidade.py:943](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:943)). Porém, estar na lista como pessoa não identifica necessariamente um dia: `dias_na_lista` inclui apenas assinaturas com data e é lida pelo vigia ([app/escala/services/cadastro_realidade.py:1135](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:1135); [app/chamados/services/vigia_fio.py:195](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/chamados/services/vigia_fio.py:195)).
   - **(e) Escritor.** Confere a porta existente: o cartório monta a ata e a entrega a `cel.lavrar_veredito`; o método delega à porta da célula, e `ata` é JSON ([app/ponto/services/cartorio.py:1898](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:1898); [app/escala/models.py:1774](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1774); [app/escala/models.py:1819](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1819)). Há lugar para gravar a razão no mesmo ato; isso não prova que todos os insumos necessários estejam disponíveis ali.
   - **(f) Rajada.** Há limiar existente: a classificação antiga agrupa órfãs consecutivas em até 15 minutos e exige pelo menos duas ([app/escala/orfa_classe.py:26](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:26); [app/escala/orfa_classe.py:40](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:40)). O leitor de produção de cluster decide **por dia**, exige também lâmpada apagada e devolve a lista inteira, não quais batidas compõem cada rajada ([app/ponto/services/cluster_espurio.py:15](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cluster_espurio.py:15)). Portanto, **número novo não é inevitável**, mas a equivalência dessa decisão à razão individual proposta permanece **[não verificado]**.

2. **PROCEDE — o corte alegado não foi comprovado nesta cópia; os cortes encontrados impõem condições.**

   A trava exige a frase de nascimento do juiz em `CORTES.md` para registrar uma autoridade nova ([CLAUDE.md:74](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/CLAUDE.md:74)). O corte específico `RAZAO-DA-SOBRA-1811-1910` e o registro correspondente no juiz vivo estão **[não verificados]**. A **L-111** manda apagar função sem chamador de produção no ato, enquanto a **L-110** exige casos respondidos pela regra antes do código ([app/docs/LEIS.md:136](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/LEIS.md:136); [app/docs/LEIS.md:137](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/LEIS.md:137)).

   Para a lavra, a **L-061** estabelece a leitura da lâmpada pelos consumidores ([app/docs/LEIS.md:95](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/LEIS.md:95)); o corte **DST-L2-1800** determina a consulta do cartório ao juiz de dia sem trabalho ([app/docs/CORTES.md:550](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/CORTES.md:550)). Para o pareador, a **L-032** protege o intervalo cadastrado ([app/docs/LEIS.md:80](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/LEIS.md:80)). Não demonstram, por si, colisão da nova razão com esses cortes; demonstram que a declaração de juiz já cortado ainda precisa ser comprovada.

3. **PROCEDE — há condições que não fecham literalmente.**

   - **Juízes novos = 0:** a existência da função antiga não comprova que o juiz das seis razões e da junção proposta já tenha sido cortado ou registrado ([app/escala/orfa_classe.py:53](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:53); corte e registro: **[não verificado]**).
   - **Uma razão por sobra, sem tocar o pareador:** o pareador pode não devolver uma batida em turno, e a ata identifica sobras por hora, enquanto o próprio aval informa 36 sobras sem batida viva correspondente ([app/ponto/turnos.py:1371](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1371); [app/ponto/services/cartorio.py:764](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:764); número 36: dado fornecido, causa **[não verificado]**). A regra para esses casos não está demonstrada.
   - **Rajada:** há números anteriores, mas o leitor vivo responde uma assinatura de dia, com guarda de lâmpada apagada, e não entrega a classificação individual pedida ([app/ponto/services/cluster_espurio.py:19](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cluster_espurio.py:19)). Logo, não se pode afirmar que basta ler sua resposta por batida.
   - **Tela:** “nunca chama o juiz” é compatível como alvo, mas não descreve toda a leitura atual: o calendário obtém órfãs da grade e marca batidas por `HH:MM`; o espelho também monta `orfas_grade` ([app/colaboradores/services/calendario.py:406](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/colaboradores/services/calendario.py:406); [app/ponto/services/espelho.py:702](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:702)).
   - **DIFF de folha da comp. 11:** a medida é conceitualmente aplicável, mas o comando existente fotografa a **competência corrente** e não oferece argumento de mês/ano ([app/folha/management/commands/simular_folha.py:94](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/folha/management/commands/simular_folha.py:94); [app/folha/management/commands/simular_folha.py:234](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/folha/management/commands/simular_folha.py:234)). Que a corrente seja a 11 para cada empresa no momento da execução é **[não verificado]**. O comparador inclui TXT e retidos ([app/folha/management/commands/simular_folha.py:202](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/folha/management/commands/simular_folha.py:202)).

4. **CONFERE, com limite de verificação — não há toque adicional demonstrado pelo escopo declarado.** O aval proíbe alterar o motor e quem paga e prevê relavra da competência 11 **na sombra**; o código mostra que a ata alimenta leitores de folha e que a relavra pode ter efeitos além do campo pretendido, tornando o DIFF necessário ([app/escala/models.py:1774](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1774); [app/ponto/services/cartorio.py:233](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:233)). Alcance real a exportada, dinheiro, escala ou vínculo é **[não verificado]**; a **L-092** protege o gravado exportado e a **L-009** trata os atos que exigem autorização ([app/docs/LEIS.md:125](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/LEIS.md:125); [app/docs/LEIS.md:67](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/LEIS.md:67)).

O hash informado não pôde ser confirmado por Git: esta cópia não se apresentou como repositório Git **[não verificado]**.

## 19/11 10:08 · sessao 61acbd037746 · DST-L2-JZ1-C6 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 3 PROCEDE.** Os itens 1, 2 e 4 conferem com as ressalvas abaixo; esta revisão é estática, sem execução nem acesso a produção.

1. **CONFERE**, com a distinção pedida em **(b)**.

   - **(a)** `justificativa_alheia` existe e, durante `_APROVANDO`, verifica apenas se o ponteiro do chamado aponta para outra justificativa pendente: [reconciliador.py:439](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/chamados/reconciliador.py:439), [reconciliador.py:452](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/chamados/reconciliador.py:452).
   - **(b)** Não há uma função separada que expresse o D3 nesse caminho. O vínculo aparece **duas vezes em linha**, em formas diferentes: a consulta em lote constrói `origem OU ponteiro` e filtra pendentes; depois a associação de cada candidata aos seus chamados repete o vínculo em Python: [justificativa.py:302](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:302), [justificativa.py:314](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:314), [justificativa.py:320](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:320).
   - **(c)** O efeito é possível, sujeito a o chamado chegar à retratação e a transição ser aceita; não é consequência incondicional de toda aprovação. `aprovar_justificativa_dp` abre o contexto `no_ato_de_aprovar(jid)`, que marca `_APROVANDO`, e chama o encerramento da origem: [justificativa.py:132](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:132), [justificativa.py:205](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:205), [reconciliador.py:426](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/chamados/reconciliador.py:426). A origem chama `retratar`; a retenção deixa passar quando o ponteiro é J1, pois ele é o `_APROVANDO`: [justificativa.py:33](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:33), [reconciliador.py:452](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/chamados/reconciliador.py:452), [reconciliador.py:538](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/chamados/reconciliador.py:538). Aceita a transição, o reconciliador chama o rejeitador sem consultar novamente o portão; este exclui J1, mas alcança J3 pela origem e a rejeita: [reconciliador.py:563](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/chamados/reconciliador.py:563), [justificativa.py:302](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:302), [justificativa.py:329](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:329).

2. **CONFERE**, quanto à mudança de comportamento declarada. O D3 vigente abrange ponteiro e origem, e o próprio escritor documenta a retenção como portão comum; ampliar essa retenção para proteger J3 não exige reduzir o universo do rejeitador: [justificativa.py:262](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:262), [justificativa.py:281](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:281). A **L-002** favorece uma autoridade compartilhada, e a **L-011** disciplina o registro de autoridade nova: [LEIS.md:60](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/docs/LEIS.md:60), [LEIS.md:69](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/docs/LEIS.md:69). A questão de se a extração exigida cabe em “juízes novos = 0” permanece no item 3.

3. **PROCEDE — a condição “uma função só” versus “juízes novos = 0” não fecha de forma verificável no literal.** O predicado está em linha, inclusive em duas representações, de modo que não há função existente para os dois consumidores simplesmente chamarem: [justificativa.py:302](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:302), [justificativa.py:320](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:320). Uma função extraída pode ser entendida como nova autoridade para a pergunta; o precedente **JUIZ-TEM-GPS-NASCE** extraiu expressão em linha e mandou registrá-la, enquanto a autonomia para declarar *sem juiz novo* exige função já existente e autoridade única medida: [CORTES.md:465](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/docs/CORTES.md:465), [CLAUDE.md:276](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/../CLAUDE.md:276). **Não está demonstrado que toda extração conte necessariamente como juiz novo**; é essa classificação que o aval precisa resolver para a condição zero ficar verificável.

   O mutante também precisa ser especificado sobre a forma que vier a existir: hoje a retenção já olha só o ponteiro, e uma alteração da função compartilhada poderia mudar também o D3 do rejeitador. O RED pedido é pertinente, mas o texto não identifica ainda uma mutação isolada da retenção após a unificação: [reconciliador.py:452](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/chamados/reconciliador.py:452), [justificativa.py:302](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/ponto/services/justificativa.py:302).

4. **CONFERE quanto ao escopo declarado**, com efeito de produção **[não verificado]**. O objeto declarado é a retenção e o vínculo da justificativa com o chamado; o caminho atual de retratação passa por uma cerca que pode recusar escritas em competência entregue: [reconciliador.py:538](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/app/chamados/reconciliador.py:538). Não há diff da c6 para demonstrar seus efeitos indiretos em exportada, dinheiro, escala ou vínculo **[não verificado]**. Se a implementação tocar o gravado de exportada, dados de escala ou vínculo, ou regra fora do pedido, a regra vigente exige `!`: [CLAUDE.md:265](/home/ronald/codex_ws/c1506fddad441510164a066974525efb904c23ae/CLAUDE.md:265).

## 19/11 09:38 · sessao 9ce3c4a07f09 · E4C-F5-CONSTROI · construtor · origem=Code

A volta foi preparada na cópia: a tag voltou ao conteúdo de `3308f7599`, as duas linhas que leem texto permanecem como na base e receberam o comentário pedido. O teste de histórico agora verifica os carimbos sem afirmar qual texto deve prevalecer. ([fio.py:190](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/services/fio.py:190), [historico.py:110](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/services/historico.py:110), [teste:64](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:64))

Os testes de custo chamam as cinco funções reais, com uma e cinco perguntas, sem prefetch montado pelo teste. O selo inclui fio e histórico; no histórico, permite `p.materializada_em` e detecta a leitura crua indireta por `getattr`. ([teste:76](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:76), [teste:141](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:141)) [nao verificado]

**Verificação:** análise sintática dos três arquivos Python alterados e `git diff --check 3308f7599 -- .` passaram. Banco, suíte, RED, GREEN e mutantes não foram executados; resultados esperados **[não verificado]**.

Diff final, a partir de `app/`:

```bash
git diff 3308f7599 -- .
```

Para RED, use uma cópia independente em `3308f7599`, copie para ela `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py` desta entrega e execute, em `app/`:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_fio_ato_responde_e_projecao_pendente \
  chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_red_5_historico_resposta_validacao_e_materializacao \
  chamados.tests.test_e4c_f5_volta1_leem_o_ato.SeloTest
```

GREEN na cópia entregue:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato
```

Mutantes em memória; cada comando deve falhar na asserção de negócio correspondente **[não verificado]**:

```bash
E4C_F5_MUTANTE=fio PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_fio_ato_responde_e_projecao_pendente

E4C_F5_MUTANTE=historico PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_red_5_historico_resposta_validacao_e_materializacao
```

Para os mutantes textuais abaixo, aplique **um por vez em cópia descartável**, a partir de `app/`, e restaure o arquivo antes do próximo. Cada teste indicado deve falhar **[não verificado]**: [nao verificado]

```bash
# cobranca
sed -i "s/^[[:space:]]*\\.prefetch_related('decisoes'))/          )/" chamados/services/cobranca.py
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_custo_cobranca

# classificar
sed -i "/^[[:space:]]*prefetch_related_objects(perguntas, 'decisoes')/d" chamados/relatorios_disputa.py
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_custo_classificar

# coletar
sed -i "s/, 'perguntas__decisoes'//" chamados/relatorios_disputa.py
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_custo_coletar

# historico: custo
sed -i "s/\\.prefetch_related('decisoes')//" chamados/services/historico.py
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_custo_historico

# fio: custo
sed -i "/                'chamado_destino__celula').prefetch_related(/,/                'validada_original_por', 'ato_por'))).order_by('id'):/c\\                'chamado_destino__celula').order_by('id'):" chamados/services/fio.py
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_custo_fio

# historico: leitura crua, mordida pelo selo
sed -i 's/getattr(leitura, campo)/getattr(p, campo)/' chamados/services/historico.py
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.SeloTest
```

LEI-AKITA: `origem=chamados/services/{fio,historico,cobranca}.py + chamados/relatorios_disputa.py; testemunha=DecisaoPergunta.leitura; quem-mais-le=tag e acoes_chamado.py:350 (volta 2); juizes novos=0; padrao=projecao reconstruivel / CQRS`. O termo consta em [PLANO-PADROES.md:57](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/docs/PLANO-PADROES.md:57).

## 19/11 09:25 · sessao a2a588345462 · ESPURIA-TELA-C11 · verificador · origem=Code

**A triagem do chip procede, com uma ressalva sobre a recusa:** no caso rejeitado em folga, o código decide `REJEITADO` e deixa `classe_dia` vazia; porém o card de `espelho.html` não imprime a palavra que identificaria a recusa. O selo de quem participou da ausência aparece na fixture, mas não distingue uma recusa de uma aprovação. ([dia_decidido.py:397](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/dia_decidido.py:397), [espelho.html:171](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/espelho.html:171), [_decidiu_icone.html:9](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/partials/_decidiu_icone.html:9).)

**a. Lei e apresentação da recusa.** A ordem escrita é cobertura, decisão rejeitada, depois tipo do dia. A triagem de manter o chip vazio concorda com essa ordem e com o aval informado na pergunta; não identifiquei lei escrita que exija o chip “Folga” nessa combinação. A lei registrada exige a palavra do veredito na **grade e no PDF**, e esses leitores têm código para desenhá-la. ([dia_decidido.py:397](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/dia_decidido.py:397), [CORTES.md:297](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/docs/CORTES.md:297), [_calendario_grade.html:97](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/colaboradores/partials/_calendario_grade.html:97), [pdf_espelho.py:923](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/relatorios/pdf_espelho.py:923); conteúdo independente do aval [não verificado].)

No **card do espelho**, nenhum elemento identifica inequivocamente o estado como recusado: o template não lê `palavra_dia` nem `veredito_dia`; o chip de ausência lê `dia.ausencia`, que vem da cobertura de cobrança não rejeitada; e o selo diz “por quem”, com título que admite aprovação **ou** recusa. Portanto, no caso da fixture há um rótulo de autoria, mas não um rótulo de recusa. Isso limita a interpretação de “a recusa não some do dia” no comentário do juiz; a omissão da palavra no espelho já existia e não foi introduzida pelo delta de teste. ([espelho.html:191](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/espelho.html:191), [espelho.py:594](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/espelho.py:594), [espelho.py:859](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/espelho.py:859), [_decidiu_icone.html:9](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/partials/_decidiu_icone.html:9), [dia_decidido.py:400](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/dia_decidido.py:400).)

**b. Mordida e fixture.** Sim: o teste exige expressamente `veredito_dia == REJEITADO`, `folga=True`, título de classe `None` e ausência do chip e do ícone de classe nas duas cascas. A decisão é construída por `cartao_do_dia` sobre uma ausência rejeitada, como no carregador de produção; a fixture, contudo, injeta os fatos, a folga e a decisão, sem testar a consulta ou a montagem integral de produção. ([test_espelho_decidiu_icone.py:185](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/tests/test_espelho_decidiu_icone.py:185), [test_espelho_decidiu_icone.py:217](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/tests/test_espelho_decidiu_icone.py:217), [dia_decidido.py:578](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/dia_decidido.py:578).)

O resumo fornecido relata RED e M1 com falha desse caso nas duas cascas e GREEN de 51 testes. Isso sustenta a triagem da evidência **como relato fornecido**; arquivos de log originais, comandos, identidade das árvores e execução independente permanecem **[não verificado]**. O código do teste demonstra por que o retorno do chip a `dia.folga` deve falhar nesse caso. ([test_espelho_decidiu_icone.py:221](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/tests/test_espelho_decidiu_icone.py:221); anexo transcrito sem linhas próprias [não verificado].)

**c. Falha de fatos.** A triagem está correta quanto ao código: com `_fatos_do_dia=None`, a porta não calcula o veredito nem a classe inicializada em `None`; chip e ícone de classe, ambos dependentes dela, somem juntos. O comentário prevê o dia sem palavra nessa falha, e o aval informado manda o chip seguir a classe; não identifiquei proibição desse desaparecimento conjunto. A ocorrência em produção é **[não verificado]**. ([espelho.py:337](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/espelho.py:337), [espelho.py:909](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/espelho.py:909), [espelho.html:171](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/espelho.html:171), [espelho.html:193](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/espelho.html:193); conteúdo independente do aval [não verificado].)

**Achados**

- **NÃO BLOQUEIA — rejeitada em folga:** retiro o bloqueio da volta 1 para a perda do chip “Folga”; a ordem escrita e a premissa agora fixada sustentam a triagem. ([dia_decidido.py:397](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/dia_decidido.py:397), [test_espelho_decidiu_icone.py:217](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/tests/test_espelho_decidiu_icone.py:217).)
- **NÃO BLOQUEIA o delta — recusa não identificada no card:** o selo mostra autoria, sem dizer qual foi a decisão; a omissão da palavra é preexistente e merece triagem própria. ([espelho.html:191](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/espelho.html:191), [_decidiu_icone.html:9](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/partials/_decidiu_icone.html:9).)
- **NÃO BLOQUEIA — evidência:** o resumo supre o relato solicitado de RED, GREEN e M1, com as limitações de verificação acima. [não verificado]
- **NÃO BLOQUEIA — falha de fatos:** chip e ícone seguem a mesma classe vazia, conforme a porta e o escopo informado. ([espelho.py:337](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/services/espelho.py:337), [espelho.html:171](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/templates/ponto/espelho.html:171).)

Não identifiquei outro achado novo bloqueante no delta fornecido; sua correspondência integral com um arquivo de patch independente é **[não verificado]**. ([test_espelho_decidiu_icone.py:185](/home/ronald/codex_ws/b81bd0b4863f37370da36316dd6682f70942b9bc/app/ponto/tests/test_espelho_decidiu_icone.py:185).)

## 19/11 09:22 · sessao cb483d0a71df · ESPURIA-TELA-C11 · verificador · origem=Code

**Resultado: BLOQUEIA.** O chip e o ícone passam a ler a mesma `classe_dia`, mas essa classe não significa sempre “o fato que venceu na precedência”: uma decisão de ausência rejeitada pode produzir `veredito_dia=rejeitado` antes de consultar `tipo_do_dia`. Assim, um dia que a precedência julga folga e cuja grade tem `folga=True` pode perder o chip “Folga” que tinha antes do diff. A ocorrência em produção não foi fornecida. ([dia_decidido.py:397](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/dia_decidido.py:397), [espelho.py:353](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:353), [espelho.html:171](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:171); diff fornecido: [não verificado] como arquivo independente.)

A cópia contém o texto novo do diff, mas não contém metadados Git acessíveis; a identidade com o hash informado não foi verificada. O anexo `leis_sitios.txt` veio vazio, e os anexos transcritos não têm linhas próprias; suas afirmações são tratadas como dado fornecido, sem atribuir-lhes linhas inexistentes. Logs, execução dos testes, mutantes e dados de produção: **[não verificado]**. [nao verificado]

### 1. Escritores, leitores e significado

- **`dia.folga`**: o montador escreve um booleano que significa que a grade disponível diz `tipo_dia == 'folga'`; grade futura ou fora da janela é descartada antes. O card e `tem_card` usam esse fato de grade para existência; o chip antigo também o usava como rótulo, significado diferente da classe julgada. O diff retira essa leitura direta do chip. ([espelho.py:687](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:687), [espelho.py:784](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:784), [espelho.py:152](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:152), [espelho.html:165](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:165); versão antiga no diff fornecido: [não verificado] como arquivo independente.)

- **`dia.veredito_dia`**: nasce como string vazia e a porta escreve o veredito devolvido por `do_dia`; o leitor novo indireto é `icone_da_classe`, chamado com esse mesmo retorno. É um veredito visual composto: cobertura e rejeição são examinadas antes de `tipo_do_dia`. Portanto, não equivale em todos os casos ao vencedor puro da precedência. A API também transporta o campo. ([espelho.py:909](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:909), [espelho.py:340](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:340), [dia_decidido.py:397](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/dia_decidido.py:397), [views.py:1393](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/api/views.py:1393).)

- **`dia.classe_dia` e `.title`**: o montador inicializa a classe como `None`; a porta a substitui por `icone_da_classe(veredito)`, que devolve dicionário apenas para os vereditos `folga` e `feriado`, com `title` vindo de `PALAVRA`. O chip lê `.title`; o partial lê o mesmo título, tipo, cor e desenho. O significado é coerente entre esses dois leitores, mas herda a distinção acima entre veredito visual e precedência. ([espelho.py:909](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:909), [espelho.py:345](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:345), [dia_decidido.py:206](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/dia_decidido.py:206), [espelho.html:171](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:171), [_dia_classe_icone.html:6](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/partials/_dia_classe_icone.html:6).)

A view de produção examinada recebe os dias de `espelho_do_colab`, que chama `aplicar_palavra_do_dia` antes de retorná-los; não encontrei nessa view um segundo caminho que entregue dias sem passar pela porta. **Passar pela porta, porém, não garante classe:** se a coleta de fatos falhar ou não devolver o dia, `_fatos_do_dia` fica `None`, a porta não escreve a classe e ela permanece `None`. Uma folga da grade pode então conservar o card e perder chip e ícone. Frequência e ocorrência reais: **[não verificado]**. ([views.py:160](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/views.py:160), [views.py:195](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/views.py:195), [espelho.py:658](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:658), [espelho.py:884](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:884), [espelho.py:337](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:337), [espelho.py:1307](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:1307).)

### 2. Chip versus ícone; perda de “Folga”

Dentro de um card renderizado, **não encontrei caminho em que o novo chip diga um título diferente do ícone**: ambos dependem da mesma classe e do mesmo `.title`. Podem, contudo, sumir juntos quando essa classe é `None`. ([espelho.html:171](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:171), [espelho.html:193](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:193), [_dia_classe_icone.html:6](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/partials/_dia_classe_icone.html:6).)

**Sim, “Folga” pode sumir mesmo com a precedência julgando folga:** com grade `folga=True`, fatos `tipo_do_dia='folga'`, sem cobertura e com decisão rejeitada, o veredito visual é `rejeitado`, portanto `classe_dia=None`. O carregador de decisões inclui rejeitadas; o dado fornecido não comprova uma ocorrência concreta desse conjunto. A falha na coleta de fatos oferece outro caminho de perda, sem afirmar que a precedência foi efetivamente calculada naquela execução. ([dia_decidido.py:397](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/dia_decidido.py:397), [dia_decidido.py:578](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/dia_decidido.py:578), [dia_decidido.py:206](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/dia_decidido.py:206), [espelho.py:337](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:337).)

### 3. Classes de reprovação e selo

O diff fornecido altera a leitura do template e o teste; nele não há novo juiz, derivação paralela ou cálculo de precedência na tela. A autoridade registrada para precedência é `fatos_do_dia`, e a classe existente traduz o veredito visual; a ressalva é a **diferença semântica** acima, não uma segunda implementação introduzida pelo diff. ([JUIZES.md:123](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/docs/JUIZES.md:123), [dia_decidido.py:206](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/dia_decidido.py:206), [espelho.html:171](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:171); escopo do diff fornecido: [não verificado] como arquivo independente.)

O teste **estrutura uma mordida válida para M1**: monta `folga=True`, injeta fatos de feriado, espera chip “Feriado” e presença do ícone nas duas cascas; devolver o chip à condição e literal antigos `dia.folga → Folga` contrariaria essa asserção em ambas. O fixture usa a porta de produção, mas injeta manualmente tanto `folga=True` quanto os fatos: ele não prova que o montador de produção cria essa combinação nem cobre rejeitada sobre folga ou falha de fatos. RED executado, GREEN, morte do mutante e logs em disco: **[não verificado]**. ([test_espelho_decidiu_icone.py:183](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/tests/test_espelho_decidiu_icone.py:183), [test_espelho_decidiu_icone.py:207](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/tests/test_espelho_decidiu_icone.py:207).)

### 4. Card e chip

Pode haver **card sem chip**: por exemplo, `folga=True` mantém o card, enquanto uma ausência ou rejeição pode deixar a classe vazia. Pode também haver uma classe de feriado sem card, se nenhum dos campos do portão estiver presente; nesse caso chip e ícone ficam ambos dentro do card que não é renderizado. A decisão registrada na cópia diz expressamente que o card do feriado fica como está e a abertura vai para backlog; isso é **fila, não bloqueio do MUDA literal do chip**. A regressão de folga com rejeitada descrita acima é um achado separado. ([espelho.html:165](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:165), [espelho.html:171](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:171), [RELATO.md:206](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/docs/RELATO.md:206).)

### 5. Sítios da fila

**Não identifiquei, entre os sítios declarados, outro rótulo de tipo do dia no próprio template do espelho lendo a grade diretamente.** O `:165` é portão de card; a faixa `:151` usa `tem_card` e status lavrado; o `:191` mostra ausência. A grade do calendário de colaboradores contém seus próprios ícones de folga e feriado e é outra tela; PDF e API têm leitores próprios identificáveis no código. A exaustividade do censo e a contagem de “13 sítios” não são demonstráveis pelo anexo transcrito: **[não verificado]**. ([espelho.html:151](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:151), [espelho.py:152](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/espelho.py:152), [espelho.html:191](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:191), [_calendario_grade.html:106](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/colaboradores/partials/_calendario_grade.html:106), [pdf_espelho.py:870](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/relatorios/pdf_espelho.py:870), [views.py:1393](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/api/views.py:1393).)

### Achados

- **BLOQUEIA — significado da testemunha:** rejeitada pode vencer no veredito visual mesmo quando a precedência diz folga, fazendo desaparecer o chip antigo. O teste novo não cobre essa combinação. ([dia_decidido.py:397](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/services/dia_decidido.py:397), [test_espelho_decidiu_icone.py:211](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/tests/test_espelho_decidiu_icone.py:211).)
- **BLOQUEIA — evidência de pronto ausente:** execução do RED, GREEN e M1 com logs, verificações finais e identidade do hash são **[não verificado]**; o código do teste, isoladamente, demonstra a lógica da mordida, não sua execução. ([test_espelho_decidiu_icone.py:207](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/ponto/tests/test_espelho_decidiu_icone.py:207).)
- **NÃO BLOQUEIA o MUDA literal — portão do card:** pode ocultar feriado sem outros sinais, mas há decisão registrada para manter o comportamento e colocá-lo em backlog. ([RELATO.md:206](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/docs/RELATO.md:206).)
- **NÃO BLOQUEIA como novo juiz — diff examinado:** chip e ícone compartilham a classe existente; não há cálculo novo de precedência no template. A falha semântica dessa testemunha permanece o primeiro bloqueio. ([espelho.html:171](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:171), [espelho.html:193](/home/ronald/codex_ws/05d3f0b8b163311cad50278f9ea76c666cc21637/app/templates/ponto/espelho.html:193).)

## 19/11 09:14 · sessao 17c06159cb79 · ESPURIA-TELA-C11 · construtor · origem=Code

Implementei a alteração no template e preparei os testes, mas **RED, GREEN e mutantes permanecem sem veredito**: a tentativa local parou antes dos testes porque o Python disponível não tem Django (`ModuleNotFoundError: No module named 'django.core'`, em [manage.py:11](/home/ronald/wt-codex-espuria-tela-c11-091200/app/manage.py:11)). [nao verificado]

Arquivos alterados:

- [test_espelho_decidiu_icone.py:185](/home/ronald/wt-codex-espuria-tela-c11-091200/app/ponto/tests/test_espelho_decidiu_icone.py:185): fixture com `folga=True` e casos para chip e card nas duas cascas, incluindo conteúdo que deve manter o card.
- [espelho.html:165](/home/ronald/wt-codex-espuria-tela-c11-091200/app/templates/ponto/espelho.html:165): o card passa a considerar `dia.classe_dia`; o chip imprime `dia.classe_dia.title` em [espelho.html:171](/home/ronald/wt-codex-espuria-tela-c11-091200/app/templates/ponto/espelho.html:171).

Diff de produção:

```diff
-  {% if dia.batidas or dia.turnos or dia.previsto or dia.previsto_canon or dia.folga or dia.ausencia %}
+  {% if dia.batidas or dia.turnos or dia.previsto or dia.previsto_canon or dia.classe_dia or dia.ausencia %}
...
-        {% if dia.folga %}<span ...>Folga</span>{% endif %}
+        {% if dia.classe_dia %}<span ...>{{ dia.classe_dia.title }}</span>{% endif %}
```

O diff completo está disponível com `git diff -- app/ponto/tests/test_espelho_decidiu_icone.py app/templates/ponto/espelho.html`, executado na raiz da cópia. A sintaxe Python foi analisada com `ast.parse` e `git diff --check` passou. O brief físico não estava no caminho indicado; usei o brief integral fornecido no enunciado. [não verificado]

Para a sessão executar o RED, use uma **cópia descartável** contendo o teste alterado e o template da base. Na raiz dessa cópia: [nao verificado]

```bash
git show 9195f5124367e55f3c594a86d383d406f3258b20:app/templates/ponto/espelho.html > app/templates/ponto/espelho.html
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir "$PWD" --only "ponto.tests.test_espelho_decidiu_icone"
```

A falha esperada é a divergência do chip em feriado-em-folga e a existência indevida do card sem classe ou conteúdo; **essas falhas não foram observadas**. [não verificado]

Para GREEN, restaure nessa cópia o template alterado desta entrega e execute:

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir "$PWD" --only "ponto.tests.test_espelho_decidiu_icone ponto.tests.test_contract_espelho_sem_emoji ponto.tests.test_palavra_parcial_e_pendente"
```

Para cada mutante, parta de outra cópia descartável com o GREEN aplicado. Execute o bloco correspondente na raiz da cópia e depois o comando de teste: [nao verificado]

```bash
# M1: chip volta a ler dia.folga
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from pathlib import Path
p = Path('app/templates/ponto/espelho.html')
s = p.read_text()
a = '{% if dia.classe_dia %}<span style="font-size:10px;padding:2px 8px;border-radius:999px;background:#f1f5f9;color:#64748b;font-weight:600;">{{ dia.classe_dia.title }}</span>{% endif %}'
b = '{% if dia.folga %}<span style="font-size:10px;padding:2px 8px;border-radius:999px;background:#f1f5f9;color:#64748b;font-weight:600;">Folga</span>{% endif %}'
assert s.count(a) == 1
p.write_text(s.replace(a, b))
PY
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir "$PWD" --only "ponto.tests.test_espelho_decidiu_icone"
```

```bash
# M2: card volta a ler dia.folga
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from pathlib import Path
p = Path('app/templates/ponto/espelho.html')
s = p.read_text()
a = 'dia.previsto_canon or dia.classe_dia or dia.ausencia'
b = 'dia.previsto_canon or dia.folga or dia.ausencia'
assert s.count(a) == 1
p.write_text(s.replace(a, b))
PY
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir "$PWD" --only "ponto.tests.test_espelho_decidiu_icone"
```

Os mutantes não foram executados e suas mortes não foram demonstradas. Também não rodei os testes vizinhos, CHECK-DE-TELA ou suite. [não verificado] [nao verificado]

Perguntas de lei: **0**, conforme o brief fornecido. [não verificado]

## 19/11 09:12 · sessao 93e5bc7468ff · ESPELHO-VERDADE · sonda · origem=Code

O contador de `engolidos()` independe dos handlers: `registrar_engolido` incrementa o total antes de chamar o logger. [app/core/observ.py:60](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/core/observ.py:60) [app/core/observ.py:73](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/core/observ.py:73)

A execução, as classes, as reconfirmações e `ERROS` permanecem **[não verificado]**: o dado raspado fornece o universo, mas não os resultados dessa sonda. O bloco abaixo é para o envelope Django da sombra; não foi executado aqui. **[não verificado]**

Limites dos predicados: `BATIDA_PLANTADA` identifica apenas `pergunta_origem` preenchida; batidas são examinadas pelo dia civil local, perguntas por `data_evento`, e `FOLGA_TRABALHADA` pelo status **lavrado** da célula. Esses recortes não provam ausência de casos fora deles. [app/ponto/models.py:30](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/models.py:30) [app/ponto/services/dia_do_colab.py:64](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/services/dia_do_colab.py:64) [app/escala/models.py:1816](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/models.py:1816)

```python
# Sonda EV V2: executar no envelope Django da sombra, tenant juliani.
# Python -B; nenhum nome ou CPF é impresso.
#
# Precedência: PERGUNTA_VIVA, BATIDA_RETRATADA, BATIDA_PLANTADA,
# ORFA_NO_DIA, AUSENCIA_NO_DIA, FERIADO_TRABALHADO,
# FOLGA_TRABALHADA, CELULA_REGENERADA, CELULA_EDITADA,
# CRUZA_MEIA_NOITE, OUTROS.
#
# As classes descrevem particularidades observadas, não causas provadas.
# Não se associa par do oráculo a período do motor.

import sys
sys.dont_write_bytecode = True

import logging

# Remove handlers de arquivo já instalados, inclusive em loggers nomeados.
# Não usa logging.disable: registrar_engolido continua contando.
for _logger in [logging.getLogger()] + [
    x for x in logging.Logger.manager.loggerDict.values()
    if isinstance(x, logging.Logger)
]:
    for _handler in list(_logger.handlers):
        if isinstance(_handler, logging.FileHandler):
            _logger.removeHandler(_handler)

import collections
import datetime
import json
import os

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from chamados.juizes import pergunta_viva
from chamados.models import PerguntaDisputa
from core.observ import engolidos
from escala.alimentacao import vinculo_do_dia_carregado
from escala.models import CelulaDia
from escala.servico_jornada import marcos_turno, turno_cruza_meia_noite
from ponto import janelas
from ponto.juiz_batida import periodos_do_dia
from ponto.management.commands import e6_oraculo as e6
from ponto.models import Batida
from ponto.precedencia import fatos_do_dia
from ponto.services.espelho import espelho_do_colab


# Cada linha: competência empresa colab_pk dias.
# Dias abreviados usam o mês indicado em cada grupo.
DADOS = """
2026-11 1 950 2026-11-11
2026-11 2 200 2026-10-29 2026-11-02
2026-11 2 218 2026-11-14
2026-11 2 220 2026-11-10
2026-11 2 252 2026-11-09 2026-11-10 2026-11-11 2026-11-13 2026-11-17
2026-11 2 296 2026-11-02
2026-11 2 334 2026-11-16
2026-11 2 382 2026-11-10
2026-11 2 516 2026-10-23
2026-11 2 727 2026-11-10
2026-11 2 841 2026-11-16
2026-11 2 874 2026-10-22
2026-11 2 884 2026-11-08
2026-11 2 922 2026-11-01 2026-11-04
2026-11 2 989 2026-10-23
2026-11 3 110 2026-11-12
2026-11 3 112 2026-10-21 2026-10-22
2026-11 3 113 2026-10-30 2026-11-01
2026-11 3 114 2026-11-08 2026-11-15
2026-11 3 130 2026-10-29
2026-11 3 909 2026-11-11
2026-11 3 996 2026-10-28 2026-11-10
2026-11 4 37 2026-11-14
2026-08 2 189 2026-07-22
2026-08 2 200 2026-07-22
2026-08 2 231 2026-08-06
2026-08 2 241 2026-08-20
2026-08 2 252 2026-07-21 2026-07-23 2026-07-24 2026-07-27 2026-07-28 2026-07-31 2026-08-03 2026-08-04 2026-08-05 2026-08-06 2026-08-10 2026-08-11 2026-08-12 2026-08-13 2026-08-14 2026-08-17 2026-08-18 2026-08-19
2026-08 2 282 2026-08-01
2026-08 2 334 2026-08-05
2026-08 2 435 2026-07-27
2026-08 2 441 2026-07-25 2026-08-08
2026-08 2 444 2026-08-04 2026-08-05
2026-08 2 451 2026-08-05
2026-08 2 457 2026-08-02 2026-08-04
2026-08 2 468 2026-07-23 2026-07-24 2026-07-25 2026-08-07
2026-08 2 516 2026-07-23 2026-08-02
2026-08 2 532 2026-07-22
2026-08 2 599 2026-08-15
2026-08 2 707 2026-07-26 2026-08-01
2026-08 2 736 2026-07-23
2026-08 2 820 2026-08-16 2026-08-18
2026-08 2 832 2026-07-25
2026-08 2 838 2026-07-21
2026-08 2 865 2026-07-30 2026-08-02
2026-08 3 110 2026-08-12 2026-08-17
2026-08 3 152 2026-08-06
2026-08 3 154 2026-07-28
2026-08 3 169 2026-08-02
2026-08 4 37 2026-08-10
2026-08 4 48 2026-07-26
2026-08 4 857 2026-07-25
"""

COMPETENCIAS = ("2026-11", "2026-08")
CLASSES = (
    "PERGUNTA_VIVA",
    "BATIDA_RETRATADA",
    "BATIDA_PLANTADA",
    "ORFA_NO_DIA",
    "AUSENCIA_NO_DIA",
    "FERIADO_TRABALHADO",
    "FOLGA_TRABALHADA",
    "CELULA_REGENERADA",
    "CELULA_EDITADA",
    "CRUZA_MEIA_NOITE",
    "OUTROS",
)


def universo():
    casos = []
    for linha in DADOS.splitlines():
        partes = linha.split()
        if not partes:
            continue
        comp, empresa, pk, *datas = partes
        for data in datas:
            casos.append(
                (comp, int(empresa), int(pk), datetime.date.fromisoformat(data))
            )
    if len(casos) != 90 or len(set(casos)) != 90:
        raise RuntimeError("universo_nao_e_90_unicos")
    cont = collections.Counter(c[0] for c in casos)
    if cont != {"2026-11": 33, "2026-08": 57}:
        raise RuntimeError("universo_por_competencia")
    return casos


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")

    especificacao = os.environ.get("TESTE_CPUSET", "4-7")
    permitidas = set()
    for parte in especificacao.split(","):
        limites = parte.strip().split("-")
        if len(limites) == 1:
            permitidas.add(int(limites[0]))
        elif len(limites) == 2:
            inicio, fim = map(int, limites)
            if inicio > fim:
                raise RuntimeError("cpuset_invalido")
            permitidas.update(range(inicio, fim + 1))
        else:
            raise RuntimeError("cpuset_invalido")
    afinidade = os.sched_getaffinity(0)
    if not afinidade or not afinidade.issubset(permitidas):
        raise RuntimeError("cpuset")

    with connection.cursor() as cursor:
        cursor.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY"
        )
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def divergencia_v1(registro, m_oraculo):
    # Mesmo filtro do anexo sonda_v1.py; None significa não comparável.
    if registro is None:
        return False, None, None, "sem_registro"
    if registro.get("isencao"):
        return False, None, None, "isencao"
    if registro.get("ausencia") is not None:
        return False, None, None, "ausencia"
    m_espelho, fonte = e6.minutos_do_espelho(registro)
    if m_espelho is None:
        return False, None, fonte, "sem_minutos"
    diff = m_oraculo - m_espelho
    if m_espelho == 0 and m_oraculo == 0:
        return False, diff, fonte, "ambos_zero"
    if e6.diferenca_e_a_janela_declarada(diff, registro):
        return False, diff, fonte, "janela_declarada"
    if abs(diff) <= 10:
        return False, diff, fonte, "ate_10_min"
    return True, diff, fonte, "diverge"


def eixo(diff):
    if diff is None:
        return "indisponivel", "indisponivel"
    sinal = "positivo" if diff > 0 else "negativo" if diff < 0 else "zero"
    magnitude = abs(diff)
    if magnitude <= 10:
        faixa = "ate_10"
    elif magnitude <= 30:
        faixa = "11-30"
    elif magnitude <= 120:
        faixa = "31-120"
    else:
        faixa = ">120"
    return sinal, faixa


def instante_local(valor):
    if valor is None:
        return None
    if timezone.is_aware(valor):
        valor = timezone.localtime(valor)
    return valor.isoformat()


def como_json(valor):
    return json.dumps(valor, ensure_ascii=True, separators=(",", ":"), default=str)


def medir_caso(colab, dia, registro, insumos, agora):
    chave = str(dia)
    m_oraculo = insumos.oraculo.minutos.get(chave, 0.0)
    passou, diff, fonte, motivo_filtro = divergencia_v1(registro, m_oraculo) [nao verificado]
    dono = insumos.dono(dia)

    cel = CelulaDia.objects.filter(colaborador=colab, data=dia).first()
    ec = vinculo_do_dia_carregado(colab.pk, dia)
    tipo = ec.tipo_escala if ec is not None else None
    marcos = marcos_turno(tipo, dia, ec=ec) if tipo is not None else []
    cruza = (
        turno_cruza_meia_noite(
            timezone.localtime(marcos[0][1]).time(),
            timezone.localtime(marcos[-1][1]).time(),
        )
        if marcos else False
    )

    fatos = fatos_do_dia(colab, dia)
    fatos_crus = fatos["fatos"]
    status_lavrado = cel.status_dia if cel is not None else None
    orfas = periodos_do_dia(cel, tipo).get("orfas", []) if cel else None

    inicio = timezone.make_aware(datetime.datetime.combine(dia, datetime.time.min))
    fim = timezone.make_aware(
        datetime.datetime.combine(dia + datetime.timedelta(days=1), datetime.time.min)
    )
    batidas = list(
        Batida.objects.filter(
            colaborador=colab, timestamp__gte=inicio, timestamp__lt=fim
        ).order_by("timestamp", "pk")
    )
    batidas_contexto = [
        {
            "hora_local": instante_local(b.timestamp),
            "origem": b.origem,
            "pergunta_origem": b.pergunta_origem_id,
            "retratada": b.retratada_em is not None,
        }
        for b in batidas
    ]

    perguntas = PerguntaDisputa.objects.filter(
        disputa__colaborador=colab, data_evento=dia
    ).select_related("disputa__chamado").order_by("pk")
    vivas = []
    for pergunta in perguntas:
        antes = engolidos()["total"]
        try:
            viva = pergunta_viva(pergunta, hoje=timezone.localtime(agora).date(), agora=agora)
        finally:
            depois = engolidos()["total"]
        if depois > antes:
            raise RuntimeError("pergunta_viva_engoliu_excecao")
        if viva:
            vivas.append(pergunta.pk)

    turnos = [] if registro is None else [
        {
            "entrada": instante_local(p.entrada),
            "saida": instante_local(p.saida),
        }
        for p in (registro.get("turnos") or [])
    ]
    dia_pago = None if registro is None else registro.get("dia_pago")
    so_motor = {
        "rubricas": None if registro is None else registro.get("rubricas"),
        "horas_falta": None if dia_pago is None else dia_pago.horas_falta,
    }
    m_espelho = (
        None if registro is None else e6.minutos_do_espelho(registro)[0]
    )

    flags = {
        "pergunta_viva": bool(vivas),
        "batida_retratada": any(b.retratada_em is not None for b in batidas),
        "batida_plantada": any(b.pergunta_origem_id is not None for b in batidas),
        "orfa_no_dia": bool(orfas),
        "ausencia_no_dia": bool(fatos_crus.get("ausencias")),
        "feriado_trabalhado": bool(fatos_crus.get("feriado")) and bool(turnos),
        "folga_trabalhada": status_lavrado == "folga_trabalhada",
        "celula_regenerada": bool(
            cel and (cel.regenerada_em is not None or cel.regeneracoes > 0)
        ),
        "celula_editada": bool(cel and cel.origem == "editada"),
        "cruza_meia_noite": cruza,
    }
    testes = ( [nao verificado]
        ("PERGUNTA_VIVA", flags["pergunta_viva"]),
        ("BATIDA_RETRATADA", flags["batida_retratada"]),
        ("BATIDA_PLANTADA", flags["batida_plantada"]),
        ("ORFA_NO_DIA", flags["orfa_no_dia"]),
        ("AUSENCIA_NO_DIA", flags["ausencia_no_dia"]),
        ("FERIADO_TRABALHADO", flags["feriado_trabalhado"]),
        ("FOLGA_TRABALHADA", flags["folga_trabalhada"]),
        ("CELULA_REGENERADA", flags["celula_regenerada"]),
        ("CELULA_EDITADA", flags["celula_editada"]),
        ("CRUZA_MEIA_NOITE", flags["cruza_meia_noite"]),
    )

    # Sem célula, órfãs e estado da célula não foram observados:
    # não converter ausência de evidência em classe OUTROS.
    classe = next((nome for nome, sim in testes if sim), None) [nao verificado]
    if classe is None:
        if cel is None:
            raise RuntimeError("sem_celula_predicados_indisponiveis")
        classe = "OUTROS"

    sinal, faixa = eixo(diff)
    contexto = {
        "oraculo_horas": insumos.oraculo.horas.get(chave, []),
        "periodos_motor": turnos,
        "so_motor": so_motor,
        "escala_tipo_ciclo": None if tipo is None else tipo.tipo_ciclo,
        "marcos_turno": [
            [papel, instante_local(instante)] for papel, instante in marcos
        ],
        "fatos": fatos_crus,
        "status_lavrado": status_lavrado,
        "orfas_ata": orfas,
        "batidas_dia_civil": batidas_contexto,
        "perguntas_vivas": vivas,
        "celula": None if cel is None else {
            "origem": cel.origem,
            "regenerada_em": instante_local(cel.regenerada_em),
            "regeneracoes": cel.regeneracoes,
        },
        "flags": flags,
    }
    return {
        "dono": dono,
        "filtro": passou, [nao verificado]
        "motivo_filtro": motivo_filtro,
        "oraculo_min": m_oraculo,
        "espelho_min": m_espelho,
        "fonte_espelho": fonte,
        "diff": diff,
        "sinal": sinal,
        "faixa": faixa,
        "classe": classe,
        "contexto": contexto,
    }


def main():
    erros = 0
    tentados = collections.Counter()
    classificados = collections.Counter()
    exemplos = collections.defaultdict(list)
    reconf = collections.Counter()
    etapa = "UNIVERSO"

    try:
        casos = universo()
        with transaction.atomic():
            etapa = "GUARDA"
            guarda_sombra()
            agora = timezone.now()
            hoje = timezone.localtime(agora).date()
            cache = {}

            for comp, empresa_id, pk, dia in casos:
                tentados[comp] += 1
                etapa = "CASO"
                try:
                    chave_cache = (comp, empresa_id, pk)
                    if chave_cache not in cache:
                        empresa = Empresa.objects.get(pk=empresa_id)
                        colab = Colaborador.objects.get(
                            pk=pk, empresa_id=empresa_id
                        )
                        ano, mes = map(int, comp.split("-"))
                        ini, fim = janelas.janela_fechamento(mes, ano, empresa)
                        espelho = espelho_do_colab(
                            colab, hoje, ini, fim, agora=agora
                        )
                        registros = {
                            str(item.get("data"))[:10]: item
                            for item in (espelho.get("dias") or [])
                        }
                        insumos = e6.InsumosDoDono(
                            colab, ini, fim, hoje, espelho=espelho, agora=agora
                        )
                        cache[chave_cache] = (colab, ini, fim, registros, insumos)

                    colab, ini, fim, registros, insumos = cache[chave_cache]
                    if not ini <= dia <= fim:
                        raise RuntimeError("dia_fora_da_janela")
                    registro = registros.get(str(dia))
                    resultado = medir_caso(colab, dia, registro, insumos, agora)

                    reconf[(comp, "dono_estrutura" if resultado["dono"] == "ESTRUTURA"
                            else "dono_mudou")] += 1
                    reconf[(comp, "filtro_diverge" if resultado["filtro"]
                            else "filtro_nao_diverge")] += 1
                    classe = resultado["classe"]
                    classificados[(comp, classe)] += 1
                    if len(exemplos[(comp, classe)]) < 2:
                        exemplos[(comp, classe)].append("%s/%s" % (pk, dia))

                    print(
                        "CASO %s colab_pk=%s empresa=%s dia=%s "
                        "| dono=%s filtro_v1=%s motivo_filtro=%s "
                        "| minutos_oraculo=%s minutos_espelho=%s fonte=%s "
                        "diff_oraculo_menos_espelho=%s sinal=%s faixa=%s "
                        "| contexto=%s | classe=%s"
                        % (
                            comp, pk, empresa_id, dia,
                            resultado["dono"], resultado["filtro"],
                            resultado["motivo_filtro"],
                            resultado["oraculo_min"], resultado["espelho_min"],
                            resultado["fonte_espelho"], resultado["diff"],
                            resultado["sinal"], resultado["faixa"],
                            como_json(resultado["contexto"]), classe,
                        )
                    )
                except Exception as exc:
                    erros = 1
                    reconf[(comp, "erro")] += 1
                    # Não imprimir mensagem de exceção: pode conter dados pessoais.
                    print(
                        "CASO %s colab_pk=%s empresa=%s dia=%s "
                        "| ERRO tipo=%s | classe=NAO_CLASSIFICADO"
                        % (comp, pk, empresa_id, dia, type(exc).__name__)
                    )

            for comp in COMPETENCIAS:
                for classe in CLASSES:
                    print(
                        "RESUMO competencia=%s classe=%s contagem=%d exemplos=%s"
                        % (
                            comp, classe, classificados[(comp, classe)],
                            ",".join(exemplos[(comp, classe)]) or "-",
                        )
                    )
                print(
                    "RECONFIRMACAO competencia=%s dono_estrutura=%d "
                    "dono_mudou=%d filtro_diverge=%d filtro_nao_diverge=%d "
                    "erro=%d"
                    % (
                        comp, reconf[(comp, "dono_estrutura")],
                        reconf[(comp, "dono_mudou")],
                        reconf[(comp, "filtro_diverge")],
                        reconf[(comp, "filtro_nao_diverge")],
                        reconf[(comp, "erro")],
                    )
                )

            total = sum(tentados.values())
            classificados_total = sum(classificados.values())
            confere = (
                total == 90
                and tentados["2026-11"] == 33
                and tentados["2026-08"] == 57
                and classificados_total + sum(
                    reconf[(c, "erro")] for c in COMPETENCIAS
                ) == 90
            )
            print(
                "CONFERENCIA tentados=%d novembro=%d agosto=%d "
                "classificados=%d erros_caso=%d 90=33+57=%s"
                % (
                    total, tentados["2026-11"], tentados["2026-08"],
                    classificados_total,
                    sum(reconf[(c, "erro")] for c in COMPETENCIAS),
                    "s" if confere else "n",
                )
            )
            if not confere:
                erros = 1

    except Exception as exc:
        erros = 1
        print("ERRO etapa=%s tipo=%s" % (etapa, type(exc).__name__))

    print("ERROS=%d" % erros)
    print("FIM_SONDA_EV_V2")
    return erros


sys.exit(main())
```

Quanto à cura, a sessão pode verificar se `CRUZA_MEIA_NOITE` se relaciona a **ESCALA-NAO-DESCREVE**, **MOTOR-LE-O-TURNO** ou **DIA-DA-BATIDA-JUIZ-UNICO**; se as classes de batida e órfã se relacionam a *

[truncado em 20000 de 20235 caracteres]

## 19/11 09:09 · sessao 30737462c82c · E4C-F5-CONSTROI · sonda · origem=Code

A sonda usa a leitura real e o juiz da cópia ([models.py:4295](app/chamados/models.py:4295), [juizes.py:866](app/chamados/juizes.py:866)). Os resultados na sombra são [não verificado].

```python
import collections
import os
import sys

from django.conf import settings
from django.db import connection, transaction

from chamados.juizes import pergunta_viva
from chamados.models import DecisaoPergunta, PerguntaDisputa


CLASSES = (
    "LEITURA_VAZIA_PROJECAO_TEXTO",
    "LEITURA_TEXTO_PROJECAO_VAZIA",
    "AMBOS_TEXTO_DIFERENTE",
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


def main():
    etapa = "GUARDA_SOMBRA"
    erros = 0
    universo = 0
    iguais = 0
    casos_com_erro = 0
    contagem = collections.Counter()
    vivas = collections.Counter()
    exemplos = collections.defaultdict(list)

    try:
        with transaction.atomic():
            guarda_sombra()

            etapa = "PERGUNTAS"
            perguntas = (
                PerguntaDisputa.objects.all()
                .order_by("pk")
                .prefetch_related("decisoes")
                .iterator(chunk_size=500)
            )

            for p in perguntas:
                universo += 1
                try:
                    leitura = DecisaoPergunta.leitura(p).resposta_colab
                    projecao = p.resposta_colab or ""

                    if leitura == projecao:
                        iguais += 1
                        continue

                    if not leitura and projecao:
                        classe = CLASSES[0]
                    elif leitura and not projecao:
                        classe = CLASSES[1]
                    elif leitura and projecao:
                        classe = CLASSES[2]
                    else:
                        raise RuntimeError("diferenca_nao_classificada")

                    viva = pergunta_viva(p)
                    contagem[classe] += 1
                    if viva:
                        vivas[classe] += 1
                    if len(exemplos[classe]) < 3:
                        exemplos[classe].append(p.pk)

                except Exception:
                    casos_com_erro += 1
                    erros = 1
                    print("ERRO_CASO pergunta_pk=%s" % p.pk)

            etapa = "SAIDA"
            soma = iguais + casos_com_erro + sum(
                contagem[classe] for classe in CLASSES
            )
            if soma != universo:
                erros = 1

            linhas = ["UNIVERSO perguntas_lidas=%d" % universo]
            linhas.append("IGUAIS contagem=%d" % iguais)
            linhas.append("CASOS_COM_ERRO contagem=%d" % casos_com_erro)

            for classe in CLASSES:
                linhas.append(
                    "CLASSE classe=%s contagem=%d pergunta_viva=%d"
                    % (classe, contagem[classe], vivas[classe])
                )
                for numero in range(1, 4):
                    if numero <= len(exemplos[classe]):
                        linhas.append(
                            "EXEMPLO classe=%s numero=%d pergunta_pk=%s"
                            % (classe, numero, exemplos[classe][numero - 1])
                        )
                    else:
                        linhas.append(
                            "EXEMPLO classe=%s numero=%d indisponivel"
                            % (classe, numero)
                        )

            linhas.append(
                "CONFERENCIA soma=%d universo=%d soma==universo=%s"
                % (soma, universo, "s" if soma == universo else "n")
            )

        for linha in linhas:
            print(linha)

    except Exception:
        print("ERRO " + etapa)
        erros = 1

    print("ERROS=%d" % erros)
    print("FIM_SONDA_F5_TEXTO")
    return erros


sys.exit(main())
```

Perguntas de lei: N.

## 19/11 09:09 · sessao cd1b747b2dd1 · ESPELHO-VERDADE · sonda · origem=Code

**Não consegui completar a sonda B com segurança nesta volta.** O bloqueio do meu trabalho é montar e conferir a extração dos pares exatos e a classificação de local sem apresentar uma associação ambígua como fato. Não vou entregar um script parcial como se satisfizesse a especificação. A execução na sombra, as classes e suas contagens permanecem **[não verificado]**.

**A) Leitura do código**

A V1 compara, em minutos, `insumos.oraculo.minutos[dia]` com `minutos_do_espelho(registro)`, usando a diferença com sinal **oráculo − espelho**. O lado espelho vem de `minutos_realizados`, quando presente, ou de `pago_h × 60`; o filtro exclui, entre outros, dias sem registro, isentos ou com ausência, a diferença explicada pela janela declarada e diferenças absolutas de até 10 minutos. [e6_oraculo.py:263](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:263) [e6_oraculo.py:328](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:328) [e6_oraculo.py:336](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:336) [e6_oraculo.py:365](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:365)

`horas_do_dia`, passado ao juiz do dono, é a lista de batidas do oráculo, usada na verificação de paridade; não é o total do motor. `InsumosDoDono.dono(dia)` chama o juiz com os demais insumos, e o retorno residual `ESTRUTURA` não nomeia uma causa. A chamada ao dono e a reaplicação do filtro são, portanto, verificações separadas. [e6_oraculo.py:106](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:106) [e6_oraculo.py:110](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:110) [e6_oraculo.py:182](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:182)

Para **onde** comparar, o núcleo soma os pares fechados, mas `oraculo.horas` guarda horários formatados em `HH:MM`, concatenados por dia: essa saída, sozinha, não preserva as datas, os segundos e as fronteiras entre turnos necessárias para reconstruir com segurança todos os pares. Os períodos do resultado do motor expõem entrada, saída, pontas efetivas e intervalos. Eles são contexto geométrico relevante; o campo efetivamente escolhido como lado espelho deve continuar identificado separadamente. [nucleo.py:236](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/calculador/nucleo.py:236) [nucleo.py:245](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/calculador/nucleo.py:245) [motor_calculo_v2.py:208](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/motor_calculo_v2.py:208) [motor_calculo_v2.py:239](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/motor_calculo_v2.py:239) [espelho.py:779](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/services/espelho.py:779)

Para as rubricas, a decisão da sessão permite imprimir extra, noturno e falta como **`so_motor:`**, sem afirmar divergência entre duas testemunhas. `rubricas_do_dia` lê as rubricas lavradas; `horas_falta` está no `DiaPago`. A pausa do oráculo é o intervalo entre pares fechados, não uma segunda medida automática de intrajornada indenizada. [dia_pago.py:901](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/services/dia_pago.py:901) [models.py:325](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/models.py:325) [nucleo.py:248](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/calculador/nucleo.py:248)

As fontes para particularidades são `vinculo_do_dia_carregado`, `marcos_turno` e `turno_cruza_meia_noite` para escala e cruzamento; `fatos_do_dia` para feriado, ausências e folga lançada; `status_da_lavra` para o status de folga trabalhada; `periodos_do_dia` para órfãs na ata; os campos da batida para origem e retratação; e os campos da célula para edição e regeneração. `pergunta_origem` nula não prova ausência de plantio anterior. [alimentacao.py:289](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/alimentacao.py:289) [servico_jornada.py:69](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/servico_jornada.py:69) [servico_jornada.py:104](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/servico_jornada.py:104) [precedencia.py:242](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/precedencia.py:242) [status_dia.py:143](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/status_dia.py:143) [juiz_batida.py:149](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/juiz_batida.py:149) [models.py:30](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/models.py:30) [models.py:1690](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/models.py:1690) [models.py:1718](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/models.py:1718)

Para pergunta viva, capturar apenas exceções externas é insuficiente: há falhas internas registradas por `registrar_engolido`, inclusive um ramo que devolve `False`. O contador `engolidos()` oferece uma testemunha em memória para detectar incremento durante a chamada; a sonda ainda precisa impedir que os handlers de logging escrevam arquivos. [juizes.py:1479](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/chamados/juizes.py:1479) [juizes.py:1488](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/chamados/juizes.py:1488) [observ.py:60](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/core/observ.py:60) [observ.py:73](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/core/observ.py:73)

**B) Pendente.** O universo informado é 90, sendo 33 de 2026-11 e 57 de 2026-08; a reconfirmação desses casos na sombra e `ERROS=0` estão **[não verificado]**.

**C) Perguntas para a sessão**, sem atribuição de cura como fato:

- Para diferenças localizadas em **ENTRADA, SAIDA, INTERVALO, PAR_SO_ORACULO ou PAR_SO_MOTOR**, a obra seria **DIA-DA-BATIDA-JUIZ-UNICO**, a **“razao da sobra”** ou **sem obra**, conforme o caso? **[não verificado]**
- Para **CRUZA_MEIA_NOITE** e particularidades de escala ou turno, caberia **ESCALA-NAO-DESCREVE**, **MOTOR-LE-O-TURNO** ou **DIA-DA-BATIDA-JUIZ-UNICO**? **[não verificado]**
- Para **BATIDA_PLANTADA, BATIDA_RETRATADA** e **ORFA_NO_DIA**, caberia **DIA-DA-BATIDA-JUIZ-UNICO** ou **sem obra**? **[não verificado]**
- Para **PERGUNTA_VIVA**, caberia **DST-L1, DST-L2, E4C-F5** ou **sem obra**? **[não verificado]**
- Para **FERIADO_TRABALHADO, FOLGA_TRABALHADA, AUSENCIA_NO_DIA, CELULA_REGENERADA, CELULA_EDITADA** e **OUTROS**, caberia a **“razao da sobra”** ou **sem obra**? **[não verificado]**

Perguntas de lei: **0**.

## 19/11 09:06 · sessao 74532414e01d · E4C-F5-CONSTROI · construtor · origem=Code

**lei:** `DecisaoPergunta.leitura(p).resposta_colab` não é garantidamente o texto corrente de `p.resposta_colab`. A leitura reconstrói o texto a partir dos atos e só acrescenta a projeção como passivo quando não encontra ato correspondente pelo instante; ela não compara os textos. ([chamados/models.py:4260](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/models.py:4260), [chamados/models.py:4308](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/models.py:4308))

No caso `RESPOSTA 'a' → REABERTURA → RESPOSTA 'b'`, com a projeção coerente, a leitura retorna `'b'`; com apenas `RESPOSTA 'a' → REABERTURA`, retorna texto vazio, e o fio espera texto. A recusa também limpa a resposta reconstruída. ([chamados/models.py:4277](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/models.py:4277), [chamados/models.py:4317](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/models.py:4317), [chamados/services/fio.py:189](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/services/fio.py:189))

Mas, se após essa sequência o ato de resposta em T3 guarda `'b'` e a pergunta corrente guarda `respondida_em=T3, resposta_colab='c'`, a leitura retorna **`'b'`**, enquanto o texto corrente é **`'c'`**: o ato já corresponde pelo instante e impede a inclusão do texto corrente como passivo. ([chamados/models.py:4260](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/models.py:4260), [chamados/models.py:4309](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/models.py:4309))

Parei conforme a condição do item 2: não fiz a cura, alterações ou testes. A ocorrência desse caso em produção é **[não verificado]**. [nao verificado]

## 19/11 08:55 · sessao 2fe06408ebda · E4C-F5-CONSTROI · verificador · origem=Code

**Parecer: BLOQUEIA nos seis itens.** O bloqueio principal é que a tag passa a decidir se existe texto pela testemunha, mas o juiz chamado em seguida classifica o texto cru da projeção; assim, uma mesma exibição pode combinar respostas diferentes (`diff_v1.patch`, hunk de `chamados_tags.py`; `app/chamados/juizes.py:1938`, `app/chamados/juizes.py:2041`). O bloqueio de custo é um caminho real de lista sem `prefetch('decisoes')` (`app/chamados/services/acoes_chamado.py:350`; `app/templates/chamados/detalhe_local.html:309`; `app/chamados/models.py:4308`).

A cópia disponível não contém `.git`: o hash informado para ela não pôde ser confirmado por Git [não verificado]. As referências ao diff e ao teste novo abaixo são ao **anexo fornecido**, pois esses arquivos não estão aplicados na cópia [não verificado]. Não executei os testes; os resultados RED/GREEN e mutantes são os do resumo fornecido [não verificado]. [nao verificado]

1. **BLOQUEIA — testemunha e leituras cruas.**

   - No diff, `espera_o_texto_do_colaborador` consulta `DecisaoPergunta.leitura` e entrega seu resultado a `respondida_sem_texto` (`diff_v1.patch`, hunk de `fio.py:185`). O juiz acessa somente `respondida_em` e `resposta_colab`, ambos presentes em `LeituraDoAto`: não há `AttributeError` por campo faltante nem default silencioso nessa chamada (`app/chamados/juizes.py:1531`; `app/chamados/models.py:19`).
   - Em `historico`, manter `p.materializada_em` cru está correto segundo o brief fornecido, que deixa materialização na projeção; esse campo não existe em `LeituraDoAto` (`diff_v1.patch`, hunk de `historico.py:94`; `app/chamados/models.py:19`; brief fornecido [não verificado]). O histórico usa a leitura para resposta e validação, inclusive no rótulo da materialização (`diff_v1.patch`, hunk de `historico.py:102`).
   - A tag **não satisfaz a afirmação “nada mais vem da projeção crua”**: depois do teste de texto pela leitura, chama `classificar_resposta_validacao(perg)`, que lê `perg.resposta_colab` diretamente para escolha e, para hora, via `delta_da_resposta` (`diff_v1.patch`, hunk de `chamados_tags.py:163`; `app/chamados/juizes.py:2034`; `app/chamados/juizes.py:1928`). O teste da tag substitui o classificador por mock, portanto não demonstra a coerência do resultado real (`diff_v1.patch`, `test_e4c_f5_volta1_leem_o_ato.py:58`).
   - Há ainda campos crus nos próprios cartões: validação e instante em `_pergunta_card.html`, e texto/instante no cartão do colaborador (`app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:32`; `app/templates/chamados/partials/_card_atendimento.html:143`). Esses templates não são alterados pelo diff fornecido [não verificado].

2. **BLOQUEIA — semântica e autoria dos campos.**

   Os campos são da mesma *espécie*: `respondida_em` e `validada_em` são instantes `DateTimeField`, não dias; `resposta_colab` é texto (`app/chamados/models.py:2634`). A leitura preserva os instantes dos atos, sem conversão explícita de fuso, e ordena por instante; comportamento de apresentação de fuso em cada tela [não verificado] (`app/chamados/models.py:4308`; `app/chamados/models.py:4317`).

   Eles **não são garantidamente os mesmos valores da projeção**: a leitura reconstrói a sequência, limpa resposta e validação em recusa/reabertura e acrescenta projeção sem linha correspondente como passivo (`app/chamados/models.py:4238`; `app/chamados/models.py:4277`; `app/chamados/models.py:4295`; `app/chamados/models.py:4317`). Sem ato e com carimbo de resposta, o passivo recupera o texto cru; texto cru sem carimbo não cria resposta sintética, logo a leitura pode devolvê-lo vazio (`app/chamados/models.py:4251`; `app/chamados/models.py:4260`). O teste fornecido também constrói deliberadamente divergências entre ato e projeção (`diff_v1.patch`, `test_e4c_f5_volta1_leem_o_ato.py:40`).

   - **`respondida_em` e `resposta_colab`:** a porta humana grava ambos e registra `RESPOSTA` com o mesmo instante e texto; resposta automática também os grava, preservando validação; reabertura os zera (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1527`; `app/chamados/services/disputa_emissao.py:1662`; `app/chamados/services/disputa_emissao.py:1617`). A leitura os devolve ao fio, à tag e ao histórico no diff; o classificador da tag também lê o texto cru (`diff_v1.patch`, hunks de `fio.py`, `chamados_tags.py` e `historico.py`; `app/chamados/juizes.py:2041`).
   - **`validada_em`:** `_carimbar_validacao` grava o carimbo e registra `VALIDACAO`; resposta humana nova e devolução podem limpá-lo (`app/chamados/services/validacao.py:161`; `app/chamados/services/disputa_emissao.py:1484`; `app/chamados/services/validacao.py:273`). A leitura o entrega ao histórico no diff (`diff_v1.patch`, hunk de `historico.py:102`).
   - **`materializada_em`:** `_escrever_carimbo` atribui o campo; a porta de reabertura em lote também o limpa (`app/chamados/models.py:3293`; `app/chamados/services/disputa_emissao.py:1608`). O histórico proposto continua lendo-o da projeção (`diff_v1.patch`, hunk de `historico.py:102`).
   - **Campos da sequência:** `registrar` grava as linhas; `leitura` consome `ato`, `ato_em`, `pk`, `ato_texto` e `resposta_mantem_validacao`, além do passivo da projeção (`app/chamados/models.py:4329`; `app/chamados/models.py:4308`; `app/chamados/models.py:4280`).

   O corte fornecido de F8 diz que leitores e silêncio seguem usando o texto corrente da pergunta e que o texto do último ato é histórico (`app/docs/CORTES.md:550`). O diff usa texto da testemunha no histórico e no primeiro teste da tag, enquanto o classificador usa texto corrente: a compatibilidade dessa combinação com o corte, diante de divergência, fica **não demonstrada** (`diff_v1.patch`, hunks de `historico.py` e `chamados_tags.py`; `app/chamados/juizes.py:2041`).

3. **BLOQUEIA — custo e chamadores.**

   Para pergunta persistida, `leitura` executa `p.decisoes.all()`; sem prefetch, cada chamada avalia a relação, e chamadas repetidas na mesma instância sem cache de prefetch podem repetir a consulta (`app/chamados/models.py:4304`; `app/chamados/models.py:4308`).

   - **Fio:** chamada direta em `montar_chamados_fio` e indireta por `aguarda_o_colaborador`; as perguntas desse caminho já chegam com `Prefetch('decisoes')` (`app/chamados/services/fio.py:258`; `app/chamados/services/fio.py:360`; `app/chamados/services/fio.py:387`).
   - **Cobrança:** `_calados` chama diretamente; o diff acrescenta o prefetch ao queryset (`app/chamados/services/cobranca.py:236`; `diff_v1.patch`, hunk de `cobranca.py:235`). Caso real de custo coberto por teste [não verificado].
   - **Relatório:** `classificar` chama `aguarda_o_colaborador`, que chama a função por pergunta; o diff faz prefetch nas perguntas dentro de `classificar` e também em `coletar` (`app/chamados/relatorios_disputa.py:11`; `app/chamados/relatorios_disputa.py:23`; `app/chamados/services/fio.py:164`; `diff_v1.patch`, hunks de `relatorios_disputa.py`). Caso real de custo coberto por teste [não verificado].
   - **Tag no modal:** recebe as perguntas de `montar_chamados_fio`, com prefetch (`app/templates/chamados/partials/modal_fio.html:274`; `app/chamados/views.py:889`; `app/chamados/services/fio.py:258`).
   - **Tag na lista de detalhe:** `montar_contexto_detalhe` carrega as perguntas sem prefetch e as passa ao template, que inclui o cartão por pergunta; a nova chamada da tag acrescenta consulta por pergunta que alcançar esse trecho. Já havia outros custos por pergunta nesse caminho; a chamada acrescentada é o N+1 desta alteração (`app/chamados/services/acoes_chamado.py:350`; `app/templates/chamados/detalhe_local.html:307`; `app/templates/chamados/partials/_pergunta_card.html:42`; `app/chamados/models.py:4308`).
   - **Tag no fragmento HTMX:** o queryset busca uma pergunta sem prefetch; há consulta adicional, mas esse caminho renderiza um único cartão, não uma lista (`app/chamados/views.py:397`; `app/chamados/views.py:404`).
   - **Histórico:** o diff acrescenta prefetch ao queryset próprio (`diff_v1.patch`, hunk de `historico.py:94`).

   `test_lote_1_e_n` cria ele próprio o queryset com prefetch e chama as funções diretamente; não testa o carregamento de nenhum desses chamadores reais. O resumo fornecido informa que passa tanto no RED quanto no GREEN [não verificado] (`diff_v1.patch`, `test_e4c_f5_volta1_leem_o_ato.py:100`).

4. **BLOQUEIA — classes de reprovação.**

   Há **fonte de resposta dividida** na tag: a testemunha abre o caminho e o juiz classifica a projeção crua (`diff_v1.patch`, hunk de `chamados_tags.py:163`; `app/chamados/juizes.py:2034`). Há também **teste de custo sem mordida no chamador real** (`diff_v1.patch`, `test_e4c_f5_volta1_leem_o_ato.py:100`; `app/chamados/services/acoes_chamado.py:350`).

   O diff fornecido não introduz definição de juiz, escritor ou leitura de relógio [não verificado]. O predicado de espera continua delegando a `respondida_sem_texto`; o juiz registrado de pergunta respondida é `pergunta_respondida` (`diff_v1.patch`, hunk de `fio.py:185`; `app/chamados/juizes.py:1531`; `app/core/juizes.py:306`). Não identifiquei no diff um novo campo com dois significados [não verificado]; a divergência demonstrada é entre **duas fontes para o texto**, não uma prova de que um campo novo tenha dois significados (`app/chamados/juizes.py:2041`; `app/chamados/models.py:4318`).

5. **BLOQUEIA — selo AST.**

   O selo fornecido inspeciona apenas as funções `espera_o_texto_do_colaborador` e `leitura_da_resposta`; não varre `historico`, `cobranca` nem `relatorios_disputa` (`diff_v1.patch`, `test_e4c_f5_volta1_leem_o_ato.py:113`). Também não percorre o corpo do classificador chamado pela tag (`diff_v1.patch`, `test_e4c_f5_volta1_leem_o_ato.py:118`; `app/chamados/juizes.py:2034`).

   Ele detecta atributos diretos do argumento e certas strings literais em chamadas; portanto `getattr(p, campo)` com nome variável, `p.__dict__` e `values()` com campo variável escapam dessa inspeção. Literais diretamente presentes em argumentos ou coleções verificadas podem ser detectados (`diff_v1.patch`, `test_e4c_f5_volta1_leem_o_ato.py:120`).

6. **BLOQUEIA — LEI-AKITA.**

   O brief fornecido declara testemunha única, zero juízes novos, um juiz por pergunta, prefetch nos caminhos em lista e selo AST com cobertura de formas indiretas [não verificado]. O diff usa a testemunha nos trechos migrados e não apresenta definição de juiz novo [não verificado], mas a tag combina testemunha e projeção no resultado, a lista de detalhe fica sem prefetch e o selo não cobre todas as formas declaradas (`diff_v1.patch`, hunks de `chamados_tags.py` e teste; `app/chamados/juizes.py:2041`; `app/chamados/services/acoes_chamado.py:350`).

   Assim, **a ausência de juiz novo está sustentada pelo diff fornecido, mas a realização integral do padrão declarado e das demais cláusulas não está** [não verificado].

**BLOQUEIA: 6 — itens 1, 2, 3, 4, 5 e 6.**

## 19/11 08:54 · sessao cfc0c8b9842d · ESPELHO-VERDADE · sonda · origem=Code

Não consigo entregar a sonda B como especificada sem inventar um segundo valor para as rubricas: a comparação que gerou os 90 casos fornece dois lados para **minutos de trabalho**, mas o veredito E6 não fornece valores equivalentes de extra, noturno ou falta. [e6_oraculo.py:328](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:328) [nucleo.py:28](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/calculador/nucleo.py:28)

**A) Leitura do código**

- `horas_do_dia` no argumento de `dono_da_divergencia` é a **lista de batidas** atribuídas ao dia, usada para verificar paridade; não é o total de horas do motor. `InsumosDoDono` obtém essas batidas apuráveis e chama `minutos_do_oraculo`. [e6_oraculo.py:98](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:98) [e6_oraculo.py:149](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:149)
- A divergência é `oraculo.minutos[dia] − minutos_do_espelho(registro)`. O primeiro soma pares fechados de batidas; o segundo usa `minutos_realizados` quando existe e, senão, `pago_h × 60`. A comparação descarta, entre outros casos, a diferença explicada pela janela declarada e diferenças de até 10 minutos. [nucleo.py:42](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/calculador/nucleo.py:42) [e6_oraculo.py:263](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:263) [e6_oraculo.py:340](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:340)
- `InsumosDoDono.dono(dia)` chama o juiz real com cadastro, batidas, fase e cadastro-versus-realidade. `ESTRUTURA` na linha 110 é o retorno residual, não uma causa diagnosticada. A reconfirmação do dono, isoladamente, também não prova que o dia continue passando pelo filtro de divergência da V1. [e6_oraculo.py:98](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:98) [e6_oraculo.py:182](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:182) [e6_oraculo.py:340](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/management/commands/e6_oraculo.py:340)
- Para medidas adicionais, `linhas_do_periodo` lê `DiaPago` por versão e `rubricas_do_dia` apresenta rubricas lavradas; `horas_falta` existe no modelo. O E6, porém, não fornece a contraparte dessas rubricas. `Veredito.pausas` mede pausas entre pares e não deve ser apresentado como equivalente automático à intrajornada indenizada. [dia_pago.py:833](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/services/dia_pago.py:833) [dia_pago.py:901](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/services/dia_pago.py:901) [models.py:336](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/models.py:336) [nucleo.py:37](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/calculador/nucleo.py:37)

Para as particularidades, as autoridades e campos identificados são:

- **Escala e cruzamento previsto:** `vinculo_do_dia_carregado`, `marcos_turno` e `turno_cruza_meia_noite`. [alimentacao.py:289](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/alimentacao.py:289) [servico_jornada.py:69](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/servico_jornada.py:69) [servico_jornada.py:104](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/servico_jornada.py:104)
- **Feriado, ausências e folga lançada:** `fatos_do_dia` devolve os fatos e o dia previsto; a classificação específica de folga trabalhada tem regra em `status_da_lavra`. [precedencia.py:242](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/precedencia.py:242) [precedencia.py:402](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/precedencia.py:402) [status_dia.py:143](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/status_dia.py:143)
- **Batidas e órfãs:** `pergunta_origem` identifica plantio vinculado, mas `NULL` também pode ser plantio anterior; `origem` e `retratada_em` são campos distintos. `periodos_do_dia` lê `orfas` e intervalo da ata. [models.py:30](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/models.py:30) [models.py:64](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/models.py:64) [juiz_batida.py:149](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/juiz_batida.py:149)
- **Pergunta viva:** `pergunta_viva` é o juiz. Em uma falha interna ele pode registrar o erro e devolver `False`; uma sonda que apenas capture exceções externas pode, portanto, tratar uma falha como resposta negativa. O registro usa logging, que precisa ser compatível com a proibição de criar ou alterar arquivos. [juizes.py:866](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/chamados/juizes.py:866) [juizes.py:1488](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/chamados/juizes.py:1488) [observ.py:73](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/core/observ.py:73)
- **Célula:** `origem`, `escala_geradora`, `dna`, `regenerada_em` e `regeneracoes` são campos de `CelulaDia`. [models.py:1681](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/models.py:1681) [models.py:1718](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/models.py:1718)

**B) Sonda:** não entreguei um script que afirme satisfazer a especificação. Faltam a definição da segunda testemunha para cada rubrica e a verificação de que ela existe para cada dia; a execução, as classes, suas contagens e `ERROS=0` permanecem **[não verificado]**. A cópia também não contém `.git`, portanto o HEAD informado não pôde ser confirmado por Git **[não verificado]**.

**C) Perguntas para a sessão, sem atribuir cura como fato:**

- Casos de escala ou turno poderiam corresponder a **ESCALA-NAO-DESCREVE** ou **MOTOR-LE-O-TURNO**? [não verificado]
- Cruzamento de meia-noite ou órfã poderia corresponder a **DIA-DA-BATIDA-JUIZ-UNICO**? [não verificado]
- Pergunta viva poderia corresponder a **DST-L1**, **DST-L2** ou **E4C-F5**? [não verificado]
- As demais classes corresponderiam à **“razao da sobra”** ou ficariam **sem obra**? [não verificado]

Para completar B, qual par de fontes deve responder por extra, noturno, falta e intervalo: as versões lavradas `motor` e `oraculo` de `DiaPago`, quando ambas existirem, ou outra comparação? A existência dessas duas lavraturas nos 90 casos está **[não verificado]**. [calculador/__init__.py:7](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/calculador/__init__.py:7) [dia_pago.py:833](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/services/dia_pago.py:833)

Perguntas de lei: **0**.

## 19/11 08:53 · sessao 236c471f76c4 · - · - · origem=Ronald (tmux)

- O vínculo do dia pode ser obtido por `vinculo_do_dia_carregado`; ele alimenta a juíza que respeita a escala geradora da célula e sua vigência ([app/escala/alimentacao.py:228](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/alimentacao.py:228), [app/escala/alimentacao.py:289](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/alimentacao.py:289)). O ciclo fica em `tipo_escala.tipo_ciclo` ([app/escala/models.py:117](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/models.py:117)).
- `marcos_turno` obtém os marcos previstos; `turno_cruza_meia_noite` decide a virada a partir de início e fim ([app/escala/servico_jornada.py:69](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/servico_jornada.py:69), [app/escala/servico_jornada.py:104](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/servico_jornada.py:104)).
- `fatos_do_dia` é de leitura e expõe previsão, ausências, feriado e folga lançada ([app/ponto/precedencia.py:242](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/precedencia.py:242), [app/ponto/precedencia.py:402](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/precedencia.py:402)). `status_da_lavra` tem a decisão de folga trabalhada, dados seus insumos ([app/ponto/status_dia.py:113](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/status_dia.py:113), [app/ponto/status_dia.py:143](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/status_dia.py:143)).
- `periodos_do_dia` devolve intervalo, órfãs e desalinhamento a partir da célula; declara-se de leitura ([app/ponto/juiz_batida.py:149](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/juiz_batida.py:149)). A célula registra origem, escala geradora e regenerações ([app/escala/models.py:1681](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/models.py:1681), [app/escala/models.py:1718](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/escala/models.py:1718)).
- `Batida.pergunta_origem` identifica plantio vinculado; valor nulo também pode ser plantio anterior ao registro desse vínculo, portanto não prova ausência de plantio ([app/ponto/models.py:30](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/models.py:30)). `retratada_em` marca a retratação ([app/ponto/models.py:64](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/ponto/models.py:64)).
- `pergunta_viva` é o juiz da pergunta ([app/chamados/juizes.py:866](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/chamados/juizes.py:866)). Sua apuração captura uma exceção e retorna `False`, registrando-a em log ([app/chamados/juizes.py:1492](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/chamados/juizes.py:1492), [app/core/observ.py:73](/home/ronald/codex_ws/3308f759942527fa084b6137304860d24c6f414d/app/core/observ.py:73)); o envelope precisa detectar esse erro e garantir que o logging não escreva arquivo. [não verificado]

## 19/11 08:53 · sessao 2653f21ede6c · - · - · origem=Ronald (tmux)

A comparação E6 e as limitações das rubricas foram enviadas ao agente principal com referências de arquivo e linha. [não verificado]

## 19/11 08:48 · sessao 72c9b0ff375a · ESPURIA-TELA-C11-1911-0845 · censo · origem=Code

O caso pedido é reproduzível **pela leitura do código**: com `dia.folga=True` e o veredito visual `feriado`, o espelho pode mostrar o chip **Folga** junto do ícone **Feriado**. A ocorrência em produção e a renderização efetiva não foram verificadas. [espelho.html:171](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:171), [espelho.py:353](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:353), [precedencia.py:46](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/precedencia.py:46).

O censo abaixo reúne os sítios identificados; **não consegui demonstrar que esgota todos os leitores de apresentação da árvore**. Os totais, portanto, são dos sítios relacionados, não um total comprovado da aplicação. [não verificado]

`Divergência` significa o par específico **chip Folga + ícone Feriado**, não qualquer diferença entre fatos ou estados. `Disponível` distingue a classe visual pronta dos fatos da precedência: `classe_dia` só representa folga/feriado e pode ser `None` para outros vereditos. [dia_decidido.py:206](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/dia_decidido.py:206).

| Sítio | Mostra; fonte e escritor | Precedência e disponibilidade; divergência | Classe |
|---|---|---|---|
| [espelho.html:171](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:171) | Chip **Folga**; `dia.folga`, escrito de `grade.tipo_dia == 'folga'` em [espelho.py:693](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:693). A grade também pode vir da célula ou do builder [espelho.py:540](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:540), [escala/utils.py:762](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/escala/utils.py:762). | **Não** lê a classe; ela já está no mesmo `dia`, sem consulta nova. **Sim**, se a grade disser folga e o veredito disser feriado. [espelho.py:345](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:345). | **MIGRA-NESTA-VOLTA**: mesmo dia do espelho. |
| [espelho.html:191](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:191) | Chip com tipo e ícone de ausência; `dia.ausencia` vem da cobertura para **cobrança**, inclusive pendentes; ícone do catálogo. [espelho.py:593](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:593), [espelho.py:859](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:859). | Não lê `classe_dia`; ela e o veredito já estão no dia. **Não** produz o par específico sem ausência; sobreposição e semântica de pendente exigem exame próprio: a cobertura de folha difere da de cobrança. [espelho.py:598](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:598). | **MIGRA-NESTA-VOLTA**, como leitor de rótulo no mesmo dia; equivalência funcional para pendentes **[não verificado]**. |
| [pdf_espelho.py:870](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/pdf_espelho.py:870) | Estrela no dia de feriado; `dia.feriado` é presença no conjunto de feriados do colaborador. [espelho.py:874](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:874), [espelho.py:1044](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:1044). | Não lê classe: ela é removida antes do desenho; `veredito_dia` e `palavra_dia` permanecem. **Não** aparece o par chip/ícone; a estrela marca também feriado trabalhado, pergunta mais ampla que a classe vencedora. [pdf_espelho.py:916](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/pdf_espelho.py:916), [espelho.py:354](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:354). | **FILA — outra pergunta**: fato de calendário, inclusive dia trabalhado. |
| [pdf_espelho.py:923](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/pdf_espelho.py:923) | Palavra de folga, feriado, ausência ou pendência; `palavra_dia`, escrita por `do_dia`. [espelho.py:341](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:341). | **Sim**, lê tradução do veredito alimentado pela precedência, com cobertura, decisão e pendente; não equivale sempre a `classe_dia`. **Não** tem o chip da grade. Classe removida, veredito disponível sem consulta. [dia_decidido.py:451](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/dia_decidido.py:451), [pdf_espelho.py:636](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/pdf_espelho.py:636). | **FILA — já lê a precedência**, preservando a pergunta da palavra do documento. |
| [\_calendario_grade.html:97](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/colaboradores/partials/_calendario_grade.html:97) | Ícone e rótulo curto de ausência; cobertura para cobrança e catálogo. [calendario.py:513](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/services/calendario.py:513), [calendario.py:700](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/services/calendario.py:700). | Não lê classe; fatos A2 e veredito já existem na montagem, sem nova consulta, embora `classe_dia` não seja publicada. O par específico **não** é exibido neste ramo; sobreposições de ausência **[não verificado]**. [calendario.py:539](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/services/calendario.py:539), [calendario.py:748](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/services/calendario.py:748). | **FILA — outra unidade**, calendário e cobertura de cobrança. |
| [\_calendario_grade.html:109](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/colaboradores/partials/_calendario_grade.html:109) | Ícone **Feriado** independente; `dia.feriado` vem do conjunto de feriados. [calendario.py:346](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/services/calendario.py:346), [calendario.py:785](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/services/calendario.py:785). | Não lê classe; A2 já está na montagem. **Não** é o par do espelho, mas o marcador pode coexistir com outro estado ou ausência porque é independente. [\_calendario_grade.html:97](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/colaboradores/partials/_calendario_grade.html:97). | **FILA — outra unidade e outra pergunta**: marca de calendário. |
| [espelho.html:151](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:151), [\_calendario_grade.html:106](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/colaboradores/partials/_calendario_grade.html:106) | Faixa semanal e calendário exibem `status`/`status_rotulo`, inclusive folga; vêm do D4. [espelho.py:167](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:167), [calendario.py:695](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/services/calendario.py:695). | Não leem `classe_dia`; A2 alimenta o calendário e a classe existe no dia do espelho. **Não** são o chip em questão; eventual combinação de estados depende do D4 **[não verificado]**. | **FILA — outra pergunta**: status da jornada. |
| [agenda_modal.html:112](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/colaboradores/partials/agenda_modal.html:112) | **Folga** para agenda sem blocos ou `eh_trab=False`; a view consulta `AgendaDia` e escala por dia. [views.py:2263](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/views.py:2263). | Sem classe/A2 no contexto identificado; custo adicional exato **[não verificado]**. **Não** mostra o par do espelho; pode chamar de folga uma data de feriado porque apresenta a agenda/escala. | **FILA — outra unidade**: agenda. |
| [painel_vinculo.html:234](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/colaboradores/partials/painel_vinculo.html:234) | **Folga** ou trabalho na projeção da agenda/escala; também lista datas do calendário de folgas. [painel_vinculo.html:203](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/colaboradores/partials/painel_vinculo.html:203), [views.py:304](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/colaboradores/views.py:304). | Classe/A2 no contexto e custo **[não verificado]**. O par do espelho **não** aparece; pode haver folga projetada em feriado. | **FILA — outra unidade**: configuração e projeção da escala. |
| [minha_escala.html:84](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/escala/minha_escala.html:84) | **Folga** nos próximos 14 dias; `trabalho` vem de `eh_dia_trabalho`. [escala/views.py:40](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/escala/views.py:40). | Não identifiquei classe/A2 no contexto; custo adicional **[não verificado]**. O par do espelho **não** aparece; a classificação de escala pode coincidir com feriado. | **FILA — outra unidade**: projeção de escala. |
| [api/views.py:252](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/api/views.py:252) | `/api/me/` fornece `trabalha_hoje` e `ancora_ausente` a partir da escala. [api/views.py:368](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/api/views.py:368). | Não fornece classe/A2 nesse bloco; custo adicional e o texto efetivamente desenhado pelo cliente **[não verificado]**. A possibilidade de interpretar `False` como folga em feriado existe no contrato; o par visual **[não verificado]**. | **FILA — outra pergunta**: escala de hoje. |
| [api/views.py:918](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/api/views.py:918) | `/api/escala/v2/` fornece `trabalho` para 14 dias, por `eh_dia_trabalho`. | Não fornece classe/A2 no bloco identificado; o código chama a escala dentro do laço, mas o número de consultas adicionais **[não verificado]**. Exibição no cliente e par visual **[não verificado]**. | **FILA — outra unidade**: escala futura. |
| [api/views.py:1392](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/api/views.py:1392), [api/views.py:1610](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/api/views.py:1610) | `/api/espelho/v2/` transporta veredito, palavra e cor do espelho, com e sem batidas. | **Sim**, a palavra vem de `do_dia` e da precedência; `_esp` já contém `classe_dia`, mas o payload mostrado não a transporta. Ler o já montado custa zero consulta; desenho no cliente e par visual **[não verificado]**. [api/views.py:1378](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/api/views.py:1378). | **FILA — já lê a precedência**; exibição do cliente não localizada **[não verificado]**. |
| [tarefa_do_dia.py:210](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/chamados/services/tarefa_do_dia.py:210), [modal_fio.html:577](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/chamados/partials/modal_fio.html:577) | A tarefa lê `tipo_dia` para encaminhamento; o modal oferece a ação **Aceitar folga**. | Não identifiquei ali rótulo de classe do dia nem classe pronta; custo adicional **[não verificado]**. O botão é ação, e o par do espelho **não** foi identificado. | **FILA — não é exibição do tipo do dia** nos sítios citados. |

**Totais dos 14 sítios agrupados acima:** `MIGRA-NESTA-VOLTA = 2`; `FILA = 12`. A classificação do chip de ausência é de escopo de exibição; não estabelece que a classe de folga/feriado substitua o tratamento de pendentes. A contagem global de leitores permanece **[não verificado]**.

A precedência declara quatro consultas **por dia** no caminho antigo e cinco para a janela no carregamento descrito; os parâmetros já carregados podem alterar o custo efetivo. No espelho, os fatos são carregados por janela e a classe é escrita no dict antes do template. [precedencia.py:259](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/precedencia.py:259), [espelho.py:658](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:658).

O selo de desempenho existente registra teto **17** para a rota `ponto/espelho` e **55** para o calendário, na fixture do próprio teste; isso não comprova o custo da tela de um colaborador selecionado ou o custo em produção. O comentário do selo do calendário diz que uma consulta nova por dia custaria pelo menos 14 na fixture. [test_selo_performance.py:150](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_selo_performance.py:150), [test_selo_performance.py:247](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_selo_performance.py:247), [test_selo_performance.py:413](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/tests/test_selo_performance.py:413).

O selo visual existente testa os dois ícones nas duas cascas, mas o fixture parte de `folga=False`; os casos citados não confrontam esse chip com feriado-em-folga. A execução e a mutação do selo **[não verificado]**. [test_espelho_decidiu_icone.py:119](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/tests/test_espelho_decidiu_icone.py:119), [test_espelho_decidiu_icone.py:198](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/tests/test_espelho_decidiu_icone.py:198).

Quanto a JavaScript próprio, a busca realizada não encontrou rotulador de folga/feriado em `app/static` fora de código de terceiros; cobertura integral de todos os clientes e das rotas `/api/ponto` **[não verificado]**. O endpoint antigo de espelho retorna 410 e aponta para v2. [api/views.py:829](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/api/views.py:829).

**Postmortem — anexos fornecidos:** no **c9**, classifico **(a), tema geral presente, não achado**: a pergunta G incluía “derivação paralela de fonte única”, mas não perguntava explicitamente se chip e ícone liam a mesma fonte; a resposta r1 não comprovou auditoria conclusiva de todos os fluxos. No **c10**, classifico **(e), não perguntado**: as quatro perguntas tratavam do medidor, linhas registradas, diff e leitores de `censo()`. Os anexos foram fornecidos no enunciado sem linhas físicas, portanto a localização arquivo:linha desses trechos é **[não verificado]**.

`perguntas de lei: 0` — conforme o aval transcrito, que declara “nenhuma lei nova” e “juízes novos=0”; localização física do anexo **[não verificado]**.

## 19/11 08:46 · sessao 5e19e34d290f · ESPURIA-TELA-C11-1911-0845 · verificador · origem=Code

**AVAL-DEVOLVIDO no topo: itens 2, 3 e 4**, nos pontos especificados abaixo. As premissas de código do item 1 procedem. A revisão é somente sobre a cópia e os anexos fornecidos; a identidade Git da cópia e o estado vivo não puderam ser verificados porque a cópia não contém `.git`. [não verificado]

**1. Premissas — PROCEDE.**

- O chip “Folga” lê `dia.folga` em [espelho.html:171](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:171); o ícone recebe `dia.classe_dia` em [espelho.html:193](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:193).
- **Quem escreve e lê `folga`:** `montar_dias` obtém `_folga` de `tipo_dia` da grade em [espelho.py:688](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:688) e grava o campo em [espelho.py:778](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:778). A fonte normal da grade é a célula; para o trecho anterior e em degradação existe o builder de escala, conforme [espelho.py:511](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:511) e [espelho.py:503](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:503). No template, `folga` também participa da condição que faz o card existir, além de produzir o chip: [espelho.html:165](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:165).
- **Quem escreve e lê `classe_dia`:** `aplicar_palavra_do_dia` escreve a classe a partir do veredito em [espelho.py:329](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:329); a tela a lê no ícone em [espelho.html:193](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:193). Os fatos vêm de `fatos_do_periodo` em [espelho.py:660](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:660), passam pelo veredito em [dia_decidido.py:397](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/dia_decidido.py:397) e pela tradução em [dia_decidido.py:206](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/dia_decidido.py:206). Cobertura ou decisão rejeitada podem anteceder o exame do tipo do fato; não se deve reduzir todo veredito à precedência feriado/folga: [dia_decidido.py:397](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/dia_decidido.py:397).
- O fixture comum fixa `folga=False` em [test_espelho_decidiu_icone.py:119](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/tests/test_espelho_decidiu_icone.py:119), e o teste de classe o reaproveita mesmo para o caso `FOLGA`, em [test_espelho_decidiu_icone.py:185](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/tests/test_espelho_decidiu_icone.py:185).
- A precedência já existe: [precedencia.py:46](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/precedencia.py:46) ordena feriado antes de folga, e [precedencia.py:379](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/precedencia.py:379) monta e ordena os fatos. O lookup anexo também a registra em [JUIZES.md:127](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/docs/JUIZES.md:127). Há a ressalva expressa de feriado apenas no pagamento em [precedencia.py:369](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/precedencia.py:369).

**2. Colisão com aval vigente — PROCEDE como ponto a devolver para o pouso.** [nao verificado]

O complemento fornecido determina que a ESPURIA reprovada sai do trem e que, para a c10 pousar por aquela via, o conteúdo final deve coincidir arquivo a arquivo com o comboio C; o trem também proíbe pousar árvore diferente da certificada. A c11 exige nova alteração, suite sobre o main do momento e opus final. Assim, **não está resolvido no literal qual certificação e qual `cmp` autorizam o pouso da c11**: comparar uma c11 alterada com a C antiga pode conflitar com a exigência de igualdade; certificar uma nova árvore não revoga por si só a cláusula específica de igualdade com C. A mudança de estado efetiva do main e da certificação é [não verificado]. Fonte: anexos fornecidos `complemento1_trem_1911_0825.txt`, `trem_de_pouso_1911_0815.txt` e `espuria_tela_c11_1911_0845.txt`, sem numeração de linhas. [nao verificado]

Devolver a compatibilização explícita: árvore resultante a certificar, referência do `cmp` e tratamento da igualdade com C. O `PROIBIDO` do trem continua relevante ao pouso; a suite e o opus pedidos na c11 não bastam, no texto fornecido, para demonstrar a revogação daquela igualdade. [não verificado] [nao verificado]

**3. Condições literais — PROCEDE como pontos incompletos, com duas premissas que fecham.**

- **Duas cascas fecham:** a segunda é `base_app.html`; o seletor a escolhe para modo app em [context_processors.py:194](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/context_processors.py:194), e o mesmo template usa `base_tpl` em [espelho.html:1](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:1). O teste existente renderiza ambas em [test_espelho_decidiu_icone.py:183](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/tests/test_espelho_decidiu_icone.py:183); isso não prova que o novo RED já exista ou tenha passado. [não verificado]
- **Exemplo por PK e feriado na comp. 11:** a existência no cadastro e o colaborador correspondente são [não verificado]. Para medir depois, o cadastro é `Feriado`, com data e abrangência em [models.py:5](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/models.py:5); a consulta por intervalo está em [feriados.py:27](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/feriados.py:27), e a aplicação ao colaborador considera o lugar do vínculo do dia em [feriados.py:135](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/core/feriados.py:135). Também seria necessário cruzar a folga da grade e o veredito efetivo. [espelho.py:688](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/ponto/services/espelho.py:688)
- **“Vaga do instrumento/tela”:** a casa documenta ordem de vagas incluindo instrumento e teto de construtores em [CLAUDE.md:290](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/CLAUDE.md:290), e há referência histórica a agente de tela em [RELATO.md:496](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/docs/RELATO.md:496). Uma vaga única formal chamada **“instrumento/tela”**, sua disponibilidade agora e o rearme do B3 são [não verificado]. O código reserva o rearme a Ronald em [disjuntor.py:228](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/bin/disjuntor.py:228).
- **Referência visual:** não localizei `referencia_1911.md` na cópia pesquisada; sua existência fora dela e a presença da medida do espelho são [não verificado]. Devolver a referência e a medida concreta antes de considerar satisfeito o CHECK-DE-TELA. [não verificado]

**4. Exportada, dinheiro ou `!` fora do declarado — PROCEDE como risco documental a delimitar; mudança de dinheiro ou gravado não demonstrada.**

O chip atual é renderização do template [espelho.html:171](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/templates/ponto/espelho.html:171). O cartão/PDF, porém, é documento: ele imprime `palavra_dia` em [pdf_espelho.py:916](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/pdf_espelho.py:916), e o PDF auditável de espelho gera registro com hash em [pdf_espelho.py:1074](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/pdf_espelho.py:1074) e [models.py:14](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/models.py:14). O cartão aplica a porta com `limpar=True`, removendo `classe_dia`, em [pdf_espelho.py:636](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/pdf_espelho.py:636).

Os totais do cartão, quando o período é a competência, leem a mesma fonte que escreve o TXT, conforme [pdf_espelho.py:624](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/relatorios/pdf_espelho.py:624); há também uma porta do export que confere divergências entre leitores em [porta_export.py:1](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/app/folha/porta_export.py:1). Isso demonstra relação com a folha e sua validação, mas **não demonstra que o rótulo do chip alimente o TXT, que o PDF alimente a assinatura, ou que a alteração proposta mova dinheiro**. [não verificado]

A lista de `!` inclui tocar o gravado de competência exportada e regra de negócio fora do pedido em [CLAUDE.md:265](/home/ronald/codex_ws/aca3771430418cb34f356b83a9080387d6c9dfe3/CLAUDE.md:265). Não encontrei nela uma regra que classifique automaticamente como `!` toda mudança de exibição posterior à assinatura; a existência, mecanismo e conteúdo de um espelho efetivamente assinado são [não verificado]. **Alterar um documento já assinado ou seu registro seria materialmente distinto de mudar a tela de uma leitura futura**; o aval deve delimitar esse alcance antes de migrar leitores documentais. [não verificado]

`AVAL-DEVOLVIDO: 2 — compatibilizar certificação e cmp da c11 com a igualdade exigida pelo complemento do trem; 3 — identificar vaga, referência e medida do espelho, e demonstrar o exemplo por PK na comp. 11; 4 — delimitar o alcance sobre documentos e assinaturas existentes e esclarecer qualquer migração documental que toque gravado ou regra fora do pedido.`

_7303 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
