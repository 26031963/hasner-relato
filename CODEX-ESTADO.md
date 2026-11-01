# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 14:40** · publicado em **01/11/2026 14:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 14:40 · sessao 67798c11cab9 · COBERTURA-PARCIAL-UM-JUIZ · censo · origem=Code

**O dado fornecido não distingue “a cura mudou a decisão” de “a versão fez o dia chegar novamente à decisão”.** A fábrica só recebe datas julgadas; impressão igual pode impedir essa chegada. Para os três casos da comp09, falta saber a impressão anterior e a declaração efetivamente usada em B do controle. (`app/ponto/services/cartorio.py:1214`, `app/ponto/services/cartorio.py:1282`, `app/ponto/services/cartorio.py:878`; estado desses casos: `[nao verificado]`)

**1. Caminho do cartório até o chamado**

O caminho comum aos dois subtipos é:

1. A passada seleciona as células anteriores a hoje e chama `julgar_colab`. O corpo monta a grade, compara a impressão e, se não pular, classifica o dia, registra sua data em `out['cods']`, lavra ata/veredito e carimba a impressão. (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:987`, `app/ponto/services/cartorio.py:1218`, `app/ponto/services/cartorio.py:1269`, `app/ponto/services/cartorio.py:1282`, `app/ponto/services/cartorio.py:1653`, `app/ponto/services/cartorio.py:1675`)

2. Depois do corpo, com `apply_`, `julgar_colab` chama `perguntar_pela_celula` para as datas presentes em `out['cods']`. (`app/ponto/services/cartorio.py:864`, `app/ponto/services/cartorio.py:868`, `app/ponto/services/cartorio.py:878`)

3. `motivos_a_perguntar` exige: colaborador ativo; dia não calado por `dia_sem_trabalho`; data não futura; veredito preenchido pertencente a `VEREDITOS_ACUSAM`; e motivo ainda não perguntado naquele dia, desconsiderando duplicatas. Também calcula quais marcos venceram no instante. (`app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/fabrica_da_celula.py:82`, `app/chamados/services/fabrica_da_celula.py:87`, `app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:111`)

4. `motivos_pela_ata` → `_ata_do_dia` → `_marcos_faltantes` → `grade_da_celula`. O leitor converte lâmpada apagada em `missing`; `_marcos_faltantes` seleciona esses marcos e atribui sua posição pela ordem da ata. (`app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/disputa_emissao.py:827`, `app/chamados/services/declaracao_texto.py:82`, `app/escala/services/leitor_celula.py:105`, `app/escala/services/leitor_celula.py:119`, `app/chamados/services/declaracao_texto.py:110`)

| Marco faltante na ata | Motivo produzido |
|---|---|
| Último par, tipo `S`: posição `fim` | `orfao_14h` |
| Par intermediário, tipo `S`: posição `int_ida` | `intervalo_saida` |

A posição e o mapa estão em `app/chamados/services/declaracao_texto.py:113`, `app/chamados/services/declaracao_texto.py:118` e `app/chamados/services/declaracao_texto.py:42`. **Nesse ramo, `orfao_14h` é o nome associado à saída final; não há teste literal de 14 horas.** O filtro temporal usa os marcos vencidos. (`app/chamados/services/fabrica_da_celula.py:27`, `app/chamados/services/fabrica_da_celula.py:59`)

5. A fábrica chama `perguntar`, um motivo por vez, com contexto `{motivo: {'data': dia}}`, dentro de `push_disputa_silenciado`. (`app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/services/fabrica_da_celula.py:124`, `app/chamados/services/fabrica_da_celula.py:159`)

6. `criar_ou_atualizar_disputa` passa `subcategoria=motivos_lista[0]` ao núcleo. Havendo disputa aberta, agrega motivos; sem ela, chama `ChamadoColaborador.abrir` com categoria/módulo `disputa_supervisao`, subcategoria e contexto recebidos. O nascimento efetivo fica no `get_or_create`. (`app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:601`, `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:471`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:514`, `app/chamados/models.py:930`)

**O pai de disputa nasce antes das perguntas e antes do eventual c2.** Depois, `gerar_perguntas_disputa` abre a pergunta e tenta reatá-la quando nasceu sem destino. (`app/chamados/services/disputa_emissao.py:605`, `app/chamados/services/disputa_emissao.py:389`, `app/chamados/services/disputa_emissao.py:404`)

**2. Alterações do diff que alcançam esse caminho**

Abaixo, identifico as linhas alteradas pela sua localização **na árvore da cura**, correspondente aos hunks fornecidos.

| Linhas alteradas | Mudança e condição de efeito |
|---|---|
| `app/ponto/services/cartorio.py:111` | Acrescenta `(3,'2026-09')`. Pode fazer chegar ao julgamento uma célula cuja impressão anterior continha outra versão; não muda sozinho o motivo emitido. A versão entra no hash em `app/ponto/services/cartorio.py:393`; o pulo ocorre em `app/ponto/services/cartorio.py:1218`. |
| `app/ponto/services/cartorio.py:349`, `app/ponto/services/cartorio.py:1054`, `app/ponto/services/cartorio.py:1210` | A impressão passa a incluir os índices dos marcos `dispensada`. Se o período muda quais marcos são dispensados, a impressão muda mesmo quando o agregado continua, por exemplo, `fora`. Sem marco dispensado, o helper devolve o insumo anterior. (`app/ponto/services/cartorio.py:358`) |
| `app/escala/utils.py:353`, `app/escala/utils.py:365`, `app/escala/utils.py:1233`, `app/escala/utils.py:1243` | A grade usa os instantes da cadeia do turno e dispensa somente `missing` dentro do período declarado. Um `S` fora continua apagado e pode gerar um dos dois motivos; dentro deixa de ser faltante. Sem período ou sem deslocamento conhecido, esse bloco não dispensa. (`app/escala/utils.py:1247`) |
| `app/ponto/turnos.py:2656`, `app/escala/services/leitor_celula.py:416` | As horas reais passam a usar deslocamentos da cadeia do turno, inclusive madrugada em D+1. Isso pode mudar o agregado `dentro/fora/nao_sei` e, portanto, a classificação de uma parcial noturna. (`app/ponto/turnos.py:2662`, `app/escala/services/leitor_celula.py:420`) |
| `app/ponto/services/cartorio.py:470`, `app/ponto/services/cartorio.py:641` | A ata conserva `dispensada` por marco e passa a carregar `cobertura_parcial=True`. Esse segundo carimbo permite à fábrica reconhecer que `tipo_dia='ausencia'` ainda tem resto cobrável. (`app/chamados/juizes.py:424`) |
| `app/ponto/services/cartorio.py:665` | A dispensa integral passa a depender do carimbo da grade, substituindo o teste cru de minutos. Na parcial, esse bloco não acende todos os marcos; preserva o resultado por período vindo da grade. No dia inteiro, pode retirar faltantes e impedir perguntas. (`app/ponto/services/cartorio.py:667`, `app/ponto/services/cartorio.py:672`) |
| `app/chamados/juizes.py:398`, `app/chamados/juizes.py:424` | **Abre o portão principal:** ata `ausencia` com `cobertura_parcial` deixa de significar dia sem trabalho, desde que `trabalha is not False`. No controle, o retorno anterior calava qualquer ata preenchida com tipo diferente de `trabalho`. (`app/chamados/juizes.py:426`, linha anterior no diff fornecido) |
| `app/chamados/services/declaracao_texto.py:97` | **Abre o segundo portão principal:** `_marcos_faltantes` passa a percorrer o dia de ausência parcial; antes, descartava todo `tipo_dia != 'trabalho'`. Só retorna os marcos que continuam `missing`. (`app/chamados/services/declaracao_texto.py:98`, `app/chamados/services/declaracao_texto.py:110`) |
| `app/ponto/services/cartorio.py:1229`, `app/escala/services/leitor_celula.py:91`, `app/escala/services/leitor_celula.py:127`, `app/escala/services/leitor_celula.py:343` | Marco dispensado deixa de contar como batida/casamento e conserva seu marcador na leitura. Isso evita alterar artificialmente o número de batidas e o enquadramento do dia. |
| `app/ponto/supra_juiz.py:311`, `app/ponto/supra_juiz.py:390`, `app/ponto/supra_juiz.py:437`, `app/ponto/supra_juiz.py:530` | Modifica a decisão consumida pelo portão de veredito: parcial sem resto não acusa furo sem batida; parcial com fato, período `fora` e marco faltante pode produzir `FURO_PARCIAL`; parcial com batida sem par passa a ser examinada; cobrança viva do resto deixa de produzir `COBRANCA_EM_DIA_COBERTO`. (`app/ponto/supra_juiz.py:313`, `app/ponto/supra_juiz.py:396`, `app/ponto/supra_juiz.py:439`, `app/ponto/supra_juiz.py:531`) |

O critério de parcial não é somente o nome do tipo: exige tipo no catálogo de parciais, um único dia e `0 < minutos < 480`. O período tem bordas inclusivas; `hora_fim < hora_ini` atravessa a meia-noite. (`app/ponto/turnos.py:2595`, `app/ponto/turnos.py:2632`, `app/ponto/turnos.py:2636`, `app/ponto/turnos.py:2639`)

Há mudanças próximas que precisam ser separadas do nascimento desses pais:

- `_coberto_por_ausencia` passa de “existe alguma não-falta cobrindo” para a ausência **vencedora**, integral ou sem período. Isso altera o agrupador/complementador e o c2; **não é veto direto de `gerar_perguntas_disputa` neste caminho**, que chama `PerguntaDisputa.abrir` diretamente. (`app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:81`, `app/chamados/services/disputa_emissao.py:763`, `app/chamados/services/disputa_emissao.py:389`, `app/chamados/reconciliador.py:828`)

- `_celula_furada` deixa de considerar qualquer ausência suficiente para regularizar o dia; passa a exigir ausência que explique o dia inteiro. É mudança de premissa/regularização, não o teste que escolhe o subtipo do pai acima. (`app/chamados/juizes.py:446`, `app/chamados/juizes.py:447`, `app/chamados/models.py:1247`)

- `parcial_so_lista`, as novas âncoras e o espelhamento dos marcos alteram os portões do **chamado do dia**. A fábrica do pai `disputa_supervisao` não aplica `parcial_so_lista`. Portanto, ausência de fato novo pode calar `batida_ausente` e ainda permitir o pai de disputa. (`app/ponto/services/cartorio.py:1560`, `app/ponto/services/cartorio.py:1578`, `app/ponto/services/cartorio.py:1630`, `app/ponto/services/cartorio.py:1643`, `app/chamados/reconciliador.py:848`, `app/chamados/services/fabrica_da_celula.py:146`, `app/chamados/services/fabrica_da_celula.py:158`)

- Pela mesma razão, **“parcial sem período” não é veto universal desse pai**: `_marcos_faltantes` aceita a parcial sem testar o período, e a fábrica admite também o veredito `indefinida`. (`app/chamados/services/declaracao_texto.py:98`, `app/chamados/services/fabrica_da_celula.py:88`, `app/escala/models.py:1735`)

**3. Os três `orfao_14h` e a subida de versão**

**O controle nativo não contém a subida v3.** A linha removida do diff declara `((1,'2026-09'), (2,'2026-10'))`; a cura acrescenta `(3,'2026-09')`. Para comp09, `versao_para` retorna respectivamente 1 e 3, pois escolhe a maior versão cujo horizonte alcance a competência. (`app/ponto/services/cartorio.py:111`, comparação fornecida no diff; `app/ponto/services/cartorio.py:205`, `app/ponto/services/cartorio.py:208`)

Assim:

- **B do controle não rejulga esses dias “por v3” apenas por executar o código do controle.** Se a impressão continua igual, pula antes da fábrica. Pode rejulgar por outro insumo diferente ou por `forcar`. (`app/ponto/services/cartorio.py:1214`, `app/ponto/services/cartorio.py:1218`)
- **Se a sonda injetou explicitamente v3 também no controle**, uma célula antes impressa com `regra:1` teria a igualdade rompida também ali. Essa injeção não é demonstrada pelo rótulo “B (bump v3)” nem pelos contadores fornecidos: `[nao verificado]`. O mecanismo é o mesmo hash e ramo de pulo. (`app/ponto/services/cartorio.py:393`, `app/ponto/services/cartorio.py:1208`, `app/ponto/services/cartorio.py:1218`)

| Hipótese | Compatibilidade pelo código |
|---|---|
| **(i) Só ganhou nova passagem; o controle já emitiria se julgasse o mesmo estado** | Compatível. Um dia de trabalho com saída final apagada, veredito acusador e sem pergunta anterior já satisfaz o caminho; a versão pode apenas fazê-lo chegar à fábrica. (`app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/fabrica_da_celula.py:111`, `app/chamados/services/declaracao_texto.py:44`) |
| **(ii) Precisa da mudança funcional da cura** | Também compatível. Se a grade/ata representa ausência parcial com saída final ainda apagada, os antigos descartes por `ausencia` impediam esse caminho, mesmo forçando o julgamento. (`app/chamados/juizes.py:424`, `app/chamados/juizes.py:426`, `app/chamados/services/declaracao_texto.py:98`, comparação com linhas removidas do diff) |

**Qual hipótese vale para col150, col304 e col476: `[nao verificado]`.** O DET não mostra ausência, período, ata anterior, impressão anterior nem a declaração efetiva da sonda B. Os totais agregados não permitem reconstruir esses valores por célula. (`app/ponto/services/cartorio.py:1205`, `app/ponto/services/cartorio.py:1214`)

A competência é calculada pelo corte da empresa, não pelo mês civil. Com corte 21, os três dias informados pertencem à comp09; o corte efetivo da empresa 3 permanece `[nao verificado]`. (`app/ponto/janelas.py:24`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:60`)

**4. Dados que decidem e consultas mínimas, sem executar**

| Caso fornecido | Dado decisivo a confrontar no estado anterior à passada |
|---|---|
| col222 — 2026-10-28 — `intervalo_saida` `[nao verificado]` | Ata/grade com `S` intermediário apagado; ausência aprovada vencedora, parcialidade e posição desse marco perante o período; veredito e pergunta anterior. (`app/chamados/services/declaracao_texto.py:118`, `app/escala/utils.py:1187`, `app/escala/utils.py:1247`, `app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/services/fabrica_da_celula.py:103`) |
| col150 — 2026-09-11 — `orfao_14h` `[nao verificado]` | Os mesmos dados, agora para o `S` final, mais impressão anterior e versão efetivamente usada em B. (`app/chamados/services/declaracao_texto.py:115`, `app/chamados/services/declaracao_texto.py:44`, `app/ponto/services/cartorio.py:1208`, `app/ponto/services/cartorio.py:1214`) |
| col304 — 2026-08-31 — `orfao_14h` `[nao verificado]` | Mesmo recorte de col150; confirmar competência pelo corte da empresa. (`app/ponto/janelas.py:60`, `app/ponto/services/cartorio.py:205`) |
| col476 — 2026-08-21 — `orfao_14h` `[nao verificado]` | Mesmo recorte de col150; confirmar especialmente a borda da competência pelo corte. (`app/ponto/janelas.py:27`, `app/ponto/janelas.py:60`) |

As consultas ORM abaixo leem as tabelas dos modelos indicados. Devem ser aplicadas **ao snapshot anterior às respectivas passadas** para decidir o contrafactual histórico; o cartório sobrescreve ata e impressão ao julgar. (`app/ponto/services/cartorio.py:1655`, `app/ponto/services/cartorio.py:1675`)

```python
# Proposta de consulta; não executada.
from datetime import date, datetime, time, timedelta
from django.db.models import Q
from django.utils import timezone
from colaboradores.models import Empresa, Colaborador
from escala.models import CelulaDia
from ponto.models import Ausencia, Batida, TipoAusencia
from chamados.models import PerguntaDisputa, DisputaSupervisao

casos = [
    (222, date(2026, 10, 28)),
    (150, date(2026, 9, 11)),
    (304, date(2026, 8, 31)),
    (476, date(2026, 8, 21)),
]

Empresa.objects.filter(pk=3).values("id", "dia_inicio_competencia")
TipoAusencia.objects.filter(ativo=True).values("codigo", "parcial")

for cid, d in casos:
    Colaborador.objects.filter(pk=cid, empresa_id=3).values(
        "id", "situacao", "isento_ponto",
        "data_admissao", "data_demissao"
    )

    CelulaDia.objects.filter(
        colaborador_id=cid, data__range=(d - timedelta(days=1), d)
    ).values(
        "id", "data", "trabalha", "escala_geradora_id", "dna",
        "ata", "veredito", "protestos", "julgada_em", "impressao",
        "editada_em", "regenerada_em"
    )

    Ausencia.objects.filter(colaborador_id=cid).exclude(
        status="rejeitada"
    ).filter(
        Q(data=d)
        | Q(data__lte=d, data_fim__gte=d)
        | Q(data__lte=d, data_fim__isnull=True,
            tipo__in=Ausencia.TIPOS_EM_ABERTO)
    ).order_by("-data", "-pk").values(
        "id", "data", "data_fim", "tipo", "status",
        "minutos", "hora_ini", "hora_fim",
        "criado_em", "atualizado_em", "aprovada_em"
    )

    t0 = timezone.make_aware(
        datetime.combine(d - timedelta(days=1), time.min))
    t1 = timezone.make_aware(
        datetime.combine(d + timedelta(days=2), time.min))
    Batida.objects.filter(
        colaborador_id=cid, timestamp__gte=t0, timestamp__lt=t1
    ).values(
        "id", "timestamp", "tipo", "is_intervalo",
        "criado_em", "retratada_em"
    )

    PerguntaDisputa.objects.filter(
        disputa__colaborador_id=cid, data_evento=d
    ).values(
        "id", "motivo", "via_resolucao", "disputa_id",
        "celula_id", "chamado_destino_id",
        "respondida_em", "validada_em", "materializada_em"
    )

    # A disputa aberta é procurada por colaborador, sem filtro de data.
    DisputaSupervisao.objects.filter(colaborador_id=cid).values(
        "id", "motivos", "contexto", "chamado_id",
        "aberta_em", "fechada_em"
    )
```

O filtro e a ordem das ausências reproduzem a autoridade de cobertura. **Para a grade, tomar a primeira aprovada; para cobrança, a primeira não rejeitada** — podem ser registros distintos. (`app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2557`, `app/escala/utils.py:1187`)

O recorte de batidas preserva também as retratadas para examinar o fato novo; a retração pode ser justamente o evento ocorrido depois do julgamento. A véspera da célula atende ao leitor de faltantes. (`app/ponto/services/cartorio.py:292`, `app/ponto/services/cartorio.py:294`, `app/chamados/services/declaracao_texto.py:81`)

Se houver parcial noturna, consultar ainda a geradora e os vínculos candidatos, sem substituí-los por “escala ativa”:

```python
from escala.models import EscalaColaborador, TipoEscala
from escala.alimentacao import q_vinculo_cobre_a_data

# gid vem de CelulaDia.escala_geradora_id.
EscalaColaborador.objects.filter(colaborador_id=cid).filter(
    Q(pk=gid) | q_vinculo_cobre_a_data(d)
).values()

# tids vêm dos vínculos retornados; incluir template_pai quando houver.
TipoEscala.objects.filter(pk__in=tids).values()
```

A geradora tem precedência; os marcos/pausas e a cadeia determinam se a madrugada pertence a D+1 e se o marco está dentro do período. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:200`, `app/escala/utils.py:353`, `app/escala/utils.py:365`; herança do template: `app/escala/models.py:806`)

Para testar especificamente o veto de fato novo dos chamados do dia, acrescentar:

```python
from ponto.models import Justificativa

Justificativa.objects.filter(
    colaborador_id=cid, data_turno=d
).values("id", "status", "criado_em", "atualizado_em", "aprovada_em")
```

Esses carimbos, junto com ausência, batida, edição/regeneração e marcos vencidos, são comparados com o julgamento anterior por `houve_fato_novo`. **Eles não substituem o exame do portão próprio do pai de disputa.** (`app/ponto/services/cartorio.py:297`, `app/ponto/services/cartorio.py:301`, `app/ponto/services/cartorio.py:307`, `app/ponto/services/cartorio.py:339`, `app/chamados/services/fabrica_da_celula.py:146`)

Nenhuma consulta ao banco resolve qual constante a **sonda** instalou em B: isso depende do instrumento executado, ausente do dado fornecido. `[nao verificado]`

## 01/11 14:39 · sessao e24157d16709 · COBPARC-ATA-DEFASADA-09 · censo · origem=Code

O dado separa **código atual × ata gravada** de **efeito adicional da subida de versão**; o registro local reproduz os cinco deltas e relata controle sem a cura COBPARC igual à cura, campo a campo. Isso não identifica, sozinho, o commit causador nem o dono de cada dia. (`app/docs/BACKLOG.md:759–760`; `app/ponto/management/commands/e6_oraculo.py:98–110`)

A correspondência histórica exata **hash → diff**, assim como um censo exaustivo dos commits, fica **[nao verificado]**: esta cópia não disponibiliza o histórico Git. Abaixo, os hashes vêm do anexo e as linhas apontam o comportamento presente na cópia; não atribuo os deltas individuais por coincidência numérica. [nao verificado]

**1. Escritores e leitores**

O caminho se divide: **FechamentoMensal não é calculado somando DiaPago**. O fechamento calcula os valores e entrega os mesmos períodos e mapas à lavratura de DiaPago; a soma de DiaPago existe como leitor separado de conferência. (`app/ponto/services/fechamento.py:877–878, 1028–1032, 1168–1185`; `app/ponto/services/dia_pago.py:548–561`)

| Campo | Quem produz e escreve | Quem lê e onde chega ao fechamento |
|---|---|---|
| `dias_previstos` | Não é rubrica de DiaPago: é contagem dos dias da grade cujo `tipo_dia` seja `trabalho` ou `ausencia`. O fechamento escreve `len(_dt_trab)`. (`app/ponto/services/dia_pago.py:99`; `app/ponto/services/fechamento.py:1014–1017, 1052`) | A grade do fechamento lê a célula/ata; `tipo_dia` vem da ata, com alternativa em `cel.trabalha`. (`app/folha/export.py:318–327`; `app/escala/services/leitor_celula.py:345–346`) |
| `minutos_previstos` | A grade soma segmentos previstos E→S; o cartório copia o resultado para a ata; a porta persiste a ata. (`app/escala/utils.py:1569–1590`; `app/ponto/services/cartorio.py:599–600`; `app/ponto/portas/celula.py:767–775`) | O leitor prefere `ata.minutos_previstos`; sem essa chave consulta `minutos_previstos_do_dia`. O fechamento soma trabalho/ausência; DiaPago recebe o mapa diário. (`app/escala/services/leitor_celula.py:347–354`; `app/ponto/services/fechamento.py:1014–1016, 1051, 1180`; `app/ponto/services/dia_pago.py:286–290`) |
| `minutos_realizados` | `realizado_dos_turnos` mede os turnos, descontando pausa efetiva ou janela fixa aplicável; a grade publica o resultado e o cartório o copia para a ata. (`app/ponto/turnos.py:705–769`; `app/escala/utils.py:1994–2007`; `app/ponto/services/cartorio.py:601`) | O leitor lê a ata; `por_dia_da_grade` aplica **cap por dia**: `min(realizado, previsto)`, no universo trabalho/ausência. Esse mapa alimenta tanto DiaPago quanto o total mensal. (`app/escala/services/leitor_celula.py:365`; `app/ponto/services/dia_pago.py:385–399`; `app/ponto/services/fechamento.py:1028–1032, 1181`) |
| `horas_trabalhadas` | O motor escreve `periodo.minutos_trabalhados`; o resultado soma períodos/60. O fechamento acrescenta os minutos sem vínculo e a folga sem escala certa. DiaPago escreve dos mesmos períodos e complementos. (`app/ponto/motor_calculo_v2.py:2773, 295–296`; `app/ponto/services/fechamento.py:877–878, 948–953`; `app/ponto/services/dia_pago.py:178, 262–269`) | A lavra da **ata não fornece esse campo**: a origem monetária é o motor, enquanto os minutos da grade seguem a ata. Os dois chegam separadamente à lavratura. (`app/ponto/services/cartorio.py:599–601`; `app/ponto/services/fechamento.py:1169–1181`) |
| Intrajornada: `horas_intra_indenizada` | O motor calcula o suprimido e escreve `minutos_intrajornada_indenizada`; o resultado soma períodos normais e de folga/60. (`app/ponto/motor_calculo_v2.py:2755–2772, 363–370`) | O fechamento escreve a propriedade do resultado; DiaPago soma a intra dos períodos normais e dos ramos de folga. (`app/ponto/services/fechamento.py:972`; `app/ponto/services/dia_pago.py:186, 223–225, 234–263`) |

A lavratura de DiaPago materializa as linhas com `bulk_create`; o fechamento persiste por `save()` ou pelo ramo de `update()` dos campos calculados. (`app/ponto/services/dia_pago.py:343–358`; `app/ponto/services/fechamento.py:1105–1112, 1128–1153`)

**2. Mudanças identificáveis e respectivas condições**

Na coluna “dono”, **D** significa aplicar a decisão ordenada do item 4; a condição técnica descrita, sozinha, não fornece os argumentos necessários para escolher um dono. (`app/ponto/management/commands/e6_oraculo.py:53–54, 98–110`)

| Hash(es) do anexo | Condição em que o valor pode mudar com os mesmos dados | Campos e trecho atual | Dono |
|---|---|---|---|
| `212971ad` | Folga lançada em parte do mês: fora da foto, o ciclo continua respondendo pela folga semanal. Células já existentes prevalecem; não se pode pressupor regeneração. | Dias/minutos previstos: `app/escala/models.py:1071–1073, 1270–1292`; limite documentado em `app/docs/TICKETS.md:319`. | D |
| `a5cd3d39` | Foto parcial em 12x36/24x48: fora da foto, usa a paridade da folga mais próxima, em vez de considerar trabalho indiscriminadamente. | Dias/minutos previstos: `app/escala/models.py:1294–1335`. | D |
| `10832b45` / `a2e903e1`, `96beb803` | Dia com segunda pausa: DNA e sequência da grade passam a carregar todas as pausas, alterando os segmentos previstos e o casamento das batidas. | Previstos e geometria do realizado: `app/escala/models.py:1141–1181`; `app/escala/utils.py:1560–1590`. | D |
| `fecb9a67` | O juiz de previsto descontava somente a primeira pausa; passa a descontar todas. O comentário documenta 635→550. É um produtor distinto da soma de segmentos da grade. | Previsto consultado pelo motor e pela alternativa do leitor: `app/escala/utils.py:2224–2268`; `app/escala/services/leitor_celula.py:347–354`; associação documental em `app/docs/TICKETS.md:243`. | D |
| `801253ed` | Ata com várias pausas legíveis: o motor passa a receber todas, preservando os instantes reais das batidas. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:694–721`. | D |
| `107e3bb5` | O papel das batidas muda na própria montagem: o realizado passa a usar a ata dessa montagem, em vez da anterior. Em intermitente, previsto também acompanha o realizado. | Realizados e previsto intermitente: `app/escala/utils.py:1802–1847, 1994–2019`. | D |
| `14ec97c2` / `825bb46a` | Vínculos concorrentes: a geradora da célula decide o vínculo do dia; geradora ausente ou fora da vigência devolve `None`. | Dias/previstos e cálculo dependente do vínculo: `app/escala/utils.py:1250–1263`; `app/escala/alimentacao.py:237–254`. | **CADASTRO quando sem vínculo**, ramo `e6_oraculo.py:98–99`; demais casos D. |
| `8ec526f2` | Folga de calendário pertence a outro vínculo: deixa de valer naquele dia; folga manual continua valendo para a pessoa. A alteração de `tipo_dia` repercute no universo contado/somado. | Dias/previstos/realizados: `app/ponto/precedencia.py:222–239`; `app/escala/utils.py:1103–1128`; `app/ponto/services/fechamento.py:1014–1017`; `app/ponto/services/dia_pago.py:385–399`. | D |
| `879ff834` | Ausência com batidas: passa a incluir realizado coberto, limitado ao previsto; o abono cobre somente o saldo. | Realizados: `app/ponto/services/dia_pago.py:373–399`. | D |
| `7b8329a1` | Pausa declarada pelas duas luzes da ata: a declaração tem prioridade sobre o casamento guloso, inclusive acima do teto da pausa deduzida. | Geometria que alimenta trabalhadas/realizados/intra: `app/ponto/turnos.py:802–814, 1777–1784, 1805–1824`. | D |
| `912ab728` | Uma perna da pausa acendeu e a outra sobrou: o ramo passa a aceitar essa forma dentro do teto. | Geometria dos mesmos campos: `app/ponto/turnos.py:1755–1765`. | D |
| `3d037f1b` | Duas células reivindicam a mesma batida: a reivindicação posterior é recusada. | Geometria dos mesmos campos: `app/ponto/turnos.py:1786–1801`. | D |
| `ce88575c` | Batida de outro dia próxima ao horário do intervalo: o envelope impede absorvê-la como pausa ou saída do turno anterior. | Geometria dos mesmos campos: `app/ponto/turnos.py:1326–1350`. | D |
| `c7b8bfdb` | Todas as pausas marcadas estão abaixo do piso: a guarda passa a consultar os intervalos efetivos, permitindo descontar a janela fixa quando aplicável. | Realizados: `app/ponto/turnos.py:742–762`. | D |
| `185b9af0` / `5d9ec893` / `a5ae5013` | Forma posicional com saída em outro turno e um único vão interior elegível: mede primeira entrada→última saída, menos a pausa. | Realizados: `app/ponto/turnos.py:671–730`. | D |
| `ed68dfd7` | Entradas recebidas juntas por flush, mas pertencentes a turnos distintos: o teto de 14h impede apagar a entrada seguinte como eco. | Geometria dos mesmos campos: `app/ponto/turnos.py:342–370`. | D |
| `dbd21fae` | Saída além do teto da jornada: passa a usar o teto do dia; a forma L-084 nas duas pontas conserva o envelope de 26h. | Geometria dos mesmos campos: `app/ponto/turnos.py:1419–1443`. | D |
| `0728ce98` | Cauda após meia-noite: substituição de períodos passa a remover pelo dia da jornada, preservando a cauda da véspera. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:679–688`. | D |
| `de5b1a61`, `596064b7`, `cfd4ff83`, `8c851906` | Empresa/dia sujeitos à janela HE, pontas fora dos limites e extra declarada: o recorte muda os instantes efetivos; folga trabalhada sai desse recorte. A L-084 é avaliada antes dele. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:1478–1544, 1664–1725, 2744–2773`. | D; se o dia estiver em CxR, aplica-se o ramo `e6_oraculo.py:110`, depois das prioridades. |
| `aad64415` | Pausa total ou parcialmente fora da janela efetiva: somente a interseção é descontada. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:1869–1877, 2750–2763`. | D |
| `871ccd5b` / `2190b3da` / `156aa1a6` | Folga/feriado com previsto zero, sujeito à janela e sem autorização; para folga, exige escala certa. O ramo zera trabalho e intra. | Trabalhadas/intra: `app/ponto/motor_calculo_v2.py:1428–1464`. | D |
| `39668e22` | O bloqueio anterior também publica o par efetivo colapsado, para o calculador não pagar o segmento bloqueado. | Trabalhadas/intra do calculador: `app/ponto/motor_calculo_v2.py:1465–1471`; `app/ponto/calculador/alimentacao.py:114–123`. | D |
| `c6c3785b` | Folga trabalhada sem escala certa: a hora entra em trabalhadas, sem o adicional de 100%. | Trabalhadas mensal e diária: `app/ponto/services/fechamento.py:935–953`; `app/ponto/services/dia_pago.py:257–265`. | D |
| `9592fc83` / `a6b653ad` | Folga trabalhada deixa de perder a intra na lavratura. | Intra diária: `app/ponto/services/dia_pago.py:213–225, 234–263`. | D |
| `438ad986` | Feriado já separado da folga certa não pode voltar a entrar em “sem escala”; elimina a duplicação de trabalhadas e de intra diária. Também passa a perguntar escala certa na competência do dia. | Trabalhadas; intra de DiaPago: `app/ponto/services/fechamento.py:903–910, 943–953`; `app/ponto/services/dia_pago.py:244–263`. | D |

Há ainda mudanças de **materialização**, sem uma nova fórmula geométrica: `67024c7e` passa a gravar os mapas de previstos/realizados em DiaPago; `38f18bbe` cria o retrato que substitui esses minutos a partir da grade, inclusive zerando dias retirados dela. Atualmente, a guarda interrompe esse retrato para a 09 exportada. (`app/ponto/services/dia_pago.py:88–91, 286–290, 449–462, 479–494`)

Alguns commits do anexo precisam ficar fora da atribuição direta aos cinco deltas: [nao verificado]

| Classe | Limite demonstrável |
|---|---|
| O116 / `070c1345` | Muda a distribuição diária pelo mapa `dia_da_jornada`; o comentário registra total mensal invariável no universo medido. (`app/ponto/services/dia_pago.py:109–132, 174, 235–258`) |
| S5b: `ec22b2f1`, `a80bf914`, `abea49b1`, `5b4e5bb8` / `e8e67d9b` | A substituição por rubricas existe, mas a chamada grava versão `oraculo` somente a partir de 10/2026; não demonstra causa da 09 em versão `motor`. (`app/ponto/services/dia_pago.py:310–328`; `app/ponto/services/fechamento.py:1233–1262`) |
| O280 exclusivo de `calculador/periodo.py`, incluindo `f6b16a7e` / `ad299e5c` | O módulo declara ausência de leitores de produção; o motor presente ainda conserva o ramo `duracao > 360`. (`app/ponto/calculador/periodo.py:8–20`; `app/ponto/motor_calculo_v2.py:2755–2773`) |
| `917b8afa` | Altera o pago exibido da folga-feriado pelo detalhe; isso não equivale a escrever `horas_trabalhadas`. (`app/ponto/services/dia_pago.py:252–256, 892–896`) |
| `32ab503c`, `bcf58258`, `861100a1`, `e15125d2` | Nos trechos correspondentes, escrevem furo ou carimbos, não novas fórmulas dos cinco campos. (`app/ponto/services/dia_pago.py:668–671, 523–541, 340–345, 333–334`) |

A causa nominal de **col211, col638, col749, col900 e col926** continua **[nao verificado]**. O registro contém os totais, mas não a decomposição diária necessária para ligar cada diferença a uma das condições acima. (`app/docs/BACKLOG.md:760`)

**3. Ramos de rejulgamento**

| Situação | Decisão |
|---|---|
| Dia com adesão/batidas, impressão igual, julgamento e veredito presentes, diferente de `nunca_bateu` | `_em_dia=True`; sem `forcar`, incrementa `pulados` e não relavra. (`app/ponto/services/cartorio.py:1190–1196`) |
| Mesmo ramo, mas impressão diferente, carimbo/veredito ausente ou veredito `nunca_bateu` | Não satisfaz o pulo; busca o dia da grade e prossegue no julgamento. (`app/ponto/services/cartorio.py:1187–1199`) |
| Ramo sem adesão, `nunca_bateu` | Tem teste próprio: julgamento + impressão igual + veredito; sem `forcar`, também pula. (`app/ponto/services/cartorio.py:1030–1038`) |
| `forcar=True` | Ignora o pulo por impressão, inclusive sem subida de versão. (`app/ponto/services/cartorio.py:774–779, 1194`) |

**Sem subida**, alterações em qualquer parte efetivamente codificada no hash podem provocar rejulgamento: batidas, cobertura, chamados, DNA, veto, teto, fio, cobertura parcial e encerramento do turno. Os minutos calculados e `tipo_dia` da grade não aparecem nesse hash. (`app/ponto/services/cartorio.py:365–421`)

**Só pela subida**, o mecanismo é: `versao_para` escolhe a maior versão cujo horizonte alcance a competência; a versão entra como `regra:N` no hash; o cartório passa esse valor ao teste de impressão. Isso alcança atas que continuariam com hash igual pelos demais insumos. (`app/ponto/services/cartorio.py:223–229, 384–385, 977–980, 1181–1186`)

Na cópia, a declaração é `((1,'2026-09'), (2,'2026-10'), (3,'2026-10'))`: a 09 recebe versão 1. Alterar o horizonte da versão 3 para `'2026-09'` faz a 09 receber 3; **o primeiro número é versão, não empresa**. (`app/ponto/services/cartorio.py:111–113, 223–229`)

Rejulgar a ata não implica escrever o fechamento exportado: a guarda atual da 09 exclui essas empresas da escrita de FechamentoMensal/DiaPago, enquanto a leitura pura permanece permitida. (`app/ponto/services/fechamento.py:27–43, 523–524, 542–559`)

**4. Dono de cada condição**

Para todas as linhas marcadas **D**, esta é a decisão exata, na ordem em que o código executa:

| Primeira condição satisfeita | Dono e ramo |
|---|---|
| `folga_nao_definida` ou `sem_vinculo_no_dia` ou `sem_vinculo_em_dia_de_trabalho` | **CADASTRO** — `app/ponto/management/commands/e6_oraculo.py:98–99` |
| `batida_fora_de_turno` | **BATIDA** — `app/ponto/management/commands/e6_oraculo.py:104–105` |
| Quantidade ímpar de `horas_do_dia` | **BATIDA** — `app/ponto/management/commands/e6_oraculo.py:106–107` |
| `fase_deslocada` | **CADASTRO** — `app/ponto/management/commands/e6_oraculo.py:108–109` |
| `em_cadastro_x_realidade` | **CADASTRO** — `app/ponto/management/commands/e6_oraculo.py:110` |
| Nenhuma das anteriores | **ESTRUTURA** — `app/ponto/management/commands/e6_oraculo.py:110` |

Portanto, **“mudou por correção de código” não basta para classificar como ESTRUTURA**: o nome do commit e o motivo técnico não são argumentos da função; os insumos são carregados por dia. (`app/ponto/management/commands/e6_oraculo.py:53–54, 168–178`)

O E6 compara minutos realizados — ou, na falta deles, `pago_h×60` —, não classifica diretamente um delta mensal de dias previstos ou de intra. Aplicar seu juiz aos cinco colaboradores exige os dias e os respectivos insumos; os donos nominais permanecem **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:246–252, 361–364`; `app/docs/BACKLOG.md:760`)

## 01/11 14:38 · sessao 9c1230f1a8a5 · - · - · origem=Ronald (tmux)

Consulta proposta, sem executar, para cada par `(cid, d)` = `(222, 2026-10-28)`, `(150, 2026-09-11)`, `(304, 2026-08-31)`, `(476, 2026-08-21)`; valores de produção e snapshots anteriores a A/B continuam `[nao verificado]`.

```python
from datetime import date, datetime, time, timedelta
from django.db.models import Q
from django.utils import timezone
from colaboradores.models import Colaborador, Empresa
from escala.models import CelulaDia, EscalaColaborador
from ponto.models import Ausencia, Batida, Justificativa, TipoAusencia
from chamados.models import PerguntaDisputa, DisputaSupervisao, ChamadoColaborador

casos = [(222, date(2026,10,28)), (150, date(2026,9,11)),
         (304, date(2026,8,31)), (476, date(2026,8,21))]

Empresa.objects.filter(pk=3).values("id", "dia_inicio_competencia")
TipoAusencia.objects.filter(ativo=True).values(
    "codigo", "parcial", "fim", "bloqueia_ponto", "efeito")

# Para cada cid,d:
Colaborador.objects.filter(pk=cid, empresa_id=3).values(
    "id", "empresa_id", "situacao", "isento_ponto")

CelulaDia.objects.filter(colaborador_id=cid, data__range=(d-timedelta(days=1), d)).values(
    "id", "data", "trabalha", "escala_geradora_id", "dna", "dna_anterior",
    "julgada_em", "impressao", "veredito", "protestos", "ata",
    "editada_em", "regenerada_em")

Ausencia.objects.filter(colaborador_id=cid).exclude(status="rejeitada").filter(
    Q(data=d) | Q(data__lte=d, data_fim__gte=d)
    | Q(data__lte=d, data_fim__isnull=True, tipo__in=Ausencia.TIPOS_EM_ABERTO)
).order_by("-data", "-pk").values(
    "id", "data", "data_fim", "tipo", "status", "minutos", "hora_ini", "hora_fim",
    "criado_em", "atualizado_em", "aprovada_em")

t0 = timezone.make_aware(datetime.combine(d-timedelta(days=1), time.min))
t1 = timezone.make_aware(datetime.combine(d+timedelta(days=2), time.min))
Batida.objects.filter(colaborador_id=cid, timestamp__gte=t0, timestamp__lt=t1).values(
    "id", "timestamp", "tipo", "is_intervalo", "origem", "aprovada",
    "retratada_em", "criado_em", "pergunta_origem_id")

Justificativa.objects.filter(colaborador_id=cid, data_turno=d).values(
    "id", "status", "criado_em", "atualizado_em", "aprovada_em")

PerguntaDisputa.objects.filter(disputa__colaborador_id=cid, data_evento=d).values(
    "id", "motivo", "celula_id", "disputa_id", "chamado_destino_id",
    "respondida_em", "validada_em", "materializada_em", "via_resolucao",
    "disputa__chamado_id", "disputa__fechada_em", "disputa__motivos",
    "disputa__contexto")

# Sem filtrar data/status: disputa é agregada por colaborador.
DisputaSupervisao.objects.filter(colaborador_id=cid).values(
    "id", "chamado_id", "motivos", "contexto", "aberta_em", "fechada_em")
ChamadoColaborador.objects.filter(colaborador_id=cid).values(
    "id", "celula_id", "categoria", "subcategoria", "modulo_origem",
    "status_local", "contexto_json", "criado_em", "atualizado_em")
```

Referências para o recorte e os campos:

- **Empresa/cadastro:** corte em `app/colaboradores/models.py:46`; situação/isento/empresa em `app/colaboradores/models.py:294` e `:321`. Catálogo lê apenas `TipoAusencia.ativo=True` em `app/ponto/catalogo/ausencias.py:249`; flag parcial integra o catálogo em `:251`, sendo consumida em `:357`.
- **Ausência vencedora:** filtro e ordenação exatos em `app/ponto/turnos.py:2552`; desempate `-data,-pk` em `:2557`. A grade toma a primeira **aprovada**, enquanto a cobrança toma a primeira **não rejeitada**, pelo filtro de efeito em `:2514` e `:2516`; grade chama `para='folha'` em `app/escala/utils.py:1187`.
- **Parcial/período:** tipo vigente + um dia + `0 < minutos < 480` em `app/ponto/turnos.py:2595`; período ausente em `:2632`, virada noturna em `:2636`, inclusão das bordas em `:2639`.
- **Ata/célula:** `trabalha`, geradora e DNA em `app/escala/models.py:1653`, `:1660`, `:1665`; `regenerada_em`, `dna_anterior`, `editada_em` em `:1686`, `:1694`, `:1700`; impressão/julgamento em `:1713`; veredito/protestos/ata em `:1738`, `:1741`, `:1746`. A véspera é relevante ao leitor de faltantes em `app/chamados/services/declaracao_texto.py:81`, que a limita a fim de turno em `:119`.
- **Batida/fato novo:** os campos estão em `app/ponto/models.py:22`, `:26`, `:40`, `:63`, `:67`, `:79`. O recorte `[d−1, d+2)` e as marcas de criação/retração são exatamente os de `app/ponto/services/cartorio.py:292`; ausência, justificativa e edição/regeneração são verificadas em `:297`, `:301`, `:307`. Não eliminar retratadas desta consulta: o próprio ato de retração pode ser fato novo, conforme `:295`.
- **Pergunta/disputa/chamado:** campos de pergunta em `app/chamados/models.py:2194`, `:2201`, `:2203`, `:2213`, `:2300`, `:2651`, `:2659`; disputa em `:1651`, `:1656`, `:1660`, `:1664`, `:1671`; chamado em `:32`, `:38`, `:41`, `:44`, `:46`, `:61`.

Para a cadeia do turno, acrescentar a geradora apontada pela célula e os demais vínculos candidatos, sem substituir a geradora por “escala ativa”: a autoridade da geradora está em `app/escala/alimentacao.py:196`; a inclusão explícita de geradoras fora da janela está em `:123`.

```python
# gid = escala_geradora_id extraído acima.
EscalaColaborador.objects.filter(colaborador_id=cid).filter(
    Q(pk=gid)
    | (Q(data_inicio__lte=d)
       & (Q(data_fim__isnull=True) | Q(data_fim__gte=d))
       & (Q(ativa=True) | Q(data_fim__isnull=False)))
).values()  # Preservar configuração integral do vínculo.
```

Campos de vigência e template: `app/escala/models.py:874`; filtro de candidatura em `app/escala/alimentacao.py:187`. Para template, consultar `TipoEscala` dos `tipo_escala_id` retornados, incluindo sua cadeia de `template_pai`: a herança é lida em `app/escala/models.py:806`.

A impressão é hash dos insumos, sem guardar a lista reversível deles; o carimbo pode ser sobrescrito ao rejulgar (`app/escala/models.py:1706`). Portanto, afirmar quais dias o **controle B efetivamente pulou** requer os mesmos campos em snapshot anterior a B ou instrumentação por célula; o estado atual sozinho não demonstra esse histórico `[nao verificado]`.

## 01/11 14:38 · sessao 28e453433db2 · - · - · origem=Ronald (tmux)

Caminho principal confirmado, com uma distinção importante: o pai `disputa_supervisao` nasce antes da geração da pergunta e antes do eventual c2. (`app/chamados/services/disputa_emissao.py:504`, `:598`, `:605`)

1. `julgar_colab` roda o corpo e, depois da lavra, chama `perguntar_pela_celula` somente para datas presentes em `out['cods']`. (`app/ponto/services/cartorio.py:831`, `:868`, `:878`)
2. O corpo lavra ata/veredito e carimba a célula antes dessa fábrica. (`app/ponto/services/cartorio.py:1653`, `:1655`, `:1675`)
3. `motivos_a_perguntar` exige dia que o juiz não cala, colaborador ativo, data não futura, marco vencido, veredito acusador quando preenchido e motivo ainda não perguntado naquele dia, excluídas duplicatas. (`app/chamados/services/fabrica_da_celula.py:80`, `:82`, `:87`, `:88`, `:103`, `:111`)
4. `motivos_pela_ata` chama `_ata_do_dia`, que lê `_marcos_faltantes`; esta consulta `grade_da_celula`, aceita ausência parcial e seleciona somente células `missing`. (`app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/disputa_emissao.py:827`, `app/chamados/services/declaracao_texto.py:82`, `:98`, `:110`)
5. A posição é derivada da ordem dos pares: primeiro=`inicio`; último=`fim`; intermediária S=`int_ida`. O mapa transforma `fim/S` em `orfao_14h` e `int_ida/S` em `intervalo_saida`. Não há limiar de 14 horas nesse ramo da fábrica pela ata. (`app/chamados/services/declaracao_texto.py:113`, `:115`, `:118`, `:42`)
6. `perguntar_pela_celula` silencia push e chama `perguntar`; esta passa um motivo por vez com `{motivo: {'data': dia.isoformat()}}` para `criar_ou_atualizar_disputa`. (`app/chamados/services/fabrica_da_celula.py:159`, `:160`, `:120`, `:124`)
7. `criar_ou_atualizar_disputa` define `subcategoria=motivos_lista[0]` e entra no núcleo. Havendo disputa aberta, agrega; sem ela, chama `ChamadoColaborador.abrir` com categoria e módulo `disputa_supervisao`, subcategoria recebida e contexto do nascimento. O INSERT fica no `get_or_create`. (`app/chamados/services/disputa_emissao.py:598`, `:601`, `:467`, `:471`, `:504`, `:514`, `:516`, `app/chamados/models.py:930`)
8. Só depois o fluxo gera perguntas, usando diretamente `PerguntaDisputa.abrir`; se a pergunta nasce sem destino, tenta `reatar_pergunta_orfa`, que pode emitir `batida_ausente` pelo c2. (`app/chamados/services/disputa_emissao.py:605`, `:606`, `:389`, `:404`, `:409`)

Pontos cruciais para não atribuir causalidade ao ramo errado:

- `_coberto_por_ausencia` **não é veto direto do fluxo acima**: `gerar_perguntas_disputa` vai diretamente a `PerguntaDisputa.abrir`. A chamada desse helper em `disputa_emissao` está em `_criar_pergunta_se_couber`, usado pelo agrupador/complementador de chamados órfãos. (`app/chamados/services/disputa_emissao.py:389`, `:756`, `:763`, `:813`, `:847`, `:956`, `:1012`, `:1043`)
- O c2 pergunta à versão nova de `_coberto_por_ausencia` e aplica `parcial_so_lista`; ele trata a emissão do chamado do dia depois que o pai de disputa já pode existir. (`app/chamados/reconciliador.py:767`, `:828`, `:848`, `:851`, `app/chamados/services/disputa_emissao.py:504`, `:605`)
- `parcial_so_lista` também veta os portões do cartório para `batida_ausente`; a fábrica `perguntar_pela_celula` não consulta esse predicado. Portanto “lista sem chamado” nesses portões não prova ausência de pai `disputa_supervisao`. (`app/ponto/services/cartorio.py:1575`, `:1630`, `app/chamados/services/fabrica_da_celula.py:146`, `:153`, `:158`)
- Os gates genéricos de absolvição e régua de `ChamadoColaborador.abrir` não incluem o módulo `disputa_supervisao`: o primeiro exige `MODULOS_DE_TURNO`; o segundo exige a união deste vocabulário com `FURO_MODULES`. (`app/chamados/models.py:921`, `:926`, `:1061`, `:1085`, `app/chamados/reconciliador.py:336`, `app/ponto/services/furos_vetados.py:79`, `:81`, `app/chamados/catalogo/furo_modulos.py:15`)

Alterações diretamente relevantes à fábrica: `dia_sem_trabalho` agora deixa passar `ata.tipo_dia='ausencia'` com `cobertura_parcial` quando `trabalha is not False`; a ata agora carrega esse carimbo; `_marcos_faltantes` agora aceita o dia parcial. (`app/chamados/juizes.py:408`, `:424`, `:425`, `app/ponto/services/cartorio.py:640`, `app/chamados/services/declaracao_texto.py:97`)

## 01/11 14:38 · sessao 850f1832e7f7 · - · - · origem=Ronald (tmux)

- Na cura, `REGRA_VERSOES` inclui `(3,'2026-09')`; a linha anterior fornecida no diff contém somente versões 1/09 e 2/10. Logo o **controle nativo não declara v3**; chamar sua passada de “B (bump v3)” não demonstra que o instrumento substituiu essa constante. Essa substituição não consta dos anexos fornecidos: `[nao verificado]`. Referência viva: `app/ponto/services/cartorio.py:111`; referência do controle: linha removida do hunk `@@ -108,7 +108,7 @@` do diff fornecido.

- `versao_para()` obtém a competência usando a empresa e escolhe a maior versão cujo horizonte a alcance: `app/ponto/services/cartorio.py:203–209`. Portanto comp09 recebe 1 na declaração do controle e 3 na declaração da cura; comp10 recebe 2 e 3, respectivamente, pela mesma regra (`app/ponto/services/cartorio.py:208–209` + diff fornecido da constante).

- A versão entra no hash como `regra:N` (`app/ponto/services/cartorio.py:393–394`), e o ramo normal pula somente quando há julgamento/veredito e a impressão coincide, salvo `forcar` (`app/ponto/services/cartorio.py:1214–1220`). O ramo sem adesão tem condição análoga (`app/ponto/services/cartorio.py:1054–1062`). Assim, **injetar v3 também no controle** faria o mesmo rompimento da igualdade para uma célula comp09 antes gravada sob regra1. A efetiva injeção na passada B e a impressão anterior de cada um dos três casos são `[nao verificado]`.

- A fábrica só é chamada para datas presentes em `out['cods']`, isto é, julgadas no corpo (`app/ponto/services/cartorio.py:1281–1282`, `app/ponto/services/cartorio.py:864–880`). Dessa forma, não emitir no controle pode resultar de não chegar à fábrica por impressão igual, sem demonstrar mudança de predicado/veto de emissão (`app/ponto/services/cartorio.py:1214–1220`, `app/ponto/services/cartorio.py:878–880`).

- O campo de produção que confirma a competência é `colaboradores_empresa.dia_inicio_competencia`, declarado em `app/colaboradores/models.py:46`. A janela usa esse corte (`app/ponto/janelas.py:36–51`, `app/ponto/janelas.py:60–63`), com a decisão mês atual/seguinte em `app/ponto/janelas.py:24–33`. **Se corte=21**, os três dias 2026-08-21, 2026-08-31 e 2026-09-11 pertencem à comp09 por esse cálculo; o valor efetivo da empresa 3 é `[nao verificado]`.

Consulta mínima sem executar, para a parte competência e entrada:

```sql
SELECT id, dia_inicio_competencia
FROM colaboradores_empresa
WHERE id = 3;

SELECT colaborador_id, data, julgada_em, impressao, veredito, protestos,
       trabalha, dna, ata
FROM escala_celuladia
WHERE (colaborador_id = 150 AND data = DATE '2026-09-11')
   OR (colaborador_id = 304 AND data = DATE '2026-08-31')
   OR (colaborador_id = 476 AND data = DATE '2026-08-21')
   OR (colaborador_id = 222 AND data = DATE '2026-10-28');
```

A impressão atual da produção sozinha não recupera a impressão anterior às passadas da sombra; esse histórico é `[nao verificado]`.

## 01/11 14:36 · sessao 028bff1ec590 · - · - · origem=Ronald (tmux)

Inventário estático consolidado; sem histórico Git, o hunk exato de cada hash é [nao verificado]. As condições abaixo têm evidência no código atual e comentários/testes que registram o comportamento anterior. [nao verificado]

| Commits do anexo | Condição de mudança, com mesmos cadastro/batidas | Campos alcançados e referência | [nao verificado]
|---|---|---|
| `212971ad` | Uma FolgaDia no mês fazia todo dia fora da foto virar trabalho, apagando folga semanal; hoje o ciclo continua respondendo fora da foto. Não reescreve células existentes. | Previsto potencial; `app/escala/models.py:1270-1292`; limite de materialização `app/docs/TICKETS.md:319`; célula existente vence cálculo `app/escala/models.py:1071-1073`. |
| `a5cd3d39` | Foto parcial em ciclo sem fase no template: antes fora da foto era trabalho; hoje usa paridade da folga mais próxima, no passado ou futuro. | Previsto potencial; `app/escala/models.py:1294-1335`; depende de célula existente pelo mesmo limite acima. |
| `10832b45` / `a2e903e1`, `96beb803` | DNA/grade com duas pausas: a sequência agora inclui todas, em vez de quatro marcos. Grade soma os trechos E→S dessa sequência. | `minutos_previstos`, potencialmente realizado pela geometria; `app/escala/models.py:1141-1181`; `app/escala/utils.py:1560-1590`. |
| `fecb9a67` | Juiz de previsto descontava só uma pausa; agora desconta todas. Exemplo documentado 635→550. É distinto do escritor do previsto na grade. | Previsto usado pelo motor; `app/escala/utils.py:2224-2268`; ligação documental ao hash `app/docs/TICKETS.md:243`. |
| `801253ed` | Ata legível com duas pausas: antes somente a primeira chegava ao motor; agora ambas chegam, preservando segundos das Batidas. | Trabalhadas/intra; `app/ponto/motor_calculo_v2.py:694-720`; antes/depois `app/ponto/tests/test_o119_intra_de_todas_as_pausas.py:11-30`. |
| `107e3bb5` | Papel das batidas muda durante montagem; antes realizado consultava ata anterior, hoje recebe ata desta montagem, depois dos overlays. Exemplo documental intermitente invertido 900/894→541/545. | Realizado; `app/escala/utils.py:1802-1847`, gravação `:1994-2007`; intermitente também copia realizado para previsto em `:2017-2019`. |
| `14ec97c2` / `825bb46a` | Vínculos concorrentes ou célula cuja geradora está fora da vigência: grade deixa seleção própria por ativa/vigência e pergunta à geradora. Geradora ausente/fora vigência devolve None. | Dias/previsto/realizado e régua dependentes do vínculo; `app/escala/utils.py:1250-1263`; `app/escala/alimentacao.py:237-254`. |
| `8ec526f2` / `9f0e7542` / `60258b80` | Folga de calendário de outro vínculo deixa de prevalecer; folga manual continua valendo para a pessoa. `60258b80` é documentação/prova segundo o próprio relato. | Overlay muda `tipo_dia`, depois recorte altera dias/previsto/realizado; `app/ponto/precedencia.py:222-239`; `app/escala/utils.py:1103-1128`; `app/ponto/services/dia_pago.py:385-399`; `app/ponto/services/fechamento.py:1017`; natureza de `60258b80` em `app/docs/RELATO.md:5763`. |
| `879ff834` | Dia de ausência com trabalho: antes realizado não entrava; hoje entra o coberto, com cap, e abono cobre apenas o restante. | `minutos_realizados`; `app/ponto/services/dia_pago.py:373-399`. |
| `aad64415` | Pausa parcialmente/totalmente fora do par efetivo recortado pela janela HE: desconta somente interseção. | Trabalhadas/intra; `app/ponto/motor_calculo_v2.py:1869-1877`, `:2750-2763`, `:3210-3219`; alimentação `app/ponto/calculador/alimentacao.py:138-155`. |
| `c7b8bfdb` | Existem pausas marcadas, mas todas abaixo do piso; guarda antiga impedia fallback para janela fixa; atual consulta intervalos efetivos. | Realizado; `app/ponto/turnos.py:742-762`. |
| `a5ae5013` / `5d9ec893` / `185b9af0` | Forma posicional cuja saída está em outro turno: junta primeira entrada à última saída e subtrai pausa. | Realizado; `app/ponto/turnos.py:720-730`. |
| `7b8329a1` | Duas luzes de intervalo da ata declaram pausa acima do teto; declaração tem prioridade sobre casamento adjacente. | Trabalhadas/realizado/intra pela geometria; `app/ponto/turnos.py:802-814`, `:1805-1824`; teto só na deduzida `:1777-1784`. |
| `912ab728` | Uma perna da pausa sobra e outra acendeu intervalo: até teto o dia passa, em vez de inteiro descartado. | Trabalhadas/realizado/intra pela geometria; `app/ponto/turnos.py:1755-1765`. |
| `3d037f1b` | Duas células reivindicam mesma batida; posterior rejeitada. | Trabalhadas/realizado/intra pela geometria; `app/ponto/turnos.py:1786-1801`. |
| `0728ce98` | Substituição de períodos pelo marco remove dia da jornada, preservando cauda da véspera cuja entrada tem data civil do dia seguinte. | Trabalhadas/intra; `app/ponto/motor_calculo_v2.py:679-688`. |
| `871ccd5b` / `2190b3da` / `156aa1a6` | Folga/feriado previsto zero, empresa dentro janela HE, sem autorização; folga exige escala certa. Zera trabalho e intra. Inverte O263-FT. | Trabalhadas/intra; condições `app/ponto/motor_calculo_v2.py:1428-1458`, zero `:1459-1464`, inversão `:1419-1426`. |
| `39668e22` | Mesmo bloqueio publica par colapsado para calculador também não pagar. | Trabalhadas/intra; `app/ponto/motor_calculo_v2.py:1465-1471`; leitor `app/ponto/calculador/alimentacao.py:114-123`. |
| `c6c3785b` | Folga sem escala certa deixa de perder hora, entra em trabalhadas sem adicional 100%. | Trabalhadas; `app/ponto/calculador/regras.py:687-691`. |
| `9942ee91` | Dia sem previsto usa bruto factual no piso de intra, incluindo intra do template no máximo. | Intra; `app/ponto/calculador/regras.py:742-763`. |

Exclusões importantes:

- `f6b16a7e` / `ad299e5c` e demais alterações exclusivas de `calculador/periodo.py` não demonstram mudança no caminho de produção: o módulo declara zero importadores de produção em `app/ponto/calculador/periodo.py:8-20`; motor atual conserva `duracao > 360` em `app/ponto/motor_calculo_v2.py:2755-2773`, e alimentação só quebra pausa se motor descontou em `app/ponto/calculador/alimentacao.py:124-137`.
- `d9a32f55` é monetariamente neutro por invariante: só recusa é revogável, enquanto pagamento consulta autorização; `app/ponto/motor_calculo_v2.py:1396-1406`.
- `5ece3a86` corrige HE dobrada, sem provar mudança direta nas cinco rubricas solicitadas; `app/ponto/motor_calculo_v2.py:1753-1784`; `app/ponto/calculador/regras.py:767-769`.
- Causa nominal dos deltas de col211/638/749/900/926 permanece [nao verificado]. O registro dos deltas existe em `app/docs/BACKLOG.md:760`; controle/cura iguais e movimento vindo do main+subida estão em `app/docs/RELATO.md:5778`.
- col211 +1,02 histórico foi aplicado na R2b, mas não prova a causa do −1,02 atual: `app/docs/RELATO-ARQUIVO.md:5402-5423`. col926 08/09 figura na R2 histórica, sem provar ser o dia atual: `app/docs/RELATO-ARQUIVO.md:6000`.

## 01/11 14:36 · sessao 7fab1430d323 · - · - · origem=Ronald (tmux)

| Commit/classe | Efeito no código atual e condição | [nao verificado]
|---|---|
| `67024c7e` | Passa a lavrar `minutos_previstos` e `minutos_realizados` dos mapas diários: declaração em `app/ponto/services/dia_pago.py:39`, histórico explícito em `:88`, escrita em `:286`. Não altera a fórmula do motor; preenche campos antes ausentes. |
| `38f18bbe` | `retratar_exportada` lê a grade do fechamento gravado, deriva os mapas (`app/ponto/services/dia_pago.py:459`) e substitui previstos/realizados (`:479`); se o dia saiu da grade, zera os três minutos (`:486`). Em HEAD, a guarda barra a 09 exportada antes desse caminho (`:449`). |
| `879ff834` | Ausência com batidas passa a carregar realizado `min(realizado,previsto)` e abono só do saldo descoberto (`app/ponto/services/dia_pago.py:391`). Para mesmo cadastro/batida, muda realizados quando a ausência tem cobertura real positiva; previstos permanecem no mesmo universo trabalho/ausência (`:385`). |
| `9592fc83` / `a6b653ad` | Folga trabalhada carrega noturno e intra, com a intra adicionada em `_rubricas_da_folga` (`app/ponto/services/dia_pago.py:223`), chamado pelos três ramos de folga (`:234`, `:244`, `:257`). Sem escala certa, a hora entra em `horas_trabalhadas` (`:262`); condição é período classificado em folga. A história das duas curas/reversão está em `:216`. |
| O116 / `070c1345` | Atribuição diária usa `dia_da_jornada`, em vez de sempre a data civil da entrada (`app/ponto/services/dia_pago.py:128`); os ramos normal e folga passam o mapa (`:174`, `:235`, `:245`, `:258`). Altera a linha diária quando entrada civil e jornada discordam; o comentário documenta total mensal invariável no universo medido (`:117`). |
| S5b `ec22b2f1`, `a80bf914`, `abea49b1`, `5b4e5bb8`/`e8e67d9b` | `rubricas` substituem o valor de cada campo declarado (`app/ponto/services/dia_pago.py:310`, `:328`). Porém o caminho está condicionado a competência ≥10/2026 e grava versão `oraculo` (`app/ponto/services/fechamento.py:1233`, `:1262`); não é explicação da 09 na versão `motor`. |
| `917b8afa` | Registra `folga_feriado_min` no detalhe (`app/ponto/services/dia_pago.py:252`) e soma esse detalhe ao pago exibido (`:895`). Muda `pago_h`/apresentação do feriado, sem nova atribuição de `horas_trabalhadas` persistida nesse trecho (`:896`). |

Exclusões relevantes do censo dos cinco campos:

- `32ab503c`: terceira porta escreve somente `furo_apurado` e `lavrado_em` (`app/ponto/services/dia_pago.py:668`).
- `bcf58258`: faz o carimbo acompanhar a escrita por `bulk_update`/`save` (`app/ponto/services/dia_pago.py:523`).
- `861100a1`: leva a impressão da régua para `versao_motor` da linha (`app/ponto/services/dia_pago.py:340`).
- `e15125d2`: campo de autoria `evento_origem`, copiado no conjunto comum (`app/ponto/services/dia_pago.py:333`).
- `b600ea56`, `2652ec8d`, `6c05f6cf`: troca de leitores; extrato consulta somas (`app/relatorios/views.py:482`), calendário lê linhas/rubricas (`app/colaboradores/services/calendario.py:370`, `:787`), espelho/papel lê helpers (`app/ponto/services/espelho.py:417`). Esses trechos não escrevem os cinco campos.

A associação exata de cada hash a um diff histórico não pode ser conferida sem `.git`; os hashes acima são associados pelos assuntos do anexo e pelos históricos/comentários atuais [nao verificado].

## 01/11 14:36 · sessao e023e2c8e912 · - · - · origem=Ronald (tmux)

Mapa estático dos grupos de mudança; vínculo exato de cada hunk a um hash depende de histórico Git, ausente nesta cópia [nao verificado].

| Commits do anexo | Campo/condição que muda | Evidência atual | [nao verificado]
|---|---|---|
| `801253ed` O119 | `horas_trabalhadas`, intra descontada/indenizada: ata legível com duas pausas, antes só a primeira chegava ao motor; agora ambas chegam com segundos das Batidas | `app/ponto/motor_calculo_v2.py:694-720`; teste descreve antes/depois em `app/ponto/tests/test_o119_intra_de_todas_as_pausas.py:11-30` |
| `aad64415` O272 | `horas_trabalhadas`, intra: intervalo parcialmente/totalmente fora do par efetivo da janela HE; só interseção é descontada | `app/ponto/motor_calculo_v2.py:1869-1877`, chamada antes de medir/descontar em `:2750-2763` e `:3210-3219`; causa/documentação em `app/ponto/calculador/alimentacao.py:138-155` |
| `c7b8bfdb` O232 A | `minutos_realizados`: existem pares marcados, mas todos abaixo do piso; guarda antiga via marca bruta impedia descontar janela fixa declarada. Guarda atual lê `intervalos_do_turno` e, vazia, usa janela declarada | `app/ponto/turnos.py:742-762` |
| `a5ae5013` O232 B / `5d9ec893`,`185b9af0` L115 | `minutos_realizados`: forma posicional cuja saída está em outro turno; junta primeira entrada à última saída e tira pausa batida | `app/ponto/turnos.py:720-730` |
| `7b8329a1` R2b | Trabalhadas/realizados/intra via geometria: as duas luzes de intervalo da ata declaram pausa mesmo acima teto; pausa declarada tem prioridade sobre casal guloso adjacente | `app/ponto/turnos.py:802-814`, `:1805-1824`; teto só continua para pausa deduzida em `:1777-1784` |
| `912ab728` R2 | Trabalhadas/realizados/intra via geometria: uma perna da pausa sobra e a outra acendeu intervalo; até teto, dia passa em vez de ser descartado | `app/ponto/turnos.py:1755-1765` |
| `3d037f1b` R3 | Trabalhadas/realizados/intra via geometria: duas células reivindicam a mesma batida, sobretudo cross midnight; posterior é rejeitada | `app/ponto/turnos.py:1786-1801` |
| `0728ce98` | Trabalhadas/intra: substituição de períodos pelo marco passa a remover os do dia da jornada, preservando cauda da véspera cuja entrada tem data civil do dia seguinte | `app/ponto/motor_calculo_v2.py:679-688` |
| `871ccd5b`,`2190b3da`,`156aa1a6` | Trabalhadas/intra: folga/feriado com previsto zero, empresa dentro janela HE, sem autorização; folga requer escala certa. Zera minutos trabalhados e intra indenizada | Condições `app/ponto/motor_calculo_v2.py:1428-1458`, zeramento `:1459-1464`; comentário declara inversão de `8c851906` O263-FT em `:1410-1426` |
| `39668e22` | Trabalhadas/intra do oráculo: caso bloqueado acima publica par colapsado, impedindo calculador de pagar o que motor zerou | `app/ponto/motor_calculo_v2.py:1465-1471`, leitor `app/ponto/calculador/alimentacao.py:114-123` |
| `c6c3785b` | Trabalhadas de folga sem escala certa deixam de sumir; hora recebe rubrica `horas_trabalhadas` sem adicional de 100% | `app/ponto/calculador/regras.py:687-691`; verificar correspondente escritor motor em `dia_pago.py` com pai |
| `9942ee91` | Intra oráculo em dia sem previsto: piso usa duração bruta factual; máximo inclui intra do template além da declarada no dia | `app/ponto/calculador/regras.py:742-763` |
| `f735dab4` | O efeito nominal “zero hora negativa” precisa ser rastreado pelo pai no núcleo; atribuição exata por hash não disponível [nao verificado] |
| `f6b16a7e`,`ad299e5c` | **Não inferir efeito em produção:** pertencem a `calculador/periodo.py`, cujo cabeçalho declara módulo ao lado sem leitor produção; família nova soma pares fechados sem portão para descontar pausas | `app/ponto/calculador/periodo.py:8-20`, `:34-59`, `:340-362`. Motor atual conserva portão `duracao > 360` no 12x36 em `app/ponto/motor_calculo_v2.py:2755-2773`; alimentação atual só quebra pausas quando motor descontou em `app/ponto/calculador/alimentacao.py:124-137` |
| `d9a32f55` O273 | **Sem mudança monetária por invariante:** decisões revogáveis são recusas, nunca autorizações; consulta só paga autorização | `app/ponto/motor_calculo_v2.py:1396-1406` |
| `5ece3a86` | Cura HE/intra dobrada altera **HE**, não por si as cinco rubricas solicitadas; devolve ao limite apenas parcela suprimida que previsto retirou | `app/ponto/motor_calculo_v2.py:1753-1784`, `app/ponto/calculador/regras.py:767-769` |

Para os casos nominais, `col211 +1,02 h` foi historicamente aplicado na R2b, mas isso **não prova** a causa do `-1,02/-61` atual: `app/docs/RELATO-ARQUIVO.md:5402-5423`; atribuição atual [nao verificado]. `col926 08/09` consta entre os sete exemplos históricos da R2, mas isso **não prova** que seja o dia do delta atual: `app/docs/RELATO-ARQUIVO.md:6000`; atribuição atual [nao verificado].

Os documentos mais recentes dizem explicitamente que o movimento dos cinco é do main + rejulgamento/subida, e que o col211 aparece também no controle: `app/docs/RELATO.md:5778`. A rodada forçada registrou B−A zero e os mesmos cinco em F0×A: `app/docs/RELATO.md:5758`. Esses registros não mostram as batidas/cadastro por dia necessários para escolher, entre os ramos acima, o causador nominal [nao verificado].

## 01/11 14:34 · sessao 7fab1430d323 · - · - · origem=Ronald (tmux)

`dono_da_divergencia` decide nesta ordem, portanto “mudou por commit” não basta para concluir ESTRUTURA: [nao verificado]

| Condição | Dono | Ramo |
|---|---|---|
| `folga_nao_definida` ou `sem_vinculo_no_dia` ou `sem_vinculo_em_dia_de_trabalho` | CADASTRO, mesmo com batidas ímpares | `app/ponto/management/commands/e6_oraculo.py:98` |
| `batida_fora_de_turno` | BATIDA | `app/ponto/management/commands/e6_oraculo.py:104` |
| Quantidade de `horas_do_dia` ímpar | BATIDA | `app/ponto/management/commands/e6_oraculo.py:106` |
| `fase_deslocada`, após passar pelos anteriores | CADASTRO | `app/ponto/management/commands/e6_oraculo.py:108` |
| Dia em `dias_cadastro_x_realidade` | CADASTRO | `app/ponto/management/commands/e6_oraculo.py:110` |
| Resíduo | ESTRUTURA | `app/ponto/management/commands/e6_oraculo.py:110` |

Os insumos são montados por `InsumosDoDono.dono`: consulta o aberto real, fase lavrada e cadastro, depois a lista CxR se necessário (`app/ponto/management/commands/e6_oraculo.py:168`). A lista CxR vem de `resultado.dias_cadastro_x_realidade` do espelho/motor (`app/ponto/management/commands/e6_oraculo.py:150`). “Batida fora” exige realizado aberto, entrada sem saída no dia e entrada existente em `batidas_apuraveis` (`app/ponto/management/commands/e6_oraculo.py:190`).

Sem vínculo resulta do carregador `vinculo_do_dia_carregado`; quando não encontra vínculo, `_insumos_do_vinculo` marca `sem_vinculo_no_dia=True` (`app/ponto/supra_juiz.py:172`, `app/ponto/supra_juiz.py:193`). Folga indefinida vem de `ec.defeito_folga_nao_definida(data)` (`app/ponto/supra_juiz.py:200`).

O E6 compara minutos do dia: prefere `minutos_realizados`; na ausência usa `pago_h*60`; sem ambos não compara (`app/ponto/management/commands/e6_oraculo.py:246`). O comando ignora isenção/ausência e diferenças de até dez minutos antes de pedir o dono (`app/ponto/management/commands/e6_oraculo.py:322`, `app/ponto/management/commands/e6_oraculo.py:348`, `app/ponto/management/commands/e6_oraculo.py:361`). Logo, a classificação de diferenças mensais de previstos ou intra, em si, não é uma decisão que esse comando execute; aplicar a função aos dias envolvidos exige os insumos desses dias [nao verificado].

O dado exato do despacho está reproduzido em `app/docs/BACKLOG.md:760`; o controle documentado diz main `9483bbce` igual à cura `113fd774` nos cinco colaboradores, e que os 16 externos não reproduziram (`app/docs/BACKLOG.md:759`). Isso documenta os deltas, mas não fornece paridade/CxR/fase/vínculo por dia; dono nominal dos cinco permanece [nao verificado].

Há duas pistas documentais adicionais, sem prova de causalidade suficiente:

- GRADE-FOLGA nomeia col638 05/12/20-09, col749 06-09 e col900 03-09 como dias da grade viva que passaram de folga para trabalho, mantendo ata antiga folga sem bump (`app/docs/RELATO.md:84`).
- Col211 teve `horas_trabalhadas` 149,04→150,06 no apply R2b de 27/09, contexto de aplicação e tabela (`app/docs/RELATO-ARQUIVO.md:5402`, `app/docs/RELATO-ARQUIVO.md:5423`). Identificar esse evento como causa do atual −1,02 seria [nao verificado].

## 01/11 14:13 · sessao 5d0afc1570dd · CONTENCAO-QUE-MORDE-V2 · censo · origem=Code

Uso `raiz/` para as três primeiras voltas e `v4/` para as três seguintes. São identificadores dos anexos colados: linha 1 é a primeira após `== anexo ==`, contando linhas vazias.

**Os itens 1 e 7 exigem ressalva:** no primeiro, houve detecção seguida de cura declarada; no segundo, falta descarte explícito do risco na triagem. As classes atribuídas abaixo ficam condicionadas a essas limitações (`raiz/codex_v1.r:38`; `raiz/codex_v1_triagem.txt:2`; `v4/codex_v1.r:80`; `v4/codex_v3_triagem.md:5`).

1. **Certificado fabricável pelo Code — (a), na reconferência da cura; enquadramento global [nao verificado].**

   Pergunta: “a cura no delta fecha o caminho que o achado descrevia?” (`raiz/codex_v2.q:11`). Resposta: “Fecha o certificado reduzido a árvore/rc: leitor exige também Ran e OK” (`raiz/codex_v2.r:35`).

   **O Codex havia visto exatamente a fabricação:** “um log sintético […] emite certificado” e “A guarda não protege essa emissão nem Write direto” (`raiz/codex_v1.r:38`). A triagem declarou **#13 curado**, não descartado (`raiz/codex_v1_triagem.txt:2`). Portanto, (a) descreve somente o residual perdido na reconferência; dizer que o Codex nunca viu seria incorreto. O caminho permanece na cópia (`bin/disjuntor.py:1294`; `bin/disjuntor.py:2496`).

2. **Prova ponta a ponta com suite real — (c).** [nao verificado]

   Pergunta: “cada mutante tira de fato a costura […]? […] caso que passaria com a cláusula removida?” (`raiz/codex_v1.q:16–19`). Resposta: “Testa emissão pela CLI com log sintético; retirar a chamada de certificação de suite.sh não é mordido” (`raiz/codex_v1.r:83`).

   A parcela estática **foi vista**; classifico em (c) pela exigência de execução real e prova ponta a ponta. A resposta declarou a execução integral **[nao verificado]** (`raiz/codex_v2.r:3`). O grep e o `|| true` são legíveis na cópia, portanto essas parcelas isoladas não ficam fora do alcance da leitura (`bin/tests/test_disjuntor.sh:1715`; `bin/suite.sh:112`).

3. **Regex do veredito derivada em paralelo — (a).**

   Pergunta: checklist contendo “derivação paralela de fonte única” (`raiz/codex_v1.q:20–22`). **Resposta específica sobre a duplicação entre `cert_suite` e `_cert_ok`: ausente.**

   A duplicação está nos reconhecedores do emissor e do leitor (`bin/disjuntor.py:1307`; `bin/disjuntor.py:1309`; `bin/disjuntor.py:1337`). A lei citada pelo opus trata do veredito pelo rc do processo (`app/docs/LEIS.md:101`). O tema estava perguntado; não há identificação específica desse achado nas respostas fornecidas.

4. **H0 antes da espera; A→B→A — (b).**

   Pergunta: caminhos indevidos envolvendo corrida e checklist de ordem de trava (`raiz/codex_v1.q:9–11`; `raiz/codex_v1.q:20–22`). Resposta: “H0 é calculado antes da trava […] A→B→A pode certificar A após testar B” (`raiz/codex_v1.r:42`).

   Triagem: “#14b A->B->A durante a suite: fila” (`raiz/codex_v1_triagem.txt:8`). **Motivo registrado: somente “fila”; justificativa adicional ausente nessa linha.**

5. **Harness aceita qualquer rc não zero — (b).**

   Pergunta: verificar se os mutantes retiram a costura anunciada (`raiz/codex_v1.q:16–19`). Resposta: “O harness aceita qualquer falha do caso como morte do mutante” (`raiz/codex_v1.r:89`).

   Triagem inicial: “harness que aceita qualquer rc nao zero: fila” (`raiz/codex_v2_triagem.txt:8`). Motivo posteriormente explícito: “já existia, não é desta volta” (`v4/codex_v2_triagem.md:8`). O comportamento permanece na cópia (`bin/tests/test_disjuntor.sh:2282`).

6. **Contrato sem relação com o diff; `--no-verify` — (e).**

   **Pergunta específica sobre pertinência semântica obra↔diff ou sobre `--no-verify`: ausente. Resposta específica: ausente.**

   A pergunta próxima cobra ordem do portão perante `SKIP_TESTS` e saída antecipada **do pre-push**; não pergunta por essas duas garantias (`raiz/codex_v1.q:23`). A implementação verifica identificação da obra e existência do contrato, sem julgamento de sua relação com o diff (`bin/contrato_do_push.py:76–83`). O bypass por `--no-verify` consta do achado fornecido (`opus_final.md:7`).

7. **SessionStart remove reservas de outra sessão viva — (b) provisório: [nao verificado].**

   Pergunta: “A poda […] que caso concreto isso erra? Qual chamador grava a reserva e qual a apaga […]?” (`v4/codex_v1.q:14`). Resposta: “Com sessão preenchida, também se remove toda outra sessão sem verificar se continua viva” (`v4/codex_v1.r:80`).

   **Linha de triagem descartando especificamente esse risco: ausente.** A triagem encaminha a cura de `poda_seq` “junto com o aval CONTENCAO-SESSOES”, sem explicitar o destino daquele risco (`v4/codex_v3_triagem.md:5`). O código assume “UMA sessão […] viva por desenho” e elimina reservas das demais (`bin/disjuntor.py:1691`; `bin/disjuntor.py:1706`). Isso sustenta aceitação do desenho, mas não comprova o descarte exigido literalmente por (b).

8. **Idempotência limitada às últimas 500 autorizações — (b).**

   Pergunta: escritores e leitores de `est['autorizados']` (`raiz/codex_v2.q:16–18`). Resposta: “idempotência termina após 500 autorizações”; entradas antigas deixam de ser reconhecidas (`raiz/codex_v2.r:17–18`).

   Triagem: “idempotência limitada a 500 autorizações: fila”, por “pergunta de desenho: TTL/teto de autorizados” (`raiz/codex_v2_triagem.txt:6`). O corte permanece na cópia (`bin/disjuntor.py:1658`).

| Achado | Classe |
|---|---|
| 1 | (a)\* |
| 2 | (c) |
| 3 | (a) |
| 4 | (b) |
| 5 | (b) |
| 6 | (e) |
| 7 | (b)\* |
| 8 | (b) |

| Classe | Contagem |
|---|---:|
| (a) | 2 — inclui o item 1 condicional |
| (b) | 4 — inclui o item 7 provisório |
| (c) | 1 |
| (d) | 0 |
| (e) | 1 |
| **Total** | **8** |

\* **Não são dois enquadramentos estritos comprovados:** o item 1 foi visto e declarado curado; no item 7, o descarte específico não está documentado (`raiz/codex_v1.r:38`; `raiz/codex_v1_triagem.txt:2`; `v4/codex_v3_triagem.md:5`).

## 01/11 14:11 · sessao 7330d4c01221 · - · - · origem=Ronald (tmux)

- `v4/codex_v1.q:14`: pergunta 5.
- `v4/codex_v1.r:80`: bullet de `clear`, incluindo “Com sessão preenchida, também se remove toda outra sessão sem verificar se continua viva”.

Contagem virtual literal: primeira linha do anexo = 1, linhas vazias incluídas, quebras visuais ignoradas.

## 01/11 14:10 · sessao ee5fc7f037ed · BUG-ENC-CELULAS-FORMA · censo · origem=Code

**Há incompatibilidade real, mas a descrição da fila mistura dois efeitos:** o mapa composto perde a **geradora na escolha do vínculo dos segmentos**; ele **não esconde, por si só, as células do leitor do papel da ata**. O cartório alcança esse repasse; a carga em lote converte as chaves corretamente. (`app/ponto/turnos.py:2148`, `app/ponto/turnos.py:1954`, `app/escala/alimentacao.py:237`, `app/ponto/turnos.py:1629`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:3005`)

**1. Formatos, acessos e efeito**

| Etapa/consumidor | Formato e acesso efetivo |
|---|---|
| `celulas_do_periodo` | Produz `{(colaborador_id, data): CelulaDia}`, filtrado pelo intervalo inclusivo. (`app/escala/alimentacao.py:41`) |
| `dias_encerrados` | Recebe esse mapa composto; quando omitido, deriva-o pelo carregador. Não devolve células: devolve `{data: bool}` ou `{data: motivo\|None}`. (`app/ponto/turnos.py:2130`, `app/ponto/turnos.py:2145`, `app/ponto/turnos.py:2166`) |
| `_dias_encerrados_puro` | Repassa o mapa composto, corretamente, tanto a `vinculo_do_dia` quanto ao julgamento do fim previsto. (`app/ponto/turnos.py:2164`, `app/ponto/turnos.py:2165`) |
| `turnos_do_colab` | Espera `{data: CelulaDia}`. Se precisa carregar, converte explicitamente o composto para esse formato. (`app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1884`) |
| Candidatos, `mais=` | Lê `celulas.values()` e extrai `escala_geradora_id`; a forma da chave não prejudica esse consumidor. (`app/ponto/turnos.py:1886`) |
| Papel da ata | Percorre `celulas.items()` e entrega **o objeto** a `periodos_do_dia`. A chave apenas identifica um balde interno. (`app/ponto/turnos.py:1629`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1702`, `app/ponto/turnos.py:1729`) |
| Escolha do vínculo dos segmentos | Reconstrói `{(_cid, _d): _c}` a partir das chaves recebidas. Depois chama `vinculo_do_dia`. (`app/ponto/turnos.py:1954`, `app/ponto/turnos.py:1958`) |
| `vinculo_do_dia` | Busca `(celulas or {}).get((colaborador_id, data))`; lê a geradora e procura seu vínculo. Sem célula encontrada, escolhe entre os vínculos que cobrem a data. (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:238`, `app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:253`) |
| Fim previsto do encerramento | `_dia_encerrado_motivo_puro` → `fim_turno` → `marcos_turno` → `ec.marcos_do_dia`, que faz `celulas.get((self.colaborador_id, data))`. Nesse ramo, o mapa composto está correto. (`app/ponto/turnos.py:2119`, `app/escala/servico_jornada.py:92`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1095`) |
| DNA por período, casca | Consulta `marcos_dna_periodo` **sem** alimentar `celulas`; portanto relê o DNA pela consulta própria. (`app/ponto/turnos.py:1910`, `app/escala/utils.py:100`) |
| DNA por período, lote | Alimenta `marcos_dna_periodo` com mapa composto, desestruturado por `(cid, d)` em `.items()`. (`app/ponto/turnos.py:3001`, `app/escala/utils.py:97`) |

A transformação defeituosa é esta: uma chave recebida como `(cid, D)` vira `(cid, (cid, D))`; a busca por `(cid, D)` não encontra a célula. O dado perdido nesse ponto é `escala_geradora_id`, e a decisão afetada é **qual vínculo fornece o template e delimita os segmentos do pareamento**. O vínculo escolhido fornece marcos de borda/intervalo e também condiciona o uso do mapa de DNA quando há segundo intervalo. (`app/ponto/turnos.py:1954`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:253`, `app/ponto/turnos.py:1930`, `app/ponto/turnos.py:1982`, `app/ponto/turnos.py:1985`)

O papel da ata continua lendo `celula.ata`, `celula.dna` e **`celula.data`**; não busca `celulas.get(data)`. Produz um mapa por minuto, lido depois por `papel_por_minuto.get(_m)`. Assim, a alegação “o formato composto torna o papel cego às células recebidas” não se sustenta nesse código. (`app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:158`, `app/ponto/juiz_batida.py:175`, `app/ponto/turnos.py:1804`, `app/ponto/turnos.py:903`)

Há, porém, uma **segunda diferença de alimentação**: o cartório passa suas células de `d0..d1`, enquanto o pareador lê batidas e papel na janela com pad, `d0−2..d1+1`. Como qualquer mapa fornecido impede a carga automática, as células externas a `d0..d1` não são acrescentadas. Isso retira a ata das bordas, independentemente do defeito das chaves. (`app/ponto/services/cartorio.py:919`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:1845`, `app/ponto/turnos.py:1882`, `app/ponto/turnos.py:1937`)

O cadastro direto do cartório continua recebendo todos os vínculos e o mapa composto correto; não se deve confundir essa consulta com a segmentação interna do pareador. (`app/ponto/services/cartorio.py:931`, `app/ponto/services/cartorio.py:933`, `app/ponto/supra_juiz.py:161`, `app/ponto/supra_juiz.py:162`)

**2. Chamadores e formatos**

Na tabela, “omitidos” significa `celulas=None`/`turnos=None`, conforme a assinatura. (`app/ponto/turnos.py:2125`)

| Chamador de `dias_encerrados` | Alimentação |
|---|---|
| `app/ponto/services/cartorio.py:942` | Mapa composto construído de `cels`; `turnos` omitido. Alcança o repasse incompatível. |
| `app/colaboradores/services/calendario.py:699` | Mapa composto `_cels_mes`; lista de turnos já calculados, passada em `app/colaboradores/services/calendario.py:700`. Não chama internamente o pareador. |
| `app/ponto/turnos.py:2177` | Ambos omitidos; composição de `par_completo_da_janela`. |
| `app/ponto/management/commands/supra_juiz.py:121` | Ambos omitidos. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:62` | Ambos omitidos. |
| `app/ponto/tests/test_c1_dias_encerrados.py:28` | Ambos omitidos. |
| `app/ponto/tests/test_c1_dias_encerrados.py:51` | Ambos omitidos. |
| `app/ponto/tests/test_c1_dias_encerrados.py:66` | Mapa composto de `celulas_do_periodo`; `turnos` omitido. |
| `app/ponto/tests/test_c1_dias_encerrados.py:77` | Mesmo mapa composto; `turnos` omitido. |
| `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:73` | Ambos omitidos. |

O calendário calcula os turnos sem células fornecidas e carrega separadamente `_cels_mes`; depois entrega ambos ao encerramento. (`app/colaboradores/services/calendario.py:301`, `app/colaboradores/services/calendario.py:395`, `app/colaboradores/services/calendario.py:699`)

| Chamador de `_dias_encerrados_puro` | Alimentação |
|---|---|
| `app/ponto/turnos.py:2147` | Mapa composto; todos os vínculos; turnos fornecidos ou produzidos pela casca. (`app/ponto/turnos.py:2145`, `app/ponto/turnos.py:2146`, `app/ponto/turnos.py:2148`) |
| `app/ponto/turnos.py:3012` | Mapa composto recortado para `[ini,fim]`; todos os vínculos do colaborador; `self.turnos(colab)` memoizado. (`app/ponto/turnos.py:3013`) |

| Instância de `CargaDaJanela` | Janela e uso |
|---|---|
| `app/ponto/status_dia.py:189` | `[data,data]`, agrupando colaboradores por data; lê turnos e motivos do encerramento. (`app/ponto/status_dia.py:195`, `app/ponto/status_dia.py:196`) |
| `app/core/services/painel_op.py:327` | `[hoje−2,hoje]`; encontra o dia da entrada e consulta encerramento antes de pedir o status D4. (`app/core/services/painel_op.py:330`, `app/core/services/painel_op.py:334`, `app/core/services/painel_op.py:336`) |

Os chamadores de `turnos_do_colab` com células **explícitas** são:

| Chamador | Formato |
|---|---|
| `app/ponto/turnos.py:2096` | Repasse da porta singular; contrato por `data`. |
| `app/ponto/turnos.py:2148` | Repasse da porta plural; mapa composto, incompatível. |
| `app/escala/utils.py:1532` | Por `data`. |
| `app/escala/utils.py:1847` | Por `data`, com cópias contendo ata construída em memória. (`app/escala/utils.py:1835`, `app/escala/utils.py:1836`) |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` | Por `data`, construído nas linhas anteriores. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`) |
| `app/ponto/tests/test_o142_turno_invariante_na_janela.py:169`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:172`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:176` | Por `data`. |
| `app/ponto/tests/test_o142_turno_invariante_na_janela.py:203`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:204`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:214` | `{}` explícito. |
| `app/escala/tests/test_saida_valida_vira_orfa.py:158` | `{DIA: cel}`, com ata em memória. (`app/escala/tests/test_saida_valida_vira_orfa.py:152`) |

Os demais chamadores abaixo **omitem `celulas`**, acionando a carga e conversão próprias; `turnos_do_colab` não recebe argumento `turnos`. (`app/ponto/turnos.py:1848`, `app/ponto/turnos.py:1882`, `app/ponto/turnos.py:1884`)

| Grupo | Chamadas |
|---|---|
| Núcleo e portas de leitura | `app/ponto/turnos.py:778`; `app/ponto/turnos.py:2179`; `app/ponto/turnos.py:2909`; `app/ponto/turnos.py:3041`; `app/ponto/turno_leitura.py:89`; `app/ponto/turno_leitura.py:95`; `app/ponto/nucleo.py:47` |
| Telas/API/relatórios | `app/ponto/views.py:377`; `app/ponto/views.py:1586`; `app/ponto/views.py:3167`; `app/api/views.py:1449`; `app/relatorios/pdf_espelho.py:213`; `app/relatorios/pdf_espelho.py:447`; `app/colaboradores/services/calendario.py:301`; `app/colaboradores/services/drawer.py:45` |
| Escala | `app/escala/regua_defesa.py:358`; `app/escala/servico_jornada.py:250`; `app/escala/servico_jornada.py:392`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18` |
| Chamados | `app/chamados/models.py:1260`; `app/chamados/juizes.py:369`; `app/chamados/juizes.py:1727`; `app/chamados/detector_anomalias.py:155`; `app/chamados/services/materializacao.py:109`; `app/chamados/services/materializacao.py:206`; `app/chamados/services/disputa_emissao.py:644` |
| Serviços de ponto | `app/ponto/services/espelho.py:625`; `app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/passivo_s84.py:82` |
| Comandos | `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/e6_oraculo.py:147`; `app/ponto/management/commands/censo_fase_12x36.py:180`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124` |
| Testes de ponto | `app/ponto/tests/test_retratada_geometria.py:39`; `app/ponto/tests/test_turnos_borda_direita.py:37`; `app/ponto/tests/test_pareador_dna_multivig.py:55`; `app/ponto/tests/test_bateria_lavratura.py:1406`; `app/ponto/tests/test_vinculo_do_dia_pela_celula.py:144`; `app/ponto/tests/test_vinculo_do_dia_pela_celula.py:156`; `app/ponto/tests/test_pareador_dna.py:50`; `app/ponto/tests/test_o280_bateria_da_lei.py:3010`; `app/ponto/tests/test_f9d_n1_caracterizacao_eco_flush.py:96`; `app/ponto/tests/test_turnos_do_colab_troca_escala.py:53` |
| Outros testes | `app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:61`; `app/relatorios/tests/test_pdf_sem_regra_propria.py:94`; `app/escala/tests/test_ata_x_turno_entrada.py:82`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:96`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:199`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:76`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:90`; `app/escala/tests/test_saida_valida_vira_orfa.py:92`; `app/escala/tests/test_saida_valida_vira_orfa.py:196`; `app/api/tests/test_espelho_app_fonte_e_geometria.py:322`; `app/chamados/tests/test_orfas_pelo_evento.py:391` |

O caminho que chega ao **veredito gravado** é o do cartório: `_turno_encerrado` alimenta o classificador e a impressão de dia em curso; a lavra recebe os códigos resultantes. Calendário e painel são leitores/apresentação nesses caminhos. (`app/ponto/services/cartorio.py:942`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1253`, `app/ponto/services/cartorio.py:1596`, `app/colaboradores/services/calendario.py:701`, `app/core/services/painel_op.py:338`)

**3. Quem escreve, quem lê e qual juiz responde**

| Dado/resultado | Escritor/produtor | Leitor no fluxo auditado |
|---|---|---|
| `trabalha`, geradora e DNA | Geração/regeneração pela porta; `montar_dna` constrói o blob e seus marcos. (`app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:593`, `app/ponto/portas/celula.py:612`, `app/ponto/management/commands/gerar_celulas.py:73`) | Geradora: candidatura e vínculo diário; DNA: marcos e papel; `trabalha`: previsão de trabalho. (`app/ponto/turnos.py:1886`, `app/escala/alimentacao.py:238`, `app/escala/models.py:1099`, `app/ponto/juiz_batida.py:158`, `app/escala/models.py:1072`) |
| Ata do julgamento | `ata_do_dia` monta natureza, lâmpadas, minutos, órfãs e contagens; cartório entrega à lavra. (`app/ponto/services/cartorio.py:599`, `app/ponto/services/cartorio.py:607`, `app/ponto/services/cartorio.py:623`, `app/ponto/services/cartorio.py:1594`) | Papel pelo juiz de batida; projeção de grade pela célula. (`app/ponto/juiz_batida.py:157`, `app/escala/services/leitor_celula.py:332`) |
| Ata persistida | `lavrar_veredito` escreve `cel.ata`; `carimbar` persiste junto dos campos lavrados. Há ainda a operação específica que escreve `ata['precedencia']`. (`app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:768`, `app/ponto/portas/celula.py:807`, `app/ponto/portas/celula.py:819`) | Folha, relatório e cobrança também leem a ata. (`app/folha/previa.py:83`, `app/folha/validacao_pdf.py:47`, `app/relatorios/cartao_pela_celula.py:287`, `app/chamados/juizes.py:407`) |
| Papel por minuto | `papel_por_minuto_da_ata` produz o mapa; o pareador escreve `_papel_ata` nas instâncias em memória. (`app/ponto/turnos.py:1804`, `app/ponto/turnos.py:1828`, `app/ponto/turnos.py:1313`) | `_papel_da_ata` consulta o mapa; pareador decide borda/intervalo; outros passos leem o carimbo. (`app/ponto/turnos.py:903`, `app/ponto/turnos.py:1314`, `app/ponto/turnos.py:1380`, `app/ponto/turnos.py:875`, `app/ponto/turnos.py:996`) |
| Resultado de `vinculo_do_dia` | Retorno calculado, sem gravação nessa função. (`app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:252`, `app/escala/alimentacao.py:254`) | Segmentação, encerramento e cadastro do classificador. (`app/ponto/turnos.py:1958`, `app/ponto/turnos.py:2164`, `app/ponto/supra_juiz.py:161`) |
| Veredito, protestos, via e instante | Porta de lavra; o modelo delega a ela. (`app/ponto/portas/celula.py:769`, `app/ponto/portas/celula.py:770`, `app/ponto/portas/celula.py:771`, `app/ponto/portas/celula.py:772`, `app/escala/models.py:1787`) | Projeção da célula e regra do status da mesma lavra. (`app/escala/services/leitor_celula.py:366`, `app/ponto/portas/celula.py:752`) |
| `status_dia`, `status_rotulo` | Regra `status_da_lavra`, aplicada pela porta junto do veredito; decisão administrativa tem tratamento próprio. (`app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:705`) | D4 lê as colunas após consultar o encerramento; painel recebe o resultado em `linha['dia']`. (`app/ponto/status_dia.py:196`, `app/ponto/status_dia.py:203`, `app/ponto/status_dia.py:205`, `app/core/services/painel_op.py:338`) |

As chaves da ata têm leitores específicos: `lampadas`/`orfas` alimentam o juiz de batida; contagens, minutos e natureza alimentam `grade_da_celula`; o cartório também extrai entrada/saída da ata para carimbar a célula. (`app/ponto/juiz_batida.py:160`, `app/ponto/juiz_batida.py:161`, `app/escala/services/leitor_celula.py:332`, `app/escala/services/leitor_celula.py:345`, `app/escala/services/leitor_celula.py:355`, `app/ponto/services/cartorio.py:1615`)

Outros leitores diretos da ata aparecem em pautas, feriados, worklist, materialização, detectores e vigias: `app/pautas/services.py:422`; `app/colaboradores/services/feriado_do_posto.py:96`; `app/ponto/worklist.py:22`; `app/chamados/services/materializacao.py:157`; `app/ponto/management/commands/detectar_intervalo_ausente.py:47`; `app/ponto/services/cluster_espurio.py:24`; `app/ponto/services/vigia_entrada.py:80`; `app/ponto/services/dia_do_colab.py:29`.

O cruzamento com o anexo deve usar o **registro vivo**, como o próprio documento determina. (`app/docs/JUIZES.md:16`)

| Pergunta tocada | Juiz registrado |
|---|---|
| A qual turno pertence a batida? | `turnos_do_colab`. (`app/core/juizes.py:196`) |
| Há turno aberto vivo? | `turno_aberto_de`. (`app/core/juizes.py:199`) |
| Quais são os marcos do dia? | `EscalaColaborador.marcos_do_dia`. (`app/core/juizes.py:201`) |
| Cruza meia-noite? | `turno_cruza_meia_noite`. (`app/core/juizes.py:204`) |
| O dia está encerrado? | `dia_encerrado`; `dias_encerrados` é carregador/composição. (`app/core/juizes.py:223`, `app/ponto/turnos.py:2134`) |
| Qual vínculo vale e se cobre a data? | `vinculo_do_dia` e `vinculo_cobre_a_data`. (`app/core/juizes.py:1275`, `app/core/juizes.py:1276`) |
| Dia de trabalho, previsto e precedência? | `eh_dia_trabalho`, `minutos_previstos_do_dia`, `fatos_do_dia`. (`app/core/juizes.py:172`, `app/core/juizes.py:173`, `app/core/juizes.py:175`) |
| Qual status do dia? | `status_do_dia_lavrado`. (`app/core/juizes.py:187`) |
| Cobrança deve calar por dia sem trabalho? | `chamados/juizes.py::dia_sem_trabalho`. (`app/core/juizes.py:278`) |
| Folga lançada vale no dia? | `folga_lancada_do_dia`, alimentada pelo vínculo diário. (`app/core/juizes.py:192`, `app/docs/JUIZES.md:84`) |

A lista resumida do anexo não inclui `dia_encerrado` no bloco de geometria, mas o registro vivo o registra expressamente; “lavra/fechamento: SEM juiz” é outra pergunta e não transforma `CargaDaJanela` ou a porta escritora em autoridade de encerramento. (`app/docs/JUIZES.md:20`, `app/core/juizes.py:223`, `app/docs/JUIZES.md:71`, `app/ponto/turnos.py:2934`)

**4. Teste existente e caso mínimo discriminante** [nao verificado]

**Existe teste com células fornecidas e template noturno.** Ele cria DNA `10–14` sobre template `19–07`, passa o mapa composto e compara os motivos com a porta singular. Porém, não cria batidas nesse teste, não nomeia uma geradora na célula e herda uma pessoa com somente um vínculo. Portanto, verifica o fim previsto por DNA, mas não distingue a perda da geradora no pareamento. (`app/ponto/tests/test_c1_dias_encerrados.py:53`, `app/ponto/tests/test_c1_dias_encerrados.py:62`, `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:70`, `app/ponto/tests/test_juiz_dia_encerrado.py:69`)

Não foi localizado teste que reúna **`dias_encerrados` com células externas + múltiplos vínculos + batidas noturnas**. Ausência de outro teste fora do inventário apresentado: `[nao verificado]`. [nao verificado]

Um caso mínimo proposto, ainda não executado, é:

- Pessoa com vínculo ativo A `08–17`, pausa `07–08`; vínculo B inativo, sem `data_fim`, início anterior a D, `19–07`, sem pausa. A célula de D nomeia B. A geradora nomeada pode vencer mesmo inativa; sem enxergá-la, o fallback escolhe A. (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:253`, `app/escala/alimentacao.py:254`)
- Apenas célula D, DNA `19–07`, ata vazia; batidas `D 19:00 E` e `D+1 07:00 S`, sem batida posterior; observar em `D+1 08:00`. Sem DNA em D+1, as distâncias usam os marcos do template escolhido. (`app/ponto/turnos.py:1227`, `app/ponto/turnos.py:1235`)
- Comparar `dias_encerrados(..., celulas=composto)` com a mesma chamada acrescida de `turnos=turnos_do_colab(..., celulas={D: cel})`, mantendo `[D,D]`. Essa alimentação evita o repasse incompatível. (`app/ponto/turnos.py:2148`)

| Caminho | Saída previst

[truncado em 20000 de 24647 caracteres]

## 01/11 14:08 · sessao 7330d4c01221 · - · - · origem=Ronald (tmux)

Proposta pela **falha residual na cópia final**: **1=a, 2=c, 3=a, 4=b, 5=b, 6=e, 7=b condicionado, 8=b**.

- **1:** a só com ressalva: o Codex viu exatamente a fabricação por CLI/log sintético/Write no `raiz/codex_v1.r` #13. A triagem declarou #13 **curado**, sem descarte. A revisão v2, perguntada sobre curas, confirmou apenas Ran/OK e perdeu o residual. Não usar descarte da guarda lexical #8 como substituto.
- **2:** c para execução real/prova ponta a ponta. A insuficiência estática do grep e do log sintético já foi vista em v1 e v2; não há descarte explícito dessa parte.
- **3:** a: derivação paralela estava no checklist; nenhuma resposta apresentada identifica especificamente as duas regex como duplicação. “NENHUM viu” além dos registros fornecidos é [nao verificado].
- **4:** b inequívoco: v1.r #14b viu ABA; v1_triagem mantém em fila.
- **5:** b inequívoco: v1.r, v2.r, v3.r e v4/v2.r viram; triagens raiz/v2 e v4/v2 dizem fila.
- **6:** e para pertinência semântica contrato↔diff e `--no-verify`: perguntas sobre existência/escritores/ordem do gate não explicitam essas garantias adicionais.
- **7:** v4/v1.r #5 viu literalmente remoção de outra sessão viva. **Falta descarte explícito nas triagens fornecidas.** v4/v3_triagem c3 só corrige `poda_seq` e remete ao aval CONTENCAO-SESSOES. Se exigir rigor literal, nenhuma classe atende integralmente: viu, mas não há linha de descarte. “Coberto pelo aval” consta no opus, não comprova descarte na triagem.
- **8:** b inequívoco: raiz/v2.r #5 e v3.r #3; raiz/v2_triagem descarta para fila por desenho TTL/teto.

A taxonomia tem duas limitações documentais: não prevê “viu e triagem declarou curado incorretamente” (#1) nem “viu, sem destino explícito desse risco na triagem” (#7). Convém declarar isso para não fabricar evidência.

## 01/11 14:07 · sessao 509803024587 · - · - · origem=Ronald (tmux)

Consolidado enviado com os chamadores e formatos. O ponto decisivo é:

- `dias_encerrados` repassa o mapa composto a `turnos_do_colab`, que reembrulha as chaves e impede a leitura da geradora na segmentação (`app/ponto/turnos.py:2148`, `app/ponto/turnos.py:1954`, `app/ponto/turnos.py:1958`).
- O papel da ata continua lendo os objetos por `.items()`; a chave composta, sozinha, não o torna invisível (`app/ponto/turnos.py:1629`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1702`, `app/ponto/turnos.py:1729`).
- Existe teste com células fornecidas e noturno, mas ele não cria batidas e herda um único vínculo; portanto não distingue o defeito de segmentação (`app/ponto/tests/test_c1_dias_encerrados.py:53`, `app/ponto/tests/test_c1_dias_encerrados.py:62`, `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_juiz_dia_encerrado.py:69`).
- O caso mínimo proposto pelo coordenador distingue estaticamente `False / turno_aberto_relogio` de `True / None`; execução `[nao verificado]` (`app/ponto/turnos.py:1354`, `app/ponto/turnos.py:1169`, `app/ponto/turnos.py:2114`, `app/ponto/turnos.py:2119`).

## 01/11 14:06 · sessao effa75d5be39 · - · - · origem=Ronald (tmux)

Rastreio de escritores/leitores e registro de juízes:

- **Ata produzida:** `app/ponto/services/cartorio.py:591` monta `tipo_dia`, `lampadas`, `minutos_previstos`, `minutos_realizados`, `sem_ancora`, `orfas`, `n_orfas`, `n_missing`, `n_celulas` (`:596-623`). Ramo principal passa ata e fatos para lavra em `:1594-1601`; ramo sem batida em `:1075-1080`.
- **Ata/veredito/status gravados:** modelo apenas delega (`app/escala/models.py:1787-1796`). Porta `lavrar_veredito` copia/compara ata (`app/ponto/portas/celula.py:726-736`), escreve ata (`:741`, `:768`), protestos/veredito/via/instante (`:769-773`), aplica status (`:686-705`). Cartório usa `save=False` e depois `carimbar` (`app/ponto/services/cartorio.py:1615-1617`); `carimbar` acrescenta `julgada_em`, impressão, ausência, `tem_entrada/tem_saida`, `insumos_em` e persiste todos os campos lavrados (`app/ponto/portas/celula.py:800-807`), com `save(update_fields=...)` em `:640`.
- **Outro escritor legítimo da ata:** `carimbar_precedencia` escreve exclusivamente `ata['precedencia']` e salva pela mesma porta (`app/ponto/portas/celula.py:811-822`). Portanto, “lavrar_veredito é o único escritor absoluto de qualquer chave de ata” seria impreciso; é a operação escritora do julgamento (`:709-713`).

**Papel da ata: a alegação da fila precisa ser corrigida.** `papel_por_minuto_da_ata` itera `celulas.items()` (`app/ponto/turnos.py:1629`), chama `periodos_do_dia(cel, te)` (`:1631`), que lê `celula.ata/dna` (`app/ponto/juiz_batida.py:157-160`) e usa `celula.data`, não a chave do mapa, ao reconstruir instantes (`:174-176`). A chave `_d` só armazena/intera baldes (`app/ponto/turnos.py:1702`, `:1729`); os papéis saem indexados por minuto (`:1802-1804`, `:1828`). Logo, mapa com chaves `(cid,data)` viola o contrato declarado `{data:CelulaDia}` (`:1921-1924`), mas **isso isoladamente não torna essa leitura de ata cega**. O pareador consulta o papel em `_papel_da_ata` (`:888-907`, especialmente `.get(_m)` em `:903`), recebe-o em `:1308` e carimba `_papel_ata` em `:1313`.

**Incompatibilidade efetiva com vínculo:** `_turnos_do_colab_puro` transforma o mapa recebido com `{(_cid,_d):_c for _d,_c in celulas.items()}` (`app/ponto/turnos.py:1954`); se `_d` já é `(cid,data)`, forma `(cid,(cid,data))`. `vinculo_do_dia` consulta `(cid,data)` (`app/escala/alimentacao.py:237`), não vê a geradora (`:238`), e usa candidatura por vigência/desempate (`:253-254`) em lugar da geradora da célula (`:247-252`).

**Cartório não está inteiro cego à geradora:** o carregador próprio de cadastro passa todos os vínculos e mapa correto em `app/ponto/services/cartorio.py:931-933`; `cadastro_do_dia` consulta o juiz e `celulas.get((cid,data))` em `app/ponto/supra_juiz.py:160-162`. O caminho afetado pelo repasse a `dias_encerrados` é `_turno_encerrado` (`app/ponto/services/cartorio.py:939-943`), usado em classificação (`:1063`, `:1253`) e em `_em_curso` (`:1180`). Dano concreto ou direção da mudança em produção: `[nao verificado]`.

**Leitores da ata e campos:**

- `periodos_do_dia`: ata, DNA, lâmpadas e órfãs (`app/ponto/juiz_batida.py:157-161`), alimentando papel e pareador pelos pontos acima.
- `grade_da_celula`: `n_celulas/n_missing/minutos_realizados` (`app/escala/services/leitor_celula.py:332-334`), `tipo_dia/minutos_previstos` (`:345-347`), lâmpadas/marcos/órfãs e contagens (`:355-365`), veredito/id (`:366`) e insumo do juiz `dia_sem_trabalho` (`:367-370`).
- Folha: prévia (`app/folha/previa.py:83`), validação PDF (`app/folha/validacao_pdf.py:47`), lâmpadas apagadas (`app/folha/services/sem_furo.py:20-22`); cartão relatório (`app/relatorios/cartao_pela_celula.py:286-287`).
- Cobrança: leitores diretos da ata em `app/chamados/juizes.py:407`, `:442`, `:1008`; materialização em `app/chamados/services/materializacao.py:157`; cartório publica ata do julgamento corrente em `app/ponto/services/cartorio.py:1219`, exposta em `:48-57`, e o leitor C6 projeta essa ata corrente em `app/chamados/juizes.py:392-394`.
- Status gravado: único leitor funcional localizado é `app/ponto/status_dia.py:203-205`; antes dele, a decisão usa `carga.turnos` e `carga.dias_encerrados` (`:195-201`). Sem célula produz `sem_dados` (`:191-194`); sem status lavrado usa previsto/sem_certidão (`:207-208`).
- Painel: `_d4_em_lote` cria carga em janela `[hoje−2,hoje]`, escreve `linha['data_turno']`, só pede D4 para dias encerrados e escreve `linha['dia']` (`app/core/services/painel_op.py:327-338`). D4 cria outra carga por data distinta (`app/ponto/status_dia.py:188-189`).

**Juízes vivos cruzados com anexo:**

| Pergunta | Registro vivo | Autoridade |
|---|---|---|
| Dia é de trabalho | `app/core/juizes.py:172` | `EscalaColaborador.eh_dia_trabalho` |
| Minutos previstos | `app/core/juizes.py:173` | `escala/utils.py::minutos_previstos_do_dia` |
| Precedência | `app/core/juizes.py:174-175` | `ponto/precedencia.py::fatos_do_dia` |
| Status do dia | `app/core/juizes.py:187` | `status_do_dia_lavrado` |
| Folga lançada válida | `app/core/juizes.py:192` | `folga_lancada_do_dia` |
| Turno da batida | `app/core/juizes.py:196` | `turnos_do_colab` |
| Turno aberto | `app/core/juizes.py:199` | `turno_aberto_de` |
| Marcos previstos | `app/core/juizes.py:201` | `EC.marcos_do_dia` |
| Cruza meia-noite | `app/core/juizes.py:204` | `turno_cruza_meia_noite` |
| Dia encerrado | `app/core/juizes.py:223` | `dia_encerrado` |
| Par completo | `app/core/juizes.py:228` | `par_completo` |
| Cobrança cala em dia sem trabalho | `app/core/juizes.py:278` | `chamados/juizes.py::dia_sem_trabalho` |
| Vínculo do dia/cobre data | `app/core/juizes.py:1275-1276` | `vinculo_do_dia`/`vinculo_cobre_a_data` |

O anexo local confirma geometria (`app/docs/JUIZES.md:20`), dia sem trabalho (`:55`), status (`:82-83`), vínculo/vigência (`:92-95`). O registro vivo acrescenta expressamente `dia_encerrado` (`app/core/juizes.py:223`), ausente da lista resumida de geometria do anexo. “Lavra/fechamento: SEM juiz” no anexo é outro domínio (`app/docs/JUIZES.md:68-71`); a porta escritora da célula não deve ser apresentada como juiz novo de encerramento ou de formato.

## 01/11 14:06 · sessao cf00d4a9f629 · - · - · origem=Ronald (tmux)

Auditoria estática, somente leitura.

| Carga | Casca singular | `CargaDaJanela` | Conclusão |
|---|---|---|---|
| Janela | `janela_com_pad`: `ini−2..fim+1` (`app/ponto/turnos.py:1845`); usada em `:1860` | Mesma função em `:2948` | Mesma janela de pad no código (`:1860`, `:2948`). |
| Células | Sem alimentação, carrega pad e converte tupla→data (`:1882–1884`) | Carrega pad uma vez, agrupa por colaborador/data (`:2951–2955`), converte ao chamar núcleo (`:2996–3005`) | Contrato padrão equivalente; lote faz a conversão necessária (`:3005`). |
| Candidatos dos turnos | `escalas_do_periodo(colab,pad_ini,fim,mais=geradoras)` (`:1885–1886`) | `escalas_do_periodo_por_colab(ids,pad_ini,fim,mais=geradoras)` (`:2957–2958`) | Mesma porta: singular delega à plural (`app/escala/alimentacao.py:153–154`). |
| Todos os vínculos para encerramento | Sem filtro de atividade/data, com `tipo_escala` (`app/ponto/turnos.py:2146`) | Mesmo universo em lote (`:2961–2962`) | Sem diferença estática identificada nesse universo (`:2146`, `:2961`). |
| Batidas | Vivas, `timestamp__date` no pad (`:1861–1864`) | `batidas_apuraveis(None)`, mesmos cortes, agrupadas/ordenadas (`:2990–2994`) | `batidas_apuraveis` impõe `retratada_em IS NULL` (`:2872`); universo equivalente nos cortes mostrados. |
| Ata | Núcleo recebe `escalas_ata=None` (`:1913`); papel chama `_escalas_da_ata` (`:1591`) | Pré-carrega `_escalas_da_ata(ids)` (`:2963`) e fornece ao núcleo (`:3005`) | Mesma seleção compartilhada (`:1536–1560`). |
| DNA | Provedor consulta `marcos_dna_periodo` sem células (`:1907–1910`) | Fornece células já carregadas (`:2999–3002`) | Singular pode reler células/DNA; lote reutiliza snapshot (`app/escala/utils.py:96–102`). |
| Turnos | Cada chamada à casca chama núcleo (`:1913`) | Cache por colaborador (`:2995–3007`) | Lote reutiliza turnos entre encerramento e dia das batidas (`:3012–3018`). |

A candidatura admite vínculos ativos ou encerrados com `data_fim`, sobrepostos à janela (`app/escala/alimentacao.py:116–122`). `mais=` busca os IDs ausentes sem o filtro de vigência/atividade e os coloca no balde do respectivo dono (`:123–127`). Portanto, geradora inativa chega por ambas as cargas; a escolha diária ainda aplica a lei da geradora e sua vigência (`:237–254`). Não identifiquei divergência de universo padrão entre lote e casca nesses pontos (`app/ponto/turnos.py:1885–1886`, `:2957–2958`).

Há duplicação da **implementação da carga**: casca em `app/ponto/turnos.py:1859–1913`, lote em `:2944–3006`; o julgamento é compartilhado em `_turnos_do_colab_puro` (`:1916`) e `_dias_encerrados_puro` (`:2152`). Além disso, sem argumentos alimentados, `dias_encerrados` carrega células de `[ini,fim]` e todos os vínculos (`:2145–2146`), chama a casca com `celulas=None` (`:2148`), e esta carrega novamente células com pad e candidatos (`:1882–1886`); o DNA pode custar outra leitura (`:1910`; `app/escala/utils.py:100–102`).

O painel monta carga de `[hoje−2,hoje]` para descobrir dia da entrada e encerramento (`app/core/services/painel_op.py:327–335`), depois chama D4 (`:336`), que monta outra carga para cada data distinta, agora `[data,data]` (`app/ponto/status_dia.py:188–196`). São janelas intencionalmente distintas no código; equivalência dos turnos entre essas janelas não deve ser presumida (`app/ponto/status_dia.py:176–179`).

**Selo existente e lacuna:** o teste D4 contém noturno e várias pessoas/datas (`app/core/tests/test_e4_7iii_lote_d4.py:67–109`), mas cada pessoa nasce com um vínculo ativo (`app/core/tests/test_e4_7iii_painel_e_presenca.py:54–59`; noturno em `app/core/tests/test_e4_7iii_lote_d4.py:101–108`). Não exercita pessoa com múltiplos vínculos nem geradora inativa nessa fixture (`:67–109`). A “paridade lote/singular” compara `status_dos_dias_lavrados` com `status_do_dia_lavrado` (`:136–147`), e este último já delega ao lote com um par (`app/ponto/status_dia.py:167–170`): não é selo independente de `CargaDaJanela` contra a casca `turnos_do_colab`.

Existe teste alimentando `dias_encerrados` com células de `celulas_do_periodo`, template noturno e DNA divergente (`app/ponto/tests/test_c1_dias_encerrados.py:53–77`); ele não cria batidas nesse teste (`:62–77`) e a pessoa tem somente um vínculo ativo (`app/ponto/tests/test_juiz_dia_encerrado.py:48–70`). Assim, não distingue a perda da geradora durante a montagem dos turnos.

**Correção importante da descrição da fila:** o papel da ata itera os **valores** de `celulas` (`app/ponto/turnos.py:1629–1631`); a chave `_d` apenas indexa `_dias` (`:1702`) e é iterada sem busca por data (`:1729`). Nesse consumidor, trocar chave data por tupla não basta para esconder a célula. A perda demonstrável ocorre ao reencapsular uma chave já composta como `(_cid, (cid,data))` (`:1954`), que não será encontrada por `vinculo_do_dia`, cujo acesso é `.get((colaborador_id,data))` (`app/escala/alimentacao.py:237`). Com mapa errado, fallback passa a escolher por vigência/atividade (`:253–254`) em vez da geradora (`:237–252`).

Execução de testes e consequências em dados de produção: `[nao verificado]`. [nao verificado]

## 01/11 13:46 · sessao 8911ef9e5156 · O261-E-F2 · censo · origem=Code

A F1 está **parcialmente realizada**: há gate, pós-estado e tripwire, mas a absorção prevista da S127 não ocorreu integralmente. A F2 ainda tem vários escritores diretos, sem função comum de escrita. (`app/chamados/models.py:147`, `app/chamados/models.py:195`, `app/chamados/models.py:391`, `app/core/tests/test_contract_escritores_estado.py:83`)

O inventário abaixo separa código operacional, atribuições somente em memória e definições de schema. Execução dos testes, sondas de produção e confirmação Git do hash informado: **[não verificado]**. [nao verificado]

**1. Passo-a-passo da F1**

| Passo do anexo | Situação encontrada | Sítios |
|---|---|---|
| **1 — Medir desligamento e RECONCILIAR** | **Documentado; execução primária [não verificado].** O relato registra 43 vivas sob 25 chamados de desligados; 106 fechamentos possíveis, dos quais 18 reatariam e 88 recusariam. A ordem do desligamento foi efetivamente invertida: disputas antes dos chamados. | Registro: `app/docs/RELATO.md:270`. Ordem atual: `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:150`. |
| **2 — RED: viva na âncora, pós-estado e costura em terminal** | **Testes presentes.** Execução histórica em RED contra a base: **[não verificado]**. | Âncora destino: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`; container: `:94`; pós-estado/slot não derivável: `:172`; célula acusadora: `:179`; costura terminal: `:252`; dedup de `abrir`: `:275`. |
| **3 — Gate reata/recusa; humano levanta; absorver `fio_vivo` da S127** | **Parcial.** Sistema compõe premissa e guarda da pergunta; reata por pergunta ou registra recusa. Humano levanta. Entretanto, a S127 continua consultando `disputa_viva_do_chamado`, cujo corpo ainda usa `fio_vivo`. | Composição: `app/chamados/models.py:627`, `:646`, `:649`; plano por pergunta: `:355`, `:371`, `:373`; reaponte: `:418`; recusa: `:378`, `:403`; humano: `:159`. S127 remanescente: `app/chamados/models.py:147`, `app/chamados/juizes.py:1967`. |
| **4 — Tripwire vivo→TERMINAIS com pós-estado** | **Mecanismo realizado.** Detecta também instância velha, consulta as perguntas no pós-estado e levanta antes da escrita. **Ressalva ao “mesmo atomic”:** a checagem antecede o `atomic` interno de `_gravar_estado`; os gates de sistema envolvem decisão e gravação em transação, mas `transicionar`→`_aplicar` não abre essa transação externa no próprio modelo. | Tripwire: `app/chamados/models.py:213`, `:215`, `:218`, `:220`; pós-estado e restauração: `:332`, `:339`, `:348`, `:349`, `:352`; `atomic` interno: `:274`; sistema: `:614`; humano: `:122`, `:428`. |
| **5 — Costura em terminal, ou declaração para F2** | **Realizado para reaponte de perguntas existentes; nascimento declarado para F2.** A guarda ficou separada de `resolvedora_valida`. O ramo de pergunta nova ainda cria diretamente. | `_costura_nao_mata_o_fio`: `app/chamados/services/fio.py:742`; declaração do nascimento para F2: `:750`; aplicação no reaponte: `:840`; dedup existente: `app/chamados/models.py:2424`; nascimento: `:2453`. |
| **6 — Selo AST com mutante** | **Presente em fonte.** Execução verde e morte efetiva dos mutantes nesta cópia: **[não verificado]**. | Selo do tripwire: `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24`, `:55`; mutantes: `:64`, `:68`, `:73`, `:78`, `:83`, `:87`. Selo amplo de status: `app/chamados/tests/test_selo_status_so_pelas_portas.py:340`; mutante de defaults: `:421`. |
| **7 — `superar` sai por L-111** | **Realizado como remoção do método.** Há lápide; o estado histórico `superado` permanece no catálogo. | `app/chamados/models.py:755`, `app/chamados/models.py:759`. |
| **8 — DIFF de quem entra no TXT, aberta antes/depois, exportada por hash** | **Documentado; prova primária [não verificado].** O relato registra base 116×cura 29, diferença nominal de entrada e 26 exportadas com hash igual; uma rodada posterior registra ENTRA 347→347. | `app/docs/RELATO.md:270`, `app/docs/RELATO.md:252`, `app/docs/RELATO.md:253`. |

A decisão de RECONCILIAR continua compartilhada com o dry-run: `reconciliaria()` consulta `_decisao_do_fechamento(registrar=False)`. A guarda nova está em `_gate_pergunta_viva`, composta com `_gate_premissa_do_fechamento`, em vez de morar integralmente nesta última. (`app/chamados/models.py:621`, `app/chamados/models.py:627`, `app/chamados/models.py:646`, `app/chamados/models.py:649`)

A linha da obra ainda diz “F1 em construtor”, portanto esse rótulo documental não certifica conclusão dos oito passos. (`app/docs/BACKLOG.md:820`)

**2. Escritores e leitores de `materializada_em`**

Há dois campos homônimos: o de `PerguntaDisputa` e o de `RegularizacaoExterna`. (`app/chamados/models.py:2659`, `app/chamados/models.py:3038`)

Todos os escritores operacionais explícitos localizados estão abaixo. “C6 antes” significa consulta local antes da atribuição; isso **não equivale** à porta F2 comum que carimba em memória e consulta C6 sobre o resultado, pois esses corpos persistem separadamente. (`app/chamados/reconciliador.py:399`, `app/chamados/reconciliador.py:402`, `app/chamados/services/disputa_emissao.py:1547`, `app/chamados/services/disputa_emissao.py:1562`)

| Função | Atribuição e persistência | Porta/guarda existente |
|---|---|---|
| `PerguntaDisputa.fechar_por_admin` | `app/chamados/models.py:2500`; `save(update_fields=...)` em `:2502` | Gate Q1; idempotência em `app/chamados/models.py:2490`; sem chamada C6 no corpo. |
| `PerguntaDisputa.fechar_por_competencia_trancada` | `app/chamados/models.py:2514`; save em `:2516` | Gate Q1; condição própria em `app/chamados/models.py:2511`; sem C6 no corpo. |
| `PerguntaDisputa.encerrar_fantasma` | `app/chamados/models.py:2539`; save em `:2541` | Gate próprio; idempotência em `app/chamados/models.py:2536`; sem C6 no corpo. |
| `PerguntaDisputa.reabrir_fantasma` | Limpa em `app/chamados/models.py:2554`; save em `:2556` | Inversa da via, condicionada em `app/chamados/models.py:2551`. |
| `PerguntaDisputa.reabrir_por_resposta` | Limpa em `app/chamados/models.py:2574`; devolve campos em `:2576` | Quem persiste é `responder_pergunta_disputa_core`, somando os campos e salvando: `app/chamados/services/disputa_emissao.py:1265`, `:1266`. |
| `PerguntaDisputa.reabrir_por_competencia` | Limpa em `app/chamados/models.py:2584`; save em `:2586` | Inversa da via, condicionada em `app/chamados/models.py:2581`. |
| `ChamadoColaborador.ressuscitar_perguntas` | `update(materializada_em=None, ...)` em `app/chamados/models.py:1488` | Seleção pelo universo comum em `:1479`; C6 julga uma cópia em memória em `:1445`. |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:402`; save em `:407` | C6 antes da atribuição: `app/chamados/reconciliador.py:399`. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:621`; save em `:624` | C6 antes, mais exclusão de dia trancado: `app/chamados/reconciliador.py:611`, `:617`. |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:511`; save em `:530` | Escrita própria após criar ausência em `app/chamados/services/materializacao.py:501`; sem guarda C6 comum. |
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:860`; save em `:870` | Ramo de identidade `paridade_recusada`; condição da lâmpada em `app/chamados/services/materializacao.py:843`. |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1045`; campos em `:1053`, save em `:1055` | Só carimba `COLISAO` com campo nulo; usa `fato_ocupado`: `app/chamados/services/materializacao.py:1044`, `:1046`. |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1151`; campos em `:1186`, save em `:1200` | Unidade compartilhada por duas filas; atomic e juiz do lastro, sem guarda C6 comum pós-carimbo: `app/chamados/services/materializacao.py:1126`, `:1139`. |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1341`; save em `:1344` | Consulta `_pergunta_materializada_de_fato` antes: `app/chamados/services/materializacao.py:1339`. |
| `resolver_perguntas_por_chamado_resolvido`, primeiro ramo S128 | `app/chamados/services/disputa_emissao.py:1562`; save em `:1583` | C6 antes e exclusão da tranca: `app/chamados/services/disputa_emissao.py:1547`, `:1551`. |
| Mesma função, ramo sem credor | `app/chamados/services/disputa_emissao.py:1644`; save em `:1659` | C6 antes: `app/chamados/services/disputa_emissao.py:1641`. |
| `reabrir_para_correcao` | Limpeza montada em dicionário: `app/chamados/services/disputa_emissao.py:1360`; `qs.update(**campos)` em `:1377` | Escrita dinâmica da porta de reabertura. |
| `retratar_duplicata` | `app/chamados/services/fio.py:894`; save em `:896` | Identidade; só preenche carimbo nulo: `app/chamados/services/fio.py:893`. |
| `desfazer_carimbo_sem_lastro` | Limpa em `app/chamados/services/relavra_via.py:231`; save em `:234` | Relê travada e exige C6 viva: `app/chamados/services/relavra_via.py:221`, `:227`. |
| **Outro modelo:** `materializar_regularizacao_externa` | `app/chamados/services/regularizacao_ext.py:198`; save em `:200` | Escreve `RegularizacaoExterna`, não a pergunta: `app/chamados/models.py:3038`. |

Há ainda uma atribuição **somente em memória**: `_universo_da_ressuscitacao` faz `copy.copy(p)`, limpa o campo na cópia e consulta C6. Essa atribuição não é salva; a persistência posterior é o `update` de `ressuscitar_perguntas`. (`app/chamados/models.py:1427`, `app/chamados/models.py:1440`, `app/chamados/models.py:1445`, `app/chamados/models.py:1488`)

`PerguntaDisputa.abrir(**campos)` também possui capacidade genérica de criação com campos extras: repassa `**campos` a `objects.create`. Chamador operacional passando `materializada_em` por essa capacidade: **[não verificado]**. (`app/chamados/models.py:2453`)

Não foi localizado `bulk_update` ou SQL cru explicitamente escrevendo esse campo; cobertura de SQL/nomes de campo inteiramente construídos em execução: **[não verificado]**. O `bulk_update` da lavra encontrado escreve outro conjunto de campos. (`app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`)

As migrações localizadas introduzem os campos no schema, sem carimbo de dados nesses sítios. (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:23`, `app/chamados/migrations/0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26`)

O compartilhamento existente é **parcial**: `_materializar_uma` atende a unidade de trabalho do admin e do passivo; os demais escritores da tabela não passam por ela. (`app/chamados/services/materializacao.py:1065`, `app/core/portas.py:284`, `app/chamados/tests/test_porta_desfecho_pergunta.py:14`)

Quanto ao dinheiro mencionado no F0: `_materializar_uma` materializa antes do carimbo, consulta o juiz do lastro e levanta `_Reprovada` dentro do atomic para provocar rollback. Já o corpo atual de `materializar_pendentes_aprovadas` apenas julga e carimba; ele declara expressamente que não fabrica batida. (`app/chamados/services/materializacao.py:1126`, `:1128`, `:1139`, `:1145`, `app/chamados/services/materializacao.py:1327`, `:1337`)

Os **leitores operacionais diretos**, incluindo consultas ORM, histórico e snapshots, são:

| Arquivo | Funções e sítios de leitura |
|---|---|
| `app/chamados/juizes.py` | `_retratada_com_trilha`: `app/chamados/juizes.py:275`; `_pergunta_materializada_de_fato`: `:624`, `:650`; `_chave_memo_pv`: `:791`; `_pergunta_viva_calc`: `:1042`, `:1071`, `:1077`, `:1107`; `pergunta_sem_lastro_ou_viva`: `:1125`; `irmas_respondidas_sem_consumo`: `:1633`. |
| `app/chamados/models.py` | `_universo_da_ressuscitacao`: `app/chamados/models.py:1421`; `respostas_sem_veredito`: `:1769`; `estado`: `:2478`, `:2481`; guardas de `fechar_por_admin`, `encerrar_fantasma`, `reabrir_fantasma`, `reabrir_por_resposta`: `:2491`, `:2536`, `:2551`, `:2571`; `q_vivas`: `:2616`, `:2618`. |
| `app/chamados/reconciliador.py` | `lavrar_sem_turno_aberto`: `app/chamados/reconciliador.py:367`; `retratar_perguntas_absolvidas_por_celula`: `:586`; `_orfas_abertas`: `:1004`; trilha de `encerrar_orfas_pela_lampada`: `:1096`, `:1106`; `perguntas_orfas_reatavies`: `:1411`. |
| `app/chamados/services/disputa_emissao.py` | `_derivar_chamado_destino`: `app/chamados/services/disputa_emissao.py:211`; `motivo_fora_da_fabrica`: `:695`; `criar_disputa_a_partir_de_chamados_orfaos`: `:874`, `:894`; `criar_disputa_ancora_de_chamado`: `:1117`; `resolver_perguntas_por_chamado_resolvido`: `:1538`, `:1635`, `:1666`. |
| `app/chamados/services/materializacao.py` | `estado_aplicacao_pergunta`: `app/chamados/services/materializacao.py:419`; `_materializar_ausencia_de_resposta`: `:510`; `perguntas_absorvidas_por`: `:579`, `:581`, `:586`; snapshot de `devolver_absorvidas_da_ausencia_rejeitada`: `:625`; `_carimbar_recusa`: `:1037`, `:1044`; `materializar_perguntas_validadas_da_disputa`: `:1291`; `materializar_pendentes_aprovadas`: `:1338`. |
| `app/chamados/services/validacao.py` | `devolver_validacao_recusada`: `app/chamados/services/validacao.py:180`, `:182`; `validar_pergunta`: `:422`, `:433`, `:451`. |
| `app/chamados/services/raiox.py` | `creditos_perguntas`: `app/chamados/services/raiox.py:133`; `pergunta_sem_acao_possivel`: `:173`. |
| `app/chamados/services/relavra_via.py` | `q_carimbo_desfazivel`: `app/chamados/services/relavra_via.py:179`; snapshot de `desfazer_carimbo_sem_lastro`: `:229`. |
| `app/chamados/services/fio.py` | `retratar_duplicata`: `app/chamados/services/fio.py:893`. |
| `app/chamados/services/fabrica_da_celula.py` | `perguntar`: `app/chamados/services/fabrica_da_celula.py:123`. |
| `app/chamados/services/painel.py` | Agregação de `marcador_do_placar`: `app/chamados/services/painel.py:67`. |
| `app/chamados/services/historico.py` | `_eventos_dos_chamados`, leitura dinâmica pelo nome do campo: `app/chamados/services/historico.py:103`, `:105`. |
| `app/chamados/services/passivo_validado.py` | `_reversao_de`, leitura dinâmica por `CAMPOS_REVERSAO`: `app/chamados/services/passivo_validado.py:64`, `:83`. |
| `app/chamados/veredito_lavrado.py` | Agregação de `marcos_do_lote`: `app/chamados/veredito_lavrado.py:163`, `:164`. |
| `app/chamados/views.py` | `responder_pergunta_disputa`: `app/chamados/views.py:1257`. |
| `app/ponto/services/procedencia_batida.py` | `procedencia`: `app/ponto/services/procedencia_batida.py:74`. |
| `app/chamados/services/regularizacao_ext.py` | `retomar_regularizacoes_confirmadas`, **outro modelo**: `app/chamados/services/regularizacao_ext.py:219`. |

Há também leitura **indireta pela expressão comum** `q_carimbo_desfazivel`: a fábrica a usa, e o vigia seleciona os carimbos e depois pergunta ao C6; `materializada_viva` conta esse resultado. (`app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/vigia_fio.py:760`, `app/chamados/services/vigia_fio.py:766`, `app/chamados/services/vigia_fio.py:800`)

Leitores em comandos:

- `auditar_disputas.handle`: `app/chamados/management/commands/auditar_disputas.py:71`, `:150`.
- `auditar_invariantes_chamados`: `universo_i6` em `app/chamados/management/commands/auditar_invariantes_chamados.py:469`; `_i6_pares` em `:537`; demais consultas/contagens em `:587`, `:904`, `:954`, `:1072`, `:1153`, `:1570`, `:1598`, `:1763`.
- `encerrar_perguntas_com_fato.handle`: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`.
- `reabrir_orfas_enterradas.handle`: `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `:87`, `:90`, `:128`, `:141`.
- `reconciliar_fantasmas.handle`: `app/ponto/management/commands/reconciliar_fantasmas.py:212`.

**3. Nomes do F0 e ponto natural de escrita**

| Nome citado | Existe hoje? |
|---|---|
| `relavra_via.VIAS_DE_IDENTIDADE` | **Sim.** Contém quatro vias: `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`. `competencia_trancada` não integra essa constante. (`app/chamados/services/relavra_via.py:166`) |
| `competencia_trancada` como via | **Sim.** É constante da pergunta; C6 a trata em degrau próprio e também no conjunto de cinco vias que encerram por identidade. (`app/chamados/models.py:2263`, `app/chamados/juizes.py:1042`, `app/chamados/juizes.py:1066`) |
| `PerguntaDisputa.fechar_por_competencia_trancada` | **Sim, ainda existe e escreve.** Continua também na lista do selo Q1; portanto a retirada prevista no F0 não está realizada. (`app/chamados/models.py:2505`, `app/chamados/models.py:2514`, `app/chamados/tests/test_porta_pergunta_identidade.py:65`) |
| `core/portas.py`, Q1 | **Sim.** O comentário histórico ainda enumera cinco atos, incluindo o método anterior. O selo atual enumera nove atos. (`app/core/portas.py:241`, `app/core/portas.py:242`, `app/chamados/tests/test_porta_pergunta_identidade.py:60`) |
| `competencia_trancada` como autoridade de fechamento | **Sim.** Há função com esse nome em fechamento e registro K4 apontando para ela. (`app/ponto/services/fechamento.py:1441`, `app/core/juizes.py:301`) |
| Serviço da competência trancada | **Sim.** O módulo declara a via e chama o encerramento do chamado. (`app/chamados/services/competencia_trancada.py:41`, `app/chamados/services/competencia_trancada.py:177`) |

**Não há um ponto único já existente que receba todas as escritas de `materializada_em`.** Os gates da pergunta e os serviços atribuem e salvam diretamente. (`app/chamados/models.py:2500`, `app/chamados/models.py:2502`, `app/chamados/reconciliador.py:621`, `app/chamados/services/materializacao.py:1151`, `app/chamados/services/materializacao.py:1200`)

Como **inferência de localização pelo desenho atual**, o locus natural seria a própria entidade `PerguntaDisputa`, onde já estão seus gates, por analogia com o núcleo de estado do chamado. Isso identifica um lugar arquitetural; não identifica uma função implementada da F2. (`app/chamados/models.py:195`, `app/chamados/models.py:2485`, `app/chamados/models.py:2505`, `app/chamados/models.py:2530`)

`_materializar_uma` é o ponto compartilhado existente para duas filas específicas. `veredito_lavrado.lavrar` também não é essa porta: ele julga pelo lastro e grava os campos da lavra, sem escrever `materializada_em`. (`app/chamados/services/materializacao.py:1065`, `app/core/portas.py:284`, `app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`)

**4. Selo de escritores e allowlist**

**Existe o P13**, `ContratoEscritoresEstadoTest`. Ele cobra ausência de arquivo novo e ausência de arquivo sancionado que deixou de escrever. (`app/core/tests/test_contract_escritores_estado.py:250`, `app/core/tests/test_contract_escritores_estado.py:251`, `app/core/tests/test_contract_escritores_estado.py:260`)

A allowlist de `materializada_em` tem estes **sete arquivos**:

| Arquivo permitido | Declaração |
|---|---|
| `chamados/services/regularizacao_ext.py` | `app/core/tests/test_contract_escritores_estado.py:85` |
| `chamados/services/disputa_emissao.py` | `app/core/tests/test_contract_escritores_estado.py:87` |
| `chamados/services/materializacao.py` | `app/core/tests/test_contract_escritores_estado.py:89` |
| `chamados/models.py` | `app/core/tests/test_contract_escritores_estado.py:90` |
| `chamados/reconciliador.py` | `app/core/tests/test_contract_escritores_estado.py:95` |
| `chamados/services/fio.py` | `app/core/tests/test_contract_escritores_estado.py:112` |
| `chamados/services/relavra_via.py` | `app/core/tests/test_contract_escritores_estado.py:117` |

Os limites do selo são concretos:

- Reconhece por regex `.campo = ...` e `update(...campo=...)`; não é varredor semântico de `save(update_fields)`, `bulk_update`, SQL ou kwargs dinâmicos. (`app/core/tests/test_contract_escritores_estado.py:227`)
- Usa `RAIZ='/app'` e seis apps: chamados, ponto, escala, colaboradores, férias e folha. (`app/core/tests/test_contract_escritores_estado.py:12`)
- Exclui diretórios de testes/migrações e arquivos `test_`. (`app/core/tests/test_contract_escritores_estado.py:239`, `app/core/tests/test_contract_escritores_estado.py:242`)
- Trabalha por nome do campo e arquivo; por isso a mes

[truncado em 20000 de 26774 caracteres]

## 01/11 13:43 · sessao 982955704424 · - · - · origem=Ronald (tmux)

Auditoria de leitores concluída, com inventários enviados por mensagem. Pontos centrais:

- `VIAS_DE_IDENTIDADE` existe e contém quatro vias; `competencia_trancada` fica fora dessa constante, mas integra as cinco vias de encerramento por identidade do C6. `app/chamados/services/relavra_via.py:166`; `app/chamados/juizes.py:1066`.
- `PerguntaDisputa.fechar_por_competencia_trancada` continua presente; a busca estática encontrou somente sua definição na produção. Continua nomeado pelo selo de identidade. `app/chamados/models.py:2505`; `app/chamados/tests/test_porta_pergunta_identidade.py:65`.
- O comentário Q1 descreve cinco atos, enquanto a allowlist real do selo tem nove atos. `app/core/portas.py:242`; `app/chamados/tests/test_porta_pergunta_identidade.py:60`.
- Não há núcleo comum existente para todos os carimbos: os gates do modelo gravam diretamente, assim como materializador, reconciliador e serviços. Inferência sobre “ponto natural”: núcleo na entidade `PerguntaDisputa`, por analogia com os gates existentes; isso é inferência, não porta implementada. `app/chamados/models.py:2485`; `app/chamados/models.py:2502`; `app/chamados/services/materializacao.py:1186`; `app/chamados/reconciliador.py:624`.
- `materializada_por` serve à procedência e à trilha; não entra no julgamento C6, que lê `materializada_em` e `via_resolucao`. `app/ponto/services/procedencia_batida.py:72`; `app/chamados/juizes.py:1042`; `app/chamados/juizes.py:1071`; `app/chamados/juizes.py:1107`.
- Dois leitores dinâmicos precisam constar: histórico lê o campo por `getattr`; snapshot do passivo lê `materializada_em`, `materializada_por_id` e `via_resolucao` por lista de campos. `app/chamados/services/historico.py:103`; `app/chamados/services/historico.py:105`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:83`.
- O vigia lê a elegibilidade comum por `q_carimbo_desfazivel`, depois C6, dentro de `carimbos_sem_lastro_por_dono`; `materializada_viva` apenas conta. `app/chamados/services/vigia_fio.py:760`; `app/chamados/services/vigia_fio.py:766`; `app/chamados/services/vigia_fio.py:800`.
- `JUIZES.md` é lookup subordinado ao vivo e não declara escritores dos três campos; seus “SEM juiz” para fechamento de disputa e significado de fechado divergem do registro atual C7/K4. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`; `app/docs/JUIZES.md:54`; `app/docs/JUIZES.md:71`; `app/core/juizes.py:267`; `app/core/juizes.py:301`.
- A frase “escritor único de validada_em” do lookup não é um inventário literal atual do campo: o próprio P13 admite quatro arquivos. `app/docs/JUIZES.md:38`; `app/core/tests/test_contract_escritores_estado.py:183`.

## 01/11 13:42 · sessao 180955daa069 · - · - · origem=Ronald (tmux)

Inventário operacional de `materializada_em`:

| Modelo/função | Atribuição | Persistência | Guarda/porta |
|---|---|---|---|
| `PerguntaDisputa.fechar_por_admin` | `app/chamados/models.py:2500` | `:2502`, `save(update_fields=...)` | Q1; idempotência em `:2490`; sem guarda C6 comum |
| `PerguntaDisputa.fechar_por_competencia_trancada` | `app/chamados/models.py:2514` | `:2516` | Q1; guarda de validada/via em `:2511`; sem C6 |
| `PerguntaDisputa.encerrar_fantasma` | `app/chamados/models.py:2539` | `:2541` | Q1; guarda idempotente em `:2536`; sem C6 na função |
| `PerguntaDisputa.reabrir_fantasma` | `app/chamados/models.py:2554`, limpa | `:2556` | Q1, inversa de via |
| `PerguntaDisputa.reabrir_por_resposta` | `app/chamados/models.py:2574`, limpa | Retorna campos em `:2576`; chamador `responder_pergunta_disputa_core` soma campos em `app/chamados/services/disputa_emissao.py:1265` e salva em `:1266` | Modelo é escritor, service persiste |
| `PerguntaDisputa.reabrir_por_competencia` | `app/chamados/models.py:2584`, limpa | `:2586` | Q1, inversa de via |
| `ChamadoColaborador.ressuscitar_perguntas` | `app/chamados/models.py:1488`, limpa | `alvo.update(materializada_em=None,...)` no mesmo sítio | Usa `_universo_da_ressuscitacao` em `:1479`; C6 da cópia em `:1445` |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:402` | `:407` | Pergunta ao C6 antes do carimbo em `:399`; escrita direta própria |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:621` | `:624` | C6 em `:611`; exclui dia trancado em `:617`; escrita própria |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:511` | `:530` | Escreve diretamente após criar ausência em `:501`; sem C6 comum |
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:860` | `:870` | Identidade `paridade_recusada`; exige lâmpada True em `:843`; sem C6 comum |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1045` | Campos adicionados `:1053`, `save` em `:1055` | Só `codigo == 'COLISAO'` e campo nulo (`:1044`); identidade `fato_ocupado` em `:1046` |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1151` | Campos em `:1186`, `save` em `:1200` | Atomic em `:1126`; materializa antes (`:1128-1132`), juiz do lastro em `:1139`, rollback `_Reprovada` em `:1145`; sem C6 comum pós-carimbo |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1341` | `:1344` | Juiz do lastro `_pergunta_materializada_de_fato` em `:1339`; sem C6 comum |
| `resolver_perguntas_por_chamado_resolvido`, S128 primeiro ramo | `app/chamados/services/disputa_emissao.py:1562` | Campos `:1565`, `save` em `:1583` | C6 antes em `:1547`; exclui trancado `:1551` |
| Mesma função, S128 sem credor | `app/chamados/services/disputa_emissao.py:1644` | Campos `:1648`, `save` em `:1659` | C6 antes em `:1641`; escrita própria |
| `reabrir_para_correcao` | `app/chamados/services/disputa_emissao.py:1360`, limpa em dict se `desfazer_absorcao` | `qs.update(**campos)` em `:1377` | Porta de reabertura; campo dinâmico, importante não omitir |
| `retratar_duplicata` | `app/chamados/services/fio.py:894` | `:896-897` | Identidade; só preenche carimbo nulo (`:893`); sem C6 comum |
| `desfazer_carimbo_sem_lastro` | `app/chamados/services/relavra_via.py:231`, limpa | `:234` | Relê travada `:221-224`, C6 viva `:227`; elegibilidade `q_carimbo_desfazivel` em `:223` |
| **Outro modelo:** `materializar_regularizacao_externa` | `app/chamados/services/regularizacao_ext.py:198` | `:200` | `RegularizacaoExterna`, campo declarado em `app/chamados/models.py:3038`; não é PerguntaDisputa (`:2659`) |

Atribuição **só em memória**, não escritor persistente: `ChamadoColaborador._universo_da_ressuscitacao`, `app/chamados/models.py:1440`; opera em `copy.copy(p)` (`:1427`) e consulta C6 em `:1445`. Atribui `materializada_em=None`, `materializada_por=None`, `validada_em=None`, `respondida_em=None`, `via_resolucao=''` (`:1440-1444`).

Capacidade genérica adicional: `PerguntaDisputa.abrir(**campos)` aceita extras e repassa a `objects.create(..., **campos)` em `app/chamados/models.py:2453-2454`. Não localizei caller operacional fornecendo `materializada_em`; fixtures fornecem. Não trataria essa capacidade como carimbo operacional explícito, mas convém declará-la como limite da análise de kwargs.

Não existe uma função comum que todos esses carimbos chamem: os sítios acima atribuem e persistem diretamente. O compartilhamento existente é `_materializar_uma` entre duas filas, documentado e testado por `app/chamados/tests/test_porta_desfecho_pergunta.py:14-21`, registrado em `app/core/portas.py:284-290`. Não confundir “porta é módulo”, declarada em `app/chamados/tests/test_porta_pergunta_identidade.py:23-25`, com uma função comum de escrita/C6.

Selo P13:

- `app/core/tests/test_contract_escritores_estado.py:83-118` permite sete arquivos: `regularizacao_ext.py` (`:85`), `disputa_emissao.py` (`:87`), `materializacao.py` (`:89`), `chamados/models.py` (`:90`), `reconciliador.py` (`:95`), `fio.py` (`:112`), `relavra_via.py` (`:117`).
- `test_nenhum_escritor_fora_do_censo` (`:251`) barra novos arquivos; `test_divida_nao_e_fantasma` (`:260`) barra allowlist sem escritor.
- `_escreve` (`:227-231`) reconhece regex `.campo =` e `update(...campo=...)`. Não cobre semanticamente SQL cru, `bulk_update`, `save(update_fields=...)` isolado ou kwargs dinâmicos.
- `RAIZ='/app'`, seis `APPS` (`:12-13`): chamados, ponto, escala, colaboradores, ferias, folha; deixa api, relatorios e core fora. Exclui testes/migrações (`:239`) e arquivos `test_` (`:242`). Não executado aqui. [nao verificado]
- Complementos: `app/chamados/tests/test_contract_via_muda.py:11-37` cobra `via_resolucao` no bloco do carimbo; `app/chamados/tests/test_contract_autor_muda.py:26-55` cobra `materializada_por`. Ambos varrem atribuição não-None por regex, pulam `reg` (outro modelo, respectivamente `:28`, `:43`), testes e migrações; não são guarda C6 comum. Anti-vacuidade de autor em `:57-69`.

Escopo global: busca por referência ao campo fora de app encontrou só comentário `bin/tests/test_artefato_de_estado.sh:5`; nenhum escritor operacional fora de app foi localizado. Nenhum SQL cru ou `bulk_update` com esse campo foi localizado na cópia; ausência de execução em produção é [não verificado].

Migrações do campo são definição de schema, sem carimbo de dado encontrado: `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:23` e `app/chamados/migrations/0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26`. Fixtures de teste são separadas dos escritores operacionais e excluídas pelo P13 (`app/core/tests/test_contract_escritores_estado.py:239`).

`passivo_validado.CAMPOS_REVERSAO` é **leitor**: enumera `materializada_em` em `app/chamados/services/passivo_validado.py:64`, lido por `getattr` em `_reversao_de`, `:83`; não há setattr/update/save desses campos nesse arquivo.

## 01/11 13:41 · sessao 2ebc7e13e9cf · - · - · origem=Ronald (tmux)

1. **Medir desligamento/RECONCILIAR: documentado, execução não verificável.** `app/docs/RELATO.md:270` registra a medida: 43 vivas em 25 chamados de desligado; 1.281 chamados vivos com viva ancorada, dos quais 106 fechariam, 18 reatariam e 88 recusariam. Não li a sonda/log citado (`logs/o261e/f1_medida_p1.resp`): resultado primário e correspondência com o hash atual **[não verificado]**. A inversão efetiva da ordem está no código: disputas primeiro em `app/colaboradores/services/desligamento.py:139`, chamados depois em `:150`.

2. **REDs: testes presentes; execução RED histórica [não verificado].** Fechamento com viva pela âncora destino: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`; pelo container: `:94`; slot não derivável/pós estado: `:172`, com contraexemplo da célula acusadora em `:179`; costura em terminal: `:252`; dedup de `abrir` reapontando existente: `:275`. A alegação de execução RED está em `app/docs/RELATO.md:270`.

3. **Gate: realizado parcialmente.** Sistema decide uma vez em `_decisao_do_fechamento`, compartilhada por `reconciliaria()` e `reconciliar()`: `app/chamados/models.py:605`, `:621`, `:627`. A guarda nova fica em `_gate_pergunta_viva`, **separada** de `_gate_premissa_do_fechamento`: `:391`, `:678`; a composição chama premissa em `:646` e pergunta viva em `:649`. Reata por pergunta via C8/E4-c em `:355`, `:371`, `:373`, e `fio.reapontar_resolvedora` em `:418`; recusa idempotente em `:378`, `:403`. Humano levanta em `transicionar`: `:159`. `cancelar`, `encerrar` e competência trancada usam guarda em `:747`, `:1532`, `:1552`. **Não ocorreu a absorção da parte `fio_vivo` da S127**: `transicionar` ainda consulta `disputa_viva_do_chamado` antes da nova guarda (`:147`), e esse juiz ainda retorna disputa por `(not perguntas) or fio_vivo(perguntas)` (`app/chamados/juizes.py:1955`, `:1967`).

4. **Tripwire/pós estado: realizado.** `_gravar_estado` detecta travessia vivo→TERMINAIS inclusive por instância velha, chama `_vivas_entre` e levanta antes da escrita em `app/chamados/models.py:213`, `:215`, `:218`, `:220`, `:222`. `_vivas_entre` coloca `status_local=novo`, aponta `p.chamado_destino=self`, consulta C6 e restaura no `finally`: `:332`, `:339`, `:348`, `:349`, `:351`. O gate de sistema reúne decisão/reatamento/save no atomic em `:614`. A relavra S2 é posterior ao save no mesmo atomic: `:274`, `:279`, `:288`; há testes de ordem e falha em `app/chamados/tests/test_o261e_f1_codex_v5.py:77`, `:93`.

5. **Costura: realizada para perguntas existentes; nascimento declarado fora.** A guarda é `_costura_nao_mata_o_fio` (`app/chamados/services/fio.py:742`), consultada pelo reaponte (`:840`) e pelo dedup de `PerguntaDisputa.abrir` (`app/chamados/models.py:2424`). O E4-c `resolvedora_valida` ficou separado, conforme `fio.py:749`. A docstring declara nascimento com destino/pai terminal para F2 em `fio.py:750`; o nascimento continua em `models.py:2453`, sem a guarda usada no ramo de existente em `:2425`.

6. **Selo AST/mutantes: realizado em fonte; execução [não verificado].** Composição dos selos anteriores explicitada em `app/chamados/tests/test_o261e_f1_selo_encerramento.py:5`; tripwire AST em `:24`, teste em `:55`, mutantes em `:64`, `:68`, `:73`, `:78`, `:83`, `:87`. Selo de status cobre atribuição, verbos, defaults, splats, update_fields, bulk_update, setattr e SQL em `app/chamados/tests/test_selo_status_so_pelas_portas.py:10`; varredura em `:340`; mutante de defaults em `:421`. Exceção declarada da troca em memória em `:40` e `app/chamados/tests/test_contract_status_nucleo.py:9`.

7. **Remoção de `superar`: realizada como método.** Lápide em `app/chamados/models.py:755` documenta retirada por zero chamador e L-111; `:759` preserva estado `superado` e indica `cancelar()` para duplicata nova. O catálogo ainda conservar evento SUPERAR é questão distinta: `app/docs/RELATO.md:204` o registra em fila.

8. **DIFF de TXT/aberta/exportada: documentado; prova primária no hash [não verificado].** `app/docs/RELATO.md:270` registra base116×cura29, mudança nominal col750/col911 só na base e 26 exportadas com hashes iguais; `:252` registra rodada posterior com ENTRA347→347 e hash26 igual; `:253` aponta comparador/logs. Não há execução dessas sondas lida nesta auditoria. A própria linha da obra ainda descreve F1 como em construtor no `app/docs/BACKLOG.md:820`.

_1408 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
